# Redis

## 简介

### 为什么需要Redis

1. Redis可以满足企业需求：
    - 高并发
    - 高可用
    - 高性能
    - 海量用户

2. 关系型数据库（如MySQL）的问题
    - MySQL本质是三层架构，数据的存储本质上还是文件，对数据库的读写本质上还是对文件的读写，即磁盘IO
    - 扩展瓶颈：数据关系复杂，扩展性差，不便于大规模集群

3. Redis 的优势
    - 内存存储-降低磁盘 IO 次数
    - 不存储关系，仅存储数据-数据间关系，越简单越好

### Redis 简介
1. 一句话: Redis (Remote DIctionary Server) 是用 C 语言开发的一个开源的高性能键值对（key-value）数据库
2. 特征
    - 数据间没有必然的关联关系
    - 高性能。官方提供测试数据，50 个并发执行 100000 个请求,读的速度是 110000 次/s,写的速度是 81000 次/s
    - 多种数据结构支持
        - 字符串类型 `string`
        - 列表类型 `list`
        - 散列类型 `hash`
        - 集合类型 `set`
        - 有序集合类型 `sorted_set`
    - 持久化支持。可以进行数据灾难恢复

3. 应用场景
    - 为热点数据加速查询，如热点商品、热点新闻、热点资讯、推广类等高访问量信息等
    - 任务队列，如秒杀、抢购、购票排队等
    - 即时信息查询，如排行榜、各类网站访问统计
    - 时效性信息控制，如验证码控制、投票控制等
    - 分布式数据共享，如分布式集群架构中的 session 分离
    - 消息队列
    - 分布式锁


###  NoSQL 数据库
1.  一句话: 即 Not-Only SQL（ 泛指非关系型的数据库），作为关系型数据库的补充
2.  作用：应对在海量用户和海量数据的情况下，带来的数据处理问题
3. NoSQL 的特点
    -  可扩容，可伸缩
    -  大数据量下高性能
    -  灵活的数据模型
    -  高可用
4.  常见 Nosql 数据库
    - Redis
    - memcache
    - HBase
    - MongoDB

## 安装

### Docker Desktop

1. 安装
    https://yeasy.gitbook.io/docker_practice/di-yi-bu-fen-ru-men-pian/03_install/3.8_windows
    https://docs.docker.com/desktop/setup/install/windows-install/

2. 汉化
    https://github.com/asxez/DockerDesktop-CN
    - 查询版本：这里看到版本是4.87.0
        ```powershell
        (base) PS C:\Users\Lenovo> docker version
        Client:
         Version:           29.7.2
         API version:       1.55
         Go version:        go1.26.5
         Git commit:        a7dcaa6
         Built:             Wed Aug  5 18:31:33 2026
         OS/Arch:           windows/amd64
         Context:           desktop-linux
        
        Server: Docker Desktop 4.87.0 (236836)
         Engine:
          Version:          29.7.2
          API version:      1.55 (minimum version 1.40)
          Go version:       go1.26.5
          Git commit:       6a43e3d
          Built:            Wed Aug  5 18:28:36 2026
          OS/Arch:          linux/amd64
          Experimental:     false
         containerd:
          Version:          v2.2.5
          GitCommit:        e53c7c1516c3b2bff98eb76f1f4117477e6f4e66
         runc:
          Version:          1.3.6
          GitCommit:        v1.3.6-0-g491b69ba
         docker-init:
          Version:          0.19.0
          GitCommit:        de40ad0
        (base) PS C:\Users\Lenovo>
        ```

    - 下载对应版本的`app-Windows-x86.zip`，并替换目标目录的文件，具体看原文档。
    - 似乎项目提供了汉化安装包，不过我没有尝试。

### WSL

#### 简介

1. WSL 全称为 Windows Subsystem for Linux，即“适用于 Linux 的 Windows 子系统”。

1. 它允许在 Windows 中运行 Ubuntu 等 Linux 发行版。Docker Desktop 可以使用 WSL 2 作为后端，从而在 Windows 上运行 Linux 容器。

1. 安装 Docker Desktop 后，通常会看到两个 WSL 发行版：

    ```text
    Ubuntu           # 用户使用的 Linux 系统
    docker-desktop    # Docker Desktop 内部使用，不应手动操作
    ```

#### 具体步骤

1. 检查硬件虚拟化

    打开`任务管理器 → 性能 → CPU`确认右下角显示：`虚拟化：已启用`

    如果显示`已禁用`，需要进入 BIOS，启用 Intel VT-x、AMD-V 或 SVM Mode。

2. 打开PowerShell（管理员）

2. 输入如下命令
    - 若先安装的Docker DeskTop，启动时会自动跳出命令行询问安装WSL

    ```powershell
    wsl --install
    ```

    该命令会：

    1. 启用 WSL 和虚拟机平台功能。
    2. 安装 WSL 2 内核。
    3. 默认安装 Ubuntu。
    4. 首次启动 Ubuntu 时要求创建 Linux 用户名和密码。

    输入 Linux 密码时，终端不会显示星号或其他字符，这是正常现象。

    安装完成后更新 WSL：

    ```powershell
    wsl --update
    ```

    设置新发行版默认使用 WSL 2：

    ```powershell
    wsl --set-default-version 2
    ```

    如果系统提示重启，则重启 Windows。

    > 如果先安装 Docker Desktop，Docker Desktop 也可能提示安装或更新 WSL。

4. 检查安装结果

    ```powershell
    wsl --version
    ```

    查看已经安装的发行版：

    ```powershell
    wsl --list --verbose
    ```

    示例：

    ```text
    NAME               STATE      VERSION
    Ubuntu             Stopped    2
    docker-desktop     Stopped    2
    ```

    其中 `VERSION` 应为 `2`。

5. 设置默认发行版

    如果星号出现在 `docker-desktop` 前面：

    ```text
    * docker-desktop
    Ubuntu
    ```

    需要将 Ubuntu 设置为默认发行版：

    ```powershell
    wsl --set-default Ubuntu
    ```

    以后执行下面的命令就会进入 Ubuntu：

    ```powershell
    wsl
    ```

    也可以明确指定：

    ```powershell
    wsl -d Ubuntu
    ```

    退出 Ubuntu：

    ```bash
    exit
    ```

#### WSL 常用命令

```powershell
# 查看发行版及运行状态
wsl --list --verbose

# 启动并进入 Ubuntu
wsl -d Ubuntu

# 停止 Ubuntu
wsl --terminate Ubuntu

# 停止所有 WSL 发行版，包括 docker-desktop
wsl --shutdown

# 更新 WSL
wsl --update
```

关闭终端后，如果没有程序继续使用 Ubuntu，Ubuntu 通常会在短暂空闲后自动停止。

WSL 2 的内部 IP 地址可能在重启后发生变化，因此应用程序不应固定使用 WSL 内部 IP。Windows 访问已经映射端口的服务时，通常使用：

```text
localhost:端口
```

### Redis（Windows Docker）

1. 启动PowerShell（管理员）

2. 输入如下命令启动Redis
    ```powershell
    docker run  -d 
                --name redis 
                --restart unless-stopped 
                -p 127.0.0.1:6379:6379 
                -v redis-data:/data 
                redis:7.2-alpine 
                redis-server --appendonly yes

    docker run -d --name redis --restart unless-stopped -p 127.0.0.1:6379:6379 -v redis-data:/data redis:7.2-alpine redis-server --appendonly yes
    ```
    | 参数 | 作用 |
    |---|---|
    | `-d` | 在后台运行 |
    | `--name redis` | 将容器命名为 `redis` |
    | `--restart unless-stopped` | Docker 启动后自动恢复容器 |
    | `-p 127.0.0.1:6379:6379` | 将 Redis 映射到本机 `6379` 端口 |
    | `-v redis-data:/data` | 使用数据卷持久化数据 |
    | `redis:7.2-alpine` | 使用 Redis 7.2 Alpine 镜像 |
    | `--appendonly yes` | 开启 AOF 持久化 |

3. 查看容器状态

    ```powershell
    (base) PS C:\Users\Lenovo> docker ps
    CONTAINER ID   IMAGE              COMMAND                  CREATED         STATUS         PORTS                      NAMES
    d6e9e675e207   redis:7.2-alpine   "docker-entrypoint.s…"   8 seconds ago   Up 8 seconds   127.0.0.1:6379->6379/tcp   redis
    (base) PS C:\Users\Lenovo>
    ```

4. 测试Redis状态
    - 预期返回`PONG`
    ```powershell
    (base) PS C:\Users\Lenovo> docker exec redis redis-cli ping
    PONG
    ```

4. 进入命令行
    ```powershell
    docker exec -it redis redis-cli
    ```

4. 常用Docker命令
    ```powershell
    # 启动 Redis
    docker start redis

    # 停止 Redis
    docker stop redis

    # 重启 Redis
    docker restart redis

    # 查看日志
    docker logs redis

    # 持续查看日志
    docker logs -f redis

    # 查看容器状态
    docker ps

    # 进入 redis-cli
    docker exec -it redis redis-cli
    ```

7. java相关配置
    - springboot2
        ```yaml
        spring:
          redis:
            host: localhost
            port: 6379
        ```

    - springboot3
        ```yaml
        spring:
          data:
            redis:
              host: localhost
              port: 6379
        ```



### Redis（WSL）

1. 通过PowerShell进入Ubuntu
    ```powershell
    wsl -d Ubuntu
    ```

2. 安装必要工具
    ```bash
    # 安装工具
    sudo apt-get update
    sudo apt-get install -y lsb-release curl gpg

    # 添加官方秘钥
    curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg
    sudo chmod 644 /usr/share/keyrings/redis-archive-keyring.gpg

    # 添加官方软件源
    echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list
    ```

2. 安装
    ```bash
    sudo apt-get install -y redis

    # 检查版本
    redis-server --version
    redis-cli --version
    ```

2. 启动
    ```bash
    sudo systemctl enable redis-server
    sudo systemctl start redis-server

    # 查看状态
    systemctl status redis-server --no-pager
    ```

2. 测试Redis
    - 预期返回：`PONG`
    ```bash
    redis-cli ping  
    ```

2. 常用命令
    ```bash
    # 启动 Redis
    sudo systemctl start redis-server

    # 停止 Redis
    sudo systemctl stop redis-server

    # 重启 Redis
    sudo systemctl restart redis-server

    # 查看状态
    systemctl status redis-server --no-pager

    # 查看日志
    sudo journalctl -u redis-server -n 100 --no-pager
    ```

### 使用Docker创建镜像与容器



## Redis 命令

### 简介

1. 文档
    - 中文：https://redis.com.cn/commands.html
    - 官方：https://redis.io/docs/latest/commands/


2. Redis 命令十分丰富，包括的命令组有 `Cluster`、`Connection`、`Geo`、`Hashes`、`HyperLogLog`、`Keys`、`Lists`、`Pub/Sub`、`Scripting`、`Server`、`Sets`、`Sorted Sets`、`Strings`、`Transactions` 一共 14个 redis 命令组两百多个 redis 命令


### 基础操作

1. `set key value`: 设置 key，value 数据

1. `get key`: 根据 key 查询对应的 value
    - 如果不存在，返回空（nil）
    - value的引号不强制，但如果value中含有空格，则必须用引号

    ```powershell
    (base) PS C:\Users\Lenovo> docker exec -it redis redis-cli
    127.0.0.1:6379> set lcq aha
    OK
    127.0.0.1:6379> get lcq
    "aha"
    127.0.0.1:6379> get aaa
    (nil)   
    127.0.0.1:6379> set lcqa aha bhb
    (error) ERR syntax error

    ```
1. `clear`: 清除屏幕中的信息

1. `quit/exit`: 退出客户端 【说明: Redis 服务没有结束】
1. `help 命令名称`: 获取命令帮助文档，获取组中所有命令信息名称
    ```powershell
    127.0.0.1:6379> help get

    GET key
    summary: Returns the string value of a key.
    since: 1.0.0
    group: string
    ```

### 对Key键操作

1. `keys *` ： 查看当前库所有 key (匹配：keys *1)
    ```powershell
    127.0.0.1:6379> keys *
    1) "k1"
    2) "lcq"
    127.0.0.1:6379> keys k*
    1) "k1"
    127.0.0.1:6379>
    ```
2. `exists key`：判断某个 key 是否存在
    - 返回`1`表示key存在，返回`0`表示不存在
    ```powershell
    127.0.0.1:6379> exists k1
    (integer) 1
    127.0.0.1:6379> exists k2
    (integer) 0
    127.0.0.1:6379>
    ```

3. `type key` ：查看你的 key 是什么类型
    ```powershell
    127.0.0.1:6379> type k1
    string
    ```

4. `del key` ： 删除指定的 key 数据
    ```powershell
    127.0.0.1:6379> set k8 666
    OK
    127.0.0.1:6379> type k8
    string
    127.0.0.1:6379> keys *
    1) "k1"
    2) "lcq"
    3) "k8"
    127.0.0.1:6379> del k8
    (integer) 1
    127.0.0.1:6379> keys *
    1) "k1"
    2) "lcq"
    127.0.0.1:6379>
    ```


5. `unlink key` 根据 value 选择非阻塞删除【仅将 keys 从 keyspace 元数据中删除，真正的删除会在后续异步操作】
    - https://redis.com.cn/commands/unlink.html
    - 返回值：删除的key的数量
    ```powershell
    127.0.0.1:6379> set k2 2
    OK
    127.0.0.1:6379> set k3 3
    OK
    127.0.0.1:6379> set k4 4
    OK
    127.0.0.1:6379> unlink k2 k3
    (integer) 2
    127.0.0.1:6379>
    ```
    >🎯 如何根据业务场景选择
    >
    >核心原则是：**如果不能百分百确定键的大小，优先选择 `UNLINK`**。
    >
    >**优先使用 `UNLINK` 的场景：**
    >
    >*   **删除大键（Big Key）**：这是 `UNLINK` 最典型的场景。当你的键是包含数百万字段的 Hash、超大 Set 或长 List 时，`UNLINK` 能避免主线程长时间阻塞。
    >*   **批量删除大量键**：例如清理过期会话或临时数据时。使用 `SCAN` 命令搭配 `UNLINK` 能安全、非阻塞地完成清理。
    >*   **对延迟敏感的高并发系统**：任何微小的主线程阻塞都可能被放大，使用 `UNLINK` 能提供更稳定的性能。
    >
    >**考虑使用 `DEL` 的有限场景：**
    >
    >*   **键的大小和数量完全可控**：明确知道要删除的是小键（如几个字节的 String），且数量极少。
    >*   **需要在事务（`MULTI`/`EXEC`）或 Pipeline 中立即删除**：在**同一个事务或管道**中，先添加再删除一个大值，`DEL` 可能是合适的，因为阻塞只发生在命令执行期间。
    >*   **Redis 版本低于 4.0**：这是硬性限制，此时只能使用 `DEL`。


6. `expire key 10`：为给定的 key 设置过期时间
    - 默认的超时时间为永久

7. `ttl key`： 查看还有多少秒过期
    - `-1` 表示永不过期，`-2` 表示已过期
    ```powershell
    127.0.0.1:6379> unlink k2 k3
    (integer) 2
    127.0.0.1:6379> set k999 100
    OK
    127.0.0.1:6379> ttl k999
    (integer) -1
    127.0.0.1:6379> EXPIRE k999 100
    (integer) 1
    127.0.0.1:6379> ttl k999
    (integer) 96
    127.0.0.1:6379> ttl k999
    (integer) 94
    127.0.0.1:6379> EXPIRE k999 5
    (integer) 1
    127.0.0.1:6379> ttl k999
    (integer) 4
    127.0.0.1:6379> ttl k999
    (integer) 2
    127.0.0.1:6379> ttl k999
    (integer) -2
    127.0.0.1:6379>
    ```

### 对数据库操作

1. 切换数据库
    - Redis默认拥有16个库，编号为0-15，默认使用0号库
        ```powershell
        127.0.0.1:6379> select 1
        OK
        127.0.0.1:6379[1]> set k1 abc
        OK
        127.0.0.1:6379[1]> get k1
        "abc"
        127.0.0.1:6379[1]> select 0
        OK
        127.0.0.1:6379> get k1
        "aaa"
        127.0.0.1:6379>
        ```

2. `dbsize`：查看当前库数量

    ```powershell

    ```

2. `flushdb`：清空当前库

    ```powershell

    ```

2. `flushall`：清空全部库【谨慎使用】

    ```powershell

    ```

## 五大数据类型

### 简介

1. 文档同上一节
    https://redis.com.cn/commands.html

2. Redis的数据存储格式
    redis自身是一个Map，通过k-v形式存储

3. 5种常用数据类型
    - string：字符串
    - list
    - hash
    - set
    - Zset

### string

#### 简介

1. String 是 Redis 最基本的类型，一个 key 对应一个 value。
2. String 类型是二进制安全的, Redis 的 string 可以包含任何数据。比如 jpg 图片或者序列化的对象。
3. String 类型是 Redis 基本的数据类型，一个 Redis 中字符串 value 最多可以是 512M

#### 常用指令

1. `set <key><value>`：添加键值对
2. `get <key>`：查询对应键值
3. `append <key><value>`：将给定的`<value>` 追加到原值的末尾
    - 返回值：追加后的字符串长度
    ```powershell
    127.0.0.1:6379> get k1
    "aaa"
    127.0.0.1:6379> append k1 666
    (integer) 6
    127.0.0.1:6379> get k1
    "aaa666"
    127.0.0.1:6379> strlen k1
    (integer) 6
    ```
4. `strlen <key>`：获得值的长度
5. `setnx <key><value>`：只有在 key 不存在时 设置 key 的值
    - 仅在 key 不存在时设置 key-value。若 key 存在则不执行任何操作。
    - 设置成功返回`1`，失败返回`0`
    ```powershell
    127.0.0.1:6379> keys k1
    1) "k1"
    127.0.0.1:6379> keys k2
    (empty array)
    127.0.0.1:6379> setnx k1 666
    (integer) 0
    127.0.0.1:6379> setnx k2 666
    (integer) 1
    ```

6. `incr <key>`： 将 key 中储存的数字值(字符串)增 1， 只能对数字值操作，如果为空，新增值为1
7. `decr <key>`： 将 key 中储存的数字值(字符串)减 1 ， 只能对数字值操作，如果为空，新增值为-1
8. `incrby / decrby <key> <步长>`：将 key 中储存的数字值增减。自定义步长

    ```powershell
    127.0.0.1:6379> get k1
    "aaa666"
    127.0.0.1:6379> setnx k2 666
    (integer) 1
    127.0.0.1:6379> incr k1
    (error) ERR value is not an integer or out of range
    127.0.0.1:6379> incr k2
    (integer) 667
    127.0.0.1:6379> incr k2 2
    (error) ERR wrong number of arguments for 'incr' command
    127.0.0.1:6379> incrby k2 2
    (integer) 669
    ```


9. `mset <key1><value1><key2><value2> .....`： 同时设置一个或多个 key-value 对
10. `mget <key1><key2><key3> .....`： 同时获取一个或多个 value
11. `msetnx <key1><value1><key2><value2> .....`： 同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在, 原子性，有一个失败则都失败
12. `getrange <key><起始位置><结束位置>`：
    - 获得值的范围，即字符串切片，类似 java 中的 substring
    - 两端为闭区间。
    - 与python类似，支持负数索引取末尾字符串
13. `setrange <key><起始位置><value>`： 用 `<value>` 覆写`<key>`所储存的字符串值，从<起始位置>开始(索引从 0 开始)。
14. `setex <key><过期时间><value>`： 设置键值的同时，设置过期时间，单位秒。
15. `getset <key><value>`： , 以新换旧，设置了新值同时获得旧值

#### 常用缩写

| 缩写 | 英文含义 | 中文含义 | 示例 |
|---|---|---|---|
| `SET` | Set | 设置 | `SET key value` |
| `GET` | Get | 获取 | `GET key` |
| `NX` | Not eXists | 不存在时才执行 | `SET key value NX` |
| `XX` | eXists | 存在时才执行 | `SET key value XX` |
| `EX` | EXpiration / EXpire | 秒级过期时间 | `SET key value EX 60` |
| `PX` | millisecond-Precision eXpiration | 毫秒级过期时间 | `SET key value PX 5000` |
| `AT` | At timestamp | 在指定时间点过期 | `EXAT`、`PXAT` |
| `M` | Multiple | 多个、批量 | `MGET`、`MSET` |
| `STR` | String | 字符串 | `STRLEN` |
| `LEN` | Length | 长度 | `STRLEN` |
| `INCR` | Increment | 增加 | `INCR` |
| `DECR` | Decrement | 减少 | `DECR` |
| `BY` | By | 按指定数值 | `INCRBY` |
| `FLOAT` | Floating-point number | 浮点数 | `INCRBYFLOAT` |
| `RANGE` | Range | 范围 | `GETRANGE` |
| `DEL` | Delete | 删除 | `GETDEL` |
| `BIT` | Bit | 二进制位 | `GETBIT` |
| `COUNT` | Count | 统计数量 | `BITCOUNT` |
| `TTL` | Time To Live | 剩余生存时间 | `TTL key` |
| `PERSIST` | Persist | 持久存在、取消过期 | `GETEX key PERSIST` |

#### String 常用指令（by codex）

> Redis 的 String 是二进制安全的，可以保存字符串、整数、浮点数、JSON 文本、图片等内容。
>
> 单个 String 值最大为 512 MB。涉及长度的命令通常返回**字节数**，不一定是字符数。

##### 1. SET：设置键值

```text
SET key value [NX | XX] [GET] [EX seconds | PX milliseconds | KEEPTTL]
```

```powershell
127.0.0.1:6379> SET name tom
OK
127.0.0.1:6379> GET name
"tom"
```

默认情况下：

- key 不存在：创建 key。
- key 已存在：覆盖旧值。
- 覆盖旧值时，原有过期时间会被清除。
- 返回 `OK` 表示设置成功。

常用选项：

| 选项 | 说明 |
|---|---|
| `NX` | 只有 key 不存在时才设置 |
| `XX` | 只有 key 已存在时才设置 |
| `GET` | 设置新值并返回旧值 |
| `EX seconds` | 设置秒级过期时间 |
| `PX milliseconds` | 设置毫秒级过期时间 |
| `KEEPTTL` | 覆盖值时保留原过期时间 |

```powershell
# key 不存在时才设置
127.0.0.1:6379> SET lock 100 NX
OK
127.0.0.1:6379> SET lock 200 NX
(nil)

# key 已存在时才设置
127.0.0.1:6379> SET lock 200 XX
OK

# 设置值并设置 60 秒过期时间
127.0.0.1:6379> SET code 123456 EX 60
OK

# 设置新值并返回旧值，Redis 6.2+
127.0.0.1:6379> SET name jack GET
"tom"
```

##### 2. GET：获取值

```text
GET key
```

- 返回 key 对应的字符串值。
- key 不存在时返回 `(nil)`。
- key 存在但不是 String 类型时返回 `WRONGTYPE` 错误。

```powershell
127.0.0.1:6379> GET name
"jack"
127.0.0.1:6379> GET not-exist
(nil)
```

##### 3. APPEND：追加内容

```text
APPEND key value
```

将 `value` 追加到原值末尾。

- key 不存在时，相当于把空字符串设置为 `value`。
- 返回追加后字符串的**字节长度**。

```powershell
127.0.0.1:6379> SET k1 aaa
OK
127.0.0.1:6379> APPEND k1 666
(integer) 6
127.0.0.1:6379> GET k1
"aaa666"
```

##### 4. STRLEN：获取字符串长度

```text
STRLEN key
```

- 返回字符串的**字节长度**。
- key 不存在时返回 `0`。
- 中文字符使用 UTF-8 时通常占 3 个字节。

```powershell
127.0.0.1:6379> STRLEN k1
(integer) 6
127.0.0.1:6379> SET msg 你好
OK
127.0.0.1:6379> STRLEN msg
(integer) 6
```

##### 5. SETNX：不存在时设置

```text
SETNX key value
```

只有 key 不存在时才设置：

- 设置成功返回 `1`。
- key 已存在时返回 `0`，不会修改旧值。

```powershell
127.0.0.1:6379> SETNX k1 first
(integer) 1
127.0.0.1:6379> SETNX k1 second
(integer) 0
127.0.0.1:6379> GET k1
"first"
```

现代 Redis 更推荐使用：

```text
SET key value NX
```

如果要实现带过期时间的锁，应在一条命令中完成：

```text
SET lock-key lock-value NX EX 30
```

不要先执行 `SETNX`，再单独执行 `EXPIRE`，否则两条命令之间发生异常时可能留下永不过期的 key。

##### 6. INCR、DECR：整数自增和自减

```text
INCR key
DECR key
```

- `INCR`：将整数值增加 `1`。
- `DECR`：将整数值减少 `1`。
- key 不存在时，先按 `0` 处理。
- 值必须能够表示为 64 位有符号整数。
- 操作具有原子性，适合实现计数器。

```powershell
127.0.0.1:6379> INCR count
(integer) 1
127.0.0.1:6379> INCR count
(integer) 2
127.0.0.1:6379> DECR count
(integer) 1

127.0.0.1:6379> SET k1 abc
OK
127.0.0.1:6379> INCR k1
(error) ERR value is not an integer or out of range
```

##### 7. INCRBY、DECRBY：按指定步长增减

```text
INCRBY key increment
DECRBY key decrement
```

```powershell
127.0.0.1:6379> SET count 100
OK
127.0.0.1:6379> INCRBY count 20
(integer) 120
127.0.0.1:6379> DECRBY count 5
(integer) 115
```

##### 8. INCRBYFLOAT：浮点数增减

```text
INCRBYFLOAT key increment
```

将 key 保存的浮点数增加指定数值。传入负数可以实现减法。

```powershell
127.0.0.1:6379> SET price 10.5
OK
127.0.0.1:6379> INCRBYFLOAT price 2.3
"12.8"
127.0.0.1:6379> INCRBYFLOAT price -0.8
"12"
```

##### 9. MSET：批量设置

```text
MSET key value [key value ...]
```

- 一次设置多个键值对。
- 会覆盖已经存在的 key。
- 操作具有原子性：所有键值一起完成设置。

```powershell
127.0.0.1:6379> MSET k1 v1 k2 v2 k3 v3
OK
```

##### 10. MGET：批量获取

```text
MGET key [key ...]
```

- 返回顺序与传入 key 的顺序一致。
- key 不存在或不是 String 类型时，对应位置返回 `(nil)`。

```powershell
127.0.0.1:6379> MGET k1 k2 not-exist
1) "v1"
2) "v2"
3) (nil)
```

##### 11. MSETNX：所有 key 都不存在时批量设置

```text
MSETNX key value [key value ...]
```

只有所有 key 都不存在时才执行：

- 全部设置成功返回 `1`。
- 只要有一个 key 已存在，就全部不设置并返回 `0`。
- 操作具有原子性，不会只设置其中一部分。

```powershell
127.0.0.1:6379> SET k1 old
OK
127.0.0.1:6379> MSETNX k1 new k2 v2
(integer) 0
127.0.0.1:6379> GET k2
(nil)
```

##### 12. GETRANGE：获取部分字符串

```text
GETRANGE key start end
```

- 索引从 `0` 开始。
- `end` 对应的字符也包含在结果中，与 Java 的 `substring()` 不同。
- 支持负数索引：`-1` 表示最后一个字节，`-2` 表示倒数第二个字节。
- key 不存在时返回空字符串。

```powershell
127.0.0.1:6379> SET msg abcdef
OK
127.0.0.1:6379> GETRANGE msg 0 2
"abc"
127.0.0.1:6379> GETRANGE msg -3 -1
"def"
127.0.0.1:6379> GETRANGE msg 0 -1
"abcdef"
```

> `GETRANGE` 按字节截取，直接截取中文 UTF-8 字符时可能截断字符并产生乱码。

##### 13. SETRANGE：覆盖部分字符串

```text
SETRANGE key offset value
```

从指定的字节偏移量开始，用 `value` 覆盖原字符串：

- offset 从 `0` 开始。
- 返回修改后字符串的字节长度。
- 不会自动删除后面未被覆盖的内容。
- key 不存在时会创建 key。
- offset 超过现有长度时，中间位置会使用空字节填充。

```powershell
127.0.0.1:6379> SET msg abcdef
OK
127.0.0.1:6379> SETRANGE msg 2 XYZ
(integer) 6
127.0.0.1:6379> GET msg
"abXYZf"
```

##### 14. SETEX、PSETEX：设置值和过期时间

```text
SETEX key seconds value
PSETEX key milliseconds value
```

设置键值的同时设置过期时间，整个操作具有原子性。

```powershell
127.0.0.1:6379> SETEX code 60 123456
OK
127.0.0.1:6379> TTL code
(integer) 58

127.0.0.1:6379> PSETEX token 5000 abc
OK
127.0.0.1:6379> PTTL token
(integer) 4996
```

现代 Redis 更推荐统一使用 `SET`：

```text
SET code 123456 EX 60
SET token abc PX 5000
```

##### 15. GETSET：设置新值并返回旧值

```text
GETSET key value
```

```powershell
127.0.0.1:6379> SET name tom
OK
127.0.0.1:6379> GETSET name jack
"tom"
127.0.0.1:6379> GET name
"jack"
```

`GETSET` 从 Redis 6.2 开始已废弃，推荐使用：

```text
SET key value GET
```

##### 16. GETEX：获取值并修改过期时间

Redis 6.2+：

```text
GETEX key [EX seconds | PX milliseconds | EXAT timestamp | PXAT timestamp | PERSIST]
```

获取值的同时设置、修改或删除过期时间：

```powershell
# 获取值并重新设置为 60 秒过期
127.0.0.1:6379> GETEX code EX 60
"123456"

# 获取值并删除过期时间，使 key 永久有效
127.0.0.1:6379> GETEX code PERSIST
"123456"
```

适合实现缓存的“读取并续期”。

##### 17. GETDEL：获取并删除

Redis 6.2+：

```text
GETDEL key
```

原子地获取 key 的值，然后删除 key。

```powershell
127.0.0.1:6379> SET code 123456
OK
127.0.0.1:6379> GETDEL code
"123456"
127.0.0.1:6379> GET code
(nil)
```

适合一次性验证码、一次性令牌等场景。

##### 18. 位操作命令

String 还可以被当作位数组使用。

```text
SETBIT key offset value
GETBIT key offset
BITCOUNT key [start end]
```

- `SETBIT`：设置指定偏移量的二进制位，只能设置为 `0` 或 `1`，返回修改前的位值。
- `GETBIT`：获取指定位置的位值。
- `BITCOUNT`：统计值中二进制位为 `1` 的数量。

```powershell
127.0.0.1:6379> SETBIT sign:2026-08 0 1
(integer) 0
127.0.0.1:6379> GETBIT sign:2026-08 0
(integer) 1
127.0.0.1:6379> BITCOUNT sign:2026-08
(integer) 1
```

可用于签到记录、用户状态和布尔标记等场景。

#### 注意事项

1. Redis 命令参数之间必须使用空格分隔：

    ```text
    SET key value
    ```

    不能写成：

    ```text
    SET <key><value>
    ```

2. `GETRANGE` 的结束索引包含在结果中：

    ```text
    GETRANGE key 0 2
    ```

    会获取索引 `0、1、2`，共三个字节。

3. `SET` 默认会清除 key 原来的过期时间。如需保留，应使用：

    ```text
    SET key value KEEPTTL
    ```

4. `INCR`、`DECR`、`INCRBY` 等操作都是原子的，适合实现计数器，但必须注意整数溢出。

5. 在 Redis Cluster 中，`MGET`、`MSET` 和 `MSETNX` 涉及的 key 通常需要处于同一个哈希槽。可以使用 Hash Tag：

    ```text
    MSET user:{1001}:name tom user:{1001}:age 18
    ```

6. 常用旧命令与推荐写法：

    | 旧写法 | 推荐写法 |
    |---|---|
    | `SETNX key value` | `SET key value NX` |
    | `SETEX key seconds value` | `SET key value EX seconds` |
    | `PSETEX key milliseconds value` | `SET key value PX milliseconds` |
    | `GETSET key value` | `SET key value GET` |


### list

####  简介


1. 一句话: list 类型, 保存多个数据，底层使用**双向链表**存储结构实现
1. Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部(左边)或者尾部(右边)。
2. 底层是个双向链表，对两端的操作性能高，通过索引下标的操作中间的节点性能较差


4. list 最佳实践
    redis 应用于具有操作先后顺序的数据控制
    应用场景：
    - 系统通知，按照时间顺序展示，将最近的通知列在前面
    - 其它，比如微信的最近转发，微博的最新关注等.（**后进先出**）


#### list 常用指令

1. `lpush/rpush <key><value1><value2><value3> ....`： 从左边/右边插入一个或多个值
    ```powershell
    127.0.0.1:6379> lpush listkey1 1 2 3 4 5
    (integer) 5
    127.0.0.1:6379> lrange listkey1 0 -1
    1) "5"
    2) "4"
    3) "3"
    4) "2"
    5) "1"
    ```

3. `lpop/rpop <key>`：从左边/右边吐出一个值
    - pop后，原值将不再存在于列表中

    ```powershell
    127.0.0.1:6379> lpop listkey1
    "5"
    127.0.0.1:6379> lrange listkey1 0 -1
    1) "4"
    2) "3"
    3) "2"
    4) "1"
    ```

4. `rpoplpush <key1><key2>`：从<key1>列表右边吐出一个值，插到<key2>列表左边


5. `lrange <key><start><stop>`： 按照索引下标获得元素(从左到右)


6. `lrange mylist 0 -1`： 0 左边第一个，-1 右边第一个，（0-1 表示获取所有）

    ```powershell
    127.0.0.1:6379> lrange listkey1 0 -1
    1) "4"
    2) "3"
    3) "2"
    4) "1"
    ```

7. `lindex <key><index>`：按照索引下标获得元素(从左到右)

    ```powershell
    127.0.0.1:6379> lindex listkey1 -1
    "1"
    ```

8. `llen <key>`：获得列表长度

    ```powershell
    127.0.0.1:6379> llen listkey1
    (integer) 4
    ```

