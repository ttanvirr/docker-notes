## Table of Contents

- [Install Docker Desktop](#get-docker---install-docker-desktop-on-windows-with-wsl2-backend)
- [Install Docker Engine on a vps hosting (ubuntu)](#install-docker-engine-on-a-vps-hosting-or-ubuntu-when-needed)
- [What is Docker](#what-is-docker)

- [Introduction](#introduction)
  - [Get Docker Desktop](#get-docker-desktop)
  - [Develop with containers](#develop-with-containers)
  - [Build and push your first image](#build-and-push-your-first-image)
    - [Method-1: CLI](#method-1-cli)
    - [Method-2: VSCode Extension](#method-2-vs-code-extension)

- [Docker Concepts](#docker-concepts)
  - [The Basics](#the-basics)
    - [What is a Container?](#what-is-a-container)
      - [Run Container using Docker Desktop](#method-1-to-run-a-container-docker-desktop-gui)
      - [Run Container using CLI](#method-2-to-run-a-container-cli)

    - [What is an Image?](#what-is-an-image)
      - [Using Docker Desktop](#method-1-to-search-and-pull-a-container-image-docker-desktop)
      - [Using CLI](#method-2-to-search-and-pull-a-container-image-cli)
    - [What is a registry?](#what-is-a-registry)

# Get Docker - Install Docker Desktop on Windows (with wsl2 backend)

[Link: Official Installation Guide](https://docs.docker.com/desktop/setup/install/windows-install/)

1. First check if required wsl version is installed on windows
   open cmd/powershell

```cmd
> wsl --version
> wsl --update
```

- You can also check your Linux distribution

```cmd
> wsl.exe --list --verbose
```

- If wsl insn't installed, install it
  [Link: wsl installation guide](https://learn.microsoft.com/en-us/windows/wsl/install)

Open cmd/powershell

```cmd
wsl --install
```

That'll all. Then check if a wsl version is installed

2. Download docker desktop installer for windows (from the installation page)
3. Double-click Docker Desktop Installer.exe to run the installer. The installer will ask which installation mode you prefer. Choosing 'per-user' (recommended) installs to %LOCALAPPDATA%\Programs\DockerDesktop and requires no administrator privileges. Choosing all users will prompt for elevation. Close after completed.
4. Start docker desktop (Search for docker desktop from windows and open). Sign in or Register.

# Install Docker Engine on a VPS hosting or Ubuntu (When needed)

While docker desktop is an GUI interface for docker, you will need to install docker engine (not docker desktop) on the vps hostings (Linux ubuntu)
[Link: Installation Guide](https://docs.docker.com/engine/install/ubuntu/)

## 1. Uninstall conflicting packages (but keep previous data)

- Run the following command to uninstall all conflicting packages:

```bash
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc | cut -f1)
```

> [!NOTE]
> Images, containers, volumes, and networks stored in /var/lib/docker/ `are not` automatically removed (will be preserved) when you uninstall Docker (running the above command).

### Clean start (if you don't want to keep previous data)

- If you want to start with a clean installation, and prefer to clean up any existing data, follow these steps:

- `WARNING:` The following steps will remove all existing data related to docker
- Uninstall the Docker Engine, CLI, containerd, and Docker Compose packages:

```bash
sudo apt purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
```

- Images, containers, volumes, or custom configuration files on your host aren't automatically removed. To delete all images, containers, and volumes:

```bash
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

- Remove source list and keyrings

```bash
sudo rm /etc/apt/sources.list.d/docker.sources
sudo rm /etc/apt/keyrings/docker.asc
```

## 2. Install using `apt` repository

- [Link: Installation guide](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)

1. Set up Docker's apt repository.

```bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

2. Install the Docker packages (latest)

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

3. After installation, verify that Docker is running:

```bash
sudo systemctl status docker
```

- If Docker is not running, start it manually:

```bash
sudo systemctl start docker
```

4. Verify that the installation is successful by running the `hello-world` image:

```bash
sudo docker run hello-world
```

# What is Docker?

- In real world, a docker is a person who works at the dock/port and load/unload shipping containers.
- [Link: Refer Documentation](https://docs.docker.com/get-started/docker-overview/)
- Docker is an open platform for developing, shipping, and running applications.
- Docker enables you to separate your applications from your infrastructure.
- significantly reduce the delay between writing code and running it in production.

## The Docker platform

- Docker provides the ability to package and run an application in a loosely isolated environment called a `container`.
- Containers are lightweight and contain everything needed to run the application, so you don't need to rely on what's installed on the host.
- You can share the same container to others.
- When you're ready, deploy your application into your production environment, as a container.

## What can I use Docker for?

- Fast, consistent delivery of your applications.
  Containers are great for continuous integration and continuous delivery (CI/CD) workflows.
- Responsive deployment and scaling.
  Docker containers can run on a developer's local laptop, on physical or virtual machines in a data center, on cloud providers, or in a mixture of environments.

## Docker architecture

- Docker uses a client-server architecture.
- The Docker client talks to the Docker daemon (in server), which does the heavy lifting of building, running, and distributing your Docker containers.

### The Docker daemon

- The Docker daemon (`dockerd`) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes.

### The Docker client

- The Docker client (`docker`) is the primary way that many Docker users interact with Docker.
- When you use commands such as `docker run`, the client sends these commands to `dockerd`, which carries them out.
- The `docker` command uses the Docker API.

### Docker Desktop

Docker Desktop is an easy-to-install application for your Mac, Windows, or Linux environment that enables you to build and share containerized applications and microservices. Docker Desktop includes the Docker daemon (dockerd), the Docker client (docker), Docker Compose, Docker Content Trust, Kubernetes, and Credential Helper. For more information see [Docker Desktop](https://docs.docker.com/desktop/)

### Docker registries

- A Docker registry stores Docker images.
- `Docker Hub` is a public registry that anyone can use, and Docker looks for images on Docker Hub by default.
- You can even run your own private registry.

### Docker Objects

#### Images

- An image is a read-only template with instructions for creating a Docker container.
- Often, an image is based on another image, with some additional customization.
- To build your own image, you create a `Dockerfile` with a simple syntax for defining the steps needed to create the image and run it.

#### Containers

- A container is a runnable instance of an image.
- You can create, start, stop, move, or delete a container using the Docker API or CLI.
- You can connect a container to one or more networks, attach storage to it, or even create a new image.
- By default, a container is relatively well isolated from other containers and its host machine.

#### Example `docker run` command

The following command runs an ubuntu container, attaches interactively to your local command-line session, and runs /bin/bash.

```bash
docker run -i -t ubuntu /bin/bash
```

1. If you don't have the ubuntu image locally, Docker pulls it from your configured registry, as though you had run `docker pull ubuntu` manually.
2. Docker creates a new container, as though you had run a `docker container create` command manually.
3. Docker allocates a read-write filesystem to the container, as its final layer.
4. Docker creates a network interface to connect the container to the default network.
5. Docker starts the container and executes `/bin/bash`.
6. When you run `exit` to terminate the /bin/bash command, the container stops but isn't removed.

## The underlying technology

- Docker is written in the `Go programming language`

# Introduction

What we'll learn in this series:

- Set up Docker Desktop
- Run your first container
- Build your first image
- Publish your image on Docker Hub

## Get Docker Desktop

Docker Desktop is the all-in-one package to build images, run containers, and so much more.

### Why Docker Desktop?

- Easily collaborate with team members
- Minimize setup and overhead to speed up the development process.

### Install Docker Desktop

- Follow [this link](#get-docker---install-docker-desktop-on-windows-with-wsl2-backend) for installation guide

### Run your first container

- First open/run the docker desktop
- open your CLI terminal and start a `container` by running `docker run` command:

```bash
docker run -d -p 8080:80 docker/welcome-to-docker
```

### Access the frontend

For this container, the frontend is accessible on port 8080. To open the website, visit http://localhost:8080 in your browser.

### Manage containers using Docker Desktop

Docker Desktop simplifies container management for developers by streamlining the setup, configuration, and compatibility of applications across different environments, thereby addressing the pain points of environment inconsistencies and deployment challenges.

1. Open Docker Desktop and select the `Containers` field on the left sidebar.
2. You can view information about your container including `logs`, and `files`, and even access the shell by selecting the `Exec` tab.
3. Select the `Inspect` field to obtain detailed information about the container.

## Develop with containers

### Start the project

1. To get started, clone the project to your local machine:

```bash
$ git clone https://github.com/docker/getting-started-todo-app
```

- navigate into the new directory:

```bash
$ cd getting-started-todo-app
```

2. Once you have the project, start the development environment using Docker Compose.

```bash
$ docker compose watch
```

3. You will see an output that shows container images being pulled down, containers starting, and more. Afer things are finished, open your browser to http://localhost to see the application up and running. It may take a few minutes for the app to run. The app is a simple to-do application

### What's in the environment?

Now that the environment is up and running, what's actually in it? At a high-level, there are several `containers` (or processes) that each serve a specific need for the application:

- React frontend - a Node container that's running the React dev server, using Vite.
- Node backend - the backend provides an API that provides the ability to retrieve, create, and delete to-do items.
- MySQL database - a database to store the list of the items.
- phpMyAdmin - a web-based interface to interact with the database that is accessible at http://db.localhost.
- Traefik proxy - Traefik is an application proxy that routes requests to the right service. It sends all requests for `localhost/api/_` to the backend, requests for `localhost/_` to the frontend, and then requests for `db.localhost` to phpMyAdmin. This provides the ability to access all applications using port 80 (instead of different ports for each service).

With this environment, you as the developer don’t need to install or configure any services, populate a database schema, configure database credentials, or anything. You only need Docker Desktop. The rest just works.

### Make changes to the app

With this environment up and running, you’re ready to make a few changes to the application and see how Docker helps provide a fast feedback loop.

#### Change the greeting

The greeting at the top of the page is populated by an API call at `/api/greeting`.

Currently, it always returns "Hello world!". You’ll now modify it to return one of three randomized messages.

1. Open `backend/src/routes/getGreeting.js`, update GREETINGS variable and the endpoint to send a random greeting from the list:

```js
const GREETINGS = [
  "Whalecome!",
  "All hands on deck!",
  "Charting the course ahead!",
]

module.exports = async (req, res) => {
  res.send({
    greeting: GREETINGS[Math.floor(Math.random() * GREETINGS.length)],
  })
}
```

2. Save the file. Every time you refresh your browser, you should see a random greeting from the list.

#### Change the placeholder text

Currently the placeholder text is simply "New Item".

1. Open `client/src/components/AddNewItemForm.jsx`.
2. Modify the placeholder attribute of the Form.Control element:

```jsx
<Form.Control
  value={newItem}
  onChange={(e) => setNewItem(e.target.value)}
  type="text"
  placeholder="What do you need to do?" // modify
  aria-label="New item"
/>
```

3. Save the file and go back to your browser. You should already see the changes.

#### Change the background color

Now, let's make the colors better.

1. Open `client/src/index.scss`.
2. Adjust the background-color attribute to any color you'd like:

```scss
body {
  background-color: #99bbff; // modified
  margin-top: 50px;
  font-family: "Lato";
}
```

Each save should let you see the change immediately in the browser.

### Recap

Within a few moments, you were able to:

- Start a complete development project with zero installation effort. The containerized environment provided the development environment, ensuring you have everything you need. You didn't have to install Node, MySQL, or any of the other dependencies directly on your machine. All you needed was `Docker Desktop` and a code editor.

- Make changes and see them immediately.

## Build and push your first image

Now you’re ready to create a container image for the application and share it on Docker Hub.

To do so, you will need to do the following:

- Sign in to [docker hub](https://hub.docker.com/) with your Docker account
- Create an image repository on Docker Hub
- Build the container image
- Push the image to Docker Hub

First, let's understand a few core concepts:

### Container images

think of them as a standardized package that contains everything needed to run an application, including its files, configuration, and dependencies. These packages can then be distributed and shared with others.

### Docker Hub

To share your Docker images, you need a place to store them. This is where registries come in. While there are many registries, Docker Hub is the default and go-to registry for images.

Docker Hub provides both a place for you to store your own images and to find images from others to either run or use as the bases for your own images.

When choosing base images, Docker Hub offers two categories of trusted, Docker-maintained images:

- Docker Official Images (DOI) – Curated images for popular software, following best practices and regularly updated.
- Docker Hardened Images (DHI) – Minimal, secure, production-ready images with near-zero CVEs, designed to reduce attack surface and simplify compliance. DHI images are free and open source under Apache 2.0.

Explore the full catalog of trusted content on Docker Hub.

### Sign in with your Docker account

sign in to [docker hub](https://hub.docker.com/) with a Docker account or create a new one.

### Create an image repository

Just as a Git repository holds source code, an image repository stores container images.

1. Go to Docker Hub.
2. Select Create repository.
3. On the Create repository page, enter the following information:
   - Repository name - `getting-started-todo-app`
   - Short description - Feel free to enter a description if you'd like.
   - Visibility - Select **Public** to allow others to pull your customized to-do app.

4. Select **Create** to create the repository.

### Build and push the image

Now that you have a repository, you are ready to build and push your image.

The image you are building extends the Node image, meaning you don't need to install or configure Node, yarn, etc. You can simply focus on what makes your application unique.

> [!Note]
> What is an image/Dockerfile?
>
> A Dockerfile is a text-based script that provides the instruction set on how to build the image. For this quick start, the repository already contains the Dockerfile.

#### Method-1: CLI

1. To get started, clone the project to your local machine. If you have already done this step, skip it.

```bash
git clone https://github.com/docker/getting-started-todo-app
cd getting-started-todo-app
```

2. Build the project by running the following command, swapping out `DOCKER_USERNAME` with your username.

```bash
docker build -t DOCKER_USERNAME/getting-started-todo-app .
```

`the '-t' flag stands for tag, which allows us to build local repo with a name. The local repo name should match the repo created on docker hub`

> [!NOTE]
> Make sure you include the dot (.) at the end of the docker build command. This tells Docker where to find the Dockerfile.

3. To verify the image exists locally, you can use either `docker image ls` or `docker images` command

4. Start a container to test the image (replace DOCKER_USERNAME):

```bash
docker run -d -p 8080:80 DOCKER_USERNAME/getting-started-todo-app
```

Verify if the container is working by visiting http://localhost:8080/ with your browser.

5. To push the image, use the `docker push` command. Be sure to replace `DOCKER_USERNAME` with your username:

```bash
docker push DOCKER_USERNAME/getting-started-todo-app
```

#### Method-2: VS Code Extension

1. Open Visual Studio Code. Ensure you have the `Docker` extension by Microsoft installed.
2. Clone the repository (https://github.com/docker/getting-started-todo-app) with vscode gui or your preferred way.
3. Right-click the `Dockerfile` and select the `Build Image...` menu item.
4. In the dialog that appears, enter a name of `DOCKER_USERNAME/getting-started-todo-app`, replacing `DOCKER_USERNAME` with your Docker username.
5. After pressing Enter, you'll see a terminal appear where the build will occur. Once it's completed, feel free to close the terminal.
6. Open the Docker Extension for VS Code by selecting the Docker or Containers logo in the left nav menu.
7. Find the image you created. It'll have a name of `docker.io/DOCKER_USERNAME/getting-started-todo-app`.
8. Expand the image to view the tags (or different versions) of the image. You should see a tag named `latest`, which is the default tag given to an image.
9. Right-click on the latest item and select the `Push...` option.
10. Press Enter with the name it suggests (repo_name:latest) to confirm and then watch as your image is pushed to Docker Hub (in the repo->tags).

Once the upload is finished, feel free to close the terminal.

# Docker Concepts

## The Basics

### What is a container?

#### Introduction

Imagine you're developing a web app that has three main components - a React frontend, a Python API, and a PostgreSQL database. If you wanted to work on this project, you'd have to install Node, Python, and PostgreSQL.

How do you make sure you have the same versions as the other developers on your team? Or your CI/CD system? Or what's used in production?

How do you ensure the version of Python (or Node or the database) your app needs isn't affected by what's already on your machine? How do you manage potential conflicts?

Enter containers!

`Containers are isolated processes` for each of your app's components (with all its files). Each component - the frontend React app, the Python API engine, and the database - runs in its own isolated environment, completely isolated from everything else on your machine.

**Containers are:**

- `Self-contained`. Each container has everything it needs to function with no reliance on any pre-installed dependencies on the host machine.
- `Isolated`. Since containers run in isolation, they have minimal influence on the host and other containers, increasing the security of your applications.
- `Independent`. Each container is independently managed. Deleting one container won't affect any others.
- `Portable`. Containers can run anywhere! The container that runs on your development machine will work the same way in a data center or anywhere in the cloud!

#### Containers versus virtual machines (VMs)

A VM is an entire operating system with its own kernel, hardware drivers, programs, and applications.

Spinning up a VM only to isolate a single application is a lot of overhead.

A container is simply an isolated process with all of the files it needs to run. If you run multiple containers, they all share the same kernel, allowing you to run more applications on less infrastructure.

Quite often, you will see containers and VMs used together. As an example, in a cloud environment, the provisioned machines are typically VMs. However, instead of provisioning one machine to run one application, a VM with a container runtime can run multiple containerized applications, increasing resource utilization and reducing costs.

#### Method-1 to Run a Container: Docker Desktop GUI

**Pulling and Image and Running Docker Container**

1. Open Docker Desktop and select the `Search` field on the top navigation bar.
2. Specify `welcome-to-docker` in the search input and then select the `Pull` button.
3. Once the image is successfully pulled, select the `Run` button.
4. Expand the Optional settings.
5. In the Container name, specify `welcome-to-docker`.
6. In the Host port, specify `8080`.
7. Select `Run` to start your container.
8. You can check the app by running http://localhost:8080/ in your browser
9. If you don't need the container for now to run, you can stop it and remove it from Containers list (keep the image for future use)

**View your container**

You can view all of your containers by going to the `Containers` view of the Docker Desktop Dashboard.

Our container runs a web server that displays a simple website. When working with more complex projects, you'll run different parts in different containers. For example, you might run a different container for the frontend, backend, and database.

**Access the Frontend**

For this container, the frontend is accessible on port `8080`. To open the website, select the link in the Port(s) column of your container or visit http://localhost:8080 in your browser.

**Explore your container**

Docker Desktop lets you explore and interact with different aspects of your container.

- Go to the `Containers` view in the Docker Desktop Dashboard.
- Select your container.
- Select the `Files` tab to explore your container's isolated file system.

**Stop your container**

The `docker/welcome-to-docker` container continues to run until you stop it.

- Go to the `Containers` view in the Docker Desktop Dashboard.
- Locate the container you'd like to stop.
- Select the `Stop` action in the Actions column.

#### Method-2 to Run a Container: CLI

**Pulling and Image and Running Docker Container**

Open your CLI terminal and start a container by using the `docker run` command:

```bash
$ docker run -d --name welcome-to-docker -p 8080:80 docker/welcome-to-docker
```

The command will pull the image `docker/welcome-to-docker` and run the container.
The output from this command is the full container ID.

**View your running containers**

- You can verify if the container is up and running by using the `docker ps` command.

> [!TIP]
> To view all containers including running and stopped, run: `docker ps -a`

**Access the frontend**

For our container, the frontend is accessible on port `8080`.  
To open the website, select the link in the Port(s) column of your container or visit `http://localhost:8080` in your browser.

**Stop your container**

The `docker/welcome-to-docker` container continues to run until you stop it.

1. Run `docker ps` to get the ID of the container
2. Provide the container ID or name to the `docker stop` command:

```
docker stop <container_id>
```

### What is an image?

#### Introduction

Seeing as a container is an isolated process, where does it get its files and configuration?  
How do you share those environments?

That's where container images come in.  
`A container image is a standardized package that includes all of the files, binaries, libraries, and configurations to run a container`.

There are two important principles of images:

1. Images are immutable. Once an image is created, it can't be modified. You can only make a new image or add changes on top of it.

2. Container images are composed of layers. Each layer represents a set of file system changes that add, remove, or modify files.

These two principles let you to extend or add to existing images. For example, if you are building a Python app, you can start from the Python image and add additional layers to install your app's dependencies and add your code.

#### Finding images

Docker Hub is the default global marketplace for storing and distributing images. You can search for `Docker Hub images` and run them directly from `Docker Desktop`.

Docker Hub provides a variety of Docker-supported and endorsed images known as Docker Trusted Content. These provide fully managed services or great starters for your own images. These include:

- `Docker Official Images` - a curated set of Docker repositories, serve as the starting point for the majority of users, and are some of the most secure on Docker Hub.
- `Docker Hardened Images` - minimal, secure, production-ready images with near-zero CVEs, designed to reduce attack surface and simplify compliance. Free and open source under Apache 2.0
- `Docker Verified Publishers` - high-quality images from commercial publishers verified by Docker
- `Docker-Sponsored Open Source` - images published and maintained by open-source projects sponsored by Docker

For example, `Redis` and `Memcached` are a few popular ready-to-go Docker Official Images. You can download these images and have these services up and running in a matter of seconds.

There are also base images, like the `Node.js` Docker image, that you can use as a starting point and add your own files and configurations.

For production workloads requiring enhanced security, Docker Hardened Images offer minimal variants of popular images like `Node.js`, `Python`, and `Go`.

#### Method-1 to search and pull a container image: Docker Desktop

**Search for and pull/download an image**

1. Select the global search bar at the top of the screen.
2. In the _Search_ field, enter "welcome-to-docker". Once the search has completed, select the `docker/welcome-to-docker` image. Select `Pull` to download the image

**Learn about the image**

1. In the Docker Desktop Dashboard, select the Images view.
2. Select the `docker/welcome-to-docker` image to open details about the image.
3. The image details page presents you with information regarding the `layers` of the image, the `packages` and libraries installed in the image, and any discovered `vulnerabilities`.

#### Method-2 to search and pull a container image: CLI

**Search for and pull/download an image**

1. Open a terminal and run:

```bash
docker search docker/welcome-to-docker
```

2. Pull the image:

```bash
docker pull docker/welcome-to-docker
```

**Learn about the image**

1. List your downloaded images using the `docker image ls` command.

> [!NOTE]
> The image size represented here reflects the uncompressed size of the image, not the download size of the layers.

2. List the image's layers:

```bash
docker image history docker/welcome-to-docker
```

### What is a registry

#### Introduction

Now, where do you store these images?

Well, you can store your container images on your computer system, but what if you want to share them with your friends or use them on another machine? That's where the image registry comes in.

`An image registry is a centralized location for storing and sharing your container images.` It can be either public or private. Docker Hub is a public registry that anyone can use and is the default registry.

There are many other available container registries available today, including Amazon Elastic Container Registry (ECR), Azure Container Registry (ACR), and Google Container Registry (GCR). You can even run your private registry on your local system or inside your organization. For example, Harbor, JFrog Artifactory, GitLab Container registry etc.

#### Registry vs. repository

A `registry` is a centralized location that stores and manages container images, whereas a `repository` is a collection of related container images within a registry. Think of it as a folder where you organize your images based on projects.

> [!TIP]
> A Docker Personal plan gives you one private repository and unlimited public repositories.

#### Build and Push a Docker image to a Docker Hub repository

Follow [the steps](#build-and-push-your-first-image) mentioned above to build and push an image.

This time try different project.

- Create a public repo in docker hub named "docker-quickstart"
- [Clone the project from this repo](https://github.com/dockersamples/helloworld-demo-node)
- Before pushing docker image, you can use `docker tag` command to label and version your image. for example:

```bash
docker tag DOCKER_USERNAME/docker-quickstart DOCKER_USERNAME/docker-quickstart:1.0
```

- Finally push the image to the registry
