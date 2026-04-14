# Advanced Container Interaction & Variables

---

# 1. Checking Software Deployment Versions

```bash
docker --version
```

Instantly prints the specific kernel release version of the active Docker engine background service (e.g., `Docker version 18.09.6`), useful for verifying deep OS compatibility.

---

# 2. Injecting Commands (bash -c)

Instead of manually navigating into a container shell space to type commands, you can inject deep structural changes identically from the host base.

```bash
docker exec -it my-apache bash -c "echo 'Hello World' > htdocs/index.html"
```

* Modifies an active file immediately within the Apache web structure.
* Entirely bypasses entering an active `/bin/bash` terminal manually.

---

# 3. Injecting Host Environment Variables

If your core machine possesses an active environment variable, you easily inject it directly to new containers cleanly without hardcoding paths.

```bash
export APP_ENV=production
docker run -e APP_ENV=$APP_ENV nginx
```

* Dynamically binds host-layer variables directly into the application space natively.

---

# 4. Validating Servers via Terminal CLI

```bash
curl http://localhost:8080
```

Allows instantaneous terminal validation verifying if the isolated container web interface responds securely without triggering an actual visual desktop browser sequence.

---
