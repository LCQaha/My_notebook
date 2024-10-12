# 通义千问：jupyter的一些小知识

在 Jupyter 的配置体系中，`c.NotebookApp.notebook_dir` 和 `c.ServerApp.root_dir` 都是用来指定 Jupyter Notebook 或 JupyterLab 启动时的工作目录，但它们分别对应了不同的配置类和不同的历史阶段。

### `c.NotebookApp.notebook_dir`

`c.NotebookApp.notebook_dir` 是早期 Jupyter Notebook 配置中使用的变量名称。它用于指定 Jupyter Notebook 的工作目录，即笔记本文件所在的目录。

### `c.ServerApp.root_dir`

随着 Jupyter 项目的演进，Jupyter Notebook 的核心组件逐渐迁移到了一个更通用的应用程序框架——Jupyter Server。在这个框架中，`c.ServerApp.root_dir` 成为了新的配置项，用于指定服务器的工作目录。这个配置项不仅适用于 Jupyter Notebook，也适用于 JupyterLab 和其他基于 Jupyter Server 的应用程序。

### 历史背景和演变

- **早期版本**：在早期版本的 Jupyter Notebook 中，使用的是 `NotebookApp` 类来进行配置。
- **新框架**：随着 Jupyter 项目的发展，引入了一个更加通用的 `ServerApp` 类来统一处理 Jupyter Notebook 和 JupyterLab 的配置。

### 配置文件示例

在您的配置文件中，您可能会看到这两个配置项都被注释掉了：

```python
# c.NotebookApp.notebook_dir = ''
# c.ServerApp.root_dir = ''
```

这是因为默认情况下，Jupyter 使用用户的主目录作为工作目录。如果您想更改这个默认设置，可以选择其中一个配置项来设置您的工作目录。

### 如何选择

- 如果您使用的是较新的 Jupyter 版本（通常是从 Jupyter Notebook 5.x 开始），建议使用 `c.ServerApp.root_dir`。
- 如果您使用的是非常老的版本，可能仍然需要使用 `c.NotebookApp.notebook_dir`。

### 示例

假设您想将工作目录设置为 `C:\Users\Lenovo\Documents\MyProjects`，您可以在配置文件中设置如下：

```python
c.ServerApp.root_dir = 'C:\\Users\\Lenovo\\Documents\\MyProjects'
```

或者，如果您确实需要使用 `NotebookApp` 类（这通常意味着您使用的是一个非常老的版本），则可以设置：

```python
c.NotebookApp.notebook_dir = 'C:\\Users\\Lenovo\\Documents\\MyProjects'
```

### 命令行覆盖

无论您在配置文件中如何设置，都可以通过命令行参数来覆盖这些设置：

```sh
jupyter notebook --notebook-dir=C:\\Users\\Lenovo\\Documents\\MyProjects
```

或者：

```sh
jupyter notebook --root-dir=C:\\Users\\Lenovo\\Documents\\MyProjects
```

### 总结

- `c.NotebookApp.notebook_dir` 和 `c.ServerApp.root_dir` 都用于指定 Jupyter Notebook 的工作目录。
- 推荐使用 `c.ServerApp.root_dir`，因为它适用于更广泛的 Jupyter 应用程序，并且是新框架的一部分。
- 您可以根据实际使用的 Jupyter 版本来选择合适的配置项。