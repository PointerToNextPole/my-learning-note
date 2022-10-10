# Spring全家桶学习笔记



## 极客时间课程《玩转Spring全家桶》笔记



#### 杂项

Spring Boot和Spring最大的区别在于：自动配置（Spring Boot无需像Spring一样自己写配置了），约定大于配置。



Spring Boot的自动配置原理在External Libraries的autoconfigure下



Controller用于接收前端的请求，<font color=FF0000>@Controller作为路由API的承载者</font>（亦即：所有请求的入口）

```java
@Controller
public class IndexController {

    @GetMapping("/hello")
    public String hello(@RequestParam(name = "name") String name, Model model){
        model.addAttribute("name", name); //将浏览器传入的值传入model中，便于将其传到上下文中
        return "index";  //返回“index”，程序将会自动去resource/templates下去寻找名为index的html文件
    }
}
```

@Component仅仅将当前类初始化到Spring容器的上下文



### HttpServletRequest & HttpServletRespone

浏览器通过http协议与Tomcat（web服务器）通信时，会生成两个对象：<font color=FF0000>一个是HttpServletRequest对象</font>，<font color=0000FF>一个是HttpServletResponse对象</font>。它们是一对数据封装对象，<font color=FF0000>前者封装客户端的请求头</font>，<font color=0000FF>后者封装服务器的响应头</font>。



#### HttpServletRequest

HttpServletRequest对象<font color=FF0000>代表客户端的请求</font>，当客户端通过HTTP协议访问服务器时，HTTP请求头中的所有信息都封装在这个对象中，通过这个对象提供的方法，可以获得客户端请求的所有信息。

##### 获得客户机信息

|       方法        |                             注解                             |
| :---------------: | :----------------------------------------------------------: |
|  getRequestURL()  |                返回客户端发出请求时的完整URL                 |
|  getRequestURI()  |                    返回请求行中的参数部分                    |
| getQueryString () |                 返回发出请求的客户机的IP地址                 |
|  getRemoteHost()  |               返回发出请求的客户机的完整主机名               |
|  getRemoteAddr()  |                 返回发出请求的客户机的IP地址                 |
|   getPathInfo()   | 返回请求URL中的额外路径信息。额外路径信息是请求URL中的位于Servlet的路径之后和查询参数之前的内容，它以"/"开头 |
|  getRemotePort()  |                 返回客户机所使用的网络端口号                 |
|  getLocalAddr()   |                    返回WEB服务器的IP地址                     |
|  getLocalName()   |                    返回WEB服务器的主机名                     |

##### 获得客户机请求头

|          方法           |                             注解                             |
| :---------------------: | :----------------------------------------------------------: |
| getHeader(string name)  | 以 String 的形式返回指定请求头的值。如果该请求不包含指定名称的头，则此方法返回 null。如果有多个具有相同名称的头，则此方法返回请求中的第一个头。头名称是不区分大小写的。可以将此方法与任何请求头一起使用 |
| getHeaders(String name) |  以 String 对象的 Enumeration 的形式返回指定请求头的所有值   |
|    getHeaderNames()     | 返回此请求包含的所有头名称的枚举。如果该请求没有头，则此方法返回一个空枚举 |

##### 获得客户机请求参数

|              方法               |                             注解                             |
| :-----------------------------: | :----------------------------------------------------------: |
|    getParameter(String name)    |                  根据name获取请求参数(常用)                  |
| getParameterValues(String name) |                根据name获取请求参数列表(常用)                |
|        getParameterMap()        | 返回的是一个Map类型的值，该返回值记录着前端（如jsp页面）所提交请求中的请求参数和请求参数值的映射关系。(编写框架时常用) |



#### Model

为什么大多程序在controller中给jsp传值时使用`model.addAttribute()`而不使用`httpServeletRequest.setAttribute()`

<font color=FF0000>事实上model数据，最终spring也是写到HttpServletRequest属性中，只是用model更符合mvc设计，减少各层间耦合</font>。

摘自：[Java SpringMVC框架学习（二）httpServeltRequest和Model传值的区别](https://www.cnblogs.com/cnki/p/9059262.html)



#### From 1.4

在代码编写完成后，在terminal键入如下代码，将会<mark>显示对应网址所显示的HTML</mark>

```powershell
curl http://localhost:8080/hello
```

类似的由于使用了`Spring Boot Actuator`，可以通过如下代码<mark>显示当前应用程序的状态</mark>

```powershell
curl http://localhost:8080/actuator/health
```

在pom.xml中如下配置的**功能**是<mark>在打包的过程中生成一个可执行的jar包</mark>

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

使用如下命令将可以进行打包：`mvn clean package -Dmaven.test.skip`

生成实际使用的jar包：`target/hello-spring-0.0.1-SNAPSHOT.jar`

通过`java -jar`便可执行jar包`java -jar hello-spring-0.0.1-SNAPSHOT.jar`，可在浏览器中访问项目内容

**如果项目由于某些原因不能使用自带的spring-boot-start-parent作为parent，可以通过修改`pom.xml`文件得以实现，代码示例如下：**<mark style=background-color:fuchsia>**（这部分没有完全看懂）**</mark>

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-dependencies</artifactId>
			<version>2.1.1.RELEASE</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
	</dependencies>
</dependencyManagement>

<build>
	<plugins>
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
			<version>2.1.1.RELEASE</version>
			<executions>
				<execution>
					<goals>
						<goal>repackage</goal>
					</goals>
				</execution>
			</executions>
		</plugin>
	</plugins>
</build>
```

另外，可以参考阅读：[Spring Boot Jar 包 配置、依赖文件分离](https://www.jianshu.com/p/a97501bb5460)，了解更多关于Spring Boot打包的知识



#### From 2.1

##### 配置单个数据源

```java
@SpringBootApplication
@Slf4j
public class DatasourcedemoApplication implements CommandLineRunner {

    @Autowired
    private DataSource dataSource;

    @Autowired
    public JdbcTemplate jdbcTemplate;

    public static void main(String[] args) {
        SpringApplication.run(DatasourcedemoApplication.class, args);
    }

    @Override
    public void run(String... arg) throws Exception{
        showConnection();
        showData();
    }

    private void showConnection() throws SQLException{
        log.info(dataSource.toString());
        Connection conn = dataSource.getConnection();
        log.info(conn.toString());
        conn.close();
    }

    private void showData(){
        jdbcTemplate.queryForList("select * from foo").forEach(row -> log.info(row.toString()));
    }
}
```



#### From 2.2

##### 配置多数据源

不同数据源的配置要分开配置

```properties
#application.properties
management.endpoints.web.exposure.include=*
spring.output.ansi.enabled=always

#数据源foo
foo.datasource.url=jdbc:h2:mem:foo
foo.datasource.username=sa
foo.datadource.password=

#数据源bar
bar.datasource.url=jdbc:h2:mem:bar
bar.datasource.username=sa
bar.datasource.password=
```

```java
 /**
  *geektime.spring.data.multidatasourcedemo.MultidatasourcedemoApplication
  */

//不使用spring boot自带的如下配置
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class,
        DataSourceTransactionManagerAutoConfiguration.class,
        JdbcTemplateAutoConfiguration.class})
@Slf4j
public class MultidatasourcedemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(MultidatasourcedemoApplication.class, args);
    }

    /**
     *对于数据源foo的使用
     */
    @Bean
    @ConfigurationProperties("foo.datasource")
    public DataSourceProperties fooDataSourceProperties(){
        return new DataSourceProperties();
    }

    @Bean
    public DataSource fooDataSource(){
        DataSourceProperties dataSourceProperties = fooDataSourceProperties();
        log.info("foo datasource: {}", dataSourceProperties.getUrl());
        return dataSourceProperties.initializeDataSourceBuilder().build();
    }

    @Bean
    @Resource
    public PlatformTransactionManager fooTxManager(DataSource fooDataSource){
        return new DataSourceTransactionManager(fooDataSource);
    }

    /**
     *对于数据源bar的使用
     */
    @Bean
    @ConfigurationProperties("bar.datasource")
    public DataSourceProperties barDataSourceProperties(){
        return new DataSourceProperties();
    }

    @Bean
    public DataSource barDataSource(){
        DataSourceProperties dataSourceProperties = barDataSourceProperties();
        log.info("bar datasource: {}", dataSourceProperties.getUrl());
        return dataSourceProperties.initializeDataSourceBuilder().build();
    }

    @Bean
    @Resource
    public PlatformTransactionManager barTxManager(DataSource barDataSource){
        return new DataSourceTransactionManager(barDataSource);
    }
}
```



#### From 2.3

**介绍hikariCP连接池**

Spring Boot2.x默认使用hikariCP



#### From 2.4

**介绍Druid连接池**



#### From 2.5

**Spring中JDBC操作**：spring-jdbc

- **core**：JdbcTemplate等相关核心接口和类
- **datasource**：数据源相关的辅助类
- **object**：将基本的JDBC操作封装成对象
- **support**：错误码等其他辅助工具

**常用的Bean注解**

- **@Component：**通用的注解，用于定义一个通用的Bean
- <font color=FF0000>**@Repository：**</font>用于DAO，相当于数据操作的仓库；<mark>建议数据库相关操作都放在@Repository定义的Bean当中</mark>
- **@Service：**用于业务的服务
- **@Controller：**用于Spring MVC
  - **RestController：**pring便于开发Restful web service而提供

**JdbcTemplate**中定义了增删改查所需的各种方法

- **query**
- **queryForObject：**取出单个对象
- **queryForList：**取出一批数据
- **queryForMap**
- **update：**可以实现插入修改删除
- **execute**

**可以使用<mark>lombok</mark>简化类定义<mark>（与JPA有关）</mark>**

- 引入lombok.Data，<marK>可使用@Data，以省去Getter，Setter方法</mark>
- 引入lombok.Builder，<mark>可使用@Builder，以省去构造方法</mark>

**SQL批处理**

- JdbcTemplate
  - batchUpdate
  - BatchPreparedStatementSetter
- NameParameterJdbcTemplate
  - batchUpdate
  - SqlParameterSourceUtils.createBatch



#### From 2.6

##### Spring事务抽象

**spring提供了<mark>一致的事务模型</mark>**

- JDBC / Hibernate / myBatis
- DataSource / JTA

**事务抽象的核心接口：**

- PlatformTransactionManager

  - **DataSource**TransactionManager
  - **Hibernate**TransactionManager
  - **Jta**TransactionManager

  其中定义了如下方法

  ```java
  void commit(TransactionStatus status) throws TransactionException;
  void rollback(TransactionStatus status) throws TransactionException;
  TransactionStatus getTransaction(@Nullable TransactionDefinition definition) throws TransactionException;
  ```

###### spring事务的传播特性（7种）

|                  传播性                   |  值  |                 描述                 |
| :---------------------------------------: | :--: | :----------------------------------: |
| PROPAGATION_REQUIRED<mark>（默认）</mark> |  0   |  当前有事物就用当前的，没有就用新的  |
|           PROPAGATION_SUPPORTS            |  1   |       事务可有可无，不是必须的       |
|           PROPAGATION_MANDATORY           |  2   |    当前一定要有事务，不然就抛异常    |
|         PROPAGATION_REQUIRES_NEW          |  3   |    无论是否有事务，都起个新的事物    |
|         PROPAGATION_NOT_SUPPORTED         |  4   |     不支持事务，按费事务方式运行     |
|            PROPAGATION__NEVER             |  5   |   不支持事务，如果有事务则抛出异常   |
|            PROPAGATION_NESTED             |  6   | 当前有事务就在当前事务里再起一个事务 |

###### 事务隔离特性（与数据库相关，由数据库决定，默认值为-1）

|           隔离性           |  值  | 脏读 | 不可复读 | 幻读 |
| :------------------------: | :--: | :--: | :------: | :--: |
| ISOLATION_READ_UNCOMMITTED |  1   |  √   |    √     |  √   |
|  ISOLATION_READ_COMMITTED  |  2   |  ×   |    √     |  √   |
| ISOLATION_REPEATABLE_READ  |  3   |  ×   |    ×     |  √   |
|   ISOLATION_SERIALIZABLE   |  4   |  ×   |    ×     |  ×   |



#### From 2.7

##### 基于注解配置声明式事务

###### 开启事务注解的两种方法

- 添加`@EnableTransactionManagement`注解
- 在xml文件中添加`<tx.annotation-driven/>`

###### 一些配置

- proxyTargetClass：设置AOP是基于接口还是类，一般情况下是定义接口，则设置为True；否则设置为False
- mode：可选AspectJ，但一般默认为Java
- order：设置事务的AOP拦截顺序

**当开启事务注解的支持后，在需要的方法和类上使用@Transactional注解；下面有一些设置**

- transactionManager：选择哪一个transactionManager，比如：DataSourceTransactionManager之类，
- propagation：传播性
- isolation：隔离性
- timeout：
- readOnly
- 如何判断回滚：设置当碰到特定的异常类时回滚

##### @Transactioinal(rollbackFor)

`@Transactional`的`rollbackFor`用于<mark>指定能够触发事务回滚的异常类型</mark>，<mark>可以指定多个，用逗号分隔</mark>。
 `rollbackFor`<mark>默认值为UncheckedException</mark>，包括了RuntimeException和Error.
 当我们直接使用`@Transactional`不指定`rollbackFor`时，Exception及其子类都不会触发回滚。

上面关于@Transactional的内容摘自：[**简书文章：@Transactional(rollbackFor)**](https://www.jianshu.com/p/9098372c108a)

**在方法上加上`@Transactinal`的注解，<font color=FF0000>将赋予整个方法以事务的特性，方法要么commit，要么rollback。</font>**



#### From 2.8

##### Spring的JDBC异常

Spring会将操作访问的异常转化为DataAccessException，即无论使用何种数据访问方式（使用何种数据库、何种ORM框架），都能使用一样的异常。这样可以方便开发人员处理。

###### 实现方法

通过SQL<mark>ErrorCode</mark>SQLExceptionTranslator类进行**解析错误码**

其中ErrorCode定义：

- 默认配置：org/springframework/jdbc/support/sql-error-codes.xml
- 若想自定义：classpath下的sql-error-codes.xml以覆盖掉官方的文件配置

如何自定义并使用，见视频



#### From 2.9

##### Java config相关注解

- **@Configuration**：标明当前类是一个配置类

- **@ImportResource**：注入配置以外的xml的配置文件的配置

- **@ComponentScan**：告诉spring的容器，可以扫描哪些package下的Bean的一些配置

  可以参考：[深入理解spring注解之@ComponentScan注解](https://www.cnblogs.com/jpfss/p/11171655.html)

- **@Bean**：在一个Java config类当中，如果一个方法被标注了@Bean，它的返回就可以作为一个Bean的配置存在于spring application context当中

- **@ConfigurationProperties**：将一些其他配置绑定来

##### 与Bean的定义相关的注解

- **@Component** 所有Java Bean都可以同@Component来定义，spring也提供了其他含有语义的注解，如下：
  -  **@Repository** 数据访问层的Bean
  - **@Service** 服务层Bean
- **@Controller** web层的Bean / **@RestController** 与REST相关的web层的Bean 
- **@RequestMapping** 帮助定义当前方法和类下面的方法<mark>在哪个url下面的</mark>，做一个映射

##### 注入相关注解

- **@AutoWired** / **@Qualifier**：有多个同类型的Bean，只使用Bean会有歧义，通过@Qualifier来指定 / **@Resource**：通过名字来注入
- **@Value**：在Bean中注入常量或SpEL表达式

***

##### Actuator提供的一些好用的Endpoint

|        URL         |         作用         |
| :----------------: | :------------------: |
|  /actuator/health  |       健康检查       |
|  /actuator/beans   | 查看容器中的所有Bean |
| /actuator/mappings |   查看Web的URL映射   |
|   /actuator/env    |     查看环境信息     |

默认`/actuator/health`和`/actuator/info`可通过web访问，其他需要配置进行解禁：

**解禁Endpoint**：

在application.properties / application.yml文件下设置`management.endpoints.web.exposure.include=*`表示解禁所有，同时也可以按需索取只配上`health`或`beans`（两者通过逗号连接），如下：`management.endpoints.web.exposure.include=health,beans`

<mark style=color:red>当然生产环境需要谨慎，比如包含shutdown等Endpoint是关闭系统的</mark>

***

有<mark>**多数据源、分库分表、读写分离（出于性能考虑）**</mark>的需求可使用<mark>数据库中间件</mark>作为路由，以使得逻辑上成为一个整体，简化开发。



#### From 2.10

**REQUIRES_NEW和NESTED事务<mark>传播特性</mark>**

**REQUIRES_NEW：**始终启动一个<mark>新事务</mark>

- 两个事务没有关联

**NESTED：**在<mark>原事务</mark>内启动一个内嵌事务

- 原事务和内嵌事务，两个事务有关联
- 若<mark>外部事务</mark>**回滚**，<mark style=background-color:aqua>内嵌事务</mark>也会**回滚**；若<mark style=background-color:aqua>内嵌事务</mark>**回滚**，<mark>外部事务</mark>**不会回滚**（不受影响）

***

关于Druid（略）



#### From 3.1

**对象与关系的范式不匹配**

|          |        Object        | RDBMS |
| :------: | :------------------: | :---: |
|   粒度   |          类          |  表   |
|   继承   |          有          |  无   |
|  唯一性  | a == b / a.equals(b) | 主键  |
|   关联   |         引用         | 外键  |
| 数据访问 |       逐级访问       |  SQL  |

***

**JPA** (Java Persistence API)：是**为对象关系映射提供了一种POJO的持久化模型**

- 简化数据持久化代码的开发工作
- 为Java社区屏蔽不同持久化API的差异

最早的时候，Spring在框架内部（spring framework）支持JDBC、Hibernate等特性；之后Spring将于数据相关的内容从spring framework中剥离出来，放入Spring Data；而Spring Data中包含`Spring Data Common`、`Spring Data JDBC`、`Spring Data JPA`、`Spring Data Redis`...在保留这些不同特性的实现基础上又提供了一层抽象

引入Spring Data的配置

- 未使用Spring Boot

  ```xml
  <dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.data</groupid>
            <artifactId>spring-data-releasetrain</artifactId>
            <version>Lovelace-SR4</version>
            <scope>import</scope>
            <type>pom</type>
        </dependency>
    </dependencies>
  </dependencyManagement>
  ```

- 使用Spring Boot

  ```xml
  <dependency>
    <groudId>org.springframework.boot</groudId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
  </dependency>
  ```



#### From 3.2

##### 常用JPA注解

**在JPA中使用注解来定义实体**，常用的注解有：

- 与实体定义相关：
  - **@Entity** 注明这个类是实体类（定义对象将会成为被JPA管理的实体，将映射到指定的数据库表）、**@Mapped<mark>Super</mark>Class** 有多个实体类，这些类有一个父类，在父类上标注@MappedSuperClass
  
    **关于@MappedSuperClass的补充：**
  
    在Jpa里, 当我们在定义多个实体类时, 可能会遇到这几个实体类都有几个共同的属性, 这时就会出现很多重复代码. 这时我们可以选择编写一个父类,将这些共同属性放到这个父类中, 并且在父类上加上@MappedSuperclass注解.
  
    <font color=FF0000>**注意：**</font>标注为@MappedSuperclass的类将不是一个完整的实体类，他将不会映射到数据库表，但是他的属性都将映射到其子类的数据库字段中。
  
    摘自：[@MappedSuperclass的作用](https://www.cnblogs.com/shenhanboBlog/p/9922244.html)
  
  - **@Table(name)** 将 实体 与 与之对应的表关联起来
- 主键
  - **@Id** 定义属性为数据库的主键，一个实体里面必须有一个
    - **@GeneratedValue(strategy, generator)** 在@Id中会有自增序列、自动生成的主键，所以通过@GeneratedValue来指定生成策略（strategy）和生成器（generator）
    - **@SequenceGenerator(name, sequenceName)** 如果使用的序列，需要指明是什么序列
- 与映射相关的注解
  - 映射
    - **@Column(name, nullable, length, insertable, updatable)** 用于定义属性和表中字段的映射关系。
      - name：字段名，一般属性名就是字段的名字(name)，当然也可以自定义name
      - nullable：字段是否为空
      - length：字段长度是多少
      - insertable、updatable：字段是否是只能在添加是添加，之后无法再修改
    - **@JoinTable(name)**、**@JoinColumn(name)**：用于关联时使用
  - 关系
    - **@OneToOne**、**@OneToMany**、**@ManyToOne**、**@ManyToMany**：用于定义<mark>表间关系</mark>
    - **@OrderBy**：用于数据排序



##### Lombok

Lombok能够自动嵌入IDE和构建工具，提升开发效率

**常用功能**

- **@Getter** / **@Setter**：可以避免写getter/setter方法
- **@ToString**：可以避免写toString方法
- **@NoArgsConstructor** / **@RequiredArgsConstructor** / **@AllArgsConstructor**：替代构造方法，其中@NoArgsConstructor是无参构造器，@AllArgsConstructor是包含所有参数的构造器
- **@Data**：混合，包含@Getter、@Setter、@ToString
- **@Builder**：生成build方法，便于构造对象
- **@Slf4j** / **@CommonsLog** / **@Log4j2**：日志相关注解
- **补充：**注解中有一个参数callSuper=true/false，表示是否调用父类的该方法。比如`@ToString(callSuper = true)`



#### From 3.3

实体定义，示例如下：

```java
/**
 *Coffee类
 */
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.hibernate.annotations.CreationTimestamp;
import org.hibernate.annotations.Type;
import org.hibernate.annotations.UpdateTimestamp;
import org.joda.money.Money;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.Table;
import java.io.Serializable;
import java.util.Date;

