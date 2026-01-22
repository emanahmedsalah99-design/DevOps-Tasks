## Title
Kubernetes Init Container for Pre-Deployment Database Setup

## Objective
Learn how to use Init Containers in Kubernetes to prepare the database before deploying the main application.
Deploy a Node.js application connected to MySQL using Kubernetes resources.
Ensure proper sequencing: MySQL Pod ready → Init Container creates database & user → Node.js app starts.

## Environment
* OS: Linux 
* Kubernetes: Minikube
* DockerHub image

## Steps & Commands

### 1. Create Secret for Database Passwords

Create a file named `db-secret.yaml`:
```
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
stringData:
  MYSQL_ROOT_PASSWORD: root123
  DB_PASSWORD: ivolve123
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab16/Screenshots/db-secret.yaml.png?raw=true)
### 2. Create ConfigMap for Database Config

Create a file named `db-configmap.yaml`:
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: db-config
data:
  DB_HOST: mysql
  DB_NAME: ivolve
  DB_USER: ivolve_user
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab16/Screenshots/db-configmap.yaml.png?raw=true)
### 3. Deploy MySQL with Service

Create a file named `mysql-deployment.yaml`:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:8.0
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: db-secret
                  key: MYSQL_ROOT_PASSWORD
            - name: MYSQL_DATABASE
              valueFrom:
                configMapKeyRef:
                  name: db-config
                  key: DB_NAME
          ports:
            - containerPort: 3306
          readinessProbe:
            exec:
              command: ["mysqladmin", "ping", "-h", "127.0.0.1"]
            initialDelaySeconds: 10
            periodSeconds: 5
---
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  selector:
    app: mysql
  ports:
    - port: 3306
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab16/Screenshots/Deploy%20MySQL%20with%20Service.png?raw=true)
### 4. Deploy Node.js App with Init Container

Create a file named `app-deployment.yaml:`:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: node-app
  template:
    metadata:
      labels:
        app: node-app
    spec:
      initContainers:
        - name: init-db
          image: mysql:5.7
          command:
            - sh
            - -c
            - |
              until mysql -h $DB_HOST -u root -p$MYSQL_ROOT_PASSWORD -e "SELECT 1;" ; do
                echo "Waiting for MySQL..."
                sleep 3
              done
              mysql -h $DB_HOST -u root -p$MYSQL_ROOT_PASSWORD <<EOF
              CREATE DATABASE IF NOT EXISTS $DB_NAME;
              CREATE USER IF NOT EXISTS '$DB_USER'@'%' IDENTIFIED BY '$DB_PASSWORD';
              GRANT ALL PRIVILEGES ON $DB_NAME.* TO '$DB_USER'@'%';
              FLUSH PRIVILEGES;
              EOF
          env:
            - name: DB_HOST
              valueFrom:
                configMapKeyRef:
                  name: db-config
                  key: DB_HOST
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: db-secret
                  key: MYSQL_ROOT_PASSWORD
            - name: DB_NAME
              valueFrom:
                configMapKeyRef:
                  name: db-config
                  key: DB_NAME
            - name: DB_USER
              valueFrom:
                configMapKeyRef:
                  name: db-config
                  key: DB_USER
            - name: DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: db-secret
                  key: DB_PASSWORD

      containers:
        - name: node-app
          image: emma175/kubernets-app:1.0
          ports:
            - containerPort: 3000
          env:
            - name: DB_HOST
              valueFrom:
                configMapKeyRef:
                  name: db-config
                  key: DB_HOST
            - name: DB_USER
              valueFrom:
                configMapKeyRef:
                  name: db-config
                  key: DB_USER
            - name: DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: db-secret
                  key: DB_PASSWORD
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab16/Screenshots/Secret%20for%20Database%20Passwords.png?raw=true)
### 5. Create Service for Node.js App

Create a file named `node-app-service.yaml`:
```
apiVersion: v1
kind: Service
metadata:
  name: node-app-service
spec:
  type: NodePort
  selector:
    app: node-app
  ports:
    - port: 3000
      targetPort: 3000

```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab16/Screenshots/Service%20for%20Node.js%20App.png?raw=true)
### 6. Apply
```
kubectl apply -f db-secret.yaml
kubectl apply -f db-configmap.yaml
kubectl apply -f mysql-deployment.yaml
kubectl apply -f app-deployment.yaml
kubectl apply -f node-app-service.yaml

```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab16/Screenshots/Apply.png?raw=true)
### 7. Verify Deployment
```
kubectl get pods
kubectl get svc

```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab16/Screenshots/Verify%20Deployment.png?raw=true)
### 8. Access Node.js Application
```
minikube service node-app-service
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab16/Screenshots/Access%20Node.js%20Application.png?raw=true)

