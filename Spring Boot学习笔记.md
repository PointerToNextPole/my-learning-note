# **Spring Boot学习笔记**
> 尚硅谷Spring Boot教程



## <font color=FF0000>**From 1**</font>

<font size=3>**简介**</br>
Spring Boot简化Spring应用开发，整个Spring技术栈的一个大整合，约定大于配置，去繁从简，just run就能创建一个独立的产品级别的应用</br>
Spring Boot -> J2EE一站式解决方案</br>
Spring Cloud -> 分布式整体解决方案</br>
**优点**</br>

- 快速创建独立运行的Spring项目以及与主流框架集成
- 使用嵌入式的Servlet容器，应用无需打包成WAR包
- starters（启动器）自动依赖与版本控制
- 大量的自动配置，简化开发，也可修改默认值
- 无需配置XML，无代码生成，开箱即用
- 准生产环境的运行时应用监控
- 与云计算天然集成
</font>

## <font color=FF0000>**From 2**</font>
<font size=3>**微服务**</br>
微服务Mircoservices是由Martin Fowler在2014年提出，是一种架构风格</br>
一个应用应该是一组小型服务，可以通过HTTP的方式进行互联</br>
每一个功能元素最终都是一个可独立替换和独立升级的软件单元</br>
</font>

## <font color=FF0000>**From 4**</font>
<font size=3>**spring boot Helloworld**</br>
1. 创建一个maven工程
2. 在pom.xml文件下导入spring-boot相关依赖
   ```xml
   <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.9.RELEASE</version>
    </parent>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
   ```
3. 编写主程序，启动spring-boot应用
   ```java
    @SpringBootApplication  //标注一个主程序类，说明这是一个Spring Boot应用
    public class HelloWorldMainApplication{
        public static void main(String[] args) {
            //Spring应用启动
            SpringApplication.run(HelloWorldMainApplication.class, args);
        }
    }
   ```
4. 编写相关的Controller、Service<font style=color:red>**（相当于实现了部分Spring MVC的功能，Spring Boot也简化了Spring MVC的一些步骤）**</font>
   ```java
   @Controller
    public class HelloController {
        @ResponseBody
        @RequestMapping("/hello")
        public String hello(){
            return "Hello World!";
        }
    }
   ```
5. 运行主程序
6. 部署（Spring Boot简化了部署），<font color=FF0000>**在pom.xml文件下配置将插件导入到项目中**</font>
   ```xml
   <!--这个插件，可以将应用打包成一个可执行的Jar包-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
   ```
   之后在IDEA界面右侧的Maven Projects选项中在对应的项目名下选择Lifecycle选项，并选择package选项，在项目target文件夹下生成了jar包，比如：spring-boot-01-helloworld-1.0-SNAPSHOT.jar。这时候在Terminal中键入 ```java -jar jar包名``` 即可在浏览器中运行（因为jar中内置Tomcat所以运行环境中无需安装Tomcat）
</font>

## <font color=FF0000>**From 5**</font>
<font size=3>**pom.xml文件**</br>
如下是pom.xml文件关于父项目的配置，该项目依赖于父项目spring-boot-starter-parent<mark>（这种关系类似于：子类依赖于父类）</mark>
```
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>        
    <version>1.5.9.RELEASE</version>
</parent>
```
而父项目spring-boot-starter-parent的配置表明：其还有父项目spring-boot-dependencies；如下：
```xml
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-dependencies</artifactId>
	<version>1.5.9.RELEASE</version>
	<relativePath>../../spring-boot-dependencies</relativePath>
</parent>
```
<mark>**父项目的父项目真正管理Spring Boot应用中的所有依赖**。我们可以将其称为Spring Boot的<font color=FF0000>**版本仲裁中心**</font></mark>以后我们导入依赖默认不需要写版本<mark>（没有在dependencies里面的依赖需要声明版本号）</mark></br>

**导入的依赖**
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```
这里依赖的是<mark>**spring-boot-starter**</mark>-<mark style=background-color:red>**web**</mark>，是spring-boot-start的<font color=FF0000>**一种**</font></br>
spring-boot-starter是spring-boot启动器（starter），<mark>**帮我们导入了web模块正常运行所需要的组件**</mark>。Spring Boot将所有的<font color=FF0000>**功能场景抽取出来，做成一个个starter**</font>，<mark>只需要在项目中引入这些starter相关场景的所有依赖都会导进来，要用什么功能就导入场景的启动器</mark></br>
更多启动器见[**Spring Boot官方文档**](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/)中的**3.1.5.Starters**
</font>

## <font color=FF0000>**From 6**</font>
<font size=3>**主程序类/主入口类**</br>
```java
@SpringBootApplication
public class HelloWorldMainApplication{

