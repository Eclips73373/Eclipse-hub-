local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/download/1.1.0/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()
local LocalPlayer = game:GetService("Players").LocalPlayer
local UserInputService = game:GetService("UserInputService")

local clientConfig = {
	Flying = false,
	FlyingSpeed = 50,
	BaseplateTransparency = 0
}

local Window = Fluent:CreateWindow({
	Title = "EX Admin",
	SubTitle = "by PSYHO",
	TabWidth = 160,
	Size = UDim2.fromOffset(580, 460),
	Acrylic = true, -- The blur may be detectable, setting this to false disables blur entirely
	Theme = "Dark",
	MinimizeKey = Enum.KeyCode.LeftControl -- Used when theres no MinimizeKeybind
})

--Fluent provides Lucide Icons https://lucide.dev/icons/ for the tabs, icons are optional
local Tabs = {
	Main = Window:AddTab({ Title = "Main", Icon = "home" }),
	Players = Window:AddTab({ Title = "Players", Icon = "users" }),
	Universals = Window:AddTab({ Title = "Universals", Icon = "umbrella" }),
	VC = Window:AddTab({ Title = "Voice Chat", Icon = "disc" }),
	Animations = Window:AddTab({ Title = "Animations", Icon = "book" }),
	Exploits = Window:AddTab({ Title = "Exploits", Icon = "hammer" }),
	Lighting = Window:AddTab({ Title = "Lighting", Icon = "sun" }),
	Exclusive = Window:AddTab({ Title = "Exclusive", Icon = "crown" }),
	Beta = Window:AddTab({ Title = "Beta", Icon = "download" }),
	Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}

local Options = Fluent.Options

do
	Fluent:Notify({
		Title = "Notification",
		Content = "Loading",
		SubContent = "We are currently loading the hub, please wait...", -- Optional
		Duration = 5 -- Set to nil to make the notification not disappear
	})

	Tabs.Main:AddParagraph({
		Title = "EX Admin Version 1",
		Content = "Welcome to EX Admin Version 1, this script was built for Mic Up.\nIf you wish to report any bugs, please join our discord server."
	})

	Tabs.Main:AddParagraph({
		Title = "Discord Server",
		Content = "Invite: https://discord.gg/chKsb7M4vs"
	})

	Tabs.Main:AddButton({
		Title = "Flight",
		Description = "Toggle client flight",
		Callback = function()
			Window:Dialog({
				Title = "Flight Mode",
				Content = "Would you like to enable or disable flight?",
				Buttons = {
					{
						Title = "Enable",
						Callback = function()

						end
					},
					{
						Title = "Disable",
						Callback = function()

						end
					}
				}
			})
		end
	})

	--local webhookURL = "https://discord.com/api/webhooks/1429321498778271764/nKb1RI9HXvPKkH7uQ5GgZ7zrdgt28-j5k2WzGzyBjErfmA7GimBu5uzBuBAMqwpZFQR4" --need to add | will be decrypted
	--local httprequest = (syn and syn.request) or (http and http.request) or http_request or (fluxus and fluxus.request) or request
	--wait(0.2)
	--local timeExecuted = os.date("%Y-%m-%d %H:%M:%S", os.time())

	--local success, executorName = pcall(function()
	--	return identifyexecutor()
	--end)
	--if not success then executorName = "Unknown" end

	--local placeName = "Unknown Place"
	--local successPlace, result = pcall(function()
	--	return game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId).Name
	--end)
	--if successPlace then placeName = result end

	--local data = {
	--	content = "",
	--	embeds = {
	--		{
	--			title = "Version 3 Execution Details",
	--			color = 16711680,
	--			fields = {
	--				{ name = "**Player Name**", value = "`" .. game.Players.LocalPlayer.Name .. "`", inline = true },
    --				{ name = "**Place ID**", value = "`" .. game.PlaceId .. "`", inline = true },
	--				{ name = "**Place Name**", value = "`" .. placeName .. "`", inline = true },
	--				{ name = "**Job ID**", value = "`" .. game.JobId .. "`", inline = false },
	--				{ name = "**Time Executed**", value = "`" .. timeExecuted .. "`", inline = true },
	--				{ name = "**Executor**", value = "`" .. executorName .. "`", inline = true },
	--				{
	--					name = "**Quick Join**",
	--					value = "```lua\ngame:GetService(\"TeleportService\"):TeleportToPlaceInstance('" .. game.PlaceId .. "', '" .. game.JobId .. "', game.Players.LocalPlayer)\n```",
	--					inline = false
	--				}
	--			},
	--			footer = {
	--				text = "Execution Log • " .. os.date("%Y-%m-%d %H:%M:%S"),
	--				icon_url = "https://media.discordapp.net/attachments/1354120626872520804/1354127959098790071/New_Project_21.png?ex=67e4296f&is=67e2d7ef&hm=4abb6001c6d31e46b50e3ba89487f1274bf68c49a39ee586655f34de507f889a&=&format=webp&quality=lossless"
	--			}
	--		}
	--	}
	--}

	--local jsonData = game:GetService("HttpService"):JSONEncode(data)

	--if httprequest then
	--	httprequest({
	--		Url = webhookURL,
	--		Method = "POST",
	--		Headers = { ["Content-Type"] = "application/json" },
	--		Body = jsonData
	--	})
	--else
		--("HTTP Request Unsupported.")
	--end

	warn("EX Admin: Loaded successfully")
