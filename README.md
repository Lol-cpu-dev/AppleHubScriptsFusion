-- 🍎 APPLE HUB ULTRA - VERSÃO 4 ABAS
-- 📱💻 Totalmente otimizado para ambos dispositivos
-- 🎮 ABA SAB - Scripts Steal a Brainrot + ICE HUB
-- 🌙 ABA 99 NIGHTS - Fox Hub
-- ⛏️ ABA DIG TO ESCAPE - Item TP
-- 💀 ABA DEAD RAILS - Auto Farm Bond
-- 🔒 SISTEMA DE WHITELIST POR NICK

-- ============================================
-- CONFIGURAÇÃO INICIAL RESPONSIVA
-- ============================================

print("🚀 INICIANDO APPLE HUB ULTRA - 4 ABAS...")

-- Configurações globais
getgenv().AppleHubUltra = {
    Version = "6.0.0",
    WhitelistEnabled = true,
    DebugMode = false,
    MaxWhitelistUsers = 50
}

-- 🔥 WHITELIST DE USUÁRIOS AUTORIZADOS
local WHITELISTED_USERS = {
    "el_gato9997",  -- ⚠️ SUBSTITUA PELO SEU NICK!
    "contadebrainrotr",
    "hekx6w",
    "Admin",
    "VIPPlayer"
}

-- ============================================
-- VERIFICAÇÃO DE WHITELIST
-- ============================================

local function checkWhitelist()
    if not getgenv().AppleHubUltra.WhitelistEnabled then
        print("⚠️ Whitelist desativada - Acesso livre")
        return true
    end
    
    local localPlayer = game:GetService("Players").LocalPlayer
    local playerName = localPlayer.Name
    
    print("🔍 Verificando whitelist para: " .. playerName)
    
    for _, allowedName in ipairs(WHITELISTED_USERS) do
        if playerName:lower() == allowedName:lower() then
            print("✅ ✅ ✅ ACESSO AUTORIZADO para: " .. playerName)
            return true
        end
    end
    
    print("❌ ❌ ❌ ACESSO NEGADO: " .. playerName .. " não está na whitelist")
    
    local deniedGui = Instance.new("ScreenGui")
    deniedGui.Name = "AccessDeniedScreen"
    deniedGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    deniedGui.Parent = game:GetService("CoreGui")
    
    local background = Instance.new("Frame")
    background.Size = UDim2.new(1, 0, 1, 0)
    background.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    background.BackgroundTransparency = 0.3
    background.Parent = deniedGui
    
    local messageFrame = Instance.new("Frame")
    messageFrame.Size = UDim2.new(0, 450, 0, 280)
    messageFrame.Position = UDim2.new(0.5, -225, 0.5, -140)
    messageFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
    messageFrame.BorderSizePixel = 0
    
    local messageCorner = Instance.new("UICorner")
    messageCorner.CornerRadius = UDim.new(0, 15)
    messageCorner.Parent = messageFrame
    
    messageFrame.Parent = deniedGui
    
    local lockIcon = Instance.new("TextLabel")
    lockIcon.Size = UDim2.new(0, 80, 0, 80)
    lockIcon.Position = UDim2.new(0.5, -40, 0, 30)
    lockIcon.BackgroundTransparency = 1
    lockIcon.Text = "🔒"
    lockIcon.TextSize = 60
    lockIcon.TextColor3 = Color3.fromRGB(220, 60, 60)
    lockIcon.Font = Enum.Font.GothamBold
    lockIcon.Parent = messageFrame
    
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(0.8, 0, 0, 40)
    title.Position = UDim2.new(0.1, 0, 0.4, 0)
    title.BackgroundTransparency = 1
    title.Text = "ACESSO NEGADO"
    title.TextSize = 26
    title.TextColor3 = Color3.fromRGB(255, 100, 100)
    title.Font = Enum.Font.GothamBlack
    title.Parent = messageFrame
    
    local message = Instance.new("TextLabel")
    message.Size = UDim2.new(0.8, 0, 0, 80)
    message.Position = UDim2.new(0.1, 0, 0.55, 0)
    message.BackgroundTransparency = 1
    message.Text = "Seu nick não está na whitelist!\n\nNick: " .. playerName .. "\n\nAdicione seu nick no código."
    message.TextSize = 16
    message.TextColor3 = Color3.fromRGB(200, 200, 220)
    message.Font = Enum.Font.GothamMedium
    message.TextWrapped = true
    message.Parent = messageFrame
    
    local closeButton = Instance.new("TextButton")
    closeButton.Size = UDim2.new(0.6, 0, 0, 45)
    closeButton.Position = UDim2.new(0.2, 0, 0.85, 0)
    closeButton.BackgroundColor3 = Color3.fromRGB(220, 60, 60)
    closeButton.Text = "FECHAR"
    closeButton.TextSize = 18
    closeButton.TextColor3 = Color3.new(1, 1, 1)
    closeButton.Font = Enum.Font.GothamBold
    
    local buttonCorner = Instance.new("UICorner")
    buttonCorner.CornerRadius = UDim.new(0, 8)
    buttonCorner.Parent = closeButton
    
    closeButton.MouseButton1Click:Connect(function()
        deniedGui:Destroy()
    end)
    
    closeButton.Parent = messageFrame
    
    return false
end

-- ============================================
-- INICIALIZAÇÃO PRINCIPAL
-- ============================================

if not checkWhitelist() then
    print("⛔ Hub não iniciado - Usuário não autorizado")
    return
end

print("✅ Whitelist aprovada - Iniciando hub...")

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

local isMobile = UserInputService.TouchEnabled

while not player do
    task.wait(0.1)
    player = Players.LocalPlayer
end

while not workspace.CurrentCamera do
    task.wait(0.1)
end

local screenSize = workspace.CurrentCamera.ViewportSize

-- ============================================
-- CONFIGURAÇÕES DE DESIGN
-- ============================================

