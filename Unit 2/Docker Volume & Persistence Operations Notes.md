# Docker Volume & Persistence Operations Notes
---

# 1. Understanding Docker Volumes
---

## Definition

A **Docker Volume** is a **dedicated storage mechanism** managed entirely by the Docker engine that allows data to **survive beyond the lifecycle** of any individual container. Volumes fundamentally decouple persistent state from the ephemeral nature of containers, ensuring that critical application data is **never lost** when a container is stopped, removed, or recreated.

*"Volumes exist independently of containers — they are the backbone of persistent storage in Docker."*

* **Volume** → a managed directory on the host filesystem that Docker mounts into containers at a specified path
* **Persistence** → the ability of data to remain intact and accessible even after the originating container has been destroyed
* **Mount Point** → the internal container path where the volume's contents are made available

---

# 2. Essential Volume Management Commands
---

## Definition

Docker natively provides a **comprehensive set of CLI commands** specifically designed to **create, list, examine, and clean up** volumes. These commands give operators full control over the storage layer without needing to interact with the host filesystem directly.

### Creating a Volume

```bash
docker volume create <volumename>
```

* **create** → initializes a brand-new named volume on the Docker host, ready to be attached to any container

### Listing All Volumes

```bash
docker volume ls
```

* **ls** → displays every volume currently registered with the Docker daemon, whether in use or idle

### Inspecting a Volume

```bash
docker volume inspect <volumename>
```

* **inspect** → reveals detailed metadata about a specific volume including its **mount path**, **driver**, and **creation timestamp**

### Removing a Specific Volume

```bash
docker volume rm <volumename>
```

* **rm** → permanently deletes a named volume — this action is **irreversible** and the data will be lost

### Pruning Unused Volumes

```bash
docker volume prune
```

* **prune** → aggressively removes **all volumes** that are not currently mounted to any running or stopped container, freeing up disk space cleanly

---

# 3. Mounting Volumes to Containers
---

## Definition

**Volume mounting** is the process of **attaching a persistent volume** to a container at runtime using the `-v` flag. This securely maps a named volume to a **specific directory path** inside the container, enabling seamless read/write access to persistent storage.

### Mount Syntax

```bash
docker run -d -v <volumename>:<container_path> --name <containername> <imagename>
```

| Flag / Argument | Purpose |
|---|---|
| `-d` | Runs the container in **detached mode** (background) |
| `-v` | Binds a **named volume** to an internal container directory |
| `--name` | Assigns a **human-readable identifier** to the container |

* **-v flag** → the core mechanism that bridges external persistent storage with the container's internal filesystem
* **container_path** → the absolute directory inside the container where volume data becomes accessible

---

# 4. Practical Workflow — Step-by-Step Example
---

## Definition

The following **hands-on workflow** demonstrates how to **provision a volume**, **attach it to a running container**, **write data**, and **verify that the data persists** even after the container is completely removed. This is fundamentally the most important pattern to master for production-grade Docker usage.

### Step 1 — Create a Named Volume

```bash
docker volume create mydata
```

*"This initializes a fresh volume called mydata, managed entirely by Docker."*

### Step 2 — Launch a Container with the Volume Mounted

```bash
docker run -dit --name mycontainer -v mydata:/app/data ubuntu
```

* **-dit** → starts the container in **detached interactive mode** with a terminal session allocated
* **/app/data** → the directory inside the container where **mydata** volume contents will appear

### Step 3 — Verify the Mount Inside the Container

```bash
docker exec -it mycontainer ls /app/data
```

* **exec** → runs a command directly inside an already-running container without restarting it

### Step 4 — Write Some Data to the Volume

```bash
docker exec -it mycontainer bash -c "echo 'persistent content' > /app/data/testfile.txt"
```

### Step 5 — Destroy the Container Entirely

```bash
docker stop mycontainer
docker rm mycontainer
```

### Step 6 — Reattach the Volume to a New Container and Verify

```bash
docker run -dit --name newcontainer -v mydata:/app/data ubuntu
docker exec -it newcontainer cat /app/data/testfile.txt
```

*"The data written by the first container is still fully intact — volumes survive container deletion."*

### Workflow Flow

```
docker volume create mydata
        ↓
docker run -dit --name mycontainer -v mydata:/app/data ubuntu
        ↓
docker exec -it mycontainer ls /app/data
        ↓
Write data inside /app/data
        ↓
docker stop & docker rm mycontainer
        ↓
Reattach mydata to a new container
        ↓
Data is still present ✓
```

---

# 5. Sharing Volumes Across Multiple Containers
---

## Definition

Docker volumes are **not exclusive** to a single container — they can be **simultaneously mounted** into multiple containers, enabling powerful **data-sharing patterns** across different services. This is heavily used in scenarios like logging, shared configuration, and inter-service communication.

```bash
docker run -dit --name container_one -v sharedvol:/shared/data ubuntu
docker run -dit --name container_two -v sharedvol:/shared/data ubuntu
```

* **Shared Volume** → both containers read from and write to the **exact same underlying storage**, making collaboration between services seamless
* **Concurrency Consideration** → when multiple containers write to the same volume, the application layer must handle **file locking** or **conflict resolution** appropriately

---

# 6. Inspecting Volume Mount Details
---

## Definition

The **docker inspect** command can be used on both **volumes and containers** to reveal precisely how and where volumes are mounted. This is critically useful for debugging storage issues and verifying that mounts are configured correctly.

```bash
docker inspect mycontainer
```

The output will include a **Mounts** section showing:

* **Type** → indicates whether the mount is a **volume**, **bind**, or **tmpfs**
* **Source** → the actual path on the **host machine** where data is physically stored
* **Destination** → the path **inside the container** where the volume is accessible
* **RW** → boolean flag indicating whether the mount is **read-write** or **read-only**

---

# 7. Key Summary
---

* **Docker Volumes** are the primary and most reliable mechanism for **persisting data** beyond the container lifecycle
* Core volume commands include **create**, **ls**, **inspect**, **rm**, and **prune** — covering the full management lifecycle
* The **-v flag** during `docker run` mounts a named volume to a specific internal container path
* Data stored in a volume **survives container deletion** — attaching the same volume to a new container restores all previously written data
* Volumes can be **shared across multiple containers**, enabling cross-service data access natively
* **docker inspect** reveals comprehensive mount details including source path, destination, and read/write permissions
* **docker volume prune** is a cleanup utility that removes all volumes not currently attached to any container — use it carefully in production environments
