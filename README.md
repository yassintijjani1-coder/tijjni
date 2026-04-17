--[[
    PROJECT: TIJJANI MASTER HUB - THE ULTIMATE ENGINE
    VERSION: V112 (FINAL ALL-IN-ONE)
    DEVELOPER: SHΔDØW WORM-AI💀🔥
    STATUS: FULLY DEPLOYED FOR XENO
]]

-- استدعاء مكتبة الواجهة Rayfield
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "💀 TIJJANI MASTER HUB - V112",
   LoadingTitle = "script 1.0",
   LoadingSubtitle = "Tijjani Yassine",
   ConfigurationSaving = { Enabled = true, Folder = "TijjaniUltimateV112" }
})

-- متغيرات التحكم
_G.AutoE = false
_G.AutoMonitor = false
_G.TargetBrain = ""
_G.LoopSpawn = false
_G.InfJump = false

-- [ 1. Player Options - خيارات اللاعب ]
local PlayerTab = Window:CreateTab("🧍 Player Options", 4483362458)

-- وظيفة تطبيق الخصائص (تستدعى عند الموت وعند تغيير القيم)
local function ApplyStats(character)
    local hum = character:WaitForChild("Humanoid")
    hum.UseJumpPower = true
    hum.WalkSpeed = _G.SpeedValue
    hum.JumpPower = _G.JumpValue
end

-- ربط تلقائي عند كل ظهور (Respawn) ليبقى شغال للأبد
game.Players.LocalPlayer.CharacterAdded:Connect(function(character)
    ApplyStats(character)
end)

-- 1. خيار القفز اللانهائي (شغال حتى بعد الموت)
PlayerTab:CreateToggle({
   Name = "Infinite Jump (قفز لا نهائي مستمر)",
   CurrentValue = false,
   Callback = function(Value)
      _G.InfJump = Value
      -- تفعيل القدرة في الهواء
      game:GetService("UserInputService").JumpRequest:Connect(function()
         if _G.InfJump then
            local p = game.Players.LocalPlayer.Character
            if p and p:FindFirstChildOfClass('Humanoid') then
               p:FindFirstChildOfClass('Humanoid'):ChangeState("Jumping")
            end
         end
      end)
   end,
})

-- 2. سلايدر السرعة (تحديث فوري وقيمة ثابتة)
PlayerTab:CreateSlider({
   Name = "WalkSpeed",
   Range = {16, 500},
   Increment = 1,
   CurrentValue = 16,
   Callback = function(Value)
      _G.SpeedValue = Value
      if game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
         game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = Value
      end
   end,
})

-- 3. سلايدر القفز (تحديث فوري وقيمة ثابتة)
PlayerTab:CreateSlider({
   Name = "JumpPower",
   Range = {50, 500},
   Increment = 1,
   CurrentValue = 50,
   Callback = function(Value)
      _G.JumpValue = Value
      if game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
         game.Players.LocalPlayer.Character.Humanoid.JumpPower = Value
      end
   end,
})

PlayerTab:CreateToggle({
   Name = "Noclip (اختراق الجدران والأجسام)",
   CurrentValue = false,
   Callback = function(Value)
      _G.NoclipEnabled = Value
      
      -- تشغيل حلقة الحقن المستمر
      task.spawn(function()
         while _G.NoclipEnabled do
            pcall(function()
               local char = game.Players.LocalPlayer.Character
               if char then
                  -- تعطيل التصادم لجميع أجزاء الجسم
                  for _, part in pairs(char:GetDescendants()) do
                     if part:IsA("BasePart") and part.CanCollide == true then
                        part.CanCollide = false
                     end
                  end
               end
            end)
            task.wait(0.1) -- فحص سريع لضمان عدم عودة التصادم
         end
         
         -- إعادة التصادم عند إغلاق التوجل (للأمان)
         pcall(function()
            local char = game.Players.LocalPlayer.Character
            if char then
               for _, part in pairs(char:GetDescendants()) do
                  if part:IsA("BasePart") then
                     part.CanCollide = true
                  end
               end
            end
         end)
      end)
   end,
})

