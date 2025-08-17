local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui")
gui.Name = "GamePanel"
gui.Parent = player.PlayerGui

-- Função de notificação
local function showNotification(message)
    local notif = Instance.new("TextLabel")
    notif.Size = UDim2.new(0, 300, 0, 50)
    notif.Position = UDim2.new(0.5, -150, 0.1, 0)
    notif.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    notif.TextColor3 = Color3.new(1, 1, 1)
    notif.Font = Enum.Font.SourceSansBold
    notif.TextSize = 22
    notif.Text = message
    notif.Parent = gui
    notif.ZIndex = 10
    notif.BackgroundTransparency = 0.2
    notif.BorderSizePixel = 0
    notif.Visible = true
    wait(2)
    notif:Destroy()
end

-- Painel principal
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 400, 0, 340)
frame.Position = UDim2.new(0.5, -200, 0.5, -170)
frame.BackgroundColor3 = Color3.fromRGB(55, 55, 55)
frame.BorderSizePixel = 0
frame.Parent = gui
frame.Visible = true

-- Botão abrir/fechar
local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(0, 100, 0, 35)
toggleBtn.Position = UDim2.new(1, -110, 0, 10)
toggleBtn.Text = "Fechar Painel"
toggleBtn.BackgroundColor3 = Color3.fromRGB(75,75,75)
toggleBtn.TextColor3 = Color3.new(1,1,1)
toggleBtn.Parent = frame

toggleBtn.MouseButton1Click:Connect(function()
    frame.Visible = not frame.Visible
    toggleBtn.Text = frame.Visible and "Fechar Painel" or "Abrir Painel"
end)

-- Mover painel para os lados
local moveLeft = Instance.new("TextButton")
moveLeft.Size = UDim2.new(0, 40, 0, 35)
moveLeft.Position = UDim2.new(0, 10, 1, -45)
moveLeft.Text = "<"
moveLeft.BackgroundColor3 = Color3.fromRGB(75,75,75)
moveLeft.TextColor3 = Color3.new(1,1,1)
moveLeft.Parent = frame

local moveRight = Instance.new("TextButton")
moveRight.Size = UDim2.new(0, 40, 0, 35)
moveRight.Position = UDim2.new(0, 60, 1, -45)
moveRight.Text = ">"
moveRight.BackgroundColor3 = Color3.fromRGB(75,75,75)
moveRight.TextColor3 = Color3.new(1,1,1)
moveRight.Parent = frame

moveLeft.MouseButton1Click:Connect(function()
    frame.Position = frame.Position + UDim2.new(-0.1, 0, 0, 0)
end)
moveRight.MouseButton1Click:Connect(function()
    frame.Position = frame.Position + UDim2.new(0.1, 0, 0, 0)
end)

-- Lista de jogos
local games = {"Murder Mystery", "Wild Horse Islands", "Blade Ball"}
local selectedGame = nil

local gameLabel = Instance.new("TextLabel")
gameLabel.Size = UDim2.new(0, 380, 0, 30)
gameLabel.Position = UDim2.new(0, 10, 0, 60)
gameLabel.BackgroundTransparency = 1
gameLabel.Text = "Selecione o jogo:"
gameLabel.TextColor3 = Color3.new(1,1,1)
gameLabel.Font = Enum.Font.SourceSansBold
gameLabel.TextSize = 22
gameLabel.Parent = frame

local gameDropdown = Instance.new("TextButton")
gameDropdown.Size = UDim2.new(0, 380, 0, 30)
gameDropdown.Position = UDim2.new(0, 10, 0, 100)
gameDropdown.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
gameDropdown.TextColor3 = Color3.new(1,1,1)
gameDropdown.Font = Enum.Font.SourceSans
gameDropdown.TextSize = 20
gameDropdown.Text = "Murder Mystery"
gameDropdown.Parent = frame

selectedGame = games[1]

