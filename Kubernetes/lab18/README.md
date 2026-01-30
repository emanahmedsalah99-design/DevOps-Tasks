
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
vim allow-app-to-mysql.yaml
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



