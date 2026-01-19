# Lab 10: Node Isolation Using Taints in Kubernetes

## Objective
Learn how to isolate Kubernetes nodes using taints and understand the `NoSchedule` effect.

---

## Environment
- Kubernetes (Minikube)
- kubectl
- Minikube cluster with 2 nodes

---

## Lab Requirements
- Run Kubernetes cluster with 2 nodes
- Taint one node with:
  - key: node
  - value: worker
  - effect: NoSchedule
- Verify taint using `kubectl describe`

---

## Steps & commands

### Step 1: Start Minikube with 2 Nodes
```bash
minikube start --nodes 2
kubectl get nodes
```


