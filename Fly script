local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local rootPart = character:WaitForChild("HumanoidRootPart")
local camera = game.Workspace.CurrentCamera

local flySpeed = 50
local riseSpeed = 10
local initialRiseTime = 2 -- Время подъема
local turningSpeed = 0.1
local flying = false

local bodyVelocity
local bodyGyro

-- UI
local function createFlyPanel()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Parent = player:WaitForChild("PlayerGui")

    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 200, 0, 100)
    frame.Position = UDim2.new(0.5, -100, 0.1, 0)
    frame.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
    frame.Parent = screenGui

    local blueBar = Instance.new("Frame")
    blueBar.Size = UDim2.new(1, 0, 0, 30)
    blueBar.Position = UDim2.new(0, 0, 0, 0)
    blueBar.BackgroundColor3 = Color3.new(0, 0.5, 1)
    blueBar.Parent = frame

    local flyLabel = Instance.new("TextLabel")
    flyLabel.Size = UDim2.new(0, 100, 1, 0)
    flyLabel.Position = UDim2.new(0, 0, 0, 0)
    flyLabel.Text = "FLY"
    flyLabel.TextColor3 = Color3.new(1, 1, 1)
    flyLabel.BackgroundTransparency = 1
    flyLabel.Font = Enum.Font.Highway
    flyLabel.TextSize = 24
    flyLabel.Parent = blueBar

    local hideButton = Instance.new("TextButton")
    hideButton.Size = UDim2.new(0, 30, 1, 0)
    hideButton.Position = UDim2.new(0.7, 0, 0, 0)
    hideButton.Text = "-"
    hideButton.TextColor3 = Color3.new(1, 1, 1)
    hideButton.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
    hideButton.Font = Enum.Font.SourceSans
    hideButton.TextSize = 24
    hideButton.Parent = blueBar

    local closeButton = Instance.new("TextButton")
    closeButton.Size = UDim2.new(0, 30, 1, 0)
    closeButton.Position = UDim2.new(0.85, 0, 0, 0)
    closeButton.Text = "X"
    closeButton.TextColor3 = Color3.new(1, 1, 1)
    closeButton.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
    closeButton.Font = Enum.Font.SourceSans
    closeButton.TextSize = 24
    closeButton.Parent = blueBar

    local textLabel = Instance.new("TextLabel")
    textLabel.Size = UDim2.new(1, 0, 0.7, 0)
    textLabel.Position = UDim2.new(0, 0, 0.3, 0)
    textLabel.Text = "By DXNNY"
    textLabel.TextColor3 = Color3.new(1, 1, 1)
    textLabel.BackgroundTransparency = 1
    textLabel.Font = Enum.Font.SourceSans
    textLabel.TextSize = 24
    textLabel.Parent = frame

    -- djdj
    local isHidden = false
    hideButton.MouseButton1Click:Connect(function()
        isHidden = not isHidden
        frame.Visible = not isHidden
        textLabel.Visible = not isHidden
    end)

    -- ppo
    closeButton.MouseButton1Click:Connect(function()
        screenGui:Destroy()
    end)

    -- ekss
    local dragging
    local dragInput
    local startPos
    local startMousePos

    local function updateDrag(input)
        local delta = input.Position - startMousePos
        frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end

    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            startMousePos = input.Position
            startPos = frame.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then
            dragInput = input
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            updateDrag(input)
        end
    end)

    return screenGui
end

-- ФY
local function initialRise()
    local startTime = tick()

    while tick() - startTime < initialRiseTime do
        bodyVelocity.Velocity = Vector3.new(0, riseSpeed, 0)
        RunService.RenderStepped:Wait()
    end
end

-- fly
local function startFlying()
    if flying then return end -- Проверка на то, что уже летим

    flying = true
    humanoid.PlatformStand = true

    -- BodyVelocity и BodyGyro
    bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.MaxForce = Vector3.new(100000, 100000, 100000)
    bodyVelocity.Velocity = Vector3.new(0, 0, 0)
    bodyVelocity.Parent = rootPart

    bodyGyro = Instance.new("BodyGyro")
    bodyGyro.MaxTorque = Vector3.new(100000, 100000, 100000)
    bodyGyro.CFrame = rootPart.CFrame
    bodyGyro.Parent = rootPart

    -- s7jsj
    local flyPanel = createFlyPanel()

    -- shshhssh
    initialRise()

    RunService.RenderStepped:Connect(function()
        if flying then
            -- shshhehsh
            local moveDirection = humanoid.MoveDirection * flySpeed
            local camLookVector = camera.CFrame.LookVector

            -- ggggggggg
            if camLookVector.Y < -0.5 and moveDirection.Magnitude > 0 then
                -- gggggg
                moveDirection = moveDirection + Vector3.new(0, math.abs(camLookVector.Y) * flySpeed, 0)
            else
                -- Дwiej
                moveDirection = moveDirection + Vector3.new(0, camLookVector.Y * flySpeed, 0)
            end

            bodyVelocity.Velocity = moveDirection

            -- sjsjdjsh
            local targetCFrame = CFrame.new(rootPart.Position, rootPart.Position + camLookVector)
            bodyGyro.CFrame = bodyGyro.CFrame:Lerp(targetCFrame, turningSpeed)
        end
    end)
end

-- ggg
local function stopFlying()
    flying = false
    humanoid.PlatformStand = false

    if bodyVelocity then bodyVelocity:Destroy() end
    if bodyGyro then bodyGyro:Destroy() end
end

-- ggggg
local function onChatMessage(message)
    if message:lower() == ";fly" then
        startFlying()
    elseif message:lower() == ";unfly" then
        stopFlying()
    end
end

player.Chatted:Connect(onChatMessage)
