---
title: Install WSL2 Ubuntu 
date: 2025-09-29
categories: 环境搭建
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

```
wsl --manage Ubuntu-22.04 --move E:/Ubuntu
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
sudo apt update && sudo apt upgrade
```



# 后续

## Install Vim from source

### 1. Connecting to GitHub using SSH keys

[Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

### 2. Install gcc-12 and g++-12

```
sudo apt install gcc-12 g++12
sudo ln -sf /usr/bin/gcc-12 /usr/bin/gcc
sudo ln -sf /usr/bin/g++-12 /usr/bin/g++
```

### 3. Compile and install Vim

https://github.com/vim/vim

1. 更改 `vim/src` 中的 `Makefile`: `--enable-python3interp`, `--disable-gui`
   ```
   cd vim/src
   ```
   
2. ```
   make reconfig
   ```
   
3. 卸载已经安装的 Vim
   ```
   sudo apt purge vim-tiny
   sudo apt purge vim
   sudo apt autoremove
   ```
   
4. ```
   sudo make install
   ```

### 4. Configure
1. Copy the [Configuration](https://github.com/cyzhou1221/dotfiles),
   ```
   cd dotfiles
   cp -r vim/.vim ~/
   cp vim/.vimrc .
   ```
   
2. Install Vim-Plug and plugins
   1. [Vim-Plug](https://github.com/junegunn/vim-plug), Download plug.vim and put it in the "autoload" directory.
   2. Before doing this, make sure you have configured the proxy correctly so that you can access github.com.
      ```
      vim
      :PlugInstall
      ```
