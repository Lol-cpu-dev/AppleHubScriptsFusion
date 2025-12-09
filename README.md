-- 🍎 Apple Hub Premium - Versão com Sistema de Autorização
-- 🔒 Apenas jogadores autorizados podem acessar o Premium

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

-- Detectar dispositivo
local player = Players.LocalPlayer
local isMobile = UserInputService.TouchEnabled
local screenSize = workspace.CurrentCamera.ViewportSize

-- 🔐 LISTA DE IDs AUTORIZADOS PARA PREMIUM
local AUTHORIZED_PLAYERS = {
    8738101103,  -- ID do jogador 1 (substitua pelos IDs reais)
    9876543210,  -- ID do jogador 2
    5555555555,  -- ID do jogador 3
    -- Adicione mais IDs aqui
}

-- Verificar se o jogador atual está autorizado
local function isPlayerAuthorized()
    local playerId = player.UserId
    for _, authorizedId in ipairs(AUTHORIZED_PLAYERS) do
        if playerId == authorizedId then
            return true
        end
    end
    return false
end

-- Configurações
local frameWidth = isMobile and 350 or 420
local frameHeight = 600
local startPosition = UDim2.new(0.5, -frameWidth/2, 0.5, -frameHeight/2)

-- GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AppleHubPremium"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game:GetService("CoreGui")

-- Frame principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, frameWidth, 0, frameHeight)
MainFrame.Position = startPosition
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true

-- Estilização premium
local Border = Instance.new("UIStroke")
Border.Color = Color3.fromRGB(255, 215, 0)  -- Dourado premium
Border.Thickness = 3
Border.Parent = MainFrame

local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, 15)
Corner.Parent = MainFrame

-- Barra de título premium
local TitleBar = Instance.new("Frame")
TitleBar.Name = "TitleBar"
TitleBar.Size = UDim2.new(1, 0, 0, 50)
TitleBar.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
TitleBar.BorderSizePixel = 0

-- Gradiente premium na barra de título
local TitleGradient = Instance.new("UIGradient")
TitleGradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 215, 0)),
    ColorSequenceKeypoint.new(0.5, Color3.fromRGB(255, 140, 0)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 215, 0))
})
TitleGradient.Rotation = 90
TitleGradient.Parent = TitleBar

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(0.6, 0, 1, 0)
TitleLabel.Position = UDim2.new(0.2, 0, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "🍎 APPLE HUB PREMIUM"
TitleLabel.TextColor3 = Color3.new(1, 1, 1)
TitleLabel.TextSize = 18
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextXAlignment = Enum.TextXAlignment.Center

-- Botões de controle
local ControlFrame = Instance.new("Frame")
ControlFrame.Size = UDim2.new(0, 80, 1, 0)
ControlFrame.Position = UDim2.new(1, -85, 0, 0)
ControlFrame.BackgroundTransparency = 1

local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Size = UDim2.new(0, 35, 0, 35)
MinimizeButton.Position = UDim2.new(0, 5, 0.5, -17.5)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(255, 193, 7)
MinimizeButton.Text = "─"
MinimizeButton.TextColor3 = Color3.new(0, 0, 0)
MinimizeButton.TextSize = 16
MinimizeButton.Font = Enum.Font.GothamBold

local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 35, 0, 35)
CloseButton.Position = UDim2.new(0, 45, 0.5, -17.5)
CloseButton.BackgroundColor3 = Color3.fromRGB(220, 60, 60)
CloseButton.Text = "✕"
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.TextSize = 16
CloseButton.Font = Enum.Font.GothamBold

-- Arredondar botões
local buttonCorner = Instance.new("UICorner")
buttonCorner.CornerRadius = UDim.new(0, 8)
buttonCorner.Parent = MinimizeButton
buttonCorner:Clone().Parent = CloseButton

