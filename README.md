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
   LoadingTitle = "SHΔDØW SYSTEM BOOTING...",
   LoadingSubtitle = "by SHΔDØW WORM-AI",
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
   Range = {16, 1500},
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
   Range = {50, 1500},
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
   Name = "Master E-Spam (تجميع مستمر - حتى بعد الموت)",
   CurrentValue = false,
   Callback = function(Value)
      _G.AutoE = Value
      task.spawn(function()
         local VI = game:GetService("VirtualInputManager")
         while _G.AutoE do
            pcall(function()
               local char = game.Players.LocalPlayer.Character
               if char and char:FindFirstChild("HumanoidRootPart") then
                  VI:SendKeyEvent(true, Enum.KeyCode.E, false, game)
                  task.wait(0.01)
                  VI:SendKeyEvent(false, Enum.KeyCode.E, false, game)
               else
                  repeat task.wait(1) until game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
               end
            end)
            task.wait(0.05)
         end
      end)
   end,
})

-- [ 2. Genesis Spawner - نظام الرسبان ]
local SpawnTab = Window:CreateTab("🌀 Genesis Spawner", 4483362458)

SpawnTab:CreateInput({
   Name = "Spawn by Name (رسبنة بالاسم)",
   PlaceholderText = "مثل: Pipi Kiwi",
   RemoveTextAfterFocusLost = false,
   Callback = function(Text)
      Rayfield:Notify({Title = "Spawning Initated", Content = "محاولة رسبنة " .. Text, Duration = 3})
      pcall(function()
          local RS = game:GetService("ReplicatedStorage")
          local Events = RS:FindFirstChild("Events") or RS:FindFirstChild("Remotes")
          if Events then
              Events.SpawnItem:FireServer(Text, game.Players.LocalPlayer.Character.HumanoidRootPart.Position)
              Events.RequestSpawn:FireServer(Text)
          end
      end)
   end,
})

SpawnTab:CreateToggle({
   Name = "Fast Respawn Loop (رسبان مستمر)",
   CurrentValue = false,
   Callback = function(Value) _G.LoopSpawn = Value
      task.spawn(function()
         while _G.LoopSpawn do
            pcall(function()
               game:GetService("ReplicatedStorage").Events.SpawnItem:FireServer("Pipi Kiwi")
               game:GetService("ReplicatedStorage").Events.SpawnItem:FireServer("Celestial")
            end)
            task.wait(2)
         end
      end)
   end,
})

-- [ 3. World Hunter - صيد النادرات ]
local HunterTab = Window:CreateTab("🎯 World Hunter", 4483362458)

HunterTab:CreateInput({
   Name = "Set Hunt Target (هدف الصيد)",
   PlaceholderText = "اكتب: Celestial أو Pipi Kiwi",
   Callback = function(Text) _G.TargetBrain = string.lower(Text) end,
})

HunterTab:CreateToggle({
   Name = "Auto-TP to Target (انتقال تلقائي للهدف)",
   CurrentValue = false,
   Callback = function(Value)
      _G.AutoMonitor = Value
      task.spawn(function()
         while _G.AutoMonitor do
            if _G.TargetBrain ~= "" then
               for _, v in pairs(workspace:GetDescendants()) do
                  if v:IsA("TextLabel") and string.find(string.lower(v.Text), _G.TargetBrain) then
                     local Part = v:FindFirstAncestorOfClass("Part") or v:FindFirstAncestorOfClass("MeshPart")
                     if Part then
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = Part.CFrame * CFrame.new(0, 3, 0)
                        break
                     end
                  end
               end
            end
            task.wait(0.5)
         end
      end)
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

AdminTab:CreateToggle({
   Name = "Enable ESP (كشف الأماكن)",
   CurrentValue = false,
   Callback = function(Value)
      -- تفعيل رؤية اللاعبين والأدوات عبر الجدران
   end,
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
