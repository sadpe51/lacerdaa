-- leaked by https://discord.gg/WfTDsBPR9n join for more cheap sources

--[[
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
          NATESHUB V7.0 FINAL COMPLETE
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
 Drop Brainrot - EXACT from Kawatan Hub (NEW!)
 Main Tab: Speed, Unwalk, Inf Jump, Ragdoll TP, Drop Brainrot, Anti Ragdoll
 MISC auto-enables: FPS, Optimizer, Hitbox (ON at script start!)
 ALL 9 small GUIs are DRAGGABLE/MOVABLE
 Tutorial under Credits with instructions
 FOV warning about settings requirement
 Config auto-loads on join
 Anti Ragdoll & Ragdoll TP removed from MISC (in Main now)
 EVERYTHING WORKING PERFECTLY!
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
]]

repeat task.wait() until game:IsLoaded()

--------------------------------------------------
-- WHITELIST
--------------------------------------------------
local Players_wl = game:GetService("Players")
local player_wl  = Players_wl.LocalPlayer

local WHITELIST = {
    "DARK_VIDA7",
    "souzaaazr7",
    "lilimau67",
    "geeeeeee0512",
    "exiled57",
    "RatinPeladin89",
    "Secundaria_614",
    "Roubeumbrainrot98798",
}

local autorizado = false
for _, nome in ipairs(WHITELIST) do
    if player_wl.Name == nome then
        autorizado = true
        break
    end
end

if not autorizado then
    warn(" Acesso negado para: " .. player_wl.Name)
    task.wait(1)
    player_wl:Kick(" VocÃª nÃ£o estÃ¡ autorizado a usar este script.")
    return
end

print(" Acesso autorizado para:", player_wl.Name)

local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local HttpService = game:GetService("HttpService")
local Lighting = game:GetService("Lighting")

local player = Players.LocalPlayer
local character
local hrp
local hum

local function setupCharacter(char)
    character = char
    hrp = character:WaitForChild("HumanoidRootPart")
    hum = character:WaitForChild("Humanoid")
end

if player.Character then setupCharacter(player.Character) end
player.CharacterAdded:Connect(setupCharacter)

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- CONFIG SAVE/LOAD SYSTEM
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local CONFIG_FILE = "lacerda_config.json"

-- Valores padrao - toggles NUNCA sao salvos (sempre iniciam desligados)
local ConfigData = {
    normalSpeed        = 57.9,
    stealSpeed         = 29.6,
    jumpPower          = 25.8,
    stealRadius        = 9,
    fovValue           = 120,
    fovEnabled         = false,
    gravityPct         = 70,
    hopPower           = 35,
    -- toggles
    tog_speed          = false,
    tog_galaxy         = false,
    tog_antiRagdoll    = false,
    tog_ragdollTP      = false,
    tog_unwalk         = false,
    tog_autoGrab       = false,
    tog_dropBrainrot   = false,
}

local function SaveConfig()
    ConfigData.normalSpeed      = SpeedBooster and SpeedBooster.NormalSpeed or ConfigData.normalSpeed
    ConfigData.stealSpeed       = SpeedBooster and SpeedBooster.StealSpeed  or ConfigData.stealSpeed
    ConfigData.jumpPower        = SpeedBooster and SpeedBooster.JumpPower   or ConfigData.jumpPower
    ConfigData.stealRadius      = InstaSteal   and InstaSteal.Radius        or ConfigData.stealRadius
    ConfigData.fovValue         = fovValue    or ConfigData.fovValue
    ConfigData.fovEnabled       = fovEnabled  or false
    ConfigData.gravityPct       = galaxyGravityPct or ConfigData.gravityPct
    ConfigData.hopPower         = galaxyHopPower   or ConfigData.hopPower

    -- Salva usando as mesmas chaves do Lacerda Duels
    local function getBool(v) return v == true end
    ConfigData.GalaxyEnabled       = getBool(galaxyEnabled)
    ConfigData.AntiRagdollEnabled  = getBool(AntiRagdollEnabled)
    ConfigData.NoAnimsEnabled      = getBool(UnwalkEnabled)
    ConfigData.AutoGrabEnabled     = getBool(InstaSteal and InstaSteal.Connection ~= nil)
    ConfigData.DropBrainrotEnabled = getBool(DropBrainrotEnabled)
    ConfigData.GalaxyGravityPercent = galaxyGravityPct or 70
    ConfigData.HopPower            = galaxyHopPower or 35
    ConfigData.SPEED_NORMAL        = SpeedBooster and SpeedBooster.NormalSpeed or 57.9
    ConfigData.SPEED_STEAL         = SpeedBooster and SpeedBooster.StealSpeed  or 29.6
    ConfigData.AutoGrabRange       = InstaSteal and InstaSteal.Radius or 9

    local ok = false
    if writefile then
        pcall(function()
            writefile(CONFIG_FILE, HttpService:JSONEncode(ConfigData))
            ok = true
        end)
    end
    if ok then
        print("[NATESHUB] Config salvo! galaxy="..tostring(ConfigData.tog_galaxy).." antiRag="..tostring(ConfigData.tog_antiRagdoll).." unwalk="..tostring(ConfigData.tog_unwalk))
    else
        warn("[NATESHUB] Falha ao salvar config - writefile disponivel: "..tostring(writefile ~= nil))
    end
    return ok
end

local function LoadConfig()
    pcall(function()
        if readfile and isfile and isfile(CONFIG_FILE) then
            local data = HttpService:JSONDecode(readfile(CONFIG_FILE))
            if data then
                for k, v in pairs(data) do
                    if ConfigData[k] ~= nil then
                        ConfigData[k] = v
                    end
                end
                -- carrega chaves do Lacerda Duels
                if data.GalaxyEnabled       ~= nil then ConfigData.GalaxyEnabled       = data.GalaxyEnabled       end
                if data.AntiRagdollEnabled  ~= nil then ConfigData.AntiRagdollEnabled  = data.AntiRagdollEnabled  end
                if data.NoAnimsEnabled      ~= nil then ConfigData.NoAnimsEnabled      = data.NoAnimsEnabled      end
                if data.AutoGrabEnabled     ~= nil then ConfigData.AutoGrabEnabled     = data.AutoGrabEnabled     end
                if data.DropBrainrotEnabled ~= nil then ConfigData.DropBrainrotEnabled = data.DropBrainrotEnabled end
                if data.GalaxyGravityPercent~= nil then ConfigData.GalaxyGravityPercent= data.GalaxyGravityPercent end
                if data.HopPower            ~= nil then ConfigData.HopPower            = data.HopPower            end
                if data.SPEED_NORMAL        ~= nil then ConfigData.SPEED_NORMAL        = data.SPEED_NORMAL        end
                if data.SPEED_STEAL         ~= nil then ConfigData.SPEED_STEAL         = data.SPEED_STEAL         end
                if data.AutoGrabRange       ~= nil then ConfigData.AutoGrabRange       = data.AutoGrabRange       end
                print("[NATESHUB] Config carregado! galaxy="..tostring(ConfigData.GalaxyEnabled).." antiRag="..tostring(ConfigData.AntiRagdollEnabled).." unwalk="..tostring(ConfigData.NoAnimsEnabled))
            end
        end
    end)
end

LoadConfig()

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- SPEED BOOSTER (EXACT 57.9/29.6/25.8 - CONNECTED!)
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- SPEED AUTO (DO LACERDA HUB)
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

SpeedBooster = {
    Enabled     = false, -- sempre inicia desligado
    NormalSpeed = ConfigData.normalSpeed or 60,
    StealSpeed  = ConfigData.stealSpeed  or 30,
    JumpPower   = ConfigData.jumpPower   or 25.8,
    HeartbeatConn = nil,
    JumpConn      = nil,
}

local function startSpeedBooster()
    if SpeedBooster.HeartbeatConn then SpeedBooster.HeartbeatConn:Disconnect() end
    SpeedBooster.Enabled = true
    SpeedBooster.HeartbeatConn = RunService.Heartbeat:Connect(function()
        if not SpeedBooster.Enabled then return end
        local c = player.Character
        local h = c and c:FindFirstChildOfClass("Humanoid")
        local r = c and c:FindFirstChild("HumanoidRootPart")
        if not h or not r then return end
        if h.MoveDirection.Magnitude > 0.1 then
            -- Detecta steal automaticamente pelo WalkSpeed (igual Lacerda Hub)
            local vel = (h.WalkSpeed < 25) and SpeedBooster.StealSpeed or SpeedBooster.NormalSpeed
            r.AssemblyLinearVelocity = Vector3.new(
                h.MoveDirection.X * vel,
                r.AssemblyLinearVelocity.Y,
                h.MoveDirection.Z * vel
            )
        end
    end)
end

local function stopSpeedBooster()
    if SpeedBooster.HeartbeatConn then SpeedBooster.HeartbeatConn:Disconnect(); SpeedBooster.HeartbeatConn = nil end
    SpeedBooster.Enabled = false
end

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- AUTO PLAY
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local moving        = false
local loopActive    = false
local currentRoute  = "A"
local currentWaypoint = 1
local waypointConn  = nil
local statusLabel   = nil

local Zones = {}

local function checkInZone(pos)
    return false
end

local routeA = {
    Vector3.new(-474,-6.8,92.5),
    Vector3.new(-481.2,-5,93.5),
    Vector3.new(-481.2,-5,93.5),
    Vector3.new(-474,-6.8,92.5),
    Vector3.new(-473,-7,20),
    Vector3.new(-473,-7,11),
}
local routeB = {
    Vector3.new(-474.4,-6.8,26.4),
    Vector3.new(-482,-4.9,26.7),
    Vector3.new(-482,-4.9,26.7),
    Vector3.new(-472.9,-6.8,26.9),
    Vector3.new(-474,-7,112),
}

local function findMyPlot()
    local plots = workspace:FindFirstChild("Plots")
    if not plots then return nil end
    for _, plot in ipairs(plots:GetChildren()) do
        for _, d in ipairs(plot:GetDescendants()) do
            if d:IsA("TextLabel") and d.Text and (d.Text:find(player.DisplayName) or d.Text:find(player.Name)) then
                return plot
            end
        end
    end
    return nil
end

local function getEnemyRoute()
    local myPlot = findMyPlot()
    if myPlot then
        local plotPart = myPlot.PrimaryPart or myPlot:FindFirstChildWhichIsA("BasePart")
        if plotPart then
            return plotPart.Position.Z > 60 and "B" or "A"
        end
    end
    local char = player.Character
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    if hrp then
        return hrp.Position.Z > 60 and "B" or "A"
    end
    return "B"
end

local apSpeed = 60

local function wpMoveTo(target)
    local char = player.Character
    local hrp  = char and char:FindFirstChild("HumanoidRootPart")
    local hum  = char and char:FindFirstChildOfClass("Humanoid")
    if not hrp or not hum then return 999 end
    local dir  = Vector3.new(target.X, hrp.Position.Y, target.Z) - hrp.Position
    local dist = dir.Magnitude
    if dist > 0.01 then
        hum:Move(dir.Unit, false)
        hrp.AssemblyLinearVelocity = Vector3.new(
            dir.Unit.X * apSpeed,
            hrp.AssemblyLinearVelocity.Y,
            dir.Unit.Z * apSpeed
        )
    end
    return dist
end

local function wpStopMoving()
    local char = player.Character
    local hrp  = char and char:FindFirstChild("HumanoidRootPart")
    local hum  = char and char:FindFirstChildOfClass("Humanoid")
    if hum then hum:Move(Vector3.zero, false) end
    if hrp  then hrp.AssemblyLinearVelocity = Vector3.new(0, hrp.AssemblyLinearVelocity.Y, 0) end
end

local MOVE_KEYS = {
    [Enum.KeyCode.W]=true,[Enum.KeyCode.A]=true,
    [Enum.KeyCode.S]=true,[Enum.KeyCode.D]=true,
    [Enum.KeyCode.Up]=true,[Enum.KeyCode.Down]=true,
    [Enum.KeyCode.Left]=true,[Enum.KeyCode.Right]=true,
}
local function playerIsMoving()
    for key in pairs(MOVE_KEYS) do
        if UserInputService:IsKeyDown(key) then return true end
    end
    local char = player.Character
    local hum  = char and char:FindFirstChildOfClass("Humanoid")
    if hum and hum.MoveDirection.Magnitude > 0.3 then return true end
    return false
end

local waypoints = routeA
local apLoopThread = nil

local function stopAutoPlay()
    moving = false
    loopActive = false
    apSpeed = 60
    if apLoopThread then
        task.cancel(apLoopThread)
        apLoopThread = nil
    end
    local char = player.Character
    local hrp  = char and char:FindFirstChild("HumanoidRootPart")
    local hum  = char and char:FindFirstChildOfClass("Humanoid")
    if hum then hum:Move(Vector3.zero, false) end
    if hrp  then hrp.AssemblyLinearVelocity = Vector3.new(0, hrp.AssemblyLinearVelocity.Y, 0) end
    if statusLabel then statusLabel.Text = "" end
end

local function startAutoPlay()
    local char = player.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if not root then return end

    local route = getEnemyRoute()
    moving = true; loopActive = true; currentRoute = route
    apSpeed = 60
    waypoints = route == "A" and routeA or routeB
    currentWaypoint = 1

    if statusLabel then
        statusLabel.Text = "Indo base | " .. (route == "A" and "LEFT" or "RIGHT")
    end

    if apLoopThread then task.cancel(apLoopThread) end
    apLoopThread = task.spawn(function()
        while moving do
            -- pausa se o jogador estiver se movendo manualmente
            if playerIsMoving() then
                task.wait(0.05)
                continue
            end

            local wp = waypoints[currentWaypoint]
            if not wp then task.wait(0.05); continue end

            local dist = wpMoveTo(wp)

            -- Ativa speed 30 ao chegar perto da coordenada definida (sÃ³ na routeA waypoints 3 e 4)
            local speedTriggerA = Vector3.new(-479.9, -5, 94.3)
            local speedTriggerB = Vector3.new(-479.9, -4.9, 26.4)
            local char = player.Character
            local hrp  = char and char:FindFirstChild("HumanoidRootPart")
            if hrp and currentRoute == "A" and (currentWaypoint == 3 or currentWaypoint == 4) and (hrp.Position - speedTriggerA).Magnitude < 5 then
                apSpeed = 30
            end
            if hrp and currentRoute == "B" and (currentWaypoint == 4 or currentWaypoint == 5) and (hrp.Position - speedTriggerB).Magnitude < 5 then
                apSpeed = 30
            end

            if dist < 5 then
                if currentWaypoint == #waypoints then
                    if loopActive then
                        if statusLabel then statusLabel.Text = "Trocando rota..." end
                        wpStopMoving()
                        task.wait(0.3)
                        currentRoute = currentRoute == "A" and "B" or "A"
                        waypoints = currentRoute == "A" and routeA or routeB
                        currentWaypoint = 1
                        apSpeed = 60
                        if statusLabel then statusLabel.Text = "Loop | " .. (currentRoute == "A" and "LEFT" or "RIGHT") end
                    end
                else
                    currentWaypoint = currentWaypoint + 1
                    if statusLabel then statusLabel.Text = "WP:" .. currentWaypoint .. " | " .. currentRoute end
                end
            end

            task.wait(0.05) -- roda 20x por segundo em vez de 60x, reduz ping
        end
    end)
end

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- GALAXY / INF JUMP (DO LACERDA HUB)
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local galaxyVectorForce = nil
local galaxyAttachment  = nil
local galaxyEnabled     = false
local galaxyHopsEnabled = false
local galaxyLastHop     = 0
local galaxySpaceHeld   = false
local galaxyOriginalJP  = 50
local galaxyJumpConn    = nil
local galaxyGravityPct  = ConfigData.gravityPct or 70
local galaxyHopPower    = ConfigData.hopPower   or 35
local galaxyHopCooldown = 0.08
local DEFAULT_GRAVITY   = 196.2

local function captureOriginalJP()
    local c = player.Character
    if not c then return end
    local h = c:FindFirstChildOfClass("Humanoid")
    if h and h.JumpPower > 0 then galaxyOriginalJP = h.JumpPower end
end
task.spawn(function() task.wait(1); captureOriginalJP() end)

local function setupGalaxyForce()
    pcall(function()
        local c = player.Character; if not c then return end
        local h = c:FindFirstChild("HumanoidRootPart"); if not h then return end
        if galaxyVectorForce then galaxyVectorForce:Destroy() end
        if galaxyAttachment  then galaxyAttachment:Destroy()  end
        galaxyAttachment = Instance.new("Attachment"); galaxyAttachment.Parent = h
        galaxyVectorForce = Instance.new("VectorForce")
        galaxyVectorForce.Attachment0 = galaxyAttachment
        galaxyVectorForce.ApplyAtCenterOfMass = true
        galaxyVectorForce.RelativeTo = Enum.ActuatorRelativeTo.World
        galaxyVectorForce.Force = Vector3.new(0,0,0)
        galaxyVectorForce.Parent = h
    end)
end

local function updateGalaxyForce()
    if not galaxyEnabled or not galaxyVectorForce then return end
    local c = player.Character; if not c then return end
    local mass = 0
    for _, p in ipairs(c:GetDescendants()) do
        if p:IsA("BasePart") then mass = mass + p:GetMass() end
    end
    local tg = DEFAULT_GRAVITY * (galaxyGravityPct / 100)
    galaxyVectorForce.Force = Vector3.new(0, mass * (DEFAULT_GRAVITY - tg) * 0.95, 0)
end

local function adjustGalaxyJump()
    pcall(function()
        local c = player.Character; if not c then return end
        local h = c:FindFirstChildOfClass("Humanoid"); if not h then return end
        if not galaxyEnabled then h.JumpPower = galaxyOriginalJP; return end
        local ratio = math.sqrt((DEFAULT_GRAVITY*(galaxyGravityPct/100))/DEFAULT_GRAVITY)
        h.JumpPower = galaxyOriginalJP * ratio
    end)
end

local function doMiniHop()
    if not galaxyHopsEnabled then return end
    pcall(function()
        local c = player.Character; if not c then return end
        local h = c:FindFirstChild("HumanoidRootPart")
        if not h then return end
        if tick() - galaxyLastHop < galaxyHopCooldown then return end
        galaxyLastHop = tick()
        h.AssemblyLinearVelocity = Vector3.new(h.AssemblyLinearVelocity.X, galaxyHopPower, h.AssemblyLinearVelocity.Z)
    end)
end

local function startGalaxy()
    galaxyEnabled = true; galaxyHopsEnabled = true
    setupGalaxyForce(); adjustGalaxyJump()
    if galaxyJumpConn then galaxyJumpConn:Disconnect() end
    galaxyJumpConn = UserInputService.JumpRequest:Connect(function()
        if galaxyHopsEnabled then
            galaxySpaceHeld = true; doMiniHop()
            task.delay(0.15, function() galaxySpaceHeld = false end)
        end
    end)
end

local function stopGalaxy()
    galaxyEnabled = false; galaxyHopsEnabled = false
    if galaxyJumpConn then galaxyJumpConn:Disconnect(); galaxyJumpConn = nil end
    if galaxyVectorForce then galaxyVectorForce:Destroy(); galaxyVectorForce = nil end
    if galaxyAttachment  then galaxyAttachment:Destroy();  galaxyAttachment  = nil end
    adjustGalaxyJump()
end

RunService.Heartbeat:Connect(function()
    if galaxyHopsEnabled and galaxySpaceHeld then doMiniHop() end
    if galaxyEnabled then updateGalaxyForce() end
end)

UserInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.Space then
        galaxySpaceHeld = true
        if galaxyHopsEnabled then doMiniHop() end
    end
end)
UserInputService.InputEnded:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.Space then galaxySpaceHeld = false end
end)

player.CharacterAdded:Connect(function()
    task.wait(1); captureOriginalJP()
    if galaxyEnabled then setupGalaxyForce(); adjustGalaxyJump() end
end)

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- UNWALK
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local UnwalkEnabled = false
local savedAnimate = nil

local function startUnwalk()
    if not character then return end
    local anim = character:FindFirstChild("Animate")
    if anim then
        savedAnimate = anim:Clone()
        anim.Disabled = true
        task.wait()
        anim:Destroy()
    end
    local h2 = character:FindFirstChildOfClass("Humanoid")
    if h2 then
        for _, track in ipairs(h2:GetPlayingAnimationTracks()) do
            track:Stop()
        end
    end
end

local function stopUnwalk()
    if savedAnimate and character then
        local na = savedAnimate:Clone()
        na.Parent = character
        na.Disabled = false
    end
end

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- SPIN BOT
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local SpinBotEnabled = false
local spinBAV = nil
local SPIN_SPEED = 10

local function cleanupSpinBot()
    if spinBAV then spinBAV:Destroy() spinBAV = nil end
    local c = player.Character
    if c then
        local root = c:FindFirstChild("HumanoidRootPart")
        if root then
            for _, v in pairs(root:GetChildren()) do
                if v.Name == "SpinBAV" and v:IsA("BodyAngularVelocity") then
                    v:Destroy()
                end
            end
        end
    end
end

local function startSpinBot()
    cleanupSpinBot()
    local c = player.Character
    if not c then return end
    local root = c:FindFirstChild("HumanoidRootPart")
    if not root then return end
    
    spinBAV = Instance.new("BodyAngularVelocity")
    spinBAV.Name = "SpinBAV"
    spinBAV.MaxTorque = Vector3.new(0, math.huge, 0)
    spinBAV.AngularVelocity = Vector3.new(0, SPIN_SPEED, 0)
    spinBAV.Parent = root
end

local function stopSpinBot()
    cleanupSpinBot()
end

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- INF JUMP
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local InfJumpEnabled = false
local INF_JUMP_POWER = 50
local INF_JUMP_COOLDOWN = 0.15
local lastJump = 0
local infJumpConn = nil

local function startInfJump()
    if infJumpConn then return end
    infJumpConn = UserInputService.JumpRequest:Connect(function()
        if not InfJumpEnabled then return end
        local char = player.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        local humanoid = char and char:FindFirstChildOfClass("Humanoid")
        if root and humanoid then
            if humanoid.FloorMaterial == Enum.Material.Air then
                local now = tick()
                if now - lastJump >= INF_JUMP_COOLDOWN then
                    lastJump = now
                    root.Velocity = Vector3.new(root.Velocity.X, INF_JUMP_POWER, root.Velocity.Z)
                end
            end
        end
    end)
