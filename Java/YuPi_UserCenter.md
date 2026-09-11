# 【程序员鱼皮】 用户中心项目

## 企业项目流程

### 流程简述

1. 需求分析
    - 由老板、产品经理、开发者提出
    - 合理性分析（如随手机壳颜色变换主题颜色）

2. 设计
    - 不写代码
    - 概要设计
    - 详细设计

3. 技术选型

4. 搭建项目/初始化
    - 引入项目
    - 自己熟悉的技术栈
    - 不熟悉也没有关系，根据文档写一个demo，自然就入门了。

5. 写demo
    - 引入组件库后，写一个demo、测试用例测试是否好用

6. 测试
    - 单元测试（如增删改查）

7. 代码提交/评审
    - 由同事、领导审核

### 需求分析

1. 登录/注册

2. 用户管理（仅管理员）

3. 用户校验（会员）
    - 对于一个简单的项目，不需要做太麻烦的校验机制。
    - 当前项目暂时只有普通会员和管理员两个角色，对于校验机制没有太大的需求

### 技术选型

1. 前端
    - 三件套
    - React（我更熟悉VUE）
    - 组件库（Ant Design）
    - 开发框架（Umi）
    - Ant Design Pro（现成的管理系统）


2. 后端
    - java
    - spring 
    - SpringMVC
    - MyBatis
    - MyBatis-Plus
    - SpringBoot
    - MySQL

3. 部署
    - 服务器/容器
    - 暂定WSL

### 计划

1. 初始化项目-前端
    - 初始化项目
    - 引入组件
    - 框架/瘦身

2. 初始化项目-后端
    - 准备环境（MySQL等）
    - 引入框架    

3. 登录/注册

4. 用户管理

## 前端准备

### Nodejs

#### 基本使用

1. 下载链接
    - https://nodejs.org/zh-cn/download

2. 注意事项
    - 建议下载长期维护版（LTS版）
    - 【占位】不同版本的区别，Docker？npm？Nvm？等

#### 简介

1. **Node.js 不是一门编程语言，它是 JavaScript 语言的“运行环境”（Runtime）。**

    在 Node.js 诞生之前，JavaScript 是一门“残疾”的语言。它被死死地锁在**浏览器**里，只能用来做网页特效、弹窗、或者向服务器发请求。它没有权限读取你电脑里的文件，也不能操作数据库。

    后来，有个天才把谷歌 Chrome 浏览器里解析 JavaScript 的核心引擎（V8引擎）拆了下来，套上了一个能在操作系统底层运行的外壳。这个结合体，就是 Node.js。

    它的出现，让 JavaScript **打破了浏览器的结界，获得了和 Java、Python、C++ 一样掌控计算机底层的能力。**

2. 有了 Node.js，你可以干什么？

    既然 JavaScript 可以操作底层系统了，Node.js 就立刻变成了全能选手：

    1. 开发真正的高性能后端服务 (Web Server)
    你可以用 Node.js 监听服务器的 80 端口，连接 MySQL 或 Redis 数据库。当用户在浏览器点击“登录”时，Node.js 可以在后台校验密码，生成 Token 并返回。这和写 Spring MVC 里的 Controller 一模一样。由于它天生具备“异步非阻塞”的特性，非常适合处理高并发的网络请求。

    2. 搭建个人全栈项目与微服务
    如果你想自己在一台云服务器上部署一个技术博客，或者给私有服务器（比如游戏私服）写一套轻量级的管理后台和 API 接口，Node.js 可以用极低的内存开销跑在服务器上，提供稳定服务。

    3. 编写强大的命令行工具和自动化脚本
    你可以用 Node.js 写脚本来批量重命名你电脑里的文件，或者每天定时爬取特定网站的数据存入本地。
    这也是为什么**前端开发离不开 Node.js**——前端那些极其复杂的打包、压缩工具（比如 Vite、Webpack），本质上全都是用 Node.js 写的自动化脚本程序。


#### 速成指南

在 Node.js 的世界里，这三个工具分工极其明确，它们分别是：**版本切换器 (nvm)**、**包与脚本管理器 (npm)**、**临时执行器 (npx)**。

1. `nvm` (Node Version Manager) —— 环境切换器

    - 定义
        它就是一个**动态修改系统环境变量**的脚本。它的唯一作用，就是让你电脑上能同时存在多个版本的 Node.js（比如 v16, v18, v20），并在你需要时，瞬间把系统的 `node` 命令指向其中一个。

    - **【核心命令】**

        * `nvm install 20`：去服务器下载 Node.js 的 20.x 版本存到本地某个隐藏深处的文件夹。
        * `nvm ls`：列出你电脑上当前下载了哪些版本。
        * `nvm use 20`：**（核心操作）** 修改环境变量，让你当前的终端敲 `node -v` 时，输出的是 20。

2. `npm` (Node Package Manager) —— 包与脚本管理器

    - 定义
        它由两部分组成：一个**依赖下载器**（对着清单下载文件）和一个**Shell脚本触发器**（帮你执行预设的命令）。

    - **【核心命令体系】**

        **A. 依赖管理 (`npm install` 或 `npm i`)**

        * `npm install`：**无参执行**。读取 `package.json`，把里面记录的所有东西原封不动下载到 `node_modules` 文件夹里。
        * `npm install <包名>`：**带参执行**。下载指定的包，并把它的名字写入 `package.json` 的生产依赖清单里（类似于 Maven 默认的 compile scope）。
        * `npm install <包名> -D`：下载指定的包，但写入**开发依赖清单**里（类似于 Maven 的 test/provided scope，打包时不会带上它）。
        * `npm install <包名> -g`：**全局安装**。把包下载到操作系统的公共目录，而不是当前项目。**（极度不推荐！除非是一个你每天都要用的系统级脚手架，否则不要污染全局环境。）**

        **B. 脚本触发 (`npm run`)**

        * **底层逻辑**：`npm run <key>` 的唯一动作，就是去 `package.json` 的 `"scripts"` 字典里，找到对应的 `<key>`，然后把它的 `<value>` 丢给操作系统的命令行去执行。并且它会**优先使用**当前项目 `node_modules/.bin` 里的工具。
        * **特例（VIP 命令）**：
        NPM 底层硬编码了几个白名单。只要你敲这几个词，NPM 会自动帮你补全 `run`：
        1. `npm start` === `npm run start` （启动）
        2. `npm test` === `npm run test` （测试）
        3. `npm stop` === `npm run stop` （停止）
        4. `npm restart` === `npm run restart` （重启，这个命令还会按顺序自动触发 prerestart 和 postrestart 脚本）
        *除此以外的任何自定义名字（如 dev, build, deploy），都必须老老实实加上 `run`。*

3. `npx` (Node Package Execute) —— 临时执行器

    - 定义
        一个**阅后即焚**的命令执行工具。它的核心哲学是：“我不想在电脑上安装这个工具，我只想用它这一次”。

    - **【诞生背景】**
        以前我们要用一个叫 `create-react-app` 的工具来生成项目，必须先用 `npm install -g create-react-app` 把它永久安装在电脑上，然后再运行它。这不仅占硬盘，而且工具版本更新了你还在用老版本。

    - **【底层逻辑】**
        当你执行 `npx <命令>` 时，它会做三件事：

        1. **找本地**：先看看当前项目的 `node_modules/.bin` 里有没有这个命令，有就直接跑。
        2. **云端拉取**：如果没有，它直接去云端（NPM 仓库）把这个工具的最新版下载到一个系统临时缓存目录。
        3. **运行并抛弃**：执行这个工具，执行完毕后，任由系统清理临时目录。

    - **【核心使用场景】**

        * **执行一次性的脚手架**：`npx create-vue@latest`（不用安装 create-vue，直接从云端拉取最新版运行，生成完项目就完事）。
        * **执行特定版本的工具**：`npx http-server@0.9.0`（临时下载一个 0.9.0 版本的静态服务器并启动，关掉终端后就当无事发生）。
        - **`npm create <框架名>` 绝对等同于 `npx create-<框架名>`**

#### NVM安装指南（Win）

1. 如果你觉得麻烦，可以直接向AI工具许愿，此处提示词待添加...
    ```text

    ```

1. 通过安装包安装nvm
    1. 通过`设置-应用-安装的应用`，查找并卸载已经安装的Nodejs

    2. 通过官方仓库下载安装包（`*-setup.exe`）
        - https://github.com/nvm-windows/nvm/releases

    3. 建议不要安装在C盘，下一小节中将讲解NVM的目录结构与工作原理
        - 安装路径：`E:\DevTools\nvm`
        - Nodejs安装路径：`E:\DevTools\nodejs`

    4. 桌面通知建议只勾选：`Node.js LTS releases`

3. 初始化
    - 检查配置
        如果按照前面的安装步骤，配置应如下所示
        ```powershell
        (base) PS C:\Users\Lenovo> where.exe nvm        
        E:\DevTools\nvm\nvm.exe
        (base) PS C:\Users\Lenovo> $env:NVM_HOME        
        E:\DevTools\nvm
        (base) PS C:\Users\Lenovo> $env:NVM_SYMLINK
        E:\DevTools\nodejs
        (base) PS C:\Users\Lenovo> nvm version
        1.2.2
        (base) PS C:\Users\Lenovo> nvm root

        Current Root: E:\DevTools\nvm
        (base) PS C:\Users\Lenovo>
        ```
    - 创建目录
        ```powershell
        New-Item -ItemType Directory -Force -Path `
            'E:\DevTools\npm-cache', `
            'E:\DevTools\npm-config', `
            'E:\DevTools\temp' |
            Out-Null
        ```
    - 配置下载镜像
        ```powershell
        nvm node_mirror https://npmmirror.com/mirrors/node/
        nvm npm_mirror https://npmmirror.com/mirrors/npm/
        nvm proxy none
        ```
        然后查看配置
        ```powershell
        (base) PS C:\Users\Lenovo> Get-Content "$env:NVM_HOME\settings.txt"
        root: E:\DevTools\nvm
        arch: 64
        proxy: none
        originalpath: .
        originalversion:
        node_mirror: https://npmmirror.com/mirrors/node/
        npm_mirror: https://npmmirror.com/mirrors/npm/
        ```
    - 安装LTS版本Nodejs
        在Windows直接安装lts失败了，原因不明，建议指定一个版本号
        ```powershell
        # 安装最新 LTS，推荐
        nvm install lts

        # 安装最新 Current，不一定适合生产环境
        nvm install latest

        # 使用本机已安装版本中最新的版本
        nvm use newest
        ```

    - 检查符号链接
        切换到某版本nodejs后，符号链接应指向对应的目录
        ```powershell
        Get-Item -LiteralPath $env:NVM_SYMLINK |
            Format-List FullName,LinkType,Target
        ```

    - 设置国内下载源与缓存目录（多版本共享）
        ```powershell
        npm config set registry `
            "https://registry.npmmirror.com" `
            --location=user

        npm config set cache `
            "E:\DevTools\npm-cache" `
            --location=user
        ```
        验证
        ```powershell
        npm config get registry
        npm config get cache
        npm config get userconfig
        ```
    - 自检代码（可直接复制）

        ```powershell
        Write-Output "NVM_HOME=$env:NVM_HOME"
        Write-Output "NVM_SYMLINK=$env:NVM_SYMLINK"

        nvm version
        nvm root
        nvm current

        where.exe nvm
        where.exe node
        where.exe npm

        node -v
        npm -v

        npm config get registry
        npm config get cache
        npm config get userconfig
        npm config get prefix
        npm root -g
        ```

#### NVM 工作原理

#### 项目部署指南


    
### AntDesignPro

#### 基本介绍

1. 简介
    - **Ant Design Pro** 是蚂蚁集团（Ant Group）推出的一款**开箱即用的企业级中后台前端/UI解决方案**。

    - 如果你需要快速开发一个带有侧边栏、顶部导航、复杂表格、数据图表以及权限管理的后台管理系统，Ant Design Pro 提供了一整套基于 React 的脚手架和模板。你不需要从零开始搭建项目基础架构，直接在这个模板上写业务逻辑即可。

    - https://03x.pro.ant.design/

2. 核心特性与优势
    * **ProComponents（高级组件）：** 针对中后台最常见的增删改查（CRUD）场景进行了重度封装。例如 `ProTable` 组件，你只需要配置好数据列（columns）和网络请求，它就能自动生成包含搜索表单、分页、刷新、密度调整的完整数据表格，能省去大量重复代码。
    * **权限路由管理：** 内置了完善的权限控制（RBAC）体系。你可以轻松根据用户的角色动态渲染菜单栏，或者控制某个按钮的显示与隐藏。
    * **全局状态与数据流：** 基于 Umi 的简易数据流（通常是 `@umijs/max` 内置的 `useModel`），状态管理非常轻量，不需要像 Redux 那样写繁琐的样板代码。
    * **多语言与国际化（i18n）：** 内置了国际化方案，如果你的系统需要出海或者提供给不同语言的用户，可以直接使用配置好的多语言插件。
    * **Mock 数据驱动：** 在后端接口还没开发好时，前端可以通过内置的 Mock 方案自己模拟数据，实现前后端分离并行开发。

3. 最新技术栈 (v6版本)

    Ant Design Pro 在 2026 年已经进化到了 **v6 版本**，其底层技术栈迎来了全面现代化升级：

    | 模块 | 使用技术 | 说明 |
    | --- | --- | --- |
    | **基础框架** | React 18/19 | 全面拥抱最新的 React 并发渲染特性。 |
    | **UI 组件库** | Ant Design v6 | 启用 CSS 变量模式，大幅提升渲染性能和动态主题切换能力。 |
    | **应用框架** | UmiJS (`@umijs/max`) | 阿里开源的 React 企业级前端应用框架，负责路由、插件机制和构建。 |
    | **样式方案** | Tailwind CSS v4 + `antd-style` | 抛弃了以前的 Less，转向更现代的原子级 CSS 和 CSS-in-JS。 |
    | **构建引擎** | Utoopack (Rust) | 默认使用基于 Turbopack 的 Rust 构建引擎，冷启动和热更新速度极快。 |
    | **AI 赋能** | Ant Design X | v6 内置了 AI 助手页面，方便快速接入大模型对话等智能化场景。 |

4. 适用场景与局限性

    **最适合的场景：**

    * **企业内部系统（ERP、CRM、CMS 等）：** 这类系统对界面交互的规范性要求高，对定制化个性化 UI 要求较低。
    * **快速 MVP 验证：** 需要在极短时间内搭建出一个看起来专业且功能完整的后台页面。
    * **全栈/后端开发者写前端：** 即便前端基础不深，依靠复制粘贴 ProComponents 的示例代码也能堆砌出标准的管理系统。

    **不适合的场景：**

    * **高度定制化的 C 端前台产品：** 如果你的项目需要极其炫酷的动画、非主流的布局设计，使用它的改造成本比从头写还要高。
    * **极简的小型项目：** Ant Design Pro 是一个“重型”武器，内置了太多依赖和抽象封装，对于只有两三个页面的小工具来说，会显得过于臃肿，且有一定学习曲线（尤其是需要先理解 Umi 框架的约定式路由和数据流）。

#### 开始使用

1. 安装
    - https://preview.pro.ant.design/welcome/

    - 创建项目：
        ```bash
        git clone --depth 1 https://github.com/ant-design/ant-design-pro.git my-project
        # 第一个不行时换第二个
        # git clone --depth 1 git@github.com:ant-design/ant-design-pro.git my-project
        cd my-project
        npm install
        ```
    - 项目提供两种模式：

        - 完整模式：包含所有示例页面（Dashboard、表单、列表、权限等），适合参考学习
        - 精简模式：仅保留登录页和基础布局，适合从零开发

    - 切换精简模式：
        ```bash
        git add -A && git commit -m "chore: save before simple"  # 先提交，以便回退
        npm run simple                                           # 删除示例页面和多余依赖
        npm install                                              # 更新依赖
        ```

2. 

## 后端准备

### 资源

1. github通过搜索`springboot-template`



### 项目初始化

1. 通过`https://start.spring.io`完成项目初始化

2. 选择的扩展
    - MyBatis-plus：https://baomidou.com/getting-started/
    - https://github.com/alibaba/druid

    ```xml

    <dependencies>
    <!-- 以下通过阿里源IDEA初始化，版本2.6.10 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <!-- 以下是官方文档添加 -->

        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.17</version>
        </dependency>

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.2.24</version>
        </dependency>

        <!-- 其他 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>

    </dependencies>
    ```

### 数据库设计

1. 创建表
    ```sql
    DROP TABLE IF EXISTS user;

    CREATE TABLE user (
        id BIGINT NOT NULL COMMENT '主键ID',
        username VARCHAR(50) NULL DEFAULT NULL COMMENT '昵称',
        userAccount VARCHAR(50) NOT NULL COMMENT '登录账号',
        avatarUrl VARCHAR(255) NULL DEFAULT NULL COMMENT '头像',
        gender TINYINT NULL DEFAULT NULL COMMENT '性别',
        userPassword VARCHAR(100) NOT NULL COMMENT '密码',
        phone VARCHAR(20) NULL DEFAULT NULL COMMENT '电话',
        email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
        userStatus INT DEFAULT 0 COMMENT '用户状态 0-正常',
        userRole INT DEFAULT 0 COMMENT '用户角色 0-普通用户 1-管理员',
        createTime DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
        updateTime DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
        isDelete TINYINT DEFAULT 0 COMMENT '是否删除 0-未删除 1-已删除',
        PRIMARY KEY (id),
        UNIQUE KEY uk_user_account (userAccount)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='用户表';
    ```
    - `userStatus`：表示用户状态，用户可能是非正常状态，如封号、注销冷静期等
    - `isDelete`：表示用户是否删除，用户删除是一种危险操作，需要先打上标记，再决定是否永久删除。

    ```sql
    DROP TABLE IF EXISTS user_account;

    CREATE TABLE user_account (
        -- 主键
        id BIGINT UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '内部用户ID',

        -- 登录认证
        user_account VARCHAR(64) NOT NULL COMMENT '登录账号',
        password_hash VARCHAR(255) NOT NULL COMMENT '密码哈希',

        -- 用户资料
        nickname VARCHAR(64) DEFAULT NULL COMMENT '昵称',
        avatar_url VARCHAR(1024) DEFAULT NULL COMMENT '头像地址',
        gender TINYINT UNSIGNED DEFAULT NULL
            COMMENT '性别：0未知，1男，2女',
        phone VARCHAR(20) DEFAULT NULL COMMENT '手机号',
        email VARCHAR(254) DEFAULT NULL COMMENT '邮箱',

        -- 状态
        status TINYINT UNSIGNED NOT NULL DEFAULT 0
            COMMENT '状态：0正常，1禁用，2注销',

        -- 审计字段
        last_login_at DATETIME DEFAULT NULL COMMENT '最后登录时间',
        created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
            COMMENT '创建时间',
        updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
            ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
        deleted_at DATETIME DEFAULT NULL COMMENT '删除时间',

        -- 主键与索引
        PRIMARY KEY (id),
        UNIQUE KEY uk_user_account (user_account),
        UNIQUE KEY uk_phone (phone),
        UNIQUE KEY uk_email (email),
        KEY idx_status_created_at (status, created_at),

        -- MySQL 8 约束
        CONSTRAINT chk_gender
            CHECK (gender IS NULL OR gender IN (0, 1, 2)),
        CONSTRAINT chk_status
            CHECK (status IN (0, 1, 2))
    ) ENGINE = InnoDB
    DEFAULT CHARSET = utf8mb4
    COLLATE = utf8mb4_unicode_ci
    COMMENT = '用户账户表';
    ```

2. 实现数据库基本操作（使用MyBatisX）

    - 在相应的表上右键，选择MyBatisX的选项

3. 快速测试
    - 安装插件`GeneratorAllSetter`
    - 在测试对象上`Alt+Enter`选择对应选项

### 项目结构

1. 项目结构
    ```text
    usrcenter
    ├── .idea
    ├── .mvn
    │   └── wrapper
    │       └── maven-wrapper.properties
    ├── src
    │   ├── main
    │   │   ├── java
    │   │   │   └── com
    │   │   │       └── lcq
    │   │   │           └── usrcenter
    │   │   │               ├── config
    │   │   │               ├── controller
    │   │   │               ├── mapper
    │   │   │               ├── model
    │   │   │               ├── service
    │   │   │               ├── utils
    │   │   │               └── UsrcenterApplication.java
    │   │   └── resources
    │   │       └── application.yaml
    │   └── test
    │       └── java
    │           └── com
    │               └── lcq
    │                   └── usrcenter
    │                       ├── SampleTest.java
    │                       └── UsrcenterApplicationTests.java
    ├── target
    ├── .gitattributes
    ├── .gitignore
    ├── HELP.md
    ├── mvnw
    ├── mvnw.cmd
    └── pom.xml
    ```

## 后端知识补充

### Spring Security

#### 密码加密

1. 简介

    用户密码不能以明文形式保存到数据库中，否则数据库泄露后，攻击者可以直接获得所有用户的密码。

    实际项目中通常使用单向哈希算法处理密码。虽然开发中经常称为“密码加密”，但更准确的说法是“密码哈希”：

    ```text
    原始密码 → BCrypt 哈希 → 保存到数据库
    ```

    单向哈希具有以下特点：

    - 不能从哈希值还原出原始密码
    - 注册时保存密码哈希
    - 登录时验证输入密码是否与哈希匹配
    - 不需要、也不应该提供“解密密码”功能

    Spring Security 提供了统一的密码处理接口：

    ```java
    public interface PasswordEncoder {

        String encode(CharSequence rawPassword);

        boolean matches(
                CharSequence rawPassword,
                String encodedPassword
        );
    }
    ```

    常用实现是：

    ```java
    BCryptPasswordEncoder
    ```

    推荐把它配置成 Spring Bean：

    ```java
    @Configuration
    public class SecurityConfig {

        @Bean
        public PasswordEncoder passwordEncoder() {
            return new BCryptPasswordEncoder();
        }
    }
    ```

    然后在 Service 中注入：

    ```java
    @Resource
    private PasswordEncoder passwordEncoder;
    ```

    注册时加密：

    ```java
    String passwordHash =
            passwordEncoder.encode(password);

    user.setPasswordHash(passwordHash);
    ```

    登录时验证：

    ```java
    boolean matched = passwordEncoder.matches(
            inputPassword,
            user.getPasswordHash()
    );

    if (!matched) {
        throw new BusinessException(
                ErrorCode.ACCOUNT_OR_PASSWORD_ERROR
        );
    }
    ```

    不能这样比较：

    ```java
    passwordEncoder.encode(inputPassword)
            .equals(user.getPasswordHash());
    ```

    因为 BCrypt 每次加密都会生成新的随机盐，即使输入相同的密码，得到的哈希通常也不同。

2. `BCrypt` 编码原理

    BCrypt 是一种专门为密码存储设计的单向哈希算法，内部基于 Blowfish 算法，并加入了：

    - 随机盐值
    - 成本因子
    - 多轮计算

    基本过程如下：

    ```text
    原始密码
    ↓
    生成随机盐值
    ↓
    根据成本因子进行多轮计算
    ↓
    生成 BCrypt 哈希
    ↓
    保存完整哈希到数据库
    ```

    例如：

    ```java
    PasswordEncoder encoder =
            new BCryptPasswordEncoder();

    String result = encoder.encode("Passw0rd!");

    System.out.println(result);
    ```

    可能生成：

    ```text
    $2a$10$mBPk4YzHrQqYxHsC4vD7QO8...
    ```

    BCrypt 哈希一般由以下部分组成：

    ```text
    $2a$10$盐值和哈希结果
    │   │
    │   └── 成本因子
    └────── BCrypt 版本
    ```

    例如：

    ```text
    $2a$10$...
    ```

    含义是：

    - `$2a$`：BCrypt 算法版本标识
    - `10`：成本因子
    - 后面的内容：随机盐值和最终哈希结果

    完整 BCrypt 字符串通常是 60 个字符，因此数据库字段可以设计为：

    ```sql
    password_hash varchar(100) not null
    ```

3. 随机盐值

    盐值是一段随机数据，会参与密码哈希计算。BCrypt 会自动生成盐值，并将盐值包含在最终哈希字符串中，不需要单独建立数据库字段保存。

    相同密码多次编码：

    ```java
    String first = encoder.encode("12345678");
    String second = encoder.encode("12345678");
    ```

    得到的结果通常不同：

    ```text
    $2a$10$abc...
    $2a$10$xyz...
    ```

    但是下面两个验证都会成功：

    ```java
    encoder.matches("12345678", first);  // true
    encoder.matches("12345678", second); // true
    ```

    随机盐值可以防止攻击者直接使用预先计算好的彩虹表批量破解密码。

4. 成本因子

    成本因子决定 BCrypt 的计算强度：

    ```java
    new BCryptPasswordEncoder(10);
    ```

    成本因子为 `10` 时，核心计算量大约与：

    ```text
    2¹⁰
    ```

    相关。

    成本因子越大：

    - 生成密码哈希越慢
    - 登录验证越慢
    - 暴力破解成本越高
    - 服务器 CPU 消耗也越高

    例如：

    ```java
    new BCryptPasswordEncoder(10);
    new BCryptPasswordEncoder(12);
    ```

    成本因子每增加 `1`，计算量大致翻倍。不能为了安全无限增大，需要根据服务器性能和并发量选择。

    Spring Security 中常见的默认成本因子是 `10`：

    ```java
    new BCryptPasswordEncoder();
    ```

