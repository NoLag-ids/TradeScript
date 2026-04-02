--// LocalScript → StarterPlayerScripts
-- OG Block Visual Script (changes "Secret" Lucky Blocks to look like "OG")

local Players          = game:GetService("Players")
local StarterGui       = game:GetService("StarterGui")
local TweenService     = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

local player    = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Create ScreenGui + UI
local sg = Instance.new("ScreenGui")
sg.Name = "OGBlockUI"
sg.ResetOnSpawn = false
sg.Parent = playerGui

local main = Instance.new("Frame")
main.Size       = UDim2.new(0, 220, 0, 150)
main.Position   = UDim2.new(0.5, -110, 0.5, -75)
main.BackgroundColor3 = Color3.fromRGB(18, 18, 22)
main.BorderSizePixel = 0
main.ClipsDescendants = true
main.Parent = sg

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = main

local stroke = Instance.new("UIStroke")
stroke.Color      = Color3.fromRGB(255, 221, 85)
stroke.Thickness  = 2
stroke.Transparency = 0.45
stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
stroke.Parent = main

-- Title bar (draggable)
local titleBar = Instance.new("Frame")
titleBar.Size               = UDim2.new(1, 0, 0, 34)
titleBar.BackgroundTransparency = 1
titleBar.Parent = main

local title = Instance.new("TextLabel")
title.Size               = UDim2.new(1, 0, 1, 0)
title.BackgroundTransparency = 1
title.Font               = Enum.Font.GothamBold
title.Text               = "OG BLOCK"
title.TextColor3         = Color3.fromRGB(255, 221, 85)
title.TextSize           = 20
title.TextStrokeTransparency = 0.75
title.TextStrokeColor3   = Color3.new(0,0,0)
title.Parent = titleBar

-- Big Inject Button
local btn = Instance.new("TextButton")
btn.Name = "InjectButton"
btn.Size       = UDim2.new(0.88, 0, 0, 54)
btn.Position   = UDim2.new(0.06, 0, 0, 62)
btn.BackgroundColor3 = Color3.fromRGB(255, 190, 40)
btn.TextColor3       = Color3.fromRGB(22, 22, 26)
btn.Font             = Enum.Font.GothamBlack
btn.Text             = "OG BLOCK"
btn.TextSize         = 26
btn.AutoButtonColor  = false
btn.Parent = main

local btnCorner = Instance.new("UICorner")
btnCorner.CornerRadius = UDim.new(0, 9)
btnCorner.Parent = btn

local btnStroke = Instance.new("UIStroke")
btnStroke.Color      = Color3.fromRGB(255, 221, 85)
btnStroke.Thickness  = 2.5
btnStroke.Transparency = 0.35
btnStroke.Parent = btn

local UIGrad = Instance.new("UIGradient")
UIGrad.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0.0, Color3.fromRGB(255,240,140)),
    ColorSequenceKeypoint.new(0.5, Color3.fromRGB(255,190,40)),
    ColorSequenceKeypoint.new(1.0, Color3.fromRGB(255,240,140)),
}
UIGrad.Rotation = 35
UIGrad.Parent = btn

-- Draggable Title Bar
local dragging, dragStart, startPos

titleBar.InputBegan:Connect(function(input)
    if not (input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch) then 
        return 
    end
    
    dragging   = true
    dragStart  = input.Position
    startPos   = main.Position
    
    local conn
    conn = input.Changed:Connect(function()
        if input.UserInputState == Enum.UserInputState.End then
            dragging = false
            conn:Disconnect()
        end
    end)
end)

