local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Corrupt's GUI", "Ocean")

-- Main
local Main = Window:NewTab("Main")
local MainSection = Main:NewSection("Aim")



MainSection:NewButton("Lock(Q)", "ButtonInfo", function()
getgenv().Prediction = 0.143
getgenv().AimPart = "HumanoidRootPart"
getgenv().Key = "Q"
getgenv().DisableKey = "P"

getgenv().FOV = true
getgenv().ShowFOV = false
getgenv().FOVSize = 55

--// Variables (Service)

local Players = game:GetService("Players")
local RS = game:GetService("RunService")
local WS = game:GetService("Workspace")
local GS = game:GetService("GuiService")
local SG = game:GetService("StarterGui")

--// Variables (regular)

local LP = Players.LocalPlayer
local Mouse = LP:GetMouse()
local Camera = WS.CurrentCamera
local GetGuiInset = GS.GetGuiInset

local AimlockState = true
local Locked
local Victim

local SelectedKey = getgenv().Key
local SelectedDisableKey = getgenv().DisableKey

--// Notification function

--// Check if aimlock is loaded

if getgenv().Loaded == true then
    Notify("Aimlock is already loaded!")
    return
end

getgenv().Loaded = true

--// FOV Circle

local fov = Drawing.new("Circle")
fov.Filled = false
fov.Transparency = 1
fov.Thickness = 1
fov.Color = Color3.fromRGB(255, 255, 0)
fov.NumSides = 1000

--// Functions

function update()
    if getgenv().FOV == true then
        if fov then
            fov.Radius = getgenv().FOVSize * 2
            fov.Visible = getgenv().ShowFOV
            fov.Position = Vector2.new(Mouse.X, Mouse.Y + GetGuiInset(GS).Y)

            return fov
        end
    end
end

function WTVP(arg)
    return Camera:WorldToViewportPoint(arg)
end

function WTSP(arg)
    return Camera.WorldToScreenPoint(Camera, arg)
end

function getClosest()
    local closestPlayer
    local shortestDistance = math.huge

    for i, v in pairs(game.Players:GetPlayers()) do
        local notKO = v.Character:WaitForChild("BodyEffects")["K.O"].Value ~= true
        local notGrabbed = v.Character:FindFirstChild("GRABBING_COINSTRAINT") == nil
        
        if v ~= game.Players.LocalPlayer and v.Character and v.Character:FindFirstChild("Humanoid") and v.Character.Humanoid.Health ~= 0 and v.Character:FindFirstChild(getgenv().AimPart) and notKO and notGrabbed then
            local pos = Camera:WorldToViewportPoint(v.Character.PrimaryPart.Position)
            local magnitude = (Vector2.new(pos.X, pos.Y) - Vector2.new(Mouse.X, Mouse.Y)).magnitude
            
            if (getgenv().FOV) then
                if (fov.Radius > magnitude and magnitude < shortestDistance) then
                    closestPlayer = v
                    shortestDistance = magnitude
                end
            else
                if (magnitude < shortestDistance) then
                    closestPlayer = v
                    shortestDistance = magnitude
                end
            end
        end
    end
    return closestPlayer
end

--// Checks if key is down

Mouse.KeyDown:Connect(function(k)
    SelectedKey = SelectedKey:lower()
    SelectedDisableKey = SelectedDisableKey:lower()
    if k == SelectedKey then
        if AimlockState == true then
            Locked = not Locked
            if Locked then
                Victim = getClosest()

                Notify("Locked onto: "..tostring(Victim.Character.Humanoid.DisplayName))
            else
                if Victim ~= nil then
                    Victim = nil

                    Notify("Unlocked!")
                end
            end
        else
            Notify("Aimlock is not enabled!")
        end
    end
    if k == SelectedDisableKey then
        AimlockState = not AimlockState
    end
end)

--// Loop update FOV and loop camera lock onto target

RS.RenderStepped:Connect(function()
    update()
    if AimlockState == true then
        if Victim ~= nil then
            Camera.CFrame = CFrame.new(Camera.CFrame.p, Victim.Character[getgenv().AimPart].Position + Victim.Character[getgenv().AimPart].Velocity*getgenv().Prediction)
        end
    end
end)
	while wait() do
        if getgenv().AutoPrediction == true then
        local pingvalue = game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValueString()
        local split = string.split(pingvalue,'(')
        local ping = tonumber(split[1])
            if ping < 225 then
            getgenv().Prediction = 1.4
        elseif ping < 215 then
            getgenv().Prediction = 1.2
	    elseif ping < 205 then
            getgenv().Prediction = 1.0
	    elseif ping < 190 then
            getgenv().Prediction = 0.10
        elseif ping < 180 then
            getgenv().Prediction = 0.12
	    elseif ping < 170 then
            getgenv().Prediction = 0.15
	    elseif ping < 160 then
            getgenv().Prediction = 0.18
	    elseif ping < 150 then
            getgenv().Prediction = 0.110
        elseif ping < 140 then
            getgenv().Prediction = 0.113
        elseif ping < 130 then
            getgenv().Prediction = 0.116
        elseif ping < 120 then
            getgenv().Prediction = 0.120
        elseif ping < 110 then
            getgenv().Prediction = 0.124
        elseif ping < 105 then
            getgenv().Prediction = 0.127
        elseif ping < 90 then
            getgenv().Prediction = 0.130
        elseif ping < 80 then
            getgenv().Prediction = 0.133
        elseif ping < 70 then
            getgenv().Prediction = 0.136
        elseif ping < 60 then
            getgenv().Prediction = 0.140
        elseif ping < 50 then
            getgenv().Prediction = 0.143
        elseif ping < 40 then
            getgenv().Prediction = 0.145
        elseif ping < 30 then
            getgenv().Prediction = 0.155
        elseif ping < 20 then
            getgenv().Prediction = 0.157
        end
        end
	end
end)



