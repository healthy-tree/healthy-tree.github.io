---
layout: "post"
title: "Questions About Homebrew"
date: "2019-10-23 07:35"
---

升级到了新的苹果操作系统凯特琳娜（MacOS Catalina），Homebrew 在升级软件的时候出现了问题，以下是我遇到问题的解决方式。

``` bash
xcrun: error: active developer path ("/Applications/Xcode.app/Contents/Developer") does not exist, use xcode-select --switch path/to/Xcode.app to specify the Xcode that you wish to use for command line developer tools (or see man xcode-select)
```

解决方案：

``` bash
sudo xcode-select --reset
```
