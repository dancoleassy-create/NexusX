#NexusX

local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "DeltaMenu"
gui.ResetOnSpawn = false

local UIS = game:GetService("UserInputService")
local runService = game:GetService("RunService")
local vim = game:GetService("VirtualInputManager")
local teleportService = game:GetService("TeleportService")
local tweenService = game:GetService("TweenService")

local colors = {
    bg = Color3.fromRGB(10, 10, 10),
    accent = Color3.fromRGB(20, 20, 20),
    button = Color3.fromRGB(25, 25, 25),
    buttonHover = Color3.fromRGB(35, 35, 35),
    text = Color3.fromRGB(210, 210, 210),
    toggleOn = Color3.fromRGB(60, 120, 60),
    toggleOff = Color3.fromRGB(50, 50, 50)
}

local settings = {
    menuOpen = true,
    fullbright = false,
    optimization = false,
    fps = false,
    minimap = false,
    minimapNicknames = true,
    minimapDistance = true,
    minimapZoom = 0.5,
    minimapRotate = false,
    minimapTrail = false,
    minimapDanger = false,
    fly = false,
    noclip = false,
    speed = 16,
    targetPlayer = false,
    hitbox = false,
    hitboxSize = 5,
    esp = false,
    espPlayers = true,
    espNick = true,
    espDist = true,
    espNpc = false,
    aimbot = false,
    aimbotPlayers = true,
    aimbotNpc = false,
    wallCheck = true,
    autoShoot = false,
    aimbotSpeed = 5,
    aimbotPart = "Head",
    aimbotFov = 100,
    showFovCircle = false,
    antiAfk = false,
    markerPos = nil,
    autoTeleport = false,
    selectedPlayer = "",
    openBtnPos = {0.5, -19, 0.5, -19},
    fpsPos = {0, 10, 0, 10},
    minimapPos = {1, -130, 0, 10},
    menuPos = {0.5, -300, 0.5, -150}
}

local dataStore = nil
local function loadSettings()
    local success, result = pcall(function()
        if not player:FindFirstChild("DeltaSettings") then
            local ds = Instance.new("Folder")
            ds.Name = "DeltaSettings"
            ds.Parent = player
        end
        return player.DeltaSettings
    end)
    if success and result then
        dataStore = result
        for name, value in pairs(settings) do
            local stored = dataStore:FindFirstChild(name)
            if stored then
                if name == "markerPos" then
                    local x = dataStore:FindFirstChild("markerX")
                    local y = dataStore:FindFirstChild("markerY")
                    local z = dataStore:FindFirstChild("markerZ")
                    if x and y and z then
                        settings.markerPos = Vector3.new(x.Value, y.Value, z.Value)
                    end
                elseif name == "minimapPos" then
                    local x = dataStore:FindFirstChild("minimapX")
                    local y = dataStore:FindFirstChild("minimapY")
                    local xs = dataStore:FindFirstChild("minimapXS")
                    local ys = dataStore:FindFirstChild("minimapYS")
                    if x and y and xs and ys then
                        settings.minimapPos = {x.Value, xs.Value, y.Value, ys.Value}
                    end
                else
                    settings[name] = stored.Value
                end
            end
        end
    end
end

local function saveSetting(name, value)
    settings[name] = value
    if dataStore then
        if name == "markerPos" then
            if value then
                local x = dataStore:FindFirstChild("markerX") or Instance.new("NumberValue", dataStore)
                x.Name = "markerX"; x.Value = value.X
                local y = dataStore:FindFirstChild("markerY") or Instance.new("NumberValue", dataStore)
                y.Name = "markerY"; y.Value = value.Y
                local z = dataStore:FindFirstChild("markerZ") or Instance.new("NumberValue", dataStore)
                z.Name = "markerZ"; z.Value = value.Z
            else
                for _, n in ipairs({"markerX","markerY","markerZ"}) do
                    local v = dataStore:FindFirstChild(n)
                    if v then v:Destroy() end
                end
            end
        elseif name == "minimapPos" then
            local x = dataStore:FindFirstChild("minimapX") or Instance.new("NumberValue", dataStore)
            x.Name = "minimapX"; x.Value = value[1]
            local xs = dataStore:FindFirstChild("minimapXS") or Instance.new("NumberValue", dataStore)
            xs.Name = "minimapXS"; xs.Value = value[2]
            local y = dataStore:FindFirstChild("minimapY") or Instance.new("NumberValue", dataStore)
            y.Name = "minimapY"; y.Value = value[3]
            local ys = dataStore:FindFirstChild("minimapYS") or Instance.new("NumberValue", dataStore)
            ys.Name = "minimapYS"; ys.Value = value[4]
        else
            local stored = dataStore:FindFirstChild(name)
            if not stored then
                if typeof(value) == "string" then
                    stored = Instance.new("StringValue")
                elseif typeof(value) == "number" then
                    stored = Instance.new("NumberValue")
                else
                    stored = Instance.new("BoolValue")
                end
                stored.Name = name
                stored.Parent = dataStore
            end
            stored.Value = value
        end
    end
end

loadSettings()

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 600, 0, 300)
main.Position = UDim2.new(settings.menuPos[1], settings.menuPos[2], settings.menuPos[3], settings.menuPos[4])
main.BackgroundColor3 = colors.bg
main.BorderSizePixel = 0
main.Visible = settings.menuOpen
main.ClipsDescendants = true
main.Name = "MainFrame"
Instance.new("UICorner", main).CornerRadius = UDim.new(0, 8)

local topBar = Instance.new("Frame", main)
topBar.Size = UDim2.new(1, 0, 0, 35)
topBar.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
topBar.BorderSizePixel = 0
topBar.Name = "TopBar"
Instance.new("UICorner", topBar).CornerRadius = UDim.new(0, 8)

local titleLabel = Instance.new("TextLabel", topBar)
titleLabel.Size = UDim2.new(0, 100, 1, 0)
titleLabel.Position = UDim2.new(0, 15, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "DELTA"
titleLabel.TextColor3 = colors.text
titleLabel.TextSize = 18
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextXAlignment = Enum.TextXAlignment.Left

local closeBtn = Instance.new("TextButton", topBar)
closeBtn.Size = UDim2.new(0, 24, 0, 24)
closeBtn.Position = UDim2.new(1, -30, 0.5, -12)
closeBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
closeBtn.BorderSizePixel = 0
closeBtn.Text = "X"
closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
closeBtn.TextSize = 14
closeBtn.Font = Enum.Font.GothamBold
closeBtn.Name = "CloseBtn"
closeBtn.AutoButtonColor = false
Instance.new("UICorner", closeBtn).CornerRadius = UDim.new(0, 6)

closeBtn.MouseEnter:Connect(function()
    tweenService:Create(closeBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(200, 60, 60)}):Play()
end)
closeBtn.MouseLeave:Connect(function()
    tweenService:Create(closeBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(180, 50, 50)}):Play()
end)

local sideBar = Instance.new("Frame", main)
sideBar.Size = UDim2.new(0, 110, 1, -35)
sideBar.Position = UDim2.new(0, 0, 0, 35)
sideBar.BackgroundColor3 = Color3.fromRGB(12, 12, 12)
sideBar.BorderSizePixel = 0
sideBar.Name = "SideBar"
Instance.new("UICorner", sideBar).CornerRadius = UDim.new(0, 8)

local contentFrame = Instance.new("Frame", main)
contentFrame.Size = UDim2.new(1, -110, 1, -35)
contentFrame.Position = UDim2.new(0, 110, 0, 35)
contentFrame.BackgroundColor3 = colors.accent
contentFrame.BorderSizePixel = 0
contentFrame.Name = "ContentFrame"
Instance.new("UICorner", contentFrame).CornerRadius = UDim.new(0, 8)

local openBtn = Instance.new("TextButton", gui)
openBtn.Size = UDim2.new(0, 38, 0, 38)
openBtn.Position = UDim2.new(settings.openBtnPos[1], settings.openBtnPos[2], settings.openBtnPos[3], settings.openBtnPos[4])
openBtn.BackgroundColor3 = colors.bg
openBtn.BorderSizePixel = 0
openBtn.Text = "Δ"
openBtn.TextColor3 = colors.text
openBtn.TextSize = 22
openBtn.Font = Enum.Font.GothamBold
openBtn.Visible = not settings.menuOpen
openBtn.ZIndex = 10
openBtn.Name = "OpenBtn"
openBtn.AutoButtonColor = false
Instance.new("UICorner", openBtn).CornerRadius = UDim.new(1, 0)

local openBtnDragging = false
local openBtnDragStart = nil
local openBtnStartPos = nil
local openBtnHasMoved = false

openBtn.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        openBtnDragging = true
        openBtnHasMoved = false
        openBtnDragStart = input.Position
        openBtnStartPos = openBtn.Position
    end
end)

UIS.InputChanged:Connect(function(input)
    if not openBtnDragging then return end
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        local delta = input.Position - openBtnDragStart
        if math.abs(delta.X) > 3 or math.abs(delta.Y) > 3 then
            openBtnHasMoved = true
        end
        local newX = openBtnStartPos.X.Offset + delta.X
        local newY = openBtnStartPos.Y.Offset + delta.Y
        local screenSize = workspace.CurrentCamera.ViewportSize
        newX = math.clamp(newX, 0, screenSize.X - openBtn.AbsoluteSize.X)
        newY = math.clamp(newY, 0, screenSize.Y - openBtn.AbsoluteSize.Y)
        openBtn.Position = UDim2.new(0, newX, 0, newY)
    end
end)

UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        openBtnDragging = false
        saveSetting("openBtnPos", {openBtn.Position.X.Scale, openBtn.Position.X.Offset, openBtn.Position.Y.Scale, openBtn.Position.Y.Offset})
    end
end)

openBtn.Activated:Connect(function()
    if not openBtnHasMoved then
        toggleMenu()
    end
end)

local tabs = {}
local contents = {}
local tabNames = {"RENDER", "ESP", "MOVEMENT", "COMBAT", "TELEPORT", "SETTING"}

local function createToggle(parent, text, posY, defaultState, callback, settingName)
    local toggleFrame = Instance.new("Frame", parent)
    toggleFrame.Size = UDim2.new(1, -20, 0, 30)
    toggleFrame.Position = UDim2.new(0, 10, 0, posY)
    toggleFrame.BackgroundTransparency = 1

    local label = Instance.new("TextLabel", toggleFrame)
    label.Size = UDim2.new(0.7, 0, 1, 0)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = colors.text
    label.TextSize = 14
    label.Font = Enum.Font.Gotham
    label.TextXAlignment = Enum.TextXAlignment.Left

    local toggle = Instance.new("TextButton", toggleFrame)
    toggle.Size = UDim2.new(0, 40, 0, 20)
    toggle.Position = UDim2.new(1, -40, 0.5, -10)
    toggle.BackgroundColor3 = defaultState and colors.toggleOn or colors.toggleOff
    toggle.BorderSizePixel = 0
    toggle.Text = ""
    Instance.new("UICorner", toggle).CornerRadius = UDim.new(1, 0)

    local dot = Instance.new("Frame", toggle)
    dot.Size = UDim2.new(0, 16, 0, 16)
    dot.Position = defaultState and UDim2.new(1, -18, 0.5, -8) or UDim2.new(0, 2, 0.5, -8)
    dot.BackgroundColor3 = colors.text
    dot.BorderSizePixel = 0
    Instance.new("UICorner", dot).CornerRadius = UDim.new(1, 0)

    local enabled = defaultState or false
    if defaultState and callback then
        callback(true)
    end

    local function switch()
        enabled = not enabled
        if enabled then
            toggle.BackgroundColor3 = colors.toggleOn
            tweenService:Create(dot, TweenInfo.new(0.15, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = UDim2.new(1, -18, 0.5, -8)}):Play()
        else
            toggle.BackgroundColor3 = colors.toggleOff
            tweenService:Create(dot, TweenInfo.new(0.15, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = UDim2.new(0, 2, 0.5, -8)}):Play()
        end
        if settingName then saveSetting(settingName, enabled) end
        if callback then callback(enabled) end
    end

    toggle.Activated:Connect(switch)

    return toggle
