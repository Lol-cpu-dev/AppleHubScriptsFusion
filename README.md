-- 🍎 APPLE HUB - DUAL CATEGORY RESPONSIVO
-- 📱💻 Totalmente otimizado para ambos dispositivos
-- 🚀 TELEGUIA INSTANTÂNEO

-- ============================================
-- CONFIGURAÇÃO INICIAL RESPONSIVA
-- ============================================

print("🚀 INICIANDO APPLE HUB...")

-- Remover hubs antigos primeiro
task.spawn(function()
    for _, gui in pairs(game:GetService("CoreGui"):GetChildren()) do
        if gui.Name:find("Apple") or gui.Name:find("Hub") then
            gui:Destroy()
        end
    end
end)

-- Serviços
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer

-- Esperar o jogador carregar
while not player do
    wait(0.1)
    player = Players.LocalPlayer
end

local isMobile = UserInputService.TouchEnabled

-- Aguardar a câmera estar disponível
while not workspace.CurrentCamera do
    wait(0.1)
end

local screenSize = workspace.CurrentCamera.ViewportSize

-- 🔧 DETECTAR DISPOSITIVO E CONFIGURAR
local function getDeviceSettings()
    if isMobile then
        print("📱 DETECTADO: MOBILE")
        return {
            width = math.min(380, screenSize.X * 0.92),
            height = 620,
            buttonHeight = 56,
            titleSize = 17,
            textSize = 14,
            iconSize = 26,
            buttonSpacing = 8,
            startPos = UDim2.new(0.5, 0, 0.2, 0),
            anchor = Vector2.new(0.5, 0),
            scrollThickness = 5,
            badgeSize = 34,
            badgeText = 9,
            notificationHeight = 40,
            categoryHeight = 46,
            tabButtonHeight = 38
        }
    else
        print("💻 DETECTADO: PC")
        return {
            width = 440,
            height = 660,
            buttonHeight = 50,
            titleSize = 19,
            textSize = 15,
            iconSize = 24,
            buttonSpacing = 10,
            startPos = UDim2.new(0.5, 0, 0.5, 0),
            anchor = Vector2.new(0.5, 0.5),
            scrollThickness = 4,
            badgeSize = 30,
            badgeText = 8,
            notificationHeight = 36,
            categoryHeight = 44,
            tabButtonHeight = 36
        }
    end
end

local settings = getDeviceSettings()

-- Variáveis globais para o sistema de teleguiado
local targetPosition = nil
local positionMarker = nil

-- Variáveis para sistema de drag
local dragging = false
local dragStart = nil
local startPos = nil

-- ============================================
-- CRIAÇÃO DA GUI RESPONSIVA
-- ============================================

-- ScreenGui principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AppleHubDualCategory"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false
ScreenGui.DisplayOrder = 999

-- Tentar inserir no CoreGui
local success, err = pcall(function()
    ScreenGui.Parent = game:GetService("CoreGui")
end)

if not success then
    print("❌ Erro ao criar ScreenGui: " .. tostring(err))
    ScreenGui.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")
end

-- FORÇAR VISIBILIDADE
ScreenGui.Enabled = true

print("✅ GUI criada para: " .. (isMobile and "Mobile" or "PC"))

-- Frame principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, settings.width, 0, settings.height)
MainFrame.Position = settings.startPos
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true
MainFrame.AnchorPoint = settings.anchor
MainFrame.Parent = ScreenGui

-- Borda
local Border = Instance.new("UIStroke")
Border.Color = Color3.fromRGB(60, 60, 75)
Border.Thickness = isMobile and 2.5 or 2
Border.Parent = MainFrame

-- Cantos arredondados
local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, isMobile and 16 or 14)
Corner.Parent = MainFrame

print("✅ Frame principal criado: " .. settings.width .. "x" .. settings.height)

-- ============================================
-- CABEÇALHO COM SISTEMA DE DRAG
-- ============================================

local Header = Instance.new("Frame")
Header.Name = "Header"
Header.Size = UDim2.new(1, 0, 0, isMobile and 70 or 65)
Header.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
Header.BorderSizePixel = 0
Header.Active = true
Header.Selectable = true
Header.Parent = MainFrame

