## Build a docker image repository locally

1. Navigate into the project directory.
2. Build the project by running the following command, swapping out `DOCKER_USERNAME` with your username.

```bash
docker build -t DOCKER_USERNAME/getting-started-todo-app .
```

> [!NOTE]
> Must have dockerfile

## Verify if the image exists locally

```bash
docker image ls
```

## Tag a built Image

Tag allows to label and version your image.

1. You can tag during building an image

```bash
docker build -t DOCKER_USERNAME/docker-quickstart:1.0 .
```

2. Then you can tag the built image to mark that as different version

```bash
docker tag DOCKER_USERNAME/docker-quickstart:1.0 DOCKER_USERNAME/docker-quickstart:latest
```

## Push the image to Docker Hub

Use the `docker push` command. Be sure to replace `DOCKER_USERNAME` with your username:

```bash
docker push DOCKER_USERNAME/getting-started-todo-app
```

## Search container image

```bash
docker search <IMAGE_NAME>
```

> Example IMAGE_NAME: docker/welcome-to-docker

## Pull container image

```bash
docker pull <IMAGE_NAME>
```

> Example IMAGE_NAME: docker/welcome-to-docker

## List the image's layers:

```bash
docker image history <IMAGE_NAME>
```

> Example IMAGE_NAME: docker/welcome-to-docker

## Pull an Image and Run Docker Container (together)

```bash
$ docker run -d --name=CONTAINER_NAME -p 8080:80 IMAGE_NAME
```

- example image name: `docker/welcome-to-docker` (this should match the real image name stored in docker hub)
- example container name: `welcome-to-docker` (arbitrary)
- `-p` flag for port. `8080` is any available `host port` and `80` is a fixed `container port`

## View your running containers

- To see only the running containers

```bash
docker ps
```

- To see all containers including running and stopped

```bash
docker ps -a
```

> [!TIP]
> To see formated output instead of table view:
>
> - install `jq` running `sudo apt install jq`
> - run `docker ps --format json | jq '{ID, Names, Image, Ports, Status}'`

## Stop your container

1. Run `docker ps` to get the ID of the container
2. Provide the container ID or name to the `docker stop` command:

```
docker stop <container_id>
```

## Delete a container

```
docker rm -f <container_id_or_name>
```

## Use docker compose to start an application

```bash
docker compose up -d --build
```

## Tear down Compose stack

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
