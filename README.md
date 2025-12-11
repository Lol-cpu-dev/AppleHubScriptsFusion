-- 🍎 APPLE HUB - VERSÃO RESPONSIVA PC/MOBILE
-- 📱💻 Totalmente otimizado para ambos dispositivos

-- ============================================
-- CONFIGURAÇÃO INICIAL RESPONSIVA
-- ============================================

-- Remover hubs antigos
for _, gui in pairs(game:GetService("CoreGui"):GetChildren()) do
    if gui.Name:find("Apple") or gui.Name:find("Hub") then
        gui:Destroy()
    end
end

print("🚀 INICIANDO APPLE HUB RESPONSIVO...")

-- Serviços
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer
local isMobile = UserInputService.TouchEnabled
local screenSize = workspace.CurrentCamera.ViewportSize

-- 🔧 DETECTAR DISPOSITIVO E CONFIGURAR
local function getDeviceSettings()
    if isMobile then
        print("📱 DETECTADO: MOBILE")
        return {
            width = math.min(360, screenSize.X * 0.92),
            height = 580,
            buttonHeight = 62,
            titleSize = 17,
            textSize = 15,
            iconSize = 28,
            buttonSpacing = 12,
            startPos = UDim2.new(0.5, 0, 0.25, 0),
            anchor = Vector2.new(0.5, 0),
            scrollThickness = 6,
            badgeSize = 38,
            badgeText = 10,
            notificationHeight = 42
        }
    else
        print("💻 DETECTADO: PC")
        return {
            width = 420,
            height = 620,
            buttonHeight = 56,
            titleSize = 19,
            textSize = 16,
            iconSize = 26,
            buttonSpacing = 14,
            startPos = UDim2.new(0.5, 0, 0.5, 0),
            anchor = Vector2.new(0.5, 0.5),
            scrollThickness = 4,
            badgeSize = 32,
            badgeText = 9,
            notificationHeight = 38
        }
    end
end

local settings = getDeviceSettings()

-- ============================================
-- CRIAÇÃO DA GUI RESPONSIVA
-- ============================================

-- ScreenGui principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AppleHubResponsive"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false
ScreenGui.DisplayOrder = 999
ScreenGui.Parent = game:GetService("CoreGui")

print("✅ GUI criada para: " .. (isMobile and "Mobile" or "PC"))

-- Frame principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, settings.width, 0, settings.height)
MainFrame.Position = settings.startPos
MainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 24)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true
MainFrame.AnchorPoint = settings.anchor
MainFrame.Parent = ScreenGui

-- Borda responsiva
local Border = Instance.new("UIStroke")
Border.Color = Color3.fromRGB(60, 60, 70)
Border.Thickness = isMobile and 2.5 or 2
Border.Parent = MainFrame

-- Cantos arredondados
local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, isMobile and 16 or 14)
Corner.Parent = MainFrame

print("✅ Frame principal criado: " .. settings.width .. "x" .. settings.height)

-- ============================================
-- BARRA DE TÍTULO OTIMIZADA
-- ============================================

local TitleBar = Instance.new("Frame")
TitleBar.Name = "TitleBar"
TitleBar.Size = UDim2.new(1, 0, 0, isMobile and 52 or 48)
TitleBar.BackgroundColor3 = Color3.fromRGB(28, 28, 34)
TitleBar.BorderSizePixel = 0
TitleBar.Active = true
TitleBar.Selectable = true
TitleBar.Parent = MainFrame

-- Ícone do dispositivo
local DeviceIcon = Instance.new("TextLabel")
DeviceIcon.Size = UDim2.new(0, isMobile and 45 or 40, 1, 0)
DeviceIcon.Position = UDim2.new(0, 10, 0, 0)
DeviceIcon.BackgroundTransparency = 1
DeviceIcon.Text = isMobile and "📱" or "💻"
DeviceIcon.TextSize = isMobile and 26 or 24
DeviceIcon.TextColor3 = Color3.new(1, 1, 1)
DeviceIcon.Font = Enum.Font.GothamBold
DeviceIcon.TextXAlignment = Enum.TextXAlignment.Center
DeviceIcon.Parent = TitleBar

