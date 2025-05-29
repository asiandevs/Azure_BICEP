# Lesson 1: Introduction to Bicep and ARM Templates

## What Is Bicep?

**Bicep** is a domain-specific language (DSL) developed by Microsoft for declaratively deploying Azure resources. It is designed to simplify the process of defining infrastructure as code (IaC) for Azure. Bicep is open source, concise, and easier to read and write compared to traditional JSON-based ARM (Azure Resource Manager) JSON templates.
Think of Bicep as the blueprint language for building in the Azure cloud—simple, structured, and readable.
## What Are ARM Templates?

**ARM Templates** are JSON files that define the infrastructure and configuration for your Azure solution. They allow you to deploy, update, or delete all the resources for your solution in a single, coordinated operation.

### ARM Templates vs Bicep

| Feature                 | ARM Templates          | Bicep              |
|-------------------------|-----------------------|--------------------|
| File Format             | JSON                  | .bicep (own syntax)|
| Readability             | Verbose, hard to read | Concise, clean     |
| Tooling                 | Native Azure support  | Native + VS Code   |
| Modularity              | Supported, verbose    | Simple, built-in   |
| Authoring Experience    | Error-prone           | Intuitive, Guided  |
| Comments support        | NO                    | Yes  |
| Source-controlled?      | Yes                   | Yes  |

## Why Choose Bicep?

- **Simpler Syntax:** Easier to read and write than JSON.
- **Modularization:** Encourages reusable components.
- **First-Class Azure Support:** Officially supported by Microsoft and native in Azure CLI.
- **No State Management:** Uses Azure Resource Manager, so no separate state files to manage.

## Step-by-Step: A Simple Example

### 1. ARM Template (JSON)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2022-09-01",
      "name": "mystorageaccountdemo",
      "location": "eastus",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "StorageV2",
      "properties": {}
    }
  ]
}
```

### 2. The Same in Bicep

```bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2022-09-01' = {
  name: 'mystorageaccountdemo'
  location: 'eastus'
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
}
```

**Notice:**  
The Bicep version is much shorter, easier to read, and less error-prone.

## How Bicep Deploys to Azure

- Bicep transpiles (compiles) your code into ARM JSON templates.
- Azure Resource Manager reads the template and deploys the resources.
- No need to manually interact with ARM JSON.
```
# Convert Bicep to JSON (optional)
bicep build main.bicep

# Deploy Bicep directly
az deployment group create \
  --resource-group myResourceGroup \
  --template-file main.bicep
```

## How to Get Started

1. **Install Azure CLI:**  
   [Azure CLI Install Guide](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
   
**Validatee:** Ensure you have the Azure CLI installed and logged in via az login.

2. **Install Bicep CLI:**
   ```sh
   az bicep install
   ```

3. **Start Authoring:**  
   Use Visual Studio Code with the [Bicep Extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-bicep) for syntax highlighting and validation.

4. **Create a Simple Bicep File**
```
// main.bicep
resource rg 'Microsoft.Resources/resourceGroups@2021-04-01' = {
  name: 'demo-bicep-rg'
  location: 'australiaeast'
}
```
5. **Deploy It**
```
az deployment sub create \
  --location australiaeast \
  --template-file main.bicep
---
```

**Next:**  
In Lesson 2, we'll dive into Bicep language basics and write your first Bicep file.
