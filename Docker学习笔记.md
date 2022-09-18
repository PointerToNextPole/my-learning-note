

# Docker学习笔记



#### Docker介绍

##### 痛点

<font color=dodgerblue>软件开发最大的麻烦事之一，就是环境配置</font>。用户计算机的环境都不相同，你怎么知道自家的软件，能在那些机器跑起来？

<font color=dodgerblue>用户必须保证两件事</font>：<font color=LightSeaGreen>操作系统的设置，各种库和组件的安装。只有它们都正确，软件才能运行</font>。举例来说，安装一个 Python 应用，计算机必须有 Python 引擎，还必须有各种依赖，可能还要配置环境变量。

环境配置如此麻烦，换一台机器，就要重来一次，旷日费时。很多人想到，能不能从根本上解决问题，软件可以带环境安装？也就是说，安装的时候，把原始环境一模一样地复制过来。

##### 虚拟机

<font color=dodgerblue>虚拟机 ( virtual machine ) 就是带环境安装的一种解决方案</font>。它可以在一种操作系统里面运行另一种操作系统，比如在 Windows 系统里面运行 Linux 系统。<font color=lightSeaGreen> 应用程序对此毫无感知，因为虚拟机看上去跟真实系统一模一样，而对于底层系统来说，虚拟机就是一个普通文件，不需要了就删掉，对其他部分毫无影响</font>。

虽然用户可以通过虚拟机还原软件的原始环境。但是，<font color=dodgerBlue>**这个方案有几个缺点**</font>

- **资源占用多** ：<font color=FF0000>**虚拟机会独占一部分内存和硬盘空间。它运行的时候，其他程序就不能使用这些资源了**</font>。哪怕虚拟机里面的应用程序，真正使用的内存只有 1MB，虚拟机依然需要几百 MB 的内存才能运行。

- **冗余步骤多** ：<font color=FF0000>虚拟机是完整的操作系统，**一些系统级别的操作步骤，往往无法跳过**，比如用户登录</font>。

- **启动慢** ：<font color=FF0000>启动操作系统需要多久，启动虚拟机就需要多久</font>。可能要等几分钟，应用程序才能真正运行。

##### Linux 容器

由于虚拟机存在这些缺点，<font color=dodgerBlue>**Linux 发展出了另一种虚拟化技术：Linux 容器**</font>（ Linux Containers，缩写为 LXC ）。

Linux 容器<font color=fuchsia>不是模拟一个完整的操作系统，而是**对进程进行隔离**</font>。<font color=red>或者说，在正常进程的外面套了一个保护层</font>。对于容器里面的进程来说，它接触到的各种资源都是虚拟的，从而实现与底层系统的隔离。

**由于容器是进程级别的，相比虚拟机有很多优势：**

- **启动快** ：<font color=fuchsia>容器里面的应用，<font size=4>**直接就是底层系统的一个进程**</font>，而不是虚拟机内部的进程</font>。所以，<font color=lightSeaGreen>启动容器相当于启动本机的一个进程，而不是启动一个操作系统，速度就快很多</font>。

- **资源占用少** ：<font color=FF0000>**容器只占用需要的资源，不占用那些没有用到的资源** </font>；虚拟机由于是完整的操作系统，不可避免要占用所有资源。<font color=FF0000> 另外，**多个容器可以共享资源，虚拟机都是独享资源** </font>。

- **体积小** ：<font color=FF0000> **容器只要包含用到的组件即可**，而虚拟机是整个操作系统的打包</font>，<font color=LightSeaGreen>所以容器文件比虚拟机文件要小很多</font>。

总之，容器有点像轻量级的虚拟机，能够提供虚拟化的环境，但是成本开销小得多。

##### 什么是 Docker

<font color=fuchsia>Docker 属于 Linux 容器的一种封装，提供简单易用的容器使用接口</font>。它是目前最流行的 Linux 容器解决方案。

<font color=FF0000> Docker 将应用程序与该程序的依赖，打包在一个文件里面。运行这个文件，就会生成一个虚拟容器。程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样</font>。有了 Docker，就不用担心环境问题。

总体来说，Docker 的接口相当简单，用户可以方便地创建和使用容器，把自己的应用放入容器。<font color=red>容器还可以进行版本管理、复制、分享、修改，就像管理普通的代码一样</font>。

##### Docker 的用途

**Docker 的主要用途，目前有三大类：**

- <font color=FF0000> **提供一次性的环境**</font>。比如，本地测试他人的软件、持续集成的时候提供单元测试和构建的环境。

- <font color=FF0000> **提供弹性的云服务**</font>。<font color=FF0000> 因为 Docker 容器可以随开随关，很适合动态扩容和缩容</font>。

- <font color=FF0000> **组建微服务架构**</font>。<font color=FF0000> 通过多个容器，一台机器可以跑多个服务，因此在本机就可以模拟出微服务架构</font>。

##### 安装后

docker 查看信息

```sh
docker version # 详细信息
docker info    # 更详细的信息
docker -v      # 简单的版本号
```