-- Título
local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(1, -120, 1, 0)
TitleLabel.Position = UDim2.new(0, isMobile and 60 or 55, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "🍎 APPLE HUB"
TitleLabel.TextSize = settings.titleSize
TitleLabel.TextColor3 = Color3.new(1, 1, 1)
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
TitleLabel.Parent = TitleBar

-- Botões de controle otimizados
local buttonCtrlSize = isMobile and 38 or 34
local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Size = UDim2.new(0, buttonCtrlSize, 0, buttonCtrlSize)
MinimizeButton.Position = UDim2.new(1, isMobile and -82 or -78, 0.5, -buttonCtrlSize/2)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(255, 193, 7)
MinimizeButton.Text = "─"
MinimizeButton.TextColor3 = Color3.new(0, 0, 0)
MinimizeButton.TextSize = isMobile and 18 or 16
MinimizeButton.Font = Enum.Font.GothamBold
MinimizeButton.Parent = TitleBar

local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, buttonCtrlSize, 0, buttonCtrlSize)
CloseButton.Position = UDim2.new(1, isMobile and -39 or -36, 0.5, -buttonCtrlSize/2)
CloseButton.BackgroundColor3 = Color3.fromRGB(220, 60, 60)
CloseButton.Text = "✕"
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.TextSize = isMobile and 18 or 16
CloseButton.Font = Enum.Font.GothamBold
CloseButton.Parent = TitleBar

-- Arredondar botões
local buttonCorner = Instance.new("UICorner")
buttonCorner.CornerRadius = UDim.new(0, isMobile and 10 or 8)
buttonCorner.Parent = MinimizeButton
buttonCorner:Clone().Parent = CloseButton

print("✅ Barra de título otimizada")

-- ============================================
-- ÁREA DE SCRIPTS RESPONSIVA
-- ============================================

local ScriptsFrame = Instance.new("ScrollingFrame")
ScriptsFrame.Name = "ScriptsFrame"
ScriptsFrame.Size = UDim2.new(1, -12, 1, -(isMobile and 75 or 70))
ScriptsFrame.Position = UDim2.new(0, 6, 0, isMobile and 60 or 56)
ScriptsFrame.BackgroundTransparency = 1
ScriptsFrame.ScrollBarThickness = settings.scrollThickness
ScriptsFrame.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 110)
ScriptsFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
ScriptsFrame.ScrollingEnabled = true
ScriptsFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
ScriptsFrame.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Padding = UDim.new(0, settings.buttonSpacing)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
UIListLayout.Parent = ScriptsFrame

UIListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    ScriptsFrame.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 10)
end)

print("✅ Área de scripts responsiva criada")

-- ============================================
-- LISTA DE SCRIPTS COMPLETA
-- ============================================

local scripts = {
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
     url = "https://pastefy.app/JJVhs3rK/raw", badge = "PREMIUM", premium = true}
}

-- ============================================
-- FUNÇÃO PARA CRIAR BOTÕES RESPONSIVOS
-- ============================================

