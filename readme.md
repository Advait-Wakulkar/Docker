# Complete Docker Guide: From Basics to Advanced

## Evolution of Application Deployment

### 1. Physical Servers
- **Traditional Approach:**  
    Applications were installed directly on dedicated physical servers. Each server typically ran a single application or service to avoid conflicts and maximize stability.
    - **Scaling:** Scaling required purchasing, installing, and configuring new hardware, which was expensive, time-consuming, and often led to underutilized resources.
    - **Maintenance:** Hardware failures or upgrades meant downtime and manual intervention.
    - **Isolation:** Applications were isolated by physical boundaries, making resource sharing inefficient.

### 2. Virtual Machines (VMs)
- **Virtualization Era:**  
    Virtualization technologies (e.g., VMware, Hyper-V, KVM) allowed multiple isolated VMs to run on a single physical server by abstracting hardware resources.
    - **Advantages:**  
        - **Better Resource Utilization:** Multiple VMs can share CPU, memory, and storage, increasing hardware efficiency.
        - **Easier Provisioning:** New VMs can be created from templates or snapshots, reducing setup time.
        - **Improved Flexibility:** VMs can be migrated between hosts for load balancing or maintenance.
        - **Isolation:** Each VM is isolated at the OS level, improving security and stability.
    - **Drawbacks:**  
        - **Resource Overhead:** Each VM runs a full guest OS, consuming significant CPU, memory, and storage.
        - **Management Complexity:** Managing many VMs, patching OSes, and handling licensing can be complex.
        - **Slower Startup:** Booting a VM takes longer due to OS initialization.

### 3. The Dependency Problem
- **"Works on My Machine" Issue:**  
    Developers often faced inconsistent environments and dependency conflicts across different systems (e.g., library versions, OS differences), making deployments unreliable and troubleshooting difficult.
    - **Manual Configuration:** Setting up environments manually led to errors and inconsistencies.
    - **Environment Drift:** Over time, differences between development, testing, and production environments caused unexpected bugs.

### 4. Containers: A Modern Solution
- **Consistent Environments:**  
    Containers package applications with all their dependencies (libraries, binaries, configuration files), ensuring they run identically across different environments—developer laptops, test servers, or production clusters.
    - **Lightweight:** Containers share the host OS kernel, reducing overhead compared to VMs.
    - **Portable:** Containers can run anywhere a compatible container engine is available.
    - **OS-Agnostic:** Containers abstract away OS differences, simplifying cross-platform development.
- **Industry Impact:**  
    - **Origins:** Google pioneered container technologies (cgroups, namespaces) to efficiently run massive workloads.
    - **Docker's Role:** Docker introduced a user-friendly interface and ecosystem, making containers accessible to developers and accelerating adoption across the industry.
    - **Ecosystem Growth:** The container ecosystem now includes registries (Docker Hub), orchestration (Kubernetes), and tooling for CI/CD, security, and monitoring.

---

## Containers vs. Virtual Machines

### Architectural Comparison

```
Virtual Machines:                Containers:

+---------------------+          +---------------------+
|   App   |   App     |          |   App   |   App     |
+---------+-----------+          +---------+-----------+
| Guest OS| Guest OS  |          |  Container Engine   |
+---------+-----------+          +---------------------+
|     Hypervisor      |          |     Host OS         |
+---------------------+          +---------------------+
|     Hardware        |          |     Hardware        |
+---------------------+          +---------------------+
```

### Resource Comparison

| Aspect | Virtual Machines | Containers |
|--------|------------------|------------|
| **Boot Time** | Minutes | Seconds |
| **Resource Usage** | High (full OS) | Low (shared kernel) |
| **Isolation** | Strong (hardware-level) | Process-level |
| **Portability** | Limited (OS-dependent) | High (consistent environment) |
| **Density** | 10-20 per host | 100-1000 per host |
| **Use Case** | Legacy apps, different OSes | Microservices, cloud-native |

- **Virtual Machines:**  
    - Each VM includes its own OS and kernel, providing strong isolation but at the cost of higher resource usage.
    - VMs are managed by a hypervisor (e.g., VMware ESXi, Microsoft Hyper-V, KVM) that allocates hardware resources and enables multiple OS instances to run on shared hardware.
    - VMs are suitable for running different OSes or legacy applications that require full OS isolation.
    - **Use Cases:** Running Windows and Linux workloads on the same server, legacy application support, strong security boundaries.

