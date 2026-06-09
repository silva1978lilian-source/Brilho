-- Código completo para efeito de brilho dourado
-- Personagem fica envolto em luz dourada com partículas

-- Pega o jogador e o personagem
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- Cria o efeito de Bloom (brilho na tela)
local Lighting = game:GetService("Lighting")
local bloom = Instance.new("BloomEffect")
bloom.Parent = Lighting
bloom.Intensity = 1.2 -- Intensidade do brilho (ajuste conforme gostar)
bloom.Size = 32 -- Tamanho do bloom
bloom.Threshold = 0.3 -- Quanto brilho é necessário para ativar

-- Cria uma luz dourada seguindo o personagem
local pointLight = Instance.new("PointLight")
pointLight.Parent = humanoidRootPart
pointLight.Color = Color3.fromRGB(255, 200, 0) -- Dourado
pointLight.Range = 12
pointLight.Brightness = 3

-- Cria um anel de partículas douradas ao redor do personagem
local attachment = Instance.new("Attachment")
attachment.Parent = humanoidRootPart
attachment.Position = Vector3.new(0, 1, 0)

local particleEmitter = Instance.new("ParticleEmitter")
particleEmitter.Parent = attachment
particleEmitter.Texture = "rbxassetid://18292991634" -- Textura de estrela/brilho
particleEmitter.Color = ColorSequence.new(Color3.fromRGB(255, 215, 0)) -- Dourado
particleEmitter.Rate = 50
particleEmitter.Lifetime = NumberRange.new(1, 2)
particleEmitter.SpreadAngle = Vector2.new(360, 360)
particleEmitter.VelocityInheritance = 0
particleEmitter.Speed = NumberRange.new(2, 5)
particleEmitter.RotSpeed = NumberRange.new(-90, 90)
particleEmitter.Transparency = NumberSequence.new({
    NumberSequenceKeypoint.new(0, 1),
    NumberSequenceKeypoint.new(0.3, 0.7),
    NumberSequenceKeypoint.new(1, 1)
})
particleEmitter.Size = NumberSequence.new({
    NumberSequenceKeypoint.new(0, 0.5),
    NumberSequenceKeypoint.new(1, 0)
})
particleEmitter.Enabled = true

-- Opcional: adicionar um brilho dourado ao redor do personagem usando um Highlight
local highlight = Instance.new("Highlight")
highlight.Parent = character
highlight.FillColor = Color3.fromRGB(255, 200, 0)
highlight.FillTransparency = 0.6
highlight.OutlineColor = Color3.fromRGB(255, 215, 0)
highlight.OutlineTransparency = 0.3

print("✨ Efeito de brilho dourado ativado!")

-- Script de limpeza (para quando o personagem resetar)
player.CharacterAdded:Connect(function(newChar)
    character = newChar
    humanoidRootPart = character:WaitForChild("HumanoidRootPart")
    
    -- Recria os efeitos no novo personagem
    local newLight = pointLight:Clone()
    newLight.Parent = humanoidRootPart
    
    local newAttachment = Instance.new("Attachment")
    newAttachment.Parent = humanoidRootPart
    newAttachment.Position = Vector3.new(0, 1, 0)
    
    local newParticle = particleEmitter:Clone()
    newParticle.Parent = newAttachment
    
    local newHighlight = highlight:Clone()
    newHighlight.Parent = character
end)
