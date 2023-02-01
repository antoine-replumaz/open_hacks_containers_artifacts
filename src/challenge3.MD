# Création du cluster AKS
az aks create --resource-group rg-3-team8  --name aks-team8-prod --load-balancer-sku standard --network-plugin azure --vnet-subnet-id /subscriptions/f50d52a5-763e-400e-9057-a7ef46a43af1/resourceGroups/teamResources/providers/Microsoft.Network/virtualNetworks/vnet/subnets/vm-subnet  --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.0.0.10 --service-cidr 10.0.0.0/24 --generate-ssh-keys --location northeurope

# Activation du module AAD
az aks update -g rg-3-team8 -n aks-team8-prod --enable-aad

# Activation du module rbac pour AKS
az aks update -g rg-3-team8 -n aks-team8-prod --enable-azure-rbac