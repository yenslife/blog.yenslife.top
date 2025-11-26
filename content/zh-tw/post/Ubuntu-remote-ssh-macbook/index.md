+++
title = 'Mac 遠端 ssh 連線到 Ubuntu'
date = 2023-01-27T00:21:03+08:00
draft = false
tags = ['Ubuntu', 'Linux', 'ssh', '連線']
categories = ['Linux']
keywords = ['Ubuntu', 'Linux', 'ssh', '連線', '舊網站文章']
description = '因為有案子需要用到 x86 架構的電腦編譯，但我的主力機是 arm 架構，我試過在 M1 電腦用 UTM 虛擬機，但效能有點慘，所以在另一台舊電腦 (MacBook pro 2013) 灌 Ubuntu 雙系統並且放在桌上插電當 server 用。這篇文章記錄 Mac 遠端 SSH 連線到 Ubuntu 的完整設定方法。'
excerpt = '記錄 Mac 遠端 SSH 連線到 Ubuntu 的設定方法，包含安裝 SSH 服務、解決連線問題，以及設定筆電蓋上不休眠等實用技巧。'
image = "https://images.unsplash.com/photo-1629654291663-b91ad427698f?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8dWJ1bnR1fGVufDB8fDB8fA%3D%3D&auto=format&fit=crop&w=800&q=60"
toc = false
comments = true
author = '海狸大師'
authorLink = 'https://github.com/yenslife'
+++

（舊網站文章，撰寫時間：2023-01-27）

因為有案子需要用到 x86 架構的電腦編譯，但我的主力機是 arm 架構，我試過在 M1 電腦用 UTM 虛擬機，但效能有點慘，所以在另一台舊電腦 (MacBook pro 2013) 灌 Ubuntu 雙系統並且放在桌上插電當 server 用，挺好用的。這邊記錄一下以免日後忘記可以回來看哈。
環境:MacBook Pro 2021, Ubuntu 20.04

## 連線

以下是連接同一個網路的方法

1. 安裝連線工具

```bash
sudo apt install openssh-server
```

2. 在 Ubuntu 上啟動 ssh

```
sudo /etc/init.d/ssh start
```

或者

```
sudo systemctl start ssh # 啟動 ssh
sudo systemctl stop ssh # 停止 ssh
sudo systemctl status ssh # 查看目前 ssh 狀態
```

最後開啟 mac 的終端機，輸入 `ssh 你在Ubuntu的username@你的ip` ，不知道 IP 可以輸入 `hostname -I` 查看(對外 ip 可以用 `curl ipinfo.io`)

<br>

如果要對外連線又不想設定對外 ip，我推薦 [tmate](https://tmate.io/) ，你可以把它理解成 terminal 版本的 [teamviewer](https://www.teamviewer.com/)，挺方便的，我自己的做法是要出門前開幾個 tmate sessions，然後把 id 記在 [Notion](https://www.notion.so/) 裡。
設定對外 ip 通常只要登入路由器的 Web 介面，通常是輸入網址 192.168.0.1 或 192.168.1.1，然後看你是用什麼路由器再去看他的說明書設定

想使用外網 IP 來連線的話，記得要設定 Port forwarding，然後將 `/etc/ssh/sshd_config` 中的 `#Port` 註解拿掉，改成你要的 port。

```shell
vim /etc/ssh/sshd_config
```
儲存退出之後，重啟 ssh 服務

```shell
service sshd restart  
```
## 一些可能會有的問題

### 如果在 Mac 那邊連線時出現類似以下的訊息

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

會這樣可能是因為之前有用這個 IP 連線過導致的 (我就是這樣)，這時候只要用 `ssh-keygen -R 你的ip` 就可以了，你也可以到 `~/.ssh/known_host` 把有問題的 ip 刪掉。

### 筆電蓋上後會自動休眠而無法連線

因為我只會 ssh 到這台舊筆電，所以用不太到螢幕，每次都要螢幕半開也挺麻煩的

1.修改 Ubuntu 登入管理配置檔案，將 `#HandleLidSwitch=suspend` 改成 `HandleLidSwitch=ignore`

```
sudo vim /etc/systemd/logind.conf # 進到編輯器修改 HandleLidSwitch 那一行
```

2.重啟服務

```
sudo systemctl restart systemd-logind
```