@Entity
@Table(name="T_MENU")
@Builder
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Coffee implements Serializable {
    @Id
    @GeneratedValue
    private Long id;
    private String name;
    @Column
    @Type(type="org.jadira.usertype.moneyandcurrency.joda.PersistentMoneyAmount",
            parameters = {@org.hibernate.annotations.Parameter(name="currencyCode", value = "CNY")})
    private Money price;
    @Column(updatable = false)
    @CreationTimestamp
    private Date createTime;
    @UpdateTimestamp
    private Date updateTime;
}
```

```java
/**
 *CoffeeOrder类
 */
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.hibernate.annotations.CreationTimestamp;
import org.hibernate.annotations.UpdateTimestamp;

import javax.persistence.*;
import java.io.Serializable;
import java.util.Date;
import java.util.List;

@Entity
@Table(name="T_ORDER")
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class CoffeeOrder implements Serializable {
    @Id
    @GeneratedValue
    private Long id;
    private String customer;
    @ManyToMany
    @JoinTable(name="T_ORDER_COFFEE")
    private List<Coffee> items;
    @Column(nullable = false)
    private Integer state;
    @Column(updatable = false)
    @CreationTimestamp
    private Date createTime;
    @UpdateTimestamp
    private Date updateTime;
}
```



#### From 3.4

##### 通过SpringDataJPA操控数据库

关于JPA可以参考文章：[SpingDataJpa](https://blog.csdn.net/HQZ820844012/article/details/80188742)

加上`@EnableJpaRepositories`注解之后，将会自动发现`Repository<T, ID>`接口，及子接口：

- CrudRepository<T, ID>
- PagingAndSortingRepository<T, ID>
- JpaRepository<T, ID>

**根据<mark>方法名</mark>定义查询**

- **find...By / read...By / query...By / get...By**：在这里find / read / query / get 被看作同义词，都表示查询
- **count...By**：作计数
- **...OrderBy...[Asc / Desc]**：根据哪个字段来进行升序和降序
- **And / Or / IngoreCase**：多个条件
- **Top / First / Distinct**

另外参考文章：[JPA 方法名规则命名查询](https://www.cnblogs.com/powerwu/articles/10734311.html)

**如要使用分页查询**，需要使用`PagingAndSortingRespository<T, ID>`，以及子接口

- **Pageable**：记录分页信息 / **Sort**：记录排序信息
- **Slice\<T>** / **Page\<T>**



@Query

@Modifying



#### From 3.5

##### Repository是如何变成Bean的

- <mark>**JpaRepositoriesRegistrar**</mark>激活了`@EnableJpaRepositories`返回了`JpaRepositoryConfigExtension`
- <mark>**RepositoryBeanDefinitionRegistrarSupport.registerBeanDefinition**</mark>注册Repository Bean（类型是JpaRepositoryFactoryBean）
- <mark>**RepositoryConfigurationExtensionSupport.getRepositoryConfigurtions**</mark>取得Repository配置
- <mark>**JpaRepositoryFactory.getTargetRepository**</mark>创建Repository

##### 接口中的方法是如何被解释的

**RepositoryFactorySupport.getRepository添加了Advice**

- DefaultMethodInvokingMethodInterceptor
- QueryExecutorMethodInterceptor

AbstractJpaQuery.execute 执行具体的查询

语法解析在Part中



#### From 3.6

**MyBatis需要自己定制SQL，而Jpa和hibernate可以根据映射关系写出来的，该如何使用？**

- 若对于数据库操作简单，可使用jpa和hibernate以屏蔽sql层面的差异
- 若数据库操作较为复杂，使用mybatis；同时在一些大厂中，更偏向于使用mybatis，这样DBA对SQL有更强的把控程度

***

在Spring中使用MyBatis

##### 在Spring中使用mybatis

- MyBatis Spring Adapter (https://github.com/mybatis/spring)

- MyBatis Spring-Boot-Starter (https://github.com/mybatis/spring-boot-starter)

  在pom.xml文件中的配置

  ```xml
  <dependency>
  	<groupId>org.mybatis.spring.boot</groupId>
  	<artifactId>mybatis-spring-boot-starter</artifactId>
  	<version>1.3.2</version>
  </dependency>
  ```

**在spring boot中使用的简单配置**

- mybatis.mapper-locations = classpath*:mapper/**/\*.xml   //mapper文件放置的位置，如果用了maven工程，mapper一般放在resource中

- mybatis.type-aliases-package = 类型别名的包名

- mybatis.type-handler-package = TypeHandler扫描包名  //类型转换

  - 在 MyBatis 的 sql 映射配置文件中，<mark>为 sql 配置的输入参数最终要从 java 类型转换成数据库能识别的类型，而从 sql 的查询结果集中获取的数据，也要从数据库的数据类型转换为对应的 Java 类型</mark>。

    在 MyBatis 中，**使用类型处理器（TypeHandler）**将数据库获取的值以合适的方式转换为 Java 类型，或者将 Java 类型的参数转换为数据库对应的类型。

    在 MyBatis 中有许多自带的类型处理器，但有时候也会满足不了开发的需求，这时候就需要配置自己的类型处理器了，而 typeHandlers 标签就是用来声明自己的类型处理器的。

    摘自：[**MyBatis 配置 typeHandlers 详解**](https://blog.csdn.net/fageweiketang/article/details/80787213)

- `mybatis.configuration.map-underscore-to-camel-case = true`   //将下划线命名与驼峰命名相对应

**Mapper的定义与扫描**

- **映射的定义** 使用XML与注解
- **@MapperScan** 配置扫描位置
- **@Mapper** 定义接口

示例如下：

```java
/**
 *coffeeMapper.java
 */
@Mapper
public interface CoffeeMapper {
    @Insert("insert into t_coffee (name, price, create_time, update_time)"
            + "values (#{name}, #{price}, now(), now())")
    @Options(useGeneratedKeys = true)
    Long save(Coffee coffee);

    @Select("select * from t_coffee where id = #{id}")
    @Results({
            @Result(id = true, column = "id", property = "id"),
            @Result(column = "create_time", property = "createTime"),
            // map-underscore-to-camel-case = true 可以实现一样的效果
            // @Result(column = "update_time", property = "updateTime"),
    })
    Coffee findById(@Param("id") Long id);
}
```

`@Options(useGeneratedKeys = true, keyProperty = “xxxx”)` <mark style=color:red>**作用**</mark>：

- useGeneratedKeys 设置为"true"表明要 MyBatis 获取由数据库自动生成的主键；
- keyProperty="id"指定把获取到的主键值注入到 XXX（实体类） 的 id 属性。

`@Result`**解释**：

```java
    @Results({
            //配置column和property将数据库中字段与类的属性相关联，id = true表明配置的是主键
            @Result(id = true, column = "id", property = "id"),
            @Result(column = "create_time", property = "createTime"),
            // map-underscore-to-camel-case = true 可以实现一样的效果
            // @Result(column = "update_time", property = "updateTime"),
    })
```

#### Mybatis RowBounds

Mybatis提供了一个简单的逻辑<font color=FF0000>**分页**</font>使用类RowBounds<mark>（物理分页当然就是我们在sql语句中指定limit和offset值）</mark>，在DefaultSqlSession提供的某些查询接口中我们可以看到RowBounds是作为参数用来进行分页的，如下接口：

```java
 public <E> List<E> selectList(String statement, Object parameter, RowBounds rowBounds)
```

摘自：[Mybatis逻辑分页原理解析RowBounds](https://blog.csdn.net/qq924862077/article/details/52611848)

调用示例如下：

```java
//new RowBounds(0, 5)，即第一页，每页取5条数据
List<Book> list = bookMapper.selectBookByName(map, new RowBounds(0, 5));
```

摘自：[mybatis RowBounds 分页](https://blog.csdn.net/wsjzzcbq/article/details/83447948)



#### From 3.7

##### MyBatis Generator

**Mybatis Generator** (https://www.mybatis.org/generator/)是一个MyBatis代码生成器，可以根据数据表生成相关代码（包括：POJO， Mapper接口， SQL Map XML）

**运行MyBatis Generator的方式**

- <mark>**命令行**</mark>
  - java - jar mybatis-generator-core-x.x.x.jar -configfile generatorConfig.xml
- <mark>**Maven Plugin(mybatis-generator-maven-plugin)**</mark>
  - mvn mybatis-generator:generate
  - ${basedir}/src/main/resources/generatorConfig.xml
- **Eclipse Plugin**
- <mark>**Java程序**</mark>
- **Ant Task**

**配置MyBatis Generator**（在generatorConfig.xml下）

在\<generatorConfiguration>下对上下文进行配置，包括：

- **jdbcConection**：jdbc连接，需要URL和密码
- **javaModelGenerator**：model生成
- **sqlMapGenerator**：mapper生成
- **javaClientGenerator** (**<mark>ANNOTATIED</mark>MAPPER** 纯注解 / **<mark>XML</mark>MAPPER** xml配置 / **<mark>MIXED</mark>MAPPER** 混合配置)
- **table**：库中表的配置，给那些表生成映射

示例如下：

```xml
<generatorConfiguration>
    <context id="H2Tables" targetRuntime="MyBatis3">
        <plugin type="org.mybatis.generator.plugins.FluentBuilderMethodsPlugin" />
        <plugin type="org.mybatis.generator.plugins.ToStringPlugin" />
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin" />
        <plugin type="org.mybatis.generator.plugins.RowBoundsPlugin" />

        <jdbcConnection driverClass="org.h2.Driver"
                        connectionURL="jdbc:h2:mem:testdb"
                        userId="sa"
                        password="">
        </jdbcConnection>

        <javaModelGenerator targetPackage="geektime.spring.data.mybatis.model"
                            targetProject="./src/main/java">
            <property name="enableSubPackages" value="true" />
            <property name="trimStrings" value="true" />
        </javaModelGenerator>

        <sqlMapGenerator targetPackage="geektime.spring.data.mybatis.mapper"
                         targetProject="./src/main/resources/mapper">
            <property name="enableSubPackages" value="true" />
        </sqlMapGenerator>

        <javaClientGenerator type="MIXEDMAPPER"
                             targetPackage="geektime.spring.data.mybatis.mapper"
                             targetProject="./src/main/java">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>

        <table tableName="t_coffee" domainObjectName="Coffee" >
            <generatedKey column="id" sqlStatement="CALL IDENTITY()" identity="true" />
            <columnOverride column="price" javaType="org.joda.money.Money" jdbcType="BIGINT"
                            typeHandler="geektime.spring.data.mybatis.handler.MoneyTypeHandler"/>
        </table>
    </context>
