-- 🍎 Apple Hub - Versão Mobile Consertada
-- 📱 Otimizado para Mobile sem bugs

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

-- Detectar dispositivo
local player = Players.LocalPlayer
local isMobile = UserInputService.TouchEnabled
local screenSize = workspace.CurrentCamera.ViewportSize

print("📱 Dispositivo: " .. (isMobile and "Mobile" or "PC"))
print("📏 Tamanho da tela: " .. screenSize.X .. "x" .. screenSize.Y)

-- 🔧 CONFIGURAÇÃO ESPECIAL PARA MOBILE
local frameWidth = 330  -- Largura aumentada para caber "NEED KEY"
local frameHeight = 520  -- Altura ajustada
local startPosition = UDim2.new(0.5, -frameWidth/2, 0.25, 0)  -- Mais alto

-- Se for PC, usar configurações diferentes
if not isMobile then
    frameWidth = 400
    frameHeight = 570
    startPosition = UDim2.new(0.5, -frameWidth/2, 0.5, -frameHeight/2)
end

-- Criar GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AppleHubMobileFixed"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false

-- Colocar na interface (método universal)
ScreenGui.Parent = game:GetService("CoreGui")

-- Frame principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, frameWidth, 0, frameHeight)
MainFrame.Position = startPosition
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true
MainFrame.AnchorPoint = Vector2.new(0.5, 0)

-- Borda e cantos
local Border = Instance.new("UIStroke")
Border.Color = Color3.fromRGB(50, 50, 60)
Border.Thickness = 2
Border.Parent = MainFrame

local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, 12)
Corner.Parent = MainFrame

-- Barra de título
local TitleBar = Instance.new("Frame")
TitleBar.Name = "TitleBar"
TitleBar.Size = UDim2.new(1, 0, 0, 45)
TitleBar.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
TitleBar.BorderSizePixel = 0

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(0.6, 0, 1, 0)
TitleLabel.Position = UDim2.new(0.2, 0, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "🍎 APPLE HUB"
TitleLabel.TextColor3 = Color3.new(1, 1, 1)
TitleLabel.TextSize = isMobile and 16 or 18
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextXAlignment = Enum.TextXAlignment.Center

-- Botão minimizar
local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Size = UDim2.new(0, 35, 0, 35)
MinimizeButton.Position = UDim2.new(1, -75, 0.5, -17.5)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(255, 193, 7)
MinimizeButton.Text = "─"
MinimizeButton.TextColor3 = Color3.new(0, 0, 0)
MinimizeButton.TextSize = isMobile and 18 or 16
MinimizeButton.Font = Enum.Font.GothamBold

-- Botão fechar
local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 35, 0, 35)
CloseButton.Position = UDim2.new(1, -35, 0.5, -17.5)
CloseButton.BackgroundColor3 = Color3.fromRGB(220, 60, 60)
CloseButton.Text = "✕"
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.TextSize = isMobile and 18 or 16
CloseButton.Font = Enum.Font.GothamBold

-- Arredondar botões
local buttonCorner = Instance.new("UICorner")
buttonCorner.CornerRadius = UDim.new(0, 8)
buttonCorner.Parent = MinimizeButton
buttonCorner:Clone().Parent = CloseButton

-- 🔧 ÁREA DE SCRIPTS OTIMIZADA PARA MOBILE
local ScriptsContainer = Instance.new("Frame")
ScriptsContainer.Name = "ScriptsContainer"
ScriptsContainer.Size = UDim2.new(1, 0, 1, -55)
ScriptsContainer.Position = UDim2.new(0, 0, 0, 50)
ScriptsContainer.BackgroundTransparency = 1
ScriptsContainer.ClipsDescendants = true

local ScriptsFrame = Instance.new("ScrollingFrame")
ScriptsFrame.Name = "ScriptsFrame"
ScriptsFrame.Size = UDim2.new(1, 0, 1, 0)
ScriptsFrame.Position = UDim2.new(0, 0, 0, 0)
ScriptsFrame.BackgroundTransparency = 1
ScriptsFrame.ScrollBarThickness = isMobile and 6 or 4
ScriptsFrame.ScrollBarImageColor3 = Color3.fromRGB(80, 80, 90)
ScriptsFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
ScriptsFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Padding = UDim.new(0, isMobile and 6 or 8)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- Montar interface
MainFrame.Parent = ScreenGui
TitleBar.Parent = MainFrame
TitleLabel.Parent = TitleBar
MinimizeButton.Parent = TitleBar
CloseButton.Parent = TitleBar
ScriptsContainer.Parent = MainFrame
ScriptsFrame.Parent = ScriptsContainer
UIListLayout.Parent = ScriptsFrame

