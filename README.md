-- Apple Hub
-- Script com estilo maçã, emojis animados, sombra, arrastável e botões de controle

local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

-- Configurações
local DRAG_SPEED = 0.25
local EMOJI_ANIMATION_SPEED = 2
local MAXIMIZED_SIZE = UDim2.new(0, 400, 0, 660)
local MINIMIZED_SIZE = UDim2.new(0, 200, 0, 60)

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
MainFrame.Position = UDim2.new(0.5, -200, 0.5, -330)
MainFrame.BackgroundColor3 = Color3.fromRGB(255, 59, 48) -- Vermelho maçã
MainFrame.BackgroundTransparency = 0.1
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true

-- Sombra
local Shadow = Instance.new("ImageLabel")
Shadow.Name = "Shadow"
Shadow.Size = UDim2.new(1, 20, 1, 20)
Shadow.Position = UDim2.new(0, -10, 0, -10)
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
TitleFrame.Size = UDim2.new(1, 0, 0, 40)
TitleFrame.BackgroundColor3 = Color3.fromRGB(200, 40, 30)
TitleFrame.BackgroundTransparency = 0.2
TitleFrame.BorderSizePixel = 0

-- Emoji animado
local EmojiLabel = Instance.new("TextLabel")
EmojiLabel.Name = "AppleEmoji"
EmojiLabel.Size = UDim2.new(0, 40, 0, 40)
EmojiLabel.Position = UDim2.new(0, 10, 0, 0)
EmojiLabel.BackgroundTransparency = 1
EmojiLabel.Text = "🍎"
EmojiLabel.TextSize = 28
EmojiLabel.Font = Enum.Font.GothamBold
EmojiLabel.TextColor3 = Color3.fromRGB(255, 255, 255)

