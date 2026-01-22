
### Lab 16: Kubernetes Init Container for Pre-Deployment Database Setup

## Objective
Learn how to use Init Containers in Kubernetes to prepare the database before deploying the main application.
Deploy a Node.js application connected to MySQL using Kubernetes resources.
Ensure proper sequencing: MySQL Pod ready → Init Container creates database & user → Node.js app starts.

## Environment
Kubernetes cluster (Minikube)
DockerHub image: emma175/kubernets-app:1.0

## Steps & Commands
