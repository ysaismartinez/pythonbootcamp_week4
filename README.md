# Container MLOps Template repository

Learn how to create a container and package it with GitHub Actions. This repository template gives you a good starting point for a Dockerfile, GitHub Actions workflow, and Python code.

Depending on the type of model you need, you will need different workflow steps and most definitely a different `main.py` file. The default uses FastApi

# Learn objectives

*
*
*
*
*

## Deploy your API to the Azure Cloud

This deployment can be done at no cost, using free resources with an Azure subscription. Use one of these to deploy this:

- [Sign in to your account]()
- [Create a (no Credit Card required) Azure For Students account]()
- [Create a new Azure account]()


## Create an Azure App Service

1. Open an [Azure Cloud Shell](https://shell.azure.com/?WT.mc_id=academic-0000-alfredodeza) to use the `az` cli
1. Create a *Resource Group*:
```
az group create --name demo-huggingface --location "East US"
```
1. Create the **FREE** App Service Plan:
```
az appservice plan create --name "demo-huggingface" --resource-group demo-huggingface --is-linux --sku FREE
```
1. Create a random identifier for a unique webapp name:
```
let "randomIdentifier=$RANDOM*$RANDOM"
```
1. Create the web app with a placeholder container using the `randomIdentifier` from before
```
az webapp create --name "demo-huggingface-$randomIdentifier" --resource-group demo-huggingface --plan demo-huggingface --deployment-container-image mcr.microsoft.com/appsvc/staticsite:latest
```
1. Head to the [App Service](https://portal.azure.com/#view/HubsExtension/BrowseResource/resourceType/Microsoft.Web%2Fsites) and confirm that your service is up and running


## Create an Azure Service Principal

You'll need the following:

1. An Azure subscription ID [find it here](https://portal.azure.com/#view/Microsoft_Azure_Billing/SubscriptionsBlade) or [follow this guide](https://docs.microsoft.com/en-us/azure/azure-portal/get-subscription-tenant-id)
1. A Service Principal with the following details the AppID, password, and tenant information. Create one with: `az ad sp create-for-rbac -n "REST API Service Principal"` and assign the IAM role for the subscription. Alternatively set the proper role access using the following command (use a real subscription id and replace it):

```
az ad sp create-for-rbac --name "CICD" --role contributor --scopes /subscriptions/$AZURE_SUBSCRIPTION_ID --sdk-auth
```

Capture the JSON output and add it as a [repository secret](/../../settings/secrets/actions/new) with the name `AZURE_CREDENTIALS`

