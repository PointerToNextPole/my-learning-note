# nginx 笔记



##### 一些资料

- [什么？2025年了发版后还要手动清浏览器缓存？](https://juejin.cn/post/7527977068417957939) ：介绍了 nginx 配置对前端浏览器缓存机制的影响，此外，给出了一份 “经过实战检验的、可以安全用于生产环境的“ Nginx 配置方案



#### 基本概念

##### 反向代理

###### 正向代理

在客户端（浏览器）配置代理服务器，通过<font color=FF0000>代理服务器</font>进行互联网访问<font color=FF0000>目标服务器</font>

###### 反向代理

服务器根据客户端的请求，<font color=FF0000>从其关系的一组或多组后端服务器（如Web服务器）上获取资源</font>，<font color=FF0000>然后再将这些资源返回给客户端</font>，<font color=LightSeaGreen>客户端只会得知反向代理的IP地址，而不知道在代理服务器后面的服务器集群的存在</font>。

摘自：[维基百科 -- 反向代理]([https://zh.wikipedia.org/wiki/%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%86](https://zh.wikipedia.org/wiki/反向代理))

###### 区别

正向代理隐藏真实客户端，反向代理隐藏真实服务端

###### 进一步解释

> 👀 上面的解释，还是有点不明白。这里再做点补充

<font color=dodgerBlue>**反向代理**</font>，假设我们是一个用户，在上述场景中，<font color=LightSeaGreen>不管增加多少台服务器，用户始终访问一个相同的域名，对于用户而言，其做的 **反向代理对用户是无感知的**</font>，这里的代理是 Nginx 中间件这一层把用户的请求代理到了我们的服务器上，同时他也是在服务端完成的，<font color=red>**反向代理的过程，隐藏了真实的服务端**</font>（👀 这里原文是客户端，应该是笔误）。<font color=red>**客户端请求的服务都被代理服务器代替来请求**</font>

<font color=dodgerBlue>**正向代理**</font>，假设这样一个场景：在日常开发中应该经常会使用到 vpn，公司的一些项目或者私有 git 或者一些内部的网站，需要登录 vpn 才能访问。其基本原理是：有一个位于客户端和原始服务器之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并且指定目标（原始服务器），然后代理向原始服务器转交请求并将获得的内容返回给客户端，客户端才能使用正向代理，<font color=LightSeaGreen>登录 vpn 之后所有的请求将会通过 vpn 的这台服务器代理客户去访问某些资源或者网站，那么这台代理服务器将会是公司内部的白名单 ip，这样所有的请求都统一从这里进入，就实现了一个正向代理</font>。如此一来，<font color=fuchsia>**正向代理的过程就隐藏了真实的客户端，客户端请求的服务都被代理服务器代替来请求**</font>。

摘自：[前端必备知识之Nginx](https://juejin.cn/post/7108394145068089374)

##### 负载均衡

##### 动静分离

将<font color=FF0000>动态资源</font>和<font color=FF0000>静态资源</font>放在<font color=FF0000>**不同的**服务器</font>解析



#### macOS 环境 nginx 文件的目录

- nginx 安装文件目录： `/usr/local/Cellar/nginx`

  这个可以通过 `which nginx` 或者 `where nginx` 找到

- nginx 配置文件目录：  `/usr/local/etc/nginx`
  - config 文件：  `/usr/local/etc/nginx/nginx.conf`

    > 这个不熟悉的情况下有点难找，在问 Gemini 2.5Pro 后得到了 [答案](https://g.co/gemini/share/e724786464dc)，不过这个不一定非要记住，在回答给了 “通过 `nginx -t` 获取配置文件” 的方法，感觉相当好用
  
- 系统 hosts 位置：  `/private/etc/hosts`

> [!NOTE]
>
> 值得注意的是：Apple Silicon 芯片 Mac 的 HomeBrew 很多规则已经改了，在我现在手头这台 M4 的 Mac 上：
>
> - nginx 的安装地址是 `/opt/homebrew/bin/nginx`
>
> - nginx 配置文件夹的地址是 `/opt/homebrew/etc/nginx` 
>   -  conf 文件的地址是 `/opt/homebrew/etc/nginx/nginx.conf`



#### nginx 常用命令

```sh
$ nginx         # 启动 nginx
$ nginx -s quit # 快速停止 nginx
$ nginx -V   	  # 查看版本，以及配置文件地址
$ nginx -v   	  # 查看版本
$ nginx -t      # 测试 nginx 配置，并离开（命令行中将会显示 ngnix 所在配置路径 ），因此可以通过该命令找到当前机器上的 nginx 配置的位置
$ nginx -s reload | reopen | stop | quit # 重新加载配置 ⭐️ | 重启 | 快速停止 | 安全关闭 ⭐️ nginx 
```

```bash
$ nginx -h         # 帮助
nginx version: nginx/1.23.1
Usage: nginx [-?hvVtTq] [-s signal] [-p prefix]
             [-e filename] [-c filename] [-g directives]

Options:
  -?,-h         : this help
  -v            : show version and exit
  -V            : show version and configure options then exit
  -t            : test configuration and exit
  -T            : test configuration, dump it and exit
  -q            : suppress non-error messages during configuration testing
  -s signal     : send signal to a master process: stop, quit, reopen, reload
  -p prefix     : set prefix path (default: /usr/local/Cellar/nginx/1.23.1/)
  -e filename   : set error log file (default: /usr/local/var/log/nginx/error.log)
  -c filename   : set configuration file (default: /usr/local/etc/nginx/nginx.conf)
  -g directives : set global directives out of configuration file
```

摘自：[mac下nginx的安装和配置](https://www.jianshu.com/p/026d67cc6cb1)



#### nginx 配置文件

nginx 配置文件( `/usr/local/etc/nginx/nginx.conf` ) 由三部分组成：

##### 全局块

从<font color=FF0000>**配置文件开始**</font>到<font color=FF0000>**event块**</font>之间的内容，主要设置<font color=FF0000>**影响nginx服务器整体运行的配置指令**</font>

```nginx
#user  nobody;
worker_processes  1;  #表示nginx处理并发的最大数量，worker_processes值越大，可以处理的并发处理量越多

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;
```

##### events 块

events 块涉及的指令<font color=FF0000>主要影响Nginx服务器与用户的网络连接</font>，常用的设置包括是否开启对多 work process 下的网络连接进行序列化、是否允许同时接收多个网络连接、选取哪种事件驱动模型来处理连接请求、每个 word process 可以同时支持的最大连接数等。 

```nginx
events {
    worker_connections  1024; # nginx所支持的最大连接数
}
```

##### http 块

这里是<font color=FF0000>nginx服务器中配置最为频繁的部分</font>，代理、缓存和日志定义等绝大多数功能和第三方配置都在这里。

需要注意的是，<font color=FF0000>**http块也可以包括：http全局块、server块**</font>

- http全局块包括文件引入、MIME-TYPE 定义、日志自定义、连接超时时间、单连接请求数上限等

- 这块和虚拟主机有密切关系，虚拟主机从用户角度看，和一台独立的硬件主机是完全一样的，该技术的产生是为了节省互联网服务器硬件成本。 


每个 http 块可以包括多个 server 块，而每个 server 块就相当于一个虚拟主机。 

而每个 server 块也分为全局 server 块，以及可以同时包含多个 locaton 块。 

###### 全局 server 块 

最常见的配置是本虚拟机主机的监听配置和本虚拟主机的名称或IP配置。 

###### location 块 

一个 server 块可以配置多个 location 块。 

这块的主要作用是基于 Nginx 服务器接收到的请求字符串（例如 `server_name/uri-string` ），对虚拟主机名称（也可以是IP别名）之外的字符串（例如 前面的 `/uri-string` ）进行匹配，对特定的请求进行处理。地址定向、数据缓存和应答控制等功能，还有许多第三方模块的配置也在这里进行。 



#### 前端必备知识之Nginx 笔记

##### 反向代理

见 [[#反向代理#进一步解释]] ，这里略

##### 配置重载

Nginx 修改完配置之后需要<font color=red>重载</font>，可以停止进程后再运行（👀 就像是 webpack ）；或者更简单的方法是使用 `nginx -s reload`

##### 如何在一台服务器部署多个网站

作为个人开发者来说，大多数情况下只有一个服务器，但是还想部署多个项目，这时该怎么办。以前端来举例，在自己电脑下开发项目的时候，同时启动了前端项目假如在 **8080 端口**，同时启动了后端 **node 项目的 3000 端口**，可以通过两个端口启动了两个服务，那么在服务端也是同理，多个服务只需要运行在不同端口即可，在 **Nginx** 的配置中，只需要根据用户访问的地址来指向不同的端口就行。示例：

```nginx
server {
    listen        80;
    server_name   chat.jiangly.com;
    location / {
        proxy_pass   http://127.0.0.1:7000;
    }   
}
```

监听 80 端口，当访问的域名是 chat.jiangly.com 时指向 7000 端口，7000 端口就是项目运行的真实端口，很明显这是个后端 node 服务

如果是前端静态项目怎么办，没有端口直接访问了，没有 `proxy_pass` 了。解决方法如下：

```nginx
server {
        listen 80;
        server_name project1.jaingly.com;

        location / { 
                root /data/project1/;
                index index.html;
        }
}
```

发现 `proxy_pass` 变成了 root，root 指向了一个地址，那么就把项目放在这个地址即可。

这样，就可以同时部署好多网站了，既可以访问前端静态网站，也可以配置 node 后端项目。

如果没有域名的话，`server_name` 就需要变成 `127.0.0.1` 加端口号或者项目地址了，比如 `127.0.0.1:7000`、`127.0.0.1/project1` 这样来区分项目。需要注意的是，这里使用的是相对路径是以 `nginx -t`显示的配置文件 `nginx.conf` 那一层为对比的路径，别放错文件。

##### 使用 HTTPS 服务

```nginx
server {
    listen 443 ssl;
    server_name   chat.jiangly.com;
    ssl_certificate conf.d/chat.jiangly.com_ssl/1_chat.jiangly.com_bundle.crt; 
    ssl_certificate_key conf.d/chat.jiangly.com_ssl/2_chat.jiangly.com.key; 
    ssl_session_timeout 5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; 
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE; 
    ssl_prefer_server_ciphers on;
    location / {
        proxy_pass   http://127.0.0.1:7000;
        root html; 
        index  index.html index.htm;
    }       
}
```

##### Websocket服务

开发项目时可能会经常使用到 websocket 服务，如果还用上面的配置，会发现，请求被拦截掉了。因为使用 ws 服务的时候需要告诉nginx 需要对协议进行升级，所以遇到这个问题时只需要增加两行配置即可：

```nginx
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection $connection_upgrade;
```

##### server匹配规则

上面写很多组的 server，<font color=fuchsia>**Nginx 是从上往下匹配的，当匹配到第一个之后就会立即退出**</font>。所以如果遇到配置怎么也不生效时，先看看是不是在上面被其他规则匹配到了。

##### history路由模式、跨域、缓存、反向代理

```nginx
# html设置 history 模式
location / {
    index index.html index.htm;
    proxy_set_header Host $host;
    # history 模式最重要就是这里    try_files $uri $uri/ /index.html;
    # index.html 文件不可以设置强缓存 设置协商缓存即可
    add_header Cache-Control 'no-cache, must-revalidate, proxy-revalidate, max-age=0';
}

# 接口反向代理
location ^~ /api/ {
    # 跨域处理 设置头部域名
    add_header Access-Control-Allow-Origin *;
    # 跨域处理 设置头部方法
    add_header Access-Control-Allow-Methods 'GET,POST,DELETE,OPTIONS,HEAD';
    # 改写路径
    rewrite ^/api/(.*)$ /$1 break;
    # 反向代理
    proxy_pass http://static_env;
    proxy_set_header Host $http_host;
}

location ~* .(?:css(.map)?|js(.map)?|gif|svg|jfif|ico|cur|heic|webp|tiff?|mp3|m4a|aac|ogg|midi?|wav|mp4|mov|webm|mpe?g|avi|ogv|flv|wmv)$ {
    # 静态资源设置七天强缓存
    expires 7d;
    access_log off;
}
```

##### 负载均衡

<font color=red>可以基于 upstream 模块来做负载均衡</font>，也就是设置权重以及配置地址

```nginx
upstream backserver{ 
    # 哈希算法，自动定位到该服务器 保证唯一ip定位到同一部机器 用于解决session登录态的问题
    ip_hash; 
    server 127.0.0.1:9090 down;     # down 表示单前的 server暂时不参与负载
    server 127.0.0.1:8080 weight=2; # weight 默认为1，weight越大，负载的权重就越大
    server 127.0.0.1:6060; 
    server 127.0.0.1:7070 backup;   # 其它所有的非backup机器down或者忙的时候，请求backup机器
} 
```

可以看到四个服务都是我们本机，正式的生产场景中，应该是别的服务器的 ip 地址。一般 nginx 会独立部署到一台服务器，其他的服务会部署在其他服务器，所以在这样的场景下，需要这多台服务器在一个内网环境中，否则如果走公网，那么就会白白增加耗时。

学习自：[前端必备知识之Nginx](https://juejin.cn/post/7108394145068089374)

> 👀 感觉这个文章写的很不清楚，不过优点是把场景与对应配置都列了出来，这里便把配置摘抄下来；未来如果有补充，或有更好的选择，也可以修改或删除。另外，可以看下 [[计算机网络#Nginx 与配置]] 互为补充。



## 配置与解释

##### location

> [!NOTE]
> 
> 一开始看到 “server block” 和 “location block” 感觉有点懵，看到更多配置才知道 `server { ... }` 块 和 `location { ... }` 块

The location directive within NGINX server block <font color=red>allows to route request to correct location within the file system</font>. The directive is <font color=red>used to tell NGINX **where to look for a resource** by including files and folders while **matching a location block against an URL**</font>. In this tutorial, we will look at NGINX location directives in details.

###### NGINX location directive syntax

The NGINX <font color=red>location block can be placed inside a server block or inside another location block with some restrictions</font>. <font color=dodgerBlue>The syntax for constructing a location block is</font>:

```nginx
location [modifier] [URI] {
  ...
  ...
}
```

The <font color=red>**modifier in the location block is optional**</font>. Having a modifier in the location block will allow NGINX to treat a URL differently. <font color=dodgerBlue>Few most common modifiers are</font>:

- none : If <font color=dodgerBlue>**no modifiers**</font> are present in a location block then <font color=red>the requested URI will be matched against the beginning of the requested URI</font>.
- `=` : The equal sign is used to match a location block exactly against a requested URI.
- `~` : The tilde sign is used for <font color=red>case-sensitive regular expression match</font> against a requested URI.
- `~*` : The tilde followed by asterisk sign is used for <font color=red>case insensitive regular expression match</font> against a requested URI.
- `^~` : The carat followed by tilde sign is used to <font color=red>perform longest nonregular expression match</font> against the requested URI. <font color=red>**If the requested URI hits such a location block, no further matching will takes place**</font>.

> [!TIP]
> 
> 这里没完全说明白，问了下 Gemini ，这里做下摘抄：
>
> 🔗 https://g.co/gemini/share/1ffc5b9ee594
> 
> 此外，看了下 [前端到底用Nginx来做啥](https://juejin.cn/post/7064378702779891749) 中的总结，感觉很简单，感觉是想多了：
> 
> - `=` 表示精确匹配。只有请求的 url 路径与后面的字符串完全相等时，才会命中。
> - `^~` 表示如果该符号后面的字符是最佳匹配，采用该规则，不再进行后续的查找。
> - `~` 表示该规则是使用正则定义的，区分大小写。
> - `~*` 表示该规则是使用正则定义的，不区分大小写。

###### How NGINX choose a location block

A location can be defined by using a prefix string or by using a regular expression. Case-insensitive regular expressions are specified with preceding `~*` modifier and for a case-insensitive regular expression, the `~` modifier is used. To find a location match for an URI, NGINX first scans the locations that is defined using the prefix strings (without regular expression). Thereafter, the location with regular expressions are checked in order of their declaration in the configuration file. NGINX will run through the following steps to select a location block against a requested URI.

- NGINX starts with looking for an exact match specified with `location = /some/path/` and if a match is found then this block is served right away.
- If there are no such exact location blocks then NGINX proceed with matching longest non-exact prefixes and if a match is found where `^~` modifier have been used then NGINX will stop searching further and this location block is selected to serve the request.
- If the matched longest prefix location does not contain `^~` modifier then the match is stored temporarily and proceed with following steps.
  - NGINX now shifts the search to the location block containing `~` and `~*` modifier and selects the first location block that matches the request URI and is immediately selected to serve the request.
  - If no locations are found in the above step that can be matched against the requested URI then the previously stored prefix location is used to serve the request.



##### proxy_pass