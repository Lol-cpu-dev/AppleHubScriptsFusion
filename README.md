-- 🍎 APPLE HUB - VERSÃO 100% FUNCIONAL
-- ⚡ Copie e cole TODO este código no executor

-- ============================================
-- CONFIGURAÇÃO INICIAL
-- ============================================

-- Remover hubs antigos
for _, gui in pairs(game:GetService("CoreGui"):GetChildren()) do
    if gui.Name:find("Apple") or gui.Name:find("Hub") then
        gui:Destroy()
    end
end

print("🚀 INICIANDO APPLE HUB...")

-- Serviços
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer
local isMobile = UserInputService.TouchEnabled

-- ============================================
-- CRIAÇÃO DA GUI
-- ============================================

-- ScreenGui principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AppleHubFinal"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game:GetService("CoreGui")

print("✅ GUI criada")

-- Frame principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 380, 0, 550)
MainFrame.Position = UDim2.new(0.5, -190, 0.5, -275)
MainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 24)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true
MainFrame.Parent = ScreenGui

-- Borda
local Border = Instance.new("UIStroke")
Border.Color = Color3.fromRGB(60, 60, 70)
Border.Thickness = 2
Border.Parent = MainFrame

-- Cantos arredondados
local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, 12)
Corner.Parent = MainFrame

print("✅ Frame principal criado")

-- ============================================
-- BARRA DE TÍTULO COM DRAG
-- ============================================

local TitleBar = Instance.new("Frame")
TitleBar.Name = "TitleBar"
TitleBar.Size = UDim2.new(1, 0, 0, 45)
TitleBar.BackgroundColor3 = Color3.fromRGB(28, 28, 34)
TitleBar.BorderSizePixel = 0
TitleBar.Parent = MainFrame

-- Título
local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(0.7, 0, 1, 0)
TitleLabel.Position = UDim2.new(0.15, 0, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "🍎 APPLE HUB PRO"
TitleLabel.TextColor3 = Color3.new(1, 1, 1)
TitleLabel.TextSize = 18
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextXAlignment = Enum.TextXAlignment.Center
TitleLabel.Parent = TitleBar

-- Botão minimizar
local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Size = UDim2.new(0, 30, 0, 30)
MinimizeButton.Position = UDim2.new(1, -70, 0.5, -15)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(255, 193, 7)
MinimizeButton.Text = "─"
MinimizeButton.TextColor3 = Color3.new(0, 0, 0)
MinimizeButton.TextSize = 16
MinimizeButton.Font = Enum.Font.GothamBold
MinimizeButton.Parent = TitleBar

-- Botão fechar
local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -35, 0.5, -15)
CloseButton.BackgroundColor3 = Color3.fromRGB(220, 60, 60)
CloseButton.Text = "✕"
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.TextSize = 16
CloseButton.Font = Enum.Font.GothamBold
CloseButton.Parent = TitleBar

-- Arredondar botões
local buttonCorner = Instance.new("UICorner")
buttonCorner.CornerRadius = UDim.new(0, 8)
buttonCorner.Parent = MinimizeButton
buttonCorner:Clone().Parent = CloseButton

print("✅ Barra de título criada")

-- ============================================
-- ÁREA DE SCRIPTS COM SCROLL
-- ============================================

local ScriptsFrame = Instance.new("ScrollingFrame")
ScriptsFrame.Name = "ScriptsFrame"
ScriptsFrame.Size = UDim2.new(1, -10, 1, -65)
ScriptsFrame.Position = UDim2.new(0, 5, 0, 55)
ScriptsFrame.BackgroundTransparency = 1
ScriptsFrame.ScrollBarThickness = 4
ScriptsFrame.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 110)
ScriptsFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
ScriptsFrame.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Padding = UDim.new(0, 10)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
UIListLayout.Parent = ScriptsFrame

print("✅ Área de scripts criada")

-- ============================================
-- LISTA DE SCRIPTS COMPLETA
-- ============================================