MainSection:NewButton("silent aim(Q)", "ButtonInfo", function()
    print("Clicked")
        Settings = {
        Kalslock = {
        Enabled = true,
        Key = "q",
        showdot = true,
        airshots = true,
        notifaction = true,
        pingprediction = true,
        FOV = math.huge,
        RESOVLER = true,
        Tracer = true,
        TracerColor = Color3.new(0, 255, 238),
        TracerTransparency = 0.75,
        TracerThickness = 5,
        Circles = false,
        CircleFOV = 500,
        CircleColor = Color3.new(255, 255, 255),
        CircleThickness = 2,
        CircleFilled = true,
        CircleTransparency = 0.10,
        CircleTransparencyOutline = 0,
        OutlineColor = Color3.new(255, 255, 255),
        Texts = false, 
        TextColor = Color3.new(255, 255, 255),
        TextSize = 35,
        TextInput = "skidded",
        Hitbox = true,
        NoBulletDelay = false,
        Autoclicker = false,
        AutoclickerTime = 0.01,
        AutoclickerKey = "v",
        AnimationPack = false
 
}
}
 
--[[
    Parts - / HumanoidRootPart / LowerTorso / UpperTorso / Head 
]]
 
        local SelectedPart = "HumanoidRootPart"
        local Prediction = true
        local PredictionValue = 0.132
 
 
 
 local AnchorCount = 0
            local MaxAnchor = 50
 
                local CC = game:GetService"Workspace".CurrentCamera
                    local Plr;
                        local enabled = false
                            local accomidationfactor = 0.132
                local mouse = game.Players.LocalPlayer:GetMouse()
 
                            local Inset = game:GetService("GuiService"):GetGuiInset().Y
                local Line = Drawing.new("Line")
                    local Text = Drawing.new("Text")
                          local Circle = Drawing.new("Circle")
                          local Circle1 = Drawing.new("Circle")
 
 
                           _G.Types = {
            Ball = Enum.PartType.Ball,
            Block = Enum.PartType.Block, 
            Cylinder = Enum.PartType.Cylinder
        }
 
                          local placemarker = Instance.new("Part", game.Workspace)
                          placemarker.Shape = _G.Types.Ball
                          placemarker.Material = "ForceField"
                          placemarker.Color = Color3.new(0, 1, 1)
 
    function makemarker(Parent, Adornee, Color, Size, Size2)
        local e = Instance.new("BillboardGui", Parent)
        e.Name = "PP"
        e.Adornee = Adornee
        e.Size = UDim2.new(Size, Size2, Size, Size2)
        e.AlwaysOnTop = Settings.Kalslock.showdot
        local a = Instance.new("Frame", e)
        if Settings.Kalslock.showdot == true then
        a.Size = UDim2.new(1, 0, 1, 0)
        else
        a.Size = UDim2.new(0, 0, 0, 0)
        end
        if Settings.Kalslock.showdot == true then
        a.Transparency = 0
        a.BackgroundTransparency = 0
        else
        a.Transparency = 1
        a.BackgroundTransparency = 1
        end
        a.BackgroundColor3 = Color
        local g = Instance.new("UICorner", a)
        if Settings.Kalslock.showdot == false then
        g.CornerRadius = UDim.new(0, 0)
        else
        g.CornerRadius = UDim.new(1, 1) 
        end
        return(e)
    end
 
 
    local data = game.Players:GetPlayers()
    function noob(player)
        local character
        repeat wait() until player.Character
        local handler = makemarker(guimain, player.Character:WaitForChild(SelectedPart), Color3.fromRGB(0, 255, 229), 0.3, 3)
        handler.Name = player.Name
        player.CharacterAdded:connect(function(Char) handler.Adornee = Char:WaitForChild(SelectedPart) end)
 
 
        spawn(function()
            while wait() do
                if player.Character then
                end
            end
        end)
    end
 
    for i = 1, #data do
        if data[i] ~= game.Players.LocalPlayer then
            noob(data[i])
        end
    end
 
    game.Players.PlayerAdded:connect(function(Player)
        noob(Player)
    end)
 
    spawn(function()
        placemarker.Anchored = true
        placemarker.CanCollide = false
        if Settings.Kalslock.showdot == true then
        placemarker.Size = Vector3.new(11, 11, 11)
        else
        placemarker.Size = Vector3.new(0, 0, 0)
        end
        if Settings.Kalslock.Hitbox == true then
        placemarker.Transparency = 0.30
        else
        placemarker.Transparency = 1
        end
        if Settings.Kalslock.showdot then
        makemarker(placemarker, placemarker, Color3.fromRGB(0, 255, 238), 0.80, 0)
        end
    end)
 
    game.Players.LocalPlayer:GetMouse().KeyDown:Connect(function(k)
        if k == Settings.Kalslock.Key and Settings.Kalslock.Enabled then
            if enabled == true then
                enabled = false
                if Settings.Kalslock.notifaction == true then
                    Plr = getClosestPlayerToCursor()
                game.StarterGui:SetCore("SendNotification", {
                    Title = "Kal Skidded This";
                    Text = "  Unlocked",
                    Icon = "http://www.roblox.com/asset/?id=5314810647",
                    Duration = 3
                })
            end
            else
                Plr = getClosestPlayerToCursor()
                enabled = true
                if Settings.Kalslock.notifaction == true then
 
                    game.StarterGui:SetCore("SendNotification", {
                        Title = "Kal Skidded This";
                        Text = "  Target: "..tostring(Plr.Character.Humanoid.DisplayName),
                        Icon = "http://www.roblox.com/asset/?id=8709610734",
                        Duration = 3
                    })
 
                end
            end
        end
    end)
 
 
 
    function getClosestPlayerToCursor()
        local closestPlayer
        local shortestDistance = Settings.Kalslock.FOV
 
        for i, v in pairs(game.Players:GetPlayers()) do
            if v ~= game.Players.LocalPlayer and v.Character and v.Character:FindFirstChild("Humanoid") and v.Character.Humanoid.Health ~= 0 and v.Character:FindFirstChild("HumanoidRootPart") then
                local pos = CC:WorldToViewportPoint(v.Character.PrimaryPart.Position)
                local magnitude = (Vector2.new(pos.X, pos.Y) - Vector2.new(mouse.X, mouse.Y)).magnitude
                if magnitude < shortestDistance then
                    closestPlayer = v
                    shortestDistance = magnitude
                end
            end
        end
        return closestPlayer
    end
 
    local pingvalue = nil;
    local split = nil;
    local ping = nil;
 
    game:GetService"RunService".Stepped:connect(function()
        if enabled and Plr.Character ~= nil and Plr.Character:FindFirstChild("HumanoidRootPart") then
            placemarker.CFrame = CFrame.new(Plr.Character.HumanoidRootPart.Position)
                            local Vector1 = CC:WorldToViewportPoint(Plr.Character.HumanoidRootPart.Position)
            Line.Color = Settings.Kalslock.TracerColor
                Line.Transparency = Settings.Kalslock.TracerTransparency
                Line.Thickness = Settings.Kalslock.TracerThickness
                Line.From = Vector2.new(mouse.X, mouse.Y + Inset)
                Line.To = Vector2.new(Vector1.X, Vector1.Y)
                Line.Visible = Settings.Kalslock.Tracer
                Text.Position = Vector2.new(mouse.X, mouse.Y + Inset)
                Text.Visible = Settings.Kalslock.Texts
                Text.Center = true
                Text.Outline = true
                Text.Font = 1
                Text.Size = Settings.Kalslock.TextSize
                Text.Color = Settings.Kalslock.TextColor
                Text.Text = Settings.Kalslock.TextInput
                Circle.Position = Vector2.new(mouse.X, mouse.Y + Inset)
                Circle.Visible = Settings.Kalslock.Circles
                Circle.Thickness = 1.5
                Circle.Thickness = Settings.Kalslock.CircleThickness
                Circle.Radius = Settings.Kalslock.CircleFOV
                Circle.Color = Settings.Kalslock.CircleColor
                Circle.Filled = Settings.Kalslock.CircleFilled
                Circle.Transparency = Settings.Kalslock.CircleTransparency
                Circle1.Visible = true
                Circle1.Radius = Settings.Kalslock.CircleTransparencyOutline
                Circle1.Position = Vector2.new(mouse.X, mouse.Y + Inset)
                Circle1.Thickness = 5
                Circle1.Color = Settings.Kalslock.OutlineColor
 
        else
                Circle.Visible = false
                Line.Visible = false
                Text.Visible = false
                Circle1.Visible = false
            placemarker.CFrame = CFrame.new(0, 9999, 0)
        end
        if Settings.Kalslock.pingprediction == true then
             pingvalue = game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValueString()
             split = string.split(pingvalue,'(')
             ping = tonumber(split[1])
            if ping < 130 then
                PredictionValue = 0.151
            elseif ping < 125 then
                PredictionValue = 0.149
            elseif ping < 110 then
                PredictionValue = 0.146
            elseif ping < 105 then
                PredictionValue = 0.138
            elseif ping < 90 then
                PredictionValue = 0.136
            elseif ping < 80 then
                PredictionValue = 0.134
            elseif ping < 70 then
                PredictionValue = 0.131
            elseif ping < 60 then
                PredictionValue = 0.1229
            elseif ping < 50 then
                PredictionValue = 0.1225
            elseif ping < 40 then
                PredictionValue = 0.1256
            end
        end
    end)
 
    local mt = getrawmetatable(game)
    local old = mt.__namecall
    setreadonly(mt, false)
    mt.__namecall = newcclosure(function(...)
        local args = {...}
        if enabled and getnamecallmethod() == "FireServer" and args[2] == "UpdateMousePos" and Settings.Kalslock.Enabled and Plr.Character ~= nil then
 
            -- args[3] = Plr.Character.HumanoidRootPart.Position+(Plr.Character.HumanoidRootPart.Velocity*accomidationfactor)
            --[[
            if Settings.Kalslock.airshots == true then
                if game.Workspace.Players[Plr.Name].Humanoid:GetState() == Enum.HumanoidStateType.Freefall then -- Plr.Character:WaitForChild("Humanoid"):GetState() == Enum.HumanoidStateType.Freefall
 
                    --// Airshot
                    args[3] = Plr.Character.LeftFoot.Position+(Plr.Character.LeftFoot.Velocity*PredictionValue)
 
                else
                    args[3] = Plr.Character.HumanoidRootPart.Position+(Plr.Character.HumanoidRootPart.Velocity*PredictionValue)
 
                end
            else
    args[3] = Plr.Character.HumanoidRootPart.Position+(Plr.Character.HumanoidRootPart.Velocity*PredictionValue)
    end
    ]]
    if Prediction == true then
 
    args[3] = Plr.Character[SelectedPart].Position+(Plr.Character[SelectedPart].Velocity*PredictionValue)
 
    else
 
    args[3] = Plr.Character[SelectedPart].Position
 
    end
 
    return old(unpack(args))
    end
    return old(...)
    end)
 
    game:GetService("RunService").RenderStepped:Connect(function()
        if Settings.Kalslock.RESOVLER == true and Plr.Character ~= nil and enabled and Settings.Kalslock.Enabled then
        if Settings.Kalslock.airshots == true and enabled and Plr.Character ~= nil then
 
        if game.Workspace.Players[Plr.Name].Humanoid:GetState() == Enum.HumanoidStateType.Freefall then -- Plr.Character:WaitForChild("Humanoid"):GetState() == Enum.HumanoidStateType.Freefall
 
 
 
        if Plr.Character ~= nil and Plr.Character.HumanoidRootPart.Anchored == true then
        AnchorCount = AnchorCount + 1
        if AnchorCount >= MaxAnchor then
        Prediction = false
        wait(2)
        AnchorCount = 0;
        end
        else
        Prediction = true
        AnchorCount = 0;
        end
 
        SelectedPart = "LeftFoot"
 
        else
 
 
        if Plr.Character ~= nil and Plr.Character.HumanoidRootPart.Anchored == true then
        AnchorCount = AnchorCount + 1
        if AnchorCount >= MaxAnchor then
        Prediction = false
        wait(2)
        AnchorCount = 0;
        end
 
        else
        Prediction = true
        AnchorCount = 0;
        end
 
        SelectedPart = "HumanoidRootPart"
 
        end
        else
 
 
 
        if Plr.Character ~= nil and Plr.Character.HumanoidRootPart.Anchored == true then
        AnchorCount = AnchorCount + 1
        if AnchorCount >= MaxAnchor then
        Prediction = false
        wait(2)
        AnchorCount = 0;
        end
        else
         Prediction = true
         AnchorCount = 0;
        end
        SelectedPart = "HumanoidRootPart"
        end
        else
        SelectedPart = "HumanoidRootPart"
        end
        end)
 
        if Settings.Kalslock.NoBulletDelay == true then
            local FuckDelay = game:GetService("CorePackages").Packages
            FuckDelay:Destroy()
        end
 
        if Settings.Kalslock.Autoclicker == true then
            local time = Settings.Kalslock.AutoclickerTime --decrease if too slow increase if too fast
 
            click = false
            m = game.Players.LocalPlayer:GetMouse()
            m.KeyDown:connect(function(key)
            if key == Settings.Kalslock.AutoclickerKey then
            if click == true then click = false
            elseif
            click == false then click = true
 
            while click == true do 
            wait(time)
            mouse1click()
            end
            end
            end
            end)
        end
    end)


