# MyBatis学习笔记

> 尚硅谷SSM教程

<font size=3 color=FF0000>**写在前面:**</font>
**Bean、Model、Entity的区别**</font>

- **Bean：** 任何一个java类都可以成为一个bean，这个类里包含对象的属性、get、set方法及其他的业务逻辑。<mark><font color=FF0000>普通的JavaBean可以包含业务逻辑代码</font></mark>
- **Model：** model是MVC中的概念，可以理解为View层展示数据的对象
- **Entity：** 数据表对应到实体类的映射。<mark><font color=FF0000>一般是用于ORM 对象关系映射 ，一个实体映射成一张表，一般无业务逻辑代码</font></mark>

## <font color=FF0000>**From 1.1**</font>

**JDBC连接接数据库流程**
获取连接 -> 获取PreparedStatement -> 写SQL -> 预编译参数 -> 执行SQL -> 封装结果
**不用原生JDBC的原因**

- 麻烦
- SQL语句是硬编码在程序中的，不方便修改；数据库层和Java编码耦合度过高

**使用Hibernate连接数据的的流程**
JavaBean -> <font color=FF0000>获取连接 -> 获取PreparedStatement -> <mark><font color=FF0000>**写SQL**</font></mark> -> 预编译参数 -> 执行SQL -> 封装结果</font> -> DB Table
其中为红色部分为高度封装部分，类似于黑箱操作，耦合度较高；而不方便传入自定义的SQL语句。
**MyBatis的优点**

- MyBatis讲重要的步骤抽取出来可以人工维护，其他步骤自动化
- 重要步骤都是写在配置文件中
- 完全解决数据的优化问题
- MyBatis底层就是对原生JDBC的简单封装
- 既将Java编码与SQL抽取了出来（这里不需要耦合），还不会失去自动化功能

## <font color=FF0000>**From 1.3**</font>

类的成员变量名和数据库的字段名要一样，同时由于数据库字段名不区分大小写：比如类的成员变量名为empName，数据库字段名为empname，依然看作一样。
</font>

## <font color=FF0000>**From 1.4**</font>

<mark>**在pom.xml文件<font color=FF0000>（这是Maven的配置文件）</font>下的重要配置**</font></mark>

```xml
    <dependencies>
        <!--配置mybatis版本-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.4</version>
        </dependency>
        <!--配置mysql的driver-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
    </dependencies>
```

<mark>**在mybatis-config.xml文件<font color=FF0000>（这是指导mybatis运行的全局配置文件）</font>下的重要配置**</font></mark>

