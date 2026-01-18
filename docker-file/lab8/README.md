# Lab 8: Custom Docker Network for Microservices


## Objective
Understand how to create a custom Docker network and run Microservices (Frontend & Backend) on it, and verify communication between containers.

## Environment
- RHEL 10
- Docker

## Steps & Commands

### Step 1: Clone the App
```bash
git clone https://github.com/Ibrahim-Adel15/Docker5.git
cd Docker5
ls
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab8/Screenshots/Clone%20the%20App.png?raw=true)
### Step 2: Frontend Dockerfile
```bash
cd Frontend
vim Dockerfile

FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]

```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab8/Screenshots/Frontend%20Dockerfile.png?raw=true)
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab8/Screenshots/frontend%20Dockerfile%20(2).png?raw=true)
### Step 3: Build Frontend Image
```bash
docker build -t frontend-image .
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab8/Screenshots/Build%20Frontend%20Image.png?raw=true)
### Step 5: Backend Dockerfile
```bash
cd ../backend
vim Dockerfile

FROM python:3.11-slim
WORKDIR /app
RUN pip install flask
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]

```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab8/Screenshots/Backend%20Dockerfile.png?raw=true)
### Step 6: Build Backend Image
```bash
docker build -t backend-image .
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab8/Screenshots/Build%20Backend%20Image.png?raw=true)
### Step 7: Create Custom Docker Network
```bash
docker network create ivolve-network
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab8/Screenshots/Create%20Custom%20Docker%20Network.png?raw=true)
### Step 8: Run Backend Container on Custom Network
```bash
docker run -d --name backend --network ivolve-network backend-image
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab8/Screenshots/Run%20Backend%20Container%20on%20Custom%20Network.png?raw=true)
### Step 8: Run Frontend Container (frontend1) on Custom Network
```bash
docker run -d --name frontend1 --network ivolve-network -p 5000:5000 frontend-image
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab8/Screenshots/Run%20Frontend%20Container%20(frontend1)%20on%20Custom%20Network.png?raw=true)
### Step 9: Run Another Frontend Container (frontend2) on Default Network
```bash
docker run -d--name frontend2 -p 5001:5000 frontend-image
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab8/Screenshots/Run%20Another%20Frontend%20Container%20(frontend2)%20on%20Default%20Network.png?raw=true)
### Step 7: Verify Communication Between Containers
```bash
http://localhost:5000
http://localhost:5001
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab8/Screenshots/Verify%20Communication%20Between%20Containers.png?raw=true)

![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab8/Screenshots/Verify%20Communication%20Between%20Containers%20(2).png?raw=true)
### Step 7: Delete Custom Network (Cleanup)
```bash
docker stop frontend1 backend
docker rm frontend1 backend
docker network rm ivolve-network
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab8/Screenshots/Delete%20Custom%20Network.png?raw=true)
## Conclusion
- Containers connected to the same Docker network can communicate using service names, while containers on different     networks remain isolated