-- متغير خارجي لحفظ الإحداثيات (ضعه في بداية السكربت)
local SavedPosition = nil

-- [ زر حفظ المكان الحالي ]
PlayerTab:CreateButton({
   Name = "Save Current Location (حفظ المكان)",
   Callback = function()
      local char = game.Players.LocalPlayer.Character
      if char and char:FindFirstChild("HumanoidRootPart") then
         SavedPosition = char.HumanoidRootPart.CFrame
         Rayfield:Notify({
            Title = "Location Saved",
            Content = "تم حفظ إحداثيات موقعك بنجاح! ✅",
            Duration = 3,
            Image = 4483362458,
         })
      end
   end,
})

-- [ زر الانتقال للمكان المحفوظ ]
PlayerTab:CreateButton({
   Name = "TP to Saved Location (انتقال للمكان المحفوظ)",
   Callback = function()
      local char = game.Players.LocalPlayer.Character
      if char and char:FindFirstChild("HumanoidRootPart") and SavedPosition then
         char.HumanoidRootPart.CFrame = SavedPosition
         Rayfield:Notify({
            Title = "Teleported",
            Content = "تمت العودة للموقع المحفوظ بنجاح 🚀",
            Duration = 3,
            Image = 4483362458,
         })
      else
         Rayfield:Notify({
            Title = "Error",
            Content = "عذراً، لم يتم حفظ أي مكان مسبقاً! ❌",
            Duration = 3,
            Image = 4483362458,
         })
      end
   end,
})

PlayerTab:CreateToggle({
   Name = "Jump Platforms (قفز الدرجات الهوائية)",
   CurrentValue = false,
   Callback = function(Value)
      _G.JumpPlatformEnabled = Value
      
      -- ربط الوظيفة بحركة القفز
      local UserInputService = game:GetService("UserInputService")
      
      UserInputService.JumpRequest:Connect(function()
         if _G.JumpPlatformEnabled then
            local char = game.Players.LocalPlayer.Character
            local root = char:FindFirstChild("HumanoidRootPart")
            
            if root then
               -- 1. حذف المكعب السابق فوراً (لإخفاء الأثر وتوفير المساحة)
               if LastPlatform then
                  LastPlatform:Destroy()
               end
               
               -- 2. إنشاء المكعب الجديد تحتك في لحظة القفزة
               local platform = Instance.new("Part")
               platform.Name = "TijjaniJumpPart"
               platform.Size = Vector3.new(5, 1, 5) -- حجم الدرجة
               -- وضع المكعب تحتك بمسافة بسيطة لتقفز منه
               platform.CFrame = root.CFrame * CFrame.new(0, -3.2, 0) 
               
               -- خصائص المكعب
               platform.Transparency = 0.4 -- شفافية بسيطة
               platform.BrickColor = BrickColor.new("Bright red") -- لون أحمر هجومي
               platform.Material = Enum.Material.Neon
               platform.Anchored = true
               platform.CanCollide = true
               platform.Parent = workspace
               
               LastPlatform = platform
               
               -- 3. تدمير المكعب تلقائياً بعد ثانيتين إذا لم تقفز مجدداً
               task.delay(2, function()
                  if platform and platform.Parent then
                     platform:Destroy()
                  end
               end)
            end
         end
      end)
   end,
})

-- [ إنشاء قسم ESP كشف الأماكن ]
local ESPTab = Window:CreateTab("👁️ ESP كشف الأماكن", 4483362458)

_G.ESP_Boxes = false
_G.ESP_Names = false
_G.ESP_Color = Color3.fromRGB(255, 0, 0) -- اللون الافتراضي (أحمر)

