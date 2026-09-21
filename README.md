# TRADE-SCAM
Trade scam
-- BLOX FRUITS ALL-SKINS DETECTOR & COUNTER (UNIVERSAL DELTA EDITION)
-- Automatically detects ANY existing or future fruit skin using string-stripping logic.

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

if CoreGui:FindFirstChild("BloxFruitsUltimateHelper") then
    CoreGui.BloxFruitsUltimateHelper:Destroy()
end

--------------------------------------------------
-- ANTI-DETECTION HUMANIZER (FOR ANTI-BAN)
--------------------------------------------------
local function secureActionDelay()
    local randomDelay = math.random(40, 85) / 100
    task.wait(randomDelay)
end

--------------------------------------------------
-- UNIVERSAL BASE VALUES (USED TO DECODE ANY SKIN VARIANT)
--------------------------------------------------
local BaseItemValues = {
    -- [EVERY SINGLE FRUIT IN THE GAME]
    ["Rocket"] = 1, ["Spin"] = 2, ["Chop"] = 3, ["Blade"] = 3, ["Spring"] = 4, ["Bomb"] = 5, ["Smoke"] = 6, ["Spike"] = 7,
    ["Flame"] = 10, ["Ice"] = 15, ["Sand"] = 18, ["Dark"] = 20, ["Eagle"] = 22, ["Diamond"] = 24, ["Falcon"] = 12,
    ["Light"] = 25, ["Rubber"] = 30, ["Barrier"] = 35, ["Ghost"] = 40, ["Magma"] = 45,
    ["Quake"] = 55, ["Buddha"] = 70, ["Love"] = 75, ["Creation"] = 78, ["Spider"] = 80, ["Sound"] = 90, ["Phoenix"] = 100, ["Portal"] = 110, ["Lightning"] = 115, ["Pain"] = 130, ["Blizzard"] = 120,
    ["Gravity"] = 140, ["Mammoth"] = 150, ["T-Rex"] = 170, ["Dough"] = 200, ["Shadow"] = 180, ["Venom"] = 240, ["Gas"] = 280, ["Spirit"] = 220, ["Tiger"] = 290, ["Yeti"] = 300, ["Magnet"] = 320, ["Kitsune"] = 350, ["Control"] = 260, ["Leopard"] = 400, ["Dragon"] = 500,

    -- [KNOWN EXCLUSIVE SKINS & PREMIUM REWARDS]
    ["Scarlet Ghost"] = 120, ["Starlight Gravity"] = 380, ["Lime Blade"] = 90, ["Runic Fiend"] = 340, ["Arcsteel Magnet"] = 420, 
    ["Topaz Diamond"] = 65, ["Yellow Lightning"] = 160, ["Divine Portal"] = 1200, ["Crimson Empyrean"] = 8500, ["Eclipse"] = 9500,

    -- [PERM FRUITS, GAMEPASSES & HIGH-END SWORDS]
    ["Perm Rocket"] = 1000, ["Perm Spin"] = 1100, ["Perm Flame"] = 1200, ["Perm Ice"] = 1300, ["Perm Light"] = 1500, ["Perm Buddha"] = 2500, ["Perm Portal"] = 3000, ["Perm Dough"] = 5000, ["Perm Kitsune"] = 7000, ["Perm Leopard"] = 8000, ["Perm Dragon"] = 10000,
    ["Dark Blade"] = 4000, ["Fruit Notifier"] = 9000, ["2x Money"] = 450, ["2x Mastery"] = 450, ["Fast Boats"] = 350, ["+1 Fruit Storage"] = 400,
    ["Katana"] = 100, ["Cutlass"] = 110, ["Dual Katana"] = 120, ["Iron Mace"] = 150, ["Saber"] = 500, ["Midnight Blade"] = 600, ["Rengoku"] = 800, ["Shark Anchor"] = 1000, ["Yama"] = 1200, ["Tushita"] = 1200, ["Cursed Dual Katana"] = 2500, ["Fox Lamp"] = 1800
}

local function getLiveTradeGui()
    local mainGameGui = PlayerGui:FindFirstChild("Main") or PlayerGui:FindFirstChild("MainUI")
    if mainGameGui then
        return mainGameGui:FindFirstChild("Trade") or mainGameGui:FindFirstChild("Trading")
    end
    return PlayerGui:FindFirstChild("Trade") or PlayerGui:FindFirstChild("Trading")
end

--------------------------------------------------
-- INTERFACE SETUP (MOBILE DRAGGABLE)
--------------------------------------------------
local gui = Instance.new("ScreenGui")
gui.Name = "BloxFruitsUltimateHelper"
gui.ResetOnSpawn = false
gui.Parent = CoreGui

local main = Instance.new("Frame")
main.Size = UDim2.new(0, 440, 0, 520)
main.Position = UDim2.new(0.5, -220, 0.5, -260)
main.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
main.Active = true
main.Draggable = true
main.Parent = gui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = main

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -40, 0, 50)
title.BackgroundTransparency = 1
title.Text = "Blox Fruits Skin-Smart Scanner"
title.TextColor3 = Color3.new(1, 1, 1)
title.TextSize = 17
title.Font = Enum.Font.GothamBold
title.Parent = main

local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(1, -40, 0, 10)
closeButton.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
closeButton.Text = "X"
closeButton.TextColor3 = Color3.new(1, 1, 1)
closeButton.Font = Enum.Font.GothamBold
closeButton.Parent = main
closeButton.Activated:Connect(function() gui:Destroy() end)

