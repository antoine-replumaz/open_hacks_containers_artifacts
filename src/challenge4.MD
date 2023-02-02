# Add the ingress-nginx repository

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
NAMESPACE=ingress-basic

# install basic ingress
helm install ingress-nginx ingress-nginx/ingress-nginx --create-namespace --namespace $NAMESPACE --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-health-probe-request-path"=/healthz

# check ingress status
kubectl --namespace ingress-basic get services -o wide -w ingress-nginx-controller

# keyvault
az aks enable-addons -g rg-3-team8 -n aks-team8-prod --addons azure-keyvault-secrets-provider

## check installation du kv

kubectl get pods -n kube-system -l 'app in (secrets-store-csi-driver,secrets-store-provider-azure)'

## create kv

az keyvault create -n kv-team8 -g rg-3-team8

## configure APP to use kv

az keyvault secret set --vault-name kv-team8 -n sql-password --value "yourStrong*Password1"
 ...
az aks show -g rg-3-team8 -n aks-team8-prod --query "addonProfiles.azureKeyvaultSecretsProvider.identity.clientId" -o tsv
//get client ID for az keyvault set-policy AND secrets.yml

az keyvault set-policy -n kv-team8 --secret-permissions get --spn 45f965d7-ea86-4ef3-a1fd-bb095439593a

