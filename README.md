local player = game.Players.LocalPlayer
local gui = script.Parent
local frame = gui:WaitForChild("Frame")
local TeleportService = game:GetService("TeleportService")
local placeId = game.PlaceId
frame.Visible = false

-- Abrir/fechar com tecla M
game:GetService("UserInputService").InputBegan:Connect(function(input, gp)
    if input.KeyCode == Enum.KeyCode.M and not gp then
        frame.Visible = not frame.Visible
    end
end)

-- Velocidade Turbo
frame.ButtonVelocidade.MouseButton1Click:Connect(function()
    local char = player.Character or player.CharacterAdded:Wait()
    local hum = char:WaitForChild("Humanoid")
    hum.WalkSpeed = 100 -- padrão: 16
end)

-- Pulo Alto
frame.ButtonPulo.MouseButton1Click:Connect(function()
    local char = player.Character or player.CharacterAdded:Wait()
    local hum = char:WaitForChild("Humanoid")
    hum.JumpPower = 150 -- padrão: 50
end)

-- Teleporte
frame.ButtonTeleporte.MouseButton1Click:Connect(function()
    local char = player.Character or player.CharacterAdded:Wait()
    char:MoveTo(Vector3.new(0, 10, 0)) -- mudar para posição desejada
end)

-- Sair
frame.ButtonSair.MouseButton1Click:Connect(function()
    player:Kick("🚪 Você saiu do jogo!")
end)

-- Reentrar
frame.ButtonReentrar.MouseButton1Click:Connect(function()
    TeleportService:Teleport(placeId, player)
end)

-- Função Raio-X
local function aplicarRaioX(cores) -- cores = {"Vermelho"}, {"Azul"} ou {"Vermelho","Azul"}
    for _, jogador in pairs(game.Players:GetPlayers()) do
        if jogador ~= player and jogador.Character then
            local highlight = jogador.Character:FindFirstChild("RaioX")
            if not highlight then
                highlight = Instance.new("Highlight")
                highlight.Name = "RaioX"
                highlight.Adornee = jogador.Character
                highlight.FillTransparency = 0.5
                highlight.OutlineTransparency = 0
                highlight.Parent = jogador.Character
            end

            -- Define a cor dependendo do time e escolha
            if jogador.Team then
                if jogador.Team.Name == "Vermelho" and table.find(cores, "Vermelho") then
                    highlight.FillColor = Color3.fromRGB(255, 0, 0)
                    highlight.OutlineColor = Color3.fromRGB(255, 0, 0)
                    highlight.Enabled = true
                elseif jogador.Team.Name == "Azul" and table.find(cores, "Azul") then
                    highlight.FillColor = Color3.fromRGB(0, 0, 255)
                    highlight.OutlineColor = Color3.fromRGB(0, 0, 255)
                    highlight.Enabled = true
                else
                    highlight.Enabled = false
                end
            end
        end
    end
end

-- Botões Raio-X
frame.ButtonRaioX_Vermelho.MouseButton1Click:Connect(function()
    aplicarRaioX({"Vermelho"})
end)

frame.ButtonRaioX_Azul.MouseButton1Click:Connect(function()
    aplicarRaioX({"Azul"})
end)

frame.ButtonRaioX_Ambos.MouseButton1Click:Connect(function()
    aplicarRaioX({"Vermelho","Azul"})
end)

-- Atualiza Raio-X quando novos jogadores entrarem
game.Players.PlayerAdded:Connect(function(jogador)
    jogador.CharacterAdded:Connect(function()
        aplicarRaioX({"Vermelho","Azul"})
    end)
end)
