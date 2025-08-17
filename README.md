local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local CollectionService = game:GetService("CollectionService")
local UserInputService = game:GetService("UserInputService")

local localPlayer = Players.LocalPlayer
local cam = workspace.CurrentCamera

-- ====== CONFIG INICIAL ======
local TAG = "Horse"
local MAX_DIST = 150
local SHOW_ENABLED = true
local FILTER_COMUM = true
local FILTER_RARO = true

-- Cores por raridade
local RARITY_COLORS = {
    Comum = Color3.fromRGB(220, 220, 220),
    Raro  = Color3.fromRGB(120, 200, 255),
}

-- ====== UI (Painel) ======
local function makePanel()
    local screen = Instance.new("ScreenGui")
    screen.Name = "HorseMarkersPanel"
    screen.ResetOnSpawn = false
    screen.IgnoreGuiInset = true
    screen.Parent = localPlayer:WaitForChild("PlayerGui")

    local frame = Instance.new("Frame")
    frame.AnchorPoint = Vector2.new(0,0)
    frame.Position = UDim2.new(0, 14, 0, 14)
    frame.Size = UDim2.new(0, 260, 0, 150)
    frame.BackgroundColor3 = Color3.fromRGB(24, 24, 28)
    frame.BorderSizePixel = 0
    frame.BackgroundTransparency = 0.15
    frame.Parent = screen

    local uiCorner = Instance.new("UICorner", frame)
    uiCorner.CornerRadius = UDim.new(0, 12)
    local pad = Instance.new("UIPadding", frame)
    pad.PaddingTop = UDim.new(0, 10)
    pad.PaddingBottom = UDim.new(0, 10)
    pad.PaddingLeft = UDim.new(0, 10)
    pad.PaddingRight = UDim.new(0, 10)

    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 22)
    title.BackgroundTransparency = 1
    title.Text = "Cavalos (marcadores seguros)"
    title.Font = Enum.Font.GothamSemibold
    title.TextSize = 16
    title.TextXAlignment = Enum.TextXAlignment.Left
    title.TextColor3 = Color3.fromRGB(255,255,255)
    title.Parent = frame

    local toggle = Instance.new("TextButton")
    toggle.Position = UDim2.new(0, 0, 0, 34)
    toggle.Size = UDim2.new(0, 120, 0, 28)
    toggle.Text = "Ativado: ON"
    toggle.Font = Enum.Font.GothamSemibold
    toggle.TextSize = 14
    toggle.BackgroundColor3 = Color3.fromRGB(34, 180, 90)
    toggle.TextColor3 = Color3.new(1,1,1)
    toggle.AutoButtonColor = true
    toggle.Parent = frame
    Instance.new("UICorner", toggle).CornerRadius = UDim.new(0, 8)

    local distLabel = Instance.new("TextLabel")
    distLabel.Position = UDim2.new(0, 0, 0, 70)
    distLabel.Size = UDim2.new(0, 120, 0, 22)
    distLabel.BackgroundTransparency = 1
    distLabel.Font = Enum.Font.Gotham
    distLabel.TextSize = 14
    distLabel.TextXAlignment = Enum.TextXAlignment.Left
    distLabel.TextColor3 = Color3.fromRGB(220,220,220)
    distLabel.Text = ("Distância: %d"):format(MAX_DIST)
    distLabel.Parent = frame

    local minus = Instance.new("TextButton")
    minus.Position = UDim2.new(0, 0, 0, 96)
    minus.Size = UDim2.new(0, 36, 0, 28)
    minus.Text = "-10"
    minus.Font = Enum.Font.GothamSemibold
    minus.TextSize = 14
    minus.BackgroundColor3 = Color3.fromRGB(60, 60, 68)
    minus.TextColor3 = Color3.new(1,1,1)
    minus.AutoButtonColor = true
    minus.Parent = frame
    Instance.new("UICorner", minus).CornerRadius = UDim.new(0, 8)

    local plus = Instance.new("TextButton")
    plus.Position = UDim2.new(0, 44, 0, 96)
    plus.Size = UDim2.new(0, 36, 0, 28)
    plus.Text = "+10"
    plus.Font = Enum.Font.GothamSemibold
    plus.TextSize = 14
    plus.BackgroundColor3 = Color3.fromRGB(60, 60, 68)
    plus.TextColor3 = Color3.new(1,1,1)
    plus.AutoButtonColor = true
    plus.Parent = frame
    Instance.new("UICorner", plus).CornerRadius = UDim.new(0, 8)

    local reset = Instance.new("TextButton")
    reset.Position = UDim2.new(0, 88, 0, 96)
    reset.Size = UDim2.new(0, 56, 0, 28)
    reset.Text = "Reset"
    reset.Font = Enum.Font.GothamSemibold
    reset.TextSize = 14
    reset.BackgroundColor3 = Color3.fromRGB(60, 60, 68)
    reset.TextColor3 = Color3.new(1,1,1)
    reset.AutoButtonColor = true
    reset.Parent = frame
    Instance.new("UICorner", reset).CornerRadius = UDim.new(0, 8)

    local comumChk = Instance.new("TextButton")
    comumChk.Position = UDim2.new(0, 140, 0, 34)
    comumChk.Size = UDim2.new(0, 110, 0, 28)
    comumChk.Text = "Comum: ON"
    comumChk.Font = Enum.Font.GothamSemibold
    comumChk.TextSize = 14
    comumChk.BackgroundColor3 = Color3.fromRGB(80, 80, 88)
    comumChk.TextColor3 = Color3.new(1,1,1)
    comumChk.AutoButtonColor = true
    comumChk.Parent = frame
    Instance.new("UICorner", comumChk).CornerRadius = UDim.new(0, 8)

    local raroChk = Instance.new("TextButton")
    raroChk.Position = UDim2.new(0, 140, 0, 70)
    raroChk.Size = UDim2.new(0, 110, 0, 28)
    raroChk.Text = "Raro: ON"
    raroChk.Font = Enum.Font.GothamSemibold
    raroChk.TextSize = 14
    raroChk.BackgroundColor3 = Color3.fromRGB(80, 80, 88)
    raroChk.TextColor3 = Color3.new(1,1,1)
    raroChk.AutoButtonColor = true
    raroChk.Parent = frame
    Instance.new("UICorner", raroChk).CornerRadius = UDim.new(0, 8)

    -- ações
    toggle.MouseButton1Click:Connect(function()
        SHOW_ENABLED = not SHOW_ENABLED
        toggle.Text = SHOW_ENABLED and "Ativado: ON" or "Ativado: OFF"
        toggle.BackgroundColor3 = SHOW_ENABLED and Color3.fromRGB(34, 180, 90) or Color3.fromRGB(150, 60, 60)
    end)

    minus.MouseButton1Click:Connect(function()
        MAX_DIST = math.clamp(MAX_DIST - 10, 20, 500)
        distLabel.Text = ("Distância: %d"):format(MAX_DIST)
    end)
    plus.MouseButton1Click:Connect(function()
        MAX_DIST = math.clamp(MAX_DIST + 10, 20, 500)
        distLabel.Text = ("Distância: %d"):format(MAX_DIST)
    end)
    reset.MouseButton1Click:Connect(function()
        MAX_DIST = 150
        distLabel.Text = "Distância: 150"
    end)

    comumChk.MouseButton1Click:Connect(function()
        FILTER_COMUM = not FILTER_COMUM
        comumChk.Text = FILTER_COMUM and "Comum: ON" or "Comum: OFF"
    end)
    raroChk.MouseButton1Click:Connect(function()
        FILTER_RARO = not FILTER_RARO
        raroChk.Text = FILTER_RARO and "Raro: ON" or "Raro: OFF"
    end)

    -- mover o painel (arrastar)
    local dragging, dragStart, startPos
    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then dragging = false end
            end)
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            local delta = input.Position - dragStart
            frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)

    return screen
