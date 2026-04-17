-- //////////////////////////////////////////////////////////////
-- //         BLOCK KEBERUNTUNGAN - SCRIPT BY RANNXITERS      //
-- //                  FREE & NO API KEY                      //
-- //              JANGAN DI PERJUAL BELIKAN!                 //
-- //////////////////////////////////////////////////////////////

-- // SERVICES
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local StarterGui = game:GetService("StarterGui")

local Player = Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
local Humanoid = Character:WaitForChild("Humanoid")

-- // VARIABLES
local WindowOpen = true
local FlySpeed = 50
local FlyEnabled = false
local NoclipEnabled = false
local BlockEnabled = false
local SpeedEnabled = false
local JumpEnabled = false
local WallHopEnabled = false
local WalkOnWallEnabled = false
local Connections = {}

-- // GUI MAKING
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local TopBar = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local CloseBtn = Instance.new("TextButton")
local MinimizeBtn = Instance.new("TextButton")
local TabHolder = Instance.new("Frame")
local ContentHolder = Instance.new("Frame")

-- // PROPERTIES
ScreenGui.Name = "RANNXITERS_HUB"
ScreenGui.Parent = game:GetService("CoreGui")
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.new(0.05, 0.05, 0.05)
MainFrame.BorderSizePixel = 2
MainFrame.BorderColor3 = Color3.new(1, 0.2, 0.5)
MainFrame.Size = UDim2.new(0, 340, 0, 480) -- UKURAN BESAR BUAT HP
MainFrame.Position = UDim2.new(0.5, -170, 0.5, -240)
MainFrame.ClipsDescendants = true

-- // TOP BAR
TopBar.Parent = MainFrame
TopBar.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
TopBar.Size = UDim2.new(1, 0, 0, 40)
TopBar.BorderSizePixel = 0

Title.Parent = TopBar
Title.BackgroundTransparency = 1
Title.Size = UDim2.new(1, -80, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.Font = Enum.Font.GothamBold
Title.TextColor3 = Color3.new(1, 1, 1)
Title.Text = "🎲 BLOCK KEBERUNTUNGAN"
Title.TextSize = 14
Title.TextXAlignment = Enum.TextXAlignment.Left

MinimizeBtn.Parent = TopBar
MinimizeBtn.BackgroundColor3 = Color3.new(0.2, 0.6, 1)
MinimizeBtn.Size = UDim2.new(0, 30, 0, 30)
MinimizeBtn.Position = UDim2.new(1, -70, 0, 5)
MinimizeBtn.Font = Enum.Font.GothamBold
MinimizeBtn.TextColor3 = Color3.new(1,1,1)
MinimizeBtn.Text = "_"
MinimizeBtn.TextSize = 14

CloseBtn.Parent = TopBar
CloseBtn.BackgroundColor3 = Color3.new(1, 0.2, 0.2)
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -35, 0, 5)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextColor3 = Color3.new(1,1,1)
CloseBtn.Text = "X"
CloseBtn.TextSize = 14

-- // TABS
TabHolder.Parent = MainFrame
TabHolder.Size = UDim2.new(1, 0, 0, 40)
TabHolder.Position = UDim2.new(0, 0, 0, 40)
TabHolder.BackgroundTransparency = 1

ContentHolder.Parent = MainFrame
ContentHolder.Size = UDim2.new(1, -20, 1, -100)
ContentHolder.Position = UDim2.new(0, 10, 0, 85)
ContentHolder.BackgroundTransparency = 1

-- // FUNCTIONS
function MakeTab(Name, Position)
    local Btn = Instance.new("TextButton")
    Btn.Parent = TabHolder
    Btn.BackgroundColor3 = Color3.new(0.15, 0.15, 0.15)
    Btn.Size = UDim2.new(0, 110, 1, 0)
    Btn.Position = UDim2.new(0, Position, 0, 0)
    Btn.Font = Enum.Font.GothamBold
    Btn.TextColor3 = Color3.new(1,1,1)
    Btn.Text = Name
    Btn.TextSize = 12
    Btn.BorderSizePixel = 0
    Btn.AutoLocalize = false
    return Btn
end

function MakeSection(TitleText)
    local Sec = Instance.new("Frame")
    Sec.BackgroundColor3 = Color3.new(0.08, 0.08, 0.08)
    Sec.Size = UDim2.new(1, 0, 1, 0)
    Sec.Position = UDim2.new(0, 0, 0, 0)
    Sec.Visible = false
    
    local Tit = Instance.new("TextLabel")
    Tit.Parent = Sec
    Tit.BackgroundTransparency = 1
    Tit.Size = UDim2.new(1, -10, 0, 30)
    Tit.Position = UDim2.new(0, 5, 0, 5)
    Tit.Font = Enum.Font.GothamBold
    Tit.TextColor3 = Color3.new(1, 0.4, 0.7)
    Tit.Text = TitleText
    Tit.TextSize = 14
    Tit.TextXAlignment = Enum.TextXAlignment.Left
    
    return Sec
