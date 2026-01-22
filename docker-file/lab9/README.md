# Lab 9: Containerized Node.js and MySQL Stack Using Docker Compose

## Objective
- Deploy a Node.js application with a MySQL database using Docker Compose.
- Ensure the application connects to a database named `ivolve`.
- Verify application health, readiness, and access logs.
- Push the application Docker image to DockerHub.

## Environment
- Docker
- Docker Compose
- Node.js application source code: [GitHub Repository](https://github.com/Ibrahim-Adel15/kubernets-app.git)

## Steps & Commands

### 1. Clone the application source code
```bash
git clone https://github.com/Ibrahim-Adel15/kubernets-app.git
cd kubernets-app
ls
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab9/Screenshots/clone%20app.png?raw=true)
### Step 2: Run Container from Image
```bash
vim docker-compose.yml
version: "3.8"

services:
  db:
    image: mysql:8.0
    container_name: mysql_db
    environment:
      MYSQL_ROOT_PASSWORD: root123
      MYSQL_DATABASE: ivolve
    volumes:
      - db_data:/var/lib/mysql
    ports:
      - "3306:3306"

  app:
    build: .
    container_name: node_app
    ports:
      - "3000:3000"
    environment:
      DB_HOST: db
      DB_USER: root
      DB_PASSWORD: root123
    depends_on:
      - db

volumes:
  db_data:
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab3/Screenshot/Run%20Container%20from%20Image.png?raw=true)
### Step 3: Run Container from Image
```bash
curl http://localhost:3000/health
curl http://localhost:3000/ready
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab3/Screenshot/Run%20Container%20from%20Image.png?raw=true)
### Step 4: Run Container from Image
```bash
docker run -d -p 8080:8080 --name container1 app1
docker ps
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab3/Screenshot/Run%20Container%20from%20Image.png?raw=true)
### Step 5: Run Container from Image
```bash
docker login
docker build -t emma175/kubernets-app:1.0 .
docker push emma175/kubernets-app:1.0
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab3/Screenshot/Run%20Container%20from%20Image.png?raw=true)


