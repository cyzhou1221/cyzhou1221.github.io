+++
title = "PyTorch 实验环境搭建" 
date = "2025-06-19"
series = "Environment Setup"
+++

# 配置全局环境
1. 设置 GPU 加速 for WSL2，如果已经安装 WSL2，只需安装与显卡型号对应的 Windows 驱动即可

   https://docs.nvidia.com/cuda/wsl-user-guide/index.html

2. 安装 miniconda 

   https://www.anaconda.com/docs/getting-started/miniconda/install#linux

   用于为每个项目创建相应的虚拟环境

   1. 取消默认激活 `base` 环境
      ```
      conda config --set auto_activate false
      ```

   2. 更换 conda 镜像源为清华源
      https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/

      配置文件参数说明：https://docs.conda.io/projects/conda/en/stable/user-guide/configuration/mirroring.html

      输入 `conda info` 查看是否换源成功

      


# 配置 GMVAE 项目环境

通过项目配置文件生成环境

https://docs.conda.io/projects/conda/en/stable/user-guide/tasks/creating-projects.html

创建 `environment.yml` 文件，采用 Python 3.12 

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

使用 pip 安装 Pytorch

https://pytorch.org/get-started/locally/
