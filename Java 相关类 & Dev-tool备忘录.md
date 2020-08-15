# Java 相关类 & Dev-tool备忘录



### apache.commons

Apache Commons 是对 JDK 的拓展，包含了很多开源的工具，用于解决平时编程经常会遇到的问题，减少重复劳动。

- Commons BeanUtils：针对 Bean 的一个工具集。由于 Bean 往往是有一堆 get 和 set 组成，所以 BeanUtils 也是在此基础上进行一些包装。一个比较常用的功能是 Bean Copy，也就是 copy bean 的属性。如果做分层架构开发的话就会用到，比如从 PO（Persistent Object）拷贝数据到 VO（Value Object）

- Commons Codec：是编码和解码组件，提供常用的编码和解码方法，如 DES、SHA1、MD5、Base64、URL 和 Soundx 等。

- Commons Collections：是一个集合组件，扩展了 Java 标准 Collections API，对常用的集合操作进行了很好的封装、抽象和补充，在保证性能的同时大大简化代码。

- Commons Compress：是一个压缩、解压缩文件的组件，可以操作 rar、cpio、Unix dump、tar、zip、gzip、XZ、Pack200 和 bzip2 格式的压缩文件。

- Commons Configuration：是一个 Java 应用程序的配置管理工具，可以从 properties 或者 xml 文件中加载配置信息。

- Commons CSV：是一个用来读写各种 Comma Separated Value(CSV) 格式文件的 Java 类库。

- Commons Daemon：实现将普通的 Java 应用变成系统的后台服务, 例如 Tomcat 就是利用这个项目来实现作为 Linux 和 Windows 的服务启动和停止的。

-  Commons DBCP：数据库连接池。

- Commons DBUtils：是 JDBC 工具组件，对传统操作数据库的类进行二次封装，可以把结果集转化成 List。

-  Commons Digester：是 XML 到 Java 对象的映射工具集。

-  Commons Email：是邮件操作组件，对 Java Mail API 进行了封装，提供了常用的邮件发送和接收类，简化邮件操作。该组件依赖 Java Mail API。

- Commons Exec：提供一些常用的方法用来执行外部进程，如执行 exe 文件或命令行。

-  Commons FileUpload：为 Web 应用程序或 Servlet 提供文件上传功能，Struts2 和 SpringMVC 的文件上传组件。

- Commons IO：是处理 IO 的工具类包，对 java.io 进行扩展，提供了更加方便的 IO 操作。

- Commons JCI：提供通用的 Java 编译器接口。

- Commons Lang3：是处理 Java 基本对象方法的工具类包，该类包提供对字符、数组等基本对象的操作，弥补了 java.lang api 基本处理方法上的不足。

- Commons Launcher：可以跨平台独立启动的 java 应用程序。

- Commons Logging：提供统一的日志接口，同时兼顾轻量级和不依赖于具体的实现。类包给中间件 / 日志工具开发者一个简单的日志操作抽象，允许程序开发人员使用不同的具体日志实现工具。

- Commons Math：轻量级自容器的数学和统计计算方法类包，包含大多数常用的数值算法。

- Commons Net：封装了各种网络协议的客户端，支持 FTP、NNTP、SMTP、POP3、Telnet 等协议。

- Commons Pool：提供了一整套用于实现对象池化的框架，以及若干各具特色的对象池实现，可以有效地减少处理对象池化时的工作量。类包用于提高像文件句柄、数据库连接、socket 通信这类大对象的调用效率，简单的说就是一种对象一次创建多次使用的技术。

-  Commons Primitives：提供了一个更小，更快和更易使用的对 Java 基本类型的支持。

- Commons Validator：提供了一个简单的、可扩展的框架来在一个 XML 文件中定义校验器 (校验方法) 和校验规则。支持校验规则的和错误消息的国际化。

-  Apache HttpClient：曾经是 Apache Commons 的子项目，后来独立出来。HttpClient 简化 HTTP 客户端与服务器的各种通讯，实现 HTTP 客户端程序（也就是浏览器程序）的功能。

摘自：[关于 APACHE COMMONS 的简介](https://www.cnblogs.com/zhuchaoli/p/10317303.html)

这里只是简介：更多内容可以看：[Apache Commons 系列简介 之 BeanUtils](http://www.blogways.net/blog/2014/01/15/apache-commons-beanutils.html)以及后面三篇文章

***

### log4j

##### **Logging 级别**

`org.apache.Log4j.Level` 类定义了日志级别，您可通过继承 `Level` 类定制自己的级别。

| 级别  |                       描述                       | Spring Boot中的颜色 |
| :---: | :----------------------------------------------: | :-----------------: |
|  ALL  |             所有级别，包括定制级别。             |                     |
| DEBUG |      指明细致的事件信息，对调试应用最有用。      |        Green        |
| ERROR |      指明错误事件，但应用可能还能继续运行。      |         Red         |
| FATAL | 指明非常严重的错误事件，可能会导致应用终止执行。 |         Red         |
| INFO  |   指明描述信息，从粗粒度上描述了应用运行过程。   |        Green        |
|  OFF  |             最高级别，用于关闭日志。             |                     |
| TRACE |            比 DEBUG 级别的粒度更细。             |        Green        |
| WARN  |               指明潜在的有害状况。               |       Yellow        |

**级别工作原理：**

在一个级别为 `q` 的 logger 对象中，一个级别为 `p` 的日志请求在 p >= q 的情况下是**开启**的。该规则是 Log4j 的核心，它假设级别是有序的。对于标准级别，其顺序为：<font color=FF0000>ALL < DEBUG < INFO < WARN < ERROR < FATAL < OFF</font>。

摘自：[极客大学 - Logging 级别](https://wiki.jikexueyuan.com/project/log4j/logging-levels.html) ｜ [Spring Boot文档 - Logging](https://docs.spring.io/spring-boot/docs/2.1.6.RELEASE/reference/html/boot-features-logging.html)



### Flyway (Maven plugin)

#### Goals

|                           **Name**                           |                       **Description**                        |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| [migrate](https://flywaydb.org/documentation/maven/migrate)  |                    Migrates the database                     |
|   [clean](https://flywaydb.org/documentation/maven/clean)    |         Drops all objects in the configured schemas          |
|    [info](https://flywaydb.org/documentation/maven/info)     | Prints the details and status information about all the migrations |
| [validate](https://flywaydb.org/documentation/maven/validate) | Validates the applied migrations against the ones available on the classpath |
| [undo](https://flywaydb.org/documentation/maven/undo)  ([Flyway Pro](https://flywaydb.org/download)) |     Undoes the most recently applied versioned migration     |
| [baseline](https://flywaydb.org/documentation/maven/baseline) | Baselines an existing database, excluding all migrations up to and including baselineVersion |
|  [repair](https://flywaydb.org/documentation/maven/repair)   |               Repairs the schema history table               |

