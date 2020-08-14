# Maven备忘录

## <font color=FF0000>maven解决了哪些需求/痛点</font>
> <font size=3>个人的一个小感受，<mark>学习一个新技术，应该以历史的眼光开看待这个新技术出现的原因，以及帮我们解决了什么问题</mark>。我们来回忆一下没有Maven的日子是怎么样的？</br>
> 1. 开发一个项目，需要用别人写好的jar包，我们先把开源的jar包下载下来放到项目的lib目录下，并把这个目录添加到CLASSPATH（告诉Java执行环境，在哪些目录下可以找到你要执行的Java程序需要的类或者包）
> 2. 我们下载了a.jar发现a.jar还需要依赖b.jar，结果又去把b.jar包下载下来开始运行。
> 3. 如果运气够好，我们的项目在添加完所有的依赖后，能正产运行了。如果运气差点，还会遇到版本的问题，例如a.jar在调用b.jar的时候发现b.jar根本没有这个方法，在别的版本中才有，现在好了，光找依赖和适配版本就能花上不少时间。
> 4. 而且我们往git上上传代码的时候，还必须把这些lib都上传上去。别人下载我们的代码时也必须把lib下载下来，这个真心耗费时间</br> 
>
> <mark>**这时候Maven作为Java世界的包管理工具出现了，当然Java世界还有其他包管理工具，例如gradle等。就像yum是Linux世界的包管理工具，webpack是前端世界的包管理工具一样**</mark></br>



## <font color=FF0000>Maven寻找jar包过程</font>

> <font size=3>Maven找jar包的过程是这样的：<mark>先在本地仓库找，找不到再去私服（如果配置了的话），再找不到去中央仓库</mark>（http://repo1.maven.org/maven2/，maven团队负责维护）
</font>

**以上节选自博文：**[**maven仓库详解**](https://blog.csdn.net/weixin_41325595/article/details/93617821)</br></font>



maven在打包时，会自动把application-*.properties中的「后缀\*」作为当前项目的profile



## <font color=FF0000>将IDEA中的Maven的镜像设置为国内镜像</font>

**打开`settings.xml`，其中查找`<mirrors>`标签**

**在`<mirrors>`标签的注释后面 加下面语句：**

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