---
title: WSL2 编译安装内核以支持驱动程序编译
date: 2025-10-16
categories: WSL2
---

<!--more-->

参考链接: 
- https://www.cnblogs.com/hartmon/p/15771731.html
- https://github.com/microsoft/WSL2-Linux-Kernel

# 0. 预备工具(build dependencies)
```
sudo apt install build-essential flex bison dwarves libssl-dev libelf-dev cpio qemu-utils
```

# 1. 下载内核代码
```
git clone --depth=1 https://github.com/microsoft/WSL2-Linux-Kernel
```

# 2. 编译
```
cd WSL2-Linux-Kernel
```

更改内核配置：采用微软默认配置
```
LOCALVERSION= make KCONFIG_CONFIG=Microsoft/config-wsl -j20
```
- `-j20`: 表示采用的线程数，这里我使用 20，最大线程数：任务管理器 --> 性能 --> CPU --> 逻辑核心数

# 3. 安装
```
sudo LOCALVERSION= make KCONFIG_CONFIG=Microsoft/config-wsl modules_install -j20
```

# 4. 检查 `/lib/modules/6.6.87.2-microsoft-standard-WSL2` 中的 `build` 文件夹
```
ls -l | grep build
```

输出为
```
-rw-r--r--  1 root root      2573 Oct 16 17:39 Kbuild
lrwxrwxrwx  1 root root        40 Oct 16 17:45 build -> /home/cyzhou/Downloads/WSL2-Linux-Kernel
```
`build` 为指向 `/home/cyzhou/Downloads/WSL2-Linux-Kernel` 的软链接