end

local function createSlider(parent, text, posY, min, max, default, callback, settingName)
    local sliderFrame = Instance.new("Frame", parent)
    sliderFrame.Size = UDim2.new(1, -20, 0, 50)
    sliderFrame.Position = UDim2.new(0, 10, 0, posY)
    sliderFrame.BackgroundTransparency = 1

    local label = Instance.new("TextLabel", sliderFrame)
    label.Size = UDim2.new(1, 0, 0, 20)
    label.BackgroundTransparency = 1
    label.Text = text .. ": " .. default
    label.TextColor3 = colors.text
    label.TextSize = 14
    label.Font = Enum.Font.Gotham
    label.TextXAlignment = Enum.TextXAlignment.Left

    local sliderBg = Instance.new("Frame", sliderFrame)
    sliderBg.Size = UDim2.new(1, 0, 0, 8)
    sliderBg.Position = UDim2.new(0, 0, 0, 28)
    sliderBg.BackgroundColor3 = colors.button
    sliderBg.BorderSizePixel = 0
    Instance.new("UICorner", sliderBg).CornerRadius = UDim.new(1, 0)

    local fill = Instance.new("Frame", sliderBg)
    local percent = (default - min) / (max - min)
    fill.Size = UDim2.new(percent, 0, 1, 0)
    fill.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    fill.BorderSizePixel = 0
    Instance.new("UICorner", fill).CornerRadius = UDim.new(1, 0)

    local dragBtn = Instance.new("TextButton", sliderBg)
    dragBtn.Size = UDim2.new(0, 18, 0, 18)
    dragBtn.Position = UDim2.new(percent, -9, 0.5, -9)
    dragBtn.BackgroundColor3 = colors.text
    dragBtn.BorderSizePixel = 0
    dragBtn.Text = ""
    dragBtn.AutoButtonColor = false
    Instance.new("UICorner", dragBtn).CornerRadius = UDim.new(1, 0)

    local function updateSlider()
        local mousePos = UIS:GetMouseLocation()
        local guiPos = sliderBg.AbsolutePosition
        local guiSize = sliderBg.AbsoluteSize
        local relativeX = math.clamp((mousePos.X - guiPos.X) / guiSize.X, 0, 1)
        local value = math.floor(min + (max - min) * relativeX + 0.5)
        label.Text = text .. ": " .. value
        tweenService:Create(fill, TweenInfo.new(0.1), {Size = UDim2.new(relativeX, 0, 1, 0)}):Play()
        tweenService:Create(dragBtn, TweenInfo.new(0.1), {Position = UDim2.new(relativeX, -9, 0.5, -9)}):Play()
        if settingName then saveSetting(settingName, value) end
        if callback then callback(value) end
    end

    local function onSliderStart()
        updateSlider()
        local connection
        connection = runService.RenderStepped:Connect(function()
            if not UIS:IsMouseButtonPressed(Enum.UserInputType.MouseButton1) then
                connection:Disconnect()
                return
            end
            updateSlider()
        end)
    end

    dragBtn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            onSliderStart()
        end
    end)

    sliderBg.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            onSliderStart()
        end
    end)
end

local function addButtonStroke(button, color, thickness)
    local stroke = Instance.new("UIStroke", button)
    stroke.Color = color or Color3.fromRGB(100, 100, 100)
    stroke.Thickness = thickness or 1
    stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    stroke.LineJoinMode = Enum.LineJoinMode.Round
end

