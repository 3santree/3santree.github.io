+++
title = 'Configure Virtualbox on arch'
date = 2023-02-10T17:23:21-05:00
draft = false
categories = ['Linux']
tags = ['virtualmachine', 'arch']
comment = false
+++

# Why virtualbox instead of vmware?

For me on arch, I tried my best to make the vmware-workstation to work, which eventually failed after a couple of hours trying to troubleshoot the problem. However, the virtualbox works prefectly fine just after few mins installing, and here's how I did it. So the answer is simple...

<!--more-->

# What do I want?

Following the book **\<How to hack like a ghost\>**, it requires me to boot up a VPS, to simulate that, I think I could boot up ubuntu server as virtual machine, and **ssh to the ubuntu from my host machine** just like what we normally do to the VPS. 

# Download VirtualBox

```
sudo pacman -S virtualbox virtualbox-guest-iso
# choose virtualbox-host-module-arch because my kernel is not LTS version
sudo gpasswd -a $USER vboxusers
sudo modprobe vboxdrv
sudo systemctl enable vboxweb.service
sudo systemctl start vboxweb.service
# Download the extension pack from virtualbox's offical website, then open virtualbox manually install it. 
```

# Configure Network

To make out ubuntu guest machine **1. have internet** and **can communicate to our host machine**, we need to set two network adapter for ubuntu.

1. NAT (Internet access, but cannot communicate to host)
2. Host-only (No internet access, but can communicate to host)

After booting up the ubuntu, I find that the new network adapter **enp0s8** for Host-only  is showing up without ip address. We need to enable the dhcp for that on ubuntu

```
# For ubuntu 22, this is the config file for network
sudo vim /etc/netplan/00-installer-config.yaml
# Add the new adapter to here, enp0s3 is for the NAT
ethernets:
    enp0s3:
      dhcp4: true
    enp0s8:
      dhcp4: true
# Save and quit
sudo netplan try
# now enp0s8 has a ip address
``` 

Let's try SSH to ubuntu, bang! I am in.