UserInputService.InputChanged:Connect(function(input)
    if not dragging then return end
    if not (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then 
        return 
    end
    
    local delta = input.Position - dragStart
    main.Position = UDim2.new(
        startPos.X.Scale,   startPos.X.Offset + delta.X,
        startPos.Y.Scale,   startPos.Y.Offset + delta.Y
    )
end)

-- Button Tween Function
local function tween(obj, props, time, style)
    TweenService:Create(obj, TweenInfo.new(time or 0.25, style or Enum.EasingStyle.Quint), props):Play()
end

-- Button Hover & Click Animations
btn.MouseEnter:Connect(function()
    tween(btn,       {BackgroundColor3 = Color3.fromRGB(255, 215, 80)}, 0.25)
    tween(btnStroke, {Transparency = 0.1}, 0.25)
    tween(UIGrad,    {Rotation = 70}, 0.8)
end)

btn.MouseLeave:Connect(function()
    tween(btn,       {BackgroundColor3 = Color3.fromRGB(255, 190, 40)}, 0.25)
    tween(btnStroke, {Transparency = 0.35}, 0.25)
    tween(UIGrad,    {Rotation = 35}, 0.8)
end)

btn.MouseButton1Down:Connect(function()
    tween(btn, {Size = UDim2.new(0.84, 0, 0, 50)}, 0.11)
end)

btn.MouseButton1Up:Connect(function()
    tween(btn, {Size = UDim2.new(0.88, 0, 0, 54)}, 0.2, Enum.EasingStyle.Back)
end)

-- Notification Helper
local function notify(text)
    StarterGui:SetCore("SendNotification", {
        Title = "OG Block",
        Text = text,
        Duration = 5,
    })
end

-- Core Logic: Change Secret → OG and $750M → $750B
local alreadyInjected = false

local function tryMakeOG(block)
    if not block:IsA("TextLabel") then return false end
    
    local parent = block.Parent
    if not parent then return false end
    
    local display = parent:FindFirstChild("DisplayName")
    if not (display and display:IsA("TextLabel") and display.Text == "Lucky Block") then
        return false
    end
    
    local changed = false

    if block.Text == "Secret" then
        block.Text = "OG"
        block.TextColor3 = Color3.fromRGB(255, 221, 85)
        
        if not block:FindFirstChildOfClass("UIStroke") then
            local st = Instance.new("UIStroke")
            st.Color = Color3.new(0,0,0)
            st.Thickness = 2
            st.Parent = block
        end
        
        local grad = block:FindFirstChildOfClass("UIGradient")
        if grad then
            grad.Color = ColorSequence.new{
                ColorSequenceKeypoint.new(0,   Color3.fromRGB(255,235,120)),
                ColorSequenceKeypoint.new(0.5, Color3.fromRGB(255,190,50)),
                ColorSequenceKeypoint.new(1,   Color3.fromRGB(255,235,120)),
            }
        end
        
        changed = true
        
    elseif block.Text == "$750M" then
        block.Text = "$750B"
        changed = true
    end
    
    return changed
end

local function injectNow()
    if alreadyInjected then
        notify("Already injected!")
        return
    end
    
    local count = 0
    
    -- Apply to existing blocks
    for _, v in ipairs(game:GetDescendants()) do
        if tryMakeOG(v) then
            count += 1
        end
    end
    
    -- Listen for new blocks (future spawns)
    game.DescendantAdded:Connect(function(child)
        task.spawn(function()
            if tryMakeOG(child) then
                count += 1
            end
        end)
    end)
    
    alreadyInjected = true
    
    if count > 0 then
        notify("Injected → Upgraded " .. count .. " Secret block(s) to OG")
    else
        notify("No Secret Lucky Blocks found right now")
    end
end

-- Button Click
btn.Activated:Connect(function()
    local oldText = btn.Text
    btn.Text = "Injecting..."
    tween(btn, {BackgroundColor3 = Color3.fromRGB(90, 200, 90)}, 0.15)
    
    task.delay(0.9, function()
        injectNow()
        
        btn.Text = oldText
        tween(btn, {BackgroundColor3 = Color3.fromRGB(255, 190, 40)}, 0.35)
    end)
end)

-- Initial Notification
notify("UI Loaded • Drag from title bar • Click button to activate OG blocks")
