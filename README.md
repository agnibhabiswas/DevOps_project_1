# DevOps Sample Application

[![CI/CD Pipeline](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-blue)]()
[![Docker](https://img.shields.io/badge/Containerized-Docker-blue)]()
[![AWS](https://img.shields.io/badge/Cloud-AWS%20EC2-orange)]()
[![Database](https://img.shields.io/badge/Database-MongoDB-green)]()
[![Infrastructure](https://img.shields.io/badge/IaC-Terraform-purple)]()

---

## Overview

This project demonstrates an end-to-end DevOps workflow for a full-stack application, including containerization, CI/CD automation, cloud deployment, and infrastructure provisioning.

The application is an item management system that allows users to create, view, and delete items with persistent storage using MongoDB.

---

## Architecture Diagram

```
                ┌──────────────────────┐
                │      User Browser     │
                └──────────┬───────────┘
                           │
                           ▼
                ┌──────────────────────┐
                │   Express Backend     │
                │   (Node.js App)       │
                └──────────┬───────────┘
                           │
                           ▼
                ┌──────────────────────┐
                │      MongoDB          │
                │   (DB Container)      │
                └──────────────────────┘

                           │
                           ▼

        ┌────────────────────────────────────┐
        │         Docker Network             │
        │   (Inter-container communication)  │
        └────────────────────────────────────┘

                           │
                           ▼

        ┌────────────────────────────────────┐
        │            AWS EC2                 │
        │   (Hosting Docker Containers)      │
        └────────────────────────────────────┘

                           │
                           ▼

        ┌────────────────────────────────────┐
        │        GitHub Actions CI/CD        │
        │ Build → Push → Deploy → Run        │
        └────────────────────────────────────┘
```

---

## Technology Stack

| Layer            | Technology          |
| ---------------- | ------------------- |
| Backend          | Node.js, Express    |
| Database         | MongoDB (Mongoose)  |
| Containerization | Docker              |
| CI/CD            | GitHub Actions      |
| Cloud            | AWS EC2             |
| Infrastructure   | Terraform           |

---

## Key Features

- RESTful API for item management
- Persistent storage using MongoDB
- Dockerized multi-container application
- Automated CI/CD pipeline (build → push → deploy)
- Infrastructure provisioning with Terraform

---

## API Endpoints

| Method | Endpoint         | Description         |
| ------ | ---------------- | ------------------- |
| GET    | `/api/items`     | Retrieve all items  |
| GET    | `/api/items/:id` | Retrieve item by ID |
| POST   | `/api/items`     | Create a new item   |
| DELETE | `/api/items/:id` | Delete an item      |

---

## Docker Architecture

The system runs as two containers connected via a custom Docker network:

| Container    | Image     | Purpose             |
| ------------ | --------- | ------------------- |
| `sample_app` | Custom    | Node.js application |
| `mongo`      | `mongo:6` | MongoDB database    |

**Connection string:**

```
mongodb://mongo:27017/sample_app
```

---

## CI/CD Pipeline

Implemented using GitHub Actions, triggered on every push to `main`.

### Workflow Steps

1. Checkout code
2. Authenticate with Docker Hub
3. Build Docker image
4. Push image to Docker Hub
5. SSH into EC2 instance
6. Pull latest image
7. Ensure Docker network exists
8. Ensure MongoDB container is running
9. Stop and remove old application container
10. Deploy new container with updated image

---

## AWS Deployment

- Hosted on AWS EC2
- Docker containers run directly on the instance
- Security Group ports: `22` (SSH), `3001` (Application)

**Access the application:**

```
http://<EC2-PUBLIC-IP>:3001
```

---

## Infrastructure as Code

Terraform is used to:

- Provision the EC2 instance
- Configure security groups
- Automate Docker setup via `user_data`
- Ensure reproducible infrastructure

---

## Screenshots

### Application UI

![Application UI](image-3.png)

### API Response

![API Response](image-2.png)

### CI/CD Pipeline

![CI/CD Pipeline](image-1.png)

### EC2 Deployment

![EC2 Deployment](image.png)

---

## Local Setup

### 1. Clone the repository

```bash
git clone https://github.com/agnibhabiswas/sample_app_devOPs.git
cd sample_app_devOPs
```

### 2. Start MongoDB

```bash
docker run -d -p 27017:27017 mongo
```

### 3. Install dependencies and run

```bash
npm install
npm run dev
```

---

## Run with Docker (Production Style)

```bash
# Create a shared network
docker network create my-network

# Start MongoDB container
docker run -d \
  --name mongo \
  --network my-network \
  mongo:6

# Start application container
docker run -d \
  -p 3001:3001 \
  --name sample_app \
  --network my-network \
  -e MONGO_URI=mongodb://mongo:27017/sample_app \
  <your-docker-username>/sample_app
```

---

## Key Learnings

- End-to-end CI/CD implementation
- Docker containerization and networking
- Multi-container system design
- AWS EC2 deployment
- Infrastructure automation using Terraform
- Debugging real-world issues:
  - Port conflicts
  - Disk space limitations
  - Docker image inconsistencies
  - Inter-container communication issues

---

## Future Improvements

- Kubernetes deployment (EKS)
- Monitoring and logging integration
- Managed database (MongoDB Atlas)
- Authentication and authorization
- Auto-scaling infrastructure

---

## Author

**Agnibha Biswas**
