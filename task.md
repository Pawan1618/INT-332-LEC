# Apache Docker Task

---

## 🔹 1. Pull Apache (httpd) Image

docker pull httpd

📸 Output:

![Step 1](https://raw.githubusercontent.com/Pawan1618/INT-332-LEC/main/images/docker%20pull.png)

---

## 🔹 2. Verify Image Downloaded

docker images

👉 Look for:

REPOSITORY   TAG     IMAGE ID  
httpd        latest  xxxx  

📸 Output:

![Step 2](https://raw.githubusercontent.com/Pawan1618/INT-332-LEC/main/images/verify.png)

---

## 🔹 3. Run Container with Name + Port Mapping

docker run -d -p 8081:80 --name myapache httpd

📸 Output:

![Step 3](https://raw.githubusercontent.com/Pawan1618/INT-332-LEC/main/images/exec.png)

---

## 🔹 4. Check Running Containers

docker ps

👉 You should see:

CONTAINER ID   IMAGE   PORTS  
xxxx           httpd   0.0.0.0:8081->80/tcp  

📸 Output:

![Step 4](https://raw.githubusercontent.com/Pawan1618/INT-332-LEC/main/images/verify.png)

---

## 🔹 5. Access Apache in Browser

http://localhost:8081

👉 You should see:

It works!

📸 Output:

![Final](https://raw.githubusercontent.com/Pawan1618/INT-332-LEC/main/images/apacheOn.png)

---

## ✅ Task Completed

![Completed](https://raw.githubusercontent.com/Pawan1618/INT-332-LEC/main/images/changed.png)
