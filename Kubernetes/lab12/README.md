# Lab 12 – Managing Configurations and Secrets using kubectl CLI

## Objective
Store MySQL non-sensitive data in a ConfigMap and sensitive data in a Secret, then inject them into a Pod using environment variables.

## Steps & Commands

### Step 1: ConfigMap
```bash
kubectl create cm ivolve -n ivolve  --from-literal=DB_HOST=mysql-service  --from-literal=DB_USER=ivolve_user --dry-run=client -o yaml
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab12/Screenshots/ConfigMap.png?raw=true)
### Step 2: Secret
```bash
kubectl create secret generic mysql-secret -n ivolve --from-literal=DB_PASSWORD=mypassword123   --from-literal=MYSQL_ROOT_PASSWORD=rootpassword456 --dry-run=client -o yaml 
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab12/Screenshots/Secret.png?raw=true)
## Conclusion
ConfigMaps handle non-sensitive data, Secrets handle sensitive data, and both can be used in Pods to enhance security and manageability.

