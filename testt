--========================================
-- REDSOUL HUB - JUNKIE KEY SYSTEM (FIXED)
--========================================

--// SERVICES
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")
local UserInputService = game:GetService("UserInputService")
local Lighting = game:GetService("Lighting")

--// CLEAN OLD UI
pcall(function()
    if CoreGui:FindFirstChild("RedSoulKeySystem") then
        CoreGui.RedSoulKeySystem:Destroy()
    end
    if Lighting:FindFirstChild("KeySystemBlur") then
        Lighting.KeySystemBlur:Destroy()
    end
end)

--// CONFIG
local CONFIG = {
    API_KEY = "e272e094-2f1e-45ff-a636-55b95ee83ffa",
    SERVICE = "RedsoulHub",
    MAIN_SCRIPT = "https://raw.githubusercontent.com/gamer8410/choco/refs/heads/main/whattt",
    ACCENT = Color3.fromRGB(255, 30, 30),
    BG = Color3.fromRGB(10, 10, 10)
}

local PROVIDERS = {
    ["6h"]  = "Checkpoint-1",
    ["12h"] = "12-hour-key",
    ["24h"] = "24-hour-key",
    ["48h"] = "48-hour-key"
}

--// LOAD JUNKIE SDK
local Junkie = loadstring(game:HttpGet(
    "https://junkie-development.de/sdk/JunkieKeySystem.lua"
))()

--// BLUR
local blur = Instance.new("BlurEffect")
blur.Name = "KeySystemBlur"
blur.Size = 0
blur.Parent = Lighting
TweenService:Create(blur, TweenInfo.new(0.4), {Size = 18}):Play()

--// GUI
local gui = Instance.new("ScreenGui")
gui.Name = "RedSoulKeySystem"
gui.Parent = CoreGui
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local main = Instance.new("Frame", gui)
main.Size = UDim2.fromOffset(350, 300)
main.Position = UDim2.fromScale(0.5, 0.5)
main.AnchorPoint = Vector2.new(0.5, 0.5)
main.BackgroundColor3 = CONFIG.BG
main.BorderSizePixel = 0
Instance.new("UICorner", main).CornerRadius = UDim.new(0, 10)

local stroke = Instance.new("UIStroke", main)
stroke.Color = CONFIG.ACCENT
stroke.Thickness = 1.6

--// DRAG
local dragging, dragStart, startPos
main.InputBegan:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = i.Position
        startPos = main.Position
    end
end)
UserInputService.InputChanged:Connect(function(i)
    if dragging and i.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = i.Position - dragStart
        main.Position = UDim2.new(
            startPos.X.Scale,
            startPos.X.Offset + delta.X,
            startPos.Y.Scale,
            startPos.Y.Offset + delta.Y
        )
    end
end)
UserInputService.InputEnded:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

--// TITLE
local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1, 0, 0, 45)
title.BackgroundTransparency = 1
title.Text = "REDSOUL HUB"
title.Font = Enum.Font.GothamBlack
title.TextSize = 22
title.TextColor3 = CONFIG.ACCENT

--// KEY BOX
local keyBox = Instance.new("TextBox", main)
keyBox.Size = UDim2.new(0.85, 0, 0, 38)
keyBox.Position = UDim2.new(0.5, 0, 0, 60)
keyBox.AnchorPoint = Vector2.new(0.5, 0)
keyBox.PlaceholderText = "Paste key here..."
keyBox.ClearTextOnFocus = false
keyBox.Font = Enum.Font.Gotham
keyBox.TextSize = 14
keyBox.TextColor3 = Color3.new(1,1,1)
keyBox.BackgroundColor3 = Color3.fromRGB(20,20,20)
Instance.new("UICorner", keyBox).CornerRadius = UDim.new(0, 6)

--// BUTTON HOLDER
local holder = Instance.new("Frame", main)
holder.Size = UDim2.new(0.85, 0, 0, 90)
holder.Position = UDim2.new(0.5, 0, 0, 110)
holder.AnchorPoint = Vector2.new(0.5, 0)
holder.BackgroundTransparency = 1

local grid = Instance.new("UIGridLayout", holder)
grid.CellPadding = UDim2.fromOffset(8, 8)
grid.CellSize = UDim2.new(0.5, -4, 0, 38)

--// NOTIFY
local function notify(msg)
    warn("[RedSoul]", msg)
end

--// CLIPBOARD SAFE COPY
local function safeCopy(link)
    local ok = false
    if setclipboard then
        pcall(function()
            setclipboard(link)
            ok = true
        end)
    end
    if not ok then
        warn("[KEY LINK]:", link)
        notify("Clipboard blocked. Link printed in console (F9).")
    end
    return ok
end

--// CREATE GET KEY BUTTON
local function createBtn(text, duration)
    local b = Instance.new("TextButton", holder)
    b.Text = text
    b.Font = Enum.Font.GothamBold
    b.TextSize = 13
    b.TextColor3 = Color3.fromRGB(220,220,220)
    b.BackgroundColor3 = Color3.fromRGB(25,10,10)
    b.AutoButtonColor = false
    Instance.new("UICorner", b).CornerRadius = UDim.new(0, 6)

    b.MouseButton1Click:Connect(function()
        b.Text = "GENERATING..."
        local provider = PROVIDERS[duration]

        local success, link = pcall(function()
            return Junkie.getLink(CONFIG.API_KEY, provider, CONFIG.SERVICE)
        end)

        if success and typeof(link) == "string" and #link > 10 then
            if safeCopy(link) then
                b.Text = "LINK COPIED ✓"
            else
                b.Text = "CHECK CONSOLE"
            end
        else
            b.Text = "TRY AGAIN"
        end

        task.delay(2, function()
            b.Text = text
        end)
    end)
end

createBtn("Get 6h Key", "6h")
createBtn("Get 12h Key", "12h")
createBtn("Get 24h Key", "24h")
createBtn("Get 48h Key", "48h")

--// VERIFY BUTTON
local verify = Instance.new("TextButton", main)
verify.Size = UDim2.new(0.85, 0, 0, 42)
verify.Position = UDim2.new(0.5, 0, 1, -15)
verify.AnchorPoint = Vector2.new(0.5, 1)
verify.Text = "VERIFY KEY"
verify.Font = Enum.Font.GothamBlack
verify.TextSize = 15
verify.TextColor3 = Color3.new(1,1,1)
verify.BackgroundColor3 = CONFIG.ACCENT
Instance.new("UICorner", verify).CornerRadius = UDim.new(0, 8)

verify.MouseButton1Click:Connect(function()
    local key = keyBox.Text:gsub("%s+", "")
    if key == "" then return end

    verify.Text = "CHECKING..."
    verify.Active = false

    local ok = false
    local success = pcall(function()
        ok = Junkie.verifyKey(CONFIG.API_KEY, key, CONFIG.SERVICE)
    end)

    if success and ok then
        verify.Text = "ACCESS GRANTED"

        TweenService:Create(blur, TweenInfo.new(0.4), {Size = 0}):Play()
        task.wait(0.4)

        gui:Destroy()
        blur:Destroy()

        loadstring(game:HttpGet(CONFIG.MAIN_SCRIPT))()
    else
        verify.Text = "INVALID KEY"
        task.wait(2)
        verify.Text = "VERIFY KEY"
        verify.Active = true
    end
end)
