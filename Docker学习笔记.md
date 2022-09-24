

# Docker 学习笔记



#### Docker 特性

##### 核心概念

1. `Build, Ship and Run`（搭建、运输、运行）

2. `Build once, Run anywhere`（一次搭建，处处运行）

   > 👀 注：类似于 Java 的 JVM

3. `Docker` 本身并不是容器，它是创建容器的工具，是应用容器引擎

4. <font color=dodgerBlue>`Docker` 三大核心概念</font>，分别是：<font color=red>镜像 `Image`，容器 `Container`、仓库 `Repository`</font>

5. <font color=fuchsia>`Docker` 技术使用 `Linux` 内核和内核功能（例如 `Cgroups` 和 `namespaces`）来分隔进程，以便各进程相互独立运行</font>

6. 由于 `Namespace` 和 `Cgroups` 功能仅在 `Linux` 上可用，因此容器无法在其他操作系统上运行。那么 `Docker` 如何在 `macOS` 或 `Windows` 上运行？ `Docker` 实际上使用了一个技巧，并在非 `Linux` 操作系统上安装 `Linux` 虚拟机，然后在虚拟机内运行容器。

7. 镜像是一个可执行包，其包含运行应用程序所需的代码、运行时、库、环境变量和配置文件，容器是镜像的**运行时实例**。

