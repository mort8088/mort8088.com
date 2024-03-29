---
url: "/homelab/docker/"
title: "Installing Docker"
date: 2023-05-06T00:00:00+00:00
author: mort8088
authorTwitter: "mort8088" #do not include @
cover: "images/DockerLogo.webp"
description: "Docker is an open-source platform that allows developers to build, package, and run applications in containers, providing an efficient and consistent way to move software between development, testing, and production environments. Containers are lightweight, portable, and isolate applications from their environment, making them easier to manage and deploy."
tags: [ "homelab", "linux", "server", "howto", "docker", "bash" ]
toc: false
readingTime: false
hideComments: false
showFullContent: false

---

Once the laptop is setup and ready, you can install Docker.  

Docker is an open-source platform that allows developers to build, package, and run applications in containers, providing an efficient and consistent way to move software between development, testing, and production environments. Containers are lightweight, portable, and isolate applications from their environment, making them easier to manage and deploy.

We'll begin by adding dependencies needed by the installation process:

``` bash
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release
```

Next, add Docker’s official GPG key:

``` bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

Use the following command to set up the repository and update your package lists:

``` bash
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Now you can install Docker:

``` bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Using Docker Without Sudo

The Docker daemon runs as root, usually you'd prefix Docker commands with sudo. This can be tedious if you’re using Docker a lot. A simple solution is to add your log-in to the docker group so you can use Docker without sudo.

``` bash
sudo usermod -aG docker $USER
```

Log out and log back in so that your group membership is re-evaluated.

## Test Docker

To test that Docker is working correctly, you can run the following command:

``` bash
docker run hello-world:latest
```

This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.

> ``` bash
> Unable to find image 'hello-world:latest' locally
> latest: Pulling from library/hello-world
> 2db29710123e: Pull complete 
> Digest: sha256:03bd980423371343f887fc7980f67463795e8d33c958665785f4b62b57989c3d
> Status: Downloaded newer image for hello-world:latest
> 
> Hello from Docker!
> This message shows that your installation appears to be working correctly.
> 
> To generate this message, Docker took the following steps:
>  1. The Docker client contacted the Docker daemon.
>  2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
>     (amd64)
>  3. The Docker daemon created a new container from that image which runs the
>     executable that produces the output you are currently reading.
>  4. The Docker daemon streamed that output to the Docker client, which sent it
>     to your terminal.
> 
> To try something more ambitious, you can run an Ubuntu container with:
>  $ docker run -it ubuntu bash
> 
> Share images, automate workflows, and more with a free Docker ID:
>  https://hub.docker.com/
> 
> For more examples and ideas, visit:
>  https://docs.docker.com/get-started/
> ```

## Manage Docker Service

You can check the Docker service is running with systemctl. There are two components to check, docker and containerd.

``` bash
sudo systemctl status docker.service
sudo systemctl status containerd.service
```

You can manage these daemons like any other service on your system. Use *systemctl stop* to stop Docker and free up system resources:

``` bash
sudo systemctl stop docker.service
```

You can restart the service with *systemctl start*.

``` bash
sudo systemctl start docker.service
```

---
That's it for today's installment. Next we'll go over docker compose. If you have any questions or feedback, feel free to leave a comment below. And don't forget to subscribe to the [RSS](/index.xml) so you'll know when I post the next enthralling installment.