    public static void main(String[] args) {
        //Spring应用启动
        SpringApplication.run(HelloWorldMainApplication.class, args);
    }
}
```
<font color=FF0000>**@SpringBootApplication**</font>即为SpringBoot应用，标注在某个类上，<mark><font color=FF0000>说明这个类是SpringBoot的主配置类</font>，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用</mark>。查看@SpringBootApplication的源码，它是由如下注解实现，源码如下
```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {...} //...表示省略
```
- <font style=color:red>**@SpringBootConfiguration**</font>：Spring Boot的配置类。标注在某个类上，<mark>表示这是一个Spring Boot的配置类</mark>，而<font style=color:red>@SpringBootConfiguration</font>是由<font style=color:fuchsia>@Configuration实现</font>。
  
  - <font style=color:fuchsia>@configuration</font>：标注在配置类（相当于配置文件，<mark>只不过Spring用更简洁的配置类来替代过于麻烦的配置文件</mark>。配置类也是容器的一个组件（@Component））上来标注这个注解
- <font style=color:lime>**@EnableAutoConfiguration**</font>：<mark>开启自动配置功能：以前我们需要配置的东西，现在Spring Boot帮我们自动配置</mark>。<font style=color:lime>@EnableAutoConfiguration</font>告诉Spring Boot开启自动配置功能，这样自动配置才能生效。<font style=color:lime>@EnableAutoConfiguration</font>是由<font style=color:navy>@AutoConfigurationPackage</font>（自动配置包）来实现，如下：
  ```java
   @AutoConfigurationPackage
   @Import({EnableAutoConfigurationImportSelector.class})
   public @interface EnableAutoConfiguration {...}
  ```
  <font style=color:lime>@EnableAutoConfiguration</font>是由<font style=color:olive>@Import</font>(<font style=color:navy>AutoConfigurationPackages.Registrar.class</font>)实现</br>
  <font style=color:olive>@Import</font>是Spring的底层注解，<mark>其作用是给容器中导入一个组件</mark>，导入的组件是括号中的内容，这里的是<font style=color:navy>AutoConfigurationPackages.Registrar.class</font>
  - <font style=color:lime>@EnableAutoConfiguration</font>的<mark style=background-color:aqua;color:red>**实际原理**</mark>是<mark>将主配置类（@SpringBootApplication标注的类）所在包及其包含的所有子包内的所有组件扫描到Spring容器中</mark>
  - <font style=color:olive>@Import</font>(<font style=color:navy>AutoConfigurationPackages.Registrar.class</font>)是给容器中导入组件，<font style=color:navy>AutoConfigurationPackages</font>是开启自动导包的选择器，将所有需要导入的组件以全类名的形式返回，这些组件就会被添加容器中；最后会给容器中导入非常多的自动配置类（xxxAutoConfiguration），这些自动配置类的作用就是给容器中导入这些场景需要的所有组件，并配置好这些组件。有来自动配置类，免去了我们手动编写配置、注入功能组件等的工作。<mark>Spring Boot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值，将这些值作为自动配置类导入容器中，自动配置类就会生效，帮我们进行自动配置工作</mark>；以前我们需要自己配置的东西，现在自动配置类都帮我们自动配置了。J2EE的整体整合解决方案和自动配置都在Spring-boot-autoconfigure-1.5.9.RELEASE.jar中
</font>

## <font color=FF0000>**From 7**</font>
<font size=3>IDE都支持使用Spring的项目创建向导（Spring Initializer）快速创建Spring Boot程序（与在 https://start.spring.io/ 生成类似）</br>
```java
// @ResponseBody  //表明这个类的所有方法返回的数据以json的形式直接写给浏览器
// @Controller
@RestController  //就相当于@ResponseBody和@Controller的合体，所以可以替换这两者
public class HelloController {

