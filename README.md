Here is a GitHub-ready `README.md` format. You can copy it directly into your repository as `Azure-DevOps-CICD-Notes.md`.

# Azure DevOps CI/CD Pipeline with Docker, ACR & Kubernetes (AKS)

## 1. What is CI/CD?

CI/CD is a DevOps practice that automates building, testing, and deploying applications whenever developers push code changes.

- **CI (Continuous Integration):** Automatically builds and tests updated code.
- **CD (Continuous Delivery/Deployment):** Automates releasing and deploying the application.

---

## 2. Complete CI/CD Pipeline Architecture

```
             👨‍💻 DEVELOPER
          Makes changes in code
                   |
                   v
          🐙 GITHUB REPOSITORY
        Stores application code,
          Dockerfile and YAML
                   |
                   v
           ⚡ PIPELINE TRIGGER
       GitHub notifies Azure DevOps
                   |
                   v
          🔧 AZURE DEVOPS AGENT
          Downloads latest code
                   |
                   v
          ┌────────────────────┐
          │    CI PIPELINE     │
          │                    │
          │ 1. Build Code      │
          │ 2. Run Tests       │
          │ 3. Build Docker    │
          │    Image           │
          └────────────────────┘
                   |
                   v
             🐳 DOCKER IMAGE
             myapp:v2
                   |
                   v
       ☁️ AZURE CONTAINER REGISTRY
            (ACR)
         Stores Docker Image
                   |
                   v
          ┌────────────────────┐
          │    CD PIPELINE     │
          │                    │
          │ 1. Connect to AKS  │
          │ 2. Update Image    │
          │ 3. Deploy App      │
          └────────────────────┘
                   |
                   v
          ☸️ KUBERNETES (AKS)
          Pulls new Docker Image
                   |
                   v
              🟢 NEW PODS
          Updated application runs
```

---

## 3. Step-by-Step Explanation

| Step | Component        | What happens?                                      |
| ---- | ---------------- | -------------------------------------------------- |
| 1    | Developer        | Makes changes in application code.                 |
| 2    | GitHub           | Stores the updated source code.                    |
| 3    | Pipeline Trigger | Notifies Azure DevOps about code changes.          |
| 4    | Azure Agent      | Downloads the latest code and executes tasks.      |
| 5    | Build & Test     | Builds the application and runs automated tests.   |
| 6    | Docker           | Uses the existing Dockerfile to build a new image. |
| 7    | Docker Image     | Creates a new application image, e.g. `myapp:v2`.  |
| 8    | ACR              | Stores the newly created Docker image.             |
| 9    | CD Pipeline      | Connects to AKS and updates the deployment.        |
| 10   | AKS              | Pulls the new image from ACR.                      |
| 11   | Kubernetes Pods  | Runs the updated application in containers.        |

---

## 4. What Happens When We Change Application Code?

Suppose the existing Python application contains:

```
print("Welcome Manoj")
```

Now we change it to:

```
print("Welcome to Azure DevOps")
```

Push the changes to GitHub:

```
git add .
git commit -m "Updated application"
git push origin main
```

### What happens automatically?

1. GitHub receives the updated code.
2. Azure DevOps pipeline gets triggered.
3. Pipeline agent downloads the latest code.
4. Application is built and tested.
5. Docker reads the existing Dockerfile.
6. Docker builds a new image using the updated code.
7. New image is pushed to ACR.
8. Pipeline updates the Kubernetes deployment.
9. Kubernetes creates new pods using the new image.

**Important:** CI/CD does not create a new Dockerfile every time. It uses the existing Dockerfile to build a new Docker image with updated application code.

---

## 5. Important Docker Commands

### Build Docker Image

```
docker build -t myapp:v2 .
```

Creates a new Docker image using the Dockerfile in the current directory.

### Login to ACR

```
az acr login --name myregistry
```

Authenticates Docker with Azure Container Registry.

### Tag Docker Image

```
docker tag myapp:v2 myregistry.azurecr.io/myapp:v2
```

Assigns the ACR repository name to the image.

### Push Image to ACR

```
docker push myregistry.azurecr.io/myapp:v2
```

Uploads the image to Azure Container Registry.

### Update Kubernetes Deployment

```
kubectl set image deployment/myapp myapp=myregistry.azurecr.io/myapp:v2
```

Updates the Kubernetes deployment to use the new image.

---

## 6. Important Components to Remember

| Component             | Purpose                                      |
| --------------------- | -------------------------------------------- |
| GitHub                | Stores source code.                          |
| Azure Pipelines       | Automates CI/CD.                             |
| Pipeline Agent        | Executes pipeline instructions.              |
| Dockerfile            | Contains instructions to build an image.     |
| Docker Image          | Packages application and dependencies.       |
| ACR                   | Stores Docker images.                        |
| AKS                   | Runs and manages containers.                 |
| Kubernetes Deployment | Manages application pods and image versions. |
| Pods                  | Run application containers.                  |

---

## 7. Easy Way to Remember

### CI – Continuous Integration

**Code → Build → Test → Docker Image**

Prepares and verifies the updated application.

### CD – Continuous Delivery / Deployment

**ACR → AKS → Pods**

Stores and deploys the updated application.

---

## 8. Interview Question

**Q: Explain a CI/CD pipeline using Azure DevOps, Docker and Kubernetes.**

**Answer:**

When a developer pushes code changes to GitHub, Azure DevOps automatically triggers the pipeline. The pipeline agent downloads the latest source code, builds and tests the application, and creates a Docker image using the existing Dockerfile. The image is pushed to Azure Container Registry. After that, the deployment pipeline connects to Azure Kubernetes Service and updates the Kubernetes deployment with the new image version. Kubernetes pulls the image and runs updated application pods.

### Remember the complete flow:

**GitHub → Azure DevOps → Build & Test → Docker Image → ACR → AKS → Pods**

**GitHub tip:** Keep this file in a folder such as `Azure-DevOps/Notes/` so you can easily find it during revision.
