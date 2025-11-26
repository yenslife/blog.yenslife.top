+++
title = 'Using Lima on Mac as Linux Environment'
date = 2022-07-05T18:35:07+08:00
draft = false
tags = ['Lima', 'Mac', 'Linux', 'picoCTF']
categories = ['Linux']
keywords = ['lima', 'Linux', 'picoCTF', 'Mac', 'old-blog-post']
description = "Sharing my experience with installing and using lima, along with the problems I encountered. Because I recently wanted to try picoCTF and needed a Linux environment, I decided to use lima as my Linux container. This article records the complete installation, usage, and problem-solving process."
excerpt = "Sharing my experience with installing and using lima, along with the problems I encountered. Including setting up Linux environment on Mac and solving read-write permission issues."
image = 'https://i.imgur.com/Ra8Ur5T.png'
toc = true
comments = true
author = '海狸大師'
authorLink = 'https://github.com/yenslife'
+++

(Old blog post, written: 2022-07-05)

## Using Lima on Mac as Linux Environment
### Table of Contents
1. [Installation](#installation)
2. [Usage](#usage)
3. [Problem 1 (Read-write issue)](#problem-1-read-write-issue)

## Preface
Because I recently wanted to try picoCTF, I needed a Linux environment. Since I'm using macOS, which isn't Linux, although some commands are universal, there are significant differences in program execution results. So I decided to use lima as my Linux container. I had originally considered using VMWare virtual machine, but the thought of making this old MacBook Pro (late 2013) suffer every time I wanted to play CTF made me feel bad.

And that's exactly what happened - as soon as I started VMWare, my old partner's fan started whining again 🤧.

### Installation
I installed it using [homebrew](https://brew.sh/). Remember to download it from the [official website](https://brew.sh/). First, enter the following commands:
```bash=
$ brew install lima # Install lima
$ limactl start # Install virtual machine
```
When the following screen appears, use arrow keys up and down to select configuration. We just need to use the default, so press enter directly. When you see READY, it's completed!
```bash=
? Creating an instance "default"  [Use arrows to move, type to filter]
> Proceed with the default configuration
  Open an editor to override the configuration
  Exit
...
INFO[0111] READY. Run `lima` to open the shell.
```
After downloading is complete, you can run `lima uname -a` to confirm the virtual machine is running
```bash=
$ Linux lima-default 5.15.0-39-generic #42-Ubuntu SMP Thu Jun 9 23:42:32 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
```

### Usage
Simply type `lima` in the terminal to run it. To exit, just type `exit`.
(~~When I first installed it, I didn't know how to exit, so I used two terminal windows - one for lima and one for zsh. Don't be as silly as me qq~~)
If you're not using the virtual machine, remember to turn it off, otherwise it will silently eat up your memory. Type `lima stop [virtual-machine-name]` to close the virtual machine.

![Demo](https://i.imgur.com/4v2K8Fw.png)

As for the container part, I haven't researched it yet. This is lima's strong point - it's claimed to be able to become a macOS Subsystem Linux that replaces docker. But since I just need a convenient Linux environment to practice CTF, interested friends can refer to the [lima source code](https://github.com/lima-vm/lima).

### Problem 1 (Read-write issue)
If you want to use the `mkdir` command in lima, or simply edit a file with `vim`, you might see this line:
`lima mkdir cannot create directory Read-only file system`

When I was looking for information, I found [this Q&A](https://ithelp.ithome.com.tw/questions/10188327). I originally thought it was a system data problem, but my situation seemed different from his. Later I found this [lima Github issue](https://github.com/lima-vm/lima/issues/34) and discovered it was actually a [lima configuration file](https://github.com/lima-vm/lima/issues/34) problem. The following commands can be used in both zsh or bash. If you use them in lima shell, you'll find this file doesn't exist. Basically, just open Mac's original terminal. The solution is as follows:

1. Modify configuration file
```bash=
$ vim ~/.lima/default/lima.yaml # First edit the configuration file with vim.
# It's quite long so I won't paste it here. Find writable: null and change it to writable: true
writable: null -> writable: true
```

2. Restart lima
```bash=
$ limactl list  This command lists all running virtual machines. I saw one named default
$ limactl stop default  Stop it from running, default can be replaced with your virtual machine name
$ limactl start default  Same as above, default can be replaced with your virtual machine name
```

3. Congratulations, you succeeded!

### References
[lima source code](https://github.com/lima-vm/lima)
[Reference (lima issue 34)](https://github.com/lima-vm/lima/blob/41fd9cc6a1e2bac73666e1f2b11604c7c42227dc/pkg/limayaml/default.TEMPLATE.yaml#L33-L41)
[Reference (lima configuration file)](https://github.com/lima-vm/lima/issues/34)
