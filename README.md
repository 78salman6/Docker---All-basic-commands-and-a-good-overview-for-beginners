# Docker---All-basic-commands-and-a-good-overview-for-beginners

Docker
Docker Image:	
To login into Docker container
->This does not work always - docker exec -ti cc55da85b915 /bin/bash
docker exec -ti cc55da85b915 /bin/sh
systemctl start docker

## Basic Commands:
#### docker build -t friendlyhello .    -----------------# Create image using this directory's Dockerfile
#### docker run -p 4000:80 friendlyhello     ----------# Run "friendlyhello" mapping port 4000 to 80
#### docker run -d -p 4000:80 friendlyhello  -------# Same thing, but in detached mode
#### docker container ls  -----------------------------# List all running containers
#### docker container ls -a   --------------------------# List all containers, even those not running
#### docker container stop <hash>   -------------------------# Gracefully stop the specified container
#### docker container kill <hash>   --------------------------# Force shutdown of the specified container
#### docker container rm <hash>     --------------------------# Remove specified container from this machine
#### docker container rm $(docker container ls -a -q)# Remove all containers
#### docker image ls -a   -----------------------------# List all images on this machine
#### docker image rm <image id>  ----------------------# Remove specified image from this machine
#### docker image rm $(docker image ls -a -q)  --------# Remove all images from this machine
#### docker login   -----------------------------------# Log in this CLI session using your Docker credentials
#### docker tag <image> username/repository:tag   -----# Tag <image> for upload to registry
#### docker push username/repository:tag    -----------# Upload tagged image to registry
#### docker run username/repository:tag     -----------# Run image from a registry


## About Services: 
Services are really just “containers in production.”  Scaling a service changes the number of container instances running that piece of software, assigning more computing resources to the service in the process.

### First docker-compose.yml file:
A docker-compose.yml file is a YAML file that defines how Docker containers should behave in production.
A single container running in a service is called a task. Tasks are given unique IDs that numerically increment, up to the number of replicas
You can run curl -4 http://localhost:4000 several times in a row, or go to that URL in your browser and hit refresh a few times.
Either way, the container ID changes, demonstrating the load-balancing; with each request, one of the 5 tasks is chosen, in a round-robin fashion, to respond.

## Basic Commands related to services:
### docker stack ls      ---------------------------------------------# List stacks or apps
### docker stack deploy -c <composefile> <appname>  ------------------# Run the specified Compose file
### docker service ls    ---------------------------------------------# List running services associated with an app
### docker service ps <service>     ----------------------------------# List tasks associated with an app
### docker inspect <task or container>    ----------------------------# Inspect task or container
### docker container ls -q     ---------------------------------------# List container IDs
### docker stack rm <appname>     ------------------------------------# Tear down an application
### docker swarm leave --force    ------------------------------------# Take down a single node swarm from the manager


## Swarms:

### Understanding swarm clusters
A swarm is a group of machines that are running Docker and joined into a cluster. The machines in a swarm can be physical or virtual. After joining a swarm, they are referred to as nodes.
Swarm managers can use several strategies to run containers, such as “emptiest node” -- which fills the least utilized machines with containers. Or “global”, which ensures that each machine gets exactly one instance of the specified container. You instruct the swarm manager to use these strategies in the Compose file, just like the one you have already been using.
Swarm managers are the only machines in a swarm that can execute your commands, or authorize other machines to join the swarm as workers. Workers are just there to provide capacity and do not have the authority to tell any other machine what it can and cannot do.
But Docker also can be switched into swarm mode, and that’s what enables the use of swarms. Enabling swarm mode instantly makes the current machine a swarm manager. 

### Setting up Swarm
A swarm is made up of multiple nodes, which can be either physical or virtual machines. The basic concept is simple enough: run docker swarm init to enable swarm mode and make your current machine a swarm manager, then run docker swarm join on other machines to have them join the swarm as workers


