# Spring学习笔记

> 尚硅谷SSM教程

## <font color=FF0000>**From 1.8**</font>

**Spring简介**

Spring是一个<mark>**容器**</mark>（可以管理所有的<mark>**组件（类）**</mark>）框架

**Spring的优良特性：**

- 非侵入式：基于Spring开发的应用中的对象<mark>可以不依赖Spring的API</mark>
- 依赖注入（DI）/ IOC（Inversion of Control 控制反转）
- AOP（Aspect Oriented Programming 面向切面编程）
- 容器：Spring是一个容器
- 组件化
- 一站式



## <font color=FF0000>**From 1.9**</font>

**简介**

- **Test**：Spring的单元测试模块
  - spring-test-x.x.x.RELEASE
- **Core Container**：核心容器（IOC），黑色部分代表这部分的功能由哪些jar包组成，要使用这部分的完整功能，这些jar包都需要导入，分别是
  - spring-beans-x.x.x.RELEASE
  - spring-core-x.x.x.RELEASE
  - spring-context-x.x.x.RELEASE
  - spring-expression-x.x.x.RELEASE
- **AOP+Aspect** 面向切面编程模块
  - spring-aop-x.x.x.RELEASE
  - spring-aspect-x.x.x.RELEASE
- **Data Access/Integration** 数据访问（数据库访问） / 数据集成
  - <mark>spring-jdbc-x.x.x.RELEASE</mark> <mark style=background-color:aqua>（属于数据访问）</mark>
  - <mark>spring-orm-x.x.x.RELEASE</mark> <mark style=background-color:aqua>（属于数据访问）</mark>
  - spring-oxm-x.x.x.RELEASE（与orm类似，这里的x表示xml）<mark style=background-color:fuchsia>（属于数据集成）</mark>
  - spring-jms-x.x.x.RELEASE （Java消息服务）<mark style=background-color:fuchsia>（属于数据集成）</mark>
  - <mark>spring-tx-x.x.x.RELEASE</mark> （事务 Transaction）<mark style=background-color:aqua>（属于数据访问）</mark>
- **web** Spring开发web应用模块
  - spring-websocket-x.x.x.RELEASE
  - spring-web-x.x.x.RELEASE （与原生web（servlet）相关）
  - spring-webmvc-x.x.x.RELEASE （对应web）
  - spring-webmvc-portlet-x.x.x.RELEASE （开发web应用的组件集成）

**总结：**用哪个模块导入哪个包



## <font color=FF0000>**From 1.12**</font>

**IOC**

IOC：（Inversion Of Control 控制反转），

控制：<mark>获取资源的方式</mark>

- 主动式：要什么资源都自己创建（new）即可
- 被动式：资源的获取不是我们自己创建，而是交给一个容器来创建和设置

容器：管理所有的组件（有功能的类），容器可以<mark>自动地探查出哪些组件（类）需要用到另一个组件（类），然后容器帮我创建</mark>

**总结**：<mark>控制反转的**反转**</mark>即为：资源的获取由主动的获取（new）变成资源被动的获得（通过容器）

**DI**（Dependency Injection 依赖注入）：IOC是一种思想，而<mark>DI对于IOC思想的实现</mark>

容器能知道哪个组件（类）运行的时候，需要另外一个组件（类）；<mark>容器通过**反射**的形式，讲容器中准备好的对象**注入（利用反射机制给属性赋值）**到需要的组件中</mark>



## <font color=FF0000>**From 1.14**</font>

写配置：spring的配置文件中集合了spring的IOC容器管理的所有组件

创建一个Spring Bean Configuration File（Spring的bean配置文件）

示例如下：ioc.xml

```xml
<!--注册一个Person对象，Spring会自动创建这个Person对象-->
<!--
一个Bean标签可以注册一个组件（类，对象）
class：写要注册的组件的全类名
id：这个对象的唯一标识
-->
<bean id="person01" class="com.atguigu.bean.Person">
	<!--使用property标签为Person对象的属性赋值
        name：指定属性名
        value：为这个属性赋值
    -->
    <property name="lastName" value="张三"></property>
    <property name="age" value="18"></property>
    <property name="email" value="zhangsan@atguigu.com"></property>
    <property name="gender" value="男"></property>
</bean>
```



## <font color=FF0000>**From 1.15**</font>

**在测试类中测试IOC**

```java
public class IOCTest{
    /**
     *从容器中拿到这个组件
     */
    @Test
    public void test(){
        //ApplicationContext：代表IOC容器
        //当前应用的xml配置文件在ClassPath下
        ApplicationContext ioc = new ClassPathXmlApplicationContext("ioc.xml");
        //容器帮我们创建好对象了
        Person bean = (Person)ioc.getBean("person01"); //根据bean的id获取bean的实例
        System.out.println(bean);
    }
}
```



## <font color=FF0000>**From 1.16**</font>

**扩展**

