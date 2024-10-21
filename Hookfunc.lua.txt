task.delay(15, function() 
    if hookfunction and not islclosure(hookfunction) then 
        workspace._WorldOrigin.ChildAdded:Connect(function(v)
            if v.Name == 'DamageCounter' then 
                v.Enabled = false 
            end
        end)

        hookfunction(require(game:GetService("ReplicatedStorage").Effect.Container.Death), function() end)
        hookfunction(require(game:GetService("ReplicatedStorage").Effect.Container.Respawn), function() end)
        hookfunction(require(game:GetService("ReplicatedStorage"):WaitForChild("GuideModule")).ChangeDisplayedNPC, function() end)
        
        task.spawn(function()
            local NGU, NGUVL
            repeat 
                NGU, NGUVL = pcall(function()
                    for i, v in pairs(getupvalues(require(game:GetService("Players").LocalPlayer.PlayerScripts.CombatFramework))[2].activeController.data) do  
                        if typeof(v) == 'function' then 
                            hookfunction(v, function() end)
                        end
                    end
                end)
                task.wait(1.5)
            until NGU 
        end)
    end 
end)
abc = true
task.spawn(function()
    local a = game.Players.LocalPlayer
    local b = require(a.PlayerScripts.CombatFramework.Particle)
    local c = require(game:GetService("ReplicatedStorage").CombatFramework.RigLib)
    
    if not shared.orl then
        shared.orl = c.wrapAttackAnimationAsync
    end
    if not shared.cpc then
        shared.cpc = b.play
    end
    
    if abc then
        pcall(function()
            c.wrapAttackAnimationAsync = function(d, e, f, g, h)
                local i = c.getBladeHits(
                    a.Character,
                    {a.Character.HumanoidRootPart},
                    60
                )
                if i then
                    b.play = function()
                    end
                    d:Play(0.1, 0.1, 0.1)
                    h(i)
                    b.play = shared.cpc
                    wait(.5)
                    d:Stop()
                end
            end
        end)
    end
end)
