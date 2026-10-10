-- =====================================================================
-- KYÔKA HUB • LOADER
-- Carrega as 3 partes (core + ui + logic)
-- =====================================================================

local LINKS = {
    core  = "https://raw.githubusercontent.com/shadowhubtertj/core.k/refs/heads/main/.lua",
    ui    = "https://raw.githubusercontent.com/shadowhubtertj/ui.k/refs/heads/main/k.lua",
    logic = "https://raw.githubusercontent.com/shadowhubtertj/logic.k/refs/heads/main/logic.lua",
}

local NOMES = { "core", "ui", "logic" }

print("[Kyôka] Carregando hub...")

local sucesso = 0
local falhou = 0

for _, nome in ipairs(NOMES) do
    local url = LINKS[nome]
    print("[Kyôka] Baixando " .. nome .. "...")

    -- Baixa
    local ok1, codigo = pcall(game.HttpGet, game, url)
    if not ok1 or not codigo or #codigo < 50 then
        warn("[Kyôka] Falha ao baixar " .. nome)
        falhou = falhou + 1
    else
        -- Compila
        local chunk, err = loadstring(codigo, "@kyoka/" .. nome .. ".lua")
        if not chunk then
            warn("[Kyôka] Erro de sintaxe em " .. nome .. ": " .. tostring(err))
            falhou = falhou + 1
        else
            -- Executa com pcall
            local ok2, err2 = pcall(chunk)
            if ok2 then
                print("[Kyôka] " .. nome .. " carregado")
                sucesso = sucesso + 1
            else
                warn("[Kyôka] Erro em " .. nome .. ": " .. tostring(err2))
                falhou = falhou + 1
            end
        end
    end

    task.wait(0.2)
end

print("[Kyôka] Carregadas: " .. sucesso .. "/3")
print("[Kyôka] Falharam: " .. falhou .. "/3")

if falhou == 0 then
    print("[Kyôka] HUB COMPLETO!")
else
    print("[Kyôka] Algumas partes falharam. Veja o F9 acima.")
end
