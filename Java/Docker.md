# Docker

## 简介

### 参考资料

[1] (Docker从入门到实践)[https://yeasy.gitbook.io/docker_practice]

## 快速入门

### 安装

#### 生产环境 or 开发环境

> 1. 生产环境（服务器部署）：
> 
>     - 优先使用官方 APT/YUM 源安装（Ubuntu、Debian、Fedora、CentOS）
> 
>     - 优势：获得官方安全更新、长期技术支持、版本管理清晰
> 
>     - 安装步骤稍多一些，但这种“麻烦”是值得的——它为你的生产系统争取了稳定性和可维护性
> 
> 2. 开发环境（本地开发机、测试服务器）：
> 
>     - 配置 Docker 官方源后 使用包管理器安装，或在一次性测试环境使用官方脚本自动安装
> 
>     - 如果你想快速上手，官方脚本（get.docker.com）是最便捷的选择
> 
>     - 国内用户注意：这一步一定要选对镜像源，否则网络卡顿会严重影响体验

1. Docker 不是只有一种安装方式，不同场景应该选不同方式。

    - Ubuntu 服务器 / 云服务器 / VMware 里的 Ubuntu：
        优先用 Docker 官方 APT 源安装 Docker Engine。

    - Windows / macOS 本地开发：
        优先用 Docker Desktop。

    - 临时测试、一次性机器：
        可以用 get.docker.com 脚本快速安装，但不建议用于正式生产环境。

#### 开始安装（以Ubuntu为例）

1. 卸载旧包
    - 在安装 Docker 官方版本之前，建议先卸载系统中可能已经存在的旧版 Docker 相关包。
    - 如果不清理这些旧的安装包，后续通过官方源安装可能出现版本冲突、依赖冲突等问题。

    ```bash
    for pkg in docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc; 
    do 
        sudo apt remove -y "$pkg" || true 
    done
    ```


2. 更新 APT 缓存并安装基础依赖

    安装 Docker 前，需要先更新 APT 软件包列表，并安装一些基础工具。

    ```bash
    sudo apt update
    sudo apt install -y ca-certificates curl
    ```
    * `ca-certificates`：用于支持 HTTPS 证书校验；
    * `curl`：用于下载 Docker 官方 GPG key。

3. 创建 Docker APT 密钥目录

    APT 需要通过 GPG key 验证软件包来源。

    Docker 官方 GPG key 建议放在：

    ```text
    /etc/apt/keyrings/
    ```

    所以需要先创建这个目录。

    ```bash
    sudo install -m 0755 -d /etc/apt/keyrings
    ```

4. 添加 Docker 官方 GPG key

    GPG key 用来验证软件包是否来自 Docker 官方仓库，防止软件包在下载过程中被篡改。

    ```bash
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

    sudo chmod a+r /etc/apt/keyrings/docker.asc
    ```

5. 添加 Docker 官方 APT 源

    Ubuntu 默认软件源里也可能有 Docker 相关包，但不是 Docker 官方推荐的主线安装来源。

    添加 Docker 官方 APT 源后，APT 就多了一个新的软件仓库：

    ```text
    https://download.docker.com/linux/ubuntu
    ```

    ```bash
    sudo tee /etc/apt/sources.list.d/docker.sources > /dev/null <<EOF
    Types: deb
    URIs: https://download.docker.com/linux/ubuntu
    Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
    Components: stable
    Architectures: $(dpkg --print-architecture)
    Signed-By: /etc/apt/keyrings/docker.asc
    EOF
    ```

6. 重新更新 APT 缓存

    ```bash
    sudo apt update
    ```

7. 安装 Docker 官方组件

    推荐安装的组件包括：

    - `docker-ce`
    - `docker-ce-cli`
    - `containerd.io`
    - `docker-buildx-plugin`
    - `docker-compose-plugin`

    它们共同组成一套完整的 Docker 使用环境。    

    ```bash
    sudo apt install -y \
        docker-ce \
        docker-ce-cli \
        containerd.io \
        docker-buildx-plugin \
        docker-compose-plugin
    ```

    - `docker-ce`：Docker Engine 社区版，也就是 Docker 后台服务本体。

    - `docker-ce-cli`：Docker 命令行工具，提供 `docker` 命令。

    - `containerd.io`：底层容器运行时，负责容器生命周期管理。

    - `docker-buildx-plugin`：增强版镜像构建插件，提供 `docker buildx` 能力。

    - `docker-compose-plugin`：Docker Compose 插件，提供：

        ```bash
        docker compose
        ```

8. 启动 Docker 并设置开机自启

    Docker 安装完成后，需要确保 Docker 后台服务正在运行。

    Docker 后台服务运行后，才能执行：

    ```bash
    docker run
    docker ps
    docker pull
    ```

    ```bash
    sudo systemctl enable --now docker
    ```


    这条命令等价于同时执行：

    ```bash
    sudo systemctl enable docker
    sudo systemctl start docker
    ```

9. 检查 Docker 服务状态

    ```bash
    sudo systemctl status docker --no-pager
    ```


10. 【可选】将当前用户加入 docker 用户组

    默认情况下，普通用户直接执行 Docker 命令可能会遇到权限问题。

    例如：

    ```bash
    docker ps
    ```

    可能提示没有权限访问 Docker socket。

    可以把当前用户加入 `docker` 用户组，这样以后执行 Docker 命令时可以不用每次加 `sudo`。

    但要注意：
    - docker 用户组权限很高，接近 root 权限。
    - 学习机可以这样做，生产服务器要谨慎。

    ```bash
    getent group docker || sudo groupadd docker
    sudo usermod -aG docker "$USER"
    ```

    执行后，需要退出当前终端并重新登录，或者临时执行：

    ```bash
    newgrp docker
    ```

    让用户组权限生效。

11. 测试 Docker 是否安装成功

    通过运行官方测试镜像 `hello-world`，可以验证 Docker 的基本功能是否正常。

    这个测试会验证：

    - Docker 命令是否可用；
    - Docker 后台服务是否可用；
    - Docker 是否能拉取镜像；
    - Docker 是否能创建并运行容器。

    ```bash
    docker run --rm hello-world
    ```

    如果看到：

    ```text
    Hello from Docker!
    ```

    说明 Docker 安装成功。

    如果看到如下提示，建议直接配置镜像源后访问
    ```bash
    lcq@lcq-VMware-Virtual-Platform:~$ docker run --rm hello-world
    Unable to find image 'hello-world:latest' locally
    docker: Error response from daemon: failed to resolve reference "docker.io/library/hello-world:latest": failed to do request: Head "https://registry-1.docker.io/v2/library/hello-world/manifests/latest": dial tcp 162.125.32.10:443: connect: connection refused

    Run 'docker run --help' for more information
    ```

13. 可选：配置 Docker 镜像加速器

    国内网络环境下，拉取 Docker Hub 镜像可能比较慢。
    **若使用甲骨文等国外服务器，则直接使用官方源，切勿切换国内源。**

    例如：

    ```bash
    docker pull nginx
    docker pull mysql
    docker pull redis
    ```

    如果速度很慢，可以配置 Docker 镜像加速器。

    **注意**：
    - 安装 Docker 慢：是 APT 软件源问题。
    - 拉取镜像慢：是 Docker 镜像仓库访问问题。


    镜像加速器主要解决的是后者。

    ```bash
    sudo mkdir -p /etc/docker

    sudo tee /etc/docker/daemon.json > /dev/null <<EOF
    {
    "registry-mirrors": [
        "https://你的镜像加速器地址"
    ]
    }
    EOF

    sudo systemctl daemon-reload        # 重新加载 systemd 配置
    sudo systemctl restart docker       # 重启 Docker 服务，让配置生效
    ```

    查看是否配置成功：

    ```bash
    docker info
    ```

    如果配置成功，可以在输出中看到 `Registry Mirrors` 相关内容。

#### 镜像说明

1. 详情
    - 由于国内镜像源于2024年已陆续停用，故当前在开发环境需要找合适的镜像。
    - 如果是云服务器，建议使用厂商自己的Docker镜像服务（通常是k8s），参考：[链接1](https://yeasy.gitbook.io/docker_practice/di-yi-bu-fen-ru-men-pian/03_install/3.9_mirror)

2. 个人开发用（DaoCloud）
    
    ```bash
    sudo mkdir -p /etc/docker

    sudo tee /etc/docker/daemon.json > /dev/null <<EOF
    {
    "registry-mirrors": [
        "https://docker.m.daocloud.io"
    ]
    }
    EOF

    sudo systemctl daemon-reload
    sudo systemctl restart docker

    docker info
    ```

3. 【施工中。。。】云厂商配置待补充

### 第一个实例
#### Docker 部署一个 Nginx + HTML 示例

1. 准备项目目录

    * Docker 项目建议统一放在 `/opt/项目名` 下。
        
        ```text
        /opt/dockerstudy-web/
        ├── Dockerfile
        ├── compose.yml
        ├── .env
        └── src/
            └── index.html
        ```
    - `/opt/dockerstudy-web/`：项目部署目录，位于宿主机上。

    - `Dockerfile`：负责定义镜像如何构建。

    - `compose.yml`：负责定义容器如何运行。

    - `.env`：负责统一管理项目名、版本号、宿主机端口等变量。

    - `src/`：放静态网页源文件。

    - `src/index.html`：需要被打包进 Nginx 镜像的页面文件。

    * `/opt/dockerstudy-web` 是宿主机上的项目目录，用来管理 Dockerfile、compose.yml、源码、配置等文件。

    ```bash
    sudo mkdir -p /opt/dockerstudy-web/src
    sudo chown -R lcq:lcq /opt/dockerstudy-web
    cd /opt/dockerstudy-web
    ```
    - 学习环境中可以把目录临时交给 lcq 用户管理，方便编辑。

    - 正式上线环境中，/opt/项目名 通常不应随便交给普通用户修改，应由 root 或专门的 deploy 用户受控管理。

2. 准备网页文件

    * `src/index.html` 是宿主机上的网页源文件。
    * 后面会通过 Dockerfile 复制进镜像。

    ```bash
    cat > src/index.html <<'EOF'
    <h1>Hello, Docker!</h1>
    EOF
    ```

3. 编写 Dockerfile

    * Dockerfile 用来描述镜像如何构建。
    * `nginx:1.27-alpine` 是基础镜像。
    * `/usr/share/nginx/html/index.html` 是 Nginx 容器内部的默认网页路径。

    ```bash
    cat > Dockerfile <<'EOF'
    FROM nginx:1.27-alpine
    COPY src/index.html /usr/share/nginx/html/index.html
    EOF
    ```

    注意：

    ```text
    /opt/dockerstudy-web/src/index.html
    ```
    是宿主机路径。

    ```text
    /usr/share/nginx/html/index.html
    ```    
    是容器内部路径。
    

4. 编写 `.env`

    * `.env` 用来统一管理项目名、版本号、端口等变量。
    * 正式部署不要依赖 `latest`，应使用明确版本号。

    ```bash
    cat > .env <<'EOF'
    APP_NAME=dockerstudy-web
    APP_VERSION=20260628-01
    HOST_PORT=18080
    EOF
    ```

    - `APP_NAME`：镜像名称。

    - `APP_VERSION`：当前发布版本。

    - `HOST_PORT`：宿主机本地监听端口。

5. 编写 `compose.yml`

   * 不建议正式项目长期使用裸 `docker run`。
   * 使用 Docker Compose 统一管理容器更清晰。
   * 容器只绑定到 `127.0.0.1:18080`，不直接暴露给外网。

   ```bash
   cat > compose.yml <<'EOF'
   services:
     web:
       image: ${APP_NAME}:${APP_VERSION}
       restart: unless-stopped
       ports:
         - "127.0.0.1:${HOST_PORT}:80"
       healthcheck:
         test: ["CMD-SHELL", "wget -qO- http://127.0.0.1/ >/dev/null || exit 1"]
         interval: 30s
         timeout: 5s
         retries: 3
         start_period: 10s
   EOF
   ```

    - `image: ${APP_NAME}:${APP_VERSION}`：使用 .env 中定义的镜像名称和版本号。

    - `restart: unless-stopped`：Docker 重启后自动恢复容器，除非用户手动停止。

    - `ports: 127.0.0.1:18080:80`
        表示：
        ```text
        宿主机 127.0.0.1:18080
            ↓
        容器内部 80 端口

        只允许宿主机本机访问该端口，外部不能直接访问。
        ```
    - `healthcheck`：用于检查容器内部 Nginx 是否正常提供页面。

6. 构建镜像

   * 根据 Dockerfile 构建镜像。
   * 镜像名和版本号要和 `.env` 保持一致。

   ```bash
   docker build -t dockerstudy-web:20260628-01 .
   ```

    查看镜像：

    ```bash
    docker images | grep dockerstudy-web
    ```

    此时镜像中已经包含静态页面：

    ```text
    dockerstudy-web:20260628-01
    └── /usr/share/nginx/html/index.html
    ```

7. 启动容器

   * 使用 Docker Compose 启动服务。
   * `-d` 表示后台运行。

   ```bash
   docker compose up -d
   ```

8. 查看运行状态

   ```bash
   docker compose ps
   ```

   查看日志：

   ```bash
   docker compose logs -f
   ```

9. 本机访问测试

   * 容器内部 Nginx 监听 80 端口。
   * 宿主机通过 `127.0.0.1:18080` 访问容器。

   ```bash
   curl http://127.0.0.1:18080
   ```

   访问流程：

   ```text
   宿主机 127.0.0.1:18080
       ↓
   Docker 端口映射
       ↓
   容器内部 80 端口
       ↓
   Nginx 读取 /usr/share/nginx/html/index.html
   ```

10. 【可选】用宿主机 Nginx 对外转发

    * 正式部署中，外部用户一般不直接访问 Docker 容器端口。
    * 推荐由宿主机 Nginx 统一作为入口。

    ```nginx
    server {
        listen 80;
        server_name dockerstudy.example.com;

        location / {
            proxy_pass http://127.0.0.1:18080;

            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
    ```

    访问流程：

    ```text
    用户 / Cloudflare
        ↓
    宿主机 Nginx :80
        ↓
    127.0.0.1:18080
        ↓
    Docker 容器内部 Nginx :80
        ↓
    index.html
    ```

11. 版本更新

    * 修改源码后，不要覆盖旧镜像。
    * 构建一个新版本镜像。
    * **注意：不修改版本号就无法对当前发布内容进行覆盖**

    ```bash
    docker build -t dockerstudy-web:20260628-02 .
    ```

    修改 `.env`：

    ```bash
    APP_VERSION=20260628-02
    ```

    重新启动：

    ```bash
    docker compose up -d
    ```

12. 版本回退

    * 如果新版本有问题，把 `.env` 改回旧版本。

    ```bash
    APP_VERSION=20260628-01
    ```

    重新启动：

    ```bash
    docker compose up -d
    ```

13. 停止和清理

    停止并删除容器：

    ```bash
    docker compose down
    ```

    查看镜像：

    ```bash
    docker images | grep dockerstudy-web
    ```

    删除指定旧镜像：

    ```bash
    docker rmi dockerstudy-web:20260628-02
    ```

## 基本概念

### 基本概念

1. **镜像 (Image)**
    Docker 镜像是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数 (如匿名卷、环境变量、用户等)。镜像不包含任何动态数据，其内容在构建之后也不会被改变。

2. **容器 (Container)**
    镜像 (Image) 和容器 (Container) 的关系，就像是面向对象程序设计中的 类 和 实例 一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。

3. **仓库 (Repository)**
    镜像构建完成后，可以很容易的在当前宿主机上运行，但是，如果需要在其它服务器上使用这个镜像，我们就需要一个集中的存储、分发镜像的服务，Docker Registry 就是这样的服务。

### 镜像

#### 基本概念

1. 基本定义
    镜像就是运行应用所需的一切代码、库、环境、配置的集合

2. 镜像是一个特殊的文件系统，包含：
    - 程序文件，如：二进制文件、代码解释起
    - 库文件：libc、OpenSSL等依赖库
    - 配置文件，如：`nginx.conf`、`my.conf`等
    - 环境变量：`PATH`、`LANG`等
    - 元数据：启动命令、暴露端口、数据卷定义。


#### 镜像分层

1. 介绍
    - 镜像的分层存储机制是 Docker 最具创新性的特性之一。通过 Union FS 技术，Docker 能够高效地构建和管理镜像。
    - 通过分层可以减少重复存储、减少传输量、提高构建缓存复用效率。
    - 需要注意，分层并不是让单个镜像的逻辑内容凭空变小，而是让多个镜像之间的相同层只保存一份。

2. 分层存储举例

    ```mermaid
    flowchart LR
        %% Docker 分层方式
        subgraph Docker["Docker 分层方式  总计：620MB ✅"]
            direction TB

            subgraph AppLayer["应用层"]
                direction LR
                A["App A<br/>50MB"]
                B["App B<br/>30MB"]
                C["App C<br/>40MB"]
            end

            U["Ubuntu<br/>共享基础层<br/>500MB"]

            A --> U
            B --> U
            C --> U
        end

        %% 传统不分层方式
        subgraph Traditional["传统方式（不分层） 总计：1.5GB ❌"]
            direction TB
            TA["App A<br/>Ubuntu 500MB"]
            TB["App B<br/>Ubuntu 500MB"]
            TC["App C<br/>Ubuntu 500MB"]
        end
    ```
    - 如果没有分层机制，每个镜像都需要各自保存一份完整的基础环境，会造成大量重复存储。
    - Docker 分层后，相同的基础层、软件层、依赖层都可以被多个镜像共同引用。
    - 这些层在逻辑上属于每个镜像的一部分，但在物理存储上只保存一份。
    - 因此，分层主要降低的是 Docker 镜像层的实际磁盘占用和镜像拉取、推送时的传输成本。

3. 分层存储进阶
    - 以构建一个简单的Nginx镜像为例
        ```docker
        FROM ubuntu:24.04                              # 第 1 层：基础系统（约 78MB）
        RUN apt-get update && apt-get install -y nginx # 第 2 层：更新索引并安装 nginx
        COPY app.conf /etc/nginx/                      # 第 3 层：复制配置文件
        ```
    - 基座Ubuntu为第一层
    - 安装Nginx为第二层
    - 配置文件为第三层
    - 可以共享的不只有基础系统层。只要某一层的上一层、构建指令、相关输入内容完全一致，这一层就可以被复用。
    - 因此，Ubuntu / Alpine 基础层可以共享，Nginx 安装层、Python 依赖层、Node 依赖层、JDK 层也可以共享。
    - 项目代码层通常变化最频繁，因此一般不能长期共享。

    ```mermaid
    flowchart BT
        U["Ubuntu 基础层<br/>共享基础系统"]
        N["Nginx 安装层<br/>共享软件层"]
        A["App A 代码层<br/>独有层"]
        B["App B 代码层<br/>独有层"]

        U --> N
        N --> A
        N --> B
    ```

5. 构建缓存的判断条件

    Docker 构建缓存并不是只看当前 Dockerfile 指令文字是否相同，而是综合判断：

    ```text
    上一层是否相同
    当前指令是否相同
    相关输入文件是否相同
    构建参数是否相同
    ```

    例如：

    ```dockerfile
    COPY index.html /usr/share/nginx/html/
    ```

    即使命令文字没有变化，只要 `index.html` 的内容发生变化，这一层也需要重新构建。

    又如：

    ```dockerfile
    RUN apt-get update && apt-get install -y nginx
    ```

    如果上一层不同，即使命令相同，也不能复用同一个层。

4. 注意事项1：切勿盲目分层
    ```docker
    ## 错误示范 ❌

    FROM ubuntu:24.04
    RUN apt-get update
    RUN apt-get install -y build-essential  # 安装编译工具（约 200MB）
    RUN make && make install                  # 编译应用
    RUN apt-get remove build-essential        # 试图删除编译工具

    ## 结果：镜像仍然包含 200MB 的编译工具！
    ```
    - 由于编译应用层和删除层是分开的，所以编译工具会被保留在镜像中，只是后续在删除后变得“不可见”，实际上仍然占据空间

    - Dockerfile 中的 `RUN` 发生在 `docker build` 阶段。
    - 每条 `RUN` 执行后，Docker 会把该步骤产生的文件变化保存为新的镜像层。
    - 因此，`RUN apt-get install -y build-essential` 安装的编译工具会进入镜像层，而不是进入普通容器的可写层。
    - 后续单独执行 `RUN apt-get remove build-essential` 时，只是在新的层中记录“删除标记”，让这些文件在最终文件系统视图中不可见。
    - 但前面安装编译工具的那一层仍然存在，所以镜像体积不会真正减小。

2. 注意事项2：在同一层安装、使用、清理
    ```docker
    ## 正确做法 ✅

    FROM ubuntu:24.04
    RUN apt-get update && \
        apt-get install -y build-essential && \
        make && make install && \
        apt-get remove -y build-essential && \
        apt-get autoremove -y && \
        rm -rf /var/lib/apt/lists/*

    ## 在同一层完成安装、使用、清理
    ```
    - 这样做有两个好处
    - 首先，没有将update操作单独分层，这样可以避免分层存储机制存储旧的update信息，导致软件版本滞后。
    - 通过命令组合构建一个独一无二的层，运行完主动删除附加组件，这样可以避免无用的内容继续存在于镜像中。
    - `apt-get update` 和 `apt-get install` 放在同一个 `RUN` 中，可以避免 Docker 复用旧的包索引缓存，降低安装失败或包索引过期的风险。
    - 安装、编译、清理放在同一个 `RUN` 中，可以让 Docker 最终只记录该层执行完成后的文件系统结果。
    - 如果编译工具在同一层内被安装又被删除，最终提交镜像层时就不会保留这些临时工具。
    - `rm -rf /var/lib/apt/lists/*` 用于清理 apt 包索引缓存，进一步减少镜像体积。

3. 注意事项3：编译型项目优先使用多阶段构建

    如果项目需要编译，例如 C/C++、Go、Java、前端 Node 构建等，更推荐使用多阶段构建。

    示例：

    ```dockerfile
    # 第 1 阶段：构建阶段
    FROM ubuntu:24.04 AS builder

    RUN apt-get update && \
        apt-get install -y build-essential

    COPY . /src
    WORKDIR /src

    RUN make


    # 第 2 阶段：运行阶段
    FROM ubuntu:24.04

    COPY --from=builder /src/myapp /usr/local/bin/myapp

    CMD ["myapp"]
    ```

    说明：

    ```text
    builder 阶段：
        包含 gcc、make、build-essential 等构建工具
        负责把程序编译出来

    最终阶段：
        不复制 build-essential
        只复制编译好的可执行文件和运行所需文件
    ```

    多阶段构建的好处是：构建依赖只存在于构建阶段，最终运行镜像更加干净。

#### 查看镜像的构建历史

1. 使用如下命令查看镜像历史
    ```bash
    docker history [镜像名]:[版本号]
    ```

2. 示例
    ```bash
    lcq@lcq-VMware-Virtual-Platform:/opt/dockerstudy-web$ docker history dockerstudy-web:20260628-01
    IMAGE          CREATED         CREATED BY                                       SIZE      COMMENT
    3d03d517dbe5   2 days ago      COPY src/index.html /usr/share/nginx/html/in…   24.6kB    buildkit.dockerfile.v0
    <missing>      14 months ago   RUN /bin/sh -c set -x     && apkArch="$(cat …   38.7MB    buildkit.dockerfile.v0
    <missing>      14 months ago   ENV NJS_RELEASE=1                                0B        buildkit.dockerfile.v0
    <missing>      14 months ago   ENV NJS_VERSION=0.8.10                           0B        buildkit.dockerfile.v0
    <missing>      14 months ago   CMD ["nginx" "-g" "daemon off;"]                 0B        buildkit.dockerfile.v0
    <missing>      14 months ago   STOPSIGNAL SIGQUIT                               0B        buildkit.dockerfile.v0
    <missing>      14 months ago   EXPOSE map[80/tcp:{}]                            0B        buildkit.dockerfile.v0
    <missing>      14 months ago   ENTRYPOINT ["/docker-entrypoint.sh"]             0B        buildkit.dockerfile.v0
    <missing>      14 months ago   COPY 30-tune-worker-processes.sh /docker-ent…   16.4kB    buildkit.dockerfile.v0
    <missing>      14 months ago   COPY 20-envsubst-on-templates.sh /docker-ent…   12.3kB    buildkit.dockerfile.v0
    <missing>      14 months ago   COPY 15-local-resolvers.envsh /docker-entryp…   12.3kB    buildkit.dockerfile.v0
    <missing>      14 months ago   COPY 10-listen-on-ipv6-by-default.sh /docker…   12.3kB    buildkit.dockerfile.v0
    <missing>      14 months ago   COPY docker-entrypoint.sh / # buildkit           8.19kB    buildkit.dockerfile.v0
    <missing>      14 months ago   RUN /bin/sh -c set -x     && addgroup -g 101…   5.36MB    buildkit.dockerfile.v0
    <missing>      14 months ago   ENV DYNPKG_RELEASE=1                             0B        buildkit.dockerfile.v0
    <missing>      14 months ago   ENV PKG_RELEASE=1                                0B        buildkit.dockerfile.v0
    <missing>      14 months ago   ENV NGINX_VERSION=1.27.5                         0B        buildkit.dockerfile.v0
    <missing>      14 months ago   LABEL maintainer=NGINX Docker Maintainers <d…   0B        buildkit.dockerfile.v0
    <missing>      16 months ago   CMD ["/bin/sh"]                                  0B        buildkit.dockerfile.v0
    <missing>      16 months ago   ADD alpine-minirootfs-3.21.3-x86_64.tar.gz /…   8.5MB     buildkit.dockerfile.v0
    lcq@lcq-VMware-Virtual-Platform:/opt/dockerstudy-web$ docker history
    docker: 'docker history' requires 1 argument

    Usage:  docker history [OPTIONS] IMAGE

    Run 'docker history --help' for more information
    lcq@lcq-VMware-Virtual-Platform:/opt/dockerstudy-web$ docker history
    dockerstudy-web:20260628-01  dockerstudy-web:20260628-02  hello-world:latest
    ```

3. 示例解读
    - 这时之前快速入门用到的示例，镜像内容如下：
        ```text
        dockerstudy-web:20260628-02
        ├── index.html 层：28.7kB
        ├── nginx 官方镜像层：38.7MB
        ├── nginx 启动脚本层：若干 kB
        ├── nginx 用户/目录层：5.36MB
        └── alpine 基础系统层：8.5MB
        ```
    
4. 输出结果说明

    - `CREATED BY` 表示该层大致由哪条 Dockerfile 指令或构建步骤产生。
    - `SIZE` 表示该层相对于上一层新增的大小，不是整个镜像的总大小。
    - `0B` 通常表示该步骤只修改了镜像元数据，没有新增可见的文件系统内容。
    - `<missing>` 不表示镜像层丢失，而是表示该历史步骤对应的中间镜像 ID 没有单独显示。
    - 只要镜像可以正常运行，说明实际 layer 是存在的。

5. 示例结论

    在本例中，`dockerstudy-web:20260628-01` 和 `dockerstudy-web:20260628-02` 的历史记录几乎完全相同，只有最上面的 `COPY` 层不同：

    ```text
    20260628-01：COPY 层 24.6kB
    20260628-02：COPY 层 28.7kB
    ```

    这说明两个镜像共享了相同的 `nginx:alpine` 基础层，包括：

    ```text
    Alpine 基础系统层
    Nginx 安装层
    Nginx 启动脚本层
    Nginx 默认元数据
    ```

    两个镜像真正不同的只是最上面的应用文件层。

#### 镜像迁移与层共享

1. 镜像迁移不是只迁移项目代码层

    如果使用如下命令导出镜像：

    ```bash
    docker save dockerstudy-web:20260628-01 -o dockerstudy-web.tar
    ```

    导出的内容不是只有最上层的 `index.html`，而是该镜像运行所需的完整层集合和镜像元数据。

    例如：

    ```text
    dockerstudy-web.tar
    ├── Alpine 基础层
    ├── Nginx 安装层
    ├── Nginx 启动脚本层
    ├── index.html 应用层
    └── 镜像元数据
    ```

2. 目标机器不需要提前安装基础层

    在新机器上执行：

    ```bash
    docker load -i dockerstudy-web.tar
    ```

    即使新机器原来没有 `nginx:alpine` 或 Alpine 基础层，也可以导入并运行该镜像。

3. Docker 会按层去重

    导入或拉取镜像时，Docker 会检查本机是否已经存在相同 layer：

    ```text
    本机已有该层：复用，不重复保存
    本机没有该层：导入或下载
    ```

    因此，层共享不是“镜像缺少基础层”，而是“镜像逻辑上引用基础层，Docker 在物理存储上对相同层去重”。

#### 镜像的标识

1. 镜像名称和标签

    镜像通常使用如下格式表示：

    ```text
    [仓库地址/]仓库名[:标签]
    ```

    示例：

    ```text
    nginx:latest
    nginx:1.27.5-alpine
    ubuntu:24.04
    dockerstudy-web:20260628-01
    ```

    如果省略标签，默认使用 `latest`：

    ```bash
    docker pull nginx
    ```

    等价于：

    ```bash
    docker pull nginx:latest
    ```

2. 镜像 ID

    可以使用如下命令查看镜像 ID：

    ```bash
    docker images
    ```

    示例：

    ```text
    REPOSITORY        TAG            IMAGE ID       SIZE
    dockerstudy-web   20260628-01    3d03d517dbe5   ...
    ```

    `IMAGE ID` 可以理解为镜像内容的一种标识。

3. 镜像摘要
    ```bash
    docker images --digests
    ```

    镜像摘要是基于镜像内容计算出的哈希值，通常形如：

    ```text
    sha256:xxxxxxxx
    ```

    标签可能被覆盖，例如 `latest` 今天和明天可能指向不同镜像；但摘要和镜像内容绑定，更适合生产环境精确锁定版本。

#### 镜像的来源

Docker 镜像常见来源有四种：

1. 从镜像仓库拉取

    ```bash
    docker pull nginx:alpine
    ```

    这是最常见方式。Docker 会从 Docker Hub 或配置的镜像仓库下载镜像所需的层。

2. 从 Dockerfile 构建

    ```bash
    docker build -t dockerstudy-web:20260628-01 .
    ```

    该方式用于制作自己的业务镜像。

3. 从容器提交生成镜像

    ```bash
    docker commit 容器名 新镜像名:标签
    ```

    该方式会把某个容器当前状态提交成镜像，但不推荐作为正式构建方式，因为过程不透明、难以复现。

4. 从文件导入镜像

    ```bash
    docker load -i dockerstudy-web.tar
    ```

    常用于离线迁移镜像。

### 容器

#### 基本概念

1. 介绍
    - 容器是 Docker 技术的核心，是应用实际运行的载体。
    - 容器是镜像的运行实例。如果把镜像比作程序，那么容器就是进程。 用面向对象编程的术语来说：镜像是类 (Class)，容器是对象 (Instance)。

2. 特点

    - 一个镜像可以创建多个容器

    - 每个容器相互独立，互不影响

    - 容器可以被创建、启动、停止、删除、暂停

#### 容器与进程

1. 介绍

    - 容器的本质是一个特殊的进程
        ```mermaid
        flowchart LR
            subgraph Container["容器进程（运行在宿主机内核上）"]
                direction TB
                A["• 独立进程空间<br/>• 独立网络环境<br/>• 独立文件系统<br/>• 独立用户空间"]
            end

            subgraph Process["普通进程"]
                direction TB
                B["• 共享系统资源<br/>• 共享网络<br/>• 共享文件系统"]
            end

            style Container fill:#ffffe6,stroke:#999900,stroke-width:1px
            style Process fill:#ffffe6,stroke:#999900,stroke-width:1px
            style A fill:#eeeeff,stroke:#9966ff,stroke-width:1px
            style B fill:#eeeeff,stroke:#9966ff,stroke-width:1px
        ```

2. 这种隔离主要通过 Linux 内核的 Namespace 实现，资源限制通常与 cgroups 配合。具体表现为：

    - 进程空间：容器看不到宿主机上的其他进程。

    - 网络：在默认网络模式下，容器通常拥有独立的网络命名空间，并可分配独立IP；使用`host`或`container:`等模式时则例外。

    - 文件系统：容器拥有独立的`root`目录。

    - 用户：默认情况下，容器内的`root`仍是`uid 0`，但通常只拥有受限capabilities；如果启用`userns-remap`或`rootless`等机制，还会进一步映射为宿主机上的低权限用户。

3. Namespace

    Namespace 用来让容器“看不到外面”。

    - `PID Namespace`：让容器有自己的进程编号空间

    - `NET Namespace`：让容器有自己的网络环境

    - `MNT Namespace`：让容器有自己的文件系统挂载视图

    - `UTS Namespace`：让容器有自己的主机名

    - `IPC Namespace`：隔离进程间通信

    - `USER Namespace`：隔离用户和用户组编号

4. cgroups 
    用来限制容器能用多少资源。

    例如：

    - 最多使用多少 CPU
    - 最多使用多少内存
    - 最多使用多少磁盘 IO
    - 最多使用多少进程数量

    例如：
    ```bash
    docker run -d --name web --memory 256m nginx
    ```
    说明：

    `--memory 256m`限制这个容器最多使用 256MB 内存

#### 容器与虚拟机

| 特性 | 容器 | 虚拟机 |
| :--- | :--- | :--- |
| **隔离级别** | 操作系统级 (Namespaces/cgroups) | 硬件虚拟化级 (Hypervisor) |
| **启动时间** | 通常更快 | 通常更慢 |
| **资源占用** | 通常更低 | 通常更高 |
| **运行开销** | 通常更接近原生 | 通常有更高虚拟化开销 |
| **内核** | 共享宿主机内核 | 各自独立内核 |

#### 容器存储层

1. 介绍
    容器运行后，在镜像只读层上面额外加的一层可写区域。

2. `Copy-on-Write`：写时复制
    当容器需要修改镜像层中的文件时：

    - Docker 将该文件 复制 到容器存储层

    - 在容器层中进行修改

    - 原始镜像层保持不变

    - **修改文件只对当前容器有效，随容器删除消失（停止不会消失）**

#### 正确的持久化方式1：Volume 数据卷

1. 介绍
    Volume 是 Docker 管理的数据存储位置。
    - 适合：

        - 数据库数据
        - 应用持久化数据
        - 正式环境数据保存

2. 常用命令
    - 创建数据卷：
        ```bash
        docker volume create mydata
        ```
    - 查看数据卷：
        ```bash
        docker volume ls
        ```
    - 查看数据卷详情：
        ```bash
        docker volume inspect mydata
        ```
    - 使用数据卷：
        ```bash
        docker run -d \
        --name mysql \
        -v mysql-data:/var/lib/mysql \
        mysql
        ```


3. 说明：

    - `mysql-data `表示 Docker 管理的数据卷名称

    - `/var/lib/mysql`表示容器内 MySQL 保存数据的位置

4. 运行效果：

    MySQL 写入 /var/lib/mysql 的数据实际保存到 mysql-data 数据卷中

5. 删除容器：
    ```bash
    docker rm -f mysql
    ```
    数据卷不会自动删除。

6. 再次使用同一个数据卷：
    ```bash
    docker run -d \
    --name mysql2 \
    -v mysql-data:/var/lib/mysql \
    mysql
    ```
    新容器仍然可以读取原来的数据库数据。


#### 正确的持久化方式2：Bind Mount 绑定挂载

1. 介绍
    Bind Mount 是把宿主机目录直接挂载到容器目录中。
    - 适合：

        - 开发环境同步代码
        - 挂载 Nginx 静态网页目录
        - 挂载配置文件
        - 挂载日志目录
        - 调试容器文件

2. 示例：
    ```bash
    docker run -d \
    --name web \
    -p 8080:80 \
    -v /opt/dockerstudy-web/src:/usr/share/nginx/html \
    nginx:alpine
    ```
2. 说明：

    - `/opt/dockerstudy-web/src`是宿主机目录

    - `/usr/share/nginx/html`是容器内 Nginx 默认网页目录

3. 效果：

    - 容器访问 `/usr/share/nginx/html`实际读写宿主机 `/opt/dockerstudy-web/src`

    所以修改宿主机文件：
    ```bash
    vim /opt/dockerstudy-web/src/index.html
    ```
    容器中的 Nginx 可以直接读取到变化，不需要重新构建镜像。



#### 容器的生命周期


1. 基本介绍
    容器不是“创建完就一直存在的黑盒子”，它有自己的状态流转过程。

    最常见的几个状态是：
    ```text
    Created  ->  Running  ->  Paused  ->  Running ->  Stopped  ->  Deleted
    ```
    可以简单理解为：

    - `Created`：容器已经创建出来了，但还没开始跑
    - `Running`：容器正在运行
    - `Paused`：容器被暂停，里面的进程先“冻住”
    - `Stopped`：容器停止运行了
    - `Deleted`：容器被删除了

2. 图解

    ```mermaid
    flowchart TD
        S((start)) -->|docker create| C["Created"]

        C -->|docker start| R["Running"]
        S -->|docker run| R

        R -->|docker pause| P["Paused"]
        P -->|docker unpause| R

        R -->|docker stop / docker kill| T["Stopped"]
        T -->|docker start| R

        C -->|docker rm| D["Deleted"]
        T -->|docker rm| D
        R -->|docker rm -f| D

        D --> E((end))
    ```

#### 常见命令与状态变化

1. 创建容器

    ```bash
    docker create nginx
    ```

    * 作用：基于镜像创建一个容器
    * 结果：容器进入 `Created` 状态
    * 特点：只创建，不启动


2. 创建并启动容器

    ```bash
    docker run nginx
    ```

    * 作用：创建容器并立即启动
    * 本质上等价于：

    ```bash
    docker create nginx
    docker start 容器ID
    ```

3. 启动容器

    ```bash
    docker start abc123
    ```

    * 作用：启动一个已经存在但当前未运行的容器
    * 状态变化：

    ```text
    Created -> Running
    Stopped -> Running
    ```

    注意：

    * `docker start` 只能启动**已存在**的容器
    * 不能拿它直接启动一个镜像

    

4. 停止容器

    ```bash
    docker stop abc123
    ```

    * 作用：优雅停止容器
    * 状态变化：

    ```text
    Running -> Stopped
    ```

    Docker 的处理方式一般是：

    1. 先给容器主进程发送 `SIGTERM`
    2. 等待一段时间
    3. 如果还没退出，再发送 `SIGKILL`

    所以它属于“尽量正常收尾”的停止方式。

5. 强制停止容器

    ```bash
    docker kill abc123
    ```

    * 作用：强制杀掉容器
    * 状态变化本质上也是：

    ```text
    Running -> Stopped
    ```

    区别是：

    * `stop`：尽量优雅退出
    * `kill`：直接强杀，通常发送 `SIGKILL`

    适合容器卡死、无法正常停止时使用。



6. 暂停容器

    ```bash
    docker pause abc123
    ```

    * 作用：暂停容器内所有进程
    * 状态变化：

    ```text
    Running -> Paused
    ```

    暂停不是停止。

    区别：

    * `Stopped`：进程已经退出
    * `Paused`：进程还在，但先冻结，不继续执行


7. 恢复容器

    ```bash
    docker unpause abc123
    ```

    * 作用：恢复被暂停的容器
    * 状态变化：

    ```text
    Paused -> Running
    ```

8. 删除容器

    ```bash
    docker rm abc123
    ```

    * 作用：删除容器
    * 一般要求容器先处于停止状态
    * 状态变化：

    ```text
    Stopped -> Deleted
    Created -> Deleted
    ```

    如果容器正在运行，普通 `docker rm` 会失败。

9. 强制删除运行中的容器

    ```bash
    docker rm -f abc123
    ```

    * 作用：强制删除容器
    * 本质上相当于：

    ```text
    先强制停止
    再删除
    ```

    也就是：

    ```text
    Running -> Stopped -> Deleted
    ```



#### 容器与进程的关系

1. 核心结论
    容器的生命周期 = 容器主进程（PID 1）的生命周期


    也就是说：

    * 主进程在运行，容器就在运行
    * 主进程退出了，容器就停止

2. 强调“主进程”

    因为一个容器里可能会有多个进程，但 Docker 关注的是**主进程**。

    这个主进程通常是容器内的：

    ```text
    PID 1
    ```

    Docker 判断容器是否还活着，主要看这个 PID 1 是否还在运行。


3. 为什么 `docker run ubuntu` 会立刻退出？

    例如：

    ```bash
    docker run ubuntu
    ```

    很多人第一次执行会发现：

    ```text
    容器马上就退出了
    ```

    原因是：

    * `ubuntu` 镜像默认不会帮你启动一个长期前台服务
    * 默认执行的命令很快就结束了
    * 主进程一结束，容器也就结束了

    如果这样运行：

    ```bash
    docker run -it ubuntu bash
    ```

    才更符合“进入 Ubuntu 容器交互”的预期。

    说明：

    * `-i`：保持标准输入
    * `-t`：分配终端
    * `bash`：让 `bash` 作为主进程运行

    只要你不退出 `bash`，容器就保持运行。


3.  为什么 `docker run nginx` 不会立刻退出？

    例如：

    ```bash
    docker run nginx
    ```

    Nginx 官方镜像通常会使用类似：

    ```bash
    nginx -g 'daemon off;'
    ```

    这句命令的关键点是：

    ```text
    daemon off;
    ```

    意思是：

    ```text
    不要让 Nginx 进入后台守护进程模式，而是保持在前台运行
    ```

    这样：

    * Nginx 主进程一直在前台
    * Docker 就认为容器还活着
    * 容器会持续保持 `Running`


#### 容器的隔离性

1. 基本介绍

    Docker 容器之所以看起来像“独立的小系统”，靠的不是虚拟机技术，而是 Linux 内核的 **Namespace** 机制。

    Namespace 的作用可以概括为：让容器“看不到外面”，也让外面“看起来和它隔开”




1. `PID Namespace`

    - 隔离内容：进程 ID

    - 效果

        * 容器内有自己的进程编号空间
        * 容器里的应用进程通常会看到自己是 `PID 1`
        * 容器看不到宿主机上的其他普通进程

        例如在容器中执行：

        ```bash
        ps -ef
        ```

        可能只看到容器内部的少量进程。

        而宿主机上执行同样命令，会看到整台机器上的大量进程。

    - 意义：让容器像拥有自己独立的进程世界



2. `NET Namespace`

    - 隔离内容：网络
    - 效果

        * 容器有独立网络栈
        * 可以有自己的 IP 地址
        * 有自己的端口空间
        * 有自己的路由、网卡、DNS 配置等

        所以容器内监听 `80` 端口，不等于宿主机也直接监听 `80`。

        通常需要端口映射：

        ```bash
        docker run -p 8080:80 nginx
        ```

        表示：

        ```text
        宿主机 8080 -> 容器 80
        ```

    - 特殊情况

        如果使用：

        * `--network host`
        * `--network container:xxx`

        那么网络隔离会减弱或改变。


3. `MNT Namespace`

    - 隔离内容：挂载点 / 文件系统视图

    - 效果

        容器有自己独立的根目录视图，例如：

        ```text
        /
        ├── bin
        ├── etc
        ├── usr
        └── var
        ```

        虽然底层还是宿主机文件系统的一部分，但容器看到的是“属于自己”的挂载视图。

        这就是为什么容器里看起来像有自己的操作系统目录结构。

    - 意义：让容器像拥有独立文件系统


4. `UTS Namespace`

    - 隔离内容：主机名、域名

    - 效果

        容器可以有自己的 hostname。

        例如启动容器时可以指定：

        ```bash
        docker run --hostname myweb nginx
        ```

        容器内部看到的主机名会是：

        ```text
        myweb
        ```

        而不是宿主机的主机名。


5. `IPC Namespace`

    - 隔离内容：进程间通信资源


        例如：

        * 信号量
        * 消息队列
        * 共享内存

    - 效果

        一个容器里的 IPC 资源通常不会直接和另一个容器混在一起。

    - 意义：避免不同容器之间互相干扰进程通信资源

6. `USER Namespace`

    - 隔离内容：用户和组 ID


    - 效果

        容器内看到的 `root`，不一定等于宿主机上真正拥有完全权限的 root。

        默认情况下：

        * 容器内 root 仍是 `uid 0`
        * 但通常能力受限（capabilities 受限）

        如果进一步开启：

        * `userns-remap`
        * `rootless`

        那么容器内的 root 还能映射成宿主机上的低权限用户。

    - 意义：减少容器逃逸或高权限操作带来的风险


7.  隔离性总结表

    | Namespace | 隔离内容  | 效果                         |
    | --------- | ----- | -------------------------- |
    | PID       | 进程 ID | 容器内 PID 1 是应用进程，看不到宿主机其他进程 |
    | NET       | 网络    | 独立网络栈、IP 地址、端口             |
    | MNT       | 文件系统  | 独立根目录和挂载点                  |
    | UTS       | 主机名   | 独立 hostname 和域名            |
    | IPC       | 进程间通信 | 独立的消息队列、信号量、共享内存           |
    | USER      | 用户    | 独立的用户和组 ID 视图              |

### 仓库

#### 基本介绍

1. 介绍

    Docker Registry 是存储和分发 Docker 镜像的服务，类似于代码的 GitHub 或包管理的 npm。

2. Registry 是什么

    Registry 可以理解成：Docker 镜像仓库服务器

    它的作用是：

    - 存储镜像
    - 分发镜像
    - 管理镜像版本
    - 控制镜像访问权限

####  Registry、仓库、标签的关系

1. 介绍

    - `Registry`：镜像仓库服务，比如 Docker Hub、ghcr.io、私有仓库
    - `Repository`：某个镜像项目，比如 nginx、mysql、myapp
    - `Tag`：这个镜像项目的具体版本，比如 latest、1.28、alpine、v1.0
2. 类比

    | 场景        | 对应服务             |
    | --------- | ---------------- |
    | 代码托管      | GitHub / GitLab  |
    | npm 包     | npm Registry     |
    | Maven 包   | Maven Repository |
    | Docker 镜像 | Docker Registry  |

3. 图解
    ```mermaid
    flowchart TD
        R["Registry<br/>镜像仓库服务<br/>例如 Docker Hub / ghcr.io / 私有仓库"]

        R --> Repo1["Repository: nginx"]
        R --> Repo2["Repository: mysql"]
        R --> Repo3["Repository: mycompany/myapp"]

        Repo1 --> N1["Tag: latest"]
        Repo1 --> N2["Tag: 1.28"]
        Repo1 --> N3["Tag: alpine"]

        Repo2 --> M1["Tag: 8.4"]
        Repo2 --> M2["Tag: 5.7"]

        Repo3 --> A1["Tag: v1.0"]
        Repo3 --> A2["Tag: v1.1"]
    ```

#### 镜像名称格式

1. 完整格式
    ```text
    [registry地址/][用户名或组织名/]仓库名[:标签]
    ```

    例如：
    ```text
    registry.example.com/mycompany/myapp:v1.2.3
    ```

2. 解释

    - `registry.example.com`：Registry 地址

    - `mycompany`：用户名或组织名

    - `myapp`：仓库名

    - `v1.2.3`：标签

3. 注意：
    如果不指定 Registry 地址，默认使用 Docker Hub。如果不指定标签，默认使用 `latest`。



#### 镜像加速器

1. 配置
    - 修改配置文件
        ```bash
        sudo vim /etc/docker/daemon.json
        ```
    - 输入如下内容（地址视环境而定）
        ```json
        {
            "registry-mirrors": [
                "https://your-accelerator-url"
            ]
        }
        ```
    - 应用配置
        ```bash
        sudo systemctl daemon-reload
        sudo systemctl restart docker
        ```
1. 常见公共 Registry
    - `Docker Hub`
    - `GitHub Container Registry：ghcr.io`
    - `Google Artifact Registry`
    - `Quay.io`
    - `阿里云 ACR`：
        - https://cn.aliyun.com/product/acr
        - https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors
    - `腾讯云 TCR`：https://cloud.tencent.com/product/tke

**阿里云和腾讯云的仓库只服务于自己的云服务平台**

其中 Docker Hub 是 Docker 默认的公共 Registry。

需要注意，Google Container Registry 已经进入迁移/下线阶段，Google 官方文档说明 Container Registry 自 2025 年 3 月 18 日起关闭写入，应迁移到 Artifact Registry；部分 gcr.io 请求可由 Artifact Registry 兼容处理。

#### 镜像推送与拉取

1. 工作流程
    - 开发者先在本机构建镜像，再推送到 Registry。
    - 生产服务器从 Registry 拉取镜像，随后基于镜像运行容器。


    ```mermaid
    sequenceDiagram
        participant Dev as 开发者机器
        participant Reg as Registry
        participant Prod as 生产服务器

        Dev->>Dev: docker build<br/>构建本地镜像
        Dev->>Dev: docker tag<br/>增加远程仓库格式的名字
        Dev->>Reg: docker push<br/>推送镜像
        Reg-->>Reg: 存储镜像层和元数据
        Prod->>Reg: docker pull<br/>拉取镜像
        Prod->>Prod: docker run<br/>运行容器
    ```
    ```mermaid
    flowchart
        A["本地项目<br/>Dockerfile + src"] --> B["docker build<br/>构建镜像"]
        B --> C["本地镜像<br/>dockerstudy-web:20260702-01"]
        C --> D["docker tag<br/>改成平台要求的名字"]
        D --> E["docker push<br/>上传到 Docker Hub"]
        E --> F["Docker Hub<br/>lcq/dockerstudy-web:20260702-01"]
        F --> G["其他服务器<br/>docker pull"]
        G --> H["docker run<br/>运行容器"]
    ```


1.  常用命令

    - 登录 Registry
        ```bash
        $ docker login                      # 登录 Docker Hub
        $ docker login registry.example.com # 登录其他 Registry
        ```
    - 拉取镜像
        ```bash
        $ docker pull nginx:1.28
        ```
    - 标记镜像（准备推送）
        ```bash
        $ docker tag myapp:latest registry.example.com/myteam/myapp:v1.0
        ```
        - 目标 Registry：`registry.example.com`，如果没有自动推送Docker Hub
        - 目标仓库：`myteam/myapp`
        - 目标标签：`v1.0`，不指定则默认`latest`

    - 推送镜像
        ```bash
        $ docker push registry.example.com/myteam/myapp:v1.0
        ```
    - 登出
        ```bash
        $ docker logout
        ```

2. tag举例

    ```bash
    lcq@lcq-VMware-Virtual-Platform:/opt/dockerstudy-web$ docker images
                                                                                                                    i Info →   U  In Use
    IMAGE                         ID             DISK USAGE   CONTENT SIZE   EXTRA
    dockerstudy-web:20260628-01   3d03d517dbe5       73.6MB           21MB    U
    dockerstudy-web:20260628-02   7ff679feab14       73.6MB           21MB
    hello-world:latest            96498ffd522e       25.9kB         9.49kB
    lcq@lcq-VMware-Virtual-Platform:/opt/dockerstudy-web$ docker tag dockerstudy-web:20260628-01 aha
    lcq@lcq-VMware-Virtual-Platform:/opt/dockerstudy-web$ docker images
                                                                                                                    i Info →   U  In Use
    IMAGE                         ID             DISK USAGE   CONTENT SIZE   EXTRA
    aha:latest                    3d03d517dbe5       73.6MB           21MB    U
    dockerstudy-web:20260628-01   3d03d517dbe5       73.6MB           21MB    U
    dockerstudy-web:20260628-02   7ff679feab14       73.6MB           21MB
    hello-world:latest            96498ffd522e       25.9kB         9.49kB
    lcq@lcq-VMware-Virtual-Platform:/opt/dockerstudy-web$
    ```

3. 私有镜像仓库模板

    ```bash
    # 1. 构建本地镜像
    docker build -t 本地镜像名:标签 .

    # 2. 登录私有 Registry
    docker login Registry地址

    # 3. 给镜像增加私有 Registry 格式的名字
    docker tag 本地镜像名:标签 Registry地址/命名空间/仓库名:标签

    # 4. 推送镜像
    docker push Registry地址/命名空间/仓库名:标签

    # 5. 生产服务器登录私有 Registry
    docker login Registry地址

    # 6. 生产服务器拉取
    docker pull Registry地址/命名空间/仓库名:标签

    # 7. 生产服务器运行
    docker run -d --name 容器名 -p 宿主机端口:容器端口 Registry地址/命名空间/仓库名:标签
    ```


#### 镜像安全性

1. 介绍
    镜像安全不是只看“容器能不能跑”，而是要回答：

    - 这个镜像是不是可信来源？
    - 这个镜像有没有被别人篡改？
    - 这个镜像里面有没有已知漏洞？
    - 这个镜像里有没有不该出现的敏感信息？
    - 这个镜像是否适合生产环境长期使用？

2. Docker镜像包含的内容
    - 操作系统基础文件
    - Nginx / MySQL / Redis 等服务程序
    - 语言运行环境，例如 JDK、Python、Node.js
    - 项目代码
    - 第三方依赖
    - 配置文件
    - 启动脚本
    - 证书、密钥、环境变量文件

2. Docker镜像存在的风险
    1. 来源不可信：镜像作者可能不可靠
    2. 内容有漏洞：系统包或依赖存在 CVE
    3. 镜像被篡改：拉到的不是发布者原本发布的内容
    4. 版本不可控：latest 标签指向发生变化
    5. 敏感信息泄露：把密码、密钥、.env 文件打进镜像
    6. 运行权限过大：容器默认 root、挂载宿主机敏感目录等


2. 漏洞扫描
    ```bash
    ## 使用 Docker Scout 扫描镜像漏洞

    $ docker scout cves nginx:latest

    ## 使用 Trivy（开源工具）

    $ trivy image nginx:latest
    ```
    - 用于扫描已知CVE，即公开登记的已知安全漏洞编号。

        例如：
        ```text
        CVE-2024-xxxx
        CVE-2025-xxxx
        ```
    漏洞扫描工具会把镜像里的软件包版本和漏洞数据库进行比对。

3. 漏洞
    - 通常包括：
        - 漏洞编号：CVE ID
        - 严重等级：LOW / MEDIUM / HIGH / CRITICAL
        - 受影响的软件包
        - 当前版本
        - 修复版本
        - 漏洞描述
    - 主要看：
        - CRITICAL：严重漏洞，优先处理
        - HIGH：高危漏洞，通常也应处理
        - 是否有 Fixed Version：有没有修复版本
        - 漏洞是否影响你的实际使用场景


## 镜像的使用

### 获取镜像

#### `docker pull`

1. 命令
    ```bash
    docker pull [选项] [Registry地址/]仓库名[:标签]
    ```

2. 镜像名称格式
    ```text
    docker.io / library / ubuntu : 24.04
    ────┬────   ───┬───   ──┬───   ──┬──
        │          │        │        │
    Registry地址  用户名    仓库名    标签
    (可省略)    (可省略)
    ```
    | 组成部分 | 说明 | 默认值 |
    | :--- | :--- | :--- |
    | Registry 地址 | 镜像仓库地址 | `docker.io` (Docker Hub) |
    | 用户名 | 镜像所属用户/组织 | `library` (官方镜像) |
    | 仓库名 | 镜像名称 | **必须指定** |
    | 标签 | 版本标识 | `latest` |

3. 示例

    ```bash
    ## 完整格式

    $ docker pull docker.io/library/ubuntu:24.04

    ## 省略 Registry（默认 Docker Hub）

    $ docker pull library/ubuntu:24.04

    ## 省略 library（官方镜像）

    $ docker pull ubuntu:24.04

    ## 省略标签（默认 latest）

    $ docker pull ubuntu

    ## 拉取第三方镜像

    $ docker pull bitnami/redis:latest

    ## 从其他 Registry 拉取

    $ docker pull ghcr.io/username/myapp:v1.0
    ```

4. 常用选项
    
    - `--all-tags`、`-a`：拉取所有标签，如：`docker pull -a ubuntu`
    - `--platform`：指定平台，如：`docker pull --platform linux/arm64 nginx`
    - `--quiet`、`-q`：安静模式，如：`docker pull -q nginx`


4. 实战
    ```bash
    lcq@lcq-VMware-Virtual-Platform:/opt/dockerstudy-web$ docker pull ubuntu
    Using default tag: latest
    latest: Pulling from library/ubuntu
    a9be9fd915e9: Pull complete
    2c1ce1d0a589: Pull complete
    bce5ee84b2b4: Download complete
    Digest: sha256:b7f48194d4d8b763a478a621cdc81c27be222ba2206ca3ca6bc42b49685f3d9e
    Status: Downloaded newer image for ubuntu:latest
    docker.io/library/ubuntu:latest
    ```
    - 如果本地有相同的层，会跳过下载直接使用

#### 拉取并运行镜像

1. 命令
    ```bash
    ## 拉取镜像

    $ docker pull ubuntu

    ## 运行容器

    $ docker run -it --rm ubuntu:latest bash
    root@5ff170a6d981:/# ll
    ```
3. 常用参数
    - `-it`：交互式终端模式
    - `--rm`：退出后自动删除容器
    - `bash`：启动命令
4. 说明
    - `docker run`在需要时会自动执行`pull`，无需单独执行

#### 验证镜像完整性

1. 命令
    为了确保下载的镜像没有被篡改且内容一致，我们可以校验镜像的摘要 (Digest)。
    ```bash
    $ docker images --digests [镜像名]
    ```

2. 示例

    ```bash
    lcq@lcq-VMware-Virtual-Platform:/opt/dockerstudy-web$ docker images --digests ubuntu
    REPOSITORY   TAG       DIGEST                                                                    IMAGE ID       CREATED      SIZE
    ubuntu       latest    sha256:b7f48194d4d8b763a478a621cdc81c27be222ba2206ca3ca6bc42b49685f3d9e   b7f48194d4d8   5 days ago   160MB
    ```

3. 通过摘要拉取可以确保完全相同
    建议：生产环境使用摘要而非标签，防止拉取时拉去了错误的镜像
    ```bash
    $ docker pull ubuntu@sha256:4bc3ae6596938cb0d9e5ac51a1152ec9dcac2a1c50829c74abd9c4361e321b26
    ```

#### 清理磁盘空间

1. 命令

    ```bash
    ## 清理未使用的镜像

    $ docker image prune

    ## 清理所有未使用资源

    $ docker system prune
    ```

### 查看镜像

#### 列出本地镜像

1. 命令
    ```bash
    docker image ls
    ```

2. 示例
    ```bash
    lcq@lcq-VMware-Virtual-Platform:/opt/dockerstudy-web$ docker image list
                                                                                                                    i Info →   U  In Use
    IMAGE                         ID             DISK USAGE   CONTENT SIZE   EXTRA
    aha:latest                    3d03d517dbe5       73.6MB           21MB    U
    dockerstudy-web:20260628-01   3d03d517dbe5       73.6MB           21MB    U
    dockerstudy-web:20260628-02   7ff679feab14       73.6MB           21MB
    hello-world:latest            96498ffd522e       25.9kB         9.49kB
    ubuntu:latest                 b7f48194d4d8        160MB         45.3MB
    lcq@lcq-VMware-Virtual-Platform:/opt/dockerstudy-web$
    ```
    老版本：
    ```bash
    $ docker image ls
    REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
    redis        latest    5f515359c7f8   5 days ago     183MB
    nginx        latest    05a60462f8ba   5 days ago     181MB
    ubuntu       24.04     329ed837d508   3 days ago     78MB
    ubuntu       noble     329ed837d508   3 days ago     78MB
    ```
    新版本同样格式：
    ```bash
    lcq@lcq-VMware-Virtual-Platform:/opt/dockerstudy-web$ docker image ls --format "table {{.Repository}}\t{{.Tag}}\t{{.ID}}\t{{.CreatedSince}}\t{{.Size}}"
    REPOSITORY        TAG           IMAGE ID       CREATED        SIZE
    dockerstudy-web   20260628-02   7ff679feab14   4 days ago     73.6MB
    aha               latest        3d03d517dbe5   4 days ago     73.6MB
    dockerstudy-web   20260628-01   3d03d517dbe5   4 days ago     73.6MB
    ubuntu            latest        b7f48194d4d8   5 days ago     160MB
    hello-world       latest        96498ffd522e   3 months ago   25.9kB
    lcq@lcq-VMware-Virtual-Platform:/opt/dockerstudy-web$
    ```

3. 字段说明
    - `REPOSITORY`：镜像的仓库名，通常包含软件名称及所属用户或组织。
    - `TAG`：镜像的标签，通常用于表示软件的版本号或其他特定标识。
    - `IMAGE ID`：镜像的唯一标识符，这里显示的是短 ID（前 12 位）。
    - `CREATED`：镜像的创建时间或最后构建时间。
    - `SIZE`：镜像在本地磁盘上占用的空间大小。

#### 镜像过滤

1. 命令
    ```bash
    ## 列出所有 ubuntu 镜像

    $ docker images ubuntu
    REPOSITORY   TAG     IMAGE ID       SIZE
    ubuntu       24.04   329ed837d508   78MB
    ubuntu       noble   329ed837d508   78MB
    ubuntu       22.04   a1b2c3d4e5f6   72MB
    ```

2. 常用参数（过滤器`--filter`）

    - **`dangling=true`**
        - **说明**：筛选**虚悬镜像**（即没有仓库名和标签的镜像，通常显示为 `<none>`）。
        - **示例**：`docker images -f dangling=true`

    - **`before=镜像`**
        - **说明**：筛选在指定镜像**之前**创建的镜像。
        - **示例**：`docker images -f before=nginx:latest`

    - **`since=镜像`**
        - **说明**：筛选在指定镜像**之后**创建的镜像。
        - **示例**：`docker images -f since=nginx:latest`

    - **`label=key=value`**
        - **说明**：按镜像中定义的 LABEL 键值对进行过滤。
        - **示例**：`docker images -f label=maintainer=example@email.com`

    - **`reference=pattern`**
        - **说明**：按名称或标签的模式匹配（支持通配符 `*`）。
        - **示例**：`docker images -f reference='*:latest'`

2. 组合使用示例

    你也可以将多个过滤条件组合使用，例如同时列出特定标签且在某镜像之后创建的镜像：

    ```bash
    # 列出所有带 latest 标签且在 nginx:latest 之后创建的镜像
    docker images -f "since=nginx:latest" -f "reference=*:latest"
    ```

#### Docker 镜像格式化输出

1. 概述
    `docker images` 命令支持通过 `--format` 参数自定义输出格式，便于脚本处理或聚焦关键信息。同时也提供几个便捷选项简化常用查询。

2. 常用选项

    | 选项 | 说明 | 示例 |
    |------|------|------|
    | `-q` | 只输出镜像 ID（一行一个） | `docker images -q` |
    | `--no-trunc` | 显示完整镜像 ID（不截断） | `docker images --no-trunc` |
    | `--digests` | 显示镜像摘要（SHA256） | `docker images --digests` |

3. **实用组合：**
    ```bash
    # 删除所有镜像
    docker rmi $(docker images -q)

    # 删除所有 redis 镜像
    docker rmi $(docker images -q redis)
    ```

3. 自定义格式 (`--format`)
    使用 Go 模板语法控制输出内容和布局。

    - 基本用法
        ```bash
        # 只显示 ID 和仓库名，冒号分隔
        docker images --format "{{.ID}}: {{.Repository}}"

        # 表格形式（带标题行）
        docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"
        ```

    - 可用模板字段

        | 字段 | 说明 |
        |------|------|
        | `.ID` | 镜像 ID（短 ID） |
        | `.Repository` | 仓库名 |
        | `.Tag` | 标签 |
        | `.Digest` | 摘要（需配合 `--digests`） |
        | `.CreatedSince` | 创建后经过的时间（如 “5 days ago”） |
        | `.CreatedAt` | 完整创建时间 |
        | `.Size` | 镜像大小 |

4. 实用命令组合

    ```bash
    # 列出所有镜像及其大小，按大小排序（依赖系统 sort 命令）
    docker images --format "{{.Size}}\t{{.Repository}}:{{.Tag}}" | sort -h

    # 查找大于 500MB 的镜像
    docker images --format "{{.Size}}\t{{.Repository}}:{{.Tag}}" | grep -E "^[0-9]+GB|^[5-9][0-9]{2}MB"

    # 导出镜像列表（仓库名:标签）
    docker images --format "{{.Repository}}:{{.Tag}}" > images.txt
    ```

### 删除本地镜像

#### 基本用法

1. 命令

    ```bash
    docker rmi [选项] <镜像1> [<镜像2>...]
    docker image rm [选项] <镜像1> [<镜像2>...]
    ```

2. 标识镜像的方式
    删除镜像时，可以使用多种方式指定镜像。
    - 短id：id的前几位，要足以辨认出具体镜像，如：
        ```bash
        docker rmi 5f5
        ```
    - 长id：完整的镜像id
        ```bash
        docker rmi 5f515359c7f8
        ```

    - `镜像名:标签`
        ```bash
        docker rmi redis:latest
        ```
    - 镜像摘要
        ```bash
        docker rmi hello-world@sha256:96498ffd522e70807ab6384a5c0485a79b9c7c08ca79ba08623edcad1054e62d
        ```

3. 实操
    ```bash
    lcq@lcq-VMware-Virtual-Platform:/opt/dockerstudy-web$ docker rmi hello-world@sha256:96498ffd522e70807ab6384a5c0485a79b9c7c08ca79ba08623edcad1054e62d
    Untagged: hello-world:latest
    Deleted: sha256:96498ffd522e70807ab6384a5c0485a79b9c7c08ca79ba08623edcad1054e62d
    lcq@lcq-VMware-Virtual-Platform:/opt/dockerstudy-web$
    ```
    - `Untagged`：移除镜像标签
    - `Deleted`：移除镜像存储层

4. 删除存储层
    Docker 会检测镜像是否有容器依赖或其他标签指向，只有在确认为无用资源时才会真正删除存储层。
    ```mermaid
    graph TD
        Start([docker rmi redis:alpine]) --> Step1["1. Untag: 移除 redis:alpine 标签"]
        Step1 --> Decision1{"2. 检查是否还有其他标签指向此镜像"}
        Decision1 -- "有" --> Result1["只 Untag，不删除"]
        Decision1 -- "无" --> Decision2{"3. 检查是否有容器依赖"}
        Decision2 -- "有" --> Result2["报错，无法删除"]
        Decision2 -- "无" --> Decision3{"4. 从上到下逐层删除，检查每层是否被其他镜像使用"}
        Decision3 -- "被使用" --> Result3["保留该层"]
        Decision3 -- "未使用" --> Result4["Deleted (删除该层)"]

        style Start fill:#fff,stroke:#333,stroke-width:2px
        style Step1 fill:#e1f5fe,stroke:#01579b
        style Decision1 fill:#fff3e0,stroke:#e65100
        style Decision2 fill:#fff3e0,stroke:#e65100
        style Decision3 fill:#fff3e0,stroke:#e65100
        style Result1 fill:#e8f5e9,stroke:#2e7d32
        style Result2 fill:#ffebee,stroke:#c62828
        style Result3 fill:#e8f5e9,stroke:#2e7d32
        style Result4 fill:#e8f5e9,stroke:#2e7d32
    ```

#### 批量删除

1. 介绍
    手动一个一个删除镜像非常繁琐，Docker 提供了 image prune 命令和 shell 组合命令来实现批量清理。

2. 删除所有虚悬镜像
    虚悬镜像 (dangling)：没有标签的镜像，通常是旧版本被新版本覆盖后产生的
    ```bash
    ## 查看虚悬镜像

    $ docker images -f dangling=true

    ## 删除虚悬镜像

    $ docker image prune

    ## 不提示确认

    $ docker image prune -f
    ```

2. 删除所有未使用的镜像
    ```bash

    ## 删除所有没有被容器使用的镜像

    $ docker image prune -a

    ## 保留最近 24 小时的

    $ docker image prune -a --filter "until=24h"
    ```
3. 按条件删除
    ```bash

    ## 删除所有 redis 镜像

    $ docker rmi $(docker images -q redis)

    ## 删除 mongo:8.0 之前的所有镜像

    $ docker rmi $(docker images -q -f before=mongo:8.0)

    ## 删除某个时间之前的镜像

    $ docker image prune -a --filter "until=168h"  # 7天前
    ```

### 使用Dockerfile定制镜像

#### 基本介绍

1. 说明

   * 镜像定制本质上是：基于某个基础镜像，逐层添加文件、配置和命令执行结果。
   * `docker commit` 可以把容器当前状态保存成镜像，但不推荐作为正式制作方式。
   * `Dockerfile` 是更规范的镜像制作方式，可以把镜像构建过程写成文本文件，方便重复构建、版本管理和排查问题。

2. Dockerfile 的作用

   ```text
   Dockerfile
       ↓
   docker build
       ↓
   image 镜像
       ↓
   docker run
       ↓
   container 容器
   ```

3. 优点

   * 构建过程清晰
   * 可以重复构建
   * 方便提交到 Git
   * 方便团队协作
   * 比 `docker commit` 更可控

#### 使用`docker init`创建项目

1. 说明
    - 这是Docker Desktop 4.17+（对应 CLI 版本 23.0+）中引入的新功能，旧版本可能没有

2. 安装
    ```bash
    # 下载 deb 包
    wget https://desktop.docker.com/linux/main/amd64/docker-desktop-amd64.deb
    sudo apt install ./docker-desktop-amd64.deb
    # 启动 Docker Desktop
    systemctl --user start docker-desktop
    ```

#### 手动创建项目

1. 创建Dockerfile
    ```bash
    mkdir myproj01
    cd myproj01
    touch dockerfile
    ```
    内容：
    ```dockerfile
    FROM nginx
    RUN echo '<h1>proj01</h1>' > /usr/share/nginx/html/index.html
    ```

#### `FROM` 指定基础镜像
1. 作用

   ```dockerfile
   FROM nginx:1.28-alpine
   ```

   * `FROM` 用来指定基础镜像。
   * 一个 Dockerfile 必须有 `FROM`。
   * 通常 `FROM` 是 Dockerfile 的第一条有效指令。

2. 理解

   ```text
   基础镜像
       ↓
   在基础镜像上修改
       ↓
   得到自己的新镜像
   ```

3. 常见基础镜像

    * 服务类镜像：

        ```text
        nginx
        redis
        mysql
        mongo
        tomcat
        httpd
        ```

    * 语言环境镜像：

        ```text
        node
        python
        golang
        eclipse-temurin
        ruby
        php
        ```

    * 操作系统镜像：

        ```text
        ubuntu
        debian
        alpine
        centos
        fedora
        ```

4. `scratch`

   ```dockerfile
   FROM scratch
   ```

   * `scratch` 表示空白镜像。
   * 它不是一个普通系统镜像。
   * 常用于 Go 这类可以静态编译的程序。
   * 好处是镜像体积极小。
   * 缺点是没有 Shell、包管理器和常见系统工具。

#### `RUN` 执行命令

1. 作用

    ```dockerfile
    RUN echo '<h1>proj01</h1>' > /usr/share/nginx/html/index.html
    ```

    * `RUN` 用来在镜像构建阶段执行命令。
    * 命令执行后的文件系统变化会保存到镜像层中。

2. 两种写法

    * shell 格式：

        ```dockerfile
        RUN echo '<h1>proj01</h1>' > /usr/share/nginx/html/index.html
        ```

    * exec 格式：

        ```dockerfile
        # RUN ["可执行文件", "参数1", "参数2"]
        RUN ["echo", "hello"]
        ```

3. 重点理解

    ```text
    RUN 不是容器启动后执行
    RUN 是 docker build 构建镜像时执行
    ```

4. 镜像层

    * 一般情况下，每个会修改文件系统的 `RUN` 指令都会产生新的镜像层。
    * 所以不要写太多零散的 `RUN`。

    不推荐：

    ```dockerfile
    RUN apt update
    RUN apt install -y curl
    RUN apt install -y vim
    ```

    推荐：

    ```dockerfile
    RUN apt update && apt install -y curl vim
    ```


#### 构建镜像

1. 命令

   ```bash
   docker build -t nginx:v3 .
   ```

2. 参数说明

   * `docker build`

     * 构建镜像。
   * `-t nginx:v3`

     * 给镜像打标签。
     * `nginx` 是镜像名。
     * `v3` 是标签名。
   * `.`

     * 指定构建上下文目录。
     * 不是单纯表示 Dockerfile 所在目录。

3. 查看镜像

   ```bash
   docker images
   ```

4. 运行镜像

   ```bash
   docker run --rm -d -p 8080:80 --name mynginx nginx:v3
   ```

5. 访问测试

   ```bash
   curl http://127.0.0.1:8080
   ```

6. 说明

   * 宿主机访问 `8080` 端口。
   * Docker 把请求转发到容器内部的 `80` 端口。
   * Nginx 返回 `/usr/share/nginx/html/index.html` 页面。

#### 镜像构建上下文

1. 命令示例

   ```bash
   docker build -t nginx:v3 .
   ```

2. 重点

   ```text
   最后的 . 表示构建上下文目录
   不是单纯表示 Dockerfile 所在目录
   ```

3. 什么是构建上下文

   * 构建上下文就是 Docker 构建镜像时可以访问的文件范围。
   * `COPY`、`ADD` 只能复制构建上下文里面的文件。
   * Docker 官方文档也说明，`.dockerignore` 位于构建上下文根目录，用来排除不需要发送给构建器的文件。

4. 示例

   当前目录：

   ```text
   myproj01/
   ├── Dockerfile
   └── index.html
   ```

   Dockerfile：

   ```dockerfile
   FROM nginx:1.28-alpine
   COPY index.html /usr/share/nginx/html/index.html
   ```

   构建：

   ```bash
   docker build -t mynginx:v1 .
   ```

   这里的：

   ```dockerfile
   COPY index.html /usr/share/nginx/html/index.html
   ```

   复制的是：

   ```text
   构建上下文目录中的 index.html
   ```

5. 错误理解

   错误理解：

   ```text
   docker build 后面的 . 是 Dockerfile 所在目录
   ```

   正确理解：

   ```text
   docker build 后面的 . 是构建上下文目录
   Dockerfile 只是默认从这个目录中查找
   ```

6. 指定其他 Dockerfile

   如果 Dockerfile 不叫默认名字，或者不在当前目录，可以使用 `-f`：

   ```bash
   docker build -f ./docker/Dockerfile -t mynginx:v1 .
   ```

   说明：

   * `-f ./docker/Dockerfile`

     * 指定 Dockerfile 文件位置。
   * 最后的 `.`

     * 仍然表示构建上下文。

#### `.dockerignore`

1. 作用

   * 类似 `.gitignore`。
   * 用于排除不需要进入构建上下文的文件。
   * 可以减少构建体积。
   * 可以避免敏感文件被复制进镜像。

2. 示例

   ```text
   .git
   node_modules
   dist
   *.log
   .env
   README.md
   ```

3. 注意

   * 被 `.dockerignore` 排除的文件，`COPY` 时也找不到。
   * 如果 Dockerfile 里写了：

   ```dockerfile
   COPY .env /app/.env
   ```

   但 `.dockerignore` 里排除了：

   ```text
   .env
   ```

   构建时就会失败。

### 导入和导出镜像

### 其它制作镜像的方式

#### `docker save`

1. 作用

   * 把本地已有镜像保存成一个 tar 归档文件。
   * 常用于离线迁移镜像。

2. 命令格式

   ```bash
   docker save [镜像名] -o [文件名]
   ```

3. 示例

   ```bash
   docker save alpine -o alpine.tar
   ```

4. 查看文件类型

   ```bash
   file alpine.tar
   ```

   结果类似：

   ```text
   alpine.tar: POSIX tar archive
   ```

5. 说明

   * `alpine`

     * 要保存的镜像。
   * `-o alpine.tar`

     * 保存到 `alpine.tar` 文件。
   * 文件名可以随便写。
   * 本质是 tar 归档文件。
   * 如果文件同名，会直接覆盖。

#### 使用 gzip 压缩保存

1. 命令

   ```bash
   docker save alpine | gzip > alpine-latest.tar.gz
   ```

2. 说明

   * `docker save alpine`

     * 输出 alpine 镜像归档内容。
   * `|`

     * 管道，把前一个命令的输出传给后一个命令。
   * `gzip`

     * 压缩。
   * `> alpine-latest.tar.gz`

     * 保存成压缩包文件。

3. 理解

   ```text
   alpine 镜像
       ↓
   docker save
       ↓
   tar 归档数据
       ↓
   gzip 压缩
       ↓
   alpine-latest.tar.gz
   ```

---

#### `docker load`

1. 作用

   * 从 `docker save` 生成的归档文件中加载镜像。
   * 常用于在另一台机器上恢复镜像。

2. 命令

   ```bash
   docker load -i alpine-latest.tar.gz
   ```

3. 输出示例

   ```text
   Loaded image: alpine:latest
   ```

4. 查看加载结果

   ```bash
   docker image ls alpine
   ```

5. 理解

   ```text
   alpine-latest.tar.gz
       ↓
   docker load
       ↓
   本地 Docker 镜像
   ```

---

### docker save / load 的典型使用场景

#### 场景一：没有网络

1. A 机器有镜像：

   ```bash
   docker save nginx:1.28-alpine -o nginx.tar
   ```

2. 复制到 B 机器：

   ```bash
   scp nginx.tar user@server:/tmp/
   ```

3. B 机器加载：

   ```bash
   docker load -i /tmp/nginx.tar
   ```

4. B 机器运行：

   ```bash
   docker run -d -p 8080:80 nginx:1.28-alpine
   ```


#### 场景二：离线部署

```text
开发机
    ↓ docker save
镜像 tar 包
    ↓ U盘 / scp / 内网传输
服务器
    ↓ docker load
运行容器
```

---

#### 场景三：服务器之间直接传输镜像

1. 普通写法

   ```bash
   docker save nginx:1.28-alpine | ssh user@server 'docker load'
   ```

2. 带压缩写法

   ```bash
   docker save nginx:1.28-alpine | gzip | ssh user@server 'gunzip | docker load'
   ```

3. 带进度条写法

   ```bash
   docker save nginx:1.28-alpine | bzip2 | pv | ssh user@server 'bunzip2 | docker load'
   ```

4. 说明

   * `docker save`

     * 导出本机镜像。
   * `bzip2`

     * 压缩。
   * `pv`

     * 显示传输进度。
   * `ssh`

     * 传输到远程服务器。
   * `docker load`

     * 远程服务器加载镜像。

## 容器操作

### 启动容器

1. 启动方式
    - 新建并启动：基于镜像创建新容器
    - 重新启动

#### 新建并启动

1. 基本语法
    ```bash
    docker run [选项] 镜像 [命令] [参数...]
    ```

    - `-i`：保持标准输入 (stdin) 打开，允许输入
    - `-t`：分配伪终端 (pseudo-TTY)，提供终端界面
    - `-it`：两者组合使用，获得交互式终端

2. 获得交互式容器

    ```bash
    $ docker run -it ubuntu:24.04 /bin/bash
    root@af8bae53bdd3:/#
    ```

#### 常用启动选项

1. 基础选项
    - `-d`：后台运行 (detach)
        ```bash
        docker run -d nginx:latest
        ```
    - `-it`：交互式终端
        ```bash
        docker run -it ubuntu:24.04 bash
        ```
    - `--name`：指定容器名称
        ```bash
        docker run --name myapp nginx:latest
        ```
    - `--rm`：退出后自动删除容器
        ```bash
        docker run --rm ubuntu:24.04 echo hi
        ```


1. 端口映射
    - 将容器的 80 端口映射到宿主机的 8080 端口
        ```bash
        $ docker run -d -p 8080:80 nginx:latest
        ```

    - 随机映射端口
        ```bash
        $ docker run -d -P nginx:latest
        ```

    - 只绑定到 localhost
        ```bash
        $ docker run -d -p 127.0.0.1:8080:80 nginx:latest
        ```

1. 数据卷挂载
    - 挂载命名卷
        ```bash
        $ docker run -v mydata:/data nginx:latest
        ```

    - 挂载宿主机目录
        ```bash
        $ docker run -v /host/path:/container/path nginx:latest
        ```

    - 只读挂载
        ```bash
        $ docker run -v /host/path:/container/path:ro nginx:latest
        ```
1. 环境变量
    - 设置单个环境变量
        ```bash
        $ docker run -e MYSQL_ROOT_PASSWORD=secret mysql
        ```

    - 从文件加载环境变量
        ```bash
        $ docker run --env-file .env myapp
        ```
1. 资源限制
    - 限制内存
        ```bash
        $ docker run -m 512m nginx:latest
        ```

    - 限制 CPU
        ```bash
        $ docker run --cpus=1.5 nginx:latest
        ```

#### 启动已终止容器

1. 使用 `docker start` 重新启动已停止的容器

    ```bash
    ## 查看所有容器（包括已停止的）

    $ docker ps -a
    CONTAINER ID  IMAGE   STATUS                     NAMES
    af8bae53bdd3  ubuntu  Exited (0) 2 minutes ago   myubuntu

    ## 重新启动

    $ docker start myubuntu

    ## 启动并附加终端

    $ docker start -ai myubuntu
    ```