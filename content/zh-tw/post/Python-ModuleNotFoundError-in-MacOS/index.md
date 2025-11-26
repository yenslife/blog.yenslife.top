+++
title = 'Macbook 找不到 Python Module 解決方法'
date = 2023-03-07T14:36:57+08:00
draft = false
tags = ['Mac', 'Python', 'Module', 'ModuleNotFoundError']
categories = ['Python']
keywords = ['python', 'mac', 'module', 'ModuleNotFoundError', '舊網站文章']
description = '在 MacOS 中使用 pip3 install 安裝套件時，可能會發生 Python 找不到套件的問題。本文提供解決方法，說明 python3 -m pip install 和 pip3 install 的差異，以及如何正確安裝 Python 套件。'
excerpt = '在 MacOS 中使用 pip3 install 安裝套件時，可能會發生 Python 找不到套件的問題。本文提供解決方法，說明 python3 -m pip install 和 pip3 install 的差異。'
image = 'https://i.imgur.com/HsOyXgx.png'
toc = true
comments = true
+++

（舊網站文章，撰寫時間：2023-03-07）

## 問題

當你在 MacOS 中使用 `pip3 install [套件]` 安裝套件時 ，可能會發現 Python 找不到ｗ套件，會長這樣
![](https://i.imgur.com/HsOyXgx.png)


## 解決方法

- 你可以用 `pip3 show [套件名稱]` 來查看套件的安裝路徑。
![](https://i.imgur.com/qJnj6oR.png)

- 然後用 `python3 -m site` 查看 USER-SITE 路徑
![](https://i.imgur.com/XD80amt.png)


會發現執行路徑和套件路徑不同，最好的解決方法就是不要用 `pip3 install`，改用

```bash
python3 -m pip install [套件名稱] # 用這個他就會幫你裝在 USER_SITE 底下了
```

`python3 -m pip install` 和 `pip3 install` 兩者的功能都是用來安裝 Python 套件，但是在執行上有些微的差異。

`python3 -m pip install` 是使用 Python 解釋器內建的 `pip` 模組進行安裝，透過 `-m` 參數指定模組名稱 `pip`，並且可以確保使用的是正確版本的 `pip`，避免了因為系統環境問題導致的版本不一致或是套件安裝到錯誤的位置的問題。

而 `pip3 install` 則是直接呼叫系統上安裝的 `pip` 工具進行安裝，這種方式可能會因為系統環境或是 PATH 設定不正確而導致安裝錯誤或是套件安裝到錯誤的位置。

結論就是，使用 `python3 -m pip install` 會比較保險，可以確保使用的是正確版本的 `pip`，而且也避免了因為系統環境問題導致的版本不一致或是套件安裝到錯誤的位置的問題。

> 結論：在 Mac 上用 python3 -m pip install 就對了：）