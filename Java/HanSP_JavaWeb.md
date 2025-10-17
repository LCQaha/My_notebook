# 韩顺平JavaWeb笔记

## PRE

### 课程目录

## B/S软件开发架构

### JavaWeb技术体系图——一图胜千言

#### 简述
1. 图解
    ![javaweb_BS_techStack](./img/javaweb_BS_techStack.png)

2. B/S框架
    - 意思是前端(Browser 浏览器)和服务器端(Server)组成的系统的框架结构。
    - B/S框架即Web架构，主要由前端、后端、数据库三大部分组成。
    - 在Web编程中，前端程序员并不在乎后端程序员使用的框架、技术，后端亦然，大家只关心交互的数据格式是不是我需要的。

3. 前端
    - 前端就是由浏览器、APP、小程序等载体构成的客户端。（一般是浏览器）
    - 前端开发技术工具包括三要素：HTML、CSS 和 JavaScript，还有很多高级的前端框架，如bootstrap、jquery，VUE 等，这些高级框架是在基础要素上扩展而来的。
    - 对于一个后端开发人员而言，对于前端技术栈，做到基本使用、打通前后端数据交互即可


4. 后端
    - 后端用于承载各种服务，前端与后端通过某种格式的数据文件相互联系。
    - 后端开发技术工具主要有：Net、JAVA、PHP, Go 等
    - Tomcat[Web 服务器 + 容器]
        - Web服务
        - 容器
    - 数据库（DB）

## HTML

### 基本内容

#### 官方文档

