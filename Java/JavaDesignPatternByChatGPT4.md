# Java设计模式学习笔记

写在前面：
- 这份笔记以 Java 语境整理常见设计模式，重点不是死记定义，而是搞清楚：什么时候该用、它替代了什么硬编码、在 JDK 和常见框架里能看到什么影子。
- 阅读建议：创建型先看“对象怎么来”，结构型再看“类之间怎么拼”，行为型重点掌握“职责怎么分、流程怎么控”。

模式分类速览：
1. 创建型：简单工厂、单例、工厂方法、抽象工厂、建造者、原型
2. 结构型：适配器、桥接、组合、装饰、外观、享元、代理
3. 行为型：责任链、命令、解释器、迭代器、中介者、备忘录、观察者、状态、策略、模板方法、访问者

## 简单工厂模式（创建型，补充）
1. 核心思想
    - 将“创建哪一个对象”的判断集中到一个工厂类中，客户端只负责提需求，不直接 `new` 具体实现类。
    - 它解决的是“对象选择逻辑四处分散”的问题，尤其适合对象种类不多、创建规则比较稳定的场景。

2. 适用场景
    - 根据消息类型创建不同通知器。
    - 根据配置文件中的字符串创建解析器、校验器、支付通道等。

3. 角色分工/结构说明
    - `Notifier`：抽象产品，定义统一发送行为。
    - `EmailNotifier`、`SmsNotifier`、`WechatNotifier`：具体产品。
    - `NotifierFactory`：简单工厂，负责根据条件返回具体产品。

