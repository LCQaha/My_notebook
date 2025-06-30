# 韩顺平 JAVA 学习笔记

写在前面：
本笔记为应某项目需求，短期速成java时进行的记录，因本人有一定编程基础，故跳过了一些基础的知识点，望读者海涵。

## PRE
### 课程目录
1. Java基础
    - 建立编程思想
    - 提升编程能力
    - 分析需求，代码实现能力
2. Java高级
3. JavaWeb
4. 主流框架和项目管理
5. 分布式、微服务、并行架构
6. DevOps（开发运维一体化）、自动化部分管理项目、解决CI/CD
7. 大数据技术
8. 项目
9. 大厂高频面试题
10. 底层源码/内核研究
11. 编程基础扩展

### 就业方向
1. JavaEE软件工程师
    - 电商、团购、众筹、SNS（社交网络）、教育、金融、搜索
2. 大数据软件工程师（Java基础，JavaSE）
    - 大数据应用工程师
    - 大数据算法工程师
    - 大数据分析和挖掘
3. Android软件工程师

### 开发场景
1. SSM
    - Spring（轻量级容器框架）
    - SpringMVC（分层的Web开发框架）
    - MyBatis（持久化框架）
2. Android核心代码
3. 大数据-Hadoop

### 应用领域
1. 企业级应用
    - 复杂的大企业的软件系统
    - 各种类型的网站
    - 金融、电信、交通、电子商务等领域应用

2. Android平台应用
    安卓应用程序使用Java编写

3. 移动领域应用
    消费和嵌入式领域，在各种小型设备上的应用：
    - 机顶盒
    - 车载大屏影音娱乐设备
    - 汽车通信设备
    - 扫码的POS机

## 概述

### 章节目录
1. Java历史
2. Java特点
3. Java运行机制及运行过程
4. Java开发环境搭建
5. Dos基本指令

### 第一个程序
1. 什么是程序？
    程序是有序指令的集合
2. 程序如何输出“1+1”？    
    ```java
    public class Test{
        public static void main(String[] args){
                int res = 1+1;
                System.out.println("1+1=" + res);
        }
    }
    ```
3. 如何执行程序？
    打开cmd，在当前目录下执行以下指令：
    ```bash
    javac Test.java
    java Test
    ```
