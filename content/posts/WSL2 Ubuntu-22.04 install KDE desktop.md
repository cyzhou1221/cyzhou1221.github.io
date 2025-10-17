---
title: WSL2 Ubuntu-22.04 install KDE desktop.md
date: 2025-10-17
categories: WSL2
---

<!--more-->

## 1. 安装步骤
- 按照[这篇教程](https://blog.csdn.net/qq_30448087/article/details/134897586)来装就好.
- 另一篇详细透彻的[英文博客](https://ivonblog.com/en-us/posts/run-linux-desktop-on-wsl/)(安装步骤相同，但是没有避坑处理).

## 2. 编写桌面开启与关闭脚本
`open-desktop.sh`
```
#!/bin/bash

Xephyr -br -ac -noreset -resizeable -screen 1920x1080 :1 &
sleep 2
export DISPLAY=:1
xrandr --size 1920x1080
dbus-launch --exit-with-session startplasma-x11 &
```
KDE 桌面启动之后，可通过桌面窗口对分辨率和缩放比例进行进一步的设置.


`close-desktop.sh`
```
#!/bin/bash

pkill -f Xephyr
```