4. Java 实际范例
    ```java
    interface Notifier {
        void send(String message);
    }

    class EmailNotifier implements Notifier {
        public void send(String message) {
            System.out.println("邮件通知: " + message);
        }
    }

    class SmsNotifier implements Notifier {
        public void send(String message) {
            System.out.println("短信通知: " + message);
        }
    }

    class WechatNotifier implements Notifier {
        public void send(String message) {
            System.out.println("企业微信通知: " + message);
        }
    }

    class NotifierFactory {
        public static Notifier create(String type) {
            if ("email".equalsIgnoreCase(type)) {
                return new EmailNotifier();
            }
            if ("sms".equalsIgnoreCase(type)) {
                return new SmsNotifier();
            }
            if ("wechat".equalsIgnoreCase(type)) {
                return new WechatNotifier();
            }
            throw new IllegalArgumentException("不支持的通知类型: " + type);
        }
    }

    class Client {
        public static void main(String[] args) {
            Notifier notifier = NotifierFactory.create("sms");
            notifier.send("订单已支付");
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `Calendar.getInstance()`：调用方不关心具体返回 `GregorianCalendar` 还是其他实现，先拿到能用的对象再说。
    - `NumberFormat.getInstance()`：根据区域或默认规则返回合适的格式化器，本质上也是“统一入口 + 条件选型”。

6. 优缺点与注意事项
    - 优点：客户端依赖抽象，创建逻辑集中，调用简单。
    - 缺点：新增一种产品通常要改工厂，违反开闭原则。
    - 注意：如果 `if-else` 分支越来越多，说明简单工厂开始吃力，应考虑升级为工厂方法或抽象工厂。
    - 和工厂方法的区别：简单工厂只有一个“总工厂”，工厂方法则把创建职责下放到多个具体工厂中。

## 单例模式（创建型）
1. 核心思想
    - 保证一个类在系统中只有一个实例，并提供全局访问点。
    - 它解决的是“同一类资源被多次创建导致状态不一致”或“对象创建成本高”的问题。

2. 适用场景
    - 配置中心、线程池管理器、缓存管理器、日志管理器。
    - 整个应用只需要一个协调对象，并且这个对象需要被多处共享。

3. 角色分工/结构说明
    - 单例类自己负责：
        - 私有化构造器
        - 保存唯一实例
        - 对外暴露获取实例的方法

4. Java 实际范例
    ```java
    class ConfigCenter {
        private ConfigCenter() {
        }

        private static class Holder {
            private static final ConfigCenter INSTANCE = new ConfigCenter();
        }

        public static ConfigCenter getInstance() {
            return Holder.INSTANCE;
        }

        public String get(String key) {
            if ("timeout".equals(key)) {
                return "3000";
            }
            return "undefined";
        }
    }

    class Client {
        public static void main(String[] args) {
            ConfigCenter c1 = ConfigCenter.getInstance();
            ConfigCenter c2 = ConfigCenter.getInstance();

            System.out.println(c1 == c2);
            System.out.println(c1.get("timeout"));
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `Runtime.getRuntime()`：JDK 中非常经典的单例入口。
    - Spring 默认单例作用域 `singleton`：虽然和 GoF 单例不完全等价，但使用效果上也是“容器内默认共享同一个 Bean 实例”。

6. 优缺点与注意事项
    - 优点：节省资源，保证状态统一，访问方便。
    - 缺点：容易被滥用成“全局变量”，导致模块耦合升高、测试困难。
    - 注意：多线程下不要写出线程不安全的懒汉式；静态内部类和枚举写法都很稳。
    - 单例不是银弹，能通过依赖注入管理生命周期时，优先让容器管理，而不是到处手写单例。

## 工厂方法模式（创建型）
1. 核心思想
    - 定义创建对象的接口，但把“实例化哪一个具体类”交给子类工厂决定。
    - 它解决的是“新增产品时总工厂频繁修改”的问题。

2. 适用场景
    - 不同导出器对应不同创建流程。
    - 框架允许业务方扩展具体产品，但不想改动原有核心创建代码。

3. 角色分工/结构说明
    - `Exporter`：抽象产品。
    - `PdfExporter`、`ExcelExporter`：具体产品。
    - `ExporterFactory`：抽象工厂方法声明。
    - `PdfExporterFactory`、`ExcelExporterFactory`：具体工厂。

4. Java 实际范例
    ```java
    interface Exporter {
        String export(String data);
    }

    class PdfExporter implements Exporter {
        public String export(String data) {
            return "PDF: " + data;
        }
    }

    class ExcelExporter implements Exporter {
        public String export(String data) {
            return "EXCEL: " + data;
        }
    }

    interface ExporterFactory {
        Exporter createExporter();
    }

    class PdfExporterFactory implements ExporterFactory {
        public Exporter createExporter() {
            return new PdfExporter();
        }
    }

    class ExcelExporterFactory implements ExporterFactory {
        public Exporter createExporter() {
            return new ExcelExporter();
        }
    }

    class Client {
        public static void main(String[] args) {
            ExporterFactory factory = new PdfExporterFactory();
            Exporter exporter = factory.createExporter();
            System.out.println(exporter.export("日报"));
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `Collection.iterator()`：集合本身像工厂，返回适配自身结构的迭代器。
    - `ThreadFactory`：并发框架中把线程创建策略抽出来，正是工厂方法思想。

6. 优缺点与注意事项
    - 优点：新增产品时通常新增一个具体工厂即可，符合开闭原则。
    - 缺点：类数量增加，结构比简单工厂更重。
    - 注意：如果不仅“产品种类”要扩展，而且“一组关联产品”要一起切换，更适合抽象工厂。
    - 和简单工厂的区别：简单工厂强调“统一入口选对象”，工厂方法强调“把创建职责分散给不同工厂”。

## 抽象工厂模式（创建型）
1. 核心思想
    - 提供一个接口，用于创建一整套彼此关联或相互依赖的对象族，而不需要指定具体类。
    - 它解决的是“一个切换动作要同时切换多个相关对象”的问题。

2. 适用场景
    - 跨数据库切换时，需要同时切换 `UserDao`、`OrderDao`、`ProductDao`。
    - 多套 UI 主题、多套设备驱动、多套云厂商 SDK 接口适配。

3. 角色分工/结构说明
    - `DatabaseFactory`：抽象工厂，负责生产一族 DAO。
    - `MysqlFactory`、`PostgreSqlFactory`：具体工厂。
    - `UserDao`、`OrderDao`：抽象产品。
    - `MysqlUserDao`、`PostgreSqlOrderDao`：具体产品。

4. Java 实际范例
    ```java
    interface UserDao {
        void saveUser(String name);
    }

    interface OrderDao {
        void saveOrder(String orderNo);
    }

    class MysqlUserDao implements UserDao {
        public void saveUser(String name) {
            System.out.println("MySQL 保存用户: " + name);
        }
    }

    class MysqlOrderDao implements OrderDao {
        public void saveOrder(String orderNo) {
            System.out.println("MySQL 保存订单: " + orderNo);
        }
    }

    class PostgreSqlUserDao implements UserDao {
        public void saveUser(String name) {
            System.out.println("PostgreSQL 保存用户: " + name);
        }
    }

    class PostgreSqlOrderDao implements OrderDao {
        public void saveOrder(String orderNo) {
            System.out.println("PostgreSQL 保存订单: " + orderNo);
        }
    }

    interface DatabaseFactory {
        UserDao createUserDao();
        OrderDao createOrderDao();
    }

    class MysqlFactory implements DatabaseFactory {
        public UserDao createUserDao() {
            return new MysqlUserDao();
        }

        public OrderDao createOrderDao() {
            return new MysqlOrderDao();
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `DocumentBuilderFactory`：先拿工厂，再由工厂创建解析器实例，隐藏底层实现差异。
    - JDBC 驱动切换、Spring 中按 profile 切换一组基础设施 Bean，也常带有抽象工厂思维。

6. 优缺点与注意事项
    - 优点：一键切换产品族，能保证同一族对象协同工作。
    - 缺点：新增“产品等级结构”很麻烦，例如突然多一个 `LogDao`，所有具体工厂都要补。
    - 注意：抽象工厂关注的是“整族对象的一致性”，不是单个对象的创建细节。
    - 和工厂方法的区别：工厂方法通常管一个产品，抽象工厂通常管多个关联产品。

## 建造者模式（创建型）
1. 核心思想
    - 将一个复杂对象的构建过程拆成多个步骤，允许按顺序逐步组装，最后得到成品。
    - 它解决的是“构造参数太多、可选参数太多、创建流程复杂”的问题。

2. 适用场景
    - 构造 SQL 查询对象、HTTP 请求对象、复杂报表参数对象。
    - 一个对象有很多可选参数，不希望出现一长串构造器重载。

3. 角色分工/结构说明
    - `QueryRequest`：最终产品。
    - `Builder`：负责分步骤设置条件。
    - `build()`：在最后统一校验并生成不可变对象。

4. Java 实际范例
    ```java
    class QueryRequest {
        private final String table;
        private final String where;
        private final String orderBy;
        private final int limit;

        private QueryRequest(Builder builder) {
            this.table = builder.table;
            this.where = builder.where;
            this.orderBy = builder.orderBy;
            this.limit = builder.limit;
        }

        public String toSql() {
            return "select * from " + table + " where " + where
                    + " order by " + orderBy + " limit " + limit;
        }

        static class Builder {
            private String table;
            private String where = "1=1";
            private String orderBy = "id desc";
            private int limit = 20;

            public Builder table(String table) {
                this.table = table;
                return this;
            }

            public Builder where(String where) {
                this.where = where;
                return this;
            }

            public Builder orderBy(String orderBy) {
                this.orderBy = orderBy;
                return this;
            }

            public Builder limit(int limit) {
                this.limit = limit;
                return this;
            }

            public QueryRequest build() {
                if (table == null || table.isEmpty()) {
                    throw new IllegalStateException("table 不能为空");
                }
                return new QueryRequest(this);
            }
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `StringBuilder`：不断追加片段，最后得到完整字符串，体现了逐步构建思想。
    - `Stream.Builder`：先逐个添加元素，再统一 `build()`。
    - Lombok 的 `@Builder` 也是企业项目中很常见的建造者落地方式。

6. 优缺点与注意事项
    - 优点：创建过程清晰，支持链式调用，可读性好，适合构建不可变对象。
    - 缺点：对象简单时会显得啰嗦。
    - 注意：建造者模式关注“组装过程”，抽象工厂关注“产品族切换”，不要混用概念。
    - 当一个对象只有两三个参数时，直接构造器或静态工厂通常更合适。

## 原型模式（创建型）
1. 核心思想
    - 通过复制现有对象来创建新对象，而不是每次都从头 `new` 并初始化。
    - 它解决的是“对象初始化成本高”或者“需要在现有对象基础上快速生成相似对象”的问题。

2. 适用场景
    - 克隆活动模板、表单模板、商品草稿、默认配置对象。
    - 对象创建依赖数据库查询、远程调用、复杂初始化时。

3. 角色分工/结构说明
    - 原型对象自己提供复制能力。
    - 客户端从已有原型复制，再按需改少量字段。

4. Java 实际范例
    ```java
    import java.util.ArrayList;
    import java.util.List;

    class ActivityTemplate implements Cloneable {
        private String title;
        private List<String> tags = new ArrayList<>();

        public ActivityTemplate(String title, List<String> tags) {
            this.title = title;
            this.tags.addAll(tags);
        }

        public void setTitle(String title) {
            this.title = title;
        }

        public List<String> getTags() {
            return tags;
        }

        @Override
        public ActivityTemplate clone() {
            try {
                ActivityTemplate copy = (ActivityTemplate) super.clone();
                copy.tags = new ArrayList<>(this.tags);
                return copy;
            } catch (CloneNotSupportedException e) {
                throw new RuntimeException(e);
            }
        }

        @Override
        public String toString() {
            return "ActivityTemplate{title='" + title + "', tags=" + tags + "}";
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `Object.clone()`：原型模式最直接的语言层入口。
    - `ArrayList.clone()`：会复制列表本身，但元素如果是引用类型，仍需关注深浅拷贝问题。

6. 优缺点与注意事项
    - 优点：减少重复初始化，提高创建效率。
    - 缺点：复制逻辑容易出坑，尤其是深拷贝、循环引用、不可变字段。
    - 注意：`clone()` 在 Java 社区里争议较大，实际项目中常用拷贝构造器、MapStruct、序列化拷贝等方式替代。
    - 核心不是“会不会 clone”，而是“是否真的适合通过复制已有状态来生成新对象”。

## 适配器模式（结构型）
1. 核心思想
    - 把一个类的接口转换成客户端期望的另一个接口，让原本不兼容的类可以协同工作。
    - 它解决的是“老代码不能改，但新系统接口已经定了”的问题。

2. 适用场景
    - 旧支付接口接入新支付网关。
    - 第三方 SDK 返回格式和自己系统标准不一致。

3. 角色分工/结构说明
    - `NewPayGateway`：目标接口，客户端想要的标准。
    - `LegacyPayService`：旧系统接口。
    - `LegacyPayAdapter`：适配器，把旧接口包装成新接口。

4. Java 实际范例
    ```java
    interface NewPayGateway {
        boolean pay(String userId, int cents);
    }

    class LegacyPayService {
        public String submit(String account, double amount) {
            System.out.println("旧系统发起支付: " + account + ", " + amount);
            return "SUCCESS";
        }
    }

    class LegacyPayAdapter implements NewPayGateway {
        private final LegacyPayService legacyPayService;

        public LegacyPayAdapter(LegacyPayService legacyPayService) {
            this.legacyPayService = legacyPayService;
        }

        public boolean pay(String userId, int cents) {
            double amount = cents / 100.0;
            return "SUCCESS".equals(legacyPayService.submit(userId, amount));
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `InputStreamReader`：把字节流接口适配成字符流接口，是 JDK 中极经典的适配器。
    - Spring MVC 参数解析器、第三方支付 SDK 包装层，经常在做“协议适配”和“参数适配”。

6. 优缺点与注意事项
    - 优点：复用旧代码，不必强行改动稳定系统。
    - 缺点：层次变多，适配逻辑复杂时调试成本上升。
    - 注意：适配器是“事后兼容”，不是一开始就设计出来的抽象。
    - 和桥接的区别：适配器是为了解决“不兼容”，桥接是为了防止“两个维度一起爆炸”。

## 桥接模式（结构型）
1. 核心思想
    - 将抽象部分与实现部分分离，使它们都可以独立变化。
    - 它解决的是“如果两个维度都在扩展，继承会导致类爆炸”的问题。

2. 适用场景
    - 消息类型和发送渠道都能独立扩展。
    - 设备类型和控制方式、图形类型和渲染平台都可独立变化。

3. 角色分工/结构说明
    - `Message`：抽象维度。
    - `Sender`：实现维度。
    - 两者通过组合关系连接，而不是通过继承硬绑在一起。

4. Java 实际范例
    ```java
    interface Sender {
        void send(String title, String content);
    }

    class SmsSender implements Sender {
        public void send(String title, String content) {
            System.out.println("短信发送: " + title + " - " + content);
        }
    }

    class EmailSender implements Sender {
        public void send(String title, String content) {
            System.out.println("邮件发送: " + title + " - " + content);
        }
    }

    abstract class Message {
        protected final Sender sender;

        protected Message(Sender sender) {
            this.sender = sender;
        }

        abstract void push(String content);
    }

    class UrgentMessage extends Message {
        public UrgentMessage(Sender sender) {
            super(sender);
        }

        void push(String content) {
            sender.send("【加急】", content);
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - JDBC 驱动体系可以理解为桥接思想的一种典型体现：上层是统一的 JDBC API，下层是不同数据库厂商驱动实现。
    - 日志门面和具体日志实现分离，也带有很强的桥接味道。

6. 优缺点与注意事项
    - 优点：避免类爆炸，两个维度都能独立演化。
    - 缺点：增加抽象层，系统理解成本变高。
    - 注意：当一个类只有单一变化维度时，没必要上桥接。
    - 和适配器的区别：桥接是“提前设计好解耦”，适配器是“后期补救兼容”。

## 组合模式（结构型）
1. 核心思想
    - 将对象组织成树形结构，使客户端对单个对象和组合对象的使用保持一致。
    - 它解决的是“层级结构递归处理困难、叶子和容器代码分裂”的问题。

2. 适用场景
    - 公司部门树、菜单树、文件目录树、组织架构树。
    - 任何“整体-部分”层级明显的场景。

3. 角色分工/结构说明
    - `Node`：统一抽象。
    - `FileNode`：叶子节点。
    - `FolderNode`：组合节点，可继续持有子节点。

4. Java 实际范例
    ```java
    import java.util.ArrayList;
    import java.util.List;

    abstract class Node {
        protected final String name;

        protected Node(String name) {
            this.name = name;
        }

        abstract void print(String prefix);
    }

    class FileNode extends Node {
        public FileNode(String name) {
            super(name);
        }

        void print(String prefix) {
            System.out.println(prefix + "- 文件: " + name);
        }
    }

    class FolderNode extends Node {
        private final List<Node> children = new ArrayList<>();

        public FolderNode(String name) {
            super(name);
        }

        public void add(Node node) {
            children.add(node);
        }

        void print(String prefix) {
            System.out.println(prefix + "+ 目录: " + name);
            for (Node child : children) {
                child.print(prefix + "  ");
            }
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `java.awt.Container`：容器中既能放普通组件，也能放其他容器，天然是组合结构。
    - 前端组件树、组织权限树、评论回复树都是组合模式高频场景。

6. 优缺点与注意事项
    - 优点：统一处理单个对象和组合对象，递归遍历自然。
    - 缺点：有时会让叶子节点暴露一些本不该支持的方法。
    - 注意：组合模式关注树结构的一致对待，不是简单的“List 里放对象”。
    - 如果层级只有一层，或者不会递归扩展，通常不需要组合模式。

## 装饰模式（结构型）
1. 核心思想
    - 在不修改原类的前提下，动态地给对象增加额外职责。
    - 它解决的是“继承扩展过多、功能组合不灵活”的问题。

2. 适用场景
    - 给输入流增加缓冲、统计、日志、压缩等附加能力。
    - 给服务调用增加缓存、监控、限流等横切能力。

3. 角色分工/结构说明
    - `Reader`：抽象组件。
    - `FileReaderImpl`：具体组件。
    - `ReaderDecorator`：装饰器基类，持有组件引用。
    - `BufferingReaderDecorator`、`MetricsReaderDecorator`：具体装饰器。

4. Java 实际范例
    ```java
    interface Reader {
        String read();
    }

    class FileReaderImpl implements Reader {
        public String read() {
            return "file-content";
        }
    }

    abstract class ReaderDecorator implements Reader {
        protected final Reader reader;

        protected ReaderDecorator(Reader reader) {
            this.reader = reader;
        }
    }

    class BufferingReaderDecorator extends ReaderDecorator {
        public BufferingReaderDecorator(Reader reader) {
            super(reader);
        }

        public String read() {
            System.out.println("先走缓冲区");
            return reader.read();
        }
    }

    class MetricsReaderDecorator extends ReaderDecorator {
        public MetricsReaderDecorator(Reader reader) {
            super(reader);
        }

        public String read() {
            long start = System.nanoTime();
            String result = reader.read();
            System.out.println("读取耗时(ns): " + (System.nanoTime() - start));
            return result;
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `BufferedInputStream`、`BufferedReader`：JDK 中最经典的装饰器模式。
    - Spring 中某些包装式增强、缓存包装器、监控包装器，也常体现装饰思想。

6. 优缺点与注意事项
    - 优点：比继承更灵活，可按需叠加多个功能。
    - 缺点：对象层级较多时，调试调用链会变长。
    - 注意：装饰器强调“功能增强但接口不变”。
    - 和代理的区别：装饰器重点是“加功能”，代理重点是“控制访问”；二者结构相似，但目的不同。

## 外观模式（结构型）
1. 核心思想
    - 为复杂子系统提供一个统一的高层入口，屏蔽内部细节。
    - 它解决的是“客户端直接依赖多个子系统，调用顺序混乱、使用门槛高”的问题。

2. 适用场景
    - 一键下单、一键部署、一键导出报表。
    - 历史系统很复杂，希望对外只暴露一个简单入口。

3. 角色分工/结构说明
    - 多个子系统类分别干自己的事。
    - `Facade` 负责组织调用顺序，对外暴露更简单的业务接口。

4. Java 实际范例
    ```java
    class InventoryService {
        public boolean reserve(String sku) {
            System.out.println("锁定库存: " + sku);
            return true;
        }
    }

    class PaymentService {
        public boolean pay(String orderNo) {
            System.out.println("完成支付: " + orderNo);
            return true;
        }
    }

    class LogisticsService {
        public void createShipment(String orderNo) {
            System.out.println("生成物流单: " + orderNo);
        }
    }

    class OrderFacade {
        private final InventoryService inventoryService = new InventoryService();
        private final PaymentService paymentService = new PaymentService();
        private final LogisticsService logisticsService = new LogisticsService();

        public void placeOrder(String orderNo, String sku) {
            if (inventoryService.reserve(sku) && paymentService.pay(orderNo)) {
                logisticsService.createShipment(orderNo);
            }
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `java.nio.file.Files`：对复杂的文件操作底层 API 做了大量门面式封装。
    - 很多 SDK 的 `Client` 类，本质上就是一组复杂 HTTP、签名、重试逻辑的外观。

6. 优缺点与注意事项
    - 优点：降低使用门槛，隔离子系统复杂度。
    - 缺点：外观类可能越来越胖，变成“上帝类”。
    - 注意：外观模式不阻止你直接访问子系统，它只是提供一个更易用的入口。
    - 和中介者的区别：外观是对外简化入口，中介者是协调内部对象交互。

## 享元模式（结构型）
1. 核心思想
    - 通过共享细粒度对象来减少内存占用，把可共享状态和不可共享状态拆开。
    - 它解决的是“系统中大量相似对象重复创建导致内存浪费”的问题。

2. 适用场景
    - 棋子颜色对象、字符字形对象、地图砖块对象。
    - 海量对象中大部分状态相同，只有少量外部状态不同。

3. 角色分工/结构说明
    - `ChessPiece`：享元对象，保存可共享的内部状态。
    - 外部状态如坐标，由调用方传入。
    - `ChessPieceFactory`：缓存和复用享元对象。

4. Java 实际范例
    ```java
    import java.util.HashMap;
    import java.util.Map;

    class ChessPiece {
        private final String color;

        public ChessPiece(String color) {
            this.color = color;
        }

        public void draw(int x, int y) {
            System.out.println("绘制 " + color + " 棋子，坐标: (" + x + ", " + y + ")");
        }
    }

    class ChessPieceFactory {
        private static final Map<String, ChessPiece> CACHE = new HashMap<>();

        public static ChessPiece get(String color) {
            return CACHE.computeIfAbsent(color, ChessPiece::new);
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `Integer.valueOf()`：对一定范围内的整数做缓存复用。
    - 字符串常量池：相同字面量会被共享，是享元思想最经典的体现之一。

6. 优缺点与注意事项
    - 优点：显著减少重复对象，节约内存。
    - 缺点：需要区分内部状态和外部状态，设计复杂度提升。
    - 注意：不要为了“看起来高级”硬上享元，只有对象量大到足够造成压力时才值得。
    - 享元强调“共享复用”，和单例“全局唯一”不是一回事。

## 代理模式（结构型）
1. 核心思想
    - 为某个对象提供一个代理对象，以控制对原对象的访问。
    - 它解决的是“访问前后要加控制逻辑，但不想把这些逻辑塞进业务类”的问题。

2. 适用场景
    - 远程服务调用前做鉴权、日志、缓存、重试。
    - 懒加载、权限控制、事务控制、AOP 增强。

3. 角色分工/结构说明
    - `OrderService`：抽象主题。
    - `RemoteOrderService`：真实主题。
    - `OrderServiceProxy`：代理，控制访问流程。

4. Java 实际范例
    ```java
    interface OrderService {
        void submit(String orderNo);
    }

    class RemoteOrderService implements OrderService {
        public void submit(String orderNo) {
            System.out.println("远程订单服务提交: " + orderNo);
        }
    }

    class OrderServiceProxy implements OrderService {
        private final OrderService target;

        public OrderServiceProxy(OrderService target) {
            this.target = target;
        }

        public void submit(String orderNo) {
            System.out.println("鉴权通过");
            System.out.println("记录调用日志");
            target.submit(orderNo);
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `java.lang.reflect.Proxy`：JDK 动态代理的核心入口。
    - Spring AOP：事务、日志、权限等横切逻辑常通过代理织入。

6. 优缺点与注意事项
    - 优点：访问控制清晰，方便做横切增强。
    - 缺点：代理层过多时，调用链不直观。
    - 注意：代理并不一定改变业务功能，很多时候它只是“在前后做控制”。
    - 和装饰的区别：代理更偏“控制访问”，装饰更偏“增强职责”。

## 责任链模式（行为型）
1. 核心思想
    - 让多个处理者依次处理同一个请求，请求沿链路传递，直到被某个处理者接住。
    - 它解决的是“多个判断分支硬编码在一起，流程又容易变”的问题。

2. 适用场景
    - 请假审批、风控校验、参数校验、网关过滤。
    - 一个请求可能被多个节点处理，且链路顺序可配置。

3. 角色分工/结构说明
    - `LeaveHandler`：抽象处理者。
    - `ManagerHandler`、`DirectorHandler`、`CeoHandler`：具体处理者。
    - 每个节点决定：
        - 自己处理
        - 传给下一个节点

4. Java 实际范例
    ```java
    abstract class LeaveHandler {
        protected LeaveHandler next;

        public LeaveHandler setNext(LeaveHandler next) {
            this.next = next;
            return next;
        }

        public abstract void handle(int days);
    }

    class ManagerHandler extends LeaveHandler {
        public void handle(int days) {
            if (days <= 2) {
                System.out.println("经理审批通过");
            } else if (next != null) {
                next.handle(days);
            }
        }
    }

    class DirectorHandler extends LeaveHandler {
        public void handle(int days) {
            if (days <= 5) {
                System.out.println("总监审批通过");
            } else if (next != null) {
                next.handle(days);
            }
        }
    }

    class CeoHandler extends LeaveHandler {
        public void handle(int days) {
            System.out.println("CEO 审批通过，天数: " + days);
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - Servlet `Filter` 链：请求在多个过滤器中层层流转，非常典型。
    - Spring Security 过滤链、责任校验链、规则引擎链式处理，也经常这样设计。

6. 优缺点与注意事项
    - 优点：请求发送者和接收者解耦，链路可插拔。
    - 缺点：链太长时不容易追踪最终由谁处理。
    - 注意：责任链强调“谁来处理不提前写死”，顺序非常关键。
    - 和命令模式的区别：责任链是“沿链寻找处理者”，命令模式是“把请求封装成对象等待执行”。

## 命令模式（行为型）
1. 核心思想
    - 将请求封装成对象，从而把请求发送者与接收者解耦。
    - 它解决的是“调用者不想关心具体执行细节，还要支持排队、撤销、记录”的问题。

2. 适用场景
    - 遥控器按钮、任务队列、操作日志、批处理。
    - 把请求缓存起来，稍后执行，甚至支持重放。

3. 角色分工/结构说明
    - `Command`：命令接口。
    - `StartServerCommand`：具体命令。
    - `Server`：接收者，真正干活的对象。
    - `Invoker`：调用者，触发命令执行。

4. Java 实际范例
    ```java
    interface Command {
        void execute();
    }

    class Server {
        public void start() {
            System.out.println("启动服务器");
        }

        public void stop() {
            System.out.println("停止服务器");
        }
    }

    class StartServerCommand implements Command {
        private final Server server;

        public StartServerCommand(Server server) {
            this.server = server;
        }

        public void execute() {
            server.start();
        }
    }

    class Invoker {
        public void submit(Command command) {
            command.execute();
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `Runnable`：把要执行的逻辑封装成对象，本质上就很像命令。
    - `ExecutorService`：接收任务命令，再统一调度执行。

6. 优缺点与注意事项
    - 优点：支持队列、日志、撤销、重试等扩展。
    - 缺点：命令类增多，简单场景会显得重。
    - 注意：命令模式关注的是“请求对象化”。
    - 如果只是简单回调，用 Lambda 就能做；但一旦要排队、持久化、记录历史，命令模式优势就出来了。

## 解释器模式（行为型）
1. 核心思想
    - 为某种语言或规则定义语法表示，并提供解释器来解释这些表达式。
    - 它解决的是“规则需要以表达式形式组合、并可重复解析”的问题。

2. 适用场景
    - 简单规则表达式、权限表达式、过滤表达式。
    - 规则语法稳定但组合较多的小型 DSL。

3. 角色分工/结构说明
    - `Expression`：抽象表达式。
    - `TerminalExpression`：终结符表达式，负责最小判断单元。
    - `AndExpression`、`OrExpression`：非终结符表达式，负责组合规则。

4. Java 实际范例
    ```java
    import java.util.Map;

    interface Expression {
        boolean interpret(Map<String, String> context);
    }

    class EqualExpression implements Expression {
        private final String key;
        private final String expectedValue;

        public EqualExpression(String key, String expectedValue) {
            this.key = key;
            this.expectedValue = expectedValue;
        }

        public boolean interpret(Map<String, String> context) {
            return expectedValue.equals(context.get(key));
        }
    }

    class AndExpression implements Expression {
        private final Expression left;
        private final Expression right;

        public AndExpression(Expression left, Expression right) {
            this.left = left;
            this.right = right;
        }

        public boolean interpret(Map<String, String> context) {
            return left.interpret(context) && right.interpret(context);
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `Pattern` 正则表达式可以理解为 JDK 中很典型的“规则解析并执行”的落地。
    - Spring EL、SQL 解析器、规则引擎 DSL 也都有解释器思想，只是复杂度高得多。

6. 优缺点与注意事项
    - 优点：规则可组合、可扩展、可读性好。
    - 缺点：语法一复杂，类会迅速膨胀。
    - 注意：解释器模式适合小语法，不适合无限膨胀的复杂语言。
    - 当规则已经复杂到需要词法、语法、优化阶段时，通常要考虑专门解析器框架，而不是手写解释器。

## 迭代器模式（行为型）
1. 核心思想
    - 提供一种顺序访问聚合对象元素的方法，而不暴露其内部表示。
    - 它解决的是“不同集合结构遍历方式不同，客户端代码耦合内部存储”的问题。

2. 适用场景
    - 统一遍历班级集合、订单集合、自定义容器。
    - 容器内部可能从数组换成链表，但遍历方不应受影响。

3. 角色分工/结构说明
    - `Aggregate`：聚合对象。
    - `Iterator`：迭代器接口。
    - 具体容器负责返回匹配自身结构的迭代器。

4. Java 实际范例
    ```java
    import java.util.Iterator;
    import java.util.List;

    class Student {
        private final String name;

        public Student(String name) {
            this.name = name;
        }

        public String getName() {
            return name;
        }
    }

    class Classroom implements Iterable<Student> {
        private final List<Student> students;

        public Classroom(List<Student> students) {
            this.students = students;
        }

        public Iterator<Student> iterator() {
            return students.iterator();
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `Iterator`：模式名几乎直接写在 JDK 里。
    - `Spliterator`：为并行遍历、拆分遍历提供更强能力，可以看作迭代器思想的扩展版本。

6. 优缺点与注意事项
    - 优点：统一遍历接口，隐藏内部结构。
    - 缺点：对某些极简容器来说，单独抽象会显得多余。
    - 注意：迭代器模式的价值在于“遍历和存储解耦”。
    - 不要一边迭代一边随意修改底层集合，否则容易触发并发修改问题。

## 中介者模式（行为型）
1. 核心思想
    - 用一个中介对象封装一组对象之间的交互，避免对象彼此直接大量引用。
    - 它解决的是“对象网状依赖太复杂，谁和谁都耦合”的问题。

2. 适用场景
    - 群聊转发中心、表单组件联动、复杂界面控件协作。
    - 多个对象之间有复杂交互规则，但希望把规则集中管理。

3. 角色分工/结构说明
    - `Mediator`：中介者接口。
    - `ChatRoom`：具体中介者。
    - `User`：同事类，只和中介者交互，不直接互相调用。

4. Java 实际范例
    ```java
    interface Mediator {
        void relay(String from, String message);
    }

    class ChatRoom implements Mediator {
        public void relay(String from, String message) {
            System.out.println("聊天室转发 [" + from + "]: " + message);
        }
    }

    class User {
        private final String name;
        private final Mediator mediator;

        public User(String name, Mediator mediator) {
            this.name = name;
            this.mediator = mediator;
        }

        public void send(String message) {
            mediator.relay(name, message);
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - GUI 控制器常作为多个控件之间的协调中枢。
    - `DispatcherServlet` 可以理解为一种“请求协调中心”思想的体现：请求先到它，再由它分发给不同组件处理。

6. 优缺点与注意事项
    - 优点：减少对象之间的直接耦合，交互规则集中。
    - 缺点：中介者容易膨胀成“总控中心”。
    - 注意：当交互规则非常多时，中介者要拆层，不然会变成巨型 if-else 调度器。
    - 和外观的区别：外观服务于“客户端调用简化”，中介者服务于“内部对象协作解耦”。

## 备忘录模式（行为型）
1. 核心思想
    - 在不破坏封装的前提下，捕获对象内部状态，并在之后恢复到该状态。
    - 它解决的是“对象要支持撤销/回滚，但不想把内部细节暴露给外部”的问题。

2. 适用场景
    - 文本编辑器撤销、表单草稿恢复、流程回滚。
    - 某个对象状态需要阶段性快照。

3. 角色分工/结构说明
    - `Editor`：原发器，负责创建和恢复快照。
    - `EditorMemento`：备忘录，保存状态。
    - `History`：负责人，管理快照历史。

4. Java 实际范例
    ```java
    import java.util.Stack;

    class Editor {
        private String text = "";

        public void type(String value) {
            text += value;
        }

        public String getText() {
            return text;
        }

        public EditorMemento save() {
            return new EditorMemento(text);
        }

        public void restore(EditorMemento memento) {
            this.text = memento.getText();
        }
    }

    class EditorMemento {
        private final String text;

        public EditorMemento(String text) {
            this.text = text;
        }

        public String getText() {
            return text;
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `UndoManager`：Swing 中的撤销管理器，很适合联想到备忘录模式。
    - 数据库事务日志、工作流回滚也常涉及“历史状态恢复”的思路。

6. 优缺点与注意事项
    - 优点：撤销逻辑清晰，状态恢复自然。
    - 缺点：快照多时非常占内存。
    - 注意：大对象做备忘录时，应考虑增量快照、压缩存储，而不是每次全量复制。
    - 如果只是简单字段回退，未必需要完整备忘录结构。

## 观察者模式（行为型）
1. 核心思想
    - 定义对象间一对多依赖，当主题状态变化时，自动通知所有观察者。
    - 它解决的是“一个状态变化后，多个模块都要响应，但又不想硬编码逐个调用”的问题。

2. 适用场景
    - 库存变化通知订阅者。
    - 订单状态变化触发积分、短信、日志、审计等多个后续动作。

3. 角色分工/结构说明
    - `Subject`：主题，负责注册、移除、通知观察者。
    - `Observer`：观察者接口。
    - 主题只知道“有人订阅”，不关心具体后续动作细节。

4. Java 实际范例
    ```java
    import java.util.ArrayList;
    import java.util.List;

    interface Observer {
        void update(String sku, int stock);
    }

    class StockSubject {
        private final List<Observer> observers = new ArrayList<>();

        public void addObserver(Observer observer) {
            observers.add(observer);
        }

        public void change(String sku, int stock) {
            for (Observer observer : observers) {
                observer.update(sku, stock);
            }
        }
    }

    class SmsObserver implements Observer {
        public void update(String sku, int stock) {
            System.out.println("短信通知: " + sku + " 库存变为 " + stock);
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `PropertyChangeListener`：JDK 中经典的观察者接口。
    - Spring 事件机制：发布事件后，多个监听器可独立响应。

6. 优缺点与注意事项
    - 优点：发布方和订阅方解耦，扩展方便。
    - 缺点：观察者过多时，事件链路难追踪。
    - 注意：同步通知可能拖慢主流程，耗时监听器要考虑异步化。
    - 观察者适合领域内联动；如果跨系统、跨进程，就更像消息队列或事件总线问题了。

## 状态模式（行为型）
1. 核心思想
    - 允许对象在内部状态改变时改变它的行为，看起来像切换了类。
    - 它解决的是“一个对象状态分支越来越多，巨型 `if-else` 难维护”的问题。

2. 适用场景
    - 订单待支付、已支付、已发货、已完成、已取消。
    - 工单流转、审批状态机、任务生命周期管理。

3. 角色分工/结构说明
    - `OrderState`：状态接口。
    - `PendingState`、`PaidState`：具体状态。
    - `OrderContext`：上下文，持有当前状态，并委托状态对象处理行为。

4. Java 实际范例
    ```java
    interface OrderState {
        void next(OrderContext context);
        String name();
    }

    class PendingState implements OrderState {
        public void next(OrderContext context) {
            context.setState(new PaidState());
        }

        public String name() {
            return "待支付";
        }
    }

    class PaidState implements OrderState {
        public void next(OrderContext context) {
            context.setState(new ShippedState());
        }

        public String name() {
            return "已支付";
        }
    }

    class ShippedState implements OrderState {
        public void next(OrderContext context) {
            System.out.println("已发货，后续进入完成态");
        }

        public String name() {
            return "已发货";
        }
    }

    class OrderContext {
        private OrderState state = new PendingState();

        public void setState(OrderState state) {
            this.state = state;
        }

        public void process() {
            System.out.println("当前状态: " + state.name());
            state.next(this);
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `Thread.State`：线程不同状态对应不同可观察行为，是理解状态模式的很好入口。
    - 工作流引擎、状态机框架本质上都在强化状态模式。

6. 优缺点与注意事项
    - 优点：状态切换逻辑清晰，避免大量条件判断。
    - 缺点：状态类数量会增加。
    - 注意：状态模式中的策略不是由客户端主动切换，而是由上下文根据状态流转自然切换。
    - 和策略的区别：策略强调“可替换算法”，状态强调“对象自身状态变化引发行为变化”。

## 策略模式（行为型）
1. 核心思想
    - 定义一组算法，把它们封装起来，并且让它们可以互换。
    - 它解决的是“同一件事有多种实现算法，但不想在业务里堆满条件分支”的问题。

2. 适用场景
    - 支付方式切换、折扣算法切换、排序规则切换。
    - 同一个入口下存在多套可替代处理逻辑。

3. 角色分工/结构说明
    - `DiscountStrategy`：抽象策略。
    - `VipDiscountStrategy`、`CouponDiscountStrategy`：具体策略。
    - `PriceContext`：上下文，持有策略并调用。

4. Java 实际范例
    ```java
    interface DiscountStrategy {
        int calc(int originalPrice);
    }

    class VipDiscountStrategy implements DiscountStrategy {
        public int calc(int originalPrice) {
            return (int) (originalPrice * 0.8);
        }
    }

    class CouponDiscountStrategy implements DiscountStrategy {
        public int calc(int originalPrice) {
            return Math.max(originalPrice - 30, 0);
        }
    }

    class PriceContext {
        private final DiscountStrategy strategy;

        public PriceContext(DiscountStrategy strategy) {
            this.strategy = strategy;
        }

        public int quote(int originalPrice) {
            return strategy.calc(originalPrice);
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `Comparator`：排序算法策略的经典统一抽象。
    - 支付渠道、路由策略、限流策略、重试策略，在项目中到处都是策略模式。

6. 优缺点与注意事项
    - 优点：算法切换方便，符合开闭原则。
    - 缺点：客户端需要理解并选择策略。
    - 注意：策略模式强调“同一目标，不同算法”。
    - 和状态模式的区别：策略通常由客户端或配置主动选择；状态通常由对象内部流转决定。

## 模板方法模式（行为型）
1. 核心思想
    - 在父类中定义算法骨架，将某些步骤延迟到子类实现。
    - 它解决的是“多个流程整体相同，但个别步骤不同”的问题。

2. 适用场景
    - 统一数据导入流程、统一任务执行流程、统一校验-转换-保存流程。
    - 框架层规定主流程，业务方只实现差异化步骤。

3. 角色分工/结构说明
    - 抽象类定义模板方法和流程骨架。
    - 子类只实现差异步骤。
    - 可选钩子方法控制局部扩展点。

4. Java 实际范例
    ```java
    abstract class DataImporter {
        public final void execute() {
            read();
            parse();
            validate();
            save();
        }

        protected abstract void read();
        protected abstract void parse();

        protected void validate() {
            System.out.println("执行通用校验");
        }

        protected void save() {
            System.out.println("保存到数据库");
        }
    }

    class CsvImporter extends DataImporter {
        protected void read() {
            System.out.println("读取 CSV 文件");
        }

        protected void parse() {
            System.out.println("解析 CSV 行数据");
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `HttpServlet.service()`：先统一分发，再调用 `doGet()`、`doPost()`，是模板方法的经典体现。
    - `AbstractList`：提供默认流程和骨架，让子类关注关键实现点。

6. 优缺点与注意事项
    - 优点：复用公共流程，统一主干逻辑。
    - 缺点：继承耦合明显，变化维度过多时不够灵活。
    - 注意：模板方法强调“流程骨架固定”，策略模式强调“算法可以整体替换”。
    - 如果流程本身也频繁变化，单靠模板方法就容易僵化。

## 访问者模式（行为型）
1. 核心思想
    - 将作用于对象结构中各元素的操作抽离出来，使得在不改变元素类的前提下增加新操作。
    - 它解决的是“对象结构比较稳定，但作用在其上的操作越来越多”的问题。

2. 适用场景
    - 报表系统要对同一批业务对象做不同统计。
    - 文件树遍历时，对节点做搜索、统计、清理、导出等多种操作。

3. 角色分工/结构说明
    - `Element`：元素接口，定义 `accept()`。
    - `Employee`、`Department`：具体元素。
    - `Visitor`：访问者接口。
    - `SalaryReportVisitor`、`PerformanceVisitor`：具体访问者。

4. Java 实际范例
    ```java
    interface Visitor {
        void visit(Employee employee);
        void visit(Department department);
    }

    interface Element {
        void accept(Visitor visitor);
    }

    class Employee implements Element {
        private final String name;
        private final int salary;

        public Employee(String name, int salary) {
            this.name = name;
            this.salary = salary;
        }

        public String getName() {
            return name;
        }

        public int getSalary() {
            return salary;
        }

        public void accept(Visitor visitor) {
            visitor.visit(this);
        }
    }

    class Department implements Element {
        private final String name;

        public Department(String name) {
            this.name = name;
        }

        public String getName() {
            return name;
        }

        public void accept(Visitor visitor) {
            visitor.visit(this);
        }
    }
    ```

5. 在常用类/框架中的延伸应用
    - `FileVisitor`：文件树遍历中非常典型的访问者模式落地。
    - 编译器 AST 遍历、代码检查器、报表统计器里也经常出现访问者结构。

6. 优缺点与注意事项
    - 优点：新增操作方便，不必反复修改元素类。
    - 缺点：元素结构一旦变化，所有访问者都可能跟着改。
    - 注意：访问者适合“元素稳定、操作易变”的场景。
    - 如果对象结构本身经常变，访问者模式会非常痛苦。