end

function MakeToggle(Parent, Text, Func)
    local Btn = Instance.new("TextButton")
    Btn.Parent = Parent
    Btn.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
    Btn.Size = UDim2.new(1, -10, 0, 50) -- TOMBOL BESAR BUAT JARI HP
    Btn.Position = UDim2.new(0, 5, 0, 40)
    Btn.Font = Enum.Font.GothamBold
    Btn.TextColor3 = Color3.new(1,1,1)
    Btn.Text = Text .. "\n❌ OFF"
    Btn.TextSize = 13
    Btn.BorderSizePixel = 0
    Btn.AutoLocalize = false
    
    local Enabled = false
    Btn.MouseButton1Click:Connect(function()
        Enabled = not Enabled
        if Enabled then
            Btn.Text = Text .. "\n✅ ON"
            Btn.BackgroundColor3 = Color3.new(0, 0.6, 0.2)
        else
            Btn.Text = Text .. "\n❌ OFF"
            Btn.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
        end
        Func(Enabled)
    end)
    return Btn
end

function MakeButton(Parent, Text, Color, Func)
    local Btn = Instance.new("TextButton")
    Btn.Parent = Parent
    Btn.BackgroundColor3 = Color3.fromHex(Color)
    Btn.Size = UDim2.new(1, -10, 0, 50)
    Btn.Position = UDim2.new(0, 5, 0, 40)
    Btn.Font = Enum.Font.GothamBold
    Btn.TextColor3 = Color3.new(1,1,1)
    Btn.Text = Text
    Btn.TextSize = 13
    Btn.BorderSizePixel = 0
    Btn.MouseButton1Click:Connect(Func)
    return Btn
end

-- // CREATE TABS
local Tab1 = MakeTab("📦 BLOCK", 0)
local Tab2 = MakeTab("🚀 MOVEMENT", 112)
local Tab3 = MakeTab("🔨 FUN TOOLS", 224)

-- // CONTENT
local ContentBlock = MakeSection("🎲 BLOCK KEBERUNTUNGAN")
local ContentMove = MakeSection("🚀 MOVEMENT TOOLS")
local ContentFun = MakeSection("🔨 FUN & ANIM")

ContentBlock.Parent = ContentHolder
ContentMove.Parent = ContentHolder
ContentFun.Parent = ContentHolder

-- ==============================================
--              TAB 1: BLOCK
-- ==============================================
MakeToggle(ContentBlock, "💎 BLOCK KEBERUNTUNGAN", function(v) 
    BlockEnabled = v
end)

-- // BLOCK LOOP
spawn(function()
    while wait() do
        if BlockEnabled then
            pcall(function()
                for _, part in pairs(workspace:GetChildren()) do
                    if part:IsA("Part") or part:IsA("UnionOperation") or part:IsA("MeshPart") then
                        if part.Name ~= HumanoidRootPart.Name and part.CanCollide == true then
                            part.CanCollide = false
                        end
                    end
                end
            end)
        end
    end
end)

-- ==============================================
--              TAB 2: MOVEMENT
-- ==============================================
MakeToggle(ContentMove, "✨ FLY MODE", function(v)
    FlyEnabled = v
    if v then
        Humanoid.PlatformStand = true
        Humanoid:SetStateEnabled(Enum.HumanoidStateType.FallingDown, false)
    else
        Humanoid.PlatformStand = false
        Humanoid:SetStateEnabled(Enum.HumanoidStateType.FallingDown, true)
    end
end)

MakeToggle(ContentMove, "👻 NOCLIP", function(v)
    NoclipEnabled = v
end)

MakeToggle(ContentMove, "⚡ SPEED HACK", function(v)
    SpeedEnabled = v
    if v then
        Humanoid.WalkSpeed = 100
    else
        Humanoid.WalkSpeed = 16
    end
end)

MakeToggle(ContentMove, "🦘 LOMBAT TINGGI", function(v)
    JumpEnabled = v
    if v then
        Humanoid.JumpPower = 150
    else
        Humanoid.JumpPower = 50
    end
end)

MakeToggle(ContentMove, "🧗 WALL HOP", function(v)
    WallHopEnabled = v
end)

MakeToggle(ContentMove, "🧱 WALK ON WALL", function(v)
    WalkOnWallEnabled = v
end)

