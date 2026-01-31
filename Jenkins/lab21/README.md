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

