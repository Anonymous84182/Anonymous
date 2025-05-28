-- Services
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local StarterGui = game:GetService("StarterGui")

-- Local Player
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- GUI Setup (Mobile-Friendly)
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "HackGui"
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true

-- Scrollable Frame
local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 250, 0, 450)
Frame.Position = UDim2.new(0, 10, 0, 50)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.BorderSizePixel = 0
Frame.Parent = ScreenGui
Frame.ClipsDescendants = true

local ScrollingFrame = Instance.new("ScrollingFrame")
ScrollingFrame.Size = UDim2.new(1, 0, 1, 0)
ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, 600)
ScrollingFrame.ScrollBarThickness = 8
ScrollingFrame.BackgroundTransparency = 1
ScrollingFrame.Parent = Frame

-- Add Glow Effect to Frame
local UIStroke = Instance.new("UIStroke")
UIStroke.Thickness = 2
UIStroke.Color = Color3.fromRGB(255, 255, 255)
UIStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
UIStroke.Parent = Frame

local UIGradient = Instance.new("UIGradient")
UIGradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 255)),
    ColorSequenceKeypoint.new(0.5, Color3.fromRGB(0, 255, 255)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 0, 255))
})
UIGradient.Rotation = 45
UIGradient.Parent = Frame

-- Show/Hide Toggle Button (Enhanced for Mobile and PC)
local ToggleGuiButton = Instance.new("TextButton")
ToggleGuiButton.Size = UDim2.new(0, 100, 0, 50)
ToggleGuiButton.Position = UDim2.new(0, 10, 0, 10)
ToggleGuiButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
ToggleGuiButton.BackgroundTransparency = 0.2
ToggleGuiButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleGuiButton.Text = "Hack Menu"
ToggleGuiButton.TextSize = 20
ToggleGuiButton.Parent = ScreenGui
ToggleGuiButton.Visible = true
ToggleGuiButton.ZIndex = 100

-- Add Glow to Toggle Button
local ButtonStroke = Instance.new("UIStroke")
ButtonStroke.Thickness = 2
ButtonStroke.Color = Color3.fromRGB(255, 255, 255)
ButtonStroke.Parent = ToggleGuiButton

local ButtonGradient = Instance.new("UIGradient")
ButtonGradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 255)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 255, 255))
})
ButtonGradient.Parent = ToggleGuiButton

-- Debug Notification for Toggle Button
pcall(function()
    StarterGui:SetCore("SendNotification", {
        Title = "Hack GUI",
        Text = "Press Tab (PC) or tap 'Hack Menu' to toggle GUI.",
        Duration = 5
    })
end)

-- Function to create a button
local function CreateButton(name, position, callback)
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(0, 220, 0, 50)
    Button.Position = position
    Button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.Text = name
    Button.TextSize = 18
    Button.Parent = ScrollingFrame
    Button.MouseButton1Click:Connect(function()
        pcall(callback)
    end)
    local BtnStroke = Instance.new("UIStroke")
    BtnStroke.Thickness = 1.5
    BtnStroke.Color = Color3.fromRGB(255, 255, 255)
    BtnStroke.Parent = Button
    local BtnGradient = Instance.new("UIGradient")
    BtnGradient.Color = ColorSequence.new({
        ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 255)),
        ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 255, 255))
    })
    BtnGradient.Parent = Button
    return Button
end