1. [HTML基础教程](https://www.w3school.com.cn/)

#### 网页

1. 结构（HTML）
    - HTML是网页内容的载体。内容就是网页制作者放在页面上想要让用户浏览的信息，可以包含文字、图片、视频等。
    - 最简单，所见即所得。
2. 表现（CSS）
    - CSS样式是表现。就像网页的外衣。比如，标题字体、颜色变化，或为标题加入背景图片、边框等。所有这些用来改变内容外观的东西称之为表现。
    - 不同的浏览器解析出的结果可能不同，不同浏览器间的兼容性可能出现问题。
3. 行为（JavaScript、JQuery）
    - JavaScript是用来实现网页上的特效效果。如：鼠标滑过弹出下拉菜单。或鼠标滑过表格的背景颜色改变。还有购物网站中图片的轮换。可以这么理解，有动画的，有交互的一般都是用JavaScript来实现的.

4. 快捷键
    - 在浏览器中，使用快捷键`Ctrl+Shift+I`或`F12`可以打开Web开发者模式。（适用于火狐、谷歌）

#### HTML

1. 概念
    - HTML（HyperText Mark-up Language），即超文本标记语言，可以展示很多内容类型。
    - HTML文本是由HTML标签组成的文本，可以包括文字、图形、动画、声音、表格、链接等。
    - HTML的结构包括头部（Head）、主体（Body）两大部分，其中头部描述浏览器所需的信息，主体包括具体内容。

2. 两种方式
    - 本地运行
    - 远程访问

3. 本地运行
    - 在本地编写HTML文件，由浏览器解析。

4. 远程访问
    - 这个方式用得最多。
    - 从远端服务器中通过HTTP响应等方式获取相应的HTML文件，由浏览器进行解析。
    - 对于需要后端交互或测试某些API的页面，可能需要​​搭建本地服务器​​（比如使用Python的http.server模块或Node.js的live-server包），而不是直接双击打开。

#### HTML快速入门

1. 使用IDEA开始开发
    - 在新版的IDEA中，不再直接提供`JavaScript`选项，可以直接选择`HTML 文件`作为替代。

2. 第一个HTML文件`hello.html`
    ```html
    <!--文档类型说明-->
    <!DOCTYPE html>
    <!--使用语言的地区：en-US（英国美国），可以改为"zh"-->
    <html lang="en">
    <!--html头-->
    <head>
        <!--charset字符集-->
        <meta charset="UTF-8">
        <!--文件标题-->
        <title>First</title>
    </head>
    <!--主体部分-->
    <body>
        <!--内容-->
        lcqaha
    </body>
    </html>
    ```

3. 注意事项
    - 不要使用电脑上不存在的浏览器软件打开预览
    - HTML 文件不需要编译，直接由浏览器进行解析执行

### HTML 标签/元素

1. 说明
    - **HTML 标签**用两个尖括号`<>`括起来。
    - HTML 标签一般是双标签，如`<b>`和`</b>` 前一个标签是起始标签, 后一个标签为结束标签。
    - 两个标签之间的文本是 html 元素的内容。
    - 某些标签称为"单标签",因为它只需单独使用就能完整地表达意思,如`<br/>`（换行） `<hr/>`（水平线）。（末尾带斜杠，自结束标签）
    - **HTML 元素**指的是从开始标签到结束标签的所有代码。

2. 标签注意事项和使用细节
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>标签使用细节</title>
    </head>
    <body>
    <!-- 标签使用细节:
    1.标签不能交叉嵌套
    2.标签必须正确关闭
    3.注释不能嵌套
    4. html 语法不严谨。有时候标签不闭合，属性值不带””也不报错
    -->
    <!--
    标签不能交叉嵌套：
    错误：标签<span>没有闭合
    -->
    <div><span>tom</div></span>

    <!--
    标签必须正确关闭
    错误：元素 span 未闭合
    -->
    <span>aha

    <!--
    注释不能嵌套
    下面为错误示范
    -->
    <!-- <!--  --> -->

    <!--  HTML语法不严谨-->
    <font color="blue">你好</font>
    <font color=red>你好</font>

    </body>
    </html>
    ```

#### font字体标签
1. 说明    
    - 对应标签的属性，顺序不做要求。
2. 示例
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>font 标签</title>
    </head>
    <body>
    <!-- 字体标签
    应用实例 1：在网页上显示 北京 ，并修改字体为 微软雅黑，
    颜色为蓝色。
    font 标签是字体标签,它可以用来修改文本的字体,颜色,大小(尺
    寸)
    (1)color 属性修改颜色
    (2)face 属性修改字体
    (3)size 属性修改文本大小
    多说一句，对应标签的属性，顺序不做要求
    -->
    <font size="40px" face="微软雅黑" color="red">北京</font>
    </body>
    </html>
    ```

#### 字符实体

1. 说明
    - 在实际前端开发中，会使用css完成相关操作。
2. 示例    
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>字符实体</title>
    </head>
    <body>
    <!-- 特殊字符 应用实例：
    把 <hr /> 变成文本 显示在页面上
    常用的特殊字符:
    < : &lt;
    > : &gt;
    空格 : &nbsp; -->
    jack
    <!--浏览器会将 <hr/>解析成一条线-->
    <hr/>
    &lt;hr/&gt;
    smith smith2 hsp<br/>
    smith
    smith2&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbs
    p;hsp
    </body>
    </html>
    ```
    - 常用字符实体
        | 显示结果 | 描述     | 实体名称       | 实体编号 |
        | :------- | :------- | :------------- | :------- |
        |    ` `      | 空格     | `&nbsp;`       | `&#160;` |
        | <        | 小于号   | `&lt;`         | `&#60;`  |
        | >        | 大于号   | `&gt;`         | `&#62;`  |
        | &        | 和号     | `&amp;`        | `&#38;`  |
        | "        | 引号     | `&quot;`       | `&#34;`  |
        | '        | 撇号     | `&apos;` (IE不支持) | `&#39;`  |
        | ¢        | 分           | `&cent;`        | `&#162;` |
        | £        | 镑           | `&pound;`       | `&#163;` |
        | ¥        | 日圆         | `&yen;`         | `&#165;` |
        | §        | 节           | `&sect;`        | `&#167;` |
        | ©        | 版权         | `&copy;`        | `&#169;` |
        | ®        | 注册商标     | `&reg;`         | `&#174;` |
        | ×        | 乘号         | `&times;`       | `&#215;` |
        | ÷        | 除号         | `&divide;`      | `&#247;` |

#### 标题标签
1. 示例
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>标题标签</title>
    </head>
    <body>
    <!-- 标题标签
    应用实例：演示标题 1 到 标题 6 的
    h1 - h6 都是标题标签 h1 : 最大 h6 : 最小
    align: 属性是对齐属性
    left: 左对齐(默认)
    center :居中
    right : 右对齐
    -->
    <h1>标签 1</h1>
    <h2>标签 2</h2>
    <h3 align="center">标签 3</h3>
    <h4>标签 4</h4>
    <h5>标签 5</h5>
    <h6 align="right">标签 6</h6>
    </body>
    </html>
    ```

#### 超链接
1. 说明
    - "​​anchor​​"（锚点）
    - href：超文本引用（hypertext reference）
2. 示例    
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>超链接标签</title>
    </head>
    <body>
    <!-- 老韩说明:
    a 标签是 超链接
    href 属性设置连接的地址 
    target 属性设置哪个目标进行跳转 
        _self : 表示当前页面(默认值), 即使用当前替换目标页
        _blank : 表示打开新页面来进行跳转
    点击超链接，打开邮件
    -->
    <a href="http://www.sohu.com">搜狐</a><br/>
    <a href="http://www.sohu.com" target="_blank">搜狐 2</a><br/>
    <a href="mailto:tom@sohu.com">联系管理员</a>
    </body>
    </html>
    ```

#### 无序列表（`ul`、`li`）
1. 说明
    - ul：unordered list
    - li：list item
    
2. 基本语法
    ```html
    <!-- 
    设定符号款式，其值有三种，如下:
    默认为 type="disc":
    type="disc" 时的列项符号为实心圆点。
    type="circle" 时的列项符号为空心圆。
    type="square" 时的列项符号为空心正方形。
        -->
    <ul type="属性值">
        <li>列表内容</li>
    <ul>
    ```
3. 示例
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>ul li 标签</title>
    </head>
    <body>
    <!-- ul : 表示无序列表
    li : 列表项
    type 属性：指定列表项前的符号
    -->
    <h1>五虎将</h1>
    <ul type="circle">
    <li>jack</li>
    <li>tom</li>
    <li>smith</li>
    <li>mary</li>
    <li>milan</li>
    </ul>
    </body>
    </html>
    ```
#### 有序列表

1. 说明
    - ol：ordered list
2. 基本语法
    ```html
    <ol type = "i" start = "4">
        <li>列表内容</li>
    <ol>
    ```
3. 示例
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>有序列表 ol-li</title>
    </head>
    <body>
    <!-- ol : 表示有序列表
    li : 列表项
    type 属性：指定列表项前排序方式
    type 设定数目款式，其值有五种。
    start 开始值，默认为 start="1"。
    i 可以取以下值中的任意一个：
    1 阿拉伯数字 1, 2, 3, ...
    a 小写字母 a, b, c, ...
    A 大写字母 A, B, C, ...
    i 小写罗马数字 i, ii, iii, ...
    I 大写罗马数字 I, II, III, ...
    -->
    <h1>五虎将</h1>
    <ol type="I" start="3">
    <li>jack</li>
    <li>tom</li>
    <li>smith</li>
    <li>mary</li>
    <li>milan</li>
    </ol>
    </body>
    </html>
    ```

#### 图像标签
1. 说明
    - img标签可以用于在HTML页面上显示图片，由于markdown支持HTML，所以这里用img标签显示一张图片作为示例。
        <img src="https://i0.hdslb.com/bfs/new_dyn/2ae4ddeb10119383d05982a94ab35850473837611.jpg" alt = "图片源丢失">
    - 注：由于新浪图床排斥外链，故图片无法从新浪引用进我的markdown文档中，当前图片资源来自B站。
2. 示例
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>图像标签</title>
    </head>
    <body>
    <!-- 应用实例：使用 img 标签显示一张美女的照片。
    img: 标签是图片标签,用来显示图片
    src: 属性可以设置图片的路径
    width: 属性设置图片的宽度
    height: 属性设置图片的高度
    border: 属性设置图片边框大小（已过时，应在CSS中设置）
    alt: 属性设置当指定路径找不到图片时,用来代替显示的文本内容
    相对路径:从工程名开始算
    绝对路径:盘符:/目录/文件名
    在 web 中路径分为相对路径和绝对路径两种
    相对路径: .表示当前文件所在的目录
    ..表示当前文件所在的上一级目录
    文件名 : 表示当前文件所在目录的文件,相当于 ./文件名 ./ 可以省略
    绝对路径: 正确格式是: http://IP 地址:port/工程名/资源路径
    错误格式是: 盘符:/目录/文件名
    应用实例：演示如何将图片做成超链接
    -->
    <!-- 如果只是指定 width 或者 height 浏览器会按比例显示，不会变形 -->
    <img src="x.png" width="300" border="1" alt="美女找不到"/><hr />
    <img src="../1.png" width="300" border="1" alt="美女找不到"/><hr />
    <img src="../aaa/1.png" width="100" border="1" alt="美女找不到"/><hr />
    <!-- 如果同时指定 width height 自己要计算，否则图像会变形 -->
    <img src="../aaa/1.png" width="200" height="80" alt="美女找不到"/><hr />
    </body>
    </html>
    ```

#### 表格标签（常用）

1. 说明
    - th：table header cell
    - td：table data cell
    - tr：table row
    - rowspan​​：​​Row Span​​（行跨度），指定一个单元格应横跨多少行。
    - colspan​​：​​Column Span​​（列跨度），指定一个单元格应横跨多少列。
2. 基本语法
    ```html
    <table 
        border = "边框宽度" 
        cellspacing = "空隙大小"
        cellpadding = "填充大小">
    </table>
    ```

2. 示例：显示一个三行三列的表格。
    <table width="500" border="10" align="center">
    <h1 align="center">表格标签的使用</h1>
    <tr>
        <th>名字</th>
        <th>住址</th>
        <th>邮件</th>
    </tr>
    <tr>
        <td>第 1 行第 1 列</td>
        <td>第 1 行第 2 列</td>
        <td>第 1 行第 3 列</td>
    </tr>
    <tr>
        <td>第 2 行第 1 列</td>
        <td>第 2 行第 2 列</td>
        <td>第 2 行第 3 列</td>
    </tr>
    <tr>
        <td>第 3 行第 1 列</td>
        <td>第 3 行第 2 列</td>
        <td>第 3 行第 3 列</td>
    </tr>
    </table>

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>表格标签</title>
    </head>
    <body>
    <!-- 说明：
    table： 标签是表格标签 border： 设置表格标签
    width： 设置表格宽度 height： 设置表格高度
    align： 设置表格相对于页面的对齐方式
    cellspacing： 设置单元格间距
    tr ：是行标签 th ：是表头标签 td ：是单元格标签
    align： 设置单元格文本对齐方式 b ：是加粗标签
    px:表示像素 - java 坦克大战
    ctrl +shift + 下光标
    -->
    <table width="500" border="6" align="center">
    <h1 align="center">表格标签的使用</h1>
    <tr>
        <th>名字</th>
        <th>住址</th>
        <th>邮件</th>
    </tr>
    <tr>
        <td>第 1 行第 1 列</td>
        <td>第 1 行第 2 列</td>
        <td>第 1 行第 3 列</td>
    </tr>
    <tr>
        <td>第 2 行第 1 列</td>
        <td>第 2 行第 2 列</td>
        <td>第 2 行第 3 列</td>
    </tr>
    <tr>
        <td>第 3 行第 1 列</td>
        <td>第 3 行第 2 列</td>
        <td>第 3 行第 3 列</td>
    </tr>
    </table>
    </body>
    </html>
    ```
4. 跨行跨列表格
    <table border="6" height="250" bordercolor="#679e40ff" cellspacing="0" width="500">
    <tr>
        <!--合并了 3 列-->
        <td align="center" colspan="3">第 1 行第 1 列</td>
    </tr>
    <tr>
        <!-- 合并行，跨行 row 行-->
        <td rowspan="2">第 2 行第 1 列</td>
        <td>第 2 行第 2 列</td>
        <td>第 2 行第 3 列</td>
    </tr>
    <tr>
        <td>第 3 行第 2 列</td>
        <td>第 3 行第 3 列</td>
    </tr>
    <tr>
        <!--合并行，跨行 row 行-->
        <td rowspan="2">第 4 行第 1 列</td>
        <td>第 4 行第 2 列</td>
        <td>第 4 行第 3 列</td>
    </tr>
    <tr>
        <td> 第 5 行 第 2 列 <img src="imgs/2.png" width="100"></td>
        <td>第 5 行第 3 列</td>
    </tr>
    </table>


    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>表格(跨行跨列)</title>
    </head>
    <body>
    <!-- column 列
    合并列 : colspan="列数" 合并行 : rowspan="行数" cellspacing : 指定单元格间的空隙大小 :0 表示没有空隙
    bordercolor: 指定表格边框的演示
    border: 表格边框
    width： 表格的宽度
    -->
    <table border="1" height="250" bordercolor="#E87EFA" cellspacing="0" width="500">
    <tr>
        <!--合并了 3 列-->
        <td align="center" colspan="3">第 1 行第 1 列</td>
    </tr>
    <tr>
        <!-- 合并行，跨行 row 行-->
        <td rowspan="2">第 2 行第 1 列</td>
        <td>第 2 行第 2 列</td>
        <td>第 2 行第 3 列</td>
    </tr>
    <tr>
        <td>第 3 行第 2 列</td>
        <td>第 3 行第 3 列</td>
    </tr>
    <tr>
        <!--合并行，跨行 row 行-->
        <td rowspan="2">第 4 行第 1 列</td>
        <td>第 4 行第 2 列</td>
        <td>第 4 行第 3 列</td>
    </tr>
    <tr>
        <td> 第 5 行 第 2 列 <img src="imgs/2.png" width="100"></td>
        <td>第 5 行第 3 列</td>
    </tr>
    </table>
    </body>
    </html>
    ```

#### form（表单）标签（常用）

1. 基本语法
    ```html
    <!-- *为GET或POST，这是HTTP协议的两种数据传输方式，后面会讲到-->
    <form action="url" method=*>
        ...
        ...
        <input type=submit> <input type=reset>
    </form>
    ```

2. 示例
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>表单登录</title>
    </head>
    <body>
    <!-- 老
    1. form 表示表单
    2. action: 提交到哪个页面
    3. method： 提交方式 ,常用 get（默认） 和 post
    4. input type=text 输入框
    5. input type=password 密码框
    6. input type=submit 提交按钮
    7. input type=reset 重置按钮
    为了个汉字对齐，输入全角的空格即可 -->
    <h1>登录系统</h1>
    <form action="ok.html" method="get">
    用户名:<input type="text" name="username"><br/>
    密　码:<input type="password" name="username"><br/>
    <input type="submit" value=" 登 录 "> <input type="reset" value="重新填写">
    </form>
    </body>
    </html>
    ```

#### input

1. 基本语法
    - `<input>`标签是​​空元素​​（或自闭合标签），用于在表单中创建各种类型的输入控件。
    - type(必选属性)​​：定义输入框的​​类型​​，如 text, password, checkbox等。如果省略，默认值为 text。
    - ​​name​​：指定输入字段的​​名称​​。​​该属性非常重要​​，在表单提交时，服务器端脚本通过 name属性来识别和接收数据。同一组单选按钮 (radio) 必须使用相同的 name属性值以实现“多选一”。
    - ​​value​​：定义输入字段的​​值​​。
        - 对于 text、password、hidden，它是输入字段的初始值或默认值。
        - 对于 checkbox、radio、image，它是该选项被选中时提交到服务器的值​​​​。
        - 对于 submit、reset、button，它定义按钮上显示的文本。
    - ​​id​​ ：为输入字段提供​​唯一标识​​。常与`<label>`标签的 for属性配合使用，以提升表单的可访问性（点击标签文字即可聚焦到对应输入框）。
    ```html
    <input type="" name="" value="">
    ```

2. 常用type
    | 类型         | 说明与示例                                                                                               | 常见属性                                       |
    | :----------- | :------------------------------------------------------------------------------------------------------- | :--------------------------------------------- |
    | **`text`**     | 单行文本输入框。`<input type="text" name="username" placeholder="请输入用户名">`             | `name`, `value`, `placeholder`, `maxlength`    |
    | **`password`** | 密码输入框，输入内容会被掩码（通常显示为星号或圆点）。`<input type="password" name="pwd">`   | `name`, `value`, `placeholder`, `maxlength`    |
    | **`email`**    | 专用于输入电子邮件地址。在移动设备上会触发更合适的虚拟键盘。支持基本的格式验证。`<input type="email" name="user_email">` | `name`, `value`, `placeholder`, `multiple`     |
    | **`url`**      | 专用于输入网址。`<input type="url" name="website">`                                          | `name`, `value`, `placeholder`                  |
    | **`number`**   | 用于输入数字。可以设置最小、最大值和步进。`<input type="number" name="age" min="0" max="120" step="1">` | `name`, `value`, `min`, `max`, `step`           |
    | **`date`**     | 日期选择器。`<input type="date" name="birthday">`                                            | `name`, `value`, `min`, `max`                   |
    | **`checkbox`** | 复选框，允许用户从多个选项中选择一项或多项。`<input type="checkbox" name="hobby" value="reading"> 阅读` | `name`, `value`, `checked`                      |
    | **`radio`**    | 单选按钮，允许用户从多个选项中选择其中一项。**同一组单选按钮必须使用相同的 `name` 属性**。`<input type="radio" name="gender" value="male"> 男` | `name`, `value`, `checked`                      |
    | **`submit`**   | 提交按钮。点击后会提交所在表单的数据。`<input type="submit" value="登录">`                   | `name`, `value`                                 |
    | **`reset`**    | 重置按钮。点击后会清空所在表单中用户已填写的内容。`<input type="reset" value="重置">`        | `value`                                         |
    | **`button`**   | 普通按钮，通常需要配合 JavaScript 实现特定功能。`<input type="button" value="点击我" onclick="...">` | `value`                                         |
    | **`file`**     | 文件上传。`<input type="file" name="avatar">`                                                | `name`, `accept` (如 `accept=".jpg, .png"`)，`multiple` |
    | **`hidden`**   | 隐藏字段。用户不可见，用于传递不需要用户输入但程序需要的数据。`<input type="hidden" name="user_id" value="12345">` | `name`, `value`                                 |

3. 常用属性
    - **`placeholder`**：在没有填写内容的输入框中显示提示信息。
    - **`required`**：布尔属性。规定字段在提交表单前必须填写。如果未填写，浏览器通常会显示提示信息。
        ```html
        <input type="text" name="username" required>
        ```
    - **`readonly`**：布尔属性。设置字段为只读，用户无法修改，但可以聚焦和选择文本，数据会提交。
        ```html
        <input type="text" name="readonly_field" value="不可修改" readonly>
        ```
    - **`disabled`**：布尔属性。禁用输入字段，用户无法与之交互，数据**不会**被提交。
        ```html
        <input type="text" name="disabled_field" value="已禁用" disabled>
        ```
    - **`checked`**：布尔属性。用于 `radio` 和 `checkbox`，设置默认选中的选项。
        ```html
        <input type="checkbox" name="agree" checked> 我同意协议
        <input type="radio" name="gender" value="male" checked> 男
        ```
    - **`maxlength`**：规定输入字段允许的**最大字符数**。
        ```html
        <input type="text" name="comment" maxlength="140">
        ```
    - **`min`, `max`, `step`**：主要用于 `number`, `range`, `date` 等类型，定义最小值、最大值和步进值。
        ```html
        <input type="number" name="quantity" min="1" max="10" step="1">
        ```
    - **`pattern`**：规定一个**正则表达式**，用于验证输入字段的值。
        ```html
        <input type="text" name="username" pattern="[A-Za-z]{3,}" title="至少3个英文字母">
        ```

#### select/option/textarea

1. 基本语法
    ```html
    <label for="lable_name"></label>
    <select>
    ```

2. 示例
    - id：用于和对应label的`for`属性（for attribute）相关联，这是为了将数据传向后端。
    - placeholder：用于在没有填写内容的输入框中显示提示内容

    ```html
    <form action="/register" method="post" name="registerForm">
    
    <label for="username">用户名：</label>
    <input type="text" id="username" name="username" placeholder="请设置用户名" required autofocus><br><br>
    
    <label for="email">邮箱：</label>
    <input type="email" id="email" name="email" placeholder="请输入有效邮箱" required><br><br>
    
    <label for="password">密码：</label>
    <input type="password" id="password" name="password" placeholder="请设置密码" required><br><br>
    
    <!-- 
    单选
    添加属性checked为默认选中
    对于单选表单，需要保证name相同，否则不视为同一表单
     -->
    <label>性别：</label>
    <label for="male"><input type="radio" id="male" name="gender" value="male"> 男</label>
    <label for="female"><input type="radio" id="female" name="gender" value="female"> 女</label><br><br>
    
    <!-- 
    多选
    添加属性checked为默认选中
     -->
    <label for="hobbies">兴趣爱好：</label>
    <label for="reading"><input type="checkbox" id="reading" name="hobby" value="reading"> 阅读</label>
    <label for="sports"><input type="checkbox" id="sports" name="hobby" value="sports"> 运动</label><br><br>
    
    <!-- 
    下拉菜单
    添加属性selected为默认选项
    option为选项
     -->
    <label for="city">所在城市：</label>
    <select id="city" name="city">
        <option value="">--请选择--</option>
        <option value="bj">北京</option>
        <option value="sh">上海</option>
    </select><br><br>
    
    <!-- 
    文本域 
    rows和cols用于设置宽度和高度
    -->
    <label for="intro">个人简介：</label>
    <textarea id="intro" name="intro" rows="4" cols="30" placeholder="简单介绍一下自己..."></textarea><br><br>
    
    <input type="submit" value="立即注册">
    <input type="reset" value="重置表单">
    
    </form>
    ```

3. 效果展示
    <form action="/register" method="post" name="registerForm">
    
    <label for="username">用户名：</label>
    <input type="text" id="username" name="username" placeholder="请设置用户名" required autofocus><br><br>
    
    <label for="email">邮箱：</label>
    <input type="email" id="email" name="email" placeholder="请输入有效邮箱" required><br><br>
    
    <label for="password">密码：</label>
    <input type="password" id="password" name="password" placeholder="请设置密码" required><br><br>
    
    <label>性别：</label>
    <label for="male"><input type="radio" id="male" name="gender" value="male"> 男</label>
    <label for="female"><input type="radio" id="female" name="gender" value="female"> 女</label><br><br>
    
    <label for="hobbies">兴趣爱好：</label>
    <label for="reading"><input type="checkbox" id="reading" name="hobby" value="reading"> 阅读</label>
    <label for="sports"><input type="checkbox" id="sports" name="hobby" value="sports"> 运动</label><br><br>
    
    <label for="city">所在城市：</label>
    <select id="city" name="city">
        <option value="">--请选择--</option>
        <option value="bj">北京</option>
        <option value="sh">上海</option>
    </select><br><br>
    
    <label for="intro">个人简介：</label>
    <textarea id="intro" name="intro" rows="4" cols="30" placeholder="简单介绍一下自己..."></textarea><br><br>
    
    <input type="submit" value="立即注册">
    <input type="reset" value="重置表单">
    
    </form>
    <br/><br/>

#### 表单格式化
1. 表单格式化
    ```html
    <form action="/register" method="post" name="registerForm">
    <h1 align="center">用户注册</h1>
    <table name="table01" align="center">
        <tr>
        <td>用户名：</td>
        <td><input type="text" name="username" placeholder="请设置用户名" required autofocus></td>
        </tr>
        <tr>
        <td>邮箱：</td>
        <td><input type="email" name="email" placeholder="请输入有效邮箱" required></td>
        </tr>
        <tr>
        <td>密码：</td>
        <td><input type="password" name="password" placeholder="请设置密码" required></td>
        </tr>
        <tr>
        <td>性别：</td>
        <td>
            <label for="male"><input type="radio" id="male" name="gender" value="male"> 男</label>
            <label for="female"><input type="radio" id="female" name="gender" value="female"> 女</label>
        </td>
        </tr>
        <tr>
        <td>兴趣爱好：</td>
        <td>
            <label for="reading"><input type="checkbox" id="reading" name="hobby" value="reading"> 阅读</label>
            <label for="sports"><input type="checkbox" id="sports" name="hobby" value="sports"> 运动</label>
        </td>
        </tr>
        <tr>
        <td>所在城市：</td>
        <td>
            <select id="city" name="city">
            <option value="">--请选择--</option>
            <option value="bj">北京</option>
            <option value="sh">上海</option>
            </select>
        </td>
        </tr>
        <tr>
        <td>个人简介：</td>
        <td><textarea id="intro" name="intro" rows="4" cols="30" placeholder="简单介绍一下自己..."></textarea><br><br></td>
        </tr>
        <tr>
        <td><input type="submit" value="立即注册"></td>
        <td><input type="reset" value="重置表单"></td>
        </tr>
    </table>
    </form>
    ```

3. 效果展示
    <form action="http://www.lcq.com" method="get" name="registerForm2">
    <h1 align="center">用户注册</h1>
    <table name="table01" align="center">
        <tr>
        <td>用户名：</td>
        <td><input type="text" name="username" placeholder="请设置用户名" required autofocus></td>
        </tr>
        <tr>
        <td>邮箱：</td>
        <td><input type="email" name="email" placeholder="请输入有效邮箱" required></td>
        </tr>
        <tr>
        <td>密码：</td>
        <td><input type="password" name="password" placeholder="请设置密码" required></td>
        </tr>
        <tr>
        <td>性别：</td>
        <td>
            <label for="male2"><input type="radio" id="male2" name="gender" value="male"> 男</label>
            <label for="female2"><input type="radio" id="female2" name="gender" value="female"> 女</label>
        </td>
        </tr>
        <tr>
        <td>兴趣爱好：</td>
        <td>
            <label for="reading2"><input type="checkbox" id="reading2" name="hobby" value="reading"> 阅读</label>
            <label for="sports2"><input type="checkbox" id="sports2" name="hobby" value="sports"> 运动</label>
        </td>
        </tr>
        <tr>
        <td>所在城市：</td>
        <td>
            <select id="city" name="city">
            <option value="">--请选择--</option>
            <option value="bj">北京</option>
            <option value="sh">上海</option>
            </select>
        </td>
        </tr>
        <tr>
        <td>个人简介：</td>
        <td><textarea id="intro" name="intro" rows="4" cols="30" placeholder="简单介绍一下自己..."></textarea><br><br></td>
        </tr>
        <tr>
        <td>头像</td>
        <td><input type="file" name="img"></td>        
        </tr>
        <tr>
        <td><input type="submit" value="立即注册"></td>
        <td><input type="reset" value="重置表单"></td>
        </tr>
    </table>
    </form>

#### 表单提交注意事项

1. action属性：设置提交的服务器地址/资源
2. method属性：当前阶段主要是GET（默认）和POST
3. name属性：这是保证服务器能够接收数据的重要属性。
    - 如果表单元素中没有name属性，这个元素不会被表单提交。
4. 对于选项，提交的是对应选项value属性的值。
    - 对于select/checkbox/radio，提交的数据是value指定的值。
    - 对于checkbox复选框，使用GET会产生信息诸如：`sport=xx&sport=yy`
5. 在同一个markdown文档中，不得使用for属性相同的label，否则会发生不可预测的错误。

6. GET请求特点
    - 在浏览器中的地址是：`form的action属性 [+?+请求参数]`
    - 请求参数的格式：`A=Avalue&B=Bvalue...`
    - 不安全
    - 有数据长度限制（一般为2k）
    - 如包含重要信息，不建议使用GET

7. POST请求特点
    - 浏览器地址栏只有action属性值
    - 相对于GET安全
    - 理论上没有数据长度限制
    - 请求数据同HTTP一同传输，在HTTP体中，相对安全。

#### div

1. 特点
    - `<div>`标签可以把文档分割为独立的、不同的部分。
    - `<div>`标签是一个块级元素，它的内容自动地开始一个新行。

#### p标签

1. 特点
    - `<p>`标签用于定义段落。
    - p元素会自动在其前后创建一些空白。

#### span标签

1. 特点
    - span标签是内联元素，不像块级元素（div、p）有换行效果。
    - 如果不对span应用样式，span标签没有任何的显示效果。
    - 语法：`<span>内容</span>`

2. 示例
    ```html
    购物车里有<span style="color:red font-size:40px">10</span>种商品。
    ```

    购物车里有<span style="color:red;font-size:40px">10</span>种商品。


### 作业

1. 雇员薪资信息
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>员工信息</title>
    <!-- 老师使用 css 完成[体验下]，同学们也可以使用前面的方法 -->
    <style type="text/css">
        table, tr, td {
        border: 2px solid blue;
        border-collapse: collapse;
        }

        /*设置 table 的背景色*/
        table {
        background-color: #CCCCCC;
        /*表格中的文字居中*/
        text-align: center;
        }
    </style>
    </head>
    <body>
    <!-- 老师分析
    1. 一共有 4 行 6 列
    -->
    <div align="center">
    <h1>雇员薪资信息</h1>
    <table border="1px" width="800">
        <tr>
        <th>第 1 行 1 列</th>
        <th>第 1 行 2 列</th>
        <th>第 1 行 3 列</th>
        <th>第 1 行 4 列</th>
        <th>第 1 行 5 列</th>
        <th>第 1 行 6 列</th>
        </tr>
        <tr>
        <td>第 1 行 1 列</td>
        <td>第 1 行 2 列</td>
        <td>第 1 行 3 列</td>
        <td>第 1 行 4 列</td>
        <td>第 1 行 5 列</td>
        <td>第 1 行 6 列</td>
        </tr>
        <tr>
        <td>第 1 行 1 列</td>
        <td>第 1 行 2 列</td>
        <td>第 1 行 3 列</td>
        <td>第 1 行 4 列</td>
        <td>第 1 行 5 列</td>
        <td>第 1 行 6 列</td>
        </tr>
        <tr>
        <td>第 1 行 1 列</td>
        <td>第 1 行 2 列</td>
        <td>第 1 行 3 列</td>
        <td>第 1 行 4 列</td>
        <td>第 1 行 5 列</td>
        <td>第 1 行 6 列</td>
        </tr>
    </table>
    </div>
    </body>
    </html>
    ```

## CSS（层叠样式表）

### 基本内容

#### 基本概念

1. 简介
    - CSS（Cascading Style Sheet）即层叠样式表
    - [文档](http://www.w3school.com.cn/css/index.asp)

2. 为什么需要CSS？
    - 在没有 CSS 之前，我们想要修改 HTML 元素的样式需要为每个 HTML 元素单独定义样式属性，费心费力。所以 CSS 就出现了。
    - 使用 CSS 将 HTML 页面的 内容与样式分离提高 web 开发的工作效率(针对前端开发)
    - CSS 可以让 html 元素(内容) + 样式(CSS)分离，更好的控制页面

#### CSS快速入门

1. 代码
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>css 快速入门</title>
    <!-- 老师解读
    1. 在 head 标签内，出现了 <style type="text/css"></style>
    2. 表示要写 css 内容
    3. div{} 表示对 div 元素进行样式的指定
    4. width: 300px(属性); 表示对 div 样式的具体指定, 可以有多个
    5. 如果有多个，使用; 分开即可, 最后属性可以没有; 但是建议写上
    6. 当运行页面时，div 就会被 div{} 渲染，修饰
    -->
    <style type="text/css">
    div {
    width: 200px;
    height: 100px;
    background-color: gold;
    }
    </style>
    </head>
    <body>
    <!--先使用传统的方法-->
    <div>hello, 北京</div>
    <br/>
    <div >hello, 上海</div>
    <br/>
    <div>hello, 天津</div>
    <br/>
    <div>hello, 深圳</div>
    <br/>
    </body>
    </html>
    ```

2. 效果
    ![javaweb_CSS_simple](./img/javaweb_CSS_simple.png)

#### CSS语法

1. 基本语法
    - CSS语法分为声明和选择器。
        ```html
        选择器{
            声明1;
            声明2;
            声明3;
            ...
        }
        ```
    - 声明包含属性和值。
        ```html
        h1{
            /*这里冒号前是属性名，冒号后是值*/
            font-size:12;
        }
        ```
    - 最后一个属性的分号可省略（但建议加上）
    - CSS中的注释使用：`/**/`包裹。

### 常用样式

#### 字体颜色

1. 颜色的三种表达方式
    - 使用颜色名，如：`red`
    - 使用16进制表示，如：`#0000FF`。
    - 使用三原色，如：`rgb(255,0,0)`

2. 演示
    - `<div style="color: red;">这是红色</div>`
        <div style="color: red;">这是红色</div>
    - `<div style="color: #00FF00;">这是绿色</div>`
        <div style="color: #00FF00;">这是绿色</div>
    - `<div style="color: rgb(0,0,255);">这是蓝色</div>`
        <div style="color: rgb(0,0,255);">这是蓝色</div>


#### 边框

1. css演示
    ```html
    div{
        <!-- 不同的值不分先后，比如border中的1px、solid、blue -->
        border: 1px solid blue;
        width: 300px;
        height: 100px;
    }
    ```

2. 演示
    <div 
    style=" color: red; 
            border: 1px solid blue;
            width: 300px;
            height: 100px">早上好</div>
            
3. 边框样式
    dashed(虚线), dotted(点线), double(双线), none(默认，无)
    - dotted​​：点状边框
    - dashed​​：虚线边框
    - double​​：双线边框。两条线及其间隔之和等于指定的 border-width值
    - groove​​：3D 凹槽效果。效果取决于边框颜色
    - ridge​​：3D 垄状轮廓效果。效果取决于边框颜色
    - inset​​：使元素内容看起来内嵌。效果取决于边框颜色
    - outset​​：使元素内容看起来外凸。效果取决于边框颜色
    - none​​：默认无边框
    - ​hidden​​：隐藏边框（在表格中用于解决边框冲突）

4. 你也可以使用针对特定边的简写属性（border-top, border-right, border-bottom, border-left）：
    ```html
    你的选择器 {
    border-bottom: 2px dashed red; /* 只为下边框设置2像素红色虚线 */
    }
    ```
#### 宽度和高度

1. 语法
    - 宽度和高度可以用像素表示
        ```html
        div{
            width: 300px;
            height: 100px;
        }
        ```
    - 还可以用百分比表示，这种方式下，宽高会随网页大小同步改变。
        ```html
        div{
            width: 50%;
            height: 20%;
        }
        ```
2. 演示
    <div 
    style=" color: red; 
            border: 1px solid blue;
            width: 50%;
            height: 20%;
            background-color: #93f254ff;">早上好</div>

#### 背景颜色

1. CSS演示
    ```html
    div{
        background-color: #82af63ff
    }
    ```

#### 字体样式

1. 介绍
    - `font-size`：指定大小，可以按照像素大小。
    - `font-weight`：指定是否粗体。
    - `font-family`：指定类型。

2. 示例
    ```html
    div{
        font-size: 40px;
        font-weight: bold;
        font-family: 仿宋;
    }
    ```
    <div style="
        font-size: 40px;
        font-weight: bold;
        font-family: 仿宋;
        border: 1px solid red;
        width: 150px;
        margin-left: auto;
        margin-right: auto;
        text-align: center">
    早上好</div>
    
#### div居中

1. CSS实现
    ```css
    div{
        margin-left: auto;
        margin-right: auto;
    }
    ```

#### 文本居中

1. CSS实现
    ```css
    div{
        text-align: center;
    }
    ```

#### 超链接去下划线

1. CSS实现
    ```css
    a{
        text-decoration: none
    }
    ```
    <a href="http://www.github.com/LCQaha" style="text-decoration: none;">GitHub by LCQaha</a>

#### 表格细线

1. 示例
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>表格细线</title>
    <style type="text/css">
    /*
    设置边框 : border: 1px solid black
    将边框合并: border-collapse: collapse;
    指定宽度： width
    设置边框： 给 td, th 指定即可 border: 1px solid black;
    1. table, tr, td 表示组合选择器
    2. 就是 table 和 tr 还有 td ,都用统一的样式指定, 可以提高复用性
    */
    table, tr, td {
    width: 300px;
    border: 1px solid black;
    border-collapse: collapse;
    }
    </style>
    </head>
    <body>
    <table>
    <tr>
    <td align=center colspan="3">星期一菜谱</td>
    </tr>
    <tr>
    <td rowspan=2>素菜</td>
    <td>青草茄子</td>
    <td>花椒扁豆</td>
    </tr>
    <tr>
    <td>小葱豆腐</td>
    <td>炒白菜</td>
    </tr>
    <tr>
    <td rowspan=2>荤菜</td>
    <td>油闷大虾</td>
    <td>海参鱼翅</td>
    </tr>
    <tr>
    <td>红烧肉</td>
    <td>烤全羊</td>
    </tr>
    </table>
    </body>
    </html>
    ```

#### 列表去修饰

1. CSS实现
    - 去掉无序列表前的小圆点。
    ```css
    ul{
        list-style: none;
    }
    ```

### CSS三种使用方式

#### 从标签设定
1. 介绍
    - 在标签的style属性上设置CSS样式。
    - 样式多了后，可读性差、代码量大，维护性差。

#### 从head标签设定
1. 介绍
    - 在head标签中使用style标签定义需要的CSS样式。
    - 
#### 从外部文件引入
1. 介绍
    - 把CSS样式写成单独的CSS文件，通过link标签引入。
        `<link rel="stylesheet" href="./css/mycss.css">`

2. 示例
    - HTML
        ```html
        <!DOCTYPE html>
        <html lang="en">
        <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <!-- rel:relation -->
        <link rel="stylesheet" href="./css/mycss.css">
        </head>
        <body>
        <div>你好</div>
        </body>
        </html>
        ```
    - css
        ```css
        div{
        font-size: 40px;
        font-weight: bold;
        font-family: 仿宋, serif;
        border: 1px solid red;
        width: 150px;
        margin-left: auto;
        margin-right: auto;
        text-align: center;
        }
        ```

### CSS选择器

#### CSS元素选择器

1. 介绍
    - 最常见的 CSS 选择器是元素选择器。换句话说，文档的元素就是最基本的选择器。
    - CSS 元素/标签选择器通常是某个 HTML 元素， 比如 p、h1、a、div 等。
    - 元素选择器会默认修饰所有默认元素。
    ```css
    h1{color:blue;}
    h2{color:yellow;}
    ...
    ```

#### CSS ID选择器

1. 介绍
    - id 选择器可以为标有特定 id 的 HTML 元素指定特定的样式。
    - id 选择器以 `#` 来定义。

2. 示例
    - CSS
        ```css
        #css1{color: red;}
        #css2{color: blue;}
        ...
        ```
    - HTML
        ```html
        <!-- 这里每个id只能使用一次 -->
        <h1 id=css1>123</h1>
        <p id=css2>456</p>
        ```

3. 注意事项
    - 使用 id 选择器，需要先在要修饰元素指定 id 属性, id 值是程序员自己指定。
    - id 是唯一的，不能重复。
    - 在`<style>`标签中指定 id 选择器时，前面需要有 `#id` 值。


#### class 选择器（类选择器）

1. 介绍
    - class类选择器，可以通过class属性选择去使用这个样式。
    - 元素选择器只能一次性修饰所有标签，id选择器只能修饰一个标签，现在缺少一个可以修饰多个而非全部的选择器。
2. 基本语法
    ```css
    .class 属性值{属性:值;}
    ```
3. 示例
    - CSS
        ```css
        .css1{color: red;}
        .css2{color: blue;}
        ```
    - HTML
        ```html
        <h1 class=css1>123</h1>
        <p class=css2>456</p>
        ```

4. 注意事项
    - 使用 class 选择器，需要在被修饰的元素上，设置 class 属性，属性值程序员指定。
    - class 属性的值，**可以重复**。
    - 需要在 `<style></style>` 指定类选择器的具体样式, 前面需要是 `.类选择器名称`。

#### 组合选择器

1. 介绍
    - 组合选择器可以让多个选择器共用同一个 css 样式代码。

2. 基本语法
    ```css
    选择器1, 选择器2,..., 选择器n{
        属性: 值;
    } 
    ```

3. 示例
    ```css
    .class01, #id01{
        color: #ff0000;
    }
    ```

#### 选择器优先级

1. 优先级顺序
    `行内样式 > ID 选择器 > class选择器 > 元素选择器` （从特殊到一般）

2. 综合示例
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        p{
        color: red;
        }
        #id01{
        color: green;
        }
        .class01{
        color: blue;
        }

        #id02, .class02{
        color: aqua;
        }
    </style>
    </head>
    <body>
    <!--p:red-->
    <p>666</p>
    <!--class:blue-->
    <p class="class01">666</p>
    <!--id01:green-->
    <p id="id01" class="class01">666</p>
    <!--style:chartreuse-->
    <p style="color: chartreuse" id="id02" class="class01">666</p>
    </body>
    </html>
    ```

## JavaScript

### 基本内容

#### 基本介绍

1. 官方文档
    - https://www.w3school.com.cn/js/index.asp

2. 介绍
    - JavaScript 能改变 HTML 内容，能改变 HTML 属性，能改变 HTML 样式 (CSS)，能完成页面的数据验证。
    - HTML控制内容，CSS控制样式，js控制行为。

3. 简单案例
    - markdown不支持js，故不能直接演示。
    ```html
    <button onclick="document.getElementById('myImage').src='./img/eg_bulbon.gif'"> 开 灯
    </button>
    <img id="myImage" border="0" src="./img/eg_bulboff.gif" style="text-align:center;">
    <button onclick="document.getElementById('myImage').src='./img/eg_bulboff.gif'"> 关 灯
    </button>
    ```

#### JS特点

1. 特点
    - JavaScript 是一种解释型的脚本语言，C、C++等语言先编译后执行，而 JavaScript 是在程序的运行过程中逐行进行解释。
    - JavaScript 是一种基于对象的脚本语言，可以创建对象，也能使用现有的对象(有对象)。
    - JavaScript 是**弱类型**的，对变量的数据类型不做严格的要求，变量的数据类型在运行过程可以变化。

2. 注意事项
    - js 代码可以写在 script 标签中
    - type="text/javascript" 表示这个脚本(script)类型是 javascript
    - type="text/javascript" 可以不写，但是建议写上
    - js 语句可以不写 ; 建议写上

3. 体现弱类型
    ```html
    <!--输出方式
    alert()：弹框
    console.log()：在调试输出（控制台）
    -->
    <script>
    // age=10
    // number
    // age=北京
    // string
    // 123abc
    // string
    var age = 10;//数值
    console.log("age=" + age)
    console.log(typeof age);
    age = "北京";
    console.log("age=" + age)
    console.log(typeof age);
    var n = 123 + "abc";
    console.log(n);
    console.log(typeof n);
    </script>
    ```

#### JS快速入门

1. 方式1：使用`<script>`标签
    - 可以出现在`<head>`和`<body>`中嵌入。
    ```HTML
    <head>
        <script type="text/javascript">
            console.log("ok");
        </script>
    </head>
    <body>
        <script type="text/javascript">
            console.log("hi~");
        </script>
    </body>
    ```
2. 方式2：使用`<script>`引入JS文件。
    - 此种方式下，`<script>`中的代码会因引用的外部文件被完全忽略，若想额外添加代码，需要另起一个`<script>`。
    ```html
    <script type="text/javascript" src="./js/my.js">
        // 这里的代码不会执行
    </script>
    <script>
        // 这里的代码可以执行
    </script>
    ```

#### 查看JS错误信息

1. 使用Chrome浏览器查看错误信息。
    - 进入开发者工具（`Ctrl+Shift+I`）
    - 在控制台查看错误信息，并点击跳转到`源代码`页面查看具体代码信息。

2. 使用火狐（FireFox）浏览器查看错误信息。
    - 在控制台查看错误信息。
    - 在调试器页面查看具体错误信息。

### 变量

1. JS变量表示存储数据的容器。
    在内存中：变量（变量名）->数据空间（值）；

2. 变量定义
    - 不是必须写类型。

#### 数据类型

1. 常用数据类型
    - `number`：数值类型（小数、整数）
    - `string`：字符串类型（字符、字符串）
    - `object`：对象类型
    - `boolean`：布尔类型
    - `function`：函数类型
    - 使用`typeof`查看类型。

2. 特殊值
    - `undefined`：变量未赋初始值，默认为`undefined`
    - `null`：空值
    - `NaN`：Not a Number，非数值
    ```js
    var a;// undefined
    var b=null;//null
    console.log(10*"abc");//NaN
    ```

3. 数据类型注意事项
    - string可以用`''`和`""`两种方式包裹字符串。

### 运算符

#### 算术运算符   
1. 符号表
    - 假设y=5

    | 运算符     | 描述               | 例子        | 结果      |
    | :--------- | :----------------- | :---------- | :-------- |
    | `+`        | 加                 | `x=y+2`     | `x=7`     |
    | `-`        | 减                 | `x=y-2`     | `x=3`     |
    | `*`        | 乘                 | `x=y*2`     | `x=10`    |
    | `/`        | 除                 | `x=y/2`     | `x=2.5`   |
    | `%`        | 求余数(保留整数)   | `x=y%2`     | `x=1`     |
    | `++`       | 累加               | `x=++y`     | `x=6`     |
    | `--`       | 递减               | `x=--y`     | `x=4`     |
    
#### 赋值运算符
1. 符号表
    x=10,y=5
    | 运算符 | 例子      | 等价于    | 结果   |
    | :----- | :-------- | :-------- | :----- |
    | `=`    | `x = y`   |           | `x = 5` |
    | `+=`   | `x += y`  | `x = x + y` | `x = 15` |
    | `-=`   | `x -= y`  | `x = x - y` | `x = 5` |
    | `*=`   | `x *= y`  | `x = x * y` | `x = 50` |
    | `/=`   | `x /= y`  | `x = x / y` | `x = 2` |
    | `%=`   | `x %= y`  | `x = x % y` | `x = 0` |

#### 关系运算符
1. 符号表
    | 运算符 | 描述           | 例子                  |
    | :----- | :------------- | :-------------------- |
    | `==`   | 等于           | `x == 8` 为 `false`   |
    | `===`  | 全等（值和类型） | `x === 5` 为 `true`<br>`x === "5"` 为 `false` |
    | `!=`   | 不等于         | `x != 8` 为 `true`    |
    | `>`    | 大于           | `x > 8` 为 `false`    |
    | `<`    | 小于           | `x < 8` 为 `true`     |
    | `>=`   | 大于或等于     | `x >= 8` 为 `false`   |
    | `<=`   | 小于或等于     | `x <= 8` 为 `true`    |

    *比较运算基于 `x = 5` 的值*

2. 示例
    ```js
    var a = 100;
    var b = "100";

    console.log(a == b);    // true
    console.log(a === b);   // false
    ```

#### 逻辑运算符
1. 符号表    
    | 运算符 | 描述      | 例子                  | 结果    |
    | :----- | :-------- | :-------------------- | :------ |
    | `&&`   | 逻辑与    | `(x < 10 && y > 1)`   | `true`  |
    | `\|\|`   | 逻辑或    | `(x == 5 \|\| y == 5)`  | `false` |
    | `!`    | 逻辑非    | `!(x == y)`           | `true`  |

    *假设值: `x = 6`, `y = 2`*
2. 注意事项
    - 在 JavaScript 语言中，所有的变量，都可以做为一个 boolean 类型的变量去使用。
    - `0` 、`null`、 `undefined`、`""`(空串)、`NaN` 都认为是 false
    - `&&` 与运算，有两种情况：（短路机制）
        - 对于`a && b`，若`a`为`false`，则**表达式`b`不执行**，直接返回表达式`a`的值。
        - 若`a`为`true`，返回`b`的值。
    - `||` 或运算, 有两种情况：（短路机制）
        - 对于`a || b`，若`a`为`true`，则**表达式`b`不执行**，直接返回表达式`a`的值。
        - 若`a`为`false`，返回`b`的值。
    - `&&` 运算 和 `||` 运算 有短路现象。

#### 条件运算符

1. 三元运算符：`a:b?c`

### 数组

#### 数组定义

1. 数组定义有4种方式

2. 方式1：中括号
    ```js
    var arr1 = ["a", "b", "c", 666];
    console.log("arr1 = " + arr1);// arr1 = a,b,c,666
    console.log("arr1[1] = " + arr1[1]);// arr1[1] = b    
    ```
3. 方式2：先定义，再赋值
    ```js
    var arr2 = [];
    arr2[0]=1;
    arr2[1]=2;
    arr2[5]=3;
    arr2[0]=4;
    alert(arr2);// 4,2,,,,3，不显示的是undefined
    alert(arr2[4]);// undefined
    ```
4. 方式3：`new`
    ```js
    var arr3 = new Array("a","b","c");
    console.log(arr3);
    alert(arr3);  // a,b,c
    console.log(arr3[1]);// b
    ```

5. 方式4：空new
    ```js
    var arr4 = new Array();
    console.log(typeof arr4);   // object
    arr4[0] = 'b';
    ```

#### 数组遍历

1. 示例
    ```js
    var arr1=[1,2,33,4,5,5,7];
    console.log("数组的长度："+arr1.length);
    for (let i = 0; i < arr1.length; i++) {
        console.info(arr1[i]);
    }
   ```

### JS函数

#### 基本概念

1. 函数是由事件驱动的，或者当其被调用时执行的，可重复使用的代码块。

2. 引例
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        function hi(){
        alert("hi~");
        }
    </script>
    </head>
    <body>
    <button onclick="hi()">点击这里</button>
    </body>
    </html>
    ```