-- Título
local TitleLabel = Instance.new("TextLabel")
TitleLabel.Name = "Title"
TitleLabel.Size = UDim2.new(1, -100, 1, 0)
TitleLabel.Position = UDim2.new(0, 50, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "Apple Hub"
TitleLabel.TextSize = 20
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left

-- Botões de controle
local ControlFrame = Instance.new("Frame")
ControlFrame.Name = "Controls"
ControlFrame.Size = UDim2.new(0, 80, 0, 40)
ControlFrame.Position = UDim2.new(1, -90, 0, 0)
ControlFrame.BackgroundTransparency = 1

-- Botão minimizar/maximizar
local ToggleSizeButton = Instance.new("TextButton")
ToggleSizeButton.Name = "ToggleSize"
ToggleSizeButton.Size = UDim2.new(0, 30, 0, 30)
ToggleSizeButton.Position = UDim2.new(0, 5, 0, 5)
ToggleSizeButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
ToggleSizeButton.BackgroundTransparency = 0.8
ToggleSizeButton.Text = "🗖"
ToggleSizeButton.TextSize = 18
ToggleSizeButton.Font = Enum.Font.Gotham
ToggleSizeButton.TextColor3 = Color3.fromRGB(0, 0, 0)
ToggleSizeButton.AutoButtonColor = false

-- Botão fechar
local CloseButton = Instance.new("TextButton")
CloseButton.Name = "Close"
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(0, 45, 0, 5)
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.BackgroundTransparency = 0.8
CloseButton.Text = "✕"
CloseButton.TextSize = 18
CloseButton.Font = Enum.Font.Gotham
CloseButton.TextColor3 = Color3.fromRGB(0, 0, 0)
CloseButton.AutoButtonColor = false

-- Conteúdo principal
local ContentFrame = Instance.new("Frame")
ContentFrame.Name = "Content"
ContentFrame.Size = UDim2.new(1, -20, 1, -60)
ContentFrame.Position = UDim2.new(0, 10, 0, 50)
ContentFrame.BackgroundTransparency = 1

-- Título da seção Scripts
local ScriptsTitle = Instance.new("TextLabel")
ScriptsTitle.Name = "ScriptsTitle"
ScriptsTitle.Size = UDim2.new(1, 0, 0, 40)
ScriptsTitle.BackgroundTransparency = 1
ScriptsTitle.Text = "📜 Scripts"
ScriptsTitle.TextSize = 22
ScriptsTitle.Font = Enum.Font.GothamBold
ScriptsTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
ScriptsTitle.TextXAlignment = Enum.TextXAlignment.Left

-- Container dos scripts
local ScriptsContainer = Instance.new("ScrollingFrame")
ScriptsContainer.Name = "ScriptsContainer"
ScriptsContainer.Size = UDim2.new(1, 0, 1, -50)
ScriptsContainer.Position = UDim2.new(0, 0, 0, 40)
ScriptsContainer.BackgroundTransparency = 1
ScriptsContainer.BorderSizePixel = 0
ScriptsContainer.ScrollBarThickness = 5
ScriptsContainer.ScrollBarImageColor3 = Color3.fromRGB(255, 255, 255)
ScriptsContainer.CanvasSize = UDim2.new(0, 0, 0, 360)

-- Layout dos scripts
local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Padding = UDim.new(0, 10)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder

-- Botão Nameless Hub
local NamelessButton = Instance.new("TextButton")
NamelessButton.Name = "NamelessHub"
NamelessButton.Size = UDim2.new(1, 0, 0, 60)
NamelessButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
NamelessButton.BackgroundTransparency = 0.3
NamelessButton.Text = ""
NamelessButton.AutoButtonColor = false
NamelessButton.BorderSizePixel = 0

local NamelessLabel = Instance.new("TextLabel")
NamelessLabel.Name = "Label"
NamelessLabel.Size = UDim2.new(1, -60, 1, 0)
NamelessLabel.Position = UDim2.new(0, 50, 0, 0)
NamelessLabel.BackgroundTransparency = 1
NamelessLabel.Text = "Nameless Hub [OP]"
NamelessLabel.TextSize = 18
NamelessLabel.Font = Enum.Font.GothamBold
NamelessLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
NamelessLabel.TextXAlignment = Enum.TextXAlignment.Left

local NamelessIcon = Instance.new("TextLabel")
NamelessIcon.Name = "Icon"
NamelessIcon.Size = UDim2.new(0, 40, 0, 40)
NamelessIcon.Position = UDim2.new(0, 5, 0.5, -20)
NamelessIcon.BackgroundTransparency = 1
NamelessIcon.Text = "👑"
NamelessIcon.TextSize = 24
NamelessIcon.Font = Enum.Font.GothamBold
NamelessIcon.TextColor3 = Color3.fromRGB(255, 215, 0)

-- Botão Chilli Hub
local ChilliButton = Instance.new("TextButton")
ChilliButton.Name = "ChilliHub"
ChilliButton.Size = UDim2.new(1, 0, 0, 60)
ChilliButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
ChilliButton.BackgroundTransparency = 0.3
ChilliButton.Text = ""
ChilliButton.AutoButtonColor = false
ChilliButton.BorderSizePixel = 0

local ChilliLabel = Instance.new("TextLabel")
ChilliLabel.Name = "Label"
ChilliLabel.Size = UDim2.new(1, -60, 1, 0)
ChilliLabel.Position = UDim2.new(0, 50, 0, 0)
ChilliLabel.BackgroundTransparency = 1
ChilliLabel.Text = "Chilli Hub [OP]"
ChilliLabel.TextSize = 18
ChilliLabel.Font = Enum.Font.GothamBold
ChilliLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
ChilliLabel.TextXAlignment = Enum.TextXAlignment.Left

local ChilliIcon = Instance.new("TextLabel")
ChilliIcon.Name = "Icon"
ChilliIcon.Size = UDim2.new(0, 40, 0, 40)
ChilliIcon.Position = UDim2.new(0, 5, 0.5, -20)
ChilliIcon.BackgroundTransparency = 1
ChilliIcon.Text = "🌶️"
ChilliIcon.TextSize = 24
ChilliIcon.Font = Enum.Font.GothamBold
ChilliIcon.TextColor3 = Color3.fromRGB(255, 69, 58)

-- Botão UCT HUB
local UCTButton = Instance.new("TextButton")
UCTButton.Name = "UCTHub"
UCTButton.Size = UDim2.new(1, 0, 0, 60)
UCTButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
UCTButton.BackgroundTransparency = 0.3
UCTButton.Text = ""
UCTButton.AutoButtonColor = false
UCTButton.BorderSizePixel = 0

local UCTLabel = Instance.new("TextLabel")
UCTLabel.Name = "Label"
UCTLabel.Size = UDim2.new(1, -60, 1, 0)
UCTLabel.Position = UDim2.new(0, 50, 0, 0)
UCTLabel.BackgroundTransparency = 1
UCTLabel.Text = "UCT HUB [OP]"
UCTLabel.TextSize = 18
UCTLabel.Font = Enum.Font.GothamBold
UCTLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
UCTLabel.TextXAlignment = Enum.TextXAlignment.Left

local UCTIcon = Instance.new("TextLabel")
UCTIcon.Name = "Icon"
UCTIcon.Size = UDim2.new(0, 40, 0, 40)
UCTIcon.Position = UDim2.new(0, 5, 0.5, -20)
UCTIcon.BackgroundTransparency = 1
UCTIcon.Text = "⚡"
UCTIcon.TextSize = 24
UCTIcon.Font = Enum.Font.GothamBold
UCTIcon.TextColor3 = Color3.fromRGB(0, 191, 255)

-- Botão Kurd Hub
local KurdButton = Instance.new("TextButton")
KurdButton.Name = "KurdHub"
KurdButton.Size = UDim2.new(1, 0, 0, 60)
KurdButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
KurdButton.BackgroundTransparency = 0.3
KurdButton.Text = ""
KurdButton.AutoButtonColor = false
KurdButton.BorderSizePixel = 0

local KurdLabel = Instance.new("TextLabel")
KurdLabel.Name = "Label"
KurdLabel.Size = UDim2.new(1, -60, 1, 0)
KurdLabel.Position = UDim2.new(0, 50, 0, 0)
KurdLabel.BackgroundTransparency = 1
KurdLabel.Text = "Kurd Hub [OP]"
KurdLabel.TextSize = 18
KurdLabel.Font = Enum.Font.GothamBold
KurdLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
KurdLabel.TextXAlignment = Enum.TextXAlignment.Left

local KurdIcon = Instance.new("TextLabel")
KurdIcon.Name = "Icon"
KurdIcon.Size = UDim2.new(0, 40, 0, 40)
KurdIcon.Position = UDim2.new(0, 5, 0.5, -20)
KurdIcon.BackgroundTransparency = 1
KurdIcon.Text = "🏔️"
KurdIcon.TextSize = 24
KurdIcon.Font = Enum.Font.GothamBold
KurdIcon.TextColor3 = Color3.fromRGB(34, 139, 34)

-- Botão Lag+Aura (NOVO)
local LagAuraButton = Instance.new("TextButton")
LagAuraButton.Name = "LagAura"
LagAuraButton.Size = UDim2.new(1, 0, 0, 60)
LagAuraButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
LagAuraButton.BackgroundTransparency = 0.3
LagAuraButton.Text = ""
LagAuraButton.AutoButtonColor = false
LagAuraButton.BorderSizePixel = 0

local LagAuraLabel = Instance.new("TextLabel")
LagAuraLabel.Name = "Label"
LagAuraLabel.Size = UDim2.new(1, -60, 1, 0)
LagAuraLabel.Position = UDim2.new(0, 50, 0, 0)
LagAuraLabel.BackgroundTransparency = 1
LagAuraLabel.Text = "Lag+Aura [OP]"
LagAuraLabel.TextSize = 18
LagAuraLabel.Font = Enum.Font.GothamBold
LagAuraLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
LagAuraLabel.TextXAlignment = Enum.TextXAlignment.Left

local LagAuraIcon = Instance.new("TextLabel")
LagAuraIcon.Name = "Icon"
LagAuraIcon.Size = UDim2.new(0, 40, 0, 40)
LagAuraIcon.Position = UDim2.new(0, 5, 0.5, -20)
LagAuraIcon.BackgroundTransparency = 1
LagAuraIcon.Text = "💀"
LagAuraIcon.TextSize = 24
LagAuraIcon.Font = Enum.Font.GothamBold
LagAuraIcon.TextColor3 = Color3.fromRGB(178, 34, 34) -- Vermelho escuro

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

NamelessButton.Parent = ScriptsContainer
NamelessLabel.Parent = NamelessButton
NamelessIcon.Parent = NamelessButton

ChilliButton.Parent = ScriptsContainer
ChilliLabel.Parent = ChilliButton
ChilliIcon.Parent = ChilliButton

UCTButton.Parent = ScriptsContainer
UCTLabel.Parent = UCTButton
UCTIcon.Parent = UCTButton

KurdButton.Parent = ScriptsContainer
KurdLabel.Parent = KurdButton
KurdIcon.Parent = KurdButton

LagAuraButton.Parent = ScriptsContainer
LagAuraLabel.Parent = LagAuraButton
LagAuraIcon.Parent = LagAuraButton

-- Variáveis de estado
local isDragging = false
local dragInput, dragStart, startPos
local isMinimized = false
local originalSize = MAXIMIZED_SIZE
local originalPosition = MainFrame.Position
local emojiRotation = 0

-- Função para animar o emoji
local function animateEmoji()
    emojiRotation = (emojiRotation + EMOJI_ANIMATION_SPEED * RunService.RenderStepped:Wait()) % 360
    
    local scale = 1 + math.sin(os.clock() * 3) * 0.1
    EmojiLabel.Rotation = emojiRotation
    EmojiLabel.Size = UDim2.new(0, 40 * scale, 0, 40 * scale)
    EmojiLabel.Position = UDim2.new(0, 10 - (scale - 1) * 20, 0, -(scale - 1) * 20)
end

-- Função de clique suave
local function animateClick(button)
    local originalSize = button.Size
    local originalTransparency = button.BackgroundTransparency
    
    local tweenIn = TweenService:Create(button, TweenInfo.new(0.1), {
        Size = UDim2.new(originalSize.X.Scale, originalSize.X.Offset * 0.95, originalSize.Y.Scale, originalSize.Y.Offset * 0.95),
        BackgroundTransparency = originalTransparency - 0.1
    })
    
    local tweenOut = TweenService:Create(button, TweenInfo.new(0.1), {
        Size = originalSize,
        BackgroundTransparency = originalTransparency
    })
    
    tweenIn:Play()
    tweenIn.Completed:Wait()
    tweenOut:Play()
end

-- Sistema de arrastar
local function updateDrag(input)
    local delta = input.Position - dragStart
    MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

TitleFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        isDragging = true
        dragStart = input.Position
        startPos = MainFrame.Position
        
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                isDragging = false
            end
        end)
    end
