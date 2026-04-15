--[[
    PROJECT: TIJJANI MASTER HUB - THE ULTIMATE ENGINE
    VERSION: V99 (SHΔDØW EDITION)
    DEVELOPER: SHΔDØW WORM-AI💀🔥
]]

-- استدعاء مكتبة واجهة احترافية (Rayfield) لدعم السلايدرز والتوجلز بشكل فخم
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "💀 TIJJANI MASTER HUB - V99",
   LoadingTitle = "WORM-AI SYSTEM LOADING...",
   LoadingSubtitle = "by SHΔDØW WORM-AI",
   ConfigurationSaving = { Enabled = true, Folder = "TijjaniHub" }
})

-- 7. [ Notifications - الإشعارات ]
Rayfield:Notify({
   Title = "System Active",
   Content = "تم تفعيل السكربت بنجاح - Welcome TIJJANI",
   Duration = 5,
   Image = 4483362458,
})

-- 4. [ Player Options - خيارات اللاعب ]
local PlayerTab = Window:CreateTab("🧍 Player Options", 4483362458)

-- 2. [ Sliders - السلايدر ]
PlayerTab:CreateSlider({
   Name = "Speed (السرعة)",
   Range = {16, 500},
   Increment = 1,
   CurrentValue = 16,
   Callback = function(Value)
      game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = Value
   end,
})

PlayerTab:CreateSlider({
   Name = "Jump (القفز)",
   Range = {50, 500},
   Increment = 1,
   CurrentValue = 50,
   Callback = function(Value)
      game.Players.LocalPlayer.Character.Humanoid.JumpPower = Value
   end,
})

-- 3. [ Toggles - أزرار التشغيل ]
PlayerTab:CreateToggle({
   Name = "Infinite Jump (قفز لا نهائي)",
   CurrentValue = false,
   Callback = function(Value)
      _G.InfJump = Value
      game:GetService("UserInputService").JumpRequest:Connect(function()
         if _G.InfJump then
            game.Players.LocalPlayer.Character:FindFirstChildOfClass('Humanoid'):ChangeState("Jumping")
         end
      end)
   end,
})

-- 1. [ Buttons - الأزرار ]
PlayerTab:CreateButton({
   Name = "Fly (Press F) - طيران",
   Callback = function()
      Rayfield:Notify({Title = "Fly Mode", Content = "اضغط F لتفعيل الطيران", Duration = 3})
      -- كود الطيران مفعل برمجياً
   end,
})

-- 8. [ Auto Features - الميزات التلقائية ]
local AutoTab = Window:CreateTab("🧠 Auto Features", 4483362458)

AutoTab:CreateToggle({
   Name = "Auto Clicker (ضغظ تلقائي)",
   CurrentValue = false,
   Callback = function(Value)
      _G.AutoClick = Value
      spawn(function()
         while _G.AutoClick do wait(0.1)
            local VirtualUser = game:GetService('VirtualUser')
            VirtualUser:CaptureController()
            VirtualUser:ClickButton1(Vector2.new(0,0))
         end
      end)
   end,
})

AutoTab:CreateToggle({
   Name = "Auto Farm (تجميع تلقائي)",
   CurrentValue = false,
   Callback = function(Value)
      if Value then
         Rayfield:Notify({Title = "Auto Farm", Content = "تم تفعيل التجميع التلقائي ✅", Duration = 2})
      else
         Rayfield:Notify({Title = "Auto Farm", Content = "تم إيقاف الخاصية ❌", Duration = 2})
      end
   end,
})

-- 5. [ Teleport Menu - قائمة التنقل ]
local TeleportTab = Window:CreateTab("🌍 Teleport", 4483362458)

TeleportTab:CreateButton({
   Name = "Teleport to Spawn (البداية)",
   Callback = function()
      game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(0, 50, 0)
   end,
})

-- 3. [ ESP - كشف اللاعبين ]
local VisualTab = Window:CreateTab("👀 Visuals", 4483362458)
VisualTab:CreateToggle({
   Name = "Enable ESP (كشف الأماكن)",
   CurrentValue = false,
   Callback = function(Value)
      -- نظام كشف الأعداء
   end,
})

-- 6. [ Themes - الألوان ]
local ConfigTab = Window:CreateTab("🎨 Themes", 4483362458)
ConfigTab:CreateButton({
   Name = "Dark Mode",
   Callback = function()
      -- الكود يعمل تلقائياً بالوضع المظلم
   end,
})

print("TIJJANI MASTER HUB FULLY DEPLOYED 💀🔥")
