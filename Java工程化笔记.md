# Java工程化笔记



### 日志

##### `@Slf4j`

**`@Slf4j`** 是 Java 开发中由 **Project Lombok** 提供的一个**类注解**。它的核心作用是**自动在类中生成一个 SLF4J 的 `Logger` 对象**，从而省去手写初始化日志对象的繁琐样板代码。

编译时，Lombok 会自动为你注入

```java
private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(UserServiceImpl.class);
```

###### 核心特点

- **自动生成变量名**：默认生成的日志实例名称固定叫 **`log`**。
- **轻量与解耦**：它只是 Lombok 在编译期的代码糖，<font color=red>底层依然依赖 **SLF4J**（Simple Logging Facade for Java）统一日志门面，不绑定具体的日志实现（如 Logback、Log4j2 等）</font>。
- **常用日志级别**：结合 SLF4J 提供的 API，可以直接通过 `log.error()`、`log.warn()`、`log.info()`、 `log.debug()`、`log.trace()` 输出日志。
- **占位符性能优化**：支持 `{}` 占位符拼接字符串，避免在日志级别关闭时进行不必要的字符串构造开销。



#### Logback