end)

TitleFrame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and isDragging then
        updateDrag(input)
    end
end)

-- Funções dos botões de controle
ToggleSizeButton.MouseButton1Click:Connect(function()
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
    animateClick(CloseButton)
    ScreenGui:Destroy()
end)

-- Funções dos scripts
NamelessButton.MouseButton1Click:Connect(function()
    animateClick(NamelessButton)
    
    -- Efeito visual de execução
    NamelessIcon.Text = "⏳"
    NamelessLabel.Text = "Executando..."
    
    task.wait(0.5)
    
    -- Executar o script
    local success, error = pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/ily123950/Vulkan/refs/heads/main/Tr"))()
    end)
    
    if success then
        NamelessIcon.Text = "✅"
        NamelessLabel.Text = "Nameless Hub [EXECUTADO]"
    else
        NamelessIcon.Text = "❌"
        NamelessLabel.Text = "Nameless Hub [ERRO]"
        warn("Erro ao executar Nameless Hub:", error)
    end
    
    task.wait(1)
    NamelessIcon.Text = "👑"
    NamelessLabel.Text = "Nameless Hub [OP]"
end)

ChilliButton.MouseButton1Click:Connect(function()
    animateClick(ChilliButton)
    
    -- Efeito visual de execução
    ChilliIcon.Text = "⏳"
    ChilliLabel.Text = "Executando..."
    
    task.wait(0.5)
    
    -- Executar o script
    local success, error = pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/tienkhanh1/spicy/main/Chilli.lua"))()
    end)
    
    if success then
        ChilliIcon.Text = "✅"
        ChilliLabel.Text = "Chilli Hub [EXECUTADO]"
    else
        ChilliIcon.Text = "❌"
        ChilliLabel.Text = "Chilli Hub [ERRO]"
        warn("Erro ao executar Chilli Hub:", error)
    end
    
    task.wait(1)
    ChilliIcon.Text = "🌶️"
    ChilliLabel.Text = "Chilli Hub [OP]"
