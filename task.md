# Apache Docker Task

---

## 🔹 1. Pull Apache (httpd) Image

```bash
docker pull httpd
📸 Output:

![Step 1](images/docker pull.png)

🔹 2. Verify Image Downloaded
docker images

👉 Look for:

REPOSITORY   TAG     IMAGE ID
httpd        latest  xxxx
📸 Output:
![Step 2](images/verify.png)

🔹 3. Run Container with Name + Port Mapping
docker run -d -p 8081:80 --name myapache httpd
📸 Output:
![Step 3](images/exec.png)

🔹 4. Check Running Containers
docker ps

👉 You should see:

CONTAINER ID   IMAGE   PORTS
xxxx           httpd   0.0.0.0:8081->80/tcp
📸 Output:
![Step 4](images/verify.png)

🔹 5. Access Apache in Browser

Open in browser:

http://localhost:8081

👉 You should see:


It works!
📸 Output:
![final](images/apacheOn.png)

✅ Task Completed
![complted](images/changed.png)