-- Ícone e título
local AppleIcon = Instance.new("TextLabel")
AppleIcon.Size = UDim2.new(0, isMobile and 45 or 42, 0, isMobile and 45 or 42)
AppleIcon.Position = UDim2.new(0, 12, 0.5, -AppleIcon.Size.Y.Offset/2)
AppleIcon.BackgroundTransparency = 1
AppleIcon.Text = "🍎"
AppleIcon.TextSize = isMobile and 35 or 32
AppleIcon.TextColor3 = Color3.fromRGB(255, 100, 100)
AppleIcon.Font = Enum.Font.GothamBold
AppleIcon.TextXAlignment = Enum.TextXAlignment.Center
AppleIcon.Parent = Header

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(1, -120, 1, 0)
TitleLabel.Position = UDim2.new(0, isMobile and 65 or 60, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "APPLE HUB"
TitleLabel.TextSize = settings.titleSize
TitleLabel.TextColor3 = Color3.new(1, 1, 1)
TitleLabel.Font = Enum.Font.GothamBlack
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
TitleLabel.Parent = Header

local Subtitle = Instance.new("TextLabel")
Subtitle.Size = UDim2.new(1, -120, 0, 20)
Subtitle.Position = UDim2.new(0, isMobile and 65 or 60, 0, isMobile and 35 or 32)
Subtitle.BackgroundTransparency = 1
Subtitle.Text = isMobile and "📱 MOBILE OPTIMIZED" or "💻 PC OPTIMIZED"
Subtitle.TextSize = isMobile and 11 or 10
Subtitle.TextColor3 = Color3.fromRGB(180, 180, 200)
Subtitle.Font = Enum.Font.GothamMedium
Subtitle.TextXAlignment = Enum.TextXAlignment.Left
Subtitle.Parent = Header

-- Botões de controle
local buttonCtrlSize = isMobile and 34 or 32
local buttonSpacing = 6

-- Botão Minimizar
local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Size = UDim2.new(0, buttonCtrlSize, 0, buttonCtrlSize)
MinimizeButton.Position = UDim2.new(1, -(buttonCtrlSize * 2 + buttonSpacing + 40), 0.5, -buttonCtrlSize/2)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(255, 193, 7)
MinimizeButton.Text = "─"
MinimizeButton.TextColor3 = Color3.new(0, 0, 0)
MinimizeButton.TextSize = isMobile and 16 or 14
MinimizeButton.Font = Enum.Font.GothamBold
MinimizeButton.Parent = Header

local MinimizeCorner = Instance.new("UICorner")
MinimizeCorner.CornerRadius = UDim.new(0, isMobile and 7 or 6)
MinimizeCorner.Parent = MinimizeButton

-- Botão Fechar
local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, buttonCtrlSize, 0, buttonCtrlSize)
CloseButton.Position = UDim2.new(1, -40, 0.5, -buttonCtrlSize/2)
CloseButton.BackgroundColor3 = Color3.fromRGB(220, 60, 60)
CloseButton.Text = "✕"
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.TextSize = isMobile and 16 or 14
CloseButton.Font = Enum.Font.GothamBold
CloseButton.Parent = Header

local CloseButtonCorner = Instance.new("UICorner")
CloseButtonCorner.CornerRadius = UDim.new(0, isMobile and 7 or 6)
CloseButtonCorner.Parent = CloseButton

print("✅ Cabeçalho com botões de controle criado")

-- ============================================
-- SISTEMA DE DRAG/ARRASTAR
-- ============================================

local function updateDrag(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(
            startPos.X.Scale,
            startPos.X.Offset + delta.X,
            startPos.Y.Scale,
            startPos.Y.Offset + delta.Y
        )
    end
end

local function startDrag(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or 
       (isMobile and input.UserInputType == Enum.UserInputType.Touch) then
        dragging = true
        dragStart = input.Position
        startPos = MainFrame.Position
        
        -- Feedback visual
        Header.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
        
        -- Para mobile, capturar input
        if isMobile then
            local connection
            connection = input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                    Header.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
                    connection:Disconnect()
                end
            end)
        end
    end
end

local function stopDrag()
    dragging = false
    Header.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
end

-- Conectar eventos de drag
Header.InputBegan:Connect(startDrag)
Header.InputChanged:Connect(updateDrag)

if not isMobile then
    Header.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            stopDrag()
        end
    end)
end

print("✅ Sistema de drag/arrastar adicionado")

-- ============================================
-- SISTEMA DE ABAS LATERAIS
-- ============================================

local SideTabsContainer = Instance.new("Frame")
SideTabsContainer.Name = "SideTabsContainer"
SideTabsContainer.Size = UDim2.new(0, 100, 1, -(isMobile and 85 or 80))
SideTabsContainer.Position = UDim2.new(0, 10, 0, isMobile and 78 or 73)
SideTabsContainer.BackgroundTransparency = 1
SideTabsContainer.Parent = MainFrame

-- Aba Scripts
local ScriptsTab = Instance.new("TextButton")
ScriptsTab.Name = "ScriptsTab"
ScriptsTab.Size = UDim2.new(1, 0, 0, settings.tabButtonHeight)
ScriptsTab.Position = UDim2.new(0, 0, 0, 0)
ScriptsTab.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
ScriptsTab.Text = "SCRIPTS"
ScriptsTab.TextColor3 = Color3.new(1, 1, 1)
ScriptsTab.TextSize = isMobile and 14 or 13
ScriptsTab.Font = Enum.Font.GothamBold
ScriptsTab.AutoButtonColor = false

local ScriptsTabCorner = Instance.new("UICorner")
ScriptsTabCorner.CornerRadius = UDim.new(0, isMobile and 8 or 7)
ScriptsTabCorner.Parent = ScriptsTab

-- Indicador de aba ativa
local ScriptsIndicator = Instance.new("Frame")
ScriptsIndicator.Name = "ActiveIndicator"
ScriptsIndicator.Size = UDim2.new(0, 4, 0.7, 0)
ScriptsIndicator.Position = UDim2.new(1, -2, 0.15, 0)
ScriptsIndicator.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
ScriptsIndicator.BorderSizePixel = 0
ScriptsIndicator.Visible = true
ScriptsIndicator.Parent = ScriptsTab