-- Function to create a slider
local function CreateSlider(name, position, callback)
    local SliderFrame = Instance.new("Frame")
    SliderFrame.Size = UDim2.new(0, 220, 0, 50)
    SliderFrame.Position = position
    SliderFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    SliderFrame.Parent = ScrollingFrame

    local SliderLabel = Instance.new("TextLabel")
    SliderLabel.Size = UDim2.new(0, 220, 0, 20)
    SliderLabel.BackgroundTransparency = 1
    SliderLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    SliderLabel.Text = name .. ": 70%"
    SliderLabel.TextSize = 16
    SliderLabel.Parent = SliderFrame

    local SliderBar = Instance.new("Frame")
    SliderBar.Size = UDim2.new(0, 200, 0, 10)
    SliderBar.Position = UDim2.new(0, 10, 0, 30)
    SliderBar.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    SliderBar.Parent = SliderFrame

    local SliderKnob = Instance.new("TextButton")
    SliderKnob.Size = UDim2.new(0, 20, 0, 20)
    SliderKnob.Position = UDim2.new(0.65, -10, 0, 25)
    SliderKnob.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    SliderKnob.Text = ""
    SliderKnob.Parent = SliderFrame

    local SliderStroke = Instance.new("UIStroke")
    SliderStroke.Thickness = 1
    SliderStroke.Color = Color3.fromRGB(255, 255, 255)
    SliderStroke.Parent = SliderBar

    local SliderKnobStroke = Instance.new("UIStroke")
    SliderKnobStroke.Thickness = 1
    SliderKnobStroke.Color = Color3.fromRGB(255, 255, 255)
    SliderKnobStroke.Parent = SliderKnob

    local isDragging = false
    SliderKnob.MouseButton1Down:Connect(function()
        isDragging = true
    end)
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            isDragging = false
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if isDragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local xPos = math.clamp((input.Position.X - SliderBar.AbsolutePosition.X) / SliderBar.AbsoluteSize.X, 0, 1)
            SliderKnob.Position = UDim2.new(xPos, -10, 0, 25)
            local hitChance = math.floor(xPos * 100)
            SliderLabel.Text = name .. ": " .. hitChance .. "%"
            callback(hitChance)
        end
    end)
    return SliderFrame
end

-- Feature Toggles
local ESPEnabled = false
local AimbotEnabled = false
local SilentAimEnabled = false
local FlyEnabled = false
local WalkspeedEnabled = false
local LoopKillEnabled = false
local FlySpeed = 50
local WalkSpeed = 16
local SilentAimHitChance = 70
local ESPBoxes = {}
local IsMobile = UserInputService.TouchEnabled and not UserInputService.MouseEnabled

-- Toggle GUI Visibility
local GuiVisible = true
local function ToggleGui()
    GuiVisible = not GuiVisible
    Frame.Visible = GuiVisible
    ToggleGuiButton.Text = GuiVisible and "Hack Menu" or "Open Menu"
end

ToggleGuiButton.MouseButton1Click:Connect(ToggleGui)

-- Tab key toggle for PC
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.Tab then
        ToggleGui()
    end
end)

if IsMobile then
    UserInputService.TouchTap:Connect(function(touchPositions, processed)
        if not processed then
            local touchPos = touchPositions[1]
            local buttonPos = ToggleGuiButton.AbsolutePosition
            local buttonSize = ToggleGuiButton.AbsoluteSize
            if touchPos.X >= buttonPos.X and touchPos.X <= buttonPos.X + buttonSize.X and
               touchPos.Y >= buttonPos.Y and touchPos.Y <= buttonPos.Y + buttonSize.Y then
                ToggleGui()
                pcall(function()
                    StarterGui:SetCore("SendNotification", {
                        Title = "GUI Toggled",
                        Text = "GUI " .. (GuiVisible and "shown" or "hidden") .. ".",
                        Duration = 2
                    })
                end)
            end
        end
    end)
end

-- ESP Function (Box ESP)
local function CreateESP(player)
    if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local Box = Instance.new("BoxHandleAdornment")
        Box.Name = "ESPBox"
        Box.Size = Vector3.new(4, 6, 4)
        Box.Color3 = Color3.fromRGB(255, 0, 0)
        Box.Transparency = 0.5
        Box.AlwaysOnTop = true
        Box.Adornee = player.Character.HumanoidRootPart
        Box.Parent = player.Character
        Box.ZIndex = 10
        ESPBoxes[player] = Box
    end
end

local function RemoveESP(player)
    if ESPBoxes[player] then
        ESPBoxes[player]:Destroy()
        ESPBoxes[player] = nil
    end
end

