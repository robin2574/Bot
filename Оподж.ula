-- ========== NEO CASE FARM + AUTO BATTLE FOR MOBILE BY ROBIN ==========
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local GuiService = game:GetService("GuiService")

-- ВСЕ КЕЙСЫ
local AllCases = {
    "Trash", "Daily", "Photon Core", "Divine", "Beggar", "Plodder",
    "Office Clerk", "Manager", "Director", "Oligarch", "Frozen Heart",
    "Bubble Gum", "Cats", "Glitch", "Dream", "Bloody Night", "Heavenfall",
    "M5 F90", "G63", "Porsche 911", "URUS", "Gold", "Dark", "Palm",
    "Burj", "Luxury", "Marina", "Cursed Demon", "Abyss Core"
}

-- НАЛАШТУВАННЯ
local Settings = {
    OpenAmount = 10,
    FarmDelay = 0.01,
    BattleCaseAmount = 10,
    BattleDelay = 35,
    LuckEnabled = false,
    LuckMultiplier = 2,
    SelectedCase = "Heavenfall",
    SelectedCases = {}
}

-- СТАН
local Farming = false
local AutoSellEnabled = false
local AutoBattleEnabled = false
local LuckActive = false
local LuckEndTime = 0

-- ========== ФУНКЦІЯ ПОШУКУ ТА АКТИВАЦІЇ КНОПОК (ПРАЦЮЄ НА ТЕЛЕФОНІ) ==========
local function findAndClick(buttonText, partial)
    for _, gui in pairs(LocalPlayer.PlayerGui:GetDescendants()) do
        if gui:IsA("TextButton") or gui:IsA("ImageButton") then
            local text = gui:IsA("TextButton") and gui.Text or gui.Name
            if partial then
                if string.find(string.lower(text), string.lower(buttonText)) then
                    gui:Activate()
                    return true
                end
            else
                if text == buttonText then
                    gui:Activate()
                    return true
                end
            end
        end
    end
    return false
end

-- ========== ФУНКЦІЯ ПОШУКУ КНОПКИ З КЕЙСОМ ==========
local function findAndClickCase(caseName)
    for _, gui in pairs(LocalPlayer.PlayerGui:GetDescendants()) do
        if gui:IsA("TextButton") then
            if gui.Text == caseName or string.find(gui.Text, caseName) then
                gui:Activate()
                return true
            end
        end
    end
    return false
end

-- ========== АВТО-БАТЛ ЦИКЛ (БЕЗ КООРДИНАТ) ==========
local function AutoBattleLoop()
    while AutoBattleEnabled do
        pcall(function()
            print("⚔️ Відкриваю BATTLES...")
            task.wait(1)
            findAndClick("BATTLES", true)
            task.wait(2)
            
            print("⚔️ Create battle...")
            findAndClick("Create battle", true) or findAndClick("Create", true)
            task.wait(2)
            
            print("📦 Додаю кейси: " .. Settings.BattleCaseAmount)
            for i = 1, Settings.BattleCaseAmount do
                findAndClick("+", false)  -- Кнопка +
                task.wait(0.5)
                findAndClickCase(Settings.SelectedCase)  -- Вибираємо кейс
                task.wait(0.3)
            end
            
            print("✅ Create...")
            findAndClick("Create", true)
            task.wait(2)
            
            print("🤖 Play with bot...")
            findAndClick("Play with bot", true) or findAndClick("Bot", true)
            
            print("⏳ Чекаю " .. Settings.BattleDelay .. " сек...")
            task.wait(Settings.BattleDelay)
        end)
    end
end

-- ========== ФАРМ КЕЙСІВ ==========
local function FarmLoop()
    while Farming do
        pcall(function()
            local Events = ReplicatedStorage:FindFirstChild("Events")
            if Events then
                local OpenCase = Events:FindFirstChild("OpenCase")
                if OpenCase then
                    for caseName, _ in pairs(Settings.SelectedCases) do
                        for i = 1, Settings.OpenAmount do
                            OpenCase:InvokeServer(caseName, 1)
                            task.wait(0.03)
                        end
                    end
                end
                
                if AutoSellEnabled then
                    local Inventory = Events:FindFirstChild("Inventory")
                    if Inventory then
                        Inventory:FireServer("Sell", "ALL")
                    end
                end
            end
        end)
        task.wait(Settings.FarmDelay)
    end
