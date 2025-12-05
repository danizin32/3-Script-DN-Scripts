--<< GUI ESTILO IGUAL AO DA IMAGEM (SEM SOMBRA + ANIMAÇÃO PREMIUM) >>--

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local TS = game:GetService("TweenService")

local savedPos = nil
local espEnabled = false

-- GUI
local ScreenGui = Instance.new("ScreenGui", LocalPlayer.PlayerGui)
ScreenGui.ResetOnSpawn = false

-- FRAME PRINCIPAL
local Main = Instance.new("Frame")
Main.Size = UDim2.new(0, 280, 0, 310)
Main.Position = UDim2.new(0.05, 0, 0.25, 0)
Main.BackgroundColor3 = Color3.fromRGB(22, 22, 22)
Main.BorderSizePixel = 0
Main.Active = true
Main.Draggable = true
Main.Parent = ScreenGui
Main.ClipsDescendants = true

local UIScale = Instance.new("UIScale", Main)
UIScale.Scale = 1

local UIStroke = Instance.new("UIStroke", Main)
UIStroke.Thickness = 2
UIStroke.Color = Color3.fromRGB(60, 60, 60)

local UICorner = Instance.new("UICorner", Main)
UICorner.CornerRadius = UDim.new(0, 12)

-- TOPO
local Top = Instance.new("Frame", Main)
Top.Size = UDim2.new(1, 0, 0, 45)
Top.BackgroundColor3 = Color3.fromRGB(90, 60, 200)
Top.BorderSizePixel = 0
Instance.new("UICorner", Top).CornerRadius = UDim.new(0, 12)

-- TÍTULO
local Title = Instance.new("TextLabel", Top)
Title.Size = UDim2.new(1, -50, 1, 0)
Title.Position = UDim2.new(0, 15, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "injection - dn scripts"
Title.Font = Enum.Font.GothamBold
Title.TextSize = 22
Title.TextColor3 = Color3.fromRGB(245, 245, 245)
Title.TextXAlignment = Enum.TextXAlignment.Left

-- BOTÃO DE MINIMIZAR
local MinBtn = Instance.new("TextButton", Top)
MinBtn.Size = UDim2.new(0, 42, 0, 30)
MinBtn.Position = UDim2.new(1, -50, 0.15, 0)
MinBtn.BackgroundColor3 = Color3.fromRGB(140, 90, 255)
MinBtn.Text = "-"
MinBtn.TextColor3 = Color3.new(1,1,1)
MinBtn.Font = Enum.Font.GothamBold
MinBtn.TextSize = 28
MinBtn.BorderSizePixel = 0
Instance.new("UICorner", MinBtn).CornerRadius = UDim.new(0, 8)

-- CORPO
local Body = Instance.new("Frame", Main)
Body.Size = UDim2.new(1, 0, 1, -45)
Body.Position = UDim2.new(0, 0, 0, 45)
Body.BackgroundTransparency = 1

-- LOG
local Log = Instance.new("TextLabel", Body)
Log.Size = UDim2.new(1, -20, 0, 28)
Log.Position = UDim2.new(0, 10, 0, 5)
Log.BackgroundTransparency = 1
Log.Font = Enum.Font.Gotham
Log.TextSize = 16
Log.TextColor3 = Color3.fromRGB(255, 255, 255)
Log.Text = "[LOG] Janela restaurada."
Log.TextXAlignment = Enum.TextXAlignment.Left

function UpdateLog(msg)
	Log.Text = "[LOG] " .. msg
end

-- BOTÕES
local function createButton(text, y)
	local Btn = Instance.new("TextButton", Body)
	Btn.Size = UDim2.new(1, -30, 0, 48)
	Btn.Position = UDim2.new(0, 15, 0, y)
	Btn.BackgroundColor3 = Color3.fromRGB(140, 90, 255)
	Btn.Text = text
	Btn.TextColor3 = Color3.new(1,1,1)
	Btn.Font = Enum.Font.GothamBold
	Btn.TextSize = 20
	Btn.BorderSizePixel = 0
	Instance.new("UICorner", Btn).CornerRadius = UDim.new(0, 10)

	-- Hover suave
	Btn.MouseEnter:Connect(function()
		TS:Create(Btn, TweenInfo.new(0.15), {
			BackgroundColor3 = Color3.fromRGB(160, 110, 255)
		}):Play()
	end)
	Btn.MouseLeave:Connect(function()
		TS:Create(Btn, TweenInfo.new(0.15), {
			BackgroundColor3 = Color3.fromRGB(140, 90, 255)
		}):Play()
	end)

	return Btn
end

local SaveBtn = createButton("Save position", 45)
local TeleportBtn = createButton("Teleport", 105)
local ESPBtn = createButton("ESP Base - Brainrot", 165)

-- SALVAR POSIÇÃO
SaveBtn.MouseButton1Click:Connect(function()
	local char = LocalPlayer.Character
	if char and char:FindFirstChild("HumanoidRootPart") then
		savedPos = char.HumanoidRootPart.CFrame
		UpdateLog("Posição salva.")
	end
end)

-- TELEPORTAR
TeleportBtn.MouseButton1Click:Connect(function()
	local char = LocalPlayer.Character
	if savedPos and char and char:FindFirstChild("HumanoidRootPart") then
		char.HumanoidRootPart.CFrame = savedPos
		UpdateLog("Teletransportado.")
	else
		UpdateLog("Nenhuma posição salva.")
	end
end)

-- ESP RAIOS-X
ESPBtn.MouseButton1Click:Connect(function()
	espEnabled = not espEnabled

	for _, obj in ipairs(workspace:GetDescendants()) do
		if obj:IsA("BasePart") then
			obj.LocalTransparencyModifier = espEnabled and 0.7 or 0
		end
	end

	UpdateLog(espEnabled and "ESP Base ativado." or "ESP Base desativado.")
end)

-- ANIMAÇÃO DE FECHAR E ABRIR (igual jogo famoso)
local minimized = false
MinBtn.MouseButton1Click:Connect(function()
	minimized = not minimized

	if minimized then
		-- fecha
		TS:Create(UIScale, TweenInfo.new(0.25, Enum.EasingStyle.Quint), {Scale = 0.82}):Play()
		TS:Create(Main, TweenInfo.new(0.35, Enum.EasingStyle.Quint), {
			Size = UDim2.new(0, 280, 0, 45)
		}):Play()
		UpdateLog("Janela minimizada.")
	else
		-- abre
		TS:Create(UIScale, TweenInfo.new(0.25, Enum.EasingStyle.Quint), {Scale = 1}):Play()
		TS:Create(Main, TweenInfo.new(0.35, Enum.EasingStyle.Quint), {
			Size = UDim2.new(0, 280, 0, 310)
		}):Play()
		UpdateLog("Janela restaurada.")
	end
end)
