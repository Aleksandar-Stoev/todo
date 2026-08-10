# Todo App - Azure Deployment Note

This project is configured and deployed to Azure App Service using a custom CI/CD workflow.

## Deployment Architecture

Due to a blocking synchronization issue with the native GitHub OAuth token within the Azure Portal (`Cannot find SourceControlToken with name GitHub`), the deployment strategy was adapted to ensure a robust and automated workflow.

The deployment was achieved using two reliable methods:

1. **Initial Deployment (Local Git):**
   * Configured via Azure CLI and deployed directly from the local repository using:
     ```bash
     git push azure main:master
     ```
   * This bypassed the broken portal authorization entirely and verified that the application builds successfully on the App Service environment.

2. **Automated CI/CD Pipeline (GitHub Actions & Service Principal):**
   * To achieve full automation upon code changes, a custom GitHub Actions workflow (`.github/workflows/`) was created manually.
   * Authentication with Azure is handled securely using an Azure Service Principal via the **`AZURE_CREDENTIALS`** secret, instead of the buggy classic GitHub token integration.
   * Any subsequent `git push origin main` triggers the GitHub runner to automatically build and deploy the latest version to Azure.

## Tech Stack & Configuration
* **Runtime:** Node.js (`22.x`)
* **Hosting Platform:** Azure App Service
* **Database & Configuration:** Handled via Environment Variables and safe fallbacks (`connections.js`).
