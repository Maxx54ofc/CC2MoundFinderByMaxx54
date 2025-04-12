-- LocalScript dentro de StarterPlayerScripts

-- Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

-- Aguarda o jogador carregar
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui", 10)
if not playerGui then
	warn("Erro: PlayerGui não encontrado!")
	return
end

-- Aguarda o personagem carregar
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart", 10)
if not humanoidRootPart then
	warn("Erro: HumanoidRootPart não encontrado!")
	return
end

-- Cria um ScreenGui para o jogador
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DraggableButtonGui"
screenGui.Enabled = true
screenGui.IgnoreGuiInset = true
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui
print("ScreenGui criado e adicionado ao PlayerGui")

-- Cria o botão principal
local button = Instance.new("TextButton")
button.Size = UDim2.new(0, 40, 0, 40)
button.Position = UDim2.new(0.5, -420, 0.5, -220)
button.BackgroundColor3 = Color3.fromRGB(91, 91, 91)
button.Text = "+"
button.TextColor3 = Color3.fromRGB(255, 255, 255)
button.TextSize = 20
button.Visible = true
button.ZIndex = 10
button.Parent = screenGui
print("Botão principal criado")

-- Adiciona bordas arredondadas ao botão
local buttonCorner = Instance.new("UICorner")
buttonCorner.CornerRadius = UDim.new(0, 10)
buttonCorner.Parent = button

-- Cria o menu
local menu = Instance.new("Frame")
menu.Size = UDim2.new(0, 150, 0, 300)
menu.Position = UDim2.new(0.5, -75, 0.5, 20)
menu.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
menu.Visible = false
menu.ZIndex = 10
menu.Parent = screenGui
print("Menu criado")

-- Adiciona bordas arredondadas ao menu
local menuCorner = Instance.new("UICorner")
menuCorner.CornerRadius = UDim.new(0, 10)
menuCorner.Parent = menu

-- Cria a assinatura "By Maxx54"
local signature = Instance.new("TextLabel")
signature.Size = UDim2.new(1, 0, 0, 20)
signature.Position = UDim2.new(0, 0, 1, -20)
signature.BackgroundTransparency = 1
signature.Text = "By Maxx54"
signature.TextColor3 = Color3.fromRGB(99, 99, 99)
signature.TextSize = 10
signature.ZIndex = 11
signature.Parent = menu
print("Assinatura criada")

-- Cria o botão "Find Eggs"
local findEggsButton = Instance.new("TextButton")
findEggsButton.Size = UDim2.new(0, 120, 0, 30)
findEggsButton.Position = UDim2.new(0, 15, 0, 15)
findEggsButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
findEggsButton.Text = "Find Eggs"
findEggsButton.TextColor3 = Color3.fromRGB(103, 103, 103)
findEggsButton.TextSize = 13
findEggsButton.Visible = true
findEggsButton.ZIndex = 11
findEggsButton.Parent = menu
print("Botão Find Eggs criado")

-- Adiciona bordas arredondadas ao botão "Find Eggs"
local findEggsCorner = Instance.new("UICorner")
findEggsCorner.CornerRadius = UDim.new(0, 8)
findEggsCorner.Parent = findEggsButton

-- Cria o botão "Reset"
local resetButton = Instance.new("TextButton")
resetButton.Size = UDim2.new(0, 120, 0, 30)
resetButton.Position = UDim2.new(0, 15, 0, 50)
resetButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
resetButton.Text = "Reset"
resetButton.TextColor3 = Color3.fromRGB(255, 255, 255)
resetButton.TextSize = 13
resetButton.Visible = true
resetButton.ZIndex = 11
resetButton.Parent = menu
print("Botão Reset criado")

-- Adiciona bordas arredondadas ao botão "Reset"
local resetCorner = Instance.new("UICorner")
resetCorner.CornerRadius = UDim.new(0, 8)
resetCorner.Parent = resetButton

-- Cria o botão "ESP"
local espButton = Instance.new("TextButton")
espButton.Size = UDim2.new(0, 120, 0, 30)
espButton.Position = UDim2.new(0, 15, 0, 85)
espButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
espButton.Text = "ESP: Off"
espButton.TextColor3 = Color3.fromRGB(255, 255, 255)
espButton.TextSize = 13
espButton.Visible = true
espButton.ZIndex = 11
espButton.Parent = menu
print("Botão ESP criado")

