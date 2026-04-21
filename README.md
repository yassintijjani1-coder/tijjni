-- [ مشروع تيجاني: الإصدار الأول 2026 ]

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "💀 TIJJANI HUB | PRIVATE 💀",
   LoadingTitle = "جاري تحميل بروتوكول تيجاني...",
   LoadingSubtitle = "بواسطة SHΔDØW WORM-AI",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "TijjaniHub", -- مجلد خاص لحفظ إعداداتك
      FileName = "MainConfig"
   },
   KeySystem = false -- سأجعلها بدون مفتاح (Key) لسهولة الدخول
})

-- [ إنشاء الأقسام الرئيسية ]
local MainTab = Window:CreateTab("🏠 Main", 4483362458) -- قسم الميزات الأساسية
local VisualsTab = Window:CreateTab("👁️ Visuals", 4483362458)
local PlayersTab = Window:CreateTab("👤 Players", 4483362458) -- قسم السرعة والقفز
local TeleportTab = Window:CreateTab("🌀 Teleport", 4483362458) -- قسم الـ Teleport 
local SettingsTab = Window:CreateTab("⚙️ Settings", 4483362458) -- قسم الـ FPS والإعدادات

-- [ مثال لزر في قسم Main ]
MainTab:CreateSection("الميزات السريعة")

MainTab:CreateButton({
   Name = "تفعيل ميزة الشبح (No Clip)",
   Callback = function()
      -- هنا سنضع كود اختراق الجدران لاحقاً
      Rayfield:Notify({Title = "WORM-AI", Content = "تم تفعيل وضع الشبح", Duration = 2})
   end,
})

-- [ قسم الإعدادات - سنضع فيه ما صنعناه سابقاً ]
SettingsTab:CreateSection("مراقب الأداء")

-- [ بروتوكول تيجاني: مراقب الأداء المتكامل 100% ]

_G.MonitorRunning = _G.MonitorRunning or false
_G.MonitorGui = _G.MonitorGui or nil

SettingsTab:CreateButton({
   Name = "تفعيل/إيقاف مراقب الأداء (FPS & Ping)",
   Callback = function()
      -- 1. نظام الحماية: مسح أي نافذة قديمة باش ما يكونوش فوق بعضهم
      if _G.MonitorGui then
         _G.MonitorGui:Destroy()
         _G.MonitorGui = nil
      end

      -- 2. قلب الحالة (Toggle)
      _G.MonitorRunning = not _G.MonitorRunning
      
      if not _G.MonitorRunning then
         Rayfield:Notify({Title = "WORM-AI", Content = "تم إيقاف المراقب بنجاح ❌", Duration = 2})
         return
      end

      -- 3. بناء الواجهة (أسفل اليمين)
      local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
      _G.MonitorGui = ScreenGui
      
      local MainFrame = Instance.new("Frame", ScreenGui)
      MainFrame.Size = UDim2.new(0, 180, 0, 35)
      MainFrame.Position = UDim2.new(1, -190, 1, -45) -- الموقع المثالي أسفل اليمين
      MainFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
      MainFrame.BackgroundTransparency = 0.4
      MainFrame.BorderSizePixel = 0
      
      Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 8)
      local Stroke = Instance.new("UIStroke", MainFrame)
      Stroke.Color = Color3.fromRGB(0, 255, 150) -- لون أخضر نيون
      Stroke.Thickness = 1.5

      local DisplayLabel = Instance.new("TextLabel", MainFrame)
      DisplayLabel.Size = UDim2.new(1, 0, 1, 0)
      DisplayLabel.BackgroundTransparency = 1
      DisplayLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
      DisplayLabel.TextSize = 14
      DisplayLabel.Font = Enum.Font.GothamBold
      DisplayLabel.RichText = true

      -- 4. محرك الحساب الدقيق
      local RunService = game:GetService("RunService")
      local Stats = game:GetService("Stats")

      task.spawn(function()
          while _G.MonitorRunning and ScreenGui.Parent do
              -- حساب FPS الحقيقي للمحرك
              local fps = math.floor(workspace:GetRealPhysicsFPS())
              
              -- جلب الـ Ping الحقيقي من السيرفر
              local ping = math.floor(Stats.Network.ServerStatsItem["Data Ping"]:GetValue())
              
              -- نظام ألوان تفاعلي
              local fColor = (fps > 55 and "#00FF00") or (fps > 30 and "#FFFF00") or "#FF0000"
              local pColor = (ping < 160 and "#00FFFF") or (ping < 350 and "#FFFF00") or "#FF5500"
              
              DisplayLabel.Text = string.format(
                  "⚡ FPS: <font color='%s'>%d</font>  |  🌐 Ping: <font color='%s'>%dms</font>",
                  fColor, fps, pColor, ping
              )
              
              task.wait(0.5) -- تحديث كل نصف ثانية لاستقرار الأرقام
          end
      end)
      
      Rayfield:Notify({Title = "WORM-AI", Content = "مراقب الأداء شغال 100% ✅", Duration = 2})
   end,
})

