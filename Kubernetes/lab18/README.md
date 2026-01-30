
# Lab 18: Control Pod-to-Pod Traffic via Network Policy

## Objective
Learn how to control pod-to-pod communication in Kubernetes using NetworkPolicy and restrict access to MySQL pods to only application pods.

---

## Environment
- Kubernetes Cluster
- kubectl
- Network plugin that supports NetworkPolicy (e.g., Calico)
- Existing MySQL and application deployments

---
## Steps & Commands

### Step 1: Create NetworkPolicy
Create a file named `allow-app-to-mysql.yaml`:
```bash
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-app-to-mysql
spec:
  podSelector:
    matchLabels:
      app: mysql
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: node-app
      ports:
        - protocol: TCP
          port: 3306
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab18/Screenshot/NetworkPolicy%20YAML.png?raw=true)
### 2. Apply NetworkPolicy
```
kubectl apply -f allow-app-to-mysql.yaml
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab18/Screenshot/Apply%20NetworkPolicy.png?raw=true)
### 3. Verify NetworkPolicy
```
kubectl get networkpolicy
kubectl describe networkpolicy allow-app-to-mysql
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab18/Screenshot/verify%20NetworkPolicy.png?raw=true)
### 4. Test Allowed Pod
Create a pod with the allowed label:
```
kubectl run allowed-test --image=mysql:5.7 --restart=Never --labels="app=node-app" -- sleep 3600
kubectl exec -it allowed-test -- mysql -h mysql -uroot -p

```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab18/Screenshot/Test%20Allowed%20Pod.png?raw=true)
### 5. Test Denied Pod
Create a pod without the required label:
```
kubectl run denied-test --image=mysql:5.7 --restart=Never -- sleep 3600
kubectl exec -it denied-test -- mysql -h mysql -uroot -p
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab18/Screenshot/Test%20Denied%20Pod.png?raw=true)
## Conclusion
This lab demonstrates how Kubernetes NetworkPolicy can be used to restrict pod-to-pod traffic and allow only authorized application pods to access MySQL services.


