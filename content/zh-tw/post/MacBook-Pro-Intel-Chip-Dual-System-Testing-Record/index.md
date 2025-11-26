+++
title = 'Macbook pro (Intel 晶片)雙系統測試紀錄'
date = 2023-12-20T22:13:46+08:00
draft = false
tags = ['重灌', 'Mac', 'Windows', 'Dual System', '雙系統', 'Intel']
categories = ['Mac']
keywords = ['Mac', 'Windows', 'Dual System', '雙系統', '重灌', '舊網站文章']
description = '我的舊筆電 MacBook pro late 2013 前陣子被我弄成純 ubuntu，但最近因為作業需求 (微算機要用到 PuTTY)，需要一個 Windows 的環境會比較方便，這篇文章記錄我這幾天的心得。接下來會以步驟以及可能遇到的問題來做說明，希望未來的我不要踩到一樣的坑。'
excerpt = '我的舊筆電 MacBook pro late 2013 前陣子被我弄成純 ubuntu，但最近因為作業需求 (微算機要用到 PuTTY)，需要一個 Windows 的環境會比較方便，這篇文章記錄我這幾天的心得。'
image = 'https://i.imgur.com/8zKsUF4.png'
toc = true
comments = true
author = '海狸大師'
authorLink = 'https://github.com/yenslife'
+++

（舊網站文章，撰寫時間：2023-12-20）

我的舊筆電 MacBook pro late 2013 前陣子被我弄成純 ubuntu，但最近因為作業需求 (微算機要用到 PuTTY)，需要一個 Windows 的環境會比較方便，這篇文章記錄我這幾天的心得。

接下來會以步驟以及可能遇到的問題來做說明，希望未來的我不要踩到一樣的坑 ㄏㄚ

別忘記一定要**備份**喔，以下也只是我自己的經驗，不代表你的操作結果就會跟我一樣，只能當作參考。

## 製作 Windows 10 開機碟

參考連結：[如何用macOS 製作Windows 開機隨身碟，兩種方式輕鬆實現 - 瘋先生 (mrmad.com.tw)](https://mrmad.com.tw/macos-makes-windows-refill-usb-flash-drive)

我使用 UNetbootin 來製作開機碟，要注意的是製作的過程可能會非常非常的久，尤其是有些檔案幾千Mb 就會卡住，但他不是壞掉他只是比較久而已。注意：不要使用 FAT32，因為檔案大小的問題。

### 遇到問題：Windows 無法開啟所需的檔案 C:\Sources\install.wim

像是下圖

![](https://i.imgur.com/tnEFMzu.png)
後來查一下才發現，原來是因為我使用的 USB 格式是 FAT32，但現在 windows 的 iso 中，install.wim 就超過 4G 了，所以應該要用更合適的格式來格式化 USB

參考資料：[安裝系統時出現 Windows無法打開所需的文件 C:\Sources\install.wim 的解決辦法_ZenDei技術網路在線](https://www.zendei.com/article/79639.html) [隨身碟格式化教學：FAT32、NTFS、exFAT怎麼選？ - 凌威科技 (linwei.com.tw)](https://www.linwei.com.tw/forum-detail/60/)

### 遇到問題：裝好的 Windows 沒有無線網卡、音效卡….

這個問題可以透過這幾年 mac 的 boot camp 來解決，Boot camp 會附帶一些驅動程式給你。但我不是用 Boot camp，而是自己切一塊空間來裝。我第一次安裝時是沒有連接網路的，第二次將電腦接上有線網路，做一些設定就可以了。

### 遇到問題：Windows 說你的磁碟無法安裝

可能是因為格式他不接受，這時候只要選擇你要的磁碟，然後按下刪除就可以了。

![](https://i.imgur.com/NC6dBi2.png)
### 遇到問題：第一次開機正常，關機後就無法使用 Windows 開機，出現 No bootable device -- insert boot disk and press any key

如下圖

![](https://i.imgur.com/SmgpMwh.png)

可能是因為把一些東西裝到 USB 了，你可以試著插上 USB 開機看看。

以下是插上 USB 之前，按下 windows 會發現他掛掉了。

![](https://i.imgur.com/rWhDI5B.png)

以下是插上 USB 之後可以看到多了兩個選項，這時候按 EFI Boot 前面的 Windows 即可開機！


![](https://i.imgur.com/8zKsUF4.png)
恩…蠻瞎的，我朋友說可以再灌一次，但我累了 身心俱疲了，先可以應付作業需求就好哈哈哈哈

(2023/12/24 更新) 我後來把 mac 更新到 MacOS Mojave 之後，就可以正常開機了，不知道為什麼，但我猜應該是因為我更新了 mac 的版本，所以他會去更新一些東西，然後就可以正常開機了)

## 如果真的完全掛掉了怎麼辦？

就直接重灌 Mac 吧！我會重灌這台 Mac 就是因為我對他沒有絲毫的牽掛，但她依舊是我的好夥伴，所以要寫這篇文章紀念一下

你可以按住電源鍵，先強制讓他關機，等個幾秒之後按下 `Command + r` 然後按下電源鍵，會進到 recovery mode 他第一次可能會出現這個畫面，要連上無線網路。

![](https://i.imgur.com/SnPJjGo.png)

然後按下重新安裝 macOS，這時候應該只能選擇安裝古老版本的 OSX Mavericks ，之後樣更新的話可以參考 [os x 10.9.5 更新不了版本 - Apple 社区](https://discussionschinese.apple.com/thread/140127319?sortBy=best) 在網站上下載 .dmg 檔案，安裝新版 OS。



下面就是一些安裝的畫面

![](https://i.imgur.com/hmhVRUx.png)

在修復模式中可以用磁碟工具程式，看你要切割還是要格式化。


![](https://i.imgur.com/B6KBBg6.png)


OSX Mavericks 開機畫面，是白蘋果！


![](https://i.imgur.com/tGZWbTg.png)

OSX Mavericks 超古老

![](https://i.imgur.com/CDrQXC2.png)


幾乎什麼程式、比較新的網站都不能用，連 Edge 都不能裝 笑死


![](https://i.imgur.com/rNM6UKc.png)

### 手動更新 MacOS
參考 [os x 10.9.5 更新不了版本 - Apple 社区](https://discussionschinese.apple.com/thread/140127319?sortBy=best) 在網站上下載 .dmg 檔案，安裝新版 OS。
(2023/12/25更新) 如果安裝更新遇到「您無法安裝到這個卷宗上，因為目前正在加密卷宗」的加密問題可以參考[這個](https://discussionschinese.apple.com/thread/140127092?sortBy=best)。

(2023/12/25更新) 你可以用[這個](https://support.apple.com/zh-tw/102465)來安裝 Windows 支援軟體。裝完之後記得在 windows 那邊檢查 apple software update 有沒有更新，有的話就更新一下。還是怪怪的就多執行 setup.exe 幾次試試看。

![](https://i.imgur.com/k5Ow6KS.jpg)

![](https://i.imgur.com/q5ZO5Q9.jpg)

(2023/12/25更新) 沒有聲音的話可以試著切換音效裝置。

![](https://i.imgur.com/oIVaB1D.jpg)

#### 在 OSX 可以切割硬碟嗎？

可以參考這個影片：[https://youtu.be/Z8-jm17YBt8?si=ClYQ_Jl982qF2GfW](https://youtu.be/Z8-jm17YBt8?si=ClYQ_Jl982qF2GfW)

    祝大家的重灌之路都很順利，尤其是 Mac 的使用者，在這個微軟爸爸最大的世界，我們要自立自強