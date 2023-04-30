---
title: "Adding An External DATA HDD"
author: mort8088
type: page
date: 2023-05-01T00:00:00+00:00
cover: "images/BashLogo.webp"
description: "Keeping all the config data and the actual stored data away from the main OS drive makes for a headache free upgrade path when you accidentally kill your server, and you will kill the server at some point."
toc: false
readingTime: false
hideComments: false
---

Keeping all the config data and the actual stored data away from the main OS drive makes for a headache free upgrade path when you accidentally kill your server, and you *will* kill the server at some point.

First off we need to check the `/media` folder to see that there is't a mount point that exists already with the name we're going to use:-

``` bash
ls /media
```

Then if there isn't one called `data` then we can go ahead and create one :-

``` bash
sudo mkdir /media/data
```

Next we need the UUID of the Hard Disk Drive that we want to be mounted to find that we need to see the attached drives:-

``` bash
sudo blkid
```

> ``` bash
> /dev/sdb1: UUID="7231ab44-4e94-474a-909f-bf611b61f4c6" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="7484dbf7-01"
> /dev/mapper/ubuntu--vg-ubuntu--lv: UUID="c122359c-473a-4db4-972d-3db9f4402b60" BLOCK_SIZE="4096" TYPE="ext4"
> /dev/sda2: UUID="c784b8fd-fd0e-46ee-bc5b-5a2bb0f79aa2" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="abb0849e-7195-4e88-b74d-cdf459fe4f5b"
> /dev/sda3: UUID="eVtsIb-ZzCz-lZeg-oGAp-Fgpt-5mAE-IMFMLF" TYPE="LVM2_member" PARTUUID="e81e1d16-47ae-425d-b72c-dbbc55921c1a"
> ```

From this output you are looking for the `UUID` of the drive ***NOT*** the `PARTUUID` make a note of this. You'll also want to check the `TYPE` of the drive so we can add that to the file system table file.

Next we want to edit the `fstab` file to mount the drive when we reboot the server:-

``` bash
sudo nano /etc/fstab
```

> ``` bash
> # /etc/fstab: static file system information.
> #
> # Use 'blkid' to print the universally unique identifier for a
> # device; this may be used with UUID= as a more robust way to name devices
> # that works even if disks are added and removed. See fstab(5).
> #
> # <file system> <mount point>   <type>  <options>       <dump>  <pass>
> # / was on /dev/ubuntu-vg/ubuntu-lv during curtin installation
> /dev/disk/by-id/dm-uuid-LVM-tGN23kHJzuXRdnvEB7pxGjSOx2JpXSh23mcOjsGqreFFk2KULJvcqqtOl0coNVqg / ext4 defaults 0 1
> # /boot was on /dev/sda2 during curtin installation
> /dev/disk/by-uuid/c784b8fd-fd0e-46ee-bc5b-5a2bb0f79aa2 /boot ext4 defaults 0 1
> /swap.img       none    swap    sw      0       0
> ```

At the bottom of the file add a new line with the following:

``` bash
UUID=<UUDI_OF_THE_NEW_DRIVE>    /media/data     <DRIVE_TYPE>    defaults    0   0
```

Use a tab between each of the options. save the file (`Ctrl-X / Y / Return`)

And before we reboot the server we need to test that it works by calling:-

``` bash
sudo mount -a
```

Once the drive has been mounted you can check the contents with:-

``` bash
ls /media/data
```

And reboot or not the drive is working now it's up to you.

That's it for today's tutorial. Next we'll set-up docker. If you have any questions or feedback, feel free to leave a comment below. And don't forget to subscribe to the blog so you'll know when I post the next exciting installment.