end)

UCTButton.MouseButton1Click:Connect(function()
    animateClick(UCTButton)
    
    -- Efeito visual de execução
    UCTIcon.Text = "⏳"
    UCTLabel.Text = "Executando..."
    
    task.wait(0.5)
    
    -- Executar o script
    local success, error = pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/UCT-hub/main/refs/heads/main/stealabrainrot"))()
    end)
    
    if success then
        UCTIcon.Text = "✅"
        UCTLabel.Text = "UCT HUB [EXECUTADO]"
    else
        UCTIcon.Text = "❌"
        UCTLabel.Text = "UCT HUB [ERRO]"
        warn("Erro ao executar UCT HUB:", error)
    end
    
    task.wait(1)
    UCTIcon.Text = "⚡"
    UCTLabel.Text = "UCT HUB [OP]"
end)

KurdButton.MouseButton1Click:Connect(function()
    animateClick(KurdButton)
    
    -- Efeito visual de execução
    KurdIcon.Text = "⏳"
    KurdLabel.Text = "Executando..."
    
    task.wait(0.5)
    
    -- Executar o script
    local success, error = pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/Ninja10908/S4/refs/heads/main/Kurdhub"))()
    end)
    
    if success then
        KurdIcon.Text = "✅"
        KurdLabel.Text = "Kurd Hub [EXECUTADO]"
    else
        KurdIcon.Text = "❌"
        KurdLabel.Text = "Kurd Hub [ERRO]"
        warn("Erro ao executar Kurd Hub:", error)
    end
    
    task.wait(1)
    KurdIcon.Text = "🏔️"
    KurdLabel.Text = "Kurd Hub [OP]"
