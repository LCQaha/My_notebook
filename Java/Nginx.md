# Nginx

## 基本介绍

### 参考资料

[1] [【DunWu】GitHub极简教程](https://github.com/dunwu/nginx-tutorial)

### 基本内容

1. Nginx 在项目上线中一般处于入口位置。典型结构：

    ```text
    用户
    ↓
    域名 DNS
    ↓
    服务器公网 IP
    ↓
    Nginx
    ↓
    前端静态文件 / 后端服务 / 其他服务
    ```

2. 最常用生产配置就是：


    - Nginx 托管前端静态文件
    - Nginx 反向代理后端 Spring Boot
    - Nginx 统一处理 HTTPS

### 应用


Nginx 是一个高性能的 Web 服务器，也常用于反向代理、负载均衡、静态资源托管、HTTPS 证书终止等场景。

常见用途：

1. 静态资源服务器
   例如部署前端项目、图片、CSS、JS 文件。

2. 反向代理服务器
   例如用户访问 `https://example.com/api/user`，Nginx 转发给后端 Spring Boot 服务 `http://127.0.0.1:8080/user`。

3. 负载均衡
   多个后端服务共同处理请求，Nginx 按规则分发流量。

4. HTTPS 入口
   用户访问 HTTPS，Nginx 负责证书和加密，后端服务可以继续使用 HTTP。

5. 网关入口
   在实际项目中，Nginx 常作为整个网站的统一入口，例如：

    ```text
    用户浏览器
    ↓
    Nginx
    ↓
    前端静态文件 / 后端 API / 文件服务 / 管理后台
    ```


## 快速入门

### Nginx 默认配置

1. 刚安装好的Nginx的目录结构如下（Ubuntu）：
    ```text
    /etc/nginx/
    ├── nginx.conf                  # 主配置文件，启动时默认读取
    ├── conf.d/                     # 通用子配置目录，*.conf 会被加载
    ├── sites-available/
    │   └── default                 # 默认站点配置，存放配置模板/源文件
    ├── sites-enabled/
    │   └── default -> ../sites-available/default
    │                                # 已启用站点配置，软链接
    ├── modules-enabled/            # 已启用模块配置
    ├── mime.types                  # 文件类型映射
    ├── snippets/                   # 可复用配置片段
    ├── fastcgi.conf
    ├── fastcgi_params
    ├── proxy_params
    ├── scgi_params
    └── uwsgi_params
    ```

2. 运行`nginx -V`可以得到详细信息
    ```bash
    lcq@lcq-VMware-Virtual-Platform:/etc/nginx/modules-enabled$ nginx -V
    nginx version: nginx/1.24.0 (Ubuntu)
    built with OpenSSL 3.0.13 30 Jan 2024
    TLS SNI support enabled
    configure arguments: --with-cc-opt='-g -O2 -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer -ffile-prefix-map=/build/nginx-XlYRYC/nginx-1.24.0=. -flto=auto -ffat-lto-objects -fstack-protector-strong -fstack-clash-protection -Wformat -Werror=format-security -fcf-protection -fdebug-prefix-map=/build/nginx-XlYRYC/nginx-1.24.0=/usr/src/nginx-1.24.0-2ubuntu7.12 -fPIC -Wdate-time -D_FORTIFY_SOURCE=3' --with-ld-opt='-Wl,-Bsymbolic-functions -flto=auto -ffat-lto-objects -Wl,-z,relro -Wl,-z,now -fPIC' --prefix=/usr/share/nginx --conf-path=/etc/nginx/nginx.conf --http-log-path=/var/log/nginx/access.log --error-log-path=stderr --lock-path=/var/lock/nginx.lock --pid-path=/run/nginx.pid --modules-path=/usr/lib/nginx/modules --http-client-body-temp-path=/var/lib/nginx/body --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-proxy-temp-path=/var/lib/nginx/proxy --http-scgi-temp-path=/var/lib/nginx/scgi --http-uwsgi-temp-path=/var/lib/nginx/uwsgi --with-compat --with-debug --with-pcre-jit --with-http_ssl_module --with-http_stub_status_module --with-http_realip_module --with-http_auth_request_module --with-http_v2_module --with-http_dav_module --with-http_slice_module --with-threads --with-http_addition_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_secure_link_module --with-http_sub_module --with-mail_ssl_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-stream_realip_module --with-http_geoip_module=dynamic --with-http_image_filter_module=dynamic --with-http_perl_module=dynamic --with-http_xslt_module=dynamic --with-mail=dynamic --with-stream=dynamic --with-stream_geoip_module=dynamic
    ```
    - `--conf-path=/etc/nginx/nginx.conf`：这是不指定配置文件时，Nginx使用的默认配置的位置

### 配置文件结构

1. Ubuntu 中常见的 Nginx 配置文件位置：

    ```text
    /etc/nginx/nginx.conf              # Nginx 主配置文件
    /etc/nginx/conf.d/                 # 额外配置目录
    /etc/nginx/sites-available/        # 可用站点配置
    /etc/nginx/sites-enabled/          # 已启用站点配置
    /var/log/nginx/access.log          # 访问日志
    /var/log/nginx/error.log           # 错误日志
    ```

2. 常见关系：

    ```text
    nginx.conf
    ↓ include
    conf.d/*.conf
    sites-enabled/*
    ↓ 软链接
    sites-available/具体站点配置
    ```

3. Ubuntu 默认习惯是：

    ```text
    sites-available：存放站点配置
    sites-enabled：启用后的站点配置
    ```

4. 启用站点本质上通常是创建软链接：

    ```bash
    sudo ln -s /etc/nginx/sites-available/my-site /etc/nginx/sites-enabled/
    ```

### 安装 Nginx

1. Ubuntu 安装：

    ```bash
    sudo apt update
    sudo apt install nginx -y
    ```

1. 查看版本：

    ```bash
    nginx -v
    ```

1. 启动 Nginx：

    ```bash
    sudo systemctl start nginx
    ```

1. 设置开机自启：

    ```bash
    sudo systemctl enable nginx
    ```

1. 查看运行状态：

    ```bash
    sudo systemctl status nginx
    ```

1. 重启 Nginx：

    ```bash
    sudo systemctl restart nginx
    ```

1. 重新加载配置，不中断已有连接：

    ```bash
    sudo systemctl reload nginx
    ```

1. 停止 Nginx：

    ```bash
    sudo systemctl stop nginx
    ```

1. 测试配置文件是否正确：

    ```bash
    sudo nginx -t
    ```

1. 常用流程：

    ```bash
    sudo nginx -t
    sudo systemctl reload nginx
    ```

    含义是：先检查配置有没有语法错误，确认没问题后再重新加载。

1. 也可以用如下命令
    ```bash
    nginx -s stop       # 快速关闭Nginx，可能不保存相关信息，并迅速终止web服务。
    nginx -s quit       # 平稳关闭Nginx，保存相关信息，有安排的结束web服务。
    nginx -s reload     # 因改变了Nginx相关配置，需要重新加载配置而重载。
    nginx -s reopen     # 重新打开日志文件。
    nginx -c filename   # 为 Nginx 指定一个配置文件，来代替缺省的。
    nginx -t            # 不运行，仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件。
    nginx -v            # 显示 nginx 的版本。
    nginx -V            # 显示 nginx 的版本，编译器版本和配置参数。
    ```

### 防火墙配置

1. 介绍
    在Ubuntu系统下，防火墙为ufw，如果要正常使用服务，就要经由防火墙释放TCP端口

2. 防火墙基本使用
    - 添加规则
        ```bash
        sudo ufw status verbose     # 查看规则
        sudo ufw status numbered    # 查看规则（带编号）
        ```
    - 移除规则
        ```bash
        sudo ufw delete allow 8080/tcp 
        ```
    - 查看防火墙状态
        ```bash
        sudo ufw status
        ```
        如果显示：
        ```text
        Status: inactive
        ```
        说明 Ubuntu 自带防火墙没有启用。

        如果显示：
        ```text
        Status: active
        ```
        说明已启用防火墙
    - 不启用防火墙查看状态
        ```bash
        sudo cat /etc/ufw/user.rules
        sudo cat /etc/ufw/user6.rules
        ```

3. 防火墙规则
    ```bash
    sudo ufw allow OpenSSH      # 放行22端口，否则ssh无法连接机器
    sudo ufw allow 80/tcp       # 放行80端口，即http协议
    sudo ufw allow 443/tcp      # 放行443端口，即https协议
    sudo ufw enable             # 启动防火墙
    sudo ufw status verbose     # 查看防火墙状态

    sudo ufw all 'Nginx Full'   # 开放网络端口
    ```


### 官方配置

#### 基本示例

1. `nginx.conf`结构

    - `全局块`
        - 作用：配置影响 nginx 全局的指令。
        - 具体包括：如用户组，nginx 进程 pid 存放路径，日志存放路径，配置文件引入，允许生成 worker process 数量等。
    - `events 块`
        - 作用：配置影响 nginx 服务器或与用户的网络连接。
        - 具体包括：如每个进程的最大连接数，选取哪种事件驱动模型处理连接请求，是否允许同时接受多个网络连接，开启多个网络连接序列化等。
    - `http 块`
        - 作用：可以嵌套多个 server，配置代理、缓存、日志定义等绝大多数功能和第三方模块的配置。
        - 具体包括：如文件引入、mime-type 定义，日志自定义，是否使用 sendfile 传输文件，连接超时时间，单连接请求数等。
        - `http 全局块`：如 upstream，错误页面，连接超时等。
        - `server 块`：配置虚拟主机的相关参数，一个 http 中可以有多个 server。
            - `location`：配置请求的路由，以及各种页面的处理情况。

2. 官方示例配置（Ubuntu）
    ```nginx
    # 启用服务的用户
    user www-data;

    # 启动的 worker 进程数
    # auto 表示自动根据 CPU 核心数决定
    worker_processes auto;

    # Nginx 主进程 PID 文件
    # PID 是进程 ID，用于记录当前 Nginx master 进程的编号
    # systemctl stop/reload nginx 时，系统可以通过这个文件找到 Nginx 主进程
    pid /run/nginx.pid;

    # 错误日志位置
    error_log /var/log/nginx/error.log;

    # 引用额外的模块配置
    # Ubuntu 中某些 Nginx 模块会放在这个目录下
    include /etc/nginx/modules-enabled/*.conf;

    events {
            # 单个 worker 进程允许的最大连接数
            worker_connections 768;

            # 是否一次性接收尽可能多的新连接
            # 默认关闭即可
            # multi_accept on;
    }

    http {

            ##
            # Basic Settings
            ##

            # 开启高效文件传输
            # 适合静态文件服务
            sendfile on;

            # 配合 sendfile 使用，优化网络包发送
            tcp_nopush on;

            # MIME 类型哈希表大小
            # 一般保持默认即可
            types_hash_max_size 2048;

            # 是否隐藏 Nginx 版本号
            # 生产环境建议打开
            # server_tokens off;

            # server_name 哈希桶大小
            # 域名很多或域名很长时可能需要调整
            # server_names_hash_bucket_size 64;

            # 是否在重定向时使用 server_name
            # 一般保持默认即可
            # server_name_in_redirect off;

            # 引入 MIME 类型配置
            # 用来告诉浏览器文件类型，例如 html、css、js、png
            include /etc/nginx/mime.types;

            # 默认文件类型
            # 如果文件类型无法识别，就按二进制流处理
            default_type application/octet-stream;

            ##
            # SSL Settings
            ##

            # 允许使用的 TLS 协议版本
            # 旧配置中可能包含 TLSv1 和 TLSv1.1
            # 生产环境一般建议只保留 TLSv1.2 TLSv1.3
            ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;

            # 优先使用服务端指定的加密套件
            ssl_prefer_server_ciphers on;

            ##
            # Logging Settings
            ##

            # 访问日志位置
            access_log /var/log/nginx/access.log;

            ##
            # Gzip Settings
            ##

            # 开启 gzip 压缩
            # 可以减少 html、css、js、json 等文本资源传输体积
            gzip on;

            # 是否在响应头中加入 Vary: Accept-Encoding
            # gzip_vary on;

            # 是否对代理请求启用 gzip
            # gzip_proxied any;

            # gzip 压缩等级，1-9，数字越大压缩越强但 CPU 消耗越高
            # gzip_comp_level 6;

            # gzip 缓冲区设置
            # gzip_buffers 16 8k;

            # 使用 gzip 的 HTTP 版本
            # gzip_http_version 1.1;

            # 指定哪些类型的文件启用 gzip
            # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

            ##
            # Virtual Host Configs
            ##

            # 引入 conf.d 目录下的配置
            include /etc/nginx/conf.d/*.conf;

            # 引入 sites-enabled 目录下的站点配置
            # Ubuntu 默认常用这种方式管理站点
            include /etc/nginx/sites-enabled/*;
    }


    # mail 模块配置
    # 一般 Web 项目用不到
    # 这是用于邮件代理的示例配置
    #mail {
    #       server {
    #               listen     localhost:110;
    #               protocol   pop3;
    #               proxy      on;
    #       }
    #
    #       server {
    #               listen     localhost:143;
    #               protocol   imap;
    #               proxy      on;
    #       }
    #}
    ```

3. Server配置
    - 注意事项
        某些版本的Nginx目录结构与此不同，默认配置文件将所有配置写在同一个文件中，这并非不对，在前面的主配置文件中，通过`include`引入其他配置所在目录，就可以将server等配置单独写出来，不至于改配置时牵一发而动全身。
    - 配置位置
        ```bash
        lcq@lcq-VMware-Virtual-Platform:/etc/nginx/modules-enabled$ ls -l /etc/nginx/sites-enabled/
        总计 0
        lrwxrwxrwx 1 root root 34  6月 22 14:16 default -> /etc/nginx/sites-available/default
        ```

    - 配置详情

        ```nginx
        ##
        # You should look at the following URL's in order to grasp a solid understanding
        # of Nginx configuration files in order to fully unleash the power of Nginx.
        # https://www.nginx.com/resources/wiki/start/
        # https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/
        # https://wiki.debian.org/Nginx/DirectoryStructure
        #
        # In most cases, administrators will remove this file from sites-enabled/ and
        # leave it as reference inside of sites-available where it will continue to be
        # updated by the nginx packaging team.
        #
        # This file will automatically load configuration files provided by other
        # applications, such as Drupal or Wordpress. These applications will be made
        # available underneath a path with that package name, such as /drupal8.
        #
        # Please see /usr/share/doc/nginx-doc/examples/ for more detailed examples.
        ##

        # Default server configuration
        #
        server {
                listen 80 default_server;
                listen [::]:80 default_server;

                # SSL configuration
                #
                # listen 443 ssl default_server;
                # listen [::]:443 ssl default_server;
                #
                # Note: You should disable gzip for SSL traffic.
                # See: https://bugs.debian.org/773332
                #
                # Read up on ssl_ciphers to ensure a secure configuration.
                # See: https://bugs.debian.org/765782
                #
                # Self signed certs generated by the ssl-cert package
                # Don't use them in a production server!
                #
                # include snippets/snakeoil.conf;

                root /var/www/html;

                # Add index.php to the list if you are using PHP
                index index.html index.htm index.nginx-debian.html;

                server_name _;

                location / {
                        # First attempt to serve request as file, then
                        # as directory, then fall back to displaying a 404.
                        try_files $uri $uri/ =404;
                }

                # pass PHP scripts to FastCGI server
                #
                #location ~ \.php$ {
                #       include snippets/fastcgi-php.conf;
                #
                #       # With php-fpm (or other unix sockets):
                #       fastcgi_pass unix:/run/php/php7.4-fpm.sock;
                #       # With php-cgi (or other tcp sockets):
                #       fastcgi_pass 127.0.0.1:9000;
                #}

                # deny access to .htaccess files, if Apache's document root
                # concurs with nginx's one
                #
                #location ~ /\.ht {
                #       deny all;
                #}
        }


        # Virtual Host configuration for example.com
        #
        # You can move that to a different file under sites-available/ and symlink that
        # to sites-enabled/ to enable it.
        #
        #server {
        #       listen 80;
        #       listen [::]:80;
        #
        #       server_name example.com;
        #
        #       root /var/www/example.com;
        #       index index.html;
        #
        #       location / {
        #               try_files $uri $uri/ =404;
        #       }
        #}
        ```

#### 示例解析

1. Ubuntu分层配置示意

   ```text
   /etc/nginx/nginx.conf
       ↓
       include /etc/nginx/conf.d/*.conf
       include /etc/nginx/sites-enabled/*
           ↓
           /etc/nginx/sites-enabled/default
               ↓
               /etc/nginx/sites-available/default
                   ↓
                   server {
                       listen 80;
                       root /var/www/html;
                   }
   ```

#### `server`块常见配置

1. `listen xx [default_server]`

    * 表示当前服务监听指定端口。
    * `xx` 代表监听本机所有 IPv4 地址的某端口，例如：   
        ```nginx
        listen 80;
        ```
    * `xxx.xxx.xxx.xxx:xx` 代表只监听某个 IPv4 地址的某端口，例如： 
        ```nginx
        listen 192.168.1.10:80;
        ```
    * `[::]:xx` 代表监听所有 IPv6 地址的某端口，例如：  
        ```nginx
        listen [::]:80;
        ```
    * `[xxxx:xxxx::xxxx]:xx` 代表监听某个具体 IPv6 地址的某端口，例如： 
        ```nginx
        listen [240c::6666]:80;
        ```
    * `default_server` 代表这是此 `IP + 端口` 下的兜底配置。
    * 相同 `IP + 端口` 只能有一个 `default_server`，否则 Nginx 会启动失败。
    * IPv4 和 IPv6 可以分别有自己的 `default_server`，例如：    
        ```nginx
        listen 80 default_server;
        listen [::]:80 default_server;
        ```

1. `root`

    * 表示当前站点的网页根目录。
    * 例如：

       ```nginx
       root /var/www/html;
       ```
    * 当用户访问：

       ```text
       http://服务器IP/index.html
       ```
    * Nginx 实际会去找：

       ```text
       /var/www/html/index.html
       ```
    * 当用户访问：

       ```text
       http://服务器IP/img/logo.png
       ```
    * Nginx 实际会去找：

       ```text
       /var/www/html/img/logo.png
       ```
    * 简单理解：`root` 会把“请求路径”拼接到“网站根目录”后面。
    * 注意：`root` 只是指定文件目录，不负责转发后端服务。
    * `--prefix=/usr/share/nginx`是默认的拼接路径，用于拼接root中写的相对路径。

1. `alias`
    * 区别于`root` 

    如果你想让：
    ```nginx
    /static/a.css
    ```
    对应到：
    ```nginx
    /var/www/html/a.css
    ```
    那就不应该用 root，而应该用 alias：

    ```nginx
    location /static/ {
        alias /var/www/html/;
    }
    ```

    此时：
    ```nginx
    /static/a.css
    ```
    对应：
    ```nginx
    /var/www/html/a.css
    ```
    简单区别：

    * root  = root + 完整请求路径
    * alias = 用 alias 路径替换 location 前缀

1. `server_name`

    * 表示当前 `server` 块匹配哪些域名。
    * 例如：

       ```nginx
       server_name example.com www.example.com;
       ```
    * 当用户访问 `example.com` 或 `www.example.com` 时，请求会进入这个 `server` 块。
    * 如果用户直接通过 IP 访问，或者访问的域名没有匹配到任何 `server_name`，Nginx 通常会使用对应 `IP + 端口` 下的 `default_server`。
    * 如果没有显式配置 `default_server`，Nginx 通常会把该 `IP + 端口` 下第一个加载的 `server` 当作默认配置。
    * 常见占位写法：

       ```nginx
       server_name _;
       ```
    * `_` 不是特殊语法，只是一个普通名字，通常用来表示“我不关心具体域名，这个 server 用来兜底”。

1. `location`

    * 表示根据 URL 路径匹配不同的处理规则。
    * `location` 匹配的是请求路径，不是磁盘路径。
    * 例如：

        ```nginx
        location / {
            root /var/www/html;
            index index.html;
        }
        ```
    * 表示所有以 `/` 开头的请求都可以进入这个规则。
    * 常见静态网站配置：

        ```nginx
        location / {
            try_files $uri $uri/ /index.html;
        }
        ```
    * 含义是：

        ```text
        先找请求对应的文件
        再找请求对应的目录
        如果都找不到，就返回 /index.html
        ```
    * 这个配置常用于 Vue、React、Ant Design Pro 等前端单页应用，避免刷新页面后出现 404。
    * 常见后端接口代理配置：

        ```nginx
        location /api/ {
            proxy_pass http://127.0.0.1:8080;
        }
        ```
    * 表示所有以 `/api/` 开头的请求都转发给本机 8080 端口的后端服务。
    * 常见匹配方式：

        ```nginx
        location /api/       # 普通前缀匹配
        location = /login    # 精确匹配
        location ~ \.php$    # 正则匹配，区分大小写
        location ~* \.jpg$   # 正则匹配，不区分大小写
        ```
    * 初学阶段最常用的是：

        ```nginx
        location / {
            try_files $uri $uri/ /index.html;
        }

        location /api/ {
            proxy_pass http://127.0.0.1:8080;
        }
        ```

1. `index`

    * 表示默认首页文件。
    * 例如：

       ```nginx
       index index.html index.htm;
       ```
    * 当用户访问目录时，例如：

       ```text
       http://服务器IP/
       ```
    * Nginx 会尝试返回：

       ```text
       /var/www/html/index.html
       ```
    * 如果没有 `index.html`，可能会继续找 `index.htm`。
    * 如果都没有，并且没有开启目录浏览，通常会返回 403。

1. `try_files`

    * 表示按顺序尝试查找文件或目录。
    * 常见写法：

       ```nginx
       try_files $uri $uri/ /index.html;
       ```
    * `$uri` 表示当前请求路径。
    * `$uri/` 表示当前请求路径对应的目录。
    * `/index.html` 表示兜底返回首页。
    * 对前端路由很重要。
    * 例如用户直接访问：

       ```text
       /user/login
       ```
    * 如果服务器上没有真实的：

       ```text
       /var/www/html/user/login
       ```
    * 那么 Nginx 会返回：

       ```text
       /var/www/html/index.html
       ```
    * 然后由前端框架自己根据 `/user/login` 渲染登录页。

1. `proxy_pass`

    * 表示把请求反向代理到另一个服务。
    * 例如：

       ```nginx
       location /api/ {
           proxy_pass http://127.0.0.1:8080;
       }
       ```
    * 用户访问：

       ```text
       http://example.com/api/user/login
       ```
    * Nginx 转发给：

       ```text
       http://127.0.0.1:8080/api/user/login
       ```
    * 如果 `proxy_pass` 后面带 `/`，路径会发生替换。
    * 例如：

       ```nginx
       location /api/ {
           proxy_pass http://127.0.0.1:8080/;
       }
       ```
    * 用户访问：

       ```text
       /api/user/login
       ```
    * 实际转发为：

       ```text
       /user/login
       ```
    * 简单记：

       ```text
       proxy_pass 不带最后的 / ：保留 location 前缀
       proxy_pass 带最后的 / ：替换 location 前缀
       ```

1. `access_log`

    * 表示访问日志位置。
    * 例如：

       ```nginx
       access_log /var/log/nginx/access.log;
       ```
    * 访问日志记录的是谁访问了服务器、访问了什么路径、返回了什么状态码。
    * 排查 404、403、接口请求是否到达 Nginx 时很有用。

1. `error_log`

    * 表示错误日志位置。
    * 例如：

       ```nginx
       error_log /var/log/nginx/error.log;
       ```
    * 错误日志记录 Nginx 启动失败、配置错误、权限错误、后端连接失败等问题。
    * 排查 502、权限不足、配置文件错误时优先看它。



#### 其他参数



### Location专题

#### `location`语法


1. 基本代码块
    ```nginx
    location [匹配修饰符] URI {
        ...
    }
    ```

    例如：

    ```nginx
    location / {
        try_files $uri $uri/ /index.html;
    }

    location /t/ {
        proxy_pass http://127.0.0.1:8080/;
    }

    location = /t {
        return 301 /t/;
    }

    location ~* \.(jpg|png|css|js)$ {
        expires 7d;
    }
    ```

    这里的 `URI` 指的是请求路径，不是完整网址。

    比如访问：

    ```text
    http://192.168.218.132/t/examples/?a=1
    ```

    Nginx 用来匹配 `location` 的主要是：

    ```text
    /t/examples/
    ```

    不是：

    ```text
    http://192.168.218.132
    ```

    也不是：

    ```text
    ?a=1
    ```

#### 几种 location 类型

1. `location = /xxx`：精确匹配

    ```nginx
    location = /t {
        return 301 /t/;
    }
    ```

    只匹配：

    ```text
    /t
    ```

    不匹配：

    ```text
    /t/
    /t/examples/
    /tt
    ```

    所以你现在这个配置：

    ```nginx
    location = /t {
        return 301 /t/;
    }
    ```

    意思是：

    ```text
    用户访问 /t 时，重定向到 /t/
    ```

    为什么要这么做？因为你真正代理 Tomcat 的规则是：

    ```nginx
    location /t/ {
        proxy_pass http://127.0.0.1:8080/;
    }
    ```

    它匹配 `/t/` 开头的路径，不一定匹配单独的 `/t`。

2. `location /xxx/`：普通前缀匹配

    ```nginx
    location /t/ {
        proxy_pass http://127.0.0.1:8080/;
    }
    ```

    匹配所有以 `/t/` 开头的路径：

    ```text
    /t/
    /t/examples/
    /t/manager/html
    /t/host-manager/html
    ```

    不匹配：

    ```text
    /t
    /tt
    /test
    ```

    你现在的：

    ```nginx
    location /t/ {
        proxy_pass http://127.0.0.1:8080/;
    }
    ```

    意思是：

    ```text
    凡是 /t/ 开头的请求，都转发给 Tomcat。
    ```

    并且由于 `proxy_pass` 后面有 `/`：

    ```nginx
    proxy_pass http://127.0.0.1:8080/;
    ```

    所以路径会这样转换：

    ```text
    /t/                  ->  /
    /t/examples/         ->  /examples/
    /t/manager/html      ->  /manager/html
    ```

3. `location /`：普通兜底前缀匹配

    ```nginx
    location / {
        try_files $uri $uri/ /index.html;
    }
    ```

    `/` 是所有 URI 的开头，所以它能匹配：

    ```text
    /
    /login
    /assets/index.js
    /user/list
    /favicon.ico
    ```

    它通常用来做兜底规则。

    比如前端项目常见配置：

    ```nginx
    location / {
        try_files $uri $uri/ /index.html;
    }
    ```

    意思是：

    ```text
    先找真实文件；
    再找真实目录；
    都找不到，就返回 index.html。
    ```

    所以：

    ```text
    /assets/index.js  -> 找真实文件
    /login            -> 找不到真实文件，返回 index.html
    ```

4. `location ~ 正则`：区分大小写的正则匹配

    ```nginx
    location ~ \.php$ {
        fastcgi_pass 127.0.0.1:9000;
    }
    ```

    `~` 表示正则匹配，区分大小写。

    这个例子匹配：

    ```text
    /index.php
    /a/b/test.php
    ```

    不匹配：

    ```text
    /index.PHP
    ```

5. `location ~* 正则`：不区分大小写的正则匹配

    ```nginx
    location ~* \.(jpg|jpeg|png|gif|css|js)$ {
        expires 7d;
    }
    ```

    匹配：

    ```text
    /a.png
    /a.PNG
    /index.js
    /index.JS
    ```

    常用于静态资源缓存。

6. `location ^~ /xxx/`：高优先级前缀匹配

    ```nginx
    location ^~ /assets/ {
        root /var/www/html;
    }
    ```

    `^~` 的意思是：

    ```text
    如果这个前缀匹配成功，就不要再检查正则 location。
    ```

    它不是正则，而是普通前缀匹配的加强版。

    例如：

    ```nginx
    location ^~ /assets/ {
        root /var/www/html;
    }

    location ~* \.js$ {
        return 403;
    }
    ```

    访问：

    ```text
    /assets/index.js
    ```

    会先匹配到：

    ```nginx
    location ^~ /assets/
    ```

    然后停止，不再进入：

    ```nginx
    location ~* \.js$
    ```

    所以不会被 403。

#### location 匹配顺序

1. 先检查精确匹配 location = xxx
   命中就直接用它，结束。

2. 再找普通前缀匹配，包括 location /xxx/ 和 location ^~ /xxx/
   如果有多个，记录“最长前缀”。

3. 如果最长前缀是 ^~，直接用它，不再检查正则。

4. 如果最长前缀不是 ^~，继续按配置文件顺序检查正则 location：
   ~ 和 ~*

5. 正则一旦命中，就用第一个命中的正则 location。

6. 如果所有正则都没命中，就使用第 2 步记录的最长普通前缀。

简单版：

```text
= 最高
^~ 次高，可以阻止正则
~ / ~* 正则按顺序
普通前缀取最长
/ 是兜底
```


### 反向代理实例

1. 场景

    假设后端 Spring Boot 运行在本机 `8080` 端口，前端访问：

    ```text
    http://example.com/api/user/login
    ```

    希望 Nginx 把它转发给：

    ```text
    http://127.0.0.1:8080/api/user/login
    ```

    - 代理指的是隐藏目标信息，我们将向网络发起请求的过程称为正向，将服务器发起响应称为反向。
    - 正向代理就是用一个转发机制掩盖用户信息，比如VPN。
    - 反向代理就是隐藏服务器信息，即Nginx

2. 配置

    ```nginx
    server {
        listen 80;
        server_name example.com;

        # 普通前端静态页面，非重点
        root /var/www/lcq-blog;
        index index.html;

        location / {
            try_files $uri $uri/ /index.html;
        }

        # 后端接口统一走 /api/
        location /api/ {
            proxy_pass http://127.0.0.1:8080;

            # 把用户原始访问的域名传给后端
            proxy_set_header Host $host;

            # 把真实客户端 IP 传给后端
            proxy_set_header X-Real-IP $remote_addr;

            # 记录完整代理链路中的客户端 IP
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

            # 告诉后端用户原始访问协议是 http 还是 https
            proxy_set_header X-Forwarded-Proto $scheme;

            # 常见超时设置，避免后端卡死时 Nginx 无限等待
            proxy_connect_timeout 5s;
            proxy_send_timeout 60s;
            proxy_read_timeout 60s;
        }
    }
    ```

3. 参数解释

    - `location /api/`
        - 表示只有以 `/api/` 开头的请求才进入这个规则。
        - 例如 `/api/user/login` 会匹配，`/assets/a.js` 不会匹配。

    - `proxy_pass http://127.0.0.1:8080;`
        - 表示把请求转发给本机 `8080` 端口。
        - 这个 `8080` 一般是 Spring Boot、Tomcat、Node、Go 服务监听的端口。
        - 用户浏览器并不知道后端真实端口，浏览器只知道访问了 `example.com`。

    - `proxy_set_header Host $host;`
        - 默认情况下，Nginx 转发请求时可能会改变 `Host`。
        - 加上这一行后，后端能知道用户原本访问的是 `example.com`。
        - 某些后端会根据域名生成回调地址、跳转地址，所以这个参数很常用。

    - `proxy_set_header X-Real-IP $remote_addr;`
        - `$remote_addr` 是直接连接 Nginx 的客户端 IP。
        - 后端如果直接读取请求来源 IP，看到的可能只是 Nginx 的 IP。
        - 加上这一行后，后端可以通过 `X-Real-IP` 获取用户真实 IP。

    - `proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;`
        - 记录完整的代理链路。
        - 例如：
            ```text
            用户IP, 第一层代理IP, 第二层代理IP
            ```
        - 如果网站经过 Cloudflare、负载均衡、Nginx 多层转发，这个字段更有用。

    - `proxy_set_header X-Forwarded-Proto $scheme;`
        - `$scheme` 表示用户访问 Nginx 时使用的协议，值一般是 `http` 或 `https`。
        - 后端常用它判断用户原始请求是不是 HTTPS。
        - 如果不传这个字段，后端可能误以为请求是 HTTP，导致生成错误跳转链接。

    - `proxy_connect_timeout 5s;`
        - Nginx 连接后端服务的超时时间。
        - 如果后端端口没开、服务挂了、网络不通，超过这个时间就报错。

    - `proxy_send_timeout 60s;`
        - Nginx 向后端发送请求数据的超时时间。
        - 上传文件、提交大表单时可能受它影响。

    - `proxy_read_timeout 60s;`
        - Nginx 等待后端返回响应的超时时间。
        - 后端接口执行很慢时，超过这个时间可能出现 `504 Gateway Timeout`。

4. `proxy_pass` 路径细节

    这是反向代理里最容易搞错的地方。

    写法一：`proxy_pass` 后面没有 `/`

    ```nginx
    location /api/ {
        proxy_pass http://127.0.0.1:8080;
    }
    ```

    转发结果：

    ```text
    /api/user/login
        -> http://127.0.0.1:8080/api/user/login
    ```

    也就是说，`/api/` 会被保留。

    写法二：`proxy_pass` 后面有 `/`

    ```nginx
    location /api/ {
        proxy_pass http://127.0.0.1:8080/;
    }
    ```

    转发结果：

    ```text
    /api/user/login
        -> http://127.0.0.1:8080/user/login
    ```

    也就是说，匹配到的 `/api/` 会被替换成 `/`。

    选择哪种写法，取决于后端接口本身有没有 `/api` 前缀：

    ```text
    后端接口本来就是 /api/user/login  -> proxy_pass 后面不加 /
    后端接口本来是 /user/login       -> proxy_pass 后面加 /
    ```

5. WebSocket 代理补充

    如果后端有 WebSocket，例如在线聊天、实时通知，需要额外加：

    ```nginx
    location /ws/ {
        proxy_pass http://127.0.0.1:8080;

        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
    ```

    - `proxy_http_version 1.1`：WebSocket 需要 HTTP/1.1 的升级机制。
    - `Upgrade` 和 `Connection`：告诉后端这个连接要从普通 HTTP 升级成 WebSocket。

### 负载均衡实例

1. 场景

    同一个后端项目启动三个实例：

    ```text
    127.0.0.1:8080
    127.0.0.1:8081
    127.0.0.1:8082
    ```

    用户仍然只访问：

    ```text
    http://example.com/api/user/login
    ```

    Nginx 负责把请求分发给不同后端。

2. 基本配置

    ```nginx
    upstream backend_pool {
        server 127.0.0.1:8080;
        server 127.0.0.1:8081;
        server 127.0.0.1:8082;
    }

    server {
        listen 80;
        server_name example.com;

        location /api/ {
            proxy_pass http://backend_pool;

            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            proxy_connect_timeout 5s;
            proxy_read_timeout 60s;
        }
    }
    ```

3. 参数解释

    - `upstream backend_pool`
        - 定义一个后端服务器组。
        - `backend_pool` 是自定义名字，后面 `proxy_pass http://backend_pool;` 会用到。
        - 它不是域名，也不是真实端口，只是 Nginx 配置里的逻辑名称。

    - `server 127.0.0.1:8080;`
        - 表示后端实例地址。
        - 可以是本机端口，也可以是其他服务器内网 IP：
            ```nginx
            server 192.168.1.11:8080;
            server 192.168.1.12:8080;
            ```

    - `proxy_pass http://backend_pool;`
        - 表示请求不再转发给单个后端，而是转发给 `backend_pool` 这个后端组。

4. 默认轮询

    不额外写策略时，Nginx 默认采用轮询：

    ```text
    第 1 个请求 -> 8080
    第 2 个请求 -> 8081
    第 3 个请求 -> 8082
    第 4 个请求 -> 8080
    ```

5. 权重配置

    如果某台服务器性能更强，可以给它更高权重：

    ```nginx
    upstream backend_pool {
        server 127.0.0.1:8080 weight=3;
        server 127.0.0.1:8081 weight=1;
        server 127.0.0.1:8082 weight=1;
    }
    ```

    含义大概是：

    ```text
    8080 分到更多请求
    8081、8082 分到较少请求
    ```

6. 故障相关参数

    ```nginx
    upstream backend_pool {
        server 127.0.0.1:8080 max_fails=3 fail_timeout=10s;
        server 127.0.0.1:8081 max_fails=3 fail_timeout=10s;
        server 127.0.0.1:8082 backup;
    }
    ```

    - `max_fails=3`
        - 在 `fail_timeout` 时间内失败 3 次，就认为这个后端暂时不可用。

    - `fail_timeout=10s`
        - 失败统计窗口也是临时摘除时间。
        - 上例表示 10 秒内失败 3 次，就暂时不再转发给它。

    - `backup`
        - 备用节点。
        - 正常节点都不可用时，才会把请求转发给它。

    - `down`
        - 手动标记某个节点不可用，常用于临时维护：
            ```nginx
            server 127.0.0.1:8081 down;
            ```

7. 常见负载均衡策略

    - 默认轮询
        ```nginx
        upstream backend_pool {
            server 127.0.0.1:8080;
            server 127.0.0.1:8081;
        }
        ```
        适合大多数无状态接口。

    - `least_conn`
        ```nginx
        upstream backend_pool {
            least_conn;
            server 127.0.0.1:8080;
            server 127.0.0.1:8081;
        }
        ```
        优先转发给当前连接数较少的后端，适合接口耗时差异较大的场景。

    - `ip_hash`
        ```nginx
        upstream backend_pool {
            ip_hash;
            server 127.0.0.1:8080;
            server 127.0.0.1:8081;
        }
        ```
        同一个客户端 IP 尽量打到同一台后端。可以缓解 Session 不共享的问题，但不是根本方案。生产中更推荐把 Session 放到 Redis 这类共享存储中。

8. 后端连接复用

    ```nginx
    upstream backend_pool {
        server 127.0.0.1:8080;
        server 127.0.0.1:8081;
        keepalive 32;
    }

    location /api/ {
        proxy_pass http://backend_pool;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
    }
    ```

    - `keepalive 32`：保留一批到后端的空闲长连接，减少频繁建立 TCP 连接的成本。
    - `proxy_http_version 1.1`：后端连接复用通常配合 HTTP/1.1。
    - `proxy_set_header Connection ""`：避免错误地把客户端的 `Connection` 头传给后端。

### 动静分离实例

1. 场景

    一个前后端分离项目通常有三类请求：

    ```text
    /                  前端页面
    /assets/index.js   前端静态资源
    /api/user/login    后端接口
    /uploads/a.png     上传文件或图片
    ```

    Nginx 直接返回静态文件，把接口转发给后端。

2. 示例配置

    ```nginx
    server {
        listen 80;
        server_name example.com;

        # 前端打包后的 dist 目录
        root /var/www/lcq-blog;
        index index.html;

        # 前端页面入口，适合 Vue / React / Umi 等单页应用
        location / {
            try_files $uri $uri/ /index.html;
        }

        # 后端接口
        location /api/ {
            proxy_pass http://127.0.0.1:8080;

            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }

        # 静态资源缓存
        location /assets/ {
            try_files $uri =404;
            expires 7d;
            add_header Cache-Control "public";
        }

        # 用户上传文件目录
        location /uploads/ {
            alias /data/uploads/;
            autoindex off;
        }
    }
    ```

3. 参数解释

    - `root /var/www/lcq-blog;`
        - 指定静态网站根目录。
        - 请求 `/assets/index.js` 时，会去找：
            ```text
            /var/www/lcq-blog/assets/index.js
            ```

    - `index index.html;`
        - 用户访问目录时默认返回的首页文件。
        - 例如访问 `/`，会尝试返回 `/var/www/lcq-blog/index.html`。

    - `try_files $uri $uri/ /index.html;`
        - 这是前端单页应用常见配置。
        - 查找顺序：
            ```text
            先找真实文件
            再找真实目录
            找不到就返回 index.html
            ```
        - 例如用户直接访问 `/user/profile`，服务器上并没有这个真实目录，但前端路由需要接管，所以返回 `index.html`。

    - `location /api/`
        - 接口请求不走前端文件目录，而是转发给后端。
        - 这就是“动态请求”和“静态请求”的分离。

    - `location /assets/`
        - 前端构建产物常放在这个目录。
        - JS、CSS、图片可以让浏览器缓存一段时间，减少重复下载。

    - `expires 7d;`
        - 告诉浏览器这类资源可以缓存 7 天。
        - 如果前端文件名带 hash，例如 `index-abc123.js`，可以设置更久。
        - 如果文件名不变，不建议缓存太久，否则更新后用户可能还拿到旧文件。

    - `add_header Cache-Control "public";`
        - 明确告诉浏览器和中间缓存，这个资源可以被缓存。

    - `alias /data/uploads/;`
        - 把 URL 路径 `/uploads/` 映射到服务器目录 `/data/uploads/`。
        - 请求：
            ```text
            /uploads/a.png
            ```
        - 实际文件：
            ```text
            /data/uploads/a.png
            ```

    - `autoindex off;`
        - 禁止用户直接浏览目录文件列表。
        - 如果打开，访问 `/uploads/` 可能会列出目录下所有文件，一般不安全。

4. `root` 和 `alias` 区别

    `root` 会把完整 URL 路径拼到目录后面：

    ```nginx
    location /uploads/ {
        root /data;
    }
    ```

    请求：

    ```text
    /uploads/a.png
    ```

    实际文件：

    ```text
    /data/uploads/a.png
    ```

    `alias` 会用指定目录替换掉 location 前缀：

    ```nginx
    location /uploads/ {
        alias /data/uploads/;
    }
    ```

    请求：

    ```text
    /uploads/a.png
    ```

    实际文件：

    ```text
    /data/uploads/a.png
    ```

    这两个例子结果刚好一样，但规则不同。更明显的例子：

    ```nginx
    location /files/ {
        alias /data/uploads/;
    }
    ```

    请求 `/files/a.png`，实际访问的是 `/data/uploads/a.png`，不是 `/data/uploads/files/a.png`。

### 配置可用证书

1. 场景

    HTTPS 配置的目标是：

    ```text
    用户浏览器 --HTTPS--> Nginx --HTTP--> 后端服务
    ```

    也就是说，证书通常配置在 Nginx 上，后端 Spring Boot 可以继续监听普通 HTTP 端口。

2. 自签名证书示例

    只适合本地学习或内网测试，浏览器一般会提示“不受信任”。

    ```bash
    sudo mkdir -p /etc/nginx/ssl

    sudo openssl req -x509 -nodes -days 365 \
      -newkey rsa:2048 \
      -keyout /etc/nginx/ssl/example.com.key \
      -out /etc/nginx/ssl/example.com.crt
    ```

    生成两个文件：

    ```text
    example.com.crt  证书文件，给浏览器看
    example.com.key  私钥文件，服务器自己保存，不能泄露
    ```

3. Nginx 使用证书

    ```nginx
    server {
        listen 443 ssl;
        server_name example.com;

        ssl_certificate /etc/nginx/ssl/example.com.crt;
        ssl_certificate_key /etc/nginx/ssl/example.com.key;

        ssl_protocols TLSv1.2 TLSv1.3;
        ssl_session_cache shared:SSL:10m;
        ssl_session_timeout 10m;

        root /var/www/lcq-blog;
        index index.html;

        location / {
            try_files $uri $uri/ /index.html;
        }

        location /api/ {
            proxy_pass http://127.0.0.1:8080;

            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
    ```

4. 参数解释

    - `listen 443 ssl;`
        - 监听 HTTPS 默认端口 `443`。
        - `ssl` 表示这个端口启用 TLS/SSL。

    - `server_name example.com;`
        - 当前证书和站点对应的域名。
        - 真实使用时要改成自己的域名，例如 `lcqblog.com` 或 `www.lcqblog.com`。

    - `ssl_certificate`
        - 证书文件路径。
        - 浏览器会用它验证服务器身份。

    - `ssl_certificate_key`
        - 私钥文件路径。
        - 必须和证书匹配。
        - 权限要保护好，不要上传到 GitHub。

    - `ssl_protocols TLSv1.2 TLSv1.3;`
        - 指定允许使用的 TLS 版本。
        - 生产环境一般不建议再开 TLSv1、TLSv1.1。

    - `ssl_session_cache shared:SSL:10m;`
        - 缓存 TLS 会话信息。
        - 用户短时间多次访问时，可以减少重复握手开销。

    - `ssl_session_timeout 10m;`
        - TLS 会话缓存保留时间。

5. HTTP 自动跳转 HTTPS

    一般还会额外配置一个 `80` 端口，把 HTTP 请求跳转到 HTTPS：

    ```nginx
    server {
        listen 80;
        server_name example.com;

        return 301 https://$host$request_uri;
    }
    ```

    - `$host`：用户访问的域名。
    - `$request_uri`：完整请求路径和参数。

    例如：

    ```text
    http://example.com/api/user?id=1
        -> https://example.com/api/user?id=1
    ```

6. 用 Let's Encrypt 获取正式证书

    有真实域名并且域名已经解析到服务器后，可以用：

    ```bash
    sudo apt install certbot python3-certbot-nginx -y
    sudo certbot --nginx -d example.com -d www.example.com
    ```

    它通常会自动完成：

    ```text
    申请证书
    写入 Nginx 配置
    配置 HTTP 到 HTTPS 跳转
    配置自动续期
    ```

    检查自动续期：

    ```bash
    sudo certbot renew --dry-run
    ```

7. 检查并重载

    ```bash
    sudo nginx -t
    sudo systemctl reload nginx
    ```

    建议每次改证书、反代、负载均衡配置后都先执行 `nginx -t`，确认配置没问题再重载。



## 实操案例

### 最小静态网站配置

1. 例如把前端项目放到：

    ```text
    /var/www/lcq-blog
    ```

2. 目录结构：

    ```text
    /var/www/lcq-blog
        ├── index.html
        ├── assets/
        └── favicon.ico
    ```

3. 创建配置文件：

    ```bash
    sudo nano /etc/nginx/sites-available/lcq-blog
    ```

    写入：

    ```nginx
    server {
        listen 80;
        server_name example.com www.example.com;

        root /var/www/lcq-blog;
        index index.html;

        location / {
            try_files $uri $uri/ /index.html;
        }
    }
    ```

4. 启用配置：

    ```bash
    sudo ln -s /etc/nginx/sites-available/lcq-blog /etc/nginx/sites-enabled/
    sudo nginx -t
    sudo systemctl reload nginx
    ```

5. 解释：
    - 表示一个虚拟主机配置。一个 Nginx 可以配置多个 `server`，对应多个网站或多个域名。

        ```nginx
        server {}
        ```

    - 监听 80 端口，也就是 HTTP 默认端口。

        ```nginx
        listen 80;
        ```

    - 指定这个配置匹配哪些域名。

        ```nginx
        server_name example.com www.example.com;
        ```

    - 指定网站根目录。

        ```nginx
        root /var/www/lcq-blog;
        ```

    - 指定默认首页文件。

        ```nginx
        index index.html;
        ```

    - 表示访问任意路径时，Nginx 会先尝试找对应文件或目录；如果找不到，就返回 `index.html`。
        ```nginx
        location / {
            try_files $uri $uri/ /index.html;
        }
        ```

### 反向代理后端服务

假设 Spring Boot 后端运行在：

```text
http://127.0.0.1:8080
```

希望用户访问：

```text
http://example.com/api/user/login
```

实际转发到：

```text
http://127.0.0.1:8080/api/user/login
```

配置：

```nginx
server {
    listen 80;
    server_name example.com www.example.com;

    location /api/ {
        proxy_pass http://127.0.0.1:8080;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

解释：

```nginx
location /api/ {}
```

表示匹配所有以 `/api/` 开头的请求。

```nginx
proxy_pass http://127.0.0.1:8080;
```

表示把请求转发给本机 8080 端口的后端服务。

```nginx
proxy_set_header Host $host;
```

把原始请求的域名传给后端。

```nginx
proxy_set_header X-Real-IP $remote_addr;
```

把用户真实 IP 传给后端。

```nginx
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```

记录经过的代理链路，常用于获取真实客户端 IP。

```nginx
proxy_set_header X-Forwarded-Proto $scheme;
```

告诉后端用户原始访问协议是 HTTP 还是 HTTPS。

---

### 前后端分离常用配置

假设：

```text
前端目录：/var/www/lcq-blog
后端服务：http://127.0.0.1:8080
域名：example.com
```

完整配置：

```nginx
server {
    listen 80;
    server_name example.com www.example.com;

    root /var/www/lcq-blog;
    index index.html;

    # 前端页面
    location / {
        try_files $uri $uri/ /index.html;
    }

    # 后端接口
    location /api/ {
        proxy_pass http://127.0.0.1:8080;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

访问关系：

```text
用户访问 example.com
   ↓
Nginx 返回 /var/www/lcq-blog/index.html

用户访问 example.com/assets/index.js
   ↓
Nginx 返回 /var/www/lcq-blog/assets/index.js

用户访问 example.com/api/user/login
   ↓
Nginx 转发给 http://127.0.0.1:8080/api/user/login
```

---



### 负载均衡示例

如果后端有多个实例：

```text
127.0.0.1:8080
127.0.0.1:8081
127.0.0.1:8082
```

可以配置：

```nginx
upstream backend_servers {
    server 127.0.0.1:8080;
    server 127.0.0.1:8081;
    server 127.0.0.1:8082;
}

server {
    listen 80;
    server_name example.com;

    location /api/ {
        proxy_pass http://backend_servers;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

访问流程：

```text
用户请求 /api/user/login
   ↓
Nginx
   ↓
转发给 8080 / 8081 / 8082 中的某一个
```

默认是轮询策略，也就是几个后端服务轮流处理请求。

## Nginx 集群部署（内网）

### keepalived

1. 简介
    - 它一般和 Nginx 集群配合，用 VIP 虚拟 IP 做主备高可用：客户端访问 VIP，正常由主 Nginx 承担；主机或 Nginx 挂了后，VIP 漂移到备机。
    - Keepalived 本身就是 Linux 上做高可用和负载均衡的软件，常用于 VRRP 场景；Nginx 官方文档也有基于 keepalived 的 HA 方案。

1. 安装

    两台 Nginx 服务器都安装

    假设：

    ```text
    主 Nginx：192.168.218.132
    备 Nginx：192.168.218.133
    VIP：     192.168.218.200
    网卡名：  ens33
    ```

    先在两台服务器都执行：

    ```bash
    sudo apt update
    sudo apt install -y keepalived

    keepalived --version
    systemctl status keepalived
    ```

    查看网卡名：

    ```bash
    ip a
    ```

    你要找类似：

    ```text
    ens33
    eth0
    enp0s3
    ```

    后面配置里的 `interface ens33` 要换成你自己的网卡名。


2. 写 Nginx 健康检查脚本

    两台服务器都执行：

    ```bash
    sudo tee /etc/keepalived/check_nginx.sh > /dev/null <<'EOF'
    #!/bin/bash
    systemctl is-active --quiet nginx
    EOF

    sudo chmod +x /etc/keepalived/check_nginx.sh
    ```

    这个脚本的意思是：

    ```text
    如果 nginx 正在运行，检查成功；
    如果 nginx 挂了，检查失败；
    keepalived 会根据检查结果决定是否降低当前节点优先级。
    ```

3. 主节点配置

    在主 Nginx 上执行：

    ```bash
    sudo tee /etc/keepalived/keepalived.conf > /dev/null <<'EOF'
    global_defs {
        router_id nginx_master_132
    }

    vrrp_script chk_nginx {
        script "/etc/keepalived/check_nginx.sh"
        interval 2
        weight -30
        fall 2
        rise 1
    }

    vrrp_instance VI_1 {
        state MASTER
        interface ens33
        virtual_router_id 51
        priority 100
        advert_int 1

        authentication {
            auth_type PASS
            auth_pass 12345678
        }

        virtual_ipaddress {
            192.168.218.200/24
        }

        track_script {
            chk_nginx
        }
    }
    EOF
    ```

    注意把：

    ```conf
    interface ens33
    ```

    改成你自己的网卡名。



4. 备节点配置

    在备 Nginx 上执行：

    ```bash
    sudo tee /etc/keepalived/keepalived.conf > /dev/null <<'EOF'
    global_defs {
        router_id nginx_backup_133
    }

    vrrp_script chk_nginx {
        script "/etc/keepalived/check_nginx.sh"
        interval 2
        weight -30
        fall 2
        rise 1
    }

    vrrp_instance VI_1 {
        state BACKUP
        interface ens33
        virtual_router_id 51
        priority 90
        advert_int 1

        authentication {
            auth_type PASS
            auth_pass 12345678
        }

        virtual_ipaddress {
            192.168.218.200/24
        }

        track_script {
            chk_nginx
        }
    }
    EOF
    ```

    主备配置差异主要是：

    ```text
    主节点：state MASTER，priority 100
    备节点：state BACKUP，priority 90
    ```

    其他关键配置要保持一致：

    ```text
    virtual_router_id 51
    auth_pass 12345678
    VIP 192.168.218.200/24
    网卡名
    ```

5. 启动 keepalived

    两台都执行：

    ```bash
    sudo systemctl enable keepalived
    sudo systemctl restart keepalived
    sudo systemctl status keepalived
    ```

    查看 VIP 是否绑定成功：

    ```bash
    ip a | grep 192.168.218.200
    ```

    正常情况下，**只有主节点**能看到：

    ```text
    192.168.218.200
    ```

    备节点看不到 VIP。

6. 测试主备切换

    在你电脑浏览器访问：

    ```text
    http://192.168.218.200
    ```

    然后在主 Nginx 上停止 Nginx：

    ```bash
    sudo systemctl stop nginx
    ```

    等待几秒后，到备节点查看：

    ```bash
    ip a | grep 192.168.218.200
    ```

    如果备节点出现 VIP，说明漂移成功。

    再访问：

    ```text
    http://192.168.218.200
    ```

    如果还能打开页面，说明 Nginx 高可用初步成功。

    恢复主节点：

    ```bash
    sudo systemctl start nginx
    ```

    如果主节点优先级更高，VIP 可能会自动漂回主节点。

7. 你要特别注意这几个坑

    1. VIP 必须和真实 IP 在同一网段

        比如你的机器是：

        ```text
        192.168.218.132
        192.168.218.133
        ```

        那 VIP 可以用：

        ```text
        192.168.218.200
        ```

        但不要乱写成：

        ```text
        192.168.1.200
        10.0.0.200
        ```

        否则客户端可能访问不到。

    2. VIP 不能被其他设备占用

        配置前可以先 ping 一下：

        ```bash
        ping 192.168.218.200
        ```

        如果没人响应，通常可以用。

    3. 虚拟机 NAT 网络可能影响 VRRP

        如果你是在 VMware 里练习，推荐两台 Ubuntu 虚拟机使用同一种网络模式，最好是：

        ```text
        桥接模式
        ```

        或者确保它们在同一个二层网络里。

        Keepalived 的 VIP 漂移依赖网络层可达性。云服务器、公网环境、NAT 环境下不一定能直接这样玩，有些云厂商不允许你随便绑定额外 IP。
    
    4. 防火墙可能拦 VRRP

        VRRP 使用的是 IP 协议号 112，不是普通 TCP/UDP 端口。测试阶段可以先关闭防火墙排查：

        ```bash
        sudo ufw status
        ```

        如果开启了，测试时可以临时关：

        ```bash
        sudo ufw disable
        ```

8. 运行效果

    ```text
    用户浏览器
        |
        | 访问 http://192.168.218.200
        v
    VIP：192.168.218.200
        |
        +--> 主 Nginx：192.168.218.132
        |
        +--> 备 Nginx：192.168.218.133
    ```

    平时：

    ```text
    VIP 在主 Nginx 上
    ```

    主 Nginx 故障后：

    ```text
    VIP 漂移到备 Nginx 上
    ```

    所以用户始终访问：

    ```text
    192.168.218.200
    ```

    而不是直接访问某一台真实服务器。




## Nginx 工作机制

### master-worker 进程模型

Nginx 不是“一个进程里开很多线程”来处理请求，而是典型的 **master-worker 多进程模型**。

可以先记住一句话：

```text
master 负责管理，worker 负责干活。
```

整体结构如下：

```text
                    Nginx 主进程
                  master process
                         |
        +----------------+----------------+
        |                |                |
  worker process   worker process   worker process
        |                |                |
   处理请求连接      处理请求连接      处理请求连接
```

1. `master process`
    - 读取和检查配置文件。
    - 创建、管理、监控 worker 进程。
    - 接收控制信号，例如停止、平滑退出、重新加载配置。
    - 当某个 worker 异常退出时，负责重新拉起新的 worker。

2. `worker process`
    - 真正处理客户端请求。
    - 接收连接、读取请求、匹配 `server` 和 `location`。
    - 返回静态资源，或者把请求反向代理给后端服务。
    - 每个 worker 是独立进程，互相之间一般不共享业务数据。

可以用下面的命令查看：

```bash
ps -ef | grep nginx
```

常见结果类似：

```text
root      1000     1  0  nginx: master process /usr/sbin/nginx
www-data  1001  1000  0  nginx: worker process
www-data  1002  1000  0  nginx: worker process
```

这里可以看到：

```text
root 用户启动 master
www-data 用户运行 worker
```

这也是为什么配置里经常会看到：

```nginx
user www-data;
```

它主要影响 worker 进程用哪个系统用户执行。静态文件权限不对时，经常就是因为 worker 用户没有读目录或文件的权限。

---

### 一个请求是怎么被处理的

以访问：

```text
http://example.com/api/user/1
```

为例，大致流程如下：

```text
浏览器
  |
  | 1. 建立 TCP 连接
  v
Nginx 监听端口 80/443
  |
  | 2. 某个 worker 接收连接
  v
匹配 server_name
  |
  | 3. 根据 Host 找 server 块
  v
匹配 location
  |
  | 4. 根据 URI 找 location 块
  v
执行对应动作
  |
  +--> 返回静态文件
  |
  +--> 反向代理到 Tomcat / Spring Boot / Node
  |
  +--> 重定向
  |
  +--> 返回错误页
```

比如下面这个配置：

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        root /var/www/html;
        index index.html;
    }

    location /api/ {
        proxy_pass http://127.0.0.1:8080/;
    }
}
```

请求处理逻辑就是：

```text
访问 /index.html  → Nginx 自己去 /var/www/html 里找文件
访问 /api/user/1 → Nginx 转发给 127.0.0.1:8080
```

所以 Nginx 的工作不是“看到请求就全部转发”，而是：

```text
先匹配 server，再匹配 location，再根据配置决定怎么处理。
```

---

### 为什么 Nginx 并发能力强

Nginx 高并发的核心不是“开了很多线程”，而是：

```text
多 worker 进程 + 非阻塞 I/O + I/O 多路复用
```

在 Linux 下常见的是 `epoll` 模型。

传统阻塞式处理可以理解为：

```text
一个连接来了
worker 等它传数据
数据没来，worker 也干不了别的
```

Nginx 的事件驱动模型更接近：

```text
连接来了，先登记
数据可读了，再通知 worker 来读
后端有响应了，再通知 worker 来转发
没有事件时，worker 不傻等
```

反向代理时尤其明显：

```text
客户端请求进来
    |
worker 把请求发给后端
    |
后端还没返回
    |
worker 不一直阻塞等待，而是注册事件
    |
worker 可以继续处理其他连接
    |
后端返回后，再继续处理这个请求
```

所以少量 worker 也能同时维护大量连接。

---

### 惊群现象和 accept_mutex

多个 worker 都监听同一个端口时，会出现一个问题：

```text
新连接来了，多个 worker 都被唤醒
但最终只有一个 worker 能 accept 成功
其他 worker 白白被叫醒
```

这类现象通常叫“惊群”。

`accept_mutex` 的作用可以简单理解成：

```text
同一时刻尽量只让一个 worker 去接收新连接
```

示例：

```nginx
events {
    accept_mutex on;
    worker_connections 1024;
}
```

它不是业务功能开关，而是连接接收层面的优化参数。普通学习和小项目可以不专门改它，先理解含义即可。

---

### reload 为什么可以不中断服务

平时修改配置后常用：

```bash
sudo nginx -t
sudo systemctl reload nginx
```

或者：

```bash
sudo nginx -s reload
```

`reload` 的大致过程是：

```text
1. master 收到 reload 信号
2. master 检查新的配置文件
3. 配置正确，则启动新的 worker
4. 新 worker 使用新配置接收新请求
5. 旧 worker 不再接收新请求
6. 旧 worker 处理完已有请求后退出
```

所以它比 `restart` 更平滑。

```text
reload：尽量不中断已有连接
restart：先停再起，影响更大
```

生产环境修改配置时，常用流程是：

```bash
sudo nginx -t && sudo systemctl reload nginx
```

含义是：

```text
只有配置检查成功，才重新加载。
```

---

### worker_processes

`worker_processes` 表示启动多少个 worker 进程。

常见写法：

```nginx
worker_processes auto;
```

或者手动指定：

```nginx
worker_processes 2;
```

含义：

```text
worker_processes 1   → 1 个 worker 处理请求
worker_processes 2   → 2 个 worker 处理请求
worker_processes auto → Nginx 根据 CPU 核心数自动设置
```

一般建议：

```text
普通服务器：worker_processes auto;
学习环境：  worker_processes 1; 或 auto 都可以
```

不要盲目把它调得很大。worker 太少会浪费 CPU，worker 太多会增加进程切换开销。

---

### worker_connections

`worker_connections` 表示：

```text
单个 worker 最多同时维护多少个连接
```

示例：

```nginx
events {
    worker_connections 1024;
}
```

如果是：

```nginx
worker_processes 2;

events {
    worker_connections 1024;
}
```

理论最大连接数约为：

```text
2 × 1024 = 2048 条连接
```

注意，这里说的是“连接数”，不是简单等于“用户数”或“请求数”。

原因是：

```text
1 个用户可能同时打开多个 TCP 连接
HTTP/1.1 Keep-Alive 会让连接保持一段时间
反向代理场景下，Nginx 还要连接后端服务器
WebSocket 长连接会长期占用连接数
```

---

### 最大并发数怎么估算

很多教程会写：

```text
静态资源最大并发 ≈ worker_processes × worker_connections / 2
反向代理最大并发 ≈ worker_processes × worker_connections / 4
```

这个说法可以当作粗略估算，但不要机械理解成“浏览器一来一回固定占两个连接”。更准确的理解是：

```text
Nginx 统计的是连接，不是业务请求。
```

静态资源场景：

```text
客户端连接 Nginx
Nginx 直接返回本地文件
```

主要消耗：

```text
客户端 ↔ Nginx 的连接
```

反向代理场景：

```text
客户端连接 Nginx
Nginx 再连接后端服务
```

主要消耗：

```text
客户端 ↔ Nginx 的连接
Nginx ↔ 后端服务的连接
```

所以反向代理一般比纯静态站更吃连接数。

举例：

```nginx
worker_processes 2;

events {
    worker_connections 4096;
}
```

理论连接上限：

```text
2 × 4096 = 8192 条连接
```

如果是纯静态资源，能承载的活跃连接接近这个数量，但还要看 Keep-Alive、文件大小、网速和系统文件描述符限制。

如果是反向代理，一个请求常常同时占用客户端连接和后端连接，那么可承载的正在处理中的代理请求数要低一些。


● 设置 worker 数量, Nginx 默认没有开启利用多核 cpu，可以通过增加 worker_cpu_affinity
```nginx
#2 核 cpu，开启 2 个进程
worker_processes 2;
worker_cpu_affinity 01 10;
#2 核 cpu，开启 4 个进程，
worker_processes 4;
worker_cpu_affinity 01 10 01 10;
#4 核 cpu，开启 2 个进程，0101 表示开启第一个和第三个内核，1010 表示开启第二个和第四个内核；
worker_processes 2;
worker_cpu_affinity 0101 1010;
#4 个 cpu，开启 4 个进程
worker_processes 4;
worker_cpu_affinity 0001 0010 0100 1000;
#8 核 cpu，开启 8 个进程
worker_processes 8;
worker_cpu_affinity 00000001 00000010 00000100 00001000 00010000 00100000 01000000 10000000;
```

---

### worker_rlimit_nofile 和系统最大打开文件数

Nginx 的连接本质上会占用文件描述符。Linux 中 socket、普通文件、日志文件都算文件描述符。

如果文件描述符不够，可能出现：

```text
too many open files
```

查看当前 shell 限制：

```bash
sudo ulimit -n
```

查看某个 Nginx 进程限制：

```bash
cat /proc/$(pgrep -n nginx)/limits | grep "open files"
```

Nginx 配置中可以写：

```nginx
worker_rlimit_nofile 65535;
```

系统层面可以在 `/etc/security/limits.conf` 中配置：

```text
* soft nofile 65535
* hard nofile 65535
```

理解关系：

```text
worker_connections 想调大
    ↓
系统允许打开的文件数也要够
    ↓
否则 Nginx 配了也跑不满
```

---

### 常见工作机制相关参数

一个比较常见的基础配置如下：

```nginx
user www-data;
worker_processes auto;
worker_rlimit_nofile 65535;

error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

events {
    use epoll;
    worker_connections 4096;
    # accept_mutex on;
    # multi_accept on;
}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
}
```

参数解释：

1. `user www-data;`
    - 指定 worker 进程的运行用户。
    - 影响 Nginx 读取静态文件、写日志、访问目录的权限。

2. `worker_processes auto;`
    - 自动根据 CPU 核心数决定 worker 数量。
    - 比手动乱写一个很大的数字更稳。

3. `worker_rlimit_nofile 65535;`
    - 提高 worker 能打开的文件描述符数量。
    - 高并发连接、日志文件、静态文件读取都会受它影响。

4. `use epoll;`
    - Linux 下常用的 I/O 多路复用模型。
    - Ubuntu 上通常可以使用。

5. `worker_connections 4096;`
    - 单个 worker 可维护的最大连接数。
    - 总连接数大致为 `worker_processes × worker_connections`。

6. `accept_mutex on;`
    - 用来减少多个 worker 同时抢连接造成的无效唤醒。
    - 学习阶段知道作用即可，不一定必须手动开启。

7. `multi_accept on;`
    - 一个 worker 被唤醒后，尽可能一次接收多个新连接。
    - 流量很高时可能有用，小项目可以不改。

8. `sendfile on;`
    - 让 Nginx 更高效地发送静态文件。
    - 静态资源站点通常建议开启。

9. `keepalive_timeout 65;`
    - 客户端长连接保持时间。
    - 太短会导致频繁建连，太长会占用连接资源。

---

### 反向代理时的连接复用

反向代理不只是把请求“转过去”，还涉及两段连接：

```text
客户端  <--连接1-->  Nginx  <--连接2-->  后端服务
```

如果每个请求都重新连接后端，会增加开销。可以在 `upstream` 中配置 `keepalive`，让 Nginx 到后端的连接复用。

示例：

```nginx
upstream backend {
    server 127.0.0.1:8080;
    server 127.0.0.1:8081;

    keepalive 32;
}

server {
    listen 80;
    server_name example.com;

    location /api/ {
        proxy_http_version 1.1;
        proxy_set_header Connection "";
        proxy_pass http://backend/;
    }
}
```

说明：

1. `keepalive 32;`
    - 表示每个 worker 可以保留一定数量的空闲上游连接。
    - 它不是限制后端总连接数，而是连接池复用参数。

2. `proxy_http_version 1.1;`
    - 使用 HTTP/1.1 转发给后端，便于复用连接。

3. `proxy_set_header Connection "";`
    - 避免把客户端的 `Connection` 头原样传给后端。
    - 常用于上游 keepalive 配置。

---

### 小实验：观察 master 和 worker

1. 查看进程：

    ```bash
    ps -ef | grep nginx
    ```

2. 查看监听端口：

    ```bash
    sudo ss -lntp | grep nginx
    ```

3. 修改 `worker_processes`：

    ```nginx
    worker_processes 2;
    ```

4. 检查并重新加载：

    ```bash
    sudo nginx -t
    sudo systemctl reload nginx
    ```

5. 再次查看进程：

    ```bash
    ps -ef | grep nginx
    ```

    如果配置生效，通常能看到 worker 数量变化。

---

### 小实验：手动杀掉 worker

先查看 worker：

```bash
ps -ef | grep "nginx: worker"
```

杀掉其中一个 worker：

```bash
sudo kill -9 <worker_pid>
```

再查看：

```bash
ps -ef | grep nginx
```

一般会发现 master 又重新拉起了一个新的 worker。

这个实验能说明：

```text
worker 真正处理请求
master 负责管理和守护 worker
```

不要杀 master 做这个实验。杀 master 会影响整个 Nginx 服务。

---

### 常见误区

1. `worker_connections` 不是网站用户数。

    它表示连接数。一个用户可能打开多个连接，一个 WebSocket 用户也可能长期占用一个连接。

2. `worker_processes` 不是越大越好。

    一般和 CPU 核心数接近即可。过多 worker 会带来额外进程切换。

3. `reload` 不是简单重启。

    reload 会让新 worker 使用新配置，旧 worker 处理完已有请求后退出，因此更适合生产环境更新配置。

4. Nginx 并发高，不代表后端也能扛住。

    Nginx 可以轻松接住大量连接，但如果后端 Tomcat、Spring Boot、数据库处理不过来，仍然会出现慢请求、502 或 504。

5. 反向代理场景要同时看 Nginx 和后端。

    排查问题时不能只看 Nginx。还要看后端端口是否监听、服务是否运行、日志是否报错。

---

### 工作机制小结

可以把 Nginx 理解成一个“高效门卫”：

```text
master：管人、收信号、重载配置、拉起 worker
worker：接待请求、匹配规则、返回静态文件、转发后端
```

高并发能力主要来自：

```text
多进程模型
事件驱动
非阻塞 I/O
I/O 多路复用
较少的线程/锁开销
```

配置时重点关注：

```text
worker_processes        worker 数量
worker_connections      单 worker 连接数
worker_rlimit_nofile    文件描述符限制
keepalive_timeout       长连接保持时间
sendfile                静态文件发送优化
upstream keepalive      反向代理后端连接复用
```



