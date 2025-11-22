-- LocalScript (place in StarterPlayerScripts or StarterCharacterScripts)

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- Configuration
local VOID_Y_POSITION = -5000  -- Way below the FallenPartsDestroyer height (safe void)

-- Function to void all parts except the ones we want to keep
local function voidUnnecessaryParts()
    if not character or not character.Parent then return end
    
    for _, part in ipairs(character:GetChildren()) do
        if part:IsA("BasePart") and part ~= humanoidRootPart and part.Name ~= "Head" and part.Name ~= "Torso" then
            -- Instantly move the part to the void
            part.CFrame = CFrame.new(0, VOID_Y_POSITION, 0)
            
            -- Optional: break joints / disable collisions so it falls cleanly
            if part:FindFirstChild("BodyGyro") then part.BodyGyro:Destroy() end
            if part:FindFirstChild("BodyPosition") then part.BodyPosition:Destroy() end
            
            -- Make it fall through kill bricks / normal parts
            part.CanCollide = false
            part.Anchored = false
        end
    end
end

-- Run when character spawns
player.CharacterAdded:Connect(function(char)
    character = char
    humanoidRootPart = char:WaitForChild("HumanoidRootPart")
    
    -- Small delay to ensure all parts are loaded (especially accessories/meshes)
    task.wait(0.5)
    voidUnnecessaryParts()
end)

-- Run immediately if character already exists
if player.Character then
    task.spawn(voidUnnecessaryParts)
end
