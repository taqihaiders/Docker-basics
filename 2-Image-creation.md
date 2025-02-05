# Testing Nginx in Docker

## Running an Nginx Container
In our previous section, we set up an Nginx container on our Docker server.

### Command to Install and Run Nginx:
```bash
docker run --name myweb -p 7090:80 -d nginx
```
- This command runs the Nginx container in detached mode (`-d`).
- It maps port **7090** of the host to port **80** of the container.

### Accessing Nginx:
1. Retrieve the **public IP** of your Docker server.
2. Ensure **All Traffic** is allowed from **your IP** in Security Group Rules.
3. Paste the public IP in the browser: `http://<Public-IP>:7090`.
4. You should see the "Welcome to Nginx" page.

## Deleting a Docker Image
### Steps:
1. Stop the running container:
   ```bash
   docker stop <container_id>
   ```
2. Remove the container:
   ```bash
   docker rm <container_id>
   ```
3. Delete the Docker image:
   ```bash
   docker rmi <image_name>
   ```

## Checking Docker Directory
You can check Docker container info by navigating to:
```bash
cd /var/lib/docker/containers
```
> **Note:** Containers run directly from images.

### Checking Container Size:
```bash
du -sh <container_id>
```

## Logging into a Container
Unlike EC2 instances, Docker containers don’t provide direct login access. However, you can interact with container files using:
```bash
docker exec <container_id> ls
```
To get an interactive shell:
```bash
docker exec -it myweb /bin/bash
```
> `-it` allows interaction inside the container.

## Viewing Docker Logs
To inspect an image:
```bash
docker inspect <image_name>
```
### Running an Nginx Container and Viewing Logs:
```bash
docker run -d -P nginx
```
- `-d`: Runs in the background.
- `-P`: Maps ports automatically.

To check logs:
```bash
docker logs <container_id>
```
> **Concept:** Docker logs help debug container crashes by providing runtime details.

### Example: Debugging a MySQL Container
```bash
docker run -d -P mysql:5.7
```
If the container **exits**, check logs:
```bash
docker logs <container_id>
```
If an environment variable is required, set it using:
```bash
docker run -d -P -e MYSQL_ROOT_PASSWORD=secretpass mysql:5.7
```

## Container Volumes
By default, stopping a container **deletes** its data. Volumes persist data even after stopping the container.

### Creating a Persistent MySQL Container:
```bash
docker run --name vprodb -d -e MYSQL_ROOT_PASSWORD=secretpass -p 3030:3306 -v /home/ubuntu/vprodbdata:/var/lib/mysql mysql:5.7
```
- `-v /home/ubuntu/vprodbdata:/var/lib/mysql` → Binds a local directory to store MySQL data.

### Verifying Persistent Storage:
```bash
docker exec -it vprodb /bin/bash
cd /var/lib/mysql
ls
```
The same files are stored in `/home/ubuntu/vprodbdata`.

## Using Docker Volume Command
Instead of manually specifying a storage path, you can create a volume:
```bash
docker volume create mydbdata
```
Run the container using the created volume:
```bash
docker run --name vprodb -d -e MYSQL_ROOT_PASSWORD=secretpass -p 3030:3306 -v mydbdata:/var/lib/mysql mysql:5.7
```
This ensures all database files are stored in `mydbdata` volume during the container's lifetime.

