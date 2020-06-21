### ARM Template

#### [Link](https://github.com/MicrosoftDocs/mslearn-logic-apps-and-arm-templates)

```
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "logicAppName": {
            "type": "string",
            "metadata": {
              "description": "The name of the logic app to create."
            }
          },
        "location": {
        "type": "string",
        "defaultValue": "[resourceGroup().location]",
        "metadata": {
              "description": "Location for all resources."
            }
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Logic/workflows",
            "apiVersion": "2017-07-01",
            "name": "[parameters('logicAppName')]",
            "location": "[parameters('location')]",
            "properties": {
                "state": "Enabled",
                "definition": {
                    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "triggers": {
                        "manual": {
                            "type": "Request",
                            "kind": "Http",
                            "inputs": {
                                "method": "GET",
                                "relativePath": "{width}/{height}",
                                "schema": {}
                            }
                        }
                    },
                    "actions": {
                        "Response": {
                            "runAfter": {},
                            "type": "Response",
                            "kind": "Http",
                            "inputs": {
                                "body": "Response from @{workflow().name}  Total area = @{triggerBody()}@{mul( int(triggerOutputs()['relativePathParameters']['height'])  , int(triggerOutputs()['relativePathParameters']['width'])  )}",                                
                                "statusCode": 200
                            }
                        }
                    },
                    "outputs": {}
                },
                "parameters": {}
            }
        }
    ],
    "outputs": {
        "logicAppUrl": {
           "type": "string",
           "value": "[listCallbackURL(concat(resourceId('Microsoft.Logic/workflows/', parameters('logicAppName')), '/triggers/manual'), '2017-07-01').value]"
        }
     }
}
```
