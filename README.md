-- LocalScript dentro de StarterPlayerScripts

-- Aguarda o jogador carregar
local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- Cria um ScreenGui para o jogador
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DraggableButtonGui"
screenGui.Parent = playerGui

-- Cria o botão principal
local button = Instance.new("TextButton")
button.Size = UDim2.new(0, 40, 0, 40) -- Tamanho 40x40 pixels
button.Position = UDim2.new(0, 100, 0, 100) -- Posição inicial (x: 100, y: 100)
button.BackgroundColor3 = Color3.fromRGB(91, 91, 91) -- Cor de fundo cinza
button.Text = "+" -- Texto inicial
button.TextSize = 20
button.Parent = screenGui

-- Adiciona bordas arredondadas ao botão
local buttonCorner = Instance.new("UICorner")
buttonCorner.CornerRadius = UDim.new(0, 10)
buttonCorner.Parent = button

-- Cria o menu (inicialmente invisível)
local menu = Instance.new("Frame")
menu.Size = UDim2.new(0, 150, 0, 300) -- Aumentado para acomodar o ScrollingFrame
menu.Position = UDim2.new(0, 100, 0, 140) -- Posição inicial (abaixo do botão)
menu.BackgroundColor3 = Color3.fromRGB(70, 70, 70) -- Cor de fundo do menu
menu.Visible = false -- Começa escondido
menu.Parent = screenGui

-- Adiciona bordas arredondadas ao menu
local menuCorner = Instance.new("UICorner")
menuCorner.CornerRadius = UDim.new(0, 10)
menuCorner.Parent = menu

-- Cria a assinatura "By Maxx54" no fundo do menu
local signature = Instance.new("TextLabel")
signature.Size = UDim2.new(1, 0, 0, 20) -- Largura total do menu, altura 20 pixels
signature.Position = UDim2.new(0, 0, 1, -25) -- Posiciona no fundo do menu
signature.BackgroundTransparency = 1 -- Sem fundo
signature.Text = "By Maxx54"
signature.TextColor3 = Color3.fromRGB(99, 99, 99) -- Texto cinza
signature.TextSize = 10
signature.Parent = menu

-- Cria o botão "Find Eggs" dentro do menu
local findEggsButton = Instance.new("TextButton")
findEggsButton.Size = UDim2.new(0, 120, 0, 30) -- Tamanho do botão
findEggsButton.Position = UDim2.new(0, 15, 0, 15) -- Posição dentro do menu (15 pixels do topo)
findEggsButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0) -- Cor verde
findEggsButton.Text = "Find Eggs"
findEggsButton.TextColor3 = Color3.fromRGB(103, 103, 103) -- Texto cinza
findEggsButton.TextSize = 13
findEggsButton.Parent = menu

-- Adiciona bordas arredondadas ao botão "Find Eggs"
local findEggsCorner = Instance.new("UICorner")
findEggsCorner.CornerRadius = UDim.new(0, 8)
findEggsCorner.Parent = findEggsButton

-- Cria o botão "Reset" abaixo do "Find Eggs"
local resetButton = Instance.new("TextButton")
resetButton.Size = UDim2.new(0, 120, 0, 30) -- Tamanho do botão
resetButton.Position = UDim2.new(0, 15, 0, 50) -- Posição abaixo do "Find Eggs" (15 + 30 + 5 de espaço)
resetButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0) -- Cor vermelha
resetButton.Text = "Reset"
resetButton.TextColor3 = Color3.fromRGB(255, 255, 255) -- Texto branco
resetButton.TextSize = 13
resetButton.Parent = menu

-- Adiciona bordas arredondadas ao botão "Reset"
local resetCorner = Instance.new("UICorner")
resetCorner.CornerRadius = UDim.new(0, 8)
resetCorner.Parent = resetButton

-- Cria o ScrollingFrame para os botões de categoria
local scrollingFrame = Instance.new("ScrollingFrame")
scrollingFrame.Size = UDim2.new(0, 120, 0, 190) -- Tamanho do ScrollingFrame
scrollingFrame.Position = UDim2.new(0, 15, 0, 90) -- Abaixo do botão "Reset"
scrollingFrame.BackgroundTransparency = 1 -- Sem fundo
scrollingFrame.ScrollBarThickness = 5 -- Espessura da barra de rolagem
scrollingFrame.Parent = menu

