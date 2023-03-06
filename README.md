# Play.Infra
Play EconomyInfrastructure components

## Add the GitHub package source
```powershell
$owner="waikahu"
$gh_pat="[PAT HERE]"

dotnet nuget add source --username USERNAME --password $gh_pat --store-password-in-clear-text --name github "https://nuget.pkg.github.com/$owner/index.json"
```

## Creating the Azure resource group ($appname is unique)
```powershell
$appname="wbplayeconomy"
az group create --name $appname --location eastus
```

## Creating the Azure CosmosDB account
```powershell
az cosmosdb create --name $appname --resource-group $appname --kind MongoDB --enable-free-tier
```

## Creating the Azure Service Bus namespace
```powershell
az servicebus namespace create --name $appname --resource-group $appname --sku Standard
```

## Creating the Azure Container Registry
```powershell
az acr create --name $appname --resource-group $appname --sku Basic
```

## Creating the Azure Kubernetes Service cluster
```powershell
az extension add --name aks-preview

az feature register --namespace "Microsoft.ContainerService" --name "EnablePodIdentityPreview"
az feature register --namespace "Microsoft.ContainerService" --name "EnableWorkloadIdentityPreview"

az aks create -n $appname -g $appname --node-vm-size Standard_B2s --node-count 2 --attach-acr $appname --enable-oidc-issuer --enable-workload-identity --generate-ssh-keys
az aks get-credentials --resource-group $appname --name $appname
```

## Creating the Azure Key Vault
```powershell
az keyvault create -n $appname -g $appname --location eastus
```

## Installing Emissary-Ingress
```powershell
helm repo add datawire https://app.getambassador.io
helm repo update

kubectl apply -f https://app.getambassador.io/yaml/emissary/3.5.1/emissary-crds.yaml
kubectl wait --timeout=90s --for=condition=available deployment emissary-apiext -n emissary-system

$namespace="emissary"
helm install emissary-ingress datawire/emissary-ingress --set service.annotations."service\.beta\.kubernetes\.io\azure-dns-label-name"=$appname -n $namespace --create-namespace
kubectl rollout status deployment/emissary-ingress -n $namespace -w

```