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



#### Java 环境变量 PATH 和 CLASSPATH

- `PATH` 是用来配置 JDK，`%JAVA_HOME%/bin/`的配置
- `CLASSPATH` 的作用是 <font color=lightSeaGreen>指定 Java 类所在的目录</font>



#### Java 除法

整形除以整形，结果等于<font color=FF0000>**整形**</font>



#### 泛型

##### Java 泛型通配符

- `?` 表示不确定的 Java 类型
- `T` ( type ) 表示具体的一个 Java类型
- `K V` (key value) 分别代表 Java 键值中的 Key Value
- `E` ( element ) 代表 Element

##### Java 泛型方法

定义泛型方法时，必须<font color=FF0000>在返回值前</font>加一个 `<T>` ，来声明这是一个泛型方法，持有一个泛型 `T` ，然后才可以用泛型 `T` 作为方法的返回值。示例如下：

```java
public <T> T methodName(T paramName) {
  ...
}
```



#### Java iterator



#### Java synchronized



#### Java 方法引用（Method references）` ::`

形如 `ClassName::methodName` 或者 `objectName::methodName` 的表达式，我们把它叫做方法引用 ( Method Reference )。它的原型来自：eta conversion (η-conversion)

###### 方法引用有四种

- 指向静态方法的引用
- 指向某个对象的实例方法的引用
- 指向某个类型的实例方法的引用
- 指向构造方法的引用

