# Java SE 备忘录



在macOS系统下，Java的安装目录在`/Library/java/JavaVirtualMachines`，而且是所有的版本的Java



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

