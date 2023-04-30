---
title: "Installing Docker Compose"
author: mort8088
type: page
date: 2023-04-08T00:21:00+00:00
cover: "images/DockerComposeLogo.webp"
description: "Docker Compose is a tool for defining and running multi-container Docker applications. It allows developers to define a set of services and their dependencies in a single file, which can then be used to build and start the entire application stack with a single command. Docker Compose also provides features like scaling, networking, and environment variables to make it easier to manage complex applications."
toc: false
readingTime: false
hideComments: false
draft: true
---

With Docker installed we can move on to getting familiar with Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

Verify that Docker Compose is installed correctly by checking the version.

```Bash
docker compose version
Docker Compose version vN.N.N
```

Where vN.N.N is placeholder text standing in for the latest version, at time of writing it is *v2.17.2*.
