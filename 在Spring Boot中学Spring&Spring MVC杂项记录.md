# 在Spring Boot中学到的Spring与Spring MVC杂项记录



#### @Deprecated

标注在某个函数上方表示该代码将要过时（比如在下个版本将会被删掉），如果调用该方法将会出现<mark>中划线和警告</mark>，这时如果一定要用且不希望其产生警告，需要在使用时在上`@SuppressWarnings("deprecation")`表示希望忽略`deprecated`这个警告，当然前提是必须有一个被注解`@Deprecated`的函数



#### Java注解的分类

按照运行机制划分

- 源码注解：注解只在源码中存在，编译成.class文件就不存在了
- 编译时注解：注解在源码和.class文件中都存在（包含@Override，@Deprecated，@SuppressWarnings）
- 运行时注解：在运行阶段还会起作用，甚至会影响运行逻辑的注解

按照来源划分

- 来自JDK的注解
- 来自第三方的注解
- 我们自定义的注解（要使用元注解（注解的注解））

关于元注解可以参考下面的讲解，以及博文：[**JAVA核心知识点--元注解详解**](https://blog.csdn.net/pengjunlee/article/details/79683621)



#### 自定义注解的语法要求

使用 `@interface` 关键字以定义注解，示例如下：

```java
/**
 *上面的四个注解为元注解，是在JDK1.5中提供的4个标准的用来对注解类型进行注解的注解类
 */
//@Target表示注解的作用域，包含CONSTRUCTOR(构造方法声明)、FIELD(字段声明)、LOCAL_VARIABLE(局部变量声明)、METHOD(方法声明)、PACKAGE(包声明)、PARAMETER(参数声明)、TYPE(类，接口)
@Target({ElementType.METHOD, ElementType.TYPE})
//@Retention是生命周期，分为SOURCE、CLASS、RUNTIME，与上面提到的“按照运行机制划分”一致
@Retention(RetentionPolicy.RUNTIME)
//@Inherited表示该注解是可以被自注解继承的
@Inherited
//@Documented生成javadoc是会包含注解
@Documented
public @interface Description{  //@interface表示当前定义的时注解
    /**
     *下面的看上去像是函数，其实表示一个成员变量，这里的成员变量必须以无参无异常的形式声明
     *同时，成员的类型是受限的，合法的类型包括原始类型、String、Class、Annotation、Enumeration
     *如果注解只有一个成员，则成员名必须取名为value(),在使用时可以忽略成员名和赋值号(=)，即不用键值对的形式
     *注解类可以没有成员，没有成员的注解称为标识注解
     */
    String desc();
    String author();
    int age() default 18;  //可以用default为成员指定一个默认值
}
```



#### 使用自定义注解

使用自定义注解语法：

```java
@<注解名>(<成员名1>=<成员值1>, <成员名2>=<成员值2>, ...)
    
@Description(desc="I am eyeColor", author="Mooc boy", age=18)
public String eyeColor(){
    return "red";
}
```



#### 解析注解

**概念：**通过反射获取类，函数或成员上的<font color=FF0000>运行时</font>注解信息，从而实现动态控制程序运行的逻辑



#### Spring MVC三层架构注解详解@Controller、@Service和@Repository

<mark>Spring MVC采用经典的三层分层控制结构</mark>，在<mark style=background-color:red>持久层</mark>，<mark style=background-color:lime>业务层</mark>和<mark style=background-color:aqua>控制层</mark>分别采用<mark style=background-color:red>@Repository</mark>、<mark style=background-color:lime>@Service</mark>、<mark style=background-color:aqua>@Controller</mark>对分层中的类进行注解，而@Component对那些比较中立的类进行注解

以上摘取自：[**Spring MVC三层架构注解详解@Controller、@Service和@Repository**](https://blog.csdn.net/qq_41357573/article/details/84454502)，<font color=FF0000>**下面还有详细讲解，由于事件原因未仔细钻研**</font>。

#### <font color=FF0000>补充（相当重要）：</font>

| **Spring注解** |   **场景说明**    |
| :------------: | :---------------: |
|  @Repository   | 数据仓库模式注解  |
|   @Component   | 通用组件模式注解  |
|    @Service    |   服务模式注解    |
|  @Controller   | Web控制器模式注解 |
| @Configuration |  配置类模式注解   |

摘自：[SpringFramework5.0 @Indexed注解 简单解析](https://www.jianshu.com/p/f61f2e020a2f)



#### 组件（@Component）

##### 什么是组件

个人的理解，<mark>组件是为了实现某个功能而整合在一起的方法及数据的集合</mark>，为了描述组件的特征组件中还包含一些描述信息，诸如组件的名称或ID，提供哪些接口，版本信息等。<mark>通常组件是以二进制文件提供的</mark>，但也可以以源代码的形式提供，只是这种情况不多见。

##### 组件和类的关系

<mark>组件可以理解为类的超集，它可能包含若干个类，当然也可以只有一个类；此外组件往往需要提供一些额外的描述信息，供组件管理器管理，而类缺乏这些信息。类加上这些必要的信息，基本上就差不多等同于组件了。</mark>不过，通常组件是以二进制形式发布，而<mark>类是源代码层面的东西</mark>。
以上摘取自：[**组件、接口、类、对象之间的关系**](http://www.cppblog.com/cforce/archive/2012/07/06/181972.aspx)



#### @Autowired

@Autowired 注释，<mark>它可以对<font color=FF0000>**类成员变量、方法及构造函数**</font>进行标注，完成<font color=FF0000>**自动装配**</font>的工作</mark>。 <font color=FF0000>**通过 @Autowired的使用来消除 set ，get方法**</font>。<mark>在使用@Autowired之前，我们对一个**bean**配置起属性时，是这时用的</mark>

```xml
<property name="属性名" value="属性值"/>
```

通过这种方式来，配置比较繁琐，而且代码比较多。在Spring 2.5 引入了 @Autowired 注释

以上摘取自：[@Autowired用法详解](https://www.cnblogs.com/fnlingnzb-learner/p/9723834.html)

**补充**：@Autowired，@Resource，@Inject都可以用来作为<mark>自动装配</mark>的注解，区别如下：

- **@Autowired：**最强大，<mark>Spring自己的注解</mark>
- **@Resource：**J2EE的注解，<mark>Java的标准，所以扩展性强</mark>
- **@Inject：**EJB的注解，几乎不用

关于的区别详见[**知乎 - @Autowired和@Resource的区别是什么？**](https://www.zhihu.com/question/39356740)



#### @RunWith

**@RunWith的<font color=FF0000>作用</font>就是一个运行器**，它是JUnit的注解，提供一个测试运行器来指导JUnit如何运行。

例如：

```java
@RunWith(JUnit4.class)  //就是指用JUnit4来运行
```

#### @Bean

@Bean是一个<mark>方法级别上的注解</mark>，<mark>主要用在@Configuration注解的类里，也可以用在@Component注解的类里</mark>。添加的bean的id为方法名



#### @Qualifier

@Qualifier作用为限定描述符，用于细粒度选择候选者，也就是注入的时候可能发现有多个可注入对象。

比如说一个Service接口有两个实现类，分别为impl1，impl2，impl3，你注入Service的时候注入的是接口，那么就可以通过@Qualifier(“你要注入的bean的名称”)来选择注入对象<mark>（在对象上加上@Qualifier以选择）</mark>



#### @Configuration

@Configuration用于定义<font color=FF0000>**配置类**</font>，**<mark style=background-color:fuchsia>可用于替换xml配置文件</mark>**，被注解的类内部包含有一个或多个被@Bean注解的方法。这些方法将会被**AnnotationConfigApplicationContext**或**AnnotationConfigWebApplicationContext**类进行扫描，并用于构建bean定义，初始化Spring容器。
 @Configuration相当于`<beans></beans>`
@Bean相当于`<bean></bean>`

摘自：[【Spring】配置类](https://www.jianshu.com/p/ea4cd7991312)

 标注在类上，该类会被CGLIB动态代理生成子类，可以达到这样的效果：在某@Bean方法下调用另一个标注了@Bean的方法，得到的会是同一个Bean对象

摘自：[Spring注解 @Configuration](https://www.cnblogs.com/lvbinbin2yujie/p/10279416.html)

##### @Configuration注解的配置类有如下要求

- @Configuration不可以是final类型；
- @Configuration不可以是匿名类；
- 嵌套的configuration必须是静态类。

摘自：[@Configuration的使用和作用](https://blog.csdn.net/BinshaoNo_1/article/details/85005935)

##### 与@Component区别

`@Component`注解也会当做配置类，但是并<mark>不会为其生成CGLIB代理Class</mark>，所以<mark>执行了两次new操作</mark>，所以<mark>是不同的对象</mark>。

`@Configuration`注解时，生成当前对象的子类Class，并对方法拦截，<mark>**第二次new时直接从BeanFactory之中获取对象，所以得到的是同一个对象。**</mark>

摘自：[配置类一@Configuration](https://www.cnblogs.com/loveer/p/11312197.html)



#### @Value

`@Value`用于不通过配置文件的注入属性

##### 通过@Value将外部的值动态注入到Bean中，使用的情况有：

- 注入普通字符串

  ```java
  @Value("normal")
  private String normal; // 注入普通字符串
  ```

- 注入操作系统属性

  ```java
  @Value("#{systemProperties['os.name']}")
  private String systemPropertiesName; // 注入操作系统属性
  ```

- 注入表达式结果

  ```java
  @Value("#{ T(java.lang.Math).random() * 100.0 }")
  private double randomNumber; //注入表达式结果
  ```

- 注入其他Bean属性（比如：注入beanInject对象的属性another）

  ```java
  @Value("#{beanInject.another}")
  private String fromAnotherBean; // 注入其他Bean属性：注入beanInject对象的属性another
  ```

- 注入文件资源

  ```java
  @Value("classpath:com/hry/spring/configinject/config.txt")
  private Resource resourceFile; // 注入文件资源
  ```

- 注入URL资源

  ```java
  @Value("http://www.baidu.com")
  private Resource testUrl; // 注入URL资源
  ```

##### 通过配置文件的注入属性的情况

通过@Value将外部配置文件的值动态注入到Bean中。配置文件主要有两类：

- application.properties。application.properties在spring boot启动时默认加载此文件
- 自定义属性文件。自定义属性文件通过@PropertySource加载。@PropertySource可以同时加载多个文件，也可以加载单个文件。如果相同第一个属性文件和第二属性文件存在相同key，则最后一个属性文件里的key启作用。加载文件的路径也可以配置变量，如${anotherfile.configinject}

摘自：[Spring Boot系列四 Spring @Value 属性注入使用总结一](https://blog.csdn.net/hry2015/article/details/72353994)



#### @Indexed

SpringFramework5.0引入了一个注解`@Indexed` ，它可以为Spring的**模式注解**添加索引，以提升应用启动性能。

**使用场景：**在应用中有大量使用`@ComponentScan`扫描的package包含的类越多的时候，Spring模式注解解析耗时就越长。

**使用方法：**在项目中使用的时候需要导入一个`spring-context-indexer` jar包，有Maven和Gradle 两种导入方式，具体可以看官网，我这里使用maven方式，引入jar配置如下：

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context-indexer</artifactId>
        <version>5.1.12.RELEASE</version>
        <optional>true</optional>
    </dependency>
</dependencies>
```

然后在代码中，对于使用了模式注解的类上加上`@Indexed`注解即可。如下：

```java
@Indexed
@Controller
public class HelloController {}
```

**原理说明：**在项目中使用了`@Indexed`之后，编译打包的时候会在项目中<mark>自动生成`META-INT/spring.components`文件</mark>。当Spring应用上下文执行`ComponentScan`扫描时，`META-INT/spring.components`将会被`CandidateComponentsIndexLoader` 读取并加载，转换为`CandidateComponentsIndex`对象，这样的话`@ComponentScan`不再扫描指定的package，而是读取`CandidateComponentsIndex`对象，从而达到提升性能的目的。

源码地址：`org.springframework.context.index.CandidateComponentsIndexLoader`

摘自：[SpringFramework5.0 @Indexed注解 简单解析](https://www.jianshu.com/p/f61f2e020a2f)

官方文档：[1.10.9. Generating an Index of Candidate Components](https://docs.spring.io/spring/docs/5.1.12.RELEASE/spring-framework-reference/core.html#beans-scanning-index)



#### @Param

@Param是<font color=FF0000>**MyBatis**</font>所<font color=FF0000>**提供**</font>的(org.apache.ibatis.annotations.Param)，作为<font color=FF0000>**Dao层的注解**</font>，<mark>作用是用于传递参数，从而可以与SQL中的的字段名相对应</mark>，一般在2<=参数数<=5时使用最佳。

摘自：[为什么要用@Param](https://blog.csdn.net/sinat_33010325/article/details/84261662)



#### Properties文件

- properties文件是一个文本文件
- properties文件的语法有两种，一种是注释，一种是配置属性
  - <mark>注释在前面加上#号</mark>
  - properties文件的一个属性配置信息值可以替换，但值不能换行，如要换行要用<mark> **\'\\' **</mark>表示
  - properties的属性配置键值前后的空格在解析时候会被忽略。
  - properties文件可以只有键而没有值。也可以仅有键和等号而没有值，但无论如何一个属性配置不能没有键。

以上摘取自：[PROPERTIES文件的编写与解析](https://blog.csdn.net/simba1949/article/details/79018336)</font>



## Spring AOP



#### Spring AOP的几个概念

- **切面(Aspect)：**切面就是一个关注点的模块化，如事务管理、日志管理、权限管理等； 
- **连接点(Joinpoint)：**程序执行时的某个特定的点，在Spring中就是一个方法的执行； 
- <font color=FF0000>**通知(Advice)**</font>：通知<mark>就是在切面的某个连接点上执行的<font color=FF0000>**操作**</font></mark>，也就是事务管理、日志管理等； 
- <font color=FF0000>**切入点(Pointcut)**</font>：切入点就是描述<mark>某一类**选定**的连接点</mark>，也就是<mark style=color:red>**指定某一类要植入通知的方法**</mark>； <mark>切入点属于连接点</mark>
- **目标对象(Target)：**就是<mark>被AOP动态代理的目标对象</mark> 
- **织入(Weaving)：**织入是指将切面代码插入到目标对象的过程。代理的invoke方法完成的工作，可以称为织入。
- **顾问(Advisor)：**顾问是切面的另一种实现，能够将通知以更为复杂的方式织入到目标对象中，是将通知包装为更复杂切面的装配器。

#### 常用增强处理类型

- Before
- AfterReturning
- AfterThrowing
- After
- Around

摘自：[springAOP前置增强、后置增强、环绕增强（编程式）](https://blog.csdn.net/tangpeng2018/article/details/79832270)

- **正常的执行顺序是：**@Around -> @Before -> 主方法体 -> @Around中ProceedingJoinPoint的proceed()方法 -> @After -> @AfterReturning
- 如果异常在Around中ProceedingJoinPoint的proceed()方法之前，则执行顺序为：@Around -> @After -> @AfterThrowing
- 如果异常在Around中ProceedingJoinPoint的proceed()方法 之后，则执行顺序为@Around ->@Before->主方法体->@Around中pjp.proceed()->@After->@AfterThrowing

摘自：[Spring AOP详解一文搞懂@Aspect、@Pointcut、@Before、@Around、@After、@AfterReturning、@AfterThrowing](https://blog.csdn.net/u011047968/article/details/104402079)



#### 实现AOP的切面主要有以下几个要素

- <font color=FF0000>**使用**</font>`@Aspect`<font color=FF0000>**注解将一个java类定义为切面类**</font>

- <font color=FF0000>**使用**</font>`@Pointcut`<font color=FF0000>**定义一个切入点**</font>，可以是一个规则表达式，比如下例中某个package下的所有函数，也可以是一个注解等。

- 根据需要在切入点不同位置的切入内容
  - 使用`@Before`在<mark>切入点开始处</mark>切入内容
  - 使用`@After`在<mark>切入点结尾处</mark>切入内容
  - 使用`@AfterReturning`在<mark>切入点return内容之后</mark>切入内容（可以用来对处理返回值做一些加工处理）
  - 使用`@Around`在<mark>切入点<font color=FF0000>**前后**</font></mark>切入内容，并自己控制何时执行切入点自身的内容
  - 使用`@AfterThrowing`用来<mark>处理当切入内容部分抛出异常之后</mark>的处理逻辑

  示例：

  ```java
  try{  
       try{  
           doBefore();//对应@Before注解的方法切面逻辑  
           method.invoke();  
       }finally{  
           doAfter();//对应@After注解的方法切面逻辑  
       }  
       doAfterReturning();//对应@AfterReturning注解的方法切面逻辑  
   }catch(Exception e){  
        doAfterThrowing();//对应@AfterThrowing注解的方法切面逻辑  
   }  
  ```

  摘自：[理解AOP@Before,@After,@AfterReturning,@AfterThrowing执行顺序](https://www.iteye.com/blog/jaychang-2350743)

#### 补充

#### @Around

##### @Around的作用

- 既可以<mark>在目标方法<font color=FF0000>之前</font>织入</mark>增强动作，也可以<mark>在执行目标方法<font color=FF0000>之后</font>织入</mark>增强动作；
- <mark>可以决定目标方法在<font color=FF0000>什么时候执行</font>，<font color=FF0000>如何执行</font>，甚至<font color=FF0000>可以完全阻止目标目标方法的执行</font></mark>；
- 可以<font color=FF0000>改变执行目标方法的<font size=4>**参数值**</font></font>，也可以<font color=FF0000>改变执行目标方法之后的<font size=4>**返回值**</font></font>； 当需要改变目标方法的返回值时，只能使用Around方法；

虽然Around功能强大，但通常需要在线程安全的环境下使用。<mark>因此，如果使用普通的Before、AfterReturing增强方法就可以解决的事情，就<font color=FF0000>没有必要使用Around增强处理了</font></mark>。

摘自：[@Around简单使用示例——SpringAOP增强处理](https://blog.csdn.net/qq_41981107/article/details/85260765)

#### @Pointcut

##### 表达式标签

- execution()：用于<mark>匹配<font color=FF0000>**方法**</font></mark>执行的连接点

  ```java
  /**
  *使用excution表达式
  * execution(
  *  modifier-pattern?           //用于匹配public、private等访问修饰符
  *  ret-type-pattern            //用于匹配返回值类型，不可省略
  *  declaring-type-pattern?     //用于匹配包类型
  *  name-pattern(param-pattern) //用于匹配类中的方法，不可省略
  *  throws-pattern?             //用于匹配抛出异常的方法
  * )
  */
  //下面的表达式解释为：匹配com.leo.controller.HelloController类中以hello开头的修饰符为public返回类型任意的方法
  @Pointcut(value = "execution(public * com.leo.controller.HelloController.hello*(..))")
  public void pointCut() {}
  ```

- args()：用于<mark>匹配<font color=FF0000>当前执行的方法传入的**参数为指定类型**的执行方法</font></mark>

  ```java
  //args匹配第一个入参是String类型的方法
  @Pointcut("args(String, ..)")
  public void pointcutArgs(){}
  ```

- this()：用于<mark>匹配当前AOP代理对象类型的执行方法</mark>；注意是AOP代理对象的类型匹配，这样就可能包括引入接口也类型匹配；

  ```java
  //this匹配目标指定的方法，此处就是HelloController的方法
  @Pointcut("this(com.leo.controller.HelloController)")
  public void pointcutThis(){}
  ```

- target()：用于<mark>匹配当前<font color=FF0000>**目标对象类型**</font>的执行方法</mark>；注意<font color=FF0000>**是目标对象的类型匹配**</font>，这样就不包括引入接口也类型匹配；

  ```java
  //target匹配实现UserInfoService接口的目标对象
  @Pointcut("target(com.leo.service.UserInfoService)")
  public void pointcutTarge(){}
  ```

- within()：用于<mark>匹配<font color=FF0000>指定类型**内**的方法</font>执行</mark>；

  ```java
  //匹配com.leo.controller包下所有的类的方法
  @Pointcut("within(com.leo.controller..*)")
  public void pointcutWithin(){}
  ```

- bean()：使用“bean(Bean id或名字通配符)”匹配特定名称的Bean对象的执行方法；Spring ASP扩展的，在AspectJ中无相应概念；

  ```java
  //bean匹配所有以Service结尾的bean里面的方法，注意：使用自动注入的时候默认实现类首字母小写为bean的id
  @Pointcut("bean(*ServiceImpl)")
  public void pointcutBean(){}
  ```

- @args()：于匹配当前<mark>执行的方法传入的参数<font color=FF0000>**持有指定注解**</font></mark>的执行；

  ```java
  //@args匹配参数中标注为@Sevice的注解的方法
  @Pointcut("@args(org.springframework.stereotype.Service)")
  public void pointcutArgsAnno(){}
  ```

- @target()：用于匹配当前目标对象类型的执行方法，其中目标对象持有指定的注解；

  ```java
  //@target匹配的是@Controller的类下面的方法，要求注解的@Controller级别为@Retention(RetentionPolicy.RUNTIME)
  @Pointcut("@target(org.springframework.stereotype.Controller)")
  public void pointcutTargetAnno(){}
  ```

- @within()：用于匹配所以持有指定注解类型内的方法；

  ```java
  //@within匹配@Controller注解下的方法，要求注解的@Controller级别为@Retention(RetentionPolicy.CLASS)
  @Pointcut("@within(org.springframework.stereotype.Controller)")
  public void pointcutWithinAnno(){}
  ```

- @annotation：用于匹配当前执行方法持有指定注解的方法

  ```java
  //@annotation匹配是@Controller类型的方法
  @Pointcut("@annotation(org.springframework.stereotype.Controller)")
  public void pointcutAnnocation(){}
  ```

- reference pointcut：表示引用其他命名切入点

  ```java
  @Pointcut(value="bean(*Service)")
  private void pointcut1(){}
  @Pointcut(value="@args(cn.javass.spring.chaper6.Source)")
  public referencePointcutTest(JoinPoint jp){}
  ```

摘自：[Spring AOP详解一文搞懂@Aspect、@Pointcut、@Before、@Around、@After、@AfterReturning、@AfterThrowing](https://blog.csdn.net/u011047968/article/details/104402079)，以及[@Pointcut语法详解](https://blog.csdn.net/qq_26860451/article/details/100554377)<font color=FF0000>**（这篇文章还有很多内容，推荐阅读！！！）**</font>



#### @EnableAspectJAutoProxy

`@EnableAspectJAutoProxy`表示开启AOP代理自动配置

另外具体用法参见：[Spring 使用 @EnableAspectJAutoProxy 注解启用 AOP](https://www.letianbiji.com/spring/spring-@enableaspectjautoproxy.html)



#### @ConfigurationProperties

在编写项目代码时，我们要求更灵活的配置，更好的模块化整合。在 Spring Boot 项目中，为满足以上要求，我们将大量的参数配置在 application.properties 或 application.yml 文件中，通过 `@ConfigurationProperties` 注解，我们可以方便的获取这些参数值

我们可以使用 `@Value` 注解或着使用 Spring `Environment` bean 访问这些属性，但是这种注入配置方式有时显得很笨重。我们将使用更安全的方式(`@ConfigurationProperties` )来获取这些属性

示例如下：

```xml
myapp.mail.enable = true
myapp.mail.default-subject=This is a Test
```

```java
@Data
@ConfigurationPropeeries(prefix = "myapp.mail")
public class MailModuleProperties {
    private Boolean enable = Boolean.True;
    private String defaultSubject;
}
```

`@ConfigurationProperties` 的基本用法非常简单：我们为每个要捕获的外部属性提供一个带有字段的类。请注意以下几点:

- <mark>前缀（prefix）</mark>定义了哪些外部属性将绑定到类的字段上
- 根据 Spring Boot 宽松的绑定规则，类的属性名称必须与外部属性的名称匹配
- 我们可以简单地用一个值初始化一个字段来定义一个默认值
- 类本身可以是包私有的
- 类的字段必须有公共 setter 方法

#### 激活@ConfigurationProperties

##### 目的

我们需要让 Spring 知道我们的 @ConfigurationProperties 类存在，以便将其加载到应用程序上下文中。有如下方法：

- 我们可以通过添加 `@Component` 注解让 Component Scan 扫描到。<mark>只有当类所在的包被 Spring </mark>`@ComponentScan`<mark> 注解扫描到才会生效</mark>（该注解默认会扫描该类所在的包下所有的配置类，相当于之前的\<context:component-scan>），默认情况下，该注解会扫描在主应用类下的所有包结构

  ```java
  @Component
  @ConfigurationPropeeries(prefix = "myapp.mail")
  class MailModuleProperties{
      //...
  }
  ```

- 我们也可以通过 Spring 的 Java Configuration 特性实现同样的效果。只要 MailModuleConfiguration 类被 Spring Boot 应用扫描到，我们就可以在应用上下文中访问 MailModuleProperties Bean

  ```java
  @Configuration
  class PropetiesConfig{
      @Bean
      public MailModuleProperties mailModuleProperties(){
          return new MailModuleProperties();
      }
  }
  ```

- 我们还可以使用 `@EnableConfigurationProperties` 注解让我们的类被 Spring Boot 所知道，在该注解中其实是用了`@Import(EnableConfigurationPropertiesImportSelector.class)` 实现

  ```java
  @Configuration
  @EnableConfigurationProperties(MailModuleProperties.class)
  class PropertiesConfig{
      
  }
  ```

##### 激活一个 @ConfigurationProperties 类的最佳方式是什么？

所有上述方法都同样有效。然而，<mark>我建议模块化你的应用程序，并让每个模块提供自己的`@ConfigurationProperties` 类，只提供它需要的属性</mark>，就像我们在上面的代码中对邮件模块所做的那样。这使得在不影响其他模块的情况下重构一个模块中的属性变得容易。

因此，<mark>我不建议在应用程序类本身上使用 `@EnableConfigurationProperties`，</mark>如许多其他教程中所示，是在特定于模块的 `@Configuration` 类上使用`@EnableConfigurationProperties`，该类也可以利用包私有的可见性对应用程序的其余部分隐藏属性。

摘自：[@ConfigurationProperties 注解使用姿势，这一篇就够了](https://www.jianshu.com/p/7f75936b573b)

这里仅摘抄一部分，后面还有内容



#### Spring 容器

<mark>Spring容器可以理解为生产对象（Object）的地方</mark>，在这里容器不只是帮我们创建了对象那么简单，<mark>它负责了对象的整个生命周期--创建、装配、销毁</mark>。而这里<mark>对象的创建管理的控制权都交给了Spring容器，所以这是一种控制权的反转，称为IOC容器</mark>，而这里IOC容器不只是Spring才有，很多框架也都有该技术。

摘自：[理解Spring容器、BeanFactory和ApplicationContext](https://www.jianshu.com/p/2854d8984dfc)



#### BeanFactory和ApplicationContext的区别

##### BeanFactory和ApplicationContext是Spring的两大核心接口

###### BeanFactory

是Spring里面最低层的接口，提供了最简单的容器的功能，只提供了实例化对象和拿对象的功能；

<font color=FF0000>Spring Ioc容器的实现，从根源上是BeanFactory</font>，但真正可以作为一个可以独立使用的ioc容器还是DefaultListableBeanFactory，因此可以这么说，DefaultListableBeanFactory 是整个spring ioc的始祖。

<img src="https://s1.ax1x.com/2020/08/16/dV2XQA.jpg" alt="img" style="zoom:75%;" />

###### ApplicationContext

应用上下文，<mark>**ApplicationContext是BeanFactory的子接口**</mark>，它是Spring的更高级的容器，提供了更多的有用的功能：

- 国际化（MessageSource）

- 访问资源，如URL和文件（ResourceLoader）

- 载入多个（有继承关系）上下文 ，使得每一个上下文都专注于一个特定的层次，比如应用的web层  

- 消息发送、响应机制（ApplicationEventPublisher）

- AOP（拦截器）

<mark>如果说BeanFactory是Sping的心脏，那么ApplicationContext就是完整的身躯了。</mark>

<img src="https://i.loli.net/2020/08/16/aLe6bvRPEzy5Ssg.jpg" alt="ApplicationContext结构图" style="zoom: 50%;" />

​                                                                                   ApplicationContext结构图<img src="https://i.loli.net/2020/08/16/3MN6rOGaTSXoj2v.jpg" alt="ApplicationContext类结构树" style="zoom: 50%;" />

​                                                                                    ApplicationContext类结构树

|     ApplicationContext常用实现类      |                             作用                             |
| :-----------------------------------: | :----------------------------------------------------------: |
|  AnnotationConfigApplicationContext   | 从一个或多个基于java的配置类中加载上下文定义，适用于java注解的方式。 |
|    ClassPathXmlApplicationContext     | 从类路径下的一个或多个xml配置文件中加载上下文定义，适用于xml配置的方式。 |
|    FileSystemXmlApplicationContext    | 从文件系统下的一个或多个xml配置文件中加载上下文定义，也就是说系统盘符中加载xml配置文件。 |
| AnnotationConfigWebApplicationContext |            专门为web应用准备的，适用于注解方式。             |
|       XmlWebApplicationContext        | 从web应用下的一个或多个xml配置文件加载上下文定义，适用于xml配置方式。 |

##### Spring具有非常大的灵活性，它提供了三种主要的装配机制

###### 在XML中进行显示配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
     <!--若没写id，则默认为com.test.Man#0,#0为一个计数形式-->
    <bean id="man" class="com.test.Man"></bean>
</beans>
```

```java
public class Test {
    public static void main(String[] args) {
        //加载项目中的spring配置文件到容器
        ApplicationContext context = new ClassPathXmlApplicationContext("resouces/applicationContext.xml");
        //加载系统盘中的配置文件到容器
        //ApplicationContext context = new FileSystemXmlApplicationContext("E:/Spring/applicationContext.xml");
        //从容器中获取对象实例
        Man man = context.getBean(Man.class);
        man.driveCar();
    }
}
```

###### 在Java中进行显示配置

```java
//同xml一样描述bean以及bean之间的依赖关系
@Configuration
public class ManConfig {
    @Bean
    public Man man() {
        return new Man(car());
    }
    @Bean
    public Car car() {
        return new QQCar();
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        //从java注解的配置中加载配置到容器
        ApplicationContext context = new AnnotationConfigApplicationContext(ManConfig.class);
        //从容器中获取对象实例
        Man man = context.getBean(Man.class);
        man.driveCar();
    }
}
```

###### 隐式的bean发现机制和自动装配

- 组件扫描（component scanning）：Spring会自动发现应用上下文中所创建的bean。
- 自动装配（autowiring）：Spring自动满足bean之间的依赖。

```java
//这个简单的注解表明该类回作为组件类，并告知Spring要为这个创建bean。
@Component
public class GameDisc implements Disc{
    @Override
    public void play() {
        System.out.println("我是马里奥游戏光盘。");
    }
}
```

<mark>**不过，组件扫描默认是不启用的。我们还需要显示配置一下Spring，从而命令它去寻找@Component注解的类，并为其创建bean。**</mark>

```java
@Configuration
@ComponentScan
public class DiscConfig {
    //...
}
```

<mark>**我们在DiscConfig上加了一个@ComponentScan注解表示在Spring中开启了组件扫描，默认扫描与配置类相同的包，就可以扫描到这个GameDisc的Bean了。这就是Spring的自动装配机制。**</mark>

<mark>（使用的优先性: **3 > 2 > 1**）尽可能地使用自动配置的机制，显示配置越少越好</mark>。当必须使用显示配置bean的时候（如：有些源码不是由你来维护的，而当你需要为这些代码配置bean的时候），推荐使用类型安全比XML更加强大的JavaConfig。最后只有当你想要使用便利的XML命名空间，并且在JavaConfig中没有同样的实现时，才使用XML。

##### 除了提供BeanFactory所支持的所有功能外ApplicationContext还有额外的功能

- 默认初始化所有的Singleton，也可以通过配置取消预初始化。
- 继承MessageSource，因此支持国际化。
- 资源访问，比如访问URL和文件。
- 事件机制。
- 同时加载多个配置文件。
- 以声明式方式启动并创建Spring容器。

摘自：[理解Spring容器、BeanFactory和ApplicationContext](https://www.jianshu.com/p/2854d8984dfc)

##### 两者装载bean的区别

- BeanFactory：BeanFactory在启动的时候不会去实例化Bean，只有从容器中拿Bean的时候才会去实例化

- **ApplicationContext：**ApplicationContext在启动的时候就把所有的Bean全部实例化了。它还可以为Bean配置lazy-init=true来让Bean延迟实例化；

##### 该用BeanFactory还是ApplicationContent 

**延迟实例化的优点（BeanFactory）：**应用启动的时候占用资源很少；对资源要求较高的应用，比较有优势； 

**不延迟实例化的优点（ApplicationContext）：**

- 所有的Bean在启动的时候都加载，系统运行的速度快； 

- 在启动的时候所有的Bean都加载了，我们就能在系统启动的时候，尽早的发现系统中的配置问题 

- 建议web应用，在启动的时候就把所有的Bean都加载了。（把费时的操作放到系统启动中完成）

摘自：[BeanFactory和ApplicationContext的区别](https://blog.csdn.net/pythias_/article/details/82752881)