Docker 需要用户具有 sudo 权限，为了避免每次命令都输入`sudo`，可以把用户加入 Docker 用户组（[官方文档](https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user)）。

```sh
sudo usermod -aG docker $USER
```

> 👀 注：对于 必须要使用 sudo 权限才能使用的 工具，似乎也可以使用该方法？

##### image 文件

<font color=fuchsia>**Docker 把应用程序及其依赖，打包在 image 文件里面**</font>。只有通过这个文件，才能生成 Docker 容器。image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。<font color=fuchsia>同一个 image 文件，可以生成多个同时运行的容器实例</font>。

image 是二进制文件。<font color=red>实际开发中，一个 image 文件往往通过继承另一个 image 文件，加上一些个性化设置而生成</font>。举例来说，<font color=LightSeaGreen>你可以在 Ubuntu 的 image 基础上，往里面加入 Apache 服务器，形成你的 image</font>。

```sh
# 列出本机的所有 image 文件。
$ docker image ls

# 删除 image 文件
$ docker image rm [imageName]
```

<font color=red>image 文件是通用的，一台机器的 image 文件拷贝到另一台机器，照样可以使用</font>。一般来说，为了节省时间，我们应该尽量使用别人制作好的 image 文件，而不是自己制作。即使要定制，也应该基于别人的 image 文件进行加工，而不是从零开始制作。

为了方便共享，image 文件制作完成后，可以上传到网上的仓库。<font color=red>Docker 的官方仓库 [Docker Hub](https://hub.docker.com/) 是最重要、最常用的 image 仓库</font>。此外，出售自己制作的 image 文件也是可以的。

##### hello-world 示例

运行下面的命令，将 image 文件从仓库抓取到本地。

```bash
$ docker image pull library/hello-world
```

上面代码中，`docker image pull` 是抓取 image 文件的命令。`library/hello-world` 是 image 文件在仓库里面的位置，其中 `library`是 image 文件所在的组，`hello-world` 是 image 文件的名字。

由于 Docker 官方提供的 image 文件，都放在 [`library`](https://hub.docker.com/r/library/) 组里面，所以它的是默认组，可以省略。因此，上面的命令可以写成下面这样：

```sh
$ docker image pull hello-world
```

抓取成功以后，就可以在本机看到这个 image 文件了：

```sh
$ docker image ls
```

现在，运行这个 image 文件：

```sh
$ docker container run hello-world
```

`docker container run` 命令会从 image 文件，生成一个正在运行的容器实例。

注意：<font color=red>`docker container run` 命令具有自动抓取 image 文件的功能</font>。<font color=LightSeaGreen>如果发现本地没有指定的 image 文件，就会从仓库自动抓取</font>。因此，前面的 `docker image pull`命令并不是必需的步骤。

如果运行成功，你会在屏幕上读到下面的输出：

<img src="https://s2.loli.net/2022/09/18/P7wJfjzqhMSNrsk.png" alt="image-20220918201006739" style="zoom:45%;" />

输出这段提示以后，`hello-world` 就会停止运行，容器自动终止。

有些容器不会自动终止，因为提供的是服务。比如，安装运行 Ubuntu 的 image，就可以在命令行体验 Ubuntu 系统。

```sh
$ docker container run -it ubuntu bash
```

<font color=red>对于那些不会自动终止的容器，必须使用 `docker container kill` 命令手动终止</font>：

```sh
$ docker container kill [containID]
```

##### 容器文件 container

<font color=red>**image 文件生成的容器实例，本身也是一个文件，称为容器文件**</font>。也就是说，<font color=fuchsia>一旦容器生成，就会同时存在两个文件：image 文件和容器文件</font>（👀 感觉 有点类似于 类和对象的关系... ? ）。而且关闭容器并不会删除容器文件，只是容器停止运行而已。

```sh
# 列出本机正在运行的容器
$ docker container ls

# 列出本机所有容器，包括终止运行的容器
$ docker container ls --all
```

> 👀 补充：`docker ps` 和 `docker container ls`  功能相同，但是语义更明确，简化了Docker的用法，所以更推荐使用新的写法

上面命令的输出结果之中，包括容器的 ID。很多地方都需要提供这个 ID，比如上面终止容器运行的 `docker container kill` 命令。

<font color=fuchsia><font size=4>**终止运行的容器文件，依然会占据硬盘空间**</font>，可以使用 `docker container rm` 命令删除</font>：

```sh
$ docker container rm [containerID]
```

运行上面的命令之后，再使用 `docker container ls --all` 命令，就会发现被删除的容器文件已经消失了。

##### Dockerfile 文件

学会使用 image 文件以后，接下来的问题就是，如何可以生成 image 文件？如果你要推广自己的软件，势必要自己制作 image 文件。

这就需要用到 Dockerfile 文件。它<font color=red>是一个文本文件，用来配置 image</font>。<font color=fuchsia>Docker 根据 该文件 **生成** 二进制的 **image 文件**</font>。

摘自：[阮一峰 - Docker 入门教程](https://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)



-i interactive

-t terminal

docker attach

ctrl + p -> ctrl + q

docker rmi

-d : detached mode

-v 映射