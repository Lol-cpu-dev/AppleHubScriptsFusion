-- Apple Hub Profissional
-- Estilo limpo e funcional

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local GuiService = game:GetService("GuiService")

-- Detectar dispositivo
local isMobile = UserInputService.TouchEnabled
local player = Players.LocalPlayer
local screenSize = workspace.CurrentCamera.ViewportSize

-- Tamanhos responsivos
local MAXIMIZED_SIZE = isMobile and UDim2.new(0.9, 0, 0, 450) or UDim2.new(0, 400, 0, 500)
local MINIMIZED_SIZE = isMobile and UDim2.new(0, 150, 0, 40) or UDim2.new(0, 150, 0, 40)

-- Cria a interface
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AppleHub"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false

-- Proteger GUI
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

-- Frame principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = MAXIMIZED_SIZE
MainFrame.Position = UDim2.new(0.5, -200, 1.3, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true

-- Borda sutil
local Border = Instance.new("UIStroke")
Border.Color = Color3.fromRGB(60, 60, 65)
Border.Thickness = 2
Border.Parent = MainFrame

-- Barra de título
local TitleBar = Instance.new("Frame")
TitleBar.Name = "TitleBar"
TitleBar.Size = UDim2.new(1, 0, 0, 40)
TitleBar.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
TitleBar.BorderSizePixel = 0

-- Título
local TitleLabel = Instance.new("TextLabel")
TitleLabel.Name = "Title"
TitleLabel.Size = UDim2.new(0.7, 0, 1, 0)
TitleLabel.Position = UDim2.new(0.15, 0, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "🍎 Apple Hub"
TitleLabel.TextSize = 20
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.TextXAlignment = Enum.TextXAlignment.Center

-- Botão minimizar
local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Size = UDim2.new(0, 30, 0, 30)
MinimizeButton.Position = UDim2.new(1, -70, 0, 5)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(80, 80, 85)
MinimizeButton.BorderSizePixel = 0
MinimizeButton.Text = "─"
MinimizeButton.TextSize = 18
MinimizeButton.Font = Enum.Font.GothamBold
MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)

-- Botão fechar
local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -35, 0, 5)
CloseButton.BackgroundColor3 = Color3.fromRGB(200, 60, 60)
CloseButton.BorderSizePixel = 0
CloseButton.Text = "✕"
CloseButton.TextSize = 18
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)

-- Container dos scripts
local ScriptsFrame = Instance.new("ScrollingFrame")
ScriptsFrame.Name = "ScriptsFrame"
ScriptsFrame.Size = UDim2.new(1, -20, 1, -60)
ScriptsFrame.Position = UDim2.new(0, 10, 0, 50)
ScriptsFrame.BackgroundTransparency = 1
ScriptsFrame.BorderSizePixel = 0
ScriptsFrame.ScrollBarThickness = 5
ScriptsFrame.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 105)
ScriptsFrame.CanvasSize = UDim2.new(0, 0, 0, 450)

-- Layout
local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Padding = UDim.new(0, 10)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- Montar interface
MainFrame.Parent = ScreenGui
TitleBar.Parent = MainFrame
TitleLabel.Parent = TitleBar
MinimizeButton.Parent = TitleBar
CloseButton.Parent = TitleBar
ScriptsFrame.Parent = MainFrame
UIListLayout.Parent = ScriptsFrame

-- Criar botões de script
local function createScriptButton(name, displayName, icon, color)
    local button = Instance.new("TextButton")
    button.Name = name
    button.Size = UDim2.new(0.95, 0, 0, 55)
    button.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
    button.BorderSizePixel = 0
    button.Text = ""
    button.AutoButtonColor = false
    
    local iconLabel = Instance.new("TextLabel")
    iconLabel.Name = "Icon"
    iconLabel.Size = UDim2.new(0, 35, 0, 35)
    iconLabel.Position = UDim2.new(0, 10, 0.5, -17.5)
    iconLabel.BackgroundTransparency = 1
    iconLabel.Text = icon
    iconLabel.TextSize = 22
    iconLabel.Font = Enum.Font.GothamBold
    iconLabel.TextColor3 = color
    iconLabel.Parent = button
    
    local textLabel = Instance.new("TextLabel")
    textLabel.Name = "Label"
    textLabel.Size = UDim2.new(1, -55, 1, 0)
    textLabel.Position = UDim2.new(0, 50, 0, 0)
    textLabel.BackgroundTransparency = 1
    textLabel.Text = displayName
    textLabel.TextSize = 18
    textLabel.Font = Enum.Font.GothamBold
    textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    textLabel.TextXAlignment = Enum.TextXAlignment.Left
    textLabel.Parent = button
    
    local buttonCorner = Instance.new("UICorner")
    buttonCorner.CornerRadius = UDim.new(0, 8)
    buttonCorner.Parent = button
    
    return button, iconLabel, textLabel
