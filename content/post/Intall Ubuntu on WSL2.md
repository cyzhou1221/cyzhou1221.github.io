+++
title = "Install Ubuntu on WSL2" 
date = "2025-06-19"
series = "Environment Setup"
+++

参考链接: https://learn.microsoft.com/zh-cn/windows/wsl/

# 1. 启用 WSL 和虚拟机平台

以管理员身份打开 powershell，输入以下命令：

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

重启

# 2. 安装 WSL
```powershell
wsl --install
```
默认安装 Ubuntu 

设置 WSL 2 为默认值

```powershell
wsl --set-default-version 2
```

查看已经安装的 WSL 发行版

```powershell
wsl -l -v
```

更换已安装 WSL 发行版的存储位置

```
wsl --manage <distro_name> --move <new_location>
```

# 3. 更换软件源为国内镜像源

备份原文件

```
sudo cp /etc/apt/sources.list.d/ubuntu.sources    /etc/apt/sources.list.d/ubuntu.sources.bak   
```

创建新的源配置文件

 ```
sudo vim /etc/apt/sources.list.d/tsinghua.sources
 ```
‌
例如，添加清华源的配置如下：

```text
Types: deb
URIs: https://mirrors.tuna.tsinghua.edu.cn/ubuntu/
#如果使用其他镜像站，上面这行可以改成其他镜像站的网址
Suites: noble noble-updates noble-backports
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

Types: deb
URIs: http://security.ubuntu.com/ubuntu
#如果安全更新需要使用镜像站，上面这行也改成 URIs: https://mirrors.tuna.tsinghua.edu.cn/ubuntu/
Suites: noble-security
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

修改完成后，运行以下命令更新软件源，升级系统：

```bash
sudo apt update
sudo apt upgrade
```


# 5. 移除 vim-tiny，安装 vim
```
sudo apt remove vim-tiny
sudo apt install vim
```


# [可选]卸载 snap
关闭所有的 snapd 服务，为卸载 snapd 软件包做准备：
1. 列出 snap 服务项，
```bash
sudo systemctl | grep snap
```
2. 依次 disable，
```bash
sudo systemctl disable snapd.xxx.xxx
```
3. 卸载 snap 
```bash
sudo apt-get purge snapd
```

注：purge - Remove packages and config files

参考链接: https://zhuanlan.zhihu.com/p/665760051
