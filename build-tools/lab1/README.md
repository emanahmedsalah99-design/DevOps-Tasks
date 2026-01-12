# Lab 1 – Building and Packaging Java Applications with Gradle

## Objective
Build and package a Java application using Gradle, run unit tests, generate JAR file, and verify the application is working.

## What is Gradle?
Gradle is a build tool used to automate compiling, testing, and packaging Java applications.

## Why Java?
Gradle and the application are Java-based, so Java is required to build and run the project.

## Environment
- OS: Red Hat Enterprise Linux 10
- Java: OpenJDK 21
- Gradle

## Steps & Commands

### Step 1: Install Java
```bash
sudo dnf install java-21-openjdk-devel -y
java -version
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/build-tools/lab1/install%20java.png?raw=true)

### Step 2: Install Gradle
```bash
sudo dnf install gradle -y
gradle -v
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/build-tools/lab1/install%20gradle.png?raw=true)
### Step 3: Clone Source Code
```bash
git clone https://github.com/Ibrahim-Adel15/build1.git
cd build1
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/build-tools/lab1/install%20java.png?raw=true)
### Step 4: Run Unit Tests
```bash
gradle test
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/build-tools/lab1/install%20java.png?raw=true)
### Step 5: Build Application
```bash
gradle build
ls build/libs
```
### Step 6: Run Application
```bash
java -jar build/libs/ivolve-app.jar
```