end

-- Criar todos os botões
local NamelessButton, NamelessIcon, NamelessLabel = createScriptButton(
    "Nameless", "Nameless Hub [OP]", "👑", Color3.fromRGB(255, 215, 0))

local ChilliButton, ChilliIcon, ChilliLabel = createScriptButton(
    "Chilli", "Chilli Hub [OP]", "🌶️", Color3.fromRGB(255, 69, 58))

local UCTButton, UCTIcon, UCTLabel = createScriptButton(
    "UCT", "UCT HUB [OP]", "⚡", Color3.fromRGB(0, 191, 255))

local KurdButton, KurdIcon, KurdLabel = createScriptButton(
    "Kurd", "Kurd Hub [OP]", "🏔️", Color3.fromRGB(34, 139, 34))

local LagAuraButton, LagAuraIcon, LagAuraLabel = createScriptButton(
    "LagAura", "Lag+Aura [OP]", "💀", Color3.fromRGB(178, 34, 34))

local AdminSpamButton, AdminSpamIcon, AdminSpamLabel = createScriptButton(
    "AdminSpam", "Admin Panel Spam [OP+]", "👑", Color3.fromRGB(255, 215, 0))

local ChilliPrivateButton, ChilliPrivateIcon, ChilliPrivateLabel = createScriptButton(
    "ChilliPrivate", "Chilli Private Server [OP]", "🔓", Color3.fromRGB(0, 200, 83))

local SpeedStealButton, SpeedStealIcon, SpeedStealLabel = createScriptButton(
    "SpeedSteal", "Speed Steal Boost [OP]", "⚡", Color3.fromRGB(0, 150, 255))

-- Adicionar botões
NamelessButton.Parent = ScriptsFrame
ChilliButton.Parent = ScriptsFrame
UCTButton.Parent = ScriptsFrame
KurdButton.Parent = ScriptsFrame
LagAuraButton.Parent = ScriptsFrame
AdminSpamButton.Parent = ScriptsFrame
ChilliPrivateButton.Parent = ScriptsFrame
SpeedStealButton.Parent = ScriptsFrame

-- ANIMAÇÃO DE ENTRADA
MainFrame.BackgroundTransparency = 1
task.wait(0.1)

for i = 1, 20 do
    MainFrame.BackgroundTransparency = 1 - (i/20)
    task.wait(0.01)
end

MainFrame:TweenPosition(
    UDim2.new(0.5, -200, 0.5, -250),
    "Out",
    "Quad",
    0.3,
    true
)

-- FECHAR & MINIMIZAR
local minimized = false

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

MinimizeButton.MouseButton1Click:Connect(function()
    minimized = not minimized

    if minimized then
        MainFrame:TweenSize(MINIMIZED_SIZE, "Out", "Quad", 0.3, true)
        ScriptsFrame.Visible = false
        MinimizeButton.Text = "+"
    else
        MainFrame:TweenSize(MAXIMIZED_SIZE, "Out", "Quad", 0.3, true)
        ScriptsFrame.Visible = true
        MinimizeButton.Text = "─"
    end
end)

-- DRAG LIVRE
local dragging = false
local dragStart, startPos

local function update(input)
    if dragging then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(
            0,
            startPos.X.Offset + delta.X,
            0,
            startPos.Y.Offset + delta.Y
        )
    end
end

MainFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = MainFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

MainFrame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        UserInputService.InputChanged:Connect(function(changed)
            if changed == input then
                update(changed)
            end
        end)
    end
end)

-- Sistema para mobile
if isMobile then
    TitleBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = MainFrame.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)
end