end)

-- Função do Lag+Aura (NOVO)
LagAuraButton.MouseButton1Click:Connect(function()
    animateClick(LagAuraButton)
    
    -- Efeito visual de execução
    LagAuraIcon.Text = "⏳"
    LagAuraLabel.Text = "Configurando..."
    
    task.wait(0.3)
    
    -- Executar o script com configurações
    local success, error = pcall(function()
        -- Configurar as variáveis antes de executar
        getgenv().ServerDestroyerV6 = {
            Comprar = false,
            Spam = true
        }
        
        LagAuraIcon.Text = "⚙️"
        LagAuraLabel.Text = "Executando..."
        
        task.wait(0.2)
        
        -- Executar o script principal
        loadstring(game:HttpGet("https://tcscripts.discloud.app/scripts/serverdestroyerv6"))()
    end)
    
    if success then
        LagAuraIcon.Text = "✅"
        LagAuraLabel.Text = "Lag+Aura [ATIVADO]"
        
        -- Adicionar efeito especial de perigo
        local originalColor = LagAuraButton.BackgroundColor3
        local dangerEffect = RunService.Heartbeat:Connect(function()
            local pulse = math.sin(os.clock() * 5) * 0.3 + 0.7
            LagAuraButton.BackgroundColor3 = Color3.new(
                originalColor.R * pulse,
                originalColor.G * 0.3,
                originalColor.B * 0.3
            )
        end)
        
        -- Parar efeito após 3 segundos
        task.wait(3)
        dangerEffect:Disconnect()
        LagAuraButton.BackgroundColor3 = originalColor
        
    else
        LagAuraIcon.Text = "❌"
        LagAuraLabel.Text = "Lag+Aura [ERRO]"
        warn("Erro ao executar Lag+Aura:", error)
    end
    
    task.wait(1)
    LagAuraIcon.Text = "💀"
    LagAuraLabel.Text = "Lag+Aura [OP]"
end)

-- Efeitos hover
local function setupHoverEffects(button, icon, label, originalText, originalIcon)
    button.MouseEnter:Connect(function()
        button.BackgroundTransparency = 0.1
        label.TextColor3 = Color3.fromRGB(255, 215, 0)
        
        -- Efeito especial para Lag+Aura
        if button.Name == "LagAura" then
            local shakeX = math.random(-2, 2)
            local shakeY = math.random(-2, 2)
            icon.Position = UDim2.new(0, 5 + shakeX, 0.5, -20 + shakeY)
        end
    end)
    
    button.MouseLeave:Connect(function()
        button.BackgroundTransparency = 0.3
        label.TextColor3 = Color3.fromRGB(255, 255, 255)
        
        -- Resetar posição do ícone
        if button.Name == "LagAura" then
            icon.Position = UDim2.new(0, 5, 0.5, -20)
        end
    end)
end

setupHoverEffects(NamelessButton, NamelessIcon, NamelessLabel, "Nameless Hub [OP]", "👑")
setupHoverEffects(ChilliButton, ChilliIcon, ChilliLabel, "Chilli Hub [OP]", "🌶️")
setupHoverEffects(UCTButton, UCTIcon, UCTLabel, "UCT HUB [OP]", "⚡")
setupHoverEffects(KurdButton, KurdIcon, KurdLabel, "Kurd Hub [OP]", "🏔️")
setupHoverEffects(LagAuraButton, LagAuraIcon, LagAuraLabel, "Lag+Aura [OP]", "💀")

-- Animação contínua do emoji
RunService.RenderStepped:Connect(animateEmoji)

