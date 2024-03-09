local intens = 600 -- set speed boost
local boostActive = true
local vehicleLoopSpeed

local function applyImpulseToHumanoid(Humanoid, intens)
    if Humanoid:IsA("Humanoid") and Humanoid.SeatPart then
        Humanoid.SeatPart:ApplyImpulse(Humanoid.SeatPart.CFrame.LookVector * Vector3.new(0, 0, intens))
    elseif Humanoid:IsA("BasePart") then
        Humanoid:ApplyImpulse(Humanoid.CFrame.LookVector * Vector3.new(0, 0, intens))
    end
end

while boostActive do
    local localPlayer = game:GetService("Players").LocalPlayer
    local humanoid = localPlayer.Character and localPlayer.Character:FindFirstChildOfClass("Humanoid")

    if humanoid then
        if not vehicleLoopSpeed then
            vehicleLoopSpeed = game:GetService("RunService").Stepped:Connect(function()
                applyImpulseToHumanoid(humanoid, intens)
            end)
        end
    else
        if vehicleLoopSpeed then
            vehicleLoopSpeed:Disconnect()
            vehicleLoopSpeed = nil
        end
    end

    wait()
end
