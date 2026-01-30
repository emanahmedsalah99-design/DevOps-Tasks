
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
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab10/Screenshots/Taint%20the%20Worker%20Node.png?raw=true)
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
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab10/Screenshots/Taint%20the%20Worker%20Node.png?raw=true)
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab10/Screenshots/Taint%20the%20Worker%20Node.png?raw=true)
### Step 3: Verify Pod Creation and Resource Allocation
```bash
kubectl get pods
kubectl describe pod node-app-74c7c4665b-sxdnx 
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab10/Screenshots/Taint%20the%20Worker%20Node.png?raw=true)
### Step 4: Monitor Pod Resource Usage
```bash
kubectl top pod node-app-74c7c4665b-sxdnx 
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab10/Screenshots/Taint%20the%20Worker%20Node.png?raw=true)


