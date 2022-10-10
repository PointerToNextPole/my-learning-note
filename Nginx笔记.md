# Nginx 笔记



### 基本概念

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

- nginx安装文件目录： `/usr/local/Cellar/nginx`
- nginx配置文件目录：  `/usr/local/etc/nginx`
- config文件目录：  `/usr/local/etc/nginx/nginx.conf`
- 系统hosts位置：  `/private/etc/hosts`





#### nginx 常用命令

```bash
$ nginx                                  # 启动 nginx
$ nginx -s quit  												 # 快速停止 nginx
$ nginx -V   														 # 查看版本，以及配置文件地址
$ nginx -v   														 # 查看版本
$ nginx -t                               # 测试 nginx 配置，并离开（命令行中将会显示 ngnix 所在配置路径 ）
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

##### ngnix -t

```bash
$ nginx -t
nginx: the configuration file /usr/local/etc/nginx/nginx.conf syntax is ok
nginx: configuration file /usr/local/etc/nginx/nginx.conf test is successful
```



#### nginx 配置文件

nginx 配置文件（`/usr/local/etc/nginx/nginx.conf`）由三部分组成：

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

- 这里是<font color=FF0000>nginx服务器中配置最为频繁的部分</font>，代理、缓存和日志定义等绝大多数功能和第三方配置都在这里。

- 需要注意的是，<font color=FF0000>**http块也可以包括：http全局块、server块**</font>

  - http全局块包括文件引入、MIME-TYPE 定义、日志自定义、连接超时时间、单连接请求数上限等

  - 这块和虚拟主机有密切关系，虚拟主机从用户角度看，和一台独立的硬件主机是完全一样的，该技术的产生是为了节省互联网服务器硬件成本。 

    每个 http 块可以包括多个 server 块，而每个 server 块就相当于一个虚拟主机。 

    而每个 server 块也分为全局 server 块，以及可以同时包含多个 locaton 块。 

    **1、全局 server 块** 

    最常见的配置是本虚拟机主机的监听配置和本虚拟主机的名称或IP配置。 

    **2、location 块** 

    一个 server 块可以配置多个 location 块。 

    这块的主要作用是基于 Nginx 服务器接收到的请求字符串（例如 server_name/uri-string），对虚拟主机名称（也可以是IP别名）之外的字符串（例如 前面的 /uri-string）进行匹配，对特定的请求进行处理。地址定向、数据缓存和应答控制等功能，还有许多第三方模块的配置也在这里进行。 



#### 前端必备知识之Nginx 笔记

##### 反向代理

见 [[#反向代理#进一步解释]] ，这里略

##### 配置重载

Nginx 修改完配置之后需要<font color=red>重载</font>，可以停止进程后再运行（👀 就像是 webpack ）；或者更简单的方法是使用 `nginx -s reload`

##### 如何在一台服务器部署多个网站



学习自：[前端必备知识之Nginx](https://juejin.cn/post/7108394145068089374)

