# Table of Contents
1. [Introduction to Docker](#1-introduction-to-docker)
	* [What is container](#1-introduction-to-docker)
	* [Pre Reqs](#pre-reqs)
	* [Check your Enviornment](#check-your-enviornment)
2. [Running Hello World Container](#2-running-hello-world-container)
3. [Dockerfile](#3-dockerfile)
4. [Docker Multi Stage Buids](4-docker-multi-stage-builds)
	* [Example one using Maven](#41a-example-one-mvn)
	* [Example two using Go Lang](#41b-example-two-go-lang)
5. [Docker Hub and running Pre Built Images](#5-docker-hub-and-running-pre-built-images)
	* [Running HTTPD Server](#5-docker-hub-and-running-pre-built-images)
	* [Running Static website](#5-docker-hub-and-running-pre-built-images)
	* [Deploying Node.js Application](#5-docker-hub-and-running-pre-built-images)
	* [Creating a Tomcat server](#4-creating-tomcat-server-in-docker-container)
	* [Running jenkins in docker via docker compose](#5-running-jenkins-as-container)
	* [Running MYSQL in container via docker-compose](#6-running-mysql-in-docker)
	* [install AWS CLI and mysql Client on docker](#7-install-aws-cli-and-mysql-client-on-docker)
6. [Data Containers](#6-data-containers)
7. [Managin Data in Docker](#7-managing-data-in-docker)
	* [Docker Bind Mounts](#1-docker-bind-mounts)
	* [Docker tmpds](#2-docker-tmpds)
	* [Docker Volumes](#3-docker-volumes)
		* [Creating Vol and binding with a directory on host](#1-example--create-a-volume-on-host-and-place-a-static-website-bind-that-volume-with-nginx-and-run-the-contianer-using-below-command)
		* [Shared Volumes between containers](#2-copying-files-between-containers-from-a-shared-volume)
8. [Docker Networking](#8-docker-networking)
	* [Overlay Networking](#1-overlay-network)
	* [Host Networking](#2-host-networking)
	* [Macvlan Networking](#3-macvlan-networking)
	* [Bridge Networking](#4-bridge-network)
		* [se default Bridge Network](#use-the-default-bridge-network)
		* [Use user-defined Bridge Network](#use-user-defined-bridge-networks)
		* [Communicating between containers using Links](#communicating-between-containers-using-links)

9. [Sharing Images on Docker Hub](#9-share-your-images-on-docker-hub)
10. [Useful commands](#10-useful-commands)
11. Docker Book For Virtulization Admins
12. Monitor Docker Container 
13. Getting a Shell in Container 

# Get Docker
You can either follow docker documentation for downloading docker or get it from 'get.docker.com' whcih we will do, this is a better way to get docker:

goto https://get.docker.com/

and run below commands in your terminal
```
# This script is meant for quick & easy install via:
#   $ curl -fsSL https://get.docker.com -o get-docker.sh
#   $ sh get-docker.sh
```
once done, run docker version
```
docker version
Client: Docker Engine - Community
 Version:           19.03.11
 API version:       1.40
 Go version:        go1.13.10
 Git commit:        42e35e61f3
 Built:             Mon Jun  1 09:12:22 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.11
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.10
  Git commit:       42e35e61f3
  Built:            Mon Jun  1 09:10:54 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
 
 Adding a user to the "docker" group will grant the ability to run
 containers which can be used to obtain root privileges on the
 docker host
```

add your user to docker 
```
sudo groupadd docker
sudo usermod -aG docker $USER
```

# Get Docker Machine 

install docker machine by going to below link

https://docs.docker.com/machine/install-machine/

```
$ base=https://github.com/docker/machine/releases/download/v0.16.0 &&
  curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine &&
  sudo mv /tmp/docker-machine /usr/local/bin/docker-machine &&
  chmod +x /usr/local/bin/docker-machine
  
jawad@DockerLab:~$ docker-machine version
docker-machine version 0.16.0, build 702c267f

```

# Get Docker Compose

https://docs.docker.com/compose/install/

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

$docker-compose version
docker-compose version 1.26.0, build d4451659
docker-py version: 4.2.1
CPython version: 3.7.7
OpenSSL version: OpenSSL 1.1.0l  10 Sep 2019

```
alternatively you can get the release from github
https://github.com/docker/compose/releases

# 1. Introduction to Docker 

## What is a Container
* [What is Container](https://www.docker.com/resources/what-container)

##  Pre Reqs
* [Installing Docker Enginer](https://docs.docker.com/engine/install/) See the official docker website for installing docker
* If you are running docker from a non root user, you shpuld add your user to `docker` group with below command:
```
sudo usermod -aG docker your-user
```

## Check Your Enviornment
*  List Version of Docker ```docker --version```
Docker version 19.03.5, build 633a0ea

# 2. Running Hello World Container

1. Testing that our installation was successful by running Docker Hello-World Image from docker hub.

```sh
docker run hello-world

    Unable to find image 'hello-world:latest' locally
    latest: Pulling from library/hello-world
    ca4f61b1923c: Pull complete
    Digest: sha256:ca0eeb6fb05351dfc8759c20733c91def84cb8007aa89a5bf606bc8b315b9fc7
    Status: Downloaded newer image for hello-world:latest

    Hello from Docker!
    This message shows that your installation appears to be working correctly.
    ...
```

2. Run `docker image ls` to list the hello-world image that we downloaded on our machine.
3. List hello-world container by running below command. add `--all` if container is stopped.
```
docker ps
  CONTAINER ID     IMAGE           COMMAND      CREATED            STATUS
  54f4984ed6a8     hello-world     "/hello"     20 seconds ago     Exited (0) 19 seconds ago
```
## Running a Simple NGINX Server
```
docker container run --publish 80:80 --name webhost -d nginx -T
```
## run mongo DB
```
docker run --name mongo -d mongo
```

## install mysql 
```
docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql
```
get root password from logs
```
docker cotainer logs db

2020-06-30 17:55:47+00:00 [Note] [Entrypoint]: GENERATED ROOT PASSWORD: mieM6engool7ieho7uiyeet0ziewaede
```

## install httpd
```
docker container run -d --name webserver -p 8080:80 httpd
```

## install nginx
```
docker container run -d --name proxy -p 80:80 nginx
```

## install alpine
```
docker container run -d --name alpineServer -p 8081:80 alpine

#to sh into new container
docker container run -it alpine sh
```
# 3. Dockerfile 

Docker builds images automatically by reading the instructions from a Dockerfile -- a text file that contains all commands, in order, needed to build a given image. A Dockerfile adheres to a specific format and set of instructions which you can find at Dockerfile reference.

A Docker image consists of read-only layers each of which represents a Dockerfile instruction. The layers are stacked and each one is a delta of the changes from the previous layer. Consider this Dockerfile:

```
FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.py
Each instruction creates one layer:
```

FROM creates a layer from the ubuntu:18.04 Docker image.
COPY adds files from your Docker client’s current directory.
RUN builds your application with make.
CMD specifies what command to run within the container.

# 4 Docker Multi Stage Builds

With multi-stage builds, you use multiple FROM statements in your Dockerfile. Each FROM instruction can use a different base, and each of them begins a new stage of the build. You can selectively copy artifacts from one stage to another, leaving behind everything you don’t want in the final image. To show how this works, let’s adapt the Dockerfile from the previous section to use multi-stage builds
#### 4.1.a. Example one Mvn
[MVN example Multi Stage Build](https://www.youtube.com/watch?v=U9p8zrYxsZw&list=PL8byALqxKKoK8Yi8lrC9J15MJ-Pye3tMb&index=8&t=0s)
[Another Example](https://www.youtube.com/watch?v=gdoXtFpXvik&list=PL8byALqxKKoK8Yi8lrC9J15MJ-Pye3tMb&index=3&t=0s)

#### 4.1.b. Example two Go Lang
`Dockerfile:`
```
FROM golang:alpine

WORKDIR /go/src/app

COPY main.go .

RUN go build -o webserver .

CMd ["./webserver"]
```

`main.go file:`

```
package main

import (
    "fmt"
    "log"
    "net/http"
)

func handler (w http.ResponseWriter, r *http.Request){
    fmt.Fprintf(w, "Hellow from webserver")
}

func main(){
	http.HandleFunc("/", handler)
	log.Fatal(http.ListenAndServe("0.0.0.0:8080", nil))
}
```
Build the Docker file, you will see your image `webserver` to be around `370MB` which is huge:

```
docker build -t webserver .
```

run the container 
```
docker run -p "8080:8080" webserver 
```

Now using multi stage build we will try and reduce the size, DockerCompose

```

FROM golang:alpine AS builder

WORKDIR /go/src/app
COPY main.go .

RUN go build -o webserver .

FROM alpine
WORKDIR /app
COPY --from=builder /go/src/app /app
CMD ["./webserver"]
```

build and run the container

```
docker build -t webserver .
docker run -p "8080:8080" webserver
```

you can see the image size reduced to `13MB`

```
docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
webserver           latest              e5d26b3f6a60        53 seconds ago      13.1MB
<none>              <none>              c81abb7b536c        56 seconds ago      377MB
```
# 5. Docker Hub and running Pre built images

The best feature of docker is you can download and use 1000's of pre built images available to you with a single command e.g. wordrpress, ubuntu, jenkins, node.js etc.
we will use some of the images and run them to get you started:

### 1. Running HTTPD Server

```
docker run --name apache -p 8080:80 httpd:2.4

or 

docker run -d --name apache2 -p 8082:80 httpd:2.4
```

To get more info about a container
```
docker logs apache2
docker stats apache2
```

**go in your container and start playing with it**

In the container, we'll be root right off and be sitting in the `/usr/local/apache2 directory`. Run a quick `ls` to see what's here, and then cd htdocs to get into where the website's index.html page is.

```
docker exec -it apache2 bash
cd htdocs/
touch jawad.html
echo 'hello jawad how are you?' > jawad.html
exit
```

now if you will goto `localhost:8082/jawad.html` you will see the new file.


### 2. Running a static website in docker

lets say we have a directory `mySite` and it has two files one `HTML` other `CSS` and we want to run them on a webserver i.e. `nginx`:

```
jawad@DockerLab:~/images/mySite$ ls
  index.html  mystyle.css
jawad@DockerLab:~/images/mySite$ pwd
/home/jawad/images/mySite
```

Create your docker file for building your image, first line defines the base image and second one copies our content of current directory to a particular directory

```
gedit Dockerfile

FROM nginx:alpine
COPY . /usr/share/nginx/html
```

build the image and name it `webserver-image:v1`

```
docker build -t webserver-image:v1 .
docker images
```

run the docker container using our image 
```
docker run -d -p 8084:80 webserver-image:v1
```

you will see your static website up and running at `http://locahost:8084`

### 3. Deploying Node.js Application

For complete details and steps goto [Docker Docks - Building a Node js App](https://docs.docker.com/get-started/part2/)

we will only see the Dockerfile

```
# Use the official image as a parent image.
FROM node:current-slim

# Set the working directory.
WORKDIR /usr/src/app

# Copy the file from your host to your current location.
COPY package.json .

# Run the command inside your image filesystem.
RUN npm install

# Inform Docker that the container is listening on the specified port at runtime.
EXPOSE 8080

# Run the specified command within the container.
CMD [ "npm", "start" ]

# Copy the rest of your app's source code from your host to your image filesystem.
COPY . .
```


You can read about wordpress image at [Docker Hub - Wordress](https://hub.docker.com/_/wordpress)

### 4. Creating Tomcat server in Docker Container

[Tomcat server as service](https://github.com/jawad1989/devops/tree/master/projects/tomcat-docker)

### 5. Running Jenkins as container
installing jenkins in VM virtual Box ubuntu 18
 #### 1. Download ubuntu image
 #### 2. Setup VM in virtual box
 #### 3. install Docker Engine <br/>
   https://docs.docker.com/engine/install/ubuntu/
   * If you would like to use Docker as a non-root user, you should now consider adding your user to the “docker” group with something like:
```
  sudo usermod -aG docker your-user
```
 #### 4. Install Docker Compose <br/>
 https://docs.docker.com/compose/install/
 
 #### 5. Install Docker Jenkins Image
 https://hub.docker.com/_/jenkins/
 
 to get docker root directory
 ```
 docker info | grep -i root
 ```
 
 too see how much space docker is taking
 ```
 sudo du -sh /var/lib/docker
 ```
 #### 6. Create Docker Compose for Jenkins
 
   * create a new folder jenkins
   * cd in folder and mkdir jenkins_home for volume
   * create a new file 'vi docker-compose.yaml'
 ```
version: '3'

services:

  jenkins:

    container_name: jenkins

    image: jenkins

    ports:

      - "8080:8080"

    volumes:

      - $PWD/jenkins_home:/var/jenkins_home # left host vol : right docker vol

    networks:

      - net

networks:

  net: 
 ```
 
 
  docker compose is like a defination for what things we want

 #### 7. Create a Docker Container for Jenkins
 
  * make sure your user has proper rights on folders
  ```
  sudo chown 1000:1000 jenkins_home -R
  ```
  
  * run the docker compose
  ```
  docker-compose up -d
  ```
  
  verify by `docker ps` you will see the container running
  ![1](https://github.com/jawad1989/Jenkins101/blob/master/images/1.PNG)
  
  * run `docker logs -f jenkins` you will see the token which we will use later
  
  ![2](https://github.com/jawad1989/Jenkins101/blob/master/images/2%20-%20docker%20logs.PNG) 
  
  * get the ip from `ip a` and in your browser paste the ip:8080
  
  ![3](https://github.com/jawad1989/Jenkins101/blob/master/images/3%20-%20jenkins.PNG)
 
 * copy the code from `docker logs -f jenkins` and paste in jenkins and select either option as per need
 ![suggested](https://github.com/jawad1989/Jenkins101/blob/master/images/4%20-%20jenkins%20started.PNG)
 
 * plugins installing
 ![plugins](https://github.com/jawad1989/Jenkins101/blob/master/images/6%20-%20plugins%20installed.PNG)
 
 ![Jenkins](https://github.com/jawad1989/Jenkins101/blob/master/images/7%20-%20admin%20users.PNG)
 
 ![Jenkins](https://github.com/jawad1989/Jenkins101/blob/master/images/8%20-%20jenkins.PNG)
 
 ![Jenkins](https://github.com/jawad1989/Jenkins101/blob/master/images/9-jenkins.PNG)
 
 ![jenkins installed](https://github.com/jawad1989/Jenkins101/blob/master/images/10%20-jenknis.PNG)
 
### 6. Running MYSQL in docker

https://hub.docker.com/_/mysql

1. Create/update `docker-compole.yml`
  ***MYSQL_ROOT_PASSWORD*** is required by mysql image, you can check in mysql image link provided above. Also mysql volume location would be `/var/lib/mysql` also can be verified in above link.
  
 ```
 db_host: 
    container_name: mysql
    image: mysql:5.7
    environment: 
      - "MYSQL_ROOT_PASSWORD=1234"
    volumes: 
      - "$PWD/db_data:/var/lib/mysql"
 ```
2. start docker-compose by running `docker-compose up -d`
3. run `docker ps` to verify
4. check if mysql is up and runnin by running `docker logs -f <container_name>`
5. login into mysql 
 ```
 docker exec -ti mysql bash
 ```
6. connect mysql using user `root` and password `1234` as given in docker-compose file and run `show databases`
 ```
 mysql -u root -p
 show databases;
 ```
 ![show databases](https://github.com/jawad1989/Jenkins101/blob/master/images/5-show%20databases.PNG)
 
### 7. install AWS CLI and mysql Client on docker
  1. update `Dockerfile` in `centos` directory for `remote_host`
  
  ```
  FROM centos

RUN yum -y install openssh-server

RUN useradd remote_user && \
    echo "remote_user:1234" | chpasswd && \
    mkdir /home/remote_user/.ssh && \
    chmod 700 /home/remote_user/.ssh

COPY remote-key.pub /home/remote_user/.ssh/authorized_keys

RUN chown remote_user:remote_user -R /home/remote_user/.ssh/ &&\
    chmod 600 /home/remote_user/.ssh/authorized_keys

#RUN /usr/sbin/sshd-keygen
RUN ssh-keygen -A && rm -rf /run/nologin

#intall mysql
RUN yum -y install mysql

# install AWS CLI
RUN yum -y install epel-release && \
    yum -y install python3-pip && \
    pip3 install --upgrade pip && \
    pip3 install awscli

CMD /usr/sbin/sshd -D

  ```
  
  2. build `docker-compose build`  and run the container `docker-compose up -d`
  3. exec into remote_hose and verify aws/mysal
  ```
  docker exec -ti remote_host bash
  aws
  mysql
  ```
# 6. Data Containers

Data containers are containers whose sole responsibility is to be a place to store/manage data.
Like other containers they are managed by the host system. However, they don't run when you perform a docker ps command.

To create a Data Container we first create a container with a well-known name for future reference. We use busybox as the base as it's small and lightweight in case we want to explore and move the container to another host.

When creating the container, we also provide a -v option to define where other containers will be reading/saving data.

```
docker create -v /config --name dataContainer busybox
docker cp config.conf dataContainer:/config/
docker run --volumes-from dataContainer ubuntu ls /config
```

# 7. Managing Data in Docker

By default all files created inside a container are stored on a writable container layer. This means that:

The data doesn’t persist when that container no longer exists, and it can be difficult to get the data out of the container if another process needs it.
A container’s writable layer is tightly coupled to the host machine where the container is running. You can’t easily move the data somewhere else.

Writing into a container’s writable layer requires a storage driver to manage the filesystem. The storage driver provides a union filesystem, using the Linux kernel. This extra abstraction reduces performance as compared to using data volumes, which write directly to the host filesystem.

Docker has two options for containers to store files in the host machine, so that the files are persisted even after the container stops: volumes, and bind mounts. If you’re running Docker on Linux you can also use a tmpfs mount. If you’re running Docker on Windows you can also use a named pipe.

Volumes are stored in a part of the host filesystem which is managed by Docker (/var/lib/docker/volumes/ on Linux). Non-Docker processes should not modify this part of the filesystem. Volumes are the best way to persist data in Docker.

Bind mounts may be stored anywhere on the host system. They may even be important system files or directories. Non-Docker processes on the Docker host or a Docker container can modify them at any time.

tmpfs mounts are stored in the host system’s memory only, and are never written to the host system’s filesystem.

### 1. Docker Bind Mounts
Bind mounts have been around since the early days of Docker. Bind mounts have limited functionality compared to volumes. When you use a bind mount, a file or directory on the host machine is mounted into a container. The file or directory is referenced by its full or relative path on the host machine. By contrast, when you use a volume, a new directory is created within Docker’s storage directory on the host machine, and Docker manages that directory’s contents.
```
docker run -d \
  -it \
  --name devtest \
  --mount type=bind,source="$(pwd)"/target,target=/app \
  nginx:latest
```
### 2. Docker tmpds

```
$ docker run -d \
  -it \
  --name tmptest \
  --mount type=tmpfs,destination=/app \
  nginx:latest

```
### 3. Docker Volumes

Docker volumes are used to persist data from within a Docker container. There are a few different types of Docker volumes: host, anonymous, and, named. Knowing what the difference is and when to use each type can be difficult, but hopefully, I can ease that pain here.

* Host Volumes
A host volume can be accessed from within a Docker container and is stored on the host, as per the name. To create a host volume, run:
```
docker run -v /path/on/host:/path/in/container
```

* Anonymous Volumes
The location of anonymous volumes is managed by Docker. Note that it can be difficult to refer to the same volume when it is anonymous. To create an anonymous volume, run:
```
docker run -v /path/in/cntainer ...
```

* Named Volumes
Named volumes and anonymous volumes are similar in that Docker manages where they are located. However, as you might guess, named volumes can be referred to by specific names. To create a named volume, run:
```
docker volume create somevolume
docker run -v name:/path/in/container
```

### 1. Example:  Create a volume on host and place a static website, bind that volume with nginx and run the contianer using below command

Static Website at host: /images/mySite
Volume at VM: /usr/share/nginx/html
container Name: siteUsingVol
Image Name: nginx

```
docker run --name:siteUsingVol -d -v /images/mySite:/usr/share/nginx/html -p 5000:80 nginx
```

Now put any index.html in `/images/mySite` at host and you can see the file in VM `localhost:5000/`

### 2. Copying files between containers from a shared volume

Creating volume
```
docker volume create devops_volume
docker volume ls
```

Creating container with the volume attached to them
docker container create --name <container_name> -it --mount source<volume_name>,target=/<folder_Name> <image_name>

```
docker container create --name myBusyBox1 -it --mount source=devops_volume,target=/app busybox
docker container create --name myBusyBox2 -it --mount source=devops_volume,target=/app busybox
```

copying files between containers from a shared volume
```
docker exec -it myBusyBox1 sh
cd /app
mkdir devops
exit
```
copy a file frm host to myBusyBox1
```
docker container cp index.html myBusyBox1:/app/devops/
```

connect to 2nd container to see the file if created, you can sh the container and see the file and folder there


# 8. Docker Networking

> "Battries Included, But Removeable" - Default works well in many cases, but you can change to customize

### Docker Network Default:
1. Bridge network
2. All containers on Virtual network can talk to each other without -p
3. Each Virtual Network routes through NAT firewall on host IP

if we run ipconfig we will see docker0/bridge network that has a subnet CIDR 127.0.0.1/8 when ever we add more containers it picks one ip from the subnet list and extends. e.g. if we run a webserver http it will be 127.0.0.2 and another container nginx would be 127.0.0.3

```
jawad@DockerLab:~/code/hw/manageContainer$ docker container inspect --format '{{.NetworkSettings.IPAddress}}' db
172.17.0.3

jawad@DockerLab:~/code/hw/manageContainer$ docker container inspect --format '{{.NetworkSettings.IPAddress}}' webserver
172.17.0.2

jawad@DockerLab:~/code/hw/manageContainer$ ifconfig
docker0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255

```
if we create one more network, the containers in this network will extend ip CIDR/Range from this network

### list networks
docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
67acf5688fc2        bridge              bridge              local
7a67397d89d0        host                host                local # gains performace by skipping virutal networks but sacrificies security of container model
134bc1956c5f        none                null                local # removes etho2 and only leaves you with localost interface in container

### inspect network
docker network inspect bridge

### create network

docker network create my_app_network

### add new container into your network
docker container run -d --name new_httpd -p 8084:80 --network my_app_network

### add existing container to network
docker network connect my_network webserver

### disconnect container from network
docker network disconnect my_network webserver

## Docker Network DNS
You  can have multiple containers to be able to communicate with each other on the same docker host of your bridge network `my_network`, by using ping against their hostnames
```
docker exec -it webserver ping nginx
```

### Best Practices:

To create a new virtual network for each app:

1. Network "my_web_app" for mysql and php/apache containers
2. Network "my_api" for mongo and nodejs containers

## Traffic Flows and Firewalls
How docker networks move packets in and out
Docker’s networking subsystem is pluggable, using drivers. Several drivers exist by default, and provide core networking functionality:

* **User-defined bridge networks** are best when you need multiple containers to communicate on the same Docker host.
* **Host networks** are best when the network stack should not be isolated from the Docker host, but you want other aspects of the container to be isolated.
* **Overlay networks** are best when you need containers running on different Docker hosts to communicate, or when multiple applications work together using swarm services.
* **Macvlan networks** are best when you are migrating from a VM setup or need your containers to look like physical hosts on your network, each with a unique MAC address.
* **Third-party network plugins** allow you to integrate Docker with specialized network stacks.

### 1. Overlay Network

The overlay network driver creates a distributed network among multiple Docker daemon hosts. This network sits on top of (overlays) the host-specific networks, allowing containers connected to it (including swarm service containers) to communicate securely when encryption is enabled. Docker transparently handles routing of each packet to and from the correct Docker daemon host and the correct destination container.

[Overlay Network Guide](https://docs.docker.com/network/network-tutorial-overlay/)

### 2. Host Networking

If you use the host network mode for a container, that container’s network stack is not isolated from the Docker host (the container shares the host’s networking namespace), and the container does not get its own IP-address allocated. For instance, if you run a container which binds to port 80 and you use host networking, the container’s application is available on port 80 on the host’s IP address.

[Overlay Network Guide](https://docs.docker.com/network/network-tutorial-host/)


### 3. Macvlan Networking

Some applications, especially legacy applications or applications which monitor network traffic, expect to be directly connected to the physical network. In this type of situation, you can use the macvlan network driver to assign a MAC address to each container’s virtual network interface, making it appear to be a physical network interface directly connected to the physical network. In this case, you need to designate a physical interface on your Docker host to use for the macvlan, as well as the subnet and gateway of the macvlan

[Overlay Network Guide](https://docs.docker.com/network/network-tutorial-macvlan/)

### 4. Bridge Network

**Networking with standalone containers**
#### Use the default bridge network
demonstrates how to use the default bridge network that Docker sets up for you automatically. This network is not the best choice for production systems.

```
docker network ls

NETWORK ID          NAME                DRIVER              SCOPE
17e324f45964        bridge              bridge              local
6ed54d316334        host                host                local
7092879f2cc8        none                null                local
```

run two containers 
```
$ docker run -dit --name alpine1 alpine ash

$ docker run -dit --name alpine2 alpine ash
```
inspect the `bridge` network you will see both containers there
```
docker network inspect bridge
```

Use the `docker attach` command to connect to `alpine1`
if you will ping `google.com`, if you will ping ip of `alpine2` you will be successful but if you will ping hostname of `alpine2` it will fail
```
docker attach alpine`
ip addr show
ping c -2 google.com
ping c -2 172.17.0.3
ping c -2 alpine2
```

#### Use user-defined bridge networks
shows how to create and use your own custom bridge networks, to connect containers running on the same Docker host. This is recommended for standalone containers running in production.

```
docker network create --driver bridge alpine-net
docker network ls

NETWORK ID          NAME                DRIVER              SCOPE
e9261a8c9a19        alpine-net          bridge              local
17e324f45964        bridge              bridge              local
6ed54d316334        host                host                local
7092879f2cc8        none                null                local

docker network inspect alpine-net

docker network inspect bridge

docker network inspect alpine-net
```

if you inspect the `alpine-net` network, you wont see any container here, you have to add the containers to network, once you have added the conainers, you will be able to ping them by ip and hostname, once you run the last command you will see you can not ping hostname of `alpine3` but all other in network `alpine2,4`:

we will add `alpine 1,2,4` to `alpine-net` network and `alpine3` to default bridhe network
```
$ docker run -dit --name alpine1 --network alpine-net alpine ash

$ docker run -dit --name alpine2 --network alpine-net alpine ash

$ docker run -dit --name alpine3 alpine ash

$ docker run -dit --name alpine4 --network alpine-net alpine ash

$ docker network connect bridge alpine4

docker container attach alpine1
```


### Communicating between containers using links
This scenario explores how to allow multiple containers to communicate with each other. The steps will explain how to connect a data-store, in this case, Redis, to an application running in a separate container.

```
docker run -d --name redis-server redis
```
In this example, we bring up a Alpine container which is linked to our redis-server. We've defined the alias as redis. When a link is created, Docker will do two things.

First, Docker will set some environment variables based on the linked to the container. These environment variables give you a way to reference information such as Ports and IP addresses via known names.

You can output all the environment variables with the env command. For example:

```
docker run --link redis-server:redis alpine env
```
Secondly, Docker will update the HOSTS file of the container with an entry for our source container with three names, the original, the alias and the hash-id. You can output the containers host entry using cat /etc/hosts
```
docker run --link redis-server:redis alpine cat /etc/hosts
```
we can ping the source container
```
docker run --link redis-server:redis alpine ping -c 1 redis
```

Example app
```
docker run -d -p 3000:3000 --link redis-server:redis katacode/redis-note-docker-example
curl docker:3000
```

Connet to redis cli
```
docker run -it --link redis-server:redis redis redis-cli -h redis
KEYS *
QUIT
```

# 9. Share your images on Docker Hub

This step helps you in developing a containerized application is to share your images on a registry like Docker Hub, so they can be easily downloaded and run on any destination machine

1. Visit the Docker Hub sign up page, https://hub.docker.com/signup.

2. Fill out the form and submit to create your Docker ID.

3. Verify your email address to complete the registration process.

4. Click on the Docker icon in your toolbar or system tray, and click Sign in / Create Docker ID.

5. Fill in your new Docker ID and password. After you have successfully authenticated, your Docker ID appears in the Docker Desktop menu in place of the ‘Sign in’ option you just used.

Lets say you have an image named `mywebserver` and you want to deploy it on docker hub. images must be namespaced correctly to share on Docker Hub. Specifically, you must name images like `<Docker ID>/<Repository Name>:<tag>`. You can relabel your `NodejsServer:1.0` image like this (of course, please replace `jawadxiv` with your Docker ID):

`docker tag mywebserver:1.0 jawadxiv/NodejsServer:1.0`

Finally you push image to docker hub:

`docker push jawadxiv/NodejsServer:1.0`

You can pull using below command in any of your enviornment:

`docker pull jawadxiv/NodejsServer:1.0`
`docker pull jawadxiv/NodejsServer:1.0`



## 10. Useful Commands

current path as a bind volume

lets assume current directory has html code,this command will help us to make live changes
```
docker run -d -p 80:80 -v $(pwd):/usr/share/nginx/html nginx

```
docker run -d -p 8080:80 -v ${pwd}--name nginx-volume nginx
interactive shell contianer using image
```
docker run -it image_name sh
```

following the images with `entrypoint'
```
docker run -it --entrypoint sh image_name
```

if you want to see how the image was build, meaning the steps in its Dockerfile, you can:

```
docker image history --no-trunc image_name > image_history
```

Remove all containers
```
docker rm -f $(docker ps -aq)
```

Remove all images
```
docker rmi $(docker images -q)
```

Stop all containers
```
docker stop $(docker ps -aq)
```

remove volume
```
docker volume rm <volume_name>
```

remove all volumes at once
```
docker volume prune
```

Docker stats:
You can use the docker stats command to live stream a container’s runtime metrics. The command supports CPU, memory usage, memory limit, and network IO metrics.
```
docker stats redis1 redis2
```


Get IP for container
```
docker container inspect --format '{{.NetworkSettings.IPAddress}}' webserver
Output: 172.17.0.2
```

Get Port details for container
```
docker container port webserver
Output: 80/tcp -> 0.0.0.0:8080
```

docker container run --rm -it centos:7 bash

docker container run --rm -it ubuntu:14.04


Entry Point

docker run ubuntu-js 10, this will overwrite 5 value of CMd, if no value is given i.e. docker run ubuntu-js then it will sleep for 5 seconds
```
dockerfile

FROM ubuntu
ENTRYPOINT ["sleep"]
CMD ["5"]
```

Docker file to run python project
```
FROM python:3.6-alpine

RUN pip install flask

COPY . /opt/

EXPOSE 8080

WORKDIR /opt

ENTRYPOINT ["python", "app.py"]
```

### formats docker
https://docs.docker.com/config/formatting/

# 11. Docker Book For Virtulization Admins
https://github.com/mikegcoleman/docker101/blob/master/Docker_eBook_Jan_2017.pdf

# 12. Window Containers
Today Docker is much more than Linux. When you think of images, which are kernel specific, we're now talking about Linux x64, Linux x86 (32-bit), Windows 64bit, and a bunch more. This course still largely focuses on Linux x64 images because 90% of the concepts are the same, but "Windows Containers" are the new hotness! Technically, they are Native Windows .exe binaries running in Docker containers on a Windows kernel, and have no Linux installed.

Windows Containers and Docker 101 [Youtube]

Windows and Linux Parity with Docker [Youtube]

Docker + Microsoft - Investing in the Future of your Applications [Youtube]

# 13. Monitoring container

docker container top <container_name> - process list in one container

docker container inspect <container_name> - details of container config

docker container stats <container_name> - performance stats of container

docker container stats - performance stats of all containers ( LIVE )


# 13. Getting a Shell in Container

1 Run Command
```
docker containe run -it
docker container run -it --name ubuntu ubuntu
docker container run -it alpine bash # wont work as bash is not cnfigured in alpine
docker container run -it alpine sh
```

2. Exec Command
```
docker container exec -it
docker container exec -it mysql bash
```