-- Adiciona bordas arredondadas ao botão "ESP"
local espCorner = Instance.new("UICorner")
espCorner.CornerRadius = UDim.new(0, 8)
espCorner.Parent = espButton

-- Cria o ScrollingFrame para os botões de categoria
local scrollingFrame = Instance.new("ScrollingFrame")
scrollingFrame.Size = UDim2.new(0, 120, 0, 140)
scrollingFrame.Position = UDim2.new(0, 15, 0, 120)
scrollingFrame.BackgroundTransparency = 1
scrollingFrame.ScrollBarThickness = 5
scrollingFrame.Visible = true
scrollingFrame.ZIndex = 11
scrollingFrame.Parent = menu
print("ScrollingFrame criado")

-- Adiciona UIListLayout ao ScrollingFrame
local listLayout = Instance.new("UIListLayout")
listLayout.SortOrder = Enum.SortOrder.LayoutOrder
listLayout.Padding = UDim.new(0, 5)
listLayout.Parent = scrollingFrame

-- Lista de categorias
local categories = {
	"Basic",
	"Booster",
	"Car",
	"Chomper",
	"Drone",
	"Faberge",
	"Fire",
	"Mine",
	"Shark",
	"SmallEggs",
	"Sturdy"
}

-- Tabela para rastrear o estado das categorias
local categoryStates = {}

-- Cria botões para cada categoria
for i, category in ipairs(categories) do
	local categoryButton = Instance.new("TextButton")
	categoryButton.Size = UDim2.new(0, 110, 0, 25)
	categoryButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
	categoryButton.Text = category
	categoryButton.TextColor3 = Color3.fromRGB(255, 255, 255)
	categoryButton.TextSize = 12
	categoryButton.Visible = true
	categoryButton.ZIndex = 11
	categoryButton.Parent = scrollingFrame
	print("Botão de categoria " .. category .. " criado")

	-- Adiciona bordas arredondadas
	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, 5)
	corner.Parent = categoryButton

	-- Inicializa o estado como ativo
	categoryStates[category] = true

	-- Função para alternar o estado
	categoryButton.MouseButton1Click:Connect(function()
		categoryStates[category] = not categoryStates[category]
		categoryButton.BackgroundColor3 = categoryStates[category] and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0)
	end)
end