local function getUltraSettings()
    if isMobile then
        return {
            width = math.min(400, screenSize.X * 0.94),
            height = 720,
            buttonHeight = 56,
            titleSize = 20,
            textSize = 14,
            iconSize = 26,
            buttonSpacing = 10,
            startPos = UDim2.new(0.5, 0, 0.22, 0),
            anchor = Vector2.new(0.5, 0),
            scrollThickness = 6,
            badgeSize = 34,
            badgeText = 9,
            notificationHeight = 40,
            categoryHeight = 46,
            tabButtonHeight = 40,
            headerHeight = 80,
            borderRadius = 16,
            buttonRadius = 10
        }
    else
        return {
            width = 520,
            height = 780,
            buttonHeight = 52,
            titleSize = 22,
            textSize = 15,
            iconSize = 24,
            buttonSpacing = 12,
            startPos = UDim2.new(0.5, 0, 0.5, 0),
            anchor = Vector2.new(0.5, 0.5),
            scrollThickness = 5,
            badgeSize = 30,
            badgeText = 8,
            notificationHeight = 36,
            categoryHeight = 44,
            tabButtonHeight = 38,
            headerHeight = 78,
            borderRadius = 14,
            buttonRadius = 8
        }
    end
end

local settings = getUltraSettings()

-- Variáveis globais
local dragging = false
local dragStart = nil
local startPos = nil

-- Cores
local colorPalette = {
    primary = Color3.fromRGB(255, 100, 100),
    secondary = Color3.fromRGB(100, 200, 255),
    accent = Color3.fromRGB(255, 193, 7),
    danger = Color3.fromRGB(220, 60, 60),
    success = Color3.fromRGB(0, 200, 83),
    dark1 = Color3.fromRGB(15, 15, 25),
    dark2 = Color3.fromRGB(25, 25, 35),
    dark3 = Color3.fromRGB(35, 35, 45),
    dark4 = Color3.fromRGB(45, 45, 55),
    light1 = Color3.new(1, 1, 1),
    light2 = Color3.fromRGB(220, 220, 220),
    light3 = Color3.fromRGB(180, 180, 200),
    sab = Color3.fromRGB(0, 150, 255),        -- Azul para SAB
    nights = Color3.fromRGB(255, 140, 0),     -- Laranja para 99 Nights
    dig = Color3.fromRGB(160, 120, 80),       -- Marrom para Dig to Escape
    dead = Color3.fromRGB(178, 34, 34),       -- Vermelho para Dead Rails
    ice = Color3.fromRGB(0, 200, 255)         -- Azul claro para Ice Hub
}

-- ============================================
-- CRIAÇÃO DA GUI
-- ============================================

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AppleHubUltra"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false
ScreenGui.DisplayOrder = 999

local success, err = pcall(function()
    ScreenGui.Parent = game:GetService("CoreGui")
end)

if not success then
    ScreenGui.Parent = player:WaitForChild("PlayerGui")
end

ScreenGui.Enabled = true
print("✨ GUI Ultra criada com sucesso!")

-- Frame principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, settings.width, 0, settings.height)
MainFrame.Position = settings.startPos
MainFrame.BackgroundColor3 = colorPalette.dark1
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true
MainFrame.AnchorPoint = settings.anchor
MainFrame.Parent = ScreenGui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, settings.borderRadius)
corner.Parent = MainFrame

local border = Instance.new("UIStroke")
border.Color = colorPalette.light3
border.Thickness = isMobile and 3 or 2.5
border.Transparency = 0.3
border.Parent = MainFrame

-- Cabeçalho
local Header = Instance.new("Frame")
Header.Name = "Header"
Header.Size = UDim2.new(1, 0, 0, settings.headerHeight)
Header.BackgroundColor3 = colorPalette.dark2
Header.BorderSizePixel = 0
Header.Active = true
Header.Selectable = true
Header.Parent = MainFrame

-- Ícone
local AppleIcon = Instance.new("TextLabel")
AppleIcon.Name = "AppleIcon"
AppleIcon.Size = UDim2.new(0, isMobile and 55 or 52, 0, isMobile and 55 or 52)
AppleIcon.Position = UDim2.new(0, 15, 0.5, -AppleIcon.Size.Y.Offset/2)
AppleIcon.BackgroundTransparency = 1
AppleIcon.Text = "🍎"
AppleIcon.TextSize = isMobile and 42 or 38
AppleIcon.TextColor3 = colorPalette.primary
AppleIcon.Font = Enum.Font.GothamBold
AppleIcon.TextXAlignment = Enum.TextXAlignment.Center
AppleIcon.Parent = Header

-- Título
local TitleContainer = Instance.new("Frame")
TitleContainer.Size = UDim2.new(1, -150, 1, 0)
TitleContainer.Position = UDim2.new(0, isMobile and 80 or 75, 0, 0)
TitleContainer.BackgroundTransparency = 1
TitleContainer.Parent = Header

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(1, 0, 0.6, 0)
TitleLabel.Position = UDim2.new(0, 0, 0, 10)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "APPLE HUB ULTRA"
TitleLabel.TextSize = settings.titleSize
TitleLabel.TextColor3 = colorPalette.light1
TitleLabel.Font = Enum.Font.GothamBlack
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
TitleLabel.Parent = TitleContainer

local Subtitle = Instance.new("TextLabel")
Subtitle.Size = UDim2.new(1, 0, 0.4, 0)
Subtitle.Position = UDim2.new(0, 0, 0, settings.headerHeight * 0.55)
Subtitle.BackgroundTransparency = 1
Subtitle.Text = "v" .. getgenv().AppleHubUltra.Version .. " | 4 ABAS COMPLETAS"
Subtitle.TextSize = isMobile and 11 or 10
Subtitle.TextColor3 = colorPalette.light3
Subtitle.Font = Enum.Font.GothamMedium
Subtitle.TextXAlignment = Enum.TextXAlignment.Left
Subtitle.Parent = TitleContainer

-- Badge de whitelist
local WhitelistBadge = Instance.new("TextButton")
WhitelistBadge.Name = "WhitelistBadge"
WhitelistBadge.Size = UDim2.new(0, isMobile and 100 or 95, 0, isMobile and 26 or 24)
WhitelistBadge.Position = UDim2.new(0.5, -50, 1, -30)
WhitelistBadge.BackgroundColor3 = colorPalette.success
WhitelistBadge.Text = "🔓 WHITELISTED"
WhitelistBadge.TextSize = isMobile and 10 or 9
WhitelistBadge.TextColor3 = colorPalette.light1
WhitelistBadge.Font = Enum.Font.GothamBold
WhitelistBadge.TextXAlignment = Enum.TextXAlignment.Center
WhitelistBadge.AutoButtonColor = false