for i, name in ipairs(tabNames) do
    local tabBtn = Instance.new("TextButton", sideBar)
    tabBtn.Size = UDim2.new(1, -10, 0, 26)
    tabBtn.Position = UDim2.new(0, 5, 0, 8 + (i - 1) * 31)
    tabBtn.BackgroundColor3 = colors.button
    tabBtn.BorderSizePixel = 0
    tabBtn.Text = name
    tabBtn.TextColor3 = colors.text
    tabBtn.TextSize = 12
    tabBtn.Font = Enum.Font.GothamSemibold
    Instance.new("UICorner", tabBtn).CornerRadius = UDim.new(0, 6)
    addButtonStroke(tabBtn, Color3.fromRGB(80, 80, 80), 1)

    local content = Instance.new("ScrollingFrame", contentFrame)
    content.Size = UDim2.new(1, 0, 1, 0)
    content.BackgroundTransparency = 1
    content.ScrollBarThickness = 4
    content.ScrollBarImageColor3 = Color3.fromRGB(40, 40, 40)
    content.CanvasSize = UDim2.new(0, 0, 0, 500)
    content.Visible = false
    content.BorderSizePixel = 0
    content.ScrollingEnabled = true

    tabs[name] = tabBtn
    contents[name] = content

    tabBtn.MouseEnter:Connect(function()
        tweenService:Create(tabBtn, TweenInfo.new(0.15), {BackgroundColor3 = colors.buttonHover}):Play()
    end)
    tabBtn.MouseLeave:Connect(function()
        if contents[name].Visible == false then
            tweenService:Create(tabBtn, TweenInfo.new(0.15), {BackgroundColor3 = colors.button}):Play()
        end
    end)

    if name == "TELEPORT" then
        local titleLabel = Instance.new("TextLabel", content)
        titleLabel.Size = UDim2.new(1, -20, 0, 20)
        titleLabel.Position = UDim2.new(0, 10, 0, 10)
        titleLabel.BackgroundTransparency = 1
        titleLabel.Text = "Teleport"
        titleLabel.TextColor3 = colors.text
        titleLabel.TextSize = 14
        titleLabel.Font = Enum.Font.GothamBold
        titleLabel.TextXAlignment = Enum.TextXAlignment.Left

        local markerSectionLabel = Instance.new("TextLabel", content)
        markerSectionLabel.Size = UDim2.new(1, -20, 0, 18)
        markerSectionLabel.Position = UDim2.new(0, 10, 0, 35)
        markerSectionLabel.BackgroundTransparency = 1
        markerSectionLabel.Text = "Marker Teleport"
        markerSectionLabel.TextColor3 = Color3.fromRGB(180, 180, 180)
        markerSectionLabel.TextSize = 12
        markerSectionLabel.Font = Enum.Font.Gotham

        local markerInfoLabel = Instance.new("TextLabel", content)
        markerInfoLabel.Size = UDim2.new(1, -20, 0, 20)
        markerInfoLabel.Position = UDim2.new(0, 10, 0, 55)
        markerInfoLabel.BackgroundTransparency = 1
        markerInfoLabel.TextColor3 = colors.text
        markerInfoLabel.TextSize = 13
        markerInfoLabel.Font = Enum.Font.Gotham
        markerInfoLabel.TextXAlignment = Enum.TextXAlignment.Left

        local btnRow = Instance.new("Frame", content)
        btnRow.Size = UDim2.new(1, -20, 0, 35)
        btnRow.Position = UDim2.new(0, 10, 0, 80)
        btnRow.BackgroundTransparency = 1

        local setMarkerBtn = Instance.new("TextButton", btnRow)
        setMarkerBtn.Size = UDim2.new(0, 120, 1, 0)
        setMarkerBtn.Position = UDim2.new(0, 0, 0, 0)
        setMarkerBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        setMarkerBtn.BorderSizePixel = 0
        setMarkerBtn.Text = "SET MARKER"
        setMarkerBtn.TextColor3 = colors.text
        setMarkerBtn.TextSize = 13
        setMarkerBtn.Font = Enum.Font.GothamBold
        Instance.new("UICorner", setMarkerBtn).CornerRadius = UDim.new(0, 5)
        addButtonStroke(setMarkerBtn, Color3.fromRGB(120, 120, 120), 1)
        setMarkerBtn.MouseEnter:Connect(function() tweenService:Create(setMarkerBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(35, 35, 35)}):Play() end)
        setMarkerBtn.MouseLeave:Connect(function() tweenService:Create(setMarkerBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(20, 20, 20)}):Play() end)
        setMarkerBtn.Activated:Connect(function()
            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                settings.markerPos = player.Character.HumanoidRootPart.Position
                saveSetting("markerPos", settings.markerPos)
                updateMarkerLabel()
            end
        end)

        local autoTpBtn = Instance.new("TextButton", btnRow)
        autoTpBtn.Size = UDim2.new(0, 120, 1, 0)
        autoTpBtn.Position = UDim2.new(0.5, -60, 0, 0)
        autoTpBtn.BackgroundColor3 = settings.autoTeleport and Color3.fromRGB(60, 120, 60) or Color3.fromRGB(20, 20, 20)
        autoTpBtn.BorderSizePixel = 0
        autoTpBtn.Text = "AUTO TP"
        autoTpBtn.TextColor3 = colors.text
        autoTpBtn.TextSize = 13
        autoTpBtn.Font = Enum.Font.GothamBold
        Instance.new("UICorner", autoTpBtn).CornerRadius = UDim.new(0, 5)
        addButtonStroke(autoTpBtn, Color3.fromRGB(120, 120, 120), 1)

        local autoTeleportEnabled = settings.autoTeleport
        local autoTpThread = nil

        local function stopAutoTeleport()
            if autoTpThread then
                task.cancel(autoTpThread)
                autoTpThread = nil
            end
            autoTpBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        end

        local function startAutoTeleport()
            stopAutoTeleport()
            autoTpThread = task.spawn(function()
                while autoTeleportEnabled and settings.markerPos do
                    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                        player.Character.HumanoidRootPart.CFrame = CFrame.new(settings.markerPos)
                    end
                    task.wait(1)
                end
                autoTpThread = nil
            end)
        end

        autoTpBtn.MouseEnter:Connect(function()
            if not autoTeleportEnabled then
                tweenService:Create(autoTpBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(35, 35, 35)}):Play()
            else
                tweenService:Create(autoTpBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(80, 140, 80)}):Play()
            end
        end)
        autoTpBtn.MouseLeave:Connect(function()
            if not autoTeleportEnabled then
                tweenService:Create(autoTpBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(20, 20, 20)}):Play()
            else
                tweenService:Create(autoTpBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(60, 120, 60)}):Play()
            end
        end)

        autoTpBtn.Activated:Connect(function()
            if not settings.markerPos then return end
            autoTeleportEnabled = not autoTeleportEnabled
            settings.autoTeleport = autoTeleportEnabled
            saveSetting("autoTeleport", autoTeleportEnabled)
            if autoTeleportEnabled then
                autoTpBtn.BackgroundColor3 = Color3.fromRGB(60, 120, 60)
                startAutoTeleport()
            else
                stopAutoTeleport()
            end
        end)

        if autoTeleportEnabled then
            startAutoTeleport()
        end

        local teleportBtn = Instance.new("TextButton", btnRow)
        teleportBtn.Size = UDim2.new(0, 120, 1, 0)
        teleportBtn.Position = UDim2.new(1, -120, 0, 0)
        teleportBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        teleportBtn.BorderSizePixel = 0
        teleportBtn.Text = "TELEPORT"
        teleportBtn.TextColor3 = colors.text
        teleportBtn.TextSize = 13
        teleportBtn.Font = Enum.Font.GothamBold
        Instance.new("UICorner", teleportBtn).CornerRadius = UDim.new(0, 5)
        addButtonStroke(teleportBtn, Color3.fromRGB(120, 120, 120), 1)
        teleportBtn.MouseEnter:Connect(function() tweenService:Create(teleportBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(35, 35, 35)}):Play() end)
        teleportBtn.MouseLeave:Connect(function() tweenService:Create(teleportBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(20, 20, 20)}):Play() end)
        teleportBtn.Activated:Connect(function()
            if not settings.markerPos then return end
            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                player.Character.HumanoidRootPart.CFrame = CFrame.new(settings.markerPos)
            end
        end)

        local clearBtn = Instance.new("TextButton", content)
        clearBtn.Size = UDim2.new(0, 120, 0, 28)
        clearBtn.Position = UDim2.new(0, 10, 0, 123)
        clearBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        clearBtn.BorderSizePixel = 0
        clearBtn.Text = "CLEAR MARKER"
        clearBtn.TextColor3 = Color3.fromRGB(180, 180, 180)
        clearBtn.TextSize = 12
        clearBtn.Font = Enum.Font.Gotham
        Instance.new("UICorner", clearBtn).CornerRadius = UDim.new(0, 5)
        addButtonStroke(clearBtn, Color3.fromRGB(120, 120, 120), 1)
        clearBtn.MouseEnter:Connect(function() tweenService:Create(clearBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(35, 35, 35)}):Play() end)
        clearBtn.MouseLeave:Connect(function() tweenService:Create(clearBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(20, 20, 20)}):Play() end)
        clearBtn.Activated:Connect(function()
            settings.markerPos = nil
            saveSetting("markerPos", nil)
            updateMarkerLabel()
            if autoTeleportEnabled then
                autoTeleportEnabled = false
                settings.autoTeleport = false
                saveSetting("autoTeleport", false)
                stopAutoTeleport()
            end
        end)

        function updateMarkerLabel()
            if settings.markerPos then
                markerInfoLabel.Text = "Marker: " .. math.floor(settings.markerPos.X) .. ", " .. math.floor(settings.markerPos.Y) .. ", " .. math.floor(settings.markerPos.Z)
            else
                markerInfoLabel.Text = "Marker: not set"
            end
        end
        updateMarkerLabel()

        local playerTpLabel = Instance.new("TextLabel", content)
        playerTpLabel.Size = UDim2.new(1, -20, 0, 18)
        playerTpLabel.Position = UDim2.new(0, 10, 0, 165)
        playerTpLabel.BackgroundTransparency = 1
        playerTpLabel.Text = "Player Teleport"
        playerTpLabel.TextColor3 = Color3.fromRGB(180, 180, 180)
        playerTpLabel.TextSize = 12
        playerTpLabel.Font = Enum.Font.Gotham

        local tpToPlayerBtn = Instance.new("TextButton", content)
        tpToPlayerBtn.Size = UDim2.new(1, -20, 0, 35)
        tpToPlayerBtn.Position = UDim2.new(0, 10, 0, 185)
        tpToPlayerBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        tpToPlayerBtn.BorderSizePixel = 0
        tpToPlayerBtn.Text = "TP TO PLAYER"
        tpToPlayerBtn.TextColor3 = colors.text
        tpToPlayerBtn.TextSize = 14
        tpToPlayerBtn.Font = Enum.Font.GothamBold
        Instance.new("UICorner", tpToPlayerBtn).CornerRadius = UDim.new(0, 5)
        addButtonStroke(tpToPlayerBtn, Color3.fromRGB(120, 120, 120), 1)
        tpToPlayerBtn.MouseEnter:Connect(function() tweenService:Create(tpToPlayerBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(35, 35, 35)}):Play() end)
        tpToPlayerBtn.MouseLeave:Connect(function() tweenService:Create(tpToPlayerBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(20, 20, 20)}):Play() end)
        tpToPlayerBtn.Activated:Connect(function()
            if settings.selectedPlayer == "" then return end
            for _, target in pairs(game.Players:GetPlayers()) do
                if target.Name == settings.selectedPlayer and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
                    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                        player.Character.HumanoidRootPart.CFrame = target.Character.HumanoidRootPart.CFrame + Vector3.new(0, 2, 0)
                    end
                    break
                end
            end
        end)

        local playerDropdown = Instance.new("Frame", content)
        playerDropdown.Size = UDim2.new(1, -20, 0, 30)
        playerDropdown.Position = UDim2.new(0, 10, 0, 230)
        playerDropdown.BackgroundColor3 = colors.button
        playerDropdown.BorderSizePixel = 0
        playerDropdown.ClipsDescendants = true
        Instance.new("UICorner", playerDropdown).CornerRadius = UDim.new(0, 5)
        addButtonStroke(playerDropdown, Color3.fromRGB(120, 120, 120), 1)

        local selectedPlayerBtn = Instance.new("TextButton", playerDropdown)
        selectedPlayerBtn.Size = UDim2.new(1, 0, 0, 30)
        selectedPlayerBtn.BackgroundTransparency = 1
        selectedPlayerBtn.Text = settings.selectedPlayer ~= "" and settings.selectedPlayer or "Choose player..."
        selectedPlayerBtn.TextColor3 = colors.text
        selectedPlayerBtn.TextSize = 13
        selectedPlayerBtn.Font = Enum.Font.Gotham
        selectedPlayerBtn.TextXAlignment = Enum.TextXAlignment.Left
        selectedPlayerBtn.Position = UDim2.new(0, 10, 0, 0)

        local arrowLabel = Instance.new("TextLabel", playerDropdown)
        arrowLabel.Size = UDim2.new(0, 20, 0, 30)
        arrowLabel.Position = UDim2.new(1, -25, 0, 0)
        arrowLabel.BackgroundTransparency = 1
        arrowLabel.Text = "▼"
        arrowLabel.TextColor3 = colors.text
        arrowLabel.TextSize = 12
        arrowLabel.Font = Enum.Font.Gotham

        local playersScroll = Instance.new("ScrollingFrame", playerDropdown)
        playersScroll.Size = UDim2.new(1, 0, 0, 0)
        playersScroll.Position = UDim2.new(0, 0, 0, 30)
        playersScroll.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        playersScroll.BorderSizePixel = 0
        playersScroll.ClipsDescendants = true
        playersScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
        playersScroll.ScrollBarThickness = 4
        playersScroll.ScrollBarImageColor3 = Color3.fromRGB(40, 40, 40)
        playersScroll.ScrollingEnabled = true

        local playersList = Instance.new("Frame", playersScroll)
        playersList.Size = UDim2.new(1, 0, 0, 0)
        playersList.BackgroundTransparency = 1
        playersList.BorderSizePixel = 0
        Instance.new("UIListLayout", playersList).SortOrder = Enum.SortOrder.Name

        local dropdownOpen = false
        local playerButtons = {}

        local function updatePlayerList()
            for _, b in ipairs(playerButtons) do
                b:Destroy()
            end
            playerButtons = {}

            local ySize = 0
            for _, target in pairs(game.Players:GetPlayers()) do
                if target ~= player then
                    local pBtn = Instance.new("TextButton", playersList)
                    pBtn.Size = UDim2.new(1, 0, 0, 28)
                    pBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
                    pBtn.BorderSizePixel = 0
                    pBtn.Text = target.Name
                    pBtn.TextColor3 = colors.text
                    pBtn.TextSize = 12
                    pBtn.Font = Enum.Font.Gotham
                    pBtn.TextXAlignment = Enum.TextXAlignment.Left
                    pBtn.Name = target.Name

                    pBtn.MouseEnter:Connect(function() tweenService:Create(pBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(40, 40, 40)}):Play() end)
                    pBtn.MouseLeave:Connect(function() tweenService:Create(pBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(20, 20, 20)}):Play() end)

                    local targetName = target.Name
                    pBtn.Activated:Connect(function()
                        settings.selectedPlayer = targetName
                        selectedPlayerBtn.Text = targetName
                        saveSetting("selectedPlayer", targetName)
                        dropdownOpen = false
                        playerDropdown.Size = UDim2.new(1, -20, 0, 30)
                        playersScroll.Size = UDim2.new(1, 0, 0, 0)
                    end)

                    table.insert(playerButtons, pBtn)
                    ySize = ySize + 28
                end
            end
            playersList.Size = UDim2.new(1, 0, 0, ySize)
            playersScroll.CanvasSize = UDim2.new(0, 0, 0, ySize)
            if ySize == 0 then
                local noPlayersLabel = Instance.new("TextLabel", playersList)
                noPlayersLabel.Size = UDim2.new(1, 0, 0, 28)
                noPlayersLabel.BackgroundTransparency = 1
                noPlayersLabel.Text = "No players"
                noPlayersLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
                noPlayersLabel.TextSize = 12
                noPlayersLabel.Font = Enum.Font.Gotham
                noPlayersLabel.TextXAlignment = Enum.TextXAlignment.Left
                table.insert(playerButtons, noPlayersLabel)
            end
        end

        updatePlayerList()

        selectedPlayerBtn.Activated:Connect(function()
            dropdownOpen = not dropdownOpen
            if dropdownOpen then
                playerDropdown.Size = UDim2.new(1, -20, 0, 200)
                playersScroll.Size = UDim2.new(1, 0, 0, 170)
                playersScroll.CanvasSize = UDim2.new(0, 0, 0, playersList.AbsoluteSize.Y)
            else
                playerDropdown.Size = UDim2.new(1, -20, 0, 30)
                playersScroll.Size = UDim2.new(1, 0, 0, 0)
            end
        end)

        content.CanvasSize = UDim2.new(0, 0, 0, 500)

        local function refreshTab()
            updatePlayerList()
            updateMarkerLabel()
            if settings.selectedPlayer ~= "" then
                local found = false
                for _, target in pairs(game.Players:GetPlayers()) do
                    if target.Name == settings.selectedPlayer then found = true break end
                end
                if not found then
                    settings.selectedPlayer = ""
                    saveSetting("selectedPlayer", "")
                    selectedPlayerBtn.Text = "Choose player..."
                else
                    selectedPlayerBtn.Text = settings.selectedPlayer
                end
            end
        end

        game.Players.PlayerAdded:Connect(function()
            if contents["TELEPORT"].Visible then refreshTab() end
        end)
        game.Players.PlayerRemoving:Connect(function()
            if contents["TELEPORT"].Visible then refreshTab() end
        end)

        local oldSelectTab = selectTab
        selectTab = function()
            oldSelectTab()
            if name == "TELEPORT" then refreshTab() end
        end
    end

    if name == "SETTING" then
        local saveConfigBtn = Instance.new("TextButton", content)
        saveConfigBtn.Size = UDim2.new(0, 120, 0, 35)
        saveConfigBtn.Position = UDim2.new(0, 10, 0, 80)
        saveConfigBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        saveConfigBtn.BorderSizePixel = 0
        saveConfigBtn.Text = "SAVE CONFIG"
        saveConfigBtn.TextColor3 = colors.text
        saveConfigBtn.TextSize = 12
        saveConfigBtn.Font = Enum.Font.GothamBold
        Instance.new("UICorner", saveConfigBtn).CornerRadius = UDim.new(0, 5)
        addButtonStroke(saveConfigBtn, Color3.fromRGB(120, 120, 120), 1)
        saveConfigBtn.MouseEnter:Connect(function() tweenService:Create(saveConfigBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(35, 35, 35)}):Play() end)
        saveConfigBtn.MouseLeave:Connect(function() tweenService:Create(saveConfigBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(20, 20, 20)}):Play() end)
        saveConfigBtn.Activated:Connect(function()
            for name, value in pairs(settings) do
                saveSetting(name, value)
            end
        end)

        local rejoinBtn = Instance.new("TextButton", content)
        rejoinBtn.Size = UDim2.new(0, 120, 0, 35)
        rejoinBtn.Position = UDim2.new(0, 10, 0, 130)
        rejoinBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        rejoinBtn.BorderSizePixel = 0
        rejoinBtn.Text = "REJOIN"
        rejoinBtn.TextColor3 = colors.text
        rejoinBtn.TextSize = 14
        rejoinBtn.Font = Enum.Font.GothamBold
        Instance.new("UICorner", rejoinBtn).CornerRadius = UDim.new(0, 5)
        addButtonStroke(rejoinBtn, Color3.fromRGB(120, 120, 120), 1)
        rejoinBtn.MouseEnter:Connect(function() tweenService:Create(rejoinBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(35, 35, 35)}):Play() end)
        rejoinBtn.MouseLeave:Connect(function() tweenService:Create(rejoinBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(20, 20, 20)}):Play() end)
        rejoinBtn.Activated:Connect(function()
            teleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId)
        end)

        createToggle(content, "Anti AFK", 180, settings.antiAfk, function(enabled)
            if enabled then
                local vim = game:GetService("VirtualUser")
                spawn(function()
                    while settings.antiAfk do
                        vim:CaptureController()
                        vim:ClickButton2(Vector2.new())
                        task.wait(120)
                    end
                end)
            end
        end, "antiAfk")
    end

    local function selectTab()
        for _, c in pairs(contents) do
            c.Visible = false
        end
        for _, b in pairs(tabs) do
            tweenService:Create(b, TweenInfo.new(0.15), {BackgroundColor3 = colors.button}):Play()
        end
        content.Visible = true
        tweenService:Create(tabBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(45, 45, 45)}):Play()
        if name == "TELEPORT" and refreshTab then
            refreshTab()
        end
    end

    tabBtn.Activated:Connect(selectTab)
