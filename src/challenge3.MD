# Création du cluster AKS
az aks create --resource-group rg-3-team8  --name aks-team8-prod --attach-acr registryubr8272 --load-balancer-sku standard --network-plugin azure --vnet-subnet-id /subscriptions/f50d52a5-763e-400e-9057-a7ef46a43af1/resourceGroups/teamResources/providers/Microsoft.Network/virtualNetworks/vnet/subnets/vm-subnet  --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.0.0.10 --service-cidr 10.0.0.0/24 --generate-ssh-keys --location northeurope --enable-aad --aad-admin-group-object-ids b74d58fb-9677-485e-a608-ecc263a83c6b

# Role Assignment
az role assignment create --assignee b74d58fb-9677-485e-a608-ecc263a83c6b --role "Azure Kubernetes Service Cluster Admin Role" --scope /subscriptions/f50d52a5-763e-400e-9057-a7ef46a43af1/resourceGroups/rg-3-team8/providers/Microsoft.ContainerService/managedClusters/aks-team8-prod
# Role Assignment to web-dev
az role assignment create --assignee 50224319-45fa-497c-b340-0c6cc88f2b23 --role "Azure Kubernetes Service Cluster User Role" --scope /subscriptions/f50d52a5-763e-400e-9057-a7ef46a43af1/resourceGroups/rg-3-team8/providers/Microsoft.ContainerService/managedClusters/aks-team8-prod
# Role Assignment to api-dev
az role assignment create --assignee 26c28a23-41de-4adc-b317-d13cd8a95f9e --role "Azure Kubernetes Service Cluster User Role" --scope /subscriptions/f50d52a5-763e-400e-9057-a7ef46a43af1/resourceGroups/rg-3-team8/providers/Microsoft.ContainerService/managedClusters/aks-team8-prod

# Se connecter au cluster
az aks get-credentials --resource-group rg-3-team8 --name aks-team8-prod --admin

# Activation du module rbac pour AKS
az aks update -g rg-3-team8 -n aks-team8-prod --enable-azure-rbac

# apply manifest
kubectl apply -f deploy_user-java.yaml

# See all pods
kubectl get pods -A

# add a AD group
az ad group create --display-name web-dev --mail-nickname web-dev

# see all group / user AD*
az ad group list
az ad user list

# Add a member to group
az ad group member add --group web-dev --member-id f167e347-aafd-4280-9ed9-b64606f66ec5
az ad group member add --group api-dev --member-id c64d1a15-925d-4ea3-a2d4-14c5f6fafb55



