---
url: "/homelab/ubuntu-server/"
title: "Installing Ubuntu Server"
author: mort8088
date: 2023-04-08T00:00:00+00:00
cover: "images/usb-drive.webp"
description: "I'm going to show you how to install Ubuntu Server on your old laptop that's been collecting dust in your closet. Why? Well, for starters, you can use it as a personal cloud storage, a media server, a VPN server, a web server, or even a Minecraft server. The possibilities are endless."
toc: false
readingTime: false
hideComments: false

authorTwitter: "mort8088" #do not include @
tags: [ "homelab", "linux", "server", "howto" ]
showFullContent: false
---

I'm going to show you how to install Ubuntu Server on your old laptop that's been collecting dust in your closet. Why? Well, for starters, you can use it as a personal cloud storage, a media server, a VPN server, a web server, or even a Minecraft server. The possibilities are endless.

Before we get started, you'll need a few things:

- An old laptop that meets the minimum requirements for Ubuntu Server (2 GB RAM, 25 GB disk space, 2 GHz processor)
- A USB flash drive with at least 4 GB of space
- A copy of Ubuntu Server ISO file (you can download it from [Here](https://ubuntu.com/download/server))

[{{< image src="images/UbuntuServer22-04-2-LTSDownloadPage.webp" alt="Ubuntu Server Download" style="margin: auto;" >}}](https://ubuntu.com/download/server)

## Create a bootable USB drive

The first thing you need to do is to create a bootable USB drive with the Ubuntu Server ISO file. You can use any tool you like, such as Rufus, Etcher, or UNetbootin. Just make sure you select the correct drive and the correct image file.

## Boot from the USB drive

[{{< image src="images/Screenshot%20Ubuntu%20GRUB%20Menu%20from%202023-04-15.webp" alt="Ubuntu Server boot menu" style="float:right" >}}](/homelab/ubuntu-server/images/Screenshot%20Ubuntu%20GRUB%20Menu%20from%202023-04-15.png)

Now plug the USB drive into your old laptop and turn it on. You may need to change the boot order in the BIOS settings to make it boot from the USB drive. If you see a screen like this, you're on the right track:

Select "Try or Install Ubuntu Server" and press Enter.

## Follow the installation wizard

The installation wizard will guide you through the steps of installing Ubuntu Server on your laptop.

[{{< image src="images/Step-001.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left;" >}}](/homelab/ubuntu-server/images/Step-001.png)

### Step 1

Choose your language.

---

[{{< image src="images/Step-002.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left;" >}}](/homelab/ubuntu-server/images/Step-002.png)

### Step 2

Choose your keyboard layout.

---

[{{< image src="images/Step-003.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left; clear: both;" >}}](/homelab/ubuntu-server/images/Step-003.png)

### Step 3

Choose type of install.

---

[{{< image src="images/Step-004.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left; clear: both;" >}}](/homelab/ubuntu-server/images/Step-004.png)

### Step 3a

Remember to select "Search for third-party drivers" just to be sure.

---

[{{< image src="images/Step-005.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left; clear: both;" >}}](/homelab/ubuntu-server/images/Step-005.png)

### Step 4

Network Connections

Select your main network adapter(eth0) and change it to manual so you can enter the static network details.

---

[{{< image src="images/Step-006.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left; clear: both;" >}}](/homelab/ubuntu-server/images/Step-006.png)

### Step 4a

**subnet** Is in the format where you type the network IP address for the lowest value and add "/24" to the end rather than using the 255.255.255.0 method it means the same thing just a different way of expressing it.

**Address** is the IP address of this computer.

**Gateway** is normally the address or your Router.

**Name Servers** is your DNS server use 8.8.8.8 for google or, if you have one, the IP address of your Pi-Hole.

---

[{{< image src="images/Step-008.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left; clear: both;" >}}](/homelab/ubuntu-server/images/Step-008.png)

### Step 4b

Once Saved Select Done to move to next step.

---

[{{< image src="images/Step-009.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left; clear: both;" >}}](/homelab/ubuntu-server/images/Step-009.png)

### Step 5

The proxy configured on this screen is used for accessing the package repository and the snap store both in the installer environment and in the installed system.

---

[{{< image src="images/Step-010.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left; clear: both;" >}}](/homelab/ubuntu-server/images/Step-010.png)

### Step 6

The installer will attempt to use `geoip` to look up an appropriate default package mirror for your location. If you want or need to use a different mirror, enter its URL here.

---

[{{< image src="images/Step-011.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left; clear: both;" >}}](/homelab/ubuntu-server/images/Step-011.png)

### Step 7

Storage configuration is a complicated topic and [has its own page for documentation](https://ubuntu.com/server/docs/install/storage).

---

[{{< image src="images/Step-014.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left; clear: both;" >}}](/homelab/ubuntu-server/images/Step-014.png)

### Step 8

Once the storage configuration is confirmed, the install begins in the background.

---

[{{< image src="images/Step-015.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left; clear: both;" >}}](/homelab/ubuntu-server/images/Step-015.png)

### Step 9

Create User, the default user will be an administrator, able to use sudo (this is why a password is needed, even if SSH public key access is enabled on the next screen).

---

[{{< image src="images/Step-016.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left; clear: both;" >}}](/homelab/ubuntu-server/images/Step-016.png)

### Step 10

Upgrade to Ubuntu Pro? NO!

---

[{{< image src="images/Step-017.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left; clear: both;" >}}](/homelab/ubuntu-server/images/Step-017.png)

### Step 11

Install OpenSHH, a default Ubuntu install has no open ports. We will want to administer servers via SSH so the installer allows it to be installed from this screen.

You can also import ssh keys for the default user from GitHub or Launchpad. If you do, then password authentication is disabled by default.

---

[{{< image src="images/Step-021.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left; clear: both;" >}}](/homelab/ubuntu-server/images/Step-021.png)

### Step 12

If a network connection is enabled, a selection of snaps that Canonical consider useful in a server environment are presented and can be selected for installation.

I choose not to use Snaps because I want to control the installations myself.

---

[{{< image src="images/Step-022.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left; clear: both;" >}}](/homelab/ubuntu-server/images/Step-022.png)

### Step 13

The installation process may take some time depending on your hardware and internet speed.

---

[{{< image src="images/Step-023.webp" alt="Ubuntu Server boot menu" style="margin:0 5px; float: left; clear: both;" >}}](/homelab/ubuntu-server/images/Step-023.png)

### Step 14

The final screen of the installer shows the progress of the installer and allows viewing of the full log file. Once the install has completed and security updates installed, the installer waits for confirmation before restarting.

---

Congratulations! You've successfully installed Ubuntu Server!

## Enjoy your new server

Now you can reboot your laptop and log in with your username and password.

That's it for today's tutorial. Next we'll set-up our client system to connect to the server using SSH and lock it down to be on the safe side. If you have any questions or feedback, feel free to leave a comment below. And don't forget to subscribe to the [RSS](https://mort8088.com/index.xml) for more content like this.
