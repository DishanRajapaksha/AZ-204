#### Create
```
az storage account create \
        --resource-group <rg> \
        --kind StorageV2 \
        --sku Standard_LRS \
        --access-tier Cool \
        --name <name> \
        --access-tier Hot
```

```
az storage account keys list --account-name myAccount --resource-group myGroup --output table
```

```
az storage container create \
  --name myContainer \
  --account-name myAccount \
  --account-key <storage account key>
```

```
az storage blob upload \
  --container-name myContainer \
  --name myBlob\
  --file blobdata.dat
  --if-unmodified-since 2019-05-26T10:30Z
```

```
az storage blob upload-batch \
  --destination myContainer \
  --source myFolder \
  --pattern *.bmp
```

```
az storage blob list \
  --container-name myContainer \
  --output table
```

```
az storage blob copy start \
  --destination-container destContainer \
  --destination-blob myBlob \
  --source-account-name mySourceAccount \
  --source-account-key mySourceAccountKey \
  --source-container myContainer \
  --source-blob myBlob
```

```
az storage blob copy start-batch \
  --destination-container destContainer \
  --source-account-name mySourceAccount \
  --source-account-key mySourceAccountKey \
  --source-container myContainer \
  --pattern *.dat
```

```
az storage blob delete \
  --container sourceContainer \
  --name sourceBlob
```