end

local fullbrightEnabled = settings.fullbright
local fullbrightConnection = nil

createToggle(contents["RENDER"], "Fullbright", 15, settings.fullbright, function(enabled)
    fullbrightEnabled = enabled
    if enabled then
        if fullbrightConnection then fullbrightConnection:Disconnect() end
        game.Lighting.Brightness = 2
        game.Lighting.ClockTime = 14
        game.Lighting.FogEnd = 999999
        game.Lighting.GlobalShadows = false
        game.Lighting.OutdoorAmbient = Color3.fromRGB(255, 255, 255)
        game.Lighting.Ambient = Color3.fromRGB(255, 255, 255)
        game.Lighting.ExposureCompensation = 0.5
        fullbrightConnection = runService.Heartbeat:Connect(function()
            if not fullbrightEnabled then fullbrightConnection:Disconnect(); return end
            game.Lighting.Brightness = 2
            game.Lighting.ClockTime = 14
            game.Lighting.FogEnd = 999999
        end)
    else
        if fullbrightConnection then fullbrightConnection:Disconnect(); fullbrightConnection = nil end
        game.Lighting.Brightness = 1
        game.Lighting.OutdoorAmbient = Color3.fromRGB(127, 127, 127)
        game.Lighting.Ambient = Color3.fromRGB(0, 0, 0)
        game.Lighting.ExposureCompensation = 0
    end
end, "fullbright")

createToggle(contents["RENDER"], "Optimization", 50, settings.optimization, function(enabled)
    if enabled then
        game.Lighting.GlobalShadows = false
        game.Lighting.ShadowSoftness = 0
    else
        game.Lighting.GlobalShadows = true
        game.Lighting.ShadowSoftness = 0.5
    end
end, "optimization")

local fpsOverlay = Instance.new("TextLabel", gui)
fpsOverlay.Size = UDim2.new(0, 80, 0, 25)
fpsOverlay.Position = UDim2.new(settings.fpsPos[1], settings.fpsPos[2], settings.fpsPos[3], settings.fpsPos[4])
fpsOverlay.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
fpsOverlay.BackgroundTransparency = 0.5
fpsOverlay.BorderSizePixel = 0
fpsOverlay.Text = "FPS: 0"
fpsOverlay.TextColor3 = Color3.fromRGB(255, 255, 255)
fpsOverlay.TextSize = 14
fpsOverlay.Font = Enum.Font.GothamBold
fpsOverlay.TextXAlignment = Enum.TextXAlignment.Center
fpsOverlay.Visible = settings.fps
fpsOverlay.ZIndex = 10
Instance.new("UICorner", fpsOverlay).CornerRadius = UDim.new(0, 6)

local fpsDragging = false
local fpsDragStart = nil
local fpsStartPos = nil

fpsOverlay.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        fpsDragging = true
        fpsDragStart = input.Position
        fpsStartPos = fpsOverlay.Position
        fpsOverlay.BackgroundTransparency = 0.3
    end
end)

UIS.InputChanged:Connect(function(input)
    if not fpsDragging then return end
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        local delta = input.Position - fpsDragStart
        local newX = fpsStartPos.X.Offset + delta.X
        local newY = fpsStartPos.Y.Offset + delta.Y
        local screenSize = workspace.CurrentCamera.ViewportSize
        newX = math.clamp(newX, 0, screenSize.X - fpsOverlay.AbsoluteSize.X)
        newY = math.clamp(newY, 0, screenSize.Y - fpsOverlay.AbsoluteSize.Y)
        fpsOverlay.Position = UDim2.new(0, newX, 0, newY)
    end
end)

UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        fpsDragging = false
        fpsOverlay.BackgroundTransparency = 0.5
        saveSetting("fpsPos", {fpsOverlay.Position.X.Scale, fpsOverlay.Position.X.Offset, fpsOverlay.Position.Y.Scale, fpsOverlay.Position.Y.Offset})
    end
end)

local lastFpsUpdate = 0
local fpsValue = 0
local frameCount = 0

runService.RenderStepped:Connect(function(deltaTime)
    frameCount = frameCount + 1
    lastFpsUpdate = lastFpsUpdate + deltaTime
    if lastFpsUpdate >= 1 then
        fpsValue = math.floor(frameCount / lastFpsUpdate + 0.5)
        if fpsOverlay and fpsOverlay.Parent then
            fpsOverlay.Text = "FPS: " .. fpsValue
        end
        frameCount = 0
        lastFpsUpdate = 0
    end
end)

createToggle(contents["RENDER"], "FPS", 85, settings.fps, function(enabled)
    fpsOverlay.Visible = enabled
end, "fps")

local minimapEnabled = settings.minimap
local minimapNicknames = settings.minimapNicknames
local minimapDistance = settings.minimapDistance
local minimapZoom = settings.minimapZoom
local minimapRotate = settings.minimapRotate
local minimapTrail = settings.minimapTrail
local minimapDanger = settings.minimapDanger

local minimapFrame = Instance.new("Frame", gui)
minimapFrame.Size = UDim2.new(0, 150, 0, 150)
minimapFrame.Position = UDim2.new(settings.minimapPos[1], settings.minimapPos[2], settings.minimapPos[3], settings.minimapPos[4])
minimapFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
minimapFrame.BackgroundTransparency = 0.3
minimapFrame.BorderSizePixel = 0
minimapFrame.Visible = settings.minimap
minimapFrame.ZIndex = 100
minimapFrame.ClipsDescendants = true
Instance.new("UICorner", minimapFrame).CornerRadius = UDim.new(1, 0)
local minimapStroke = Instance.new("UIStroke", minimapFrame)
minimapStroke.Color = Color3.fromRGB(100, 100, 100)
minimapStroke.Thickness = 2

local minimapDragging = false
local minimapDragStart = nil
local minimapStartPos = nil

minimapFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        minimapDragging = true
        minimapDragStart = input.Position
        minimapStartPos = minimapFrame.Position
    end
end)

UIS.InputChanged:Connect(function(input)
    if not minimapDragging then return end
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        local delta = input.Position - minimapDragStart
        local newX = minimapStartPos.X.Offset + delta.X
        local newY = minimapStartPos.Y.Offset + delta.Y
        local screenSize = workspace.CurrentCamera.ViewportSize
        newX = math.clamp(newX, 0, screenSize.X - minimapFrame.AbsoluteSize.X)
        newY = math.clamp(newY, 0, screenSize.Y - minimapFrame.AbsoluteSize.Y)
        minimapFrame.Position = UDim2.new(0, newX, 0, newY)
    end
end)

UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        minimapDragging = false
        saveSetting("minimapPos", {minimapFrame.Position.X.Scale, minimapFrame.Position.X.Offset, minimapFrame.Position.Y.Scale, minimapFrame.Position.Y.Offset})
    end
end)

local zoomOutBtn = Instance.new("TextButton", minimapFrame)
zoomOutBtn.Size = UDim2.new(0, 20, 0, 20)
zoomOutBtn.Position = UDim2.new(0, 2, 0, 2)
zoomOutBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
zoomOutBtn.BackgroundTransparency = 0.5
zoomOutBtn.BorderSizePixel = 0
zoomOutBtn.Text = "-"
zoomOutBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
zoomOutBtn.TextSize = 16
zoomOutBtn.Font = Enum.Font.GothamBold
zoomOutBtn.ZIndex = 102
Instance.new("UICorner", zoomOutBtn).CornerRadius = UDim.new(0, 4)

local zoomInBtn = Instance.new("TextButton", minimapFrame)
zoomInBtn.Size = UDim2.new(0, 20, 0, 20)
zoomInBtn.Position = UDim2.new(1, -22, 0, 2)
zoomInBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
zoomInBtn.BackgroundTransparency = 0.5
zoomInBtn.BorderSizePixel = 0
zoomInBtn.Text = "+"
zoomInBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
zoomInBtn.TextSize = 16
zoomInBtn.Font = Enum.Font.GothamBold
zoomInBtn.ZIndex = 102
Instance.new("UICorner", zoomInBtn).CornerRadius = UDim.new(0, 4)

zoomOutBtn.Activated:Connect(function()
    minimapZoom = math.max(0.1, minimapZoom - 0.1)
    settings.minimapZoom = minimapZoom
    saveSetting("minimapZoom", minimapZoom)
end)

zoomInBtn.Activated:Connect(function()
    minimapZoom = math.min(2, minimapZoom + 0.1)
    settings.minimapZoom = minimapZoom
    saveSetting("minimapZoom", minimapZoom)
end)

local nLabel = Instance.new("TextLabel", minimapFrame)
nLabel.Size = UDim2.new(0, 15, 0, 15)
nLabel.Position = UDim2.new(0.5, -7, 0, 2)
nLabel.BackgroundTransparency = 1
nLabel.Text = "N"
nLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
nLabel.TextSize = 14
nLabel.Font = Enum.Font.GothamBold
nLabel.ZIndex = 102

local sLabel = Instance.new("TextLabel", minimapFrame)
sLabel.Size = UDim2.new(0, 15, 0, 15)
sLabel.Position = UDim2.new(0.5, -7, 1, -17)
sLabel.BackgroundTransparency = 1
sLabel.Text = "S"
sLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
sLabel.TextSize = 14
sLabel.Font = Enum.Font.GothamBold
sLabel.ZIndex = 102

local wLabel = Instance.new("TextLabel", minimapFrame)
wLabel.Size = UDim2.new(0, 15, 0, 15)
wLabel.Position = UDim2.new(0, 2, 0.5, -7)
wLabel.BackgroundTransparency = 1
wLabel.Text = "W"
wLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
wLabel.TextSize = 14
wLabel.Font = Enum.Font.GothamBold
wLabel.ZIndex = 102