end

-- ========== GUI (ОПТИМІЗОВАНО ДЛЯ ТЕЛЕФОНА) ==========
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "NeoFarmMobile"
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Головне меню (більше для телефона)
local MainFrame = Instance.new("Frame")
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 5, 25)
MainFrame.BackgroundTransparency = 0.1
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0.25, 0, 0.2, 0)
MainFrame.Size = UDim2.new(0, 300, 0, 450)
MainFrame.Active = true
MainFrame.Draggable = true

local stroke = Instance.new("UIStroke")
stroke.Color = Color3.fromRGB(200, 0, 255)
stroke.Thickness = 2
stroke.Parent = MainFrame

Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)

-- Заголовок
local Title = Instance.new("TextLabel")
Title.Parent = MainFrame
Title.BackgroundTransparency = 1
Title.Position = UDim2.new(0, 0, 0, 10)
Title.Size = UDim2.new(1, 0, 0, 35)
Title.Font = Enum.Font.GothamBold
Title.Text = "⚡ NEO FARM MOBILE ⚡"
Title.TextColor3 = Color3.fromRGB(200, 0, 255)
Title.TextSize = 16

-- Вибраний кейс
local SelectedLabel = Instance.new("TextLabel")
SelectedLabel.Parent = MainFrame
SelectedLabel.BackgroundColor3 = Color3.fromRGB(25, 10, 35)
SelectedLabel.BorderSizePixel = 0
SelectedLabel.Position = UDim2.new(0.05, 0, 0.12, 0)
SelectedLabel.Size = UDim2.new(0.9, 0, 0, 35)
SelectedLabel.Font = Enum.Font.GothamBold
SelectedLabel.Text = "🎯 " .. Settings.SelectedCase
SelectedLabel.TextColor3 = Color3.fromRGB(0, 255, 100)
SelectedLabel.TextSize = 13
Instance.new("UIStroke", SelectedLabel).Color = Color3.fromRGB(200, 0, 255)
Instance.new("UICorner", SelectedLabel).CornerRadius = UDim.new(0, 8)

-- Список кейсів
local ScrollFrame = Instance.new("ScrollingFrame")
ScrollFrame.Parent = MainFrame
ScrollFrame.BackgroundColor3 = Color3.fromRGB(20, 8, 30)
ScrollFrame.BorderSizePixel = 0
ScrollFrame.Position = UDim2.new(0.05, 0, 0.22, 0)
ScrollFrame.Size = UDim2.new(0.9, 0, 0, 150)
ScrollFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
ScrollFrame.ScrollBarThickness = 6
ScrollFrame.ScrollBarImageColor3 = Color3.fromRGB(200, 0, 255)
Instance.new("UIStroke", ScrollFrame).Color = Color3.fromRGB(200, 0, 255)
Instance.new("UICorner", ScrollFrame).CornerRadius = UDim.new(0, 8)

local CasesList = Instance.new("UIListLayout")
CasesList.Parent = ScrollFrame
CasesList.Padding = UDim.new(0, 5)

-- Оновлення списку кейсів
local function RefreshCases()
    for _, child in pairs(ScrollFrame:GetChildren()) do
        if child:IsA("TextButton") then child:Destroy() end
    end
    
    local yPos = 0
    for _, caseName in ipairs(AllCases) do
        local btn = Instance.new("TextButton")
        btn.Parent = ScrollFrame
        btn.BackgroundColor3 = Settings.SelectedCases[caseName] and Color3.fromRGB(200, 0, 255) or Color3.fromRGB(25, 10, 35)
        btn.BorderSizePixel = 0
        btn.Size = UDim2.new(1, -10, 0, 35)
        btn.Position = UDim2.new(0, 5, 0, yPos)
        btn.Font = Enum.Font.Gotham
        btn.Text = (Settings.SelectedCases[caseName] and "✅ " or "☐ ") .. caseName
        btn.TextColor3 = Settings.SelectedCases[caseName] and Color3.fromRGB(0, 0, 0) or Color3.fromRGB(255, 255, 255)
        btn.TextSize = 12
        Instance.new("UIStroke", btn).Color = Color3.fromRGB(200, 0, 255)
        Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
        
        btn.MouseButton1Click:Connect(function()
            Settings.SelectedCase = caseName
            SelectedLabel.Text = "🎯 " .. caseName
            RefreshCases()
        end)
        
        yPos = yPos + 40
    end
    
    ScrollFrame.CanvasSize = UDim2.new(0, 0, 0, yPos + 10)
