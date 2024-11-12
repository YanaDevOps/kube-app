# VProfile Kubernetes Project

## 🛠️ Technology Badges
![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.20-blue)
![Docker](https://img.shields.io/badge/Docker-latest-blue)
![MySQL](https://img.shields.io/badge/MySQL-5.7-orange)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-2.5.0-brightgreen)
![Memcached](https://img.shields.io/badge/Memcached-1.6-lightgrey)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-3.9.5-orange)
![Java](https://img.shields.io/badge/Java-11-blue)

## 📦 CI/CD and Tools
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-CI%2FCD-brightgreen)
![Ansible](https://img.shields.io/badge/Ansible-2.9-blue)
![Python](https://img.shields.io/badge/Python-3.8-green)

## ☁️ AWS Services Used
![AWS EC2](https://img.shields.io/badge/AWS-EC2-yellow)
![AWS S3](https://img.shields.io/badge/AWS-S3-yellow)
![AWS Route 53](https://img.shields.io/badge/AWS-Route%2053-yellowgreen)
![AWS EBS](https://img.shields.io/badge/AWS-EBS-blueviolet)

## 🗂️ Deployment Tools
![Kops](https://img.shields.io/badge/Kops-v1.19-blue)
![kubectl](https://img.shields.io/badge/Kubectl-v1.20-blue)
![Ingress](https://img.shields.io/badge/Ingress-Nginx-brightgreen)

## 📋 Project Overview
VProfile is a microservices-based web application deployed using Kubernetes. The application includes several components such as MySQL, Memcached, RabbitMQ, and a Spring Boot web application. The project uses Kubernetes for container orchestration and AWS for infrastructure management.

## 🛠️ Tech Stack
* Kubernetes (Kops, EKS)
* Docker
* MySQL (Database)
* RabbitMQ (Message Queue)
* Memcached (Caching)
* Spring Boot (Back-end)
* AWS EC2, S3, Route53, EBS (Cloud Infrastructure)

## 📁 Project Structure
```bash
kube-app/
├── app-secret.yaml        # Secrets for database and RabbitMQ
├── db-CIP.yaml            # Service for MySQL
├── mc-CIP.yaml            # Service for Memcached
├── rmq-CIP.yaml           # Service for RabbitMQ
├── vproapp-service.yaml   # Service for the web application
├── vproappdep.yaml        # Deployment for the Spring Boot application
├── vprodbdep.yaml         # Deployment for MySQL database
├── mcdep.yaml             # Deployment for Memcached
├── rmq-dep.yaml           # Deployment for RabbitMQ
└── vproingress.yaml       # Ingress configuration for accessing the application
```

### 🚀 Deployment Instructions
**Prerequisites**
Make sure you have the following tools installed:
- [Kubernetes CLI (kubectl)](https://kubernetes.io/docs/reference/kubectl/)
- [Kops](https://kops.sigs.k8s.io/)
- [AWS CLI](https://aws.amazon.com/cli/)
- [Docker](https://www.docker.com/)

### Infrastructure Setup
1. Create a Kubernetes cluster using Kops:
  ```bash
  kops create cluster --name=kubevpro.yanixproject.online --state=s3://vprofile-kops-yana --zones=us-west-1a \
  --node-size=t3.medium --node-count=2 --master-size=t3.medium --master-count=1 --dns-zone=yanixproject.online
  kops update cluster --name=kubevpro.yanixproject.online --yes --state=s3://vprofile-kops-yana
  ```
2. Validate the cluster:
   ```bash
   kops validate cluster --state=s3://vprofile-kops-yana
   ```

### Application Deployment
1. Apply Kubernetes manifests:
   ```bash
   kubectl apply -f app-secret.yaml
   kubectl apply -f db-CIP.yaml
   kubectl apply -f vprodbdep.yaml
   kubectl apply -f mc-CIP.yaml
   kubectl apply -f mcdep.yaml
   kubectl apply -f rmq-CIP.yaml
   kubectl apply -f rmq-dep.yaml
   kubectl apply -f vproapp-service.yaml
   kubectl apply -f vproappdep.yaml
   kubectl apply -f vproingress.yaml
   ```
2. Check the status of your pods and services:
   ```bash
   kubectl get pods
   kubectl get svc
   kubectl get ingress
   ```

### Testing Connectivity
1. Verify database connectivity from a pod:
   ```bash
   kubectl run debug --rm -it --image=busybox:1.28 --restart=Never -- sh
   nslookup vprodb
   mysql -h vprodb -uroot -pYOUR_PASSWORD
   ```
2. Access the web application:
   Once the setup is complete, visit the URL associated with your Load Balancer. The URL will look something like:
   ```bash
   http://<your-load-balancer-dns>.us-west-1.elb.amazonaws.com
   ```
Replace `<your-load-balancer-dns>` with the actual DNS name of your Load Balancer, which you can find in your AWS Management Console under **EC2 > Load Balancers**.

### 🛠️ Troubleshooting
1. Init Container issues:
* Ensure all services (MySQL, Memcached, RabbitMQ) are configured and accessible.
* Use a specific version of busybox (e.g., busybox:1.28) if latest causes problems.

2. 500 Internal Server Error in Spring Boot:
* Confirm that the database is reachable and that environment variables are correctly set.

3. Ingress access issues:
* Verify that the Ingress Controller is installed and configured.
* Check AWS Security Groups to ensure proper port access.

### Author
Name: Yana Lysenko

GitHub: [YanaDevOps](https://github.com/YanaDevOps)
