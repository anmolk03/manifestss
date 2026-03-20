# Install Kubectl
sudo az aks install-cli

# Install Helm
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

az login
# Use your subscription ID
az account set --subscription "ce1fe2d6-685f-4758-a44e-d005c9d82354"

# Pull credentials
az aks get-credentials --resource-group playgroundcleansub0 --name aks-private-cluster

# 1. Add and update the repo
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

# 2. Create the namespace (if it doesn't exist)
kubectl create namespace ingress-nginx --dry-run=client -o yaml | kubectl apply -f -

# 3. Install the chart
helm upgrade --install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --set controller.service.type=LoadBalancer \
  --set controller.replicaCount=2




  #########
  - script: |
        # Ensure we have the right context
        az aks get-credentials --resource-group $(aksResourceGroup) --name $(aksClusterName)
        
        # Run Helm directly
        helm upgrade --install $(helmReleaseName) ingress-nginx/ingress-nginx \
          --namespace $(helmNamespace) \
          --create-namespace \
          --set controller.service.type=LoadBalancer \
          --set controller.replicaCount=2
      displayName: 'Manual Helm Install via Bash'