local function createResponsiveButton(scriptData, index)
    local button = Instance.new("TextButton")
    button.Name = "Btn_" .. index
    button.Size = UDim2.new(0.94, 0, 0, settings.buttonHeight)
    button.BackgroundColor3 = scriptData.premium and Color3.fromRGB(45, 35, 55) or Color3.fromRGB(38, 38, 44)
    button.BorderSizePixel = 0
    button.Text = ""
    button.AutoButtonColor = false
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, isMobile and 12 or 10)
    corner.Parent = button
    
    -- Borda premium
    if scriptData.premium then
        local premiumBorder = Instance.new("UIStroke")
        premiumBorder.Color = Color3.fromRGB(255, 215, 0)
        premiumBorder.Thickness = isMobile and 2 or 1.5
        premiumBorder.Parent = button
    end
    
    -- Ícone
    local iconSize = settings.iconSize
    local icon = Instance.new("TextLabel")
    icon.Size = UDim2.new(0, iconSize, 0, iconSize)
    icon.Position = UDim2.new(0, 12, 0.5, -iconSize/2)
    icon.BackgroundTransparency = 1
    icon.Text = scriptData.icon
    icon.TextSize = iconSize - (isMobile and 6 or 8)
    icon.TextColor3 = scriptData.color
    icon.Font = Enum.Font.GothamBold
    icon.Parent = button
    
    -- Nome do script
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -(iconSize + 20), 0.7, 0)
    label.Position = UDim2.new(0, iconSize + 15, 0, 6)
    label.BackgroundTransparency = 1
    label.Text = scriptData.name
    label.TextSize = settings.textSize
    label.TextColor3 = Color3.new(1, 1, 1)
    label.Font = Enum.Font.GothamBold
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.TextTruncate = Enum.TextTruncate.AtEnd
    label.Parent = button
    
    -- Badge responsivo
    if scriptData.badge then
        local badgeColor
        local badgeText = scriptData.badge
        
        if scriptData.badge == "PREMIUM" then
            badgeColor = Color3.fromRGB(255, 215, 0)
        elseif scriptData.badge == "KEY" then
            badgeColor = Color3.fromRGB(255, 152, 0)
        elseif scriptData.badge == "NEW" then
            badgeColor = Color3.fromRGB(0, 180, 216)
        else
            badgeColor = Color3.fromRGB(100, 100, 100)
        end
        
        -- Tamanho do badge baseado no texto
        local badgeWidth
        if badgeText == "PREMIUM" then
            badgeWidth = settings.badgeSize + 15
        elseif badgeText == "NEW" then
            badgeWidth = settings.badgeSize + 8
        else
            badgeWidth = settings.badgeSize
        end
        
        local badge = Instance.new("Frame")
        badge.Size = UDim2.new(0, badgeWidth, 0, isMobile and 22 or 20)
        badge.Position = UDim2.new(1, isMobile and -badgeWidth-8 or -badgeWidth-6, 0.1, 0)
        badge.BackgroundColor3 = badgeColor
        badge.BorderSizePixel = 0
        
        local badgeCorner = Instance.new("UICorner")
        badgeCorner.CornerRadius = UDim.new(0, isMobile and 6 or 4)
        badgeCorner.Parent = badge
        
        local badgeLabel = Instance.new("TextLabel")
        badgeLabel.Size = UDim2.new(1, 0, 1, 0)
        badgeLabel.BackgroundTransparency = 1
        badgeLabel.Text = badgeText
        badgeLabel.TextColor3 = badgeText == "PREMIUM" and Color3.new(0, 0, 0) or Color3.new(1, 1, 1)
        badgeLabel.TextSize = settings.badgeText
        badgeLabel.Font = Enum.Font.GothamBold
        badgeLabel.Parent = badge
        
        badge.Parent = button
    end
    
    return button, icon, label
end

-- ============================================
-- FUNÇÃO DE NOTIFICAÇÃO RESPONSIVA
-- ============================================

local function showResponsiveNotification(message, color)
    local notification = Instance.new("TextLabel")
    notification.Size = UDim2.new(1, -20, 0, settings.notificationHeight)
    notification.Position = UDim2.new(0, 10, 0, -50)
    notification.BackgroundColor3 = color
    notification.TextColor3 = Color3.new(1, 1, 1)
    notification.Text = message
    notification.TextSize = isMobile and 15 or 14
    notification.Font = Enum.Font.GothamBold
    notification.TextXAlignment = Enum.TextXAlignment.Center
    
    local notifCorner = Instance.new("UICorner")
    notifCorner.CornerRadius = UDim.new(0, isMobile and 10 or 8)
    notifCorner.Parent = notification
    
    notification.Parent = MainFrame
    
    -- Animação de entrada
    game:GetService("TweenService"):Create(notification, TweenInfo.new(0.3), {Position = UDim2.new(0, 10, 0, 15)}):Play()
    task.wait(2.5)
    
    -- Animação de saída
    game:GetService("TweenService"):Create(notification, TweenInfo.new(0.3), {Position = UDim2.new(0, 10, 0, -50)}):Play()
    task.wait(0.3)
    notification:Destroy()
end

-- ============================================
-- CRIAR BOTÕES COM INTERAÇÃO RESPONSIVA
-- ============================================