local BadgeCorner = Instance.new("UICorner")
BadgeCorner.CornerRadius = UDim.new(0, isMobile and 8 or 7)
BadgeCorner.Parent = WhitelistBadge

WhitelistBadge.Parent = Header

-- Botões de controle
local buttonCtrlSize = isMobile and 40 or 38
local buttonSpacing = 10

local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Size = UDim2.new(0, buttonCtrlSize, 0, buttonCtrlSize)
MinimizeButton.Position = UDim2.new(1, -(buttonCtrlSize * 3 + buttonSpacing * 2 + 60), 0.5, -buttonCtrlSize/2)
MinimizeButton.BackgroundColor3 = colorPalette.accent
MinimizeButton.Text = "─"
MinimizeButton.TextColor3 = Color3.new(0, 0, 0)
MinimizeButton.TextSize = isMobile and 20 or 18
MinimizeButton.Font = Enum.Font.GothamBold
MinimizeButton.AutoButtonColor = false

local MinimizeCorner = Instance.new("UICorner")
MinimizeCorner.CornerRadius = UDim.new(0, isMobile and 10 or 9)
MinimizeCorner.Parent = MinimizeButton

local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, buttonCtrlSize, 0, buttonCtrlSize)
CloseButton.Position = UDim2.new(1, -50, 0.5, -buttonCtrlSize/2)
CloseButton.BackgroundColor3 = colorPalette.danger
CloseButton.Text = "✕"
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.TextSize = isMobile and 20 or 18
CloseButton.Font = Enum.Font.GothamBold
CloseButton.AutoButtonColor = false

local CloseButtonCorner = Instance.new("UICorner")
CloseButtonCorner.CornerRadius = UDim.new(0, isMobile and 10 or 9)
CloseButtonCorner.Parent = CloseButton

MinimizeButton.Parent = Header
CloseButton.Parent = Header

-- ============================================
-- SISTEMA DE DRAG
-- ============================================

local function startDrag(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or 
       (isMobile and input.UserInputType == Enum.UserInputType.Touch) then
        dragging = true
        dragStart = input.Position
        startPos = MainFrame.Position
    end
end

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

local function stopDrag()
    dragging = false
end

Header.InputBegan:Connect(startDrag)
Header.InputChanged:Connect(updateDrag)

if not isMobile then
    Header.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            stopDrag()
        end
    end)
end

-- ============================================
-- SISTEMA DE 4 ABAS
-- ============================================

local SideTabsContainer = Instance.new("Frame")
SideTabsContainer.Name = "SideTabsContainer"
SideTabsContainer.Size = UDim2.new(0, 110, 1, -(settings.headerHeight + 15))
SideTabsContainer.Position = UDim2.new(0, 15, 0, settings.headerHeight + 10)
SideTabsContainer.BackgroundTransparency = 1
SideTabsContainer.Parent = MainFrame

-- Aba SAB
local SABTab = Instance.new("TextButton")
SABTab.Name = "SABTab"
SABTab.Size = UDim2.new(1, 0, 0, settings.tabButtonHeight)
SABTab.Position = UDim2.new(0, 0, 0, 0)
SABTab.BackgroundColor3 = colorPalette.dark3
SABTab.Text = "🎮 SAB"
SABTab.TextColor3 = colorPalette.sab
SABTab.TextSize = isMobile and 13 or 12
SABTab.Font = Enum.Font.GothamBold
SABTab.AutoButtonColor = false

local SABTabCorner = Instance.new("UICorner")
SABTabCorner.CornerRadius = UDim.new(0, settings.buttonRadius)
SABTabCorner.Parent = SABTab

-- Aba 99 Nights
local NightsTab = Instance.new("TextButton")
NightsTab.Name = "NightsTab"
NightsTab.Size = UDim2.new(1, 0, 0, settings.tabButtonHeight)
NightsTab.Position = UDim2.new(0, 0, 0, settings.tabButtonHeight + 6)
NightsTab.BackgroundColor3 = colorPalette.dark2
NightsTab.Text = "🌙 99 NIGHTS"
NightsTab.TextColor3 = colorPalette.light3
NightsTab.TextSize = isMobile and 12 or 11
NightsTab.Font = Enum.Font.GothamBold
NightsTab.AutoButtonColor = false

local NightsTabCorner = Instance.new("UICorner")
NightsTabCorner.CornerRadius = UDim.new(0, settings.buttonRadius)
NightsTabCorner.Parent = NightsTab

-- Aba Dig to Escape
local DigTab = Instance.new("TextButton")
DigTab.Name = "DigTab"
DigTab.Size = UDim2.new(1, 0, 0, settings.tabButtonHeight)
DigTab.Position = UDim2.new(0, 0, 0, (settings.tabButtonHeight + 6) * 2)
DigTab.BackgroundColor3 = colorPalette.dark2
DigTab.Text = "⛏️ DIG TO ESCAPE"
DigTab.TextColor3 = colorPalette.light3
DigTab.TextSize = isMobile and 11 or 10
DigTab.Font = Enum.Font.GothamBold
DigTab.AutoButtonColor = false

local DigTabCorner = Instance.new("UICorner")
DigTabCorner.CornerRadius = UDim.new(0, settings.buttonRadius)
DigTabCorner.Parent = DigTab

-- Aba Dead Rails
local DeadTab = Instance.new("TextButton")
DeadTab.Name = "DeadTab"
DeadTab.Size = UDim2.new(1, 0, 0, settings.tabButtonHeight)
DeadTab.Position = UDim2.new(0, 0, 0, (settings.tabButtonHeight + 6) * 3)
DeadTab.BackgroundColor3 = colorPalette.dark2
DeadTab.Text = "💀 DEAD RAILS"
DeadTab.TextColor3 = colorPalette.light3
DeadTab.TextSize = isMobile and 11 or 10
DeadTab.Font = Enum.Font.GothamBold
DeadTab.AutoButtonColor = false

local DeadTabCorner = Instance.new("UICorner")
DeadTabCorner.CornerRadius = UDim.new(0, settings.buttonRadius)
DeadTabCorner.Parent = DeadTab

