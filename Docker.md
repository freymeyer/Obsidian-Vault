---
tags:
  - infrastructure/containers
  - infrastructure/cloud
---

[[Container]]

uses the programming syntax [[YAML]] to allow developers to instruct how a container should be built and what is run.

Originally created by Solomon Hykes in 2013, Docker is open-source and has become a well-renowned name within containerisation.

Docker started as an internal project for dotCloud (a PaaS provider), where it was then showcased in PyCon in 2013 and then quickly made open-source.

While containerisation's original concepts started in 1979 with Unix V7, Docker has made containerisation a popular technology since its release in 2013. Docker’s popularity is due to making the benefits of containerisation accessible and modern.

As of April 2022, It is fair to say that Docker is extremely popular. To be precise:

- 13 million developers are using Docker _[1]_
- There are 7 million applications made and ready to use with Docker _[2]_
- 13 billion applications are downloaded monthly! _[3]_
- …and this is just from the official repository


### Docker is Free
The Docker ecosystem is free to use and open-sourced. While business plans exist, you can completely download, use, create, run and share images.

### Docker is Compatible
The Docker platform is compatible with Linux, macOS and Windows. Because of how containerisation works, if a device supports the Docker Engine, you can run any container, regardless of the application or dependencies.

### Docker is Efficient & Minimal
Docker is an efficient way to isolate applications in comparison to alternatives such as virtual machines. This is because the Docker Engine runs and interacts with the host operating system, and containers do not run a fully-fledged operating system for each container. For example, containers can share a minimal operating system image, meaning you only need to store it once.

A minimal Ubuntu image is 100MB~ which can be stored once and used multiple times. Compare this to the Ubuntu server image, about 1GB after a fresh install per VM.

Inspecting the size of the "ubuntu" docker image
```
ubuntu@thm:~$ docker image ls
```

### Docker is Easy to Get Started With
The Docker developer documentation is very well documented, with lots of articles, working examples and answered questions on the Internet. The chances are, if you want to do something in Docker, someone has already asked about or done it.

The syntax to get started with Docker is easy to pick up. You can start your first container in no time (the fact that there are docker images for all sorts of applications already published helps.) 

### Docker is Easy to Share With Others
A significant benefit of Docker is its portability. Docker uses “images” to store instructions to dictate how the container should be built (just an instruction manual!).

These “images” can be exported, shared and uploaded to both public and private repositories such as DockerHub and GitHub. The “image” can be run by anything that supports the Docker engine, as long as the syntax is valid.

### Docker is Minimal
These Docker images discussed above are minimal. You will often find many-core and luxurious tools and packages in a container that are missing. While this can look like a disadvantage, it, in fact, allows:
Containers to be built exactly how the developer wishes
Better security, knowing exactly what runs within a container can reduce the risk of unnecessary packages becoming vulnerable and posing a security risk.

### Docker is Cheaper to Run
Running containers is usually a cheaper option than running virtual machines. This is especially noticeable in cloud environments, where CPU, RAM, and Disk space are expensive. 

For example, you can quite happily run a few containers on a single $5 cloud provider VPS, whereas you will not be able to run a virtual machine. This is due to the fact that:
Running virtual machines requires hardware that supports virtualisation, which is only found on costly tiers of a cloud provider (if at all!)
Virtual machines require lots of memory and disk space, as you are running a separate operating system on top of the physical machine.

## Managing Docker Images

### Docker Pull
downloading the image
```
docker pull nginx
```

### Docker Image x/y/z 
list the available options
```
docker image
```

### Docker Image ls
list all images stored on the local system
```
Docker image ls
```

### Docker Image rm
remove an image from the system
```
docker image rm ubuntu:22.04
```

### Docker run
launched in 'interactive' mode
"Interactively" by providing the `-it` switch
```
docker run -it helloworld /bin/bash

docker run -it (interactive)
docker run -d (detached)
docker run -p 80:80 (bind port 80:80)
```

###  Docker ps
To list running containers, we can use the docker ps command
```
docker ps

docker ps -a (to list all)
```

## Dockerfiles
Dockerfiles are formatted in the following way: `INSTRUCTION argument`

1. Use the “Ubuntu” (version 22.04) operating system as the base.
2. Set the working directory to be the root of the container.
3. Create the text file “helloworld.txt”.
```
# THIS IS A COMMENT
# Use Ubuntu 22.04 as the base operating system of the container
FROM ubuntu:22.04

# Set the working directory to the root of the container
WORKDIR / 

# Create helloworld.txt
RUN touch helloworld.txt
```