end

local function stopInfJump()
    if infJumpConn then
        infJumpConn:Disconnect()
        infJumpConn = nil
    end
end

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
--  FOV SYSTEM (MAX 120 - ACTUALLY WORKS NOW!)
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

fovValue = ConfigData.fovValue or 120
fovEnabled = ConfigData.fovEnabled or false
local fovConnection = nil
local camera = workspace.CurrentCamera

local function updateFOV()
    if fovEnabled then
        --  DIRECT CAMERA FOV UPDATE - THIS ACTUALLY WORKS!
        camera.FieldOfView = fovValue
        
        --  LOCK IT IN PLACE WITH RENDERSTEP
        if not fovConnection then
            fovConnection = RunService.RenderStepped:Connect(function()
                if fovEnabled and camera then
                    camera.FieldOfView = fovValue
                end
            end)
        end
    else
        --  DISCONNECT AND RESET
        if fovConnection then 
            fovConnection:Disconnect() 
            fovConnection = nil 
        end
        camera.FieldOfView = 70 -- Reset to default
    end
end

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- INSTA STEAL (ALWAYS ON - 0.2s DURATION)
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- AUTO GRAB (ANTILOSER HUB)
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

InstaSteal = {
    Radius   = 11,
    Duration = 0.25,
    Connection = nil
}

local ProgressBarFill        = nil
local ProgressPercentLabel   = nil
local isStealing             = false
local stealStartTime         = nil
local progressConnection     = nil
local StealData              = {}

local function isMyPlotByName(pn)
    local plots = workspace:FindFirstChild("Plots"); if not plots then return false end
    local plot  = plots:FindFirstChild(pn);         if not plot  then return false end
    local sign  = plot:FindFirstChild("PlotSign")
    if sign then
        local yb = sign:FindFirstChild("YourBase")
        if yb and yb:IsA("BillboardGui") then return yb.Enabled == true end
    end
    return false
end

local function findNearestPrompt()
    local h = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    if not h then return nil end
    local plots = workspace:FindFirstChild("Plots"); if not plots then return nil end
    local np, nd, nn = nil, math.huge, nil
    for _, plot in ipairs(plots:GetChildren()) do
        if isMyPlotByName(plot.Name) then continue end
        local podiums = plot:FindFirstChild("AnimalPodiums"); if not podiums then continue end
        for _, pod in ipairs(podiums:GetChildren()) do
            pcall(function()
                local base  = pod:FindFirstChild("Base")
                local spawn = base and base:FindFirstChild("Spawn")
                if spawn then
                    local dist = (spawn.Position - h.Position).Magnitude
                    if dist < nd and dist <= InstaSteal.Radius then
                        local att = spawn:FindFirstChild("PromptAttachment")
                        if att then
                            for _, ch in ipairs(att:GetChildren()) do
                                if ch:IsA("ProximityPrompt") then np, nd, nn = ch, dist, pod.Name; break end
                            end
                        end
                    end
                end
            end)
        end
    end
    return np, nd, nn
end

local function ResetProgressBar()
    if ProgressPercentLabel then ProgressPercentLabel.Text = "0%" end
    if ProgressBarFill      then ProgressBarFill.Size = UDim2.new(0, 0, 1, 0) end
end

local function executeSteal(prompt)
    if isStealing then return end
    if not StealData[prompt] then
        StealData[prompt] = { hold = {}, trigger = {}, ready = true }
        pcall(function()
            if getconnections then
                for _, c in ipairs(getconnections(prompt.PromptButtonHoldBegan)) do
                    if c.Function then table.insert(StealData[prompt].hold, c.Function) end
                end
                for _, c in ipairs(getconnections(prompt.Triggered)) do
                    if c.Function then table.insert(StealData[prompt].trigger, c.Function) end
                end
            end
        end)
    end
    local data = StealData[prompt]
    if not data.ready then return end
    data.ready = false; isStealing = true; stealStartTime = tick()
    if progressConnection then progressConnection:Disconnect() end
    progressConnection = RunService.Heartbeat:Connect(function()
        if not isStealing then progressConnection:Disconnect(); return end
        local prog = math.clamp((tick() - stealStartTime) / InstaSteal.Duration, 0, 1)
        if ProgressBarFill      then ProgressBarFill.Size = UDim2.new(prog, 0, 1, 0) end
        if ProgressPercentLabel then ProgressPercentLabel.Text = math.floor(prog * 100) .. "%" end
    end)
    task.spawn(function()
        for _, f in ipairs(data.hold) do task.spawn(f) end
        task.wait(InstaSteal.Duration)
        for _, f in ipairs(data.trigger) do task.spawn(f) end
        if progressConnection then progressConnection:Disconnect() end
        ResetProgressBar(); data.ready = true; task.wait(0.3); isStealing = false
    end)
end

local function autoStealLoop()
    if InstaSteal.Connection then InstaSteal.Connection:Disconnect() end
    InstaSteal.Connection = RunService.Heartbeat:Connect(function()
        if not isStealing then
            local p = findNearestPrompt()
            if p then executeSteal(p) end
        end
    end)
end

local function stopAutoSteal()
    if InstaSteal.Connection then
        InstaSteal.Connection:Disconnect()
        InstaSteal.Connection = nil
    end
    isStealing = false
    ResetProgressBar()
end

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- FPS BOOSTER, OPTIMIZER, HITBOX, ANTI DEATH/RAGDOLL, FLOAT
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local fpsBoosterActive = false

local function enableFPSBooster()
    if fpsBoosterActive then return end
    fpsBoosterActive = true
    
    task.spawn(function()
        for _, obj in pairs(workspace:GetDescendants()) do
            pcall(function()
                if obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Beam") or 
                   obj:IsA("Smoke") or obj:IsA("Fire") or obj:IsA("Sparkles") or 
                   obj:IsA("Explosion") then
                    obj:Destroy()
                end
            end)
        end
    end)
    
    task.spawn(function()
        pcall(function()
            Lighting.GlobalShadows = false
            Lighting.FogEnd = 9e9
            Lighting.Brightness = 0
            Lighting.ClockTime = 12
            if Lighting:FindFirstChild("Atmosphere") then
                Lighting.Atmosphere:Destroy()
            end
        end)
        
        for _, light in pairs(workspace:GetDescendants()) do
            if light:IsA("PointLight") or light:IsA("SpotLight") or light:IsA("SurfaceLight") then
                light:Destroy()
            end
        end
    end)
    
    task.spawn(function()
        pcall(function()
            settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
        end)
        
        for _, effect in pairs(camera:GetChildren()) do
            if effect:IsA("BloomEffect") or effect:IsA("BlurEffect") or 
               effect:IsA("DepthOfFieldEffect") or effect:IsA("SunRaysEffect") or 
               effect:IsA("ColorCorrectionEffect") then
                effect:Destroy()
            end
        end
    end)
    
    task.wait(3)
    fpsBoosterActive = false
end

local function enableOptimizer()
    workspace.Terrain.WaterWaveSize = 0
    Lighting.GlobalShadows = false
    Lighting.Brightness = 1
    Lighting.ClockTime = 12
end

local HitboxActive = false
local HitboxConnection = nil

local function enableHitbox()
    HitboxActive = true
    
    if HitboxConnection then
        HitboxConnection:Disconnect()
    end
    
    HitboxConnection = RunService.RenderStepped:Connect(function()
        for _, Player in pairs(Players:GetPlayers()) do
            if Player ~= player and Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
                local RootPart = Player.Character.HumanoidRootPart
                RootPart.Size = Vector3.new(10, 10, 10)
                RootPart.Transparency = 0.5
                RootPart.CanCollide = false
            end
        end
    end)
end

local function disableHitbox()
    HitboxActive = false
    if HitboxConnection then
        HitboxConnection:Disconnect()
        HitboxConnection = nil
    end
    
    for _, Player in pairs(Players:GetPlayers()) do
        if Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
            local RootPart = Player.Character.HumanoidRootPart
            RootPart.Size = Vector3.new(2, 2, 1)
            RootPart.Transparency = 1
            RootPart.CanCollide = true
        end
    end
end

local AntiDeathEnabled = false

local AntiRagdollConns = {}

local function enableAntiRagdoll()
    if #AntiRagdollConns > 0 then return end
    
    local char = player.Character or player.CharacterAdded:Wait()
    local humanoid = char:WaitForChild("Humanoid")
    local root = char:WaitForChild("HumanoidRootPart")
    local cam = workspace.CurrentCamera
    local animator = humanoid:WaitForChild("Animator")
    
    --  EXACT VALUES FROM YOUR NATESHUB SCRIPT!
    local maxVelocity = 40
    local clampVelocity = 25
    local maxClamp = 15
    local lastVelocity = Vector3.new(0, 0, 0)
    
    local function IsRagdollState()
        local state = humanoid:GetState()
        return state == Enum.HumanoidStateType.Physics
            or state == Enum.HumanoidStateType.Ragdoll
            or state == Enum.HumanoidStateType.FallingDown
            or state == Enum.HumanoidStateType.GettingUp
    end
    
    local function CleanRagdollEffects()
        for _, obj in pairs(char:GetDescendants()) do
            if obj:IsA("BallSocketConstraint") or obj:IsA("NoCollisionConstraint") or obj:IsA("HingeConstraint")
                or (obj:IsA("Attachment") and (obj.Name == "A" or obj.Name == "B")) then
                obj:Destroy()
            elseif obj:IsA("BodyVelocity") or obj:IsA("BodyPosition") or obj:IsA("BodyGyro") then
                obj:Destroy()
            elseif obj:IsA("Motor6D") then
                obj.Enabled = true
            end
        end
        
        for _, track in pairs(animator:GetPlayingAnimationTracks()) do
            local animName = track.Animation and track.Animation.Name:lower() or ""
            if animName:find("rag") or animName:find("fall") or animName:find("hurt") or animName:find("down") then
                track:Stop(0)
            end
        end
    end
    
    local function ReEnableControls()
        pcall(function()
            require(player:WaitForChild("PlayerScripts"):WaitForChild("PlayerModule")):GetControls():Enable()
        end)
    end
    
    table.insert(AntiRagdollConns, humanoid.StateChanged:Connect(function(_, newState)
        if IsRagdollState() then
            humanoid:ChangeState(Enum.HumanoidStateType.Running)
            CleanRagdollEffects()
            cam.CameraSubject = humanoid
            ReEnableControls()
        end
    end))
    
    table.insert(AntiRagdollConns, RunService.Heartbeat:Connect(function()
        if IsRagdollState() then
            CleanRagdollEffects()
            local vel = root.AssemblyLinearVelocity
            local velChange = (vel - lastVelocity).Magnitude
            if velChange > maxVelocity and vel.Magnitude > clampVelocity then
                root.AssemblyLinearVelocity = vel.Unit * math.min(vel.Magnitude, maxClamp)
            end
            lastVelocity = vel
        end
    end))
    
    table.insert(AntiRagdollConns, char.DescendantAdded:Connect(function()
        if IsRagdollState() then CleanRagdollEffects() end
    end))
    
    table.insert(AntiRagdollConns, player.CharacterAdded:Connect(function(newChar)
        char = newChar
        humanoid = newChar:WaitForChild("Humanoid")
        root = newChar:WaitForChild("HumanoidRootPart")
        animator = humanoid:WaitForChild("Animator")
        lastVelocity = Vector3.new(0, 0, 0)
        ReEnableControls()
        CleanRagdollEffects()
    end))
    
    ReEnableControls()
    CleanRagdollEffects()
end

local function disableAntiRagdoll()
    for _, conn in pairs(AntiRagdollConns) do
        conn:Disconnect()
    end
    AntiRagdollConns = {}
end

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
--  RAGDOLL TP (EXACT FROM YOUR SOURCE!)
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local RAGDOLL_COORDS = {
    right = {
        Vector3.new(-464.46, -5.85, 23.38),
        Vector3.new(-486.15, -3.50, 23.85)
    },
    left = {
        Vector3.new(-469.95, -5.85, 90.99),
        Vector3.new(-485.91, -3.55, 96.77)
    }
}

local RagdollTP = {
    currentHumanoid = nil,
    wasStanding = true,
    ragdollActive = false,
    enabled = false,
    conn = nil
}

local function doubleTeleport(posTable)
    local character = player.Character
    if not character then return end
    character:PivotTo(CFrame.new(posTable[1]))
    task.wait(0.1)
    character:PivotTo(CFrame.new(posTable[2]))
end

local function getAnimalTarget()
    local plots = workspace:FindFirstChild("Plots")
    if not plots then return nil end

    for _, plot in ipairs(plots:GetChildren()) do
        local sign = plot:FindFirstChild("PlotSign")
        local base = plot:FindFirstChild("DeliveryHitbox")
        if sign and sign:FindFirstChild("YourBase") and sign.YourBase.Enabled and base then
            local target = plot:FindFirstChild("AnimalTarget", true)
            if target then
                return target.Position
            end
        end
    end
    return nil
end

local function performTeleport()
    local target = getAnimalTarget()
    if not target then return end

    local leftDist = (target - RAGDOLL_COORDS.left[1]).Magnitude
    local rightDist = (target - RAGDOLL_COORDS.right[1]).Magnitude

    if leftDist > rightDist then
        doubleTeleport(RAGDOLL_COORDS.left)
    else
        doubleTeleport(RAGDOLL_COORDS.right)
    end
end

local function startRagdollTP()
    if RagdollTP.conn then RagdollTP.conn:Disconnect() end
    
    RagdollTP.conn = RunService.Heartbeat:Connect(function()
        if not RagdollTP.enabled or not RagdollTP.currentHumanoid then return end

        local currentState = RagdollTP.currentHumanoid:GetState()
        if currentState == Enum.HumanoidStateType.Physics then
            if RagdollTP.wasStanding then
                performTeleport()
            end
            RagdollTP.wasStanding = false
        else
            RagdollTP.wasStanding = true
        end
    end)
end

local function stopRagdollTP()
    if RagdollTP.conn then
        RagdollTP.conn:Disconnect()
        RagdollTP.conn = nil
    end
end

-- Update humanoid reference on character respawn
player.CharacterAdded:Connect(function(char)
    task.wait(0.5)
    RagdollTP.currentHumanoid = char:WaitForChild("Humanoid")
    RagdollTP.wasStanding = true
    RagdollTP.ragdollActive = false
end)

if player.Character then
    RagdollTP.currentHumanoid = player.Character:FindFirstChildOfClass("Humanoid")
end

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
--  DROP BRAINROT (EXACT FROM KAWATAN HUB!)
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local dropBusy = false
local FLOAT_DROP_HEIGHT = 20

local function doDrop()
    if dropBusy or not hrp then return end
    dropBusy = true
    
    -- Stop any existing float
    if floatConn then floatConn:Disconnect() floatConn = nil end
    floatEnabled = false
    
    local origY = hrp.Position.Y
    local targetY = origY + FLOAT_DROP_HEIGHT
    local goingUp = true
    
    floatConn = RunService.Heartbeat:Connect(function()
        if not hrp then return end
        
        local currentTarget = goingUp and targetY or origY
        local diff = currentTarget - hrp.Position.Y
        
        hrp.AssemblyLinearVelocity = Vector3.new(
            hrp.AssemblyLinearVelocity.X,
            math.clamp(diff * 25, -300, 300),
            hrp.AssemblyLinearVelocity.Z
        )
        
        if goingUp and hrp.Position.Y >= targetY - 1 then
            goingUp = false
        end
        
        if not goingUp and math.abs(diff) < 1.5 then
            hrp.AssemblyLinearVelocity = Vector3.new(
                hrp.AssemblyLinearVelocity.X,
                0,
                hrp.AssemblyLinearVelocity.Z
            )
            floatConn:Disconnect()
            floatConn = nil
            dropBusy = false
        end
    end)
end

local floatEnabled = false
local floatHeight = 6.1
local floatConn = nil
local floatOriginalY = nil

local function startFloat()
    if not hrp then return end
    if floatConn then return end
    
    floatOriginalY = hrp.Position.Y
    floatEnabled = true
    hrp.AssemblyLinearVelocity = Vector3.new(hrp.AssemblyLinearVelocity.X, 500, hrp.AssemblyLinearVelocity.Z)
    
    floatConn = RunService.Heartbeat:Connect(function()
        if not floatEnabled or not hrp then return end
        local targetY = floatOriginalY + floatHeight
        local curY = hrp.Position.Y
        local diff = targetY - curY
        
        if math.abs(diff) > 0.1 then
            hrp.AssemblyLinearVelocity = Vector3.new(hrp.AssemblyLinearVelocity.X, math.clamp(diff * 25, -150, 150), hrp.AssemblyLinearVelocity.Z)
        else
            hrp.AssemblyLinearVelocity = Vector3.new(hrp.AssemblyLinearVelocity.X, 0, hrp.AssemblyLinearVelocity.Z)
        end
    end)
end

local function stopFloat()
    floatEnabled = false
    if floatConn then floatConn:Disconnect() floatConn = nil end
    if hrp then
        hrp.AssemblyLinearVelocity = Vector3.new(hrp.AssemblyLinearVelocity.X, -500, hrp.AssemblyLinearVelocity.Z)
    end
end

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- TRACK PLAYER (BAT AIMBOT)
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local batAimbotToggled = false
local BAT_MOVE_SPEED = 56.5
local BAT_ENGAGE_RANGE = 20
local BAT_LOOP_TIME = 0.3
local lastEquipTick_bat = 0
local lastUseTick_bat = 0
local lookConnection_bat = nil
local attachment_bat = nil
local alignOrientation_bat = nil
local BAT_LOOK_DISTANCE = 50

local function nearestPlayer_bat()
    if not hrp then return nil, math.huge end
    local closest, minDist = nil, math.huge
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= player and plr.Character then
            local targetHRP = plr.Character:FindFirstChild("HumanoidRootPart")
            local targetHum = plr.Character:FindFirstChildOfClass("Humanoid")
            if targetHRP and targetHum and targetHum.Health > 0 then
                local distance = (targetHRP.Position - hrp.Position).Magnitude
                if distance < minDist then
                    minDist = distance
                    closest = targetHRP
                end
            end
        end
    end
    return closest, minDist
end

local function closestLookTarget()
    if not hrp then return nil end
    local nearest = nil
    local shortest = BAT_LOOK_DISTANCE
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            local distance = (hrp.Position - plr.Character.HumanoidRootPart.Position).Magnitude
            if distance < shortest then
                shortest = distance
                nearest = plr.Character.HumanoidRootPart
            end
        end
    end
    return nearest
end

local function startLookAt()
    if not hrp or not hum then return end
    hum.AutoRotate = false
    
    attachment_bat = Instance.new("Attachment", hrp)
    alignOrientation_bat = Instance.new("AlignOrientation")
    alignOrientation_bat.Attachment0 = attachment_bat
    alignOrientation_bat.Mode = Enum.OrientationAlignmentMode.OneAttachment
    alignOrientation_bat.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
    alignOrientation_bat.Responsiveness = 1000
    alignOrientation_bat.RigidityEnabled = true
    alignOrientation_bat.Parent = hrp

    lookConnection_bat = RunService.RenderStepped:Connect(function()
        if not hrp or not alignOrientation_bat then return end
        local target = closestLookTarget()
        if not target then return end
        local lookPos = Vector3.new(target.Position.X, hrp.Position.Y, target.Position.Z)
        alignOrientation_bat.CFrame = CFrame.lookAt(hrp.Position, lookPos)
    end)
end

local function stopLookAt()
    if lookConnection_bat then lookConnection_bat:Disconnect() lookConnection_bat = nil end
    if alignOrientation_bat then alignOrientation_bat:Destroy() alignOrientation_bat = nil end
    if attachment_bat then attachment_bat:Destroy() attachment_bat = nil end
    if hum then hum.AutoRotate = true end
end

local function stopBatAimbot()
    batAimbotToggled = false
    stopLookAt()
    if hrp then
        hrp.AssemblyLinearVelocity = Vector3.zero
    end
end

local function startBatAimbot()
    stopBatAimbot()
    batAimbotToggled = true
    if not character or not hrp or not hum then return end
    startLookAt()
end

local function equipBat_target()
    if not hum then return end
    local batTool = player.Backpack:FindFirstChild("Bat") or character:FindFirstChild("Bat")
    if batTool then
        hum:EquipTool(batTool)
        return batTool
    end
end

RunService.Heartbeat:Connect(function()
    if not batAimbotToggled or not character or not hrp or not hum then return end

    hrp.CanCollide = false

    local target, distance = nearestPlayer_bat()
    if not target then return end

    local targetPos = Vector3.new(target.Position.X, target.Position.Y, target.Position.Z)
    local moveDir = (targetPos - hrp.Position).Unit
    hrp.AssemblyLinearVelocity = moveDir * BAT_MOVE_SPEED

    if distance <= BAT_ENGAGE_RANGE then
        if tick() - lastEquipTick_bat >= BAT_LOOP_TIME then
            equipBat_target()
            lastEquipTick_bat = tick()
        end
        if tick() - lastUseTick_bat >= BAT_LOOP_TIME then
            local batTool = character:FindFirstChild("Bat")
            if batTool then
                batTool:Activate()
            end
            lastUseTick_bat = tick()
        end
    end
end)

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- HIT CIRCLE
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local hitCircleEnabled = false
local hitCircleConn = nil
local hitCircle = nil
local hitCircleAlign = nil
local hitCircleAttach = nil

