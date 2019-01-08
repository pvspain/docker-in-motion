# Docker in Motion

My notes for the Manning video course, [Docker In Motion][1]

## Index

- [Docker in Motion](#docker-in-motion)
  - [Index](#index)
  - [Introduction](#introduction)
  - [What is the Docker Engine?](#what-is-the-docker-engine)
    - [Server](#server)
    - [Client](#client)
    - [RESTful API](#restful-api)
  - [Docker tools and services](#docker-tools-and-services)
    - [Container](#container)
    - [Hub](#hub)
    - [Compose](#compose)
    - [Image](#image)
    - [Logs](#logs)
    - [Networking](#networking)
    - [Volumes](#volumes)
    - [Swarm](#swarm)
    - [Registry](#registry)
    - [Machine](#machine)

## Introduction

This course does not cover the installation of Docker. I've followed the [official installation notes][2] from Docker on Ubuntu/Debian Linux environments.

The [Linode][3]-tailored kernel required some additional tweaking (as root/sudo):

- Docker CE version: `docker-ce/bionic,now 5:18.09.0~3-0~ubuntu-bionic amd64`
- Linux kernel: `Linux thelonius 4.19.8-x86_64-linode120 #1 SMP PREEMPT Mon Dec 10 18:25:47 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux`
  
```Bash
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

 The API itself is versioned, which allows the interface to be backwards compatible if needed. A new version of the API is released when new features are ready, so you don't need to update the API unless you really want to take advantage of the new features. 
 
 Also, since the API is a RESTful interface, we can directly access it via `curl` if we wish. As the API decouples the server from the client, we are able to create our own Docker clients that harness the power of the Docker Engine via the API.

## Docker tools and services

### Container

A **container** houses all the required libraries and dependencies for it to run in isolation. In doing so, the container isolates its contents from its surroundings, which allows us to manage the container on multiple platforms. A container shares the host operating system's kernel, allowing it to start instantly and use less RAM.

### Hub

The **Hub** is a cloud-based registry service that offers public and private code repositories, workflows, pipelines, and collaboration tools. Here you can search and download public Docker images from organizations such as Ubuntu, Apache, MySQL, and many many more. From the Hub, you can push and pull your own Docker images, which are sent to the *Docker Cloud*. Both the Docker Cloud and the *Docker Hub* require a **Docker ID**, which you can get by registering a Docker Hub account. The collaboration element of Docker Hub allows you to create and manage teams of users that have different access levels to your Docker images. Features such as automated builds with webhooks can also be used within the build pipeline.

### Compose

With **Docker Compose**, you are able to configure multiple services which make up your application in one or more Docker Compose files. These files are created in a *YAML* structure, and can be committed to source control. Docker Compose is very useful when deploying to different sets of environments, such as development, staging, or even production.

### Image

A **Docker image** is *a set of layers* that create *instances of containers*. These are immutable and isolated. Docker images are built using **Dockerfiles**.

### Logs

**Container logs** can be accessed via the Docker Compose, as well as through the normal Docker command-line interface. By default, the Unix and Linux standard output and standard error streams can be accessed via the `docker log` command. Certain logging mechanisms may require additional steps to allow Docker to access the container logs; for instance, a service may send the log output to a file or database instead of a stream. This will mean the logs will be unavailable until the logging output is configured.

Different logging drivers can be supplied that alter how logs are handled by the service. This allows Docker to access the log output.

### Networking

By default, Docker will create a network between running containers. This (default) network is configured to behave like a bridge between the host and the containers. New containers are automatically connected to this default bridge when they are ran. The default bridge supports port mapping to allow communication between Docker containers.

We can also supply different options when creating containers that allow us to use user-defined networks instead. These networks can be configured to behave like:

- bridge networks
- overlay networks
- Mac VLANs

### Volumes

A data volume is a means of sharing and storing persistent storage. A host directory can be bind mounted, or a **Docker volume** can be used. Volumes can be shared amongst containers, and backup and copy features are also available.

### Swarm

A **Docker swarm** is a *cluster of Docker Engines* that are configured on *remote Docker Machines*. This allows us to manage multiple Docker Engines and therefore control multiple services that have been deployed. Replication, load balancing, rolling image, and configuration updates, multi-host networking, and cluster management are but a few features that the Docker swarm tool offers. [Kubernetes][4] is an alternative to Docker swarm, that is also supported by Docker.

### Registry

The **Docker Registry** is an application for storing and distributing Docker images. This is a scalable and cloud-based solution that can be run privately to your host and share your own images. Once set up, you can search, commit, push, and pull images within your own team.

### Machine

The **Docker Machine** is a virtual host that is set up to run the *Docker Engine*. If you think of it like a virtual machine in which you have an isolated machine which has its own Docker Engine, and therefore its own set of Docker containers.

[1]: https://www.manning.com/livevideo/docker
[2]: https://docs.docker.com/install/linux/docker-ce/ubuntu/
[3]: https://www.linode.com/
[4]: https://kubernetes.io/