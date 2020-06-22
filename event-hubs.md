```
az configure --defaults group= location=
```

### Create Namespace
```
az eventhubs namespace create --name --resource-group
```

### Get CS
```
az eventhubs namespace authorization-rule keys list --name RootManageSharedAccessKey --namespace-name 
```

### Create Hub
```
az eventhubs eventhub create --name --namespace-name 
```