#### 函数定义

1. 方式1：`function`关键字
    ```js
    function 函数名(形参列表){
        函数体;
        return 表达式;
    }
    ```

    ```js
    // 没有返回值的函数
    // 定义无返回值的函数
    function f1(){
        alert("f1()");
    }
    // 调用函数
    f1();

    // 这里的形参不需要指定类型，传什么类型就是什么类型
    function f2(name){
        alert("hi " + name)
    }
    f2(666); // hi 666
    f2("早上好"); // hi 早上好

    function f3(x,y){
        return x+y;
    }
    // f3(1,10)=11
    alert("f3(1,10)="+f3(1,10));
    // f3('aha',666)aha666
    alert("f3('aha',666)"+f3('aha',666));
    ```

2. 方式2：将函数赋给变量
    ```js
    var 函数名 = function(形参列表){
        函数体;
        return 表达式;
    }
    ```

    ```js
    var f1 = function (){
        alert("f1()");
    }

    console.log(typeof f1);   // function
    console.log(typeof f1()); // undefined

    var f3=f1;
    f3();
    ```

#### 注意事项

1. JS 中函数的重载会覆盖掉上一次的定义
    ```js
    function f1(){
        alert("a");
    }
    function f1(name){
        alert("b "+name);
    }
    f1(); // b undefined
    ```
2. 函数的 arguments 隐形参数（作用域在 function 函数内）
    - 隐形参数: 在 function 函数中不需要定义，可以直接用来获取所有参数的变量。
    - 隐形参数特别像 java 的可变参数一样。 public void fun( int ... args )
    - js 中的隐形参数跟 java 的可变参数一样。操作类似数组
    - 如果函数有形参，传入实参按顺序匹配，多余的参数赋给`arguments`。
    - 如果传入实参个数不足，则剩余形参值为`undefined`。
    ```js
    function f1(){
        // 隐形变量是一个arguments数组
        console.warn("arguments.length="+arguments.length);
        console.log(arguments)
        for (let i = 0; i < arguments.length; i++) {
            console.log("arguments["+i+"]"+arguments[i]);
        }
    }
    f1(10,20,30);
    // arguments.length=3
    // Arguments()...
    // arguments[0]10
    // arguments[1]20
    // arguments[2]30
    ```

### 对象

#### 自定义对象

1. 对象的定义
    ```js
    var 对象名 = new Object();
    对象名.属性名 = value;
    对象名.函数名 = function(){};
    ```

2. 对象访问
    ```js
    对象名.属性;
    对象名.函数名();
    ```

3. 示例 
    ```js
    var obj = new Object();
    obj.name = "name";
    obj.age = 12;
    obj.func1 = function(){
        console.log(this.name);
        console.log(this.name2);
        console.log(name);
    }
    name = 666;
    obj.func1();
    // name
    // undefined
    // 666
    ```

#### 自定义对象方式2

1. 对象的定义
    ```js
    var 对象名 = {
        属性1 = value1;
        属性2 = value2;
        函数名: function(){
            ...
        }
    }
    ```

2. 对象访问
    ```js
    对象名.属性名;
    对象名.函数名;
    ```

3. 示例
    ```js
    var obj2 = {
        name: "name2",
        age: 222,
        func1: function(){
        console.log(this.name + this.age);
        }
    }
    obj2.func1();// name2222
    ```

4. 注意
    - 赋值用`:`冒号，而不是等号。
    - 分隔用`,`逗号，而不是分号。

### 事件

#### 基本内容

1. 官方文档
    https://www.w3school.com.cn/js/js_events.asp

2. 事件一览表 
    | 属性 | 当以下情况发生时，出现此事件 |
    |------|--------------------------|
    | onabort | 图像加载被中断 |
    | onblur | 元素失去焦点 |
    | onchange | 用户改变域的内容 |
    | onclick | 鼠标点击某个对象 |
    | ondblclick | 鼠标双击某个对象 |
    | onerror | 当加载文档或图像时发生某个错误 |
    | onfocus | 元素获得焦点 |
    | onkeydown | 某个键盘的键被按下 |
    | onkeypress | 某个键盘的键被按下或按住 |
    | onkeyup | 某个键盘的键被松开 |
    | onload | 某个页面或图像被完成加载 |
    | onmousedown | 某个鼠标按键被按下 |
    | onmousemove | 鼠标被移动 |
    | onmouseout | 鼠标从某元素移开 |
    | onmouseover | 鼠标被移到某元素之上 |
    | onmouseup | 某个鼠标按键被松开 |
    | onreset | 重置按钮被点击 |
    | onresize | 窗口或框架被调整尺寸 |
    | onselect | 文本被选定 |
    | onsubmit | 提交按钮被点击 |
    | onunload | 用户退出页面 |

#### 事件分类
1. 事件的注册（绑定）
    当事件响应(触发)后要浏览器执行哪些操作代码，叫事件注册或事件绑定。
2. 静态注册事件
    - 通过 html 标签的事件属性直接赋于事件响应后的代码，这种方式叫静态注册
    - 直接在HTML标签中通过`onclick`属性指定JavaScript代码：
        ```html
        <button onclick="alert('静态注册：你好！')">点击我（静态）</button>
        ```
    - 或者调用在`script`标签中定义的函数：
        ```html
        <script>
        function showStaticMessage() {
            alert('静态注册：你好！');
        }
        </script>
        <button onclick="showStaticMessage()">点击我（静态）</button>
        ```
    - **特点**：事件处理逻辑直接写在HTML属性中。

3. 动态注册事件
    - 通过 js 代码得到标签的 dom 对象，然后再通过 `dom 对象.事件名 = function(){}` 这种形式叫动态注册。
    - 动态注册的核心优势在于​​将行为（JavaScript）与结构（HTML）分离​​，这使得代码更易于维护和管理，也是现代前端开发的主流做法。
    - 通过JavaScript代码获取按钮的DOM对象，然后为其属性赋值：
        ```html
        <button id="btn">点击我（动态）</button>
        <script>
        // 1. 获取标签的DOM对象
        var buttonObj = document.getElementById("btn"); 
        // 2. 为DOM对象的事件属性赋值一个函数
        buttonObj.onclick = function() { 
            alert("动态注册：你好！");
        };
        </script>
        ```
    - **特点**：事件处理逻辑在JavaScript中定义，与HTML结构分离。

#### 动态注册事件步骤
1. 获取标签<--->dom 对象
2. dom 对象.事件名 = fucntion(){}

#### 案例：onload加载完成事件

1. 简介
    Onload：某个页面或图像被完成加载。

2. 示例
    ```js
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>Title</title>
        <script>
            var func1 = function () {
                alert("加载完毕！！");
            }
            // 无论是直接给 window.onload赋值，还是通过 <body onload>属性设置，
            // 它们最终都是在操作 同一个window对象的onload属性

            window.onload = function(){
                alert("窗口");
            }

            // 这个地方的窗口不关闭，<body>块会因阻塞不执行。
            alert("阻塞");
        </script>
    </head>
    <!--后定义的onload会覆盖掉前面的window.onload-->
    <body onload="func1()">
    aha
    </body>
    </html>
    ```

#### 案例：onclick 鼠标单击事件
1. 示例
    ```js
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>Title</title>

    </head>
    <body>
        <button onclick="alert('OK')">
            say OK!
        </button>

        <button id = "sayHi">
            say Hi!
        </button>

        <script>
        var obj = document.getElementById("sayHi");
        obj.onclick = function (){
            alert("Hi");
        }
        </script>

    </body>
    </html>
    ```

2. 注意事项
    - 对于动态注册/绑定，获取id操作的script代码**需要写在相关标签之后**，否则，无法正确获取对象。

3. 改进版本
    - 上面的代码过于松散，可以用`windows.onload`让代码变得更具有结构性。
    ```js
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>
        window.onload = function (){
        var obj = document.getElementById("sayHi");
        obj.onclick = function (){
            alert("Hi");
        }
        }
    </script>
    </head>
    <body>
    <button onclick="alert('OK')">
    say OK!
    </button>

    <button id = "sayHi">
    say Hi!
    </button>
    </body>
    </html>
    ```

#### 案例：失去焦点事件

1. 简介
    - onblur：元素失去焦点。
    - 填写一个输入框时，如果光标离开输入框，就称为“失去焦点”。

2. 示例
    ```js
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>Onblur</title>
    <script type="text/javascript">
        // 静态绑定
        var upperCase01 = function (){
        var elementById = document.getElementById("upper01");
        elementById.value=elementById.value.toUpperCase()
        }
        window.onload = function (){
        var elementById = document.getElementById("upper02");
        elementById.onblur = function (){
            elementById.value = elementById.value.toUpperCase();
        }
        }
    </script>
    </head>
    <body>
    <label for="upper01">静态绑定</label>
    <input type="text" onblur="upperCase01()" id = "upper01">
    <label for="upper01">动态绑定</label>
    <input type="text" id = "upper02">
    </body>
    </html>
    ```


#### 案例：内容（域）发生改变事件

1. 简介
    - 当一个输入框的内容发生改变，其他内容随之发生改变

2. 示例
    ```js
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        window.onblur = function (){
        var elementById = document.getElementById("sex");
        elementById.onchange = function (){
            alert("性别修改了");
        }
        }
    </script>
    </head>
    <body>
    <label for="sal">工资水平</label>
    <select id="sal" onchange="alert('内容变化了~')">
    <option>--请选择--</option>
    <option>1k以下</option>
    <option>1k~3k</option>
    <option>3k以上</option>
    </select>

    <label for="sex">性别</label>
    <select id="sex">
    <option>--请选择--</option>
    <option>男</option>
    <option>女</option>
    </select>
    </body>
    </html>
    ```

#### 案例（重点）：表单提交事件

1. 简介
    - 当表单提交时，触发。
    - 可用于验证表单信息是否正确。

2. 示例
    - 静态注册时，可以通过`"return Xxx()"`令相关的事件获取对应的布尔值。这里的布尔值会影响提交是否成功。
    - 动态注册时，可以通过`form`的id和内部元素的name组合获取相应的内容。
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>onsubmit 表单提交事件</title>
    <script type="text/javascript">
    //静态注册表单提交事件
    function register() {
    //先得到输入的用户名和密码
    var username = document.getElementById("username");
    var pwd = document.getElementById("pwd");
    //判断是否为空""
    if ("" == username.value || "" == pwd.value) {
    alert("用户名和密码不能为空, 不能提交");
    return false;//不提交
    }
    //表示要提交
    return true;
    }
    //动态注册表单提交事件
    window.onload = function () {
    //使用折半法, 观察原页面是否真的是最新的, 是不是修改的页面和访
    问的页面一致
    //得到 from2 表单 dom 对象
    var form2 = document.getElementById("form2");
    // //给 form2 绑定 onsubmit 事件
    // 老韩解释 onsubmit 绑定的函数，会直接将结果(f,t)返回给 onsubmit
    form2.onsubmit = function () {
    if(form2.username.value == "" || form2.pwd.value == "") {
    alert("用户名和密码不能为空, 不能提交");
    return false;//不提交
    }
    return true;
    }
    }
    </script>
    </head>
    <body>
    <h1>注册用户 1</h1> <!-- 静态注册表单提交事件 -->
    <form action="ok.html" onsubmit="return register()">
    u: <input type="text" id="username" name="username"/><br/>
    p: <input type="password" id="pwd" name="pwd"/><br/>
    <input type="submit" value="注册用户"/>
    </form>
    <h1>注册用户 2</h1> <!-- 动态注册表单提交事件 -->
    <form action="ok.html" id="form2">
    u: <input type="text" name="username"/><br/>
    p: <input type="password" name="pwd"/><br/>
    <input type="submit" value="注册用户"/></form>
    </body>
    </html>
    ```
#### 作业
1. 示例
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>onsubmit 表单提交事件</title>
    <script type="text/javascript">
    //动态注册表单提交事件
    window.onload = function () {
    //使用折半法, 观察原页面是否真的是最新的, 是不是修改的页面和访问的页面一致
    //得到 from2 表单 dom 对象
    var form2 = document.getElementById("form2");
    // //给 form2 绑定 onsubmit 事件
    // 老韩解释 onsubmit 绑定的函数，会直接将结果(f,t)返回给 onsubmit
    form2.onsubmit = function () {
    //过关斩将
    if (!(form2.username.value.length >= 4 && form2.username.value.length
    <= 6)) {
    alert("用户名长度(4-6)");
    return false;//不提交
    }
    if (form2.pwd.value.length != 6) {
    alert("密码长度(6)");
    return false;//不提交
    }
    if (form2.pwd.value != form2.pwd2.value) {
    alert("两次密码不等");
    return false;//不提交
    }
    //电子邮件->正则表达式 ^[\\w-]+@([a-zA-Z]+\\.)+[a-zA-Z]+$
    var emailPatt = /^[\w-]+@([a-zA-Z]+\.)+[a-zA-Z]+$/;
    if (!emailPatt.test(form2.email.value)) {
    //4 提示用户
    alert("电子邮件格式不正确~")
    return false;
    }
    return true;
    }
    }
    </script>
    </head>
    <body>
    <h1>注册用户</h1> <!-- 动态注册表单提交事件 -->
    <form action="ok.html" id="form2">
    用户名: <input type="text" name="username"/>长度(4-6)<br/>
    密 码: <input type="password" name="pwd"/>长度(6)<br/>
    确 认: <input type="password" name="pwd2">长度(6)<br/>
    电 邮: <input type="text" name="email">满足基本格式<br/>
    <input type="submit" value="注册用户"/></form>
    </body>
    </html>
    ```
