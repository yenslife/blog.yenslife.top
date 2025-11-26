+++
date = '2025-11-26T02:04:48+08:00'
draft = false
title = '在 MacOS 中如果意外把 /etc/pam.d/sudo 的東西改壞怎麼辦？(在 MacOS 中使用指紋來驗證 sudo)'

tags = [ 'sudo', 'Mac', '指紋' ]
image = 'https://i.meee.com.tw/CzGJ70z.png'
categories = [ 'Mac' ]
keywords = [ 'sudo', 'Mac', '指紋', 'pam', 'pam_tid' ]
description = '在 MacOS 中如果意外把 /etc/pam.d/sudo 的東西改壞怎麼辦？(在 MacOS 中使用指紋來驗證 sudo 遇到的問題)'

+++

## 前情提要

我一開始是因為看到這個影片，作者 Chuck 提到的最後一個 MacOS terminal trick

{{< youtube qOrlYzqXPa8>}}

他說可以透過修改 `/etc/pam.d/sudo`，加入 `auth sufficent pam_tid.so` 來讓之後需要輸入 sudo 的地方都可以使用指紋來驗證，超帥超快的，特別是如果你的密碼很長，應該會輸入到很躁

```bash
cat /etc/pam.d/sudo
# sudo: auth account password session
auth sufficient     pam_tid.so
auth       include        sudo_local
auth       sufficient     pam_smartcard.so
auth       required       pam_opendirectory.so
account    required       pam_permit.so
password   required       pam_deny.so
session    required       pam_permit.so
```

但好笑的是，我不小心把 `pam_tid.so` 寫成 `pam_unix.so` (有時候用 GitHub copilot 還是要注意呀QQ)

導致我存檔離開並打算用 `sudo` 的時候，出現 `sudo: unable to initialize PAM: No such file or directory`，意思就是 pam 設定檔錯誤，然後想要改這個檔案又需要 sudo 的權限，所以哈哈，尷尬了，悲劇了，完蛋了

## 解決方法

跟 Perplextiy 聊過之後，才知道這種系統層級的東西壞掉，在 MacOS 上可以透過「恢復模式」來修，在這個模式下的終端機預設就是 super user，所以只要用這個模式把 `/etc/pam.d/sudo` 改好就好了！

以下是步驟

1. 關機
2. 按住電源鍵不放，直到出現「繼續按住來進入選項」
3. 這時候你可以選擇你的磁碟和選項，當然是要選擇「選項」，然後按下繼續

Perplexity 跟我說可以用「磁碟工具程式」來選取掛載 `/etc` 的磁碟，然後進入終端機用 vi 去改。我後來發現其實不用這樣，這一步應該只是要確定這個 Volume 的名字而已，我最後是用以下命令來改

至於進入終端機的方式，你可以看到螢幕最上方繪有一條 menu bar，那邊有一個地方可以開啟終端機

```bash
$ vi /Volumes/Macintosh\ HD/etc/pam.d/sudo
```

改好存檔之後，重新開機就可以用指紋來驗證 sudo 啦！！超酷的

![系統要求使用指紋來驗證 sudo 的截圖](https://i.meee.com.tw/CzGJ70z.png)

