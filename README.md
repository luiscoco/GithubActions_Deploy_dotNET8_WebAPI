GithubActions: How to deploy .NET 8 Web API in Azure Web App

## 1. Creating the Service Principal and Obtaining Azure Credentials

Run this command for getting the Azure Credentials:

```
az ad sp create-for-rbac --name "myserviceprincipalluis" ^
--role contributor ^
--scopes /subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX ^
--sdk-auth
```

## 2. We create the Github repository secret for storing the AZURE_CREDENTIALS

We create a new Secret for storing the **AZURE_CREDENTIALS**:

```
{
  "clientId": "b47e48b4-895f-4bd7-818e-2da0deb8cd0c",
  "clientSecret": "Bhd8Q~OY7xS.C0gQ5~yXQu4xjZZQChyKL9yqnb0I",
  "subscriptionId": "846901e6-da09-45c8-98ca-7cca2353ff0e",
  "tenantId": "e099cebd-5eea-41a3-88db-bcb9a9cba83e"
}
```

## 3. Github actions main.yaml file

See this github repo for more information: https://github.com/luiscoco/GithuActions_Azure_SDK_for_dotNET

To set up a GitHub Actions workflow for continuously deploying your Web API to Azure using the provided command, you need to create a .yml file in your GitHub repository under the .github/workflows directory. 

The file, often named main.yml or something similar, defines the workflow. Below is an example of how your main.yml file could look. 

This workflow will trigger on every push to the main branch and will require you to set up Azure credentials as GitHub secrets.

```yaml
name: Azure WebApp Deployment

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2

    - name: Set up Python
      uses: actions/setup-python@v1
      with:
        python-version: '3.x'

    - name: Install Azure CLI
      run: |
        curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

    - name: Login to Azure
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}

    - name: Deploy to Azure Web App
      run: |
        az webapp up \
        --name myWebAppluiscoco19777 \
        --resource-group myRG \
        --location EastUS \
        --sku B1 \
        --os-type Windows \
        --runtime "dotnet:8"
```

## 4. Verify in Azure Portal the new Web app service

![image](https://github.com/luiscoco/GithubActions_Deploy_dotNET8_WebAPI/assets/32194879/f7ceca0a-bb02-4d89-8e60-88a92c64ce0b)

![image](https://github.com/luiscoco/GithubActions_Deploy_dotNET8_WebAPI/assets/32194879/4b9396f3-d2fb-4f60-96aa-89ce1c9687fd)