    //@ResponseBody //表明当前方法返回的数据以json的形式直接写给浏览器
    @RequestMapping("/hello")
    public String hello(){
        return "hello world quick";
    }
}
```
**默认生成的Spring Boot项目有如下特点：**</br>
- 主程序都生成好了，我们只需要编写逻辑
- resources文件夹中的目录结构
  - static：保存所有的静态资源，js css images等
  - template：保存所有的模板页面（Spring Boot默认jar包使用嵌入式Tomcat，默认不支持JSP页面），可以使用模板引擎（freemarker、thymeleaf）
  - application.properties：Spring Boot应用的配置文件：比如修改使用的端口号只需写入```server.port=8081```，修改sql端口、密码、驱动等。该文件可以修改一些默认设置。
</font>

## <font color=FF0000>**From 8**</font>
<font size=3>**Spring Boot的配置文件**</br>
Spring Boot使用两种形式的配置文件作为全局配置文件，且配置文件名是固定的，如下：
- application.properties
- application.yml

**配置文件的作用**：修改Spring Boot自动配置的默认值（默认值即为Spring Boot在底层给我们配置好的配置）</br>
yml是YAML(YAML Ain't Markup Language)语言的文件，**是以数据为中心**（更加简洁），比json、xml等更适合做配置文件。yml配置格式示例如下：
```yaml
server:
    port: 8081
```
</font>

## <font color=FF0000>**From 9**</font>
<font size=3>**YAML基本语法**</br>
```key: value``` 表示一对键值对（<font color=FF0000>key和value之间空格必须有</font>）</br>
以**空格**的缩进来控制层级关系，只要是左对齐的一列数据都是同一层级的；<font color=FF0000>同时属性和值也是大小写敏感的</font>，示例如下：
```yaml
server:
    port: 8081
    path: /hello
```
**值（value）的写法**
- value是<font color=FF0000>**字面量**</font>：k: v 字面量直接来写。字符串默认<mark><font color=FF0000>**不用**</font></mark>加上<mark>单引号或者双引号</mark>
  - ""：双引号；<mark><font color=FF0000>**不会**</font>转义字符串里面的特殊字符：**特殊字符会作为本身想要表达的意思**</mark>，比如：name: "zhangsan \n lisi" -> 输出： zhangsan 换行 lisi
  - ''：单引号；<mark><font color=FF0000>**会**</font>转义字符串里面的特殊字符，**特殊字符最终只是一个普通得字符串数据**</mark>，比如：name: "zhangsan \n lisi" -> 输出： zhangsan \n lisi
- value是<font color=FF0000>**对象/Map**</font>：k: v 在下一行来写对象的属性和值的关系，注意缩进。同时对象还是k: v的方式。示例如下：</br>
  传统写法：
  ```yaml
  friends:
      lastName: zhangsan
      age: 20
  ```
  行内写法：
  ```yaml
  friends: {lastName: zhangsan,age: 18}
  ```
- value是<font color=FF0000>**数组（List，Set）**</font>：用-值表示数组中的一个元素</br>
  传统写法：
  ```yaml
  pets:
   - cat
   - dog
   - pig
  ```
  行内写法：
  ```yaml
  pets: [cat,dog,pig]
  ```
</font>

## <font color=FF0000>**From 10**</font>
<font size=3>**使用yaml在配置文件生成对象的数据，并将其与对应的类绑定**</br>
使用<font color=FF0000>注解@ConfigurationProperties(prefix="属性名")标注在需要绑定的类的定义上方</font>，将配置文件中的配置的每一个属性的值，映射到这个组件中。@ConfigurationProperties告诉Spring Boot将本类中的所有属性和配置文件中相关的配置进行绑定。<mark>另外，<font color=FF0000>非常重要的是</font>：只有这个组件（类）是容器的组件（加上@Component），才能使用容器所提供的@ConfigurationProperties功能。</mark>示例如下：
```java
@Component
@ConfigurationProperties(prefix="person")
public class Person{...} //类的定义内容省略
```
prefix="person"：配置文件中哪个下面的所有属性进行一一对应。</br>
同时，还需要在pom.xml文件中加上如下配置，否则IDE将会警告
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuartion-processor</artifactId>
    <optional>true</optional>
</dependency>
```
这样的话，<mark>导入配置文件处理器</mark>，配置文件进行绑定就会有提示。
</font>

