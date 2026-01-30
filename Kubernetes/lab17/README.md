
# Lab 17: Pod Resource Management with CPU and Memory Requests and Limits

## Objective
- Add CPU and memory requests and limits to Node.js deployment.
- Verify resource settings and monitor usage.

## Environment
- Kubernetes cluster with Node.js app (`node-app`) and MySQL service.
- Metrics Server for monitoring.

## Steps & Commands

### 1. Verify Cluster and Deployment
```bash
kubectl get nodes
kubectl config current-context
kubectl get deployments
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab17/Screenshots/Verify%20Cluster%20and%20Deployment.png?raw=true)
### Step 2: Edit Deployment to Add Resource Requests and Limits
```bash
kubectl edit deployment node-app

resources:
  requests:
    cpu: "1"
    memory: "1Gi"
  limits:
    cpu: "2"
    memory: "2Gi"
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab17/Screenshots/Edit%20the%20Node.js%20deployment%20to%20add%20resources%202.png?raw=true)
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab17/Screenshots/Edit%20the%20Node.js%20deployment%20to%20add%20resources.png?raw=true)
### Step 3: Verify Pod Creation and Resource Allocation
```bash
kubectl get pods
kubectl describe pod node-app-74c7c4665b-sxdnx 
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab17/Screenshots/Verify%20Pod%20Creation%20and%20Resource%20Allocation.png?raw=true)
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab17/Screenshots/Verify%20Pod%20Creation%20and%20Resource%20Allocation%202.png?raw=true)
### Step 4: Monitor Pod Resource Usage
```bash
kubectl top pod node-app-74c7c4665b-sxdnx 
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab17/Screenshots/Monitor%20Pod%20Resource%20Usage.png?raw=true)