SABTab.Parent = SideTabsContainer
NightsTab.Parent = SideTabsContainer
DigTab.Parent = SideTabsContainer
DeadTab.Parent = SideTabsContainer

-- ============================================
-- CONTAINER DE CONTEÚDO
-- ============================================

local ContentContainer = Instance.new("Frame")
ContentContainer.Name = "ContentContainer"
ContentContainer.Size = UDim2.new(1, -130, 1, -(settings.headerHeight + 20))
ContentContainer.Position = UDim2.new(0, 125, 0, settings.headerHeight + 10)
ContentContainer.BackgroundTransparency = 1
ContentContainer.Parent = MainFrame

-- Container para SAB
local SABContainer = Instance.new("ScrollingFrame")
SABContainer.Name = "SABContainer"
SABContainer.Size = UDim2.new(1, 0, 1, 0)
SABContainer.Position = UDim2.new(0, 0, 0, 0)
SABContainer.BackgroundTransparency = 1
SABContainer.ScrollBarThickness = settings.scrollThickness
SABContainer.ScrollBarImageColor3 = colorPalette.light3
SABContainer.AutomaticCanvasSize = Enum.AutomaticSize.Y
SABContainer.ScrollingEnabled = true
SABContainer.CanvasSize = UDim2.new(0, 0, 0, 0)
SABContainer.Visible = true
SABContainer.Parent = ContentContainer

-- Container para 99 Nights
local NightsContainer = Instance.new("ScrollingFrame")
NightsContainer.Name = "NightsContainer"
NightsContainer.Size = UDim2.new(1, 0, 1, 0)
NightsContainer.Position = UDim2.new(0, 0, 0, 0)
NightsContainer.BackgroundTransparency = 1
NightsContainer.ScrollBarThickness = settings.scrollThickness
NightsContainer.ScrollBarImageColor3 = colorPalette.light3
NightsContainer.AutomaticCanvasSize = Enum.AutomaticSize.Y
NightsContainer.ScrollingEnabled = true
NightsContainer.CanvasSize = UDim2.new(0, 0, 0, 0)
NightsContainer.Visible = false
NightsContainer.Parent = ContentContainer

-- Container para Dig to Escape
local DigContainer = Instance.new("ScrollingFrame")
DigContainer.Name = "DigContainer"
DigContainer.Size = UDim2.new(1, 0, 1, 0)
DigContainer.Position = UDim2.new(0, 0, 0, 0)
DigContainer.BackgroundTransparency = 1
DigContainer.ScrollBarThickness = settings.scrollThickness
DigContainer.ScrollBarImageColor3 = colorPalette.light3
DigContainer.AutomaticCanvasSize = Enum.AutomaticSize.Y
DigContainer.ScrollingEnabled = true
DigContainer.CanvasSize = UDim2.new(0, 0, 0, 0)
DigContainer.Visible = false
DigContainer.Parent = ContentContainer

-- Container para Dead Rails
local DeadContainer = Instance.new("ScrollingFrame")
DeadContainer.Name = "DeadContainer"
DeadContainer.Size = UDim2.new(1, 0, 1, 0)
DeadContainer.Position = UDim2.new(0, 0, 0, 0)
DeadContainer.BackgroundTransparency = 1
DeadContainer.ScrollBarThickness = settings.scrollThickness
DeadContainer.ScrollBarImageColor3 = colorPalette.light3
DeadContainer.AutomaticCanvasSize = Enum.AutomaticSize.Y
DeadContainer.ScrollingEnabled = true
DeadContainer.CanvasSize = UDim2.new(0, 0, 0, 0)
DeadContainer.Visible = false
DeadContainer.Parent = ContentContainer

local SABLayout = Instance.new("UIListLayout")
SABLayout.Padding = UDim.new(0, settings.buttonSpacing)
SABLayout.SortOrder = Enum.SortOrder.LayoutOrder
SABLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
SABLayout.Parent = SABContainer

local NightsLayout = Instance.new("UIListLayout")
NightsLayout.Padding = UDim.new(0, settings.buttonSpacing)
NightsLayout.SortOrder = Enum.SortOrder.LayoutOrder
NightsLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
NightsLayout.Parent = NightsContainer

local DigLayout = Instance.new("UIListLayout")
DigLayout.Padding = UDim.new(0, settings.buttonSpacing)
DigLayout.SortOrder = Enum.SortOrder.LayoutOrder
DigLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
DigLayout.Parent = DigContainer

local DeadLayout = Instance.new("UIListLayout")
DeadLayout.Padding = UDim.new(0, settings.buttonSpacing)
DeadLayout.SortOrder = Enum.SortOrder.LayoutOrder
DeadLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
DeadLayout.Parent = DeadContainer

-- ============================================
-- SISTEMA DE NOTIFICAÇÕES
-- ============================================

local function showNotification(message, color, duration, icon)
    duration = duration or 3
    icon = icon or "✨"
    
    local notification = Instance.new("Frame")
    notification.Size = UDim2.new(1, -20, 0, settings.notificationHeight)
    notification.Position = UDim2.new(0, 10, 0, -60)
    notification.BackgroundColor3 = color
    notification.ZIndex = 1000
    
    local notifCorner = Instance.new("UICorner")
    notifCorner.CornerRadius = UDim.new(0, settings.buttonRadius)
    notifCorner.Parent = notification
    
    local notifIcon = Instance.new("TextLabel")
    notifIcon.Size = UDim2.new(0, 30, 1, 0)
    notifIcon.Position = UDim2.new(0, 10, 0, 0)
    notifIcon.BackgroundTransparency = 1
    notifIcon.Text = icon
    notifIcon.TextSize = 20
    notifIcon.TextColor3 = Color3.new(1, 1, 1)
    notifIcon.Font = Enum.Font.GothamBold
    notifIcon.Parent = notification
    
    local notifText = Instance.new("TextLabel")
    notifText.Size = UDim2.new(1, -50, 1, 0)
    notifText.Position = UDim2.new(0, 45, 0, 0)
    notifText.BackgroundTransparency = 1
    notifText.Text = message
    notifText.TextSize = isMobile and 14 or 13
    notifText.TextColor3 = Color3.new(1, 1, 1)
    notifText.Font = Enum.Font.GothamBold
    notifText.TextXAlignment = Enum.TextXAlignment.Left
    notifText.TextWrapped = true
    notifText.Parent = notification
    
    notification.Parent = MainFrame
    
    notification:TweenPosition(UDim2.new(0, 10, 0, 10), "Out", "Quad", 0.3, true)
    
    task.wait(duration)
    
    notification:TweenPosition(UDim2.new(0, 10, 0, -60), "Out", "Quad", 0.3, true)
    
    task.wait(0.3)
    notification:Destroy()