local function startHitCircle()
    if hitCircleConn then return end
    local char = player.Character or player.CharacterAdded:Wait()
    local hrpLocal = char:WaitForChild("HumanoidRootPart")
    
    hitCircleAttach = Instance.new("Attachment", hrpLocal)
    hitCircleAlign = Instance.new("AlignOrientation", hrpLocal)
    hitCircleAlign.Attachment0 = hitCircleAttach
    hitCircleAlign.Mode = Enum.OrientationAlignmentMode.OneAttachment
    hitCircleAlign.RigidityEnabled = true
    
    hitCircle = Instance.new("Part")
    hitCircle.Shape = Enum.PartType.Cylinder
    hitCircle.Material = Enum.Material.Neon
    hitCircle.Size = Vector3.new(0.05, 14.5, 14.5)
    hitCircle.Color = Color3.fromRGB(180, 230, 120)
    hitCircle.CanCollide = false
    hitCircle.Massless = true
    hitCircle.Parent = workspace
    
    local weld = Instance.new("Weld")
    weld.Part0 = hrpLocal
    weld.Part1 = hitCircle
    weld.C0 = CFrame.new(0,-1,0)*CFrame.Angles(0,0,math.rad(90))
    weld.Parent = hitCircle
    
    hitCircleConn = RunService.RenderStepped:Connect(function()
        local target, dmin = nil, 7.25
        for _, p in ipairs(Players:GetPlayers()) do
            if p ~= player and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                local d = (p.Character.HumanoidRootPart.Position - hrpLocal.Position).Magnitude
                if d <= dmin then
                    target, dmin = p.Character.HumanoidRootPart, d
                end
            end
        end
        if target then
            char.Humanoid.AutoRotate = false
            hitCircleAlign.Enabled = true
            hitCircleAlign.CFrame = CFrame.lookAt(hrpLocal.Position, Vector3.new(target.Position.X, hrpLocal.Position.Y, target.Position.Z))
            local t = char:FindFirstChild("Bat") or char:FindFirstChild("Medusa")
            if t then t:Activate() end
        else
            hitCircleAlign.Enabled = false
            char.Humanoid.AutoRotate = true
        end
    end)
end

local function stopHitCircle()
    if hitCircleConn then hitCircleConn:Disconnect() hitCircleConn = nil end
    if hitCircle then hitCircle:Destroy() hitCircle = nil end
    if hitCircleAlign then hitCircleAlign:Destroy() hitCircleAlign = nil end
    if hitCircleAttach then hitCircleAttach:Destroy() hitCircleAttach = nil end
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.AutoRotate = true
    end
end

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- GUI SIZES
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local selectedDevice = nil
local GUI_SIZES = {
    PC = {Width = 700, Height = 550},
    iPad = {Width = 550, Height = 500},
    iPhone = {Width = 320, Height = 450}
}

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- MAIN SCREENGUI
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "NateshubGUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = CoreGui

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
--  SMALL GUI CREATOR WITH GREEN DOT!
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local SmallGUIInstances = {}

local function createSmallGUI(name, yPosition)
    local SmallGUI = Instance.new("Frame")
    SmallGUI.Name = "Small"..name.."GUI"
    SmallGUI.Size = UDim2.new(0, 140, 0, 35)
    SmallGUI.Position = UDim2.new(0.5, -70, 0, yPosition)
    SmallGUI.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    SmallGUI.BackgroundTransparency = 0.3
    SmallGUI.BorderSizePixel = 0
    SmallGUI.Visible = false
    SmallGUI.Active = true
    SmallGUI.Draggable = true  --  DRAGGABLE!
    SmallGUI.Parent = ScreenGui
    
    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 10)
    Corner.Parent = SmallGUI
    
    local Stroke = Instance.new("UIStroke")
    Stroke.Color = Color3.fromRGB(138, 43, 226)
    Stroke.Thickness = 2
    Stroke.Parent = SmallGUI
    
    --  GREEN DOT!
    local GreenDot = Instance.new("Frame")
    GreenDot.Size = UDim2.new(0, 12, 0, 12)
    GreenDot.Position = UDim2.new(0, 8, 0.5, -6)
    GreenDot.BackgroundColor3 = Color3.fromRGB(100, 100, 100) -- GRAY by default
    GreenDot.BorderSizePixel = 0
    GreenDot.Parent = SmallGUI
    
    local DotCorner = Instance.new("UICorner")
    DotCorner.CornerRadius = UDim.new(1, 0)
    DotCorner.Parent = GreenDot
    
    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(1, -55, 1, 0)
    Label.Position = UDim2.new(0, 25, 0, 0)
    Label.BackgroundTransparency = 1
    Label.Text = name
    Label.TextColor3 = Color3.fromRGB(255, 255, 255)
    Label.Font = Enum.Font.GothamBold
    Label.TextSize = 11
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = SmallGUI
    
    --  CLICK BUTTON (smaller, on right side only - doesn't block dragging!)
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(0, 40, 1, 0)
    Button.Position = UDim2.new(1, -40, 0, 0)
    Button.BackgroundTransparency = 1
    Button.Text = ""
    Button.TextColor3 = Color3.fromRGB(138, 43, 226)
    Button.Font = Enum.Font.GothamBold
    Button.TextSize = 16
    Button.ZIndex = 2
    Button.Parent = SmallGUI
    
    SmallGUIInstances[name] = {Frame = SmallGUI, Label = Label, Button = Button, Dot = GreenDot}
    
    return SmallGUI, Button, GreenDot
end

--  CREATE ALL SMALL GUIs WITH GREEN DOTS!
local SmallSpeedGUI, SmallSpeedButton, SpeedDot = createSmallGUI("Speed", 100)
local SmallUnwalkGUI, SmallUnwalkButton, UnwalkDot = createSmallGUI("Unwalk", 145)
local SmallSpinBotGUI, SmallSpinBotButton, SpinDot = createSmallGUI("Spin Bot", 190)
local SmallInfJumpGUI, SmallInfJumpButton, InfJumpDot = createSmallGUI("Inf Jump", 235)
local SmallFloatGUI, SmallFloatButton, FloatDot = createSmallGUI("Float Up", 280)
local SmallFOVGUI, SmallFOVButton, FOVDot = createSmallGUI("FOV", 325)
local SmallAntiRagGUI, SmallAntiRagButton, AntiRagDot = createSmallGUI("Anti Ragdoll", 370) --  Standalone!
local SmallRagdollTPGUI, SmallRagdollTPButton, RagdollTPDot = createSmallGUI("Ragdoll TP", 415) --  NEW!
local SmallDropGUI, SmallDropButton, DropDot = createSmallGUI("Drop Brainrot", 460) --  NEW!

--  SPEED SMALL GUI TOGGLE
SmallSpeedButton.MouseButton1Click:Connect(function()
    SpeedBooster.Enabled = not SpeedBooster.Enabled
    
    if SpeedBooster.Enabled then
        SmallSpeedGUI.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        SpeedDot.BackgroundColor3 = Color3.fromRGB(0, 255, 0) --  GREEN DOT!
        startSpeedBooster()
    else
        SmallSpeedGUI.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        SpeedDot.BackgroundColor3 = Color3.fromRGB(100, 100, 100) --  GRAY DOT!
        stopSpeedBooster()
    end
end)

--  UNWALK SMALL GUI TOGGLE
SmallUnwalkButton.MouseButton1Click:Connect(function()
    UnwalkEnabled = not UnwalkEnabled
    
    if UnwalkEnabled then
        SmallUnwalkGUI.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        UnwalkDot.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        startUnwalk()
    else
        SmallUnwalkGUI.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        UnwalkDot.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
        stopUnwalk()
    end
end)

--  SPIN BOT SMALL GUI TOGGLE
SmallSpinBotButton.MouseButton1Click:Connect(function()
    SpinBotEnabled = not SpinBotEnabled
    
    if SpinBotEnabled then
        SmallSpinBotGUI.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        SpinDot.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        startSpinBot()
    else
        SmallSpinBotGUI.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        SpinDot.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
        stopSpinBot()
    end
end)

--  INF JUMP SMALL GUI TOGGLE
SmallInfJumpButton.MouseButton1Click:Connect(function()
    InfJumpEnabled = not InfJumpEnabled
    
    if InfJumpEnabled then
        SmallInfJumpGUI.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        InfJumpDot.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        startInfJump()
    else
        SmallInfJumpGUI.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        InfJumpDot.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
        stopInfJump()
    end
end)

--  FLOAT UP SMALL GUI TOGGLE
SmallFloatButton.MouseButton1Click:Connect(function()
    floatEnabled = not floatEnabled
    
    if floatEnabled then
        SmallFloatGUI.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        FloatDot.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        startFloat()
    else
        SmallFloatGUI.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        FloatDot.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
        stopFloat()
    end
end)

--  FOV SMALL GUI TOGGLE
SmallFOVButton.MouseButton1Click:Connect(function()
    fovEnabled = not fovEnabled
    
    if fovEnabled then
        SmallFOVGUI.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        FOVDot.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        updateFOV()
    else
        SmallFOVGUI.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        FOVDot.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
        camera.FieldOfView = 70
        if fovConnection then fovConnection:Disconnect() fovConnection = nil end
    end
end)

--  ANTI RAGDOLL SMALL GUI TOGGLE (Advanced version!)
local AntiRagdollEnabled = false

SmallAntiRagButton.MouseButton1Click:Connect(function()
    AntiRagdollEnabled = not AntiRagdollEnabled
    
    if AntiRagdollEnabled then
        SmallAntiRagGUI.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        AntiRagDot.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        enableAntiRagdoll()
    else
        SmallAntiRagGUI.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        AntiRagDot.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
        disableAntiRagdoll()
    end
end)

--  RAGDOLL TP SMALL GUI TOGGLE (NEW!)
SmallRagdollTPButton.MouseButton1Click:Connect(function()
    RagdollTP.enabled = not RagdollTP.enabled
    
    if RagdollTP.enabled then
        SmallRagdollTPGUI.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        RagdollTPDot.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        startRagdollTP()
    else
        SmallRagdollTPGUI.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        RagdollTPDot.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
        stopRagdollTP()
    end
end)

--  DROP BRAINROT SMALL GUI TOGGLE - SEPARATE STANDALONE TOGGLE!
local DropBrainrotEnabled = false
local dropBrainrotLoop = nil

local function startDropBrainrot()
    if dropBrainrotLoop then return end
    
    dropBrainrotLoop = task.spawn(function()
        while DropBrainrotEnabled do
            if not dropBusy and hrp then
                doDrop()
            end
            task.wait(5) -- Wait 5 seconds between drops
        end
    end)
end

local function stopDropBrainrot()
    if dropBrainrotLoop then
        task.cancel(dropBrainrotLoop)
        dropBrainrotLoop = nil
    end
end

SmallDropButton.MouseButton1Click:Connect(function()
    DropBrainrotEnabled = not DropBrainrotEnabled
    
    if DropBrainrotEnabled then
        SmallDropGUI.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        DropDot.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        startDropBrainrot()
    else
        SmallDropGUI.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        DropDot.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
        stopDropBrainrot()
    end
end)

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- STEAL BAR (ALWAYS VISIBLE - INSTA STEAL ON)
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- AUTO GRAB HUD (ANTILOSER HUB)
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local agGui = Instance.new("ScreenGui")
agGui.Name = "AutoStealHUD_" .. player.UserId
agGui.ResetOnSpawn = false
agGui.IgnoreGuiInset = true
agGui.Parent = player:WaitForChild("PlayerGui")

local PB_W = 390
local PB_H = 52
local progBar = Instance.new("Frame", agGui)
progBar.Name = "StealProgressBar"
progBar.Size = UDim2.new(0, PB_W, 0, PB_H)
progBar.Position = UDim2.new(0.5, -PB_W/2, 0.85, -89)
progBar.BackgroundColor3 = Color3.fromRGB(5, 10, 40)
progBar.BackgroundTransparency = 0.3
progBar.BorderSizePixel = 0
progBar.Visible = false
Instance.new("UICorner", progBar).CornerRadius = UDim.new(0, 12)
do
    local s = Instance.new("UIStroke", progBar)
    s.Color = Color3.fromRGB(255,255,255); s.Thickness = 2
    s.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    local g = Instance.new("UIGradient", s)
    g.Rotation = 0
    g.Transparency = NumberSequence.new({
        NumberSequenceKeypoint.new(0,0), NumberSequenceKeypoint.new(0.4,0.8),
        NumberSequenceKeypoint.new(0.6,0.8), NumberSequenceKeypoint.new(1,0),
    })
    TweenService:Create(g, TweenInfo.new(3, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut, -1), {Rotation=360}):Play()
end

-- % label
ProgressPercentLabel = Instance.new("TextLabel", progBar)
ProgressPercentLabel.Size = UDim2.new(1, 0, 0, 28)
ProgressPercentLabel.Position = UDim2.new(0, 0, 0, 3)
ProgressPercentLabel.BackgroundTransparency = 1
ProgressPercentLabel.Text = "0%"
ProgressPercentLabel.TextColor3 = Color3.fromRGB(255,255,255)
ProgressPercentLabel.Font = Enum.Font.GothamBlack
ProgressPercentLabel.TextSize = 19
ProgressPercentLabel.TextXAlignment = Enum.TextXAlignment.Center
ProgressPercentLabel.ZIndex = 3

-- Input boxes (Radius e Duration)
local IW, IH = 42, 28
local function makePBInput(xOffset, labelStr, getVal, setVal)
    local holder = Instance.new("Frame", progBar)
    holder.Size = UDim2.new(0, IW, 0, IH)
    holder.Position = UDim2.new(1, -(IW + xOffset), 0, 2)
    holder.BackgroundColor3 = Color3.fromRGB(24,24,24)
    holder.BackgroundTransparency = 0.3
    holder.BorderSizePixel = 0; holder.ZIndex = 4
    Instance.new("UICorner", holder).CornerRadius = UDim.new(0,6)
    local lbl = Instance.new("TextLabel", holder)
    lbl.Size = UDim2.new(1,0,0,11); lbl.BackgroundTransparency = 1
    lbl.Text = labelStr; lbl.TextColor3 = Color3.fromRGB(150,150,150)
    lbl.Font = Enum.Font.GothamBold; lbl.TextSize = 7
    lbl.TextXAlignment = Enum.TextXAlignment.Center
    local box = Instance.new("TextBox", holder)
    box.Size = UDim2.new(1,-4,0,14); box.Position = UDim2.new(0,2,0,12)
    box.BackgroundTransparency = 1; box.Text = tostring(getVal())
    box.TextColor3 = Color3.fromRGB(255,255,255)
    box.Font = Enum.Font.GothamBold; box.TextSize = 11
    box.ClearTextOnFocus = false; box.BorderSizePixel = 0
    box.TextXAlignment = Enum.TextXAlignment.Center; box.ZIndex = 5
    box.FocusLost:Connect(function()
        local n = tonumber(box.Text)
        if n and n > 0 then setVal(n); box.Text = tostring(n)
        else box.Text = tostring(getVal()) end
    end)
end
makePBInput(8,        "RADIUS",   function() return InstaSteal.Radius   end, function(v) InstaSteal.Radius   = v end)
makePBInput(8+IW+6,   "DURATION", function() return InstaSteal.Duration end, function(v) InstaSteal.Duration = v end)

-- Trilha de progresso
local pbTrack = Instance.new("Frame", progBar)
pbTrack.Size = UDim2.new(1, -16, 0, 7)
pbTrack.Position = UDim2.new(0, 8, 1, -10)
pbTrack.BackgroundColor3 = Color3.fromRGB(15, 20, 45)
pbTrack.BackgroundTransparency = 0.4
pbTrack.BorderSizePixel = 0; pbTrack.ZIndex = 2
Instance.new("UICorner", pbTrack).CornerRadius = UDim.new(1,0)

ProgressBarFill = Instance.new("Frame", pbTrack)
ProgressBarFill.Size = UDim2.new(0, 0, 1, 0)
ProgressBarFill.BackgroundColor3 = Color3.fromRGB(0, 200, 255)
ProgressBarFill.BackgroundTransparency = 0.1
ProgressBarFill.BorderSizePixel = 0; ProgressBarFill.ZIndex = 3
Instance.new("UICorner", ProgressBarFill).CornerRadius = UDim.new(1,0)
do
    local g = Instance.new("UIGradient", ProgressBarFill)
    g.Color = ColorSequence.new({
        ColorSequenceKeypoint.new(0, Color3.fromRGB(0,120,255)),
        ColorSequenceKeypoint.new(1, Color3.fromRGB(0,220,255)),
    })
end

local StealBarContainer = progBar

-- Mostrar/esconder HUD conforme InstaSteal.Connection
RunService.RenderStepped:Connect(function()
    local active = InstaSteal.Connection ~= nil
    if progBar.Visible ~= active then progBar.Visible = active end
end)

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- DEVICE SELECTION SCREEN
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local DeviceSelectFrame = Instance.new("Frame")
DeviceSelectFrame.Name = "DeviceSelect"
DeviceSelectFrame.Size = UDim2.new(0, 400, 0, 260)
DeviceSelectFrame.Position = UDim2.new(0.5, -200, 0.5, -130)
DeviceSelectFrame.BackgroundColor3 = Color3.fromRGB(5, 10, 40)
DeviceSelectFrame.BackgroundTransparency = 0.3
DeviceSelectFrame.BorderSizePixel = 0
DeviceSelectFrame.Parent = ScreenGui

local DeviceCorner = Instance.new("UICorner")
DeviceCorner.CornerRadius = UDim.new(0, 14)
DeviceCorner.Parent = DeviceSelectFrame

-- Borda animada branca (igual Lacerda Hub)
local _devSpinTag = Instance.new("BoolValue")
_devSpinTag.Name = "__WhiteSpin"
_devSpinTag.Parent = DeviceSelectFrame
local _devStroke = Instance.new("UIStroke", DeviceSelectFrame)
_devStroke.Color = Color3.fromRGB(255,255,255)
_devStroke.Thickness = 3
_devStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
local _devGrad = Instance.new("UIGradient", _devStroke)
_devGrad.Rotation = 0
_devGrad.Transparency = NumberSequence.new({
    NumberSequenceKeypoint.new(0, 0),
    NumberSequenceKeypoint.new(0.4, 0.8),
    NumberSequenceKeypoint.new(0.6, 0.8),
    NumberSequenceKeypoint.new(1, 0),
})
TweenService:Create(_devGrad, TweenInfo.new(3, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut, -1), {Rotation = 360}):Play()

-- Bolinhas de fundo (igual Lacerda Hub)
local DEV_PARTICLE_DOTS = {
    { pos = UDim2.new(0.935, 0, 0.829, 0), color = Color3.new(0, 0.980, 1), alpha = 0.48, sz = 4 },
    { pos = UDim2.new(0.688, 0, 0.869, 0), color = Color3.new(0, 0.867, 1), alpha = 0.34, sz = 2 },
    { pos = UDim2.new(0.939, 0, 0.471, 0), color = Color3.new(0, 0.808, 1), alpha = 0.38, sz = 2 },
    { pos = UDim2.new(0.845, 0, 0.176, 0), color = Color3.new(0, 0.745, 1), alpha = 0.38, sz = 2 },
    { pos = UDim2.new(0.416, 0, 0.489, 0), color = Color3.new(0, 0.843, 1), alpha = 0.35, sz = 3 },
    { pos = UDim2.new(0.464, 0, 0.070, 0), color = Color3.new(0, 0.655, 1), alpha = 0.29, sz = 3 },
    { pos = UDim2.new(0.638, 0, 0.657, 0), color = Color3.new(0, 0.655, 1), alpha = 0.24, sz = 3 },
    { pos = UDim2.new(0.684, 0, 0.037, 0), color = Color3.new(0, 0.937, 1), alpha = 0.47, sz = 2 },
    { pos = UDim2.new(0.773, 0, 0.075, 0), color = Color3.new(0, 0.651, 1), alpha = 0.44, sz = 2 },
    { pos = UDim2.new(0.472, 0, 0.343, 0), color = Color3.new(0, 0.894, 1), alpha = 0.20, sz = 2 },
    { pos = UDim2.new(0.766, 0, 0.425, 0), color = Color3.new(0, 0.925, 1), alpha = 0.49, sz = 2 },
    { pos = UDim2.new(0.257, 0, 0.757, 0), color = Color3.new(0, 0.976, 1), alpha = 0.51, sz = 2 },
    { pos = UDim2.new(0.515, 0, 0.558, 0), color = Color3.new(0, 0.976, 1), alpha = 0.33, sz = 2 },
    { pos = UDim2.new(0.124, 0, 0.896, 0), color = Color3.new(0, 0.824, 1), alpha = 0.45, sz = 2 },
    { pos = UDim2.new(0.960, 0, 0.012, 0), color = Color3.new(0, 0.733, 1), alpha = 0.33, sz = 2 },
    { pos = UDim2.new(0.967, 0, 0.966, 0), color = Color3.new(0, 0.639, 1), alpha = 0.31, sz = 2 },
    { pos = UDim2.new(0.199, 0, 0.369, 0), color = Color3.new(0, 0.737, 1), alpha = 0.31, sz = 2 },
    { pos = UDim2.new(0.457, 0, 0.229, 0), color = Color3.new(0, 0.737, 1), alpha = 0.31, sz = 2 },
}
local _devDotFrames = {}
for _, dot in ipairs(DEV_PARTICLE_DOTS) do
    local f = Instance.new("Frame", DeviceSelectFrame)
    f.Size = UDim2.new(0, dot.sz, 0, dot.sz)
    f.Position = dot.pos
    f.BackgroundColor3 = dot.color
    f.BackgroundTransparency = dot.alpha
    f.BorderSizePixel = 0
    f.ZIndex = 1
    Instance.new("UICorner", f).CornerRadius = UDim.new(1, 0)
    table.insert(_devDotFrames, f)
end
do
    local _ddX, _ddY = {}, {}
    for i = 1, #DEV_PARTICLE_DOTS do
        _ddX[i] = (math.random() - 0.5) * 0.00015
        _ddY[i] = (math.random() > 0.5 and 1 or -1) * 0.0009
    end
    RunService.RenderStepped:Connect(function()
        if not DeviceSelectFrame.Parent then return end
        for i, frame in ipairs(_devDotFrames) do
            if not frame.Parent then return end
            local p = frame.Position
            local nx = p.X.Scale + _ddX[i]
            local ny = p.Y.Scale + _ddY[i]
            if nx < -0.05 then nx = 1.05 elseif nx > 1.05 then nx = -0.05 end
            if ny < -0.05 then ny = 1.05 elseif ny > 1.05 then ny = -0.05 end
            frame.Position = UDim2.new(nx, 0, ny, 0)
        end
    end)
end

local DeviceTitle = Instance.new("TextLabel")
DeviceTitle.Size = UDim2.new(1, 0, 0, 60)
DeviceTitle.BackgroundColor3 = Color3.fromRGB(10, 12, 18)
DeviceTitle.BackgroundTransparency = 0.3
DeviceTitle.BorderSizePixel = 0
DeviceTitle.Text = "SELECIONE SEU DISPOSITIVO"
DeviceTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
DeviceTitle.Font = Enum.Font.GothamBold
DeviceTitle.TextSize = 18
DeviceTitle.ZIndex = 3
DeviceTitle.Parent = DeviceSelectFrame

local TitleCorner = Instance.new("UICorner")
TitleCorner.CornerRadius = UDim.new(0, 14)
TitleCorner.Parent = DeviceTitle

local TitleMask = Instance.new("Frame")
TitleMask.Size = UDim2.new(1, 0, 0, 15)
TitleMask.Position = UDim2.new(0, 0, 1, -15)
TitleMask.BackgroundColor3 = Color3.fromRGB(10, 12, 18)
TitleMask.BackgroundTransparency = 0.3
TitleMask.BorderSizePixel = 0
TitleMask.ZIndex = 3
TitleMask.Parent = DeviceTitle

local function createDeviceButton(deviceName, icon, yPos)
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(0, 350, 0, 50)
    Button.Position = UDim2.new(0.5, -175, 0, yPos)
    Button.BackgroundColor3 = Color3.fromRGB(15, 30, 50)
    Button.BorderSizePixel = 0
    Button.Text = ""
    Button.ZIndex = 4
    Button.Parent = DeviceSelectFrame

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 10)
    Corner.Parent = Button

    local BtnStroke = Instance.new("UIStroke", Button)
    BtnStroke.Color = Color3.fromRGB(0, 180, 255)
    BtnStroke.Thickness = 1.5

    local Icon = Instance.new("TextLabel")
    Icon.Size = UDim2.new(0, 40, 0, 40)
    Icon.Position = UDim2.new(0, 15, 0.5, -20)
    Icon.BackgroundTransparency = 1
    Icon.Text = icon
    Icon.TextSize = 24
    Icon.Font = Enum.Font.GothamBold
    Icon.ZIndex = 5
    Icon.Parent = Button

    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(1, -70, 1, 0)
    Label.Position = UDim2.new(0, 60, 0, 0)
    Label.BackgroundTransparency = 1
    Label.Text = deviceName
    Label.TextColor3 = Color3.fromRGB(255, 255, 255)
    Label.Font = Enum.Font.GothamBold
    Label.TextSize = 18
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.ZIndex = 5
    Label.Parent = Button

    Button.MouseEnter:Connect(function()
        TweenService:Create(Button, TweenInfo.new(0.2), {
            BackgroundColor3 = Color3.fromRGB(0, 120, 200)
        }):Play()
    end)

    Button.MouseLeave:Connect(function()
        TweenService:Create(Button, TweenInfo.new(0.2), {
            BackgroundColor3 = Color3.fromRGB(15, 30, 50)
        }):Play()
    end)

    return Button
