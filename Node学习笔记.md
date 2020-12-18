# Node学习笔记



## npm使用

#### 命令摘录

- npm version：除了返回npm的版本外，还可以返回其他关于包的信息，比如：你正在使用的node.js的版本，openSSL或者V8的版本
- npm list：显示当前npm所有的安装的包及其相关信息（比如：版本号）

- npm install系列

  - **npm install** **<font color=FF0000>=</font>** **npm i**。在git clone项目的时候，项目文件中并没有 node_modules文件夹，项目的依赖文件可能很大。直接执行，<font color=FF0000>npm会根据package.json配置文件中的依赖配置下载安装</font>。

  - **-global** **<font color=FF0000>=</font>** **-g**，全局安装，安装后的包位于系统预设目录下

  - **--save** **<font color=FF0000>=</font>** **-S**，<font color=FF0000>安装的包将写入package.json里面的dependencies</font>，<mark>dependencies：生产环境需要依赖的库</mark>

  - **--save-dev** **<font color=FF0000>=</font>** **-D**，<font color=FF0000>安装的包将写入packege.json里面的devDependencies</font>，<mark>devdependencies：只有开发环境下需要依赖的库</mark>

摘自：[npm install说明](https://www.jianshu.com/p/b3e407942ac5)



#### --save / --save-dev / --save-optional

npm install takes 3 exclusive, optional flags which save or update the package version in your main package.json:

npm install 提供了3种独立的、可选的用于保存和更新在你主要的package.json的包版本的标记

- <mark>-S, --save: Package will appear in your dependencies.</mark>
  - -S是--save的缩写，package将会出现在你的dependencies中（自动把模块和版本号添加到dependencies部分（生产环境））

- <mark>-D, --save-dev: Package will appear in your devDependencies.</mark>
  - -D是--save-dev的缩写，package将会出现在你的devDependencies中（自动把模块和版本号添加到devDependencies部分（开发环境））

- <mark>-O, --save-optional: Package will appear in your optionalDependencies.</mark>
  - -O是--save-optinal的缩写，package将会出现在你的optionalDependencies中

摘自：[npm 安装参数中的 --save-dev 是什么意思](https://segmentfault.com/q/1010000000403629)



#### npm init

在现代新建一个 JS 相关的项目往往都是从 `package.json` 文件开始的，不过这个文件里需要的字段实在是太多了，正常人都记不住，所以 <font color=FF0000>npm 官方提供了 **`npm init`** 命令帮助我们快速初始化 **`package.json`** 文件</font>。<mark>执行之后会有一个交互式的命令行让你输入需要的字段值，当然如果你想直接使用默认值，也可以使用 **`npm init -y`** 来超速初始化。</mark>

随着技术的快速发展，发现初始化 package.json 已经无法满足大家的需求了，<font color=FF0000>越来越多的项目需要进行整个项目的初始化。脚手架工具应运而生</font>，除了有通用的脚手架工具 yeoman, sao 之外，<font color=FF0000>很多项目也会开发针对自己项目的脚手架工具，例如 vue-cli, create-react-app 以及专门用来初始化 ThinkJS 项目的脚手架工具 think-cli</font>。

摘自：[你不知道的 npm init](https://zhuanlan.zhihu.com/p/45151808)

**npm init功能：**创建一个 package.json 文件 / 可用于设置新的或现有的 npm 软件包。

摘自：[npm init 全方位解读](https://juejin.im/post/6844903960831082503)



#### 查看全局安装（-g）的位置

```sh
npm root -g
```

运行结果：

<img src="https://s1.ax1x.com/2020/10/28/B1tKUO.png" style="zoom:75%;" />



#### 镜像设置

- 查看npm<font color=FF0000>默认</font>镜像地址

  ```sh
  npm config get registry
  ```

- 将npm<font color=FF0000>默认</font>镜像地址修改为淘宝

  ```sh
  npm config set registry https://registry.npm.taobao.org
  ```

  

