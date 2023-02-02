# Doc to install istio through helm : https://istio.io/latest/docs/setup/install/helm/
# Configure the helm repository for Istio

helm repo add istio https://istio-release.storage.googleapis.com/charts
helm repo update

# Namespace creation for istio
kubectl create namespace istio-system

# Install the Istio base chart
helm install istio-base istio/base -n istio-system