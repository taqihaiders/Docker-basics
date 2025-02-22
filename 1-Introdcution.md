# Docker Learning Journey

## Introduction to Docker
Docker is a tool used to manage containers. Containers run inside Docker, providing isolated environments to execute applications efficiently.

### What is a Container?
Containers are lightweight, isolated environments that bundle applications with their dependencies. This ensures consistency across different systems.

---

## Setting Up Docker
We will set up a Docker engine to create and manage containers.

### Steps to Install Docker:
1. Launch an EC2 instance.
2. Visit the official Docker documentation.
3. Navigate to the Ubuntu installation section.
4. Follow the commands provided in the documentation for installation.

---

## Common Docker Commands

### Basic Commands
#### Pull an Image from a Registry:
```bash
docker pull <image>:<tag>
```
Downloads a specified image from Docker Hub or another registry.

#### Create a New Container:
```bash
docker create --name <container_name> <image>:<tag>
```
Creates a container without starting it.

#### Run a New Container:
```bash
docker run [options] --name <container_name> <image>:<tag>
```
Creates and starts a container in one step.

#### Start a Stopped Container:
```bash
docker start <container_name_or_id>
```
Starts an existing container.

#### Stop a Running Container:
```bash
docker stop <container_name_or_id>
```
Gracefully stops a running container.

#### Remove a Container:
```bash
docker rm <container_name_or_id>
```
Deletes a container (must be stopped first).

#### Remove an Image:
```bash
docker rmi <image>:<tag>
```
Deletes a specified image from the local system.

#### List Running Containers:
```bash
docker ps
```
Shows currently running containers.

#### List All Containers:
```bash
docker ps -a
```
Displays all containers, including stopped ones.

---

### Advanced Commands
#### Build an Image from a Dockerfile:
```bash
docker build -t <image_name>:<tag> .
```
Creates a new image from a Dockerfile in the current directory.

#### View Logs of a Container:
```bash
docker logs <container_name_or_id>
```
Displays the logs generated by a container.

#### Execute a Command Inside a Running Container:
```bash
docker exec -it <container_name_or_id> <command>
```
Runs a command inside a running container.

#### Copy Files from Host to Container:
```bash
docker cp <source_path> <container_name_or_id>:<destination_path>
```
Transfers files to a container.

#### Copy Files from Container to Host:
```bash
docker cp <container_name_or_id>:<source_path> <destination_path>
```
Transfers files from a container to the host.

#### View Container Details:
```bash
docker inspect <container_name_or_id>
```
Retrieves detailed configuration of a container.

#### Stop All Running Containers:
```bash
docker stop $(docker ps -q)
```
Stops all running containers.

#### Remove All Containers:
```bash
docker rm $(docker ps -aq)
```
Deletes all stopped containers.

#### Remove All Unused Images:
```bash
docker image prune
```
Removes dangling and unused images.

---

## Docker Networking
#### List All Networks:
```bash
docker network ls
```
Shows all available Docker networks.

#### Create a Network:
```bash
docker network create <network_name>
```
Creates a new Docker network.

#### Connect a Container to a Network:
```bash
docker network connect <network_name> <container_name_or_id>
```
Adds a container to a network.

#### Disconnect a Container from a Network:
```bash
docker network disconnect <network_name> <container_name_or_id>
```
Removes a container from a network.

---

## Docker Hub
Docker Hub is a cloud-based registry for storing and sharing Docker images.

### What is a Docker Image?
A Docker image is a lightweight, stand-alone package containing everything needed to run an application, including the code, runtime, libraries, and dependencies. Containers are instances of images running on Docker Engine.

#### Pulling the First Docker Image:
```bash
docker pull nginx
```
Downloads the Nginx image from Docker Hub.

---

This guide provides essential Docker concepts and commands to help you start your containerization journey efficiently!

