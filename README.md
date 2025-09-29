1. Install Prerequisites
a) Install Docker

Docker is required for building and running container images locally.

Docker Desktop
 works well for Windows/macOS.

On Linux:

sudo apt-get update
sudo apt-get install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker


Test:

docker --version

b) Install Minikube

Minikube spins up a local Kubernetes cluster.

Install:

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube


Verify:

minikube version

c) Install kubectl

kubectl is the Kubernetes command-line tool.

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client

2. Start Minikube
minikube start --driver=docker


Verify cluster:

kubectl cluster-info
kubectl get nodes

3. Deploy a Sample Application
a) Create a Deployment

Example: Nginx deployment.

nginx-deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80


Apply:

kubectl apply -f nginx-deployment.yaml
kubectl get deployments
kubectl get pods

b) Expose the Deployment via Service

nginx-service.yaml

apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort


Apply:

kubectl apply -f nginx-service.yaml
kubectl get services


Access the app:

minikube service nginx-service
  