2. deepseek示例
    ```html
    <!DOCTYPE html>
    <html lang="zh-CN">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>用户注册</title>
        <style>
            * {
                box-sizing: border-box;
                margin: 0;
                padding: 0;
                font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            }
            
            body {
                background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
                display: flex;
                justify-content: center;
                align-items: center;
                min-height: 100vh;
                padding: 20px;
            }
            
            .container {
                background-color: white;
                border-radius: 10px;
                box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
                width: 100%;
                max-width: 450px;
                padding: 40px;
            }
            
            h1 {
                text-align: center;
                color: #333;
                margin-bottom: 30px;
                font-weight: 600;
            }
            
            .form-group {
                margin-bottom: 20px;
            }
            
            label {
                display: block;
                margin-bottom: 8px;
                color: #555;
                font-weight: 500;
            }
            
            input[type="text"],
            input[type="password"],
            input[type="email"] {
                width: 100%;
                padding: 12px 15px;
                border: 1px solid #ddd;
                border-radius: 5px;
                font-size: 16px;
                transition: border-color 0.3s;
            }
            
            input[type="text"]:focus,
            input[type="password"]:focus,
            input[type="email"]:focus {
                border-color: #667eea;
                outline: none;
                box-shadow: 0 0 0 2px rgba(102, 126, 234, 0.2);
            }
            
            .error-message {
                color: #e74c3c;
                font-size: 14px;
                margin-top: 5px;
                display: none;
            }
            
            .submit-btn {
                width: 100%;
                padding: 12px;
                background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
                color: white;
                border: none;
                border-radius: 5px;
                font-size: 16px;
                font-weight: 600;
                cursor: pointer;
                transition: transform 0.2s;
            }
            
            .submit-btn:hover {
                transform: translateY(-2px);
            }
            
            .submit-btn:active {
                transform: translateY(0);
            }
            
            .login-link {
                text-align: center;
                margin-top: 20px;
                color: #666;
            }
            
            .login-link a {
                color: #667eea;
                text-decoration: none;
            }
            
            .login-link a:hover {
                text-decoration: underline;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <h1>用户注册</h1>
            <form id="registerForm" action="#" method="post" onsubmit="return validateForm()">
                <div class="form-group">
                    <label for="username">用户名</label>
                    <input type="text" id="username" name="username" placeholder="请输入用户名">
                    <div class="error-message" id="usernameError">用户名不能为空</div>
                </div>
                
                <div class="form-group">
                    <label for="email">电子邮箱</label>
                    <input type="email" id="email" name="email" placeholder="请输入电子邮箱">
                    <div class="error-message" id="emailError">请输入有效的电子邮箱地址</div>
                </div>
                
                <div class="form-group">
                    <label for="password">密码</label>
                    <input type="password" id="password" name="password" placeholder="请输入密码">
                    <div class="error-message" id="passwordError">密码长度至少6位</div>
                </div>
                
                <div class="form-group">
                    <label for="confirmPassword">确认密码</label>
                    <input type="password" id="confirmPassword" name="confirmPassword" placeholder="请再次输入密码">
                    <div class="error-message" id="confirmPasswordError">两次输入的密码不一致</div>
                </div>
                
                <button type="submit" class="submit-btn">注册</button>
            </form>
            
            <div class="login-link">
                已有账号？<a href="#">立即登录</a>
            </div>
        </div>

        <script>
            function validateForm() {
                // 获取表单元素值
                const username = document.getElementById('username').value.trim();
                const email = document.getElementById('email').value.trim();
                const password = document.getElementById('password').value;
                const confirmPassword = document.getElementById('confirmPassword').value;
                
                // 获取错误消息元素
                const usernameError = document.getElementById('usernameError');
                const emailError = document.getElementById('emailError');
                const passwordError = document.getElementById('passwordError');
                const confirmPasswordError = document.getElementById('confirmPasswordError');
                
                // 重置错误消息
                usernameError.style.display = 'none';
                emailError.style.display = 'none';
                passwordError.style.display = 'none';
                confirmPasswordError.style.display = 'none';
                
                let isValid = true;
                
                // 验证用户名
                if (username === '') {
                    usernameError.style.display = 'block';
                    isValid = false;
                }
                
                // 验证邮箱格式
                // 正则表达式以/^开头，以$/结尾，不要带引号！
                const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
                if (!emailRegex.test(email)) {
                    emailError.style.display = 'block';
                    isValid = false;
                }
                
                // 验证密码长度
                if (password.length < 6) {
                    passwordError.style.display = 'block';
                    isValid = false;
                }
                
                // 验证密码匹配
                if (password !== confirmPassword) {
                    confirmPasswordError.style.display = 'block';
                    isValid = false;
                }
                
                // 如果验证通过，可以提交表单
                if (isValid) {
                    alert('注册成功！');
                    // 在实际应用中，这里通常会发送数据到服务器
                    // 例如：document.getElementById('registerForm').submit();
                }
                
                return isValid;
            }
        </script>
    </body>
    </html>
    ```

## DOM（文档对象类型）

### 基本内容

1. 官方文档
    - www.w3school.com.cn/js/js_htmldom.asp

#### 基本介绍

1. DOM，Document Object Model 文档对象模型
2. 将文档中的标签、属性、文本转换成对象来管理。
3. DOM分三种：
    - html DOM（重点） 
    - CSS DOM 
    - XML DOM

#### HTML DOM

1. 当网页被加载时，浏览器会创建页面的DOM。
2. 示意图
    ![javaweb_DOM_structureTree](./img/javaweb_DOM_structureTree.png)

### Document对象

#### Document说明

1. 简介
    - Document管理所有的HTML文档内容。
    - Document是一种树结构文档。
    - 有层级关系，在DOM中把所有标签都对象化。
    - 通过Document可以访问所有的标签对象。

#### 获取对象

1. `getElementById()`
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        window.onload = function() {
        var elementById = document.getElementById("test01");
        alert(elementById);//[object HTMLParagraphElement]
        alert(elementById.innerText);// 你好
        alert(elementById.innerHTML);// <div>你好</div>
        }
    </script>
    </head>
    <body>
    <div id="test01"><div>你好</div></div>
    </body>
    </html>
    ```

#### 案例：多选项反选

1. 示例
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>getElementsByName 函数</title>
    <script type="text/javascript">
    //完成全选
    function selectAll() {
    //1.获取到 sport 这一组复选框
    var sports = document.getElementsByName("sport");
    //sports 是什么? 是 nodeList 即时一个集合
    //alert(sports);
    //2. 拿到[dom ,集合]，操作【属性和方法 api】泥瓦匠|工程师 清华
    // 遍历 sports， 修改
    for (var i = 0; i < sports.length; i++) {
    sports[i].checked = true;//选中
    }
    }
    //全不选
    function selectNone() {
    //1.获取到 sport 这一组复选框
    var sports = document.getElementsByName("sport");
    //sports 是什么? 是 nodeList 即时一个集合
    //alert(sports);
    //2. 拿到[dom ,集合]，操作【属性和方法 api】泥瓦匠|工程师 清华
    // 遍历 sports， 修改
    for (var i = 0; i < sports.length; i++) {
    sports[i].checked = false;//全部不选中
    }
    }
    //反选 selectReverse
    function selectReverse() {
    //1.获取到 sport 这一组复选框
    var sports = document.getElementsByName("sport");
    //2. 拿到[dom ,集合]，操作【属性和方法 api】泥瓦匠|工程师 清华
    // 遍历 sports， 修改
    for (var i = 0; i < sports.length; i++) {
    // if(sports[i].checked) {//js true
    // sports[i].checked = false;
    // } else {
    // sports[i].checked = true;//选中
    // }
    sports[i].checked = !sports[i].checked;
    }
    }
    </script>
    </head>
    <body>
    你会的运动项目：
    <input type="checkbox" name="sport" value="zq" checked="checked">足球
    <input type="checkbox" name="sport" value="tq">台球
    <input type="checkbox" name="sport" value="ppq">乒乓球 <br/><br/>
    <button onclick="selectAll()">全选</button>
    <button onclick="selectNone()">全不选</button>
    <button onclick="selectReverse()">反选</button>
    </body>
    </html>
    ```

2. by deepseek
    ```html
    <!DOCTYPE html>
    <html lang="zh-CN">
    <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>多选题表单示例</title>
    <style>
        body {
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        line-height: 1.6;
        color: #333;
        max-width: 800px;
        margin: 0 auto;
        padding: 20px;
        background-color: #f5f5f5;
        }
        .container {
        background-color: white;
        border-radius: 8px;
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        padding: 30px;
        margin-top: 20px;
        }
        h1 {
        color: #2c3e50;
        text-align: center;
        margin-bottom: 30px;
        }
        .question {
        margin-bottom: 20px;
        padding: 15px;
        background-color: #f8f9fa;
        border-radius: 5px;
        border-left: 4px solid #3498db;
        }
        .options {
        margin: 15px 0;
        }
        .option {
        display: block;
        margin: 10px 0;
        padding: 8px;
        cursor: pointer;
        }
        .option:hover {
        background-color: #e9f7fe;
        border-radius: 3px;
        }
        .option input {
        margin-right: 10px;
        cursor: pointer;
        }
        .buttons {
        margin-top: 25px;
        display: flex;
        gap: 10px;
        flex-wrap: wrap;
        }
        button {
        padding: 10px 15px;
        background-color: #3498db;
        color: white;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        font-size: 14px;
        transition: background-color 0.3s;
        }
        button:hover {
        background-color: #2980b9;
        }
        #submitBtn {
        background-color: #2ecc71;
        }
        #submitBtn:hover {
        background-color: #27ae60;
        }
        .result {
        margin-top: 20px;
        padding: 15px;
        border-radius: 5px;
        display: none;
        }
        .correct {
        background-color: #d4edda;
        color: #155724;
        border: 1px solid #c3e6cb;
        }
        .incorrect {
        background-color: #f8d7da;
        color: #721c24;
        border: 1px solid #f5c6cb;
        }
    </style>
    </head>
    <body>
    <div class="container">
    <h1>知识测试 - 多选题</h1>

    <form id="quizForm">
        <div class="question">
        <h3>题目：以下哪些是编程语言？（可多选）</h3>
        <div class="options">
            <label class="option">
            <input type="checkbox" name="question1" value="A"> Python
            </label>
            <label class="option">
            <input type="checkbox" name="question1" value="B"> HTML
            </label>
            <label class="option">
            <input type="checkbox" name="question1" value="C"> Java
            </label>
            <label class="option">
            <input type="checkbox" name="question1" value="D"> CSS
            </label>
            <label class="option">
            <input type="checkbox" name="question1" value="E"> JavaScript
            </label>
        </div>
        </div>

        <div class="buttons">
        <button type="button" id="selectAllBtn">全选</button>
        <button type="button" id="invertSelectionBtn">反选</button>
        <button type="button" id="clearSelectionBtn">清除选择</button>
        <button type="button" id="submitBtn">提交答案</button>
        </div>

        <div id="result" class="result"></div>
    </form>
    </div>

    <script>
    // 获取DOM元素
    const checkboxes = document.querySelectorAll('input[name="question1"]');
    const selectAllBtn = document.getElementById('selectAllBtn');
    const invertSelectionBtn = document.getElementById('invertSelectionBtn');
    const clearSelectionBtn = document.getElementById('clearSelectionBtn');
    const submitBtn = document.getElementById('submitBtn');
    const resultDiv = document.getElementById('result');

    // 正确答案（示例）
    const correctAnswers = ['A', 'C', 'E']; // Python, Java, JavaScript

    // 全选功能
    selectAllBtn.addEventListener('click', function() {
        checkboxes.forEach(checkbox => {
        checkbox.checked = true;
        });
    });

    // 清除选择
    clearSelectionBtn.addEventListener('click', function() {
        checkboxes.forEach(checkbox => {
        checkbox.checked = false;
        });
    });

    // 反选功能 - 核心实现
    invertSelectionBtn.addEventListener('click', function() {
        checkboxes.forEach(checkbox => {
        checkbox.checked = !checkbox.checked;
        });
    });

    // 提交答案
    submitBtn.addEventListener('click', function() {
        const userAnswers = [];
        checkboxes.forEach(checkbox => {
        if (checkbox.checked) {
            userAnswers.push(checkbox.value);
        }
        });

        // 验证答案
        const isCorrect = userAnswers.length === correctAnswers.length &&
        userAnswers.every((value, index) => value === correctAnswers[index]);

        // 显示结果
        resultDiv.style.display = 'block';
        if (isCorrect) {
        resultDiv.textContent = '恭喜！答案正确！';
        resultDiv.className = 'result correct';
        } else {
        resultDiv.textContent = `答案不正确。正确答案是：${correctAnswers.join(', ')}`;
        resultDiv.className = 'result incorrect';
        }
    });
    </script>
    </body>
    </html>
    ```

#### 案例：图片切换

1. 知识点
    - 使用`getElementsByTagName`获取所有对应标签的对象。
    - 使用`document.createElement()`创建标签，然后设置这个对象的属性，然后添加到body。（具体件示例2）
2. 示例1
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>getElementsByTagName</title>
    <script type="text/javascript">
    function changeImgs() {
    //1. 得到所有的 img
    var imgs = document.getElementsByTagName("img");
    //老师说 imgs 是 HTMLCollections
    alert("猫猫的数量是=" + imgs.length);
    //2. 修改 src,遍历修改
    for (var i = 0; i < imgs.length; i++) {
    imgs[i].src = "./img/" + (i+4) +".png";
    }
    //3 课后作业->再评讲
    //思路
    //(1) input 增加 id, 可以修改 value
    //(2) 根据 input 的 value 值来决定是切换猫还是狗 if -- else if ---
    //(3) 其它自己先思考
    }
    </script>
    </head>
    <body>
    <img src="./img/1.png" height="100">
    <img src="./img/2.png" height="100">
    <img src="./img/3.png" height="100">
    <br/>
    <input type="button" onclick="changeImgs()" value="查看多少小猫,并切换成小狗"/>
    </body>
    </html>
    ```

3. 示例2
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>createElement</title>
    <script type="text/javascript">
    function addImg() {//添加小猫
    // 在浏览器内存中 创建<img></img>
    var img = document.createElement("img");
    img.src = '1.png';
    img.height = '100';
    //添加到 body 标签
    document.body.appendChild(img);
    }
    </script>
    </head>
    <body>
    <input type="button" onclick="addImg()" value="点击创建一只小猫~"/>
    </body>
    </html>
    ```

### HTML DOM

#### 基本介绍

1. 在HTML DOM中，每部分都是节点
    - 文档本身是文档节点
    - 所有HTML元素是元素节点
    - 所有HTML属性是属性节点
    - HTML元素内的文本是文本节点
    - 注释是注释节点。

2. 文档
    https://www.w3school.com.cn/jsref/dom_obj_all.asp


#### 节点常用方法

1. 获取特定节点
    ```js
    document.getElementById("id");
    ```
2. 获取特定标签节点
    ```js
    document.getElementByTagName("Tag name");
    ```
3. 获取特定name节点
    ```js
    document.getElementByName("name");
    ```
4. 获取某标签下特定节点
    ```js
    document.getElementById("select01").getElementByTagName("option");
    ```
5. 获取某标签下所有节点
    ```js
    parentElement = document.getElementByName("name");
    // 获取除文本、注释的所有子节点
    parentElement.children;
    parentElement[i];
    // 获取包含文本、注释等在内的所有节点
    parentElement.childNodes;
    ```
6. 获取某标签下第一个子节点
    ```js
    // parentElement.children[0]
    parentElement.firstElementChild;
    // parentElement.childNode[0]
    parentElement.firstChild;
    ```
7. 获取某节点的父节点
    ```js
    childElement.parentElement;
    ```
8. 获取某节点的前后兄弟节点
    ```js
    // 选取的节点包含文本
    element.previousElementSibling;
    element.nextElementSibling
    ```
9. 获取某节点的value
    ```js
    element.value;
    ```

#### 案例：乌龟吃鸡

1. deepseek
    ```html
    <!DOCTYPE html>
    <html lang="zh-CN">
    <head>
    <meta charset="UTF-8">
    <title>乌龟吃鸡游戏</title>
    <style type="text/css">
        body {
        font-family: Arial, sans-serif;
        text-align: center;
        background-color: #f0f0f0;
        margin: 0;
        padding: 20px;
        }
        .game-container {
        margin: 0 auto;
        max-width: 500px;
        }
        h1 {
        color: #333;
        }
        table {
        border-collapse: collapse;
        margin: 20px auto;
        background-color: #fff;
        }
        td {
        width: 50px;
        height: 50px;
        border: 1px solid #ccc;
        text-align: center;
        font-size: 24px;
        }
        .turtle {
        background-color: green;
        color: white;
        }
        .chicken {
        background-color: yellow;
        color: black;
        }
        .controls {
        margin: 10px 0;
        }
        button {
        padding: 10px 15px;
        margin: 5px;
        font-size: 16px;
        cursor: pointer;
        background-color: #4CAF50;
        color: white;
        border: none;
        border-radius: 4px;
        }
        button:hover {
        background-color: #45a049;
        }
        #score {
        font-size: 20px;
        margin: 10px;
        color: #333;
        }
    </style>
    <script type="text/javascript">
        // 游戏变量
        let turtlePosition = { row: 2, col: 2 }; // 乌龟初始位置
        let chickenPosition = { row: 0, col: 0 }; // 鸡初始位置
        let score = 0; // 初始得分
        const gridSize = 5; // 网格大小 5x5

        // 页面加载完成后初始化游戏
        window.onload = function() {
        initGame();
        setupEventListeners();
        };

        // 初始化游戏：生成网格和随机鸡位置
        function initGame() {
        generateGrid();
        generateChicken();
        updateDisplay();
        }

        // 生成游戏网格
        function generateGrid() {
        const gameGrid = document.getElementById('gameGrid');
        gameGrid.innerHTML = ''; // 清空现有网格
        for (let i = 0; i < gridSize; i++) {
            const row = document.createElement('tr');
            for (let j = 0; j < gridSize; j++) {
            const cell = document.createElement('td');
            cell.id = `cell-${i}-${j}`;
            row.appendChild(cell);
            }
            gameGrid.appendChild(row);
        }
        }

        // 随机生成鸡的位置
        function generateChicken() {
        // 确保鸡不生成在乌龟当前位置
        let newRow, newCol;
        do {
            newRow = Math.floor(Math.random() * gridSize);
            newCol = Math.floor(Math.random() * gridSize);
        } while (newRow === turtlePosition.row && newCol === turtlePosition.col);

        chickenPosition = { row: newRow, col: newCol };
        }

        // 更新游戏显示：绘制乌龟和鸡
        function updateDisplay() {
        // 清除所有单元格
        for (let i = 0; i < gridSize; i++) {
            for (let j = 0; j < gridSize; j++) {
            const cell = document.getElementById(`cell-${i}-${j}`);
            cell.className = '';
            cell.textContent = '';
            }
        }

        // 绘制乌龟
        const turtleCell = document.getElementById(`cell-${turtlePosition.row}-${turtlePosition.col}`);
        turtleCell.className = 'turtle';
        turtleCell.textContent = '龟';

        // 绘制鸡
        const chickenCell = document.getElementById(`cell-${chickenPosition.row}-${chickenPosition.col}`);
        chickenCell.className = 'chicken';
        chickenCell.textContent = '鸡';

        // 更新得分显示
        document.getElementById('score').textContent = `得分: ${score}`;
        }

        // 移动乌龟
        function moveTurtle(direction) {
        let newRow = turtlePosition.row;
        let newCol = turtlePosition.col;

        switch (direction) {
            case 'up':
            newRow = Math.max(0, turtlePosition.row - 1);
            break;
            case 'down':
            newRow = Math.min(gridSize - 1, turtlePosition.row + 1);
            break;
            case 'left':
            newCol = Math.max(0, turtlePosition.col - 1);
            break;
            case 'right':
            newCol = Math.min(gridSize - 1, turtlePosition.col + 1);
            break;
        }

        turtlePosition = { row: newRow, col: newCol };
        checkEatChicken();
        updateDisplay();
        }

        // 检查是否吃到鸡
        function checkEatChicken() {
        if (turtlePosition.row === chickenPosition.row && turtlePosition.col === chickenPosition.col) {
            score++;
            generateChicken();
        }
        }

        // 设置按钮事件监听器
        function setupEventListeners() {
        document.getElementById('up').addEventListener('click', () => moveTurtle('up'));
        document.getElementById('down').addEventListener('click', () => moveTurtle('down'));
        document.getElementById('left').addEventListener('click', () => moveTurtle('left'));
        document.getElementById('right').addEventListener('click', () => moveTurtle('right'));
        }
    </script>
    </head>
    <body>
    <div class="game-container">
    <h1>乌龟吃鸡游戏</h1>
    <div id="score">得分: 0</div>
    <table id="gameGrid"></table>
    <div class="controls">
        <button id="up">向上</button><br>
        <button id="left">向左</button>
        <button id="right">向右</button><br>
        <button id="down">向下</button>
    </div>
    </div>
    </body>
    </html>

    ```

## XML

### 简介

1. 文档
    https://www.w3school.com.cn/xml/index.asp

2. XML(Extensible Markup Language，可扩展标记语言)

2. 为什么需要XML
    - 两个程序间进行数据通信。
    - 为服务器作配置文件。
    - 存储复杂的数据关系。
3. 应用
    - spring：ico配置文件，`beans.xml`
    - mybatis：`XXXMapper.xml`
    - tomcat：`server.xml` `web.xml`
    - maven：`pom.xml`

4. XML 用于解决什么问题？
    - 程序间数据传输，如QQ之间通过XML传输消息。
    - 作配置文件。
    - 充当小型数据库。（直接读取文件比访问数据库效率更高）

### 快速入门

1. 示例
    ```xml
    <!-- 
    xml:表示文件类型
    version:版本
    encoding:编码
     -->
    <?xml version="1.0" encoding="utf-8" ?>
    <!-- 
    students:根元素（root），
    根元素只有一个，具体名称自己定义 -->
    <students>
        <student id="1">
            <name>jack</name>
            <age>18</age>
            <gender>男</gender>
        </student>
        <student id="2">
            <name>jason</name>
            <age>19</age>
            <gender>男</gender>
        </student>
    </students>
    ```

### 语法

#### 基本部分
1. 文档声明
1. 元素
1. 属性
1. 注释
1. CDATA区、特殊字符

#### 文档声明
1. 特点
    - XML 声明放在文档第一行。
    - XML声明由`version`和`encoding`两部分构成。
    - `version="1.0"`代表文档符合XML1.0规范。

#### 元素
1. 根元素
    - 每个XML文档必须有且只有一个根元素。
    - 根元素是一个完全包括文档中其他所有元素的元素。
    - 根元素的起始标记要放在所有其他元素的起始标记之前。
    - 根元素的结束标记要放在所有其他元素的结束标记之后。

2. 标签
    - XML 元素指 XML 文件中出现的标签，一个标签分为开始标签和结束标签
    - 包含标签体写法：`<a>www.sohu.cn</a>`。
    - 不含标签体的：`<a></a>`, 简写为：`<a/>`。
    - 一个标签中也可以嵌套若干子标签。但所有标签必须合理的嵌套，绝对不允许交叉嵌。
3. 在很多时候，说 标签、元素、节点是相同的意思
4. XML 元素命名规则
    - 区分大小写，例如，`<P>`和`<p>`是两个不同的标记。
    - 不能以数字开头。
    - 不能包含空格。
    - 名称中间不能包含冒号（`:`）。
    - 如果标签单词需要间隔，建议使用下划线 比如 `<book_title>hello</book_title>`

#### 属性