-- Aba Helper
local HelperTab = Instance.new("TextButton")
HelperTab.Name = "HelperTab"
HelperTab.Size = UDim2.new(1, 0, 0, settings.tabButtonHeight)
HelperTab.Position = UDim2.new(0, 0, 0, settings.tabButtonHeight + 8)
HelperTab.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
HelperTab.Text = "HELPER"
HelperTab.TextColor3 = Color3.fromRGB(180, 180, 180)
HelperTab.TextSize = isMobile and 14 or 13
HelperTab.Font = Enum.Font.GothamBold
HelperTab.AutoButtonColor = false

local HelperTabCorner = Instance.new("UICorner")
HelperTabCorner.CornerRadius = UDim.new(0, isMobile and 8 or 7)
HelperTabCorner.Parent = HelperTab

-- Indicador de aba ativa
local HelperIndicator = Instance.new("Frame")
HelperIndicator.Name = "ActiveIndicator"
HelperIndicator.Size = UDim2.new(0, 4, 0.7, 0)
HelperIndicator.Position = UDim2.new(1, -2, 0.15, 0)
HelperIndicator.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
HelperIndicator.BorderSizePixel = 0
HelperIndicator.Visible = false
HelperIndicator.Parent = HelperTab

ScriptsTab.Parent = SideTabsContainer
HelperTab.Parent = SideTabsContainer

-- ============================================
-- CONTAINER PARA CONTEÚDO DAS ABAS
-- ============================================

local ContentContainer = Instance.new("Frame")
ContentContainer.Name = "ContentContainer"
ContentContainer.Size = UDim2.new(1, -115, 1, -(isMobile and 85 or 80))
ContentContainer.Position = UDim2.new(0, 110, 0, isMobile and 78 or 73)
ContentContainer.BackgroundTransparency = 1
ContentContainer.Parent = MainFrame

-- Container para scripts
local ScriptsContainer = Instance.new("ScrollingFrame")
ScriptsContainer.Name = "ScriptsContainer"
ScriptsContainer.Size = UDim2.new(1, 0, 1, 0)
ScriptsContainer.Position = UDim2.new(0, 0, 0, 0)
ScriptsContainer.BackgroundTransparency = 1
ScriptsContainer.ScrollBarThickness = settings.scrollThickness
ScriptsContainer.ScrollBarImageColor3 = Color3.fromRGB(80, 80, 90)
ScriptsContainer.AutomaticCanvasSize = Enum.AutomaticSize.Y
ScriptsContainer.ScrollingEnabled = true
ScriptsContainer.CanvasSize = UDim2.new(0, 0, 0, 0)
ScriptsContainer.Visible = true
ScriptsContainer.Parent = ContentContainer

-- Container para helper
local HelperContainer = Instance.new("ScrollingFrame")
HelperContainer.Name = "HelperContainer"
HelperContainer.Size = UDim2.new(1, 0, 1, 0)
HelperContainer.Position = UDim2.new(0, 0, 0, 0)
HelperContainer.BackgroundTransparency = 1
HelperContainer.ScrollBarThickness = settings.scrollThickness
HelperContainer.ScrollBarImageColor3 = Color3.fromRGB(80, 80, 90)
HelperContainer.AutomaticCanvasSize = Enum.AutomaticSize.Y
HelperContainer.ScrollingEnabled = true
HelperContainer.CanvasSize = UDim2.new(0, 0, 0, 0)
HelperContainer.Visible = false
HelperContainer.Parent = ContentContainer

-- Layout para scripts
local ScriptsLayout = Instance.new("UIListLayout")
ScriptsLayout.Padding = UDim.new(0, settings.buttonSpacing)
ScriptsLayout.SortOrder = Enum.SortOrder.LayoutOrder
ScriptsLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
ScriptsLayout.Parent = ScriptsContainer

-- Layout para helper
local HelperLayout = Instance.new("UIListLayout")
HelperLayout.Padding = UDim.new(0, settings.buttonSpacing)
HelperLayout.SortOrder = Enum.SortOrder.LayoutOrder
HelperLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
HelperLayout.Parent = HelperContainer

print("✅ Sistema de abas laterais criado")

-- ============================================
-- FUNÇÃO PARA MUDAR DE ABA
-- ============================================

local currentTab = "SCRIPTS"

local function switchTab(tabName)
    currentTab = tabName
    
    if tabName == "SCRIPTS" then
        ScriptsTab.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
        ScriptsTab.TextColor3 = Color3.new(1, 1, 1)
        ScriptsIndicator.Visible = true
        
        HelperTab.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
        HelperTab.TextColor3 = Color3.fromRGB(180, 180, 180)
        HelperIndicator.Visible = false
        
        ScriptsContainer.Visible = true
        HelperContainer.Visible = false
    else
        HelperTab.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
        HelperTab.TextColor3 = Color3.new(1, 1, 1)
        HelperIndicator.Visible = true
        
        ScriptsTab.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
        ScriptsTab.TextColor3 = Color3.fromRGB(180, 180, 180)
        ScriptsIndicator.Visible = false
        
        ScriptsContainer.Visible = false
        HelperContainer.Visible = true
    end
    
    print("📁 Aba mudada para: " .. tabName)
end

-- Conectar eventos das abas
ScriptsTab.MouseButton1Click:Connect(function()
    switchTab("SCRIPTS")
end)

HelperTab.MouseButton1Click:Connect(function()
    switchTab("HELPER")
end)

