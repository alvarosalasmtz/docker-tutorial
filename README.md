# Docker Tutorial

 - [Docker get started](https://docs.docker.com/get-started/)
 - [Docker hub](https://hub.docker.com/)

## Docker concepts

Docker is a platform for developers and sysadmins to  **develop, deploy, and run**  applications with containers. The use of Linux containers to deploy applications is called  _containerization_. Containers are not new, but their use for easily deploying applications is.

Containerization is increasingly popular because containers are:

-   Flexible: Even the most complex applications can be containerized.
-   Lightweight: Containers leverage and share the host kernel.
-   Interchangeable: You can deploy updates and upgrades on-the-fly.
-   Portable: You can build locally, deploy to the cloud, and run anywhere.
-   Scalable: You can increase and automatically distribute container replicas.
-   Stackable: You can stack services vertically and on-the-fly.

## Images and containers

A container is launched by running an image. An  **image**  is an executable package that includes everything needed to run an application--the code, a runtime, libraries, environment variables, and configuration files.

A  **container**  is a runtime instance of an image--what the image becomes in memory when executed (that is, an image with state, or a user process). You can see a list of your running containers with the command,  `docker ps`, just as you would in Linux.

## Containers and virtual machines
![Containers and virtual machines](https://www.docker.com/sites/default/files/d8/2018-11/docker-containerized-and-vm-transparent-bg.png)