end

-- Кнопка СТАРТ ФАРМ
local StartFarmBtn = Instance.new("TextButton")
StartFarmBtn.Parent = MainFrame
StartFarmBtn.BackgroundColor3 = Color3.fromRGB(25, 10, 35)
StartFarmBtn.BorderSizePixel = 0
StartFarmBtn.Position = UDim2.new(0.05, 0, 0.58, 0)
StartFarmBtn.Size = UDim2.new(0.43, 0, 0, 45)
StartFarmBtn.Font = Enum.Font.GothamBold
StartFarmBtn.Text = "▶ ФАРМ"
StartFarmBtn.TextColor3 = Color3.fromRGB(200, 0, 255)
StartFarmBtn.TextSize = 14
Instance.new("UIStroke", StartFarmBtn).Color = Color3.fromRGB(200, 0, 255)
Instance.new("UICorner", StartFarmBtn).CornerRadius = UDim.new(0, 8)

-- Кнопка ПРОДАЖА
local SellBtn = Instance.new("TextButton")
SellBtn.Parent = MainFrame
SellBtn.BackgroundColor3 = Color3.fromRGB(25, 10, 35)
SellBtn.BorderSizePixel = 0
SellBtn.Position = UDim2.new(0.52, 0, 0.58, 0)
SellBtn.Size = UDim2.new(0.43, 0, 0, 45)
SellBtn.Font = Enum.Font.GothamBold
SellBtn.Text = "🛒 ПРОДАЖ"
SellBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
SellBtn.TextSize = 13
Instance.new("UIStroke", SellBtn).Color = Color3.fromRGB(255, 100, 100)
Instance.new("UICorner", SellBtn).CornerRadius = UDim.new(0, 8)

-- Кнопка БАТЛ
local BattleBtn = Instance.new("TextButton")
BattleBtn.Parent = MainFrame
BattleBtn.BackgroundColor3 = Color3.fromRGB(25, 10, 35)
BattleBtn.BorderSizePixel = 0
BattleBtn.Position = UDim2.new(0.05, 0, 0.7, 0)
BattleBtn.Size = UDim2.new(0.9, 0, 0, 45)
BattleBtn.Font = Enum.Font.GothamBold
BattleBtn.Text = "⚔️ БАТЛ: ВЫКЛ"
BattleBtn.TextColor3 = Color3.fromRGB(150, 100, 255)
BattleBtn.TextSize = 14
Instance.new("UIStroke", BattleBtn).Color = Color3.fromRGB(150, 100, 255)
Instance.new("UICorner", BattleBtn).CornerRadius = UDim.new(0, 8)

-- Кнопка ВИБРАТИ ВСІ
local SelectAllBtn = Instance.new("TextButton")
SelectAllBtn.Parent = MainFrame
SelectAllBtn.BackgroundColor3 = Color3.fromRGB(25, 10, 35)
SelectAllBtn.BorderSizePixel = 0
SelectAllBtn.Position = UDim2.new(0.05, 0, 0.82, 0)
SelectAllBtn.Size = UDim2.new(0.43, 0, 0, 35)
SelectAllBtn.Font = Enum.Font.GothamBold
SelectAllBtn.Text = "✅ ВСІ"
SelectAllBtn.TextColor3 = Color3.fromRGB(0, 255, 100)
SelectAllBtn.TextSize = 12
Instance.new("UIStroke", SelectAllBtn).Color = Color3.fromRGB(0, 255, 100)
Instance.new("UICorner", SelectAllBtn).CornerRadius = UDim.new(0, 8)

