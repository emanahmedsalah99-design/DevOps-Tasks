
# Lab 5 – Multi-Stage Build for Java Spring Boot App

## Objective
Build and run a Java Spring Boot application using Docker Multi-Stage Build.

## Environment
- RHEL 10
- Java 21 (host)
- Java 17 (container)
- Maven
- Docker

## Steps & Commands

### Step 1: Clone Application Code
```bash
git clone https://github.com/Ibrahim-Adel15/Docker-1.git
cd Docker-1
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab5/Screenshots/Clone%20App.png?raw=true)
### Step 2: Dockerfile (Multi-Stage)
```bash
vim dockerfile
# -------- Stage 1: Build --------
FROM maven:3.9.9-eclipse-temurin-17 AS builder
WORKDIR /build
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests
# -------- Stage 2: Run --------
FROM eclipse-temurin:17-jdk
WORKDIR /app
COPY --from=builder /build/target/demo-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
CMD ["java","-jar","app.jar"]
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab5/Screenshots/Dockerfile.png?raw=true)
### Step 3: Build Docker Image
```bash
docker build -t app3 .
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab5/Screenshots/Build%20Image.png?raw=true)
### Step 4: Check image size
```bash
docker images
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab5/Screenshots/Image%20size.png?raw=true)
### Step 5: Run the Container
```bash
docker run -d -p 8090:8080 --name container3 app3
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab5/Screenshots/Run%20Container.png?raw=true)
### Step 6: Test the Application
```bash
curl http://localhost:8090
http://localhost:8090
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab5/Screenshots/Test.png?raw=true)
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab5/Screenshots/T%20app.png?raw=true)
### Step 7: Stop and Delete Container
```bash
docker stop container3
docker rm container3
docker ps -a 
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab5/Screenshots/Stop%20&%20Delete%20Container.png?raw=true)



