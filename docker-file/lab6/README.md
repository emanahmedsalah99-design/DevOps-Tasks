# Lab 6 – Managing Docker Environment Variables Across Build and Runtime


## Objective
Understand how to manage environment variables in Docker containers using different approaches and run the same Flask application across multiple environments.

## Environment
- RHEL 10
- Python 3
- Flask
- Docker

## Steps & Commands

### Step 1: Clone Application Code
```bash
git clone https://github.com/Ibrahim-Adel15/Docker-3.git
cd Docker-3
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab6/Screenshots/Clone%20Application%20Code.png?raw=true)
### Step 2: Dockerfile 
```bash
vim dockerfile
FROM python:3.10-slim
WORKDIR /app
COPY . .
RUN pip install flask
EXPOSE 8080
CMD ["python", "app.py"]
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab6/Screenshots/Dockerfile1.png?raw=true)
### Step 3: Build Docker Image
```bash
docker build -t python-image .
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab6/Screenshots/Build%20Image%20(python-image).png?raw=true)
### Step 4: Run Containers with Environment Variables
```bash
docker run -d --name container -p 8088:8080 -e APP_MODE=development -e APP_REGION=us-east python-image
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab6/Screenshots/Run%20Containers%20with%20Environment%20Variables.png?raw=true)
### Step 5: Test the Container
```bash
curl http://localhost:8088
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab6/Screenshots/Test.png?raw=true)
### Step 6: Environment File Variables (Staging)
```bash
vim env
APP_MODE=staging
APP_REGION=us-west
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab6/Screenshots/Environment%20File%20Variables.png?raw=true)
### Step 7: Run the container using the env file:
```bash
docker run -d --name container -p 8081:8080 --env-file env python-image
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab5/Screenshots/Stop%20&%20Delete%20Container.png?raw=true)
### Step 8: Test the Container
```bash
curl http://localhost:8081
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab5/Screenshots/Run%20Container.png?raw=true)
### Step 9: Dockerfile Variables (Production)
```bash
vim Dockerfile.prod
FROM python:3.10-alpine
WORKDIR /app
COPY . .
RUN pip install flask
ENV APP_MODE=production
ENV APP_REGION=canada-west
EXPOSE 8080
CMD ["python", "app.py"]
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab5/Screenshots/Run%20Container.png?raw=true)
### Step 10: Build the production container
```bash
docker build -t python-image2 -f Dockerfile.prod .
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab5/Screenshots/Run%20Container.png?raw=true)
### Step 11: run the production container
```bash
docker run -d --name container3 -p 8082:8080 python-image2
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab5/Screenshots/Run%20Container.png?raw=true)
### Step 12: Test the Container
```bash
curl http://localhost:8082
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab5/Screenshots/Run%20Container.png?raw=true)