摘自：[Docker 边学边用](http://jartto.wang/2020/07/04/learn-docker/)



#### Docker 介绍

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
$ docker images # 👀 可以起到同样的作用，而且更加简洁

# 删除 image 文件
$ docker image rm [imageName]
$ docker rm [imageName] # 👀 可以起到同样的作用，而且更加简洁
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

> 👀 注：这里的 container 可以省略，即：`docker run hello-world` 。类似的也有：`docker kill` 、`docker stop` 、`docker start` 、`docker exec` 、`docker logs` 、`docker cp` 、`docker rm` 等，这种写法也更常见的。

`docker container run` 命令会从 image 文件，生成一个正在运行的容器实例。

> 👀 补充：`docker run` 如果没有制定名称，则 docker 会给他一个名字；另外，可以通过 `--name` 选项给 当前 container 自定义一个名字：`--name targetName`

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

> 👀 补充：`docker ps` 和 `docker container ls`  功能相同，但是语义更明确，简化了用法，更推荐使用 `docekr ps`

上面命令的输出结果之中，包括容器的 ID。很多地方都需要提供这个 ID，比如上面终止容器运行的 `docker container kill` 命令。

<font color=fuchsia><font size=4>**终止运行的容器文件，依然会占据硬盘空间**</font>，可以使用 `docker container rm` 命令删除</font>：

```sh
$ docker container rm [containerID]
```

运行上面的命令之后，再使用 `docker container ls --all` 命令，就会发现被删除的容器文件已经消失了。

> 👀 注：之前在 container 中的所有操作也会消失。以 ubuntu 为例，`docker run -it ubuntu` 之后，生了一个 ubuntu 的 container，使用过程中安装了 node 环境；如果之后 `docker container rm [ubuntu-container-ID]` 后；再次运行 `docker run -it ubuntu` ，会发现 “当前 contanier” 中没有 node 环境了，随着上一个 container 一起被删除了。

##### Dockerfile 文件

学会使用 image 文件以后，接下来的问题就是，如何可以生成 image 文件？如果你要推广自己的软件，势必要自己制作 image 文件。

这就需要用到 Dockerfile 文件。它<font color=red>是一个文本文件，用来配置 image</font>。<font color=fuchsia>Docker 根据 该文件 **生成** 二进制的 **image 文件**</font>。

##### 制作自己的 Docker 容器

> 👀 注：下面的内容有部分省略，有不理解的地方见原文

以阮一峰写的 [koa-demos](https://www.ruanyifeng.com/blog/2017/08/koa.html) 为例。

###### 编写 .dockerignore 文件

<font color=dodgerBlue>在项目的根目录下，新建文本文件 `.dockerignore`</font> （和 `.gitignore` 类似），标识 文件/路径 “不要打包进入 image 文件”：

```.gitnode_modulesnpm-debug.log
.git
node_modules
npm-debug.log
```

###### 编写 Dockerfile 文件

<font color=dodgerBlue>在项目的根目录下，新建文本文件 `Dockerfile`</font>，写入如下内容：

```dockerfile
FROM node:8.4
COPY . /app
WORKDIR /app
RUN npm install --registry=https://registry.npm.taobao.org
EXPOSE 3000
```

<font color=dodgerBlue>上面代码一共五行，含义如下：</font>

- `FROM node:8.4` ：<font color=red>该 image 文件继承官方的 node image</font>，冒号表示标签，这里标签是 `8.4`，即 8.4 版本的 node
- `COPY . /app` ：<font color=red>将当前目录下的所有文件</font>（除了 `.dockerignore` 排除的路径），<font color=red>都拷贝进入 image 文件的 `/app` 目录</font>
- `WORKDIR /app` ：<font color=fuchsia>指定接下来的工作路径为 `/app`</font>
- `RUN npm install` ：<font color=red>在 `/app` 目录下</font>，<font color=fuchsia>运行 `npm install` 命令安装依赖</font>。注意，<font color=fuchsia>安装后所有的依赖，都将打包进入 image 文件</font>。
- `EXPOSE 3000` ：<font color=fuchsia>将容器 3000 端口暴露出来， 允许外部连接这个端口</font>

###### 创建 image 文件

<font color=red>有了 Dockerfile 文件以后，就可以使用 `docker image build` 命令创建 image 文件了</font>。

```sh
$ docker image build -t koa-demo .
# 或者
$ docker image build -t koa-demo:0.0.1 .
```

上面代码中，<font color=red>**`-t` 参数用来指定 image 文件的名字**</font>，<font color=LightSeaGreen>后面还可以用冒号指定标签</font>。如果不指定，默认的标签就是`latest`。<font color=fuchsia>最后的 `.` 表示 Dockerfile 文件所在的路径</font>（ 👀 路径是必填的（没有默认值），注意下，不要漏掉），上例是当前路径，所以是一个点。

<font color=red>**如果运行成功，运行 `docker ps` 就可以看到新生成的 image 文件 `koa-demo` 了**</font>。

###### 生成容器

`docker container run` 命令会从 image 文件生成容器。

```sh
$ docker container run -p 8000:3000 -it koa-demo /bin/bash
# 或者
$ docker container run -p 8000:3000 -it koa-demo:0.0.1 /bin/bash # 👀 多了一个版本号
```

<font color=dodgerBlue>上面命令的各个参数含义如下：</font>

- `-p` 参数 ：<font color=red>容器的 3000 端口映射</font>（ 👀 影响 ）<font color=red>到本机的 8000 端口</font>

  > 👀 注：<font color=LightSeaGreen>因为本机是无法访问 docker 的端口的，必须要做映射</font>

- `-it` 参数 ：容器的 Shell 映射到当前的 Shell，然后你在本机窗口输入的命令，就会传入容器

  > 👀 注：`-i` 意为 interactive 交互，表示：以 “交互模式” 运行容器；`-t` 意为 terminal 命令行，表示：为容器重新分配一个伪输入终端。
  >
  > 👀 补充：除了上面所说的 “交互模式”，还有 “detached mode”，即 “分离模式”，用 `-d` 来启用。作用是：后台运行容器，并返回 “容器ID”；类似于 `docker start [containerID]` 的效果。

- `koa-demo:0.0.1` ：image 文件的名字（如果有标签，还需要提供标签，默认是 latest 标签）

- `/bin/bash` ：<font color=fuchsia>**容器启动以后，内部第一个执行的命令**</font>。这里是启动 Bash，保证用户可以使用 Shell

  > 👀 注：之所以 启用 bash、使用 shell， 是为了后面使用 node 等命令行工具。

如果一切正常，运行上面的命令以后，就会返回一个命令行提示符：

```sh
root@66d80f4aaf1e:/app#
```

这表示你已经在容器里面了，返回的提示符就是容器内部的 Shell 提示符。执行下面的命令：

```sh
root@66d80f4aaf1e:/app# node demos/01.js
```

这时，（ 👀 image 内置的）Koa 框架已经运行起来了。打开本机的浏览器，访问 http://127.0.0.1:8000，网页显示"Not Found"，这是因为这个 [demo](https://github.com/ruanyf/koa-demos/blob/master/demos/01.js) 没有写路由。

这个例子中，<font color=red>Node 进程运行在 Docker 容器的虚拟环境里面</font>，<font color=fuchsia>进程接触到的文件系统和网络接口都是虚拟的，与本机的文件系统和网络接口是隔离的，因此需要定义容器与物理机的端口映射 ( map )</font>

###### 终止容器

现在，在容器的命令行，按下 `Ctrl + c` 停止 Node 进程，然后按下 `Ctrl + d` （或者输入 exit）退出容器。此外，也可以用 `docker container kill` 终止容器运行。

```bash
# 在本机的另一个终端窗口，查出容器的 ID
$ docker container ls
$ docker ps # 更好的选择

# 停止指定的容器运行
$ docker container kill [containerID]
```

容器停止运行之后，并不会消失，用下面的命令删除容器文件。

```sh
# 查出容器的 ID
$ docker container ls --all
$ docker ps -a # 更好的选择

# 删除指定的容器文件
$ docker container rm [containerID]
```

也可以<font color=LightSeaGreen>使用 `docker container run` 命令的 `--rm` 参数，**在容器终止运行后自动删除容器文件**</font>：

```sh
$ docker container run --rm -p 8000:3000 -it koa-demo /bin/bash
```

> 👀 补充：可以使用 `Ctrl + P` + `Ctrl + Q` 返回本机命令行，而不退出当前 container 的运行。
>
> 如果之后还想运行上一次的 container（而不是再新建一个），可以使用
>
> ```sh
> # 启动目标 containerID
> $ docker start [containerID]
> 
> # 进入目标 containerID，比如进入 ubuntu 的 bash
> $ docker attach [containerID]
> # 或者，似乎是更好的方法
> docker exec -it [containerID]
> ```

###### CMD 命令

上面的例子中，容器启动以后，需要手动输入命令 `node demos/01.js`。<font color=fuchsia>可以把这个命令写在 Dockerfile 里面，这样容器启动以后，这个命令就已经执行了，不用再手动输入了</font>。

```bash
FROM node:8.4
COPY . /app
WORKDIR /app
RUN npm install --registry=https://registry.npm.taobao.org
EXPOSE 3000
CMD node demos/01.js
```

<font color=dodgerBlue>上面的 Dockerfile 里面，多了最后一行 `CMD node demos/01.js`</font> ，它<font color=red>表示容器启动后自动执行 `node demos/01.js`</font> 。

你可能会问，<font color=dodgerBlue>**`RUN` 命令与 `CMD` 命令的区别在哪里？**</font>简单说，<font color=fuchsia>`RUN` 命令 <font size=4>**在 image 文件的构建阶段执行，执行结果都会打包进入 image 文件**</font></font>；<font color=fuchsia>`CMD` 命令则<font size=4>**是在容器启动后执行**</font></font>。另外，<font color=fuchsia>**一个 Dockerfile 可以包含多个 `RUN` 命令，但是只能有一个 `CMD` 命令**</font>。

注意：<font color=fuchsia>指定了 `CMD` 命令以后，`docker container run` 命令就不能附加命令了</font>（<font color=red>比如前面的 `/bin/bash`</font> ），<font color=fuchsia>否则它会覆盖 `CMD` 命令</font>。现在，启动容器可以使用下面的命令：

```sh
$ docker container run --rm -p 8000:3000 -it koa-demo:0.0.1
```

###### 发布 image 文件

容器运行成功后，就确认了 image 文件的有效性。这时，我们就可以考虑把 image 文件分享到网上，让其他人使用。

首先，去 [hub.docker.com](https://hub.docker.com/) 或 [cloud.docker.com](https://cloud.docker.com/) 注册一个账户。然后，用下面的命令登录。

```sh
$ docker login
```

接着，为本地的 image 标注用户名和版本：

```sh
# 语法
$ docker image tag [imageName] [username]/[repository]:[tag]
# 示例
$ docker image tag koa-demos:0.0.1 ruanyf/koa-demos:0.0.1 # 👀 这里的用户名是 ruanyf
```

也可以不标注用户名，重新构建一下 image 文件：

```sh
$ docker image build -t [username]/[repository]:[tag] .
```

最后，发布 image 文件：

```sh
$ docker image push [username]/[repository]:[tag]
```

发布成功以后，登录 hub.docker.com，就可以看到已经发布的 image 文件。

##### 其他有用的命令

###### docker container start

> 👀 注：根据 [stack overflow - Is there any difference between docker start and docker container start?](https://stackoverflow.com/questions/51023407/is-there-any-difference-between-docker-start-and-docker-container-start) 的说法：`docker start` 和 `docker container start` 没有区别。也是更常见的写法。

前面的 `docker container run` 命令是新建容器，<font color=LightSeaGreen>每运行一次，就会新建一个容器</font>。同样的命令<font color=LightSeaGreen>运行两次，就会生成两个一模一样的容器文件</font>。<font color=red>如果希望重复使用容器，就要使用 `docker container start` 命令，它用来启动已经生成、已经停止运行的容器文件</font>。

```sh
$ docker container start [containerID]
$ docker start [containerID]
```

###### docker container stop

前面的 <font color=red>`docker container kill` 命令终止容器运行，相当于 向容器里面的主进程发出 `SIGKILL` 信号</font>。而 <font color=fuchsia>`docker container stop` 命令也是用来终止容器运行，相当于向容器里面的主进程发出 `SIGTERM` 信号，然后 **过一段时间再发出 `SIGKILL` 信号**</font>。

```bash
$ docker container stop [containerID]
$ docker stop [containerID]
```

<font color=dodgerBlue>这两个信号的差别是</font>：<font color=fuchsia>应用程序收到 `SIGTERM` 信号以后，**可以自行进行收尾清理工作**，但也可以不理会这个信号</font>。如果<font color=fuchsia>收到 `SIGKILL` 信号，就**会强行立即终止**，那些**正在进行中的操作会全部丢失**</font>。

###### docker container logs

`docker container logs` 命令用来查看 docker 容器的输出，即容器里面 Shell 的标准输出。<font color=red>如果 `docker run` 命令运行容器的时候，**没有使用 `-it` 参数，就要用这个命令查看输出**</font>

```sh
$ docker container logs [containerID]
$ docker logs [containerID]
```

###### docker container exec

`docker container exec` 命令用于进入一个正在运行的 docker 容器。<font color=dodgerBlue>如果 `docker run` 命令运行容器</font>的时候，<font color=red>没有使用 `-it` 参数</font>，就要用这个命令进入容器。<font color=fuchsia>一旦进入了容器，就可以在容器的 Shell 执行命令了</font>。

```sh
$ docker container exec -it [containerID] /bin/bash
```

###### docker container cp

`docker container cp` 命令<font color=red>用于从正在运行的 Docker 容器里面，将文件拷贝到本机</font>。下面是拷贝到当前目录的写法：

```sh
$ docker container cp [containID]:[/path/to/file] .
```

摘自：[阮一峰 - Docker 入门教程](https://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)



#### Dockerfile

Dockerfile 指令说明 **简洁版**：

- FROM ：构建镜像基于哪个镜像

- MAINTAINER ：镜像维护者姓名或邮箱地址

- RUN ：构建镜像时运行的指令

- CMD ：运行容器时执行的shell环境

- VOLUME ：指定容器挂载点到宿主机自动生成的目录或其他容器

- USER ：为 RUN、CMD、和 ENTRYPOINT 执行命令指定运行用户

- WORKDIR ：为 RUN、CMD、ENTRYPOINT、COPY 和 ADD 设置工作目录，就是切换目录

- HEALTHCHECH ：健康检查

- ARG ：构建时指定的一些参数

- EXPOSE ：声明容器的服务端口（仅仅是声明）

- ENV ：设置容器环境变量

- ADD ：拷贝文件或目录到容器中，如果是URL或压缩包便会自动下载或自动解压

- COPY ：拷贝文件或目录到容器中，跟ADD类似，但不具备自动下载或解压的功能

- ENTRYPOINT ：运行容器时执行的shell命令

摘自：[runoob - Docker Dockerfile](https://www.runoob.com/docker/docker-dockerfile.html)



### docker 命令

> 👀 注：具体参考 [Docker Doc - Docker CLI](https://docs.docker.com/engine/reference/commandline/docker/) ，不过并没有深度使用 docker，且官方文档不够简洁，所以选择摘抄 [runoob](https://www.runoob.com/) 中的内容



#### docker run

`docker run` ：创建一个新的容器并运行一个命令

##### 语法

```sh
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

###### OPTIONS 说明

> 👀 注：更多 选项参见：[Docker Doc - docker run # options](https://docs.docker.com/engine/reference/commandline/run/#options)

- **`-a stdin`** ：指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；

- **`-d`** ：后台运行容器，并返回容器ID；

- **`-i`** ：以交互模式运行容器，通常与 `-t` 同时使用；

- **`-P`** ：随机端口映射，容器内部端口**随机**映射到主机的端口

- **`-p`** ：指定端口映射，格式为：**`主机(宿主)端口:容器端口`**

- **`-t`** ：为容器重新分配一个伪输入终端，通常与`-i` 同时使用；

- **`--name="nginx-lb"`** ：为容器指定一个名称

  > 👀 补充：除了上面的 `--name="targetName"` ，也可以使用 `--name targetName` ；这两个都可以在 [Docker Doc - Docker run reference](https://docs.docker.com/engine/reference/run/#docker-run-reference) 中找到的

- **`--dns 8.8.8.8`** ：指定容器使用的DNS服务器，默认和宿主一致；

- **`--dns-search example.com`** ：指定容器DNS搜索域名，默认和宿主一致；

- **`-h "mars"`** ：指定容器的 hostname

- **`[--env | -e] username="ritchie"`** ：设置环境变量

  > 👀 补充：
  >
  > `-e` / `--env` 使用示例：`--env MYSQL_ROOT_PASSWORD=123456`：向容器进程传入一个环境变量 `MYSQL_ROOT_PASSWORD`，该变量会被用作 MySQL 的根密码。
  >
  > 在同一条命令中，`-e` 可以设置多个。
  >
  > 摘自：[阮一峰 - Docker 微服务教程](https://www.ruanyifeng.com/blog/2018/02/docker-wordpress-tutorial.html)

- **`--env-file=[]`** ：从指定文件读入环境变量

- **`--cpuset="0-2"` / `--cpuset="0,1,2"`** ：绑定容器到指定CPU运行；

- **`-m`** ：设置容器使用内存最大值；

- **`--net="bridge"`** ：指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型

- **`--link=[]`** ：添加链接到另一个容器

  > 👀 补充：
  >
  > `--link` 使用示例：`--link wordpressdb:mysql` 表示 WordPress 容器（ `docker run` 的最后一个参数）要连到 `wordpressdb` 容器，冒号表示该容器的别名是`mysql` 。

- **`--expose=[]`** ：开放一个端口或一组端口；

- **`--volume` /  `-v`** ：绑定一个卷

  > 👀 补充：
  >
  > `-v` 使用示例：`--volume "$PWD/":/var/www/html` ，意思是：将当前目录 `$PWD` <font color=fuchsia>**映射到**</font> 容器的 `/var/www/html`（ Apache 对外访问的默认目录）。因此，<font color=fuchsia>**当前目录的任何修改，都会反映到容器里面，进而被外部访问到**</font>。
  >
  > 摘自：[阮一峰 - Docker 微服务教程](https://www.ruanyifeng.com/blog/2018/02/docker-wordpress-tutorial.html)
  >
  > 👀 其他补充：
  >
  > `-v` 选项，可以将 本地代码和 docker 容器绑定起来，在本地编写项目，修改同样会反映在 docker 容器中；相当方便 ⭐️
  >
  > 一般 前端在 打包后、上传打包 之前 会使用 [http-server](https://github.com/http-party/http-server) 或 [serve](https://github.com/vercel/serve) 运行打包结果，以检查“打包结果” 和 开发环境 是否有区别。而在使用 docker 后，提供了另一种方案：
  >
  > 在 `dist` 文件夹下，运行如下命令：
  >
  > ```sh
  > $ docker run --name web-server -d -p 8000:80 -v $(pwd):/usr/share/nginx/html nginx
  > #            命名container  独立模式 映射端口   映射本地文件夹                    运行nginx image
  > ```
  >
  > 这时，可以访问 `localhost:8000`  ，查看 dist 中的打包效果。
  >
  > 学习自：[codingstartup - 初探 Docker](https://www.bilibili.com/video/BV1vD4y1X7ce)

摘自：[runoob - Docker run 命令](https://www.runoob.com/docker/docker-run-command.html)



#### docker rmi

Remove one or more images

##### 用法

```sh
$ docker rmi [OPTIONS] IMAGE [IMAGE...]
```

##### 选项

- **`-force` , `-f`** ：强制删除
- **`--no-prune`** ：不移除该镜像的过程镜像，默认移除

摘自：[runoob - Docker rmi 命令](https://www.runoob.com/docker/docker-rmi-command.html)



#### docker commit 

`docker commit` ：从容器（ 👀 为基础）创建一个新的镜像。

##### 语法

```sh
$ docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```

##### 选项

- **`-a`** ：提交的镜像作者
- **`-c`** ：使用 Dockerfile 指令来创建镜像
- **`-m`** ：提交时的说明文字；
- **`-p`** ：在 commit 时，将容器暂停。

摘自：[runoob - Docker commit 命令](https://www.runoob.com/docker/docker-commit-command.html)



#### docker diff

`docker diff` : 检查<font color=red>容器里文件结构</font>的更改。

##### 语法

```sh
docker diff [OPTIONS] CONTAINER
```

摘自：[runoob - Docker diff 命令](https://www.runoob.com/docker/docker-diff-command.html)



#### docker attach

`docker attach` ：<font color=fuchsia>连接到正在运行中的容器</font>

##### 语法

```
docker attach [OPTIONS] CONTAINER
```

要 attach 上去的容器必须正在运行，可以同时连接上同一个container来共享屏幕（与 screen 命令的attach类似）。

官方文档中说 attach 后可以通过 CTRL-C 来 detach，但实际上经过我的测试，如果 container 当前在运行 bash，CTRL-C 自然是当前行的输入，没有退出；如果 container 当前正在前台运行进程，如输出 nginx 的 access.log 日志，CTRL-C 不仅会导致退出容器，而且还stop 了。这不是我们想要的，detach 的意思按理应该是脱离容器终端，但容器依然运行。好在 attach 是可以带上`--sig-proxy=false` 来确保 CTRL-D 或 CTRL-C 不会关闭容器。

摘自：[runoob - Docker attach 命令](https://www.runoob.com/docker/docker-attach-command.html)



### Docker compose

#### 《阮一峰 - Docker 微服务教程》笔记

[Compose](https://docs.docker.com/compose/) 是 Docker 公司推出的一个工具软件，可以管理多个 Docker 容器组成一个应用。你需要定义一个 [YAML](https://www.ruanyifeng.com/blog/2016/07/yaml.html) 格式的配置文件`docker-compose.yml`，写好多个容器之间的调用关系。然后，只要一个命令，就能同时启动/关闭这些容器。

```sh
# 启动所有服务
$ docker-compose up
# 关闭所有服务
$ docker-compose stop
```

Mac 和 Windows 在安装 docker 的时候，会一起安装 docker compose。Linux 系统下的安装参考[官方文档](https://docs.docker.com/compose/install/#install-compose)。

安装完成后，运行下面的命令：

```sh
$ docker-compose --version
```

##### 示例

```yaml
# docker-compose.yml
mysql:
    image: mysql:5.7
    environment:
     - MYSQL_ROOT_PASSWORD=123456
     - MYSQL_DATABASE=wordpress
web:
    image: wordpress
    links:
     - mysql
    environment:
     - WORDPRESS_DB_PASSWORD=123456
    ports:
     - "127.0.0.3:8080:80"
    working_dir: /var/www/html
    volumes:
     - wordpress:/var/www/html
```

上面代码中，两个顶层标签表示有两个容器 `mysql `和  `web` 。每个容器的具体设置，前面都已经讲解过了，还是挺容易理解的。

> 👀 注：这里的 `docker-compose.yml` 是老版本的，在 当前 `Docker Compose version v2.10.2` 环境下无法运行，尝试在顶层加上 `services:` 也还是有问题，暂时没时间仔细研究，这里略。

启动两个容器：

```sh
$ docker-compose up
```

浏览器访问 http://127.0.0.3:8080，应该就能看到 WordPress 的安装界面。

现在关闭两个容器：

```sh
$ docker-compose stop
```

<font color=red>关闭以后，这两个容器文件还是存在的，写在里面的数据不会丢失；下次启动的时候，还可以复用</font>。下面的命令可以把这两个容器文件删除（容器必须已经停止运行）。

```sh
$ docker-compose rm
```

摘自：[阮一峰 - Docker 微服务教程](https://www.ruanyifeng.com/blog/2018/02/docker-wordpress-tutorial.html)