## <font color=FF0000>**From 11**</font>
<font size=3>**在application.properties中配置组件的值**</br>
示例如下：
```java
#idea的properties文件默认使用utf-8，需要在运行时进行编码转换
# 配置person的值
person.last-name=张三
person.age=18
person.birth=2017/12/15
person.boss=false
person.maps.k1=v1
person.maps.k2=14
person.lists=a,b,c
person.dog.name=dog
person.dog.age=15
</font>
```
## <font color=FF0000>**From 12**</font>
<font size=3>**其他获取值的方法。使用@Value，示例如下：**

```java
@Component
public class Person{
    /**
    *JavaBean的传统写法
    *<bean class="Person">
    *    <property name="lastName" value="字面量"/${key}从环境变量、配置文件中获取值/#{SpEL} Spring EL表达式></property>
    *</bean>
    */
    @Value("${person.last-name}") 
    public String lastName;
    @Value("#{11*2}")  //SpEL表达式
    private Integer age;
    @Value("true")  //字面量
    private Boolean boss;
}
```
**@Value获取值和@ConfigurationProperties获取值比较**

|                                                              | @ConfigurationProperties |    @Value    |
| :----------------------------------------------------------: | :----------------------: | :----------: |
|                             功能                             |  批量注入配置文件的属性  | 一个一个指定 |
| 松散绑定<mark>(使用驼峰命名法与使用'-'和'_'连接命名没有区别)</mark> |           支持           |    不支持    |
|                             SpEL                             |          不支持          |     支持     |
|                        JSR303数据校验                        |           支持           |    不支持    |
|          <font color=FF0000>**复杂类型封装**</font>          |           支持           |    不支持    |

<mark>@Value获取值和@ConfigurationProperties获取值都可以在yml和properties中获得值</mark></br>

这两者该如何选择？

- 如果说，我们只是在某个业务逻辑中需要获取一下配置文件的某项值，使用@Value
- 如果说，我们专门编写了一个JavaBean来与配置文件进行映射，我们就使用@ConfigurationProperties

</font>

## <font color=FF0000>**From 13**</font>
<font size=3>**@PropertySource**</br>
因为<mark>@ConfigurationProperties默认是从全局配置文件中获取数据</mark>，这样配置文件将会很大，且耦合度过高。而<mark>**@PropertySource功能是加载指定的配置文件**</mark>。</br>
@PropertySource用法示例如下：

```java
@PropertySource(value={"classpath:person.properties"})
```
**@ImportResource**: 导入Spring的配置文件，让配置文件里面的内容生效。<mark>Spring Boot里面没有Spring的配置文件，我们自己编写的配置文件也不能自动识别</mark>。<mark>如果想要让Spring配置文件生效（被加载），就需要用@ImportResource标注在一个配置类上</mark>

@ImportResource用法示例如下：

```java
@ImportResource(location={"classpth:bean.xml"})
```

然而这种方法（编写Spring的配置文件）还是需要写配置文件，较为麻烦；所以Spring Boot推荐的给容器中添加组件的方式，<mark>推荐使用全注解的方式</mark>

- 写配置类（相当于Spring配置文件），需要在配置类之前添加注解@Configuration，用于指明当前类是一个配置类，用于替代之前的Spring配置文件
- 使用@Bean给容器添加组件

完整实例如下：

```java
@Configuration
public class MyAppConfig{
    @Bean
    public HelloService helloService(){...}
}
```

</font>



## <font color=FF0000>**From 14**</font>

<font size=3>**配置文件中的占位符**</br>

- RandomValuePropertySource：配置文件中可以使用<mark>随机数</mark>

  - ${random.value}
  - ${random.int}
  - ${random.long}
  - ${random.int(10)}
  - ${random.int[1024,65535]}

- 配置文件占位符：可以使用占位符获取之前文件中配置的值，如果没有，可以使用冒号<mark style=color:red>**( : )**</mark>指定默认值。示例如下：

  ```java
  person.dog.name=${person.hello:hello}_dog
  ```

  

</font>



## <font color=FF0000>**From 15**</font>

<font size=3>**Profile**</br>

Profile是Spring对不同环境（开发环境、生产环境、测试环境）提供不同配置功能的支持，可以通过激活、指定参数等方式<mark style=color:red>**快速切换环境**</mark>（比如在不同环境下使用不同的端口号）。

- **多profile文件**：我们在主配置文件编写的时候，文件名可以是application-{profile（这里可以填写dev、prod之类）}.properties / .yml。<mark>默认使用application.properties的配置</mark>

