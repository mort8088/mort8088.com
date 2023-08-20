+++
title = "Publishing things is fun."
date = "2023-02-19T00:00:00Z"
author = "mort8088"
authorTwitter = "mort8088" #do not include @
cover = "images/brokenDevelopment.png"
tags = ["irl", "itch.io", "monoGame", "bash"]
description = "This week I've been working on getting my development environment working again and adding the games I made last year to my [Itch.io](https://mort8088.itch.io/) Page."
showFullContent = false
readingTime = true
hideComments = false
toc = false

+++

This week I've been working on getting my development environment working again and adding the games I made last year to my [Itch.io](https://mort8088.itch.io/) Page.

The main problem I had was the script that I used last year to install everything hasn't been kept up to date with the changes in MonoGame v3.8.1 like in the fact they changed over to only use .NET 6, and that the way he was adding .NET to the system has changed now.  I might take some time to fork his script and fix it but that'll have to be a project for next week.

Like I said you can now get the three games I "finished" last year on [Itch.io](https://mort8088.itch.io/) they are :-

{{< itchio src="https://itch.io/embed/1931086" href="https://mort8088.itch.io/slider" name="Slider" >}}

{{< itchio src="https://itch.io/embed/1932013" href="https://mort8088.itch.io/monomine" name="MonoMine" >}}

{{< itchio src="https://itch.io/embed/1932434" href="https://mort8088.itch.io/monotris" name="Monotris" >}}

I'm going to go back to working on [MazeFodder](https://mort8088.itch.io/mazefodder/) but this time I'm just going to concentrate on getting the basic game done so no lighting or post effects, I'm also shelving [SystemX](https://github.com/mort8088/SystemX) for the time being, I'm not happy with the way it's holding together and it relies on too many services that aren't available anymore or that I never implemented correctly.

{{< image src="/images/uploads/2022/02/bash-logo-small-150x150.webp" alt="Hugo image" style="float: right; margin: 0px 0px 0px 10px;" >}}

Another thing I updated was my **newGame** bash script and the generated **build** script it produces, now that I finally understand how to use butler *(no idea what was so confusing last time)*. The build script now makes the project for windows and linux and uploads the result to the correct [Itch.io](https://mort8088.itch.io/) page. I still need to extend it to add in the support files for VS Code that make it possible to run/debug games in the editor.
