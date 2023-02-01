# Add the ingress-nginx repository

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
NAMESPACE=ingress-basic

# install basic ingress
helm install ingress-nginx ingress-nginx/ingress-nginx --create-namespace --namespace $NAMESPACE --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-health-probe-request-path"=/healthz

# check ingress status
kubectl --namespace ingress-basic get services -o wide -w ingress-nginx-controller