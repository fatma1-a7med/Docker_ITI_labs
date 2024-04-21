# ITI - Docker Lab 1 🐋

## Task 1: Working with Docker Hello-world Image
### Objective
Learn how to run a container using the hello-world image and manage containers and images.

### Steps
#### 1. Run a Container with hello-world Image
       
```bash
docker run hello-world
docker pull hello-world

```
#### 2. Check Container Status and Explain
```bash
docker ps -a
The container status is "Exited (0)" ->  The reason for this status is that the hello-world container is a very simple container that is designed to just print a message ("Hello from Docker!") to confirm that your Docker installation is working correctly. After printing this message, the container completes its task and exits 
```
#### 3. Start the Stopped Container
```bash
docker start hello-world
```
#### 4. Remove the Container
```bash
docker rm trusting_northcutt
```
#### 5. Remove the Image
```bash
docker rmi hello-world
```
---

## Task 2: Running Container with Ubuntu Image
### Objective
Run an Ubuntu container in interactive mode, create a file inside it, and manage containers.

### Steps
#### 1. Run Ubuntu Container in Interactive Mode
```bash
docker pull ubuntu
docker run -it ubuntu
```
#### 2. Create a File inside the Container
```bash
touch hello-docker
```
#### 3. Stop and Remove the Container
```bash
exit
docker stop zealous_roentgen
docker rm zealous_roentgen
```
#### 4. Check File Status
```bash
removed
```
#### 5. What happened to hello-docker file?
```bash
Docker removes the container along with all its associated files and changes, including the "hello-docker" file
```
#### 6. Remove All Stopped Containers
```bash

docker rm -f $(docker ps -aq)
```
#### 7. Bonus: Remove All Containers in One Command
```bash
docker container prune
```

---
## Task 3: Creating a Custom Nginx Docker Image
### Objective
Create a custom Docker image using Nginx and a local HTML file.

### Steps
#### 1. Create a Local HTML File
```bash
vi index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Name</title>
</head>
<body>
  <h1>Fatma Alzahraa Ahmed Fathy</h1>
</body>
</html>
```
#### 2. Write Dockerfile and Copy the HTML file to the Docker Image
```bash
vi  Dockerfile :
           FROM nginx:alpine
           COPY index.html /usr/share/nginx/html/index.html
           EXPOSE 80
```
#### 3. Run Container with New Image
```bash
docker build -t nginx-custom .
```

#### 4. Test the Container, open your browser and navigate to http://localhost:8088 to check if everything is okay
```bash
docker run -d -p 8080:80 nginx-custom
docker ps -a
(add port 8080 and get link and will work )
```





# ITI - Docker Lab 2 🐋

## Task 1: Run a container using nginx image, and mount a directory from your host into the Docker container. (bind mount)
##Steps
1. Create Bind Mount Directory
```bash
mkdir bindMount_dir
cd bindMount_dir

```
2. Run a container using nginx image

```bash
docker run -d --name bindMount_dir -v /root/bindMount_dir:/user/share/nginx/html nginx
docker exec -it bindMount_dir bash

```
3. Echo any content to show when curl ip-address

```bash
cd /user/share/nginx/html
echo "Hello from Bind Mount Nginx" > index.html
exit
docker inspect -f '{{.NetworkSettings.IPAddress}}' bindMount_dir    ->172.17.0.2
curl 172.17.0.2

```

## Task 2: Create 2 docker network (net-1 & net-2)
## Steps

1. Create 2 docker network (net-1 & net-2)

```bash
docker network create network_1
docker network create network_2

```
2. Run 2 new containers using nginx:alpine image, and attach the net-1 to them.

```bash
docker run -d --name nginx_net1 --net network_1 nginx:alpine
docker run -d --name nginx_net2 --network network_1 nginx:alpine

```
3. Run another 1 new containers using nginx:alpine image, and attach the net-2 to them.

```bash

docker run -d --name nginx_net3 --net network_2 nginx:alpine

```
4. Inspect the 3 containers to know their IPs and write them aside.

```bash

docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_net1
172.18.0.2

docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_net2
172.18.0.3

docker inspect -f '{{.NetworkSettings.Networks.network_2.IPAddress}}' nginx_net3
172.20.0.2
```
5. Enter a container in the net-1 network and try to ping a container in the net-2 network (What do you notice?)

```bash
docker exec -it nginx_net1 sh 
Use ping or curl
ping 172.20.0.2
ping fails, because the two containers exist in different networks so we can see any response in terminal.

```

6. Enter a container in the net-1 network and try to ping the other container in the same network (What do you notice?)

```bash

docker exec -it nginx_net1 sh 
curl 172.18.0.2
ping 172.18.0.3
ping successful and return response, because the two containers exist in the same network.

```


Task 3: Explain the difference between Docker volumes and Bind Mount.

*Docker Volumes*


Volumes are stored within the Docker environment, typically in a directory on the host filesystem, but the location is managed by Docker and not directly accessible or configurable by the user.

*Docker Bind Mounts*


Bind mounts give you more control over where the data is stored and how it's accessed because you can specify any path on the host machine.