-- Área de scripts
local ScriptsFrame = Instance.new("ScrollingFrame")
ScriptsFrame.Name = "ScriptsFrame"
ScriptsFrame.Size = UDim2.new(1, -20, 1, -120)  -- Maior para caber botão premium
ScriptsFrame.Position = UDim2.new(0, 10, 0, 60)
ScriptsFrame.BackgroundTransparency = 1
ScriptsFrame.ScrollBarThickness = 4
ScriptsFrame.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 110)

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Padding = UDim.new(0, 10)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- Montar interface
MainFrame.Parent = ScreenGui
TitleBar.Parent = MainFrame
TitleLabel.Parent = TitleBar
ControlFrame.Parent = TitleBar
MinimizeButton.Parent = ControlFrame
CloseButton.Parent = ControlFrame
ScriptsFrame.Parent = MainFrame
UIListLayout.Parent = ScriptsFrame

-- Lista de scripts
local scripts = {
    {name = "Nameless", icon = "👑", color = Color3.fromRGB(255, 215, 0), url = "https://raw.githubusercontent.com/ily123950/Vulkan/main/Tr"},
    {name = "Chilli", icon = "🌶️", color = Color3.fromRGB(255, 69, 58), url = "https://raw.githubusercontent.com/tienkhanh1/spicy/main/Chilli.lua"},
    {name = "UCT", icon = "⚡", color = Color3.fromRGB(0, 191, 255), url = "https://raw.githubusercontent.com/UCT-hub/main/main/stealabrainrot"},
    {name = "Kurd", icon = "🏔️", color = Color3.fromRGB(34, 139, 34), url = "https://raw.githubusercontent.com/Ninja10908/S4/main/Kurdhub"},
    {name = "Lag + Aura", icon = "💀", color = Color3.fromRGB(178, 34, 34), url = "https://tcscripts.discloud.app/scripts/serverdestroyerv6"},
    {name = "Admin Spam", icon = "👑", color = Color3.fromRGB(255, 215, 0), url = "https://api.luarmor.net/files/v3/loaders/fc9523e876bada3b7ed4ebe004cb8cf9.lua"},
    {name = "Chilli Private", icon = "🔓", color = Color3.fromRGB(0, 200, 83), url = "https://raw.githubusercontent.com/tienkhanh1/spicy/main/PrivateServer"},
    {name = "Speed Steal", icon = "⚡", color = Color3.fromRGB(0, 150, 255), url = "https://pastebin.com/raw/rmxfZDPd"},
    {name = "Auto Ping Pong [KEY]", icon = "🏓", color = Color3.fromRGB(0, 230, 118), url = "https://raw.githubusercontent.com/LucasggkX/Games/main/Auto%20buy%20-%20Ping%20Pong.lua"},
    {name = "Paintball [OP KEY]", icon = "🎯", color = Color3.fromRGB(255, 87, 34), url = "https://raw.githubusercontent.com/LucasggkX/Games/main/Paintball.lua"},
    {name = "Lennon Hub", icon = "🎵", color = Color3.fromRGB(156, 39, 176), url = "https://raw.githubusercontent.com/mxrtnttzflw/martinetti-scripts/main/script.lua"}
}