MainSection:NewButton("anti lock", "ButtonInfo", function()
    print("Clicked")
--< Aimviewer Settings >--
AimviewerToggleKeybind = "z" -- Aimviewer toggle keybind
AimviewerSwitchKeybind = "x" -- Switch player keybind
AimviewerColor = Color3.fromRGB(28, 56, 139) -- Color of Aimviewer beam
AimviewerBeamWidth = 0.4 -- Width of Aimviewer beam
AimviewerMaterial = "ForceField" -- Material of Aimviewer beam (if material is invalid it will default to neon)


--< Prediction Settings >--
PredictionKeybind = "v" -- Prediction toggle keybind
PredictionMultiplier = 3 -- Prediction Multiplier

--< Antilock >--
AntiKeybind = "c" -- Antilock toggle keybind
AntiVelocity = 1450,0,0 -- Only change this if yk what youre doing

--< Miscallaneous >--
icon = "rbxassetid://12319510751" -- Notification Image
console = true -- Console Toggle [True = Enabled, False = Disabled]
consolename = ".gg/antilock" -- Console Name
consolecolor = "@@BLUE@@" -- Console Text Color
--------------------------------------------------------------------------------
loadstring(game:HttpGet("https://raw.githubusercontent.com/fear0004/script/main/ggantilock"))()
end)



