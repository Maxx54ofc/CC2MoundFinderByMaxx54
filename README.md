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
menu.Size = UDim2.new(0, 150, 0, 200) -- Tamanho do menu
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

-- Cria o botão "Find Mounds" dentro do menu
local findMoundsButton = Instance.new("TextButton")
findMoundsButton.Size = UDim2.new(0, 120, 0, 30) -- Tamanho do botão
findMoundsButton.Position = UDim2.new(0, 15, 0, 15) -- Posição dentro do menu (15 pixels do topo)
findMoundsButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0) -- Cor verde
findMoundsButton.Text = "Find Mounds"
findMoundsButton.TextColor3 = Color3.fromRGB(103, 103, 103) -- Texto cinza
findMoundsButton.TextSize = 13
findMoundsButton.Parent = menu

-- Adiciona bordas arredondadas ao botão "Find Mounds"
local findMoundsCorner = Instance.new("UICorner")
findMoundsCorner.CornerRadius = UDim.new(0, 8)
findMoundsCorner.Parent = findMoundsButton

-- Cria o botão "Reset" abaixo do "Find Mounds"
local resetButton = Instance.new("TextButton")
resetButton.Size = UDim2.new(0, 120, 0, 30) -- Tamanho do botão
resetButton.Position = UDim2.new(0, 15, 0, 50) -- Posição abaixo do "Find Mounds" (15 + 30 + 5 de espaço)
resetButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0) -- Cor vermelha
resetButton.Text = "Reset"
resetButton.TextColor3 = Color3.fromRGB(255, 255, 255) -- Texto branco
resetButton.TextSize = 13
resetButton.Parent = menu

-- Adiciona bordas arredondadas ao botão "Reset"
local resetCorner = Instance.new("UICorner")
resetCorner.CornerRadius = UDim.new(0, 8)
resetCorner.Parent = resetButton

-- Variáveis para controle de arrastar o botão
local buttonDragging = false
local buttonDragStart = nil
local buttonStartPos = nil

-- Variáveis para controle de arrastar o menu
local menuDragging = false
local menuDragStart = nil
local menuStartPos = nil

-- Tabela para rastrear Mounds visitados
local visitedMounds = {}

-- Função para teleportar para um Mound aleatório e congelar no ar
local function teleportToRandomMound()
	local moundsFolder = game.Workspace:FindFirstChild("DirtEventContent") and game.Workspace.DirtEventContent:FindFirstChild("Mounds")
	if not moundsFolder then
		warn("Pasta 'Workspace.DirtEventContent.Mounds' não encontrada!")
		return
	end

	local mounds = moundsFolder:GetChildren()
	local availableMounds = {}

	-- Filtra Mounds não visitados
	for _, mound in pairs(mounds) do
		if mound:IsA("BasePart") or mound:IsA("Model") then
			if not visitedMounds[mound] then
				table.insert(availableMounds, mound)
			end
		end
	end

	-- Verifica se há Mounds disponíveis
	if #availableMounds == 0 then
		warn("Todos os Mounds já foram visitados!")
		return
	end

	-- Escolhe um Mound aleatório
	local randomIndex = math.random(1, #availableMounds)
	local selectedMound = availableMounds[randomIndex]

	-- Marca o Mound como visitado
	visitedMounds[selectedMound] = true

	-- Calcula a posição 10 studs acima do Mound
	local moundPosition
	if selectedMound:IsA("Model") then
		moundPosition = selectedMound:GetPivot().Position -- Usa o pivô do modelo
	else
		moundPosition = selectedMound.Position -- Usa a posição da parte
	end
	local teleportPosition = moundPosition + Vector3.new(0, 10, 0)

	-- Teleporta o jogador e congela no ar
	if humanoidRootPart then
		humanoidRootPart.CFrame = CFrame.new(teleportPosition)
		humanoidRootPart.Anchored = true -- Congela o jogador no ar
		task.wait(2) -- Espera 2 segundos
		humanoidRootPart.Anchored = false -- Libera para cair
	end
end

-- Função para resetar os Mounds visitados
local function resetVisitedMounds()
	visitedMounds = {} -- Limpa a tabela de Mounds visitados
	print("Mounds visitados resetados!")
end

-- Conecta as funções aos botões
findMoundsButton.MouseButton1Click:Connect(teleportToRandomMound)
resetButton.MouseButton1Click:Connect(resetVisitedMounds)

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