local buttons = {}
for i, scriptData in ipairs(scripts) do
    local button, icon, label = createResponsiveButton(scriptData, i)
    button.LayoutOrder = i
    
    -- 📱 SISTEMA DE INTERAÇÃO PARA MOBILE
    if isMobile then
        local touchStartTime = 0
        local touchStartPos = Vector2.new(0, 0)
        
        button.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch then
                touchStartTime = tick()
                touchStartPos = input.Position
                button.BackgroundColor3 = scriptData.premium and Color3.fromRGB(55, 45, 65) or Color3.fromRGB(48, 48, 54)
            end
        end)
        
        button.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch then
                local touchDuration = tick() - touchStartTime
                local touchDistance = (input.Position - touchStartPos).Magnitude
                
                -- Verificar se foi um toque simples (não arrasto)
                if touchDuration < 0.5 and touchDistance < 10 then
                    button.MouseButton1Click:Fire()
                end
                
                button.BackgroundColor3 = scriptData.premium and Color3.fromRGB(45, 35, 55) or Color3.fromRGB(38, 38, 44)
            end
        end)
    else
        -- 💻 HOVER EFFECTS PARA PC
        button.MouseEnter:Connect(function()
            button.BackgroundColor3 = scriptData.premium and Color3.fromRGB(55, 45, 65) or Color3.fromRGB(48, 48, 54)
        end)
        
        button.MouseLeave:Connect(function()
            button.BackgroundColor3 = scriptData.premium and Color3.fromRGB(45, 35, 55) or Color3.fromRGB(38, 38, 44)
        end)
    end
    
    -- Função de execução universal
    local function executeScript()
        local originalColor = button.BackgroundColor3
        local originalIcon = icon.Text
        local originalText = label.Text
        
        -- Animação de clique
        button.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
        icon.Text = "⏳"
        label.Text = "Carregando..."
        
        task.wait(0.2)
        
        -- Configurações especiais
        if scriptData.special then
            getgenv().ServerDestroyerV6 = {Comprar = false, Spam = true}
        end
        
        -- Executar script
        local success, err = pcall(function()
            local scriptContent = game:HttpGet(scriptData.url, true)
            loadstring(scriptContent)()
        end)
        
        -- Feedback visual
        if success then
            button.BackgroundColor3 = Color3.fromRGB(60, 180, 80)
            icon.Text = "✅"
            label.Text = "Sucesso!"
            print("✅ " .. scriptData.name .. " executado!")
            
            -- Notificações especiais
            if scriptData.name == "Miranda Hub" then
                task.wait(0.5)
                showResponsiveNotification("⚡ MIRANDA HUB ATIVADO!", Color3.fromRGB(0, 200, 255))
            elseif scriptData.name == "LKZ HELPER [OP]" then
                task.wait(0.5)
                showResponsiveNotification("🔧 LKZ HELPER CARREGADO!", Color3.fromRGB(0, 180, 216))
            end
        else
            button.BackgroundColor3 = Color3.fromRGB(180, 60, 60)
            icon.Text = "❌"
            label.Text = "Erro!"
            print("❌ Erro em " .. scriptData.name .. ": " .. tostring(err))
        end
        
        task.wait(1.5)
        button.BackgroundColor3 = originalColor
        icon.Text = originalIcon
        label.Text = originalText
    end
    
    -- Conectar clique
    button.MouseButton1Click:Connect(executeScript)
    
    button.Parent = ScriptsFrame
    buttons[scriptData.name] = button
end

print("✅ " .. #scripts .. " botões criados otimizados")

-- ============================================
-- SISTEMA DE DRAG RESPONSIVO
-- ============================================

local dragging = false
local dragStart, frameStart

local function startDrag(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or 
       (isMobile and input.UserInputType == Enum.UserInputType.Touch) then
        dragging = true
        dragStart = input.Position
        frameStart = MainFrame.Position
        TitleBar.BackgroundColor3 = Color3.fromRGB(38, 38, 44)
        
        if isMobile then
            game:GetService("UserInputService"):SelectGui(TitleBar)
        end
    end
end

local function updateDrag(input)
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
end

local function stopDrag()
    dragging = false
    TitleBar.BackgroundColor3 = Color3.fromRGB(28, 28, 34)
end

-- Conectar eventos de drag
TitleBar.InputBegan:Connect(startDrag)
TitleBar.InputChanged:Connect(function(input)
    if dragging then
        updateDrag(input)
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseButton1 or 
       input.UserInputType == Enum.UserInputType.Touch) then
        stopDrag()
    end
end)

print("✅ Sistema de drag otimizado para: " .. (isMobile and "Mobile" or "PC"))

-- ============================================
-- CONTROLES RESPONSIVOS
-- ============================================

local minimized = false

local function toggleMinimize()
    minimized = not minimized
    
    if minimized then
        game:GetService("TweenService"):Create(MainFrame, TweenInfo.new(0.2), {
            Size = UDim2.new(0, isMobile and 160 or 140, 0, isMobile and 52 or 48)
        }):Play()
        ScriptsFrame.Visible = false
        MinimizeButton.Text = "+"
    else
        game:GetService("TweenService"):Create(MainFrame, TweenInfo.new(0.2), {
            Size = UDim2.new(0, settings.width, 0, settings.height)
        }):Play()
        ScriptsFrame.Visible = true
        MinimizeButton.Text = "─"
    end
end

MinimizeButton.MouseButton1Click:Connect(toggleMinimize)

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
    print("🔒 Apple Hub fechado")
end)