1. 属性介绍
    - 属性值用双引号（`"`）或单引号（`'`）分隔（如果属性值中有`'`，用`"`分隔；有`"`，用`'`分隔）
    - 一个元素可以有多个属性，它的基本格式为：`<元素名 属性名="属性值">`
    - 特定的属性名称在同一个元素标记中只能出现一次
    - 属性值不能包括`&`字符

#### 注释

6.5.5 注释
1. `<!--这是一个注释-->`
2. 注释内容中不要出现- -；
3. 不要把注释放在标记中间；错误写法 `<Name <!--the name-->>TOM</Name>`
4. 注释不能嵌套
5. 可以在除标记以外的任何地方放注释。（不能再标签中放注释）

#### CDATA

1. 简介
    有些内容不想让解析引擎执行，而是当作原始内容处理(即当做普通文本)，可以使用 CDATA 包括起来，CDATA 节中的所有字符都会被当作简单文本，而不是 XML 标记。
1. 语法：
    ```xml
    <![CDATA[
    这里可以把你输入的字符原样显示，不会解析 xml
    ]]>
    ```
3. 注意
    - 可以输入任意字符（除`]]>`外）
    - 不能嵌套

#### 转义字符
1. 常用转义符号
    | 转义符    | 符号 |
    | :-------- | :--- |
    | `&lt;`   | <    |
    | `&gt;`   | >    |
    | `&amp;`  | &    |
    | `&quot;` | "    |
    | `&apos;` | '    |

2. 示例
    ```xml
    <age>年龄&lt;18</age>
    <!-- 年龄<0018 -->
    ```

### DOM4j

#### 基本内容

1. 文档
    https://dom4j.github.io/javadoc/1.6.1

2. 技术原理
    - 不管是 html 文件还是 xml 文件它们都是标记型文档，都可以使用 w3c 组织制定的dom 技术来解析。
    - document 对象表示的是整个文档（可以是 html 文档，也可以是 xml 文档）。

3. XML 解析技术介绍
    - 早期 JDK 为我们提供了两种 xml 解析技术 DOM 和 Sax 简介
        - dom 解析技术是 W3C 组织制定的，而所有的编程语言都对这个解析技术使用了自己语言的特点进行实现。 Java 对 dom 技术解析也做了实现
        - sun 公司在 JDK5 版本对 dom 解析技术进行升级：SAX（ Simple API for XML ） SAX解析，它是以类似事件机制通过回调告诉用户当前正在解析的内容。 是一行一行的读取 xml 文件进行解析的。不会创建大量的 dom 对象。 所以它在解析 xml 的时候，在性能上优于 Dom 解析
        - 这两种技术已经过时，知道有这两种技术即可
    - 第三方的 XML 解析技术
        - jdom 在 dom 基础上进行了封装
        - dom4j 又对 jdom 进行了封装。
        - pull 主要用在 Android 手机开发，是在跟 sax 非常类似都是事件机制解析 xml 文件

4. DOM4J简介
    - Dom4j 是一个简单、灵活的开放源代码的库(用于解析/处理 XML 文件)。Dom4j 是由早期开发 JDOM 的人分离出来而后独立开发的。
    - 与 JDOM 不同的是，dom4j 使用接口和抽象基类，虽然 Dom4j 的 API 相对要复杂一些，但它提供了比 JDOM 更好的灵活性。
    - Dom4j 是一个非常优秀的 Java XML API，具有性能优异、功能强大和极易使用的特点。现在很多软件采用的 Dom4j。
    - 使用 Dom4j 开发，需下载 dom4j 相应的 jar 文件
    - 下载链接：dom4j.github.io

#### 获取Document对象

1. 读取XML文件，获取document对象
    ```java
    SAXReader reader = new SAXReader(); //创建一个解析器
    Document document = reader.read(new File("src/input.xml"));//XML Document
    ```
2. 解析XML文本，得到document对象
    ```java
    String text = "<members></members>";
    Document document = DocumentHelper.parseText(text);
    ```
3. 主动创建document对象
    ```java
    Document document = DocumentHelper.createDocument(); //创建根节点
    Element root = document.addElement("members");
    ```

#### 加载XML文件

1. 代码
    ```java
    public class LoadXML01 {
        @Test
        public void loadXML() throws DocumentException {
            String url = "src/students.xml";
            loadXML(url);
            System.out.println(url);
        }

        public void loadXML(String url) throws DocumentException {
            SAXReader saxReader = new SAXReader();
            Document read = saxReader.read(new File(url));
            System.out.println(read);
        }
    }
    ```

2. 测试xml
    ```XML
    <?xml version="1.0" encoding="utf-8" ?>
    <students>
        <student id="1">
            <name>jack</name>
            <age>18</age>
            <gender>男</gender>
        </student>
        <student id="2">
            <name>jason</name>
            <age>19</age>
            <gender>男</gender>
        </student>
    </students>
    ```

3. 调试截图
    - 为什么会读取到5个元素？
        答：因为缩进也会被作为文本元素读取。
    - 在`document.content`中存储整个xml结构
    ![javaweb_XML_Debug01](./img/javaweb_XML_Debug01.png)

#### 遍历XML

1. 示例
    ```java
    @Test
    public void listXML() throws DocumentException {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read("src/students.xml");
        Element rootElement = document.getRootElement();
        List<Element> student = rootElement.elements("student");
        System.out.println(student.size()); // 2
        for (Element s : student) {
            // s.element("name").getText()等效于
            // s.elementText("name");
            Element name = s.element("name");
            Element age = s.element("age");
            Element address = s.element("address");
            String name1 = name.elementText("name");
            //org.dom4j.tree.DefaultElement@2cd2a21f [Element: <name attributes: []/>]
            //org.dom4j.tree.DefaultElement@2e55dd0c [Element: <age attributes: []/>]
            //null
            //null
            System.out.println(name);
            System.out.println(age);
            System.out.println(address);
            System.out.println(name1);

            System.out.println(name.getText());// 打印name
        }
    }
    ```

#### 指定读取XML元素

1. 示例
    ```java
    @Test
    public void listOneXML() throws DocumentException {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read("src/students.xml");
        Element rootElement = document.getRootElement();
        // 获取第一个学生
        Element student = (Element) rootElement.elements("student").get(0);
        // 获取id属性
        System.out.println(student.attributeValue("id"));
    }
    ```

#### 增删改

1. 增加示例
    ```java
    @Test
    public void addElement() throws DocumentException, IOException {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read("src/students.xml");
        Element newStudent = DocumentHelper.createElement("student");
        Element newAge = DocumentHelper.createElement("age");
        newAge.setText("18");
        Element newName = DocumentHelper.createElement("name");
        newName.setText("aha");
        Element newAddress = DocumentHelper.createElement("address");
        newAddress.setText("Beijing");

        // 整理树结构
        newStudent.add(newAge);
        newStudent.add(newName);
        newStudent.add(newAddress);
        document.getRootElement().add(newStudent);

        // 设置编码
        OutputFormat format = OutputFormat.createPrettyPrint();
        format.setEncoding("UTF-8");

        XMLWriter xmlWriter = new XMLWriter(new FileOutputStream(new File("src/students.xml")), format);

        xmlWriter.write(document);
        xmlWriter.close();
    }
    ```

2. 删除思路
    - 获取父节点
    - 获取要删除的节点
    - remove
    - 写入xml

3. 改思路
    - 读取后set

## TomCat

### 基本内容
1. 官方文档
    https://tomcat.apache.org/tomcat-8.0-doc/
2. WEB 开发介绍
    - WEB，在英语中 web 表示网/网络资源(页面,图片,css,js)意思，它用于表示 WEB 服务器(主机)供浏览器访问的资源
    - WEB 服务器(主机)上供外界访问的 Web 资源分为：
        - 静态 web 资源（如 html 页面）：指 web 页面中供人们浏览的数据始终是不变。
        - 动态 web 资源，比如 Servlet(java)、PHP 等。
    - 静态 web 资源开发技术：Html、CSS,js 等。
    - 常用动态 web 资源开发技术：Servlet、SpringBoot、SpringMVC、PHP、`ASP.NET` 等。

#### BS与CS开发

1. BS介绍
    - B:Browser,浏览器，由第三方开发
    - S:Server,服务器，自己开发
    - C:Client,客户端，自己开发
    - 图解
        ![javaweb_TomCat_BS01](./img/javaweb_TomCat_BS01.png)

2. BS 解读
    - **兼容性**, 因为浏览器的种类很多，发现你写的程序，在某个浏览器会出现问题，其它浏览器正常
    - **安全性**, 通常情况下，BS 安全性不如 CS 好控制
    - **易用性**, BS 好于 CS, 浏览器电脑有
    - **扩展性**, BS 相对统一，只需要写 Server

#### Web服务软件

1. 简介
    学习 JavaWeb 开发，需要先安装 JavaWeb 服务软件【我们把安装了JavaWeb 服务软件主机称为 Web 服务器/JavaWeb 服务器】，然后在 web 服务器中开发相应的 web 资源。[Javaweb 服务器，Mysql 服务器]
2. 学习 JavaWeb 开发，为什么必须要先装 WEB 服务软件?
    Tomcat 本质就是一个 Java 程序, 但是这个 Java 程序可以处理来自浏览器的 HTTP 请求, 和我们前面讲的 java 网络服务（多人聊天, Server）

2. 手搓简单的Web服务程序
    - 由浏览器发出HTTP请求，java程序在9999端口监听，并读取文件`hello.html`.
    - **经测试，只有火狐浏览器能完成这个操作。**
    ```java
    import java.io.*;
    import java.net.ServerSocket;
    import java.net.Socket;

    public class Server01 {
        public static void main(String[] args) throws IOException {
            ServerSocket serverSocket = new ServerSocket(9999);
            Socket clientSocket = null;
            FileInputStream fileInputStream = null;
            OutputStream outputStream = null;

            fileInputStream = new FileInputStream(new File("src/hello.html"));

            while (!serverSocket.isClosed()) {
                System.out.println("waiting for client on port 9999");
                clientSocket = serverSocket.accept();
                System.out.println("client connected!!!");
                outputStream = clientSocket.getOutputStream();

                outputStream.write("Hello World!".getBytes());
    //            outputStream.flush();

                BufferedReader bufferedReader =
                new BufferedReader(new FileReader("src/hello.html"));
                String buf = "";
    // 循环读取 hello.html
                while ((buf = bufferedReader.readLine())!=null) {
                    outputStream.write(buf.getBytes());
                }

                outputStream.close();
                clientSocket.close();
            }


            fileInputStream.close();
            outputStream.close();
            serverSocket.close();
            clientSocket.close();
        }
    }

    ```

3. 常用javaweb服务软件
    - `Tomcat`：由 Apache 组织提供的一种 Web 服务器，提供对 jsp 和 Servlet 的支持。它是一种轻量级的 javaWeb 容器（服务器），也是当前应用最广的 JavaWeb 服务器（免费）。
    - `Jboss`：是一个遵从 JavaEE 规范的、它支持所有的 JavaEE 规范（免费）。
    - `GlassFish`： 由 Oracle 公司开发的一款 JavaWeb 服务器，是一款商业服务器，达到产品级质量（应用很少）。
    - `Resin`：是 CAUCHO 公司的产品，是一个非常流行的服务器，对 servlet 和 JSP 提供了良好的支持， 性能也比较优良（收费）。
    - `WebLogic【很猛】`：是 Oracle 公司的产品，支持 JavaEE 规范， 而且不断的完善以适应新的开发要求，适合大型项目（收费，用的不多，适合大公司）。

### TomCat使用

#### 下载与安装
1. Tomcat 官方站点：http://tomcat.apache.org/
2. 获取 Tomcat 安装程序包
    - tar.gz文件是Linux操作系统下的安装版本
    - zip文件是Windows系统下的压缩版本
3. 使用 zip 包安装 Tomcat
    - 找到你需要用的 Tomcat 版本对应的 zip 压缩包，解压到需要安装的目录即可
4. which version？
     https://tomcat.apache.org/whichversion.html ,可以看到Tomcat仍然是支持
5. TOMCAT搭配JSP使用

#### 启动
1. 流程
    - 解压后，点击bin目录下的`startup.bat`（Windows系统）
    - 在浏览器输入http://localhost:8080（**注意，是`http`，不是`https`**）
    - **注意：关闭黑色窗口会导致TomCat终止运行。**
    - 可以在管理员身份下，输入`netstat -anb`，可以查看正在监听的端口。
        ```sh
        (base) PS C:\Users\85035> netstat -anb | findstr "8080"
        TCP    0.0.0.0:8080           0.0.0.0:0              LISTENING
        ```

2. catalina 启动 Tomcat
    - 进入到 Tomcat 的 bin 目录下
    - 执行命令： `catalina run`

5. 启动故障排除
    - 双击 startup.bat 文件，出现一个小黑窗口然后就没了，原因是因为没有配置好JAVA_HOME 环境变量
    - Tomcat 本质是一个 Java 程序，所以要 jdk, 会去根据 JAVA_HOME 使用指定 jdk
    - JAVA_HOME 必须全大写
    - JAVA_HOME 中间必须是下划线
    - JAVA_HOME 配置的路径只需要配置到 jdk 的安装目录即可。不需要带上 bin 目录
    - 端口 8080 被占用 [查看端口 netstat -anb, 使用的非常多]
    - 如果其它服务程序占用了 8080 端口，可以关闭该服务，或者修改 Tomcat 服务的默认端口 8080 [后面讲]
    - 配置 JAVA_HOME 环境变量

#### TomCat目录结构

1. 图解
    ![javaweb_TomCat_dir_structure](./img/javaweb_TomCat_dir_structure.png)

2. `conf`
    - `server.xml`：TomCat的基本设置。（启动端口、主机名等）
    - `web.xml`：用于指定TomCat运行时配置。（serverlet等）

#### 停止 Tomcat
1. 点击 tomcat 服务器窗口，直接点击的关闭按钮
2. 进入 Tomcat 的 bin 目录下的 shutdown.bat 双击，就可以停止 Tomcat 服务器(推荐，通过端口8009关闭)

#### 部署web应用

1. 工程目录结构
    - 这个目录结构可以由IDEA自动生成
    ![javaweb_TomCat_webApps01](./img/javaweb_TomCat_webApps01.png)

2. ROOT目录
    ```sh
    (base) PS E:\code\IntelliJ\apache-tomcat-8.0.50\webapps> tree.com /F .\ROOT\
    卷 新加卷 的文件夹 PATH 列表
    卷序列号为 78D3-7F28
    E:\CODE\INTELLIJ\APACHE-TOMCAT-8.0.50\WEBAPPS\ROOT
    │  asf-logo-wide.svg
    │  bg-button.png
    │  bg-middle.png
    │  bg-nav-item.png
    │  bg-nav.png
    │  bg-upper.png
    │  favicon.ico
    │  index.jsp
    │  RELEASE-NOTES.txt
    │  tomcat-power.gif
    │  tomcat.css
    │  tomcat.gif
    │  tomcat.png
    │  tomcat.svg
    │
    └─WEB-INF
        web.xml
    ```

3. 部署方式1：将 web 工程的目录拷贝到 Tomcat 的 webapps 目录下
    - news Web工程(目前都是静态资源 html, 图片)
    - 将该news目录/文件夹 拷贝到 Tomcat 的webapps目录下
    - 浏览器输入： `http://ip[域名]:port/news/子目录../文件名`

4. 部署方式2：通过配置文件来部署(只做介绍)
    - 在Tomcat 下的 conf 目录\Catalina\localhost\ 下,配置文件，比如hsp.xml(提醒：知道Tomcat通过配置，可以把一个web应用，映射到指定的目录，可以解决磁盘空间分配的问题.)
    - 通过增加的配置文件修改APP位置。
        ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <Context path="/hsp" docBase="D:\album" />
        ```
    - 访问web工程: `http://ip[域名]:port/hsp/index.html`就表示访问`D:\album` 目录下的`index.html`
    - IDEA 会自动完成操作。

5. 访问ROOT工程
    - 在浏览器地址栏中输入访问地址如下：`http://ip[域名]:port`，没有`Web工程/应用名`时，默认访问的是 ROOT 工程。
    - 在浏览器地址栏中输入的访问地址如下： `http://ip[域名]:port/工程名/`，没有资源名，默认访问 `index.jsp`或`index.html` 页面。

#### UML 时序图（浏览器访问 Web 服务过程详解）

1. 说明
    - 对浏览器访问web服务器资源（html、css、图片、js）做详解。
    - 通过一个时序图理解这个过程。
2. 图解
    - GET 开头的是请求头
    - HTTP开头的是响应头
    ![javaweb_TomCat_UML01](./img/javaweb_TomCat_UML01.png)

3. 简述
    对于一个有文字的图片的页面，请求主要分下面几次。
    - 第一次请求获得html文件原始代码。
    - 第二次请求获得标签引用内容。（如img标签，每次请求只会请求一个资源，多个图片对应多次请求）

### IDEA开发JavaWeb工程

#### 修改TomCat服务端口

1. 修改`conf/server.xml`
    - 在文件中找到如下内容
        ```xml
        <Connector port="8080" protocol="HTTP/1.1"
                connectionTimeout="20000"
                redirectPort="8443" />
        ```
    - 默认端口为`8080`，端口号范围1~65535
    - 建议将端口号修改为`>1024`的值，最好`>10000`
    - 修改后，重启Tomcat，配置生效。
    - `http://localhost`默认访问80端口。

#### 创建JavaWeb工程

1. 创建Java项目
2. 在新建好的项目上点击右键，点（`Add Framework Support`）
    - 在高版本中，需要通过`File - Project Structure - modules`点加号添加框架支持。
        ![javaweb_TomCat_IDEA_frameworkSupport](./img/javaweb_TomCat_IDEA_frameworkSupport.png)
    - 然后在`Artifacts`中添加扩展
        ![javaweb_TomCat_IDEA_artifacts01](./img/javaweb_TomCat_IDEA_artifacts01.png)
        ![javaweb_TomCat_IDEA_artifacts02](./img/javaweb_TomCat_IDEA_artifacts02.png)
3. 点击导入Web Application，点击ok即可

#### 配置TomCat并启动项目

1. 点击右上角的编辑配置
    ![javaweb_TomCat_IDEA_config01](./img/javaweb_TomCat_IDEA_config01.png)
2. 点击+号，找到Tomcat Server
    ![javaweb_TomCat_IDEA_config02](./img/javaweb_TomCat_IDEA_config02.png)
 
3. 选择一个本地下载好的Tomcat
    ![javaweb_TomCat_IDEA_config03](./img/javaweb_TomCat_IDEA_config03.png)
 
4. 在右边的部署栏Deployment中点击+号，添加构建
    - 两种发布方式：war包和源码
    - 这里可以选择只保留`/`，但不建议。
    - 建议改成当前Web工程名(项目名)，比如 `/news` ， `/crm` 等, 更好区分。

    ![javaweb_TomCat_IDEA_config041](./img/javaweb_TomCat_IDEA_config041.png)
    ![javaweb_TomCat_IDEA_config042](./img/javaweb_TomCat_IDEA_config042.png)
    ![javaweb_TomCat_IDEA_config043](./img/javaweb_TomCat_IDEA_config043.png)

5. 选择热加载，点击ok完成运行环境部署 
    - 可以在这里选择运行的浏览器。
    - 配置按图中所示配置
    - 若端口有更改，需要改掉这里的8080
    - **为了应对可能出现的控制台乱码情况，可以在`虚拟机选项`中填入`-Dfile.encoding=UTF-8`**

    ![javaweb_TomCat_IDEA_config05](./img/javaweb_TomCat_IDEA_config05.png)

6. 运行Tomcat，访问项目
    - tomcat启动成功后，会自动跳转到浏览器打开页面

    ![javaweb_TomCat_IDEA_config06](./img/javaweb_TomCat_IDEA_config06.png)

#### TomCat配置细节解读

1. 热加载选项说明：为什么选择`Update classes and resources（更新类和资源）`？
    - `on 'Update' action`：执行更新操作时，选择更新类和资源。即当jsp/html文件修改时，可以生效，但java文件修改后仍需要redeploy。
    - `on frame deactivation`：切换出IDE时（失去焦点时），选择更新类和资源。即IDEA软件不再是活动窗口时，执行的操作。

2. 端口修改
    - IDEA中TomCat的端口可以任意修改，修改的端口会即刻生效且只作用于当前项目，不会修改`server.xml`。
    - **手动启动和IDEA启动的TomCat不能使用同一端口！！！**

3. `out`目录
    当tomcat启动时，会生成out目录，该目录就是原项目资源的映射，我们浏览器访问的资源是out目录。

4. 当我们从外部拷贝资源到项目(图片, 文件, js , css 等), 如果出现 404 不能访问错误。
    解决方式： `rebulid project -> 重启 Tomcat`

5. IDEA工作目录解读
    ![javaweb_TomCat_IDEA_dir](./img/javaweb_TomCat_IDEA_dir.png)

## 动态Web开发核心-Servlet

### 基本内容

#### 参考材料
1. 官方文档
    https://tomcat.apache.org/tomcat-8.0-doc/servletapi/index.html

#### serverlet简介
1. Servlet和TomCat的关系
    - TomCat支持Servlet

3. 为什么会出现Servlet？
    - 现有的前端三件套（html、css、js）无法满足某些需求：开发网站，让用户留言、购物、支付。
    - 需要一些后端技术对刚才提到的一些需求提供技术支撑。
    - 引入动态网页技术（Servlet 可以与用户交互）

4. 细化的JavaWeb体系图
    - 将容器内容细化
    - Servlet会根据请求的资源类型选择静态资源或java。

    ![javaweb_Servlet_techStack](./img/javaweb_Servlet_techStack.png)

5. 什么是Servlet？
    - Servlet 在开发动态 WEB 工程中，得到广泛的应用，掌握好 Servlet 非常重要了, Servlet(基石)是 SpringMVC 的基础。

6. Servlet(java 服务器小程序)，它的特点:
    - 他是由服务器端调用和执行的(一句话：是Tomcat解析和执行)
    - 他是用java语言编写的, 本质就是Java类
    - 他是按照Servlet规范开发的(除了tomcat->Servlet weblogic->Servlet)
    - 功能强大，可以完成几乎所有的网站功能(老程员常使用Servlet开发网站) **随着技术进步技术栈要求高**

### Servlet 基本使用

#### Servlet 开发方式说明
1. servlet3.0 前使用 web.xml , servlet3.0 版本以后(包括 3.0)支持注解， 同时支持 web.xml
配置
3. ssm , springboot 后面全部使用注解方式进行学习
4. 对于Serverlet，主要理解其原理，原生的 Servlet 在项目中使用很少。

#### 快速入门：手动开发Serverlet

