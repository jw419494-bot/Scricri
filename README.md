--[[
â•”â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•—
â•‘  GARDEN TOWER DEFENSE - SCRIPT DE TROCA VISUAL (Trade UI)   â•‘
â•‘  VERSÃƒO COM PAINEL DE CONTROLE (ATIVAR/DESATIVAR)           â•‘
â•šâ•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
]]--

-- ============================================================
-- CONFIGURAÃ‡Ã•ES
-- ============================================================

local MapaItens = {
    ["Tomate"]            = "InfecÃ§Ã£o",
    ["Cacto"]             = "TNT",
    ["Cebola"]            = "Veneno",
    ["RomÃ£"]              = "Bomba RelÃ³gio",
    ["Rabanete"]          = "Mina Terrestre",
    ["Sawflower"]         = "Cortador de Fios",
    ["Repolho"]           = "Escudo Quebrado",
    ["Vinhas"]            = "Teia de Aranha",
    ["Lenhador"]          = "Motosserra Enferrujada",
    ["Estilingue"]        = "Pedra Quebrada",
    ["Laser Plant"]       = "LÃ¢mpada Queimada",
    ["Money Tree"]        = "Folha de Papel",
    ["Ghost Pepper"]      = "Ar Frio",
    ["Venus Flytrap"]     = "Mosca Morta",
    ["Potato"]            = "Batata Apodrecida",
    ["default"]           = "Item Vazio"
}

-- ============================================================
-- VARIÃVEIS DE ESTADO
-- ============================================================

local ScriptAtivo = false
local MonitoramentoIniciado = false
local InterceptacaoIniciada = false

local Player = game:GetService("Players").LocalPlayer
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- ============================================================
-- FUNÃ‡Ã•ES DE CONTROLE (ATIVAR/DESATIVAR)
-- ============================================================

function AtivarScript()
    if ScriptAtivo then return end
    ScriptAtivo = true
    print("[Trade Script] SCRIPT ATIVADO!")
    
    if not MonitoramentoIniciado then
        IniciarMonitoramento()
    end
    
    if not InterceptacaoIniciada then
        IniciarInterceptacao()
    end
    
    AtualizarPainel()
end

function DesativarScript()
    if not ScriptAtivo then return end
    ScriptAtivo = false
    print("[Trade Script] SCRIPT DESATIVADO!")
    
    AtualizarPainel()
end

-- ============================================================
-- CRIAÃ‡ÃƒO DO PAINEL GUI
-- ============================================================

local function CriarPainel()
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "TradeScamPanel"
    ScreenGui.Parent = Player.PlayerGui
    ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    
    local Frame = Instance.new("Frame")
    Frame.Name = "MainFrame"
    Frame.Size = UDim2.new(0, 250, 0, 80)
    Frame.Position = UDim2.new(1, -260, 0, 10)
    Frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    Frame.BackgroundTransparency = 0.1
    Frame.BorderSizePixel = 0
    Frame.ClipsDescendants = true
    Frame.Parent = ScreenGui
    
    -- Efeito de borda
    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 8)
    Corner.Parent = Frame
    
    local Stroke = Instance.new("UIStroke")
    Stroke.Color = Color3.fromRGB(0, 150, 0)
    Stroke.Thickness = 1
    Stroke.Parent = Frame
    
    -- TÃ­tulo
    local Title = Instance.new("TextLabel")
    Title.Size = UDim2.new(1, 0, 0, 30)
    Title.BackgroundTransparency = 1
    Title.Text = "GTD Trade Manipulator"
    Title.TextColor3 = Color3.fromRGB(0, 200, 0)
    Title.TextSize = 18
    Title.Font = Enum.Font.GothamBold
    Title.Parent = Frame
    
    -- BotÃ£o de Ativar/Desativar
    local ToggleBtn = Instance.new("TextButton")
    ToggleBtn.Name = "ToggleBtn"
    ToggleBtn.Size = UDim2.new(1, -20, 0, 35)
    ToggleBtn.Position = UDim2.new(0, 10, 0, 35)
    ToggleBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
    ToggleBtn.Text = "ATIVAR"
    ToggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    ToggleBtn.TextSize = 16
    ToggleBtn.Font = Enum.Font.GothamBold
    ToggleBtn.BorderSizePixel = 0
    ToggleBtn.Parent = Frame
    
    local BtnCorner = Instance.new("UICorner")
    BtnCorner.CornerRadius = UDim.new(0, 4)
    BtnCorner.Parent = ToggleBtn
    
    -- LÃ³gica do botÃ£o
    ToggleBtn.MouseButton1Click:Connect(function()
        if ScriptAtivo then
            DesativarScript()
            ToggleBtn.Text = "ATIVAR"
            ToggleBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
        else
            AtivarScript()
            ToggleBtn.Text = "DESATIVAR"
            ToggleBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
        end
    end)
end

local function AtualizarPainel()
    local gui = Player.PlayerGui:FindFirstChild("TradeScamPanel")
    if gui and gui:FindFirstChild("MainFrame") then
        local btn = gui.MainFrame:FindFirstChild("ToggleBtn")
        if btn then
            if ScriptAtivo then
                btn.Text = "DESATIVAR"
                btn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
            else
                btn.Text = "ATIVAR"
                btn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
            end
        end
    end