- src，源码包开始的路径，称之为类路径的开始；同时，在编译之后，所有源码包里的内容都会被合并放在类路径下
  - 对于一般Java应用：包中的内容在/bin/文件下
  - 对于web应用，包中的内容在/WEB-INF/classes/文件下

**细节**

- 对于1.15的`ClassPathXmlApplicationContext()`，这是ioc容器配置文件在<mark>类路径下所使用的方法</mark>。同时还有另一个方法：`FileSystemXmlApplicationContext()`，这是ioc容器配置文件在<mark>磁盘路径下</mark>（内填绝对地址）
- 给容器中注册一个组件，我们也从容器中按照id拿到了这个组件的对象；组件的创建工作是容器完成的。
  - <mark>容器中对象的创建是在容器创建完成的时候（容器一启动）就已经创建好了</mark>（不用等getBean()方法调用，就创建好了）
  - 同一个组件在ioc容器中是单实例的（只创建一次）
  - 容器中如果没有该组件，如果获取组件将会报异常
  - ioc组件在创建这个组件对象的时候，`<property>`会利用<mark>**setter**</mark>方法为JavaBean的属性进行赋值
  - <mark>JavaBean的属性名（\<property name=?>）是由getter/setter方法的名称决定的（即：将getter/setter的get/set去掉剩下的小写字母单词就是属性名）</mark>



## <font color=FF0000>**From 2.1**</font>

**根据bean的类型从IOC容器中获取bean的实例**

```java
Person bean = ioc.getBean(Person.class);
```

上面的代码存在问题：由于是根据bean的类型获取，而配置文件中可能会出现多个同一类型的组件；这时候将会报错。这时候可以<mark>同时传入类型和bean的id</mark>从而获取bean的实例，如下：

```java
Person bean = ioc.getBean("person02", Person.class);
```



## <font color=FF0000>**From 2.2**</font>

**通过有参构造器的方法从IOC容器中获取bean的实例**

在ioc.xml中要使用\<construction-arg>，示例如下：

```xml
<bean id="person03" class="com.atguigu.bean.Person">
    <construction-arg name="lastName" value="小明"></construction-arg>
    <construction-arg name="email" value="xiaoming@atguigu.com"></construction-arg>
    <construction-arg name="gender" value="男"></construction-arg>
    <construction-arg name="age" value="18"></construction-arg>
</bean>
```

这时候还是按照根据bean的id获取bean的实例，如下：

```java
Person bean = ioc.getBean("person03");
```



## <font color=FF0000>**From 2.3**</font>

**另一种通过有参构造器的方法从IOC容器中获取bean的实例**

若<mark>严格按照组件属性声明的**顺序**进行配置</mark>，可以在`<construction-arg ...>`中不配置name，只配置value

同时，如果不按照顺序也可以，但需要为参数指定索引，从0开始：

```xml
<bean id="person04" class="com.atguigu.bean.Person">
    <construction-arg value="小明"></construction-arg>
    <construction-arg value="xiaoming@atguigu.com"></construction-arg>
    <construction-arg value="18" index="3"></construction-arg> <!--如2.2中的代码，第三行的本来在第四行，所以这里index=3-->
    <construction-arg value="男" index="2"></construction-arg>
</bean>
```

同时还有另一种情况：<mark>**存在有参构造器重载的情况**</mark>，这时候就可以再在\<construction-arg>中添加上type属性（组件属性的类型），从而进行区分。

```xml
<bean id="person05" class="com.atguigu.bean.Person">
    <construction-arg value="小丽"></construction-arg>
    <construction-arg value="10" index="1" type="java.lang.Integer"></construction-arg> 
    <construction-arg value="男"></construction-arg>
</bean>
```

</font>



## <font color=FF0000>**From 2.4**</font>

**通过名称空间从IOC容器中获取bean的实例**

在xml文件中，<mark>名称空间是用来防止标签重复的</mark>，比如：

```xml
<book>
    <b:name>西游记</b:name>
    <price>19.98</price>
    <author>
    	<a:name>吴承恩</a:name>
        <gender>男</gender>
    </author>
</book>
```

在这里出现了两个属性name，通过a和b来区分，这里a和b就是名称空间。另外还有带前缀的标签\<c:forEach>、\<jsp:forward>

在xml文件开头有`<beans xmlns="...">`其中`...`中的内容是xml默认的名称空间，另外还有`xmlns:xsi="..."`其中`xsi`就是这个名称空间的名称。

在这里我们要用的是p名称空间，步骤分两步：

- 导入名称空间：在开头的<beans ...>内加上`xmlns:p="http://www.springframework.org/schema/p"`

- 配置xml文件，写带前缀的标签/属性

  ```xml
  <bean id="person06" class="com.atguigu.bean.Person"
        p:age="18" p:email="xiaoming@atguigu.com" p:lastName="哈哈" p:gender="男">
  </bean>
  ```




## <font color=FF0000>**From 2.2**</font>

**从IOC容器中获取bean的实例**

