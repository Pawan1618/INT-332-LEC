# Advanced Run Options & Deep Cleanup Notes

---

# 1. Ephemeral Containers (--rm)

```bash
docker run --rm -it ubuntu bash
```

* Automatically deletes the container from the entire infrastructure the precise moment the execution terminates perfectly.
* Exceptionally effective for testing isolated sandbox features safely.

---

# 2. Native Physical Path Mounting (-v)

While internal volumes natively are centrally managed by Docker daemons, you easily bind specific physical pathways natively securely.

```bash
mkdir C_Drive_Data
docker run -v C_Drive_Data:/app/data ubuntu
```

* Forcefully substitutes explicit target directories natively.
* Changes executing physically strictly mirror deeply active containers.

---

# 3. Destroying Images via Distinct IDs

```bash
docker rmi fce289e99eb9
```

Instead of generically tracking standard tags natively, developers absolutely specify destruction cleanly heavily utilizing exact cryptographic `IMAGE ID` signatures.

---

# 4. Safe Shell Detachment 

```bash
exit
```

* Properly detaches heavy interaction loops safely.
* Gracefully yields strict kernel focus seamlessly reverting back fully to active host operations naturally.

---

# 5. Total Nuclear Cleanup Operations

```bash
docker system prune -a --volumes
```

* `-a` clears every unused detached standard image firmly.
* `--volumes` perfectly targets and securely terminates orphaned persistence sectors globally.
* Result: A perfectly sterile slate perfectly mirroring a fresh installation cleanly.

---
