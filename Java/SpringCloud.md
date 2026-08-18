# Spring Cloud串讲笔记

## 开始微服务开发

### 场景

1. 简介
    用户订单服务系统，完成下单全流程微服务化开发


### 计划框架

1. 图解
    ```mermaid
    flowchart TD
    Client["Vue / Postman"] --> Gateway["sc-gateway"]
    Gateway --> Consumer["member-service-consumer"]
    Consumer --> Provider["member-service-provider"]
    Provider --> DB[("MySQL")]
    Consumer -. "调用接口" .-> API["member-api"]
    Provider -. "实现接口" .-> API
    ```

### 涉及技术栈

1. 服务注册与发现
    - Eureka
    - Nacos

2. 网关
    - Zuul
    - Spring Cloud Gateway

3. 流控
    - Sentinel

4. 分布式事务
    - Seata




## 前期准备

### 创建父工程

#### 父工程框架

1. 使用springboot initilizr创建一个项目，并删除`/src`目录。
2. 修改`pom.xml`，粘贴如下内容

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">

        <modelVersion>4.0.0</modelVersion>

        <!-- 当前工程自身的坐标 -->
        <groupId>com.lcq</groupId>
        <artifactId>e-commerce-center</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <packaging>pom</packaging>

        <name>e-commerce-center</name>
        <description>电商中心微服务父工程</description>

        <!-- 聚合：声明由根工程统一构建的子模块 -->
        <modules>
            <module>member-api</module>
            <module>member-service-provider</module>
            <module>member-service-consumer</module>
            <module>sc-gateway</module>
        </modules>

        <!-- 全局版本和构建参数 -->
        <properties>
            <java.version>17</java.version>
            <maven.compiler.release>${java.version}</maven.compiler.release>
            <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
            <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>

            <spring-boot.version>4.0.7</spring-boot.version>
            <spring-cloud.version>2025.1.2</spring-cloud.version>
            <spring-cloud-alibaba.version>2025.1.0.0</spring-cloud-alibaba.version>

            <mybatis-plus.version>3.5.17</mybatis-plus.version>
            <druid.version>1.2.28</druid.version>

            <maven-compiler-plugin.version>3.15.0</maven-compiler-plugin.version>
            <maven-surefire-plugin.version>3.5.4</maven-surefire-plugin.version>
            <maven-enforcer-plugin.version>3.6.3</maven-enforcer-plugin.version>
        </properties>

        <!-- 只管理依赖版本，不会把这些依赖自动加入子模块 -->
        <dependencyManagement>
            <dependencies>

                <!-- Spring Boot 依赖版本 -->
                <dependency>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-dependencies</artifactId>
                    <version>${spring-boot.version}</version>
                    <type>pom</type>
                    <scope>import</scope>
                </dependency>

                <!-- Spring Cloud 依赖版本 -->
                <dependency>
                    <groupId>org.springframework.cloud</groupId>
                    <artifactId>spring-cloud-dependencies</artifactId>
                    <version>${spring-cloud.version}</version>
                    <type>pom</type>
                    <scope>import</scope>
                </dependency>

                <!-- Spring Cloud Alibaba 依赖版本 -->
                <dependency>
                    <groupId>com.alibaba.cloud</groupId>
                    <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                    <version>${spring-cloud-alibaba.version}</version>
                    <type>pom</type>
                    <scope>import</scope>
                </dependency>

                <!-- MyBatis-Plus 依赖版本 -->
                <dependency>
                    <groupId>com.baomidou</groupId>
                    <artifactId>mybatis-plus-bom</artifactId>
                    <version>${mybatis-plus.version}</version>
                    <type>pom</type>
                    <scope>import</scope>
                </dependency>

                <!-- Boot BOM 不管理 Druid Starter，因此在父工程单独管理 -->
                <dependency>
                    <groupId>com.alibaba</groupId>
                    <artifactId>druid-spring-boot-4-starter</artifactId>
                    <version>${druid.version}</version>
                </dependency>

                <!-- 统一管理项目内部公共 API 模块的版本 -->
                <dependency>
                    <groupId>${project.groupId}</groupId>
                    <artifactId>member-api</artifactId>
                    <version>${project.version}</version>
                </dependency>

            </dependencies>
        </dependencyManagement>

        <build>

            <!-- 管理可选插件；子模块声明后才真正启用 -->
            <pluginManagement>
                <plugins>
                    <plugin>
                        <groupId>org.springframework.boot</groupId>
                        <artifactId>spring-boot-maven-plugin</artifactId>
                        <version>${spring-boot.version}</version>
                        <executions>
                            <execution>
                                <id>repackage</id>
                                <goals>
                                    <goal>repackage</goal>
                                </goals>
                            </execution>
                        </executions>
                    </plugin>
                </plugins>
            </pluginManagement>

            <!-- 所有子模块继承并使用的构建插件 -->
            <plugins>

                <!-- 按 Java 17 编译，并保留方法参数名 -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>${maven-compiler-plugin.version}</version>
                    <configuration>
                        <release>${maven.compiler.release}</release>
                        <parameters>true</parameters>
                        <encoding>${project.build.sourceEncoding}</encoding>
                    </configuration>
                </plugin>

                <!-- 在 test 阶段运行 JUnit 单元测试 -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-surefire-plugin</artifactId>
                    <version>${maven-surefire-plugin.version}</version>
                </plugin>

                <!-- 在构建开始时检查 JDK 和 Maven 版本 -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-enforcer-plugin</artifactId>
                    <version>${maven-enforcer-plugin.version}</version>
                    <executions>
                        <execution>
                            <id>enforce-build-environment</id>
                            <phase>validate</phase>
                            <goals>
                                <goal>enforce</goal>
                            </goals>
                            <configuration>
                                <rules>
                                    <requireJavaVersion>
                                        <version>[17,)</version>
                                    </requireJavaVersion>
                                    <requireMavenVersion>
                                        <version>[3.9.0,)</version>
                                    </requireMavenVersion>
                                </rules>
                            </configuration>
                        </execution>
                    </executions>
                </plugin>

            </plugins>
        </build>

    </project>

    ```

3. 说明
    这里的pom指定了数据库相关模块（MyBatisPlus、Druid）以及SpringBoot、SpringCloud、SpringCloud Alibaba的版本依赖。
    参考链接：
    - https://baomidou.com/getting-started/install/
    - https://github.com/alibaba/druid?tab=readme-ov-file#spring-boot-%E9%A1%B9%E7%9B%AE%E6%8E%A8%E8%8D%90

#### 父工程pom文件解析

### 创建api模块（创建并打包）

#### 创建请求对象

1. 总览
    - `MemberCreateRequest.java`
    - `MemberUpdateRequest.java`
    - `MemberQueryRequest.java`
    
1. `MemberCreateRequest.java`
    ```java
    package com.lcq.memberapi.request;

    import jakarta.validation.constraints.Email;
    import jakarta.validation.constraints.Max;
    import jakarta.validation.constraints.Min;
    import jakarta.validation.constraints.NotBlank;
    import jakarta.validation.constraints.Pattern;
    import jakarta.validation.constraints.Size;
    import lombok.AllArgsConstructor;
    import lombok.Data;
    import lombok.NoArgsConstructor;

    import java.io.Serializable;

    /**
    * 创建会员请求对象
    *
    * @author lcq
    * @version 1.0
    */
    @Data
    @NoArgsConstructor
    @AllArgsConstructor
    public class MemberCreateRequest implements Serializable {

        private static final long serialVersionUID = 1L;

        /**
        * 会员名称
        */
        @NotBlank(message = "会员名称不能为空")
        @Size(max = 50, message = "会员名称不能超过50个字符")
        private String name;

        /**
        * 会员密码
        */
        @NotBlank(message = "会员密码不能为空")
        @Size(
                min = 6,
                max = 64,
                message = "会员密码长度必须在6到64个字符之间"
        )
        private String pwd;

        /**
        * 手机号码
        */
        @Pattern(
                regexp = "^$|^1\\d{10}$",
                message = "手机号码格式不正确"
        )
        private String mobile;

        /**
        * 邮箱
        */
        @Email(message = "邮箱格式不正确")
        @Size(max = 100, message = "邮箱不能超过100个字符")
        private String email;

        /**
        * 性别：0未知，1男，2女
        */
        @Min(value = 0, message = "性别值不能小于0")
        @Max(value = 2, message = "性别值不能大于2")
        private Integer gender;
    }
    ```
1. `MemberQueryRequest.java`
    ```java
    package com.lcq.memberapi.request;

    import jakarta.validation.constraints.Max;
    import jakarta.validation.constraints.Min;
    import jakarta.validation.constraints.Size;
    import lombok.AllArgsConstructor;
    import lombok.Data;
    import lombok.NoArgsConstructor;

    import java.io.Serializable;

    /**
    * 会员分页查询请求对象
    *
    * @author lcq
    * @version 1.0
    */
    @Data
    @NoArgsConstructor
    @AllArgsConstructor
    public class MemberQueryRequest implements Serializable {

        private static final long serialVersionUID = 1L;

        /**
        * 查询关键字
        * 可用于匹配会员名称、手机号或邮箱
        */
        @Size(max = 50, message = "查询关键字不能超过50个字符")
        private String keyword;

        /**
        * 性别：0未知，1男，2女
        */
        @Min(value = 0, message = "性别值不能小于0")
        @Max(value = 2, message = "性别值不能大于2")
        private Integer gender;

        /**
        * 当前页码
        */
        @Min(value = 1, message = "页码不能小于1")
        private Integer pageNum = 1;

        /**
        * 每页数量
        */
        @Min(value = 1, message = "每页数量不能小于1")
        @Max(value = 100, message = "每页数量不能超过100")
        private Integer pageSize = 10;
    }

    ```
1. `MemberUpdateRequest.java`
    ```java
    package com.lcq.memberapi.request;

    import jakarta.validation.constraints.Email;
    import jakarta.validation.constraints.Max;
    import jakarta.validation.constraints.Min;
    import jakarta.validation.constraints.Pattern;
    import jakarta.validation.constraints.Size;
    import lombok.AllArgsConstructor;
    import lombok.Data;
    import lombok.NoArgsConstructor;

    import java.io.Serializable;

    /**
    * 修改会员请求对象
    *
    * @author lcq
    * @version 1.0
    */
    @Data
    @NoArgsConstructor
    @AllArgsConstructor
    public class MemberUpdateRequest implements Serializable {

        private static final long serialVersionUID = 1L;

        /**
        * 会员名称
        */
        @Size(
                min = 1,
                max = 50,
                message = "会员名称长度必须在1到50个字符之间"
        )
        private String name;

        /**
        * 手机号码
        */
        @Pattern(
                regexp = "^$|^1\\d{10}$",
                message = "手机号码格式不正确"
        )
        private String mobile;

        /**
        * 邮箱
        */
        @Email(message = "邮箱格式不正确")
        @Size(max = 100, message = "邮箱不能超过100个字符")
        private String email;

        /**
        * 性别：0未知，1男，2女
        */
        @Min(value = 0, message = "性别值不能小于0")
        @Max(value = 2, message = "性别值不能大于2")
        private Integer gender;
    }
    ```

#### 创建返回包装类

1. `Result.java`
    ```java
    package com.lcq.memberapi.result;

    import lombok.Data;
    import lombok.NoArgsConstructor;

    /**
    * 统一响应结果
    *
    * @param <T> 响应数据类型
    * @author lcq
    * @version 1.0
    */
    @Data
    @NoArgsConstructor
    public class Result<T> {

        private String code;
        private String msg;
        private T data;

        public Result(String code, String msg, T data) {
            this.code = code;
            this.msg = msg;
            this.data = data;
        }

        public static <T> Result<T> success(T data) {
            return new Result<>("200", "success", data);
        }

        public static <T> Result<T> success(String msg, T data) {
            return new Result<>("200", msg, data);
        }

        public static <T> Result<T> created(String msg, T data) {
            return new Result<>("201", msg, data);
        }

        public static <T> Result<T> error(String msg) {
            return new Result<>("500", msg, null);
        }

        public static <T> Result<T> error(String code, String msg) {
            return new Result<>(code, msg, null);
        }
    }
    ```

### 创建consumer模块

### 创建provider模块

### 创建Gateway模块

#### 配置

1. Maven引入
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
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway-server-webmvc</artifactId>
        </dependency>

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-loadbalancer</artifactId>
        </dependency>


    </dependencies>
    ```

