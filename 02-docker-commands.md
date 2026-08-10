# Table of Contents <!-- omit in toc -->

- [1. Docker image](#1-docker-image)
  - [1.1. Build a docker image repository locally](#11-build-a-docker-image-repository-locally)
  - [1.2. Verify if the image exists locally](#12-verify-if-the-image-exists-locally)
  - [1.3. Tag a built Image](#13-tag-a-built-image)
  - [1.4. Push the image to Docker Hub](#14-push-the-image-to-docker-hub)
  - [1.5. Search container image](#15-search-container-image)
  - [1.6. Pull container image](#16-pull-container-image)
  - [1.7. List the image's layers:](#17-list-the-images-layers)
- [2. Docker container](#2-docker-container)
  - [2.1. Pull an Image and Run Docker Container (together)](#21-pull-an-image-and-run-docker-container-together)
  - [2.2. View your running containers](#22-view-your-running-containers)
  - [2.3. Stop your container](#23-stop-your-container)
  - [2.4. Delete a container](#24-delete-a-container)
  - [2.5. View filesystem inside a container](#25-view-filesystem-inside-a-container)
  - [2.6. View container logs](#26-view-container-logs)
  - [2.7. Comprehensive commad to run a container](#27-comprehensive-commad-to-run-a-container)
- [3. Docker compose](#3-docker-compose)
  - [3.1. Use docker compose to start an application](#31-use-docker-compose-to-start-an-application)
  - [3.2. Tear down Compose stack](#32-tear-down-compose-stack)

# 1. Docker image

## 1.1. Build a docker image repository locally

1. Navigate into the project directory.
2. Build the project by running the following command, swapping out `DOCKER_USERNAME` with your username.

```bash
docker build -t DOCKER_USERNAME/getting-started-todo-app .
```

> [!NOTE]
> Must have dockerfile

## 1.2. Verify if the image exists locally

```bash
docker image ls
```

## 1.3. Tag a built Image

Tag allows to label and version your image.

1. You can tag during building an image

```bash
docker build -t DOCKER_USERNAME/docker-quickstart:1.0 .
```

2. Then you can tag the built image to mark that as different version

```bash
docker tag DOCKER_USERNAME/docker-quickstart:1.0 DOCKER_USERNAME/docker-quickstart:latest
```

## 1.4. Push the image to Docker Hub

Use the `docker push` command. Be sure to replace `DOCKER_USERNAME` with your username:

```bash
docker push DOCKER_USERNAME/getting-started-todo-app
```

## 1.5. Search container image

```bash
docker search <IMAGE_NAME>
```

> Example IMAGE_NAME: docker/welcome-to-docker

## 1.6. Pull container image

```bash
docker pull <IMAGE_NAME>
```

> Example IMAGE_NAME: docker/welcome-to-docker

## 1.7. List the image's layers:

```bash
docker image history <IMAGE_NAME>
```

> Example IMAGE_NAME: docker/welcome-to-docker

Several of the lines may get truncated. Add `--no-trunc` flag to get the full output.

```bash
$ docker image history --no-trunc <IMAGE_NAME>
```

[⬆️Return to Table of contents](#table-of-contents)

# 2. Docker container

## 2.1. Pull an Image and Run Docker Container (together)

```bash
$ docker run -d --name=CONTAINER_NAME -p 8080:80 IMAGE_NAME
```

- example image name: `docker/welcome-to-docker` (this should match the real image name stored in docker hub)
- example container name: `welcome-to-docker` (arbitrary)
- `-p` flag for port. `8080` is any available `host port` and `80` is a fixed `container port`

## 2.2. View your running containers

- To see only the running containers

```bash
$ docker ps
```

- To see all containers including running and stopped

```bash
$ docker ps -a
```

> [!TIP]
> To see formated output instead of table view:
>
> - install `jq` running `sudo apt install jq`
> - run `docker ps --format json | jq '{ID, Names, Image, Ports, Status}'`

## 2.3. Stop your container

1. Run `docker ps` to get the ID of the container
2. Provide the container ID or name to the `docker stop` command:

```bash
$ docker stop <container_id>
```

## 2.4. Delete a container

```bash
$ docker rm -f <container_id_or_name>
```

## 2.5. View filesystem inside a container

- Open a shell inside a running container, use docker exec

  ```bash
  $ docker exec -it <CONTAINER_ID> bash
  ```

  If the container doesn't have `bash` (common with Alpine-based images), use `sh` instead:

  ```bash
  $ docker exec -it <CONTAINER_ID> sh
  ```

- Then run `ls` to view filesystem inside the container

## 2.6. View container logs

- Show logs and keep following on terminal
  ```bash
  $ docker logs -f <container-id>
  ```
  Press `Ctrl`+`C` to exit.
- Show logs but don't follow
  ```bash
    $ docker logs <container-id>
  ```

## 2.7. Comprehensive commad to run a container

```bash
$ docker run -dp 127.0.0.1:3000:3000 \
-w /app -v ".:/app" \
--network todo-app \
-e MYSQL_HOST=mysql \
-e MYSQL_USER=root \
-e MYSQL_PASSWORD=secret \
-e MYSQL_DB=todos \
node:24-alpine \
sh -c "npm install && npm run dev"
```

- `-dp` - detatch mode with port.
- `-w` - working directory
- `-v` - volume ("HOST_DIRECTORY:WORKING_DIRECTORY")
- `--network` - network name to attach with.
- `-e` - environment variable
- `sh -c` - shell command

[⬆️Return to Table of contents](#table-of-contents)

# 3. Docker compose

## 3.1. Use docker compose to start an application

```bash
docker compose up -d --build
```

## 3.2. Tear down Compose stack

```bash
docker compose down
```

> [!NOTE]
>
> By default, volumes aren't automatically removed when you tear down a Compose stack.
> If you do want to remove the volumes, add the `--volumes` flag:
>
> ```bash
> docker compose down --volumes
> ```

[⬆️Return to Table of contents](#table-of-contents)