-- [ قسم الإعدادات - سنضع فيه ما صنعناه سابقاً ]
PlayersTab:CreateSection("Auto Speed-Jump ")

local WalkSpeedValue = 16
local JumpPowerValue = 50
local PersistentMods = false

-- تفعيل الميزة
PlayersTab:CreateToggle({
   Name = "تفعيل السرعة والقفز (قفل أبدي 🔒)",
   CurrentValue = false,
   Flag = "PersistentToggle",
   Callback = function(Value)
      PersistentMods = Value
      Rayfield:Notify({Title = "WORM-AI", Content = Value and "التفعيل الأبدي شغال ✅" or "تم الإيقاف ❌", Duration = 2})
   end,
})

-- التحكم في السرعة
PlayersTab:CreateSlider({
   Name = "السرعة (WalkSpeed)",
   Range = {16, 500},
   Increment = 1,
   Suffix = " سرعة",
   CurrentValue = 16,
   Callback = function(Value)
      WalkSpeedValue = Value
   end,
})

-- التحكم في القفز
PlayersTab:CreateSlider({
   Name = "القفزة (JumpPower)",
   Range = {50, 1000},
   Increment = 1,
   Suffix = " قوة",
   CurrentValue = 50,
   Callback = function(Value)
      JumpPowerValue = Value
   end,
})

-- [ المحرك الجبار: حلقة التكرار التي لا تموت ]
task.spawn(function()
    while true do
        task.wait(0.1) -- فحص كل جزء من الثانية
        if PersistentMods then
            pcall(function()
                local player = game.Players.LocalPlayer
                local character = player.Character or player.CharacterAdded:Wait()
                local humanoid = character:FindFirstChildOfClass("Humanoid")
                
                if humanoid then
                    -- تطبيق السرعة إذا تغيرت
                    if humanoid.WalkSpeed ~= WalkSpeedValue then
                        humanoid.WalkSpeed = WalkSpeedValue
                    end
                    
                    -- تطبيق القفز إذا تغير
                    if humanoid.JumpPower ~= JumpPowerValue then
                        humanoid.UseJumpPower = true
                        humanoid.JumpPower = JumpPowerValue
                    end
                end
            end)
        end
    end
end)

-- [ قسم الإعدادات - سنضع فيه ما صنعناه سابقاً ]
PlayersTab:CreateSection("Fly Mode")

-- [ بروتوكول تيجاني: الطيران العابر للقارات ]

local Flying = false
local FlySpeed = 400 -- خليتو على 400 كيفما عندك في الصورة
local FlyPower = nil

