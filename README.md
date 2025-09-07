-- Carregar a UI library
local redzlib = loadstring(game:HttpGet("https://raw.githubusercontent.com/tbao143/Library-ui/refs/heads/main/Redzhubui"))()

-- Criar a janela principal (Zoeir4 hub)
local Window = redzlib:MakeWindow({
    Title = "Zoeir4 hub",
    SubTitle = "by Xzbrian_darkzX",
    SaveFolder = "Zoeir4Hub | redz lib v5.lua"
})

-- Ícone de minimizar com imagem ajustada
Window:AddMinimizeButton({
    Button = { Image = "rbxassetid://18751483361", BackgroundTransparency = 0 },
    Corner = { CornerRadius = UDim.new(35, 1) },
})

-- Criar a primeira aba (DISCORD)
local DiscordTab = Window:MakeTab({ Name = "💬 DISCORD", Icon = "rbxassetid://7734053495" })

-- Auto copiar link ao executar
setclipboard("https://discord.gg/Cvy8kXcc")

-- Botão para copiar o link do servidor
DiscordTab:AddButton({
    Name = "Copiar Link do Servidor",
    Callback = function()
        setclipboard("https://discord.gg/Cvy8kXcc")
    end
})

-- Criar a aba Cliente (segunda aba)
local Tab = Window:MakeTab({ Name = "👤 Cliente", Icon = "rbxassetid://7733674079" })

-- Variáveis padrão
local SpeedValue = 16
local JumpValue = 50
local GravityDefault = 196.2

local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- ===== Funções =====
local function setSpeed(value)
    SpeedValue = value
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.WalkSpeed = SpeedValue
    end
end

local function setJump(value)
    JumpValue = value
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.JumpPower = JumpValue
    end
end

local function setGravity(value)
    workspace.Gravity = value
end

-- ===== Sliders e Botões =====
Tab:AddSlider({ Name = "Velocidade", Min = 16, Max = 200, Default = 16, Increment = 1, Callback = setSpeed })
Tab:AddButton({ Name = "Resetar Velocidade", Callback = function() setSpeed(16) end })

Tab:AddSlider({ Name = "Pulo", Min = 50, Max = 500, Default = 50, Increment = 5, Callback = setJump })
Tab:AddButton({ Name = "Resetar Pulo", Callback = function() setJump(50) end })

Tab:AddSlider({ Name = "Gravidade", Min = 0, Max = 500, Default = GravityDefault, Increment = 1, Callback = setGravity })
Tab:AddButton({ Name = "Resetar Gravidade", Callback = function() setGravity(GravityDefault) end })

-- ===== Noclip =====
local noclip = false
Tab:AddToggle({
    Name = "Noclip",
    Default = false,
    Callback = function(Value)
        noclip = Value
        game.StarterGui:SetCore("SendNotification", { Title = "Zoeir4 hub", Text = noclip and "Noclip ON" or "Noclip OFF", Duration = 3 })
    end
})

RunService.Stepped:Connect(function()
    if noclip and player.Character then
        for _, part in pairs(player.Character:GetDescendants()) do
            if part:IsA("BasePart") and part.CanCollide then
                part.CanCollide = false
            end
        end
    end
end)

-- ===== Fly V3 =====
Tab:AddButton({
    Name = "Fly V3",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Fly-V3-48359"))()
    end
})

-- ===== TP TOOL =====
Tab:AddButton({
    Name = "TP Tool",
    Callback = function()
        local tool = Instance.new("Tool")
        tool.RequiresHandle = false
        tool.Name = "TP Tool"
        tool.Activated:Connect(function()
            local mouse = player:GetMouse()
            if mouse.Hit then
                player.Character:MoveTo(mouse.Hit.p)
            end
        end)
        tool.Parent = player.Backpack
        game.StarterGui:SetCore("SendNotification", { Title = "Zoeir4 hub", Text = "TP Tool adicionado!", Duration = 3 })
    end
})

