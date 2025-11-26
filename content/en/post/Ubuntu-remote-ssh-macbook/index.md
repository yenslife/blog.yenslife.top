+++
title = 'Mac Remote SSH Connection to Ubuntu'
date = 2023-01-27T00:21:03+08:00
draft = false
tags = ['Ubuntu', 'Linux', 'ssh', 'connection']
categories = ['Linux']
keywords = ['Ubuntu', 'Linux', 'ssh', 'connection', 'old-blog-post']
description = "Because I needed to use an x86 architecture computer for compilation, but my main machine is ARM architecture, I tried using UTM virtual machine on M1 computer, but the performance was quite poor. So I installed Ubuntu dual boot on another old computer (MacBook Pro 2013) and used it as a server by plugging it in on my desk. This article records the complete setup method for Mac remote SSH connection to Ubuntu."
excerpt = "Recording the setup method for Mac remote SSH connection to Ubuntu, including installing SSH service, solving connection problems, and setting laptop lid not to sleep."
image = "https://images.unsplash.com/photo-1629654291663-b91ad427698f?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8dWJ1bnR1fGVufDB8fDB8fA%3D%3D&auto=format&fit=crop&w=800&q=60"
toc = false
comments = true
author = '海狸大師'
authorLink = 'https://github.com/yenslife'
+++

(Old blog post, written: 2023-01-27)

Because I needed to use an x86 architecture computer for compilation, but my main machine is ARM architecture, I tried using UTM virtual machine on M1 computer, but the performance was quite poor. So I installed Ubuntu dual boot on another old computer (MacBook Pro 2013) and used it as a server by plugging it in on my desk. It worked quite well. I'm recording this here so I can come back and check if I forget later.
Environment: MacBook Pro 2021, Ubuntu 20.04

## Connection

Below are methods for connecting to the same network

1. Install connection tools

```bash
sudo apt install openssh-server
```

2. Start SSH on Ubuntu

```
sudo /etc/init.d/ssh start
```

Or

```
sudo systemctl start ssh # Start SSH
sudo systemctl stop ssh # Stop SSH
sudo systemctl status ssh # Check current SSH status
```

Finally, open Mac's terminal and type `ssh your-ubuntu-username@your-ip`. If you don't know the IP, you can type `hostname -I` to check (for external IP, you can use `curl ipinfo.io`)

<br>

If you want to connect externally but don't want to set up external IP, I recommend [tmate](https://tmate.io/). You can think of it as the terminal version of [TeamViewer](https://www.teamviewer.com/), which is quite convenient. My approach is to start several tmate sessions before going out, then save the IDs in [Notion](https://www.notion.so/).

Setting up external IP usually just requires logging into your router's web interface, typically by entering the address 192.168.0.1 or 192.168.1.1, then check what router you're using and refer to its manual for configuration.

If you want to use external IP to connect, remember to set up Port forwarding, then remove the `#Port` comment in `/etc/ssh/sshd_config` and change it to your desired port.

```shell
vim /etc/ssh/sshd_config
```
After saving and exiting, restart SSH service

```shell
service sshd restart  
```

## Some Possible Issues

### If you see messages similar to the following when connecting from Mac

```text
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ED25519 key sent by the remote host is
SHA256:blablablablablablablablablablablablablablab.
Please contact your system administrator.
Add correct host key in /Users/username/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /Users/username/.ssh/known_hosts:5
Host key for 192.168.1.137 has changed and you have requested strict checking.
Host key verification failed.
```

This might happen because you've used this IP to connect before (that's what happened to me). In this case, just use `ssh-keygen -R your-ip` to fix it, or you can go to `~/.ssh/known_host` and delete the problematic IP.

### Laptop automatically sleeps when lid is closed, making it unable to connect

Since I only SSH to this old laptop, I don't really need the screen, and it's quite troublesome to keep it half-open every time.

1. Modify Ubuntu login management configuration file, change `#HandleLidSwitch=suspend` to `HandleLidSwitch=ignore`

```
sudo vim /etc/systemd/logind.conf # Enter editor to modify the HandleLidSwitch line
```

2. Restart service

```
sudo systemctl restart systemd-logind
```
