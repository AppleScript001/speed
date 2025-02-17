-- Libraries and Dependencies
local Library = loadstring(game:HttpGetAsync("https://github.com/ActualMasterOogway/Fluent-Renewed/releases/latest/download/Fluent.luau"))()
local SaveManager = loadstring(game:HttpGetAsync("https://raw.githubusercontent.com/ActualMasterOogway/Fluent-Renewed/master/Addons/SaveManager.luau"))()
local InterfaceManager = loadstring(game:HttpGetAsync("https://raw.githubusercontent.com/ActualMasterOogway/Fluent-Renewed/master/Addons/InterfaceManager.luau"))()

-- Wait until game is fully loaded
repeat wait() until game:IsLoaded()

Orb = {}
for i,v in pairs(workspace.orbFolder:GetChildren()) do
    table.insert(Orb,v.Name) 
end

-- Window Creation
local Window = Library:CreateWindow{
    Title = "Legends Of Speed |",
    SubTitle = "by AppleScript001",
    TabWidth = 90,
    Size = UDim2.fromOffset(450, 300),
    Resize = false,
    MinSize = Vector2.new(470, 380),
    Acrylic = true,
    Theme = "Vynixu", -- You can change the theme as needed.
    MinimizeKey = Enum.KeyCode.RightControl
}

-- Tabs Setup
local Tabs = {
    OP = Window:CreateTab{
        Title = "Main",
        Icon = "gem"
    },
}

local Paragraph = Tabs.OP:CreateParagraph("orb selected!", {
    Title = "AutoFarm",
    Content = "collect - selected!"
})
local Dropdown = Tabs.OP:AddDropdown("Dropdown", {
    Title = "Select ! World",
    Values = Orb,
    Multi = false,
    Default = 1,
})

Dropdown:OnChanged(function(t)
    CrateWeapon = t
end)
local Toggle2 = Tabs.OP:AddToggle("collect orb", { Title = "Auto Collect", Default = false })
Toggle2:OnChanged(function(t)
    _G.collect = t
    while _G.collect do
        task.wait(0.001)
        for i,v in pairs(workspace.orbFolder[CrateWeapon]:GetChildren()) do
            local args = {
            [1] = "collectOrb",
            [2] = v.Name,
            [3] = CrateWeapon
            }
            
            game:GetService("ReplicatedStorage"):WaitForChild("rEvents"):WaitForChild("orbEvent"):FireServer(unpack(args))
            wait()
            for _,v in pairs(workspace.Hoops:GetDescendants()) do
            if v:IsA("TouchTransmitter") then
            firetouchinterest(game.Players.LocalPlayer.Character.HumanoidRootPart, v.Parent, 0) --0 is touch
            firetouchinterest(game.Players.LocalPlayer.Character.HumanoidRootPart, v.Parent, 1) -- 1 is untouch
            end
            end
        end                
    end
end)
local Toggle2 = Tabs.OP:AddToggle("rebirths", { Title = "Auto Rebirth", Default = false })
Toggle2:OnChanged(function(t)
    _G.rb = t
    while _G.rb do
        task.wait(0.001)
        game:GetService("ReplicatedStorage"):WaitForChild("rEvents"):WaitForChild("rebirthEvent"):FireServer("rebirthRequest")
    end
end)