1. 需求
    - 开发一个 HelloServlet
    - 当浏览器 访问 http://localhost:8080/web 应用名/helloServlet 时，**后台**输出 "hi HelloServelt"
2. 具体步骤
    - 编写类`HelloServlet`去实现 Servlet 接口
    - 实现 service 方法，处理请求，并响应数据
    - 在 web.xml 中去配置 servlet 程序的访问地址
3. 开始实操
    - 创建 JavaWeb工程，并配置好Tomcat。
    - 添加servlet-api.jar(在tomcat/lib下) 到工程，放在`web/WEB-INF/lib`目录下。现在项目可以进行Servlet开发了。
    - 在src中的`HelloServlet.java`实现Servlet接口。
    ```java
    package com.lcq.servlet;

    import javax.servlet.*;
    import java.io.IOException;

    /**
    * 1. 开发Servlet需要实现Servlet接口
    * 2. 实现Servlet，共有5个方法
    */
    public class HelloServlet implements Servlet {

        /**
        * 1. 初始化Servlet
        * 2. 当创建HelloServlet实例时，会调用init方法
        * 3. 这个方法只会被调用一次
        * @param servletConfig
        * @throws ServletException
        */
        @Override
        public void init(ServletConfig servletConfig) throws ServletException {
            System.out.println("init被调用了~");
        }

        /**
        * 返回Servlet的配置ServletConfig
        * @return
        */
        @Override
        public ServletConfig getServletConfig() {
            return null;
        }

        /**
        * 1. service方法处理浏览器的请求（GET/POST）
        * 2. 当浏览器每次请求Servlet时，都会调用一次service方法。
        * 3. 当TomCat调用该方法时，会将HTTP请求的数据封装成实现ServletRequest接口的Request对象
        * 4. 通过servletRequest对象，可以获得用户提交的数据
        * 5. servletResponse用于返回数据，经由TomCat传回浏览器。
        * @param servletRequest
        * @param servletResponse
        * @throws ServletException
        * @throws IOException
        */
        @Override
        public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {

        }

        /**
        * 用于返回Servlet信息，使用较少
        * @return
        */
        @Override
        public String getServletInfo() {
            return "";
        }

        /**
        * 1. 该方法在Servlet销毁时调用
        * 2. 只会调用一次
        */
        @Override
        public void destroy() {

        }
    }
    ```
    - 在web.xml配置HelloServlet，即:给HelloServlet 提供对外访问地址
        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
                version="4.0">
            <!--web.xml主要用来配置该Web应用使用到的Servlet-->
            <!--配置HelloServlet-->
            <servlet>
                <!--servlet-name：Servlet名称，这个名称是唯一的-->
                <servlet-name>HelloServlet</servlet-name>
                <!--servlet-class:Servlet的类路径，TomCat在反射生成该Servlet需要使用-->
                <servlet-class>com.lcq.servlet.HelloServlet</servlet-class>
            </servlet>
            <servlet-mapping>
                <!--名字保持一致-->
                <servlet-name>HelloServlet</servlet-name>
                <!--url-pattern：该servlet访问的配置（路径）-->
                <!--访问方式：http://localhost:8080/servlet01/helloServlet-->
                <url-pattern>/helloServlet</url-pattern>
            </servlet-mapping>
        </web-app>
        ```
    - 通过浏览器访问HelloServlet ,看是否正确(记住要redeploy[快] 或者 restart[慢])

#### 浏览器调用Servlet流程分析（UML）

1. 一图胜千言
    ![javaweb_Servlet_analysis](./img/javaweb_Servlet_analysis.png)

#### Servlet生命周期

1. 主要方法
    - `init()`：初始化阶段
    - `service()`：处理浏览器请求阶段
    - `destroy()`：终止阶段

2. 图解
    ![javaweb_Servlet_lifePriod](./img/javaweb_Servlet_lifePriod.png)

3. 初始化阶段
    Servlet 容器(比如: Tomcat)加载 Servlet，加载完成后，Servlet 容器会创建一个 Servlet 实例，并调用 init()方法，init()方法只会调用一次, Servlet 容器在下面的情况装载 Servlet：
    - Servlet 容器(Tomcat)**启动时**自动装载某些 servlet，实现这个需要在 `web.xml` 文件中添加`<load-on-startup>1</load-on-startup>`，`1`表示装载的顺序。
        ```xml
        <servlet>
            <!--servlet-name：Servlet名称，这个名称是唯一的-->
            <servlet-name>HelloServlet</servlet-name>
            <!--servlet-class:Servlet的类路径，TomCat在反射生成该Servlet需要使用-->
            <servlet-class>com.lcq.servlet.HelloServlet</servlet-class>
            <load-on-startup>1</load-on-startup>
        </servlet>
        <servlet-mapping>
            <!--名字保持一致-->
            <servlet-name>HelloServlet</servlet-name>
            <!--url-pattern：该servlet访问的配置（路径）-->
            <!--访问方式：http://localhost:8080/servlet01/helloServlet-->
            <url-pattern>/helloServlet</url-pattern>
        </servlet-mapping>
        ```
    - 在 Servlet 容器启动后，浏览器**首次**向 Servlet 发送请求。
    - Servlet 重新装载时(比如 tomcat 进行 redeploy，**redeploy 会销毁所有的 Servlet 实例**)，浏览器再向 Servlet 发送请求的第 1 次

4. 处理浏览器请求阶段(service 方法)
    - 每收到一个 http 请求，服务器就会产生一个**新的线程**去处理。
    - 创建一个用于封装 HTTP 请求消息的 `ServletRequest` 对象和一个代表 HTTP 响应消息的`ServletResponse` 对象
    - 然后调用 Servlet 的 service()方法并将请求和响应对象作为参数传递进去

5. 终止阶段 destory 方法：**体现 Servlet 完整的生命周期**
    - 会调用 destroy()方法
        - 当web 应用被终止
        - 或者Servlet 容器终止运行
        - 或者Servlet 类重新装载时
    - 比如重启 tomcat ,或者 redeploy web 应用

#### GET和POST请求的分发处理

1. 简介
    - 开发 Servlet, 通常编写 `doGet`、`doPost` 方法。来对表单的 GET 和 POST 请求进行分发处理。

2. html页面
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>注册</title>
    </head>
    <body>
    <h1>注册用户</h1>
    <form action="http://localhost:8080/servlet01/HelloServlet" method="post">
        u: <input type="text" name="username"/><br><br>
        <input type="submit" value="注册用户"/>
    </form>
    </body>
    </html>
    ```

3. 增加的代码
    ```java
    /**
     * 1. service 方法处理浏览器的请求(包括 get/post)
     * 2. 当浏览器每次请求 Servlet 时，就会调用一次 service
     * 3. 当 tomcat 调用该方法时，会把 http 请求的数据封装成实现 ServletRequest 接口的 request 对象
     * 4. 通过 servletRequest 对象，可以得到用户提交的数据
     * 5. servletResponse 对象可以用于返回数据给 tomcat->浏览器
     * @param servletRequest
     * @param servletResponse
     * @throws ServletException
     * @throws IOException
     */
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException,IOException {
        count++;
        //如果 count 的值，在不停的累计，说明 HelloServlet 是单例的
        System.out.println("hi HelloServlet~ count= " + count);
        //Tomcat 每处理一次 http 请求，就生成一个新的线程
        System.out.println("当前线程 id= " + Thread.currentThread().getId());
        //思考->从 servletRequest 对象来获取请求方式->
        //1. ServletRequest 没有得到提交方式的方法
        //2. ServletRequest 看看 ServletRequest 子接口有没有相关方法
        //3. ctrl+alt+b => 可以看到接口的子接口和实现子类
        //4. 把 servletReqeust 转成 HttpServletRequest 引用
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        String method = httpServletRequest.getMethod();
        if("GET".equals(method)) {
            doGet(); //用 doGet() 处理 GET 请求
        } else if("POST".equals(method)) {
            doPost(); //用 doPost() 处理 POST 请求
        }
    }
    /**
     * 用于响应 get 请求的
     */
    public void doGet() {
        System.out.println("doGet() 被调用..");
    }
    /**
     * 用于响应 post 请求的
     */
    public void doPost() {
        System.out.println("doPost() 被调用..");
    }
    ```

#### 通过继承`HttpServlet`开发Servlet

1. 介绍
    - 在实际项目中，都是使用继承 HttpServlet 类开发 Servlet 程序，更加方便。
    - `HttpServlet`继承了一个实现了`Servlet`接口的抽象类。

2. 实操
    - 通过继承 HttpServlet 开发一个 HiServlet
    - 当浏览器 访问 `http://localhost:8080/web 应用名/hiServlet` 时，后台输出 `"hi HiServelt"`

3. 具体的开发步骤
    - 编写一个类去继承 HttpServlet 类。
    - 根据业务需要重写 doGet 或 doPost 方法。
    - 到 web.xml 中的配置 Servlet 程序。

#### IDEA开发Servlet

1. 介绍
    - 编手动开发 Servlet 需要程序员自己配置 Servlet ,比较麻烦，在工作中，直接使用 IDEA 开发 Servlet 会更加方便。
    - 但是从2024版IDEA开始，模板不再被提供，转而提供配置方法，具体内容参照下面的网址。
    - https://www.jetbrains.com/help/idea/creating-and-configuring-web-application-elements.html
    - **但由于这是以注解形式写的模板，故需要先看后面的内容，然后回来补充本小节的知识。**

2. 旧版本默认生成的代码
    - `web.xml`已自动配置
    - 但`<servlet-mapping>`仍需自己补充
    ```java
    import javax.servlet.ServletException;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import java.io.IOException;

    public class OkServlet extends HttpServlet {
        protected void doPost(HttpServletRequest request, HttpServletResponse response) 
            throws ServletException, IOException {
        //可以写自己的业务处理代码
            System.out.println("OkServlet doPost()");
        }
        protected void doGet(HttpServletRequest request, HttpServletResponse response) 
            throws ServletException, IOException {
        //可以写自己的业务处理代码
            System.out.println("OkServlet doGet()");
        }
    }
    ```

#### Servlet 注意事项和细节
1. Servlet 是一个供其他 Java 程序（Servlet 引擎）调用的 Java 类，不能独立运行。
2. 针对浏览器的多次 Servlet 请求，通常情况下，服务器只会创建一个 Servlet 实例对象，也就是说 Servlet 实例对象一旦创建，它就会驻留在内存中，为后续的其它请求服务，直至web 容器退出/或者 redeploy 该 web 应用，servlet 实例对象才会销毁。
3. 在 Servlet 的整个生命周期内，`init` 方法只被调用一次。而对每次请求都导致 Servlet 引擎调用一次 servlet 的 `service` 方法。
4. 对于每次访问请求，Servlet 引擎都会创建一个新的 HttpServletRequest 请求对象和一个新的 HttpServletResponse 响应对象，然后将这两个对象作为参数传递给它调用的 Servlet的 service()方法，service 方法再根据请求方式分别调用 `doXXX` 方法
5. 如果在`<servlet>`元素中配置了一个`<load-on-startup>`元素，那么 WEB 应用程序在启动时，就会装载并创建 Servlet 的实例对象、以及调用 Servlet 实例对象的 init()方法。(如定时发送邮件的服务/自动启动->完成任务)

### Servlet注解方式

#### 快速入门
1. 介绍
    - 使用注解方式开发 Servlet 时，​​通常可以不再需要手动配置 web.xml文件​​。这得益于 Servlet 3.0 及以后版本引入的注解功能。

1. 基本步骤
    - 编写类`OkServlet`并继承`HttpServlet`。
    - 注解方式配置`OkServlet`，一个`Servlet`支持配置多个`urlPattern`

3. 注解源码
    | 注解参数 (Annotation Attribute) | 对应的 web.xml 标签 (web.xml Tag) | 作用描述 (Description) |
    | :--- | :--- | :--- |
    | `name` | `<servlet-name>` | 指定 Servlet 的名称。如果未显式指定，默认值为该 Servlet 类的全限定名。 |
    | `value` | `<url-pattern>` | **`value` 和 `urlPatterns` 属性是等价的**，两者不能同时使用。用于指定 Servlet 的 URL 映射模式。如果只设置此属性，可以省略属性名，如 `@WebServlet("/example")`。 |
    | `urlPatterns` | `<url-pattern>` | 指定一组 Servlet 的 URL 匹配模式。支持精确匹配（如 `/user`）、路径匹配（如 `/admin/*`）、扩展名匹配（如 `*.do`）等。 |
    | `loadOnStartup` | `<load-on-startup>` | 控制 Servlet 的加载时机和顺序。**负值（默认-1）**：首次被请求时加载。**0或正值**：Web应用启动时加载，数值越小优先级越高。 |
    | `initParams` | `<init-param>` | 用于配置 Servlet 的初始化参数（需配合 `@WebInitParam` 注解使用）。在 Servlet 中可通过 `getServletConfig().getInitParameter("参数名")` 获取。 |
    | `asyncSupported` | `<async-supported>` | 声明此 Servlet 是否支持异步处理模式。默认为 `false`。若需处理耗时操作（如长轮询、文件上传），应设置为 `true`。 |
    | `description` | `<description>` | 提供关于此 Servlet 的文本描述信息，纯文档用途，无实际运行逻辑影响。 |
    | `displayName` | `<display-name>` | 指定此 Servlet 的显示名称，通常用于 IDE 或服务器管理控制台等工具中显示，便于识别。 |
    | `smallIcon` | `<small-icon>` | 指定一个小图标的路径，用于工具可视化显示 Servlet（现代开发中较少使用）。 |
    | `largeIcon` | `<large-icon>` | 指定一个大图标的路径，用于工具可视化显示 Servlet（现代开发中较少使用）。 |

    ```java
    //
    // Source code recreated from a .class file by IntelliJ IDEA
    // (powered by FernFlower decompiler)
    //

    package javax.servlet.annotation;

    import java.lang.annotation.Documented;
    import java.lang.annotation.ElementType;
    import java.lang.annotation.Retention;
    import java.lang.annotation.RetentionPolicy;
    import java.lang.annotation.Target;

    @Target({ElementType.TYPE})
    @Retention(RetentionPolicy.RUNTIME)
    @Documented
    public @interface WebServlet {
        String name() default "";

        String[] value() default {};

        String[] urlPatterns() default {};

        int loadOnStartup() default -1;

        WebInitParam[] initParams() default {};

        boolean asyncSupported() default false;

        String smallIcon() default "";

        String largeIcon() default "";

        String description() default "";

        String displayName() default "";
    }
    ```

4. 示例
    - `urlPattern`对应`web.xml`的`<url-pattern></url-pattern>`。
    - `urlPattern`的参数`{"/ok1","/ok2"}`可以配置多个url访问同一个服务，用下列url均可访问对应服务：
        - `http://localhost:8080/servlet01/ok1`
        - `http://localhost:8080/servlet01/ok2`
    - 这段注解代替`web.xml`配置生效。
    ```java
    package com.lcq.servlet.annotation;

    import javax.servlet.ServletException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import java.io.IOException;

    @WebServlet(urlPatterns = {"/ok1", "/ok2"})
    public class OkServlet extends HttpServlet {
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            System.out.println("annotation in doGet");
        }

        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            System.out.println("annotation in doPost");
        }
    }
    ```

5. 通过注解读取配置的代码实现演示
    ```java
    public class TestAnnotationServlet {
        public static void main(String[] args) throws ClassNotFoundException {
            String classAllPath = "com.lcq.servlet.annotation.OkServlet";
            Class<?> aClass = Class.forName(classAllPath);
            WebServlet annotation = aClass.getAnnotation(WebServlet.class);
            System.out.println(annotation);
            String[] strings = annotation.urlPatterns();
            // @javax.servlet.annotation.WebServlet(loadOnStartup=-1, initParams=[], urlPatterns=[/ok1, /ok2], displayName=, name=, largeIcon=, asyncSupported=false, description=, smallIcon=, value=[])
            //  /ok1
            //  /ok2
            for (String string : strings) {
                System.out.println(string);
            }
        }
    }
    ```

#### Servlet注解URL4种匹配方式

1. 精确匹配
    - 配置路径：`@WebServlet("/ok/ok1")`
    - 访问路径：`http://localhost:8080/servlet01/ok/ok1`

2. 目录匹配
    - 配置路径 : `@WebServlet("/ok/*")`
    - 访问文件: `http://localhost:8080/servlet01/ok/aaa`、`localhost:8080/servlet01/ok/bbb`、`localhost:8080/servlet01/ok/bbb/ccc`...
    - 支持多级目录。

3. 扩展名匹配
    - 配置路径 : `@WebServlet("*.action")`
    - 访问文件: `http://localhost:8080/servlet01/zs.action` `http://localhost:8080/servlet01/ls.action`。
    - `@WebServlet("/*.action")`**写法不规范**，不能带 `/` ，否则 tomcat 报错
    
4. 任意匹配
    - 配置路径：@WebServlet("/") @WebServlet("/*")
    - 访问文件：`http://localhost:8080/servlet01/aaa`、 `http://localhost:8080/servlet01/bbb`、 `http://localhost:8080/servlet01/ccc`...
    - `/` 和 `/*`的配置，会匹配所有的请求，这个比较麻烦，要避免。
    - 值得注意的是，`/` 和 `/*`是两个不完全相同的配置。
        - 若配置`/`（这个配置主要用于返回静态资源），则`http://localhost:8080/servlet01`依靠列表欢迎机制仍有可能访问到`servlet01/index.html`，但直接输入`http://localhost:8080/servlet01/index.html`会被拦截。（**内容来自deepseek，在我实际操作中，index.html仍被拦截**）
        - 若配置`/*`会拦截所有静态页面等请求。

5. 注意事项和使用细节
    - 当 Servlet 配置了 `"/"`, 会覆盖 tomcat 的 DefaultServlet, 当其他的 utl-pattern 都匹配不上时，都会走这个Servlet,这样可以拦截到其它静态资源,比如`http://localhost:8080/servlet01\hi.html`
    - 查看：`tomcat/conf/web.xml` 配置的 `DefaultServlet`
        ```xml
        <!-- The default servlet for all web applications, that serves static resources.  -->
        这个默认的 servlet 是处理静态资源的，一旦拦截，静态资源不能处理
        ```
        ```xml
        <servlet>
        <servlet-name>default</servlet-name>
        <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
        <init-param>
            <param-name>debug</param-name>
            <param-value>0</param-value>
        </init-param>
        <init-param>
            <param-name>listings</param-name>
            <param-value>false</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
        </servlet>
                <!-- The mapping for the default servlet -->
        <servlet-mapping>
        <servlet-name>default</servlet-name>
        <url-pattern>/</url-pattern>
        </servlet-mapping>
        ```
    - 当 Servelt 配置了 `"/*"`, 表示可以匹配任意访问路径。
    - 提示: 建议不要使用 `/` 和 `/*`，建议尽量使用精确匹配。
    - 优先级遵守: `精确路径 > 目录路径 > 扩展名路径 > /* > /`，由特殊到一般。

### `ServletConfig`

#### 基本介绍

1. 基本介绍
    - `ServletConfig` 类是为 `Servlet` 程序的配置信息的类。
    - `Servlet` 程序和 `ServletConfig` 对象都是由 Tomcat 负责创建。
    - `Servlet` 程序默认是第 1 次访问的时候创建，`ServletConfig` 在 `Servlet` 程序创建时，就创建一个对应的 `ServletConfig` 对象。

2. ServletConfig 类能干什么
    - 获取 Servlet 程序的 `servlet-name` 的值
    - 获取初始化参数 `init-param`
    - 获取 `ServletContext` 对象

#### 应用实例

1. 需求: 编写 `DBServlet.java` 完成如下功能
    - 在 `web.xml` 配置连接 `mysql` 的用户名和密码
    - 在 `DBServlet` 执行 `doGet()/doPost()` 时，可以获取到 `web.xml` 配置的用户名和密码。

2. `web.xml`
    ```xml
    <servlet>
        <servlet-name>DBServlet</servlet-name>
        <servlet-class>com.lcq.servlet.DBServlet</servlet-class>
    <init-param>
        <param-name>username</param-name>
        <param-value>root</param-value>
    </init-param>
        <init-param>
            <param-name>password</param-name>
            <param-value>lcq</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>DBServlet</servlet-name>
        <url-pattern>/db</url-pattern>
    </servlet-mapping>
    ```

3. `DBServlet.java`
    ```java
    package com.lcq.servlet;

    import javax.servlet.ServletConfig;
    import javax.servlet.ServletException;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import java.io.IOException;

    public class DBServlet extends HttpServlet {
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            ServletConfig servletConfig = this.getServletConfig();
            String username = servletConfig.getInitParameter("username");
            String password = servletConfig.getInitParameter("password");
            System.out.println("username:" + username + "\npassword:" + password);
        }

        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            this.doGet(req, resp);
        }
    }

    ```

### `ServletContext`

#### 基本介绍

1. 为什么需要 `ServletContext`
    - 先看一个需求： 如果我们希望统计某个 web 应用的所有 Servlet 被访问的次数，怎么办?
    - 方案 1-DB
    - 方案 2-`ServletContext`

2. 方案2的特点
    - 这个对象在tomcat中全局唯一。
    - 可以被多个Servlet共享。
    - 可以实现Servlet间的通信。
    - 数据存储形式：`Key-Value`
    - 数据存在于内存中

2. `ServletContext` 基本介绍
    - `ServletContext` 是一个接口，它表示 Servlet 上下文对象
    - 一个 web 工程，只有一个 `ServletContext` 对象实例
    - `ServletContext` 对象 是在 web 工程启动的时候创建，在 web 工程停止的时销毁
    - `ServletContext` 对象可以通过 `ServletConfig.getServletContext` 方法获得对 `ServletContext`对象的引用，也可以通过 `this.getServletContext()`来获得其对象的引用。
    - 由于一个 WEB 应用中的所有 Servlet 共享同一个 `ServletContext` 对象，因此 Servlet 对象之间可以通过 `ServletContext` 对象来实现多个 Servlet 间通讯。`ServletContext` 对象通常也被称之为域对象。

3. `ServletContext` 可以做什么
    - 获取 `web.xml` 中配置的上下文参数 `context-param` [信息和整个 web 应用相关，而不是属于某个 Servlet，区别于`<init-param>`]
    - 获取当前的工程路径，格式: /工程路径 =》 比如 /servlet
    - 获 取 工 程 部 署 后 在 服 务 器 硬 盘 上 的 绝 对 路 径 ( 比 如 :`D:\hspedu_javaweb\servlet\out\artifacts\servlet_war_exploded`)
    - 像 Map 一样存取数据, 多个 Servlet 共享数据

