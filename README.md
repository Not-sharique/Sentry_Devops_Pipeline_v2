# Sentry Azure DevOps Integration Pipeline

This project demonstrates how to set up an Azure DevOps pipeline for a Python Flask application that integrates with Sentry to automatically log bugs as work items in Azure DevOps.

## Prerequisites

- Azure DevOps organization and project
- Sentry account (sentry.io)
- Azure subscription (for deployment)

## Setup Steps

### 1. Azure DevOps Setup

1. Go to [Azure DevOps](https://dev.azure.com) and create an organization if you don't have one.
2. Create a new project.
3. Create a new repository in the project.
4. Clone this repository locally or push the code to the new repo.

### 2. Sentry Setup

1. Sign up for a Sentry account at [sentry.io](https://sentry.io).
2. Create a new project:
   - Choose platform: Python
   - Name your project (e.g., "sentry-ado-integration")
3. After creation, copy the DSN from the project settings.

### 3. Azure DevOps Personal Access Token

1. In Azure DevOps, go to User Settings > Personal Access Tokens.
2. Create a new token with the following scopes:
   - Work Items: Read & Write
3. Copy the token (keep it secure).

### 4. Configure Sentry Integration

1. In Sentry, go to Settings > Integrations.
2. Search for "Azure DevOps" and click "Install".
3. Configure the integration:
   - Azure DevOps Organization URL: `https://dev.azure.com/your-organization-name`
   - Personal Access Token: Paste the token from step 3
   - Default Project: Select your Azure DevOps project
4. Save the configuration.

### 5. Set Up Issue Alert Rule

1. In Sentry, go to your project > Alerts.
2. Create a new alert rule:
   - Name: "Create Bug in Azure DevOps"
   - Conditions: When an issue is first seen
   - Actions: Create a work item in Azure DevOps
     - Work Item Type: Bug
     - Project: Your project
     - Title: Use template like "Sentry Issue: {{ issue.title }}"
     - Description: Include issue details

### 6. Pipeline Setup

1. In Azure DevOps, go to Pipelines > New Pipeline.
2. Select "Azure Repos Git" and choose your repository.
3. Select "Existing Azure Pipelines YAML file" and choose `azure-pipelines.yml`.
4. Save and run the pipeline.

### 7. Configure Environment Variables

1. In Azure DevOps, go to Pipelines > Your Pipeline > Edit > Variables.
2. Add a variable named `SENTRY_DSN` with the value from Sentry.

For deployment to Azure Web App:
- Create an Azure Web App in the Azure portal.
- In Azure DevOps, create a service connection for Azure Resource Manager.
- Update the pipeline YAML with your service connection name and app name.
- Set `SENTRY_DSN` in the Web App's Application Settings.

## Testing

1. Run the pipeline in Azure DevOps.
2. After deployment, access the app's `/error` endpoint to trigger a test error.
3. Check Sentry for the error and Azure DevOps for the created bug work item.

## Local Testing

1. Install dependencies: `pip install -r requirements.txt`
2. Set environment variable: `export SENTRY_DSN=your-dsn`
3. Run: `python app.py`
4. Visit `http://localhost:3000/error` to trigger an error.