end

-- ============================================
-- FUNÇÃO PARA MUDAR DE ABA
-- ============================================

local currentTab = "SAB"

local function switchTab(tabName)
    currentTab = tabName
    
    -- Resetar todas as abas
    SABTab.BackgroundColor3 = colorPalette.dark2
    SABTab.TextColor3 = colorPalette.light3
    
    NightsTab.BackgroundColor3 = colorPalette.dark2
    NightsTab.TextColor3 = colorPalette.light3
    
    DigTab.BackgroundColor3 = colorPalette.dark2
    DigTab.TextColor3 = colorPalette.light3
    
    DeadTab.BackgroundColor3 = colorPalette.dark2
    DeadTab.TextColor3 = colorPalette.light3
    
    SABContainer.Visible = false
    NightsContainer.Visible = false
    DigContainer.Visible = false
    DeadContainer.Visible = false
    
    -- Ativar aba selecionada
    if tabName == "SAB" then
        SABTab.BackgroundColor3 = colorPalette.dark3
        SABTab.TextColor3 = colorPalette.sab
        SABContainer.Visible = true
        showNotification("🎮 Aba SAB ativada", colorPalette.sab, 2, "🎮")
        
    elseif tabName == "99NIGHTS" then
        NightsTab.BackgroundColor3 = colorPalette.dark3
        NightsTab.TextColor3 = colorPalette.nights
        NightsContainer.Visible = true
        showNotification("🌙 Aba 99 Nights ativada", colorPalette.nights, 2, "🌙")
        
    elseif tabName == "DIG" then
        DigTab.BackgroundColor3 = colorPalette.dark3
        DigTab.TextColor3 = colorPalette.dig
        DigContainer.Visible = true
        showNotification("⛏️ Aba Dig to Escape ativada", colorPalette.dig, 2, "⛏️")
        
    elseif tabName == "DEAD" then
        DeadTab.BackgroundColor3 = colorPalette.dark3
        DeadTab.TextColor3 = colorPalette.dead
        DeadContainer.Visible = true
        showNotification("💀 Aba Dead Rails ativada", colorPalette.dead, 2, "💀")
    end
end

SABTab.MouseButton1Click:Connect(function() switchTab("SAB") end)
NightsTab.MouseButton1Click:Connect(function() switchTab("99NIGHTS") end)
DigTab.MouseButton1Click:Connect(function() switchTab("DIG") end)
DeadTab.MouseButton1Click:Connect(function() switchTab("DEAD") end)

if isMobile then
    SABTab.TouchTap:Connect(function() switchTab("SAB") end)
    NightsTab.TouchTap:Connect(function() switchTab("99NIGHTS") end)
    DigTab.TouchTap:Connect(function() switchTab("DIG") end)
    DeadTab.TouchTap:Connect(function() switchTab("DEAD") end)
end

-- ============================================
-- LISTA DE SCRIPTS SAB (STEAL A BRAINROT + ICE HUB)
-- ============================================

local sabScripts = {
    -- ICE HUB (PRIMEIRO)
    {name = "ICE HUB", icon = "🧊", color = colorPalette.ice, 
     url = "https://api.luarmor.net/files/v3/loaders/26a720688fde4907da845a1314a1ce5e.lua", badge = "NEW"},
    
    -- Scripts originais
    {name = "Nameless Executor", icon = "👑", color = Color3.fromRGB(255, 215, 0), 
     url = "https://raw.githubusercontent.com/ily123950/Vulkan/main/Tr"},
    
    {name = "Chilli Hub", icon = "🌶️", color = Color3.fromRGB(255, 69, 58), 
     url = "https://raw.githubusercontent.com/tienkhanh1/spicy/main/Chilli.lua"},
    
    {name = "Ultimate Control", icon = "⚡", color = Color3.fromRGB(0, 191, 255), 
     url = "https://raw.githubusercontent.com/UCT-hub/main/main/stealabrainrot"},
    
    {name = "Kurd Hub", icon = "🏔️", color = Color3.fromRGB(34, 139, 34), 
     url = "https://raw.githubusercontent.com/Ninja10908/S4/main/Kurdhub"},
    
    {name = "Server Destroyer V6", icon = "💀", color = Color3.fromRGB(178, 34, 34), 
     url = "https://tcscripts.discloud.app/scripts/serverdestroyerv6", special = true},
    
    {name = "Admin Spam Tool", icon = "👑", color = Color3.fromRGB(255, 215, 0), 
     url = "https://api.luarmor.net/files/v3/loaders/fc9523e876bada3b7ed4ebe004cb8cf9.lua"},
    
    {name = "Chilli Private", icon = "🔓", color = Color3.fromRGB(0, 200, 83), 
     url = "https://raw.githubusercontent.com/tienkhanh1/spicy/main/PrivateServer"},
    
    {name = "Speed Steal Hack", icon = "⚡", color = Color3.fromRGB(0, 150, 255), 
     url = "https://pastebin.com/raw/rmxfZDPd"},
    
    {name = "Auto Ping Pong", icon = "🏓", color = Color3.fromRGB(0, 230, 118), 
     url = "https://raw.githubusercontent.com/LucasggkX/Games/main/Auto%20buy%20-%20Ping%20Pong.lua"},
    
    {name = "Paintball Hack", icon = "🎯", color = Color3.fromRGB(255, 87, 34), 
     url = "https://raw.githubusercontent.com/LucasggkX/Games/main/Paintball.lua"},
    
    {name = "Lennon Music Hub", icon = "🎵", color = Color3.fromRGB(156, 39, 176), 
     url = "https://raw.githubusercontent.com/mxrtnttzflw/martinetti-scripts/main/script.lua"},
    
    {name = "LKZ Helper OP", icon = "🔧", color = Color3.fromRGB(0, 180, 216), 
     url = "https://raw.githubusercontent.com/LucasggkX/Games/main/LKZ%20Helper.lua"},
    
    {name = "Miranda Hub 2024", icon = "⚡", color = Color3.fromRGB(0, 200, 255), 
     url = "https://pastefy.app/JJVhs3rK/raw"},
    
    {name = "Miranda V2", icon = "⚡", color = Color3.fromRGB(0, 180, 255), 
     url = "https://raw.githubusercontent.com/NagisaScript1/FakeModz/refs/heads/main/Miranda-Steal-a-Brainrot"},
    
    {name = "Adidas Animations", icon = "👟", color = Color3.fromRGB(0, 0, 0), 
     url = "https://raw.githubusercontent.com/kolllooomcj-bit/MCJANIMATION/refs/heads/main/ADIDAS%20COMMUNITY%20MCJ"},
    
    {name = "Auto Block on Steal", icon = "🛡️", color = Color3.fromRGB(0, 150, 255), 
     url = "https://raw.githubusercontent.com/LucasggkX/Games/refs/heads/main/Auto%20Block.lua"}
}

