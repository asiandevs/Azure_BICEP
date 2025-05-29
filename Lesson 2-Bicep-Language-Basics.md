# Lesson 2: Bicep Language Basics

## Objective

Learn the core components of a Bicep file and how to write a basic infrastructure deployment.
- Bicep file structure
- Declaring parameters and variables
- Creating resources
- Output values
- Step-by-step example

---

## Bicep Building Blocks

| Keyword       | Purpose                                         | Analogy                             |
| ------------- | ----------------------------------------------- | ----------------------------------- |
| `targetScope` | Sets the deployment level (e.g. resource group) | Kitchen location (oven vs stovetop) |
| `param`       | Defines input values                            | Ingredients                         |
| `var`         | Stores calculated or reusable values            | Pre-mixed ingredients               |
| `resource`    | Describes Azure resources to deploy             | The cake or dish being made         |
| `output`      | Returns values after deployment                 | Serving label or summary            |

---

## 1. Bicep File Structure

A Bicep file (`.bicep`) is a plain text file that defines Azure resources and their configuration.

**Basic Structure:**

```bicep
// Parameters: values you provide at deployment time
param location string = 'eastus'

// Variables: local values for reuse
var storageName = 'mystorage${uniqueString(resourceGroup().id)}'

// Resources: Azure resources to deploy
resource storageAcct 'Microsoft.Storage/storageAccounts@2022-09-01' = {
  name: storageName
  location: location
  sku: { name: 'Standard_LRS' }
  kind: 'StorageV2'
}

// Outputs: values returned after deployment
output storageAccountName string = storageAcct.name
```

---

## 2. Declaring Parameters

Parameters make your templates flexible and reusable.

```bicep
param location string = 'eastus'
param storageSku string = 'Standard_LRS'
```

- You can override these values during deployment.

---

## 3. Using Variables

Variables help avoid repetition and improve maintainability.

```bicep
var storageName = 'mystorage${uniqueString(resourceGroup().id)}'
```

- Here, `uniqueString(...)` ensures the storage account name is unique in Azure.

---

## 4. Defining Resources

The `resource` keyword declares an Azure resource.

```bicep
resource storageAcct 'Microsoft.Storage/storageAccounts@2022-09-01' = {
  name: storageName
  location: location
  sku: {
    name: storageSku
  }
  kind: 'StorageV2'
}
```

---

## 5. Outputs

Outputs display values after deployment (useful for scripts, pipelines, or reference).

```bicep
output storageAccountName string = storageAcct.name
```

---

## Step-by-Step: Create a Storage Account

1. **Create a file:** `main.bicep`

2. **A Simple Example:**

```bicep
targetScope = 'resourceGroup'

param storageName string = 'mybicepstorage'

resource myStorage 'Microsoft.Storage/storageAccounts@2022-09-01' = {
  name: storageName
  location: resourceGroup().location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {}
}

output storageId string = myStorage.id
```

## Line-by-Line Breakdown

* `targetScope = 'resourceGroup'`
  This template will be deployed to a specific Azure resource group.

* `param storageName string = 'mybicepstorage'`
  We define a **parameter** for the storage account name.

* `resource myStorage …`
  This block **creates a storage account** in Azure using a well-defined schema.

* `output storageId …`
  After deployment, we return the **resource ID** so other tools or scripts can use it.

---

3. **Deploy using Azure CLI:**

```sh
az deployment group create \
  --resource-group <YOUR-RESOURCE-GROUP> \
  --template-file main.bicep
```

4. **View the output:**  
   The deployment will show the `storageAccountName` created.

---
## 🛠 Try It Yourself

### Step 1: Create the Bicep file

Save the above example as `storage.bicep`.

### Step 2: Create a Resource Group (if not already)

```bash
az group create --name bicep-demo-rg --location australiaeast
```

### Step 3: Deploy the Bicep Template

```bash
az deployment group create \
  --resource-group bicep-demo-rg \
  --template-file storage.bicep
```

### Step 4: Verify

```bash
az storage account show --name mybicepstorage --resource-group bicep-demo-rg
```

---
## Summary

- Parameters = Inputs
- Variables = Local values
- Resources = What Azure creates
- Outputs = Results after deployment

**Next:**  
In Lesson 3, we'll dive more into Bicep language basics.


