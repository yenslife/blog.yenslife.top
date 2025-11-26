+++
title = 'Mac 實用小工具'
date = 2022-08-26T18:24:34+08:00
draft = false
tags = ['Mac', '小工具', 'Menu bar']
categories = ['工具分享']
keywords = ['Mac', 'tool', '實用', '小工具', '工具分享', 'Menu bar', '桌面', '舊網站文章']
description = 'Mac 的實用小工具分享，選擇標準是只要有「如果當初有人告訴我去使用這個工具那該有多好」的感覺就會上榜。包含桌面系統工具、程式開發工具，以及各種提升效率的實用軟體。'
excerpt = 'Mac 實用小工具分享，包含 Alfred、Rectangle、Bartender 等提升工作效率的必備工具，以及 Homebrew、iTerm2 等開發者工具。'
image = 'https://i.imgur.com/4uvpVox.png'
toc = true
comments = true
author = '海狸大師'
authorLink = 'https://github.com/yenslife'
+++

（舊網站文章，撰寫時間：2022-08-26）


## Mac 實用小工具（不定期更新！）

身為一個 Mac 筆電使用者，今天想跟大家分享我常用的小工具或軟體，不論在寫程式或者日常使用都很常用到的工具，只要有「如果當初有人告訴我去使用這個工具那該有多好」的感覺就會出現在目錄。順便提醒之後要換筆電的自己要裝什麼（雖然我應該沒錢換🤧）。有興趣可以裝來玩玩看，有些工具能節省你非常多時間！這篇文章不會太過著墨在操作方法以及安裝，主要是在分享。以下是目錄。

### 桌面與系統相關（日常生活）

