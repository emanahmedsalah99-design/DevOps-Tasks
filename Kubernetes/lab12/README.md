# Lab 12 – Managing Configurations and Secrets using kubectl CLI

## Objective
Store MySQL non-sensitive data in a ConfigMap and sensitive data in a Secret, then inject them into a Pod using environment variables.

## Steps & Commands

1. Generate ConfigMap YAML:
```bash
kubectl create cm mysql-config -n ivolve \
  --from-literal=DB_HOST=mysql-service \
  --from-literal=DB_USER=ivolve_user \
  --dry-run=client -o yaml > configmap.yaml