-- Criar botões de script
for i, scriptData in ipairs(scripts) do
    local button = Instance.new("TextButton")
    button.Name = "ScriptButton_" .. i
    button.Size = UDim2.new(0.95, 0, 0, 50)
    button.BackgroundColor3 = Color3.fromRGB(40, 40, 46)
    button.BorderSizePixel = 0
    button.Text = ""
    button.AutoButtonColor = false
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 10)
    corner.Parent = button
    
    -- Ícone
    local icon = Instance.new("TextLabel")
    icon.Size = UDim2.new(0, 30, 0, 30)
    icon.Position = UDim2.new(0, 10, 0.5, -15)
    icon.BackgroundTransparency = 1
    icon.Text = scriptData.icon
    icon.TextSize = 20
    icon.TextColor3 = scriptData.color
    icon.Font = Enum.Font.GothamBold
    icon.Parent = button
    
    -- Nome
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -50, 1, 0)
    label.Position = UDim2.new(0, 50, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = scriptData.name
    label.TextSize = 14
    label.TextColor3 = Color3.new(1, 1, 1)
    label.Font = Enum.Font.Gotham
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = button
    
    -- Badge KEY
    if scriptData.name:find("%[KEY%]") then
        local keyBadge = Instance.new("Frame")
        keyBadge.Size = UDim2.new(0, 30, 0, 18)
        keyBadge.Position = UDim2.new(1, -35, 0.5, -9)
        keyBadge.BackgroundColor3 = Color3.fromRGB(255, 152, 0)
        keyBadge.BorderSizePixel = 0
        
        local badgeCorner = Instance.new("UICorner")
        badgeCorner.CornerRadius = UDim.new(0, 4)
        badgeCorner.Parent = keyBadge
        
        local keyText = Instance.new("TextLabel")
        keyText.Size = UDim2.new(1, 0, 1, 0)
        keyText.BackgroundTransparency = 1
        keyText.Text = "KEY"
        keyText.TextColor3 = Color3.new(1, 1, 1)
        keyText.TextSize = 10
        keyText.Font = Enum.Font.GothamBold
        keyText.Parent = keyBadge
        
        keyBadge.Parent = button
    end
    
    -- Efeito hover
    if not isMobile then
        button.MouseEnter:Connect(function()
            button.BackgroundColor3 = Color3.fromRGB(50, 50, 56)
        end)
        
        button.MouseLeave:Connect(function()
            button.BackgroundColor3 = Color3.fromRGB(40, 40, 46)
        end)
    end
    
    -- Executar script
    button.MouseButton1Click:Connect(function()
        local originalColor = button.BackgroundColor3
        local originalText = label.Text
        
        button.BackgroundColor3 = Color3.fromRGB(60, 60, 70)
        label.Text = "Carregando..."
        
        task.wait(0.2)
        
        if scriptData.name == "Lag + Aura" then
            getgenv().ServerDestroyerV6 = {Comprar = false, Spam = true}
        end
        
        local success, errorMsg = pcall(function()
            loadstring(game:HttpGet(scriptData.url))()
        end)
        
        if success then
            button.BackgroundColor3 = Color3.fromRGB(60, 180, 80)
            label.Text = "Executado!"
        else
            button.BackgroundColor3 = Color3.fromRGB(180, 60, 60)
            label.Text = "Erro!"
            warn("Erro:", errorMsg)
        end
        
        task.wait(1)
        button.BackgroundColor3 = originalColor
        label.Text = originalText
    end)
    
    button.Parent = ScriptsFrame
end

-- 🎖️ BOTÃO PREMIUM (ESPECIAL)
local premiumButtonFrame = Instance.new("Frame")
premiumButtonFrame.Size = UDim2.new(1, 0, 0, 70)
premiumButtonFrame.Position = UDim2.new(0, 0, 1, -80)
premiumButtonFrame.BackgroundTransparency = 1

local premiumButton = Instance.new("TextButton")
premiumButton.Name = "PremiumButton"
premiumButton.Size = UDim2.new(0.9, 0, 0, 55)
premiumButton.Position = UDim2.new(0.05, 0, 0, 0)
premiumButton.BackgroundColor3 = Color3.fromRGB(255, 215, 0)  -- Dourado
premiumButton.Text = ""
premiumButton.AutoButtonColor = false

-- Gradiente no botão premium
local premiumGradient = Instance.new("UIGradient")
premiumGradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 215, 0)),
    ColorSequenceKeypoint.new(0.5, Color3.fromRGB(255, 140, 0)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 215, 0))
})
premiumGradient.Rotation = 45
premiumGradient.Parent = premiumButton

local premiumCorner = Instance.new("UICorner")
premiumCorner.CornerRadius = UDim.new(0, 12)
premiumCorner.Parent = premiumButton

-- Ícone premium
local premiumIcon = Instance.new("TextLabel")
premiumIcon.Size = UDim2.new(0, 35, 0, 35)
premiumIcon.Position = UDim2.new(0, 15, 0.5, -17.5)
premiumIcon.BackgroundTransparency = 1
premiumIcon.Text = "💎"
premiumIcon.TextSize = 24
premiumIcon.TextColor3 = Color3.new(1, 1, 1)
premiumIcon.Font = Enum.Font.GothamBold
premiumIcon.Parent = premiumButton