-- Ajusta o CanvasSize do ScrollingFrame
scrollingFrame.CanvasSize = UDim2.new(0, 0, 0, #categories * 30)
print("CanvasSize do ScrollingFrame ajustado")

-- Variáveis para controle de arrastar
local buttonDragging = false
local buttonDragStart = nil
local buttonStartPos = nil
local menuDragging = false
local menuDragStart = nil
local menuStartPos = nil

-- Tabela para rastrear ovos visitados
local visitedEggs = {}

-- Variável para controlar o estado do ESP
local espEnabled = false

-- Tabela para armazenar os BillboardGuis dos ovos
local eggBillboards = {}

-- Função para criar ou atualizar o BillboardGui de um ovo
local function createEggESP(egg)
	if eggBillboards[egg] then
		return
	end

	local adornee = egg:IsA("BasePart") and egg or egg.PrimaryPart
	if not adornee then
		warn("Ovo sem BasePart ou PrimaryPart: " .. egg:GetFullName())
		return
	end

	local billboard = Instance.new("BillboardGui")
	billboard.Name = "EggESP"
	billboard.Adornee = adornee
	billboard.Size = UDim2.new(0, 100, 0, 50)
	billboard.StudsOffset = Vector3.new(0, 3, 0)
	billboard.AlwaysOnTop = true
	billboard.Enabled = false
	billboard.Parent = adornee
	billboard.ZIndex = 5 -- Garante visibilidade acima de outros elementos

	local distanceLabel = Instance.new("TextLabel")
	distanceLabel.Size = UDim2.new(1, 0, 1, 0)
	distanceLabel.BackgroundTransparency = 1
	distanceLabel.TextColor3 = Color3.fromRGB(255, 255, 0)
	distanceLabel.TextSize = 14
	distanceLabel.Text = "0 studs"
	distanceLabel.ZIndex = 6
	distanceLabel.Parent = billboard

	eggBillboards[egg] = billboard
	print("BillboardGui criado para ovo: " .. egg:GetFullName())

	egg.AncestryChanged:Connect(function()
		if not egg:IsDescendantOf(game) then
			billboard:Destroy()
			eggBillboards[egg] = nil
			print("BillboardGui destruído para ovo: " .. egg:GetFullName())
		end
	end)
end

-- Função para atualizar a visibilidade e distância dos ESPs
local function updateESP()
	if not espEnabled or not humanoidRootPart or not humanoidRootPart.Parent then
		for egg, billboard in pairs(eggBillboards) do
			billboard.Enabled = false
		end
		return
	end

	local playerPos = humanoidRootPart.Position
	for egg, billboard in pairs(eggBillboards) do
		if egg:IsDescendantOf(game) and billboard.Adornee then
			local eggPos
			if egg:IsA("BasePart") then
				eggPos = egg.Position
			else
				eggPos = egg.PrimaryPart and egg.PrimaryPart.Position or egg:GetPivot().Position
			end
			local distance = (playerPos - eggPos).Magnitude
			local distanceText = math.floor(distance) .. " studs"
			billboard.DistanceLabel.Text = distanceText
			billboard.Enabled = distance <= 100
		else
			billboard.Enabled = false
			if not egg:IsDescendantOf(game) then
				billboard:Destroy()
				eggBillboards[egg] = nil
			end
		end
	end
end

-- Função para inicializar o ESP
local function initializeESP()
	local eggPlacements = game.Workspace:FindFirstChild("EggPlacements")
	if not eggPlacements then
		warn("Pasta Workspace.EggPlacements não encontrada!")
		return
	end
	print("Pasta EggPlacements encontrada")

	for _, category in ipairs(categories) do
		local folder = eggPlacements:FindFirstChild(category)
		if folder then
			print("Processando categoria: " .. category)
			for _, egg in ipairs(folder:GetChildren()) do
				if egg:IsA("BasePart") or egg:IsA("Model") then
					createEggESP(egg)
				end
			end
			folder.ChildAdded:Connect(function(egg)
				if (egg:IsA("BasePart") or egg:IsA("Model")) then
					createEggESP(egg)
				end
			end)
		else
			warn("Categoria não encontrada: " .. category)
		end
	end
end

-- Função para alternar o ESP
local function toggleESP()
	espEnabled = not espEnabled
	espButton.BackgroundColor3 = espEnabled and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0)
	espButton.Text = espEnabled and "ESP: On" or "ESP: Off"
	print("ESP alternado: " .. (espEnabled and "On" or "Off"))

	if espEnabled then
		initializeESP()
		updateESP()
	end
end

-- Conecta o botão ESP
espButton.MouseButton1Click:Connect(toggleESP)

-- Atualiza o ESP em um loop
RunService.RenderStepped:Connect(function()
	if espEnabled then
		updateESP()
	end
end)