local eLabel = Instance.new("TextLabel", minimapFrame)
eLabel.Size = UDim2.new(0, 15, 0, 15)
eLabel.Position = UDim2.new(1, -17, 0.5, -7)
eLabel.BackgroundTransparency = 1
eLabel.Text = "E"
eLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
eLabel.TextSize = 14
eLabel.Font = Enum.Font.GothamBold
eLabel.ZIndex = 102

local containerFrame = Instance.new("Frame", minimapFrame)
containerFrame.Size = UDim2.new(1, 0, 1, 0)
containerFrame.BackgroundTransparency = 1
containerFrame.ZIndex = 101

local myDot = Instance.new("Frame", minimapFrame)
myDot.Size = UDim2.new(0, 8, 0, 8)
myDot.Position = UDim2.new(0.5, -4, 0.5, -4)
myDot.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
myDot.BorderSizePixel = 0
myDot.ZIndex = 103
Instance.new("UICorner", myDot).CornerRadius = UDim.new(1, 0)
myDot.Visible = false

local directionLine = Instance.new("Frame", minimapFrame)
directionLine.Size = UDim2.new(0, 15, 0, 2)
directionLine.Position = UDim2.new(0.5, -3, 0.5, -1)
directionLine.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
directionLine.BorderSizePixel = 0
directionLine.ZIndex = 103
directionLine.AnchorPoint = Vector2.new(0, 0.5)
directionLine.Visible = false

local trailDots = {}
local trailPositions = {}
local dangerDots = {}

local function clearTrail()
    for _, dot in pairs(trailDots) do
        dot:Destroy()
    end
    trailDots = {}
    trailPositions = {}
end

local function clearDanger()
    for _, dot in pairs(dangerDots) do
        dot:Destroy()
    end
    dangerDots = {}
end

local function updateDangerZones()
    clearDanger()
    if not minimapDanger then return end
    local dangerZones = {}
    if _G.AI and _G.AI.DangerZones then
        for _, zone in pairs(_G.AI.DangerZones) do
            table.insert(dangerZones, zone.Pos)
        end
    end
    for i, pos in ipairs(dangerZones) do
        local dot = Instance.new("Frame", minimapFrame)
        dot.Size = UDim2.new(0, 12, 0, 12)
        dot.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
        dot.BackgroundTransparency = 0.5
        dot.BorderSizePixel = 0
        dot.ZIndex = 99
        Instance.new("UICorner", dot).CornerRadius = UDim.new(1, 0)
        dot.Visible = false
        dangerDots[i] = dot
    end
end

local minimapPlayerDots = {}
local minimapNpcDots = {}

runService.RenderStepped:Connect(function()
    if not minimapEnabled then
        myDot.Visible = false
        directionLine.Visible = false
        nLabel.Visible = false
        sLabel.Visible = false
        wLabel.Visible = false
        eLabel.Visible = false
        for _, data in pairs(minimapPlayerDots) do
            data.dot.Visible = false
            if data.label then data.label.Visible = false end
        end
        for _, dot in pairs(minimapNpcDots) do
            dot.Visible = false
        end
        for _, dot in pairs(trailDots) do
            dot.Visible = false
        end
        for _, dot in pairs(dangerDots) do
            dot.Visible = false
        end
        return
    end
    
    myDot.Visible = true
    directionLine.Visible = true
    nLabel.Visible = true
    sLabel.Visible = true
    wLabel.Visible = true
    eLabel.Visible = true
    
    if not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then return end
    
    local myPos = player.Character.HumanoidRootPart.Position
    local mapSize = 150
    local cameraRotation = 0
    
    if minimapRotate then
        local lookVector = workspace.CurrentCamera.CFrame.LookVector
        cameraRotation = -math.deg(math.atan2(lookVector.X, lookVector.Z))
        containerFrame.Rotation = cameraRotation
    else
        containerFrame.Rotation = 0
    end
    
    local lookVector = player.Character.HumanoidRootPart.CFrame.LookVector
    local angle = math.atan2(lookVector.X, lookVector.Z)
    directionLine.Rotation = math.deg(angle) + 180
    
    if minimapTrail then
        table.insert(trailPositions, myPos)
        if #trailPositions > 20 then
            table.remove(trailPositions, 1)
        end
        while #trailDots < #trailPositions do
            local dot = Instance.new("Frame", minimapFrame)
            dot.Size = UDim2.new(0, 3, 0, 3)
            dot.BackgroundColor3 = Color3.fromRGB(100, 255, 100)
            dot.BackgroundTransparency = 0.5
            dot.BorderSizePixel = 0
            dot.ZIndex = 98
            Instance.new("UICorner", dot).CornerRadius = UDim.new(1, 0)
            table.insert(trailDots, dot)
        end
        while #trailDots > #trailPositions do
            local dot = table.remove(trailDots)
            dot:Destroy()
        end
        for i, pos in ipairs(trailPositions) do
            local relative = pos - myPos
            local dotX = (relative.X * minimapZoom) + mapSize/2 - 1.5
            local dotZ = (relative.Z * minimapZoom) + mapSize/2 - 1.5
            if minimapRotate then
                local rad = math.rad(cameraRotation)
                local rotatedX = (dotX - mapSize/2) * math.cos(rad) - (dotZ - mapSize/2) * math.sin(rad) + mapSize/2
                local rotatedZ = (dotX - mapSize/2) * math.sin(rad) + (dotZ - mapSize/2) * math.cos(rad) + mapSize/2
                dotX, dotZ = rotatedX, rotatedZ
            end
            dotX = math.clamp(dotX, -3, mapSize)
            dotZ = math.clamp(dotZ, -3, mapSize)
            trailDots[i].Position = UDim2.new(0, dotX, 0, dotZ)
            trailDots[i].Visible = true
            trailDots[i].BackgroundTransparency = 0.5 - (i / #trailPositions) * 0.4
        end
    else
        clearTrail()
    end
    
    if minimapDanger then
        for i, pos in pairs(dangerDots) do
            if dangerZones[i] then
                local relative = dangerZones[i] - myPos
                local dotX = (relative.X * minimapZoom) + mapSize/2 - 6
                local dotZ = (relative.Z * minimapZoom) + mapSize/2 - 6
                if minimapRotate then
                    local rad = math.rad(cameraRotation)
                    local rotatedX = (dotX - mapSize/2) * math.cos(rad) - (dotZ - mapSize/2) * math.sin(rad) + mapSize/2
                    local rotatedZ = (dotX - mapSize/2) * math.sin(rad) + (dotZ - mapSize/2) * math.cos(rad) + mapSize/2
                    dotX, dotZ = rotatedX, rotatedZ
                end
                dotX = math.clamp(dotX, -6, mapSize - 6)
                dotZ = math.clamp(dotZ, -6, mapSize - 6)
                pos.Position = UDim2.new(0, dotX, 0, dotZ)
                pos.Visible = true
            else
                pos.Visible = false
            end
        end
    else
        for _, dot in pairs(dangerDots) do
            dot.Visible = false
        end
    end
    
    for _, target in pairs(game.Players:GetPlayers()) do
        if target ~= player and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
            if not minimapPlayerDots[target.UserId] then
                local dot = Instance.new("Frame", containerFrame)
                dot.Size = UDim2.new(0, 4, 0, 4)
                dot.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                dot.BorderSizePixel = 0
                dot.ZIndex = 101
                Instance.new("UICorner", dot).CornerRadius = UDim.new(1, 0)
                
                local nameLabel = Instance.new("TextLabel", containerFrame)
                nameLabel.Size = UDim2.new(0, 60, 0, 14)
                nameLabel.BackgroundTransparency = 0.5
                nameLabel.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
                nameLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
                nameLabel.TextSize = 10
                nameLabel.Font = Enum.Font.Gotham
                nameLabel.Visible = false
                nameLabel.ZIndex = 102
                Instance.new("UICorner", nameLabel).CornerRadius = UDim.new(0, 3)
                
                minimapPlayerDots[target.UserId] = {dot = dot, label = nameLabel}
            end
            
            local data = minimapPlayerDots[target.UserId]
            local dot = data.dot
            local nameLabel = data.label
            dot.Visible = true
            
            local targetPos = target.Character.HumanoidRootPart.Position
            local relative = targetPos - myPos
            
            local dotX = (relative.X * minimapZoom) + mapSize/2 - 2
            local dotZ = (relative.Z * minimapZoom) + mapSize/2 - 2
            
            if minimapRotate then
                local rad = math.rad(cameraRotation)
                local rotatedX = (dotX - mapSize/2) * math.cos(rad) - (dotZ - mapSize/2) * math.sin(rad) + mapSize/2
                local rotatedZ = (dotX - mapSize/2) * math.sin(rad) + (dotZ - mapSize/2) * math.cos(rad) + mapSize/2
                dotX, dotZ = rotatedX, rotatedZ
            end
            
            dotX = math.clamp(dotX, -4, mapSize)
            dotZ = math.clamp(dotZ, -4, mapSize)
            
            dot.Position = UDim2.new(0, dotX, 0, dotZ)
            
            if minimapNicknames or minimapDistance then
                nameLabel.Visible = true
                local text = ""
                if minimapNicknames then text = text .. target.Name end
                if minimapDistance then
                    local dist = math.floor(relative.Magnitude)
                    if minimapNicknames then text = text .. " | " end
                    text = text .. dist .. "m"
                end
                nameLabel.Text = text
                if dotZ > mapSize/2 then
                    nameLabel.Position = UDim2.new(0, -28, 0, -16)
                else
                    nameLabel.Position = UDim2.new(0, -28, 1, 2)
                end
            else
                nameLabel.Visible = false
            end
        else
            if minimapPlayerDots[target.UserId] then
                minimapPlayerDots[target.UserId].dot.Visible = false
                minimapPlayerDots[target.UserId].label.Visible = false
            end
        end
    end
    
    if espEnabled and espNpcEnabled then
        for _, obj in pairs(workspace:GetDescendants()) do
            if obj:IsA("Model") and obj:FindFirstChild("Humanoid") and obj:FindFirstChild("Head") and obj:FindFirstChild("HumanoidRootPart") then
                local isPlayerCharacter = false
                for _, plr in pairs(game.Players:GetPlayers()) do
                    if plr.Character == obj then isPlayerCharacter = true; break end
                end
                if not isPlayerCharacter then
                    local npcId = obj.Name
                    if not minimapNpcDots[npcId] then
                        local dot = Instance.new("Frame", containerFrame)
                        dot.Size = UDim2.new(0, 4, 0, 4)
                        dot.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
                        dot.BorderSizePixel = 0
                        dot.ZIndex = 101
                        Instance.new("UICorner", dot).CornerRadius = UDim.new(1, 0)
                        minimapNpcDots[npcId] = dot
                    end
                    
                    local dot = minimapNpcDots[npcId]
                    dot.Visible = true
                    
                    local targetPos = obj.HumanoidRootPart.Position
                    local relative = targetPos - myPos
                    
                    local dotX = (relative.X * minimapZoom) + mapSize/2 - 2
                    local dotZ = (relative.Z * minimapZoom) + mapSize/2 - 2
                    
                    if minimapRotate then
                        local rad = math.rad(cameraRotation)
                        local rotatedX = (dotX - mapSize/2) * math.cos(rad) - (dotZ - mapSize/2) * math.sin(rad) + mapSize/2
                        local rotatedZ = (dotX - mapSize/2) * math.sin(rad) + (dotZ - mapSize/2) * math.cos(rad) + mapSize/2
                        dotX, dotZ = rotatedX, rotatedZ
                    end
                    
                    dotX = math.clamp(dotX, -4, mapSize)
                    dotZ = math.clamp(dotZ, -4, mapSize)
                    
                    dot.Position = UDim2.new(0, dotX, 0, dotZ)
                end
            end
        end
    end
end)

createToggle(contents["RENDER"], "Minimap", 120, settings.minimap, function(enabled)
    minimapEnabled = enabled
    minimapFrame.Visible = enabled
end, "minimap")

createToggle(contents["RENDER"], "Minimap Nicknames", 155, settings.minimapNicknames, function(enabled)
    minimapNicknames = enabled
end, "minimapNicknames")

createToggle(contents["RENDER"], "Minimap Distance", 190, settings.minimapDistance, function(enabled)
    minimapDistance = enabled
end, "minimapDistance")

createToggle(contents["RENDER"], "Minimap Rotate", 225, settings.minimapRotate, function(enabled)
    minimapRotate = enabled
end, "minimapRotate")

createToggle(contents["RENDER"], "Minimap Trail", 260, settings.minimapTrail, function(enabled)
    minimapTrail = enabled
    if not enabled then clearTrail() end
end, "minimapTrail")

createToggle(contents["RENDER"], "Minimap Danger", 295, settings.minimapDanger, function(enabled)
    minimapDanger = enabled
    if enabled then updateDangerZones() else clearDanger() end
end, "minimapDanger")

local flyEnabled = settings.fly
local flyConnection = nil
local mobileFlyGui = nil
local mobileInput = { forward = false, back = false, left = false, right = false, up = false, down = false }

local function createMobileFlyUI()
    if mobileFlyGui then mobileFlyGui:Destroy() end
    mobileFlyGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
    mobileFlyGui.Name = "DeltaMobileFly"
    mobileFlyGui.ResetOnSpawn = false
    mobileFlyGui.Enabled = false

    local moveFrame = Instance.new("Frame", mobileFlyGui)
    moveFrame.Size = UDim2.new(0, 150, 0, 150)
    moveFrame.Position = UDim2.new(0, 20, 0.7, -75)
    moveFrame.BackgroundTransparency = 1

    local btnSize = 50

    local function createBtn(name, posX, posY, xDir, yDir)
        local btn = Instance.new("TextButton", moveFrame)
        btn.Size = UDim2.new(0, btnSize, 0, btnSize)
        btn.Position = UDim2.new(0, posX, 0, posY)
        btn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        btn.BackgroundTransparency = 0.5
        btn.BorderSizePixel = 0
        btn.Text = name
        btn.TextColor3 = colors.text
        btn.TextSize = 20
        btn.Font = Enum.Font.GothamBold
        Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)

        btn.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch then
                if xDir == 1 then mobileInput.right = true
                elseif xDir == -1 then mobileInput.left = true
                elseif yDir == 1 then mobileInput.forward = true
                elseif yDir == -1 then mobileInput.back = true
                end
            end
        end)
        btn.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch then
                if xDir == 1 then mobileInput.right = false
                elseif xDir == -1 then mobileInput.left = false
                elseif yDir == 1 then mobileInput.forward = false
                elseif yDir == -1 then mobileInput.back = false
                end
            end
        end)
        return btn
    end

    createBtn("W", 50, 0, 0, 1)
    createBtn("S", 50, 100, 0, -1)
    createBtn("A", 0, 50, -1, 0)
    createBtn("D", 100, 50, 1, 0)

    local upBtn = Instance.new("TextButton", mobileFlyGui)
    upBtn.Size = UDim2.new(0, 60, 0, 60)
    upBtn.Position = UDim2.new(1, -80, 0.7, -80)
    upBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    upBtn.BackgroundTransparency = 0.5
    upBtn.BorderSizePixel = 0
    upBtn.Text = "▲"
    upBtn.TextColor3 = colors.text
    upBtn.TextSize = 24
    upBtn.Font = Enum.Font.GothamBold
    Instance.new("UICorner", upBtn).CornerRadius = UDim.new(0, 8)

    local downBtn = Instance.new("TextButton", mobileFlyGui)
    downBtn.Size = UDim2.new(0, 60, 0, 60)
    downBtn.Position = UDim2.new(1, -80, 0.7, 0)
    downBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    downBtn.BackgroundTransparency = 0.5
    downBtn.BorderSizePixel = 0
    downBtn.Text = "▼"
    downBtn.TextColor3 = colors.text
    downBtn.TextSize = 24
    downBtn.Font = Enum.Font.GothamBold
    Instance.new("UICorner", downBtn).CornerRadius = UDim.new(0, 8)

    upBtn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch then mobileInput.up = true end
    end)
    upBtn.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch then mobileInput.up = false end
    end)
    downBtn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch then mobileInput.down = true end
    end)
    downBtn.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch then mobileInput.down = false end
    end)
