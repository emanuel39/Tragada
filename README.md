# Tragada-- Função para marcar os inimigos e aliados com cores
local function marcarJogadores()
    -- Para cada jogador no jogo
    for _, jogador in pairs(game.Players:GetChildren()) do
        -- Verifica se o jogador está em uma equipe inimiga ou aliada (exemplo simples)
        if jogador.Team and jogador.Team.Name == "Inimigos" then
            -- Marca o jogador de vermelho (inimigo)
            if jogador.Character then
                jogador.Character.HumanoidRootPart.BrickColor = BrickColor.Red()
            end
        elseif jogador.Team and jogador.Team.Name == "Aliados" then
            -- Marca o jogador de azul (aliado)
            if jogador.Character then
                jogador.Character.HumanoidRootPart.BrickColor = BrickColor.Blue()
            end
        end
    end
end

-- Função para disparar balas e garantir que acertam inimigos
local function dispararBala(alvo)
    -- Cria a bala
    local bala = Instance.new("Part")
    bala.Shape = Enum.PartType.Ball
    bala.Size = Vector3.new(1, 1, 1)
    bala.BrickColor = BrickColor.White()  -- Cor da bala
    bala.Position = game.Workspace.Player1.Character.HumanoidRootPart.Position -- Posição de disparo
    bala.Anchored = false
    bala.CanCollide = false
    bala.Parent = game.Workspace

    -- Aplica uma força para disparar a bala (simulando a direção do disparo)
    local direcao = (alvo.HumanoidRootPart.Position - bala.Position).unit
    local velocidade = 100 -- Velocidade da bala
    local corpo = Instance.new("BodyVelocity")
    corpo.Velocity = direcao * velocidade
    corpo.MaxForce = Vector3.new(5000, 5000, 5000) -- Força máxima para a bala
    corpo.Parent = bala

    -- Detecta quando a bala colide com algo
    bala.Touched:Connect(function(hit)
        -- Verifica se a bala acertou um inimigo
        if hit.Parent:FindFirstChild("Humanoid") and hit.Parent:FindFirstChild("Team") and hit.Parent.Team.Name == "Inimigos" then
            -- Se acertou o inimigo, pode aplicar dano ou alguma outra ação
            hit.Parent.Humanoid:TakeDamage(10)
            bala:Destroy() -- Destroi a bala
        end
    end)
end

-- Chama a função para marcar os jogadores
marcarJogadores()

-- Exemplo: disparar uma bala contra um inimigo específico
-- Isso poderia ser chamado dentro de um evento de disparo
game.ReplicatedStorage.Disparar.OnServerEvent:Connect(function(jogador, alvo)
    dispararBala(alvo)
end)
