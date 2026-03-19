# DevOps Container Image Notes

---

# 1. Container Image

## Definition

A **container image** is a **read-only blueprint used to create containers**.

It contains everything required to run an application in a consistent environment.

---

# 2. What a Container Image Contains

A container image includes:

* **Base OS (lightweight)**
* **Application code**
* **Libraries and dependencies**
* **Runtime environment**
* **Configuration files**

---

# 3. Key Characteristics of Container Images

## 3.1 Read-Only

Container images are **read-only**.

Once built, they **cannot be modified directly**.

---

## 3.2 Immutable

### Definition

**Immutability** means once an image is created, it **does not change**.

To make changes:

* you create a **new image**
* instead of modifying the existing one

---

## 3.3 Portable

Images can run on:

* local machine
* cloud
* any system with a container runtime

---

## 3.4 Consistent Environment

Ensures:

* same behavior in dev, test, and production
* eliminates "it works on my machine" problem

---

# 4. Example of a Container Image

Example:

```id="p8zq2f"
python:3.11-slim
```

This image includes:

* lightweight Linux OS
* Python 3.11 runtime
* standard Python libraries

---

# 5. Image vs Container

| Image         | Container                 |
| ------------- | ------------------------- |
| Blueprint     | Running instance          |
| Read-only     | Read + Write              |
| Static        | Dynamic                   |
| Cannot change | Can change during runtime |

---

# 6. Image Layers

## Definition

A container image is built as a **stack of layers**.

Each layer represents a **change or instruction**.

---

## How Layers Are Created

Each instruction in a Dockerfile creates **one layer**.

Example:

```id="7u4j1m"
FROM python:3.11-slim
COPY . /app
RUN pip install -r requirements.txt
```

This creates multiple layers:

1. Base image layer
2. Copy files layer
3. Install dependencies layer

---

# 7. Characteristics of Image Layers

## 7.1 Immutable

Each layer is **read-only** and cannot be changed.

---

## 7.2 Cached

Layers are cached to:

* speed up builds
* avoid rebuilding unchanged layers

---

## 7.3 Reusable

Layers can be reused across images.

Example:

* Multiple images using `python:3.11-slim` share the same base layer

---

# 8. Layered Architecture (Concept)

```id="7q8gzt"
Application Layer
-----------------
Dependencies Layer
-----------------
Runtime Layer
-----------------
Base OS Layer
```

Each layer builds on top of the previous one.

---

# 9. Why Layers Are Important

* Faster image builds (due to caching)
* Efficient storage (shared layers)
* Reduced network transfer
* Better version control

---

# 10. Dockerfile and Image Creation

## Definition

A **Dockerfile** is a script containing instructions to build an image.

Each instruction creates a new layer.

---

## Example Flow

```id="v2kq4x"
Dockerfile
   ↓
Build Image
   ↓
Image Layers Created
   ↓
Image Stored
```

---

# 11. Interview Questions

## Q1: What is a container image?

A container image is a **read-only blueprint that contains everything needed to run an application**.

---

## Q2: Why are images immutable?

Images are immutable to ensure:

* consistency
* reproducibility
* stability

---

## Q3: What are image layers?

Image layers are **stacked read-only layers created from Dockerfile instructions**.

---

## Q4: Why are layers useful?

Layers:

* improve build speed (caching)
* reduce storage (sharing)
* enable reuse across images

---

# 12. Key Summary

* Container image = **blueprint**
* Container = **running instance**
* Images are **immutable and read-only**
* Built using **Dockerfile**
* Composed of **multiple reusable layers**

---
