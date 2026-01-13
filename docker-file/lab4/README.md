# Lab 4: Run Java Spring Boot App in a Container

## Objective
Run a Java Spring Boot application inside a Docker container using Java 17.

## Environment
- Host OS: Red Hat Enterprise Linux 10
- Host Java: 21
- Container Java: 17
- Maven
- Docker

## Steps & Commands

### Step 1: Clone Application Code
```bash
git clone https://github.com/Ibrahim-Adel15/Docker-1.git
cd Docker-1
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab4/Screenshots/clone%20the%20app.png?raw=true)
### Step 2: Build the Application
```bash
mvn clean package
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab4/Screenshots/Build%20The%20App.png?raw=true)
### Step 3: Dockerfile
```bash
vim dockerfile

FROM eclipse-temurin:17-jdk
WORKDIR /lab4
COPY target/demo-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
CMD ["java","-jar","app.jar"]

```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab4/Screenshots/Docker%20File.png?raw=true)
### Step 4: Build Docker Image
```bash
docker build -t app2 .
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab4/Screenshots/Build%20Image.png?raw=true)
### Step 5: Check image size
```bash
docker images
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab4/Screenshots/Image%20size.png?raw=true)
### Step 6: Run the Container
```bash
docker run -d -p 8089:8080 --name container2 app2
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab4/Screenshots/Run%20Container.png?raw=true)
### Step 7: Test the Application
```bash
curl http://localhost:8089
http://localhost:8089

```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab4/Screenshots/Test%20App.png?raw=true)
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab4/Screenshots/T%20APP.png?raw=true)
### Step 8: Stop and Delete Container
```bash
docker stop container2
docker rm container2
ps -a 
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab3/Screenshot/Clone%20Application%20Code.png?raw=true)


