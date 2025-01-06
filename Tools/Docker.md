# New Docker Container

To build a new image from a DockerFile

```bash
docker build -t <image_name> .
```

Replace `<image_name>` by the name you want to give to the image.

``` bash
docker exec -it <container_name> /bin/bash
```


```shell
docker-compose up -d
```

```bash
# Delete the image
docker rmi <image_name>

# Launch a specific service of a docker compose
docker-compose up <service_name>
```

# DockerFile