-- 🎯 LISTA DE SCRIPTS ATUALIZADA (com Paintball [OP NEED KEY])
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
    -- 🎯 PAINTBALL [OP NEED KEY] ADICIONADO
    {name = "Paintball [OP KEY]", icon = "🎯", color = Color3.fromRGB(255, 87, 34), url = "https://raw.githubusercontent.com/LucasggkX/Games/main/Paintball.lua", specialBadge = true},
    {name = "Lennon Hub", icon = "🎵", color = Color3.fromRGB(156, 39, 176), url = "https://raw.githubusercontent.com/mxrtnttzflw/martinetti-scripts/main/script.lua"}
}

-- 🔧 FUNÇÃO PARA CRIAR BOTÕES OTIMIZADOS
local function createMobileButton(scriptData, index)
    local button = Instance.new("TextButton")
    button.Name = "Btn_" .. scriptData.name:gsub("%[", ""):gsub("%]", ""):gsub(" ", "_")
    button.Size = UDim2.new(0.92, 0, 0, isMobile and 55 or 50)
    button.BackgroundColor3 = Color3.fromRGB(40, 40, 46)
    button.BorderSizePixel = 0
    button.Text = ""
    button.AutoButtonColor = false
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 10)
    corner.Parent = button
    
    -- Ícone
    local icon = Instance.new("TextLabel")
    icon.Size = UDim2.new(0, 35, 0, 35)
    icon.Position = UDim2.new(0, 10, 0.5, -17.5)
    icon.BackgroundTransparency = 1
    icon.Text = scriptData.icon
    icon.TextSize = isMobile and 22 or 20
    icon.TextColor3 = scriptData.color
    icon.Font = Enum.Font.GothamBold
    icon.Parent = button
    
    -- Nome (truncado se necessário)
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -50, 0.6, 0)
    label.Position = UDim2.new(0, 50, 0.1, 0)
    label.BackgroundTransparency = 1
    label.Text = scriptData.name
    label.TextSize = isMobile and 14 or 13
    label.TextColor3 = Color3.new(1, 1, 1)
    label.Font = Enum.Font.GothamBold
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.TextTruncate = Enum.TextTruncate.AtEnd
    label.Parent = button
    
    -- Subtítulo para scripts com KEY
    if scriptData.name:find("%[KEY%]") or scriptData.name:find("KEY") then
        local subtitle = Instance.new("TextLabel")
        subtitle.Size = UDim2.new(1, -50, 0.4, 0)
        subtitle.Position = UDim2.new(0, 50, 0.6, 0)
        subtitle.BackgroundTransparency = 1
        subtitle.Text = "NEED KEY"
        subtitle.TextSize = isMobile and 11 or 10
        subtitle.TextColor3 = Color3.fromRGB(255, 152, 0)
        subtitle.Font = Enum.Font.GothamBold
        subtitle.TextXAlignment = Enum.TextXAlignment.Left
        subtitle.Parent = button
    end
    
    -- Badge KEY duplo para Paintball [OP KEY]
    if scriptData.name == "Paintball [OP KEY]" then
        local keyBadge = Instance.new("Frame")
        keyBadge.Size = UDim2.new(0, 45, 0, 18)
        keyBadge.Position = UDim2.new(1, -50, 0.5, -9)
        keyBadge.BackgroundColor3 = Color3.fromRGB(255, 87, 34)  -- Laranja do Paintball
        
        local badgeCorner = Instance.new("UICorner")
        badgeCorner.CornerRadius = UDim.new(0, 4)
        badgeCorner.Parent = keyBadge
        
        local keyText = Instance.new("TextLabel")
        keyText.Size = UDim2.new(1, 0, 1, 0)
        keyText.BackgroundTransparency = 1
        keyText.Text = "OP KEY"
        keyText.TextColor3 = Color3.new(1, 1, 1)
        keyText.TextSize = 9
        keyText.Font = Enum.Font.GothamBold
        keyText.Parent = keyBadge
        
        keyBadge.Parent = button
        
        -- Badge extra "OP"
        local opBadge = Instance.new("Frame")
        opBadge.Size = UDim2.new(0, 25, 0, 15)
        opBadge.Position = UDim2.new(0, 5, 0, 5)
        opBadge.BackgroundColor3 = Color3.fromRGB(255, 215, 0)  -- Dourado
        
        local opCorner = Instance.new("UICorner")
        opCorner.CornerRadius = UDim.new(0, 3)
        opCorner.Parent = opBadge
        
        local opText = Instance.new("TextLabel")
        opText.Size = UDim2.new(1, 0, 1, 0)
        opText.BackgroundTransparency = 1
        opText.Text = "OP"
        opText.TextColor3 = Color3.new(0, 0, 0)
        opText.TextSize = 8
        opText.Font = Enum.Font.GothamBold
        opText.Parent = opBadge
        
        opBadge.Parent = button
    elseif scriptData.name == "Auto Ping Pong [KEY]" then
        -- Badge para Auto Ping Pong
        local keyBadge = Instance.new("Frame")
        keyBadge.Size = UDim2.new(0, 30, 0, 16)
        keyBadge.Position = UDim2.new(1, -35, 0.5, -8)
        keyBadge.BackgroundColor3 = Color3.fromRGB(255, 152, 0)
        
        local badgeCorner = Instance.new("UICorner")
        badgeCorner.CornerRadius = UDim.new(0, 4)
        badgeCorner.Parent = keyBadge
        
        local keyText = Instance.new("TextLabel")
        keyText.Size = UDim2.new(1, 0, 1, 0)
        keyText.BackgroundTransparency = 1
        keyText.Text = "KEY"
        keyText.TextColor3 = Color3.new(1, 1, 1)
        keyText.TextSize = 9
        keyText.Font = Enum.Font.GothamBold
        keyText.Parent = keyBadge
        
        keyBadge.Parent = button
    end
    
    return button, icon, label
