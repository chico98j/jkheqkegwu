local deathBall = script.Parent  -- A bola
local pushForce = 200  -- Força que será aplicada para empurrar a bola

-- Função para empurrar a bola
local function pushBall(player)
    -- Detectar o ponto de colisão (local onde o jogador tocou a bola)
    local character = player.Character
    if character and character:FindFirstChild("HumanoidRootPart") then
        local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
        
        -- Calcular a direção para empurrar a bola para longe do jogador
        local direction = (deathBall.Position - humanoidRootPart.Position).unit
        
        -- Aplicar a força na bola para empurrá-la
        local bodyVelocity = deathBall:FindFirstChild("BodyVelocity")
        if not bodyVelocity then
            bodyVelocity = Instance.new("BodyVelocity")
            bodyVelocity.MaxForce = Vector3.new(100000, 100000, 100000)
            bodyVelocity.Parent = deathBall
        end
        
        bodyVelocity.Velocity = direction * pushForce  -- Empurrar a bola
    end
end

-- Conectar a função ao evento Touched
deathBall.Touched:Connect(function(hit)
    local character = hit.Parent
    if character:IsA("Model") and character:FindFirstChild("Humanoid") then
        local player = game:GetService("Players"):GetPlayerFromCharacter(character)
        if player then
            pushBall(player)
        end
    end
end)
