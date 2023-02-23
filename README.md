# Play.Infra
Play EconomyInfrastructure components

## Add the GitHub package source
```powershell
$owner="waikahu"
$gh_pat="[PAT HERE]"

dotnet nuget add source --username USERNAME --password $gh_pat --store-password-in-clear-text --name github "https://nuget.pkg.github.com/$owner/index.json"
```

## Creating Azure resource group
```powershell
$appname="playeconomy"
az group create --name $appname --location eastus
```