5.  密码验证过程

    登录时，BCrypt 不会解密数据库中的密码，而是：

    1. 从数据库哈希中读取版本、成本因子和盐值。
    2. 使用相同参数处理用户输入的密码。
    3. 比较计算结果是否一致。

    示意：

    ```text
    用户输入的密码
        ↓
    读取数据库哈希中的盐值和成本因子
        ↓
    重新执行 BCrypt 计算
        ↓
    比较结果
        ↓
    相同：密码正确
    不同：密码错误
    ```

    Spring Security 已经封装了这个过程：

    ```java
    boolean matched = passwordEncoder.matches(
            rawPassword,
            passwordHash
    );
    ```

7. 使用注意事项

    - 不能保存明文密码。
    - 不能把密码或密码哈希返回给前端。
    - 不能在日志中输出原始密码。
    - 应使用 `matches()` 验证密码。
    - BCrypt 盐值已经包含在哈希中，无需单独保存。
    - 数据库字段可以使用 `varchar(100)`。
    - 登录失败时建议统一提示“账号或密码错误”。
    - 用户忘记密码时只能重置，不能找回原密码。
    - 注册接口应限制 BCrypt 原始密码不超过 72 字节。

### MyBatis相关

#### 删除标记

1. 用于在查询时排除对应结果，如搜索账号时，不显示已删除（注销）的记录

2. 两种形式
    - 注解

    - 配置

3. 原理
    通过修改SQL实现忽略
    - 如何查询已删除的数据，方法较多，暂时不列举，你自己问AI去
    ```sql
    -- 查询
    SELECT ... FROM user_account
    WHERE deleted_at IS NULL;

    -- 更新
    UPDATE user_account
    SET ...
    WHERE id = ? AND deleted_at IS NULL;

    -- 删除
    UPDATE user_account
    SET deleted_at = NOW()
    WHERE id = ? AND deleted_at IS NULL;
    ```

3. 注解形式
    - `value`：逻辑未删除值，如果不是这个值都会被忽略
    - `delval`：逻辑删除值，删除后赋的值，此处为删除时的时间戳
    ```java
    import com.baomidou.mybatisplus.annotation.TableLogic;

    @TableField(value = "deleted_at")
    @TableLogic(value = "null", delval = "now()")
    private LocalDateTime deletedAt;
    ```

4. 配置形式
    ```yaml
    mybatis-plus:
      global-config:
        db-config:
          # 关键配置：指定实体类中作为逻辑删除标记的字段名
          logic-delete-field: deleted
          # 以下两个是可选配置，有默认值
          logic-delete-value: 1  # 逻辑已删除值，默认为 1
          logic-not-delete-value: 0 # 逻辑未删除值，默认为 0[reference:6][reference:7]
    ```

### 服务器存储用户数据

#### Session

1. 分类
    | 概念 | 主要职责 | 数据通常保存在哪里 |
    |---|---|---|
    | HttpSession | 保存一次会话中的普通数据 | 服务器内存 |
    | Spring Security + Session | 保存登录用户、角色和权限 | HttpSession |
    | Spring Session | 改造和统一 Session 的存储方式 | Redis、数据库等 |

2. 简介
    1. `HttpSession`（原始快递柜）
        - **是什么**：Tomcat/Jetty等服务器自带的**内存格子**。
        - **职责**：放什么都行（用户名、购物车、验证码）。
        - **存哪**：**服务器本地内存**。
        - **痛点**：如果服务器重启，格子炸了，数据全丢。如果有多台服务器（集群），A机器的格子看不到B机器的格子。



    2. `Spring Security + Session`（放进格子里的“VIP通行证”）
        - **是什么**：它**不是**一个新的存储地方，它只是**往上述的快递柜（HttpSession）里塞了一张专用纸条**。
        - **职责**：只负责往 `HttpSession` 里写入“当前登录人是谁、有什么权限”。
        - **存哪**：**依然存在服务器本地内存（HttpSession）里**。 
        - **关键点**：表里写“保存在 HttpSession”，意思就是“我把数据藏在了第1项那个柜子里”，而不是它自己又开了一个新仓库。



    3. `Spring Session`（把整个快递柜搬到云端）
        - **是什么**：它**不是**用来存用户信息的，它是一个**拦截器/替换器**。
        - **职责**：它把 `HttpSession` 原本的“存内存”动作**拦截下来**，强行改成“存 Redis/数据库”。
        - **存哪**：**Redis、数据库等外部中间件**。
        - **关键点**：用了它之后，第1项（原始柜子）就失效了，所有数据（包括第2项写的登录信息）全部被搬到了Redis里。这样多台服务器就能共享同一个柜子了。

    帮你把表格“翻译”成人话：

    | 概念 | 人话解释 |
    |---|---|
    | **HttpSession** | 服务器自带的内存条，存临时数据。 |
    | **Spring Security + Session** | 往上面那个内存条里专门存“登录状态”，它自己没找新地方。 |
    | **Spring Session** | 看不上内存条（容易丢），强行把所有数据（包括上面的登录状态）转移到 Redis 这种永久共享仓库去。 |

3. 【待补充】配置详情


#### 注意事项

1. HS 不等于“内存 Session”

    `HttpSession` 本质上是一个接口/抽象，不规定必须放在内存。

    没有 Spring Session 时，通常由 Tomcat 提供实现：

    ```text
    Cookie: JSESSIONID=S1
            ↓
    Tomcat 的 Session 映射表
            ↓
    S1 → HttpSession
        ├── cart
        ├── code
        └── SPRING_SECURITY_CONTEXT
    ```

    Tomcat 默认往往保存在本机内存中，所以看起来像：

    ```java
    Map<SessionId, HttpSession>
    ```

    但“内存存储”是容器实现策略，不是 `HttpSession` 的本质。

2. SSS 不负责给 Session 换存储环境

    Spring Security Session 管理主要干两类事情：

    - 保存认证信息

        它把 `SecurityContext` 作为普通属性放入 `HttpSession`：

        ```text
        HttpSession
        └── SPRING_SECURITY_CONTEXT
            └── SecurityContext
                └── Authentication
        ```

        Session 本身仍由 Tomcat 或 Spring Session 管理。

    - 管理 Session 安全

        包括：

        - 登录后更换 Session ID，防止会话固定攻击
        - 限制同一账号同时登录数量
        - Session 失效处理
        - 退出时销毁 Session

    > Spring Security 使用 Session，并对它施加登录安全策略。


3. Session ID 不会一直变

    你说的“HS 中的索引会一直变”，应该是在说登录后：

    ```text
    旧 Session ID：S1
    登录成功
    新 Session ID：S2
    ```

    这是 Spring Security 的 Session Fixation Protection。

    但它通常只在身份发生变化时执行，例如匿名用户登录成功，不是每次请求都变。

    在较新的 Servlet 容器中，默认类似：

    ```java
    request.changeSessionId();
    ```

    效果可以理解成：

    ```text
    修改前：S1 → Session 对象
    修改后：S2 → 同一个逻辑 Session
    ```

    属性一般保留：

    ```text
    cart
    verificationCode
    SPRING_SECURITY_CONTEXT
    ```

    但也不能绝对认为“内存位置不变”，因为不同策略行为不同：

    - `changeSessionId`：只更换 ID，保留 Session 属性
    - `migrateSession`：创建新 Session，并复制属性
    - `newSession`：创建新的干净 Session

    因此应关注“逻辑 Session 和属性是否保留”，不要依赖对象的物理内存位置。

4. Spring Session 会接管 HttpSession

    加入 Spring Session + Redis 后，请求链路变成：

    ```text
    Cookie: SESSION=S1
            ↓
    SessionRepositoryFilter
            ↓
    Redis：S1 → Session 数据
                ├── cart
                ├── code
                └── SPRING_SECURITY_CONTEXT
    ```

    Spring Session 提供一个兼容 `HttpSession` 的包装实现，所以业务代码不需要改变：

    ```java
    HttpSession session = request.getSession();
    session.setAttribute("cart", cart);
    ```

    代码看到的依然是 `HttpSession`，但数据实际可能在 Redis。

    官方的说法也是：`SessionRepositoryFilter` 会用 Spring Session 提供的实现替换原来的 `HttpSession`。

5. SS 需要把索引存在原来的 HS 里吗？

    **不需要。**

    如果使用 Spring Session + Redis，主要查找链路是：

    ```text
    浏览器 Cookie 保存 Session ID
            ↓
    Spring Session 根据 ID 查询
            ↓
    Redis 保存 Session 数据
    ```

    也就是：

    ```text
    客户端：SESSION=S1

    Redis：
    S1 → {
        creationTime,
        lastAccessedTime,
        attributes,
        SPRING_SECURITY_CONTEXT
    }
    ```

    原生 Tomcat `HttpSession` 不需要再保存一份索引，否则会出现：

    - 重复存储
    - 两份状态可能不一致
    - 换一台服务器后仍然找不到 Tomcat 内存中的索引
    - 失去分布式 Session 的意义

    所以 Spring Session 接管后的结构是：

    ```text
                    ┌──────────────────────┐
    SESSION=S1 ─────→│ Spring Session       │
                    │ SessionRepository    │
                    └──────────┬───────────┘
                                ↓
                        Redis / JDBC
    ```

    而不是：

    ```text
    SESSION=S1 → Tomcat HttpSession → Redis
    ```

6. 但 Spring Session 可能有自己的二级索引

    这里需要区分两种索引。

    - 主索引：Session ID → Session

        用于正常请求：

        ```text
        S1 → Session 数据
        ```

    - 二级索引：用户名 → Session ID 集合

        用于查询某个用户登录了哪些设备：

        ```text
        zhangsan → [S1, S8, S15]
        ```

        它可以服务于：

        - 限制同时登录数量
        - 强制用户下线
        - 查询用户当前登录设备
        - 集群环境下的 Session 管理

    这些索引也存储在 Redis、数据库等 Session Repository 中，而不是原生 Tomcat `HttpSession` 中。Spring Session 可以通过 `SpringSessionBackedSessionRegistry` 给 Spring Security 提供分布式并发会话查询。
    > Spring Session 不需要把主索引再放进原生 HttpSession。Session ID 在客户端 Cookie 中，Session 数据和相关索引位于 Redis/JDBC 等 Repository 中；Spring Security 只通过统一的 `HttpSession` 接口使用它。

7. 最准确的模型

    ```text
    Spring Security
    ├── 生成并读取 SecurityContext
    ├── 把 SecurityContext 作为属性放进 HttpSession
    ├── 登录时可能更换 Session ID
    └── 根据需要管理并发登录

    HttpSession
    └── 面向业务代码的统一会话接口

    Spring Session
    ├── 接管 HttpSession 的具体实现
    ├── 根据 Session ID 查找数据
    ├── 将数据保存到 Redis/JDBC
    └── 可维护“用户名 → Session ID”二级索引
    ```


### 错误码

本节为示例，本人脑力有限，已经燃尽了还没有完全看明白



### 外部资源

1. 头像

### 测试规范

1. 数据库测试
    - 添加注解`@Transaction`和`@SpringBootTest`
    ```java
    @SpringBootTest
    @Transactional
    public class UserAccountServiceTest {}
    ```

2. 所有的数据库相关测试，在测试结束后会自动回滚，因此无法直接通过打断点获取测试程序修改的信息。
    - 【未验证】如果你非得看，可以把数据库隔离等级改成读未提交，查询未提交的修改。

### Spring 机制

#### 配置文件加载

## 后端业务

### 插件推荐

1. MyBatisX：一键生成MyBatisPlus代码，但可能会出现版本兼容问题
2. GenerateAllSetter：安装后，在一个`new`的对象上`Alt+Enter`可以快速生成get和set方法

### 注册

#### 业务梳理

1. 基本功能
    - 用户在前端输入账户、密码和校验码
    - 校验账户密码
        - 账户长度不小于4
        - 密码大于等于8位
        - 账户不能重复，不能包含特殊字符
        - 密码与确认密码相同
    - 对密码加密

2. 数据库交互

    - 向数据库插入数据

#### 开始开发

1. 引入包
    - 用于简化开发
    ```xml
    <!-- Source: https://mvnrepository.com/artifact/org.apache.commons/commons-lang3 -->
    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-lang3</artifactId>
        <version>3.20.0</version>
        <scope>compile</scope>
    </dependency>
    ```

2. 服务接口
    ```java

    ```
3. 服务类

4. Controller接口类

2. 服务层
    ```java
    @Service
    public class UsrTestServiceImpl extends ServiceImpl<UsrTestMapper, UsrTest>
            implements UsrTestService {

            /**
            * 账号规则：字母开头，只允许字母、数字、下划线，长度 4~20
            */
            private static final Pattern ACCOUNT_PATTERN =
                    Pattern.compile("^[a-zA-Z][a-zA-Z0-9_]{3,19}$");

            /**
            * 密码最小长度
            */
            private static final int MIN_PASSWORD_LENGTH = 8;

            /**
            * 默认用户名
            */
            private static final String DEFAULT_USERNAME = "新用户";

            /**
            * 默认用户状态：0 正常
            */
            private static final int DEFAULT_STATUS = 0;

            /**
            * 密码加密器
            */
            private static final BCryptPasswordEncoder PASSWORD_ENCODER = new BCryptPasswordEncoder();

            @Transactional(rollbackFor = Exception.class)
            @Override
            public long userRegisterPro(String userAccount, String password, String checkPassword) {

                // 1. 校验参数是否为空
                if (StringUtils.isAnyBlank(userAccount, password, checkPassword)) {
                    return -1;
                }

                // 2. 去除账号首尾空格
                String account = userAccount.trim();

                // 3. 校验账号格式
                if (!ACCOUNT_PATTERN.matcher(account).matches()) {
                    return -1;
                }

                // 4. 校验密码长度
                if (password.length() < MIN_PASSWORD_LENGTH) {
                    return -1;
                }

                // 5. 校验两次密码是否一致
                if (!password.equals(checkPassword)) {
                    return -1;
                }

                // 6. 校验账号是否重复
                LambdaQueryWrapper<UsrTest> queryWrapper = new LambdaQueryWrapper<>();
                queryWrapper.eq(UsrTest::getAccount, account);

                long count = this.count(queryWrapper);
                if (count > 0) {
                    return -1;
                }

                // 7. 加密密码
                String encryptPassword = PASSWORD_ENCODER.encode(password);

                // 8. 创建用户对象
                UsrTest user = new UsrTest();
                user.setAccount(account);
                user.setUsername(DEFAULT_USERNAME);
                user.setPassword(encryptPassword);
                user.setStatus(DEFAULT_STATUS);
                user.setGender(0);

                // 9. 保存用户
                try {
                    boolean saveResult = this.save(user);
                    if (!saveResult || user.getId() == null) {
                        return -1;
                    }
                } catch (DuplicateKeyException e) {
                    return -1;
                }

                // 10. 返回用户 id
                return user.getId();
            }
    }
    ```

3. 单元测试
    ```java
    @Test
    void userRegister_accountIsBlank_shouldFail() {
        long result = usrTestService.userRegisterPro("", "12345678", "12345678");
        Assertions.assertEquals(-1L, result);
    }

    @Test
    void userRegister_accountTooShort_shouldFail() {
        long result = usrTestService.userRegisterPro("lcq", "12345678", "12345678");
        Assertions.assertEquals(-1L, result);
    }

    @Test
    void userRegister_passwordTooShort_shouldFail() {
        long result = usrTestService.userRegisterPro("lcqaaaaaaaa", "123456", "123456");
        Assertions.assertEquals(-1L, result);
    }

    @Test
    void userRegister_accountContainsSpace_shouldFail() {
        long result = usrTestService.userRegisterPro("l cqaaaaaaaa", "12345678", "12345678");
        Assertions.assertEquals(-1L, result);
    }

    @Test
    void userRegister_passwordNotMatch_shouldFail() {
        long result = usrTestService.userRegisterPro("lcqaaaaaaa", "12345678", "123456789");
        Assertions.assertEquals(-1L, result);
    }

    @Test
    void userRegister_validInput_shouldSuccess() {
        String userAccount = "newUser" + System.currentTimeMillis();

        long result = usrTestService.userRegisterPro(userAccount, "12345678", "12345678");

        Assertions.assertTrue(result > 0);
    }
    ```

### 登录

#### 业务梳理

1. 基本功能
    - POST请求
    - 接收账号、密码
    - 返回用户信息（脱敏）

2. 基本逻辑
    - 校验账户密码是否合法（规避无效的数据库查询）
        - 非空
        - 账户长度合规
        - 密码长度合规
        - 账户不包含特殊字符
    - 校验密码，与密文比对
    - 记录用户登录态（SESSION）
    - 返回用户信息（脱敏）




## 上线与部署

- 多环境
- 部署
    - 原始
    - 宝塔
    - 容器
    - 容器平台

- 前后端联调
- 扩展与规划


### 多环境

1. 简介
    - 每个环境互不影响
    - 根据项目的不同阶段接入不同的环境，如：开发、测试、生产
    - 根据环境需要调整环境配置，如：JVM参数、本地日志级别、精简依赖

2. 多环境分类
    - 本地环境，即`localhost`，在开发者自己电脑上运行的环境
    - 开发环境，远程连接同一台开发服务器
    - 测试环境，开发、测试、产品、性能测试、功能测试、系统集成测试...，用独立的数据库与服务器
    - 预发布环境，类似体验服，用正式环境的数据检查
    - 正式环境
    - 沙箱（实验）环境，测试功能等，用完即销毁。

### 前端实战

1. 修改请求目标为非本地地址。
    - 开发阶段是`run dev`，更改启动环境需要更改路由。

2. 

### 【腾讯云】服务器购买与防护

#### CloudFlare 配置

#### 根证书配置

1. https://developers.cloudflare.com/ssl/origin-configuration/origin-ca/


### 【腾讯云】Nginx部署与规范

#### 部署服务器

1. 目录结构

    ```text
    /opt/docker/nginx/
    ├── compose.yml
    ├── .env                         # 只放镜像引用，不放私钥
    ├── conf.d/
    │   ├── 00-catchall.conf
    │   ├── 10-cloudflare-realip.conf
    │   ├── 20-upstream.conf
    │   └── dashboard.conf
    └── certs/
        ├── dashboard.pem            # Cloudflare 创建的源站证书
        ├── dashboard.key            # 对应私钥
        └── origin-ca-root.pem       # 本地 curl 验证用，选证书签发 CA 对应的根

    /srv/www/dashboard.19271949.xyz/
    ├── releases/
    │   ├── v1.0.0/
    │   │   └── public/
    │   │       ├── index.html
    │   │       └── assets/
    │   └── v1.0.1/
    │       └── public/
    │           ├── index.html
    │           └── assets/
    ├── current -> releases/v1.0.1
    └── shared/
    ```

2. 各目录职责
    | 目录 | 职责 |
    |---|---|
    | `/opt/docker/nginx` | 描述 Nginx 容器怎么运行 |
    | `conf` | Nginx 配置 |
    | `/srv/www/.../releases` | 保存每次发布的前端版本 |
    | `current` | 指向当前线上版本 |
    | `shared` | 跨版本共享文件，纯静态项目可以暂时不用 |

3. Docker运行配置说明
    这个方案使用官方 Nginx 镜像，并在外部挂载配置和网站文件，因此：
    - 不需要 `Dockerfile`
    - 不需要 `docker build`
    - 只需要 `compose.yml`

    通过配置`compose.yml`，即可`docker compose up`一键启动容器

4. 目录创建

    ```bash

    # 创建目录并设置权限
    sudo install -d -m 0755 /opt/docker/nginx/conf.d
    sudo install -d -m 0700 /opt/docker/nginx/certs
    sudo install -d -m 0755 \
    /srv/www/dashboard.19271949.xyz/releases/v1.0.0/public \
    /srv/www/dashboard.19271949.xyz/shared

    # 已有网络先检查，不要重复创建或擅自删除
    sudo docker network inspect app-net
    # 仅当上条明确返回网络不存在时执行
    sudo docker network create --driver bridge app-net
    
    # 3. 为两个版本创建占位首页
    sudo touch \
      /srv/www/dashboard.19271949.xyz/releases/{v1.0.0,v1.0.1}/public/index.html
    
    # 4. 创建 current 软链接，指向 v1.0.1；已有 current 时不会覆盖
    sudo ln -sT \
      releases/v1.0.1 \
      /srv/www/dashboard.19271949.xyz/current
    ```

#### 准备证书

