---
title: "How to use a broadcom fingerprint reader on arch linux"
date: 2024-07-05T19:58:00+02:00
draft: false
toc: false
images:
tags:
  - linux
  - archlinux
  - arch
  - broadcom
  - security
---

Hey everyone, i'm back on my blog with a guide!

Recently, with windows 11 making my new laptop that has been **manifactured in 2024 and is officially compatible with windows 11** overheat and with Microsoft deciding to bloat windows *even more*, i switched to arch linux and i have to say, **it made everything better**.

However, i have a fingerprint reader in the laptop that i'm using right now to write this post, and it's a broadcom fingerprint reader, which is not officially supported by fprint, so i found a package on the AUR to make it work but unfortunately, it always gave me an error whenever i tried to install it and it always failed on the "update-fw" step.

So today, i decided to make a guide to teach you, how to make it work.

First of all, we need [libfprint-2-tod1-broadcom](https://aur.archlinux.org/packages/libfprint-2-tod1-broadcom) from the AUR (**this package requires [libfprint-tod-git](https://aur.archlinux.org/packages/libfprint-tod-git) already installed!**)

To do this we paste the following commands:

```
git clone https://aur.archlinux.org/libfprint-2-tod1-broadcom.git
cd libfprint-2-tod1-broadcom
```

After we do this, we have to modify the PKGBUILD file by removing these two lines:

```
# firmware
install -Dm 755 var/lib/fprint/fw/* "$pkgdir/var/lib/fprint/fw/"
```

Now we can install the package by typing:

```
makepkg -si
```

Wait for the package to compile and install itself, after it's installed, we need to clone this repository by pasting these commands:

```
git clone https://git.launchpad.net/~oem-solutions-engineers/libfprint-2-tod1-broadcom/+git/libfprint-2-tod1-broadcom/
cd libfprint-2-tod1-broadcom
```

Now, we need to install the firmware, in order to do that, we need to execute these commands:

```
sudo cp -r usr/* /usr/
sudo cp -r var/* /var/
```

After that, we need to edit the "fprintd.service" file

```
sudo nano /usr/lib/systemd/system/fprintd.service
```

We need to add two lines in this file, go at the end of the file and add this:

```
[Install]
WantedBy=multi-user.target
```

Then execute:

```
sudo systemctl daemon-reload
sudo systemctl enable fprintd
sudo systemctl start fprintd
```

Now reboot your system and go in user settings, if there's a fingerprint login section, click on it, and if it makes you add a fingerprint then you followed the guide correctly

Now i'll return to setting up my homelab, buh-bye! 
