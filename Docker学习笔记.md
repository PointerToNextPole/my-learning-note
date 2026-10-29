

# Docker 学习笔记

> 2026-10-29 补充：根据 [学习 Docker 全套实践](codex://threads/019f8f6a-a20e-7c01-b885-bf7a8268f7dc) 整理的系统笔记见 [[#实践复盘：Docker、Dockerfile 与 Compose]]；优先复习其中的 [[#反复出错的概念与命令]]。



#### Docker 特性

##### 核心概念

1. `Build, Ship and Run`（搭建、运输、运行）

2. `Build once, Run anywhere`（一次搭建，处处运行）

   > 👀 类似于 Java 的 JVM

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

<font color=dodgerBlue>Docker 的主要用途，目前有三大类：</font>

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

> 👀 对于 必须要使用 sudo 权限才能使用的 工具，似乎也可以使用该方法？

##### image 文件

<font color=fuchsia>**Docker 把应用程序及其依赖，打包在 image 文件里面**</font>。只有通过这个文件，才能生成 Docker 容器。image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。<font color=fuchsia>同一个 image 文件，可以生成多个同时运行的容器实例</font>。

image 是二进制文件。<font color=red>实际开发中，一个 image 文件往往通过继承另一个 image 文件，加上一些个性化设置而生成</font>。举例来说，<font color=LightSeaGreen>你可以在 Ubuntu 的 image 基础上，往里面加入 Apache 服务器，形成你的 image</font>。

```sh
# 列出本机的所有 image 文件。
$ docker image ls
$ docker images # 👀 可以起到同样的作用，而且更加简洁

# 删除 image 文件
$ docker image rm [imageName]
$ docker rmi [imageName] # 简写；docker rm 删除的是容器，不是镜像
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

> [!NOTE]
> 这里的 container 可以省略，即：`docker run hello-world` 。类似的也有：`docker kill` 、`docker stop` 、`docker start` 、`docker exec` 、`docker logs` 、`docker cp` 、`docker rm` 等，这种写法也更常见的。

`docker container run` 命令会从 image 文件，生成一个正在运行的容器实例。

> [!NOTE]
> 
> `docker run` 如果没有制定名称，则 docker 会给他一个名字；另外，可以通过 `--name` 选项给 当前 container 自定义一个名字：`--name targetName`

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

> [!NOTE]
> 
> **`docker ps` 和 `docker container ls`  功能相同**，但是语义更明确，简化了用法，更推荐使用 `docekr ps`

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

> 👀 下面的内容有部分省略，有不理解的地方见原文

以阮一峰写的 [koa-demos](https://www.ruanyifeng.com/blog/2017/08/koa.html) 为例。

###### 编写 `.dockerignore` 文件

<font color=dodgerBlue>在项目的根目录下，新建文本文件 `.dockerignore`</font> （和 `.gitignore` 类似），标识 文件/路径 “不要打包进入 image 文件”：

```
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
- `EXPOSE 3000` ：声明容器预期使用的服务端口；不会启动监听，也不会自动向宿主机发布端口。宿主机访问通常还需要 `docker run -p 主机端口:容器端口`。

###### 创建 image 文件

<font color=red>有了 Dockerfile 文件以后，就可以使用 `docker image build` 命令创建 image 文件了</font>。

```sh
$ docker image build -t koa-demo .
# 或者
$ docker image build -t koa-demo:0.0.1 .
```

上面代码中，<font color=red>**`-t` 参数用来指定 image 文件的名字**</font>，<font color=LightSeaGreen>后面还可以用冒号指定标签</font>。**如果不指定，默认的标签就是`latest` **。<font color=fuchsia>最后的 `.` 表示 Dockerfile 文件所在的路径</font>（ 👀 路径是必填的（没有默认值），注意下，不要漏掉），上例是当前路径，所以是一个点。

> [!NOTE]
> 虽然上面说 `docker build` 中 `-t` 用来指定 image 的名字，但是 `-t` 的全称是 `--tag`

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

  > 👀 <font color=LightSeaGreen>因为本机是无法访问 docker 的端口的，必须要做映射</font>

- `-it` 参数 ：容器的 Shell 映射到当前的 Shell，然后你在本机窗口输入的命令，就会传入容器

  > 👀 `-i` 意为 interactive 交互，表示：以 “交互模式” 运行容器；`-t` 意为 terminal 命令行，表示：为容器重新分配一个伪输入终端。
  >
  > 💡 除了上面所说的 “交互模式”，还有 “detached mode”，即 “分离模式”，用 `-d` 来启用。作用是：后台运行容器，并返回 “容器ID”；类似于 `docker start [containerID]` 的效果。
  >
  > > 容器将在后台运行，而不会占用当前终端。
  > >
  > > 摘自：[前端开发必会的Nginx知识及结合Docker的项目部署实战](https://mp.weixin.qq.com/s/Xj_kqT4BFytZA7LvJQvAXQ)

- `koa-demo:0.0.1` ：image 文件的名字（如果有标签，还需要提供标签，默认是 latest 标签）

- `/bin/bash` ：<font color=fuchsia>**容器启动以后，内部第一个执行的命令**</font>。这里是启动 Bash，保证用户可以使用 Shell

  > 👀 之所以 启用 bash、使用 shell， 是为了后面使用 node 等命令行工具。

如果一切正常，运行上面的命令以后，就会返回一个命令行提示符：

```sh
root@66d80f4aaf1e:/app#
```

这表示你已经在容器里面了，返回的提示符就是容器内部的 Shell 提示符。执行下面的命令：

```sh
root@66d80f4aaf1e:/app# node demos/01.js
```

这时，（ 👀 image 内置的）Koa 框架已经运行起来了。打开本机的浏览器，访问 http://127.0.0.1:8000 ，网页显示"Not Found"，这是因为这个 [demo](https://github.com/ruanyf/koa-demos/blob/master/demos/01.js) 没有写路由。

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

> [!NOTE]
> 
> 可以使用 `Ctrl + P` + `Ctrl + Q` 返回本机命令行，而不退出当前 container 的运行。
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

你可能会问，<font color=dodgerBlue>**`RUN` 命令与 `CMD` 命令的区别在哪里？**</font>简单说，<font color=fuchsia>`RUN` 命令 <font size=4>**在 image 文件的构建阶段执行，执行结果都会打包进入 image 文件**</font></font>；<font color=fuchsia>`CMD` 命令则<font size=4>**是在容器启动后执行**</font></font>。一个 Dockerfile 可以有多个 `RUN`；也可以出现多个 `CMD`，但只有最后一条 `CMD` 生效，因此通常只写一条。

注意：指定 `CMD` 以后，`docker container run IMAGE COMMAND` 仍可附加命令；这会覆盖镜像中的默认 `CMD`，是有意调试时的正常用法。若还设置了 `ENTRYPOINT`，则要结合两者的组合规则理解。现在，按默认命令启动容器可以使用：

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

> 👀 根据 [stack overflow - Is there any difference between docker start and docker container start?](https://stackoverflow.com/questions/51023407/is-there-any-difference-between-docker-start-and-docker-container-start) 的说法：`docker start` 和 `docker container start` 没有区别。也是更常见的写法。

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



### Dockerfile

##### 指令说明

Dockerfile 指令说明 **简洁版**：

- FROM ：构建镜像基于哪个镜像

- MAINTAINER ：镜像维护者姓名或邮箱地址

- RUN ：构建镜像时运行的指令

- CMD ：容器的默认启动命令或默认参数；不等于必须启动 shell

- VOLUME ：指定容器挂载点到宿主机自动生成的目录或其他容器

- USER ：为 RUN、CMD、和 ENTRYPOINT 执行命令指定运行用户

- WORKDIR ：为 RUN、CMD、ENTRYPOINT、COPY 和 ADD 设置工作目录，就是切换目录

- HEALTHCHECK ：声明容器运行期间的健康检查

- ARG ：构建时指定的一些参数

- EXPOSE ：声明容器的服务端口（仅仅是声明）

- ENV ：设置容器环境变量

- ADD ：拷贝文件或目录到容器中，如果是 URL 或压缩包便会自动下载或自动解压

- COPY ：拷贝文件或目录到容器中，跟 ADD 类似，但不具备自动下载或解压的功能

- ENTRYPOINT ：容器启动入口，支持 exec form 与 shell form

摘自：[runoob - Docker Dockerfile](https://www.runoob.com/docker/docker-dockerfile.html)



#### Dockerfile 优化方法

##### 使用 alpine 镜像，而不是默认的 linux 镜像

docker 容器内跑的是 linux 系统，各种镜像的 dockerfile 都会继承 linux 镜像作为基础镜像，该基础镜像实际上是继承 debian 的 Linux 镜像。其实可以换成更小的版本，也就是 alpine ：它裁剪了很多不必要的 linux 功能，使得镜像体积大幅减小了。经过实验可以发现：相较 debian，使用 alpine build 生成的镜像大小可以缩小 900M。

##### 使用多阶段构建

如下 dockerfile ：

```dockerfile
FROM node:18-alpine3.14

WORKDIR /app

COPY package.json .

RUN npm config set registry https://registry.npmmirror.com/
RUN npm install

COPY . .

RUN npm run build

EXPOSE 3000

CMD [ "node", "./dist/main.js" ]
```

<font color=dodgerBlue>可能会存在如下疑问</font>：<font color=lightSeaGreen>为什么先复制 `package.json` 进去，安装依赖之后再复制其他文件；**直接全部复制进去不就行了**？</font><font color=red>**不行，这两种写法的效果不同**</font>。

<font color=fuchsia>docker 是分层存储的，**dockerfile 里的每一行指令是一层，会做缓存**</font>。<font color=red>每次 `docker build` 的时候，只会从变化的层开始重新构建，没变的层会直接复用</font>。也就说：现在这种写法，如果 `package.json` 没变，那么就不会执行 `npm install` ，直接复用之前的。

如果一开始就把所有文件复制进去呢？那不管 `package.json` 变没变，任何一个文件变了，都会重新 `npm install` ，这样没法充分利用缓存，性能不好。

> 👀 关于这里的内容，想了下才搞懂这里在说什么：`npm install` 的执行，应该只和 `package.json` 中内容的变化有关，不应该受到其他文件（比如 `/src/` 中的文件）变化的影响。所以，先复制 `package.json` ，并运行 `npm install` 将变化范围缩小到只有 `package.json` ，有变化，则运行；无变化，则不运行。等 install 完成后，再复制其他文件

> 💡另外，看上面存在执行步骤的问题，便想到一个问题 “在 dockerfile 中，每一行指令的执行是否是同步的？”，问了 gpt，得到如下回复：
>
> <img src="https://s2.loli.net/2024/07/13/YOmi7TNZ8ntFyP3.png" alt="image-20240713164408216" style="zoom:50%;" />

<font color=dodgerBlue>另一个问题是</font>：源码和很多构建的依赖是不需要的，但是现在都保存在了镜像里。实际上只需要构建出来的 `./dist` 目录下的文件还有运行时的依赖。

这时可以用多阶段构建：

```dockerfile
FROM node:18-alpine3.14 as build-stage

WORKDIR /app

COPY package.json .

RUN npm config set registry https://registry.npmmirror.com/

RUN npm install

COPY . .

RUN npm run build

# production stage
FROM node:18-alpine3.14 as production-stage

COPY --from=build-stage /app/dist /app
COPY --from=build-stage /app/package.json /app/package.json

WORKDIR /app

RUN npm config set registry https://registry.npmmirror.com/

RUN npm install --production

EXPOSE 3000

CMD ["node", "/app/main.js"]
```

> 💡 这里指令与指令之间 “空一行” 和 “没空行” 引起了我的好奇：
>
> <img src="https://s2.loli.net/2025/01/23/Pjatz5xc9AlYrQu.png" alt="image-20240713164929309" style="zoom:50%;" />

`FROM` 后面添加一个 `as` 来指定当前构建阶段的名字。

通过 `COPY --from=xxx` 可以从上个阶段复制文件过来。

`npm install` 的时候添加 `--production` ，这样只会安装 dependencies 的依赖。

默认构建目标是最后一个阶段，也可通过 `--target` 指定其他阶段；构建器仍可能保留中间阶段的缓存。最终镜像包含什么，取决于目标阶段的基础镜像与该阶段的指令，不能一概说“只有 dist、一定没有源码”。例如，以 `nginx:alpine` 为基础且只复制 `dist` 的最终阶段，会包含 Nginx 及基础文件系统、构建产物和配置，不会因此继承构建阶段的 Node、源码和 `node_modules`。



### docker 命令

> 👀 具体参考 [Docker Doc - Docker CLI](https://docs.docker.com/engine/reference/commandline/docker/) ，不过并没有深度使用 docker，且官方文档不够简洁，所以选择摘抄 [runoob](https://www.runoob.com/) 中的内容



#### docker run

`docker run` ：创建一个新的容器并运行一个命令

##### 语法

```sh
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

###### OPTIONS 说明

> 👀 更多 选项参见：[Docker Doc - docker run # options](https://docs.docker.com/engine/reference/commandline/run/#options)

- **`-a stdin`** ：指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；

- **`-d`** ：后台运行容器，并返回容器ID；

- **`-i`** ：以交互模式运行容器，通常与 `-t` 同时使用；

- **`-P`** ：随机端口映射，容器内部端口**随机**映射到主机的端口

- **`-p`** ：指定端口映射，格式为：**`主机(宿主)端口:容器端口`**

- **`-t`** ：为容器重新分配一个伪输入终端，通常与 `-i` 同时使用

- **`--name="nginx-lb"`** ：为容器指定一个名称

  > 💡 补充
  >
  > 除了上面的 `--name="targetName"` ，也可以使用 `--name targetName` ；这两个都可以在 [Docker Doc - Docker run reference](https://docs.docker.com/engine/reference/run/#docker-run-reference) 中找到的

- **`--dns 8.8.8.8`** ：指定容器使用的 DNS 服务器，默认和宿主一致；

- **`--dns-search example.com`** ：指定容器DNS搜索域名，默认和宿主一致；

- **`-h "mars"`** ：指定容器的 hostname

- **`[--env | -e] username="ritchie"`** ：设置环境变量

  > 💡补充
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

  > [!TIP]
  > `--link` 使用示例：`--link wordpressdb:mysql` 表示 WordPress 容器（ `docker run` 的最后一个参数）要连到 `wordpressdb` 容器，冒号表示该容器的别名是`mysql` 。

- **`--expose=[]`** ：开放一个端口或一组端口；

- **`--volume` /  `-v`** ：绑定一个卷

  > [!TIP]
  >
  > `-v` 使用示例：`--volume "$PWD/":/var/www/html` ，意思是：将当前目录 `$PWD` <font color=fuchsia>**映射到**</font> 容器的 `/var/www/html`（ Apache 对外访问的默认目录）。因此，<font color=fuchsia>**当前目录的任何修改，都会反映到容器里面，进而被外部访问到**</font>。
  >
  > 摘自：[阮一峰 - Docker 微服务教程](https://www.ruanyifeng.com/blog/2018/02/docker-wordpress-tutorial.html)
  >
  > 其他补充：
  >
  > `-v` 选项，可以将 本地代码和 docker 容器绑定起来，在本地编写项目，修改同样会反映在 docker 容器中；相当方便 ⭐️
  >
  > 一般 前端在 打包后、上传打包 之前 会使用 [http-server](https://github.com/http-party/http-server) 或 [serve](https://github.com/vercel/serve) 运行打包结果，以检查“打包结果” 和 开发环境 是否有区别。而在使用 docker 后，提供了另一种方案：
  >
  > 在 `dist` 文件夹下，运行如下命令：
  >
  > ```sh
  > $ docker run --name web-server -d -p 8000:80 -v "$(pwd):/usr/share/nginx/html" nginx
  > #            命名container  独立模式 映射端口   映射本地文件夹                    运行nginx image
  > ```
  >
  > > 👀 `$(pwd)` 是执行 `pwd` 后取输出，`$PWD` 是读取 shell 变量，两者都可用于这里；写成 `"$PWD:/usr/share/nginx/html"` 也可以。变量名区分大小写，不能把 `$pwd` 当成 `$PWD`；也不能据此推导命令名称不区分大小写。路径应加引号，以支持空格等字符。
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

要 attach 上去的容器必须正在运行，可以同时连接上同一个container来共享屏幕（与 screen 命令的 `attach` 类似）。

官方文档中说 attach 后可以通过 CTRL-C 来 detach，但实际上经过我的测试，如果 container 当前在运行 bash，CTRL-C 自然是当前行的输入，没有退出；如果 container 当前正在前台运行进程，如输出 nginx 的 access.log 日志，CTRL-C 不仅会导致退出容器，而且还stop 了。这不是我们想要的，detach 的意思按理应该是脱离容器终端，但容器依然运行。好在 attach 是可以带上`--sig-proxy=false` 来确保 CTRL-D 或 CTRL-C 不会关闭容器。

摘自：[runoob - Docker attach 命令](https://www.runoob.com/docker/docker-attach-command.html)



### Docker compose

#### 《阮一峰 - Docker 微服务教程》笔记

[Compose](https://docs.docker.com/compose/) 是 Docker 公司推出的一个工具软件，可以管理多个 Docker 容器组成一个应用。你需要定义一个 [YAML](https://www.ruanyifeng.com/blog/2016/07/yaml.html) 格式的配置文件`docker-compose.yml`，写好多个容器之间的调用关系。然后，只要一个命令，就能同时启动/关闭这些容器。

```sh
# 启动所有服务
$ docker compose up
# 关闭所有服务
$ docker compose stop
```

Mac 和 Windows 在安装 docker 的时候，会一起安装 docker compose。Linux 系统下的安装参考[官方文档](https://docs.docker.com/compose/install/#install-compose)。

安装完成后，运行下面的命令：

```sh
$ docker compose --version
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

> 👀 历史笔记：这个旧版示例当时在 `Docker Compose v2.10.2` 环境下未跑通。下面保留原记录；本轮学习已补充可对照的现代完整配置，见 [[#完整配置总参考]]。

启动两个容器：

```sh
$ docker compose up
```

浏览器访问 http://127.0.0.3:8080 ，应该就能看到 WordPress 的安装界面。

现在关闭两个容器：

```sh
$ docker compose stop
```

<font color=red>关闭以后，这两个容器文件还是存在的，写在里面的数据不会丢失；下次启动的时候，还可以复用</font>。下面的命令可以把这两个容器文件删除（容器必须已经停止运行）。

```sh
$ docker compose rm
```

摘自：[阮一峰 - Docker 微服务教程](https://www.ruanyifeng.com/blog/2018/02/docker-wordpress-tutorial.html)



### OrbStack 使用

##### docker context

- `docker context use orbstack`
- `docker context show`


## 实践复盘：Docker、Dockerfile 与 Compose

来源：[学习 Docker 全套实践](codex://threads/019f8f6a-a20e-7c01-b885-bf7a8268f7dc)。对话中的学习记录覆盖 2026-07-23 至 2026-08-04，本节整理于 2026-10-29。

### 学习范围与复习顺序

学习目标是：以前端开发者的身份，独立构建 Vue 镜像，用 Nginx 提供页面与反向代理，用 Compose 串起 Spring Boot 和 MySQL，并能解释常见故障。

原对话最后的进度：前 8 项已通过概念检查和独立练习；第 9 项“镜像体积、安全与健康检查”完成概念检查，停在完整 API Dockerfile 的独立改造练习；第 10 项“简历表达与综合排障”尚未开始。不能把示例由老师运行成功，直接当成自己已经独立掌握。

复习入口：

- [[#反复出错的概念与命令]]：先看带“反复出现”的问题，再用反例判断自己是否真的理解。
- [[#完整配置总参考]]：把 Dockerfile、Nginx、Compose 放在一起看，避免只记孤立指令。
- [[#镜像安全、非 root 与健康检查]]：第 9 项内容及尚未完成的练习。
- [[#本次实践中的真实故障]]：Mac、OrbStack、代理、挂载权限和端口冲突。
- [[#复习自测与答案]]：先独立回答，再展开答案。

> [!important] 最容易反复混淆的是“对象、执行位置和生命周期”
> 每次先问：这里操作的是 image、container、volume，还是 Compose service？命令由 Mac 的 shell 解释，还是在 container 内执行？是在 build 时、创建 container 时，还是现有 container 运行中发生？

### 基础对象与命令的对应关系

| 对象 | 作用 | 创建或使用方式 | 删除方式 |
| --- | --- | --- | --- |
| image | 只读模板，包含基础文件系统、应用文件和默认配置 | `docker build -t demo .` / `docker pull alpine` | `docker image rm demo` 或 `docker rmi demo` |
| container | 根据 image 创建的运行实例，有自己的可写层和运行配置 | `docker run --name job alpine ...` | `docker rm job`，通常先停止 |
| named volume | Docker 管理的独立数据对象 | `docker volume create notes`；通过挂载供 container 访问 | `docker volume rm notes`，需先解除使用 |
| Compose project | 一组服务、网络和卷的管理边界 | Compose 顶层 `name:` 或命令行 `-p` 等指定 | `docker compose down` 移除项目容器和相应网络；卷另有规则 |
| Compose service | 对一种应用组件的声明，可管理一个或多个 container 实例 | `services:` 下的 `frontend:`、`api:` 等 | 由 Compose 生命周期命令管理 |

```text
Dockerfile + 构建上下文 ──docker build──> image
image + 运行配置 ──docker run──> container
独立的 volume ──mount──> container 内的访问路径
compose.yaml ──docker compose up──> 协调 services、containers、network、volumes
```

`docker build` 的交付结果是 image，不会因此新增一个可在 `docker ps -a` 中管理的业务 container。`docker start` 则启动已有 container，既不构建 image，也不创建新 container。

### 反复出错的概念与命令

#### 1. 反复出现：volume 身份由 source 决定，target 只是访问入口

**当时的错误：**先认为换成 `/backup` 后就读不到原数据，又把 source 和 target 的职责反过来；综合题中再次把右侧说成“镜像的路径”。后来 Compose 练习中，又在 `reader` 里使用了 `writer` 的挂载路径。

```text
-v notes:/data
   └─┬─┘ └─┬─┘
   source  target
   数据对象  当前 container 内的挂载位置
```

这里的 `notes` 是 named volume，不是 image 的存放位置。`/data` 是当前 container 内访问它的路径，不是某个跨 container 统一的地址。

| 挂载配置 | 数据对象 | 当前 container 中的入口 |
| --- | --- | --- |
| `app-data:/mnt` | `app-data` | `/mnt` |
| `app-data:/backup` | 同一个 `app-data` | `/backup` |
| `other-data:/mnt` | 另一个 `other-data` | `/mnt` |

**关键反例：**前两项入口不同，仍是同一份数据；第一项和第三项入口相同，仍可能是两份不同的数据。

完整小实验，使用一个专用的练习卷：

```sh
docker volume create frontend-cache

docker run --rm -v frontend-cache:/cache alpine \
  sh -c 'echo vite > /cache/tool'

docker run --rm -v frontend-cache:/restore alpine \
  cat /restore/tool
```

最后输出 `vite`。第一个 container 看到 `/cache/tool`，第二个看到 `/restore/tool`，实际上都在访问 `frontend-cache` 中的 `tool`。

> [!warning] `--rm` 删除什么，也曾连续答错
> `docker run [OPTIONS] IMAGE [COMMAND] [ARG...]` 中，`--rm` 是一个不需要值的开关；`alpine` 是 image 参数，不是 `--rm` 的值。`--rm` 表示 container 退出后自动删除该 container，不删除 `alpine` image，也不删除这里显式命名的 `frontend-cache` volume。匿名卷的自动清理规则不同，不要推广为“任何卷都保留”。

常见拼写与顺序错误也要单独记住：

```sh
# 正确结构：docker <对象组> <动作> <对象名>
docker volume create frontend-cache

# 镜像拼写是 alpine，不是 alphine
docker run --rm alpine echo hello
```

**Compose 的两层 `volumes`：**顶层声明可用的卷；service 内声明把哪个卷挂到哪里。完整参考见 [[#网络、共享卷与环境变量实验]]。顶层 `app-data:` 的空值表示采用默认卷配置，也可写成 `app-data: {}`。service 下的 `- app-data:/data` 则是一个列表字符串，使用 Compose 的 `source:target` 短语法。

**自检：**如果在 `reader` 执行 `cat`，先看 `reader.volumes` 的 target；不能因为文件由 `writer` 写入，就沿用 `writer` 的 target。

#### 2. 反复出现：logs 保存输出，inspect 查看退出状态

**错误演变：**起初认为日志会自动记录“手动退出”；纠正后，又过度推导成“container 退出后没有日志”。在后续复习中，即便口头记得 stdout/stderr，仍漏掉了 `echo start`，再次回答“无内容”。

**正确模型：**

```text
进程写到 stdout / stderr 的文字 → docker logs
Docker 保存的运行状态、退出码等 → docker inspect
container 已退出 ≠ 之前收集的输出被清空
```

对话示例使用普通日志配置、没有 `--rm`，也没有禁用日志；不能把示例结论误套到已经删除的 container 上。

| 主命令 | `docker logs` | `.State.ExitCode` |
| --- | --- | --- |
| `sh -c 'exit 1'` | 无输出 | `1` |
| `sh -c 'echo starting; exit 2'` | `starting` | `2` |
| `sh -c 'echo error >&2; exit 7'` | `error` | `7` |
| `sh -c 'echo ready; echo failed >&2; exit 9'` | 包含 `ready` 和 `failed` | `9` |

`>&2` 是 shell 重定向语法，不是输出正文；`exit 9` 设置退出状态，不会自动打印一行 `exit 9`。

完整复现实验：

```sh
docker run --name report alpine \
  sh -c 'echo ready; echo failed >&2; exit 9'

docker ps -a --filter name=report
docker logs report
docker inspect --format '{{.State.Status}} {{.State.ExitCode}}' report
```

应看到 `report` 已退出，日志包含两行文字，状态与退出码为 `exited 9`。`docker run` 本身返回非零是这个实验的预期现象。

> [!tip] 避免再犯的判断顺序
> 先逐句圈出 `echo`、程序日志和 stderr 输出，再单独判断 `exit`。不要先看到“已经退出”，就直接推导“日志为空”。

#### 3. 反复出现：PID 1、exec、start 和 -d 不是同一件事

**当时的错误：**把“归还终端”说成 container 状态；认为 `sleep 300` 结束后会静止等待指令；认为 `exec` 会自动 `start`；后来又把 PID 1 和其他命令理解成串行排队，并把 `exec` 成功归因于 detached。

| 概念或命令 | 实际职责 |
| --- | --- |
| PID 1 | container 内的主进程；它退出后，该 container 停止 |
| `docker run` | 根据 image 创建新 container，按最终选定的启动配置运行主进程 |
| `docker start` | 启动已有的停止 container，重新执行它保存的主命令 |
| `docker exec` | 在运行中的 container 内启动额外进程；不自动 start，也不重跑默认主命令 |
| `-d` | 让启动它的 Docker CLI 不持续占用当前终端，不决定 container 寿命 |
| `--rm` | container 退出后自动删除；与是否 detached 无关 |

```sh
docker run -d --name worker alpine sleep 60
docker exec worker echo hello
```

在 60 秒内及时执行第二条时：PID 1 仍是 `sleep 60`，`echo hello` 是额外启动的进程。`echo` 退出后，`sleep` 仍在运行；60 秒后，`sleep` 结束，container 进入 `exited`。它不是永久等待任务的工作队列。

**反例：不用 `-d` 也能 `exec`。**在终端 A 执行：

```sh
docker run --name foreground-worker alpine sleep 60
```

趁它仍在运行，在终端 B 执行：

```sh
docker exec foreground-worker echo hello
```

仍可成功。限制是 container 正在运行，而不是创建时有没有 `-d`。

再看“启动命令覆盖”，假设使用下面的完整最小 Dockerfile：

```dockerfile
FROM alpine
CMD ["sleep", "60"]
```

```sh
docker build -t lifecycle-demo .
docker run lifecycle-demo
docker run lifecycle-demo echo hi
```

第一种主命令是默认的 `sleep 60`；第二种用 `echo hi` 覆盖 `CMD`，不会先 sleep 再 echo。这个例子没有显式 `ENTRYPOINT`，有 `ENTRYPOINT` 时需结合两者的组合规则。

**另一个陷阱：**对 `sh -c 'echo starting; exit 2'` 创建的 container 执行 `docker start`，只会再次输出、再次退出。即使刚刚 start，也不代表它能持续存活到下一条 `exec`。

#### 4. 多次操作错误：shell 解析边界、重定向、管道与文件读取

错误命令：

```sh
docker run --name debug-box alpine echo booting; sleep 120
```

Mac 的 zsh 会先把分号解析为两个命令：container 只运行 `echo booting`，随后退出；`sleep 120` 在 Mac 上执行。要把整串命令交给 container 内的 shell：

```sh
docker run -d --name debug-box alpine \
  sh -c 'echo booting; sleep 120'
```

`sh -c '...'`：启动 shell，并让它解释后面的字符串。单引号还能避免 Mac 的 shell 提前展开字符串内的 `$变量` 等内容。

| 写法 | 含义 | 当时的误用 |
| --- | --- | --- |
| `cmd > file` | 把 stdout 写入文件，通常覆盖 | `> grep` 不会调用 grep，`> echo` 也不会打印响应 |
| `cmd >> file` | 把 stdout 追加到文件 | writer 每启动一次，会再次追加一行 |
| `cmd1 \| cmd2` | 把前一命令的 stdout 交给后一命令 | 筛选应使用管道，或直接使用 Docker 的 `--filter` |
| `echo /restore/note.txt` | 打印路径字符串 | 不会读取文件 |
| `cat /restore/note.txt` | 读取该路径的文件内容 | 要使用当前执行 container 的挂载路径 |
| `echo error >&2` | 把 error 写到 stderr | `>&2` 不会作为文字输出 |

Compose 中临时请求接口的正确形式：

```sh
docker compose exec client wget -qO- http://frontend:80/api/projects
```

- `client` 是执行命令的 service，不能遗漏。
- `-q`：不显示下载进度；`-O-`：把响应体输出到 stdout。
- 中间是大写字母 `O`，不是数字 `0`。
- `wget -qO-` 已经打印响应体，不需要接 `> echo`。
- 当前 Alpine 示例有 BusyBox `wget`；是否有 `curl` 要看 image，不能假设所有镜像都预装。

#### 5. 容器重启、容器重建与镜像重建必须区分

**当时的疑问和错误：**修改 `nginx.conf` 后不清楚如何更新；后来已经理解 `start` 不换 image，却又认为只改 Compose `ports` 也要重新 build image。

| 操作或变化 | 要重新构建 image 吗 | 要重新创建 container 吗 |
| --- | --- | --- |
| 对已有 container 执行 `stop/start` 或 `restart` | 否 | 否 |
| 修改被 Dockerfile `COPY` 进去的源码或 `nginx.conf` | 要让这些新内容进入 image，需要重新 build | 需要，才能用新 image |
| 只修改 Compose `ports` | 否 | 需要按新运行配置重新创建 |
| 只修改 Compose `environment` | 否 | 需要按新环境变量重新创建 |
| 只修改 bind mount 的宿主机文件内容 | 不靠 build 更新该挂载内容 | 文件可映射进去，但程序是否自动加载或需要 reload，要看程序 |

Dockerfile `COPY` 是构建时复制，不是文件同步。`resume-site` 是可移动的 image tag；重新 build 后，这个 tag 可以指向新 image，但已有 container 仍绑定创建时的 image。

单 container 更新的完整命令，假设 `frontend` 正在运行：

```sh
docker build --target production -t resume-site .
docker stop frontend
docker rm frontend
docker run -d --name frontend -p 8080:80 resume-site
```

两处实践遗漏：普通 `docker rm` 前要先停运行中的 container；重新 `run` 时还要带上原先需要的 `-p` 等运行参数，否则新 container 不会自动继承已删除 container 的配置。

Compose 的对应做法：

```sh
# 源码、Dockerfile 或复制进 image 的文件有变更
docker compose up --build -d

# 仅修改 ports / environment，让 Compose 按配置协调 container
docker compose up -d
```

普通 `up -d` 不能作为“保证按当前源码重新构建”的命令；显式要求构建时使用 `--build`。已有 container 只 `restart`，也不会应用新改的 Compose 配置。

#### 6. Compose 的 project、service、image、container 名称

以完整配置为例：

```yaml
name: resume-stack

services:
  frontend:
    build:
      context: .
      target: production
    image: resume-site
    ports:
      - "8080:80"
```

| 名称 | 对象 | 来自哪里 |
| --- | --- | --- |
| `resume-stack` | project | 顶层 `name:` |
| `frontend` | service | `services:` 下的键 |
| `production` | Dockerfile stage | `FROM ... AS production`，由 `build.target` 选择 |
| `resume-site` | image 引用／tag | `image:`，构建结果的名称 |
| `resume-stack-frontend-1` | 默认命名下的某个 container 实例 | Compose 创建，可受项目名和命名配置影响 |

**当时误把 `resume-site` 当 project，误把 `docker compose logs frontend` 的参数当 container 名。**Compose 命令这里引用的是 service；一个 service 可以有多个 container 实例，所以它不等于某个固定 container 名。

`docker compose up -d` 是协调期望配置：配置没变且已有实例正常时，不会每次都多创建一个；配置变了可能替换实例；显式 scale 才是增加实例数量的另一回事。

Compose 也不必须搭配 Dockerfile。只有 `image: nginx:alpine` 就能使用现成镜像；即便是自己公司的定制镜像，也可以由 CI 构建、推到 registry，部署机只拉取运行，无须带源码和 Dockerfile。

#### 7. 反复出现：发布给 Mac 的端口与 Compose 内部地址

**错误演变：**先认为删掉 `ports` 会导致 container 之间不能访问，或者 Docker 会自动给 Mac 分配端口；纠正后，完整项目中又把浏览器第一跳说成访问 `frontend:80`，还曾回答成“client 的 host 端口”。

先固定执行位置，再决定地址：

| 请求从哪里发出 | 访问哪个目标 | 当前完整项目中的地址 |
| --- | --- | --- |
| Mac 浏览器／Mac 的 curl | Nginx | `http://localhost:8088` |
| 同一 Compose 默认网络中的 client | Nginx | `http://frontend:80` |
| Nginx container | Spring Boot API | `http://api:8080` |
| API container | MySQL | `jdbc:mysql://db:3306/resume` |
| API container 内的健康检查 | API 自己 | `http://localhost:8080/api/projects` |

```text
Mac 浏览器发起请求：localhost:8088
            ↓ host 端口发布 8088:80
frontend container 内：Nginx listen 80
            ↓ Compose DNS 查询 api
api:8080
            ↓ Compose DNS 查询 db
db:3306
```

浏览器没有先解析 `frontend`。`frontend`、`api`、`db` 是当前 Compose 网络内的 service DNS 名称；普通 Mac 浏览器不使用这套内部 DNS。

`localhost` 始终先问“是谁的本机”：在 `client` 中是 client 自己；在 api 中是 api 自己；在 Mac 浏览器中是 Mac。container 之间没有共用同一个 localhost。

删除 Nginx 的 `ports` 后，在当前默认 bridge 网络示例中：

- `client → http://frontend:80` 仍然可以通，只要 Nginx 在监听、服务正常。
- Mac 不能再通过原来的 `localhost:8088` 发布入口访问它。
- 不写 `ports` 不会自动获得随机 host 端口；显式仅写 container port 的发布语法才是另一种情况。
- 内部连接不要求先写 `EXPOSE` 或 Compose `expose`。它们不负责启动应用的监听。

> [!important] 监听、声明、发布是三件事
> Nginx 的 `listen 80` 决定应用监听；Dockerfile 的 `EXPOSE 80` 声明预期端口；`-p 8088:80` / Compose `ports` 把它发布到 host。不是 Docker 或 Nginx“自动分配内部 80 端口”。

对于未显式配置其他网络的这些服务，Compose 自动创建 `<project>_default` 并把它们接入，不需要为了让 client 请求 frontend 而临时猜一段 `networks`。

#### 8. 反复出现：SPA fallback 是内部选文件，不是浏览器跳转

**当时先说：**刷新 `/users/42` 应该回退到 `/users/42`。纠正后，在后续综合题又说 Nginx 返回 `index.html` 会改变浏览器地址。

根因是把三种路径当成同一个对象：

| 层次 | 刷新订单页时的值 | 谁处理 |
| --- | --- | --- |
| 浏览器 URL | `/orders/9` | 浏览器／Vue Router |
| Nginx 选择的磁盘文件 | `/usr/share/nginx/html/index.html` | Nginx |
| 前端路由 | `/orders/9` | 加载后运行的 Vue Router |

完整 Nginx 配置见 [[#完整配置总参考]]，其中：

```nginx
location / {
    try_files $uri $uri/ /index.html;
}
```

先尝试请求对应的文件，再尝试目录，找不到则内部回退到 `/index.html`。不存在 `/orders/9` 这个磁盘文件时，再回退到它本身不能解决问题；需要返回能启动前端应用的 `index.html`。

| 配置 | 浏览器是否收到重定向响应 | 该导航是否因此发起第二次请求 | 地址栏 |
| --- | --- | --- | --- |
| `try_files ... /index.html` | 否 | 不会因这次 fallback 再请求一次页面 | 保持 `/orders/9` |
| `return 302 /index.html` | 是 | 是，随后请求 `/index.html` | 变为 `/index.html` |

这里说“没有第二次请求”指页面导航的重定向流程；HTML 加载 JS、CSS，以及 Vue 发 API 请求，当然仍会产生其他请求。

正确链路：浏览器请求 `/orders/9` → Nginx 内部返回 `index.html` → 浏览器加载 JS → Vue Router 读取仍然保留的 `/orders/9` → 渲染订单组件。

#### 9. 多次混淆：最终 image = 目标阶段的基础内容 + 该阶段的改动

**当时的错误：**先把“不包含 Node”解释成“没有引入 assets 阶段的资源”；修正后，又说最终 image 只有 dist、连 Nginx 也不包含。

对完整前端 Dockerfile，应同时计算 `FROM` 和 `COPY`：

```text
FROM nginx:alpine
  提供 Nginx、Alpine 基础文件系统及默认启动配置
  + COPY nginx.conf ...
  + COPY --from=assets /workspace/dist ...
  = 最终 production image
```

`COPY --from=assets` 的意思是从该阶段选择性复制文件，既不是“整个 assets 都继承”，也不是“完全没有使用 assets”。

| 内容 | 本例最终前端 image 中是否包含 | 原因 |
| --- | --- | --- |
| Nginx | 是 | 来自 `FROM nginx:alpine` |
| 自定义 Nginx 配置 | 是 | 来自相应 `COPY` |
| dist 产物 | 是 | 来自 `COPY --from=assets` |
| Node 可执行程序 | 否 | 最终基础镜像未提供，也没有复制进来 |
| 完整 node_modules | 否 | 未从 assets 复制进来 |
| 原始 Vue 源码目录 | 否 | 未把源码目录复制到最终阶段 |

“没有源码目录”不等于绝不可能泄露源代码：如果 dist 包含 source map 等产物，仍需看实际输出配置。

每条 `FROM` 开启一个 stage，`AS assets`、`AS production` 只是阶段别名，不是运行中的 container，也不是自动生成的 image 名。两个阶段都在 `docker build` 期间处理；“runtime stage”只是说它的产物将成为运行环境。

```sh
docker build --target production -t resume-site .
docker build --target assets -t resume-builder .
```

前一条导出 Nginx 运行镜像；后一条导出包含 Node、依赖、源码及构建产物的构建阶段。`--target` 选择阶段，`-t` 给导出的 image 命名，最后的 `.` 是构建上下文。

如果 Dockerfile 在 `production` 后还有 `FROM ... AS debug`，默认构建目标会变成最后的 debug；需要 `--target production` 选择 production。BuildKit 按阶段依赖构建，目标不依赖的 debug 不会仅因写在文件中就进入目标镜像。

中间阶段可保留构建缓存；“没有进入最终 image”不等于“本机完全没保留任何构建缓存”。也不能直接 `docker run assets` 来运行某个 stage，除非事先将它导出并命名为 image。

**反例：**删掉把 dist 复制到 Nginx 的那条 `COPY`，Nginx 依然可能正常运行，只是它没有这个 Vue 项目的构建产物。页面不对不等于服务进程没启动。

#### 10. 反复确认：缓存依赖上游状态，不只看当前指令文本

先用一个简化模型：

```text
缓存匹配输入 ≈ 当前指令 + 相关输入文件／参数 + 上游构建状态
```

**当时的错误：**先不能判断源码修改为何导致 `npm ci` 重跑；后来能正确定位 JAR 的 `COPY`，却仍把其后的 `USER`、`EXPOSE` 说成可沿用旧缓存。最后才补全“父 image 状态已变”的解释。

常规线性链中，某步变化后，依赖它的下游接收到不同状态，不能直接沿用旧链的结果；不是 Docker 从此“不再检查缓存”。独立阶段要按自己的依赖分析。

为什么先复制依赖清单？

```dockerfile
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build
```

假设基础镜像和其他构建参数没变，且已有对应缓存：

| 变更 | 复用到哪里 | 从哪里重新处理 |
| --- | --- | --- |
| 只改 `src/App.vue` | `COPY package*.json`、`RUN npm ci` | `COPY . .` 及其下游 |
| 改 `package-lock.json` 内容 | 前面的基础镜像、工作目录等 | 复制依赖清单及其下游 |
| 先 `COPY . .` 再 `RUN npm ci`，只改 App.vue | 只能到广泛复制源码之前 | `COPY . .`，导致安装依赖也受影响 |

Docker 不会自动推理“npm ci 其实只关心 lockfile”，需要通过 Dockerfile 顺序把这个缓存边界表达出来。

多阶段前端中，只修改 App.vue，并假设新 dist 内容随之变化：

```text
assets：
  FROM / WORKDIR / COPY package*.json / RUN npm ci → 可复用
  COPY . . / RUN npm test && npm run build         → 重新处理

production：
  FROM nginx:alpine                                → 可复用
  COPY nginx.conf ...                              → 配置没变，可复用
  COPY --from=assets ...dist...                     → 产物变了，重新处理
```

不要因为 assets 有变化，就断言 production 从第一行全部失效。跨阶段依赖实际在复制相应产物时接入；如果复制输入的内容保持相同，也要据此分析。

**API runtime 的重点反例：**本节后文完整安全 Dockerfile 的顺序是 `FROM` → 创建用户 → `WORKDIR` → 复制 JAR → `USER` → `EXPOSE` → `HEALTHCHECK` → `ENTRYPOINT`。

| 变化 | 可复用的前段 | 重新处理的部分 |
| --- | --- | --- |
| Java 源码变化，且 JAR 内容变化 | runtime 的 `FROM`、创建用户的 `RUN`、`WORKDIR` | 从 JAR 的跨阶段 `COPY` 开始，连同后续元数据指令 |
| JAR 不变，只把 `USER app` 改成 `USER app:app` | 可以复用到 JAR 的 `COPY` | 从变更的 `USER` 开始及其下游 |

`USER`、`EXPOSE`、`ENTRYPOINT` 不启动昂贵的构建进程，不等于它们可以脱离变更的上游状态复用旧结果；重新处理这些元数据的成本通常很低。

#### 11. MySQL 初始化看数据目录，不看 container 是不是新建

**当时的错误：**修改初始化密码后，认为删掉并重建 container 就会让新密码生效。忽略了新 container 又挂回同一个非空的 named volume。

MySQL 官方镜像的首次初始化脚本和初始化用户／密码，关键条件是挂载的数据目录是否已经初始化。本例是 `mysql-data:/var/lib/mysql`；`init.sql` 则只读挂载到 `/docker-entrypoint-initdb.d/01-init.sql`。

| 操作 | 数据卷是否保留 | 下次启动会否按首次初始化重新建库／用户 |
| --- | --- | --- |
| `docker compose stop` / `start` | 保留 | 已初始化时不会 |
| 普通 `docker compose down` 再 `up -d` | 保留本例 named volume | 不会 |
| 只删 db container 再重建 | 同一个 named volume 仍保留 | 不会 |
| `docker compose down --volumes` 后再 `up -d` | 本例非 external 的项目卷被删除 | 创建空卷后重新初始化，旧数据也被删除 |

> [!danger] `down --volumes` 是删数据，不是普通重启
> 这里仅用于可丢弃的学习数据库。需要保留数据库时，修改真实账户密码应通过数据库管理操作（如 `ALTER USER`），同时更新应用连接配置；不能靠反复修改初始化环境变量来变更现有账户。

同理，改了 `init.sql` 再普通 `down/up`，不代表脚本会自动重跑。正式数据库结构升级需要单独的迁移流程，不能把首次初始化目录当成每次启动的迁移工具。

#### 12. 两次混淆：安装了依赖、构建时执行、打包进 dist 是三件事

**当时先说：**`node_modules` 的内容会经过编译进入 dist；反例中又把“Vitest 不进入 dist”归因于它属于 devDependencies。

正确的三条判断线：

```text
npm ci 安装了什么
       ≠ 构建期间实际执行了什么
       ≠ bundler 最终把哪些模块写入 dist
```

当前前端 Dockerfile 的构建阶段执行普通 `npm ci`，没有省略开发依赖；随后用测试工具和 Vite 构建。普通 Vue/Vite 示例中，生产代码的 import graph、tree shaking 等 bundler 行为决定实际纳入的代码，不能只按 package.json 的分类猜。

| 依赖情形 | 当前例子中是否进入 dist | 判断原因 |
| --- | --- | --- |
| Vitest 只运行测试，生产代码没 import 它 | 通常不进入 | 不属于生产入口可达的打包模块 |
| lodash 即便放在 devDependencies，仍被生产代码 import 并使用 | 相关代码可能进入 | 它可由生产 import graph 到达；具体保留多少受 bundler 优化影响 |
| npm 安装的其他包 | 不能一概判断 | 需要看用途、import graph、插件、静态资源复制等构建行为 |

更准确的记忆链是：**生产入口 → import graph → bundler 处理与优化 → dist**。

最终 Nginx image 没有完整 `node_modules`，仍不能直接忽略构建依赖漏洞：一方面要看产物是否包含受影响代码；另一方面依赖可能通过 lifecycle script、测试器、Vite plugin 等在构建时执行，影响构建环境或产物。漏洞报告也不等于已经确认存在恶意 hook，需要进一步看触发条件。

原对话当时的 `npm audit` 报告是 `1 high + 1 critical`，这是历史结果，不代表 2026-10-29 重新扫描后的状态。没有把强制升级或漏洞修复记成已经完成。

### Dockerfile 的基础语法补充

| 指令／参数 | 何时起作用 | 本例要记住的含义 |
| --- | --- | --- |
| `FROM` | 构建时 | 开始一个 stage，选择基础 image |
| `WORKDIR /workspace` | 构建时设置，并保留默认工作目录配置 | 后续相对目标路径在这里解析；不是挂载卷 |
| `COPY package*.json ./` | 构建时 | 从构建上下文复制依赖清单到当前工作目录 |
| `RUN npm ci` | 构建时 | 执行安装，结果用于后续构建；不是每次 container 启动都执行 |
| `CMD [...]` | 保存默认启动配置，运行时使用 | image 的默认命令；显式运行命令可以覆盖它 |
| `ENTRYPOINT [...]` | 保存启动入口，运行时使用 | 常用于固定启动程序；exec form 的 CMD／运行参数可作为它的参数 |
| `USER app` | 影响后续相应构建操作，并保留运行用户配置 | 要先确保当前阶段存在该用户及必要权限 |
| `EXPOSE 8080` | image 元数据 | 声明预期服务端口，不发布 host 端口 |
| `HEALTHCHECK` | 声明检查方式，container 运行期间执行 | 检查服务状态；不会替代主启动命令 |
| `--target production` | `docker build` 命令行 | 选择导出哪个阶段，不写在 Dockerfile 内 |
| `-t resume-site` | `docker build` 命令行 | 给构建的 image 指定名称／tag |

#### 构建上下文、两个点和 dockerignore

```sh
docker build -t resume-site .
```

最后的 `.` 是构建上下文。在本地目录构建的例子里，普通 `COPY` 的源路径相对于上下文解析，不是相对于 container，也不能当作任意读取 Mac 文件系统的命令。

```dockerfile
WORKDIR /workspace
COPY . .
```

第一个点是构建上下文的根；第二个点是 image 内当前工作目录，这里是 `/workspace`。`.dockerignore` 排除的本地文件不进入对应构建输入。排除本机 `node_modules`，也能避免把 Mac 上安装的原生依赖直接覆盖到 Linux 构建环境。

一条 `COPY` 可以列出多个源，最后一个参数是目标目录，例如 `COPY package.json package-lock.json ./`。分开多条 COPY 还能划分缓存边界。

`WORKDIR` 只设置工作目录，不等于持久化；反过来，挂载 `-v notes:/app` 也不会自动把当前工作目录切换到 `/app`。挂载到已有目录时，image 中该处文件的可见性还会受挂载内容影响。

#### 注释、CMD 与 ENTRYPOINT

Dockerfile 的说明注释单独占行：

```dockerfile
# 容器默认启动命令
CMD ["npm", "run", "preview"]
```

不要在 JSON exec form 后追加 `# 说明`。Dockerfile 解析器不会统一把行尾 `#` 当注释；shell form 的 RUN 即使碰巧能由 shell 处理，也不值得混用。

原对话中的行尾备注是回答时标记“这行参考了上下文”，并不一定真的写入文件，因此这属于**语法补充**，不应算成反复犯错。

以 Java API 为例，`ENTRYPOINT ["java", "-jar", "/app/app.jar"]` 固定了启动入口；想临时使用 shell，要明确覆盖入口，例如 `docker run --rm --entrypoint sh resume-api:1.0`。不能把“在 image 后写 sh，一定能进 shell”当成通则。

### 完整配置总参考

下面保留学习项目的完整 Docker 配置，业务代码仍在原项目中。前端采用 Node 构建、Nginx 运行；API 采用 Maven 构建、JRE 运行；MySQL 使用现成镜像。image tags 沿用当时教学示例，不表示最新版本。

原项目目录：`/Users/yan/Documents/Codex/2026-07-23/6-vue3-srping-boot-spring/sigma/docker/examples/fullstack-compose`。

```text
fullstack-compose/
├── compose.yaml
├── frontend/
│   ├── Dockerfile
│   ├── .dockerignore
│   ├── nginx.conf
│   ├── package.json
│   ├── package-lock.json
│   └── src/
├── api/
│   ├── Dockerfile
│   ├── .dockerignore
│   ├── pom.xml
│   └── src/
└── db/
    └── init.sql
```

#### frontend/Dockerfile

```dockerfile
FROM node:22-alpine AS assets

WORKDIR /workspace

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm test && npm run build

FROM nginx:alpine AS production

COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=assets /workspace/dist /usr/share/nginx/html
```

没有另写 CMD，是因为 Nginx 基础镜像已经提供默认入口／启动配置。Node 在这里承担构建工作；Nginx 提供静态 HTTP 服务；浏览器执行前端 JavaScript。普通 Vue SPA 的 Nginx 运行镜像不需要完整 Node 环境；SSR 或 Node API 则是另一种运行需求。

Vite 的 `npm run preview` 用于本地预览构建产物，不作为这里的生产部署方案。

`frontend/.dockerignore`：

```text
node_modules
dist
.git
.DS_Store
npm-debug.log*
```

#### frontend/nginx.conf

```nginx
server {
    listen 80;
    server_name _;

    root /usr/share/nginx/html;
    index index.html;

    location /api/ {
        proxy_pass http://api:8080;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

`/api/` 请求交给 API；普通前端路由使用 SPA fallback。把这两类路径分开，能避免 API 错误被一个 `index.html` 掩盖。

这里 `proxy_pass http://api:8080` **没有尾部 `/`**，因此 `/api/projects` 按原路径传给后端。本例如果写成 `proxy_pass http://api:8080/`，在这个 location 下会把匹配的 `/api/` 前缀替换成 `/`，请求变成 `/projects`；不能随意加斜杠。

因为配置引用 Compose 内部名称 `api`，应在项目网络内验证：

```sh
docker compose exec frontend nginx -t
```

单独在不认识 `api` 的网络里运行这个配置，可能因为 upstream 无法解析而失败，不应直接推断 Nginx 配置语法写错。

#### api/Dockerfile：项目当时的基础版本

```dockerfile
FROM maven:3.9.11-eclipse-temurin-21-alpine AS build

WORKDIR /workspace

COPY pom.xml ./
RUN mvn -DskipTests dependency:go-offline

COPY src ./src
RUN mvn test package

FROM eclipse-temurin:21-jre-alpine AS runtime

WORKDIR /app

COPY --from=build /workspace/target/resume-api-0.0.1-SNAPSHOT.jar /app/app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

JAR 文件名来自这份项目的 Maven 配置；换项目时要对应修改。构建阶段有 Maven、源码和测试，最终阶段保留 JRE 与 JAR。不要把 `build` 阶段有的工具、用户或文件当成会自动进入这个独立基础镜像的 `runtime`。

`api/.dockerignore`：

```text
target
.git
.idea
*.iml
```

#### compose.yaml：三服务完整配置

以下密码是原学习项目的演示值。

```yaml
name: resume-stack

services:
  frontend:
    build:
      context: ./frontend
      target: production
    image: resume-frontend:1.0
    ports:
      - "8088:80"
    depends_on:
      - api

  api:
    build:
      context: ./api
      target: runtime
    image: resume-api:1.0
    environment:
      SPRING_DATASOURCE_URL: jdbc:mysql://db:3306/resume?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=Asia/Shanghai
      SPRING_DATASOURCE_USERNAME: resume_app
      SPRING_DATASOURCE_PASSWORD: learning-password
    depends_on:
      db:
        condition: service_healthy

  db:
    image: mysql:8.4
    environment:
      MYSQL_DATABASE: resume
      MYSQL_USER: resume_app
      MYSQL_PASSWORD: learning-password
      MYSQL_ROOT_PASSWORD: learning-root-password
    volumes:
      - ./db/init.sql:/docker-entrypoint-initdb.d/01-init.sql:ro
      - mysql-data:/var/lib/mysql
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost", "-u", "root", "--password=learning-root-password"]
      interval: 5s
      timeout: 3s
      retries: 10

volumes:
  mysql-data:
```

`./db/init.sql:...:ro` 是 bind mount：左边是项目内的宿主机文件，`ro` 是只读。`mysql-data:/var/lib/mysql` 是 named volume：左边是 Docker 管理的数据对象。两者不能因为都写在 `volumes` 下就混为一谈。

这个 Compose project 下，实际卷名默认带 project 前缀，通常是 `resume-stack_mysql-data`；不能把另一个项目里同名的逻辑键就当成同一个物理卷。

`db/init.sql`：

```sql
USE resume;

CREATE TABLE projects (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(120) NOT NULL,
    summary VARCHAR(500) NOT NULL,
    technologies VARCHAR(255) NOT NULL,
    display_order INT NOT NULL
);

INSERT INTO projects (title, summary, technologies, display_order)
VALUES (
    'Containerized Resume Site',
    'Vue, Spring Boot and MySQL orchestrated with Docker Compose.',
    'Vue 3, Spring Boot, MySQL, Docker Compose',
    1
);
```

#### 启动、验证与临时 client

在 `fullstack-compose` 根目录依次执行：

```sh
docker compose config
docker compose up --build -d
docker compose ps -a
docker compose exec frontend nginx -t
curl --noproxy '*' http://localhost:8088/api/projects
docker compose logs --tail=100 frontend api db
```

浏览器访问 `http://localhost:8088`；Vue 使用相对路径 `fetch('/api/projects')`。由 Nginx 代理到 API，API 再通过 JDBC 查询数据库。

如需复现对话最后通过的内部 HTTP 练习，在同一份文件的 `services` 下增加以下 service，其他配置保留：

```yaml
  client:
    image: alpine
    command: ["sleep", "300"]
```

```sh
docker compose up -d client
docker compose exec client wget -qO- http://frontend:80/api/projects
```

`sleep` 是让 client 暂时保持运行的主命令；`wget` 是后来通过 `exec` 添加的命令。不要为了做一次请求而把 client 的长期主命令直接替换掉。默认网络已经提供内部访问，不必发布 client 端口。

#### 网络、共享卷与环境变量实验

这是另一份独立的完整 `compose.yaml`，不要与上面的三服务文件混在同一目录执行：

```yaml
name: network-lab

services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"

  client:
    image: alpine
    command: ["sleep", "300"]

  writer:
    image: alpine
    environment:
      MESSAGE: compose
    command: ["sh", "-c", "printenv MESSAGE >> /data/messages.txt; sleep 300"]
    volumes:
      - app-data:/data

  reader:
    image: alpine
    command: ["sleep", "300"]
    volumes:
      - app-data:/backup

volumes:
  app-data:
```

在这个实验目录执行，并在 300 秒内进行 exec：

```sh
docker compose up -d
docker compose exec client wget -qO- http://web:80
docker compose exec writer printenv MESSAGE
docker compose exec reader cat /backup/messages.txt
```

writer 与 reader 共享文件数据，但不共享进程环境。`docker compose exec reader printenv MESSAGE` 在变量未定义时没有内容输出，通常退出码为 `1`；这和“显式设置了一个空字符串变量”不同。

从全新实验卷开始，第一次 writer 启动会追加一行 `compose`。普通 `down` 后再 `up -d`，卷保留、新 writer 再执行一次主命令，因此通常累计两行。只修改 `MESSAGE` 为 `hello` 后 `up -d`，不需要 build Alpine image；writer 按新配置重建，向原文件追加 `hello`。

### 镜像安全、非 root 与健康检查

#### 多阶段构建降低攻击面，但不消除所有风险

最终 API image 不包含 Maven、源码、构建缓存和构建依赖，减少了可被利用的软件组件，以及攻击者可能直接使用的构建工具。但 JRE、应用代码和 JAR 内的运行依赖仍可能有漏洞。

非 root 运行限制应用在 container 内能修改的文件和能执行的权限操作。container 内的 root 不等于直接获得 Mac root，但危险挂载、额外 capabilities 或内核漏洞可能扩大影响。多阶段构建与非 root 解决的是不同边界，不能互相替代。

当前 API 的 runtime 从独立的 JRE 基础 image 开始，因此 `app` 用户应在 **runtime** 中创建。顺序是：创建用户／组 → 复制并设置所有者 → 切换用户 → 保留启动入口。`COPY --chown=app:app` 使用名字时，当前阶段必须能解析这个用户／组。

#### running、healthy 与业务可用

| 判断 | 能说明什么 | 不能直接证明什么 |
| --- | --- | --- |
| `running` | container 的主进程仍在运行 | HTTP 能正常响应、数据库可用 |
| `healthy` | 配置的健康检查按规则通过 | 检查没覆盖的业务链路也一定正常 |
| 本例 HTTP 查询成功 | 当前 `/api/projects` 路径可以工作 | 所有接口、所有业务状态都正常 |

检查 Java 进程存在，不足以发现“进程活着但请求一直 500”。本例选择实际请求 `/api/projects`，能覆盖更多应用链路；正式项目的健康端点应按依赖和业务需求设计。

`HEALTHCHECK` 在 **container 内**执行，所以这里的 `localhost:8080` 指 API 自己，与 Mac 上发布的 8088 无关。示例用退出码 `0` 表示成功、`1` 表示失败；健康状态与进程是否退出是两套状态。

Compose `depends_on` 也分两种：

- 短写 `depends_on: [db]`：控制依赖启动顺序，不等于等待数据库已可响应。
- 长写 `condition: service_healthy`：等待配置的健康检查通过，再启动依赖它的 service。

`service_healthy` 是依赖条件，不是 container 的 `.State.Status` 值。它控制启动门槛；数据库后来变为 unhealthy，不代表 Compose 自动停止／重启 API。单独配置 HEALTHCHECK，也不会让 Docker 自动重启一个 unhealthy 但主进程仍存活的 container。

本例 `mysqladmin ping` 主要检查数据库服务可响应，不能替代真实账号连接和查询验证；尤其不能只凭它就断言应用密码、表结构和 SQL 都正确。

#### 待完成练习与完整参考答案

原对话最后要求独立改写完整 API Dockerfile：在 runtime 创建 app 用户／组、复制 JAR 时设置所有者、非 root 运行，并添加 HTTP 健康检查：间隔 10 秒、超时 3 秒、连续失败重试阈值 5 次。

> [!note] 学习状态
> 下面是本次整理补全的参考实现。原 thread 停在出题位置，尚没有这道独立练习通过的记录；本次也没有重新构建或运行这份加固版本。

```dockerfile
FROM maven:3.9.11-eclipse-temurin-21-alpine AS build

WORKDIR /workspace

COPY pom.xml ./
RUN mvn -DskipTests dependency:go-offline

COPY src ./src
RUN mvn test package

FROM eclipse-temurin:21-jre-alpine AS runtime

RUN addgroup -S app \
    && adduser -S app -G app

WORKDIR /app

COPY --from=build --chown=app:app \
    /workspace/target/resume-api-0.0.1-SNAPSHOT.jar /app/app.jar

USER app

EXPOSE 8080

HEALTHCHECK --interval=10s --timeout=3s --retries=5 \
    CMD wget -q -O /dev/null http://localhost:8080/api/projects || exit 1

ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

`addgroup`／`adduser` 的参数按当前 Alpine 基础镜像编写，不能原样套到所有发行版；这份例子使用 Alpine 的 wget。`USER` 之前完成需要 root 的创建操作，之后 API 与健康检查按指定用户运行。JAR 由 app 所有并不表示应用应获得整个系统目录的写权限。

以后实际练习时，在同一个 Compose 项目中重建 API、观察健康状态与用户：

```sh
docker compose up --build -d api
docker compose ps -a
docker compose exec api id
docker inspect --format '{{json .State.Health}}' "$(docker compose ps -q api)"
```

能复制答案不等于能解释：需要自己说清为什么在 runtime 建用户、为什么 `chown` 在 `USER` 前、为什么健康请求用 localhost、为什么健康检查失败不等于主进程退出。

### 本次实践中的真实故障

以下是原对话当时实际遇到的问题和验证结果，不表示当前机器仍处于相同状态。

#### 1. Docker daemon 拉镜像超时，与终端能访问外网不是一回事

现象：拉取 `mysql:8.4` 连续超时，而 Mac 直接访问 registry 可以成功；重启 OrbStack 后仍然失败。

原对话排查发现：shell 代理变量指向 `127.0.0.1:7897`，但 macOS 系统代理关闭，OrbStack 的 `network_proxy` 为 `auto`。当时确认 auto 跟随系统代理，并不是自动读取当前 zsh 的代理环境变量；开启系统代理后，MySQL image 拉取成功。后续 Maven image 也出现过同类代理路径问题。

**排障顺序：**区分 Mac shell、Docker CLI 和实际下载镜像的 daemon；检查各自走的网络路径，再判断是 registry、DNS、代理还是应用构建问题。`127.0.0.1:7897` 是当时本机代理的值，不是所有机器通用的 Docker 配置。

重启 OrbStack 会影响它管理的其他 container；在这个案例里，重启并未直接修复代理问题。

#### 2. Documents 下的 bind mount 报 Operation not permitted

现象：MySQL 无法读取挂载的 SQL；用 Alpine 做最小复现，同样无法读取 Documents 下的文件，但挂载 `/etc/hosts` 可以读取。

这个对照排除了“SQL 写错”或“Compose 语法错”，把问题缩小到 macOS 对 OrbStack 的 Documents 访问权限。授予文稿目录访问权限后，最小 Alpine 读取成功，随后干净数据库初始化也通过。

本案优先检查系统隐私权限及文件访问范围，不应把 macOS 隐私限制一律当成 Unix mode bits 问题，更不应直接用 `chmod 777` 掩盖。

#### 3. host 的 8080 已被其他项目占用

原本设计发布 `8080:80`，落地时发现 Mac 的 8080 已被另一个项目使用，因此只把教学项目改为 `8088:80`。

- 浏览器入口变为 `http://localhost:8088`。
- Nginx 仍监听 container 内的 80。
- Spring Boot 仍监听自己的 8080，Nginx 继续连接 `api:8080`。

多个 container 的内部 8080 可以各自存在；争用的是同一 host 上绑定的同一入口。不要为了 host 端口冲突而把所有内部端口一起改掉。

#### 4. 代理可能干扰本地 HTTP 验证

当时为了排除本地请求经过代理，使用：

```sh
curl --noproxy '*' http://localhost:8088/api/projects
```

这只绕过该次 curl 请求的代理，并不是修改 Docker daemon 的代理配置。

#### 5. 页面正常但接口 502，应先看哪一跳

对话中的故障判断题是：Vue 页面正常，`/api/projects` 返回 502，frontend running、api exited (1)、db healthy。

优先怀疑 Nginx → API 这一段，先查看 API 输出和完整状态：

```sh
docker compose logs --tail=100 api
docker compose ps -a api
docker compose logs --tail=100 frontend
```

API 已停止时不要先对它 `exec`。等 API 保持运行后，再从内部请求接口，逐跳确认网络与业务：

```sh
docker compose exec frontend wget -qO- http://api:8080/api/projects
```

502 倾向于代理无法正常获得上游响应；若 API 能接收请求但查询数据库失败，可能是应用返回 500，需继续看 API 错误。不要把所有“页面没数据”都归因于前端构建或 Docker 端口映射。

原对话完整项目当时已验证：API 2/2 测试、Vue 2/2 测试与生产构建通过；页面可展示数据库记录；普通 down 保留数据；删除练习卷后可重新初始化。这些是当时项目的验证证据，与上面的安全改造练习是否独立通过分开记录。

### 常用命令与排障速查

| 目的 | 命令 | 注意点 |
| --- | --- | --- |
| 看运行中 container | `docker ps` | 不展示普通的已退出实例 |
| 包含已退出 container | `docker ps -a` | 区分退出与删除 |
| 看应用输出 | `docker logs --tail=100 NAME` | 已停止但未删除时仍可读取已有日志 |
| 跟踪输出 | `docker logs -f NAME` | 跟踪客户端与服务主进程不是一回事 |
| 临时进入运行 container | `docker exec -it NAME sh` | image 要有 sh，container 要能执行新进程 |
| 精确看状态 | `docker inspect --format '{{.State.Status}}' NAME` | 原始状态值如 running、exited |
| 精确看退出码 | `docker inspect --format '{{.State.ExitCode}}' NAME` | 不是从日志猜退出码 |
| 看挂载 | `docker inspect --format '{{json .Mounts}}' NAME` | 区分 source、destination 与挂载类型 |
| 看 Compose 完整状态 | `docker compose ps -a` | 使用 service 视角，包含停止实例 |
| 校验解析后的编排配置 | `docker compose config` | 语法通过不等于所有服务能工作 |
| 明确构建并后台协调 | `docker compose up --build -d` | 有效构建缓存仍会复用 |
| 只协调某个服务及必要依赖 | `docker compose up -d client` | 本例 client 没有 depends_on；不会凭空启动所有服务 |
| 停止、保留 container | `docker compose stop` | 日志、配置和可写层仍属于原 container |
| 启动已有 container | `docker compose start` | 不应用新的运行配置、不创建已被 down 删除的实例 |
| 停止并删除项目 container／网络 | `docker compose down` | 本例默认保留 image、named volume |
| 重置可丢弃的项目数据 | `docker compose down --volumes` | 有数据删除后果，不能当普通关闭命令 |
| 额外移除相关 image | `docker compose down --rmi all` | `--rmi` 后需要值，不是单独开关 |

`--format` 用的是 Go template，无需为了 Docker 系统学习 Go。`{{.State.Status}}` 从当前结构化对象取字段；`{{json .Mounts}}` 把字段转成 JSON。`docker ps --format` 的字段如 `.Names`、`.Status` 和 inspect JSON 的嵌套字段不完全相同。

**退出日志界面与停止服务：**`docker compose up` 前台运行时，Ctrl+C 会结束这次 up 并停止相关 container；先 `up -d` 再 `docker compose logs -f` 时，Ctrl+C 只是结束日志跟踪，服务继续运行。

状态术语也要准确：`up` 是命令，`running`／`exited` 是 container 状态，`healthy`／`unhealthy` 是健康状态。不要再把“归还终端”“up”“等待指令”混作 container 状态。

### 复习自测与答案

先不看答案，用一句“结论 + 原因”回答。重点不是背出术语，而是能把新场景推导出来。

1. 同一 named volume 在两个 container 中分别挂到 `/cache` 和 `/restore`，第二个应从哪里读取第一个写的 `tool`？
2. `docker run --rm -v notes:/data alpine echo hi` 完成后，image、container、named volume 各自还在吗？
3. `sh -c 'echo start; exit 7'` 已退出，logs 是空的吗？退出码从哪里看？
4. 终端 A 前台运行 `sleep 60` 的 container，终端 B 能及时 exec 吗？如果主进程是 echo，结果有何不同？
5. `echo hello; sleep 120` 不加 `sh -c '...'` 时，分号由哪一层解释？`> echo` 又会做什么？
6. 修改了 COPY 进 image 的 Nginx 配置，再 build 同一个 tag，只 restart 旧 container 会生效吗？
7. 只改 Compose `ports` 从 `8080:80` 到 `9090:80`，需要 build image 吗？实例名称不变，是否说明没有 recreate？
8. 同一 Compose 网络内 client 请求 Nginx，用 `frontend:80` 还是 `localhost:8088`？Mac 浏览器呢？删 ports 后分别怎样？
9. 刷新 `/orders/9` 时，Nginx 回退到 index.html，为何地址栏仍是 `/orders/9`？
10. 最终阶段是 Nginx，并只从 Node 阶段复制 dist，最终 image 是否“只有 dist”？
11. JAR 的跨阶段 COPY 内容变了，后面的 USER／EXPOSE 文本没变，是否直接沿用旧链缓存？只改 USER 又从哪里失效？
12. 改 MySQL 初始化密码，再删 container、保留 mysql-data，新密码会自动生效吗？
13. test-only 的 Vitest 和生产代码 import 的 lodash，能否只凭 devDependencies 分类判断谁进 dist？
14. API running，但 HTTP 持续 500，是否就算健康？healthcheck 失败会自动重启主进程吗？

> [!faq]- 参考答案：完成独立判断后再展开
> 1. 从第二个 container 的 `/restore/tool` 读取。source 是同一卷，target 可以不同。
> 2. alpine image 和显式命名的 notes 卷保留，临时 container 退出后删除。
> 3. logs 有 `start`；用 inspect 的 `.State.ExitCode` 看 `7`。exit 本身不打印文字，也不清空旧输出。
> 4. 在主进程仍运行、container 可执行新进程时可以，与是否 -d 无直接关系。echo 很快结束后，container 已退出，exec 失败。
> 5. 未引用的分号先由 Mac shell 解释；`> echo` 把输出写入名为 echo 的文件。
> 6. 不会。旧 container 仍绑定旧 image，需要基于新 image recreate。
> 7. 不需要 build；up 按新端口 recreate。container 名可以复用，ID 才会变。
> 8. client 用 `frontend:80`，Mac 用发布的 `localhost:8088`。删 ports 仍保留内部入口，失去原 host 发布入口。
> 9. fallback 是 Nginx 内部选文件，没有让浏览器进行 302 重定向；Vue Router 读取保留的 URL。
> 10. 不是。还包含 Nginx 基础镜像的文件和启动配置，以及本阶段添加的 Nginx 配置。
> 11. 不能直接沿用旧上游链结果；JAR 变化从 COPY 接入下游。只改 USER，则此前的 JAR COPY 可以复用，从 USER 及其下游重新处理。
> 12. 不会。数据库已在保留卷中初始化，重建 container 不等于清空数据库；保留数据改密码需操作真实账户并更新应用配置。
> 13. 不能。要看生产 import graph、bundler 行为和构建时执行风险。
> 14. running 只说明主进程存活；是否 healthy 取决于检查定义，HEALTHCHECK 本身不会自动重启一个仍在运行的主进程。

### 简历与面试表达的边界

这是依据已做项目整理的表达草稿，并非原对话已完成第 10 项训练：

> 在个人练习项目中，使用多阶段 Dockerfile 构建 Vue 前端和 Spring Boot API 镜像，以 Nginx 提供静态资源、SPA 路由回退和 API 反向代理；通过 Docker Compose 编排前端、API、MySQL，配置服务发现、数据库持久化与启动健康依赖，并排查镜像拉取、挂载权限、端口冲突及上游连接问题。

能独立复现并解释后再采用这类描述。不要把“个人学习项目”改写成生产环境经验；没有测量数据，也不写镜像体积减少或部署加速的百分比。非 root 和 API HTTP HEALTHCHECK 仍是原 thread 尚未完成的独立练习，不应记成已验证落地。

### 来源与继续查阅

- 主要来源：[学习 Docker 全套实践](codex://threads/019f8f6a-a20e-7c01-b885-bf7a8268f7dc)，119 轮学习对话；本文将相近误区合并为 12 组，重点保留复发过程和纠正依据。
- 交叉核对：原项目 `sigma/docker/session.md` 中的误区表、学习进度，以及 `examples/vue-nginx`、`examples/compose-network`、`examples/fullstack-compose` 中的完整配置与 README。
- 继续复习时，优先独立重写上述配置，再逐条解释“对象是谁、在哪执行、何时执行、数据保存在哪里”。
