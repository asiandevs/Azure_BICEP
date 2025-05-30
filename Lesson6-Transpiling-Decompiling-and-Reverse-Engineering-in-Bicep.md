# Lesson 5: Exporting and Importing Types in Bicep

##  Lesson Objectives

By the end of this lesson, you will:

* Understand what **types** are in Bicep
* Know how to **import external types** from published Azure resource schemas
* Learn to use the `bicep-types` tool to explore and export types
* Learn best practices for **auto-completion and validation** using types
* Use the **`import` statement** for type definitions

---
##  What Are Types in Bicep?

Types in Bicep define the **structure** of a resource, module, or variable — including available properties, required fields, and data formats.

>  Think of a type as a **template or blueprint** for how a resource should be declared.

---

## Why Export and Import Types?

- **Reusability:** Share complex configurations between modules.
- **Separation of Concerns:** Keep your main files clean and delegate details to modules.
- **Maintainability:** Make changes in one place and use them everywhere.

---

## 1. Exporting Types from a Module

You can **export** (output) values from a Bicep module using the `output` keyword. These can be primitive types (string, int, bool), arrays, or even complex objects.

### Example: Exporting a Storage Account Connection String

**storageAccount.bicep**
```bicep
param storageAccountName string
param location string = resourceGroup().location

resource sa 'Microsoft.Storage/storageAccounts@2022-09-01' = {
  name: storageAccountName
  location: location
  sku: { name: 'Standard_LRS' }
  kind: 'StorageV2'
}

output storageAccountId string = sa.id
output storageAccountName string = sa.name
```

- Here, we export `storageAccountId` and `storageAccountName` from the module.
- You’re using the `Microsoft.Storage/storageAccounts@2022-09-01` type — which has a specific schema defined by Azure.

---

## 2. Importing Types in Parent Modules

When you use a module in a parent Bicep file, you can **import** its outputs and use them as variables in your main deployment.

**main.bicep**
```bicep
module storageModule './storageAccount.bicep' = {
  name: 'deployStorage'
  params: {
    storageAccountName: 'myuniquestorage${uniqueString(resourceGroup().id)}'
    location: 'eastus'
  }
}

output exportedStorageId string = storageModule.outputs.storageAccountId
output exportedStorageName string = storageModule.outputs.storageAccountName
```

- Access a module's output using:  
  `moduleSymbol.outputs.outputName`

---

## 3. Exporting and Importing Complex Types (Objects & Arrays)

You can also export and import more complex types, such as objects and arrays.

### Example: Exporting an Object

**vnetModule.bicep**
```bicep
param vnetName string
param location string

resource vnet 'Microsoft.Network/virtualNetworks@2023-05-01' = {
  name: vnetName
  location: location
  properties: {
    addressSpace: {
      addressPrefixes: [
        '10.1.0.0/16'
      ]
    }
  }
}

output vnetInfo object = {
  id: vnet.id
  name: vnet.name
  location: vnet.location
}
```

**main.bicep**
```bicep
module vnetModule './vnetModule.bicep' = {
  name: 'deployVnet'
  params: {
    vnetName: 'myVnet'
    location: 'eastus'
  }
}

output vnetDetails object = vnetModule.outputs.vnetInfo
```

- Now, `vnetDetails` is an object with multiple properties that you can use in other modules or outputs.

---

## 4. Best Practices

- **Type Consistency:** Always ensure the type you export matches the type you expect to import (string, int, object, array, etc.).
- **Naming:** Use clear and descriptive output names.
- **Documentation:** Comment your outputs for clarity, especially for complex objects.

---

## 5. Step-by-Step Recap

1. **Define outputs in your module using the `output` keyword.**
2. **Reference those outputs from the module in your main Bicep file:**
   - `moduleSymbol.outputs.outputName`
3. **Use these values elsewhere in your deployment, or export them as outputs again.**

---

## Summary Table

| Action         | In Module                           | In Parent (Main)                             |
|----------------|-------------------------------------|----------------------------------------------|
| Export String  | `output foo string = ...`           | `moduleSymbol.outputs.foo`                   |
| Export Object  | `output bar object = { ... }`       | `moduleSymbol.outputs.bar`                   |
| Export Array   | `output items array = [ ... ]`      | `moduleSymbol.outputs.items`                 |

---

## Next Steps

- Try creating a module that exports multiple types (string, object, array).
- Use those outputs in your main file or pass them to other modules.
- This pattern helps you build scalable, maintainable, and professional Azure Bicep solutions!

---
