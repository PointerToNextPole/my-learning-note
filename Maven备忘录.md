# Maven 备忘录



#### maven 解决了哪些需求/痛点
个人的一个小感受，<font color=dodgerBlue>学习一个新技术，应该以历史的眼光开看待这个新技术出现的原因，以及帮我们解决了什么问题</font>。我们来回忆一下没有Maven的日子是怎么样的？

1. 开发一个项目，需要用别人写好的jar包，我们先把开源的jar包下载下来放到项目的lib目录下，并把这个目录添加到CLASSPATH（告诉Java执行环境，在哪些目录下可以找到你要执行的Java程序需要的类或者包）
2. 我们下载了a.jar发现a.jar还需要依赖b.jar，结果又去把b.jar包下载下来开始运行。
3. 如果运气够好，我们的项目在添加完所有的依赖后，能正产运行了。如果运气差点，还会遇到版本的问题，例如a.jar在调用b.jar的时候发现b.jar根本没有这个方法，在别的版本中才有，现在好了，光找依赖和适配版本就能花上不少时间。
4. 而且我们往git上上传代码的时候，还必须把这些lib都上传上去。别人下载我们的代码时也必须把lib下载下来，这个真心耗费时间

这时候Maven作为Java世界的包管理工具出现了，当然Java世界还有其他包管理工具，例如gradle等。就像yum是Linux世界的包管理工具，webpack是前端世界的包管理工具一样

摘自：[把Maven的架构，用法，坑点介绍的清清楚楚](https://cloud.tencent.com/developer/article/1429941)



#### Maven 寻找jar包过程

<font size=3>Maven找jar包的过程是这样的：<mark>先在本地仓库找，找不到再去私服（如果配置了的话），再找不到去中央仓库</mark>（http://repo1.maven.org/maven2/，maven团队负责维护）
</font>

摘自：[maven仓库详解](https://blog.csdn.net/weixin_41325595/article/details/93617821)



maven在打包时，会自动把 `application-*.properties` 中的「后缀\*」作为当前项目的 profile



#### 将IDEA中的Maven的镜像设置为国内镜像

打开 `settings.xml` ，其中查找 `<mirrors>` 标签

在 `<mirrors>` 标签的注释后面 加下面语句：

```xml
<mirror>
  <id>aliyun-public</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun public</name>
  <url>https://maven.aliyun.com/repository/public</url>
</mirror>

<mirror>
  <id>aliyun-central</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun central</name>
  <url>https://maven.aliyun.com/repository/central</url>
</mirror>

<mirror>
  <id>aliyun-spring</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun spring</name>
  <url>https://maven.aliyun.com/repository/spring</url>
</mirror>

<mirror>
  <id>aliyun-spring-plugin</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun spring-plugin</name>
  <url>https://maven.aliyun.com/repository/spring-plugin</url>
</mirror>
```

摘自：[IntelliJ IDEA设置自带的maven为国内镜像](https://www.cnblogs.com/com3/p/12408669.html)



#### 约定配置

Maven 提倡使用一个共同的标准目录结构，Maven 使用<font color=FF0000>约定优于配置</font>的原则，<font color=FF0000>大家尽可能的遵守这样的目录结构</font>。如下所示：

| 目录                               | 目的                                                         |
| :--------------------------------- | :----------------------------------------------------------- |
| ${basedir}                         | 存放pom.xml和所有的子目录                                    |
| ${basedir}/src/main/java           | 项目的java源代码                                             |
| ${basedir}/src/main/resources      | 项目的资源，比如说property文件，springmvc.xml                |
| ${basedir}/src/test/java           | 项目的测试类，比如说Junit代码                                |
| ${basedir}/src/test/resources      | 测试用的资源                                                 |
| ${basedir}/src/main/webapp/WEB-INF | web应用文件目录，web项目的信息，比如存放web.xml、本地图片、jsp视图页面 |
| ${basedir}/target                  | 打包输出目录                                                 |
| ${basedir}/target/classes          | 编译输出目录                                                 |
| ${basedir}/target/test-classes     | 测试编译输出目录                                             |
| Test.java                          | Maven只会自动运行符合该命名规则的测试类                      |
| ~/.m2/repository                   | Maven默认的本地仓库目录位置                                  |



#### pom.xml结构

```xml
<project xmlns = "http://maven.apache.org/POM/4.0.0"
    xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation = "http://maven.apache.org/POM/4.0.0
    http://maven.apache.org/xsd/maven-4.0.0.xsd">
 
    <!-- 模型版本 -->
    <modelVersion>4.0.0</modelVersion>
    <!-- 公司或者组织的唯一标志，并且配置时生成的路径也是由此生成， 如com.companyname.project-group，maven会将该项目打成的jar包放本地路径：/com/companyname/project-group -->
    <groupId>com.companyname.project-group</groupId>
 
    <!-- 项目的唯一ID，一个groupId下面可能多个项目，就是靠artifactId来区分的 -->
    <artifactId>project</artifactId>
 
    <!-- 版本号 -->
    <version>1.0</version>
</project>
```

所有 POM 文件都需要 project 元素和三个必需字段：groupId，artifactId，version。

| 节点         | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| project      | 工程的根标签。                                               |
| modelVersion | 模型版本需要设置为 4.0。                                     |
| groupId      | 这是<font color=FF0000>工程组的标识</font>。它在一个组织或者项目中通常是唯一的。例如，一个银行组织 com.companyname.project-group 拥有所有的和银行相关的项目。 |
| artifactId   | 这是<font color=FF0000>工程的标识</font>。它通常是工程的名称。例如，消费者银行。groupId 和 artifactId 一起定义了 artifact 在仓库中的位置。 |
| version      | 这是<font color=FF0000>工程的版本号</font>。在 artifact 的仓库中，它用来区分不同的版本。 |



#### Maven构建生命周期

Maven 构建生命周期定义了一个项目构建跟发布的过程。

一个典型的 Maven 构建（build）生命周期是由以下几个阶段的序列组成的：

**开始 --> validate --> compile  --> test --> package --> verify --> install --> deploy --> 开始**

| 阶段          | 处理     | 描述                                                     |
| :------------ | :------- | :------------------------------------------------------- |
| 验证 validate | 验证项目 | 验证项目是否正确且所有必须信息是可用的                   |
| 编译 compile  | 执行编译 | 源代码编译在此阶段完成                                   |
| 测试 test     | 测试     | 使用适当的单元测试框架（例如JUnit）运行测试。            |
| 包装 package  | 打包     | 创建JAR/WAR包如在 pom.xml 中定义提及的包                 |
| 检查 verify   | 检查     | 对集成测试的结果进行检查，以保证质量达标                 |
| 安装 install  | 安装     | 安装打包的项目到本地仓库，以供其他项目使用               |
| 部署 deploy   | 部署     | 拷贝最终的工程包到远程仓库中，以共享给其他开发人员和工程 |