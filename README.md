local Rayfield = loadstring(game:HttpGet('https://raw.githubusercontent.com/shlexware/Rayfield/main/source'))()
local Window = Rayfield:CreateWindow({
   Name = "Phantom Forces",
   LoadingTitle = "Bird hub",
   LoadingSubtitle = "by Birdhub",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil, -- Create a custom folder for your hub/game
      FileName = "Big Hub"
   },
   Discord = {
      Enabled = false,
      Invite = "noinvitelink", -- The Discord invite code, do not include discord.gg/. E.g. discord.gg/ABCD would be ABCD.
      RememberJoins = true -- Set this to false to make them join the discord every time they load it up
   },
   KeySystem = true, -- Set this to true to use our key system
   KeySettings = {
      Title = "Phantom Forces",
      Subtitle = "Key System",
      Note = "Trem only)",
      FileName = "SiriusKey",
      SaveKey = true,
      GrabKeyFromSite = false, -- If this is true, set Key below to the RAW site you would like Rayfield to get the key from
      Key = "tremontop"
   }
})

local MainTab = Window:CreateTab("Tab Example", 4483362458) -- Title, Image
local Section = MainTab:CreateSection("Section Example")

local Button = MainTab:CreateButton({
   Name = "Aimlock leigt",
   Callback = function()
   getgenv().S = {
    Legit = true,
    FOV = 250,
    WallCheck = true,
    Smooth = 0.1
}
for i,v in pairs(game:GetChildren()) do
    getgenv()[v.ClassName] = v 
end 
local Player = Players.LocalPlayer
local Mouse = Player:GetMouse()
local Shoot = false

function NotObstructing(i, v)
    if S.WallCheck then
        c = Workspace.CurrentCamera.CFrame.Position
        a = Ray.new(c, i - c)
        f = Workspace:FindPartOnRayWithIgnoreList(a, v)
        return f == nil
    else
        return true
    end
end

local GetClosestPlayerToCurser = function()
    local Target = nil 
    local MaxDistance = math.huge
    for i,v in pairs(Workspace.Players:GetChildren()) do
        if v.Name ~= Player.TeamColor.Name then
            for i,v in pairs(v:GetChildren()) do
                if v:IsA("Model") then
                    local OnPoint, OnScreen = Workspace.CurrentCamera:WorldToViewportPoint(v:GetModelCFrame().Position)
                    if OnScreen and NotObstructing(v:GetModelCFrame().Position, {Player.Character, v}) then
                    local Mag = (Vector2.new(Mouse.X, Mouse.Y) - Vector2.new(OnPoint.X, OnPoint.Y)).Magnitude
                        if Mag < MaxDistance then
                            Target = v
                            MaxDistance = Mag 
                        end 
                    end 
                end 
            end 
        end 
    end 
    return Target 
end 

local Return = function(A)
    return Workspace.CurrentCamera:WorldToScreenPoint(A)
end

UserInputService.InputBegan:Connect(
    function(v)
        if v.UserInputType == Enum.UserInputType.MouseButton2 then
            Shoot = true
        end
    end
)

UserInputService.InputEnded:Connect(
    function(v)
        if v.UserInputType == Enum.UserInputType.MouseButton2 then
            Shoot = false
        end
    end
)

RunService.Stepped:Connect(function()
    pcall(function()
        if not S.Legit or not Shoot then return end
        local SexPosition = Return(GetClosestPlayerToCurser():GetModelCFrame().Position)
        local MousePosition = Return(Mouse.Hit.Position)
        local OldX, OldY = (SexPosition.X - MousePosition.X), (SexPosition.Y - MousePosition.Y) 
        mousemoverel(OldX * S.Smooth, OldY * S.Smooth)
    end)
end)
   end,
})

local Button = MainTab:CreateButton({
   Name = "ESP",
   Callback = function()
   local library = loadstring(game:HttpGet(('https://raw.githubusercontent.com/bloodball/-back-ups-for-libs/main/wall%20v3')))()
local ESP = loadstring(game:HttpGet("https://raw.githubusercontent.com/zekgt/lua/main/phantom%20forces/libraries/ESP.lua"))()
ESP.Boxes = false
ESP.Names = false
local window = library:CreateWindow("Phantom Forces")
local folder1 = window:CreateFolder("ESP")
local folder2 = window:CreateFolder("Misc")
local Ghosts = game.Workspace.Players['Bright orange']
local Phantoms = game.Workspace.Players['Bright blue']
ESP:AddObjectListener(Ghosts, {
  Recursive = true,
  Type = "Model",
  CustomName = " ",
  Color = Color3.fromRGB(255,165,0),
      Validator = function(obj)
      return obj:FindFirstChild("Torso")
  end,
  IsEnabled = "GhostsESP"
})
ESP:AddObjectListener(Phantoms, {
  Recursive = true,
  Type = "Model",
  CustomName = " ",
  Color = Color3.fromRGB(0,0,233),
      Validator = function(obj)
      return obj:FindFirstChild("Torso")
  end,
  IsEnabled = "PhantomsESP"
})
ESP.GhostsESP = false
ESP.PhantomsESP = false 
folder1:Toggle("Enabled", function(v)
	if v then
		ESP:Toggle(true)
	else
		ESP:Toggle(false)
	end
end)
folder1:Toggle("Ghosts", function(v) -- so fucking sorry for adding this because i couldn't figure out teamcheck
    ESP.GhostsESP = v
end)
folder1:Toggle("Phantoms", function(v) -- so fucking sorry for adding this because i couldn't figure out teamcheck
    ESP.PhantomsESP = v
end)
folder1:Toggle("Boxes", function(v)
	if v then
		ESP.Boxes = true
		else
		ESP.Boxes = false
	end
end)
folder1:Toggle("Tracers", function(v)
	if v then
		ESP.Tracers = true
		else
			ESP.Tracers = false
	end
end)
folder1:Toggle("Distance", function(v)
if v then
	ESP.Names = true
	else
		ESP.Names = false
	end
end)
folder2:Button("Rejoin Server", function()
    local ts = game:GetService("TeleportService")
    local p = game:GetService("Players").LocalPlayer
    ts:Teleport(game.PlaceId, p)
end)
local creditsTable = {
    "UI: Aika",
    "ESP: Kiriot22",
    "Script: ZekGT",
}
folder2:Dropdown("Credits", creditsTable, true, function()
	warn""
end)
folder2:Slider("FPS Capacity",{
    min = 10;
    max = 300;
    precise = false;
    }, function(v)
    setfpscap(v)
end)
print("ESP Loaded")
   end,
})