<font size=4><span id="1d4">**1.8、1.13、1.15的页内跳转示例如下**</span>，[**返回1.8**](#1d8) ，[**返回1.13**](#1d13)，[**返回1.15**](#1d15)</font>

```xml
<configuration>
    <!--
        声明外部配置的位置，其中config.properties是需要引入的外部配置文件名
        resource：表示从类路径下寻找资源；还有其他选项url，是在磁盘路径或网络路径中需寻找资源
    -->
    <properties resource="config.properties"/>
    <typeAliases>
        <package name=""/>
    </typeAliases>

    <!--default是默认使用哪个环境，如果有多个环境，在选择时只需要对default进行修改即可进行选择-->
    <environments default="development">
        <!--
            environment是配置一个具体的环境，包含一个事务管理器和一个数据源（不过在Spring项目中事务管理和数据源都是Spring来做，所以无需在mybatis中配置）
            id是environment的唯一标识（因为可以有多个环境，多个环境也就可以有多个数据源）
        -->
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <!--这里${}中的内容从上面的<properties resource="config.properties"中获得-->
                <property name="driver" value="${database.driver}"/>
                <property name="url" value="${database.url}"/>
                <property name="username" value="${database.username}"/>
                <property name="password" value="${database.password}"/>
            </dataSource>
        </environment>
    </environments>

    <!--引入我们自己编写的每一个接口的实现文件-->
    <mappers>
        <!--
            resource：默认使用resource， 表示从类路径下寻找sql映射文件（xml文件）
            class：直接引用接口的全类名（接口），如果使用这种方法要将配置文件放到与接口同一个包路径下（同时配置文件名也要和接口名一致），否则两者无法绑定。（当然这种方法某种程度上不方便文件管理和管理）
                甚至使用class选项，可以不写xml配置文件（只将class对应的值设为全类名），只需要在接口的实现方法前面加上注解，也可以。示例如下：
                public interface EmployeeDaoWithAnnotation{
                    //除了Select注解外，类似的注解还有Update、Delete、Insert
                    @Select("select * from t_employee where id=#{id}")
                    public Employee getEmpById(Integer id);
                }
                不过这种方法虽然简单，但代码耦合度过高，不够灵活；建议偶尔使用
            url：从磁盘或网络路径引用

            同时由于一个一个mapper进行注册过于繁琐，所以可以进行批量注册，即对于整个包进行注册，用package，示例如下：<package name="com.atguigu.dao"/>
            不过：和上面使用class一样，要将配置文件放到与接口同一个包路径下，否则无法绑定
        -->
        <mapper resource="mapper/StudentMapper.xml"/>
    </mappers>
</configuration>
```

<mark>**在*Mapper.xml文件下的重要配置<font color=FF0000>（描述mapper中每一个方法如何工作）</font>**</font></mark>

<span id="1d4d1"><font size=4>**关于1.14的databaseId相关示例与注释在这里，[返回1.14](#1d14)**</font></span>

```xml
<!-- 
    namespace:名称空间，写接口的完整类名，相当于告诉MyBatis这个配置文件是实现哪个接口的
-->
<mapper namespace="dao.EmployeeDao">
    <!--
        select: 用来定义一个查询操作，类似的还可以create、update等
        id：方法名，相当于这个配置是对某个方法的实现
        resultType: 指定方法运行后的返回值类型，查询操作必须指定
        databaseId: 由于不同数据库SQL语句会有所不同（不兼容），且一个项目可能使用不同数据库作为数据源（或者进行数据移植），这时候要对不同的数据库（根据databaseId区分）设置不同的SQL语句。当然，如果只使用一个厂商的数据库，可以不设置（当然这也是绝大多数情况）
        #{属性名}：这里是#{id}，代表出去传递过来的某个参数的值
    -->
    <select id="getEmpById" resultType="bean.Employee" databaseId="mysql">
        <!--这里注意SQL语句不建议在最后加分号-->
        select * from t_employee where id = #{id}
    </select>

    <!--
        这里举的是update的例子
        增删改可以不写返回值类型（因为都是影响的记录行数）
        这里empname=#{empname}的#{}中填写引入的id包含对象的成员即可
    -->
    <update id="dao.updateEmployee">
        update t_employee set empname=#{empName}, gender=#{gender}, email=#{email} where id=#{id}
    </update>
</mapper>
```

## <font color=FF0000>**From 1.5**</font>

```java
        //根据全局配置文件创建出一个SqlSessionFactory
        //SqlSessionFactory：是SqlSession工厂，负责创建SqlSession对象
        String resource = "conf/mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        //SqlSession: Sql会话，代表和数据库的一次会话
        SqlSession openSession = sqlSessionFactory.openSession();
        Employee employee = null;
        //这里建议使用try catch finally
        try {
            //获取和数据库的一次会话：getConnection()
            //使用SqlSession操作数据库，获取到mapper接口的实现
            EmployeeDao employeeDao = openSession.getMapper(EmployeeDao.class);
            employee = employeeDao.getEmpById(1);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //如果这里是对数据库数据进行更新（插入、删除）的话，必须要加上commit()
            //openSession.commit();
            openSession.close();
        }
        System.out.println(employee.toString());
```

## <font color=FF0000>**From 1.8**</font>

**MyBatis的两个重要配置文件**

- 全局配置文件：mybatis-config.xml，指导mybatis正确运行的一些全局设置
- SQL配置文件：*Mapper.xml，相当于是对mapper接口的实现描述

**细节**

- 获取到的是接口的代理对象，mybatis自动创建的
- SqlSessionFactory和SqlSession
  - SqlSessionFactory创建SqlSession对象，Factory只需创建（new）一次
  - SqlSession相当于connection，和数据库交互数据的一次会话；每和数据库会话一次，就需要创建一个新的sqlSession

## <font color=FF0000>**From 1.8**</font>

**使用全局配置文件(.properties)引入外部配置文件**

```xml
<configuration>
    <properties resource="config.properties"/>
</configuration>
```

<span id="1d8">这里的使用例子在上面</span>，[点击跳转](#1d4)
</font>

## <font color=FF0000>**From 1.9**</font>

**\<settings>标签**
\<setttings>是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为；**举例如下：**

<font size=4><span id="1d9">**2.5关于驼峰命名的配置如下，[返回2.5](#2d5)**</span></font>

```xml
<settings>
    <!--
        官方文档对于mapUnderscoreToCamelCase的解释：
        是否开启自动驼峰命名规则（camel case）映射，即从经典数据库列名 A_COLUMN 到经典 Java 属性名 aColumn 的类似映射。
    -->
    <setting name="mapUnderscoreToCamelCase" value="true"/>
</settings>
```

## <font color=FF0000>**From 1.10**</font>

**\<typeAliases>**
\<typeAliases>类型别名是<font color=FF0000>为Java类型设置一个短的名字</font>。它只和XML配置有关，存在的意义仅在于用来减少类完全限定名的冗余；**举例如下：**

```xml
<typeAliases>
    <!--typeAlias: 就是为JavaBean（包含包的路径）起一个别名，别名默认就是类名（不包含包的路径），不区分大小写；这里将其改为emp-->
    <typeAlias type="com.atguigu.bean.Employee" alias="emp">

    <!--批量起别名（由于一个项目中类过多，所以上面的方法很不方便），默认别名就是类名。即使在批量起别名的情况下，如果需要对某个类不按照批量起别名的方法，想要起其他名字，可以在类的定义前面加上@Alias("AliasName")-->
    <package name="com.atguigu.bean"/>

    <!--即使拥有以上方法，依然不推荐使用；使用包含包路径的类名可以在出了问题后准确定位，而在起别名后将（在Eclipse上）无法定位-->
</typeAliases>
```

除此以外，在起别名时要注意<font color=FF0000>**Java类型内建的相应的类型别名（它们都是不区分大小写），不能与之冲突**</font>

## <font color=FF0000>**From 1.11**</font>

**\<typeHandlers>**
\<typeHandlers>类型处理器：无论是MyBatis在预处理语句（PreparedStatement）中设置一个参数时（setInt(), setString()...），还是从结果集中取出一个值时，都会用类型处理器将获取的值以合适的方式转换成Java类型。
如果要使用自己定义的类型处理器，配置xml代码如下：

```xml
<typeHandlers>
    <typeHandler handler="...">
</typeHandlers>
```

## <font color=FF0000>**From 1.12**</font>

**\<objectFactory>**
\<objectFactory> <font color=FF0000>MyBatis每次创建结果对象的新实例时，它都会使用一个对象工厂（ObjectFactory）实例来完成</font>。默认的对象工厂需要做的仅仅是实例化目标类，要么通过默认构造方法，要么在参数映射存在的时候通过参数构造方法来实例化。<font color=FF0000>如果想覆盖对象工厂的默认行为，则可以通过创建自己的对象工厂来实现</font>
**\<plugins>**
我们通过插件来修改MyBatis的一些核心行为。插件通过动态代理机制，可以<font color=FF0000>**介入** 四大对象</font>的任何一个方法的执行。**四大对象如下：**

- Executor (update, query, flushStatements, commit, rollback, getTransaction, close, isClosed)  //执行器
- ParameterHandler (getParameterObject, setParameters)  //参数处理器
- ResultSetHandler (handleResultSets, handleOutputParameters)  //结果集处理器，用于封装查询结果为对象
- StatementHandler (prepare, parameterize, batch, update, query)

## <font color=FF0000>**From 1.13**</font>

**\<environments>**
<span id=1d13>**这里关于environment相关的例子和讲解在上面**</span>，[**点击跳转**](#1d4的)
</font>

## <font color=FF0000>**From 1.14**</font>

**mybatis-config.xml文件<font color=FF0000>必须按照如下顺序配置</font>，否则配置文件将会报错<font color=FF0000>（可以有部分没有配置，但不能出现顺序错误）</font>**
properties -> settings -> typeAliases -> typeHandlers -> objectFactory -> objectWrapperFactory -> reflectorFactory -> plugins -> environments -> databaseIdProvider -> mappers
**\<databaseIdProvider>**
\<databaseeIdProvider>是用于数据库移植的，<font color=FF0000>**对于有多个不同厂商的数据库进行区分**</font>，**示例如下**

```xml
<databaseIdProvider type="DB_VENDOR">
    <!--
        name: 数据库厂商标识（MySQL Oracle SQL Server）
        value: 自定义的别名
    -->
    <property name="MySQL" value="mysql"/>
    <property name="SQL Server" value="sqlserver"/>
    <property name="Oracle" value="oracle">
</databaseIdProvider>
```

<span id="1d14">**至于上面内容的用途，与databaseId相关；具体见上面，**[**点击跳转**](#1d4d1)</span>

## <font color=FF0000>**From 1.15**</font>

**\<mapper>**
MyBatis的行为已经由上面的元素配置完了，我们现在就要定义SQL映射语句了。但是首先我们需要告诉 MyBatis到哪里去找到这些语句（即设定）。
<span id="1d15">**对于mapper的使用与示例在上面，**</span>[**点击跳转**](#1d4)
</font>

## <font color=FF0000>**From 1.17**</font>

**mapper.xml文件中可以写的标签**

- \<cache> 和缓存相关
- \<cache-ref> 和缓存相关
- \<select> | \<insert> | \<update> | \<delete> 增删改查
- \<paramenterMap> 参数Map，用于作复杂参数映射<font color=FF0000>**（这是一个废弃的标签）**</font>
- \<resultMap> 结果映射：自定义结果集的封装规则
- \<sql> sql语句，用于抽取可重用的sql

**增删改的标签**

| 标签                                                          | 解释                                                                                                                                                   |
|:-----------------------------------------------------------:|:----------------------------------------------------------------------------------------------------------------------------------------------------:|
| <mark><font color=FF0000>id</font></mark>                   | <font color=FF0000>命名空间中的唯一标识符</font>                                                                                                                |
| parameterType                                               | <font color=FF0000>将要传输语句的参数的完全限定类名或别名</font>，这个属性是可选的，<font color=FF0000>**因为MyBatis可以通过TypeHandler推断出具体传入语句的参数类型**</font>，默认值为unset                |
| flushCache                                                  | 将其设置为true，任何时候只要语句被调用都会导致本地缓存和耳机缓存都会被清空，默认值为true（对应插入、更新和删除语句）                                                                                       |
| <mark><font color=FF0000>**timeout**</font></mark>          | 这个设置是在抛出异常之前，驱动程序<font color=FF0000>**等待数据库返回请求结果的秒数**</font>。默认值为unset（依赖驱动）                                                                        |
| statementType                                               | STATEMENT,PREPARED或CALLABLE的一个。这会使MyBatis分别使用Statement，preparedStatement或CallableStatement，<font color=FF0000>**默认值：PREPATED**</font>                |
| <mark><font color=FF0000>**useGeneratedKeys**</font></mark> | （仅对insert和update有用）这会令MyBatis使用JDBC的getGeneratedKeys方法来取出数据库内部生成的主键（比如：像MySQL和SQL Server这样的关系数据库管理系统的<font color=FF0000>**自动递增字段**</font>），默认值：false |
| <mark><font color=FF0000>**keyProperty**</font></mark>      | （仅对insert和update有用）唯一标记一个属性，<font color=FF0000>**MyBatis会通过getGeneratedKeys的返回值或者通过insert语句的selectKey子元素设置它的键值**</font>，默认值：unset                    |
| keyColumn                                                   | （仅对insert和update有用）通过生成的键值设置表中的列名，这个设置尽在某些数据库（像PostgreSQL）是必须的，当主键列不是表中的第一列的时候需要设置。如果希望得到多个生成的列，也可以是逗号分隔的属性名称列表                                      |
| <mark><font color=FF0000>**databaseId**</font></mark>       | <font color=FF0000>**如果设置了databaseIdProvider，MyBatis会加载所有的不带databaseId或匹配当前databaseId的语句**</font>；如果带或者不带的语句都有，则不带的会被忽略                              |

## <font color=FF0000>**From 1.19**</font>

**\<select>**
在查询语句，如果查询参数只有一个，那么在SQL语句中#{}的内容<font color=FF0000>**甚至可以随便写**（因为它尤其仅有一个参数）</font>
<font color=FF0000>**如果查询参数不止一个**</font>，那么在sql语句中#{}的内容必须是0,1,...或者param1,param2,...
示例如下：

```sql
<select id="getEmpByIdAndEmpName" resultType="com.atguigu.bean.Employee">
    select * from t_employee where id=#{param1} and empname=#{param2}
    select * from t_employee where id=#{0} and empname=#{1}
</select>
```

<font color=FF0000>**总结：**</font>

- 单个参数
  
  - 基本类型：取值#{任意} 
  - 传入pojo 

- 多个参数
  
  ```java
  public Employee getEmpIdAndEmpName(Integer id, String empName);
  ```
  
  - 取值:#{参数名}无效
  
  - 可用：0,1（参数的索引）或者param1,param2,...paramN
  
  - <font color=FF0000>**原因：之后要传入了多个参数，mybatis将会自动的把这几个参数封装在一个map中，封装时使用的key就是参数的索引和参数的第几个表示**</font>
    Map<String, object> map = new HashMap<>();
    map.put("1", 传入的值); map.put("2", 传入的值);
  
  - <font color=FF0000>这时可以告诉MyBatis，封装参数map时使用我们指定的key</font>，<mark>**示例如下：**</mark>
    
    ```
    //使用标注@Param:为参数指定key，为命名参数；事实上也推荐这么做，这样便于代码维护管理
    public Employee getEmpIdAndEmpName(@Param("id")Integer id, @Param("empName")String empName);
    
    <select id="getEmpByIdAndEmpName" resultType="com.atguigu.bean.Employee">
        select * from t_employee where id=#{id} and empname=#{empName}
    </select>
    ```

- 传入pojo(JavaBean)：取值：#{pojo的属性名(object.property)}

- <mark><font color=FF0000>**传入Map：将多个要使用的参数封装起来，封装为一个参数**</font></mark>**，在调用前将属性名与属性值以键值对的方式传入Map（使用map.put(key, value)的方法）**，<font color=FF0000>**示例如下：**</font>
  
  ```java
  public Employee getEmpIdAndEmpName(Map<String, Object> map);
  ```

- 扩展：多个参数，MyBatis自动封装为map
  
  ```java
  method1(@Param("id")Integer id, String empName, Employee employee);
  //Integer id -> #{id}
  //String empName -> #{param2}
  //Employee employee(取出其中的email) -> #{param3.email}
  ```

## <font color=FF0000>**From 2.2**</font>

**#{属性名} 和 ${属性名}的区别**
<font color=FF0000>**试验如下:**</font>

```sql
select * from t_employee where id=${id} and empname=#{empName}的日志文件显示：
    select * from t_employee where id = 1 and empname=?

select * from t_employee where id=#{id} and empname=#{empName}的日志文件显示：
    select * from t_employee where id = ? and empname=?
```

#{属性名}：是参数<mark><font color=FF0000>**预编译的方式**，参数的位置都是用?替代</font></mark>，参数都是后来预编译设置进去的。这样做<font color=FF0000>**安全**</font>，不会有SQL注入
${属性名}：不是参数预编译，而是直接和SQL语句进行拼串，<font color=FF0000>**不安全**</font>，比如id对应的填写"1 or 1=1"，就变成了id=1 or 1=1，该限制将毫无意义。<font color=FF0000>${}没有用：在SQL语句中<mark><font color=FF0000>只有参数位置</font></mark>是支持预编译的**</font>，而对于其他，比如说对于表名可以使用\${}进行设定：

```sql
--是错误的，因为非参数位置不可以进行预编译
select * from #{} where id = #{} and empname = #{}
--是正确的
select * from ${} where id = #{} and empname = #{}
```

<font color=FF0000>**总结：**</font>一般都是使用#{}，这样做安全；<font color=FF0000>${}</font>

## <font color=FF0000>**From 2.3**</font>

**查询返回一个List**

无需在配置文件中做任何配置，<font color=FF0000>**resutlType仍然写List中的内容的类型**</font>，MyBatis自动为你封装并返回一个List

## <font color=FF0000>**From 2.4**</font>

**查询一个返回一个Map（单条记录放入map）**

```xml
<!-- 查询一个返回一个map，列名作为Key，列值作为Value -->
<!-- public Map<String, Object> getEmpByIDReturnMap(Integer id); -->
<select id="getEmpByIdReturnMap resultType="map">
    <select * from t_employee where id=#{id}>
</select>
```

**查询多个返回一个Map（多条记录放入map）**
要设定将查询记录的id值作为key封装这个map，<font color=FF0000>**要使用注解**</font>，示例如下

```xml
@MapKey("id")
public Map(Integer, Employee) getAllEmpsReturnMap();

<!-- 查询多个返回一个map，集合中写元素类型 -->
<!-- public Map<Integer, Employee> getAllEmpsReturnMap(); -->
<select id="getAllEmpsReturnMap" resultType="com.atguigu.bean.Employee">
    select * from t_employee
</select>
```

## <font color=FF0000>**From 2.5**</font>

**当数据库属性名与bean属性名不一致的情况**
<span id=2d5>默认mybatis自动封装结果集</span>

- 按照列名和属性名一一对应的规则（不区分大小写）

- 如果不一一对应
  
  - 开启驼峰命名法（上面有示例，[点击跳转](#1d9)）
  
  - 起别名
    
    - 方法一:为数据库的属性名起别名；示例：
      
      ```xml
      <select id="getCatById" resultType="com.atguigu.bean.Cat">
        select id, cname name, cAge age, cgender gender form t_cat where id=#{id}
      </select>
      ```
    - 方法二：通过配置自定义结果集（自己定义每一列数据和JavaBean的映射规则），使用标识resultMap，其中resultMap下有两个属性；分别为type和id，type指定哪一个JavaBean自定义封装规则，id为唯一标识（可以随意命名，只要唯一即可），让别名再后面引用。示例：
      
      ```xml
      <resultMap type="com.atguigu.bean.Cat" id="mycat"></resultMap>
        <!--
            指定主键列的对应规则：
            column="id":指定哪一列是主键列
            property="":指定cat的哪一个属性封装id这一列数据
        -->
        <id property="id" column="id"/>
        <!-- 指定普通列的对应规则 -->
        <result property="name" column="cName"/>
        <result property="age" column="cAge"/>
        <result property="gender" column="cGender">
      </resultMap>
      ```
    
    <!-- 同时，要将resultType改为resultMap，告诉mybatis：查出数据封装结果的时候，使用mycat（上面的id的设定） -->
    
    <!-- <select id="getCatById" resultType="com.atguigu.bean.Cat" -->
    
    <select id="getCatById" resultMap=""myCat>
    
        select * from t_cat where id=#{id}
    
    </select>
    ```

## <font color=FF0000>**From 2.7**</font>

**对于<font color=FF0000>多表级联查询</font><mark>(JavaBean的类中有子类，数据库的表中有外键)</mark>，除了使用传统的方法(和2.5讲的方法一样)外，mybatis推荐使用<font color=FF0000>association</font>；同样，如果<font color=FF0000>JavaBean的类中有一个集合</font>，mybatis推荐使用<font color=FF0000>collection</font>**
**传统方法示例:**

```xml
<resultMap type="com.atguigu.bean.Key" id="mykey">
    <id property="id" column="id"/>
    <result property="keyName" column="keyName"/>
    <!--对子类进行配置-->
    <result property="lock.id" column="lid"/>
    <result property="lock.lockName" column="lockName"/>
</resultMap>
```

**使用association方法示例：**

```xml
<resultMap type="com.atguigu.bean.Key" id="mykey">
    <id property="id" column="id"/>
    <result property="keyName" column="keyname"/>
    <!--接下来的属性是一个对象，自定义这个对象的封装规则，使用association，表示联合了一个对象-->
    <!--javaType:指定这个属性的类型-->
    <association property="lock" javaType="com.atguigu.bean.Lock">
        <id property="id" column="lid"/>
        <result property="lockName" column="lockName"/>
    </association>
</resultMap>
```

## <font color=FF0000>**From 2.9**</font>

**如2.7所说：如果<font color=FF0000>JavaBean的类中有一个集合</font>，mybatis推荐使用<font color=FF0000>collection</font>，示例如下：**

```xml
<resultMap type="com.atguigu.bean.lock" id="mylock">
    <id property="id" column="id"/>
    <result property="lockName" column="lockName"/>
    <!--
        collection：指定集合元素的封装
        property：指定哪个属性是集合属性
        ofType: 指定集合内元素的类型
    -->
    <collection property="keys" ofType="com.atguigu.bean.Key">
        <!--标签体中指定集合中这个元素的封装规则-->
        <id property="id" column="kid"/>
        <result property="keyName" column="keyname"/>
    </collection>
</resultMap>
```

## <font color=FF0000>**From 2.10**</font>

**MyBatis分步查询**

没有完全看懂...

## <font color=FF0000>**From 2.11**</font>

<font color=FF0000>**按需加载和延迟加载**</font>：分步查询会导致性能下降，这时可以配置按需加载和延迟加载

- 按需加载：需要时再去加载，全局开启按需加载策略
- 延迟加载：不着急加载（查询对象）
  
  ```xml
  <settings>
    <!--开启延迟加载（默认为关闭）-->
    <setting name="lazyLoadingEnabled" value="true"/>
    <!--开启属性按需加载（默认为关闭，即激进）-->
    <setting name="aggressiveLazyLoading" value="false"/>
  </settings>
  ```

## <font color=FF0000>**From 2.12**</font>

**不推荐使用分布查询，因为进行一次操作要对数据库进行多次查询，性能较低**
</font>

## <font color=FF0000>**From 2.13**</font>

**动态SQL：可以简化SQL语句动态拼串操作<mark><font color=FF0000>（这里用的是OGNL表达式）</font>**</mark>
<font color=FF0000>**sql-if示例如下：**</font>

```xml
<select id="getTeacherByCondition" resultMap="teacherMap">
    select * from t_teacher where
    <!-- test: 编写判断条件-->
    <if test="id!=null">
        id > #{id}
    </if>
    <!--
        下面的代码如果使用Java编写的话应该是if(name!=null && name!="")
        由于在xml中 ", ', &, <, >需要进行转义，具体见下面；
        所以&&要转义，也可以用and。同时也支持or
        另外，这里建议使用equals()方法，而不是==
    -->
    <if test="name!=null and name!=""">
        and teacherName like #{name}
    </if>
    <if test="birth!=null">
        and birth_date > #{birth}
    </if>
</select>
```

- <font color=FF0000>**"**</font>  : \&quot;

- <font color=FF0000>**'**</font>  : \&apos;

- <font color=FF0000>**&**</font>  : \&amp;

- <font color=FF0000>**<**</font>  : \&lt;

- <font color=FF0000>**\>**</font>  : \&gt;

具体可以看[w3schoolISO-8859-1 参考手册](https://www.w3school.com.cn/tags/html_ref_entities.html)

## <font color=FF0000>**From 2.14**</font>

**sql-where标签**
如上2.13的代码，sql-if语句：如果第一个if不成立，后面的if成立，将会出现SQL语句多出一个and的情况，示例如下：

```sql
select * from blog where and title like
```

这时候在所有的sql-if语句外加上\<where>标签<mark><font color=FF0000>**（似乎只有去掉前面的and的功能）**</font></mark>，即可避免如上情况。示例如下：

```xml
<select id="getTeacherByCondition" resultMap="teacherMap">
    select * from t_teacher where
    <where>
        <if test="id!=null">
            id > #{id}
        </if>
        <if test="name!=null and name!=""">
            and teacherName like #{name}
        </if>
        <if test="birth!=null">
            and birth_date > #{birth}
        </if>
    </where>
</select>
```

## <font color=FF0000>**From 2.15**</font>

**sql-trim标签用于截取字符串**
**sql-trim的属性：**

- prefix : 为整体添加一个前缀
- prefixOverrides : 去除整体字符串前面多余的字符
- suffix : 为整体添加一个后缀
- suffixOverrides : 去除整体字符串后面多余的字符

## <font color=FF0000>**From 2.16**</font>

**sql-foreach用于遍历元素**
<mark><font color=FF0000>**foreach相关属性：**</font></mark>

- collection: 指定遍历集合的key
- index: 索引
  - 如果遍历的是一个List
    - index: 指定的变量保存了当前索引
    - item: 保存当前遍历的元素的值
  - 如果遍历的是一个Map
    - index: 指定的变量就是保存了当前遍历的元素的key
    - item: 就是保存当前遍历的元素的值   
- item: 每次遍历出的元素奇异果变量名方便引用
- separator: 每次遍历元素的分隔符
- open: 以什么开始
- close：以什么结束

**示例如下：**

```xml
<!--
    实现效果：select * from t_teacher where id in (1, 2, 3, 4, 5)
-->
select * from t_teacher where id in
<foreach collection="ids" iteam="id_item" separator="," open="(" close=")")>
    #{id_item}
</foreach>
```

## <font color=FF0000>**From 2.17**</font>

**sql-choose：分支选择，相当于switch-case**
<font color=FF0000>**示例如下：**</font>

```xml
<where>
    <choose>
        <when test="id!=null">
            id=#{id}
        </when>
        <when test="name!=null and !name.equals("")">
            teacherName=#{name}
        </when>
        <when test="birth!=null">
            birth_date=#{birth}
        </when>
        <otherwise>
            1=1  <!--相当于true--> 
        </otherwise>
    </choose>
<where>
```

## <font color=FF0000>**From 2.18**</font>

**sql-set**
在update语句中，如果配合if使用，将会出现多出逗号的(,)问题；这时候可以用set来去掉逗号，示例如下：

```xml
<update id="updateTeacher">
    update t_teacher
    <set>
        <if test="name!=null and !name.equals("")">
            teacherName=#{name},
        </if>
        <if test="course!=null and !course.equals("")">
            class_name=#{course},
        </if>
        <if test="address!=null and !address.equals("")">
            address=#{address},
        </if>
        <if test="birth!=null">
            birth_date=#{birth},
        </if>
        <where>
           id=#{id}
        </where>
    </set>
</update>
```

## <font color=FF0000>**From 2.19**</font>

**mybatis中其余两个OGNL参数**

- _parameter: 代表传入来的参数
  - 传入了单个参数：_parameter就代表这个参数
  - 传入了多个参数：_parameter就代表多个参数集合起来的map
- _databaseId: 代表当前所使用的数据库厂商：如果配置了databaseIdProvider，_databaseId就有值

## <font color=FF0000>**From 3.1**</font>

**bind标签:** 用于绑定模糊查询（实现代码解耦，虽然这样做没有什么意思）
**示例如下：**

```xml
<bind name="_name" value="'%'+name+'%'"/>
<if test="name!=null && !name.equals("")">
    teacherName like #{_name} and
</if>
```

\<sql>标签用于抽取可重用的sql语句，同时\<include>标签与之配合使用
示例如下：

```xml
<sql id="selectSql">select * from t_teacher</sql>
<select id="getTeacherById" resultMap="teacherMap">
    <include refid="selectSql"></include>
    where id=#{id}
</select>
```

## <font color=FF0000>**From 3.2**</font>

**缓存机制：暂时的存储一些数据，加快系统的查询速度**
MyBatis缓存机制：就是一个Map，能保存查询出一些数据

- 一级缓存：线程级别的缓存，也称为本地缓存，也称为SqlSession级别的缓存
- 二级缓存：全局范围的缓存，除了当前缓存、SqlSession能用之外，其他也能用

## <font color=FF0000>**From 3.3**</font>

**一级缓存的机制**

只要之前查询过的数据，mybatis就会保存在一个缓存中（Map）；下次获取直接从缓存中拿

## <font color=FF0000>**From 3.4**</font>

**以及缓存失效的几种情况**

- 不同的SqlSession，使用不同的一级缓存
  只有在同一个SqlSession期间查询到的数据会保存在这个SqlSession的缓存中；下次使用这个SqlSession查询会从缓存中拿
- 同一个方法，不同的参数，由于可能之前没有查过，所有还会发新的sql
- 在这个SqlSession期间执行任何一次<font color=FF0000>**增删改**</font>操作，增删改操作会把缓存清空
- 手动清空缓存，使用了SqlSession.clearCache()方法
  每次查询，先看一级缓存中有没有，如果没有就去发送新的sql；<font color=FF0000>**每个SqlSession拥有自己的一级缓存**

## <font color=FF0000>**From 3.5**</font>

**二级缓存**
一级缓存在SqlSession关闭或被提交之后会被放到二级缓存中，同时mybatis默认不适用二级缓存。
**二级缓存的使用**

- 全局配置(mybatis.config)开启二级缓存
  
  ```
  <settings>
    <setting name="cacheEnabled" value="true"/>
  </settings>
  ```
- 配置某个需要开启二级缓存JavaBean对应的mapper.xml文件，使其使用二级缓存
  
  ```xml
  <mapper namespace="com.atguigu.dao.TeacherDao">
    <!--这里仅仅表示开启，关于cache更多的属性和配置见3.6-->
    <cache></cache>
    ...
  </mapper>
  ```
- JavaBean继承Serializable接口

## <font color=FF0000>**From 3.6**</font>

**缓存相关属性**

- eviction 缓存回收策略
  - LRU: 最近最少使用，该策略是<font color=FF0000>**默认策略**</font>
  - FIFO：先进先出
  - SOFT: 软引用，移除基于垃圾回收器状态和软引用规则的对象
  - WEAK: 弱引用，更积极地移除基于垃圾收集器状态和弱引用规则的对象
- flushInterval 刷新间隔，单位为毫秒
  - 默认情况是不是设置，即没有刷新间隔，缓存仅仅调用语句时刷新
- size 引用数目，正整数
  - 默认情况最多可以存储多少个对象，太大容易导致内存溢出 
- readOnly 只读，true/false
  - true：只读缓存，会给所有调用者返回缓存对象的相同实例。因此这些对象不能被修改。这提供了很重要的性能优势
  - false：读写缓存，会返回缓存对象的拷贝（通过序列化）。这会慢一些，但是安全，因此<font color=FF0000>**默认是false**</font>

<mark>**一级缓存二级缓存总结**</font></mark>

- <font color=FF0000>**不会出现一级缓存和二级缓存中有同一个数据的情况**</font>
  - 一级缓存中的数据：<mark>**二级缓存中没有此数据，就会看一级缓存**</mark>，以及缓存中没有就会去查数据库，<mark>**在数据库查询之后的结果放在一级缓存中**</mark>
  - 二级缓存中的数据：一级缓存关闭了，数据将会放到二级缓存中
- <font color=FF0000>**任何时候都是先看二级缓存，如果没有，再看一级缓存；如果都没有，最后查询数据库**</font>
- <font color=FF0000>**每一个mapper都有自己的二级缓存，每一个SqlSession都有自己的一级缓存**</font>

## <font color=FF0000>**From 3.8**</font>

**缓存相关设置**

- 全局setting的<font color=FF0000>**cacheEnable**</font>：<mark>配置二级缓存的开关</mark>，一级缓存一直是打开的
- select标签的<font color=FF0000>**useCache**</font>属性：<mark>配置这个select是否使用二级缓存</mark>，一级缓存一直是使用的
- sql标签的<font color=FF0000>**flushCache**</font>属性：增删改默认flushCache=true。sql执行以后，<font color=FF0000>会同时清空一级和二级缓存</font>。查询默认flushCache=false
- sqlSession.<font color=FF0000>**clearCache**</font>()：只是用来清除一级缓存
- 当在一个作用域（一级缓存Session/二级缓存Namespace）进行Create / Update / Delete操作后，默认该作用域下<font color=FF0000>**所有select中的缓存将被clear**</font>

## <font color=FF0000>**From 3.9**</font>

**整合第三方缓存**
由于mybatis只是使用一个map来实现缓存，所以功能或性能上会不足，所以mybatis也提供了缓存接口，供其他缓存库使用与实现。
</font>