- **yml支持多文档块方式**（<mark>通过---来分隔文档块</mark>），示例如下：

  ```yaml
  server:
    port: 8081
  spring:
    profiles:
      active: prod  #默认
  
  ---
  server:
    port: 8083
  spring:
    profiles: dev  #指定属于哪个环境
  
  ---
  server:
    port: 8084
  spring:
    profiles: prod  #指定属于哪个环境
  ```

  

- **激活指定profile**

  - 在配置文件中指定，如：

    ```java
    spring.profiles.active=dev
    ```

  - 通过命令行的方式：

    - 可以在IDEA中的Run/Debug Configurations中设置program arguments

      ```--spring.profiles.avtive=dev```

    - 也可以在Terminal中进行设置

      ```java -jar jar包名.jar --spring.profiles.avtive=dev```

    - 虚拟机参数：在IDEA中的Run/Debug Configurations中设置VM options

      ```-Dspring.profiles.active=dev```

    

</font>



## <font color=FF0000>**From 16**</font>

<font size=3>配置文件加载位置</br>

Spring Boot<mark>会<font color=FF0000>**依次扫描**</font>（<font color=FF0000>**优先级从高到低，所有位置的文件都会被加载**</font>，<font color=FF0000>**高优先级配置的内容会覆盖低优先级配置的内容**</font>，可形成互补配置（即：高优先级如果有就用高优先级提供的，如果没有就用低优先级的））</mark>以下位置application.properties或者application.yml文件作为Spring Boot的默认文件

- file:./config/ （根目录下config文件夹）
- file:./ （根目录下）
- classpath:/config  （resource文件夹下config文件夹）
- classpath:/  （resource文件夹下）

我们也可以通过配置spring.config.location来改变配置文件位置。

项目打包以后，<mark>我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置</mark>；指定配置文件和默认加载的这些配置文件共同起作用形成互补配置。示例如下：

```
java -jar jar包名.jar --spring.config.location=配置文件地址
```

</font>



## <font color=FF0000>**From 17**</font>

**外部配置加载顺序**

Spring Boot也可以从以下顺序（<mark>按照优先级从高到低</mark>）加载位置，高优先级的配置会覆盖低优先级的配置，所有配置会形成互补配置<

**简单版如下：**

1. 命令行参数
2. 来自java:comp/env的JNDI属性
3. Java系统属性（ System.getProperties() ）
4. 操作系统环境变量
5. RandomValuePropertySource配置的random.*属性值
6. jar包外部的application-{profile}.properties或application.yml（带spring.profile）配置文件
7. jar包内部的application-{profile}.properties或application.yml（带spring.profile）配置文件
8. jar包外部的application.properties或application.yml（不带spring.profile）配置文件
9. jar包内部的application.properties或application.yml（不带spring.profile）配置文件
10. @Configuration注解类上的@PropertySource

如要了解详细的可以看官方文档 https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#boot-features  4.2. Externalized Configuration的内容

</font>



## <font color=FF0000>**From 18**</font>

配置文件到底能写什么，怎么写：可见官方文档 [**Appendix A: Common Application properties**](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#common-application-properties)  

- Spring Boot启动的时候加载主配置类，开启了自动配置功能（使用的是：`@EnableAutoConfiguration`）

- `@EnableAutoConfiguration`作用：利用`EnableAutoConfigurationImportSelector`给容器中导入一些组件。而`EnableAutoConfigurationImportSelector`的父类`AutoConfigurationImportSelector`规定了方法`selectImports()`，我们可以查看`selectImports()`方法中的内容。其中有一个非常重要的代码：

  ```java
  List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);
  ```

  功能是获取备选的配置。查看`getCandidateConfigurations()`方法的源码，有这样一行代码：`SpringFactoriesLoader.loadFactoryNames()`，`loadFactoryNames()`会扫描所有jar包类路径下的`META-INF/spring.factories`文件，把扫描到的这些文件的内容包装成properties对象。并从properties中获取EnableAutoConfiguration.class类名对应的值，然后将其添加到容器中。

  以上总结：将类路径下META-INF/Spring-factories里面配置的所有EnableAutoConfiguration的值加入到容器中

  




## <font color=FF0000>**From 19**</font>

<font size=3></br>

</font>