### Java历史
![java_history](./img/java_history.png)
[点击此处](https://www.oracle.com/java/technologies/java-se-support-roadmap.html)查看Java历史版本详情。
课程内容主要围绕Java 8和Java 11，因为这两个版本是长期支持（LTS）的版本。具体内容如下图（图片获取时间：2025-4-10）：
![java_history_version1](./img/java_history_version1.png)

### Java特点

1. Java技术体系平台
    ![java_Edition](./img/java_Edition.png)

2. 特点
    - Java语言是面向对象（oop）的。
    - Java语言是健壮的。Java的强类型机制、异常处理、垃圾的自动收集是Java程序健壮性的重要保证。
    - Java语言是跨平台性的。（一个编译好的`*.class`文件可以在多个不同系统下运行）
    - Java语言是**解释性**的。（不能直接由机器执行，而是经由解释器执行）

### 常见Java开发工具
1. editplus 、 notepad++
2. Sublime Text
    - 下载：[https://www.sublimetext.com/3](https://www.sublimetext.com/3)
    - 中文设置：https://www.bilibili.com/video/BV1HKHzeZEZA
        - 按组合键`Ctrl+Shift+P`，输入：`install`，选择`install package control`，等待弹窗。
        - 按组合键`Ctrl+Shift+P`，输入：`install`，等待弹窗。
        - 输入`Chinese`，然后确定，等待安装完成。
3. Eclipse
4. IntelliJ IDEA

### Java运行机制及运行过程
1. JAVA程序如何运行？
    - Java的`*.class`文件是运行在Java虚拟机（JVM,Java Virtual Machine）中的
    - JVM包含在jdk中。

2. Java代码从书写到运行的过程
    - 编译：使用`javac`命令将源代码编译成字节码文件（`.class`）
    - 运行：使用`java`命令运行字节码文件（`.class`）

3. 什么是jdk、jre？
    - jdk：Java Development Kit，Java开发工具包，包含jre和Java开发工具（java命令，javac命令，javadoc命令，javap命令等）。
    - jre：Java Runtime Environment，Java运行环境，包含JVM和Java核心类库。
    - 安装了jdk就不需要jre，如果需要运行一个开发好的Java程序，只需要安装jre即可。
    - 即：jdk用于开发程序（也可运行），jre只用于运行程序。


### 开发环境搭建
1. 【待完善】下载安装jdk
    此处未完成，仅列大纲，供后续更新参考。
    - 下载不同版本的JDK：
        - 官方：[下载链接](https://www.oracle.com/java/technologies/downloads/)
        - 百度网盘：[下载链接](https://pan.baidu.com/s/1nCIs0X072X0jma9H-0R0KQ?pwd=p2wd)
    - 在不同系统、权限下安装jdk
    - 对于windows，32位系统下载x86版本，64位系统下载x64版本。
2. **下面以安装路径为`D:\programs\jdk`为例。**
3. 配置环境变量（Windows系统）
    - `此电脑-属性-高级系统设置-环境变量`
    - 添加变量`JAVA_HOME`，内容为`D:\programs\jdk`，即jdk安装目录。
    - 编辑`path`环境变量，增加`%JAVA_HOME%\bin`
    - 打开命令行，输入`java`或`javac`，若没提示`找不到变量`，则配置成功。

### 学习方法
如何快速学习？如何寻找学习动力？

1. 为什么学习？（需求）
    - 工作需要
    - 跳槽、对方要求
    - 兴趣、技术控

2. 能否使用传统/现有技术解决？（解决需求）
    - 能解决，但是不完美
    - 解决不了

3. 引出我们学习的新技术和知识点
    明确要学的知识点。

4. 先学习新技术或知识点的基本原理和基本语法。（不要考虑细节）

5. 快速入门案例（基本程序，如CRUD）
    有了“增删改查”的基本能力，就可以上手了。

6. 开始研究技术的注意事项、使用细节、使用规范、如何规范。
    优化是永无止境的，细节要最后扣。




### 快速入门：Hello World!

1. 课程中提到的所有程序，都将遵循以下步骤：
    - 提出需求
    - 列出开发步骤
    - 阐明原理
    - 讲明细节


1. 需求
    - 写一个`Hello.java`程序，输出`Hello,World!`
2. 代码
    ```java
    // Java 快速入门，演示Java的开发步骤

    //对代码的相关说明
    //1. public class Hello 表示："Hello"是一个public(公有)类。
    //2. public static void main(String[] args)表示程序主方法，是程序的入口。
    //3. Java程序需要分号";"表示语句结束。
    public class Hello{

        // 编写主方法

        public static void main(String[] args){
            System.out.println("Hello,World!");
        }
    }
    ```
2. 命令行
    ```bash
    javac Hello.java
    java Hello
    ```
3. 错误解析
    - 如果使用的是之前提到的Sublime Text，可能无法编译（如下），这可能是因为控制台中文编码与文本编辑器的编码不一致导致的。
        ```bash
        hello.java:1: 错误: 编码GBK的不可映射字符
        // Java 蹇?熷叆闂紝婕旂ずJava鐨勫紑鍙戞楠?
        ```
        解决方法：
        用管理员身份打开命令行，右键顶端进入属性，确认编码格式（上述错误信息中显示为GBK）。
        然后将文本编辑器保存的编码设置为与命令行编码一致。

        解决方法2：强制指定编码格式。
        ```bash
        javac -encoding utf-8 Hello.java
        ```

    - 文件名不一致
        ```bash
        E:\code\HSPJava>javac -encoding UTF-8 hello.java
        hello.java:7: 错误: 类Hello是公共的, 应在名为 Hello.java 的文件中声明
        public class Hello{
            ^
        1 个错误
        ```
        这是因为Java严格区分文件名、类名的大小写，如果文件名与类名不一致，编译器会报错。
        这里类名为`Hello`，但文件名为`hello.java`，所以编译器报错。

    - 错误的运行方式
        ```bash
        E:\code\HSPJava>java Hello.class
        错误: 找不到或无法加载主类 Hello.class
        ```
        这里运行时，不需要带`.class`后缀，直接`java Hello`即可。
        ```bash
        E:\code\HSPJava>java Hello
        Hello,World!
        ```

4. 其他注意事项
    - Java源文件以`.java`为扩展名，源文件的基本组成部分是类`class`。
    - Java程序的执行入口是main()方法，有固定的书写格式，不得更改。
        ```java
        public static void main(String[] args){...}
        ```
    - 一个源文件中只能有一个public类，且类名必须与源文件名一致。但其他类名不限。
        ```java
        public class Hello{...}

        class Dog{...}

        class Cat{...}
        ```
        此时编译，会生成三个文件，`Hello.class`，`Dog.class`，`Cat.class`。
        即：编译后，每一个类对应一个`.class`文件
    - 如果源文件包含一个`public`类，则源文件名必须与此类相同。
    - 可以将main方法卸载非public类中，然后指定运行非public类，这样入口方法就是非public类的main方法。这样的类不可以跨包访问。


### 转义字符
1. Java常用转义字符
    - `\t`：制表位
    - `\n`：换行符
    - `\\`：`\`符号
    - `\"`：`"`符号
    - `\'`：`'`符号
    - `\r`：回车不换行

2. 演示代码
    ```java
    public class ChangeChar{

        public static void main(String[] args){
            //制表符
            System.out.println("北京\t天津\t上海");
            //换行转义符
            System.out.println("jack\nsmith\nmary");
            //斜杠转义符
            System.out.println("E:\\code\\HSPJava");
            //引号转义符
            System.out.println("\"早上好\"");
            System.out.println("\'早上好\'");
            // '\r'
            System.out.println("早上好中午好\r晚上好");
        }
    }
    ```
3. 练习代码
    ```java
    public class ChangeCharExer01{

        public static void main(String[] args){
            System.out.println("书名\t价格\t作者\t销量");
            System.out.println("三国\t罗贯中\t120\t1000");
        }
    }
    ```
    ```bash
    E:\code\HSPJava>java ChangeCharExer01
    书名    价格    作者    销量
    三国    罗贯中  120     1000
    ```

### 注释
1. 特点
    - 注释是注解说明解释程序的文字。**注释是程序的重要组成部分。**
    - 注释部分不会被JVM解释执行
2. 对比
    - 坏的代码
        ```java
        int a = 10;
        int b = 20;
        int c = a + b;
        System.out.println(c);
        ```
    - 好的代码
        ```java
        // 下面的代码是一个简单的加法程序

        // 定义变量
        int a = 10;
        int b = 20;

        // 计算
        int c = a + b;

        // 输出结果
        System.out.println(c);
        ```
3. 分类
    - 单行注释
    - 多行注释
    - 文档注释

4. 单行注释
    使用`//`作为一行开头的注释。
5. 多行注释
    ```java
    /*  早上好
        中午好
        晚上好 */
    ```
    注意：多行注释不能嵌套。下列方式是不被允许的。
    ```java
    /*  早上好
    /*  中午好 */
        晚上好 */
    ```
6. 文档注释
    - 注释内容可以被JDK提供的javadoc解析，生成以网页文件形式体现的该程序的说明文档。
    - 文档注释以`/**`为程序开头，以`*/`为程序结尾。其中每行以`*`开头。
    ```java
    /**
     * @author: lcq
     * @version: 1.0
    */
    ```
    - 使用如下命令生成文档注释：
        ```bash
        javadoc -d <dir> -xx -yy <java file>
        ```
        - `-d`：指定输出目录
        - `xx`和`yy`：表示需要生成的javadoc标签。
    
    - 【长期补充】可用的javadoc标签：
        - `@author`：作者
        - `@version`：版本
        - `@param`：参数
        - `@return`：返回值
        - `@throws`：异常
        - `@see`：相关类
### Java代码规范
1. 类、方法的注释，要以Javadoc的方式写。
2. 非Javadoc的注释（单行/多行注释），往往是给代码维护者看的，着重告诉读者为什么这么写，如何修改，注意什么问题。
3. 运算符和`=`通常习惯在两边空一格。`c = a + b;`
4. 源文件使用UTF-8编码。
5. 行宽度不要超过80个字符。
6. 对于大括号，分为行尾风格和次行风格两种。推荐行尾风格。

### 【了解】Dos
运行中使用cmd打开的界面就是Dos系统（Disk Operating System，磁盘操作系统）

#### 常用指令


1. **`dir`**  
   **列出目录内容**，支持多种参数筛选和格式化输出  
   ```bash
   dir E:\            # 列出E盘根目录所有文件和文件夹
   dir *.txt /s /p   # 递归查找所有.txt文件并分页显示
   dir /ah           # 显示当前目录下的隐藏文件
   ```  
   **参数说明**：  
   • `/s`：递归显示子目录内容  
   • `/p`：分页显示（按任意键继续）  
   • `/w`：紧凑模式（每行显示5项）  
   • `/a`：显示包括隐藏文件的所有文件（如`/ah`仅显示隐藏文件）  
   **特性**：支持通配符（如`dir *.exe`）和路径重定向（如`dir > filelist.txt`）

2. **`cd`**  
   **切换工作目录**，支持跨驱动器操作  
   ```bash
   cd D:\Project     # 进入D盘Project目录
   cd ..             # 返回上级目录
   cd /d E:\Backup   # 切换到E盘Backup目录（跨驱动器必须加/d）
   ```  
   **特殊用法**：  
   • `cd \`：返回当前驱动器的根目录  
   • `cd ..\..`：向上返回两级目录  
   • `cd`（不带参数）：显示当前完整路径  

3. **`tree`**  
   **显示目录树结构**，支持文件列表导出  
   ```bash
   tree D:\          # 显示D盘目录树结构
   tree /F > tree.txt # 将目录及文件结构导出为文本文件
   tree /A           # 使用ASCII字符替代扩展字符（兼容性更好）
   ```  
   **参数说明**：  
   • `/F`：显示每个目录中的文件名  
   • `/A`：简化图形符号（适合纯文本环境）  
   **注意**：路径中若含空格需用引号包裹（如`tree "C:\Program Files"`）

4. **`cls`**  
   **清空屏幕内容**，仅保留命令行提示符  
   ```bash
   cls  # 执行后屏幕内容完全清除，不影响系统状态
   ```  
   **适用场景**：  
   • 连续操作后需整理界面  
   • 执行敏感操作前隐藏历史命令（但可通过↑键回溯）

5. **`exit`**  
   **退出命令行窗口或批处理脚本**  
   ```bash
   exit        # 关闭当前CMD窗口
   exit /b     # 退出批处理脚本但不关闭窗口
   ```  
   **扩展用途**：  
   • 在脚本中配合错误码（如`exit 1`表示异常终止）  
   • 终止远程桌面会话（需结合其他命令）

可通过 `命令 /?`（如 `dir /?`）查看详细参数说明，或在CMD中输入 `help` 获取完整命令列表。

6. `md`（Make Directory）  
    **创建新目录**，相当于Linux的`mkdir`  
    ```bash
    md project       # 在当前目录创建project文件夹
    md D:\backup\2025  # 在D盘backup目录下创建2025子目录
    ```  
    

7. `rd`（Remove Directory）  
    **删除空目录**，相当于Linux的`rmdir`  
    ```bash
    rd temp          # 删除当前目录下的空temp文件夹
    rd /s /q D:\old  # 强制删除D盘old目录及其所有子文件（/s递归删除，/q静默模式）
    ```  
    **注意**：普通rd只能删除空目录，非空目录需配合/s参数

8. `del`（Delete）  
    **删除文件**，相当于Linux的`rm`  
    ```bash
    del test.txt     # 删除当前目录下的test.txt
    del *.tmp /s /q  # 递归删除所有子目录中的.tmp文件（/s包含子目录，/q不提示确认）
    ```  
    **特性**：  
    ◦ 支持通配符（如`del *.bak`）  
    ◦ /F参数强制删除只读文件  
    ◦ 无法直接删除目录（需配合rd）

9. `copy`  
    **文件复制**，相当于Linux的`cp`  
    ```bash
    copy a.txt b.txt        # 复制a.txt为b.txt
    copy *.log D:\logs      # 复制所有.log文件到D盘logs目录
    copy 1.txt+2.txt sum.txt # 合并1.txt和2.txt为sum.txt
    ```  
    **高级用法**：  
    ◦ 支持文件合并（`copy file1+file2 merged`）  
    ◦ /Y参数自动覆盖目标文件

10. `echo`  
    **显示信息或创建文件**  
    ```bash
    echo Hello World        # 输出"Hello World"
    echo. > newfile.txt     # 创建空文件（.代表空内容）
    echo %PATH%            # 显示环境变量PATH的值
    ```  
    **注意**：`echo off`常用于批处理脚本开头隐藏命令回显

11. `type`  
    **显示文本文件内容**，相当于Linux的`cat`  
    ```bash
    type config.ini        # 显示当前目录config.ini内容
    type D:\readme.txt | more  # 分页显示长文本（配合more命令）
    ```  
    **限制**：  
    ◦ 不支持通配符（如`type *.txt`无效）  
    ◦ 非文本文件（如.exe）会显示乱码

12. `move`  
    **移动文件/重命名**，相当于Linux的`mv`  
    ```bash
    move report.doc D:\docs       # 移动文件到指定目录
    move oldname.txt newname.txt  # 重命名文件
    move /Y *.jpg \photos        # 移动所有.jpg到photos目录（/Y覆盖不提示）
    ```  
    **特性**：  
    ◦ 可同时完成移动和重命名操作  
    ◦ 支持通配符批量操作

### 章节作业
1. 编写Hello,world程序
2. 将个人基本信息（姓名、性别、籍贯、住址）打印到控制台，每条信息占一行
3. JDK、JVM、JRE的关系
4. 环境变量path的配置机器作用
5. Java的编写步骤
6. Java编写的7个规范
7. 初学者学Java的易犯错误

## 变量
### 章节目录
1. 变量介绍
2. `+`符号的使用
3. 数据类型
4. 编码
5. 数据类型转换

### 变量介绍
1. 变量是程序的基本组成单位。
2. 变量有三要素：**类型、名称、值**
    ```java
    class Test{
        public static void main(String[] args){
            int a = 1 ;             // 变量a的类型是int，名称是a，值是1
            int b = 3 ;             // 变量b的类型是int，名称是b，值是3
            b = 80;                 // 变量b的值被修改为80

            System.out.println(a);  // 输出变量a的值
            System.out.println(b);  // 输出变量b的值
        }
    }
    ```

3. 变量的使用步骤
    - 声明变量：`int a`
    - 赋值：`a = 10`
    - 使用

4. 变量使用注意事项
    - 变量表示内存中的一个存储区域，不同的变量，类型不同，占用的空间也不同。
        比如：int占4个字节，double占8个字节。（后面会详细讲）
    - 变量需要有名称和类型。
    - 变量使用前必须先声明，后使用。
    - 变量的值可以在**同类型下**变换。
    - 变量在同一个作用域中不能重名。
    - 变量 = 类型 + 变量名 + 值。

### `+`符号的使用
1. 实例1
    ```java 
    System.out.println(100 + 98);// 198
    System.out.println("100" + 98);// 10098
    System.out.println(100 + 3 + "hello");// 103hello
    System.out.println("hello" + 100 + 3);// hello1003
    ```
    - 运算顺序从左至右。
    - 当左右两边均为数值时，做加法运算。
    - 当左右两边有一边为字符串时，做拼接运算。

### 数据类型
1. 每一种数据都定义了明确的数据类型，在内存中分配了不同大小的内存空间（字节）。
    ![java_data_type](./img/java_data_type.png)
    - 图中内容务必记住。
    - Java数据类型分为两大类：基本数据类型和引用类型
    - 基本数据类型有8种：`byte`、`short`、`int`、`long`、`float`、`double`、`char`、`boolean`。
    - 引用数据类型有3种：类`class`、接口`interface`、数组`[]`。
    - **字符串不是基本数据类型。**

#### 整数类型
1. 整形的类型
    |类型|占用存储空间|范围|
    |:---:|:---:|:---:|
    |`byte`|1字节|-128~127|
    |`short`|2字节|-32768~32767<br>$-2^{15} \sim 2^{15}-1$|
    |`int`|4字节|-2147483648~2147483647<br>$-2^{31} \sim 2^{31}-1$|
    |`long`|8字节|-9223372036854775808~9223372036854775807<br>$-2^{63} \sim 2^{63}-1$|

2. 特性
    - Java各整数类型有固定的范围和字段长度，不受具体OS（操作系统）的影响，以保证Java程序的可移植性。
    - Java的整形**常量**默认为`int`型，声明`long`型需要在常量后加上`L`或`l`。
        ```java
        int i = 100;    // 默认int类型
        long l = 100L;  // long类型
        ```
    - Java程序中变量常声明为`int`型，只有不足以表示大数时，才使用`long`型。
    - `bit`是计算机中最小的存储单位，1byte = 8bit。

#### 浮点类型
1. 分类
    |类型|占用存储空间|范围|
    |:---:|:---:|:---:|
    |`float`|4字节|$-3.403\times10^{38} \sim 3.403\times10^{38}$|
    |`double`|8字节|$-1.798\times10^{308} \sim 1.798\times10^{308}$|

2. 特性
    - 浮点数在机器中存放的形式：符号位 + 指数位 + 尾数位。
    - 尾数部分可能丢失，造成精度损失。所以小数都是近似值。
    - Java中的浮点型常量默认为`double`类型，声明`float`型常量，须在常量末尾加上`f`或`F`。
        ```java
        float f = 1.0f;
        double d = 1.0;
        ```
    - 去
        ```java
        double num1 = 2.123456789123 // 2.123456789123
        double num2 = 2.123456789123f // 2.1234567
        ```

3. 浮点数的两种表示形式
    - 十进制：`5.12`、`512.0f`、`.512`（可以把小数点前的`0`取消）
    - 科学计数法：`5.12E2`（$5.12 \times 10^2$）、 `512E-2`（$512 \times 10^{-2}$）

4. 一个陷阱
    ```java
    double num1 = 2.7;
    double num2 = 8.1 / 3; // 2.6666666666666667

    // 通过Java API判断两者是否相等
    System.out.println(Math.abs(num1 - num2));
    ```
    - 计算小数时，可以先将小数乘以10，运算完成后再除以10。
    - 浮点数之间不能进行相等判断，而是改为将两者作差，看两者误差是否位于一定精度内。
        判断的误差按照实际需求即可。
    - 在线API网站：Matools.com

4. 【题外话】为什么0.1+0.2不等于0.3？
    首先，要解决一个问题：**什么是浮点数？**
    前面已经讲过，一个浮点数由三部分组成。其中的尾数位，`float`类型有23位，`double`类型有52位。
    $$
    2^{-23} = 1.1920929 \times 10^{-7} \\
    2^{-52} = 2.2204460492503131 \times 10^{-16}
    $$
    这就是为什么单精度提供约7位精度，双精度提供约16位精度。

    这里给出单精度的计算，双精度同理。（不一定正确，仅在此处进行一下记录）
    $$
    {(0.1)}_{10} = (0.00011001100110011001101)_2 \\
    {(0.2)}_{10} = (0.00110011001100110011010)_2 \\
    {(0.1 + 0.2)}_{10} = (0.01001100110011001100110)_2 = 0.3000000715… \\
    $$

#### 字符类型
1. 基本介绍
    - 字符类型可以表示单个字符。
    - 多个字符用字符串`String`
    - 字符型变量可以存放：中英文字符、转义字符、数字。
        ```java
        char ch = 'a';
        char ch1 = '中';
        char ch2 = '\\';
        char ch3 = 97;

        System.out.println(ch);     //a
        System.out.println(ch1);    //中
        System.out.println(ch2);    //'\'
        System.out.println(ch3);    //a     数字会由编码转换
        ```

2. 注意事项
    - 字符常量用单引号括起来。
    - 字符常量可以是转义字符。
    - 字符常来那个的本质是一个整数，输出的是Unicode码对应的字符。
    - `char`类型可以进行运算，因为其本质上是一个整数。

#### 布尔类型
1. 基本介绍
    - `boolean`类型只有两种值：`True`和`False`。
    - `boolean`类型占1字节。
    - `boolean`类型适用于逻辑运算，一般用于程序流程控制。
        `if`、`while`、`do-while`、`for`等。
    - **Java不能用0或非0表示布尔值。**

### 基本数据类型转换

#### 自动类型转换

1. 基本介绍
    Java程序在进行赋值或运算时，精度小的类型会自动转换为精度大的类型。
    - 数据类型按精度大小排序：（从左至右自动转换）
        - `char`、`int`、`long`、`float`、`double`
        - `byte`、`short`、`int`、`long`、`float`、`double`
2. 特性
    - 有多种类型数据混合运算时，系统首先将所有数据转换成容量最大的那种数据类型。
    - 将精度大的数据赋值给精度小的类型时，会出现精度损失，会报错。反之则自动转换类型。
    - （`byte`，`short`）和`char`之间不能进行自动类型转换。
        但他们之间可以进行运算，运算时先将类型全部转换为`int`。
    - `boolean`类型不参与类型转换。

#### 强制类型转换

1. 基本介绍
    强制类型转换可以将精度大的数据类型转换成精度小的数据类型，**但这个过程可能造成精度损失**。
    - 用强制转换符`()`进行强制转换。
2. 举例
    ```java
    int i = (int)1.9;
    System.out.println(i); // 1
    
    int n = (byte)2000;
    System.out.println(n); // -48
    ```
    为什么会发生这种情况？
    $$
    (2000)_{10}=(11111010000)_2 \\
    (11011111)_{补码} = (10110000)_{原码} = (-48)_{10}
    $$
3. 使用技巧
    - 强制转换符号只对最近的操作数生效，可以通过增加小括号提升优先级。
        ```java
        int n1 = (int)10*3.5+6*1.5;     //仅对10生效，编译时会报错。
        int n2 = (int)(10*3.5+6*1.5);   //对后面整个括号的运算结果生效。
        ```
    - **`char`类型可以保存`int`类型常量，但不能保存`int`变量，需要强制转换。**
        ```java
        char c1 = 100;
        int n1 = 100;

        char c2 = n1;       //报错，两端类型不匹配，不能进行赋值操作。
        char c3 = (char)n1; //正常编译，两端类型经过强制转换后相同。
        ```
#### 基本数据类型与字符串转换。
1. 将基本数据类型转换成字符串。
    采用原有数据加一个空字符串的方法
    ```java
    int n1 = 100;
    float f1 = 1.1F;
    double d1 = 4.5;
    boolean b1 = true;
    
    // 字符串在+号任意一边，运算结果为字符串。
    String s1 = n1 + "";
    String s2 = f1 + "";
    String s3 = d1 + "";
    String s4 = b1 + "";
    System.out.println(s1 + " " + s2 + " " + s3 + " " + s4)
    ```

2. 将字符串转换为基本数据类型。
    通过基本类型的包装类调用`parseXX`方法。
    ```java
    String s5 = "123";
    
    int num1 = Integer.parseInt(s5);            //123
    double num2 = Double.parseDouble(s5);       //123.0
    float num3 = Float.parseFloat(s5);          //123.0
    long num4 = Long.parseLong(s5);             //123
    byte num5 = Byte.parseByte(s5);             //123
    boolean b = Boolean.parseBoolean("true");   //true
    short num6 = Short.parseShort(s5);          //123
    ```

3. 取字符串中的单个字符
    ```java
    String s5 = "123"
    System.out.println(s5.charAt(0));           //1
    ```

4. 注意事项
    - 在将`String`类型转换成基本数据类型时，要确保`String`类型能够转成有效的数据。
        比如`"hello"`无论如何都不可能转换成一个整数。
    - 如果格式不正确，就会抛出异常，**抛出异常，程序就会终止**。

## 运算符
### 章节目录
1. 运算符介绍
2. 算术运算符
3. 关系运算符（比较运算符）
4. 逻辑运算符
4. 位运算符
5. 赋值运算符
6. 三元运算符
7. 运算符优先级

### 算术运算符
1. 介绍
    - 对数值类型的变量进行运算。
    - 
2. 常用的算术运算符
    ![java_operator](./img/java_operator.png)

#### 易错点
1. `%`
    在Java中，`%`运算遵循以下公式：
    `a % b = a - (a / b) * b`
    ```java 
    System.out.println(10 % 3);     // 1
    System.out.println(-10 % 3);    // -1
    System.out.println(10 % -3);    // 1
    System.out.println(-10 % -3);   // -1
    ```
    从上面的输出结果可以看出，取模运算的符号仅与右侧操作数正负有关。

2. 面试题1
    下列两段代码的输出结果。
    ```java
    int i = 1;
    i = i++;
    System.out.println(i);  // 1
    ```
    ```java
    int i = 1;
    i = ++i;
    System.out.println(i);  // 2
    ```
    造成这个现象的原因，可以这样理解：
    在对变量进行赋值时，执行了两个步骤：
    - 将变量值赋给一个临时变量`temp = i`
    - 将`temp`的值赋给变量`i`
    在第一段代码中，`i++`先将`i`的值赋给`temp`，然后再将`temp`的值赋给`i`，所以`i`的值还是`1`。
    在第二段代码中，`++i`先将`i`的值加1，然后再将`i`的值赋给`temp`，所以`i`的值是`2`。

3. 华氏度计算

### 关系运算符
1. 常用的关系运算符
    ![java_operator_relation](./img/java_operator_relation.png)

### 逻辑运算符
1. 常用的逻辑运算符
    ![java_operator_logic](./img/java_operator_logic.png)
2. 说明
    分为两组：
    - 短路与`&&`、短路或`||`、取反`!`
    - 与`&`、或`|`、异或`^`。
3. 逻辑与和短路与的区别
    - 短路运算：当左边为`false`时，右边不再运算。
    - 逻辑运算：当左边为`false`时，右边继续运算。
    ```java
    int a = 10;
    int b = 20;
    if (a < 0 && b++ > 0) {
        System.out.println("aha");
    }
    System.out.println(b);// 20
    if (a < 0 & b++ > 0) {
        System.out.println("aha2");
    }
    System.out.println(b);// 21
    ```
    - 短路或运算：当左边为`true`时，右边不再运算。
    - **在开发中，为提升效率，一般使用短路运算。**

### 赋值运算符
1. 简介
    - 基本赋值运算符：`=`

    - 复合赋值运算符：`+=`,`-=`,`*=`,`/=`,`%=`

2. 特点
    - 赋值运算符的运算顺序为从右向左。
    - 赋值运算符左边只能是变量，右边可以是变量、常量、表达式。
    - **复合运算符进行计算时会自带强制类型转换。**

### 三元运算符
1. 介绍
    - `条件表达式？表达式1:表达式2`
    - 如果条件表达式为`true`，则返回表达式1，`false`则返回表达式2。
2. 使用细节
    - 表达式1和表达式2要为可以赋值给接收变量的类型（或可以自动转换/进行强制类型转换）。
    - 三元运算符可以转为`if-else`语句。

### 运算符优先级
1. 运算符优先级排序（从上至下优先级逐渐降低）
    ![java_operator_priority](./img/java_operator_priority.png)
    - 只有单目运算符和赋值运算符从右向左运算
    - 括号优先级最高，赋值运算优先级最低。
    - 可以这样记：（不推荐硬记，用多了就会了）
        括号>单目运算符>算术运算符>位移运算符>比较运算符>逻辑运算符>三元运算符>赋值运算符

### 进制
1. 介绍
    - 二进制（Binary）：以`0B`或`0b`开头表示。用01表示。
    - 十进制（Decimal）
    - 八进制（Octal）：以`0`开头表示。用0-7表示。
    - 十六进制（Hexadecimal）：以`0X`或`0x`开头表示。用0-9、A-F表示。
2. 代码
    ```java
    //n1 二进制
    int n1 = 0b1010;
    //n2 10 进制
    int n2 = 1010;
    //n3 8 进制
    int n3 = 01010;
    //n4 16 进制
    int n4 = 0X10101;
    System.out.println("n1=" + n1); //输出结果：n1=10
    System.out.println("n2=" + n2); //输出结果：n2=1010
    System.out.println("n3=" + n3); //输出结果：n3=520
    System.out.println("n4=" + n4); //输出结果：n4=65793
    System.out.println(0x23A);      //输出结果：570
    ```
3. 进制转换（分四组）
    - 第一组：二/八/十六进制转十进制
    - 第二组：十进制转二/八/十六进制
    - 第三组：二进制转八/十六进制
    - 第四组：八/十六进制转二进制
### 位运算
1. 符号
    - `~`：按位取反
    - `&`：按位与
    - `|`：按位或
    - `^`：按位异或
    - `>>`：算术右移
    - `<<`：算术左移
    - `>>>`：逻辑右移
2. 特性
    - **所有的运算都是对补码进行运算。**
    - 算术右移`>>`：低位溢出，符号位不变，用符号位补高位。
    - 算术左移`<<`：符号位不变，低位补0。
    - 逻辑右移`>>>`：又称无符号右移，低位溢出，高位补0。
    - **没有`<<<`符号。**

3. 简单计算与推导
    - `2&3`
        $$
        \begin{aligned}
        (2)_{10} &= (\texttt{00000000}\ \texttt{00000000}\ \texttt{00000000}\ \texttt{00000010})_{\text{原码}} \\
        &= (\texttt{00000000}\ \texttt{00000000}\ \texttt{00000000}\ \texttt{00000010})_{\text{补码}} \\
        (3)_{10} &= (\texttt{00000000}\ \texttt{00000000}\ \texttt{00000000}\ \texttt{00000011})_{\text{原码}} \\
        &= (\texttt{00000000}\ \texttt{00000000}\ \texttt{00000000}\ \texttt{00000011})_{\text{补码}} \\
        (2)_{10} \mathbin{\&} (3)_{10} &= (\texttt{00000000}\; \texttt{00000000}\; \texttt{00000000}\; \texttt{00000010}) \\ &\mathbin{\&} (\texttt{00000000}\; \texttt{00000000}\; \texttt{00000000}\; \texttt{00000011}) \\
        &= (\texttt{00000000}\ \texttt{00000000}\ \texttt{00000000}\ \texttt{00000010}) = (2)_{10}
        \end{aligned}
        $$
2. 一段代码（位运算）
    ```java
    System.out.println(2&3); // 输出 2
    System.out.println(~-2); // 输出 1
    System.out.println(~2);  // 输出 -3
    System.out.println(2|3); // 输出 3
    System.out.println(2^3); // 输出 1
    ```
2. 一段代码（移位运算）
    ```java
    int a=1>>2; // 1 向右位移 2 位
    int b=-1>>2;//算术右移
    int c=1<<2;//算术左移
    int d=-1<<2;//
    int e=3>>>2;//无符号右移
    //a,b,c,d,e 结果是多少
    System.out.println("a="+a);
    System.out.println("b="+b);
    System.out.println("c="+c);
    System.out.println("d="+d);
    System.out.println("e="+e);
    ```
    - 对于左移和右移，实际上就是乘2和除2，除2为右移，乘2为左移。
6. 【待补充】本部分仍有完善空间，由于时间紧迫，暂时不做展开
    - 【循环】多次左右移的数字对比
    - 【补码】负数的移位运算
    - 逻辑右移的结果。
### 附加内容
#### 标识符的规则与规范
1. 概念
    - Java对各种变量、方法、类等命名时使用的字符序列称为标识符。
2. 规则（必须遵守）
    - 由26个英文字母的大小写、10个阿拉伯数字、下划线`_`、美元符号`$`组成。
    - 数字不可以作为标识符的开头。
    - 不可以使用**关键字**和**保留字**。
    - **Java严格区分标识符大小写，但对长度无限制。**
    - 标识符不得包含空格。

3. 规范（不强制推荐采取的命名方式）
    - 包名：多个单词组成时，所有字母小写。如：`com.han.sp`
    - 类名、接口名：多个单词组成时，每个单词首字母大写，其余小写。如：`HanSP`（大驼峰法）
    - 变量名、方法名：多个单词组成时，第一个单词首字母小写，其余大写。如：`hanSP`（小驼峰法）
    - 常量名：多个单词组成时，每个单词所有字母大写，其余小写。如：`HAN_SP`（全大写）

4. 关键字和保留字
    - 关键字：被Java语言赋予特殊含义，用作专门用途的字符串。
        ![java_keywords](./img/java_kewords.png)
    - 保留字：现有Java版本尚未使用，但以后版本可能会作为关键字使用的字符串。
        `byValue`、`cast`、`future`、`generic`、`inner`、`operator`、`outer`、`rest`、`var`、`goto`、`const`等。

#### 键盘输入语句
1. 介绍
    - Java中，如果要接收用户输入的数据，就可以使用键盘输入语句。
    - 需要一个扫描器对象`Scanner`。
2. 使用步骤
    - 导入该类所在包`java.util.*`。
    - 创建一个`Scanner`对象。
3. 代码实例`Input.java`
    ```java
    import java.util.*;	//导入java.util下的Scanner类

    public class Input{

        public static void main(String[] args) {
                
            // 创建 Scanner 对象
            Scanner scanner = new Scanner(System.in);

            // 接收用户输入
            System.out.println("输入内容（姓名）：");
            String name = scanner.next();

            System.out.println("输入内容（年龄）：");
            int age = scanner.nextInt();

            System.out.println("输入内容（薪水）：");
            Double salary = scanner.nextDouble();

            System.out.println("基本信息如下");
            System.out.println("姓名\t年龄\t薪水");
            System.out.println(name + "\t" + age + "\t" + salary + "\t" );
            }	
    }
    /************************
    输入内容（姓名）：
    li
    输入内容（年龄）：
    24
    输入内容（薪水）：
    223333
    基本信息如下
    姓名    年龄    薪水
    li      24      223333.0
    ************************/
    ```

#### 【待补充】原码、反码、补码
我会，等我学完回来补
1. **Java没有无符号数**

## 程序控制结构
### 章节目录
1. 顺序控制
2. 分支控制（`if-else`、`switch`）
3. 循环控制（`for`、`while`、`do-while`）
4. `break`
5. `continue`
6. `return`

### 顺序控制
1. 介绍
    - 程序从上到下逐行执行，中间没有任何跳转和判断。
    - **顺序控制是程序默认的控制方式。**
    - Java定义变量时采用前向引用的方式，即先声明后调用。
### 分支控制
#### 单分支（`if`）
1. 基本语法
    ```java
    if(条件表达式){
        执行代码块;
    }
    ```
    - 当条件表达式为`true`时，执行代码块。
    - 当条件表达式为`false`时，不执行代码块。
    - 如果执行代码只有一行，可以不加大括号。（不推荐）
2. 流程图
    ![java_control_branch_if](./img/java_control_branch_if.png)
#### 双分支
1. 基本语法
    ```java
    if(条件表达式){
        执行代码块1;
    }
    else{
        执行代码块2;
    }
    ```
2. 流程图
    ![java_control_branch_if_else](./img/java_control_branch_if_else.png)
#### 多分支
1. 基本语法
    ```java
    if(条件表达式1){
        执行代码块1;
    } 
    else if(条件表达式2){
        执行代码块2;
    }
    ...
    else {
        执行代码块n;
    }
    ```
2. 流程图
    ![java_control_branch_if_elif](./img/java_control_branch_if_elif.png)

#### 嵌套分支
1. 基本语法
    ```java
    if(条件表达式1){
        if(){
            //if-else
        }else{
            //if-else
        }
    }
    ```
    - **为保证程序可读性，建议嵌套不要超过三层。**

#### `switch`分支结构
1. 基本语法
    ```java
    switch(表达式){
        case 常量值1:
            执行代码块1;break;
        case 常量值2:
            执行代码块2;break;
        ...
        case 常量值n:
            执行代码块n;break;
        default:
            执行代码块n+1;
            break;
    }
    ```
2. 流程图
    ![java_control_branch_switch](./img/java_control_branch_switch.png)
4. 注意事项
    - 标的是的数据类型应与`case`后的常亮数据类型一致（或可以转换成对应的类型）。否则会报错。
    - `switch`的表达式的返回值必须是以下类型中的一种：
        `byte`、`short`、`int`、`char`、`num`、`String`
    - **`case`子句中的值只能是常量，不能是变量。**
    - `default`子句是可选的。
    - `switch`结构具有穿透的特点，会从符合条件的`case`处执行到最后，而`break`用于跳出代码块。

### 循环控制

#### for循环

1. 基本语法
    ```java
    for(循环变量初始化;循环条件;循环变量迭代){
        循环操作;
    }
    ```
2. 流程图：
    ![java_control_loop_for](./img/java_control_loop_for.png)

3. 注意事项
    - 循环条件返回一个布尔值表达式。
    - 初始化和变量迭代可以写在别的地方，**但分号不能省略**。
    - 初始化变量可以有多条语句，用逗号隔开，**要求类型相同**。

#### while循环
1. 基本语法
    ```java
    while(条件表达式){
        循环体语句;
        变量迭代;
    }
    ```
2. 流程图
    ![java_control_loop_while](./img/java_control_loop_while.png)

#### do-while循环
1. 基本语法
    ```java
    do{
        循环体语句;
        变量迭代;
    }while(条件表达式);
    ```
2. 流程图
    ![java_control_loop_dowhile](./img/java_control_loop_dowhile.png)
 
#### 多重循环
1. 介绍
    - 将一个循环嵌套在另一个循环中，称为多重循环。
    - 建议一般使用2层循环，3层循环以上不建议使用。

### 其他控制语句
#### `break`
1. 流程图
    ![java_control_jump_break](./img/java_control_jump_break.png)
2. 使用细节
    - `break`语句可以通过标签指明终止哪一层的语句块。
        ```java
        lable1:
        for(int i=0;i<10;i++){
            lable2:
            for(int j=0;j<10;j++){
                label3:
                for(int k=0;k<10;k++){
                    if(k==5){
                        break lable2;
                    }
                }
            }
        }
        ```
        建议在实际开发中尽量不使用标签，这会降低代码的可读性。

#### `continue`
1. 流程图
    ![java_control_jump_continue](./img/java_control_jump_continue.png)

#### `return`
1. 介绍
    - 当`return`被用于一个方法时，其将结束当前方法并返回值。
    - 如果`return`被应用于主方法，将直接退出程序。

## 数组、排序、查找
### 章节目录
1. 数组
2. 排序
3. 查找
4. 多维数组

### 数组
1. 引例
    给n个母鸡称重
    ```java
    public class Array01{
        public static void main (String[] args){
            double[] hens = {1,2,3,4,56,73,5,23};       //定义一个double类型的数组
            double totalWeight = 0;                     //定义累加器
            //遍历数组
            for (int i = 0; i < hens.length; i++){                             // 通过数组的length方法获得数组长度
                System.out.println("第"+(i+1)+"个母鸡的重量为"+hens[i]+"kg");   // 可以通过下标访问数组中的元素
                totalWeight += hens[i];
            }
            System.out.println("平均值：" + (totalWeight/hens.length));
            System.out.println("总重：" + (totalWeight));
            
        }
    }
    ```
2. 数组的动态初始化
    - 用法1
        - 数组定义
            ```java
            int[] array01 = {}      // 第1种：将中括号写在数据类型后
            int array02[] = {}      // 第2种：将中括号写在数组名后
            int array03[] = new int[5]  // 定义一个长度为5的数组，存放5个int类型数据，初始值为0
            ```
            通用的定义格式：`数据类型 数组名[] = new 数据类型[大小]`
        - 数组的引用（使用）
            `array01[i]`：访问数组`array01`中的第`i+1`个数。
    -  用法2
        - 数组声明
            `数据类型 数组名[];`
            `数据类型[] 数组名;`
        - 创建数组
            `数组名 = new 数据类型[大小]`
        ```java
        int[] arrayint01;
        arrayint01 = new int[5];
        ```

3. 数组的静态初始化
    如果知道数组的大小，则可以使用静态初始化。
    -  语法
        `数据类型 数组名[] = {值1, 值2, 值3, ...};`

4. 注意事项
    - 数组是多个**相同类型**数据的组合。
    - 数组中的元素可以是任何数据类型，包括基本类型和引用类型，但不能混用
    - 数组创建后，如果没有赋值，则有默认值
        - `int`：`0`
        - `short`：`0`
        - `byte`：`0`
        - `long`：`0L`
        - `float`：`0.0f`
        - `double`：`0.0`
        - `char`：`\u0000`
        - `boolean`：`false`
        - `String`：`null`
    - 数组下标必须在指定范围使用。
    - **数组属于引用类型，数组型数据是对象`object`。**

5. 数组添加/扩容
    ```java
    int[] arr = {1, 2, 3};
    int[] arrNew = new int[arr.length + 1];
    for (int i = 0; i < arr.length; i++) {
        arrNew[i] = arr[i];
    }
    arrNew[arrNew.length - 1] = 4;
    arr = arrNew;
    ```

### 排序（基本，冒泡排序）
详细排序算法待高级篇讲解。
1. 介绍
    - 排序是将多个数据，依指定顺序进行排序的过程。
2. 分类
    - 内部排序
        指将需要处理的所有数据**加载到内部存储器中**，然后进行排序。
        包括：交换式排序、选择式排序、插入式排序。
    - 外部排序
        数据量过大，无法全部加载到内存中，需要**借助外部存储**进行排序。
        包括：合并排序、直接合并排序。
3. 冒泡排序
    ```java
    for (int i = 0; i < length - 1; i++)
        for (int j = 0; j < length - 1 - i; j++)
            // ...
    ```
    - 通过对待排序序列从后向前依次比较相邻元素的值，若发生逆序，则交换。
    - 像水底的气泡向上冒。
    - 冒泡排序的每一轮排序一定能将剩余元素中的最大元素移到序列的末尾。
    ![java_sort_bubble_sort](./img/java_sort_bubble_sort.png)
    ```java
    public class BubbleSort{
        public static void main (String[] args){

            int[] arr1 = {100,34,36,424,456,33,18};

            System.out.println("====初始数组====");
            for (int k = 0; k < arr1.length; k++){
                System.out.print(arr1[k] + "\t");
            }
            System.out.println("");

            for (int i = arr1.length - 1; i > 0; i--){
                int temp = 0;
                for (int j = 1; j < i + 1; j++){
                    if (arr1[j-1] > arr1[j]){           // 更改这里的">"为"<"，实现降序排序
                        temp = arr1[j-1];
                        arr1[j-1] = arr1[j];
                        arr1[j] = temp;
                        temp = 0;
                    }
                }
                System.out.println("====第"+(arr1.length - i)+"轮排序====");
                for (int k = 0; k < arr1.length; k++){
                    System.out.print(arr1[k] + "\t");
                }
                System.out.println("");
                }
        }
    }
    ```
### 查找
详细内容在数据结构与算法，这里只做简要说明。
1. 介绍
    - 常用的查找有两种：顺序查找和二分查找。
2. 顺序查找

### 二维数组
1. 定义
    ```java
    int[][] arr = new int[3][4];
    int arr2[][] = { {1,2,3,4},
                     {5,6,7,8},
                     {9,10,11,12} };
    ```
2. 动态初始化
    - `类型[][] 数组名 = new 类型[大小][大小]`
        ```java
        // 方法1
        int[][] arr = new int[3][4];
        
        // 方法2（分开）
        int[][] arr2;
        arr2 = new int[3][4];
        ```
    - 方法3：一种特殊的动态初始化
        每行的元素数量可以不一样（列数不确定）
        ![java_array_two_dimension_init3](./img/java_array_two_dimension_init3.png)
        ```java
        int[][] arr = { {1}, 
                        {2, 2}, 
                        {3, 3, 3} };

        int[][] arr2 = new int[3][];// 不确定的位置不填值
        for (int i = 0; i < arr2.length; i++){
            arr2[i] = new int[i + 1];// 分配内存
            for (int j = 0; j < arr2[i].length; j++){
                arr2[i][j] = i + 1;     // 赋值
            }
        }
        ```
2. 静态初始化
    ```java
    int[][] arr = { {1, 2, 3},
                    {4, 5, 6}, 
                    {7, 8, 9}};

    int[][] arr2 = { {1}, 
                    {2, 2}, 
                    {3, 3, 3} };

    //一个错误的例子：
    int[][] arr2 = { 1,             // 哪怕只有一个数字，数组也需要括起来 
                    {2, 2}, 
                    {3, 3, 3} };    
    ```
3. 内存布局
    ![java_array_two_dimension](./img/java_array_two_dimension.png)

### 附加内容
#### 【自编，待完善】关于new
在之前的例子中，我们发现，定义一个数组时，可以在声明的同时new，也可以先声明在之后的代码中new，这有什么区别呢？
看一下下面几个例子。
1. 数组初始化（一行完成）
    ```java
    int[] arr1 = new int[5];          // 动态初始化，元素默认值为0
    int[] arr2 = new int[]{1, 2, 3}; // 显式静态初始化
    int[] arr3 = {1, 2, 3};          // 隐式静态初始化（编译器自动补全为 new int[]）

    // 下面是两种错误的初始化
    // 对于显示静态初始化，[] 必须有且不能有值。
    // int[] arr4 = new int[5]{1, 2, 3, 4, 5}; 
    // int[] arr5 = new int{1, 2, 3, 4, 5};
    ```
2. 数组初始化（分开完成）
    ```java
    int[] arr;
    if (condition) {
        arr = new int[10]; // 根据条件动态分配大小
    } else {
        arr = new int[5];
    }
    ```
new起到的是一个分配空间的作用，声明仅是定义的过程，此时赋值会抛出**空指针异常**。

#### 赋值机制
1. 图解
    ![java_array_eq](./img/java_array_eq.png)
2. 解释
    - 对于一个变量，初始化后存储在栈中。
    - 对于一个数组，初始化后地址存储在栈中，数组内容存储在堆中。
        因此，将数组`arr1`整体直接赋值给`arr2`，相当于将数组`arr1`的地址赋值给`arr2`，两个数组的地址相同，对`arr1[i]`进行修改，`arr2[i]`也会同步修改。
3. 数组拷贝
    显然，数组之间直接进行赋值是行不通的，因此我们需要另辟蹊径。
    ```java
    int[] arr1 = {1, 2, 3, 4, 5};
    int[] arr2 = new int[arr1.length];
    for (int i = 0; i < arr1.length; i++) {
        arr2[i] = arr1[i];
    }
    ```

## 面向对象编程基础
### 章节目录
1. 类和对象
2. 成员方法
3. 成员方法传参机制
4. overload
5. 可变参数
6. 作用域
7. 构造器
8. this

### 类和对象
1. 引例
    张老太养了两只猫猫:一只名字叫小白,今年 3 岁,白色。还有一只叫小花,今年 100 岁,花色。请编写一个程序，当用户输入小猫的名字时，就显示该猫的名字，年龄，颜色。如果用户输入的小猫名错误，则显示 张老太没有这只猫猫。
    用之前学过的知识，或许我们可以使用单独定义变量或数组解决，但这不利于变量的管理，现在引出类和对象的概念。
    - 一个对象由属性和行为构成，而类是一种数据类型，用于定义对象实例。
        举个例子，猫类`Cat`是一种自定义的数据类型，而具体的一只猫是一个对象。
    - 三种叫法
        - 创建一个对象
        - 实例化一个对象
        - 把类实例化

    一个面向对象编程（oop, Oobject-oriented programming）实例
    ```java
    public class Object01 {
        Cat cat1 = new Cat();
        cat1.name = "小白";
        cat1.age = 3;
        cat1.color = "白色";

        Cat cat2 = new Cat();
        cat2.name = "小花";
        cat2.age = 100;
        cat2.color = "花色";
    }
    class Cat {
        String name;    // 名字
        int age;        // 年龄
        String color;   // 颜色
    }
    ```
2. 类与对象的区别
    - 类是抽象的、概念的，代表**一类事物**，如：猫类、狗类、人类。即：类是数据类型。
    - 对象是具体的、实体的，代表**某一具体事物**，如：一只猫、一只狗、一个人类。即：对象是实例。
    - 类是对象的模板，对象是类的一个个体，对应一个实例。
3. 对象的内存分布
    ![java_object_memory](./img/java_oopbase_object_memory.png)
    - 基本数据类型存于堆，其他数据存于方法区。
4. 属性/成员变量
    - 成员变量=属性=字段
    - 属性是类的一个组成部分，一般是基本数据类型，也可以是引用类型（对象、数组）。
    - 属性的定义语法和变量相同
        `访问修饰符 属性类型 属性名;`
        访问修饰符：控制属性的访问范围。有`public`、`protected`、默认、`private`四种。
    - 属性的定义类型可以是任意类型（基本数据类型和引用类型）。
    - 属性如果不赋值，默认值与变量相同。

5. 创建对象
    ```java
    // 先声明再创建
    Cat cat1;
    cat1 = new Cat();

    // 直接创建
    Cat cat2 = new Cat();
    ```
6. 访问属性
    ```java
    cat1.name
    cat1.age
    cat1.color
    ```
7. 一个例子
    看一个练习题，并分析画出内存布局图，进行分析
    ```java
    Person person1 = new Person();
    person1.name = "小明";
    person1.age = 18;
    Person person2 = person1;
    System.out.println(person2.name);
    ```
    ![java_object_memory_example1](./img/java_oopbase_object_memory_example1.png)
8. 类和对象的内存分配机制
    - 栈：一般存放基本数据类型（局部变量）。
    - 堆：存放对象（自建类、数组等）。
    - 方法区：常量池（常量，如字符串）；类加载信息。
9. Java创建对象的流程简述
    - 加载类信息（属性和方法），仅在首次创建时加载。
    - 在堆中分配空间进行默认初始化。
    - 把地址赋值给变量。（`new`）
    - 进行指定初始化（`cat.age = 3;`）

### 成员方法
1. 基本介绍
    - 成员方法也称方法。
2. 快速入门
    创建一个名为`Method01.java`的类，并添加如下方法：
    - 添加 `speak` 成员方法,输出 `"我是一个好人"` 
    - 添加 `cal01` 成员方法,可以计算从 `1+..+1000` 的结果
    - 添加 `cal02` 成员方法,该方法可以接收一个数 `n`，计算 `1+..+n` 的结果
    - 添加 `getSum` 成员方法,可以计算两个数的和
    ```java
    public class Method01 {
        public static void main(String[] args) {
            Person person = new Person();
            person.speak();
            System.out.println(person.cal01());
            System.out.println(person.cal02(100));
            System.out.println(person.getSum(10, 20));
        }
    }
    class Person {
        String name;
        int age;

        public void speak() {
            System.out.println("我是一个好人");
        }
        
        public int cal01() {
            int sum = 0;
            for (int i = 1; i <= 1000; i++) {
                sum += i;
            }
            return sum;
        }

        public int cal02(int n) {
            int sum = 0;
            for (int i = 1; i <= n; i++) {
                sum += i;
            }
            return sum;
        }

        public int getSum(int a, int b) {
            return a + b;
        }
    }
    ```
3. 剖析方法
    `public void speak()`
    - `public`：访问修饰符，控制方法的访问范围。这里`public`表示公开。
    - `void`：返回值类型，`void`表示该方法没有返回值。
    - `speak`：方法名，自定义。
    - `()`：形参列表，方法可以接收参数，也可以不接收参数。
4. 图解
    ![java_oopbase_method_img](./img/java_oopbase_method_img.png)

5. 成员方法定义语法
    ```java
    访问修饰符 返回值类型 方法名(参数（形参）列表) {
        语句;
        return 返回值;
    }
    ```
    - 访问修饰符：如果不写默认访问，[有四种: public, protected, 默认, private]。
    - 返回数据类型
        1) 一个方法最多有一个返回值 [思考，如何返回多个结果 返回数组 ]
        2) 返回类型可以为任意类型，包含基本类型或引用类型(数组，对象)
        3) 如果方法要求有返回数据类型，则方法体中最后的执行语句必须为 return 值; 而且要求返回值类型必须和 return 的
        值类型一致或兼容
        4) 如果方法是 void，则方法体中可以没有 return 语句，或者 只写 return（用于终止方法） ;
    - 方法名
        遵循驼峰命名法，最好见名知义，表达出该功能的意思即可, 比如 得到两个数的和 getSum, 开发中按照规范
    - 形参列表
        - 调用带参数的方法时，应对应参数列表传入相同类型或兼容类型的参数。如形参为`double`可传入`double`、`int`等
        - 方法定义时的参数称为形式参数，方法调用时传入的参数为实际参数，两者需要一一对应。

6. 方法调用
    - 同一个类中调用方法，直接调用即可。如：A类中有方法f1和f2。
        ```java
        class A{
            public void f1(){
                f2();       // 无需A.f2()
            }
            public void f2(){
                return;     // 空的return语句
            }
        }
        ```
    - 跨类调用需要通过对象名调用。
        ```java
        class B{
            public void f1(){
                A a = new A();      // 先实例化对象
                a.f1();             // 通过对象调用
            }
        }
        ```
    - 跨类方法调用与访问修饰符相关。

### 成员方法的传参机制
1. 基本数据类型的传参机制
    - 基本数据类型的值存储于栈中，传递的是值，其内容不受方法影响。
    ```java
    public class MethodParameter01 {
    //编写一个 main 方法
        public static void main(String[] args) {
            int a = 10;
            int b = 20;
            //创建 AA 对象 名字 obj
            AA obj = new AA();
            obj.swap(a, b); //调用 swap
            System.out.println("main 方法 a=" + a + " b=" + b);//a=10 b=20
        }
    }

    class AA {
        public void swap(int a,int b){
            System.out.println("\na 和 b 交换前的值\na=" + a + "\tb=" + b);//a=10 b=20
            //完成了 a 和 b 的交换
            int tmp = a;
            a = b;
            b = tmp;
            System.out.println("\na 和 b 交换后的值\na=" + a + "\tb=" + b);//a=20 b=10
        }
    }
    ```
    ![java_method_param_basic](./img/java_method_param_basic.png)
2. 引用数据类型的传参机制
    - 引用类型的地址存于栈，具体值存于堆（数组）或方法区（自建类）
    - 当将引用类型传入方法中时，传递的是地址，而不是值，对相应对象的操作会影响原来的对象。
    ```java
    public class MethodParameter02 {
    //编写一个 main 方法
        public static void main(String[] args) {
            //测试
            B b = new B();
            // int[] arr = {1, 2, 3};
            // b.test100(arr);//调用方法
            // System.out.println(" main 的 arr 数组 ");
            // //遍历数组
            // for(int i = 0; i < arr.length; i++) {
            // System.out.print(arr[i] + "\t");
            // }
            // System.out.println();
            //测试
            Person p = new Person();
            p.name = "jack";
            p.age = 10;
            b.test200(p);
            //测试题, 如果 test200 执行的是 p = null ,下面的结果是 10
            //测试题, 如果 test200 执行的是 p = new Person();..., 下面输出的是 10
            System.out.println("main 的 p.age=" + p.age);//10000
        }
    }

    class Person {
        String name;
        int age;
    }
    class B {
        public void test200(Person p) {
            //p.age = 10000; //修改对象属性
            //思考
            p = new Person();
            p.name = "tom";
            p.age = 99;
            //思考
            //p = null;
        }
        //B 类中编写一个方法 test100，
        //可以接收一个数组，在方法中修改该数组，看看原来的数组是否变化
        public void test100(int[] arr) {
            arr[0] = 200;//修改元素
            //遍历数组
            System.out.println(" test100 的 arr 数组 ");
            for(int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + "\t");
            }
            System.out.println();
        }
    }
    ```
3. 扩展
    在上面的代码中，提到了两种特殊情况：
    - 当方法内部将传入的引用对象赋值为null时，传入参数会发生什么？
        答：什么都不会发生，仅会改变传入的地址值，对原地址指向的内容不会产生任何影响。
        ![java_method_refParam_null](./img/java_method_refParam_null.png)
    - 当方法内部将传入的引用对象new为另一个对象时，传入参数会发生什么？
        答：什么都不会发生，仅会将新申请的空间赋值给方法内的变量，对传入的引用对象本身不会产生任何影响。
        ![java_method_refParam_new](./img/java_method_refParam_new.png)

4. 对象克隆
    ```java
    class Person {
        String name;
        int age;
    }
    class MyTools {
        public Person copyPerson(Person p) {
            //创建一个新的对象
            Person p2 = new Person();
            p2.name = p.name; //把原来对象的名字赋给 p2.name
            p2.age = p.age; //把原来对象的年龄赋给 p2.age
            return p2;
        }
    }
    ```
### 方法递归调用
1. 引例
    ```java
    public class Test {
        public static void main(String[] args) {
            T t = new T();
            t.test(4);
        }
    }
    class T {
        public void test(int n) {
            if (n > 2) {
                test(n - 1);
            }
            System.out.println("n=" + n);
        }
    }
    ```
    ![java_recursion_example](./img/java_recursion_example.png)
2. 递归的规则
    - 执行一个方法时，就创建一个新的受保护的独立空间（栈空间）。
    - 方法的局部变量是独立的，不会相互影响。
    - 如果方法中使用的是引用类型变量，就会共享该引用类型。
    - **递归必须向推出递归的条件逼近，否则会无限递归。**
    - 一个方法执行完毕时，或遇到return，就会返回，遵循**谁调用就将结果返回给谁**的规则，同时当方法执行完毕或返回时，该方法执行完毕。

#### 实例
1. 斐波那契数列
    ```java
    /*
    请使用递归的方式求出斐波那契数 1,1,2,3,5,8,13...给你一个整数 n，求出它的值是多
    思路分析
    1. 当 n = 1 斐波那契数 是 1
    2. 当 n = 2 斐波那契数 是 1
    3. 当 n >= 3 斐波那契数 是前两个数的和
    4. 这里就是一个递归的思路
    */
    int fibonacci(int n){
		if (n>=1){
			if (n == 1 || n == 2){
				return 1;
			}else{
				return fibonacci(n-1) + fibonacci(n-2);
			}
		}else{
			System.out.print("错误");
			return -1;
		}
	}
    ```
2. 猴子吃桃
    ```java
    /*
    猴子吃桃子问题：有一堆桃子，猴子第一天吃了其中的一半，并再多吃了一个！
    以后每天猴子都吃其中的一半，然后再多吃一个。当到第 10 天时，
    想再吃时（即还没吃），发现只有 1 个桃子了。问题：最初共多少个桃子？
    思路分析 逆推
    1. day = 10 时 有 1 个桃子
    2. day = 9 时 有 (day10 + 1) * 2 = 4
    3. day = 8 时 有 (day9 + 1) * 2 = 10
    4. 规律就是 前一天的桃子 = (后一天的桃子 + 1) *2//就是我们的能力
    5. 递归
    */
    public int peach(int day) {
        if(day == 10) {//第 10 天，只有 1 个桃
            return 1;
        } else if ( day >= 1 && day <=9 ) {
            return (peach(day + 1) + 1) * 2;//规则，自己要想
        } else {
            System.out.println("day 在 1-10");
            return -1;
        }
    }
    ```
3. 迷宫问题
   
4. 汉诺塔

5. 八皇后

### 方法重载（overload）
1. 基本介绍
    - **java中允许同一个类中有多个同名方法存在，但要求形参列表不一致。（顺序、类型不同）**
    - 利于接口编程。
### 可变参数
1. 基本介绍
    java允许将同一个类中的多个同名同功能但参数个数不同的方法，封装成一个方法。可以通过可变参数实现。
2. 基本语法
    ```java
    访问修饰符 返回值类型 方法名(参数类型... 变量名) {}
    ```
3. 一个例子
    ```java
    class Method01{
        public int sum(int n1, int n2){
            return n1 + n2;
        }
        public int sum(int n1, int n2, int n3){
            return n1 + n2 + n3;
        }
        //...
    }
    class VarParam {
        // 可变参数的传入个数可以是0个至多个
        public int sum(int... n) {
            int sum = 0;
            for (int i = 0; i < n.length; i++) {
                sum += n[i];
            }
            return sum;
        }
    }
    ```
4. 可变参数细节
    - 可变参数的实参可以为0个或任意多个。
    - 可变参数的实参可以是一个数组。
        **如果传入的是数组，则等价于将数组中的所有元素依次传入方法。**
    - 可变参数的本质就是数组。
    - 可变参数可以和普通类型参数放在一个形参列表中，但可变参数必须放在最后。
    - 一个形参列表中只能出现一个可变参数。

### 作用域
1. 基本介绍
    - 在java编程中，主要的变量就是属性（成员变量）和局部变量。
    - 局部变量一般指定义在成员方法中的变量。
    - 作用域分类
        - 全局变量，即属性，作用域为整个类。
        - 局部变量，即除属性外的其他变量，作用域为定义它的代码块。
    - 全局变量有默认值，可以不赋值直接使用；局部变量没有默认值，必须赋值后使用。
2. 使用细节
    - 属性和局部变量可以重名，访问时遵循就近原则。
    - 在同一个作用域中，两个局部变量不能重名。
    -   属性生命周期较长，随对象创建和销毁；
        局部变量生命周期较短，随代码块创建和销毁。
    - 作用域范围不同
        - 全局变量（属性）可以被本类或其他类使用。
        - 局部变量只能在本类中对应方法中使用。
    - 修饰符不同
        全局变量（属性）可以加修饰符。
        局部变量不可以加修饰符。

### 构造器
1. 基本介绍
    当创建一个对象时，如果需要在new的同时定义其中属性的值，就需要使用构造器。
    构造方法又叫构造器（constructor），是类的一种特殊方法，用于完成对新对象的初始化。
2. 基本语法
    ```java
    [修饰符] 方法名(形参列表){
        方法体;
    }
    ```
    - 构造器的修饰符没有要求。
    - 构造器没有返回值。**（也不能写`void`）**
    - **`方法名`和类名必须相同。**
    - `参数列表`和成员方法规则相同。
    - 构造器的调用由系统完成（new时完成）。 
3. 快速入门
    ```java
    public class Constructor {
        public static void main(String[] args) {
            Person p = new Person(18,"jason");
            System.out.println(p.age);
            System.out.println(p.name);
        }
    }
    
    class Person {
        int age;
        String name;

        public Person(int pAge, String pName) {
            age = pAge;
            name = pName;
        }
    }
4. 细节
    - 一个类可以定义多个构造器（构造器重载）
        ```java
        class Person {
            int age;
            String name;
            public Person(int pAge, String pName) {
                age = pAge;
                name = pName;
            }
            public Person(String pName) {
                name = pName;
                age = 0;
            }
        }
    - **构造器名和类型须相同。**
    - 构造器没有返回值。
    - 构造器是完成对象初始化，不是创建对象。
    - 创建对象时，系统自动调用该类的构造方法。
    - 如果没有定义构造器，系统会自动生成一个默认的构造器。
        这是一个默认无参构造器，可以用`javap`指令反编译查看。
        ```java
        //Dog类的默认构造器举例
        public class Dog {
            public Dog() {}    
        }
        ```
    - **一但定义了自己的构造器，默认构造器就会被覆盖，除非重新显式定义无参构造器。**
5. 【面试题】对象创建流程分析
    ```java
    class Person {
        int age = 90;
        String name;
        Person(String pName, int pAge) {
            name = pName;
            age = pAge;
        }
    }

    Person p = new Person("jason", 18);
    ```
    - 在方法区中加载类信息。
    - 在堆中分配内存空间
    - 完成对象初始化
        - 默认初始化：`age = 0;name = null;`
        - 显式初始化：`age = 90;name = null;`
        - 构造器初始化：`name = "jason";age = 18;`
    - 将对象在堆中的地址返回给p。
    [【p332】断点调试展现类创建过程](https://www.bilibili.com/video/BV1fh411y7R8/?p=333)

### this
1. 引例
    - 在之前的构造器学习中，我们创建过这样的构造器。
        ```java
        public Person(int pAge, String pName){
            age = pAge;
            name = pName;
        }
        ```
    - 显然`pAge`和`pName`并没有什么意义，所以我们想出了一种办法。
        ```java
        public Person(int age, String name){
            this.age = age;
            this.name = name;
        }
        ```

    - 简单地说，哪个对象调用，`this`就代表哪个对象。
2. 图解
    ![java_class_this](./img/java_class_this.png)
3. 使用细节
    - `this`关键字可以用来访问本类的属性、方法、构造器。
    - `this`用于区分当前类的属性和局部变量。
    - 访问成员方法的语法：`this.方法名(参数列表);`
    - **访问构造器语法：`this(参数列表);` 注意只能在构造器中使用(即只能在构造器中访问另外一个构造器, 而且必须放在第一条语句)**
    - this 不能在类定义的外部使用，只能在类定义的方法中使用。
### 附加内容
#### 代码健壮性
1. 引例
    一个返回double数组最大值的方法：
    ```java
    public class Homework01{
        public static void main(String[] args) {
            A01 a01 = new A01();
            double[] arr;
            int length = 20;
            arr = new double[length];
            for (int i = 0; i < length; i++){
                arr[i] = Math.random() * 100;
                System.out.println("arr[" + i + "] = " + arr[i]);
            }
            double max = a01.max(arr);
            if (max == null){
                System.out.println("输入错误");
            }
            else
                System.out.println("max:" + a01.max(arr));
        }
    }

    class A01{
        public Double max(double[] arr){
            if(arr != null && arr.length > 0){
                double res = arr[0];
                for (int i = 1; i < arr.length; i++){
                    res = arr[i] > res ? arr[i] : res;
                }
                return res;
            }
            else
                return null;
        }
    }
    ```
2. 健壮性体现
    - 当传入参数为`{}`或`null`时，方法返回`null`。
    - 采用`double`的包装类`Double`，避免返回`null`产生错误。
    - 在执行代码中加入对`null`的判断，避免空指针异常。

#### 匿名对象
1. 例子
    ```java
    new Test().f1();
    ```
2. 特点
    - 匿名对象，由于没有赋值给任意对象，所以使用一次就销毁

## 面向对象编程中级

### IDE
1. IntelliJ IDEA
    - 业界公认最好的IDE
    - 除java外，还支持HTML、CSS、PHP、MySQL、Python等。
    - [下载链接](https://www.jetbrains.com/idea/download/)
    - [【黑马IDEA快速入门，最精简的IDEA使用教程，从下载IDEA到模块使用】 ](https://www.bilibili.com/video/BV1he4y1s7Yb/?p=3&share_source=copy_web&vd_source=d2703b4814ac43d97585ae499ae4f355)
2. Eclipse
    - 完全免费、开源的IDE

#### 创建一个项目
1. `文件-新建-项目`
2. 基本配置
    - 选择项目类型为`Java`。
    - 选择要使用的JDK。
    - 选择项目位置。
    ![java_IDEA_newProject](./img/java_IDEA_newProject.png)
3. 创建一个类
    - 在左侧文件列表的`src`文件夹上`右键-新建-java类`。
        ![java_IDEA_newProject_newClass](./img/java_IDEA_newProject_newClass.png)
4. 运行代码
    点击代码区的绿色三角或在代码上`右键-运行`可以运行当前代码。
    运行结果在下方终端查看。
    ![java_IDEA_newProject_run](./img/java_IDEA_newProject_run.png)

#### IDEA设置
1. 设置文件列表大小
    - `文件-设置-外观-无障碍功能-使用自定义字体`
        修改字体大小。
        ![java_IDEA_config_listSize](./img/java_IDEA_config_listSize.png)
2. 设置代码区代码大小
    - `文件-设置-编辑器-字体-字体大小`
        ![java_IDEA_config_fontSize](./img/java_IDEA_config_fontSize.png)
3. 字体加粗/倾斜
    - `文件-设置-编辑器-配色方案-常规-文本-默认文本`
        选择加粗或斜体。
        ![java_IDEA_config_fontBold](./img/java_IDEA_config_fontBold.png)
4. 更改背景
    - `文件-设置-编辑器-配色方案`
        更改配色
        ![java_IDEA_config_background](./img/java_IDEA_config_background.png)
5. 更改文件默认编码
    - `文件-设置-编辑器-文件编码`
        更改编码
        ![java_IDEA_config_fileEncoding](./img/java_IDEA_config_fileEncoding.png)
6. 嵌入提示
    - `文件-设置-编辑器-嵌入提示`
        关闭对应选项
        ![java_IDEA_config_inlayHints](./img/java_IDEA_config_inlayHints.png)
        ![java_IDEA_config_inlayHintsExample](./img/java_IDEA_config_inlayHintsExample.png)
         
#### IDEA快捷键
可以通过`设置-按键映射`对快捷键进行查看和更改。
1. 删除当前行
    - 软件默认为`Ctrl+Y`。
    - 可以更改为`Ctrl+D`。
2. 复制当前行
    - 建议配置：`Ctrl+Alt+向下箭头`
3. 补全代码
    - `Alt+/`
4. 添加和取消注释
    - `Ctrl+/`
5. 导入当前行所需的类
    - 用于补全忘记写的import语句。
    - 在`设置-编辑器-常规-自动补全`中进行配置。
        ![java_IDEA_config_autoImport](./img/java_IDEA_config_autoImport.png)
    - 在需要配置导入的行按`Alt+Enter`
6. 快速格式化代码
    - `Ctrl+Alt+L`
7. 快速运行
    - `Shift+F10`
    - 也可以配置为`Alt+R`。（在按键映射中搜索`run`进行配置）
8. 生成构造器等
    - `右键-生成-构造器`
    - `Alt+Insert`
    ![java_IDEA_config_generate](./img/java_IDEA_config_generate.png)
9. 查看类的继承关系
    - `Ctrl+H`
10. 定位方法
    - 将光标放在想要查找的方法上，然后`Ctrl+B`
11. 自动分配变量名
    - 在一行代码后加`.var`然后回车。
        ```java
        new MyTools.var     // 回车

        MyTools myTools = new MyTools();    // 生成结果
        MyTools myTools1 = new MyTools();    // 生成结果
        MyTools myTools2 = new MyTools();    // 生成结果
        // ...
        ```

#### 模板/自定义模板
1. 简介
    - 输入`main`然后tab，会出现一大长串。这就是内置的模板。
        ```java
        main // tab
        
        // 生成的内容
        public static void main(String[] args) {

        }
        ```
    - `设置-编辑器-实时模板`可以查看。
    - 展开java选项可以查看java的模板，点击旁边的+号可以添加自定义的模板（须写明模板的生效范围）。
        ![java_IDEA_config_liveTemplate](./img/java_IDEA_config_liveTemplate.png)
        ![java_IDEA_config_liveTemplateCreate](./img/java_IDEA_config_liveTemplateCreate.png)

#### 待整理的其他IDEA技巧
1. 点击IDE左侧的`structure`可以查看光标处类的详细信息。
2. 查看jdk源码
    - `文件-项目结构-SDK-源文件路径`
        添加`$JDK_PATH$`下的`src.zip`和`javafx-src.zip`。
    - 通过`Ctrl+B`或`右键-goto-声明或用例(Declaration or Usages)`查看源码。
3. IDEA中为程序传入实参
    在右上角图示位置选择编辑配置：
    ![java_IDEA_paramIn1](./img/java_IDEA_paramIn1.png)
    修改程序实参选项，参数间用空格隔开。
    ![java_IDEA_paramIn2](./img/java_IDEA_paramIn2.png)
4. 为每个程序开头添加作者版本信息
    `设置-编辑器-文件和代码模板-includes-FileHeader`中修改，这些内容会自动输入到每个代码文件开头。
    ![java_IDEA_fileHeader](./img/java_IDEA_fileHeader.png)
5. 【社区版没有】在一个类中右键`图表-显示图`可以打开类的结构图，选中任意对象按`Ctrl+Alt+B`即可向下扩展。
    - 左上方按钮可以增加显示的信息
        - `f`：字段，定义的变量
        - `m*`：构造器
        - `m`：方法
        - `p`：显示属性（`setXXX` or `getXXX`）
        - `I`：显示内部类
        
6.  选中区域，按快捷键`Ctrl+Alt+T`可以快速生成代码块（循环、错误捕获等代码块包围方式）
7. `Ctrl+J`可以查看快速生成代码的指令，即显示快捷键的快捷键。（如`itit`生成迭代while循环）
8. 常用的快捷键
    - `I`：生成增强for循环。
9. JUnit快捷键：在`@Test`上按`Alt+Enter`可以快速生成单元测试。
10. IDEA中dubug时可能会简化显示（如`ArrayList`扩容后的null不会显示，此时需要通过`设置-构建、执行、部署-调试器-数据视图-Java`取消勾选`启用集合类的替代视图`恢复显示）
    ![java_IDEA_config_enableAlternativeViewForCollectionClasses](./img/java_IDEA_config_enableAlternativeViewForCollectionClasses.png)
11. `Alt+Insert`在生成中选择生成`equals()和hashCode()`可以进入快速生成向导，选定`equals()`的逻辑。这个方法可以服务于哈希表的哈希值和相等判断逻辑。

### 包
1. 基本介绍：包的三大作用
    - 区分相同名字的类。
    - 当类很多时，可以很好的管理类。
    - 控制访问范围
2. 基本语法
    ```java
    package com.lcq;
    ```
    - `package`：打包关键字。
    - `com.lcq`：包名。
3. 包的本质
    - 实际上就是创建不同的文件夹来保存类文件。
    ![java_package](./img/java_package.png)
4. 快速入门
    - `右键-新建-软件包`创建一个名为`com.xiaoming`的包。
    - 文件结构
        - `./src/com/xiaoming/Dog.java`
        - `./src/com/xiaoqiang/Dog.java`
    - 此时new一个Dog，IDEA会让你选择你想要使用的是哪个Dog。
    - 当同时引用了不同包的Dog类时，可以将路径写全来避免冲突。
        ```java
        import com.xiaoming.Dog;

        public class Test {
            public static void main(String[] args) {
                com.xiaoqiang.Dog dog1 = new com.xiaoqiang.Dog();   // 小强的Dog
                Dog dog2 = new Dog();                               // 小明的Dog
            }
        }
        ```
5. 包的命名规则与规范
    - 包的命名规则：只能包含数组、字母、下划线、小圆点，不能用数字开头，不能是关键字或保留字。
    - 命名规范：一般是小写字母加小圆点。
        例：
        ```java
        // com.公司名.项目名.业务模块名
        com.lcq.oa.model        // lcq公司，OA项目，模型开发
        com.lcq.oa.controller   // lcq公司，OA项目，控制器开发

        com.sina.crm.user       // 新浪公司，CRM项目，用户模块
        com.sina.crm.order      // 新浪公司，CRM项目，订单模块
        com.sina.crm.util       // 新浪公司，CRM项目，工具类
        ```
6. 常用包
    - `java.lang.*`：基本包，默认引入，不需要再引入。
    - `java.util.*`：工具包，系统提供的工具包，工具类。
    - `java.net.*` ：网络包，网络开发。
    - `java.awt.*` ：java界面开发，GUI。

#### 使用细节
1. 导入包
    两种导入方式：
    ```java
    import java.util.*;         // 导入java.util包下的所有类
    import java.util.Scanner;   // 导入java.util包下的Scanner类
    ```
    建议使用什么包，就导入什么包，不要全导入。
2. 使用系统默认类完成数组排序
    ```java
    import java.util.Arrays;
    //...
    int[] arr = {1, 2, 3, 4, 5};
    Arrays.sort(arr);
    for (int i = 0; i < arr.length; i++) {
        System.out.println(arr[i]);
    }
    ```
3. 注意事项和细节
    - `package`用于声明当前类所在的包，需要放在类的最上面，一个类中最多只能有一句`package ...`
    - `import`用于导入java包，需要放在`package`声明的下面，类定义前面，没有数量和顺序要求。


### 访问修饰符
1. 基本介绍
    java提供四种访问控制修饰符号，用于控制方法和属性（成员变量）的访问权限（范围）。
    - 公开级别：用 `public` 修饰,对外公开
    - 受保护级别：用 `protected` 修饰,对子类和同一个包中的类公开
    - 默认级别：没有修饰符号,向同一个包的类公开. 
    - 私有级别：用 `private` 修饰,只有类本身可以访问,不对外公开.
    **务必记住！！！**
    ![java_modifier](./img/java_modifier.png)

2. 注意事项
    - 修饰符可以用来修饰类中的属性、成员方法、类本身。
    - 只有默认和`public`可以修饰类本身，其他修饰符不能修饰类。
    - 子类访问问题学了继承后讲解。
    - 成员方法的访问规则与属性完全一样
    自己添加：
    - 哪怕写在同一个文件中的两个类，也不能互相调用对方的私有权限方法。

### 封装
1. 介绍
    封装（encapsulation）就是把抽象出的数据[属性]和对数据的操作[方法]封装在一起，数据被保护在内部，程序的其他部分只有通过被授权的操作[方法]，才能对数据进行操作。
2. 好处
    - 隐藏实现细节。（调用者不关心）
    - 可以对数据进行验证，保证安全合理。
3. 封装实现的步骤
    - 对属性进行私有化。（不能直接修改属性）
    - 提供一个公共的`set`方法，用于对属性判断与赋值。
        ```java
        public void setXxx(类型 参数名){
            // 数据验证的业务逻辑
            this.xxx = 参数名;
        }
        ```
    - 提供一个公共的`get`方法，用于获取属性值。
        ```java
        public 返回值类型 getXxx(){
            // 权限判断
            return this.xxx;
        }
        ```
4. 快速入门
    实例：设计一个小程序，不能随意查看人的年龄、工资等隐私，对年龄、name进行合理的验证.
    - name为2~6个字符。
    - 年龄为1~120。
    ```java
    public class Person {
        private double salary;
        private int age;
        private String job;
        public String name;

        public void setAge(int age) {
            if (age >= 1 && age <= 120) {
                this.age = age;
            }
            else {
                System.out.println("年龄不合法，默认修改年龄为18");
                this.age = 18;
            }
        }      
    }
    ```
    - `Alt+Insert`选择`getter和setter`可以快捷生成`get`和`set`方法。
5. 将构造器与`setXxx`方法结合
    ```java
    public class Person {
        // ...

        public Person(String name, int age, double salary) {
            setName(name);
            setAge(age);
            setSalary(salary);
        }
    }
    ```

### 继承
1. 介绍
    - 继承可以解决类代码复用性差的问题。
    - 当多个类存在相同的属性（变量）和方法时，可以从这些类中抽象出父类，在父类中定义这些相同的属性和方法。
    - 所有子类无需重新定义这些属性和方法，只需通过`extends`声明继承父类即可。

2. 基本语法
    ```java
    class 子类 extends 父类{
        // ...
    }
    ```
    - 子类自动拥有父类定义的属性和方法。
    - 父类又叫超类/基类。
    - 子类又叫派生类。

3. 快速入门
    - `Student.java`
        ```java
        public class Student {
            public String name;
            public int age;
            private double score;

            public double getScore() {
                return score;
            }

            public void setScore(double score) {
                this.score = score;
            }

            public void showInfo(){
                System.out.println("name:"+name);
                System.out.println("age:"+age);
                System.out.println("score:"+score);
            }
        }
        ```
    - `Pupil.java`
        ```java
        public class Graduate extends Student {
            public void testing(){
                System.out.println("小学生考试中...");
            }
        }
        ```
    - `Graduate.java`
        ```java
        public class Graduate extends Student {
            public void testing(){
                System.out.println("大学生考试中...");
            }
        }
        ```

4. 细节
    - 子类继承了所有的属性和方法，但是私有属性和方法不能在子类中直接访问，要通过公共方法访问。（如：`setXxx()`、`getXxx()`）
        **对于跨包继承，属性需要用`protect`或`public`才能直接访问，`private`和默认无法直接访问。**
    - **子类必须调用父类的构造器，完成父类的初始化。**
        这里隐藏了一个`super()`方法，后面会讲。
        ```java
        public 子类 (){
            super(); // 编译器强制添加
            // ...
        }
        ```
    - 创建子类对象时，不管使用哪个子类构造器，默认情况下总会调用父类的无参构造器。
    - 如果父类没有提供无参构造器，则必须在子类的构造器中使用`super`指定使用父类的哪个构造器完成对父类的初始化工作，否则编译不通过。
    - 如果希望指定去调用父类的某个构造器，则显式的调用一下 : `super(参数列表)`
    - **`super` 在使用时，必须放在构造器第一行（`super` 只能在构造器中使用）**
    - `super()` 和 `this()` 都只能放在构造器第一行，因此这两个方法不能共存在一个构造器
    - java 所有类都是 Object 类的子类, Object 是所有类的基类. 
    - 父类构造器的调用不限于直接父类！将一直往上追溯直到 Object 类(顶级父类)
    - 子类最多只能继承一个父类(指直接继承)，即 java 中是单继承机制。思考：如何让 A 类继承 B 类和 C 类？ 【A 继承 B， B 继承 C】
    - 不能滥用继承，子类和父类之间必须满足 is-a 的逻辑关系

#### 继承的本质分析
当创建子类对象时，内存中发生的变化。
1. 图解
    ![java_extends_theory](./img/java_extends_theory.png)
2. 返回信息的规则
    - 首先看子类是否有该属性
    - 如果子类有这个属性，并且可以访问，则返回信息
    - 如果子类没有这个属性，就看父类有没有这个属性(如果父类有该属性，并且可以访问，就返回信息..)
    - 如果父类没有就按照(3)的规则，继续找上级父类，直到 Object...

### super
1. 基本介绍
    - super代表父类的引用，用于访问父类的属性、方法、构造器。
2. 基本语法
    - 访问父类的属性，但不能访问父类的private属性。
    - 访问父类的方法，但不能访问父类的private方法。
    - 访问父类的构造器：`super(参数列表)`。只能出现一句且只能放在第一句。

3. 细节
    - 调用父类构造器的好处：分工明确，父类属性由父类初始化，子类属性由子类初始化。
    - 当子类中有何父类的成员（属性和方法）重名时，为了访问父类的成员，必须通过super，没有重名，可以直接访问。`super.func()`
    - super的访问不限于直接父类，如果爷爷类和本类中有同名的成员，也可以使用super去访问爷爷类的成员；如果多个基类都拥有同名成员，采用就近原则。
    - **子类必须调用父类的构造器，完成父类的初始化，若构造器中未显式调用`this`或`super`，则默认调用父类的无参构造器。**

4. super和this
    ![java_super_compare_this](./img/java_super_compare_this.png)

### 方法重写/覆盖（overwrite）
1. 基本介绍
    方法覆盖（重写）就是子类有一个方法，和父类的某个方法名称、返回类型、参数一致，就称此方法覆盖了父类的方法。
    **方法重写严格受父类同名方法约束。**
2. 细节
    - 子类的方法的参数、方法名称要和父类的参数、方法名称完全一致。
    - **子类方法的返回类型和父类方法一致，或是父类返回类型的子类。**
        ```java
        // String是Object的子类，构成重写。
        class A{  
            public Object getInfo(){
                return null;
              }
        }
        class B extends A{  
            public String getInfo(){  
                return null;
            }
        }
        ```
        反过来会直接报错（不准篡位）。
    - 子类方法不能缩小父类方法的访问权限。
        public > protected > default > private
        ```java
        class A{  
            public String getInfo(){  
                return null;
            }    
        }
        class B extends A{  
            public String getInfo(){      // 这里只能是public，不能缩小。 
                return null;
            }
        }
        ```
3. 重载与重写的区别
    ![java_overwrite_compare_overload](./img/java_overwrite_compare_overload.png)

### 多态（polymorphic）
1. 引例
    现在编写`Food`类和三个子类：`Bone`、`Fish`、`Rice`。包含名称属性。
    编写`Animal`类和三个子类：`Cat`、`Dog`、`Chicken`。包含名称属性。
    编写`Master`类和`feed`方法给动物吃东西。

    - 上述代码如果用传统方式解决，需要为每一个动物编写一个feed方法。
        这种方式代码复用性不高，且不利于代码维护。
    - 由此引出多态。

2. 基本介绍
    方法或对象具有多种形态，是面向对象的第三大特征，多态是建立在封装和继承基础上的。
    - 方法重载体现多态。
        ```java
        // sum方法构成多态
        class A {
            public int sum(int a, int b){
                return a + b;
            }
            public int sum(int a, int b, int c){
                return a + b + c;
            }
        }
        ```
    - 方法重写体现多态。
        ```java
        class A {
            public void show(){
                System.out.println("A");
            }
        }
        class B extends A {
            // 两种show方法体现多态
            public void show(){
                System.out.println("B");
            }
        }
        ```

3. 快速入门
    ```java
    public class Animal {...}
    public class Dog extends Animal {...}
    public class Cat extends Animal {...}
    public class Food {...}
    public class Bone extends Food {...}
    public class Fish extends Food {...}
    public class Master {
        public void feed(Animal a, Food f) {
            System.out.println("主人喂食" + a.getName() + "吃" + f.getName());
        }
    }
    ```
    - 【自编】这里有一个问题，没有避免猫吃骨头的问题，这个问题可以使用**泛型**解决，会在第15章讲。

#### 对象的多态
1. 重要的几句话
    - 一个对象的编译类型和运行类型可以不一致。
    - 编译类型在定义对象时就确定了，不能改变。
    - 运行类型可以变化。
    - 编译类型看定义时`=`左边的类型，运行类型看`=`右边的类型。
        ```java
        // class Dog extends Animal{}
        
        Animal a = new Dog();   // 父类的引用可以指向子类的对象（狗是动物，动物不是狗）
                                // 编译类型是Animal，运行类型是Dog
                                // 运行类型的改变也是多态
        ```

2. 重要实例
    ```java
    // 编译类型是Animal，运行类型是Dog
    Animal animal = new Dog();
    // 此时运行类型为Dog，故执行Dog.cry()
    animal.cry();
    // 编译类型是Animal，运行类型是Cat
    animal = new Cat();
    // 此时运行类型为Cat，故执行Cat.cry()
    animal.cry();
    ```

#### 多态的细节
1. 多态的前提是：两个对象/类存在继承关系。
2. 多态的向上转型。
    - 父类引用指向子类对象。
    - 语法
        ```java
        父类类型 引用名 = new 子类类型();
        ```
    - 特点：编译类型看左边，运行类型看右边。
        - 可以调用父类中的所有成员（须遵守访问权限）
        - **不能调用子类中的特有成员。**
        - 最终运行效果看子类的具体实现。
3. 多态的向下转型。
    - 语法
        ```java
        子类类型 引用名 = (子类类型) 父类引用名;
        ```
    - 只能强转父类的引用，不能强转父类的对象。
    - 要求父类引用必须指向**当前目标类型**子类对象。
    - 可以调用子类类型中的所有成员。

4. 注意事项
    - 属性没有重写的说法，属性的值看编译类型。
        ```java
        class Base {
            int num = 10;
        }
        class Sub extends Base {
            int num = 20;
        }
        // ...
        Base base = new Sub();
        System.out.println(base.num);   // 10
        ```
        在这个例子中，尽管运行类型是 Sub，编译类型是 Base，属性的初始化跟随编译类型。
        回顾一下创建类的过程，可以发现，加载类信息在new之前。
        >Java创建对象的流程简述
        >1. 加载类信息（属性和方法），仅在首次创建时加载。
        >2. 在堆中分配空间进行默认初始化。
        >3. 把地址赋值给变量。（`new`）
        >4. 进行指定初始化（`cat.age = 3;`）

#### `instanceof`
1. `instanceof`
    - 用于判断对象**运行类型**是否为某类型或其子类。
    - `实例 instanceof 类型`
2. 例子
    ```java
    public class Animal{}
    public class Cat extends Animal{}
    public class Dog extends Animal{}
    public class Food{}
    public class Bone extends Food{}
    // ...
    System.out.println(cat instanceof Animal);  // true
    System.out.println(cat instanceof Cat);     // true
    System.out.println(cat instanceof Dog);     // 不在同一继承树，直接报错

    // 只能验证直系血亲，旁系会报错。
    String str = new String()
    System.out.println(str instanceof Animal);  // 报错
    ```

#### 动态绑定机制（dynamic binding）
1. 引例
    ```java
    package com.hspedu.poly_.dynamic_;
    public class DynamicBinding {
        public static void main(String[] args) {
            //a 的编译类型 A, 运行类型 B
            A a = new B();//向上转型
            System.out.println(a.sum());//?40 -> 30
            System.out.println(a.sum1());//?30-> 20
        }
    }
    class A {//父类
        public int i = 10;
        //动态绑定机制:
        public int sum() {//父类 sum()
            return getI() + 10;//20 + 10
        }
        public int sum1() {//父类 sum1()
            return i + 10;//10 + 10
        }
        public int getI() {//父类 getI
            return i;
        }
    }
    class B extends A {//子类
        public int i = 20;
        // public int sum() {
        // return i + 20;
        // }
        public int getI() {//子类 getI()
            return i;
        }
        // public int sum1() {
        //  return i + 10;
        // }
    }
    ```
    - 当不注释时，输出`40\n30`
        调用B类方法。
    - 当注释B类`sum`和`sum1`时，输出`30\n20`
        `sum()`调用A类方法和B类`getI()`方法，`getI()`就近调用B类的`i=20`。
        `sum1()`就近调用A类的`i=10`。
    - 注释掉B类所有方法，输出`20\n20`。
2. 简介
    - 当调用对象方法的时候，该方法回合对象的内存地址/运行类型绑定。
    - 当调用对象属性时，没有动态绑定机制，哪里声明。哪里使用。

#### 多态数组
1. 简介
    - 数组的定义为父类类型，里面的元素是子类类型。
2. 示例
    ```java
    public class Person{
        private String name;
        private int age;
        // consructor, getter and setter ...
        public String say(){
            return "name = " + getName() + ", age = " + getAge() + " ";
        }
    }

    public class Student extends Person{
        private int score;
        // consructor, getter and setter ...
        public String say(){
            return super.say() + "score = " + getScore() + " ";
        }
        public void study(){
            System.out.println("studying...");
        }
    }
    public class Teacher extends Person{
        private int salary;
        // consructor, getter and setter ...
        public String say(){
            return super.say() + "salary = " + getSalary() + " ";
        }
        public void teach(){
            System.out.println("teaching...");
        }
    }
    // main
    public class Main{
        public static void main(String[] args){
            Person p[] = new Person[5];
            p[0] = new Person("Tom", 20);
            p[1] = new Teacher("Mike", 30, 5000);
            p[2] = new Teacher("Jerry", 25, 4000);
            p[3] = new Student("Tony", 20, 3.8);
            p[4] = new Student("Jim", 22, 3.9);

            for(int i = 0; i < p.length; i++){
                System.out.println(p[i].say());     // 多态可以让不同子类调用其重写的方法。
                if(p[i] instanceof Student){       
                    // instanceof 判断对象运行类型是否为某类型或其子类。
                    ((Student)p[i]).study();
                }
                else if(p[i] instanceof Teacher){
                    ((Teacher)p[i]).teach();
                }
                else if (p[i] instanceof Person){
                    //...
                }
                else{
                    System.out.println("unknown class");
                }
            }
        }
    }
    ```

#### 多态参数
1. 简介
    - 多态参数，即形参为父类类型，实参为子类类型。
2. 实例

### Object类详解
1. Object类
    - `java.lang.Object`
    - 类`Object`时类层次结构的根类，每个类都使用`Object`作为超类，所有对象（包括数组）都实现这个类的方法。
2. 常用方法
    - `clone()`
    - `equals()`
    - `finalize()`
    - `getClass()`
    - `hashCode()`
    - `toString()`

#### `equals()`
1. 【面试题】`==`和`equals()`区别
    - `==`是一个比较运算符
        - `==`既可以判断基本类型，也可以判断引用类型。
        - `==`如果判断基本类型，判断的是值是否相等。
        - `==`如果判断引用类型，判断的是引用地址是否相等。
    - `equals()`是Object类中的一个方法
        - `equals()`只能判断引用类型。

2. `java.lang.String`中`equanls()`方法的源码
    ```java
    public boolean equals(Object anObject) {
        if (this == anObject) {                         //判断对象是否是同一个对象
            return true;
        }
        if (anObject instanceof String) {               //判断对象是否是String类型
            String aString = (String)anObject;          // 向下转型
            if (coder() == aString.coder()) {
                return isLatin1() ? StringLatin1.equals(value, aString.value)
                                  : StringUTF16.equals(value, aString.value);
            }
        }
        return false;                                   // 不是同一类型
    }
    ```
3. `Objects.equals()` 方法：
    ```java 
    public boolean equals(Object anObject){
        return this == anObject;
    }
    ```
    jdk其他的类均重写了此方法，自定义类中，若不重写此方法，则默认与`==`效果相同。

#### `hashCode()` 
1. 介绍
    - `hashCode()` 方法，用于获取对象的哈希码，哈希码是一个整数值，用于表示对象的唯一性。
    - 支持此方法时为了提高哈希表（如`java.util.Hashtable`）的性能。

2. 六个小结（待后面课程详细讲解）
    - 提高具有哈希结构的容器的效率。（`HashSet`、`HashMap`、`Hashtable`等）
    - 两个引用，如果指向的是同一个对象，则哈希值肯定是一样的。
    - 两个引用，如果指向的是不同对象，则哈希值是不同的。
    - 哈希值主要根据地址号生成的（一般是将该对象的内部地址转换成一个整数来实现的，但是对于Java编程语言不需要这种实现技巧），不能完全将哈希值等价于地址。
    - 在集合这个知识点中，`hashCode()`方法可能会重写。

#### `toString()`
1. 介绍
    - 默认返回`全类名+@+十六进制哈希值`
        全类名：包名+类名
        ```java
        public String toString(){
            return getClass().getName() + "@" + Integer.toHexString(hashCode());
        }
        ```
    - `Alt+Insert`可以快速重写`toString()`方法。一般是输出对象属性。
    - 当直接输出对象时，默认会调用`toString()`方法。

#### `finalize()`
1. 介绍
    - 当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。子类可以重写该方法，做一些释放资源的操作。（释放资源：数据库链接、文件流等）
    - 什么时候被回收：当某个对象没有任何引用时，则jvm就认为这个对象是一个垃圾对象，就会使用垃圾回收机制来销毁该对象，在销毁该对象前，会先调用`finalize()`方法。
    - 垃圾回收机制的调用，是由系统来决定，也可以通过`System.gc()`方法主动触发垃圾回收机制。

### 断点调试
1. 场景
    开发过程中，需要查找错误，可以使用断点调试，一步一步看源码执行的过程。
    **注意**：断点调试过程中，是运行状态，是以对象的运行类型来执行的。
2. 介绍
    - 断点调试是指在程序的某一行设置一个断点，调试时，程序运行到这一行就会停住，然后可以一步一步往下调试，调试过程中可以看各个变量的值，出错的话，调试到出错的代码行即显示错误并停下，进行分析从而找到bug。
    - 断点调试是程序员必须掌握的技能。
    - 断点调试也能帮助我们查看Java底层源代码的执行过程，提高程序员Java水平。
3. 断点调试快捷键
    - `F7`：跳入，跳入方法内。
    - `F8`：跳过，逐行执行代码。
    - `Shift+F8`：跳出，跳出方法。
    - `F9`：resume，运行到下一个断点处。
    
4. 使用断点调试工具追踪jdk源码
    - 方法
        - 执行跳入时，改用`Alt+Shift+F7`强制跳入。
        - 在`设置-构建、执行、部署-调试器-步进`选项下取消勾选`java.*`和`javax.*`。
            ![java_debug_forceStepInto](./img/java_debug_forceStepInto.png) 
5. 技巧
    - 可以动态下断点：在debug过程中，在后续代码临时增加断点。
    - 在分支/循环结构中设置断点，可以按`F9`跳到下一个断点查看并分析业务逻辑。


## 【项目】微信零钱通
### 项目开发流程说明
1. 项目需求
    使用Java开发零钱通项目，可以完成收益入账、消费、查看明细、退出系统等功能。
2. 项目界面

3. 项目代码实现
    - 编写文件 `SmallChangeSys.java` 完成基本功能（过程编程）
    - 将过程编程的版本改成OOP版本，体会OOP编程带来的好处。
4. 代码实现改进
    - 用户退出时，给出提示`你确定要退出吗？y/n`。
    - 在收益入账和消费时，判断金额是否合理。
    - 将面向过程代码修改成面向对象方法，编写`SmallChangeSysOOP.java`类并使用`SmallChangeSysApp.java`完成测试。

### 开始项目开发
1. 原则：化繁为简，先死后活。
    - 先完成显示菜单，并可以选择
    - 完成零钱通明细. 
    - 完成收益入账
    - 消费
    - 退出

### 过程编程
#### 基本业务逻辑
1. 写出基本框架
    ```java
    package com.lcq.smallchange;

    import java.util.Scanner;

    public class SmallChangeSys {
        public static void main(String[] args) {
            
            boolean loop = true;                        // 程序持续运行的标记
            Scanner scanner = new Scanner(System.in);   // 创建Scanner对象获取输入
            String key = "";                            // 用于记录输入选项
            
            // 用do-while循环完成菜单的业务逻辑
            do{
                // 基本的显式菜单和选项功能
                System.out.println("\n============零钱通菜单============");
                System.out.println("\t\t\t1 零钱通明细");
                System.out.println("\t\t\t2 收益入账");
                System.out.println("\t\t\t3 消费");
                System.out.println("\t\t\t4 退出");

                System.out.print("请选择(1~4)：");
                key = scanner.next();


                switch (key) {      // 业务逻辑暂时以占位符呈现
                    case "1":
                        System.out.println("\t\t\t1 零钱通明细");
                        break;
                    case "2":
                        System.out.println("\t\t\t2 收益入账");
                        break;
                    case "3":
                        System.out.println("\t\t\t3 消费");
                        break;
                    case "4":
                        System.out.println("\t\t\t4 退出");
                        loop = false;
                        break;
                    default:
                        System.out.println("输入有误，请重新输入！");
                }
            }while(loop);
        }
    }
    ```
2. 完成零钱通明细
    - 三种方案
        - 保存到数组（但动态数组扩展比较麻烦）
        - 保存为对象。
        - 用字符串拼接（最简单，本例使用的方法）
    ```java
    // 创建用于拼接的字符串
    String details = "============零钱通明细============";
    //...case
    case "1": // 输出拼接的字符串
        System.out.println(details);break;
    ```
3. 完成收益入账
    - 完成收益入账简单的字符串拼接
    - 未完成对输入金额校验的业务逻辑
    ```java
    import java.text.SimpleDateFormat;
    import java.util.Date;

    //...main
        double money = 0;           // 用于记录变化的金额
        double balance = 0;         // 用于记录余额
        Date date = null;           // 创建日期对象
        // 设置日期格式
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

        //...case
        case "2": 
            System.out.print("收益入账金额：");
            money = scanner.nextDouble();
            // check...
            balance += money;
            // details...
            date = new Date();
            System.out.println(sdf.format(date));
            details += "\n收益入账\t+" + money + "\t" + sdf.format(date) + "\t" + balance;
            break;
    ```

4. 完成消费
    ```java
    String note = "";       // 用于记录消费原因

    //...case
    case "3":
        System.out.print("消费金额：");
        money = scanner.nextDouble();
        System.out.print("消费说明：");
        note = scanner.next();

        //check...
        balance -= money;

        date = new Date();
        System.out.println(sdf.format(date));

        details += "\n" + note + "\t-" + money + "\t" + sdf.format(date) + "\t" + balance;
        break;
    ```

5. 完成退出
    ```java
    case "4":
        System.out.println("============零钱通退出============");
        loop = false;
        break;
    ```

#### 业务逻辑改进
1. 退出
    用户退出时，给出提示`你确定要退出吗？y/n`。
    ```java
    case "4":
        //用户输入4退出时，给出提示"你确定要退出吗? y/n"，必须输入正确的y/n ，
        // 否则循环输入指令，直到输入y 或者 n
        // 老韩思路分析
        // (1) 定义一个变量 choice, 接收用户的输入
        // (2) 使用 while + break, 来处理接收到的输入时 y 或者 n
        // (3) 退出while后，再判断choice是y还是n ,就可以决定是否退出
        // (4) 建议一段代码，完成一个小功能，尽量不要混在一起
        String choice = "";
        while (true) { //要求用户必须输入y/n ,否则就一直循环
            System.out.println("你确定要退出吗? y/n");
            choice = scanner.next();
            if ("y".equals(choice) || "n".equals(choice)) {
                break;
            }
            //第二个方案
    //        if("y".equals(choice)) {
    //            loop = false;
    //            break;
    //        } else if ("n".equals(choice)) {
    //            break;
    //        }
        }

        //当用户退出while ,进行判断
        if (choice.equals("y")) {
            loop = false;
        }
        break;
    ```
    【编程思想】在这段代码中，有两种解决方案：
    - 将所有的逻辑全写在一个if逻辑里面。
    - 将判断输入是否合法和输入值的代码分开。
    
    这里建议采用后者，原因如下：
    - 现在有的选，是因为只有两个选项，代码修改起来比较灵活，当遇到大项目时，混起来的业务逻辑不便于修改。
    - 将每个小功能分开，可以方便阅读和修改，也便于增删功能。
2. 零钱通金额校验
    找出不正确的金额条件，然后给出提示，直接break。
    ```java
    case "2":
        System.out.print("收益入账金额:");
        money = scanner.nextDouble();
        //money 的值范围应该校验 -》 一会在完善
        //老师思路, 编程思想
        //找出不正确的金额条件，然后给出提示, 就直接break
        if(money <= 0) {
            System.out.println("收益入账金额 需要 大于 0");
            break;
        }
        //找出正确金额的条件
        balance += money;
        //拼接收益入账信息到 details
        date = new Date(); //获取当前日期
        details += "\n收益入账\t+" + money + "\t" + sdf.format(date) + "\t" + balance;
        break;
    case "3":
        System.out.print("消费金额:");
        money = scanner.nextDouble();
        //money 的值范围应该校验 -》 一会在完善
        //找出金额不正确的情况
        //过关斩将 校验方式.
        if(money <= 0 || money > balance) {
            System.out.println("你的消费金额 应该在 0-" + balance);
            break;
        }
        System.out.print("消费说明:");
        note = scanner.next();
        balance -= money;
        //拼接消费信息到 details
        date = new Date(); //获取当前日期
        details += "\n" + note + "\t-" + money + "\t" + sdf.format(date) + "\t" + balance;
        break;
    ```
    【编程思想】过关斩将:
    找出不正确的条件退出程序，而不是写出正确的条件，原因如下：
    - 这里可以灵活选择，是因为终止条件少。
    - 如果要写出正确的条件，就会进入无休止的嵌套，判断满足所有条件才可以启动程序。且由于判断正确后，需要将整个业务逻辑塞进正确分支的代码块，再写else或其他逻辑，不利于阅读。
    - 判断错误条件，可以在业务逻辑开始时就将错误条件写在一起，便于修改和阅读。

#### 本阶段完整代码
1. `SmallChangeSys.java`
    ```java
    package com.lcq.smallchange;

    import java.text.SimpleDateFormat;
    import java.util.Date;
    import java.util.Scanner;

    public class SmallChangeSys {
        public static void main(String[] args) {
            boolean loop = true;
            Scanner scanner = new Scanner(System.in);
            String key = "";

            String details = "============零钱通明细============";

            double money = 0;
            double balance = 0;
            Date date = null;
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

            String note = "";


            do{
                System.out.println("\n============零钱通菜单============");
                System.out.println("\t\t\t1 零钱通明细");
                System.out.println("\t\t\t2 收益入账");
                System.out.println("\t\t\t3 消费");
                System.out.println("\t\t\t4 退出");

                System.out.print("请选择(1~4)：");
                key = scanner.next();

                switch (key) {
                    case "1":
                        System.out.println(details);break;
                    case "2":
                        System.out.print("收益入账金额：");
                        money = scanner.nextDouble();
                        // check...
                        if (money < 0) {
                            System.out.println("金额不合法");
                            break;
                        }
                        balance += money;
                        // details...
                        date = new Date();
                        System.out.println(sdf.format(date));
                        details += "\n收益入账\t+" + money + "\t" + sdf.format(date) + "\t" + balance;
                        break;
                    case "3":
                        System.out.print("消费金额：");
                        money = scanner.nextDouble();
                        System.out.print("消费说明：");
                        note = scanner.next();

                        //check...
                        if (money < 0 || money > balance) {
                            System.out.println("金额不合法");
                            break;
                        }
                        balance -= money;

                        date = new Date();
                        System.out.println(sdf.format(date));

                        details += "\n" + note + "\t-" + money + "\t" + sdf.format(date) + "\t" + balance;
                        break;
                    case "4":
                        String choice = "";
                        while (true) {
                            System.out.println("确定要退出吗？(y/n)：");
                            choice = scanner.next();
                            if (choice.equals("y") || choice.equals("n")) {
                                break;
                            }
                        }

                        if (choice.equals("y")) {
                            System.out.println("============零钱通退出============");
                            loop = false;
                        }
                        break;
                    default:
                        System.out.println("输入有误，请重新输入！");
                }
            }while(loop);
        }
    }
    ```

### 面向对象编程

1. 修改
    - 将所有和`switch-case`相关的break改成return。
    - 将每个功能分开写。
2. 优点
    方便维护
3. 程序入口：`SmallChangeSysApp.java`
    ```java
    package com.lcq.smallchange.oop;

    public class SmallChangeSysApp {
        public static void main(String[] args) {
            SmallChangeSysOOP smallChangeSysOOP = new SmallChangeSysOOP();
            smallChangeSysOOP.mainMenu();
        }
    }
    ```
4. 功能实现：`SmallChangeSysOOP.java`
    ```java
    package com.lcq.smallchange.oop;

    import java.text.SimpleDateFormat;
    import java.util.Date;
    import java.util.Scanner;

    public class SmallChangeSysOOP {
        boolean loop = true;
        Scanner scanner = new Scanner(System.in);
        String key = "";

        String details = "============零钱通明细============";

        double money = 0;
        double balance = 0;
        Date date = null;
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

        String note = "";

        public void mainMenu(){
            do{
                System.out.println("\n============零钱通菜单OOP============");
                System.out.println("\t\t\t1 零钱通明细");
                System.out.println("\t\t\t2 收益入账");
                System.out.println("\t\t\t3 消费");
                System.out.println("\t\t\t4 退出");

                System.out.print("请选择(1~4)：");
                key = scanner.next();

                switch (key) {
                    case "1":
                        this.detail();
                        break;
                    case "2":
                        this.income();
                        break;
                    case "3":
                        this.pay();
                        break;
                    case "4":
                        this.exit();
                        break;
                    default:
                        System.out.println("输入有误，请重新输入！");
                }

            }while(loop);
        }
        public void detail(){
            System.out.println(details);
            return;
        }
        public void income(){
            System.out.print("收益入账金额：");
            money = scanner.nextDouble();
            // check...
            if (money < 0) {
                System.out.println("金额不合法");
                return;
            }
            balance += money;
            // details...
            date = new Date();
            System.out.println(sdf.format(date));
            details += "\n收益入账\t+" + money + "\t" + sdf.format(date) + "\t" + balance;
        }
        public void pay(){
            System.out.print("消费金额：");
            money = scanner.nextDouble();
            System.out.print("消费说明：");
            note = scanner.next();

            //check...
            if (money < 0 || money > balance) {
                System.out.println("金额不合法");
                return;
            }
            balance -= money;

            date = new Date();
            System.out.println(sdf.format(date));

            details += "\n" + note + "\t-" + money + "\t" + sdf.format(date) + "\t" + balance;
        }
        public void exit(){
            String choice = "";
            while (true) {
                System.out.println("确定要退出吗？(y/n)：");
                choice = scanner.next();
                if (choice.equals("y") || choice.equals("n")) {
                    break;
                }
            }

            if (choice.equals("y")) {
                System.out.println("============零钱通退出============");
                loop = false;
            }
        }
    }

    ```

## 【项目】房屋出租系统
### 项目需求说明
1. 基本需求
    实现基于文本界面的《房屋出租软件》，实现对房屋信息的修改和删除（由于没有学习数据库，这里用数组实现），并能够打印房屋明细表。
2. 项目图片预览
    - 项目界面-主菜单
        ![java_project_houseRentSystem_mainMenu](./img/java_project_houseRentSystem_mainMenu.png)
    - 项目界面-新增房源
        ![java_project_houseRentSystem_addHouse](./img/java_project_houseRentSystem_addHouse.png)
    - 项目界面-查找房源
        ![java_project_houseRentSystem_findHouse](./img/java_project_houseRentSystem_findHouse.png)
    - 项目界面-删除房源
        ![java_project_houseRentSystem_deleteHouse](./img/java_project_houseRentSystem_deleteHouse.png)
    - 项目界面-修改房源
        ![java_project_houseRentSystem_updateHouse](./img/java_project_houseRentSystem_updateHouse.png)
    - 项目界面-房屋列表
        ![java_project_houseRentSystem_houseList](./img/java_project_houseRentSystem_houseList.png)
    - 项目界面-退出系统
        ![java_project_houseRentSystem_exit](./img/java_project_houseRentSystem_exit.png)

### 项目设计

1. 程序框架图（分层模式）
    还有Model2、MVC等设计模式，会在后面接触。
    ![java_project_houseRentSystem_frame](./img/java_project_houseRentSystem_frame.png)
2. 解析
    - 分三个层：界面层、业务层、数据层（又称模型model层、domain层）

### 开始项目开发
1. 创建项目结构
    这个项目虽然简单，但仍需遵守标准化的流程将代码分类存放，因为在后面的更大型项目中，可能还会采用分层结构设计，这时每个包下就会有更复杂的代码。
    - `com.lcq.houserentsys.view`：用于存放界面层代码。
    - `com.lcq.houserentsys.service`：用于存放业务层代码。
    - `com.lcq.houserentsys.domain`：用于存放数据层代码。
    - `com.lcq.houserentsys.utils`：用于存放工具类代码。
    - `HouseRentApp.java`：程序主入口

2. 完成数据层编写
    - 编写`House.java`用于存放房屋信息。
3. 完成功能-主菜单显示
    - 在`HouseView.java`中编写`mainMenu()`方法用于显示主菜单。
    - 在`HouseRentApp.java`中调用`HouseView.mainMenu()`方法用于启动程序。
3. 完成功能-房屋列表显示
    - 在`HouseService.java`中编写`list()`方法返回房屋信息。
    - 在`HouseService.java`中添加数组`House[]`。
    - 在`HouseView.java`中编写`listHouses()`方法用于显示列表界面和调用业务层。
    - 调用方法前，记得进行类初始化，否则无法调用非静态方法。
4. 完成功能-添加房源
    - 在`HouseService.java`中编写`add()`方法添加房屋信息。
    - 在`HouseView.java`中编写`addHouse()`方法用于显示添加房源界面、数据输入和传入业务层。返回boolean用于判断信息是否成功添加。
    - 在`HouseService.java`中添加变量：
        - `idCounter = 1`：用于生成唯一ID。
        - `houseNum`：用于记录房源信息数量，判断数组空间是否足够。
5. 完成功能-删除房屋信息
    - 在`HouseService.java`中编写`del()`方法删除房屋信息。
    - 在`HouseView.java`中编写`delHouse()`方法用于显示删除房源界面、数据输入和传入业务层。返回boolean用于判断信息是否成功删除。

### 项目源码
1. `House.java`
    ```java
    package com.lcq.houserentsys.domain;

    /**
     * House对象表示一个房屋信息
     */
    public class House {
        private int houseId;
        private String houseHost;
        private String phone;
        private String address;
        private int rent;
        private String state;

        public House(int houseId, String houseHost, String phone, String address, int rent, String state) {
            this.houseId = houseId;
            this.houseHost = houseHost;
            this.phone = phone;
            this.address = address;
            this.rent = rent;
            this.state = state;
        }

        public int getHouseId() {
            return houseId;
        }

        public void setHouseId(int houseId) {
            this.houseId = houseId;
        }

        public String getHouseHost() {
            return houseHost;
        }

        public void setHouseHost(String houseHost) {
            this.houseHost = houseHost;
        }

        public String getPhone() {
            return phone;
        }

        public void setPhone(String phone) {
            this.phone = phone;
        }

        public String getAddress() {
            return address;
        }

        public void setAddress(String address) {
            this.address = address;
        }

        public int getRent() {
            return rent;
        }

        public void setRent(int rent) {
            this.rent = rent;
        }

        public String getState() {
            return state;
        }

        public void setState(String state) {
            this.state = state;
        }

        @Override
        public String toString() {
            return houseId +
                    "\t" +houseHost +
                    "\t" +phone +
                    "\t" +address +
                    "\t" +rent +
                    "\t" +state;
        }
    }

    ```
2. `HouseView.java`
    ```java
    package com.lcq.houserentsys.view;

    import com.lcq.houserentsys.domain.House;
    import com.lcq.houserentsys.service.HouseService;
    import com.lcq.houserentsys.utils.Utility;

    /**
     * 1. 显示界面
     * 2. 接收用户输入
     * 3. 调用HouseService完成对房屋信息的各种操作
     */
    public class HouseView {

        private boolean loop = true;
        private char key = ' ';
        private HouseService houseService = new HouseService(10);

        public void mainMenu(){
            do {
                System.out.println("\n\n===========房屋出租系统===========");
                System.out.println("\t\t\t1 新 增 房 源");
                System.out.println("\t\t\t2 查 找 房 屋");
                System.out.println("\t\t\t3 删 除 房 屋");
                System.out.println("\t\t\t4 修 改 房 屋 信 息");
                System.out.println("\t\t\t5 房 屋 列 表");
                System.out.println("\t\t\t6 退       出");
                System.out.print("请输入选项（1~6）：");
                key = Utility.readChar();
                switch (key){
                    case '1':
                        addHouse();
                        break;
                    case '2':
                        findHouse();
                        break;
                    case '3':
                        delHouse();
                        break;
                    case '4':
                        update();
                        break;
                    case '5':
                        listHouses();
                        break;
                    case '6':
                        exit();
                        break;
                }
            }while(loop);
        }

        public void listHouses(){
            System.out.println("===========房屋信息列表===========");

            House[] houses = houseService.list();
            System.out.println("编号\t房主\t\t电话\t\t地址\t\t月租金\t状态");
            for(int i = 0; i<houses.length; i++){
                if (houses[i] != null)
                    System.out.println(houses[i]);
            }
            System.out.println("===========房屋信息完毕===========");
        }

        public void addHouse(){
            System.out.println("===========添加房屋信息===========");
            System.out.print("姓名：");
            String name = Utility.readString(8);
            System.out.print("电话：");
            String phone = Utility.readString(12);
            System.out.print("地址：");
            String address = Utility.readString(16);
            System.out.print("月租：");
            int rent = Utility.readInt();
            System.out.print("状态：");
            String state = Utility.readString(8);
            // ID 是系统自动分配，用户不能输入。
            House house = new House(0, name, phone, address, rent, state);
            //...传进业务层
            if(houseService.add(house)){
                System.out.println("添加房屋成功");
            }else {
                System.out.println("添加房屋失败");
            }
            System.out.println("==========房屋信息添加完成=========");
        }

        public void delHouse(){
            System.out.println("===========删除房屋信息===========");
            System.out.print("输入想删除的房源ID（-1退出）：");
            int delid = Utility.readInt();
            if (delid == -1){
                System.out.println("放弃了删除房屋信息");
                return;
            }

            char choice = Utility.readConfirmSelection();

            if (choice == 'Y') {
                if (houseService.del(delid)) {
                    System.out.println("删除成功");
                }else{
                    System.out.println("删除失败");
                }
            }else if (choice == 'N'){
                System.out.println("放弃了删除房屋信息");
            }
        }

        public void findHouse(){
            System.out.println("===========查找房屋信息===========");
            System.out.print("输入要查找的房源ID（输入-1退出）：");
            int id = Utility.readInt();
            if (id == -1){
                return;
            }
            House house = houseService.findById(id);
            if (house != null) {
                System.out.println(house);
            }else{
                System.out.println("未查找到ID为 "+ id +" 的房源");
            }
        }

        public void update(){
            System.out.println("===========修改房屋信息===========");

            System.out.println("输入要修改的房源ID（输入-1退出）：");
            int id = Utility.readInt();
            if (id == -1){
                return;
            }
            House house = houseService.findById(id);
            if (house == null) {
                System.out.println("ID不存在！");
                return;
            }

            System.out.print("房主（"+house.getHouseHost()+"）：");
            String houseHost = Utility.readString(8,"");
            if (!("".equals(houseHost))){
                house.setHouseHost(houseHost);
            }

            System.out.print("电话（"+house.getPhone()+"）：");
            String phone = Utility.readString(12,"");
            if (!("".equals(phone))){
                house.setPhone(phone);
            }

            System.out.print("地址（"+house.getAddress()+"）：");
            String address = Utility.readString(16,"");
            if (!("".equals(address))){
                house.setAddress(address);
            }

            System.out.print("月租（"+house.getRent()+"）：");
            int rent = Utility.readInt(-1);
            if (rent != -1){
                house.setRent(rent);
            }

            System.out.print("状态（"+house.getState()+"）：");
            String state = Utility.readString(8,"");
            if (!("".equals(state))){
                house.setState(state);
            }
        }

        public void exit(){
            char choice = Utility.readConfirmSelection();
            if (choice == 'Y') {
                loop = false;
            }
        }
    }
    ```
3. `HouseService.java`
    ```java
    package com.lcq.houserentsys.service;

    import com.lcq.houserentsys.domain.House;

    public class HouseService {

        private House[] houses;
        private int houseNum;       // 记录房屋信息数量
        private int idCounter = 1;

        public HouseService(int size) {
            houses = new House[size];
            //配合测试的初始化House对象
            houses[0] = new House(idCounter, "jason", "123321", "海淀区", 2000, "已出租");
            houseNum++;

        }
        public House[] list() {
            return houses;
        }
        public boolean add(House newhouse) {
            // 判断能否继续添加（暂不考虑数组扩容）
            if (houseNum == houses.length) {
                System.out.println("数组已满，不可继续添加");
                return false;
            }

            // 存放信息
            houses[houseNum++] = newhouse;

            // 设置房屋ID
            newhouse.setHouseId(++idCounter);

            return true;
        }

        public boolean del(int delId){
            int index = -1;
            for (int i = 0; i < houseNum; i++) {
                if (houses[i].getHouseId() == delId) {
                    index = i;
                    break;
                }
            }
            if (index == -1) {
                return false;
            }

            for (int i = index; i < houseNum - 1; i++) {
                houses[i] = houses[i + 1];
            }
            houses[--houseNum] = null;

            return true;
        }

        public House findById(int id) {
            for (int i = 0; i < houseNum; i++) {
                if (houses[i].getHouseId() == id) {
                    return houses[i];
                }
            }
            return null;
        }
    }

    ```
4. `HouseRentSysApp.java`
    ```java
    package com.lcq.houserentsys;

    import com.lcq.houserentsys.view.HouseView;

    public class HouseRentApp {
        public static void main(String[] args) {
            // 创建HouseView对象并显示主菜单
            new HouseView().mainMenu();
        }
    }

    ```

5. `Utility.java`
    ```java
    package com.lcq.houserentsys.utils;


    /**
        工具类的作用:
        处理各种情况的用户输入，并且能够按照程序员的需求，得到用户的控制台输入。
    */

    import java.util.*;
    /**

        
    */
    public class Utility {
        //静态属性。。。
        private static Scanner scanner = new Scanner(System.in);

        
        /**
         * 功能：读取键盘输入的一个菜单选项，值：1——5的范围
         * @return 1——5
         */
        public static char readMenuSelection() {
            char c;
            for (; ; ) {
                String str = readKeyBoard(1, false);//包含一个字符的字符串
                c = str.charAt(0);//将字符串转换成字符char类型
                if (c != '1' && c != '2' && 
                    c != '3' && c != '4' && c != '5') {
                    System.out.print("选择错误，请重新输入：");
                } else break;
            }
            return c;
        }

        /**
         * 功能：读取键盘输入的一个字符
         * @return 一个字符
         */
        public static char readChar() {
            String str = readKeyBoard(1, false);//就是一个字符
            return str.charAt(0);
        }
        /**
         * 功能：读取键盘输入的一个字符，如果直接按回车，则返回指定的默认值；否则返回输入的那个字符
         * @param defaultValue 指定的默认值
         * @return 默认值或输入的字符
         */
        
        public static char readChar(char defaultValue) {
            String str = readKeyBoard(1, true);//要么是空字符串，要么是一个字符
            return (str.length() == 0) ? defaultValue : str.charAt(0);
        }
        
        /**
         * 功能：读取键盘输入的整型，长度小于2位
         * @return 整数
         */
        public static int readInt() {
            int n;
            for (; ; ) {
                String str = readKeyBoard(10, false);//一个整数，长度<=10位
                try {
                    n = Integer.parseInt(str);//将字符串转换成整数
                    break;
                } catch (NumberFormatException e) {
                    System.out.print("数字输入错误，请重新输入：");
                }
            }
            return n;
        }
        /**
         * 功能：读取键盘输入的 整数或默认值，如果直接回车，则返回默认值，否则返回输入的整数
         * @param defaultValue 指定的默认值
         * @return 整数或默认值
         */
        public static int readInt(int defaultValue) {
            int n;
            for (; ; ) {
                String str = readKeyBoard(10, true);
                if (str.equals("")) {
                    return defaultValue;
                }
                
                //异常处理...
                try {
                    n = Integer.parseInt(str);
                    break;
                } catch (NumberFormatException e) {
                    System.out.print("数字输入错误，请重新输入：");
                }
            }
            return n;
        }

        /**
         * 功能：读取键盘输入的指定长度的字符串
         * @param limit 限制的长度
         * @return 指定长度的字符串
         */

        public static String readString(int limit) {
            return readKeyBoard(limit, false);
        }

        /**
         * 功能：读取键盘输入的指定长度的字符串或默认值，如果直接回车，返回默认值，否则返回字符串
         * @param limit 限制的长度
         * @param defaultValue 指定的默认值
         * @return 指定长度的字符串
         */
        
        public static String readString(int limit, String defaultValue) {
            String str = readKeyBoard(limit, true);
            return str.equals("")? defaultValue : str;
        }


        /**
         * 功能：读取键盘输入的确认选项，Y或N
         * 将小的功能，封装到一个方法中.
         * @return Y或N
         */
        public static char readConfirmSelection() {
            System.out.println("请输入你的选择(Y/N): 请小心选择");
            char c;
            for (; ; ) {//无限循环
                //在这里，将接受到字符，转成了大写字母
                //y => Y n=>N
                String str = readKeyBoard(1, false).toUpperCase();
                c = str.charAt(0);
                if (c == 'Y' || c == 'N') {
                    break;
                } else {
                    System.out.print("选择错误，请重新输入：");
                }
            }
            return c;
        }

        /**
         * 功能： 读取一个字符串
         * @param limit 读取的长度
         * @param blankReturn 如果为true ,表示 可以读空字符串。 
         * 					  如果为false表示 不能读空字符串。
         * 			
         *	如果输入为空，或者输入大于limit的长度，就会提示重新输入。
        * @return
        */
        private static String readKeyBoard(int limit, boolean blankReturn) {
            
            //定义了字符串
            String line = "";

            //scanner.hasNextLine() 判断有没有下一行
            while (scanner.hasNextLine()) {
                line = scanner.nextLine();//读取这一行
            
                //如果line.length=0, 即用户没有输入任何内容，直接回车
                if (line.length() == 0) {
                    if (blankReturn) return line;//如果blankReturn=true,可以返回空串
                    else continue; //如果blankReturn=false,不接受空串，必须输入内容
                }

                //如果用户输入的内容大于了 limit，就提示重写输入  
                //如果用户如的内容 >0 <= limit ,我就接受
                if (line.length() < 1 || line.length() > limit) {
                    System.out.print("输入长度（不能大于" + limit + "）错误，请重新输入：");
                    continue;
                }
                break;
            }

            return line;
        }
    }

    ```

## 面向对象编程高级
### 章节目录
1. 类变量和类方法
2. 理解main方法、语法static
3. 代码块
4. 单例设计模式
5. final 关键字
6. 抽象类
7. 接口
8. 内部类

### 类变量和类方法（static）
#### 类变量
1. 引例
    有一群小孩，现在要统计做游戏的小孩人数，给出解决的方案。
    - 传统方案：定义一个int变量count，记录游戏的人数，每次调用方法时，将变量加1，并返回结果。
    - 类变量：定义类变量，令所有Child对象共用一个计数器。
2. 类变量（静态变量）简介
    在类内作如下定义：
    ```java
    public static int count = 0;
    ```
    此时对Child.count的操作会反映到所有Child对象上。

3. 类变量的内存布局
    - 图解
        ![java_staticVar_memory](./img/java_staticVar_memory.png)
    - JDK7及以前，静态变量存储在方法区，JDK8及以后，静态变量存储在堆中。
    - 静态变量的存放涉及反射、class原型对象等知识，后面会讲。
    - 类变量所有该类对象是共享的。
    - 静态变量在类加载时就确定了，无论静态变量如何存储，都不会改变其使用细节。
    - 参考文章
        - [【CSDN】Java static变量保存在哪？](https://blog.csdn.net/x_iya/article/details/81260154/)
        - [【知乎】java中的静态变量和Class对象究竟存放在哪个区域？](https://www.zhihu.com/question/59174759/answer/163207831)
4. 定义访问
    - 定义语法
        ```java
        访问修饰符 static 数据类型 变量名; // 推荐
        static 访问修饰符 数据类型 变量名;
        ```
    - 访问类变量
        ```java
        类名.类变量名;      //类变量随类的加载创建，即使没有创建实例也可以访问
        对象名.类变量名;
        ```
5. 类变量注意事项和细节讨论
    - 什么时候需要用类变量？
        当我们需要让某个类的所有对象都共享一个变量时，就可以考虑使用类变量（静态变量）。
        例：定义学生类，统计所有学生共交多少钱。
    - 类变量与实例变量（普通属性）的区别
        类变量是该类所有对象共享的，而实例变量是每个对象独享的。
    - 加上`static`称为类变量（静态变量），否则称为实例变量（普通变量、非静态变量）。
    - 推荐使用`类名.变量名`的形式访问类变量。访问类变量须满足访问修饰符的访问权限和范围。
    - 实例变量不能通过`类名.变量名`的形式访问。
    - 类变量在类加载时就完成了初始化，也就是说，即使没有创建对象，只要类加载了，就可以使用类变量。
    - 类变量的生命周期是随类的加载开始，随着类的消亡而销毁。

#### 类方法
1. 介绍
    - 类方法也称静态方法
2. 类方法的定义
    ```java
    访问修饰符 static 返回值类型 方法名(参数列表){};    // 推荐
    static 访问修饰符 返回值类型 方法名(参数列表){};
    ```
2. 类方法的调用
    调用前提是满足访问修饰符的访问权限和范围。
    ```java
    类名.类方法名(参数列表);
    对象名.类方法名(参数列表);
    ```
4. 类方法经典使用场景
    - 当方法中不涉及到任何和对象相关的成员，则可以将方法设计成静态方法，提高开发效率。
        如：工具类中的方法`java.util.*`、`Math`类、`Arrays`类、`Collections`集合类等。
    - 开发自己的工具类时，可以将方法做成静态的。
5. 类方法注意事项
    - 类方法和普通方法都是随着类的加载而加载，将结构信息存储在方法区中。
        - **类方法中无`this.*`参数。**
        - 普通方法中隐含this参数。
    - 类方法可以通过类名调用，也可以通过对象名调用。
    - **普通方法和对象有关，需要通过对象名调用，比如`对象名.方法名(参数)`，不能通过类名调用。**
    - 类方法中不允许使用和对象有关的关键字，如`this`、`super`。普通成员方法可以。
    - **类方法（静态方法）中只能访问静态变量或静态方法。**
    - 普通成员方法既可以访问普通变量（方法），也可以访问静态变量（方法）。

### 理解main方法语法
1. 深入理解main方法
    - main方法的形式：
        ```java
        public static void main (String[] args){}
        ```
    - main方法由java虚拟机调用。
    - java虚拟机需要调用类的main方法，所以该方法的访问权限必须是`public`。
    - java虚拟机在执行`main()`方法时不必创建对象，所以该方法必须是`static`。
    - 该方法接收`String`类型的数组参数，该数组中保存执行java命令时传递给所有运行的类的参数。
    - `java 执行的程序 参数1 参数2 ...`
2. 特别提示
    - 在`main()`方法中，我们可以直接调用main方法所在类的静态方法和静态属性。
    - 但是，不能直接访问该类的非静态成员，必须创建该类的一个实例对象后，才能通过这个对象去访问类中的非静态成员。

### 代码块
1. 介绍
    代码块又称为初始化块，属于类中的成员（是类的一部分），类似于方法，将逻辑语句封装在方法体内，用`{}`包围起来。
    但和方法不同，没有方法名、没有返回、没有参数、只有方法体，而且不能通过对象或类显示调用，而是加载类时，或创建对象时隐式调用。
2. 基本语法
    ```java
    [修饰符] {
        逻辑语句;
    };
    ```
    - 修饰符可选，要写也只能写`static`。
    - 代码块分为两类，用`static`修饰的代码块称为静态代码块，其他代码块称为普通代码块。
    - 逻辑语句可以为任何逻辑语句（输入、输出、方法调用、循环、判断等）
    - 最后的`;`可写可省略。
3. 使用场景
    - 代码块相当于另一种形式的构造器（对构造器的补充机制），可以做初始化的操作。
    - 场景：如果多个构造器中有重复的雨具，可以抽取到初始化块中，提高代码的重用性。
    - **代码块的调用先于构造器的调用。**
#### 代码块注意事项
1. `static`代码块也叫静态代码块。作用就是对类进行初始化，而且它随着类的加载而执行，且只执行一次。**（类加载只需一次。）**
    如果是普通代码块，每创建一个对象就会执行一次。
2. 类什么时候加载？
    - 创建对象实例时。
    - 创建子类对象实例时，父类也会被加载。父类加载先于子类。
    - 直接使用类的静态成员时（静态属性、静态方法）。
3. 普通代码块，在创建对象实例时，会被隐式调用，被创建一次，就会调用一次。
    如果只是使用类的静态成员时，普通代码块并不会执行。
4. 【重点】创建一个对象时，在一个类调用的顺序是？
    1. 调用静态代码块和静态属性初始化。
        注意：静态代码块和静态属性初始化调用的优先级相同，如果有多个静态代码块和多个静态变量初始化，按其定义顺序调用。
    2. 调用普通代码块和普通属性初始化。
        注意：普通代码块和普通属性初始化调用的优先级相同，如果有多个普通代码块和多个普通变量初始化，按其定义顺序调用。
    3. 最后调用构造器
    4. **小结：由于静态属性调用优于普通属性，故静态属性/方法只能调用静态属性/方法，不能调用普通属性/方法。而普通属性/方法可以调用加载完成的所有属性/方法。**
    5. 实际的执行顺序按照属性/方法的出现顺序执行。
        先来看一段代码：
        ```java
        class A{
            int static a = getAValue();
            static{
                System.out.println("静态代码块");
            }
            public int static getAValue(){
                System.out.println("getAValue");
                return 1;
            }
        }
        ```
        当首次加载A类时，上面的代码输出结果为：
        ```bash
        getAValue
        静态代码块
        ```
        原因：`getAValue()`方法先于静态代码块出现
5. 构造器的最前面，隐含了`super()`和调用普通代码块。
    ```java
    class A{
        public A(){
            // super();
            // 调用普通代码块...
            // 构造器体...
        }
    }
    ```
    另外，静态相关代码块、属性初始化，在类加载时就执行完毕，这部分先于构造器调用。
6. 【面试题】创建一个子类时（继承关系），它们的静态代码块、静态属性初始化、普通代码块、普通属性初始化、构造方法的调用顺序如下：
    1. 父类的静态代码块和静态属性（优先级一样，按定义顺序执行）。
    2. 子类的静态代码块和静态属性（优先级一样，按定义顺序执行）。
    3. **父类的普通代码块和普通属性初始化（优先级一样，按定义顺序执行）。**
    4. 父类的构造方法
    5. 子类的普通代码块和普通属性初始化（优先级一样，按定义顺序执行）。
    6. 子类的构造方法
    7. 用一个简单的代码说明一下。
        ```java
        class A extends B{
            // 调用构造器前，从父类到子类进行加载，此时完成静态方法加载。
            public A(){
                super();                // 调用构造器时，首先调用父类构造方法，依次为：父类普通代码块/普通属性初始化、父类构造器。
                调用普通代码块...        // 调用完父类构造器后，调用子类普通代码块
                构造器体...             // 最后调用子类构造方法
            }
        }
        ```
7. 静态代码块只能直接调用静态成员，普通代码块可以调用任意成员。

### 单例设计模式
1. 什么是设计模式？
    - 静态方法和属性的经典使用模式。
    - 设计模式是在大量的实践中总结和理论化之后优选的代码结构、编程风格以及解决问题的思考方式。好比下棋的棋谱。
2. 什么是单例模式？
    - 所谓的单例设计模式，就是采取一定方法保证在整个软件系统中，对某个类只能存在一个对象实例，并且给类只提供一个取得其对象实例的方法。
        在整个程序的生命周期只创建一个实例。单例，单个实例。
    - 单例模式有两种方式：饿汉式和懒汉式。
3. 饿汉式和懒汉式
    - 二者最主要的区别在于创建对象的时机不同：饿汉式在类加载的时候就创建对象，懒汉式在使用的时候才创建对象。
    - 饿汉式不存在线程安全问题，懒汉式存在线程安全问题。（待学习线程后完善）
    - 饿汉式存在浪费资源的可能。因为如果程序员一个对象实例都没有使用，那么饿汉式创建的对象就浪费了。懒汉式不存在这个问题。
    - 在JavaSE标准类中，`java.lang.Runtime`就是经典的单例模式。
`
#### 单例模式饿汉式
1. 解释
    饿汉式，饿汉很着急，还没有调用，先把对象创建出来。（随类加载创建对象）
2. 饿汉式实现
    - 构造器私有化（防止直接new）
    - 类的内部创建对象（饿汉式）
    - 向外暴露一个静态的公共方法（用于返回唯一的对象）
    - 代码实现
        ```java
        class GirlFriend{   // 一个人只能有一个女朋友
            private String name;
            // 类内部创建对象，这个对象是静态的。
            private static GilrFriend girlFriend = new GirlFriend("小红");
            private GirlFriend(){// 构造器私有化
                this.name = name;
            }
            // 向外暴露一个静态的公共方法
            public static GirlFriend getInstance(){
                return girlFriend;
            }
        }
        ```
3. 饿汉式缺点
    - 仅调用类的静态属性/方法时，对象也会随类加载创建，尽管程序中不需要这个使用这个对象。这样会浪费内存。
    - 采用单例设计模式的对象，通常是重量级对象，这对内存的消耗是较大的。

#### 单例模式懒汉式
1. 解释
    懒汉式，懒汉很懒，只有调用了，才会创建对象。
2. 懒汉式实现
    - 构造器私有化
    - 定义一个static静态属性对象（不进行初始化）
    - 提供一个public static方法，返回这个静态属性对象。
    ```java
    class Cat{  // 我只养一只猫
        private String name;
        private static Cat cat;     // 定义一个static静态属性对象（不进行初始化）
        private Cat(String name){
            this.name = name;
        }
        public static Cat getInstance(){
            if(cat == null){
                cat = new Cat("小猫");
            }
            return cat;
        }
    }
    ```

### final关键字
1. 介绍
    - final 最后的，最终的
    - final可以修饰类、属性、方法、局部变量。
    
    在某些情况下，程序员拥有如下需求会用final修饰。
    - 当不希望类被继承时。
    - 当不希望父类的某个方法被子类覆盖、重写时。
    - 当不希望类的某个属性的值被修改时。（比如税率`TAX_RATE`）
    - 当不希望某个局部变量被修改时。
####  注意事项和细节
1. final修饰的属性又叫常亮，命名一般采用全大写加下划线：`XX_XX_XX`。
2. final修饰的属性在定义时，必须赋初值，并且以后不能再修改，赋值可在如下位置之一：
    - 定义时：`public final double TAX_RATE = 0.05;`
    - 在构造器中。（须提前定义）
    - 在代码块中。（须提前定义）
3. 如果final修饰的属性是静态的，则初始化的位置只能是：
    - 静态代码块中。（须提前定义）
    - 定义时。
    - （不能在构造器中赋值，因为类加载时无法同步赋值）
4. final类不能继承，但可以实例化对象。
5. 如果类不是final类，但含有final方法，则该方法不能重写，但类可以继承。
6. 一般来说，如果一个类已经是final类了，就没有必要将其方法修饰为final。
7. final不能修饰构造方法。
8. final和static往往搭配使用，效率更高，**不会导致类加载**，底层编译器做了优化处理。（如调用静态final变量，不会执行静态代码块）
9. 包装类（`Interger`、`Double`、`Float`、`Boolean`）、`String`都是final类。

### 抽象类
1. 引例
    ```java
    public class Animal {
        private String name;
        private int age;
        public Animal(String name, int age){
            this.name = name;
            this.age = age;
        }
        public void eat(){
            System.out.println("这是一个动物，但暂时不知道吃什么");
        }
    }
    ```
    - 上面的例子中，父类有一个不确定的方法`eat()`，需要子类重写来确定。
    - 抽象类：当父类某些方法需要声明，但是又不确定如何实现时，可以将其声明为抽象方法，这个类就是抽象类。
2. 快速入门
    - 父类方法具有不确定性，考虑将方法设计为抽象(abstract)方法。
    - 所谓抽象方法就是没有实现的方法，所谓没有实现指的是没有方法体
    - 当一个类存在抽象方法时，需要将该类声明为抽象类。
    - 一般来说，抽象类会被继承，由其子类实现抽象方法。
    ```java
    public abstract class Animal {
        //...
        public abstract void eat(); //抽象方法，没有花括号。
    }
    ```
3. 抽象类介绍
    - 用abstract关键字修饰的类就是抽象类。
        ```java
        访问修饰符 abstract class 类名{}
        ```
    - 用abstract关键字修饰的方法就是抽象方法。
        ```java
        访问修饰符 abstract 返回值类型 方法名(参数列表);    // 没有方法体（花括号）
        ```
    - 抽象类的价值更多作用是在于设计，是设计者设计好后，让子类继承并实现。
    - **抽象类是面试题重点，在框架和设计模式使用较多。**

4. 注意事项
    - 抽象类不能实例化对象。
    - 抽象类不一定要包含抽象方法。
    - 一旦包含抽象方法，那么类必须声明为抽象类。
    - abstract只能修饰类和方法，不能修饰属性和其他。
    - 抽象类可以有任意成员（抽象类本质还是类，可以拥有非抽象方法、构造器、静态属性等）
    - 抽象方法不能有主体，即不能实现。
    - 如果一个类继承了抽象类，则它必须实现抽象类中的所有抽象方法，除非它自己也声明为抽象类。
    - **抽象方法不能用private、final、static修饰。他们和重写相违背。**

#### 抽象类的最佳实践——模板设计模式
1. 需求
    - 有多个类，完成不同的任务`job`。
    - 要求能够统计得到各自完成任务的时间。
    ```java
    abstract public class Template {
        public abstract void job() {}       // 子类只重写job()方法
        public void countTime() {
            long start = System.currentTimeMillis();
            job();      // 动态绑定机制
            long end = System.currentTimeMillis();
            System.out.println("耗时：" + (end - start));
        }
    }
    ```
2. 中心思想：提高代码复用性

### 接口
1. 基本介绍
    接口就是给出一些没有实现的方法，封装到一起，到某个类要使用的时候，再根据具体情况把这些方法写出来。
2. 语法
    ```java
    public interface 接口名 {       // 接口名首字母大写
        // 属性
        // 方法
    }

    class 类名 implements 接口名 {
        // 自己属性
        // 自己方法
        // 必须实现的接口的抽象方法
    }
    ```
3. 特性
    - 在JDK7前，接口里的所有方法都没有方法体。  
    - JDK8之后的就扣类可以有静态方法、默认方法，也就是说就口中可以有方法的具体实现。
        - **默认方法须默认修饰符修饰。**
        - **静态方法须static修饰符修饰。**

4. 细节
    - 接口不能被实例化。
    - 接口中的所有方法都是public方法，接口中的抽象方法可以不用abstract修饰。
    - **一个普通类实现接口，就必须将该接口的所有方法都实现。**（可使用Alt+Enter快捷键）
    - 抽象类实现接口时，可以不实现接口的抽象方法。
    - 一个类可以同时实现多个接口。
        ```java
        class A implements IB,IC{}
        ```
    - 接口中的属性只能是final的，而且是`public static final`修饰。
    - 接口中属性的访问形式：`接口名.属性名`。
    - 接口不能继承其它的类，但能继承多个别的接口。
        ```java
        interface IA extends IB,IC{}
        ```
    - 接口的修饰符只能是`public`和`default`。
5. 实现接口与继承的区别
    - 实现机制是Java对单继承机制的一种补充。
        一个类只有一个爹，父类有的技能可以遗传给子类，其他实现要通过接口学习。
    - 接口和继承解决的问题不同
        - 继承的价值在于：解决代码的复用性和可维护性。
        - 接口的价值在于：设计，设计好各种规范（方法），让其他类去实现这些方法，即更加的灵活。
    - 接口比继承更加灵活
        - 接口比继承更灵活，继承是满足is-a的关系，而接口是满足like-a的关系。
    - 接口在一定程度上实现了代码的解耦。（接口规范性+动态绑定）

#### 快速入门案例
1. `Camara.java`
    ```java
    package com.lcq.interface_;

    public class Camara implements UsbInterface {
        public void start() {
            System.out.println("Camara start");
        }
        public void stop() {
            System.out.println("Camara stop");
        }
    }
    ```
2. `Computer.java`
    ```java
    package com.lcq.interface_;

    public class Computer {
        public void work(UsbInterface usbInterface){
            usbInterface.start();
            usbInterface.stop();
        }
    }
    ```
3. `Phone.java`
    ```java
    package com.lcq.interface_;

    public class Phone implements UsbInterface {
        public void start() {
            System.out.println("Phone start");
        }

        @Override
        public void stop() {
            System.out.println("Phone stop");
        }
    }
    ```
4. `UsbInterface.java`
    ```java
    package com.lcq.interface_;

    public interface UsbInterface {
        public void start();
        public void stop();
    }
    ```
5. `InterfaceTest01.java`
    ```java
    package com.lcq.interface_;

    public class InterfaceTest01 {
        public static void main(String[] args) {
            Computer computer = new Computer();
            Camara camara = new Camara();
            Phone phone = new Phone();
            computer.work(camara);
            computer.work(phone);
        }
    }
    ```
#### 接口的多态特性
1. 多态参数
    - 在上面的`UsbInterface`中，既可以接收`Camara`，也可以接收`Phone`，体现了接口的多态。**（接口引用可以指向实现了接口的类的对象）**
    - 接口也适用于向上转型，可以实现类似于继承的功能
        ```java
        main{
            IA a = new A();
            a = new B();
        }
        class A implements IA{...}
        class B implements IA{...}
        ```
2. 多态数组实例
    ```java
    package com.hspedu.interface_;

    public class InterfacePolyArr {
        public static void main(String[] args) {

            //多态数组 -> 接口类型数组
            Usb[] usbs = new Usb[2];
            usbs[0] = new Phone_();
            usbs[1] = new Camera_();
            /*
            给Usb数组中，存放 Phone  和  相机对象，Phone类还有一个特有的方法call（），
            请遍历Usb数组，如果是Phone对象，除了调用Usb 接口定义的方法外，
            还需要调用Phone 特有方法 call
            */
            for(int i = 0; i < usbs.length; i++) {
                usbs[i].work();//动态绑定..
                //和前面一样，我们仍然需要进行类型的向下转型
                if(usbs[i] instanceof Phone_) {//判断他的运行类型是 Phone_
                    ((Phone_) usbs[i]).call();
                }
            }

        }
    }

    interface Usb{
        void work();
    }
    class Phone_ implements Usb {
        public void call() {
            System.out.println("手机可以打电话...");
        }

        @Override
        public void work() {
            System.out.println("手机工作中...");
        }
    }
    class Camera_ implements Usb {

        @Override
        public void work() {
            System.out.println("相机工作中...");
        }
    }
    ```
3. 接口的多态传递
    ```java
    main{
        IB ib = new A();
        IA ia = new A();    // 接口多态传递
    }
    interface IA{}
    interface IB extends IA{}   // 接口继承
    class A implements IB{}
    ```

### 内部类
1. 介绍
    - 一个类的内部又完整的嵌套了另一个类结构。被嵌套的类称为内部类（inner class），嵌套其他类的类称为外部类（outer class）。是类的第五大成员（**属性、构造器、代码块、方法、内部类**）。
    - 内部类的最大特点，是可以直接访问私有属性，并且可以体现类与类之间的包含关系。
2. 基本语法
    ```java
    class OuterClass{           // 外部类
        class InnerClass{...}   // 内部类
    }
    class Other{}               // 外部其他类
    ```
3. 内部类分类
    定义在外部类局部位置上（如方法内）
    - 局部内部类（有类名）
    - 匿名内部类（没有类名，重点！！）
    
    定义在外部类的成员位置上
    - 成员内部类（无static修饰）
    - 静态内部类（有static修饰）

#### 局部内部类
1. 注意事项
    局部内部类定义在外部类的局部位置，比如方法中，并且有类名。
    本质是一个地位与局部变量相当的类。
    - 可以直接访问外部类的所有成员，包括私有成员。
    - 不能添加访问修饰符，因为他的地位就是一个局部变量。局部变量不能使用修饰符，但可以使用final修饰。
    - 作用域：仅仅在定义它的方法或者代码块中。
    - **局部内部类访问外部类成员：直接访问。**
    - **外部类访问局部内部类成员：创建对象再访问（前提是在作用域中）**
    - 外部其他类不能访问局部内部类。
    - 如果外部类和局部内部类成员重名时，默认遵循就近原则，如果想访问外部类的成员，则可使用`外部类名.this.成员`访问。
        `Outer.this`的本质就是一个外部类对象。
#### 匿名内部类（最重要）
1. 介绍
    - 匿名内部类(anonymous inner class)的性质：
        - 匿名内部类本质是一个类，是一个内部类。
        - 这个类没有名字。
        - 匿名内部类同时是一个对象。
    - 匿名内部类是定义在外部类的局部位置，比如方法中，并且没有类名。
2. 基本语法
    ```java
    new 类或接口(参数列表){
        类体
    }
    ```
3. 基于接口的匿名内部类
    先看一段代码
    ```java
    public class AnonymousInnerClass01 {
        public static void main(String[] args) {
            MyInterface myInterface = new Tiger();
            myInterface.cry();
        }            
    }
    interface MyInterface{
        void cry();
    }
    class Outer01{
        private int num = 10;
        public void method(){
            // 要求：在本方法中调用MyInterface接口并创建对象
        }
    }
    ```
    - 传统方案：定义一个类实现接口
        ```java
        public class AnonymousInnerClass01 {
            public static void main(String[] args) {
                MyInterface myInterface = new Outer01();
                myInterface.method();
            }            
        }
        interface MyInterface{
            void cry();
        }
        class Tiger implements MyInterface{         // 定义一个Tiger类实现MyInterface接口
            @Override
            void cry() {
                System.out.println("老虎叫");
            }
        };
        class Outer01{
            private int num = 10;
            public void method(){
                MyInterface tiger = new Tiger();          // new一个Tiger，然后调用cry
                tiger.cry();
            }
        }
        ```      
    - 比较要命的是，老虎类我就是一时兴起只用一次，以后再也不用了，此时匿名内部类就可以登场了，起到一个阅后即焚的作用。
        ```java 
        public class AnonymousInnerClass02 {
            public static void main(String[] args) {
                MyInterface myInterface = new Outer01();
                myInterface.method();                
            }            
        }
        interface MyInterface{
            void cry();
        }
        class Outer01 {
            private int num = 10;
            public void method(){
                // 定义一个匿名内部类并实例化
                MyInterface tiger = new MyInterface(){
                    @Override
                    public void cry() {
                        System.out.println("老虎叫");
                    }
                };
                tiger.cry();
            }
        }

        /*
        底层实现：
        class Outer01$1 implements MyInterface{ 
            @Override
            public void cry() {
                System.out.println("老虎叫");
            }
        }

        如果还有别的匿名内部类，则命名顺延：Outer01$2...
        */
        ```
        - `Outer01$1`就是底层匿名内部类的实际名称，是由系统在编译过程中自动分配的。带美元符号的就是内部类。
        - 匿名内部类实例化一次就不能再使用了。
4. 基于类的匿名内部类
    ```java
    class Father{
        public Father(String name){}
        public void method01(){}
    class Outer02{
        private int num = 1;
        public void method(){
            Father father = new Father("name"){};   // 后面带了大括号，这是个匿名内部类，名为Outer02$1
            System.out.println(father.getClass());
        }
    }
    }

    /*
    底层实现
    class Outer02$1 extends Father{}
    */
    ```
    这有什么意义？多人协作时，别人写的类不一定完美契合你的需求，需要修改。
5. 基于抽象类的匿名内部类
    由于类中部分方法是抽象的，匿名内部类必须实现抽象方法才能调用。
    特别的，基于抽象类的匿名内部类不实现抽象方法会报错。

6. 匿名内部类的注意事项
    - 匿名内部类的语法比较特别，因为匿名内部类既是一个类的定义，其本身也是一个对象，因此从语法上看，其既有定义类的特征，也有创建对象的特征。
    - 调用匿名内部类的两种方法
        - 第一种
            ```java
            new A (){
                @Override
                public void show(){
                    System.out.println("匿名内部类");
                }
            }.show();
            ```
            **由于动态绑定机制的存在，这样做是有意义的，这可以改变所调用方法内部的逻辑。比如A类中有一个方法`method01`调用了这里的`show`方法，我们以匿名内部类调用的方法时`method01`，此时其会优先调用匿名内部类中的`show`方法，而不会调用A类中的`show`方法。**
        - 第2种
            ```java
            A a = new A(){
                @Override
                public void show(){
                    System.out.println("匿名内部类");
                }
            }
            a.show();
            ```
            
    - 可以直接访问外部类的所有成员（包括私有的）
    - 匿名内部类不能添加访问修饰符，因为它是一个局部变量。
    - 作用域：仅仅在定义它的方法或代码块中。
    - 外部其他类不能访问匿名内部类（因为其本质就是一个局部变量）。
    - 外部类成员和匿名内部类成员重名时，按就近原则优先访问匿名内部类成员，此时访问外部类成员需要`外部类名.this.成员名`。

7. 匿名内部类的最佳实践
    - 将匿名内部类当做实参直接传递
        ```java
        public class InnerClassExercise01 {
            public static void main(String[] args) {
                f1(new IA(){
                    public void show() {
                        System.out.println("hello world");
                    }
                })
            }
            public static void f1(IA ia) {
                ia.show();
            }
        }
        interface IA {
            void show();
        }
        ```
    - 一个例子
        ```java
        package com.lcq.innerclass;

        public class InnerClassExercise02 {
            public static void main(String[] args) {
                CellPhone.alarmclock(new Bell() {
                    @Override
                    public void ring() {
                        System.out.println("懒猪起床了");
                    }
                });
                CellPhone.alarmclock(new Bell() {
                    @Override
                    public void ring() {
                        System.out.println("小伙伴上课了");
                    }
                });
            }
        }

        interface Bell{
            public void ring();
        }

        class CellPhone{
            public static void alarmclock(Bell bell){
                bell.ring();
                System.out.println(bell.getClass());
            }
        }
        ```
#### 成员内部类
1. 介绍
    - 成员内部类是定义在外部类的成员位置的类，且没有static修饰。
    - 成员内部类可以直接访问外部类的所有成员，包括私有成员。
    - 成员内部类可以添加任意访问修饰符，因为它的地位等同于成员。
    - 作用域：和外部类其他成员一样，为整个外部类类体。（先实例化再调用）
    - 成员内部类可以直接访问外部类成员。
    - 外部类访问内部类需要先创建对象再访问。
    - 外部其他类访问内部类
        ```java
        // 现有外部类Outer和内部类Inner
        // 其他外部类访问成员内部类方法如下：

        // 方法1：实例化外部类，然后定义内部类。
        Outer outer = new Outer();              
        Outer.Inner inner = outer.new Inner();  

        // 方法2：在外部类中定义一个返回内部类的方法 Inner getInnerInstance(){return new Inner();}
        Outer.Inner inner = outer.getInnerInstance();

        // 方法3：调用一个外部类匿名对象
        Outer.Inner inner = new Outer().new Inner();        
        ```
    - 外部类和成员内部类成员，内部类访问遵循就近原则，如果想访问外部类成员，需要`外部类名.this.成员名`。
#### 静态内部类
1. 介绍
    - 定义在外部类的成员位置，并且有static修饰。
    - 静态内部类可以直接访问外部类的所有静态成员，包括私有的，但不能直接访问非静态成员。
    - 作用域：同其他成员，为整个整体。
    - 静态内部类可以直接访问外部类的**所有静态成员**。
    - 外部类访问静态内部类需要先创建对象再访问。
    - 外部其他类访问静态内部类：
        ```java
        // 现有外部类Outer和内部类Inner
        // 其他外部类访问静态内部类方法如下：
        
        // 方法1：实例化外部类，然后访问内部类
        Outer.Inner inner = new Outer.Inner();  // 静态内部类可以直接通过类名访问（前提是满足访问权限）
        inner.method();

        // 方法2：在外部类中定义一个返回静态内部类对象的方法 static Inner getInnerInstance(){return new Inner();}
        Outer.Inner inner = Outer.getInnerInstance();

        // 方法2+：可以定义非静态方法，先实例化外部类，再调用外部类方法，获取静态内部类对象
        // Inner getInnerInstance(){return new Inner();}
        Outer outer = new Outer();
        Outer.Inner inner = outer.getInnerInstance();
        ```
    - 外部类和静态内部类成员重名时，静态内部类访问采取就近原则，访问外部类成员需要`外部类名.成员名`。

## 枚举与注解
### 枚举类
1. 引例
    设计一个季节类，包含名字和描述两个属性。
    ```java
    class Season{
        private String name;
        private String desc;
    }
    ```
    这样设计有一个问题，那就是季节是有数的，但这样一个类显然可以随意实例化，由此引出枚举。
    而且季节名是固定不可修改的，所以：
    - 季节的值是有限的几个值。
    - 季节的值是只读的，不需要修改。
2. 介绍 
    - 枚举（enum, enumeration）
    - 枚举是一组常量的集合。
    - 枚举是一种特殊的类，只包含一组有限的特定对象。
3. 实现方式
    - 自定义枚举
        - 构造器私有化
        - 去掉`setXXX()`方法，防止属性被修改。
        - 在类内部直接创建固定的对象。
        - 对外暴露对象（暴露public static final）
        ```java
        class Season{
            
            public final static Season SPRING = new Season("春天", "温暖");
            public final static Season SUMMER = new Season("夏天", "炎热");
            public final static Season AUTUMN = new Season("秋天", "凉爽");
            public final static Season WINTER = new Season("冬天", "寒冷");

            private Season (String name, String desc){
                this.name = name;
                this.desc = desc;
            }
            // 仅保留get方法。。。
        }
        ```
    - 使用enum关键字实现枚举
        - 使用enum替代class
        - 写作`常量名(属性),常量名(属性),...,常量名(属性);`
            逗号分隔，分号结尾。
        - 枚举的常量要写在枚举类开头。
        ```java
        enum Season2{
            SPRING("春天", "温暖"),
            SUMMER("夏天", "炎热"),
            AUTUMN("秋天", "凉爽"),
            WINTER("冬天", "寒冷");
            private String name;
            private String desc;
            private Season2 (String name, String desc){
                this.name = name;
                this.desc = desc;
            }
            public String getName(){
                return name;
            }
            public String getDesc(){
                return desc;
            }
        }
        ```

4. 自定义枚举小结
    - 不需要提供set方法，因为枚举对象通常为只读。
    - 对枚举对象/属性使用final+static共同修饰，实现底层优化。
    - 枚举对象名通常使用全部大写（常量的命名规范）
    - 枚举对象根据需要，也可以有多个属性。

5. enum注意事项
    - 使用enum关键字开发一个枚举类时，默认会继承Enum类。（通过javap反编译可以看见）
    - 需要明确调用的构造器，如果调用无参构造器，则可以不写括号和实参。
    - 有多个枚举对象时，使用逗号间隔，最后一个以分号结尾。
    - 枚举对象必须放在枚举类行首。

6. 关于Enum
    ```java
    public abstract class Enum<E extends Enum<E>> implements Comparable<E>, Serializable {}
    ```
    - `toString()`方法：默认返回枚举对象的名字，子类可以重写这个方法。
        ```java
        public String toString() { return name(); }
        ```
    - `name()`：返回当前对象（常量名），不能重写。
        ```java
        public final String name() { return name; }
        ```
    - `ordinal()`：返回当前对象在枚举类中的索引，从0开始。
    - `values()`：static，返回枚举类中的所有对象。
    - `valueOf()`：static，将字符串转换成枚举对象（必须是已有常量名！）
    - `compareTo()`：比较两个枚举对象的位置号，返回两个对象在枚举类中的索引差值。（`self.ordinal() - other.ordinal()`） 
        ```java
        <Enum1>.compareTo(<Enum2>)
        return <Enum1>.ordinal() - <Enum2>.ordinal()
        ```

7. enum实现接口
    - 使用enum关键字后，就不能继承类了，只能实现接口。（因为enum关键字隐式继承Enum类，Java不支持多继承）
    - 枚举类可以实现接口。

### 注解（Annotation）
1. 注解的理解
    - 注解也被称为元数据，用于修饰解释 包、类、方法、属性、构造器、局部变量等数据信息。
    - 和注释一样，注解不影响程序逻辑，但注解可以被编译与运行，相当于嵌入在代码中的补充信息。
    - 在JavaSE中，注解的使用目的比较简单，例如标记过时的功能、忽略警告等。
    - 在JavaEE中，注解的作用比较强大，例如：配置应用程序的任何切面、代替JavaEE旧版中所遗留的繁冗代码和XML配置等。

2. 基本的Annotation
    使用Annotation时，要在前面增加`@`符号，并把该Annotation当做一个修饰符使用。用于修饰它支持的程序元素。
    - `@Override`：限定某个方法，是重写父类方法，该注解只能用于方法。
    - `@Deprecated`：用于表示某个程序元素（类、方法等）已过时。
    - `@SuppressWarnings`：抑制编译器警告。

3. `@Override`使用说明
    - `@Override`表示指定重写父类的方法（从编译层面验证），如果父类没有此方法，直接报错。
    - 如果不写`@Override`，但父类仍有相同方法，则构成重写。
    - `@Override`只能修饰方法。（由源码中的`@Target(ElementType.METHOD)`限定，这里的`@Target`是修饰注解的注解，称为元注解）
4. `@Deprecated`使用说明
    - 用于表示某个程序元素（类、方法等）已经过时，不建议使用。
    - 可以修饰方法、类、字段、包、参数等。
    - 源码
        ```java
        @Documented
        @Retention(RetentionPolicy.RUNTIME)
        // 从左至右：构造器、属性、局部变量、方法、包、参数、类型
        @Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, PARAMETER, TYPE})
        public @interface Deprecated {
        }
        ```
    - `@Deprecated`的作用可以做到新旧版本的兼容和过渡。
5. `@SuppressWarnings`使用说明
    - 语法
        ```java
        @SuppressWarnings({"unchecked","deprecation"})
        public void test(){...}
        ```
    - 将注解放在不同位置，可以控制抑制警告的生效范围（比如方法、类）
    - 源码
        ```java
        //从左至右：类型、属性、方法、参数、构造器、局部变量，且需传入字符串数组
        @Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})
        @Retention(RetentionPolicy.SOURCE)
        public @interface SuppressWarnings {
            /**
            * The set of warnings that are to be suppressed by the compiler in the
            * annotated element.  Duplicate names are permitted.  The second and
            * successive occurrences of a name are ignored.  The presence of
            * unrecognized warning names is <i>not</i> an error: Compilers must
            * ignore any warning names they do not recognize.  They are, however,
            * free to emit a warning if an annotation contains an unrecognized
            * warning name.
            *
            * <p> The string {@code "unchecked"} is used to suppress
            * unchecked warnings. Compiler vendors should document the
            * additional warning names they support in conjunction with this
            * annotation type. They are encouraged to cooperate to ensure
            * that the same names work across multiple compilers.
            * @return the set of warnings to be suppressed
            */
            String[] value();
        }
        ```
    - 可以指定的警告类型有
        -  all，抑制所有警告
        -  boxing，抑制与封装/拆装作业相关的警告
        -  cast，抑制与强制转型作业相关的警告
        -  dep-ann，抑制与淘汰注释相关的警告
        -  deprecation，抑制与淘汰的相关警告
        -  fallthrough，抑制与 switch 陈述式中遗漏 break 相关的警告
        -  finally，抑制与未传回 finally 区块相关的警告
        -  hiding，抑制与隐藏变数的区域变数相关的警告
        -  incomplete-switch，抑制与 switch 陈述式(enum case)中遗漏项目相关的警告
        -  javadoc，抑制与 javadoc 相关的警告
        -  nls，抑制与非 nls 字串文字相关的警告
        -  null，抑制与空值分析相关的警告
        -  rawtypes，抑制与使用 raw 类型相关的警告
        -  resource，抑制与使用 Closeable 类型的资源相关的警告
        -  restriction，抑制与使用不建议或禁止参照相关的警告
        -  serial，抑制与可序列化的类别遗漏 serialVersionUID 栏位相关的警告
        -  static-access，抑制与静态存取不正确相关的警告
        -  static-method，抑制与可能宣告为 static 的方法相关的警告
        -  super，抑制与置换方法相关但不含 super 呼叫的警告
        -  synthetic-access，抑制与内部类别的存取未最佳化相关的警告
        -  sync-override，抑制因为置换同步方法而遗漏同步化的警告
        -  unchecked，抑制与未检查的作业相关的警告
        -  unqualified-field-access，抑制与栏位存取不合格相关的警告
        -  unused，抑制与未用的程式码及停用的程式码相关的警告

6. 关于注解的补充说明
    - 当查看注解的源码时，会看到如下内容：
        ```java
        @Target(ElementType.METHOD)
        @Retention(RetentionPolicy.SOURCE)
        public @interface Override {
        }
        ```
        这并不代表注解是一个接口，`@interface`仅代表其是一个注解类。
        这时jdk5.0后添加的。

7. 【了解】元注解
    元注解就是修饰注解的注解。
    - `@Target`：用于描述注解的使用范围（即：被描述的注解可以用在什么地方）。
    - `@Retention`：用于描述注解的作用范围（即：被描述的注解在什么范围内有效）。
        - 只能用于修饰一个Annotation的定义，用于指定该Annotation被保留的时间长短。其包含一个RentationPolicy类型的变量，使用`@Retention`时必须为该value成员指定值
            - `RentationPolicy.SOURCE`：编译器使用后，直接丢弃这种策略的注释。
            - `RentationPolicy.CLASS`：编译器将注解记录在class文件中，但JVM将会忽略。
            - `RentationPolicy.RUNTIME`：编译器将注解记录在class文件中，JVM将会保留。
        - 图解
            ![java_annotation_retention](./img/java_annotation_retention.jpg)
    - `@Documented`：用于描述注解是否被包含在JavaDoc中。
    - `@Inherited`：用于描述注解是否被继承。


    
### 附加内容
#### 增强for循环
1. 语法
    ```java
    for(<数据类型> <变量名> : <数组名>){
        <语句>
    }
    ```
    - 这里的数组是一个可迭代对象。
    - 在循环中，`<变量名>`会自动赋值为数组中的每一个元素。
    - 在IDEA中，输入iter可以快速生成增强for结构。

## 异常（Exception）
### 引例
1. 代码
    ```java
    public class Exception01 {
        public static void main(String[] args) {
            int num1 = 10;
            int num2 = 0;
            int res = num1 / num2;
            System.out.println("continue...");
        }
    }
    ```
2. 错误反馈
    ```bash
    Exception01
    Exception in thread "main" java.lang.ArithmeticException: / by zero
        at Exception01.main(Exception01.java:9)
    ```
3. 点评
    - 当程序遇到除以0的错误时，就会抛出`ArithmeticException`异常。
    - 抛出异常后，程序直接崩溃，下面的代码不再执行。
    - 这种程序并不好，因为一个不算致命的小问题导致了整个系统的崩溃。
    - Java设计者提供了一个异常处理机制解决这个问题。

4. 异常捕获
    - 对上述代码进行更改
        ```java
        //...
        try {
            int res = num1 / num2;
        }catch (Exception e){
            //两种方法
            e.printStackTrace();
            System.out.println(e.getMessage());
        }
        ```
    - 运行结果
        ```bash
        java.lang.ArithmeticException: / by zero
            at Exception01.main(Exception01.java:10)
        / by zero
        continue...
        ```
### 异常介绍
1. 基本概念
    - Java语言中，将程序执行中发生的不正常情况称为“异常”。（**开发过程中的语法错误和逻辑错误不是异常**）
2. 执行过程中的两类异常事件
    - `Error`（错误）：Java虚拟机无法解决的严重问题。如：JVM系统内部错误、资源耗尽等严重情况。如：`StackOverflowError`（堆栈溢出）、OOM（`out of memory`，内存溢出）。Error是严重错误，程序会崩溃。
    - `Exception`（异常）：其他因编程错误或偶然的外在因素导致的一般性问题，可以使用针对性的代码进行处理。例如：空指针访问、试图读取不存在的文件、网络连接中断等。
        **Exception也分为两类：运行时异常和编译时异常。**
3. 异常体系图
    ![java_exception_hierarchy](./img/java_exception_hierarchy.png)
    - 异常分为两大类：运行时异常（非受检异常）和编译时异常（受检异常）。
    - 运行时异常，编译器检查不出来。一般是指编程时的逻辑错误，是程序员应该避免其出现的异常。`java.lang.RuntimeException`是所有运行时异常的基类。
    - 运行时异常可以不做处理，因为这类异常很普遍，如果全处理会对程序的可读性和运行效率产生影响。
    - 编译时异常是编译器要求必须处理的异常。
#### 常见运行时异常
1. `NullPointerException` 空指针异常
    - 当应用程序试图在需要对象的地方使用null时抛出该异常。
    - 示例：
        ```java
        public static void main(String[] args) {
            String name = null;
            System.out.println(name.length());
        }
        ```
    - 错误
        ```bash
        Exception in thread "main" java.lang.NullPointerException
        at com.lcq.exception_.NullPointerException_.main(NullPointerException_.java:10)
        ```    
2. `ArithmeticException` 数学运算异常
    - 当出现异常的运算条件时，抛出此异常。
3. `ArrayIndexOutOfBoundsException` 数组下标越界异常
    - 用非法索引访问数组时抛出的异常。（如索引为负或大于等于`arr.length`时）
    - 示例：
        ```java
        public static void main(String[] args) {
            int[] arr = new int[10];
            System.out.println(arr[10]);
        }
        ```
    - 错误
        ```bash
        Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: 10
        at com.lcq.exception_.ArrayIndexOutOfBoundsException_.main(ArrayIndexOutOfBoundsException_.java:10)
        ```    
4. `ClassCastException` 类型转换异常
    - 当试图将一个对象强转成非该实例子类时，抛出的异常。
5. `NumberFormatException` 数字格式不正确异常
    - 当应用程序试图将字符串转换成一种数值类型，但该字符串不能转换成适当格式时，抛出此异常。
    - 使用这个异常可以确保输入的是满足条件的数字。
    - 例如：
        ```java
        public static void main(String[] args) {
            String str1 = "123";
            int num = Integer.parseInt(str1);
            String str2 = "aha";
            int num2 = Integer.parseInt(str2);  // 抛出异常
        }
        ```
#### 常见编译异常
1. 介绍
    - 编译异常是指在编译期间就必须处理的异常，否则代码不能通过编译。
2. 常见编译异常
    - `SQLException`：操作数据库时，查询表可能发生异常。
    - `IOException`：操作文件时，发生的异常。
    - `FileNotFoundException`：当操作一个不存在的文件时，发生异常。
    - `ClassNotFoundException`：加载类，而该类不存在时，异常。
    - `EOFException`：操作文件，到文件末尾，发生异常。
    - `llegalArguementException`：参数异常。

### 异常处理
1. 基本介绍
    - 异常处理就是异常发生时，对异常的处理方式。
2. 异常处理的方式
    - `try-catch-finally`
        程序员在代码中捕获发生的异常。
        ![java_exception_tryCatchFinally](./img/java_exception_tryCatchFinally.png)
    - `throws`
        将发生的异常抛出，交给调用者（方法）处理，最顶级的处理者就是JVM。
        ![java_exception_throws](./img/java_exception_throws.png)

#### try-catch异常处理
1. 说明
    - Java提供try-catch块处理异常。
    - try块用于包含可能出错的代码。
    - catch块用于处理try块中的异常。
    - 可以根据需要在程序中有多个数量的try-catch块。
2. 基本语法
    ```java
    try {
        // 可能会出错的代码
    } catch (ExceptionType1 e1) {
        // 处理异常的代码
    }
    ```
3. 细节
    - 如果异常发生了，则try块中异常发生位置后的代码不会执行，直接跳到catch块中。
        ```java
        public static void main(String[] args) {
            String name = "aha";
            try {
                int a = Integer.parseInt(name);     // 异常位置
                System.out.println(a);              // 不会执行
            } catch (NumberFormatException e) {
                System.out.println(e.getMessage());
            }
        }
        ```
    - 如果异常没有发生，则顺序执行try，不执行catch。
    - 如果希望不管是否发生异常，都执行某段代码（比如关闭连接、释放资源等）则将这部分写入finally块中。
    - 如果一个try块中可能包含多个异常，可以用多个catch块分别捕获异常，这时要遵循：**子类异常优先捕获。**
    - 存在try-finally结构，此时不会捕获异常，而是直接崩掉，这种结构的作用是，无论程序是否崩溃，都会执行某个业务逻辑。

4. try-catch-finally执行顺序
    - 如果没有出现异常，则执行try块，然后执行finally块。
    - 如果出现异常，自try块异常点开始的所有语句均不执行，直接执行catch块，然后执行finally块。

5. 一个例子
    ```java
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int num;
        boolean loop = true;
        while (loop) {
            try {
                System.out.print("Enter number:");
                num = scanner.nextInt();
                System.out.println("You entered: " + num);
                loop = false;
            } catch (InputMismatchException e) {
                System.out.println("必须输入一个整数！！！");
                scanner.next();
            }
                
            }
        }
    ```

#### throws 异常处理
1. 基本介绍
    - 如果一个方法（中的语句执行时）可能生成某种异常，但是并不能确定如何处理这种异常，则此方法应显式声明抛出异常，表明该方法不对这些异常进行处理，而由该方法的调用者负责处理。
    - 在方法声明中使用throws关键字，可以声明抛出异常的列表，throws后面的异常类型可以是方法中产生的异常类型，也可以是他的父类。
2. 用法实例
    - 这里给出一个调用了IO流的实例。
    - 从源码可以看出，构造器向上抛出`FileNotFoundException`异常，方法`func`调用时捕捉到异常。
    - 这是一个编译异常，只有解决了才可以编译，此时方法`func`有两个方案：继续向上抛（`throws`）或者自行解决（`try-catch-finally`）。例子中采用了前者的方法。
    ```java
    // 可以抛出多个异常，嫌麻烦可以直接抛出一个涵盖异常较广的父类，比如所有异常的祖宗`Exception`。
    public void func() throws FileNotFoundException,NullPointerException{
        FileInputStream fis = new FileInputStream("d:\\lcq.txt");
    }
    /*
        public FileInputStream(String name) throws FileNotFoundException {
            this(name != null ? new File(name) : null);
        }
    */
    ```
3. 注意事项
    - 对于编译异常，程序中必须处理，如`try-catch`和`throws`。
    - 对于运行时异常，程序中如果没处理，默认`throws`。
    - 子类重写父类的方法时，对抛出异常的规定：子类重写的方法，所抛出的异常要么和父类异常一致，要么是父类抛出异常的子类异常。（儿子就是儿子）。
    - 在throws过程中，如果有方法try-catch，相当于处理异常，就不必throws。

### 自定义异常
1. 基本概念
    - 当程序中出现了某些“错误”，但该错误信息并没有在`Throwable`子类中描述处理，这个时候就可以设计自己的异常类，用于描述这个错误。
2. 自定义异常的步骤
    - 定义类：自定义异常类名，继承`Exception`或`RuntimeException`。
    - 如果继承前者，属于编译异常。
    - 如果继承后者，属于运行时异常。（一般选择继承后者）
3. 自定义异常实例
    ```java
    public class CustomException {
        public static void main(String[] args) /*throws AgeException*/{
            int age = 20;
            if(!(age >= 18 && age <= 120)){
                throw new AgeException("年龄不合法");
            }else {
                System.out.println("合法");
            }
        }
    }
    class AgeException extends RuntimeException {
        public AgeException(String message) {
            super(message);
        }
    }
    ```
    - 将异常包装成运行时异常，可以使用默认的处理机制，否则需要再main方法显式抛出你的异常。

4. `throws`和`throw`
    ||意义|位置|后面跟的东西|
    |:--:|:--:|:--:|:--:|
    |`throws`|异常处理的一种方式|方法声明处|异常类型|
    |`throw`|手动生成异常的关键字|方法体中|异常对象（new）|

## 常用类

### 章节目录
1. 包装类
2. `String`类
3. `StringBuffer`和`StringBuilder`类
4. `Math`类
5. `Date`日期类、`Calendar`日历类
6. `System`类
7. `Arrays`类
8. `BigInterger`类和`BigDecimal`类

### 包装类（Wrapper）
1. 包装类的分类
    - 针对八种基本数据类型相应的引用类型——包装类。
    - 有了类的特点，就可以调用类中的方法。

2. 对照表
    |基本数据类型|包装类|
    |:---:|:---:|
    |`boolean`|`Boolean`|
    |`char`|`Character`|
    |`byte`|`Byte`|
    |`short`|`Short`|
    |`int`|`Integer`|
    |`long`|`Long`|
    |`float`|`Float`|
    |`double`|`Double`|

3. 包装类与基本数据类型之间的转换
    - jdk5之前的手动装箱拆箱方式（装箱：基本类型->包装类型）
    - jdk5之后的自动装箱和拆箱方式。
    - 自动装箱底层调用的是`valueOf()`，如：`Integer.valueOf()`。
    ```java
    // 手动装箱
    int i = 1;
    Integer integer = new Integer(i);
    Integer integer1 = Integer.valueOf(i);

    // 手动拆箱
    int j = integer1.intValue();

    // jdk5后，自动装箱拆箱（底层还是valueOf）
    int n2 = 200;
    Integer integer2 = n2;
    ```

4. 包装类和`String`的转换
    ```java
    Integer i = 100;

    // 包装类 -> 字符串
    // 方法1
    String str1 = i + "";

    // 方法2：toString
    String str2 = i.toString();

    // 方法3：valueOf
    String str3 = String.valueOf(i);

    // 字符串 -> 包装类
    String str4 = "123";
    Integer i2 = Integer.parseInt(str4);
    Integer i3 = new Integer(str4);
    ```

5. `Integer`常用方法
    - `Integer.MIN_VALUE`：返回最大值。
    - `Integer.MAX_VALUE`：返回最小值。
6. `Character`常用方法
    - `Character.isDigit('a')`：判断是否为数字。
    - `Character.isLetter('a')`：判断是否为字母。
    - `Character.isUpperCase('A')`：判断是否为大写。
    - `Character.isLowerCase('A')`：判断是否为小写。
    - `Character.isWhitespace('a')`：判断是不是空格。
    - `Character.toUpperCase('a')`：转成大写。
    - `Character.toLowerCase('a')`：转成小写。

7. `Integer`创建机制
    ```java
    Integer i = new Integer(1);
    Integer j = new Integer(1);
    System.out.println(i == j); // false，对象不相同。

    Integer m = 100;
    Integer n = 100;
    // true，源码显示，调用valueOf，当数值为-128~127时，
    // 直接从cache数组中返回，视为同一对象
    System.out.println(i == j); 

    Integer x = 129;
    Integer y = 129;
    System.out.println(x == y); // false，原因同上。
    ```

### `String`
1. 简介
    - `String 对象用于保存字符串，也就是一组字符序列。
    - 字符串常量对象是用双引号括起来的字符序列。（`"你好"`、`"123"`、`"boy"`）
    - 字符串的字符使用Unicode字符编码，一个字符（无论字母汉字）占两个字节。
    - `String`是一个final类（不可被继承）。
    - `String`中有属性`private final char value[]`（常量），用于存储字符串。**（这里的不可修改，指的是value不能指向新的地址，但是单个字符的内容可以变化）**

2. 常用构造方法
    ```java
    String str1 = new String(); // 空串
    String str2 = new String(String original); // 一些常量
    String str3 = new String(char[] chr);   // char数组
    String str4 = new String(char[] chr, int startIndex, int count); // 从char数组的某一位置开始构建串
    String str5 = new String(byte[] b); // byte数组
    ```      
3. 图解
    ![java_commonClass_String_img](./img/java_commonClass_String_img.png)
    - 实现了接口`Serializable`，可以串行化（在网络传输）。
    - 实现了接口`Comparable`，可以进行比较。
4. 创建`String`对象的两种方式
    ```java
    // 方法1：直接赋值
    // 方法2：调用构造器
    ```
    - 方式1：先从常量池查看是否有目标字符串的数据空间，如果有，直接指向；如果没有，则重新创建再指向。以此法创建的字符串指向常量池的空间地址。
    - 方法2：先在堆中创建空间，里面维护了value的属性，指向常量池的地址空间（...）。对象指向的是堆中的空间地址。
    - 内存分布图
        ![java_commonClass_String_memory](./img/java_commonClass_String_memory.png)

5. 面试题错题本
    - 字符串相加
        ```java
        String a = "hello"; // 常量池变量"hello"
        String b = "abc"; // 常量池变量"abc"
        String c = "hello" + "abc"; // 由编译器底层优化，即使没有a和b，也只在常量池创建"helloabc"
        String d = a + b;   // 底层调用StringBuilder，d指向堆。
        System.out.println(c == d); // false，两者并非都指向常量池
        ```
    
#### `String`类常用方法
1. 简介
    - `String`类用于保存字符串常量，每次更新都需要重新开辟空间，效率较低。于是开发者提供了`StringBuilder`和`StringBuffer`用于增强功能提高效率。

2. 常用方法演示
    
    - `equals`： 区分大小写，判断内容是否相等。
    - `equalsIgnoreCase`：忽略大小写的判断内容是否相等。
    - `length`： 获取字符的个数，字符串的长度。
    - `indexOf`：获取字符在字符串中第1次出现的索引，索引从0开始，如果找不到，返回-1。
    - `lastIndexOf`：获取字符在字符串中最后1次出现的索引，索引从0开始，如找不到，返回-1。
    - `substring`：截取指定范围的子串。
    - `trim`：去前后空格。
    - `charAt`：获取某索引处的字符，注意不能使用Str[index] 这种方式。
    - `intern`：获得String对象在常量池中的地址。
    
    ```java
    System.out.println("hello".equals("Hello"));    // false，区分大小写
    System.out.println("hello".equalsIgnoreCase("Hello"));  // true，忽略大小写
    System.out.println("aaa@aaaa@".indexOf('@'));   // 3，取第一次出现的位置，若找不到返回-1
    System.out.println("aaa@aaaa@".lastIndexOf('@'));   // 8，取最后一次出现的位置
    System.out.println("hello,张三".substring(6));  // 张三
    System.out.println("hello,张三".substring(2,5));    // llo，左闭右开
    ```

    - `toUpperCase`：转换成大写。
    - `toLowerCase`：转换成小写。
    - `concat`：连接字符串。**（这个操作速度快于直接相加）**
    - `replace`：替换字符。对字符串中**所有**符合条件的子串执行替换。
    - `split`：分割字符串
    - `toCharArray`：字符串转换成字符数组。
    - `compareTo`：比较两个字符串。（`a.compareTo(b)`）
        - 长度相同且内容一致，返回0。
        - 进行比较时，比较到某一字符串末端前出现不相同，返回不同字符的码值差值（`a.charAt(i) - b.charAt(i)`）
        - 比较到某一字符串末尾未出现不同，返回不同字符串长度的差值。（`a.length() - b.length()`）
    

    ```java
    System.out.println("abc".toUpperCase());    // ABC
    System.out.println("Abc".toLowerCase());    // abc
    System.out.println("abc".concat("def").concat("abc"));  // abcdefabc
    System.out.println("abcdefabc".replace("abc","aha"));   // ahadefaha
    //[Ljava.lang.String;@76ccd017
    System.out.println("锄禾日当午，汗滴禾下土，谁知盘中餐，粒粒皆辛苦".split("，"));
    // [锄禾日当午, 汗滴禾下土, 谁知盘中餐, 粒粒皆辛苦]
    System.out.println(Arrays.toString("锄禾日当午，汗滴禾下土，谁知盘中餐，粒粒皆辛苦".split("，")));
    ```
    
    - `format`：格式化字符串。

    ```java
    int age = 18;
    String name = "LCQ";
    char gender = '男';
    float score = 90.566f;

    System.out.println("我的姓名是" + name + "，性别是" + gender + "，年龄是" + age + "岁，成绩是" + score + "分。");
    System.out.println(String.format("我的姓名是%s，性别是%c，年龄是%d岁，成绩是%.2f分。", name, gender, age, score));
    System.out.println();
    ```

#### `StringBuffer`
1. 源码
    ```java
     public final class StringBuffer
    extends AbstractStringBuilder
    implements Serializable, CharSequence
    ```
    - 父类是`AbstractStringBuilder`。
        - 包含非final的`char[] value`。用于存放字符串内容，不是常量，存放于堆中。
    - 实现了串行化接口。
    - `StringBuffer`是final的。
    - 使用`StringBuffer`，不用每次创建新对象，效率高于`String`。
2. 构造器
    ```java
    StringBuffer stringBuffer = new StringBuffer(); // 默认创建一个大小为16的char数组。
    StringBuffer stringBuffer = new StringBuffer(100);  // 创建一个大小为100的数组。
    StringBuffer stringBuffer = new StringBuffer("hello");  // 创建一个输入字符串+16的数组，这里实际空间是21。
    ```
3. 使用示例
    ```java
    String str1 = "hello";
    // 方法1：使用构造器
    StringBuffer stringBuffer = new StringBuffer(str1);
    // 方法2：使用append
    StringBuffer stringBuffer02 = new StringBuffer();
    stringBuffer02 = stringBuffer02.append(str1);       
    // 注意，接受返回值并非必须，接收返回值是为了链式调用（如：str.append().append()...）
    
    // StringBuffer -> String
    // 方法1：toString
    String str2 = stringBuffer.toString();
    // 方法2：构造器
    String str3 = new String(stringBuffer); 
    ```

4. 增删改查
    ```java
    // 增
    StringBuffer stringBuffer = new StringBuffer("hello");
    stringBuffer.append("lcq").append("aha");
    // 删
    stringBuffer.delete(5,8);   // 删除[5,8)范围内的字符（左闭右开）
    // 改
    stringBuffer.replace(0,5,"byebye"); // 将[0,5)替换为byebye
    // 插入
    stringBuffer.insert(6,"666");   // 将索引9往后的内容后移并插入"666"
    // 查
    System.out.println(stringBuffer.indexOf("aha"));    // 返回值为int
    ```

#### `StringBuilder`
1. 简介
    - 与`StringBuffer`一样，用于存储一个可变的字符序列，且提供与`StringBuffer`相同的API，但不保证线程安全。
    - 对于单线程场景，优先考虑`StringBuilder`，其速度更快。

#### 小结
1. `String`和`StringBuffer`/`StringBuilder`的区别
    - `StringBuffer`和`StringBuilder`都是用于存储可变字符序列的类，方法相同。
    - `String`是不可变字符序列，效率低，复用率高。（复用率高：同样的String会指向常量池同一对象）
    - `StringBuffer`是可变字符序列，效率较高，线程安全。
    - `StringBuider`是可变字符序列，效率最高，线程不安全。
2. 使用注意事项
    - 如果对一个字符串`s`进行大量修改，会极大影响系统性能。
        ```java
        String str = "abc"  // 常量池创建"abc"
        str += "d";         // 创建新的字符串对象"abcd"
        // ...
        ```
    - 字符串存在大量修改操作，且多线程：`StringBuffer`。
    - 字符串存在大量修改操作，且单线程：`StringBuilder`。
    - 字符串很少修改，被多个对象引用：`String`，如配置信息。

### `Math`
1. 常用方法
    - `abs(i)`：求绝对值。
    - `pow(i,j)`：求幂，i的j次方
    - `ceil(i)`：向上取整（大于等于i的最小整数，注意负数）
    - `floor(i)`：向下取整（小于等于i的最大整数，注意负数）。
    - `round(i)`：返回long，四舍五入，相当于`Math.floor(i+0.5)`
    - `sqrt(i)`：开方
    - `random()`：返回0~1的随机小数（0<=x<1）
    - `max(i,j)`：返回两个数中的较大值
    - `min(i,j)`：返回两个数中的较小值

### `Arrays`
1. 常用方法
    - `Arrays.toString(obj)`：数组转字符串
    - `Arrays.sort(obj)`：升序排序，由于数组是引用类型，所以排序后原数组也会改变。   
    - `Arrays.sort(obj, new Comparator<T>(){...})`：定制排序，传入一个数组和实现了Comparator接口的匿名内部类。
        ```java
        Integer[] arr = {1, 5, 3, 2, 4};
        Arrays.sort(arr, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;         // 这里结果大于0 or 小于0会影响排序逻辑
            }
        })
        ```
    - `Arrays.binarySearch(obj,i)`：对一个有序数组进行二分查找，查找目标是`i`。
    - `copyOf(obj,i)`：拷贝i个原数组元素，若i<0，抛出`NegativeArraySizeException`异常，若i>obj.length，多余的值均为null。
    - `fill(obj,i)`：数组填充，将数组内的值全替换为`i`。
    - `equals(obj1,obj2)`：比较两个数组的内容是否完全一致。
    - `asList()`：将一组值转换为List。

### `System`类
1. 常用方法
    - `exit()`：退出当前程序。
    - `arraycopy(src, srcPos, dest destPos, length)`：复制数组元素，覆盖目标数组相应位置。
        - `src`：源数组。
        - `srcPos`：开始索引。
        - `dest`：目标数组。
        - `destPos`：目标数组开始索引。
        - `length`：拷贝长度。
    - `currentTimeMillens()`：返回时间戳（距1970-1-1的毫秒数）。
    - `gc()`：垃圾回收机制。

### 大数处理方案：`BigInteger`和`BigDecimal`
1. `BigInteger`
    - 编程中，不可避免的要处理很大的数，甚至long都不够用。
    ```java
    // 创建对象时，应使用字符串。
    BigInteger bigInteger = new BigInteger("1234567890123456789012345678901234567890");
    BigInteger bigInteger2 = new BigInteger("100");

    // 加减乘除用对应的方法实现，且算子两端均是BigInteger对象。
    BigInteger add = bigInteger.add(bigInteger2);
    System.out.println(add);
    BigInteger subtract = bigInteger.subtract(bigInteger2);
    System.out.println(subtract);
    BigInteger multiply = bigInteger.multiply(bigInteger2);
    System.out.println(multiply);
    BigInteger divide = bigInteger.divide(bigInteger2);
    System.out.println(divide);
    
    ```

2. `BigDecimal`
    - 当需要保存一个精度很高的数（double的精度都不够），可用这个类。
    ```java
    BigDecimal bigDecimal = new BigDecimal("123.4567891413165314314353154135413534");
    BigDecimal bigDecimal2 = new BigDecimal("0.123456784562342592525234532523");

    // 加减乘除用对应的方法实现，且算子两端均是BigDecimal对象。
    BigDecimal result = bigDecimal.add(bigDecimal2);
    BigDecimal result2 = bigDecimal.subtract(bigDecimal2);
    BigDecimal result3 = bigDecimal.multiply(bigDecimal2);
    try{// 可能抛出ArithmeticException异常，因为可能除不尽。
        BigDecimal result4 = bigDecimal.divide(bigDecimal2);
        // BigDecimal result4 = bigDecimal.divide(bigDecimal2, BigDecimal.ROUND_CEILING); 结果保留和被除数（分子）相同的精度。   
    }catch(Exception e){
        System.out.println(e.getMessage());
    }
    System.out.println(result);
    System.out.println(result2);
    System.out.println(result3);
    System.out.println(result4);
    
    ```

### 日期类
#### 第一代日期类
1. `java.util.Date`
    - 精确到毫秒，代表特定瞬间
    - 实现了`Cloneable`、`Comparable`、`Serializable`接口
    - 大部分方法过时。
    ```java
    import java.util.Date;// 不是java.sql.Date
    // ...
    Date d1 = new Date();   // 无参构造器，获得当前系统时间
    System.out.println(d1); // 输出示例（默认）：Fri Jan 08 16:07:05 CST 2021 ，CST：中国标准时间
    // 可以对格式进行转换
    // 下列格式：2021-1-8 16:07:05 星期五
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss E");
    String date = sdf.format(d1);
    System.out.println(date);

    // 将一个格式化的String转换成Date对象
    String dateStr = "2021-1-8 16:07:05 星期五";
    Date d2 = sdf.parse(dateStr);
    System.out.println(d2);
    ```
2. `SimpleDateFormat`
    - 格式和解析日期的类
    - 字母规定：
        ![java_commonClass_dateTime_dateformat](./img/java_commonClass_dateTime_dateformat.png)

#### 第二代日期类
1. `Calendar`
    - 实现了`Cloneable`、`Comparable`、`Serializable`接口。
    - 采用单例模式。
    - `Calendar`类是一个抽象类，它为特定瞬间、一组（诸如`YEAR`、`MONTH`、`DAY_OF_MONTH`、`HOUR`等日历字段之间的转换提供了一些方法，并未操作日历字段提供了一些方法）
    ```java
    Calendar c = Calendar.getInstance();    // 单例模式以此法获得日历类对象
    System.out.println(c);                  // 查看对象格式，所有的字段
    System.out.println("年：" + c.get(Calendar.YEAR));
    System.out.println("月：" + c.get(Calendar.MONTH + 1));     // 返回的是0-11，需要+1
    System.out.println("日：" + c.get(Calendar.DAY_OF_MONTH));
    System.out.println("时：" + c.get(Calendar.HOUR_OF_DAY));   // HOUR_OF_DAY是24小时制，HOUR是12小时制
    System.out.println("分：" + c.get(Calendar.MINUTE));
    System.out.println("秒：" + c.get(Calendar.SECOND));

    // 没有专门的格式化方法，需要自己组合。
    System.out.println(c.get(Calendar.YEAR) + "年" + (c.get(Calendar.MONTH) + 1) + "月" + c.get(Calendar.DAY_OF_MONTH) + "日" + c.get(Calendar.HOUR_OF_DAY) + "时" + c.get(Calendar.MINUTE) + "分" + c.get(Calendar.SECOND) + "秒");
    ```
#### 第三代日期类
1. 前两代的不足
    - 可变性：日期和时间这样的类应该是不可变的。
    - 偏移性：`Date`的年份是从1900开始的，而月份是从0开始的。
    - 格式化：格式化只对`Date`有用，对`Calendar`无效。
    - 它们不是线程安全的，且不能处理闰秒。
2. 常用类
    从JDK8开始，引入第三代类。
    - `LocalDate`：日期
    - `LocalTime`：时间
    - `LocalDateTime`：日期和时间
3. 示例
    ```java
    LocalDateTime ldt = LocalDateTime.now();
    System.out.println(ldt);            // 输出示例：2021-05-05T08:09:09.000
    System.out.println(ldt.getYear());
    System.out.println(ldt.getMonthValue());    // 例：3
    System.out.println(ldt.getMonth());         // 例：March
    System.out.println(ldt.getDayOfMonth());    
    System.out.println(ldt.getHour());
    System.out.println(ldt.getMinute());
    System.out.println(ldt.getSecond());

    // LocalTime可以获取时、分、秒
    LocalTime lt = LocalTime.now();
    System.out.println(lt.getHour());
    System.out.println(lt.getMinute());
    System.out.println(lt.getSecond());

    // LocalDate可以获取年、月、日
    LocalDate ld = LocalDate.now();
    System.out.println(ld.getYear());
    System.out.println(ld.getMonthValue());
    System.out.println(ld.getDayOfMonth());

    // 第三代日期类的格式化
    DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
    System.out.println(dtf.format(LocalDateTime.now()));

    // 其他方法
    // 使用plus和minus调整时间。
    LocalDateTime ldt2 = ldt.plusDays(400); // 400天后
    LocalDateTime ldt3 = ldt.minusHours(10);// 10小时前

    System,out.println(LocalDateTime.now());
    System.out.println(dtf.format(LocalDateTime.now().plusDays(400).minusHours(10).plusMinutes(30)));
    ```

4. 时间戳`Instant`
    ```java
    Instant now = Instant.now();
    System.out.println(now);                
    Date date = Date.from(now);             // 时间戳转日期类
    Instant instant = date.toInstant();     // 日期类转时间戳
    ```


    
## 集合

### 章节目录
1. 集合框架体系
2. `Collection`
    - `List`
        - `ArrayList`
        - `LinkedList`
        - `Vector`
    - `Set`
        - `HashSet`
        - `LinkedHashSet`
        - `TreeSet`
3. `Map`
    - `HashMap`
    - `HashTable`
    - `LinkedHashMap`
    - `TreeMap`
    - `Properties`
4. `Collections`

### 集合基本介绍

1. 数组的缺陷
    - 长度开始时必须指定，一旦指定不能更改。
    - 保存的必须为同一类型的元素。
    - 使用数组进行增加、删除元素比较麻烦。

2. 集合的优势
    - 可以动态保存任意多个对象，使用比较方便。
    - 提供了一些列方便操作对象的方法：`add()`、`remove()`、`set()`、`get()`等。
    - 使用集合添加、删除新元素的示意代码。

### 集合框架体系
1. 【记住】`Collection`框架图 **（单列集合）**
    ![java_collection_collection_framework](./img/java_collection_collection_framework.png)
2. 【记住】`Map`框架图 **（双列集合）**
    ![java_collection_map_framework](./img/java_collection_map_framework.png)

3. 单列集合与双列集合
    - `Collection`接口有两个重要的子接口：`List`和`Set`，他们实现的子类是单列集合。
    - `Map`解耦的实现子类是双列集合，存放`Key-Value`（`K-V`，键值对）。

### `Collection`接口

#### 接口基本使用
1. 代码
    ```java
    public interface Collection<E> extends Iterable<E> {}
    ```
2. `Collection`接口实现类的特点
    - `Collection`实现子类可以存放多个元素，每一个元素可以是`Object`类型。
    - 有些`Collection`的实现类，可以存放重复的元素，有些则不可以。
    - `Collection`的实现类，有些事有序的（`List`），有些不是有序的（`Set`）。
    - `Collection`接口没有直接实现子类，而是通过子接口`Set`和`List`实现。

3. `Collection`接口的常用方法（用`ArrayList`实现类举例）
    
    - `add`：添加单个元素。
        可以在`List`尾部添加任意元素。
        **如果add的是基本数据类型，会进行自动装箱为包装类，因为索引也是int，自动装箱可以保证逻辑正确**
        ```java
        list.add("hello");
        list.add(true);
        ```
    - `remove`：删除指定元素。
        ```java
        list.remove("hello");   // 删除指定元素"hello"，return boolean，表示删除是否成功
        list.remove(0);         // 删除索引为0的元素，return Object，返回目标位置的值
        list.remove((Integer) value);// 删除值为int的元素，须为包装类
        ```
    - `contains`：查找元素是否存在。
        ```java
        System.out.println(list.contains("hello"));  // return boolean，表示是否存在
        ```
    - `size`：获取元素个数。
        ```java
        System.out.println(list.size());  // return int，表示元素个数
        ```
    - `isEmpty`：判断是否为空。
        ```java
        System.out.println(list.isEmpty());  // return boolean，表示是否为空
        ```
    - `clear`：清空。
        ```java
        list.clear();
        System.out.println(list);               // []
        System.out.println(list.size());        // 0
        System.out.println(list.isEmpty());     // true
        ```
    - `addAll`：添加多个元素。
        ```java
        ArrayList list2 = new ArrayList();
        list2.add("hello");
        list2.add("world");
        list.addAll(list2);
        ```
    - `containsAll`：查找多个元素是否都存在。
        ```java
        System.out.println(list.containsAll(list2));   // boolean
        ```
    - `removeAll`：删除多个元素。
        ```java
        list.removeAll(list2);
        System.out.println(list);      
        ```

#### 迭代器遍历
1. 介绍
    `Collection`接口遍历元素方式1：使用`Iterator`迭代器。
    - `iterator`对象称为迭代器，用于遍历`Collection`集合中的元素。
    - 所有实现了`Collection`接口的集合类都有一个`iterator()`方法，用以返回一个实现了`Iterator`接口的对象（即一个迭代器）。
    - Iterator结构
        ![]()
    - `Iterator`仅用于遍历对象，其本身不存放对象。
    - `Iterable`就是`Collection`的父类。
2. 迭代器执行原理
    ```java
    Iterator iterator = <collection及其子类对象>.iterator(); 
    while(iterator.hasNext()){          // 当迭代器中还有元素
        Object obj = iterator.next();   // 获得下一个迭代器中的元素
        //System.out.println(obj);
    }
    ```
    - 调用`iterator()`方法前，必须进行`hasNext()`判断，否则会报`NoSuchElementException`异常。
    - 当迭代完成后，迭代器的指针已经指到最后的元素位置，如果需要重新使用迭代器，须重新执行以下代码：
        ```java
        Iterator iterator = <collection及其子类对象>.iterator(); 
        ```

#### 增强for循环

1. 介绍
    增强for循环可以代替iterator迭代器，相当于一个简化版的Iterator，**只能遍历集合或数组**
2. 基本语法
    ```java
    for(数据类型 变量名 : 集合或数组){ 
        //循环体，在此访问迭代的元素。
        Object obj = 变量名;
        System.out.println(obj);
    }
    ```
    - 增强for的底层还是迭代器。

#### List接口方法

1. 基本介绍
    - `List`接口是`Collection`接口的子接口。
    - `List`集合类中元素有序且可重复。（添加与取出顺序一致）
    - `List`集合中的每一个元素都有其对应的索引，索引从0开始。（支持索引）
    - `List`容器中的元素都都对应一个整数型的序号记载其在容器中的位置，可以根据序号存取容器中的元素。
    - `JDK API`中`List`接口实现类有：
    `AbstractList`、`AbstractSequentialList`、`ArrayList`、`AttributeList`、`CopyOnWriteArrayList`、`LinkedList`、`RoleList`、`RoleUnresolvedList`、`Stack`、`Vector`。
    常用的有：`ArrayList`、`LinkedList`、`Vector`等。

2. List接口常用方法
    - `void add(int index, Object ele)`：在 index 位置插入 ele 元素
    - `boolean addAll(int index, Collection eles)`：从 index 位置开始将 eles 中的所有元素添加进来
    - `Object get(int index)`：获取指定 index 位置的元素
    - `int indexOf(Object obj)`：返回 obj 在集合中首次出现的位置
    - `int lastIndexOf(Object obj)`：返回 obj 在当前集合中末次出现的位置
    - `Object remove(int index)`：移除指定 index 位置的元素，并返回此元素
    - `Object set(int index, Object ele)`：设置指定 index 位置的元素为 ele , 相当于是替换. list.set(1, "玛丽");（`index`<`ele.length`）
    - `List subList(int fromIndex, int toIndex)`：返回从 `fromIndex` 到 `toIndex` 位置的子集合（注意返回的子集合 `fromIndex` <= `subList` < `toIndex`，左闭右开）

3. `List`3种遍历方法
    ```java
    List list = new ArrayList();
    // 三种任选其一即可运行
    // List list = new Vector();
    // List list = new LinkedList();
    
    // 第1种：iterator
    Iterator iterator = list.iterator();
    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }
    // 第2种：增强for
    for (Object o : list) {
        System.out.println(o);
    }
    // 第3种：普通for
    for (int i = 0; i < list.size(); i++) {
        System.out.println(list.get(i));
    }
    ```

#### `ArrayList`源码解析

1. 结论
    - `ArrayList`中维护的是一个`Object`类的数组`elementData`。
    - 当创建一个`ArrayList`时，如果使用的是无参构造器，则初始`elementData`大小为0，第1次添加元素时，`elementData`大小扩容到10，再次到上限后，扩容至`1.5*elementData.length`。
    - 如果使用的是指定大小的构造器，初始`elementData`大小为指定大小，再次添加元素时，`elementData`大小扩容至`1.5*elementData.length`。
2. 图解
    ![java_collection_arrayList_source1](./img/java_collection_arrayList_source1.png)
    ![java_collection_arrayList_source2](./img/java_collection_arrayList_source2.png)
3. 源码注释
    - 执行代码
        ```java
        public class ListGrow01 {
            public static void main(String[] args) {
                List list = new ArrayList();
                // List list = new Vector();
                // List list = new LinkedList();

                for (int i = 0; i < 10; i++) {
                    list.add("hello" + i);
                }
                list.add("world");
                System.out.println(list);

            }
        }
        ```
    - 源码注释
        ```java
        // ArrayList() 无参构造器，创建一个空的ArrayList对象
        // private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
        public ArrayList() {
            this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
        }

        // add() 方法
        public boolean add(E e) {
            // ensureCapacityInternal() 方法用于确保数组不越界
            ensureCapacityInternal(size + 1);  // Increments modCount!!
            elementData[size++] = e;
            return true;
        }

        // ensureCapacityInternal() 方法
        private void ensureCapacityInternal(int minCapacity) {
            // calculateCapacity() 方法确定扩容策略
            // ensureExplicitCapacity() 方法确认是否真的需要扩容并执行
            ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
        }

        private static int calculateCapacity(Object[] elementData, int minCapacity) {
            if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) { 
                // 当elementData为空数组时，返回请求大小和默认大小中的最大值
                // 其中，DEFAULT_CAPACITY = 10
                // 这是为了应对addAll()方法可能一次性添加超过10个数据。
                return Math.max(DEFAULT_CAPACITY, minCapacity);
            }
            return minCapacity;
        }

        private void ensureExplicitCapacity(int minCapacity) {
            modCount++;         // 记录操作次数

            // overflow-conscious code
            if (minCapacity - elementData.length > 0)
                // 当确实容量不足时，执行扩容操作，大小为calculateCapacity()方法返回的大小
                grow(minCapacity);
        }

        private void grow(int minCapacity) {
            // overflow-conscious code
            int oldCapacity = elementData.length;                       // 获取当前容量
            int newCapacity = oldCapacity + (oldCapacity >> 1);         // 初始化新容量，默认为原容量的1.5倍
            if (newCapacity - minCapacity < 0)
                // 如果初始化的newCapacity达不到要求，则将minCapacity直接传入。                      
                newCapacity = minCapacity;
            if (newCapacity - MAX_ARRAY_SIZE > 0)
                // 检查新容量是否超出了数组最大长度限制
                newCapacity = hugeCapacity(minCapacity);
            // 使用Arrays.copyOf()方法将原数组复制到新数组中
            // `copyOf(obj,i)`：拷贝i个原数组元素，若i<0，抛出`NegativeArraySizeException`异常，若i>obj.length，多余的值均为null。
            // minCapacity is usually close to size, so this is a win:
            elementData = Arrays.copyOf(elementData, newCapacity);
        }
        ```

#### `Vector`类

1. 基本介绍
    - 继承了`AbstractList`。
        ```java
        public class Vector<E>
            extends AbstractList<E>
            implements List<E>, RandomAccess, Cloneable, java.io.Serializable
        ```
    - `Vector`底层维护的是一个`protected Object[] elementData`。
    - `Vector`时线程同步（线程安全）的，`Vector`类的操作带有`synchronized`。
    - 在开发中，需要线程同步安全时，用到`Vector`。

2. `Vector`扩容机制

    |类|`ArrayList`|`Vector`|
    |:---:|:---|:---|
    |底层结构|可变数组`Object[]`|可变数组|
    |版本|jdk1.2|jdk1.0|
    |线程安全（同步）效率|线程不安全<br>效率高|线程安全<br>效率低|
    |扩容倍数|有参构造器：按1.5倍扩容；<br>无参构造器：<br>第一次默认为10，<br>第二次开始按1.5倍扩容。|有参构造器：按2倍扩容；<br>无参构造器：<br>第一次默认为10，<br>第二次开始按2倍扩容。|

3. `Vector`源码分析
    ```java
    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;   // 获取当前数组长度
        // 根据扩容策略初始化新数组长度，与ArrayList不同，Vector的策略为双倍扩容。
        // 其中，capacityIncrement是一个须显示指定的扩容策略，默认为0，即默认双倍扩容
        int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                        capacityIncrement : oldCapacity);
        if (newCapacity - minCapacity < 0)
            // 初始值不满足需求时，将需求量作为新数组容量
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            // 检测是否超出了数组最大值
            newCapacity = hugeCapacity(minCapacity);
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
    ```

4. 三种`Vector`构造器
    - `Vector()`：无参构造器，初始化容量为`10`。
        ```java
        public Vector() {
            this(10);
        }
        ```
    - `Vector(int initialCapacity)`：指定初始容量的构造器。
        ```java
        public Vector(int initialCapacity) {
            this(initialCapacity, 0);
        }
        ```
    - `Vector(int initialCapacity, int capacityIncrement)`：指定初始容量和容量增量的构造器。
        ```java
        public Vector(int initialCapacity, int capacityIncrement) {
            super();
            if (initialCapacity < 0)
                throw new IllegalArgumentException("Illegal Capacity: "+
                                                initialCapacity);
            this.elementData = new Object[initialCapacity];
            this.capacityIncrement = capacityIncrement;
        }
        ```

#### `LinkedList`

1. 说明
    - `LinkedList`底层实现了双向链表和双端队列的特点。
    - 可以添加任意元素（元素可以重复），包括`null`。
    - 线程不安全。

2. `LinkedList`底层操作机制。
    - `LinkedList`底层维护了一个双向链表。
    - `LinkedList`中维护了两个属性：`first`和`last`，分别指向首、尾节点。
    - 每个节点（Node对象，是一个内部类），里面维护了`prev`、`next`、`item`三个属性，通过`prev`指向前一个节点，通过`next`指向后一个节点，最终实现双向链表。
    - `LinkedList`的元素的添加和删除，不是通过数组完成的，相对来说效率较高。
3. 【例程】模拟一个简单的双向链表
    ```java
    public class LinkedList01 {

        public static void main(String[] args) {
            Node a = new Node("a");
            Node b = new Node("b");
            Node c = new Node("c");

            // a <-> b <-> c
            a.next = b;
            b.next = c;
            c.prev = b;
            b.prev = a;

            Node first = a;
            Node last = c;

            while (first != null) {
                System.out.println(first);
                first = first.next;
            }
        }

        static class Node {
            public Object item;
            public Node next;
            public Node prev;

            public Node(Object item) {
                this.item = item;
            }

            @Override
            public String toString() {
                return "Node{" +
                        "item=" + item +
                        '}';
            }
        }
    }   
    ```

4. `LinkedList`增删改查
    ```java
    LinkedList list = new LinkedList();
    for (int i = 0; i < 10; i++) {
        list.add(i);                // 在尾部增加元素
    }
    System.out.println(list);

    list.remove();                  // 默认移除第一个元素
    list.remove(6);           // 指定移除的元素
    System.out.println(list);

    list.set(0, "set1");            // 在指定位置改变元素
    list.set(7, "set2");
    System.out.println(list);


    System.out.println(list.get(0));
    System.out.println(list.indexOf("set1"));
    ```

5. `LinkedList`底层分析
    - `add()`
        ```java
        // 无参构造器不会执行任何操作
        public LinkedList() {
        }

        // add()方法直接调用linkLast()
        public boolean add(E e) {
            linkLast(e);
            return true;
        }

        void linkLast(E e) {
            final Node<E> l = last; // 创建l指向LinkedList.last
            final Node<E> newNode = new Node<>(l, e, null); // 创建新节点，pre指向last节点，last置空
            last = newNode;         // 将新节点设置为新的last
            if (l == null)
                first = newNode;    // 当LinkedList为空时（first == null），将首节点指向新节点
            else
                l.next = newNode;   // 否则，将l.next设置为新节点
            // 调整List大小与操作次数
            size++;
            modCount++;
        }
        ```
    - `remove()`
        ```java
        public E remove() {
            return removeFirst();       // 默认调用removeFirst()删除首节点
        }
        
        public E removeFirst() {
            final Node<E> f = first;
            if (f == null)
                // 如果首节点为空，抛出异常
                throw new NoSuchElementException();
            return unlinkFirst(f);      // 调用unlinkFirst()
        }

        private E unlinkFirst(Node<E> f) {
            // 提取首节点信息
            // assert f == first && f != null;
            final E element = f.item;
            final Node<E> next = f.next;
            
            // 释放首节点资源
            f.item = null;
            f.next = null; // help GC

            // 改变首节点指向，并处理两种可能情况
            first = next;
            if (next == null)
                last = null;
            else
                next.prev = null;

            // 操作次数+1，大小-1
            size--;
            modCount++;

            // 返回删除的元素
            return element;
        }
        ```

#### `List`集合选择

1. `ArrayList`和`LinkedList`比较
    ||`ArrayList`|`LinkedList`|
    |:---:|:---:|:---:|
    |底层结构|可变数组|双向链表|
    |增删效率|较低，采用数组扩容机制|较高，采用链表机制|
    |改查的效率|较高|较低|

2. 两者的选择
    - 如果改查的操作多，选择`ArrayList`。
    - 如果增删的操作多，选择`LinkedList`.
    - 一般来说，在程序中，80%~90%都是查询，因此大部分情况下选择`ArrayList`。
    - 在一个项目中，根据业务灵活选择，不同的模块可以使用不同的集合类。
    - 值得注意的是：**两者都是线程不安全的。**

#### `Set`接口方法

1. 基本介绍
    - `Set`接口是无序的，添加和取出的顺序不一致，没有索引。
    - 不允许重复元素，因此最多包含一个`null`。
    - `JDK`API 中`Set`接口的实现类有：
        `AbstractSet`、`ConcurrentSkipListSet`、`CopyOnWriteArraySet`、`EnumSet`、`HashSet`、`LinkedHashSet`、`TreeSet`、`JobStateReasons`。

2. `Set`接口常用方法
    `Set`接口是`Collection`子接口，常用方法和`Collection`相同。

3. `Set`接口的遍历方式
    - 可以使用迭代器。
    - 可以使用增强for循环。
    - 不能使用索引。
4. `Set`接口的常用方法（以`HashSet`为例）
    ```java
    Set set = new HashSet();
    System.out.println(set.add(1)); // return boolean
    set.add(3);
    set.add(null);
    set.add(2);
    set.add(1);                     // return False，因为对象已存在

    // [null, 1, 2, 3]
    // 添加和取出顺序不一致，但每次运行不会出现不同的输出
    System.out.println(set);
    ```

#### `HashSet`

1. 说明
    - `HashSet`实现了`Set`接口。
    - `HashSet`实际上是`HashMap`。（`HashMap`底层是数组+链表+红黑树）
    - `HashSet`可以存放`null`，但只能存放一个。
    - `HashSet`不保证元素是有序的，取决于hash后，再确定索引的结果。
    - 不能有重复元素/对象。


2. 【例程】简单的数组+链表结构
    - 这里的数组是`Node[]`，链表是`Node[i]`。
    - 使用链表的原因是为了存储更加高效（高于数组），当节点数组中某一索引达到一定长度时，选择使用树结构，这是一种比链表效率更高的数据结构。
    ```java
    public class HashSetStructure {
        public static void main(String[] args) {
            // 创建一个数组，类型为Node[]。
            // Node[]被某些人称为“表”
            Node[] table = new Node[16];

            // 在index == 2 处设置 "john -> jack -> rose"
            Node john = new Node("john", null);
            table[2] = john;
            Node jack = new Node("jack", null);
            john.next = jack;
            Node rose = new Node("rose", null);
            jack.next = rose;
            System.out.println(table);
            
            // 在index == 3 设置"lucy"
            Node lucy = new Node("lucy", null);
            table[3] = lucy;
        }
        static class Node {
            Object item;
            Node next;

            public Node(Object item, Node next) {
                this.item = item;
                this.next = next;
            }
        }
    }
    ```

3. `HashSet`底层机制
    分析`HashSet`的添加元素底层是如何实现的（`hash()`+`equals()`）
    步骤：
    - `HashSet`底层是`HashMap`。
    - 添加一个元素时，先得到hash值，然后将hash值转成索引。
    - 找到存储数据表table，看这个索引是否已经存放有元素。
    - 如果没有，直接加入。
    - 如果有，调用`equals()`方法 **（这里可以由程序员控制）**，看两个元素是否相等。如果相同，放弃添加，如果不相同，添加到最后。
    - 在java8中，如果一条链表的元素个数超过`TREEIFY_THRESHOLD`（默认值是8），且table大小>=`MIN_TREEIFY_CAPACITY`（默认值是64），就会进行树化。（红黑树）

4. 扩容机制
    ```java
    HashSet set = new HashSet();
    set.add("java");
    set.add("cpp");
    set.add("java");
    System.out.println(set);
    ```
    - `HashSet()`
        ```java
        public HashSet() {
            map = new HashMap<>();          // 底层是HashMap
        }

        public HashMap() {
            this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
        }        
        ```
    
    - `set.add()`
        ```java
        public boolean add(E e) {
            // private static final Object PRESENT = new Object();
            // put()会在键存在时替换返回当前值（这里是PRESENT），键不存在时返回null
            // 因此，返回值为：true表示添加成功，false表示添加失败
            return map.put(e, PRESENT)==null;
        }

        public V put(K key, V value) {
            return putVal(hash(key), key, value, false, true);
        }

        static final int hash(Object key) {
            int h;
            // 按位异或和算术右移16位：可以增加哈希值的随机性，减少碰撞概率。
            // **这里只需要知道哈希值并非简单的hashCode()即可，其还经过了一些处理
            return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
        }

        final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {
            // 定义辅助变量，Node数组 tab、节点 p、数组长度 n、索引 i
            Node<K,V>[] tab; Node<K,V> p; int n, i;
            // Node[] table
            // 第一次扩容触发，将初始大小设置为16
            // 将resize()方法返回的table赋给tab，并将大小赋给n
            if ((tab = table) == null || (n = tab.length) == 0)
                n = (tab = resize()).length;
            // 我的理解：用(1111)与hash(key)进行按位与运算，得到索引i=[0,15]
            // 当table扩容时，(1111)也会变成(11111)，以此类推
            // 这里的p是当前索引位置对应链表的第一个Node
            // 如果当前位置为null，触发节点创建
            if ((p = tab[i = (n - 1) & hash]) == null)
                tab[i] = newNode(hash, key, value, null);
            else {
                // 当索引位置有值时，分三种情况：
                // 1. 新对象与索引位置首节点值相同。
                // 2. 不满足1，但首节点是一个TreeNode（红黑树）。
                // 3. 其他。

                // 开发技巧：定义变量时，不建议将所有变量都在程序/函数开头定义好，而是在
                // 对应代码块中定义，控制变量的生命周期。
                Node<K,V> e; K k;
                // 先判断哈希值是否相等，如果相等，考虑哈希碰撞，判断地址和值是否相等。
                // 这是降低成本的一种短路设计，哈希不相等，对象必然不是一个，地址相等值一定相等（equals()一定为true）
                if (p.hash == hash && ((k = p.key) == key || (key != null && key.equals(k))))
                    e = p;

                // 【待完善】红黑树查找算法，先按下不讲。
                else if (p instanceof TreeNode)
                    e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);

                // 其他情况
                else {
                    for (int binCount = 0; ; ++binCount) {
                        // 当匹配到最后仍不存在，创建一个Node并将节点放到末尾。
                        if ((e = p.next) == null) {
                            p.next = newNode(hash, key, value, null);
                            // 【待完善】如果链表大于等于7，执行树化
                            // 树化还必须满足table.length >= 64（会在treeifyBin()中判断）
                            // 也就是说，会优先将Node[] table的大小扩容到64
                            if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                                treeifyBin(tab, hash);
                            break;
                        }
                        // 对下一节点进行判断是否与待添加数据相等，逻辑与第一条分支相同
                        if (e.hash == hash && ((k = e.key) == key || (key != null && key.equals(k))))
                            break;
                        p = e;      // 更新p来执行新的p.next()
                    }
                }

                // 下面的代码会在添加失败时执行（即键匹配成功时）
                if (e != null) { // existing mapping for key
                    V oldValue = e.value;   // 获取旧值
                    if (!onlyIfAbsent || oldValue == null)
                        e.value = value;
                    afterNodeAccess(e);
                    // 输出 oldValue 表示添加失败。
                    return oldValue;
                }
            }
            ++modCount;         // 操作次数+1
            // 如果大小触及门槛，触发扩容
            if (++size > threshold)
                resize();

            // aafterNodeInsertion(true);这个方法为空，用来为HashMap子类留空，比如LinkedHashMap
            afterNodeInsertion(evict);
            // return null ，表示成功
            return null;
        }

        final Node<K,V>[] resize() {
            Node<K,V>[] oldTab = table;
            // 当HashMap为空时，oldCap = 0；否则等于table的大小
            int oldCap = (oldTab == null) ? 0 : oldTab.length;            
            int oldThr = threshold;         // threshold , 扩容临界值，当达到这个大小，就要准备扩容
            int newCap, newThr = 0;
            if (oldCap > 0) {
                if (oldCap >= MAXIMUM_CAPACITY) {
                    threshold = Integer.MAX_VALUE;
                    return oldTab;
                }
                else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                        oldCap >= DEFAULT_INITIAL_CAPACITY)
                    newThr = oldThr << 1; // double threshold
            }
            else if (oldThr > 0) // initial capacity was placed in threshold
                newCap = oldThr;
            else {               // zero initial threshold signifies using defaults
                // 初始化容量
                newCap = DEFAULT_INITIAL_CAPACITY;      // DEFAULT_INITIAL_CAPACITY = 1 << 4 ，即16，这样写是因为移位运算更快。
                newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
            }
            if (newThr == 0) {
                float ft = (float)newCap * loadFactor;
                newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                        (int)ft : Integer.MAX_VALUE);
            }
            threshold = newThr;
            @SuppressWarnings({"rawtypes","unchecked"})
            Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
            table = newTab;
            if (oldTab != null) {
                for (int j = 0; j < oldCap; ++j) {
                    Node<K,V> e;
                    if ((e = oldTab[j]) != null) {
                        oldTab[j] = null;
                        if (e.next == null)
                            newTab[e.hash & (newCap - 1)] = e;
                        else if (e instanceof TreeNode)
                            ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                        else { // preserve order
                            Node<K,V> loHead = null, loTail = null;
                            Node<K,V> hiHead = null, hiTail = null;
                            Node<K,V> next;
                            do {
                                next = e.next;
                                if ((e.hash & oldCap) == 0) {
                                    if (loTail == null)
                                        loHead = e;
                                    else
                                        loTail.next = e;
                                    loTail = e;
                                }
                                else {
                                    if (hiTail == null)
                                        hiHead = e;
                                    else
                                        hiTail.next = e;
                                    hiTail = e;
                                }
                            } while ((e = next) != null);
                            if (loTail != null) {
                                loTail.next = null;
                                newTab[j] = loHead;
                            }
                            if (hiTail != null) {
                                hiTail.next = null;
                                newTab[j + oldCap] = hiHead;
                            }
                        }
                    }
                }
            }
            return newTab;
        }
        ```

5. `HashSet`扩容与转红黑树
    - `HashSet`底层是`HashMap`，第一次添加时，table数组扩容到16，临界值是`默认值（16）*加载因子（0.75）=12`。
    - 如果`table`数组使用到了临界值，会扩容到`16*2=32`（两倍扩容），新的临界值就是`32*0.75=24`，以此类推。
    - 在Java8中，如果一条链表的元素个数达到`TREEIFY_THRESHOLD`（默认值8）且`table.length >= MIN_TREEIFY_CAPACITY`（默认值64），那么就会进行树化（红黑树），否则依然采用数组扩容机制。

6. 自定义`equals()`和`hashCode()`
    - 在IDEA的快捷生成中，选择生成`equals()和hashCode()`方法，可以进入生成向导。
    - `Objects.hash`底层调用的`Arrays.hashCode()`方法。
    ```java
    public class HashSetExercise {
        public static void main(String[] args) {
            Set set = new HashSet();
            set.add(new Employee("aha" , 22));
            set.add(new Employee("aha" , 23));
            set.add(new Employee("bhb" , 24));
            set.add(new Employee("aha" , 22));
            System.out.println(set.size());
            System.out.println(set);
        }
    }

    class Employee{
        private String name;
        private int age;

        public Employee(String name, int age) {
            this.name = name;
            this.age = age;
        }

        @Override
        public String toString() {
            return "Employee{" +
                    "name='" + name + '\'' +
                    ", age=" + age +
                    '}';
        }

        @Override
        public boolean equals(Object o) {
            if (!(o instanceof Employee)) return false;
            Employee employee = (Employee) o;
            return age == employee.age && Objects.equals(name, employee.name);
        }

        @Override
        public int hashCode() {
            return Objects.hash(name, age);
        }
    }
    ```

#### `LinkedHashSet`

1. 说明
    - `LinkedHashSet`是`HashSet`的子类。
        ```java
        public class LinkedHashSet<E>
            extends HashSet<E>
            implements Set<E>, Cloneable, java.io.Serializable
        ```
    - `LinkedHashSet`底层是一个`LinkedHashMap`，底层维护了一个数组+双向链表。
    - `LinkedHashSet`根据元素的`hashCode()`值决定元素的存储位置，同时使用链表维护元素的持续，这使得元素看起来是以插入顺序保存的。
        ![java_collection_set_linkedHashSet1](./img/java_collection_set_linkedHashSet1.png)
    - `LinkedHashSet`的元素是唯一的，即不能有重复的元素。

2. `LinkedHashSet`底层

3. 源码解读
    - 第一次创建时，table扩容至16，存放的节点类型是`LinkedHashMap$Entry`。
    - 数组是`HashMap$Node[]`类型，数组中存放的数据是`LinkedHashMap$Entry`。
        ```java
        // 和Node一样，Entre是一个静态内部类
        static class Entry<K,V> extends HashMap.Node<K,V> {
            Entry<K,V> before, after;   // 相比Node，增加了保证顺序的链表结构
            Entry(int hash, K key, V value, Node<K,V> next) {
                super(hash, key, value, next);
            }
        }
        ```

### `Map` 接口

#### 接口的基本使用

1. `Map`接口实现类的特点
    - 这里讲的是JDK8的Map接口
    - `Map`与`Collection`并列存在，用于保存具有映射关系的数据`Key-Value`。
    - `Map`中的`key`和`value`可以是任何引用类型数据，会封装到`HashMap$Node`对象中。
    - `Map`中的`key`不能重复，原因与`HashSet`相同，之前章节有对应源码。
    - `Map`中的`value`可以重复。
    - `Map`中的`key`和`value`均可以是`null`，但`key`的`null`只能有一个，`value`的`null`可以有多个。
    - 常用`String`类作为`Map`的`key`。
    - `key`和`value`存在单向一对一关系，即通过指定的`key`总能找到对应的`value`。

2. `key-value`底层
    - k-v最后是`HashMap$Node node = newNode(hash, key, value, null);`
    - 为方便程序员遍历，还会创建EntrySet集合，该集合存放Entry元素，一个Entry中含有k、v。`EntrySet<Entry<K,V>>`
    - 但由于`HashMap$Node`实现了`Map.Entry`接口，所以`HashMap$Node`对象可以存放进EntrySet集合中。
    - 方便遍历的原因：`Map.Entry`接口提供了两个方法：`K getKey()`、`V getValue()`。
        ```java
        for (Object obj : set){
            Map.Entry entry = (Map.Entry) obj;
            System.out.println(entry.getKey() + ":" + entry.getValue());
        }
        ```
    - 常用内部类
        - 总的来说，`HashMap.entry()`返回返回类型为`HashMap.EntrySet`的键值对集合。
        - `HashMap.keySet()`返回类型为`HashMap.KeySet`的键集合。
        - `HashMap.values()`返回类型为`HashMap.Values`的值集合。
        ```java
        public class HashMap01 {
            public static void main(String[] args) {
                HashMap map = new HashMap();
                map.put("key", "value");
                map.put("key2", "value2");
                map.put("key3", "value");
                map.put("key4", "value4");
                for (Object object : map.entrySet()) {
                    HashMap.Entry entry = (HashMap.Entry) object;
                    System.out.println(entry.getKey() + " " + entry.getValue());
                }

                Set set = map.keySet();
                System.out.println(set.getClass());
                System.out.println(set);
                System.out.println(map.values());
            }
        }
        ```

3. `Map`接口和常用方法
    - `put(K key, V value)`：添加键值对，键不在则返回`null`，键在则返回旧值。
    - `remove()`
        - `remove(K key)`：删除指定键的键值对，返回旧值。
        - `remove(K key, V value)`：删除指定键值对，返回boolean。
    - `get(K key)`：返回键对应的值。
    - `size()`：返回键值对数量。
    - `isEmpty()`：判断是否为空。
    - `clear()`：清空所有键值对。
    - `containsKey()`：判断键是否存在。

#### `Map`六大遍历方法

1. 遍历涉及的方法
    - `containsKey()`：查找键是否存在。
    - `keySet()`：返回所有键。
    - `entrySet()`：键值对集合。
    - `values()`：返回所有值。

2. 第一组：通过key遍历
    ```java
    // 增强for
    for (K key : map.keySet()) {
        System.out.println(key + ":" + map.get(key));
    }

    // 迭代器
    Set set = map.keySet();
    System.out.println(set.getClass());
    System.out.println(set);
    System.out.println(map.values());
    
    Iterator iterator = set.iterator();
    while (iterator.hasNext()) {
        Object next =  iterator.next();
        System.out.println(next + "-" + map.get(next));        
    }
    ```

3. 第二组：取出values
    ```java
    Collection values = map.values();
    // 增强for，迭代器
    ```

3. 第三组：`EntrySet`
    ```java
    Set entrySet = map.entrySet();
    // 增强for
    for (Object entry : entrySet) {
        Map.Entry entry1 = (Map.Entry) entry;
        System.out.println(entry1.getKey() + "-" + entry1.getValue());
    }

    // 迭代器
    Iterator iterator = entrySet.iterator();
    while (iterator.hasNext()) {
        Object entry = iterator.next();
        Map.Entry entry1 = (Map.Entry) entry;
        System.out.println(entry1.getKey() + "-" + entry1.getValue());
    }
    ```

#### `HashMap`阶段小结

1. 小结
    - `Map`接口的常用实现类：`HashMap`、`HashTable`、`Properties`。
    - `HashMap`是`Map`接口使用频率最高的实现类。
    - `HashMap`是以`key-value`的方式来存储数据的。
    - key值不能重复，但是值可以，允许使用`null`键和`null`值。
    - 如果添加相同的key，则会覆盖原来的key-value，相当于修改。
    - 和`HashSet`一样，不保证映射的顺序。
    - `HashMap`，没有实现同步，是线程不安全的。

#### `HashMap`底层源码

1. 扩容机制（与`HashSet`一致）
    - `HashMap`底层维护了`Node[] table`，默认为`null`。
    - 当创建对象时，将加载因子`laoadFactor`设置为0.75。
    - 当添加key-val时，通过key的哈希值得到在table的索引，并判断索引处是否有元素，如果没有元素直接添加。如果该索引处有元素，继续判断该元素的key和添加的key是否相等，如果相等则覆盖，不相等则判断是树结构还是链表结构，作出相应处理，如果添加时容量不够，则需要扩容。
    - 第一次添加，需要扩容table容量为16，临界值`threshold = 12`。
    - 之后扩容，table容量和threshold均为原来的两倍。
    - 在JAVA8中，如果一条链表的元素个数超过`TREEIFY_THRESHOLD`（默认值8）时，且`table.length >= MIN_TREEIFY_CAPACITY`（默认值64）时，链表会转换成红黑树。

2. 源码
    ```java
    public class HashMapSource01 {
        public static void main(String[] args) {
            Map map = new HashMap();
            map.put("java", 10);
            map.put("php", 10);
            map.put("java", 20);

            System.out.println(map);
        }
    }

    public HashMap() {
        // 初始化加载因子
        // DEFAULT_LOAD_FACTOR = 0.75f;
        this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
    }

    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }

    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }

    final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
        int oldThr = threshold;
        int newCap, newThr = 0;
        if (oldCap > 0) {
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
        }
        else if (oldThr > 0) // initial capacity was placed in threshold
            newCap = oldThr;
        else {               // zero initial threshold signifies using defaults
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        if (newThr == 0) {
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab;
        if (oldTab != null) {
            for (int j = 0; j < oldCap; ++j) {
                Node<K,V> e;
                if ((e = oldTab[j]) != null) {
                    oldTab[j] = null;
                    if (e.next == null)
                        newTab[e.hash & (newCap - 1)] = e;
                    else if (e instanceof TreeNode)
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // preserve order
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                        do {
                            next = e.next;
                            if ((e.hash & oldCap) == 0) {
                                if (loTail == null)
                                    loHead = e;
                                else
                                    loTail.next = e;
                                loTail = e;
                            }
                            else {
                                if (hiTail == null)
                                    hiHead = e;
                                else
                                    hiTail.next = e;
                                hiTail = e;
                            }
                        } while ((e = next) != null);
                        if (loTail != null) {
                            loTail.next = null;
                            newTab[j] = loHead;
                        }
                        if (hiTail != null) {
                            hiTail.next = null;
                            newTab[j + oldCap] = hiHead;
                        }
                    }
                }
            }
        }
        return newTab;
    }
    ```

#### `Hashtable`

1. 基本介绍
    - `Hashtable`存放的是键值对`K-V`。
    - `Hashtable`的键和值都不能为空，否则会抛出空指针异常`NullPointerException`。
    - `Hashtable`使用方法和`HashMap`一样。
    - **`Hashtable`是线程安全的（`synchronized`），`HashMap`是线程不安全的。**

2. 继承关系
    ```java
    public class Hashtable<K,V>
        extends Dictionary<K,V>
        implements Map<K,V>, Cloneable, java.io.Serializable 
    ```

3. `Hashtable`底层
    - 底层有数组`Hashtable$Entry[]`，初始化大小为11。
        ```java
        public Hashtable() {
                this(11, 0.75f);
            }
        ```
    - `Hashtable`的扩容机制：到达`threshold`后，扩容为原来的2倍+1。
        ```java
        int newCapacity = (oldCapacity << 1) + 1;
        ```

#### `Properties`

1. 基本介绍
    - `Properties`类，继承自`Hashtable`类，并实现`Map`接口。以键值对形式保存数据。
    - 使用特点与`Hashtable`类似，比如不能有空键与空值。
    - `Properties`类可用于从`*.properties`文件中加载数据到`Properties`对象中，并进行读取和修改。
    - 工作中，`*.properties`文件通常作为配置文件，这个知识点在IO流举例。
    - 相关文章：[旭东的博客：Java 读写Properties配置文件](https://www.cnblogs.com/xudong-bupt/p/3758136.html)

#### `TreeSet`

1. 无参构造器
    - 会调用传入对象的`CompareTo()`方法进行排序，如果对象未实现`Comparable`接口，则抛出`ClassCastException`异常。
    ```java
    TreeSet set = new TreeSet();
    set.add("jack");
    set.add("tom");
    set.add("sp");
    set.add("a");

    System.out.println(set);
    ```

2. 传入比较器的构造器
    - 传入一个类型为`Comparator`的匿名内部类。
    - 底层会调用`TreeMap`。
    - 在存入元素时，通过调用比较器确定元素的位置，如果比较器返回0，会重新设置值。
    - 不同的`Comparator`策略会影响排序和加入元素的结果。（比如按字符串长度排序，将无法加入长度相同的字符串）
    ```java
    TreeSet set = new TreeSet(new Comparator() {
        @Override
        public int compare(Object o1, Object o2) {
            return ((String)o1).compareTo((String)o2);
        }
    });
    set.add("jack");
    set.add("tom");
    set.add("jack");
    set.add("sp");
    set.add("aha");

    System.out.println(set);
    ```

3. 【待完善】等我有空了回来追一遍源码。

#### `TreeMap`

1. 无参构造器
    - 和`TreeSet`一样，使用无参构造器时，键不会排序。



### 集合选型规则
1. 先判断存储类型（一组对象-单列；一组键值对-双列）

2. 一组对象：`Collection`接口
    - 允许重复：`List`。
        - 增删多：`LinkedList`（底层维护双向链表）
        - 改查多：`ArrayList`（底层维护`Object`可变数组）
    - 不允许重复：`Set`。
        - 无序：`HashSet`，底层是`HashMap`，维护了哈希表（数组+链表+红黑树）
        - 排序：`TreeSet`
        - 插入和取出顺序一致：`LinkedHashSet`，底层维护数组+双向链表

3. 一组键值对：`Map`接口
    - 键无序：`HashMap`，底层是哈希表。
    - 键排序：`TreeMap`。
    - 键插入和取出顺序一致：`LinkedHashMap`，底层维护数组+双向链表。
    - 读取文件：`Properties`。

### `Collections`工具类

1. 介绍
    - `Collections`是一个操作`Set`、`List`、`Map`等集合的工具类。
    - `Collections`类中提供了一系列静态方法对集合元素进行排序、查询和修改等操作。

2. 排序操作
    - `reverse(List)`：反转List中的元素顺序。
    - `shuffle(List)`：对List集合元素进行随机排序。
    - `sort(List)`：升序排序，本质上是调用`Comparable`接口的`compareTo()`方法。
    - `sort(List, Comparator)`：自定义排序
    - `swap(List, int, int)`：交换两个指定位置的元素

3. 查找、替换
    - `Object max(Collection)`：根据元素自然顺序，返回集合中最大的元素。
    - `Object max(Collection, Comparator)`：根据自定义排序，返回最大元素。
    - `Object min(Collection)`：
    - `Object min(Collection, Comparator)`：
    - `int frequency(Collection, Object)`：返回某元素在给定集合中的出现次数。
    - `void copy(List dest, List src)`：复制List到指定位置。
    - `boolean replaceAll (List list, Object oldval, Object newval)`：使用新值替换所有旧值。
    

## 泛型

### 本章目录

1. 泛型语法
2. 自定义泛型
    - 泛型类
    - 泛型接口
    - 泛型方法
3. 泛型继承和通配符

4. generic

### 引例
1. 需求
    编写一个程序，在ArrayList中添加3个Dog对象。使用get方法输出。
2. 传统解决方案
    ```java
    // main...
    ArrayList arrayList = new ArrayList();
    arrayList.add(new Dog("大黄1", 1));
    arrayList.add(new Dog("大黄2", 2));
    arrayList.add(new Dog("大黄3", 3));

    for (Object obj : arrayList){       // 这里只接受Object类型的迭代对象
        Dog dog = (Dog)obj;
        System.out.println("name:" + dog.getName() + ",age:" + dog.getAge());
    }
    ```
    - 这个解决方案有几个缺点。
    - 需要向下转型，当数据量很大的时候，这行代码会造成明显的时间浪费。
    - 当ArrayList出现非Dog对象时，会报错。
3. 泛型引出
    ```java
    // main....
    ArrayList<Dog> arrayList = new ArrayList<Dog>();
    arrayList.add(new Dog("xiaoming", 1));
    arrayList.add(new Dog("xiaohong", 2));
    for (Dog dog : arrayList){
        System.out.println("name:" + dog.getName() + ",age:" + dog.getAge());
    }
    ```
    - `ArrayList<Dog>`表示ArrayList中存放的是Dog对象，编译器会检查是否是Dog对象，如果不是，则会报错。
    - 使用泛型后，可以直接在遍历中获取Dog对象，而不是Object，不需要强转。

### 基本介绍

1. 泛型介绍
    - 泛型又称参数化类型，是JDK5.0后的新特性，解决数据类型的安全性问题。
    - 在类声明或实例化时，只要指定好需要的具体类型即可。
    - Java泛型可以保证如果程序在编译时没有发生警告，运行时就不会抛出`ClassCastException`异常。
    - 泛型的作用是：可以在类声明时通过一个标识标识类中某个属性的，类型，或者是某个方法的返回值类型，或者是参数类型。

2. 泛型语法
    - 声明
        ```java
        Interface IA <T> {}
        class ClassA <K,V> {}
        ```
        - 一般用`T`（Type）、`K`（Key）、`V`（Value）表示。
        - 理论上可以使用任意字母表示，吸管刷
    - 实例化
        ```java
        List<String> list = new ArrayList<String>();
        Iterator<Customer> iter = customers.iterator();
        ```
    
3. 注意事项与细节
    - 泛型只能是引用类型，不能是基本类型。（比如指定`int`，需要使用`<Integer>`，否则会报错`类型参数不能是基元类型`）。
    - 在给泛型指定具体类型后，可以传入该类型或该类型的子类型。
    - 泛型的使用形式
        ```java
        List<Integer> list1 = new ArrayList<Integer>(); 
        List<Integer> list2 = new ArrayList<>();        // 【推荐】右侧泛型类型可以省略
        List list3 = new ArrayList<>();                 // 不指定泛型，默认为Object类型
        ```

### 自定义泛型

1. 基本语法
    ```java
    class 类名 <T, R...>{
        // 成员
    }
    ```
2. 注意细节
    - 普通成员可以使用泛型（属性、方法）。
    - 使用泛型的数组，不能初始化。（不能确定类型，无法在内存中开辟空间，不能`new`）
    - 静态方法中不能使用类的泛型。（静态方法与类相关，类加载先于对象创建，如果静态方法使用泛型，JVM无法完成初始化）
    - 泛型类的类型，是在创建对象时确定的。
    - 如果在创建对象时，没有指定类型，默认为`Object`类型。

3. 自定义泛型接口
    ```java
    interface 接口名 <T>{}
    ```
4. 接口细节
    - 接口中，静态成员也不能使用泛型。
        - 对于接口，无法用泛型定义属性，因为其属性都是`final static`修饰的。
    - 泛型接口的类型在**继承接口**或者**实现接口**时确定。不写默认是`Object`。
        - 继承接口
            ```java
            interface IA extends IB<Double, String>{...}
            ```
        - 实现接口
            ```java
            class A implements IB<Double, String>{...}
            ```
    - 没有指定类型，默认为`Object`类型。


#### 自定义泛型方法

1. 基本语法
    ```java
    修饰符 <T,R...> 返回类型 方法名(参数方法){}
    ```

2. 注意细节
    - 泛型方法可以定义在普通类中，也可以定义在泛型类中。
    - 当泛型方法被调用时，类型会确定。（不须显式传入泛型，编译器会根据传入的值确定泛型类型）
    - 在下面的例子中，修饰符后没有泛型的方法不是泛型方法，只是使用了泛型。
        ```java
        public void eat(E e){}
        ```

3. 泛型方法举例
    ```java
    class Car{
        public void run(){...}
        public <T, R> void fly(T t, R r){...}

    car.fly(1, "hello");// T = Integer, R = String
    car.fly("hello", 1);// T = String, R = Integer
    }
    ```

### 泛型继承与通配符

1. 泛型不具备继承性
    - 下面的例子中，虽然`String`继承自`Object`，但是`List<String>`不能继承`List<Object>`
    ```java
    List<Object> list = new ArrayList<String>();// 错误
    ```

2. 通配符
    - `<?>`：支持任意类型的泛型。**可以将类型限定为单一类型，不像`<Object>`一样可以添加任意类型到同一列表中。**
    - `<? extends A>`：支持A类及其子类，规定了泛型的上限。
    - `<? super A>`：支持A类及其父类，规定了泛型的下限。
    


### 附加内容

#### JUnit单元测试类

1. 为什么需要JUnit
    - 一个类有很多功能代码需要测试，为了测试，就需要写入到main方法中。
    - 如果有多个功能代码测试，就需要来回注销，切换很麻烦。
    - 如果可以直接运行一个方法，就方便很多，并且可以各处相关信息就好了。

2. 基本介绍
    - JUnit是Java语言的单元测试框架。
    - 多数Java环境都已经集成了JUnit作为单元测试工具。

3. 使用步骤
    - 在需要测试的方法上方增加`@Test`。
    - 注解位置此时会报红，在注解上按`Alt+Enter`选择适合的JUnit工具，这里我们选择`JUnit5.8.1`。
        ![java_IDEA_JUnitUse2](./img/java_IDEA_JUnitUse2.png)
        ![java_IDEA_JUnitUse3](./img/java_IDEA_JUnitUse3.png)
        
    - 此时IDEA对应方法左侧会出现一个绿色三角，点击即可运行。

## 【项目】坦克大战（v1.0）

### 项目内容简述

1. 为什么写这个项目？
    - 趣味性高。
    - 涉及多种技术
        - Java面向对象编程。
        - Java多线程。
        - 文件IO操作。
        - 数据库。
    - 巩固旧知识，学习新知识。

### 素材绘制

#### 程序主框架

1. `TankGame02.java`
    ```java
    public class TankGame02 extends JFrame {
        private MyPanel panel = null;

        public static void main(String[] args) {
            TankGame02 tankGame02 = new TankGame02();
        }

        public TankGame02() {
            this.panel = new MyPanel();

            this.add(panel);
            this.addKeyListener(panel);
            this.setSize(1000, 700);
            this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            this.setVisible(true);
        }
    }
    ```

#### 完成坦克绘制

1. `Tank.java
    ```java
    public class Tank {
        private int x;
        private int y;
        private int direction;

        public Tank(){
            this(0,0);
        }

        public Tank(int x, int y) {
            this.x = x;
            this.y = y;
            this.direction = 0;
        }

        public int getX() {
            return x;
        }

        public void setX(int x) {
            this.x = x;
        }

        public int getY() {
            return y;
        }

        public void setY(int y) {
            this.y = y;
        }

        public int getDirection() {
            return direction;
        }

        public void setDirection(int direction) {
            this.direction = direction;
        }
    }
    ```

