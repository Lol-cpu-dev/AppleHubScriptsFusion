-- 🍎 Apple Hub - Versão Consertada
-- 🚀 Estável e funcional para Mobile/PC

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

-- Detectar dispositivo
local player = Players.LocalPlayer
local isMobile = UserInputService.TouchEnabled
local screenSize = workspace.CurrentCamera.ViewportSize

-- Configurações básicas
local frameWidth = isMobile and 350 or 400
local frameHeight = 550  -- Aumentado para caber mais scripts
local startPosition = UDim2.new(0.5, -frameWidth/2, 0.5, -frameHeight/2)

-- Criar GUI simples
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AppleHubFixed"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false

-- Colocar na interface
if gethui then
    ScreenGui.Parent = gethui()
elseif syn and syn.protect_gui then
    syn.protect_gui(ScreenGui)
    ScreenGui.Parent = game.CoreGui
else
    ScreenGui.Parent = player:WaitForChild("PlayerGui")
end

-- Frame principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, frameWidth, 0, frameHeight)
MainFrame.Position = startPosition
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true

-- Borda e cantos
local Border = Instance.new("UIStroke")
Border.Color = Color3.fromRGB(60, 60, 70)
Border.Thickness = 2
Border.Parent = MainFrame

local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, 10)
Corner.Parent = MainFrame

-- Barra de título
local TitleBar = Instance.new("Frame")
TitleBar.Name = "TitleBar"
TitleBar.Size = UDim2.new(1, 0, 0, 40)
TitleBar.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
TitleBar.BorderSizePixel = 0

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(0.6, 0, 1, 0)
TitleLabel.Position = UDim2.new(0.2, 0, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "APPLE HUB"
TitleLabel.TextColor3 = Color3.new(1, 1, 1)
TitleLabel.TextSize = 18
TitleLabel.Font = Enum.Font.GothamBold

-- Botão minimizar
local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Size = UDim2.new(0, 30, 0, 30)
MinimizeButton.Position = UDim2.new(1, -70, 0.5, -15)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(255, 193, 7)
MinimizeButton.Text = "─"
MinimizeButton.TextColor3 = Color3.new(0, 0, 0)
MinimizeButton.TextSize = 16

-- Botão fechar
local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -35, 0.5, -15)
CloseButton.BackgroundColor3 = Color3.fromRGB(220, 60, 60)
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.TextSize = 16

-- Área de rolagem
local ScriptsFrame = Instance.new("ScrollingFrame")
ScriptsFrame.Name = "ScriptsFrame"
ScriptsFrame.Size = UDim2.new(1, -10, 1, -60)
ScriptsFrame.Position = UDim2.new(0, 5, 0, 50)
ScriptsFrame.BackgroundTransparency = 1
ScriptsFrame.ScrollBarThickness = 4
ScriptsFrame.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 110)

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Padding = UDim.new(0, 8)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder

-- Montar interface
MainFrame.Parent = ScreenGui
TitleBar.Parent = MainFrame
TitleLabel.Parent = TitleBar
MinimizeButton.Parent = TitleBar
CloseButton.Parent = TitleBar
ScriptsFrame.Parent = MainFrame
UIListLayout.Parent = ScriptsFrame

