-- 🍎 Apple Hub - Versão Completa Mobile/Pc
-- 🚀 Drag 100% + Mobile otimizado + Ping Pong

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

-- Detectar dispositivo
local player = Players.LocalPlayer
local isMobile = UserInputService.TouchEnabled
local screenSize = workspace.CurrentCamera.ViewportSize

-- 🔧 CONFIGURAÇÕES ESPECÍFICAS PARA MOBILE
local mobileScale = 0.85  -- Reduzido para não cobrir tela toda
local pcWidth = 420
local mobileWidth = screenSize.X * mobileScale
local frameWidth = isMobile and mobileWidth or pcWidth
local frameHeight = isMobile and 500 or 550  -- Menor no mobile
local minimizedHeight = 45

-- GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AppleHubMobile"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false
ScreenGui.DisplayOrder = 999

-- Posição inicial (mais alta no mobile para não cobrir tudo)
local startPosition = isMobile and 
    UDim2.new(0.5, -frameWidth/2, 0.3, -frameHeight/2) or  -- Mais alto no mobile
    UDim2.new(0.5, -frameWidth/2, 0.5, -frameHeight/2)      -- Central no PC

-- Proteção
if syn and syn.protect_gui then
    syn.protect_gui(ScreenGui)
    ScreenGui.Parent = game.CoreGui
elseif gethui then
    ScreenGui.Parent = gethui()
else
    ScreenGui.Parent = game:GetService("CoreGui")
end

-- Frame principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, frameWidth, 0, frameHeight)
MainFrame.Position = startPosition
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true
MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)

-- Estilização
local Border = Instance.new("UIStroke")
Border.Color = Color3.fromRGB(60, 60, 70)
Border.Thickness = 2
Border.Parent = MainFrame

local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, 14)
Corner.Parent = MainFrame

-- Barra de título
local TitleBar = Instance.new("Frame")
TitleBar.Name = "TitleBar"
TitleBar.Size = UDim2.new(1, 0, 0, 50)
TitleBar.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
TitleBar.BorderSizePixel = 0

-- Ícone mobile/PC
local DeviceIcon = Instance.new("TextLabel")
DeviceIcon.Size = UDim2.new(0, 40, 1, 0)
DeviceIcon.Position = UDim2.new(0, 10, 0, 0)
DeviceIcon.BackgroundTransparency = 1
DeviceIcon.Text = isMobile and "📱" or "💻"
DeviceIcon.TextSize = 22
DeviceIcon.TextColor3 = Color3.new(1, 1, 1)
DeviceIcon.Font = Enum.Font.GothamBold

-- Título
local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(1, -100, 1, 0)
TitleLabel.Position = UDim2.new(0, 55, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "APPLE HUB"
TitleLabel.TextColor3 = Color3.new(1, 1, 1)
TitleLabel.TextSize = isMobile and 18 or 20
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left

-- Botões de controle (maiores no mobile)
local buttonSize = isMobile and 35 or 30
local ControlFrame = Instance.new("Frame")
ControlFrame.Size = UDim2.new(0, 80, 1, 0)
ControlFrame.Position = UDim2.new(1, -85, 0, 0)
ControlFrame.BackgroundTransparency = 1

local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Size = UDim2.new(0, buttonSize, 0, buttonSize)
MinimizeButton.Position = UDim2.new(0, 5, 0.5, -buttonSize/2)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(255, 193, 7)
MinimizeButton.Text = "─"
MinimizeButton.TextColor3 = Color3.new(0, 0, 0)
MinimizeButton.TextSize = isMobile and 18 or 16
MinimizeButton.Font = Enum.Font.GothamBold

local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, buttonSize, 0, buttonSize)
CloseButton.Position = UDim2.new(0, 45, 0.5, -buttonSize/2)
CloseButton.BackgroundColor3 = Color3.fromRGB(244, 67, 54)
CloseButton.Text = "✕"
CloseButton.Text = "✕"
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.TextSize = isMobile and 18 or 16
CloseButton.Font = Enum.Font.GothamBold

