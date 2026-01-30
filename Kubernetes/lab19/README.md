# Lab 19: Node-Wide Pod Management with DaemonSet

## Objective
- Create a dedicated namespace for monitoring.
- Deploy a DaemonSet for Prometheus Node Exporter on all nodes.
- Ensure the DaemonSet tolerates all existing taints on nodes.
- Verify that each node has a Pod running and that metrics are exposed on port 9100.

## Environment
- OS: Linux 
- Kubernetes: Minikube

## Steps & Commands

### 1. Create Monitoring Namespace
```bash
kubectl create namespace monitoring
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab19/Screenshots/Namespace%20monitoring.png?raw=true)
### 2. Create Node Exporter DaemonSet
Create a file named `node-exporter-daemonset.yaml`:
```bash
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: node-exporter
  namespace: monitoring
  labels:
    app: node-exporter
spec:
  selector:
    matchLabels:
      app: node-exporter
  template:
    metadata:
      labels:
        app: node-exporter
    spec:
      tolerations:
      - operator: "Exists"  # Tolerate all existing taints
      containers:
      - name: node-exporter
        image: prom/node-exporter:latest
        ports:
        - containerPort: 9100
          name: metrics
      hostNetwork: true
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab19/Screenshots/node-exporter-daemonset.yaml.png?raw=true)

### 3. Apply the DaemonSet
```bash
kubectl apply -f node-exporter-daemonset.yaml

```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab19/Screenshots/Apply%20DaemonSet.png?raw=true)

### 4. Verify Pods on Each Node
```bash
kubectl get pods -n monitoring -o wide
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab19/Screenshots/Verify%20pod.png?raw=true)

### 5. Verify Metrics Exposure
```bash
curl http://192.168.49.2:9100/metrics
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab19/Screenshots/Verify%20Metrics%20Exposure.png?raw=true)


