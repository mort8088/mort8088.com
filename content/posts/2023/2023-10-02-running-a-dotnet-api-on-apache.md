---
title: How to host a .NET Core API on Linux with Hestia
description: This post provides a step-by-step guide on how to host a .NET Core API on Linux using the Hestia control panel.
date: 2023-10-08T11:10:51.511Z
cover: /images/dotNetAPI.webp
draft: false
tags:
  - dotNet
  - linux
  - Hestia
categories: []
author: mort8088
showFullContent: false
readingTime: true
hideComments: false
toc: true
---

## Overview

This post provides a step-by-step guide on how to host a .NET Core API on Linux using the Hestia control panel. It covers everything from installing the necessary software to configuring the reverse proxy and systemd service.

Recently in work I have been told I'll need to learn React so I can support the new applications that are being written in my department.

With that in mind I thought I'd use my site as a test bed for this endeavour. A part of my plan to do this was to write an API that will serve the data for the site front end. So now I've created the need for me to understand not only how to design and write a maintainable API but also how I can host one on my Linux box. The simple answer and one I've done in the past is to write the API in PHP and be done with it.

But that would be too easy, I work as a dotNet developer so it make sense that I make this API in c# and use hosting it as a learning experience.

## Getting started

Where to start, well I already have dotNet installed on my dev system from working with monoGame so that's sorted, all I need to do is install dotNet to the hosting server and... What?

How do I get it to run?
How do I access it from the outside world?
All things I need to figure out, **ONWARD!**

## DotNet server install

Well I'm running [Ubuntu 22.04 (LTS)](https://ubuntu.com/) so this should be simple, Yep here it is:-

``` bash
sudo apt-get update && sudo apt-get install -y aspnetcore-runtime-7.0
```

Well that was easy.

## The Hestia problem

Ok this is where my knowledge hits a brick wall, after some Googling I found what I needed.

I use Hestia to manage my web install I couldn't just follow [the microsoft guides for how to set up Apache](https://learn.microsoft.com/en-us/aspnet/core/host-and-deploy/linux-apache?view=aspnetcore-7.0). But some enterprising chap has written up a script that when installed in the Hestia proxie templates folder will sort everything out for me.

The process is very simple. Clone the repository from [hestia-netcore](https://github.com/gabatronic/hestia-netcore) then copy the following files to the the Hestia Ngnix's templates:-

- netcore5000.sh - shell script that runs the app
- netcore5000.tpl - http ngnix proxy template
- netcore5000.stpl - https ngnix proxy template

``` bash
cp ./netcore5000.* /usr/local/hestia/data/templates/web/ngnix/
```

Make sure the bash script has the proper run permissions:-

``` bash
sudo chmod 755 /usr/local/hestia/data/templates/ngnix/netcore*.sh
```

## Make the site

Create a new site from the Control Panel, you'll want to set up the TLS, however *you* do that, I use the [Let's Encrypt](https://letsencrypt.org/) option. Once you have a working site to go to, you can select it and click on **Advanced options**, change the **Proxy template** to **netcore5000**.

Presuming you have your dotNet API to deploy and have built a Release version:-

``` csharp
dotnet publish --configuration Release
```

You can place your aspNetCore app to the **netcoreapp** folder, using the Hestia file manager.

## At your service

Now we need to create the service definition file:

``` bash
sudo nano /etc/systemd/system/kestrel.service
```

Copy this into the file:-

``` bash
[Unit]
Description=A wordy description of the servec something about dotNet

[Service]
WorkingDirectory=/home/%username%/web/%sitename%/netcoreapp
ExecStart=/usr/local/bin/dotnet /home/%username%/web/%sitename%/netcoreapp/%NameOfTheApp%.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-api-name
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production 
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

Remember:-

- Description - Should be changed to something relating to your app.
- WorkingDirectory - *%username%* id the user you created the site under and *%sitename%* is the domain name you created.
- ExecStart - *%username%/%sitename%* same here, but also *%NameOfTheApp%.dll* is the name of the project and it's the entry point of your app.
- SyslogIdentifier - Should be something that means something when you look in the logs.
- User - *www-data* is the user that the web server runs as, *www-data* is usually right.

To make the service start on boot up enable it:-

``` bash
sudo systemctl enable kestrel.service
```

Start the service:-

``` bash
sudo systemctl start kestrel.service
```

and verify that it's running:-

``` bash
sudo systemctl status kestrel.service
```

You should see something like the following:-

``` bash
◝ kestrel-helloapp.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel.service
            └─9021 /usr/local/bin/dotnet /home/%username%/web/%sitename%/netcoreapp/%NameOfTheApp%.dll
```

With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser.

## Testing

Swagger is not part of the Release build, so you'll have to test it another way, might I suggest [Postman](https://www.postman.com/) it has a free option that will allow you to test your API. But the best thing is to ***make sure it works before you launch it!***

## Conclusion

Keep in mind that the proxy template netcore5000.sh runs the application on TCP port 5000 so that port will be unavailable for other applications to use. The developer suggest you use Unix Sockets instead or create different templates for different TPC ports.

Also I haven't looked into how insecure this set up is yet, and I reserve the right to completely change my mind about it later 😉.

Have fun 🧑‍💻
