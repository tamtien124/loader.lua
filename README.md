-- [[ PC ULTIMIZER V13 - PREMIUM NOTIFICATION EDITION ]]
if not game:IsLoaded() then game.Loaded:Wait() end

local CoreGui = game:GetService("CoreGui")
local Lighting = game:GetService("Lighting")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

-- [ 1. THÔNG BÁO PREMIUM (SLIDE NOTIFICATION) ]
local function ShowPremiumNotify()
    local NotifyGui = Instance.new("ScreenGui", CoreGui)
    local NotifyFrame = Instance.new("Frame", NotifyGui)
    NotifyFrame.Size = UDim2.new(0, 250, 0, 70)
    NotifyFrame.Position = UDim2.new(1, 20, 0.1, 0) -- Bắt đầu từ ngoài màn hình bên phải
    NotifyFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
    NotifyFrame.BorderSizePixel = 0
    
    local Corner = Instance.new("UICorner", NotifyFrame)
    Corner.CornerRadius = UDim.new(0, 8)
    local Stroke = Instance.new("UIStroke", NotifyFrame)
    Stroke.Color = Color3.fromRGB(0, 255, 150)
    Stroke.Thickness = 2
    
    local Icon = Instance.new("TextLabel", NotifyFrame)
    Icon.Size = UDim2.new(0, 40, 1, 0)
    Icon.Text = "✔"
    Icon.TextColor3 = Color3.fromRGB(0, 255, 150)
    Icon.TextSize = 25
    Icon.BackgroundTransparency = 1
    
    local Msg = Instance.new("TextLabel", NotifyFrame)
    Msg.Size = UDim2.new(1, -50, 1, 0)
    Msg.Position = UDim2.new(0, 45, 0, 0)
    Msg.Text = "Cảm ơn đã sử dụng tối ưu!\nChúc bạn trải nghiệm cực mượt."
    Msg.TextColor3 = Color3.new(1, 1, 1)
    Msg.TextSize = 13
    Msg.Font = Enum.Font.SourceSansBold
    Msg.TextXAlignment = Enum.TextXAlignment.Left
    Msg.BackgroundTransparency = 1

    -- Hiệu ứng trượt vào (Slide In)
    NotifyFrame:TweenPosition(UDim2.new(1, -270, 0.1, 0), "Out", "Quart", 1)
    
    -- Đợi 4 giây rồi trượt ra (Slide Out)
    task.delay(4, function()
        NotifyFrame:TweenPosition(UDim2.new(1, 20, 0.1, 0), "In", "Quart", 1)
        task.wait(1)
        NotifyGui:Destroy()
    end)
end

ShowPremiumNotify()

-- [ 2. TỰ ĐỘNG XÓA SƯƠNG MÙ ]
local function ClearFog()
    Lighting.FogEnd = 9e9
    Lighting.FogStart = 9e9
    for _, v in pairs(Lighting:GetDescendants()) do
        if v:IsA("Atmosphere") or v:IsA("Clouds") then v:Destroy() end
    end
end
ClearFog()

-- [ 3. FPS LABEL ]
local ScreenGui = Instance.new("ScreenGui", CoreGui)
local FPSLabel = Instance.new("TextLabel", ScreenGui)
FPSLabel.Size = UDim2.new(0, 80, 0, 22)
FPSLabel.Position = UDim2.new(0, 10, 0, 10)
FPSLabel.BackgroundTransparency = 0.5
FPSLabel.BackgroundColor3 = Color3.new(0, 0, 0)
FPSLabel.TextColor3 = Color3.fromRGB(0, 255, 150)
FPSLabel.Font = Enum.Font.Code
FPSLabel.TextSize = 14
Instance.new("UICorner", FPSLabel)

RunService.RenderStepped:Connect(function()
    FPSLabel.Text = "FPS: " .. math.floor(1/RunService.RenderStepped:Wait())
end)

-- [ 4. NÚT SHOW/HIDE ]
local menuVisible = true
local toggleBtn = Instance.new("TextButton", ScreenGui)
toggleBtn.Size = UDim2.new(0, 80, 0, 25)
toggleBtn.Position = UDim2.new(0, 10, 0.35, -40)
toggleBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
toggleBtn.Text = "HIDE MENU"
toggleBtn.TextColor3 = Color3.new(1, 1, 1)
toggleBtn.Font = Enum.Font.SourceSansBold
toggleBtn.TextSize = 11
Instance.new("UICorner", toggleBtn)

-- [ 5. CỤM NÚT FIX LAG ]
local ButtonFrame = Instance.new("Frame", ScreenGui)
ButtonFrame.Size = UDim2.new(0, 130, 0, 150)
ButtonFrame.Position = UDim2.new(0, 10, 0.35, 0)
ButtonFrame.BackgroundTransparency = 1
local Layout = Instance.new("UIListLayout", ButtonFrame)
Layout.Padding = UDim.new(0, 6)

-- [ 6. LOGIC FIX LAG ]
local function ApplyOptimize(lv)
    if lv >= 1 then
        settings().Rendering.QualityLevel = 1
        Lighting.GlobalShadows = false
        ClearFog()
    end
    if lv >= 2 then
        for _, v in pairs(game:GetDescendants()) do
            if v:IsA("BasePart") or v:IsA("MeshPart") then
                v.Material = Enum.Material.SmoothPlastic
                v.CastShadow = false
            end
        end
    end
    if lv >= 3 then
        for _, v in pairs(game:GetDescendants()) do
            if v:IsA("Decal") or v:IsA("Texture") or v:IsA("ParticleEmitter") or v:IsA("Trail") then
                v:Destroy()
            end
        end
        for _, p in pairs(game.Players:GetPlayers()) do
            if p ~= game.Players.LocalPlayer and p.Character then
                for _, acc in pairs(p.Character:GetChildren()) do
                    if acc:IsA("Accessory") then acc:Destroy() end
                end
            end
        end
    end
end

-- [ 7. RENDER NÚT ]
local function CreateBtn(name, color, callback)
    local btn = Instance.new("TextButton", ButtonFrame)
    btn.Size = UDim2.new(1, 0, 0, 32)
    btn.BackgroundColor3 = color
    btn.Text = name
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Font = Enum.Font.SourceSansBold
    btn.TextSize = 12
    Instance.new("UICorner", btn)
    btn.MouseButton1Click:Connect(function() callback(btn) end)
end

CreateBtn("LEVEL 1: LOW", Color3.fromRGB(45, 45, 45), function() ApplyOptimize(1) end)
CreateBtn("LEVEL 2: MEDIUM", Color3.fromRGB(0, 85, 160), function() ApplyOptimize(2) end)
CreateBtn("LEVEL 3: ULTIMATE", Color3.fromRGB(160, 0, 0), function() ApplyOptimize(3) end)

local is120 = false
if setfpscap then setfpscap(30) end 

CreateBtn("FPS CAP: 30", Color3.fromRGB(90, 90, 90), function(b)
    is120 = not is120
    if setfpscap then setfpscap(is120 and 120 or 30) end
    b.Text = "FPS CAP: " .. (is120 and "120" or "30")
    b.BackgroundColor3 = is120 and Color3.fromRGB(0, 160, 100) or Color3.fromRGB(90, 90, 90)
end)

toggleBtn.MouseButton1Click:Connect(function()
    menuVisible = not menuVisible
    ButtonFrame.Visible = menuVisible
    toggleBtn.Text = menuVisible and "HIDE MENU" or "SHOW MENU"
end)