2. `EnemyTank.java`
    ```java
    public class EnemyTank extends Tank {
        public EnemyTank(int x, int y) {
            super(x, y);
        }
    }
    ```

3. `Hero.java`
    ```java
    public class Hero extends Tank {

        public Hero(int x, int y) {
            super(x, y);
        }
    }
    ```

4. `MyPanel.java`
    ```java
    public class MyPanel extends JPanel implements KeyListener {

        Hero hero = null;

        Vector<EnemyTank> enemyTanks = new Vector<>();
        private static final int ENEMY_TANK_SIZE = 3;

        public MyPanel() {
            hero = new Hero(200, 400);
            for (int i = 0; i < ENEMY_TANK_SIZE; i++) {
                EnemyTank enemyTank = new EnemyTank(100 * (i + 1), 30);
                enemyTank.setDirection(2);
                enemyTanks.add(enemyTank);
            }
        }

        @Override
        public void paint(Graphics g) {
            super.paint(g);
            drawTank(hero, g, hero.getDirection(), 0);

            for (EnemyTank enemyTank : enemyTanks) {
                drawTank(enemyTank, g, enemyTank.getDirection(), 1);
            }
        }

        /**
         * @param tank
         * @param g
         * @param direction (0:上,1:右,2:下,3:左)
         * @param type
         */

        public void drawTank(Tank tank, Graphics g, int direction, int type) {
            int x = tank.getX();
            int y = tank.getY();

            switch (type) {
                case 0:
                    g.setColor(Color.cyan);
                    break;
                case 1:
                    g.setColor(Color.yellow);
                    break;
            }

            switch (direction) {
                case 0:
                    g.fill3DRect(x, y, 10, 60, false);
                    g.fill3DRect(x + 10, y + 10, 20, 40, false);
                    g.fill3DRect(x + 30, y, 10, 60, false);
                    g.fillOval(x + 10, y + 20, 20, 20);
                    g.drawLine(x + 20, y + 30, x + 20, y);
                    break;
                case 1:
                    g.fill3DRect(x, y, 60, 10, false);
                    g.fill3DRect(x + 10, y + 10, 40, 20, false);
                    g.fill3DRect(x, y + 30, 60, 10, false);
                    g.fillOval(x + 20, y + 10, 20, 20);
                    g.drawLine(x + 30, y + 20, x + 60, y + 20);
                    break;
                case 2:
                    g.fill3DRect(x, y, 10, 60, false);
                    g.fill3DRect(x + 10, y + 10, 20, 40, false);
                    g.fill3DRect(x + 30, y, 10, 60, false);
                    g.fillOval(x + 10, y + 20, 20, 20);
                    g.drawLine(x + 20, y + 30, x + 20, y + 60);
                    break;
                case 3:
                    g.fill3DRect(x, y, 60, 10, false);
                    g.fill3DRect(x + 10, y + 10, 40, 20, false);
                    g.fill3DRect(x, y + 30, 60, 10, false);
                    g.fillOval(x + 20, y + 10, 20, 20);
                    g.drawLine(x + 30, y + 20, x, y + 20);
                    break;

            }
        }

        @Override
        public void keyTyped(KeyEvent e) {

        }

        @Override
        public void keyPressed(KeyEvent e) {
            switch (e.getKeyCode()) {
                case KeyEvent.VK_UP:
                    hero.setDirection(0);
                    hero.setY(hero.getY() - 10);
                    break;
                case KeyEvent.VK_LEFT:
                    hero.setDirection(3);
                    hero.setX(hero.getX() - 10);
                    break;
                case KeyEvent.VK_RIGHT:
                    hero.setDirection(1);
                    hero.setX(hero.getX() + 10);
                    break;
                case KeyEvent.VK_DOWN:
                    hero.setDirection(2);
                    hero.setY(hero.getY() + 10);
                    break;
            }

            this.repaint();
        }

        @Override
        public void keyReleased(KeyEvent e) {

        }
    }
    ```

