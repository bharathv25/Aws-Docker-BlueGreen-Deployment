 AWS Docker Blue-Green Deployment

📌 Project Overview
Production-grade AWS infrastructure built from scratch featuring VPC networking, Dockerized web application with Blue-Green deployment strategy, MySQL primary-replica replication across public/private subnets, and automated S3 log storage using IAM roles.

🏗️ Architecture
![Architecture Diagram](architecture/Architecture-Diagram.jpeg)

 🛠️ Tech Stack
- Cloud: AWS (VPC, EC2, S3, IAM, NAT Gateway)
- Containerization: Docker, Docker Compose
- Web Application: BookStack
- Database: MySQL 8.0
- Reverse Proxy:Nginx
- OS:Ubuntu 24.04 LTS

 ✨ Features
- Custom VPC with Public and Private Subnets
- Blue-Green Zero Downtime Deployment
- MySQL Primary-Replica Replication
- Nginx Reverse Proxy Traffic Switching
- Automated MySQL Log Upload to S3
- IAM Role based S3 Authentication
- NAT Gateway for Private EC2 internet access

