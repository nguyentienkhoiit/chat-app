# Chat App

## Getting Started

### Prerequisites

- Java 21
- Maven 3.9.9
- PostgreSQL
- Docker

## Running the Application

### Run the Application Locally

1. **Start the Application**:
   ```bash
   ./mvnw spring-boot:run
   ```

### Run the Application with Docker

#### Step 1: Set Up Docker Network

1. **Create a Docker Network** (optional but recommended for container communication):
   ```bash
   docker network create chat-app-network
   ```

#### Step 2: Run PostgreSQL Container

1. **Run PostgreSQL Container**:
   ```bash
   docker run -d \
     --name postgres-chat-app \
     --network chat-app-network \
     -e POSTGRES_USER=postgres \
     -e POSTGRES_PASSWORD=12345678 \
     -e POSTGRES_DB=chat_app \
     -p 5432:5432 \
     postgres:latest
   ```

#### Step 3: Build and Run the Application Container

1. **Build Docker Image**:
   ```bash
   docker build -t chat-app:0.0.1 .
   ```

2. **Run Application Container**:
   ```bash
   docker run -dp 8080:8080 \
     --name chat-app \
     --network chat-app-network \
     -e DATABASE_URL=jdbc:postgresql://postgres-chat-app:5432/chat_app \
     -e DATABASE_USERNAME=postgres \
     -e DATABASE_PASSWORD=12345678 \
     chat-app:0.0.1
   ```

### Additional Docker Commands

- **List Docker Networks**:
  ```bash
  docker network ls
  ```

This revised order ensures that you first set up the necessary Docker network, then run the PostgreSQL container, and finally build and run the application container, all while maintaining clear communication between the containers.

## Push Docker Image to Docker Hub

### Step 1: Build the Docker Image

1. **Build the Docker Image**:
   ```bash
   docker build -t nguyentienkhoi1509/chat-app:0.0.1.1 .
   ```

### Step 2: Log in to Docker Hub

1. **Log in to Docker Hub**:
   ```bash
   docker login
   ```

   - Enter your Docker Hub username and password when prompted.

### Step 3: Push the Docker Image

1. **Push the Docker Image to Docker Hub**:
   ```bash
   docker push nguyentienkhoi1509/chat-app:0.0.1.1
   ```

This sequence ensures that you first build the Docker image, log in to Docker Hub, and then push the image to your Docker Hub repository. Make sure you have an account on Docker Hub and are logged in before attempting to push the image.

## Install Docker on Ubuntu Server

```bash
# Update the package index
sudo apt-get update

# Install required packages for Docker installation
sudo apt-get install -y ca-certificates curl

# Create a directory for Docker's GPG key
sudo install -m 0755 -d /etc/apt/keyrings

# Download Docker's official GPG key
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

# Ensure the GPG key is readable
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add Docker's official repository to Apt sources
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update the package index again
sudo apt-get update

# Install Docker Engine, Docker CLI, containerd, and Docker Compose
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
