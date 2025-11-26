+++
title = 'UVa10190: Divide, But Not Quite Conquer!'
date = 2022-07-11T19:27:36+08:00
draft = false
tags = ['CPE', 'Problem Solving', 'UVa']
categories = ['Problem Solving']
keywords = ['CPE', 'UVa', 'problem solving', 'UVa10190', 'old-blog-post']
description = "UVa10190: Divide, But Not Quite Conquer! Problem solving reflection and issue analysis. This article records the problems I encountered while practicing CPE past questions, including pow() function numerical precision issues and boundary condition handling details."
excerpt = "UVa10190 problem solving reflection, mainly discussing pow() function numerical precision issues and boundary condition handling, reminding myself to pay attention to details in programming."
image = 'https://i.imgur.com/vFpXPab.png'
toc = false
comments = true
author = '海狸大師'
authorLink = 'https://github.com/yenslife'
+++

(Old blog post, written: 2022-07-11)

## UVa10190: Divide, But Not Quite Conquer!

This is my problem solving reflection written while practicing CPE past questions, to remind myself not to make the same mistakes again.

Problem link: [10190.pdf (onlinejudge.org)](https://onlinejudge.org/external/101/10190.pdf)

I mainly fell into two pitfalls with this problem:
1. [Not understanding the implementation details of the `pow()` method, leading to numerical errors during type casting.](#problem-analysis-pow-function-pitfall)
2. [Not considering cases where the denominator is 0 or 1.](#problem-analysis-not-careful-enough)

Here is my code:

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

My approach was to first use a recursive function (because it felt cool, and I had this feeling that using `#define` at the front was awesome XD, but actually the performance is terrible, though it doesn't matter much for this one-star problem) to determine if the numerator is a power of the denominator. If not, I can directly print "Boring!"; if yes, I first use a loop to calculate the exponent size, then I can print out the answer.

### Problem Analysis (Pow Function Pitfall)

Reference website [C/C++ pow() function result forced type conversion to integer error analysis](https://blog.csdn.net/qq_41115379/article/details/105159542) (Chinese)

Look at the following function. What do you think the execution result will be?

```cpp
printf("%d", (int)pow(10,2))
```

The answer is: 99. Strange, shouldn't the result be 100? The reason is actually quite simple. The return value of `double pow(double x, double y)` is obtained through numerical approximation methods. For example: 99.9999 will finally become 99.

Here [关切得大神](https://blog.csdn.net/qq_41115379) gave a pretty good method - just do one rounding `round()` on the return value of `pow()`, because we know that the return value of `pow()` is already very close to the mathematical calculation result. Please refer to the code below.

```cpp
printf("%d", (int)round(pow(10,2)))
```

Because I really hadn't written code for too long, and I thought I should be right about everything, I almost thought it was a compiler problem (where did this confidence come from). Amazingly, the original code (before rounding) worked normally in online compilers and VS Code in Mac environment, but encountered numerical errors in "瘋狂程設" (Crazy Programming). This story tells me that fortunately I installed "瘋狂程設" to test. If I had practiced on Mac the same way, I might never have discovered this error. When I encounter this problem in the CPE exam, it would be disastrous. By the way, I'm using a virtual machine to run Windows, ~~I'll share the free usage method if I have the chance nope~~.

### Problem Analysis (Not Careful Enough)

Then there are the test cases I didn't consider. Maybe because I hadn't solved programming problems for too long, I actually forgot to consider cases where the denominator is 0 or 1. In this problem, if the denominator is 0 or 1, meaning there's no result, then I should print "Boring!". If I don't do conditional checking, I'll get a floating-point exception, so be careful.