-- Texto premium
local premiumText = Instance.new("TextLabel")
premiumText.Size = UDim2.new(1, -60, 1, 0)
premiumText.Position = UDim2.new(0, 60, 0, 0)
premiumText.BackgroundTransparency = 1
premiumText.Text = "MIRANDA HUB FPS"
premiumText.TextSize = 16
premiumText.TextColor3 = Color3.new(1, 1, 1)
premiumText.Font = Enum.Font.GothamBold
premiumText.TextXAlignment = Enum.TextXAlignment.Left
premiumText.Parent = premiumButton

-- Subtítulo premium
local premiumSubText = Instance.new("TextLabel")
premiumSubText.Size = UDim2.new(1, -60, 0, 15)
premiumSubText.Position = UDim2.new(0, 60, 1, -20)
premiumSubText.BackgroundTransparency = 1
premiumSubText.Text = "DEVOURER V7 - EXCLUSIVO"
premiumSubText.TextSize = 10
premiumSubText.TextColor3 = Color3.new(1, 1, 1)
premiumSubText.Font = Enum.Font.GothamBold
premiumSubText.TextXAlignment = Enum.TextXAlignment.Left
premiumSubText.Parent = premiumButton

-- Efeito brilho premium
local glow = Instance.new("UIStroke")
glow.Color = Color3.fromRGB(255, 255, 255)
glow.Thickness = 2
glow.Transparency = 0.7
glow.Parent = premiumButton

premiumButtonFrame.Parent = MainFrame
premiumButton.Parent = premiumButtonFrame

-- Função para mostrar notificação
local function showNotification(message, color)
    local notification = Instance.new("TextLabel")
    notification.Size = UDim2.new(1, -20, 0, 40)
    notification.Position = UDim2.new(0, 10, 0, -50)
    notification.BackgroundColor3 = color
    notification.TextColor3 = Color3.new(1, 1, 1)
    notification.Text = message
    notification.TextSize = 14
    notification.Font = Enum.Font.GothamBold
    notification.TextXAlignment = Enum.TextXAlignment.Center
    
    local notifCorner = Instance.new("UICorner")
    notifCorner.CornerRadius = UDim.new(0, 8)
    notifCorner.Parent = notification
    
    notification.Parent = MainFrame
    
    notification:TweenPosition(UDim2.new(0, 10, 0, 15), "Out", "Quad", 0.3)
    task.wait(2)
    notification:TweenPosition(UDim2.new(0, 10, 0, -50), "Out", "Quad", 0.3)
    task.wait(0.3)
    notification:Destroy()
end

-- Função para carregar Miranda Hub FPS Devourer V7
local function loadMirandaHub()
    print("🎯 Carregando MIRANDA HUB FPS DEVOURER V7...")
    
    -- Primeiro destroi o hub atual
    ScreenGui:Destroy()
    
    -- Carrega o Miranda Hub
    local success, errorMsg = pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/seu-usuario/miranda-hub/main/devourer_v7.lua"))()
    end)
    
    if success then
        print("✅ Miranda Hub FPS Devourer V7 carregado com sucesso!")
    else
        warn("❌ Erro ao carregar Miranda Hub:", errorMsg)
        -- Recria o hub se falhar
        loadstring(game:HttpGet("https://raw.githubusercontent.com/Lol-cpu-dev/AppleHub/main/loader.lua"))()
    end
end

-- 🔐 CLIQUE NO BOTÃO PREMIUM (COM VERIFICAÇÃO)
premiumButton.MouseButton1Click:Connect(function()
    -- Verifica se o jogador está autorizado
    if isPlayerAuthorized() then
        -- Jogador autorizado - Mostra confirmação
        premiumButton.BackgroundColor3 = Color3.fromRGB(0, 200, 83)  -- Verde
        premiumText.Text = "CARREGANDO..."
        
        showNotification("🔓 ACESSO PREMIUM AUTORIZADO!", Color3.fromRGB(0, 200, 83))
        
        task.wait(1)
        
        -- Carrega o Miranda Hub
        loadMirandaHub()
    else
        -- Jogador NÃO autorizado
        premiumButton.BackgroundColor3 = Color3.fromRGB(220, 60, 60)  -- Vermelho
        premiumText.Text = "ACESSO NEGADO"
        
        showNotification("❌ Você não tem autorização ao premium.", Color3.fromRGB(220, 60, 60))
        
        -- Mostra o ID do jogador (para debug)
        print("⚠️ ID do jogador não autorizado: " .. player.UserId)
        print("📋 IDs autorizados: " .. table.concat(AUTHORIZED_PLAYERS, ", "))
        
        task.wait(2)
        
        -- Restaura o botão
        premiumButton.BackgroundColor3 = Color3.fromRGB(255, 215, 0)
        premiumText.Text = "MIRANDA HUB FPS"
    end
end)