end

local function setupFly(character)
    if not character or not character:FindFirstChild("HumanoidRootPart") then return end
    local root = character.HumanoidRootPart
    
    local bodyGyro = root:FindFirstChild("FlyGyro")
    if not bodyGyro then
        bodyGyro = Instance.new("BodyGyro")
        bodyGyro.Name = "FlyGyro"
        bodyGyro.MaxTorque = Vector3.new(400000, 400000, 400000)
        bodyGyro.Parent = root
    end
    
    local bodyVelocity = root:FindFirstChild("FlyVelocity")
    if not bodyVelocity then
        bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Name = "FlyVelocity"
        bodyVelocity.MaxForce = Vector3.new(400000, 400000, 400000)
        bodyVelocity.Parent = root
    end
    
    bodyGyro.CFrame = workspace.CurrentCamera.CFrame
    bodyVelocity.Velocity = Vector3.new(0, 0, 0)
    return bodyGyro, bodyVelocity
end

local function cleanupFly()
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local root = player.Character.HumanoidRootPart
        for _, v in pairs(root:GetChildren()) do
            if v:IsA("BodyVelocity") and v.Name == "FlyVelocity" then v:Destroy() end
            if v:IsA("BodyGyro") and v.Name == "FlyGyro" then v:Destroy() end
        end
    end
    if flyConnection then flyConnection:Disconnect(); flyConnection = nil end
    if mobileFlyGui then mobileFlyGui.Enabled = false end
end

createToggle(contents["MOVEMENT"], "Fly", 15, settings.fly, function(enabled)
    flyEnabled = enabled
    if enabled then
        if UIS.TouchEnabled and not mobileFlyGui then createMobileFlyUI() end
        if mobileFlyGui then mobileFlyGui.Enabled = true end
        if player.Character then
            local bodyGyro, bodyVelocity = setupFly(player.Character)
            if flyConnection then flyConnection:Disconnect() end
            flyConnection = runService.Heartbeat:Connect(function()
                if not flyEnabled then cleanupFly(); return end
                if not bodyVelocity or not bodyVelocity.Parent or not bodyGyro or not bodyGyro.Parent then return end
                bodyGyro.CFrame = workspace.CurrentCamera.CFrame
                local moveVector = Vector3.zero
                if UIS.TouchEnabled then
                    if mobileInput.forward then moveVector += workspace.CurrentCamera.CFrame.LookVector end
                    if mobileInput.back then moveVector -= workspace.CurrentCamera.CFrame.LookVector end
                    if mobileInput.left then moveVector -= workspace.CurrentCamera.CFrame.RightVector end
                    if mobileInput.right then moveVector += workspace.CurrentCamera.CFrame.RightVector end
                    if mobileInput.up then moveVector += Vector3.new(0, 1, 0) end
                    if mobileInput.down then moveVector -= Vector3.new(0, 1, 0) end
                else
                    if UIS:IsKeyDown(Enum.KeyCode.W) then moveVector += workspace.CurrentCamera.CFrame.LookVector end
                    if UIS:IsKeyDown(Enum.KeyCode.S) then moveVector -= workspace.CurrentCamera.CFrame.LookVector end
                    if UIS:IsKeyDown(Enum.KeyCode.A) then moveVector -= workspace.CurrentCamera.CFrame.RightVector end
                    if UIS:IsKeyDown(Enum.KeyCode.D) then moveVector += workspace.CurrentCamera.CFrame.RightVector end
                    if UIS:IsKeyDown(Enum.KeyCode.Space) then moveVector += Vector3.new(0, 1, 0) end
                    if UIS:IsKeyDown(Enum.KeyCode.LeftShift) then moveVector -= Vector3.new(0, 1, 0) end
                end
                bodyVelocity.Velocity = moveVector * 50
            end)
        end
    else
        cleanupFly()
    end
end, "fly")

player.CharacterAdded:Connect(function(character)
    local humanoid = character:WaitForChild("Humanoid")
    humanoid.WalkSpeed = settings.speed
    if flyEnabled then
        task.wait(0.1)
        cleanupFly()
        if UIS.TouchEnabled and not mobileFlyGui then createMobileFlyUI() end
        if mobileFlyGui then mobileFlyGui.Enabled = true end
        local bodyGyro, bodyVelocity = setupFly(character)
        if flyConnection then flyConnection:Disconnect() end
        flyConnection = runService.Heartbeat:Connect(function()
            if not flyEnabled then cleanupFly(); return end
            if not bodyVelocity or not bodyVelocity.Parent or not bodyGyro or not bodyGyro.Parent then return end
            bodyGyro.CFrame = workspace.CurrentCamera.CFrame
            local moveVector = Vector3.zero
            if UIS.TouchEnabled then
                if mobileInput.forward then moveVector += workspace.CurrentCamera.CFrame.LookVector end
                if mobileInput.back then moveVector -= workspace.CurrentCamera.CFrame.LookVector end
                if mobileInput.left then moveVector -= workspace.CurrentCamera.CFrame.RightVector end
                if mobileInput.right then moveVector += workspace.CurrentCamera.CFrame.RightVector end
                if mobileInput.up then moveVector += Vector3.new(0, 1, 0) end
                if mobileInput.down then moveVector -= Vector3.new(0, 1, 0) end
            else
                if UIS:IsKeyDown(Enum.KeyCode.W) then moveVector += workspace.CurrentCamera.CFrame.LookVector end
                if UIS:IsKeyDown(Enum.KeyCode.S) then moveVector -= workspace.CurrentCamera.CFrame.LookVector end
                if UIS:IsKeyDown(Enum.KeyCode.A) then moveVector -= workspace.CurrentCamera.CFrame.RightVector end
                if UIS:IsKeyDown(Enum.KeyCode.D) then moveVector += workspace.CurrentCamera.CFrame.RightVector end
                if UIS:IsKeyDown(Enum.KeyCode.Space) then moveVector += Vector3.new(0, 1, 0) end
                if UIS:IsKeyDown(Enum.KeyCode.LeftShift) then moveVector -= Vector3.new(0, 1, 0) end
            end
            bodyVelocity.Velocity = moveVector * 50
        end)
    end
    if noclipEnabled then applyNoclip() end
end)

local noclipEnabled = settings.noclip
local noclipConnection = nil

local function applyNoclip()
    if player.Character then
        for _, part in pairs(player.Character:GetDescendants()) do
            if part:IsA("BasePart") then part.CanCollide = false end
        end
    end
end

local function removeNoclip()
    if player.Character then
        for _, part in pairs(player.Character:GetDescendants()) do
            if part:IsA("BasePart") then part.CanCollide = true end
        end
    end
end

