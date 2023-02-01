az group create --name rg-team8 --location francecentral
az aks create -g rg-team8  -n aks-team8 --enable-managed-identity --node-count 3 --enable-addons monitoring --enable-msi-auth-for-monitoring  --generate-ssh-keys

# Attach ACR to an existing AKS 
az aks update -n aks-team8 -g rg-team8 --attach-acr registryubr8272

# Apply yaml config
kubectl apply -f <path-to-yaml>

# Delete a deployment if needed
kubectl delete -f <path-to-yaml>

Server=tcp:sqlserverubr8272.database.windows.net,1433;Initial Catalog=mydrivingDB;Persist Security Info=False;User ID=sqladminuBr8272;Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;

az group create --name rg-3-team8 --location northeurope
az aks create --resource-group rg-3-team8  --name aks-team8-prod --load-balancer-sku standard --network-plugin azure --vnet-subnet-id /subscriptions/f50d52a5-763e-400e-9057-a7ef46a43af1/resourceGroups/teamResources/providers/Microsoft.Network/virtualNetworks/vnet/subnets/vm-subnet  --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.0.0.10 --service-cidr 10.0.0.0/24 --generate-ssh-keys --location northeurope