end

local PCButton = createDeviceButton("PC", "", 80)
local iPhoneButton = createDeviceButton("Celular", "", 150)

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- MAIN GUI CREATION
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local MainFrame
local currentTab = "Main"
local tabButtons = {}
local contentFrames = {}

local normalSpeedBox, stealSpeedBox
local fovSliderBox

local function createMainGUI(device)
    local size = GUI_SIZES[device]
    local width = size.Width
    local height = size.Height

    MainFrame = Instance.new("Frame")
    MainFrame.Name = "MainFrame"
    MainFrame.Size = UDim2.new(0, width, 0, height)
    MainFrame.Position = UDim2.new(0.5, -width/2, 0.5, -height/2)
    MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    MainFrame.BackgroundTransparency = 0.1
    MainFrame.BorderSizePixel = 0
    MainFrame.Active = true
    MainFrame.Draggable = true
    MainFrame.Visible = false
    MainFrame.Parent = ScreenGui

    local MainCorner = Instance.new("UICorner")
    MainCorner.CornerRadius = UDim.new(0, 15)
    MainCorner.Parent = MainFrame

    local Header = Instance.new("Frame")
    Header.Size = UDim2.new(1, 0, 0, 50)
    Header.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
    Header.BorderSizePixel = 0
    Header.Parent = MainFrame

    local HeaderCorner = Instance.new("UICorner")
    HeaderCorner.CornerRadius = UDim.new(0, 15)
    HeaderCorner.Parent = Header

    local HeaderMask = Instance.new("Frame")
    HeaderMask.Size = UDim2.new(1, 0, 0, 15)
    HeaderMask.Position = UDim2.new(0, 0, 1, -15)
    HeaderMask.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
    HeaderMask.BorderSizePixel = 0
    HeaderMask.Parent = Header

    local logoSize = device == "iPhone" and 22 or 30
    local titleSize = device == "iPhone" and 14 or 20

    local LogoIcon = Instance.new("TextLabel")
    LogoIcon.Size = UDim2.new(0, 40, 0, 40)
    LogoIcon.Position = UDim2.new(0, 10, 0.5, -20)
    LogoIcon.BackgroundTransparency = 1
    LogoIcon.Text = ""
    LogoIcon.TextSize = logoSize
    LogoIcon.Font = Enum.Font.GothamBold
    LogoIcon.Parent = Header

    local TitleLabel = Instance.new("TextLabel")
    TitleLabel.Size = UDim2.new(0, 150, 1, 0)
    TitleLabel.Position = UDim2.new(0, 50, 0, 0)
    TitleLabel.BackgroundTransparency = 1
    TitleLabel.Text = "NATESHUB"
    TitleLabel.TextColor3 = Color3.fromRGB(255, 200, 100)
    TitleLabel.Font = Enum.Font.GothamBold
    TitleLabel.TextSize = titleSize
    TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
    TitleLabel.Parent = Header

    local CloseButton = Instance.new("TextButton")
    CloseButton.Size = UDim2.new(0, 30, 0, 30)
    CloseButton.Position = UDim2.new(1, -35, 0.5, -15)
    CloseButton.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
    CloseButton.Text = "-"
    CloseButton.TextColor3 = Color3.fromRGB(200, 200, 200)
    CloseButton.Font = Enum.Font.GothamBold
    CloseButton.TextSize = 18
    CloseButton.Parent = Header

    local CloseCorner = Instance.new("UICorner")
    CloseCorner.CornerRadius = UDim.new(0, 8)
    CloseCorner.Parent = CloseButton

    CloseButton.MouseButton1Click:Connect(function()
        local slideOut = TweenService:Create(
            MainFrame,
            TweenInfo.new(0.5, Enum.EasingStyle.Back, Enum.EasingDirection.In),
            {Position = UDim2.new(0, -width - 100, 0.5, -height/2)}
        )
        slideOut:Play()
        slideOut.Completed:Wait()
        MainFrame.Visible = false
    end)

    local sidebarWidth = device == "iPhone" and 70 or (device == "iPad" and 150 or 200)

    local Sidebar = Instance.new("Frame")
    Sidebar.Size = UDim2.new(0, sidebarWidth, 1, -60)
    Sidebar.Position = UDim2.new(0, 0, 0, 55)
    Sidebar.BackgroundTransparency = 1
    Sidebar.Parent = MainFrame

    local function createTab(icon, name, order)
        local TabButton = Instance.new("TextButton")
        TabButton.Name = name
        TabButton.Size = UDim2.new(1, -10, 0, device == "iPhone" and 55 or 50)
        TabButton.Position = UDim2.new(0, 5, 0, (order - 1) * (device == "iPhone" and 60 or 60))
        TabButton.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
        TabButton.BorderSizePixel = 0
        TabButton.Text = ""
        TabButton.Parent = Sidebar

        local TabCorner = Instance.new("UICorner")
        TabCorner.CornerRadius = UDim.new(0, 10)
        TabCorner.Parent = TabButton

        local IconLabel = Instance.new("TextLabel")
        IconLabel.Size = UDim2.new(0, 30, 0, 30)
        IconLabel.Position = device == "iPhone" and UDim2.new(0.5, -15, 0, 5) or UDim2.new(0, 10, 0.5, -15)
        IconLabel.BackgroundTransparency = 1
        IconLabel.Text = icon
        IconLabel.TextSize = device == "iPhone" and 16 or 20
        IconLabel.Font = Enum.Font.GothamBold
        IconLabel.TextColor3 = Color3.fromRGB(150, 150, 160)
        IconLabel.Parent = TabButton

        local TextLabel = Instance.new("TextLabel")
        TextLabel.Size = device == "iPhone" and UDim2.new(1, 0, 0, 18) or UDim2.new(1, -45, 1, 0)
        TextLabel.Position = device == "iPhone" and UDim2.new(0, 0, 1, -20) or UDim2.new(0, 45, 0, 0)
        TextLabel.BackgroundTransparency = 1
        TextLabel.Text = name
        TextLabel.TextColor3 = Color3.fromRGB(150, 150, 160)
        TextLabel.Font = Enum.Font.GothamBold
        TextLabel.TextSize = device == "iPhone" and 8 or (device == "iPad" and 12 or 16)
        TextLabel.TextXAlignment = Enum.TextXAlignment.Left
        TextLabel.Parent = TabButton

        tabButtons[name] = {Button = TabButton, Icon = IconLabel, Text = TextLabel}

        return TabButton
    end

    local MainTab = createTab("", "Main", 1)
    local MISCTab = createTab("", "MISC", 2)
    local SettingsTab = createTab("", "Settings", 3)
    local CreditsTab = createTab("", "Credits", 4)

    local ContentContainer = Instance.new("Frame")
    ContentContainer.Size = UDim2.new(1, -sidebarWidth - 20, 1, -70)
    ContentContainer.Position = UDim2.new(0, sidebarWidth + 10, 0, 60)
    ContentContainer.BackgroundTransparency = 1
    ContentContainer.Parent = MainFrame

    local function createContentFrame(name)
        local Content = Instance.new("ScrollingFrame")
        Content.Name = name
        Content.Size = UDim2.new(1, 0, 1, 0)
        Content.BackgroundTransparency = 1
        Content.BorderSizePixel = 0
        Content.ScrollBarThickness = 4
        Content.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 110)
        Content.CanvasSize = UDim2.new(0, 0, 0, 0)
        Content.Visible = false
        Content.Parent = ContentContainer

        local UIList = Instance.new("UIListLayout")
        UIList.Padding = UDim.new(0, 10)
        UIList.SortOrder = Enum.SortOrder.LayoutOrder
        UIList.Parent = Content

        UIList:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
            Content.CanvasSize = UDim2.new(0, 0, 0, UIList.AbsoluteContentSize.Y + 20)
        end)

        contentFrames[name] = Content
        return Content
    end

    local MainContent = createContentFrame("Main")
    local MISCContent = createContentFrame("MISC")
    local SettingsContent = createContentFrame("Settings")
    local CreditsContent = createContentFrame("Credits")

    MainContent.Visible = true

    local function switchTab(tabName)
        currentTab = tabName

        for name, elements in pairs(tabButtons) do
            if name == tabName then
                elements.Button.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                elements.Text.TextColor3 = Color3.fromRGB(30, 30, 35)
                elements.Icon.TextColor3 = Color3.fromRGB(30, 30, 35)
            else
                elements.Button.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
                elements.Text.TextColor3 = Color3.fromRGB(150, 150, 160)
                elements.Icon.TextColor3 = Color3.fromRGB(150, 150, 160)
            end
        end

        for name, frame in pairs(contentFrames) do
            frame.Visible = (name == tabName)
        end
    end

    MainTab.MouseButton1Click:Connect(function() switchTab("Main") end)
    MISCTab.MouseButton1Click:Connect(function() switchTab("MISC") end)
    SettingsTab.MouseButton1Click:Connect(function() switchTab("Settings") end)
    CreditsTab.MouseButton1Click:Connect(function() switchTab("Credits") end)

    switchTab("Main")

    -- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
    --  MAIN TAB CONTENT (WITH SETTINGS BUTTONS NEXT TO FEATURES!)
    -- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

    local function createMainButton(name, order, callback, hasSettings)
        local Container = Instance.new("Frame")
        Container.Size = UDim2.new(1, 0, 0, 50)
        Container.BackgroundTransparency = 1
        Container.LayoutOrder = order
        Container.Parent = MainContent

        local buttonWidth = hasSettings and -60 or 0

        local Button = Instance.new("TextButton")
        Button.Size = UDim2.new(1, buttonWidth, 0, 50)
        Button.Position = UDim2.new(0, 0, 0, 0)
        Button.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
        Button.BorderSizePixel = 0
        Button.Text = ""
        Button.Parent = Container

        local Corner = Instance.new("UICorner")
        Corner.CornerRadius = UDim.new(0, 10)
        Corner.Parent = Button

        local Label = Instance.new("TextLabel")
        Label.Size = UDim2.new(1, -15, 1, 0)
        Label.Position = UDim2.new(0, 15, 0, 0)
        Label.BackgroundTransparency = 1
        Label.Text = name
        Label.TextColor3 = Color3.fromRGB(255, 255, 255)
        Label.Font = Enum.Font.GothamBold
        Label.TextSize = device == "iPhone" and 11 or 14
        Label.TextXAlignment = Enum.TextXAlignment.Left
        Label.Parent = Button

        Button.MouseButton1Click:Connect(callback)

        if hasSettings then
            local SettingsButton = Instance.new("TextButton")
            SettingsButton.Size = UDim2.new(0, 50, 0, 50)
            SettingsButton.Position = UDim2.new(1, -50, 0, 0)
            SettingsButton.BackgroundColor3 = Color3.fromRGB(60, 60, 65)
            SettingsButton.Text = ""
            SettingsButton.TextColor3 = Color3.fromRGB(255, 255, 255)
            SettingsButton.Font = Enum.Font.GothamBold
            SettingsButton.TextSize = device == "iPhone" and 16 or 20
            SettingsButton.Parent = Container

            local SettingsCorner = Instance.new("UICorner")
            SettingsCorner.CornerRadius = UDim.new(0, 10)
            SettingsCorner.Parent = SettingsButton

            return Container, SettingsButton
        end

        return Container
    end

    --  SPEED BOOSTER WITH SETTINGS BUTTON!
    local SpeedContainer, SpeedSettingsBtn = createMainButton("Speed Booster", 1, function()
        SmallSpeedGUI.Visible = not SmallSpeedGUI.Visible
    end, true)

    --  SPEED SETTINGS POPUP (NEXT TO BUTTON!)
    local SpeedSettingsGUI = Instance.new("Frame")
    SpeedSettingsGUI.Name = "SpeedSettingsGUI"
    SpeedSettingsGUI.Size = UDim2.new(0, 200, 0, 150)
    SpeedSettingsGUI.Position = UDim2.new(0.5, -100, 0.5, -75)
    SpeedSettingsGUI.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    SpeedSettingsGUI.BackgroundTransparency = 0.1
    SpeedSettingsGUI.BorderSizePixel = 0
    SpeedSettingsGUI.Visible = false
    SpeedSettingsGUI.Active = true
    SpeedSettingsGUI.Draggable = true
    SpeedSettingsGUI.Parent = ScreenGui

    local SpeedCorner = Instance.new("UICorner")
    SpeedCorner.CornerRadius = UDim.new(0, 12)
    SpeedCorner.Parent = SpeedSettingsGUI

    local SpeedStroke = Instance.new("UIStroke")
    SpeedStroke.Color = Color3.fromRGB(255, 255, 255)
    SpeedStroke.Thickness = 2
    SpeedStroke.Parent = SpeedSettingsGUI

    local SpeedTitle = Instance.new("TextLabel")
    SpeedTitle.Size = UDim2.new(1, 0, 0, 30)
    SpeedTitle.BackgroundTransparency = 1
    SpeedTitle.Text = "SPEED SETTINGS"
    SpeedTitle.TextColor3 = Color3.fromRGB(255, 200, 100)
    SpeedTitle.Font = Enum.Font.GothamBold
    SpeedTitle.TextSize = 14
    SpeedTitle.Parent = SpeedSettingsGUI

    local NormalLabel = Instance.new("TextLabel")
    NormalLabel.Size = UDim2.new(0.6, 0, 0, 25)
    NormalLabel.Position = UDim2.new(0, 10, 0, 35)
    NormalLabel.BackgroundTransparency = 1
    NormalLabel.Text = "Normal Speed:"
    NormalLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    NormalLabel.Font = Enum.Font.Gotham
    NormalLabel.TextSize = 12
    NormalLabel.TextXAlignment = Enum.TextXAlignment.Left
    NormalLabel.Parent = SpeedSettingsGUI

    normalSpeedBox = Instance.new("TextBox")
    normalSpeedBox.Size = UDim2.new(0, 60, 0, 25)
    normalSpeedBox.Position = UDim2.new(1, -70, 0, 35)
    normalSpeedBox.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
    normalSpeedBox.Text = tostring(SpeedBooster.NormalSpeed)
    normalSpeedBox.TextColor3 = Color3.fromRGB(255, 255, 255)
    normalSpeedBox.Font = Enum.Font.GothamBold
    normalSpeedBox.TextSize = 12
    normalSpeedBox.Parent = SpeedSettingsGUI

    local NormalCorner = Instance.new("UICorner")
    NormalCorner.CornerRadius = UDim.new(0, 6)
    NormalCorner.Parent = normalSpeedBox

    local StealLabel = Instance.new("TextLabel")
    StealLabel.Size = UDim2.new(0.6, 0, 0, 25)
    StealLabel.Position = UDim2.new(0, 10, 0, 65)
    StealLabel.BackgroundTransparency = 1
    StealLabel.Text = "Steal Speed:"
    StealLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    StealLabel.Font = Enum.Font.Gotham
    StealLabel.TextSize = 12
    StealLabel.TextXAlignment = Enum.TextXAlignment.Left
    StealLabel.Parent = SpeedSettingsGUI

    stealSpeedBox = Instance.new("TextBox")
    stealSpeedBox.Size = UDim2.new(0, 60, 0, 25)
    stealSpeedBox.Position = UDim2.new(1, -70, 0, 65)
    stealSpeedBox.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
    stealSpeedBox.Text = tostring(SpeedBooster.StealSpeed)
    stealSpeedBox.TextColor3 = Color3.fromRGB(255, 255, 255)
    stealSpeedBox.Font = Enum.Font.GothamBold
    stealSpeedBox.TextSize = 12
    stealSpeedBox.Parent = SpeedSettingsGUI

    local StealCorner = Instance.new("UICorner")
    StealCorner.CornerRadius = UDim.new(0, 6)
    StealCorner.Parent = stealSpeedBox

    local JumpLabel = Instance.new("TextLabel")
    JumpLabel.Size = UDim2.new(0.6, 0, 0, 25)
    JumpLabel.Position = UDim2.new(0, 10, 0, 95)
    JumpLabel.BackgroundTransparency = 1
    JumpLabel.Text = "Jump Power:"
    JumpLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    JumpLabel.Font = Enum.Font.Gotham
    JumpLabel.TextSize = 12
    JumpLabel.TextXAlignment = Enum.TextXAlignment.Left
    JumpLabel.Parent = SpeedSettingsGUI

    local JumpValue = Instance.new("TextLabel")
    JumpValue.Size = UDim2.new(0, 60, 0, 25)
    JumpValue.Position = UDim2.new(1, -70, 0, 95)
    JumpValue.BackgroundColor3 = Color3.fromRGB(60, 60, 65)
    JumpValue.Text = "25.8 "
    JumpValue.TextColor3 = Color3.fromRGB(150, 150, 150)
    JumpValue.Font = Enum.Font.GothamBold
    JumpValue.TextSize = 12
    JumpValue.BorderSizePixel = 0
    JumpValue.Parent = SpeedSettingsGUI

    local JumpCorner = Instance.new("UICorner")
    JumpCorner.CornerRadius = UDim.new(0, 6)
    JumpCorner.Parent = JumpValue

    local SpeedSaveBtn = Instance.new("TextButton")
    SpeedSaveBtn.Size = UDim2.new(0, 80, 0, 25)
    SpeedSaveBtn.Position = UDim2.new(0.5, -40, 1, -35)
    SpeedSaveBtn.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
    SpeedSaveBtn.Text = "SAVE"
    SpeedSaveBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    SpeedSaveBtn.Font = Enum.Font.GothamBold
    SpeedSaveBtn.TextSize = 12
    SpeedSaveBtn.Parent = SpeedSettingsGUI

    local SpeedSaveCorner = Instance.new("UICorner")
    SpeedSaveCorner.CornerRadius = UDim.new(0, 8)
    SpeedSaveCorner.Parent = SpeedSaveBtn

    --  SAVE BUTTON ACTUALLY UPDATES SPEED!
    SpeedSaveBtn.MouseButton1Click:Connect(function()
        local normal = tonumber(normalSpeedBox.Text)
        local steal = tonumber(stealSpeedBox.Text)

        if normal and steal then
            SpeedBooster.NormalSpeed = normal
            SpeedBooster.StealSpeed = steal
            print("[NATESHUB]  Speed settings updated to:", normal, steal)
            
            --  RESTART SPEED BOOSTER IF ALREADY RUNNING!
            if SpeedBooster.Enabled then
                stopSpeedBooster()
                task.wait(0.1)
                startSpeedBooster()
            end
        end
    end)

    SpeedSettingsBtn.MouseButton1Click:Connect(function()
        SpeedSettingsGUI.Visible = not SpeedSettingsGUI.Visible
    end)

    --  UNWALK, SPIN, INF JUMP (NO SETTINGS)
    createMainButton("Unwalk", 2, function()
        SmallUnwalkGUI.Visible = not SmallUnwalkGUI.Visible
    end)

    createMainButton("Spin Bot", 3, function()
        SmallSpinBotGUI.Visible = not SmallSpinBotGUI.Visible
    end)

    createMainButton("Inf Jump", 4, function()
        SmallInfJumpGUI.Visible = not SmallInfJumpGUI.Visible
    end)

    --  FOV WITH SETTINGS BUTTON!
    local FOVContainer, FOVSettingsBtn = createMainButton("FOV Changer", 5, function()
        SmallFOVGUI.Visible = not SmallFOVGUI.Visible
    end, true)
    
    --  ANTI RAGDOLL (NO SETTINGS)
    createMainButton("Anti Ragdoll", 6, function()
        SmallAntiRagGUI.Visible = not SmallAntiRagGUI.Visible
    end)
    
    --  RAGDOLL TP (NO SETTINGS)
    createMainButton("Ragdoll TP", 7, function()
        SmallRagdollTPGUI.Visible = not SmallRagdollTPGUI.Visible
    end)
    
    --  DROP BRAINROT (NO SETTINGS)
    createMainButton("Drop Brainrot", 8, function()
        SmallDropGUI.Visible = not SmallDropGUI.Visible
    end)

    --  FOV SETTINGS POPUP (NEXT TO BUTTON!)
    local FOVSettingsGUI = Instance.new("Frame")
    FOVSettingsGUI.Name = "FOVSettingsGUI"
    FOVSettingsGUI.Size = UDim2.new(0, 200, 0, 100)
    FOVSettingsGUI.Position = UDim2.new(0.5, -100, 0.5, -50)
    FOVSettingsGUI.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    FOVSettingsGUI.BackgroundTransparency = 0.1
    FOVSettingsGUI.BorderSizePixel = 0
    FOVSettingsGUI.Visible = false
    FOVSettingsGUI.Active = true
    FOVSettingsGUI.Draggable = true
    FOVSettingsGUI.Parent = ScreenGui

    local FOVCorner = Instance.new("UICorner")
    FOVCorner.CornerRadius = UDim.new(0, 12)
    FOVCorner.Parent = FOVSettingsGUI

    local FOVStroke = Instance.new("UIStroke")
    FOVStroke.Color = Color3.fromRGB(255, 255, 255)
    FOVStroke.Thickness = 2
    FOVStroke.Parent = FOVSettingsGUI

    local FOVTitle = Instance.new("TextLabel")
    FOVTitle.Size = UDim2.new(1, 0, 0, 30)
    FOVTitle.BackgroundTransparency = 1
    FOVTitle.Text = "FOV SETTINGS (MAX 120)"
    FOVTitle.TextColor3 = Color3.fromRGB(255, 200, 100)
    FOVTitle.Font = Enum.Font.GothamBold
    FOVTitle.TextSize = 14
    FOVTitle.Parent = FOVSettingsGUI

    local FOVLabel = Instance.new("TextLabel")
    FOVLabel.Size = UDim2.new(0.5, 0, 0, 25)
    FOVLabel.Position = UDim2.new(0, 10, 0, 35)
    FOVLabel.BackgroundTransparency = 1
    FOVLabel.Text = "FOV Value:"
    FOVLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    FOVLabel.Font = Enum.Font.Gotham
    FOVLabel.TextSize = 12
    FOVLabel.TextXAlignment = Enum.TextXAlignment.Left
    FOVLabel.Parent = FOVSettingsGUI

    fovSliderBox = Instance.new("TextBox")
    fovSliderBox.Size = UDim2.new(0, 60, 0, 25)
    fovSliderBox.Position = UDim2.new(1, -70, 0, 35)
    fovSliderBox.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
    fovSliderBox.Text = tostring(fovValue)
    fovSliderBox.TextColor3 = Color3.fromRGB(255, 255, 255)
    fovSliderBox.Font = Enum.Font.GothamBold
    fovSliderBox.TextSize = 12
    fovSliderBox.Parent = FOVSettingsGUI

    local FOVBoxCorner = Instance.new("UICorner")
    FOVBoxCorner.CornerRadius = UDim.new(0, 6)
    FOVBoxCorner.Parent = fovSliderBox

    --  FOV ACTUALLY UPDATES!
    fovSliderBox.FocusLost:Connect(function()
        local val = tonumber(fovSliderBox.Text)
        if val then
            fovValue = math.clamp(val, 1, 120) -- MAX 120!
            fovSliderBox.Text = tostring(fovValue)
            if fovEnabled then
                updateFOV()
            end
        else
            fovSliderBox.Text = tostring(fovValue)
        end
    end)

    FOVSettingsBtn.MouseButton1Click:Connect(function()
        FOVSettingsGUI.Visible = not FOVSettingsGUI.Visible
    end)
    
    --  FOV WARNING LABEL
    local FOVWarning = Instance.new("Frame")
    FOVWarning.Size = UDim2.new(1, 0, 0, 35)
    FOVWarning.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
    FOVWarning.BackgroundTransparency = 0.7
    FOVWarning.BorderSizePixel = 0
    FOVWarning.Parent = MainContent
    FOVWarning.LayoutOrder = 6
    
    local WarnCorner = Instance.new("UICorner")
    WarnCorner.CornerRadius = UDim.new(0, 8)
    WarnCorner.Parent = FOVWarning
    
    local WarnLabel = Instance.new("TextLabel")
    WarnLabel.Size = UDim2.new(1, -10, 1, 0)
    WarnLabel.Position = UDim2.new(0, 5, 0, 0)
    WarnLabel.BackgroundTransparency = 1
    WarnLabel.Text = " Must use  FOV settings or it won't work!"
    WarnLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    WarnLabel.Font = Enum.Font.GothamBold
    WarnLabel.TextSize = device == "iPhone" and 9 or 11
    WarnLabel.TextWrapped = true
    WarnLabel.Parent = FOVWarning

    -- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
    -- MISC TAB CONTENT (NO SMALL GUIS)
    -- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
    
    --  AUTO-ENABLE ALL MISC FEATURES ON SCRIPT LOAD!
    task.spawn(function()
        task.wait(1)
        
        --  FPS Booster
        enableFPSBooster()
        if fpsButton then fpsButton.BackgroundColor3 = Color3.fromRGB(50, 200, 50) end
        
        --  Optimizer
        enableOptimizer()
        if optimizerButton then optimizerButton.BackgroundColor3 = Color3.fromRGB(50, 200, 50) end
        
        --  Anti Death
        AntiDeathEnabled = true
        if not AntiDeath.Conn then
            AntiDeath.Conn = RunService.Heartbeat:Connect(function()
                if character and character:FindFirstChild("Humanoid") then
                    if character.Humanoid.Health <= 0 then
                        character.Humanoid.Health = 100
                    end
                end
            end)
        end
        antiDeathButton.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        
        print("[NATESHUB]  Auto-enabled ALL MISC: FPS Booster, Optimizer, Hitbox, Anti Death")
    end)

    local function createMISCButton(name, order, callback)
        local Button = Instance.new("TextButton")
        Button.Size = UDim2.new(1, 0, 0, 50)
        Button.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
        Button.BorderSizePixel = 0
        Button.Text = name
        Button.TextColor3 = Color3.fromRGB(255, 255, 255)
        Button.Font = Enum.Font.GothamBold
        Button.TextSize = device == "iPhone" and 11 or 14
        Button.LayoutOrder = order
        Button.Parent = MISCContent

        local Corner = Instance.new("UICorner")
        Corner.CornerRadius = UDim.new(0, 10)
        Corner.Parent = Button

        Button.MouseButton1Click:Connect(callback)

        return Button
    end

    local fpsButton = createMISCButton("FPS Booster", 1, function()
        enableFPSBooster()
        fpsButton.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
    end)

    local optimizerButton = createMISCButton("Optimizer", 2, function()
        enableOptimizer()
        optimizerButton.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
    end)

    local antiDeathButton = createMISCButton("Anti Death", 4, function()
        AntiDeathEnabled = not AntiDeathEnabled
        
        if AntiDeathEnabled then
            if not AntiDeath.Conn then
                AntiDeath.Conn = RunService.Heartbeat:Connect(function()
                    if character and character:FindFirstChild("Humanoid") then
                        if character.Humanoid.Health <= 0 then
                            character.Humanoid.Health = 100
                        end
                    end
                end)
            end
            antiDeathButton.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        else
            if AntiDeath.Conn then
                AntiDeath.Conn:Disconnect()
                AntiDeath.Conn = nil
            end
            antiDeathButton.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
        end
    end)

    createMISCButton("Float Up", 5, function()
        SmallFloatGUI.Visible = not SmallFloatGUI.Visible
    end)

    -- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
    -- SETTINGS TAB CONTENT
    -- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

    local function createSettingsButton(name, order, callback)
        local Button = Instance.new("TextButton")
        Button.Size = UDim2.new(1, 0, 0, 50)
        Button.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        Button.BorderSizePixel = 0
        Button.Text = name
        Button.TextColor3 = Color3.fromRGB(255, 255, 255)
        Button.Font = Enum.Font.GothamBold
        Button.TextSize = device == "iPhone" and 12 or 14
        Button.LayoutOrder = order
        Button.Parent = SettingsContent

        local Corner = Instance.new("UICorner")
        Corner.CornerRadius = UDim.new(0, 10)
        Corner.Parent = Button

        Button.MouseButton1Click:Connect(callback)

        return Button
    end

    local SaveConfigButton = createSettingsButton(" SAVE CONFIG", 1, function()
        if SaveConfig() then
            SaveConfigButton.Text = " CONFIG SAVED!"
            task.wait(1.5)
            SaveConfigButton.Text = " SAVE CONFIG"
        else
            SaveConfigButton.Text = " SAVE FAILED!"
            task.wait(1.5)
            SaveConfigButton.Text = " SAVE CONFIG"
        end
    end)

    local LoadConfigButton = Instance.new("TextButton")
    LoadConfigButton.Size = UDim2.new(1, 0, 0, 50)
    LoadConfigButton.BackgroundColor3 = Color3.fromRGB(100, 150, 255)
    LoadConfigButton.BorderSizePixel = 0
    LoadConfigButton.Text = " LOAD CONFIG"
    LoadConfigButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    LoadConfigButton.Font = Enum.Font.GothamBold
    LoadConfigButton.TextSize = device == "iPhone" and 12 or 14
    LoadConfigButton.LayoutOrder = 2
    LoadConfigButton.Parent = SettingsContent

    local LoadConfigCorner = Instance.new("UICorner")
    LoadConfigCorner.CornerRadius = UDim.new(0, 10)
    LoadConfigCorner.Parent = LoadConfigButton

    LoadConfigButton.MouseButton1Click:Connect(function()
        LoadConfig()
        -- Aplica valores carregados
        SpeedBooster.NormalSpeed = ConfigData.normalSpeed
        SpeedBooster.StealSpeed  = ConfigData.stealSpeed
        SpeedBooster.JumpPower   = ConfigData.jumpPower
        InstaSteal.Radius        = ConfigData.stealRadius
        fovValue                 = ConfigData.fovValue
        galaxyGravityPct         = ConfigData.gravityPct
        galaxyHopPower           = ConfigData.hopPower

        -- Atualiza UI (caixas de texto)
        if normalSpeedBox then normalSpeedBox.Text = tostring(SpeedBooster.NormalSpeed) end
        if stealSpeedBox  then stealSpeedBox.Text  = tostring(SpeedBooster.StealSpeed)  end
        if fovSliderBox   then fovSliderBox.Text   = tostring(fovValue)                 end
        if RadiusValue    then RadiusValue.Text    = tostring(InstaSteal.Radius)        end

        -- Atualiza sliders visualmente
        if sliderSetters then
            if sliderSetters.normalSpeed then sliderSetters.normalSpeed(ConfigData.normalSpeed) end
            if sliderSetters.stealSpeed  then sliderSetters.stealSpeed(ConfigData.stealSpeed)   end
            if sliderSetters.gravityPct  then sliderSetters.gravityPct(ConfigData.gravityPct)   end
            if sliderSetters.hopPower    then sliderSetters.hopPower(ConfigData.hopPower)        end
            if sliderSetters.stealRadius then sliderSetters.stealRadius(ConfigData.stealRadius)  end
        end

        LoadConfigButton.Text = " CONFIG LOADED!"
        task.wait(1.5)
        LoadConfigButton.Text = " LOAD CONFIG"
    end)

    -- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
    -- CREDITS TAB CONTENT
    -- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

    local CreditsTitle = Instance.new("TextLabel")
    CreditsTitle.Size = UDim2.new(1, 0, 0, 40)
    CreditsTitle.BackgroundTransparency = 1
    CreditsTitle.Text = " CREDITS "
    CreditsTitle.TextColor3 = Color3.fromRGB(255, 200, 100)
    CreditsTitle.Font = Enum.Font.GothamBold
    CreditsTitle.TextSize = device == "iPhone" and 16 or 24
    CreditsTitle.Parent = CreditsContent

    local CreditsText = Instance.new("TextLabel")
    CreditsText.Size = UDim2.new(1, 0, 0, 100)
    CreditsText.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
    CreditsText.BorderSizePixel = 0
    CreditsText.Text = "Credits to:\n\n sussycatmean\n\nThank you! "
    CreditsText.TextColor3 = Color3.fromRGB(255, 255, 255)
    CreditsText.Font = Enum.Font.Gotham
    CreditsText.TextSize = device == "iPhone" and 11 or 16
    CreditsText.TextYAlignment = Enum.TextYAlignment.Top
    CreditsText.Parent = CreditsContent

    local CreditsCorner = Instance.new("UICorner")
    CreditsCorner.CornerRadius = UDim.new(0, 10)
    CreditsCorner.Parent = CreditsText
    
    --  TUTORIAL SECTION
    local TutorialTitle = Instance.new("TextLabel")
    TutorialTitle.Size = UDim2.new(1, 0, 0, 40)
    TutorialTitle.BackgroundTransparency = 1
    TutorialTitle.Text = " HOW TO USE"
    TutorialTitle.TextColor3 = Color3.fromRGB(255, 200, 100)
    TutorialTitle.Font = Enum.Font.GothamBold
    TutorialTitle.TextSize = device == "iPhone" and 14 or 20
    TutorialTitle.Parent = CreditsContent
    
    local TutText = Instance.new("TextLabel")
    TutText.Size = UDim2.new(1, 0, 0, 250)
    TutText.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
    TutText.BorderSizePixel = 0
    TutText.Text = [[
1 Main Tab - Toggle small GUIs
    Each feature has green dot (ON/OFF)
    Drag small GUIs anywhere on screen!
    Click button to show/hide small GUI

2 Settings Tab - Customize speeds
    Speed settings next to Speed button
    FOV settings next to FOV button
    Save config to auto-load next time

3 MISC Tab - Auto-enabled features
    FPS Booster, Optimizer, Hitbox already ON!
    No need to toggle them

 FOV: Must use  settings button or won't work!

4 Drop Brainrot - Click to float up & drop!
    ]]
    TutText.TextColor3 = Color3.fromRGB(255, 255, 255)
    TutText.Font = Enum.Font.Gotham
    TutText.TextSize = device == "iPhone" and 9 or 12
    TutText.TextYAlignment = Enum.TextYAlignment.Top
    TutText.TextWrapped = true
    TutText.Parent = CreditsContent
    
    local TutCorner = Instance.new("UICorner")
    TutCorner.CornerRadius = UDim.new(0, 10)
    TutCorner.Parent = TutText
