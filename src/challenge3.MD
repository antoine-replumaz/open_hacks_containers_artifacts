# Création du cluster AKS
az aks create --resource-group rg-3-team8  --name aks-team8-prod --attach-acr registryubr8272 --load-balancer-sku standard --network-plugin azure --vnet-subnet-id /subscriptions/f50d52a5-763e-400e-9057-a7ef46a43af1/resourceGroups/teamResources/providers/Microsoft.Network/virtualNetworks/vnet/subnets/vm-subnet  --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.0.0.10 --service-cidr 10.0.0.0/24 --generate-ssh-keys --location northeurope --enable-aad --aad-admin-group-object-ids b74d58fb-9677-485e-a608-ecc263a83c6b

# Role Assignment
az role assignment create --assignee b74d58fb-9677-485e-a608-ecc263a83c6b --role "Azure Kubernetes Service Cluster Admin Role" --scope /subscriptions/f50d52a5-763e-400e-9057-a7ef46a43af1/resourceGroups/rg-3-team8/providers/Microsoft.ContainerService/managedClusters/aks-team8-prod

# Se connecter au cluster
az aks get-credentials --resource-group rg-3-team8 --name aks-team8-prod --admin

# Activation du module rbac pour AKS
az aks update -g rg-3-team8 -n aks-team8-prod --enable-azure-rbac