摘自：[Java 8 方法引用](http://liwenkun.me/2017/03/23/java-8-method-references/)



#### try-with-resources

> [!NOTE]
>
> 这是学习 TypeScript 5.2 `using` 关键字顺带学习的内容

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



### 数据结构

#### record

Java `record` 是一种简化的数据类声明方式，于 Java 16 正式发布：

```java
public record User(String name, int age) {}
```

编译器会自动生成：

- `private final` <font color=red>**字段**</font>
- 构造方法
- 访问方法 `name()`、`age()`
- `equals()`
- `hashCode()`
- `toString()`

##### `public` 与 `private final` 的作用对象

下面定义中的 `public` 修饰的是 `User` 类型，表示这个类型可以被其他包访问：

```java
public record User(String name, int age) {}
// ↑ public 修饰 User 类型
```

括号中的 `name` 和 `age` 称为 **record components（记录组件）**，它们不是 `public` 字段。编译器会根据记录组件生成 `private final` 字段，以及对应的 `public` 访问方法。上面的 record 在成员结构上大致等价于下面的概念代码（普通类不能在源码中直接继承 `java.lang.Record`）：

```java
public final class User extends Record {
    private final String name;
    private final int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String name() {
        return name;
    }

    public int age() {
        return age;
    }

    // 还有自动生成的 equals()、hashCode() 和 toString()
}
```

因此，外部可以通过访问方法读取记录组件，但不能直接访问或重新赋值内部字段：

```java
User user = new User("Alice", 20);

user.name();   // 可以，通过 public 访问方法读取
user.age();    // 可以
user.name;     // 编译错误：字段是 private
user.age = 21; // 编译错误：字段是 private final
```

如果去掉 record 声明开头的 `public`：

```java
record User(String name, int age) {}
```

变化的是 `User` 类型成为包级可见，内部字段仍然是 `private final`。

使用方式：

```java
User user = new User("Alice", 20);

System.out.println(user.name());
System.out.println(user.age());
System.out.println(user); // User[name=Alice, age=20]
```

它适合 DTO、接口返回值、值对象等以“承载数据”为主的类型。

需要注意，record 只是**浅层不可变**：

```java
public record User(String name, List<String> roles) {}
```

`roles` 引用不能重新赋值，但其中的内容仍可能被修改。需要真正不可变时，可以进行防御性复制：

```java
public record User(String name, List<String> roles) {
    public User {
        roles = List.copyOf(roles);
    }
}
```

Java record 本质上仍是一个真正的类：

- 运行时存在
- 可以实现接口
- 可以添加方法和校验逻辑
- 不能继承其他类
- record 类本身隐式为 `final`
- 隐式继承 `java.lang.Record`



#### ImmutableSet

ImmutableSet（不可变集合）是一种<font color=red>在初始化后无法被修改的 Set 集合</font>。任何试图对其进行结构性修改（如 `add`、`remove`、`clear` 或 `retainAll`）的操作，都会直接抛出 `UnsupportedOperationException`。

在 Java 中主要有两种主要的实现途径：**Java 9 及以上版本的标准库**，以及 **Google Guava 库**。

###### 核心特点与优势

1. **线程安全（Thread-Safe）**：由于不可变，多个线程同时读取时无需任何同步加锁操作，天生线程安全。
2. **拒绝 Null 值**：无论是标准库的 `Set.of()` 还是 Guava 的 `ImmutableSet`，均<font color=red>不允许存入 `null` 元素</font>（若传入 `null` 会抛出 `NullPointerException`），这有效避免了常见的 `NullPointerException` 隐患。
3. **内存与性能优化**：底层通常采用紧凑的存储结构（如优化过哈希冲突的数组），比标准的 `HashSet` 占用更少的内存。
4. **防御性编程**：作为方法返回值或常量时，可以防止调用方不小心篡改内部数据。

##### 使用方式

###### Java 9+ 标准库方式 (`java.util.Set`)

```java
import java.util.Set;
import java.util.List;

// 1. 使用 Set.of 直接创建
Set<String> languages = Set.of("Java", "Python", "Go");

// 2. 从现有集合复制创建
List<String> list = List.of("A", "B", "C");
Set<String> copySet = Set.copyOf(list);
```

**注意事项**

- `Set.of()` 不能包含重复元素，否则会抛出 `IllegalArgumentException`。
- `Set.of()` 对元素的传入顺序不作保证（虽然打印时可能看起来有序）。
- 最多支持传入一定数量的参数（`Set.of()` 重载方法最多支持 10 个独立元素，若更多需使用 `Set.of(E... elements)` 可变参数，但有性能和重复项检查开销）。





### Stream API

#### stream 方法

##### stream 不可替代的原因

###### 链式流水线处理（Pipeline）

现代编程（例如 JavaScript 数组的 `.filter().map().reduce()`）非常依赖链式调用来处理数据。 <font color=red>Java 的 `List` 接口本身并没有提供 `.filter()` 或 `.map()` 方法</font>。`stream()` 的核心作用就是**把一个集合桥接到一个“流管道”中**，可以流畅地组合多个操作：

```java
// 找出所有大于 10 的数字，平方后降序排序，最后取前 3 个
List<Integer> result = list.stream()
    .filter(n -> n > 10)           // 过滤
    .map(n -> n * n)               // 映射转换
    .sorted((a, b) -> b - a)       // 排序
    .limit(3)                      // 限制数量
    .collect(Collectors.toList()); // 收集为新 List
```

如果没有 `stream()`，用传统的 `for` 循环或直接在 `List` 上操作，代码会变得非常臃肿且难以维护。

###### 非破坏性（保护原数据）

`List` 原生方法（如 `removeIf`、`replaceAll`、`sort`）大多是原地修改（In-place）的，会直接破坏或改变原集合的内容。 而 Stream 操作默认是非破坏性的：

- 它的过滤、映射、排序等操作都会产生**新的流或结果**。
- 原有的 `List` 保持不变，这在多线程或需要保证数据安全（Immutability）的场景下至关重要。

###### 强大的高级聚合与分组（类似 SQL）

集合自带的 Lambda 方法（如 `forEach`）无法进行复杂的统计和聚合。而 Stream 配合 `Collectors` 可以轻松实现类似 SQL 的强大功能：

- **分组 (`groupingBy`)**：按某个属性将员工按部门分类。
- **分区 (`partitioningBy`)**：按条件将数据分为两组（比如及格与不及格）。
- **统计 (`summarizingInt`)**：获取总数、平均值、最大值、最小值和总和。

```java
// 按部门对员工进行分组
Map<String, List<Employee>> employeesByDept = employees.stream()
    .collect(Collectors.groupingBy(Employee::getDepartment));
```

这种复杂的数据重组，直接在 `List` 上是无法实现的。

###### 轻松实现并行计算（Parallelism）

如果你有一个包含百万级元素的超大集合，想要利用多核 CPU 加速处理：

用传统的 `for` 循环或 `List` 方法，需要自己手写线程池、分片逻辑、处理线程安全问题，非常痛苦。只需把 `.stream()` 改成 `.parallelStream()`，底层就会自动帮你切分任务、多线程并行处理：

```java
long count = largeList.parallelStream()
    .filter(n -> n > 1000)
    .count();
```

###### 惰性求值与短路优化（Short-circuiting）

Stream 的中间操作具有“惰性”（Lazy），在遇到终结操作前不会实际执行。同时，它支持**短路操作**（如 `findFirst`、`anyMatch`、`limit`）。 例如，在一个有 100 万个元素的列表中查找第一个满足条件的大写字母：

```java
String first = list.stream()
    .filter(s -> s.startsWith("A"))
    .findFirst()
    .orElse(null);
```

Stream 一旦找到第一个匹配的元素就会**立即停止遍历**，后面的 99 万个元素不会被处理，效率极高。而直接对 `List` 操作很难优雅地实现这种短路控制。

摘自：https://gemini.google.com/app/c72f5aaaaea77c1a
