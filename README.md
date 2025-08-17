local player = game.Players.LocalPlayer
local gui = script.Parent
local frame = gui:WaitForChild("Frame")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TeleportService = game:GetService("TeleportService")
local placeId = game.PlaceId

-- Menu invisível no começo
frame.Visible = false

-- Abrir/fechar com tecla M
local UIS = game:GetService("UserInputService")
UIS.InputBegan:Connect(function(input, gp)
    if input.KeyCode == Enum.KeyCode.M and not gp then
        frame.Visible = not frame.Visible
    end
end)

-- Função segura para pegar leaderstats Moedas
local function getMoedas(p)
    local ls = p:FindFirstChild("leaderstats")
    if ls then
        return ls:FindFirstChild("Moedas")
    end
end

-- Botão Moedas Infinitas
frame.ButtonMoedas.MouseButton1Click:Connect(function()
    local moedas = getMoedas(player)
    if moedas then
        moedas.Value = math.huge
        print("💰 Moedas infinitas ativadas!")
    end
end)

-- Botão Frontman
frame.ButtonFrontman.MouseButton1Click:Connect(function()
    local frontman = game.Workspace:FindFirstChild("Frontman")
    if frontman and frontman:FindFirstChild("Head") then
        if not frontman:FindFirstChild("ClickDetector") then
            local cd = Instance.new("ClickDetector")
            cd.Parent = frontman.Head
            cd.MouseClick:Connect(function(p)
                local moedas = getMoedas(p)
                if moedas then
                    moedas.Value = math.huge
                end
            end)
        end
    end
end)

-- Velocidade Turbo
frame.ButtonVelocidade.MouseButton1Click:Connect(function()
    local char = player.Character or player.CharacterAdded:Wait()
    local hum = char:WaitForChild("Humanoid")
    hum.WalkSpeed = 100
end)

-- Pulo Alto
frame.ButtonPulo.MouseButton1Click:Connect(function()
    local char = player.Character or player.CharacterAdded:Wait()
    local hum = char:WaitForChild("Humanoid")
    hum.JumpPower = 150
end)

-- Cabo de Guerra
frame.ButtonCabo.MouseButton1Click:Connect(function()
    if ReplicatedStorage:FindFirstChild("ForceWin") then
        ReplicatedStorage.ForceWin:FireServer("Azul")
    end
end)

-- Sair
frame.ButtonSair.MouseButton1Click:Connect(function()
    player:Kick("🚪 Você saiu do jogo!")
end)

-- Reentrar
frame.ButtonReentrar.MouseButton1Click:Connect(function()
    TeleportService:Teleport(placeId, player)
end)