#### 敌方坦克
1. 分析
    - 因为敌人坦克在`MyPanel`中，所以坦克绘制在`MyPanel`中。
    - 因为敌人的坦克后期会有特殊方法，所以可以单开一个`Enemy`类继承`Tank`类。
    - 敌人坦克数量多，考虑多线程问题，使用线程安全的`Vector`类。

### 附加内容

#### Java绘图坐标体系

1. 介绍
    - 坐标原点位于左上角，坐标以像素为单位
    - 向右为x正方向，向下为y正方向，数值表示离原点的水平/垂直像素量。

2. 快速入门-绘制一个圆
    ```java
    public class DrawCircle extends JFrame {

        private MyPanel panel = null;

        public static void main(String[] args) {
            DrawCircle drawCircle = new DrawCircle();
            System.out.println("Finish!!!");
        }

        public DrawCircle() {
            panel = new MyPanel();      // 创建画板

            this.add(panel);            // 将画板加入窗口
            this.setSize(1920, 1080);     // 初始化窗口大小
            this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); // 设置为关闭窗口即程序结束
            this.setVisible(true);                  // 可以显示
        }
    }

    class MyPanel extends JPanel {

        @Override
        public void paint(Graphics g) {
            super.paint(g);     // 完成面板初始化
            // 输出日志，当持续改变窗口大小时，日志会不断输出。
            System.out.println("paint be used\t"+
                    DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss").
                            format(LocalDateTime.now()));

            // 绘制一个椭圆，xy决定外接矩形的左上顶点，width和height决定外接矩形的大小。
            g.drawOval(10,10,1000,1000);
        }
    }
    ```
    - `Component` 类提供了来年各个和绘图相关最重要的方法：
        - `void paint(Graphics g)`：绘制组件外观。
        - `repaint()`：刷新组件外观。
    - 当组件第一次在屏幕上显示时，会自动调用`paint()`方法。
    - 下列情况发生时，`paint()`方法会再次调用：
        - 窗口最小化，再最大化。
        - 窗口的大小发生变化。
        - `repaint()`方法被调用。
    - 在本例中：
        - `MyPanel`类继承自`JPanel`，相当于一个画板，而`Graphics g`相当于一支画笔。
        - `Graphics`对象封装了所有绘制的方法。
    - **绘图框架解读**：
        - 主类继承`JFrame`（窗口）类，并创建一个`MyPanel`（画板）对象作为属性。
        - 通过主类构造器完成窗口基本设置与画板的指定。
        - 创建`MyPanel`类并重写`paint(Graphic g)`方法（画笔），用`Graphic`类完成绘图。

