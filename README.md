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
$appname="playeconomy"
az group create --name $appname --location eastus
```

## Creating the Azure CosmosDB account
```powershell
$dbname="wbplayeconomy"
az cosmosdb create --name $dbname --resource-group $appname --kind MongoDB --enable-free-tier
```

## Creating the Azure Service Bus namespace
```powershell
$sbname="wbplayeconomy"
az servicebus namespace create --name $sbname --resource-group $appname --sku Standard
```

## Creating the Azure Container Registry
```powershell
$crname="wbplayeconomy"
az acr create --name $crname --resource-group $appname --sku Basic
```