-- OPEN SOURCE EGG/PET HUB
-- UI inspirada nas imagens enviadas
-- Sem a marca/textos originais

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Remove versão antiga
local old = playerGui:FindFirstChild("OpenEggHub")
if old then
	old:Destroy()
end

-- GUI
local gui = Instance.new("ScreenGui")
gui.Name = "OpenEggHub"
gui.ResetOnSpawn = false
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
gui.Parent = playerGui

-- Funções auxiliares
local function corner(obj, radius)
	local c = Instance.new("UICorner")
	c.CornerRadius = UDim.new(0, radius or 10)
	c.Parent = obj
end

local function stroke(obj, color, thickness)
	local s = Instance.new("UIStroke")
	s.Color = color
	s.Thickness = thickness or 1
	s.Parent = obj
end

local function makeLabel(parent, text, size, position, textSize)
	local l = Instance.new("TextLabel")
	l.BackgroundTransparency = 1
	l.Text = text
	l.TextColor3 = Color3.fromRGB(235, 245, 240)
	l.TextSize = textSize or 16
	l.Font = Enum.Font.GothamMedium
	l.Size = size
	l.Position = position
	l.TextXAlignment = Enum.TextXAlignment.Left
	l.Parent = parent
	return l
end

local function makeButton(parent, text, size, position)
	local b = Instance.new("TextButton")

	b.AutoButtonColor = false
	b.BackgroundColor3 = Color3.fromRGB(13, 23, 24)
	b.TextColor3 = Color3.fromRGB(235, 245, 240)
	b.Text = text
	b.TextSize = 15
	b.Font = Enum.Font.GothamSemibold
	b.Size = size
	b.Position = position
	b.Parent = parent

	corner(b, 10)
	stroke(b, Color3.fromRGB(48, 100, 87), 1)

	b.MouseEnter:Connect(function()
		TweenService:Create(
			b,
			TweenInfo.new(0.15),
			{
				BackgroundColor3 = Color3.fromRGB(24, 52, 45)
			}
		):Play()
	end)

	b.MouseLeave:Connect(function()
		TweenService:Create(
			b,
			TweenInfo.new(0.15),
			{
				BackgroundColor3 = Color3.fromRGB(13, 23, 24)
			}
		):Play()
	end)

	return b
end

-- Janela principal
local main = Instance.new("Frame")
main.Name = "Main"
main.AnchorPoint = Vector2.new(0.5, 0.5)
main.Position = UDim2.fromScale(0.5, 0.5)
main.Size = UDim2.fromOffset(620, 430)
main.BackgroundColor3 = Color3.fromRGB(5, 13, 14)
main.Parent = gui

corner(main, 18)
stroke(main, Color3.fromRGB(35, 150, 105), 2)

-- Cabeçalho
local header = Instance.new("Frame")
header.Size = UDim2.new(1, 0, 0, 72)
header.BackgroundColor3 = Color3.fromRGB(7, 19, 20)
header.Parent = main
corner(header, 18)

-- Botão fechar
local close = makeButton(
	header,
	"×",
	UDim2.fromOffset(42, 42),
	UDim2.new(1, -55, 0, 15)
)

close.TextSize = 28

close.MouseButton1Click:Connect(function()
	gui:Destroy()
end)

-- Abas
local tabs = Instance.new("Frame")
tabs.BackgroundTransparency = 1
tabs.Position = UDim2.fromOffset(18, 82)
tabs.Size = UDim2.new(1, -36, 0, 42)
tabs.Parent = main

local eggTab = makeButton(
	tabs,
	"EGGS",
	UDim2.fromOffset(120, 40),
	UDim2.fromOffset(0, 0)
)

local petTab = makeButton(
	tabs,
	"PETS",
	UDim2.fromOffset(120, 40),
	UDim2.fromOffset(130, 0)
)

local tpTab = makeButton(
	tabs,
	"TELEPORT",
	UDim2.fromOffset(120, 40),
	UDim2.fromOffset(260, 0)
)

