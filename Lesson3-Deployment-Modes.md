# Lesson 3: Deployment Modes, What-If, Comments, and Resource Parenting in Bicep

Azure Bicep offers powerful features to help you manage your cloud infrastructure efficiently. In this lesson, you'll learn about deployment modes, previewing changes before deployment, adding comments for clarity, and defining parent-child relationships between resources.

## Objective

By the end of this lesson, you will understand:

* The difference between **incremental** and **complete** deployment modes
* How to safely preview deployments using **`what-if`**
* How to use **comments** in Bicep
* How to define **parent-child relationships** (e.g., subnets inside virtual networks)

---

## 1. Deployment Modes: `incremental` vs `complete`

When deploying Bicep templates, Azure gives you control over how it **handles existing resources** in the target resource group. You choose (a **deployment mode**) this with the `--mode` flag during deployment.This determines how Azure updates your resources in the target resource group.

### **Incremental Mode** (Default)

- **What it does:** Only adds or updates resources defined in your Bicep file. Resources not mentioned in your template remain untouched.
  * **Adds or updates** resources defined in the template
  * **Keeps existing** resources that are not in the template
- **Use case:** Safest for most scenarios, especially when you want to avoid deleting existing resources accidentally. Ideal for most use cases.

**Example:**

```sh
az deployment group create \
  --resource-group my-rg \
  --template-file main.bicep \
  --mode Incremental
```

### **Complete Mode**

- **What it does:** Azure will make the resource group match *exactly* what is defined in your Bicep template. Any resources in the resource group **not** listed in the template will be **deleted**.
  * **Deletes any resource** in the target resource group that is **not** in the Bicep file
- **Use case:** Use with caution! Best when you want to enforce strict control over the resource group’s contents.
  * **Dangerous**  if used without caution — this **overwrites** the environment to match exactly what is declared in the Bicep file.

**Example:**

```sh
az deployment group create \
  --resource-group my-rg \
  --template-file main.bicep \
  --mode Complete
```
---

>  **Think of it like this**:
>
> * `Incremental` = Only change what's needed
> * `Complete` = Replace everything with what's in the template

---

## 2. Safe Preview with `what-if`

**What-If** is a powerful Azure CLI which provides a **dry-run tool** feature that lets you preview changes Azure will make *before* you actually deploy your template.

- **Why use it?** To see what resources will be created, modified, or deleted—helping avoid surprises.

**Example:**

```sh
az deployment group what-if \
  --resource-group my-rg \
  --template-file main.bicep
```

**Typical Output:**
- `Create` indicates a new resource will be added.
- `Modify` indicates an existing resource will be updated.
- `Delete` indicates a resource will be removed (usually in Complete mode).

### Output Example:

```txt
Resource changes:
~ Microsoft.Storage/storageAccounts/mybicepstg123 [2022-09-01]
    sku.name: "Standard_LRS" => "Standard_GRS"
```
**Benefits**:

* No surprises in production
* Review changes before approving
* Validates syntax and parameters

---

## 3. Comments in Bicep

Comments are essential for making your code understandable, especially for teams collaboration and future maintenance.

### Single-line comment 
- **Single-line comment:** Use `//`

**Example:**

```bicep
// This is a storage account for logs
resource logStorage 'Microsoft.Storage/storageAccounts@2022-09-01' = {
  ...
}
```

### Multi-line comment 
 - **Multi-line comment:** Use `/* ... */`

**Example:**

```bicep
/*
This section defines:
- The virtual network
- A subnet for the application tier
*/
```

**Example (inside code):**

```bicep
// This is a single-line comment

/*
  This is a 
  multi-line comment
*/

resource storageAcct 'Microsoft.Storage/storageAccounts@2022-09-01' = {
  name: 'examplestorageacct'
  location: 'eastus'
  sku: { name: 'Standard_LRS' }
  kind: 'StorageV2'
}
```

**Tip**: Keep your Bicep readable. Comment *why* you're doing something — not just *what*.

---

## 4. Parent-Child Resources

In Azure, some resources **must exist inside others**, like:

* Subnets inside virtual networks
* Route tables inside a virtual network
* Virtual machines inside a resource group

- **Bicep uses the `parent` property to define this relationship.**
- This ensures correct deployment order and proper resource hierarchy.

**Example: Creating a subnet as a child of a virtual network**

```bicep
resource vnet 'Microsoft.Network/virtualNetworks@2023-05-01' = {
  name: 'myVnet'
  location: 'eastus'
  properties: {
    addressSpace: {
      addressPrefixes: [
        '10.1.0.0/16'
      ]
    }
  }
}

resource subnet 'Microsoft.Network/virtualNetworks/subnets@2023-05-01' = {
  name: 'mySubnet'
  parent: vnet
  properties: {
    addressPrefix: '10.1.0.0/24'
  }
}
```

### Key Concepts:

* The `subnet` is a **child** of the `vnet`
* Use `parent: vnet` to create that relationship
* Ensures Azure deploys in the correct hierarchy

> Pro Tip: When a resource has a nested name like `virtualNetworks/subnets`, it probably needs a parent.

---

##  Hands-On Exercise
- Try deploying a simple template with both deployment modes.
- Use What-If before every deployment to stay safe.
- Add comments to your Bicep files for clarity.
- Practice defining parent-child relationships for resources like VNets and subnets.

###  Step 1: Create `lesson3.bicep`

```bicep
targetScope = 'resourceGroup'

// Parameters
param storageName string = 'mystorage${uniqueString(resourceGroup().id)}'

// Resource
resource storage 'Microsoft.Storage/storageAccounts@2022-09-01' = {
  name: storageName
  location: resourceGroup().location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {}
}

// Output
output storageId string = storage.id
```

###  Step 2: Run What-If

```bash
az deployment group what-if \
  --resource-group myRG \
  --template-file lesson3.bicep
```

###  Step 3: Deploy in Complete Mode (Test only in empty RG)

```bash
az deployment group create \
  --resource-group myRG \
  --template-file lesson3.bicep \
  --mode Complete
```

---
## **Summary Table**

| Feature         | How to Use                                              | Example / Command                                   |
|-----------------|--------------------------------------------------------|-----------------------------------------------------|
| `Incremental`     | Default, only adds/updates, doesn't delete              | `--mode Incremental`                                |
| `Complete`        | Deletes resources not in template/Replace everything with what’s in the Bicep file                       | `--mode Complete`                                   |
| `What-If`         | Preview changes before deploying                        | `az deployment group what-if ...`                   |
| `Comments`        | `//` for single-line, `/* ... */` for multi-line        | `// Single line`<br>`/* Multi-line */`              |
| `Parent` Property | Define resource hierarchy (child inside parent)          | `parent: vnet` within subnet resource               |

---
Ready for the next lesson? You’ll soon learn about modules and how to organize complex Bicep projects!

