local CC = game:GetService('Workspace').CurrentCamera

local Active = true

local Enabled = true

local Prediction = 0.100

local D = false

local Mouse = game.Players.LocalPlayer:GetMouse()

local LocalPlayer = game.Players.LocalPlayer

local Plr

local T = {}



setmetatable(T, {

    __index = function(t,k)

        if k == 'Ball' then

            return game.Workspace.Ball.BallPart

        end

    end,

})

local A = T.Ball



Mouse.KeyDown:Connect(function(k)

    if k ~= 'e' then return end

    if Active then

        if Enabled then

            Enabled = false

        else

            Enabled = true

            Plr = GetPlayerNearMouse()

            if Plr then

            else

            end

        end

    end

end)





function GetPlayerNearMouse()

    local closestPlayer

    local shortestDistance = math.huge

    local cameraDirection = CC.CFrame.LookVector



    for _, player in ipairs(game.Workspace.Ball:GetChildren()) do

        if player and player:IsA("MeshPart") then

            local playerHead = player

            local playerDirection = (playerHead.Position - CC.CFrame.Position).unit

            local dot = cameraDirection:Dot(playerDirection)



            if dot &gt; 0 then

                local pos = CC:WorldToViewportPoint(playerHead.Position)

                local magnitude = (Vector2.new(pos.X, pos.Y) - Vector2.new(Mouse.X, Mouse.Y)).magnitude

                if magnitude &lt; shortestDistance then

                    closestPlayer = player

                    shortestDistance = magnitude

                end

            end

        end

    end



    return closestPlayer

end







game:GetService("RunService").RenderStepped:Connect(function()

    if Enabled and Plr and not D then

        game.Workspace.CurrentCamera.CFrame = CFrame.new(game.Workspace.CurrentCamera.CFrame.Position, Plr.Position + Plr.Velocity * Prediction)

    elseif Enabled and D and Plr then

        if Plr:FindFirstChild("HumanoidRootPart") then

            local root = Plr.HumanoidRootPart

            local currentPosition = root.Position

            local currentTime = tick()



            wait(0.00350)



            local newPosition = root.Position

            local newTime = tick()



            local distanceTraveled = (newPosition - currentPosition)



            local timeInterval = newTime - currentTime

            local velocity = distanceTraveled / timeInterval

            currentPosition = newPosition

            currentTime = newTime



            game.Workspace.CurrentCamera.CFrame = CFrame.new(game.Workspace.CurrentCamera.CFrame.Position, currentPosition + velocity * 0.150)

        end

    end

end)
