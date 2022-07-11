---
title: DeepLearning 学习笔记 - 01
date: 2022-02-09 15:12:50
id: DeepLearning/01
categories:
- 学习笔记
tags:
- 深度学习
- Python
- 编程
hidden: false
---

基于 WSL2 的 DL 开发环境配置, 使用 Python 库: Pytorch.  
{% post_link "DeepLearning学习笔记" "点击查看索引" %}

<!-- more -->

## WSL2 的安装与配置  

见 {% post_link "Windows11下WSL2的安装与配置" %} .  

## Anaconda 的安装与使用  

### Anaconda 的安装

打开 [Anaconda 官网](https://www.anaconda.com/) , 选择 *Products - Individual Edition* 进入下载界面(如图1), 选择其他版本(如图2), 下载 Linux 版本(如图3).  

![](./anaconda.png)
![](./individual.png)
![](./linux.png)

之后打开 Terminal, `cd` 至下载文件位置, 运行命令: `sh Anacondaxxxx.sh`, 其中要改为你自己下载的文件名. 然后就开始安装啦, 都选默认就可以.

### Anaconda 的配置  

安装完成后, 在命令行输入 `conda`, 会发现提示 "*command not found*". 这是因为 shell 找不到 conda 的位置, 需要配置环境变量.  
用编辑器打开 **/.zshrc** (如果是 bash 就打开 **/.bashrc**), 在文件中添加:  

```shell
export PATH=/home/username/anaconda3/bin:$PATH
```

其中要将 bin 的地址换为你的安装路径.  
保存退出后更新配置:  

```shell
source /.zshrc
```

输入: `conda init zsh` 在 zsh 中初始化 conda.  
输入: `conda -V` 检测是否安装成功, 若输出:  

```
conda x.x.x
```

即代表配置成功.  

### Anaconda 的使用  

Anaconda 除了自带许多我们所需要的 **数据处理和科学计算** 的 Python Packages 外, 还具有环境管理的功能, 为了防止不同工作中所需要的 Packages 产生冲突, 我们最好为不同的工作需求配置不同的工作环境, 虚拟环境就应运而生.  
创建虚拟环境: `conda create --name DL python=3.9.7` 其中 *--name* 后是所创建环境的名称.  
查看环境列表: `conda info -e` 或 `conda env list`.  
切换虚拟环境: `conda activate DL`.  
查看环境中安装的 Packages: `conda list`.  
其他的不一一列举, 如有需要请自行 Google.  
这里我们创建了一个名为 **DL** 的虚拟环境并切换到其中, 此时就可以进行后续 Python 的配置啦~  

## Python 环境的配置  

配置完 conda 之后, 我们就可以开始 Python 环境的搭建了.  

在 Terminal 中输入: `python` 即可进入 Python 的命令行, 代表 Python 安装成功.  

### 安装 Pytorch  

在 DL 领域, 有两个框架: TensorFlow 和 Pytorch, 这两个有一定的区别, 我才疏学浅无法叙述. 个人来讲, 选择 Pytorch 的原因是对 Python 的偏好.  

进入 [Pytorch 官网](https://pytorch.org/), 进入下载界面, 选择 **Stable - Linux - Conda - Python - CUDA 11.3**, 复制下方的命令在 Terminal 中运行, 文件较大需要耐心等待.  

注意: 需要在 `conda install` 后加入 `-n DL`, 才能在 DL 环境中安装.  

安装完成后, 进入 Python 命令行, 输入以下代码进行测试:  

```python
import torch
a = torch.rand(6, 6)
a
```

得到类似于以下的运行结果即代表安装成功:  

![](./torch.png)

### CUDA 与 cuDNN 的安装  

这方面网络上有许多教程, 此处只简述:  

1. 进入 [CUDA 官网](https://developer.nvidia.cn/cuda-toolkit) 下载安装 **wsl 版本** 的 **与 Pytorch 相对应** 的版本的 CUDA Toolkit.  

![](./cuda.png)

2. cuDNN 是能够免费下载的, 但是需要注册 NVIDA 的账号, 之后下载 **与 CUDA 相对应** 的版本的 cuDNN, 并根据网络上的教程进行安装.  

![](./cudnn.png)

3. 测试 CUDA 和 cuDNN:  

在 Python 命令行输入:  

```python
import torch
from torch.backends import cudnn
torch.cuda.is_available()
cudnn.is_available()
```

若输出均为 `True` 即安装成功.  

![](./cudatest.png)

## Jupyter Notebook 的安装与配置  

### Jupyter Notebook 的开启  

因为 Jupyter Notebook 是 Anaconda 自带的, 所以可以开箱即用.  
在 Terminal 中输入 `jupyter notebook` 即可开启 Jupyter Notebook.  
之后进入 [](http://127.0.0.1:8888) 即可看到 Jupyter Notebook 的界面.  

### 在 VSCode 中使用 Jupyter Notebook  

打开 VSCode, 安装插件: **Python**, **Jupyter Notebook**.  
之后创建一个后缀为 **.ipynb** 的文件, kernel 选择 math, 即可开始使用.  