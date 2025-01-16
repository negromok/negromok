-- Decompiler will be improved soon!
-- Decompiled with Konstant V2.1, a fast Luau decompiler made in Luau by plusgiant5 (https://discord.gg/wyButjTMhM)
-- Decompiled on 2025-01-16 11:39:28
-- Luau version 6, Types version 3
-- Time taken: 0.002794 seconds

local LocalPlayer_upvr = game.Players.LocalPlayer
repeat
	wait()
until LocalPlayer_upvr.Character
local Humanoid_upvr = LocalPlayer_upvr.Character:WaitForChild("Humanoid")
local Parent = script.Parent
local ReplicatedStorage_upvr = game:GetService("ReplicatedStorage")
local Punch = ReplicatedStorage_upvr:WaitForChild("gameAnims"):WaitForChild("Tools"):WaitForChild("Punch")
local any_LoadAnimation_result1_upvr = Humanoid_upvr:LoadAnimation(Punch:WaitForChild("idle"))
local tbl_3_upvr = {}
for _, v in pairs(Punch:WaitForChild("attacks"):GetChildren()) do
	if v ~= nil and v:IsA("Animation") then
		table.insert(tbl_3_upvr, 1, Humanoid_upvr:LoadAnimation(v))
	end
end
local var19_upvw
local function chooseAttackAnim_upvr() -- Line 51, Named "chooseAttackAnim"
	--[[ Upvalues[5]:
		[1]: LocalPlayer_upvr (readonly)
		[2]: ReplicatedStorage_upvr (readonly)
		[3]: Humanoid_upvr (readonly)
		[4]: var19_upvw (read and write)
		[5]: tbl_3_upvr (readonly)
	]]
	local equippedAttackAnim = LocalPlayer_upvr:FindFirstChild("equippedAttackAnim")
	local var37
	if equippedAttackAnim ~= nil and equippedAttackAnim ~= "" then
		local attackMoveAnims = ReplicatedStorage_upvr:FindFirstChild("attackMoveAnims")
		if attackMoveAnims ~= nil then
			local SOME = attackMoveAnims:FindFirstChild(equippedAttackAnim.Value)
			if SOME ~= nil then
				var37 = Humanoid_upvr:LoadAnimation(SOME)
			end
		end
	end
	if var37 == nil then
		if var19_upvw ~= nil then
			local tbl_2 = {}
			for i_3, v_3 in pairs(tbl_3_upvr) do
				if v_3 ~= nil and v_3 ~= var19_upvw then
					table.insert(tbl_2, 1, v_3)
				end
			end
			var37 = tbl_2[math.random(1, #tbl_2)]
		else
			i_3 = tbl_3_upvr
			var37 = tbl_3_upvr[math.random(1, #i_3)]
		end
	end
	var19_upvw = var37
	return var37
end
local var44_upvw = 0
local attackTime_upvr = Parent:WaitForChild("attackTime")
local muscleEvent_upvr = LocalPlayer_upvr:WaitForChild("muscleEvent")
Parent.Equipped:Connect(function() -- Line 37, Named "toolEquipped"
	--[[ Upvalues[1]:
		[1]: any_LoadAnimation_result1_upvr (readonly)
	]]
	any_LoadAnimation_result1_upvr:Play()
end)
Parent.Unequipped:Connect(function() -- Line 41, Named "toolUnequipped"
	--[[ Upvalues[2]:
		[1]: any_LoadAnimation_result1_upvr (readonly)
		[2]: tbl_3_upvr (readonly)
	]]
	any_LoadAnimation_result1_upvr:Stop()
	for _, v_2 in pairs(tbl_3_upvr) do
		v_2:Stop()
	end
end)
Parent.Activated:Connect(function() -- Line 98, Named "toolActivated"
	--[[ Upvalues[5]:
		[1]: Humanoid_upvr (readonly)
		[2]: var44_upvw (read and write)
		[3]: attackTime_upvr (readonly)
		[4]: chooseAttackAnim_upvr (readonly)
		[5]: muscleEvent_upvr (readonly)
	]]
	if 0 < Humanoid_upvr.Health and attackTime_upvr.Value <= tick() - var44_upvw then
		var44_upvw = tick()
		local chooseAttackAnim_result1 = chooseAttackAnim_upvr()
		if chooseAttackAnim_result1 ~= nil then
			chooseAttackAnim_result1:Play()
		end
		local var48 = "rightHand"
		if chooseAttackAnim_result1.Animation.Name == "rightAttack" then
			var48 = "rightHand"
		elseif chooseAttackAnim_result1.Animation.Name == "leftAttack" then
			var48 = "leftHand"
		end
		muscleEvent_upvr:FireServer("punch", var48)
	end
end)