-- ============================================
-- LISTA DE SCRIPTS 99 NIGHTS
-- ============================================

local nightsScripts = {
    {
        name = "FOX HUB", 
        icon = "🦊", 
        color = colorPalette.nights, 
        description = "Hub completo para vários jogos",
        url = "https://raw.githubusercontent.com/caomod2077/Script/refs/heads/main/FoxnameHub.lua"
    }
}

-- ============================================
-- LISTA DE SCRIPTS DIG TO ESCAPE
-- ============================================

local digScripts = {
    {
        name = "ITEM TP", 
        icon = "📍", 
        color = colorPalette.dig, 
        description = "Teleporte automático para itens no Dig to Escape",
        url = "https://raw.githubusercontent.com/SlayingAgain/Hook-Software/refs/heads/main/Dig-to-Escape"
    }
}

-- ============================================
-- LISTA DE SCRIPTS DEAD RAILS
-- ============================================

local deadScripts = {
    {
        name = "AUTO FARM BOND", 
        icon = "💎", 
        color = colorPalette.dead, 
        description = "Farm automático de bond no Dead Rails",
        url = "https://raw.githubusercontent.com/Lol-cpu-dev/AppleHubBondFarm/refs/heads/main/README.md"
    }
}

-- ============================================
-- FUNÇÃO PARA CRIAR BOTÕES
-- ============================================

local function createUltraButton(buttonData, index, container, showDescription)
    local button = Instance.new("TextButton")
    button.Name = "Btn_" .. buttonData.name:gsub("%s+", "")
    button.Size = UDim2.new(0.96, 0, 0, settings.buttonHeight + (showDescription and 15 or 0))
    button.BackgroundColor3 = showDescription and colorPalette.dark2 or colorPalette.dark3
    button.BorderSizePixel = 0
    button.Text = ""
    button.AutoButtonColor = false
    button.LayoutOrder = index
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, settings.buttonRadius)
    corner.Parent = button
    
    local border = Instance.new("UIStroke")
    border.Color = buttonData.color
    border.Thickness = showDescription and 2.5 or 2
    border.Parent = button
    
    local icon = Instance.new("TextLabel")
    icon.Name = "Icon"
    icon.Size = UDim2.new(0, settings.iconSize, 0, settings.iconSize)
    icon.Position = UDim2.new(0, 15, 0.5, -settings.iconSize/2)
    icon.BackgroundTransparency = 1
    icon.Text = buttonData.icon
    icon.TextSize = settings.iconSize - (isMobile and 6 or 5)
    icon.TextColor3 = buttonData.color
    icon.Font = Enum.Font.GothamBold
    icon.Parent = button
    
    local label = Instance.new("TextLabel")
    label.Name = "Label"
    label.Size = UDim2.new(1, -(settings.iconSize + 25), 0.6, 0)
    label.Position = UDim2.new(0, settings.iconSize + 20, 0, 8)
    label.BackgroundTransparency = 1
    label.Text = buttonData.name
    label.TextSize = settings.textSize
    label.TextColor3 = colorPalette.light1
    label.Font = Enum.Font.GothamBold
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.TextTruncate = Enum.TextTruncate.AtEnd
    label.Parent = button
    
    if showDescription then
        local desc = Instance.new("TextLabel")
        desc.Name = "Description"
        desc.Size = UDim2.new(1, -(settings.iconSize + 25), 0.4, 0)
        desc.Position = UDim2.new(0, settings.iconSize + 20, 0, settings.buttonHeight/2 + 6)
        desc.BackgroundTransparency = 1
        desc.Text = buttonData.description
        desc.TextSize = isMobile and 12 or 11
        desc.TextColor3 = colorPalette.light3
        desc.Font = Enum.Font.GothamMedium
        desc.TextXAlignment = Enum.TextXAlignment.Left
        desc.TextTruncate = Enum.TextTruncate.AtEnd
        desc.Parent = button
    end
    
    if buttonData.badge then
        local badge = Instance.new("Frame")
        badge.Name = "Badge"
        badge.Size = UDim2.new(0, 50, 0, isMobile and 24 or 22)
        badge.Position = UDim2.new(1, -55, 0.1, 0)
        badge.BackgroundColor3 = buttonData.color
        
        local badgeCorner = Instance.new("UICorner")
        badgeCorner.CornerRadius = UDim.new(0, isMobile and 7 or 6)
        badgeCorner.Parent = badge
        
        local badgeLabel = Instance.new("TextLabel")
        badgeLabel.Size = UDim2.new(1, 0, 1, 0)
        badgeLabel.BackgroundTransparency = 1
        badgeLabel.Text = buttonData.badge
        badgeLabel.TextColor3 = Color3.new(1, 1, 1)
        badgeLabel.TextSize = isMobile and 10 or 9
        badgeLabel.Font = Enum.Font.GothamBold
        badgeLabel.Parent = badge
        
        badge.Parent = button
    end
    
    button.Parent = container
    return button, border
end

-- ============================================
-- CRIAR SCRIPTS SAB
-- ============================================

