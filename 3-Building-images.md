

## Building Images ##  
Now we are going to build some Docker images according to our needs.  
We can use existing images from DockerHub and modify them.  

### Steps:  
1. Create a directory `images`.  
2. Navigate into the directory and create another directory `nano`.  
3. Download any web template from Tooplate using the `wget` command:  

```bash
wget https://www.tooplate.com/zip-templates/2137_barista_cafe.zip
sudo apt install unzip -y
ls
unzip 2137_barista_cafe.zip
clear
cd 2137_barista_cafe/
ls
tar czvf nano.tar.gz *
ls
mv nano.tar.gz ../
ls
cd ..
ls
rm -rf 2137_barista_cafe 2137_barista_cafe.zip
mv nano.tar.gz nano
```

These are the commands I ran.  

### Writing the Dockerfile in the `nano` directory:  

```dockerfile
FROM ubuntu:latest
LABEL "Author"="TAQI"
LABEL "Project"="nano"
RUN apt update && apt install git -y
RUN apt install apache2 -y
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
EXPOSE 80
WORKDIR /var/www/html
VOLUME /var/log/apache2
ADD nano.tar.gz /var/www/html
```

Now we can run the command:  

```bash
docker build -t nanoimg .
```

- `nanoimg`: Name of the image to be built.  
- `.`: Current working directory.  

Now, we run the Docker image and test it through the web server using the container's IP.  

---

## Hosting Image on DockerHub Registry ##  

1. Create an account on DockerHub.  
2. Create an image from the existing `nanoimg`.  
3. Run the container from this image.  
4. Log in to DockerHub using:  

```bash
docker login
```

5. Push the image to DockerHub using:  

```bash
docker push taqihaider/nanoimg
```

---

## CMD and ENTRYPOINT ##  

- **ENTRYPOINT**: Defines the main executable or script that runs when the container starts. It is not easily overridden.  

**Example:**  

```dockerfile
ENTRYPOINT ["python", "app.py"]
```

- **CMD**: Provides default arguments to ENTRYPOINT or acts as the container's default command if no ENTRYPOINT is set. It **can be overridden** at runtime.  

**Example:**  

```dockerfile
CMD ["--port", "8080"]
```

### Key Differences:  
- `ENTRYPOINT` is **fixed** and sets the executable.  
- `CMD` is **flexible** and can be overridden.  
- They can be used together for versatility.  

If no custom command is given, the default `CMD` is executed.  

---

## Docker Compose ##  

### Why use Docker Compose?  
- When multiple containers need to connect and work together.  
- It allows better container management.  

> **Note:** Always refer to the official documentation.  

### Installing Docker Compose on Linux:  

```bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

Make the binary executable:  

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

Verify the installation:  

```bash
docker-compose --version
```

Now, follow the official documentation to run a quickstart Docker Compose file.  

---

## Multi-Stage Dockerfile ##  

A **multi-stage Dockerfile** makes images smaller by splitting the build process into different stages.  
Each stage has a specific purpose (e.g., building the app or running it), and only essential files are included in the final image.  

### Why use it?  
- Reduces image size by removing unnecessary build dependencies.  
- Keeps the final image **clean and efficient**.  

### Example:  

```dockerfile
FROM openjdk:8 as BUILD_IMAGE
RUN apt update && apt install maven -y
RUN git clone -b vp-docker https://github.com/imranvisualpath/vprofile-repo.git
RUN cd vprofile-repo && mvn install

FROM tomcat:8-jre8
RUN rm -rf /usr/local/tomcat/webapps/*
COPY --from=BUILD_IMAGE vprofile-repo/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war
EXPOSE 8080
CMD ["catalina.sh", "run"]
```

### Benefits:  
- **Builds the artifact** in the first stage.  
- **Copies only the necessary artifact** to the final image, making it smaller.  

---


