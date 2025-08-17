local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Workspace = game:GetService("Workspace")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

-- GUI Setup (Basic example)
local ScreenGui = Instance.new("ScreenGui", LocalPlayer.PlayerGui)
ScreenGui.Name = "WildHorseIslandsScript"

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 300, 0, 380)
MainFrame.Position = UDim2.new(0.05, 0, 0.2, 0)
MainFrame.BackgroundColor3 = Color3.new(0.16, 0.16, 0.16)
MainFrame.BorderSizePixel = 0

local Title = Instance.new("TextLabel", MainFrame)
Title.Text = "Wild Horse Islands Script"
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.TextColor3 = Color3.new(1,1,1)
Title.Font = Enum.Font.SourceSansBold
Title.TextScaled = true

-- Feature toggles
local autoFarmEnabled = false
local flyEnabled = false

-- Simple Button Creation Function
local function createButton(text, posY, callback)
    local btn = Instance.new("TextButton", MainFrame)
    btn.Text = text
    btn.Size = UDim2.new(0.9, 0, 0, 40)
    btn.Position = UDim2.new(0.05, 0, 0, posY)
    btn.BackgroundColor3 = Color3.new(0.25, 0.25, 0.35)
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Font = Enum.Font.SourceSans
    btn.TextScaled = true
    btn.MouseButton1Click:Connect(callback)
    return btn
end

-- Auto Farm Example (Collects nearby apples/flowers)
local function autoFarmLoop()
    while autoFarmEnabled do
        for _, item in pairs(Workspace:GetChildren()) do
            if item:IsA("Part") and (item.Name == "Apple" or item.Name == "Flower") then
                if (LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")) then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = item.CFrame + Vector3.new(0,2,0)
                    firetouchinterest(LocalPlayer.Character.HumanoidRootPart, item, 0)
                    wait(0.1)
                    firetouchinterest(LocalPlayer.Character.HumanoidRootPart, item, 1)
                end
                wait(0.3)
            end
        end
        wait(1)
    end
end

-- Fly Mode Example
local flySpeed = 50
local function flyLoop()
    local char = LocalPlayer.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return end
    local hrp = char.HumanoidRootPart
    local flying = true
    local bodyVel = Instance.new("BodyVelocity", hrp)
    bodyVel.MaxForce = Vector3.new(1e5,1e5,1e5)
    bodyVel.Velocity = Vector3.new(0,0,0)
    RunService.RenderStepped:Connect(function()
        if flyEnabled then
            bodyVel.Velocity = hrp.CFrame.lookVector * flySpeed
        else
            flying = false
            bodyVel:Destroy()
        end
    end)
end

-- Teleport Example
local function teleportTo(locationCFrame)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        LocalPlayer.Character.HumanoidRootPart.CFrame = locationCFrame
    end
end

-- Buttons for features
createButton("Toggle Auto Farm", 50, function()
    autoFarmEnabled = not autoFarmEnabled
    if autoFarmEnabled then
        autoFarmLoop()
    end
end)

createButton("Toggle Fly Mode", 100, function()
    flyEnabled = not flyEnabled
    if flyEnabled then
        flyLoop()
    end
end)

createButton("Teleport to Volcano Island", 150, function()
    -- Example CFrame, replace with actual location
    teleportTo(CFrame.new(500, 50, 1000))
end)

createButton("Teleport to Mainland", 200, function()
    teleportTo(CFrame.new(0, 50, 0))
end)

-- Info Button
createButton("Credits & Info", 300, function()
    Title.Text = "Wild Horse Islands Script\nUpdated for 2025!"
end)

-- Pastebin Fetch Example (Requires executor with HTTP request support)
-- Example: loadstring(game:HttpGet("https://pastebin.com/raw/yourcode"))()
-- For security, always validate and use trusted sources.

-- End of Script
print("Wild Horse Islands Script Loaded. Enjoy!")
