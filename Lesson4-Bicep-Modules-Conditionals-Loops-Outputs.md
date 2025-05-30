# Lesson 3: Modules, Conditionals, Loops, and Output Variables in Bicep

## Objectives

By the end of this lesson, you’ll be able to:

* Break your Bicep code into **reusable modules**
* Use **conditions** to control when resources are deployed
* Use **loops** to deploy multiple resources dynamically
* Use **output variables** to share data between templates or print results

---

## 1. Modules – Reuse and Organize Your Code

**Modules** allow you to break your Bicep code into reusable building blocks. This makes your projects easier to manage, test, and share.

### Why Use Modules?
Modules let you split your Bicep code into **smaller files** that you can **reuse** and manage more easily.
- **Reuse:** Define resources once, deploy them many times.
- **Organization:** Simplifies large deployments.
- **Separation:** Teams can work on different modules independently.

>  **Think of modules like functions in programming** — reusable blocks of logic.

### Example: Using a Module | File Structure
Suppose you have a module file named `storageModule.bicep`:

```
/main.bicep
/storageModule.bicep
```

###  storageModule.bicep

```bicep
param storageAccountName string

resource storage 'Microsoft.Storage/storageAccounts@2022-09-01' = {
  name: storageAccountName
  location: resourceGroup().location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {}
}

output storageId string = storage.id
```

###  main.bicep

```bicep
module storageMod './storageModule.bicep' = {
  name: 'deployStorage'
  params: {
    storageAccountName: 'mybicepstg123'
  }
}

output storageResourceId string = storageMod.outputs.storageId
```

>  Now you can **reuse** `storageModule.bicep` in multiple projects!

**Step-by-step:**
1. Author your reusable logic in a `.bicep` module file.
2. Reference the module with the `module` keyword in your main file.
3. Pass parameters and use outputs as needed.

---

## 2. Conditionals – Deploy Only If Needed

You can make resource creation or property assignment conditional using `if`.

###  Example: Only deploy if `isDev` is true

```bicep
param isDev bool = true

resource devStorage 'Microsoft.Storage/storageAccounts@2022-09-01' = if (isDev) {
  name: 'devstorageaccount123'
  location: resourceGroup().location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {}
}
```

>  The storage account will only be deployed if `isDev` is `true`.

---

## 3. Loops – Create Multiple Resources

Loops allow you to declare multiple resources based on a list or count. You can use `for` to **deploy multiple resources** with similar structure.

###  Example: Deploy 3 storage accounts

```bicep
var storageNames = [
  'storage1'
  'storage2'
  'storage3'
]

resource storageAccounts 'Microsoft.Storage/storageAccounts@2022-09-01' = [for name in storageNames: {
  name: name
  location: resourceGroup().location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {}
}]
```
>  One loop creates **three storage accounts** with different names for each name in the `storageNames` array.

---

## 4. Output Variables – Return Values from Templates or Modules

**Outputs** allow you to expose  **return values** after deployment, such as IDs, connection strings, or URLs.

###  Example: Output the storage account ID

```bicep
resource storage 'Microsoft.Storage/storageAccounts@2022-09-01' = {
  name: 'outputdemo123'
  location: resourceGroup().location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {}
}

output storageId string = storage.id
```

>  After deployment, the storage ID will be printed to the CLI and can be used by other templates.
---

## Summary Table


| Feature      |Purpose                                   |How to Use             | Example Syntax                                      |
|--------------|----------------------------------------- |------------------------|-----------------------------------------------------|
| Modules      |Reuse and organize Bicep files            | `module` keyword       | `module myMod './mod.bicep' = { ... }`             |
| Conditionals |Conditionally deploy resources            | `if` keyword           | `resource ... = if (condition) { ... }`             |
| Loops        |Loop to deploy multiple similar resources | `for ... in ...:`      | `[for name in names: { ... }]`                      |
| Outputs      |Return values from templates or modules   | `output` keyword       | `output name type = value`                          |


---

##  Hands-On Exercise

###  Step 1: Create a Module File

📄 `storageModule.bicep`

```bicep
param storageAccountName string

resource storage 'Microsoft.Storage/storageAccounts@2022-09-01' = {
  name: storageAccountName
  location: resourceGroup().location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {}
}

output storageId string = storage.id
```

###  Step 2: Create a Main File with a Loop and Condition

📄 `main.bicep`

```bicep
param deployProd bool = false

var accountNames = [
  'appstore1'
  'appstore2'
]

module prodStorage './storageModule.bicep' = [for name in accountNames: if (deployProd) {
  name: 'storage-${name}'
  params: {
    storageAccountName: name
  }
}]
```

###  Step 3: Run Deployment

```bash
az deployment group create \
  --resource-group myRG \
  --template-file main.bicep \
  --parameters deployProd=true
```

---

## Next Steps

**In the next lesson, you'll learn about parameter files and advanced deployment scenarios!**







