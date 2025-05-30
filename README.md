# Azure Bicep Fundamentals – Beginner-Friendly Infrastructure as Code

Welcome to **Azure Bicep Fundamentals**, a practical learning repository for those who want to understand and use **Azure Bicep**—Microsoft’s powerful and cleaner domain-specific language (DSL) for deploying Azure resources using **infrastructure as code** (IaC).
This project is designed for learners with limited technical background who want to understand and use [Azure Bicep](https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/overview) to deploy and manage Azure resources using Infrastructure as Code.

---

##  What is Bicep?

**Bicep** is a domain-specific language (DSL) for deploying Azure resources declaratively.  
It simplifies authoring, reduces errors, and provides a more readable experience compared to traditional ARM (Azure Resource Manager) JSON templates.

---

##  Who Is This For?

This repository is designed for:
- **Cloud beginners** with limited technical background
- **IT professionals**, **system admins**, or **students** transitioning into Azure
- Developers or architects seeking to simplify and automate Azure deployments

>  No deep coding or ARM template experience required. We’ll guide you step-by-step.

---
##  Lessons Covered

This repository is organized into a series of step-by-step lessons. Start from the beginning and progress at your own pace!

| Lesson | Topic                                              |
|--------|----------------------------------------------------|
| 1      | Introduction to Bicep and ARM Templates            |
| 2      | Bicep Language Basics                              |
| 3      | Deployment Modes, What-If, Comments, Parenting     |
| 4      | Modules, Conditionals, Loops, Output Variables     |
| 5      | Exporting and Importing Types in Bicep     |
| 6      | Transpiling, Decompiling, and Reverse Engineering with Bicep     |
| ...    | More lessons coming soon!                          |

Each lesson contains clear explanations, practical examples, and hands-on exercises.

>  More lessons coming soon! Each topic is broken down clearly with real examples and beginner-friendly explanations.

---

##  Prerequisites
To follow along, you’ll need:

- **Azure Subscription:** [Sign up here](https://azure.microsoft.com/free/) if you don’t have one.
- **Azure CLI:** [Install Guide](https://docs.microsoft.com/cli/azure/install-azure-cli)
- **Bicep CLI:**  
  Install via Azure CLI:
  ```sh
  az bicep install
  ```
- **VS Code (Recommended):**  
  [Download Visual Studio Code](https://code.visualstudio.com/)
- **VS Code Bicep Extension:**  
  [Install from Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-bicep)
- Basic understanding of cloud services (recommended but not required)

---

##  Repository Structure

```text
/
├── lessons/
│   ├── Lesson1-Introduction-to-Bicep-and-ARM-Templates.md
│   ├── Lesson2-Bicep-Language-Basics.md
│   ├── Lesson3-Bicep-Deployment-Modes-and-Essential-Features.md
│   ├── Lesson4-Bicep-Modules-Conditionals-Loops-Outputs.md
│   └── ... (more lessons)
├── examples/
│   └── (sample Bicep files and deployment scripts)
├── README.md
└── .gitignore
```

---

##  Getting Started

1. **Clone this repo**

   ```bash
   git clone https://github.com/your-username/azure-bicep-fundamentals.git
   cd azure-bicep-fundamentals
   ```

2. **Login to Azure**

   ```bash
   az login
   ```

3. **Create a resource group**

   ```bash
   az group create --name myRG --location eastus
   ```

4. **Deploy a lesson example**

   ```bash
   az deployment group create \
     --resource-group myRG \
     --template-file lesson1-intro/main.bicep
   ```
   
5. **Experiment:**  
   Modify the Bicep files and see how Azure responds!
---

##  Need Help?

- **Official Docs:** [Azure Bicep Documentation](https://docs.microsoft.com/azure/azure-resource-manager/bicep/)
- **Ask in Discussions:** Use the [GitHub Discussions](https://github.com/<your-org-or-username>/azure-bicep-learning/discussions) tab to ask questions.
- **Issues:** Found a problem or have a suggestion? [Open an Issue](https://github.com/<your-org-or-username>/azure-bicep-learning/issues).

---

##  Contributing

Contributions are welcome!  
If you want to improve a lesson or add new examples, please submit a pull request.

---

##  License

This repository is licensed under the [MIT License](LICENSE).

---

##  Acknowledgments

Created and maintained by an experienced **Azure Cloud Architect** to help bridge the gap between theory and hands-on Azure IaC practice.

If you find this repo useful, please ⭐️ it and share it with your peers!

---

###  Next Steps:

Let me know if you'd like this adapted for GitHub Pages with navigation or if you want automated deployment scripts (`deploy.sh`) or GitHub Actions included in the repo.

