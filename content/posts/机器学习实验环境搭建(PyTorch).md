---
title: 机器学习实验环境搭建(PyTorch) 
date: 2025-07-12
categories: 环境搭建
---

<!--more-->

# 1. 配置全局环境
1. 设置 GPU 加速 for WSL2，只需安装与显卡型号对应的 Windows 驱动即可

   点击 https://www.nvidia.cn/drivers/lookup/ , 选择对应的 Windows 版本驱动进行下载.

   驱动安装成功之后, 在 WSL 中输入 `nvidia-smi` 查看显卡型号信息是否正常显示.

2. 安装 Miniconda 

   https://www.anaconda.com/docs/getting-started/miniconda/install#linux

   用于为每个项目创建相应的虚拟环境

   1. 取消默认激活 `base` 环境
      ```
      conda config --set auto_activate false
      ```

   2. 更换 conda 镜像源为清华源
      https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/

      将 `~/.condarc` 文件设置如下
      
      ```
      channels:
        - defaults
      show_channel_urls: true
      default_channels:
        - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
        - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
        - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
      ```
   
   设置好之后, 输入 `conda info` 查看是否换源成功.



# 2. 配置 GMVAE 项目环境

通过命令行创建环境(Simple)
```
conda create -n GMVAE pip
```

通过项目配置文件生成环境 https://docs.conda.io/projects/conda/en/stable/user-guide/tasks/creating-projects.html

1. 创建 `environment.yml` 文件，这里采用 Python 3.12 
    ```
    name: GMVAE
    channels:
      - defaults
    dependencies:
      - python=3.12
      - matplotlib
      - scipy
      - scikit-learn
    ```

2. 输入  
    ```
    conda env --create environment.yml
    ```
    进行环境创建

3. 激活环境
    ```
    conda activate GMVAE
    ```

4. 使用 pip(创建环境安装 Python 时会自动安装) 安装 Pytorch https://pytorch.org/get-started/locally/

    选择好对应的配置, 会出现对应的安装命令, 如:

    ```
    pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128
    ```

    复制到命令行进行安装即可
    
    (注: 这是 CUDA 12.8 版本对应的 PyTorch, 而 `nvidia-smi` 显示本机 CUDA 版本是 12.9, 经过测试, 可以兼容.)



# 3. 测试

进入 `GMVAE-python/pytorch` 文件夹, 输入

```
python3 main.py
```

程序正常运行, 输入 `nvidia-smi` 查看 GPU 使用情况.
