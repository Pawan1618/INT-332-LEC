# Docker Run, Port Mapping & Environment Variable Notes

---

# 1. Understanding `docker run`

## Definition

The **`docker run`** command is the fundamental mechanism through which a **container is instantiated and executed** from a specified **image**. It essentially combines container creation and startup into a single, powerful operation — making it the most heavily used command in the entire Docker workflow.

*"Running a container is the act of breathing life into a static image."*

## Command Structure

The general syntax follows this pattern:

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

* **OPTIONS** → flags that modify container behavior (networking, naming, interactivity, etc.)
* **IMAGE** → the base image from which the container is derived
* **COMMAND** → an optional override for the default entrypoint of the image
* **ARG...** → additional arguments passed directly to the command

## Basic Execution Examples

Launching a container from the **httpd** image and immediately echoing a message:

```bash
docker run httpd echo 'Hello, World!'
```

Assigning a **custom name** to the container for easier identification:

```bash
docker run --name my-container httpd echo 'Hello, World!'
```

---

# 2. Common `docker run` Options

## Definition

**Options** (also called **flags**) are special parameters that fundamentally alter how a container is created and behaves at runtime. They provide granular control over everything from networking to cleanup behavior.

## Options at a Glance

| Option | Flag | Purpose |
|---|---|---|
| Interactive Terminal | `-it` | Attaches an interactive TTY session to the container |
| Detached Mode | `-d` | Runs the container silently in the background |
| Custom Name | `--name` | Assigns a human-readable name to the container |
| Auto-Remove | `--rm` | Automatically deletes the container once it exits |
| Port Mapping | `-p` | Publishes and maps a host port to a container port |
| Environment Variable | `-e` | Injects a key-value configuration variable |
| Volume Mount | `-v` | Mounts a host directory or volume into the container |

## Arrow Summary

* **`-it`** → opens an interactive shell session, combining `-i` (stdin) and `-t` (pseudo-TTY)
* **`-d`** → detaches the process so the container runs cleanly in the background
* **`--name`** → gives the container a memorable, user-defined identifier
* **`--rm`** → ensures the container is automatically purged after it stops executing
* **`-p`** → securely maps a port on the host machine to one inside the container
* **`-e`** → passes environment configuration natively into the container runtime
* **`-v`** → persistently mounts external storage into the container filesystem

---

# 3. Port Mapping & Publishing

## Definition

**Port mapping** (triggered by the **`-p`** flag, which stands for **publish**) is the process of explicitly linking a **host machine port** to a **container port**, thereby exposing the containerized application to external network traffic.

*"Without port mapping, a container is an island — running but completely unreachable from the outside world."*

## Why Port Mapping Is Necessary

By default, every container operates within its own **isolated network namespace**. This means that services running inside the container are fundamentally **inaccessible** from the host machine or any external client. Port publishing bridges this gap securely.

## Syntax

```bash
docker run -p <HOST_PORT>:<CONTAINER_PORT> IMAGE
```

## Flow of a Port-Mapped Request

```
Client Request on Host Port
        ↓
Docker Engine Intercepts
        ↓
Traffic Forwarded to Container Port
        ↓
Application Inside Container Responds
```

## Practical Example

Running an **httpd** web server and mapping host port **8080** to the container's internal port **80**:

```bash
docker run -d -p 8080:80 httpd
```

Now the web server is cleanly accessible at `http://localhost:8080` on the host machine.

---

# 4. Environment Variables

## Definition

**Environment variables** are **key-value pairs** that are injected into the container's runtime environment to **configure application behavior** without modifying the underlying image or code.

*"Environment variables let you control what an application does without ever touching its source code."*

## Common Use Cases

* **Reading configuration** → applications natively pull settings like database URLs or API endpoints from environment variables
* **Storing secrets** → sensitive data such as passwords and tokens can be passed securely at runtime
* **Controlling behavior** → toggles like debug mode, log levels, or feature flags are heavily managed through environment variables

## Injecting Variables with `-e`

```bash
docker run -e MY_VAR=value httpd env
```

This command launches the **httpd** container with the variable **MY_VAR** set to **value**, then runs the `env` command to print all active environment variables — confirming the injection.

## Multiple Variables

You can pass several variables by repeating the **`-e`** flag:

```bash
docker run -e DB_HOST=localhost -e DB_PORT=5432 -e DB_NAME=mydb httpd
```

---

# 5. Interacting with Running Containers

## Definition

Docker provides several powerful commands to **interact with**, **inspect**, and **manage** containers that are already running — enabling deep access to the container's internal state.

## Executing Commands Inside a Container

The **`docker exec`** command runs a new process inside an already-running container. Combined with **`-it`**, it opens a fully interactive shell session:

```bash
docker exec -it <ContainerID> bash
```

To quickly inspect all environment variables inside a live container:

```bash
docker exec -it <ContainerID> env
```

## Copying Files Between Host and Container

The **`docker cp`** command transfers files bidirectionally between the **host filesystem** and the **container filesystem**:

```bash
docker cp <ContainerID>:/path/in/container /path/on/host
```

```bash
docker cp /path/on/host <ContainerID>:/path/in/container
```

## Attaching to a Running Container

The **`docker attach`** command connects your terminal directly to the **primary process** of a running container, allowing you to observe its standard output and interact with its standard input:

```bash
docker attach <ContainerID>
```

> **Note:** Attaching hooks into the main process — pressing `Ctrl+C` may terminate the container entirely, unlike `docker exec` which runs a separate process.

---

# 6. Key Summary

* **`docker run`** is the single most essential command — it **creates and starts** a container from an image in one step
* The **`-it`** flag provides interactive terminal access, while **`-d`** runs containers silently in the background
* **Port mapping** with **`-p`** is absolutely necessary to expose containerized services to the host network
* Containers are **network-isolated by default** — without publishing ports, no external traffic can reach them
* **Environment variables** (`-e`) offer a clean, code-free mechanism to configure application behavior at runtime
* **`docker exec`** runs commands inside live containers; **`docker cp`** handles file transfers; **`docker attach`** connects to the main process
* The **`--rm`** flag ensures containers are automatically cleaned up after exit, preventing resource buildup
* **`-v`** mounts persistent volumes, ensuring data survives container restarts and removals
