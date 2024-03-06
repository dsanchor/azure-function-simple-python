# Azure functions walk-through

## Clone this repo

```bash
git clone https://github.com/dsanchor/azure-function-simple-python.git
```

## Initialize environment variables

```bash
RG=<target-rg> # Ex: test-function-rg
LOCATION=<resources-location> # Ex: westeurope
SA=<storage-account-name> # Ex: testfunction1123sa
FUNCAPP=<func-app-name> #Ex: test-function-1123
CODE_LOCATION=<cloned-code-location>
```
## Deploy to Azure

### Login to Azure

```bash
az login
```

### Create a resource group (if it doesn't exist)

```bash
az group create --name $RG --location $LOCATION
```

### Create a storage account (if it doesn't exist)

```bash
az storage account create --name $SA --location $LOCATION --resource-group $RG --sku Standard_LRS
```

### Create a function app

```bash
az functionapp create --resource-group $RG --consumption-plan-location $LOCATION --runtime python --runtime-version 3.8 --functions-version 4 --name $FUNCAPP --os-type linux --storage-account $SA
```

### Publish function to Azure

- Navigate to the function app directory

```bash
cd $CODE_LOCATION
```

Two options:

- Publish the function app to Azure with **func** cli:

```bash
func azure functionapp publish $FUNCAPP
```

- Publish the function using the **az** cli:

```bash
zip publish.zip -r function_app.py host.json
az functionapp deployment source config-zip --resource-group $RG --name $FUNCAPP --src ./publish.zip
```

### Test the function

```bash
curl https://$FUNCAPP.azurewebsites.net/api/http_trigger?name=Azure
```