createToggle(contents["MOVEMENT"], "Noclip", 50, settings.noclip, function(enabled)
    noclipEnabled = enabled
    if enabled then
        applyNoclip()
        if noclipConnection then noclipConnection:Disconnect() end
        noclipConnection = runService.Stepped:Connect(function()
            if noclipEnabled then applyNoclip() end
        end)
    else
        if noclipConnection then noclipConnection:Disconnect(); noclipConnection = nil end
        removeNoclip()
    end
end, "noclip")

createSlider(contents["MOVEMENT"], "Speed", 85, 16, 200, settings.speed, function(value)
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.WalkSpeed = value
    end
end, "speed")

local targetPlayerEnabled = settings.targetPlayer
local targetPlayerConnection = nil

local function findNearestPlayer()
    if not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then return nil end
    local myPos = player.Character.HumanoidRootPart.Position
    local nearest = nil
    local minDist = math.huge
    for _, target in pairs(game.Players:GetPlayers()) do
        if target ~= player and target.Character and target.Character:FindFirstChild("HumanoidRootPart") and target.Character:FindFirstChild("Humanoid") and target.Character.Humanoid.Health > 0 then
            local dist = (target.Character.HumanoidRootPart.Position - myPos).Magnitude
            if dist < minDist then minDist = dist; nearest = target end
        end
    end
    return nearest
end

createToggle(contents["MOVEMENT"], "Target Player", 120, settings.targetPlayer, function(enabled)
    targetPlayerEnabled = enabled
    if enabled then
        if targetPlayerConnection then targetPlayerConnection:Disconnect() end
        targetPlayerConnection = runService.Heartbeat:Connect(function()
            if not targetPlayerEnabled then if targetPlayerConnection then targetPlayerConnection:Disconnect() end; return end
            local target = findNearestPlayer()
            if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
                local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
                if humanoid then humanoid:MoveTo(target.Character.HumanoidRootPart.Position) end
            end
        end)
    else
        if targetPlayerConnection then targetPlayerConnection:Disconnect(); targetPlayerConnection = nil end
    end
end, "targetPlayer")

local hitboxEnabled = settings.hitbox
local hitboxSize = settings.hitboxSize
local hitboxParts = {}

local function applyHitbox()
    local sizeMultiplier = hitboxSize / 5
    for _, target in pairs(game.Players:GetPlayers()) do
        if target ~= player and target.Character then
            local head = target.Character:FindFirstChild("Head")
            if head then
                local headHitbox = head:FindFirstChild("DeltaHitbox_Head")
                if not headHitbox then
                    headHitbox = Instance.new("Part")
                    headHitbox.Name = "DeltaHitbox_Head"
                    headHitbox.Size = Vector3.new(2, 2, 2)
                    headHitbox.Transparency = 1
                    headHitbox.CanCollide = true
                    headHitbox.Anchored = false
                    headHitbox.Massless = true
                    headHitbox.Parent = target.Character
                    local weld = Instance.new("WeldConstraint")
                    weld.Part0 = headHitbox; weld.Part1 = head; weld.Parent = headHitbox
                end
                headHitbox.Size = Vector3.new(2 * sizeMultiplier, 2 * sizeMultiplier, 2 * sizeMultiplier)
                table.insert(hitboxParts, headHitbox)
            end
            local torso = target.Character:FindFirstChild("UpperTorso") or target.Character:FindFirstChild("Torso")
            if torso then
                local torsoHitbox = torso:FindFirstChild("DeltaHitbox_Torso")
                if not torsoHitbox then
                    torsoHitbox = Instance.new("Part")
                    torsoHitbox.Name = "DeltaHitbox_Torso"
                    torsoHitbox.Size = Vector3.new(2, 2, 2)
                    torsoHitbox.Transparency = 1
                    torsoHitbox.CanCollide = true
                    torsoHitbox.Anchored = false
                    torsoHitbox.Massless = true
                    torsoHitbox.Parent = target.Character
                    local weld = Instance.new("WeldConstraint")
                    weld.Part0 = torsoHitbox; weld.Part1 = torso; weld.Parent = torsoHitbox
                end
                torsoHitbox.Size = Vector3.new(2 * sizeMultiplier, 2 * sizeMultiplier, 2 * sizeMultiplier)
                table.insert(hitboxParts, torsoHitbox)
            end
        end
    end
end

local function removeHitbox()
    for _, part in ipairs(hitboxParts) do
        if part and part.Parent then part:Destroy() end
    end
    hitboxParts = {}
end

createToggle(contents["MOVEMENT"], "Hitbox", 155, settings.hitbox, function(enabled)
    hitboxEnabled = enabled
    if enabled then applyHitbox() else removeHitbox() end
end, "hitbox")

createSlider(contents["MOVEMENT"], "Hitbox Size", 190, 1, 10, settings.hitboxSize, function(value)
    hitboxSize = value
    if hitboxEnabled then removeHitbox(); applyHitbox() end
end, "hitboxSize")

local espEnabled = settings.esp
local espPlayersEnabled = settings.espPlayers
local espNickEnabled = settings.espNick
local espDistEnabled = settings.espDist
local espNpcEnabled = settings.espNpc

local playerEspObjects = {}
local playerEspConnections = {}
local npcEspObjects = {}
local npcEspConnections = {}

local function clearESP()
    for _, obj in pairs(playerEspObjects) do if obj then pcall(function() obj:Destroy() end) end end
    playerEspObjects = {}
    for _, conn in pairs(playerEspConnections) do if conn then pcall(function() conn:Disconnect() end) end end
    playerEspConnections = {}
    for _, obj in pairs(npcEspObjects) do if obj then pcall(function() obj:Destroy() end) end end
    npcEspObjects = {}
    for _, conn in pairs(npcEspConnections) do if conn then pcall(function() conn:Disconnect() end) end end
    npcEspConnections = {}
end

local function createESPforPlayer(target)
    if not espEnabled or not espPlayersEnabled then return end
    if target == player then return end
    if not target.Character then return end

    local function setupESP()
        local character = target.Character
        local head = character:FindFirstChild("Head")
        if not head then return end

        local billboard = Instance.new("BillboardGui")
        billboard.Adornee = head
        billboard.Size = UDim2.new(0, 200, 0, 60)
        billboard.StudsOffset = Vector3.new(0, 2.5, 0)
        billboard.AlwaysOnTop = true
        billboard.Parent = head

        local info = Instance.new("TextLabel", billboard)
        info.Size = UDim2.new(1, 0, 1, 0)
        info.BackgroundTransparency = 1
        info.TextColor3 = Color3.fromRGB(255, 255, 255)
        info.TextSize = 14
        info.Font = Enum.Font.GothamSemibold
        info.TextStrokeTransparency = 0.3

        local highlight = Instance.new("Highlight")
        highlight.Adornee = character
        highlight.FillTransparency = 1
        highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
        highlight.OutlineTransparency = 0
        highlight.Parent = character

        table.insert(playerEspObjects, billboard)
        table.insert(playerEspObjects, highlight)

        local function updateESP()
            if not billboard or not billboard.Parent then return end
            local text = ""
            if espNickEnabled then text = text .. target.Name .. "\n" end
            if espDistEnabled and player.Character and player.Character:FindFirstChild("Head") then
                local dist = math.floor((player.Character.Head.Position - head.Position).Magnitude + 0.5)
                text = text .. dist .. "m"
            end
            info.Text = text
        end

        local updateConnection = runService.RenderStepped:Connect(function()
            if not espEnabled or not espPlayersEnabled then updateConnection:Disconnect(); return end
            updateESP()
        end)
        table.insert(playerEspConnections, updateConnection)
    end

    if target.Character:FindFirstChild("Head") then setupESP() end
    local charConn = target.CharacterAdded:Connect(function()
        task.wait(0.5)
        setupESP()
    end)
    table.insert(playerEspConnections, charConn)
end

local function createESPforNPC(npcModel)
    if not espNpcEnabled or not espEnabled then return end
    if not npcModel:FindFirstChild("Humanoid") or not npcModel:FindFirstChild("Head") then return end

    local highlight = Instance.new("Highlight")
    highlight.Adornee = npcModel
    highlight.FillTransparency = 1
    highlight.OutlineColor = Color3.fromRGB(255, 0, 0)
    highlight.OutlineTransparency = 0
    highlight.Parent = npcModel

    table.insert(npcEspObjects, highlight)
    local updateConnection = runService.RenderStepped:Connect(function()
        if not espEnabled or not espNpcEnabled then updateConnection:Disconnect(); return end
    end)
    table.insert(npcEspConnections, updateConnection)
end

local function refreshAllESP()
    for _, obj in pairs(playerEspObjects) do if obj then pcall(function() obj:Destroy() end) end end
    playerEspObjects = {}
    for _, conn in pairs(playerEspConnections) do if conn then pcall(function() conn:Disconnect() end) end end
    playerEspConnections = {}
    for _, obj in pairs(npcEspObjects) do if obj then pcall(function() obj:Destroy() end) end end
    npcEspObjects = {}
    for _, conn in pairs(npcEspConnections) do if conn then pcall(function() conn:Disconnect() end) end end
    npcEspConnections = {}

    if not espEnabled then return end

    if espPlayersEnabled then
        for _, target in pairs(game.Players:GetPlayers()) do
            if target ~= player then createESPforPlayer(target) end
        end
    end

    if espNpcEnabled then
        for _, obj in pairs(workspace:GetDescendants()) do
            if obj:IsA("Model") and obj:FindFirstChild("Humanoid") and obj:FindFirstChild("Head") then
                local isPlayerCharacter = false
                for _, plr in pairs(game.Players:GetPlayers()) do
                    if plr.Character == obj then isPlayerCharacter = true; break end
                end
                if not isPlayerCharacter then createESPforNPC(obj) end
            end
        end
    end
end

createToggle(contents["ESP"], "ESP", 15, settings.esp, function(enabled)
    espEnabled = enabled
    if enabled then refreshAllESP() else clearESP() end
end, "esp")

createToggle(contents["ESP"], "Players", 50, settings.espPlayers, function(enabled)
    espPlayersEnabled = enabled
    if espEnabled then refreshAllESP() end
end, "espPlayers")

createToggle(contents["ESP"], "NPC", 85, settings.espNpc, function(enabled)
    espNpcEnabled = enabled
    if espEnabled then refreshAllESP() end
end, "espNpc")

createToggle(contents["ESP"], "Nickname", 120, settings.espNick, function(enabled)
    espNickEnabled = enabled
end, "espNick")

createToggle(contents["ESP"], "Distance", 155, settings.espDist, function(enabled)
    espDistEnabled = enabled
end, "espDist")

game.Players.PlayerAdded:Connect(function(newPlayer)
    if espEnabled and espPlayersEnabled then createESPforPlayer(newPlayer) end
end)

game.Players.PlayerRemoving:Connect(function()
    if espEnabled then refreshAllESP() end
end)

player.CharacterAdded:Connect(function()
    if espEnabled then task.wait(0.5); refreshAllESP() end
end)

spawn(function()
    while true do
        if espEnabled and espNpcEnabled then refreshAllESP() end
        task.wait(5)
    end
end)

local aimbotEnabled = settings.aimbot
local aimbotPlayersEnabled = settings.aimbotPlayers
local aimbotNpcEnabled = settings.aimbotNpc
local wallCheck = settings.wallCheck
local autoShoot = settings.autoShoot
local aimbotSpeed = settings.aimbotSpeed
local aimbotPart = settings.aimbotPart
local aimbotFov = settings.aimbotFov
local showFovCircle = settings.showFovCircle

local fovCircle = Instance.new("ScreenGui", gui)
fovCircle.Name = "FovCircle"
fovCircle.ResetOnSpawn = false
fovCircle.Enabled = false