local MainSection = Main:NewSection("movement")


MainSection:NewButton("fly(c)", "ButtonInfo", function()
    print("Clicked")
local plr = game.Players.LocalPlayer
local mouse = plr:GetMouse()

localplayer = plr

if workspace:FindFirstChild("Core") then
workspace.Core:Destroy()
end

local Core = Instance.new("Part")
Core.Name = "Core"
Core.Size = Vector3.new(0.05, 0.05, 0.05)

spawn(function()
Core.Parent = workspace
local Weld = Instance.new("Weld", Core)
Weld.Part0 = Core
Weld.Part1 = localplayer.Character.LowerTorso
Weld.C0 = CFrame.new(0, 0, 0)
end)

workspace:WaitForChild("Core")

local torso = workspace.Core
flying = true
local speed=10
local keys={a=false,d=false,w=false,s=false}
local e1
local e2
local function start()
local pos = Instance.new("BodyPosition",torso)
local gyro = Instance.new("BodyGyro",torso)
pos.Name="EPIXPOS"
pos.maxForce = Vector3.new(math.huge, math.huge, math.huge)
pos.position = torso.Position
gyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
gyro.cframe = torso.CFrame
repeat
wait()
localplayer.Character.Humanoid.PlatformStand=true
local new=gyro.cframe - gyro.cframe.p + pos.position
if not keys.w and not keys.s and not keys.a and not keys.d then
speed=5
end
if keys.w then
new = new + workspace.CurrentCamera.CoordinateFrame.lookVector * speed
speed=speed+0
end
if keys.s then
new = new - workspace.CurrentCamera.CoordinateFrame.lookVector * speed
speed=speed+0
end
if keys.d then
new = new * CFrame.new(speed,0,0)
speed=speed+0
end
if keys.a then
new = new * CFrame.new(-speed,0,0)
speed=speed+0
end
if speed>10 then
speed=5
end
pos.position=new.p
if keys.w then
gyro.cframe = workspace.CurrentCamera.CoordinateFrame*CFrame.Angles(-math.rad(speed*0),0,0)
elseif keys.s then
gyro.cframe = workspace.CurrentCamera.CoordinateFrame*CFrame.Angles(math.rad(speed*0),0,0)
else
gyro.cframe = workspace.CurrentCamera.CoordinateFrame
end
until flying == false
if gyro then gyro:Destroy() end
if pos then pos:Destroy() end
flying=false
localplayer.Character.Humanoid.PlatformStand=false
speed=10
end
e1=mouse.KeyDown:connect(function(key)
if not torso or not torso.Parent then flying=false e1:disconnect() e2:disconnect() return end
if key=="w" then
keys.w=true
elseif key=="s" then
keys.s=true
elseif key=="a" then
keys.a=true
elseif key=="d" then
keys.d=true
elseif key=="c" then
if flying==true then
flying=false
else
flying=true
start()
end
end
end)
e2=mouse.KeyUp:connect(function(key)
if key=="w" then
keys.w=false
elseif key=="s" then
keys.s=false
elseif key=="a" then
keys.a=false
elseif key=="d" then
keys.d=false
end
end)
start()
end)