3. `Graphic`类的常用方法
    - `drawLine(int x1, int y1, int x2, int y2)`：画直线。
    - `drawRect(int x, int y, int width, int height)`：画矩形。
    - `drawOval(int x, int y, int width, int height)`：画圆。
    - `fillRect(int x, int y, int width, int height)`：填充矩形。
    - `fillOval(int x, int y, int width, int height)`：填充圆。
    - `drawImage(Image img, int x, int y)`：画图片。
    - `drawString(String str, int x, int y)`：画字符串，此时的坐标为文字的**左下角**。
    - `setFont(Font font)`：设置字体。
    - `setColor(Color color)`：设置颜色。
    ```java
    public class Draw02 extends JFrame {

        private DrawPanel02 drawPanel02 = null;
        public static void main(String[] args) {
            Draw02 draw02 = new Draw02();
            System.out.println("Finish!!!");
        }

        public Draw02() throws HeadlessException {
            drawPanel02 = new DrawPanel02();

            this.add(drawPanel02);
            this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            this.setSize(1920, 1080);
            this.setVisible(true);
        }
    }

    class DrawPanel02 extends JPanel {
        @Override
        public void paint(Graphics g) {
            super.paint(g);

            System.out.println("paint be used\t"+
                    DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss").
                            format(LocalDateTime.now()));

            g.drawOval(10,10,100,100);
            g.drawLine(0,60,120,60);
            g.drawRect(10,10,100,100);

            // 图片路径为out/production/project_name/test.png
            Image img = Toolkit.getDefaultToolkit().getImage(Panel.class.getResource("/test.png"));
            g.drawImage(img, 130, 10, 500, 500, this);

            g.setColor(Color.red);
            g.setFont(new Font("Times New Roman", Font.BOLD, 20));
            g.drawString("Hello World", 10, 200);
        }
    }
    ```