end

-- Criar botões
local buttons = {}
for i, scriptData in ipairs(scripts) do
    local button, icon, label = createMobileButton(scriptData, i)
    
    -- Efeito de toque (mobile)
    if isMobile then
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
    else
        -- Efeito hover (PC)
        button.MouseEnter:Connect(function()
            button.BackgroundColor3 = Color3.fromRGB(50, 50, 56)
        end)
        
        button.MouseLeave:Connect(function()
            button.BackgroundColor3 = Color3.fromRGB(40, 40, 46)
        end)
    end
    
    -- Função de execução
    button.MouseButton1Click:Connect(function()
        local originalColor = button.BackgroundColor3
        local originalText = label.Text
        local originalIcon = icon.Text
        
        -- Animação
        button.BackgroundColor3 = Color3.fromRGB(60, 60, 70)
        icon.Text = "⏳"
        label.Text = "Carregando..."
        
        task.wait(0.2)
        
        -- Configurar variáveis especiais
        if scriptData.name == "Lag + Aura" then
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
            
            -- Feedback especial para Paintball [OP KEY]
            if scriptData.name == "Paintball [OP KEY]" then
                task.wait(0.5)
                local specialNotif = Instance.new("TextLabel")
                specialNotif.Size = UDim2.new(1, -20, 0, 30)
                specialNotif.Position = UDim2.new(0, 10, 0, -35)
                specialNotif.BackgroundColor3 = Color3.fromRGB(255, 87, 34)
                specialNotif.TextColor3 = Color3.new(1, 1, 1)
                specialNotif.Text = "🎯 Paintball OP ativado!"
                specialNotif.TextSize = 13
                specialNotif.Font = Enum.Font.GothamBold
                specialNotif.TextXAlignment = Enum.TextXAlignment.Center
                
                local notifCorner = Instance.new("UICorner")
                notifCorner.CornerRadius = UDim.new(0, 6)
                notifCorner.Parent = specialNotif
                
                specialNotif.Parent = MainFrame
                
                specialNotif:TweenPosition(UDim2.new(0, 10, 0, 15), "Out", "Quad", 0.3)
                task.wait(2.5)
                specialNotif:TweenPosition(UDim2.new(0, 10, 0, -35), "Out", "Quad", 0.3)
                task.wait(0.3)
                specialNotif:Destroy()
            end
            
            print("✅ " .. scriptData.name .. " executado!")
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
    end)
    
    -- Conectar toque mobile
    if isMobile then
        button.TouchTap:Connect(function()
            button.MouseButton1Click:Fire()
        end)
    end
    
    button.Parent = ScriptsFrame
    buttons[scriptData.name] = {button = button, icon = icon, label = label}
end

-- 🔧 SISTEMA DE DRAG SIMPLIFICADO PARA MOBILE
local dragging = false
local dragStart, frameStart

TitleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or 
       (isMobile and input.UserInputType == Enum.UserInputType.Touch) then
        dragging = true
        dragStart = input.Position
        frameStart = MainFrame.Position
        TitleBar.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
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
        TitleBar.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
    end
end)

-- 🔧 SISTEMA DE MINIMIZAR/FECHAR
local minimized = false

MinimizeButton.MouseButton1Click:Connect(function()
    minimized = not minimized
    
    if minimized then
        MainFrame:TweenSize(UDim2.new(0, 120, 0, 45), "Out", "Quad", 0.2)
        ScriptsContainer.Visible = false
        MinimizeButton.Text = "+"
    else
        MainFrame:TweenSize(UDim2.new(0, frameWidth, 0, frameHeight), "Out", "Quad", 0.2)
        ScriptsContainer.Visible = true
        MinimizeButton.Text = "─"
    end
end)