9. `linsert <key> before <value><newvalue>`：在<value>的前面插入<newvalue>插入值
    - linsert默认只对查找到的第一个值前后进行插入
    ```powershell
    127.0.0.1:6379> lpush listkey2 1 2 3 4 5 6 7 6 5 4 3 2 1
    (integer) 13
    127.0.0.1:6379> LINSERT listkey2 before 5 666
    (integer) 14
    127.0.0.1:6379> lrange listkey2 0 -1
    1) "1"
    2) "2"
    3) "3"
    4) "4"
    5) "666"
    6) "5"
    7) "6"
    8) "7"
    9) "6"
    10) "5"
    11) "4"
    12) "3"
    13) "2"
    14) "1"
    127.0.0.1:6379> rINSERT listkey2 before 5 666
    (error) ERR unknown command 'rINSERT', with args beginning with: 'listkey2' 'before' '5' '666'
    127.0.0.1:6379> lINSERT listkey2 after 5 666
    (integer) 15
    127.0.0.1:6379> lrange listkey2 0 -1
    1) "1"
    2) "2"
    3) "3"
    4) "4"
    5) "666"
    6) "5"
    7) "666"
    8) "6"
    9) "7"
    10) "6"
    11) "5"
    12) "4"
    13) "3"
    14) "2"
    15) "1"
    ```

10. `lrem <key><n><value>`：从左边删除 n 个 value(从左到右)

    ```powershell

    ```

11. `lset<key><index><value>`：将列表 key 下标为 index 的值替换成 value

    ```powershell

    ```

#### List 常用指令（by codex）

> Redis List 是按照插入顺序排列的字符串列表，允许保存重复元素。
>
> List 支持从左右两端插入和弹出元素，可以实现栈、队列和双端队列。两端操作通常为 `O(1)`，中间位置操作通常为 `O(N)`。

##### 1. LPUSH、RPUSH：向列表两端插入

```text
LPUSH key element [element ...]
RPUSH key element [element ...]
```

- `LPUSH`：从列表左侧插入。
- `RPUSH`：从列表右侧插入。
- key 不存在时自动创建 List。
- 返回插入后列表的长度。

```powershell
127.0.0.1:6379> RPUSH mylist 1 2 3
(integer) 3
127.0.0.1:6379> LRANGE mylist 0 -1
1) "1"
2) "2"
3) "3"

127.0.0.1:6379> LPUSH mylist 0
(integer) 4
127.0.0.1:6379> LRANGE mylist 0 -1
1) "0"
2) "1"
3) "2"
4) "3"
```

一次向左插入多个元素时，最终顺序会反转：

```powershell
127.0.0.1:6379> LPUSH numbers 1 2 3
(integer) 3
127.0.0.1:6379> LRANGE numbers 0 -1
1) "3"
2) "2"
3) "1"
```

##### 2. LRANGE：获取指定范围的元素

```text
LRANGE key start stop
```

- 索引从 `0` 开始。
- `start` 和 `stop` 都包含在结果中。
- `-1` 表示最后一个元素，`-2` 表示倒数第二个元素。
- 不会删除列表中的元素。

```powershell
127.0.0.1:6379> RPUSH letters a b c d e
(integer) 5
127.0.0.1:6379> LRANGE letters 1 3
1) "b"
2) "c"
3) "d"
127.0.0.1:6379> LRANGE letters -3 -1
1) "c"
2) "d"
3) "e"
```

获取全部元素：

```text
LRANGE key 0 -1
```

##### 3. LINDEX：根据索引获取元素

```text
LINDEX key index
```

- 返回指定索引位置的元素。
- 支持负数索引。
- 索引超出范围或 key 不存在时返回 `(nil)`。
- 不会删除元素。

```powershell
127.0.0.1:6379> LINDEX letters 0
"a"
127.0.0.1:6379> LINDEX letters -1
"e"
127.0.0.1:6379> LINDEX letters 100
(nil)
```

##### 4. LLEN：获取列表长度

```text
LLEN key
```

- 返回列表中的元素数量。
- key 不存在时返回 `0`。

```powershell
127.0.0.1:6379> LLEN letters
(integer) 5
```

##### 5. LPOP、RPOP：从列表两端弹出

```text
LPOP key [count]
RPOP key [count]
```

- `LPOP`：删除并返回列表左侧元素。
- `RPOP`：删除并返回列表右侧元素。
- 被弹出的元素会从列表中删除。
- key 不存在时返回 `(nil)`。
- Redis 6.2+ 支持通过 `count` 一次弹出多个元素。

```powershell
127.0.0.1:6379> RPUSH tasks a b c d
(integer) 4
127.0.0.1:6379> LPOP tasks
"a"
127.0.0.1:6379> RPOP tasks 2
1) "d"
2) "c"
127.0.0.1:6379> LRANGE tasks 0 -1
1) "b"
```

最后一个元素被弹出后，对应的 key 会自动删除。

##### 6. LPUSHX、RPUSHX：列表存在时才插入

```text
LPUSHX key element [element ...]
RPUSHX key element [element ...]
```

- 只有 key 已存在且为 List 时才插入。
- key 不存在时不会创建 List，返回 `0`。
- 返回操作后的列表长度。

```powershell
127.0.0.1:6379> LPUSHX not-exist a
(integer) 0
127.0.0.1:6379> EXISTS not-exist
(integer) 0
```

##### 7. LINSERT：在指定元素前后插入

```text
LINSERT key BEFORE|AFTER pivot element
```

- 在第一个匹配的 `pivot` 前面或后面插入新元素。
- 插入成功时返回列表的新长度。
- key 不存在时返回 `0`。
- 没有找到 `pivot` 时返回 `-1`。

```powershell
127.0.0.1:6379> RPUSH colors red blue
(integer) 2
127.0.0.1:6379> LINSERT colors BEFORE blue green
(integer) 3
127.0.0.1:6379> LRANGE colors 0 -1
1) "red"
2) "green"
3) "blue"
```

##### 8. LSET：修改指定位置的元素

```text
LSET key index element
```

- 将指定索引位置的元素替换为新值。
- 支持负数索引。
- 修改成功返回 `OK`。
- key 不存在或索引越界时返回错误。

```powershell
127.0.0.1:6379> LSET colors 1 yellow
OK
127.0.0.1:6379> LSET colors -1 black
OK
127.0.0.1:6379> LRANGE colors 0 -1
1) "red"
2) "yellow"
3) "black"
```

##### 9. LREM：删除指定元素

```text
LREM key count element
```

从列表中删除与 `element` 相等的元素，返回实际删除的数量。

| count | 含义 |
|---|---|
| `count > 0` | 从左向右删除前 `count` 个匹配元素 |
| `count < 0` | 从右向左删除前 `abs(count)` 个匹配元素 |
| `count = 0` | 删除所有匹配元素 |

```powershell
127.0.0.1:6379> RPUSH letters a b a b a
(integer) 5
127.0.0.1:6379> LREM letters 2 a
(integer) 2
127.0.0.1:6379> LRANGE letters 0 -1
1) "b"
2) "b"
3) "a"

127.0.0.1:6379> LREM letters -1 b
(integer) 1
127.0.0.1:6379> LREM letters 0 a
(integer) 1
```

##### 10. LTRIM：只保留指定范围

```text
LTRIM key start stop
```

- 保留指定索引范围内的元素。
- 范围外的元素会被删除。
- `start` 和 `stop` 都包含在保留范围内。
- 如果保留范围为空，对应的 key 会被删除。

```powershell
127.0.0.1:6379> RPUSH news n1 n2 n3 n4 n5
(integer) 5
127.0.0.1:6379> LTRIM news 0 2
OK
127.0.0.1:6379> LRANGE news 0 -1
1) "n1"
2) "n2"
3) "n3"
```

只保留最新 100 条记录：

```text
LPUSH news new-message
LTRIM news 0 99
```

##### 11. LPOS：查找元素位置

Redis 6.0.6+：

```text
LPOS key element [RANK rank] [COUNT num-matches] [MAXLEN len]
```

- 默认返回第一个匹配元素的索引。
- 没有找到时返回 `(nil)`。
- `RANK`：查找第几个匹配元素。
- `COUNT`：返回多个匹配位置。
- `MAXLEN`：限制最多扫描的元素数量。

```powershell
127.0.0.1:6379> RPUSH letters a b a c
(integer) 4
127.0.0.1:6379> LPOS letters a
(integer) 0
127.0.0.1:6379> LPOS letters a RANK 2
(integer) 2
127.0.0.1:6379> LPOS letters a COUNT 0
1) (integer) 0
2) (integer) 2
```

##### 12. LMOVE：在两个列表之间移动元素

Redis 6.2+：

```text
LMOVE source destination LEFT|RIGHT LEFT|RIGHT
```

原子地从 source 的一端弹出元素，再插入 destination 的指定一端。

```powershell
127.0.0.1:6379> RPUSH waiting task1 task2
(integer) 2
127.0.0.1:6379> LMOVE waiting processing LEFT RIGHT
"task1"
127.0.0.1:6379> LRANGE waiting 0 -1
1) "task2"
127.0.0.1:6379> LRANGE processing 0 -1
1) "task1"
```

旧命令：

```text
RPOPLPUSH source destination
```

等价于：

```text
LMOVE source destination RIGHT LEFT
```

`RPOPLPUSH` 从 Redis 6.2 开始已废弃，推荐使用 `LMOVE`。

##### 13. BLPOP、BRPOP：阻塞式弹出

```text
BLPOP key [key ...] timeout
BRPOP key [key ...] timeout
```

- `BLPOP`：阻塞地从左侧弹出元素。
- `BRPOP`：阻塞地从右侧弹出元素。
- 列表为空时，会等待其他客户端插入元素。
- `timeout` 单位为秒。
- `timeout` 为 `0` 表示一直等待。
- 返回 key 名和弹出的元素。

客户端一：

```powershell
127.0.0.1:6379> BLPOP task-queue 10
```

客户端二：

```powershell
127.0.0.1:6379> RPUSH task-queue task1
(integer) 1
```

客户端一返回：

```powershell
1) "task-queue"
2) "task1"
```

##### 14. BLMOVE：阻塞式移动元素

Redis 6.2+：

```text
BLMOVE source destination LEFT|RIGHT LEFT|RIGHT timeout
```

- 功能与 `LMOVE` 相同。
- source 为空时会阻塞等待。
- `timeout` 为 `0` 表示一直等待。

旧命令：

```text
BRPOPLPUSH source destination timeout
```

等价于：

```text
BLMOVE source destination RIGHT LEFT timeout
```

`BRPOPLPUSH` 从 Redis 6.2 开始已废弃，推荐使用 `BLMOVE`。

#### 注意事项

1. Redis List 中的元素允许重复，并且按照插入顺序排列。

2. List 左侧为表头，右侧为表尾：

    ```text
    左侧                                   右侧
    index=0                           index=-1
       ↓                                  ↓
    [a] [b] [c] [d] [e]
    ```

3. `LRANGE` 的结束索引包含在结果中：

    ```text
    LRANGE key 0 2
    ```

    会返回索引 `0、1、2`，共三个元素。

4. `LPUSH key 1 2 3` 的最终顺序为：

    ```text
    3 2 1
    ```

    因为每个元素都会依次插入列表左侧。

5. `LPOP`、`RPOP` 会删除元素；`LINDEX`、`LRANGE` 只读取元素。

6. 最后一个元素被弹出或删除后，对应的 List key 会自动删除。

7. List 两端操作速度较快，但以下命令通常需要遍历列表：

    ```text
    LINDEX
    LINSERT
    LSET
    LREM
    ```

    不建议对超大 List 频繁执行中间位置操作。

8. 不建议在生产环境中对超大 List 直接执行：

    ```text
    LRANGE key 0 -1
    ```

    否则可能一次返回大量数据，占用网络带宽和客户端内存。

9. 实现普通队列：

    ```text
    RPUSH queue value
    LPOP queue
    ```

    实现栈：

    ```text
    LPUSH stack value
    LPOP stack
    ```

10. 使用 `LPUSH + LTRIM` 可以实现固定长度的最新消息列表：

    ```text
    LPUSH news new-message
    LTRIM news 0 99
    ```

11. 在 Redis Cluster 中，`LMOVE`、`BLMOVE` 等涉及两个 key 的命令，通常要求两个 key 位于同一个哈希槽：

    ```text
    LMOVE queue:{order}:waiting queue:{order}:processing LEFT RIGHT
    ```

12. 如果业务需要消费者组、消息确认、失败重试和消息追踪，应该优先考虑 Redis Stream，而不是仅使用 List。

13. 常用旧命令与推荐写法：

    | 旧命令 | 推荐命令 |
    |---|---|
    | `RPOPLPUSH source destination` | `LMOVE source destination RIGHT LEFT` |
    | `BRPOPLPUSH source destination timeout` | `BLMOVE source destination RIGHT LEFT timeout` |


### set

#### 简介

1. 一句话: set 提供的功能与 list 类似是一个列表的功能，特殊之处在于 set 是可以自动排重的, 即值是不允许重复的




#### set 常用指令

3. `sadd <key><value1><value2> .....`：将一个或多个 member 元素加入到集合 key 中，已经存在的member 元素将被忽略
4. `smembers <key>`：取出该集合的所有值。
5. `sismember <key><value>`：判断集合`<key>`是否为含有该`<value>`值，有 1，没有 0
6. `scard<key>`：返回该集合的元素个数。
    CARD 是数学术语 Cardinality（基数）的缩写。

    在数学集合论中，“基数”指的是一个集合中不重复元素的个数。Redis 直接借用了这个严格的数学词汇
7. `srem <key><value1><value2> ....`：删除集合中的某个元素。
8. `spop <key>`：随机从该集合中吐出一个值。
9. `srandmember <key><n>`：随机从该集合中取出 n 个值。不会从集合中删除 。
10. `smove <source><destination>value`： 把集合中一个值从一个集合移动到另一个集合
11. `sinter <key1><key2>`：返回两个集合的交集元素。
12. `sunion <key1><key2>`：返回两个集合的并集元素。
13. `sdiff <key1><key2>`：返回两个集合的差集元素(key1 中的，不包含 key2 中的)

#### Set 常用指令（by codex）

> Redis Set 是一个无序、元素唯一的字符串集合。
>
> Set 会自动去重，适合保存标签、好友关系、点赞用户、抽奖名单等数据，并支持交集、并集和差集运算。

##### 1. SADD：添加成员

```text
SADD key member [member ...]
```

- 向 Set 中添加一个或多个成员。
- key 不存在时自动创建 Set。
- 已存在的成员会被忽略。
- 返回本次成功添加的新成员数量，不包括重复成员。

```powershell
127.0.0.1:6379> SADD fruits apple banana apple
(integer) 2
127.0.0.1:6379> SADD fruits orange
(integer) 1
```

##### 2. SMEMBERS：获取全部成员

```text
SMEMBERS key
```

- 返回 Set 中的全部成员。
- key 不存在时返回空集合。
- 返回顺序不固定。

```powershell
127.0.0.1:6379> SMEMBERS fruits
1) "banana"
2) "apple"
3) "orange"
```

> Set 是无序集合，实际返回顺序可能与示例不同。

##### 3. SISMEMBER：判断成员是否存在

```text
SISMEMBER key member
```

- 成员存在时返回 `1`。
- 成员不存在或 key 不存在时返回 `0`。

```powershell
127.0.0.1:6379> SISMEMBER fruits apple
(integer) 1
127.0.0.1:6379> SISMEMBER fruits grape
(integer) 0
```

##### 4. SMISMEMBER：批量判断成员是否存在

Redis 6.2+：

```text
SMISMEMBER key member [member ...]
```

按照传入顺序返回每个成员是否存在：

```powershell
127.0.0.1:6379> SMISMEMBER fruits apple grape banana
1) (integer) 1
2) (integer) 0
3) (integer) 1
```

##### 5. SCARD：获取成员数量

```text
SCARD key
```

- 返回 Set 中成员的数量。
- key 不存在时返回 `0`。
- `CARD` 是 Cardinality，表示集合基数。

```powershell
127.0.0.1:6379> SCARD fruits
(integer) 3
```

##### 6. SREM：删除成员

```text
SREM key member [member ...]
```

- 删除一个或多个成员。
- 不存在的成员会被忽略。
- 返回实际删除的成员数量。

```powershell
127.0.0.1:6379> SREM fruits banana grape
(integer) 1
127.0.0.1:6379> SMEMBERS fruits
1) "apple"
2) "orange"
```

##### 7. SPOP：随机弹出成员

```text
SPOP key [count]
```

- 随机返回并删除一个或多个成员。
- 不指定 `count` 时返回一个成员。
- 指定 `count` 时返回最多 `count` 个不同成员。
- key 不存在时返回 `(nil)` 或空集合。

```powershell
127.0.0.1:6379> SADD numbers 1 2 3 4 5
(integer) 5
127.0.0.1:6379> SPOP numbers
"3"
127.0.0.1:6379> SPOP numbers 2
1) "1"
2) "5"
```

> `SPOP` 的结果是随机的，实际结果可能不同，并且被返回的成员会从 Set 中删除。

##### 8. SRANDMEMBER：随机获取成员

```text
SRANDMEMBER key [count]
```

随机返回成员，但不会删除成员。

`count` 的正负具有不同含义：

| count | 含义 |
|---|---|
| 不指定 | 随机返回一个成员 |
| `count > 0` | 返回最多 count 个不同成员 |
| `count < 0` | 返回 abs(count) 个成员，允许重复 |

```powershell
127.0.0.1:6379> SRANDMEMBER numbers
"2"
127.0.0.1:6379> SRANDMEMBER numbers 2
1) "2"
2) "4"
127.0.0.1:6379> SRANDMEMBER numbers -3
1) "2"
2) "4"
3) "2"
```

##### 9. SMOVE：在两个集合之间移动成员

```text
SMOVE source destination member
```

- 原子地从 source 中删除成员，并添加到 destination。
- 移动成功返回 `1`。
- source 中不存在该成员时返回 `0`。

```powershell
127.0.0.1:6379> SADD source a b
(integer) 2
127.0.0.1:6379> SADD destination c
(integer) 1
127.0.0.1:6379> SMOVE source destination a
(integer) 1

127.0.0.1:6379> SMEMBERS source
1) "b"
127.0.0.1:6379> SMEMBERS destination
1) "a"
2) "c"
```

##### 10. SINTER：求交集

```text
SINTER key [key ...]
```

返回同时存在于所有给定集合中的成员。

```powershell
127.0.0.1:6379> SADD set1 a b c
(integer) 3
127.0.0.1:6379> SADD set2 b c d
(integer) 3
127.0.0.1:6379> SINTER set1 set2
1) "b"
2) "c"
```

##### 11. SUNION：求并集

```text
SUNION key [key ...]
```

返回所有给定集合中的全部成员，重复成员只保留一个。

```powershell
127.0.0.1:6379> SUNION set1 set2
1) "a"
2) "b"
3) "c"
4) "d"
```

##### 12. SDIFF：求差集

```text
SDIFF key [key ...]
```

返回第一个集合中存在，但其他集合中不存在的成员。

```powershell
127.0.0.1:6379> SDIFF set1 set2
1) "a"
127.0.0.1:6379> SDIFF set2 set1
1) "d"
```

> 差集具有方向性，`SDIFF set1 set2` 和 `SDIFF set2 set1` 的结果通常不同。

##### 13. SINTERSTORE、SUNIONSTORE、SDIFFSTORE：保存计算结果

```text
SINTERSTORE destination key [key ...]
SUNIONSTORE destination key [key ...]
SDIFFSTORE destination key [key ...]
```

- 计算交集、并集或差集，并保存到 destination。
- destination 已存在时会被覆盖。
- 返回结果集合的成员数量。

```powershell
127.0.0.1:6379> SINTERSTORE result set1 set2
(integer) 2
127.0.0.1:6379> SMEMBERS result
1) "b"
2) "c"
```

##### 14. SSCAN：渐进式遍历集合

```text
SSCAN key cursor [MATCH pattern] [COUNT count]
```

- 渐进式遍历 Set，适合处理大型集合。
- 第一次游标传入 `0`。
- 返回的新游标为 `0` 时遍历结束。
- `MATCH` 用于匹配成员。
- `COUNT` 是每次期望扫描的数量，只是提示值，不保证准确。

```powershell
127.0.0.1:6379> SSCAN fruits 0 MATCH a* COUNT 10
1) "0"
2) 1) "apple"
```

#### 注意事项

1. Set 是无序集合，不能依赖 `SMEMBERS` 的返回顺序。

2. Set 中的成员不能重复：

    ```powershell
    127.0.0.1:6379> SADD fruits apple apple apple
    (integer) 1
    ```

3. `SADD` 返回的是本次新增成员的数量，不是集合总成员数。集合总成员数应使用：

    ```text
    SCARD key
    ```

4. `SPOP` 和 `SRANDMEMBER` 的区别：

    | 命令 | 是否随机 | 是否删除成员 |
    |---|---|---|
    | `SPOP` | 是 | 是 |
    | `SRANDMEMBER` | 是 | 否 |

5. `SDIFF` 差集运算具有方向性：

    ```text
    SDIFF set1 set2
    ```

    表示查找 set1 中存在、但 set2 中不存在的成员。

6. `SMEMBERS` 会一次返回全部成员。对于超大 Set，建议使用：

    ```text
    SSCAN key cursor
    ```

7. 交集、并集和差集计算可能消耗较多 CPU，不建议对多个超大 Set 高频执行集合运算。

8. `SINTERSTORE`、`SUNIONSTORE`、`SDIFFSTORE` 会覆盖 destination 原来的内容。

9. 最后一个成员被删除或弹出后，对应的 Set key 会自动删除。

10. Set 只保证成员唯一，不保证顺序。如果需要按照分数排序，应使用 Sorted Set，也就是 ZSet。

11. 在 Redis Cluster 中，多 key 集合运算通常要求所有 key 位于同一个哈希槽，可以使用 Hash Tag：

    ```text
    SINTER user:{1001}:following user:{1001}:recommended
    ```

12. 常见应用场景：

    - 数据去重
    - 用户标签
    - 点赞用户
    - 共同好友：`SINTER`
    - 可能认识的人：`SDIFF`
    - 抽奖：`SRANDMEMBER` 或 `SPOP`


### hash


#### 简介

1. Redis hash 是一个键值对集合，hash 适合用于存储对象， 类似 Java 里面的`Map<String,Object>`

2. key对应的Value是一个域，域中由Field和Value组成键值对

#### hash 常用指令


3. `hset <key><field><value>`：给`<key>`集合中的 `<field>`键赋值`<value>`
4. `hget <key1><field>`：从`<key1>`集合`<field>`取出 value （不存在则返回`(nil)`）
5. `hmset <key1><field1><value1><field2><value2>...`： 批量设置 hash 的值
6. `hmget <key1><field1> <field2>...`： 批量取出 hash 的 filed 值
7. `hexists<key1><field>`：查看哈希表 key 中，给定域 field 是否存在
8. `hkeys <key>`：列出该 hash 集合的所有 field
9. `hvals <key>`：列出该 hash 集合的所有 value
10. `hincrby <key><field><increment>`：为哈希表 key 中的域 field 的值value加上增量 1 -1
11. `hsetnx <key><field><value>`：将哈希表 key 中的域 field 的值设置为 value ，当且仅当域 field 不存在


#### Hash 常用指令（by codex）

> Redis Hash 是一个 field-value 键值对集合，结构类似 Java 中的 `Map<String, String>`。
>
> 一个 Redis key 对应一个 Hash，一个 Hash 中可以保存多个 field-value，适合存储对象的多个属性。

```text
Redis key：user:1001

┌─────────┬──────────┐
│ field   │ value    │
├─────────┼──────────┤
│ name    │ Tom      │
│ age     │ 18       │
│ email   │ tom@xx   │
└─────────┴──────────┘
```

##### 1. HSET：设置一个或多个字段

```text
HSET key field value [field value ...]
```

- key 不存在时自动创建 Hash。
- field 不存在时新增 field。
- field 已存在时覆盖旧值。
- 返回本次新增的 field 数量，不包括被覆盖的 field。

```powershell
127.0.0.1:6379> HSET user:1001 name Tom age 18
(integer) 2
127.0.0.1:6379> HSET user:1001 age 19
(integer) 0
```

第二条命令虽然返回 `0`，但 `age` 已经成功从 `18` 修改为 `19`。返回 `0` 是因为没有新增 field。

##### 2. HGET：获取指定字段

```text
HGET key field
```

- 返回指定 field 对应的 value。
- key 或 field 不存在时返回 `(nil)`。

```powershell
127.0.0.1:6379> HGET user:1001 name
"Tom"
127.0.0.1:6379> HGET user:1001 age
"19"
127.0.0.1:6379> HGET user:1001 address
(nil)
```

##### 3. HMGET：批量获取字段

```text
HMGET key field [field ...]
```

- 一次获取多个 field 的 value。
- 返回顺序与传入 field 的顺序一致。
- 不存在的 field 对应位置返回 `(nil)`。

```powershell
127.0.0.1:6379> HMGET user:1001 name age address
1) "Tom"
2) "19"
3) (nil)
```

##### 4. HMSET：批量设置字段

旧写法：

```text
HMSET key field value [field value ...]
```

`HMSET` 从 Redis 4.0 开始已废弃，应使用支持多个 field-value 的 `HSET`：

```text
HSET key field value [field value ...]
```

```powershell
127.0.0.1:6379> HSET user:1001 name Tom age 18 email tom@example.com
(integer) 3
```

##### 5. HGETALL：获取所有字段和值

```text
HGETALL key
```

返回 Hash 中的所有 field-value。

```powershell
127.0.0.1:6379> HGETALL user:1001
1) "name"
2) "Tom"
3) "age"
4) "18"
5) "email"
6) "tom@example.com"
```

返回结果按照下面的形式成对出现：

```text
field1
value1
field2
value2
```

Hash 本身不保证 field 的返回顺序。

##### 6. HEXISTS：判断字段是否存在

```text
HEXISTS key field
```

- field 存在时返回 `1`。
- field 不存在或 key 不存在时返回 `0`。

```powershell
127.0.0.1:6379> HEXISTS user:1001 name
(integer) 1
127.0.0.1:6379> HEXISTS user:1001 address
(integer) 0
```

##### 7. HKEYS：获取所有字段名

```text
HKEYS key
```

```powershell
127.0.0.1:6379> HKEYS user:1001
1) "name"
2) "age"
3) "email"
```

##### 8. HVALS：获取所有字段值

```text
HVALS key
```

```powershell
127.0.0.1:6379> HVALS user:1001
1) "Tom"
2) "18"
3) "tom@example.com"
```

##### 9. HLEN：获取字段数量

```text
HLEN key
```

- 返回 Hash 中 field 的数量。
- key 不存在时返回 `0`。

```powershell
127.0.0.1:6379> HLEN user:1001
(integer) 3
```

##### 10. HDEL：删除字段

```text
HDEL key field [field ...]
```

- 删除一个或多个 field。
- 不存在的 field 会被忽略。
- 返回实际删除的 field 数量。

```powershell
127.0.0.1:6379> HDEL user:1001 email address
(integer) 1
127.0.0.1:6379> HGET user:1001 email
(nil)
```

##### 11. HSETNX：字段不存在时设置

```text
HSETNX key field value
```

- 只有 field 不存在时才设置。
- 设置成功返回 `1`。
- field 已存在时返回 `0`，不会覆盖旧值。

```powershell
127.0.0.1:6379> HSETNX user:1001 phone 13800000000
(integer) 1
127.0.0.1:6379> HSETNX user:1001 phone 13900000000
(integer) 0
127.0.0.1:6379> HGET user:1001 phone
"13800000000"
```

##### 12. HINCRBY：整数增减

```text
HINCRBY key field increment
```

- 将 field 保存的整数增加指定值。
- increment 为负数时表示减少。
- field 不存在时先按 `0` 处理。
- field 的值必须能够表示为整数。
- 操作具有原子性。

```powershell
127.0.0.1:6379> HSET user:1001 age 18
(integer) 1
127.0.0.1:6379> HINCRBY user:1001 age 1
(integer) 19
127.0.0.1:6379> HINCRBY user:1001 age -2
(integer) 17
```

##### 13. HINCRBYFLOAT：浮点数增减

```text
HINCRBYFLOAT key field increment
```

- 将 field 保存的浮点数增加指定值。
- 传入负数可以实现减法。
- field 不存在时先按 `0` 处理。

```powershell
127.0.0.1:6379> HSET product:1 price 19.9
(integer) 1
127.0.0.1:6379> HINCRBYFLOAT product:1 price 1.6
"21.5"
127.0.0.1:6379> HINCRBYFLOAT product:1 price -0.5
"21"
```

##### 14. HSTRLEN：获取字段值的长度

```text
HSTRLEN key field
```

- 返回 field 对应 value 的字节长度。
- key 或 field 不存在时返回 `0`。

```powershell
127.0.0.1:6379> HSET user:1001 name Tom
(integer) 1
127.0.0.1:6379> HSTRLEN user:1001 name
(integer) 3
```

##### 15. HRANDFIELD：随机获取字段

Redis 6.2+：

```text
HRANDFIELD key [count [WITHVALUES]]
```

- 不指定 count 时，随机返回一个 field。
- count 为正数时，返回多个不重复 field。
- count 为负数时，允许重复返回 field。
- `WITHVALUES` 表示同时返回 value。
- 不会删除 field。

```powershell
127.0.0.1:6379> HRANDFIELD user:1001
"name"
127.0.0.1:6379> HRANDFIELD user:1001 2 WITHVALUES
1) "name"
2) "Tom"
3) "age"
4) "18"
```

> 返回结果是随机的，实际结果可能不同。

##### 16. HSCAN：渐进式遍历 Hash

```text
HSCAN key cursor [MATCH pattern] [COUNT count]
```

- 渐进式遍历 Hash，适合处理大型 Hash。
- 第一次游标传入 `0`。
- 返回的新游标为 `0` 时表示遍历结束。
- `MATCH` 可以筛选 field。
- `COUNT` 是每次期望扫描的数量。

```powershell
127.0.0.1:6379> HSCAN user:1001 0 MATCH n* COUNT 10
1) "0"
2) 1) "name"
   2) "Tom"
```

#### 注意事项

1. Redis Hash 类似于：

    ```java
    Map<String, String>
    ```

    field 和 value 本质上都是字符串，不能直接保存嵌套 Java 对象。复杂对象需要序列化，或者拆成多个 Hash。

2. 同一个 Hash 中 field 不能重复。再次设置相同 field 会覆盖旧值：

    ```text
    HSET user:1001 name Tom
    HSET user:1001 name Jack
    ```

3. `HSET` 返回的是本次新增 field 的数量，不是修改成功的数量：

    ```powershell
    127.0.0.1:6379> HSET user:1001 name Tom
    (integer) 1
    127.0.0.1:6379> HSET user:1001 name Jack
    (integer) 0
    ```

    第二条命令返回 `0`，但更新已经成功。

4. `HMSET` 已废弃，批量设置应使用：

    ```text
    HSET key field1 value1 field2 value2
    ```

5. `HGETALL`、`HKEYS` 和 `HVALS` 会一次返回全部数据。对于超大 Hash，建议使用：

    ```text
    HSCAN key cursor
    ```

6. Hash 不保证 field 的返回顺序，不能依赖 `HGETALL`、`HKEYS` 或 `HVALS` 的结果顺序。

7. `HDEL` 删除最后一个 field 后，对应的 Hash key 会自动删除。

8. `HINCRBY` 和 `HINCRBYFLOAT` 都是原子操作，适合实现对象属性计数，例如：

    ```text
    HINCRBY article:1001 views 1
    HINCRBY product:1001 stock -1
    ```

9. `HINCRBY` 只能操作整数值。如果 field 保存的不是合法整数，会返回错误。

10. 在较早的 Redis 版本中，过期时间只能设置在整个 Hash key 上：

    ```text
    EXPIRE user:1001 60
    ```

    这会使整个 Hash 过期。Redis 7.4+ 才开始支持 field 级过期时间，使用旧版本时不能单独为某个 field 设置过期时间。

11. Redis 7.4+ 设置 field 过期时间的基本写法：

    ```text
    HEXPIRE user:1001 60 FIELDS 1 code
    ```

    表示只让 `code` 字段在 60 秒后过期。使用前需要确认 Redis 版本。

12. Hash 适合保存结构相对稳定的对象属性，例如：

    - 用户信息
    - 商品信息
    - 购物车中商品数量
    - 文章统计数据
    - 系统配置

13. 不建议把无限增长的数据全部放进一个超大 Hash。Redis Cluster 按 key 分片，一个 Hash key 中的所有 field 都位于同一个节点，无法自动拆分到多个节点。

14. `HSTRLEN` 返回的是字节长度，不一定是字符数量。UTF-8 编码下，一个中文字符通常占 3 个字节。

15. 常用旧命令与推荐写法：

    | 旧命令 | 推荐命令 |
    |---|---|
    | `HMSET key field value [...]` | `HSET key field value [...]` |




### Zset（有序集合）

#### 简介
1. Redis 有序集合 zset 与普通集合 set 非常相似，是一个没有重复元素的字符串集合。
2. 不同之处是有序集合的每个成员都关联了一个评分(score),这个评分(score)被用来按照从最低分到最高分的方式排序集合中的成员。集合的成员是唯一的，但是评分可以是重复的，若评分相同，按照字典算法排序。
3. 因为元素是有序的, 所以也可以很快的根据评分(score)或者次序(position)来获取一个范围的元素。
4. 访问有序集合的中间元素也是非常快的, 你能够使用有序集合作为一个没有重复成员的列表。


#### sorted set 常用指令&使用

1. `zadd <key><score1><value1><score2><value2>…`： 将一个或多个 member 元素及其 score 值加入到有序集 key 当中。
4. `zrange <key><start><stop> [WITHSCORES]`： 返回有序集 key 中，下标在`<start>` `<stop>`之间的元素，带 `WITHSCORES`，可以让分数一起和值返回到结果集。**`<start>` `<stop>`可以为负数**，`zrevrange`可以倒序输出
5. `zscore <key><member>`： 返回有序集 key 中，成员 member 的 score 值
6. `zrangebyscore key min max [withscores] [limit offset count]`： 返回有序集 key 中，所有 score 值介于min 和 max 之间(包括等于 min 或 max )的成员。有序集成员按 score 值递增(从小到大)次序排列。
    - LIMIT是偏移，`limit 偏移量 条目数量`，即跳过前`offset`个元素
7. `zrevrangebyscore key max min [withscores] [limit offset count]`： 同上，改为从大到小排列。
8. `zincrby <key><increment><value>`： 为元素的 score 加上增量
9. `zrem <key><value>`：删除该集合下，指定值的元素
10. `zcount <key><min><max>`：统计该集合，分数区间内的元素个数
11. `zrank <key><value>`：返回该值在集合中的排名，从 0 开始。



#### ZSet 常用指令（by codex）