-- FUNÇÃO PARA EXECUTAR SCRIPTS
local function executeScript(button, icon, label, originalText, scriptFunc)
    local function onClick()
        -- Animação de clique
        button.BackgroundColor3 = Color3.fromRGB(60, 60, 65)
        task.wait(0.1)
        button.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
        
        icon.Text = "⏳"
        label.Text = "Executando..."
        
        task.wait(0.5)
        
        local success, error = pcall(scriptFunc)
        
        if success then
            icon.Text = "✅"
            label.Text = "Sucesso!"
        else
            icon.Text = "❌"
            label.Text = "Erro!"
            warn("Erro ao executar:", error)
        end
        
        task.wait(1)
        label.Text = originalText
        
        -- Restaurar ícone original
        if button.Name == "Nameless" then icon.Text = "👑"
        elseif button.Name == "Chilli" then icon.Text = "🌶️"
        elseif button.Name == "UCT" then icon.Text = "⚡"
        elseif button.Name == "Kurd" then icon.Text = "🏔️"
        elseif button.Name == "LagAura" then icon.Text = "💀"
        elseif button.Name == "AdminSpam" then icon.Text = "👑"
        elseif button.Name == "ChilliPrivate" then icon.Text = "🔓"
        elseif button.Name == "SpeedSteal" then icon.Text = "⚡" end
    end
    
    -- PC
    button.MouseButton1Click:Connect(onClick)
    
    -- Mobile
    if isMobile then
        button.TouchTap:Connect(onClick)
    end
end

-- CONFIGURAR TODOS OS SCRIPTS
executeScript(NamelessButton, NamelessIcon, NamelessLabel, "Nameless Hub [OP]", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/ily123950/Vulkan/refs/heads/main/Tr"))()
end)

executeScript(ChilliButton, ChilliIcon, ChilliLabel, "Chilli Hub [OP]", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/tienkhanh1/spicy/main/Chilli.lua"))()
end)

executeScript(UCTButton, UCTIcon, UCTLabel, "UCT HUB [OP]", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/UCT-hub/main/refs/heads/main/stealabrainrot"))()
end)

executeScript(KurdButton, KurdIcon, KurdLabel, "Kurd Hub [OP]", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/Ninja10908/S4/refs/heads/main/Kurdhub"))()
end)

executeScript(LagAuraButton, LagAuraIcon, LagAuraLabel, "Lag+Aura [OP]", function()
    getgenv().ServerDestroyerV6 = {
        Comprar = false,
        Spam = true
    }
    loadstring(game:HttpGet("https://tcscripts.discloud.app/scripts/serverdestroyerv6"))()
end)

executeScript(AdminSpamButton, AdminSpamIcon, AdminSpamLabel, "Admin Panel Spam [OP+]", function()
    loadstring(game:HttpGet("https://api.luarmor.net/files/v3/loaders/fc9523e876bada3b7ed4ebe004cb8cf9.lua"))()
end)

executeScript(ChilliPrivateButton, ChilliPrivateIcon, ChilliPrivateLabel, "Chilli Private Server [OP]", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/tienkhanh1/spicy/main/PrivateServer"))()
end)

executeScript(SpeedStealButton, SpeedStealIcon, SpeedStealLabel, "Speed Steal Boost [OP]", function()
    loadstring(game:HttpGet("https://pastebin.com/raw/rmxfZDPd"))()
end)

-- Efeitos hover (PC apenas)
if not isMobile then
    local function setupHover(button)
        button.MouseEnter:Connect(function()
            button.BackgroundColor3 = Color3.fromRGB(55, 55, 60)
        end)
        
        button.MouseLeave:Connect(function()
            button.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
        end)
    end
    
    for _, button in pairs({
        NamelessButton, ChilliButton, UCTButton, KurdButton,
        LagAuraButton, AdminSpamButton, ChilliPrivateButton, SpeedStealButton
    }) do
        setupHover(button)
    end
end

-- Ajustar canvas size
UIListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    ScriptsFrame.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 20)
end)

-- Console message
print("🍎 Apple Hub carregado com sucesso!")
print("📱 Dispositivo: " .. (isMobile and "Mobile" or "PC"))
print("📜 Scripts disponíveis: 8")
print("🎯 Estilo: Profissional limpo")