- **Containers:**  
    - Containers share the host OS kernel, isolating only processes, file systems, and dependencies.
    - They are lightweight, with minimal overhead, enabling fast startup and high density (many containers per host).
    - Managed by a container engine (e.g., Docker Engine, containerd) that handles image management, networking, and lifecycle.
    - **Use Cases:** Microservices, stateless applications, rapid scaling, CI/CD pipelines.

### Practical Example
If you want to share your website with a friend, just sending the code may cause dependency issues (e.g., missing libraries, incompatible OS). By sharing a Dockerfile or container image, your friend can build or run the app in a container, ensuring all dependencies are included and the environment is consistent—solving the "works on my machine" problem.
- **Dockerfile:** Defines the environment and steps to build the image.
- **Image:** Packaged application and dependencies.
- **Container:** Running instance of the image.

---

## Summary

Containers have transformed application deployment by making it simple to package, distribute, and run software consistently across environments.  
However, containers and VMs often complement each other--containers may run inside VMs for added isolation, security, or compatibility with cloud infrastructure.

- **Docker:** The tool and company that popularized containers, providing a complete platform for building, sharing, and running containerized applications.
- **OS Compatibility:**  
    - **Windows containers** require a Windows host (or Windows VM).
    - **Linux containers** require a Linux host (or Linux VM).
    - Some platforms (like Docker Desktop) use lightweight VMs to run Linux containers on Windows or Mac.

- **Security:** Containers provide process-level isolation, but not as strong as VMs. For sensitive workloads, containers can be run inside VMs for defense in depth.

---

## Docker Desktop and WSL

Docker enables running applications in isolated containers. On Windows, Docker Desktop leverages WSL (Windows Subsystem for Linux) to provide a seamless Linux-compatible environment for containers.
- **WSL 2 Integration:**  
    - WSL 2 provides a real Linux kernel running in a lightweight VM, enabling full compatibility with Linux containers.
    - Docker Desktop manages networking, file sharing, and integration with Windows tools.
- **Benefits:**  
    - Developers can use Linux-based containers natively on Windows.
    - Improved performance and compatibility compared to legacy solutions.

---

## Docker Architecture

### High-Level Overview

```
+-----------------------------+
|      Docker CLI / API       |  <-- User interacts via commands or REST API
+-----------------------------+
|         Docker Daemon       |  <-- Manages containers, images, networks, volumes
+-----------------------------+
|         containerd          |  <-- Handles container lifecycle (start, stop, snapshot)
+-----------------------------+
|           runc              |  <-- Creates and runs containers using OCI standards
+-----------------------------+
|        Host OS Kernel       |  <-- Provides namespaces, cgroups, filesystem isolation
+-----------------------------+
|          Hardware           |  <-- Physical or virtual machine resources
+-----------------------------+
```

### Component Breakdown

#### Docker Client
- **CLI Interface:** Command-line tool for interacting with Docker
- **REST API:** Programmatic interface for automation and integrations
- **Docker Compose:** Tool for defining multi-container applications
- **Docker Desktop:** GUI application for managing Docker on desktop systems

#### Docker Daemon (dockerd)
- **Core Service:** Background service that manages Docker objects
- **Image Management:** Pulls, builds, and stores container images
- **Container Lifecycle:** Creates, starts, stops, and removes containers
- **Network Management:** Creates and manages container networks
- **Volume Management:** Handles persistent data storage
- **Security:** Manages container isolation and access controls

#### containerd
- **Container Runtime:** Industry-standard container runtime
- **Image Distribution:** Handles image pulling and pushing
- **Snapshot Management:** Manages container filesystems
- **Network Plugins:** Supports various networking solutions
- **Storage Plugins:** Supports different storage backends

#### runc
- **OCI Runtime:** Reference implementation of OCI runtime specification
- **Container Creation:** Creates containers using Linux kernel features
- **Resource Management:** Applies cgroups for resource limits
- **Namespace Isolation:** Provides process and network isolation
- **Security:** Implements security policies and restrictions

### How Docker Works

1. **User Interaction:**  
     Users interact with Docker via the CLI (`docker run`, `docker build`, etc.) or programmatically via the REST API.

2. **Docker CLI/API → Docker Daemon:**  
     Commands are sent to the Docker Daemon (`dockerd`), which orchestrates container operations, image management, networking, and storage.