-- Para mobile
if isMobile then
    ScriptsTab.TouchTap:Connect(function()
        switchTab("SCRIPTS")
    end)
    
    HelperTab.TouchTap:Connect(function()
        switchTab("HELPER")
    end)
end

-- ============================================
-- LISTA DE SCRIPTS
-- ============================================

local mainScripts = {
    {name = "Nameless", icon = "👑", color = Color3.fromRGB(255, 215, 0), 
     url = "https://raw.githubusercontent.com/ily123950/Vulkan/main/Tr"},
    
    {name = "Chilli", icon = "🌶️", color = Color3.fromRGB(255, 69, 58), 
     url = "https://raw.githubusercontent.com/tienkhanh1/spicy/main/Chilli.lua"},
    
    {name = "UCT", icon = "⚡", color = Color3.fromRGB(0, 191, 255), 
     url = "https://raw.githubusercontent.com/UCT-hub/main/main/stealabrainrot"},
    
    {name = "Kurd", icon = "🏔️", color = Color3.fromRGB(34, 139, 34), 
     url = "https://raw.githubusercontent.com/Ninja10908/S4/main/Kurdhub"},
    
    {name = "Lag + Aura", icon = "💀", color = Color3.fromRGB(178, 34, 34), 
     url = "https://tcscripts.discloud.app/scripts/serverdestroyerv6", special = true},
    
    {name = "Admin Spam", icon = "👑", color = Color3.fromRGB(255, 215, 0), 
     url = "https://api.luarmor.net/files/v3/loaders/fc9523e876bada3b7ed4ebe004cb8cf9.lua"},
    
    {name = "Chilli Private", icon = "🔓", color = Color3.fromRGB(0, 200, 83), 
     url = "https://raw.githubusercontent.com/tienkhanh1/spicy/main/PrivateServer"},
    
    {name = "Speed Steal", icon = "⚡", color = Color3.fromRGB(0, 150, 255), 
     url = "https://pastebin.com/raw/rmxfZDPd"},
    
    {name = "Auto Ping Pong", icon = "🏓", color = Color3.fromRGB(0, 230, 118), 
     url = "https://raw.githubusercontent.com/LucasggkX/Games/main/Auto%20buy%20-%20Ping%20Pong.lua", badge = "KEY"},
    
    {name = "Paintball", icon = "🎯", color = Color3.fromRGB(255, 87, 34), 
     url = "https://raw.githubusercontent.com/LucasggkX/Games/main/Paintball.lua", badge = "KEY"},
    
    {name = "Lennon Hub", icon = "🎵", color = Color3.fromRGB(156, 39, 176), 
     url = "https://raw.githubusercontent.com/mxrtnttzflw/martinetti-scripts/main/script.lua"},
    
    {name = "LKZ HELPER [OP]", icon = "🔧", color = Color3.fromRGB(0, 180, 216), 
     url = "https://raw.githubusercontent.com/LucasggkX/Games/main/LKZ%20Helper.lua", badge = "NEW"},
    
    {name = "Miranda Hub", icon = "⚡", color = Color3.fromRGB(0, 200, 255), 
     url = "https://pastefy.app/JJVhs3rK/raw", badge = "UPDATE"},
    
    {name = "Miranda 2", icon = "⚡", color = Color3.fromRGB(0, 180, 255), 
     url = "https://raw.githubusercontent.com/NagisaScript1/FakeModz/refs/heads/main/Miranda-Steal-a-Brainrot", badge = "NEW"},
    
    {name = "ADIDAS PACK", icon = "👟", color = Color3.fromRGB(0, 0, 0), 
     url = "https://raw.githubusercontent.com/kolllooomcj-bit/MCJANIMATION/refs/heads/main/ADIDAS%20COMMUNITY%20MCJ", badge = "ANIM"},
    
    {name = "Auto Block on Steal", icon = "🛡️", color = Color3.fromRGB(0, 150, 255), 
     url = "https://raw.githubusercontent.com/LucasggkX/Games/refs/heads/main/Auto%20Block.lua", badge = "NEW"}
}

-- ============================================
-- FUNÇÕES DO HELPER (SIMPLIFICADO)
-- ============================================

local helperFunctions = {
    {
        name = "MARCAR POSIÇÃO", 
        icon = "📍", 
        color = Color3.fromRGB(255, 100, 100), 
        description = "Marca sua posição atual",
        functionType = "markposition"
    },
    
    {
        name = "TELEGUIADO", 
        icon = "🚀", 
        color = Color3.fromRGB(100, 200, 255), 
        description = "Teleporta instantaneamente",
        functionType = "teleflight"
    }
}

-- ============================================
-- SISTEMA DE NOTIFICAÇÕES
-- ============================================

