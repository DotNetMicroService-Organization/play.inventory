# Play.Inventory 
Play Economy Inventory microservice.

## Create and publish package Contracts
```powershell
$version="1.0.4"
$owner="DotNetMicroService-Organization"
$ph_pat="[PAT HERE]"

dotnet pack src\Play.Inventory.Contracts\ --configuration Release -p:PackageVersion=$version -p:RepositoryUrl=https://github.com/$owner/play.inventory -o ..\packages

dotnet nuget push ..\packages\Play.Inventory.Contracts.$version.nupkg --api-key $ph_pat --source "github"
```

## Build the docker image
```powershell
$env:GH_OWNER="DotNetMicroService-Organization"
$env:GH_PAT="[PAT HERE]"
docker build --secret id=GH_OWNER --secret id=GH_PAT -t play.inventory:$version .
```

## run the docker image
```powershell
$cosmosDbConnString="[CONN STRING HERE]"
$serviceBusConnString="[CONN STRING HERE]"
docker run -it --rm -p 5004:5004 --name inventory -e MongoDbSettings__ConnectionString=$cosmosDbConnString -e ServiceBusSettings__ConnectionString=$serviceBusConnString -e ServiceSettings__MessageBroker="SERVICEBUS" play.inventory:$version
```