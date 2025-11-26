+++
title = 'Mac (intel x86), Linux Dual Boot, Ubuntu Partition Resize Record'
date = 2023-02-13T17:57:12+08:00
draft = false
tags = ['Ubuntu', 'Linux', 'Partition', 'Disk', 'Mac']
categories = ['Linux']
keywords = ['Linux', 'Ubuntu', 'Disk', 'partition', 'mac', 'old-blog-post']
description = 'Complete record of installing Ubuntu dual boot on MacBook Pro 2013 and performing disk partition resizing. Includes creating Live USB, using GParted partition tool, and solving partition resize issues.'
excerpt = 'Complete record of installing Ubuntu dual boot on MacBook Pro 2013 and performing disk partition resizing. Includes detailed steps for creating Live USB and using GParted partition tool.'
image = 'https://i.imgur.com/Q90BaZ4.png'
toc = true
comments = true
author = '海狸大師'
authorLink = 'https://github.com/yenslife'
+++

(Old blog post, written: 2023-02-13)

(Important note: I actually broke my Mac OS with this process. This article records how I broke it. Please be very careful when operating on disks, and also how I solved it.)

## Background

Recently, I needed to use an x86 architecture computer for compilation, but my main machine is ARM architecture. I tried using UTM virtual machine on my M1 computer, but the performance was quite poor. So I installed Ubuntu dual boot on another old computer (MacBook Pro 2013) and used it as a server by plugging it in on my desk. It worked quite well. I'm recording this here so I can come back and check if I forget later.

Dual boot reference: [How to install Linux system on MacBook Pro? - Zhihu](https://www.zhihu.com/question/348950956) (Chinese article)

Although I had already backed up my home directory to the new laptop using AirDrop, I still want to share how I broke my Mac.

Initially, I only partitioned about 30GB of disk space, but when doing cross-platform compilation of Linux kernel, I found the space wasn't enough. So I wanted to allocate some space from the Mac side. I used the method from the article above to partition, but when I used `Option + Power Key` to change the boot disk, I found that the Mac disk had disappeared.

At that moment, I wasn't too panicked (after all, I had broken and reinstalled it once before). Instead, I was excited - finally had an excuse to focus properly on Linux. So I directly wiped out the Mac partition and turned the entire laptop into Linux~

## Resizing Partition

Environment:
Hardware: MacBook Pro 2013
Operating System: Ubuntu 20.04 x86

If you forgot to backup, you can try using [Disk Drill](https://www.cleverfiles.com/) to recover data. (But not guaranteed to recover)

The problem I encountered was that the partition couldn't be resized. I later discovered that this is actually because we can't modify the disk while running on the disk you want to resize (a bit convoluted - you can't partition yourself). So we need to use a live USB boot environment to make the adjustments. Here's the complete process.

### Creating Live USB and Booting

If you don't know how to create a live USB, you can use [balenaEtcher](https://www.balena.io/etcher). There are already many tutorials about it online, so I won't go into detail here. The best part is that it can be used on Mac!

After creating it, shut down the power, plug in your USB, hold `option + Power Key`, select this USB (usually golden), then keep pressing Enter, and select `try Ubuntu`.

### Using GParted for Partitioning

Enter the command `sudo apt install gparted` to install, then you'll see it in your applications.

![](https://i.imgur.com/L070nGq.png)

My root directory is at `/dev/sda4`, the 31.55GB one.
![](https://i.imgur.com/FlL6Mzc.png)

Right-click on the partition you want to move, select Resize/Move.
![](https://i.imgur.com/AOUnRwJ.png)

Now you can move freely. Move your disk to the left, so the unallocated space will be moved to the right, allowing you to expand freely.
![](https://i.imgur.com/TNOogmi.png)

Use `df -l` to check disk usage, and you can see it really got bigger (this is a screenshot from SSH connection on another laptop).
![](https://i.imgur.com/wEDEBBi.png)

And let me record that I really wiped out the old Mac stuff. jserv said that to learn Linux well, the fastest way is to install Linux entirely. I think I have some [GUTS](https://www.youtube.com/c/GUTS4tech) too 😎
![](https://i.imgur.com/vQeUI1J.png)

References:
[partitioning - How to merge partitions? - Ask Ubuntu](https://askubuntu.com/questions/66000/how-to-merge-partitions)
[ubuntu - Resizing the root partition using free space - Super User](https://superuser.com/questions/1712492/resizing-the-root-partition-using-free-space)
[Resize or Extend a Linux Partition/Volume/Disk (Swap - Ubuntu - Gparted) - YouTube](https://www.youtube.com/watch?v=Kyz9x71gEPI&ab_channel=SavvyNik)
