+++
title = 'UVa10190：Divide, But Not Quite Conquer!'
date = 2022-07-11T19:27:36+08:00
draft = false
tags = ['CPE', '程式解題', 'UVa']
categories = ['程式解題']
keywords = ['CPE', 'UVa', '程式解題', 'UVa10190', '舊網站文章']
description = 'UVa10190：Divide, But Not Quite Conquer! 解題心得與問題檢討。這篇文章記錄我練習 CPE 歷屆題目時遇到的問題，包含 pow() 函數的數值誤差問題以及邊界條件處理的細心程度。'
excerpt = 'UVa10190 解題心得，主要探討 pow() 函數的數值誤差問題以及邊界條件處理，提醒自己在程式設計中要注意細節。'
image = 'https://i.imgur.com/vFpXPab.png'
toc = false
comments = true
author = '海狸大師'
authorLink = 'https://github.com/yenslife'
+++

（舊網站文章，撰寫時間：2022-07-11）

## UVa10190：Divide, But Not Quite Conquer! 

這是我在練習 CPE 歷屆題目時，順手寫下的解題心得，提醒自己不再重蹈覆徹。

題目連結：[10190.pdf (onlinejudge.org)](https://onlinejudge.org/external/101/10190.pdf)

這題我主要採了兩個坑：
1. [搞不清楚`pow()` 方法的實作細節，導致在強制轉型時數值有誤差。](#問題檢討（踩到pow的坑了）)
2. [沒有考慮到分母為0和1的情況。](#問題檢討（不夠細心）)

以下是我的程式碼：

```cpp
#define true 1
#define false 0
#define bool int

#include<stdio.h>
#include<math.h>

bool division(long a, long b);

int main() {
    long a = 0, b = 0, power = 0;
    while (scanf("%ld %ld", &a, &b) != EOF) {
        if (!division(a, b)) {
            printf("Boring!\n");
        } else if (division(a, b)) {
            power = 0;
            for (int i = 0; i < a; i++) {
                if ((long)round(pow((double)b, (double)i)) == a) {
                    power = i;
                    break;
                }
            }
            for (int i = power; i > 0; i--) {
                printf("%d ", (int)round(pow(b, i)));
            }
            printf("1\n");
        }
    }
    return 0;
}


bool division(long a, long b) {
    if (b == 0 || b == 1 || a % b != 0)
        return false;
    if (a / b == 1 && a % b == 0) {
        return true;
    }
    return division(a / b, b);
}
```

我的思路是，先用遞迴函式(因為感覺很帥，還有不知道為什麼在前面用`#define`很厲害的感覺XD，但其實效能很慘，這題一顆星而已所以影響不大)判斷分子是否為分母的次方數，若為否則可直接印出boring!；若為是，則先用迴圈計算指數大小，就可以將答案印出來。

### 問題檢討（踩到pow的坑了）

參考網站 [C/C++ pow()函数结果强制类型转换为整型的误差分析_关切得大神的博客-CSDN博客](https://blog.csdn.net/qq_41115379/article/details/105159542)

觀察以下函數 你覺得執行結果是什麼呢

```cpp
printf("%d", (int)pow(10,2))
```

答案是：99。奇怪，結果不應該是100嗎？原因其實很簡單，`double pow(double x, double y)` 回傳值是以數值逼近的方法來的到double的數值的。舉例來說：99.9999最後會變成99。

這邊[关切得大神](https://blog.csdn.net/qq_41115379)給了個相當不錯的方法，對`pow()` 的回傳值做一次四捨五入`round()` 就行了，因為我們知道，`pow()` 的回傳值已經很接近數學計算的答案了。請參考下方程式碼。

```cpp
printf("%d", (int)round(pow(10,2)))
```

因為真的太久沒寫，又覺得自己應該都是對的，導致我差點以為是編譯器的問題(這哪來的自信)。神奇的是，原本的程式碼(還沒四捨五入)的執行結果在 online compiler 以及 mac 環境的 vs code 中都可以正常運作，但在瘋狂程設卻會遇到數值誤差。這個故事告訴我，幸好有裝瘋狂程設來試，如果我一樣在 Mac 練習可能永遠無法發現這個錯誤，到時候考 CPE 遇到這問題就爆炸ㄌ。順帶一提，我是用虛擬機跑 Windows 的，~~有機會再分享免費使用的方法 沒~~。

### 問題檢討（不夠細心）

再來就是沒有考慮到的測資了，可能是太就沒有解程式題目了，居然忘記要把分母為0和1的情況考慮進去。這題的情況是，若分母為0或1，也就是沒有結果，那就要印出boring!，如果沒有做條件判斷將會得到浮點數例外，要小心喔。