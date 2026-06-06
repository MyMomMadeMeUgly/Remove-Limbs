-- LocalScript

local Players = game:GetService("Players")
local player = Players.LocalPlayer

local hasRun = false
local VOID_Y_POSITION = -5000

local function voidUnnecessaryParts(character)
	if hasRun then
		return
	end
	hasRun = true

	local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

	for _, part in ipairs(character:GetChildren()) do
		if part:IsA("BasePart")
			and part ~= humanoidRootPart
			and part.Name ~= "Head"
			and part.Name ~= "Torso" then

			part.CFrame = CFrame.new(0, VOID_Y_POSITION, 0)

			if part:FindFirstChild("BodyGyro") then
				part.BodyGyro:Destroy()
			end

			if part:FindFirstChild("BodyPosition") then
				part.BodyPosition:Destroy()
			end

			part.CanCollide = false
			part.Anchored = false
		end
	end
end

local character = player.Character or player.CharacterAdded:Wait()
task.wait(0.5)
voidUnnecessaryParts(character)
