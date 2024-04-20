# ITI - Docker Lab 🐋

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

