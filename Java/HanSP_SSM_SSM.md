# 【韩顺平主流框架】SSM 整合项目

写在前面：
- 本笔记承接已经整理好的 `Spring`、`SpringMVC`、`MyBatis` 三部分笔记，重点不再重复单个框架的底层原理，而是把三者放回一个完整项目中，理解它们如何协作。
- 本章项目采用 **Vue3 + ElementPlus + Axios + SSM + MySQL** 的前后端分离结构。学习重点不是把页面做得多漂亮，而是打通：前端请求、后端接口、业务层、持久层、数据库、统一返回、分页、校验。
- 这份笔记按照之前笔记的风格整理：先讲需求和图景，再讲配置与代码，再补充工程避坑和现代项目延伸。

## PRE

### 课程定位

1. SSM 指的是：
    - `SpringMVC`：Web 层框架，负责接收 HTTP 请求、参数绑定、返回 JSON。
    - `Spring`：业务容器，负责 IOC、DI、事务管理、整合各类对象。
    - `MyBatis`：持久层框架，负责执行 SQL、完成 Mapper 接口与 XML 映射。

2. 本项目技术栈
    | 层次 | 技术 | 作用 |
    |---|---|---|
    | 前端 | Vue3 | 构建单页应用和组件化页面 |
    | UI | ElementPlus | 提供表格、按钮、分页、表单、弹窗等组件 |
    | HTTP | Axios | 前端向后端发送 Ajax 请求 |
    | Web 层 | SpringMVC | 暴露接口、参数绑定、JSON 响应 |
    | 业务层 | Spring | 管理 Service、事务、依赖注入 |
    | 持久层 | MyBatis | 操作 MySQL 数据库 |
    | 数据库 | MySQL | 保存家居商品数据 |
    | 构建 | Maven | 管理后端依赖 |
    | 分页 | PageHelper | 后端物理分页 |
    | 代码生成 | MyBatis Generator | 根据表反向生成 Bean、Mapper、Mapper.xml |

3. 项目目标
    ```text
    后台管理页面
            ↓
    家居信息新增 / 查询 / 修改 / 删除
            ↓
    分页显示
            ↓
    条件分页查询
            ↓
    前端表单校验 + 后端表单校验
    ```

### SSM 整体执行流程

1. 一次“新增家居”的完整链路

    ```text
    Vue 表单
        ↓ axios.post('/api/save', form)
    SpringMVC Controller
        ↓ 参数绑定 @RequestBody Furn
    Spring Service
        ↓ 调用 save(furn)，事务控制
    MyBatis Mapper
        ↓ insertSelective(furn)
    MySQL furn 表
        ↓ 返回影响行数 / 结果
    Msg.success() / Msg.fail()
        ↓ JSON
    Vue 根据 code 刷新表格或显示错误
    ```

2. 三层结构在本项目中的落地

    ```text
    com.hspedu.furns.controller   Web 层：接请求，返回 JSON
    com.hspedu.furns.service      业务层：业务规则，事务边界
    com.hspedu.furns.dao          持久层：Mapper 接口，执行 SQL
    com.hspedu.furns.bean         实体类 / 返回消息类
    resources/mapper              Mapper.xml
    ```

3. 开发顺序建议
    - 先后端，后前端。
    - 后端每写一层就测试一层。
    - `dao -> service -> controller -> postman -> vue`。
    - 不要一口气把前后端都写完再排错，否则很难定位问题。

## 项目基础环境搭建

### 创建 Maven Web 项目

1. 创建 Maven 项目
    - 使用 `maven-archetype-webapp` 创建 Web 工程。
    - 配置 Maven 镜像和本地仓库，避免依赖下载失败。

2. 标准目录结构
    ```text
    furns_ssm
    ├── pom.xml
    ├── src
    │   ├── main
    │   │   ├── java
    │   │   │   └── com.hspedu.furns
    │   │   │       ├── bean
    │   │   │       ├── controller
    │   │   │       ├── dao
    │   │   │       └── service
    │   │   ├── resources
    │   │   │   ├── applicationContext.xml
    │   │   │   ├── jdbc.properties
    │   │   │   ├── mybatis-config.xml
    │   │   │   └── mapper
    │   │   └── webapp
    │   │       └── WEB-INF
    │   │           ├── web.xml
    │   │           └── dispatcher-servlet.xml
    │   └── test
    │       └── java
    ```

3. 编译版本
    ```xml
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>
    ```

### Maven 依赖

1. 基础依赖
    ```xml
    <dependencies>
        <!-- JUnit 测试 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>

        <!-- SpringMVC，会引入 Spring Web 相关包 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.8</version>
        </dependency>

        <!-- Spring JDBC / 事务支持 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.3.8</version>
        </dependency>

        <!-- Spring AOP / 声明式事务切面 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>5.3.8</version>
        </dependency>

        <!-- MyBatis 核心 -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.7</version>
        </dependency>

        <!-- MyBatis 整合 Spring -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.6</version>
        </dependency>

        <!-- Druid 数据库连接池 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.6</version>
        </dependency>

        <!-- MySQL 驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.49</version>
        </dependency>
    </dependencies>
    ```

2. 后续功能依赖
    ```xml
    <!-- PageHelper 分页插件 -->
    <dependency>
        <groupId>com.github.pagehelper</groupId>
        <artifactId>pagehelper</artifactId>
        <version>5.2.1</version>
    </dependency>

    <!-- JSR303 后端数据校验 -->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-validator</artifactId>
        <version>6.1.0.Final</version>
    </dependency>
    ```

3. 依赖理解
    - `spring-webmvc`：Web 层入口，含 `DispatcherServlet`。
    - `spring-jdbc`：提供事务管理器 `DataSourceTransactionManager`。
    - `spring-aspects`：支持基于 AOP 的声明式事务。
    - `mybatis-spring`：把 MyBatis 的 `SqlSessionFactory`、Mapper 代理对象交给 Spring 管理。
    - `druid`：连接池，避免频繁创建数据库连接。
    - `pagehelper`：基于 MyBatis 拦截器实现分页。
    - `hibernate-validator`：实现注解式后端参数校验。

## web.xml 全局配置

### 作用说明

1. `web.xml` 是传统 JavaWeb 项目的全局入口配置文件。
2. 在本项目中，它主要做三件事：
    - 启动 Spring 根容器。
    - 注册 SpringMVC 的 `DispatcherServlet`。
    - 配置字符编码过滤器和 REST 风格请求过滤器。

### 完整配置骨架