3. **Docker Daemon → containerd:**  
     The daemon delegates low-level container lifecycle management (create, start, stop, snapshot) to `containerd`, a core container runtime.

4. **containerd → runc:**  
     `containerd` uses `runc` to create and run containers according to OCI (Open Container Initiative) standards, ensuring compatibility across platforms.

5. **runc → Host OS Kernel:**  
     `runc` leverages kernel features (namespaces for isolation, cgroups for resource control, union filesystems for storage) to securely run containers.

6. **Host OS Kernel → Hardware:**  
     The kernel manages hardware resources (CPU, memory, storage, networking) for all containers and processes.

#### Communication Flow

```
User (CLI/API)
            |
            v
+-------------------+
|   Docker Daemon   |  <-- Orchestrates containers, images, networks
+-------------------+
            |
            v
+-------------------+
|     containerd    |  <-- Manages container lifecycle
+-------------------+
            |
            v
+-------------------+
|       runc        |  <-- Runs containers using kernel features
+-------------------+
            |
            v
+-------------------+
|   Host OS Kernel  |  <-- Provides isolation and resource management
+-------------------+
            |
            v
+-------------------+
|     Hardware      |  <-- Physical or virtual resources
+-------------------+
```

### Linux Kernel Features Used by Docker

#### Namespaces
- **PID Namespace:** Process isolation - each container sees its own process tree
- **Network Namespace:** Network isolation - separate network stack per container
- **Mount Namespace:** Filesystem isolation - separate mount points
- **UTS Namespace:** Hostname isolation - separate hostname and domain name
- **IPC Namespace:** Inter-process communication isolation
- **User Namespace:** User ID isolation - map container users to host users

#### Control Groups (cgroups)
- **CPU Limits:** Restrict CPU usage per container
- **Memory Limits:** Set memory usage boundaries
- **I/O Limits:** Control disk read/write operations
- **Network Bandwidth:** Limit network usage
- **Device Access:** Control access to devices

#### Union Filesystems
- **OverlayFS:** Efficient layered filesystem for images
- **AUFS:** Alternative union filesystem
- **Device Mapper:** Block-level storage driver
- **Btrfs:** Copy-on-write filesystem with snapshotting

---

## Docker Networking

### Network Types

#### Bridge Network (Default)
- **Isolated Network:** Containers on the same bridge can communicate
- **Port Mapping:** Expose container ports to host
- **DNS Resolution:** Automatic service discovery within network
- **Use Case:** Single-host applications, development environments

#### Host Network
- **Shared Network:** Container uses host's network stack directly
- **Performance:** No network overhead, maximum performance
- **Port Conflicts:** Container ports directly bind to host ports
- **Use Case:** High-performance applications, network monitoring tools

#### None Network
- **No Network:** Container has no network access
- **Security:** Complete network isolation
- **Use Case:** Batch processing, security-sensitive applications

#### Custom Networks
- **User-Defined:** Create custom bridge networks
- **Advanced Features:** Custom IP ranges, DNS, isolation
- **Multi-Host:** Connect containers across multiple hosts
- **Use Case:** Complex applications, microservices

### Network Commands

```bash
# List networks
docker network ls

# Create custom network
docker network create mynetwork

# Connect container to network
docker network connect mynetwork mycontainer

# Inspect network details
docker network inspect mynetwork

# Remove network
docker network rm mynetwork
```

---

## Docker Storage

### Volume Types

#### Named Volumes
- **Managed by Docker:** Docker handles storage location and lifecycle
- **Persistent:** Data survives container deletion
- **Shared:** Multiple containers can share the same volume
- **Backup-Friendly:** Easy to backup and restore

```bash
# Create named volume
docker volume create myvolume

# Use named volume
docker run -v myvolume:/data myapp
```

#### Bind Mounts
- **Host Path:** Mount specific host directory or file
- **Development:** Ideal for development workflows
- **Direct Access:** Host can directly access and modify files
- **Path Dependency:** Requires specific host path structure

```bash
# Bind mount host directory
docker run -v /host/path:/container/path myapp

# Bind mount current directory
docker run -v $(pwd):/app myapp
```

#### tmpfs Mounts
- **Memory Storage:** Store data in host memory
- **Temporary:** Data is lost when container stops
- **Performance:** Very fast read/write operations
- **Security:** No data persistence on disk

```bash
# Create tmpfs mount
docker run --tmpfs /tmp myapp
```

