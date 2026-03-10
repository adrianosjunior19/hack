local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AnimeUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

local main = Instance.new("Frame")
main.Name = "Main"
main.Size = UDim2.new(0, 420, 0, 260)
main.Position = UDim2.new(0.5, -210, 0.5, -130)
main.BackgroundColor3 = Color3.fromRGB(22, 22, 30)
main.BorderSizePixel = 0
main.Parent = screenGui

Instance.new("UICorner", main).CornerRadius = UDim.new(0, 14)

local stroke = Instance.new("UIStroke")
stroke.Color = Color3.fromRGB(90, 90, 140)
stroke.Thickness = 1.5
stroke.Parent = main

local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 42)
titleBar.BackgroundColor3 = Color3.fromRGB(32, 32, 44)
titleBar.BorderSizePixel = 0
titleBar.Parent = main

Instance.new("UICorner", titleBar).CornerRadius = UDim.new(0, 14)

local fixMask = Instance.new("Frame")
fixMask.Size = UDim2.new(1, 0, 0, 16)
fixMask.Position = UDim2.new(0, 0, 1, -16)
fixMask.BackgroundColor3 = Color3.fromRGB(32, 32, 44)
fixMask.BorderSizePixel = 0
fixMask.Parent = titleBar

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -20, 1, 0)
title.Position = UDim2.new(0, 12, 0, 0)
title.BackgroundTransparency = 1
title.Text = "Anime Combates UI"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextSize = 22
title.Font = Enum.Font.GothamBold
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = titleBar

local content = Instance.new("Frame")
content.Size = UDim2.new(1, -20, 1, -62)
content.Position = UDim2.new(0, 10, 0, 52)
content.BackgroundTransparency = 1
content.Parent = main

local layout = Instance.new("UIListLayout")
layout.Padding = UDim.new(0, 10)
layout.Parent = content

local function createCard(text)
	local card = Instance.new("Frame")
	card.Size = UDim2.new(1, 0, 0, 54)
	card.BackgroundColor3 = Color3.fromRGB(28, 28, 38)
	card.BorderSizePixel = 0
	card.Parent = content

	Instance.new("UICorner", card).CornerRadius = UDim.new(0, 12)

	local label = Instance.new("TextLabel")
	label.Size = UDim2.new(0.65, 0, 1, 0)
	label.Position = UDim2.new(0, 14, 0, 0)
	label.BackgroundTransparency = 1
	label.Text = text
	label.TextColor3 = Color3.fromRGB(235, 235, 235)
	label.TextSize = 18
	label.Font = Enum.Font.GothamSemibold
	label.TextXAlignment = Enum.TextXAlignment.Left
	label.Parent = card

	local toggle = Instance.new("TextButton")
	toggle.Size = UDim2.new(0, 110, 0, 34)
	toggle.Position = UDim2.new(1, -124, 0.5, -17)
	toggle.BackgroundColor3 = Color3.fromRGB(60, 60, 80)
	toggle.Text = "DESLIGADO"
	toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
	toggle.TextSize = 14
	toggle.Font = Enum.Font.GothamBold
	toggle.Parent = card

	Instance.new("UICorner", toggle).CornerRadius = UDim.new(0, 10)

	local enabled = false
	toggle.MouseButton1Click:Connect(function()
		enabled = not enabled
		if enabled then
			toggle.Text = "LIGADO"
			TweenService:Create(toggle, TweenInfo.new(0.2), {
				BackgroundColor3 = Color3.fromRGB(80, 170, 120)
			}):Play()
		else
			toggle.Text = "DESLIGADO"
			TweenService:Create(toggle, TweenInfo.new(0.2), {
				BackgroundColor3 = Color3.fromRGB(60, 60, 80)
			}):Play()
		end
	end)
end

createCard("Auto Ganho")
createCard("Luck Visual")
createCard("Notificações")

local dragging = false
local dragStart
local startPos

titleBar.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		dragStart = input.Position
		startPos = main.Position
	end
end)

titleBar.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = false
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
		local delta = input.Position - dragStart
		main.Position = UDim2.new(
			startPos.X.Scale,
			startPos.X.Offset + delta.X,
			startPos.Y.Scale,
			startPos.Y.Offset + delta.Y
		)
	end
end)

main.Size = UDim2.new(0, 0, 0, 0)
main.Position = UDim2.new(0.5, 0, 0.5, 0)
TweenService:Create(main, TweenInfo.new(0.35, Enum.EasingStyle.Back), {
	Size = UDim2.new(0, 420, 0, 260),
	Position = UDim2.new(0.5, -210, 0.5, -130)
}):Play()
