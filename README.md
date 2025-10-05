# DIO-Deploy

Script para Deploy de uma API na nuvem

1. Variáveis de Configuração:
# Nomes únicos para os recursos
# OBS: O nome do Web App deve ser globalmente único no Azure.
RESOURCE_GROUP_NAME="rg-suaapi-prod"
APP_SERVICE_PLAN_NAME="plan-suaapi-prod"
WEB_APP_NAME="suaapi-catalogo-filmes-unico-xyz" 
LOCATION="eastus" # Escolha a região mais próxima de seus usuários (ex: brazilsouth)

# SKU (nível de preço) - F1 é o nível Gratuito (Free) para testes
SKU_TIER="F1"

2. Comandos de Deploy:
   # -------------------------------------------------------------------
# PASSO 1: Fazer Login no Azure (se ainda não estiver logado)
# -------------------------------------------------------------------
az login

# -------------------------------------------------------------------
# PASSO 2: Criar o Grupo de Recursos (Container Lógico)
# -------------------------------------------------------------------
echo "Criando o Grupo de Recursos: $RESOURCE_GROUP_NAME..."
az group create --name $RESOURCE_GROUP_NAME --location $LOCATION

# -------------------------------------------------------------------
# PASSO 3: Criar o Plano do App Service (define a capacidade da VM)
# (Este passo é opcional ao usar 'az webapp up', mas é bom para controle)
# -------------------------------------------------------------------
echo "Criando o Plano do App Service: $APP_SERVICE_PLAN_NAME (SKU: $SKU_TIER)..."
az appservice plan create \
  --name $APP_SERVICE_PLAN_NAME \
  --resource-group $RESOURCE_GROUP_NAME \
  --location $LOCATION \
  --sku $SKU_TIER

# -------------------------------------------------------------------
# PASSO 4: Criar o Web App e Implantar o Código (Comando Simplificado)
#
# O comando 'az webapp up' é o mais simples: ele cria o Web App, 
# detecta o Runtime (ex: .NET, Python), e faz o deploy do ZIP.
# Você deve estar no diretório raiz do seu projeto ao executar este comando.
# -------------------------------------------------------------------
echo "Criando o Web App e implantando o código..."
az webapp up \
  --name $WEB_APP_NAME \
  --resource-group $RESOURCE_GROUP_NAME \
  --plan $APP_SERVICE_PLAN_NAME \
  --os-type linux # Use 'windows' se o seu projeto exigir

# -------------------------------------------------------------------
# PASSO 5: Exibir a URL da API
# -------------------------------------------------------------------
echo "Deploy concluído!"
echo "Sua API está acessível em: https://$WEB_APP_NAME.azurewebsites.net"

# Opcional: Ativar ambiente de desenvolvimento para ver o Swagger (.NET)
# az webapp config appsettings set --name $WEB_APP_NAME --resource-group $RESOURCE_GROUP_NAME --settings ASPNETCORE_ENVIRONMENT="Development"