> Redis ZSet（Sorted Set）是一个有序、成员唯一的字符串集合。
>
> 每个 member 都关联一个 score，Redis 默认按照 score 从小到大排序。member 不能重复，但不同 member 可以拥有相同的 score。

##### 1. ZADD：添加成员或修改分数

```text
ZADD key [NX | XX] [GT | LT] [CH] [INCR] score member [score member ...]
```

- key 不存在时自动创建 ZSet。
- member 不存在时添加成员。
- member 已存在时更新 score，不会添加重复成员。
- 默认返回本次新增的成员数量，不包括只更新 score 的成员。

```powershell
127.0.0.1:6379> ZADD leaderboard 100 tom 80 jack 90 lucy
(integer) 3
127.0.0.1:6379> ZADD leaderboard 110 tom
(integer) 0
127.0.0.1:6379> ZSCORE leaderboard tom
"110"
```

第二条命令虽然返回 `0`，但 tom 的 score 已经成功更新为 `110`。

常用选项：

| 选项 | 含义 |
|---|---|
| `NX` | 只添加不存在的 member，不更新已有 member |
| `XX` | 只更新已存在的 member，不添加新 member |
| `GT` | 只有新 score 大于当前 score 时才更新 |
| `LT` | 只有新 score 小于当前 score 时才更新 |
| `CH` | 返回新增和分数发生变化的成员总数 |
| `INCR` | 将 score 增加指定值，类似 `ZINCRBY` |

```powershell
127.0.0.1:6379> ZADD leaderboard CH 120 tom
(integer) 1
127.0.0.1:6379> ZADD leaderboard NX 100 tom
(integer) 0
127.0.0.1:6379> ZADD leaderboard XX 95 lucy
(integer) 0
```

##### 2. ZCARD：获取成员数量

```text
ZCARD key
```

- 返回 ZSet 中成员的数量。
- key 不存在时返回 `0`。
- `CARD` 是 Cardinality，表示集合基数。

```powershell
127.0.0.1:6379> ZCARD leaderboard
(integer) 3
```

##### 3. ZSCORE：获取成员分数

```text
ZSCORE key member
```

- 返回指定 member 的 score。
- member 或 key 不存在时返回 `(nil)`。

```powershell
127.0.0.1:6379> ZSCORE leaderboard tom
"120"
127.0.0.1:6379> ZSCORE leaderboard bob
(nil)
```

##### 4. ZMSCORE：批量获取成员分数

Redis 6.2+：

```text
ZMSCORE key member [member ...]
```

按照传入顺序返回多个 member 的 score：

```powershell
127.0.0.1:6379> ZMSCORE leaderboard tom jack bob
1) "120"
2) "80"
3) (nil)
```

##### 5. ZRANGE：按照排名查询成员

```text
ZRANGE key start stop [REV] [WITHSCORES]
```

默认按照 score 从小到大返回指定排名范围的成员：

- 排名从 `0` 开始。
- `start` 和 `stop` 都包含在结果中。
- `-1` 表示最后一名。
- `WITHSCORES` 表示同时返回 score。
- `REV` 表示按照 score 从大到小排列。

```powershell
127.0.0.1:6379> ZRANGE leaderboard 0 -1 WITHSCORES
1) "jack"
2) "80"
3) "lucy"
4) "95"
5) "tom"
6) "120"
```

按照 score 从大到小查询前两名：

```powershell
127.0.0.1:6379> ZRANGE leaderboard 0 1 REV WITHSCORES
1) "tom"
2) "120"
3) "lucy"
4) "95"
```

##### 6. ZRANGE BYSCORE：按照分数范围查询

Redis 6.2+ 推荐写法：

```text
ZRANGE key min max BYSCORE [REV] [LIMIT offset count] [WITHSCORES]
```

查询 score 在指定范围内的成员，默认按照 score 从小到大排列。

```powershell
127.0.0.1:6379> ZRANGE leaderboard 80 100 BYSCORE WITHSCORES
1) "jack"
2) "80"
3) "lucy"
4) "95"
```

默认包含 min 和 max。使用圆括号表示不包含边界：

```powershell
127.0.0.1:6379> ZRANGE leaderboard (80 100 BYSCORE WITHSCORES
1) "lucy"
2) "95"
```

特殊值：

```text
-inf    表示负无穷
+inf    表示正无穷
```

查询所有成员：

```text
ZRANGE leaderboard -inf +inf BYSCORE WITHSCORES
```

分页查询：

```text
ZRANGE leaderboard 0 100 BYSCORE LIMIT 0 10 WITHSCORES
```

按照 score 从大到小查询时，需要把最大值写在前面：

```powershell
127.0.0.1:6379> ZRANGE leaderboard +inf -inf BYSCORE REV WITHSCORES
1) "tom"
2) "120"
3) "lucy"
4) "95"
5) "jack"
6) "80"
```

旧写法：

```text
ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]
ZREVRANGEBYSCORE key max min [WITHSCORES] [LIMIT offset count]
```

对应的新写法：

```text
ZRANGE key min max BYSCORE [LIMIT offset count] [WITHSCORES]
ZRANGE key max min BYSCORE REV [LIMIT offset count] [WITHSCORES]
```

`ZRANGEBYSCORE` 和 `ZREVRANGEBYSCORE` 从 Redis 6.2 开始已废弃，推荐统一使用 `ZRANGE`。

##### 7. ZRANK、ZREVRANK：获取成员排名

```text
ZRANK key member
ZREVRANK key member
```

- `ZRANK`：按照 score 从小到大计算排名。
- `ZREVRANK`：按照 score 从大到小计算排名。
- 排名从 `0` 开始。
- member 不存在时返回 `(nil)`。

```powershell
127.0.0.1:6379> ZRANK leaderboard tom
(integer) 2
127.0.0.1:6379> ZREVRANK leaderboard tom
(integer) 0

127.0.0.1:6379> ZRANK leaderboard jack
(integer) 0
127.0.0.1:6379> ZREVRANK leaderboard jack
(integer) 2
```

在排行榜场景中，通常使用 `ZREVRANK`，因为分数越高排名越靠前。

业务中显示的名次通常需要加 `1`：

```text
Redis 返回排名 0 → 第 1 名
Redis 返回排名 1 → 第 2 名
```

##### 8. ZINCRBY：增加或减少成员分数

```text
ZINCRBY key increment member
```

- 将 member 的 score 增加指定值。
- increment 为负数时表示减少。
- member 不存在时会先创建，初始 score 按 `0` 处理。
- 返回修改后的 score。
- 操作具有原子性。

```powershell
127.0.0.1:6379> ZINCRBY leaderboard 15 jack
"95"
127.0.0.1:6379> ZINCRBY leaderboard -5 tom
"115"
```

##### 9. ZCOUNT：统计分数范围内的成员数量

```text
ZCOUNT key min max
```

- 统计 score 在指定范围内的成员数量。
- 默认包含 min 和 max。
- 使用 `(数字` 表示不包含该边界。
- 支持 `-inf` 和 `+inf`。

```powershell
127.0.0.1:6379> ZCOUNT leaderboard 80 100
(integer) 2
127.0.0.1:6379> ZCOUNT leaderboard (80 100
(integer) 2
127.0.0.1:6379> ZCOUNT leaderboard -inf +inf
(integer) 3
```

##### 10. ZREM：删除指定成员

```text
ZREM key member [member ...]
```

- 删除一个或多个 member。
- 不存在的 member 会被忽略。
- 返回实际删除的成员数量。

```powershell
127.0.0.1:6379> ZREM leaderboard jack bob
(integer) 1
127.0.0.1:6379> ZSCORE leaderboard jack
(nil)
```

##### 11. ZREMRANGEBYRANK：按照排名范围删除

```text
ZREMRANGEBYRANK key start stop
```

- 删除指定排名范围内的成员。
- 默认按照 score 从小到大计算排名。
- `start` 和 `stop` 都包含在删除范围中。
- 支持负数排名。
- 返回实际删除的成员数量。

```powershell
127.0.0.1:6379> ZADD scores 10 a 20 b 30 c 40 d
(integer) 4
127.0.0.1:6379> ZREMRANGEBYRANK scores 0 1
(integer) 2
127.0.0.1:6379> ZRANGE scores 0 -1 WITHSCORES
1) "c"
2) "30"
3) "d"
4) "40"
```

删除分数最低的成员：

```text
ZREMRANGEBYRANK key 0 0
```

##### 12. ZREMRANGEBYSCORE：按照分数范围删除

```text
ZREMRANGEBYSCORE key min max
```

- 删除 score 在指定范围内的成员。
- 默认包含 min 和 max。
- 支持开区间以及正负无穷。
- 返回实际删除的成员数量。

```powershell
127.0.0.1:6379> ZADD scores 10 a 20 b 30 c 40 d
(integer) 4
127.0.0.1:6379> ZREMRANGEBYSCORE scores 10 20
(integer) 2
127.0.0.1:6379> ZRANGE scores 0 -1 WITHSCORES
1) "c"
2) "30"
3) "d"
4) "40"
```

删除 score 小于 60 的成员，但不包含 60：

```text
ZREMRANGEBYSCORE key -inf (60
```

##### 13. ZPOPMIN、ZPOPMAX：弹出最低分或最高分成员

```text
ZPOPMIN key [count]
ZPOPMAX key [count]
```

- `ZPOPMIN`：删除并返回 score 最低的成员。
- `ZPOPMAX`：删除并返回 score 最高的成员。
- 可以通过 count 一次弹出多个成员。
- 返回 member 和 score。

```powershell
127.0.0.1:6379> ZADD scores 10 a 20 b 30 c
(integer) 3
127.0.0.1:6379> ZPOPMIN scores
1) "a"
2) "10"
127.0.0.1:6379> ZPOPMAX scores
1) "c"
2) "30"
```

##### 14. ZRANDMEMBER：随机获取成员

Redis 6.2+：

```text
ZRANDMEMBER key [count [WITHSCORES]]
```

- 随机返回一个或多个成员。
- 不会删除成员。
- count 为正数时，返回不重复成员。
- count 为负数时，允许重复返回成员。
- `WITHSCORES` 表示同时返回 score。

```powershell
127.0.0.1:6379> ZRANDMEMBER leaderboard
"lucy"
127.0.0.1:6379> ZRANDMEMBER leaderboard 2 WITHSCORES
1) "lucy"
2) "95"
3) "tom"
4) "115"
```

> 返回结果是随机的，实际结果可能不同。

##### 15. ZSCAN：渐进式遍历 ZSet

```text
ZSCAN key cursor [MATCH pattern] [COUNT count]
```

- 渐进式遍历 ZSet，适合大型集合。
- 第一次游标传入 `0`。
- 返回的新游标为 `0` 时表示遍历结束。
- 返回 member 和 score。
- `MATCH` 用于匹配 member。
- `COUNT` 是每次期望扫描的数量。

```powershell
127.0.0.1:6379> ZSCAN leaderboard 0 MATCH t* COUNT 10
1) "0"
2) 1) "tom"
   2) "115"
```

> `ZSCAN` 的遍历结果不保证按照 score 排序。如果需要有序结果，应使用 `ZRANGE`。

##### 16. ZINTER、ZUNION：交集和并集

Redis 6.2+：

```text
ZINTER numkeys key [key ...]
    [WEIGHTS weight [weight ...]]
    [AGGREGATE SUM|MIN|MAX]
    [WITHSCORES]

ZUNION numkeys key [key ...]
    [WEIGHTS weight [weight ...]]
    [AGGREGATE SUM|MIN|MAX]
    [WITHSCORES]
```

- `ZINTER`：计算多个 ZSet 的交集。
- `ZUNION`：计算多个 ZSet 的并集。
- `numkeys` 表示参与运算的 key 数量。
- 默认使用 `SUM` 合并相同 member 的 score。
- `MIN` 取最小 score。
- `MAX` 取最大 score。
- `WEIGHTS` 可以为不同 ZSet 的 score 设置权重。

```powershell
127.0.0.1:6379> ZADD zset1 10 a 20 b
(integer) 2
127.0.0.1:6379> ZADD zset2 5 a 30 c
(integer) 2

127.0.0.1:6379> ZINTER 2 zset1 zset2 WITHSCORES
1) "a"
2) "15"

127.0.0.1:6379> ZUNION 2 zset1 zset2 WITHSCORES
1) "a"
2) "15"
3) "b"
4) "20"
5) "c"
6) "30"
```

保存结果可使用：

```text
ZINTERSTORE destination numkeys key [key ...]
ZUNIONSTORE destination numkeys key [key ...]
```

#### 注意事项

1. ZSet 中 member 唯一，但 score 可以重复：

    ```text
    ZADD leaderboard 100 tom 100 jack
    ```

    tom 和 jack 可以拥有相同的 score。

2. 再次添加同一个 member 不会产生重复数据，而是更新它的 score：

    ```text
    ZADD leaderboard 100 tom
    ZADD leaderboard 120 tom
    ```

3. `ZADD` 默认返回新增 member 的数量，不包括仅更新 score 的 member。需要统计分数发生变化的成员时使用：

    ```text
    ZADD key CH score member
    ```

4. ZSet 默认按照 score 从小到大排序：

    ```text
    ZRANGE key 0 -1
    ```

    按照 score 从大到小排序：

    ```text
    ZRANGE key 0 -1 REV
    ```

5. `ZRANK` 和 `ZREVRANK` 返回的排名从 `0` 开始。业务显示名次时通常需要加 `1`。

6. 多个 member 的 score 相同时，Redis 会按照 member 的字典序排列，而不是按照插入顺序排列。

7. 按排名查询与按分数查询的含义不同：

    ```text
    ZRANGE key 0 10
    ```

    表示查询排名 `0～10`。

    ```text
    ZRANGE key 0 10 BYSCORE
    ```

    表示查询 score `0～10`。

8. 分数范围默认是闭区间：

    ```text
    ZRANGE key 60 100 BYSCORE
    ```

    表示：

    ```text
    60 <= score <= 100
    ```

    使用圆括号排除边界：

    ```text
    ZRANGE key (60 100 BYSCORE
    ```

    表示：

    ```text
    60 < score <= 100
    ```

9. `ZPOPMIN`、`ZPOPMAX` 会删除成员；`ZRANGE`、`ZRANDMEMBER` 只读取，不会删除。

10. `ZSCAN` 不保证按照 score 顺序返回数据。有序查询应使用 `ZRANGE`。

11. score 使用双精度浮点数存储。整数在 `-2^53` 到 `2^53` 范围内可以精确表示；特别大的整数或小数运算可能出现浮点精度问题。

12. 不建议对超大 ZSet 直接执行：

    ```text
    ZRANGE key 0 -1 WITHSCORES
    ```

    否则可能一次返回大量数据。应使用分页范围或 `LIMIT`。

13. 常用操作的时间复杂度：

    | 命令 | 时间复杂度 |
    |---|---|
    | `ZADD` | `O(log N)` |
    | `ZREM` | `O(log N)` |
    | `ZSCORE` | `O(1)` |
    | `ZRANK`、`ZREVRANK` | `O(log N)` |
    | `ZRANGE` | `O(log N + M)` |
    | `ZCOUNT` | `O(log N)` |

    其中 N 是集合成员数，M 是返回的成员数。

14. 最后一个 member 被删除或弹出后，对应的 ZSet key 会自动删除。

15. 在 Redis Cluster 中，`ZINTER`、`ZUNION` 等多 key 命令通常要求所有 key 位于同一个哈希槽：

    ```text
    ZINTER 2 rank:{game}:week rank:{game}:month
    ```

16. Set 与 ZSet 的区别：

    | 数据类型 | 是否去重 | 是否有 score | 是否有序 |
    |---|---|---|---|
    | Set | 是 | 否 | 否 |
    | ZSet | 是 | 是 | 是 |

17. ZSet 常见应用场景：

    - 排行榜
    - 带权重的任务队列
    - 延迟任务
    - 热门商品
    - 成绩排名
    - 按时间戳排序的数据

18. 常用旧命令与推荐写法：

    | 旧命令 | Redis 6.2+ 推荐写法 |
    |---|---|
    | `ZRANGEBYSCORE key min max` | `ZRANGE key min max BYSCORE` |
    | `ZREVRANGEBYSCORE key max min` | `ZRANGE key max min BYSCORE REV` |
    | `ZREVRANGE key start stop` | `ZRANGE key start stop REV` |

## Redis 配置

### 基本介绍

1. 文档

    - https://www.cnblogs.com/nhdlb/p/14048083.html#_label0
    - https://redis.io/docs/latest/operate/oss_and_stack/management/config/

2. 原始配置（`7.2.16`）
    https://raw.githubusercontent.com/redis/redis/7.2.16/redis.conf


### 常规配置


#### 设置密码

#### daemonize

#### loglevel

#### logfile

#### 设定库的数量

### Units单位

### `#NETWORK#`

#### 配置块

1. 官方配置

    ```conf
    ################################## NETWORK #####################################

    # By default, if no "bind" configuration directive is specified, Redis listens
    # for connections from all available network interfaces on the host machine.
    # It is possible to listen to just one or multiple selected interfaces using
    # the "bind" configuration directive, followed by one or more IP addresses.
    # Each address can be prefixed by "-", which means that redis will not fail to
    # start if the address is not available. Being not available only refers to
    # addresses that does not correspond to any network interface. Addresses that
    # are already in use will always fail, and unsupported protocols will always BE
    # silently skipped.
    #
    # Examples:
    #
    # bind 192.168.1.100 10.0.0.1     # listens on two specific IPv4 addresses
    # bind 127.0.0.1 ::1              # listens on loopback IPv4 and IPv6
    # bind * -::*                     # like the default, all available interfaces
    #
    # ~~~ WARNING ~~~ If the computer running Redis is directly exposed to the
    # internet, binding to all the interfaces is dangerous and will expose the
    # instance to everybody on the internet. So by default we uncomment the
    # following bind directive, that will force Redis to listen only on the
    # IPv4 and IPv6 (if available) loopback interface addresses (this means Redis
    # will only be able to accept client connections from the same host that it is
    # running on).
    #
    # IF YOU ARE SURE YOU WANT YOUR INSTANCE TO LISTEN TO ALL THE INTERFACES
    # COMMENT OUT THE FOLLOWING LINE.
    #
    # You will also need to set a password unless you explicitly disable protected
    # mode.
    # ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    bind 127.0.0.1 -::1

    # By default, outgoing connections (from replica to master, from Sentinel to
    # instances, cluster bus, etc.) are not bound to a specific local address. In
    # most cases, this means the operating system will handle that based on routing
    # and the interface through which the connection goes out.
    #
    # Using bind-source-addr it is possible to configure a specific address to bind
    # to, which may also affect how the connection gets routed.
    #
    # Example:
    #
    # bind-source-addr 10.0.0.1

    # Protected mode is a layer of security protection, in order to avoid that
    # Redis instances left open on the internet are accessed and exploited.
    #
    # When protected mode is on and the default user has no password, the server
    # only accepts local connections from the IPv4 address (127.0.0.1), IPv6 address
    # (::1) or Unix domain sockets.
    #
    # By default protected mode is enabled. You should disable it only if
    # you are sure you want clients from other hosts to connect to Redis
    # even if no authentication is configured.
    protected-mode yes

    # Redis uses default hardened security configuration directives to reduce the
    # attack surface on innocent users. Therefore, several sensitive configuration
    # directives are immutable, and some potentially-dangerous commands are blocked.
    #
    # Configuration directives that control files that Redis writes to (e.g., 'dir'
    # and 'dbfilename') and that aren't usually modified during runtime
    # are protected by making them immutable.
    #
    # Commands that can increase the attack surface of Redis and that aren't usually
    # called by users are blocked by default.
    #
    # These can be exposed to either all connections or just local ones by setting
    # each of the configs listed below to either of these values:
    #
    # no    - Block for any connection (remain immutable)
    # yes   - Allow for any connection (no protection)
    # local - Allow only for local connections. Ones originating from the
    #         IPv4 address (127.0.0.1), IPv6 address (::1) or Unix domain sockets.
    #
    # enable-protected-configs no
    # enable-debug-command no
    # enable-module-command no

    # Accept connections on the specified port, default is 6379 (IANA #815344).
    # If port 0 is specified Redis will not listen on a TCP socket.
    port 6379

    # TCP listen() backlog.
    #
    # In high requests-per-second environments you need a high backlog in order
    # to avoid slow clients connection issues. Note that the Linux kernel
    # will silently truncate it to the value of /proc/sys/net/core/somaxconn so
    # make sure to raise both the value of somaxconn and tcp_max_syn_backlog
    # in order to get the desired effect.
    tcp-backlog 511

    # Unix socket.
    #
    # Specify the path for the Unix socket that will be used to listen for
    # incoming connections. There is no default, so Redis will not listen
    # on a unix socket when not specified.
    #
    # unixsocket /run/redis.sock
    # unixsocketperm 700

    # Close the connection after a client is idle for N seconds (0 to disable)
    timeout 0

    # TCP keepalive.
    #
    # If non-zero, use SO_KEEPALIVE to send TCP ACKs to clients in absence
    # of communication. This is useful for two reasons:
    #
    # 1) Detect dead peers.
    # 2) Force network equipment in the middle to consider the connection to be
    #    alive.
    #
    # On Linux, the specified value (in seconds) is the period used to send ACKs.
    # Note that to close the connection the double of the time is needed.
    # On other kernels the period depends on the kernel configuration.
    #
    # A reasonable value for this option is 300 seconds, which is the new
    # Redis default starting with Redis 3.2.1.
    tcp-keepalive 300

    # Apply OS-specific mechanism to mark the listening socket with the specified
    # ID, to support advanced routing and filtering capabilities.
    #
    # On Linux, the ID represents a connection mark.
    # On FreeBSD, the ID represents a socket cookie ID.
    # On OpenBSD, the ID represents a route table ID.
    #
    # The default value is 0, which implies no marking is required.
    # socket-mark-id 0
    ```

2. 注释译文

    ```conf
    ################################## 网络 #####################################

    # 默认情况下，如果未指定“bind”配置指令，Redis 会监听主机上所有可用网络接口的连接。
    # 可以使用“bind”配置指令，后跟一个或多个 IP 地址，来仅监听一个或多个选定的接口。
    # 每个地址前可加“-”前缀，表示如果该地址不可用时，Redis 不会启动失败。不可用仅指不对应于任何网络接口的地址。
    # 已被占用的地址总会失败，不支持的协议总会被静默跳过。
    #
    # 示例：
    #
    # bind 192.168.1.100 10.0.0.1     # 监听两个特定的 IPv4 地址
    # bind 127.0.0.1 ::1              # 监听 IPv4 和 IPv6 的回环地址
    # bind * -::*                     # 类似于默认值，监听所有可用接口
    #
    # ~~~ 警告 ~~~ 如果运行 Redis 的计算机直接暴露于互联网，绑定到所有接口是危险的，会将实例暴露给互联网上的所有人。
    # 因此，默认情况下我们取消注释以下 bind 指令，这将强制 Redis 仅监听 IPv4 和 IPv6（如果可用）的回环接口地址
    #（这意味着 Redis 将只能接受来自运行它的同一主机的客户端连接）。
    #
    # 如果你确定要让实例监听所有接口，请注释掉下面那行。
    #
    # 除非你显式禁用保护模式，否则你还需要设置密码。
    # ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    bind 127.0.0.1 -::1

    # 默认情况下，出站连接（从副本到主节点、从 Sentinel 到实例、集群总线等）不绑定到特定的本地地址。
    # 在大多数情况下，这意味着操作系统将根据路由和连接出去的接口来处理。
    #
    # 使用 bind-source-addr 可以配置要绑定的特定地址，这也可能影响连接的路由方式。
    #
    # 示例：
    #
    # bind-source-addr 10.0.0.1

    # 保护模式是一层安全防护，用于避免 Redis 实例暴露在互联网上被访问和利用。
    #
    # 当保护模式开启且默认用户没有密码时，服务器仅接受来自 IPv4 地址（127.0.0.1）、IPv6 地址（::1）或 Unix 域套接字的本地连接。
    #
    # 默认情况下保护模式是启用的。只有在确定即使未配置身份验证也想允许来自其他主机的客户端连接 Redis 时，才应禁用它。
    protected-mode yes

    # Redis 使用默认的强化安全配置指令来减少普通用户的攻击面。因此，一些敏感的配置指令是不可变的，某些潜在危险的命令被阻止。
    #
    # 控制 Redis 写入文件（例如“dir”和“dbfilename”）且通常在运行时不会被修改的配置指令，通过设为不可变来保护。
    #
    # 可能增加 Redis 攻击面且通常不会被用户调用的命令，默认被阻止。
    #
    # 可以通过将下面列出的每个配置项设置为以下值之一，将这些命令暴露给所有连接或仅本地连接：
    #
    # no    - 阻止任何连接（保持不可变）
    # yes   - 允许任何连接（无保护）
    # local - 仅允许本地连接。即源自 IPv4 地址（127.0.0.1）、IPv6 地址（::1）或 Unix 域套接字的连接。
    #
    # enable-protected-configs no
    # enable-debug-command no
    # enable-module-command no

    # 在指定端口上接受连接，默认端口为 6379（IANA #815344）。
    # 如果端口指定为 0，则 Redis 不会监听 TCP 套接字。
    port 6379

    # TCP listen() 积压（backlog）。
    #
    # 在每秒请求数很高的环境中，需要较大的积压值以避免慢客户端连接问题。
    # 请注意，Linux 内核会将其静默截断为 /proc/sys/net/core/somaxconn 的值，
    # 因此请确保同时提高 somaxconn 和 tcp_max_syn_backlog 的值以达到预期效果。
    tcp-backlog 511

    # Unix 套接字。
    #
    # 指定用于监听传入连接的 Unix 套接字路径。没有默认值，因此未指定时 Redis 不会监听 Unix 套接字。
    #
    # unixsocket /run/redis.sock
    # unixsocketperm 700

    # 客户端空闲 N 秒后关闭连接（0 表示禁用）
    timeout 0

    # TCP keepalive。
    #
    # 如果非零，则使用 SO_KEEPALIVE 在无通信时向客户端发送 TCP ACK。
    # 这有两个好处：
    #
    # 1) 检测死对端。
    # 2) 强制中间的网路设备认为连接是存活的。
    #
    # 在 Linux 上，指定的值（秒）是发送 ACK 的周期。
    # 注意，关闭连接需要双倍时间。
    # 在其他内核上，周期取决于内核配置。
    #
    # 该选项的合理值是 300 秒，这是自 Redis 3.2.1 起的新默认值。
    tcp-keepalive 300

    # 应用操作系统特定机制，使用指定 ID 标记监听套接字，以支持高级路由和过滤功能。
    #
    # 在 Linux 上，该 ID 表示连接标记。
    # 在 FreeBSD 上，该 ID 表示套接字 cookie ID。
    # 在 OpenBSD 上，该 ID 表示路由表 ID。
    #
    # 默认值为 0，表示不需要标记。
    # socket-mark-id 0
    ```

#### 属性解析

1. bind
    - 该属性用于通过允许登录Redis的地址，`127.0.0.1`即默认仅允许本地登录，外部访问需通过ssh隧道访问
    - 注释掉此项，即默认接受所有地址的访问
    ```conf
    bind 127.0.0.1 -::1
    ```

2. protected-mode
    - 保护模式开关
    > 当保护模式开启且默认用户没有密码时，服务器仅接受来自 IPv4 地址（127.0.0.1）、IPv6 地址（::1）或 Unix 域套接字的本地连接。

    ```conf
    protected-mode yes
    ```

3. port
    - Redis 的默认端口为6379
    - 为了防止在公网上被扫描到，可以选择将端口修改成其他值

    ```conf
    port 6379
    ```

    - 修改端口后，关闭服务不能使用`redis-cli shutdown`，而应该使用`redis-cli -p <你的服务端口> shutdown`

4. timeout
    - 客户端超时时间，默认为`0`，意为永不超时
    ```conf
    # 永不超时
    timeout 0

    # 30s后超时
    timeout 30
    ```

    - 超时指的是`redis-cli`开启后的无操作时长，超时后报错，需重新连接



5. tcp-keepalive

    >**关于这个“300”秒的补充细节（非常重要）：**
    >
    >在 Linux 系统中，`tcp-keepalive` 其实是**三层探测机制**：
    >
    >1.  **空闲时间**（即你设置的 `300`）：连接空闲 300 秒后开始探测。
    >2.  **探测间隔**（系统内核参数 `tcp_keepalive_intvl`，默认 75秒）：如果第一次探测没回应，每隔 75 秒再发一次。
    >3.  **探测次数**（系统内核参数 `tcp_keepalive_probes`，默认 9次）：连续发 9 次都没回应，才判定连接死亡。
    >
    >**所以，实际判定连接死亡的总时长大约是：**
    >300秒（空闲）+ 75秒 × 9次（探测） ≈ **975秒（约16分钟）**。
    >
    >**建议：**
    >`300` 是 Redis 官方推荐的黄金值。**不建议设得太短**（比如 10 秒），因为频繁的心跳包会额外消耗 Redis 的 CPU 资源和网络带宽，在高并发场景下反而会影响性能。设成 300 秒既能及时清理僵尸，又不会对性能造成明显负担。


### `#GENERAL#` 通用

#### 配置块

1. 官方配置

    ```conf
    ################################# GENERAL #####################################
    
    # By default Redis does not run as a daemon. Use 'yes' if you need it.
    # Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
    # When Redis is supervised by upstart or systemd, this parameter has no impact.
    daemonize no
    
    # If you run Redis from upstart or systemd, Redis can interact with your
    # supervision tree. Options:
    #   supervised no      - no supervision interaction
    #   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
    #                        requires "expect stop" in your upstart job config
    #   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
    #                        on startup, and updating Redis status on a regular
    #                        basis.
    #   supervised auto    - detect upstart or systemd method based on
    #                        UPSTART_JOB or NOTIFY_SOCKET environment variables
    # Note: these supervision methods only signal "process is ready."
    #       They do not enable continuous pings back to your supervisor.
    #
    # The default is "no". To run under upstart/systemd, you can simply uncomment
    # the line below:
    #
    # supervised auto
    
    # If a pid file is specified, Redis writes it where specified at startup
    # and removes it at exit.
    #
    # When the server runs non daemonized, no pid file is created if none is
    # specified in the configuration. When the server is daemonized, the pid file
    # is used even if not specified, defaulting to "/var/run/redis.pid".
    #
    # Creating a pid file is best effort: if Redis is not able to create it
    # nothing bad happens, the server will start and run normally.
    #
    # Note that on modern Linux systems "/run/redis.pid" is more conforming
    # and should be used instead.
    pidfile /var/run/redis_6379.pid
    
    # Specify the server verbosity level.
    # This can be one of:
    # debug (a lot of information, useful for development/testing)
    # verbose (many rarely useful info, but not a mess like the debug level)
    # notice (moderately verbose, what you want in production probably)
    # warning (only very important / critical messages are logged)
    # nothing (nothing is logged)
    loglevel notice
    
    # Specify the log file name. Also the empty string can be used to force
    # Redis to log on the standard output. Note that if you use standard
    # output for logging but daemonize, logs will be sent to /dev/null
    logfile ""
    
    # To enable logging to the system logger, just set 'syslog-enabled' to yes,
    # and optionally update the other syslog parameters to suit your needs.
    # syslog-enabled no
    
    # Specify the syslog identity.
    # syslog-ident redis
    
    # Specify the syslog facility. Must be USER or between LOCAL0-LOCAL7.
    # syslog-facility local0
    
    # To disable the built in crash log, which will possibly produce cleaner core
    # dumps when they are needed, uncomment the following:
    #
    # crash-log-enabled no
    
    # To disable the fast memory check that's run as part of the crash log, which
    # will possibly let redis terminate sooner, uncomment the following:
    #
    # crash-memcheck-enabled no
    
    # Set the number of databases. The default database is DB 0, you can select
    # a different one on a per-connection basis using SELECT <dbid> where
    # dbid is a number between 0 and 'databases'-1
    databases 16
    
    # By default Redis shows an ASCII art logo only when started to log to the
    # standard output and if the standard output is a TTY and syslog logging is
    # disabled. Basically this means that normally a logo is displayed only in
    # interactive sessions.
    #
    # However it is possible to force the pre-4.0 behavior and always show a
    # ASCII art logo in startup logs by setting the following option to yes.
    always-show-logo no
    
    # By default, Redis modifies the process title (as seen in 'top' and 'ps') to
    # provide some runtime information. It is possible to disable this and leave
    # the process name as executed by setting the following to no.
    set-proc-title yes
    
    # When changing the process title, Redis uses the following template to construct
    # the modified title.
    #
    # Template variables are specified in curly brackets. The following variables are
    # supported:
    #
    # {title}           Name of process as executed if parent, or type of child process.
    # {listen-addr}     Bind address or '*' followed by TCP or TLS port listening on, or
    #                   Unix socket if only that's available.
    # {server-mode}     Special mode, i.e. "[sentinel]" or "[cluster]".
    # {port}            TCP port listening on, or 0.
    # {tls-port}        TLS port listening on, or 0.
    # {unixsocket}      Unix domain socket listening on, or "".
    # {config-file}     Name of configuration file used.
    #
    proc-title-template "{title} {listen-addr} {server-mode}"
    
    # Set the local environment which is used for string comparison operations, and 
    # also affect the performance of Lua scripts. Empty String indicates the locale 
    # is derived from the environment variables.
    locale-collate ""
    ```

