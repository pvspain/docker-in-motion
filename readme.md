# Docker in Motion

My notes for the Manning video course, [Docker In Motion][1]

## Introduction

This course does not cover the installation of Docker. I've followed the [official installation notes][2] from Docker on Ubuntu/Debian Linux environments.

The [Linode][3]-tailored kernel required some additional tweaking (as root/sudo):

- Docker CE version: `docker-ce/bionic,now 5:18.09.0~3-0~ubuntu-bionic amd64` 
- Linux kernel: `Linux thelonius 4.19.8-x86_64-linode120 #1 SMP PREEMPT Mon Dec 10 18:25:47 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux`
```
# SERVICEDIR=/etc/systemd/system/containerd.service.d
# mkdir -p $SERVICEDIR
# cat << EOF > $SERVICEDIR/override.conf
[Service]
ExecStartPre=
EOF

# systemctl daemon-reload
# systemctl start docker
```

## What is the Docker Engine?

The Engine consists of three components:the server, a client, as well as a RESTful API.

### Server
The server is a long-running daemon called `dockerd`. Its job is to create and manage Docker objects such as – but not limited to – containers, images, networks, swarms, and volumes. The server listens to requests that are sent to the Docker socket `unix:///var/run/docker.sock`. This can be mapped to another port or another Unix socket.

### Client

The client is a command-line interface called `docker`, which sends commands to the server via the API. The client commands are very singular and precise, like `docker run`, `docker create`, and `docker restart`; but, behind the scenes, the Docker API is doing a lot of work. For instance, the command docker run will cause the API to create a container and then create an image if it doesn't exist. Start the container once the image is built and then attach the container if it's required.

### RESTful API

The API is the glue between the server and the client. The API's job is to expose the capabilities of the Docker Engine to the client. This allows us to:
- create, run, and control Docker containers
- build, manage, and commit, pull, and push Docker images
- read and monitor logs from containers
- build and manage networks
- create and control data volumes with persistent storage
- provision Docker swarms and scale services
- ...

### Docker tools and services

#### Container

A **container** houses all the required libraries and dependencies for it to run in isolation. In doing so, the container isolates its contents from its surroundings, which allows us to manage the container on multiple platforms. A container shares the host operating system's kernel, allowing it to start instantly and use less RAM.

#### Hub

The **Hub** is a cloud-based registry service that offers public and private code repositories, workflows, pipelines, and collaboration tools. Here you can search and download public Docker images from organizations such as Ubuntu, Apache, MySQL, and many many more. From the Hub, you can push and pull your own Docker images, which are sent to the *Docker Cloud*. Both the Docker Cloud and the *Docker Hub* require a **Docker ID**, which you can get by registering a Docker Hub account. The collaboration element of Docker Hub allows you to create and manage teams of users that have different access levels to your Docker images. Features such as automated builds with webhooks can also be used within the build pipeline.

#### Docker Compose

With **Docker Compose**, you are able to configure multiple services which make up your application in one or more Docker Compose files. These files are created in a *YAML* structure, and can be committed to source control. Docker Compose is very useful when deploying to different sets of environments, such as development, staging, or even production.

#### Docker image

A **Docker image** is *a set of layers* that create *instances of containers*. These are immutable and isolated. We will look further into how Docker images are built using **Dockerfiles**, and how a container can be managed.


[1]: https://www.manning.com/livevideo/docker
[2]: https://docs.docker.com/install/linux/docker-ce/ubuntu/
[3]: https://www.linode.com/