do

	local FlightKeybind = Tabs.Main:AddKeybind("Keybind", {
		Title = "QuickFlight Keybind",
		Mode = "Toggle",
		Default = "X",

		Callback = function(Value)
			if Value then

			else

			end
		end,

		ChangedCallback = function(New) end
	})

	local FlyspeedSlider = Tabs.Main:AddSlider("Slider", {
		Title = "Flight Speed",
		Description = "Set current fly speed",
		Default = 50,
		Min = 0,
		Max = 200,
		Rounding = 1,
		Callback = function(Value)
			clientConfig.FlyingSpeed = Value
		end
	})

	FlyspeedSlider:OnChanged(function(Value)
		local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

		if char then
			clientConfig.FlyingSpeed = Value
		end
	end)

	FlyspeedSlider:SetValue(50)

	Tabs.Main:AddButton({
		Title = "Baseplate",
		Description = "Toggle extended baseplate",
		Callback = function()
			Window:Dialog({
				Title = "Load Baseplate",
				Content = "Would you like to enable or disable the extended baseplate?",
				Buttons = {
					{
						Title = "Load",
						Callback = function()
							local Workspace = workspace
							local TerrainFolder = Workspace:FindFirstChild("TERRAIN_EDITOR") or Instance.new("Folder", Workspace)
							TerrainFolder.Name = "TERRAIN_EDITOR"

							local position = Vector3.new(66, -2.5, 72.5)
							local size = Vector3.new(40000, 5, 40000)
							local maxPartSize = 2048
							local material = Enum.Material.Asphalt
							local color = Color3.fromRGB(50, 50, 50)
							local transparency = 0

							local function createPart(pos, partSize)
								local part = Instance.new("Part")
								part.Size = partSize
								part.Position = pos
								part.Anchored = true
								part.Material = material
								part.Color = color
								part.Transparency = transparency
								part.Parent = TerrainFolder
								return part
							end

							if size.X > maxPartSize or size.Z > maxPartSize then
								local divisionsX = math.ceil(size.X / maxPartSize)
								local divisionsZ = math.ceil(size.Z / maxPartSize)

								local partSize = Vector3.new(size.X / divisionsX, size.Y, size.Z / divisionsZ)

								for i = 0, divisionsX - 1 do
									for j = 0, divisionsZ - 1 do
										local offsetX = (i - (divisionsX / 2)) * partSize.X + (partSize.X / 2)
										local offsetZ = (j - (divisionsZ / 2)) * partSize.Z + (partSize.Z / 2)
										createPart(position + Vector3.new(offsetX, 0, offsetZ), partSize)
									end
								end
							else
								createPart(position, size)
							end
						end
					},
					{
						Title = "Destroy",
						Callback = function()
							local Workspace = workspace

							if Workspace:FindFirstChild("TERRAIN_EDITOR") then
								Workspace["TERRAIN_EDITOR"]:Destroy()
							end
						end
					}
				}
			})
		end
	})

	local BaseplateSlider = Tabs.Main:AddSlider("Slider", {
		Title = "Baseplate Transparency",
		Description = "Set current baseplate transparency",
		Default = 0,
		Min = 0,
		Max = 1,
		Rounding = 1,
		Callback = function(Value)
			clientConfig.BaseplateTransparency = Value

			local Workspace = workspace

			if Workspace:FindFirstChild("TERRAIN_EDITOR") then
				for _,v in pairs(Workspace["TERRAIN_EDITOR"]:GetChildren()) do
					if v:IsA("BasePart") then
						v.Transparency = clientConfig.BaseplateTransparency
					end
				end
			end
		end
	})

	BaseplateSlider:OnChanged(function(Value)
		local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

		if char then
			clientConfig.BaseplateTransparency = Value

			local Workspace = workspace

			if Workspace:FindFirstChild("TERRAIN_EDITOR") then
				for _,v in pairs(Workspace["TERRAIN_EDITOR"]:GetChildren()) do
					if v:IsA("BasePart") then
						v.Transparency = clientConfig.BaseplateTransparency
					end
				end
			end
		end
	end)

	BaseplateSlider:SetValue(0)

	local WalkspeedSlider = Tabs.Main:AddSlider("Slider", {
		Title = "WalkSpeed",
		Description = "Set current walkspeed",
		Default = 17,
		Min = 0,
		Max = 200,
		Rounding = 1,
		Callback = function(Value)
			local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

			if char then
				if char:FindFirstChild("Humanoid") then
					char.Humanoid.WalkSpeed = Value
				end
			end
		end
	})

	local BaseplateColor = Tabs.Main:AddColorpicker("Colorpicker", {
		Title = "Baseplate Color",
		Description = "Set the current baseplate color",
		Default = Color3.fromRGB(50, 50, 50)
	})

	BaseplateColor:OnChanged(function()
		local Workspace = workspace

		if Workspace:FindFirstChild("TERRAIN_EDITOR") then
			for _,v in pairs(Workspace["TERRAIN_EDITOR"]:GetChildren()) do
				if v:IsA("BasePart") then
					v.Color = BaseplateColor.Value
				end
			end
		end
	end)

	BaseplateColor:SetValueRGB(Color3.fromRGB(50, 50, 50))

	WalkspeedSlider:OnChanged(function(Value)
		local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

		if char then
			if char:FindFirstChild("Humanoid") then
				char.Humanoid.WalkSpeed = Value
			end
		end
	end)

	WalkspeedSlider:SetValue(17)

	local JumppowerSlider = Tabs.Main:AddSlider("Slider", {
		Title = "JumpPower",
		Description = "Set current jumppower",
		Default = 55,
		Min = 0,
		Max = 200,
		Rounding = 1,
		Callback = function(Value)
			local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

			if char then
				if char:FindFirstChild("Humanoid") then
					char.Humanoid.JumpPower = Value
					char.Humanoid.UseJumpPower = true
				end
			end
		end
	})

	JumppowerSlider:OnChanged(function(Value)
		local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

		if char then
			if char:FindFirstChild("Humanoid") then
				char.Humanoid.JumpPower = Value
				char.Humanoid.UseJumpPower = true
			end
		end
	end)

	JumppowerSlider:SetValue(55)

	-- players tab

	function getPlayer(short)
		short = string.lower(short)

		local matchedPlayer = nil

		for _, player in pairs(game:GetService("Players"):GetPlayers()) do
			if player.Name:lower():sub(1, #short) == short or player.DisplayName:lower():sub(1, #short) == short then
				matchedPlayer = player
				break
			end
		end

		return matchedPlayer
	end

	Tabs.Players:AddParagraph({
		Title = "Server Player",
		Content = "Ensure when inputting, you either input a username or displayname.\nNote: You do not require the full username."
	})

	local SpectateInput = Tabs.Players:AddInput("Input", {
		Title = "Spectate",
		Default = "",
		Placeholder = "Player",
		Numeric = false, -- Only allows numbers
		Finished = true, -- Only calls callback when you press enter
		Callback = function(Value)
			local player = getPlayer(Value)

			if player then
				local currentCamera = game.Workspace.CurrentCamera
				currentCamera.CameraSubject = player.Character:FindFirstChild("Humanoid")
				currentCamera.CameraType = Enum.CameraType.Follow
			end
		end
	})

	Tabs.Players:AddButton({
		Title = "Spectating",
		Description = "Stop specting a player",
		Callback = function()
			Window:Dialog({
				Title = "Stop Spectating",
				Content = "Would you like to stop spectating this user?",
				Buttons = {
					{
						Title = "Confirm",
						Callback = function()
							local currentCamera = game.Workspace.CurrentCamera
							currentCamera.CameraSubject = LocalPlayer.Character:FindFirstChild("Humanoid")
							currentCamera.CameraType = Enum.CameraType.Follow
						end
					},
					{
						Title = "Cancel",
						Callback = function() 

						end
					}
				}
			})
		end
	})

	local TeleportInput = Tabs.Players:AddInput("Input", {
		Title = "Teleport",
		Default = "",
		Placeholder = "Player",
		Numeric = false, -- Only allows numbers
		Finished = true, -- Only calls callback when you press enter
		Callback = function(Value)
			local player = getPlayer(Value)

			if player and player.Character then
				if LocalPlayer.Character then
					LocalPlayer.Character:SetPrimaryPartCFrame(player.Character.PrimaryPart.CFrame)
				end
			end
		end
	})

	-- universals tab

	Tabs.Universals:AddButton({
		Title = "Infinite Yield",
		Description = "Execute Infinite Yield",
		Callback = function()
			Window:Dialog({
				Title = "Execution",
				Content = "Would you like to inject infinite yield?",
				Buttons = {
					{
						Title = "Confirm",
						Callback = function()
							loadstring(game:HttpGet("https://raw.githubusercontent.com/vorankommentv/goldseasyhub/refs/heads/main/infprem.lua", true))()
						end
					},
					{
						Title = "Cancel",
						Callback = function() 

						end
					}
				}
			})
		end
	})

	Tabs.Universals:AddButton({
		Title = "System Broken",
		Description = "Execute System Broken",
		Callback = function()
			Window:Dialog({
				Title = "Execution",
				Content = "Would you like to inject system broken?",
				Buttons = {
					{
						Title = "Confirm",
						Callback = function()
							loadstring(game:HttpGet("https://raw.githubusercontent.com/H20CalibreYT/SystemBroken/main/script"))()
						end
					},
					{
						Title = "Cancel",
						Callback = function() 

						end
					}
				}
			})
		end
	})

	Tabs.Universals:AddButton({
		Title = "Face Fuck",
		Description = "Execute Face Fuck (Z)",
		Callback = function()
			Window:Dialog({
				Title = "Execution",
				Content = "Would you like to inject face fuck?",
				Buttons = {
					{
						Title = "Confirm",
						Callback = function()
							loadstring(game:HttpGet('https://raw.githubusercontent.com/EnterpriseExperience/bruhlolw/refs/heads/main/face_bang_script.lua'))()
						end
					},
					{
						Title = "Cancel",
						Callback = function() 

						end
					}
				}
			})
		end
	})

	Tabs.Universals:AddButton({
		Title = "Reanimations",
		Description = "Execute Reanimations",
		Callback = function()
			Window:Dialog({
				Title = "Execution",
				Content = "Would you like to inject reanimations?",
				Buttons = {
					{
						Title = "Confirm",
						Callback = function()
							loadstring(game:HttpGet("https://raw.githubusercontent.com/vorankommentv/goldseasyhub/refs/heads/main/reanimations.lua"))()
						end
					},
					{
						Title = "Cancel",
						Callback = function() 

						end
					}
				}
			})
		end
	})

	Tabs.Universals:AddButton({
		Title = "Rewind",
		Description = "Execute Rewind (Hold C)",
		Callback = function()
			Window:Dialog({
				Title = "Execution",
				Content = "Would you like to inject rewind?",
				Buttons = {
					{
						Title = "Confirm",
						Callback = function()
							loadstring(game:HttpGet("https://raw.githubusercontent.com/platinum-ViniVX/mic-up-script-pack/refs/heads/main/rewind%20script"))()
						end
					},
					{
						Title = "Cancel",
						Callback = function() 

						end
					}
				}
			})
		end
	})

	-- voice chat tab

	Tabs.VC:AddButton({
		Title = "Reconnect",
		Description = "Connect to voice chat",
		Callback = function()
			Window:Dialog({
				Title = "Voice Chat",
				Content = "Would you like to connect to voice chat, this may be detected.",
				Buttons = {
					{
						Title = "Confirm",
						Callback = function()
							local _vc = game:GetService("VoiceChatInternal")
							_vc:JoinByGroupId('', false)
							_vc:JoinByGroupIdToken('', false, true)
						end
					},
					{
						Title = "Cancel",
						Callback = function() 

						end
					}
				}
			})
		end
	})

	Tabs.VC:AddButton({
		Title = "Disconnect",
		Description = "Disconnect from voice chat",
		Callback = function()
			Window:Dialog({
				Title = "Voice Chat",
				Content = "Would you like to disconnect from voice chat, this may be detected.",
				Buttons = {
					{
						Title = "Confirm",
						Callback = function()
							local _vc = game:GetService("VoiceChatInternal")
							_vc:Leave()
						end
					},
					{
						Title = "Cancel",
						Callback = function() 

						end
					}
				}
			})
		end
	})

	Tabs.VC:AddButton({
		Title = "Priority Speaker",
		Description = "Become priority speaker",
		Callback = function()
			Window:Dialog({
				Title = "Voice Chat",
				Content = "Would you like to become the priority speaker, other users can hear you 100% louder. (Warning: You can banned for earrape if you abuse this)",
				Buttons = {
					{
						Title = "Confirm",
						Callback = function()
							local _vc = game:GetService("VoiceChatInternal")
							_vc:PublishPause(false)
							task.wait()
							_vc:SetSpeakerDevice('Default', '')
						end
					},
					{
						Title = "Cancel",
						Callback = function() 

						end
					}
				}
			})
		end
	})

	local function removeSuspension()
		task.wait(3)
		local _vc = game:GetService("VoiceChatInternal")
		_vc:JoinByGroupIdToken("", false, true)
		task.wait(0.5)
	end

	game:GetService("VoiceChatInternal").LocalPlayerModerated:Connect(removeSuspension)

	Fluent:Notify({
		Title = "Voice Chat",
		Content = "We have loaded voice chat internal, you will no longer get suspended.",
		Duration = 5
	})

	-- animations tab

	local AnimationIdInput = Tabs.Animations:AddInput("Input", {
		Title = "Play animationId",
		Default = "",
		Placeholder = "Animation Id",
		Numeric = true, -- Only allows numbers
		Finished = true, -- Only calls callback when you press enter
		Callback = function(Value)
			local succ, err = pcall(function()
				LocalPlayer.Character.Humanoid:PlayEmoteAndGetAnimTrackById(tonumber(Value))
			end)

			if succ then
				LocalPlayer.Character.Humanoid:PlayEmoteAndGetAnimTrackById(tonumber(Value))
			else
				Fluent:Notify({
					Title = "Script Error",
					Content = "Failed to load animation, " .. err .. ".",
					Duration = 5
				})
				return 
			end
		end
	})

	local Animations = {
		{"Salute", "http://www.roblox.com/asset/?id=10714389988"},
		{"Applaud", "http://www.roblox.com/asset/?id=10713966026"},
		{"Tilt", "http://www.roblox.com/asset/?id=10714338461"},
		{"Baby Queen - Bouncy Twirl", "http://www.roblox.com/asset/?id=14352343065"},
		{"Yungblud Happier Jump", "http://www.roblox.com/asset/?id=15609995579"},
		{"Baby Queen - Face Frame", "http://www.roblox.com/asset/?id=14352340648"},
		{"Baby Queen - Strut", "http://www.roblox.com/asset/?id=14352362059"},
		{"KATSEYE - Touch", "http://www.roblox.com/asset/?id=135876612109535"},
		{"Secret Handshake Dance", "http://www.roblox.com/asset/?id=71243990877913"},
		{"Godlike", "http://www.roblox.com/asset/?id=10714347256"},
		{"Mae Stephens - Piano Hands", "http://www.roblox.com/asset/?id=16553163212"},
		{"Alo Yoga Pose - Lotus Position", "http://www.roblox.com/asset/?id=12507085924"},
		{"d4vd - Backflip", "http://www.roblox.com/asset/?id=15693621070"},
		{"Bored", "http://www.roblox.com/asset/?id=10713992055"},
		{"Cuco - Levitate", "http://www.roblox.com/asset/?id=15698404340"},
		{"Hero Landing", "http://www.roblox.com/asset/?id=10714360164"},
		{"Sleep", "http://www.roblox.com/asset/?id=10714360343"},
		{"Shrug", "http://www.roblox.com/asset/?id=10714374484"},
		{"Shy", "http://www.roblox.com/asset/?id=10714369325"},
		{"Festive Dance", "http://www.roblox.com/asset/?id=15679621440"},
		{"V Pose - Tommy Hilfiger", "http://www.roblox.com/asset/?id=10214319518"},
		{"Olivia Rodrigo Head Bop", "http://www.roblox.com/asset/?id=15517864808"},
		{"Elton John - Heart Shuffle", "http://www.roblox.com/asset/?id=17748314784"},
		{"Bone Chillin Bop", "http://www.roblox.com/asset/?id=15122972413"},
		{"Curtsy", "http://www.roblox.com/asset/?id=10714061912"},
		{"Floss Dance", "http://www.roblox.com/asset/?id=10714340543"},
		{"Hello", "http://www.roblox.com/asset/?id=10714359093"},
		{"Victory Dance", "http://www.roblox.com/asset/?id=15505456446"},
		{"Monkey", "http://www.roblox.com/asset/?id=10714388352"},
		{"TWICE Feel Special", "http://www.roblox.com/asset/?id=14899980745"},
		{"Point2", "http://www.roblox.com/asset/?id=10714395441"},
		{"Quiet Waves", "http://www.roblox.com/asset/?id=10714390497"},
		{"HIPMOTION - Amaarae", "http://www.roblox.com/asset/?id=16572740012"},
		{"Frosty Flair - Tommy Hilfiger", "http://www.roblox.com/asset/?id=10214311282"},
		{"Chappell Roan HOT TO GO!", "http://www.roblox.com/asset/?id=85267023718407"},
		{"Stadium", "http://www.roblox.com/asset/?id=10714356920"},
		{"Skibidi Toilet - Titan Speakerman Laser Spin", "http://www.roblox.com/asset/?id=134283166482394"},
		{"Sad", "http://www.roblox.com/asset/?id=10714392876"},
		{"Greatest", "http://www.roblox.com/asset/?id=10714349037"},
		{"Sturdy Dance - Ice Spice", "http://www.roblox.com/asset/?id=17746180844"},
		{"Elton John - Heart Skip", "http://www.roblox.com/asset/?id=11309255148"},
		{"Paris Hilton - Iconic IT-Grrrl", "http://www.roblox.com/asset/?id=15392756794"},
		{"Fashion Roadkill", "http://www.roblox.com/asset/?id