#### 应用实例 1-获取工程相关信息
1. 需求
    - 获取 web.xml 中配置的上下文参数 context-param
    - 获取当前的工程路径，格式: /工程路径
    - 获取工程部署后在服务器硬盘上的绝对路径

2. xml参数
    ```xml
    <context-param>
        <param-name>name</param-name>
        <param-value>aha</param-value>
    </context-param>
    ```

3. 代码实现
    ```java
    package com.lcq.servlet;

    import javax.servlet.ServletContext;
    import javax.servlet.ServletException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import java.io.IOException;

    @WebServlet(urlPatterns = {"/sc"})
    public class ServletContext01 extends HttpServlet {
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            ServletContext servletContext = this.getServletContext();
            String name = servletContext.getInitParameter("name");
            System.out.println("name:" + name);
            String contextPath = servletContext.getContextPath();
            System.out.println("contextPath:" + contextPath);
            String contextPath1 = req.getContextPath();
            System.out.println("contextPath1:" + contextPath1);
            String realPath = servletContext.getRealPath("/");
            System.out.println("realPath:" + realPath);
            //name:aha
            //contextPath:/http
            //contextPath1:/http
            //realPath:E:\code\IntelliJ\http\out\artifacts\http_war_exploded\
        }

        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            this.doGet(req, resp);
        }
    }
    ```


#### 应用实例 2-简单的网站访问次数计数器
1. 需求分析/图解
    - 需求: 完成一个简单的网站访问次数计数器：
    - 使用 Chrome 访问 Servlet01, 每访问一次，就增加 1 访问次数，在后台输出，并将结果返回给浏览器显示
    - 使用火狐访问 Servlet02，每访问一次，就增加 1 访问次数，在后台输出，并将结果返回给浏览器显示

2. 注意事项
    - **访问次数不能使用初始的参数，而是在java中配置属性。**

2. 示例
    ```java
    package com.lcq.servlet;
    
    import javax.servlet.ServletContext;
    import javax.servlet.ServletException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import java.io.IOException;
    import java.io.PrintWriter;
    
    @WebServlet(urlPatterns = {"/sc02", "/sc03"})
    public class ServletContext02 extends HttpServlet {
    
        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            this.doGet(req, resp);
        }
    
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            ServletContext servletContext = this.getServletContext();
            Object visit_count = servletContext.getAttribute("visit_count");
    
            if (visit_count == null) {
                visit_count = 1;
                servletContext.setAttribute("visit_count", visit_count);
            } else {
                visit_count = Integer.parseInt(visit_count.toString()) + 1;
                servletContext.setAttribute("visit_count", visit_count);
            }
    
            resp.setContentType("text/html;charset=utf-8");
            PrintWriter writer = resp.getWriter();
            writer.println("<h1>访问次数：" + visit_count + "<h1>");
            System.out.println("访问次数：" + visit_count);
            writer.flush();
            writer.close();
        }
    }
    ```

3. 改进版   
    - 将获取访问次数的过程包装成方法
    ```java
    public class WebUtils {
        public static Object visitCount(ServletContext servletContext){
            Object visit_count = servletContext.getAttribute("visit_count");

            if (visit_count == null) {
                visit_count = 1;
                servletContext.setAttribute("visit_count", visit_count);
            } else {
                visit_count = Integer.parseInt(visit_count.toString()) + 1;
                servletContext.setAttribute("visit_count", visit_count);
            }
            return visit_count;
        }
    }
    ```

### `HttpServletRequest`

#### 基本介绍

1. 介绍
    - `HttpServletRequest` 对象代表客户端的请求
    - 当客户端/浏览器通过 HTTP 协议访问服务器时，HTTP 请求头中的所有信息都封装在这个对象中
    - 通过这个对象的方法，可以获得客户端这些信息。

2. 继承关系
    ```java
    public interface HttpServletRequest extends ServletRequest
    ```

3. HttpServletRequest 常用方法
    - `getRequestURI()` 获取请求的资源路径 `/servlet/loginServlet`
    - `getRequestURL()` 获 取 请 求 的 统 一 资 源 定 位 符 （ 绝 对 路 径 ）`http://localhost:8080/servlet/loginServlet`
    - `getRemoteHost()` 获取客户端的主机（完整主机名）, `getRemoteAddr()`获取客户端IP
    - `getHeader()` 获取请求头
    - `getParameter()` 获取请求的参数，**表单`name`属性对应的标签**。
    - `getParameterValues()` 获取请求的参数（多个值的时候使用） , 比如 checkbox, 返回的数组。
    - `getMethod()` 获取请求的方式 GET 或 POST
    - `setAttribute(key, value)`; 设置域数据
    - `getAttribute(key)`; 获取域数据，**这两个方法区别于`getParameter()`，这是将属性绑定到请求对象上，用于在不同服务间流转这些参数。**
    - `getRequestDispatcher()` 获取请求转发对象, 请求转发的核心对象

#### HttpServletRequest 应用实例
1. 需求: 
    - 说明： 在一个表单提交数据给 Servlet
    - 然后在 Servlet 通过 HttpServletRequest对象获取相关数据


2. 综合案例
    ```java
    @WebServlet(urlPatterns = {"/registerOk2"})
    public class HttpServletreq02 extends HttpServlet {
        protected void doPost(HttpServletRequest request, HttpServletResponse response)
                throws ServletException, IOException {
            //这里我们使用 request 对象，获取表单提交的各种数据
            System.out.println("HttpServletRequestMethods doPost() 被调用..");
            /*
            获取和 http 请求头相关信息
            */
            System.out.println("请求的资源路径 URI= " + request.getRequestURI());
            //http://主机/uri
            System.out.println(" 请 求 的 统 一 资 源 定 位 符 （ 绝 对 路 径 ） URL= " +
                    request.getRequestURL());
            System.out.println("请求的客户端 ip 地址= " + request.getRemoteAddr());//本地就是 127.0 .01

            //思考题：如发现某个 ip 在 10s 中，访问的次数超过 100 次，就封 ip
            //实现思路：1 用一个集合 concurrentHashmap[ip:访问次数] 2[线程/定时扫描]3 做成处理
            // 获取 http 请求头的信息，可以指定其他，比如 User-Agent , Host 等待
            System.out.println("http 请求头 HOST= " + request.getHeader("Host"));
            // 说 明 ， 如 果 我 们 希 望 得 到 请 求 的 头 的 相 关 信 息 ， 可 以 使 用
            //request.getHeader("请求头字段")
            System.out.println("该请求的发起地址是= " + request.getHeader("Referer"));
            // 请获取访问网站的浏览器是什么？
            String userAgent = request.getHeader("User-Agent");
            System.out.println("User-Agent= " + userAgent);
            // 取出 FireFox, 取出最后
            String[] s = userAgent.split(" ");
            System.out.println("浏览器=" + s[s.length - 1].split("/")[0]);
            //课堂练习: 要求同学们取出 Windows NT 10.0 和 Win64
            // 主要是 Get / Post
            System.out.println("http 请求方式~= " + request.getMethod());
            /*
            获取和请求参数相关信息, 注意要求在返回数据前，获取参数
            */
            //1. 获取表单的数据[单个数据]
            //username=tom&pwd=&hobby=hsp&hobby=spls
            String username = request.getParameter("username");
            String pwd = request.getParameter("pwd");
            //2. 获取表单一组数据
            String[] hobbies = request.getParameterValues("hobby");
            System.out.println("username= " + username);
            System.out.println("pwd= " + pwd);
            //增强 for 循环的快捷键 iter->回车即可 , 能使用快捷键，就使用快捷键
            for (String hobby : hobbies) {
                System.out.println("hobby=" + hobby);
            }
            //推而广之, 如果是 单选 , 下拉框 等等. => 作业布置
        }

        protected void doGet(HttpServletRequest request, HttpServletResponse
                response) throws ServletException, IOException {
            doPost(request, response);
        }
    }
    ```

#### 使用细节

1. 解决中文乱码问题
    - 由于请求使用的url编码，而控制台使用utf8编码，直接读取传进来的中文数据会出现乱码。
    ```java
    req.setCharacterEncoding("utf-8");
    ```

2. 数据回送
    ```java
    resp.setContentType("text/html;charset=utf-8");
    resp.getWriter();
    //...
    ```

3. 再次理解`content-type`
    - 如果回送数据格式设置为`text/html`，html代码就会被解析，比如：`<h1>你好</h1>`
    - 如果回送数据格式为`text/plain`，则不会解析，转而直接输出原始文本。
    - 同理，`appication/x-tar`表明响应是一个文件，浏览器会弹出下载窗口。

### 请求转发

#### 基本内容

1. 为什么需要请求转发
    - 目前我们学习的都是一次请求，对应一个 Servlet。
    - 但是在实际开发中，往往业务比较复杂，需要在一次请求中，使用到多个 Servlet 完成一个任务(Servlet 链, 流水作业)。
    - 比如一个管理页面，经过权限验证，分发给不同权限对应的下级Servlet。

2. 请求转发说明
    - 实现请求转发：请求转发指一个 web 资源收到客户端请求后，通知服务器去调用另外一个 web 资源进行处理
    - `HttpServletRequest` 对象(也叫 Request 对象)提供了一个`getRequestDispatcher`（调度器） 方法，该方法返回一个 `RequestDispatcher` 对象，调用这个对象的 `forward` 方法可以实现请求转发。
    - request 对象同时也是一个域对象，开发人员通过 request 对象在实现转发时，把数据通过 request 对象带给其它 web 资源处理
        - `setAttribute`方法
        - `getAttribute`方法
        - `removeAttribute`方法
        - `getAttributeNames`方法

3. 请求转发原理示意图
    ![javaweb_Servlet_DispatcherUML](./img/javaweb_Servlet_Dispatcher01.png)

#### 请求转发应用实例
1. 需求说明：
    - 如果是 tom，提示为管理员，其它是普通用户
    - 使用`login.html`发送请求。

2. `CheckServlet.java`
    ```java
    package com.lcq.servlet.check;

    import javax.servlet.RequestDispatcher;
    import javax.servlet.ServletException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import java.io.IOException;

    @WebServlet(urlPatterns = {"/check"})
    public class CheckServlet extends HttpServlet {
        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            this.doGet(req, resp);
        }

        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            String username = req.getParameter("username");

            if ("tom".equals(username)) {
                req.setAttribute("role", "管理员");
            } else {
                req.setAttribute("role", "普通用户");
            }

            RequestDispatcher requestDispatcher = req.getRequestDispatcher("/manage");
            requestDispatcher.forward(req, resp);
        }
    }
    ```

3. `ManageServlet.java`
    ```java
    package com.lcq.servlet.check;
    
    import javax.servlet.ServletException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import java.io.IOException;
    import java.io.PrintWriter;
    
    @WebServlet(urlPatterns = {"/manage"})
    public class ManagerServlet extends HttpServlet {
        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            this.doGet(req, resp);
        }
    
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            String username = req.getParameter("username");
            Object role = req.getAttribute("role");
    
            resp.setContentType("text/html;charset=utf-8");
            PrintWriter writer = resp.getWriter();
            writer.println("<h1>"+username+":"+role+"</h1>");
        }
    }
    ```

#### 注意事项和细节

1. 在请求转发的过程中，浏览器的地址不会发生改变。
    - 区别于重定向。
    - 整个过程中浏览器只发出了一次HTTP请求，收到了一次HTTP响应。

2. 关于HTTP请求
    - 在同一次HTTP请求中，进行多次转发，仍然是一次HTTP请求。
    - 在同一次HTTP请求中，进行多次转发，多个Servlet可以共享request域/对象数据，即request域/对象数据（参数-属性）的作用域是在一次HTTP请求中有效

3. 可以转发到`WEB-INF`目录下
4. **不能访问当前WEB工程外的资源。**
5. 因为浏览器地址栏停止在第一个Servlet，如果刷新页面，会再次发出请求（并带数据），所以在支付页面情况下会造成重复支付，不要使用请求转发，而使用重定向。

### `HttpServletResponse`

#### 基本内容

1. 介绍
    - 每次 HTTP 请求，Tomcat 会创建一个 HttpServletResponse 对象传递给 Servlet 程序去使用。
    - HttpServletRequest 表示请求过来的信息，HttpServletResponse 表示所有响应的信息，如果需要设置返回给客户端的信息，通过 HttpServletResponse 对象来进行设置即可。

2. 常用方法
    - `setStatus(int)`：设置状态码。
    - `setHeader(String, String)`：设置响应头。
    

3. 向客户端返回数据方法
    - 字节流 `getOutputStream()`; 常用于下载（处理二进制数据）
    - 字符流 `getWriter()`; 常用于回传字符串
    - 细节：**两个流同时只能使用一个。 使用了字节流，就不能再使用字符流，反之亦然，否则就会报错。**

#### 注意事项和细节

1. 处理中文乱码问题（方法1）
    ```java
    //设置服务器字符集为UTF-8
    resp.setCharacterEncoding("UTF-8");
    //通过响应头，设置浏览器也使用UTF-8字符集
    resp.setHeader("Content-Type", "text/html; charset=UTF-8");
    ```

2. 处理中文乱码问题（方法2）
    ```java
    /*
    1.setContentType会设置服务器和客户端都用utf-8字符集，还设置了响应头
    2.setContentType要在获取流对象(getWriter)之前调用才有效*/
    response.setContentType("text/html;charset=utf-8"); 
    PrintWriter writer = response.getWriter();
    ```

#### 请求重定向

1. 流程示意图
    ![javaweb_Servlet_Redirect](./img/javaweb_Servlet_Redirect.png) 

2. 请求重定向注意事项和细节
    - 最佳应用场景：网站迁移，比如原域名是 `www.lcq.com` 迁移到 `www.lcq.cn` ，但是百度抓取的还是原来网址. 
    - 浏览器地址会发生变化，本质是两次 http 请求. 
    - **不能共享 Request 域中的数据**，本质是两次 http 请求，会生成两个 HttpServletRequest对象
    - **不能重定向到 `/WEB-INF` 下的资源**
    - 可以重定向到 Web 工程以外的资源， 比如 到`www.baidu.com`
    - 重定向有两种方式, 推荐使用第 1 种（`sendRedirect()`）。

3. 另一种重定向方法
    ```java
    resp.setStatus("302");
    resp.setHeader("Location", "http://www.baidu.com");
    ```

4. 动态获取web应用地址
    - `this.getServletContext().getContextPath()`方法可以获得web应用的URI
    - 这是在 Servlet 中获取 Web 应用上下文路径（Context Path）的有效方式之一
    ```java
    resp.sendRedirect(this.getServletContext().getContextPath() + "/serv01");
    ```

#### 实例演示：文件下载

1. 需求
    - 创建`DownloadServlet`和`FileServlet`。
    - 由前者重定向到后者，并完成资源下载。（仅弹出下载框）  


## Web开发通信协议——HTTP

### 基本内容

1. HTTP常见请求和响应头

2. HTTP响应状态码

#### 通过示例内容简单分析

1. 示例（GET请求头）
    ```http
    GET /servlet01/ok1 HTTP/1.1
    Host: localhost:8080
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:143.0) Gecko/20100101 Firefox/143.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
    Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
    Accept-Encoding: gzip, deflate, br, zstd
    Connection: keep-alive
    Cookie: Idea-e0c37dd3=7f4f7749-282f-455b-8e3d-292e8ce0f3d2
    Upgrade-Insecure-Requests: 1
    Sec-Fetch-Dest: document
    Sec-Fetch-Mode: navigate
    Sec-Fetch-Site: none
    Sec-Fetch-User: ?1
    Priority: u=0, i
    ```
    -   `GET /servlet01/ok1 HTTP/1.1`：请求行（下面的全是请求头）
        -   `GET`：请求方式
        -   `/servlet01/ok1`：URI，统一资源标识符，标注资源在web服务器上的位置。**如果有表单数据提交，也会在这里显示，如`/servlet01/ok1?username=lcq`**。
        -   `HTTP/1.1`：协议名及版本号。
    -   `Host: localhost:8080`
        -   `Host`：指定请求的目标服务器的域名和端口号。这是 HTTP/1.1 **必须**的头部字段。
    -   `User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:143.0) Gecko/20100101 Firefox/143.0`
        -   `User-Agent`：包含发出请求的用户信息
        - 包含客户端（浏览器）的应用程序类型、操作系统、软件开发商和版本号等信息。
        - Mozilla：浏览器
        - **TomCat会根据具体的浏览器型号返回不同的响应格式。**
    -   `Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8`
        -   `Accept`：告知服务器客户端能够处理的内容类型（MIME类型）及其相对优先级（通过`q`质量权重值表示）。
        - **如果响应没有指明返回数据的格式，浏览器会根据请求头中提及的格式从左至右依次尝试解析。**
    -   `Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2`
        -   `Accept-Language`：告知服务器客户端偏好的自然语言（如中文、英文）及其相对优先级。
    -   `Accept-Encoding: gzip, deflate, br, zstd`
        -   `Accept-Encoding`：告知服务器客户端支持的内容压缩算法，服务器可选择其中一种对响应体进行压缩以节省带宽。
    -   `Connection: keep-alive`
        -   `Connection`：控制本次网络连接在请求完成后是否关闭。`keep-alive` 表示希望保持连接打开以供后续请求使用，这是 HTTP/1.1 的默认行为。
    - `Referer`：表示这个请求由哪个页面发起（存在于表单提交中），如`Referer: http://localhost:8080/http/login.html`，可以防盗链（比如某些网站只接受自家网站发起的请求）。
    -   `Cookie: Idea-e0c37dd3=7f4f7749-282f-455b-8e3d-292e8ce0f3d2`
        -   `Cookie`：将由客户端存储并在每次请求时自动发送回该服务器域下的所有 Cookie 信息。此例中的 Cookie 通常由 JetBrains IntelliJ IDEA 生成，用于会话管理等。
    -   `Upgrade-Insecure-Requests: 1`
        -   `Upgrade-Insecure-Requests`：向服务器表明客户端（浏览器）倾向于使用加密和认证的响应（即 HTTPS），并愿意处理相关的重定向。
    -   `Sec-Fetch-Dest: document`
        -   `Sec-Fetch-Dest`：**Fetch Metadata** 请求头之一，告知服务器请求的目的地或最终使用资源的环境是什么类型。`document` 表示目标是导航到一个 HTML 文档。
    -   `Sec-Fetch-Mode: navigate`
        -   `Sec-Fetch-Mode`：**Fetch Metadata** 请求头之一，描述请求的模式。`navigate` 表示这是一个页面导航请求（即用户通过地址栏、链接跳转等发起的请求）。
    -   `Sec-Fetch-Site: none`
        -   `Sec-Fetch-Site`：**Fetch Metadata** 请求头之一，描述请求发起者和目标资源之间的“关系”或“站点”来源。`none` 表示此导航由用户直接发起的（例如在地址栏输入URL），而非来自其他网站的链接。
    -   `Sec-Fetch-User: ?1`
        -   `Sec-Fetch-User`：**Fetch Metadata** 请求头之一，表明导航请求是否由用户交互（如点击链接）触发。`?1` 表示 **true**（是）。
    -   `Priority: u=0, i`
        -   `Priority`：向服务器提示该请求的相对优先级，帮助服务器进行资源调度。此处的具体值由浏览器内部算法决定。

2. 示例（响应头）
    ```http
    HTTP/1.1 200 OK
    Server: Apache-Coyote/1.1
    Accept-Ranges: bytes
    ETag: W/"204-1759760370604"
    Last-Modified: Mon, 06 Oct 2025 14:19:30 GMT
    Content-Type: text/html
    Content-Length: 204
    Date: Tue, 07 Oct 2025 12:47:31 GMT
    ```
    - `HTTP/1.1 200 OK`
        - `HTTP/1.1`：协议名。
        - `200 OK`：状态码200表示请求已成功处理，页面不存在则返回`404 NOT FOUND`。
    - `Server: Apache-Coyote/1.1`：web服务器软件名称。处理请求的服务器软件是Apache Coyote（通常为Tomcat服务器的连接组件）。
    - `Accept-Ranges: bytes`：表明服务器是否支持指定范围请求及哪种类型的分段请求，告知客户端服务器支持以​​字节​​为单位请求部分资源（即断点续传）
    - `ETag: W/"204-1759760370604"`：对资源的标识（token）
    - `Last-Modified: Mon, 06 Oct 2025 14:19:30 GMT`：​最后修改时间​​：该资源在服务器上最后一次被修改的时间，用于缓存条件验证。如果多次请求下，这个值不发生改变，则不会重新响应整个页面。**转而返回304代码**
    - `Content-Type: text/html`：表示放哪会资源类型，浏览器会按照这样的格式解析。
    - `Content-Length: 204`：响应体长度，即浏览器要读取的字节数。
    - `Date: Tue, 07 Oct 2025 12:47:31 GMT`：服务器响应时间。

#### HTTP状态码详解

1. 基本概念

    - **HTTP状态码**（HTTP Status Code）是用以表示网页服务器HTTP响应状态的3位数字代码。当浏览器向服务器发出请求后，服务器会返回一个包含状态码的信息头（Server Header）来告知浏览器请求的处理结果。

    - 状态码由三个十进制数字组成，**第一个数字**定义了状态码的**类别**，后两个数字则用于区分具体状态。

2. 状态码分类

    HTTP状态码共分为5类，其分类依据及描述如下：

    | 分类代码范围 | 分类描述 | 说明 |
    | :--- | :--- | :--- |
    | **1xx** | **信息响应** (Informational) | 表示请求已被接收，需要继续处理 |
    | **2xx** | **成功响应** (Success) | 表示请求已成功被服务器接收、理解并接受 |
    | **3xx** | **重定向** (Redirection) | 表示需要客户端采取进一步的操作才能完成请求 |
    | **4xx** | **客户端错误** (Client Error) | 表示请求包含语法错误或无法完成请求 |
    | **5xx** | **服务器错误** (Server Error) | 表示服务器在处理请求的过程中发生了错误 |

