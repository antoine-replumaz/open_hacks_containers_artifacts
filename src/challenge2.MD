az group create --name rg-team8 --location francecentral
az aks create -g rg-team8  -n aks-team8 --enable-managed-identity --node-count 3 --enable-addons monitoring --enable-msi-auth-for-monitoring  --generate-ssh-keys

Server=tcp:sqlserverubr8272.database.windows.net,1433;Initial Catalog=mydrivingDB;Persist Security Info=False;User ID=sqladminuBr8272;Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;

az group create --name rg-3-team8 --location northeurope
az aks create --resource-group rg-3-team8  --name aks-private-team8 --load-balancer-sku standard  --enable-private-cluster --network-plugin azure --vnet-subnet-id /subscriptions/f50d52a5-763e-400e-9057-a7ef46a43af1/resourceGroups/teamResources/providers/Microsoft.Network/virtualNetworks/vnet/subnets/vm-subnet  --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.0.0.10 --service-cidr 10.0.0.0/24 --generate-ssh-keys --location northeurope