2. 注释译文

    ```conf
    ################################# 通用配置 #####################################
    
    # Redis 默认不以守护进程运行，需要时设为 yes。
    # Redis 后台运行时会把 PID 写入 /var/run/redis.pid。
    # 如果 Redis 由 upstart 或 systemd 管理，此参数没有影响。
    daemonize no
    
    # 如果通过 upstart 或 systemd 运行 Redis，Redis 可以与进程监管体系交互。可选值：
    #   supervised no      - 不与监管系统交互
    #   supervised upstart - 通过让 Redis 进入 SIGSTOP 状态通知 upstart；
    #                        upstart 作业配置中需要写 expect stop
    #   supervised systemd - 启动时向 $NOTIFY_SOCKET 写入 READY=1 通知 systemd，
    #                        并定期更新 Redis 状态
    #   supervised auto    - 根据 UPSTART_JOB 或 NOTIFY_SOCKET 环境变量，
    #                        自动检测使用 upstart 还是 systemd
    # 注意：这些监管方式只会通知“进程已就绪”，不会持续向监管程序发送心跳。
    #
    # 默认值为 no。要在 upstart/systemd 下运行，可以取消下面一行的注释：
    #
    # supervised auto
    
    # 如果指定了 PID 文件，Redis 会在启动时写入，并在退出时删除。
    #
    # 服务器以前台方式运行时，如果配置中没有指定 PID 文件，就不会创建。
    # 服务器作为守护进程运行时，即使没有指定也会使用 PID 文件，默认路径为
    # /var/run/redis.pid。
    #
    # 创建 PID 文件属于尽力而为：即使 Redis 无法创建它，服务器仍会正常启动运行。
    #
    # 在现代 Linux 系统中，使用 /run/redis.pid 更符合规范。
    pidfile /var/run/redis_6379.pid
    
    # 指定服务器日志详细程度，可选值：
    # debug   - 大量信息，适合开发和测试
    # verbose - 信息很多，通常用处不大，但不像 debug 那么繁杂
    # notice  - 适度详细，通常适合生产环境
    # warning - 只记录非常重要或严重的消息
    # nothing - 不记录任何日志
    loglevel notice
    
    # 指定日志文件名。空字符串可以强制 Redis 把日志写到标准输出。
    # 注意：如果既使用标准输出记录日志又启用 daemonize，日志会被发送到 /dev/null。
    logfile ""
    
    # 要启用系统日志，把 syslog-enabled 设为 yes，并按需要调整其他 syslog 参数。
    # syslog-enabled no
    
    # 指定 syslog 中的进程标识。
    # syslog-ident redis
    
    # 指定 syslog facility，必须是 USER 或 LOCAL0 到 LOCAL7 之一。
    # syslog-facility local0
    
    # 如果要关闭内置崩溃日志（需要 core dump 时可能使其更干净），取消下面一行的注释：
    #
    # crash-log-enabled no
    
    # 如果要关闭崩溃日志阶段执行的快速内存检查（可能让 Redis 更快终止），
    # 取消下面一行的注释：
    #
    # crash-memcheck-enabled no
    
    # 设置数据库数量。默认数据库是 DB 0。每条连接可以使用 SELECT <dbid> 选择其他数据库，
    # dbid 的取值范围为 0 到 databases-1。
    databases 16
    
    # 默认情况下，只有当 Redis 把日志写到标准输出、标准输出是 TTY 且 syslog 关闭时，
    # 才会显示 ASCII 艺术字标志。也就是说，通常只在交互式会话中显示。
    #
    # 如果希望恢复 4.0 之前的行为，始终在启动日志中显示标志，可把下面选项设为 yes。
    always-show-logo no
    
    # Redis 默认会修改进程标题（可在 top 和 ps 中看到），以提供一些运行时信息。
    # 把下面配置设为 no，可以关闭该行为并保留启动时的原始进程名。
    set-proc-title yes
    
    # 修改进程标题时，Redis 使用以下模板构造新标题。
    #
    # 模板变量写在花括号中，支持以下变量：
    #
    # {title}           父进程的启动名称，或子进程类型
    # {listen-addr}     绑定地址，或“*”加正在监听的 TCP/TLS 端口；如果只使用
    #                   Unix 套接字，则显示该套接字
    # {server-mode}     特殊模式，例如 [sentinel] 或 [cluster]
    # {port}            正在监听的 TCP 端口，或 0
    # {tls-port}        正在监听的 TLS 端口，或 0
    # {unixsocket}      正在监听的 Unix 域套接字，或空字符串
    # {config-file}     使用的配置文件名称
    #
    proc-title-template "{title} {listen-addr} {server-mode}"
    
    # 设置字符串比较操作使用的本地区域环境，它也会影响 Lua 脚本性能。
    # 空字符串表示从环境变量获取区域设置。
    locale-collate ""
    ```

#### 属性解析

1. `daemonize`

    - 控制 Redis 是否自行转入后台运行。
    - **Docker 中必须保持 `no`，让 `redis-server` 作为容器前台主进程；否则主进程退出后容器也会退出。**
    - systemd 管理 Redis 时通常也保持 `no`，后台化由 systemd 负责。

    ```conf
    daemonize no
    ```

2. `supervised`

    - 控制 Redis 是否向 upstart 或 systemd 报告“进程已就绪”。
    - `no` 表示不与监管系统交互；`systemd`、`upstart` 分别使用对应机制；`auto` 根据环境变量判断。
    - Docker 容器通常使用 `no`。

    ```conf
    # supervised auto
    ```

3. `pidfile`

    - 指定 PID 文件路径，这个文件会记录进程的PID号。以前台方式运行且未指定时，不需要 PID 文件。
    - 守护进程模式下即使未配置也会使用默认 PID 文件。
    - Docker 中基本不依赖 PID 文件；现代 Linux 更推荐 `/run/redis.pid`。

    ```conf
    pidfile /var/run/redis_6379.pid
    ```

4. `loglevel`

    - `debug`：信息最多，适合开发排错。
    - `verbose`：较详细。
    - `notice`：官方模板默认值，**通常适合生产环境**。
    - `warning`：只记录重要和严重信息。
    - `nothing`：不记录日志，一般不建议。

    ```conf
    loglevel notice
    ```

5. `logfile`

    - 空字符串表示输出到标准输出，最适合 Docker，由 `docker logs` 统一收集。
    - 如果 `daemonize yes` 同时使用空字符串，标准输出会被送到 `/dev/null`，日志相当于丢失。
    - 传统宿主机部署也可以指定绝对文件路径，但要确保 Redis 用户有写权限并配置日志轮转。

    ```conf
    logfile ""
    ```

6. `syslog-enabled`、`syslog-ident`、`syslog-facility`

    - 用于把日志写入系统日志服务。
    - Docker 一般不启用，继续写标准输出即可。
    - `facility` 只能使用 `USER` 或 `LOCAL0` 到 `LOCAL7`。

7. `crash-log-enabled` 与 `crash-memcheck-enabled`

    - 内置崩溃日志和快速内存检查默认开启。
    - 没有明确的 core dump 调试需求时，不建议关闭。

8. `databases`

    - 默认创建 16 个逻辑数据库，编号 `0` 到 `15`。
    - 使用 `SELECT <dbid>` 切换；连接池必须明确数据库编号，避免借出连接时状态混乱。
    - Redis Cluster 只支持数据库 `0`，不能依靠多逻辑库隔离业务。

    ```conf
    databases 16
    ```

9. `always-show-logo`

    - 控制是否始终在启动日志显示 Redis ASCII 图标。
    - 只影响日志外观，不影响性能和功能。

10. `set-proc-title` 与 `proc-title-template`

    - 用于修改 `top`、`ps` 中看到的进程标题，便于区分端口、模式和子进程。
    - 模板里的 `{title}`、`{listen-addr}`、`{server-mode}` 等变量由 Redis 替换。

11. `locale-collate`

    - 控制字符串比较使用的 locale，也可能影响 Lua 脚本性能。
    - 空字符串表示从进程环境变量继承；没有明确排序兼容需求时保持默认即可。

---

### `#SECURITY#` 安全

#### 配置块

1. 官方配置

    ```conf
    ################################## SECURITY ###################################
    
    # Warning: since Redis is pretty fast, an outside user can try up to
    # 1 million passwords per second against a modern box. This means that you
    # should use very strong passwords, otherwise they will be very easy to break.
    # Note that because the password is really a shared secret between the client
    # and the server, and should not be memorized by any human, the password
    # can be easily a long string from /dev/urandom or whatever, so by using a
    # long and unguessable password no brute force attack will be possible.
    
    # Redis ACL users are defined in the following format:
    #
    #   user <username> ... acl rules ...
    #
    # For example:
    #
    #   user worker +@list +@connection ~jobs:* on >ffa9203c493aa99
    #
    # The special username "default" is used for new connections. If this user
    # has the "nopass" rule, then new connections will be immediately authenticated
    # as the "default" user without the need of any password provided via the
    # AUTH command. Otherwise if the "default" user is not flagged with "nopass"
    # the connections will start in not authenticated state, and will require
    # AUTH (or the HELLO command AUTH option) in order to be authenticated and
    # start to work.
    #
    # The ACL rules that describe what a user can do are the following:
    #
    #  on           Enable the user: it is possible to authenticate as this user.
    #  off          Disable the user: it's no longer possible to authenticate
    #               with this user, however the already authenticated connections
    #               will still work.
    #  skip-sanitize-payload    RESTORE dump-payload sanitization is skipped.
    #  sanitize-payload         RESTORE dump-payload is sanitized (default).
    #  +<command>   Allow the execution of that command.
    #               May be used with `|` for allowing subcommands (e.g "+config|get")
    #  -<command>   Disallow the execution of that command.
    #               May be used with `|` for blocking subcommands (e.g "-config|set")
    #  +@<category> Allow the execution of all the commands in such category
    #               with valid categories are like @admin, @set, @sortedset, ...
    #               and so forth, see the full list in the server.c file where
    #               the Redis command table is described and defined.
    #               The special category @all means all the commands, but currently
    #               present in the server, and that will be loaded in the future
    #               via modules.
    #  +<command>|first-arg  Allow a specific first argument of an otherwise
    #                        disabled command. It is only supported on commands with
    #                        no sub-commands, and is not allowed as negative form
    #                        like -SELECT|1, only additive starting with "+". This
    #                        feature is deprecated and may be removed in the future.
    #  allcommands  Alias for +@all. Note that it implies the ability to execute
    #               all the future commands loaded via the modules system.
    #  nocommands   Alias for -@all.
    #  ~<pattern>   Add a pattern of keys that can be mentioned as part of
    #               commands. For instance ~* allows all the keys. The pattern
    #               is a glob-style pattern like the one of KEYS.
    #               It is possible to specify multiple patterns.
    # %R~<pattern>  Add key read pattern that specifies which keys can be read 
    #               from.
    # %W~<pattern>  Add key write pattern that specifies which keys can be
    #               written to. 
    #  allkeys      Alias for ~*
    #  resetkeys    Flush the list of allowed keys patterns.
    #  &<pattern>   Add a glob-style pattern of Pub/Sub channels that can be
    #               accessed by the user. It is possible to specify multiple channel
    #               patterns.
    #  allchannels  Alias for &*
    #  resetchannels            Flush the list of allowed channel patterns.
    #  ><password>  Add this password to the list of valid password for the user.
    #               For example >mypass will add "mypass" to the list.
    #               This directive clears the "nopass" flag (see later).
    #  <<password>  Remove this password from the list of valid passwords.
    #  nopass       All the set passwords of the user are removed, and the user
    #               is flagged as requiring no password: it means that every
    #               password will work against this user. If this directive is
    #               used for the default user, every new connection will be
    #               immediately authenticated with the default user without
    #               any explicit AUTH command required. Note that the "resetpass"
    #               directive will clear this condition.
    #  resetpass    Flush the list of allowed passwords. Moreover removes the
    #               "nopass" status. After "resetpass" the user has no associated
    #               passwords and there is no way to authenticate without adding
    #               some password (or setting it as "nopass" later).
    #  reset        Performs the following actions: resetpass, resetkeys, resetchannels,
    #               allchannels (if acl-pubsub-default is set), off, clearselectors, -@all.
    #               The user returns to the same state it has immediately after its creation.
    # (<options>)   Create a new selector with the options specified within the
    #               parentheses and attach it to the user. Each option should be 
    #               space separated. The first character must be ( and the last 
    #               character must be ).
    # clearselectors            Remove all of the currently attached selectors. 
    #                           Note this does not change the "root" user permissions,
    #                           which are the permissions directly applied onto the
    #                           user (outside the parentheses).
    #
    # ACL rules can be specified in any order: for instance you can start with
    # passwords, then flags, or key patterns. However note that the additive
    # and subtractive rules will CHANGE MEANING depending on the ordering.
    # For instance see the following example:
    #
    #   user alice on +@all -DEBUG ~* >somepassword
    #
    # This will allow "alice" to use all the commands with the exception of the
    # DEBUG command, since +@all added all the commands to the set of the commands
    # alice can use, and later DEBUG was removed. However if we invert the order
    # of two ACL rules the result will be different:
    #
    #   user alice on -DEBUG +@all ~* >somepassword
    #
    # Now DEBUG was removed when alice had yet no commands in the set of allowed
    # commands, later all the commands are added, so the user will be able to
    # execute everything.
    #
    # Basically ACL rules are processed left-to-right.
    #
    # The following is a list of command categories and their meanings:
    # * keyspace - Writing or reading from keys, databases, or their metadata 
    #     in a type agnostic way. Includes DEL, RESTORE, DUMP, RENAME, EXISTS, DBSIZE,
    #     KEYS, EXPIRE, TTL, FLUSHALL, etc. Commands that may modify the keyspace,
    #     key or metadata will also have `write` category. Commands that only read
    #     the keyspace, key or metadata will have the `read` category.
    # * read - Reading from keys (values or metadata). Note that commands that don't
    #     interact with keys, will not have either `read` or `write`.
    # * write - Writing to keys (values or metadata)
    # * admin - Administrative commands. Normal applications will never need to use
    #     these. Includes REPLICAOF, CONFIG, DEBUG, SAVE, MONITOR, ACL, SHUTDOWN, etc.
    # * dangerous - Potentially dangerous (each should be considered with care for
    #     various reasons). This includes FLUSHALL, MIGRATE, RESTORE, SORT, KEYS,
    #     CLIENT, DEBUG, INFO, CONFIG, SAVE, REPLICAOF, etc.
    # * connection - Commands affecting the connection or other connections.
    #     This includes AUTH, SELECT, COMMAND, CLIENT, ECHO, PING, etc.
    # * blocking - Potentially blocking the connection until released by another
    #     command.
    # * fast - Fast O(1) commands. May loop on the number of arguments, but not the
    #     number of elements in the key.
    # * slow - All commands that are not Fast.
    # * pubsub - PUBLISH / SUBSCRIBE related
    # * transaction - WATCH / MULTI / EXEC related commands.
    # * scripting - Scripting related.
    # * set - Data type: sets related.
    # * sortedset - Data type: zsets related.
    # * list - Data type: lists related.
    # * hash - Data type: hashes related.
    # * string - Data type: strings related.
    # * bitmap - Data type: bitmaps related.
    # * hyperloglog - Data type: hyperloglog related.
    # * geo - Data type: geo related.
    # * stream - Data type: streams related.
    #
    # For more information about ACL configuration please refer to
    # the Redis web site at https://redis.io/topics/acl
    
    # ACL LOG
    #
    # The ACL Log tracks failed commands and authentication events associated
    # with ACLs. The ACL Log is useful to troubleshoot failed commands blocked
    # by ACLs. The ACL Log is stored in memory. You can reclaim memory with
    # ACL LOG RESET. Define the maximum entry length of the ACL Log below.
    acllog-max-len 128
    
    # Using an external ACL file
    #
    # Instead of configuring users here in this file, it is possible to use
    # a stand-alone file just listing users. The two methods cannot be mixed:
    # if you configure users here and at the same time you activate the external
    # ACL file, the server will refuse to start.
    #
    # The format of the external ACL user file is exactly the same as the
    # format that is used inside redis.conf to describe users.
    #
    # aclfile /etc/redis/users.acl
    
    # IMPORTANT NOTE: starting with Redis 6 "requirepass" is just a compatibility
    # layer on top of the new ACL system. The option effect will be just setting
    # the password for the default user. Clients will still authenticate using
    # AUTH <password> as usually, or more explicitly with AUTH default <password>
    # if they follow the new protocol: both will work.
    #
    # The requirepass is not compatible with aclfile option and the ACL LOAD
    # command, these will cause requirepass to be ignored.
    #
    # requirepass foobared
    
    # New users are initialized with restrictive permissions by default, via the
    # equivalent of this ACL rule 'off resetkeys -@all'. Starting with Redis 6.2, it
    # is possible to manage access to Pub/Sub channels with ACL rules as well. The
    # default Pub/Sub channels permission if new users is controlled by the
    # acl-pubsub-default configuration directive, which accepts one of these values:
    #
    # allchannels: grants access to all Pub/Sub channels
    # resetchannels: revokes access to all Pub/Sub channels
    #
    # From Redis 7.0, acl-pubsub-default defaults to 'resetchannels' permission.
    #
    # acl-pubsub-default resetchannels
    
    # Command renaming (DEPRECATED).
    #
    # ------------------------------------------------------------------------
    # WARNING: avoid using this option if possible. Instead use ACLs to remove
    # commands from the default user, and put them only in some admin user you
    # create for administrative purposes.
    # ------------------------------------------------------------------------
    #
    # It is possible to change the name of dangerous commands in a shared
    # environment. For instance the CONFIG command may be renamed into something
    # hard to guess so that it will still be available for internal-use tools
    # but not available for general clients.
    #
    # Example:
    #
    # rename-command CONFIG b840fc02d524045429941cc15f59e41cb7be6c52
    #
    # It is also possible to completely kill a command by renaming it into
    # an empty string:
    #
    # rename-command CONFIG ""
    #
    # Please note that changing the name of commands that are logged into the
    # AOF file or transmitted to replicas may cause problems.
    ```

2. 注释译文

    ```conf
    ################################## 安全 ###################################
    
    # 警告：Redis 速度非常快，在现代计算机上，外部用户每秒最多可尝试约 100 万个密码。
    # 因此必须使用非常强的密码，否则很容易被破解。密码实际上只是客户端与服务器共享的
    # 密钥，不需要任何人记忆，所以完全可以使用来自 /dev/urandom 等来源的长随机字符串。
    # 只要密码足够长且无法猜测，暴力破解就不可行。
    
    # Redis ACL 用户使用以下格式定义：
    #
    #   user <用户名> ... ACL 规则 ...
    #
    # 例如：
    #
    #   user worker +@list +@connection ~jobs:* on >ffa9203c493aa99
    #
    # 特殊用户名 default 用于新连接。如果该用户带有 nopass 规则，新连接会立即以 default
    # 用户完成认证，不需要通过 AUTH 提供密码。如果 default 用户没有 nopass 标志，
    # 新连接会处于未认证状态，必须使用 AUTH（或 HELLO 命令的 AUTH 选项）完成认证后才能工作。
    #
    # 描述用户能力的 ACL 规则如下：
    #
    #  on           启用用户，可以用该用户身份认证。
    #  off          禁用用户，不能再以该用户认证；但已经认证的现有连接仍可继续工作。
    #  skip-sanitize-payload    跳过对 RESTORE dump 载荷的安全检查。
    #  sanitize-payload         对 RESTORE dump 载荷执行安全检查（默认值）。
    #  +<命令>      允许执行该命令。
    #               可用 | 允许子命令，例如 +config|get。
    #  -<命令>      禁止执行该命令。
    #               可用 | 禁止子命令，例如 -config|set。
    #  +@<分类>     允许执行该分类中的所有命令。有效分类包括 @admin、@set、@sortedset 等。
    #               完整列表见 server.c 中 Redis 命令表的描述和定义。
    #               特殊分类 @all 表示服务器当前已有的所有命令，以及以后通过模块加载的命令。
    #  +<命令>|首个参数
    #               对原本被禁用的命令，只允许一个指定的首参数。仅支持没有子命令的命令，
    #               不允许 -SELECT|1 这种负向形式，只允许以 + 开头的增加形式。
    #               此功能已经废弃，将来可能删除。
    #  allcommands  +@all 的别名。注意它也允许执行将来通过模块系统加载的所有命令。
    #  nocommands   -@all 的别名。
    #  ~<模式>      增加命令允许涉及的键模式。例如 ~* 允许所有键。
    #               模式采用与 KEYS 相同的 glob 风格，可以指定多个模式。
    #  %R~<模式>    增加键读取模式，规定允许读取哪些键。
    #  %W~<模式>    增加键写入模式，规定允许写入哪些键。
    #  allkeys      ~* 的别名。
    #  resetkeys    清空允许的键模式列表。
    #  &<模式>      增加用户可访问的 Pub/Sub 频道 glob 模式，可以指定多个频道模式。
    #  allchannels  &* 的别名。
    #  resetchannels
    #               清空允许的频道模式列表。
    #  ><密码>      把密码加入该用户的有效密码列表。例如 >mypass 会加入 mypass。
    #               此指令会清除 nopass 标志。
    #  <<密码>      从有效密码列表中删除该密码。
    #  nopass       删除用户已经设置的所有密码，并标记为不需要密码，也就是任意密码都可通过。
    #               如果对 default 用户使用，新连接会立刻以默认用户认证，不需要显式 AUTH。
    #               resetpass 会清除这种状态。
    #  resetpass    清空允许的密码列表，并移除 nopass 状态。执行后用户没有关联密码；
    #               必须添加密码或以后重新设置 nopass，否则无法认证。
    #  reset        依次执行 resetpass、resetkeys、resetchannels、allchannels
    #               （如果设置了 acl-pubsub-default）、off、clearselectors、-@all。
    #               用户恢复到刚创建时的状态。
    #  (<选项>)     用括号内指定的选项创建新选择器并关联到用户。各选项用空格分隔；
    #               第一个字符必须是 (，最后一个字符必须是 )。
    #  clearselectors
    #               删除当前关联的所有选择器。注意这不会改变用户的“根”权限，
    #               即直接应用到用户、写在括号外的权限。
    #
    # ACL 规则可以按任意顺序书写，例如先写密码，再写标志或键模式。
    # 但增加和删除规则的含义会随顺序改变。例如：
    #
    #   user alice on +@all -DEBUG ~* >somepassword
    #
    # 这允许 alice 使用除 DEBUG 外的所有命令，因为 +@all 先把所有命令加入允许集合，
    # 后面的 -DEBUG 再把 DEBUG 删除。但交换两条 ACL 规则后结果不同：
    #
    #   user alice on -DEBUG +@all ~* >somepassword
    #
    # 此时删除 DEBUG 时，alice 的允许集合中还没有任何命令；之后 +@all 又加入全部命令，
    # 因此用户最终可以执行所有命令。
    #
    # 总之，ACL 规则从左到右处理。
    #
    # 以下是命令分类及其含义：
    # * keyspace - 以不区分数据类型的方式读取或写入键、数据库或其元数据。包括 DEL、
    #     RESTORE、DUMP、RENAME、EXISTS、DBSIZE、KEYS、EXPIRE、TTL、FLUSHALL 等。
    #     可能修改键空间、键或元数据的命令也属于 write；只读取这些内容的命令也属于 read。
    # * read - 读取键的值或元数据。不与键交互的命令既不属于 read，也不属于 write。
    # * write - 写入键的值或元数据。
    # * admin - 管理命令，普通应用永远不需要使用。包括 REPLICAOF、CONFIG、DEBUG、SAVE、
    #     MONITOR、ACL、SHUTDOWN 等。
    # * dangerous - 潜在危险命令，各自都应根据不同原因谨慎考虑。包括 FLUSHALL、MIGRATE、
    #     RESTORE、SORT、KEYS、CLIENT、DEBUG、INFO、CONFIG、SAVE、REPLICAOF 等。
    # * connection - 影响当前或其他连接的命令，包括 AUTH、SELECT、COMMAND、CLIENT、
    #     ECHO、PING 等。
    # * blocking - 可能一直阻塞连接，直到被另一条命令解除。
    # * fast - 时间复杂度 O(1) 的快速命令。可以按参数数量循环，但不会按键中元素数量循环。
    # * slow - 所有不属于 fast 的命令。
    # * pubsub - PUBLISH/SUBSCRIBE 相关命令。
    # * transaction - WATCH/MULTI/EXEC 相关命令。
    # * scripting - 脚本相关命令。
    # * set - Set 数据类型相关命令。
    # * sortedset - Sorted Set（ZSet）数据类型相关命令。
    # * list - List 数据类型相关命令。
    # * hash - Hash 数据类型相关命令。
    # * string - String 数据类型相关命令。
    # * bitmap - Bitmap 数据类型相关命令。
    # * hyperloglog - HyperLogLog 数据类型相关命令。
    # * geo - 地理位置数据类型相关命令。
    # * stream - Stream 数据类型相关命令。
    #
    # 更多 ACL 配置说明请参阅 Redis 网站：https://redis.io/topics/acl
    
    # ACL 日志
    #
    # ACL 日志记录与 ACL 有关的失败命令和认证事件，可用于排查被 ACL 阻止的命令。
    # ACL 日志保存在内存中，可以执行 ACL LOG RESET 回收内存。
    # 下面定义 ACL 日志最多保留的条目数量。
    acllog-max-len 128
    
    # 使用外部 ACL 文件
    #
    # 除了在本配置文件中定义用户，也可以使用一份只列出用户的独立文件。
    # 两种方式不能混用：如果既在这里配置用户，又启用外部 ACL 文件，服务器会拒绝启动。
    #
    # 外部 ACL 用户文件描述用户的格式，与 redis.conf 内使用的格式完全相同。
    #
    # aclfile /etc/redis/users.acl
    
    # 重要说明：从 Redis 6 开始，requirepass 只是新 ACL 系统之上的兼容层。
    # 它的效果只是为 default 用户设置密码。客户端仍可像以前一样使用 AUTH <密码> 认证，
    # 也可以按新协议明确使用 AUTH default <密码>，两种方式都有效。
    #
    # requirepass 与 aclfile 选项及 ACL LOAD 命令不兼容；使用后两者会使 requirepass 被忽略。
    #
    # requirepass foobared
    
    # 新用户默认以相当于 off resetkeys -@all 的 ACL 规则初始化，权限非常严格。
    # 从 Redis 6.2 开始，还可以用 ACL 规则管理 Pub/Sub 频道访问权限。
    # 新用户默认的 Pub/Sub 频道权限由 acl-pubsub-default 控制，可选值：
    #
    # allchannels  - 授予所有 Pub/Sub 频道访问权
    # resetchannels - 撤销所有 Pub/Sub 频道访问权
    #
    # 从 Redis 7.0 开始，acl-pubsub-default 默认为 resetchannels。
    #
    # acl-pubsub-default resetchannels
    
    # 命令重命名（已废弃）。
    #
    # ------------------------------------------------------------------------
    # 警告：尽量不要使用此选项。应优先使用 ACL 从 default 用户移除命令，
    # 再只把这些命令授予专门创建的管理员用户。
    # ------------------------------------------------------------------------
    #
    # 在共享环境中可以修改危险命令的名称。例如把 CONFIG 改成难以猜测的名称，
    # 使内部工具仍可使用，但普通客户端无法使用。
    #
    # 示例：
    #
    # rename-command CONFIG b840fc02d524045429941cc15f59e41cb7be6c52
    #
    # 也可以把命令重命名为空字符串，从而彻底禁用：
    #
    # rename-command CONFIG ""
    #
    # 注意：修改会被记录到 AOF 或传输给副本的命令名称，可能引发问题。
    ```

#### 属性解析

1. Redis 6 以后优先使用 ACL

    `requirepass` 仍可使用，但本质上只是给 `default` 用户设置密码的兼容层。ACL 可以同时限制用户、密码、命令类别、键名模式和 Pub/Sub 频道，权限边界更清楚。

2. `user` 基本格式

    ```conf
    user <用户名> <开关> <密码规则> <键规则> <命令规则>
    ```

    示例：

    ```conf
    user app on >请替换为高强度随机密码 ~app:* +@read +@write +@connection -@dangerous
    ```

    含义：

    - `on`：启用用户。
    - `>密码`：添加一个明文密码；Redis 实际保存为哈希。
    - `~app:*`：只允许访问 `app:` 前缀的键。
    - `+@read +@write`：允许读写类别命令。
    - `+@connection`：允许 `PING`、`SELECT` 等连接类命令。
    - `-@dangerous`：移除危险命令。ACL 从左向右处理，因此删除规则应放在授权规则后面。

3. `default` 用户

    - 新连接默认对应 `default` 用户。
    - `nopass` 表示无需密码，甚至任意密码都能通过；对外提供服务时不要保留这种状态。
    - 如果只允许专用业务用户，可以设置 `user default off`。

    通常情况下，登录示例如下
    ```powershell
    # 登录默认用户default
    auth <secret>

    # 登录自定义用户aaa，密码bbb
    auth aaa bbb
    ```

4. 密码强度

    - Redis 验证速度很快，短密码容易被暴力破解。
    - 应使用足够长的随机字符串，不要使用姓名、手机号或常见单词。
    - 不要把真实生产密码提交到 Git；可以把 ACL 文件作为 Docker Secret、Kubernetes Secret 或受控只读挂载提供。

5. `aclfile`

    - 把 ACL 用户规则拆到独立文件，便于权限管理和避免主配置中出现密钥。
    - 不能同时在 `redis.conf` 中定义用户并启用外部 ACL 文件，否则 Redis 会拒绝启动。

    ```conf
    aclfile /etc/redis/users.acl
    ```

6. `acllog-max-len`

    - ACL 日志记录认证失败和被权限拒绝的命令，便于排错和审计。
    - 日志保存在内存中；`ACL LOG RESET` 可以清空并回收内存。

    ```conf
    acllog-max-len 128
    ```

7. `acl-pubsub-default`

    - Redis 7 默认 `resetchannels`，新用户不能自动访问任何 Pub/Sub 频道。
    - 需要频道权限时显式使用 `&频道模式` 或 `allchannels`，不要无意间开放全部频道。

8. `rename-command`

    - 已废弃，不再推荐用“把危险命令改成乱码”的方式做安全控制。
    - 应使用 ACL 从业务用户移除 `CONFIG`、`DEBUG`、`FLUSHALL` 等命令。
    - 修改会写入 AOF 或发送给副本的命令名称还可能导致兼容问题。

9. 最低安全组合

    - 只监听需要的网络接口。
    - 保持 `protected-mode yes`。
    - 配置 ACL 或至少设置高强度密码。
    - 防火墙只允许可信来源。
    - 跨不可信网络传输时启用 TLS。
    - 不要把“修改端口”当作认证或访问控制。

#### 设置密码的方式

1. 在conf中设置

2. 在命令行`redis-cli`设置密码
    - 不在配置文件中设置，只是临时生效，用户密码将会在重启后丢失
    ```powershell
    127.0.0.1:6379> config set requirepass abc
    OK
    127.0.0.1:6379> auth abc
    OK
    127.0.0.1:6379>
    ```

### `#LIMITS#` 限制

> Redis 7.2.16 官方模板已经没有单独的 `LIMITS` 标题。这里为了延续笔记结构，把官方的 `CLIENTS` 与 `MEMORY MANAGEMENT` 合并为“限制”章节。

#### 配置块

1. 官方配置

    ```conf
    ################################### CLIENTS ####################################
    
    # Set the max number of connected clients at the same time. By default
    # this limit is set to 10000 clients, however if the Redis server is not
    # able to configure the process file limit to allow for the specified limit
    # the max number of allowed clients is set to the current file limit
    # minus 32 (as Redis reserves a few file descriptors for internal uses).
    #
    # Once the limit is reached Redis will close all the new connections sending
    # an error 'max number of clients reached'.
    #
    # IMPORTANT: When Redis Cluster is used, the max number of connections is also
    # shared with the cluster bus: every node in the cluster will use two
    # connections, one incoming and another outgoing. It is important to size the
    # limit accordingly in case of very large clusters.
    #
    # maxclients 10000
    
    ############################## MEMORY MANAGEMENT ################################
    
    # Set a memory usage limit to the specified amount of bytes.
    # When the memory limit is reached Redis will try to remove keys
    # according to the eviction policy selected (see maxmemory-policy).
    #
    # If Redis can't remove keys according to the policy, or if the policy is
    # set to 'noeviction', Redis will start to reply with errors to commands
    # that would use more memory, like SET, LPUSH, and so on, and will continue
    # to reply to read-only commands like GET.
    #
    # This option is usually useful when using Redis as an LRU or LFU cache, or to
    # set a hard memory limit for an instance (using the 'noeviction' policy).
    #
    # WARNING: If you have replicas attached to an instance with maxmemory on,
    # the size of the output buffers needed to feed the replicas are subtracted
    # from the used memory count, so that network problems / resyncs will
    # not trigger a loop where keys are evicted, and in turn the output
    # buffer of replicas is full with DELs of keys evicted triggering the deletion
    # of more keys, and so forth until the database is completely emptied.
    #
    # In short... if you have replicas attached it is suggested that you set a lower
    # limit for maxmemory so that there is some free RAM on the system for replica
    # output buffers (but this is not needed if the policy is 'noeviction').
    #
    # maxmemory <bytes>
    
    # MAXMEMORY POLICY: how Redis will select what to remove when maxmemory
    # is reached. You can select one from the following behaviors:
    #
    # volatile-lru -> Evict using approximated LRU, only keys with an expire set.
    # allkeys-lru -> Evict any key using approximated LRU.
    # volatile-lfu -> Evict using approximated LFU, only keys with an expire set.
    # allkeys-lfu -> Evict any key using approximated LFU.
    # volatile-random -> Remove a random key having an expire set.
    # allkeys-random -> Remove a random key, any key.
    # volatile-ttl -> Remove the key with the nearest expire time (minor TTL)
    # noeviction -> Don't evict anything, just return an error on write operations.
    #
    # LRU means Least Recently Used
    # LFU means Least Frequently Used
    #
    # Both LRU, LFU and volatile-ttl are implemented using approximated
    # randomized algorithms.
    #
    # Note: with any of the above policies, when there are no suitable keys for
    # eviction, Redis will return an error on write operations that require
    # more memory. These are usually commands that create new keys, add data or
    # modify existing keys. A few examples are: SET, INCR, HSET, LPUSH, SUNIONSTORE,
    # SORT (due to the STORE argument), and EXEC (if the transaction includes any
    # command that requires memory).
    #
    # The default is:
    #
    # maxmemory-policy noeviction
    
    # LRU, LFU and minimal TTL algorithms are not precise algorithms but approximated
    # algorithms (in order to save memory), so you can tune it for speed or
    # accuracy. By default Redis will check five keys and pick the one that was
    # used least recently, you can change the sample size using the following
    # configuration directive.
    #
    # The default of 5 produces good enough results. 10 Approximates very closely
    # true LRU but costs more CPU. 3 is faster but not very accurate.
    #
    # maxmemory-samples 5
    
    # Eviction processing is designed to function well with the default setting.
    # If there is an unusually large amount of write traffic, this value may need to
    # be increased.  Decreasing this value may reduce latency at the risk of
    # eviction processing effectiveness
    #   0 = minimum latency, 10 = default, 100 = process without regard to latency
    #
    # maxmemory-eviction-tenacity 10
    
    # Starting from Redis 5, by default a replica will ignore its maxmemory setting
    # (unless it is promoted to master after a failover or manually). It means
    # that the eviction of keys will be just handled by the master, sending the
    # DEL commands to the replica as keys evict in the master side.
    #
    # This behavior ensures that masters and replicas stay consistent, and is usually
    # what you want, however if your replica is writable, or you want the replica
    # to have a different memory setting, and you are sure all the writes performed
    # to the replica are idempotent, then you may change this default (but be sure
    # to understand what you are doing).
    #
    # Note that since the replica by default does not evict, it may end using more
    # memory than the one set via maxmemory (there are certain buffers that may
    # be larger on the replica, or data structures may sometimes take more memory
    # and so forth). So make sure you monitor your replicas and make sure they
    # have enough memory to never hit a real out-of-memory condition before the
    # master hits the configured maxmemory setting.
    #
    # replica-ignore-maxmemory yes
    
    # Redis reclaims expired keys in two ways: upon access when those keys are
    # found to be expired, and also in background, in what is called the
    # "active expire key". The key space is slowly and interactively scanned
    # looking for expired keys to reclaim, so that it is possible to free memory
    # of keys that are expired and will never be accessed again in a short time.
    #
    # The default effort of the expire cycle will try to avoid having more than
    # ten percent of expired keys still in memory, and will try to avoid consuming
    # more than 25% of total memory and to add latency to the system. However
    # it is possible to increase the expire "effort" that is normally set to
    # "1", to a greater value, up to the value "10". At its maximum value the
    # system will use more CPU, longer cycles (and technically may introduce
    # more latency), and will tolerate less already expired keys still present
    # in the system. It's a tradeoff between memory, CPU and latency.
    #
    # active-expire-effort 1
    ```

