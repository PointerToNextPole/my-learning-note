# 项目打包和自动化部署

## 一. 项目部署和DevOps

### 1.1. 传统的开发模式

在传统的开发模式中，开发的整个过程是按部就班就行：

<img src="https://s2.loli.net/2025/03/23/VjkB6pdHelsTgQq.jpg" alt="早期的开发模式" style="zoom:67%;" />

但是这种模式存在很大的弊端：

* 工作的不协调：开发人员在开发阶段，测试和运维人员其实是处于等待的状态。等到测试阶段，开发人员等待测试反馈bug，也会处于等待状态。
* 线上bug的隐患：项目准备交付时，突然出现了 bug，所有人员需要加班、等待问题的处理；



### 1.2. DevOps开发模式

DevOps 是 Development 和 Operations 两个词的结合，将开发和运维结合起来的模式：

![打开格局](https://s2.loli.net/2025/03/23/tqWRx5uzP3yEFne.jpg)

<img src="https://s2.loli.net/2025/03/23/z72bprFALVmUeu8.jpg" alt="DevOps模式" style="zoom:67%;" />

### 1.3. 持续集成和持续交付

伴随着 DevOps 一起出现的两个词就是持续集成和持续交付（部署）：

* CI 是 Continuous Integration（持续集成）；
* CD 是两种翻译：Continuous Delivery（持续交付）或 Continuous Deployment（持续部署）；

持续集成CI：

<img src="https://s2.loli.net/2022/09/20/gAtcfVDJn9UC3Yj.jpg" alt="持续集成" alt="https://tva1.sinaimg.cn/large/008i3skNgy1gtt3bgnnvbj60rs0c8dgu02.jpg" style="zoom:67%;" />

持续交付和持续部署：

<img src="https://s2.loli.net/2022/09/20/hOEJpPAwdBDLY1b.jpg" alt="持续交付" alt="https://tva1.sinaimg.cn/large/008i3skNgy1gtt3da1cd2j60rs0kkmz602.jpg" style="zoom:67%;" />

<img src="https://s2.loli.net/2022/09/20/J6FNMr9lobzOkWd.jpg" alt="持续部署" alt="https://tva1.sinaimg.cn/large/008i3skNgy1gtt3dtbewsj60rs0kk76a02.jpg" style="zoom:67%;" />

### 1.4. 自动化部署流程

<img src="https://s2.loli.net/2022/09/20/Yd9rQNGFqxOtPzf.jpg" alt="自动化部署的流程" alt="https://tva1.sinaimg.cn/large/008i3skNgy1gtt5buus2kj60wu0lgq7002.jpg" style="zoom:100%;" />



## 二. 购买云服务器

### 2.1. 注册阿里云的账号

云服务器我们可以有很多的选择：阿里云、腾讯云、华为云。

* 目前在公司使用比较多的是阿里云；
* 我自己之前也一直使用阿里云，也在使用腾讯云；
* 之前华为云也有找我帮忙推广他们的活动；

但是在我们的课程中，我选择目前使用更加广泛的阿里云来讲解：

我们需要注册阿里云账号

* https://aliyun.com/

* 注册即可，非常简单



### 2.2. 购买云服务器

购买云服务器其实是购买一个实例。

1. 来到控制台：

![image-20201204152032769](https://s2.loli.net/2025/03/23/qPnxwuS3N9D1zQV.jpg)



2. 创建实例，选择类型和配置

3. 配置网络安全组

![image-20201203103725892](https://s2.loli.net/2025/03/23/mdgSs2hAw4RolLK.jpg)

4. 创建实例

![image-20201203104249296](https://s2.loli.net/2025/03/23/6SWikcZzfHp4sUu.jpg)



## 三. 搭建服务器环境

### 3.1. jenkins自动化部署

#### 3.1.1. 安装Java环境

Jenkins 本身是依赖 Java 的，所以我们需要先安装 Java 环境：

* 这里我安装了Java1.8的环境

```shell
dnf search java-1.8
dnf install java-1.8.0-openjdk.x86_64
```



#### 3.1.2. 安装Jenkins

因为 Jenkins 本身是没有在 dnf 的软件仓库包中的，所以我们需要连接 Jenkins 仓库：

* wget 是 Linux 中下载文件的一个工具，`-O` 表示输出到某个文件夹并且命名为什么文件；
* rpm ：全称为 **The RPM Package Manage**，是 Linux 下一个软件包管理器；

```shell
wget –O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo

# 导入GPG密钥以确保您的软件合法
rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
# 或者
rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key
```

编辑一下文件 `/etc/yum.repos.d/jenkins.repo`

* 可以通过 vim 编辑

```
[jenkins]

name=Jenkins-stable

baseurl=http://pkg.jenkins.io/redhat

gpgcheck=1
```

安装 Jenkins

```shell
dnf install jenkins # --nogpgcheck(可以不加)
```

启动 Jenkins 的服务：

```shell
systemctl start jenkins
systemctl status jenkins
systemctl enable jenkins
```

Jenkins 默认使用 8080 端口提供服务，所以需要加入到安全组中：

![image-20201204173117359](https://s2.loli.net/2025/03/23/XQ5AMUPDZsWldCn.jpg)





#### 3.1.3. Jenkins用户

我们后面会访问 centos 中的某些文件夹，默认 Jenkins 使用的用户是 `jenkins`，可能会没有访问权限，所以我们需要修改一下它的用户：

修改文件的路径：`/etc/sysconfig/jenkins`

![image-20210825162827962](https://s2.loli.net/2025/03/23/8yV6hOo1kvrfj74.jpg)

之后需要重启一下 Jenkins：

```shell
systemctl restart jenkins
```



#### 3.1.4. Jenkins配置

打开浏览器，输入：http://8.134.60.235:8080/

* 注意：你输入自己的 IP 地址

获取输入管理员密码：

* 在下面的地址中 `cat /var/lib/jenkins/secrets/initialAdminPassword`

![image-20201203173047824](https://s2.loli.net/2025/03/23/Vz36k5ZNfa8wSpv.png)

可以安装推荐的插件：

![安装推荐的插件](https://s2.loli.net/2025/03/23/Xwv8tlxe2dKnUmW.jpg)



#### 3.1.5. Jenkins任务

**新建任务：**

![新建任务](https://s2.loli.net/2025/03/23/SCxjtXuPq1yifJR.jpg)

![image-20201204185613061](https://s2.loli.net/2025/03/23/QjwqlKIbYUzrPuS.jpg)

**配置项目和保留策略：**

![image-20210825160744119](https://s2.loli.net/2025/03/23/5HnFcrbZXGIuy1m.jpg)

**源码管理：**

![image-20210825160818122](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aba21574b5b84649a7f044126f590822~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.jpg)

**构建触发器：**

这里的触发器规则是这样的：

* 定时字符串从左往右分别是：分 时 日 月 周

```js
#每半小时构建一次OR每半小时检查一次远程代码分支，有更新则构建
H/30 * * * *

#每两小时构建一次OR每两小时检查一次远程代码分支，有更新则构建
H H/2 * * *

#每天凌晨两点定时构建
H 2 * * *

#每月15号执行构建
H H 15 * *

#工作日，上午9点整执行
H 9 * * 1-5

#每周1,3,5，从8:30开始，截止19:30，每4小时30分构建一次
H/30 8-20/4 * * 1,3,5
```



![触发器](https://s2.loli.net/2025/03/23/H2cpvm7O5uyNibn.jpg)

**构建环境：**

注意：我们需要搭建 Node 的环境

* 第一步：配置 Node的环境；
* 第二步：安装 Node 的插件；

![image-20201204190055096](https://s2.loli.net/2025/03/23/Z1CkNEchlm2wbqf.jpg)

第一步：配置Node的环境

![node环境](https://s2.loli.net/2025/03/23/BfvGxO8PAoJqUSF.jpg)

第二步：安装Node的插件

* 这里因为我已经安装过了，所以没有搜索到

![image-20201204185949452](https://s2.loli.net/2025/03/23/86HmhjZkfNBtEor.jpg)



构建执行的任务：

* 查看 Node 的版本等是否有问题；
* 执行 `npm install` 安装项目的依赖；
* 移除原来 mall_cms 文件的所有内容；
* 将打包的dist文件夹内容移动到 mall_cms 文件夹；

```shell
pwd
node -v
npm -v

npm install 
npm run build

pwd

echo '构建成功'

ls

# 删除/root/mall_cms文件夹里所有的内容
rm -rf /root/mall_cms/* 

cp -rf ./dist/* /root/mall_cms/
```



![执行构建任务](https://s2.loli.net/2025/03/23/9SKFMV1hnG4CfQa.jpg)



### 3.2. nginx安装和配置

#### 3.2.1. 安装nginx

后续我们部署会使用nginx，所以需要先安装一下nginx：

```shell
dnf install nginx
```

启动 nginx：

```shell
systemctl start nginx
systemctl status nginx
systemctl enable nginx
```



#### 3.2.2. 配置nginx

我们这里主要配置 nginx 的用户和默认访问目录：

配置用户：

![image-20210825163329209](https://s2.loli.net/2025/03/23/HE7tjNGTvezVU6s.jpg)



通过 Linux 命令创建文件夹和文件：

```shell
mkdir /root/mall_cms
cd /root/mall_cms
touch index.html

vi index.html
```



配置访问目录：

![image-20210825163406566](https://s2.loli.net/2025/03/23/L3rkEWCXTx48sYU.jpg)