local function updateGameContent()
    -- Limpa conteúdos antigos
    for _, v in pairs(frame:GetChildren()) do
        if v.Name == "GameOption" then
            v:Destroy()
        end
    end

    if selectedGame == "Murder Mystery" then
        local roles = {
            {name="Xerife", color=Color3.fromRGB(60, 122, 255)},
            {name="Assassino", color=Color3.fromRGB(255, 60, 80)},
            {name="Inocente", color=Color3.fromRGB(60, 255, 80)},
            {name="Inocente com arma", color=Color3.fromRGB(255, 255, 80)}
        }
        for i, role in ipairs(roles) do
            local btn = Instance.new("TextButton")
            btn.Name = "GameOption"
            btn.Size = UDim2.new(0, 180, 0, 30)
            btn.Position = UDim2.new(0, 10 + (i-1)*190, 0, 140)
            btn.BackgroundColor3 = role.color
            btn.Text = role.name
            btn.TextColor3 = Color3.new(0,0,0)
            btn.Parent = frame
            btn.MouseButton1Click:Connect(function()
                showNotification("Função " .. role.name .. " ativada!")
            end)
        end

        -- UnESP, Noclip, UnNoclip
        local optNames = {"UnESP", "Noclip", "UnNoclip"}
        for i, opt in ipairs(optNames) do
            local btn = Instance.new("TextButton")
            btn.Name = "GameOption"
            btn.Size = UDim2.new(0, 120, 0, 30)
            btn.Position = UDim2.new(0, 10 + (i-1)*130, 0, 190)
            btn.BackgroundColor3 = Color3.fromRGB(220,220,220)
            btn.Text = opt
            btn.TextColor3 = Color3.new(0,0,0)
            btn.Parent = frame
            btn.MouseButton1Click:Connect(function()
                showNotification("Função " .. opt .. " desativada!")
            end)
        end

    elseif selectedGame == "Wild Horse Islands" then
        local horseBtn = Instance.new("TextButton")
        horseBtn.Name = "GameOption"
        horseBtn.Size = UDim2.new(0, 180, 0, 30)
        horseBtn.Position = UDim2.new(0, 10, 0, 140)
        horseBtn.BackgroundColor3 = Color3.fromRGB(255, 220, 100)
        horseBtn.TextColor3 = Color3.new(0,0,0)
        horseBtn.Text = "ESP Nós Cavalo"
        horseBtn.Parent = frame
        horseBtn.MouseButton1Click:Connect(function()
            showNotification("Função ESP Nós Cavalo ativada!")
        end)

        local npcBtn = Instance.new("TextButton")
        npcBtn.Name = "GameOption"
        npcBtn.Size = UDim2.new(0, 180, 0, 30)
        npcBtn.Position = UDim2.new(0, 210, 0, 140)
        npcBtn.BackgroundColor3 = Color3.fromRGB(100, 200, 255)
        npcBtn.TextColor3 = Color3.new(0,0,0)
        npcBtn.Text = "ESP Nós NPC"
        npcBtn.Parent = frame
        npcBtn.MouseButton1Click:Connect(function()
            showNotification("Função ESP Nós NPC ativada!")
        end)

    elseif selectedGame == "Blade Ball" then
        local autoBtn = Instance.new("TextButton")
        autoBtn.Name = "GameOption"
        autoBtn.Size = UDim2.new(0, 380, 0, 30)
        autoBtn.Position = UDim2.new(0, 10, 0, 140)
        autoBtn.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
        autoBtn.TextColor3 = Color3.new(1,1,1)
        autoBtn.Text = "Auto Reparte a Bola"
        autoBtn.Parent = frame
        autoBtn.MouseButton1Click:Connect(function()
            showNotification("Função Auto Reparte ativada! Aguarde 7 segundos.")
            wait(7)
            showNotification("Script carregado!")
        end)
    end
end

gameDropdown.MouseButton1Click:Connect(function()
    -- Troca de jogo (simples, alterna entre os jogos)
    local idx = table.find(games, selectedGame)
    idx = idx % #games + 1
    selectedGame = games[idx]
    gameDropdown.Text = selectedGame
    updateGameContent()
end)

updateGameContent()
