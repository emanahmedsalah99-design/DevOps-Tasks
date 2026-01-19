# Lab 10: Node Isolation Using Taints in Kubernetes

## Objective
Learn how to isolate Kubernetes nodes using taints and understand the `NoSchedule` effect.



## Environment
- Kubernetes (Minikube)
- kubectl
- Minikube cluster with 2 nodes



## Lab Requirements
- Run Kubernetes cluster with 2 nodes
- Taint one node with:
  - key: node
  - value: worker
  - effect: NoSchedule
- Verify taint using `kubectl describe`



## Steps & commands

### Step 1: Start Minikube with 2 Nodes
```bash
minikube start --nodes 2
kubectl get nodes
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/build-tools/lab1/Screenshot/install%20java.png?raw=true)
### Step 2: Apply Taint to Worker Node
```bash
kubectl taint nodes minikube-m02 node=worker:NoSchedule
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab10/Screenshots/Taint%20the%20Worker%20Node.png?raw=true)

### Step 3: Verify Taint
```bash
kubectl describe node minikube-m02
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab10/Screenshots/Describe%20Nodes%20(Verify%20Taint).png?raw=true)
## Conclusion
- This lab demonstrates how to use Kubernetes taints to control pod placement and isolate nodes effectively.