### Storage Drivers

#### OverlayFS
- **Default Driver:** Most commonly used storage driver
- **Efficient:** Good performance for read/write operations
- **Layered:** Supports Docker's layered image system
- **Requirements:** Requires modern Linux kernel

#### Device Mapper
- **Block-Level:** Operates at block device level
- **Snapshots:** Efficient copy-on-write snapshots
- **Enterprise:** Often used in enterprise environments
- **Configuration:** Requires specific storage configuration

---

## Docker Image Layers and Optimization

### Understanding Image Layers

Docker images are built in layers, with each instruction in a Dockerfile creating a new, immutable layer on top of the previous one.

```
Image Layer Structure:
+------------------------+
|  Layer 4: App Files    |  <-- COPY . .
+------------------------+
|  Layer 3: Dependencies |  <-- RUN pip install
+------------------------+
|  Layer 2: Requirements |  <-- COPY requirements.txt
+------------------------+
|  Layer 1: Base Image   |  <-- FROM python:3.11
+------------------------+
```

### Layer Caching Strategy

Docker caches layers to speed up builds. Understanding this helps optimize Dockerfiles:

```dockerfile
# Inefficient - cache busted on every code change
FROM python:3.11
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "app.py"]

# Efficient - dependencies cached separately
FROM python:3.11
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

### Multi-Stage Builds

Build applications in multiple stages to reduce final image size:

```dockerfile
# Build stage
FROM node:16 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

# Runtime stage
FROM node:16-alpine
WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY . .
CMD ["node", "index.js"]
```

### Image Optimization Best Practices

#### Use Appropriate Base Images
- **Alpine Linux:** Minimal size (~5MB) for production
- **Distroless:** Google's minimal images with only runtime dependencies
- **Scratch:** Empty image for static binaries

#### Minimize Layers
```dockerfile
# Multiple layers
RUN apt-get update
RUN apt-get install -y curl
RUN apt-get clean

# Single layer
RUN apt-get update && \
    apt-get install -y curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
```

#### Use .dockerignore
Create a `.dockerignore` file to exclude unnecessary files:

```
.git
node_modules
*.log
.env
README.md
```

---

## Docker Compose

### What is Docker Compose?

Docker Compose is a tool for defining and running multi-container Docker applications. It uses a YAML file to configure application services.

### Basic docker-compose.yml Structure

```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "8000:8000"
    depends_on:
      - db
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/myapp
    volumes:
      - .:/app
  
  db:
    image: postgres:13
    environment:
      - POSTGRES_DB=myapp
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
    volumes:
      - postgres_data:/var/lib/postgresql/data
  
  redis:
    image: redis:alpine
    ports:
      - "6379:6379"

volumes:
  postgres_data:

networks:
  default:
    driver: bridge
```

### Common Docker Compose Commands

```bash
# Start all services
docker-compose up

# Start in background
docker-compose up -d

# Stop all services
docker-compose down

# Build and start
docker-compose up --build

# View logs
docker-compose logs

# Scale services
docker-compose up --scale web=3

# Execute command in service
docker-compose exec web bash
```

### Environment Variables

```yaml
# Using environment file
services:
  web:
    env_file:
      - .env
    environment:
      - DEBUG=true
      - DATABASE_URL=${DATABASE_URL}
```

### Health Checks

```yaml
services:
  web:
    image: nginx
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

---

## Container Orchestration

### Why Orchestration?

Managing many containers requires orchestration tools like Docker Swarm or Kubernetes.

- **Orchestration Features:**  
    - **Automatic Restarts:** Failed containers are restarted automatically.
    - **Self-Healing:** Unhealthy containers are replaced to maintain desired state.
    - **Zero-Downtime Updates:** Rolling updates and rollbacks ensure continuous availability during deployments.
    - **Dynamic Scaling:** Containers are added or removed based on demand (horizontal scaling).
    - **Service Discovery:** Containers can find and communicate with each other automatically.
    - **Load Balancing:** Distribute traffic across multiple container instances.
    - **Secrets Management:** Securely manage sensitive data like passwords and API keys.
    - **Configuration Management:** Manage application configuration across environments.

### Docker Swarm

Docker Swarm is Docker's native clustering and orchestration solution.

#### Initialize Swarm
```bash
# Initialize swarm on manager node
docker swarm init

# Join worker nodes
docker swarm join --token <token> <manager-ip>:2377

# Deploy stack
docker stack deploy -c docker-compose.yml myapp
```

