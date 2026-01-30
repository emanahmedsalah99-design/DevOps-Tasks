# Lab 19: Node-Wide Pod Management with DaemonSet

## Objective
- Create a dedicated namespace for monitoring.
- Deploy a DaemonSet for Prometheus Node Exporter on all nodes.
- Ensure the DaemonSet tolerates all existing taints on nodes.
- Verify that each node has a Pod running and that metrics are exposed on port 9100.

## Environment
- Kubernetes Cluster (Minikube or any other cluster)
- kubectl
- curl

## Steps & Commands

### 1. Create Monitoring Namespace
```bash
kubectl create namespace monitoring
```


