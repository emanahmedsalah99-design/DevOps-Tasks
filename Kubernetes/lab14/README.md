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
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/build-tools/lab1/Screenshot/install%20java.png?raw=true)

Verify nodes:

```bash
kubectl get nodes
```

![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/build-
tools/lab1/Screenshot/install%20java.png?raw=true)

---

### 2. Taint Worker Node

Apply a taint to isolate one node:

```bash
kubectl taint nodes minikube-m02 node=worker:NoSchedule
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/build-tools/lab1/Screenshot/install%20java.png?raw=true)
Verify taint:

```bash
kubectl describe node minikube-m02 | grep -i taints
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/build-tools/lab1/Screenshot/install%20java.png?raw=true)

---

### 3. Create Secret for MySQL Root Password

```bash
kubectl create secret generic mysql-secret \
  --from-literal=MYSQL_ROOT_PASSWORD=myrootpassword
```

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

Create directory on node:

```bash
minikube ssh
sudo mkdir -p /mnt/data/mysql
sudo chmod 777 /mnt/data/mysql
exit
```

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

---

### 8. Apply All Manifests

```bash
kubectl apply -f mysql-pv.yaml
kubectl apply -f mysql-pvc.yaml
kubectl apply -f mysql-service.yaml
kubectl apply -f mysql-statefulset.yaml
```

---

### 9. Verify Resources

```bash
kubectl get pods -l app=mysql
kubectl get pvc
kubectl get pv
```

---

### 10. Connect to MySQL

```bash
kubectl exec -it mysql-0 -- mysql -uroot -p
```

Expected output:

```
mysql>
```

---

## Conclusion

This lab demonstrates how Kubernetes StatefulSets manage stateful workloads using persistent storage, stable network identities, and controlled scheduling. MySQL is successfully deployed with persistent data, secure credentials, and node isolation.

---

✅ Lab Completed Successfully

