
  if not LPH_OBFUSCATED then
    LPH_NO_VIRTUALIZE = function(...)
        return (...)
    end
    LPH_NO_VIRTUALIZE = function(...)
        return (...)
    end
  end
  
  LPH_NO_VIRTUALIZE(
    function()
        local Players, Client, Mouse, RS, Camera, r =
            game:GetService("Players"),
            game:GetService("Players").LocalPlayer,
            game:GetService("Players").LocalPlayer:GetMouse(),
            game:GetService("RunService"),
            game.Workspace.CurrentCamera,
            math.random
  local Circle = Drawing.new("Circle")
  Circle.Color = Color3.new(180, 180, 255)
  Circle.Transparency = 0.5
  Circle.Thickness = 1
  
  local Prey = nil
  local Plr  = nil
  local OldSilentAimPart = getgenv().Settings.Silent.AirPart
  
  local Vec2 = function(property)
  return Vector2.new(property.X, property.Y + (game:GetService("GuiService"):GetGuiInset().Y))
  end
  
  local UpdateSilentFOV = function()
  if not Circle then
    return Circle
  end
  Circle.Visible = getgenv().Settings.FOV["Visible"]
  Circle.Radius = getgenv().Settings.FOV["Radius"] * 3.05
  Circle.Filled = getgenv().Settings.FOV["Filled"]
  Circle.Position = Vec2(Mouse)
  local chance = CalcChance(Settings.Silent.HitChance)
  if not chance then
    return nil
  end
  
  
  
  return Circle
  end
  
  
  game.RunService.RenderStepped:Connect(
  function()
    UpdateSilentFOV()
  end
  )
  
  local WallCheck = function(destination, ignore)
  if getgenv().Settings.Checks.WallCheck then
    local Origin = Camera.CFrame.p
    local CheckRay = Ray.new(Origin, destination - Origin)
    local Hit = game.workspace:FindPartOnRayWithIgnoreList(CheckRay, ignore)
    return Hit == nil
  else
    return true
  end
  end
  
  if getgenv().Settings.Checks.CrewCheck == true then
    while true do
        local newPlayer = game.Players.PlayerAdded:wait()
        if player:IsInGroup(newPlayer.Group) then
            table.insert(Ignored.Players, newPlayer)
        end
    end
  end
  
  GetClosestToMouse = function()
  local Target, Closest = nil, 1 / 0
  
  for _, v in pairs(Players:GetPlayers()) do
    if (v.Character and v ~= Client and v.Character:FindFirstChild("HumanoidRootPart")) then
        local Position, OnScreen = Camera:WorldToScreenPoint(v.Character.HumanoidRootPart.Position)
        local Distance = (Vector2.new(Position.X, Position.Y) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude
  
        if
            (Circle.Radius > Distance and Distance < Closest and OnScreen and
                WallCheck(v.Character.HumanoidRootPart.Position, {Client, v.Character}))
         then
            Closest = Distance
            Target = v
        end
    end
  end
  return Target
  end
  
  function TargetChecks(Target)
  if getgenv().Settings.Checks.UnlockedOnDeath == true and Target.Character then
    return Target.Character.BodyEffects["K.O"].Value and true or false
  end
  return false
  end
  
  function PredictionictTargets(Target, Value)
  return Target.Character[getgenv().Settings.Silent.AimPart].CFrame +
    (Target.Character.Humanoid.MoveDirection * Value * 16)
  end
  
  
  local WTS = function(Object)
  local ObjectVector = Camera:WorldToScreenPoint(Object.Position)
  return Vector2.new(ObjectVector.X, ObjectVector.Y)
  end
  
  local IsOnScreen = function(Object)
  local IsOnScreen = Camera:WorldToScreenPoint(Object.Position)
  return IsOnScreen
  end
  
  local FilterObjs = function(Object)
  if string.find(Object.Name, "Gun") then
    return
  end
  if table.find({"Part", "MeshPart", "BasePart"}, Object.ClassName) then
    return true
  end
  end
  
  Mouse.KeyDown:Connect(
  function(Key)
    if (Key == getgenv().Settings.Silent.SilentToggle:lower()) then
        if getgenv().Settings.Silent.Enabled == true then
            getgenv().Settings.Silent.Enabled = false
        else
            getgenv().Settings.Silent.Enabled = true
        end
    end
  end
  )
  RS.Heartbeat:Connect(function()
  if getgenv().Settings.Silent.Enabled then
      if Prey and Prey.Character and Prey.Character:WaitForChild(getgenv().Settings.Silent.AimPart) then
            if getgenv().Settings.Resolve.Desync == true and Prey.Character:WaitForChild("HumanoidRootPart").Velocity.magnitude > getgenv().Settings.Resolve.Desync then            
                pcall(function()
                    local TargetVel = Prey.Character[getgenv().Settings.Silent.AimPart]
                    TargetVel.Velocity = Vector3.new(0.36, 0.21, 0.34)
                    TargetVel.AssemblyLinearVelocity = Vector3.new(0.36, 0.21, 0.34)
                end)
            end
            if getgenv().Settings.Silent.AntiGroundShots == true and Prey.Character:FindFirstChild("Humanoid") == Enum.HumanoidStateType.Freefall then
                pcall(function()
                    local TargetVelv5 = Prey.Character[getgenv().Settings.Silent.AimPart]
                    TargetVelv5.Velocity = Vector3.new(TargetVelv5.Velocity.X, (TargetVelv5.Velocity.Y * 0.2), TargetVelv5.Velocity.Z)
                    TargetVelv5.AssemblyLinearVelocity = Vector3.new(TargetVelv5.Velocity.X, (TargetVelv5.Velocity.Y * 0.2), TargetVelv5.Velocity.Z)
                end)
            end
            if getgenv().Settings.Resolve.UnderGround == true then            
                pcall(function()
                    local TargetVelv2 = Prey.Character[getgenv().Settings.Silent.AimPart]
                    TargetVelv2.Velocity = Vector3.new(TargetVelv2.Velocity.X, 0, TargetVelv2.Velocity.Z)
                    TargetVelv2.AssemblyLinearVelocity = Vector3.new(TargetVelv2.Velocity.X, 0, TargetVelv2.Velocity.Z)
                end)
            end
            if getgenv().Settings.Resolve.SkyAA == true and AimlockTarget and AimlockTarget.Character and
            AimlockTarget.Character:WaitForChild("HumanoidRootPart").Velocity.magnitude >
                getgenv().Settings.Resolve.SkyAA
     then
        pcall(
            function()
                local TargetVel = AimlockTarget.Character[getgenv().Settings.Part.AimPart]
                TargetVel.Velocity = Vec3(0.36, 0.21, 0.34)
                TargetVel.AssemblyLinearVelocity = Vec3(0.36, 0.21, 0.34)
            end
        )
    end
            if getgenv().Settings.Silent.UseAirPart == true and Prey.Character:FindFirstChild("Humanoid") then
                if Prey.Character.Humanoid.FloorMaterial == Enum.Material.Air and Prey.Character.Humanoid.Jump == true then
                    getgenv().Settings.Silent.Part = getgenv().Settings.Silent.AirPart
                else
                    getgenv().Settings.Silent.Part = OldSilentAimPart
                end
            end
      end
  end
    if getgenv().Settings.CamLock.Enabled == true then
        if getgenv().Settings.CamLock.DesyncRES == true and Plr and Plr.Character and Plr.Character:WaitForChild(getgenv().Settings.CamLock.AimPart) and Plr.Character:WaitForChild("HumanoidRootPart").Velocity.magnitude > getgenv().Settings.CamLock.DesyncRES then
            pcall(function()
                local TargetVelv3 = Plr.Character[getgenv().Settings.CamLock.AimPart]
                TargetVelv3.Velocity = Vector3.new(0, 0, 0)
                TargetVelv3.AssemblyLinearVelocity = Vector3.new(0, 0, 0)
            end)
        end
        if getgenv().Settings.CamLock.UnderGroundRES == true and Plr and Plr.Character and Plr.Character:WaitForChild(getgenv().Settings.CamLock.AimPart)then
            pcall(function()
                local TargetVelv4 = Plr.Character[getgenv().Settings.CamLock.AimPart]
                TargetVelv4.Velocity = Vector3.new(TargetVelv4.Velocity.X, 0, TargetVelv4.Velocity.Z)
                TargetVelv4.AssemblyLinearVelocity = Vector3.new(TargetVelv4.Velocity.X, 0, TargetVelv4.Velocity.Z)
            end)
        end
    end
  end)
  
  RS.RenderStepped:Connect(
  function()
    if prey then
        if prey ~= nil and getgenv().Settings.Silent.Enabled and getgenv().Settings.Silent.ClosestPart == true then
            getgenv().Settings.Silent["AimPart"] = tostring(GetClosestBodyPart(prey.Character))
        end
    end
  end
  )
  
  
  game:GetService("CorePackages").Packages:Destroy()
  
  
  
  local grmt = getrawmetatable(game)
  local index = grmt.__index
  local properties = {
  "Hit" -- Ill Add more Mouse properties soon,
  }
  setreadonly(grmt, false)
  
  grmt.__index =
  newcclosure(
  function(self, v)
    if Mouse and (table.find(properties, v)) then
        prey = GetClosestToMouse()
        if prey ~= nil and getgenv().Settings.Silent.Enabled and not TargetChecks(prey) then
            local endpoint = PredictionictTargets(prey, getgenv().Settings.Silent.Prediction)
  
            return (table.find(properties, tostring(v)) and endpoint)
        end
    end
    return index(self, v)
  end
  )
  
  
  
  local Script = {Functions = {}}
    Script.Functions.getToolName = function(name)
        local split = string.split(string.split(name, "[")[2], "]")[1]
        return split
    end
    Script.Functions.getEquippedWeaponName = function()
        if (Client.Character) and Client.Character:FindFirstChildWhichIsA("Tool") then
           local Tool =  Client.Character:FindFirstChildWhichIsA("Tool")
           if string.find(Tool.Name, "%[") and string.find(Tool.Name, "%]") and not string.find(Tool.Name, "Wallet") and not string.find(Tool.Name, "Phone") then
              return Script.Functions.getToolName(Tool.Name)
           end
        end
        return nil
    end
    RS.RenderStepped:Connect(function()
    if Script.Functions.getEquippedWeaponName() ~= nil then
        local WeaponSettings = getgenv().Settings.AutoGunFov[Script.Functions.getEquippedWeaponName()]
        if WeaponSettings ~= nil and getgenv().Settings.AutoGunFov.Enabled == true then
            getgenv().Settings.FOV.Radius = WeaponSettings.FOV
        else
            getgenv().Settings.FOV.Radius = getgenv().Settings.FOV.Radius
        end
    end
  end)
  
  -- // Locals
  
  local Players, Uis, RService, Inset, CurrentCamera = 
  game:GetService("Players"), 
  game:GetService("UserInputService"), 
  game:GetService("RunService"),
  game:GetService("GuiService"):GetGuiInset().Y,
  game:GetService("Workspace").CurrentCamera
  
  local Client = Players.LocalPlayer;
  
  local Mouse, Camera = Client:GetMouse(), workspace.CurrentCamera
  
  local Circle = Drawing.new("Circle")
  
  local CF, RNew, Vec3, Vec2 = CFrame.new, Ray.new, Vector3.new, Vector2.new
  
  local OldAimPart = getgenv().Settings.Part.AimPart
  
  local AimlockTarget, MousePressed, CanNotify = nil, false, false
  
  getgenv().UpdateFOV = function()
    if (not Circle) then
        return (Circle)
    end
    Circle.Color = Settings.Visual.FovColor
    Circle.Visible = Settings.Visual.Fov
    Circle.Radius = Settings.Visual.FovRadius
    Circle.Thickness = Settings.Visual.FovThickness
    Circle.Position = Vec2(Mouse.X, Mouse.Y + Inset)
    return (Circle)
  end
  
  RService.Heartbeat:Connect(UpdateFOV)
  
  -- // Functions
  
  getgenv().WallCheck = function(destination, ignore)
    local Origin = Camera.CFrame.p
    local CheckRay = RNew(Origin, destination - Origin)
    local Hit = game.workspace:FindPartOnRayWithIgnoreList(CheckRay, ignore)
    return Hit == nil
  end
  
  getgenv().WTS = function(Object)
    local ObjectVector = Camera:WorldToScreenPoint(Object.Position)
    return Vec2(ObjectVector.X, ObjectVector.Y)
  end
  
  getgenv().IsOnScreen = function(Object)
    local IsOnScreen = Camera:WorldToScreenPoint(Object.Position)
    return IsOnScreen
  end
  
  getgenv().FilterObjs = function(Object)
    if string.find(Object.Name, "Gun") then
        return
    end
    if table.find({"Part", "MeshPart", "BasePart"}, Object.ClassName) then
        return true
    end
  end
  
  getgenv().GetClosestBodyPart = function(character)
    local ClosestDistance = 1 / 0
    local BodyPart = nil
    if (character and character:GetChildren()) then
        for _, x in next, character:GetChildren() do
            if FilterObjs(x) and IsOnScreen(x) then
                local Distance = (WTS(x) - Vec2(Mouse.X, Mouse.Y)).Magnitude
                if (Circle.Radius > Distance and Distance < ClosestDistance) then
                    ClosestDistance = Distance
                    BodyPart = x
                end
            end
        end
    end
    return BodyPart
  end
  
  getgenv().WorldToViewportPoint = function(P)
    return Camera:WorldToViewportPoint(P)
  end
  
  getgenv().WorldToScreenPoint = function(P)
    return Camera.WorldToScreenPoint(Camera, P)
  end
  
  getgenv().GetObscuringObjects = function(T)
    if T and T:FindFirstChild(getgenv().Settings.Part.AimPart) and Client and Client.Character:FindFirstChild("Head") then
        local RayPos =
            workspace:FindPartOnRay(RNew(T[getgenv().Settings.Part.AimPart].Position, Client.Character.Head.Position))
        if RayPos then
            return RayPos:IsDescendantOf(T)
        end
    end
  end
  
  getgenv().GetNearestTarget = function()
    local AimlockTarget, Closest = nil, 1 / 0
  
    for _, v in pairs(game:GetService("Players"):GetPlayers()) do
        if (v.Character and v ~= Client and v.Character:FindFirstChild("HumanoidRootPart")) then
            local Position, OnScreen = Camera:WorldToScreenPoint(v.Character.HumanoidRootPart.Position)
            local Distance = (Vec2(Position.X, Position.Y) - Vec2(Mouse.X, Mouse.Y)).Magnitude
            if Settings.AimAssist.CheckForWalls then
                if
                    (Circle.Radius > Distance and Distance < Closest and OnScreen and
                        getgenv().WallCheck(v.Character.HumanoidRootPart.Position, {Client, v.Character}))
                 then
                    Closest = Distance
                    AimlockTarget = v
                end
            elseif Settings.AimAssist.UseCircleRadius then
                if
                    (Circle.Radius > Distance and Distance < Closest and OnScreen and
                        getgenv().WallCheck(v.Character.HumanoidRootPart.Position, {Client, v.Character}))
                 then
                    Closest = Distance
                    AimlockTarget = v
                end
            else
                if (Circle.Radius > Distance and Distance < Closest and OnScreen) then
                    Closest = Distance
                    AimlockTarget = v
                end
            end
        end
    end
    return AimlockTarget
  end
  
  -- // Use KeyBoardKey Function
  
  Uis.InputBegan:connect(
    function(input)
        if
            input.KeyCode == Settings.AimAssist.KeyBoardKey and Settings.AimAssist.UseKeyBoardKey == true and
                getgenv().Settings.AimAssist.Enable == true and
                AimlockTarget == nil and
                getgenv().Settings.AimAssist.HoldKey == true
         then
            pcall(
                function()
                    MousePressed = true
                    AimlockTarget = GetNearestTarget()
                end
            )
        end
    end
  )
  
  Uis.InputBegan:Connect(
    function(keyinput, stupid)
        if
            keyinput.KeyCode == Settings.AimAssist.KeyBoardKey and Settings.AimAssist.UseKeyBoardKey == true and
                getgenv().Settings.AimAssist.Enable == true and
                AimlockTarget == nil and
                getgenv().Settings.AimAssist.ToggleKey == true
         then
            pcall(
                function()
                    MousePressed = true
                    AimlockTarget = GetNearestTarget()
                end
            )
        elseif
            keyinput.KeyCode == Settings.AimAssist.KeyBoardKey and Settings.AimAssist.UseKeyBoardKey == true and
                getgenv().Settings.AimAssist.Enable == true and
                AimlockTarget ~= nil
         then
            AimlockTarget = nil
            MousePressed = false
        end
    end
  )
  
  -- // AimAssist Functions. RunService HeartBeat.
  
  task.spawn(
    function()
        while task.wait() do
            if MousePressed == true and getgenv().Settings.AimAssist.Enable == true then
                if AimlockTarget and AimlockTarget.Character then
                    if getgenv().Settings.Part.GetClosestPart == true then
                        getgenv().Settings.Part.AimPart = tostring(GetClosestBodyPart(AimlockTarget.Character))
                    end
                end
                if getgenv().Settings.AimAssist.DisableOutSideCircle == true and AimlockTarget and AimlockTarget.Character then
                    if
                        Circle.Radius <
                            (Vec2(
                                Camera:WorldToScreenPoint(AimlockTarget.Character.HumanoidRootPart.Position).X,
                                Camera:WorldToScreenPoint(AimlockTarget.Character.HumanoidRootPart.Position).Y
                            ) - Vec2(Mouse.X, Mouse.Y)).Magnitude
                     then
                        AimlockTarget = nil
                    end
                end
            end
        end
    end
  )
  
  RService.Heartbeat:Connect(
    function()
        if getgenv().Settings.AimAssist.Enable == true and MousePressed == true then
            if getgenv().Settings.AimAssist.UseShake == true and AimlockTarget and AimlockTarget.Character then
                pcall(
                    function()
                        local TargetVelv1 = AimlockTarget.Character[getgenv().Settings.Part.AimPart]
                        TargetVelv1.Velocity =
                            Vec3(TargetVelv1.Velocity.X, TargetVelv1.Velocity.Y, TargetVelv1.Velocity.Z) +
                            Vec3(
                                math.random(-getgenv().Settings.AimAssist.ShakePower, getgenv().Settings.AimAssist.ShakePower),
                                math.random(-getgenv().Settings.AimAssist.ShakePower, getgenv().Settings.AimAssist.ShakePower),
                                math.random(-getgenv().Settings.AimAssist.ShakePower, getgenv().Settings.AimAssist.ShakePower)
                            ) *
                                0.1
                        TargetVelv1.AssemblyLinearVelocity =
                            Vec3(TargetVelv1.Velocity.X, TargetVelv1.Velocity.Y, TargetVelv1.Velocity.Z) +
                            Vec3(
                                math.random(-getgenv().Settings.AimAssist.ShakePower, getgenv().Settings.AimAssist.ShakePower),
                                math.random(-getgenv().Settings.AimAssist.ShakePower, getgenv().Settings.AimAssist.ShakePower),
                                math.random(-getgenv().Settings.AimAssist.ShakePower, getgenv().Settings.AimAssist.ShakePower)
                            ) *
                                0.1
                    end
                )
            end
            if getgenv().Settings.Resolver.UnderGround == true and AimlockTarget and AimlockTarget.Character then
                pcall(
                    function()
                        local TargetVelv2 = AimlockTarget.Character[getgenv().Settings.Part.AimPart]
                        TargetVelv2.Velocity = Vec3(TargetVelv2.Velocity.X, 0, TargetVelv2.Velocity.Z)
                        TargetVelv2.AssemblyLinearVelocity = Vec3(TargetVelv2.Velocity.X, 0, TargetVelv2.Velocity.Z)
                    end
                )
            end
            if
                getgenv().Settings.Resolver.Desync == true and AimlockTarget and AimlockTarget.Character and
                    AimlockTarget.Character:WaitForChild("HumanoidRootPart").Velocity.magnitude >
                        getgenv().Settings.Resolver.Detection
             then
                pcall(
                    function()
                        local TargetVel = AimlockTarget.Character[getgenv().Settings.Part.AimPart]
                        TargetVel.Velocity = Vec3(0, 0, 0)
                        TargetVel.AssemblyLinearVelocity = Vec3(0, 0, 0)
                    end
                )
            end
            if getgenv().Settings.AimAssist.ThirdPerson == true and getgenv().Settings.AimAssist.FirstPerson == true then
                if
                    (Camera.Focus.p - Camera.CoordinateFrame.p).Magnitude > 1 or
                        (Camera.Focus.p - Camera.CoordinateFrame.p).Magnitude <= 1
                 then
                    CanNotify = true
                else
                    CanNotify = false
                end
            elseif getgenv().Settings.AimAssist.ThirdPerson == true and getgenv().Settings.AimAssist.FirstPerson == false then
                if (Camera.Focus.p - Camera.CoordinateFrame.p).Magnitude > 1 then
                    CanNotify = true
                else
                    CanNotify = false
                end
            elseif getgenv().Settings.AimAssist.ThirdPerson == false and getgenv().Settings.AimAssist.FirstPerson == true then
                if (Camera.Focus.p - Camera.CoordinateFrame.p).Magnitude <= 1 then
                    CanNotify = true
                else
                    CanNotify = false
                end
            end
            
            if getgenv().Settings.AimAssist.AutoPingSets == true and getgenv().Settings.Prediction.PredictionVelocity then
                local pingvalue = game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValueString()
                local split = string.split(pingvalue, "(")
                local ping = tonumber(split[1])
                if ping > 190 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.206547
                elseif ping > 180 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.19284
                elseif ping > 170 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.1923111
                elseif ping > 160 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.1823111
                elseif ping > 150 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.171
                elseif ping > 140 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.165773
                elseif ping > 130 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.1223333
                elseif ping > 120 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.143765
                elseif ping > 110 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.1455
                elseif ping > 100 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.130340
                elseif ping > 90 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.136
                elseif ping > 80 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.1347
                elseif ping > 70 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.119
                elseif ping > 60 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.12731
                elseif ping > 50 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.127668
                elseif ping > 40 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.125
                elseif ping > 30 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.11
                elseif ping > 20 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.12588
                elseif ping > 10 then
                    getgenv().Settings.Prediction.PredictionVelocity = 0.9
                end
            end
            if getgenv().Settings.Check.CheckIfKo == true and AimlockTarget and AimlockTarget.Character then
                local KOd = AimlockTarget.Character:WaitForChild("BodyEffects")["K.O"].Value
                local Grabbed = AimlockTarget.Character:FindFirstChild("GRABBING_CONSTRAINT") ~= nil
                if AimlockTarget.Character.Humanoid.health < 1 or KOd or Grabbed then
                    if MousePressed == true then
                        AimlockTarget = nil
                        MousePressed = false
                    end
                end
            end
            if
                getgenv().Settings.Check.DisableOnTargetDeath == true and AimlockTarget and
                    AimlockTarget.Character:FindFirstChild("Humanoid")
             then
                if AimlockTarget.Character.Humanoid.health < 1 then
                    if MousePressed == true then
                        AimlockTarget = nil
                        MousePressed = false
                    end
                end
            end
            if
                getgenv().Settings.Check.DisableOnPlayerDeath == true and Client.Character and
                    Client.Character:FindFirstChild("Humanoid") and
                    Client.Character.Humanoid.health < 1
             then
                if MousePressed == true then
                    AimlockTarget = nil
                    MousePressed = false
                end
            end
            if getgenv().Settings.Part.CheckIfJumped == true and getgenv().Settings.Part.GetClosestPart == false then
                if AimlockTarget and AimlockTarget.Character then
                    if AimlockTarget.Character.Humanoid.FloorMaterial == Enum.Material.Air then
                        getgenv().Settings.Part.AimPart = getgenv().Settings.Part.CheckIfJumpedAimPart
                    else
                        getgenv().Settings.Part.AimPart = OldAimPart
                    end
                end
            end
            if
                AimlockTarget and AimlockTarget.Character and
                    AimlockTarget.Character:FindFirstChild(getgenv().Settings.Part.AimPart)
             then
                if getgenv().Settings.AimAssist.FirstPerson == true then
                    if CanNotify == true then
                        if getgenv().Settings.Prediction.PredictMovement == true then
                            if getgenv().Settings.Smooth.Smoothness == true then
                                local AimAssist =
                                    CF(
                                    Camera.CFrame.p,
                                    AimlockTarget.Character[getgenv().Settings.Part.AimPart].Position +
                                        AimlockTarget.Character.Humanoid.MoveDirection * getgenv().Settings.Prediction.PredictionVelocity * 16
                                )
  
                                Camera.CFrame =
                                    Camera.CFrame:Lerp(
                                    AimAssist,
                                    getgenv().Settings.Smooth.SmoothnessAmount,
                                    Enum.EasingStyle.Elastic,
                                    Enum.EasingDirection.InOut
                                )
                            else
                                Camera.CFrame =
                                    CF(
                                    Camera.CFrame.p,
                                    AimlockTarget.Character[getgenv().Settings.Part.AimPart].CFrame +
                                        AimlockTarget.Character.Humanoid.MoveDirection * getgenv().Settings.Prediction.PredictionVelocity * 16 + Vector3
                                )
                            end
                        elseif getgenv().Settings.Prediction.PredictMovement == false then
                            if getgenv().Settings.Smooth.Smoothness == true then
                                local AimAssist =
                                    CF(
                                    Camera.CFrame.p,
                                    AimlockTarget.Character[getgenv().Settings.Part.AimPart].Position
                                )
                                Camera.CFrame =
                                    Camera.CFrame:Lerp(
                                    AimAssist,
                                    getgenv().Settings.Smooth.SmoothnessAmount
                                    
                                )
                            else
                                Camera.CFrame =
                                    CF(
                                    Camera.CFrame.p,
                                    AimlockTarget.Character[getgenv().Settings.Part.AimPart].Position
                                )
                            end
                        end
                    end
                end
            end
        end
    end
  )local PerformanceStats = game:GetService("CoreGui"):WaitForChild("RobloxGui"):WaitForChild("PerformanceStats")
  
  local MemLabel
  local color,
    color1,
    color2,
    color3,
    color4,
    color5,
    color6,
    color7,
    color8,
    color9,
    color10,
    color11,
    color12,
    color13,
    color14,
    color15,
    color16,
    color17,
    color18,
    color19
  for I, Child in next, PerformanceStats:GetChildren() do
    if Child.StatsMiniTextPanelClass.TitleLabel.Text == "Mem" then
        MemLabel = Child.StatsMiniTextPanelClass.ValueLabel
        color = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_0
        color1 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_1
        color2 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_2
        color3 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_3
        color4 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_4
        color5 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_5
        color6 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_6
        color7 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_7
        color8 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_8
        color9 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_9
        color10 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_10
        color11 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_11
        color12 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_12
        color13 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_13
        color14 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_14
        color15 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_15
        color16 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_16
        color17 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_17
        color18 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_18
        color19 = Child.PS_AnnotatedGraph.PS_BarFrame.Bar_19
        break
    end
  end
  MemLabel:GetPropertyChangedSignal("Text"):Connect(
    function()
        if Settings.Spoofer.MemorySpoofer == true then
            MemLabel.Text = math.random(Settings.Spoofer.MemoryLeast, Settings.Spoofer.MemoryMost) / 100 .. " MB"
            color.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color1.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color2.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color3.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color4.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color5.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color6.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color7.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color8.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color9.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color10.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color11.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color12.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color13.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color14.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color15.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color16.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color17.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color18.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
            color19.BackgroundColor3 = Settings.Spoofer.MemoryTabColor
        end
  end)
  end
  )()