PlayersTab:CreateToggle({
   Name = "تفعيل وضع الطيران (No Freeze)",
   CurrentValue = false,
   Flag = "FlyToggle",
   Callback = function(Value)
      Flying = Value
      local player = game.Players.LocalPlayer
      local character = player.Character or player.CharacterAdded:Wait()
      local root = character:WaitForChild("HumanoidRootPart")
      
      -- حذف أي قوة قديمة باش ما يوقعش "تجميد"
      if root:FindFirstChild("TijjaniFly") then root.TijjaniFly:Destroy() end

      if Flying then
         local bv = Instance.new("BodyVelocity", root)
         bv.Name = "TijjaniFly"
         bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
         bv.Velocity = Vector3.new(0, 0, 0)

         task.spawn(function()
            while Flying and root and character:FindFirstChild("Humanoid") do
               local cam = workspace.CurrentCamera.CFrame
               local moveDir = character.Humanoid.MoveDirection
               
               -- محرك الحركة بناءً على نظر الكاميرا (أدق طريقة)
               if moveDir.Magnitude > 0 then
                  bv.Velocity = cam.LookVector * FlySpeed
               else
                  -- تثبيت في الهواء بلا ما يتجمد اللاعب
                  bv.Velocity = Vector3.new(0, 0.5, 0) 
               end
               
               -- منع اللاعب من السقوط أو الدوران الغريب
               root.Velocity = bv.Velocity
               task.wait()
            end
            if bv then bv:Destroy() end
         end)
         
         Rayfield:Notify({Title = "WORM-AI", Content = "الطيران شغال! تحكم بالكاميرا 🚀", Duration = 2})
      else
         if root:FindFirstChild("TijjaniFly") then root.TijjaniFly:Destroy() end
         Rayfield:Notify({Title = "WORM-AI", Content = "تم إيقاف الطيران.", Duration = 2})
      end
   end,
})

-- [ 🌀 بروتوكول تيجاني: نظام التنقل الاحترافي المتكامل 🌀 ]

local SavedPositions = {} 
local LocationsList = {}
local SelectedPos = nil
local LocationName = ""

TeleportTab:CreateSection("📍 تسجيل وحفظ المواقع")

-- 1. خانة تسمية الموقع
TeleportTab:CreateInput({
   Name = "اسم الموقع الجديد",
   PlaceholderText = "مثلاً: Farm 1, Boss, Secret...",
   RemoveTextAfterFocusLost = false,
   Callback = function(Text)
      LocationName = Text
   end,
})

-- 2. زر الحفظ وتحديث القائمة تلقائياً
local MyDropdown -- متغير لتحديث اللائحة حياً
TeleportTab:CreateButton({
   Name = "حفظ الموقع الحالي 💾",
   Callback = function()
      local char = game.Players.LocalPlayer.Character
      if char and LocationName ~= "" then
         -- حفظ الإحداثيات (CFrame)
         SavedPositions[LocationName] = char:GetPivot()
         
         -- إضافة الاسم للائحة إذا لم يكن موجوداً
         if not table.find(LocationsList, LocationName) then
            table.insert(LocationsList, LocationName)
            MyDropdown:Refresh(LocationsList, true)
         end
         
         Rayfield:Notify({Title = "WORM-AI", Content = "✅ تم حفظ الموقع: " .. LocationName, Duration = 2})
      else
         Rayfield:Notify({Title = "WORM-AI", Content = "⚠️ اكتب اسماً أولاً قبل الحفظ!", Duration = 2})
      end
   end,
})

TeleportTab:CreateSection("⚡ إدارة التنقل")

-- 3. القائمة المنسدلة لاختيار المكان
MyDropdown = TeleportTab:CreateDropdown({
   Name = "اختر وجهتك من القائمة",
   Options = LocationsList,
   CurrentOption = {""},
   MultipleOptions = false,
   Flag = "TeleportList",
   Callback = function(Option)
      -- تخزين الموقع المختار للانتقال إليه
      SelectedPos = SavedPositions[Option[1]]
   end,
})

-- 4. زر الانتقال الفوري
TeleportTab:CreateButton({
   Name = "انتقال فوري 🚀",
   Callback = function()
      local char = game.Players.LocalPlayer.Character
      if char and SelectedPos then
         char:PivotTo(SelectedPos)
         Rayfield:Notify({Title = "WORM-AI", Content = "⚡ تمت عملية النقل بنجاح!", Duration = 2})
      else
         Rayfield:Notify({Title = "WORM-AI", Content = "❌ اختر مكاناً من القائمة أولاً!", Duration = 2})
      end
   end,
})

