```
az vm create \
  --resource-group learn-726e639f-a412-460a-97e5-eef5faa2b2f5 \
  --location westus \
  --name SampleVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys \
  --verbose
```
#### Popular images
```
az vm image list --output table
```

```
az vm image list --sku Wordpress --output table --all
```

```
az vm image list --publisher Microsoft --output table --all
```

```
az vm list-sizes --location eastus --output table
```

```
az vm create \
    --resource-group learn-726e639f-a412-460a-97e5-eef5faa2b2f5 \
    --name SampleVM2 \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --verbose \
    --size "Standard_DS5_v2"
```

#### VM sizes available in a resource group/cluster
```
az vm list-vm-resize-options \
    -g <rg> \
    -n <name> \
    --output table
```

```
az vm resize \
    -g <rg> \
    -n <name> \
    --size Standard_D2s_v3
```

```
az vm list -g <rg> --output table
```

```
az vm list-ip-addresses -g <rg> -n <name> -o table
```

#### Query the output JSON
```
az vm show \
    -g <rg> \
    -n <name> \
    --query "osProfile.adminUsername"
```
```
az vm show \
    -g <rg> \
    -n <name> \
    --query "networkProfile.networkInterfaces[].id"
```

```
start / stop / restart

az vm restart \
    -n <name> \
    -g <rg>
```

### Open ports
```
az vm open-port \
    --port 80 \
    -g <rg> \
    -n <name>
```
