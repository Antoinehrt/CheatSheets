---
date: 2025-12-16
---
# Docker commands

> **Description**: **Docker** is a tool that is used to automate the deployment of applications in lightweight containers so that applications can work efficiently in different environments in isolation.

#Containerization #DevOps #Microservices #Automation #Docker #DockerCompose

---

## 📋 **Table of Contents**

- [Image management commands](#image-management-commands)
- [Container Management Commands](#container-management-commands)
- [Docker Compose Commands](#docker-compose-commands)
- [DockerFile](#dockerfile)

---
### Image management commands

- Build a new image from a DockerFile:

```bash
docker build -t "<image_name>" .
```

Replace `<image_name>` with the desired image name.

- Run an image from a DockerFile:

```bash
docker run "<image_name>"
```

You can add the `--detach` argument to run the image detached from the terminal

- List all Docker images:
```bash
docker images
```

- Delete an image:
```bash
docker rmi <image_name>
```

### Container Management Commands
- Run a container interactively
``` bash
docker exec -it <container_name> /bin/bash
```

- Stop a running container: 
```bash
docker stop <container_name>
```

- Remove a container:
```bash
docker rm <container_name>
```
### Docker Compose Commands
- Start all services defined in `docker-compose.yml`
```shell
docker-compose up
```
`-d` option stands for detach. It will run services in the background.
`--build` option force the rebuild of the images.

- Launch a specific service:
```bash
docker-compose up <service_name>
```

- Check logs of a specific service:
```bash
docker-compose logs <service_name>
```
```bash
docker-compose logs -f
```

# DockerFile

A `DockerFile` is a text document containing instructions to build an image. 