</generatorConfiguration>
```

**MyBatis Generator插件**：内置插件都在`org.mybatis.generator.plugins`包中，包含FluentBuilderMethodsPlugin，ToStringPlugin，SerializablePlugin，RowBoundPlugin，...

**使用生成的对象**

- **简单操作**：直接使用生成的xxxMapper方法
- **复杂操作**：使用生成的xxxExample对象

补充：如果需要mybatis-generator又需要进行微调，建议将微调（手动修改）的东西放在放在不同的位置

由于讲的不够明白，所以可以看参考资料：[利用mybatis-generator自动生成代码](https://www.cnblogs.com/yjmyzz/p/mybatis-generator-tutorial.html)



#### From 3.8

##### MyBatis PageHelper

mybatis-pagehelper用于作分页

- 支持多种数据库
- 支持多种分页方式
- spring-boot支持（https://github.com/pagehelper/pagehelper-spring-boot）
  - 在Maven中配置pagehelper-spring-boot-starter



#### From 4.1

##### Docker常用命令

###### 镜像相关

- **docker pull \<image>：**从hub下载镜像
- **docker search \<image>：**搜索镜像

###### 容器相关

- **docker run：**运行镜像
- **docker start / stop <容器名>：**启动和停止容器
- **docker ps \<容器名>：**
- **docker logs \<容器名>：**查看日志

###### docker run 常用选项

**docker run [options] image [command] [arg...]**，选项说明：

- -d，后台运行容器
- -e，设置运行环境
- --expose / -p 宿主端口：容器端口
- --name，指定容器名称
- --link，连接不同容器
- -v 宿主目录:容器目录，挂载磁盘卷



#### From 4.2

##### Spring Data MongoDB 的基本用法
**注解**

- **@Document**：<mark>表明这个类对应的是哪一个文档 </mark>。把一个Java类声明为mongodb的文档，可以通过collection参数指定这个类对应的文档。

  **补充：**

  - <mark>文档是 mongodb 基本的数据组织单元</mark>，类似于mysql 中的记录
  - 文档由多个键值对组成，每个键值对表达一个数据项
  - 文档是bson 数据（类似于json）

- **@Id**：在mongodb中每一个文档都会有一个id

**MongoTemplate**包含增删改查的方法

- **save / remove**
- **Criteria / Query / Update**

##### 初始化MongoDB的库及权限

- **创建库 ：**`use springbucks;`

- **创建用户：**

  ```json
  db.createUser(
    {
      user: "springbucks",
      pwd: "springbucks",
      roles: [
         { role: "readWrite", db: "springbucks" }
      ]
    }
  )
  ```

##### Spring Data MongoDB 的 Repository

**@EnableMongoRepositories对应接口**

- **MongoRepository<T, ID>**
- **PagingAndSortingRepository<T, ID>**
- **CrudRepository<T, ID>**



#### From 4.3

##### Spring 对 Redis 的支持

**Spring Data Redis**

- **支持的客户端 Jedis / Lettuce**
- **RedisTemplate**
- **Repository 支持**

##### Jedis 客户端的简单使用

- Jedis 不是线程安全的（所以往往使用 JedisPool ）
- 通过 JedisPool 获得 Jedis 实例
- 直接使用 Jedis 中的方法

##### Jedis客户端的简单使用

```java
@Bean
@ConfigurationProperties("redis")
public JedisPoolConfig jedisPoolConfig() {
	return new JedisPoolConfig();
}

@Bean(destroyMethod = "close")
public JedisPool jedisPool(@Value("${redis.host}") String host) {
	return new JedisPool(jedisPoolConfig(), host);
}
```



#### From 4.4

##### Redis的哨兵和集群模式



#### From 4.5

##### Spring 的缓存抽象

**Spring为不同的缓存提供一层抽象**

- 为 Java 方法增加缓存，缓存执行结果
- 支持ConcurrentMap、EhCache、Caffeine、JCache（JSR-107）
- **接口**
  - org.springframework.cache.Cache
  - org.springframework.cache.CacheManager

##### Spring基于注解的缓存
**@EnableCaching**

- **@Cacheable**：开启缓存
- **@CacheEvict**：缓存清理
- **@CachePut**：无论如何，做一个缓存的设置
- **@Caching**：对缓存清理等上面设置的打包，可以在其中放入多个操作
- **@CacheConfig**：对缓存着一个设置，设置缓存的名字



#### From 4.6

##### Redis 在 Spring 中的其他⽤用法

**配置连接工厂**

- <font color=FF0000>**LettuceConnectionFactory**</font> （Spring Data Redis的默认客户端）与 **JedisConnectionFactory**
- **RedisStandaloneConfiguration**
- **RedisSentinelConfiguration**
- **RedisClusterConfiguration**

##### 读写分离

**Lettuce 内置支持读写分离**

- **只读主、只读从**
- **优先读主、优先读从**

LettuceClientConfiguration
LettucePoolingClientConfiguration
LettuceClientConfigurationBuilderCustomizer



#### From 5.1

##### Project Reactor

**响应式编程**

> 在计算机中，响应式编程或反应式编程（英语：Reactive Programming）是一种<mark>面向数据流和变化传播的编程范式</mark>。这意味着可以在编程语言中很方便地表达静态或动态的数据流，而相关的计算模型会自动将变化的值通过数据流进行传播。



#### From 6.1

##### Spring MVC

**核心：DispatcherServlet**（即<font color=FF0000>**前端控制器**</font>，是所有请求的入口）

- **Controller：**定义请求如何处理
- **xxxResolver：**

  - **ViewResolver：**视图解析器
  - **HandlerExceptionResolver：**异常解析器
  - **MultipartResolver：**
  - **...**
- **HandlerMapping：**请求如何映射到Controller上，请求映射处理的逻辑

#####  补充：SpringMVC中的Servlet

SpringMVC中的Servlet一共有三个层次，分别是HttpServletBean、FrameworkServlet和 DispatcherServlet

- HttpServletBean直接继承自java的HttpServlet，其作用是将Servlet中配置的参数设置到相应的属性
- FrameworkServlet初始化了WebApplicationContext
- <font color=FF0000>DispatcherServlet初始化了自身的9个组件</font>

摘自：[【SpringMVC】9大组件概览](https://blog.csdn.net/hu_zhiting/article/details/73648939)

##### 补充：Spring MVC九大组件

Spring MVC工作的时候，关键位置都是由这九大组件完成的；<font color=FF0000>**这九大组件都是接口**</font>（接口就是规范）

- **HandlerMapping：**Handler映射器

  <font color=FF0000>**用来查找Handler的**</font>，在SpringMVC中会有很多请求，每个请求都需要一个Handler处理，具体接收到一个请求之后使用哪个Handler进行处理，这就时HandlerMapping要做的

  只定义了一个方法，就是<font color=FF0000>通过request找到HandlerExecutionChain</font>，而HandlerExecutionChain包装了一个Handler和一组Interceptors

  ```java
  public interface HandlerMapping {
      HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception;
  }
  ```

- **HandlerAdapter：**Handler适配器

  需要HandlerAdapter是因为Spring MVC没有对Handler做任何规定，它可以是类，可以是方法，也可以是任何其他东西，我们可以看到Handler的类型是Object，这样会非常灵活。但是<font color=FF0000>怎么让任意类型的Handler处理固定格式的请求呢？没错，就是使用适配器，每种Handler都要有对应的HandlerAdapter才能处理请求。</font>

- **HandlerExceptionResolver：**Handler异常解析器

  在处理请求的过程中，难免会出现异常，HandlerExceptionResolver就是专门来处理异常的组件，<font color=FF0000>它能根据异常设置ModelAndView，然后交给render进行渲染</font>

- **ViewResolver：**视图解析器

  <font color=FF0000>ViewResolver是根据viewName查找View</font>，用来将String类型的视图名和本地化信息Locale解析成View类型的视图

- **RequestToViewNameTranslator：**请求到视图的转换器

  ViewName是根据ViewName查找View，<font color=FF0000>但有的Handler处理完后并没有设置View也没有设置ViewName，这时就需要从request获取ViewName了，如何从request中获取ViewName就是RequestToViewNameTranslator要做的事情了</font>。RequestToViewNameTranslator在Spring MVC容器里只可以配置一个，所以所有request到ViewName的转换规则都要在一个Translator里面全部实现。

- **LocaleResolver：**区域信息解析器（与国际化有关）

- **ThemeResolver：**主题解析器（与主题更换相关，<mark>用的很少</mark>）

- **MultipartResolver：**文件上传解析器

- **FlashMapManager：**spring mvc种运行重定向携带数据的功能

部分摘自：[Spring MVC九大组件简介](https://www.jianshu.com/p/a728acd817b7) 和 [【SpringMVC】9大组件概览](https://blog.csdn.net/hu_zhiting/article/details/73648939)

同时，<font color=FF0000>几个组件之间的执行流程是：</font>

handlerMapping --> handlerAdapter --> requestToViewNameResolver  --> handlerExceptionResolver -->  viewResolver --> View

摘自：[从DISPATCHERSERVLET中的DOSERVICE了解SPRING组件之间的处理流程](https://www.cnblogs.com/afraidToForget/p/10102909.html)

**补充：**<mark>DispatcherServlet继承于HttpServlet</mark>，可以说spring mvc是基于Servlet的一个实现，DispatcherServlet负责协调和组织不同组件以完成请求处理并返回响应的工作，实现了MVC模式。

摘自：[跟我一起造轮子 手写springmvc](https://www.jianshu.com/p/fd01beb1749a)

##### Spring MVC工作流程示意图

![Spring MVC工作流程示意图](https://upload-images.jianshu.io/upload_images/11644829-7a9a30a9ec382011.png?imageMogr2/auto-orient/strip|imageView2/2/w/720/format/webp)

<font color=FF0000>**文字描述：**</font>

1. 浏览器发送请求到前端控制器（DispatcherServlet），DispatcherServlet请求处理器映射器（HandlerMappering）去查找处理器（Handler）：通过xml配置或者注解进行查找

2. HandlerMapping匹配到处理该url请求的Controller、Interceptor（根据xml配置、注解进行查找）返回给DispatcherServlet 

3. DispatcherServlet调用Interceptor、Controller进行请求处理，Controller处理结果为ModelAndView返回给DispatcherServlet

4. DispatcherServlet调用ViewResolver渲染ModelAndView为最终的View，最终转为response返回给用户

摘自：[Spring专题2：MVC是如何工作的](https://www.jianshu.com/p/05d7594b70f1)

**上面的另一种说法作为补充：**

1. 客户端发送请求到DispatcherServlet（分发器）

2. 由DispacherServlet控制器查询HanderMapping，找到处理请求的Controller

3. Controller调用业务逻辑处理后，返回ModelAndView

4. DispatcherServlet查询视图解析器，找到ModelAndView指定的视图

5. 视图负责将结果显示到客户端

摘自：[spring boot和SSM开发中有什么区别？ - 东方翌的回答 - 知乎](https://www.zhihu.com/question/284488830/answer/439068110)

有待研究：DispatcherServlet的doServlet方法和doDispatch方法

##### Spring MVC 中的常用注解

- **@Controller**：定义控制器
  - **@RestController**：结合Rest服务的控制器（@RequestBody + @Controller = @RestController）
- **@RequestMapping**：Controller要处理哪些请求
  - **@GetMapping** 处理get请求 / **@PostMapping** 处理post请求
  - **@PutMapping / @DeleteMapping**
- **@RequestBody** 对应请求报文器 / **@ResponseBody** 对应响应报文器 / **@ResponseStatus** 返回这个请求HTTP的响应码

**Controller代码示例如下，使用@Controller**：

```java
//
@Controller
@RequestMapping("/coffee")  //mappping定义在类上，类中所有的mapping都要以它为基础
public class CoffeeController {
    @Autowired
    private CoffeeService coffeeService;

    //mppping定义在函数上，这里相当于/coffee/...
    @GetMapping("/")  
    @ResponseBody  
    //这个函数的返回结果List<Coffee>，作为ResponseBody返回
    //另外，这个注释可以去掉，并把外层的@Controller替换为@RestController，示例见下面一段代码
    public List<Coffee> getAll() {
        return coffeeService.getAllCoffee();
    }
}
```

**RestController代码示例如下，使用@RestController以替换@Controller和@ResponseBody**：

```java
@RestController
@RequestMapping("/order")
@Slf4j
public class CoffeeOrderController {
    @Autowired
    private CoffeeOrderService orderService;
    @Autowired
    private CoffeeService coffeeService;

