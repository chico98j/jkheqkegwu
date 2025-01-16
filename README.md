-- Script Local para Defesa Automática (StarterCharacterScripts)
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local deathBall = workspace:WaitForChild("DeathBall")  -- Referência à bola no workspace
local humanoid = character:WaitForChild("Humanoid")
local defendRange = 15  -- Distância máxima para tentar defender a bola
local defenseForce = 300  -- Força para empurrar a bola
local moveSpeed = 20  -- Velocidade do movimento do personagem

-- Função para mover o personagem em direção à bola
local function moveToBall()
    while true do
        local distanceToBall = (deathBall.Position - humanoidRootPart.Position).Magnitude
        
        -- Verificar se o personagem está perto o suficiente da bola para defender
        if distanceToBall <= defendRange then
            -- Calcular a direção em que o personagem deve se mover (em direção à bola)
            local direction = (deathBall.Position - humanoidRootPart.Position).unit
            humanoid:MoveTo(deathBall.Position - direction * 2)  -- Mover o personagem para perto da bola, mas não sobrepor completamente
            
            -- Calcular a direção para empurrar a bola
            local bodyVelocity = deathBall:FindFirstChild("BodyVelocity")
            
            -- Criar ou obter o BodyVelocity para empurrar a bola
            if not bodyVelocity then
                bodyVelocity = Instance.new("BodyVelocity")
                bodyVelocity.MaxForce = Vector3.new(100000, 100000, 100000)
                bodyVelocity.Parent = deathBall
            end
            
            -- Aplicar a força para desviar a bola
            bodyVelocity.Velocity = (deathBall.Position - humanoidRootPart.Position).unit * defenseForce
        end
        
        -- Aguardar um pouco antes de calcular novamente
        wait(0.1)
    end
end

-- Iniciar a defesa automática
spawn(moveToBall)