-- ============================================
-- ANIMAÇÃO DE ENTRADA
-- ============================================

MainFrame.BackgroundTransparency = 1
TitleBar.BackgroundTransparency = 1

task.wait(0.1)

for i = 1, 10 do
    MainFrame.BackgroundTransparency = 1 - (i/10)
    TitleBar.BackgroundTransparency = 1 - (i/10)
    task.wait(0.02)
end

-- ============================================
-- NOTIFICAÇÃO INICIAL
-- ============================================

task.wait(0.5)
showResponsiveNotification("🍎 APPLE HUB " .. (isMobile and "📱" or "💻") .. " CARREGADO!", Color3.fromRGB(0, 150, 255))

-- ============================================
-- RODAPÉ INFORMATIVO RESPONSIVO
-- ============================================

local footer = Instance.new("TextLabel")
footer.Size = UDim2.new(1, -20, 0, isMobile and 30 or 26)
footer.Position = UDim2.new(0, 10, 1, -35)
footer.BackgroundColor3 = Color3.fromRGB(40, 40, 46)
footer.TextColor3 = Color3.fromRGB(180, 180, 180)
footer.Text = (isMobile and "📱" or "💻") .. " " .. (isMobile and "Toque nos botões" or "Clique nos botões") .. 
             " | 🎯 " .. #scripts .. " scripts | 🆕 LKZ Helper"
footer.TextSize = isMobile and 12 or 11
footer.Font = Enum.Font.Gotham
footer.TextXAlignment = Enum.TextXAlignment.Center
footer.TextWrapped = true

local footerCorner = Instance.new("UICorner")
footerCorner.CornerRadius = UDim.new(0, isMobile and 8 or 6)
footerCorner.Parent = footer

footer.Parent = MainFrame

-- ============================================
-- SISTEMA DE ORIENTAÇÃO MOBILE
-- ============================================

if isMobile then
    local function adjustForOrientation()
        task.wait(0.2)
        local viewport = workspace.CurrentCamera.ViewportSize
        
        -- Ajustar baseado na orientação
        local newWidth, newHeight
        if viewport.Y > viewport.X then
            -- Portrait (vertical)
            newWidth = math.min(360, viewport.X * 0.92)
            newHeight = 580
        else
            -- Landscape (horizontal)
            newWidth = math.min(420, viewport.X * 0.8)
            newHeight = 500
        end
        
        -- Aplicar novos tamanhos
        settings.width = newWidth
        settings.height = newHeight
        
        if not minimized then
            MainFrame.Size = UDim2.new(0, newWidth, 0, newHeight)
        end
        
        print("📱 Tela ajustada para: " .. newWidth .. "x" .. newHeight)
    end
    
    -- Ajustar inicialmente
    adjustForOrientation()
    
    -- Monitorar mudanças de tela
    workspace.CurrentCamera:GetPropertyChangedSignal("ViewportSize"):Connect(adjustForOrientation)
end

-- ============================================
-- ATUALIZAÇÃO DO SCROLL AUTOMÁTICO
-- ============================================

task.spawn(function()
    while ScreenGui.Parent do
        ScriptsFrame.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 20)
        task.wait(0.5)
    end
end)

-- ============================================
-- LOG FINAL
-- ============================================

print("════════════════════════════════════")
print("🎮 APPLE HUB - VERSÃO RESPONSIVA")
print("📱 Plataforma: " .. (isMobile and "MOBILE" or "PC"))
print("📏 Tamanho: " .. settings.width .. "x" .. settings.height)
print("👤 Jogador: " .. player.Name)
print("🎯 Scripts: " .. #scripts .. " disponíveis")
print("🆕 Destaque: LKZ HELPER [OP]")
print("🖱️  Drag: " .. (isMobile and "Toque e arraste" or "Clique e arraste"))
print("════════════════════════════════════")
print("✨ HUB 100% OTIMIZADO PARA " .. (isMobile and "MOBILE 📱" or "PC 💻"))
