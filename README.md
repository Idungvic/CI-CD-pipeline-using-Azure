# CI-CD-pipeline-using-Azure
# Azure DevOps CI/CD Pipeline Project

## Project Overview

This project demonstrates the implementation of a complete CI/CD pipeline using Azure DevOps for a containerized web application.

The objective of the project was to automate the entire software delivery lifecycle, from source code integration to container deployment using Azure cloud services.

The project integrates:

* GitHub for version control
* Azure DevOps for CI/CD automation
* Docker for application containerization
* Azure Container Registry (ACR) for image storage
* Azure App Service for hosting the application

The implementation follows modern DevOps practices by enabling automated builds, automated deployments, and continuous integration workflows.

---

# Project Architecture

The workflow of the project follows this structure:

```text
Developer
   ↓
VS Code
   ↓
GitHub Repository
   ↓
Azure DevOps Pipeline Trigger
   ↓
Build Docker Image
   ↓
Push Image to Azure Container Registry
   ↓
Deploy Container to Azure App Service
   ↓
Public Website Access
```

---

# Technologies Used

| Technology               | Purpose                      |
| ------------------------ | ---------------------------- |
| GitHub                   | Source code management       |
| Azure DevOps             | CI/CD automation             |
| Docker                   | Application containerization |
| Azure Container Registry | Container image storage      |
| Azure App Service        | Web application hosting      |
| VS Code                  | Development environment      |
| Git                      | Version control              |

---

# Project Structure

```text
.
├── index.html
├── Dockerfile
├── azure-pipelines.yml
└── README.md
```

---

# Application Setup

## Step 1: Local Project Creation

A local project directory was created using Visual Studio Code.

The project contained:

* HTML application file
* Dockerfile
* Azure pipeline YAML file

The application file was created to verify successful deployment through the CI/CD pipeline.

### index.html

```html
<!DOCTYPE html>
<html>
<head>
    <title>Azure DevOps CI/CD</title>
</head>
<body>
    <h1>CI/CD Pipeline Successful</h1>
</body>
</html>
```

---

# Docker Containerization

The application was containerized using Docker and served with NGINX.

## Dockerfile

```dockerfile
FROM nginx:latest

COPY . /usr/share/nginx/html

EXPOSE 80
```

## Why Docker Was Used

Docker was used to:

* Package the application into a portable container
* Ensure consistency across environments
* Simplify deployment to Azure App Service
* Enable automated CI/CD workflows

---

# GitHub Repository Setup

A GitHub repository was created to store the project source code.

The repository was connected to the local machine using Git.

## Git Commands Used

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin <repository-url>
git push -u origin main
```

## Why GitHub Was Used

GitHub was used to:

* Manage source code versions
* Enable collaboration
* Trigger Azure DevOps pipelines automatically
* Maintain centralized project storage

---

# Azure Container Registry (ACR)

An Azure Container Registry was created to securely store Docker images generated during the pipeline process.

## Registry Name

```text
codealphamyacrregistry
```

## Image Repository

```text
idungviccicdpipelineusingazure
```

---

# Azure DevOps Pipeline Setup

Azure DevOps was connected to the GitHub repository to automate the CI/CD workflow.

The pipeline was configured using YAML.

## CI/CD Pipeline Workflow

The pipeline performs the following tasks automatically:

1. Pull source code from GitHub
2. Build Docker image
3. Push image to Azure Container Registry
4. Deploy container to Azure App Service
5. Publish application updates automatically

---

# Azure Pipeline Configuration

## azure-pipelines.yml

```yaml
trigger:
- main

resources:
- repo: self

variables:
  dockerRegistryServiceConnection: 'acr-service-connection'
  imageRepository: 'idungviccicdpipelineusingazure'
  containerRegistry: 'codealphamyacrregistry.azurecr.io'
  dockerfilePath: '$(Build.SourcesDirectory)/Dockerfile'
  tag: '$(Build.BuildId)'
  vmImageName: 'ubuntu-latest'

stages:

- stage: Build
  displayName: Build and Push Stage

  jobs:
  - job: Build
    pool:
      vmImage: $(vmImageName)

    steps:
    - task: Docker@2
      displayName: Build and Push Image
      inputs:
        command: buildAndPush
        repository: $(imageRepository)
        dockerfile: $(dockerfilePath)
        containerRegistry: $(dockerRegistryServiceConnection)
        tags: |
          $(tag)
          latest

- stage: Deploy
  displayName: Deploy to Azure App Service

  jobs:
  - job: Deploy
    pool:
      vmImage: $(vmImageName)

    steps:
    - task: AzureWebAppContainer@1
      displayName: Deploy to App Service

      inputs:
        azureSubscription: 'Devops-project'
        appName: 'codealpha'
        containers: 'codealphamyacrregistry.azurecr.io/idungviccicdpipelineusingazure:latest'
```

---

# Build Stage Explanation

The Build Stage performs:

* Docker image creation
* Image tagging
* Image upload to Azure Container Registry

Each pipeline execution generates a new container image version.

The `latest` tag was also implemented to ensure stable deployments from Azure App Service.

---

# Deployment Stage Explanation

The Deploy Stage automatically:

* Pulls the latest container image from ACR
* Deploys the container to Azure App Service
* Publishes the updated application online

This eliminates the need for manual deployment.

---

# Azure App Service Deployment

An Azure Linux Web App was created to host the Docker container.

## Web App Name

```text
codealpha
```

## Deployment Type

```text
Docker Container Deployment
```

---

# Managed Identity & Security Configuration

To allow Azure App Service securely access the Azure Container Registry, Managed Identity authentication was configured.

## Security Configurations Implemented

* System Assigned Managed Identity enabled
* AcrPull IAM role assigned
* Azure Resource Manager service connection configured
* Docker Registry service connection configured

## Why Managed Identity Was Used

Managed Identity was used to:

* Eliminate hardcoded credentials
* Improve cloud security
* Enable secure authentication between Azure services

---

# Continuous Integration (CI)

The Continuous Integration process automatically:

* Detects code changes pushed to GitHub
* Triggers Azure DevOps pipeline
* Builds updated Docker image
* Pushes image to Azure Container Registry

This ensures rapid and consistent software integration.

---

# Continuous Deployment (CD)

The Continuous Deployment process automatically:

* Pulls the latest Docker image from ACR
* Deploys updated container to Azure App Service
* Makes application updates available publicly

This enables faster and automated software delivery.

---

# Pipeline Monitoring

Pipeline execution and deployment monitoring were performed using:

* Azure DevOps Pipeline Runs
* Azure App Service Log Stream
* Azure Portal Monitoring
* Deployment logs

Monitoring helped identify and resolve deployment and authentication issues during implementation.

---

# Key Challenges Encountered

## ACR Authentication Failure

During deployment, Azure App Service failed to pull images from Azure Container Registry due to authorization issues.

### Error Encountered

```text
ImagePullUnauthorizedFailure
```

### Resolution

The issue was resolved by:

* Enabling Managed Identity on Azure App Service
* Assigning the AcrPull role to the App Service identity
* Configuring Azure App Service to authenticate using Managed Identity

This allowed secure image pulling from ACR.

---

# Live Application URL

```text
https://codealpha-gse9cxe5eyc3f4fr.canadacentral-01.azurewebsites.net
```

---

# Learning Outcomes

This project provided hands-on experience in:

* CI/CD pipeline automation
* Docker containerization
* Azure DevOps YAML pipelines
* Azure App Service deployment
* Azure Container Registry integration
* Cloud authentication and IAM
* GitHub integration with Azure DevOps
* Troubleshooting cloud deployment issues

---

# Conclusion

The project successfully implemented a complete CI/CD pipeline using Azure cloud technologies.

The final solution automated:

* Source code integration
* Docker image creation
* Container registry management
* Web application deployment

The implementation demonstrates modern DevOps engineering practices using Microsoft Azure services.

---

# Author

Victor Idung

