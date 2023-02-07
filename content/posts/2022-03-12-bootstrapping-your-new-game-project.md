---
title: "Bootstrapping your new game project"
author: "mort8088"
type: "posts"
date: "2022-03-12T00:00:12+00:00"
url: "/2022/03/12/bootstrapping-your-new-game-project/"
cover: "/wp-content/uploads/2022/02/bash-logo-small-150x150.webp"
authorTwitter: "mort8088" #do not include @
tags : ["bash", "gameDevelopment", "indieGame", "linux", "monoGame"]
showFullContent : false
readingTime : true
hideComments : false

---
When you're just starting out with MonoGame programming on Linux it can be quite daunting especially if you've only ever used Visual Studio to create your projects, in this post I will give you a script that will get your project started so you can stop worrying about setting up your project and just get on with it.

## It all starts with a new script

To get started we need to create a new bash script in your bin folder:-

Make a new file called newGame in your bin folder

```Bash
  touch ~/bin/newGame
```

Make that file executable

```Bash
  chmod +x ~/bin/newGame
```

open the new file in the editor of your choice

```Bash
  nano ~/bin/newGame
```

## Start writing the script

Now we just need to write out the basics for what this script needs to do.

First off we need to indicate the script uses the bash shell, and grab the first parameter that will be passed into a variable for later reference.

```Bash
  #!/bin/bash

  projectName=$1
```

## A good source is controlled

Next, it's always a good idea to have your project in some sort of source control we have Git so we'll use it

```Bash
  git init
```

## Make me a project

Now we need to create and connect up all the files that make up our project remember the _**projectName**_ is the parameter that will be passed in when this script is called

```Bash
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
```

## Something we can build on

As an added bonus writing this script means we can add our own scripts to the project folder when we call it, such as the next part that adds a build script.

This script will not only do the publish build for both Linux x64 & window$ x64 but it'll also compress both of the folders up into neat zip files for distribution.

```Bash
  cat > build<<EOF
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
```

## That all-important first commit

And to cap it all off we'll have the script add everything to staging and take care of the important _&#8220;Initial Commit&#8221;_

```Bash
  git add .
  git commit -am "Initial commit"
```

## Just do it

When you've finished save the newly minted script and move to a folder that you want to create your new game in and invoke it

```Bash
  newGame monogametest001
```

You should see something like this:-

{{< image src="/wp-content/uploads/2022/03/newGame-monogametest001.png" alt="" style="margin: auto;" >}}

The only thing left to do is start Visual Studio Code and get coding but I'll leave that as an exercise for the reader.

## The script in full

```Bash
  #!/bin/bash

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

  cat > build<<EOF
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
  git commit -am "Initial commit"
```
