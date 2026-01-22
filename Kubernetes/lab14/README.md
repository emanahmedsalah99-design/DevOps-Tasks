# Lab 14: StatefulSet with Headless Service (MySQL)

## Title

StatefulSet with Headless Service using MySQL

## Objective

The objective of this lab is to understand how to deploy a **stateful application (MySQL)** on Kubernetes using:
* StatefulSet
* Headless Service
* Secrets for sensitive data
* Persistent Volumes and Persistent Volume Claims
* Taints and Tolerations for node scheduling

By the end of this lab, MySQL will be running with persistent storage and accessible through a stable network identity.

## Environment

* Kubernetes cluster (Minikube)
* Docker
* MySQL 8 image
* OS: Linux

## Steps & Commands

### 1. Start Minikube with Multiple Nodes

```bash
minikube start --nodes=2 --driver=docker
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab14/Screenshots/Start%20Minikube%20with%20Multiple%20Nodes.png?raw=true)

Verify nodes:

```bash
kubectl get nodes
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab14/Screenshots/Verify%20nodes.png?raw=true)

---

### 2. Taint Worker Node

Apply a taint to isolate one node:

```bash
kubectl taint nodes minikube-m02 node=worker:NoSchedule
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab14/Screenshots/Taint%20Worker%20Node.png?raw=true)
Verify taint:

```bash
kubectl describe node minikube-m02 | grep -i taints
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab14/Screenshots/verify%20taint.png?raw=true)

---

### 3. Create Secret for MySQL Root Password

```bash
kubectl create secret generic mysql-secret \
  --from-literal=MYSQL_ROOT_PASSWORD=myrootpassword
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab14/Screenshots/Create%20secert.png?raw=true)

---

### 4. Create Persistent Volume (PV)

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data/mysql
  persistentVolumeReclaimPolicy: Retain
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab14/Screenshots/pv.yaml.png?raw=true)

Create directory on node:

```bash
minikube ssh
sudo mkdir -p /mnt/data/mysql
sudo chmod 777 /mnt/data/mysql
exit
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab14/Screenshots/node%20permission.png?raw=true)

---

### 5. Create Persistent Volume Claim (PVC)

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab14/Screenshots/pvc.yaml.png?raw=true)

---

### 6. Create Headless Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  clusterIP: None
  selector:
    app: mysql
  ports:
    - port: 3306
      targetPort: 3306
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab14/Screenshots/Headless%20Service.png?raw=true)

---

### 7. Create MySQL StatefulSet

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: "mysql"
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      tolerations:
        - key: "node"
          operator: "Equal"
          value: "worker"
          effect: "NoSchedule"
      containers:
        - name: mysql
          image: mysql:8
          ports:
            - containerPort: 3306
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-secret
                  key: MYSQL_ROOT_PASSWORD
          volumeMounts:
            - name: mysql-storage
              mountPath: /var/lib/mysql
  volumeClaimTemplates:
    - metadata:
        name: mysql-storage
      spec:
        accessModes:
          - ReadWriteOnce
        resources:
          requests:
            storage: 1Gi
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab14/Screenshots/StatefulSet.png?raw=true)


---

### 8. Apply All Manifests

```bash
kubectl apply -f PV.yaml
kubectl apply -f pvc.yaml
kubectl apply -f service.yaml
kubectl apply -f statefulset.yaml
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab14/Screenshots/Create%20secert.png?raw=true)

---

### 9. Verify Resources

```bash
kubectl get pods -l app=mysql
kubectl get pvc
kubectl get pv
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab14/Screenshots/Create%20secert.png?raw=true)

---

### 10. Connect to MySQL

```bash
kubectl exec -it mysql-0 -- mysql -uroot -p
```

Expected output:

```
mysql>
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab14/Screenshots/Create%20secert.png?raw=true)

---

## Conclusion

This lab demonstrates how Kubernetes StatefulSets manage stateful workloads using persistent storage, stable network identities, and controlled scheduling. MySQL is successfully deployed with persistent data, secure credentials, and node isolation.

---

✅ Lab Completed Successfully

