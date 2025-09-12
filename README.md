--// Eclipse Hub (Fancy Rayfield + Unique Icons)

local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()
local LocalPlayer = game.Players.LocalPlayer

--// Blur effect
local Blur = Instance.new("BlurEffect")
Blur.Size = 0
Blur.Parent = game.Lighting
task.spawn(function()
    for i = 1, 20 do
        Blur.Size = i
        task.wait(0.03)
    end
end)

--// Typing Effect
local function Typewrite(text, speed)
    local result = ""
    for i = 1, #text do
        result = result .. string.sub(text, i, i)
        task.wait(speed or 0.05)
    end
    return result
end

--// Window
local Window = Rayfield:CreateWindow({
    Name = Typewrite("Eclipse Hub", 0.05),
    LoadingTitle = Typewrite("Loading Eclipse Hub...", 0.05),
    LoadingSubtitle = Typewrite("Mobile Edition", 0.05),
    ConfigurationSaving = {Enabled = true, FolderName = "EclipseHubConfig", FileName = "EclipseHub"},
    KeySystem = false
})

-- Remove blur after load
task.delay(3, function()
    for i = 20, 0, -1 do
        Blur.Size = i
        task.wait(0.03)
    end
    Blur:Destroy()
end)

--// Local-Only Fancy Rank Tag for Executor
local function createRankTag()
    local rankName = (LocalPlayer.Name == "Yoolacjo") and "Owner" or "Eclipse User"
    local rankColor = (rankName == "Owner") and Color3.fromRGB(128,0,128) or Color3.fromRGB(255,255,255)

    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Head") then
        if LocalPlayer.Character.Head:FindFirstChild("RankTag") then
            LocalPlayer.Character.Head.RankTag:Destroy()
        end

        local billboard = Instance.new("BillboardGui")
        billboard.Name = "RankTag"
        billboard.Size = UDim2.new(0, 180, 0, 60)
        billboard.StudsOffset = Vector3.new(0, 2.5, 0)
        billboard.AlwaysOnTop = true
        billboard.Parent = LocalPlayer.Character.Head

        local label = Instance.new("TextLabel")
        label.Size = UDim2.new(1,0,1,0)
        label.BackgroundTransparency = 1
        label.Text = rankName
        label.TextColor3 = rankColor
        label.TextScaled = true
        label.Font = Enum.Font.GothamBold
        label.Parent = billboard
    end
end

-- Apply the rank tag immediately and on respawn
if LocalPlayer.Character then
    createRankTag()
end
LocalPlayer.CharacterAdded:Connect(function()
    task.wait(0.5)
    createRankTag()
end)

--// Commands Tab (⚙️)
local CommandsTab = Window:CreateTab("Commands", 6031280882) -- Gear icon

CommandsTab:CreateSlider({
    Name = "WalkSpeed",
    Range = {16, 100},
    Increment = 1,
    CurrentValue = 16,
    Callback = function(v)
        local h = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if h then h.WalkSpeed = v end
    end
})

CommandsTab:CreateSlider({
    Name = "JumpPower",
    Range = {50, 150},
    Increment = 5,
    CurrentValue = 50,
    Callback = function(v)
        local h = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if h then h.JumpPower = v end
    end
})

CommandsTab:CreateButton({Name="FastOOF", Callback=function()
    local h = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.Health=0 end
end})

CommandsTab:CreateButton({Name="Super Saiyan Teleport Tool", Callback=function()
    local Tool = Instance.new("Tool")
    Tool.Name = "Super Saiyan Teleport"
    Tool.RequiresHandle = false
    Tool.CanBeDropped = false
    Tool.Parent = LocalPlayer.Backpack
    local mouse = LocalPlayer:GetMouse()
    Tool.Activated:Connect(function()
        local hrp = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if hrp and mouse.Hit then hrp.CFrame = CFrame.new(mouse.Hit.p + Vector3.new(0,3,0)) end
    end)
end})

-- REMOVED Sword Reach (broken)

CommandsTab:CreateButton({Name="Emotes GUI", Callback=function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/TrixAde/Proxima-Hub/main/UniversalDance-AnimGui.lua"))()
end})

CommandsTab:CreateButton({Name="Fuck", Callback=function()
    loadstring(game:HttpGet('https://raw.githubusercontent.com/4gh9/Bang-Script-Gui/main/bang%20gui.lua'))()
end})

--// Steal a Brainrot Tab (🧠)
local BrainrotTab = Window:CreateTab("Steal a Brainrot", 6034509994) -- 🧠 icon ID

BrainrotTab:CreateButton({
    Name = "Steal a Brainrot",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/SOON-Steal-a-Brainrot-chills-47933"))()
    end
})

--// Grow a Garden Tab (🌱)
local GardenTab = Window:CreateTab("Grow a Garden", 6026568198) -- 🌱 icon ID

-- First garden script (original)
GardenTab:CreateButton({
    Name = "Grow a Garden (Original)",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Update-Grow-A-Garden-42076"))()
    end
})

-- Second garden script (ZapHub)
GardenTab:CreateButton({
    Name = "Grow a Garden (ZapHub)",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Grow-a-Garden-The-Best-GAG-Script-ZapHub-Keyless-51562"))()
    end
})

--// MM2 Tab (🔪)
Window:CreateTab("MM2", 6034390595):CreateButton({
    Name = "Load MM2",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Vexon-Hub-Keyless-51499"))()
    end
})

--// Underground War Tab (⚔️)
Window:CreateTab("Underground War 2.0⚔️", 6034989554):CreateButton({
    Name = "Load Underground War 2.0",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Underground-War-2.0-NUKE-OP-Panel-35617"))()
    end
})

--// Ink Game Tab (🖌️)
Window:CreateTab("Ink Game", 6034390217):CreateButton({
    Name = "Load Ink Game",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/VapeVoidware/VW-Add/main/inkgame.lua"))()
    end
})

--// 99 Nights Tab (🌙)
Window:CreateTab("99 Nights", 6034684930):CreateButton({
    Name = "Load 99 Nights",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/VapeVoidware/VW-Add/main/nightsintheforest.lua"))()
    end
})

--// Discord Tab (💬)
local DiscordTab = Window:CreateTab("Discord", 6035047409)
DiscordTab:CreateParagraph({
    Title = "Join Discord",
    Content = "https://discord.gg/kwDHzJhp"
})
DiscordTab:CreateButton({
    Name = "Copy Invite",
    Callback = function()
        setclipboard("https://discord.gg/kwDHzJhp")
    end
})

Rayfield:LoadConfiguration()
