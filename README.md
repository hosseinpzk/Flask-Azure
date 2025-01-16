# Flask-Azure: Running a Flask Project on Azure Cloud

## Step 1: Build and Run the Docker Image

To build and run the Flask application container locally, follow these steps:

1. **Build the Docker image:**

   ```bash
   docker build -t flask-app .
   ```

2. **Run the Docker image:**

   ```bash
   docker run -p 5000:5000 flask-app
   ```

## Step 2: Set Up Azure Resources

Before deploying to Azure Kubernetes Service (AKS), create the necessary resources.

1. **Login to Azure:**

   ```bash
   az login
   ```

2. **Create a Resource Group:**

   ```bash
   az group create --name myResourceGroup --location eastus
   ```

3. **Create Azure Container Registry (ACR):**

   ```bash
   az acr create --resource-group myResourceGroup --name myacr123 --sku Basic
   ```

4. **Create AKS Cluster with ACR Integration:**

   ```bash
   az aks create \
     --resource-group myResourceGroup \
     --name myAKSCluster \
     --node-count 1 \
     --generate-ssh-keys \
     --attach-acr myacr123
   ```

## Step 3: Prepare Your Project Repository

Ensure your repository structure looks like this:

```
/your-project
├── app.py
├── requirements.txt
├── Dockerfile
├── azure-pipelines.yml
└── k8s/
    ├── deployment.yaml
    └── service.yaml
```

## Step 4: Set Up Azure DevOps

1. **Go to Azure DevOps**: Navigate to [dev.azure.com](https://dev.azure.com).
2. **Create or use an existing project**.
3. **Configure Service Connections**:

   - **ACR Service Connection**:
     - Go to `Project Settings > Service connections`.
     - Choose `New service connection > Docker Registry > Azure Container Registry`.
     - Select your subscription and ACR, then name it `acr-service-connection`.

   - **AKS Service Connection**:
     - Go to `Project Settings > Service connections`.
     - Choose `New service connection > Kubernetes > Select your subscription and AKS cluster`.
     - Name it `aks-service-connection`.

## Step 5: Create and Configure Azure Pipeline

1. **Create a Pipeline**:
   - Go to `Pipelines > New Pipeline`.
   - Choose `Azure Repos Git` (or your source repository).
   - Select your repository and choose `Existing Azure Pipelines YAML file`.
   - Select `/azure-pipelines.yml`.

2. **Update Pipeline Variables**:
   In `azure-pipelines.yml`, replace the following values:

   ```yaml
   acrName: 'myacr123.azurecr.io'  # Your ACR name
   resourceGroup: 'myResourceGroup'  # Your resource group name
   aksClusterName: 'myAKSCluster'  # Your AKS cluster name
   ```

3. **Run the Pipeline**:
   - Save and run the pipeline.
   - Monitor the stages as they execute.

## Step 6: Verify the Deployment

1. **Get AKS credentials**:

   ```bash
   az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
   ```

2. **Check Pods**:

   ```bash
   kubectl get pods
   ```

3. **Check Service (to get the external IP)**:

   ```bash
   kubectl get service flask-app
   ```

---
.
