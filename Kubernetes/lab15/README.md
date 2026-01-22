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

```yaml
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

```yaml
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


### 4. Deploy Node.js Application (from DockerHub)

Create a file named `app-deployment.yaml`:

```yaml
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
          image: emanahmedsalah99/kubernets-app:1.0
          ports:
            - containerPort: 3000
          env:
            - name: DB_HOST
              value: mysql
            - name: DB_USER
              value: root
            - name: DB_PASSWORD
              value: root123
```

### 5. Create Service for Node.js App

Create a file named `node-app-service.yaml`:

```yaml
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

### 6. Apply Resources to Kubernetes

```bash
kubectl apply -f mysql-deployment.yaml
kubectl apply -f mysql-service.yaml
kubectl apply -f app-deployment.yaml
kubectl apply -f node-app-service.yaml
```

### 7. Verify Deployment

```bash
kubectl get pods
kubectl get svc
```

### 8. Access the Application

```bash
minikube service node-app-service
```

If everything is configured correctly, the Node.js application will be accessible via the generated URL and connected to the MySQL database named `ivolve`.
