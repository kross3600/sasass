-- Esse script deve ser um LocalScript dentro de StarterPlayerScripts ou StarterGui

-- Variáveis de personalização
local teamName = "Testers" -- Nome da equipe que pode ver o ESP
local espEnabled = false -- ESP está desabilitado inicialmente

-- Criando a interface (UI)
local player = game.Players.LocalPlayer
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Botão para ativar/desativar o ESP
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 200, 0, 50)
toggleButton.Position = UDim2.new(0.5, -100, 0.9, -25)
toggleButton.Text = "Ativar ESP"
toggleButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.Parent = screenGui

-- Função para ativar ou desativar o ESP
toggleButton.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    toggleButton.Text = espEnabled and "Desativar ESP" or "Ativar ESP"
    if not espEnabled then
        -- Limpar todos os ESPs ao desativar
        for _, player in pairs(game.Players:GetPlayers()) do
            if player.Character and player.Character:FindFirstChild("ESP") then
                player.Character.ESP:Destroy()
            end
        end
    end
end)

-- Função para aplicar o ESP aos jogadores
game.Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function(character)
        -- Só adiciona o ESP se o script estiver ativado e o jogador estiver na equipe certa
        if espEnabled and player.Team and player.Team.Name == teamName then
            -- Criando a Highlight para o jogador
            local esp = Instance.new("Highlight")
            esp.Parent = character
            esp.FillColor = Color3.fromRGB(255, 0, 0)  -- Cor do ESP (vermelho)
            esp.OutlineColor = Color3.fromRGB(0, 0, 0)  -- Cor do contorno
            esp.OutlineTransparency = 0  -- Transparência do contorno
            esp.FillTransparency = 0.5  -- Transparência do preenchimento
            esp.Name = "ESP"  -- Damos um nome para o Highlight, assim podemos removê-lo depois

            -- Adicionando a Highlight à parte do corpo do personagem
            for _, part in pairs(character:GetChildren()) do
                if part:IsA("BasePart") then
                    local partHighlight = esp:Clone()
                    partHighlight.Parent = part
                end
            end
        end
    end)
end)

-- Verifique se o jogador tem um personagem e adicione o ESP se necessário
for _, player in pairs(game.Players:GetPlayers()) do
    if player.Character then
        -- Se o jogador estiver na equipe certa e o ESP estiver ativado, aplicamos
        if espEnabled and player.Team and player.Team.Name == teamName then
            local esp = Instance.new("Highlight")
            esp.Parent = player.Character
            esp.FillColor = Color3.fromRGB(255, 0, 0)  -- Cor do ESP
            esp.OutlineColor = Color3.fromRGB(0, 0, 0)
            esp.OutlineTransparency = 0
            esp.FillTransparency = 0.5
            esp.Name = "ESP"

            -- Aplica o Highlight a todas as partes do corpo
            for _, part in pairs(player.Character:GetChildren()) do
                if part:IsA("BasePart") then
                    local partHighlight = esp:Clone()
                    partHighlight.Parent = part
                end
            end
        end
    end
end