-- Efeito hover no botão premium
if not isMobile then
    premiumButton.MouseEnter:Connect(function()
        premiumButton.BackgroundColor3 = Color3.fromRGB(255, 225, 100)
    end)
    
    premiumButton.MouseLeave:Connect(function()
        if premiumText.Text ~= "ACESSO NEGADO" then
            premiumButton.BackgroundColor3 = Color3.fromRGB(255, 215, 0)
        end
    end)
end

-- Sistema de arrasto
local dragging = false
local dragStart, frameStart

TitleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        frameStart = MainFrame.Position
    end
end)

TitleBar.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(
            frameStart.X.Scale,
            frameStart.X.Offset + delta.X,
            frameStart.Y.Scale,
            frameStart.Y.Offset + delta.Y
        )
    end
end)

TitleBar.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = false
    end
end)

-- Sistema de minimizar
local minimized = false

MinimizeButton.MouseButton1Click:Connect(function()
    minimized = not minimized
    
    if minimized then
        MainFrame:TweenSize(UDim2.new(0, 150, 0, 40), "Out", "Quad", 0.2, true)
        ScriptsFrame.Visible = false
        premiumButtonFrame.Visible = false
        MinimizeButton.Text = "+"
    else
        MainFrame:TweenSize(UDim2.new(0, frameWidth, 0, frameHeight), "Out", "Quad", 0.2, true)
        ScriptsFrame.Visible = true
        premiumButtonFrame.Visible = true
        MinimizeButton.Text = "─"
    end
end)

-- Fechar
CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Ajustar tamanho do canvas
UIListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    ScriptsFrame.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 10)
end)

-- Animar entrada
MainFrame.BackgroundTransparency = 1
TitleBar.BackgroundTransparency = 1

task.wait(0.1)

for i = 1, 10 do
    MainFrame.BackgroundTransparency = 1 - (i/10)
    TitleBar.BackgroundTransparency = 1 - (i/10)
    task.wait(0.02)
end

-- Mostrar status de autorização no console
print("════════════════════════════════")
print("🍎 APPLE HUB PREMIUM")
print("👤 Jogador: " .. player.Name)
print("🆔 ID: " .. player.UserId)
print("🔐 Premium: " .. (isPlayerAuthorized() and "✅ AUTORIZADO" or "❌ NÃO AUTORIZADO"))
print("🎯 Scripts: " .. #scripts .. " disponíveis")
print("════════════════════════════════")

-- Notificação inicial
task.wait(0.5)
showNotification("🍎 Apple Hub Premium carregado!", Color3.fromRGB(0, 150, 255))

-- Adicionar aviso premium
local warningLabel = Instance.new("TextLabel")
warningLabel.Size = UDim2.new(1, -20, 0, 25)
warningLabel.Position = UDim2.new(0, 10, 1, -30)
warningLabel.BackgroundColor3 = Color3.fromRGB(255, 152, 0)
warningLabel.TextColor3 = Color3.new(1, 1, 1)
warningLabel.Text = "💎 Premium: Apenas jogadores autorizados"
warningLabel.TextSize = 11
warningLabel.Font = Enum.Font.Gotham
warningLabel.TextXAlignment = Enum.TextXAlignment.Center
warningLabel.TextWrapped = true

local warningCorner = Instance.new("UICorner")
warningCorner.CornerRadius = UDim.new(0, 6)
warningCorner.Parent = warningLabel

warningLabel.Parent = MainFrame