for i, scriptData in ipairs(sabScripts) do
    local button, border = createUltraButton(scriptData, i, SABContainer, false)
    
    button.MouseButton1Click:Connect(function()
        local originalIcon = button.Icon.Text
        
        button.Icon.Text = "⏳"
        
        local success, err = pcall(function()
            if scriptData.special then
                getgenv().ServerDestroyerV6 = {Comprar = false, Spam = true}
            end
            
            if scriptData.name == "ICE HUB" then
                loadstring(game:HttpGet("https://api.luarmor.net/files/v3/loaders/26a720688fde4907da845a1314a1ce5e.lua"))()
            else
                local scriptContent = game:HttpGet(scriptData.url, true)
                loadstring(scriptContent)()
            end
        end)
        
        if success then
            button.Icon.Text = "✅"
            border.Color = colorPalette.success
            
            if scriptData.name == "ICE HUB" then
                showNotification("🧊 ICE HUB carregado com sucesso!", colorPalette.ice, 3, "🧊")
            else
                showNotification("✅ " .. scriptData.name .. " executado!", colorPalette.success, 3, "✅")
            end
        else
            button.Icon.Text = "❌"
            border.Color = colorPalette.danger
            showNotification("❌ Erro em " .. scriptData.name, colorPalette.danger, 3, "❌")
        end
        
        task.wait(1.5)
        
        button.Icon.Text = originalIcon
        border.Color = scriptData.color
    end)
end

