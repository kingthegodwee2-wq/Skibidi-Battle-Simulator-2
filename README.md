-- TEST
-- Skibidi Battle Simulator Simple Purple GUI

local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "TestGemGUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

-- Draggable T Logo
local tButton = Instance.new("TextButton")
tButton.Size = UDim2.new(0, 50, 0, 50)
tButton.Position = UDim2.new(0.5, -25, 0.3, -90)
tButton.BackgroundColor3 = Color3.fromRGB(60, 20, 90)
tButton.Text = "T"
tButton.TextColor3 = Color3.new(1,1,1)
tButton.TextScaled = true
tButton.Font = Enum.Font.GothamBold
tButton.Parent = screenGui

local tCorner = Instance.new("UICorner")
tCorner.CornerRadius = UDim.new(1, 0)
tCorner.Parent = tButton

-- Draggable T Logo
local draggingT = false
tButton.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        draggingT = true
        local dragStart = input.Position
        local startPos = tButton.Position
        local conn = game:GetService("UserInputService").InputChanged:Connect(function(inp)
            if draggingT and (inp.UserInputType == Enum.UserInputType.MouseMovement or inp.UserInputType == Enum.UserInputType.Touch) then
                local delta = inp.Position - dragStart
                tButton.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
            end
        end)
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then draggingT = false conn:Disconnect() end
        end)
    end
end)

-- TEST Header (Dark Purple)
local testHeader = Instance.new("Frame")
testHeader.Size = UDim2.new(0, 280, 0, 45)
testHeader.Position = UDim2.new(0.5, -140, 0.5, -145)
testHeader.BackgroundColor3 = Color3.fromRGB(50, 20, 80)
testHeader.BorderSizePixel = 0
testHeader.Parent = screenGui

local headerCorner = Instance.new("UICorner")
headerCorner.CornerRadius = UDim.new(0, 10)
headerCorner.Parent = testHeader

local testLabel = Instance.new("TextLabel")
testLabel.Size = UDim2.new(1, 0, 1, 0)
testLabel.BackgroundTransparency = 1
testLabel.Text = "TEST"
testLabel.TextColor3 = Color3.new(1, 1, 1)
testLabel.TextScaled = true
testLabel.Font = Enum.Font.GothamBlack
testLabel.Parent = testHeader

-- Main Frame
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 280, 0, 200)
frame.Position = UDim2.new(0.5, -140, 0.5, -90)
frame.BackgroundColor3 = Color3.fromRGB(30, 25, 50)
frame.BorderSizePixel = 0
frame.Parent = screenGui

local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim.new(0, 12)
uiCorner.Parent = frame

-- Top Profile Bar
local topBar = Instance.new("Frame")
topBar.Size = UDim2.new(1, 0, 0, 50)
topBar.BackgroundColor3 = Color3.fromRGB(45, 35, 70)
topBar.Parent = frame

local profileIcon = Instance.new("ImageLabel")
profileIcon.Size = UDim2.new(0, 35, 0, 35)
profileIcon.Position = UDim2.new(0, 15, 0.5, -17.5)
profileIcon.BackgroundTransparency = 1
profileIcon.Image = "rbxthumb://type=AvatarHeadShot&id=" .. player.UserId .. "&w=48&h=48"
profileIcon.Parent = topBar

local profileName = Instance.new("TextLabel")
profileName.Size = UDim2.new(1, -100, 1, 0)
profileName.Position = UDim2.new(0, 60, 0, 0)
profileName.BackgroundTransparency = 1
profileName.Text = player.DisplayName
profileName.TextColor3 = Color3.new(1,1,1)
profileName.TextScaled = true
profileName.Font = Enum.Font.GothamBold
profileName.Parent = topBar

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 35, 0, 35)
closeBtn.Position = UDim2.new(1, -45, 0, 7)
closeBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 60)
closeBtn.Text = "✕"
closeBtn.TextColor3 = Color3.new(1,1,1)
closeBtn.TextScaled = true
closeBtn.Parent = topBar

-- Big Purple +1 Gem Button
local gemButton = Instance.new("TextButton")
gemButton.Size = UDim2.new(0.85, 0, 0, 75)
gemButton.Position = UDim2.new(0.075, 0, 0, 70)
gemButton.BackgroundColor3 = Color3.fromRGB(160, 60, 255)
gemButton.Text = "+1 Gem"
gemButton.TextColor3 = Color3.new(1,1,1)
gemButton.TextScaled = true
gemButton.Font = Enum.Font.GothamBold
gemButton.Parent = frame

local btnCorner = Instance.new("UICorner")
btnCorner.CornerRadius = UDim.new(0, 12)
btnCorner.Parent = gemButton

-- Animations
local TweenService = game:GetService("TweenService")
local tweenInfo = TweenInfo.new(0.4, Enum.EasingStyle.Quint)

local function openGUI()
    frame.Visible = true
    frame.Size = UDim2.new(0,0,0,0)
    TweenService:Create(frame, tweenInfo, {Size = UDim2.new(0,280,0,200)}):Play()
end

local function closeGUI()
    local t = TweenService:Create(frame, tweenInfo, {Size = UDim2.new(0,0,0,0)})
    t:Play()
    t.Completed:Connect(function() frame.Visible = false end)
end

-- Draggable Main Window
local dragging
topBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        local dragStart = input.Position
        local startPos = frame.Position
        local conn = game:GetService("UserInputService").InputChanged:Connect(function(inp)
            if dragging and (inp.UserInputType == Enum.UserInputType.MouseMovement or inp.UserInputType == Enum.UserInputType.Touch) then
                local delta = inp.Position - dragStart
                frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
            end
        end)
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then dragging = false conn:Disconnect() end
        end)
    end
end)

gemButton.MouseButton1Click:Connect(function()
    pcall(function()
        firesignal(game:GetService("ReplicatedStorage").Events.GameEvent.OnClientEvent, {
            type = "NotifyPlayer",
            method = "N_RefreshGemUI",
            args = {1, 1, Vector3.new(0,0,0)}
        })
    end)
end)

closeBtn.MouseButton1Click:Connect(closeGUI)
tButton.MouseButton1Click:Connect(openGUI)

openGUI()
print("✅ TEST Purple GUI Loaded!")
