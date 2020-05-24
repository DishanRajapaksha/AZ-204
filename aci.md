### Create
```
az container create \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer \
  --image microsoft/aci-helloworld \
  --ports 80 \
  --dns-name-label <unique url for the internet> \
  --location eastus
```

### Show
```
az container show \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer \
  --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
  --out table
```

### Control restart behavior
```
az container create \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer-restart-demo \
  --image microsoft/aci-wordcount:latest \
  --restart-policy OnFailure \
  --location eastus
```

### View logs
```
az container logs \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer-restart-demo
```

### Get container events
```
az container attach \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer
```

### Set environment variables
```
az container create \
  --resource-group learn-deploy-aci-rg \
  --name aci-demo \
  --image microsoft/azure-vote-front:cosmosdb \
  --ip-address Public \
  --location eastus \
  --environment-variables \
    COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

### Secue environment variables
```
az container create \
  --resource-group learn-deploy-aci-rg \
  --name aci-demo-secure \
  --image microsoft/azure-vote-front:cosmosdb \
  --ip-address Public \
  --location eastus \
  --secure-environment-variables \
    COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

### Use data volumes

#### Create storage
```
az storage account create \
  --resource-group learn-deploy-aci-rg \
  --name $STORAGE_ACCOUNT_NAME \
  --sku Standard_LRS \
  --location eastus
```
```
export AZURE_STORAGE_CONNECTION_STRING=$(az storage account show-connection-string \
  --resource-group learn-deploy-aci-rg \
  --name $STORAGE_ACCOUNT_NAME \
  --output tsv)
```
```
STORAGE_KEY=$(az storage account keys list \
  --resource-group learn-deploy-aci-rg \
  --account-name $STORAGE_ACCOUNT_NAME \
  --query "[0].value" \
  --output tsv)
```
#### Create file share (users AZURE_STORAGE_CONNECTION_STRING)
```
az storage share create --name aci-share-demo
```
#### Container with file share
```
az container create \
  --resource-group learn-deploy-aci-rg \
  --name aci-demo-files \
  --image microsoft/aci-hellofiles \
  --location eastus \
  --ports 80 \
  --ip-address Public \
  --azure-file-volume-account-name $STORAGE_ACCOUNT_NAME \
  --azure-file-volume-account-key $STORAGE_KEY \
  --azure-file-volume-share-name aci-share-demo \
  --azure-file-volume-mount-path /aci/logs/
```
#### Show files
```
az storage file list -s aci-share-demo -o table
```
#### Download files
```
az storage file download -s aci-share-demo -p <filename>
```

### Execute a command in your container
```
az container exec \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer \
  --exec-command /bin/sh
```

### Monitor container
```
CONTAINER_ID=$(az container show \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer \
  --query id \
  --output tsv)
```
```
az monitor metrics list \
  --resource $CONTAINER_ID \
  --metric CPUUsage \
  --output table
```
```
az monitor metrics list \
  --resource $CONTAINER_ID \
  --metric MemoryUsage \
  --output table
```