### docker build
```
# Use Ubuntu 22.04 as the base operating system of the container
FROM ubuntu:22.04

# Set the working directory to the root of the container
WORKDIR / 

# Create helloworld.txt
RUN touch helloworld.txt
```
Docker starting to build the image
```
`docker build -t helloworld .` (-t will name the image)
```

1. Use Ubuntu 22.04 as the base operating system for the container.
2. Install the “apache2” web server.
3. Add some networking. As this is a web server, we will need to be able to connect to the container over the network somehow. I will achieve this by using the `EXPOSE` instruction and telling the container to expose port _80_.
4. Tell the container to start the “apache2” service at startup. Containers do not have service managers like `systemd` (this is by design - it is bad practice to run multiple applications in the same container. For example, this container is for the apache2 web server - and the apache2 web server only).

```
# THIS IS A COMMENT
FROM ubuntu:22.04

# Update the APT repository to ensure we get the latest version of apache2
RUN apt-get update -y 

# Install apache2
RUN apt-get install apache2 -y

# Tell the container to expose port 80 to allow us to connect to the web server
EXPOSE 80 

# Tell the container to run the apache2 service
CMD ["apache2ctl", "-D","FOREGROUND"]
```
command to build this would be 
```
docker run -d --name webserver -p 80:80  webserver
```
we can navigate to the IP address of our local machine

## Docker Compose

Docker Compose, in summary, allows multiple containers (or applications) to interact with each other when needed while running in isolation from one another.

More often than not, applications require additional services to run, which we cannot do in a single container. For example, modern - dynamic - websites use services such as databases and a web server. For the sake of this task, we will consider each application as a “[[Microservice]]”.

While we can spin up multiple containers or “microservices” individually and connect them, doing so one by one is cumbersome and inefficient. Docker Compose allows us to create these “microservices” as one singular “service”. 

This illustration shows how containers are deployed together using Docker Compose Vs. Docker:
![A blue box (representing a computer) with a caption of docker, is isolated from another set of blue boxes (representing a computer).](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/c7c8b38dc06c22207134fb6f58d036a0.png)


### Essential Docker Compose commands

```
docker-compose up - (re)create/build and start the containers specified in the compose file.
docker-compose start - start (but requires the containers already being built) the containers specified in the compose file.
docker-compose down - stop and **delete** the containers specified in the compose file.
docker-compose stop - stop (**not** delete) the containers specified in the compose file.
docker-compose build - build (but will not start) the containers specified in the compose file.
```

This _docker-compose.yml_ file assumes the following:
1. We will run one web server (named web) from the previously mentioned scenario.
2. We will run a database server (named database) from the previously mentioned scenario.
3. The web server is going to be built using its Dockerfile, but we are going to use an already-built image for the database server (MySQL)
4. The containers will be networked to communicate with each other (the network is called ecommerce).
5. Our directory listing looks like the following:
6. docker-compose.yml
7. web/Dockerfile

```
version: '3.3'
services:
  web:
    build: ./web
    networks:
      - ecommerce
    ports:
      - '80:80'


  database:
    image: mysql:latest
    networks:
      - ecommerce
    environment:
      - MYSQL_DATABASE=ecommerce
      - MYSQL_USERNAME=root
      - MYSQL_ROOT_PASSWORD=helloword
    
networks:
  ecommerce:
```

## Docker Socket
When you install Docker, there are two programs that get installed:
1. The Docker Client
2. The Docker Server

Docker works in a client/server model. These two programs communicate with each other to form the Docker. Docker achieves this communication using something called a socket. Sockets are an essential feature of the operating system that allows data to be communicated.

For example, when using a chat program, there could be two sockets:
1. A socket for storing a message that you are sending
2. A socket for storing a message that someone is sending you.

The program will interact with these two sockets to store or retrieve the data within them! A socket can either be a network connection or what is represented as a file. What's important to know about sockets is that they allow for [[Interprocess Communication (IPC)]]

In the context of Docker, the Docker Server is effectively just an [[Application Programming Interface (API)]]. The Docker Server uses this API to **listen** for requests, whereas the Docker Client uses the API to **send** requests.

![[Pasted image 20251207162526.png]]