-- ===== ESP Players Vermelho =====
local ESPEnabled = false
Tab:AddToggle({
    Name = "ESP Players (Vermelho)",
    Default = false,
    Callback = function(Value)
        ESPEnabled = Value
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= player then
                local char = plr.Character
                if char then
                    local highlight = char:FindFirstChild("Zoeir4ESP")
                    if ESPEnabled then
                        if not highlight then
                            highlight = Instance.new("Highlight")
                            highlight.Name = "Zoeir4ESP"
                            highlight.Adornee = char
                            highlight.FillColor = Color3.fromRGB(255, 0, 0)
                            highlight.OutlineColor = Color3.fromRGB(255, 0, 0)
                            highlight.Parent = char
                        end
                    else
                        if highlight then highlight:Destroy() end
                    end
                end
            end
        end
    end
})

Players.PlayerAdded:Connect(function(plr)
    plr.CharacterAdded:Connect(function(char)
        if ESPEnabled then
            local highlight = Instance.new("Highlight")
            highlight.Name = "Zoeir4ESP"
            highlight.Adornee = char
            highlight.FillColor = Color3.fromRGB(255, 0, 0)
            highlight.OutlineColor = Color3.fromRGB(255, 0, 0)
            highlight.Parent = char
        end
    end)
end)

-- ===== Anti Lag =====
local AntiLagEnabled = false
Tab:AddToggle({
    Name = "Anti Lag",
    Default = false,
    Callback = function(Value)
        AntiLagEnabled = Value
        if AntiLagEnabled then
            for _, v in pairs(workspace:GetDescendants()) do
                if v:IsA("ParticleEmitter") or v:IsA("Trail") or v:IsA("Beam") then
                    v.Enabled = false
                elseif v:IsA("Texture") or v:IsA("Decal") then
                    v.Transparency = 1
                elseif v:IsA("MeshPart") or v:IsA("UnionOperation") then
                    v.Material = Enum.Material.Plastic
                    v.Reflectance = 0
                end
            end
            for _, light in pairs(workspace:GetDescendants()) do
                if light:IsA("Light") then
                    light.Enabled = false
                end
            end
            game.Lighting.GlobalShadows = false
            game.Lighting.FogEnd = 999999
        else
            for _, v in pairs(workspace:GetDescendants()) do
                if v:IsA("ParticleEmitter") or v:IsA("Trail") or v:IsA("Beam") then
                    v.Enabled = true
                elseif v:IsA("Texture") or v:IsA("Decal") then
                    v.Transparency = 0
                elseif v:IsA("MeshPart") or v:IsA("UnionOperation") then
                    v.Material = Enum.Material.SmoothPlastic
                end
            end
            for _, light in pairs(workspace:GetDescendants()) do
                if light:IsA("Light") then
                    light.Enabled = true
                end
            end
            game.Lighting.GlobalShadows = true
            game.Lighting.FogEnd = 1000
        end
    end
})

-- ===== Bio Móvel =====
local gui = Instance.new("ScreenGui")
gui.Name = "Zoeir4HubBio"
gui.Parent = player:WaitForChild("PlayerGui")
local label = Instance.new("TextLabel")
label.Parent = gui
label.Size = UDim2.new(0, 200, 0, 50)
label.Position = UDim2.new(0.5, -100, 0.1, 0)
label.BackgroundTransparency = 0.3
label.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
label.TextColor3 = Color3.fromRGB(255, 255, 255)
label.TextScaled = true
label.Font = Enum.Font.GothamBold
label.Text = "user Zoeir4 hub"
label.TextStrokeTransparency = 0.2
label.Active = true
label.Draggable = true

-- ===== Aba Casa =====
local CasaTab = Window:MakeTab({ Name = "🔨 Casa", Icon = "rbxassetid://7733960981" })
local casaNoclip = false
CasaTab:AddButton({
    Name = "UNBAN (Noclip)",
    Callback = function()
        casaNoclip = not casaNoclip
        game.StarterGui:SetCore("SendNotification", { Title="Zoeir4 hub", Text = casaNoclip and "Noclip ON" or "Noclip OFF", Duration=3 })
    end
})

RunService.Stepped:Connect(function()
    if casaNoclip and player.Character then
        for _, part in pairs(player.Character:GetDescendants()) do
            if part:IsA("BasePart") and part.CanCollide then
                part.CanCollide = false
            end
        end
    end
end)