end

-- ====== Billboard ======
local function createBillboard(text, color)
    local gui = Instance.new("BillboardGui")
    gui.Size = UDim2.new(0, 140, 0, 30)
    gui.AlwaysOnTop = true
    gui.StudsOffset = Vector3.new(0, 3, 0)

    local holder = Instance.new("Frame")
    holder.Size = UDim2.fromScale(1,1)
    holder.BackgroundColor3 = Color3.fromRGB(20,20,24)
    holder.BackgroundTransparency = 0.35
    holder.BorderSizePixel = 0
    holder.Parent = gui
    local corner = Instance.new("UICorner", holder)
    corner.CornerRadius = UDim.new(0, 8)

    local label = Instance.new("TextLabel")
    label.BackgroundTransparency = 1
    label.Size = UDim2.fromScale(1,1)
    label.TextScaled = true
    label.Font = Enum.Font.GothamSemibold
    label.Text = text
    label.TextColor3 = color or Color3.new(1,1,1)
    label.Parent = holder

    return gui
end

local function getAnchor(model)
    if model.PrimaryPart then return model.PrimaryPart end
    for _,d in ipairs(model:GetDescendants()) do
        if d:IsA("BasePart") then return d end
    end
end

-- ====== Gerenciamento ======
local markers = {} -- model -> {gui=BillboardGui, rarity=string}