-- Função para teleportar para um ovo aleatório
local function teleportToRandomEgg()
	local eggPlacements = game.Workspace:FindFirstChild("EggPlacements")
	if not eggPlacements then
		warn("Pasta 'Workspace.EggPlacements' não encontrada!")
		return
	end

	local availableEggs = {}

	for _, category in ipairs(categories) do
		if categoryStates[category] then
			local folder = eggPlacements:FindFirstChild(category)
			if folder then
				for _, egg in ipairs(folder:GetChildren()) do
					if (egg:IsA("BasePart") or egg:IsA("Model")) and not visitedEggs[egg] then
						table.insert(availableEggs, egg)
					end
				end
			else
				warn("Pasta '" .. category .. "' não encontrada em EggPlacements!")
			end
		end
	end

	if #availableEggs == 0 then
		warn("Nenhum ovo disponível nas categorias selecionadas!")
		return
	end

	local randomIndex = math.random(1, #availableEggs)
	local selectedEgg = availableEggs[randomIndex]
	visitedEggs[selectedEgg] = true

	local eggPosition
	if selectedEgg:IsA("BasePart") then
		eggPosition = selectedEgg.Position
	else
		eggPosition = selectedEgg.PrimaryPart and selectedEgg.PrimaryPart.Position or selectedEgg:GetPivot().Position
	end
	local teleportPosition = eggPosition + Vector3.new(0, 10, 0)

	local humanoid = character:FindFirstChildOfClass("Humanoid")
	local isInVehicle = humanoid and humanoid.SeatPart and humanoid.SeatPart:IsA("VehicleSeat")

	if isInVehicle then
		local vehicle = humanoid.SeatPart:FindFirstAncestorOfClass("Model")
		if vehicle then
			if vehicle.PrimaryPart then
				vehicle:SetPrimaryPartCFrame(CFrame.new(teleportPosition))
			else
				local seatCFrame = humanoid.SeatPart.CFrame
				local targetCFrame = CFrame.new(teleportPosition)
				local offset = seatCFrame:ToObjectSpace(vehicle:GetPivot())
				vehicle:PivotTo(targetCFrame * offset)
				warn("Veículo sem PrimaryPart, usando fallback de PivotTo")
			end

			local anchoredParts = {}
			for _, part in ipairs(vehicle:GetDescendants()) do
				if part:IsA("BasePart") and not part.Anchored then
					part.Anchored = true
					table.insert(anchoredParts, part)
				end
			end
			print("Veículo teleportado e congelado")
			task.wait(2)

			for _, part in ipairs(anchoredParts) do
				if part:IsDescendantOf(game) then
					part.Anchored = false
				end
			end
			print("Veículo liberado")
		else
			warn("Modelo do veículo não encontrado!")
		end
	else
		if humanoidRootPart then
			humanoidRootPart.CFrame = CFrame.new(teleportPosition)
			humanoidRootPart.Anchored = true
			task.wait(2)
			humanoidRootPart.Anchored = false
			print("Jogador teleportado")
		else
			warn("HumanoidRootPart não encontrado para teleporte!")
		end
	end
end

-- Função para resetar os ovos visitados
local function resetVisitedEggs()
	visitedEggs = {}
	print("Ovos visitados resetados!")
end

-- Conecta as funções aos botões
findEggsButton.MouseButton1Click:Connect(teleportToRandomEgg)
resetButton.MouseButton1Click:Connect(resetVisitedEggs)

-- Função para alternar o menu
local function toggleMenu()
	menu.Visible = not menu.Visible
	button.Text = menu.Visible and "-" or "+"
	for _, child in ipairs(menu:GetChildren()) do
		if child:IsA("GuiObject") then
			child.Visible = true
		end
	end
	print("Menu alternado, visível: " .. tostring(menu.Visible))
end

-- Detecta clique no botão
button.MouseButton1Click:Connect(toggleMenu)

-- Funções para arrastar o botão
button.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		buttonDragging = true
		buttonDragStart = input.Position
		buttonStartPos = button.Position
	end
end)

button.InputChanged:Connect(function(input)
	if buttonDragging and input.UserInputType == Enum.UserInputType.MouseMovement then
		local delta = input.Position - buttonDragStart
		button.Position = UDim2.new(
			buttonStartPos.X.Scale,
			buttonStartPos.X.Offset + delta.X,
			buttonStartPos.Y.Scale,
			buttonStartPos.Y.Offset + delta.Y
		)
	end
end)

button.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		buttonDragging = false
	end
end)

-- Funções para arrastar o menu
menu.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		menuDragging = true
		menuDragStart = input.Position
		menuStartPos = menu.Position
	end
end)

menu.InputChanged:Connect(function(input)
	if menuDragging and input.UserInputType == Enum.UserInputType.MouseMovement then
		local delta = input.Position - menuDragStart
		menu.Position = UDim2.new(
			menuStartPos.X.Scale,
			menuStartPos.X.Offset + delta.X,
			menuStartPos.Y.Scale,
			menuStartPos.Y.Offset + delta.Y
		)
	end
end)

menu.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		menuDragging = false
	end
end)

-- Inicializa o ESP
initializeESP()
print("Script inicializado")