#### Java事件处理机制

1. 基本说明
    - Java事件处理是采取“委派事件模型”。当事件发生时，产生事件的对象会把此信息传递给“事件监听者”处理。
    - 这里所说的“信息”，实际上就是`java.awt.event`事件类库里某个类创建的对象，把它称为“事件的对象”。

2. 事件处理机制详解
    - 下面介绍事件源、事件、时间监听器等概念。
    - 事件源：一个产生事件的对象，比如按钮、窗口等。
    - 事件：承载事件源状态改变时的对象，如键盘事件、鼠标事件、窗口事件等，会生成一个事件对象，该对象保存着当前事件很多信息，比如`KeyEvent`对象包含被按下键的`Code`值。`java.awt.event`和`javax.swing.event`包里定义了各种事件类型。

3. 常见事件类型
    |事件类|说明|
    |:---:|:---:|
    |`ActionEvent`|通常在按下按钮，<br>或双击一个列表项，<br>或选中某个菜单时发生|
    |`AdjustmentEvent`|当操作一个滚动条时发生|
    |`ComponentEvent`|当一个组件隐藏、移动、改变大小时发生|
    |`ContainerEvent`|当一个组件从容器中加入或者删除时发生|
    |`FoucusEvent`|当一个组件获得或失去焦点时发生|
    |`ItemEvent`|当一个复选框或是列表被选中时，<br>当一个选择框或选择菜单被选中时发生|
    |`KeyEvent`|当键盘按键被按下/松开时发生|
    |`MouseEvent`|当鼠标被拖动、移动、点击、按下时发生|
    |`TextEvent`|当文本区和文本域的文本发生改变时发生|
    |`WindowEvent`|当一个窗口激活、关闭、失效、恢复、最小化时发生|
    