local function ToggleESP()
    ESPEnabled = not ESPEnabled
    if ESPEnabled then
        for _, player in ipairs(Players:GetPlayers()) do
            pcall(CreateESP, player)
        end
        Players.PlayerAdded:Connect(function(player)
            if ESPEnabled then
                pcall(CreateESP, player)
            end
        end)
        Players.PlayerRemoving:Connect(function(player)
            pcall(RemoveESP, player)
        end)
    else
        for _, player in ipairs(Players:GetPlayers()) do
            pcall(RemoveESP, player)
        end
    end
end

-- Walkspeed Function
local function ToggleWalkspeed()
    WalkspeedEnabled = not WalkspeedEnabled
    local character = LocalPlayer.Character
    if character and character:FindFirstChild("Humanoid") then
        local humanoid = character.Humanoid
        humanoid.WalkSpeed = WalkspeedEnabled and 50 or 16
    end
end

-- LoopKill All Function
local function ToggleLoopKill()
    LoopKillEnabled = not LoopKillEnabled
    if LoopKillEnabled then
        spawn(function()
            while LoopKillEnabled do
                local success, result = pcall(function()
                    for _, player in ipairs(Players:GetPlayers()) do
                        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Humanoid") then
                            player.Character.Humanoid.Health = 0
                        end
                    end
                end)
                if not success then
                    warn("LoopKill error: " .. result)
                end
                wait(0.5)
            end
        end)
    end
end

-- Silent Aim Function (Smooth Tracking)
local SilentAimTarget = nil
local function GetClosestPlayer()
    local closestPlayer = nil
    local closestDistance = math.huge
    local screenCenter = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)

    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") and player.Character:FindFirstChild("Humanoid") and player.Character.Humanoid.Health > 0 then
            local head = player.Character.Head
            local screenPoint, onScreen = Camera:WorldToViewportPoint(head.Position)
            if onScreen then
                local distance = (Vector2.new(screenPoint.X, ScreenGui.IgnoreGuiInset and screenPoint.Y or screenPoint.Y - 36) - screenCenter).Magnitude
                if distance < closestDistance then
                    closestDistance = distance
                    closestPlayer = player
                end
            end
        end
    end
    return closestPlayer
end

local function ToggleSilentAim()
    SilentAimEnabled = not SilentAimEnabled
    if not SilentAimEnabled then
        SilentAimTarget = nil
    end
end

local function UpdateHitChance(hitChance)
    SilentAimHitChance = hitChance
end

local function SilentAim()
    if SilentAimEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local tool = LocalPlayer.Character:FindFirstChildOfClass("Tool")
        if tool then
            local success, result = pcall(function()
                if SilentAimTarget and SilentAimTarget.Character and SilentAimTarget.Character:FindFirstChild("Humanoid") and SilentAimTarget.Character.Humanoid.Health <= 0 then
                    SilentAimTarget = nil
                end
                if not SilentAimTarget then
                    SilentAimTarget = GetClosestPlayer()
                end
                if SilentAimTarget and SilentAimTarget.Character and SilentAimTarget.Character:FindFirstChild("Head") then
                    local hitChance = math.random(100)
                    if hitChance <= SilentAimHitChance then
                        local headPos = SilentAimTarget.Character.Head.Position
                        local currentLook = Camera.CFrame.LookVector
                        local targetLook = (headPos - Camera.CFrame.Position).Unit
                        local smoothLook = currentLook:Lerp(targetLook, 0.1) -- Smooth tracking
                        Camera.CFrame = CFrame.new(Camera.CFrame.Position, Camera.CFrame.Position + smoothLook)
                    end
                end
            end)
            if not success then
                warn("Silent Aim error: " .. result)
            end
        else
            SilentAimTarget = nil
        end
    end
end

-- Aimbot Function
local AimbotTarget = nil
if IsMobile then
    local LastTouchTime = 0
    UserInputService.TouchTap:Connect(function(touchPositions, processed)
        if AimbotEnabled and not processed then
            local currentTime = tick()
            if currentTime - LastTouchTime < 0.5 then
                AimbotTarget = GetClosestPlayer()
            end
            LastTouchTime = currentTime
        end
    end)
