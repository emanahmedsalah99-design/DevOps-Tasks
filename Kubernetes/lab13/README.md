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