local function showNotification(message, color)
    local notification = Instance.new("TextLabel")
    notification.Size = UDim2.new(1, -20, 0, settings.notificationHeight)
    notification.Position = UDim2.new(0, 10, 0, -50)
    notification.BackgroundColor3 = color or Color3.fromRGB(0, 150, 255)
    notification.TextColor3 = Color3.new(1, 1, 1)
    notification.Text = message
    notification.TextSize = isMobile and 14 or 13
    notification.Font = Enum.Font.GothamBold
    notification.TextXAlignment = Enum.TextXAlignment.Center
    notification.ZIndex = 1000
    
    local notifCorner = Instance.new("UICorner")
    notifCorner.CornerRadius = UDim.new(0, isMobile and 8 or 7)
    notifCorner.Parent = notification
    
    notification.Parent = MainFrame
    
    -- Animação de entrada
    notification:TweenPosition(UDim2.new(0, 10, 0, 10), "Out", "Quad", 0.3)
    
    task.wait(2.5)
    
    -- Animação de saída
    notification:TweenPosition(UDim2.new(0, 10, 0, -50), "Out", "Quad", 0.3)
    
    task.wait(0.3)
    notification:Destroy()
end

-- ============================================
-- FUNÇÃO PARA CRIAR BOTÕES
-- ============================================

local function createButton(buttonData, index, container, isHelper)
    local button = Instance.new("TextButton")
    button.Name = "Btn_" .. index
    button.Size = UDim2.new(0.94, 0, 0, settings.buttonHeight + (isHelper and 10 or 0))
    button.BackgroundColor3 = isHelper and Color3.fromRGB(30, 30, 45) or Color3.fromRGB(35, 35, 45)
    button.BorderSizePixel = 0
    button.Text = ""
    button.AutoButtonColor = false
    button.LayoutOrder = index
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, isMobile and 10 or 9)
    corner.Parent = button
    
    -- Borda
    local border = Instance.new("UIStroke")
    border.Color = isHelper and buttonData.color or Color3.fromRGB(60, 60, 75)
    border.Thickness = isHelper and 2 or 1.5
    border.Parent = button
    
    -- Ícone
    local icon = Instance.new("TextLabel")
    icon.Size = UDim2.new(0, settings.iconSize, 0, settings.iconSize)
    icon.Position = UDim2.new(0, 10, 0.5, -settings.iconSize/2)
    icon.BackgroundTransparency = 1
    icon.Text = buttonData.icon
    icon.TextSize = settings.iconSize - (isMobile and 6 or 5)
    icon.TextColor3 = buttonData.color
    icon.Font = Enum.Font.GothamBold
    icon.Parent = button
    
    -- Nome
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -(settings.iconSize + 20), 0.6, 0)
    label.Position = UDim2.new(0, settings.iconSize + 12, 0, 6)
    label.BackgroundTransparency = 1
    label.Text = buttonData.name
    label.TextSize = settings.textSize
    label.TextColor3 = Color3.new(1, 1, 1)
    label.Font = Enum.Font.GothamBold
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.TextTruncate = Enum.TextTruncate.AtEnd
    label.Parent = button
    
    -- Descrição (apenas para helper)
    if isHelper then
        local desc = Instance.new("TextLabel")
        desc.Size = UDim2.new(1, -(settings.iconSize + 20), 0.4, 0)
        desc.Position = UDim2.new(0, settings.iconSize + 12, 0, settings.buttonHeight/2 + 2)
        desc.BackgroundTransparency = 1
        desc.Text = buttonData.description
        desc.TextSize = isMobile and 11 or 10
        desc.TextColor3 = Color3.fromRGB(200, 200, 220)
        desc.Font = Enum.Font.GothamMedium
        desc.TextXAlignment = Enum.TextXAlignment.Left
        desc.TextTruncate = Enum.TextTruncate.AtEnd
        desc.Parent = button
    end
    
    -- Badge
    if buttonData.badge then
        local badgeColor
        local badgeText = buttonData.badge
        
        if badgeText == "NEW" then
            badgeColor = Color3.fromRGB(0, 180, 216)
        elseif badgeText == "UPDATE" then
            badgeColor = Color3.fromRGB(0, 200, 255)
        elseif badgeText == "KEY" then
            badgeColor = Color3.fromRGB(255, 152, 0)
        elseif badgeText == "ANIM" then
            badgeColor = Color3.fromRGB(0, 0, 0)
        else
            badgeColor = Color3.fromRGB(100, 100, 100)
        end
        
        local badgeWidth = settings.badgeSize + (badgeText == "NEW" and 6 or 0)
        
        local badge = Instance.new("Frame")
        badge.Size = UDim2.new(0, badgeWidth, 0, isMobile and 20 or 18)
        badge.Position = UDim2.new(1, isMobile and -badgeWidth-6 or -badgeWidth-5, 0.1, 0)
        badge.BackgroundColor3 = badgeColor
        badge.BorderSizePixel = 0
        
        local badgeCorner = Instance.new("UICorner")
        badgeCorner.CornerRadius = UDim.new(0, isMobile and 5 or 4)
        badgeCorner.Parent = badge
        
        local badgeLabel = Instance.new("TextLabel")
        badgeLabel.Size = UDim2.new(1, 0, 1, 0)
        badgeLabel.BackgroundTransparency = 1
        badgeLabel.Text = badgeText
        badgeLabel.TextColor3 = Color3.new(1, 1, 1)
        badgeLabel.TextSize = settings.badgeText
        badgeLabel.Font = Enum.Font.GothamBold
        badgeLabel.Parent = badge
        
        badge.Parent = button
    end
    
    -- Efeitos de hover (só no PC)
    if not isMobile then
        button.MouseEnter:Connect(function()
            button.BackgroundColor3 = isHelper and Color3.fromRGB(40, 40, 55) or Color3.fromRGB(45, 45, 55)
        end)
        
        button.MouseLeave:Connect(function()
            button.BackgroundColor3 = isHelper and Color3.fromRGB(30, 30, 45) or Color3.fromRGB(35, 35, 45)
        end)
    end
    
    button.Parent = container
    return button, border