-- Кнопка ЗНЯТИ ВСІ
local DeselectAllBtn = Instance.new("TextButton")
DeselectAllBtn.Parent = MainFrame
DeselectAllBtn.BackgroundColor3 = Color3.fromRGB(25, 10, 35)
DeselectAllBtn.BorderSizePixel = 0
DeselectAllBtn.Position = UDim2.new(0.52, 0, 0.82, 0)
DeselectAllBtn.Size = UDim2.new(0.43, 0, 0, 35)
DeselectAllBtn.Font = Enum.Font.GothamBold
DeselectAllBtn.Text = "❌ ЗНЯТИ"
DeselectAllBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
DeselectAllBtn.TextSize = 12
Instance.new("UIStroke", DeselectAllBtn).Color = Color3.fromRGB(255, 100, 100)
Instance.new("UICorner", DeselectAllBtn).CornerRadius = UDim.new(0, 8)

-- Статус
local StatusLabel = Instance.new("TextLabel")
StatusLabel.Parent = MainFrame
StatusLabel.BackgroundTransparency = 1
StatusLabel.Position = UDim2.new(0.05, 0, 0.92, 0)
StatusLabel.Size = UDim2.new(0.9, 0, 0, 25)
StatusLabel.Font = Enum.Font.Gotham
StatusLabel.Text = "👑 ROBIN | MOBILE"
StatusLabel.TextColor3 = Color3.fromRGB(200, 0, 255)
StatusLabel.TextSize = 11

-- ========== ПІДКЛЮЧЕННЯ ПОДІЙ ==========
StartFarmBtn.MouseButton1Click:Connect(function()
    if next(Settings.SelectedCases) == nil then
        StatusLabel.Text = "⚠️ ВИБЕРИ КЕЙСИ!"
        task.wait(2)
        StatusLabel.Text = "👑 ROBIN | MOBILE"
        return
    end
    
    Farming = not Farming
    if Farming then
        StartFarmBtn.Text = "⏹ СТОП"
        StartFarmBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
        StatusLabel.Text = "⚡ ФАРМ АКТИВНИЙ"
        task.spawn(FarmLoop)
    else
        StartFarmBtn.Text = "▶ ФАРМ"
        StartFarmBtn.TextColor3 = Color3.fromRGB(200, 0, 255)
        StatusLabel.Text = "👑 ROBIN | MOBILE"
    end
end)

SellBtn.MouseButton1Click:Connect(function()
    AutoSellEnabled = not AutoSellEnabled
    SellBtn.Text = AutoSellEnabled and "🟢 ПРОДАЖ" or "🛒 ПРОДАЖ"
    SellBtn.TextColor3 = AutoSellEnabled and Color3.fromRGB(0, 255, 100) or Color3.fromRGB(255, 100, 100)
end)

BattleBtn.MouseButton1Click:Connect(function()
    AutoBattleEnabled = not AutoBattleEnabled
    if AutoBattleEnabled then
        BattleBtn.Text = "⚔️ БАТЛ: ВКЛ"
        BattleBtn.TextColor3 = Color3.fromRGB(0, 255, 100)
        StatusLabel.Text = "⚔️ БАТЛ АКТИВНИЙ"
        task.spawn(AutoBattleLoop)
    else
        BattleBtn.Text = "⚔️ БАТЛ: ВЫКЛ"
        BattleBtn.TextColor3 = Color3.fromRGB(150, 100, 255)
        StatusLabel.Text = "👑 ROBIN | MOBILE"
    end
end)

SelectAllBtn.MouseButton1Click:Connect(function()
    for _, caseName in ipairs(AllCases) do
        Settings.SelectedCases[caseName] = true
    end
    RefreshCases()
end)

DeselectAllBtn.MouseButton1Click:Connect(function()
    Settings.SelectedCases = {}
    RefreshCases()
end)

-- Ініціалізація
RefreshCases()

print("✅ NEO FARM MOBILE BY ROBIN LOADED!")
print("📱 ПРАЦЮЄ НА ТЕЛЕФОНІ БЕЗ КООРДИНАТ!")
