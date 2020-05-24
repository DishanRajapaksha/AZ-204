### Create ACR
```
az acr create --resource-group learn-deploy-acr-rg --name $ACR_NAME --sku Premium
```

### List admin credentials 
```
az acr credential show --name $ACR_NAME
```

### Login to ACR
```
docker login myregistry.azurecr.io
```

### Buid image
```
az acr build --registry $ACR_NAME --image <docker file path>
```

### Push an image to ACR
```
docker push myregistry.azurecr.io/myapp:v1
```

### List ACR images
```
az acr repository list --name $ACR_NAME --output table
```

### List the images in the registry
```
az acr repository show --repository myapp --name myregistry
```

### Enable the registry admin account
```
az acr update -n $ACR_NAME --admin-enabled true
```

### Deploy a container with Azure CLI
```
az container create \
    --resource-group learn-deploy-acr-rg \
    --name acr-tasks \
    --image $ACR_NAME.azurecr.io/helloacrtasks:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --ip-address Public \
    --location <location> \
    --registry-username [username] \
    --registry-password [password]
```

### Get container IP
```
az container show --resource-group  learn-deploy-acr-rg --name acr-tasks --query ipAddress.ip --output table
```

### ACR replication
```
az acr replication create --registry $ACR_NAME --location japaneast
```

### See replica list
```
az acr replication list --registry $ACR_NAME --output table
```
