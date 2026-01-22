---
date: 2025-12-16
---
# Most common Docker commands

> **Description**: **Docker** is a tool that is used to automate the deployment of applications in lightweight containers so that applications can run efficiently in isolated environments.


#Containerization #DevOps #Microservices #Automation #Docker #DockerCompose

---

## 📋 **Table of Contents**

- [Image management commands](#image-management-commands)
- [Container Management Commands](#container-management-commands)
- [Docker Compose Commands](#docker-compose-commands)
- [DockerFile](#dockerfile)

---

## Image management commands

- Build a new image from a Dockerfile:

```bash
docker build -t "<image_name>" .
```

Replace `<image_name>` with the desired image name.

- Run a container from an image:

```bash
docker run "<image_name>"
```

You can add the `--detach` argument to run the image detached from the terminal

- List all Docker images:
```bash
docker images
```

- Remove an image:
```bash
docker rmi <image_name>
```

## Container Management Commands
- Open an interactive shell in a running container:
``` bash
docker exec -it <container_name> /bin/bash
```
- List running containers:
```bash
docker ps
```
`-a` to also show stopped containers

- Stop a running container: 
```bash
docker stop <container_name>
```

- Remove a container:
```bash
docker rm <container_name>
```
## Docker Compose Commands
- Start all services defined in `docker-compose.yml`
```shell
docker compose up
```
`-d` option stands for detach. It will run services in the background.
`--build` option force the rebuild of the images.

> Note: newer Docker versions use `docker compose` instead of `docker-compose`.


- Launch a specific service:
```bash
docker compose up <service_name>
```
- Build all services:
```bash
docker compose build
```
`--no-cache` to build from scratch

-  To destroy containers:
```bash
docker compose down
```
`-v` to also destroy volumes and networks

- Check logs of a specific service:
```bash
docker compose logs <service_name>
```
```bash
docker compose logs -f
```

## Dockerfile

A `Dockerfile` is a text document containing instructions to build a Docker image.

Common instructions:
- `FROM`
- `WORKDIR`
- `COPY`
- `RUN`
- `CMD` / `ENTRYPOINT`