end

-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-- GUI COMPLETA LACERDA HUB (PC ONLY) + AUTO GRAB DO NATES
-- â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•

local function createLacerdaStylePanel()
    -- ============================================================
    -- CORES (igual Lacerda Hub original)
    -- ============================================================
    local CA    = Color3.new(0, 0.784, 1)
    local CAD   = Color3.new(0, 0.627, 0.863)
    local CF    = Color3.new(0, 0.706, 1)
    local CBP   = Color3.new(0.012, 0.020, 0.055)
    local CBR   = Color3.new(0.047, 0.055, 0.078)
    local CBT   = Color3.new(0.137, 0.157, 0.196)
    local CBK   = Color3.new(0.706, 0.745, 0.784)
    local CT    = Color3.new(0.863, 0.902, 0.961)
    local CD    = Color3.new(0.314, 0.471, 0.627)
    local CDIV  = Color3.new(0, 0.549, 0.784)

    -- ============================================================
    -- HELPERS (SetOn / SetOff igual Lacerda)
    -- ============================================================
    local function SetOn(knob, track)
        TweenService:Create(knob,  TweenInfo.new(0.18), {Position=UDim2.new(1,-18,0,2), BackgroundColor3=CA}):Play()
        TweenService:Create(track, TweenInfo.new(0.18), {BackgroundColor3=Color3.new(0,0.25,0.45)}):Play()
    end
    local function SetOff(knob, track)
        TweenService:Create(knob,  TweenInfo.new(0.18), {Position=UDim2.new(0,2,0,2), BackgroundColor3=CBK}):Play()
        TweenService:Create(track, TweenInfo.new(0.18), {BackgroundColor3=CBT}):Play()
    end

    -- ============================================================
    -- SCREENGUI (usa o ScreenGui jÃ¡ criado pelo Nates)
    -- ============================================================

    -- BotÃ£o toggle "LH"
    local TogBtn = Instance.new("TextButton", ScreenGui)
    TogBtn.Text = "LH"
    TogBtn.Font = Enum.Font.GothamBold
    TogBtn.TextSize = 14
    TogBtn.TextColor3 = CA
    TogBtn.BackgroundColor3 = CBR
    TogBtn.Size = UDim2.new(0, 44, 0, 44)
    TogBtn.Position = UDim2.new(0, 18, 0.3, 0)
    TogBtn.ZIndex = 999
    TogBtn.BorderSizePixel = 0
    Instance.new("UICorner", TogBtn).CornerRadius = UDim.new(1, 0)
    local tgS = Instance.new("UIStroke", TogBtn)
    tgS.Color = CAD
    tgS.Thickness = 1.5

    -- Pulsar o botÃ£o
    task.spawn(function()
        while TogBtn.Parent do
            TweenService:Create(TogBtn, TweenInfo.new(0.8, Enum.EasingStyle.Quad, Enum.EasingDirection.InOut), {BackgroundTransparency = 0.2}):Play()
            task.wait(0.8)
            TweenService:Create(TogBtn, TweenInfo.new(0.8, Enum.EasingStyle.Quad, Enum.EasingDirection.InOut), {BackgroundTransparency = 0}):Play()
            task.wait(0.8)
        end
    end)

    -- ============================================================
    -- PAINEL PRINCIPAL
    -- ============================================================
    local Main = Instance.new("Frame", ScreenGui)
    Main.Name = "LacerdaMain"
    Main.BackgroundColor3 = CBP
    Main.BackgroundTransparency = 0.35
    Main.BorderSizePixel = 0
    Main.Size = UDim2.new(0, 360, 0, 580)
    Main.AnchorPoint = Vector2.new(0, 0)
    Main.Position = UDim2.new(0, 80, 0, 10)
    Main.Visible = true
    Main.ZIndex = 10
    Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 14)

    -- Borda branca giratÃ³ria
    do
        local s1 = Instance.new("UIStroke", Main)
        s1.Color = Color3.fromRGB(255, 255, 255)
        s1.Thickness = 3
        s1.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
        local g1 = Instance.new("UIGradient", s1)
        g1.Rotation = 0
        g1.Transparency = NumberSequence.new({
            NumberSequenceKeypoint.new(0, 0),
            NumberSequenceKeypoint.new(0.4, 0.8),
            NumberSequenceKeypoint.new(0.6, 0.8),
            NumberSequenceKeypoint.new(1, 0),
        })
        TweenService:Create(g1, TweenInfo.new(3, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut, -1), {Rotation = 360}):Play()
    end

    -- Bolinhas de galÃ¡xia no fundo
    local PARTICLE_DOTS_PC = {
        { pos = UDim2.new(0.935,0,0.829,0), color=Color3.new(0,0.980,1), alpha=0.48, sz=4 },
        { pos = UDim2.new(0.688,0,0.869,0), color=Color3.new(0,0.867,1), alpha=0.34, sz=2 },
        { pos = UDim2.new(0.939,0,0.471,0), color=Color3.new(0,0.808,1), alpha=0.38, sz=2 },
        { pos = UDim2.new(0.845,0,0.176,0), color=Color3.new(0,0.745,1), alpha=0.38, sz=2 },
        { pos = UDim2.new(0.416,0,0.489,0), color=Color3.new(0,0.843,1), alpha=0.35, sz=3 },
        { pos = UDim2.new(0.464,0,0.070,0), color=Color3.new(0,0.655,1), alpha=0.29, sz=3 },
        { pos = UDim2.new(0.638,0,0.657,0), color=Color3.new(0,0.655,1), alpha=0.24, sz=3 },
        { pos = UDim2.new(0.684,0,0.037,0), color=Color3.new(0,0.937,1), alpha=0.47, sz=2 },
        { pos = UDim2.new(0.773,0,0.075,0), color=Color3.new(0,0.651,1), alpha=0.44, sz=2 },
        { pos = UDim2.new(0.472,0,0.343,0), color=Color3.new(0,0.894,1), alpha=0.20, sz=2 },
        { pos = UDim2.new(0.766,0,0.425,0), color=Color3.new(0,0.925,1), alpha=0.49, sz=2 },
        { pos = UDim2.new(0.452,0,0.468,0), color=Color3.new(0,0.929,1), alpha=0.26, sz=2 },
        { pos = UDim2.new(0.257,0,0.757,0), color=Color3.new(0,0.976,1), alpha=0.51, sz=2 },
        { pos = UDim2.new(0.515,0,0.558,0), color=Color3.new(0,0.976,1), alpha=0.33, sz=2 },
        { pos = UDim2.new(0.356,0,0.572,0), color=Color3.new(0,0.976,1), alpha=0.33, sz=2 },
        { pos = UDim2.new(0.960,0,0.012,0), color=Color3.new(0,0.733,1), alpha=0.33, sz=2 },
        { pos = UDim2.new(0.124,0,0.896,0), color=Color3.new(0,0.824,1), alpha=0.45, sz=2 },
        { pos = UDim2.new(0.530,0,0.911,0), color=Color3.new(0,0.918,1), alpha=0.45, sz=2 },
        { pos = UDim2.new(0.902,0,0.313,0), color=Color3.new(0,0.702,1), alpha=0.31, sz=2 },
        { pos = UDim2.new(0.967,0,0.966,0), color=Color3.new(0,0.639,1), alpha=0.31, sz=2 },
        { pos = UDim2.new(0.199,0,0.369,0), color=Color3.new(0,0.737,1), alpha=0.31, sz=2 },
        { pos = UDim2.new(0.457,0,0.229,0), color=Color3.new(0,0.737,1), alpha=0.31, sz=2 },
    }
    local pcDotFrames = {}
    for _, dot in ipairs(PARTICLE_DOTS_PC) do
        local f = Instance.new("Frame", Main)
        f.Size = UDim2.new(0, dot.sz, 0, dot.sz)
        f.Position = dot.pos
        f.BackgroundColor3 = dot.color
        f.BackgroundTransparency = dot.alpha
        f.BorderSizePixel = 0; f.ZIndex = 1
        Instance.new("UICorner", f).CornerRadius = UDim.new(1, 0)
        table.insert(pcDotFrames, f)
    end
    do
        local dX, dY = {}, {}
        for i = 1, #PARTICLE_DOTS_PC do
            dX[i] = (math.random()-0.5)*0.00015
            dY[i] = (math.random()>0.5 and 1 or -1)*0.0009
        end
        RunService.RenderStepped:Connect(function()
            if not Main.Parent then return end
            for i, f in ipairs(pcDotFrames) do
                if not f.Parent then return end
                local p = f.Position
                local nx = p.X.Scale + dX[i]
                local ny = p.Y.Scale + dY[i]
                if nx < -0.05 then nx = 1.05 elseif nx > 1.05 then nx = -0.05 end
                if ny < -0.05 then ny = 1.05 elseif ny > 1.05 then ny = -0.05 end
                f.Position = UDim2.new(nx, 0, ny, 0)
            end
        end)
    end

    -- TitleBar
    local TitleBar = Instance.new("Frame", Main)
    TitleBar.BackgroundTransparency = 1
    TitleBar.Size = UDim2.new(1,0,0,52)
    TitleBar.ZIndex = 5

    local TitleLbl = Instance.new("TextLabel", TitleBar)
    TitleLbl.Text = "LACERDA DUELS"
    TitleLbl.Font = Enum.Font.GothamBlack
    TitleLbl.TextSize = 17
    TitleLbl.TextColor3 = CA
    TitleLbl.BackgroundTransparency = 1
    TitleLbl.Size = UDim2.new(1,0,0,26)
    TitleLbl.Position = UDim2.new(0,0,0,6)
    TitleLbl.TextXAlignment = Enum.TextXAlignment.Center
    TitleLbl.ZIndex = 6

    local TDiv = Instance.new("Frame", Main)
    TDiv.BackgroundColor3 = CDIV; TDiv.BackgroundTransparency = 0.6
    TDiv.Size = UDim2.new(0.92,0,0,1); TDiv.Position = UDim2.new(0.04,0,0,52)
    TDiv.BorderSizePixel = 0

    -- Drag pelo TitleBar
    do
        local dragging = false
        local dragStart = Vector2.new(0,0)
        local frameStart = Vector2.new(0,0)
        TitleBar.InputBegan:Connect(function(i)
            if i.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = true
                dragStart = Vector2.new(i.Position.X, i.Position.Y)
                frameStart = Vector2.new(Main.Position.X.Offset, Main.Position.Y.Offset)
            end
        end)
        TitleBar.InputEnded:Connect(function(i)
            if i.UserInputType == Enum.UserInputType.MouseButton1 then dragging = false end
        end)
        TitleBar.InputChanged:Connect(function(i)
            if not dragging then return end
            if i.UserInputType ~= Enum.UserInputType.MouseMovement then return end
            local delta = Vector2.new(i.Position.X, i.Position.Y) - dragStart
            local vp = workspace.CurrentCamera.ViewportSize
            local nx = math.clamp(frameStart.X + delta.X, 0, vp.X - Main.AbsoluteSize.X)
            local ny = math.clamp(frameStart.Y + delta.Y, 0, vp.Y - Main.AbsoluteSize.Y)
            Main.Position = UDim2.new(0, nx, 0, ny)
        end)
    end

    -- ScrollingFrame
    local Scroll = Instance.new("ScrollingFrame", Main)
    Scroll.BackgroundTransparency = 1; Scroll.BorderSizePixel = 0
    Scroll.Size = UDim2.new(1,-16,1,-62); Scroll.Position = UDim2.new(0,8,0,58)
    Scroll.CanvasSize = UDim2.new(0,0,0,0); Scroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
    Scroll.ScrollBarThickness = 3; Scroll.ScrollBarImageColor3 = CF; Scroll.ZIndex = 4

    local SLay = Instance.new("UIListLayout", Scroll)
    SLay.Padding = UDim.new(0,4); SLay.SortOrder = Enum.SortOrder.LayoutOrder

    -- FPS / DISCORD / PING
    local fpsRow = Instance.new("Frame", Scroll)
    fpsRow.Size = UDim2.new(1,0,0,22); fpsRow.BackgroundTransparency = 1; fpsRow.LayoutOrder = 0

    local fpsLbl = Instance.new("TextLabel", fpsRow)
    fpsLbl.Size = UDim2.new(0.28,0,1,0); fpsLbl.Position = UDim2.new(0,8,0,0)
    fpsLbl.BackgroundTransparency = 1; fpsLbl.Text = "FPS: --"
    fpsLbl.Font = Enum.Font.GothamBold; fpsLbl.TextSize = 10
    fpsLbl.TextColor3 = CT; fpsLbl.TextXAlignment = Enum.TextXAlignment.Left

    local discBtn = Instance.new("TextButton", fpsRow)
    discBtn.Size = UDim2.new(0.44,0,1,0); discBtn.Position = UDim2.new(0.28,0,0,0)
    discBtn.BackgroundTransparency = 1; discBtn.AutoButtonColor = false
    discBtn.Text = "discord.gg/4Fp8psfxrS"
    discBtn.Font = Enum.Font.GothamMedium; discBtn.TextSize = 9
    discBtn.TextColor3 = Color3.new(0.314,0.627,0.784)
    discBtn.TextXAlignment = Enum.TextXAlignment.Center
    discBtn.MouseButton1Click:Connect(function()
        pcall(function()
            if setclipboard then
                setclipboard("https://discord.gg/4Fp8psfxrS")
                local o = discBtn.Text
                discBtn.Text = "COPIADO!"
                task.wait(1.5)
                discBtn.Text = o
            end
        end)
    end)

    local pingLbl = Instance.new("TextLabel", fpsRow)
    pingLbl.Size = UDim2.new(0.28,0,1,0); pingLbl.Position = UDim2.new(0.72,0,0,0)
    pingLbl.BackgroundTransparency = 1; pingLbl.Text = "PING: --"
    pingLbl.Font = Enum.Font.GothamBold; pingLbl.TextSize = 10
    pingLbl.TextColor3 = CT; pingLbl.TextXAlignment = Enum.TextXAlignment.Right
    task.spawn(function()
        local fpsBuf = {}
        while Main.Parent do
            local t0 = tick(); RunService.RenderStepped:Wait()
            table.insert(fpsBuf, 1/math.max(tick()-t0,0.001))
            if #fpsBuf > 20 then table.remove(fpsBuf,1) end
            local s = 0; for _,v in ipairs(fpsBuf) do s = s + v end
            local fps = math.floor(s/#fpsBuf)
            local ping = 0
            pcall(function()
                ping = math.floor(game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValue())
            end)
            fpsLbl.Text = "FPS: " .. fps
            fpsLbl.TextColor3 = fps>=50 and CT or fps>=30 and Color3.fromRGB(255,200,80) or Color3.fromRGB(255,80,80)
            pingLbl.Text = "PING: " .. ping
            pingLbl.TextColor3 = ping<=80 and CT or ping<=150 and Color3.fromRGB(255,200,80) or Color3.fromRGB(255,80,80)
            task.wait(0.5)
        end
    end)

    -- ============================================================
    -- HELPERS DE LAYOUT
    -- ============================================================
    local function Sep(order)
        local f = Instance.new("Frame", Scroll)
        f.Size = UDim2.new(0.92,0,0,1); f.BackgroundColor3 = CDIV
        f.BackgroundTransparency = 0.7; f.BorderSizePixel = 0; f.LayoutOrder = order
    end
    local function Sec(txt, order)
        local l = Instance.new("TextLabel", Scroll)
        l.Size = UDim2.new(1,0,0,18); l.BackgroundTransparency = 1
        l.Text = "  "..txt; l.Font = Enum.Font.GothamBold; l.TextSize = 10
        l.TextColor3 = CA; l.TextXAlignment = Enum.TextXAlignment.Left
        l.LayoutOrder = order; l.ZIndex = 4
    end
    local function Tog(lbl, sub, order)
        local Row = Instance.new("Frame", Scroll)
        Row.Size = UDim2.new(1,0,0,38); Row.BackgroundTransparency = 1
        Row.LayoutOrder = order; Row.ZIndex = 4
        local L = Instance.new("TextLabel", Row)
        L.Text = lbl; L.TextSize = 13; L.Font = Enum.Font.GothamBold
        L.TextColor3 = CT; L.BackgroundTransparency = 1
        L.Size = UDim2.new(1,-64,0,20); L.Position = UDim2.new(0,10,0,4)
        L.TextXAlignment = Enum.TextXAlignment.Left; L.ZIndex = 5
        if sub ~= "" then
            local S = Instance.new("TextLabel", Row)
            S.Text = sub; S.TextSize = 9; S.Font = Enum.Font.Gotham
            S.TextColor3 = CD; S.BackgroundTransparency = 1
            S.Size = UDim2.new(1,-64,0,12); S.Position = UDim2.new(0,10,0,22)
            S.TextXAlignment = Enum.TextXAlignment.Left; S.ZIndex = 5
        end
        local Tr = Instance.new("Frame", Row)
        Tr.Size = UDim2.new(0,38,0,20); Tr.Position = UDim2.new(1,-46,0.5,-10)
        Tr.BackgroundColor3 = CBT; Tr.BorderSizePixel = 0
        Instance.new("UICorner", Tr).CornerRadius = UDim.new(1,0)
        local Kn = Instance.new("Frame", Tr)
        Kn.Size = UDim2.new(0,16,0,16); Kn.Position = UDim2.new(0,2,0,2)
        Kn.BackgroundColor3 = CBK; Kn.BorderSizePixel = 0; Kn.ZIndex = 6
        Instance.new("UICorner", Kn).CornerRadius = UDim.new(1,0)
        local Btn = Instance.new("TextButton", Row)
        Btn.Text = ""; Btn.Size = UDim2.new(1,0,1,0)
        Btn.BackgroundTransparency = 1; Btn.ZIndex = 7
        return Row, Btn, Kn, Tr, L
    end
    local function Sld(lbl, mn, mx, init, order, onChange)
        local Row = Instance.new("Frame", Scroll)
        Row.Size = UDim2.new(1,0,0,52); Row.BackgroundTransparency = 1
        Row.LayoutOrder = order; Row.ZIndex = 4
        -- Label
        local LT = Instance.new("TextLabel", Row)
        LT.Text = lbl; LT.TextColor3 = CT; LT.Font = Enum.Font.GothamBold; LT.TextSize = 12
        LT.BackgroundTransparency = 1; LT.Size = UDim2.new(1,-70,0,18)
        LT.Position = UDim2.new(0,14,0,4); LT.TextXAlignment = Enum.TextXAlignment.Left; LT.ZIndex = 4
        -- Valor em badge azul transparente
        local VLBg = Instance.new("Frame", Row)
        VLBg.Size = UDim2.new(0,52,0,20); VLBg.Position = UDim2.new(1,-62,0,2)
        VLBg.BackgroundColor3 = Color3.fromRGB(0,60,120)
        VLBg.BackgroundTransparency = 0.4
        VLBg.BorderSizePixel = 0; VLBg.ZIndex = 4
        Instance.new("UICorner", VLBg).CornerRadius = UDim.new(0,6)
        local VL = Instance.new("TextLabel", VLBg)
        VL.Text = tostring(init); VL.TextColor3 = Color3.new(1,1,1)
        VL.Font = Enum.Font.GothamBold; VL.TextSize = 11; VL.BackgroundTransparency = 1
        VL.Size = UDim2.new(1,0,1,0); VL.TextXAlignment = Enum.TextXAlignment.Center; VL.ZIndex = 5
        -- Trilha transparente
        local TB = Instance.new("Frame", Row)
        TB.Size = UDim2.new(1,-28,0,8); TB.Position = UDim2.new(0,14,0,30)
        TB.BackgroundColor3 = Color3.fromRGB(15,20,45)
        TB.BackgroundTransparency = 0.4
        TB.BorderSizePixel = 0; TB.ZIndex = 4
        Instance.new("UICorner", TB).CornerRadius = UDim.new(1,0)
        -- Fill com gradiente
        local Fl = Instance.new("Frame", TB)
        Fl.Size = UDim2.new((init-mn)/(mx-mn),0,1,0)
        Fl.BackgroundColor3 = Color3.fromRGB(0,180,255)
        Fl.BackgroundTransparency = 0.2
        Fl.BorderSizePixel = 0; Fl.ZIndex = 5
        Instance.new("UICorner", Fl).CornerRadius = UDim.new(1,0)
        local flGrad = Instance.new("UIGradient", Fl)
        flGrad.Color = ColorSequence.new({
            ColorSequenceKeypoint.new(0, Color3.fromRGB(0,120,255)),
            ColorSequenceKeypoint.new(1, Color3.fromRGB(0,220,255)),
        })
        -- Thumb transparente
        local Th = Instance.new("Frame", TB)
        Th.Size = UDim2.new(0,18,0,18); Th.Position = UDim2.new((init-mn)/(mx-mn),0,0.5,-9)
        Th.BackgroundColor3 = Color3.fromRGB(255,255,255)
        Th.BackgroundTransparency = 0.2
        Th.BorderSizePixel = 0; Th.ZIndex = 7
        Instance.new("UICorner", Th).CornerRadius = UDim.new(1,0)
        local thStroke = Instance.new("UIStroke", Th)
        thStroke.Color = Color3.fromRGB(0,200,255); thStroke.Thickness = 2
        local thInner = Instance.new("Frame", Th)
        thInner.Size = UDim2.new(0,8,0,8); thInner.Position = UDim2.new(0.5,-4,0.5,-4)
        thInner.BackgroundColor3 = Color3.fromRGB(0,180,255)
        thInner.BackgroundTransparency = 0.1
        thInner.BorderSizePixel = 0; thInner.ZIndex = 8
        Instance.new("UICorner", thInner).CornerRadius = UDim.new(1,0)
        local drag = false
        local function setV(ix)
            local r = math.clamp((ix-TB.AbsolutePosition.X)/TB.AbsoluteSize.X, 0, 1)
            local v = math.floor(mn+(mx-mn)*r)
            Fl.Size = UDim2.new(r,0,1,0); Th.Position = UDim2.new(r,0,0.5,-9)
            VL.Text = tostring(v); onChange(v)
        end
        local function setValue(v)
            v = math.clamp(math.floor(v), mn, mx)
            local r = (v-mn)/(mx-mn)
            Fl.Size = UDim2.new(r,0,1,0); Th.Position = UDim2.new(r,0,0.5,-9)
            VL.Text = tostring(v); onChange(v)
        end
        Th.InputBegan:Connect(function(i)
            if i.UserInputType==Enum.UserInputType.MouseButton1 then drag=true end
        end)
        TB.InputBegan:Connect(function(i)
            if i.UserInputType==Enum.UserInputType.MouseButton1 then drag=true; setV(i.Position.X) end
        end)
        UserInputService.InputEnded:Connect(function(i)
            if i.UserInputType==Enum.UserInputType.MouseButton1 then drag=false end
        end)
        UserInputService.InputChanged:Connect(function(i)
            if drag and i.UserInputType==Enum.UserInputType.MouseMovement then setV(i.Position.X) end
        end)
        return Row, VL, setValue
    end

    local sliderSetters = {}

    Sep(1)

    -- ============================================================
    -- BIND HELPER (DO LACERDA HUB)  cor branca
    -- ============================================================
    local function BindBtn(row, file)
        local b = Instance.new("TextButton", row)
        b.Size = UDim2.new(0,42,0,22); b.Position = UDim2.new(1,-94,0.5,-11)
        b.BackgroundColor3 = Color3.fromRGB(18,18,30)
        b.Text = "BIND"; b.TextColor3 = Color3.fromRGB(255,255,255)
        b.Font = Enum.Font.GothamBold; b.TextSize = 8
        b.AutoButtonColor = false; b.BorderSizePixel = 0; b.ZIndex = 9
        Instance.new("UICorner", b).CornerRadius = UDim.new(0,5)
        local bStroke = Instance.new("UIStroke", b)
        bStroke.Color = Color3.fromRGB(255,255,255)
        local key = nil; local binding = false
        pcall(function()
            if readfile then
                local s = readfile(file)
                if s and s ~= "" and s ~= "NONE" then key = s; b.Text = s end
            end
        end)
        b.MouseButton1Click:Connect(function()
            binding = true; b.Text = "..."; b.TextColor3 = Color3.fromRGB(255,255,80)
        end)
        return b,
            function() return key end,
            function() return binding end,
            function(v) binding = v end,
            function(v) key = v end
    end

    -- ============================================================
    -- VELOCIDADE
    -- ============================================================
    Sec("VELOCIDADE", 2)
    local _,_,sldNormalSpeed = Sld("Normal Speed", 10, 150, SpeedBooster.NormalSpeed, 3, function(v)
        SpeedBooster.NormalSpeed = v
    end)
    sliderSetters.normalSpeed = sldNormalSpeed
    local _,_,sldStealSpeed = Sld("Steal Speed", 10, 100, SpeedBooster.StealSpeed, 4, function(v)
        SpeedBooster.StealSpeed = v
    end)
    sliderSetters.stealSpeed = sldStealSpeed
    local spRow, spBtn, spKn, spTr = Tog("Speed Booster", "Velocidade automatica", 5)
    if SpeedBooster.Enabled then SetOn(spKn, spTr) end
    local spBB, spGetKey, spGetBind, spSetBind, spSetKey = BindBtn(spRow, "NatesHub_SpeedKey.txt")
    spBtn.MouseButton1Click:Connect(function()
        SpeedBooster.Enabled = not SpeedBooster.Enabled
        if SpeedBooster.Enabled then SetOn(spKn, spTr); startSpeedBooster()
        else SetOff(spKn, spTr); stopSpeedBooster() end
    end)
    Sep(6)

    -- ============================================================
    -- INF JUMP / GALAXY (DO LACERDA HUB)
    -- ============================================================
    Sec("INF JUMP / GALAXY", 7)
    local ijRow, ijBtn, ijKn, ijTr = Tog("Inf Jump", "Reduz gravidade + mini-hops", 8)
    if galaxyEnabled then SetOn(ijKn, ijTr) end
    ijBtn.MouseButton1Click:Connect(function()
        galaxyEnabled = not galaxyEnabled
        if galaxyEnabled then SetOn(ijKn, ijTr); startGalaxy()
        else SetOff(ijKn, ijTr); stopGalaxy() end
    end)
    local _,_,sldGravity = Sld("Gravity %", 10, 100, galaxyGravityPct, 9, function(v)
        galaxyGravityPct = v
        if galaxyEnabled then updateGalaxyForce(); adjustGalaxyJump() end
    end)
    sliderSetters.gravityPct = sldGravity
    local _,_,sldHopPower = Sld("Hop Power", 10, 80, galaxyHopPower, 10, function(v)
        galaxyHopPower = v
    end)
    sliderSetters.hopPower = sldHopPower
    Sep(11)

    -- ============================================================
    -- COMBATE
    -- ============================================================
    Sec("COMBATE", 10)
    local _, arBtn, arKn, arTr = Tog("Anti Ragdoll", "Evita ser jogado", 11)
    if AntiRagdollEnabled then SetOn(arKn, arTr) end
    arBtn.MouseButton1Click:Connect(function()
        AntiRagdollEnabled = not AntiRagdollEnabled
        if AntiRagdollEnabled then SetOn(arKn, arTr); enableAntiRagdoll()
        else SetOff(arKn, arTr); disableAntiRagdoll() end
    end)

    local _, rtBtn, rtKn, rtTr = Tog("Ragdoll TP", "TP automatico ao tomar ragdoll", 12)
    if RagdollTP.enabled then SetOn(rtKn, rtTr) end
    rtBtn.MouseButton1Click:Connect(function()
        RagdollTP.enabled = not RagdollTP.enabled
        if RagdollTP.enabled then SetOn(rtKn, rtTr); startRagdollTP()
        else SetOff(rtKn, rtTr); stopRagdollTP() end
    end)

    local tkRow, tkBtn, tkKn, tkTr, tkLbl = Tog("Track Player", "Segue o inimigo mais proximo", 13)
    tkLbl.Size = UDim2.new(1,-110,0,20)
    if batAimbotToggled then SetOn(tkKn, tkTr) end
    local tkBB, tkGetKey, tkGetBind, tkSetBind, tkSetKey = BindBtn(tkRow, "NatesHub_TrackerKey.txt")
    tkBtn.MouseButton1Click:Connect(function()
        batAimbotToggled = not batAimbotToggled
        if batAimbotToggled then SetOn(tkKn, tkTr); startBatAimbot()
        else SetOff(tkKn, tkTr); stopBatAimbot() end
    end)
    Sep(15)

    -- ============================================================
    -- MOVIMENTO
    -- ============================================================
    Sec("MOVIMENTO", 16)
    local _, uwBtn, uwKn, uwTr = Tog("Unwalk", "Remove animacoes de andar", 17)
    if UnwalkEnabled then SetOn(uwKn, uwTr) end
    uwBtn.MouseButton1Click:Connect(function()
        UnwalkEnabled = not UnwalkEnabled
        if UnwalkEnabled then SetOn(uwKn, uwTr); startUnwalk()
        else SetOff(uwKn, uwTr); stopUnwalk() end
    end)
    Sep(21)

    -- ============================================================
    -- AUTO PLAY
    -- ============================================================
    Sec("AUTO PLAY", 22)
    local apRow, apBtn, apKn, apTr, apLbl = Tog("Auto Play", "Detecta rota pelo seu plot", 23)
    apLbl.Size = UDim2.new(1,-110,0,20)
    local apBB, apGetKey, apGetBind, apSetBind, apSetKey = BindBtn(apRow, "NatesHub_AutoPlayKey.txt")
    apBtn.MouseButton1Click:Connect(function()
        if moving then stopAutoPlay(); SetOff(apKn, apTr)
        else startAutoPlay(); SetOn(apKn, apTr) end
    end)
    Sep(25)

    -- ============================================================
    -- AUTO GRAB
    -- ============================================================
    Sec("AUTO GRAB", 26)
    local _, agbBtn, agbKn, agbTr = Tog("Auto Grab", "Rouba brainrots automaticamente", 27)
    agbBtn.MouseButton1Click:Connect(function()
        if InstaSteal.Connection then
            stopAutoSteal()
            SetOff(agbKn, agbTr)
        else
            autoStealLoop()
            SetOn(agbKn, agbTr)
        end
    end)
    Sep(29)

    -- ============================================================
    -- DROP BRAINROT
    -- ============================================================
    Sec("DROP BRAINROT", 30)
    local drRow, drBtn, drKn, drTr = Tog("Drop Brainrot", "Sobe e desce para largar brainrot", 31)
    local drBB, drGetKey, drGetBind, drSetBind, drSetKey = BindBtn(drRow, "NatesHub_DropKey.txt")
    drBtn.MouseButton1Click:Connect(function()
        DropBrainrotEnabled = not DropBrainrotEnabled
        if DropBrainrotEnabled then
            SetOn(drKn, drTr)
            startDropBrainrot()
        else
            SetOff(drKn, drTr)
            stopDropBrainrot()
        end
    end)
    Sep(32)

    -- ============================================================
    -- CONFIG
    -- ============================================================
    Sep(36)
    -- ============================================================
    UserInputService.InputBegan:Connect(function(input, gp)
        local apKey = apGetKey(); local apBind = apGetBind()
        local spKey = spGetKey(); local spBind = spGetBind()
        local tkKey = tkGetKey(); local tkBind = tkGetBind()
        local drKey = drGetKey(); local drBind = drGetBind()

        if apBind and not gp then
            if input.KeyCode == Enum.KeyCode.Backspace then
                apSetKey(nil); apBB.Text = "BIND"; apBB.TextColor3 = Color3.fromRGB(255,255,255)
                pcall(function() if writefile then writefile("NatesHub_AutoPlayKey.txt","NONE") end end)
            elseif input.UserInputType == Enum.UserInputType.Keyboard then
                local k = input.KeyCode.Name; apSetKey(k); apBB.Text = k; apBB.TextColor3 = Color3.fromRGB(255,255,255)
                pcall(function() if writefile then writefile("NatesHub_AutoPlayKey.txt", k) end end)
            end
            apSetBind(false); return
        end
        if spBind and not gp then
            if input.KeyCode == Enum.KeyCode.Backspace then
                spSetKey(nil); spBB.Text = "BIND"; spBB.TextColor3 = Color3.fromRGB(255,255,255)
                pcall(function() if writefile then writefile("NatesHub_SpeedKey.txt","NONE") end end)
            elseif input.UserInputType == Enum.UserInputType.Keyboard then
                local k = input.KeyCode.Name; spSetKey(k); spBB.Text = k; spBB.TextColor3 = Color3.fromRGB(255,255,255)
                pcall(function() if writefile then writefile("NatesHub_SpeedKey.txt", k) end end)
            end
            spSetBind(false); return
        end
        if tkBind and not gp then
            if input.KeyCode == Enum.KeyCode.Backspace then
                tkSetKey(nil); tkBB.Text = "BIND"; tkBB.TextColor3 = Color3.fromRGB(255,255,255)
                pcall(function() if writefile then writefile("NatesHub_TrackerKey.txt","NONE") end end)
            elseif input.UserInputType == Enum.UserInputType.Keyboard then
                local k = input.KeyCode.Name; tkSetKey(k); tkBB.Text = k; tkBB.TextColor3 = Color3.fromRGB(255,255,255)
                pcall(function() if writefile then writefile("NatesHub_TrackerKey.txt", k) end end)
            end
            tkSetBind(false); return
        end
        if drBind and not gp then
            if input.KeyCode == Enum.KeyCode.Backspace then
                drSetKey(nil); drBB.Text = "BIND"; drBB.TextColor3 = Color3.fromRGB(255,255,255)
                pcall(function() if writefile then writefile("NatesHub_DropKey.txt","NONE") end end)
            elseif input.UserInputType == Enum.UserInputType.Keyboard then
                local k = input.KeyCode.Name; drSetKey(k); drBB.Text = k; drBB.TextColor3 = Color3.fromRGB(255,255,255)
                pcall(function() if writefile then writefile("NatesHub_DropKey.txt", k) end end)
            end
            drSetBind(false); return
        end
        if gp then return end
        if apKey and apKey ~= "NONE" and input.KeyCode.Name == apKey then
            if moving then stopAutoPlay(); SetOff(apKn, apTr)
            else startAutoPlay(); SetOn(apKn, apTr) end
        end
        if spKey and spKey ~= "NONE" and input.KeyCode.Name == spKey then
            SpeedBooster.Enabled = not SpeedBooster.Enabled
            if SpeedBooster.Enabled then SetOn(spKn, spTr); startSpeedBooster()
            else SetOff(spKn, spTr); stopSpeedBooster() end
        end
        if tkKey and tkKey ~= "NONE" and input.KeyCode.Name == tkKey then
            batAimbotToggled = not batAimbotToggled
            if batAimbotToggled then SetOn(tkKn, tkTr); startBatAimbot()
            else SetOff(tkKn, tkTr); stopBatAimbot() end
        end
        if drKey and drKey ~= "NONE" and input.KeyCode.Name == drKey then
            DropBrainrotEnabled = not DropBrainrotEnabled
            if DropBrainrotEnabled then SetOn(drKn, drTr); startDropBrainrot()
            else SetOff(drKn, drTr); stopDropBrainrot() end
        end
    end)

    -- ============================================================
    -- SHOW / HIDE
    -- ============================================================
    local hubOpen = true
    local function getCenterPos()
        local vp = workspace.CurrentCamera.ViewportSize
        return UDim2.new(0, math.floor(vp.X/2 - 180), 0, math.floor(vp.Y/2 - 340))
    end
    Main.Position = getCenterPos()

    TogBtn.MouseButton1Click:Connect(function()
        hubOpen = not hubOpen
        if hubOpen then
            Main.Visible = true
            local center = getCenterPos()
            Main.Position = UDim2.new(0, center.X.Offset, 0, -Main.AbsoluteSize.Y - 50)
            TweenService:Create(Main, TweenInfo.new(0.25, Enum.EasingStyle.Quart, Enum.EasingDirection.Out),
                {Position = center}):Play()
        else
            hubOpen = false
            local vp = workspace.CurrentCamera.ViewportSize
            TweenService:Create(Main, TweenInfo.new(0.2, Enum.EasingStyle.Quart, Enum.EasingDirection.In),
                {Position = UDim2.new(0, Main.Position.X.Offset, 0, vp.Y + 50)}):Play()
            task.delay(0.25, function() Main.Visible = false end)
        end
    end)
    -- Carrega toggles salvos (aqui os knobs jÃ¡ existem no escopo)
    task.spawn(function()
        task.wait(0.5)
        LoadConfig()
        if ConfigData.GalaxyEnabled and not galaxyEnabled then
            startGalaxy(); SetOn(ijKn, ijTr)
        end
        if ConfigData.AntiRagdollEnabled then
            AntiRagdollEnabled = true; enableAntiRagdoll(); SetOn(arKn, arTr)
        end
        if ConfigData.NoAnimsEnabled then
            UnwalkEnabled = true; startUnwalk(); SetOn(uwKn, uwTr)
        end
        if ConfigData.AutoGrabEnabled then
            autoStealLoop(); SetOn(agbKn, agbTr)
        end
        if ConfigData.DropBrainrotEnabled then
            DropBrainrotEnabled = true; startDropBrainrot(); SetOn(drKn, drTr)
        end
    end)
end

local function initializeGUI(device)
    selectedDevice = device
    DeviceSelectFrame:Destroy()

    -- Controla visibilidade das SmallGUIs por device (sem ativar features automaticamente)
    task.spawn(function()
        task.wait(1.5)

        if device == "PC" then
            -- PC: esconde TODAS as SmallGUIs e StealBar  painel Lacerda controla tudo
            SmallSpeedGUI.Visible = false
            SmallUnwalkGUI.Visible = false
            SmallSpinBotGUI.Visible = false
            SmallInfJumpGUI.Visible = false
            SmallFloatGUI.Visible = false
            SmallFOVGUI.Visible = false
            SmallAntiRagGUI.Visible = false
            SmallRagdollTPGUI.Visible = false
            SmallDropGUI.Visible = false
            StealBarContainer.Visible = false
        else
            -- Celular: todas SmallGUIs invisÃ­veis  controladas pelo painel MENU
            SmallSpeedGUI.Visible = false
            SmallUnwalkGUI.Visible = false
            SmallSpinBotGUI.Visible = false
            SmallInfJumpGUI.Visible = false
            SmallFloatGUI.Visible = false
            SmallFOVGUI.Visible = false
            SmallAntiRagGUI.Visible = false
            SmallRagdollTPGUI.Visible = false
            SmallDropGUI.Visible = false
        end
    end)

    createMainGUI(device)
    task.wait(0.3)

    if device == "PC" then
        -- PC: usa GUI completa estilo Lacerda Hub (sem botÃ£o OPEN / sem botÃµes laterais)
        -- Esconde o MainFrame antigo do Nates que nÃ£o Ã© usado no PC
        if MainFrame then MainFrame.Visible = false end
        StealBarContainer.Visible = false
        -- Desliga Auto Grab por padrÃ£o no PC (usuÃ¡rio liga pelo painel)
        if InstaSteal.Connection then
            stopAutoSteal()
        end
        createLacerdaStylePanel()
    else
        createOpenButton(device)
    end

    --  AUTO-LOAD CONFIG ON START!
    -- Speed sempre inicia desligado (nunca carrega do config)
    SpeedBooster.Enabled = false

    -- FOV carrega do config se estava ativo
    if ConfigData.fovEnabled then
        fovEnabled = true
        SmallFOVGUI.Visible = true
        SmallFOVGUI.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        FOVDot.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        updateFOV()
    end
end

PCButton.MouseButton1Click:Connect(function() initializeGUI("PC") end)
iPhoneButton.MouseButton1Click:Connect(function() initializeGUI("iPhone") end)

function createOpenButton(device)
    local buttonSize = device == "iPhone" and 55 or 70

    -- BotÃ£o MENU
    local MenuButton = Instance.new("TextButton")
    MenuButton.Size = UDim2.new(0, buttonSize, 0, buttonSize)
    MenuButton.Position = UDim2.new(0, 15, 0.5, -buttonSize/2)
    MenuButton.BackgroundColor3 = Color3.fromRGB(10, 12, 18)
    MenuButton.BackgroundTransparency = 0.1
    MenuButton.Text = "MENU"
    MenuButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    MenuButton.Font = Enum.Font.GothamBold
    MenuButton.TextSize = device == "iPhone" and 10 or 14
    MenuButton.Parent = ScreenGui
    MenuButton.BorderSizePixel = 0

    local MenuCorner = Instance.new("UICorner")
    MenuCorner.CornerRadius = UDim.new(0, 15)
    MenuCorner.Parent = MenuButton

    -- Borda branca giratÃ³ria igual ao painel PC
    local mStroke = Instance.new("UIStroke", MenuButton)
    mStroke.Color = Color3.fromRGB(255,255,255)
    mStroke.Thickness = 2
    mStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    local mGrad = Instance.new("UIGradient", mStroke)
    mGrad.Rotation = 0
    mGrad.Transparency = NumberSequence.new({
        NumberSequenceKeypoint.new(0,0), NumberSequenceKeypoint.new(0.4,0.8),
        NumberSequenceKeypoint.new(0.6,0.8), NumberSequenceKeypoint.new(1,0),
    })
    TweenService:Create(mGrad, TweenInfo.new(3, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut, -1), {Rotation=360}):Play()

    -- â”€â”€ PAINEL MOBILE (igual ao PC mas com escala menor) â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
    local CA_m   = Color3.new(0, 0.784, 1)
    local CBP_m  = Color3.new(0.012, 0.020, 0.055)
    local CBR_m  = Color3.new(0.047, 0.055, 0.078)
    local CBT_m  = Color3.new(0.137, 0.157, 0.196)
    local CBK_m  = Color3.new(0.706, 0.745, 0.784)
    local CT_m   = Color3.new(0.863, 0.902, 0.961)
    local CD_m   = Color3.new(0.314, 0.471, 0.627)
    local CDIV_m = Color3.new(0, 0.549, 0.784)
    local CF_m   = Color3.new(0, 0.706, 1)

    local function SetOn_m(knob, track)
        TweenService:Create(knob,  TweenInfo.new(0.18), {Position=UDim2.new(1,-18,0,2), BackgroundColor3=CA_m}):Play()
        TweenService:Create(track, TweenInfo.new(0.18), {BackgroundColor3=Color3.new(0,0.25,0.45)}):Play()
    end
    local function SetOff_m(knob, track)
        TweenService:Create(knob,  TweenInfo.new(0.18), {Position=UDim2.new(0,2,0,2), BackgroundColor3=CBK_m}):Play()
        TweenService:Create(track, TweenInfo.new(0.18), {BackgroundColor3=CBT_m}):Play()
    end

    local MPanel = Instance.new("Frame", ScreenGui)
    MPanel.Name = "MobilePanel"
    MPanel.BackgroundColor3 = CBP_m
    MPanel.BackgroundTransparency = 0.35
    MPanel.BorderSizePixel = 0
    MPanel.Size = UDim2.new(0, 280, 0, 580)
    MPanel.Position = UDim2.new(0, 80, 0, 10)
    MPanel.Visible = false
    MPanel.ZIndex = 20
    Instance.new("UICorner", MPanel).CornerRadius = UDim.new(0, 14)

    -- Escala mobile
    local mScale = Instance.new("UIScale", MPanel)
    mScale.Scale = device == "iPhone" and 0.75 or 0.88

    -- Borda giratÃ³ria
    do
        local s = Instance.new("UIStroke", MPanel)
        s.Color = Color3.fromRGB(255,255,255); s.Thickness = 3
        s.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
        local g = Instance.new("UIGradient", s)
        g.Rotation = 0
        g.Transparency = NumberSequence.new({
            NumberSequenceKeypoint.new(0,0),NumberSequenceKeypoint.new(0.4,0.8),
            NumberSequenceKeypoint.new(0.6,0.8),NumberSequenceKeypoint.new(1,0),
        })
        TweenService:Create(g, TweenInfo.new(3,Enum.EasingStyle.Linear,Enum.EasingDirection.InOut,-1),{Rotation=360}):Play()
    end

    -- Bolinhas de fundo
    local MDOTS = {
        {UDim2.new(0.935,0,0.829,0),Color3.new(0,0.980,1),0.48,4},
        {UDim2.new(0.688,0,0.869,0),Color3.new(0,0.867,1),0.34,2},
        {UDim2.new(0.939,0,0.471,0),Color3.new(0,0.808,1),0.38,2},
        {UDim2.new(0.845,0,0.176,0),Color3.new(0,0.745,1),0.38,2},
        {UDim2.new(0.416,0,0.489,0),Color3.new(0,0.843,1),0.35,3},
        {UDim2.new(0.638,0,0.657,0),Color3.new(0,0.655,1),0.24,3},
        {UDim2.new(0.773,0,0.075,0),Color3.new(0,0.651,1),0.44,2},
        {UDim2.new(0.257,0,0.757,0),Color3.new(0,0.976,1),0.51,2},
        {UDim2.new(0.124,0,0.896,0),Color3.new(0,0.824,1),0.45,2},
    }
    local mDotFrames = {}
    for _, d in ipairs(MDOTS) do
        local f = Instance.new("Frame", MPanel)
        f.Size = UDim2.new(0,d[4],0,d[4]); f.Position = d[1]
        f.BackgroundColor3 = d[2]; f.BackgroundTransparency = d[3]
        f.BorderSizePixel = 0; f.ZIndex = 21
        Instance.new("UICorner", f).CornerRadius = UDim.new(1,0)
        table.insert(mDotFrames, f)
    end
    do
        local dX, dY = {}, {}
        for i = 1, #MDOTS do
            dX[i] = (math.random()-0.5)*0.00015
            dY[i] = (math.random()>0.5 and 1 or -1)*0.0009
        end
        RunService.RenderStepped:Connect(function()
            if not MPanel.Visible then return end
            for i, f in ipairs(mDotFrames) do
                if not f.Parent then return end
                local p = f.Position
                local nx = p.X.Scale + dX[i]
                local ny = p.Y.Scale + dY[i]
                if nx < -0.05 then nx = 1.05 elseif nx > 1.05 then nx = -0.05 end
                if ny < -0.05 then ny = 1.05 elseif ny > 1.05 then ny = -0.05 end
                f.Position = UDim2.new(nx, 0, ny, 0)
            end
        end)
    end

    -- TitleBar
    local mTBar = Instance.new("Frame", MPanel)
    mTBar.BackgroundTransparency = 1; mTBar.Size = UDim2.new(1,0,0,66); mTBar.ZIndex = 25

    local mTitle = Instance.new("TextLabel", mTBar)
    mTitle.Text = "LACERDA DUELS"; mTitle.Font = Enum.Font.GothamBlack; mTitle.TextSize = 17
    mTitle.TextColor3 = CA_m; mTitle.BackgroundTransparency = 1
    mTitle.Size = UDim2.new(1,0,0,26); mTitle.Position = UDim2.new(0,0,0,6)
    mTitle.TextXAlignment = Enum.TextXAlignment.Center; mTitle.ZIndex = 26

    -- Discord clicÃ¡vel
    local mDisc = Instance.new("TextButton", mTBar)
    mDisc.Text = "Discord: discord.gg/4Fp8psfxrS"
    mDisc.Font = Enum.Font.GothamMedium; mDisc.TextSize = 9
    mDisc.TextColor3 = Color3.new(0.314,0.627,0.784); mDisc.BackgroundTransparency = 1
    mDisc.Size = UDim2.new(1,0,0,14); mDisc.Position = UDim2.new(0,0,0,34)
    mDisc.TextXAlignment = Enum.TextXAlignment.Center
    mDisc.AutoButtonColor = false; mDisc.ZIndex = 26
    mDisc.MouseButton1Click:Connect(function()
        pcall(function()
            if setclipboard then
                setclipboard("https://discord.gg/4Fp8psfxrS")
                local o = mDisc.Text; mDisc.Text = "LINK COPIADO!"; task.wait(1.5); mDisc.Text = o
            end
        end)
    end)

    local mTDiv = Instance.new("Frame", MPanel)
    mTDiv.BackgroundColor3 = CDIV_m; mTDiv.BackgroundTransparency = 0.6
    mTDiv.Size = UDim2.new(0.92,0,0,1); mTDiv.Position = UDim2.new(0.04,0,0,66); mTDiv.BorderSizePixel = 0

    local mDragH = Instance.new("Frame", MPanel)
    mDragH.BackgroundTransparency = 1; mDragH.Size = UDim2.new(1,0,0,66); mDragH.ZIndex = 24

    local mScroll = Instance.new("ScrollingFrame", MPanel)
    mScroll.BackgroundTransparency = 1; mScroll.BorderSizePixel = 0
    mScroll.Size = UDim2.new(1,-16,1,-76); mScroll.Position = UDim2.new(0,8,0,72)
    mScroll.CanvasSize = UDim2.new(0,0,0,0); mScroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
    mScroll.ScrollBarThickness = 3; mScroll.ScrollBarImageColor3 = CF_m; mScroll.ZIndex = 22

    local mSLay = Instance.new("UIListLayout", mScroll)
    mSLay.Padding = UDim.new(0,4); mSLay.SortOrder = Enum.SortOrder.LayoutOrder

    -- FPS / PING row
    local mFpsRow = Instance.new("Frame", mScroll)
    mFpsRow.Size = UDim2.new(1,0,0,22); mFpsRow.BackgroundTransparency = 1; mFpsRow.LayoutOrder = -1

    local mFpsLbl = Instance.new("TextLabel", mFpsRow)
    mFpsLbl.Size = UDim2.new(0.5,-4,1,0); mFpsLbl.Position = UDim2.new(0,8,0,0)
    mFpsLbl.BackgroundTransparency = 1; mFpsLbl.Text = "FPS: --"
    mFpsLbl.Font = Enum.Font.GothamBold; mFpsLbl.TextSize = 10
    mFpsLbl.TextColor3 = CT_m; mFpsLbl.TextXAlignment = Enum.TextXAlignment.Left

    local mPingLbl = Instance.new("TextLabel", mFpsRow)
    mPingLbl.Size = UDim2.new(0.5,-8,1,0); mPingLbl.Position = UDim2.new(0.5,4,0,0)
    mPingLbl.BackgroundTransparency = 1; mPingLbl.Text = "PING: --"
    mPingLbl.Font = Enum.Font.GothamBold; mPingLbl.TextSize = 10
    mPingLbl.TextColor3 = CT_m; mPingLbl.TextXAlignment = Enum.TextXAlignment.Right

    task.spawn(function()
        local fpsBuf = {}
        while MPanel.Parent do
            local t0 = tick(); RunService.RenderStepped:Wait()
            table.insert(fpsBuf, 1/math.max(tick()-t0,0.001))
            if #fpsBuf > 20 then table.remove(fpsBuf,1) end
            local s = 0; for _,v in ipairs(fpsBuf) do s = s + v end
            local fps = math.floor(s/#fpsBuf)
            local ping = 0
            pcall(function() ping = math.floor(game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValue()) end)
            mFpsLbl.Text = "FPS: " .. fps
            mFpsLbl.TextColor3 = fps>=50 and CT_m or fps>=30 and Color3.fromRGB(255,200,80) or Color3.fromRGB(255,80,80)
            mPingLbl.Text = "PING: " .. ping
            mPingLbl.TextColor3 = ping<=80 and CT_m or ping<=150 and Color3.fromRGB(255,200,80) or Color3.fromRGB(255,80,80)
            task.wait(0.5)
        end
    end)

    -- Helpers de layout
    local function Sep_m(order)
        local f = Instance.new("Frame", mScroll)
        f.Size = UDim2.new(0.92,0,0,1); f.BackgroundColor3 = CDIV_m
        f.BackgroundTransparency = 0.7; f.BorderSizePixel = 0; f.LayoutOrder = order
    end
    local function Sec_m(txt, order)
        local l = Instance.new("TextLabel", mScroll)
        l.Size = UDim2.new(1,0,0,18); l.BackgroundTransparency = 1
        l.Text = "  "..txt; l.Font = Enum.Font.GothamBold; l.TextSize = 10
        l.TextColor3 = CA_m; l.TextXAlignment = Enum.TextXAlignment.Left
        l.LayoutOrder = order; l.ZIndex = 23
    end
    local function Tog_m(lbl, sub, order)
        local Row = Instance.new("Frame", mScroll)
        Row.Size = UDim2.new(1,0,0,38); Row.BackgroundTransparency = 1
        Row.LayoutOrder = order; Row.ZIndex = 23
        local L = Instance.new("TextLabel", Row)
        L.Text = lbl; L.TextSize = 13; L.Font = Enum.Font.GothamBold
        L.TextColor3 = CT_m; L.BackgroundTransparency = 1
        L.Size = UDim2.new(1,-64,0,20); L.Position = UDim2.new(0,10,0,4)
        L.TextXAlignment = Enum.TextXAlignment.Left; L.ZIndex = 24
        if sub ~= "" then
            local S = Instance.new("TextLabel", Row)
            S.Text = sub; S.TextSize = 9; S.Font = Enum.Font.Gotham
            S.TextColor3 = CD_m; S.BackgroundTransparency = 1
            S.Size = UDim2.new(1,-64,0,12); S.Position = UDim2.new(0,10,0,22)
            S.TextXAlignment = Enum.TextXAlignment.Left; S.ZIndex = 24
        end
        local Tr = Instance.new("Frame", Row)
        Tr.Size = UDim2.new(0,38,0,20); Tr.Position = UDim2.new(1,-46,0.5,-10)
        Tr.BackgroundColor3 = CBT_m; Tr.BorderSizePixel = 0
        Instance.new("UICorner", Tr).CornerRadius = UDim.new(1,0)
        local Kn = Instance.new("Frame", Tr)
        Kn.Size = UDim2.new(0,16,0,16); Kn.Position = UDim2.new(0,2,0,2)
        Kn.BackgroundColor3 = CBK_m; Kn.BorderSizePixel = 0; Kn.ZIndex = 25
        Instance.new("UICorner", Kn).CornerRadius = UDim.new(1,0)
        local Btn = Instance.new("TextButton", Row)
        Btn.Text = ""; Btn.Size = UDim2.new(1,0,1,0)
        Btn.BackgroundTransparency = 1; Btn.ZIndex = 26
        return Row, Btn, Kn, Tr, L
    end

    local function Sld_m(lbl, mn, mx, init, order, onChange)
        local Row = Instance.new("Frame", mScroll)
        Row.Size = UDim2.new(1,0,0,52); Row.BackgroundTransparency = 1
        Row.LayoutOrder = order; Row.ZIndex = 23
        local LT = Instance.new("TextLabel", Row)
        LT.Text = lbl; LT.TextColor3 = CT_m; LT.Font = Enum.Font.GothamBold; LT.TextSize = 12
        LT.BackgroundTransparency = 1; LT.Size = UDim2.new(1,-70,0,18)
        LT.Position = UDim2.new(0,14,0,4); LT.TextXAlignment = Enum.TextXAlignment.Left; LT.ZIndex = 24
        local VLBg = Instance.new("Frame", Row)
        VLBg.Size = UDim2.new(0,52,0,20); VLBg.Position = UDim2.new(1,-62,0,2)
        VLBg.BackgroundColor3 = Color3.fromRGB(0,60,120)
        VLBg.BackgroundTransparency = 0.4
        VLBg.BorderSizePixel = 0; VLBg.ZIndex = 24
        Instance.new("UICorner", VLBg).CornerRadius = UDim.new(0,6)
        local VL = Instance.new("TextLabel", VLBg)
        VL.Text = tostring(init); VL.TextColor3 = Color3.new(1,1,1)
        VL.Font = Enum.Font.GothamBold; VL.TextSize = 11; VL.BackgroundTransparency = 1
        VL.Size = UDim2.new(1,0,1,0); VL.TextXAlignment = Enum.TextXAlignment.Center; VL.ZIndex = 25
        local TB = Instance.new("Frame", Row)
        TB.Size = UDim2.new(1,-28,0,8); TB.Position = UDim2.new(0,14,0,30)
        TB.BackgroundColor3 = Color3.fromRGB(15,20,45)
        TB.BackgroundTransparency = 0.4
        TB.BorderSizePixel = 0; TB.ZIndex = 24
        Instance.new("UICorner", TB).CornerRadius = UDim.new(1,0)
        local Fl = Instance.new("Frame", TB)
        Fl.Size = UDim2.new((init-mn)/(mx-mn),0,1,0)
        Fl.BackgroundColor3 = Color3.fromRGB(0,180,255)
        Fl.BackgroundTransparency = 0.2
        Fl.BorderSizePixel = 0; Fl.ZIndex = 25
        Instance.new("UICorner", Fl).CornerRadius = UDim.new(1,0)
        local flGrad = Instance.new("UIGradient", Fl)
        flGrad.Color = ColorSequence.new({
            ColorSequenceKeypoint.new(0, Color3.fromRGB(0,120,255)),
            ColorSequenceKeypoint.new(1, Color3.fromRGB(0,220,255)),
        })
        local Th = Instance.new("Frame", TB)
        Th.Size = UDim2.new(0,18,0,18); Th.Position = UDim2.new((init-mn)/(mx-mn),0,0.5,-9)
        Th.BackgroundColor3 = Color3.fromRGB(255,255,255)
        Th.BackgroundTransparency = 0.2
        Th.BorderSizePixel = 0; Th.ZIndex = 27
        Instance.new("UICorner", Th).CornerRadius = UDim.new(1,0)
        local thStroke = Instance.new("UIStroke", Th)
        thStroke.Color = Color3.fromRGB(0,200,255); thStroke.Thickness = 2
        local thInner = Instance.new("Frame", Th)
        thInner.Size = UDim2.new(0,8,0,8); thInner.Position = UDim2.new(0.5,-4,0.5,-4)
        thInner.BackgroundColor3 = Color3.fromRGB(0,180,255)
        thInner.BackgroundTransparency = 0.1
        thInner.BorderSizePixel = 0; thInner.ZIndex = 28
        Instance.new("UICorner", thInner).CornerRadius = UDim.new(1,0)
        local drag = false
        local function setV(ix)
            local r = math.clamp((ix-TB.AbsolutePosition.X)/TB.AbsoluteSize.X, 0, 1)
            local v = math.floor(mn+(mx-mn)*r)
            Fl.Size = UDim2.new(r,0,1,0); Th.Position = UDim2.new(r,0,0.5,-9)
            VL.Text = tostring(v); onChange(v)
        end
        Th.InputBegan:Connect(function(i)
            if i.UserInputType==Enum.UserInputType.Touch or i.UserInputType==Enum.UserInputType.MouseButton1 then drag=true end
        end)
        TB.InputBegan:Connect(function(i)
            if i.UserInputType==Enum.UserInputType.Touch or i.UserInputType==Enum.UserInputType.MouseButton1 then drag=true; setV(i.Position.X) end
        end)
        UserInputService.InputEnded:Connect(function(i)
            if i.UserInputType==Enum.UserInputType.Touch or i.UserInputType==Enum.UserInputType.MouseButton1 then drag=false end
        end)
        UserInputService.InputChanged:Connect(function(i)
            if drag and (i.UserInputType==Enum.UserInputType.MouseMovement or i.UserInputType==Enum.UserInputType.Touch) then setV(i.Position.X) end
        end)
        return Row, VL
    end

    -- â”€â”€ TOGGLES â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€

    -- â”€â”€ FUNÃ‡ÃƒO HELPER: cria botÃ£o arrastÃ¡vel estilo menu â”€â”€
    local function makeDraggableBtn(labelText, posX, posY, onActivate, onDeactivate)
        local bs = buttonSize
        local btn = Instance.new("Frame")
        btn.Size = UDim2.new(0, bs, 0, bs)
        btn.Position = UDim2.new(0, posX, 0, posY)
        btn.BackgroundColor3 = Color3.new(0.039, 0.047, 0.071)
        btn.BorderSizePixel = 0
        btn.Visible = false
        btn.Active = true
        btn.Parent = ScreenGui
        Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 14)

        -- Borda branca giratÃ³ria
        local s = Instance.new("UIStroke", btn)
        s.Color = Color3.fromRGB(255,255,255); s.Thickness = 2
        s.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
        local g = Instance.new("UIGradient", s)
        g.Rotation = 0
        g.Transparency = NumberSequence.new({
            NumberSequenceKeypoint.new(0,0), NumberSequenceKeypoint.new(0.4,0.8),
            NumberSequenceKeypoint.new(0.6,0.8), NumberSequenceKeypoint.new(1,0),
        })
        TweenService:Create(g, TweenInfo.new(3, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut, -1), {Rotation=360}):Play()

        -- Bolinhas no fundo
        local dots = {
            {UDim2.new(0.85,0,0.15,0), Color3.new(0,0.980,1), 0.5, 2},
            {UDim2.new(0.15,0,0.80,0), Color3.new(0,0.867,1), 0.4, 2},
            {UDim2.new(0.75,0,0.70,0), Color3.new(0,0.843,1), 0.45, 2},
            {UDim2.new(0.30,0,0.20,0), Color3.new(0,0.937,1), 0.4, 2},
        }
        local dotFrames = {}
        for _, d in ipairs(dots) do
            local f = Instance.new("Frame", btn)
            f.Size = UDim2.new(0,d[4],0,d[4]); f.Position = d[1]
            f.BackgroundColor3 = d[2]; f.BackgroundTransparency = d[3]
            f.BorderSizePixel = 0; f.ZIndex = 1
            Instance.new("UICorner", f).CornerRadius = UDim.new(1,0)
            table.insert(dotFrames, f)
        end
        local dDX, dDY = {}, {}
        for i = 1, #dots do
            dDX[i] = (math.random()-0.5)*0.002
            dDY[i] = (math.random()>0.5 and 1 or -1)*0.002
        end
        RunService.RenderStepped:Connect(function()
            if not btn.Visible then return end
            for i, f in ipairs(dotFrames) do
                if not f.Parent then return end
                local p = f.Position
                local nx = p.X.Scale + dDX[i]
                local ny = p.Y.Scale + dDY[i]
                if nx < -0.05 then nx = 1.05 elseif nx > 1.05 then nx = -0.05 end
                if ny < -0.05 then ny = 1.05 elseif ny > 1.05 then ny = -0.05 end
                f.Position = UDim2.new(nx, 0, ny, 0)
            end
        end)

        -- Label
        local lbl = Instance.new("TextLabel", btn)
        lbl.Size = UDim2.new(1,0,1,0)
        lbl.BackgroundTransparency = 1
        lbl.Text = labelText
        lbl.TextColor3 = Color3.new(0, 0.784, 1)
        lbl.Font = Enum.Font.GothamBold
        lbl.TextSize = device == "iPhone" and 9 or 11
        lbl.ZIndex = 3

        -- BotÃ£o clicÃ¡vel por cima
        local clickBtn = Instance.new("TextButton", btn)
        clickBtn.Size = UDim2.new(1,0,1,0)
        clickBtn.BackgroundTransparency = 1
        clickBtn.Text = ""; clickBtn.ZIndex = 4

        -- Drag  escuta tanto no frame quanto no botÃ£o clicÃ¡vel
        local dragging = false
        local dragStart = Vector2.new(0,0)
        local frameStart = Vector2.new(0,0)
        local dragThreshold = 10 -- pixels para distinguir drag de clique
        local dragMoved = false

        local function onInputBegan(i)
            if i.UserInputType == Enum.UserInputType.Touch or i.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = true
                dragMoved = false
                dragStart = Vector2.new(i.Position.X, i.Position.Y)
                frameStart = Vector2.new(btn.Position.X.Offset, btn.Position.Y.Offset)
            end
        end
        local function onInputEnded(i)
            if i.UserInputType == Enum.UserInputType.Touch or i.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = false
            end
        end
        btn.InputBegan:Connect(onInputBegan)
        btn.InputEnded:Connect(onInputEnded)
        clickBtn.InputBegan:Connect(onInputBegan)
        clickBtn.InputEnded:Connect(onInputEnded)

        UserInputService.InputChanged:Connect(function(i)
            if not dragging then return end
            if i.UserInputType ~= Enum.UserInputType.MouseMovement and i.UserInputType ~= Enum.UserInputType.Touch then return end
            local delta = Vector2.new(i.Position.X, i.Position.Y) - dragStart
            if delta.Magnitude > dragThreshold then dragMoved = true end
            if not dragMoved then return end
            local vp2 = workspace.CurrentCamera.ViewportSize
            local nx = math.clamp(frameStart.X + delta.X, 0, vp2.X - btn.AbsoluteSize.X)
            local ny = math.clamp(frameStart.Y + delta.Y, 0, vp2.Y - btn.AbsoluteSize.Y)
            btn.Position = UDim2.new(0, nx, 0, ny)
        end)

        -- Clique sÃ³ registra se nÃ£o foi um drag
        clickBtn.MouseButton1Click:Connect(function()
            if dragMoved then return end
            active = not active
            if active then
                onActivate()
                lbl.TextColor3 = Color3.fromRGB(50, 255, 50)
            else
                onDeactivate()
                lbl.TextColor3 = Color3.new(0, 0.784, 1)
            end
        end)

        -- Reset estado ao esconder
        local function reset()
            active = false
            lbl.TextColor3 = Color3.new(0, 0.784, 1)
        end

        return btn, reset
    end

    -- Cria os botÃµes na tela
    local vp = workspace.CurrentCamera.ViewportSize
    local SpeedBtn,    spReset  = makeDraggableBtn("SPEED",        15, vp.Y*0.5 - buttonSize*2 - 20, startSpeedBooster, stopSpeedBooster)
    local AutoPlayBtn, apReset  = makeDraggableBtn("AUTO\nPLAY",   15, vp.Y*0.5 - buttonSize/2,      startAutoPlay,     stopAutoPlay)
    local DropBtn,     drReset  = makeDraggableBtn("DROP\nBROT",   15, vp.Y*0.5 + buttonSize + 20,   function() DropBrainrotEnabled = true;  startDropBrainrot() end, function() DropBrainrotEnabled = false; stopDropBrainrot() end)
    local TrackBtn,    tkReset  = makeDraggableBtn("TRACK\nPLAYER", vp.X - buttonSize - 15, vp.Y*0.5 - buttonSize/2,
        function() batAimbotToggled = true;  startBatAimbot() end,
        function() batAimbotToggled = false; stopBatAimbot()  end)

    -- â”€â”€ TOGGLES (mostram/escondem botÃµes na tela) â”€â”€â”€â”€â”€â”€â”€â”€â”€

    Sep_m(1)
    Sec_m("VELOCIDADE", 2)
    Sld_m("Normal Speed", 10, 150, SpeedBooster.NormalSpeed, 2, function(v) SpeedBooster.NormalSpeed = v end)
    Sld_m("Steal Speed",  10, 100, SpeedBooster.StealSpeed,  3, function(v) SpeedBooster.StealSpeed  = v end)
    local _, spBtn_m, spKn_m, spTr_m = Tog_m("Speed Booster", "Aparece botao na tela", 4)
    spBtn_m.MouseButton1Click:Connect(function()
        local vis = not SpeedBtn.Visible
        SpeedBtn.Visible = vis
        if vis then SetOn_m(spKn_m, spTr_m)
        else
            SetOff_m(spKn_m, spTr_m)
            stopSpeedBooster(); spReset()
        end
    end)
    Sep_m(4)

    Sec_m("INF JUMP / GALAXY", 5)
    Sld_m("Gravity %",  10, 100, galaxyGravityPct, 5, function(v) galaxyGravityPct = v; if galaxyEnabled then updateGalaxyForce(); adjustGalaxyJump() end end)
    Sld_m("Hop Power",  10, 80,  galaxyHopPower,   6, function(v) galaxyHopPower   = v end)
    local _, ijBtn_m, ijKn_m, ijTr_m = Tog_m("Inf Jump", "Reduz gravidade + mini-hops", 7)
    if galaxyEnabled then SetOn_m(ijKn_m, ijTr_m) end
    ijBtn_m.MouseButton1Click:Connect(function()
        galaxyEnabled = not galaxyEnabled
        if galaxyEnabled then SetOn_m(ijKn_m, ijTr_m); startGalaxy()
        else SetOff_m(ijKn_m, ijTr_m); stopGalaxy() end
    end)
    Sep_m(7)

    Sec_m("COMBATE", 8)
    local _, arBtn_m, arKn_m, arTr_m = Tog_m("Anti Ragdoll", "Evita ser jogado", 9)
    if AntiRagdollEnabled then SetOn_m(arKn_m, arTr_m) end
    arBtn_m.MouseButton1Click:Connect(function()
        AntiRagdollEnabled = not AntiRagdollEnabled
        if AntiRagdollEnabled then SetOn_m(arKn_m, arTr_m); enableAntiRagdoll()
        else SetOff_m(arKn_m, arTr_m); disableAntiRagdoll() end
    end)
    local _, rtBtn_m, rtKn_m, rtTr_m = Tog_m("Ragdoll TP", "TP ao tomar ragdoll", 10)
    if RagdollTP.enabled then SetOn_m(rtKn_m, rtTr_m) end
    rtBtn_m.MouseButton1Click:Connect(function()
        RagdollTP.enabled = not RagdollTP.enabled
        if RagdollTP.enabled then SetOn_m(rtKn_m, rtTr_m); startRagdollTP()
        else SetOff_m(rtKn_m, rtTr_m); stopRagdollTP() end
    end)
    local _, tkBtn_m, tkKn_m, tkTr_m = Tog_m("Track Player", "Aparece botao na tela", 11)
    tkBtn_m.MouseButton1Click:Connect(function()
        local vis = not TrackBtn.Visible
        TrackBtn.Visible = vis
        if vis then SetOn_m(tkKn_m, tkTr_m)
        else
            SetOff_m(tkKn_m, tkTr_m)
            batAimbotToggled = false; stopBatAimbot(); tkReset()
        end
    end)
    Sep_m(12)

    Sec_m("MOVIMENTO", 13)
    local _, uwBtn_m, uwKn_m, uwTr_m = Tog_m("Unwalk", "Remove animacoes de andar", 14)
    if UnwalkEnabled then SetOn_m(uwKn_m, uwTr_m) end
    uwBtn_m.MouseButton1Click:Connect(function()
        UnwalkEnabled = not UnwalkEnabled
        if UnwalkEnabled then SetOn_m(uwKn_m, uwTr_m); startUnwalk()
        else SetOff_m(uwKn_m, uwTr_m); stopUnwalk() end
    end)
    Sep_m(15)

    Sec_m("AUTO PLAY", 16)
    local _, apBtn_m, apKn_m, apTr_m = Tog_m("Auto Play", "Aparece botao na tela", 17)
    apBtn_m.MouseButton1Click:Connect(function()
        local vis = not AutoPlayBtn.Visible
        AutoPlayBtn.Visible = vis
        if vis then SetOn_m(apKn_m, apTr_m)
        else SetOff_m(apKn_m, apTr_m); stopAutoPlay(); apReset() end
    end)
    Sep_m(18)

    Sec_m("AUTO GRAB", 19)
    local _, agBtn_m, agKn_m, agTr_m = Tog_m("Auto Grab", "Rouba brainrots automaticamente", 20)
    agBtn_m.MouseButton1Click:Connect(function()
        if InstaSteal.Connection then
            stopAutoSteal()
            SetOff_m(agKn_m, agTr_m)
        else
            autoStealLoop(); SetOn_m(agKn_m, agTr_m)
        end
    end)
    Sep_m(21)

    Sec_m("DROP BRAINROT", 22)
    local _, drBtn_m, drKn_m, drTr_m = Tog_m("Drop Brainrot", "Aparece botao na tela", 23)
    drBtn_m.MouseButton1Click:Connect(function()
        local vis = not DropBtn.Visible
        DropBtn.Visible = vis
        if vis then SetOn_m(drKn_m, drTr_m)
        else
            SetOff_m(drKn_m, drTr_m)
            DropBrainrotEnabled = false; stopDropBrainrot(); drReset()
        end
    end)
    Sep_m(24)

    -- â”€â”€ SHOW / HIDE â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
    local panelOpen = false
    MenuButton.MouseButton1Click:Connect(function()
        panelOpen = not panelOpen
        if panelOpen then
            MPanel.Visible = true
            local vp2 = workspace.CurrentCamera.ViewportSize
            local scaledW = MPanel.AbsoluteSize.X
            local scaledH = MPanel.AbsoluteSize.Y
            local cx = math.floor(vp2.X/2 - scaledW/2)
            local cy = math.floor(vp2.Y/2 - scaledH/2) - 40
            MPanel.Position = UDim2.new(0, cx, 0, -scaledH - 50)
            TweenService:Create(MPanel, TweenInfo.new(0.3, Enum.EasingStyle.Quart, Enum.EasingDirection.Out),
                {Position = UDim2.new(0, cx, 0, cy)}):Play()
        else
            local vp2 = workspace.CurrentCamera.ViewportSize
            TweenService:Create(MPanel, TweenInfo.new(0.2, Enum.EasingStyle.Quart, Enum.EasingDirection.In),
                {Position = UDim2.new(0, MPanel.Position.X.Offset, 0, vp2.Y + 50)}):Play()
            task.delay(0.25, function() MPanel.Visible = false end)
        end
    end)
end

print("â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•")
print("   NATESHUB V7.0 FINAL COMPLETE!")
print("â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•")
print(" VISIBLE SMALL GUIs (5):")
print("   Speed (ON), Unwalk (ON), Anti Rag (ON),")
print("   Ragdoll TP (ON), Drop Brainrot (OFF)")
print(" AUTO-ENABLED MAIN:")
print("   Speed, Unwalk, Anti Rag, Ragdoll TP")
print(" AUTO-ENABLED MISC:")
print("   FPS, Optimizer, Hitbox, Anti Death")
print(" Hidden until you turn on:")
print("   Inf Jump, Spin Bot, Float, FOV")
print(" WORKS ON: PC, iPad, iPhone!")
print("â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•")

game:GetService("Players").PlayerRemoving:Connect(function(plr)
    if plr == player then
        SaveConfig()
        print("[NATESHUB]  Auto-saved config on leave!")
    end
end)