local function rarityOf(model)
    local r = nil
    if model:GetAttribute("Rarity") then
        r = tostring(model:GetAttribute("Rarity"))
    else
        -- fallback: tenta por nome
        local name = string.lower(model.Name)
        if string.find(name, "raro") then r = "Raro" end
        if string.find(name, "comum") then r = "Comum" end
    end
    return r or "Comum"
end

local function allowedByFilter(r)
    if r == "Raro" then return FILTER_RARO end
    if r == "Comum" then return FILTER_COMUM end
    return true
end

local function attachMarker(model)
    if markers[model] then return end
    local part = getAnchor(model)
    if not part then return end

    local r = rarityOf(model)
    local color = RARITY_COLORS[r] or Color3.fromRGB(255,255,255)
    local text = string.format("Cavalo • %s", r)

    local gui = createBillboard(text, color)
    gui.Adornee = part
    gui.Parent = part

    markers[model] = {gui = gui, rarity = r}
end

local function detachMarker(model)
    local entry = markers[model]
    if entry then
        if entry.gui then entry.gui:Destroy() end
        markers[model] = nil
    end
end

-- Inicializa painel
makePanel()

-- Conecta cavalos já existentes
for _,m in ipairs(CollectionService:GetTagged(TAG)) do
    attachMarker(m)
end
CollectionService:GetInstanceAddedSignal(TAG):Connect(attachMarker)
CollectionService:GetInstanceRemovedSignal(TAG):Connect(detachMarker)

-- ====== Atualização por frame (distância + linha de visão + filtros + toggle) ======
RunService.RenderStepped:Connect(function()
    local char = localPlayer.Character
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    if not hrp then return end

    for model, entry in pairs(markers) do
        if not model.Parent then
            detachMarker(model)
        else
            local gui = entry.gui
            local part = gui and gui.Adornee
            if gui and part then
                if not SHOW_ENABLED then
                    gui.Enabled = false
                else
                    local r = entry.rarity
                    local passFilter = allowedByFilter(r)
                    if not passFilter then
                        gui.Enabled = false
                    else
                        local dist = (part.Position - hrp.Position).Magnitude
                        if dist <= MAX_DIST then
                            -- Raycast da câmera até o cavalo, ignorando o próprio cavalo e o player
                            local params = RaycastParams.new()
                            params.FilterType = Enum.RaycastFilterType.Blacklist
                            params.FilterDescendantsInstances = {model, char}
                            local dir = (part.Position - cam.CFrame.Position)
                            local result = workspace:Raycast(cam.CFrame.Position, dir, params)
                            local visible = (result == nil) or result.Instance:IsDescendantOf(model)
                            gui.Enabled = visible
                        else
                            gui.Enabled = false
                        end
                    end
                end
            end
        end
    end
end)
