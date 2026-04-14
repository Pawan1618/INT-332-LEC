# Container Architecture, Kernel Features & Environment Variables Notes

---

# 1. Virtualization Challenges

## What is Virtualization?

Virtualization is the process of partitioning a physical server into multiple virtual servers using a software called a **hypervisor**. After partitioning, each virtual server runs its own full OS.

---

## Drawbacks of Virtualization

Virtualization directly led to the need for containerization due to these limitations:

* **Heavy Resource Overhead** → Full OS in every VM consumes massive RAM/CPU.
* **Slow Boot Time** → Requires minutes to boot up.
* **Inefficient Scalability** → Hard to scale rapidly under load.
* **Limited Portability** → Migrating VMs between environments is complex.

---

# 2. Container Runtime

## Definition

A **container runtime** is the execution engine responsible for creating, starting, stopping, and managing containers on a host system.

*"Container runtime is what actually runs containers."*

---

## Types of Container Runtimes

### 2.1 High-Level Runtimes

Used by users and DevOps tools to manage the broader container lifecycle.

* **Examples:** Docker Engine, containerd, CRI-O
* **Handles:** Image pulling, networking, storage

---

### 2.2 Low-Level Runtimes

Responsible for interacting with the OS to actually execute the container process.

* **Examples:** runc (OCI-compliant)
* **Handles:** Interacting directly with the Linux Kernel (namespaces and cgroups)

---

# 3. Process Isolation with Namespaces

## Definition

**Namespaces** are Linux kernel features that partition system resources so that processes see only what belongs to them. 

*"Namespaces create the illusion of a separate system for each container."*

---

## Key Types of Namespaces

* **PID Namespace (Process Isolation):** Each container has its own process tree. Processes inside start from PID 1.
* **Network Namespace:** Gives each container its own isolated IP address and ports.
* **Mount Namespace:** Gives each container its own isolated filesystem view. Changes don't affect the host.
* **UTS Namespace:** Allows each container to have its own unique hostname.
* **User Namespace:** Securely maps container "root" users to non-root users on the host machine.

---

# 4. Resource Limits: Control Groups (cgroups)

## Definition

**Control Groups (cgroups)** are Linux kernel features that limit, control, and monitor the resource usage of processes.

*"Namespaces isolate, cgroups control."*

---

## Why cgroups are essential

Without cgroups, one rogue container could consume 100% of the host system's CPU or RAM, crashing everything else.

They manage:
* **CPU:** Limit usage and assign CPU shares.
* **Memory:** Set max limits (OOM kills the container if exceeded).
* **Disk I/O:** Throttle read/write speeds.

---

# 5. Environment Variables in Docker

## Why we need them

Environment variables allow us to dynamically inject configuration (like passwords, modes, or DB connections) into containers without hardcoding them in the image.

(Example: Passing `MYSQL_ROOT_PASSWORD` to a MySQL container).

---

## Using `-e` Flag

```bash
docker run -d -e NGINX_PORT=8080 nginx
```

Sets a single environment variable inside the container.

---

## Using `--env-file` Flag

```bash
docker run -d --env-file .env myapp
```

Used to inject multiple environment variables cleanly from a `.env` file instead of cluttering the CLI.

---

# 6. Additional Docker Management Commands

## Docker Info

```bash
docker info
```

Displays deep system-wide Docker information, including Kernel version, active storage drivers, and the total number of running containers/images.

---

## Docker History

```bash
docker history <image_name>
```

Shows the layer-by-layer history of how a specific Docker image was built (size, commands used to create each layer).

---

## Docker Pause

```bash
docker pause <container_name>
```

Temporarily suspends all processes running inside the container without stopping it entirely. Use `docker unpause` to resume.

---

# 7. Key Summary

* Virtualization needs a hypervisor; containers need a **Container Runtime**.
* **Namespaces** = Isolation (Network, Process, File System).
* **cgroups** = Resource Control (RAM, CPU limits).
* Use **-e** or **--env-file** to pass dynamic configurations into running containers.
* Use **docker history** to audit image layers and **docker info** for system stats.

---
