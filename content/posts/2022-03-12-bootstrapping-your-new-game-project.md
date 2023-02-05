---
title: Bootstrapping your new game project
author: mort8088
type: "posts"
date: "2022-03-12T00:00:12+00:00"
url: /2022/03/12/bootstrapping-your-new-game-project/
featured_image: /wp-content/uploads/2022/02/bash-logo-small.webp
authorTwitter: "mort8088" #do not include @
tags : ["Bash", Game Development"", "Indie-Game", "Linux", "MonoGame"]
keywords : ["", ""]
description : ""
showFullContent : false
readingTime : true
hideComments : false
color : "" #color from the theme settings

---
When you&#8217;re just starting out with MonoGame programming on Linux it can be quite daunting especially if you&#8217;ve only ever used Visual Studio to create your projects, in this post I will give you a script that will get your project started so you can stop worrying about setting up your project and just get on with it.

<!--more-->

## It all starts with a new script

To get started we need to create a new bash script in your bin folder:-

Make a new file called newGame in your bin folder

<div class="hcb_wrap">
  <pre class="prism undefined-numbers lang-bash" data-lang="Bash"><code>touch ~/bin/newGame</code></pre>
</div>

Make that file executable

<div class="hcb_wrap">
  <pre class="prism undefined-numbers lang-bash" data-lang="Bash"><code>chmod +x ~/bin/newGame</code></pre>
</div>

open the new file in the editor of your choice

<div class="hcb_wrap">
  <pre class="prism undefined-numbers lang-bash" data-lang="Bash"><code>nano ~/bin/newGame</code></pre>
</div>

## Start writing the script

Now we just need to write out the basics for what this script needs to do.

First off we need to <span>indicate the script uses the bash shell,</span> and grab the first parameter that will be passed into a variable for later reference.

<div class="hcb_wrap">
  <pre class="prism undefined-numbers lang-bash" data-lang="Bash"><code>#!/bin/bash

projectName=$1</code></pre>
</div>

## A good source is controlled

Next, it&#8217;s always a good idea to have your project in some sort of source control we have Git so we&#8217;ll use it

<div class="hcb_wrap">
  <pre class="prism undefined-numbers lang-bash" data-lang="Bash"><code>git init</code></pre>
</div>

## Make me a project

Now we need to create and connect up all the files that make up our project remember the _**projectName**_ is the parameter that will be passed in when this script is called

<div class="hcb_wrap">
  <pre class="prism undefined-numbers lang-bash" data-lang="Bash"><code># First create a solution file
dotnet new sln --name $projectName
# Next the main MonoGame project, I'm using the OpenGL template
dotnet new mgdesktopgl -o $projectName
# Because we are using Git as source control we'll need a Git ignore file this will be set up with all the things we need
dotnet new gitignore
# Now we need to connect the project to the solution
dotnet sln add ./$projectName/$projectName.csproj
# The next command uses NuGet to restore dependencies as well as project-specific tools that are specified in the project file.
dotnet restore
# the next two lines will build and run the new project just to be sure.
dotnet build $projectName.sln
dotnet run --project ./$projectName/$projectName.csproj</code></pre>
</div>

## Something we can build on

As an added bonus writing this script means we can add our own scripts to the project folder when we call it, such as the next part that adds a build script.

This script will not only do the publish build for both Linux x64 & window$ x64 but it&#8217;ll also compress both of the folders up into neat zip files for distribution.

<div class="hcb_wrap">
  <pre class="prism undefined-numbers lang-bash" data-lang="Bash"><code>cat &gt; build&lt;&lt;EOF
#!/bin/bash

dotnet publish -c Release -r win-x64 /p:PublishReadyToRun=false /p:TieredCompilation=false -o ./publish/windows/ --self-contained
cd ./publish/windows/
zip -r ../$projectName-windows.zip ./*
cd ../../
dotnet publish -c Release -r linux-x64 /p:PublishReadyToRun=false /p:TieredCompilation=false -o ./publish/linux/ --self-contained
cd ./publish/linux/
zip -r ../$projectName-linux.zip ./*
cd ../../
EOF
chmod +x ./build</code></pre>
</div>

## That all-important first commit

And to cap it all off we&#8217;ll have the script add everything to staging and take care of the important _&#8220;Initial Commit&#8221;_

<div class="hcb_wrap">
  <pre class="prism undefined-numbers lang-bash" data-lang="Bash"><code>git add .
git commit -am "Initial commit"</code></pre>
</div>

## Just do it

When you&#8217;ve finished save the newly minted script and move to a folder that you want to create your new game in and invoke it

<div class="hcb_wrap">
  <pre class="prism undefined-numbers lang-bash" data-lang="Bash"><code>newGame monogametest001</code></pre>
</div>

You should see something like this:-

<img decoding="async" loading="lazy" src="https://mort8088.com/wp-content/uploads/2022/03/newGame-monogametest001.png" alt="" width="856" height="641" class="aligncenter size-full wp-image-289" srcset="https://mort8088.com/wp-content/uploads/2022/03/newGame-monogametest001.png 856w, https://mort8088.com/wp-content/uploads/2022/03/newGame-monogametest001-300x225.png 300w, https://mort8088.com/wp-content/uploads/2022/03/newGame-monogametest001-150x112.png 150w, https://mort8088.com/wp-content/uploads/2022/03/newGame-monogametest001-768x575.png 768w" sizes="(max-width: 856px) 100vw, 856px" /> 

The only thing left to do is start Visual Studio Code and get coding but I&#8217;ll leave that as an exercise for the reader.

## The script in full

<div class="hcb_wrap">
  <pre class="prism undefined-numbers lang-bash" data-lang="Bash"><code>#!/bin/bash

projectName=$1

git init

# First create a solution file
dotnet new sln --name $projectName
# Next the main MonoGame project, I'm using the OpenGL template
dotnet new mgdesktopgl -o $projectName
# Because we are using Git as source control we'll need a Git ignore file this will be set up with all the things we need
dotnet new gitignore
# Now we need to connect the project to the solution
dotnet sln add ./$projectName/$projectName.csproj
# The next command uses NuGet to restore dependencies as well as project-specific tools that are specified in the project file.
dotnet restore
# the next two lines will build and run the new project just to be sure.
dotnet build $projectName.sln
dotnet run --project ./$projectName/$projectName.csproj

cat &gt; build&lt;&lt;EOF
#!/bin/bash

dotnet publish -c Release -r win-x64 /p:PublishReadyToRun=false /p:TieredCompilation=false -o ./publish/windows/ --self-contained
cd ./publish/windows/
zip -r ../$projectName-windows.zip ./*
cd ../../
dotnet publish -c Release -r linux-x64 /p:PublishReadyToRun=false /p:TieredCompilation=false -o ./publish/linux/ --self-contained
cd ./publish/linux/
zip -r ../$projectName-linux.zip ./*
cd ../../
EOF
chmod +x ./build

git add .
git commit -am "Initial commit"</code></pre>
</div>

&nbsp;