-- Script de ESP para jogadores
local function CreateESP(player)
    local function ApplyESP(character)
        if character:FindFirstChild("ESP") then
            character:FindFirstChild("ESP"):Destroy()
        end

        if character:FindFirstChild("Head") then
            -- Criar Highlight (Box)
            local highlight = Instance.new("Highlight")
            highlight.Name = "ESP"
            highlight.Adornee = character
            highlight.Parent = character
            highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
            highlight.FillColor = Color3.new(1, 1, 1)
            highlight.FillTransparency = 0.7
            highlight.OutlineColor = Color3.new(1, 1, 1)
            highlight.OutlineTransparency = 0

            -- Criar Billboard (Nome)
            local billboard = Instance.new("BillboardGui")
            billboard.Name = "NameESP"
            billboard.Adornee = character.Head
            billboard.Size = UDim2.new(0, 100, 0, 60)  -- Aumentando o tamanho para acomodar o HP
            billboard.StudsOffset = Vector3.new(0, 2, 0)
            billboard.AlwaysOnTop = true
            billboard.Parent = character.Head

            local nameLabel = Instance.new("TextLabel")
            nameLabel.Parent = billboard
            nameLabel.Size = UDim2.new(1, 0, 0.5, 0)  -- A metade do tamanho para o nome
            nameLabel.BackgroundTransparency = 1
            nameLabel.Text = player.Name
            nameLabel.TextColor3 = Color3.new(1, 1, 1)
            nameLabel.TextSize = 15
            nameLabel.Font = Enum.Font.SourceSansBold

            -- Criar TextLabel para mostrar HP
            local hpLabel = Instance.new("TextLabel")
            hpLabel.Parent = billboard
            hpLabel.Size = UDim2.new(1, 0, 0.5, 0)  -- A outra metade do tamanho para o HP
            hpLabel.Position = UDim2.new(0, 0, 0.5, 0)
            hpLabel.BackgroundTransparency = 1
            hpLabel.TextColor3 = Color3.new(1, 0, 0)  -- Cor do texto de HP (vermelho)
            hpLabel.TextSize = 15
            hpLabel.Font = Enum.Font.SourceSans
            hpLabel.Text = "HP: " .. math.floor(character:FindFirstChild("Humanoid").Health) .. " / " .. character:FindFirstChild("Humanoid").MaxHealth

            -- Atualizar o HP enquanto o jogador estiver vivo
            character:WaitForChild("Humanoid").HealthChanged:Connect(function()
                hpLabel.Text = "HP: " .. math.floor(character:FindFirstChild("Humanoid").Health) .. " / " .. character:FindFirstChild("Humanoid").MaxHealth
            end)
        end
    end

    player.CharacterAdded:Connect(function(character)
        character:WaitForChild("Head")
        ApplyESP(character)
    end)

    if player.Character then
        ApplyESP(player.Character)
    end
end

-- Função para remover ESP
local function RemoveESP(player)
    if player.Character then
        local char = player.Character
        if char:FindFirstChild("ESP") then
            char:FindFirstChild("ESP"):Destroy()
        end
        if char:FindFirstChild("Head") and char.Head:FindFirstChild("NameESP") then
            char.Head:FindFirstChild("NameESP"):Destroy()
        end
    end
end

-- Função para ativar ESP para todos os jogadores
local function EnableESP()
    for _, player in pairs(game:GetService("Players"):GetPlayers()) do
        if player ~= game:GetService("Players").LocalPlayer then
            CreateESP(player)
        end
    end

    game:GetService("Players").PlayerAdded:Connect(function(player)
        CreateESP(player)
    end)

    game:GetService("Players").PlayerRemoving:Connect(function(player)
        RemoveESP(player)
    end)
end

-- Ativando o ESP
EnableESP()

-- Script de Velocidade (para integrar com a interface e a ativação do ESP)
_G.SpeedEnabled = false
_G.WS = 57  -- Velocidade máxima

local function SetSpeed()
    local player = game.Players.LocalPlayer
    local humanoid = player.Character and player.Character:FindFirstChildWhichIsA("Humanoid")
    if humanoid then
        if _G.SpeedEnabled then
            humanoid.WalkSpeed = _G.WS
            humanoid:GetPropertyChangedSignal("WalkSpeed"):Connect(function()
                if _G.SpeedEnabled then
                    humanoid.WalkSpeed = _G.WS
                end
            end)
        else
            humanoid.WalkSpeed = 16  -- Velocidade padrão
        end
    end
end

-- Listener para reaparecimento do personagem
local Player = game.Players.LocalPlayer
Player.CharacterAdded:Connect(function(Character)
    Character:WaitForChild("Humanoid")
    SetSpeed()  -- Aplica a velocidade assim que o personagem é adicionado
end)

if Player.Character then
    Player.Character:WaitForChild("Humanoid")
    SetSpeed()  -- Inicializa a velocidade para o personagem atual
end

-- Função para alternar a ativação da velocidade com a UI
local function CreateUI()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Parent = game.Players.LocalPlayer.PlayerGui

    local frame = Instance.new("Frame")
    frame.Parent = screenGui
    frame.Size = UDim2.new(0, 200, 0, 100)
    frame.Position = UDim2.new(0, 10, 0, 10)
    frame.BackgroundTransparency = 1

    local toggleButton = Instance.new("TextButton")
    toggleButton.Parent = frame
    toggleButton.Size = UDim2.new(0, 180, 0, 50)
    toggleButton.Position = UDim2.new(0, 10, 0, 10)
    toggleButton.Text = "Ativar Velocidade"
    toggleButton.BackgroundColor3 = Color3.fromRGB(0, 0, 0)

    local statusLabel = Instance.new("TextLabel")
    statusLabel.Parent = frame
    statusLabel.Size = UDim2.new(0, 180, 0, 30)
    statusLabel.Position = UDim2.new(0, 10, 0, 60)
    statusLabel.Text = "Status: Desativado"
    statusLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    statusLabel.BackgroundTransparency = 1

    toggleButton.MouseButton1Click:Connect(function()
        if _G.SpeedEnabled then
            _G.SpeedEnabled = false
            statusLabel.Text = "Status: Desativado"
            toggleButton.Text = "Ativar Velocidade"
        else
            _G.SpeedEnabled = true
            statusLabel.Text = "Status: Ativado"
            toggleButton.Text = "Desativar Velocidade"
        end
        SetSpeed()
    end)
end

CreateUI()