4. 事件监听器接口
    - 当事件源产生一个事件，可以传送给事件监听者处理。
    - 事件监听者实际就是一个类，该类实现了某个事件监听器的接口，比如在下面的示例中，`MyPanel`类实现了`KeyListener`接口。
    - 事件监听器接口有多种，不同的事件监听器接口可以监听不同的事件，一个类可以实现多个监听接口。
    - 这些接口在`java.awt.event`和`javax.swing.event`包中定义，具体请自行查阅JDK文档。

5. 【示例】控制小球移动
    ```java
    public class BallMove extends JFrame {
        MyPanel panel = null;

        public static void main(String[] args) {
            BallMove ballMove = new BallMove();
            System.out.println("Finish!!!");
        }

        public BallMove() throws HeadlessException {
            panel = new MyPanel();

            this.add(panel);
            this.setSize(800, 600);
            this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            this.setVisible(true);

            this.addKeyListener(panel);     // 将对键盘的监听加入JFrame窗口
        }
    }

    class MyPanel extends JPanel implements KeyListener {

        int x = 10;
        int y = 10;

        @Override
        public void paint(Graphics g) {
            super.paint(g);
            g.fillOval(x, y, 20, 20);
        }


        @Override
        public void keyTyped(KeyEvent e) {  // 字符输出时触发
            System.out.println((char) e.getKeyCode() + " keyout!!!");
        }

        @Override
        public void keyPressed(KeyEvent e) {    // 键盘按下时触发
            System.out.println((char) e.getKeyCode() + " keyPressed!!!");
            switch (e.getKeyCode()) {
                case KeyEvent.VK_UP:
                    y--;
                    break;
                case KeyEvent.VK_DOWN:
                    y++;
                    break;
                case KeyEvent.VK_LEFT:
                    x--;
                    break;
                case KeyEvent.VK_RIGHT:
                    x++;
                    break;
            }
            repaint();
        }

        @Override
        public void keyReleased(KeyEvent e) {   // 键盘释放时触发
            System.out.println((char) e.getKeyCode() + " keyReleased!!!");
        }
    }

    ```






    
