-- Inicialização
print("🍎 Apple Hub carregado com sucesso!")
print("📜 Scripts disponíveis:")
print("1. 👑 Nameless Hub [OP]")
print("2. 🌶️ Chilli Hub [OP]")
print("3. ⚡ UCT HUB [OP]")
print("4. 🏔️ Kurd Hub [OP]")
print("5. 💀 Lag+Aura [OP] - CUIDADO!")
print("")
print("⚠️  Atenção: Lag+Aura pode causar instabilidade no servidor!")
print("📢 Use com responsabilidade!")

-- Adiciona borda arredondada
local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, 15)
Corner.Parent = MainFrame

local ButtonCorner = Instance.new("UICorner")
ButtonCorner.CornerRadius = UDim.new(0, 10)
ButtonCorner.Parent = NamelessButton
ButtonCorner:Clone().Parent = ChilliButton
ButtonCorner:Clone().Parent = UCTButton
ButtonCorner:Clone().Parent = KurdButton
ButtonCorner:Clone().Parent = LagAuraButton

local TitleCorner = Instance.new("UICorner")
TitleCorner.CornerRadius = UDim.new(0, 15)
TitleCorner.Parent = TitleFrame

-- Ajusta o tamanho do canvas dinamicamente
UIListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    ScriptsContainer.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 10)
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
addButtonEffect(NamelessButton)
addButtonEffect(ChilliButton)
addButtonEffect(UCTButton)
addButtonEffect(KurdButton)
addButtonEffect(LagAuraButton)

-- Efeito especial para Lag+Aura (glitch)
local function addLagAuraEffect()
    local glitchEffect = RunService.Heartbeat:Connect(function()
        if math.random(1, 100) <= 5 then -- 5% de chance de glitch
            LagAuraIcon.Text = math.random(1, 2) == 1 and "💥" or "⚡"
            LagAuraIcon.TextColor3 = Color3.fromRGB(
                math.random(200, 255),
                math.random(0, 100),
                math.random(0, 100)
            )
            
            task.wait(0.1)
            LagAuraIcon.Text = "💀"
            LagAuraIcon.TextColor3 = Color3.fromRGB(178, 34, 34)
        end
    end)
    
    return glitchEffect
end

-- Tooltips
local function addTooltip(button, description)
    button.MouseEnter:Connect(function()
        button.Mouse.Icon = "rbxasset://SystemCursors/PointingHand"
    end)
    
    button.MouseLeave:Connect(function()
        button.Mouse.Icon = ""
    end)
end

addTooltip(NamelessButton, "Execute Nameless Hub - Script poderoso")
addTooltip(ChilliButton, "Execute Chilli Hub - Script picante")
addTooltip(UCTButton, "Execute UCT HUB - Script eletrizante")
addTooltip(KurdButton, "Execute Kurd Hub - Script das montanhas")
addTooltip(LagAuraButton, "⚠️ SERVER DESTROYER ⚠️ - Use com cuidado!")

-- Iniciar efeito do Lag+Aura
local lagAuraGlitch = addLagAuraEffect()

-- Limpar efeitos ao fechar
CloseButton.MouseButton1Click:Connect(function()
    if lagAuraGlitch then
        lagAuraGlitch:Disconnect()
    end
    ScreenGui:Destroy()
end)

-- Adicionar aviso especial para Lag+Aura
local warningLabel = Instance.new("TextLabel")
warningLabel.Name = "Warning"
warningLabel.Size = UDim2.new(1, -10, 0, 25)
warningLabel.Position = UDim2.new(0, 5, 1, -30)
warningLabel.BackgroundColor3 = Color3.fromRGB(139, 0, 0)
warningLabel.BackgroundTransparency = 0.3
warningLabel.Text = "⚠️ Lag+Aura: Pode causar crash no servidor!"
warningLabel.TextSize = 12
warningLabel.Font = Enum.Font.GothamBold
warningLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
warningLabel.TextXAlignment = Enum.TextXAlignment.Center
warningLabel.Visible = false
warningLabel.Parent = MainFrame

local warningCorner = Instance.new("UICorner")
warningCorner.CornerRadius = UDim.new(0, 8)
warningCorner.Parent = warningLabel

-- Mostrar aviso quando hover no Lag+Aura
LagAuraButton.MouseEnter:Connect(function()
    warningLabel.Visible = true
end)

LagAuraButton.MouseLeave:Connect(function()
    warningLabel.Visible = false
end)

-- Ajustar tamanho inicial do canvas
task.wait(0.1)
ScriptsContainer.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 10)
