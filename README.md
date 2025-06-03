# 🚀 Full Stack CI/CD Pipeline

This project demonstrates a full-stack CI/CD pipeline for a microservices-based application using React (frontend), Java Spring Boot (backend), PostgreSQL/MongoDB (databases), Docker, Jenkins, and AWS EC2.

---

## 🏗️ System Architecture

![System Architecture](Full%20stack%20App%20System%20architecture.png)

---

## 📦 Tech Stack

| Layer        | Technology                         |
| ------------ | ---------------------------------- |
| Frontend     | React.js / Next.js                 |
| Backend      | Java + Spring Boot (Microservices) |
| Database     | PostgreSQL, MongoDB                |
| Realtime     | WebSockets, Redis                  |
| File Storage | AWS S3                             |
| CI/CD        | Jenkins + Docker                   |
| Deployment   | AWS EC2 (Amazon Linux)             |

---

## 🔧 Jenkins Pipeline Stages

1. **Checkout**
2. **Transfer Code to EC2**
3. **Deploy with Docker Compose**

---

## ⚙️ Jenkinsfile Overview

* Clones the GitHub repository
* Transfers code to EC2 instance using SCP & SSH
* Deploys using `docker-compose`

---

## 🔑 Credentials Management

* SSH key is stored in Jenkins Credentials (`ec2-ssh-key-id`)
* `.pem` file used with `sshUserPrivateKey` in pipeline

---

## 🐳 Docker Setup

* Backend and frontend services containerized
* Docker Compose handles orchestration on EC2

---

## 📝 Setup Instructions

1. Add SSH credentials to Jenkins
2. Ensure EC2 instance allows SSH and Docker
3. Install Jenkins, Docker, and Docker Compose on EC2
4. Connect Jenkins to GitHub repository
5. Trigger build manually or on push

---

## 📁 Project Structure

```
/Full-stack-cicd-pipeline
├── backend
├── frontend
├── docker-compose.yml
├── Jenkinsfile
└── README.md
```

---

## ✅ Outcomes

* Code automatically deployed on each push to `main`
* Dockerized microservices running on EC2
* Infrastructure is repeatable and secure

---

## 📬 Contact

For questions or issues, reach out to Shahid Khan on [GitHub](https://github.com/ShahidKhan232).
