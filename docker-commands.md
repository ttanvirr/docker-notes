## Build a docker image repository locally

1. Navigate into the project directory.
2. Build the project by running the following command, swapping out `DOCKER_USERNAME` with your username.

```bash
docker build -t DOCKER_USERNAME/getting-started-todo-app .
```

## Verify if the image exists locally

```bash
docker image ls
```

## Push the image to Docker Hub

Use the `docker push` command. Be sure to replace `DOCKER_USERNAME` with your username:

```bash
docker push DOCKER_USERNAME/getting-started-todo-app
```

