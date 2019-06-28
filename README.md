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


## Comands


```console
Usage:	docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/Users/alvarosalasmtz/.docker")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level ("debug"|"info"|"warn"|"error"|"fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default "/Users/alvarosalasmtz/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/Users/alvarosalasmtz/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/Users/alvarosalasmtz/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  builder     Manage builds
  config      Manage Docker configs
  container   Manage containers
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes
```
### Practica:
```console
#Version de docker
> docker -v

#Realizar pull a imagen(ver hub de docker)
> docker pull <image>:<version | default: latest>

#Ver imagnes docker
> docker images

#Ver todos los contenedores
> docker ps -a

#Crear contenedor con interación y una terminal basado de una imagen ubuntu 18.04
> docker run -i -t ubuntu:18.04

#Iniciar un contenedor 
> docker start <CONTAINER ID>
#Nota para salir de un contenedor usar comando: > exit

#Entrar al contenedor
> docker attach <CONTAINER ID>

#Eliminar un contenedor
> docker rm <CONTAINER ID>

#Guardar nueva imagen a partir de contenedor
> docker commit <CONTAINER ID> <NAME NEW IMAGE>

#Ver historico de comandos de contenedor
> docker logs <CONTAINER ID>
```

## Practica 2
## Define a container with  `Dockerfile`

`Dockerfile`  defines what goes on in the environment inside your container. Access to resources like networking interfaces and disk drives is virtualized inside this environment, which is isolated from the rest of your system, so you need to map ports to the outside world, and be specific about what files you want to “copy in” to that environment. However, after doing that, you can expect that the build of your app defined in this  `Dockerfile`  behaves exactly the same wherever it runs.

### `Dockerfile`

Create an empty directory on your local machine. Change directories (`cd`) into the new directory, create a file called  `Dockerfile`, copy-and-paste the following content into that file, and save it. Take note of the comments that explain each statement in your new Dockerfile.

```
# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]

```

This  `Dockerfile`  refers to a couple of files we haven’t created yet, namely  `app.py`  and  `requirements.txt`. Let’s create those next.

## The app itself

Create two more files,  `requirements.txt`  and  `app.py`, and put them in the same folder with the  `Dockerfile`. This completes our app, which as you can see is quite simple. When the above  `Dockerfile`  is built into an image,  `app.py`  and  `requirements.txt`  is present because of that  `Dockerfile`’s  `COPY`  command, and the output from  `app.py`  is accessible over HTTP thanks to the  `EXPOSE`  command.

### `requirements.txt`

```
Flask
Redis

```

### `app.py`

```
from flask import Flask
from redis import Redis, RedisError
import os
import socket

# Connect to Redis
redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)

app = Flask(__name__)

@app.route("/")
def hello():
    try:
        visits = redis.incr("counter")
    except RedisError:
        visits = "<i>cannot connect to Redis, counter disabled</i>"

    html = "<h3>Hello {name}!</h3>" \
           "<b>Hostname:</b> {hostname}<br/>" \
           "<b>Visits:</b> {visits}"
    return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)

```

Now we see that  `pip install -r requirements.txt`  installs the Flask and Redis libraries for Python, and the app prints the environment variable  `NAME`, as well as the output of a call to  `socket.gethostname()`. Finally, because Redis isn’t running (as we’ve only installed the Python library, and not Redis itself), we should expect that the attempt to use it here fails and produces the error message.

> **Note**: Accessing the name of the host when inside a container retrieves the container ID, which is like the process ID for a running executable.

That’s it! You don’t need Python or anything in  `requirements.txt`  on your system, nor does building or running this image install them on your system. It doesn’t seem like you’ve really set up an environment with Python and Flask, but you have.

## Build the app

We are ready to build the app. Make sure you are still at the top level of your new directory. Here’s what  `ls`  should show:

```
$ ls
Dockerfile		app.py			requirements.txt

```

Now run the build command. This creates a Docker image, which we’re going to name using the  `--tag`  option. Use  `-t`  if you want to use the shorter option.

```
docker build --tag=friendlyhello .

```

Where is your built image? It’s in your machine’s local Docker image registry:

```
$ docker image ls

REPOSITORY            TAG                 IMAGE ID
friendlyhello         latest              326387cea398

```
## Run the app

Run the app, mapping your machine’s port 4000 to the container’s published port 80 using  `-p`:

```
docker run -p 4000:80 friendlyhello

```

You should see a message that Python is serving your app at  `http://0.0.0.0:80`. But that message is coming from inside the container, which doesn’t know you mapped port 80 of that container to 4000, making the correct URL  `http://localhost:4000`.