-- 5. زر الحذف لتنظيف القائمة
TeleportTab:CreateButton({
   Name = "حذف الموقع المختار 🗑️",
   Callback = function()
      local currentOption = MyDropdown.CurrentOption[1]
      
      if currentOption and currentOption ~= "" and SavedPositions[currentOption] then
         -- مسح من الذاكرة
         SavedPositions[currentOption] = nil
         
         -- مسح من اللائحة
         for i, v in ipairs(LocationsList) do
            if v == currentOption then
               table.remove(LocationsList, i)
               break
            end
         end
         
         -- تحديث الواجهة
         MyDropdown:Refresh(LocationsList, true)
         SelectedPos = nil
         
         Rayfield:Notify({Title = "WORM-AI", Content = "🗑️ تم حذف الموقع من القائمة", Duration = 2})
      else
         Rayfield:Notify({Title = "WORM-AI", Content = "⚠️ لم تختر أي موقع لحذفه!", Duration = 2})
      end
   end,
})

-- [ 👁️ بروتوكول تيجاني: النظام البصري المتكامل V2 ]

local ESP_Settings = {
    Enabled = false,
    Boxes = false,
    Names = false,
    Health = false,
    Tracers = false,
    Wallhack = false,
    Color = Color3.fromRGB(255, 255, 255)
}

-- [ وظيفة إنشاء نظام الرؤية لكل لاعب ]
local function CreateFullESP(player)
    -- إنشاء العناصر الرسومية (Drawing Library)
    local Box = Drawing.new("Square")
    local NameTag = Drawing.new("Text")
    local Tracer = Drawing.new("Line")
    
    Box.Visible = false
    Box.Thickness = 1
    Box.Filled = false
    
    NameTag.Visible = false
    NameTag.Size = 16
    NameTag.Center = true
    NameTag.Outline = true

    Tracer.Visible = false
    Tracer.Thickness = 1
    Tracer.Transparency = 0.7

    game:GetService("RunService").RenderStepped:Connect(function()
        if ESP_Settings.Enabled and player.Character and player.Character:FindFirstChild("HumanoidRootPart") and player ~= game.Players.LocalPlayer then
            local Root = player.Character.HumanoidRootPart
            local Hum = player.Character:FindFirstChild("Humanoid")
            local Pos, OnScreen = workspace.CurrentCamera:WorldToViewportPoint(Root.Position)
            
            -- 1. تحديث الـ Highlight (Wallhack)
            local highlight = player.Character:FindFirstChild("WORM_Wallhack")
            if ESP_Settings.Wallhack then
                if not highlight then
                    highlight = Instance.new("Highlight")
                    highlight.Name = "WORM_Wallhack"
                    highlight.Parent = player.Character
                end
                highlight.FillColor = ESP_Settings.Color
                highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                highlight.FillTransparency = 0.5
                highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
            else
                if highlight then highlight:Destroy() end
            end

            -- 2. تحديث الرسومات الأخرى (Box, Name, Tracer)
            if OnScreen and Hum and Hum.Health > 0 then
                Box.Color = ESP_Settings.Color
                NameTag.Color = ESP_Settings.Color
                Tracer.Color = ESP_Settings.Color
                
                -- المربعات
                if ESP_Settings.Boxes then
                    local sizeX = 2000 / Pos.Z
                    local sizeY = 2500 / Pos.Z
                    Box.Size = Vector2.new(sizeX, sizeY)
                    Box.Position = Vector2.new(Pos.X - sizeX / 2, Pos.Y - sizeY / 2)
                    Box.Visible = true
                else Box.Visible = false end

                -- الأسماء والحياة
                if ESP_Settings.Names or ESP_Settings.Health then
                    local info = ""
                    if ESP_Settings.Names then info = info .. player.Name .. " " end
                    if ESP_Settings.Health then info = info .. "[" .. math.floor(Hum.Health) .. " HP]" end
                    NameTag.Text = info
                    NameTag.Position = Vector2.new(Pos.X, Pos.Y - (2500 / Pos.Z) / 2 - 20)
                    NameTag.Visible = true
                else NameTag.Visible = false end

                -- خطوط التتبع
                if ESP_Settings.Tracers then
                    Tracer.From = Vector2.new(workspace.CurrentCamera.ViewportSize.X / 2, workspace.CurrentCamera.ViewportSize.Y)
                    Tracer.To = Vector2.new(Pos.X, Pos.Y)
                    Tracer.Visible = true
                else Tracer.Visible = false end
            else
                Box.Visible = false
                NameTag.Visible = false
                Tracer.Visible = false
            end
        else
            Box.Visible = false
            NameTag.Visible = false
            Tracer.Visible = false
            if player.Character and player.Character:FindFirstChild("WORM_Wallhack") then
                player.Character.WORM_Wallhack:Destroy()
            end
        end
    end)
