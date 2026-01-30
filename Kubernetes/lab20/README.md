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

### Step 2: Create ServiceAccount
```bash
kubectl create serviceaccount jenkins-sa -n ivolve
kubectl get serviceaccount jenkins-sa -n ivolve
```

### Step 3: Create Secret for ServiceAccount Token
Create file jenkins-sa-secret.yaml:
```bash

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: jenkins-sa-token
  namespace: ivolve
  annotations:
    kubernetes.io/service-account.name: jenkins-sa
type: kubernetes.io/service-account-token
```
### Step 4: Create Role (pod-reader)
Create file pod-reader-role.yaml
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

