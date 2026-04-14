# Docker Network Operations Notes

---

# 1. The Need For Custom Networks

By default, Docker containers run in heavily isolated internal networks.

Applications inside these containers are **NOT** naturally accessible from the host system securely, unless explicitly published. Additionally, containers cannot discover each other by domain name easily on the default system network.

---

# 2. Creating a Custom Network

```bash
docker network create mynetwork
```

* Provisions a completely custom, user-defined bridge network.
* Allows seamless, domain-name-driven communication between multiple independent containers.

---

# 3. Inspecting a Target Network

```bash
docker network inspect mynetwork
```

Outputs complex JSON details about the internal structure of the network, definitively showcasing its underlying subnet architecture and precisely which active containers are bridged to it.

---

# 4. Removing a Network

```bash
docker network rm mynetwork
```

* Safely dismantles the custom network interface natively.
* Requires that all currently attached containers be either completely stopped or explicitly disconnected beforehand.

---

# 5. Attaching Containers to Networks

```bash
docker run -d --name secure_db --network mynetwork mysql
```

* Utilizing the `--network` override explicitly isolates the newly instantiated container directly into the exact network provided, bypassing standard defaults securely.

---