end

-- ============================================
-- CRIAR TODOS OS BOTÕES DE SCRIPT
-- ============================================

for i, scriptData in ipairs(mainScripts) do
    local button, border = createButton(scriptData, i, ScriptsContainer, false)
    
    button.MouseButton1Click:Connect(function()
        local originalColor = button.BackgroundColor3
        local originalIcon = button:FindFirstChildOfClass("TextLabel").Text
        
        -- Animação de clique
        button.BackgroundColor3 = Color3.fromRGB(60, 60, 75)
        
        task.wait(0.1)
        
        -- Mudar ícone para carregamento
        button:FindFirstChildOfClass("TextLabel").Text = "⏳"
        
        -- Executar script
        local success, err = pcall(function()
            if scriptData.special then
                getgenv().ServerDestroyerV6 = {Comprar = false, Spam = true}
            end
            
            local scriptContent = game:HttpGet(scriptData.url, true)
            loadstring(scriptContent)()
        end)
        
        -- Feedback
        if success then
            button:FindFirstChildOfClass("TextLabel").Text = "✅"
            button.BackgroundColor3 = Color3.fromRGB(60, 180, 80)
            border.Color = Color3.fromRGB(60, 180, 80)
            print("✅ " .. scriptData.name .. " executado!")
            
            -- Notificação especial
            if scriptData.name == "Auto Block on Steal" then
                task.wait(0.5)
                showNotification("🛡️ AUTO BLOCK ATIVADO!", Color3.fromRGB(0, 150, 255))
            end
        else
            button:FindFirstChildOfClass("TextLabel").Text = "❌"
            button.BackgroundColor3 = Color3.fromRGB(180, 60, 60)
            border.Color = Color3.fromRGB(180, 60, 60)
            print("❌ Erro em " .. scriptData.name .. ": " .. tostring(err))
        end
        
        task.wait(1.5)
        
        -- Restaurar
        button.BackgroundColor3 = originalColor
        border.Color = scriptData.color
        button:FindFirstChildOfClass("TextLabel").Text = originalIcon
    end)
end