#### Service Management
```bash
# Create service
docker service create --name web --replicas 3 nginx

# Scale service
docker service scale web=5

# Update service
docker service update --image nginx:latest web

# Remove service
docker service rm web
```

### Kubernetes Basics

Kubernetes is the most widely used container orchestrator, supporting advanced features like auto-scaling, service meshes, and multi-cloud deployments.

#### Key Concepts
- **Pods:** Smallest deployable units containing one or more containers
- **Services:** Expose applications and provide load balancing
- **Deployments:** Manage application lifecycle and updates
- **ConfigMaps:** Store configuration data
- **Secrets:** Store sensitive information
- **Namespaces:** Isolate resources within clusters

#### Basic Kubernetes Manifest
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: web
        image: nginx:latest
        ports:
        - containerPort: 80
```

---

## Docker Security Best Practices

### Image Security

#### Use Official Base Images
- **Trusted Sources:** Use official images from Docker Hub
- **Minimal Images:** Prefer Alpine or distroless images
- **Regular Updates:** Keep base images updated
- **Image Scanning:** Use tools like Trivy, Clair, or Docker Scout

#### Non-Root User
```dockerfile
# Create non-root user
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nextjs -u 1001

# Switch to non-root user
USER nextjs
```

### Runtime Security

#### Resource Limits
```bash
# CPU and memory limits
docker run --cpus="0.5" --memory="512m" myapp

# In docker-compose.yml
services:
  web:
    image: myapp
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: 512M
```

#### Read-Only Filesystem
```bash
# Make root filesystem read-only
docker run --read-only myapp
```

#### Security Options
```bash
# Drop capabilities
docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE myapp

# Use security profiles
docker run --security-opt seccomp=default.json myapp
```

### Network Security

#### Custom Networks
```bash
# Create isolated network
docker network create --driver bridge isolated-network

# Run containers on isolated network
docker run --network isolated-network myapp
```

#### Secrets Management
```bash
# Docker Swarm secrets
echo "mysecret" | docker secret create db_password -

# Use secret in service
docker service create --secret db_password myapp
```

---

## Monitoring and Logging

### Container Monitoring

#### Built-in Monitoring
```bash
# View container stats
docker stats

# Monitor specific container
docker stats mycontainer

# View resource usage
docker system df

# System-wide information
docker system info
```

#### Health Checks
```dockerfile
# Dockerfile health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost/ || exit 1
```

### Logging

#### Log Drivers
```bash
# JSON file logging (default)
docker run --log-driver json-file myapp

# Syslog logging
docker run --log-driver syslog myapp

# Journald logging
docker run --log-driver journald myapp
```

#### Centralized Logging
```yaml
# ELK Stack example
version: '3.8'
services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.14.0
    environment:
      - discovery.type=single-node
    ports:
      - "9200:9200"
  
  logstash:
    image: docker.elastic.co/logstash/logstash:7.14.0
    depends_on:
      - elasticsearch
  
  kibana:
    image: docker.elastic.co/kibana/kibana:7.14.0
    depends_on:
      - elasticsearch
    ports:
      - "5601:5601"
```

---

## Development Workflows

### Development Environment Setup

#### Hot Reloading
```dockerfile
# Development Dockerfile
FROM node:16
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
CMD ["npm", "run", "dev"]
```

```yaml
# docker-compose.dev.yml
version: '3.8'
services:
  web:
    build: .
    volumes:
      - .:/app
      - /app/node_modules
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
```

#### Multi-Stage Development
```dockerfile
# Base stage
FROM node:16 AS base
WORKDIR /app
COPY package*.json ./

# Development stage
FROM base AS development
RUN npm install
COPY . .
CMD ["npm", "run", "dev"]

# Production stage
FROM base AS production
RUN npm ci --only=production
COPY . .
CMD ["npm", "start"]
```

### CI/CD Integration

#### GitLab CI Example
```yaml
# .gitlab-ci.yml
stages:
  - build
  - test
  - deploy

build:
  stage: build
  script:
    - docker build -t myapp:$CI_COMMIT_SHA .
    - docker push myapp:$CI_COMMIT_SHA

test:
  stage: test
  script:
    - docker run --rm myapp:$CI_COMMIT_SHA npm test