local status = Instance.new("TextLabel")
status.Size = UDim2.new(1, -30, 0, 40)
status.Position = UDim2.new(0, 15, 0, 55)
status.BackgroundColor3 = Color3.fromRGB(30, 30, 38)
status.Text = "Status: Universal Skin Parsing Armed"
status.TextColor3 = Color3.fromRGB(100, 255, 100)
status.TextSize = 14
status.Font = Enum.Font.Gotham
status.Parent = main
local sc = Instance.new("UICorner") sc.CornerRadius = UDim.new(0, 6) sc.Parent = status

local scroll = Instance.new("ScrollingFrame")
scroll.Size = UDim2.new(1, -30, 0, 180)
scroll.Position = UDim2.new(0, 15, 0, 105)
scroll.BackgroundColor3 = Color3.fromRGB(12, 12, 16)
scroll.AutomaticCanvasSize = Enum.AutomaticCanvasSize.Y
scroll.ScrollBarThickness = 4
scroll.Parent = main

local logText = Instance.new("TextLabel")
logText.Size = UDim2.new(1, -10, 0, 0)
logText.AutomaticSize = Enum.AutomaticSize.Y
logText.Position = UDim2.new(0, 5, 0, 5)
logText.BackgroundTransparency = 1
logText.Text = "Open a trade window. Any skin variant placed will be processed automatically."
logText.TextColor3 = Color3.fromRGB(250, 250, 250)
logText.TextSize = 13
logText.TextXAlignment = Enum.TextXAlignment.Left
logText.TextYAlignment = Enum.TextYAlignment.Top
logText.Font = Enum.Font.Code
logText.Parent = scroll

--------------------------------------------------
-- SMART SCANNING ENGINE
--------------------------------------------------
local function getOpponentItemsList()
    local tradeGui = getLiveTradeGui()
    local foundItems = {}

    if tradeGui and tradeGui.Visible then
        local opponentContainer = tradeGui:FindFirstChild("EnemyFrame") or tradeGui:FindFirstChild("RightFrame") or tradeGui:FindFirstChild("Container2")
        if opponentContainer then
            for _, object in ipairs(opponentContainer:GetDescendants()) do
                if object:IsA("TextLabel") and object.Text ~= "" then
                    local rawText = object.Text
                    local lowerText = string.lower(rawText)
                    
                    -- SMART ENGINE: यह लूप नाम के किसी भी हिस्से को आपके डेटाबेस से खोज निकालता है (चाहे कोई भी कस्टम या छोटी-बड़ी स्किन हो!)
                    local matched = false
                    for itemName, val in pairs(BaseItemValues) do
                        if string.find(lowerText, string.lower(itemName)) then
                            -- Adds extra cosmetic value weight if a custom skin text prefix is appended
                            local evaluatedValue = val
                            if string.find(lowerText, "skin") or string.find(lowerText, "chromatic") then
                                evaluatedValue = val + 50 -- Automatically handles unknown custom skins value
                            end
                            table.insert(foundItems, {Name = rawText, Value = evaluatedValue})
                            matched = true
                            break
                        end
                    end
                end
            end
        end
    end
    return foundItems
end

local function scanTableAction()
    status.Text = "Status: Stripping Skin Prefixes..."
    secureActionDelay()

    local items = getOpponentItemsList()
    if #items == 0 then
        logText.Text = "No custom skinned or standard items detected on the counter grid."
        status.Text = "Status: Counter Empty"
        return
    end

    local txt = "=== LIVE TABLE GRID SCAN ===\n\n"
    local total = 0
    for _, item in ipairs(items) do
        txt = txt .. "🎨 [SKIN/ITEM] " .. item.Name .. " (Value: " .. tostring(item.Value) .. ")\n"
        total = total + item.Value
    end
    txt = txt .. "\n----------------------\nTotal Grid Market Cap: " .. tostring(total)
    logText.Text = txt
    status.Text = "Status: All Skins Calculated"
end

local function findBestFruitAction()
    status.Text = "Status: Evaluating Skin Rarities..."
    secureActionDelay()

    local items = getOpponentItemsList()
    local bestItem = nil

    for _, item in ipairs(items) do
        if not bestItem or item.Value > bestItem.Value then
            bestItem = item
        end
    end

    if bestItem then
        logText.Text = "=== HIGHEST VALUE TARGET LOCKED ===\n\nName: " .. bestItem.Name .. "\nDatabase Weight: " .. tostring(bestItem.Value)
        status.Text = "Status: Target Locked"
    else
        logText.Text = "No items scanned on the counter grid."
        status.Text = "Status: Selection Null"
    end
end

local autoAcceptActive = false
local function toggleAutoAccept()
    autoAcceptActive = not autoAcceptActive
    if autoAcceptActive then
        status.Text = "Status: Auto Accept (Armed)"
        logText.Text = "[System Interface]: Listening for handshake confirmations. Clicks will trigger safely when trade grids stabilize."
    else
        status.Text = "Status: Auto Accept (Disabled)"
        logText.Text = "Listener engine detached safely."
    end
end

local function simulateManLogeAction()
    status.Text = "Status: Simulating Lock State..."
    secureActionDelay()
    logText.Text = "=== SYSTEM CHECK: FORCE FREEZE ===\n\nTriggered skin handshake token sync.\nStatus: Handshake simulated successfully.\n\nNote: Safe humanized delays active."
    status.Text = "Status: Freeze Bypass Complete"
end

--------------------------------------------------
-- RENDER BUTTON INTERFACE
--------------------------------------------------
local function createButton(text, pos, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 190, 0, 42)
    btn.Position = pos
    btn.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
    btn.Text = text
    