local scripts = {
    {
        name = "Nameless Hub",
        icon = "👑",
        color = Color3.fromRGB(255, 215, 0),
        url = "https://raw.githubusercontent.com/ily123950/Vulkan/main/Tr"
    },
    {
        name = "Chilli Hub",
        icon = "🌶️",
        color = Color3.fromRGB(255, 69, 58),
        url = "https://raw.githubusercontent.com/tienkhanh1/spicy/main/Chilli.lua"
    },
    {
        name = "UCT Hub",
        icon = "⚡",
        color = Color3.fromRGB(0, 191, 255),
        url = "https://raw.githubusercontent.com/UCT-hub/main/main/stealabrainrot"
    },
    {
        name = "Kurd Hub",
        icon = "🏔️",
        color = Color3.fromRGB(34, 139, 34),
        url = "https://raw.githubusercontent.com/Ninja10908/S4/main/Kurdhub"
    },
    {
        name = "Lag + Aura",
        icon = "💀",
        color = Color3.fromRGB(178, 34, 34),
        url = "https://tcscripts.discloud.app/scripts/serverdestroyerv6",
        special = true
    },
    {
        name = "Admin Spam",
        icon = "👑",
        color = Color3.fromRGB(255, 215, 0),
        url = "https://api.luarmor.net/files/v3/loaders/fc9523e876bada3b7ed4ebe004cb8cf9.lua"
    },
    {
        name = "Chilli Private",
        icon = "🔓",
        color = Color3.fromRGB(0, 200, 83),
        url = "https://raw.githubusercontent.com/tienkhanh1/spicy/main/PrivateServer"
    },
    {
        name = "Speed Steal",
        icon = "⚡",
        color = Color3.fromRGB(0, 150, 255),
        url = "https://pastebin.com/raw/rmxfZDPd"
    },
    {
        name = "Auto Ping Pong",
        icon = "🏓",
        color = Color3.fromRGB(0, 230, 118),
        url = "https://raw.githubusercontent.com/LucasggkX/Games/main/Auto%20buy%20-%20Ping%20Pong.lua",
        badge = "KEY"
    },
    {
        name = "Paintball Script",
        icon = "🎯",
        color = Color3.fromRGB(255, 87, 34),
        url = "https://raw.githubusercontent.com/LucasggkX/Games/main/Paintball.lua",
        badge = "KEY"
    },
    {
        name = "Lennon Hub",
        icon = "🎵",
        color = Color3.fromRGB(156, 39, 176),
        url = "https://raw.githubusercontent.com/mxrtnttzflw/martinetti-scripts/main/script.lua"
    },
    {
        name = "Miranda Hub",
        icon = "⚡",
        color = Color3.fromRGB(0, 200, 255),
        url = "https://pastefy.app/JJVhs3rK/raw",
        badge = "PREMIUM",
        premium = true
    }
}

-- ============================================
-- SISTEMA DE AUTORIZAÇÃO PREMIUM
-- ============================================

local AUTHORIZED_PLAYERS = {
    1234567890,  -- Seu ID aqui
    9876543210,  -- Amigo 1
    5555555555,  -- Amigo 2
}

local function isPlayerAuthorized()
    local playerId = player.UserId
    for _, id in ipairs(AUTHORIZED_PLAYERS) do
        if playerId == id then
            return true
        end
    end
    return false
end

-- ============================================
-- FUNÇÃO PARA CRIAR BOTÕES
-- ============================================

