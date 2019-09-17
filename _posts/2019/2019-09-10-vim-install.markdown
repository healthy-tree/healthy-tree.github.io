---
layout: post
title: "vim install"
date: "2019-09-10 11:48:32 +0800"
---

Just for MacOS with brew.

``` Bash
#!/bin/bash

brew install vim ctags git astyle
sudo ARCHFLAGS=-Wno-error=unused-command-line-argument-hard-error-in-future easy_install twisted

sudo easy_install -ZU autopep8 twisted
sudo ln -s /usr/bin/ctags /usr/local/bin/ctags
mv -f ~/vim ~/vim_old
cd ~/ && git clone https://github.com/ma6174/vim.git
mv -f ~/.vim ~/.vim_old
mv -f ~/vim ~/.vim
mv -f ~/.vimrc ~/.vimrc_old
mv -f ~/.vim/.vimrc ~/
git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle
vim vim_mac -c "BundleInstall" -c "q" -c "q"
rm vim_mac
echo "安装完成"
```
