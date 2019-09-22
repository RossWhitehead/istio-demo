# ACR
## Log in to the container registry
```
az acr login -n ssor9876
```

## List repositories and tags
```
az acr repository list -n ssorcontainerregistry
az acr repository show-tags -n ssorcontainerregistry --repository <repositoryname>
```

## Grant AKS access to ACR
```
AKS_RESOURCE_GROUP=Sandbox
AKS_CLUSTER_NAME=AKS
ACR_RESOURCE_GROUP=Sandbox
ACR_NAME=ssor9876

# Get the id of the service principal configured for AKS
CLIENT_ID=$(az aks show --resource-group $AKS_RESOURCE_GROUP --name $AKS_CLUSTER_NAME --query "servicePrincipalProfile.clientId" --output tsv)

# Get the ACR registry resource id
ACR_ID=$(az acr show --name $ACR_NAME --resource-group $ACR_RESOURCE_GROUP --query "id" --output tsv)

# Create role assignment
az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```