deploy:
  stage: deploy
  script:
    - docker service update --image myapp:$CI_COMMIT_SHA myapp_web
```

#### GitHub Actions Example
```yaml
# .github/workflows/docker.yml
name: Docker Build and Deploy

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    
    - name: Build Docker image
      run: docker build -t myapp:${{ github.sha }} .
    
    - name: Run tests
      run: docker run --rm myapp:${{ github.sha }} npm test
    
    - name: Deploy to production
      run: |
        docker tag myapp:${{ github.sha }} myapp:latest
        docker push myapp:latest
```

---

## Advanced Docker Features

### Docker Buildx

Multi-platform builds and advanced build features:

```bash
# Create builder instance
docker buildx create --name mybuilder --use

# Build for multiple platforms
docker buildx build --platform linux/amd64,linux/arm64 -t myapp:latest .

# Build with advanced features
docker buildx build --target production --cache-from type=registry,ref=myapp:cache .
```

### Docker Scout

Security analysis and recommendations:

```bash
# Analyze image for vulnerabilities
docker scout quickview myapp:latest

# Compare images
docker scout compare myapp:v1.0 myapp:v2.0

# View detailed recommendations
docker scout recommendations myapp:latest
```

### Docker Extensions

Extend Docker Desktop functionality:

```bash
# Install extensions
docker extension install docker/disk-usage-extension

# List installed extensions
docker extension ls

# Enable/disable extensions
docker extension enable docker/disk-usage-extension
```

---

## Troubleshooting Guide

### Common Issues and Solutions

#### Container Won't Start
```bash
# Check container logs
docker logs mycontainer

# Check container configuration
docker inspect mycontainer

# Run container interactively
docker run -it myimage /bin/bash
```

#### Permission Issues
```bash
# Check file permissions
docker run -it myimage ls -la /app

# Fix ownership in Dockerfile
RUN chown -R appuser:appuser /app
USER appuser
```

#### Network Connectivity
```bash
# Test container networking
docker exec mycontainer ping google.com

# Check network configuration
docker network inspect bridge

# Test port connectivity
docker exec mycontainer netstat -tlnp
```

#### Resource Issues
```bash
# Check system resources
docker system df

# Clean up unused resources
docker system prune

# Monitor resource usage
docker stats
```

### Debugging Techniques

#### Container Debugging
```bash
# Access running container
docker exec -it mycontainer /bin/bash

# Copy files from container
docker cp mycontainer:/app/logs ./logs

# Check container processes
docker exec mycontainer ps aux
```

#### Build Debugging
```bash
# Build with verbose output
docker build --progress=plain .

# Debug build cache
docker build --no-cache .

# Build specific stage
docker build --target development .
```

---

## Performance Optimization

### Build Performance

#### Optimize Docker Context
```dockerfile
# Use .dockerignore
.git
node_modules
*.log
.env
README.md
```

#### Parallel Builds
```bash
# Use BuildKit for parallel builds
DOCKER_BUILDKIT=1 docker build .

# Multi-stage parallel building
docker build --target stage1 . &
docker build --target stage2 . &
wait
```

### Runtime Performance

#### Resource Optimization
```bash
# Optimize container resources
docker run --cpus="2" --memory="4g" --memory-swap="4g" myapp

# Use resource constraints
docker run --oom-kill-disable --memory="1g" myapp
```

#### Storage Optimization
```bash
# Use tmpfs for temporary data
docker run --tmpfs /tmp:rw,noexec,nosuid,size=100m myapp

# Optimize volume performance
docker run -v myvolume:/data:rw,Z myapp
```

---

## Essential Docker Commands Reference

### Image Management

```bash
# List images
docker images
docker image ls

# Pull image
docker pull nginx:latest

# Build image
docker build -t myapp:latest .

# Tag image
docker tag myapp:latest myapp:v1.0

# Push image
docker push myapp:latest

# Remove image
docker rmi myapp:latest

# Remove unused images
docker image prune

# Remove all images
docker image prune -a

# Save image to tar
docker save myapp:latest > myapp.tar

# Load image from tar
docker load < myapp.tar

# View image history
docker history myapp:latest

# Inspect image
docker inspect myapp:latest
```

### Container Management

```bash
# List running containers
docker ps

# List all containers
docker ps -a

# Run container interactively
docker run -it ubuntu /bin/bash

# Run container in background
docker run -d nginx

# Run with port mapping
docker run -p 8080:80 nginx