-- Lista de scripts (com Paintball adicionado)
local scripts = {
    {name = "Nameless Hub", icon = "👑", url = "https://raw.githubusercontent.com/ily123950/Vulkan/main/Tr"},
    {name = "Chilli Hub", icon = "🌶️", url = "https://raw.githubusercontent.com/tienkhanh1/spicy/main/Chilli.lua"},
    {name = "UCT Hub", icon = "⚡", url = "https://raw.githubusercontent.com/UCT-hub/main/main/stealabrainrot"},
    {name = "Kurd Hub", icon = "🏔️", url = "https://raw.githubusercontent.com/Ninja10908/S4/main/Kurdhub"},
    {name = "Lag + Aura", icon = "💀", url = "https://tcscripts.discloud.app/scripts/serverdestroyerv6"},
    {name = "Admin Spam", icon = "👑", url = "https://api.luarmor.net/files/v3/loaders/fc9523e876bada3b7ed4ebe004cb8cf9.lua"},
    {name = "Chilli Private", icon = "🔓", url = "https://raw.githubusercontent.com/tienkhanh1/spicy/main/PrivateServer"},
    {name = "Speed Steal", icon = "⚡", url = "https://pastebin.com/raw/rmxfZDPd"},
    {name = "Auto Ping Pong [KEY]", icon = "🏓", url = "https://raw.githubusercontent.com/LucasggkX/Games/main/Auto%20buy%20-%20Ping%20Pong.lua"},
    -- 🎯 NOVO SCRIPT ADICIONADO: PAINTBALL
    {name = "Paintball Script [KEY]", icon = "🎯", url = "https://raw.githubusercontent.com/LucasggkX/Games/main/Paintball.lua"}
}

-- Criar botões simples
for i, scriptData in ipairs(scripts) do
    local button = Instance.new("TextButton")
    button.Name = "ScriptButton_" .. i
    button.Size = UDim2.new(0.95, 0, 0, 50)
    button.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
    button.BorderSizePixel = 0
    button.Text = ""
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = button
    
    -- Ícone
    local icon = Instance.new("TextLabel")
    icon.Size = UDim2.new(0, 30, 0, 30)
    icon.Position = UDim2.new(0, 10, 0.5, -15)
    icon.BackgroundTransparency = 1
    icon.Text = scriptData.icon
    icon.TextSize = 20
    icon.TextColor3 = Color3.new(1, 1, 1)
    icon.Font = Enum.Font.GothamBold
    icon.Parent = button
    
    -- Nome
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -50, 1, 0)
    label.Position = UDim2.new(0, 45, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = scriptData.name
    label.TextSize = 14
    label.TextColor3 = Color3.new(1, 1, 1)
    label.Font = Enum.Font.Gotham
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = button
    
    -- Badge para scripts que precisam de key
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
    
    -- Função de execução
    button.MouseButton1Click:Connect(function()
        local originalColor = button.BackgroundColor3
        local originalText = label.Text
        
        -- Animação de clique
        button.BackgroundColor3 = Color3.fromRGB(60, 60, 70)
        label.Text = "Carregando..."
        
        task.wait(0.2)
        
        -- Configurar variáveis especiais se necessário
        if scriptData.name == "Lag + Aura" then
            getgenv().ServerDestroyerV6 = {Comprar = false, Spam = true}
        end
        
        -- Executar script
        local success, errorMsg = pcall(function()
            loadstring(game:HttpGet(scriptData.url))()
        end)
        
        -- Feedback
        if success then
            button.BackgroundColor3 = Color3.fromRGB(60, 180, 80)
            label.Text = "Executado!"
            
            -- Feedback especial para Paintball
            if scriptData.name == "Paintball Script" then
                local paintballNotif = Instance.new("TextLabel")
                paintballNotif.Size = UDim2.new(1, -20, 0, 25)
                paintballNotif.Position = UDim2.new(0, 10, 0, -35)
                paintballNotif.BackgroundColor3 = Color3.fromRGB(255, 87, 34)
                paintballNotif.TextColor3 = Color3.new(1, 1, 1)
                paintballNotif.Text = "🎯 Paintball ativado!"
                paintballNotif.TextSize = 12
                paintballNotif.Font = Enum.Font.GothamBold
                paintballNotif.TextXAlignment = Enum.TextXAlignment.Center
                
                local notifCorner = Instance.new("UICorner")
                notifCorner.CornerRadius = UDim.new(0, 6)
                notifCorner.Parent = paintballNotif
                
                paintballNotif.Parent = MainFrame
                
                paintballNotif:TweenPosition(UDim2.new(0, 10, 0, 15), "Out", "Quad", 0.3)
                task.wait(2)
                paintballNotif:TweenPosition(UDim2.new(0, 10, 0, -35), "Out", "Quad", 0.3)
                task.wait(0.3)
                paintballNotif:Destroy()
            end
        else
            button.BackgroundColor3 = Color3.fromRGB(180, 60, 60)
            label.Text = "Erro!"
            warn("Erro ao executar " .. scriptData.name .. ":", errorMsg)
        end
        
        task.wait(1)
        button.BackgroundColor3 = originalColor
        label.Text = originalText
    end)
    
    if isMobile then
        button.TouchTap:Connect(function()
            button.MouseButton1Click:Fire()
        end)
    end
    
    button.Parent = ScriptsFrame
end

-- Sistema de arrasto SIMPLES E FUNCIONAL
local dragging = false
local dragStart, frameStart

TitleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        frameStart = MainFrame.Position
        
        -- Feedback visual no mobile
        if isMobile then
            TitleBar.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
        end
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
        if isMobile then
            TitleBar.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
        end
    end
end)