    @PostMapping("/")
    @ResponseStatus(HttpStatus.CREATED)
    public CoffeeOrder create(@RequestBody NewOrderRequest newOrder) {
        log.info("Receive new Order {}", newOrder);
        Coffee[] coffeeList = coffeeService.getCoffeeByName(newOrder.getItems())
                .toArray(new Coffee[] {});
        return orderService.createOrder(newOrder.getCustomer(), coffeeList);
    }
}
```



#### From 6.2

##### Spring 的应用程序上下文

**关于上下文常用的接口及其实现**

- **BeanFactory**  很少使用
  -  **DefaultListableBeanFactory**
- **ApplicationContext** 更常用
  - **ClassPathXmlApplicationContext**：基于类路径
  - **FileSystemXmlApplicationContext**：基于文件系统
  - **AnnotationConfigApplicationContext**：基于注解的
- **WebApplicationContext**

以上详细内容参见：[理解Spring容器、BeanFactory和ApplicationContext](https://www.jianshu.com/p/2854d8984dfc)



`@AfterReturning("bean(testBean*)")`是拦截所有以testBean为开头的Bean。



#### From 6.3

##### 一个请求的大致处理流程（DispatcherServlet内部）
**绑定一些 Attribute**

- WebApplicationContext / LocaleResolver / ThemeResolver

**处理 Multipart**

- 如果是，则将请求转为 MultipartHttpServletRequest

**Handler 处理**

- 如果找到对应 Handler，执行 Controller 及前后置处理器逻辑

**处理返回的 Model ，呈现视图**

<mark>（这里并没有听懂...）</mark>



#### 视图与视图解析器

请求处理方法执行完成后，最终返回一个 ModelAndView 对象。对于那些返回 String、View 或 ModelMap 等类型的处理方法，<mark>Spring MVC 也会在内部将它们装配成一个 ModelAndView 对象</mark>，该对象包含了视图逻辑名和模型对象的信息。

Spring MVC 借助视图解析器（ViewResolver）得到最终的视图对象（View），这可能是我们常见的 JSP 视图，也可能是一个基于Thymeleaf、FreeMarker、Velocity 模板技术的视图，还可能是 PDF、Excel、XML、JSON 等各种形式的视图。

对于最终究竟采取何种视图对象对模型数据进行渲染，处理器并不关心，处理器的工作重点聚焦在生产模型数据的工作上，从而实现 MVC 的充分解耦。

**视图：**

**视图的作用**是<mark>渲染模型数据，将模型里的数据以某种形式呈现给客户</mark>。视图对象可以是常见的 JSP，还可以是 Excel 或 PDF 等形式不一的媒体形式。为了实现视图模型和具体实现技术的解耦，Spring 在 org.springframework.web.servlet 包中定义了一个高度抽象的 View 接口，该接口中定义了两个方法。

- `String getContentType()`：视图对应的 MIME 类型，如 text/html、Image/jpeg 等。

- `void render(Map model,HttpServletRequest request，HttpServletResponse response)`：将模型数据以某种 MIME 类型渲染出来。

<mark>视图对象是一个 Bean</mark>，通常情况下，<mark>视图对象由视图解析器负责实例化</mark>。由于视图 Bean 是无状态的，所以它们不会有线程安全的问题。

不同类型的视图实现技术对应不同的 View 实现类，这些视图实现类都位于 org.springframework.web.servlet.view 包中。

![不同类型的View实现类](https://img2018.cnblogs.com/blog/1502218/201907/1502218-20190709235651287-1976113765.png)

**视图解析器：**

Spring MVC 为逻辑视图名的解析提供了不同的策略，可以在Spring Web 上下文中配置一种或多种解析策略，并指定它们之间的先后顺序。**<font color=FF0000>每种解析策略对应一个具体的视图解析器实现类</font>**。视图解析器的工作比较单一，即将逻辑视图名解析为一个具体的视图对象。<font  color=FF0000>**所有视图解析器都实现了 ViewResolver 接口**</font>，该接口仅有一个方法。

```java
View resolveViewName(String viewName,Locale locale)
```

resolveViewName() 方法的签名清楚地向我们传达了视图解析器工作的内涵：根据逻辑视图名和本地化对象得到一个视图对象。Spring 拥有众多的视图解析器实现类，通过下图进行概括性说明。

![Spring的视图解析器实现类](https://img2018.cnblogs.com/blog/1502218/201907/1502218-20190710000140302-1452810757.png)

用户可以选择一种视图解析器或混用多种视图解析器，每个视图解析器都实现了Ordered 接口并开放出一个 orderNo 属性，可以通过该属性指定解析器的优先顺序，值越小优先级越高。有些视图解析器默认为最高优先级（如 ContentNegotiatingViewResolver），而有些视图解析器默认为最低优先级（如 InternalResourceViewResoIver、XsltViewResolver 等），具体请参考 API 文档。

Spring MVC 会按照视图解析器的优先级顺序对逻辑视图名进行解析，直到解析成功并返回视图对象，否则将抛出 ServletException 异常。

摘自：[视图和视图解析器概述](https://www.cnblogs.com/jwen1994/p/11161296.html)

**SpringMVC controller的几种返回方式**

SpringMVC 处理方法（Controller）提供了以下几种返回方式：ModelAndView, Model, ModelMap, Map,View, String, void（摘自：[SpringMVC controller的几种返回方式](https://blog.csdn.net/zgs921364401/article/details/81558110)）。但返回值最终都会被SpringMVC统一转为ModelAndView对象并返回；随后Spring就会用ViewResolver，把返回的ModelAndView对象中的View渲染给用户看（即返回给浏览器）

摘自：[SpringMVC--视图](https://www.cnblogs.com/gu-bin/p/10544952.html)

**Spring MVC提供了以下几种途径输出模型数据：**

- <mark style=background-color:fuchsia><font size=4>**ModelAndView：**</font></mark>处理方法返回值类型为ModelAndView时，方法体即可通过该对象添加模型数据

  **详细来说：**使用ModelAndView类用来存储处理完后的结果数据，<font color=FF0000>**并显示该数据的视图**</font>。从名字上看ModelAndView中的Model代表模型，View代表视图，这个名字就很好地解释了该类的作用。<mark>业务处理器调用模型层处理完用户请求后，<font color=FF0000>把结果数据存储在该类的model属性中，把要返回的视图信息存储在该类的view属性中</font>，然后<font color=FF0000>将该ModelAndView</font>返回该Spring MVC框架<font color=FF0000>（即：返回到DispatcherServlet）</font></mark>。框架通过调用配置文件中定义的视图解析器，对该对象进行解析，最后把结果数据显示在指定的页面上。 

  摘自：[SpringMVC之ModelAndView的用法（转）](https://www.cnblogs.com/alsf/p/9134552.html)

  **添加<font color=FF0000>模型数据</font>的方法**

  - `ModelAndView addObject(String attributeName, Object attributeValue);`
  - `ModelAndView addAllObject(Map modelMap);`

  **设置<font color=FF0000>视图</font>的方法**

  - `void setView(View view)`
  - `void setViewName(String viewName);`

  <mark>SpringMVC会把ModelAndView的<font color=FF0000>model中的数据放入到request域对象中，因此可以直接使用request来获取数据</font></mark>

- <mark style=background-color:fuchsia><font size=4>**Map及Model：**</font></mark>入参为`org.springframeword.ui.Model`、`org.springframeword.ui.ModelMap`或`java.uitl.Map`时，处理方法返回时，Map中的数据会自动添加到模型中。

- <mark style=background-color:fuchsia><font size=4>**@SessionAttributes：**</font></mark>将模型中的某个属性暂存到HttpSession中，<mark>以便**多个请求之间可以共享这个属性**</mark>

  **详细来说：**若<font color=FF0000>希望在多个请求之间共用某个模型属性数据</font>，则可以在控制器类上标注一个`@SessionAttributes`，Spring MVC会将在模型中对应的属性暂存到HttpSession中。**有一点需要注意的是**，<font color=FF0000>这个注解只能在类上面标注，而不能在方法上面标注</font>。

  `@SessionAttributes`<mark>除了可以通过**属性名**指定需要放到会话中的属性外</mark>，<font color=FF0000>还可以通过模型属性的**对象类型**指定哪些模型属性需要放到会话中</font>，例如：

  - `@SessionAttributes(types=User.class)`会将隐含模型中所有类型为User.class的属性添加到会话中
  - `@SessionAttributes(value={"user1","user2"})`会将隐含模型中key值为"user1"和"user2"的属性添加到会话中
  - `@SessionAttributes(types={User.class,Dept.class})`会将隐含模型中所有类型为User.class和Dept.class的属性添加到会话中
  - `@SessionAttributes(value={"user1","user2"},types={Dept.class})`会将隐含模型中所有类型为Dept.class和key值为"user1"和"user2"的属性添加到会话中

- <mark style=background-color:fuchsia><font size=4>**@ModelAttribute：**</font></mark>方法入参标注该注解后，入参的对象就会放到数据模型中。

  **关于`@ModelAttribute`解决了什么开发上的问题（痛点）**，可以参考：[Springmvc知识二细节-----@ModelAttribute](https://blog.csdn.net/sinat_28978689/article/details/65627501) 或者Spring MVC视频

  **@ModelAttribute**最主要的作用是<mark>将数据添加到模型对象中，用于视图页面展示时使用</mark>。

  @ModelAttribute等价于 model.addAttribute("attributeName", abc)

  摘自：[@ModelAttribute注解的使用总结](https://blog.csdn.net/leo3070/article/details/81046383)

  被`@ModelAttribute`注解的方法会在Controller的<font color=FF0000>**每个方法执行之前**执行（预加载）</font>

  摘自：[@ModelAttribute运用详解(转)](https://www.jianshu.com/p/bf2e67abe8f7)


摘自：[SpringMVC学习笔记 | SpringMVC中处理模型数据的几种方法（ModelAndView|@SessionAttributes|@ModelAttribute）以及运行原理](https://www.jianshu.com/p/3f47048b01fc)



#### From 6.4

##### 定义映射关系

**@Controller**
**@RequestMapping**

- **path** / **method** 指定映射路径与方法

  - value / path（两者等价）：指定请求的实际地址
  - method 方法：<mark>包含GET / POST / PUT / DELETE / PATCH</mark>（Patch方式是对put方式的一种补充）

- **params** / **headers** 限定映射范围

  - params：指定<mark>request中必须包含某些<font color=FF0000>参数值</font></mark>，才让该方法处理
  - headers：指定<mark>request中必须包含某些指定的header值</mark>，才让该方法处理请求

- **consumes** / **produces** 限定请求与响应格式

  - **consumes**：<font size=4>**指定**</font>处理<font color=FF0000>**请求**的提交内容类型</font>（Context-Type），即：用来限制<mark style=background-color:aqua>**ContentType**</mark>（<mark>ContentType告诉服务器当前发送的数据是什么格式</mark>），例如application/json, text/html

    **示例如下：**

    ```java
    @Controller  
    @RequestMapping(value = "/pets", method = RequestMethod.POST, consumes="application/json")  
    public void addPet(@RequestBody Pet pet, Model model) {      
        // implementation omitted  
    }
    ```

  - **produces**：<font size=4>**指定**</font><font color=FF0000>**返回**的内容类型</font>，即：用来限制<mark style=background-color:aqua>**Accept**</mark>（<mark>**Accept**用来告诉服务器，客户端能认识哪些格式，最好返回这些格式</mark>）；还可以设置返回值的字符编码（charset）

    **示例如下：**

    第一种使用，返回json数据。

    因为我们已经使用了注解<mark>@responseBody就是返回值是json数据，所以下边的代码可以省略produces属性</mark>：

    ```java
    @Controller  
    @RequestMapping(value = "/pets/{petId}", method = RequestMethod.GET, produces="application/json")  
    @ResponseBody  
    public Pet getPet(@PathVariable String petId, Model model) {     
        // implementation omitted  
    }  
    ```

    第二种使用，返回json数据的字符编码为utf-8

    ```java
    @Controller  
    @RequestMapping(value = "/pets/{petId}", produces="MediaType.APPLICATION_JSON_VALUE"+";charset=utf-8")  
    @ResponseBody  
    public Pet getPet(@PathVariable String petId, Model model) {      
        // implementation omitted  
    }  
    ```

  部分摘自：[RequestMapping 中produces 和 consumes](https://www.jianshu.com/p/f78b43f048e6) 和 [produces在@requestMapping中的使用方式和作用](https://blog.csdn.net/jaryle/article/details/72965885)

##### **一些快捷方式**（相当于组合注解）

- **@RestController**：相当于@Controller+@ResponseBody

- **@GetMapping** ：相当于@requestMapping(method = RequestMethod.GET)

  ```java
  @RequestMapping(value = "/get/{id}", method = RequestMethod.GET)
  //可以简化为
  @GetMapping("/get/{id}")
  ```

- **@PostMapping**：相当于@requestMapping(method = RequestMethod.POST)

- **@PutMapping**

- **@DeleteMapping**

- **@PatchMapping**

##### 定义处理理方法
- **@RequestBody** 接收请求正文 / **@ResponseBody** 发送响应正文 / **@ResponseStatus** 响应Http的状态码
- **@PathVariable** 路径变量 / **@RequestParam** 请求参数 / **@RequestHeader** 请求Http的头
- **HttpEntity** / **ResponseEntity**

以上可以参考：[spring文档中的1.3.3 Handler Methods](https://docs.spring.io/spring/docs/5.1.5.RELEASE/spring-framework-reference/web.html#mvc-ann-methods)

##### **@RequestBody**

 **@RequestBody**主要用来<font style=color:red size=4>**接收前端传递给后端**</font>的<font style=color:red>**json字符串**</font>中的<font style=color:red size=4>**数据**</font>并<font color=FF0000>**将其封装为对应的JavaBean**</font>(**<font size=4><font color=FF0000>请求体</font>中的数据</font>**)。封装时使用到的一个对象是系统默认配置的 HttpMessageConverter进行解析，然后封装到形参上。

GET方式无请求体，所以<mark>使用`@RequestBody`接收数据时，前端不能使用GET方式提交数据，而是用POST方式进行提交</mark>。**在后端的<font color=FF0000>同一个接收方法里</font>**，<mark>`@RequestBody`与`@RequestParam`**可以同时使用**</mark>，<mark>`@RequestBody`最多只能有一个，而`@RequestParam()`可以有多个</mark>（即：一个请求，只有一个RequestBody；一个请求，可以有多个RequestParam）。另外：当同时使用`@RequestParam`和`@RequestBody`时，`@RequestParam()`中指定的参数可以是普通元素、数组、集合、对象等；<mark>且RequestBody 接收的是请求体里面的数据；而<font color=FF0000>**RequestParam接收的是key-value里面的参数**</font></mark>

**以上总结：**

如果参数时**放在请求体中**传入后台的话，那么后台要用`@RequestBody`才能接收到；

如果**不是放在请求体中**的话，后台接收前台传过来的参数，要用`@RequestParam`来接收，或者形参前什么也不写也能接收。

如果参数前<font color=FF0000>**写了**</font>`@RequestParam(xxx)`，那么前端<mark>**必须有**对应的xxx名字</mark>才行(不管其是否有值，当然可以通过设置该注解的required属性来调节是否必须传)，<mark>如果没有xxx名的话，那么请求会出错，报400。</mark>

**示例：**

```java
@RequestMapping("mytest")
public String myTestController(@RequestBody String jsonString){}
```

如果参数前<font color=FF0000>**不写**</font>@RequestParam(xxx)的话，那么就前端<mark>**可以有也可以没有**对应的xxx名字</mark>，如果有xxx名的话，那么就会自动匹配；没有的话，请求也能正确发送。

摘自：[@RequestBody的使用](https://blog.csdn.net/justry_deng/article/details/80972817)

##### @ResponseBody

简单地说：@ResponseBody的**作用**其实是**<mark style=color:red>将java对象转为json格式的数据</mark>**。

@responseBody注解的作用是<mark><font color=FF0000>将controller的方法返回的对象</font>通过适当的转换器转换为指定的格式之后，写入到response对象的body区，通常用来返回JSON数据或者是XML数据</mark>。

注意：<mark>在使用此注解之后不会再走视图处理器，而是直接将数据写入到输入流中</mark>，他的效果等同于通过response对象输出指定格式的数据。

@ResponseBody是<mark>作用在方法上</mark>的，@ResponseBody 表示该方法的返回结果直接写入 HTTP response body 中，一般在异步获取数据时使用（也就是AJAX）。

注意：<mark>在使用 @RequestMapping后，返回值通常解析为跳转路径</mark>，但是<mark style=color:red>加上 @ResponseBody 后返回结果不会被解析为跳转路径，而是直接写入 HTTP response body 中</mark>。

摘自：[@ResponseBody详解](https://blog.csdn.net/originations/article/details/89492884)

##### **@ResponseStatus**

@ResponseStatus注解有两种用法，一种是加载自定义异常类上，一种是加在目标方法中

注解中有两个参数：value属性设置异常的状态码（对应枚举HttpStatus的值，此值对应相应404,403,500），reaseon是异常的描述（即是界面提示文字）。示例如下：

```java
@ResponseStatus(value=HttpStatus.INTERNAL_SERVER_ERROR,reason="服务器内部错误")
```

摘自：[springmvc中@ResponseStatus注解使用](https://blog.csdn.net/qq_36722039/article/details/80825117)

##### HttpStatus

**常用HttpStatus状态：**

- HttpStatus.OK = 200
- HttpStatus.CREATED = 201
- HttpStatus.BADREQUEST = 400
- HttpStatus.FORBIDDEN = 403
- HttpStatus.NOTFOUND = 404
- HttpStatus.TIMEOUT = 408
- HttpStatus.SERVERERROR = 500

更多见下面关于http的内容，[点击跳转](#httpStatus)

摘自：[HttpStatus详解](https://blog.csdn.net/csdn1844295154/article/details/78980174)

##### @PathVariable

**@PathVariable**是spring3.0的一个新功能：<mark>接收请求路径中<font color=FF0000>**占位符**</font>的值</mark>

**示例：注意path的用法**

```java
@RequestMapping(path = "/{id}", method = RequestMethod.GET, produces = MediaType.APPLICATION_JSON_UTF8_VALUE)
@ResponseBody
public Coffee getById(@PathVariable("id") Long id) {
	Coffee coffee = coffeeService.getCoffee(id);
    return coffee;
}
```

**示例解释：**通过@PathVariable 可以将URL中占位符参数`{id}`绑定到处理器类的方法形参中`@PathVariable("id")`中

部分摘自：[@PathVariable注解使用](https://blog.csdn.net/sswqzx/article/details/84194979)

##### @RequestParam

在访问各种各样的网站时，经常会发现网站的URL的最后一部分形如：`?xx=yy&zz=ww`。<mark>这就是HTTP协议中的<font color=FF0000>**Request参数**</font></mark>，如：`https://www.zhihu.com/search?type=content&q=web`，这里的type=content&q=web就是搜索请求的参数，不同参数之间用&分隔，每个参数形如name=value的形式，分别表示参数名字和参数值。

**示例：注意parms的用法**

```java
@GetMapping(path = "/", params = "name")
@ResponseBody
public Coffee getByName(@RequestParam String name) {
    return coffeeService.getCoffee(name);
}
```

**示例解释：**当我们访问/user/?name=123时，SpringMVC帮助我们将Request参数name的值绑定到了处理函数的参数name上。这样就能够轻松获取用户输入，并根据它的值进行计算并返回了。

