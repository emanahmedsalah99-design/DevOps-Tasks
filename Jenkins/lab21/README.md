# Lab 21: Role-Based Authorization (RBAC)

## Objective
- Create two users: `user1` and `user2`
- Assign:
  - `admin` role to `user1`
  - `read-only` role to `user2`

## Environment
- Running Kubernetes cluster
- `kubectl` configured
- Access to cluster as admin

## Steps & Commands

### Step 1: Create Private Keys for Users
```bash
openssl genrsa -out user1.key 2048
openssl genrsa -out user2.key 2048
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Jenkins/lab21/Screenshots/Create%20Private%20Keys%20for%20Users.png?raw=true)
### Step 2: Create Certificate Signing Requests (CSR)
```bash
openssl req -new -key user1.key -out user1.csr -subj "/CN=user1"
openssl req -new -key user2.key -out user2.csr -subj "/CN=user2"
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Jenkins/lab21/Screenshots/Create%20Certificate%20Signing%20Requests.png?raw=true)
### Step 3: Sign Certificates Using Kubernetes CA
```bash
openssl x509 -req -in user1.csr \
-CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out user1.crt -days 365

openssl x509 -req -in user2.csr \
-CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out user2.crt -days 365
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Jenkins/lab21/Screenshots/Sign%20Certificates%20Using%20Kubernetes.png?raw=true)

### Step 4: Create kubeconfig Entries for Users
```bash
# user1
kubectl config set-credentials user1 --client-certificate=user1.crt --client-key=user1.key

# user2
kubectl config set-credentials user2 --client-certificate=user2.crt --client-key=user2.key
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Jenkins/lab21/Screenshots/Create%20kubeconfig%20Entries%20for%20Users.png?raw=true)

### Step 5: Create Contexts
```bash
kubectl config set-context user1-context --cluster=kubernetes --user=user1
kubectl config set-context user2-context --cluster=kubernetes --user=user2
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Jenkins/lab21/Screenshots/Create%20Contexts.png?raw=true)

### Step 6: Assign Admin Role to user1
```bash
kubectl create clusterrolebinding user1-admin-binding --clusterrole=cluster-admin --user=user1

```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Jenkins/lab21/Screenshots/Assign%20Admin%20Role%20to%20user1.png?raw=true)

### Step 7: Create Read-Only Role for user2
Create file name`read-only-role.yaml`
```bash
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: read-only
  namespace: default
rules:
- apiGroups: [""]
  resources: ["pods", "services", "configmaps"]
  verbs: ["get", "list", "watch"]
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Jenkins/lab21/Screenshots/Create%20Read-Only%20Role%20for%20user2.png?raw=true)
Apply the role
```bash
kubectl apply -f read-only-role.yaml
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Jenkins/lab21/Screenshots/Apply%20it.png?raw=true)
### Step 8: Bind Read-Only Role to user2
Create file name`user2-rolebinding.yaml`
```bash
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: user2-read-only-binding
  namespace: default
subjects:
- kind: User
  name: user2
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: read-only
  apiGroup: rbac.authorization.k8s.io

```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Jenkins/lab21/Screenshots/Bind%20Read-Only%20Role%20to%20user2.png?raw=true)
Apply the role binding:
```bash
kubectl apply -f user2-rolebinding.yaml
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Jenkins/lab21/Screenshots/Apply%20Read-Only%20Role.png?raw=true)

### Step 9: Validate Permissions
```bash
kubectl config use-context user1-context
kubectl get pods
kubectl delete pod <pod-name>  # ✅ Should work (admin access)

```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Jenkins/lab21/Screenshots/Validate%20Permissions.png?raw=true)
```bash
kubectl config use-context user2-context
kubectl get pods
kubectl delete pod <pod-name>  # ❌ Should fail (read-only access)
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Jenkins/lab21/Screenshots/Switch%20to%20user2%20(Read-Only).png?raw=true)