if isMobile then
    MinimizeButton.TouchTap:Connect(function()
        MinimizeButton.MouseButton1Click:Fire()
    end)
end

CloseButton.MouseButton1Click:Connect(function()
    local exitTween = TweenService:Create(MainFrame, TweenInfo.new(0.3), {
        Position = UDim2.new(0.5, -frameWidth/2, -0.5, 0),
        BackgroundTransparency = 1
    })
    exitTween:Play()
    exitTween.Completed:Wait()
    ScreenGui:Destroy()
end)

if isMobile then
    CloseButton.TouchTap:Connect(function()
        CloseButton.MouseButton1Click:Fire()
    end)
end

-- 🔧 AJUSTAR TAMANHO DO CANVAS
UIListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    local contentHeight = UIListLayout.AbsoluteContentSize.Y
    local maxHeight = frameHeight - 100
    
    if contentHeight > maxHeight then
        ScriptsFrame.ScrollingEnabled = true
        ScriptsFrame.CanvasSize = UDim2.new(0, 0, 0, contentHeight + 20)
    else
        ScriptsFrame.ScrollingEnabled = false
        ScriptsFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
    end
end)

-- 🔧 ANIMAÇÃO DE ENTRADA
MainFrame.BackgroundTransparency = 1
TitleBar.BackgroundTransparency = 1

task.wait(0.1)

for i = 1, 10 do
    MainFrame.BackgroundTransparency = 1 - (i/10)
    TitleBar.BackgroundTransparency = 1 - (i/10)
    task.wait(0.02)
end

-- 🔧 NOTIFICAÇÃO INICIAL
print("════════════════════════════════")
print("🍎 APPLE HUB MOBILE FIX")
print("📱 Otimizado para: " .. (isMobile and "MOBILE" or "PC"))
print("🎯 Scripts: " .. #scripts)
print("🆕 NOVO: Paintball [OP KEY] 🎯")
print("📏 Tamanho: " .. frameWidth .. "x" .. frameHeight)
print("════════════════════════════════")

task.wait(0.5)

-- Notificação visual
local notif = Instance.new("TextLabel")
notif.Size = UDim2.new(1, -20, 0, 35)
notif.Position = UDim2.new(0, 10, 0, -40)
notif.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
notif.TextColor3 = Color3.new(1, 1, 1)
notif.Text = "✅ Paintball [OP KEY] adicionado!"
notif.TextSize = isMobile and 13 or 14
notif.Font = Enum.Font.GothamBold
notif.TextXAlignment = Enum.TextXAlignment.Center

local notifCorner = Instance.new("UICorner")
notifCorner.CornerRadius = UDim.new(0, 8)
notifCorner.Parent = notif

notif.Parent = MainFrame

notif:TweenPosition(UDim2.new(0, 10, 0, 10), "Out", "Quad", 0.3)
task.wait(2)
notif:TweenPosition(UDim2.new(0, 10, 0, -40), "Out", "Quad", 0.3)
task.wait(0.3)
notif:Destroy()

-- 🔧 SISTEMA DE ORIENTAÇÃO MOBILE
if isMobile then
    local function adjustForOrientation()
        task.wait(0.1)
        local viewport = workspace.CurrentCamera.ViewportSize
        
        if viewport.Y > viewport.X then
            -- Portrait (vertical)
            frameWidth = math.min(330, viewport.X * 0.9)
        else
            -- Landscape (horizontal)
            frameWidth = math.min(420, viewport.X * 0.7)
        end
        
        MainFrame.Size = UDim2.new(0, frameWidth, 0, frameHeight)
        MainFrame.Position = UDim2.new(0.5, -frameWidth/2, 0.25, 0)
    end
    
    -- Ajustar inicialmente
    adjustForOrientation()
    
    -- Ajustar quando a tela mudar
    workspace.CurrentCamera:GetPropertyChangedSignal("ViewportSize"):Connect(adjustForOrientation)
end

-- 🔧 AVISO PARA SCRIPTS COM KEY
local keyWarning = Instance.new("TextLabel")
keyWarning.Size = UDim2.new(1, -20, 0, 25)
keyWarning.Position = UDim2.new(0, 10, 1, -30)
keyWarning.BackgroundColor3 = Color3.fromRGB(255, 152, 0)
keyWarning.TextColor3 = Color3.new(1, 1, 1)
keyWarning.Text = "⚠️ Scripts com KEY precisam de chave"
keyWarning.TextSize = isMobile and 11 or 10
keyWarning.Font = Enum.Font.Gotham
keyWarning.TextXAlignment = Enum.TextXAlignment.Center
keyWarning.TextWrapped = true

local warningCorner = Instance.new("UICorner")
warningCorner.CornerRadius = UDim.new(0, 6)
warningCorner.Parent = keyWarning

keyWarning.Parent = MainFrame