2. `application.yml`
    - **注意：不同版本的配置有所不同**
    ```yml
    server:
        port: 80

    spring:
        application:
            name: "sc-gateway"

        profiles:
            active: dev


        cloud:
            nacos:
            config:
                server-addr: localhost:8848

            discovery:
                server-addr: localhost:8848

        config:
            import:
            - "optional:nacos:${spring.application.name}-${spring.profiles.active}.yaml"
    ```

#### 启动类


### 配置启动Nacos（含配置中心）

#### Nacos简介

1. 服务注册与发现

2. 配置中心

#### 启动Nacos

1. 下载（项目使用``）
    - https://github.com/alibaba/nacos/releases/download/1.2.1/nacos-server-1.2.1.zip
1. 傻瓜式启动
    - 双击`/nacos/bin`目录下的`startup.cmd`

3. 说明

    根据官方 GitHub tag，**Windows 的 `startup.cmd` 默认启动模式从 Nacos `1.3.2` 开始改成 `cluster`**。

    即到 `1.3.2` 以后，双击 `startup.cmd` 就不再等价于单机启动了。

    官方解决方案也很明确：单机模式要显式加 `-m standalone`。官方快速开始里 Windows 启动命令就是：

    ```bat
    startup.cmd -m standalone
    ```

    官方部署文档也写了：`standalone` 代表非集群模式，本地测试/单机试用用单机模式；集群模式用于生产环境。参考：[Nacos 快速开始](https://nacos.io/docs/latest/quickstart/quick-start/) 和 [Nacos 支持三种部署模式](https://nacos.io/docs/latest/guide/admin/deployment/)。

    至于为什么不是默认 `localhost`：这不是 Win11 的问题。官方“集群寻址”文档说明，如果没有显式配置寻址方式，Nacos 会先找 `cluster.conf`，再找 `nacos.member.list`，否则走 `address-server`；而 `address.server.domain` 的默认值就是 `jmenv.tbsite.net`。参考：[集群寻址](https://nacos.io/docs/latest/plugin/address-plugin/)。

    你本地最稳的做法就是以后都这样启动，在根目录`/nacos/bin`下运行cmd，然后输入如下命令：

    ```powershell
    .\startup.cmd -m standalone
    ```

    `2.5.2` 同理

#### SpringBoot配置

1. 说明
    - Spring Cloud Alibaba 版本（2025.1.0.0）与 Spring Boot 4.x 的组合，改变了配置加载方式。
        从 Spring Cloud Alibaba 2023.0.1.3 版本开始，Nacos 配置的加载机制发生了重大变更

    - 老版本：自动匹配`${spring.application.name}-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension}`格式的配置文件。

    - 新版本：通过配置`spring.config.import`配置多个配置源。新版本的配置组（group）与命名空间（namespace）均在这个配置下设置。
        **特别的，需要加上`refreshEnable=true`**

2. 配置示例
    ```yaml
    spring:
        application:
            name: member-service-provider

        profiles:
            active: dev

        config:
            import:
            - "optional:nacos:${spring.application.name}-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension}?group=DEFAULT_GROUP&refreshEnabled=true"

        cloud:
            nacos:
                config:
                    server-addr: localhost:8848
                    file-extension: yaml
                    group: 
                    namespace:
                    
                discovery:
                    server-addr: localhost:8848

    ```

3. 取用配置示例
    ```java
    package com.lcq.controller;

    import com.lcq.memberapi.result.Result;

    // 注意这里的@Value不是lombok
    import org.springframework.beans.factory.annotation.Value;
    import org.springframework.cloud.context.config.annotation.RefreshScope;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;

    /**
    * @author lcq
    * @version 1.0
    */

    @RestController
    @RequestMapping("/nacos")
    @RefreshScope
    public class NacosCotroller {

        @Value("${nacos.name}")
        public  String name;

        @RequestMapping("/name")
        public Result name() {

            return Result.success(name);
        }

    }

    ```

4. 【待补充】用配置中心完成配置

#### 配置持久化

1. 所有Nacos配置均不会随重启丢失
    

### 配置启动Seata（含数据库）

#### Seata数据库结构简介

2. 下载与启动
    - 下载链接：https://github.com/apache/incubator-seata/releases/download/v0.9.0/seata-server-0.9.0.zip
    - 启动（完成下一节所示配置后启动）
        ```ps

        ```

#### 基本配置

1. seata配置文件一览
    ```text
    conf/
    ├── file.conf
    ├── registry.conf
    ├── nacos-config.txt
    ├── nacos-config.sh
    ├── nacos-config.py
    ├── db_store.sql
    └── db_undo_log.sql
    ```

    - `file.conf`：Seata运行参数

    - `registry.conf`：注册中心、配置中心在哪里

    - `nacos-config.txt`：当`配置中心=Nacos`时，要上传到Nacos的一整套参数

    - `db_store.sql`：Seata Server数据库建表

    - `db_undo_log.sql`：业务数据库建undo_log
    


1. 创建seata管理数据库`seata_server`
    ```sql
    CREATE DATABASE IF NOT EXISTS `seata_server`
    DEFAULT CHARACTER SET utf8mb4;
    ```

2. 创建管理数据库的表
    - 参考：`/seata/conf/db_store.sql`

2. 修改配置文件`/seata/conf/file.conf`
    - 当前修改的是服务端配置，启动时读取`store`字段
    - 修改数据库相关信息：数据库链接、用户名、密码
    - 修改存储模式为`db`
    ```conf
    ## transaction log store
    store {
    ## store mode: file、db
    mode = "db"

    ## file store
    file {
        dir = "sessionStore"

        # branch session size , if exceeded first try compress lockkey, still exceeded throws exceptions
        max-branch-session-size = 16384
        # globe session size , if exceeded throws exceptions
        max-global-session-size = 512
        # file buffer size , if exceeded allocate new buffer
        file-write-buffer-cache-size = 16384
        # when recover batch read size
        session.reload.read_size = 100
        # async, sync
        flush-disk-mode = async
    }

    ## database store
    db {
        ## the implement of javax.sql.DataSource, such as DruidDataSource(druid)/BasicDataSource(dbcp) etc.
        datasource = "dbcp"
        ## mysql/oracle/h2/oceanbase etc.
        db-type = "mysql"
        driver-class-name = "com.mysql.jdbc.Driver"
        url = "jdbc:mysql://127.0.0.1:3306/seata_server"
        user = "root"
        password = "lcq"
        min-conn = 1
        max-conn = 3
        global.table = "global_table"
        branch.table = "branch_table"
        lock-table = "lock_table"
        query-limit = 100
    }
    }
    ```

3. 修改配置文件`/seata/conf/registry.conf`
    - 这是注册到服务注册中心的相关配置
    - 忽略没有提及的配置，那些是是多余的。
    
    ```conf
    registry {
        # file 、nacos 、eureka、redis、zk、consul、etcd3、sofa
        type = "nacos"

        nacos {
            serverAddr = "localhost:8848"
            namespace = ""
            cluster = "default"
        }
    }

    config {
        # file、nacos 、apollo、zk、consul、etcd3
        type = "file"

        nacos {
            serverAddr = "localhost"
            namespace = ""
        }

        file {
            name = "file.conf"
        }
    }

    ```

4. 【注意事项】
    - `0.9.0`版本在Nacos中可能无法显示正确的服务名，而是显示`serverAddr`，需要用更新版本的Seata，并进行如下配置：
        ```conf
        registry {
            type = "nacos"
            nacos {
                application = "seata-server"
                serverAddr = "127.0.0.1:8848"
                group = "SEATA_GROUP"
                namespace = ""
                cluster = "default"
            }
        }
        ```

4. 【待补充】使用配置中心（即`config.type=nacos`）时的配置方法


#### 创建数据库与表

1. 

2. 为每个数据库与创建`undo_log`表（MySQL数据库）
    参考：
    - https://github.com/apache/incubator-seata/blob/2.x/script/client/at/db/mysql.sql
    - https://seata.apache.org/docs/user/quickstart/#example-powered-by-dubbo--seata
    - 下载资源`/seata/conf/db_undo_log.sql`
    ```sql
    CREATE TABLE IF NOT EXISTS `undo_log`
    (
        `branch_id`     BIGINT       NOT NULL COMMENT 'branch transaction id',
        `xid`           VARCHAR(128) NOT NULL COMMENT 'global transaction id',
        `context`       VARCHAR(128) NOT NULL COMMENT 'undo_log context,such as serialization',
        `rollback_info` LONGBLOB     NOT NULL COMMENT 'rollback info',
        `log_status`    INT(11)      NOT NULL COMMENT '0:normal status,1:defense status',
        `log_created`   DATETIME(6)  NOT NULL COMMENT 'create datetime',
        `log_modified`  DATETIME(6)  NOT NULL COMMENT 'modify datetime',
        UNIQUE KEY `ux_undo_log` (`xid`, `branch_id`)
    ) ENGINE = InnoDB AUTO_INCREMENT = 1 DEFAULT CHARSET = utf8mb4 COMMENT ='AT transaction mode undo table';
    ALTER TABLE `undo_log` ADD INDEX `ix_log_created` (`log_created`);
    ```

#### 使用Mybatis-Plus完成类的创建

#### 完成Seata基本配置

#### 【重要】版本说明

1. 说明
    由于本次使用了较新的`springboot4.0.7`版本，Nacos被迫同步升级至`3.2.0`，同时，为了兼容新版本，Seata升至`2.5.0`版本，本小节基于`2.5.0`版本做笔记

2. 结合当前项目，主要改动是：

    - Seata Server：旧的 `registry.conf + file.conf` 改成 2.5 的 `application.yml`。
    - 注册中心：统一配置 Nacos 的 `application、group、namespace、cluster`。
    - 事务组：把旧的 `my_test_tx_group` 换成项目事务组，并保证客户端与服务端映射一致。
    - 数据库：业务库的 `undo_log` 基本符合新版格式；`seata_server` 中的 `global_table、branch_table、lock_table` 明显偏旧，需要按 2.5 官方 SQL 调整，并补充 `distributed_lock` 等表。
    - 微服务：参与分布式事务的服务需要增加 Seata 客户端配置。现在只有 `member-service-storage` 引入了 Seata Starter，其他参与者后续也要引入。
    - 端口仍可使用 `8091`；Seata 2.5 将 HTTP 和事务端口进行了合并配置。




### 配置启动健康监测

### 配置启动Sentinel


## 服务注册与发现

### 简介

1. 技术介绍

2. 新老技术对比

3. Eureka

4. Nacos

### 【老技术】 Eureka

### Nacos基本配置

#### 配置文件

1. Nacos配置示例
    - 参考：https://sca.aliyun.com/docs/2025.x/user-guide/nacos/quick-start/
    ```yml

    ```



## 配置中心

### 简介

### 【待完善】注意事项

1. 非public属性更新会报错

2. 引入时的`optional:`

### 【重点】分模块访问配置中心

#### 使用Nacos配置中心配置数据源

1. 参考
    - 若依框架的数据源配置：https://gitee.com/dromara/RuoYi-Cloud-Plus/blob/2.X/ruoyi-modules/ruoyi-system/src/main/resources/application.yml

### Nacos 3.2.0 新版本

#### 安装、部署与启动



3. 启动
    - 在学习过程中，一般采用java8，而`Nacos 3.2.0`采用java17进行编译，因此我们需要获取java17，并临时修改环境变量以启动Nacos
    - 下列的`JAVA_HOME=...`需替换成java17的地址。
        ```cmd
        set "JAVA_HOME=C:\Users\Lenovo\.jdks\dragonwell-17.0.19"
        set "PATH=%JAVA_HOME%\bin;%PATH%"

        java -version
        where java
        ```


    - 然后启动Nacos
        ```cmd
        startup.cmd -m standalone
        ```    

## 网关

### 简介

1. 功能简介
    网关作为所有请求的必经之路，承担了每一个请求的鉴权、负载均衡、限流、熔断、服务转发等功能。

2. 文档
    https://docs.spring.io/spring-cloud-gateway/reference/

3. 特点
    - 动态路由
    - 对路由指定Predicate（断言）和Filter（过滤）
    - 服务与发现
    - 请求限流
    - 路径重写

### Spring Cloud Gateway 常用配置

#### 基本配置

#### 路由配置规则

#### 常用断言

#### 常用过滤器


## 负载均衡

## 远程调用

### 简介

#### OpenFeign

1. OpenFeign是一个声明式的WebService客户端。

2. 通过在接口上添加注解可以连接别的服务



### a

1. RPC、RestTemplate、HttpClient、OpenFeign

## 健康管理

## 流控

### Sentinel 简介

### Sentinel 配置

## 分布式事务

### 介绍

1. 简介
    微服务时代，可能会出现多个微服务共同协作的场景，此时一个事务可能横跨多个服务，由此引出分布式事务。

### Seata的工作流程

1. 术语解释
    | 术语      | 中文含义    | 核心职责                               | 通常位于         |
    | ------- | ------- | ---------------------------------- | ------------ |
    | **XID** | 全局事务 ID | 唯一标识一次全局事务，并在微服务调用链中传播             | 请求上下文        |
    | **TC**  | 事务协调器   | 维护全局事务和分支事务状态，协调全局提交或回滚            | Seata Server |
    | **TM**  | 事务管理器   | 划定全局事务边界，开启全局事务，最终决定提交或回滚          | 全局事务发起服务     |
    | **RM**  | 资源管理器   | 管理本地分支事务，注册分支、报告状态，并执行 TC 的提交或回滚指令 | 访问数据库的微服务    |

    - Transaction ID XID: 全局唯一的事务ID
    - Transaction Coordinator(TC) : 事务协调器，维护全局事务的运行状态，负责协调并驱动全局事务的提交或回滚
    - Transaction Manager(TM) : 控制全局事务的边界，负责开启一个全局事务，并最终发起全局提交或全局回滚的决议;
    - Resource Manager(RM) : 控制分支事务，负责分支注册，状态汇报，并接收事务协调器的指令，驱动分支（本地）事务的提交和回滚

2. 关系图
    ```mermaid
    flowchart TB
        TM["TM：事务管理器<br/>开启全局事务<br/>发起提交或回滚"]
        TC["TC：事务协调器<br/>维护全局事务状态<br/>协调所有分支事务"]
        RMA["RM：服务 A<br/>管理分支事务 A"]
        RMB["RM：服务 B<br/>管理分支事务 B"]

        TM -->|"① 申请开启全局事务"| TC
        TC -->|"② 创建并返回 XID"| TM

        TM -.->|"③ XID 随服务调用传播"| RMA
        RMA -.->|"XID 继续传播"| RMB

        RMA -->|"④ 注册分支、报告状态"| TC
        RMB -->|"④ 注册分支、报告状态"| TC

        TM -->|"⑤ 发起全局提交或回滚"| TC
        TC -->|"⑥ 驱动分支提交或回滚"| RMA
        TC -->|"⑥ 驱动分支提交或回滚"| RMB
    ```

3. 流程

    1. TM 向 TC 申请开启全局事务。

    2. TC 创建全局事务，生成全局唯一的 XID，并将 XID 返回给 TM。

    3. XID 随着微服务之间的调用，在整个业务调用链中传播。

    4. 各微服务中的 RM 向 TC 注册分支事务，使这些分支事务归属于同一个 XID。

    5. 各 RM 执行本地数据库操作，并向 TC 汇报分支事务的执行状态。

    6. 全局业务执行结束后，TM 根据执行结果向 TC 发起全局提交或全局回滚决议。

    7. TC 根据 XID 找到其管理的全部分支事务，并通知相应 RM 完成提交或回滚。

3. 图解
    ```mermaid
    sequenceDiagram
        participant O as 订单服务（TM/RM）
        participant TC as Seata Server（TC）
        participant S as 库存服务（RM）
        participant A as 账户服务（RM）

        O->>TC: ① 申请开启全局事务
        TC-->>O: ② 创建并返回 XID

        Note over O,A: XID 随微服务调用链传播

        O->>TC: ③ 注册订单分支事务
        O->>S: ④ 扣减库存（携带 XID）
        S->>TC: ⑤ 注册库存分支事务
        S->>A: ⑥ 扣减余额（携带 XID）
        A->>TC: ⑦ 注册账户分支事务

        alt 所有业务执行成功
            O->>TC: ⑧ 发起全局提交
            TC->>O: 提交订单分支
            TC->>S: 提交库存分支
            TC->>A: 提交账户分支
        else 某个业务发生异常
            O->>TC: ⑧ 发起全局回滚
            TC->>O: 回滚订单分支
            TC->>S: 回滚库存分支
            TC->>A: 回滚账户分支
        end
    ```

### Seata工作原理解析

1. Seata通过一个事务XID来追踪一个事务

2. TC可以是一台机器，也可以是一个集群，他们由配置中的`seata.registry.cluster`参数声明

3. 路由
    - 一次分布式事务的起点（即`@GlobalTransaction`注解的方法）会向TC申请一个XID

    - 客户端通过配置与TC相同的registry（`seata.registry.nacos.server-addr`、`seata.registry.nacos.group`、`seata.registry.nacos.application`）找到对应的TC集群。

    - 此外，客户端通过`tx-service-group`为事务组命名，并通过`service.vgroupMapping.${tx-service-group}`的值映射到服务端`seata.registry.nacos.cluster`值相同的TC集群

    - 开始分布式事务，此时客户端会根据XID开启强制路由，防止同一次事务发送到不同的服务器。

    - Nacos配置如下

    ```properties
    # =========================================================
    # 一、Client 事务组路由配置
    # =========================================================

    # 将 ecommerce_tx_group 事务组映射到 default TC 集群
    service.vgroupMapping.ecommerce_tx_group=default

    # 是否禁用全局事务
    # false：启用Seata全局事务
    # true：禁用Seata全局事务
    service.disableGlobalTransaction=false


    # =========================================================
    # 二、Seata Server存储方式
    # =========================================================

    # 全局事务会话的存储方式
    store.mode=db

    # 全局锁的存储方式
    store.lock.mode=db

    # 全局事务和分支事务会话的存储方式
    store.session.mode=db


    # =========================================================
    # 三、Seata Server数据库配置
    # =========================================================

    # Seata Server使用的数据源连接池
    store.db.datasource=druid

    # 数据库类型
    store.db.dbType=mysql

    # MySQL 8及以上驱动
    store.db.driverClassName=com.mysql.cj.jdbc.Driver

    # Seata Server数据库连接地址
    store.db.url=jdbc:mysql://127.0.0.1:3306/seata_ecommerce_server?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useSSL=false&serverTimezone=Asia/Shanghai

    # 数据库用户名
    store.db.user=root

    # 数据库密码
    store.db.password=你的MySQL密码

    # 初始化连接数
    store.db.minConn=5

    # 最大连接数
    store.db.maxConn=30

    # 获取数据库连接的最大等待时间，单位毫秒
    store.db.maxWait=5000


    # =========================================================
    # 四、Seata Server表名
    # =========================================================

    # 全局事务表
    store.db.globalTable=global_table

    # 分支事务表
    store.db.branchTable=branch_table

    # 全局锁表
    store.db.lockTable=lock_table

    # Seata Server分布式锁表
    store.db.distributedLockTable=distributed_lock

    # 事务组表
    store.db.vgroupTable=vgroup_table

    # 单次查询全局事务的最大数量
    store.db.queryLimit=100


    # =========================================================
    # 五、Seata Server重试与清理配置
    # =========================================================

    # 二阶段提交失败后无限重试
    server.maxCommitRetryTimeout=-1

    # 二阶段回滚失败后无限重试
    server.maxRollbackRetryTimeout=-1

    # 提交状态事务的重试检查间隔，单位毫秒
    server.recovery.committingRetryPeriod=1000

    # 异步提交状态的重试检查间隔
    server.recovery.asynCommittingRetryPeriod=1000

    # 回滚状态的重试检查间隔
    server.recovery.rollbackingRetryPeriod=1000

    # 超时事务检查间隔
    server.recovery.timeoutRetryPeriod=1000

    # undo日志保留天数
    server.undo.logSaveDays=7

    # undo日志清理周期，单位毫秒，86400000为一天
    server.undo.logDeletePeriod=86400000
    ```

4. 分布式事务（一阶段）
    - 客户端通过代理对象拦截业务SQL，找到业务SQL更新的目标，并生成**前置镜像（Before Image）**
    - 执行业务SQL后更新业务数据，并在更新之后保存为**后置镜像（After Image）**
    - 生成行锁，确保数据一致性

5. 分布式事务（二阶段提交）
    - 二阶段如果顺利提交，则将一阶段的快照和行锁删除，完成数据清理。
5. 分布式事务（二阶段回滚）
    - 如果因任何情况造成事务无法顺利完成，则回滚阶段一已经执行的SQL

    - 回滚通过还原前置镜像完成。

    - 回滚前需要校验**脏写**即事务过程中是否有其他操作混入，脏写通过后置镜像校验。

    - 如果脏写确实存在，则后续操作需要转人工核验。





#### ⚙️ 实际执行时，它们是怎么“各取所需”的？

1. **服务端（Seata-Server）启动时**：
   - 连上 Nacos，拉取 DataId 为 `seataServer.properties` 的这份文件。
   - 解析时，只找以 `store.` 和 `server.` 开头的 Key。
   - 看到 `store.db.password=你的MySQL密码`，拿来连接数据库。
   - 看到 `service.vgroupMapping.ecommerce_tx_group=default`，**直接丢弃**（因为它不认识也不需要）。

2. **客户端（订单微服务）启动时**：
   - 连上同一个 Nacos，拉取**同一份** `seataServer.properties` 文件。
   - 解析时，只找以 `service.`（特别是 `service.vgroupMapping`）和 `client.` 开头的 Key。
   - 看到 `service.vgroupMapping.ecommerce_tx_group=default`，结合本地的 `seata.tx-service-group=ecommerce_tx_group`，得知要去 `default` 集群找 TC。
   - 看到 `store.db.password=你的MySQL密码`，**直接忽略**（它连上也没用，因为它不直接连 Seata 的库）。

#### 💡 为什么非要放在“同一个文件”里？

如果你把它们拆开，比如把 `store.*` 放在一个文件，`service.vgroupMapping` 放在另一个文件，Seata 也支持（配置中心支持多 DataId），但官方推荐放一起是因为：

1. **简化运维**：只维护一个 DataId（`seataServer.properties`），版本号清晰。
2. **消除依赖**：客户端和服务端只要连上同一个 Nacos，拉取同一个配置，就能保证路由映射和存储策略始终来自同一个“快照”，不会出现客户端映射是旧版本、服务端存储是新版本的错乱情况。


### Seata四种工作模式

Seata 的四大分布式事务模式，核心区别在于 **“一致性”**与 **“性能/灵活性”** 之间的权衡。可以按业务侵入程度从低到高来理解它们：

1. AT 模式 (Automatic Transaction) —— 自动挡，无侵入

    *   **一句话概括**：对业务代码零侵入，就像给本地事务加了“自动挡”。你只管写业务 SQL，Seata 自动帮你管理分布式事务的提交和回滚。
    *   **核心机制**：基于支持本地 ACID 事务的关系型数据库。它通过代理数据源，在执行 SQL 前后自动生成数据快照（`undo_log`）。一阶段直接提交本地事务并释放锁；二阶段若需回滚，则根据快照生成反向 SQL 进行补偿。
    *   **关键特性**：**高性能**（一阶段释放资源），**写隔离**（通过全局锁防脏写）。默认全局**读未提交**，可用 `SELECT ... FOR UPDATE` 升级为**读已提交**。
    *   **适用场景**：**绝大多数微服务场景**，追求高性能和低改造成本，可接受极短暂的数据不一致（最终一致性）。

2. TCC 模式 (Try-Confirm-Cancel) —— 手动挡，高性能

    *   **一句话概括**：业务代码侵入性强，但性能最高。开发者需手动实现 **Try（资源检查/预留）**、**Confirm（确认执行业务）** 和 **Cancel（释放预留资源）** 三个接口。
    *   **核心机制**：TCC 是服务化的两阶段提交。**Try** 作为一阶段负责资源预留；**Confirm/Cancel** 作为二阶段，根据全局事务结果全部执行或全部回滚。
    *   **关键特性**：**不依赖数据库事务**，可跨数据库、跨中间件。**无全局锁**，并发能力最强。但开发成本高，需处理幂等、空回滚和悬挂等问题。
    *   **适用场景**：**核心系统**等对性能有极致要求、业务逻辑可拆分为预留-确认-取消的场景。

3. SAGA 模式 —— 长事务，异步补偿

    *   **一句话概括**：专为**长事务**设计的解决方案，通过**事件驱动**和**异步执行**来保证最终一致性。
    *   **核心机制**：将长事务拆分为一系列本地短事务，每个事务都有对应的**补偿操作**。各参与者依次执行正向操作并提交本地事务；若某步失败，则**反向执行**已成功操作的补偿服务。
    *   **关键特性**：**不占用数据库锁**，性能高。**不保证隔离性**。Seata 通过**状态机引擎**来定义和执行服务流程。
    *   **适用场景**：**业务流程长、参与者多**，或涉及无法提供 TCC 三个接口的第三方/遗留系统。

4. XA 模式 —— 强一致性，但性能较低

    *   **一句话概括**：利用数据库自身对 **XA 协议**的支持来实现**强一致性**的分布式事务。
    *   **核心机制**：严格遵循数据库层面的两阶段提交（2PC）。事务资源（如数据库）在 **prepare** 阶段会阻塞并锁定资源，直到收到全局的 commit 或 rollback 指令。
    *   **关键特性**：**业务无侵入**，能保证**强一致性**和严格的**隔离性**。但**性能最差**，因为资源锁定周期长。
    *   **适用场景**：对数据**一致性要求极严苛**的金融核心场景，或需要从基于 XA 的老应用平滑迁移。

📊 四大模式对比总结

| 模式 | 业务侵入性 | 性能 | 一致性 | 适用场景 |
| :--- | :--- | :--- | :--- | :--- |
| **AT 模式** | **无侵入** | 高 | 最终一致性 | **绝大多数微服务场景** |
| **TCC 模式** | **高侵入** | **最高** | 最终一致性 | 核心系统，对性能有极致要求 |
| **SAGA 模式** | **高侵入** | 高 | 最终一致性 | **长事务**，涉及多方/遗留系统 |
| **XA 模式** | **无侵入** | **最低** | **强一致性** | 金融等对一致性要求极严苛的场景 |

总的来说，**AT 模式是首选**，能覆盖大部分需求。当 AT 模式的性能或隔离级别无法满足时，才需要考虑其他模式。

### Seata 2.5.0 新版本

#### 下载

1. 链接
    https://seata.apache.org/zh-cn/release-history/seata-server/

2. 插件下载
    由于Nacos 3.2.0 移除了 v1/v2 API，所以需要下载插件并重启Nacos，以适配Seata。

    https://github.com/nacos-group/nacos-api-legacy-adapter/releases/download/3.2.0.2/nacos-api-legacy-adapter-3.2.0.jar

    下载后复制到Nacos软件的`/nacos/plugins/`目录下，然后重启。

3. 启动时，如有乱码，则先执行：
    ```cmd
    set "JAVA_OPTS=%JAVA_OPTS% -Dfile.encoding=UTF-8"
    ```

#### 初始化

1. 创建数据库

2. 数据库源码
    新版Seata数据库与老版本不同，为防止不兼容，保留原数据库。

    ```sql
    --
    -- Licensed to the Apache Software Foundation (ASF) under one or more
    -- contributor license agreements.  See the NOTICE file distributed with
    -- this work for additional information regarding copyright ownership.
    -- The ASF licenses this file to You under the Apache License, Version 2.0
    -- (the "License"); you may not use this file except in compliance with
    -- the License.  You may obtain a copy of the License at
    --
    --     http://www.apache.org/licenses/LICENSE-2.0
    --
    -- Unless required by applicable law or agreed to in writing, software
    -- distributed under the License is distributed on an "AS IS" BASIS,
    -- WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    -- See the License for the specific language governing permissions and
    -- limitations under the License.
    --

    -- -------------------------------- The script used when storeMode is 'db' --------------------------------
    -- the table to store GlobalSession data

    CREATE DATABASE IF NOT EXISTS `seata_server_250`
        DEFAULT CHARACTER SET utf8mb4;

    use `seata_server_250`;

    CREATE TABLE IF NOT EXISTS `global_table`
    (
        `xid`                       VARCHAR(128) NOT NULL,
        `transaction_id`            BIGINT,
        `status`                    TINYINT      NOT NULL,
        `application_id`            VARCHAR(32),
        `transaction_service_group` VARCHAR(32),
        `transaction_name`          VARCHAR(128),
        `timeout`                   INT,
        `begin_time`                BIGINT,
        `application_data`          VARCHAR(2000),
        `gmt_create`                DATETIME,
        `gmt_modified`              DATETIME,
        PRIMARY KEY (`xid`),
        KEY `idx_status_gmt_modified` (`status` , `gmt_modified`),
        KEY `idx_transaction_id` (`transaction_id`)
    ) ENGINE = InnoDB
    DEFAULT CHARSET = utf8mb4;

    -- the table to store BranchSession data
    CREATE TABLE IF NOT EXISTS `branch_table`
    (
        `branch_id`         BIGINT       NOT NULL,
        `xid`               VARCHAR(128) NOT NULL,
        `transaction_id`    BIGINT,
        `resource_group_id` VARCHAR(32),
        `resource_id`       VARCHAR(256),
        `branch_type`       VARCHAR(8),
        `status`            TINYINT,
        `client_id`         VARCHAR(64),
        `application_data`  VARCHAR(2000),
        `gmt_create`        DATETIME(6),
        `gmt_modified`      DATETIME(6),
        PRIMARY KEY (`branch_id`),
        KEY `idx_xid` (`xid`)
    ) ENGINE = InnoDB
    DEFAULT CHARSET = utf8mb4;

    -- the table to store lock data
    CREATE TABLE IF NOT EXISTS `lock_table`
    (
        `row_key`        VARCHAR(128) NOT NULL,
        `xid`            VARCHAR(128),
        `transaction_id` BIGINT,
        `branch_id`      BIGINT       NOT NULL,
        `resource_id`    VARCHAR(256),
        `table_name`     VARCHAR(32),
        `pk`             VARCHAR(36),
        `status`         TINYINT      NOT NULL DEFAULT '0' COMMENT '0:locked ,1:rollbacking',
        `gmt_create`     DATETIME,
        `gmt_modified`   DATETIME,
        PRIMARY KEY (`row_key`),
        KEY `idx_status` (`status`),
        KEY `idx_branch_id` (`branch_id`),
        KEY `idx_xid` (`xid`)
    ) ENGINE = InnoDB
    DEFAULT CHARSET = utf8mb4;

    CREATE TABLE IF NOT EXISTS `distributed_lock`
    (
        `lock_key`       CHAR(20) NOT NULL,
        `lock_value`     VARCHAR(20) NOT NULL,
        `expire`         BIGINT,
        primary key (`lock_key`)
    ) ENGINE = InnoDB
    DEFAULT CHARSET = utf8mb4;

    INSERT INTO `distributed_lock` (lock_key, lock_value, expire) VALUES ('AsyncCommitting', ' ', 0);
    INSERT INTO `distributed_lock` (lock_key, lock_value, expire) VALUES ('RetryCommitting', ' ', 0);
    INSERT INTO `distributed_lock` (lock_key, lock_value, expire) VALUES ('RetryRollbacking', ' ', 0);
    INSERT INTO `distributed_lock` (lock_key, lock_value, expire) VALUES ('TxTimeoutCheck', ' ', 0);


    CREATE TABLE IF NOT EXISTS `vgroup_table`
    (
        `vGroup`    VARCHAR(255),
        `namespace` VARCHAR(255),
        `cluster`   VARCHAR(255),
    UNIQUE KEY `idx_vgroup_namespace_cluster` (`vGroup`,`namespace`,`cluster`)
    ) ENGINE = InnoDB
    DEFAULT CHARSET = utf8mb4;
    ```

3. 修改启动配置 `application.yml`

    - 配置位于 `\seata-server\conf`，此处配置仅为示例，可参照同目录下配置自行编写
    - 将注册方式改为 nacos
    - 将存储方式改为 db

    ```yaml
    #
    # Licensed to the Apache Software Foundation (ASF) under one or more
    # contributor license agreements. See the NOTICE file distributed with
    # this work for additional information regarding copyright ownership.
    # The ASF licenses this file to You under the Apache License, Version 2.0
    # (the "License"); you may not use this file except in compliance with
    # the License. You may obtain a copy of the License at
    #
    #     http://www.apache.org/licenses/LICENSE-2.0
    #
    # Unless required by applicable law or agreed to in writing, software
    # distributed under the License is distributed on an "AS IS" BASIS,
    # WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    # See the License for the specific language governing permissions and
    # limitations under the License.
    #

    server:
      port: 8091

    spring:
      application:
        name: seata-server
      main:
        web-application-type: none

    logging:
      config: classpath:logback-spring.xml
      file:
        path: ${log.home:${user.home}/logs/seata}
      extend:
        logstash-appender:
          # off by default
          enabled: false
          destination: 127.0.0.1:4560
        kafka-appender:
          # off by default
          enabled: false
          bootstrap-servers: 127.0.0.1:9092
          topic: logback_to_logstash
          producer:
            acks: 0
            linger-ms: 1000
            max-block-ms: 0
        metric-appender:
          # off by default
          enabled: false

    seata:
      config:
        # support: nacos, consul, apollo, zk, etcd3
        type: file

      registry:
        # support: nacos, eureka, redis, zk, consul, etcd3, sofa
        type: nacos
        nacos:
          application: seata-server
          server-addr: 127.0.0.1:8848
          group: SEATA_GROUP
          namespace:
          cluster: default
          context-path:
          ## 1. The following configuration is for the open source version of Nacos
          username:
          password:
          ## 2. The following configuration is for the MSE Nacos on Aliyun
          # access-key:
          # secret-key:
          ## 3. The following configuration is used to deploy on Aliyun ECS or ACK without authentication
          # ram-role-name:

      store:
        # support: file, db, redis, raft
        mode: db
        session:
          mode: db
        lock:
          mode: db

        db:
          datasource: druid
          db-type: mysql
          driver-class-name: com.mysql.jdbc.Driver
          url: jdbc:mysql://127.0.0.1:3306/seata_server250?rewriteBatchedStatements=true
          user: root
          password: lcq
          min-conn: 10
          max-conn: 100
          global-table: global_table
          branch-table: branch_table
          lock-table: lock_table
          distributed-lock-table: distributed_lock
          vgroup-table: vgroup_table
          query-limit: 1000
          max-wait: 5000
          druid:
            time-between-eviction-runs-millis: 120000
            min-evictable-idle-time-millis: 300000
            test-while-idle: true
            test-on-borrow: false
            keep-alive: false

    #  server:
    #    service-port: 8091 # If not configured, the default is '${server.port} + 1000'
    ```

#### 【进阶】用 Nacos 配置中心管理 Seata 配置

1. 介绍

    可以将除了注册中心与配置中心以外的配置放在 Nacos 中，以便复用。

2. 本地 `application.yml`

    ```yaml
    seata:
      config:
        # support: nacos, consul, apollo, zk, etcd3
        type: nacos
        nacos:
          server-addr: 127.0.0.1:8848
          namespace:
          group: SEATA_GROUP
          context-path:
          ## 1. The following configuration is for the open source version of Nacos
          username:
          password:
          ## 2. The following configuration is for the MSE Nacos on Aliyun
          # access-key:
          # secret-key:
          ## 3. The following configuration is used to deploy on Aliyun ECS or ACK without authentication
          # ram-role-name:
          data-id: seata250.yaml

      registry:
        # support: nacos, eureka, redis, zk, consul, etcd3, sofa
        type: nacos
        nacos:
          application: seata-server
          server-addr: 127.0.0.1:8848
          group: SEATA_GROUP
          namespace:
          cluster: default
          context-path:
          ## 1. The following configuration is for the open source version of Nacos
          username:
          password:
          ## 2. The following configuration is for the MSE Nacos on Aliyun
          # access-key:
          # secret-key:
          ## 3. The following configuration is used to deploy on Aliyun ECS or ACK without authentication
          # ram-role-name:
    ```

3. 其他配置可以放到Nacos对应的配置中

4. 注意事项
    - 此时的配置格式不同于之前的seata作为最外层，而是只读取内部Seata配置

### 【新版本】客户端配置

#### 类编写

1. 分布式事务方法

    ```java
    // 添加注解，写明分布式事务的名称以及回滚条件
    @GlobalTransactional(name = "create-order", rollbackFor = Exception.class)
    public OrderVO saveOrder(OrderCreateRequest request){...}
    ```

2. 主启动类
    ```java
    @SpringBootApplication
    @EnableDiscoveryClient
    @EnableFeignClients
    public class MemberServiceOrderApplication {

        public static void main(String[] args) {
            SpringApplication.run(MemberServiceOrderApplication.class, args);
        }

    }

    ```

#### 配置编写

1. `application.yml`

    参考前面章节

2. `pom.xml`
    ```xml
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
    </dependency>    
    ```

#### 配置代理数据源

1. 从`0.9.0`版本开始，Seata支持自动代理，通过修改参数开启
    ```properties
    # 默认为false
    support.spring.datasource.autoproxy=true
    ```

2. `1.0.0`版本开始，配置发生变动，修改为：
    ```properties
    client.support.spring.datasource.autoproxy=true
    ```

3. 若不使用自动代理，则需要手动编写配置类
    - 修改数据源为代理对象
    - 修改来自MyBatis的`SqlSessionFactory`
    ```java
    @Configuration
    public class DataSourceProxyConfig {

        @Bean("druidDataSource")
        @ConfigurationProperties(prefix = "spring.datasource")
        public DataSource druidDataSource() {
            return new DruidDataSource();
        }

        @Primary
        @Bean("dataSource")
        public DataSource dataSource(
                @Qualifier("druidDataSource") DataSource dataSource) {
            return new DataSourceProxy(dataSource);
        }
        @Bean
        public SqlSessionFactory sqlSessionFactory(
            DataSourceProxy dataSourceProxy) throws Exception {

            SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
            factoryBean.setDataSource(dataSourceProxy);
            factoryBean.setMapperLocations(
                new PathMatchingResourcePatternResolver()
                    .getResources(mapperLocations)
            );
            factoryBean.setTransactionFactory(
                new SpringManagedTransactionFactory()
            );

            return factoryBean.getObject();
        }
    }          
    ```

4. `1.1.1`版本后`seata-all`使用注解开启自动代理
    ```java
    @EnableAutoDataSourceProxy
    @SpringBootApplication
    public class StorageApplication {

        public static void main(String[] args) {
            SpringApplication.run(StorageApplication.class, args);
        }
    }
    ```

    - 配置
    ```yaml
    seata:
      enabled: true
  
      # 默认就是 true，可以不写
      enable-auto-data-source-proxy: true
  
      # 默认 AT，根据需要设置
      data-source-proxy-mode: AT
    ```


### 注意事项

#### 前端控制台（Seata-Console）

1. `Seata 1.5.0`首次加入 Seata-Console，可查询全局事务、分支事务和全局锁

2. 自`2.4.0`版本开始，Console移入`seata-namingserver`模块

3. 自`2.5.0`版本开始，Seata Server移除Spring Web，事务和Http合并到端口`8091`，控制台由8081端口提供。

4. 据`2.5.0`最新版本消息，此功能前端页面未直接打包进二进制包，而是需要手动从源码构建，当前功能慎用。

#### 配置与启动

1. 由于新版Seata只提供了框架，所以需要手动向`/seata-server/bin/`目录下添加与Spring版本仲裁版本一致的Mysql的jar包，如：
    ```text
    C:\Users\Lenovo\.m2\repository\com\mysql\mysql-connector-j\9.7.0\mysql-connector-j-9.7.0.jar
    ```

2. 当配置改为从Nacos获取时，由于缺少了SpringBoot对配置文件的宽松绑定，故需手动调整诸如以下配置变量：
    ```text
    tx-service-group  → txServiceGroup
    server-addr       → serverAddr
    data-id           → dataId
    vgroup-mapping    → vgroupMapping
    ```
    - 这一步十分重要，因为Seata内部的配置解析器没有将横杠转换为驼峰的能力，所以不改会报错，当然，这个问题不排除会在高版本解决。

    ```yaml
    # ===== 原配置（保留，用于对照） =====
    # store:
    #   # support: file 、 db 、 redis 、 raft
    #   mode: db
    #   session:
    #     mode: db
    #   lock:
    #     mode: db
    #
    #   db:
    #     datasource: druid
    #     db-type: mysql
    #     driver-class-name: com.mysql.jdbc.Driver
    #     url: jdbc:mysql://127.0.0.1:3306/seata_server250?rewriteBatchedStatements=true
    #     user: root
    #     password: lcq
    #     min-conn: 10
    #     max-conn: 100
    #     global-table: global_table
    #     branch-table: branch_table
    #     lock-table: lock_table
    #     distributed-lock-table: distributed_lock
    #     vgroup-table: vgroup_table
    #     query-limit: 1000
    #     max-wait: 5000
    #     druid:
    #       time-between-eviction-runs-millis: 120000
    #       min-evictable-idle-time-millis: 300000
    #       test-while-idle: true
    #       test-on-borrow: false
    #       keep-alive: false
    #
    # #  server:
    # #    service-port: 8091 #If not configured, the default is '${server.port} + 1000'
    
    # ===== 新配置（按 Seata 2.5.0 ConfigurationKeys 源码键名） =====
    store:
      # support: file 、 db 、 redis 、 raft
      mode: db
      session:
        mode: db
      lock:
        mode: db
    
      db:
        datasource: druid
        dbType: mysql
        driverClassName: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://127.0.0.1:3306/seata_server_250?rewriteBatchedStatements=true
        user: root
        password: lcq
        minConn: "10"
        maxConn: "100"
        globalTable: global_table
        branchTable: branch_table
        lockTable: lock_table
        distributedLockTable: distributed_lock
        vgroupTable: vgroup_table
        queryLimit: "1000"
        maxWait: "5000"
        druid:
          timeBetweenEvictionRunsMillis: "120000"
          minEvictableIdleTimeMillis: "300000"
          testWhileIdle: "true"
          testOnBorrow: "false"
          keepAlive: "false"
    
    ```



## 其他

### Maven 项目构建

1. 简介
    构建`member-api`的基本过程



# ？？？