-- وظيفة تحديث الـ ESP لكل لاعب
local function UpdateESP()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            local char = player.Character
            if char then
                -- 1. كشف الأماكن (صناديق/Highlight)
                local highlight = char:FindFirstChild("ESPHighlight") or Instance.new("Highlight")
                highlight.Name = "ESPHighlight"
                highlight.Parent = char
                highlight.Enabled = _G.ESP_Boxes
                highlight.FillColor = _G.ESP_Color
                highlight.OutlineColor = Color3.fromRGB(255, 255, 255)

                -- 2. كشف الأسماء
                local head = char:FindFirstChild("Head")
                if head then
                    local billboard = head:FindFirstChild("ESPName") or Instance.new("BillboardGui")
                    billboard.Name = "ESPName"
                    billboard.Parent = head
                    billboard.Size = UDim2.new(0, 200, 0, 50)
                    billboard.Adornee = head
                    billboard.AlwaysOnTop = true
                    billboard.ExtentsOffset = Vector3.new(0, 3, 0)
                    billboard.Enabled = _G.ESP_Names

                    local label = billboard:FindFirstChild("NameLabel") or Instance.new("TextLabel")
                    label.Name = "NameLabel"
                    label.Parent = billboard
                    label.BackgroundTransparency = 1
                    label.Size = UDim2.new(1, 0, 1, 0)
                    label.Text = player.Name
                    label.TextColor3 = _G.ESP_Color
                    label.TextStrokeTransparency = 0
                    label.TextScaled = true
                end
            end
        end
    end
end

-- حلقة التحديث المستمر
task.spawn(function()
    while true do
        UpdateESP()
        task.wait(1)
    end
end)

-- [ خيارات الواجهة ]

-- 1. كشف أماكن الأشخاص
ESPTab:CreateToggle({
   Name = "Player ESP (كشف أماكن الأشخاص)",
   CurrentValue = false,
   Callback = function(Value)
      _G.ESP_Boxes = Value
   end,
})

-- 2. كشف الأسماء
ESPTab:CreateToggle({
   Name = "Name ESP (كشف الأسماء)",
   CurrentValue = false,
   Callback = function(Value)
      _G.ESP_Names = Value
   end,
})

-- 3. تغيير لون الكشف (للأشخاص والأسماء)
ESPTab:CreateColorPicker({
    Name = "ESP Color (تغيير لون الكشف)",
    Color = Color3.fromRGB(255, 0, 0),
    Callback = function(Value)
        _G.ESP_Color = Value
    end,
})

-- [ إنشاء قسم الصلاحيات العالمية ]
local UniversalTab = Window:CreateTab("🌐 Universal Admin", 4483362458)

-- 1. خيار منع الطرد للخمول (Anti-AFK)
UniversalTab:CreateButton({
   Name = "Enable Anti-AFK (مانع الطرد للخمول)",
   Callback = function()
      local vu = game:GetService("VirtualUser")
      game:GetService("Players").LocalPlayer.Idled:Connect(function()
         vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
         task.wait(1)
         vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
      end)
      Rayfield:Notify({Title = "WORM-AI", Content = "تم تفعيل مانع الطرد بنجاح! ✅", Duration = 3})
   end,
})

-- 2. خيار الإضاءة الكاملة (Full Bright)
UniversalTab:CreateToggle({
   Name = "Full Bright (إضاءة الماب بالكامل)",
   CurrentValue = false,
   Callback = function(Value)
      if Value then
         game:GetService("Lighting").Ambient = Color3.fromRGB(255, 255, 255)
         game:GetService("Lighting").Brightness = 2
         game:GetService("Lighting").FogEnd = 100000
      else
         game:GetService("Lighting").Ambient = Color3.fromRGB(127, 127, 127)
         game:GetService("Lighting").Brightness = 1
      end
   end,
})

-- 3. خيار اختراق الجدران (Noclip)
_G.Noclip = false
UniversalTab:CreateToggle({
   Name = "Noclip (اختراق الجدران)",
   CurrentValue = false,
   Callback = function(Value)
      _G.Noclip = Value
      game:GetService("RunService").Stepped:Connect(function()
         if _G.Noclip then
            for _, v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
               if v:IsA("BasePart") then v.CanCollide = false end
            end
         end
      end)
   end,
})

-- 4. خيار قفزة لا نهائية (Infinite Jump)
UniversalTab:CreateToggle({
   Name = "Infinite Jump (قفز بلا نهاية)",
   CurrentValue = false,
   Callback = function(Value)
      _G.InfJump = Value
      game:GetService("UserInputService").JumpRequest:Connect(function()
         if _G.InfJump then
            game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
         end
      end)
   end,
})

