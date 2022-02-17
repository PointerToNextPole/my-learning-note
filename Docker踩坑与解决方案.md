

# Docker学习笔记



## Docker介绍

#### **痛点**

<mark>软件开发最大的麻烦事之一，就是环境配置</mark>。用户计算机的环境都不相同，你怎么知道自家的软件，能在那些机器跑起来？

<mark>用户必须保证两件事：操作系统的设置，各种库和组件的安装。只有它们都正确，软件才能运行</mark>。举例来说，安装一个 Python 应用，计算机必须有 Python 引擎，还必须有各种依赖，可能还要配置环境变量。

环境配置如此麻烦，换一台机器，就要重来一次，旷日费时。很多人想到，能不能从根本上解决问题，软件可以带环境安装？也就是说，安装的时候，把原始环境一模一样地复制过来。

#### 虚拟机

<mark>虚拟机（virtual machine）就是带环境安装的一种解决方案</mark>。它可以在一种操作系统里面运行另一种操作系统，比如在 Windows 系统里面运行 Linux 系统。<font color=FF0000> 应用程序对此毫无感知，因为虚拟机看上去跟真实系统一模一样，而对于底层系统来说，虚拟机就是一个普通文件，不需要了就删掉，对其他部分毫无影响</font>。

虽然用户可以通过虚拟机还原软件的原始环境。但是，**这个方案有几个缺点**。

- **资源占用多：**<font color=FF0000> **虚拟机会独占一部分内存和硬盘空间。它运行的时候，其他程序就不能使用这些资源了**</font>。哪怕虚拟机里面的应用程序，真正使用的内存只有 1MB，虚拟机依然需要几百 MB 的内存才能运行。

- **冗余步骤多：**<font color=FF0000> 虚拟机是完整的操作系统，**一些系统级别的操作步骤，往往无法跳过**，比如用户登录</font>。

- **启动慢：**<font color=FF0000> **启动操作系统需要多久，启动虚拟机就需要多久**</font>。可能要等几分钟，应用程序才能真正运行。

#### Linux 容器

<font color=FF0000> **由于虚拟机存在这些缺点，Linux 发展出了另一种虚拟化技术：Linux 容器**</font>（Linux Containers，缩写为 LXC）。

Linux 容器不是模拟一个完整的操作系统，而是对进程进行隔离。或者说，在正常进程的外面套了一个保护层。对于容器里面的进程来说，它接触到的各种资源都是虚拟的，从而实现与底层系统的隔离。

**由于容器是进程级别的，相比虚拟机有很多优势。**

- **启动快：**<font color=FF0000> 容器里面的应用，<font size=4>**直接就是底层系统的一个进程**</font>，而不是虚拟机内部的进程</font>。所以，<mark>启动容器相当于启动本机的一个进程，而不是启动一个操作系统，速度就快很多</mark>。

- **资源占用少：**<font color=FF0000> **容器只占用需要的资源，不占用那些没有用到的资源** </font>；虚拟机由于是完整的操作系统，不可避免要占用所有资源。<font color=FF0000> 另外，**多个容器可以共享资源，虚拟机都是独享资源** </font>。

- **体积小：**<font color=FF0000> **容器只要包含用到的组件即可**，而虚拟机是整个操作系统的打包</font>，<mark>所以容器文件比虚拟机文件要小很多</mark>。

总之，容器有点像轻量级的虚拟机，能够提供虚拟化的环境，但是成本开销小得多。

#### 什么是Docker

<font color=FF0000> Docker 属于 Linux 容器的一种封装，提供简单易用的容器使用接口</font>。它是目前最流行的 Linux 容器解决方案。

<font color=FF0000> Docker 将应用程序与该程序的依赖，打包在一个文件里面。运行这个文件，就会生成一个虚拟容器。程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样</font>。有了 Docker，就不用担心环境问题。

总体来说，Docker 的接口相当简单，用户可以方便地创建和使用容器，把自己的应用放入容器。容器还可以进行版本管理、复制、分享、修改，就像管理普通的代码一样。

#### Docker 的用途

**Docker 的主要用途，目前有三大类**。

- <font color=FF0000> **提供一次性的环境**</font>。比如，本地测试他人的软件、持续集成的时候提供单元测试和构建的环境。

- <font color=FF0000> **提供弹性的云服务**</font>。<font color=FF0000> 因为 Docker 容器可以随开随关，很适合动态扩容和缩容</font>。

- <font color=FF0000> **组建微服务架构**</font>。<font color=FF0000> 通过多个容器，一台机器可以跑多个服务，因此在本机就可以模拟出微服务架构</font>。



## Docker踩坑与解决方案

键入docker命令时（比如：`docker ps`）出现如下提示：

```
error during connect: Get http://%2F%2F.%2Fpipe%2Fdocker_engine/v1.39/containers/json: open //./pipe/docker_engine: The system cannot find the file specified. In the default daemon configuration on Windows, the docker client must be run elevated to connect. This error may also indicate that the docker daemon is not running.
```

**解决方案：**

- 进入docker安装地址 `C:\Program Files\Docker\Docker`（默认安装地址为`C:\Program Files\Docker\Docker`）
- 键入`./DockerCli.exe -SwitchDaemon`

就正常了。

参考：[[docker][win10]安装的坑](https://www.jianshu.com/p/09d53c822cf8)



#### 关于Docker for Windows docker pull的镜像存储位置

参见如下两篇：

[思否 - Docker for Windows docker pull 镜像到哪里去了](https://segmentfault.com/q/1010000006745913)

[docker for windows pull镜像文件的安装位置改变方法](https://blog.csdn.net/StemQ/article/details/53150939)