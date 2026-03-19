# Docker, DockerHub, and Container Lifecycle Notes

---

# 1. DockerHub

## Definition

**DockerHub** is a **centralized repository for storing and sharing Docker images**.

It allows developers to **upload, download, and manage container images**.

---

# 2. Features of DockerHub

* **Public repositories** → anyone can access images
* **Private repositories** → restricted access
* **Official images** → verified and maintained images (e.g., nginx, python)
* **Automated builds** → automatically build images from source code

---

## Automated Builds (How it works)

DockerHub can connect to:

* GitHub
* Bitbucket

Flow:

```
Code pushed to GitHub
   ↓
DockerHub detects changes
   ↓
Automatically builds image
   ↓
Stores image in repository
```

---

# 3. Importance of DockerHub in DevOps

DockerHub is a **key component in DevOps pipelines** because it enables:

* easy image sharing
* version control of images
* CI/CD integration
* faster deployment
* collaboration between teams

---

# 4. Docker Architecture (Key Components)

## 4.1 Docker Client

### Definition

The **Docker Client** is the interface used by users to interact with Docker.

Examples:

* CLI (`docker` commands)
* Docker Desktop

---

## 4.2 Docker Daemon (Docker Engine)

### Definition

The **Docker Daemon** is the core service that runs in the background and manages:

* images
* containers
* networks
* volumes

---

## 4.3 Docker Images

### Definition

Docker images are **read-only templates used to create containers**.

They contain:

* application code
* dependencies
* runtime
* configuration

---

## 4.4 Containers

### Definition

A **container is a running instance of a Docker image**.

It is:

* lightweight
* isolated
* fast to start

---

## 4.5 Docker Registry

### Definition

A **Docker Registry** is a storage system for Docker images.

Example:

* DockerHub (public registry)

---

# 5. Docker Workflow (Lifecycle)

## Step 1: Build

Create a Docker image using a Dockerfile.

```
docker build
```

---

## Step 2: Ship

Share images via DockerHub or other registries.

```
docker push
docker pull
```

---

## Step 3: Run

Start containers from images.

```
docker run
```

---

## Complete Flow

```
Build → Ship → Run
```

---

# 6. Container States

Containers go through different states:

* **Created** → container is created but not started
* **Running** → container is executing
* **Paused** → execution temporarily stopped
* **Stopped** → container is not running

---

# 7. Why Docker is Better than Virtual Machines

## Key Differences

### 7.1 Shared Kernel

Containers:

* share the host OS kernel

VMs:

* require separate guest OS

---

### 7.2 No Guest OS Boot

Containers:

* no OS boot required

VMs:

* full OS boot needed

---

### 7.3 Resource Usage

Containers:

* lightweight
* low memory usage

VMs:

* heavy
* high resource usage

---

### 7.4 Startup Time

Containers:

* start in seconds

VMs:

* take minutes

---

# 8. Container vs Virtual Machine

| Feature        | Container   | Virtual Machine |
| -------------- | ----------- | --------------- |
| Kernel         | Shared      | Separate        |
| OS             | No guest OS | Full OS         |
| Startup        | Fast        | Slow            |
| Resource Usage | Low         | High            |
| Performance    | High        | Lower           |

---

# 9. Key Summary

* DockerHub = **central image repository**
* Docker = **container platform**
* Image = **blueprint**
* Container = **running instance**
* Lifecycle = **Build → Ship → Run**
* Containers are **faster and lighter than VMs**

---
