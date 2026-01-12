# Lab 3 – Run Java Spring Boot App in a Container

## Objective
Run a Java Spring Boot application inside a Docker container.

## Environment
- Java 17
- Maven
- Docker

## Steps & Commands

### Step 1: Clone Application Code
```bash
git clone https://github.com/Ibrahim-Adel15/Docker-1.git
cd Docker-1
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab3/Screenshot/Clone%20Application%20Code.png?raw=true)
### Step 2: Dockerfile
```bash
vim Dockerfile

FROM maven:3.9.9-eclipse-temurin-17
WORKDIR /app
COPY . .
RUN mvn package -DskipTests
EXPOSE 8080
CMD ["java","-jar","target/demo-0.0.1-SNAPSHOT.jar"]
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab3/Screenshot/Dockerfile.png?raw=true)
### Step 3: Build Docker Image
```bash
docker build -t app1 .
docker images
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab3/Screenshot/Build%20Docker%20Image.png?raw=true)
### Step 4: Run Container from Image
```bash
docker run -d -p 8080:8080 --name container1 app1
docker ps
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab3/Screenshot/Run%20Container%20from%20Image.png?raw=true)
### Step 5: Test the Application
```bash
http://localhost:8080

```
![Repository Cloned]()
### Step 1: Clone Application Code
```bash
git clone https://github.com/Ibrahim-Adel15/Docker-1.git
cd Docker-1
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab3/Screenshot/Clone%20Application%20Code.png?raw=true)
### Step 1: Clone Application Code
```bash
git clone https://github.com/Ibrahim-Adel15/Docker-1.git
cd Docker-1
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab3/Screenshot/Clone%20Application%20Code.png?raw=true)
### Step 1: Clone Application Code
```bash
git clone https://github.com/Ibrahim-Adel15/Docker-1.git
cd Docker-1
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab3/Screenshot/Clone%20Application%20Code.png?raw=true)


