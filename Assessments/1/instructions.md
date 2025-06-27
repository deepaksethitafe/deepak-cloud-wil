# 💼 Azure Bicep Deployment Guide

This document contains steps to build and deploy Bicep templates using the Azure CLI.

Each resource must have a Tag with the Key: Environment set to Value: Development

---

##	Log into Azure Portal
##  Launch Cloud Shell
##  Using PowerShell or CLI commands

## Step 1: Create a resource group
Azure CLI: 

`az group create --name rg_assess1 --location australiaeast --tags Environment=Development`


## Step 2: Create a database inside the resource group.

serverName="assess1Server"  
adminLogin="adminUser"  
password="P@ssw0rd!"  
az sql server create --name $serverName --resource-group rg_assess1 --location australiaeast --admin-user $adminLogin --admin-password $password  
az sql db create --resource-group rg_assess1 --server $serverName --name assess1DB --service-objective Basic --tags Environment=Development

## Step 3: Create an app service plan in the resource group.
az appservice plan create --name assess1AppServicePlan --resource-group rg_assess1 --location australiaeast --sku S1 --tags Environment=Development


## Step 4:	Create an app service in the plan in the resource group to house the API layer.
az webapp create --name assess1APIApp --resource-group rg_assess1 --plan assess1AppServicePlan --tags Environment=Development  


## Step 5: Create an app service in the plan in the resource group to house the user interface layer.
az webapp create --name assess1UIApp --resource-group rg_assess1 --plan assess1AppServicePlan --tags Environment=Development


Use the Azure CLI to transpile Bicep files into ARM templates.

```bash
# Azure CLI
bicep build web.bicep
bicep build sql.bicep
