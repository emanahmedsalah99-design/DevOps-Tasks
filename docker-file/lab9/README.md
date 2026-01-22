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
### Step 2: Docker Compose Setup
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
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab9/Screenshots/docker-compose.yml.png?raw=true)
### Step 3: Run the Stack
```bash
docker compose up -d --build
docker ps

```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab9/Screenshots/Docker%20Compose.png?raw=true)
### Step 4: Verify the Application
```bash
curl http://localhost:3000/health
curl http://localhost:3000/ready
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab9/Screenshots/health,%20ready.png?raw=true)
### Step 5: Logs
```bash
docker exec -it node_app sh
ls /app/logs
cat /app/logs/access.log
exit
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab9/Screenshots/logs.png?raw=true)
### Step 6: Run Container from Image
```bash
docker login
docker build -t emma175/kubernets-app:1.0 .
docker push emma175/kubernets-app:1.0
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab9/Screenshots/DockerHub.png?raw=true)
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab9/Screenshots/DockerHub2.png?raw=true)
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab9/Screenshots/DockerHub3.png?raw=true)



