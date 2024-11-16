# Flask-Azure
runnig flask project in azure cloud


for creating image: 
docker build -t flask-app .

for running image:
docker run -p 5000:5000 flask-app
======================================================
on aks in azure:
First, create these Azure resources (using Azure Cloud Shell or Azure CLI):
---
az login

# Create Resource Group
az group create --name myResourceGroup --location eastus

# Create Azure Container Registry (ACR)
az acr create --resource-group myResourceGroup \
              --name myacr123 \
              --sku Basic

# Create AKS cluster with ACR integration
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --generate-ssh-keys \
    --attach-acr myacr123
---

2 - create dep.yml
