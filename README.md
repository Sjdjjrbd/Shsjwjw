# Fly-teste
Estou apenas testando
--[[
    Script Fly Toggle para Roblox Mobile
    - Toque no botão para ativar/desativar o voo.
    - Interface amigável e adaptada para mobile.
    - Salve este script como LocalScript em StarterPlayerScripts.
]]--

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local mouse = player:GetMouse()
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

-- Interface Setup
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.ResetOnSpawn = false
ScreenGui.Name = "FlyMobileUI"
ScreenGui.Parent = player:WaitForChild("PlayerGui")

local flyButton = Instance.new("ImageButton")
flyButton.Name = "FlyButton"
flyButton.Size = UDim2.new(0, 80, 0, 80)
flyButton.Position = UDim2.new(1, -95, 1, -95)
flyButton.BackgroundTransparency = 0.1
flyButton.BackgroundColor3 = Color3.fromRGB(34, 34, 64)
flyButton.Image = "rbxassetid://7733960981" -- Ícone de asa (você pode trocar)
flyButton.ScaleType = Enum.ScaleType.Fit
flyButton.AnchorPoint = Vector2.new(0.5,0.5)
flyButton.Parent = ScreenGui
flyButton.AutoButtonColor = true
flyButton.ClipsDescendants = true
flyButton.BorderSizePixel = 0

-- Sombras e detalhes
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0.5, 0)
UICorner.Parent = flyButton

local UIStroke = Instance.new("UIStroke")
UIStroke.Color = Color3.fromRGB(61, 180, 242)
UIStroke.Thickness = 4
UIStroke.Parent = flyButton

local flyLabel = Instance.new("TextLabel")
flyLabel.Parent = flyButton
flyLabel.Size = UDim2.new(1, 0, 0.35, 0)
flyLabel.Position = UDim2.new(0, 0, 1, -25)
flyLabel.BackgroundTransparency = 1
flyLabel.Text = "Ativar Voo"
flyLabel.TextColor3 = Color3.fromRGB(230, 255, 255)
flyLabel.TextScaled = true
flyLabel.Font = Enum.Font.GothamBold

-- Fly Logic
local flying = false
local connection = nil

-- Função para voar
local function fly()
    local char = player.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") or not char:FindFirstChildOfClass("Humanoid") then return end
    local humanoid = char:FindFirstChildOfClass("Humanoid")
    humanoid.PlatformStand = true
    
    local bv = Instance.new("BodyVelocity")
    bv.Name = "FlyVelocity"
    bv.MaxForce = Vector3.new(1e5, 1e5, 1e5)
    bv.Velocity = Vector3.new(0,0,0)
    bv.Parent = char.HumanoidRootPart

    connection = RunService.RenderStepped:Connect(function()
        local moveDirection = Vector3.new()
        -- Mobile usa Humanoid.MoveDirection ao invés de teclas
        if humanoid.MoveDirection.Magnitude > 0 then
            moveDirection = humanoid.MoveDirection.unit
        end
        bv.Velocity = Vector3.new(moveDirection.X, 1, moveDirection.Z) * 50
    end)
end

-- Função para parar de voar
local function unfly()
    local char = player.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") or not char:FindFirstChildOfClass("Humanoid") then return end
    local humanoid = char:FindFirstChildOfClass("Humanoid")
    humanoid.PlatformStand = false
    if char.HumanoidRootPart:FindFirstChild("FlyVelocity") then
        char.HumanoidRootPart.FlyVelocity:Destroy()
    end
    if connection then
        connection:Disconnect()
        connection = nil
    end
end

-- Alternar Voo
local function toggleFly()
    flying = not flying
    if flying then
        fly()
        flyButton.ImageColor3 = Color3.fromRGB(61,180,242)
        flyLabel.Text = "Voo ON"
    else
        unfly()
        flyButton.ImageColor3 = Color3.fromRGB(180,70,70)
        flyLabel.Text = "Voo OFF"
        wait(0.4)
        flyLabel.Text = "Ativar Voo"
        flyButton.ImageColor3 = Color3.new(1,1,1)
    end
end

flyButton.MouseButton1Click:Connect(toggleFly)

-- Segurança: Para caso morra, reseta o estado
player.CharacterAdded:Connect(function()
    flying = false
    flyLabel.Text = "Ativar Voo"
    if connection then
        connection:Disconnect()
        connection = nil
    end
    -- Remove BodyVelocity se existir
    wait(1)
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") and char.HumanoidRootPart:FindFirstChild("FlyVelocity") then
        char.HumanoidRootPart.FlyVelocity:Destroy()
    end
end)
