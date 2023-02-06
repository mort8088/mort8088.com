---
title: "How to start game development in Linux using MonoGame"
author: "mort8088"
type: "posts"
date: "2022-03-05T00:00:23+00:00"
url: "/2022/03/05/start-game-development-in-linux/"
cover: "/wp-content/uploads/2022/02/MonoGame-SquareLogo_256px-150x150.webp"
authorTwitter: "mort8088" #do not include @
tags : ["Bash", "Game_Development", "Indie-Game", "Linux", "MonoGame"]
showFullContent : false
readingTime : true
hideComments : false
toc : false
---

Getting set up with Mono Game is as easy as running a script thanks to [Kwyrky](https://github.com/Kwyrky)

To make it as easy as possible to install MonoGame on Linux and to have content building work right from the start.

This will work on any Debian based Linux Distribution so Ubuntu and Mint are definitely going to work fine.

We need to start by making sure that we have git installed so open a terminal and type the following command:-

```Bash
  sudo apt update && sudo apt install git
```

Once this has been installed we need to make a temporary directory then move into that folder

```Bash
  mkdir ~\Documents\github.com && cd ~\Documents\github.com
```

and grab the latest version of the install script

```Bash
  git clone https://github.com/Kwyrky/InstallMonoGameLinuxScript.git
```

{{< image src="/wp-content/uploads/2022/03/install-monoGame-Screenshot-001.png" alt="result of calling git clone command" style="margin: auto;" >}}

Once that command has been completed you should have a new folder called InstallMonoGameLinuxScript change in to that folder with the cd command and there will be a _.sh_ file inside as with anything on the internet you should read through it so you are confident that it does what you expect it to, I did which is why I'm confident that this works as intended. Call the script, because we're installing new software to your system it'll ask you for your sudo password then sit back and watch as everything you need to get started writing games in linux is put in the right place.

{{< image src="/wp-content/uploads/2022/03/install-monoGame-Screenshot-002.png" alt="Starting the script with request your SUDO password to install software" style="margin: auto;" >}}

You'll know that the script is finished when you get your command prompt back.

{{< image src="/wp-content/uploads/2022/03/install-monoGame-Screenshot-003.png" alt="End of the script returns to the command prompt." style="margin:auto;" >}}

Towards the end of the script, it will create two test projects and run them to demonstrate that everything has worked, you can just press the _**Esc**_ key to exit from each of them.

Then as the script says it would be a good idea to log out and back in to make sure all the changes take effect.
