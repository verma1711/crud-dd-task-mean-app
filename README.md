MEAN Stack CRUD Application – Deployment

Overview

This project is a full-stack CRUD application built using the MEAN stack (MongoDB, Express, Angular 15 and Node.js).

The main goal of this task was not just building the application, but containerizing it properly and deploying it on AWS EC2 using Docker and GitHub Actions CI/CD.

The app manages tutorials. Each tutorial has:

ID

Title

Description

Published status

Users can create, update, delete, retrieve and also search tutorials by title.

Tech Stack Used

Frontend:

Angular 15

HTTPClient

Backend:

Node.js

Express

Database:

MongoDB

DevOps & Deployment:

Docker

Docker Compose

Nginx (as reverse proxy)

AWS EC2

GitHub Actions

Docker Hub

Architecture Overview

The traffic flow looks like this:

User (Browser)
→ Nginx (running on EC2, port 80)
→ Frontend container
→ Backend container
→ MongoDB container

Nginx handles all incoming traffic.

Requests to / go to frontend

Requests to /api go to backend

Backend connects to MongoDB using Docker internal network. MongoDB data is stored in a named volume so it does not get lost when containers restart.

Containerization

The application is fully containerized using Docker.

Instead of building images directly on EC2, images are built through GitHub Actions and pushed to Docker Hub:

anurag1711/mean-backend:latest

anurag1711/mean-frontend:latest

EC2 only pulls these images and runs them using docker-compose.

To start containers on EC2:

docker compose up -d

Deployment on AWS EC2

Steps followed:

Created Ubuntu EC2 instance.

Installed Docker and Docker Compose.

Added docker-compose.yml and Nginx config on the server.

Opened port 80 in security group.

Used GitHub Actions to automatically deploy.

After deployment, application is accessible using:

http://<EC2-PUBLIC-IP>

CI/CD Pipeline

The GitHub Actions workflow runs on every push to main branch.

Pipeline does the following:

Checkout the code

Build backend Docker image

Build frontend Docker image

Push images to Docker Hub

SSH into EC2

Pull latest images

Restart containers

So whenever I push changes, the server updates automatically. No need to manually login and deploy again.

Repository Structure

backend/
frontend/
nginx/default.conf
docker-compose.yml
.github/workflows/
README.md

Environment Configuration

The backend uses environment variable for database connection:

MONGO_URL=mongodb://mongo:27017/dd_db

Since containers are on same Docker network, they can communicate using service names.


Final Notes

This project helped me understand properly how multi-container applications work together, how reverse proxy is configured using Nginx, and how CI/CD can automate the entire deployment process.

The focus of this task was mainly on containerization, automation and cloud deployment rather than just building the application.