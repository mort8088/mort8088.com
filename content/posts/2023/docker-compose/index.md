---
url: "/homelab/docker-compose/"
title: "Installing Docker Compose"
date: 2023-06-13T00:21:00+00:00
author: mort8088
authorTwitter: "mort8088" #do not include @
cover: "images/DockerComposeLogo.webp"
description: "Docker Compose is a tool for defining and running multi-container Docker applications. It allows developers to define a set of services and their dependencies in a single file, which can then be used to build and start the entire application stack with a single command. Docker Compose also provides features like scaling, networking, and environment variables to make it easier to manage complex applications."
tags: [ "homelab", "linux", "server", "howto", "docker", "dockercompose", "bash" ]
toc: false
readingTime: false
hideComments: false
showFullContent: false

draft: true
---

With Docker installed we can move on to getting familiar with Compose. Docker Compose lets you specify how and where to run your containers, and to manage their environment. You'll use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

First verify that Docker Compose is installed correctly by checking the version.

``` bash
docker compose version
```

> ``` bash
> Docker Compose version vN.N.N
> ```

Where vN.N.N is placeholder text standing in for the latest version, at time of writing it is *v2.17.3*.

So I keep all my docker files on the external HDD so the first thing to do is move over there and as you remember from *"[Adding An External DATA HDD](/homelab/ext-data-hdd/)"* we mounted that drive at */media/data*:-

``` bash
cd /media/data/
```

So now your prompt "should" look like -

``` bash
[<username>@<servername>]data$ _
```

next we need a folder to hold our Docker stuff:-

``` bash
mkdir Docker
cd Docker
```

Next we want to have a folder to hold the configurations for each container, I'm going to set up a [JellyFin](https://jellyfin.org) instance from [linuxserver.io](https://www.linuxserver.io). The reason for using the container from linuxserver.io is that they give a lot of documentation about getting them set up.

---

That's it for today's installment. Next %Teaser for the next post%. If you have any questions or feedback, feel free to leave a comment below. And don't forget to subscribe to the [RSS](https://mort8088.com/index.xml) so you'll know when I post the next captivating installment.

<!-- %adjective% List 
-- exciting
-- enthralling
-- captivating
engrossing
fascinating
gripping
riveting
consuming
interesting
-->
