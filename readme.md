# LocalMate App

Welcome to the **LocalMate App** repository. This branch showcases the demo version of the application, highlighting its core features and functionalities.

## 🧰 Prerequisites

Before running the application locally or deploying it, ensure you have the following installed:

```
- Docker
- Kubernetes (Kind)
- Python 3
- Virtualenv
- Jenkins (for CI/CD)
```

## 🚀 Local Deployment
To run the application locally:

1. Set up a virtual environment:
```
virtualenv env
```
2. Activate the virtual environment:
```
source env/bin/activate
```
3. Install the required dependencies:
```
pip install -r requirements.txt
```
4. Apply database migrations:
```
python3 manage.py migrate
```
5. Start the development server:
```
python3 manage.py runserver 
```

Access the application at http://localhost:8000.


## 🐳 Docker Deployment

1. Build the Docker image:
```
docker build -t django-app .
```
2. Run the Docker container:
```
docker run -d -p 8000:8000 django-app
```

The application will be accessible at http://localhost:8000.

## ☸️ Kubernetes Deployment (Using Kind)
1. Create a Kubernetes cluster using Kind:
```
kind cluster create --name django-cluster --config=config.yml
```
```
config.yml

kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
    image: kindest/node:v1.31.0
  - role: worker
    image: kindest/node:v1.31.0

```
```
kubectl get nodes
```

### Apply the Kubernetes configurations:

```
kubectl apply -f namespace.yml
```
```
kubectl apply -f deployment.yml
```
```
kubectl apply -f service.yml
```
```
kubectl get all -n django
```

Access the application:
```
kubectl port-forward service/django-service 80:80 -n django --address=0.0.0.0 
```

visit http://localhost:80 to see the deployment 


## 🔧 Jenkins CI/CD Pipeline (DevSecOps)

This repository includes a Jenkins pipeline (Jenkinsfile) that automates the following steps:

1. Clean Workspace :
- Deletes old build artifacts to ensure a clean environment.

2. Checkout Code :
- Pulls the latest code from the GitHub repository.

3. Dependency Check (OWASP / Trivy) :
- Scans the project for security vulnerabilities in dependencies.

4. SonarQube Analysis :
- Performs code quality and security analysis using SonarQube.

5. Build Docker Image :
- Creates a Docker image for the application.

6. Push to Docker Registry :
- Pushes the image to Docker Hub (or any configured registry).

7. :Deploy to Kubernetes (Kind Cluster) :
- Applies Kubernetes manifests (Deployment, Service, Ingress) to deploy the app in a local Kind cluster.


### ✅ Notes 

- Jenkins triggers can be automated via GitHub webhooks on push events.

- Using ngrok can expose local Jenkins to GitHub for webhook testing.

- The pipeline ensures security, quality, and deployment are automated in a single workflow.

![hello](https://github.com/DattaRahegaonkar/Localmate-App/blob/12656c1d7d84b5d57c5306e6a811d397a56971bc/Jenkins%20(%20CI-CD%20)%20Pipeline%20.png)
