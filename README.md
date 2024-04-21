# docker_lab2
lab2
# ITI - Docker Lab Two🐋
## Task 1: Run a container using nginx image, and mount a directory from your host into the Docker container.
example: /home/samy/nginx:/home/nginx (bind mount)

### Steps
#### 1. Create Bind Mount Directory
```bash
mkdir file_mount_dir

```

#### 2. Run a container using nginx image
```bash
docker run -d --name file_mount_dir -v /root/file_mount_dir:/user/share/nginx/html nginx
docker exec -it file_mount_dir bash
```

#### 3. Echo any content to show when curl ip-address
```bash
cd /user/share/nginx/html
echo "Hello iam nyraa" > home.html
exit
docker inspect -f '{{.NetworkSettings.IPAddress}}' file_mount_dir   ->172.17.0.8
curl 172.17.0.8
```

---
## Task 2: Create 2 docker network (net-1 & net-2)

### Steps
#### 1. Create 2 docker network (net-1 & net-2)
```bash
docker network create net_work1
docker network create net_work2
```

#### 2. Run 2 new containers using nginx:alpine image, and attach the net-1 to them.
```bash
docker run -d --name nginx_net_work1 --net net_work1 nginx:alpine
docker run -d --name nginx_net_work2 --net net_work2 nginx:alpine
```

#### 3. Run another 1 new containers using nginx:alpine image, and attach the net-2 to them.
```bash
docker run -d --name nginx_net_work3 --net net_work2 nginx:alpine
```

#### 4. Inspect the 3 containers to know their IPs and write them aside.
```bash
docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_net_work1
172.18.0.2

docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_net_work2
172.18.0.3

docker inspect -f '{{.NetworkSettings.Networks.network_2.IPAddress}}' nginx_net_work3
172.20.0.2
 docker inspect nginx_net_work1 | grep IPAddress
```

#### 5. Enter a container in the net-1 network and try to ping a container in the net-2 network (What do you notice?)
```bash
docker exec -it nginx_net_work1 sh 
Use ping or curl
ping 172.20.0.2
ping it faild because it in the same network
```

#### 6. Enter a container in the net-1 network and try to ping the other container in the same network (What do you notice?)
```bash
docker exec -it nginx_net_work1 sh 
curl 172.18.0.2
ping 172.18.0.3
ping successfully working!! because it in the same network
```

## Task 3: Explain the difference between Docker volumes and Bind Mount.
Docker volumes are like organized storage spaces managed by Docker itself. They're great for keeping important data safe and portable between different Docker setups, making them perfect for serious projects.
Bind mounts, on the other hand, directly connect folders or files from your computer to your Docker container. They're handy for quick access to files during development or testing, but they're less portable since they rely on the specific setup of your computer.
