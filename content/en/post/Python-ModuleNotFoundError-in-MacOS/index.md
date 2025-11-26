+++
title = 'How to Fix Python ModuleNotFoundError on MacBook'
date = 2023-03-07T14:36:57+08:00
draft = false
tags = ['Mac', 'Python', 'Module', 'ModuleNotFoundError']
categories = ['Python']
keywords = ['python', 'mac', 'module', 'ModuleNotFoundError', 'old-blog-post']
description = 'When using pip3 install to install packages on MacOS, you might encounter Python not finding the packages. This article provides solutions, explains the differences between python3 -m pip install and pip3 install, and how to correctly install Python packages.'
excerpt = 'When using pip3 install to install packages on MacOS, you might encounter Python not finding the packages. This article provides solutions and explains the differences between python3 -m pip install and pip3 install.'
image = 'https://i.imgur.com/HsOyXgx.png'
toc = true
comments = true
+++

(Old blog post, written: 2023-03-07)

## Problem

When you use `pip3 install [package]` to install packages on MacOS, you might find that Python cannot find the package, which looks like this:
![](https://i.imgur.com/HsOyXgx.png)

## Solution

- You can use `pip3 show [package_name]` to check the installation path of the package.
![](https://i.imgur.com/qJnj6oR.png)

- Then use `python3 -m site` to check the USER-SITE path
![](https://i.imgur.com/XD80amt.png)

You'll discover that the execution path and package path are different. The best solution is to not use `pip3 install`, but instead use:

```bash
python3 -m pip install [package_name] # This will install it under USER_SITE
```

Both `python3 -m pip install` and `pip3 install` are used to install Python packages, but there are slight differences in execution.

`python3 -m pip install` uses the built-in `pip` module of the Python interpreter for installation. Through the `-m` parameter specifying the module name `pip`, it ensures that the correct version of `pip` is used, avoiding version inconsistencies or packages being installed in the wrong location due to system environment issues.

On the other hand, `pip3 install` directly calls the `pip` tool installed on the system for installation. This method might cause installation errors or packages being installed in the wrong location due to system environment or incorrect PATH settings.

In conclusion, using `python3 -m pip install` is safer, ensuring that the correct version of `pip` is used, and avoiding version inconsistencies or packages being installed in the wrong location due to system environment issues.

> Conclusion: On Mac, just use python3 -m pip install :)
