```markdown
# E-Commerce Deployment

## Prerequisites

1. **Clone the repository**: Begin by cloning the e-commerce deployment repository using the following command:
   ```bash
   git clone https://github.com/sohitkumar/e-commerce-deployment.git
   ```
2. **Navigate to the repository directory**:
   ```bash
   cd e-commerce-deployment
   ```

## Cluster Creation

Create an EKS cluster using the following command:
```bash
eksctl create cluster \
  --name ecommerce-cluster \
  --region ap-south-1 \
  --nodegroup-name t2-medium-nodegroup \
  --node-type t2.medium \
  --nodes 2 \
  --nodes-min 2 \
  --nodes-max 4 \
  --managed
```

## Deployment

### Frontend Components

Deploy the frontend configurations, deployment, services, and HPA:
```bash
kubectl apply -f deployment/frontend/fe-configmap.yaml
kubectl apply -f deployment/frontend/fe-deployment.yaml
kubectl apply -f deployment/frontend/fe-service.yaml
kubectl apply -f deployment/frontend/fe-hpa.yaml
```

### Database Components

Deploy the MongoDB configurations, secrets, deployment, services, and PVC:
```bash
kubectl apply -f deployment/db/mongo-secret.yaml
kubectl apply -f deployment/db/mongo-deployment.yml
kubectl apply -f deployment/db/mongo-service.yml
kubectl apply -f deployment/db/mongo-pvc.yaml
```

### Backend Components

Deploy the backend configurations, secrets, deployment, services, and HPA:
```bash
kubectl apply -f deployment/backend/be-configmap.yaml
kubectl apply -f deployment/backend/be-secret.yaml
kubectl apply -f deployment/backend/be-deployment.yaml
kubectl apply -f deployment/backend/be-service.yaml
kubectl apply -f deployment/backend/be-hpa.yaml
```

### RabbitMQ Components

Deploy RabbitMQ deployment, PVC, and services:
```bash
kubectl apply -f deployment/backend/rabbitmq/rabbitmq-deployment.yaml
kubectl apply -f deployment/backend/rabbitmq/rabbitmq-pvc.yaml
kubectl apply -f deployment/backend/rabbitmq/rabbitmq-service.yaml
```

## Load Balancer and Ingress Setup

Ensure your Kubernetes cluster is running and accessible:
```bash
kubectl cluster-info
```

### Install Helm

Ensure Helm (version 3 or above) is installed:
```bash
helm version
```

### Add Ingress-nginx Helm Repository

Add the ingress-nginx repository and update it:
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```

### Install the Ingress-nginx Controller

Optionally, specify the version of ingress-nginx for compatibility:
```bash
helm install ingress-nginx ingress-nginx/ingress-nginx \
    --namespace ingress-nginx \
    --create-namespace \
    --set controller.service.externalTrafficPolicy=Local
```

### Verify the Installation

Check the running resources in the ingress-nginx namespace:
```bash
kubectl get all -n ingress-nginx
```

### Apply Ingress Configuration

Apply the ingress configuration to route traffic to your services:
```bash
kubectl apply -f deployment/ingress/ingress.yaml
```

### Confirm Ingress Setup

Verify that the ingress resource is set up correctly:
```bash
kubectl get ingress
```

### Verify NGINX Ingress Controller Status

Check the status of the ingress-nginx controller pods:
```bash
kubectl get pods -n ingress-nginx
```

## Cluster Autoscaling

For information on setting up cluster autoscaling, refer to the following document:
[Cluster Autoscaling Info](observability-and-scaling/autoscale/info.md)
```