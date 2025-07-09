Command#1
git clone https://github.com/mbilalamjad/openai-end-to-end-baseline

Command#2
cd openai-end-to-end-baseline

Command#3
DOMAIN_NAME_APPSERV="contoso.com"

Command#4
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -out appgw.crt -keyout appgw.key -subj "/CN=${DOMAIN_NAME_APPSERV}/O=Contoso" -addext "subjectAltName = DNS:${DOMAIN_NAME_APPSERV}" -addext "keyUsage = digitalSignature" -addext "extendedKeyUsage = serverAuth"

Command#5
openssl pkcs12 -export -out appgw.pfx -in appgw.crt -inkey appgw.key -passout pass:

Command#6
APP_GATEWAY_LISTENER_CERTIFICATE_APPSERV=$(cat appgw.pfx | base64 | tr -d '\n')

Command#7
echo APP_GATEWAY_LISTENER_CERTIFICATE_APPSERV: $APP_GATEWAY_LISTENER_CERTIFICATE_APPSERV

Command#8
LOCATION=swedencentral

Command#9 - Set the value of the below variable to the ID in Help section
BASE_NAME=

Command#10
RESOURCE_GROUP=rg-chat-baseline

Command#11
az group create -l $LOCATION -n $RESOURCE_GROUP

Command#12
PRINCIPAL_ID=$(az ad signed-in-user show --query id -o tsv)

Command#13
az deployment group create -f ./infra-as-code/bicep/main.bicep -g $RESOURCE_GROUP -p appGatewayListenerCertificate=${APP_GATEWAY_LISTENER_CERTIFICATE_APPSERV} -p baseName=${BASE_NAME} -p yourPrincipalId=${PRINCIPAL_ID}

Command#14
Pa$$w0rd