-- Conteúdo
local content = Instance.new("Frame")
content.BackgroundTransparency = 1
content.Position = UDim2.fromOffset(18, 135)
content.Size = UDim2.new(1, -36, 1, -153)
content.Parent = main

--------------------------------------------------
-- EGGS
--------------------------------------------------

local eggsPage = Instance.new("Frame")
eggsPage.BackgroundTransparency = 1
eggsPage.Size = UDim2.fromScale(1, 1)
eggsPage.Parent = content

makeLabel(
	eggsPage,
	"BEST EGG",
	UDim2.fromOffset(200, 30),
	UDim2.fromOffset(8, 0),
	18
)

makeLabel(
	eggsPage,
	"Escolha um ovo",
	UDim2.fromOffset(300, 24),
	UDim2.fromOffset(8, 30),
	13
)

local eggBox = Instance.new("Frame")
eggBox.Position = UDim2.fromOffset(0, 64)
eggBox.Size = UDim2.new(1, 0, 0, 82)
eggBox.BackgroundColor3 = Color3.fromRGB(8, 20, 21)
eggBox.Parent = eggsPage

corner(eggBox, 14)
stroke(eggBox, Color3.fromRGB(35, 90, 76), 1)

makeLabel(
	eggBox,
	"Melhor Ovo",
	UDim2.fromOffset(270, 30),
	UDim2.fromOffset(18, 12),
	18
)

local eggStatus = makeLabel(
	eggBox,
	"Selecionado: nenhum",
	UDim2.fromOffset(300, 24),
	UDim2.fromOffset(18, 45),
	13
)

local selectEgg = makeButton(
	eggBox,
	"SELECIONAR",
	UDim2.fromOffset(125, 42),
	UDim2.new(1, -145, 0, 20)
)

local eggs = {
	"Egg 1",
	"Egg 2",
	"Egg 3",
	"Egg 4"
}

local eggIndex = 0

