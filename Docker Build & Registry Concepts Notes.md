# Docker Build & Registry Concepts Notes

---

# 1. The Docker Build Execution Phase

To forge a custom container image natively, you execute the build process leveraging an active `Dockerfile` blueprint.

```bash
docker build -t my-app:v1.0 .
```

* `-t my-app:v1.0`: Assigns a specific human-readable **name** (`my-app`) and version **tag** (`v1.0`).
* `.` (The Dot): Represents the **Build Context**, commanding the builder to analyze the current working directory for necessary files cleanly.

---

# 2. How Images Are Structured During Build

For absolutely every single structural command within the `Dockerfile` (e.g., `FROM`, `COPY`, `RUN`), the builder engine compiles a highly distinct, read-only immutable layer.

If a layer's foundational data remains unmodified during subsequent rebuilds, the builder intelligently recycles the previously cached layer, massively decreasing compilation times dynamically.

---

# 3. The Core Concept of an Image Registry

An **Image Registry** acts securely as a vast centralized repository architecture utilized specifically to securely store, maintain, and distribute final container images across entire developer teams globally.

## The Distinction: Registry vs. Repository
* **Registry**: The broad foundational hosting service globally (e.g., Docker Hub, AWS ECR, Azure ACR).
* **Repository**: A specialized folder housing related images directly within a Registry natively (e.g., all tagged versions of `payment-service`).

---

# 4. Main Registry Classifications

* **Public Registries**: Foundational systems like the public Docker Hub freely supplying highly reliable, officially maintained foundations like `nginx` or `ubuntu` totally transparently.
* **Private Registries**: Heavily encrypted, authentication-gated environments intensely utilized by enterprise infrastructure to securely shield proprietary company assets heavily.

---

# 5. Standardized Naming Convention Requirements

To successfully upload an image gracefully, developers must strictly adhere to specific architectural naming paths seamlessly:

```
[REGISTRY HOST]/[NAMESPACE]/[IMAGE_NAME]:[TAG]
```

*Example:* `docker.io/pawan1618/payment-service:v2.1`

---

# 6. The Registry's Role in DevOps (CI/CD)

Within modern automated pipelines securely, the underlying Registry strictly acts as the definitive central bridge safely connecting code integration deeply to deployment safely.

1. **Build**: Commits naturally trigger automatic `docker build` processes natively.
2. **Ship**: The automated CI systems firmly issue `docker push` storing the encrypted images tightly within the secure Registry.
3. **Run**: Distinct live production servers automatically initiate `docker pull` consuming the finished artifacts cleanly.

---