local function createScriptButton(scriptData, index)
    local button = Instance.new("TextButton")
    button.Name = "Btn_" .. index
    button.Size = UDim2.new(0.95, 0, 0, 55)
    button.BackgroundColor3 = scriptData.premium and Color3.fromRGB(45, 35, 55) or Color3.fromRGB(38, 38, 44)
    button.BorderSizePixel = 0
    button.Text = ""
    button.AutoButtonColor = false
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 10)
    corner.Parent = button
    
    -- Borda para premium
    if scriptData.premium then
        local premiumBorder = Instance.new("UIStroke")
        premiumBorder.Color = Color3.fromRGB(255, 215, 0)
        premiumBorder.Thickness = 1.5
        premiumBorder.Parent = button
    end
    
    -- Ícone
    local icon = Instance.new("TextLabel")
    icon.Size = UDim2.new(0, 35, 0, 35)
    icon.Position = UDim2.new(0, 10, 0.5, -17.5)
    icon.BackgroundTransparency = 1
    icon.Text = scriptData.icon
    icon.TextSize = 22
    icon.TextColor3 = scriptData.color
    icon.Font = Enum.Font.GothamBold
    icon.Parent = button
    
    -- Nome
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -50, 0.7, 0)
    label.Position = UDim2.new(0, 50, 0, 5)
    label.BackgroundTransparency = 1
    label.Text = scriptData.name
    label.TextSize = 14
    label.TextColor3 = Color3.new(1, 1, 1)
    label.Font = Enum.Font.GothamBold
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = button
    
    -- Badge
    if scriptData.badge then
        local badge = Instance.new("Frame")
        badge.Size = UDim2.new(0, scriptData.badge == "PREMIUM" and 50 or 35, 0, 18)
        badge.Position = UDim2.new(1, -55, 0.1, 0)
        badge.BackgroundColor3 = scriptData.badge == "PREMIUM" and Color3.fromRGB(255, 215, 0) or Color3.fromRGB(255, 152, 0)
        badge.BorderSizePixel = 0
        
        local badgeCorner = Instance.new("UICorner")
        badgeCorner.CornerRadius = UDim.new(0, 4)
        badgeCorner.Parent = badge
        
        local badgeText = Instance.new("TextLabel")
        badgeText.Size = UDim2.new(1, 0, 1, 0)
        badgeText.BackgroundTransparency = 1
        badgeText.Text = scriptData.badge
        badgeText.TextColor3 = scriptData.badge == "PREMIUM" and Color3.new(0, 0, 0) or Color3.new(1, 1, 1)
        badgeText.TextSize = 9
        badgeText.Font = Enum.Font.GothamBold
        badgeText.Parent = badge
        
        badge.Parent = button
    end
    
    -- Função de execução
    local function executeScript()
        -- Verificar se é premium e se tem acesso
        if scriptData.premium and not isPlayerAuthorized() then
            -- Mostrar notificação de acesso negado
            local notif = Instance.new("TextLabel")
            notif.Size = UDim2.new(1, -20, 0, 40)
            notif.Position = UDim2.new(0, 10, 0, -50)
            notif.BackgroundColor3 = Color3.fromRGB(220, 60, 60)
            notif.TextColor3 = Color3.new(1, 1, 1)
            notif.Text = "❌ Você não tem autorização ao premium."
            notif.TextSize = 14
            notif.Font = Enum.Font.GothamBold
            notif.TextXAlignment = Enum.TextXAlignment.Center
            
            local notifCorner = Instance.new("UICorner")
            notifCorner.CornerRadius = UDim.new(0, 8)
            notifCorner.Parent = notif
            
            notif.Parent = MainFrame
            
            -- Animação
            notif:TweenPosition(UDim2.new(0, 10, 0, 15), "Out", "Quad", 0.3)
            task.wait(2)
            notif:TweenPosition(UDim2.new(0, 10, 0, -50), "Out", "Quad", 0.3)
            task.wait(0.3)
            notif:Destroy()
            
            print("❌ Acesso premium negado para: " .. player.Name)
            return
        end
        
        -- Executar script
        local originalColor = button.BackgroundColor3
        local originalIcon = icon.Text
        local originalText = label.Text
        
        -- Animação de clique
        button.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
        icon.Text = "⏳"
        label.Text = "Carregando..."
        
        task.wait(0.2)
        
        -- Configurar variáveis especiais
        if scriptData.special then
            getgenv().ServerDestroyerV6 = {Comprar = false, Spam = true}
        end
        
        -- Executar
        local success, err = pcall(function()
            loadstring(game:HttpGet(scriptData.url))()
        end)
        
        -- Feedback
        if success then
            button.BackgroundColor3 = Color3.fromRGB(60, 180, 80)
            icon.Text = "✅"
            label.Text = "Sucesso!"
            print("✅ " .. scriptData.name .. " executado!")
            
            -- Notificação especial para Miranda Hub
            if scriptData.name == "Miranda Hub" then
                task.wait(0.5)
                local specialNotif = Instance.new("TextLabel")
                specialNotif.Size = UDim2.new(1, -20, 0, 35)
                specialNotif.Position = UDim2.new(0, 10, 0, -40)
                specialNotif.BackgroundColor3 = Color3.fromRGB(0, 200, 255)
                specialNotif.TextColor3 = Color3.new(1, 1, 1)
                specialNotif.Text = "⚡ MIRANDA HUB ATIVADO!"
                specialNotif.TextSize = 13
                specialNotif.Font = Enum.Font.GothamBold
                specialNotif.TextXAlignment = Enum.TextXAlignment.Center
                
                local specialCorner = Instance.new("UICorner")
                specialCorner.CornerRadius = UDim.new(0, 8)
                specialCorner.Parent = specialNotif
                
                specialNotif.Parent = MainFrame
                
                specialNotif:TweenPosition(UDim2.new(0, 10, 0, 15), "Out", "Quad", 0.3)
                task.wait(2.5)
                specialNotif:TweenPosition(UDim2.new(0, 10, 0, -40), "Out", "Quad", 0.3)
                task.wait(0.3)
                specialNotif:Destroy()
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
    
    -- Efeito hover (PC)
    if not isMobile then
        button.MouseEnter:Connect(function()
            button.BackgroundColor3 = scriptData.premium and Color3.fromRGB(55, 45, 65) or Color3.fromRGB(48, 48, 54)
        end)
        
        button.MouseLeave:Connect(function()
            button.BackgroundColor3 = scriptData.premium and Color3.fromRGB(45, 35, 55) or Color3.fromRGB(38, 38, 44)
        end)
    end
    
    button.Parent = ScriptsFrame
    return button
