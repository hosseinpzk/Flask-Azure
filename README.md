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

2 - create deployment.yml
3 - create azure-pipeline.yaml
4 - Prepare repository
===
# Your repository structure should look like this:
/your-project
  ├── app.py
  ├── requirements.txt
  ├── Dockerfile
  ├── azure-pipelines.yml
  └── k8s/
      ├── deployment.yaml
      └── service.yaml
===
5 - Set Up Azure DevOps:

- Go to dev.azure.com
- Create a new project or use existing one
- Go to Project Settings > Service connections


6 - Create Service Connections:

6.1 : Create ACR Service Connection:
-- Go to Project Settings > Service connections
-- New service connection
-- Choose "Docker Registry"
-- Select "Azure Container Registry"
-- Select your subscription and ACR
-- Name it "acr-service-connection"

6.2 : Create AKS Service Connection:
-- New service connection
-- Choose "Kubernetes"
-- Select your subscription and AKS cluster
-- Name it "aks-service-connection"




7 - Create the Pipeline:
Go to Pipelines > New Pipeline
Choose Azure Repos Git (or your source)
Select your repository
Choose "Existing Azure Pipelines YAML file"
Select /azure-pipelines.yml


8 - Update Variables:
Replace these values in azure-pipelines.yaml:

acrName: 'myacr123.azurecr.io'  # Your ACR name
resourceGroup: 'myResourceGroup'  # Your resource group
aksClusterName: 'myAKSCluster'   # Your AKS cluster name

9 - Run the Pipeline:
Save and run the pipeline
Review the stages as they execute


10 - Verify Deployment:
# Get AKS credentials
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster

# Check pods
kubectl get pods

# Check service (to get external IP)
kubectl get service flask-app


