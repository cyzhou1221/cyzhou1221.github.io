---
title: Install WSL2 Ubuntu 
date: 2025-07-12
series: Environment Setup
---

<!--more-->

参考链接: https://learn.microsoft.com/zh-cn/windows/wsl/

# 1. 启用 WSL 和虚拟机平台

以管理员身份打开 powershell，输入以下命令：

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

重启电脑.



# 2. 安装 WSL

更新 WSL

```
wsl --update
```
设置 WSL 2 为默认值

```
wsl --set-default-version 2
```

查看可安装的 Linux 发行版

```
wsl --list --online
```

安装 Ubuntu 22.04 LTS

```
wsl --install Ubuntu-22.04
```

更改已安装的 WSL 发行版的存储位置

```
wsl --manage <distro_name> --move <new_location>
```



# 3. 更换软件源为清华源

教程链接: https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/

备份原文件

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.save  
```

```
sudo vim /etc/apt/sources.list
```
将其内容更换为:

```text
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse

# 以下安全更新软件源包含了官方源与镜像站配置，如有需要可自行修改注释切换
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
```

修改完成后，运行以下命令更新软件源，升级系统：

```
sudo apt update
sudo apt upgrade
```



# 后续

##  移除 vim-tiny，安装 vim
```
sudo apt remove vim-tiny
sudo apt install vim
```


##  [可选]卸载 snap
教程链接: https://zhuanlan.zhihu.com/p/665760051

1. 卸载 snap 管理的软件
   1. 列出软件 
        ```
        sudo snap list
        ```
	2. 依次卸载
        ```
        sudo snap remove xxx
        ```
2. 卸载 snap 服务项
    1. 列出 snap 服务项，
        ```
        sudo systemctl | grep snap
        ```
    2. 依次 disable，
        ```
        sudo systemctl disable snapd.xxx.xxx
        ```
3. 卸载 snap 
    ```
    sudo apt-get purge snapd
    ```

注：purge - Remove packages and config files
