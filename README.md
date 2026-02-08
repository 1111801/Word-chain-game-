local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local LocalPlayer = game:GetService("Players").LocalPlayer

local Library = { CurrentColor = Color3.fromRGB(50, 255, 50) }

local function AddCorner(obj, radius)
    local corner = Instance.new("UICorner")
    corner.CornerRadius = radius or UDim.new(0, 8)
    corner.Parent = obj
end

function Library:CreateWindow(config)
    local title = config.Title or "UI Library"
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "Gemini_UI"
    ScreenGui.ResetOnSpawn = false
    ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

    local MainFrame = Instance.new("Frame")
    MainFrame.Size = UDim2.new(0, 300, 0, 200)
    MainFrame.Position = UDim2.new(0.5, -150, 0.5, -100)
    MainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    MainFrame.ClipsDescendants = true
    MainFrame.Active = true
    MainFrame.Parent = ScreenGui
    AddCorner(MainFrame, UDim.new(0, 10))

    -- 상단바 (드래그 영역)
    local TopBar = Instance.new("Frame")
    TopBar.Size = UDim2.new(1, 0, 0, 35)
    TopBar.BackgroundTransparency = 1
    TopBar.Parent = MainFrame

    local TitleLabel = Instance.new("TextLabel")
    TitleLabel.Size = UDim2.new(1, -50, 1, 0)
    TitleLabel.Position = UDim2.new(0, 40, 0, 0)
    TitleLabel.Text = title
    TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    TitleLabel.Font = Enum.Font.GothamBold
    TitleLabel.TextSize = 14
    TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
    TitleLabel.BackgroundTransparency = 1
    TitleLabel.Parent = TopBar

    local TabBtn = Instance.new("TextButton")
    TabBtn.Size = UDim2.new(0, 30, 0, 30)
    TabBtn.Position = UDim2.new(0, 5, 0, 2)
    TabBtn.Text = "≡"
    TabBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    TabBtn.TextSize = 20
    TabBtn.BackgroundTransparency = 1
    TabBtn.Parent = TopBar

    -- 사이드 탭 (이름 형식)
    local SideTab = Instance.new("Frame")
    SideTab.Size = UDim2.new(0, 100, 1, -35)
    SideTab.Position = UDim2.new(-1.1, 0, 0, 35)
    SideTab.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    SideTab.ZIndex = 10
    SideTab.Parent = MainFrame
    AddCorner(SideTab, UDim.new(0, 5))

    local TabList = Instance.new("UIListLayout")
    TabList.Padding = UDim.new(0, 5)
    TabList.HorizontalAlignment = Enum.HorizontalAlignment.Center
    TabList.Parent = SideTab

    -- 컨텐츠 영역
    local Container = Instance.new("ScrollingFrame")
    Container.Size = UDim2.new(1, -20, 1, -45)
    Container.Position = UDim2.new(0, 10, 0, 40)
    Container.BackgroundTransparency = 1
    Container.ScrollBarThickness = 0
    Container.Parent = MainFrame

    local UIList = Instance.new("UIListLayout")
    UIList.Padding = UDim.new(0, 6)
    UIList.Parent = Container

    -- 기능 추가용 내부 함수
    local Elements = {}
    local function CreatePocket(name)
        local Pocket = Instance.new("Frame")
        Pocket.Size = UDim2.new(1, 0, 0, 35)
        Pocket.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        Pocket.Parent = Container
        AddCorner(Pocket)
        local L = Instance.new("TextLabel")
        L.Size = UDim2.new(0.5, 0, 1, 0); L.Position = UDim2.new(0, 10, 0, 0)
        L.Text = name; L.TextColor3 = Color3.fromRGB(255,255,255); L.BackgroundTransparency = 1
        L.Font = Enum.Font.Gotham; L.TextSize = 12; L.TextXAlignment = Enum.TextXAlignment.Left; L.Parent = Pocket
        return Pocket
    end

    function Elements:NewTab(name)
        local B = Instance.new("TextButton")
        B.Size = UDim2.new(0.9, 0, 0, 30)
        B.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        B.Text = name; B.TextColor3 = Color3.fromRGB(255, 255, 255)
        B.Font = Enum.Font.Gotham; B.TextSize = 12; B.Parent = SideTab
        AddCorner(B, UDim.new(0, 4))
        return B
    end

    -- 드래그 & 탭 애니메이션 로직 생략(위와 동일하게 내장됨)
    local sideOpen = false
    TabBtn.MouseButton1Click:Connect(function()
        sideOpen = not sideOpen
        SideTab:TweenPosition(sideOpen and UDim2.new(0, 0, 0, 35) or UDim2.new(-1.1, 0, 0, 35), "Out", "Quart", 0.3, true)
    end)

    return Elements
end

return Library

