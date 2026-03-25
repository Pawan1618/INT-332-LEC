# 📅 March 25 - Docker Notes (Single Continuous File)

## Run MySQL Container
docker run -d -p 3307:3306 --name mysql-test -e MYSQL_ROOT_PASSWORD=root123 mysql
OUTPUT:
<container_id>

## Access Container
docker exec -it mysql-test bash
OUTPUT:
root@<container_id>:/#

## Connect to MySQL
mysql -h 127.0.0.1 -P 3306 -u root -p
OUTPUT:
Enter password:
mysql>

## List Containers
docker ps
OUTPUT:
CONTAINER ID   IMAGE   STATUS   PORTS           NAMES
abc123         mysql   Up       0.0.0.0:3307    mysql-test

## docker ps vs docker top
docker top mysql-test
OUTPUT:
UID   PID   PPID   CMD
root  ...   ...    mysqld

INFO:
docker ps  → containers info
docker top → processes inside container

## Task

Run Ubuntu with ENV
docker run -it -e college=cse ubuntu bash
OUTPUT:
root@<container_id>:/#

Print ENV
echo $college
OUTPUT:
cse

Stop Container
docker stop <container_id>
OUTPUT:
<container_id>

Check After Stop
docker ps
OUTPUT:
(no output)

docker ps -a
OUTPUT:
<container_id>   ubuntu   Exited

## Remove Image
docker rmi -f <image_id>
OUTPUT:
Deleted: ...

## Notes
docker rm → only works on stopped containers
docker rm -f → force remove running container
docker rmi -f → force remove image

✔️ Your understanding is correct
