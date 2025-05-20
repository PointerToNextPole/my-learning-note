# Java SE 备忘录



在 macOS 系统下，Java 的安装目录在 `/Library/java/JavaVirtualMachines`，而且是所有的版本的 Java



#### JDK & JRE

- **JRE**（Java Runtime Environment java运行时环境）包含了java虚拟机，java基础类库。是使用java语言编写的程序运行所需要的软件环境，<font color=FF0000>是提供给想运行java程序的**用户**使用的</font>。

- **JDK**（Java Development Kit java开发工具包）是程序员使用java语言编写java程序所需的开发工具包，<font color=FF0000>是提供给程序员使用的</font>。JDK包含了JRE，同时还包含了编译java源码的编译器javac，还包含了很多java程序调试和分析的工具：jconsole，jvisualvm等工具软件，还包含了java程序编写所需的文档和demo例子程序。


###### 总结

如果你需要运行 java 程序，只需安装 JRE 就可以了。如果你需要编写java程序，需要安装JDK。

JRE根据不同操作系统（如：windows，linux等）和不同JRE提供商（IBM,ORACLE等）有很多版本。

摘自：[JRE 和 JDK 的区别是什么？ - 王博的回答 - 知乎](https://www.zhihu.com/question/20317448/answer/14737358)

###### 补充

<img src="https://i.loli.net/2020/08/16/T3Ldk5zv4Pl7JAR.png" style="zoom:50%;" />

另外，最新的 jdk 已经不包含 jre 了，判断依据：是否有 `javac` 命令



#### Java 除法

整形除以整形，结果等于<font color=FF0000>**整形**</font>



#### Java泛型通配符

- `?` 表示不确定的 Java 类型
- `T` ( type ) 表示具体的一个 Java类型
- `K V` (key value) 分别代表 Java 键值中的 Key Value
- `E` ( element ) 代表 Element

#### Java泛型方法

定义泛型方法时，必须<font color=FF0000>在返回值前</font>加一个 `<T>` ，来声明这是一个泛型方法，持有一个泛型`T` ，然后才可以用泛型 `T` 作为方法的返回值。示例如下：

```java
public <T> T methodName(T paramName){
  ...
}
```



#### Java iterator



#### Java synchronized



#### Java 环境变量 PATH 和 CLASSPATH

- `PATH` 是用来配置 JDK，`%JAVA_HOME%/bin/`的配置
- `CLASSPATH` 的作用是 <font color=lightSeaGreen>指定 Java 类所在的目录</font>



#### Java 方法引用（Method references）` ::`

形如 `ClassName::methodName` 或者 `objectName::methodName` 的表达式，我们把它叫做方法引用 ( Method Reference )。它的原型来自：eta conversion (η-conversion)

###### 方法引用有四种

- 指向静态方法的引用
- 指向某个对象的实例方法的引用
- 指向某个类型的实例方法的引用
- 指向构造方法的引用

摘自：[Java 8 方法引用](http://liwenkun.me/2017/03/23/java-8-method-references/)



#### Java流式编程



#### try-with-resources

> 👀 这个是学习 TypeScript 5.2 `using` 关键字顺带学习的内容

“try-with-resources” 声明是 <font color=red>Java 7 中引入的一个异常处理特性</font>，旨在更简洁、更安全地管理实现了 `java.lang.AutoCloseable` 或 `java.io.Closeable` 接口的资源。这些资源通常是那些在使用完毕后需要显式关闭以释放系统资源的对象，例如文件流、数据库连接、网络连接等。

###### 引入背景（传统方式的问题）

在 Java 7 之前，当我们需要使用这类资源时，通常采用 `try-catch-finally` 结构来确保资源在使用完毕后能够被正确关闭。一个典型的例子如下：

```java
FileInputStream fis = null;
try {
    fis = new FileInputStream("file.txt");
    // 使用 fis 进行文件读取操作
    // ...
} catch (IOException e) {
    // 处理异常
    e.printStackTrace();
} finally {
    if (fis != null) {
        try {
            fis.close();
        } catch (IOException e) {
            // 处理关闭时的异常
            e.printStackTrace();
        }
    }
}
```

这种方式存在一些问题：

1. **代码冗余且复杂** ：`finally` 块中的关闭逻辑，尤其是嵌套的 `try-catch`（用于处理 `close()` 方法本身可能抛出的异常），使得代码显得冗长且不易阅读。
2. **容易出错** ：<font color=lightSeaGreen>开发者很容易忘记在 `finally` 块中关闭资源</font>，或者<font color=red>在关闭资源时没有正确处理可能发生的异常，从而导致资源泄露</font>。
3. **异常屏蔽 (Exception Masking/Suppressed Exceptions)** ： <font color=red>如果在 `try` 块和 `finally` 块中的 `close()` 方法都抛出了异常，那么 `try` 块中的原始异常可能会被 `finally` 块中的异常所覆盖，导致难以追踪到问题的根本原因</font>。

##### try-with-resources 的引入与优势

为了解决上述问题，Java 7 引入了 try-with-resources 声明。它的<font color=dodgerBlue>核心思想是</font>：<font color=red>**在 `try` 关键字后面的圆括号中声明并初始化一个或多个资源，这些资源会在 `try` 语句块执行完毕后（无论是正常结束还是因异常退出）被自动关闭**</font>

其基本语法如下：

```java
try (ResourceType resource1 = initialization1;
     ResourceType resource2 = initialization2;
     // ... 可以声明多个资源，用分号隔开
    ) {
    // 使用 resource1, resource2 等资源进行操作
    // ...
} catch (ExceptionType e) {
    // 处理在 try 块中或资源初始化、自动关闭过程中发生的异常
    e.printStackTrace();
}
// finally 块是可选的，如果需要，仍然可以添加
// finally {
//    // 一些其他的清理工作，但不需要在这里关闭 try(...) 中声明的资源
// }
```

##### 核心特点与优势

1. **自动资源管理 (Automatic Resource Management - ARM)** ：这是最核心的优势。开发者不再需要显式地在 `finally` 块中调用 `close()` 方法。JVM 会自动确保在 `try` 块结束时，按照资源声明的相反顺序调用它们的 `close()` 方法。
2. **代码简洁性** ： 大大减少了模板代码，使得代码更加清晰易读。
3. **减少资源泄露风险** ： 由于资源会自动关闭，因此因忘记关闭资源而导致的泄露风险显著降低。
4. **改进的异常处理 (Suppressed Exceptions)** ： 如果在 `try` 块中和资源的 `close()` 方法中都发生了异常，`close()` 方法中抛出的异常会被 “抑制” ( suppressed )，而 `try` 块中的原始异常会被优先抛出。被抑制的异常可以通过 `Throwable.getSuppressed()` 方法获取，这样就不会丢失重要的异常信息。

##### 使用 try-with-resources 的条件

<font color=dodgerBlue>要使一个资源能够被 try-with-resources 语句管理</font>，<font color=red>该资源的类必须实现 `java.lang.AutoCloseable` 接口（或其子接口 `java.io.Closeable` ）</font>。这两个接口都定义了一个 `close()` 方法。`AutoCloseable` 接口的 `close()` 方法声明抛出 `Exception`，而 `Closeable` 接口的 `close()` 方法声明抛出 `IOException`。

##### 示例

使用 try-with-resources 读取文件的例子：

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class TryWithResourcesExample {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("example.txt"))) {
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.err.println("读取文件时发生错误: " + e.getMessage());
            // 打印被抑制的异常 (如果 close() 方法也抛出异常)
            for (Throwable suppressed : e.getSuppressed()) {
                System.err.println("被抑制的异常: " + suppressed);
            }
        }
    }
}
```

在这个例子中：

- `BufferedReader br = new BufferedReader(new FileReader("example.txt"))` 在 `try` 后的圆括号中声明和初始化。
- `BufferedReader` 和 `FileReader` 都实现了 `Closeable` 接口。
- 无论 `try` 块中的代码是正常执行完毕还是因 `IOException` 退出，`br` 对象（以及内部的 `FileReader` 对象）的 `close()` 方法都会被自动调用。
- 如果 `br.close()` 方法本身也抛出 `IOException`，并且 `try` 块中已经有一个 `IOException` 发生，那么 `close()`的异常会被作为“被抑制的异常”添加到原始异常中

##### 多个资源的情况

<font color=red>可以声明多个资源，它们会按照声明的相反顺序关闭</font>。

```java
try (java.util.zip.ZipFile zf = new java.util.zip.ZipFile(zipFileName);
     java.io.BufferedWriter writer = java.nio.file.Files.newBufferedWriter(outputFilePath, charset)) {
    // 使用 zf 和 writer
} catch (IOException e) {
    // 处理异常
}
```

在这个例子中，`writer` 会先于 `zf` 关闭。

**总结：**

try-with-resources 是 Java 中一个非常实用的特性，它极大地简化了资源管理代码，提高了代码的可读性和健壮性，并有效防止了资源泄露。在处理需要关闭的资源时，强烈推荐使用 try-with-resources 语句。

摘自 Gemini 的回答 🔗 https://g.co/gemini/share/6c62bab7919d