# Run with volume
docker run -v myvolume:/data nginx

# Run with environment variables
docker run -e NODE_ENV=production myapp

# Start stopped container
docker start mycontainer

# Stop running container
docker stop mycontainer

# Restart container
docker restart mycontainer

# Remove container
docker rm mycontainer

# Remove all stopped containers
docker container prune

# Execute command in running container
docker exec -it mycontainer /bin/bash

# View container logs
docker logs mycontainer

# Follow log output
docker logs -f mycontainer

# Copy files to/from container
docker cp file.txt mycontainer:/app/
docker cp mycontainer:/app/file.txt ./

# View container resource usage
docker stats mycontainer

# Inspect container
docker inspect mycontainer
```

### Network Management

```bash
# List networks
docker network ls

# Create network
docker network create mynetwork

# Create network with options
docker network create --driver bridge --subnet=192.168.1.0/24 mynetwork

# Connect container to network
docker network connect mynetwork mycontainer

# Disconnect container from network
docker network disconnect mynetwork mycontainer

# Inspect network
docker network inspect mynetwork

# Remove network
docker network rm mynetwork

# Remove unused networks
docker network prune
```

### Volume Management

```bash
# List volumes
docker volume ls

# Create volume
docker volume create myvolume

# Create volume with options
docker volume create --driver local --opt type=tmpfs myvolume

# Inspect volume
docker volume inspect myvolume

# Remove volume
docker volume rm myvolume

# Remove unused volumes
docker volume prune

# Mount volume in container
docker run -v myvolume:/data nginx

# Bind mount host directory
docker run -v /host/path:/container/path nginx

# Mount current directory
docker run -v $(pwd):/app nginx
```

### System Management

```bash
# View system information
docker system info

# View disk usage
docker system df

# Clean up unused resources
docker system prune

# Clean up everything
docker system prune -a

# Clean up volumes too
docker system prune -a --volumes

# Monitor system events
docker system events

# View Docker version
docker version

# View Docker info
docker info
```

---

## Analogy: Recipes and Containers

Imagine you want to share a special dish with a distant friend. Instead of sending the cooked meal (which may spoil), you send the recipe and ingredients. Your friend can then recreate the dish exactly as you made it, regardless of their kitchen setup.

Similarly, with Docker, you share a Dockerfile (the recipe) or a container image (the prepared ingredients), so anyone can reproduce the same application environment anywhere.

- **Dockerfile:** The recipe (instructions for building the image).
- **Image:** Prepared ingredients (packaged environment with app and dependencies).
- **Container:** The finished dish (running application instance).

```
Recipe (Dockerfile) ---> Image (Prepared Ingredients) ---> Container (Dish Ready to Eat)
```

### How does Docker work?

When you run:

```bash
docker run hello-world
```

- **Step 1:** Docker checks if the `hello-world` image exists locally.
- **Step 2:** If not found, Docker downloads the image from a remote repository (Docker Hub).
- **Step 3:** Docker creates a new container from the image and starts it.
- **Step 4:** The container runs the application (in this case, prints a hello message) and then exits.

**In summary:**  
You are running an image to create a container (hello-world). Docker handles downloading, setting up the environment, and running the application, ensuring it works the same way on any system with Docker installed.

### Docker Image Caching

When you run a Docker command to start a container (e.g., `docker run hello-world`), Docker first checks if the required image is already present in your local image cache:

- **Image Already Present:**  
    If the image exists locally, Docker uses the cached image immediately, avoiding the need to download it again. This makes container startup much faster and saves bandwidth.

- **Image Not Present:**  
    If the image is not found locally, Docker pulls it from a remote registry (such as Docker Hub) and stores it in the local cache for future use.

**Benefits of Caching:**
- Faster container startup for frequently used images.
- Reduced network usage and download times.
- Efficient use of storage by reusing existing image layers.

**Example Workflow:**
1. First run:  
    ```bash
    docker run ubuntu
    ```
    - Docker downloads the `ubuntu` image if not present, then runs the container.

2. Subsequent runs:  
    ```bash
    docker run ubuntu
    ```
    - Docker uses the cached `ubuntu` image, starting the container instantly.

This caching mechanism also applies when building images with Dockerfiles—unchanged layers are reused from cache to speed up builds.

### Do Docker Containers Have OS Files and Dependencies?

Yes, Docker containers include some operating system files and dependencies, but not a full OS. Each container is created from an image that contains:

- **Application code** and any required binaries or scripts.
- **Dependencies** such as libraries, language runtimes, and tools needed by the application.
- **Minimal OS files** (e.g., from a base image like `alpine`, `ubuntu`, or `debian`). These provide essential utilities and libraries, but not a full operating system or kernel.

**Key Points:**
- Containers share the host OS kernel, so they do not run their own kernel.
- Each container has its own isolated filesystem, processes, and network stack.
- Multiple containers can run different dependencies and OS files simultaneously, without interfering with each other.

**Example:**  
A Python app in a container might use a `python:3.11-slim` image, which includes Python, required libraries, and minimal Debian OS files—just enough to run the app, but much less than a full VM.

This lightweight approach makes containers fast to start, efficient, and portable across environments.

---

## Example: Creating a Simple Dockerfile

Let's create a basic Dockerfile that uses Ubuntu, updates package lists, and prints "Hello World" when the container runs.

**Dockerfile:**
```dockerfile
FROM ubuntu:latest
RUN apt-get update
CMD echo "Hello World"
```

**How it works:**
- `FROM ubuntu:latest` sets the base image to Ubuntu.
- `RUN apt-get update` updates the package lists inside the image.
- `CMD echo "Hello World"` specifies the command to run when the container starts.

**Build and run the container:**
```bash
docker build -t ubuntu-hello .
docker run ubuntu-hello
```

**Expected output:**
```
Hello World
```

This demonstrates how to create a custom Docker image that performs setup steps and runs a command when started.

---

## Real-World Examples

### Web Application Example

**Dockerfile:**
```dockerfile
FROM node:16-alpine

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy application code
COPY . .

