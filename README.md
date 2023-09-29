# Project Rain Addon Fix 
## (kinda)
Temporary fix for project rain addons not working(and a way to modify Linoria UI library externally)
Quick Explanation:
Since we dont need much to have Linoria Window and Tabs system works it can usually adds it from anywhere, so this method is basically fork of Linoria that requires you to add previous Linoria library path
Usage:
```lua
local Outer,TabContainer,TabListLayout,TabArea
--shitty method but this works
for _,v in ipairs(game:GetService("CoreGui").ScreenGui:GetChildren()) do
    if v.ClassName == "Frame" then
        if v.Size == UDim2.fromOffset(550, 600) then Outer = v
        elseif v.Size == UDim2.new(1, -16, 1, -38) v.ClassName == "Frame" then TabContainer = v
        elseif v.Size == UDim2.new(1, -16, 0, 21) then TabArea = v
        elseif v.ClassName == "UIListLayout" then TabListLayout = v
        end
    end
end
if (TabListLayout ~= nil and Outer ~= nil and TabArea ~= nil and TabContainer ~= nil) then
	local Library = loadstring(game:HttpGet("https://github.com/ikottiz/projectRainAddons/blob/main/linoria_fork"))() --loadfile("linoria_fork.lua")()
	Library.TabContainer = TabContainer
	Library.Outer = Outer
	Library.TabListLayout = TabListLayout
	Library.TabArea = TabArea
        local Tabs = {
		ESP = Window:AddTab('Addons Test'),
	}
	local Groupbox = ESP:AddRightGroupbox("Test!")
	Groupbox:AddButton({
		Text = "Test!",
		Func = function()
			Library:Notify('You clicked a button!')
		end
	end})
end
```
