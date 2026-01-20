# Lab 11: Namespace Management and Resource Quota Enforcement

## Objective
- Create a Kubernetes namespace called `ivolve`.
- Apply a ResourceQuota to limit the number of pods to only **2 pods** within the namespace.

## Steps & commands

## Step 1: Generate Namespace YAML (Dry-Run)

```bash
kubectl create ns ivolve --dry-run=client -o yaml > ivolve.yaml
kubectl apply -f ivolve.yaml
kubectl get ns 
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab11/Screenshots/Create%20NS%20from%20YAML.png?raw=true)
## Step 2: Generate ResourceQuota YAML

```bash
kubectl create quota pod-quota --hard=pods=2 -n ivolve --dry-run=client -o yaml > ivolve-quota.yaml
kubectl apply -f ivolve-quota.yaml

```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab11/Screenshots/Create%20ResourceQuata.png?raw=true)

## Step 3: Test ResourceQuota
```bash
kubectl run pod1 --image=nginx -n ivolve
kubectl run pod2 --image=nginx -n ivolve
kubectl run pod3 --image=nginx -n ivolve
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab11/Screenshots/Test%20the%20quota.png?raw=true)
## Conclusion
- Namespace ivolve was created using YAML.
- ResourceQuota successfully limits pods to only 2 inside the namespace.
- The third pod will fail due to exceeding the ResourceQuota limit.


