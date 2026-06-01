# Custom Docker Image Creation & Build Process Notes

---

# 1. What Is a Custom Docker Image?

## Definition

A **Custom Docker Image** is a **self-crafted, purpose-built container blueprint** that you construct from scratch using a **Dockerfile**. Rather than relying solely on pre-made images pulled from Docker Hub, you fundamentally define your own application environment — bundling your **source files**, **configuration**, and **runtime directives** into a single, portable image that can be deployed anywhere Docker runs.

*"Building a custom image means you own every layer of your container — from base OS to final application."*

---

# 2. Setting Up the Project Directory

## Definition

A **project directory** is a **dedicated workspace folder** that cleanly organizes all the files required for your Docker image build — including your application source code, server configuration, and the Dockerfile itself.

## Steps to Initialize

* **Create the project folder** → use `mkdir` to establish a fresh, isolated directory for the build context
* **Navigate into it** → move into the directory so all subsequent files are created in the correct location

```bash
mkdir nginx-app
cd nginx-app
```

*"Every well-structured Docker build begins with a clean, dedicated project directory — never scatter your build files across random locations."*

---

# 3. Crafting the Custom HTML Page

## Definition

The **index.html** file serves as the **primary web page** that your Nginx container will natively serve to users. This is the actual content visitors see when they hit your containerized web server.

## Creating index.html

* **Purpose** → acts as the front-facing content delivered by the Nginx web server inside the container
* **Location** → must reside in the root of your project directory alongside the Dockerfile

```html
<!DOCTYPE html>
<html>
<head>
    <title>Custom Docker Nginx App</title>
</head>
<body>
    <h1>Welcome to My Custom Docker Nginx App</h1>
    <p>This is a medium-level Docker project</p>
</body>
</html>
```

*"Your HTML file is the soul of the container — it's what the outside world actually interacts with once the image is running."*

---

# 4. Writing the Nginx Server Configuration

## Definition

The **default.conf** file is the **Nginx server block configuration** that explicitly dictates how the web server listens for incoming traffic, which hostname it responds to, and where it looks for the files to serve.

## Creating default.conf

* **listen 80** → instructs Nginx to actively listen on port 80 for all incoming HTTP requests
* **server_name localhost** → binds the server block to the localhost hostname
* **root /usr/share/nginx/html** → defines the document root directory where Nginx searches for web content
* **index index.html** → specifies `index.html` as the default file to serve when a directory is requested

```nginx
server {
    listen 80;
    server_name localhost;

    location / {
        root /usr/share/nginx/html;
        index index.html;
    }
}
```

*"Without a properly defined server configuration, your Nginx container has no idea how to route or serve traffic — this file is absolutely essential."*

---

# 5. Constructing the Dockerfile

## Definition

A **Dockerfile** is a **sequential instruction manifest** that Docker reads line-by-line to systematically assemble your custom image. Each instruction creates a new **layer** in the final image, stacking configurations on top of a chosen **base image**.

## Writing the Dockerfile

* **FROM nginx:alpine** → establishes the lightweight Alpine-based Nginx image as the foundational base layer
* **COPY index.html** → transfers your custom HTML page into the container's web root directory
* **COPY default.conf** → injects your server configuration into Nginx's configuration directory, overriding the default
* **EXPOSE 80** → formally declares that the container will be listening on port 80 at runtime

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html
COPY default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
```

### Important Note on File Naming

* **Dockerfile must have no extension** → if your system saves it as `Dockerfile.txt`, you must securely rename it before building

```bash
ren Dockerfile.txt Dockerfile
```

*"The Dockerfile is the architectural blueprint — Docker cannot build anything without this precise instruction set."*

---

# 6. Building the Custom Image

## Definition

The **docker build** command is the **image compilation step** where Docker reads your Dockerfile, executes each instruction sequentially, and produces a finalized, tagged image stored in your local image registry.

## Build Command

* **-t custom-nginx:v1** → assigns a human-readable name and version tag to the resulting image
* **.** (dot) → specifies the current directory as the **build context**, meaning Docker will look here for the Dockerfile and all referenced files

```bash
docker build -t custom-nginx:v1 .
```

## Build Flow Diagram

```
  Dockerfile (Instructions)
         ↓
  FROM nginx:alpine (Base Layer)
         ↓
  COPY index.html (Application Layer)
         ↓
  COPY default.conf (Configuration Layer)
         ↓
  EXPOSE 80 (Port Declaration)
         ↓
  docker build -t custom-nginx:v1 .
         ↓
  Custom Image: custom-nginx:v1 (Ready)
```

*"The build process heavily processes each instruction in order — every line in your Dockerfile translates directly into an immutable image layer."*

---

# 7. Running the Container from the Custom Image

## Definition

The **docker run** command **instantiates a live container** from your freshly built custom image, mapping host ports to container ports so the application becomes externally accessible.

## Run Command

* **-d** → launches the container in **detached mode**, running silently in the background
* **-p 8080:80** → maps port **8080** on the host machine to port **80** inside the container, enabling external access
* **--name nginx-container** → assigns a memorable, descriptive name to the running container instance
* **custom-nginx:v1** → specifies which image to use for spawning the container

```bash
docker run -d -p 8080:80 --name nginx-container custom-nginx:v1
```

## Accessing the Application

Once the container is running, your custom Nginx application is immediately accessible through any web browser:

```
http://localhost:8080
```

*"Port mapping is the bridge between your host system and the isolated container world — without it, nothing reaches your application."*

---

# 8. Complete Build & Deploy Flow

```
  mkdir nginx-app && cd nginx-app
              ↓
  Create index.html (Web Content)
              ↓
  Create default.conf (Server Config)
              ↓
  Create Dockerfile (Build Instructions)
              ↓
  docker build -t custom-nginx:v1 .
              ↓
  docker run -d -p 8080:80 --name nginx-container custom-nginx:v1
              ↓
  Access at http://localhost:8080
```

---

# 9. File Structure Overview

| File | Purpose | Destination in Container |
|------|---------|--------------------------|
| **index.html** | Custom web page content | `/usr/share/nginx/html/index.html` |
| **default.conf** | Nginx server block configuration | `/etc/nginx/conf.d/default.conf` |
| **Dockerfile** | Image build instruction manifest | Used only during build process |

---

# 10. Key Summary

* **Custom Docker images** are built by writing a **Dockerfile** that layers your files and configurations on top of a base image
* **Project setup** starts with creating a **dedicated directory** (`nginx-app`) to cleanly house all build-related files
* **index.html** provides the **actual web content** that Nginx serves to visitors hitting the container
* **default.conf** configures Nginx to **listen on port 80**, serve from the correct root, and use `index.html` as the default page
* **Dockerfile instructions** run sequentially — `FROM` sets the base, `COPY` transfers files, `EXPOSE` declares ports
* **docker build -t custom-nginx:v1 .** compiles everything into a **tagged, reusable image**
* **docker run -d -p 8080:80** launches the container in the background with **host-to-container port mapping**
* **http://localhost:8080** is where the running application becomes **immediately accessible**
* Always ensure the **Dockerfile has no file extension** — rename `Dockerfile.txt` to `Dockerfile` if your editor adds one automatically
* The entire workflow follows a **fundamentally linear pipeline**: create files → build image → run container → access application
