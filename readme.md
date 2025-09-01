# Greetings

## Prerequisites
1. docker installed
2. Kind cluster installed
3. python3 installed
4. virtualenv installed 

### local deployment
```
virtualenv env
```
```
source env/bin/activate
```
```
pip install -r requirements.txt
```
```
python3 manage.py migrate
```
```
python3 manage.py runserver 
```

### docker deployment 
```
docker build -t django-app .
```
```
docker run -d -p 8000:8000 django-app
```

### Kubernetes Deployment
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
```
kubectl port-forward service/django-service 80:80 -n django --address=0.0.0.0 
```
### visit http://localhost:80 to see the deployment 