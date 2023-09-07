---
description: >-
  A lightweight, stand-alone package that contains everything needed to run a
  piece of software, including the code, runtime, system tools, system
  libraries, and settings.
cover: https://images3.alphacoders.com/109/1091500.jpg
coverY: 0
---

# Docker

## <mark style="color:red;">**Essential Docker Commands**</mark>

### <mark style="color:yellow;">**Docker build**</mark>

&#x20;Builds an image from a Dockerfile.

```bash
docker build -t your_image_name:tag .
```

### <mark style="color:yellow;">**Docker run**</mark>

Runs a Docker container from an image.

```bash
docker run -d -p host_port:container_port --name container_name image_name:tag
```

### <mark style="color:yellow;">**Docker ps**</mark>

Lists running containers.

```bash
docker ps
```

{% hint style="info" %}
**Tip**: To see all containers (including stopped ones), use `docker ps -a`.
{% endhint %}

### <mark style="color:yellow;">**Docker stop/rm**</mark>

Stops or removes a running container.

```bash
docker stop container_name_or_id # Stops one container
docker rm container_name_or_id # Remove one container
docker stop $(docker ps -aq) # Removes all containers
```

### <mark style="color:yellow;">**Docker rmi**</mark>

Removes a Docker image.

```bash
docker rmi image_name:tag
```

### <mark style="color:yellow;">**Docker logs**</mark>

Fetches the logs of a container.

```bash
docker logs container_name_or_id
```

### <mark style="color:yellow;">**Docker exec**</mark>

Opens a shell for commands inside a running container.

```bash
docker exec -it container_name_or_id /bin/sh
```

## <mark style="color:red;">**Common Mistakes and Troubleshooting**</mark>

1. <mark style="color:purple;">**Docker daemon not running**</mark><mark style="color:purple;">:</mark> Ensure the Docker service is started.
2. <mark style="color:purple;">**Port conflicts**</mark><mark style="color:purple;">:</mark> Make sure the port you're trying to bind to on the host is not already in use.
3. <mark style="color:purple;">**Issues with Dockerfile**</mark><mark style="color:purple;">:</mark> Always check the Dockerfile for any syntax errors or missing files.
4. <mark style="color:purple;">**Image/Container naming**</mark><mark style="color:purple;">:</mark> Be consistent with naming. Avoid using the same name for different containers or images.
5. <mark style="color:purple;">**`CMD`**</mark><mark style="color:purple;">** **</mark><mark style="color:purple;">**vs**</mark><mark style="color:purple;">** **</mark><mark style="color:purple;">**`ENTRYPOINT`**</mark><mark style="color:purple;">** **</mark><mark style="color:purple;">**in Dockerfile**</mark><mark style="color:purple;">:</mark> They can both be used to specify the command to run when starting a container, but `ENTRYPOINT` provides a more fixed way of defining it.

## <mark style="color:red;">**Docker in CI/CD Pipelines**</mark>

### <mark style="color:yellow;">**Setting up DooD (Docker Outside Of Docker)**</mark>

#### <mark style="color:purple;">To run a Docker client inside a container and connect it to the Docker host, you'll mount the Docker socket using a volume:</mark>

```bash
docker run -v /var/run/docker.sock:/var/run/docker.sock -ti docker:latest
```

This command maps the Docker socket from the host to the same location in the container.

&#x20;Inside the container, any Docker command you execute will actually be executed by the Docker daemon on the host.

#### <mark style="color:purple;">**Docker Compose example**</mark><mark style="color:purple;">:</mark>

```yaml
version: '3'
services:
  dood:
    image: docker:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```

This defines a service that has access to the host's Docker socket, giving it the ability to start, stop, build, and manage containers and images on the host.

### <mark style="color:yellow;">**Example using DooD in GitLab CI/CD**</mark>

#### <mark style="color:purple;">You can utilize DooD to manage Docker containers:</mark>

```yaml
variables:
  CONTAINER_IMAGE: my-docker-image

before_script:
  - docker login -u "$DOCKER_USERNAME" -p "$DOCKER_PASSWORD"

build:
  image: docker:latest
  script:
    - docker build -t $CONTAINER_IMAGE:$CI_COMMIT_REF_SLUG .
    - docker push $CONTAINER_IMAGE:$CI_COMMIT_REF_SLUG
  volumes:
    - /var/run/docker.sock:/var/run/docker.sock
```

{% hint style="info" %}
Note that we added `volumes` to the CI job to allow it to communicate with the Docker daemon on the host.
{% endhint %}

## <mark style="color:red;">Mounting Volumes in Docker</mark>

When working with Docker, sometimes it's necessary to persist data or share data between your local system and your Docker container. This is commonly done using Docker volumes.&#x20;

### <mark style="color:yellow;">How to Mount a Volume</mark>

To mount a volume (or bind mount a local directory) when starting a container, use the `-v` or `--volume` flag with the `docker run` command.

#### <mark style="color:purple;">**Syntax**</mark><mark style="color:purple;">:</mark>

```bash
docker run -v /path/on/host:/path/in/container my_image_name
```

#### <mark style="color:purple;">**Example**</mark><mark style="color:purple;">:</mark>

```bash
docker run -v $(pwd):/app my_image_name
```

In the example above, the current directory (`$(pwd)`) on the host will be mounted to the `/app` directory in the container.
