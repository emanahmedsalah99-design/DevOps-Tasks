# Lab 2 – Building and Packaging Java Applications with Maven

## Objective
Build and package a Java application using Maven, run unit tests, generate JAR file, and verify the application is working.

## Environment
- OS: Red Hat Enterprise Linux 10
- Java: OpenJDK 21
- Maven

## Steps & Commands

### Step 1: Install Maven
```bash
sudo dnf install maven -y
mvn -version
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Screen%20lab2/maven-version.png.png?raw=true)
### Step 2: Clone Source Code
```bash
git clone https://github.com/Ibrahim-Adel15/build2.git
cd build2
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Screen%20lab2/clone_repo.png.png?raw=true)
### Step 3: Run Unit Tests
```bash
mvn test
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Screen%20lab2/tests-success.png.png?raw=true)
### Step 4: Build Application
```bash
mvn package
ls target/
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Screen%20lab2/jar-file.png.png?raw=true)
### Step 5: Run Application
```bash
java -jar target/hello-ivolve-1.0-SNAPSHOT.jar
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Screen%20lab2/app-running.png.png?raw=true)