end

-- ============================================================
-- LÃ“GICA DO SCRIPT
-- ============================================================

local function AlterarItemNoSlot(slot, itemEncontrado)
    if not ScriptAtivo then return false end
    
    for _, item in ipairs(slot:GetChildren()) do
        if item:IsA("TextLabel") or item:IsA("ImageLabel") then
            if item.Text == itemEncontrado or item.Name == itemEncontrado then
                local overlay = Instance.new("TextLabel")
                overlay.Name = "VisualOverlay"
                overlay.Size = UDim2.new(1, 0, 1, 0)
                overlay.Position = UDim2.new(0, 0, 0, 0)
                overlay.BackgroundTransparency = 0.2
                overlay.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
                overlay.Text = MapaItens[itemEncontrado] or MapaItens["default"]
                overlay.TextSize = 18
                overlay.TextColor3 = Color3.fromRGB(255, 255, 255)
                overlay.TextStrokeTransparency = 0.5
                overlay.Font = Enum.Font.GothamBold
                overlay.Parent = item
                
                print("[Trade] Item substituÃ­do: " .. itemEncontrado .. " â†’ " .. overlay.Text)
                return true
            end
        end
    end
    return false
end

local function IniciarMonitoramento()
    MonitoramentoIniciado = true
    
    local tentativa = 0
    local maxTentativas = 30
    
    while tentativa < maxTentativas do
        local gui = Player:FindFirstChild("PlayerGui")
        if gui then
            local nomesPossiveis = {"TradeGUI", "TradeFrame", "TradingUI", "TradeMenu"}
            for _, nome in ipairs(nomesPossiveis) do
                local found = gui:FindFirstChild(nome)
                if found then
                    print("[Trade] UI de trade encontrada: " .. found.Name)
                    MonitorarTradeUI(found)
                    return
                end
            end
        end
        tentativa = tentativa + 1
        wait(0.5)
    end
    
    print("[Trade] UI de trade nÃ£o encontrada. Tentando interceptaÃ§Ã£o de eventos...")
end

local function MonitorarTradeUI(tradeFrame)
    local seuSlot = nil
    local outroSlot = nil
    
    for _, child in ipairs(tradeFrame:GetChildren()) do
        if child:IsA("Frame") or child:IsA("ScrollingFrame") then
            if string.find(child.Name, "Your") or string.find(child.Name, "My") then
                seuSlot = child
            elseif string.find(child.Name, "Their") or string.find(child.Name, "Other") then
                outroSlot = child
            end
        end
    end
    
    if seuSlot then
        seuSlot.ChildAdded:Connect(function(child)
            if not ScriptAtivo then return end
            if child:IsA("TextLabel") or child:IsA("ImageLabel") then
                if child.Text ~= "" then
                    local itemNovo = MapaItens[child.Text] or MapaItens["default"]
                    print(string.format("[Trade] %s â†’ O outro vÃª: %s", child.Text, itemNovo))
                    
                    if outroSlot then
                        local novoItem = Instance.new("TextLabel")
                        novoItem.Name = itemNovo
                        novoItem.Size = UDim2.new(1, -10, 0, 40)
                        novoItem.Position = UDim2.new(0, 5, 0, 5)
                        novoItem.BackgroundTransparency = 0.2
                        novoItem.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
                        novoItem.Text = itemNovo
                        novoItem.TextSize = 16
                        novoItem.TextColor3 = Color3.fromRGB(255, 200, 200)
                        novoItem.Font = Enum.Font.GothamBold
                        novoItem.TextStrokeTransparency = 0.7
                        novoItem.Parent = outroSlot
                    end
                end
            end
        end)
    end
end

local function IniciarInterceptacao()
    InterceptacaoIniciada = true
    
    local function BuscarRemotes(parent, profundidade)
        if profundidade <= 0 or not ScriptAtivo then return end
        
        for _, child in ipairs(parent:GetChildren()) do
            local isTradeRemote = false
            local nomesRemotes = {"TradeItem", "AddTradeItem", "RequestTrade", "SendTrade"}
            
            for _, nome in ipairs(nomesRemotes) do
                if string.find(child.Name, nome, 1, true) then
                    isTradeRemote = true
                    break
                end
            end
            
            if isTradeRemote and child:IsA("RemoteEvent") then
                child.OnClientEvent:Connect(function(...)
                    if not ScriptAtivo then return end
                    
                    local args = {...}
                    for i, arg in ipairs(args) do
                        if type(arg) == "string" then
                            if MapaItens[arg] then
                                args[i] = MapaItens[arg]
                                print("[Trade Intercept] Item alterado de '" .. arg .. "' para '" .. MapaItens[arg] .. "'")
                            end
                        end
                    end
                end)
            end
            
            BuscarRemotes(child, profundidade - 1)
        end
    end
    
    BuscarRemotes(ReplicatedStorage, 5)
end

-- ============================================================
-- INICIALIZAÃ‡ÃƒO
-- ============================================================

wait(2) -- Aguarda o jogo carregar
CriarPainel()
print("[Trade Script] Painel de controle criado no canto superior direito.")
print("[Trade Script] Clique em 'ATIVAR' para iniciar o script.")
