-- Ferrari Hub - Script de Verificação com Tela de Carregamento, Imagem e Créditos
-- Criado por Rafa/Leo
-- Versão: v1.0

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

-- GUI principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "FerrariVerificacaoGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game:GetService("CoreGui")

-- Tela de carregamento
local LoadingFrame = Instance.new("Frame")
LoadingFrame.Size = UDim2.new(1, 0, 1, 0) -- Tela cheia
LoadingFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
LoadingFrame.BackgroundTransparency = 0
LoadingFrame.Parent = ScreenGui

-- Imagem da Ferrari
local FerrariImage = Instance.new("ImageLabel")
FerrariImage.Size = UDim2.new(0, 500, 0, 250) -- Maior
FerrariImage.Position = UDim2.new(0.5, -250, 0.3, -125)
FerrariImage.BackgroundTransparency = 1
FerrariImage.Image = "rbxassetid://INSERT_ID_DA_FERRARI_AQUI" -- Substitua pelo AssetID
FerrariImage.Parent = LoadingFrame

-- Texto de carregamento
local LoadingText = Instance.new("TextLabel")
LoadingText.Size = UDim2.new(0, 500, 0, 50)
LoadingText.Position = UDim2.new(0.5, -250, 0.65, 0)
LoadingText.BackgroundTransparency = 1
LoadingText.TextColor3 = Color3.fromRGB(255, 255, 0)
LoadingText.Font = Enum.Font.SourceSansBold
LoadingText.TextSize = 28
LoadingText.Text = "Carregando Ferrari Verificação..."
LoadingText.Parent = LoadingFrame

-- Texto da versão e créditos
local VersionText = Instance.new("TextLabel")
VersionText.Size = UDim2.new(0, 400, 0, 30)
VersionText.Position = UDim2.new(0.5, -200, 0.75, 0)
VersionText.BackgroundTransparency = 1
VersionText.TextColor3 = Color3.fromRGB(255, 255, 0)
VersionText.Font = Enum.Font.SourceSansBold
VersionText.TextSize = 22
VersionText.Text = "v1.0 | Criado por Leo/Rafa"
VersionText.Parent = LoadingFrame

-- Função para animação de carregamento
local carregamentoCompleto = false
spawn(function()
    local barra = ""
    while not carregamentoCompleto do
        barra = barra .. "█"
        LoadingText.Text = "Carregando Ferrari Verificação... " .. barra
        wait(0.3)
        if #barra > 10 then
            barra = ""
        end
    end
end)

-- Espera 10s para carregar
delay(10, function()
    carregamentoCompleto = true
    LoadingFrame:Destroy() -- Remove tela de carregamento
    iniciarVerificacao()  -- Ativa verificação
end)

-- Lista de mensagens atuais
local mensagensAtuais = {}

-- Função para criar mensagens temporárias
function criarMensagem(texto)
    local mensagem = Instance.new("TextLabel")
    mensagem.Size = UDim2.new(0, 450, 0, 40)
    mensagem.Position = UDim2.new(0.5, -225, 0, 50)
    mensagem.BackgroundTransparency = 0.3
    mensagem.BackgroundColor3 = Color3.fromRGB(255, 255, 0)
    mensagem.TextColor3 = Color3.fromRGB(0, 0, 0)
    mensagem.Font = Enum.Font.SourceSansBold
    mensagem.TextSize = 22
    mensagem.Text = texto
    mensagem.Parent = ScreenGui

    table.insert(mensagensAtuais, mensagem)

    for i, v in ipairs(mensagensAtuais) do
        v.Position = UDim2.new(0.5, -225, 0, 50 + (i-1)*45)
    end

    delay(6, function()
        mensagem:Destroy()
        for i, v in ipairs(mensagensAtuais) do
            if v == mensagem then
                table.remove(mensagensAtuais, i)
                break
            end
        end
        for i, v in ipairs(mensagensAtuais) do
            v.Position = UDim2.new(0.5, -225, 0, 50 + (i-1)*45)
        end
    end)
end

-- Função principal de verificação
function iniciarVerificacao()
    -- Mensagem inicial
    criarMensagem("Ferrari Verificação Ativo")

    local function onPlayerAdded(player)
        criarMensagem(player.Name .. " entrou")

        player.CharacterAdded:Connect(function(char)
            criarMensagem(player.Name .. " regenerou")
        end)

        -- Exemplos fictícios de ações
        -- criarMensagem(player.Name .. " quebrou regras de spawn")
        -- criarMensagem(player.Name .. " web namoro")
        -- criarMensagem(player.Name .. " fingindo ser famoso")
        -- criarMensagem(player.Name .. " com exploit")
    end

    local function onPlayerRemoving(player)
        criarMensagem(player.Name .. " saiu")
    end

    Players.PlayerAdded:Connect(onPlayerAdded)
    Players.PlayerRemoving:Connect(onPlayerRemoving)

    for _, player in pairs(Players:GetPlayers()) do
        onPlayerAdded(player)
    end
end