-- Arredondar botões
local buttonCorner = Instance.new("UICorner")
buttonCorner.CornerRadius = UDim.new(0, 8)
buttonCorner.Parent = MinimizeButton
buttonCorner:Clone().Parent = CloseButton

-- Área de scripts
local ScriptsFrame = Instance.new("ScrollingFrame")
ScriptsFrame.Size = UDim2.new(1, -20, 1, -70)
ScriptsFrame.Position = UDim2.new(0, 10, 0, 60)
ScriptsFrame.BackgroundTransparency = 1
ScriptsFrame.ScrollBarThickness = isMobile and 6 or 4
ScriptsFrame.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 110)
ScriptsFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Padding = UDim.new(0, isMobile and 12 or 10)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- Montar interface
MainFrame.Parent = ScreenGui
TitleBar.Parent = MainFrame
DeviceIcon.Parent = TitleBar
TitleLabel.Parent = TitleBar
ControlFrame.Parent = TitleBar
MinimizeButton.Parent = ControlFrame
CloseButton.Parent = ControlFrame
ScriptsFrame.Parent = MainFrame
UIListLayout.Parent = ScriptsFrame

-- Lista de scripts
local scripts = {
    {name = "Nameless", icon = "👑", color = Color3.fromRGB(255, 215, 0), url = "https://raw.githubusercontent.com/ily123950/Vulkan/main/Tr"},
    {name = "Chilli Hub", icon = "🌶️", color = Color3.fromRGB(255, 69, 58), url = "https://raw.githubusercontent.com/tienkhanh1/spicy/main/Chilli.lua"},
    {name = "UCT Hub", icon = "⚡", color = Color3.fromRGB(0, 191, 255), url = "https://raw.githubusercontent.com/UCT-hub/main/main/stealabrainrot"},
    {name = "Kurd Hub", icon = "🏔️", color = Color3.fromRGB(34, 139, 34), url = "https://raw.githubusercontent.com/Ninja10908/S4/main/Kurdhub"},
    {name = "Lag + Aura", icon = "💀", color = Color3.fromRGB(178, 34, 34), url = "https://tcscripts.discloud.app/scripts/serverdestroyerv6"},
    {name = "Admin Spam", icon = "👑", color = Color3.fromRGB(255, 215, 0), url = "https://api.luarmor.net/files/v3/loaders/fc9523e876bada3b7ed4ebe004cb8cf9.lua"},
    {name = "Chilli Private", icon = "🔓", color = Color3.fromRGB(0, 200, 83), url = "https://raw.githubusercontent.com/tienkhanh1/spicy/main/PrivateServer"},
    {name = "Speed Steal", icon = "⚡", color = Color3.fromRGB(0, 150, 255), url = "https://pastebin.com/raw/rmxfZDPd"},
    -- 🔥 NOVO SCRIPT ADICIONADO
    {name = "Auto Ping Pong [KEY]", icon = "🏓", color = Color3.fromRGB(0, 230, 118), 
     url = "https://raw.githubusercontent.com/LucasggkX/Games/main/Auto%20buy%20-%20Ping%20Pong.lua"}
}