-- Adiciona UIListLayout ao ScrollingFrame
local listLayout = Instance.new("UIListLayout")
listLayout.SortOrder = Enum.SortOrder.LayoutOrder
listLayout.Padding = UDim.new(0, 5) -- Espaçamento entre botões
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

-- Tabela para rastrear o estado das categorias (ativo/inativo)
local categoryStates = {}

-- Cria botões para cada categoria
for _, category in ipairs(categories) do
	local categoryButton = Instance.new("TextButton")
	categoryButton.Size = UDim2.new(0, 110, 0, 25) -- Tamanho do botão
	categoryButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0) -- Começa ativo (verde)
	categoryButton.Text = category
	categoryButton.TextColor3 = Color3.fromRGB(255, 255, 255) -- Texto branco
	categoryButton.TextSize = 12
	categoryButton.Parent = scrollingFrame

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

-- Ajusta o CanvasSize do ScrollingFrame com base no número de botões
scrollingFrame.CanvasSize = UDim2.new(0, 0, 0, #categories * 30)

-- Variáveis para controle de arrastar o botão
local buttonDragging = false
local buttonDragStart = nil
local buttonStartPos = nil

-- Variáveis para controle de arrastar o menu
local menuDragging = false
local menuDragStart = nil
local menuStartPos = nil

-- Tabela para rastrear ovos visitados
local visitedEggs = {}

-- Função para teleportar para um ovo aleatório e congelar no ar
local function teleportToRandomEgg()
	local eggPlacements = game.Workspace:FindFirstChild("EggPlacements")
	if not eggPlacements then
		warn("Pasta 'Workspace.EggPlacements' não encontrada!")
		return
	end

	local availableEggs = {}

	-- Coleta todos os ovos não visitados das categorias ativas
	for _, category in ipairs(categories) do
		if categoryStates[category] then -- Verifica se a categoria está ativa
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

	-- Verifica se há ovos disponíveis
	if #availableEggs == 0 then
		warn("Nenhum ovo disponível nas categorias selecionadas!")
		return
	end

	-- Escolhe um ovo aleatório
	local randomIndex = math.random(1, #availableEggs)
	local selectedEgg = availableEggs[randomIndex]

	-- Marca o ovo como visitado
	visitedEggs[selectedEgg] = true

	-- Calcula a posição 10 studs acima do ovo
	local eggPosition
	if selectedEgg:IsA("Model") then
		eggPosition = selectedEgg:GetPivot().Position -- Usa o pivô do modelo
	else
		eggPosition = selectedEgg.Position -- Usa a posição da parte
	end
	local teleportPosition = eggPosition + Vector3.new(0, 10, 0)

	-- Verifica se o jogador está em um carro
	local humanoid = character:FindFirstChildOfClass("Humanoid")
	local isInVehicle = humanoid and humanoid.SeatPart and humanoid.SeatPart:IsA("VehicleSeat")

	if isInVehicle then
		-- Teleporta o carro
		local vehicle = humanoid.SeatPart:FindFirstAncestorOfClass("Model")
		if vehicle then
			vehicle:SetPrimaryPartCFrame(CFrame.new(teleportPosition))
			-- Congela o carro no ar
			for _, part in ipairs(vehicle:GetDescendants()) do
				if part:IsA("BasePart") then
					part.Anchored = true
				end
			end
			task.wait(2) -- Espera 2 segundos
			-- Libera o carro
			for _, part in ipairs(vehicle:GetDescendants()) do
				if part:IsA("BasePart") then
					part.Anchored = false
				end
			end
		end
	else
		-- Teleporta apenas o jogador
		if humanoidRootPart then
			humanoidRootPart.CFrame = CFrame.new(teleportPosition)
			humanoidRootPart.Anchored = true -- Congela o jogador no ar
			task.wait(2) -- Espera 2 segundos
			humanoidRootPart.Anchored = false -- Libera para cair
		end
	end
end

-- Função para resetar os ovos visitados
local function resetVisitedEggs()
	visitedEggs = {} -- Limpa a tabela de ovos visitados
	print("Ovos visitados resetados!")
end

-- Conecta as funções aos botões
findEggsButton.MouseButton1Click:Connect(teleportToRandomEgg)
resetButton.MouseButton1Click:Connect(resetVisitedEggs)

-- Função para alternar o menu e o texto do botão
local function toggleMenu()
	menu.Visible = not menu.Visible
	button.Text = menu.Visible and "-" or "+"
end

-- Detecta clique no botão para abrir/fechar o menu
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
