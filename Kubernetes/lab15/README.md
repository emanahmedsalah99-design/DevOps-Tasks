## Title

Deploy Node.js Application with MySQL on Kubernetes (Minikube)

## Objective

Deploy a containerized Node.js application connected to a MySQL database on a Kubernetes cluster using Minikube. The application image is pulled from DockerHub and exposed using a NodePort service.

## Environment

* OS: Linux 
* Kubernetes: Minikube
* Docker
* DockerHub account

## Steps & Commands

### 1. Create MySQL Deployment & MySQL Service

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
              value: root123
            - name: MYSQL_DATABASE
              value: ivolve
          ports:
            - containerPort: 3306
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
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab15/Screenshots/mysql-deployment.yaml.png?raw=true)
### 2. Deploy Node.js Application (from DockerHub)

Create a file named `app-deployment.yaml`:
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
      containers:
        - name: node-app
          image: emma175/kubernets-app:1.0
          imagePullPolicy: Always
          ports:
            - containerPort: 3000
          env:
            - name: DB_HOST
              value: mysql
            - name: DB_USER
              value: root
            - name: DB_PASSWORD
              value: root123
---
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
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab15/Screenshots/app-deployment.yaml.png?raw=true)
### 3. Apply Resources to Kubernetes & Verify Deployment

```bash
kubectl apply -f mysql-deployment.yaml
kubectl apply -f app-deployment.yaml
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab15/Screenshots/apply.png?raw=true)
### 4. Verify Deployment
```
kubectl get pods
kubectl get svc
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab15/Screenshots/verify.png?raw=true)

### 5. Access the Application

```bash
minikube service node-app-service
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Kubernetes/lab15/Screenshots/Access%20the%20Application.png?raw=true)

