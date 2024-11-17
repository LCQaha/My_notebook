# 小土堆 Pytorch 笔记

## 安装

```sh
conda

conda install nb_conda

torch.cuda.is_available()   # 检查GPU是否可用

```

## 基础

### 0.技巧

1. `dir()`
   显示方法名。
   `dir(torch)`
2. `help()`
   显示方法详细用途。

### 1. 三种运行场景

1. IDE（Python 文件）
   IDE 下每次运行的是整个 Python 文件。代码是以块运行的整体，Python 文件的块是所有行的代码。
   便于传播，适用于大型项目。
2. 命令行（控制台）
   控制台中以行（或几行）为单位运行。
   可以显示每一个变量属性，但不利于阅读与修改。
3. Jupyter Notebook
   可以人为拆分代码，且不影响阅读。
   利于阅读与修改。

### Pytorch 加载数据

1. `Dataset`
   提供一种获取数据及其 lable 的方式。

2. `Dataloader`
   为后面的网络提供不同的数据形式。
