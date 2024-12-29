local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")

-- Função para desenhar as linhas conectando o jogador a outros
local function drawLine(from, to)
    -- Cria uma nova instância de Line
    local line = Drawing.new("Line")
    line.Color = Color3.fromRGB(255, 255, 255)  -- Cor branca
    line.Thickness = 2  -- Espessura da linha
    line.Transparency = 1  -- Totalmente opaca (não transparente)
    line.ZIndex = 10  -- Ordem da camada (acima da interface)

    -- Atualiza a linha a cada frame
    RunService.RenderStepped:Connect(function()
        -- Converte as posições dos mundos para coordenadas da tela
        local fromPos = from:ToScreenSpace()
        local toPos = to:ToScreenSpace()

        -- Define as posições da linha
        line.From = fromPos
        line.To = toPos
    end)

    return line
end

-- Atualiza constantemente para desenhar linhas para todos os jogadores
RunService.Heartbeat:Connect(function()
    -- Desenha linhas para cada jogador no jogo
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
            local head = player.Character.Head
            local playerHeadPosition = head.Position
            local myHeadPosition = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Head") and LocalPlayer.Character.Head.Position

            if myHeadPosition then
                -- Chama a função para desenhar a linha
                drawLine(game.Workspace.CurrentCamera.CFrame.Position, player.Character.Head.Position)
            end
        end
    end
end)
