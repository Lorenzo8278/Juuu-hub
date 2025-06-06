-- Blox Fruits Script com painel animado abrir/fechar por ChatGPT
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local lp = Players.LocalPlayer
local char = lp.Character or lp.CharacterAdded:Wait()

-- Estados
local autoFarm = false
local autoItem = false
local autoBoss = false
local autoSea = false
local panelOpen = true

-- GUI
local gui = Instance.new("ScreenGui", lp:WaitForChild("PlayerGui"))
gui.Name = "BloxFruitsHub"
gui.ResetOnSpawn = false

-- Painel principal
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 200, 0, 250)
frame.Position = UDim2.new(0, 10, 0.3, 0)
frame.BackgroundTransparency = 0.3
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
frame.BorderSizePixel = 0

-- Layout dos botões
local layout = Instance.new("UIListLayout", frame)
layout.Padding = UDim.new(0, 5)
layout.SortOrder = Enum.SortOrder.LayoutOrder

-- Função de botão
local function createButton(text, callback)
	local btn = Instance.new("TextButton", frame)
	btn.Size = UDim2.new(1, -20, 0, 35)
	btn.Position = UDim2.new(0, 10, 0, 0)
	btn.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.Font = Enum.Font.Gotham
	btn.TextSize = 14
	btn.Text = text
	btn.BorderSizePixel = 0
	btn.AutoButtonColor = true
	btn.MouseButton1Click:Connect(callback)
	return btn
end

-- Função do Toggle Button
local toggleBtn = Instance.new("TextButton", gui)
toggleBtn.Size = UDim2.new(0, 40, 0, 40)
toggleBtn.Position = UDim2.new(0, 10, 0.25, 0)
toggleBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
toggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleBtn.Text = "≡"
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextSize = 20
toggleBtn.BorderSizePixel = 0
toggleBtn.ZIndex = 5

-- Abrir/fechar com animação
local function togglePanel()
	panelOpen = not panelOpen
	local goal = {}
	if panelOpen then
		goal.Position = UDim2.new(0, 10, 0.3, 0)
	else
		goal.Position = UDim2.new(0, -220, 0.3, 0)
	end
	TweenService:Create(frame, TweenInfo.new(0.5, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), goal):Play()
end

toggleBtn.MouseButton1Click:Connect(togglePanel)

-- Botões
createButton("Auto Farm", function()
	autoFarm = not autoFarm
end)

createButton("Auto Item", function()
	autoItem = not autoItem
end)

createButton("Auto Boss", function()
	autoBoss = not autoBoss
end)

createButton("Auto Sea", function()
	autoSea = not autoSea
end)

-- Funções principais
spawn(function()
	while task.wait(0.5) do
		if autoFarm then
			pcall(function()
				local enemy = workspace:FindFirstChild("Enemies")
				if enemy then
					for _, mob in pairs(enemy:GetChildren()) do
						if mob:FindFirstChild("HumanoidRootPart") and mob:FindFirstChild("Humanoid") and mob.Humanoid.Health > 0 then
							lp.Character.HumanoidRootPart.CFrame = mob.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
							-- ataque fictício
							break
						end
					end
				end
			end)
		end
	end
end)

spawn(function()
	while task.wait(2) do
		if autoItem then
			for _, item in pairs(workspace:GetDescendants()) do
				if item:IsA("Tool") and item.Parent == workspace then
					item.Parent = lp.Backpack
				end
			end
		end
	end
end)

spawn(function()
	while task.wait(3) do
		if autoBoss then
			local bosses = {"Boss1", "Boss2", "Boss3"} -- adapte
			for _, name in pairs(bosses) do
				local boss = workspace.Enemies:FindFirstChild(name)
				if boss and boss:FindFirstChild("Humanoid") and boss.Humanoid.Health > 0 then
					lp.Character.HumanoidRootPart.CFrame = boss.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
					break
				end
			end
		end
	end
end)

spawn(function()
	while task.wait(5) do
		if autoSea then
			local beast = workspace:FindFirstChild("SeaBeasts")
			if beast then
				for _, b in pairs(beast:GetChildren()) do
					if b:FindFirstChild("Humanoid") and b.Humanoid.Health > 0 then
						lp.Character.HumanoidRootPart.CFrame = b.HumanoidRootPart.CFrame * CFrame.new(0, 10, 0)
						break
					end
				end
			end
		end
	end
end)