-- Sistema de minimizar
local minimized = false

MinimizeButton.MouseButton1Click:Connect(function()
    minimized = not minimized
    
    if minimized then
        MainFrame:TweenSize(UDim2.new(0, 150, 0, 40), "Out", "Quad", 0.2, true)
        ScriptsFrame.Visible = false
        MinimizeButton.Text = "+"
    else
        MainFrame:TweenSize(UDim2.new(0, frameWidth, 0, frameHeight), "Out", "Quad", 0.2, true)
        ScriptsFrame.Visible = true
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

-- Efeito hover simples (apenas PC)
if not isMobile then
    for _, button in pairs(ScriptsFrame:GetChildren()) do
        if button:IsA("TextButton") then
            button.MouseEnter:Connect(function()
                button.BackgroundColor3 = Color3.fromRGB(55, 55, 60)
            end)
            
            button.MouseLeave:Connect(function()
                button.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
            end)
        end
    end
end

-- Animar entrada
MainFrame.BackgroundTransparency = 1
TitleBar.BackgroundTransparency = 1

task.wait(0.1)

for i = 1, 10 do
    MainFrame.BackgroundTransparency = 1 - (i/10)
    TitleBar.BackgroundTransparency = 1 - (i/10)
    task.wait(0.02)
end

-- Mensagem de sucesso
print("════════════════════════════════")
print("✅ Apple Hub carregado com sucesso!")
print("📱 Dispositivo: " .. (isMobile and "Mobile" or "PC"))
print("🎯 Scripts: " .. #scripts .. " disponíveis")
print("🆕 Novo: Paintball Script 🎯")
print("🖱️  Arraste pela barra superior")
print("════════════════════════════════")

-- Notificação visual de boas-vindas
task.wait(0.5)
local notification = Instance.new("TextLabel")
notification.Size = UDim2.new(1, -20, 0, 35)
notification.Position = UDim2.new(0, 10, 0, -45)
notification.BackgroundColor3 = Color3.fromRGB(0, 120, 215)
notification.TextColor3 = Color3.new(1, 1, 1)
notification.Text = "✅ Apple Hub v3.0 - Paintball Adicionado!"
notification.TextSize = 14
notification.Font = Enum.Font.GothamBold
notification.TextXAlignment = Enum.TextXAlignment.Center

local notifCorner = Instance.new("UICorner")
notifCorner.CornerRadius = UDim.new(0, 8)
notifCorner.Parent = notification

notification.Parent = MainFrame

-- Animação de entrada
notification:TweenPosition(UDim2.new(0, 10, 0, 15), "Out", "Quad", 0.3)
task.wait(2.5)

-- Animação de saída
notification:TweenPosition(UDim2.new(0, 10, 0, -45), "Out", "Quad", 0.3)
task.wait(0.3)
notification:Destroy()
