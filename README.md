# GithubActions: How to deployt a .NET 8 Web API

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
