-- Skibidi Battle Simulator Gem GUI (Profile Style + Animations)
-- Load with: loadstring(game:HttpGet("YOUR_RAW_URL_HERE"))()

local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "SkibidiGemHub"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

-- Small K Logo (Above GUI)
local kButton = Instance.new("TextButton")
kButton.Size = UDim2.new(0, 45, 0, 45)
kButton.Position = UDim2.new(0.5, -22, 0.28, -60)
kButton.BackgroundColor3 = Color3.fromRGB(220, 50, 50)
kButton.Text = "K"
kButton.TextColor3 = Color3.new(1,1,1)
kButton.TextScaled = true
kButton.Font = Enum.Font.GothamBold
kButton.Parent = screenGui

local kCorner = Instance.new("UICorner")
kCorner.CornerRadius = UDim.new(1, 0)
kCorner.Parent = kButton

-- Main Frame
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 280, 0, 360)
frame.Position = UDim2.new(0.5, -140, 0.5, -180)
frame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
frame.BorderSizePixel = 0
frame.Parent = screenGui

local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim.new(0, 10)
uiCorner.Parent = frame

-- Top Bar
local topBar = Instance.new("Frame")
topBar.Size = UDim2.new(1, 0, 0, 50)
topBar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
topBar.Parent = frame

local profileIcon = Instance.new("ImageLabel")
profileIcon.Size = UDim2.new(0, 32, 0, 32)
profileIcon.Position = UDim2.new(0, 12, 0.5, -16)
profileIcon.BackgroundTransparency = 1
profileIcon.Image = "rbxthumb://type=AvatarHeadShot&id=" .. player.UserId .. "&w=48&h=48"
profileIcon.Parent = topBar

local profileName = Instance.new("TextLabel")
profileName.Size = UDim2.new(1, -100, 1, 0)
profileName.Position = UDim2.new(0, 55, 0, 0)
profileName.BackgroundTransparency = 1
profileName.Text = player.DisplayName .. "'s Gems"
profileName.TextColor3 = Color3.new(1,1,1)
profileName.TextXAlignment = Enum.TextXAlignment.Left
profileName.TextScaled = true
profileName.Font = Enum.Font.GothamBold
profileName.Parent = topBar

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 36, 0, 36)
closeBtn.Position = UDim2.new(1, -42, 0, 7)
closeBtn.BackgroundColor3 = Color3.fromRGB(200, 40, 40)
closeBtn.Text = "✕"
closeBtn.TextColor3 = Color3.new(1,1,1)
closeBtn.TextScaled = true
closeBtn.Parent = topBar

local accent = Instance.new("Frame")
accent.Size = UDim2.new(1, 0, 0, 4)
accent.Position = UDim2.new(0, 0, 0, 50)
accent.BackgroundColor3 = Color3.fromRGB(220, 50, 50)
accent.Parent = frame

local TweenService = game:GetService("TweenService")
local tweenInfo = TweenInfo.new(0.4, Enum.EasingStyle.Quint, Enum.EasingDirection.Out)

local function openGUI()
    frame.Visible = true
    frame.Size = UDim2.new(0, 0, 0, 0)
    TweenService:Create(frame, tweenInfo, {Size = UDim2.new(0, 280, 0, 360), Position = UDim2.new(0.5, -140, 0.5, -180)}):Play()
end

local function closeGUI()
    local t = TweenService:Create(frame, tweenInfo, {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)})
    t:Play()
    t.Completed:Connect(function() frame.Visible = false end)
end

-- Draggable
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

local function createButton(text, amount, yPos)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.9, 0, 0, 50)
    btn.Position = UDim2.new(0.05, 0, 0, yPos)
    btn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
    btn.Text = text
    btn.TextColor3 = Color3.new(1,1,1)
    btn.TextScaled = true
    btn.Font = Enum.Font.GothamSemibold
    btn.Parent = frame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 10)
    corner.Parent = btn

    btn.MouseButton1Click:Connect(function()
        pcall(function()
            firesignal(game:GetService("ReplicatedStorage").Events.GameEvent.OnClientEvent, {
                type = "NotifyPlayer",
                method = "N_RefreshGemUI",
                args = {1, amount, Vector3.new(0,0,0)}
            })
        end)
    end)
end

createButton("Add 100 Gems", 100, 70)
createButton("Add 500 Gems", 500, 130)
createButton("Add 1,000 Gems", 1000, 190)
createButton("Add 5,000 Gems", 5000, 250)
createButton("Add 10,000 Gems", 10000, 310)

closeBtn.MouseButton1Click:Connect(closeGUI)
kButton.MouseButton1Click:Connect(openGUI)

openGUI()
print("✅ Skibidi Gem Hub Loaded!")
