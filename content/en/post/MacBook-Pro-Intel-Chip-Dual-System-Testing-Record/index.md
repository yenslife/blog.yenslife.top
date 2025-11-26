+++
title = 'MacBook Pro Intel Chip Dual System Testing Record'
date = 2023-12-20T22:13:46+08:00
draft = false
tags = ['reinstall', 'Mac', 'Windows', 'Dual System', 'Intel']
categories = ['Mac']
keywords = ['Mac', 'Windows', 'Dual System', 'reinstall', 'old-blog-post']
description = "My old laptop MacBook Pro late 2013 was recently converted to pure Ubuntu, but recently due to assignment requirements (microcomputer needs to use PuTTY), I needed a Windows environment for convenience. This article records my experiences from the past few days. The following will explain steps and potential problems, hoping future me won't fall into the same traps."
excerpt = 'My old laptop MacBook Pro late 2013 was recently converted to pure Ubuntu, but recently due to assignment requirements (microcomputer needs to use PuTTY), I needed a Windows environment for convenience. This article records my experiences from the past few days.'
image = 'https://i.imgur.com/8zKsUF4.png'
toc = true
comments = true
author = '海狸大師'
authorLink = 'https://github.com/yenslife'
+++

(Old blog post, written: 2023-12-20)

My old laptop MacBook Pro late 2013 was recently converted to pure Ubuntu, but recently due to assignment requirements (microcomputer needs to use PuTTY), I needed a Windows environment for convenience. This article records my experiences from the past few days.

The following will explain steps and potential problems, hoping future me won't fall into the same traps.

Don't forget to **backup**! The following is just my personal experience, it doesn't mean your operation results will be the same as mine, it can only be used as reference.

## Creating Windows 10 Boot Disk

Reference link: [How to create Windows boot disk using macOS - Mr. Mad](https://mrmad.com.tw/macos-makes-windows-refill-usb-flash-drive) (Chinese article)

I used UNetbootin to create the boot disk. Note that the creation process might take a very, very long time, especially when some files are several thousand MB, it might get stuck, but it's not broken, it just takes longer. Note: Don't use FAT32 due to file size issues.

### Problem Encountered: Windows cannot open required file C:\Sources\install.wim

Like the image below

![](https://i.imgur.com/tnEFMzu.png)

Later I found out that it was because I used FAT32 format for the USB, but in current Windows ISO, install.wim is over 4GB, so you should use a more suitable format to format the USB.

References: [Solution for Windows cannot open required file C:\Sources\install.wim during installation](https://www.zendei.com/article/79639.html) [USB formatting tutorial: How to choose between FAT32, NTFS, exFAT?](https://www.linwei.com.tw/forum-detail/60/)

### Problem Encountered: Installed Windows has no wireless network card, sound card...

This problem can be solved through recent Mac Boot Camp. Boot Camp comes with some drivers for you. But I didn't use Boot Camp, instead I partitioned a space myself to install. The first time I installed, I didn't connect to the network. The second time I connected the computer to wired network and did some settings, it worked.

### Problem Encountered: Windows says your disk cannot be installed

It might be because of the format it doesn't accept. At this time, just select the disk you want, then press delete.

![](https://i.imgur.com/NC6dBi2.png)

### Problem Encountered: First boot is normal, but after shutdown, Windows cannot boot, showing "No bootable device -- insert boot disk and press any key"

As shown in the image below

![](https://i.imgur.com/SmgpMwh.png)

It might be because something was installed to the USB. You can try plugging in the USB to boot.

Below is before plugging in USB, pressing Windows shows it crashed.

![](https://i.imgur.com/rWhDI5B.png)

Below is after plugging in USB, you can see two more options. At this time, press Windows in front of EFI Boot to boot!

![](https://i.imgur.com/8zKsUF4.png)

Well... this is quite silly. My friend said I could reinstall it, but I'm tired, physically and mentally exhausted. As long as it can handle assignment needs, it's fine hahaha

(2023/12/24 update) Later I updated Mac to MacOS Mojave, and it could boot normally. I don't know why, but I guess it's because I updated the Mac version, so it updated some things, and then it could boot normally)

## What to do if it's completely broken?

Just reinstall Mac! The reason I reinstalled this Mac is because I have no attachment to it, but it's still my good partner, so I want to write this article to commemorate it.

You can hold the power button to force shutdown, wait a few seconds, then press `Command + r` and press the power button. It will enter recovery mode. The first time it might show this screen, requiring wireless network connection.

![](https://i.imgur.com/SnPJjGo.png)

Then press reinstall macOS. At this time, you should only be able to install the ancient version OSX Mavericks. For later updates, you can refer to [os x 10.9.5 cannot update version - Apple Community](https://discussionschinese.apple.com/thread/140127319?sortBy=best) to download .dmg files from the website and install new OS.

Below are some installation screens

![](https://i.imgur.com/hmhVRUx.png)

In recovery mode, you can use Disk Utility to partition or format as needed.

![](https://i.imgur.com/B6KBBg6.png)

OSX Mavericks boot screen, it's the white apple!

![](https://i.imgur.com/tGZWbTg.png)

OSX Mavericks is super ancient

![](https://i.imgur.com/CDrQXC2.png)

Almost no programs, relatively new websites can be used, even Edge cannot be installed, ridiculous

![](https://i.imgur.com/rNM6UKc.png)

### Manual MacOS Update
Refer to [os x 10.9.5 cannot update version - Apple Community](https://discussionschinese.apple.com/thread/140127319?sortBy=best) to download .dmg files from the website and install new OS.
(2023/12/25 update) If you encounter encryption issues during installation updates like "You cannot install on this volume because it is currently being encrypted," you can refer to [this](https://discussionschinese.apple.com/thread/140127092?sortBy=best).

(2023/12/25 update) You can use [this](https://support.apple.com/zh-tw/102465) to install Windows support software. After installation, remember to check if there are updates in Apple Software Update on Windows side. If there are, update them. If it's still weird, try running setup.exe a few more times.

![](https://i.imgur.com/k5Ow6KS.jpg)

![](https://i.imgur.com/q5ZO5Q9.jpg)

(2023/12/25 update) If there's no sound, try switching audio devices.

![](https://i.imgur.com/oIVaB1D.jpg)

#### Can you partition hard disk in OSX?

You can refer to this video: [https://youtu.be/Z8-jm17YBt8?si=ClYQ_Jl982qF2GfW](https://youtu.be/Z8-jm17YBt8?si=ClYQ_Jl982qF2GfW)

    Wish everyone's reinstallation journey goes smoothly, especially Mac users. In this world where Microsoft is the biggest daddy, we need to be self-reliant and strong.