print("✅ " .. #sabScripts .. " scripts SAB criados")

-- ============================================
-- CRIAR SCRIPTS 99 NIGHTS
-- ============================================

for i, scriptData in ipairs(nightsScripts) do
    local button, border = createUltraButton(scriptData, i, NightsContainer, true)
    
    button.MouseButton1Click:Connect(function()
        local originalIcon = button.Icon.Text
        
        button.Icon.Text = "⏳"
        
        local success, err = pcall(function()
            loadstring(game:HttpGet("https://raw.githubusercontent.com/caomod2077/Script/refs/heads/main/FoxnameHub.lua"))()
        end)
        
        if success then
            button.Icon.Text = "✅"
            border.Color = colorPalette.success
            showNotification("🦊 FOX HUB carregado com sucesso!", colorPalette.nights, 3, "🦊")
        else
            button.Icon.Text = "❌"
            border.Color = colorPalette.danger
            showNotification("❌ Erro ao carregar FOX HUB", colorPalette.danger, 3, "❌")
        end
        
        task.wait(1.5)
        
        button.Icon.Text = originalIcon
        border.Color = scriptData.color
    end)
end

print("✅ " .. #nightsScripts .. " script 99 Nights criado")

-- ============================================
-- CRIAR SCRIPTS DIG TO ESCAPE
-- ============================================

for i, scriptData in ipairs(digScripts) do
    local button, border = createUltraButton(scriptData, i, DigContainer, true)
    
    button.MouseButton1Click:Connect(function()
        local originalIcon = button.Icon.Text
        
        button.Icon.Text = "⏳"
        
        local success, err = pcall(function()
            loadstring(game:HttpGet("https://raw.githubusercontent.com/SlayingAgain/Hook-Software/refs/heads/main/Dig-to-Escape"))()
        end)
        
        if success then
            button.Icon.Text = "✅"
            border.Color = colorPalette.success
            showNotification("⛏️ ITEM TP carregado com sucesso!", colorPalette.dig, 3, "⛏️")
        else
            button.Icon.Text = "❌"
            border.Color = colorPalette.danger
            showNotification("❌ Erro ao carregar ITEM TP", colorPalette.danger, 3, "❌")
        end
        
        task.wait(1.5)
        
        button.Icon.Text = originalIcon
        border.Color = scriptData.color
    end)
end

print("✅ " .. #digScripts .. " script Dig to Escape criado")

-- ============================================
-- CRIAR SCRIPTS DEAD RAILS
-- ============================================

for i, scriptData in ipairs(deadScripts) do
    local button, border = createUltraButton(scriptData, i, DeadContainer, true)
    
    button.MouseButton1Click:Connect(function()
        local originalIcon = button.Icon.Text
        
        button.Icon.Text = "⏳"
        
        local success, err = pcall(function()
            loadstring(game:HttpGet("https://api.getpolsec.com/scripts/files/8296210a2f042b36145c0db05bd725f73dcf465e2cd54e02ee843c6578a40a91.lua"))()
        end)
        
        if success then
            button.Icon.Text = "✅"
            border.Color = colorPalette.success
            showNotification("💀 AUTO FARM BOND carregado com sucesso!", colorPalette.dead, 3, "💀")
        else
            button.Icon.Text = "❌"
            border.Color = colorPalette.danger
            showNotification("❌ Erro ao carregar AUTO FARM BOND", colorPalette.danger, 3, "❌")
        end
        
        task.wait(1.5)
        
        button.Icon.Text = originalIcon
        border.Color = scriptData.color
    end)
end

print("✅ " .. #deadScripts .. " script Dead Rails criado")

-- ============================================
-- FUNCIONALIDADES DOS BOTÕES DE CONTROLE
-- ============================================

local isMinimized = false

local function toggleMinimize()
    isMinimized = not isMinimized
    
    if isMinimized then
        MainFrame.Size = UDim2.new(0, isMobile and 150 or 140, 0, settings.headerHeight)
        SideTabsContainer.Visible = false
        ContentContainer.Visible = false
        MinimizeButton.Text = "+"
        showNotification("📦 Hub minimizado", colorPalette.accent, 2, "📦")
    else
        MainFrame.Size = UDim2.new(0, settings.width, 0, settings.height)
        SideTabsContainer.Visible = true
        ContentContainer.Visible = true
        MinimizeButton.Text = "─"
        showNotification("📂 Hub restaurado", colorPalette.accent, 2, "📂")
    end
end

MinimizeButton.MouseButton1Click:Connect(toggleMinimize)

CloseButton.MouseButton1Click:Connect(function()
    MainFrame:TweenSize(UDim2.new(0, 0, 0, 0), "Out", "Quad", 0.3, true)
    
    showNotification("🔒 Apple Hub Ultra fechado", colorPalette.danger, 3, "🔒")
    
    task.wait(0.3)
    ScreenGui:Destroy()
end)

-- ============================================
-- SISTEMA DE TOGGLE COM TECLA
-- ============================================

local hubVisible = true

local function toggleHubVisibility()
    hubVisible = not hubVisible
    ScreenGui.Enabled = hubVisible
    
    if hubVisible then
        showNotification("🍎 HUB ULTRA ABERTO", colorPalette.primary, 2, "🍎")
    else
        showNotification("📂 HUB FECHADO (RightShift)", colorPalette.light3, 2, "📂")
    end
end

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    
    if input.KeyCode == Enum.KeyCode.RightShift then
        toggleHubVisibility()
    elseif input.KeyCode == Enum.KeyCode.F10 then
        toggleHubVisibility()
    end
end)

-- ============================================
-- RODAPÉ
-- ============================================

local footer = Instance.new("Frame")
footer.Size = UDim2.new(1, -20, 0, isMobile and 30 or 28)
footer.Position = UDim2.new(0, 10, 1, -35)
footer.BackgroundColor3 = colorPalette.dark2

local footerCorner = Instance.new("UICorner")
footerCorner.CornerRadius = UDim.new(0, settings.buttonRadius)
footerCorner.Parent = footer

local footerText = Instance.new("TextLabel")
footerText.Size = UDim2.new(1, 0, 1, 0)
footerText.BackgroundTransparency = 1
footerText.Text = "🎮 SAB: " .. #sabScripts .. " | 🌙 99 N: 1 | ⛏️ DIG: 1 | 💀 DEAD: 1"
footerText.TextSize = isMobile and 10 or 9
footerText.TextColor3 = colorPalette.light3
footerText.Font = Enum.Font.GothamMedium
footerText.TextXAlignment = Enum.TextXAlignment.Center
footerText.Parent = footer

footer.Parent = MainFrame

-- ============================================
-- ATUALIZAR SCROLL AUTOMATICAMENTE
-- ============================================

task.spawn(function()
    while ScreenGui.Parent do
        SABContainer.CanvasSize = UDim2.new(0, 0, 0, SABLayout.AbsoluteContentSize.Y + 20)
        NightsContainer.CanvasSize = UDim2.new(0, 0, 0, NightsLayout.AbsoluteContentSize.Y + 20)
        DigContainer.CanvasSize = UDim2.new(0, 0, 0, DigLayout.AbsoluteContentSize.Y + 20)
        DeadContainer.CanvasSize = UDim2.new(0, 0, 0, DeadLayout.AbsoluteContentSize.Y + 20)
        task.wait(0.5)
    end
end)

-- ============================================
-- NOTIFICAÇÃO INICIAL
-- ============================================

task.wait(1)
showNotification(
    "🍎 APPLE HUB ULTRA v" .. getgenv().AppleHubUltra.Version .. 
    "\n🎮 SAB: " .. #sabScripts .. " scripts" ..
    "\n🌙 99 NIGHTS: Fox Hub" ..
    "\n⛏️ DIG TO ESCAPE: Item TP" ..
    "\n💀 DEAD RAILS: Auto Farm Bond" ..
    "\n👤 Usuário: " .. player.Name,
    colorPalette.primary,
    5,
    "✨"
)

-- ============================================
-- BOTÃO FLUTUANTE PARA MOBILE
-- ============================================

if isMobile then
    local FloatingButton = Instance.new("TextButton")
    FloatingButton.Name = "FloatingButton"
    FloatingButton.Size = UDim2.new(0, 70, 0, 70)
    FloatingButton.Position = UDim2.new(0, 20, 0.5, -35)
    FloatingButton.BackgroundColor3 = colorPalette.primary
    FloatingButton.Text = "🍎"
    FloatingButton.TextSize = 36
    FloatingButton.TextColor3 = Color3.new(1, 1, 1)
    FloatingButton.Font = Enum.Font.GothamBold
    FloatingButton.Visible = false
    FloatingButton.ZIndex = 9999
    FloatingButton.AutoButtonColor = false
    
    local floatCorner = Instance.new("UICorner")
    floatCorner.CornerRadius = UDim.new(1, 0)
    floatCorner.Parent = FloatingButton
    
    FloatingButton.Parent = ScreenGui
    
    FloatingButton.MouseButton1Click:Connect(function()
        ScreenGui.Enabled = true
        FloatingButton.Visible = false
        showNotification("🍎 HUB ULTRA ABERTO", colorPalette.primary, 2, "🍎")
    end)
    
    local originalToggle = toggleHubVisibility
    toggleHubVisibility = function()
        hubVisible = not hubVisible
        ScreenGui.Enabled = hubVisible
        FloatingButton.Visible = not hubVisible
        
        if hubVisible then
            showNotification("🍎 HUB ULTRA ABERTO", colorPalette.primary, 2, "🍎")
        else
            showNotification("📂 Clique no 🍎 para abrir", colorPalette.light3, 2, "📂")
        end
    end
    
    task.wait(2)
    toggleHubVisibility()
end

-- ============================================
-- LIMPEZA
-- ============================================

Players.PlayerRemoving:Connect(function(leavingPlayer)
    if leavingPlayer == player then
        if ScreenGui then
            ScreenGui:Destroy()
        end
    end
end)

-- ============================================
-- LOG FINAL
-- ============================================

print("\n" .. string.rep("=", 50))
print("✨ APPLE HUB ULTRA v" .. getgenv().AppleHubUltra.Version)
print(string.rep("=", 50))
print("✅ Whitelist: ATIVA")
print("✅ Usuário: " .. player.Name .. " AUTORIZADO")
print("📱 Plataforma: " .. (isMobile and "MOBILE" or "PC"))
print("🎮 SAB: " .. #sabScripts .. " scripts (com ICE HUB)")
print("🌙 99 NIGHTS: Fox Hub")
print("⛏️ DIG TO ESCAPE: Item TP")
print("💀 DEAD RAILS: Auto Farm Bond")
print(string.rep("=", 50))
print("🎮 CONTROLES:")
print("• RightShift/F10: Abrir/fechar hub")
print("• Arraste o cabeçalho: Mover hub")
print("• Botão 🍎 (mobile): Abrir hub")
print(string.rep("=", 50))
print("🔥 HUB ULTRA COM 4 ABAS PRONTO PARA USO!")
print(string.rep("=", 50))
