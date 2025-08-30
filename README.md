local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoidRoot = char:WaitForChild("HumanoidRootPart")

-- Criar GUI
local gui = Instance.new("ScreenGui", player.PlayerGui)
gui.Name = "PainelCustom"

-- Botão Abrir (⚪)
local abrirBtn = Instance.new("TextButton", gui)
abrirBtn.Size = UDim2.new(0, 40, 0, 40)
abrirBtn.Position = UDim2.new(0, 10, 0, 200)
abrirBtn.Text = "O"
abrirBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 255)
abrirBtn.TextScaled = true
abrirBtn.Visible = true

-- Painel
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 300, 0, 250)
frame.Position = UDim2.new(0.3, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
frame.Active = true
frame.Draggable = true
frame.Visible = false

-- Botão Fechar (❌)
local fecharBtn = Instance.new("TextButton", frame)
fecharBtn.Size = UDim2.new(0, 30, 0, 30)
fecharBtn.Position = UDim2.new(1, -35, 0, 5)
fecharBtn.Text = "X"
fecharBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
fecharBtn.TextScaled = true

-- Botão Ativar/Desativar Auto-Rebater
local autoBtn = Instance.new("TextButton", frame)
autoBtn.Size = UDim2.new(0, 200, 0, 40)
autoBtn.Position = UDim2.new(0, 50, 0, 50)
autoBtn.Text = "Ativar Auto-Rebater"
autoBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
autoBtn.TextScaled = true

-- Campo de força (lado)
local forcaLabel = Instance.new("TextLabel", frame)
forcaLabel.Size = UDim2.new(0, 200, 0, 25)
forcaLabel.Position = UDim2.new(0, 50, 0, 100)
forcaLabel.Text = "Força Lateral:"
forcaLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
forcaLabel.BackgroundTransparency = 1

local forcaBox = Instance.new("TextBox
