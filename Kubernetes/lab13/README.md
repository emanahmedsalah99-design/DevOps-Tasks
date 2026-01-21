# Lab 13 – Persistent Storage Setup for Application Logging

## Objective
Configure persistent storage for application logs using Kubernetes PersistentVolume (PV) and PersistentVolumeClaim (PVC) on a Minikube cluster.

## Environment
- Minikube
- kubectl
- Linux 
- YAML files for PV & PVC

## Steps & Commands

### Step 1: Prepare storage directory
```bash
sudo mkdir -p /mnt/app-logs
sudo chmod 777 /mnt/app-logs
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab13/Screenshots/Create%20directory%20on%20node.png?raw=true)
### Step 2: Create PV manifest
```bash
vim pv.yaml

apiVersion: v1
kind: PersistentVolume
metadata:
  name: app-logs-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  storageClassName: ""
  hostPath:
    path: /mnt/app-logs
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab13/Screenshots/pv.yaml.png?raw=true)

### Step 3: Create PVC manifest
```bash
vim pvc.yaml

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: app-logs-pvc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
  volumeName: app-logs-pv
  storageClassName: ""
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab13/Screenshots/pvc.yaml.png?raw=true)

### Step 4: Apply PV and PVC
```bash
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab13/Screenshots/Apply%20PV%20and%20PVC.png?raw=true)
### Step 5: Verify PV and PVC
```bash
kubectl get pv
kubectl get pvc
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab13/Screenshots/Verify%20PV%20and%20PVC.png?raw=true)




