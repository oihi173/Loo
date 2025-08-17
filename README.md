-- LocalScript dentro de StarterGui

-- Criar ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Criar Frame (painel)
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 200, 0, 100)
frame.Position = UDim2.new(0.3, 0, 0.2, 0)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.Active = true
frame.Draggable = true
frame.Parent = screenGui

-- Botão Ativar/Desativar ESP
local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(0, 180, 0, 30)
toggleBtn.Position = UDim2.new(0, 10, 0, 10)
toggleBtn.Text = "Ativar ESP"
toggleBtn.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
toggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleBtn.Parent = frame

-- Botão Fechar Painel
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 180, 0, 30)
closeBtn.Position = UDim2.new(0, 10, 0, 50)
closeBtn.Text = "Fechar Painel"
closeBtn.BackgroundColor3 = Color3.fromRGB(150, 50, 50)
closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
closeBtn.Parent = frame

-- Variáveis
local espAtivo = false
local horsesESP = {}

-- Função para criar marcador
local function criarESP(horse)
    local highlight = Instance.new("Highlight")
    highlight.FillTransparency = 1
    highlight.OutlineColor = Color3.fromRGB(0, 255, 0)
    highlight.Parent = horse
    horsesESP[horse] = highlight
end

-- Função para ativar/desativar
local function setESP(state)
    espAtivo = state
    if state then
        toggleBtn.Text = "Desativar ESP"
        for _, horse in pairs(workspace:GetChildren()) do
            if horse.Name == "Horse" and not horsesESP[horse] then
                criarESP(horse)
            end
        end
        workspace.ChildAdded:Connect(function(obj)
            if espAtivo and obj.Name == "Horse" then
                criarESP(obj)
            end
        end)
    else
        toggleBtn.Text = "Ativar ESP"
        for horse, esp in pairs(horsesESP) do
            if esp then
                esp:Destroy()
            end
        end
        horsesESP = {}
    end
end

-- Clique no botão toggle
toggleBtn.MouseButton1Click:Connect(function()
    setESP(not espAtivo)
end)

-- Clique no botão fechar
closeBtn.MouseButton1Click:Connect(function()
    frame.Visible = false
end)