1. 配置源站自签证书
    在 Cloudflare 创建覆盖 `dashboard.19271949.xyz` 的 Origin CA 证书，将 Certificate 和 Private key 分别保存到上面的 `.pem`、`.key`，可以用 `sudoedit` 编辑。私钥设置为 root 所有、0600；Nginx 主进程读取它。根证书从 [Origin CA 官方下载位置](https://developers.cloudflare.com/ssl/origin-configuration/origin-ca/#cloudflare-origin-ca-root-certificate) 获取，按实际签发 CA 选择。

    ```bash
    sudo chmod 0600 /opt/docker/nginx/certs/dashboard.key
    sudo openssl x509 -in /opt/docker/nginx/certs/dashboard.pem \
    -noout -subject -issuer -dates -ext subjectAltName

    # 两次输出的公钥摘要必须相同；此命令不打印私钥
    sudo openssl x509 -in /opt/docker/nginx/certs/dashboard.pem -pubkey -noout \
    | openssl pkey -pubin -outform DER | openssl dgst -sha256
    sudo openssl pkey -in /opt/docker/nginx/certs/dashboard.key -pubout -outform DER \
    | openssl dgst -sha256
    ```

    Origin CA 证书不被普通浏览器直接信任。灰云直连若需要浏览器信任，应使用公共 CA 证书。长有效期仍需记录到期日、私钥保存位置和泄露后的吊销重签流程。

2. 注意事项
    - 私钥`*.key`单独一个文件，叶子`*.pem`和Cloud Flare 证书组成完整的文件
    - 还可以生成两个`default`文件用于其他未识别站点的访问，具体查看后面的附加材料

#### 配置Docker

1. 前置准备
    ```bash
    # 前面已提及，用途是配置的Docker互相通信
    sudo docker network create --driver bridge app-net

    cd /opt/docker/nginx
    sudo docker pull nginx:1.30.4
    # 仅在 pull 成功后执行，记录刚拉取镜像的不可变引用
    sudo docker image inspect nginx:1.30.4 --format '{{index .RepoDigests 0}}'
    sudoedit /opt/docker/nginx/.env
    ```
2. 配置`compose.yml`
    - 注意，实际的容器名应默认为`{name}-{service.?}-[序号]`，这里是`nginx-prod-nginx-1`
    ```yaml
    name: nginx-prod
    
    services:
      nginx:
        image: nginx:1.30.4
        restart: unless-stopped
        stop_signal: SIGQUIT
        stop_grace_period: 30s
    
        ports:
          - "80:80"
          - "443:443"
    
        volumes:
          - type: bind
            # Docker 宿主机上的绝对路径。
            source: /opt/docker/nginx/conf.d
            target: /etc/nginx/conf.d
            read_only: true
            bind:
              create_host_path: false      
              
          - type: bind
            # Docker 宿主机上的绝对路径。
            source: /opt/docker/nginx/certs
            target: /etc/nginx/certs
            read_only: true
            bind:
              create_host_path: false
    
          - type: bind
            source: /srv/www/
            target: /srv/www/
            read_only: true
            bind:
              create_host_path: false
    
        logging:
          driver: local
          options:
            max-size: "10m"
            max-file: "5"
        # 加入共享网络，与其他独立 Compose 项目的服务通信。
        networks:
          - app-net
    
    # 顶层网络声明，与 services 同级。
    networks:
      app-net:
        # 网络由项目外创建和管理；此处引用已存在的网络。
        external: true
        name: app-net
    
    ```

    >这里会发布宿主机端口，应与腾讯云安全组联动控制访问来源。Docker 发布的端口可能绕过通常的 UFW 入站规则；使用 iptables 与 nftables 后端时处理方式也不同，不要盲目复制一段 `iptables` 命令。明确你的 Docker 防火墙后端后再配置，并从外部验证。[Docker 防火墙说明](https://docs.docker.com/engine/network/packet-filtering-firewalls/)
    - 换句话说，Docker的策略会绕过ufw防火墙，云服务建议配置安全组策略，弥补防火墙缺陷。



2. 配置Nginx
    - 与系统版不同，Docker版不存在`sites-enable`目录，统一并入`conf.d`
    - 对于部署网站而言，只需增加server块
    - 文件保存到`/opt/docker/nginx/conf`

    - **注意：后端相关配置在部署完毕前需要注释掉，否则Nginx会因为找不到服务疯狂重启。**

    - `./conf.d/dashboard.conf`
        ```nginx
        server {
            listen 80;
            server_name dashboard.19271949.xyz;
            return 301 https://dashboard.19271949.xyz$request_uri;
        }
        
        server {
            listen 443 ssl;
            http2 on;
            server_name dashboard.19271949.xyz;
        
            ssl_certificate /etc/nginx/certs/dashboard.pem;
            ssl_certificate_key /etc/nginx/certs/dashboard.key;
            ssl_protocols TLSv1.2 TLSv1.3;
            ssl_session_cache shared:SSL:10m;
            ssl_session_timeout 1d;
        
            root /srv/www/dashboard.19271949.xyz/current/public;
            index index.html;
        
            # 限制当前域下的请求体最大12M
            client_max_body_size 12m;
        
            # 附带永不缓存标签
            location = /index.html {
                try_files $uri =404;
                add_header Cache-Control "no-cache" always;
            }
            location / {
                try_files $uri $uri/ /index.html;
                add_header Cache-Control "no-cache" always;
            }
            location ^~ /assets/ {
                try_files $uri =404;
                add_header Cache-Control "public, max-age=604800, immutable";
            }
        
            # 管理端点仅供容器内部探活，不公开到互联网
            location = /api/actuator { return 404; }
            location ^~ /api/actuator/ { return 404; }
            location = /api { return 404; }
        
            location /api/ {
                proxy_pass http://usercenter_api/;
                proxy_http_version 1.1;
                proxy_set_header Connection "";
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $remote_addr;
                proxy_set_header X-Forwarded-Host $host;
                proxy_set_header X-Forwarded-Proto https;
                proxy_set_header X-Forwarded-Port 443;
                proxy_set_header Forwarded "";
                proxy_set_header X-Forwarded-Prefix "";
                proxy_set_header X-Forwarded-Ssl "";
                proxy_set_header CF-Connecting-IP "";
                proxy_connect_timeout 5s;
                proxy_read_timeout 60s;
                proxy_send_timeout 60s;
                proxy_intercept_errors off;
                proxy_cache off;
                proxy_hide_header Cache-Control;
                add_header Cache-Control "no-store" always;
            }
        }
        ```

        以下为注释版
        ```nginx
        location /api/ {

            # =========================================================
            # 1. 上游服务与 HTTP 连接
            # =========================================================

            # 转发到 usercenter_api upstream
            # 注意末尾不加 "/"，会保留原始 /api/... 请求路径
            proxy_pass http://usercenter_api/;

            # 与后端使用 HTTP/1.1
            # 便于连接复用，也为 WebSocket 等扩展场景提供基础
            proxy_http_version 1.1;

            # 清除客户端传入的 Connection 头
            # 避免 hop-by-hop 头继续传递给后端
            proxy_set_header Connection "";


            # =========================================================
            # 2. 向后端传递原始请求信息
            # =========================================================

            # 保留用户访问的原始域名
            proxy_set_header Host $host;

            # 真实客户端 IP
            # 前提：Nginx 已通过 Cloudflare Real IP 配置恢复 $remote_addr
            proxy_set_header X-Real-IP $remote_addr;

            # 向后端传递客户端来源 IP
            # 此处直接使用经过 Nginx 信任边界确认后的 $remote_addr
            proxy_set_header X-Forwarded-For $remote_addr;

            # 用户最初访问的 Host
            proxy_set_header X-Forwarded-Host $host;

            # 用户对外访问协议为 HTTPS
            # Nginx 与后端内部即使使用 HTTP，也不影响后端判断原始协议
            proxy_set_header X-Forwarded-Proto https;

            # 用户对外访问端口为 443
            proxy_set_header X-Forwarded-Port 443;


            # =========================================================
            # 3. 清理不需要或可能产生歧义的代理头
            # =========================================================

            # 不向后端继续传递 RFC Forwarded 头
            # 统一使用上面明确设置的 X-Forwarded-* 体系
            proxy_set_header Forwarded "";

            # 当前没有进行 /api 前缀剥离，因此不传递代理路径前缀
            proxy_set_header X-Forwarded-Prefix "";

            # 清除旧式 HTTPS 标记
            # HTTPS 状态统一由 X-Forwarded-Proto 表示
            proxy_set_header X-Forwarded-Ssl "";

            # Cloudflare 专有真实 IP 头只在 Nginx 边界使用
            # 后端统一通过 X-Real-IP / X-Forwarded-For 获取客户端 IP
            proxy_set_header CF-Connecting-IP "";


            # =========================================================
            # 4. 上游连接与响应超时
            # =========================================================

            # 建立到后端 TCP 连接的最大等待时间
            proxy_connect_timeout 5s;

            # 等待后端返回数据的最大间隔
            proxy_read_timeout 60s;

            # 向后端发送请求数据的最大间隔
            proxy_send_timeout 60s;


            # =========================================================
            # 5. 错误响应处理
            # =========================================================

            # 后端返回 4xx / 5xx 时直接透传
            # 不让 Nginx 使用自身 error_page 替换 API 响应
            proxy_intercept_errors off;


            # =========================================================
            # 6. API 缓存策略
            # =========================================================

            # Nginx 不缓存 API 响应
            proxy_cache off;

            # 隐藏后端自己返回的 Cache-Control
            # 防止后端意外声明 public / max-age 等缓存策略
            proxy_hide_header Cache-Control;

            # 由 Nginx 统一声明 API 响应禁止存储
            # 避免浏览器、CDN 或中间代理缓存用户数据、会话信息等
            add_header Cache-Control "no-store" always;
        }
        ```
    - `catchall.conf`
        ```nginx
        # 隐藏Nginx版本
        server_tokens off;

        # ============ 80 兜底：未匹配 Host → 直接断开 ============
        server {
            listen 80 default_server;

            # 启用 IPv6 后取消注释
            # listen [::]:80 default_server;

            server_name _;

            # 兜底请求默认不记录访问日志
            access_log off;

            # 排查问题时可临时改为输出到 Docker 日志
            # access_log /dev/stdout;

            return 444;
        }


        # ============ 443 兜底：未匹配 SNI → TLS 握手阶段直接拒绝 ============
        server {
            listen 443 ssl default_server;

            # 启用 IPv6 后取消注释
            # listen [::]:443 ssl default_server;

            server_name _;

            # 兜底请求默认不记录访问日志
            access_log off;

            # 排查问题时可临时改为输出到 Docker 日志
            # access_log /dev/stdout;

            ssl_reject_handshake on;
        }
        ```

4. `10-cloudflare-realip.conf`
    从 [Cloudflare IPv4 列表](https://www.cloudflare.com/ips-v4/) 和 [IPv6 列表](https://www.cloudflare.com/ips-v6/) 取当前全部网段，每个网段写一行 `set_real_ip_from 网段;`，最后加 `real_ip_header CF-Connecting-IP;`。不要使用 `0.0.0.0/0` 或 `::/0` 作为信任来源。

    下面脚本只生成候选配置，下载或解析失败就终止；需要服务器已安装 `curl` 和 `python3`。运行后检查输出是否为完整网段，再安装到 `conf.d`：

    ```bash
    set -euo pipefail
    ip_workdir=$(mktemp -d)
    curl -fsS https://www.cloudflare.com/ips-v4/ -o "$ip_workdir/ips-v4"
    curl -fsS https://www.cloudflare.com/ips-v6/ -o "$ip_workdir/ips-v6"
    python3 - "$ip_workdir" <<'PY'
    import ipaddress, pathlib, sys
    p = pathlib.Path(sys.argv[1])
    lines = []
    for filename, version in [('ips-v4', 4), ('ips-v6', 6)]:
        networks = [ipaddress.ip_network(x.strip()) for x in
                    (p / filename).read_text().splitlines() if x.strip()]
        if not networks or any(n.version != version or n.prefixlen == 0 for n in networks):
            raise SystemExit('Cloudflare IP 列表无效，请人工核查')
        lines.extend(f'set_real_ip_from {n};' for n in networks)
    lines.append('real_ip_header CF-Connecting-IP;')
    (p / '10-cloudflare-realip.conf').write_text('\n'.join(lines) + '\n')
    PY
    cat "$ip_workdir/10-cloudflare-realip.conf"
    # 检查后执行；已有文件先备份，之后按第六节 nginx -t / reload
    sudo install -m 0644 "$ip_workdir/10-cloudflare-realip.conf" \
    /opt/docker/nginx/conf.d/10-cloudflare-realip.conf
    ```

    Cloudflare 网段变化时，安全组与这份信任列表都要更新。若 Docker/NAT 或其他入口代理改变了 Nginx 看到的连接源地址，应先查日志确认，不能为了“修好 IP”把整个 Docker 子网无条件加入信任列表。

      所以 Nginx 直接看到的连接者，通常是 **Cloudflare 节点**，而不是最终用户。

      例如真实用户：

      ```text
      用户 IP：1.2.3.4
      Cloudflare IP：104.x.x.x
      ```

      请求到 Nginx 后，如果什么都不配置，Nginx 可能认为：

      ```text
      remote_addr = 104.x.x.x
      ```

      于是你的日志全成：

      ```text
      104.x.x.x
      104.x.x.x
      104.x.x.x
      ```

      没法知道真正用户是谁。

      Cloudflare 会额外带一个 HTTP Header：

      ```http
      CF-Connecting-IP: 1.2.3.4
      ```

      所以这份配置：

      ```nginx
      set_real_ip_from 173.245.48.0/20;
      set_real_ip_from 103.21.244.0/22;
      ...
      real_ip_header CF-Connecting-IP;
      ```

      就是告诉 Nginx：

      > 如果请求确实来自 Cloudflare 官方 IP，那么我信任它携带的 `CF-Connecting-IP`，把那个 IP 当作真实客户端 IP。

      于是变成：

      ```text
      Cloudflare连接IP
      104.x.x.x
            ↓
      Nginx确认：
      “这是Cloudflare官方IP，可以信”
            ↓
      读取 CF-Connecting-IP
            ↓
      真实用户 = 1.2.3.4
      ```

3. `20-upstream.conf`
    - 创建后端路由，通过Docker内部DNS精准找到服务。
    - 配置占用64k内存，超时时间为5s，与后端Docker建立16个长连接构成连接池
    - 低版本Nginx解析时，不会随时向Docker DNS获取解析地址，因此在重启后端后可能导致一定时间内路由不到，通过此法可解决此问题。
    - 注意：负载均衡涉及的多个实例仍然需要手动配置，若实例数量是动态的，需要考虑服务注册于发现，这就是微服务的范畴了。
    ```nginx
    upstream usercenter_api {
        zone usercenter_api 64k;
        resolver 127.0.0.11 valid=10s ipv6=off;
        resolver_timeout 5s;
        server usercenter-backend:8080 resolve;
        keepalive 16;
    }
    ```




#### 启动容器

1. 首次启动

    ```bash
    cd /opt/docker/nginx

    # 1. 检查 Compose 声明（语法预检）
    sudo docker compose config -q

    # 2. 下载镜像
    sudo docker compose pull

    # 3. 用临时容器检查 Nginx 配置
    sudo docker compose run --rm --no-deps nginx nginx -t

    # 4. 检查通过后，启动正式容器
    sudo docker compose up -d
    ```

    启动后执行

    ```bash
    sudo docker compose ps
    sudo docker compose logs --tail=100 nginx

    curl -fsS \
    -H 'Host: dashboard.19271949.xyz' \
    http://127.0.0.1/

    # 以下命令可在任意目录执行

    # 查看状态（在任何目录都能执行）
    sudo docker compose -f /opt/docker/nginx/compose.yml ps

    # 查看日志（在任何目录都能执行）
    sudo docker compose -f /opt/docker/nginx/compose.yml logs --tail=100 nginx
    ```

2. 配置更新

    ```bash
    
    # 语法检查
    # && 是安全锁，可以防止语法检查未通过时加载
    sudo docker compose exec -T nginx nginx -t &&
    sudo docker compose exec -T nginx nginx -s reload

    sudo docker compose logs --tail=100 nginx
    ```

3. 更改容器配置后
    ```bash
    sudo docker compose config -q
    sudo docker compose up -d
    ```

4. 更新前端静态资源
    ```bash
    # # 4. 创建 current 软链接，指向 v1.0.1；已有 current 时不会覆盖
    # sudo ln -sT \
    #   releases/v1.0.1 \
    #   /srv/www/dashboard.19271949.xyz/current

    # 修改
    sudo ln -sfnT \
      releases/v1.0.1 \
      /srv/www/dashboard.19271949.xyz/current
    ```

### 【腾讯云】SpringBoot部署与规范

#### 【待整理】

/opt/docker/usercenter-backend/
├── compose.yml
├── .env                      # 当前正在运行的版本
├── Dockerfile
├── .dockerignore
└── releases/
    ├── v1.0.0.env
    ├── v1.0.1.env
    └── v1.0.2.env

#### 一、部署前先确认

1. 版本与代码前提


    | 需要检查 | 检查位置／方法 |
    | --- | --- |
    | Spring Boot 版本 | `pom.xml` 的 parent 或 dependencyManagement |
    | 编译 Java 版本 | `java.version`、`maven.compiler.release`、compiler 插件及 `./mvnw -v` |
    | 可执行 Jar | 包含 Spring Boot loader、`BOOT-INF/classes` 和依赖；不能使用 `.original` 或普通薄 Jar |
    | 路由前缀 | `/api` 来自 context-path 还是 Controller，不能叠加成 `/api/api` |
    | 数据访问 | Connector/J、MyBatis-Plus、Druid starter 与当前 Boot 版本兼容 |
    | 数据库结构 | 使用后端项目真实、已审阅的建表或迁移脚本 |
    | 登录机制 | 原笔记是 Spring Security + Session + CSRF，要把 Cookie 与反向代理一起验收 |

    Boot 2.7 的最低 Java 基线为 8，Boot 3 和 Boot 4 为 17；具体支持的 Java 上限依小版本而变。Java 21 编译的字节码不能因为“17 是最低要求”就放进 Java 17 镜像。旧 Boot 的依赖体系、`javax`/`jakarta` 差异也不能靠换 JDK 解决。[Boot 2.7 要求](https://docs.spring.io/spring-boot/docs/2.7.18/reference/html/getting-started.html#getting-started.system-requirements)、[Boot 3.5 要求](https://docs.spring.io/spring-boot/3.5/system-requirements.html)、[Boot 4 要求](https://docs.spring.io/spring-boot/system-requirements.html)

#### 二、目录结构与职责

```text
开发机：后端项目/
├── pom.xml
├── mvnw
├── src/
├── target/                          # Maven 产物
└── deploy-image/                    # 本次制作镜像的最小上下文
    ├── Dockerfile
    ├── .dockerignore
    ├── app.jar
    └── runtime-image.txt            # 本次使用的基础镜像 digest 记录

服务器：
/opt/docker/usercenter-backend/
├── compose.yml
└── .env                            # APP_VERSION、APP_IMAGE；无密码

/srv/usercenter-backend/
├── releases/
│   ├── v1.0.0/
│   │   ├── app-image.tar
│   │   ├── app-image.tar.sha256
│   │   └── config/application-prod.yml
│   └── v1.0.1/...
└── shared/                         # 以后有上传文件时再挂载专用子目录

/opt/secrets/usercenter/DB_PASSWORD  # MySQL 教程中已创建的同一密码文件
```

| 内容 | 保存原则 |
| --- | --- |
| Jar 与镜像 | 同一版本构建一次，发布后不覆盖原标签 |
| 生产非秘密配置 | 随发布版本保存，回滚时与应用配套 |
| 密码 | 由 Compose secret 挂载，不打进 Jar 或镜像 |
| 日志 | 写 stdout/stderr，由 Docker 限额轮转 |
| 数据库和上传文件 | 属于持久化数据，不随应用回滚或删除 |

#### 三、准备生产配置与健康检查

1. `application-prod.yml`

    部署时保存到服务器 `/srv/usercenter-backend/releases/v1.0.0/config/application-prod.yml`。该文件补充 Jar 内的公共配置，保留原项目 MyBatis/Druid 等必要设置。生产 Profile 由 Compose 激活，不在公共 `application.yml` 中写死 `dev`。

    ```yaml
    server:
    address: 0.0.0.0
    port: 8080
    shutdown: graceful
    forward-headers-strategy: framework
    servlet:
        context-path: /api
        session:
        timeout: 30m
        cookie:
            http-only: true
            secure: true
            same-site: lax

    spring:
    config:
        import: "configtree:/run/secrets/"
    lifecycle:
        timeout-per-shutdown-phase: 30s
    datasource:
        url: ${DB_URL}
        username: ${DB_USERNAME}
        password: ${DB_PASSWORD}
        driver-class-name: com.mysql.cj.jdbc.Driver
    sql:
        init:
        mode: never
    servlet:
        multipart:
        max-file-size: 10MB
        max-request-size: 12MB

    management:
    endpoints:
        web:
        exposure:
            include: health
    endpoint:
        health:
        show-details: never
        probes:
            enabled: true
        group:
            readiness:
            include: readinessState,db
            liveness:
            include: livenessState

    logging:
    level:
        root: info
        com.lcq.usercenter: info
    ```

    **实际采纳的配置（封装在jar里，对应v1.0.2）**

    ```yml
    # 生产环境专用配置
    server:
      shutdown: graceful
      forward-headers-strategy: framework
    
      servlet:
        session:
          cookie:
            # 生产环境通过 HTTPS 访问
            secure: true
    
    spring:
      # 将 /run/secrets/DB_PASSWORD 读取为名为 DB_PASSWORD 的配置属性
      config:
        import: "configtree:/run/secrets/"
    
      lifecycle:
        timeout-per-shutdown-phase: 30s
    
      datasource:
        url: ${DB_URL}
        username: ${DB_USERNAME}
        password: ${DB_PASSWORD}
    
      sql:
        init:
          # 生产表结构由部署前的 SQL 脚本维护，业务账号不执行 DDL
          mode: never
    ```

    关键点：

    - `configtree` 把 `/run/secrets/DB_PASSWORD` 的内容读成 Spring 属性，供 `${DB_PASSWORD}` 使用；它不是自动设置系统环境变量。这里不加 `optional:`，也不提供密码默认值。configtree 从 Boot 2.4 起提供。
    - `server.address` 要允许 Nginx 从容器网络连接；不公开后端端口通过 Compose 无 `ports` 实现。
    - `forward-headers-strategy` 依赖 [Nginx 修订配置](nginx-review.md) 清除不可信的转发头。只加此项并不能安全地信任所有请求来源。
    - `secure: true` 表示浏览器只通过 HTTPS 发送 Session Cookie。正式登录测试应走域名 HTTPS；在公网直接访问 HTTP 后端会造成 Cookie 不生效。
    - `spring.sql.init.mode: never` 防止误用自动初始化脚本，不会替你关闭 Flyway、Liquibase 或自写初始化逻辑。若项目使用这些迁移工具，需要单独设计迁移账号与发布步骤。

    上传配置允许单文件最多 10 MiB，总请求最多 12 MiB，为 multipart 边界和字段留出空间；Nginx 的请求体限制对应设为 `12m`。

    配置来源：[Spring 外部配置](https://docs.spring.io/spring-boot/reference/features/external-config.html)、[ConfigTree 版本](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/env/ConfigTreePropertySource.html)、[反向代理配置](https://docs.spring.io/spring-boot/how-to/webserver.html#howto.webserver.use-behind-a-proxy-server)、[数据库初始化](https://docs.spring.io/spring-boot/how-to/data-initialization.html)。

2. 健康检查需要后端实际支持

    在后端 `pom.xml` 加入 Actuator；如果已经有，不重复添加。依赖版本交给现有 Spring Boot 依赖管理：

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    ```

    本例没有单独的 management 端口，因此容器内部地址为：

    ```text
    GET http://127.0.0.1:8080/actuator/health/readiness
    ```

    在**现有** Spring Security 规则中，先于兜底认证规则放行探针 GET。Boot 3 写法片段如下；合并进去，不要因此删掉原登录、管理员和 CSRF 规则，也不要多创建一个互相冲突的过滤链：

    ```java
    .requestMatchers(
        HttpMethod.GET,
        "/actuator/health/readiness",
        "/actuator/health/liveness"
    ).permitAll()
    ```

    这里的 `/api` 是 context-path，Security matcher 不包含它。自定义鉴权 Filter 也要允许探针请求通过。Nginx 已屏蔽 `/api/actuator`，不对公网暴露环境变量、堆转储或管理端点。[Spring Security 请求匹配](https://docs.spring.io/spring-security/reference/servlet/authorization/authorize-http-requests.html)、[Actuator 探针](https://docs.spring.io/spring-boot/3.5/reference/actuator/endpoints.html#actuator.endpoints.kubernetes-probes)

    本例把 `db` 显式加入 readiness，所以数据库连接异常时不宣称服务就绪。liveness 只判断应用自身，避免数据库故障引起全体进程重启。数据库探针也不能证明业务表正确、权限完整或登录流程无误。

#### 四、开发机：打包 Jar 并制作镜像

1. 先测试，再打包

    在后端仓库的 Bash／WSL 中执行。先按原笔记配置独立测试库和 `TEST_DB_*`，确保集成测试显式激活 `test`；不要让测试指向生产库。

    ```bash
    java -version
    ./mvnw -v
    ./mvnw -B clean verify
    ```

    没有 Maven Wrapper 的项目使用已核对版本的 `mvn`。`verify` 失败先修复，不把 `-DskipTests` 写成默认部署步骤。若 `pom.xml` 没有 Spring Boot Maven 插件，应在已有构建配置中补齐 `spring-boot-maven-plugin` 的 `repackage` 设置，直到产物确实是可执行 Jar。[Spring Boot Maven 打包](https://docs.spring.io/spring-boot/maven-plugin/packaging.html)

2. 选中正确的 Jar
    - 一般可以通过`mvn clean package`简易打包，但要去掉pom中可能出现的`<skip>true</skip>`

    ```bash
    set -euo pipefail
    artifact_name=$(./mvnw -q -Dstyle.color=never -DforceStdout \
    help:evaluate -Dexpression=project.build.finalName)
    test -s "target/$artifact_name.jar"
    jar tf "target/$artifact_name.jar" > /tmp/usercenter-jar-entries.txt
    grep -q '^BOOT-INF/classes/' /tmp/usercenter-jar-entries.txt
    grep -q '^BOOT-INF/lib/' /tmp/usercenter-jar-entries.txt

    # deploy-image 作为本地构建目录，应加入后端项目 .gitignore
    mkdir -p deploy-image
    cp "target/$artifact_name.jar" deploy-image/app.jar
    ```

    多模块项目要进入实际启动模块，或使用它的 `target` 路径；自定义 classifier 的项目先核对产物名称。不要用 `COPY target/*.jar`，因为一个目录中可能有多个 Jar。

3. Dockerfile

    保存到 `deploy-image/Dockerfile`：

    ```dockerfile
    # syntax=docker/dockerfile:1
    ARG RUNTIME_IMAGE
    FROM ${RUNTIME_IMAGE}

    USER root
    RUN apt-get update \
        && apt-get install -y --no-install-recommends curl ca-certificates \
        && rm -rf /var/lib/apt/lists/* \
        && groupadd --gid 10001 app \
        && useradd --uid 10001 --gid 10001 --no-create-home --shell /usr/sbin/nologin app

    WORKDIR /app
    COPY --chown=10001:10001 app.jar /app/app.jar
    USER 10001:10001
    EXPOSE 8080
    ENTRYPOINT ["java", "-jar", "/app/app.jar"]

    ```
    注释版
    ```dockerfile

    # 使用 Dockerfile v1 语法。
    # syntax=docker/dockerfile:1

    # 构建时传入 Java 运行时基础镜像。
    ARG RUNTIME_IMAGE

    # 使用指定的 JRE 镜像作为基础环境。
    FROM ${RUNTIME_IMAGE}

    # 临时使用 root，便于安装软件和创建用户。
    USER root

    # 安装 curl/CA 证书，并创建低权限 app 用户。
    RUN apt-get update \
        && apt-get install -y --no-install-recommends curl ca-certificates \
        && rm -rf /var/lib/apt/lists/* \
        && groupadd --gid 10001 app \
        && useradd --uid 10001 --gid 10001 --no-create-home --shell /usr/sbin/nologin app

    # 设置容器默认工作目录。
    WORKDIR /app

    # 将 Jar 复制进镜像，并交给 app 用户。
    COPY --chown=10001:10001 app.jar /app/app.jar

    # 使用普通用户运行 Spring Boot，避免 root 权限。
    USER 10001:10001

    # 声明应用预计监听 8080，实际端口发布仍由 Docker/Compose 决定。
    EXPOSE 8080

    # 容器启动时执行 java -jar。
    ENTRYPOINT ["java", "-jar", "/app/app.jar"]
    ```

    本例采用 Ubuntu Jammy 变体，因此可以用 `apt-get`，不要换成 Alpine 后仍照抄安装命令。容器运行时使用 JRE 即可；测试编译在构建机 JDK 完成。`curl` 为健康检查显式安装，不能假设所有 Java 镜像都自带。[Eclipse Temurin 官方镜像](https://hub.docker.com/_/eclipse-temurin)

    `EXPOSE` 是端口声明，不会公开宿主机端口。JSON 数组形式的 ENTRYPOINT 让 Java 正确接收停止信号，不用 `nohup`、后台 `&` 或无限 `tail` 来维持容器。[Dockerfile ENTRYPOINT](https://docs.docker.com/reference/dockerfile/#entrypoint)

    `deploy-image/.dockerignore`：

    ```text
    *
    !Dockerfile
    !.dockerignore
    !app.jar
    ```

4. 构建并导出镜像

    先在服务器执行 `uname -m`：`x86_64` 对应 `linux/amd64`，`aarch64` 对应 `linux/arm64`。下面开发机命令以 `linux/amd64` 为例；架构不同必须修改，特别留意 JNI 等原生依赖。

    ```bash
    cd deploy-image
    docker pull --platform linux/amd64 eclipse-temurin:17-jre-jammy
    runtime_image=$(docker image inspect eclipse-temurin:17-jre-jammy \
    --format '{{index .RepoDigests 0}}')
    printf '%s\n' "$runtime_image" > runtime-image.txt

    # v1.0.0 是演示发布号，已有同名发布时改用新版本，不覆盖
    docker build --platform linux/amd64 \
    --build-arg RUNTIME_IMAGE="$runtime_image" \
    -t usercenter-backend:v1.0.0 .

    docker image inspect usercenter-backend:v1.0.0 --format '{{.Id}}'
    docker save -o app-image.tar usercenter-backend:v1.0.0
    sha256sum app-image.tar > app-image.tar.sha256
    ```

    镜像 digest 固定基础镜像内容；构建时 apt 软件包源仍会变化，所以要保存本次最终镜像，发布或回滚时加载原镜像，不把“重新构建同版本”当成复现。镜像标签和构建记录一起保存；若以后使用私有镜像仓库，改用 Registry digest 部署即可。

    将 `app-image.tar`、`.sha256`、Dockerfile 和 `runtime-image.txt` 上传到服务器的版本目录。示例使用 SCP；替换服务器别名和用户名，并先按下一节创建上传目录：

    ```bash
    # 开发机执行；server-alias 为你已配置好的 SSH 主机别名
    scp app-image.tar app-image.tar.sha256 Dockerfile runtime-image.txt \
    server-alias:~/usercenter-upload/v1.0.0/
    ```

#### 五、服务器：配置 Docker


1. 云服务器目录结构总览

    ```text
    开发机：

    Spring Boot 项目/
    ├── pom.xml
    ├── mvnw
    ├── src/
    └── target/
        └── usercenter-backend-1.0.0.jar

    服务器：

    /opt/docker/usercenter-backend/
    ├── Dockerfile
    ├── .dockerignore
    ├── compose.yml
    ├── build-image.sh
    ├── deploy.sh
    ├── .env -> releases/v1.0.0.env
    ├── .build/                         # 临时 Docker 构建输入
    └── releases/
        ├── v1.0.0.env
        ├── v1.0.1.env
        └── ...

    /srv/usercenter-backend/
    ├── releases/
    │   ├── v1.0.0/
    │   │   └── usercenter-backend-1.0.0.jar
    │   ├── v1.0.1/
    │   │   └── usercenter-backend-1.0.1.jar
    │   └── ...
    └── shared/                         # 将来上传文件等持久化数据

    /opt/secrets/usercenter/
    └── DB_PASSWORD
    ```

1. 创建目录并核对前置条件

    ```bash
    # 1. 创建后端部署目录
    sudo install -d -m 0755 /opt/docker/usercenter-backend

    # 2. 创建当前发布版本的 Jar 归档目录
    sudo install -d -m 0755 /srv/usercenter-backend/releases/v1.0.0

    # 3. 可选：预留以后持久化上传文件等数据
    sudo install -d -m 0755 /srv/usercenter-backend/shared

    # 4. 确认 Nginx 和 MySQL 所需 Docker 网络已经存在
    sudo docker network inspect app-net
    sudo docker network inspect usercenter-db-net

    # 5. 确认 MySQL 当前正常运行
    sudo docker compose \
    --env-file /opt/docker/mysql/.env \
    -f /opt/docker/mysql/compose.yml \
    ps

    # 6. 确认后端使用的数据库密码 Secret 已存在且非空
    sudo test -s /opt/secrets/usercenter/DB_PASSWORD
    ```

2. 创建目录存储开发机生成的jar
    ```bash
    sudo install -D -m 0644 \
    usercenter-backend-1.0.2.jar \
    /srv/usercenter-backend/releases/v1.0.2/usercenter-backend-1.0.2.jar
    ```

4. 建议先建立脚本并修改权限
    ```bash
    sudo chmod 0755 \
    /opt/docker/usercenter-backend/build-image.sh \
    /opt/docker/usercenter-backend/deploy.sh
    ```

3. 通过镜像构建脚本完成构建
    
    ```bash
    cd /opt/docker/usercenter-backend

    sudo ./build-image.sh 1.0.0
    ```

    `.dockerignore`（只放行需要的文件）
    ```
    *
    !Dockerfile
    !.build/
    !.build/app.jar
    ```

    `Dockerfile`
    ```dockerfile
    ARG RUNTIME_IMAGE
    FROM ${RUNTIME_IMAGE}

    USER root
    RUN groupadd --gid 10001 app \
        && useradd --uid 10001 --gid 10001 \
        --no-create-home \
        --shell /usr/sbin/nologin app

    WORKDIR /app

    # build-image.sh 会提前把指定版本 Jar 准备到这里。
    COPY --chown=10001:10001 .build/app.jar /app/app.jar

    USER 10001:10001

    EXPOSE 8080

    ENTRYPOINT ["java", "-jar", "/app/app.jar"]
    ```
        
    脚本内容
    ```bash
    #!/usr/bin/env bash
    set -euo pipefail

    VERSION="${1:-}"
    VERSION="${VERSION#v}"

    if [ -z "$VERSION" ]; then
        echo "用法：./build-image.sh 1.0.0"
        exit 1
    fi

    # 正式发布 Jar
    JAR="/srv/usercenter-backend/releases/v${VERSION}/usercenter-backend-${VERSION}.jar"

    # Docker 临时构建输入
    BUILD_DIR="/opt/docker/usercenter-backend/.build"
    BUILD_JAR="${BUILD_DIR}/app.jar"

    # 当前版本的发布记录
    ENV_FILE="/opt/docker/usercenter-backend/releases/v${VERSION}.env"

    # 最终应用镜像
    IMAGE="usercenter-backend:v${VERSION}"

    # Java 基础镜像选择
    RUNTIME_TAG="eclipse-temurin:17-jre-jammy"


    # 检查对应版本 Jar 是否存在
    if [ ! -f "$JAR" ]; then
        echo "错误：找不到 Jar：$JAR"
        exit 1
    fi

    # 禁止覆盖已经建立发布记录的版本
    if [ -f "$ENV_FILE" ]; then
        echo "错误：版本 v${VERSION} 已存在：$ENV_FILE"
        exit 1
    fi

    # 禁止重新覆盖同名镜像版本
    if docker image inspect "$IMAGE" >/dev/null 2>&1; then
        echo "错误：镜像 $IMAGE 已存在"
        exit 1
    fi


    # 创建临时构建目录
    mkdir -p "$BUILD_DIR"

    # 无论脚本正常结束还是异常退出，都自动清理临时 Jar
    trap 'rm -f "$BUILD_JAR"' EXIT

    # 把指定版本 Jar 准备成 Dockerfile 固定使用的 app.jar
    cp "$JAR" "$BUILD_JAR"


    # 拉取 Java 17 JRE Jammy 基础镜像
    docker pull "$RUNTIME_TAG"

    # 取得本次真正使用的基础镜像 digest
    RUNTIME_IMAGE=$(docker image inspect "$RUNTIME_TAG" \
        --format '{{index .RepoDigests 0}}')

    echo "Jar：$JAR"
    echo "基础镜像：$RUNTIME_IMAGE"
    echo "目标镜像：$IMAGE"


    # 构建应用镜像
    docker build \
        --build-arg RUNTIME_IMAGE="$RUNTIME_IMAGE" \
        -t "$IMAGE" \
        /opt/docker/usercenter-backend


    # 获取构建完成后的本地镜像 ID
    IMAGE_ID=$(docker image inspect "$IMAGE" --format '{{.Id}}')


    # 创建版本记录目录
    mkdir -p /opt/docker/usercenter-backend/releases

    # 保存本次发布记录
    cat > "$ENV_FILE" <<EOF
    APP_VERSION=${VERSION}
    APP_IMAGE=${IMAGE}
    APP_IMAGE_ID=${IMAGE_ID}
    RUNTIME_IMAGE=${RUNTIME_IMAGE}
    EOF


    # 临时 Jar 已经进入镜像，不再需要
    rm -f "$BUILD_JAR"

    echo
    echo "构建完成"
    echo "版本：v${VERSION}"
    echo "镜像：${IMAGE}"
    echo "镜像 ID：${IMAGE_ID}"
    echo "版本记录：${ENV_FILE}"
    ```

4. 容器文件构建`compose.yml`
    ```yml
    # 固定 Compose 项目名，避免目录名变化影响容器/网络命名。
    name: usercenter-prod

    services:
      usercenter-backend:

        # 运行当前版本对应的本地镜像，APP_IMAGE 由 .env 提供。
        image: ${APP_IMAGE:?请先选择部署版本}

        # 只使用本地已构建镜像，不自动从远程仓库拉取。
        pull_policy: never

        # 容器异常退出或服务器重启后自动恢复。
        restart: unless-stopped

        # 启用轻量 init 进程，帮助正确处理信号和僵尸子进程。
        init: true

        # 强制使用低权限 UID/GID 10001 运行应用。
        user: "10001:10001"

        # 将容器根文件系统设为只读，减少运行时被修改的风险。
        read_only: true

        # 为应用提供可写的临时目录，数据只存在内存中。
        tmpfs:
          - /tmp:rw,noexec,nosuid,size=128m,mode=1777

        # 删除容器默认获得的 Linux capabilities。
        cap_drop:
          - ALL

        # 禁止容器内进程通过 setuid 等方式提升权限。
        security_opt:
          - no-new-privileges:true

        # 限制容器最大可用内存为 768MB。
        mem_limit: 768m

        # 停止容器时最多等待 60 秒，让 Spring Boot 完成优雅停机。
        stop_grace_period: 60s

        environment:
          # 激活 Jar 内部的 application-prod.yml。
          SPRING_PROFILES_ACTIVE: prod

          # 通过 Docker 内部网络访问 MySQL 服务。
          DB_URL: "jdbc:mysql://mysql:3306/usercenter_prod?sslMode=REQUIRED&connectionTimeZone=UTC"

          # Spring Boot 使用的数据库业务账号。
          DB_USERNAME: usercenter_app

          # 限制 JVM 内存占比，OOM 时退出，并统一使用 UTC 时区。
          JAVA_TOOL_OPTIONS: "-XX:MaxRAMPercentage=60.0 -XX:+ExitOnOutOfMemoryError -Duser.timezone=UTC"

        secrets:
          # 将数据库密码挂载到容器内 /run/secrets/DB_PASSWORD。
          - source: usercenter_db_password
            target: DB_PASSWORD

        logging:
          # 使用 Docker local 日志驱动并限制日志文件体积。
          driver: local
          options:
            max-size: "10m"
            max-file: "5"

        networks:
          # 连接 Nginx 所在网络。
          - app-net

          # 连接 MySQL 所在网络。
          - usercenter-db-net


    secrets:
      # 数据库密码来源于宿主机已有的 Secret 文件。
      usercenter_db_password:
        file: /opt/secrets/usercenter/DB_PASSWORD


    networks:
      # app-net 已由其他 Compose/部署步骤创建，这里只引用。
      app-net:
        external: true
        name: app-net

      # usercenter-db-net 已由 MySQL 部署创建，这里只引用。
      usercenter-db-net:
        external: true
        name: usercenter-db-net
    ```

5. 容器启动脚本

    ```bash
    sudo ./deploy.sh 1.0.0
    ```
    脚本内容
    ```bash
    #!/usr/bin/env bash
    set -euo pipefail

    VERSION="${1:-}"
    VERSION="${VERSION#v}"

    if [ -z "$VERSION" ]; then
        echo "用法：./deploy.sh 1.0.0"
        exit 1
    fi

    ENV_FILE="/opt/docker/usercenter-backend/releases/v${VERSION}.env"

    if [ ! -f "$ENV_FILE" ]; then
        echo "错误：找不到发布记录 $ENV_FILE"
        echo "请先执行：./build-image.sh ${VERSION}"
        exit 1
    fi

    # 读取目标镜像名称
    APP_IMAGE=$(grep '^APP_IMAGE=' "$ENV_FILE" | cut -d= -f2-)

    # 确保服务器中确实存在该镜像
    if ! docker image inspect "$APP_IMAGE" >/dev/null 2>&1; then
        echo "错误：本机不存在镜像 $APP_IMAGE"
        exit 1
    fi

    # 将 .env 指向目标版本
    ln -sfn "releases/v${VERSION}.env" .env

    echo "当前版本切换为：v${VERSION}"
    echo "镜像：$APP_IMAGE"

    # 按新 .env 重建容器
    docker compose up -d

    docker compose ps
    ```


#### 六、首次启动与联调

1. 启动后端

```bash
cd /opt/docker/usercenter-backend
sudo docker compose config -q
sudo docker compose up -d
sudo docker compose ps
sudo docker compose logs --tail=200 usercenter-backend

# 等启动完成再执行，期望 HTTP 200、JSON 中 status 为 UP
sudo docker compose exec -T usercenter-backend \
  curl -sS -i http://127.0.0.1:8080/api/actuator/health/readiness

backend_id=$(sudo docker compose ps -q usercenter-backend)
sudo docker inspect "$backend_id" --format '{{.State.Health.Status}}'
```

日志应明确激活 `prod`，不能仍显示 `dev` 或默认 Profile。端点返回 401/302，先查 Security 放行；返回 503，查数据库连接；返回 404，查 Actuator 依赖、context-path 与探针配置，不要通过移除健康检查掩盖问题。

2. 接入 Nginx

确认 Nginx 已采用 [修订示例](nginx-review.md) 中 `usercenter_api` upstream 和 `/api/` location，然后检查并 reload：

```bash
cd /opt/docker/nginx
sudo docker compose exec -T nginx nginx -t &&
sudo docker compose exec -T nginx nginx -s reload
```

如果仍沿用旧静态 upstream，后端重建改变 IP 后也需要 reload。动态 DNS 会更新地址，但 Nginx 并不自动读取 Compose 的 healthy 状态，单实例重建期间仍可能短暂出现 502。

3. 验证 Session 与 CSRF

原笔记明确给出 `GET /api/auth/csrf`，可先做只读自测：

```bash
curl -sS -D /tmp/usercenter-csrf.headers \
  -c /tmp/usercenter.cookies \
  https://dashboard.19271949.xyz/api/auth/csrf
```

此接口的预期是 HTTP 200 和 JSON，包括 CSRF token 及要提交的 headerName；具体外层包装按项目实现，不假定 `code` 固定为某值。Token 和 Cookie 属于会话信息，不贴进公开日志。

再在浏览器完成实际业务验收：

| 操作 | 期望结果 |
| --- | --- |
| 打开前端，获取 CSRF token | 请求走同源 `/api`，不再出现开发机 localhost 地址 |
| 使用测试账号登录 | 携带同一会话 Cookie 和正确 CSRF Header，登录成功 |
| 登录后获取当前用户 | 保持登录态，Cookie 具备 Secure、HttpOnly，域名和 Path 正确 |
| 登录／退出后再次获取 CSRF | 依据服务端令牌生命周期刷新，不能永久复用旧 token |
| 普通用户访问管理员接口 | 返回预期 403 JSON |
| 退出后访问受保护接口 | 返回预期 401 JSON |
| 不存在的 API | 不应以 HTTP 200 返回前端 `index.html` |
| 浏览器刷新深层前端页面 | SPA 页面正常恢复，资源无错误缓存 |

不要因为生产环境出现 403 就关闭 CSRF，也不要配置“任意 Origin + 携带凭据”的跨域。此方案前后端走同一域名，无需为了基本登录另外开放跨域。

#### 七、更新、回滚和日常维护

1. 发布新版本

  1. 构建并验证 `v1.0.1` 镜像，上传到新的版本目录，校验 SHA-256 后 `docker load`。
  2. 为新版本保存配套 `config/application-prod.yml`，审查配置差异；真实密码仍只来自 secret。
  3. 若有数据库结构变更，先备份并验证兼容迁移方案。运行账号只有业务读写权限，建表/改表用迁移账号或管理员执行。
  4. 记录旧的 `.env` 和镜像 ID，将 `APP_VERSION`、`APP_IMAGE` **一起**切到 `v1.0.1`。
  5. 执行以下命令，然后重新做健康检查和登录联调。

  ```bash
  cd /opt/docker/usercenter-backend
  sudo docker compose config -q
  sudo docker compose up -d --no-deps usercenter-backend
  sudo docker compose logs --tail=200 usercenter-backend
  ```

  `docker compose restart` 不会应用新镜像、环境变量或挂载路径。单实例更新会有短暂停机；不要把 `restart: unless-stopped` 称为高可用。[Compose restart](https://docs.docker.com/reference/cli/docker/compose/restart/)

  只修改当前版本内的挂载文件时，应记录这次配置变更，并重建容器以重新读取配置与 secrets：

  ```bash
  cd /opt/docker/usercenter-backend
  sudo docker compose up -d --no-deps --force-recreate usercenter-backend
  ```

2. 回滚应用

先确认当前数据库结构仍兼容旧代码。保留旧镜像与旧版本配置，将 `.env` 恢复为：

```dotenv
APP_VERSION=v1.0.0
APP_IMAGE=usercenter-backend:v1.0.0
```

再执行 `config -q`、`up -d --no-deps usercenter-backend`，验证健康与登录流程。没有旧镜像时从保存的旧 `app-image.tar` 加载；不要临时重新构建一个同名旧标签冒充原发布。

回滚 Jar／镜像不会回滚数据库。删除列、重写数据等不可逆迁移需要独立的数据恢复决策；不要为了回退代码自动用旧备份覆盖现有生产数据。

3. 停机、日志和 Session 边界

```bash
cd /opt/docker/usercenter-backend
sudo docker compose logs --tail=200 -f usercenter-backend
sudo docker compose stop usercenter-backend
# 需要恢复服务时
sudo docker compose up -d
```

停止时 Docker 发出终止信号，Spring Boot 按配置进行 graceful shutdown。本例每个 Spring 生命周期停机阶段允许 30 秒，Docker 总等待 60 秒；有多个耗时阶段时需增加并实测。[Spring Boot 优雅停机](https://docs.spring.io/spring-boot/3.5/reference/web/graceful-shutdown.html)

默认内存 Session 会在重启／更新时丢失，用户需要重新登录，多副本也不能自动共享会话。以后需要保留登录态或扩容，再采用 Spring Session + Redis/JDBC 等共享会话方案，并验证序列化与版本兼容；不要先把单实例直接扩成多副本。[Spring Session](https://docs.spring.io/spring-session/reference/)

#### 八、常见问题

| 现象 | 原因与处理 |
| --- | --- |
| `UnsupportedClassVersionError` | Jar 编译版本高于运行 JRE，核对实际字节码和镜像 |
| `no main manifest attribute` | 普通 Jar 或错误产物，检查 Boot repackage |
| 找不到 `DB_PASSWORD` | configtree 导入、secret 文件名、读取权限、prod 激活有误 |
| MySQL `Access denied` | 密码／账号／host 不一致；改 secret 不会自动修改数据库密码 |
| `Communications link failure` | 数据库未就绪、服务名/网络错误、TLS配置或连接被中断 |
| `Public Key Retrieval is not allowed` | 优先检查 JDBC TLS 是否实际建立和驱动兼容性，不通用关闭 SSL |
| 健康检查 404 | Actuator 不在包内、路径前缀或版本配置不匹配 |
| 健康检查 401／302 | 探针被 Security 或自定义 Filter 拦截 |
| 容器 running 但 unhealthy | 看 health 日志；普通 Compose 不会仅因此自动重启 |
| 容器退出 137／OOMKilled | 查看 `docker inspect` 与宿主机内存，减少内存占用或增加容量 |
| 登录成功后仍未登录 | Cookie 未保存／发送、HTTPS转发头不正确、CSRF流程或Session丢失 |
| 只读文件系统错误 | 应用在写容器根目录；配置专用可写持久化目录 |
| 后端重建后 Nginx 502 | 确认动态 DNS配置、网络和实际8080监听，再做请求验证 |

本文是部署操作教程；本次未取得实际后端源码、Jar、生产服务器或数据库，不能宣称以上配置已完成真实运行验收。



### 【腾讯云】MySQL部署与规范

本篇接在 Nginx 部署之后，目标是在同一台腾讯云 Linux 服务器上运行用户中心数据库，并给 Spring Boot 提供稳定的内部连接地址。命令面向 **Ubuntu/Debian + Bash + Docker Engine + Compose v2**；先核对 `docker version`、`docker compose version`。本文是部署教程，写入本文不代表服务器已经执行了这些命令。

示例选择官方 `mysql:8.4` LTS 系列，生产部署时固定实际拉取到的 digest。8.4 是本教程选定的兼容系列，不能把 `latest` 或会随发行线变化的 `lts` 标签当成固定版本。[MySQL 发行模型](https://dev.mysql.com/doc/refman/8.4/en/mysql-releases.html)、[官方镜像支持标签](https://hub.docker.com/_/mysql)

#### 1. 先明确目录、账号与连接关系

```text
/opt/docker/mysql/
├── compose.yml                 # MySQL 服务、挂载、健康检查
├── .env                        # 只有 MYSQL_IMAGE，不放密码
├── image-record.txt            # 拉取时间、digest、MySQL 版本
├── schema.sql                  # 审查后的建表 SQL，首次导入时准备
├── backup.sh                   # 逻辑备份脚本
└── restore-check.sh            # 隔离恢复演练脚本

/opt/secrets/                    # root:root，0700
├── mysql/                      # root:root，0700
│   └── MYSQL_ROOT_PASSWORD     # root:root，0444
└── usercenter/                 # root:root，0700
    └── DB_PASSWORD             # root:root，0444；MySQL 与后端共享

/srv/mysql/
├── data/                       # MySQL 实际数据，挂到 /var/lib/mysql
└── backups/                    # SQL 压缩备份、校验文件，root 专用
```

| 对象 | 职责 | 不应该放什么 |
| --- | --- | --- |
| `/opt/docker/mysql` | 管理服务配置和维护脚本 | 明文账号密码、实际数据库文件 |
| `/srv/mysql/data` | 保存表、索引、系统表、redo/binlog 等运行数据 | 下载的 SQL、手工备份、其他应用文件 |
| `/srv/mysql/backups` | 保存可校验、可恢复的逻辑备份 | 唯一一份灾难恢复副本 |
| `root@localhost` | 容器内管理、导入结构、授权、维护 | 应用连接池凭据 |
| `usercenter_app@%` | 后端访问 `usercenter_prod` | 全局权限、授权权限、日常建表权限 |
| `usercenter-db-net` | 只有数据库和需要访问它的后端加入 | Nginx、无关测试容器 |

连接链路如下：

```text
Nginx ── app-net ── Spring Boot
                        │
               usercenter-db-net（internal）
                        │
                    mysql:3306
                        │
                 usercenter_prod
```

后端连接 `mysql:3306/usercenter_prod`。这里的 `mysql` 是 Docker 网络中的服务名，不是公网域名；后端容器里的 `localhost` 指后端自己。MySQL 不写 `ports:`，腾讯云安全组也不开放 3306/33060。服务间通过自定义网络直接访问，不需要 `expose:` 才能通信。

#### 2. 建目录、生成密码并固定镜像

1. **在服务器上进入管理员 Bash。** 后面的服务器命令默认在这个会话中执行，脚本则通过 `bash 文件名` 执行。
    > **注意，整个部署周期不建议退出ROOT**

    ```bash
    sudo -i
    bash
    set -euo pipefail
    umask 077

    docker version
    docker compose version
    openssl version

    install -d -o root -g root -m 0750 /opt/docker/mysql /srv/mysql
    install -d -o root -g root -m 0700 /srv/mysql/data /srv/mysql/backups
    install -d -o root -g root -m 0700 \
    /opt/secrets /opt/secrets/mysql /opt/secrets/usercenter

    # 只检查；已有数据时不要继续按“全新实例”初始化。
    find /srv/mysql/data -mindepth 1 -maxdepth 1 -printf '%f\n'
    ```

    全新实例的 `data` 应为空。如果输出已有 `mysql`、`ibdata1` 等文件，先确定其来源和版本，按已有实例接管或迁移。不要靠清空 `data` 解决密码错误或初始化失败。

1. **仅当密码文件不存在时生成密码。** 以下命令不会打印密码，也不会覆盖已有密码：

    >这里创建了两个文件，用于存储数据库ROOT用户和`usercenter-app`用户

    ```bash
    for secret_file in \
    /opt/secrets/mysql/MYSQL_ROOT_PASSWORD \
    /opt/secrets/usercenter/DB_PASSWORD
    do
    if [ -e "$secret_file" ]; then
        test -f "$secret_file" && test -s "$secret_file"
        printf '保留已有密码文件：%s\n' "$secret_file"
    else
        secret_value=$(openssl rand -hex 32)
        printf '%s' "$secret_value" > "$secret_file"
        unset secret_value
    fi
    chown root:root "$secret_file"
    chmod 0444 "$secret_file"
    done

    stat -c '%a %U:%G %n' \
    /opt/secrets /opt/secrets/mysql /opt/secrets/usercenter \
    /opt/secrets/mysql/MYSQL_ROOT_PASSWORD /opt/secrets/usercenter/DB_PASSWORD
    ```

    密码由 32 个随机字节生成，写成没有末尾换行的 64 位十六进制文本，便于 MySQL 和 Spring Boot 读取同一个值。不要把文件内容复制进 Git、截图或 `.env`。已有密码文件若含末尾换行，先确认数据库实际密码、文件内容和后端读取方式；确认换行只是文件格式后，再受控规范化并重建读取它的容器。不要盲目去掉密码本身的空白，也不要把文件规范化误当成数据库改密。

    为什么文件是 `0444`，父目录却是 `0700`？普通宿主机用户不能穿过 root 专有目录读取文件；文件单独挂进容器后，MySQL 用户和后端的非 root 用户可以读到它。本例针对普通 rootful Docker；已有 ACL、rootless Docker 或用户命名空间映射需要另外验证读取权限。不要用 `chmod -R 777` 修复。

    本地 Compose 的文件型 secrets 是只读 bind mount，**不是加密的秘密管理服务**，其 `uid/gid/mode` 不能用来重写宿主文件权限，所以权限在宿主机设置。宿主 root、Docker 管理员及获得该 secret 的容器仍可读取内容。[Compose secrets 使用说明](https://docs.docker.com/compose/how-tos/use-secrets/)、[文件型 secret 的权限限制](https://docs.docker.com/reference/compose-file/services/#secrets)


3. **拉取选定系列并记录实际镜像。** 首次部署执行以下命令；以后不要自动覆盖 `.env` 触发数据库升级。

    ```bash
    cd /opt/docker/mysql
    test ! -e .env || { echo '.env 已存在，请先检查当前版本记录。' >&2; exit 1; }

    docker pull mysql:8.4
    mysql_image_ref=$(docker image inspect mysql:8.4 --format '{{index .RepoDigests 0}}')
    case "$mysql_image_ref" in
    mysql@sha256:*|docker.io/library/mysql@sha256:*) ;;
    *) echo '没有取得可识别的官方镜像 digest，停止。' >&2; exit 1 ;;
    esac

    printf 'MYSQL_IMAGE=%s\n' "$mysql_image_ref" > .env
    {
    date -Is
    printf 'source_tag=mysql:8.4\nimage=%s\n' "$mysql_image_ref"
    docker run --rm --network none "$mysql_image_ref" mysqld --version
    } > image-record.txt
    chmod 0640 .env image-record.txt
    cat image-record.txt
    ```

    标签可能指向新镜像，digest 指向具体内容。实际补丁号由输出确定，不照抄笔记里某个“最新版本号”。正式上线前，还要用该镜像验证后端驱动、SQL 和备份恢复流程。[按 digest 拉取镜像](https://docs.docker.com/reference/cli/docker/image/pull/#pull-an-image-by-digest-immutable-identifier)

#### 3. 创建并核对数据库内部网络

```bash
if docker network inspect usercenter-db-net >/dev/null 2>&1; then
  network_properties=$(docker network inspect usercenter-db-net \
    --format '{{.Driver}} {{.Internal}}')
  if [ "$network_properties" != 'bridge true' ]; then
    printf '已有网络属性不符合要求：%s；先安排迁移，不要直接删除在用网络。\n' \
      "$network_properties" >&2
    exit 1
  fi
else
  docker network create --driver bridge --internal usercenter-db-net
fi

docker network inspect usercenter-db-net \
  --format 'driver={{.Driver}} internal={{.Internal}} containers={{json .Containers}}'
```

`internal` 限制该网络与其他网络之间的通信；它不阻止宿主机访问容器，也不能防止有 Docker 管理权限的人加入网络。后端同时加入 `app-net` 和数据库网络`usercenter-db-net`，Nginx 只加入前者。不要给 MySQL 再挂普通 bridge 网络。[Docker internal 网络行为](https://docs.docker.com/reference/cli/docker/network/create/#network-internal-mode---internal)

#### 4. 编写 Compose

保存为 `/opt/docker/mysql/compose.yml`：

```yaml
name: usercenter-mysql

services:
  mysql:
    image: ${MYSQL_IMAGE:?请先在.env填写已核实的镜像digest}
    restart: unless-stopped
    stop_grace_period: 1m
    environment:
      MYSQL_ROOT_HOST: localhost
      MYSQL_ROOT_PASSWORD_FILE: /run/secrets/MYSQL_ROOT_PASSWORD
      MYSQL_DATABASE: usercenter_prod
      MYSQL_USER: usercenter_app
      MYSQL_PASSWORD_FILE: /run/secrets/DB_PASSWORD
    
    # 这里取代了ini文件
    command:
      - --character-set-server=utf8mb4
      - --collation-server=utf8mb4_0900_ai_ci
      - --default-time-zone=+00:00
      - --require-secure-transport=ON
      - --binlog-expire-logs-seconds=604800
    volumes:
      - type: bind
        source: /srv/mysql/data
        target: /var/lib/mysql
        bind:
          create_host_path: false
    secrets:
      - source: mysql_root_password
        target: MYSQL_ROOT_PASSWORD
      - source: usercenter_db_password
        target: DB_PASSWORD
    networks:
      - usercenter-db-net
    healthcheck:
      test:
        - CMD-SHELL
        - >-
          MYSQL_PWD="$$(cat /run/secrets/DB_PASSWORD)"
          mysql --protocol=TCP --host=127.0.0.1 --port=3306
          --user=usercenter_app --database=usercenter_prod
          --ssl-mode=REQUIRED --connect-timeout=5
          --batch --skip-column-names --execute='SELECT 1' >/dev/null
      interval: 15s
      timeout: 8s
      retries: 10
      start_period: 90s
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"

networks:
  usercenter-db-net:
    external: true
    name: usercenter-db-net

secrets:
  mysql_root_password:
    file: /opt/secrets/mysql/MYSQL_ROOT_PASSWORD
  usercenter_db_password:
    file: /opt/secrets/usercenter/DB_PASSWORD
```

关键点：

1. **只有数据目录持久化。** 官方入口会在默认 root 启动阶段整理目录属主，再切换到 mysql 用户。不要盲目固定 `user: 999:999`；不同镜像的 UID 未必相同。
2. **root 明确限制为 `localhost`。** 当前官方 8.4 入口源码的 `MYSQL_ROOT_HOST` 默认值为 `%`，因此这里显式覆盖。已有数据库不会因此自动删除远程 root，应先查询账号再处理。[官方 8.4 入口脚本](https://github.com/docker-library/mysql/blob/master/8.4/docker-entrypoint.sh)
3. **密码配置只包含文件路径。** `_FILE` 由官方入口读取；不要同时写 `MYSQL_PASSWORD` 和 `MYSQL_PASSWORD_FILE`。入口实现会把读到的值放入进程环境，因此这并不保证运行时进程环境里绝无密码；它避免把密码写进 Compose、镜像配置和命令行参数。
4. **`$$` 留给容器里的 shell。** 单个 `$` 可能被 Compose 提前解析。健康检查临时设置 `MYSQL_PWD`，使这一条客户端命令真正认证并查询；该进程存活时，具备权限的进程仍可能查看它的环境。不要打开 `set -x` 或输出秘密文件。
5. **健康检查走 TCP 并执行 `SELECT 1`。** 初始化阶段的临时 mysqld 禁用 TCP，不能过早误报就绪；单独使用 `mysqladmin ping` 即使遇到拒绝认证也可能返回成功。[mysqladmin ping 的退出状态](https://dev.mysql.com/doc/refman/8.4/en/mysqladmin.html)
6. **连接要求加密。** `require_secure_transport=ON` 允许本机 Unix socket 管理，TCP 客户端必须使用 TLS。7 天 binlog 保留只是一项容量示例，需结合备份间隔、复制和恢复目标调整；Docker 日志轮转不会清理 binlog。[MySQL 加密连接配置](https://dev.mysql.com/doc/refman/8.4/en/using-encrypted-connections.html)

注释版
```yml
# Compose 项目名。
# docker compose ps 等命令里会看到这个项目名。
name: usercenter-mysql

services:

  # 定义一个名为 mysql 的服务。
  # 在同一个 Docker 网络里，其他容器可以通过主机名 mysql 找到它。
  mysql:

    # 从当前目录的 .env 文件中读取 MYSQL_IMAGE。
    #
    # 例如 .env：
    # MYSQL_IMAGE=docker.io/library/mysql@sha256:xxxx
    #
    # :?MYSQL_IMAGE is required 的意思是：
    # 如果 MYSQL_IMAGE 没有定义或者为空，Compose 直接报错，不继续启动。
    image: ${MYSQL_IMAGE:?MYSQL_IMAGE is required}

    # 容器异常退出、Docker 重启、服务器重启时自动拉起。
    # 但如果你手工 docker stop，它不会马上自己启动。
    restart: unless-stopped

    # 停止 MySQL 时，给它最多 1 分钟进行优雅关闭。
    # 避免直接杀进程导致正在刷盘的数据来不及处理。
    stop_grace_period: 1m


    # =========================================================
    # MySQL 官方镜像的初始化参数
    # =========================================================
    environment:

      # root 用户仅允许 localhost 来源。
      #
      # 这里的 localhost 指 MySQL 容器内部。
      # 也就是 root 主要用于容器内部管理，不给其他容器拿 root 连。
      MYSQL_ROOT_HOST: localhost

      # root 密码不直接写在 compose.yml 中。
      #
      # MySQL 官方 entrypoint 会读取：
      # /run/secrets/MYSQL_ROOT_PASSWORD
      #
      # 这个文件下面会通过 secrets 挂进去。
      MYSQL_ROOT_PASSWORD_FILE: /run/secrets/MYSQL_ROOT_PASSWORD

      # 第一次初始化全新数据库时自动创建这个数据库。
      MYSQL_DATABASE: usercenter_prod

      # 第一次初始化时自动创建业务用户。
      MYSQL_USER: usercenter_app

      # usercenter_app 的密码同样从文件读取。
      MYSQL_PASSWORD_FILE: /run/secrets/DB_PASSWORD


    # =========================================================
    # 传给 mysqld 的启动参数
    # =========================================================
    command:

      # 默认字符集使用 utf8mb4。
      # 支持完整 UTF-8，包括中文、emoji 等。
      - --character-set-server=utf8mb4

      # 默认排序规则。
      # ai = accent insensitive，通常表示不区分重音
      # ci = case insensitive，不区分大小写
      - --collation-server=utf8mb4_0900_ai_ci

      # MySQL 服务器内部默认时区使用 UTC。
      # 应用层需要北京时间/东京时间时再转换。
      - --default-time-zone=+00:00

      # 要求 TCP 连接使用安全传输。
      # 也就是 TCP 客户端必须使用 TLS。
      #
      # Unix Socket 本地管理不受这个限制。
      - --require-secure-transport=ON

      # binlog 最多保留 604800 秒。
      # 604800 秒 = 7 天。
      #
      # binlog 用于复制、时间点恢复等。
      - --binlog-expire-logs-seconds=604800


    # =========================================================
    # 数据持久化
    # =========================================================
    volumes:

      - type: bind

        # 宿主机真实目录。
        source: /srv/mysql/data

        # MySQL 容器默认的数据目录。
        target: /var/lib/mysql

        bind:
          # 如果宿主机 source 不存在，不允许 Docker 偷偷创建。
          # 直接失败，让管理员发现目录配置问题。
          create_host_path: false


    # =========================================================
    # 把密码文件挂进这个容器
    # =========================================================
    secrets:

      # 使用下面顶层定义的 mysql_root_password
      - source: mysql_root_password

        # 最终容器内部形成：
        # /run/secrets/MYSQL_ROOT_PASSWORD
        target: MYSQL_ROOT_PASSWORD

      # 使用下面顶层定义的 usercenter_db_password
      - source: usercenter_db_password

        # 最终容器内部形成：
        # /run/secrets/DB_PASSWORD
        target: DB_PASSWORD


    # =========================================================
    # Docker 网络
    # =========================================================
    networks:

      # MySQL 只加入数据库内部网络。
      - usercenter-db-net


    # =========================================================
    # 健康检查
    # =========================================================
    healthcheck:

      test:

        # 使用容器内部 shell 执行下一整条命令。
        - CMD-SHELL

        # >- 是 YAML 的多行字符串写法。
        # 实际上下面这些行最终会拼成一条 shell 命令。
        - >-

          # 从 secret 文件读取业务用户密码，
          # 临时放到 MYSQL_PWD 环境变量里。
          #
          # 注意这里是 $$，不是 $。
          # $$ 是为了让 Compose 不提前解析，
          # 把 $ 留给容器里的 shell。
          MYSQL_PWD="$$(cat /run/secrets/DB_PASSWORD)"

          # 使用 TCP 连接当前 MySQL。
          #
          # 这里 127.0.0.1 是 MySQL 容器自己，
          # 因为 healthcheck 就是在 MySQL 容器内部执行。
          mysql --protocol=TCP --host=127.0.0.1 --port=3306

          # 使用业务账号和业务数据库。
          --user=usercenter_app --database=usercenter_prod

          # 必须使用 TLS。
          # 最多等 5 秒建立连接。
          --ssl-mode=REQUIRED --connect-timeout=5

          # 非交互模式执行：
          # SELECT 1
          #
          # 如果连接、认证、数据库或 TLS 有问题，
          # 这里就失败。
          --batch --skip-column-names --execute='SELECT 1' >/dev/null

      # 每 15 秒检查一次。
      interval: 15s

      # 单次健康检查超过 8 秒视为失败。
      timeout: 8s

      # 连续失败 10 次后判为 unhealthy。
      retries: 10

      # 容器刚启动后的前 90 秒属于初始化宽限期。
      # MySQL 第一次初始化可能比较慢。
      start_period: 90s


    # =========================================================
    # Docker 容器日志
    # =========================================================
    logging:

      # 使用 Docker 默认常见的 json-file 日志驱动。
      driver: json-file

      options:

        # 单个日志文件最大 10MB。
        max-size: "10m"

        # 最多保留 3 个轮转文件。
        max-file: "3"


# =============================================================
# 顶层网络声明
# =============================================================
networks:

  usercenter-db-net:

    # 表示这个网络不是由本 compose.yml 创建。
    # 它必须提前存在。
    external: true

    # 实际 Docker 网络名。
    name: usercenter-db-net


# =============================================================
# 顶层 secrets 声明
# =============================================================
secrets:

  # 逻辑 secret 名。
  mysql_root_password:

    # 宿主机真实密码文件。
    file: /opt/secrets/mysql/MYSQL_ROOT_PASSWORD


  # 业务数据库密码。
  usercenter_db_password:

    # 宿主机真实密码文件。
    file: /opt/secrets/usercenter/DB_PASSWORD
```

#### 5. 启动与首次初始化验收

**1）先检查配置，再启动。**

```bash
cd /opt/docker/mysql
docker compose config --quiet
docker compose up -d --wait --wait-timeout 300
docker compose ps
docker compose logs --tail=100 mysql
```

等待超时就检查日志，不要继续启动后端。`--wait` 检查健康状态；服务器磁盘很慢时，查明原因后再调整等待时间。

官方镜像只会对全新数据目录自动建立库、账号，以及运行 `/docker-entrypoint-initdb.d` 中的初始化脚本；**修改环境变量或 secret 文件，不会修改已有数据库的账号密码、库名或表结构**。初始化中断可能留下部分数据，下次也不一定自动重跑完整流程，应先保留现场、查看失败步骤。[官方镜像初始化约定](https://hub.docker.com/_/mysql#initializing-a-fresh-instance)

**2）确认端口、网络和 SQL 连接。**

```bash
#找到 mysql 服务对应容器的 ID，然后保存进变量
mysql_container_id=$(docker compose ps -q mysql)
test -n "$mysql_container_id"
docker port "$mysql_container_id"
docker inspect "$mysql_container_id" --format '{{json .NetworkSettings.Networks}}'
docker inspect "$mysql_container_id" --format '{{.State.Health.Status}}'

docker compose exec -T mysql sh -ec '
  MYSQL_PWD="$(cat /run/secrets/DB_PASSWORD)" \
  mysql --protocol=TCP -h127.0.0.1 -P3306 \
    -uusercenter_app --ssl-mode=REQUIRED usercenter_prod \
    --execute="SELECT DATABASE(), CURRENT_USER(), VERSION(); SHOW SESSION STATUS LIKE '\''Ssl_cipher'\'';"
'
```

`docker port` 应没有输出；网络列表只有 `usercenter-db-net`；健康状态为 `healthy`；查询显示库名 `usercenter_prod`，`Ssl_cipher` 有非空值。`docker ps` 的 `3306/tcp` 只表示镜像声明的容器端口，`0.0.0.0:3306->3306/tcp` 才表示宿主机发布。

**3）进入管理员会话查看账号。**

```bash
# 方式1：手动输入 root 密码
docker compose exec \
  -e MYSQL_HISTFILE=/dev/null \
  mysql \
  mysql --protocol=SOCKET -uroot -p


# 方式2：自动从 secret 读取 root 密码
docker compose exec -it \
  -e MYSQL_HISTFILE=/dev/null \
  mysql \
  sh -c '
    MYSQL_PWD="$(cat /run/secrets/MYSQL_ROOT_PASSWORD)" \
    exec mysql --protocol=SOCKET -uroot
  '
```

在密码提示处输入密码管理器中保存的 root 密码；若使用上面的随机生成方式，管理员可通过受控方式读取密码文件。不要使用 `-p明文密码`。进入后执行：

```sql
SELECT User, Host, plugin FROM mysql.user ORDER BY User, Host;
SHOW GRANTS FOR 'usercenter_app'@'%';
SHOW VARIABLES LIKE 'require_secure_transport';
SHOW VARIABLES LIKE 'character_set_server';
SHOW VARIABLES LIKE 'collation_server';
EXIT;
```

新实例应只有本地 root。应用账号默认使用 `caching_sha2_password`；MySQL 8.4 的 `mysql_native_password` 默认禁用，不要照搬旧教程加 `--default-authentication-plugin=mysql_native_password` 或切换旧插件。[MySQL 8.4 认证插件说明](https://dev.mysql.com/doc/refman/8.4/en/native-pluggable-authentication.html)

#### 6. 导入表结构，并把应用账号权限收窄

**1）准备项目真实 SQL。** 从后端仓库复制经过检查的建表脚本到 `/opt/docker/mysql/schema.sql`。确认使用 `usercenter_prod`、`utf8mb4`、InnoDB，确认没有误带开发密码、`DROP DATABASE`、其他库的 `USE` 或开发测试数据。本文没有假造用户表结构。

用本地管理账号导入；不把建表权限留给业务连接池：

```bash
cd /opt/docker/mysql
test -s schema.sql
chmod 0600 schema.sql

docker compose exec -T mysql sh -ec '
  MYSQL_PWD="$(cat /run/secrets/MYSQL_ROOT_PASSWORD)" \
  mysql --protocol=SOCKET --user=root \
    --default-character-set=utf8mb4 --database=usercenter_prod
' < schema.sql
```
在此处修改
```bash
cd /opt/docker/mysql/sql
test -s 20260908_001_create_user_account.sql
chmod 0600 20260908_001_create_user_account.sql

docker compose exec -T mysql sh -ec '
  MYSQL_PWD="$(cat /run/secrets/MYSQL_ROOT_PASSWORD)" \
  mysql --protocol=SOCKET --user=root \
    --default-character-set=utf8mb4 --database=usercenter_prod
' < 20260908_001_create_user_account.sql
```

**通过数据库ROOT修改权限授权超级管理员**

```sql
START TRANSACTION;

SELECT id, user_account, user_role, status, deleted_at
FROM usercenter_prod.user_account
WHERE user_account = 'AdminProPlusMax'
FOR UPDATE;

UPDATE usercenter_prod.user_account
SET user_role = 15
WHERE user_account = 'AdminProPlusMax'
  AND status = 0
  AND deleted_at IS NULL;

SELECT ROW_COUNT();

COMMIT;
```

建议以下列形式维护建表文件
```text
/opt/docker/mysql/sql/
├── 20260908_001_create_user_account.sql
├── 20260908_002_create_user_profile.sql
├── 20260908_003_seed_super_admin.sql
├── 20260915_001_add_user_email_index.sql
└── 20261002_001_add_login_log_table.sql
```

命令以非零状态退出时先定位 SQL 错误，不要加 `--force` 跳过。DDL 可能已经部分生效，不能假设整份 SQL 会自动回滚。

**2）在上节的管理员 SQL 会话执行以下授权。** 镜像自动创建的应用账号起初拥有本库 `ALL`，其中包含建表、删表等权限；完成初始化后再开放后端访问。

```sql
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'usercenter_app'@'%';
GRANT SELECT, INSERT, UPDATE, DELETE
  ON `usercenter\_prod`.* TO 'usercenter_app'@'%';
SHOW GRANTS FOR 'usercenter_app'@'%';
```

这是常规读写账号；若应用只读，把上面的 `GRANT` 改为只授予 `SELECT`，两种方案按实际业务选一。不要继续执行一个更宽的 `GRANT ALL`。对本例默认的 `partial_revokes=OFF`，数据库级授权中的 `_` 有通配含义，因此用 `\_` 精确匹配下划线；实际数据库仍叫 `usercenter_prod`，JDBC URL 和 `USE` 不加反斜杠。若接管实例启用了 `partial_revokes`，先按该配置核对授权语义。[GRANT 数据库级权限](https://dev.mysql.com/doc/refman/8.4/en/grant.html)、[REVOKE 语法](https://dev.mysql.com/doc/refman/8.4/en/revoke.html)

`REVOKE/GRANT` 会直接生效，不需要另跑 `FLUSH PRIVILEGES`。`@'%'` 允许匹配来源主机，但不等于已开放公网；其边界还依靠无端口发布、私有网络和数据库权限。迁移工具如 Flyway/Liquibase 应在独立管理流程运行；不要为了让应用启动成功恢复业务账号的 DDL 权限。JPA 也不要在生产设置 `ddl-auto=create/update`。

#### 7. 与 Spring Boot 对接及 TLS 说明

| 后端项 | 本套笔记的值 |
| --- | --- |
| 数据源主机/端口 | `mysql:3306` |
| 数据库 | `usercenter_prod` |
| 用户名 | `usercenter_app` |
| 容器内密码文件 | `/run/secrets/DB_PASSWORD` |
| 密码来源 | 与 MySQL 相同的 `/opt/secrets/usercenter/DB_PASSWORD` |
| 网络 | 后端加入外部网络 `usercenter-db-net` |

后端通过 `configtree:/run/secrets/` 读取密码，完整配置见 [Spring Boot 部署](springboot.md)。先检查项目使用的 Connector/J 版本是否支持选定 MySQL 版本。

同机受控内部网络的入门配置可以使用 `sslMode=REQUIRED` 保证加密；它**不验证服务端身份**。严格生产配置应使用 `sslMode=VERIFY_IDENTITY`，为 MySQL 配置受信任 CA 签发且 SAN 包含连接名 `mysql` 的服务端证书，并把 CA 导入 Java 信任库。MySQL 自动生成的证书不一定符合该主机名校验要求，不能直接把 URL 改成 `VERIFY_IDENTITY` 就宣称配置完成。[Connector/J TLS 模式](https://dev.mysql.com/doc/connector-j/en/connector-j-connp-props-security.html)、[Java 信任库配置](https://dev.mysql.com/doc/connector-j/en/connector-j-server-authentication.html)

出现 `Public Key Retrieval is not allowed` 时，核对驱动版本、是否建立 TLS、服务端认证插件和证书信任；不要把 `useSSL=false&allowPublicKeyRetrieval=true` 当成通用修复。本教程要求的 TLS 连接不需要靠关闭 TLS 解决 RSA 认证问题。

#### 8. 备份：先保证失败可发现，再谈定时执行

**1）明确备份范围。** 下面备份 `usercenter_prod` 的表结构、数据、触发器、存储过程和事件，不包含 MySQL 系统用户及授权。secret、Compose、授权记录与镜像版本另行受控保存。演练前先确认表引擎：

```sql
SELECT TABLE_NAME, ENGINE
FROM information_schema.TABLES
WHERE TABLE_SCHEMA = 'usercenter_prod' AND TABLE_TYPE = 'BASE TABLE';
```

`--single-transaction` 对 InnoDB 提供一致性快照；备份期间不要做 `ALTER/CREATE/DROP/RENAME/TRUNCATE TABLE`。若有 MyISAM 等非事务表，需要停写或设计另外的锁定策略。定期完整备份只能恢复到备份时刻；要求更小的数据丢失窗口时，还要安排 binlog 归档及时间点恢复。[mysqldump 事务与权限说明](https://dev.mysql.com/doc/refman/8.4/en/mysqldump.html)、[备份与恢复策略](https://dev.mysql.com/doc/refman/8.4/en/backup-and-recovery.html)

**2）保存为 `/opt/docker/mysql/backup.sh`。**

```bash
#!/usr/bin/env bash
set -euo pipefail
umask 077

cd /opt/docker/mysql
exec 9>/srv/mysql/backups/.backup.lock
flock -n 9 || { echo '已有备份任务运行，停止本次执行。' >&2; exit 1; }

backup_file="/srv/mysql/backups/usercenter_prod-$(date -u +%Y%m%dT%H%M%SZ)-$$.sql.gz"
partial_file="${backup_file}.part"
trap 'rm -f -- "$partial_file"' EXIT

# pipefail 会让 mysqldump、docker exec、gzip 任意一个失败都导致脚本失败。
# stderr 不写入 SQL，因此认证错误不会伪装成一份“成功备份”。
docker compose exec -T mysql sh -ec '
  MYSQL_PWD="$(cat /run/secrets/MYSQL_ROOT_PASSWORD)" \
  mysqldump --protocol=SOCKET --user=root \
    --single-transaction --quick --no-tablespaces \
    --routines --events --triggers --hex-blob \
    --set-gtid-purged=OFF --default-character-set=utf8mb4 \
    --databases usercenter_prod
' | gzip -c > "$partial_file"

test -s "$partial_file"
gzip -t "$partial_file"

# gzip 文件本身非空仍可能只压缩了空输入；检查实际 SQL 非空。
# wc 读取完整数据，避免 grep -q 提前退出导致 pipefail 误判 SIGPIPE。
sql_bytes=$(gzip -dc "$partial_file" | wc -c)
test "$sql_bytes" -gt 0

mv -- "$partial_file" "$backup_file"
(
  cd /srv/mysql/backups
  sha256sum "$(basename "$backup_file")" > "$(basename "$backup_file").sha256"
)
printf '备份已生成，仍需恢复演练：%s\n' "$backup_file"
```

执行并检查：

```bash
chmod 0700 /opt/docker/mysql/backup.sh
bash -n /opt/docker/mysql/backup.sh
bash /opt/docker/mysql/backup.sh
ls -lh /srv/mysql/backups
```

`set -euo pipefail`、临时后缀和完整管道状态避免把失败输出发布成正式备份；`gzip -t` 和 SHA-256 只能确认压缩与传输完整，不能证明 SQL 能恢复。`--set-gtid-purged=OFF` 适用于本篇单库逻辑恢复示例，不用于直接配置复制位点。大库的恢复耗时可能超出窗口，应评估并行逻辑备份或适合版本的物理备份方案。

首次手工备份和下一节恢复演练通过后，再用服务器的 cron/systemd timer 定时执行该脚本，并让监控检查退出状态、最近一次成功时间和文件大小。明确每日/每周保留策略，将备份加密复制到其他机器或对象存储并限制访问；不要一开始就写自动删除历史备份的命令。

#### 9. 恢复演练：把 SQL 真正导入隔离容器

不要拿生产库尝试恢复命令。下面建立独立临时容器、临时 volume 和独立 root 密码；使用 `--network none`，不发布端口，也不挂载 `/srv/mysql/data`。备份内含 `CREATE DATABASE/USE usercenter_prod`，因此通过**隔离实例**规避误导入生产库和跨库定义的问题。

**1）保存为 `/opt/docker/mysql/restore-check.sh`。** 该脚本只接受上一节生成的本地备份路径。

```bash
#!/usr/bin/env bash
set -euo pipefail
umask 077

backup_file=$(realpath -e -- "${1:?用法：bash restore-check.sh /srv/mysql/backups/具体文件.sql.gz}")
case "$backup_file" in
  /srv/mysql/backups/*.sql.gz) ;;
  *) echo '只接受 /srv/mysql/backups 下的 .sql.gz 备份。' >&2; exit 1 ;;
esac
test -s "$backup_file"
(
  cd "$(dirname "$backup_file")"
  sha256sum -c "$(basename "$backup_file").sha256"
)
gzip -t "$backup_file"

cd /opt/docker/mysql
# .env 是第 2 节生成并由 root 管理的纯 MYSQL_IMAGE 赋值文件。
source .env
case "$MYSQL_IMAGE" in
  mysql@sha256:*|docker.io/library/mysql@sha256:*) ;;
  *) echo '请使用已记录的官方镜像 digest。' >&2; exit 1 ;;
esac

restore_name="mysql-restore-check-$(date -u +%Y%m%dT%H%M%SZ)-$$"
restore_volume="${restore_name}-data"
restore_secret=$(mktemp /opt/secrets/mysql/restore-root.XXXXXX)
openssl rand -hex 32 > "$restore_secret"
chmod 0444 "$restore_secret"

# 名称包含时间和 PID，并且拒绝复用已存在的卷。
if docker volume inspect "$restore_volume" >/dev/null 2>&1; then
  echo '临时卷已存在，停止，避免误复用数据。' >&2
  exit 1
fi
docker volume create --label purpose=mysql-restore-check "$restore_volume" >/dev/null
printf '恢复容器=%s\n恢复卷=%s\n临时密码文件=%s\n' \
  "$restore_name" "$restore_volume" "$restore_secret"

docker run -d --name "$restore_name" --network none \
  --label purpose=mysql-restore-check \
  --mount "type=volume,source=$restore_volume,target=/var/lib/mysql" \
  --mount "type=bind,source=$restore_secret,target=/run/secrets/RESTORE_ROOT,readonly" \
  --env MYSQL_ROOT_HOST=localhost \
  --env MYSQL_ROOT_PASSWORD_FILE=/run/secrets/RESTORE_ROOT \
  "$MYSQL_IMAGE" --event-scheduler=OFF >/dev/null

# TCP 就绪检测排除入口脚本的临时初始化服务器。
restore_ready=0
for attempt in $(seq 1 120); do
  if docker exec "$restore_name" sh -ec '
    MYSQL_PWD="$(cat /run/secrets/RESTORE_ROOT)" \
    mysql --protocol=TCP -h127.0.0.1 -uroot --ssl-mode=REQUIRED \
      --connect-timeout=3 --execute="SELECT 1" >/dev/null
  ' 2>/dev/null; then
    restore_ready=1
    break
  fi
  sleep 2
done
if [ "$restore_ready" -ne 1 ]; then
  docker logs --tail=100 "$restore_name"
  echo '隔离实例未就绪，保留现场。' >&2
  exit 1
fi

# 不使用 --force；gzip 或 SQL 导入失败都会导致非零退出。
gzip -dc "$backup_file" | docker exec -i "$restore_name" sh -ec '
  MYSQL_PWD="$(cat /run/secrets/RESTORE_ROOT)" \
  mysql --protocol=SOCKET -uroot --default-character-set=utf8mb4
'

docker exec "$restore_name" sh -ec '
  MYSQL_PWD="$(cat /run/secrets/RESTORE_ROOT)" \
  mysqlcheck --protocol=SOCKET -uroot --check usercenter_prod
'

docker exec "$restore_name" sh -ec '
  MYSQL_PWD="$(cat /run/secrets/RESTORE_ROOT)" \
  mysql --protocol=SOCKET -uroot --database=usercenter_prod \
    --execute="SHOW TABLES; SELECT VERSION();"
'
printf '导入与表检查通过。请继续核对业务数据；容器保留供检查：%s\n' "$restore_name"
```

**2）选择一份具体备份运行。** 把示例文件名替换成 `ls` 实际列出的文件，不能原样照抄日期占位符。

```bash
chmod 0700 /opt/docker/mysql/restore-check.sh
bash -n /opt/docker/mysql/restore-check.sh
bash /opt/docker/mysql/restore-check.sh \
  /srv/mysql/backups/usercenter_prod-实际时间-实际PID.sql.gz
```

**3）做业务校验并记录结果。** 脚本通过代表 SQL 能导入、表能检查，还应在打印出的隔离容器上执行真实业务表的 `COUNT(*)`、已知账号/角色等只读查询，核对备份时间、关键记录、索引和视图。若要比较精确行数，需有备份对应时刻的基线；生产库持续写入时，不能把当前行数差异直接判成备份损坏。需要验证登录业务时，使用隔离测试应用和测试网络，避免发送真实邮件或触发外部任务。

本例关闭事件调度，导入的事件不会被自动执行。若项目有 `DEFINER` 对象，库备份不包含其账号定义，须先在隔离实例恢复相应账号/授权，再验证视图或存储过程能实际执行；仅 `SHOW TABLES` 不足以验收这些对象。

**4）检查完成后清理演练资源。** 用脚本打印的精确名称设置以下变量，先看标签和挂载，确认卷不是生产数据后逐条执行：

```bash
restore_name='替换为脚本打印的恢复容器名'
restore_volume='替换为脚本打印的恢复卷名'
restore_secret='替换为脚本打印的临时密码文件路径'

docker inspect "$restore_name" --format '{{json .Config.Labels}} {{json .Mounts}}'
docker volume inspect "$restore_volume"

# 只允许清理本教程生成的演练资源。
case "$restore_name" in mysql-restore-check-*) ;; *) exit 1 ;; esac
case "$restore_volume" in "${restore_name}-data") ;; *) exit 1 ;; esac
case "$restore_secret" in /opt/secrets/mysql/restore-root.*) ;; *) exit 1 ;; esac

docker stop --time 60 "$restore_name"
docker rm "$restore_name"
docker volume rm "$restore_volume"
rm -f -- "$restore_secret"
```

真正灾难恢复时也应先恢复到干净实例，验证后再让后端连接，并按第 6 节重建/核验应用账号和权限。不要把运行中的 `data` 目录直接复制一份就称为可靠备份，也不要把 `docker compose down -v` 当成恢复步骤。[使用 SQL 备份恢复](https://dev.mysql.com/doc/refman/8.4/en/reloading-sql-format-dumps.html)

#### 10. 更新、密码轮换与日常维护

**1）密码轮换必须同步数据库和文件。** 更改 `DB_PASSWORD` 文件再重启，并不会执行 `ALTER USER`，反而会让健康检查和后端认证失败。

对于本篇单机方案，先安排维护窗口，停止后端写入；在管理员会话中执行应用账号的 `ALTER USER`，把同一个新密码写入 secret，再重建 MySQL 与后端容器并校验连接。SQL 形式如下，字符串是占位符，不能直接使用：

```sql
ALTER USER 'usercenter_app'@'%' IDENTIFIED BY '替换成安全生成的新密码';
```

使用关闭客户端历史的管理员会话，不在 shell 命令行中拼明文密码；自动化轮换应通过权限受限的输入文件或专用秘密管理流程处理。secret 文件建议先写新文件、设 `root:root/0444`，再原子替换，避免读到半份内容；单文件 bind mount 可能仍引用旧 inode，所以要重建使用者，单纯 `docker compose restart` 也不会重新加载 Compose 配置。root 密码单独轮换并验证备份脚本，不能与应用密码混用。需要无停机轮换时再设计双密码过渡和失败回退流程。[ALTER USER 与双密码](https://dev.mysql.com/doc/refman/8.4/en/alter-user.html)

**2）数据库升级按变更执行。** 记录当前 digest、补丁版本、表结构版本；备份并在隔离实例恢复；拉取候选版本，核查官方升级路径和后端兼容；在维护窗口停止后端写入，更新 `.env` 的 digest，执行 `docker compose up -d --wait --wait-timeout 300`，检查日志、版本和业务读写。镜像固定后，单独 `docker compose pull` 不会自动替换成新补丁。

**3）回滚不能直接照搬前端“切回旧镜像”。** 新版 MySQL 可能已变更数据格式或业务表结构。官方当前允许同一 LTS 系列部分原地降级路径，但跨系列和具体版本有条件限制；必须核对确切源/目标版本及发布说明。保守恢复方案是在干净实例上用升级前备份恢复，并评估维护窗口后新增数据如何处理；不可把较旧版本直接挂上生产数据目录试运气。[MySQL 升级](https://dev.mysql.com/doc/refman/8.4/en/upgrading.html)、[MySQL 降级支持路径](https://dev.mysql.com/doc/refman/8.4/en/downgrading.html)

**4）定期查看容量和日志。**

```bash
cd /opt/docker/mysql
df -h /srv/mysql /var/lib/docker
df -i /srv/mysql /var/lib/docker
du -sh /srv/mysql/data /srv/mysql/backups
docker stats --no-stream
docker compose logs --tail=200 mysql
```

根据磁盘、内存和实际连接数调整 buffer pool 与连接池，不照抄“给数据库全部内存”的参数。开启慢查询日志后要单独规划轮转与敏感 SQL 保留；`logging.max-size` 仅限制容器输出日志。binlog 应按恢复策略通过 MySQL 管理，不能在 `data` 内手工删除正在使用的日志文件。

#### 11. 常见问题定位

| 现象 | 先检查什么 | 处理方向 |
| --- | --- | --- |
| `Access denied` | 连接的库/账号/Host、已有数据目录、secret 是否与数据库一致 | 用已知管理凭据查询账号并受控改密，不清空目录 |
| 容器不断重启 | `docker compose logs`、磁盘、权限、无效参数、OOM | 按第一条明确错误修复；不要反复重启掩盖问题 |
| 健康检查失败，日志已 ready | secret 可读性、应用账号权限、TLS、实际 `SELECT 1` | 用第 5 节相同连接方式复现认证问题 |
| `Unknown MySQL server host 'mysql'` | 后端是否加入 `usercenter-db-net` | 两个 Compose 引用同一个 external 网络，避免拼写/别名冲突 |
| `Connection refused` | MySQL 是否完成启动、后端是否错连 `localhost` | 使用 `mysql:3306`，先等数据库健康 |
| `Public Key Retrieval is not allowed` | 驱动、TLS 协商、信任库与证书 | 修复 TLS/驱动配置，不通用关闭 TLS |
| `Plugin 'mysql_native_password' is not loaded` | 旧账号认证插件、旧初始化 SQL | 验证客户端支持后迁移到 `caching_sha2_password` |
| 改了 `.env` 或 secret 但库没变 | 是否是已经初始化的数据目录 | 环境变量仅控制首次初始化；通过 SQL 做迁移/改密 |
| 备份文件存在但恢复失败 | 备份退出状态、压缩校验、SQL 首个错误、DEFINER | 保留失败现场，解决缺少权限/对象/版本差异后重新演练 |

验收时至少记录：镜像 digest 与版本、未发布端口、网络 `internal=true`、真实认证查询成功、应用账号权限、后端读写结果、备份文件与一次隔离恢复演练结果。完成后继续 [Spring Boot 部署与规范](springboot.md)。



# ？？？


## 后端知识补充

### Spring Security


你说得对：只保留一个全局枚举只是短期收口，长期会重新膨胀。更稳妥的方案是：

> 统一错误码协议，按业务域拆枚举，所有枚举实现同一个接口。

这样既不会出现一个无限增长的 `ErrorCodeEnum`，也不会退回“登录一个枚举、注册一个枚举”的碎片化状态。

## 错误码体系（待整理）

#### 一、推荐目录

```text
com.lcq.usercenter
├─ common
│  ├─ error
│  │  ├─ ErrorCode.java
│  │  └─ CommonErrorCodeEnum.java
│  ├─ exception
│  │  ├─ BusinessException.java
│  │  └─ GlobalExceptionHandler.java
│  └─ BaseResponse.java
│
└─ user
   ├─ error
   │  └─ UserErrorCodeEnum.java
   ├─ controller
   ├─ service
   └─ model
```

将来出现其他业务域时平行增加：

```text
order/error/OrderErrorCodeEnum.java
payment/error/PaymentErrorCodeEnum.java
```

不要放成：

```text
common/error/UserLoginResultCodeEnum
common/error/UserRegisterResultCodeEnum
```

因为登录、注册只是操作；`user` 才是业务域。

#### 二、统一错误码接口

`ErrorCode` 是接口，因此不需要 `Enum` 后缀：

```java
public interface ErrorCode {

    String getCode();

    String getMessage();
}
```

以后无论有多少个错误码枚举，`BusinessException`、`BaseResponse` 和异常处理器都只认识这个接口。

#### 三、采用黄山版字符串编码

建议将错误码从 `long` 改成 `String`，使用黄山版常见的五位编码：

```text
Axxxx：用户端错误
Bxxxx：系统执行错误
Cxxxx：第三方服务错误
```

例如：

```text
A0111  用户账号已存在
A0121  密码长度不足
A0202  用户账号被冻结
B0101  用户保存失败
B0201  登录时间更新失败
C0101  短信服务调用失败
```

这里的“用户端错误”不是指只能放在 `user` 模块，而是表示错误责任来源。

例如：

- 用户输入密码不合法：`Axxxx`
- 数据库更新登录时间失败：`Bxxxx`
- 调用短信平台失败：`Cxxxx`

#### 四、错误码区间分配

建议维护固定号段：

| 号段 | 所属范围 |
|---|---|
| `A0001-A0099` | 公共请求错误 |
| `A0100-A0199` | 用户注册 |
| `A0200-A0299` | 用户登录、认证 |
| `A0300-A0399` | 用户权限 |
| `A0400-A0499` | 用户资料 |
| `B0001-B0099` | 公共系统错误 |
| `B0100-B0199` | 用户注册内部错误 |
| `B0200-B0299` | 用户登录内部错误 |
| `C0001-C0099` | 公共第三方服务错误 |
| `C0100-C0199` | 短信、邮件等用户相关服务 |

将来可以继续登记：

```text
A1000-A1999  订单域
A2000-A2999  支付域
B1000-B1999  订单系统错误
B2000-B2999  支付系统错误
```

重点不是号段必须长这样，而是号段必须事先分配，禁止开发时随便找一个数字往后加。

#### 五、公共错误码

```java
@Getter
@AllArgsConstructor
public enum CommonErrorCodeEnum implements ErrorCode {

    SUCCESS("00000", "成功"),

    PARAM_ERROR("A0001", "请求参数错误"),
    REQUEST_METHOD_ERROR("A0002", "请求方式错误"),
    REQUEST_BODY_MISSING("A0003", "请求体不能为空"),

    SYSTEM_ERROR("B0001", "系统执行错误"),
    DATABASE_ERROR("B0002", "数据库操作失败"),

    THIRD_PARTY_ERROR("C0001", "第三方服务调用失败");

    private final String code;
    private final String message;
}
```

公共枚举只放真正跨业务通用的错误。

不要把下面这些重新塞进去：

```text
账号已存在
密码长度不足
账号已禁用
登录时间更新失败
```

它们都属于用户域。

#### 六、用户域错误码

```java
@Getter
@AllArgsConstructor
public enum UserErrorCodeEnum implements ErrorCode {

    // 注册：A01xx
    ACCOUNT_FORMAT_INVALID("A0110", "账号格式错误"),
    ACCOUNT_ALREADY_EXISTS("A0111", "账号已存在"),

    PASSWORD_TOO_SHORT("A0121", "密码长度不足"),
    PASSWORD_TOO_LONG("A0122", "密码长度过长"),
    PASSWORD_MISMATCH("A0123", "两次输入的密码不一致"),

    // 登录：A02xx
    ACCOUNT_OR_PASSWORD_INVALID("A0210", "账号或密码错误"),
    ACCOUNT_DISABLED("A0202", "账号已禁用"),
    ACCOUNT_CANCELLED("A0203", "账号已注销"),
    ACCOUNT_UNAVAILABLE("A0204", "账号状态异常"),

    // 用户模块内部错误：B01xx/B02xx
    USER_SAVE_FAILED("B0101", "用户保存失败"),
    LOGIN_TIME_UPDATE_FAILED("B0201", "登录信息更新失败");

    private final String code;
    private final String message;
}
```

同一个 `UserErrorCodeEnum` 同时出现 `A` 和 `B` 没有问题：

- 枚举归属表示“哪个业务域负责维护”；
- 首字母表示“错误责任来源”。

这是两个不同的维度。

#### 七、BusinessException 只依赖接口

```java
@Getter
public class BusinessException extends RuntimeException {

    private final ErrorCode errorCode;

    public BusinessException(ErrorCode errorCode) {
        super(errorCode.getMessage());
        this.errorCode = errorCode;
    }

    public BusinessException(ErrorCode errorCode, String message) {
        super(message);
        this.errorCode = errorCode;
    }

    public String getCode() {
        return errorCode.getCode();
    }
}
```

这样 Service 可以抛任何业务域的错误：

```java
throw new BusinessException(
        UserErrorCodeEnum.ACCOUNT_ALREADY_EXISTS
);
```

公共参数错误则是：

```java
throw new BusinessException(
        CommonErrorCodeEnum.PARAM_ERROR
);
```

不能让 `BusinessException` 分别提供这种构造器：

```java
BusinessException(CommonErrorCodeEnum errorCode)
BusinessException(UserErrorCodeEnum errorCode)
BusinessException(OrderErrorCodeEnum errorCode)
```

否则每增加一个业务域都要修改异常类，违反开闭原则。

#### 八、BaseResponse 同样依赖接口

```java
@Data
@AllArgsConstructor
public class BaseResponse<T> {

    private String code;

    private String message;

    private T data;

    public static <T> BaseResponse<T> success(T data) {
        return new BaseResponse<>(
                CommonErrorCodeEnum.SUCCESS.getCode(),
                CommonErrorCodeEnum.SUCCESS.getMessage(),
                data
        );
    }

    public static <T> BaseResponse<T> error(ErrorCode errorCode) {
        return new BaseResponse<>(
                errorCode.getCode(),
                errorCode.getMessage(),
                null
        );
    }

    public static <T> BaseResponse<T> error(
            ErrorCode errorCode,
            String message) {

        return new BaseResponse<>(
                errorCode.getCode(),
                message,
                null
        );
    }
}
```

参数校验时，就能同时使用统一错误码和具体校验信息：

```java
return BaseResponse.error(
        CommonErrorCodeEnum.PARAM_ERROR,
        "用户账号不能为空"
);
```

返回结果：

```json
{
  "code": "A0001",
  "message": "用户账号不能为空",
  "data": null
}
```

#### 九、什么时候继续拆 UserErrorCodeEnum

目前只需要一个：

```text
UserErrorCodeEnum
```

当出现以下情况之一再继续拆：

- 单个枚举超过约 30～50 个错误码；
- 认证、用户资料由不同模块或团队负责；
- `auth` 已经成为独立业务域；
- 不同部分拥有独立的号段和发布周期。

这时可以拆为：

```text
user/error/UserErrorCodeEnum
auth/error/AuthErrorCodeEnum
```

或者：

```text
user/registration/error/UserRegistrationErrorCodeEnum
user/authentication/error/UserAuthenticationErrorCodeEnum
```

但不能仅仅因为“有一个登录方法”就建立一个登录错误码枚举。拆分依据是业务边界和维护边界，不是接口数量。

#### 十、防止错误码重复

枚举拆开后，最大的风险是编号冲突。因此要增加一个错误码唯一性测试，统一收集所有枚举：

```java
List<ErrorCode[]> groups = List.of(
        CommonErrorCodeEnum.values(),
        UserErrorCodeEnum.values()
);
```

然后检查：

```java
Set<String> codes = new HashSet<>();

for (ErrorCode[] group : groups) {
    for (ErrorCode errorCode : group) {
        Assertions.assertTrue(
                codes.add(errorCode.getCode()),
                "错误码重复：" + errorCode.getCode()
        );
    }
}
```

以后增加 `OrderErrorCodeEnum`，只需要把它登记进这个测试。这样重复编号会在测试阶段直接失败。

#### 十一、当前项目的迁移方案

你当前项目建议一次收敛到下面这套：

1. 新建 `ErrorCode` 接口。
2. 新建 `CommonErrorCodeEnum`。
3. 新建 `UserErrorCodeEnum`。
4. 将 `BaseResponse.code` 从 `long` 改为 `String`。
5. `BusinessException` 改为接收 `ErrorCode`。
6. 修改 `GlobalExceptionHandler`。
7. Service 使用 `UserErrorCodeEnum` 抛异常。
8. 删除旧的 `ErrorCode`。
9. 删除 `UserLoginResultCode` 和 `UserRegisterResultCode`。
10. 增加错误码唯一性测试。

最终依赖关系应当是：

```text
CommonErrorCodeEnum ─┐
UserErrorCodeEnum ───┼─ implements ErrorCode
OrderErrorCodeEnum ──┘
                         ↓
                  BusinessException
                         ↓
              GlobalExceptionHandler
                         ↓
                   BaseResponse
```

这套结构的核心不是“建立更多类”，而是稳定扩展点：以后新增业务域只增加对应的错误码枚举，不需要修改异常类、响应类和全局处理器。

“统一错误码”不等于“把所有错误塞进一个巨大的枚举”。更优雅的方式是：

```text
统一接口
   ├─ 公共错误码枚举
   ├─ 用户业务错误码枚举
   ├─ 订单业务错误码枚举（以后）
   └─ 第三方服务错误码枚举（以后）
```

每个业务域维护自己的枚举，但都实现同一个 `ErrorCode` 接口，最终由同一个 `BusinessException` 和 `GlobalExceptionHandler` 处理。

### 体系2

#### 一、推荐目录

最终可以这样组织：

```text
com.lcq.usercenter
├─ common
│  ├─ error
│  │  ├─ ErrorCode.java
│  │  └─ CommonErrorCodeEnum.java
│  ├─ exception
│  │  ├─ BusinessException.java
│  │  └─ GlobalExceptionHandler.java
│  └─ response
│     └─ BaseResponse.java
└─ user
   └─ error
      └─ UserErrorCodeEnum.java
```

注意：

- `ErrorCode` 是接口，不需要 `Enum` 后缀；
- `CommonErrorCodeEnum`、`UserErrorCodeEnum` 是枚举，应使用 `Enum` 后缀；
- 错误码属于接口协议和业务规则，不建议放在模糊的 `model.enums`；
- 不再按方法拆成 `UserLoginResultCodeEnum`、`UserRegisterResultCodeEnum`。

也就是说：

```text
不推荐：一个接口一个错误码枚举
推荐：一个业务域一个错误码枚举
```

#### 二、错误码建议使用字符串

如果接口还没有正式发布，建议把错误码从 `long` 改为 `String`：

```text
00000  成功
Axxxx  用户端/请求错误
Bxxxx  当前系统执行错误
Cxxxx  第三方服务错误
```

字符串的优点：

- 能保留 `00000` 的前导零；
- 可以表达 A/B/C 错误来源；
- 不容易和 HTTP 状态码混淆；
- 与黄山版错误码结构更接近。

建议分段：

| 范围 | 含义 |
|---|---|
| `00000` | 成功 |
| `A0001` | 通用客户端参数错误 |
| `A0100～A0199` | 用户注册错误 |
| `A0200～A0299` | 用户登录错误 |
| `A0300～A0399` | 用户资料错误 |
| `B0001` | 系统内部错误 |
| `B0100～B0199` | 用户注册执行失败 |
| `B0200～B0299` | 用户登录执行失败 |
| `C0001～C0999` | 第三方服务调用错误 |

业务错误码与 HTTP 状态码应该分开：

```text
HTTP 400 + 业务码 A0001
HTTP 401 + 业务码 A0201
HTTP 500 + 业务码 B0001
```

不要直接把 `401`、`500` 当成全部业务错误码。

#### 三、统一错误码接口

`ErrorCode.java`：

```java
package com.lcq.usercenter.common.error;

/**
 * 所有业务错误码必须实现的统一接口。
 */
public interface ErrorCode {

    /**
     * 稳定且唯一的错误码。
     */
    String getCode();

    /**
     * 可以安全返回给调用方的默认消息。
     */
    String getMessage();
}
```

后续 `BusinessException` 只依赖这个接口，不关心错误码具体来自哪个枚举。

#### 四、公共错误码

`CommonErrorCodeEnum.java`：

```java
package com.lcq.usercenter.common.error;

/**
 * 与具体业务领域无关的公共错误码。
 */
public enum CommonErrorCodeEnum implements ErrorCode {

    PARAM_ERROR("A0001", "请求参数错误"),

    UNAUTHORIZED("A0002", "用户未登录"),

    FORBIDDEN("A0003", "无权执行该操作"),

    NOT_FOUND("A0004", "请求的资源不存在"),

    SYSTEM_ERROR("B0001", "系统内部异常"),

    DATABASE_ERROR("B0002", "数据处理失败"),

    THIRD_PARTY_ERROR("C0001", "第三方服务调用失败");

    private final String code;
    private final String message;

    CommonErrorCodeEnum(String code, String message) {
        this.code = code;
        this.message = message;
    }

    @Override
    public String getCode() {
        return code;
    }

    @Override
    public String getMessage() {
        return message;
    }
}
```

公共枚举中只放真正跨业务复用的错误，不要放：

```text
账号已存在
订单已关闭
库存不足
支付失败
```

这些应该分别属于用户、订单、库存、支付业务域。

#### 五、用户业务错误码

把登录和注册错误统一到 `UserErrorCodeEnum`：

```java
package com.lcq.usercenter.user.error;

import com.lcq.usercenter.common.error.ErrorCode;

/**
 * 用户领域错误码。
 */
public enum UserErrorCodeEnum implements ErrorCode {

    // A01xx：用户注册错误

    REGISTER_ACCOUNT_INVALID(
            "A0110",
            "账号格式不正确"
    ),

    REGISTER_ACCOUNT_ALREADY_EXISTS(
            "A0111",
            "账号已存在"
    ),

    REGISTER_PASSWORD_INVALID(
            "A0120",
            "密码格式不正确"
    ),

    REGISTER_PASSWORD_TOO_SHORT(
            "A0121",
            "密码不能少于 8 位"
    ),

    REGISTER_PASSWORD_TOO_LONG(
            "A0122",
            "密码不能超过 72 个 UTF-8 字节"
    ),

    REGISTER_PASSWORD_MISMATCH(
            "A0123",
            "两次输入的密码不一致"
    ),

    // A02xx：用户登录错误

    LOGIN_CREDENTIAL_INVALID(
            "A0201",
            "账号或密码错误"
    ),

    ACCOUNT_DISABLED(
            "A0202",
            "账号已被禁用"
    ),

    ACCOUNT_CANCELLED(
            "A0203",
            "账号已注销"
    ),

    ACCOUNT_UNAVAILABLE(
            "A0204",
            "账号当前不可用"
    ),

    // B01xx/B02xx：用户领域的系统执行错误

    REGISTER_SAVE_FAILED(
            "B0101",
            "用户注册失败"
    ),

    LOGIN_TIME_UPDATE_FAILED(
            "B0201",
            "登录信息更新失败"
    );

    private final String code;
    private final String message;

    UserErrorCodeEnum(String code, String message) {
        this.code = code;
        this.message = message;
    }

    @Override
    public String getCode() {
        return code;
    }

    @Override
    public String getMessage() {
        return message;
    }
}
```

这种划分比两个枚举更好：

```text
UserLoginResultCodeEnum
UserRegisterResultCodeEnum
```

因为账号禁用、账号注销等错误并不一定只属于某一个方法。以后管理员查询、刷新令牌、修改密码，也可能复用这些错误。

#### 六、枚举成员应该怎么命名

枚举成员统一使用：

```text
业务场景 + 失败原因
```

推荐：

```java
REGISTER_ACCOUNT_ALREADY_EXISTS
LOGIN_CREDENTIAL_INVALID
ACCOUNT_DISABLED
REGISTER_PASSWORD_TOO_LONG
```

不推荐：

```java
FAILED
ERROR
INVALID
CODE_1
LOGIN_ERROR_2
USER_EXCEPTION
```

名称应该能够脱离中文消息独立表达含义。

另外，不要把枚举的 `ordinal()` 或 `name()` 返回给前端：

```java
// 禁止
errorCode.ordinal();
errorCode.name();
```

因为调整枚举顺序或重命名成员会破坏接口协议。对外只使用显式定义的 `code`。

#### 七、BusinessException 依赖统一接口

```java
package com.lcq.usercenter.common.exception;

import com.lcq.usercenter.common.error.ErrorCode;

public class BusinessException extends RuntimeException {

    private static final long serialVersionUID = 1L;

    private final ErrorCode errorCode;

    public BusinessException(ErrorCode errorCode) {
        super(errorCode.getMessage());
        this.errorCode = errorCode;
    }

    public BusinessException(
            ErrorCode errorCode,
            String message) {
        super(message);
        this.errorCode = errorCode;
    }

    public String getCode() {
        return errorCode.getCode();
    }

    public ErrorCode getErrorCode() {
        return errorCode;
    }
}
```

Service 中使用：

```java
if (accountExists) {
    throw new BusinessException(
            UserErrorCodeEnum.REGISTER_ACCOUNT_ALREADY_EXISTS
    );
}
```

登录失败：

```java
if (user == null
        || !passwordEncoder.matches(
                password,
                user.getPasswordHash())) {
    throw new BusinessException(
            UserErrorCodeEnum.LOGIN_CREDENTIAL_INVALID
    );
}
```

系统异常：

```java
if (userAccountMapper.insert(user) != 1) {
    throw new BusinessException(
            UserErrorCodeEnum.REGISTER_SAVE_FAILED
    );
}
```

#### 八、BaseResponse 使用字符串错误码

```java
public class BaseResponse<T> {

    private static final String SUCCESS_CODE = "00000";
    private static final String SUCCESS_MESSAGE = "成功";

    private final String code;
    private final String message;
    private final T data;

    private BaseResponse(
            String code,
            String message,
            T data) {
        this.code = code;
        this.message = message;
        this.data = data;
    }

    public static <T> BaseResponse<T> success(T data) {
        return new BaseResponse<>(
                SUCCESS_CODE,
                SUCCESS_MESSAGE,
                data
        );
    }

    public static <T> BaseResponse<T> error(
            ErrorCode errorCode) {
        return new BaseResponse<>(
                errorCode.getCode(),
                errorCode.getMessage(),
                null
        );
    }

    public static <T> BaseResponse<T> error(
            String code,
            String message) {
        return new BaseResponse<>(code, message, null);
    }

    public String getCode() {
        return code;
    }

    public String getMessage() {
        return message;
    }

    public T getData() {
        return data;
    }
}
```

全局异常处理：

```java
@ExceptionHandler(BusinessException.class)
public BaseResponse<Void> handleBusinessException(
        BusinessException exception) {

    return BaseResponse.error(
            exception.getCode(),
            exception.getMessage()
    );
}
```

#### 九、哪些信息不能放进错误消息

错误消息会返回给前端，因此必须是安全信息。

推荐：

```text
账号或密码错误
用户当前不可用
系统内部异常
```

不推荐：

```text
数据库中没有找到账号 user001
账号存在，但 BCrypt 密码比较失败
执行 SELECT user_account 时发生 SQLSyntaxErrorException
用户 ID 123 的 password_hash 为空
```

具体账号、SQL、堆栈等信息只写日志。

特别是登录场景，不应分别返回：

```text
账号不存在
密码错误
```

统一返回：

```text
账号或密码错误
```

避免攻击者探测账号是否存在。

#### 十、错误码维护规则

建议将下面几条写进项目规范：

1. 错误码一经发布，不允许改变语义。
2. 已废弃的错误码不允许分配给其他错误。
3. 同一个失败原因不要创建多个错误码。
4. 错误码与中文消息分离，客户端主要判断 code。
5. 禁止前端根据中文 message 判断业务。
6. 禁止直接返回异常类名、数据库错误或堆栈。
7. 禁止使用枚举 ordinal 作为错误码。
8. 新增错误码时检查全局唯一性。
9. Controller 不手工拼装错误码。
10. Service 不返回负数错误码，只返回成功数据或抛业务异常。

还可以维护一份：

```text
docs/error-codes.md
```

用于记录错误码、含义、所属业务域和处理建议。枚举是代码事实来源，文档用于前后端协作。

#### 十一、增加唯一性测试

可以增加一个简单测试，防止重复分配错误码：

```java
@Test
void errorCodesShouldBeUnique() {
    Set<String> codes = new HashSet<>();

    for (CommonErrorCodeEnum value
            : CommonErrorCodeEnum.values()) {
        assertTrue(
                codes.add(value.getCode()),
                "重复错误码：" + value.getCode()
        );
    }

    for (UserErrorCodeEnum value
            : UserErrorCodeEnum.values()) {
        assertTrue(
                codes.add(value.getCode()),
                "重复错误码：" + value.getCode()
        );
    }
}
```

最终推荐：

```text
ErrorCode                         统一接口
CommonErrorCodeEnum               公共错误
UserErrorCodeEnum                 用户领域错误
BusinessException                 携带任意 ErrorCode
GlobalExceptionHandler            统一转换响应
BaseResponse<T>                   统一返回格式
docs/error-codes.md               前后端协作文档
```

核心原则是：**统一的是错误码契约和处理流程，不是把全部业务错误塞进一个枚举。**


## Spring yml

这四个文件不是四套完全独立的配置，而是：

```text
application.yml         公共基础配置
        +
application-{环境}.yml  当前环境的差异配置
        =
应用最终配置
```

同一个 Jar 包不需要为开发、测试、生产分别构建，只需要在启动时选择 Profile。

#### 一、每个文件负责什么

| 文件 | 用途 | 是否自动生效 |
|---|---|---|
| `application.yml` | 所有环境共用的配置 | 始终加载 |
| `application-dev.yml` | 本地开发环境 | 激活 `dev` 时加载 |
| `application-test.yml` | 自动化测试环境 | 激活 `test` 时加载 |
| `application-prod.yml` | 生产环境 | 激活 `prod` 时加载 |

例如激活 `dev`：

```text
application.yml
        +
application-dev.yml
        =
开发环境最终配置
```

激活 `prod`：

```text
application.yml
        +
application-prod.yml
        =
生产环境最终配置
```

Profile 文件中的同名配置会覆盖 `application.yml`。

#### 二、application.yml 放公共配置

适合放以下内容：

- 应用名称；
- MyBatis-Plus Mapper 路径；
- 通用 JSON 配置；
- 通用 Druid 连接池参数；
- 所有环境都相同的业务配置。

```yaml
server:
  port: ${SERVER_PORT:8080}

spring:
  application:
    name: usercenter-backend

  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    druid:
      initial-size: 5
      min-idle: 5
      max-active: 20
      max-wait: 60000
      filter:
        stat:
          enabled: true
          log-slow-sql: true
          slow-sql-millis: 2000
        wall:
          enabled: true

mybatis-plus:
  mapper-locations: classpath*:/mapper/**/*.xml
  configuration:
    map-underscore-to-camel-case: true
```

这里没有数据库地址、账号和密码，因为它们通常因环境而不同。

`${SERVER_PORT:8080}` 的含义是：

```text
如果存在 SERVER_PORT 环境变量
    → 使用环境变量
否则
    → 使用默认值 8080
```

#### 三、application-dev.yml 放开发配置

```yaml
spring:
  config:
    activate:
      on-profile: dev

  datasource:
    url: ${DEV_DB_URL:jdbc:mysql://localhost:3306/usercenter}
    username: ${DEV_DB_USERNAME:root}
    password: ${DEV_DB_PASSWORD}

logging:
  level:
    com.lcq.usercenter: debug
```

开发环境一般具备以下特点：

- 连接本地数据库；
- 日志级别比较详细；
- 可以开启 SQL 日志；
- 可以使用开发工具；
- 不允许连接生产数据库。

即便是开发环境，密码也不建议直接写成：

```yaml
password: lcq
```

而应该使用环境变量：

```yaml
password: ${DEV_DB_PASSWORD}
```

PowerShell 中设置：

```powershell
$env:DEV_DB_PASSWORD = "实际密码"
```

#### 四、application-test.yml 放测试配置

```yaml
spring:
  config:
    activate:
      on-profile: test

  datasource:
    url: ${TEST_DB_URL:jdbc:mysql://localhost:3306/usercenter_test}
    username: ${TEST_DB_USERNAME:root}
    password: ${TEST_DB_PASSWORD}

logging:
  level:
    root: warn
    com.lcq.usercenter: info
```

测试环境必须使用独立数据库：

```text
开发库：usercenter
测试库：usercenter_test
生产库：usercenter_prod
```

不能让自动化测试连接开发库或生产库。即使测试使用 `@Transactional` 回滚，也不应该依赖这种方式保证数据安全。

测试类需要显式激活：

```java
@SpringBootTest
@ActiveProfiles("test")
class UserAccountServiceTest {
}
```

注意：文件名叫 `application-test.yml` 并不意味着执行测试时会自动加载。没有 `@ActiveProfiles("test")` 时，Spring 仍然使用默认 Profile。

当前测试日志如果出现：

```text
No active profile set, falling back to default profile
```

就说明 `application-test.yml` 没有生效。

可以建立一个测试基类：

```java
@SpringBootTest
@ActiveProfiles("test")
public abstract class BaseIntegrationTest {
}
```

其他集成测试继承：

```java
@Transactional
class UserAccountServiceTest
        extends BaseIntegrationTest {
}
```

#### 五、application-prod.yml 放生产配置

```yaml
spring:
  config:
    activate:
      on-profile: prod

  datasource:
    url: ${DB_URL}
    username: ${DB_USERNAME}
    password: ${DB_PASSWORD}

logging:
  level:
    root: info
    com.lcq.usercenter: info
```

生产环境有几个原则：

- 数据库地址不得提供本地默认值；
- 用户名、密码必须由部署环境提供；
- 不开启 DEBUG 日志；
- 不在仓库中保存真实密码；
- 缺少关键环境变量时宁可启动失败。

因此生产配置推荐：

```yaml
password: ${DB_PASSWORD}
```

不推荐：

```yaml
password: ${DB_PASSWORD:123456}
```

因为后者在环境变量配置遗漏时，会偷偷使用默认密码运行。

#### 六、如何激活不同环境

##### IntelliJ IDEA

在运行配置中设置：

```text
Active profiles: dev
```

或者 VM Options：

```text
-Dspring.profiles.active=dev
```

##### 命令行运行 Jar

开发环境：

```powershell
java -jar target/usercenter-backend.jar `
  --spring.profiles.active=dev
```

生产环境：

```powershell
java -jar target/usercenter-backend.jar `
  --spring.profiles.active=prod
```

##### 使用环境变量

PowerShell：

```powershell
$env:SPRING_PROFILES_ACTIVE = "dev"
$env:DEV_DB_PASSWORD = "实际密码"

java -jar target/usercenter-backend.jar
```

Linux：

```bash
export SPRING_PROFILES_ACTIVE=prod
export DB_URL='jdbc:mysql://mysql:3306/usercenter'
export DB_USERNAME='usercenter_app'
export DB_PASSWORD='实际密码'

java -jar usercenter-backend.jar
```

##### 单元测试

```java
@ActiveProfiles("test")
```

或者 Maven：

```powershell
mvn test -Dspring.profiles.active=test
```

#### 七、配置是怎样覆盖的

假设基础配置：

```yaml
# application.yml
server:
  port: 8080

logging:
  level:
    root: info
```

开发配置：

```yaml
# application-dev.yml
server:
  port: 8081

logging:
  level:
    root: debug
```

激活 `dev` 后，最终结果：

```yaml
server:
  port: 8081

logging:
  level:
    root: debug
```

因为 Profile 配置覆盖公共配置。

大致优先级从低到高是：

```text
application.yml
    ↓
application-{profile}.yml
    ↓
操作系统环境变量
    ↓
Java -D 系统参数
    ↓
命令行 --参数
```

例如即使 `application-prod.yml` 配置端口为 8080，仍可以启动时覆盖：

```powershell
java -jar app.jar `
  --spring.profiles.active=prod `
  --server.port=9090
```

#### 八、不要在 application.yml 固定 dev

不建议提交：

```yaml
spring:
  profiles:
    active: dev
```

因为打包后的应用如果没有覆盖它，生产环境也可能误用开发配置。

更安全的方式是由运行环境决定：

```text
本地 IntelliJ → dev
自动化测试 → test
生产部署 → prod
```

如果希望本地有个人专属配置，可以增加：

```text
application-local.yml
```

并将它加入 `.gitignore`：

```gitignore
application-local.yml
```

本地激活：

```text
dev,local
```

公共的 `application-dev.yml` 只保留可提交内容，个人密码放在未提交的 `application-local.yml` 或环境变量中。

#### 九、推荐最终配置结构

```text
src/main/resources
├─ application.yml
│  └─ 应用名、MyBatis-Plus、公共连接池配置
├─ application-dev.yml
│  └─ 本地数据库、开发日志
├─ application-test.yml
│  └─ 独立测试数据库、测试日志
├─ application-prod.yml
│  └─ 环境变量引用、生产日志
└─ mapper
   └─ user
      └─ UserAccountMapper.xml
```

核心理解是：

> `application.yml` 是公共底座，`application-dev/test/prod.yml` 只保存各环境之间的差异；Profile 负责在运行时选择差异配置，密码仍然需要通过环境变量或密钥系统提供。

## Spring Security + Session

对，你这个感觉很正常。你现在这两个 `RestHandler` 看起来确实有一点：

```text
SecurityConfig
    ↓
RestXXXHandler
    ↓
SecurityErrorResponseWriter
    ↓
ObjectMapper
    ↓
HttpServletResponse
```

像是“对象一层一层往下传”。

但拆开职责以后，其实每一层干的事不一样。你当前这套设计整体是合理的，只是**项目比较小时会显得层数多**。下面我直接按你现在的代码讲。

### 一、先看两个 Handler 到底是什么

Spring Security 在 Filter 链里遇到安全错误时，不能直接走你 Controller 里的：

```java
@RestControllerAdvice
@ExceptionHandler
```

因为很多错误发生时：

```text
请求
 ↓
Spring Security Filter
 ↓
出错了
 ↓
Controller 都还没进去
```

所以 Spring Security 自己定义了两个出口：

```text
未登录
    ↓
AuthenticationEntryPoint

已登录但没权限
    ↓
AccessDeniedHandler
```

你的两个类就是把这两个默认出口换成了：

```text
返回你项目统一格式的 JSON
```

而不是默认 HTML / 普通 403。

---
### 二、`RestAuthenticationEntryPoint`

你的代码：

```java
@Component
public class RestAuthenticationEntryPoint
        implements AuthenticationEntryPoint {

    private final SecurityErrorResponseWriter responseWriter;

    public RestAuthenticationEntryPoint(
            SecurityErrorResponseWriter responseWriter) {
        this.responseWriter = responseWriter;
    }

    @Override
    public void commence(
            HttpServletRequest request,
            HttpServletResponse response,
            AuthenticationException authException)
            throws IOException {

        responseWriter.write(
                response,
                HttpStatus.UNAUTHORIZED.value(),
                UserErrorCodeEnum.NOT_LOGGED_IN
        );
    }
}
```

它只处理一种情况：

> **用户还没有登录，却访问了要求登录的接口。**

比如：

```java
.antMatchers("/api/**").authenticated()
```

用户没有 `Authentication`：

```http
GET /api/user/current
```

Spring Security发现：

```text
当前 Authentication 不存在
        ↓
这个接口 authenticated()
        ↓
不能访问
        ↓
AuthenticationEntryPoint
```

于是调用：

```java
commence(...)
```

然后你的代码返回：

```text
HTTP 401
+
NOT_LOGGED_IN
```

所以这个类可以直接记成：

> **“未登录处理器”**

---

### 三、这里三个参数为什么只用了一个半

接口规定：

```java
commence(
    HttpServletRequest request,
    HttpServletResponse response,
    AuthenticationException authException
)
```

Spring Security必须把这些都给你。

但是你的项目现在不关心：

```java
request
```

也不区分具体：

```java
authException
```

所有未登录情况统一：

```text
NOT_LOGGED_IN
```

所以最后只需要：

```java
response
```

往客户端写结果。

这不是你代码写多了，而是：

> `AuthenticationEntryPoint` 接口规定必须长这个样子。

以后你想记录：

```text
哪个 URL 未登录
什么异常导致
客户端 IP
```

这些参数就会有用：

```java
log.warn(
    "未登录访问 {}, 原因 {}",
    request.getRequestURI(),
    authException.getMessage()
);
```

---
### 四、`RestAccessDeniedHandler`

这个就比上一个多干了一件事。

你的代码：

```java
@Component
public class RestAccessDeniedHandler
        implements AccessDeniedHandler {

    private final SecurityErrorResponseWriter responseWriter;

    public RestAccessDeniedHandler(
            SecurityErrorResponseWriter responseWriter) {
        this.responseWriter = responseWriter;
    }

    @Override
    public void handle(
            HttpServletRequest request,
            HttpServletResponse response,
            AccessDeniedException accessDeniedException)
            throws IOException {

        UserErrorCodeEnum errorCode =
                accessDeniedException
                        instanceof MissingCsrfTokenException
                || accessDeniedException
                        instanceof InvalidCsrfTokenException

                ? UserErrorCodeEnum.CSRF_VALIDATION_FAILED
                : UserErrorCodeEnum.ACCESS_DENIED;

        responseWriter.write(
                response,
                HttpStatus.FORBIDDEN.value(),
                errorCode
        );
    }
}
```

这个类处理：

> **403 Forbidden**

但你的 403 分成了两种业务含义。

第一种：

```text
已经登录
但是权限不足
```

例如：

```text
当前：

ROLE_USER

访问：

/api/admin/user/list

要求：

ROLE_ADMIN
```

于是：

```text
AccessDeniedException
        ↓
ACCESS_DENIED
        ↓
403
```

---

第二种：

```text
CSRF Token 错误
```

比如：

```http
POST /api/user/update

没有 X-CSRF-TOKEN
```

或者 Token 不对。

产生：

```java
MissingCsrfTokenException
```

或者：

```java
InvalidCsrfTokenException
```

于是：

```text
CSRF_VALIDATION_FAILED
        ↓
403
```

所以这个 Handler 不只是：

> “返回 403。”

它还负责：

> **判断这个 403 究竟是权限问题还是 CSRF 问题。**

按你现在代码的设计就是这样。

---
### 五、所以 401 和 403 要这样区分

这个非常重要：

```text
请求访问受保护资源
        │
        ↓
Spring Security
        │
        ├── 根本没登录
        │       ↓
        │ AuthenticationEntryPoint
        │       ↓
        │      401
        │
        └── 已经登录
             但没有权限
                ↓
          AccessDeniedHandler
                ↓
               403
```

再加你的 CSRF：

```text
CSRF 验证失败
      ↓
AccessDeniedHandler
      ↓
403
```

所以：

| 情况            | 你的类                            | HTTP |
| ------------- | ------------------------------ | ---: |
| 没登录           | `RestAuthenticationEntryPoint` |  401 |
| 已登录但角色不够      | `RestAccessDeniedHandler`      |  403 |
| CSRF Token 错误 | `RestAccessDeniedHandler`      |  403 |

---
### 六、你觉得“左手倒右手”的核心其实在这里

两个 Handler 都有：

```java
private final SecurityErrorResponseWriter responseWriter;
```

构造函数：

```java
public RestAuthenticationEntryPoint(
        SecurityErrorResponseWriter responseWriter) {
    this.responseWriter = responseWriter;
}
```

然后最后：

```java
responseWriter.write(...);
```

你可能在想：

> 我为什么不直接在 Handler 里写 response？

当然可以。

比如完全可以：

```java
@Override
public void commence(...) throws IOException {

    response.setStatus(401);
    response.setCharacterEncoding("UTF-8");
    response.setContentType("application/json");

    objectMapper.writeValue(
            response.getWriter(),
            BaseResponse.error(
                    UserErrorCodeEnum.NOT_LOGGED_IN
            )
    );
}
```

这样：

```text
RestAuthenticationEntryPoint
```

一个类就全干完了。

---
### 七、但然后你看看另一个 Handler

它又得写：

```java
response.setStatus(403);
response.setCharacterEncoding("UTF-8");
response.setContentType("application/json");

objectMapper.writeValue(
        response.getWriter(),
        BaseResponse.error(errorCode)
);
```

于是重复了：

```text
设置 HTTP 状态
设置 UTF-8
设置 JSON Content-Type
构造 BaseResponse
调用 ObjectMapper
写 response
```

所以你现在抽了一个：

```java
SecurityErrorResponseWriter
```

专门负责这件事情。

---
### 八、`SecurityErrorResponseWriter` 的真正作用

你的代码：

```java
@Component
public class SecurityErrorResponseWriter {

    private final ObjectMapper objectMapper;

    public SecurityErrorResponseWriter(
            ObjectMapper objectMapper) {
        this.objectMapper = objectMapper;
    }

    public void write(
            HttpServletResponse response,
            int httpStatus,
            ErrorCode errorCode)
            throws IOException {

        response.setStatus(httpStatus);

        response.setCharacterEncoding(
                StandardCharsets.UTF_8.name()
        );

        response.setContentType(
                MediaType.APPLICATION_JSON_VALUE
        );

        objectMapper.writeValue(
                response.getWriter(),
                BaseResponse.error(errorCode)
        );
    }
}
```

这个类根本不负责判断：

```text
这是401？
这是403？
这是CSRF？
这是权限不足？
```

它只负责：

> **“你告诉我 HTTP 状态码和业务错误码，我负责按照项目统一 JSON 格式写出去。”**

所以：

```text
RestAuthenticationEntryPoint
负责决定：
401 + NOT_LOGGED_IN


RestAccessDeniedHandler
负责决定：
403 + ACCESS_DENIED

或者：

403 + CSRF_VALIDATION_FAILED


SecurityErrorResponseWriter
负责：
把上面的结果真正写成 JSON
```

这其实是职责分离。

---
### 九、整个关系画出来就不绕了

```text
                    Spring Security
                           │
              ┌────────────┴────────────┐
              │                         │
           没登录                    权限/CSRF失败
              │                         │
              ↓                         ↓
RestAuthenticationEntryPoint   RestAccessDeniedHandler
              │                         │
              │ 决定                    │ 判断
              │                         │
      401 + NOT_LOGGED_IN        ┌──────┴───────┐
              │                  │              │
              │               CSRF失败        权限不足
              │                  │              │
              │                  ↓              ↓
              │      CSRF_VALIDATION_FAILED ACCESS_DENIED
              │                  │              │
              └──────────────┬───┴──────────────┘
                             ↓
                 SecurityErrorResponseWriter
                             │
                             ↓
                       BaseResponse
                             │
                             ↓
                       ObjectMapper
                             │
                             ↓
                  HttpServletResponse
                             │
                             ↓
                           前端
```

这样看就不会觉得是在无意义倒手了。

---
### 十、那 `responseWriter` 这个变量到底是什么

这里：

```java
private final SecurityErrorResponseWriter responseWriter;
```

就是：

> 这个 Handler 依赖的另一个组件。

Spring 启动以后：

```text
SecurityErrorResponseWriter
        ↓
@Component
        ↓
Spring 创建对象
```

然后创建：

```text
RestAuthenticationEntryPoint
```

发现构造函数需要：

```java
SecurityErrorResponseWriter
```

Spring 就把那个对象传进来：

```java
new RestAuthenticationEntryPoint(
        securityErrorResponseWriter
);
```

所以：

```java
this.responseWriter = responseWriter;
```

纯粹是把依赖保存成成员变量，等后面的：

```java
commence()
```

调用。

这是标准的：

```text
构造器依赖注入
```

不是业务数据倒来倒去。

---
### 十一、再往上一层，SecurityConfig 也是一样

你：

```java
private final RestAuthenticationEntryPoint authenticationEntryPoint;
private final RestAccessDeniedHandler accessDeniedHandler;
```

然后：

```java
public SecurityConfig(
        RestAuthenticationEntryPoint authenticationEntryPoint,
        RestAccessDeniedHandler accessDeniedHandler) {

    this.authenticationEntryPoint =
            authenticationEntryPoint;

    this.accessDeniedHandler =
            accessDeniedHandler;
}
```

最后：

```java
.exceptionHandling(exceptions -> exceptions
        .authenticationEntryPoint(
                authenticationEntryPoint)
        .accessDeniedHandler(
                accessDeniedHandler))
```

这一层不是：

> “SecurityConfig 又把东西倒了一遍。”

而是在告诉 Spring Security：

```text
以后遇到401
    ↓
用这个对象：
RestAuthenticationEntryPoint


以后遇到403
    ↓
用这个对象：
RestAccessDeniedHandler
```

也就是**注册策略**。

---
### 十二、所以完整的依赖关系其实非常规整

```text
SecurityConfig
│
├── RestAuthenticationEntryPoint
│      │
│      └── SecurityErrorResponseWriter
│                  │
│                  └── ObjectMapper
│
└── RestAccessDeniedHandler
       │
       └── SecurityErrorResponseWriter
                   │
                   └── ObjectMapper
```

注意两个 Handler 用的是**同一个类型的 Writer Bean**。

所以不是：

```text
Writer1
Writer2
```

而更像：

```text
              SecurityErrorResponseWriter
                    ↗        ↖
                   /          \
          EntryPoint         DeniedHandler
```

---
### 十三、那这一层 Writer 有没有必要？

我的判断是：

> **可以保留，而且我建议保留。**

你的项目已经有：

```java
BaseResponse
ErrorCode
UserErrorCodeEnum
```

说明你本身就在做统一 API 响应。

如果把 `SecurityErrorResponseWriter` 删掉，两个 Handler 都要各自重复：

```java
response.setStatus(...);
response.setCharacterEncoding(...);
response.setContentType(...);

objectMapper.writeValue(
    response.getWriter(),
    BaseResponse.error(...)
);
```

以后如果你统一响应格式从：

```json
{
    "code": ...,
    "message": ...
}
```

改了，两个地方还得一起改。

现在只有：

```text
SecurityErrorResponseWriter
```

需要改。

所以这个抽象属于：

> **代码不多，但职责是有价值的。**

---
### 十四、再把你现在所有 Security 类说清楚

你现在这几个类，可以这样记。

#### `SecurityConfig`

整个 Security 系统的**总装配类**。

负责：

```text
哪些接口公开
哪些接口需要登录
哪些接口需要管理员

CSRF 怎么存

Session 怎么管理

401 交给谁
403 交给谁

是否开启 formLogin
是否开启 httpBasic
是否开启默认 logout
```

它自己基本不处理请求。

更像：

> **Security 的配置中心。**

---

#### `CsrfTokenController`

负责：

```http
GET /api/auth/csrf
```

给前端提供：

```text
CSRF Token
```

它不是生成算法本身。

真正生成/存储 Token 的是 Spring Security。

这个 Controller 只是：

```text
Spring Security的 CsrfToken
        ↓
包装成 CsrfTokenVO
        ↓
返回前端
```

---

#### `CsrfTokenVO`

纯数据对象：

```java
headerName
parameterName
token
```

例如返回：

```json
{
    "headerName": "X-CSRF-TOKEN",
    "parameterName": "_csrf",
    "token": "abc..."
}
```

就是：

> **CSRF 接口的响应格式。**

---

#### `LoginUserPrincipal`

这是：

> **Spring Security眼里的“当前用户”。**

你的：

```java
Authentication
```

里面：

```text
principal
    ↓
LoginUserPrincipal
```

保存：

```text
userId
role
```

以后：

```java
authentication.getPrincipal()
```

得到的就是它。

---

#### `RestAuthenticationEntryPoint`

一句话：

> **未登录 → 401 JSON。**

```text
没有 Authentication
+
访问 authenticated() 接口
        ↓
RestAuthenticationEntryPoint
```

---

#### `RestAccessDeniedHandler`

一句话：

> **禁止访问 → 403 JSON。**

并且你的版本还负责细分：

```text
403
├── ACCESS_DENIED
└── CSRF_VALIDATION_FAILED
```

---

#### `SecurityErrorResponseWriter`

一句话：

> **Security Filter 层统一 JSON 输出工具。**

它存在的一个重要原因是：

```text
Spring Security Filter 出错
       ↓
还没进入 Controller
       ↓
@ControllerAdvice 很可能帮不上忙
       ↓
自己写 HttpServletResponse
```

所以专门抽了这个组件。

---
### 十五、三个 DTO/VO 又是什么

你文件里的这些不算 Security 核心基础设施，而是登录业务的数据结构。

`UserLoginRequestDTO`：

```text
前端 → 后端
```

装：

```text
username
password
```

---

`LoginResultDTO`：

```text
Service → Controller
```

装：

```text
userId
access
```

属于内部业务传输对象。

---

`LoginResultVO`：

```text
Controller → 前端
```

装：

```text
status
type
currentAuthority
```

所以：

```text
RequestDTO
前端传进来

ResultDTO
后端内部传

ResultVO
返回给前端
```

---

### 十六、你现在最值得记的一张图

```text
                        SecurityConfig
                              │
              配置整个 Spring Security
                              │
          ┌───────────────────┴───────────────────┐
          │                                       │
       认证状态                                 CSRF
          │                                       │
          ↓                                       ↓
 Authentication                           CsrfTokenRepository
          │                                       │
          ↓                                       ↓
LoginUserPrincipal                            Session
          │
          ↓
   URL 权限判断
          │
     ┌────┴─────┐
     │          │
   未登录      没权限
     │          │
     ↓          ↓
 EntryPoint   AccessDeniedHandler
     │          │
     └────┬─────┘
          ↓
SecurityErrorResponseWriter
          ↓
     BaseResponse
          ↓
      JSON 响应
```

所以你说“左手倒右手”并没有看错，**代码确实在层层委托**。

但这里不是为了绕，而是把：

```text
错误发生
错误分类
错误码决定
JSON序列化
HTTP输出
```

拆开了。

如果让我评价你当前这部分：**`SecurityErrorResponseWriter` 可以留，两个 Handler 也都应该留。它们的边界目前是清楚的，不建议为了少两个成员变量强行合并。**

## CloudFlare证书

这套方案可以做到 **100% 免费**，而且证书几乎免维护：边缘用 Cloudflare 免费版的 **Universal SSL**（自动签发自动续），源站用 Cloudflare 的 **Origin CA 证书**（免费、一次签发 15 年，不需要 certbot/Let's Encrypt 那套 90 天续期流程）。下面是完整操作方案。

---

### 一、目标架构

```
用户浏览器
   │  https://dashboard.19271949.xyz
   ▼
Cloudflare 边缘  ── 443 ──►  你的 nginx (docker)
 · Universal SSL(免费,自动续)   · Origin CA 证书(免费,15年)
 · Full (strict) 回源校验        · 只认 CF 的源站防火墙
```

关键认知：**证书分两段**。
- 浏览器 ↔ CF 边缘：用 CF 的 **Universal SSL**（免费计划自带，橙色云开启后自动签发）；
- CF 边缘 ↔ 你的 nginx：用 **Origin CA 证书**。这张证书**只有 Cloudflare 信任**，浏览器不认——但这没关系，因为按你的防火墙设计，源站只接受 CF 的连接，它本来就只服务 CF 一个"客户端"。

---

### 二、Cloudflare 控制台设置（按顺序做）

#### 第 0 步：先确认 DNS 是"橙色云"（代理）状态

DNS 管理页里 `dashboard.19271949.xyz` 的 A 记录必须是 **Proxied（橙色云图标）**，而不是仅 DNS（灰云）。灰色云 = 不走 CF = 没有边缘证书，后面全部免谈。切橙云后等几分钟，Universal SSL 自动签发（[Universal SSL 文档](https://developers.cloudflare.com/ssl/edge-certificates/universal-ssl/)），状态变 **Active**。

#### 第 1 步：创建 Origin 证书（在切模式**之前**先做服务器那半边）

导航：**SSL/TLS → Origin Server → Create Certificate**（[官方文档](https://developers.cloudflare.com/ssl/origin-configuration/origin-ca/)）：

- **Hostnames**：至少填 `dashboard.19271949.xyz`。如果这台机器以后还要给主站 `19271949.xyz` 服务，建议一次填全：`19271949.xyz`、`dashboard.19271949.xyz`、`*.19271949.xyz`（注意通配符不含裸域，裸域要单独写）。
- **Key type**：ECC（P-256）或 RSA 2048 都行，没有特殊兼容需求选 ECC。
- **Validity**：15 years。
- 点创建后**立刻把 "Certificate" 和 "Private key" 两段保存下来**——私钥只在这次展示，丢了只能重新签发。

**先别切 SSL 模式**，去把服务器配好（第三节），再回控制台做第 2 步。

#### 第 2 步：切 SSL 模式到 Full (strict)

服务器 443 就绪并验证通过后：**SSL/TLS → Overview**，把模式从现在的（大概率是 Flexible/Full）切成 **Full (strict)**（[模式说明](https://developers.cloudflare.com/ssl/origin-configuration/ssl-modes/full-strict/)）。

Full (strict) 的含义：CF 回源**必须走 443**，且会校验源站证书的**主机名 + 证书链**必须来自 Cloudflare Origin CA。这是免费方案里最安全的档位，也是你这套"443 + 证书"想法的正确落地方式（别再退回 Flexible，那是明文回源）。

#### 第 3 步：边缘行为开关

**SSL/TLS → Edge Certificates**：

| 开关 | 建议 | 说明 |
|---|---|---|
| Always Use HTTPS | ✅ 开 | 访问 http:// 自动 301 到 https（在边缘完成，源站几乎收不到 80） |
| Automatic HTTPS Rewrites | ✅ 开 | 修正页面里 http:// 的链接 |
| HSTS | ⏸ **最后再开** | 见下方警告 |
| Minimum TLS Version | TLS 1.2 | 免费版默认即此 |

**HSTS 警告**：HSTS 一旦下发，浏览器会在设定时长内**强制直连 https**、拒绝任何证书错误。务必等 Full (strict) 生效、https 全链路稳定两三天后再开，且先用 `max-age=2592000`（30 天）试水，别一上来就 preload / includeSubDomains。

---

### 三、服务器端：证书 + nginx

#### 1. 放证书文件

https://developers.cloudflare.com/ssl/origin-configuration/origin-ca/#cloudflare-origin-ca-root-certificate

也有教程说根证书不需要，自行分辨吧

```bash
cd ~/code/nginx/docker-deploy/nginx
mkdir -p certs && cd certs

# ① 把控制台保存的两段粘贴成文件（vim/nano）：
#    dashboard.pem   ← Certificate 段（叶子证书）
#    dashboard.key   ← Private key 段

# ② Full (strict) 需要完整证书链：叶子 + Cloudflare Origin CA 根证书。
#    根证书在 Origin CA 文档页/创建结果页有下载链接（分 RSA 和 ECC 两种，
#    和你签发时选的密钥类型对应）。
curl -fsSL -o origin-ca-root.pem <文档页给的根证书下载链接>

# curl -fsSL -o origin-ca-root.pem https://developers.cloudflare.com/ssl/static/origin_ca_rsa_root.pem

# ③ 拼接成 nginx 用的 fullchain（顺序：叶子在前）
cat dashboard.pem origin-ca-root.pem > dashboard-fullchain.pem
chmod 600 dashboard.key

# ④ 443 兜底 server 用的自签证书（见配置，一次生成即可）
openssl req -x509 -nodes -newkey rsa:2048 -days 3650 \
  -keyout default.key -out default.crt -subj "/CN=unknown-host"

ls -l
# 期望看到: dashboard.pem dashboard.key dashboard-fullchain.pem
#           origin-ca-root.pem default.crt default.key
```

> 若第 ② 步的根证书链接找不到，可先只用 `dashboard.pem` 试跑——如果切 Full (strict) 后出现 526，九成就是缺链/链不对，再把根拼回去（见第五节排查）。
> **这里命名不够规范，如果是全站通用证书建议重命名为域名，如：`19271949.xyz-fullchain.pem`**

#### 2. compose.yml 挂载证书目录

在你的 nginx service 的 `volumes` 里加一行（和你现有 `./conf.d` 挂载并列）：

```yaml
volumes:
  - ./conf.d:/etc/nginx/conf.d:ro
  - ./certs:/etc/nginx/certs:ro     # 新增
```

#### 3. 重写 dashboard.conf（80 跳转 + 443 主站）

```nginx
# ================= HTTP → HTTPS =================
server {
    listen 80;
    server_name dashboard.19271949.xyz;
    # 正常流量在 CF 边缘(Always Use HTTPS)就已跳转,这条是直连/测试兜底
    return 301 https://$host$request_uri;
}

# ================= HTTPS 主站 =================
server {
    listen 443 ssl;
    http2 on;              # nginx >= 1.25.1 写法
    # 若你的镜像较旧(< 1.25.1),请改用: listen 443 ssl http2;
    server_name dashboard.19271949.xyz;

    # ---- Cloudflare Origin CA 证书(免费,15 年) ----
    ssl_certificate     /etc/nginx/certs/dashboard-fullchain.pem;
    ssl_certificate_key /etc/nginx/certs/dashboard.key;
    ssl_protocols       TLSv1.2 TLSv1.3;
    ssl_session_cache   shared:SSL:10m;
    ssl_session_timeout 1d;

    root  /srv/www/dashboard.19271949.xyz/current/public;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /assets/ {
        expires 7d;
        add_header Cache-Control "public";
    }

    # 以后启用后端时打开(内部走 docker 网络,与 443 无关)
    # location /api/ {
    #     proxy_pass http://usercenter-backend:8080/;
    #     proxy_set_header Host $host;
    #     proxy_set_header X-Real-IP $remote_addr;
    #     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    #     proxy_set_header X-Forwarded-Proto $scheme;
    # }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/share/nginx/html;
    }
}
```

#### 4. 更新 00-catchall.conf（补上 443 兜底）

之前那份只有 80，现在 443 也必须有一个 default server——**注意：443 的默认 server 同样需要证书才能完成 TLS 握手**，这里用自签的即可（真实域名会命中上面的 dashboard 块，永远轮不到它）：

```nginx
# ============ 80 兜底:未匹配域名 → 直接断开 ============
server {
    listen 80 default_server;
    server_name _;
    access_log off;
    return 444;
}

# ============ 443 兜底:未匹配 SNI → 直接断开 ============
server {
    listen 443 ssl default_server;
    http2 on;
    server_name _;

    ssl_certificate     /etc/nginx/certs/default.crt;
    ssl_certificate_key /etc/nginx/certs/default.key;

    access_log off;
    return 444;
}
```

#### 5. 防火墙

保持你的"只放行 Cloudflare IP"策略，**确认 443 也在放行范围内**（IP 清单以官方为准：[Cloudflare IP 列表](https://www.cloudflare.com/ips/)）。理论上 Full (strict) 下源站只需要 443，80 可留作自测或过渡期。如果之前是逐条加的，顺手核对一次列表是否最新。

#### 6. 校验并生效

```bash
cd ~/code/nginx/docker-deploy/nginx
docker compose exec nginx nginx -t          # 语法检查,尤其确认证书文件路径
docker compose exec nginx nginx -s reload

# 源站本地自测(证书是 CF Origin CA 签的,普通 curl 会报证书不受信,属正常)
# 方法 A: 忽略证书校验,确认能握手且返回你的页面
curl -skI --resolve dashboard.19271949.xyz:443:127.0.0.1 \
     https://dashboard.19271949.xyz/ | head -5
# 方法 B: 用 CF 根证书做真校验(更严谨)
curl -sI --cacert ~/code/nginx/docker-deploy/nginx/certs/origin-ca-root.pem \
     --resolve dashboard.19271949.xyz:443:127.0.0.1 \
     https://dashboard.19271949.xyz/ | head -3
```

`nginx -t` 报"cannot load certificate"就是文件路径/拼接问题，先解决再 reload。

#### 7. 回控制台收尾

本地验证 OK 后：SSL 模式切 **Full (strict)** → 开 **Always Use HTTPS** → 浏览器访问 `https://dashboard.19271949.xyz` 确认正常 → 再考虑 HSTS（见上文警告）。

---

### 四、验证清单

| 检查项 | 方法 | 期望 |
|---|---|---|
| 边缘证书 | `curl -vI https://dashboard.19271949.xyz/` | 证书 CN 是 `sni.cloudflaressl.com` 之类（CF 签发），有 `cf-ray` 响应头，HTTP/2 |
| 回源模式 | CF 控制台 SSL/TLS → Overview | 显示 **Full (strict)** |
| http 跳转 | 浏览器开 http:// 地址 | 301 → https |
| 源站 443 | 上面第 6 步 curl | 200 + 你的页面 |
| Universal SSL | Edge Certificates 页 | Active |

---

### 五、切完模式后的常见报错速查

| CF 状态码 | 含义 | 大概率原因 | 修法 |
|---|---|---|---|
| [521](https://developers.cloudflare.com/support/troubleshooting/http-status-codes/cloudflare-5xx-errors/) | 源站拒连 | nginx 没监听 443 / 防火墙没放行 CF 段 443 | `nginx -t`、看容器端口、查防火墙 |
| [525](https://community.cloudflare.com/t/error-525-ssl-handshake-failed-after-origin-certificate-generation/784770) | 回源 TLS 握手失败 | 证书与私钥不匹配 / 拼错文件 | 核对 `dashboard.pem`↔`dashboard.key` 是否同一张 |
| [526](https://developers.cloudflare.com/support/troubleshooting/http-status-codes/cloudflare-5xx-errors/error-526/) | 源站证书无效 | 主机名没写进 SAN / 证书链缺根 / 模式其实是 Full | fullchain 补根证书、确认签发时包含该域名 |

---

### 六、几个补充提醒

1. **续期成本≈0**：Universal SSL 全自动续；Origin CA 15 年到期前 CF 会邮件提醒，重新点一次签发、替换两个文件即可。整个方案没有 certbot、没有 cron，全部免费。
2. **nignx 日志里的"访客 IP"其实是 CF 边缘 IP**。以后想看真实访客 IP，用 `CF-Connecting-IP` 请求头配合 `set_real_ip_from`（CF IP 段）+ `real_ip_header CF-Connecting-IP;`；将来给 Spring Boot 反代开 `/api/` 时同理，后端读 `CF-Connecting-IP` 即可，别再用 `$remote_addr`。
3. **HSTS 顺序很重要**：Full (strict) 稳定运行几天后再开，先短 `max-age` 试水，避免把用户锁死在坏证书上。
4. **www/主站**：如果以后 `www.19271949.xyz` 或主站也要走这台 nginx，Origin 证书签发时就带好这些 SAN（第 1 步已提示），到时各加一个 443 vhost 即可，兜底块不用动。
5. **`/api/` 反代启用后**，后端 5xx 会先被 `error_page` 换成 50x.html 再经 CF 返回，行为和你现在 80 上的一致，无需额外改动。

核心文档存档：[Origin CA 创建](https://developers.cloudflare.com/ssl/origin-configuration/origin-ca/)、[Full (strict) 模式](https://developers.cloudflare.com/ssl/origin-configuration/ssl-modes/full-strict/)、[Universal SSL](https://developers.cloudflare.com/ssl/edge-certificates/universal-ssl/)、[强制 HTTPS 最佳实践](https://developers.cloudflare.com/use-cases/solutions/encrypt-all-keep-site-secure/)。按"服务器先就绪 → 再切模式 → 最后开 HSTS"的顺序操作，可以做到全程零中断。
