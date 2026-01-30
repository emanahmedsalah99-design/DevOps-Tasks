
# Lab 17: Pod Resource Management with CPU and Memory Requests and Limits

## Objective
- Add CPU and memory requests and limits to Node.js deployment.
- Verify resource settings and monitor usage.

## Environment
- Kubernetes cluster with Node.js app (`node-app`) and MySQL service.
- Metrics Server for monitoring.

## Steps & Commands

### 1. Verify current pods
```bash
kubectl get pods
```
