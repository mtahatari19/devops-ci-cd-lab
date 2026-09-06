# DevOps CI/CD Lab

A hands-on DevOps portfolio project demonstrating containerization, service configuration, health monitoring, and CI/CD workflows using Docker, Docker Compose, Nginx, Linux, and GitLab CI/CD.

## Overview

This project runs a lightweight web application inside an Nginx container and demonstrates a practical container-based application delivery workflow.

The repository includes:

- Docker image creation
- Docker Compose orchestration
- Custom Nginx configuration
- Docker container health checks
- Application health endpoint
- GitLab CI/CD pipeline configuration
- Automated repository validation
- Docker image build and testing
- Container Registry publishing workflow

## Architecture

```text
Developer
    |
    | git push
    v
Git Repository
    |
    v
GitLab CI/CD
    |
    +-------------------+
    |                   |
    v                   |
 Validate               |
    |                   |
    v                   |
 Build Docker Image     |
    |                   |
    v                   |
 Test Container         |
    |                   |
    +--> GET /           |
    |                   |
    +--> GET /health     |
    |                   |
    v                   |
 Publish Image          |
    |                   |
    v                   |
Container Registry <----+

Tech Stack
Linux
Docker
Docker Compose
Nginx
Git
GitLab CI/CD
Shell
Container Health Checks
Project Structure
devops-ci-cd-lab/
├── app/
│   └── index.html
├── nginx/
│   └── nginx.conf
├── Dockerfile
├── docker-compose.yml
├── .dockerignore
├── .gitlab-ci.yml
└── README.md
Application

The project contains a simple static web application served by Nginx.

The main application endpoint is:

http://localhost:8080

The application also exposes a health endpoint:

http://localhost:8080/health

Expected health response:

ok
Run Locally
1. Clone the repository

Using SSH:

git clone git@github.com:mtahatari19/devops-ci-cd-lab.git
cd devops-ci-cd-lab

Or using HTTPS:

git clone https://github.com/mtahatari19/devops-ci-cd-lab.git
cd devops-ci-cd-lab
2. Build and start the environment
docker compose up -d --build
3. Check the running container
docker ps

A healthy container should show a status similar to:

Up (...) (healthy)
4. Test the application
curl http://localhost:8080
5. Test the health endpoint
curl http://localhost:8080/health

Expected output:

ok
6. Check Docker health status
docker inspect \
  --format='{{.State.Health.Status}}' \
  devops-ci-cd-lab

Expected output:

healthy
Docker

The application is packaged using an Alpine-based Nginx image.

The Docker image:

Copies the static application into the Nginx web root
Uses a custom Nginx configuration
Exposes port 80
Includes a Docker health check
Verifies the /health endpoint automatically

Build the image manually:

docker build -t devops-ci-cd-lab .

Run it manually:

docker run -d \
  --name devops-ci-cd-lab \
  -p 8080:80 \
  devops-ci-cd-lab
Docker Compose

Docker Compose is used to simplify local deployment.

Start the environment:

docker compose up -d --build

View running services:

docker compose ps

View logs:

docker compose logs -f

Stop and remove the environment:

docker compose down
Nginx

Nginx is used as the web server for the application.

The custom configuration provides:

Static content serving
SPA-compatible fallback routing
Dedicated /health endpoint
Lightweight health monitoring

Health endpoint:

location = /health {
    access_log off;
    add_header Content-Type text/plain;
    return 200 "ok\n";
}
CI/CD Pipeline

The repository contains a GitLab CI/CD configuration in:

.gitlab-ci.yml

The pipeline is designed with four stages:

Validate
   |
   v
Build
   |
   v
Test
   |
   v
Publish
Validate

Checks that the required project files exist before continuing.

Files validated include:

Dockerfile
docker-compose.yml
nginx/nginx.conf
app/index.html
Build

Builds a Docker image using the current commit SHA as the image tag.

Example:

devops-ci-cd-lab:a1b2c3d4

The image is then exported as a pipeline artifact for use in later stages.

Test

The test stage:

Loads the Docker image generated during the build stage
Starts a container
Tests the main application endpoint
Tests the /health endpoint
Verifies that the application is reachable

Endpoints tested:

/

and:

/health
Publish

When the pipeline runs on the default branch, the Docker image is prepared for publishing to the GitLab Container Registry.

Images are tagged using:

$CI_COMMIT_SHORT_SHA

and:

latest

The pipeline uses GitLab-provided registry variables:

CI_REGISTRY
CI_REGISTRY_USER
CI_REGISTRY_PASSWORD
CI_REGISTRY_IMAGE
CI/CD Workflow
Developer
    |
    | Push Code
    v
GitLab Repository
    |
    v
Pipeline Triggered
    |
    v
Validate Repository
    |
    v
Build Docker Image
    |
    v
Run Container
    |
    +------> Test /
    |
    +------> Test /health
    |
    v
Publish Docker Image
    |
    v
GitLab Container Registry
Health Monitoring

The Dockerfile contains a built-in health check.

Docker periodically requests:

http://127.0.0.1/health

The container is considered healthy when the endpoint responds successfully.

Check the status using:

docker inspect \
  --format='{{.State.Health.Status}}' \
  devops-ci-cd-lab
Useful Commands

Build and start:

docker compose up -d --build

Check containers:

docker ps

Check Compose services:

docker compose ps

View logs:

docker compose logs -f

Test application:

curl http://localhost:8080

Test health endpoint:

curl http://localhost:8080/health

Check health status:

docker inspect \
  --format='{{.State.Health.Status}}' \
  devops-ci-cd-lab

Stop environment:

docker compose down

Rebuild from scratch:

docker compose down
docker compose build --no-cache
docker compose up -d
Repository Purpose

This repository is part of my DevOps portfolio.

It was created to demonstrate practical experience with:

Containerization
Docker image creation
Docker Compose
Linux-based application environments
Nginx configuration
Application health checks
CI/CD pipeline design
Automated testing
Container Registry workflows
Deployment automation
Infrastructure troubleshooting
GitHub and GitLab

The public source code is currently hosted on GitHub for portfolio visibility.

The .gitlab-ci.yml file is designed for GitLab CI/CD.

The CI/CD pipeline becomes executable when the repository is pushed or mirrored to a GitLab project with an appropriate GitLab Runner and Container Registry configuration.

Author

Taha Tari

DevOps Engineer

GitHub:
https://github.com/mtahatari19

LinkedIn:
https://www.linkedin.com/in/taha-tari19
