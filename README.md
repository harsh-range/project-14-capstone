# Project 14: App Deployment using EKS

## Objective
Deploy an application on Amazon EKS and expose it publicly.

## Prerequisites
- AWS CLI
- kubectl
- eksctl
- Configured AWS account

## Create EKS Cluster

```bash
eksctl create cluster \
--name project14-capstone-cluster \
--region us-east-1 \
--nodes 2 \
--node-type t3.medium
```

## Connect to Cluster

```bash
aws eks update-kubeconfig \
--region us-east-1 \
--name project14-capstone-cluster
```

Verify:

```bash
kubectl get nodes
```

## Deploy Application

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
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
        image: nginx
        ports:
        - containerPort: 80
```

```bash
kubectl apply -f deployment.yaml
```

## Expose Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
```

```bash
kubectl apply -f service.yaml
```

Get public URL:

```bash
kubectl get svc
```

## Verification

Open the LoadBalancer URL in a browser and verify the **"Welcome to nginx!"** page appears.

## Deliverables

- deployment.yaml
- service.yaml
- Screenshot of the public application

## Cleanup

```bash
eksctl delete cluster \
--name project14-capstone-cluster \
--region us-east-1
```