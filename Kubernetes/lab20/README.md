# Lab 20: Securing Kubernetes with RBAC and Service Accounts

## Objective
Learn how to secure Kubernetes access using RBAC (Role-Based Access Control) and Service Accounts by granting a ServiceAccount read-only access to Pods in a specific namespace.

## Environment
- Kubernetes Cluster
- kubectl
- Namespace: ivolve

##  Steps & Commands

### Step 1: Create Namespace
```bash
kubectl create namespace ivolve
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab20/Screenshots/Create%20Namespace.png?raw=true)
### Step 2: Create ServiceAccount
```bash
kubectl create serviceaccount jenkins-sa -n ivolve
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab20/Screenshots/Create%20ServiceAccount.png?raw=true)
### Step 3: Verify ServiceAccount
```bash
kubectl get serviceaccount jenkins-sa -n ivolve
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab20/Screenshots/Verify%20ServiceAccount.png?raw=true)
### Step 4: Create Secret for ServiceAccount Token
Create file `jenkins-sa-secret.yaml`:
```bash
apiVersion: v1
kind: Secret
metadata:
  name: jenkins-sa-token
  namespace: ivolve
  annotations:
    kubernetes.io/service-account.name: jenkins-sa
type: kubernetes.io/service-account-token
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab20/Screenshots/Create%20a%20Secret%20for%20the%20ServiceAccount%20Token.png?raw=true)
Apply the Secret
```bash
kubectl apply -f jenkins-sa-secret.yaml
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab20/Screenshots/Apply%20jenkins-sa-secret.png?raw=true)
Retrieve the token
```bash
kubectl get secret jenkins-sa-token -n ivolve -o jsonpath='{.data.token}' | base64 --decode
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab20/Screenshots/Retrieve%20the%20token.png?raw=true)
### Step 5: Create Role (pod-reader)
Create file `pod-reader-role.yaml`
```bash
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: ivolve
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list"]
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab20/Screenshots/Create%20Role%20(pod-reader).png?raw=true)
Apply the Role:
```bash
kubectl apply -f pod-reader-role.yaml
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab20/Screenshots/Apply%20pod-reader-binding.yaml.png?raw=true)
### Step 6: Create RoleBinding
Create file `pod-reader-binding.yaml`
```bash
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-reader-binding
  namespace: ivolve
subjects:
- kind: ServiceAccount
  name: jenkins-sa
  namespace: ivolve
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab20/Screenshots/Create%20RoleBinding.png?raw=true)

Apply the RoleBinding
```bash
kubectl apply -f pod-reader-binding.yaml
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab20/Screenshots/Apply%20the%20Role.png?raw=true)
### Step 7: Validate Permissions
Check if ServiceAccount can list Pods
```bash
kubectl auth can-i list pods --as=system:serviceaccount:ivolve:jenkins-sa -n ivolve
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab20/Screenshots/Check%20if%20ServiceAccount%20can%20list%20Pods.png?raw=true)
Check forbidden action (example: delete pod)
```bash
kubectl auth can-i delete pods --as=system:serviceaccount:ivolve:jenkins-sa -n ivolve
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab20/Screenshots/Check%20forbidden%20action%20(example%20delete%20pod).png?raw=true)

## Conclusion
- The jenkins-sa ServiceAccount has read-only access to Pods in the ivolve namespace.
- RBAC ensures the ServiceAccount cannot perform unauthorized actions such as deleting Pods.
- This setup is commonly used for CI/CD tools like Jenkins to safely interact with Kubernetes.