end

local function ToggleAimbot()
    AimbotEnabled = not AimbotEnabled
    if not AimbotEnabled then
        AimbotTarget = nil
    end
end

local function Aimbot()
    if AimbotEnabled then
        local success, result = pcall(function()
            if IsMobile and AimbotTarget and AimbotTarget.Character and AimbotTarget.Character:FindFirstChild("Head") and AimbotTarget.Character:FindFirstChild("Humanoid") and AimbotTarget.Character.Humanoid.Health > 0 then
                local headPos = AimbotTarget.Character.Head.Position
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, headPos)
            elseif not IsMobile then
                local closestPlayer = GetClosestPlayer()
                if closestPlayer and closestPlayer.Character and closestPlayer.Character:FindFirstChild("Head") then
                    local headPos = closestPlayer.Character.Head.Position
                    Camera.CFrame = CFrame.new(Camera.CFrame.Position, headPos)
                end
            end
        end)
        if not success then
            warn("Aimbot error: " .. result)
        end
    end
end

-- Shooting Mechanism
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.UserInputType == Enum.UserInputType.MouseButton1 then
        local tool = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Tool")
        if tool then
            tool:Activate()
        end
    end
end)

-- Fly Function
local BodyVelocity
local BodyGyro
local JoystickFrame
local JoystickThumb
local JoystickCenter = Vector2.new(0, 0)
local JoystickInput = Vector3.new(0, 0, 0)

if IsMobile then
    JoystickFrame = Instance.new("Frame")
    JoystickFrame.Size = UDim2.new(0, 100, 0, 100)
    JoystickFrame.Position = UDim2.new(0, 10, 1, -110)
    JoystickFrame.BackgroundTransparency = 0.5
    JoystickFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    JoystickFrame.Parent = ScreenGui
    JoystickFrame.Visible = false

    JoystickThumb = Instance.new("Frame")
    JoystickThumb.Size = UDim2.new(0, 40, 0, 40)
    JoystickThumb.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    JoystickThumb.BorderSizePixel = 0
    JoystickThumb.Parent = JoystickFrame

    local function UpdateJoystick(touch)
        local center = Vector2.new(JoystickFrame.AbsolutePosition.X + JoystickFrame.AbsoluteSize.X / 2, JoystickFrame.AbsolutePosition.Y + JoystickFrame.AbsoluteSize.Y / 2)
        local delta = Vector2.new(touch.Position.X - center.X, touch.Position.Y - center.Y)
        local magnitude = delta.Magnitude
        local maxRadius = 50
        if magnitude > maxRadius then
            delta = delta.Unit * maxRadius
        end
        JoystickThumb.Position = UDim2.new(0, center.X - JoystickFrame.AbsolutePosition.X + delta.X - 20, 0, center.Y - JoystickFrame.AbsolutePosition.Y + delta.Y - 20)
        JoystickInput = Vector3.new(delta.X / maxRadius, 0, delta.Y / maxRadius)
    end

    UserInputService.TouchStarted:Connect(function(touch, processed)
        if FlyEnabled and not processed and JoystickFrame.Visible then
            JoystickCenter = touch.Position
            UpdateJoystick(touch)
        end
    end)

    UserInputService.TouchMoved:Connect(function(touch, processed)
        if FlyEnabled and not processed and JoystickFrame.Visible then
            UpdateJoystick(touch)
        end
    end)

    UserInputService.TouchEnded:Connect(function(touch, processed)
        if FlyEnabled and not processed then
            JoystickThumb.Position = UDim2.new(0.5, -20, 0.5, -20)
            JoystickInput = Vector3.new(0, 0, 0)
        end
    end)
end