print("✅ " .. #mainScripts .. " scripts criados")

-- ============================================
-- SISTEMA DE TELEGUIADO INSTANTÂNEO
-- ============================================

local function createPositionMarker(position)
    if positionMarker and positionMarker.Parent then
        positionMarker:Destroy()
    end
    
    -- Criar marcador
    positionMarker = Instance.new("Part")
    positionMarker.Name = "AppleHub_PositionMarker"
    positionMarker.Size = Vector3.new(2, 2, 2)
    positionMarker.Position = position + Vector3.new(0, 1, 0)
    positionMarker.Anchored = true
    positionMarker.CanCollide = false
    positionMarker.Material = EnumMaterial.Neon
    positionMarker.BrickColor = BrickColor.new("Bright red")
    positionMarker.Transparency = 0.3
    
    -- Criar aura
    local aura = Instance.new("Part")
    aura.Name = "Aura"
    aura.Size = Vector3.new(6, 0.5, 6)
    aura.Position = position + Vector3.new(0, 0.1, 0)
    aura.Anchored = true
    aura.CanCollide = false
    aura.Material = EnumMaterial.Neon
    aura.BrickColor = BrickColor.new("Bright red")
    aura.Transparency = 0.7
    aura.Parent = positionMarker
    
    -- Luz
    local pointLight = Instance.new("PointLight")
    pointLight.Brightness = 2
    pointLight.Range = 15
    pointLight.Color = Color3.new(1, 0, 0)
    pointLight.Parent = positionMarker
    
    -- Rotação suave
    task.spawn(function()
        while positionMarker and positionMarker.Parent do
            positionMarker.CFrame = positionMarker.CFrame * CFrame.Angles(0, math.rad(2), 0)
            task.wait()
        end
    end)
    
    positionMarker.Parent = workspace
    return positionMarker
end

-- TELEPORTE INSTANTÂNEO DIRETO
local function instantTeleport()
    if not targetPosition then
        showNotification("❌ Marque uma posição primeiro!", Color3.fromRGB(255, 50, 50))
        return false
    end
    
    local character = player.Character
    if not character then 
        showNotification("❌ Personagem não encontrado!", Color3.fromRGB(255, 50, 50))
        return false 
    end
    
    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    if not humanoidRootPart then 
        showNotification("❌ HumanoidRootPart não encontrado!", Color3.fromRGB(255, 50, 50))
        return false 
    end
    
    showNotification("🚀 TELEPORTE INSTANTÂNEO!", Color3.fromRGB(100, 200, 255))
    
    -- TELEPORTE DIRETO E INSTANTÂNEO
    humanoidRootPart.CFrame = CFrame.new(targetPosition + Vector3.new(0, 3, 0))
    
    task.wait(0.1)
    
    -- Pequeno ajuste para garantir que está no chão
    humanoidRootPart.CFrame = CFrame.new(targetPosition)
    
    showNotification("✅ CHEGOU AO DESTINO!", Color3.fromRGB(50, 200, 50))
    
    -- Remover marcador após teleporte
    if positionMarker then
        positionMarker:Destroy()
        positionMarker = nil
    end
    
    -- Limpar posição após teleporte
    targetPosition = nil
    
    return true
end

-- ============================================
-- CRIAR BOTÕES DO HELPER
-- ============================================

for i, helperData in ipairs(helperFunctions) do
    local button, border = createButton(helperData, i, HelperContainer, true)
    
    button.MouseButton1Click:Connect(function()
        -- Animação de clique
        button.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
        
        task.wait(0.1)
        
        button.BackgroundColor3 = Color3.fromRGB(30, 30, 45)
        
        if helperData.functionType == "markposition" then
            local character = player.Character
            if character then
                local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
                if humanoidRootPart then
                    -- Limpar posição anterior
                    targetPosition = nil
                    if positionMarker then
                        positionMarker:Destroy()
                        positionMarker = nil
                    end
                    
                    -- Marcar nova posição
                    targetPosition = humanoidRootPart.Position
                    createPositionMarker(targetPosition)
                    showNotification("📍 POSIÇÃO MARCADA!", helperData.color)
                    print("📍 Posição marcada: " .. tostring(targetPosition))
                else
                    showNotification("❌ HumanoidRootPart não encontrado!", Color3.fromRGB(255, 50, 50))
                end
            else
                showNotification("❌ Personagem não encontrado!", Color3.fromRGB(255, 50, 50))
            end
            
        elseif helperData.functionType == "teleflight" then
            -- TELEPORTE INSTANTÂNEO
            instantTeleport()
        end
    end)
end

print("✅ " .. #helperFunctions .. " funções do helper criadas")

-- ============================================
-- SISTEMA DE MINIMIZAR
-- ============================================

local isMinimized = false

local function toggleMinimize()
    isMinimized = not isMinimized
    
    if isMinimized then
        -- Minimizar
        MainFrame.Size = UDim2.new(0, isMobile and 140 or 130, 0, isMobile and 70 or 65)
        SideTabsContainer.Visible = false
        ContentContainer.Visible = false
        MinimizeButton.Text = "+"
        showNotification("📦 Hub minimizado", Color3.fromRGB(255, 193, 7))
    else
        -- Restaurar
        MainFrame.Size = UDim2.new(0, settings.width, 0, settings.height)
        SideTabsContainer.Visible = true
        ContentContainer.Visible = true
        MinimizeButton.Text = "─"
        showNotification("📂 Hub restaurado", Color3.fromRGB(255, 193, 7))
    end
end

-- ============================================
-- BOTÕES DE CONTROLE
-- ============================================

MinimizeButton.MouseButton1Click:Connect(toggleMinimize)

CloseButton.MouseButton1Click:Connect(function()
    -- Remover marcador
    if positionMarker then
        positionMarker:Destroy()
    end
    
    -- Fechar hub
    ScreenGui:Destroy()
    showNotification("🔒 Apple Hub fechado", Color3.fromRGB(220, 60, 60))
    print("🔒 Apple Hub fechado")
end)

-- ============================================
-- SISTEMA DE TOGGLE COM TECLA
-- ============================================

local hubVisible = true

local function toggleHubVisibility()
    hubVisible = not hubVisible
    ScreenGui.Enabled = hubVisible
    
    if hubVisible then
        showNotification("🍎 HUB ABERTO", Color3.fromRGB(255, 100, 100))
    else
        showNotification("📂 HUB FECHADO (Pressione RightShift)", Color3.fromRGB(150, 150, 150))
    end
end

-- Atalhos de tecla para abrir/fechar
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    
    if input.KeyCode == Enum.KeyCode.RightShift then
        toggleHubVisibility()
    elseif input.KeyCode == Enum.KeyCode.F10 then
        toggleHubVisibility()
    end
end)

print("✅ Atalhos configurados: RightShift/F10 para abrir/fechar")

-- ============================================
-- ANIMAÇÃO DE ENTRADA
-- ============================================

MainFrame.BackgroundTransparency = 1
Header.BackgroundTransparency = 1

task.wait(0.1)

for i = 1, 10 do
    MainFrame.BackgroundTransparency = 1 - (i/10)
    Header.BackgroundTransparency = 1 - (i/10)
    task.wait(0.02)
end

-- ============================================
-- NOTIFICAÇÃO INICIAL
-- ============================================

task.wait(0.5)
showNotification("🍎 APPLE HUB CARREGADO! (RightShift para esconder)", Color3.fromRGB(255, 100, 100))

-- ============================================
-- RODAPÉ
-- ============================================

local footer = Instance.new("TextLabel")
footer.Size = UDim2.new(1, -20, 0, isMobile and 26 or 24)
footer.Position = UDim2.new(0, 10, 1, -30)
footer.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
footer.TextColor3 = Color3.fromRGB(200, 200, 220)
footer.Text = "🎯 " .. #mainScripts .. " Scripts | 🚀 Teleguiado Instantâneo | " .. (isMobile and "📱" or "💻")
footer.TextSize = isMobile and 10 or 9
footer.Font = Enum.Font.GothamMedium
footer.TextXAlignment = Enum.TextXAlignment.Center

local footerCorner = Instance.new("UICorner")
footerCorner.CornerRadius = UDim.new(0, isMobile and 6 or 5)
footerCorner.Parent = footer

footer.Parent = MainFrame

-- ============================================
-- ATUALIZAR SCROLL AUTOMATICAMENTE
-- ============================================

task.spawn(function()
    while ScreenGui.Parent do
        ScriptsContainer.CanvasSize = UDim2.new(0, 0, 0, ScriptsLayout.AbsoluteContentSize.Y + 10)
        HelperContainer.CanvasSize = UDim2.new(0, 0, 0, HelperLayout.AbsoluteContentSize.Y + 10)
        task.wait(0.5)
    end
end)

-- ============================================
-- AJUSTE PARA MOBILE
-- ============================================

if isMobile then
    task.spawn(function()
        while ScreenGui.Parent do
            local viewport = workspace.CurrentCamera.ViewportSize
            
            -- Ajustar largura para portrait/landscape
            local newWidth = math.min(380, viewport.X * 0.92)
            local newHeight = viewport.Y > viewport.X and 620 or 500
            
            if not isMinimized then
                MainFrame.Size = UDim2.new(0, newWidth, 0, newHeight)
            end
            
            task.wait(1)
        end
    end)
end

-- ============================================
-- LIMPEZA
-- ============================================

game:GetService("Players").PlayerRemoving:Connect(function(leavingPlayer)
    if leavingPlayer == player then
        if positionMarker then
            positionMarker:Destroy()
        end
        
        if ScreenGui then
            ScreenGui:Destroy()
        end
    end
end)

-- ============================================
-- BOTÃO FLUTUANTE PARA ABRIR HUB
-- ============================================

-- Criar botão flutuante
local FloatingButton = Instance.new("TextButton")
FloatingButton.Name = "FloatingHubButton"
FloatingButton.Size = UDim2.new(0, isMobile and 60 or 50, 0, isMobile and 60 or 50)
FloatingButton.Position = UDim2.new(0, 20, 0.5, -isMobile and 30 or 25)
FloatingButton.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
FloatingButton.Text = "🍎"
FloatingButton.TextSize = isMobile and 28 or 24
FloatingButton.TextColor3 = Color3.new(1, 1, 1)
FloatingButton.Font = Enum.Font.GothamBold
FloatingButton.Visible = false
FloatingButton.ZIndex = 9999

local FloatCorner = Instance.new("UICorner")
FloatCorner.CornerRadius = UDim.new(1, 0)
FloatCorner.Parent = FloatingButton

local FloatShadow = Instance.new("UIStroke")
FloatShadow.Color = Color3.fromRGB(0, 0, 0)
FloatShadow.Thickness = 2
FloatShadow.Parent = FloatingButton

FloatingButton.Parent = ScreenGui

-- Conectar botão flutuante
FloatingButton.MouseButton1Click:Connect(function()
    ScreenGui.Enabled = true
    FloatingButton.Visible = false
    showNotification("🍎 HUB ABERTO", Color3.fromRGB(255, 100, 100))
end)

-- Sistema de toggle atualizado para mostrar botão flutuante
local originalToggle = toggleHubVisibility
toggleHubVisibility = function()
    hubVisible = not hubVisible
    ScreenGui.Enabled = hubVisible
    
    if hubVisible then
        FloatingButton.Visible = false
        showNotification("🍎 HUB ABERTO", Color3.fromRGB(255, 100, 100))
    else
        FloatingButton.Visible = true
        showNotification("📂 Hub minimizado - Clique no 🍎 ou RightShift", Color3.fromRGB(150, 150, 150))
    end
end

-- Atualizar conexão do teclado
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    
    if input.KeyCode == Enum.KeyCode.RightShift then
        toggleHubVisibility()
    elseif input.KeyCode == Enum.KeyCode.F10 then
        toggleHubVisibility()
    end
end)

