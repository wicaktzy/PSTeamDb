-- Hyper Stealer V2 (OrionLib UI)
-- Load OrionLib
local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Orion/main/source"))()
local Window = OrionLib:MakeWindow({Name = "⚡ Hyper Stealer Hub V2", HidePremium = true, SaveConfig = true, ConfigFolder = "HyperStealerV2"})

local Main = Window:MakeTab({Name = "Main", Icon = "rbxassetid://4483345998"})
local Other = Window:MakeTab({Name = "Other", Icon = "rbxassetid://6022668885"})

local noclip, autofarm, espEnabled, bypass, serverHop, rejoin, fpsBoost, clearLag, antiVoid, antiAfk, setRespawn
local speed = 30
local jump = 50
local respawnPoint = nil

-- Core functions
local RS = game:GetService("RunService")
local Players = game:GetService("Players")
local lp = Players.LocalPlayer

-- Noclip
RS.Stepped:Connect(function()
    if noclip and lp.Character then
        for _,v in ipairs(lp.Character:GetDescendants()) do
            if v:IsA("BasePart") then v.CanCollide = false end
        end
    end
end)

-- ESP
RS.RenderStepped:Connect(function()
    if espEnabled then
        for _,plr in pairs(Players:GetPlayers()) do
            -- implement ESP drawing logic here
        end
    end
end)

-- Auto Farm Coin
Main:AddToggle({
    Name = "Auto Farm Coin",
    Default = false,
    Flag = "autofarm",
    Callback = function(v) autofarm = v end
})
spawn(function()
    while wait(1) do
        if autofarm and lp.Character then
            for _,v in pairs(workspace:GetChildren()) do
                if v.Name:match("Brainrot") and v:FindFirstChild("ProximityPrompt") then
                    fireproximityprompt(v.ProximityPrompt)
                    wait(0.3)
                end
            end
        end
    end
end)

-- UI elements
Main:AddToggle({Name = "Noclip", Default = false, Callback = function(v) noclip = v end})
Main:AddSlider({Name = "WalkSpeed", Min = 16, Max = 150, Default = speed, Increment = 1, Callback = function(v) speed = v; if lp.Character then lp.Character.Humanoid.WalkSpeed = v end end})
Main:AddSlider({Name = "JumpPower", Min = 50, Max = 200, Default = jump, Increment = 1, Callback = function(v) jump = v; if lp.Character then lp.Character.Humanoid.JumpPower = v end end})
Main:AddToggle({Name = "ESP", Default = false, Callback = function(v) espEnabled = v end})
Main:AddToggle({Name = "Bypass Anti-Cheat", Default = false, Callback = function(v) bypass = v; if bypass then
    local mt = getrawmetatable(game); setreadonly(mt, false)
    mt.__namecall = newcclosure(function(self, ...)
        local method = getnamecallmethod()
        if tostring(self):lower():find("kick") then return nil end
        return mt.__namecall(self, ...)
    end)
end end})
Main:AddButton({Name = "Rejoin Server", Callback = function()
    game:GetService("TeleportService"):Teleport(game.PlaceId, lp)
end})
Main:AddButton({Name = "Server Hop", Callback = function()
    local Http = game:GetService("HttpService")
    local servers = Http:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?sortOrder=Asc&limit=100")).data
    for _,s in ipairs(servers) do
        if s.playing < s.maxPlayers then
            game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, s.id)
            break
        end
    end
end})

Other:AddToggle({Name = "FPS Booster", Default = false, Callback = function(v)
    fpsBoost = v
    if fpsBoost then
        for _,d in pairs(workspace:GetDescendants()) do
            if d:IsA("ParticleEmitter") or d:IsA("Explosion") then d.Enabled = false end
        end
    end
end})
Other:AddToggle({Name = "Clear Lag", Default = false, Callback = function(v)
    clearLag = v
    if clearLag then
        for _,d in pairs(workspace:GetDescendants()) do
            if d:IsA("Decal") or d:IsA("ParticleEmitter") then d:Destroy() end
        end
    end
end})
spawn(function()
    while wait(300) do
        if antiAfk then
            lp.Character:MoveTo(lp.Character:GetPivot().Position + Vector3.new(0,0.1,0))
        end
    end
end)
Other:AddToggle({Name = "Anti AFK", Default = false, Callback = function(v) antiAfk = v end})
Other:AddButton({Name = "Set Respawn Point", Callback = function()
    respawnPoint = lp.Character:GetPivot().Position
end})
Other:AddButton({Name = "Teleport to Respawn", Callback = function()
    if respawnPoint then lp.Character:MoveTo(respawnPoint) end
end})
Other:AddToggle({Name = "Anti Void", Default = false, Callback = function(v) antiVoid = v; if antiVoid then
    game:GetService("RunService").Heartbeat:Connect(function()
        local root = lp.Character and lp.Character:FindFirstChild("HumanoidRootPart")
        if root and root.Position.Y < -50 then
            local part = Instance.new("Part", workspace)
            part.Size = Vector3.new(50,1,50)
            part.CFrame = CFrame.new(root.Position.X, root.Position.Y - 5, root.Position.Z)
            part.Anchored = true
            wait(1)
            part:Destroy()
        end
    end)
end end})

-- Keybind GUI toggle
Window:MakeKeybind({
    Name = "Toggle UI",
    Default = Enum.KeyCode.RightShift,
    Callback = function()
        OrionLib:Toggle()
    end
})

OrionLib:Init()