2. 注释译文

    ```conf
    ################################### 客户端 ####################################
    
    # 设置允许同时连接的最大客户端数量，默认上限为 10000。如果 Redis 服务器无法把进程
    # 文件描述符限制调高到满足该值，允许的最大客户端数会改为当前文件限制减 32，
    # 因为 Redis 要为内部用途保留一些文件描述符。
    #
    # 到达限制后，Redis 会关闭所有新连接，并发送错误 max number of clients reached。
    #
    # 重要：使用 Redis Cluster 时，最大连接数也与集群总线共享。集群中每个节点会使用
    # 两条连接，一条入站、一条出站。对于非常大的集群，必须据此合理设置上限。
    #
    # maxclients 10000
    
    ############################## 内存管理 ################################
    
    # 把内存使用上限设为指定字节数。
    # 到达内存上限时，Redis 会按照所选淘汰策略（见 maxmemory-policy）尝试删除键。
    #
    # 如果 Redis 无法按策略删除键，或策略为 noeviction，Redis 会对 SET、LPUSH 等需要
    # 更多内存的命令返回错误，但仍会继续响应 GET 等只读命令。
    #
    # 把 Redis 用作 LRU/LFU 缓存，或者使用 noeviction 策略为实例设置硬性内存上限时，
    # 此选项通常非常有用。
    #
    # 警告：如果设置 maxmemory 的实例连接了副本，用于向副本发送数据的输出缓冲区大小
    # 会从已用内存统计中扣除。这样可避免网络故障或重新同步触发恶性循环：键被淘汰，
    # 副本输出缓冲区又被大量 DEL 填满，进一步触发更多键被删除，直到数据库完全清空。
    #
    # 简单说，如果连接了副本，建议把 maxmemory 设得低一些，为副本输出缓冲区保留系统
    # 空闲内存；但使用 noeviction 策略时不需要这样做。
    #
    # maxmemory <字节数>
    
    # 最大内存淘汰策略：到达 maxmemory 时，Redis 如何选择要删除的内容。
    # 可以选择以下行为之一：
    #
    # volatile-lru    -> 使用近似 LRU，只淘汰设置了过期时间的键。
    # allkeys-lru     -> 使用近似 LRU，任意键都可淘汰。
    # volatile-lfu    -> 使用近似 LFU，只淘汰设置了过期时间的键。
    # allkeys-lfu     -> 使用近似 LFU，任意键都可淘汰。
    # volatile-random -> 随机删除一个设置了过期时间的键。
    # allkeys-random  -> 随机删除任意键。
    # volatile-ttl    -> 删除最接近过期的键，也就是 TTL 最小的键。
    # noeviction      -> 不淘汰任何键，需要更多内存的写操作直接返回错误。
    #
    # LRU 表示“最近最少使用”。
    # LFU 表示“最不经常使用”。
    #
    # LRU、LFU 和 volatile-ttl 都使用近似的随机算法实现。
    #
    # 注意：无论使用上述哪种策略，如果找不到符合条件的可淘汰键，需要更多内存的写操作
    # 都会返回错误。这类命令通常会创建新键、增加数据或修改已有键，例如 SET、INCR、
    # HSET、LPUSH、SUNIONSTORE、带 STORE 参数的 SORT，以及事务中包含需要内存命令的 EXEC。
    #
    # 默认值：
    #
    # maxmemory-policy noeviction
    
    # 为了节省内存，LRU、LFU 和最小 TTL 使用的都不是精确算法，而是近似算法，因此可以
    # 在速度和准确性之间调节。Redis 默认检查 5 个键并选择最近最少使用的那个；
    # 可以使用下面的配置修改样本数量。
    #
    # 默认值 5 的结果已经足够好。10 非常接近真实 LRU，但会消耗更多 CPU；3 更快但不太准确。
    #
    # maxmemory-samples 5
    
    # 淘汰处理按默认设置设计即可良好工作。如果写流量异常大，可能需要提高此值。
    # 降低此值可以减少延迟，但会牺牲淘汰处理效果。
    #   0 = 最低延迟，10 = 默认值，100 = 完全不考虑延迟地处理
    #
    # maxmemory-eviction-tenacity 10
    
    # 从 Redis 5 开始，副本默认忽略自身 maxmemory 设置，除非发生故障转移或手动操作后
    # 被提升为主节点。这意味着键淘汰只由主节点处理；主节点淘汰键时，会向副本发送 DEL。
    #
    # 此行为确保主节点和副本保持一致，通常正是你所需要的。但如果副本可写，或者希望副本
    # 使用不同内存设置，并且能确认副本上的所有写入都是幂等的，可以修改默认行为；
    # 不过务必先理解这样做的影响。
    #
    # 注意：副本默认不淘汰键，因此实际使用内存可能超过 maxmemory。副本上的某些缓冲区
    # 可能更大，数据结构有时也会占用更多内存。务必监控副本并保证内存充足，避免主节点
    # 到达已配置 maxmemory 之前，副本先发生真正的内存不足。
    #
    # replica-ignore-maxmemory yes
    
    # Redis 通过两种方式回收过期键：访问键时发现它已过期便回收；以及在后台执行所谓的
    #“主动过期键”处理。Redis 会缓慢、交替地扫描键空间寻找过期键，从而尽快释放那些
    # 已经过期且可能永远不会再被访问的键所占内存。
    #
    # 默认的过期周期力度会尽量让内存中残留的过期键不超过 10%，同时尽量避免消耗超过
    # 25% 的总处理时间或给系统增加延迟。通常设为 1 的过期“力度”可以提高，最大为 10。
    # 值越高，系统使用更多 CPU、执行更长周期（技术上可能增加延迟），但容忍残留的过期键
    # 更少。这是在内存、CPU 和延迟之间的权衡。
    #
    # active-expire-effort 1
    ```

#### 属性解析

1. `maxclients`

    - 同时连接的客户端数量上限，默认 10000。
    - 实际上限还受操作系统文件描述符数量影响；Redis 会为内部用途保留一部分描述符。
    - 达到上限后，新连接收到 `max number of clients reached`。
    - Cluster 环境还要为节点间集群总线连接留出数量。

    ```conf
    maxclients 10000
    ```

2. `maxmemory`

    - Redis 可使用的内存上限；`0` 表示不设置应用层上限。
    - Docker 设置了容器内存限制时，Redis 的 `maxmemory` 应低于容器上限，为连接缓冲区、复制缓冲区、Jemalloc 碎片以及 fork 写时复制留出空间。
    - 不要把 `maxmemory` 恰好设成容器内存上限，否则 Redis 可能先被 OOM Killer 终止。

    ```conf
    maxmemory 512mb
    ```

3. `maxmemory-policy`
    内存爆了之后的淘汰策略

    - `noeviction`：不淘汰，写命令内存不足时报错；适合把 Redis 当数据库使用。
    - `allkeys-lru`：从所有键中近似淘汰最近最少使用的键；常用于通用缓存。
    - `allkeys-lfu`：从所有键中近似淘汰最不常使用的键；热点分布明显时常用。
    - `volatile-lru` / `volatile-lfu`：只淘汰设置了过期时间的键。
    - `volatile-ttl`：优先淘汰最接近过期的键。
    - `allkeys-random` / `volatile-random`：随机淘汰。

    | 分类维度 | 策略前缀 | 含义 |
    | :--- | :--- | :--- |
    | **淘汰范围** | `allkeys-*` | 枪口对准**所有键**（无论是否设置了过期时间）。 |
    | | `volatile-*` | 枪口只对准**设置了过期时间（TTL）的键**。如果没有符合条件的键可删，则行为回退为 `noeviction`（报错）。 |
    | **淘汰算法** | `-lru` | 淘汰最近最少使用（按**访问时间**）。 |
    | | `-lfu` | 淘汰最不常使用（按**访问频率**）。 |
    | | `-ttl` | 淘汰剩余存活时间最短的（按**过期时间**）。 |
    | | `-random` | 随机抽选淘汰。 |

    LRU（Least Recently Used）	LFU（Least Frequently Used）
    ```conf
    maxmemory-policy noeviction
    ```

    如果使用缓存且允许任意键被淘汰，常见组合是：

    ```conf
    maxmemory 512mb
    maxmemory-policy allkeys-lru
    ```

4. `maxmemory-samples`

    - LRU、LFU 采用近似算法，不会扫描全部键。
    - 默认采样 5 个键，性能和准确性较均衡；提高到 10 更准确但更消耗 CPU。

5. `maxmemory-eviction-tenacity`

    - 控制 Redis 为及时完成淘汰愿意投入多少处理力度。
    - 默认 10；降低可减少延迟但可能让淘汰跟不上，提高则会更积极地处理淘汰。

6. `replica-ignore-maxmemory`

    - 副本默认忽略自己的 `maxmemory`，由主节点决定淘汰并把 DEL 传播给副本，以保持一致。
    - 因此副本实际内存可能超过主节点设置的 `maxmemory`，必须单独监控副本内存。

7. `active-expire-effort`

    - 控制后台主动清理过期键的力度，范围 1 到 10。
    - 值越高，残留过期键越少，但消耗更多 CPU，并可能增加延迟。
    - 默认 1 通常足够；确认存在大量过期键滞留后再调整。


## 发布与订阅

### 基本介绍

1. 发布和订阅是什么
    Redis 发布/订阅 (pub/sub) 是一种消息通信模式：发送者 (pub) 发送消息，订阅者 (sub) 接收消息

2. Redis 客户端可以订阅任意数量的频道

3. 示意图
    ```text
    Publisher      Channel       Subscriber
    发布者  ─────→ 频道 ─────→ 订阅者
                        ├────→ 订阅者
                        └────→ 订阅者
    ```

4. 示例
    ```text
    Publisher
        |
        | PUBLISH news "Redis 8 发布了"
        v
    +------------+
    |    news    |       Channel
    +------------+
    |      |
    v      v
    客户端A   客户端B
    ```

1. 如何理解发布和订阅模式
    - 一条消息发出去，所有订阅者都能收到一模一样的副本（人手一份）
    - 与任务队列进行交互的实体有两类，一类是生产者（producer），另一类则是消费者（consumer）。
    - 生产者将需要处理的任务放入任务队列中，而消费者则不断地从任务队列中读入任务信息并执行


### 发布订阅模式分类

1. 一个发布者，多个订阅者

    1. 实现场景
        - 通知
        - 公告

        - 作为消息队列或者管道

1. 多个发布者，一个订阅者

    1. 死现场型
        - 排行榜
        - 投票
        - 计数
        - 统计

    2. 各应用程序作为 Publisher 向 Channel 中发送消息，Subscriber 端收到消息后执行相应的业务逻辑，比如写数据库，显示..

1. 多个发布者，多个订阅者

    1. 实现场景
        - 群聊

### 命令行实现发布与订阅


1. `PUBLISH channel msg`：将信息 message 发送到指定的频道 channel

2. `SUBSCRIBE channel [channel ...]`：订阅频道，可以同时订阅多个频道

3. `UNSUBSCRIBE [channel ...]`：取消订阅指定的频道, 如果不指定频道，则会取消订阅所有频道

4. `PSUBSCRIBE pattern [pattern ...]`：订阅一个或多个符合给定模式的频道，每个模式以 `*` 作为匹配符，比如 `it*` 匹配所 有以 `it` 开头的频道( `it.news` 、 `it.blog` 、 `it.tweets` 等等)， `news.*` 匹配所有 以 `news.`开头的频道( `news.it` 、 `news.global.today` 等等)，诸如此类

5. `PUNSUBSCRIBE [pattern [pattern ...]]`：退订指定的规则, 如果没有参数则会退订所有规则


6. 快速入门
    - 打开一个客户端订阅 channel1
        ```powershell
        subscribe channel1
        ```
    - 打开另一个客户端，给 channel1 发布消息 hello，返回订阅者数量。
        ```powershell
        publish channel1 hello
        ```
    - 发布的消息没有持久化
    - 订阅的客户端, 只能收到订阅后发布的消息



## Jedis

### 简介

1. 文档
    - 中文
    - 英文：https://javadoc.io/doc/redis.clients/jedis/latest/index.html

2. 介绍


### 快速入门

1. 搭建Maven项目

    ```xml
    <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
        <version>7.2.0</version>
    </dependency>
    ```

2. 测试

    ```java
    package com.lcq;

    import org.junit.jupiter.api.Test;
    import redis.clients.jedis.Jedis;

    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import java.util.Set;

    /**
     * @author lcq
     * @version 1.0
     */
    public class JedisTest {

        @Test
        public void conn() {
            Jedis jedis = new Jedis("localhost", 6379);

            jedis.auth("abc");

            //"PONG"
            System.out.println(jedis.ping());
            jedis.close();

        }

        @Test
        public void key() {
            Jedis jedis = new Jedis("localhost", 6379);
            jedis.auth("abc");
            jedis.set("k1", "v1");
            jedis.set("k2", "v2");
            jedis.set("k3", "v3");
            Set<String> keys = jedis.keys("*");

            /*
            7

            k1
            k2
            k3
            k4
            lcq
            listkey2
            listkey1

            true
            -1
            v1
            */

            System.out.println(keys.size());
            for (String key : keys) {
                System.out.println(key);
            }
            System.out.println(jedis.exists("k1"));
            System.out.println(jedis.ttl("k1"));
            System.out.println(jedis.get("k1"));
        }

        @Test
        public void string() {
            Jedis jedis = new Jedis("localhost", 6379);
            jedis.auth("abc");
            jedis.mset("k1", "v1", "k2", "v2", "k3", "v3");
            System.out.println(jedis.mget("k1", "k2", "k3"));
        }

        @Test
        public void list() {
            Jedis jedis = new Jedis("localhost", 6379);
            jedis.auth("abc");
            jedis.lpush("nameList", "jack", "tom", "mary");
            List<String> list = jedis.lrange("nameList", 0, -1);
            for (String element : list) {
                System.out.println(element);
            }
        }

        @Test
        public void set() {
            Jedis jedis = new Jedis("localhost", 6379);
            jedis.auth("abc");
            jedis.sadd("city", "北京");
            jedis.sadd("city", "上海");
            jedis.sadd("city", "广州");
            jedis.sadd("city", "深圳");
            Set<String> smembers = jedis.smembers("city");
            for (String order : smembers) {
                System.out.println(order);
            }
        }

        @Test
        public void hash() {
            Jedis jedis = new Jedis("localhost", 6379);
            jedis.auth("abc");
            jedis.hset("hash01", "name", "李白");
            System.out.println(jedis.hget("hash01", "name"));
            Map<String, String> map = new HashMap<String, String>();
            map.put("job", "java 工程师");
            map.put("address", "北京");
            map.put("email", "hello@sohu.com");
            jedis.hmset("hash02", map);
            List<String> result = jedis.hmget("hash02", "job", "email");
            System.out.println("----------------------------");
            for (String element : result) {
                System.out.println(element);
            }
        }

        @Test
        public void zset() {
            Jedis jedis = new Jedis("localhost", 6379);
            jedis.auth("abc");
            jedis.zadd("hero", 2, "张飞");
            jedis.zadd("hero", 9, "马超");
            jedis.zadd("hero", 7, "赵云");
            jedis.zadd("hero", 1, "关羽");
            List<String> zrange = jedis.zrange("hero", 0, -1);
            for (String e : zrange) {
                System.out.println(e);
            }
        }
    }

    ```

## SpringBoot 整合Redis

### 简介

1. `RestTemplate`


2. `commons-pool2`
    `commons-pool2` 是一个**通用对象池框架**，它本身并不知道 Redis 是什么，也不会主动连接 Redis。

    它可以管理任何创建成本较高、能够重复使用的对象，例如：

    - Redis 连接
    - 数据库连接
    - FTP/SFTP 连接
    - Socket 网络连接
    - HTTP 客户端
    - 线程或其他昂贵对象

    核心思想是：

    ```text
    创建对象 → 放进池中 → 借出使用 → 使用完归还 → 下次复用
    ```

    在 Redis 项目中的关系是：

    ```text
    Spring Data Redis
        ↓
    Lettuce / Jedis 客户端
        ↓
    Commons Pool 2 管理连接池
        ↓
    Redis Server
    ```

    `commons-pool2` 只是负责“管理连接对象”，真正连接和操作 Redis 的是 Lettuce 或 Jedis。

    它最常见的核心类是：

    ```java
    GenericObjectPool<T>
    ```

    其中泛型 `T` 可以是 Redis 连接，也可以是任何其他可复用对象。

### 创建项目