```xml
<web-app>
    <display-name>Archetype Created Web Application</display-name>

    <!-- 1. 启动 Spring 容器：主要管理 Service、Mapper、数据源、事务等 -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>

    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!-- 2. SpringMVC 前端控制器：接收所有应用请求 -->
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!-- 3. 字符编码过滤器：必须尽量放在所有过滤器前面 -->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>

    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!-- 4. HiddenHttpMethodFilter：让 POST 请求转换成 PUT / DELETE -->
    <filter>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

### 避坑说明

1. `DispatcherServlet` 的 `<url-pattern>` 推荐写 `/`，不要写 `/*`。
    - `/`：拦截绝大多数请求，但不会强行拦截 JSP 转发。
    - `/*`：容易把 JSP / 内部转发也拦截掉，导致 404。

2. 如果没有显式指定 SpringMVC 配置文件，默认规则是：
    ```text
    /WEB-INF/dispatcher-servlet.xml
    ```
    因为 `servlet-name` 是 `dispatcher`。

3. `ContextLoaderListener` 加载的是 Spring 根容器；`DispatcherServlet` 加载的是 SpringMVC 子容器。两者分工不同。

## SpringMVC 配置

### dispatcher-servlet.xml

1. 位置
    ```text
    src/main/webapp/WEB-INF/dispatcher-servlet.xml
    ```

2. 作用
    - 管理 Web 层组件。
    - 扫描 `@Controller`。
    - 配置视图解析器。
    - 开启 SpringMVC 注解驱动。
    - 放行静态资源。

3. 基本配置
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xmlns:mvc="http://www.springframework.org/schema/mvc"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           https://www.springframework.org/schema/context/spring-context.xsd
           http://www.springframework.org/schema/mvc
           https://www.springframework.org/schema/mvc/spring-mvc.xsd">

        <!-- SpringMVC 只扫描 Controller -->
        <context:component-scan base-package="com.hspedu" use-default-filters="false">
            <context:include-filter type="annotation"
                expression="org.springframework.stereotype.Controller"/>
        </context:component-scan>

        <!-- 视图解析器：传统页面跳转使用。前后端分离后存在感降低 -->
        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            <property name="prefix" value="/WEB-INF/views/"/>
            <property name="suffix" value=".html"/>
        </bean>

        <!-- 静态资源交给 Tomcat 处理 -->
        <mvc:default-servlet-handler/>

        <!-- 支持注解驱动、JSON 转换、参数绑定、校验等高级功能 -->
        <mvc:annotation-driven/>
    </beans>
    ```

### 测试 Controller

```java
@Controller
public class TestController {

    @RequestMapping(value = "/hi")
    public String hi() {
        System.out.println("TestController hi()");
        return "hi";
    }
}
```

1. 访问路径
    ```text
    http://localhost:8080/ssm/hi
    ```

2. 返回逻辑视图名 `hi` 后，会被视图解析器拼接成：
    ```text
    /WEB-INF/views/hi.html
    ```

3. 前后端分离后，更多使用：
    ```java
    @ResponseBody
    @GetMapping("/xxx")
    public Msg xxx() { ... }
    ```
    或直接使用：
    ```java
    @RestController
    ```

## Spring 和 MyBatis 整合

### applicationContext.xml

1. 位置
    ```text
    src/main/resources/applicationContext.xml
    ```

2. 作用
    - 管理非 Web 层对象。
    - 扫描 Service、Dao 等组件，但排除 Controller。
    - 配置数据源。
    - 配置 MyBatis 的 `SqlSessionFactory`。
    - 扫描 Mapper 接口。
    - 配置声明式事务。

### 组件扫描

```xml
<context:component-scan base-package="com.hspedu">
    <context:exclude-filter type="annotation"
        expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

1. 为什么要排除 Controller？
    - Controller 属于 SpringMVC 子容器管理。
    - Service、Mapper、事务属于 Spring 根容器管理。
    - 如果两边重复扫描，可能导致对象重复创建、依赖关系混乱。

### 数据库连接配置

1. `jdbc.properties`
    ```properties
    jdbc.userName=root
    jdbc.password=hsp
    jdbc.driverClass=com.mysql.jdbc.Driver
    jdbc.url=jdbc:mysql://localhost:3306/furns_ssm?useSSL=true&useUnicode=true&characterEncoding=UTF-8
    ```

2. Druid 数据源
    ```xml
    <context:property-placeholder location="classpath:jdbc.properties"/>

    <bean id="pooledDataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="url" value="${jdbc.url}"/>
        <property name="driverClassName" value="${jdbc.driverClass}"/>
        <property name="username" value="${jdbc.userName}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
    ```

3. 注意
    - XML 中 URL 如果直接写 `&`，要转义为 `&amp;`。
    - properties 文件中一般直接写 `&`。
    - 课程使用 MySQL 5 驱动，驱动类是 `com.mysql.jdbc.Driver`；如果换 MySQL 8，通常使用 `com.mysql.cj.jdbc.Driver`，还要注意 `serverTimezone`。

### SqlSessionFactoryBean

```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <!-- MyBatis 全局配置文件 -->
    <property name="configLocation" value="classpath:mybatis-config.xml"/>

    <!-- 数据源交给 Spring 管理 -->
    <property name="dataSource" ref="pooledDataSource"/>

    <!-- Mapper.xml 文件位置 -->
    <property name="mapperLocations" value="classpath:mapper/*.xml"/>
</bean>
```

1. 传统 MyBatis 写法需要手动创建：
    ```text
    SqlSessionFactoryBuilder -> SqlSessionFactory -> SqlSession -> Mapper
    ```

2. SSM 整合后：
    - `SqlSessionFactoryBean` 创建 `SqlSessionFactory`。
    - Mapper 接口由 Spring 扫描并放入 IOC 容器。
    - Service 直接注入 Mapper。

### mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 可配置类型别名、插件等 -->
</configuration>
```

后续加入 PageHelper 时，需要在这里配置插件。

### MapperScannerConfigurer

```xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.hspedu.furns.dao"/>
</bean>
```

1. 作用
    - 扫描 `dao` 包下的 Mapper 接口。
    - 根据接口和 XML 动态生成代理对象。
    - 将代理对象放入 Spring IOC 容器。

2. 效果
    ```java
    @Autowired
    private FurnMapper furnMapper;
    ```
    Service 中可以直接注入并使用 Mapper。

### 声明式事务

1. 事务管理器
    ```xml
    <bean id="transactionManager"
          class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="pooledDataSource"/>
    </bean>
    ```

2. AOP 切入 Service 层
    ```xml
    <aop:config>
        <aop:pointcut id="txPoint"
            expression="execution(* com.hspedu.furns.service..*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPoint"/>
    </aop:config>

    <tx:advice id="txAdvice">
        <tx:attributes>
            <tx:method name="*"/>
            <tx:method name="get*" read-only="true"/>
        </tx:attributes>
    </tx:advice>
    ```

3. 理解
    - 事务边界一般放在 Service 层，而不是 Controller 或 Mapper 层。
    - `get*` 查询方法设置只读，可以让数据库和框架做一定优化。
    - 本质是 Spring AOP 给 Service 方法套了一层事务代理。

### 整合测试

```java
public class T1 {

    @Test
    public void testSqlSessionFactoryBean() {
        ApplicationContext ac =
            new ClassPathXmlApplicationContext("applicationContext.xml");
        System.out.println(ac.getBean("pooledDataSource"));
        System.out.println(ac.getBean("sqlSessionFactory"));
    }
}
```

如果能拿到 Druid 数据源和 `DefaultSqlSessionFactory`，说明 Spring + MyBatis 基础整合成功。

## 数据库与逆向工程

### 基本内容

1. 创建数据库和表

    ```sql
    CREATE DATABASE furns_ssm DEFAULT CHARACTER SET utf8;

    USE furns_ssm;

    CREATE TABLE furn (
        id INT PRIMARY KEY AUTO_INCREMENT,
        name VARCHAR(64) NOT NULL,
        maker VARCHAR(64) NOT NULL,
        price DECIMAL(11,2) NOT NULL,
        sales INT NOT NULL,
        stock INT NOT NULL,
        img_path VARCHAR(256) DEFAULT 'assets/images/product-image/1.jpg'
    ) CHARSET=utf8;
    ```

2. MAVEN
    ```xml
    <dependency>
        <groupId>org.mybatis.generator</groupId>
        <artifactId>mybatis-generator-core</artifactId>
        <version>1.4.0</version>
    </dependency>
    ```

3. 文档
    - https://mybatis.org/generator/

4. 配置`mbg.xml`

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE generatorConfiguration
            PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
            "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

    <generatorConfiguration>
        <context id="DB2Tables" targetRuntime="MyBatis3">
            
            <commentGenerator>
                <property name="suppressAllComments" value="true"/>
            </commentGenerator>

            <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                            connectionURL="jdbc:mysql://localhost:3306/furns_ssm?characterEncoding=utf8"
                            userId="root"
                            password="lcq">
            </jdbcConnection>

            <javaTypeResolver>
                <property name="forceBigDecimals" value="false"/>
            </javaTypeResolver>

            <javaModelGenerator targetPackage="com.lcq.furn.bean" targetProject=".\src\main\java">
                <property name="enableSubPackages" value="true"/>
                <property name="trimStrings" value="true"/>
            </javaModelGenerator>

            <sqlMapGenerator targetPackage="mapper" targetProject=".\src\main\resources">
                <property name="enableSubPackages" value="true"/>
            </sqlMapGenerator>

            <javaClientGenerator type="XMLMAPPER" targetPackage="com.lcq.furn.dao" targetProject=".\src\main\java">
                <property name="enableSubPackages" value="true"/>
            </javaClientGenerator>

            <table tableName="furn" domainObjectName="Furn"></table>
            
        </context>
    </generatorConfiguration>

    ```

5. 执行代码
    ```java
    import org.junit.jupiter.api.Test;
    import org.mybatis.generator.api.MyBatisGenerator;
    import org.mybatis.generator.config.Configuration;
    import org.mybatis.generator.config.xml.ConfigurationParser;
    import org.mybatis.generator.internal.DefaultShellCallback;

    import java.io.File;
    import java.io.InputStream;
    import java.util.ArrayList;
    import java.util.List;

    public class MBGTest {
        @Test
        public void test2() throws Exception {
            List<String> warnings = new ArrayList<>();
            boolean overwrite = true;

            // ✅ 通过类加载器从 classpath 读取
            InputStream is = MBGTest.class.getClassLoader().getResourceAsStream("MBG.xml");

            ConfigurationParser cp = new ConfigurationParser(warnings);
            Configuration config = cp.parseConfiguration(is); // parseConfiguration 有 InputStream 重载

            DefaultShellCallback callback = new DefaultShellCallback(overwrite);
            MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
            myBatisGenerator.generate(null);

            warnings.forEach(System.out::println);
        }
    }
    ```

### 📖 核心配置项详细解释

这份配置文件主要分为 **7 个核心模块**，它就像是一张“流水线图纸”，告诉 MyBatis Generator 该去哪里拉取数据，又该把生产出来的代码存放到哪里。

1. `<commentGenerator>` (注释控制)

    * **作用**：默认情况下，逆向工程会在生成的代码里加上一大堆包含日期的注释，非常影响代码阅读。
    * **解释**：`suppressAllComments="true"` 的作用就是**强制关闭所有自动生成的注释**，让最终生成的 Java 类保持干净清爽。

2. `<jdbcConnection>` (数据库连接)

    * **作用**：告诉插件去哪里读取表结构。
    * **解释**：你这里连接的是本地 MySQL 的 `furns_ssm` 数据库，账号是 `root`，密码是 `hsp`。运行插件时，它会登录这个数据库去考察表结构。

3. `<javaTypeResolver>` (类型转换)

    * **作用**：处理数据库类型到 Java 类型的映射规则。
    * **解释**：`forceBigDecimals="false"` 表示如果数据库里有类似于 `DECIMAL` 类型的字段，默认会被转换为 Java 的 `Double` 或其他合适的基础类型，而不是强行转换为稍显笨重的 `java.math.BigDecimal`。

4. `<javaModelGenerator>` (实体类去哪儿)

    * **作用**：控制生成的 POJO/Bean 放在哪个包下。
    * **解释**：它会将生成的 Java 类（包含属性和 getters/setters）存放到项目的 `.\src\main\java` 目录下的 `com.hspedu.furns.bean` 包中。`trimStrings="true"` 会自动给生成的 String 类型字段加上 `.trim()` 方法，防止数据库读出来的字符串前后有空格。

5. `<sqlMapGenerator>` (XML 映射文件去哪儿)

    * **作用**：控制生成的包含 SQL 语句的 XML 文件放在哪里。
    * **解释**：它会将生成的 XML 存放到 `.\src\main\resources` 目录下的 `mapper` 文件夹中。这也是目前最标准、最不容易踩坑的存放位置。

6. `<javaClientGenerator>` (Dao 接口去哪儿)

    * **作用**：控制生成的 Mapper 接口文件放在哪里。
    * **解释**：`type="XMLMAPPER"` 表示生成的接口会和刚才生成的 XML 文件配合使用。接口会被放在 `.\src\main\java` 的 `com.hspedu.furns.dao` 包下。

7. `<table>` (要处理哪张表)

    * **作用**：指定逆向工程的具体目标。
    * **解释**：
    * `tableName="furn"`：插件会去数据库里找一张名叫 `furn` 的表。
    * `domainObjectName="Furn"`：根据这张表生成的实体类名字叫做 `Furn`，生成的接口叫 `FurnMapper`，生成的 XML 叫 `FurnMapper.xml`。
    * 如果你以后数据库里加了新表（比如 `user`），只需要在这里多加一行 `<table tableName="user" domainObjectName="User"></table>` 即可。

### MyBatis Generator 的作用

1. 根据表自动生成：
    ```text
    Furn.java
    FurnExample.java
    FurnMapper.java
    FurnMapper.xml
    ```

2. 生成文件的作用
    | 文件 | 作用 |
    |---|---|
    | `Furn.java` | 实体类，对应表中的一行数据 |
    | `FurnExample.java` | 条件查询构造器，用于拼接 where 条件 |
    | `FurnMapper.java` | Mapper 接口，声明 CRUD 方法 |
    | `FurnMapper.xml` | SQL 映射文件，保存具体 SQL |

3. 生成后的常用方法
    | 方法 | 说明 |
    |---|---|
    | `insert` | 插入所有字段，没设置的字段也会插入 null |
    | `insertSelective` | 只插入不为 null 的字段，常用于新增 |
    | `deleteByPrimaryKey` | 根据主键删除 |
    | `updateByPrimaryKey` | 根据主键更新所有字段 |
    | `updateByPrimaryKeySelective` | 根据主键选择性更新不为 null 的字段 |
    | `selectByPrimaryKey` | 根据主键查询 |
    | `selectByExample` | 根据 Example 条件查询 |

### insert 和 insertSelective 的区别

1. `insertSelective`
    - 只保存对象中有值的字段。
    - 字段为 null 时，不参与 SQL。
    - 更适合新增数据。
    
2. `insert`
    - 不管字段有没有值，都会进入 SQL。
    - 没设置的字段可能插入 null。
    - 如果数据库字段不允许 null，就容易报错。
    
3. 示例理解
    ```java
    @Test
    public void test2() {
        ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
        FurnMapper bean = ioc.getBean(FurnMapper.class);

        Furn furn = new Furn();
        furn.setName("北欧实木餐桌");
        furn.setMaker("全友家居");
        furn.setPrice(new BigDecimal("1299.00"));
        furn.setSales(235);
        furn.setStock(50);
        furn.setImgPath("assets/images/product-image/1.jpg");

        bean.insertSelective(furn);
    }
    ```
    生成 SQL 时只插入 `name` 等有值字段，其他字段交给数据库默认值或保持空。

## Vue 前端工程

### 创建 Vue3 项目

1. 安装
    - 文档：https://cli.vuejs.org/zh
    - 命令：`npm install -g @vue/cli`

2. 因为是前后端分离，所以前端单独创建项目。在项目文件夹输入：
    ```bash
    vue create ssm_vue
    ```

3. 配置过程一览
    - 选择`Manually select features`
    - 选择的组件：
        - Babel
        - Router
        - Vuex
    - 选择`3.x`

    ```bash
    Microsoft Windows [版本 10.0.26100.4946]
    (c) Microsoft Corporation。保留所有权利。

    E:\code\IntelliJ\ssmproj1_vue>vue create ssm_vue


    Vue CLI v5.0.9
    ? Please pick a preset: Manually select features
    ? Check the features needed for your project: Babel, Router, Vuex
    ? Choose a version of Vue.js that you want to start the project with 3.x
    ? Use history mode for router? (Requires proper server setup for index fallback in production) Yes
    ? Where do you prefer placing config for Babel, ESLint, etc.? In package.json
    ? Save this as a preset for future projects? Yes
    ? Save preset as: ssm_vue

    🎉  Preset ssm_vue saved in C:\Users\85035\.vuerc


    Vue CLI v5.0.9
    ✨  Creating project in E:\code\IntelliJ\ssmproj1_vue\ssm_vue.
    🗃  Initializing git repository...
    ⚙️  Installing CLI plugins. This might take a while...


    added 809 packages in 1m

    108 packages are looking for funding
    run `npm fund` for details
    🚀  Invoking generators...
    📦  Installing additional dependencies...


    added 9 packages in 7s

    109 packages are looking for funding
    run `npm fund` for details
    ⚓  Running completion hooks...

    📄  Generating README.md...

    🎉  Successfully created project ssm_vue.
    👉  Get started with the following commands:

    $ cd ssm_vue
    $ npm run serve


    E:\code\IntelliJ\ssmproj1_vue>
    ```

2. 启动项目
    ```bash
    cd ssm_vue
    npm run serve
    ```

3. Vue3 关键目录
    ```text
    ssm_vue
    ├── public
    │   └── index.html
    ├── src
    │   ├── assets       静态资源
    │   ├── components   公共组件
    │   ├── router       路由配置
    │   ├── store        状态管理
    │   ├── views        页面组件
    │   ├── App.vue      根组件
    │   └── main.js      入口文件
    └── package.json     前端依赖配置，类似 Maven 的 pom.xml
    ```

6. 用IDEA打开并运行
    - 添加配置`npm`
    - 设定Node解释器与Vue一致
    - 

### Vue简介

1. `index.xml`
    - 前端项目首页
    - `<div id = "app">`在编译后会完成替换

2. `App.vue`
    - 用于界面布局
    - `<router-view>`：路由指令，把路由到的内容展示/替换到标签位置。

3. `router/index.js`
    - 路由配置
    - 

### 配置前端端口

1. 创建或修改 `vue.config.js`
    ```js
    module.exports = {
      devServer: {
        port: 9875
      }
    }
    ```

2. 浏览器访问
    ```text
    http://localhost:9875/
    ```

### ElementPlus

1. 文档：
    - https://element.eleme.cn
    - https://element-plus.org/#/zh-CN
2. ElementPlus 是适配 Vue3 的 UI 组件库。
2. 安装
    https://element-plus.org/zh-CN/guide/installation
    ```bash
    npm install element-plus --save
    ```

3. 在 `main.js` 中引入
    ```js
    import { createApp } from 'vue'
    import ElementPlus from 'element-plus'
    import 'element-plus/dist/index.css'
    import App from './App.vue'

    createApp(App).use(ElementPlus).mount('#app')
    ```

4. 其他说明
    - Element Plus 是 Element 对 Vue 3.0 的升级适配
    - Element 诞生于 2016 年，起初是饿了么内部的业务组件库，开源后深受广大前端开发者的喜爱，成为 Vue 生态中最流行的 UI 组件库之一。
    - Element Plus 是重构的全新项目。Element 团队重写了 Element 的代码，用于支持 Vue3
    - Element UI 还在维护和升级，因为 Vue2 仍然有项目在使用, Vue3 支持的浏览器范围有所减少, 这是一个大的改变, 所以在一段时间内, Vue2 仍然会在项目使用.

### 页面基础布局

1. 常见组件拆分
    ```text
    Header.vue   顶部栏
    Aside.vue    左侧菜单
    HomeView.vue 主显示区域
    ```

2. 页面大结构
    ```vue
    <template>
      <div>
        <Header />
        <div style="display: flex">
          <Aside />
          <router-view style="flex: 1" />
        </div>
      </div>
    </template>
    ```

3. 工程理解
    - Vue 负责页面显示、数据绑定、按钮事件。
    - 后端只提供接口，不再返回 JSP 页面。
    - 页面数据通过 Axios 请求后端 JSON。

## Axios 与前后端分离

### 安装 Axios

```bash
npm install axios --save
```

### 封装 request.js

1. 创建：
    ```text
    src/utils/request.js
    ```

2. 基本封装
    ```js
    import axios from 'axios'

    const request = axios.create({
      baseURL: '/api',
      timeout: 5000
    })

    // response 拦截器：统一处理返回结果
    request.interceptors.response.use(
      response => {
        let res = response.data
        if (response.config.responseType === 'blob') {
          return res
        }
        if (typeof res === 'string') {
          res = res ? JSON.parse(res) : res
        }
        return res
      },
      error => {
        console.log('err' + error)
        return Promise.reject(error)
      }
    )

    export default request
    ```

### 配置代理

1. 问题
    - 前端运行在 `localhost:9875`。
    - 后端运行在 `localhost:10001` 或 Tomcat 对应端口。
    - 浏览器直接请求不同端口会遇到跨域问题。

2. Vue 代理配置
    ```js
    module.exports = {
      devServer: {
        port: 9875,
        proxy: {
          '/api': {
            target: 'http://localhost:10001',
            changeOrigin: true,
            pathRewrite: {
              '^/api': ''
            }
          }
        }
      }
    }
    ```

3. 访问关系
    ```text
    前端代码：request.post('/api/save', form)
            ↓
    代理转发：http://localhost:10001/save
            ↓
    后端 Controller：@PostMapping('/save')
    ```

### 前后端分离的核心

1. 前端负责：
    - 页面组件。
    - 表单绑定。
    - 表格展示。
    - 分页控件。
    - 调用接口。

2. 后端负责：
    - 接收参数。
    - 业务处理。
    - 数据库操作。
    - 返回 JSON。

3. 双方约定：
    ```json
    {
      "code": 200,
      "msg": "success",
      "extend": {}
    }
    ```

## 通用返回对象 Msg

### 为什么需要 Msg

1. 如果每个接口返回格式不同，前端处理会很乱。
2. 通用返回对象可以统一表达：
    - 是否成功。
    - 提示信息。
    - 附加数据。

### Msg.java

```java
public class Msg {
    // 200 成功，400 失败
    private int code;

    // 提示信息
    private String msg;

    // 返回给浏览器的数据
    private Map<String, Object> extend = new HashMap<>();

    public static Msg success() {
        Msg res = new Msg();
        res.setCode(200);
        res.setMsg("success");
        return res;
    }

    public static Msg fail() {
        Msg res = new Msg();
        res.setCode(400);
        res.setMsg("fail");
        return res;
    }

    public Msg add(String key, Object value) {
        this.getExtend().put(key, value);
        return this;
    }

    // getter / setter
}
```

### 使用方式

```java
return Msg.success().add("furnsList", furnList);
```

前端拿数据：

```js
this.tableData = res.extend.furnsList
```

## 实现功能 01：添加家居信息

### 需求分析

1. 前端打开新增弹窗。
2. 用户填写家居名、厂商、价格、销量、库存。
3. 点击确定后，通过 Axios 发送 JSON。
4. 后端保存到 MySQL。
5. 保存成功后关闭弹窗，并刷新列表。

### 后端 Service

1. 接口
    ```java
    public interface FurnService {
        void save(Furn furn);
    }
    ```

2. 实现类
    ```java
    @Service
    public class FurnServiceImpl implements FurnService {

        @Autowired
        private FurnMapper furnMapper;

        @Override
        public void save(Furn furn) {
            // id 自增长，选择性插入更合适
            furnMapper.insertSelective(furn);
        }
    }
    ```

3. 注意
    - 新增时推荐 `insertSelective`。
    - `imgPath` 可以在实体类中设置默认值。

### Furn 默认图片处理

```java
private String imgPath = "assets/images/product-image/1.jpg";

public Furn(Integer id, String name, String maker, BigDecimal price,
            Integer sales, Integer stock, String imgPath) {
    this.id = id;
    this.name = name;
    this.maker = maker;
    this.price = price;
    this.sales = sales;
    this.stock = stock;
    if (!(imgPath == null || imgPath.equals(""))) {
        this.imgPath = imgPath;
    }
}
```

### Service 测试

```java
public class FurnServiceTest {
    private ApplicationContext ac;
    private FurnService furnService;

    @Before
    public void init() {
        ac = new ClassPathXmlApplicationContext("applicationContext.xml");
        furnService = ac.getBean(FurnService.class);
    }

    @Test
    public void save() {
        Furn furn = new Furn(null, "北欧风格沙发", "顺平家居",
                new BigDecimal(180), 666, 7, null);
        furnService.save(furn);
        System.out.println("save ok");
    }
}
```

### Controller

```java
@Controller
public class FurnController {

    @Autowired
    private FurnService furnService;

    @PostMapping("/save")
    @ResponseBody
    public Msg save(@RequestBody Furn furn) {
        furnService.save(furn);
        return Msg.success();
    }
}
```

1. `@RequestBody`
    - 表示从请求体中读取 JSON。
    - SpringMVC 会把 JSON 转成 `Furn` 对象。

2. `@ResponseBody`
    - 表示返回值直接写入响应体。
    - `Msg` 会被转换成 JSON。

### 前端新增弹窗

```vue
<el-dialog title="提示" v-model="dialogVisible" width="30%">
  <el-form :model="form" label-width="120px">
    <el-form-item label="家居名">
      <el-input v-model="form.name" style="width: 60%" />
    </el-form-item>
    <el-form-item label="厂商">
      <el-input v-model="form.maker" style="width: 60%" />
    </el-form-item>
    <el-form-item label="价格">
      <el-input v-model="form.price" style="width: 60%" />
    </el-form-item>
    <el-form-item label="销量">
      <el-input v-model="form.sales" style="width: 60%" />
    </el-form-item>
    <el-form-item label="库存">
      <el-input v-model="form.stock" style="width: 60%" />
    </el-form-item>
  </el-form>

  <template #footer>
    <span class="dialog-footer">
      <el-button @click="dialogVisible = false">取消</el-button>
      <el-button type="primary" @click="save">确定</el-button>
    </span>
  </template>
</el-dialog>
```

### 前端保存方法

```js
save() {
  request.post('/api/save', this.form).then(res => {
    console.log(res)
    this.dialogVisible = false
    this.list()
  })
}
```

## 实现功能 02：显示家居信息

### 需求分析

1. 页面加载后显示所有家居信息。
2. 后端返回列表数据。
3. 前端使用 ElementPlus 表格展示。

### Service

```java
public interface FurnService {
    void save(Furn furn);
    List<Furn> findAll();
}
```

```java
@Override
public List<Furn> findAll() {
    return furnMapper.selectByExample(null);
}
```

### Controller

```java
@ResponseBody
@RequestMapping("/furns")
public Msg listFurns() {
    List<Furn> furns = furnService.findAll();
    return Msg.success().add("furnsList", furns);
}
```

### 前端表格

```vue
<el-table :data="tableData" border stripe>
  <el-table-column prop="id" label="ID" />
  <el-table-column prop="name" label="家居名" />
  <el-table-column prop="maker" label="厂商" />
  <el-table-column prop="price" label="价格" />
  <el-table-column prop="sales" label="销量" />
  <el-table-column prop="stock" label="库存" />
</el-table>
```

### 前端查询方法

```js
created() {
  this.list()
},
methods: {
  list() {
    request.get('/api/furns').then(res => {
      this.tableData = res.extend.furnsList
    })
  }
}
```

## 实现功能 03：修改家居信息

### 需求分析

1. 点击某一行的“编辑”。
2. 将该行数据回显到弹窗。
3. 修改后提交。
4. 后端根据主键更新。
5. 成功后刷新列表。

### Service

```java
public interface FurnService {
    void update(Furn furn);
}
```

```java
@Override
public void update(Furn furn) {
    furnMapper.updateByPrimaryKeySelective(furn);
}
```

### Controller

```java
@PutMapping("/update")
@ResponseBody
public Msg update(@RequestBody Furn furn) {
    furnService.update(furn);
    return Msg.success();
}
```

### 前端编辑按钮

```vue
<el-button type="primary" @click="handleEdit(scope.row)">编辑</el-button>
```

### 前端回显

```js
handleEdit(row) {
  this.form = JSON.parse(JSON.stringify(row))
  this.dialogVisible = true
}
```

这里使用深拷贝，避免直接修改表格行对象导致页面提前变化。

### 前端保存中区分新增和修改

```js
save() {
  if (this.form.id) {
    request.put('/api/update', this.form).then(res => {
      if (res.code === 200) {
        this.$message({ type: 'success', message: '更新成功' })
      }
      this.dialogVisible = false
      this.list()
    })
  } else {
    request.post('/api/save', this.form).then(res => {
      this.dialogVisible = false
      this.list()
    })
  }
}
```

## 实现功能 04：删除家居信息

### 需求分析

1. 点击删除按钮。
2. 弹出确认框。
3. 用户确认后发送 DELETE 请求。
4. 后端根据 id 删除。
5. 前端刷新列表。

### Service

```java
public interface FurnService {
    void delete(Integer id);
}
```

```java
@Override
public void delete(Integer id) {
    furnMapper.deleteByPrimaryKey(id);
}
```

### Controller

```java
@DeleteMapping("/delete/{id}")
@ResponseBody
public Msg delete(@PathVariable("id") Integer id) {
    furnService.delete(id);
    return Msg.success();
}
```

### 前端删除

```js
del(id) {
  this.$confirm('此操作将永久删除该记录, 是否继续?', '提示', {
    confirmButtonText: '确定',
    cancelButtonText: '取消',
    type: 'warning'
  }).then(() => {
    request.delete('/api/delete/' + id).then(res => {
      if (res.code === 200) {
        this.$message({ type: 'success', message: '删除成功' })
        this.list()
      }
    })
  }).catch(() => {
    this.$message({ type: 'info', message: '已取消删除' })
  })
}
```

## 实现功能 05：分页显示列表

### 需求分析

1. 后端使用 PageHelper 进行物理分页。
2. 前端使用 ElementPlus 分页组件。
3. 每次切换页码或每页条数时，都重新请求后端。

### PageHelper 依赖

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.2.1</version>
</dependency>
```

### mybatis-config.xml 配置插件

```xml
<plugins>
    <plugin interceptor="com.github.pagehelper.PageInterceptor">
        <!-- 分页合理化：页码过大查最后一页，页码过小查第一页 -->
        <property name="reasonable" value="true"/>
    </plugin>
</plugins>
```

注意：`plugins` 标签在 MyBatis 配置文件中有顺序要求，一般放在 `typeAliases` 后面。

### Controller 分页接口

```java
@ResponseBody
@RequestMapping("/furnsByPage")
public Msg listFurnsByPage(
        @RequestParam(defaultValue = "1") Integer pageNum,
        @RequestParam(defaultValue = "5") Integer pageSize) {

    // 必须在查询前调用
    PageHelper.startPage(pageNum, pageSize);

    List<Furn> furnList = furnService.findAll();

    // navigatePages 表示导航页码数量
    PageInfo pageInfo = new PageInfo(furnList, pageSize);

    return Msg.success().add("pageInfo", pageInfo);
}
```

### PageHelper 关键点

1. `PageHelper.startPage(pageNum, pageSize)` 必须紧贴查询语句之前。
2. 它只对后面第一个 MyBatis 查询生效。
3. 它是物理分页，会在 SQL 中拼接 `limit`。
4. `PageInfo` 会封装：
    - 当前页数据 `list`。
    - 总记录数 `total`。
    - 总页数 `pages`。
    - 当前页码 `pageNum`。
    - 每页条数 `pageSize`。
    - 是否有上一页、下一页等。

### 前端分页组件

```vue
<el-pagination
  @size-change="handlePageSizeChange"
  @current-change="handleCurrentChange"
  :current-page="currentPage"
  :page-sizes="[5, 10]"
  :page-size="pageSize"
  layout="total, sizes, prev, pager, next, jumper"
  :total="total">
</el-pagination>
```

### 前端分页数据

```js
data() {
  return {
    currentPage: 1,
    pageSize: 5,
    total: 0,
    tableData: []
  }
}
```

### 前端分页查询

```js
list() {
  request.get('/api/furnsByPage', {
    params: {
      pageNum: this.currentPage,
      pageSize: this.pageSize
    }
  }).then(res => {
    this.tableData = res.extend.pageInfo.list
    this.total = res.extend.pageInfo.total
  })
},

handlePageSizeChange(pageSize) {
  this.pageSize = pageSize
  this.currentPage = 1
  this.list()
},

handleCurrentChange(pageNum) {
  this.currentPage = pageNum
  this.list()
}
```

## 实现功能 06：带条件查询分页显示

### 需求分析

1. 用户输入关键词。
2. 后端按家居名模糊查询。
3. 查询结果仍然分页。
4. 前端点击检索后刷新表格。

### Service

```java
public interface FurnService {
    List<Furn> findByCondition(String name);
}
```

```java
@Override
public List<Furn> findByCondition(String name) {
    FurnExample furnExample = new FurnExample();
    FurnExample.Criteria criteria = furnExample.createCriteria();

    if (!(name == null || "".equals(name))) {
        criteria.andNameLike("%" + name + "%");
    }

    return furnMapper.selectByExample(furnExample);
}
```

### Controller

```java
@ResponseBody
@RequestMapping("/furnsBySearchPage")
public Msg listFurnsByConditionPage(
        @RequestParam(defaultValue = "1") Integer pageNum,
        @RequestParam(defaultValue = "5") Integer pageSize,
        @RequestParam(defaultValue = "") String search) {

    PageHelper.startPage(pageNum, pageSize);
    List<Furn> furnList = furnService.findByCondition(search);
    PageInfo pageInfo = new PageInfo(furnList, pageSize);

    return Msg.success().add("pageInfo", pageInfo);
}
```

### 前端搜索区域

```vue
<div style="margin: 10px 0">
  <el-input
    v-model="search"
    placeholder="请输入关键字"
    style="width: 20%"
    clearable />
  <el-button type="primary" style="margin-left: 5px" @click="list">检索</el-button>
</div>
```

### 前端查询方法改造

```js
list() {
  request.get('/api/furnsBySearchPage', {
    params: {
      pageNum: this.currentPage,
      pageSize: this.pageSize,
      search: this.search
    }
  }).then(res => {
    this.tableData = res.extend.pageInfo.list
    this.total = res.extend.pageInfo.total
  })
}
```

### 注意

1. 点击检索时，最好把当前页重置为第一页。
    ```js
    searchFurns() {
      this.currentPage = 1
      this.list()
    }
    ```

2. 条件为空时，查询全部。
3. 模糊查询要注意 SQL 性能：`like '%关键字%'` 在数据量很大时通常无法很好使用普通索引。

## 实现功能 07：添加家居表单前端校验

### 需求分析

1. 前端先做基础校验，提升用户体验。
2. 必填项为空时，不发送请求。
3. 数字字段必须符合要求。

### ElementPlus 表单规则

```js
data() {
  return {
    rules: {
      name: [
        { required: true, message: '请输入家居名', trigger: 'blur' }
      ],
      maker: [
        { required: true, message: '请输入厂商', trigger: 'blur' }
      ],
      price: [
        { required: true, message: '请输入价格', trigger: 'blur' }
      ],
      sales: [
        { required: true, message: '请输入销量', trigger: 'blur' }
      ],
      stock: [
        { required: true, message: '请输入库存', trigger: 'blur' }
      ]
    }
  }
}
```

### 表单绑定规则

```vue
<el-form :model="form" :rules="rules" ref="form" label-width="120px">
  <el-form-item label="家居名" prop="name">
    <el-input v-model="form.name" />
  </el-form-item>
</el-form>
```

### 提交前校验

```js
this.$refs['form'].validate((valid) => {
  if (valid) {
    request.post('/api/save', this.form).then(res => {
      this.dialogVisible = false
      this.list()
    })
  } else {
    this.$message({
      type: 'error',
      message: '验证失败，不提交'
    })
    return false
  }
})
```

### 为什么前端校验不够

1. 前端校验可以被绕过。
2. 用户可以用 Postman、curl、脚本直接请求后端接口。
3. 浏览器开发者工具也可以篡改前端代码。
4. 因此：
    ```text
    前端校验负责体验。
    后端校验负责安全和数据正确性。
    ```

## 实现功能 08：添加家居表单后端校验

### 需求分析

1. 即使前端校验通过或被绕过，后端也必须再次校验。
2. 校验失败时不入库。
3. 后端返回字段级错误信息。
4. 前端把错误信息显示在对应表单项旁边。

### 引入校验依赖

```xml
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>6.1.0.Final</version>
</dependency>
```

### 实体类加注解

```java
public class Furn {
    private Integer id;

    @NotEmpty(message = "请输入家居名")
    private String name;

    @NotEmpty(message = "请输入厂家名")
    private String maker;

    @NotNull(message = "请输入数字")
    @Range(min = 0, message = "价格不能小于0")
    private BigDecimal price;

    @NotNull(message = "请输入数字")
    @Range(min = 0, message = "销量不能小于0")
    private Integer sales;

    @NotNull(message = "请输入数字")
    @Range(min = 0, message = "库存不能小于0")
    private Integer stock;
}
```

### Controller 后端校验

```java
@PostMapping("/save")
@ResponseBody
public Msg save(@Validated @RequestBody Furn furn, Errors errors) {
    Map<String, Object> map = new HashMap<>();

    List<FieldError> fieldErrors = errors.getFieldErrors();
    for (FieldError e : fieldErrors) {
        map.put(e.getField(), e.getDefaultMessage());
    }

    if (map.isEmpty()) {
        furnService.save(furn);
        return Msg.success();
    } else {
        return Msg.fail().add("errorMsg", map);
    }
}
```

### 关键注解说明

| 注解 / 类型 | 作用 |
|---|---|
| `@Validated` | 开启参数校验 |
| `@RequestBody` | 把请求体 JSON 转成 Java 对象 |
| `Errors` | 接收校验错误信息 |
| `FieldError` | 表示某个字段的校验错误 |
| `@NotEmpty` | 字符串不能为空 |
| `@NotNull` | 对象不能为 null |
| `@Range` | 数值范围校验 |

### 前端接收后端错误

```js
data() {
  return {
    serverValidErrors: {}
  }
}
```

```js
request.post('/api/save', this.form).then(res => {
  if (res.code === 200) {
    this.dialogVisible = false
    this.list()
  } else if (res.code === 400) {
    this.serverValidErrors.name = res.extend.errorMsg.name
    this.serverValidErrors.sales = res.extend.errorMsg.sales
    this.serverValidErrors.price = res.extend.errorMsg.price
    this.serverValidErrors.maker = res.extend.errorMsg.maker
    this.serverValidErrors.stock = res.extend.errorMsg.stock
  }
})
```

### 页面显示后端错误

```vue
<el-form-item label="家居名" prop="name">
  <el-input v-model="form.name" style="width: 60%" />
  {{ serverValidErrors.name }}
</el-form-item>
```

### 打开新增弹窗时清空错误

```js
add() {
  this.dialogVisible = true
  this.form = {}
  this.$refs['form'].resetFields()
  this.serverValidErrors = {}
}
```

## 项目常见错误与排查

### 后端启动类错误

| 现象 | 可能原因 | 解决方式 |
|---|---|---|
| 找不到 `applicationContext.xml` | 文件未放到 `resources` | 放到类路径根目录 |
| 找不到 Mapper.xml | XML 未放到 `resources/mapper` 或 Maven 未导出 | 检查 `mapperLocations` |
| Mapper 注入失败 | `MapperScannerConfigurer` 包路径错误 | 检查 `basePackage` |
| Controller 不生效 | SpringMVC 没扫描 Controller | 检查 `dispatcher-servlet.xml` |
| Service 不生效 | Spring 根容器没扫描 Service | 检查 `applicationContext.xml` |

### 请求错误

| 现象 | 可能原因 | 解决方式 |
|---|---|---|
| 404 | URL 不匹配、项目上下文路径错误 | 检查 `@RequestMapping` 和前端 `/api` 代理 |
| 405 | 请求方式不匹配 | POST/PUT/DELETE/GET 是否对应 |
| 415 | JSON 请求头不正确 | Axios 默认一般没问题，Postman 要选 JSON |
| 400 | 参数绑定失败或校验失败 | 检查字段名和类型 |
| 500 | 后端业务异常 | 看控制台堆栈，从最底层 Caused by 查起 |

### 前后端字段名不一致

1. Vue 表单字段：
    ```js
    form.name
    form.maker
    form.price
    ```

2. JavaBean 属性必须一致：
    ```java
    private String name;
    private String maker;
    private BigDecimal price;
    ```

3. 如果字段名不一致，JSON 无法正确绑定。

### PageHelper 不生效

1. `PageHelper.startPage()` 没写在查询前。
2. 查询不是 MyBatis 查询。
3. 插件没有配置到 `mybatis-config.xml`。
4. `mybatis-config.xml` 没被 `SqlSessionFactoryBean` 加载。

### 前端刷新不及时

1. 新增、修改、删除成功后要调用 `list()`。
2. 搜索后建议重置页码。
3. 修改对象时最好深拷贝，避免表格数据提前被修改。

## SSM 注解配置方案：从 XML 走向 Java Config

### 为什么要补充注解配置

1. 目前前文配置主要采用 XML，是传统 SSM 教学里最经典、最容易看清底层对象关系的写法。
2. 但真实项目的发展路线一般是：

    ```text
    XML 配置
        ↓
    XML + 注解混合配置
        ↓
    Java Config 注解配置
        ↓
    Spring Boot 自动配置
    ```

3. 所以学习时不要只记配置文件长什么样，而要看懂每个配置背后到底在创建什么对象：
    - `web.xml` 本质是在注册 `DispatcherServlet`、监听器和过滤器。
    - `dispatcher-servlet.xml` 本质是在配置 SpringMVC 子容器。
    - `applicationContext.xml` 本质是在配置 Spring 根容器、数据源、MyBatis、事务。
    - `mybatis-config.xml` 本质是在配置 MyBatis 自己的插件、别名、日志等。

4. 注解配置不是“换个写法背代码”，而是把原来 XML 中的 Bean 声明迁移到 Java 类中。这样更容易重构、跳转源码，也更接近 Spring Boot 的内部自动配置思想。

### 注解配置的总体拆分

1. 传统 XML 里有两个容器：

    ```text
    Spring 根容器
        管 Service / Mapper / DataSource / Transaction

    SpringMVC 子容器
        管 Controller / HandlerMapping / HandlerAdapter / JSON / 静态资源
    ```

2. 使用 Java Config 后，也建议保持这个边界：

    ```text
    SpringConfig.java       替代 applicationContext.xml
    SpringMvcConfig.java    替代 dispatcher-servlet.xml
    WebAppInitializer.java  替代 web.xml
    ```

3. 注意：即使使用注解配置，`Mapper.xml` 通常仍然保留。因为 MyBatis 的核心价值之一就是让复杂 SQL 留在 XML 中，Java 代码只声明 Mapper 接口。

### Maven 依赖补充

1. 如果后端返回 JSON，需要确保项目中有 Jackson 依赖，否则 `@ResponseBody` / `@RestController` 返回对象时可能无法自动转成 JSON。

    ```xml
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.12.3</version>
    </dependency>
    ```

2. 如果使用 Servlet 3.0+ 的注解式启动类，外部 Tomcat 环境需要支持 Servlet 3.0 以上。普通 Maven Web 项目中可以继续打包成 war 部署。

### WebAppInitializer：替代 web.xml

1. `WebAppInitializer` 的作用就是在 Web 容器启动时，用 Java 代码完成原来 `web.xml` 的工作。
2. 它需要继承 `AbstractAnnotationConfigDispatcherServletInitializer`。

```java
package com.hspedu.furns.config;

import org.springframework.web.filter.CharacterEncodingFilter;
import org.springframework.web.filter.HiddenHttpMethodFilter;
import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

import javax.servlet.Filter;

public class WebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

    // Spring 根容器配置类：Service、Mapper、数据源、事务
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }

    // SpringMVC 子容器配置类：Controller、JSON、静态资源、视图解析
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }

    // DispatcherServlet 映射路径，仍然推荐写 /，不要写 /*
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    // 替代 web.xml 中的过滤器配置
    @Override
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter encodingFilter = new CharacterEncodingFilter();
        encodingFilter.setEncoding("UTF-8");
        encodingFilter.setForceRequestEncoding(true);
        encodingFilter.setForceResponseEncoding(true);

        HiddenHttpMethodFilter hiddenHttpMethodFilter = new HiddenHttpMethodFilter();

        return new Filter[]{encodingFilter, hiddenHttpMethodFilter};
    }
}
```

3. 对照关系：

    | web.xml 配置 | Java Config 写法 |
    |---|---|
    | `ContextLoaderListener` | `getRootConfigClasses()` |
    | `DispatcherServlet` | `getServletConfigClasses()` + `getServletMappings()` |
    | `CharacterEncodingFilter` | `getServletFilters()` |
    | `HiddenHttpMethodFilter` | `getServletFilters()` |

4. 这里最重要的是理解两个容器的关系：根容器是父容器，MVC 容器是子容器。Controller 可以注入 Service，因为子容器能看见父容器中的 Bean；反过来，Service 不应该依赖 Controller。

### SpringMvcConfig：替代 dispatcher-servlet.xml

1. SpringMVC 配置类主要完成下面几件事：
    - 只扫描 `@Controller` / `@RestController`。
    - 开启 SpringMVC 注解驱动。
    - 放行静态资源。
    - 根据需要配置视图解析器。
    - 根据需要配置跨域。

```java
package com.hspedu.furns.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.DefaultServletHandlerConfigurer;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.InternalResourceViewResolver;

@Configuration
@EnableWebMvc
@ComponentScan(
        basePackages = "com.hspedu.furns",
        useDefaultFilters = false,
        includeFilters = {
                @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = Controller.class),
                @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = RestController.class)
        }
)
public class SpringMvcConfig implements WebMvcConfigurer {

    // 等价于 <mvc:default-servlet-handler/>
    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }

    // 传统页面跳转时需要；前后端分离项目中可以不配或弱化
    @Bean
    public InternalResourceViewResolver viewResolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".html");
        return resolver;
    }

    // 如果开发期不用 Vue 代理，也可以在后端临时开启 CORS
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("http://localhost:9875")
                .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                .allowedHeaders("*")
                .allowCredentials(true);
    }
}
```

2. `@EnableWebMvc` 可以理解为 XML 里的：

    ```xml
    <mvc:annotation-driven/>
    ```

3. 前后端分离项目中，视图解析器不是重点。更现代的写法是 Controller 直接返回 JSON：

    ```java
    @RestController
    @RequestMapping("/furns")
    public class FurnController {
        // 返回对象，由 Jackson 转成 JSON
    }
    ```

4. 开发时更推荐用 Vue 代理解决跨域；后端 CORS 配置适合前后端完全分开部署时使用。不要两边乱配，否则排错会很痛苦。

### SpringConfig：替代 applicationContext.xml

1. Spring 根容器配置类负责非 Web 层对象：
    - `DataSource`
    - `SqlSessionFactoryBean`
    - Mapper 扫描
    - Service 扫描
    - 事务管理器
    - 声明式事务开关

```java
package com.hspedu.furns.config;

import com.alibaba.druid.pool.DruidDataSource;
import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType;
import org.springframework.context.annotation.PropertySource;
import org.springframework.core.io.support.PathMatchingResourcePatternResolver;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.stereotype.Controller;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import javax.sql.DataSource;

@Configuration
@PropertySource("classpath:jdbc.properties")
@EnableTransactionManagement
@MapperScan("com.hspedu.furns.dao")
@ComponentScan(
        basePackages = "com.hspedu.furns",
        excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = Controller.class)
)
public class SpringConfig {

    @Value("${jdbc.url}")
    private String jdbcUrl;

    @Value("${jdbc.driverClass}")
    private String driverClass;

    @Value("${jdbc.userName}")
    private String userName;

    @Value("${jdbc.password}")
    private String password;

    @Bean
    public DataSource dataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setUrl(jdbcUrl);
        dataSource.setDriverClassName(driverClass);
        dataSource.setUsername(userName);
        dataSource.setPassword(password);
        return dataSource;
    }

    @Bean
    public SqlSessionFactory sqlSessionFactory(DataSource dataSource) throws Exception {
        SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
        factoryBean.setDataSource(dataSource);

        // 保留 MyBatis 全局配置文件，用于 PageHelper、日志、别名等配置
        factoryBean.setConfigLocation(
                new PathMatchingResourcePatternResolver()
                        .getResource("classpath:mybatis-config.xml")
        );

        // Mapper.xml 仍然建议保留
        factoryBean.setMapperLocations(
                new PathMatchingResourcePatternResolver()
                        .getResources("classpath:mapper/*.xml")
        );

        return factoryBean.getObject();
    }

    @Bean
    public PlatformTransactionManager transactionManager(DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }
}
```

2. 对照关系：

    | applicationContext.xml | Java Config |
    |---|---|
    | `<context:property-placeholder>` | `@PropertySource` + `@Value` |
    | `<context:component-scan>` | `@ComponentScan` |
    | `DruidDataSource` Bean | `@Bean dataSource()` |
    | `SqlSessionFactoryBean` Bean | `@Bean sqlSessionFactory()` |
    | `MapperScannerConfigurer` | `@MapperScan` |
    | `<tx:advice>` + `<aop:config>` | `@EnableTransactionManagement` + `@Transactional` |

3. 这里有一个学习重点：`@MapperScan` 扫描的是 MyBatis 的 Mapper 接口；`@ComponentScan` 扫描的是 Spring 的组件注解，例如 `@Service`、`@Component` 等。Mapper 接口一般没有实现类，不能完全当作普通 Spring 组件理解。

### 使用 @Transactional 替代 XML 事务切面

1. XML 事务配置通常会写成：

    ```xml
    <tx:advice id="txAdvice">
        <tx:attributes>
            <tx:method name="*"/>
            <tx:method name="get*" read-only="true"/>
        </tx:attributes>
    </tx:advice>
    ```

2. 注解式事务更常见的写法是直接放在 Service 层：

```java
@Service
@Transactional
public class FurnServiceImpl implements FurnService {

    @Autowired
    private FurnMapper furnMapper;

    @Override
    public void save(Furn furn) {
        furnMapper.insertSelective(furn);
    }

    @Override
    @Transactional(readOnly = true)
    public List<Furn> findAll() {
        return furnMapper.selectByExample(null);
    }
}
```

3. 使用细节：
    - 事务通常加在 Service 层，不加在 Controller 层。
    - 查询方法可以设置 `readOnly = true`。
    - `@Transactional` 依赖 Spring AOP，默认只对通过 Spring 代理对象调用的 public 方法稳定生效。
    - 同一个类内部方法互相调用，可能绕过代理，导致事务不生效。这个坑在项目里很常见。

### Controller 的现代注解写法

1. 传统写法：

```java
@Controller
public class FurnController {

    @PostMapping("/save")
    @ResponseBody
    public Msg save(@RequestBody Furn furn) {
        return Msg.success();
    }
}
```

2. 现代前后端分离写法：

```java
@RestController
@RequestMapping("/furns")
public class FurnController {

    @Autowired
    private FurnService furnService;

    @PostMapping
    public Msg save(@Validated @RequestBody Furn furn, Errors errors) {
        if (errors.hasErrors()) {
            Map<String, Object> map = new HashMap<>();
            for (FieldError error : errors.getFieldErrors()) {
                map.put(error.getField(), error.getDefaultMessage());
            }
            return Msg.fail().add("errorMsg", map);
        }

        furnService.save(furn);
        return Msg.success();
    }

    @GetMapping
    public Msg list() {
        return Msg.success().add("furnsList", furnService.findAll());
    }

    @PutMapping
    public Msg update(@RequestBody Furn furn) {
        furnService.update(furn);
        return Msg.success();
    }

    @DeleteMapping("/{id}")
    public Msg delete(@PathVariable Integer id) {
        furnService.delete(id);
        return Msg.success();
    }
}
```

3. `@RestController` 等价于：

    ```text
    @Controller + @ResponseBody
    ```

4. REST 风格下，接口命名可以从“动词路径”变成“资源路径 + HTTP 方法”：

    | 旧写法 | REST 写法 | 含义 |
    |---|---|---|
    | `POST /save` | `POST /furns` | 新增 |
    | `GET /furns` | `GET /furns` | 查询列表 |
    | `PUT /update` | `PUT /furns` | 修改 |
    | `DELETE /delete/{id}` | `DELETE /furns/{id}` | 删除 |

### 注解配置的优缺点

1. 优点
    - 配置和代码距离更近，IDE 跳转更方便。
    - 类型安全比 XML 好，类名写错更容易在编译或启动时暴露。
    - 更接近 Spring Boot 自动配置的真实写法。

2. 缺点
    - 初学时不如 XML 直观，因为很多 Bean 创建过程藏在 Java 方法和注解背后。
    - 配置类写多了以后，也会变成另一种形式的“样板代码”。
    - 如果没有理解容器边界，很容易把 Controller、Service、Mapper 全扫混。

3. 最推荐的学习方式：

    ```text
    先用 XML 看清楚对象关系
        ↓
    再用 Java Config 改写一遍
        ↓
    最后看 Spring Boot 自动配置到底帮你省掉了什么
    ```


## 工程延伸：SSM 与 Spring Boot 的关系

### 传统 SSM 的特点

1. 传统 SSM 配置较多：

    ```text
    web.xml
    dispatcher-servlet.xml
    applicationContext.xml
    mybatis-config.xml
    jdbc.properties
    mapper/*.xml
    ```

2. 它的优点是“透明”：
    - `DispatcherServlet` 怎么注册，看得见。
    - Spring 根容器和 SpringMVC 子容器怎么分工，看得见。
    - 数据源、`SqlSessionFactoryBean`、Mapper 扫描、事务管理器怎么创建，看得见。

3. 它的缺点也很明显：
    - 样板配置多。
    - 项目搭建慢。
    - XML、依赖版本、Tomcat 部署路径都可能成为错误来源。
    - 新人经常不是业务代码写错，而是配置文件写错。

4. 所以传统 SSM 很适合学习底层协作方式，但真正新项目一般不会再从大量 XML 开始搭建。

### Spring Boot 不是替代 SSM，而是封装 SSM

1. 很多人会误以为：

    ```text
    学了 Spring Boot 就不用学 Spring / SpringMVC / MyBatis 了
    ```

    这个理解是错的。

2. 更准确的理解是：

    ```text
    Spring Boot = Spring 生态的快速装配方式
    ```

3. 以 Web 项目为例，Spring Boot 默认帮我们做了很多传统 SSM 中手动配置的事：

    | 传统 SSM | Spring Boot |
    |---|---|
    | 外部 Tomcat 部署 war | 内嵌 Tomcat，直接运行 main 方法 |
    | `web.xml` | 自动注册 `DispatcherServlet`、过滤器等 |
    | `dispatcher-servlet.xml` | SpringMVC 自动配置 |
    | `applicationContext.xml` | 自动组件扫描 + Java Config |
    | `jdbc.properties` | `application.yml` / `application.properties` |
    | 手写 `SqlSessionFactoryBean` | MyBatis Starter 自动创建 |
    | `MapperScannerConfigurer` | `@MapperScan` |
    | XML 事务 AOP | `@EnableTransactionManagement` + `@Transactional`，多数情况下自动生效 |

4. 也就是说，Spring Boot 不是把这些东西删掉了，而是把“通用且重复”的配置自动做好了。

### Spring Boot 版项目依赖思路

1. 传统 SSM 中我们手动引入：

    ```text
    spring-webmvc
    spring-jdbc
    mybatis
    mybatis-spring
    mysql-connector-java
    druid
    pagehelper
    hibernate-validator
    jackson-databind
    ```

2. Spring Boot 中更常见的是引入 Starter：

    ```xml
    <dependencies>
        <!-- Web 开发：包含 SpringMVC、内嵌 Tomcat、JSON 处理等 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- 参数校验 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>

        <!-- MyBatis 整合 Spring Boot -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>

        <!-- MySQL 驱动。Spring Boot 3 常用 com.mysql:mysql-connector-j -->
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <scope>runtime</scope>
        </dependency>

        <!-- 测试 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
    ```

3. 如果使用 Druid 或 PageHelper，也可以引入对应 starter。实际项目中要注意 starter 版本和 Spring Boot 大版本兼容，不能无脑复制。

### application.yml：替代大部分配置文件

1. Spring Boot 中通常把配置集中到：

    ```text
    src/main/resources/application.yml
    ```

2. 一个简化版配置如下：

    ```yaml
    server:
      port: 10001
      servlet:
        context-path: /

    spring:
      datasource:
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://localhost:3306/furns_ssm?useUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai
        username: root
        password: hsp

    mybatis:
      mapper-locations: classpath:mapper/*.xml
      type-aliases-package: com.hspedu.furns.entity
      configuration:
        map-underscore-to-camel-case: true
        log-impl: org.apache.ibatis.logging.stdout.StdOutImpl

    pagehelper:
      helper-dialect: mysql
      reasonable: true
      support-methods-arguments: true
    ```

3. 对照理解：

    | yml 配置 | 替代内容 |
    |---|---|
    | `server.port` | Tomcat 端口配置 |
    | `spring.datasource` | 数据源配置 |
    | `mybatis.mapper-locations` | Mapper.xml 路径 |
    | `mybatis.type-aliases-package` | MyBatis 类型别名 |
    | `mybatis.configuration` | MyBatis 全局设置 |
    | `pagehelper` | PageHelper 插件设置 |

4. `application.yml` 不是魔法。它本质上是给自动配置类提供参数，自动配置类再根据这些参数创建对应 Bean。

### 启动类：替代 web.xml 和外部 Tomcat 部署流程

1. Spring Boot 项目一般有一个主启动类：

```java
package com.hspedu.furns;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@MapperScan("com.hspedu.furns.mapper")
public class FurnApplication {

    public static void main(String[] args) {
        SpringApplication.run(FurnApplication.class, args);
    }
}
```

2. `@SpringBootApplication` 可以粗略理解为三件事的组合：
    - `@SpringBootConfiguration`：当前类是配置类。
    - `@EnableAutoConfiguration`：开启自动配置。
    - `@ComponentScan`：扫描当前包及其子包下的组件。

3. 因此 Spring Boot 项目中包结构很重要。启动类最好放在项目根包下，例如：

    ```text
    com.hspedu.furns.FurnApplication
    com.hspedu.furns.controller
    com.hspedu.furns.service
    com.hspedu.furns.mapper
    ```

4. 如果启动类放得太深，可能扫描不到其他包里的 Controller 或 Service。

### Spring Boot 版 Controller 思路

1. 传统 SSM 常写：

    ```java
    @Controller
    @ResponseBody
    ```

2. Spring Boot 前后端分离项目更常写：

```java
@RestController
@RequestMapping("/api/furns")
public class FurnController {

    private final FurnService furnService;

    public FurnController(FurnService furnService) {
        this.furnService = furnService;
    }

    @PostMapping
    public Result<Void> save(@Validated @RequestBody FurnSaveRequest request) {
        furnService.save(request);
        return Result.success();
    }

    @GetMapping
    public Result<PageResult<FurnVO>> page(FurnQueryRequest request) {
        return Result.success(furnService.page(request));
    }

    @PutMapping("/{id}")
    public Result<Void> update(@PathVariable Integer id,
                               @Validated @RequestBody FurnUpdateRequest request) {
        furnService.update(id, request);
        return Result.success();
    }

    @DeleteMapping("/{id}")
    public Result<Void> delete(@PathVariable Integer id) {
        furnService.delete(id);
        return Result.success();
    }
}
```

3. 这个写法里有几个现代项目倾向：
    - 构造器注入优先于字段注入，依赖更清晰，也方便测试。
    - 接口路径以资源为中心，例如 `/api/furns`。
    - 新增、修改、查询分别使用不同 Request 对象。
    - 返回给前端的是 VO 或统一分页对象，而不是直接把数据库实体丢出去。

### DTO / VO / Entity 分层思考

1. 课程项目为了简单，直接让 `Furn` 同时承担：
    - 数据库实体。
    - 前端请求对象。
    - 前端响应对象。

2. 小项目可以这样写，理解链路最快。但到了真实项目，最好拆开：

    | 类型 | 作用 |
    |---|---|
    | `FurnEntity` / `FurnDO` | 对应数据库表 |
    | `FurnSaveRequest` | 新增请求参数 |
    | `FurnUpdateRequest` | 修改请求参数 |
    | `FurnQueryRequest` | 查询参数 |
    | `FurnVO` | 返回给前端展示的数据 |

3. 为什么要拆？
    - 数据库字段不一定都能暴露给前端。
    - 前端新增和修改需要的字段可能不同。
    - 返回字段可能需要组合、格式化、脱敏。
    - 后期字段变化时，避免数据库表结构直接污染接口契约。

4. 可以用手写转换，也可以用 MapStruct 这类工具做对象转换。刚入门时先手写，能看清楚数据流。

### 统一异常处理：替代每个 Controller 手写错误判断

1. 前面的课程写法是在 Controller 方法里手动处理 `Errors`：

    ```java
    if (errors.hasErrors()) {
        return Msg.fail().add("errorMsg", map);
    }
    ```

2. Spring Boot 项目更常见的是统一异常处理：

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public Result<Map<String, String>> handleValidException(MethodArgumentNotValidException e) {
        Map<String, String> errors = new HashMap<>();
        for (FieldError fieldError : e.getBindingResult().getFieldErrors()) {
            errors.put(fieldError.getField(), fieldError.getDefaultMessage());
        }
        return Result.fail("参数校验失败", errors);
    }

    @ExceptionHandler(Exception.class)
    public Result<Void> handleException(Exception e) {
        // 实际项目中这里应该记录日志
        return Result.fail("系统异常，请稍后再试");
    }
}
```

3. 这样 Controller 可以保持干净：

    ```java
    @PostMapping
    public Result<Void> save(@Validated @RequestBody FurnSaveRequest request) {
        furnService.save(request);
        return Result.success();
    }
    ```

4. 这也是现代项目的一个核心思路：

    ```text
    Controller 少写流程控制
    Service 专注业务
    ExceptionHandler 统一处理异常
    Interceptor / Filter 统一处理横切逻辑
    ```

### Service 层仍然是事务边界

1. 不管是 XML SSM、Java Config SSM，还是 Spring Boot，事务边界都建议放在 Service 层。

```java
@Service
public class FurnServiceImpl implements FurnService {

    private final FurnMapper furnMapper;

    public FurnServiceImpl(FurnMapper furnMapper) {
        this.furnMapper = furnMapper;
    }

    @Override
    @Transactional
    public void save(FurnSaveRequest request) {
        FurnEntity entity = new FurnEntity();
        entity.setName(request.getName());
        entity.setMaker(request.getMaker());
        entity.setPrice(request.getPrice());
        entity.setSales(request.getSales());
        entity.setStock(request.getStock());
        furnMapper.insertSelective(entity);
    }

    @Override
    @Transactional(readOnly = true)
    public PageResult<FurnVO> page(FurnQueryRequest request) {
        // 查询逻辑
        return null;
    }
}
```

2. 注意：
    - 单表简单 CRUD 可以很薄。
    - 一旦涉及多表写入、库存扣减、订单创建，Service 就是事务一致性的核心。
    - 不要把业务逻辑堆在 Controller，也不要把复杂业务塞进 Mapper。

### MyBatis、MyBatis-Plus 与复杂 SQL

1. 课程使用 MyBatis Generator，可以自动生成基础 CRUD。
2. 现代项目中很多团队会使用 MyBatis-Plus 简化单表 CRUD。
3. 但要记住：

    ```text
    MyBatis-Plus 简化的是常规 CRUD，
    不是让你彻底不用理解 SQL。
    ```

4. 遇到复杂报表、多表联查、性能优化、手写索引友好的 SQL 时，MyBatis XML 仍然很有价值。
5. 真实工作里比较健康的搭配是：

    ```text
    简单单表 CRUD：MyBatis-Plus / Generator
    复杂查询和性能敏感 SQL：手写 Mapper.xml
    ```

### 前后端分离下的跨域与代理思考

1. 开发阶段一般推荐：

    ```text
    Vue devServer proxy 代理到后端
    ```

    好处是浏览器看到的是同源请求，少折腾跨域。

2. 联调或部署阶段常见做法：

    ```text
    Nginx
      /            -> 前端静态资源
      /api         -> 后端服务
    ```

3. 后端也可以通过 CORS 放行，但要注意：
    - 不要随便 `allowedOrigins("*")` 搭配凭证。
    - 生产环境应明确允许的前端域名。
    - 登录态如果依赖 Cookie，还要考虑 `SameSite`、HTTPS、跨域携带凭证等问题。

4. 入门项目先用代理最省心；真实部署再理解 Nginx 和 CORS。

### 从传统 SSM 迁移到 Spring Boot 的路径

1. 推荐迁移步骤：

    ```text
    第一步：保留原有 Controller / Service / Mapper / Mapper.xml
    第二步：新建 Spring Boot 启动类
    第三步：引入 starter 依赖
    第四步：把 jdbc.properties、mybatis-config.xml 中的大部分配置迁移到 application.yml
    第五步：使用 @MapperScan 扫描 Mapper
    第六步：用 @Transactional 替代 XML 事务切面
    第七步：删除 web.xml、dispatcher-servlet.xml、applicationContext.xml
    第八步：补统一异常处理、DTO/VO、接口路径规范
    ```

2. 迁移时不要一口气重构全部代码。先让项目跑起来，再逐步现代化。
3. 最容易出错的地方：
    - 包扫描路径。
    - Mapper.xml 路径。
    - MySQL 驱动类和 URL 参数。
    - Boot 2 与 Boot 3 的 `javax.*` / `jakarta.*` 包名差异。
    - PageHelper 或 Druid starter 的版本兼容。

### 现代 Spring Boot 项目结构建议

```text
com.hspedu.furns
├─ FurnApplication.java
├─ common
│  ├─ Result.java
│  ├─ PageResult.java
│  └─ ErrorCode.java
├─ config
│  ├─ WebConfig.java
│  └─ MyBatisConfig.java
├─ controller
│  └─ FurnController.java
├─ service
│  ├─ FurnService.java
│  └─ impl
│     └─ FurnServiceImpl.java
├─ mapper
│  └─ FurnMapper.java
├─ entity
│  └─ FurnEntity.java
├─ model
│  ├─ request
│  │  ├─ FurnSaveRequest.java
│  │  ├─ FurnUpdateRequest.java
│  │  └─ FurnQueryRequest.java
│  └─ vo
│     └─ FurnVO.java
└─ exception
   └─ GlobalExceptionHandler.java
```

1. 这个结构比课程项目更啰嗦，但更适合多人协作。
2. 课程阶段可以先不用完全拆这么细；找实习、做简历项目时，建议至少具备：
    - `controller`
    - `service`
    - `mapper`
    - `entity`
    - `request`
    - `vo`
    - `common`
    - `exception`

### 对学习路线的建议

1. XML SSM 阶段要学会的是“底层对象关系”：

    ```text
    DispatcherServlet 为什么能接请求
    Controller 为什么能注入 Service
    Service 为什么能注入 Mapper
    Mapper 为什么能执行 XML 中的 SQL
    事务为什么能自动提交/回滚
    ```

2. 注解 SSM 阶段要学会的是“配置迁移”：

    ```text
    XML Bean -> @Bean
    XML 扫描 -> @ComponentScan
    XML 事务 -> @Transactional
    web.xml -> WebAppInitializer
    ```

3. Spring Boot 阶段要学会的是“约定和工程化”：

    ```text
    自动配置
    Starter 依赖
    application.yml
    统一响应
    统一异常
    DTO / VO 分层
    接口文档
    日志
    环境隔离
    部署
    ```

4. 用一句话总结：

    ```text
    SSM 让你知道车怎么造，Spring Boot 让你更快把车开上路。
    ```

5. 找 Java 后端实习时，能把这个项目从 XML SSM 改成 Spring Boot，是一个很好的练习。面试里如果能讲清楚“我不是只会写 Controller，而是知道自动配置替我做了什么”，会比只背八股扎实很多。


## 项目小结

### 本章必须掌握

1. SSM 三层整合关系
    ```text
    SpringMVC 管 Controller
    Spring 管 Service、事务、数据源、MyBatis
    MyBatis 管 SQL 和数据库映射
    ```

2. 前后端分离通信关系
    ```text
    Vue + Axios 发送 JSON
    SpringMVC 接收 JSON
    Service 执行业务
    MyBatis 操作数据库
    后端返回统一 JSON
    Vue 根据结果更新页面
    ```

3. CRUD 基本套路
    ```text
    先写 Mapper / 逆向生成
    再写 Service
    再写 Controller
    用 Postman 测接口
    最后接 Vue 页面
    ```

4. 分页套路
    ```text
    PageHelper.startPage()
    查询 List
    new PageInfo(list)
    Msg.success().add("pageInfo", pageInfo)
    前端绑定 pageInfo.list 和 pageInfo.total
    ```

5. 校验套路
    ```text
    前端 ElementPlus rules 校验体验
    后端 Hibernate Validator 兜底
    Errors 收集字段错误
    Msg.fail().add("errorMsg", map)
    前端显示字段级错误
    ```

### 推荐练习

1. 独立重写项目，不看老师代码完成：
    - 新增家居。
    - 查询列表。
    - 修改家居。
    - 删除家居。
    - 分页查询。
    - 条件分页查询。
    - 前后端校验。

2. 加强功能
    - 批量删除。
    - 图片上传。
    - 统一异常处理。
    - 登录拦截。
    - 操作日志。
    - 后端 DTO/VO 分层。

3. 升级练习
    - 用 Spring Boot 重写该 SSM 项目。
    - 用 MyBatis-Plus 简化 CRUD。
    - 用 Redis 缓存列表或详情页。
    - 用 Swagger / Knife4j 生成接口文档。

### 最后一张脑图式总结

```text
SSM 前后端分离项目
│
├─ 后端基础环境
│  ├─ Maven Web 工程
│  ├─ web.xml
│  ├─ dispatcher-servlet.xml
│  ├─ applicationContext.xml
│  ├─ jdbc.properties
│  └─ mybatis-config.xml
│
├─ SpringMVC
│  ├─ DispatcherServlet
│  ├─ @Controller
│  ├─ @RequestMapping / @GetMapping / @PostMapping
│  ├─ @RequestBody
│  ├─ @ResponseBody
│  └─ JSON 转换
│
├─ Spring
│  ├─ IOC / DI
│  ├─ @Service
│  ├─ 数据源
│  ├─ Mapper 扫描
│  └─ 声明式事务
│
├─ MyBatis
│  ├─ Mapper 接口
│  ├─ Mapper.xml
│  ├─ MyBatis Generator
│  ├─ Example 条件查询
│  └─ PageHelper 分页
│
├─ Vue3 前端
│  ├─ ElementPlus 表格 / 表单 / 弹窗 / 分页
│  ├─ Axios 请求
│  ├─ request.js 封装
│  ├─ proxy 代理
│  └─ 数据绑定与页面刷新
│
└─ 项目功能
   ├─ 新增
   ├─ 查询
   ├─ 修改
   ├─ 删除
   ├─ 分页
   ├─ 条件查询
   └─ 前后端校验
```
