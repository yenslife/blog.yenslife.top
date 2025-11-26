+++
title = 'Mac (intel x86), Linux 雙系統、Ubuntu 切割硬碟紀錄'
date = 2023-02-13T17:57:12+08:00
draft = false
tags = ['Ubuntu', 'Linux', 'Partition', 'Disk', '硬碟', 'Mac']
categories = ['Linux']
keywords = ['Linux', 'Ubuntu', 'Disk', 'partition', '分割', 'mac', '舊網站文章']
description = '在 MacBook Pro 2013 上安裝 Ubuntu 雙系統，並進行硬碟分割區調整的完整紀錄。包含製作 Live USB、使用 GParted 分割工具，以及解決分割區無法調整大小的問題。'
excerpt = '在 MacBook Pro 2013 上安裝 Ubuntu 雙系統，並進行硬碟分割區調整的完整紀錄。包含製作 Live USB、使用 GParted 分割工具的詳細步驟。'
image = 'https://i.imgur.com/Q90BaZ4.png'
toc = true
comments = true
author = '海狸大師'
authorLink = 'https://github.com/yenslife'
+++

（舊網站文章，撰寫時間：2023-02-13）

（重要事項：我已經把我的 mac 作業系統弄壞了，這篇是紀錄我怎麼弄壞的，請大家操作硬碟一定要小心謹慎，還有我是怎麼解決的）

## 前情提要
由於最近需要用到 x86 架構的電腦編譯，但我的主力機是 arm 架構，我試過在 M1 電腦用 UTM 虛擬機，但效能有點慘，所以在另一台舊電腦 (MacBook pro 2013) 灌 Ubuntu 雙系統並且放在桌上插電當 server 用，挺好用的。這邊記錄一下以免日後忘記可以回來看哈。

雙系統參考：[如何在MacBook Pro上安装Linux系统？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/348950956) 這篇文章

雖然我已經用 AirDrop 將家目錄備份到新筆電了，但我還是想分享一下我是如何把 Mac 弄壞的。

當初，我只切割了 30G 左右的硬碟空間，但在做跨平台編譯 Linux kernel 時，發現空間不夠用，於是我想要從 Mac 那邊切一些空間過來。我是用上面那篇文章的方式來切的，但當我用 `Option + 開機鍵` 更改開機啟動磁碟時，發現 Mac 硬碟消失了。

當下，我並沒有太慌 (畢竟已經弄壞重灌過一次了)，反而很興奮，終於有藉口好好專注在 Linux 上，於是我直接擦掉 Mac 的分割區，把整個筆電弄成 Linux 了～

## 調整分割區大小
環境：
硬體：MacBook Pro 2013
作業系統：Ubuntu 20.04 x86

如果忘記備份，可以試著用 [Disk Drill](https://www.cleverfiles.com/) 把資料挖回來。(但不一定挖得回來)

我遇到的問題是分割區無法調整大小，後來發現這其實是因為我們不能在你要調整大小的硬碟中修改你的硬碟 (有點拗口，就是你不能切自己)，所以我們需要用 live usb 開機的環境來調整。以下介紹完整流程。

### 製作 Live USB 並開機
不知道怎麼製作 live usb 可以使用 [balenaEtcher](https://www.balena.io/etcher) ，網路上已經有很多關於他的教學了這邊就不多提了，最棒的是他可以在 Mac 上面用喔！

弄好之後將電源關閉，插上你的 USB，按住 `option + 電源鍵`，選擇這個 USB（通常是金色的），然後就一直 Enter 下去，選擇 `try Ubuntu`

### 使用 GParted 分割

輸入指令 `sudo apt install gparted` 安裝，然後你就會看到他在應用程式中。

![](https://i.imgur.com/L070nGq.png)

我的根目錄是在 `/dev/sda4` 這邊，31.55GB 那個
![](https://i.imgur.com/FlL6Mzc.png)

右鍵選擇你要移動的分割區，點選 Resize/Move
![](https://i.imgur.com/AOUnRwJ.png)

這時候就可以自由移動了，把你的硬碟移到左邊，所以 unallocated 的空間會被移到右邊，這樣就可以自由擴充了
![](https://i.imgur.com/TNOogmi.png)


使用 `df -l` 查看硬碟使用情形，可以看到真的變大ㄌ (以下是在另一台筆電 ssh 連線使用的畫面)
![](https://i.imgur.com/wEDEBBi.png)


然後記錄一下我真的把舊 Mac 的東西擦掉了，jserv 說過要好好學習 Linux 就整個都灌 Linux 最快，我想我也是有一點 [GUTS](https://www.youtube.com/c/GUTS4tech) 的😎
![](https://i.imgur.com/vQeUI1J.png)


參考資料：
[partitioning - How to merge partitions? - Ask Ubuntu](https://askubuntu.com/questions/66000/how-to-merge-partitions)
[ubuntu - Resizing the root partition using free space - Super User](https://superuser.com/questions/1712492/resizing-the-root-partition-using-free-space)
[Resize or Extend a Linux Partition/Volume/Disk (Swap - Ubuntu - Gparted) - YouTube](https://www.youtube.com/watch?v=Kyz9x71gEPI&ab_channel=SavvyNik)