MainSection:NewButton("bike fly", "ButtonInfo", function()
    print("Clicked")
local speed = 20
	local old
	local Plr = game.Players.LocalPlayer
	local wheels = {}
	local control = {q=false,e=false,w=false,a=false,s=false,d=false}
	local Mouse = Plr:GetMouse()

	Mouse.KeyDown:connect(function(KEY)
		local key = KEY:lower()
		if control[key] ~= nil then
			control[key]=true
		end
	end)

	Mouse.KeyUp:connect(function(KEY)
		local key = KEY:lower()
		if control[key] ~= nil then
			control[key]=false
		end
	end)

	while game.RunService.Stepped:wait() do
		local seat = (Plr.Character or Plr.CharacterAdded:wait()):WaitForChild("Humanoid").SeatPart
		if Plr.PlayerGui:FindFirstChild("MainScreenGui") and Plr.PlayerGui.MainScreenGui:FindFirstChild("Bar") and Plr.PlayerGui.MainScreenGui.Bar:FindFirstChild("Speed") then
			Plr.PlayerGui.MainScreenGui.Bar.Speed.bar.Size = UDim2.new(speed / 100 * 0.95, 0, 0.83, 0)
		else
			local c = Plr.PlayerGui.MainScreenGui.Bar.HP
			local g = c:Clone()
			g.Name = "Speed"
			g.Position = UDim2.new(0.5, 0, 1, -120)
			g.bar.BackgroundColor3 = Color3.fromRGB(255, 155, 0)
			g.Picture.Image.Image = "rbxassetid://181035717"
			g.TextLabel.Text = "Speed"
			g.Parent = c.Parent
		end
		if seat ~= nil and seat:IsDescendantOf(game.Workspace.Vehicles) then
			if seat ~= old then
				if old then
					old.Vel:Destroy()
					old.Gyro:Destroy()
				end
				old = seat
				for i = 1, 2 do
					if wheels[i] then
						wheels[i][2].Parent = wheels[i][1]
					end
					local wheel = seat.Parent.Wheel
					wheels[i] = {seat.Parent, wheel}
					wheel:remove()
				end
				local gyro = Instance.new("BodyGyro", seat)
				gyro.Name = "Gyro"
				local pos = Instance.new("BodyVelocity", seat)
				pos.Name = "Vel"
				gyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
				pos.MaxForce = Vector3.new(9e9, 9e9, 9e9)
			else 
				seat.Gyro.cframe = workspace.CurrentCamera.CoordinateFrame
				local vel = CFrame.new(0, 0, 0)
				if control.w then
					vel = vel * CFrame.new(0, 0, -speed)
				end
				if control.s then
					vel = vel * CFrame.new(0, 0, speed)
				end
				if control.a then
					vel = vel * CFrame.new(-speed, 0, 0)
				end
				if control.d then
					vel = vel * CFrame.new(speed, 0, 0)
				end
				seat.Vel.Velocity = (seat.CFrame*vel).p - seat.CFrame.p
			end
		end
		if control.e and speed<100 then
			speed = speed + 1
		end
		if control.q and speed > 0 then
			speed = speed - 1
		end
	end
end)