1. pom
    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-pool2</artifactId>
    </dependency>
    ```

2. application.yml
    ```yaml
    server:
      port: 10000
    
    spring:
      data:
        redis:
          # Redis 服务器
          host: localhost
          port: 6379
    
          # Redis 密码
          password: "${REDIS_PASSWORD:abc}"
    
          # 数据库索引
          database: 0
    
          # 建立连接的超时时间
          connect-timeout: 3s
    
          # Redis 命令读取超时时间
          timeout: 3s
    
          # 明确使用 Lettuce 客户端
          client-type: lettuce
    
          lettuce:
            pool:
    
              enabled: true
    
              # 最大连接数，需要根据并发量调整
              max-active: 20
    
              # 连接池耗尽后最多等待2秒
              max-wait: 2s
    
              # 最大空闲连接
              max-idle: 8
    
              # 最小空闲连接
              min-idle: 1
    
              # 定期检查空闲连接
              time-between-eviction-runs: 30s
    ```
3. 配置示例
    ```properties
    #Redis 服务器地址
    spring.redis.host=localhost
    #Redis 服务器连接端口
    spring.redis.port=6379
    #Redis 如果有密码,需要配置, 没有密码就不要写
    spring.redis.password=abc
    #Redis 数据库索引（默认为 0）
    spring.redis.database=0
    #连接超时时间（毫秒）
    spring.redis.timeout=1800000
    #连接池最大连接数（使用负值表示没有限制）
    spring.redis.lettuce.pool.max-active=20
    #最大阻塞等待时间(负数表示没限制)
    spring.redis.lettuce.pool.max-wait=-1
    #连接池中的最大空闲连接
    spring.redis.lettuce.pool.max-idle=5
    #连接池中的最小空闲连接
    spring.redis.lettuce.pool.min-idle=0
    ```

4. 配置序列化器
    - RedisTemplate默认使用JDK序列化，这要求对象必须实现`Serializible`接口，且存储过程容易出问题。
    - JDK 序列化保存的是二进制数据，可读性差，并且与 Java 类结构耦合较强。
    - 配置 JSON 序列化后，对象不需要实现 Serializable，Redis 中的数据也更容易查看。
    - 通过覆盖默认序列化器为JSON序列化，可以省不少事

    ```java
    @Bean
    public RedisTemplate<String, Object> redisTemplate(
            RedisConnectionFactory factory) {

        RedisTemplate<String, Object> template =
                new RedisTemplate<>();

        StringRedisSerializer stringSerializer =
                new StringRedisSerializer();

        GenericJackson2JsonRedisSerializer jsonSerializer =
                new GenericJackson2JsonRedisSerializer();

        template.setConnectionFactory(factory);
        template.setKeySerializer(stringSerializer);
        template.setValueSerializer(jsonSerializer);
        template.setHashKeySerializer(stringSerializer);
        template.setHashValueSerializer(jsonSerializer);

        return template;
    }
    ```

### 案例

1. 代码
    ```java
    package com.lcq.jedisboot.controller;

    import jakarta.annotation.Resource;
    import org.springframework.data.redis.core.RedisTemplate;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;

    import java.util.List;

    /**
    * @author lcq
    * @version 1.0
    */

    @RestController
    @RequestMapping("/redis")
    public class RedisController {

        @Resource
        RedisTemplate<String, Object> redisTemplate;

        @RequestMapping("/a")
        public String get(){
            redisTemplate.opsForValue().set("a", "b");

            String s = (String) redisTemplate.opsForValue().get("a");

            redisTemplate.opsForList().leftPushAll("names", "jack", "tom");

            return s;
        }

        @RequestMapping("/b")
        public List getB(){
            redisTemplate.opsForValue().set("a", "b");


            Long l = redisTemplate.opsForList().leftPushAll("names", "jack", "tom");

            List<Object> names = redisTemplate.opsForList().range("names", 0, -1);

            return names;
        }
    }
    ```

### 序列化问题

不是“不修改序列化器一定造成 Bug”，而是：**写入和读取使用的格式不一致时才会出问题。**

1. 为什么 `keys("*")` 得到空集合

    默认 `RedisTemplate` 使用 JDK 序列化。写入：

    ```java
    redisTemplate.opsForValue().set("name", "韩顺平教育");
    ```

    Redis 中真正保存的 Key 并不是纯文本：

    ```text
    name
    ```

    而是类似下面的 JDK 二进制：

    ```text
    AC ED 00 05 74 00 04 6E 61 6D 65
    ```

    执行：

    ```java
    redisTemplate.keys("*");
    ```

    星号也会被 JDK 序列化：

    ```text
    AC ED 00 05 74 00 01 2A
    ```

    Redis 收到的查询模式并不是单独的 `*`，前面还有 JDK 序列化头和字符串长度，因此匹配不到之前保存的 Key。

    改成：

    ```java
    template.setKeySerializer(
            new StringRedisSerializer()
    );
    ```

    以后：

    ```text
    "name" → name
    "*"    → *
    ```

    Redis 才能正确进行模糊匹配。

2. 为什么出现 `Unrecognized token 'beijing'`

    假设 Value 配置成 JSON：

    ```java
    template.setValueSerializer(
            new GenericJackson2JsonRedisSerializer()
    );
    ```

    应用读取 Redis 时，会把内容当成 JSON 解析。

    但是你在 Redis 客户端中直接执行：

    ```redis
    SET city beijing
    ```

    保存的是普通文本：

    ```text
    beijing
    ```

    它不是合法的 JSON 字符串。合法的 JSON 字符串应该包含双引号：

    ```json
    "beijing"
    ```

    所以 JSON 反序列化器解析 `beijing` 时会报：

    ```text
    Unrecognized token 'beijing'
    ```

3. 本质原因

    ```text
    写入格式：普通字符串
    读取格式：JSON
    结果：反序列化失败
    ```

    或者：

    ```text
    写入格式：JDK 二进制
    查询模式：经过 JDK 序列化的 "*"
    结果：模糊匹配失败
    ```

    因此应该统一约定：

    ```text
    Key        → StringRedisSerializer
    Java 对象  → GenericJackson2JsonRedisSerializer
    普通字符串 → StringRedisTemplate
    ```

    例如普通字符串使用：

    ```java
    stringRedisTemplate.opsForValue()
            .set("city", "beijing");
    ```

    Java 对象使用：

    ```java
    redisTemplate.opsForValue()
            .set("user:1", user);
    ```

    不要对同一个 Key 混合使用：

    - Redis CLI 原始字符串；
    - `StringRedisTemplate`；
    - JDK 序列化的 `RedisTemplate`；
    - JSON 序列化的 `RedisTemplate`。

    一句话总结：**修改序列化器不是为了修复 Redis，而是为了统一数据协议，让 Key 保持普通字符串、对象保持 JSON，避免不同读写方式互相不认识。**

## Redis持久化-RDB

### 简介

1. 在线文档 : https://redis.io/topics/persistence

2. 持久化方案
    -  RDB（Redis DataBase）
    -  AOF（Append Of File）

3. RDB 是什么？
    在指定的时间间隔内将内存中的数据集快照写入磁盘，也就是 Snapshot 快照，恢复时将快照文件读到内存。


### RDB 持久化流程

1. 图示
    ```mermaid
    flowchart TD
        A([执行 BGSAVE<br/>或自动触发 RDB]) --> B[Redis 主进程接收持久化请求]

        B --> C{当前是否已有<br/>RDB/AOF 后台子进程?}

        C -- 是 --> D([本次 BGSAVE 不执行<br/>直接返回])
        C -- 否 --> E[主进程执行 fork]

        E --> F[父进程]
        E --> G[子进程]

        F --> H[继续处理客户端请求<br/>GET / SET / HSET ...]

        G --> I[读取 fork 时刻的数据快照<br/>Copy-On-Write]
        I --> J[将数据写入临时 RDB 文件]
        J --> K{临时 RDB<br/>是否写入成功?}

        K -- 否 --> L[记录 RDB 保存失败]
        K -- 是 --> M[原子替换旧的 dump.rdb]
        M --> N[新的 dump.rdb 持久化完成]

        L --> O[子进程退出]
        N --> O

        O --> P[主进程收到子进程结束通知<br/>SIGCHLD]
        P --> Q[更新持久化状态、时间等统计信息]

        H -. 主进程持续运行 .-> Q
        Q --> R([BGSAVE 流程结束])
    ```

    ```mermaid
    flowchart TD
        A([触发 BGSAVE]) --> B{是否已有后台<br/>持久化子进程?}

        B -- 是 --> C([直接返回])
        B -- 否 --> D[Redis 主进程 fork]

        D --> E[父进程]
        D --> F[子进程]

        E --> G[继续处理客户端请求]

        F --> H[基于 fork 时刻的数据快照]
        H --> I[写入临时 RDB 文件]
        I --> J[原子替换旧 dump.rdb]
        J --> K[子进程退出]

        K --> L[通知主进程]
        L --> M[更新 RDB 状态和统计信息]

        M --> N([RDB 持久化完成])
    ```
2. 步骤

    1. redis 客户端执行 bgsave 命令或者自动触发 bgsave 命令
        `SAVE` 和 `BGSAVE` 都是 Redis 用于生成 RDB 持久化文件的命令，但它们在**执行方式**和**对服务的影响**上有根本不同。

        简单来说：**`SAVE` 是同步阻塞的，而 `BGSAVE` 是异步非阻塞的**。在生产环境中，我们几乎总是使用 `BGSAVE`。

        核心区别对比

        🔴 SAVE：同步阻塞
        *   **执行方式**：由 Redis **主进程**直接执行，**同步**完成保存操作。
        *   **服务影响**：执行期间**完全阻塞** Redis 服务器，无法处理任何客户端的读写请求。
        *   **性能与资源**：不创建新进程，因此**内存开销小**；但会因阻塞导致服务不可用。
        *   **适用场景**：**极其有限**。仅适用于数据量小、可以接受服务中断的场景，如**停机维护**或**服务低谷期**的手动备份。

        🟢 BGSAVE：异步非阻塞
        *   **执行方式**：**Fork** 出一个**子进程**，由子进程在**后台异步**完成保存。
        *   **服务影响**：主进程**不阻塞**，可继续正常处理客户端请求。仅在 Fork 子进程的瞬间有短暂停顿。
        *   **性能与资源**：Fork 子进程会**消耗额外内存**，并可能增加 CPU 开销。
        *   **适用场景**：**线上生产环境的标准选择**。适合数据量大、对服务可用性要求高的场景。

    2. 主进程判断当前是否已经存在正在执行的子进程，如果存在，那么主进程直接返回；
    3. 如果不存在正在执行的子进程，那么就 fork 一个新的子进程进行持久化数据，fork 过程是阻塞的，fork 操作完成后主进程即可执行其他操作；
    4. 子进程先将数据写入到临时的 rdb 文件中，待快照数据写入完成后再原子替换旧的 rdb文件；
    5. 同时发送信号给主进程，通知主进程 rdb 持久化完成，主进程更新相关的统计信息



3. 小结
    - 整个过程中，主进程是不进行任何 IO 操作的，这就确保了极高的性能
        对于`bgsave`，磁盘IO操作由fork的子进程完成
    - 如果需要进行大规模数据的恢复, 且对于数据恢复的完整性不是非常敏感，那 RDB 方式要比 AOF 方式更加的高效
        AOF需要根据操作日志重建数据，RDB只需要直接恢复快照时的状态。
    - RDB 的缺点是最后一次持久化后的数据可能丢失
        > RDB 是阶段性快照，所以两次快照之间天然存在一个数据丢失窗口。


###  Fork&Copy-On-Write

1. Fork 的作用是复制一个与当前进程一样的进程。新进程的所有数据(变量、环境变量、程序计数器等) 数值都和原进程一致，但是是一个全新的进程，并作为原进程的子进程

1. 在 Linux 程序中，fork()会产生一个和父进程完全相同的子进程，但子进程在此后多会exec 系统调用，出于效率考虑，Linux 中引入了"写时复制技术 即: copy-on-write" , 有兴趣的参考: https://blog.csdn.net/Code_beeps/article/details/92838520

1. 一般情况父进程和子进程会共用同一段物理内存，只有进程空间的各段的内容要发生变化时，才会将父进程的内容复制一份给子进程。


### RDB 配置

#### 简介

1. `dump.rdb` 文件

    - 在 `redis.conf` 中配置文件名称，默认为 `dump.rdb`
    - 执行 flushall 命令，也会产生 dump.rdb 文件, 数据为空.


#### 相关配置快

1. 英文
    ```conf
    ################################ SNAPSHOTTING  ################################

    # Save the DB to disk.
    #
    # save <seconds> <changes> [<seconds> <changes> ...]
    #
    # Redis will save the DB if the given number of seconds elapsed and it
    # surpassed the given number of write operations against the DB.
    #
    # Snapshotting can be completely disabled with a single empty string argument
    # as in following example:
    #
    # save ""
    #
    # Unless specified otherwise, by default Redis will save the DB:
    #   * After 3600 seconds (an hour) if at least 1 change was performed
    #   * After 300 seconds (5 minutes) if at least 100 changes were performed
    #   * After 60 seconds if at least 10000 changes were performed
    #
    # You can set these explicitly by uncommenting the following line.
    #
    # save 3600 1 300 100 60 10000

    # By default Redis will stop accepting writes if RDB snapshots are enabled
    # (at least one save point) and the latest background save failed.
    # This will make the user aware (in a hard way) that data is not persisting
    # on disk properly, otherwise chances are that no one will notice and some
    # disaster will happen.
    #
    # If the background saving process will start working again Redis will
    # automatically allow writes again.
    #
    # However if you have setup your proper monitoring of the Redis server
    # and persistence, you may want to disable this feature so that Redis will
    # continue to work as usual even if there are problems with disk,
    # permissions, and so forth.
    stop-writes-on-bgsave-error yes

    # Compress string objects using LZF when dump .rdb databases?
    # By default compression is enabled as it's almost always a win.
    # If you want to save some CPU in the saving child set it to 'no' but
    # the dataset will likely be bigger if you have compressible values or keys.
    rdbcompression yes

    # Since version 5 of RDB a CRC64 checksum is placed at the end of the file.
    # This makes the format more resistant to corruption but there is a performance
    # hit to pay (around 10%) when saving and loading RDB files, so you can disable it
    # for maximum performances.
    #
    # RDB files created with checksum disabled have a checksum of zero that will
    # tell the loading code to skip the check.
    rdbchecksum yes

    # Enables or disables full sanitization checks for ziplist and listpack etc when
    # loading an RDB or RESTORE payload. This reduces the chances of a assertion or
    # crash later on while processing commands.
    # Options:
    #   no         - Never perform full sanitization
    #   yes        - Always perform full sanitization
    #   clients    - Perform full sanitization only for user connections.
    #                Excludes: RDB files, RESTORE commands received from the master
    #                connection, and client connections which have the
    #                skip-sanitize-payload ACL flag.
    # The default should be 'clients' but since it currently affects cluster
    # resharding via MIGRATE, it is temporarily set to 'no' by default.
    #
    # sanitize-dump-payload no

    # The filename where to dump the DB
    dbfilename dump.rdb

    # Remove RDB files used by replication in instances without persistence
    # enabled. By default this option is disabled, however there are environments
    # where for regulations or other security concerns, RDB files persisted on
    # disk by masters in order to feed replicas, or stored on disk by replicas
    # in order to load them for the initial synchronization, should be deleted
    # ASAP. Note that this option ONLY WORKS in instances that have both AOF
    # and RDB persistence disabled, otherwise is completely ignored.
    #
    # An alternative (and sometimes better) way to obtain the same effect is
    # to use diskless replication on both master and replicas instances. However
    # in the case of replicas, diskless is not always an option.
    rdb-del-sync-files no

    # The working directory.
    #
    # The DB will be written inside this directory, with the filename specified
    # above using the 'dbfilename' configuration directive.
    #
    # The Append Only File will also be created inside this directory.
    #
    # Note that you must specify a directory here, not a file name.
    dir ./
    ```

2. 中文

    ```conf
    ################################ RDB 快照持久化 ################################

    # 把数据库保存到磁盘。
    #
    # save <秒数> <写入次数> [<秒数> <写入次数> ...]
    #
    # 当指定秒数已经过去，并且对数据库执行的写操作次数超过对应阈值时，Redis 会保存数据库。
    #
    # 可以使用一个空字符串参数完全关闭快照，例如：
    #
    # save ""
    #
    # 除非另外指定，Redis 默认会在以下条件保存数据库：
    #   * 3600 秒（1 小时）内至少发生 1 次修改
    #   * 300 秒（5 分钟）内至少发生 100 次修改
    #   * 60 秒内至少发生 10000 次修改
    #
    # 取消下面一行的注释，可以显式设置这些保存条件。
    #
    # save 3600 1 300 100 60 10000

    # 默认情况下，如果启用了 RDB 快照（至少配置一个 save 条件）且最近一次后台保存失败，
    # Redis 将停止接受写操作。这会以比较强硬的方式提醒使用者：数据没有被正确持久化到磁盘；
    # 否则可能一直无人察觉，最终导致严重事故。
    #
    # 后台保存恢复正常后，Redis 会自动重新允许写操作。
    #
    # 如果已经对 Redis 服务器及其持久化建立了完善监控，可以考虑关闭此功能，
    # 让 Redis 即使遇到磁盘、权限等问题也继续照常工作。
    stop-writes-on-bgsave-error yes

    # 生成 .rdb 数据库时，是否使用 LZF 压缩字符串对象？
    # 压缩默认开启，因为它几乎总能带来收益。如果希望保存子进程少消耗一些 CPU，
    # 可以设为 no；但如果值或键可以压缩，生成的数据集很可能会更大。
    rdbcompression yes

    # 从 RDB 格式版本 5 开始，文件尾部会写入 CRC64 校验和。这能增强格式抵抗损坏的能力，
    # 但保存和加载 RDB 时会带来约 10% 的性能开销；追求最高性能时可以关闭。
    #
    # 关闭校验后生成的 RDB 文件会把校验和写为 0，加载代码看到 0 就会跳过校验。
    rdbchecksum yes

    # 加载 RDB 或 RESTORE 载荷时，启用或关闭对 ziplist、listpack 等结构的完整安全检查。
    # 这样可以降低随后处理命令时触发断言或崩溃的概率。
    # 可选值：
    #   no      - 从不执行完整检查
    #   yes     - 始终执行完整检查
    #   clients - 只对用户连接执行完整检查；不包括 RDB 文件、通过主节点连接收到的
    #             RESTORE 命令，以及带有 skip-sanitize-payload ACL 标志的客户端连接
    # 理论默认值应为 clients，但它当前会影响使用 MIGRATE 的集群重新分片，
    # 因此暂时默认设为 no。
    #
    # sanitize-dump-payload no

    # 数据库转储文件名。
    dbfilename dump.rdb

    # 在未启用持久化的实例中，删除复制过程使用的 RDB 文件。该选项默认关闭。
    # 某些环境由于法规或其他安全要求，需要尽快删除主节点为了向副本提供数据而落盘的
    # RDB 文件，或副本为了初次同步加载数据而保存到磁盘的 RDB 文件。
    # 注意：此选项只在 AOF 和 RDB 持久化都关闭的实例中有效，否则会被完全忽略。
    #
    # 另一种有时更好的实现方式，是让主节点和副本都使用无盘复制；但对副本而言，
    # 无盘加载并不总是可选方案。
    rdb-del-sync-files no

    # 工作目录。
    #
    # 数据库会写到这个目录中，文件名由上面的 dbfilename 指定。
    #
    # AOF 文件也会创建在这个目录中。
    #
    # 注意：这里必须指定目录，不能指定文件名。
    dir ./
    ```

#### 属性解析

1. `save`

    - 格式为 `save <seconds> <changes>`，可以连续配置多组；**任意一组**条件满足，Redis 就会发起一次后台 RDB 保存。
    - 例如 `save 300 10` 的意思是：距离上次成功保存至少 300 秒，且其后至少发生 10 次数据修改时触发。它不是“每 300 秒无条件保存”，也不是“300 秒内刚好有 10 次修改”。
    - Redis 7.2.16 默认规则为 `save 3600 1 300 100 60 10000`：低频写入最多约一小时生成一次快照，高频写入则可按一分钟规则生成快照。配置文件中该行被注释，只是因为这些规则已内建为默认值；显式写出可让配置意图更清楚。
    - `save ""` 仅关闭**自动** RDB 快照，仍可手动执行 `SAVE` 或 `BGSAVE`。`SAVE` 会阻塞 Redis 主线程，线上通常使用 `BGSAVE` 或自动规则。
    - RDB 不是增量日志，因此 `save 60 10000` 不代表“最多丢失 60 秒”：若一分钟内写入不足 10000 次，快照不会触发，实际数据丢失窗口会更长。
    - 运行期间，可以通过`CONFIG SET save ""`停止当前实例的自动备份，重启后失效。

    ```conf
    # 低频写入也希望每 15 分钟保存一次
    save 900 1

    # 高频写入时每分钟保存一次
    save 60 10000
    ```

2. `stop-writes-on-bgsave-error`

    - 值为 `yes` 时，已启用至少一个 `save` 规则且最近一次后台保存失败，Redis 会拒绝后续写命令，避免应用把“数据已持久化”当成事实；读请求仍可处理。
    - 常见失败原因包括磁盘已满、目录无写权限、目录不存在和文件系统 I/O 错误。问题排除后，下一次后台保存成功会自动恢复写入。
    - 设为 `no` 能让业务在磁盘故障时继续写入，但这些数据可能只留在内存。只有已配置磁盘空间、持久化状态和告警监控时才考虑关闭。

    ```conf
    stop-writes-on-bgsave-error yes
    ```

3. `rdbcompression`

    - 控制 RDB 写入时是否用 LZF 压缩**字符串对象**，默认 `yes`。压缩通常能减少 RDB 文件体积和磁盘 I/O。
    - 代价是后台保存子进程消耗更多 CPU。只有数据本身已压缩（如 JPEG、ZIP、密文）或实测保存阶段 CPU 是瓶颈时，才考虑改为 `no`。
    - 它不影响内存中的数据，也不提供加密；敏感数据仍应依靠磁盘/卷加密和访问控制保护。

    ```conf
    rdbcompression yes
    ```

4. `rdbchecksum`

    - 控制是否在 RDB 文件末尾写入 CRC64 校验和，默认 `yes`；Redis 加载文件时可借此检测传输或磁盘损坏。
    - 校验和只解决完整性检测，不能替代备份，也不提供保密性。Redis 模板说明保存和加载校验会有约 10% 的开销；未实测到瓶颈时不建议关闭。
    - 设为 `no` 后，生成文件的校验和为 0，加载时会跳过校验，发现损坏的能力也随之降低。

    ```conf
    rdbchecksum yes
    ```

5. `sanitize-dump-payload`

    - 控制加载 RDB 或处理 `RESTORE` 数据时，是否对 ziplist、listpack 等内部编码进行完整合法性检查，以降低随后执行命令时断言失败或崩溃的风险。
    - `no`：从不完整检查，当前默认值；`yes`：始终检查，包括 RDB 文件；`clients`：只检查普通客户端传来的载荷，**不检查 RDB 文件**。
    - 通常保持默认值即可。Cluster 中使用 `MIGRATE`/重分片时，调整前应先做兼容性测试；若目的是检查启动加载的 RDB，`clients` 不能满足要求，应使用 `yes`。

    ```conf
    # 默认值；通常不必显式配置
    sanitize-dump-payload no
    ```

6. `dbfilename`

    - RDB 文件名，默认 `dump.rdb`；完整路径由 `dir` 和 `dbfilename` 共同决定。例如 `dir /data` 与 `dbfilename redis-6379.rdb` 会写入 `/data/redis-6379.rdb`。
    - 多个 Redis 实例不能共用同一个 `dir` + `dbfilename` 组合，否则可能互相覆盖或争抢文件。应为每个实例分配独立数据目录。
    - 若 AOF 也已开启，Redis 重启时会优先使用 AOF 重建数据，因为它通常包含更完整的写入记录。

    ```conf
    dbfilename dump.rdb
    ```

7. `rdb-del-sync-files`

    - 复制全量同步时，主从可能为了传输或加载数据临时在磁盘上使用 RDB 文件。设为 `yes` 会在使用完后尽快删除这类**仅用于复制**的文件，降低磁盘残留数据的风险。
    - 它只在 RDB 和 AOF 持久化都关闭时生效；只要任一持久化方式开启，Redis 都会忽略此项。因此它不能用来删除正常持久化的 `dump.rdb`。
    - 有合规或敏感数据落盘限制时可评估开启；普通部署保持默认 `no` 即可。

    ```conf
    rdb-del-sync-files no
    ```

8. `dir`

    - Redis 的工作数据目录。RDB 的实际位置为 `dir` + `dbfilename`；Redis 7 的 AOF 文件默认位于此目录下的 `appenddirname`（默认 `appendonlydir`）中。
    - `dir ./` 是相对 Redis 进程工作目录的路径。在 systemd、Docker 等环境中，它不一定等于执行命令时所在目录；生产环境应使用明确的绝对路径，并确保 Redis 运行用户拥有读写权限。
    - Docker 中通常设为 `/data` 并挂载到宿主机或持久卷；否则容器重建后 RDB/AOF 都可能丢失。不要让多个实例共用数据目录。
    - `dir` 与 `dbfilename` 属于 Redis 写文件相关的敏感配置；默认强化安全配置下，不应依赖远程 `CONFIG SET` 在运行时修改它们，应在配置文件中设置并按变更流程重启。

    ```conf
    dir /data
    dbfilename dump.rdb
    ```

9. RDB 配置取舍

    - **缓存且允许丢失**：可以使用 `save ""` 关闭自动 RDB；若同时关闭 AOF，实例重启后数据为空。
    - **只使用 RDB**：配置多个 `save` 规则，并接受“从最近一次成功快照到故障之间的数据会丢失”的恢复点目标；还要把快照复制到 Redis 节点之外的备份介质。
    - **业务数据较重要**：常见做法是保留 RDB 用于备份和快速恢复，同时开启 AOF（通常 `appendfsync everysec`）缩小数据丢失窗口。两者同时开启时，启动恢复优先使用 AOF。

    可用下列命令检查 RDB 状态和最近成功保存时间：

    ```powershell
    redis-cli INFO persistence
    redis-cli LASTSAVE
    ```

### RDB 备份&恢复

1. 注意
    - Redis 可以充当缓存, 对项目进行优化, 因此重要/敏感的数据建议在 Mysql要保存一份
    - 从设计层面来说, Redis 的内存数据, 都是可以重新获取的(可能来自程序, 也可能来自Mysql)

2. 备份
    - `config get dir` 查询 rdb 文件的目录
    - 将 dump.rdb 进行备份, 如果有必要可以写 shell 脚本来定时备份 [参考 Linux 课程 定时备份 Mysql 数据库, 视频地址 https://www.bilibili.com/video/BV1Sv411r7vd?p=105 ] 

3. 意义
    Redis的价值不仅限于缓存数据库，还可以存储一些仅需存在于内存的数据，比如：
    - 集群共享的Session
    - 分布式锁

### RDB 持久化小结
1. 优势
    - 适合大规模的数据恢复
    - 对数据完整性和一致性要求不高更适合使用
    - 节省磁盘空间
    - 恢复速度快

1. 劣势
    - 虽然 Redis 在 fork 时使用了写时拷贝技术(Copy-On-Write), 但是如果数据庞大时还是比较消耗性能。
    - 在备份周期在一定间隔时间做一次备份，所以如果 Redis 意外 down 掉的话(如果正常关闭 Redis, 仍然会进行 RDB 备份, 不会丢失数据), 就会丢失最后一次快照后的所有修改

## AOF持久化

### 基本介绍

1. AOF 是什么?
    - AOF(Append Only File)
    - 以日志的形式来记录每个写操作(增量保存)，将 Redis 执行过的所有写指令记录下来(比如 set/del 操作会记录, 读操作 get 不记录)
    - 只许追加文件但不可以改写文件
    - redis 启动之初会读取该文件重新构建数据
    - redis 重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作


### AOF 持久化流程

1. 持久化流程示意图

    ```mermaid
    flowchart TD
        A[命令写入] -->|1. append| B[AOF 缓冲区]
        B -->|2. sync| C[AOF 文件]

        C -->|3. rewrite| C

        C -->|4. load| D[Redis 重启恢复]
    ```

2. 解读
    - 客户端的请求写命令会被 append 追加到 AOF 缓冲区内
    - AOF 缓冲区根据 AOF 持久化策略[always,everysec,no]将操作 sync 同步到磁盘的 AOF 文件中
    - AOF 文件大小超过重写策略或手动重写时，会对 AOF 文件 rewrite 重写，压缩 AOF 文件容量
    - Redis 服务重启时，会重新 load 加载 AOF 文件中的写操作达到数据恢复的目的

### AOF 配置

1. 英文
    ```conf
    ############################## APPEND ONLY MODE ###############################

    # By default Redis asynchronously dumps the dataset on disk. This mode is
    # good enough in many applications, but an issue with the Redis process or
    # a power outage may result into a few minutes of writes lost (depending on
    # the configured save points).
    #
    # The Append Only File is an alternative persistence mode that provides
    # much better durability. For instance using the default data fsync policy
    # (see later in the config file) Redis can lose just one second of writes in a
    # dramatic event like a server power outage, or a single write if something
    # wrong with the Redis process itself happens, but the operating system is
    # still running correctly.
    #
    # AOF and RDB persistence can be enabled at the same time without problems.
    # If the AOF is enabled on startup Redis will load the AOF, that is the file
    # with the better durability guarantees.
    #
    # Please check https://redis.io/topics/persistence for more information.

    appendonly no

    # The base name of the append only file.
    #
    # Redis 7 and newer use a set of append-only files to persist the dataset
    # and changes applied to it. There are two basic types of files in use:
    #
    # - Base files, which are a snapshot representing the complete state of the
    #   dataset at the time the file was created. Base files can be either in
    #   the form of RDB (binary serialized) or AOF (textual commands).
    # - Incremental files, which contain additional commands that were applied
    #   to the dataset following the previous file.
    #
    # In addition, manifest files are used to track the files and the order in
    # which they were created and should be applied.
    #
    # Append-only file names are created by Redis following a specific pattern.
    # The file name's prefix is based on the 'appendfilename' configuration
    # parameter, followed by additional information about the sequence and type.
    #
    # For example, if appendfilename is set to appendonly.aof, the following file
    # names could be derived:
    #
    # - appendonly.aof.1.base.rdb as a base file.
    # - appendonly.aof.1.incr.aof, appendonly.aof.2.incr.aof as incremental files.
    # - appendonly.aof.manifest as a manifest file.

    appendfilename "appendonly.aof"

    # For convenience, Redis stores all persistent append-only files in a dedicated
    # directory. The name of the directory is determined by the appenddirname
    # configuration parameter.

    appenddirname "appendonlydir"

    # The fsync() call tells the Operating System to actually write data on disk
    # instead of waiting for more data in the output buffer. Some OS will really flush
    # data on disk, some other OS will just try to do it ASAP.
    #
    # Redis supports three different modes:
    #
    # no: don't fsync, just let the OS flush the data when it wants. Faster.
    # always: fsync after every write to the append only log. Slow, Safest.
    # everysec: fsync only one time every second. Compromise.
    #
    # The default is "everysec", as that's usually the right compromise between
    # speed and data safety. It's up to you to understand if you can relax this to
    # "no" that will let the operating system flush the output buffer when
    # it wants, for better performances (but if you can live with the idea of
    # some data loss consider the default persistence mode that's snapshotting),
    # or on the contrary, use "always" that's very slow but a bit safer than
    # everysec.
    #
    # More details please check the following article:
    # http://antirez.com/post/redis-persistence-demystified.html
    #
    # If unsure, use "everysec".

    # appendfsync always
    appendfsync everysec
    # appendfsync no

    # When the AOF fsync policy is set to always or everysec, and a background
    # saving process (a background save or AOF log background rewriting) is
    # performing a lot of I/O against the disk, in some Linux configurations
    # Redis may block too long on the fsync() call. Note that there is no fix for
    # this currently, as even performing fsync in a different thread will block
    # our synchronous write(2) call.
    #
    # In order to mitigate this problem it's possible to use the following option
    # that will prevent fsync() from being called in the main process while a
    # BGSAVE or BGREWRITEAOF is in progress.
    #
    # This means that while another child is saving, the durability of Redis is
    # the same as "appendfsync no". In practical terms, this means that it is
    # possible to lose up to 30 seconds of log in the worst scenario (with the
    # default Linux settings).
    #
    # If you have latency problems turn this to "yes". Otherwise leave it as
    # "no" that is the safest pick from the point of view of durability.

    no-appendfsync-on-rewrite no

    # Automatic rewrite of the append only file.
    # Redis is able to automatically rewrite the log file implicitly calling
    # BGREWRITEAOF when the AOF log size grows by the specified percentage.
    #
    # This is how it works: Redis remembers the size of the AOF file after the
    # latest rewrite (if no rewrite has happened since the restart, the size of
    # the AOF at startup is used).
    #
    # This base size is compared to the current size. If the current size is
    # bigger than the specified percentage, the rewrite is triggered. Also
    # you need to specify a minimal size for the AOF file to be rewritten, this
    # is useful to avoid rewriting the AOF file even if the percentage increase
    # is reached but it is still pretty small.
    #
    # Specify a percentage of zero in order to disable the automatic AOF
    # rewrite feature.

    auto-aof-rewrite-percentage 100
    auto-aof-rewrite-min-size 64mb

    # An AOF file may be found to be truncated at the end during the Redis
    # startup process, when the AOF data gets loaded back into memory.
    # This may happen when the system where Redis is running
    # crashes, especially when an ext4 filesystem is mounted without the
    # data=ordered option (however this can't happen when Redis itself
    # crashes or aborts but the operating system still works correctly).
    #
    # Redis can either exit with an error when this happens, or load as much
    # data as possible (the default now) and start if the AOF file is found
    # to be truncated at the end. The following option controls this behavior.
    #
    # If aof-load-truncated is set to yes, a truncated AOF file is loaded and
    # the Redis server starts emitting a log to inform the user of the event.
    # Otherwise if the option is set to no, the server aborts with an error
    # and refuses to start. When the option is set to no, the user requires
    # to fix the AOF file using the "redis-check-aof" utility before to restart
    # the server.
    #
    # Note that if the AOF file will be found to be corrupted in the middle
    # the server will still exit with an error. This option only applies when
    # Redis will try to read more data from the AOF file but not enough bytes
    # will be found.
    aof-load-truncated yes

    # Redis can create append-only base files in either RDB or AOF formats. Using
    # the RDB format is always faster and more efficient, and disabling it is only
    # supported for backward compatibility purposes.
    aof-use-rdb-preamble yes

    # Redis supports recording timestamp annotations in the AOF to support restoring
    # the data from a specific point-in-time. However, using this capability changes
    # the AOF format in a way that may not be compatible with existing AOF parsers.
    aof-timestamp-enabled no
    ```

2. 中文
    ```conf
    ############################## AOF 持久化模式 ###############################

    # Redis 默认以异步方式把数据集转储到磁盘。这种模式对许多应用已经足够，但 Redis 进程
    # 故障或断电可能导致最近几分钟的写入丢失，具体取决于配置的 save 条件。
    #
    # AOF 是另一种持久化模式，能提供好得多的持久性。例如使用默认 fsync 策略（见后文）时，
    # 即使服务器断电等严重事故，Redis 通常只会丢失 1 秒写入；如果只是 Redis 进程自身出错
    # 而操作系统仍正常运行，则可能只丢失一条写入。
    #
    # AOF 和 RDB 可以同时启用，不会冲突。启动时如果 AOF 已启用，Redis 会加载持久性保障
    # 更好的 AOF 文件。
    #
    # 更多信息：https://redis.io/topics/persistence

    appendonly no

    # AOF 文件的基础名称。
    #
    # Redis 7 及更新版本使用一组 AOF 文件持久化数据集及其变更，基本分为两类：
    #
    # - 基础文件：表示文件创建时完整数据集状态的快照，可以采用 RDB（二进制序列化）
    #   或 AOF（文本命令）格式。
    # - 增量文件：包含前一个文件之后应用到数据集的新增命令。
    #
    # 此外，还使用 manifest 清单文件跟踪这些文件、文件创建顺序和应用顺序。
    #
    # Redis 按固定规则生成 AOF 文件名：前缀来自 appendfilename，后面追加序号和类型信息。
    #
    # 例如 appendfilename 设为 appendonly.aof 时，可能生成：
    #
    # - appendonly.aof.1.base.rdb：基础文件。
    # - appendonly.aof.1.incr.aof、appendonly.aof.2.incr.aof：增量文件。
    # - appendonly.aof.manifest：清单文件。

    appendfilename "appendonly.aof"

    # 为便于管理，Redis 把所有持久化 AOF 文件放在专用目录中，目录名由 appenddirname 决定。

    appenddirname "appendonlydir"

    # fsync() 调用要求操作系统真正把数据写入磁盘，而不是继续等待输出缓冲区中的更多数据。
    # 有些操作系统会真正立即刷盘，另一些只会尽快尝试。
    #
    # Redis 支持三种模式：
    #
    # no       - Redis 不调用 fsync，由操作系统自行决定何时刷盘，速度最快。
    # always   - 每次写入 AOF 日志后都 fsync，最慢但最安全。
    # everysec - 每秒 fsync 一次，是性能与安全的折中。
    #
    # 默认 everysec，因为它通常能在速度和数据安全之间取得正确平衡。你需要判断是否可以放宽
    # 到 no，让操作系统自行决定何时刷新输出缓冲区，以获得更高性能；但如果能接受一定数据
    # 丢失，也可以考虑默认的 RDB 快照持久化。相反，always 非常慢，但比 everysec 稍安全。
    #
    # 更多细节：
    # http://antirez.com/post/redis-persistence-demystified.html
    #
    # 不确定时，请使用 everysec。

    # appendfsync always
    appendfsync everysec
    # appendfsync no

    # 当 AOF fsync 策略为 always 或 everysec，并且后台保存进程（后台快照或 AOF 后台重写）
    # 正在对磁盘执行大量 I/O 时，某些 Linux 配置下 Redis 可能在 fsync() 上阻塞很久。
    # 目前没有彻底修复方式，因为即使把 fsync 放到另一个线程，同步 write(2) 仍会被阻塞。
    #
    # 为缓解该问题，可使用下面选项：BGSAVE 或 BGREWRITEAOF 进行期间，阻止主进程调用 fsync()。
    #
    # 这意味着另一个子进程保存数据期间，Redis 持久性相当于 appendfsync no。
    # 实际上，使用默认 Linux 设置时，最坏情况下可能丢失最多约 30 秒日志。
    #
    # 如果存在延迟问题可设为 yes；否则保持 no，从持久性角度看最安全。

    no-appendfsync-on-rewrite no

    # 自动重写 AOF。
    # 当 AOF 日志大小增长达到指定百分比时，Redis 可以隐式调用 BGREWRITEAOF 自动重写。
    #
    # 工作原理：Redis 记住最近一次重写后的 AOF 大小；如果重启后尚未重写，则使用启动时
    # AOF 的大小。
    #
    # Redis 将此基础大小与当前大小比较。如果当前大小增长超过指定百分比，就触发重写。
    # 还必须指定允许重写的最小 AOF 大小，避免文件仍很小时仅因达到增长百分比就频繁重写。
    #
    # 百分比设为 0 可以关闭自动 AOF 重写。

    auto-aof-rewrite-percentage 100
    auto-aof-rewrite-min-size 64mb

    # Redis 启动并把 AOF 数据重新载入内存时，可能发现 AOF 尾部被截断。
    # 运行 Redis 的系统崩溃时可能发生这种情况，尤其是 ext4 文件系统未使用 data=ordered
    # 挂载时；但如果只有 Redis 自身崩溃或终止，而操作系统仍正常运行，就不会出现。
    #
    # 遇到尾部截断时，Redis 可以报错退出；也可以尽量加载数据后启动，后者是当前默认行为。
    # 以下选项控制该行为。
    #
    # aof-load-truncated 设为 yes 时，会加载截断的 AOF 并启动，同时记录日志通知用户。
    # 设为 no 时，服务器报错终止并拒绝启动；用户必须先用 redis-check-aof 修复 AOF，
    # 然后才能重新启动。
    #
    # 注意：如果损坏出现在 AOF 中间，服务器仍会报错退出。此选项只适用于 Redis 继续读取
    # AOF 时发现剩余字节不足，也就是文件尾部截断的情况。
    aof-load-truncated yes

    # Redis 可以用 RDB 或 AOF 格式创建 AOF 基础文件。RDB 格式始终更快、更高效；
    # 允许关闭它只是为了向后兼容。
    aof-use-rdb-preamble yes

    # Redis 支持在 AOF 中记录时间戳注释，以便从指定时间点恢复数据。
    # 但使用此能力会改变 AOF 格式，可能与现有 AOF 解析器不兼容。
    aof-timestamp-enabled no
    ```

#### 属性解析

1. `appendonly`

    - 是否启用 AOF，默认 `no`。设为 `yes` 后，改变数据集的写命令会追加到 AOF；`GET` 等只读命令不会记录。
    - AOF 和 RDB 可以同时开启。**两者都存在时，Redis 重启优先加载 AOF**，因为它通常包含更完整的写入记录；RDB 仍很适合独立备份和较快恢复。同时开启时，备份文件都会生成，只是默认先取用AOF
    - 线上从 RDB 切换到 AOF，不要只修改配置然后立即重启。应先备份 RDB，在运行中的实例执行 `CONFIG SET appendonly yes`，等待 AOF 重写完成并确认状态正常，再通过 `CONFIG REWRITE` 或配置管理把变更持久化。

    ```conf
    appendonly yes
    ```

2. `appendfilename`

    - AOF 文件的**名称前缀**，默认 `appendonly.aof`，而不是 Redis 7 中唯一的单个文件名。
    - 例如默认前缀可能产生 `appendonly.aof.1.base.rdb`（基础文件）、`appendonly.aof.1.incr.aof`（增量文件）和 `appendonly.aof.manifest`（清单文件）。启动恢复时，Redis 按 manifest 指定的顺序加载整个文件集。
    - 不要手动改名、删除其中一个文件，或只备份某一个 `.aof` 文件；应把 `appenddirname` 内的基础、增量和 manifest 文件作为一个整体处理。

    ```conf
    appendfilename "appendonly.aof"
    ```

3. `appenddirname`

    - 存放多段 AOF 文件的专用目录名，默认 `appendonlydir`。它位于 Redis 的 `dir` 数据目录之下，例如 `dir /data` 时默认路径为 `/data/appendonlydir`。
    - 用专用目录可避免 AOF 文件与 `dump.rdb` 混放，也能让备份脚本清楚地将整个 AOF 文件集一并复制。
    - Docker 中必须把 `dir`（通常 `/data`）挂载到持久卷；只挂载单个旧式 `appendonly.aof` 文件既不适用于 Redis 7 的多段 AOF，也不能保证恢复完整。

    ```conf
    dir /data
    appenddirname "appendonlydir"
    ```

4. `appendfsync`

    - 控制 Redis 调用 `fsync` 要求操作系统将 AOF 缓冲内容落盘的频率，是 AOF 在性能、延迟和数据丢失窗口之间最重要的取舍。

    | 取值 | 刷盘时机 | 取舍 |
    | :--- | :--- | :--- |
    | `always` | 每批写命令追加后 | 最安全，但磁盘延迟会直接影响写性能；极少数需要极低数据丢失目标的场景才考虑。 |
    | `everysec` | 每秒一次 | 默认值；性能与安全性较平衡。发生宕机时通常可能丢失约 1 秒写入。 |
    | `no` | Redis 不主动 `fsync`，交给操作系统 | 速度最快，但实际丢失窗口取决于内核和文件系统，常见 Linux 配置可能达到约 30 秒。 |

    - `everysec` 的“一秒”是目标而非绝对承诺；磁盘长时间卡顿、文件系统或掉电等异常仍可能扩大数据损失窗口。重要数据还应配合副本、监控和异机备份。
    - 不确定时保持 `everysec`；不要仅为追求吞吐将其改为 `no`，再把 AOF 当成强持久化保证。

    ```conf
    appendfsync everysec
    ```

5. `no-appendfsync-on-rewrite`

    - 当 `BGSAVE` 或 `BGREWRITEAOF` 正在进行时，磁盘 I/O 可能很重。设为 `yes` 会临时停止主进程的 AOF `fsync`，用于缓解写延迟尖刺。
    - 代价是这段期间的持久性退化为近似 `appendfsync no`；Redis 7.2.16 模板提示，在常见 Linux 默认设置下最坏可能丢失约 30 秒日志。
    - 默认 `no` 优先保证持久性。只有监控证实重写期间的 fsync 导致不可接受的尾延迟，并且业务能承受更大恢复点目标时才设为 `yes`。

    ```conf
    no-appendfsync-on-rewrite no
    ```

6. `auto-aof-rewrite-percentage` 与 `auto-aof-rewrite-min-size`

    - AOF 会随历史写入持续增长。重写不是简单压缩文件，而是在后台根据当前内存数据生成一个更精简的新的基础 AOF；Redis 仍继续处理写请求，并在完成后原子切换到新文件集。
    - `auto-aof-rewrite-percentage 100` 表示当前 AOF 相对上次重写后的基准大小增长 100% 时可以触发重写；若重启后从未重写，则以启动时 AOF 大小为基准。`0` 关闭自动重写。
    - `auto-aof-rewrite-min-size 64mb` 是第二道门槛：即使增长比例达到要求，当前 AOF 未达到 64 MB 也不重写，避免小文件频繁 `fork` 和 I/O。两项必须同时满足。
    - 数据量大时重写同样会 `fork`，要为写时复制内存和磁盘空间预留余量。可用 `BGREWRITEAOF` 手动触发，但应避开业务高峰。

    ```conf
    auto-aof-rewrite-percentage 100
    auto-aof-rewrite-min-size 64mb
    ```

7. `aof-load-truncated`

    - Redis 启动加载 AOF 时，如果只发现**文件尾部**因宕机或磁盘满而不完整，`yes`（默认）会尽可能加载有效部分、丢弃最后不完整命令并启动，同时在日志中告警。
    - 设为 `no` 时，遇到尾部截断 Redis 会拒绝启动，要求人工检查并使用 `redis-check-aof` 修复后再启动；适合更重视人工确认而非自动可用性的流程。
    - 该配置不处理文件中间的损坏。中间出现非法字节时 Redis 仍会启动失败；修复前先备份整个 `appendonlydir`，因为 `redis-check-aof --fix` 可能丢弃损坏位置之后的内容。

    ```conf
    aof-load-truncated yes
    ```

8. `aof-use-rdb-preamble`

    - 控制 AOF 重写生成的基础文件采用 RDB 二进制格式还是传统 AOF 命令格式。默认 `yes`，因此常会看到扩展名为 `.base.rdb` 的基础文件。
    - RDB 格式创建和加载都更快、占用空间也更小；Redis 官方仅为兼容旧工具或旧版本而支持关闭它，普通 Redis 7 部署保持 `yes`。
    - 这不是关闭 RDB 持久化，也不会改变 `save` 快照规则；它只决定**AOF 文件集中的基础文件格式**。

    ```conf
    aof-use-rdb-preamble yes
    ```

9. `aof-timestamp-enabled`

    - 开启后，Redis 会在 AOF 中写入时间戳注释，为按指定时间点恢复提供依据。
    - 该能力会改变 AOF 格式，可能与自研或旧版 AOF 解析器不兼容；没有配套时间点恢复工具链时保持默认 `no`。
    - 它不是备份策略，也不会替代异机备份、RDB 快照或副本。

    ```conf
    aof-timestamp-enabled no
    ```

10. AOF 配置取舍与巡检

    - **一般业务**：`appendonly yes` + `appendfsync everysec` + 默认重写阈值是常用起点；保留 RDB 快照用于独立备份。
    - **低延迟缓存**：若能接受较大数据丢失窗口，可使用 RDB 或无持久化；不要因 AOF 开启了就误以为数据一定可恢复。
    - **高数据安全要求**：不能只依赖 `appendfsync always`。仍需要副本、磁盘/卷可靠性、异机备份和恢复演练；AOF 损坏、节点或可用区整体故障都不是单机 fsync 能解决的。

    ```powershell
    redis-cli INFO persistence
    redis-cli BGREWRITEAOF
    ```

    重点观察 `aof_enabled`、`aof_current_size`、`aof_base_size`、`aof_rewrite_in_progress`、`aof_last_bgrewrite_status` 和 `aof_last_write_status`。备份时要将 `appendonlydir` 整体纳入，并与正在进行的重写协调，不能只复制一个增量文件。


### AOF 启动/修复/恢复

1. 基本说明
AOF 的备份机制和性能虽然和 RDB 不同, 但是备份和恢复的操作同 RDB 一样, 都是拷贝备份文件, 需要恢复时再拷贝到 Redis 工作目录下，启动系统即加载

2. 正常恢复
    - 修改默认的 `appendonly no`，改为 `yes`
    - 将有数据的 aof 文件定时备份, 需要恢复时, 复制一份保存到对应目录(查看目录：`config get dir`)
    - 恢复：重启 redis 然后重新加载

3. 异常恢复
    - 如遇到 AOF 文件损坏，通过`/usr/local/bin/redis-check-aof --fix appendonly.aof` 进行恢复
    - 建议先: 备份被写坏的 AOF 文件
    - 恢复：重启 redis，然后重新加载


### Rewrite 压缩

1. vrewrite 重写介绍
    - AOF 文件越来越大，需要定期对 AOF 文件进行重写达到压缩
    - 旧的 AOF 文件含有无效命令会被忽略，保留最新的数据命令 , 比如 `set a a1 ; set a b1 ;set a c1;` 保留最后一条指令就可以了
    - 多条写命令可以合并为一个 , 比如 `set a c1 b b1 c c1`
    - AOF 重写降低了文件占用空间
    - 更小的 AOF 文件可以更快的被 redis 加载

2. 重写触发配置
    - 手动触发
        直接调用 `bgrewriteaof` 命令
    - 自动触发
        auto-aof-rewrite-min-size: AOF 文件最小重写大小, 只有当 AOF 文件大小大于该值时候才能重写, 默认配置 64MB
        auto-aof-rewrite-percentage: 当前 AOF 文件大小和最后一次重写后的大小之间的比率等于或者大于指定的增长百分比，如 100 代表当前 AOF 文件是上次重写的两倍时候才重写系统载入时或者上次重写完毕时，Redis 会记录此时 AOF 大小，设为`base_size`。
    - 如果 `Redis 的 AOF 当前大小>= base_size +base_size*100%` (默认)且`当前大小>=64mb`(默认)的情况下，Redis 会对 AOF 进行重写

### AOF 持久化小结

1. 优势
    - 备份机制更稳健，丢失数据概率更低。
    - 可读的日志文本，通过操作 AOF 稳健，可以处理误操作
2. 劣势
    - 比起 RDB 占用更多的磁盘空间
    - 恢复备份速度要慢
    - 每次读写都同步的话，有一定的性能压力

3.  RDB 还是 AOF?
    - 官方文档地址: https://redis.io/topics/persistence
    - 官方推荐两个都启用
    - **如果只做缓存**：如果你只希望你的数据在服务器运行的时候存在, 你也可以不使用任何持久化方式


## Redis 事务

### 简介

1. Redis 的事务是什么?
    - Redis 事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行
    - 事务在执行的过程中，不会被其他客户端发送来的命令请求所打断
    - Redis 事务的主要作用就是串联多个命令防止别的命令插队

    > Redis事务区别于MySQL事务，不具有原子性，具有隔离性。
    > MySQL事务用于确保命令全部生效
    > Redis事务用于确保命令执行顺序，一条失败不会影响其他提交


2. Redis 事务三特性
    - 单独的隔离操作
        - 事务中的所有命令都会序列化、按顺序地执行
        - 事务在执行的过程中，不会被其他客户端发送来的命令请求所打断
    - 没有隔离级别的概念
        - 队列中的命令(指令), 在没有提交前都不会实际被执行
    - 不保证原子性
        事务执行过程中, 如果有指令执行失败，其它的指令仍然会被执行, 没有回滚, 


###  事务相关指令 Multi、Exec、discard

组队，执行，丢弃

1. 图示
    ```mermaid
    flowchart LR
        A([MULTI<br/>开启事务]) --> B[命令1入队]
        B --> C[命令2入队]
        C --> D[...]
        D --> E[命令N入队]

        E --> F{如何处理事务?}

        F -->|EXEC| G[按顺序执行队列中的命令]
        G --> H[命令1执行]
        H --> I[命令2执行]
        I --> J[...]
        J --> K[命令N执行]
        K --> L([事务结束])

        F -->|DISCARD| M[清空事务队列]
        M --> N([放弃事务])
    ```

2. 解读
    - 从输入 Multi 命令开始，输入的命令都会依次进入命令队列中，但不会执行(类似 Mysql的 start transaction 开启事务)
    - 输入 Exec 后，Redis 会将之前的命令队列中的命令依次执行(类似 Mysql 的 commit 提交事务)
    - 组队的过程中可以通过 discard 来放弃组队(类似 Mysql 的 rollback 回顾事务)


### 快速入门

1. 需求: 请依次向 Redis 中, 添加三组数据, k1-v1 k2-v2 k3-v3, 要求使用 Redis 的事务完成

2. 实现
    ```bash
    127.0.0.1:6379> multi
    OK
    127.0.0.1:6379(TX)> set m1 m1
    QUEUED
    127.0.0.1:6379(TX)> set m2 m2
    QUEUED
    127.0.0.1:6379(TX)> set m3 m3
    QUEUED
    127.0.0.1:6379(TX)> excute
    (error) ERR unknown command 'excute', with args beginning with:
    127.0.0.1:6379(TX)> exec
    (error) EXECABORT Transaction discarded because of previous errors.
    127.0.0.1:6379> discard
    (error) ERR DISCARD without MULTI
    127.0.0.1:6379> multi
    OK
    127.0.0.1:6379(TX)> set m1 m1
    QUEUED
    127.0.0.1:6379(TX)> set m2 m2
    QUEUED
    127.0.0.1:6379(TX)> set m3 m3
    QUEUED
    127.0.0.1:6379(TX)> exec
    1) OK
    2) OK
    3) OK
    127.0.0.1:6379>
    ```

3. 解决方案
    在事务输入过程中，如果你输入了错误命令导致提示 `(error)`，此时想取消事务重来，可以使用 **`DISCARD`** 命令清空事务队列，而不是直接敲 `EXEC`（因为敲 `EXEC` 只会返回 `EXECABORT` 让你白忙活）。
    ```bash
    127.0.0.1:6379> MULTI
    OK
    127.0.0.1:6379(TX)> SET m1 m1
    QUEUED
    127.0.0.1:6379(TX)> excute   # 啊，手滑打错了
    (error) ERR unknown command
    127.0.0.1:6379(TX)> DISCARD  # 直接丢弃，清空队列
    OK
    127.0.0.1:6379> MULTI       # 重新开启干净的事务
    ...
    ```

4.  注意事项和细节
    - 组队的过程中, 可以通过 discard 来放弃组队
    - 如果在组队阶段报错, 会导致 exec 失败, 那么事务的所有指令都不会被执行
    - 如果组队成功, 但是指令有不能正常执行的, 那么 exec 提交, 会出现有成功有失败情况,也就是事务得到部分执行, 这种情况下, Redis 事务不具备原子性.


### 事务冲突及解决方案

#### 引例

1. 先看一个经典的抢票问题
    - 一个请求想购买 6
    - 一个请求想购买 5
    - 一个请求想购买 1

    ```mermaid
    flowchart LR
        S["10 张票"]

        S -->|"① 查询得到 10"| A["if 10 > 6<br/>then -6"]
        S -->|"② 查询得到 10"| B["if 10 > 5<br/>then -5"]
        S -->|"③ 查询得到 10"| C["if 10 > 1<br/>then -1"]

        A -->|"-6"| R1["4"]
        R1 -->|"-5"| R2["-1"]
        R2 -->|"-1"| R3["最终值：-2"]

        B -.-> R2
        C -.-> R3
    ```
2. 解读
    - 如果没有控制, 会造成超卖现象
    - 如果 3 个指令, 都得到执行, 最后剩余的票数是 -2

#### 悲观锁

1. 图解
    ```mermaid
    flowchart LR
        S["🔒 10 张票"]

        A["① 查询得到 10"]
        A1["if 10 > 6<br/>then -6"]

        B["② 请求访问库存"]
        B1["被阻塞"]

        R1["🔒 4 张票"]

        B2["② 获得锁后<br/>查询得到 4"]
        B3["if 4 > 5<br/>then -5"]

        R2["🔒 4 张票"]

        C["③ 继续处理<br/>第三个请求"]

        S --> A
        A --> A1
        A1 -->|"-6"| R1

        S -. "当前已加锁" .-> B
        B -->|block| B1
        B1 -->|"等待锁释放"| R1

        R1 --> B2
        B2 --> B3
        B3 -->|"4 < 5，不执行扣减"| R2

        C --> R2
    ```

1) 悲观锁(Pessimistic Lock), 顾名思义，就是很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁
2) 这样别人/其它请求想拿这个数据就会 block 直到它拿到锁。
3) 悲观锁是锁设计理念, 传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁. 


#### 乐观锁

1. 图解
    ```mermaid
    flowchart LR
        S["初始库存<br/>10 张票<br/>version = v1.0"]

        A["请求 1 读取<br/>库存 = 10<br/>version = v1.0"]
        B["请求 2 读取<br/>库存 = 10<br/>version = v1.0"]

        A1["判断：10 >= 6<br/>准备购买 6 张"]
        B1["判断：10 >= 5<br/>准备购买 5 张"]

        C1{"提交时检查版本<br/>当前 version 是否仍为 v1.0?"}
        C2{"提交时检查版本<br/>当前 version 是否仍为 v1.0?"}

        R1["修改成功<br/>库存：10 → 4<br/>version：v1.0 → v1.1"]

        F["修改失败<br/>v1.0 ≠ v1.1"]

        R2["重新读取<br/>库存 = 4<br/>version = v1.1"]

        J{"4 >= 5 ?"}

        N["库存不足<br/>不执行扣减"]

        E["最终库存<br/>4 张票<br/>version = v1.1"]

        S --> A
        S --> B

        A --> A1
        B --> B1

        A1 --> C1
        C1 -->|"是"| R1

        B1 --> C2
        R1 --> C2

        C2 -->|"否"| F
        F --> R2
        R2 --> J
        J -->|"否"| N

        R1 --> E
        N --> E
    ```

1) 乐观锁(Optimistic Lock), 顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁
2) 但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。
3) 乐观锁适用于多读的应用类型，这样可以提高吞吐量。Redis 就是利用这种 check-and-set机制实现事务的
3) 乐观锁是锁设计理念

#### watch & unwatch

1. watch
    - 基本语法: `watch key [key ...]`

    - 在执行 multi 之前，先执行 `watch key1 [key2]`,可以监视一个(或多个) key ，如果在事务执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断. 

    - 这里可以结合乐观锁机制进行理解. 


2.  unwatch
    - 基本语法 `unwatch`
    - 取消 watch 命令对所有 key 的监视。
    - 如果在执行 watch 命令后，exec 命令或 discard 命令先被执行了的话，那么就不需要再执行 unwatch 了

## 火车票抢票

### 需求分析

1. 不能出现复购，每名用户只能买一张

2. 不能出现超购，库存不能为负数

3. 不能出现遗留，需要把票卖光


### 【老技术】Jedis连接池


1. 连接池介绍
    节省每次连接 redis 服务带来的消耗，把连接好的实例反复利用。

2. 链接池参数
    - `MaxTotal`：控制一个 pool 可分配多少个 jedis 实例，通过 pool.getResource()来获取；如果赋值为-1，则表示不限制
    - `maxIdle`：控制一个 pool 最多有多少个状态为 idle(空闲)的 jedis 实例
    - `MaxWaitMillis`：表示当获取一个 jedis 实例时，最大的等待毫秒数，如果超过等待时间，则直接抛 JedisConnectionException
    - `testOnBorrow`：获得一个 jedis 实例的时候是否检查连接可用性（ping()）；如果为 true，则得到的 jedis 实例均是可用的

3. 创建
    ```java
    public class JedisPoolUtil {
        private static volatile JedisPool jedisPool = null;
        private JedisPoolUtil() {
        }
        public static JedisPool getJedisPoolInstance() {
            if (null == jedisPool) {
                synchronized (JedisPoolUtil.class) {
                    if (null == jedisPool) {
                        JedisPoolConfig poolConfig = new JedisPoolConfig();
                        poolConfig.setMaxTotal(200);
                        poolConfig.setMaxIdle(32);
                        poolConfig.setMaxWaitMillis(100 * 1000);
                        poolConfig.setBlockWhenExhausted(true);
                        poolConfig.setTestOnBorrow(true);
                        jedisPool = new JedisPool(poolConfig, "localhost", 6379, 60000);
                    }
                }
            }
            return jedisPool;
        }
        public static void release(Jedis jedis) {
            if (null != jedis) {
                jedis.close();
            }
        }
    }
    ```
    - 通过双重检查锁`synchronized`、not null、`valatile`关键字，确保不会重复创建
    - `valatile`关键字用于确保单例模式线程正确初始化，跳过工作内存强制读取主内存。
        不同线程默认访问线程的工作内存而非主内存，需要强制修改共享变量从主内存读取，防止出现幻觉
        不同的线程会因指令重排出现错误的结果

4. 执行实例
    ```java
    public static boolean doSecKill(String uid, String ticketNo) {
        //1 uid 和 ticketNo 非空判断
        if (uid == null || ticketNo == null) {
            return false;
        }
        //2.连接到 redis
        //
        //1) 每一个来秒杀的用户, 都会连接一把 Reids
        // Jedis jedis = new Jedis("localhost", 6379);

        JedisPool jedisPoolInstance = JedisPoolUtil.getJedisPoolInstance();
        Jedis jedis = jedisPoolInstance.getResource();

        //3 拼接 key
        // 3.1 库存 key
        String stockKey = "sk:" + ticketNo + ":ticket";
        // 3.2 秒杀成功用户 key=> 对应的值是 set , 可以存放有多个秒杀成功用户的 id
        String userKey = "sk:" + ticketNo + ":user";

        //监视库存
        jedis.watch(stockKey);

        //4 获取库存，如果库存 null，秒杀还没有开始
        String stock = jedis.get(stockKey);
        if (stock == null) {
            System.out.println("秒杀还没有开始，请等待..");
            jedis.close();
            return false;
        }
        // 5 判断用户是否重复秒杀操作
        if (jedis.sismember(userKey, uid)) {
            System.out.println(uid + " 不能重复秒杀...");
            jedis.close();
            return false;
        }
        //6 判断如果火车票数量，剩余数量小于 1，秒杀结束
        if (Integer.parseInt(stock) <= 0) {
            System.out.println("票已经卖光, 秒杀已经结束了");
            jedis.close();
            return false;
        }


        // //7 秒杀过程
        // //使用事务
        // Transaction multi = jedis.multi();
        // //组队操作
        // multi.decr(stockKey);
        // multi.sadd(userKey, uid);
        // //执行
        // List<Object> results = multi.exec();

        //7.1 火车票数量 - 1
        jedis.decr(stockKey);
        //7.2 把秒杀成功用户添加清单里面
        jedis.sadd(userKey, uid);
        System.out.println("秒杀成功了..");
        jedis.close();
        return true;
    }
    ```
   

### LUA脚本

1. 简介
    上面的例子中，通过乐观锁可以避免超卖的问题，但又引出了新的问题：当用户同时访问一个有大量库存的票仓，有可能会因为乐观锁的数据版本频繁变换，导致同一批次的人大量买不到票，虽然避免的超卖，但同时把真实需求拦了出去导致卖不完。

2. LUA 介绍
    - Lua 是一个小巧的脚本语言，Lua 脚本可以很容易的被 C/C++ 代码调用，也可以反过来调用 C/C++的函数，Lua 并没有提供强大的库，一个完整的 Lua 解释器不过 200k，所以 Lua 不适合作为开发独立应用程序的语言，而是作为嵌入式脚本语言。
    - 很多应用程序、游戏使用 LUA 作为自己的嵌入式脚本语言，以此来实现可配置性、可扩展性。
    - 将复杂的或者多步的 Redis 操作，写为一个脚本，一次提交给 redis 执行，减少反复连接 redis 的次数。提升性能。
    - LUA 脚本是类似 Redis 事务，有一定的原子性，不会被其他命令插队，可以完成一些 redis 事务性的操作
    - Redis 的 lua 脚本功能，只有在 Redis 2.6 以上的版本才可以使用
    - 通过 lua 脚本解决争抢问题，实际上是 Redis 利用其单线程的特性，用任务队列的方式解决多任务并发问题

3. 图示
    ```mermaid
    flowchart LR

        U1[1号用户请求]
        U2[2号用户请求]
        U3[3号用户请求]

        U1 --> Q
        U2 --> Q
        U3 --> Q

        Q[Redis任务队列<br/>单线程串行执行]

        Q --> L3


        subgraph Redis_Lua[Redis Lua脚本]
            L1["1号用户<br/>检查库存<br/>库存-1"]
            L2["2号用户<br/>检查库存<br/>库存-1"]
            L3["3号用户<br/>检查库存<br/>库存-1"]
        end

        L3 --> L2
        L2 --> L1
        L1 --> STOCK

        STOCK[(剩余票数:600)]
    ```


4.  LUA 脚本
    - LUA 脚本是类似 Redis 事务，有一定的原子性，不会被其他命令插队，能完成 Redis事务性的操作
    - 通过 lua 脚本解决争抢问题，Redis 利用其单线程的特性，将请求形成任务队列, 从而解决多任务并发问题

    ```lua
    local userid=KEYS[1]; -- 获取传入的第一个参数
    local ticketno=KEYS[2]; -- 获取传入的第二个参数
    local stockKey='sk:'..ticketno..:ticket; -- 拼接 stockKey
    local usersKey='sk:'..ticketno..:user; -- 拼接 usersKey
    local userExists=redis.call(sismember,usersKey,userid); -- 查看在 redis 的usersKey set 中是否有该用户
    if tonumber(userExists)==1 then
    return 2; -- 如果该用户已经购买, 返回 2
    end
    local num= redis.call("get" ,stockKey); -- 获取剩余票数
    if tonumber(num)<=0 then
    return 0; -- 如果已经没有票, 返回 0
    else
    redis.call("decr",stockKey); -- 将剩余票数-1
    redis.call("sadd",usersKey,userid); -- 将抢到票的用户加入 set
    end
    return 1 -- 返回 1 表示抢票成功
    ```
    - 参考文档: https://blog.csdn.net/qq_41286942/article/details/124161359


2. 创 建

    ```java
    public class SecKillRedisByLua {
        /**
        * 老师说明
        * 1. 这个脚本字符串是在 lua 脚本上修改的, 但是要注意不完全是字符串处理
        * 2. 比如 : 这里我就使用了 \" , 还有换行使用了 \r\n
        * 3. 这些都是细节，如果你直接包 lua 脚本粘贴过来，不好使,一定要注意细节
        * 4. 如果写的不成功，就在老师这个代码上修改即可
        */
        static String secKillScript = "local userid=KEYS[1];\r\n" +
                "local ticketno=KEYS[2];\r\n" +
                "local stockKey='sk:'..ticketno..\":ticket\";\r\n" +
                "local usersKey='sk:'..ticketno..\":user\";\r\n" +
                "local userExists=redis.call(\"sismember\",usersKey,userid);\r\n" +
                "if tonumber(userExists)==1 then \r\n" +
                " return 2;\r\n" +
                "end\r\n" +
                "local num= redis.call(\"get\" ,stockKey);\r\n" +
                "if tonumber(num)<=0 then \r\n" +
                " return 0;\r\n" +
                "else \r\n" +
                " redis.call(\"decr\",stockKey);\r\n" +
                " redis.call(\"sadd\",usersKey,userid);\r\n" +
                "end\r\n" +
                "return 1";

        public static boolean doSecKill(String uid, String ticketNo) throws IOException {
            JedisPool jedispool = JedisPoolUtil.getJedisPoolInstance();
            Jedis jedis = jedispool.getResource();
            String sha1 = jedis.scriptLoad(secKillScript);
            //Redis Evalsha 命令根据给定的 sha1 校验码，执行缓存在服务器中的脚本
            // 这里的2是参数数量
            Object result = jedis.evalsha(sha1, 2, uid, ticketNo);
            String reString = String.valueOf(result);
            if ("0".equals(reString)) {
                System.err.println("票已经卖光..");
            } else if ("1".equals(reString)) {
                System.out.println("购票成功..");
            } else if ("2".equals(reString)) {
                System.err.println("不能重复购买..");
            } else {
                System.err.println("购票失败..");
            }
            jedis.close();
            return true;
        }
    }
    ```
3. 修 改
    ```java
    public class SecKillServlet extends HttpServlet {
        protected void doPost(HttpServletRequest request, HttpServletResponse response) throws
                ServletException, IOException {
            //请求时, 模拟生成一个 userId
            String userId = new Random().nextInt(10000) + "";
            //获取用户要购买的票的编号
            String ticketNo = request.getParameter("ticketNo");
            //调用秒杀
            //boolean isOK = SecKillRedis.doSecKill(userId, ticketNo);
            //使用 lua 完成秒杀
            boolean isOK = SecKillRedisByLua.doSecKill(userId, ticketNo);
            //返回结果
            response.getWriter().print(isOK);
        }
        protected void doGet(HttpServletRequest request, HttpServletResponse response) throws
                ServletException, IOException {
            doPost(request, response);
        }
    }
    ```




## 主从复制

### 简介

1. 单个 Redis 的问题
    - 多客户端操作同一Redis，会对Redis由较大的压力

2. 图解

    ```mermaid
    flowchart TB

        APP[应用程序]

        subgraph Redis主从复制
            M[(Master<br/>主节点<br/>写)]

            S1[(Slave<br/>从节点1<br/>读)]
            S2[(Slave<br/>从节点2<br/>读)]
            S3[(Slave<br/>从节点3<br/>读)]
        end


        APP -->|写请求| M

        APP -->|读请求| S1
        APP -->|读请求| S2
        APP -->|读请求| S3


        M -->|数据复制| S1
        M -->|数据复制| S2
        M -->|数据复制| S3
    ```

3. 解析
    - 上图描述了主机数据更新后, 自动同步到备机的 master/slaver 机制
    - Master 以写为主，Slaver 以读为主
    - 好处: 读写分离, 提升效率 (理解: 读写分离后, 将读和写操作分布到不同的 Reids, 减少单个 Redis 的压力, 提升效率)
    - 好处: 容灾快速恢复 (理解: 如果某个 slaver , 不能正常工作, 可以切换到另一个 slaver)
    - 主从复制, 要求是 1 主多从, 不能有多个 Master( 理解: 如果有多个主服务器 Master,那么 slaver 不能确定和哪个 Master 进行同步, 出现数据紊乱)
    - 要解决主服务器的高可用性, 可以使用 Redis 集群
    - 可通过`slaveof <master_ip> <master_port>`设置从服务器，但新版本建议使用`REPLICAOF NO ONE`等命令



###  搭建一主多从

1. Docker创建一主多从
    - 创建网络
        ```powershell
        (base) PS C:\Users\Lenovo> docker network create redis-net
        6b19ee7792a6a0a2a5ecabbec1916b268c7e5e1188c1d37b4bb2d276326553d1
        ```
    - 创建主节点
        ```powershell
        docker run -d `
        --name redis-master `
        --restart unless-stopped `
        --network redis-net `
        -p 127.0.0.1:6379:6379 `
        -v redis-master-data:/data `
        redis:7.2-alpine `
        redis-server --appendonly yes
        ```

    - 创建从节点1
        ```powershell
        docker run -d `
        --name redis-slave1 `
        --restart unless-stopped `
        --network redis-net `
        -p 127.0.0.1:6380:6379 `
        -v redis-slave1-data:/data `
        redis:7.2-alpine `
        redis-server --appendonly yes --replicaof redis-master 6379
        ```

    - 创建从节点2
        ```powershell
        docker run -d `
        --name redis-slave2 `
        --restart unless-stopped `
        --network redis-net `
        -p 127.0.0.1:6381:6379 `
        -v redis-slave2-data:/data `
        redis:7.2-alpine `
        redis-server --appendonly yes --replicaof redis-master 6379
        ```
    - `--network redis-net`：把容器插到名叫 redis-net 的虚拟网线（网络）上。作用：让同网络的容器可以通过容器名（即 redis-master）互相访问，而不用查 IP。
    - `redis-server --replicaof redis-master 6379`：这是从节点最关键的一行（覆盖了默认启动命令）。
        - 默认情况下，Redis 镜像启动时会直接跑 redis-server。
        - 这里我们追加了参数，告诉 Redis 容器：启动后，立即把自己变成从节点（replica），去找名叫 redis-master 的容器，连接它的 6379 端口，并同步数据。
        （注：replicaof 是 Redis 7 的官方术语，旧版叫 slaveof）

