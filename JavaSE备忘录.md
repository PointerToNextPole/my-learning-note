# Java SE 备忘录



在macOS系统下，Java的安装目录在`/Library/java/JavaVirtualMachines`，而且是所有的版本的Java



#### JDK & JRE

- **JRE**（Java Runtime Environment java运行时环境）包含了java虚拟机，java基础类库。是使用java语言编写的程序运行所需要的软件环境，<font color=FF0000>是提供给想运行java程序的**用户**使用的</font>。

- **JDK**（Java Development Kit java开发工具包）是程序员使用java语言编写java程序所需的开发工具包，<font color=FF0000>是提供给程序员使用的</font>。JDK包含了JRE，同时还包含了编译java源码的编译器javac，还包含了很多java程序调试和分析的工具：jconsole，jvisualvm等工具软件，还包含了java程序编写所需的文档和demo例子程序。

- **总结：**如果你需要运行java程序，只需安装JRE就可以了。如果你需要编写java程序，需要安装JDK。

  JRE根据不同操作系统（如：windows，linux等）和不同JRE提供商（IBM,ORACLE等）有很多版本。

摘自：[JRE 和 JDK 的区别是什么？ - 王博的回答 - 知乎](https://www.zhihu.com/question/20317448/answer/14737358)

补充：

<img src="https://i.loli.net/2020/08/16/T3Ldk5zv4Pl7JAR.png" style="zoom:50%;" />

另外，最新的jdk已经不包含jre了，判断依据：是否有javac命令



#### Java除法

整形除以整形，结果等于<font color=FF0000>**整形**</font>



#### Java泛型通配符

- ？ 表示不确定的 java 类型
- T (type) 表示具体的一个java类型
- K V (key value) 分别代表java键值中的Key Value
- E (element) 代表Element

#### Java泛型方法

定义泛型方法时，必须<font color=FF0000>在返回值前</font>加一个`<T>`，来声明这是一个泛型方法，持有一个泛型T，然后才可以用泛型T作为方法的返回值。示例如下：

```java
public <T> T methodName(T paramName){
  ...
}
```



#### Java iterator



#### Java synchronized



#### Java环境变量PATH和CLASSPATH

- PATH是用来配置JDK，`%JAVA_HOME%/bin/`的配置
- CLASSPATH的作用是<mark>指定Java类所在的目录</mark>



#### Java 方法引用（Method references）` ::`

形如 `ClassName::methodName` 或者 `objectName::methodName` 的表达式，我们把它叫做方法引用（Method Reference）。它的原型来自：eta conversion (η-conversion)

**方法引用有四种，分别是：**

- 指向静态方法的引用
- 指向某个对象的实例方法的引用
- 指向某个类型的实例方法的引用
- 指向构造方法的引用

摘自：[Java 8 方法引用](http://liwenkun.me/2017/03/23/java-8-method-references/)



#### Java流式编程

