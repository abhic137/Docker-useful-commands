# Docker-useful-commands
ref: https://linuxhint.com/save-docker-container-as-image/
saving the running docker state:
```
docker commit <container_name>
docker images -a
docker tag <IMAGE_id> <TAG_Name>
docker images -a
```
downloading(saving locally) the image:
```
sudo docker save -o <FILENAME>.tar <REPONAME>

```

loading the image:
```
sudo docker load --input <FILENAME>.tar
sudo docker tag <Image-ID> oai-gnb:latest # May not be needed

```

pushing images in docker hub:
```
sudo docker login
sudo docker images
sudo docker tag <IMAGE_ID> USERNAME/REPONAME:TAG
sudo docker push USERNAME/REPONAME 

```
# Entrypoint Scripts:

An entrypoint script is a shell script or executable that is specified in your Docker image and is the first command that is executed when a container is started from that image. It's a way to ensure that certain setup or initialization tasks are performed before your application starts running.

Here's an example of how to use an entrypoint script

- Create a new directory for your project and navigate to it.
- Create a file named entrypoint.sh and add the following content

bash

```
#!/bin/bash

# Perform setup tasks here
echo "Initializing the application..."

# Run the command provided as arguments (this is the CMD or overridden command)
exec "$@"
```
    Make the script executable:

bash
```
chmod +x entrypoint.sh
```
    Create a Dockerfile in the same directory with the following content:

Dockerfile
```
# Use a base image
FROM ubuntu:latest

# Copy the entrypoint script into the image
COPY entrypoint.sh /usr/local/bin/

# Set the entrypoint script as the entrypoint for the container
ENTRYPOINT ["entrypoint.sh"]

# Default command to run if none is provided during container startup
CMD ["echo", "Hello, Docker World!"]
```
    Build the Docker image:

bash
```
docker build -t entrypoint-example .
```
    Run a container using the image:

bash
```
docker run entrypoint-example
```
Now, when you run the container, it will execute the entrypoint.sh script first and then execute the command specified in the CMD instruction (in this case, echo "Hello, Docker World!").
# Health Checks:

Health checks are used to determine the health of a running container. They allow you to define a command or a script that is periodically run inside the container to check if the application is functioning properly. This is crucial for automated orchestration systems (like Kubernetes) to know when a container is ready to serve traffic.

Here's an example of how to implement a basic health check:

    Update the Dockerfile to include a health check:

Dockerfile
```
# Use a base image
FROM ubuntu:latest

# Copy the entrypoint script into the image
COPY entrypoint.sh /usr/local/bin/

# Set the entrypoint script as the entrypoint for the container
ENTRYPOINT ["entrypoint.sh"]

# Default command to run if none is provided during container startup
CMD ["echo", "Hello, Docker World!"]

# Define a health check
HEALTHCHECK --interval=30s --timeout=10s CMD echo "Health check passed."
```
    Build the Docker image again:

bash
```
docker build -t health-check-example .
```
    Run a container with the health check enabled:

bash
```
docker run --health-cmd="echo 'Custom health check command!'" --health-interval=5s --health-timeout=2s health-check-example
```
In this example, the HEALTHCHECK instruction has been added to the Dockerfile. It specifies that every 30 seconds (as per --interval), Docker will run the provided command (in this case, echo "Health check passed."). If the command's exit status indicates success, the container is considered healthy; otherwise, it's marked as unhealthy.

When you run the container, you can see the health status using:

bash
```
docker ps
```
The --health-cmd, --health-interval, and --health-timeout flags in the docker run command allow you to customize the health check behavior.

These are just basic examples to get you started with entrypoint scripts and health checks in Docker. In real-world scenarios, you might have more complex setup tasks and health checks tailored to your application's requirements.

# A docker compose file with entrypoint-health script example demonstration
```
version: '3'
services:
  entrypoint-health-example:
    build:
      context: .
      dockerfile: Dockerfile
    environment:
      - MESSAGE=Hello from Docker Compose!
    healthcheck:
      test: ["CMD", "echo", "Health check passed."]
      interval: 30s
      timeout: 10s
    ports:
      - "8080:80"

```
```
sudo docker-compose up -d
```
for downing 
```
sudo docker-compose down
```