# Expose port
EXPOSE 3000

# Create non-root user
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nextjs -u 1001

# Change ownership
RUN chown -R nextjs:nodejs /app
USER nextjs

# Start application
CMD ["npm", "start"]
```

**docker-compose.yml:**
```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://user:password@db:5432/myapp
    depends_on:
      - db
      - redis
    restart: unless-stopped

  db:
    image: postgres:13-alpine
    environment:
      - POSTGRES_DB=myapp
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: unless-stopped

  redis:
    image: redis:6-alpine
    volumes:
      - redis_data:/data
    restart: unless-stopped

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - web
    restart: unless-stopped

volumes:
  postgres_data:
  redis_data:
```

### Microservices Example

**API Service Dockerfile:**
```dockerfile
FROM python:3.11-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements and install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Create non-root user
RUN useradd --create-home --shell /bin/bash app
RUN chown -R app:app /app
USER app

# Expose port
EXPOSE 8000

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:8000/health || exit 1

# Start application
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "app:create_app()"]
```

**docker-compose.yml for Microservices:**
```yaml
version: '3.8'

services:
  api:
    build: ./api
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/api
      - REDIS_URL=redis://redis:6379
    depends_on:
      - db
      - redis
    networks:
      - backend

  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    environment:
      - REACT_APP_API_URL=http://localhost:8000
    depends_on:
      - api
    networks:
      - frontend

  db:
    image: postgres:13-alpine
    environment:
      - POSTGRES_DB=api
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - backend

  redis:
    image: redis:6-alpine
    volumes:
      - redis_data:/data
    networks:
      - backend

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - api
      - frontend
    networks:
      - frontend
      - backend

volumes:
  postgres_data:
  redis_data:

networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge
```

---

## Conclusion

Docker has revolutionized how we build, ship, and run applications by providing a consistent, portable, and efficient containerization platform. From simple single-container applications to complex microservices architectures, Docker provides the tools and ecosystem needed for modern software development and deployment.

Key takeaways:
- **Containers solve the "works on my machine" problem** by packaging applications with their dependencies
- **Docker provides a complete ecosystem** for container management, from development to production
- **Orchestration tools** like Docker Swarm and Kubernetes enable scalable, resilient applications
- **Best practices** around security, performance, and optimization are crucial for production deployments
- **Integration with CI/CD** pipelines streamlines development and deployment workflows

Whether you're a developer looking to containerize your applications or a DevOps engineer managing production workloads, Docker provides the foundation for modern, cloud-native applications.

---

*This guide covers the essential concepts and practical knowledge needed to effectively use Docker in real-world scenarios. Continue exploring the Docker ecosystem and community resources to deepen your understanding and stay updated with the latest developments.*