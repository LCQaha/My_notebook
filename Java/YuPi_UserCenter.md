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

### 技术选型

1. 前端
    - 三件套
    - React
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

## 后端准备

### 项目初始化

1. 通过`https://start.spring.io`完成项目初始化

2. 选择的扩展
    ```xml
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <scope>annotationProcessor</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>3.0.5</version>
        </dependency>
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <scope>annotationProcessor</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter-test</artifactId>
            <version>3.0.5</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-spring-boot4-starter</artifactId>
            <version>3.5.15</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-3-starter</artifactId>
            <version>1.2.21</version>
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

## 后端业务

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