2. 演示
    - master
        ```powershell
        (base) PS C:\Users\Lenovo> docker exec -it redis-master redis-cli
        127.0.0.1:6379> keys *
        (empty array)
        127.0.0.1:6379> set k1 aaa
        OK
        ```
    - slave1
        ```powershell
        (base) PS C:\Users\Lenovo> docker exec -it redis-slave1 redis-cli
        127.0.0.1:6379> keys *
        1) "k1"
        127.0.0.1:6379>
        ```
    - slave2
        ```powershell
        (base) PS C:\Users\Lenovo>  docker exec -it redis-slave2 redis-cli
        127.0.0.1:6379> keys *
        (empty array)
        127.0.0.1:6379> set k1 aaa
        (error) READONLY You can't write against a read only replica.
        127.0.0.1:6379> keys *
        1) "k1"
        127.0.0.1:6379>127.0.0.1:6379> info replication
        # Replication
        role:slave
        master_host:redis-master
        master_port:6379
        master_link_status:up
        master_last_io_seconds_ago:3
        master_sync_in_progress:0
        slave_read_repl_offset:1411
        slave_repl_offset:1411
        slave_priority:100
        slave_read_only:1
        replica_announced:1
        connected_slaves:0
        master_failover_state:no-failover
        master_replid:0f273d09491bbd41316983e34b7fc71dc0240530
        master_replid2:0000000000000000000000000000000000000000
        master_repl_offset:1411
        second_repl_offset:-1
        repl_backlog_active:1
        repl_backlog_size:1048576
        repl_backlog_first_byte_offset:29
        repl_backlog_histlen:1383
        127.0.0.1:6379>
        ```
        

### 主从复制-原理

1. 主从复制流程
    - Slave 启动成功连接到 master 后会发送一个 sync 命令
    - Master 接到命令启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令， 在后台进程执行完毕之后, master 将传送整个数据文件到 slave,以完成一次完全同步
    - slave 服务在接收到数据库文件数据后，将其存盘并加载到内存中, 即 **全量复制**
    - Master 数据变化了, 会将新的收集到的修改命令依次传给 slave, 完成同步, 即 **增量复制**
    - 但是只要是重新连接 master,一次完全同步（全量复制)将被自动执行


2.  一主二仆
    - 如果从服务器 down 了, 重新启动, 仍然可以获取 Master 的最新数据
    - 如果主服务器 down 了, 从服务器并不会抢占为主服务器, 当主服务器恢复后, 从服务器仍然指向原来的主服务器.



3. 薪火相传
    ```mermaid
    flowchart LR

        M[(Master)]

        S1[(Slave1)]

        S2[(Slave2)]


        M -->|复制| S1

        S1 -->|复制| S2
    ```
    - 上一个 Slave 可以是下一个 slave 的 Master，Slave 同样可以接收其他 slaves 的连接和同步请求，那么该 slave 作为了链条中下一个的 master, 可以有效减轻 master 的写压力,去中心化降低风险
    - 用 `slaveof <master_ip> <master_port>`
    - 风险是一旦某个 slave 宕机，后面的 slave 都没法同步
    - 主机挂了，从机还是从机，无法写数据了


4.  反客为主
    - 在薪火相传的结构下, 当一个 master 宕机后, 指向 Master 的 slave 可以升为 master, 其后面的 slave 不用做任何修改
    - 用 `slaveof no one` 将从机变为主机 (后面可以使用哨兵模式, 自动完成切换.)
    - 新版建议使用`REPLICAOF NO ONE`
    - 反客为主不需要主节点宕机，但是不建议在主节点未宕机时强行提升从节点地位


### 哨兵模式(sentinel)

1. 图解
    ```mermaid
    flowchart LR

        APP[应用程序]

        M[(原 Master<br/>故障)]
        
        S1[(Slave1)]
        S2[(Slave2<br/>升级为新 Master)]

        Sentinel[Sentinel<br/>哨兵<br/>监控与故障转移]


        M -->|主从复制| S1
        M -->|主从复制| S2


        Sentinel -.监控.-> M

        M -.故障.-> X[Master宕机]


        Sentinel -->|故障检测| X

        Sentinel -->|选举Slave升级| S2

        S2 -->|成为新的Master| S1

        APP -->|写请求| S2
        APP -->|读请求| S1
    ```

2. 哨兵模式: 反客为主的自动版，能够后台监控主机是否故障，如果故障了根据投票数自动将从库转换为主库


3. 配置
    ```conf
    sentinel monitor redis_master 127.0.0.1 6379 1
    ```
    说明: 
    - redis_master 为监控对象起的服务器名称
    - 1 表示至少有多少个哨兵同意迁移的数量，表示只要有1个哨兵同意迁移就可以切换
    - 哨兵的端口是 26379
    - 当主机挂掉，从机选举中产生新的主机
    - 如果原来的主机重启, 会自动成为从机


4. 注意事项和细节
    哨兵如何在从机中, 推选新的 Master 主机, 选择的条件依次为:
    - 优先级在 redis.conf 中默认：`replica-priority 100`，值越小优先级越高
    - 偏移量是指获得原主机数据的量, 数据量最全的优先级高
    - 每个 redis 实例启动后都会随机生成一个 40 位的 runid, 值越小优先级越高



#### Docker 搭建 Redis Sentinel

1. 为什么 Sentinel 需要单独容器？

    Sentinel 不是 Redis 节点。

    它是：

    > 一个特殊的 Redis 进程，负责监控 Redis 实例。

    所以：

    ```
    Redis Server

    redis-server


    Sentinel

    redis-sentinel
    ```

    是两个程序。

2. 创建 Sentinel 配置文件

    目录：

    例如：

    ```
    redis-sentinel/

    ├── sentinel1.conf
    ├── sentinel2.conf
    └── sentinel3.conf
    ```

    配置

    sentinel1.conf：

    ```conf
    port 26379

    sentinel monitor redis-master redis-master 6379 2

    sentinel down-after-milliseconds redis-master 5000

    sentinel failover-timeout redis-master 10000

    sentinel parallel-syncs redis-master 1
    ```

2. 解释：

    - monitor

        重点：

        ```conf
        sentinel monitor redis-master redis-master 6379 2
        ```

        格式：

        ```
        sentinel monitor <名字> <master地址> <端口> <投票数>
        ```

        你的：

        ```
        redis-master
        ```

        第一个：

        是给这个主节点取名字。

        第二个：

        ```
        redis-master
        ```

        是 Docker 网络中的容器名。

        `6379`：Redis端口。

        `2`表示投票：至少两个 Sentinel 认为 Master挂了，才执行切换。

    - down-after

        ```conf
        sentinel down-after-milliseconds redis-master 5000
        ```

        5秒没有响应：

        认为：

        ```
        主观下线
        ```

    - failover-timeout

        ```conf
        sentinel failover-timeout redis-master 10000
        ```

        故障转移超时时间。

    - parallel-syncs

        ```conf
        sentinel parallel-syncs redis-master 1
        ```

        一次有几个slave同步新master。

3. 创建 Sentinel 容器

    sentinel1

    ```powershell
    docker run -d `
    --name redis-sentinel1 `
    --restart unless-stopped `
    --network redis-net `
    -p 26379:26379 `
    -v ./sentinel1.conf:/etc/redis/sentinel.conf `
    redis:7.2-alpine `
    redis-sentinel /etc/redis/sentinel.conf
    ```

    

    sentinel2：

    ```powershell
    docker run -d `
    --name redis-sentinel2 `
    --restart unless-stopped `
    --network redis-net `
    -p 26380:26379 `
    -v ./sentinel2.conf:/etc/redis/sentinel.conf `
    redis:7.2-alpine `
    redis-sentinel /etc/redis/sentinel.conf
    ```

    sentinel3：

    ```powershell
    docker run -d `
    --name redis-sentinel3 `
    --restart unless-stopped `
    --network redis-net `
    -p 26381:26379 `
    -v ./sentinel3.conf:/etc/redis/sentinel.conf `
    redis:7.2-alpine `
    redis-sentinel /etc/redis/sentinel.conf
    ```

4. 为什么推荐三个 Sentinel？

    因为 Sentinel 自己也需要高可用。

    一个：

    ```
    Sentinel
    |
    Master
    ```

    如果 Sentinel 挂：

    没人判断。

    生产：

    ```
    Sentinel1
    Sentinel2
    Sentinel3
    ```

    投票：

    ```
    quorum=2
    ```

    表示：

    至少两个 Sentinel 同意。

5. 查看 Sentinel 状态

    进入：

    ```powershell
    docker exec -it redis-sentinel1 redis-cli -p 26379
    ```

    查看：

    ```redis
    sentinel masters
    ```

    看到：

    ```
    redis-master
    ```    

    查看slave：

    ```redis
    sentinel slaves redis-master
    ```

    
    查看 Sentinel：

    ```redis
    sentinel sentinels redis-master
    ```

6. 测试故障转移

    停止 Master：

    ```powershell
    docker stop redis-master
    ```

    等待几秒。

    查看：

    进入 Sentinel：

    ```redis
    sentinel get-master-addr-by-name redis-master
    ```

    结果：

    例如：

    ```
    1) "redis-slave1"
    2) "6379"
    ```

    说明：

    slave1已经成为Master。

    ---

    启动原Master：

    ```powershell
    docker start redis-master
    ```

    它不会重新成为Master。

    而是：

    ```
    原Master

            ↓

    新的Slave
    ```
    > 如果原来的主机重启，会自动成为从机


## 集群

### 简介

1. 生产环境的实际需求和问题
    - 容量不够，redis 如何进行扩容？
    - 并发写操作， redis 如何分摊？
    - 主从模式，薪火相传模式，主机宕机，会导致 ip 地址发生变化，应用程序中配置需要修改对应的主机地址、端口等信息


2. 传统解决方案-代理主机来解决
    ```mermaid
    flowchart TB

        Client[客户端请求]

        ProxyM[(代理主机<br/>Master)]
        ProxyS[(代理从机<br/>Slave)]

        subgraph ClusterA[业务节点A]
            A_M[(A主机<br/>Master)]
            A_S[(A从机<br/>Slave)]
        end

        subgraph ClusterB[业务节点B]
            B_M[(B主机<br/>Master)]
            B_S[(B从机<br/>Slave)]
        end

        subgraph ClusterC[业务节点C]
            C_M[(C主机<br/>Master)]
            C_S[(C从机<br/>Slave)]
        end


        Client --> ProxyM

        ProxyM --> A_M
        ProxyM --> B_M
        ProxyM --> C_M


        A_M --> A_S
        B_M --> B_S
        C_M --> C_S


        ProxyM --> ProxyS
    ```
    - 客户端请求先到代理服务器
    - 由代理服务器进行请求转发到对应的业务处理服务器
    - 为保证高可用，代理层和 Redis 节点通常都需要部署冗余节点。
    - 随着 Redis 节点数量增加，代理模式部署和维护成本较高。




3. redis3.0 提供解决方案-无中心化集群配置
    ```mermaid
    flowchart LR

        Client[客户端请求]

        subgraph RedisCluster[Redis Cluster 无中心化集群]

            subgraph NodeA[A节点]
                AM[(A主机<br/>Master)]
                AS[(A从机<br/>Slave)]

                AM --> AS
            end


            subgraph NodeB[B节点]
                BM[(B主机<br/>Master)]
                BS[(B从机<br/>Slave)]

                BM --> BS
            end


            subgraph NodeC[C节点]
                CM[(C主机<br/>Master)]
                CS[(C从机<br/>Slave)]

                CM --> CS
            end

        end


        Client --> AM
        Client --> BM
        Client --> CM


        AM <-->|节点通信| BM
        BM <-->|节点通信| CM
        CM <-->|节点通信| AM
    ```


    - 各个 Redis 服务仍然采用主从结构
    - 各个 Redis 服务是连通的, 任何一台服务器, 都可以作为请求入口
    - 各 Redis 节点通过 Cluster Bus 进行通信，客户端可以连接任意节点。（**MOVE重定向**）
    - 当请求访问的数据不属于当前节点时，当前节点返回目标节点信息，由客户端重新访问。
    - 这种方式, 就无中心化集群配置, 可以看到，只需要 6 台服务器即可搞定
    - 无中心化集群配置, 还会根据 key 值, 计算 slot , 把数据分散到不同的主机, 从而缓解单个主机的存取压力.
        - Redis Cluster共有16384个哈希槽。
        - 每个Master负责一部分slot。
        - 客户端根据key计算slot，决定访问哪个节点。
    - Redis 推荐使用无中心化集群配置
    - 在实际生成环境 各个 Redis 服务器, 应当部署到不同的机器(防止机器宕机, 主从复制失效)

3.  集群介绍
    - Redis 集群实现了对 Redis 的水平扩容，即启动 N 个 redis 节点，将整个数据库分布存储，在这 N 个节点中，每个节点存储总数据的 1/N。
        - Redis Cluster通过水平扩容，将数据按照slot分布到多个Master节点。
        - 每个节点负责部分数据，而不是简单平均划分。
    - Redis 集群通过分区（partition）来提供一定程度的可用性（availability）： 即使集群中有一部分节点失效或者无法进行通讯， 集群也可以继续处理命令请求

### 集群搭建

集群搭建

1. Redis Cluster 核心配置

    ```conf
    # 开启集群模式
    cluster-enabled yes

    # Redis自动维护的节点配置文件
    cluster-config-file nodes.conf

    # 节点失联超时时间
    cluster-node-timeout 15000
    ```

    * `cluster-enabled yes`：开启 Redis Cluster 模式。
    * `cluster-config-file`：保存节点 ID、主从关系、Slot 分配等集群信息，由 Redis 自动维护。
    * `cluster-node-timeout`：节点失联判断时间，超过指定时间后可触发集群故障判断和主从切换。

2. Docker 启动 6 个 Redis 节点

    * 创建网络

        ```powershell
        docker network create redis-cluster-net
        ```

    * 目录结构

        ```text
        redis-cluster/
        ├── node1/
        │   └── redis.conf
        ├── node2/
        │   └── redis.conf
        ├── node3/
        │   └── redis.conf
        ├── node4/
        │   └── redis.conf
        ├── node5/
        │   └── redis.conf
        └── node6/
            └── redis.conf
        ```

    * node1 配置

        ```conf
        port 7001

        bind 0.0.0.0
        protected-mode no

        # Cluster
        cluster-enabled yes
        cluster-config-file nodes.conf
        cluster-node-timeout 15000

        # 持久化
        appendonly yes

        # 对外公布的集群地址
        cluster-announce-ip 192.168.198.130
        cluster-announce-port 7001
        cluster-announce-bus-port 17001
        ```

    * 其他节点只需对应修改：

        ```text
        node1：7001 / 17001
        node2：7002 / 17002
        node3：7003 / 17003
        node4：7004 / 17004
        node5：7005 / 17005
        node6：7006 / 17006
        ```

    * node1 启动示例

        ```powershell
        docker run -d `
        --name redis-node1 `
        --restart unless-stopped `
        --network redis-cluster-net `
        -p 7001:7001 `
        -p 17001:17001 `
        -v ./node1/redis.conf:/etc/redis/redis.conf `
        -v redis-node1-data:/data `
        redis:7.2-alpine `
        redis-server /etc/redis/redis.conf
        ```

    其他 5 个节点按照对应端口启动。

