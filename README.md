# Project Rain Addon Fix 
## (kinda)
Temporary fix for project rain addons not working(and a way to modify Linoria UI library externally)
Quick Explanation:
Since we dont need much to have Linoria Window and Tabs system works it can usually adds it from anywhere, so this method is basically fork of Linoria that requires you to add previous Linoria library path
Usage:
```lua
local Outer,TabContainer,TabListLayout,TabArea
--shitty method but this works
for _,v in ipairs(game:GetService("CoreGui").ScreenGui:GetDescendants()) do
    if v.ClassName == "Frame" then
        if v.Size == UDim2.fromOffset(550, 600) then Outer = v
        elseif v.Size == UDim2.new(1, -16, 1, -38) then TabContainer = v
        elseif v.Size == UDim2.new(1, -16, 0, 21) then TabArea = v
        end
	elseif v.ClassName == "UIListLayout" and v.Parent.Size == UDim2.new(1, -16, 0, 21) then TabListLayout = v
    end
end
if TabListLayout ~= nil and Outer ~= nil and TabArea ~= nil and TabContainer ~= nil then
	local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/ikottiz/projectRainAddons/main/linoria_fork"))() --loadfile("linoria_fork.lua")()
	Library.TabContainer = TabContainer
	Library.Outer = Outer
	Library.TabListLayout = TabListLayout
	Library.TabArea = TabArea
	local Window = Library:UseWindow()
    local Tabs = {
		Addon = Window:AddTab('Addons Test'),
	}
	local Groupbox = Tabs.Addon:AddLeftGroupbox("Test!")
	local MiscGroupbox  = Tabs.Addon:AddRightGroupbox('Misc')
	
	Groupbox:AddButton({
		Text = "Test!",
		Func = function()
			Library:Notify('You clicked a button!')
		end
	})
	MiscGroupbox:AddToggle('VoidWalkerNotifier', {
		Text = 'Void Walker Notifier',
		Default = true,
	})
	local players = game:GetService("Players")
	local localPlayer = players.LocalPlayer
	local miscConnection = players.PlayerAdded:Connect(function(player)
		if Library.Unloaded then
			return
		end
		if not Toggles.VoidWalkerNotifier.Value then
			return
		end
		local character = player.Character or player.CharacterAdded:Wait()
		if character ~= nil then
			local voidEye = v.Backpack:FindFirstChild("Talent:Voideye")
			if voidEye ~= nil then
				Library:Notify(v.Name .. " is a voidwalker!")
			end
		end
	end)
	local function checkVoidWalkers()
		for _,v in ipairs(players:GetPlayers()) do
			if v == localPlayer then
				continue
			end
			if Library.Unloaded then
				return
			end
			if not Toggles.VoidWalkerNotifier.Value then
				return
			end
			local backpack = v.Backpack or v:WaitForChild("Backpack")
			if backpack ~= nil then
				local voidEye = backpack:FindFirstChild("Talent:Voideye")
				if voidEye ~= nil then
					Library:Notify(v.Name .. " is a voidwalker!")
				end
			end
		end
	end
	Toggles.VoidWalkerNotifier:OnChanged(function()
		checkVoidWalkers()
	end)
	Library:GiveSignal(miscConnection)
end
```