selectEgg.MouseButton1Click:Connect(function()

	eggIndex = (eggIndex % #eggs) + 1

	eggStatus.Text =
		"Selecionado: " .. eggs[eggIndex]

end)

local openEgg = makeButton(
	eggsPage,
	"ABRIR / TESTAR",
	UDim2.fromOffset(180, 44),
	UDim2.fromOffset(0, 164)
)

openEgg.MouseButton1Click:Connect(function()

	if eggIndex == 0 then
		eggStatus.Text = "Selecione um ovo primeiro"
	else
		eggStatus.Text =
			"Ação: " .. eggs[eggIndex]
	end

end)

--------------------------------------------------
-- PETS
--------------------------------------------------

local petsPage = Instance.new("Frame")
petsPage.BackgroundTransparency = 1
petsPage.Size = UDim2.fromScale(1, 1)
petsPage.Visible = false
petsPage.Parent = content

makeLabel(
	petsPage,
	"TOP 4 PETS",
	UDim2.fromOffset(250, 30),
	UDim2.fromOffset(8, 0),
	18
)

local pets = {
	{"#1", "Pet A", "$8.9M", "Mythic"},
	{"#2", "Pet B", "$1.54M", "Mythic"},
	{"#3", "Pet C", "$1.35M", "Legendary"},
	{"#4", "Pet D", "$263.05K", "Legendary"}
}

for i, pet in ipairs(pets) do

	local row = Instance.new("Frame")

	row.Position =
		UDim2.fromOffset(
			0,
			38 + (i - 1) * 58
		)

	row.Size =
		UDim2.new(1, 0, 0, 50)

	row.BackgroundColor3 =
		Color3.fromRGB(8, 20, 21)

	row.Parent = petsPage

	corner(row, 10)

	makeLabel(
		row,
		pet[1],
		UDim2.fromOffset(50, 25),
		UDim2.fromOffset(14, 13),
		15
	)

	makeLabel(
		row,
		pet[2],
		UDim2.fromOffset(230, 25),
		UDim2.fromOffset(68, 7),
		16
	)

	local rarity = makeLabel(
		row,
		pet[4],
		UDim2.fromOffset(180, 20),
		UDim2.fromOffset(68, 27),
		12
	)

	rarity.TextColor3 =
		Color3.fromRGB(160, 100, 255)

	local value = makeLabel(
		row,
		pet[3],
		UDim2.fromOffset(130, 28),
		UDim2.new(1, -150, 0, 11),
		16
	)

	value.TextXAlignment =
		Enum.TextXAlignment.Right

	value.TextColor3 =
		Color3.fromRGB(45, 230, 105)

end

--------------------------------------------------
-- TELEPORT
--------------------------------------------------

local tpPage = Instance.new("Frame")
tpPage.BackgroundTransparency = 1
tpPage.Size = UDim2.fromScale(1, 1)
tpPage.Visible = false
tpPage.Parent = content

makeLabel(
	tpPage,
	"TELEPORT",
	UDim2.fromOffset(250, 30),
	UDim2.fromOffset(8, 0),
	18
)

makeLabel(
	tpPage,
	"One Shot / Loop",
	UDim2.fromOffset(250, 24),
	UDim2.fromOffset(8, 30),
	13
)

local destination = "Destino 1"

local destinationBtn = makeButton(
	tpPage,
	destination,
	UDim2.fromOffset(250, 44),
	UDim2.fromOffset(0, 70)
)

destinationBtn.MouseButton1Click:Connect(function()

	if destination == "Destino 1" then
		destination = "Destino 2"

	elseif destination == "Destino 2" then
		destination = "Destino 3"

	else
		destination = "Destino 1"
	end

	destinationBtn.Text = destination

end)

local teleportBtn = makeButton(
	tpPage,
	"TELEPORTAR",
	UDim2.fromOffset(160, 44),
	UDim2.fromOffset(270, 70)
)

teleportBtn.MouseButton1Click:Connect(function()

	-- Conecte aqui o sistema de teleporte
	-- do seu próprio jogo.

	teleportBtn.Text =
		"OK: " .. destination

	task.delay(1, function()

		if teleportBtn.Parent then
			teleportBtn.Text = "TELEPORTAR"
		end

	end)

end)

local loop = false

local loopBtn = makeButton(
	tpPage,
	"LOOP: OFF",
	UDim2.fromOffset(160, 44),
	UDim2.fromOffset(0, 130)
)

loopBtn.MouseButton1Click:Connect(function()

	loop = not loop

	if loop then
		loopBtn.Text = "LOOP: ON"
	else
		loopBtn.Text = "LOOP: OFF"
	end

end)

--------------------------------------------------
-- Troca de abas
--------------------------------------------------

local function showPage(page)

	eggsPage.Visible = page == eggsPage
	petsPage.Visible = page == petsPage
	tpPage.Visible = page == tpPage

end

eggTab.MouseButton1Click:Connect(function()
	showPage(eggsPage)
end)

petTab.MouseButton1Click:Connect(function()
	showPage(petsPage)
end)

tpTab.MouseButton1Click:Connect(function()
	showPage(tpPage)
end)

--------------------------------------------------
-- Arrastar janela
--------------------------------------------------

local dragging = false
local dragStart
local startPosition

header.InputBegan:Connect(function(input)

	if input.UserInputType == Enum.UserInputType.MouseButton1
		or input.UserInputType == Enum.UserInputType.Touch then

		dragging = true
		dragStart = input.Position
		startPosition = main.Position

		input.Changed:Connect(function()

			if input.UserInputState ==
				Enum.UserInputState.End then

				dragging = false

			end

		end)

	end

end)

UserInputService.InputChanged:Connect(function(input)

	if not dragging then
		return
	end

	if input.UserInputType ==
		Enum.UserInputType.MouseMovement
		or input.UserInputType ==
		Enum.UserInputType.Touch then

		local delta =
			input.Position - dragStart

		main.Position =
			UDim2.new(
				startPosition.X.Scale,
				startPosition.X.Offset + delta.X,

				startPosition.Y.Scale,
				startPosition.Y.Offset + delta.Y
			)

	end

end)

print("OpenEggHub carregado!")
