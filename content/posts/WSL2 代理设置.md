---
title: WSL 镜像模式使用主机代理 
date: 2025-09-30
categories: 环境搭建
---

<!--more-->

参考链接：https://zinglix.xyz/2020/04/18/wsl2-proxy/

**背景**：

> 在 **镜像网络模式** 下，WSL2 会将主机的网络接口和 IP 地址等信息“镜像”到 Linux 实例中，这意味着 **WSL2 可以直接使用主机的 `127.0.0.1` （localhost）来访问主机上运行的服务**，包括代理软件。

-----



## 1\. 启用 WSL2 镜像网络模式

编辑或创建 WSL 的配置文件 **`.wslconfig`**。

1.  打开 **文件资源管理器**，然后在地址栏输入 `%USERPROFILE%` 并回车（这会带你到你的用户文件夹，例如 `C:\Users\YourName`）。

2.  在这个目录下，找到或创建一个名为 **`.wslconfig`** 的文件（注意文件名以点开头）。

3.  用记事本或其他文本编辑器打开 `.wslconfig`，并添加以下内容：

    ```ini
    [wsl2]
    networkingMode=mirrored
    ```
	确保文件只有这些内容，**保存**文件。

-----



## 2\. 重启 WSL 

1.  打开 **PowerShell**，关闭 WSL

    ```powershell
    wsl --shutdown
    ```

2.  等待几秒，然后重新启动 WSL Linux 发行版
    ```powershell
    wsl
    ```

-----



## 3\. 在 WSL 中配置代理（通过 `127.0.0.1`）

如果代理软件（例如 Clash, V2rayN, ShadowSocks 等）在 Windows 上是以 HTTP 或 SOCKS5 代理的形式运行，并且**监听在主机的 `127.0.0.1` 地址和某个端口上**（例如 `7890`）。

在 **镜像模式** 下，WSL 实例中的 **`127.0.0.1`** 端口也会指向主机的对应端口。

可以在 WSL 中设置环境变量来使用主机的代理：

### A. 设置临时环境变量

在 WSL 终端中运行以下命令。请将 `7890` 替换为代理软件实际监听的端口。

```bash
# 假设 HTTP/HTTPS 代理端口是 7890
export http_proxy="http://127.0.0.1:7890"
export https_proxy="http://127.0.0.1:7890"
```

### B. 设置永久环境变量

为了避免每次都手动输入，你可以将这些命令添加到你的 shell 配置文件中，例如 `~/.bashrc` 或 `~/.zshrc`。

1.  打开你的 shell 配置文件（以 Bash 为例）：
    ```bash
    nano ~/.bashrc
    ```
2.  在文件末尾添加上述 `export` 命令。
3.  保存并退出。
4.  运行 `source ~/.bashrc` 使配置立即生效，或重启你的 WSL 终端。

### 4\. 验证代理是否工作

你可以使用 `curl` 命令来测试代理是否成功：

```bash
curl google.com
```

如果能正常返回 HTML 内容，比如

```
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
```

说明 WSL 已经成功通过主机的代理进行网络访问了。

-----

**总结一下：**

**关键是 `networkingMode=mirrored` 这个设置。** 它消除了传统模式下 WSL 和主机 IP 地址不固定的问题，让 WSL 可以直接使用 **`127.0.0.1`** 来访问主机上的服务。

**注意：** 确保你的 **Windows 防火墙** 没有阻止 WSL 实例访问你的代理软件监听的端口。通常情况下，如果代理软件是监听在 **`127.0.0.1`**，防火墙不会阻止本地流量.

---



##  4\. 编写脚本 `proxy.sh` 进行自动化

```
#!/bin/sh
port=7890

PROXY_HTTP="http://127.0.0.1:${port}"

set_proxy(){
    export http_proxy="${PROXY_HTTP}"
    export HTTP_PROXY="${PROXY_HTTP}"

    export https_proxy="${PROXY_HTTP}"
    export HTTPS_proxy="${PROXY_HTTP}"

    git config --global http.proxy "${PROXY_HTTP}"
    git config --global https.proxy "${PROXY_HTTP}"

    echo "Proxy set"
}

unset_proxy(){
    unset http_proxy
    unset HTTP_PROXY
    unset https_proxy
    unset HTTPS_PROXY
    git config --global --unset http.proxy
    git config --global --unset https.proxy

    echo "Proxy unset"
}

test_setting(){
    curl google.com
}

if [ "$1" = "set" ]
then
    set_proxy

elif [ "$1" = "unset" ]
then
    unset_proxy

elif [ "$1" = "test" ]
then
    test_setting
else
    echo "Unsupported arguments."
fi
```

在 `~/.bashrc` 中添加

```
alias proxy="source ~/scripts/proxy.sh"
```

这句话的意思是为这个脚本设置别名 `proxy`，这样在任何路径下都可以通过 `proxy` 命令使用这个脚本，如

```
proxy set
proxy unset
proxy test
```

---



## 5\. 测试

Clash 开启系统代理，选择规则模式，在 WSL 中执行

```
proxy set
proxy test
```
终端输出为:
```
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
```
说明代理正常工作.

关闭代理:
```
proxy unset
proxy test
```
终端输出为：
```
curl: (56) Recv failure: Connection reset by peer
```

说明已停止工作.