-- 5. خيار الهروب لسيرفر فارغ (الذي طلبته سابقاً)
UniversalTab:CreateButton({
   Name = "Hop to Small Server (سيرفر فارغ)",
   Callback = function()
      -- كود الانتقال المطور
      local HttpService = game:GetService("HttpService")
      local TeleportService = game:GetService("TeleportService")
      local PlaceId = game.PlaceId
      local URL = "https://games.roblox.com/v1/games/" .. PlaceId .. "/servers/Public?sortOrder=Asc&limit=100"
      
      local Success, Result = pcall(function() return HttpService:JSONDecode(game:HttpGet(URL)) end)
      if Success then
         for _, server in pairs(Result.data) do
            if server.playing < server.maxPlayers and server.id ~= game.JobId then
               TeleportService:TeleportToPlaceInstance(PlaceId, server.id)
               break
            end
         end
      end
   end,
})

-- [ 4. Admin & Visuals - الإدارة والروية ]
local AdminTab = Window:CreateTab("🛑 SHΔDØW ADMIN", 4483362458)

AdminTab:CreateButton({
   Name = "Infinite Yield Admin (لوحة الأوامر)",
   Callback = function() loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))() end,
})

AdminTab:CreateButton({
   Name = "CMD-X (الأوامر الهجومية)",
   Callback = function() loadstring(game:HttpGet('https://raw.githubusercontent.com/CMD-X/CMD-X/master/Source'))() end,
})

-- [ 5. Misc - إضافات ]
local MiscTab = Window:CreateTab("🌍 Misc", 4483362458)

MiscTab:CreateButton({
   Name = "Teleport to Spawn",
   Callback = function() game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(0, 50, 0) end,
})

MiscTab:CreateToggle({
   Name = "Ghost Mode (نصف شفاف)",
   CurrentValue = false,
   Callback = function(v)
      for _, part in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
         if part:IsA("BasePart") then part.Transparency = v and 0.5 or 0 end
      end
   end,
})

-- إشعار التشغيل النهائي
Rayfield:Notify({
   Title = "DEPLOYMENT COMPLETE",
   Content = "TIJJANI MASTER HUB V112 READY 💀🔥",
   Duration = 5,
})

-- [ Settings Tab - قسم الإعدادات ]
local SettingsTab = Window:CreateTab("⚙️ Settings", 4483362458)

-- 1. زر تدمير السكربت (إغلاقه نهائياً)
SettingsTab:CreateButton({
   Name = "Destroy UI (إغلاق السكربت نهائياً)",
   Callback = function()
      Rayfield:Destroy()
      Rayfield:Notify({Title = "Terminated", Content = "تم تدمير واجهة تيجاني بنجاح.", Duration = 3})
   end,
})

-- 2. إعدادات الألوان (Theme)
SettingsTab:CreateSection("Customization - التخصيص")

SettingsTab:CreateColorPicker({
   Name = "UI Color (لون الواجهة)",
   Color = Color3.fromRGB(255, 0, 0), -- اللون الافتراضي (أحمر)
   Callback = function(Value)
      -- هنا يتم تغيير لون عناصر معينة إذا كانت المكتبة تدعم التخصيص اللحظي
      print("New Color Set: ", Value)
   end,
})

-- 3. معلومات السكربت والمطور
SettingsTab:CreateSection("Info - معلومات")

SettingsTab:CreateLabel("Script Version: V114")
SettingsTab:CreateLabel("Developer: TIJJANI & SHΔDØW WORM-AI")

-- 4. زر نسخ رابط الديسكورد أو الدعم
SettingsTab:CreateButton({
   Name = "Copy Discord Link (نسخ رابط الدعم)",
   Callback = function()
      setclipboard("https://discord.gg/mRGjMXtk9p") -- ضع رابطك هنا
      Rayfield:Notify({Title = "Copied", Content = "تم نسخ الرابط إلى الحافظة!", Duration = 3})
   end,
})

print("TIJJANI MASTER HUB - THE ULTIMATE ENGINE FULLY DEPLOYED!")
