local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local rootPart = character:WaitForChild("HumanoidRootPart")

local weaving = false
local weaveDirection = 3
local weaveAmount = 15 -- studs to move left/right
local weaveSpeed = 15 -- how fast to weave
local weaveInterval = 0.3 -- seconds between direction changes

local function weaveStep()
    local startPos = rootPart.Position
    local leftPos = startPos + rootPart.CFrame.RightVector * weaveAmount
    local rightPos = startPos - rootPart.CFrame.RightVector * weaveAmount

    while weaving do
        local targetPos = (weaveDirection == 1) and leftPos or rightPos
        local elapsed = 0
        while elapsed < weaveInterval and weaving do
            local dt = RunService.Heartbeat:Wait()
            elapsed = elapsed + dt
            local alpha = elapsed / weaveInterval
            local newPos = startPos:Lerp(targetPos, alpha)
            rootPart.CFrame = CFrame.new(newPos, newPos + rootPart.CFrame.LookVector)
        end
        weaveDirection = -weaveDirection
        startPos = rootPart.Position
    end
end

local function startWeave(botton)
    if weaving then return end
    weaving = true
    coroutine.wrap(weaveStep)()
end

local function stopWeave(botton)
    weaving = false
end

local autoWeave = false
local autoWeaveConnection

local function toggleAutoWeave(botton)
    autoWeave = not autoWeave
    if autoWeave then
        startWeave(botton)
        autoWeaveConnection = RunService.Heartbeat:Connect(function()
            -- You can add auto attack or other logic here while weaving
        end)
    else
        stopWeave(botton)
        if autoWeaveConnection then
            autoWeaveConnection:Disconnect()
            autoWeaveConnection = nil
        end
    end
end

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.E then
        toggleAutoWeave(botton)
    end
end)