## Docker Overview:
Docker is an open platform for developing, shipping, and running applications. By taking advantage of Docker’s methodologies for shipping, testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in production.

### The Docker Platform
The isolation and security allow you to run many containers simultaneously on a given host. Containers are lightweight because they don’t need the extra load of a hypervisor, but run directly within the host machine’s kernel. This means you can run more containers on a given hardware combination than if you were using virtual machines. You can even run Docker containers within host machines that are actually virtual machines!

### Docker Engine
Docker Engine is a client-server application with these major components:
1.	A server which is a type of long-running program called a daemon process
2.	A REST API which specifies interfaces that programs can use to talk to the daemon and instruct it what to do.
3.	A command line interface (CLI) client. 
The CLI uses the Docker REST API to control or interact with the Docker daemon through scripting or direct CLI commands.The daemon creates and manages Docker objects, such as images, containers, networks, and volumes.

### What can I use Docker for?
#### Example scenarios:
1.	Your developers write code locally and share their work with their colleagues using Docker containers.
2.	hey use Docker to push their applications into a test environment and execute automated and manual tests.
3.	When developers find bugs, they can fix them in the development environment and redeploy them to the test environment for testing and validation.
4.	When testing is complete, getting the fix to the customer is as simple as pushing the updated image to the production environment.

### Responsive deployment and scaling
Docker’s container-based platform allows for highly portable workloads. Docker containers can run on a developer’s local laptop, on physical or virtual machines in a data center, on cloud providers, or in a mixture of environments.

### Running more workloads on the same hardware
Docker is lightweight and fast. It provides a viable, cost-effective alternative to hypervisor-based virtual machines, so you can use more of your compute capacity to achieve your business goals. 

### Docker Architecture:
Docker uses a client-server architecture. The Docker client talks to the Docker daemon, which does the heavy lifting of building, running, and distributing your Docker containers. The Docker client and daemon can run on the same system, or you can connect a Docker client to a remote Docker daemon. The Docker client and daemon communicate using a REST API, over UNIX sockets or a network interface.

### The Docker daemon
The Docker daemon (dockerd) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.

### The Docker Client
The Docker client (docker) is the primary way that many Docker users interact with Docker. When you use commands such as docker run, the client sends these commands to dockerd, which carries them out. Docker client can communicate with more than one daemon.

### Docker Registries
A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to look for images on Docker Hub by default. You can even run your own private registry.
When you use the docker pull or docker run commands, the required images are pulled from your configured registry. When you use the docker push command, your image is pushed to your configured registry.

### Docker Objects

### IMAGES:
An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.
To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.

### CONTAINERS:
A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI. You can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.
By default, a container is relatively well isolated from other containers and its host machine. A container is defined by its image as well as any configuration options you provide to it when you create or start it. When a container is removed, any changes to its state that are not stored in persistent storage disappear.
### SERVICES:
Services allow you to scale containers across multiple Docker daemons, which all work together as a swarm with multiple managers and workers. A service allows you to define the desired state, such as the number of replicas of the service that must be available at any given time. By default, the service is load-balanced across all worker nodes. To the consumer, the Docker service appears to be a single application. Docker Engine supports swarm mode in Docker 1.12 and higher.

## SSH in Docker Container:
Sample Docker File to do SSH
  FROM ubuntu:16.04
  RUN apt-get update && apt-get install -y openssh-server
  RUN mkdir /var/run/sshd
  RUN echo 'root:screencast' | chpasswd
  RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
  # SSH login fix. Otherwise user is kicked off after login
  RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
  ENV NOTVISIBLE "in users profile"
  RUN echo "export VISIBLE=now" >> /etc/profile
  EXPOSE 22
  CMD ["/usr/sbin/sshd", "-D"]

### Now run the docker 
### Now to do SSH from any machine
### ssh  root@10.177.218.23 -p 6001

## To run multiple services in Docker container run all the services in background except one.
/usr/sbin/sshd -D &>/dev/null &





