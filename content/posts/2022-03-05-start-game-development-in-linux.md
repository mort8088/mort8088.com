---
title: How to start game development in Linux using MonoGame
author: mort8088
type: "posts"
date: "2022-03-05T00:00:23+00:00"
url: /2022/03/05/start-game-development-in-linux/
cover: /wp-content/uploads/2022/02/MonoGame-SquareLogo_256px.png
authorTwitter: "mort8088" #do not include @
tags : ["Bash", "Game Development", "Indie-Game", "Linux", "MonoGame"]
keywords : ["", ""]
description : ""
showFullContent : false
readingTime : true
hideComments : false
color : "" #color from the theme settings

---
<img decoding="async" loading="lazy" src="https://mort8088.com/wp-content/uploads/2022/02/MonoGame-SquareLogo_256px.png" alt="MonoGame SquareLogo" width="256" height="256" class="alignleft size-full wp-image-217" srcset="https://mort8088.com/wp-content/uploads/2022/02/MonoGame-SquareLogo_256px.png 256w, https://mort8088.com/wp-content/uploads/2022/02/MonoGame-SquareLogo_256px-150x150.png 150w" sizes="(max-width: 256px) 100vw, 256px" />  
Getting set up with Mono Game is as easy as running a script thanks to <a href="https://github.com/Kwyrky" rel="noopener" target="_blank"><i class="fab fa-github-square"></i>Kwyrky</a>

To make it as easy as possible to install MonoGame on Linux and to have content building work right from the start.

This will work on any Debian based Linux Distribution so Ubuntu and Mint are definitely going to work fine.

<!--more-->

We need to start by making sure that we have git installed so open a terminal and type the following command:-

<div class="hcb_wrap">
  <pre class="prism undefined-numbers lang-bash" data-lang="Bash"><code>sudo apt update | sudo apt install git</code></pre>
</div>

Once this has been installed we need to make a temporary directory then move into that folder

<div class="hcb_wrap">
  <pre class="prism undefined-numbers lang-bash" data-lang="Bash"><code>mkdir ~\Documents\github.com | cd ~\Documents\github.com
</code></pre>
</div>

and grab the latest version of the install script

<div class="hcb_wrap">
  <pre class="prism undefined-numbers lang-bash" data-lang="Bash"><code>git clone https://github.com/Kwyrky/InstallMonoGameLinuxScript.git</code></pre>
</div>

<img decoding="async" loading="lazy" src="https://mort8088.com/wp-content/uploads/2022/03/install-monoGame-Screenshot-001.png" alt="result of calling git clone command" width="960" height="234" class="aligncenter wp-image-222 size-full" srcset="https://mort8088.com/wp-content/uploads/2022/03/install-monoGame-Screenshot-001.png 960w, https://mort8088.com/wp-content/uploads/2022/03/install-monoGame-Screenshot-001-300x73.png 300w, https://mort8088.com/wp-content/uploads/2022/03/install-monoGame-Screenshot-001-150x37.png 150w, https://mort8088.com/wp-content/uploads/2022/03/install-monoGame-Screenshot-001-768x187.png 768w" sizes="(max-width: 960px) 100vw, 960px" /> 

Once that command has been completed you should have a new folder called InstallMonoGameLinuxScript change in to that folder with the cd command and there will be a _.sh _file inside as with anything on the internet you should read through it so you are confident that it does what you expect it to, I did which is why I&#8217;m confident that this works as intended. Call the script, because we&#8217;re installing new software to your system it&#8217;ll ask you for your sudo password then sit back and watch as everything you need to get started writing games in linux is put in the right place.

<img decoding="async" loading="lazy" src="https://mort8088.com/wp-content/uploads/2022/03/install-monoGame-Screenshot-002.png" alt="Starting the script with request your SUDO password to install software" width="910" height="524" class="aligncenter wp-image-221 size-full" srcset="https://mort8088.com/wp-content/uploads/2022/03/install-monoGame-Screenshot-002.png 910w, https://mort8088.com/wp-content/uploads/2022/03/install-monoGame-Screenshot-002-300x173.png 300w, https://mort8088.com/wp-content/uploads/2022/03/install-monoGame-Screenshot-002-150x86.png 150w, https://mort8088.com/wp-content/uploads/2022/03/install-monoGame-Screenshot-002-768x442.png 768w" sizes="(max-width: 910px) 100vw, 910px" /> 

You&#8217;ll know that the script is finished when you get your command prompt back.

<img decoding="async" loading="lazy" src="https://mort8088.com/wp-content/uploads/2022/03/install-monoGame-Screenshot-003.png" alt="End of the script returns to the command prompt." width="794" height="493" class="aligncenter wp-image-220 size-full" srcset="https://mort8088.com/wp-content/uploads/2022/03/install-monoGame-Screenshot-003.png 794w, https://mort8088.com/wp-content/uploads/2022/03/install-monoGame-Screenshot-003-300x186.png 300w, https://mort8088.com/wp-content/uploads/2022/03/install-monoGame-Screenshot-003-150x93.png 150w, https://mort8088.com/wp-content/uploads/2022/03/install-monoGame-Screenshot-003-768x477.png 768w" sizes="(max-width: 794px) 100vw, 794px" /> 

Towards the end of the script, it will create two test projects and run them to demonstrate that everything has worked, you can just press the _**Esc**_ key to exit from each of them.

Then as the script says it would be a good idea to log out and back in to make sure all the changes take effect.