-- Criar botões
for _, scriptData in ipairs(scripts) do
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0.95, 0, 0, isMobile and 65 or 60)
    button.BackgroundColor3 = Color3.fromRGB(40, 40, 46)
    button.BorderSizePixel = 0
    button.Text = ""
    button.AutoButtonColor = false
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 10)
    corner.Parent = button
    
    -- Ícone
    local icon = Instance.new("TextLabel")
    icon.Size = UDim2.new(0, isMobile and 45 or 40, 0, isMobile and 45 or 40)
    icon.Position = UDim2.new(0, 10, 0.5, -isMobile and 22.5 or 20)
    icon.BackgroundTransparency = 1
    icon.Text = scriptData.icon
    icon.TextSize = isMobile and 26 or 24
    icon.TextColor3 = scriptData.color
    icon.Font = Enum.Font.GothamBold
    icon.Parent = button
    
    -- Nome
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -70, 1, 0)
    label.Position = UDim2.new(0, isMobile and 65 or 60, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = scriptData.name
    label.TextSize = isMobile and 16 or 15
    label.TextColor3 = Color3.new(1, 1, 1)
    label.Font = Enum.Font.GothamBold
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = button
    
    -- Indicador de necessidade de key
    if scriptData.name:find("KEY") then
        local keyBadge = Instance.new("Frame")
        keyBadge.Size = UDim2.new(0, 25, 0, 15)
        keyBadge.Position = UDim2.new(1, -30, 0.5, -7.5)
        keyBadge.BackgroundColor3 = Color3.fromRGB(255, 152, 0)
        keyBadge.BorderSizePixel = 0
        
        local keyCorner = Instance.new("UICorner")
        keyCorner.CornerRadius = UDim.new(0, 4)
        keyCorner.Parent = keyBadge
        
        local keyLabel = Instance.new("TextLabel")
        keyLabel.Size = UDim2.new(1, 0, 1, 0)
        keyLabel.BackgroundTransparency = 1
        keyLabel.Text = "KEY"
        keyLabel.TextSize = 10
        keyLabel.TextColor3 = Color3.new(1, 1, 1)
        keyLabel.Font = Enum.Font.GothamBold
        keyLabel.Parent = keyBadge
        
        keyBadge.Parent = button
    end
    
    -- Efeito hover (apenas PC)
    if not isMobile then
        button.MouseEnter:Connect(function()
            button.BackgroundColor3 = Color3.fromRGB(50, 50, 56)
        end)
        
        button.MouseLeave:Connect(function()
            button.BackgroundColor3 = Color3.fromRGB(40, 40, 46)
        end)
    else
        -- Efeito de toque no mobile
        button.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch then
                button.BackgroundColor3 = Color3.fromRGB(50, 50, 56)
            end
        end)
        
        button.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch then
                button.BackgroundColor3 = Color3.fromRGB(40, 40, 46)
            end
        end)
    end
    
    -- Executar script
    local function executeScript()
        local originalColor = button.BackgroundColor3
        local originalIcon = icon.Text
        local originalText = label.Text
        
        -- Animação de clique
        button.BackgroundColor3 = Color3.fromRGB(60, 60, 66)
        icon.Text = "⏳"
        label.Text = "Executando..."
        
        task.wait(0.2)
        
        -- Executar
        local success, err = pcall(function()
            if scriptData.name == "Lag + Aura" then
                getgenv().ServerDestroyerV6 = {Comprar = false, Spam = true}
            end
            loadstring(game:HttpGet(scriptData.url))()
        end)
        
        -- Feedback
        if success then
            button.BackgroundColor3 = Color3.fromRGB(40, 180, 70)
            icon.Text = "✅"
            label.Text = "Sucesso!"
        else
            button.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
            icon.Text = "❌"
            label.Text = "Erro!"
            warn("Erro em " .. scriptData.name .. ":", err)
        end
        
        task.wait(1.5)
        button.BackgroundColor3 = originalColor
        icon.Text = originalIcon
        label.Text = originalText
    end
    
    -- Conectar eventos
    button.MouseButton1Click:Connect(executeScript)
    
    if isMobile then
        button.TouchTap:Connect(executeScript)
    end
    
    button.Parent = ScriptsFrame
end

-- 🔥 SISTEMA DE DRAG SIMPLIFICADO (FUNCIONA EM MOBILE E PC)
local dragging = false
local dragStart, frameStart

TitleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or 
       (isMobile and input.UserInputType == Enum.UserInputType.Touch) then
        dragging = true
        dragStart = input.Position
        frameStart = MainFrame.Position
        
        if isMobile then
            -- Feedback visual no mobile
            TitleBar.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
        end
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or 
       input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(
            frameStart.X.Scale,
            frameStart.X.Offset + delta.X,
            frameStart.Y.Scale,
            frameStart.Y.Offset + delta.Y
        )
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or 
       input.UserInputType == Enum.UserInputType.Touch then
        dragging = false
        if isMobile then
            TitleBar.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        end
    end
