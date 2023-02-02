# Doc to install istio through helm : https://istio.io/latest/docs/setup/install/helm/
# Configure the helm repository for Istio

helm repo add istio https://istio-release.storage.googleapis.com/charts
helm repo update

# Namespace creation for istio
kubectl create namespace istio-system

# Install the Istio base chart
helm install istio-base istio/base -n istio-system



# add a tag on namespace
kubectl label namespace api istio-injection=enabled
kubectl label namespace web istio-injection=enabled

# redeploy all workload to add an istio sidecar

# test that workload app is using tls 1.3
kubectl exec "$(kubectl get pod -l app=userprofile -n api -o jsonpath={.items..metadata.name})" -c istio-proxy -n api -- openssl s_client -alpn istio -tls1_3 -connect userprofile.api.svc.cluster.local:80 | grep "TLSv1.3"

# grafana deployment 
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.16/samples/addons/grafana.yaml

# prometheus deployment
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.16/samples/addons/prometheus.yaml

# install istioctl command
curl -sL https://istio.io/downloadIstioctl | sh -
export PATH=$HOME/.istioctl/bin:$PATH

# open grafana dashboard for istio
istioctl dashboard grafana