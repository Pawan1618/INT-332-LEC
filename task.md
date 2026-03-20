🔹 1. Pull Apache (httpd) image
docker pull httpd
🔹 2. Verify image downloaded
docker images

👉 Look for:

REPOSITORY   TAG     IMAGE ID
httpd        latest  xxxx
🔹 3. Run container with name + port mapping
docker run -d -p 8081:80 --name myapache httpd
🔹 4. Check running containers
docker ps

👉 You should see:

CONTAINER ID   IMAGE   PORTS
xxxx           httpd   0.0.0.0:8081->80/tcp
🔹 5. Access Apache in browser

Open:

http://localhost:8081

👉 You should see:

It works!
pic here for task