end)

-- Minimizar/Fechar
local minimized = false

MinimizeButton.MouseButton1Click:Connect(function()
    minimized = not minimized
    
    if minimized then
        MainFrame:TweenSize(UDim2.new(0, frameWidth, 0, minimizedHeight), "Out", "Quad", 0.2)
        ScriptsFrame.Visible = false
        MinimizeButton.Text = "+"
    else
        MainFrame:TweenSize(UDim2.new(0, frameWidth, 0, frameHeight), "Out", "Quad", 0.2)
        ScriptsFrame.Visible = true
        MinimizeButton.Text = "─"
    end
end)

if isMobile then
    MinimizeButton.TouchTap:Connect(function()
        MinimizeButton.MouseButton1Click:Fire()
    end)
end

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

if isMobile then
    CloseButton.TouchTap:Connect(function()
        CloseButton.MouseButton1Click:Fire()
    end)
end

-- Animações de entrada
MainFrame.BackgroundTransparency = 1
TitleBar.BackgroundTransparency = 1

task.wait(0.1)

for i = 1, 20 do
    MainFrame.BackgroundTransparency = 1 - (i/20)
    TitleBar.BackgroundTransparency = 1 - (i/20)
    task.wait(0.01)
end

-- Feedback inicial
print("════════════════════════════════")
print("🍎 APPLE HUB - Mobile Optimized")
print("📱 Dispositivo: " .. (isMobile and "Mobile" or "PC"))
print("📏 Tamanho: " .. frameWidth .. "x" .. frameHeight)
print("🎯 Scripts: " .. #scripts .. " disponíveis")
print("🖱️  Arraste pela barra superior")
print("════════════════════════════════")

-- Notificação de boas-vindas
task.wait(0.5)
local function showNotification(msg, color)
    local notif = Instance.new("TextLabel")
    notif.Size = UDim2.new(1, -20, 0, 40)
    notif.Position = UDim2.new(0, 10, 0, -50)
    notif.BackgroundColor3 = color
    notif.TextColor3 = Color3.new(1, 1, 1)
    notif.Text = msg
    notif.TextSize = 14
    notif.Font = Enum.Font.GothamBold
    notif.TextXAlignment = Enum.TextXAlignment.Center
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = notif
    
    notif.Parent = MainFrame
    
    notif:TweenPosition(UDim2.new(0, 10, 0, 10), "Out", "Quad", 0.3)
    task.wait(2)
    notif:TweenPosition(UDim2.new(0, 10, 0, -50), "Out", "Quad", 0.3)
    task.wait(0.3)
    notif:Destroy()
end

showNotification("✅ Apple Hub carregado!", Color3.fromRGB(0, 150, 255))

-- 🔧 SISTEMA DE AJUSTE DE TELA MOBILE
if isMobile then
    -- Verificar se está cobrindo muito a tela
    local function checkScreenCoverage()
        local frameAbs = MainFrame.AbsoluteSize
        local screenAbs = screenSize
        
        if frameAbs.Y > screenAbs.Y * 0.7 then
            -- Reduzir altura se estiver muito grande
            frameHeight = screenAbs.Y * 0.65
            MainFrame.Size = UDim2.new(0, frameWidth, 0, frameHeight)
            ScriptsFrame.Size = UDim2.new(1, -20, 1, -70)
        end
    end
    
    -- Ajustar após carregamento
    task.wait(0.5)
    checkScreenCoverage()
    
    -- Ajustar quando a tela girar
    workspace.CurrentCamera:GetPropertyChangedSignal("ViewportSize"):Connect(function()
        task.wait(0.2)
        screenSize = workspace.CurrentCamera.ViewportSize
        frameWidth = screenSize.X * mobileScale
        MainFrame.Size = UDim2.new(0, frameWidth, 0, frameHeight)
        checkScreenCoverage()
    end)
end