3. 将 6 个节点组成一个 Cluster

    6 个 Redis 启动后，**此时只是 6 个开启 Cluster 模式的独立节点**，还没有真正组成集群。

    只需要执行一次：

    ```powershell
    redis-cli --cluster create `
    192.168.198.130:7001 `
    192.168.198.130:7002 `
    192.168.198.130:7003 `
    192.168.198.130:7004 `
    192.168.198.130:7005 `
    192.168.198.130:7006 `
    --cluster-replicas 1
    ```

    其中：

    ```text
    --cluster-replicas 1
    ```

    表示：

    > 每个 Master 配置 1 个 Replica。

    最终形成：

    ```text
    Master1 ── Replica1
    Master2 ── Replica2
    Master3 ── Replica3
    ```

    同时 Redis 自动将：

    ```text
    0 ~ 16383
    ```

    共 `16384` 个 Slot 分配给 3 个 Master。

4. 集群方式连接

    ```powershell
    redis-cli -c -h 192.168.198.130 -p 7001
    ```

    其中：

    `-c`表示开启 Cluster 重定向支持。

    查看集群：`CLUSTER NODES`
    查看集群状态：`CLUSTER INFO`

1. 流程总结

    ```text
    redis.conf
        ↓
    让每个Redis“具备Cluster能力”

    docker run × 6
        ↓
    得到6个独立Cluster节点

    redis-cli --cluster create（只执行一次）
        ↓
    建立节点关系 + 主从关系 + 分配Slot

    最终得到真正的Redis Cluster
    ```

    注意：Redis Cluster 和主从/Sentinel不同：

    * Sentinel：多个 Redis 实例 + Sentinel进程
    * Cluster：Redis实例自身开启 `cluster-enabled yes`

    所以不需要额外 Sentinel。


### Redis 集群使用

1. 什么是 slots
    - 一个 Redis 集群包含 16384 个插槽（hash slot），编号从 0-16383, Reids 中的每个键都属于这 16384 个插槽的其中一个
    - 集群使用公式 `CRC16(key) % 16384` 来计算键 key 属于哪个槽， 其中 CRC16(key) 语句用于计算键 key 的 CRC16 校验和


    - 集群中的每个节点负责处理一部分插槽。 
    
2. 在集群中录入值
    - 在 redis 每次录入、查询键值，redis 都会计算出该 key 应该送往的插槽，如果不是该客户端对应服务器的插槽，redis 会告知应前往的 redis 实例地址和端口。
    - redis-cli 客户端提供了 –c 参数实现自动重定向。
    - 如 `redis-cli -c –p 6379` 登入后，再录入、查询键值对可以自动重定向
    - 不在一个 slot 下的键值，是不能使用 mget,mset 等多键操作
    - 可以通过HashTag`{}`来定义组的概念，从而使 key 中{}内相同内容的键值对放到一个 slot 中去


3. 查询集群中的值
    -  `CLUSTER KEYSLOT <key>` 返回 key 对应的 slot 值
    -  `CLUSTER COUNTKEYSINSLOT <slot>` 返回 slot 有多少个 key
    -  `CLUSTER GETKEYSINSLOT <slot><count>` 返回 count 个 slot 槽中的键

### 集群故障恢复


1. 如果主节点下线, 从节点会自动升为主节点(注意 15 秒超时, 再观察比较准确)
2. 主节点恢复后，主节点回来变成从机
3. 如果所有某一段插槽的主从节点都宕掉，Redis 服务是否还能继续, 要根据不同的配置而言
    - 如果某一段插槽的主从都挂掉，而 `cluster-require-full-coverage` 为 yes ，那么 ，整个集群都挂掉
    - 如果某一段插槽的主从都挂掉，而 `cluster-require-full-coverage` 为 no , 那么, 只是该插槽数据不能使用，也无法存储


### 集群的 Jedis 开发

1. JedisCluster 的作用

   * 普通 `Jedis` 连接的是某一个 Redis 实例。
   * `JedisCluster` 面向整个 Redis Cluster，可以根据 Key 对应的 Slot，自动找到真正负责该 Slot 的 Master。
   * 即使最开始连接的节点并不负责当前 Key，JedisCluster 也可以根据 Cluster 返回的 `MOVED` 信息重新访问正确节点。

    ```mermaid
    flowchart TB

        APP[业务代码<br/>jedisCluster.set / get]

        JC[JedisCluster]

        SLOT[计算Key对应Slot]

        ROUTE[查询Cluster节点拓扑]

        M1[(Master1<br/>Slot 0~5460)]
        M2[(Master2<br/>Slot 5461~10922)]
        M3[(Master3<br/>Slot 10923~16383)]

        APP --> JC
        JC --> SLOT
        SLOT --> ROUTE

        ROUTE --> M1
        ROUTE --> M2
        ROUTE --> M3
    ```

2. Cluster 中的数据不是每台 Master 都有一份

   例如：

   ```text
   Master A：Slot 0 ~ 5460
   Master B：Slot 5461 ~ 10922
   Master C：Slot 10923 ~ 16383
   ```

   执行：

   ```java
   jedisCluster.set("user:1", "Tom");
   ```

   假设：

   ```text
   user:1
      ↓
   Slot 8000
      ↓
   Master B
   ```

   那么这条数据主要存储在：

   ```text
   Master B
      ↓
   Replica B
   ```

   而不是：

   ```text
   Master A 也有
   Master B 也有
   Master C 也有
   ```


   > 无论客户端从哪个 Cluster 节点进入，JedisCluster 都可以根据 Slot 信息找到真正保存该 Key 的节点，因此从客户端使用体验上看，可以直接通过 JedisCluster 访问整个集群。

3. Master 和 Replica 的作用

   * Master 负责 Slot，并处理对应数据的写操作。
   * Replica 保存对应 Master 的数据副本，主要用于故障恢复。
   * **不要简单记成“Master写、Replica读”。**

   在 Redis Cluster 中，JedisCluster 默认主要根据 Slot 定位对应 Master 完成普通读写，并不是简单地：

   ```text
   SET → Master
   GET → Replica
   ```

   Replica 读属于另外的读副本策略，不是这段代码自动实现的。

4. JedisCluster 示例

    ```java
    import redis.clients.jedis.HostAndPort;
    import redis.clients.jedis.JedisCluster;

    import java.util.HashSet;
    import java.util.Set;

    public class JedisClusterDemo {

        public static void main(String[] args) {

            Set<HostAndPort> nodes = new HashSet<>();

            // 添加一个或多个Cluster入口节点
            nodes.add(new HostAndPort("192.168.198.130", 7001));
            nodes.add(new HostAndPort("192.168.198.130", 7002));
            nodes.add(new HostAndPort("192.168.198.130", 7003));

            JedisCluster jedisCluster = new JedisCluster(nodes);

            jedisCluster.set("k1", "v1");

            String value = jedisCluster.get("k1");

            System.out.println("k1 = " + value);

            jedisCluster.close();
        }
    }
    ```

    这里最重要的是：

    ```java
    Set<HostAndPort> nodes = new HashSet<>();
    ```

    它不是表示：

    > 我要同时操作这几个 Redis。

    而是给 `JedisCluster` 几个**集群入口地址**。

    例如：

    ```text
    7001
    7002
    7003
    ```

    只要其中一个节点能够连接，JedisCluster 就可以向它获取整个 Cluster 的 Slot 拓扑。

    随后内部大致变成：

    ```text
    JedisCluster启动
        ↓
    连接7001
        ↓
    获取Cluster拓扑
        ↓
    知道：

    Slot 0~5460       → 7001
    Slot 5461~10922   → 7002
    Slot 10923~16383  → 7003
    ```

    以后执行：

    ```java
    jedisCluster.set("k1", "v1");
    ```

    JedisCluster 会：

    ```text
    k1
    ↓
    计算Slot
    ↓
    假设Slot=9000
    ↓
    查拓扑
    ↓
    9000属于7002
    ↓
    直接访问7002
    ```

    所以你业务代码根本不用写：

    ```java
    if(slot属于7001) {
        ...
    } else if(slot属于7002) {
        ...
    }
    ```

    这个就是 `JedisCluster` 最大的价值。

    另外，课程说：

    > 集群相关端口都要打开，否则 `No more cluster attempts left`

    这个思路是对的。

    因为客户端可能：

    ```text
    先连接7001
        ↓
    发现数据在7003
        ↓
    转去7003
    ```

    如果：

    ```text
    7003端口没开放
    ```

    客户端就无法完成重定向。

    所以 Cluster 客户端必须能够访问集群中返回给它的节点地址和客户端端口。

    你这节最值得记的一句话是：

    > **JedisCluster = 帮 Java 客户端维护 Redis Cluster 拓扑，并根据 Key 的 Slot 自动访问真正负责该数据的 Master。**

    而不是：

    > “所有 Redis 主机都有相同数据，所以连哪台都能读。”

    后者其实是**主从全量副本思维**，和 Redis Cluster 的**分片思维**正好相反。







### Redis 集群的优缺点
1. 优点
    - 实现扩容
    - 分摊压力
    - 无中心配置相对简单
1. 缺点
    - 多键操作是不被支持的
    - 多键的 Redis 事务是不被支持的。lua 脚本不被支持
    - 由于集群方案出现较晚，很多公司已经采用了其他的集群方案，而其它方案想要迁移至 redis cluster，需要整体迁移而不是逐步过渡，复杂度较大

## Redis应用问题与解决方案

### 缓存穿透

1. 示意图
    ```mermaid
    flowchart LR
        U["👤 用户"]
        B["🌐 浏览器"]
        W["☁️ Web 服务"]
        R[("🔴 Redis<br/>缓存")]
        D[("🟠 数据库")]

        U -->|"① 访问"| B
        B -->|"② 请求 Web 服务"| W
        W -->|"③ 访问缓存<br/>获取不到数据"| R
        W -->|"④ 查询数据库<br/>数据库中也不存在该数据"| D
        D -.->|"⑤ 无法同步到缓存"| R
    ```

2. 原因
    - key对应的数据在数据源（如MySQL）不存在，每次针对key的请求从缓存获取不到，请求会压到数据源，极端情况下会压垮数据源。
    - 如：获取不存在的用户ID的用户信息，此时Redis作为缓存的功能就会失效。请求还是会压力数据库

3. 后果
    - 应用服务器压力变大
    - Redis命中率降低

4. 解决方案
    - 对空值缓存：对空结果null进行缓存。设置空结果的过期时间应该短一些，不建议超过5分钟
    - 设置一个可访问的白名单，每次和白名单比较，不在白名单内的用户不允许访问。如：使用bitmap
    - 布隆过滤器
        布隆过滤器用于检索一个元素是否存在于集合中，空间效率和查询时间优于一般算法，缺点是有一定的误识别率且删除困难

    - 实时监控
        当发现Redis命中急剧降低，手动排查、设置黑名单。

### 缓存击穿


1. 示意图

    ```mermaid
    flowchart LR
        U["👤 用户"]
        B["🌐 浏览器"]
        W["☁️ Web 服务"]
        R[("🔴 Redis<br/>缓存")]
        D[("🟠 数据库")]

        U -->|"① 访问"| B
        B -->|"② 请求 Web 服务"| W
        W -->|"③ 访问缓存<br/>某一热点 Key 过期"| R
        W -->|"④ 瞬时大量请求<br/>直接访问数据库"| D
    ```

2. 原因
    - key对应数据存在，但在Redis中过期，此时如果有大量并发请求，还是会把请求打到数据库
    - 某个热点数据在某些时间点被超高并发访问，容易出现缓存击穿

3. 后果
    - 数据库访问压力瞬间增大
    - Redis正常运行，但是数据库已经挂掉了

4. 解决方案
    - 预先设置热门数据
        在Redis高峰访问之前，把热门数据提前存入Redis，并加大这些key的过期时间
    - 实时调整key过期时间
    - 锁
        抢到锁的干活，去查数据库，其他请求休眠一定时间，再去查Redis，去拿抢到锁的请求的劳动成果。
        这可以避免批量返回错误信息。
        ```mermaid
        flowchart TD
            A["查询 Redis<br/>GET k1"]
            B["返回结果为空"]
            C{"设置排它锁<br/>SET key_mutex 1 EX 180 NX"}
            D["查询数据库<br/>同步到缓存<br/>删除排它锁<br/>SET k1 v1"]
            E["排它锁已被其他线程获取<br/>其他线程正在查询数据库<br/>并同步缓存"]
            F["睡眠一段时间<br/>再次查询 Redis"]

            A --> B
            B --> C

            C -->|"成功"| D
            C -->|"失败"| E

            E --> F
            F --> A
        ```

### 缓存雪崩


1. 示意图（正常情况）

    ```mermaid
    flowchart LR
        U["👤 用户"]
        B["🌐 浏览器"]

        subgraph S["Web 服务集群"]
            direction TB
            N["Nginx"]

            subgraph W[" "]
                direction LR
                W1["☁️ Web 服务1"]
                W2["☁️ Web 服务2"]
                W3["☁️ Web 服务3"]
            end
        end

        R[("🔴 Redis<br/>缓存")]
        D[("🟠 数据库")]

        U -->|"① 访问"| B
        B -->|"② 请求 Web 服务"| N

        N -->|"③ 转发"| W1
        N -.-> W2
        N -.-> W3

        W1 -->|"④ 访问缓存"| R
        W2 -.-> R
        W3 -.-> R

        D -->|"定期更新缓存"| R
    ```

2. 示意图（缓存雪崩）

    ```mermaid
    flowchart LR
        U["👤 用户"]
        B["🌐 浏览器"]

        subgraph S["Web 服务集群"]
            direction TB
            N["Nginx"]

            subgraph W[" "]
                direction LR
                W1["☁️ Web 服务1"]
                W2["☁️ Web 服务2"]
                W3["☁️ Web 服务3"]
            end
        end

        R[("🔴 Redis<br/>缓存")]
        D[("🟠 数据库")]

        U -->|"① 访问"| B
        B -->|"② 请求 Web 服务"| N

        N -->|"③ 转发"| W1
        N -.-> W2
        N -.-> W3

        W1 -->|"④ 访问缓存<br/>大量 Key 同时过期"| R
        W2 -.-> R
        W3 -.-> R

        W1 -->|"⑤ 缓存未命中<br/>查询数据库"| D
        W2 -.->|"大量请求"| D
        W3 -.->|"大量请求"| D

        D -.->|"数据库压力过大<br/>缓存更新失败"| R
    ```

3. 缓存雪崩的原因

    - key对应的数据存在，但在redis中过期，此时若有大量并发请求过来，这些请求发现缓存过期一般都会从后端DB加载数据并回设到缓存
    - 这个时候大并发的请求可能会瞬间把后端DB压垮。
    - 缓存雪崩与缓存击穿的区别在于这里针对很多key缓存，前者则是某一个key

4. 缓存雪崩的现象/表象
    - 数据库访问压力变大，服务器崩溃
    - 在极短时间内，访问大量Key，而这些Key集中过期

2. 解决方案
    - 构建多级缓存架构
        nginx缓存 + redis缓存 + 其他缓存（ehcache等），这种方式开发/维护成本较高
    - 使用锁或队列
        用加锁或者队列的方式保证来保证不会有大量的线程对数据库一次性进行读写，从而避免失效时大量的并发请求落到底层存储系统上。不适用高并发情况
    - 设置过期标志更新缓存
        记录缓存数据是否过期，如果过期会触发通知，另外线程在后台更新缓存。
    - 【经典考点】**给 Key 的过期时间增加随机值，避免大量 Key 同时过期。** 这是缓存雪崩解决的第一梯队。

### 分布式锁

1. 问题描述

    - 单体单机部署的系统被演化成分布式集群系统后
    - 由于分布式系统多线程、多进程并且分布在不同机器上，这将使原单机部署情况下的并发控制锁策略失效
    - 单纯的Java API并不能提供分布式锁的能力
    - 为了解决这个问题就需要一种跨JVM的互斥机制来控制共享资源的访问，这就是分布式锁要解决的问题
    - 换句话说，就是把java进程的锁转化为所有进程共用的公共锁，避免跨进程争抢

2. 示意图（单体结构项目）
    ```mermaid
    flowchart TD
        R1["请求A"]
        R2["请求B"]
        R3["请求C"]

        P["单体项目"]

        L["🔒 锁"]

        B["业务代码"]

        R1 --> P
        R2 --> P
        R3 --> P

        P --> L
        L -->|"只有获得锁的请求才能进入"| B
    ```

3. 分布式项目
    ```mermaid
    flowchart TD
        R1["请求A"]
        R2["请求B"]
        R3["请求C"]

        L["🔒 分布式锁"]

        subgraph P["分布式 / 集群项目"]
            direction LR

            subgraph M1["业务子项目 / 子模块1"]
                B1["业务"]
            end

            subgraph M2["业务子项目 / 子模块2"]
                B2["业务"]
            end

            B1 -->|"OpenFeign 远程调用"| B2
        end

        R1 --> L
        R2 --> L
        R3 --> L

        L -->|"请求A获得锁"| B1
        L -.->|"其他请求等待"| B2
    ```

4. 主流实现方案
    - 基于数据库实现分布式锁
    - 基于缓存
    - 基于zookeeper

    - 优缺点
        - redis性能最高
        - zookeeper最可靠


#### 分布式锁实现

1. 依靠的指令
    ```text
    set lock:1 100 nx ex 100
    ```
    - 如果设定成功，则上锁成功（返回`1`）
    - 设置过期时间防止死锁

2. 原理
    ```mermaid
    flowchart TD
        I1["index1"]
        I2["index2"]
        I3["index3"]

        R["Redis<br/>通过 SETNX('lock', xxx)"]

        T{"返回 true"}
        F{"返回 false"}

        B["业务逻辑"]

        I1 --> R
        I2 --> R
        I3 --> R

        R -->|"获取到锁"| T
        R -->|"没有获取到锁"| F

        T --> B

        F -->|"等待重试"| R
    ```

3. 分布式锁组件

    ```java
    package com.lcq.redis.lock;

    import org.springframework.data.redis.core.StringRedisTemplate;
    import org.springframework.data.redis.core.script.DefaultRedisScript;
    import org.springframework.stereotype.Component;

    import java.time.Duration;
    import java.util.Collections;
    import java.util.Optional;
    import java.util.UUID;
    import java.util.concurrent.ThreadLocalRandom;

    @Component
    public class RedisDistributedLock {

        private static final String LOCK_PREFIX = "lock:";

        private static final DefaultRedisScript<Long> UNLOCK_SCRIPT =
                new DefaultRedisScript<>(
                        """
                        if redis.call('get', KEYS[1]) == ARGV[1] then
                            return redis.call('del', KEYS[1])
                        else
                            return 0
                        end
                        """,
                        Long.class
                );

        private final StringRedisTemplate redisTemplate;

        public RedisDistributedLock(StringRedisTemplate redisTemplate) {
            this.redisTemplate = redisTemplate;
        }

        /**
         * 尝试获取分布式锁
         *
         * @param businessKey 业务锁标识，例如 stock:1001
         * @param waitTime    最长等待时间
         * @param leaseTime   锁自动过期时间
         */
        public Optional<LockToken> tryLock(
                String businessKey,
                Duration waitTime,
                Duration leaseTime) {

            String lockKey = LOCK_PREFIX + businessKey;
            String lockValue = UUID.randomUUID().toString();

            long deadline = System.nanoTime() + waitTime.toNanos();

            while (true) {

                Boolean success = redisTemplate.opsForValue()
                        .setIfAbsent(lockKey, lockValue, leaseTime);

                if (Boolean.TRUE.equals(success)) {
                    return Optional.of(
                            new LockToken(lockKey, lockValue)
                    );
                }

                if (System.nanoTime() >= deadline) {
                    return Optional.empty();
                }

                try {
                    // 加一点随机抖动，避免大量线程同时再次抢锁
                    long sleepMillis =
                            ThreadLocalRandom.current()
                                    .nextLong(30, 81);

                    Thread.sleep(sleepMillis);

                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                    return Optional.empty();
                }
            }
        }

        /**
         * 释放锁
         */
        public boolean unlock(LockToken token) {
            
            Long result = redisTemplate.execute(
                    UNLOCK_SCRIPT,                              // script
                    Collections.singletonList(token.key()),     // KEY[]
                    token.value()                               // ARGV[]
            );

            return Long.valueOf(1L).equals(result);
        }

        /**
         * 表示某一次加锁获得的凭证
         */
        public record LockToken(
                String key,
                String value
        ) {
        }
    }
    ```

1. 业务层
    ```java
    package com.lcq.product.service;

    import com.lcq.redis.lock.RedisDistributedLock;
    import org.springframework.stereotype.Service;

    import java.time.Duration;

    @Service
    public class StockService {

        private final RedisDistributedLock distributedLock;
        private final StockTransactionService transactionService;

        public StockService(
                RedisDistributedLock distributedLock,
                StockTransactionService transactionService) {

            this.distributedLock = distributedLock;
            this.transactionService = transactionService;
        }

        public void deductStock(Long skuId, Integer quantity) {

            String businessKey = "stock:" + skuId;

            var lockToken = distributedLock
                    .tryLock(
                            businessKey,
                            Duration.ofSeconds(2),
                            Duration.ofSeconds(10)
                    )
                    .orElseThrow(
                            () -> new IllegalStateException(
                                    "系统繁忙，请稍后重试"
                            )
                    );

            try {

                transactionService.deduct(
                        skuId,
                        quantity
                );

            } finally {

                distributedLock.unlock(lockToken);
            }
        }
    }
    ```


#### 分布式锁生命周期解析

1. 生成锁信息

   * 根据业务资源生成锁 Key，例如：`lock:stock:1001`
   * 生成唯一 Owner，例如 UUID，用于标识“这把锁是谁的”

2. 获取锁

   * 执行：

     ```text
     SET lock:stock:1001 UUID-A NX EX 30
     ```
   * `NX`：保证只有一个 JVM 能获得锁，实现互斥
   * `EX`：设置过期时间，防止 JVM 宕机后锁永久存在
   * 获取失败的请求等待、重试，超过等待时间后返回失败

3. 执行业务

   * 获得锁后进入临界区，执行扣库存、修改订单等共享资源操作
   * 其他 JVM 因无法获得同一把锁，不能同时执行该业务

4. Watchdog 自动续期

   * 如果业务执行时间不确定，可以使用 Watchdog
   * 业务未结束且锁仍属于当前线程时，后台定期延长锁的 TTL
   * 作用：防止业务还没执行完，锁却因为 TTL 到期提前释放
   * 如果 JVM 宕机，Watchdog 停止续期，锁最终会因 TTL 自动过期

5. 业务结束释放锁

   * 一般在 `finally` 中释放，保证业务发生异常时也会执行解锁
   * 不能直接 `DEL lock`
   * 必须先判断 Redis 中的 Owner 是否仍然等于自己的 UUID，防止误删其他线程后来获得的新锁

6. Lua 安全解锁

   * 使用 Lua 将“判断 Owner + 删除锁”作为一个原子操作：

     ```lua
     if redis.call('get', KEYS[1]) == ARGV[1] then
         return redis.call('del', KEYS[1])
     else
         return 0
     end
     ```
   * Owner 相同：删除自己的锁
   * Owner 不同：说明锁已经过期并被别人获得，不能删除
   * 整个生命周期可以记为：

   ```text
   生成 Key + Owner
        ↓
   SET NX EX 获取锁
        ↓
   执行业务
        ↓
   Watchdog 必要时续期
        ↓
   finally 解锁
        ↓
   Lua 判断 Owner + DEL
   ```

一句话记忆：

**NX 管“谁能拿锁”，TTL 管“锁最终会失效”，Watchdog 管“锁别提前失效”，UUID 管“锁是谁的”，Lua 管“安全释放锁”。**

### 


## Redis 新功能


1. ACL 简介

   * ACL（Access Control List），即**访问控制列表**
   * Redis 6.0 开始引入
   * 可以创建多个 Redis 用户，并分别控制：

     * 用户是否启用
     * 用户密码
     * 可以访问哪些 Key
     * 可以执行哪些命令
   * 默认用户为：`default`

2. `ACL LIST`：查看所有用户及其权限配置

   ```text
   ACL LIST
   ```

   例如：

   ```text
   user default on nopass ~* &* +@all
   ```

   含义：

   ```text
   default     用户名
   on          用户启用
   nopass      不需要密码
   ~*          可以访问所有 Key
   +@all       可以执行所有类别的命令
   ```

3. `ACL CAT`：查看命令类别

   ```text
   ACL CAT
   ```

   可以查看 Redis 支持的权限类别，例如：

   ```text
   string
   hash
   list
   set
   sortedset
   connection
   admin
   dangerous
   ...
   ```

   指定类别：

   ```text
   ACL CAT string
   ```

   表示查看 `string` 类别包含哪些命令，例如：

   ```text
   get
   set
   mget
   mset
   incr
   ...
   ```

4. `ACL WHOAMI`：查看当前登录用户

   ```text
   ACL WHOAMI
   ```

   例如返回：

   ```text
   default
   ```

5. `ACL USERS`：查看所有用户名

   ```text
   ACL USERS
   ```

   例如：

   ```text
   default
   lcq
   admin
   ```

6. `ACL GETUSER`：查看指定用户的详细权限

   ```text
   ACL GETUSER lcq
   ```

   可以查看：

   ```text
   flags       用户状态
   passwords   密码信息
   commands    命令权限
   keys        Key权限
   ```

7. `ACL SETUSER`：创建或修改用户

   基本格式：

   ```text
   ACL SETUSER 用户名 权限规则...
   ```

   例如创建用户 `lcq`：

   ```text
   ACL SETUSER lcq on >123456 ~* +@all
   ```

   含义：

   ```text
   lcq        用户名

   on         启用用户

   >123456    设置密码为 123456

   ~*         允许访问所有 Key

   +@all      允许执行所有类别的命令
   ```

   之后可以使用：

   ```text
   AUTH lcq 123456
   ```

   登录该用户。

8. 用户启用与禁用

   启用：

   ```text
   ACL SETUSER lcq on
   ```

   禁用：

   ```text
   ACL SETUSER lcq off
   ```

   `off` 后用户存在，但不能登录。

9. 设置密码

   设置密码：

   ```text
   ACL SETUSER lcq >123456
   ```

   `>` 表示：

   ```text
   添加一个密码
   ```

   删除指定密码：

   ```text
   ACL SETUSER lcq <123456
   ```

   设置为无需密码：

   ```text
   ACL SETUSER lcq nopass
   ```

   清除已有密码：

   ```text
   ACL SETUSER lcq resetpass
   ```

10. 设置 Key 访问范围

    ```text
    ~pattern
    ```

    表示允许用户访问满足 pattern 的 Key。

    例如允许访问所有 Key：

    ```text
    ~*
    ```

    只允许访问 `user:` 开头的 Key：

    ```text
    ACL SETUSER lcq ~user:*
    ```

    此时：

    ```text
    user:1      ✅
    user:100    ✅

    order:1     ❌
    ```

11. 设置命令权限

    `+`：增加权限

    ```text
    +GET
    ```

    `-`：删除权限

    ```text
    -DEL
    ```

    例如：

    ```text
    ACL SETUSER lcq +GET +SET -DEL
    ```

    表示允许：

    ```text
    GET
    SET
    ```

    禁止：

    ```text
    DEL
    ```

12. 按命令类别授权

    Redis 将命令分成不同类别，可以使用：

    ```text
    +@类别
    -@类别
    ```

    例如允许所有字符串命令：

    ```text
    +@string
    ```

    禁止所有危险命令：

    ```text
    -@dangerous
    ```

    例如：

    ```text
    ACL SETUSER lcq on >123456 ~user:* +@string -@dangerous
    ```

    表示：

    ```text
    用户：lcq
    密码：123456

    只能访问：
    user:*

    允许：
    string 类命令

    禁止：
    dangerous 类命令
    ```

13. `ACL DELUSER`：删除用户

    ```text
    ACL DELUSER lcq
    ```

    可以一次删除多个：

    ```text
    ACL DELUSER lcq user1 user2
    ```

14. 重置用户权限

    ```text
    ACL SETUSER lcq reset
    ```

    `reset` 会将用户恢复到较初始的权限状态，已有密码、Key规则、命令权限等会被清除，使用时要注意。

15. 常见 ACL 权限符号

    | 配置            | 作用               |
    | ------------- | ---------------- |
    | `on`          | 启用用户             |
    | `off`         | 禁用用户             |
    | `>password`   | 添加密码             |
    | `<password`   | 删除密码             |
    | `nopass`      | 无需密码             |
    | `~*`          | 允许访问所有 Key       |
    | `~user:*`     | 只允许访问指定模式的 Key   |
    | `+GET`        | 允许执行 GET         |
    | `-GET`        | 禁止执行 GET         |
    | `+@string`    | 允许 string 类全部命令  |
    | `-@dangerous` | 禁止 dangerous 类命令 |
    | `+@all`       | 允许所有类别命令         |
    | `-@all`       | 禁止所有命令           |

16. 一个实际例子

    创建一个只允许操作 `product:*` 数据的业务用户：

    ```text
    ACL SETUSER product-service on >123456 ~product:* +GET +SET +DEL
    ```

    那么：

    ```text
    GET product:1001
    ```

    可以执行。

    但是：

    ```text
    GET user:1
    ```

    会因为没有 Key 权限而被拒绝。

    同样：

    ```text
    FLUSHALL
    ```

    因为没有授予该命令权限，也无法执行。

最核心可以记成：

```text
ACL = 用户 + 密码 + Key权限 + 命令权限

on / off       → 用户能不能用
>password      → 用什么密码
~pattern       → 能操作哪些 Key
+/- command    → 能执行哪些命令
+/- @category  → 能执行哪些类别的命令
```

### IO多线程

1. 简介
    - IO多线程是指和客户端交互时，网络IO交互处理模块多线程，而非执行命令多线程
    - Redis6执行命令依然是单线程
    - 也就是说，Redis和客户端的交互是多线程，在执行指令的时候，仍然是单线程+IO多路复用

2. 原理架构
    Redis 6 加入多线程，Redis 的多线程部分只是用来处理网络数据的读写和协议解析，执行命令仍然是单线程。之所以这么设计是不想因为多线程而变得复杂，需要去控制 key、lua、事务，LPUSH/LPOP 等等的并发问题。



    ```mermaid
    flowchart LR
        subgraph IN["客户端请求"]
            P11["Packet"]
            P12["Packet"]
            P21["Packet"]
            P22["Packet"]
            P31["Packet"]
            P32["Packet"]
            PN["..."]
        end

        subgraph R["Redis"]
            direction LR

            IO1["多线程 IO<br/>读取 / 解析网络请求"]
            CMD["命令执行<br/>Execute Command"]
            IO2["多线程 IO<br/>写回响应"]

            IO1 --> CMD
            CMD --> IO2
        end

        subgraph OUT["客户端响应"]
            O11["Packet"]
            O12["Packet"]
            O21["Packet"]
            O22["Packet"]
            O31["Packet"]
            O32["Packet"]
            ON["..."]
        end

        P11 --> P12 --> IO1
        P21 --> P22 --> IO1
        P31 --> P32 --> IO1
        PN --> IO1

        IO2 --> O11 --> O12
        IO2 --> O21 --> O22
        IO2 --> O31 --> O32
        IO2 --> ON
    ```

3. 配置开启

    ```conf
    io-threads 4
    io-threads-do-reads yes
    ```
    * `io-threads 4`

        * 设置 IO 线程数量为 4
        * 包含主线程在内，所以并不是“额外再创建 4 个 IO 线程”
        * 一般建议根据 CPU 核数设置，不是越大越好
        * 线程数过多会增加线程调度和上下文切换开销

    * `io-threads-do-reads yes`

        * 开启多线程读取客户端请求
        * 默认情况下，Redis IO 多线程主要可以参与响应写回
        * 设置为 `yes` 后，读取请求和协议解析也可以交给 IO 线程并行处理



> `io-threads` 增加的是网络 IO 处理能力，不会让 Redis 普通命令变成多线程并行执行。
