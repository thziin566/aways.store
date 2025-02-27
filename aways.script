-- Script (ServerScriptService)

local DataStoreService = game:GetService("DataStoreService")
local staffDataStore = DataStoreService:GetDataStore("StaffList")
local staffList = {} -- Tabela para guardar os UsersIds

local function carregarStaff()
    local success, errorMessage = pcall(function()
       staffList = staffDataStore:GetAsync("staff") or {}
    end)

    if not success then
        warn("Erro ao carregar os Staffs" .. errorMessage)
    end
end

--Comandos para adicionar ou remover Staffs (exemplo)
local function adicionarStaff(userId)
    if not table.find(staffList, userId) then
       table.insert(staffList, userId)
       salvarStaff()
       print("Staff Adicionado")
    else
        print("Staff já é um Staff")
    end
end

local function removerStaff(userId)
    if table.find(staffList, userId) then
       table.remove(staffList, table.find(staffList, userId))
       salvarStaff()
       print("Staff Removido")
    else
       print("Staff não é um Staff")
    end
end

local function salvarStaff()
    local success, errorMessage = pcall(function()
        staffDataStore:SetAsync("staff", staffList)
    end)

    if not success then
        warn("Erro ao salvar os Staffs" .. errorMessage)
    end
end

-- Função para detectar quando um jogador entra e define o atributo IsStaff
local function onPlayerAdded(player)
    if player:IsA("Player") then
        local userId = player.UserId
        local isStaff = table.find(staffList, userId) ~= nil

        if not player:HasAttribute("IsStaff") or player:GetAttribute("IsStaff") ~= isStaff then
            player:SetAttribute("IsStaff", isStaff)
        end

        --Monitora futuras mudanças no atributo IsStaff
        player:GetAttributeChangedSignal("IsStaff"):Connect(function()
            local currentIsStaff = player:GetAttribute("IsStaff")
            if currentIsStaff ~= isStaff then
                isStaff = currentIsStaff
            end
        end)
    end
end

-- Conectando ao evento de entrada do jogador
game.Players.PlayerAdded:Connect(onPlayerAdded)

-- Para jogadores que já estão no servidor quando o script é iniciado
for _, player in ipairs(game.Players:GetPlayers()) do
    onPlayerAdded(player)
end

-- Salvar a lista quando o servidor for desligado (opcional)
game:BindToClose(function()
  salvarStaff()
end)

carregarStaff() -- Carrega os Staffs na inicialização do servidor