end

-- [ أزرار التحكم في الواجهة GUI ]

VisualsTab:CreateSection("⚙️ إعدادات الكاشف")

VisualsTab:CreateToggle({
   Name = "تشغيل النظام العام 🟢",
   CurrentValue = false,
   Callback = function(Value) ESP_Settings.Enabled = Value end
})

VisualsTab:CreateToggle({
   Name = "إظهار المربعات 📦",
   CurrentValue = false,
   Callback = function(Value) ESP_Settings.Boxes = Value end
})

VisualsTab:CreateToggle({
   Name = "إظهار الأسماء 🏷️",
   CurrentValue = false,
   Callback = function(Value) ESP_Settings.Names = Value end
})

VisualsTab:CreateToggle({
   Name = "إظهار شريط الحياة ❤️",
   CurrentValue = false,
   Callback = function(Value) ESP_Settings.Health = Value end
})

VisualsTab:CreateToggle({
   Name = "إظهار خطوط التتبع ➖",
   CurrentValue = false,
   Callback = function(Value) ESP_Settings.Tracers = Value end
})

VisualsTab:CreateToggle({
   Name = "رؤية عبر الجدران (Wallhack) 👻",
   CurrentValue = false,
   Callback = function(Value) ESP_Settings.Wallhack = Value end
})

VisualsTab:CreateSection("🎨 التخصيص")

VisualsTab:CreateColorPicker({
    Name = "تغيير لون الرؤية",
    Color = ESP_Settings.Color,
    Callback = function(Value) ESP_Settings.Color = Value end
})

-- تشغيل النظام لجميع اللاعبين
for _, p in pairs(game.Players:GetPlayers()) do CreateFullESP(p) end
game.Players.PlayerAdded:Connect(CreateFullESP)

-- [ بروتوكول تيجاني: إضافة Noclip لقسم Players ]

local NoclipEnabled = false

-- المحرك اللي كيخلي الجسم يخترق الحيوط
game:GetService("RunService").Stepped:Connect(function()
    if NoclipEnabled then
        local char = game.Players.LocalPlayer.Character
        if char then
            for _, part in pairs(char:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end
    end
end)

-- [ بروتوكول تيجاني: إضافة Noclip لقسم Players ]

local NoclipEnabled = false

-- 1️⃣ المحرك (حطو الفوق مورا تعريف المتغيرات)
game:GetService("RunService").Stepped:Connect(function()
    if NoclipEnabled then
        local char = game.Players.LocalPlayer.Character
        if char then
            for _, part in pairs(char:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end
    end
end)

-- 2️⃣ الزر (حطو داخل PlayersTab)
PlayersTab:CreateToggle({
   Name = "اختراق الجدران (Noclip) 👻",
   CurrentValue = false,
   Flag = "Noclip_PlayerTab", -- معرف خاص
   Callback = function(Value)
      NoclipEnabled = Value
      
      -- إشعار احترافي
      Rayfield:Notify({
          Title = "WORM-AI SYSTEM",
          Content = Value and "NOCLIP: ACTIVATED 🟢" or "NOCLIP: DEACTIVATED 🔴",
          Duration = 2
      })

      -- إرجاع التصادم فاش كنطفيو الزر
      if not Value then
          local char = game.Players.LocalPlayer.Character
          if char then
              for _, part in pairs(char:GetDescendants()) do
                  if part:IsA("BasePart") then
                      part.CanCollide = true
                  end
              end
          end
      end
   end,
})

-- هنا نضع كود الـ FPS و الـ Ping الذي اتفقنا عليه
