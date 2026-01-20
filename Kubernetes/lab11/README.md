# Lab 11: Namespace Management and Resource Quota Enforcement

## Objective
- Create a Kubernetes namespace called `ivolve`.
- Apply a ResourceQuota to limit the number of pods to only **2 pods** within the namespace.

---

## Step 1: Generate Namespace YAML (Dry-Run)

```bash
kubectl create namespace ivolve --dry-run=client -o yaml > ivolve-namespace.yaml

