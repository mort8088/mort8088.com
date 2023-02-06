---
title: "System update script"
author: "mort8088"
type: "posts"
date: "2021-12-01T00:00:16+00:00"
url: "/2021/12/01/118/"
cover: "/wp-content/uploads/2022/02/bash-logo-small-150x150.webp"
authorTwitter: "mort8088" #do not include @
tags : ["Bash", "Linux"]
showFullContent : false
readingTime : true
hideComments : false
description : "Most Debian based Linux users will just tell you > It's simple just type \"_sudo apt update && sudo apt upgrade_\" But at some point you're going to want to clean up your package manager and what about FlatPak update?"

---

Most Debian based Linux users will just tell you

> It's simple just type "_sudo apt update && sudo apt upgrade_"

But at some point you're going to want to clean up your package manager and what about FlatPak update?

## The Script

To those ends, I wrote a bash script called update that lives in my local bin folder (~/bin)

My full script is as follows:-

```Bash
sudo ls ~/ > /dev/null
echo "Starting full system update..."
echo "*** APT ***"
sudo apt update
sudo apt list --upgradable
sudo apt upgrade -yy
sudo apt autoremove -yy
sudo apt autoclean -yy

echo "*** FLATPAK ***"
sudo flatpak update

echo "Update Compleate!"
```

As you can see it does a little more than your basic update so here's the breakdown.

## Getting Started

```Bash
sudo ls ~/ > /dev/null
```

Because I use Linux Mint, that has a feature whereby if you execute a command with SuperUserDO and supply your password for a given period of time after you have validated yourself you don't need to provide your password to run another SUDO elevated command so this first line is just to get that timer to start, I could error handle the user getting the password wrong and exit but in this script I just let it carry on because you'll be prompted for the password again for each command in the script and if you get it wrong for each of them the command will not run until you get it right.

## Advanced Packaging Tool system update

```Bash
echo "Starting full system update..."
echo "*** APT ***"
```

With that out of the way we just print out some text so the user knows what's happening now, we're starting a full system update starting with APT.

```Bash
sudo apt update
```

The first APT command download lists of new/upgradable packages.

```Bash
sudo apt list --upgradable
```

The second APT command is to let the user know what is going to be updated.

```Bash
sudo apt full-upgrade -yy
```

## Advanced Packaging Tool system clean up

The third APT command will upgrade the system by removing/installing/upgrading packages, as opposed to upgrade which will perform a safe upgrade.

```Bash
sudo apt autoremove -yy
```

Once the upgrade is complete, the next APT command will remove automatically all unused packages

```Bash
sudo apt autoclean -yy
```

Followed by erasing old downloaded archive files.

## FlatPak update

```Bash
echo "*** FLATPAK ***"
sudo flatpak update
```

Lastly we print out that we're moving on to the FlatPak portion of the update where any FlatPak packages that have been updated are checked and updated, I only added this because I have a few Flatpack apps that I run but apparently this is all you need to do to keep them current.

```Bash
echo "Update Complete!"
```

Then print out that we are done.
