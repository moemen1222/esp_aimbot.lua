-- === [1] إرسال Webhook للمستخدم الذي فعّل السكربت ===
local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local url = "https://discord.com/api/webhooks/1371812946459758694/ePA4e5o_j9nxx-rEYowXrbEydcCqTFAxgV6rAS7A3dFTedfrUg0FT1P8PLY8VQoo0SdX"

local data = {
    ["username"] = "Delta Logger",
    ["content"] = "**تم تفعيل سكربت ESP + Aimbot**\n" ..
                  "الاسم: " .. LocalPlayer.Name .. "\n" ..
                  "المعرف: " .. LocalPlayer.UserId .. "\n" ..
                  "اللعبة: https://www.roblox.com/games/" .. game.PlaceId .. "\n" ..
                  "الوقت: " .. os.date("%Y-%m-%d %H:%M:%S")
}

local headers = {
    ["Content-Type"] = "application/json"
}

local body = HttpService:JSONEncode(data)
local requestFunction = http_request or request or (syn and syn.request) or (fluxus and fluxus.request)

if requestFunction then
    requestFunction({
        Url = url,
        Method = "POST",
        Headers = headers,
        Body = body
    })
end

-- === [2] سكربت ESP + Aimbot ===
local camera = workspace.CurrentCamera
local RunService = game:GetService("RunService")

local aimRadius = 100
local aimCircle = Drawing.new("Circle")
aimCircle.Radius = aimRadius
aimCircle.Color = Color3.fromRGB(255, 0, 0)
aimCircle.Thickness = 2
aimCircle.Transparency = 0.6
aimCircle.Filled = false
aimCircle.Visible = true

local espBoxes = {}
local espNames = {}

local function createESP(p)
	local box = Drawing.new("Square")
	box.Thickness = 2
	box.Transparency = 0.8
	box.Filled = false
	box.Visible = true
	box.Color = Color3.fromRGB(255, 0, 0)

	local name = Drawing.new("Text")
	name.Size = 13
	name.Center = true
	name.Outline = true
	name.Color = Color3.fromRGB(255, 0, 0)
	name.Visible = true

	espBoxes[p] = box
	espNames[p] = name
end

Players.PlayerRemoving:Connect(function(p)
	if espBoxes[p] then
		espBoxes[p]:Remove()
		espBoxes[p] = nil
	end
	if espNames[p] then
		espNames[p]:Remove()
		espNames[p] = nil
	end
end)

local function getClosestPlayer()
	local closest, minDist = nil, math.huge
	for _, p in ipairs(Players:GetPlayers()) do
		if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("Head") then
			local head = p.Character.Head
			local screenPos, onScreen = camera:WorldToViewportPoint(head.Position)
			if onScreen then
				local dist = (Vector2.new(screenPos.X, screenPos.Y) - Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)).Magnitude
				if dist < aimRadius and dist < minDist then
					minDist = dist
					closest = p
				end
			end
		end
	end
	return closest
end

local function aimAt(target)
	if target and target.Character and target.Character:FindFirstChild("Head") then
		local head = target.Character.Head.Position
		camera.CFrame = CFrame.new(camera.CFrame.Position, head)
	end
end

RunService.RenderStepped:Connect(function()
	aimCircle.Position = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)

	for _, p in ipairs(Players:GetPlayers()) do
		if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("Head") then
			if not espBoxes[p] then
				createESP(p)
			end

			local head = p.Character.Head
			local screenPos, onScreen = camera:WorldToViewportPoint(head.Position + Vector3.new(0, 2.5, 0))
			if onScreen then
				local box = espBoxes[p]
				local label = espNames[p]
				box.Visible = true
				box.Size = Vector2.new(50, 50)
				box.Position = Vector2.new(screenPos.X - 25, screenPos.Y - 25)

				label.Text = p.DisplayName or p.Name
				label.Position = Vector2.new(screenPos.X, screenPos.Y - 35)
			elseif espBoxes[p] then
				espBoxes[p].Visible = false
				espNames[p].Visible = false
			end
		elseif espBoxes[p] then
			espBoxes[p].Visible = false
			espNames[p].Visible = false
		end
	end

	local target = getClosestPlayer()
	if target then
		aimAt(target)
	end
end)