MainSection:NewButton("noclip", "ButtonInfo", function()
    print("Clicked")
local Noclip = nil
local Clip = nil

function noclip()
	Clip = false
	local function Nocl()
		if Clip == false and game.Players.LocalPlayer.Character ~= nil then
			for _,v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
				if v:IsA('BasePart') and v.CanCollide and v.Name ~= floatName then
					v.CanCollide = false
				end
			end
		end
		wait(0.21) -- basic optimization
	end
	Noclip = game:GetService('RunService').Stepped:Connect(Nocl)
end

function clip()
	if Noclip then Noclip:Disconnect() end
	Clip = true
end

noclip() -- to toggle noclip() and clip()
 end)

MainSection:NewButton("inf jump", "ButtonInfo", function()
    print("Clicked")
local Player = game:GetService'Players'.LocalPlayer;
local UIS = game:GetService'UserInputService';
 
_G.JumpHeight = 50;
 
function Action(Object, Function) if Object ~= nil then Function(Object); end end
 
UIS.InputBegan:connect(function(UserInput)
    if UserInput.UserInputType == Enum.UserInputType.Keyboard and UserInput.KeyCode == Enum.KeyCode.Space then
        Action(Player.Character.Humanoid, function(self)
            if self:GetState() == Enum.HumanoidStateType.Jumping or self:GetState() == Enum.HumanoidStateType.Freefall then
                Action(self.Parent.HumanoidRootPart, function(self)
                    self.Velocity = Vector3.new(0, _G.JumpHeight, 0);
                end)
            end
        end)
    end
end)
end)

local MainSection = Main:NewSection("player")


MainSection:NewButton("ESP", "ButtonInfo", function()
    print("Clicked")
loadstring(game:HttpGet("https://pastebin.com/raw/c4FNAXCW"))()
loadstring(game:HttpGet("https://pastebin.com/raw/Cx4B9rNd"))()
loadstring(game:HttpGet("https://pastebin.com/raw/nq27eCVX"))()
loadstring(game:HttpGet("https://pastebin.com/raw/Nj4dG8CZ"))()
end)