- [Alfred（Spotlight 升級版）](#Alfred（Spotlight升級版）)
- [Rectangle（視窗調整工具）](#Rectangle（視窗調整工具）)
- [Bartander（Menu-Bar-管理工具）](#Bartander（Menu-Bar-管理工具）)
- [Itsycal（行事曆小工具）](#Itsycal（行事曆小工具）)
- [Stats（免費開源的系統資源監控軟體）](#Stats（免費開源的系統資源監控軟體）)
- [Gifski（影片轉gif的超好用工具）](#Gifski（影片轉gif的超好用工具）)

### 程式電腦相關（資工系可能會用到）

- [Homebrew（神級套件管理工具）](#Homebrew（神級套件管理工具）)
- [iterm2（更好的 teminal）](#iterm2（更好的teminal）)
- [Fig（終端機自動補全）](#Fig（終端機自動補全）)
- [Cloudflare WARP VPN（免費方便的 vpn）](#Cloudflare-WARP-VPN（免費方便的vpn）)

### 其他（雜七雜八ㄉ東東）
- 一些設定（角落關螢幕、dock隱藏）

<br>

### Alfred（Spotlight升級版）

![Alfred](https://i.imgur.com/kB65WF7.png)

它叫[阿福](https://www.alfredapp.com/)？因為他就像蝙蝠俠的管家一樣幫你從軟體到檔案管的好好的，只要按下 `command + space(空白鍵)` 就可以叫出來，幾乎可以實現不用滑鼠及觸控板的老鳥級操作，想叫什麼就叫什麼，可以讓自己的雙手幾乎是黏在鍵盤上！

一開始要到 Preference 中做一些設定，並到**系統偏好設定**中把 Mac 內建的 Spotlight 改成 Alfred 即可使用（當然你想同時保留 Spotlight 或不想用 command + 空白鍵也可以）

![](https://i.imgur.com/mcIAeat.png)


基本用法：command + 空白鍵 -> 跳出搜尋欄，以下是我常用的搜尋關鍵字：

<div style="text-align: center">
<img src="https://i.imgur.com/O9mKg0v.png" width="50%"/>
    <p style="font-size:50%;">搜尋欄長這樣，也可以自訂喜歡的樣式</p>
</div>

- open + 檔案 -> 開啟檔案
- find + 檔案 -> 從 finder 找到檔案位置
- 直接打軟體名稱 -> 開啟軟體
- 直接打搜尋內容 -> 用瀏覽起開啟搜尋結果

Alfred 唯一的小缺點就是要收費，聽說他還可以叫聯絡人和音樂等等，但我只需要基本功能，所以免費版就可以符合我的需求，進階玩法就留給大家試試咯。

官網連結: https://www.alfredapp.com/

<br>

### Rectangle（視窗調整工具）

<div style="text-align: center">
<img src="https://i.imgur.com/Hwwz1Ud.png" width="35%"/>
</div>

不知道你有沒有這樣的經驗：打開軟體發現他的視窗位置很奇葩，又或是想同時用兩個視窗來做事情（邊看資料邊打字），於是花了一些時間調整視窗的 size 和位置，但就是沒辦法完美分割成兩個左右視窗？ 讓 [Rectangle](https://rectangleapp.com/) 幫你搞定吧！免費的真香～

點擊 menu bar 上的圖示就可以看到所有功能快捷鍵了，主要都是透過 `control + alt(option) + 某個鍵` 來控制當下使用的視窗位置與大小。舉幾個我最常用的快捷鍵吧！

<div style="text-align: center">
<img src="https://i.imgur.com/GmCrsSd.png" width="30%"/>
    <p style="font-size:50%;">圖示長這樣</p>
</div>


- `control + alt(option) + enter` -> 最大化視窗
- `control + alt(option) + 左/右方向鍵` -> 左半/右半
- `control + alt(option) + B` -> 接近最大化（有時候這樣蠻舒服的）
- `control + alt(option) + C` -> 中央視窗
- `control + alt(option) + + / -` -> 放大/縮小視窗

結果會像這樣：

![Rectangle 示範](https://i.imgur.com/rnr6a07.gif)

官網連結：https://rectangleapp.com/

<br>

### Bartander（Menu-Bar-管理工具）

![](https://i.imgur.com/hdWrFw3.png)

因為我常常裝一些小工具在 Mac 上，一開始沒什麼，但當 Menu bar (螢幕右上角那個)上的東西越來越多時，情況開始不受控制，有些 icon 甚至會消失，Mac 居然沒有內建的管理工具，誇張！於是我找到了這款被人推崇的 [Bartender](https://www.macbartender.com/)，現在我的右上角看起來整齊多了！(如下，鼠標移到上方叫出所有圖示)

![bartender 自動](https://i.imgur.com/N2Pu8e8.gif)

當然，你也可以自定義喜歡的樣式，像是隱藏的圖示，甚至是建立群組等等。他好像還有快捷鍵叫出的進階玩法，這部分就留給讀者玩玩看吧！

![](https://i.imgur.com/TFcWyrV.gif)

唯一的小缺點就是：他要錢QQ。如果真的想體驗的話，網路上有很多免費(~~盜版~~)版本的可以下載，但有機會還是支持一下作者吧！他是買斷制的，目前是US$16.80。

官網連結：https://www.macbartender.com/

<br>

### Itsycal（行事曆小工具）

![](https://i.imgur.com/dSLqpBm.png)


[Itsycal](https://www.mowglii.com/itsycal/) 是一款 Menu bar 上的行事曆軟體。有時候看到一些活動是在下個月的某某號，想知道他是禮拜幾卻又不想打開行事曆的話，輕點右上角的 [Itsycal](https://www.mowglii.com/itsycal/) 圖示便能看到行事曆。若你有在使用 Mac 內建的行事曆，他甚至能幫你直接在 Menu bar 安排活動，不需要再多打開一個 app。

![](https://i.imgur.com/Y4OCUa1.gif)

<div style="text-align: center">
<p style="font-size:50%;">效果展示</p>
</div>

他還可以自訂圖示，有一套[語法](https://www.mowglii.com/itsycal/datetime.html)，不會太難，我沒有用因為 mac 就有內建的日期顯示了，所以用一個可愛的表情符號代替 ٩(◕‿◕｡)۶，只要進到 Preference 設定即可！

免費的就是香，官網[連結](https://www.mowglii.com/itsycal/datetime.html)

<br>

### Stats（免費開源的系統資源監控軟體）

![](https://i.imgur.com/gGQ88ci.png)

我們 Mac 使用者應該是不會太常把電腦關機吧（還是只有我這樣XD），有時候發現電腦開始變卡，不知道是哪個程序在搞，又不想打開「活動監視器」App 來看，這時候就可以透過 [Stats](https://github.com/exelban/stats) 來觀察執行程序 Ram 使用量，甚至是 GPU、CPU、網路、風扇等等的運作狀態。重點是，只要點右上角的圖示就可以看到了，超方便ㄉ！我之前覺得電腦卡卡的，看了 Stats 馬上就知道原來是虛擬機忘記關、或是我又開了什麼奇怪的程序XD。

![](https://i.imgur.com/HDqX4Jp.gif)

<div style="text-align:center">
<p style="font-size:50%;">效果展示</p>
</div>


Homebrew 安裝指令：`brew install --cask stats`
官方 Github [連結](https://github.com/exelban/stats)
下載 dmg 安裝檔 [連結](https://github.com/exelban/stats/releases/latest/download/Stats.dmg)

<br>

### Gifski（影片轉gif的超好用工具）

![](https://i.imgur.com/WJBEHjS.png)

[Gifski](https://apps.apple.com/tw/app/gifski/id1351639930?mt=12) 是一款 mac 影片轉檔工具，轉檔完成後可以直接 copy 影片到剪貼簿，再貼到像是 HackMD 之類的的地方，非常方便。你可以在 App Store 上找到它，他是免費的，這篇文章的 Gif 大部分都是用它來做的呦！


![](https://i.imgur.com/TcAt0YH.gif)

<div style="text-align:center">
<p style="font-size:50%;">將影片拖近來就可以轉檔，操作簡單明瞭</p>
</div>

App Store [連結](https://apps.apple.com/tw/app/gifski/id1351639930?mt=12)

<br>

### Homebrew（神級套件管理工具）

![](https://i.imgur.com/aI1q8PU.png)


[Homebrew](https://brew.sh/) 就像 Mac 版本的 apt (Linux 上的套件管理工具) ，可以在終端機 (terminal) 用指令來安裝套件、軟體等等。想像一下人家 Windows 還在解壓縮，你只要打幾個字就好了，這就是為什麼 Homebrew 如此滴神orz。

安裝方法如下：

1. 打開終端機 (terminal)。
2. 在終端機輸入以下指令，然後按 Enter。

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

分享一些 Homebrew 基本指令：

```bash
brew install [套件名稱] // 安裝套件

brew uninstall [套件名稱] // 解除安裝

brew update // 更新 brew 到最新版本

brew upgrate [套件名稱] // 更新指定套件

brew list // 查看現有的套件以及 Cask

brew search [關鍵字] // 查詢套件
```

- Homebrew Cask 是安裝 GUI 軟體在用的，使用方法同上，只是要在 brew 之後加上 `--cask`

<br>

### iterm2（更好的teminal）

![](https://i.imgur.com/5uQ5ukT.png)

[iterm2](https://iterm2.com/) 是一款可以取代 Mac 原生 terminal 的好選擇，不信你看

![](https://i.imgur.com/S2ewQPq.png)

是不是很漂亮ㄚ，通常 iterm2 會搭配 oh my zsh 框架使用，其實網路上已經有很多很棒的教學了，我覺得自己應該教不贏他們 (也忘記了XD) ，請自行 Google 搜尋一下ㄅ。（關鍵字：oh my zsh, iterm2 教學）

[iterm2 官網](https://iterm2.com/)
Home brew 安裝：`brew install --cask iterm2`

<br>

### Fig（終端機自動補全）

![](https://i.imgur.com/VUdtEMA.png)

想不到吧？終端機竟然可以和 IDE 一樣自動補全指令，有了 Fig 就不用一直按 tab 鍵了，上下方向鍵即可選擇指令，安裝後即可發現新世界 (示範如下)，而且他是免費的！

![](https://i.imgur.com/MhpcGzM.gif)

[Fig 官網連結](https://fig.io/)

<br>

### Cloudflare-WARP-VPN（免費方便的vpn）

![](https://i.imgur.com/yu4u6W8.png)

如果你是成大的學生，剛好最近在研究加密貨幣投資，那你可能會用到「幣安」這個交易所對吧？但學校的網路居然把它給擋下來了QQ（學校網路總是擋了很多東西），剛好你又沒有使用 VPN 的服務，這時候 [Warp VPN](https://1.1.1.1/) 就可以幫你一個大忙了！他是 Cloudflare 公司免費提供的 VPN 服務，不用擔心隱私問題，只要點擊 Menu bar 上的雲朵圖示即可使用！


<div style="text-align:center">
<img src="https://i.imgur.com/DOhXUCZ.gif" width="30%">
<p style="font-size:50%">簡潔有力的使用方式</p>
</div>

官網網址：https://1.1.1.1/

### Clipy

Mac 剪貼簿歷史紀錄功能，按下 `command + shift + v` 即可叫出歷史紀錄，可以設定紀錄筆數，非常方便！

[官網連結](https://clipy-app.com/)

![](https://imgur.com/cjLMYWs.png)


### Bob

OCR 文字識別工具，可以將圖片上的文字轉成文字檔，很適合在有時候線上 meeting 時需要快速記錄文字的時候使用，真的太多時候用到了！重點是他是免費的！

使用方法：`optoin + s` 即可叫出，然後圈選出你要識別的區域，就像 Mac 的 `command + shift + 4` 截圖一樣

https://github.com/ripperhe/Bob

![](https://imgur.com/DCTvH5s.gif)



### 一些設定（角落關螢幕、dock隱藏）
(待補)