-- // FLY LOOP
RunService.RenderStepped:Connect(function()
    if FlyEnabled and HumanoidRootPart then
        local CameraCF = workspace.CurrentCamera.CFrame
        local Direction = Vector3.new()
        
        -- MOBILE SUPPORT & KEYBOARD
        if UserInputService:IsKeyDown(Enum.KeyCode.W) then Direction += CameraCF.LookVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.S) then Direction += -CameraCF.LookVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.A) then Direction += -CameraCF.RightVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.D) then Direction += CameraCF.RightVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.Space) then Direction += Vector3.new(0,1,0) end
        if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then Direction += Vector3.new(0,-1,0) end
        
        HumanoidRootPart.Velocity = Direction * FlySpeed
    end
end)

-- // NOCLIP LOOP
RunService.Stepped:Connect(function()
    if NoclipEnabled and Character then
        for _, part in pairs(Character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

-- // WALLHOP
UserInputService.JumpRequest:Connect(function()
    if WallHopEnabled and Character then
        local hrp = Character:FindChild("HumanoidRootPart")
        if hrp then
            hrp.Velocity = Vector3.new(0, 100, 0)
        end
    end
end)

-- // WALK ON WALL
RunService.Stepped:Connect(function()
    if WalkOnWallEnabled and Character and Humanoid then
        Humanoid:ChangeState(Enum.HumanoidStateType.Climbing)
    end
end)

-- ==============================================
--              TAB 3: FUN TOOLS
-- ==============================================
MakeButton(ContentFun, "🔨 BAN HAMMER", "#ff4444", function()
    local Hammer = Instance.new("Part")
    Hammer.Name = "BanHammer"
    Hammer.Parent = Workspace
    Hammer.Shape = Enum.PartType.Cylinder
    Hammer.Size = Vector3.new(2, 5, 2)
    Hammer.CFrame = HumanoidRootPart.CFrame * CFrame.new(0, 0, -10)
    Hammer.BrickColor = BrickColor.new("Really black")
    Hammer.Material = Enum.Material.Metal
    Hammer.CanCollide = false
    
    local Head = Instance.new("Part")
    Head.Name = "Head"
    Head.Parent = Workspace
    Head.Shape = Enum.PartType.Ball
    Head.Size = Vector3.new(4,4,4)
    Head.CFrame = Hammer.CFrame * CFrame.new(0, -3, 0)
    Head.BrickColor = BrickColor.new("Really red")
    Head.Material = Enum.Material.Neon
    Head.CanCollide = false
    
    local Weld = Instance.new("Weld")
    Weld.Part0 = Hammer
    Weld.Part1 = Head
    Weld.Parent = Hammer
    
    -- ANIMASI PALU NENGKOL
    spawn(function()
        for i = 1, 10 do
            Hammer.CFrame = Hammer.CFrame * CFrame.new(0, 0, -2)
            wait(0.05)
            Hammer.CFrame = Hammer.CFrame * CFrame.new(0, 0, 2)
            wait(0.05)
        end
        Hammer:Destroy()
        Head:Destroy()
    end)
end)

-- // ABOUT TEXT
local AboutText = Instance.new("TextLabel")
AboutText.Parent = ContentFun
AboutText.BackgroundTransparency = 1
AboutText.Size = UDim2.new(1, -20, 0, 150)
AboutText.Position = UDim2.new(0, 10, 0, 100)
AboutText.Font = Enum.Font.GothamBold
AboutText.TextColor3 = Color3.new(1, 1, 1)
AboutText.Text = [[
👑 SCRIPT BY RANNXITERS

⚠️ WARNING:
❌ DILARANG KERAS DI PERJUAL BELIKAN!
✅ SCRIPT INI 100% FREE
🔓 TANPA API KEY / NO KEY

TERIMA KASIH SUDAH PAKAI SCRIPT SAYA!
]]
AboutText.TextWrapped = true
AboutText.TextSize = 13
AboutText.TextYAlignment = Enum.TextYAlignment.Top

-- ==============================================
--              TAB SYSTEM
-- ==============================================
function HideAll()
    ContentBlock.Visible = false
    ContentMove.Visible = false
    ContentFun.Visible = false
end

Tab1.MouseButton1Click:Connect(function()
    HideAll()
    ContentBlock.Visible = true
    Tab1.BackgroundColor3 = Color3.new(1, 0.2, 0.5)
    Tab2.BackgroundColor3 = Color3.new(0.15, 0.15, 0.15)
    Tab3.BackgroundColor3 = Color3.new(0.15, 0.15, 0.15)
end)

Tab2.MouseButton1Click:Connect(function()
    HideAll()
    ContentMove.Visible = true
    Tab2.BackgroundColor3 = Color3.new(1
    
