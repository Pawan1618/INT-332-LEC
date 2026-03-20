# Docker Commands, Logs, Volumes, Network & Troubleshooting Notes

---

# 1. Basic Docker Commands (18 March)

## Pull Image

```bash
docker pull nginx
```

Downloads an image from DockerHub.

---

## Run Container

```bash
docker run -d -p 8080:80 --name mynginx nginx
```

* `-d` → run in background (detached mode)
* `-p` → port mapping (host:container)

---

## List Running Containers

```bash
docker ps
```

Shows only running containers.

---

## Stop Container

```bash
docker stop mynginx
```

---

## Start Container

```bash
docker start mynginx
```

---

## Remove Container

```bash
docker rm -f mynginx
```

* `-f` → force remove (stop + delete)

---

## Remove Image

```bash
docker rmi nginx
```

Deletes image.

---

## Inspect Container

```bash
docker inspect <container>
```

Shows detailed info:

* IP
* network
* mounts
* config

---

## View Logs

```bash
docker logs <container>
```

Displays container output logs.

---

# 2. Container Interaction

## Open Terminal Inside Container

```bash
docker exec -it <container> bash
```

Used to:

* debug
* run commands inside container

---

# 3. Logs and Inspection (19 March)

## Logs

```bash
docker logs mynginx
```

---

## Inspect

```bash
docker inspect mynginx
```

---

# 4. docker run vs -dit

## docker run -d

* Runs container in background

---

## docker run -dit

* `-d` → detached mode
* `-i` → interactive
* `-t` → terminal

Used when you want:

* background + terminal access

---

# 5. Monitor Resource Usage

```bash
docker stats
```

Shows:

* CPU usage
* memory usage
* network usage

---

# 6. Save Data (Volumes)

## Definition

A **volume** is a persistent storage mechanism used to **store data outside containers**.

Data remains even if container is deleted.

---

## Create Volume

```bash
docker volume create myvolume
```

---

## List Volumes

```bash
docker volume ls
```

---

## Inspect Volume

```bash
docker volume inspect myvolume
```

---

## Remove Volume

```bash
docker volume rm myvolume
```

---

# 7. Docker Network Commands

## List Networks

```bash
docker network ls
```

Shows all available networks.

---

# 8. Docker Cleanup

## Check All Containers (Including Stopped)

```bash
docker ps -aq
```

### Definition

* `-a` → all containers
* `-q` → only IDs

Used to:

* get all container IDs for bulk operations

---

## Stop All Containers

```bash
docker stop $(docker ps -aq)
```

---

## Remove All Containers

```bash
docker rm $(docker ps -aq)
```

---

## Remove All Images

```bash
docker rmi $(docker images -q)
```

---

## System Cleanup

```bash
docker system prune -a
```

Removes:

* unused containers
* unused networks
* unused images
* build cache

---

# 9. Troubleshooting a Running Container

## Step 1: Open Bash Inside Container

```bash
docker exec -it <container> bash
```

---

## Step 2: View Logs

```bash
docker logs <container>
```

---

## Step 3: Show Running Processes

Inside container:

```bash
ps aux
```

---

# 10. docker ps vs docker top

## docker ps

```bash
docker ps
```

### Shows:

* running containers
* container ID
* image
* ports
* status

---

## docker top

```bash
docker top <container>
```

### Shows:

* processes running inside container

---

## Difference

| Command    | Purpose                         |
| ---------- | ------------------------------- |
| docker ps  | list containers                 |
| docker top | list processes inside container |

---

## Why Both Exist

* `docker ps` → container-level view
* `docker top` → process-level view

Different use cases:

* monitoring containers → use `docker ps`
* debugging processes → use `docker top`

---

# 11. Key Summary

* docker pull → download image
* docker run → start container
* docker ps → list containers
* docker logs → view logs
* docker exec → access container
* docker stats → monitor resources
* volumes → persistent storage
* networks → container communication
* prune → cleanup system

---