local circleFrame = Instance.new("Frame", fovCircle)
circleFrame.Size = UDim2.new(0, aimbotFov, 0, aimbotFov)
circleFrame.Position = UDim2.new(0.5, -aimbotFov/2, 0.5, -aimbotFov/2)
circleFrame.BackgroundTransparency = 1
circleFrame.ZIndex = 999
Instance.new("UICorner", circleFrame).CornerRadius = UDim.new(1, 0)

local circleStroke = Instance.new("UIStroke", circleFrame)
circleStroke.Color = Color3.fromRGB(255, 0, 0)
circleStroke.Thickness = 2

local function updateFovCircle()
    if showFovCircle and aimbotEnabled then
        fovCircle.Enabled = true
        circleFrame.Size = UDim2.new(0, aimbotFov, 0, aimbotFov)
        circleFrame.Position = UDim2.new(0.5, -aimbotFov/2, 0.5, -aimbotFov/2)
    else
        fovCircle.Enabled = false
    end
end

local function getTargetPart(character)
    if not character then return nil end
    if aimbotPart == "Head" then
        return character:FindFirstChild("Head")
    elseif aimbotPart == "Torso" then
        return character:FindFirstChild("UpperTorso") or character:FindFirstChild("Torso")
    else
        local partMap = {
            ["Left Arm"] = "LeftUpperArm",
            ["Right Arm"] = "RightUpperArm", 
            ["Left Leg"] = "LeftUpperLeg",
            ["Right Leg"] = "RightUpperLeg"
        }
        local partName = partMap[aimbotPart]
        if partName then return character:FindFirstChild(partName) end
    end
    return character:FindFirstChild("Head")
end

local function findTarget()
    if not aimbotEnabled then return nil end
    local camera = workspace.CurrentCamera
    local bestTarget = nil
    local bestDistance = aimbotFov

    if aimbotPlayersEnabled then
        for _, target in pairs(game.Players:GetPlayers()) do
            if target ~= player and target.Character and target.Character:FindFirstChild("Humanoid") and target.Character.Humanoid.Health > 0 then
                local targetPart = getTargetPart(target.Character)
                if targetPart then
                    local screenPos, onScreen = camera:WorldToViewportPoint(targetPart.Position)
                    if onScreen then
                        local screenCenter = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)
                        local targetPos = Vector2.new(screenPos.X, screenPos.Y)
                        local distance = (targetPos - screenCenter).Magnitude
                        if distance < bestDistance then
                            if wallCheck then
                                local rayParams = RaycastParams.new()
                                rayParams.FilterType = Enum.RaycastFilterType.Blacklist
                                rayParams.FilterDescendantsInstances = {player.Character}
                                local ray = workspace:Raycast(camera.CFrame.Position, (targetPart.Position - camera.CFrame.Position).Unit * 1000, rayParams)
                                if ray and ray.Instance:IsDescendantOf(target.Character) then
                                    bestTarget = targetPart
                                    bestDistance = distance
                                end
                            else
                                bestTarget = targetPart
                                bestDistance = distance
                            end
                        end
                    end
                end
            end
        end
    end

    if aimbotNpcEnabled then
        for _, obj in pairs(workspace:GetDescendants()) do
            if obj:IsA("Model") and obj:FindFirstChild("Humanoid") and obj.Humanoid.Health > 0 then
                local isPlayerCharacter = false
                for _, plr in pairs(game.Players:GetPlayers()) do
                    if plr.Character == obj then isPlayerCharacter = true; break end
                end
                if not isPlayerCharacter then
                    local targetPart = getTargetPart(obj)
                    if targetPart then
                        local screenPos, onScreen = camera:WorldToViewportPoint(targetPart.Position)
                        if onScreen then
                            local screenCenter = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)
                            local targetPos = Vector2.new(screenPos.X, screenPos.Y)
                            local distance = (targetPos - screenCenter).Magnitude
                            if distance < bestDistance then
                                if wallCheck then
                                    local rayParams = RaycastParams.new()
                                    rayParams.FilterType = Enum.RaycastFilterType.Blacklist
                                    rayParams.FilterDescendantsInstances = {player.Character}
                                    local ray = workspace:Raycast(camera.CFrame.Position, (targetPart.Position - camera.CFrame.Position).Unit * 1000, rayParams)
                                    if ray and ray.Instance:IsDescendantOf(obj) then
                                        bestTarget = targetPart
                                        bestDistance = distance
                                    end
                                else
                                    bestTarget = targetPart
                                    bestDistance = distance
                                end
                            end
                        end
                    end
                end
            end
        end
    end

    return bestTarget
end

createToggle(contents["COMBAT"], "Aimbot", 15, settings.aimbot, function(enabled)
    aimbotEnabled = enabled
    updateFovCircle()
end, "aimbot")

createToggle(contents["COMBAT"], "Wall Check", 50, settings.wallCheck, function(enabled)
    wallCheck = enabled
end, "wallCheck")

createToggle(contents["COMBAT"], "Auto Shoot", 85, settings.autoShoot, function(enabled)
    autoShoot = enabled
end, "autoShoot")

createToggle(contents["COMBAT"], "Aimbot Players", 120, settings.aimbotPlayers, function(enabled)
    aimbotPlayersEnabled = enabled
end, "aimbotPlayers")

createToggle(contents["COMBAT"], "Aimbot NPC", 155, settings.aimbotNpc, function(enabled)
    aimbotNpcEnabled = enabled
end, "aimbotNpc")

createSlider(contents["COMBAT"], "Speed", 190, 1, 20, settings.aimbotSpeed, function(value)
    aimbotSpeed = value
end, "aimbotSpeed")

createSlider(contents["COMBAT"], "FOV", 250, 1, 360, settings.aimbotFov, function(value)
    aimbotFov = value
    updateFovCircle()
end, "aimbotFov")

createToggle(contents["COMBAT"], "FOV Circle", 310, settings.showFovCircle, function(enabled)
    showFovCircle = enabled
    updateFovCircle()
end, "showFovCircle")

local aimbotPartDropdown = Instance.new("Frame", contents["COMBAT"])
aimbotPartDropdown.Size = UDim2.new(1, -20, 0, 30)
aimbotPartDropdown.Position = UDim2.new(0, 10, 0, 350)
aimbotPartDropdown.BackgroundColor3 = colors.button
aimbotPartDropdown.BorderSizePixel = 0
aimbotPartDropdown.ClipsDescendants = true
Instance.new("UICorner", aimbotPartDropdown).CornerRadius = UDim.new(0, 5)

local aimbotPartLabel = Instance.new("TextLabel", aimbotPartDropdown)
aimbotPartLabel.Size = UDim2.new(1, 0, 0, 30)
aimbotPartLabel.BackgroundTransparency = 1
aimbotPartLabel.Text = "Part: " .. settings.aimbotPart
aimbotPartLabel.TextColor3 = colors.text
aimbotPartLabel.TextSize = 13
aimbotPartLabel.Font = Enum.Font.Gotham
aimbotPartLabel.TextXAlignment = Enum.TextXAlignment.Left
aimbotPartLabel.Position = UDim2.new(0, 10, 0, 0)

local aimbotPartList = Instance.new("Frame", aimbotPartDropdown)
aimbotPartList.Size = UDim2.new(1, 0, 0, 0)
aimbotPartList.Position = UDim2.new(0, 0, 0, 30)
aimbotPartList.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
aimbotPartList.BorderSizePixel = 0
aimbotPartList.ClipsDescendants = true
Instance.new("UIListLayout", aimbotPartList)

local parts = {"Head", "Torso", "Left Arm", "Right Arm", "Left Leg", "Right Leg"}
local aimbotPartOpen = false

for _, partName in ipairs(parts) do
    local partBtn = Instance.new("TextButton", aimbotPartList)
    partBtn.Size = UDim2.new(1, 0, 0, 28)
    partBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    partBtn.BorderSizePixel = 0
    partBtn.Text = partName
    partBtn.TextColor3 = colors.text
    partBtn.TextSize = 12
    partBtn.Font = Enum.Font.Gotham
    partBtn.TextXAlignment = Enum.TextXAlignment.Left
    
    partBtn.MouseEnter:Connect(function() tweenService:Create(partBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(40, 40, 40)}):Play() end)
    partBtn.MouseLeave:Connect(function() tweenService:Create(partBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(20, 20, 20)}):Play() end)
    
    partBtn.Activated:Connect(function()
        settings.aimbotPart = partName
        aimbotPart = partName
        aimbotPartLabel.Text = "Part: " .. partName
        saveSetting("aimbotPart", partName)
        aimbotPartOpen = false
        aimbotPartList.Size = UDim2.new(1, 0, 0, 0)
        aimbotPartDropdown.Size = UDim2.new(1, -20, 0, 30)
    end)
end

aimbotPartLabel.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        aimbotPartOpen = not aimbotPartOpen
        if aimbotPartOpen then
            aimbotPartList.Size = UDim2.new(1, 0, 0, 168)
            aimbotPartDropdown.Size = UDim2.new(1, -20, 0, 198)
        else
            aimbotPartList.Size = UDim2.new(1, 0, 0, 0)
            aimbotPartDropdown.Size = UDim2.new(1, -20, 0, 30)
        end
    end
end)

runService.RenderStepped:Connect(function()
    if not aimbotEnabled then return end
    local target = findTarget()
    if target then
        local smoothness = math.clamp(aimbotSpeed / 20, 0.05, 1)
        local targetCFrame = CFrame.new(workspace.CurrentCamera.CFrame.Position, target.Position)
        workspace.CurrentCamera.CFrame = workspace.CurrentCamera.CFrame:Lerp(targetCFrame, smoothness)
        if autoShoot then
            vim:SendMouseButtonEvent(0, 0, 0, true, game, 0)
            task.wait(0.05)
            vim:SendMouseButtonEvent(0, 0, 0, false, game, 0)
        end
    end
end)

local devLabel = Instance.new("TextLabel", contents["SETTING"])
devLabel.Size = UDim2.new(1, -20, 0, 30)
devLabel.Position = UDim2.new(0, 10, 0, 15)
devLabel.BackgroundTransparency = 1
devLabel.Text = "Developer: DrexGhost"
devLabel.TextColor3 = colors.text
devLabel.TextSize = 16
devLabel.Font = Enum.Font.GothamBold
devLabel.TextXAlignment = Enum.TextXAlignment.Left

local versionLabel = Instance.new("TextLabel", contents["SETTING"])
versionLabel.Size = UDim2.new(1, -20, 0, 25)
versionLabel.Position = UDim2.new(0, 10, 0, 50)
versionLabel.BackgroundTransparency = 1
versionLabel.Text = "Delta v1.0"
versionLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
versionLabel.TextSize = 13
versionLabel.Font = Enum.Font.Gotham
versionLabel.TextXAlignment = Enum.TextXAlignment.Left

tabs["RENDER"].BackgroundColor3 = Color3.fromRGB(45, 45, 45)
contents["RENDER"].Visible = true

function toggleMenu()
    settings.menuOpen = not settings.menuOpen
    saveSetting("menuOpen", settings.menuOpen)
    
    if settings.menuOpen then
        main.Visible = true
        main.Size = UDim2.new(0, 0, 0, 300)
        local tween = tweenService:Create(main, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = UDim2.new(0, 600, 0, 300)})
        tween:Play()
        openBtn.Visible = false
    else
        local tween = tweenService:Create(main, TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 300)})
        tween:Play()
        tween.Completed:Connect(function()
            main.Visible = false
        end)
        openBtn.Visible = true
    end
end

closeBtn.Activated:Connect(toggleMenu)

UIS.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.LeftAlt then toggleMenu() end
end)