-- Inicialmente mostrar apenas o botão flutuante se for mobile
if isMobile then
    task.wait(1)
    toggleHubVisibility() -- Começar com hub fechado e botão visível
end

print("✅ Botão flutuante criado - Clique no 🍎 para abrir")

-- ============================================
-- LOG FINAL
-- ============================================

print("════════════════════════════════════")
print("🍎 APPLE HUB - TELEGUIA INSTANTÂNEO")
print("📱 Plataforma: " .. (isMobile and "MOBILE" or "PC"))
print("🎯 Scripts: " .. #mainScripts .. " disponíveis")
print("🚀 Teleguiado: INSTANTÂNEO (CFrame direto)")
print("📍 Marcador: Visual com aura vermelha")
print("🖱️  Drag: Cabeçalho arrastável")
print("📦 Minimizar: Funcional")
print("🔧 Atalho: RightShift/F10 para esconder")
print("════════════════════════════════════")
print("✨ HUB PRONTO PARA USO!")
print("✨ Pressione RightShift para esconder/mostrar")
print("✨ Arraste o cabeçalho para mover")
print("════════════════════════════════════")
print("🎮 COMO USAR TELEGUIADO:")
print("1. Clique em 'MARCAR POSIÇÃO'")
print("2. Mova-se para onde quiser")
print("3. Clique em 'TELEGUIADO'")
print("4. Você será teleportado INSTANTANEAMENTE!")
print("════════════════════════════════════")