MainSection:NewButton("trashtalk(y)", "ButtonInfo", function()
    print("Clicked")
local words = {
    'ur bad',
    'seed',
    'im not locking ur just bad',
    'kid im not locking XDXDXDXD ur just bad',
    'sad',
    'sonned',
    'how did u fail to get me',
}

local player = game.Players.LocalPlayer
local keybind = 'y'

local event = game.ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest

player:GetMouse().KeyDown:connect(function(key)
    if key == keybind then
        event:FireServer(words[math.random(#words)], "All")
    end
end)
end)

MainSection:NewButton("fake tryhard", "ButtonInfo", function()
    print("Clicked")
loadstring(game:HttpGet("https://raw.githubusercontent.com/LunarWareOP/Animation-Changer-Source/main/Script", true))()
end)

MainSection:NewButton("headless/korblox (v)", "ButtonInfo", function()
    print("Clicked")
loadstring(game:HttpGet("https://raw.githubusercontent.com/y10ko/Niggaless/main/FE%20Headless%20%26%20Korblox",true))()
end)



-- CFrame
local speed = Window:NewTab("Speed")
local rs = speed:NewSection("CFrame Speed")
rs:NewButton("CFrame Guns FIX", "ButtonInfo", function()
    for _, v in pairs(game.Players.LocalPlayer.Character:GetChildren()) do
        if v:IsA("Script") and v.Name ~= "Health" and v.Name ~= "Sound" and v:FindFirstChild("LocalScript") then
            v:Destroy()
        end
    end
    game.Players.LocalPlayer.CharacterAdded:Connect(function(char)
        repeat
            wait()
        until game.Players.LocalPlayer.Character
        char.ChildAdded:Connect(function(child)
            if child:IsA("Script") then 
                wait(0.1)
                if child:FindFirstChild("LocalScript") then
                    child.LocalScript:FireServer()
                end
            end
        end)
    end)
end)
rs:NewButton("CFrame Speed (Z)", "ButtonInfo", function()
        repeat
        wait()
    until game:IsLoaded()
    local L_134_ = game:service('Players')
    local L_135_ = L_134_.LocalPlayer
    repeat
        wait()
    until L_135_.Character
    local L_136_ = game:service('UserInputService')
    local L_137_ = game:service('RunService')
    getgenv().Multiplier = 0.5
    local L_138_ = true
    local L_139_
    L_136_.InputBegan:connect(function(L_140_arg0)
        if L_140_arg0.KeyCode == Enum.KeyCode.LeftBracket then
            Multiplier = Multiplier + 0.01
            print(Multiplier)
            wait(0.2)
            while L_136_:IsKeyDown(Enum.KeyCode.LeftBracket) do
                wait()
                Multiplier = Multiplier + 0.01
                print(Multiplier)
            end
        end
        if L_140_arg0.KeyCode == Enum.KeyCode.RightBracket then
            Multiplier = Multiplier - 0.01
            print(Multiplier)
            wait(0.2)
            while L_136_:IsKeyDown(Enum.KeyCode.RightBracket) do
                wait()
                Multiplier = Multiplier - 0.01
                print(Multiplier)
            end
        end
        if L_140_arg0.KeyCode == Enum.KeyCode.Z then
            L_138_ = not L_138_
            if L_138_ == true then
                repeat
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame + game.Players.LocalPlayer.Character.Humanoid.MoveDirection * Multiplier
                    game:GetService("RunService").Stepped:wait()
                until L_138_ == false
            end
        end
    end)
end)
rs:NewSlider("CFrame Speed ", "SliderInfo", 5, 0, function(s) -- 500 (MaxValue) | 0 (MinValue)
    getgenv().Multiplier = s
end)
rs:NewKeybind("Toggle UI", "KeybindInfo", Enum.KeyCode.V, function()
    Library.ToggleUI()
end)





local MainSection = Main:NewSection("main")
MainSection:NewButton("auto armor", "ButtonInfo", function()
    print("Clicked")
    local PERCENT_TO_BUY_ARMOR   = 50         --\\ percent of armor left that u want to buy
local COMMAND_TO_STOP_BUYING = ('/e stop') --\\ message to stop buying

------------------------
------------------------

function announce(title,text,time)
    game.StarterGui:SetCore("SendNotification", {
        Title = title;
        Text = text;
        Duration = time;
    })
end
announce('Autobuying armor at %' .. tostring(PERCENT_TO_BUY_ARMOR), 'chat ' .. COMMAND_TO_STOP_BUYING .. ' to stop', 8)

local Stopped = false
local Player = game.Players.LocalPlayer
function Main1()
    while wait() do
        local function AutoArmor()
            local Origin = Player.Character.HumanoidRootPart.CFrame
            local Armor = Player.Character.BodyEffects.Armor
            if Armor.Value <= PERCENT_TO_BUY_ARMOR and Stopped == false then
                repeat
                    wait()    
                    Player.Character.HumanoidRootPart.CFrame = game:GetService("Workspace").Ignored.Shop["[Medium Armor] - $1000"].Head.CFrame
                    fireclickdetector(game:GetService("Workspace").Ignored.Shop["[Medium Armor] - $1000"].ClickDetector)
                until Armor.Value == 100 or Player.DataFolder.Currency.Value < 1000
                Player.Character.HumanoidRootPart.CFrame = Origin
            end
        end
        local s,e = pcall(AutoArmor)
    end
end
function Main2()
    Player.Chatted:Connect(function(msg)
        if msg == COMMAND_TO_STOP_BUYING and Stopped == false then
            Stopped = true
            announce('stopped buying', '',5)
        end
    end)
end
coroutine.resume(coroutine.create(Main1))
coroutine.resume(coroutine.create(Main2))
end)

MainSection:NewButton("auto block", "ButtonInfo", function()
    print("Clicked")
    --[[




]]--


MainEvent = game:GetService('ReplicatedStorage').MainEvent
player = game.Players.LocalPlayer;
Distancia = 15; -- Here put the distance to activate

game:GetService('RunService'):BindToRenderStep("Auto-Block", 0 , function()

    local forbidden = {'[Popcorn]','[HotDog]','[GrenadeLauncher]','[RPG]','[SMG]','[TacticalShotgun]','[AK47]','[AUG]','[Glock]', '[Shotgun]','[Flamethrower]','[Silencer]','[AR]','[Revolver]','[SilencerAR]','[LMG]','[P90]','[DrumGun]','[Double-Barrel SG]','[Hamburger]','[Chicken]','[Pizza]','[Cranberry]','[Donut]','[Taco]','[Starblox Latte]','[BrownBag]','[Weights]','[HeavyWeights]'}
local Found = false
for _,v in pairs(game.Workspace.Players:GetChildren()) do
    if (v.UpperTorso.Position - player.Character.HumanoidRootPart.Position).Magnitude <= Distancia then
        if v.BodyEffects.Attacking.Value == true and not table.find(forbidden, v:FindFirstChildWhichIsA('Tool').Name ) and v.Name ~= player.Name then
            Found = true
            MainEvent:FireServer('Block', player.Name)
            
        end
    end
end
if Found == false then
    if player.Character.BodyEffects:FindFirstChild('Block') then player.Character.BodyEffects.Block:Destroy() end
    end
end)
end)

MainSection:NewButton("autofarm", "ButtonInfo", function()
    print("Clicked")
loadstring(game:HttpGet("https://raw.githubusercontent.com/EpicPug/Stuff/main/AutoFarmsV2.lua"))()
end)

MainSection:NewButton("chat spy", "ButtonInfo", function()
    print("Clicked")
--[[
	Simple Chat Spy
	Type "spy" to enable or disable the chat spy.
	Only tested if this works executed with Synapse (should work with other exploits though)
--]]

print("-- Chat Spy Executed --")
print("Type \"spy\" to enable or disable the chat spy.")
print("Only tested if this works executed with Synapse (should work with other exploits though)")
print("https://github.com/dehoisted/Chat-Spy")

-- Config
Config = {
	enabled = true,
	spyOnMyself = true,
	public = false,
	publicItalics = true
}

-- Customizing Log Output
PrivateProperties = {
	Color = Color3.fromRGB(0,255,255); 
	Font = Enum.Font.SourceSansBold;
	TextSize = 18;
}

	local StarterGui = game:GetService("StarterGui")
	local Players = game:GetService("Players")
	local player = Players.LocalPlayer
	local saymsg = game:GetService("ReplicatedStorage"):WaitForChild("DefaultChatSystemChatEvents"):WaitForChild("SayMessageRequest")
	local getmsg = game:GetService("ReplicatedStorage"):WaitForChild("DefaultChatSystemChatEvents"):WaitForChild("OnMessageDoneFiltering")
	local instance = (_G.chatSpyInstance or 0) + 1
	_G.chatSpyInstance = instance

	local function onChatted(p,msg)
		if _G.chatSpyInstance == instance then
			if p==player and msg:lower():sub(1,4)=="/spy" then
				Config.enabled = not Config.enabled
				wait(0.3)
				PrivateProperties.Text = "{SPY "..(Config.enabled and "EN" or "DIS").."ABLED}"
				StarterGui:SetCore("ChatMakeSystemMessage", PrivateProperties)
			elseif Config.enabled and (Config.spyOnMyself==true or p~=player) then
				msg = msg:gsub("[\n\r]",''):gsub("\t",' '):gsub("[ ]+",' ')
				local hidden = true
				local conn = getmsg.OnClientEvent:Connect(function(packet,channel)
					if packet.SpeakerUserId==p.UserId and packet.Message==msg:sub(#msg-#packet.Message+1) and (channel=="All" or (channel=="Team" and Config.public==false and Players[packet.FromSpeaker].Team==player.Team)) then
						hidden = false
					end
				end)
				wait(1)
				conn:Disconnect()
				if hidden and Config.enabled then
					if Config.public then
						saymsg:FireServer((Config.publicItalics and "/me " or '').."{SPY} [".. p.Name .."]: "..msg,"All")
					else
						PrivateProperties.Text = "{SPY} [".. p.Name .."]: "..msg
						StarterGui:SetCore("ChatMakeSystemMessage", PrivateProperties)
					end
				end
			end
		end
	end
	
	for _,p in ipairs(Players:GetPlayers()) do
		p.Chatted:Connect(function(msg) onChatted(p,msg) end)
	end

	Players.PlayerAdded:Connect(function(p)
		p.Chatted:Connect(function(msg) onChatted(p,msg) end)
	end)

	PrivateProperties.Text = "{SPY "..(Config.enabled and "EN" or "DIS").."ABLED}"
	StarterGui:SetCore("ChatMakeSystemMessage", PrivateProperties)
	local chatFrame = player.PlayerGui.Chat.Frame
	chatFrame.ChatChannelParentFrame.Visible = true
	chatFrame.ChatBarParentFrame.Position = chatFrame.ChatChannelParentFrame.Position+UDim2.new(UDim.new(),chatFrame.ChatChannelParentFrame.Size.Y)
end)

-- Other

local Other = Window:NewTab("Other")
local OtherSection = Other:NewSection("Credits to corrupted#2222 for making this script")

OtherSection:NewLabel("dm me if u have any issues")
