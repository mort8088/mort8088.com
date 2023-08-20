---
url: "/homelab/server-lockdown/"
title: "Post install configuration"
author: mort8088
date: 2023-04-30T00:00:00+00:00
cover: "images/BashLogo.webp"
description: "Once you've installed the Operating System on your laptop we need to secure it so it's not so easy for someone to get into your data."
toc: true
readingTime: false
hideComments: false

authorTwitter: "mort8088" #do not include @
tags: [ "homelab", "linux", "server", "howto", "security", "bash" ]
showFullContent: false
---

Once you've installed the Operating System on your laptop we need to secure it so it's not so easy for someone to get into your data.

## SSH Public/Private Keys

Using [SSH](https://en.wikipedia.org/wiki/Secure_Shell) public/private keys is more secure than using a password. It also makes it easier and faster, to connect to our server because you don't have to enter a password.

1. From the computer you're going to use to connect to your server, **the client**, not the server itself, create an Ed25519[^1] key with:

    ``` bash
    ssh-keygen -t ed25519
    ```

    Basically accepting the defaults by hitting return until you get back to the command prompt is fine but you can set a passphrase for extra security.

    > ``` bash
    > Generating public/private ed25519 key pair.
    > Enter file in which to save the key (/home/user/.ssh/id_ed25519):
    > Created directory '/home/user/.ssh'.
    > Enter passphrase (empty for no passphrase):
    > Enter same passphrase again:
    > Your identification has been saved in /home/user/.ssh/id_ed25519.
    > Your public key has been saved in /home/user/.ssh/id_ed25519.pub.
    > The key fingerprint is:
    > SHA256:F44D4dr2zoHqgj0i2iVIHQ32uk/Lx4P+raayEAQjlcs user@client
    > The key's randomart image is:
    > +--[ED25519 256]--+
    > |xxxx  x          |
    > |o.o +. .         |
    > | o o oo   .      |
    > |. E oo . o .     |
    > | o o. o S o      |
    > |... .. o o       |
    > |.+....+ o        |
    > |+.=++o.B..       |
    > |+..=**=o=.       |
    > +----[SHA256]-----+
    > ```

    **Note**: If you set a passphrase, you'll need to enter it every time you connect to your server using this key, unless you're using `ssh-agent`.

    **Note 2**: You only need to do this once after that you can skip to step 2 to copy this key to as many servers as you want they can all use the same key.

2. Now you need to send the public key from your client to your server. Since we're at home on a LAN, we're probably safe from MITM[^2] attacks, so we'll use `ssh-copy-id` to transfer the public key:

    ``` bash
    ssh-copy-id user@server
    ```

    If this is the first time you've tried to connect to this server over SSH then you'll be prompted to confirm authenticity of the host by it's ECDSA key fingerprint, *sounds fancy* just type 'yes' to continue and don't worry about it unless it says it again and you haven't done anything to change the server, that could mean a breach.

    You will be asked for the password of the user you are setting the keys to, that's the server user's password, not the key password.

    > ``` bash
    > /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/user/.ssh/id_ed25519.pub"
    > The authenticity of host 'host (192.168.0.200)' can't be established.
    > ECDSA key fingerprint is SHA256:QaDQb/X0XyVlogh87sDXE7MR8YIK7ko4wS5hXjRySJE.
    > Are you sure you want to continue connecting (yes/no)? yes
    > /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
    > /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
    > user@host's password:
    > 
    > Number of key(s) added: 1
    > 
    > Now try logging into the machine, with:   "ssh 'user@host'"
    > and check to make sure that only the key(s) you wanted were added.
    > ```

Now you can log into your new server without having to type in a password.

``` bash
ssh user@server
```

## Update your server

Now you're connected to your server the first thing you should do to secure it is to update the local repositories and upgrade the OS and installed applications by applying the latest patches.

``` bash
sudo apt-get update && sudo apt-get upgrade --assume-yes
```

## Secure SSH

Next, make these three changes:

- Disable SSH password authentication
- Restrict root from logging in remotely
- Restrict access to IPv4 or IPv6

Open `/etc/ssh/sshd_config` using your text editor of choice and ensure these lines:

``` bash
PasswordAuthentication yes
PermitRootLogin yes
```

look like this:

``` bash
PasswordAuthentication no
PermitRootLogin no
```

Next, restrict the SSH service to either IPv4 or IPv6 by modifying the AddressFamily option. To change it to use only IPv4 (which should be fine for most folks) make this change:

``` bash
AddressFamily inet
```

Restart the SSH service to enable your changes. Note that it's a good idea to have two active connections to your server before restarting the SSH server. Having that extra connection allows you to fix anything should the restart go wrong.

``` bash
sudo service sshd restart
```

## Enable a firewall

Now you need to install a firewall, enable it, and configure it only to allow network traffic that you designate. [Uncomplicated Firewall (UFW)](https://en.wikipedia.org/wiki/Uncomplicated_Firewall) is an easy-to-use interface to iptables that greatly simplifies the process of configuring a firewall.

You can install UFW with:

``` bash
sudo apt install ufw --assume-yes
```

By default, UFW blocks all incoming connections and allows all outgoing connections. This means any application on your server can reach the internet, but anything trying to reach your server cannot connect.

First, make sure you can log in by enabling access to SSH, HTTP, and HTTPS:

``` bash
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
```

Then enable UFW:

``` bash
sudo ufw enable
```

You can see what services are allowed and denied with:

``` bash
sudo ufw status
```

If you ever want to disable UFW, you can do so by typing:

``` bash
sudo ufw disable
```

## Install Fail2ban

[Fail2ban](https://en.wikipedia.org/wiki/Fail2ban) is an application that examines server logs looking for repeated or automated attacks. If any are found, it will alter the firewall to block the attacker's IP address either permanently or for a specified amount of time.

You can install Fail2ban by typing:

``` bash
sudo apt install fail2ban --assume-yes
```

Then copy the included configuration file:

``` bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

And restart Fail2ban:

``` bash
sudo service fail2ban restart
```

That's all there is to it. The software will continuously examine the log files looking for attacks. After a while, the app will build up quite a list of banned IP addresses. You can view this list by requesting the current status of the SSH service with:

``` bash
sudo fail2ban-client status sshd
```

## Ignore closing laptop lid

One thing you'll need to fix is if you want the laptop to stay on even if you close the lid down.

To disable entering the sleep mode edit `/etc/systemd/logind.conf` using your text editor of choice and ensure this line:

``` bash
# HandleLidSwitch=suspend
```

look like this:

``` bash
HandleLidSwitch=ignore
```

Then Save the file (`Ctrl-X / Y / Return`) and restart the login daemon:-

``` bash
sudo service systemd-logind restart
```

## Final thoughts

This tutorial presents the bare minimum needed to harden a Linux server. Additional security layers can and should be enabled depending on how a server is used. These layers can include things like individual application configurations, intrusion detection software, and enabling access controls, e.g., two-factor authentication.

That's it for today, next we'll set-up an external Hard drive to hold all our data. If you have any questions or feedback, feel free to leave a comment below. And don't forget to subscribe to the [RSS](/index.xml) so you'll know when I post the next exciting installment.

[^1]: [Ed25519](https://linux-audit.com/using-ed25519-openssh-keys-instead-of-dsa-rsa-ecdsa/) In public-key cryptography, Edwards-curve Digital Signature Algorithm (EdDSA) is a digital signature scheme using a variant of Schnorr signature based on twisted Edwards curves. It is designed to be faster than existing digital signature schemes without sacrificing security.
[^2]: [MITM](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) In cryptography and computer security, a man-in-the-middle attack is a cyberattack where the attacker secretly relays and possibly alters the communications between two parties who believe that they are directly communicating with each other, as the attacker has inserted themselves between the two parties.