local function ToggleFly()
    FlyEnabled = not FlyEnabled
    local character = LocalPlayer.Character
    if not character or not character:FindFirstChild("HumanoidRootPart") or not character:FindFirstChild("Humanoid") then return end
    local rootPart = character.HumanoidRootPart
    local humanoid = character.Humanoid

    if FlyEnabled then
        if IsMobile then
            JoystickFrame.Visible = true
        end
        BodyVelocity = Instance.new("BodyVelocity")
        BodyVelocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
        BodyVelocity.Velocity = Vector3.new(0, 0, 0)
        BodyVelocity.Parent = rootPart

        BodyGyro = Instance.new("BodyGyro")
        BodyGyro.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
        BodyGyro.CFrame = rootPart.CFrame
        BodyGyro.Parent = rootPart

        humanoid.PlatformStand = true

        spawn(function()
            while FlyEnabled and character and character.Parent do
                local success, result = pcall(function()
                    local moveDirection = Vector3.new(0, 0, 0)
                    if IsMobile then
                        moveDirection = Camera.CFrame * JoystickInput * FlySpeed
                        for _, touch in ipairs(UserInputService:GetTouches()) do
                            if touch.Position.Y < Camera.ViewportSize.Y / 2 then
                                moveDirection = moveDirection + Vector3.new(0, FlySpeed, 0)
                            else
                                moveDirection = moveDirection - Vector3.new(0, FlySpeed, 0)
                            end
                        end
                    else
                        if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                            moveDirection = moveDirection + Camera.CFrame.LookVector
                        end
                        if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                            moveDirection = moveDirection - Camera.CFrame.LookVector
                        end
                        if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                            moveDirection = moveDirection - Camera.CFrame.RightVector
                        end
                        if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                            moveDirection = moveDirection + Camera.CFrame.RightVector
                        end
                        if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
                            moveDirection = moveDirection + Vector3.new(0, 1, 0)
                        end
                        if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then
                            moveDirection = moveDirection - Vector3.new(0, 1, 0)
                        end
                        moveDirection = moveDirection * FlySpeed
                    end
                    BodyVelocity.Velocity = moveDirection
                    BodyGyro.CFrame = Camera.CFrame
                end)
                if not success then
                    warn("Fly error: " .. result)
                    break
                end
                RunService.Stepped:Wait()
            end
            if BodyVelocity then
                BodyVelocity:Destroy()
                BodyVelocity = nil
            end
            if BodyGyro then
                BodyGyro:Destroy()
                BodyGyro = nil
            end
            if IsMobile then
                JoystickFrame.Visible = false
            end
            humanoid.PlatformStand = false
        end)
    else
        if BodyVelocity then
            BodyVelocity:Destroy()
            BodyVelocity = nil
        end
        if BodyGyro then
            BodyGyro:Destroy()
            BodyGyro = nil
        end
        if IsMobile then
            JoystickFrame.Visible = false
        end
        humanoid.PlatformStand = false
    end
end

-- Create GUI Buttons and Slider
CreateButton("Toggle ESP", UDim2.new(0, 15, 0, 60), ToggleESP)
CreateButton("Toggle Aimbot", UDim2.new(0, 15, 0, 120), ToggleAimbot)
CreateButton("Toggle Silent Aim", UDim2.new(0, 15, 0, 180), ToggleSilentAim)
CreateButton("Toggle Fly", UDim2.new(0, 15, 0, 240), ToggleFly)
CreateButton("Toggle Walkspeed", UDim2.new(0, 15, 0, 300), ToggleWalkspeed)
CreateButton("Toggle LoopKill All", UDim2.new(0, 15, 0, 360), ToggleLoopKill)
CreateSlider("Silent Aim Hit Chance", UDim2.new(0, 15, 0, 420), UpdateHitChance)

-- Connect Aimbot and Silent Aim to RenderStepped
RunService.RenderStepped:Connect(function()
    Aimbot()
    SilentAim()
end)

-- Notification
pcall(function()
    StarterGui:SetCore("SendNotification", {
        Title = "Ultimate Hack GUI Loaded",
        Text = "ESP, Aimbot, Silent Aim (smooth), Fly, Walkspeed, LoopKill loaded. Press Tab (PC) or tap 'Hack Menu' to toggle.",
        Duration = 5
    })
end)
