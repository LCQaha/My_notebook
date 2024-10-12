# anaconda 环境安装指南

Anaconda 是一个开源的 Python 发行版，它集成了 Python、R、Julia、Scala、C++、Java 等语言，并且可以安装其他语言的包。

这份指南参考的几份资料在在下方列出：

[Jack Cui：别再折腾开发环境了，一劳永逸的搭建方法](https://mp.weixin.qq.com/s/ghKGVutz9RQ4fMGJDHWtww)

[一文弄懂 Jupyter 的配置与使用(呕心沥血版)](https://blog.jiumoz.com/archives/yi-wen-nong-dong-jupyter-de-pei-zhi-yu-shi-yong--ou-xin-li-xue-ban-)

[清华大学开源软件镜像站-Anaconda 软件仓库](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)

## Anaconda 安装

[简介待完善]
可以通过这个网址下载 anaconda 安装包：[https://www.anaconda.com/download/success](https://www.anaconda.com/download/success)。

下载后直接安装。

若使用的系统为 Windows，安装完成后，需要将环境变量添加到 PATH 中。（Mac 和 linux 会在安装过程中提示是否添加环境变量）通过`此电脑-右键-属性-高级系统设置-环境变量系统变量-path`添加如下路径（以安装路径`D:\Anaconda3`为例）：

```
D:\Anaconda3\Scripts
D:\Anaconda3
```

## Jupyter Notebook 安装与使用

### 安装 Jupyter Notebook

【有误】这一段应表示为：在`python`环境或`anaconda`环境安装的不同点，本节待修改。
1.pip 安装
先将 pip 工具更新到最新版

```sh
pip install --upgrade pip
```

然后安装 jupyter notebook

```sh
pip install jupyter
```

2.anaconda 安装
安装 Anaconda。从 Anaconda 官方网站下载最新版本的 Anaconda。

启动 Anaconda Navigator。安装完毕后，启动 Anaconda Navigator，它是一个可视化的应用程序，方便用户管理和运行 Anaconda 中包含的各种工具和应用。

安装 Jupyter Notebook。在 Anaconda Navigator 中，点击左侧导航栏的 “Environments”，然后在右边的区域中选中需要安装 Jupyter Notebook 的环境，在下方的 “Packages” 标签页中搜索 “jupyter”，并勾选 “jupyter” 和 “notebook”。然后点击 “Apply” 按钮进行安装即可。

**注意**：要在系统变量中添加，而不是用户变量。（个人认为是为了安装一次，所有系统一起用）

### 常见问题

1. 初次打开 jupyter notebook 时，文件目录十分混乱，如何指定它的工作目录？

   Jupyter notebook 是随 Python 一起安装的，安装位置为对应版本 Python 的安装目录下的 Scripts 文件夹下，如：`C:\Program Files\Python36\Scripts\jupyter-notebook.exe`，同时这也是 jupyter notebook 的默认启动目录。（若是 Anaconda 环境，默认路径是）因此，只要指定启动目录（一般是代码文件的存放地址），就可以得到一个简洁的画面了。具体步骤如下：

   - 在命令提示窗口输入`jupyter notebook --generator-config`
   - 此处需要注意的是，如果你已经配置过 notebooks 的相关信息，执行此命令会提示你是否覆盖原有配置。如果是首次执行此命令，则生成配置到相应目录。如下图所示，输入 y 直接覆盖。
     ![](.\img\jupyter_notebook_config.png)
   - 在返回值中可以看到配置文件的储存位置，记住这个位置
   - 打开生成的配置文件（这里是`C:\Users\85035\.jupyter`），找到`## The directory to use for notebooks and kernels.`然后更改两行之后的`#c.NotebookApp.notebook_dir = ''`或`#c.ServerApp.root_dir = ''`，在单引号中填入你想要保存的位置即可。
     **注意**：这行代码被注释掉了，修改完成后记得删掉前面的`#`
     如果你不理解上述操作，可以[点击这里](.\tongyi\通义千问：jupyter的一些小知识.md)，或许可以解答你的疑惑。

2.

## 指令

这一部分的指令包括 s

### 创建一个环境

```
conda create -n env_name
```

这条指令用于创建一个环境，其中：
`-n`同`--name`，用于命名新环境。
`env_name` 为环境名称。

可以在创建环境的同时安装一些包，比如：

```sh
conda create --name env_name numpy pandas
```

在创建环境`env_name`同时安装了 `numpy` 和 `pandas`包。

### 若干常用指令汇总

### help

当你不知道 conda 的命令时，可以输入如下命令来获取 anaconda 的帮助信息。

```sh
conda --help
```

这个指令会产生如下输出：

```

C:\Users\admin>conda --help
usage: conda-script.py [-h] [--no-plugins] [-V] COMMAND ...

conda is a tool for managing and deploying applications, environments and packages.

options:
  -h, --help          Show this help message and exit.
  --no-plugins        Disable all plugins that are not built into conda.
  -V, --version       Show the conda version number and exit.

commands:
  The following built-in and plugins subcommands are available.

  COMMAND
    build             Build conda packages from a conda recipe.
    clean             Remove unused packages and caches.
    compare           Compare packages between conda environments.
    config            Modify configuration values in .condarc.
    content-trust     Signing and verification tools for Conda
    convert           Convert pure Python packages to other platforms (a.k.a., subdirs).
    create            Create a new conda environment from a list of specified packages.
    debug             Debug the build or test phases of conda recipes.
    develop           Install a Python package in 'development mode'. Similar to `pip install --editable`.
    doctor            Display a health report for your environment.
    env               See `conda env --help`.
    index             Update package index metadata files. Pending deprecation, use https://github.com/conda/conda-
                      index instead.
    info              Display information about current conda install.
    init              Initialize conda for shell interaction.
    inspect           Tools for inspecting conda packages.
    install           Install a list of packages into a specified conda environment.
    list              List installed packages in a conda environment.
    metapackage       Specialty tool for generating conda metapackage.
    notices           Retrieve latest channel notifications.
    pack              See `conda pack --help`.
    package           Create low-level conda packages. (EXPERIMENTAL)
    remove (uninstall)
                      Remove a list of packages from a specified conda environment.
    rename            Rename an existing environment.
    render            Expand a conda recipe into a platform-specific recipe.
    repo              See `conda repo --help`.
    run               Run an executable in a conda environment.
    search            Search for packages and display associated information using the MatchSpec format.
    server            See `conda server --help`.
    skeleton          Generate boilerplate conda recipes.
    token             See `conda token --help`.
    update (upgrade)  Update conda packages to the latest compatible version.
    verify            See `conda verify --help`.

C:\Users\admin>

```

显然，这对不懂英语的人来讲是一个灾难，下面我们来翻译一下：

```sh
usage: conda-script.py [-h] [--no-plugins] [-V] COMMAND ...

conda 是一个用于管理和部署应用程序、环境和包的工具。

选项:
  -h, --help          显示此帮助信息并退出。
  --no-plugins        禁用所有非内置插件。
  -V, --version       显示 conda 版本号并退出。

命令:
  以下是一些内置和插件子命令。

  COMMAND
    build             从 conda 配方构建 conda 包。
    clean             移除未使用的包和缓存。
    compare           比较 conda 环境之间的包。
    config            修改 .condarc 中的配置值。
    content-trust     Conda 的签名和验证工具
    convert           将纯 Python 包转换为其他平台（即，子目录）。
    create            从指定的包列表创建一个新的 conda 环境。
    debug             调试 conda 配方的构建或测试阶段。
    develop           以“开发模式”安装 Python 包。类似于 `pip install --editable`。
    doctor            显示你的环境健康报告。
    env               见 `conda env --help`。
    index             更新包索引元数据文件。即将废弃，改用 https://github.com/conda/conda-index。
    info              显示当前 conda 安装的信息。
    init              初始化 conda 以便与 shell 交互。
    inspect           用于检查 conda 包的工具。
    install           在指定的 conda 环境中安装包列表。
    list              列出 conda 环境中已安装的包。
    metapackage       用于生成 conda 元包的专用工具。
    notices           获取最新频道通知。
    pack              见 `conda pack --help`。
    package           创建低级别的 conda 包。（实验性）
    remove (uninstall) 移除指定 conda 环境中的包列表。
    rename            重命名现有的环境。
    render            将 conda 配方扩展为平台特定的配方。
    repo              见 `conda repo --help`。
    run               在 conda 环境中运行可执行文件。
    search            使用 MatchSpec 格式搜索包并显示相关信息。
    server            见 `conda server --help`。
    skeleton          生成 conda 配方模板。
    token             见 `conda token --help`。
    update (upgrade)  将 conda 包更新到最新的兼容版本。
    verify            见 `conda verify --help`。
```

## 一些问题的解答

1. 【存疑】我的 anaconda 软件安装在了 d 盘，但新建的环境出现在了 c 盘，我该如何解决？
   答：这是由于文件夹权限不足导致的。首先我们需要将 anaconda 安装目录下的 `env` 文件夹属性作如下修改：将安全-用户的权限改为全部允许。
   然后，在 `C:\Users\用户名`下有一个 `.condarc` 文件，将其打开，添加下面内容：

   ```
   envs_dirs:
     - E://Env//anaconda//envs
   ```

2. 【未解决】输入指令 `conda create -n my` 可正常生成环境并使用 `conda info -e` 查看，但如果将指令改为 `conda create -n my jupyter notebook` 环境可以正常打开，但无法通过指令发现。

### 清华大学镜像站的使用

1. 清华镜像站提供 Anaconda 安装包的下载，具体下载链接如下：
   [https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)
2. 镜像站提供 Anaconda 仓库
