### Create ACR
```
az acr create --resource-group learn-deploy-acr-rg --name <container_registry_name> --sku Premium
```

### List admin credentials 
```
az acr credential show --name <container_registry_name>
```

### Service principal 
```
az ad sp create-for-rbac --name http://<service principal name> --scopes <acr id> --role acrpull
```

### Login to ACR
```
docker login <container_registry_name>.azurecr.io
```

### Create an image
```
az container create --resource-group <rg name> --name <container name> --image <tag> --cpu 1 --memory 1 --registry-login-server <acr server url> --registry-username <service principal username> --registry-password <service principal password> --dns-name-label <app dns name> --ports 80
```

### Buid image
```
az acr build --registry <container_registry_name> --image <docker file path>
```

### Push an image to ACR
```
docker push myregistry.azurecr.io/myapp:v1
```

### List ACR images
```
az acr repository list --name <container_registry_name> --output table
```

### List the images in the registry
```
az acr repository show --repository myapp --name <container_registry_name>
```

### Enable the registry admin account
```
az acr update -n <container_registry_name> --admin-enabled true
```

### Deploy a container with Azure CLI
```
az container create \
    --resource-group <rg name> \
    --name <container name> \
    --image <container_registry_name>.azurecr.io/helloacrtasks:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --ip-address Public \
    --location <location> \
    --registry-username [username] \
    --registry-password [password]
```

### Get container IP
```
az container show --resource-group  <rg name> --name <container name> --query ipAddress.ip --output table
```

### ACR replication
```
az acr replication create --registry <container_registry_name> --location <location>
```

### See replica list
```
az acr replication list --registry <container_registry_name> --output table
```

###ACR Tasks Rebuild on source code change
```
az acr task create --registry <container_registry_name> --name buildwebapp --image webimage --context https://github.com/MicrosoftDocs/mslearn-deploy-run-container-app-service.git --branch master --file Dockerfile --git-access-token <access_token>
```