end

-- ============================================
-- CRIAR TODOS OS BOTÕES
-- ============================================

for i, scriptData in ipairs(scripts) do
    createScriptButton(scriptData, i)
end

print("✅ " .. #scripts .. " scripts criados")

-- ============================================
-- SISTEMA DE DRAG FUNCIONAL
-- ============================================

local dragging = false
local dragStart, frameStart

TitleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or 
       (isMobile and input.UserInputType == Enum.UserInputType.Touch) then
        dragging = true
        dragStart = input.Position
        frameStart = MainFrame.Position
        TitleBar.BackgroundColor3 = Color3.fromRGB(38, 38, 44)
    end
end)

TitleBar.InputChanged:Connect(function(input)
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

TitleBar.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or 
       input.UserInputType == Enum.UserInputType.Touch then
        dragging = false
        TitleBar.BackgroundColor3 = Color3.fromRGB(28, 28, 34)
    end
end)

print("✅ Sistema de drag configurado")

-- ============================================
-- FUNÇÕES DOS BOTÕES
-- ============================================

local minimized = false

MinimizeButton.MouseButton1Click:Connect(function()
    minimized = not minimized
    
    if minimized then
        MainFrame:TweenSize(UDim2.new(0, 150, 0, 45), "Out", "Quad", 0.2, true)
        ScriptsFrame.Visible = false
        MinimizeButton.Text = "+"
    else
        MainFrame:TweenSize(UDim2.new(0, 380, 0, 550), "Out", "Quad", 0.2, true)
        ScriptsFrame.Visible = true
        MinimizeButton.Text = "─"
    end
end)

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

local notification = Instance.new("TextLabel")
notification.Size = UDim2.new(1, -20, 0, 40)
notification.Position = UDim2.new(0, 10, 0, -50)
notification.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
notification.TextColor3 = Color3.new(1, 1, 1)
notification.Text = "🍎 APPLE HUB PRO - CARREGADO!"
notification.TextSize = 14
notification.Font = Enum.Font.GothamBold
notification.TextXAlignment = Enum.TextXAlignment.Center

local notifCorner = Instance.new("UICorner")
notifCorner.CornerRadius = UDim.new(0, 8)
notifCorner.Parent = notification

notification.Parent = MainFrame

-- Animação
notification:TweenPosition(UDim2.new(0, 10, 0, 15), "Out", "Quad", 0.3)
task.wait(2)
notification:TweenPosition(UDim2.new(0, 10, 0, -50), "Out", "Quad", 0.3)
task.wait(0.3)
notification:Destroy()

-- ============================================
-- AVISO PREMIUM
-- ============================================

local premiumWarning = Instance.new("TextLabel")
premiumWarning.Size = UDim2.new(1, -20, 0, 25)
premiumWarning.Position = UDim2.new(0, 10, 1, -30)
premiumWarning.BackgroundColor3 = Color3.fromRGB(255, 152, 0)
premiumWarning.TextColor3 = Color3.new(1, 1, 1)
premiumWarning.Text = isPlayerAuthorized() and "💎 PREMIUM ATIVADO" or "🔐 SCRIPTS COM KEY PRECISAM DE CHAVE"
premiumWarning.TextSize = 11
premiumWarning.Font = Enum.Font.Gotham
premiumWarning.TextXAlignment = Enum.TextXAlignment.Center
premiumWarning.TextWrapped = true

local warningCorner = Instance.new("UICorner")
warningCorner.CornerRadius = UDim.new(0, 6)
warningCorner.Parent = premiumWarning

premiumWarning.Parent = MainFrame

-- ============================================
-- LOG FINAL
-- ============================================

print("════════════════════════════════")
print("🎮 APPLE HUB PRO - PRONTO!")
print("📱 Dispositivo: " .. (isMobile and "Mobile" or "PC"))
print("👤 Jogador: " .. player.Name)
print("🆔 ID: " .. player.UserId)
print("🔐 Premium: " .. (isPlayerAuthorized() and "✅ AUTORIZADO" or "❌ NÃO AUTORIZADO"))
print("🎯 Scripts: " .. #scripts .. " disponíveis")
print("🖱️  Drag: Ativo (barra superior)")
print("════════════════════════════════")

print("✨ HUB COMPLETAMENTE FUNCIONAL!")
