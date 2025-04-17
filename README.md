local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local hrp = char:WaitForChild("HumanoidRootPart")
local cam = workspace.CurrentCamera
local UIS = game:GetService("UserInputService")
local RS = game:GetService("RunService")

local flySpeed = 60
local flying = false
local fling = false
local dir = Vector3.zero

-- Criar GUI
local gui = Instance.new("ScreenGui")
gui.Name = "NightGlitchHub"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

local function createBtn(txt, pos, callback)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(0, 150, 0, 40)
	btn.Position = pos
	btn.Text = txt
	btn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
	btn.TextColor3 = Color3.new(1, 1, 1)
	btn.Font = Enum.Font.SourceSansBold
	btn.TextScaled = true
	btn.Parent = gui
	btn.MouseButton1Click:Connect(callback)
	return btn
end

-- Botões
local flyBtn = createBtn("Ativar Fly", UDim2.new(0, 20, 0, 100), function()
	flying = not flying
	flyBtn.Text = flying and "Desativar Fly" or "Ativar Fly"
end)

local flingBtn = createBtn("Ativar Fling", UDim2.new(0, 20, 0, 160), function()
	fling = not fling
	flingBtn.Text = fling and "Desativar Fling" or "Ativar Fling"
end)

-- Movimentação
UIS.InputBegan:Connect(function(input, gpe)
	if gpe then return end
	local key = input.KeyCode
	if key == Enum.KeyCode.W then dir = Vector3.new(0, 0, -1)
	elseif key == Enum.KeyCode.S then dir = Vector3.new(0, 0, 1)
	elseif key == Enum.KeyCode.A then dir = Vector3.new(-1, 0, 0)
	elseif key == Enum.KeyCode.D then dir = Vector3.new(1, 0, 0)
	elseif key == Enum.KeyCode.Space then dir = Vector3.new(0, 1, 0)
	elseif key == Enum.KeyCode.LeftControl then dir = Vector3.new(0, -1, 0)
	end
end)

UIS.InputEnded:Connect(function(input, gpe)
	if gpe then return end
	dir = Vector3.zero
end)

RS.RenderStepped:Connect(function()
	if flying and dir.Magnitude > 0 then
		hrp.Velocity = (cam.CFrame:VectorToWorldSpace(dir)).Unit * flySpeed
	elseif not flying then
		hrp.Velocity = Vector3.zero
	end
end)

-- Fling
local flingPower = 4000
RS.Heartbeat:Connect(function()
	if fling and hrp then
		local direction = (hrp.Position - cam.CFrame.Position).Unit
		hrp.Velocity = direction * flingPower
	end
end)
# Script-
