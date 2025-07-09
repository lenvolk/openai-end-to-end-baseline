Command#1
az login

Command#2 - replace xxxxx with subscription id
az account set --subscription xxxxx

Command#3 - Set the value of the below variable to the ID in Help section
BASE_NAME=

Command#4
$RESOURCE_GROUP="rg-chat-baseline-${BASE_NAME}"

Command#5
$AI_FOUNDRY_NAME="aif${BASE_NAME}"

Command#6
$BING_CONNECTION_NAME="bingaiagent${BASE_NAME}"

Command#7
$AI_FOUNDRY_PROJECT_NAME="projchat"

Command#8
$MODEL_CONNECTION_NAME="agent-model"

Command#9
$BING_CONNECTION_ID="$(az cognitiveservices account show -n $AI_FOUNDRY_NAME -g $RESOURCE_GROUP --query 'id' --out tsv)/projects/${AI_FOUNDRY_PROJECT_NAME}/connections/${BING_CONNECTION_NAME}"

Command#10
$AI_FOUNDRY_AGENT_CREATE_URL="https://${AI_FOUNDRY_NAME}.services.ai.azure.com/api/projects/

Command#11
${AI_FOUNDRY_PROJECT_NAME}/assistants?api-version=2025-05-15-preview"

Command#12
echo $BING_CONNECTION_ID

Command#13
echo $MODEL_CONNECTION_NAME

Command#14
echo $AI_FOUNDRY_AGENT_CREATE_URL

Command#15
Invoke-WebRequest -Uri "https://github.com/Azure-Samples/openai-end-to-end-baseline/raw/refs/heads/main/agents/chat-with-bing.json" -OutFile "chat-with-bing.json"

Command#16
${c:chat-with-bing-output.json} = ${c:chat-with-bing.json} -replace 'MODEL_CONNECTION_NAME', $MODEL_CONNECTION_NAME -replace 'BING_CONNECTION_ID', $BING_CONNECTION_ID

Command#17
az rest -u $AI_FOUNDRY_AGENT_CREATE_URL -m "post" --resource "https://ai.azure.com" -b @chat-with-bing-output.json

Command#19
$AGENT_ID="$(az rest -u $AI_FOUNDRY_AGENT_CREATE_URL -m 'get' --resource 'https://ai.azure.com' --query 'data[0].id' -o tsv)"

Command#20
echo $AGENT_ID