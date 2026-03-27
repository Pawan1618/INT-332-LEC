# 📘 Docker Practice Notes

**Date:** 27 March

```bash
# Modify Apache HTML inside container
docker exec -it testapache bash -c "echo '<h1>hello from docker </h1>'> /usr/local/apache2/htdocs/index.html"

# Check output
curl http://localhost:8080
<h1>hello from docker </h1>

# Stop container
docker stop myapache
myapache

# Remove container
docker rm myapache
myapache

# Run container with env + volume + port
docker run -it -p 8080:80 -e APP_ENV=production -v /app/data:/data --name my_app httpd

# Verify environment variable
docker run -it --name my_app -e APP_ENV=production -v /app/data:/data ubuntu
echo $APP_ENV
production

# Unpause container
docker unpause <container_id_or_name>

# Kill container
docker kill <container_id_or_name>

# Remove container
docker rm <container_id_or_name>

# Remove all stopped containers
docker container prune
Deleted Containers:
<container_id>
Total reclaimed space: XXMB
```
