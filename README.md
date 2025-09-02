
-- Services
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer

-- Variables
local savedPosition = nil
local forwardDistance = 20 -- studs forward
local tweenTime = 1 -- seconds for tween speed

-- GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "TeleportGui"
ScreenGui.Parent = game.CoreGui

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 200, 0, 160)
Frame.Position = UDim2.new(0, 50, 0, 50)
Frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Frame.Active = true
Frame.Draggable = true
Frame.Parent = ScreenGui

-- Save Position Button
local saveBtn = Instance.new("TextButton")
saveBtn.Size = UDim2.new(0, 180, 0, 40)
saveBtn.Position = UDim2.new(0, 10, 0, 10)
saveBtn.Text = "Save Position"
saveBtn.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
saveBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
saveBtn.Parent = Frame

-- Teleport to Saved Position Button
local tpSavedBtn = Instance.new("TextButton")
tpSavedBtn.Size = UDim2.new(0, 180, 0, 40)
tpSavedBtn.Position = UDim2.new(0, 10, 0, 60)
tpSavedBtn.Text = "Teleport to Saved"
tpSavedBtn.BackgroundColor3 = Color3.fromRGB(0, 255, 128)
tpSavedBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
tpSavedBtn.Parent = Frame

-- Teleport Forward Button
local forwardBtn = Instance.new("TextButton")
forwardBtn.Size = UDim2.new(0, 180, 0, 40)
forwardBtn.Position = UDim2.new(0, 10, 0, 110)
forwardBtn.Text = "Teleport Forward"
forwardBtn.BackgroundColor3 = Color3.fromRGB(255, 170, 0)
forwardBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
forwardBtn.Parent = Frame

-- Smooth Teleport Function
local function smoothTeleport(hrp, targetPos)
    local tweenInfo = TweenInfo.new(tweenTime, Enum.EasingStyle.Sine, Enum.EasingDirection.Out)
    local tween = TweenService:Create(hrp, tweenInfo, {CFrame = CFrame.new(targetPos)})
    tween:Play()
end

-- Button Functions
saveBtn.MouseButton1Click:Connect(function()
    local hrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    if hrp then
        savedPosition = hrp.Position
        saveBtn.Text = "Position Saved ✅"
    end
end)

tpSavedBtn.MouseButton1Click:Connect(function()
    local hrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    if hrp and savedPosition then
        smoothTeleport(hrp, savedPosition)
        tpSavedBtn.Text = "Teleported ✅"
    else
        tpSavedBtn.Text = "No Position Saved ❌"
    end
end)

forwardBtn.MouseButton1Click:Connect(function()
    local hrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    if hrp then
        local targetPos = hrp.Position + hrp.CFrame.LookVector * forwardDistance
        smoothTeleport(hrp, targetPos)
        forwardBtn.Text = "Teleported Forward ✅"
    end
end)
