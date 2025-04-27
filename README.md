--[[ 
Script Celestial Ascension Android v3.0
Funciona mesmo se nada aparecer na tela
]]

-- CONFIGURAÇÕES (ALTERE COM SUAS COORDENADAS)
local coordAtaque = {x = 500, y = 1200}    -- Toque no ataque básico
local coordPocao = {x = 300, y = 1800}     -- Toque na poção de vida 
local coordBuff = {x = 700, y = 1800}      -- Toque no buff de ataque

-- TIMERS (em milissegundos)
local tempoFarm = 2500    -- A cada 2.5 segundos
local tempoBuff = 180000  -- A cada 3 minutos
local tempoCura = 1000    -- Verifica vida a cada 1s

-- NÃO ALTERAR ABAIXO
local ativo = false
local timerFarm, timerBuff, timerCura

-- FUNÇÃO PRINCIPAL
function main()
    if ativo then
        -- Rotina de ataque
        touchDown(coordAtaque.x, coordAtaque.y)
        sleep(math.random(80, 120))  -- Toque mais humano
        touchUp()
        
        -- Verificação de emergência
        if isScreenBlack() then      -- Previne bugs em loading
            stopScript()
        end
    end
end

-- SISTEMA AUTOMÁTICO DE CURA
function autoCura()
    local corPixel = getColor(coordPocao.x, coordPocao.y) -- Cor do ícone de poção
    
    -- Se o ícone estiver ativo (mudar para o valor HEX da cor)
    if corPixel == 0xFF0000 then     -- Substitua pela cor real do ícone
        touchDown(coordPocao.x, coordPocao.y)
        sleep(100)
        touchUp()
    end
end

-- ATIVAÇÃO/DESATIVAÇÃO
setKeyCallback("VOLUME_DOWN", function()  -- Usa botão físico
    ativo = not ativo
    toast(ativo and "🟢 SCRIPT ATIVADO" or "🔴 SCRIPT DESATIVADO")
    
    if ativo then
        timerFarm = setInterval(main, tempoFarm)
        timerBuff = setInterval(buffAuto, tempoBuff)
        timerCura = setInterval(autoCura, tempoCura)
    else
        clearInterval(timerFarm)
        clearInterval(timerBuff)
        clearInterval(timerCura)
    end
end)

-- MENSAGEM INICIAL
toast("Script pronto! Pressione VOLUME DOWN para ativar", 3000)