摘自：[@RequestParam和@PathVariable的用法与区别](https://blog.csdn.net/a15028596338/article/details/84976223)

`@RequestParam`：将请求参数绑定到你控制器的方法参数上

语法：

```java
@RequestParam(value=”参数名”,required=”true/false”,defaultValue=””)
```

- value：参数名
- required：是否包含该参数，默认为true，表示该请求路径中必须包含该参数，如果不包含就报错
- defaultValue：默认参数值，如果设置了该值，required=true将失效，自动为false,如果没有传该参数，就使用默认值

摘自：[@RequestParam注解使用](https://blog.csdn.net/sswqzx/article/details/84195043)

**补充：**很多时候，需要对URL变量进行更加精确的定义，例如-用户名只可能包含小写字母，数字，下划线，这时除了简单地定义{username}变量，<mark>还可以定义<font color=FF0000>**正则表达式**</font>进行更精确的控制</mark>

摘自：[SpringBoot-@PathVariable](https://www.cnblogs.com/williamjie/p/9139548.html)

##### @RequestParam和@PathVariable<font color=FF0000>相同与区别</font>

 @RequestParam和@PathVariable都能够完成类似的功能——因为本质上，它们都是用户的输入，只不过输入的部分不同，一个在URL路径部分，另一个在参数部分。要访问一篇博客文章，这两种URL设计都是可以的：

- 通过**@PathVariable**，例如/blogs/1
- 通过**@RequestParam**，例如blogs?blogId=1

<font color=FF0000>即：`/book/[{user}(PathVariable)]?[user=admin(RequestParam)]`</font>

那么究竟应该选择哪一种呢？建议：

- 当URL指向的是<mark>某一具体业务资源（或资源列表）</mark>，例如博客（博客ID），用户（用户ID）时，使用**@PathVariable**
- 当URL需要<mark>对资源或者资源列表进行过滤</mark>，筛选时，用**@RequestParam**

例如我们会这样设计URL：

- /blogs/{blogId}
- /blogs?state=publish而不是/blogs/state/publish来表示处于发布状态的博客文章

摘自：[@RequestParam和@PathVariable的用法与区别](https://blog.csdn.net/a15028596338/article/details/84976223)

##### @RequestHeader

**@RequestHeader**注解用于<mark>将**<font color=FF0000 size=4>请求头信息</font>数据**<font color=FF0000>映射</font>到请求处理方法的**形式参数**上</mark>

**使用@RequestHeader注解属性如下：**

|      属性      |   类型    | 是否必要 |              说明              |
| :------------: | :-------: | :------: | :----------------------------: |
|     `name`     | `String`  |    否    |     指定请求参数绑定的名称     |
|    `value`     | `String`  |    否    |        `name`属性的别名        |
|   `required`   | `boolean` |    否    |      指示参数是否必须绑定      |
| `defaultValue` | `String`  |    否    | 如果没有传递参数而使用的默认值 |

摘自：[3.7 @RequestHeader注解](https://www.dazhuanlan.com/2019/10/11/5d9fa9ed195b5/)

##### HttpEntity

**Spring Web将类`HttpEntity`包装一个HTTP请求或响应的以下内容 : <font color=FF0000>头部和消息体。</font>**

<font color=FF0000>**从概念上来讲**</font>，可以简单理解成这样 ：`1 HttpEntity = n headers + 1 body`

<font color=FF0000>**从具体实现上来讲**</font>，可以理解成这样 : `1 HttpEntity = n HttpHeader(s) + 1 T` ；这里 T 是泛型类型，指消息体的类型

**Spring Web针对<font color=FF0000>请求</font>和<font color=FF0000>响应</font>两种情况<font color=FF0000>分别继承HttpEntity</font>提供了实现类 :**

**<font color=FF0000>Request</font>Entity：**`RequestEntity = HttpEntity + HttpMethod + URI + Type(消息体类型)`

**<font color=FF0000>Response</font>Entity：**`ResponseEntity = HttpEntity + (HttpStatus/Intgeger类型) 状态码`

摘自：[Spring Web : 概念模型 HttpEntity](https://blog.csdn.net/andy_zhang2007/article/details/100193873)

##### RequestEntity

无相关介绍

##### ResponseEntity

ResponseEntity可以定义返回的HttpStatus（状态码）和HttpHeaders（消息头：请求头和响应头）

ResponseEntity ：标识整个http响应：状态码、头部信息、响应体内容(spring)
**@ResponseBody：**加在请求处理方法上，能够处理方法结果值作为http响应体（springmvc）
**@ResponseStatus：**加在方法上、返回自定义http状态码(spring)

摘自：[spring中的ResponseEntity理解](https://www.cnblogs.com/flypig666/p/11729757.html)

##### @CookieValue

@CookieValue用于获取某个cookie的值

##### HTTP

**简介：**HTTP协议是Hyper Text Transfer Protocol（超文本传输协议）的缩写，是用于从万维网（WWW: World Wide Web ）服务器传输超文本到本地浏览器的传送协议。

HTTP是一个<font color=FF0000>基于TCP/IP通信协议</font>来传递数据（HTML 文件, 图片文件, 查询结果等）

一个HTTP<font color=FF0000>**"客户端"**是一个应用程序</font>（Web浏览器或其他任何客户端），通过连接到服务器达到向服务器发送一个或多个HTTP的请求的目的。

一个HTTP<font color=FF0000>**"服务器"**同样也是一个应用程序</font>（通常是一个Web服务，如Apache Web服务器或IIS服务器等），通过接收客户端的请求并向客户端发送HTTP响应数据。

HTTP<mark>使用<font color=FF0000>**统一资源标识符**</font>（Uniform Resource Identifiers, URI）来传输数据和建立连接</mark>。一旦建立连接后，数据消息就通过类似Internet邮件所使用的格式[RFC5322]和多用途Internet邮件扩展（MIME）[RFC2045]来传送。

**工作原理：**HTTP协议<font oolor=FF0000>工作于客户端-服务端架构（C/S架构）</font>上。浏览器作为HTTP客户端通过URL向HTTP服务端即WEB服务器发送所有请求。HTTP<font color=FF0000>默认端口号为80</font>，但是你也可以改为8080或者其他端口。

**三点注意事项：**

- **HTTP是<font color=FF0000>无连接</font>：**无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
- **HTTP是媒体独立的：**这意味着，只要客户端和服务器知道如何处理的数据内容，<font color=FF0000>任何类型的数据都可以通过HTTP发送。**客户端以及服务器指定使用适合的MIME-type内容类型**。</font>
- **HTTP是<font color=FF0000>无状态</font>的：**HTTP协议是无状态协议。无状态是指协议对于事务处理<font color=FF0000>没有记忆能力</font>。<font color=FF0000>缺少状态意味着如果后续处理需要前面的信息，**则它必须重传**</font>，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

**以下图表展示了HTTP协议通信流程：**

![HTTP协议通信流程](https://www.runoob.com/wp-content/uploads/2013/11/cgiarch.gif)

**<font color=FF0000>客户端请求</font>信息结构**

客户端发送一个HTTP请求到服务器的请求消息包括以下格式：<font color=FF0000>请求行（request line）</font>、<font color=FF0000>请求头部（header）</font>、<font color=FF0000>空行</font>和<font color=FF0000>请求数据</font>四个部分组成，请求报文的一般格式如下所示：

![客户端请求消息](https://www.runoob.com/wp-content/uploads/2013/11/2012072810301161.png)

客户端请求示例：

```http
GET /hello.txt HTTP/1.1
User-Agent: curl/7.16.3 libcurl/7.16.3 OpenSSL/0.9.7l zlib/1.2.3
Host: www.example.com
Accept-Language: en, mi
```

**<font color=FF0000>服务器响应</font>消息结构**

HTTP响应也由四个部分组成，分别是：<font color=FF0000>状态行</font>、<font color=FF0000>消息报头</font>、<font color=FF0000>空行</font>和<font color=FF0000>响应正文</font>

![服务器响应消息](https://www.runoob.com/wp-content/uploads/2013/11/httpmessage.jpg)

服务端响应示例：

```http
HTTP/1.1 200 OK
Date: Mon, 27 Jul 2009 12:28:53 GMT
Server: Apache
Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT
ETag: "34aa387-d-1568eb00"
Accept-Ranges: bytes
Content-Length: 51
Vary: Accept-Encoding
Content-Type: text/plain
```

**http请求方法**

根据 HTTP 标准，HTTP 请求可以使用多种请求方法。

HTTP1.0 定义了三种请求方法： GET, POST 和 HEAD方法。

HTTP1.1 新增了六种请求方法：OPTIONS、PUT、PATCH、DELETE、TRACE 和 CONNECT 方法。

| 序号 |  方法   |                             描述                             |
| :--: | :-----: | :----------------------------------------------------------: |
|  1   |   GET   | 请求**<font color=FF0000>指定的页面**信息</font>，并<font color=FF0000>**返回实体主体**</font>。 |
|  2   |  HEAD   | <mark>类似于 GET 请求，只不过返回的响应中没有具体的内容</mark>，<font color=FF0000>**用于获取报头**</font> |
|  3   |  POST   | <font color=FF0000>向**指定资源**提交数据进行处理请求</font>（例如提交表单或者上传文件）。<font color=FF0000>**数据被包含在请求体中**</font>。POST 请求可能会导致新的资源的建立和/或已有资源的修改。 |
|  4   |   PUT   |       从客户端向服务器传送的数据取代指定的文档的内容。       |
|  5   | DELETE  |                  请求服务器删除指定的页面。                  |
|  6   | CONNECT |  HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器。   |
|  7   | OPTIONS |                 允许客户端查看服务器的性能。                 |
|  8   |  TRACE  |          回显服务器收到的请求，主要用于测试或诊断。          |
|  9   |  PATCH  |      是对 PUT 方法的补充，用来对已知资源进行局部更新 。      |

**HTTP 响应头信息**

HTTP请求头提供了关于请求，响应或者其他的发送实体的信息。

|                 应答头                 |                             说明                             |
| :------------------------------------: | :----------------------------------------------------------: |
|    <font color=FF0000>Allow</font>     |    <mark>服务器支持哪些请求方法（如GET、POST等）</mark>。    |
|            Content-Encoding            | 文档的编码（Encode）方法。只有在解码之后才可以得到Content-Type头指定的内容类型。利用gzip压缩文档能够显著地减少HTML文档的下载时间。Java的GZIPOutputStream可以很方便地进行gzip压缩，但只有Unix上的Netscape和Windows上的IE 4、IE 5才支持它。因此，Servlet应该通过查看Accept-Encoding头（即request.getHeader("Accept-Encoding")）检查浏览器是否支持gzip，为支持gzip的浏览器返回经gzip压缩的HTML页面，为其他浏览器返回普通页面。 |
|             Content-Length             | 表示内容长度。只有当浏览器使用持久HTTP连接时才需要这个数据。如果你想要利用持久连接的优势，可以把输出文档写入 ByteArrayOutputStream，完成后查看其大小，然后把该值放入Content-Length头，最后通过byteArrayStream.writeTo(response.getOutputStream()发送内容。 |
| <font color=FF0000>Content-Type</font> | 表示后面的文档属于什么MIME类型。Servlet默认为text/plain，但通常需要显式地指定为text/html。由于经常要设置Content-Type，因此HttpServletResponse提供了一个专用的方法setContentType。 |
|                  Date                  | 当前的GMT时间。你可以用setDateHeader来设置这个头以避免转换时间格式的麻烦。 |
|                Expires                 |       应该在什么时候认为文档已经过期，从而不再缓存它？       |
|             Last-Modified              | 文档的最后改动时间。客户可以通过If-Modified-Since请求头提供一个日期，该请求将被视为一个条件GET，只有改动时间迟于指定时间的文档才会返回，否则返回一个304（Not Modified）状态。Last-Modified也可用setDateHeader方法来设置。 |
|                Location                | 表示客户应当到哪里去提取文档。Location通常不是直接设置的，而是通过HttpServletResponse的sendRedirect方法，该方法同时设置状态代码为302。 |
|                Refresh                 | 表示浏览器应该在多少时间之后刷新文档，以秒计。除了刷新当前文档之外，你还可以通过setHeader("Refresh", "5; URL=http://host/path")让浏览器读取指定的页面。 注意这种功能通常是通过设置HTML页面HEAD区的＜META HTTP-EQUIV="Refresh" CONTENT="5;URL=http://host/path"＞实现，这是因为，自动刷新或重定向对于那些不能使用CGI或Servlet的HTML编写者十分重要。但是，对于Servlet来说，直接设置Refresh头更加方便。  注意Refresh的意义是"N秒之后刷新本页面或访问指定页面"，而不是"每隔N秒刷新本页面或访问指定页面"。因此，连续刷新要求每次都发送一个Refresh头，而发送204状态代码则可以阻止浏览器继续刷新，不管是使用Refresh头还是＜META HTTP-EQUIV="Refresh" ...＞。  注意Refresh头不属于HTTP 1.1正式规范的一部分，而是一个扩展，但Netscape和IE都支持它。 |
|                 Server                 | 服务器名字。Servlet一般不设置这个值，而是由Web服务器自己设置。 |
|               Set-Cookie               | 设置和页面关联的Cookie。Servlet不应使用response.setHeader("Set-Cookie", ...)，而是应使用HttpServletResponse提供的专用方法addCookie。参见下文有关Cookie设置的讨论。 |
|            WWW-Authenticate            | 客户应该在Authorization头中提供什么类型的授权信息？在包含401（Unauthorized）状态行的应答中这个头是必需的。例如，response.setHeader("WWW-Authenticate", "BASIC realm=＼"executives＼"")。 注意Servlet一般不进行这方面的处理，而是让Web服务器的专门机制来控制受密码保护页面的访问（例如.htaccess）。 |

<span id="httpStatus">**http状态码**</span>

```json
HttpStatus = {  
        //Informational 1xx  信息
        '100' : 'Continue',  //继续
        '101' : 'Switching Protocols',  //交换协议
 
        //Successful 2xx  成功
        '200' : 'OK',  //OK
        '201' : 'Created',  //创建
        '202' : 'Accepted',  //已接受
        '203' : 'Non-Authoritative Information',  //非权威信息
        '204' : 'No Content',  //没有内容
        '205' : 'Reset Content',  //重置内容
        '206' : 'Partial Content',  //部分内容
 
        //Redirection 3xx  重定向
        '300' : 'Multiple Choices',  //多种选择
        '301' : 'Moved Permanently',  //永久移动
        '302' : 'Found',  //找到
        '303' : 'See Other',  //参见其他
        '304' : 'Not Modified',  //未修改
        '305' : 'Use Proxy',  //使用代理
        '306' : 'Unused',  //未使用
        '307' : 'Temporary Redirect',  //暂时重定向
 
        //Client Error 4xx  客户端错误
        '400' : 'Bad Request',  //错误的请求
        '401' : 'Unauthorized',  //未经授权
        '402' : 'Payment Required',  //付费请求
        '403' : 'Forbidden',  //禁止
        '404' : 'Not Found',  //没有找到
        '405' : 'Method Not Allowed',  //方法不允许
        '406' : 'Not Acceptable',  //不可接受
        '407' : 'Proxy Authentication Required',  //需要代理身份验证
        '408' : 'Request Timeout',  //请求超时
        '409' : 'Conflict',  //指令冲突
        '410' : 'Gone',  //文档永久地离开了指定的位置
        '411' : 'Length Required',  //需要Content-Length头请求
        '412' : 'Precondition Failed',  //前提条件失败
        '413' : 'Request Entity Too Large',  //请求实体太大
        '414' : 'Request-URI Too Long',  //请求URI太长
        '415' : 'Unsupported Media Type',  //不支持的媒体类型
        '416' : 'Requested Range Not Satisfiable',  //请求的范围不可满足
        '417' : 'Expectation Failed',  //期望失败
 
        //Server Error 5xx  服务器错误
        '500' : 'Internal Server Error',  //内部服务器错误
        '501' : 'Not Implemented',  //未实现
        '502' : 'Bad Gateway',  //错误的网关
        '503' : 'Service Unavailable',  //服务不可用
        '504' : 'Gateway Timeout',  //网关超时
        '505' : 'HTTP Version Not Supported'  //HTTP版本不支持
};  
```

**HTTP content-type**

Content-Type（内容类型），一般是指网页中存在的 Content-Type，<font color=FF0000>用于定义网络文件的类型和网页的编码，决定浏览器将以什么形式、什么编码读取这个文件</font>。Content-Type 标头告诉客户端实际返回的内容的内容类型。

**语法示例：**

```http
Content-Type: text/html; charset=utf-8
Content-Type: multipart/form-data; boundary=something
```

**常见的媒体格式类型如下：**

- text/html ： HTML格式
- text/plain ：纯文本格式
- text/xml ： XML格式
- image/gif ：gif图片格式
- image/jpeg ：jpg图片格式
- image/png：png图片格式

**以application开头的媒体格式类型：**

- application/xhtml+xml ：XHTML格式
- application/xml： XML数据格式
- application/atom+xml ：Atom XML聚合格式
- application/json： JSON数据格式
- application/pdf：pdf格式
- application/msword ： Word文档格式
- application/octet-stream ： 二进制流数据（如常见的文件下载）
- application/x-www-form-urlencoded ：\<form encType="">中默认的encType，form表单数据被编码为key/value格式发送到服务器（表单默认的提交数据的格式）

另外一种常见的媒体格式是上传文件之时使用的：

- multipart/form-data ： 需要在表单中进行文件上传时，就需要使用该格式

以上摘自：[RUNOOB.com - HTTP教程](https://www.runoob.com/http/http-tutorial.html)

**HTTP请求GET和POST的区别**

1. <font color=FF0000>**GET提交：**</font><mark>请求的数据会附在URL之后</mark>（就是把数据放置在<mark style=color:red>HTTP协议头**＜request-line 请求行＞**</mark>中），以?分割URL和传输数据，多个参数用&连接；如果数据是英文字母/数字，原样发送，如果是空格，转换为+，如果是中文/其他字符，则直接把字符串用BASE64加密，得出如： %E4%BD%A0%E5%A5%BD，其中％XX中的XX为该符号以16进制表示的ASCII。

   <font color=FF0000>**POST提交：**</font><mark>把提交的数据放置在是<font color=FF0000>HTTP包的包体＜request-body＞中</font></mark>。上文示例中红色字体标明的就是实际的传输数据

    因此，GET提交的数据会在地址栏中显示出来，而POST提交，地址栏不会改变

2. <font color=FF0000>**传输数据的大小：**</font>

   首先声明，HTTP协议没有对传输的数据大小进行限制，HTTP协议规范也没有对URL长度进行限制。 而在实际开发中存在的限制主要有：

   - **GET：**<mark>特定浏览器和服务器对URL长度有限制</mark>，例如IE对URL长度的限制是2083字节(2K+35)。<mark>**对于其他浏览器**，如Netscape、FireFox等，理论上没有长度限制，其限制取决于操作系统的支持</mark>。因此对于GET提交时，传输数据就会受到URL长度的限制。

   - **POST：**<mark>由于不是通过URL传值，<font color=FF0000>理论上数据不受限</font></mark>。但实际各个WEB服务器会规定对post提交数据大小进行限制，Apache、IIS6都有各自的配置。

3. <font color=FF0000>**安全性：**</font>

   <font color=FF0000>POST的安全性要比GET的安全性高</font>。注意：这里所说的安全性和上面GET提到的“安全”不是同个概念。上面“安全”的含义仅仅是不作数据修改，而这里安全的含义是真正的Security的含义，<mark>比如：通过GET提交数据，用户名和密码将明文出现在URL上</mark>，因为登录页面有可能被浏览器缓存， 其他人查看浏览器的历史纪录，那么别人就可以拿到你的账号和密码了

摘自：[Http请求头和响应头(Get和Post)](https://www.cnblogs.com/lauhp/p/8979393.html)

补充阅读：[http请求头及其作用](https://blog.csdn.net/qq_42820268/article/details/82424353)



#### From 6.5

##### 定义类型转换

**自己实现 WebMvcConfigurer**

- Spring Boot 在 WebMvcAutoConfiguration 中实现了了一个
- 添加自定义的 Converter
- 添加自定义的 Formatter

##### 定义校验
- <font color=FF0000 size=4>**通过 Validator 对绑定结果进行校验**</font>
  
  - Hibernate Validator
  
- <font color=FF0000 size=4>**@Valid 注解**</font>

  **主要用途：**主要用于表单验证,减轻了代码量

  **依赖：**`org.springframework.boot:spring-boot-starter-web`

  **相关注解：**

  - @Null   限制只能为null
  - @NotNull    限制必须不为null
  - @AssertFalse    限制必须为false
  - @AssertTrue 限制必须为true
  - @DecimalMax(value)  限制必须为一个不大于指定值的数字
  - @DecimalMin(value)  限制必须为一个不小于指定值的数字
  - @Digits(integer,fraction)   限制必须为一个小数，且整数部分的位数不能超过integer，小数部分的位数不能超过fraction
  - @Future 限制必须是一个将来的日期
  - @Max(value) 限制必须为一个不大于指定值的数字
  - @Min(value) 限制必须为一个不小于指定值的数字
  - @Past   限制必须是一个过去的日期
  - @Pattern(value) 限制必须符合指定的正则表达式
  - @Size(max,min)  限制字符长度必须在min到max之间
  - @Past   验证注解的元素值（日期类型）比当前时间早
  - @NotEmpty   验证注解的元素值不为null且不为空（字符串长度不为0、集合大小不为0）
  - @NotBlank   验证注解的元素值不为空（不为null、去除首位空格后长度为0），不同于@NotEmpty
  - @NotBlank只应用于字符串且在比较时会去除字符串的空格
  - @Email  验证注解的元素值是Email，也可以通过正则表达式和flag指定自定义的email格式

  摘自：[@Valid介绍及相关注解](https://www.jianshu.com/p/c8686fa5ef63)

  ##### @Valid和@Validated的区别

  @Validated：属于spring下，用在类型、方法和方法参数上；但<font color=FF0000>**不能**</font>用于<mark>成员属性</mark>（字段 field）；<mark>提供分组功能</mark>

  @Valid：属于javax下，可以用在方法、构造函数、方法参数和<mark>成员属性</mark>（字段 field）上；<mark>**没有**分组功能</mark>

  **而两者是否能用于<mark>成员属性</mark>（字段）上直接影响能否提供嵌套验证的功能。**

  总结：@Valid属于javax下的，而@Validated属于spring下；@Valid支持嵌套校验、而@Validated不支持，@Validated支持分组，而@Valid不支持。

  摘自：[@Validated和@Valid区别：Spring validation验证框架对入参实体进行嵌套验证必须在相应属性（字段）加上@Valid而不是@Validated](https://blog.csdn.net/qq_27680317/article/details/79970590)以及[Springboot @Validated和@Valid的区别 及使用](https://blog.csdn.net/herojuice/article/details/86020101)

- <font color=FF0000 size=4>**BindingResult**</font>

  BindingResult代表数据绑定的结果，继承了 Errors 接口。而Errors是存储和暴露关于数据绑定错误和验证错误相关信息的接口，提供了相关存储和获取错误消息的方法

  摘自：[详解SpringMVC中的Errors和BindingResult数据验证](https://www.lisa33xiaoq.net/552.html)

   BindingResult和@Valid是一 一对应的，如果有多个@Valid，那么每个@Valid后面都需要<mark>添加BindingResult用于接收bean中的校验信息</mark>

  摘自：[Springboot 使用BindingResult校验参数](https://blog.csdn.net/lihua5419/article/details/83418043)

##### Multipart 上传（文件上传）

- 配置 MultipartResolver
  - Spring Boot 自动配置 MultipartAutoConfiguration
- 支持类型 multipart/form-data
- MultipartFile 类型

**解释：**

一般的提交简单的文本格式的数据，基于文本的表单提交可以满足要求，但是**<mark>对于传输视频和照片<font color=FF0000>二进制文件</font>，就不行了</mark>**。multipart<mark>可以将表单拆分成多个部分</mark>，在一般表单输入域中，它会是基于文本型的数据。如果是上传文件可以对应为二进制。

Multipart/form-data是建立在HTTP的<mark>**POST请求方式**</mark>以上的请求，其一般用于HTTP文件上传。
所以我们需要在表单(form)元素中如下设置，使得该表单请求用于处理文件：

```html
 <form class="" action="" method="post" enctype="multipart/form-data">
```

摘自：[SpringMVC处理Multipart数据](https://www.jianshu.com/p/c74049afaee2)



#### From 6.6

##### 视图解析的实现基础

**视图解析通过ViewResolver 与 View 接口实现**

- AbstractCachingViewResolver
- UrlBasedViewResolver
- FreeMarkerViewResolver
- ContentNegotiatingViewResolver
- InternalResourceViewResolver

**DispatcherServlet 中的视图解析逻辑**

- initStrategies()
  - initViewResolvers() 初始化了对应 ViewResolver
- doDispatch()
  - processDispatchResult()
    - 没有返回视图的话，尝试 RequestToViewNameTranslator
    - resolveViewName() 解析 View 对象



#### From 6.7

##### DispatcherServlet 中的视图解析逻辑

**使用 @ResponseBody 的情况**

- 在 HandlerAdapter.handle() 的中完成了 Response 输出
  - RequestMappingHandlerAdapter.invokeHandlerMethod()
    - HandlerMethodReturnValueHandlerComposite.handleReturnValue()
      - RequestResponseBodyMethodProcessor.handleReturnValue()

##### 重定向

**两种不同的重定向前缀**

- `redirect:`：<font color=FF0000>到目标地址的**重定向**的操作</font>。相当于http 302的跳转，是客户端发起的跳转（浏览器的url发生了跳转），同时会丢失request中的一些信息导致无法获取上一个request的信息；另外由于是集群化部署，上一个请求和当前请求发往的未必是同一个服务器，所以可以认为：redirect会丢失上一个request的信息
- `forward:`：<font color=FF0000>到目标地址的**转发**操作</font>。是在服务端发起的跳转（浏览器的url未发生跳转）



#### From 6.8

##### Spring MVC 中的常用视图

参见文档：[1.9. View Technologies](https://docs.spring.io/spring/docs/5.1.5.RELEASE/spring-framework-reference/web.html#mvc-view)

##### Jackson

Java生态圈中有很多<mark>处理JSON和XML格式化的类库</mark>，Jackson是其中比较著名的一个。虽然 JDK 自带了XML处理类库，但是相对来说比较低级，使用Jackson等高级类库处理起来会方便很多。

Jackson类库包含了很多注解，可以让我们快速建立Java类与JSON之间的关系。详细文档可以参考[Jackson-Annotations](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

摘自：[Jackson快速入门](https://www.jianshu.com/p/f3fc3bf6dd2b)，<mark>关于Jackson的使用其中有很多介绍</mark>

Jackson中有**ObjectMapper**类非常重要，可以<mark>将java对象序列化为json</mark>以及<mark>将json字符串反序列化为java对象</mark>

可以参考阅读：[介绍 Jackson ObjectMapper](https://blog.csdn.net/neweastsun/article/details/87940257)

##### Spring Boot 对 Jackson 的支持

- JacksonAutoConfiguration
  - Spring Boot 通过 @JsonComponent 注册 JSON 序列化组件
  -  Jackson2ObjectMapperBuilderCustomizer
- JacksonHttpMessageConvertersConfiguration
  - 增加 jackson-dataformat-xml 以支持 XML 序列列化



#### From 6.9

##### 模板引擎

在做前端开发项目的时候，有时候经常需要根据后端返回的json数据，然后来生成html，再渲染页面。传统技术非常麻烦（比如JSP，<mark>JSP也是一种模板引擎</mark>），这时候就出现了模板引擎

摘自：[为什么会用到模板引擎？模板引擎的使用简析](https://blog.csdn.net/weixin_43924228/article/details/86724134)

##### Thymeleaf 简介

Thymeleaf 是一个跟 Velocity、FreeMarker 类似的模板引擎，<mark>它可以完全替代 JSP</mark> 。相较与其他的模板引擎，它有如下三个极吸引人的特点：

- Thymeleaf <mark>在有网络和无网络的环境下皆可运行</mark>，即它可以让美工在浏览器查看页面的静态效果，也可以让程序员在服务器查看带数据的动态页面效果。这是由于它支持 html 原型，然后在 html 标签里增加额外的属性来达到模板+数据的展示方式。浏览器解释 html 时会忽略未定义的标签属性，所以 thymeleaf 的模板可以静态地运行；当有数据返回到页面时，Thymeleaf 标签会动态地替换掉静态内容，使页面动态显示。

- Thymeleaf 开箱即用的特性。它提供标准和spring标准两种方言，<mark>可以直接套用模板实现 JSTL、 OGNL表达式效果</mark>，避免每天套模板，改jstl、改标签的困扰。同时开发人员也可以扩展和创建自定义的方言。

- Thymeleaf 提供spring标准方言和一个与 SpringMVC 完美集成的可选模块，可以快速的实现表单绑定、属性编辑器、国际化等功能。

摘自：[Thymeleaf-模板引擎](https://www.cnblogs.com/Zender/p/8709031.html)

##### 使用 Thymeleaf

**添加 Thymeleaf 依赖**

- org.springframework.boot:spring-boot-starter-thymeleaf

**Spring Boot 的自动配置**

- ThymeleafAutoConfiguration
  - ThymeleafViewResolver

##### Thymeleaf 的一些默认配置

```properties
spring.thymeleaf.cache=true                     #开启缓存
spring.thymeleaf.check-template=true            #校验模板
spring.thymeleaf.check-template-location=true   #检查模板位置
spring.thymeleaf.enabled=true                   
spring.thymeleaf.encoding=UTF-8
spring.thymeleaf.mode=HTML
spring.thymeleaf.servlet.content-type=text/html #引擎加工出来的都是html
spring.thymeleaf.prefix=classpath:/templates/   #指定模板的前缀
spring.thymeleaf.suffix=.html                   #指定模板后缀
```



#### From 6.10

##### Spring Boot 中的静态资源配置

**核心逻辑**

- WebMvcConfigurer.addResourceHandlers()

**常用配置**

- spring.mvc.static-path-pattern=/**
- spring.resources.static-locations=classpath:/META-INF/
  resources/,classpath:/resources/,classpath:/static/,classpath:/public/

#### <font color=FF0000>补充：</font>SpringBoot 访问web中的静态资源

**<font color=FF0000>总体来讲</font> SpringBoot 访问web中的静态资源，有<font color=FF0000>两种方式：</font>**

- **classpath 类目录 (src/mian/resource)**。classpath 即 WEB-INF 下面的 classes 目录 ，在 SpringBoot  项目中是 src/main/resource 目录。

- **ServletContext 根目录下(src/main/webapp)**

<font color=FF0000>**展开来讲：**</font>SpringBoot<mark>默认指定了一些固定的目录结构</mark>，静态资源放到这些目录中的某一个，系统运行后浏览器就可以访问到。在 <font color=FF0000>**WebMvcAutoConfiguration 类**</font>（`\org\springframework\boot\autoconfigure\web\servlet\WebMvcAutoConfiguration.class`）中可以看到 SpringMVC自动化配置的相关内容，找到静态资源拦截的配置信息的源码。

**1、SpringBoot 默认指定的可以存放静态资源的目录有哪些**

- classpath:/META-INF/resources/    ## 需创建/META-INF/resources/ 目录

- classpath:/resources/                       ## 需创建/resources/目录

- classpath:/static/                               ## 工具自动生成的static目录，也是用的最多的目录

- classpath:/public/                              ## 需创建/public/ 目录

- src/main/webapp/                             ## 需创建/webapp/ 目录

**2、在全局配置文件中修改这些默认的目录**

**方式一：配置文件修改**

- YAML 文件：

```yaml
server:
  port: 80
spring:
  resources:
    static-locations:
      - classpath:resources
      - classpath:static
```

- properties 文件 

```properties
server.port=80
spring.resources.static-locations=classpath:resources,classpath:static
```

**方式二：配置类修改**

```java
@Configuration
public class WebMVCConfig implements WebMvcConfigurer {
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/**")
            .addResourceLocations("classpath:/static/","classpath:/aa");
    }
}
```

**3、SpringBoot <font color=FF0000>默认的首页</font>是放在任一个静态资源目录下的index.html** ，实现是在WebMvcAutoConfiguraton类的welcomePageHandlerMapping方法，如下：

```java
public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext){//...}
```

**4、SpringBoot <font color=FF0000>默认的web页面图标</font>是放在任一静态资源目录下的favicon.ico**，实现是在WebMvcAutoConfiguraton类的faviconHandlerMapping方法，如下：

```java
public SimpleUrlHandlerMapping faviconHandlerMapping() {//...}
```

摘自：[SpringBoot 访问web中的静态资源](https://blog.csdn.net/qq_42402854/article/details/90295079)

##### 补充：自定义静态资源访问

静态资源路径是指系统可以直接访问的路径，且路径下的所有文件均可被用户直接读取。

在Springboot中默认的静态资源路径有：classpath:/META-INF/resources/，classpath:/resources/，classpath:/static/，classpath:/public/ <mark>（Spring Boot 默认会挨个从 public resources static 里面找是否存在相应的资源，如果有则直接返回）</mark>，从这里可以看出这里的静态资源路径都是在classpath中（也就是在项目路径下指定的这几个文件夹）

<font color=FF0000>**试想这样一种情况：**</font>一个网站有文件上传文件的功能，如果被上传的文件放在上述的那些文件夹中会有怎样的后果？

- 网站数据与程序代码不能有效分离；
- 当项目被打包成一个.jar文件部署时，再将上传的文件放到这个.jar文件中是有多么低的效率；
- 网站数据的备份将会很痛苦。

此时可能<font color=FF0000>最佳的解决办法</font>是<mark>将静态资源路径设置到磁盘的基本个目录</mark>。有两种方式：

- 第一种方法：配置类

  ```java
  @Configuration
  public class WebMvcConfig extends WebMvcConfigurerAdapter {
  
      @Override
      public void addResourceHandlers(ResourceHandlerRegistry registry) {
          //将所有C:/Users/gzpost05/Desktop/springboot博客/ 访问都映射到/myTest/** 路径下
          registry.addResourceHandler("/myTest/**")
              .addResourceLocations("file:C:/Users/gzpost05/Desktop/springboot博客/");
      }
  }
  ```

- **第二种方法：配置文件（配置`application.properties`）**

  ```properties
  #web.upload-path：这个属于自定义的属性，指定了一个路径，注意要以/结尾；
  web.upload-path=C:/Users/gzpost05/Desktop/test/
  #表示所有的访问都经过静态资源路径；
  spring.mvc.static-path-pattern=/**
  #spring.resources.static-locations：在这里配置静态资源路径，前面说了这里的配置是覆盖默认配置，所以需要将默认的也加上否则static、public等这些路径将不能被当作静态资源路径，在这个最末尾的file:${web.upload-path}之所有要加file:是因为指定的是一个具体的硬盘路径，其他的使用classpath指的是系统环境变量
  spring.resources.static-locations=classpath:/META-INF/resources/,classpath:/resources/,\
    classpath:/static/,classpath:/public/,file:${web.upload-path}
  ```

摘自：[玩转springboot：默认静态资源和自定义静态资源实战](https://blog.csdn.net/sihai12345/article/details/81205359)

##### 补充：webjars

SpringBoot 使用 Webjars 统一管理静态资源，以jar包的方式引入静态资源（官网是：[webjars.org](www.webjars.org)）

**推荐使用Webjars的三大理由:**

- 将静态资源版本化，更利于升级和维护。
- 剥离静态资源，提高编译速度和打包效率。
- 实现资源共享，有利于统一前端开发。

**示例：**

```xml
<dependency>
	<groupId>org.webjars</groupId>
	<artifactId>jquery</artifactId>
	<version>3.3.1</version>
</dependency>
```

摘自：[【SpringBoot】使用Maven添加jQuery、bootstrap等依赖（WebJars）](https://blog.csdn.net/sinat_42483341/article/details/104095618)

##### Spring Boot 中的缓存配置

**常用配置（默认时间单位都是秒）**

- ResourceProperties.Cache                                                           类名
- spring.resources.cache.cachecontrol.max-age=时间              缓存控制最大时间
- spring.resources.cache.cachecontrol.no-cache=true/false    是否想让它做缓存
- spring.resources.cache.cachecontrol.s-max-age=时间           公共资源最长缓存的时间



#### From 6.11

##### Spring MVC 的异常解析

**核心接口**

- HandlerExceptionResolver

**实现类**

- SimpleMappingExceptionResolver
- DefaultHandlerExceptionResolver
- ResponseStatusExceptionResolver
- ExceptionHandlerExceptionResolver

##### 异常处理理方法

**处理方法**

- @ExceptionHandler：标识该方法是用来添加异常的

**添加位置**，可以是：

- @Controller / @RestController
- @ControllerAdvice / @RestControllerAdvice （类似于AOP）

在@ControllerAdvice中定义的方法优先级低于@Controller



#### From 6.12

##### Spring MVC 的拦截器

**核心接口**

- **HandlerInteceptor：**HandlerInteceptor接口定义了如下三个方法
  
  - **boolean <font color=FF0000>pre</font>Handle()：**在<font color=FF0000>业务处理器处理请求之前</font>被调用。预处理，可以进行编码、安全控制、权限校验等处理
  
    ```java
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {//...}
    ```
  
  - **void <font color=FF0000>post</font>Handle()：**在<font color=FF0000>业务处理器处理请求执行完成后</font>，生成视图之前执行
  
    ```java
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {//...}
    ```
  
  - **void <font color=FF0000>after</font>Completion()：**在 <font color=FF0000>DispatcherServlet 完全处理完请求后被调用</font>，可用于清理资源等。
  
    ```java
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {//...}
    ```

**针对 @ResponseBody 和 ResponseEntity 的情况**

- ResponseBodyAdvice

**针对异步请求的接口**

- AsyncHandlerInterceptor
  - void afterConcurrentHandlingStarted()

##### 拦截器的配置方式

**常规方法**

- WebMvcConfigurer.addInterceptors()

**Spring Boot 中的配置**

- 创建一个带 @Configuration 的 WebMvcConfigurer 配置类
- 不能带 @EnableWebMvc（想彻底自己控制 MVC 配置除外）

##### 补充

Spring MVC 中的拦截器（Interceptor）<mark>类似于 Servlet  开发中的过滤器 Filter</mark>，它主要用于拦截用户请求并作相应的处理，它也是 AOP 编程思想的体现，底层通过动态代理模式完成。

**拦截器有两种实现方式：**

1. <font color=FF0000>实现</font> HandlerInterceptor <font color=FF0000>接口</font>（见上面6.12开始处）
2. <font color=FF0000>继承</font> HandlerInterceptorAdapter <font color=FF0000>抽象类</font>（看源码最底层也是通过 HandlerInterceptor 接口 实现）

**应用场景：**

- 日志记录：记录请求信息的日志，以便进行信息监控、信息统计、计算PV（Page View）等；
- 登录鉴权：如登录检测，进入处理器检测是否登录；
- 性能监控：检测方法的执行时间；
- 其他通用行为。

**与 Filter 过滤器的区别**

- 拦截器是<font color=FF0000>基于java的反射机制的</font>，而过滤器是基于函数回调。
- 拦截器<font color=FF0000>不依赖于servlet容器</font>，而过滤器依赖于servlet容器。
- <font color=FF0000>拦截器只能对Controller请求起作用</font>，而过滤器则可以对几乎所有的请求起作用。
- 拦截器可以访问action上下文、值栈里的对象，而过滤器不能访问。
- <font color=FF0000>在Controller的生命周期中，拦截器可以多次被调用</font>，而过滤器只能在容器初始化时被调用一次。
- 拦截器可以获取IOC容器中的各个bean，而过滤器不行。这点很重要，在拦截器里注入一个service，可以调用业务逻辑。

摘自：[Spring Boot 2.X(九)：Spring MVC - 拦截器（Interceptor）](https://www.jianshu.com/p/c6b44175d06b)



#### From 7.1

##### Spring Boot 中的 RestTemplate

- Spring Boot 中没有自动配置 RestTemplate（需要自己new）
- Spring Boot 提供了RestTemplateBuilder
  - RestTemplateBuilder.build()

##### RestTemplate常用方法

- **GET 请求**
  - getForObject()  / getForEntity()
- **POST 请求**
  - postForObject()  / postForEntity()
- **PUT 请求**
  - put()
- **DELETE 请求**
  - delete()

代码示例如下，演示了如下使用RestTemplate获取对象：

```java
String result = restTemplate.getForObject(
    "http://example.com/hotels/{hotel}/{booking}", String.class, "42", "21");
```

上面的代码是手写的一个URI，其实可以构造URI，如下：

##### 构造URI

**构造 URI**

- UriComponentsBuilder

**构造相对于当前请求的 URI**

- ServletUriComponentsBuilder

**构造指向 Controller 的 URI**

- MvcUriComponentsBuilder

##### 补充：RestTemplate相关

RestTemplate就是<font color=FF0000>Spring 封装的处理同步 HTTP 请求的类</font>。

RestTemplate是Spring提供的<font color=FF0000>用于访问Rest服务的客户端</font>，RestTemplate提供了多种便捷访问远程Http服务的方法,能够大大提高客户端的编写效率。

RestTemplate是Spring的通过客户端访问RESTful服务端的核心类，和`JdbcTemplate、JmsTemplate`概念相似，都是Spring提供的模板类

RestTemplate的行为可以通过callback回调方法和配置`HttpMessageConverter`来定制，用来把对象封装到HTTP请求体，将响应信息放到一个对象中



spring boot官方文档中关于RestTemplate和WebClient的部分：[1.8. REST Endpoints](https://docs.spring.io/spring/docs/5.1.6.RELEASE/spring-framework-reference/integration.html#rest-client-access)



#### From 7.2

##### RestTemplate 的高阶用法

**传递 HTTP Header**

- RestTemplate.exchange()
- RequestEntity\<T> / ResponseEntity\<T>

**类型转换**

- JsonSerializer / JsonDeserializer
- @JsonComponent

**解析泛型对象**

- RestTemplate.exchange()
- ParameterizedTypeReference\<T>



#### From 7.3

##### RestTemplate支持的 HTTP 库

**通用接口**

- ClientHttpRequestFactory

**默认实现**

- SimpleClientHttpRequestFactory

**Apache HttpComponents**

- HttpComponentsClientHttpRequestFactory

**Netty**

- Netty4ClientHttpRequestFactory

**OkHttp**

- OkHttp3ClientHttpRequestFactory

##### 优化底层请求策略

**连接管理**

- PoolingHttpClientConnectionManager

- KeepAlive 策略

**超时设置**

- connectTimeout / readTimeout

**SSL校验**

- 证书检查策略



#### From 7.4

##### 通过 WebClient 访问 Web 资源

**WebClient**

- 一个以 Reactive 方式处理理 HTTP 请求的非阻塞式的客户端

**支持的底层 HTTP 库**

- Reactor Netty - ReactorClientHttpConnector
- Jetty ReactiveStream HttpClient - JettyClientHttpConnector

##### WebClient 的基本用法

**创建 WebClient**

- WebClient.create() 
- WebClient.builder()

**发起请求**

- get() / post() / put() / delete() / patch()

**获得结果**

- retrieve() / exchange()

**处理 HTTP Status**

- onStatus()

**应答正文**

- bodyToMono() / bodyToFlux()



#### From 8.1 & 8.2

##### 如何实现 Restful Web Service

- **识别资源**

  - 找到领域名词
    - 能用 CRUD 操作的<font color=FF0000>**名词**</font>
  - 将资源组织为集合（即集合资源）
  - 将资源合并为复合资源
  - 计算或处理函数

- **选择合适的资源粒度：**找到资源之后，如何设计资源，让其有一个合适的资源粒度

  - 站在<font color=FF0000>服务端</font>的角度，要考虑
    - 网络效率
    - 表述的多少
    - 客户端的易用程度
  - 站在<font color=FF0000>客户端</font>的角度，要考虑
    - 可缓存性
    - 修改频率
    - 可变性

- **设计 URI**

  - 使用域及子域对资源进行合理的分组或划分
  - 在 URI 的路径部分使用<font color=FF0000>斜杠分隔符 ( / )</font> 来表示资源之间的<font color=FF0000>层次关系</font>
  - 在 URI 的路径部分使用<font color=FF0000>逗号 ( , ) 和分号 ( ; )</font> 来表示<font color=FF0000>非层次元素</font>（使用频率较低）
  - 使用<font color=FF0000>连字符 ( - ) 和下划线 ( _ ) </font>来<font color=FF0000>改善长路路径中名称的可读性</font>
  - 在 URI 的查询部分<font color=FF0000>使用“与”符号 ( & ) 来分隔参数</font>
  - 在 URI 中<font color=FF0000>避免出现文件扩展名</font> ( 例如 .php，.aspx 和 .jsp )

- **选择合适的 HTTP 方法和返回码**

  |  动作   | 安全/幂等 |                             用途                             |
  | :-----: | :-------: | :----------------------------------------------------------: |
  |   GET   |   Y / Y   |                           信息获取                           |
  |  POST   |   N / N   | 该方法用途广泛，可用于<font color=FF0000>创建、更新</font>或<font color=FF0000>一次性对多个资源进行修改</font> |
  | DELETE  |   N / Y   |                           删除资源                           |
  |   PUT   |   N / Y   | <font color=FF0000>更新</font>或者<font color=FF0000>完全替换</font>一个资源 |
  |  HEAD   |   Y / Y   | 获取与GET一样的<font color=FF0000>HTTP头信息</font>，但<font color=FF0000>没有响应体</font> |
  | OPTIONS |   Y / Y   |                 获取资源支持的HTTP方法列列表                 |
  |  TRACE  |   Y / Y   |                  让服务器返回其收到的HTTP头                  |

  <mark>其中“安全“是不会改变资源的内容，”幂等“是无论请求多少次结果都是一样的。</mark>

  **URI 与 HTTP 方法的组合**

  |     URI      | HTTP方法 |       含义       |
  | :----------: | :------: | :--------------: |
  |   /coffee/   |   GET    | 获取全部咖啡信息 |
  |   /coffee/   |   POST   | 添加新的咖啡信息 |
  | /coffee/{id} |   GET    | 获取特定咖啡信息 |
  | /coffee/{id} |  DELETE  | 删除特定咖啡信息 |
  | /coffee/{id} |   PUT    | 修改特定咖啡信息 |

  **HTTP状态码**

  | 状态码 |        描述        | 状态码 |         描述          |
  | :----: | :----------------: | :----: | :-------------------: |
  |  200   |         OK         |  400   |      Bad Request      |
  |  201   |      Created       |  401   |     Unauthorized      |
  |  202   |      Accepted      |  403   |       Forbidden       |
  |  301   | Moved Permanently  |  404   |       Not Found       |
  |  303   |     See other      |  410   |         Gone          |
  |  304   |    Not Modified    |  500   | Internal Server Error |
  |  307   | Temporary Redirect |  503   |  Service Unavailable  |

- **设计资源的表述：**用Json / XML / Web页面

  - **JSON**
    - MappingJackson2HttpMessageConverter
    - GsonHttpMessageConverter（Google的）
    - JsonbHttpMessageConverter

  - **XML**
    - MappingJackson2XmlHttpMessageConverter
    - Jaxb2RootElementHttpMessageConverter

  - **HTML**

  - **ProtoBuf**
    - ProtobufHttpMessageConverter

## <font color=FF0000>From 8.3</font>

#### HATEOAS

**Richardson 成熟度模型**

- Level 3 - Hypermedia Controls（超媒体）

**HATEOAS**

- Hybermedia As The Engine Of Application State
- REST 统一接口的必要组成部分

##### HATEOAS v.s. WSDL

**HATEOAS**

- 表述中的超链接会提供服务所需的各种 REST 接口信息
- 无需事先约定如何访问服务

**传统的服务契约（WSDL）**

- 必须事先约定服务的地址与格式

##### 常用的超链接类型

|    REL     |                      描述                      |
| :--------: | :--------------------------------------------: |
|    self    |             指向当前资源本身的链接             |
|    edit    |         指向一个可以编辑当前资源的链接         |
| collection | 如果当前资源包含在某个集合中，指向该集合的链接 |
|   search   |   指向一个可以搜索当前资源与其相关资源的链接   |
|  related   |          指向一个与当前资源相关的链接          |
|   first    |    集合遍历相关的类型，指向第一个资源的链接    |
|    last    |   集合遍历相关的类型，指向最后一个资源的链接   |
|  previous  |    集合遍历相关的类型，指向上一个资源的链接    |
|    next    |    集合遍历相关的类型，指向下一个资源的链接    |



#### From 8.4

##### 认识 HAL

**HAL**

- Hypertext Application Language
- HAL 是一种简单的格式，为 API 中的资源提供简单一致的链接

**HAL 模型**

- 链接
- 内嵌资源
- 状态

##### Spring Data REST

Spring Data REST 可以帮我们实现Controller

**Spring Boot 依赖**

- spring-boot-starter-data-rest

**常用注解与类**

- **@RepositoryRestResource：**自动将Repository变成RESTful的接口，把它变成对应的资源
- **Resource\<T>：**T类型的资源
- **PagedResource\<T>：**变成T类型的分页资源



#### From 8.5

##### 如何访问 HATEOAS 服务

**配置 Jackson JSON**

- 注册 HAL 支持

**操作超链接**

- 找到需要的 Link
- 访问超链接



#### From 8.6

##### 分布式环境中如何解决 Session 的问题

**常见的会话解决方案**

- **粘性会话 Sticky Session：**会话落在同一台服务器上（缺点：如果掉线将会出问题）
- **会话复制 Session Replication：**将每台机器上的会话都做一次复制（缺点：成本过高）
- **<font color=FF0000>集中会话 Centralized Session</font>：**用 JDBC / Redis 集中存储会话信息，只要有相同的Session ID，就可以将Session从集中会话中取出，无论会话是在哪一台，只要是相同的Session ID，就可以取到相同的会话

##### Spring用Spring Session来实现集中会话

**Spring Session**

- 简化集群中的用户会话管理
- 无需绑定容器特定解决方案

**支持的存储**

- Redis
- MongoDB
- JDBC
- Hazelcast

##### Spring Session实现原理

**定制 HttpSession**

- 通过定制的 HttpServletRequest 返回定制的 HttpSession
  - SessionRepositoryRequestWrapper
  - SessionRepositoryFilter
  - DelegatingFilterProxy

##### 基于 Redis 的 HttpSession
**引入依赖**

- spring-session-data-redis

**基本配置**

- @EnableRedisHttpSession
- 提供 RedisConnectionFactory
- 实现 AbstractHttpSessionApplicationInitializer
  - 配置 DelegatingFilterProxy

##### Spring Boot 对 Spring Session 的⽀支持
**application.properties**

- spring.session.store-type=redis
- spring.session.timeout=
  - server.servlet.session.timeout=

- spring.session.redis.flush-mode=on-save
- spring.session.redis.namespace=spring:session



#### From 8.7 & 8.8

##### 使用 WebFlux 代替 Spring MVC

**什么是 WebFlux**

- 用于构建基于 Reactive 技术栈之上的 Web 应用程序
- 基于 Reactive Streams API ，运行在非阻塞服务器上

**为什么会有 WebFlux**

- 对于非阻塞 Web 应用的需要
- 函数式编程

**关于 WebFlux 的性能**

- 请求的耗时并不会有很大的改善
- 仅需少量固定数量的线程和较少的内存即可实现扩展

##### WebMVC v.s. WebFlux

- 已有 Spring MVC 应用，运行正常，就别改了
- 依赖了了大量阻塞式持久化 API （比如MySQL数据库）和网络 API，建议使用 Spring MVC
- 已经使用了非阻塞技术栈（Redi，MongoDB），可以考虑使用 WebFlux
- 想要使用 Java 8 Lambda 结合轻量级函数式框架，可以考虑 WebFlux

##### WebFlux 中的编程模型

**两种编程模型**

- 基于注解的控制器

  **常用注解**

  - @Controller
  - @RequestMapping 及其等价注解
  - @RequestBody / @ResponseBody

  **返回值**

  - Mono\<T> / Flux\<T>

- 函数式 Endpoints

##### 补充：WebFlux

了解 WebFlux，首先了解下什么是 Reactive Streams。Reactive Streams 是 JVM 中面向流的库标准和规范：

- 处理可能无限数量的元素
- 按顺序处理
- 组件之间异步传递
- 强制性非阻塞背压（Backpressure）
  - 背压是一种常用策略，使得发布者拥有无限制的缓冲区存储元素，用于确保发布者发布元素太快时，不会去压制订阅者。

Spring Boot Webflux 就是基于 Reactor 实现的。Spring Boot 2.0 包括一个新的 spring-webflux 模块。该模块包含对响应式 HTTP 和 WebSocket 客户端的支持，以及对 REST，HTML 和 WebSocket 交互等程序的支持。

**常用的 Spring Boot 2.0 WebFlux 生产的特性如下：**

- **响应式 API**

  - 响应式项目编程实战中，通过基于 Reactive Streams 规范实现的框架 Reactor 去实战。Reactor 一般提供两种响应式 API （这两个API是非阻塞的写法，使用这两个API才能发挥 WebFlux 非阻塞 + 异步的特性。）
    - Mono：实现发布者，并返回 0 或 1 个元素
    - Flux：实现发布者，并返回 N 个元素

- **编程模型**

  - Spring 5 web 模块包含了 Spring WebFlux 的 HTTP 抽象。类似 Servlet API ,  WebFlux 提供了 WebHandler API 去定义非阻塞 API 抽象接口。可以选择以下两种编程模型实现：
    - 注解控制层。和 MVC 保持一致，WebFlux 也支持响应性 @RequestBody 注解。
    - 功能性端点。基于 lambda 轻量级编程模型，用来路由和处理请求的小工具。和上面最大的区别就是，这种模型，全程控制了请求 - 响应的生命流程

- **适用性**（见下面 Spring Boot与WebFlux的异同点 ）

- **内嵌容器**

  - 跟 Spring Boot 大框架一样启动应用，但 WebFlux 默认是通过 Netty 启动，并且自动设置了默认端口为 8080。另外还提供了对 Jetty、Undertow 等容器的支持。开发者自行在添加对应的容器 Starter 组件依赖，即可配置并使用对应内嵌容器实例。

    但是要注意，必须是 Servlet 3.1+ 容器，如 Tomcat、Jetty；或者非 Servlet 容器，如 Netty 和 Undertow。

- **Starter 组件**

  - 跟 Spring Boot 大框架一样，Spring Boot Webflux 提供了很多 “开箱即用” 的 Starter 组件。Starter 组件是可被加载在应用中的 Maven 依赖项。只需要在 Maven 配置中添加对应的依赖配置，即可使用对应的 Starter 组件。例如，添加 `spring-boot-starter-webflux` 依赖，就可用于构建响应式 API 服务，其包含了 Web Flux 和 Tomcat 内嵌容器等。

    Spring Boot WebFlux 官方提供了很多 Starter 组件，每个模块会有多种技术实现选型支持，来实现各种复杂的业务需求：

    - Web：Spring WebFlux
    - 模板引擎：Thymeleaf
    - 存储：Redis、MongoDB、Cassandra。不支持 MySQL
    - 内嵌容器：Tomcat、Jetty、Undertow

- 还有对日志、Web、消息、测试及扩展等支持。

摘自：[Spring Boot 2.0 WebFlux 上手系列课程：快速入门（一）](https://www.jianshu.com/p/3ccfca09dcd6)

##### Spring MVC和WebFlux

<img src="https://upload-images.jianshu.io/upload_images/6010936-15cbf9d5be1c15b6?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp" alt="Spring Boot2.0 Reactor" style="zoom:50%;" />

**Spring MVC的定义**

> Spring MVC is built on the Servlet API and uses a synchronous blocking I/O architecture whth a one-request-per-thread model.

**Spring MVC** <mark><font color=FF0000>构建于 Servlet API 之上</font>，使用的是<font color=FF0000>**同步**</font><font color=0000FF>**阻塞式**</font> I/O 模型</mark>，什么是同步阻塞式 I/O 模型呢？就是说，<font color=FF0000>每一个请求对应一个线程去处理</font>。

**WebFlux的定义**

> Spring WebFlux is a non-blocking web framework built from the ground up to take advantage of multi-core, next-generation processors and handle massive numbers of concurrent connections.

Spring WebFlux 是一个<mark><font color=FF0000>**异步**</font><font color=0000FF>**非阻塞式**</font>的 Web 框架</mark>，它能够<font color=FF0000>充分利用多核 CPU 的硬件资源去处理大量的并发请求</font>。

**WebFlux 的优势&提升性能**

WebFlux 内部使用的是响应式编程（Reactive Programming），以 Reactor 库为基础，基于异步和事件驱动，可以让我们在不扩充硬件资源的前提下，<font color=FF0000>提升系统的吞吐量和伸缩性</font>。

当然：WebFlux 并不能使接口的响应时间缩短，它仅仅能够提升吞吐量和伸缩性。

**WebFlux 应用场景**

WebFlux特别适合应用在 IO 密集型的服务中，比如微服务网关这样的应用中。（<mark>IO 密集型包括：<font color=FF0000>**磁盘**</font>IO密集型，<font color=FF0000>**网络**</font>IO密集型</mark>，微服务网关就属于网络 IO 密集型，使用异步非阻塞式编程模型，能够显著地提升网关对下游服务转发的吞吐量）

**Spring Boot与WebFlux的异同点**

<img src="https://upload-images.jianshu.io/upload_images/6010936-c41a04a0d7c4a6bb?imageMogr2/auto-orient/strip|imageView2/2/w/800/format/webp" style="zoom: 65%;" />

**相同点：**

- 都可以使用 Spring MVC 注解，如 `@Controller`, 方便我们在两个 Web 框架中自由转换；
- 均可以使用 Tomcat, Jetty, Undertow Servlet 容器（Servlet 3.1+）；
- ...

**注意点：**

- <font color=FF0000>Spring MVC</font> 因为是使用的同步阻塞式，<font color=FF0000>更方便开发人员编写功能代码，Debug 测试等</font>，一般来说，<font color=FF0000>如果 Spring MVC 能够满足的场景，就尽量不要用 WebFlux</font>;
- WebFlux 默认情况下使用 Netty 作为服务器;
- WebFlux 不支持 MySql;

WebFlux 的前端控制器是 `DispatcherHandler`，它实现了 `WebHandler` 接口

```java
public interface WebHandler{
    Mono<Void> handle(ServerWebExchange var1);
}
```

摘自：[Spring Boot 2.0 WebFlux 教程 (一) | 入门篇](https://www.jianshu.com/p/66571e29c610) 和 [Spring Boot 2.0 WebFlux 上手系列课程：快速入门（一）](https://www.jianshu.com/p/3ccfca09dcd6)



#### From 9.1

##### 认识 Spring Boot 的组成部分

**Spring Boot 的特性**

- 方便地创建可独立运行的 Spring 应用程序
- 直接内嵌 Tomcat、Jetty 或 Undertow
- 简化了了项目的构建配置
- 为 Spring 及第三方库提供自动配置	
- 提供生产级特性无需生成代码或进行 XML 配置

**Spring Boot 的四大核心**

- 自动配置 - Auto Configuration
- 起步依赖 - Starter Dependency
- 命令行界面 - Spring Boot CLI
- Actuator



#### From 9.2

##### 了解自动配置的实现原理

**自动配置**

- 基于添加的 JAR 依赖自动对 Spring Boot 应用程序进行配置
- 自动配置代码都在spring-boot-autoconfiguration的JAR包中

**开启自动配置**

- @EnableAutoConfiguration
  - exclude = Class<?>[]：排除掉自动配置类
- @SpringBootApplication：包含@EnableAutoConfiguration

##### 自动配置的实现原理

**@EnableAutoConfiguration**

- AutoConfigurationImportSelector
- META-INF/spring.factories
  - org.springframework.boot.autoconfigure.EnableAutoConfiguration

**条件注解**

- **@Conditional**
- **@ConditionalOnClass：**当我的classPath上有XX类的时候，条件生效
- **@ConditionalOnBean：**在我的Spring容器中存在某个XX Bean的时候，条件生效
- **@ConditionalOnMissingBean：**在我的Spring容器中不存在某个XX Bean的时候，条件生效
- **@ConditionalOnProperty：**在我配置了特定属性之后，条件生效
- **……**

##### 了解自动配置的情况

**观察自动配置的判断结果**

- --debug （在命令行中）
- 设置在IDEA的`Run/Debug Configuration`下的`Program arguments`选项中填写`--debug`

**判断结果是通过ConditionEvaluationReportLoggingListener输出**

- Positive matches：匹配的自动配置
- Negative matches：未匹配的自动配置
- Exclusions：排除掉的自动配置
- Unconditional classes：无条件配置的



#### From 9.3

##### 动手实现自己的自动配置，主要工作内容

编写 Java Config

- @Configuration

添加条件

- @Conditional

定位自动配置

- META-INF/spring.factories

##### 条件注解大家庭

条件注解

- @Conditional

类条件

- @ConditionalOnClass
- @ConditionalOnMissingClass

属性条件

- @ConditionalOnProperty

Bean 条件

- @ConditionalOnBean
- @ConditionalOnMissingBean
- @ConditionalOnSingleCandidate

资源条件

- @ConditionalOnResource

Web 应用条件

- @ConditionalOnWebApplication
- @ConditionalOnNotWebApplication

其他条件

- @ConditionalOnExpression
- @ConditionalOnJava
- @ConditionalOnJndi

##### 自动配置的执行顺序

**执行顺序**

- @AutoConfigureBefore
- @AutoConfigureAfter
- @AutoConfigureOrder



**在pom.xml文件中导入自定义的Module：**

在File -> New -> Module from Existing Sources -> 选择对应的Module（也是pom.xml文件）



#### From 9.4

##### 如何在低版本 Spring 中快速实现类似自动配置的功能

##### **需求与问题**

**核心的诉求**

- 现存系统，不打算重构
- Spring 版本3.x，不打算升级版本和引入 Spring Boot
- 期望能够在少改代码的前提下实现一些功能增强

**面临的问题**

- <font color=FF0000>3.x 的 Spring 没有条件注解</font>
- 无法自动定位需要加载的自动配置

****

##### 核心解决思路路
**条件判断**

- 通过 BeanFactoryPostProcessor 进行判断

**配置加载**

- 编写 Java Config 类
- 引入配置类
  - 通过 component-scan
  - 通过 XML 文件 import



##### Spring 的两个扩展点

**BeanPostProcessor**

- 针对 Bean 实例
- 在 Bean 创建后提供定制逻辑回调

**BeanFactoryPostProcessor**

- 针对 Bean 定义
- 在容器器创建 Bean 前获取配置元数据
- Java Config 中需要定义为 static 方法



##### 关于 Bean 的一些定制
**Lifecycle Callback**

- InitializingBean / @PostConstruct / init-method
- DisposableBean / @PreDestroy / destroy-method

**XxxAware 接口**

- ApplicationContextAware
- BeanFactoryAware
- BeanNameAware



##### **一些常用操作**

判断类是否存在

- ClassUtils.isPresent()

判断 Bean 是否已定义

- ListableBeanFactory.containsBeanDefinition()
- ListableBeanFactory.getBeanNamesForType()

注册 Bean 定义

- BeanDefinitionRegistry.registerBeanDefinition()
  - GenericBeanDefinition
- BeanFactory.registerSingleton()



#### From 9.5

##### 关于 Maven 依赖管理的一些小技巧

**痛点**

- 你能记得多少 Maven 依赖
- 要实现一个功能，需要引入哪些依赖
- 多个依赖项目之间是否会有兼容问题
- ……

**了解你的依赖**

- mvn dependency:tree 列出依赖树
- IDEA Maven Helper 插件

**排除特定依赖**

- exclusion

**统一管理依赖**

- dependencyManagement
- Bill of Materials - bom



##### Spring Boot 的起步依赖

**Starter Dependencies**

- 直接面向功能
- 一站获得所有相关依赖，不再复制粘贴

**官方的 Starters**

- spring-boot-starter-*



#### From 9.6

##### 定制自己的起步依赖

**主要内容**

- **autoconfigure 模块：**包含自动配置代码
- **starter 模块：**包含指向自动配置模块的依赖及其他相关依赖

**命名方式**

- xxx-spring-boot-autoconfigure
- xxx-spring-boot-starter

**注意事项**

- <font color=FF0000>不要使用 spring-boot 作为依赖的前缀</font>
- <font color=FF0000>不要使用 spring-boot 的配置命名空间</font>
- starter 中仅添加必要的依赖
- 声明对 spring-boot-starter 的依赖



#### From 9.7

##### 深挖 Spring Boot 的配置加载机制

**外化配置加载顺序**

- 开启 DevTools 时，~/.spring-boot-devtools.properties
- 测试类上的 @TestPropertySource 注解
- @SpringBootTest#properties 属性
- <font color=FF0000>命令行参数（ --server.port=9000 ）</font>
- SPRING_APPLICATION_JSON 中的属性
- ServletConfig 初始化参数
- ServletContext 初始化参数
- java:comp/env 中的 JNDI 属性
- <font color=FF0000>System.getProperties()</font>
- <font color=FF0000>操作系统环境变量</font>
- random.* 涉及到的 RandomValuePropertySource

##### **外化配置加载顺序**

- jar 包外部的 application-{profile}.properties 或 .yml
- jar 包内部的 application-{profile}.properties 或 .yml
- jar 包外部的 application.properties 或 .yml
- jar 包内部的 application.properties 或 .yml

- @Configuration 类上的 @PropertySource
- SpringApplication.setDefaultProperties() 设置的默认属性

##### **application.properties可以放在哪里**
**默认位置**

- ./config
- ./
- CLASSPATH 中的 /config
- CLASSPATH 中的 /

**修改名字或路径**

- spring.config.name
- spring.config.location
- spring.config.additional-location

##### Relaxed Binding

|      命名风格      |             使用范围              |              示例               |
| :----------------: | :-------------------------------: | :-----------------------------: |
|     短划线分隔     | Properties文件 / YAML文件系统属性 | geektime.spring-boot.first-demo |
|       驼峰式       |               同上                |  geektime.springBoot.firstDemo  |
|     下划线分隔     |               同上                | geektime.spring_boot.first_demo |
| 全大写，下划线分隔 |             环境变量              |  GEEKTIME_SPRINGBOOT_FIRSTDEMO  |



#### From 9.8

##### 理解配置背后的 PropertySource 抽象

**添加 PropertySource**

- \<context:property-placeholder>
- PropertySourcesPlaceholderConfigurer
  - PropertyPlaceholderConfigurer
- @PropertySource
- @PropertySources

**Spring Boot 中的 @ConfigurationProperties**

- 可以将属性绑定到结构化对象上
- 支持 Relaxed Binding
- 支持安全的类型转换
- @EnableConfigurationProperties

##### **定制 PropertySource**

**主要步骤**

- 实现 PropertySource\<T>
- 从 Environment 取得 PropertySources
- 将自己的 PropertySource 添加到合适的位置

**切入位置**

- EnvironmentPostProcessor
- BeanFactoryPostProcessor



#### From 10.1

##### Actuator

**目的：**监控并管理理应⽤用程序

**访问Actuator的方式**

- HTTP
- JMX

**依赖：**spring-boot-starter-actuator

#### 一些常用Actuator Endpoint

|       ID       |                 说明                  |          默认开启           |          默认HTTP           | 默认JMX |
| :------------: | :-----------------------------------: | :-------------------------: | :-------------------------: | :-----: |
|     beans      |        显示容器中的 Bean 列表         |              Y              |              N              |    Y    |
|     caches     |           显示应用中的缓存            |              Y              |              N              |    Y    |
|   conditions   |        显示配置条件的计算情况         |              Y              |              N              |    Y    |
|  configprops   | 显示 @ConfigurationProperties 的信息  |              Y              |              N              |    Y    |
|      env       | 显示 ConfigurableEnvironment 中的属性 |              Y              |              N              |    Y    |
|     health     |           显示健康检查信息            |              Y              | <font color=FF0000>Y</font> |    Y    |
|   httptrace    |         显示 HTTP Trace 信息          |              Y              |              N              |    Y    |
|      info      |         显示设置好的应用信息          |              Y              | <font color=FF0000>Y</font> |    Y    |
|    loggers     |          显示并更新日志配置           |              Y              |              N              |    Y    |
|    metrics     |          显示应用的度量信息           |              Y              |              N              |    Y    |
|    mappings    |    显示所有的 @RequestMapping 信息    |              Y              |              N              |    Y    |
| scheduledtasks |        显示应用的调度任务信息         |              Y              |              N              |    Y    |
|    shutdown    |          优雅地关闭应用程序           | <font color=FF0000>N</font> |              N              |    Y    |
|   threaddump   |           执行 Thread Dump            |              Y              |              N              |    Y    |
|    heapdump    |   返回 Heap Dump 文件，格式为 HPROF   |              Y              |              N              |  N / A  |
|   prometheus   |    返回可供 Prometheus 抓取的信息     |              Y              |              N              |  N / A  |

##### 如何访问 Actuator Endpoint
**HTTP 访问**

- /actuator/\<id>

**端口与路径**

- management.server.address=
- management.server.port=                                                           设置发布端口和应用程序端口等端口
- management.endpoints.web.base-path=/actuator
- management.endpoints.web.path-mapping.\<id>=路路径

**开启 Endpoint**

- management.endpoint.\<id>.enabled=true
- management.endpoints.enabled-by-default=false

**暴露 Endpoint**

- management.endpoints.jmx.exposure.exclude=
- management.endpoints.jmx.exposure.include=*
- management.endpoints.web.exposure.exclude=
- management.endpoints.web.exposure.include=info, health



#### From 10.2

##### Spring Boot 自带的 Health Indicator

**目的**

- 检查应用程序的运行状态

**状态**

- DOWN - 503
- OUT_OF_SERVICE - 503
- UP - 200
- UNKNOWN - 200

##### 下面没看完



#### From 10.5