3. 常见HTTP状态码列表

    - 1xx 信息响应

        | 状态码 | 英文名称 | 中文描述 |
        | :--- | :--- | :--- |
        | 100 | Continue | 继续。客户端应继续其请求。 |
        | 101 | Switching Protocols | 切换协议。服务器根据客户端的请求切换协议。 |

    - 2xx 成功响应

        | 状态码 | 英文名称 | 中文描述 |
        | :--- | :--- | :--- |
        | **200** | **OK** | **请求成功**。一般用于GET与POST请求。 |
        | 201 | Created | 已创建。成功请求并创建了新的资源。 |
        | 202 | Accepted | 已接受。已经接受请求，但未处理完成。 |
        | 204 | No Content | 无内容。服务器成功处理，但未返回内容。 |
        | 206 | Partial Content | 部分内容。服务器成功处理了部分GET请求。 |

    - 3xx 重定向

        | 状态码 | 英文名称 | 中文描述 |
        | :--- | :--- | :--- |
        | **301** | **Moved Permanently** | **永久移动**。请求的资源已被永久移动到新URI。 |
        | **302** | **Found** | **临时移动**。资源只是临时被移动，客户端应继续使用原有URI。 |
        | 304 | Not Modified | 未修改。所请求的资源未修改，客户端可直接使用缓存。 |

    - 4xx 客户端错误

        | 状态码 | 英文名称 | 中文描述 |
        | :--- | :--- | :--- |
        | **400** | **Bad Request** | **错误请求**。客户端请求的语法错误，服务器无法理解。 |
        | **401** | **Unauthorized** | **未授权**。请求要求用户的身份认证。 |
        | **403** | **Forbidden** | **禁止访问**。服务器理解请求，但拒绝执行。 |
        | **404** | **Not Found** | **未找到**。服务器无法根据客户端的请求找到资源（如网页）。 |
        | 405 | Method Not Allowed | 方法禁用。客户端请求中的方法被禁止。 |
        | 408 | Request Timeout | 请求超时。服务器等待客户端发送的请求时间过长。 |

    - 5xx 服务器错误

        | 状态码 | 英文名称 | 中文描述 |
        | :--- | :--- | :--- |
        | **500** | **Internal Server Error** | **内部服务器错误**。服务器遇到错误，无法完成请求。 |
        | **502** | **Bad Gateway** | **错误网关**。充当网关或代理的服务器从上游服务器收到了无效响应。 |
        | **503** | **Service Unavailable** | **服务不可用**。由于超载或系统维护，服务器暂时无法处理请求。 |
        | **504** | **Gateway Timeout** | **网关超时**。充当网关或代理的服务器未及时从上游服务器获取请求。 |

4. 工作原理简述

    HTTP事务处理通常遵循以下步骤：
    - **建立连接**：客户端与服务器建立连接。
    - **发送请求**：客户端向服务器发送请求。
    - **返回响应**：服务器处理请求，并返回包含状态码的响应。
    - **关闭连接**：一次请求/响应完成后，连接通常会被关闭（HTTP/1.1支持持久连接）。

#### HTTP常见请求和响应头

1. 核心概念
    - **HTTP头部**：HTTP协议中用于在客户端和服务器之间传递附加信息的字段，采用`键: 值`的格式。
    - **请求头**：由客户端发送，告知服务器关于客户端的能力、偏好和请求的上下文。
    - **响应头**：由服务器返回，描述了响应的属性、服务器信息以及对客户端的指令。

1. HTTP请求头速查表

    | 头部字段 | 解释 | 示例 |
    | :--- | :--- | :--- |
    | **Accept** | 指定客户端能够接收的内容类型 | `Accept: text/plain, text/html` |
    | **Accept-Charset** | 浏览器可以接受的字符编码集 | `Accept-Charset: iso-8859-5` |
    | **Accept-Encoding** | 指定浏览器可以支持的web服务器返回内容压缩编码类型 | `Accept-Encoding: compress, gzip` |
    | **Accept-Language** | 浏览器可接受的语言 | `Accept-Language: en,zh` |
    | **Accept-Ranges** | 可以请求网页实体的一个或者多个子范围字段 | `Accept-Ranges: bytes` |
    | **Authorization** | HTTP授权的授权证书 | `Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==` |
    | **Cache-Control** | 指定请求和响应遵循的缓存机制 | `Cache-Control: no-cache` |
    | **Connection** | 表示是否需要持久连接 | `Connection: close` |
    | **Cookie** | HTTP请求发送时，会把保存在该请求域名下的所有cookie值一起发送给web服务器 | `Cookie: $Version=1; Skin=new;` |
    | **Content-Length** | 请求的内容长度 | `Content-Length: 348` |
    | **Content-Type** | 请求的与实体对应的MIME信息 | `Content-Type: application/x-www-form-urlencoded` |
    | **Date** | 请求发送的日期和时间 | `Date: Tue, 15 Nov 2010 08:12:31 GMT` |
    | **Expect** | 请求的特定的服务器行为 | `Expect: 100-continue` |
    | **From** | 发出请求的用户的Email | `From: user@email.com` |
    | **Host** | 指定请求的服务器的域名和端口号 | `Host: www.zcmhi.com` |
    | **If-Match** | 只有请求内容与实体相匹配才有效 | `If-Match: "737060cd8c284d8af7ad3082f209582d"` |
    | **If-Modified-Since** | 如果请求的部分在指定时间之后被修改则请求成功，未被修改则返回304代码 | `If-Modified-Since: Sat, 29 Oct 2010 19:43:31 GMT` |
    | **If-None-Match** | 如果内容未改变返回304代码，参数为服务器先前发送的Etag，与服务器回应的Etag比较判断是否改变 | `If-None-Match: "737060cd8c284d8af7ad3082f209582d"` |
    | **If-Range** | 如果实体未改变，服务器发送客户端丢失的部分，否则发送整个实体。参数也为Etag | `If-Range: "737060cd8c284d8af7ad3082f209582d"` |
    | **If-Unmodified-Since** | 只在实体在指定时间之后未被修改才请求成功 | `If-Unmodified-Since: Sat, 29 Oct 2010 19:43:31 GMT` |
    | **Max-Forwards** | 限制信息通过代理和网关传送的时间 | `Max-Forwards: 10` |
    | **Pragma** | 用来包含实现特定的指令 | `Pragma: no-cache` |
    | **Proxy-Authorization** | 连接到代理的授权证书 | `Proxy-Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==` |
    | **Range** | 只请求实体的一部分，指定范围 | `Range: bytes=500-999` |
    | **Referer** | 先前网页的地址，当前请求网页紧随其后，即来路 | `Referer: http://www.zcmhi.com/archives/71.html` |
    | **TE** | 客户端愿意接受的传输编码，并通知服务器接受接受尾加头信息 | `TE: trailers,deflate;q=0.5` |
    | **Upgrade** | 向服务器指定某种传输协议以便服务器进行转换（如果支持） | `Upgrade: HTTP/2.0, SHTTP/1.3, IRC/6.9, RTA/x11` |
    | **User-Agent** | User-Agent的内容包含发出请求的用户信息 | `User-Agent: Mozilla/5.0 (Linux; X11)` |
    | **Via** | 通知中间网关或代理服务器地址，通信协议 | `Via: 1.0 fred, 1.1 nowhere.com (Apache/1.1)` |
    | **Warning** | 关于消息实体的警告信息 | `Warn: 199 Miscellaneous warning` |

1. HTTP响应头速查表

    | 头部字段 | 解释 | 示例 |
    | :--- | :--- | :--- |
    | **Accept-Ranges** | 表明服务器是否支持指定范围请求及哪种类型的分段请求 | `Accept-Ranges: bytes` |
    | **Age** | 从原始服务器到代理缓存形成的估算时间（以秒计，非负） | `Age: 12` |
    | **Allow** | 对某网络资源的有效的请求行为，不允许则返回405 | `Allow: GET, HEAD` |
    | **Cache-Control** | 告诉所有的缓存机制是否可以缓存及哪种类型 | `Cache-Control: no-cache` |
    | **Content-Encoding** | web服务器支持的返回内容压缩编码类型 | `Content-Encoding: gzip` |
    | **Content-Language** | 响应体的语言 | `Content-Language: en,zh` |
    | **Content-Length** | 响应体的长度 | `Content-Length: 348` |
    | **Content-Location** | 请求资源可替代的备用的另一地址 | `Content-Location: /index.htm` |
    | **Content-MD5** | 返回资源的MD5校验值 | `Content-MD5: Q2hlY2sgSW50ZWdyaXR5IQ==` |
    | **Content-Range** | 在整个返回体中本部分的字节位置 | `Content-Range: bytes 21010-47021/47022` |
    | **Content-Type** | 返回内容的MIME类型 | `Content-Type: text/html; charset=utf-8` |
    | **Date** | 原始服务器消息发出的时间 | `Date: Tue, 15 Nov 2010 08:12:31 GMT` |
    | **ETag** | 请求变量的实体标签的当前值 | `ETag: "737060cd8c284d8af7ad3082f209582d"` |
    | **Expires** | 响应过期的日期和时间 | `Expires: Thu, 01 Dec 2010 16:00:00 GMT` |
    | **Last-Modified** | 请求资源的最后修改时间 | `Last-Modified: Tue, 15 Nov 2010 12:45:26 GMT` |
    | **Location** | 用来重定向接收方到非请求URL的位置来完成请求或标识新的资源 | `Location: http://www.zcmhi.com/archives/94.html` |
    | **Pragma** | 包括实现特定的指令，它可应用到响应链上的任何接收方 | `Pragma: no-cache` |
    | **Proxy-Authenticate** | 它指出认证方案和可应用到代理的该URL上的参数 | `Proxy-Authenticate: Basic` |
    | **Refresh** | 应用于重定向或一个新的资源被创造，在5秒之后重定向 | `Refresh: 5; url=http://www.zcmhi.com/archives/94.html` |
    | **Retry-After** | 如果实体暂时不可取，通知客户端在指定时间之后再次尝试 | `Retry-After: 120` |
    | **Server** | web服务器软件名称 | `Server: Apache/1.3.27 (Unix) (Red-Hat/Linux)` |
    | **Set-Cookie** | 设置Http Cookie | `Set-Cookie: UserID=JohnDoe; Max-Age=3600; Version=1` |
    | **Trailer** | 指出头域在分块传输编码的尾部存在 | `Trailer: Max-Forwards` |
    | **Transfer-Encoding** | 文件传输编码 | `Transfer-Encoding: chunked` |
    | **Vary** | 告诉下游代理是使用缓存响应还是从原始服务器请求 | `Vary: *` |
    | **Via** | 告知代理客户端响应是通过哪里发送的 | `Via: 1.0 fred, 1.1 nowhere.com (Apache/1.1)` |
    | **Warning** | 警告实体可能存在的问题 | `Warning: 199 Miscellaneous warning` |
    | **WWW-Authenticate** | 表明客户端请求实体应该使用的授权方案 | `WWW-Authenticate: Basic` |

1. 实用备注
    - **内容协商**：`Accept*`系列请求头（如`Accept`, `Accept-Language`）让客户端告知服务器其偏好，服务器据此返回最合适的内容 。
    - **条件请求**：配合`If-*`请求头（如`If-Modified-Since`, `If-None-Match`）和`ETag`/`Last-Modified`响应头，可高效验证缓存有效性，节省带宽 。
    - **连接管理**：`Connection: keep-alive`（HTTP/1.1默认）允许在同一TCP连接上发送多个请求/响应，减少建立连接的开销，提升性能 。

### 快速入门

#### 状态码演示

1. 下面是常见的HTTP状态码：
    - **`200`** - 请求成功
    - **`301`** - 资源（网页等）被永久转移到其它URL
    - **`404`** - 请求的资源（网页等）不存在
    - **`500`** - 内部服务器错误

2. 状态码`500`演示
    - 在对应`doGet`方法中添加如下代码
        ```java
        // 众所周知，这里会报错
        System.out.println(10/0);
        ```
    - 重新部署后，在浏览器输入对应服务地址后，响应头第一行为：
        ```http
        HTTP/1.1 500 Internal Server Error
        ```

#### 什么是HTTP协议

1. 超文本传输协议(HTTP，HyperText Transfer Protocol)是互联网上应用广泛的一种网络协议。是工作在 tcp/ip 协议基础上的,所有的 WWW 文件都遵守这个标准。
2. http1.0 短连接 和 http1.1 长连接（目前使用）
3. http 是 TCP/IP 协议的一个应用层协议,http 也是我们 web 开发的基础

#### 用谷歌浏览器抓包

1. 快捷键`Ctrl+Shift+I`进入开发者工具
2. 在`网络`或`Network`选项卡下查看抓包结果。

#### 页面请求次数分析

1. 分析下面网页向服务器请求的次数
    - 共3次，html页面1次，两张图片请求两次
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>hi</title>
    </head>
    <body>
    <h1>Hello,user!</h1>
    <!--<img src="http://localhost:8080/http/01.jpg">-->
    <img src="01.jpg"  alt=""/>
    <img src="http://localhost:8080/http/02.jpg" alt=""/>
    </body>
    </html>
    ```

2. 响应头分析
    - 内容类型为图片`image/jpeg`
    ```http
    HTTP/1.1 200 OK
    Server: Apache-Coyote/1.1
    Accept-Ranges: bytes
    ETag: W/"183091-1756901682582"
    Last-Modified: Wed, 03 Sep 2025 12:14:42 GMT
    Content-Type: image/jpeg
    Content-Length: 183091
    Date: Tue, 07 Oct 2025 14:58:10 GMT
    ```

3. 请求头分析
    ```http
    GET /http/02.jpg HTTP/1.1
    Host: localhost:8080
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:143.0) Gecko/20100101 Firefox/143.0
    Accept: image/avif,image/webp,image/png,image/svg+xml,image/*;q=0.8,*/*;q=0.5
    Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
    Accept-Encoding: gzip, deflate, br, zstd
    Connection: keep-alive
    Referer: http://localhost:8080/http/hi.html
    Cookie: Idea-e0c37dd3=7f4f7749-282f-455b-8e3d-292e8ce0f3d2
    Sec-Fetch-Dest: image
    Sec-Fetch-Mode: no-cors
    Sec-Fetch-Site: same-origin
    Priority: u=4, i
    ```

### HTTP请求包分析

#### GET分析

1. GET请求在前面已提及。

#### POST分析

1. `login.html`
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>login</title>
    </head>
    <body>
    <form action="/http/submit01" method="post">
        用户名：
        <input type="text" name="username"/><br/>
        密码：
        <input type="password" name="password"/><br/>
        <input type="submit" value="提交">
        <input type="reset"  value="清空">
    </form>
    </body>
    </html>
    ```

2. `LoginServlet.java`
    ```java
    package com.lcq.servlet;

    import javax.servlet.ServletException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import java.io.IOException;
    import java.io.PrintWriter;

    @WebServlet(urlPatterns = {"/submit01"})
    public class LoginServlet extends HttpServlet {
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            // 服务端输出
            System.out.println("doGet");
            // 给浏览器回送数据
            // 1. 通过response对象获取流PrintWriter
            // 2. 给回送数据设置编码
            // 3. 这里的“text/html”是MIME类型（大类型/小类型）
            // 4. charset设置编码
            // 5. 设置编码要在得到流之前，否则无效
            resp.setContentType("text/html;charset=UTF-8");
            PrintWriter writer = resp.getWriter();
            writer.println("<h1>OK！？</h1>");

            // 保险起见，可以加上flush/close
            // 这个方法用于将缓存数据刷出，并关闭流释放资源。
            writer.flush();
            writer.close();
        }

        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            System.out.println("doPost");

            resp.setContentType("text/html;charset=UTF-8");
            PrintWriter writer = resp.getWriter();
            writer.println("<h1>OK！？</h1><br/><div>by post<div>");
            writer.flush();
            writer.close();
        }
    }
    ```

3. 请求头
    - 分为请求行、请求头、请求体
    ```http
    POST /http/submit01 HTTP/1.1
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
    Accept-Encoding: gzip, deflate, br, zstd
    Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
    Cache-Control: max-age=0
    Connection: keep-alive
    Content-Length: 44
    Content-Type: application/x-www-form-urlencoded
    Cookie: Idea-e0c37dd3=7f4f7749-282f-455b-8e3d-292e8ce0f3d2
    Host: localhost:8080
    Origin: http://localhost:8080
    Referer: http://localhost:8080/http/login.html
    Sec-Fetch-Dest: document
    Sec-Fetch-Mode: navigate
    Sec-Fetch-Site: same-origin
    Sec-Fetch-User: ?1
    Upgrade-Insecure-Requests: 1
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
    sec-ch-ua: "Chromium";v="140", "Not=A?Brand";v="24", "Google Chrome";v="140"
    sec-ch-ua-mobile: ?0
    sec-ch-ua-platform: "Windows"

    username=lcq&password=123456
    ```

    - `Content-Type: application/x-www-form-urlencoded`：表示提交的数据格式。
        - `x-www-form-urlencoded`：表单数据是url编码（汉字显示为`%E2`的类似形式）
        - 可以自行找一个url编解码器对编码数据进行解码。

    - `Content-Length: 44`：发送数据（请求体）长度为44字节
    - `Origin: http://localhost:8080`：与Host不同，这是发出请求的地址。Host是目标地址。


#### GET请求和POST请求区别

1. GET请求有哪些
    - `<form>`标签，`method="get"`
    - `<a>`标签，必然是get请求。
    - `<link>`标签，引入css文件。
    - `<script>`标签，引入js文件
    - `<img>`标签
    - `<iframe>`标签
    - 浏览器地址栏直接回车。

2. POST请求有哪些
    - `<form>`标签，`method="post"`

3. HTTP 请求中怎样选择 Get 和 Post 方式
    - 在大部分情况下，我们不需要考虑这个问题，因为业务本身就会自动区别，比如你要显示图片，引入 css/js 这个天然的就是 get 请求，比如你登录，发帖，上传文件，你就会使用 post(感情的自然流露)
    - 传输的数据大小区别
        - get 传送的数据量较小。不能大于 2KB(不同浏览器不一样)。
        - post 传送的数据量较大。一般默认不受限制。
    - 什么情况下使用 post 请求
        - post 请求是会在浏览器上隐藏參数部分的，在安全要求的部分都会使用到 POST 请求。如用户登录。数据增上改等等。都会把參数隐藏起来，这样就不会通过你的请求暴露你的參数格式。比方：`del?id=1`，别人就能够用 `del?id=3` 来删除你其它数据。
        - 在向 server 传递数据较大的时候。使用 POST，get 是有限制的, 比如发帖, 上传文件
    - 什么情况下使用 get 方式
        - 在前台页面展示，比如分页内容等，可以保留传递参数, 可用来非常好的分享和传播, POST 中链接地址是不变化的。
4. 建议：
    - get 方式的安全性较 Post 方式要差些。包括机密信息的话。建议用 Post 数据提交方式
    - 在做数据查询时。建议用 Get 方式；而在做数据加入、改动或删除时，建议用 Post方式

### HTTP响应包

1. 组成部分
    - 响应行
    - 响应头
    - 响应体
    - 服务器响应时，将三者一并返回，在开发者调试工具中，响应体通常被单独列出。

2. HTTP响应分析（详情见前面内容）

3. 一个完整的响应
    - **注意响应头和响应体之间有一个空行**
    ```http
    HTTP/1.1 200 OK
    Server: Apache-Coyote/1.1
    Accept-Ranges: bytes
    ETag: W/"400-1759928855219"
    Last-Modified: Wed, 08 Oct 2025 13:07:35 GMT
    Content-Type: text/html
    Content-Length: 400
    Date: Wed, 08 Oct 2025 13:10:26 GMT

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>login</title>
    </head>
    <body>
    <form action="/http/submit01" method="post">
        用户名：
        <input type="text" name="username"/><br/>
        密码：
        <input type="password" name="password"/><br/>
        <input type="submit" value="提交">
        <input type="reset"  value="清空">
    </form>
    </body>
    </html>
    ```

#### 常用响应状态码

1. 常用状态码
    - 200 - OK - 请求成功
    - 301 - Moved Permanently - 永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替。
    - 302 - Found - 临时移动，与301类似，但资源只是临时被移动，客户端应继续使用原有的URI。（**重定向**）
    - 304 - Not Modified - 未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源。
    - 404 - 请求的资源（网页等）不存在
    - 500 - 内部服务器错误

2. 302状态码演示
    - 浏览器请求`T1Servlet`服务。
    - `T1Servlet`返回状态码`302`，并重定向到`hi.html`
    - 浏览器发送第二次请求`hi.html`
    - 示例
        ```java
        package com.lcq.servlet;

        import javax.servlet.ServletException;
        import javax.servlet.annotation.WebServlet;
        import javax.servlet.http.HttpServlet;
        import javax.servlet.http.HttpServletRequest;
        import javax.servlet.http.HttpServletResponse;
        import java.io.IOException;

        @WebServlet(urlPatterns = {"/T1Servlet"})
        public class T1Servlet extends HttpServlet {
            @Override
            protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
                // 如果有请求进来，则进行重定向
                System.out.println(req.getContextPath());
                resp.sendRedirect(req.getContextPath()+"/hi.html");
            }

            @Override
            protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
                this.doGet(req, resp);
            }
        }
        ```
    - 响应
        ```http
        HTTP/1.1 302 Found
        Server: Apache-Coyote/1.1
        Location: /hi.html
        Content-Length: 0
        Date: Wed, 08 Oct 2025 13:54:27 GMT
        ```

3. 304状态码演示
    - 取消开发者工具中的`禁用缓存`选项
    - 从第二次请求开始，请求头会添加参数`If-Modified-Since`，这个参数是从响应头中获取的相应内容最后修改的时间`Last-Modified`。（如果禁用缓存，请求头不会带这个参数）
    - 如果最后修改时间未改变，则返回`304 Not Modified`

4. 404页面替换
    - 可以在`<webapp>`下添加如下配置，切换为自定义404页面
    ```xml
    <error-page>
        <error-code>404</error-code>
        <location>/404.html</location>
    </error-page>
    ```
### MIME 类型

1. 简介
    - MIME 是 HTTP 协议中数据类型。
    - MIME 的英文全称是"Multipurpose Internet Mail Extensions" 多功能 Internet 邮件扩充服务。
    - MIME 类型的格式是"大类型/小类型"，并与某一种文件的扩展名相对应
    - 由响应包的`Content-Type`指定。

2. 常见的MIME类型
    | 文件类型             | 扩展名                      | 表达形式（MIME 类型）        |
    | :------------------- | :-------------------------- | :--------------------------- |
    | 超文本标记语言文本   | `.html`, `.htm`             | `text/html`                  |
    | 普通文本             | `.txt`                      | `text/plain`                 |
    | RTF文本              | `.rtf`                      | `application/rtf`            |
    | GIF图形              | `.gif`                      | `image/gif`                  |
    | JPEG图形             | `.jpeg`, `.jpg`             | `image/jpeg`                 |
    | au声音文件           | `.au`                       | `audio/basic`                |
    | MIDI音乐文件         | `.mid`, `.midi`             | `audio/midi`, `audio/x-midi` |
    | RealAudio音乐文件    | `.ra`, `.ram`               | `audio/x-pn-realaudio`       |
    | MPEG文件             | `.mpg`, `.mpeg`             | `video/mpeg`                 |
    | AVI文件              | `.avi`                      | `video/x-msvideo`            |
    | GZIP文件             | `.gz`                       | `application/x-gzip`         |
    | TAR文件              | `.tar`                      | `application/x-tar`           |

