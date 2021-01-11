# Node学习笔记



## npm

**NPM**（node package manager），通常称为node包管理器。顾名思义，它的主要功能就是管理node包，包括：安装、卸载、更新、查看、搜索、发布等。



#### 基本使用

- **安装**

  ```sh
  npm install package-name  					#本地安装
  npm install package-name @"version" #安装特定版本
  npm install -g package-name  				#全局安装
  npm install     										#通过package.json安装，将项目依赖的包都在文件内声明
  ```

  安装之前，npm install会先检查，node_modules目录之中是否已经存在指定模块。如果存在，就不再重新安装了，即使远程仓库已经有了一个新版本，也是如此。

  如果你希望，一个模块不管是否安装过，npm 都要强制重新安装，可以使用`*-f`或`*--force`参数。

  ```sh
  npm install packageName -force
  ```

- **卸载**

  ```sh
  npm uninstall package-name
  ```

- **查看已安装的包**

  ```sh
  npm ls      						#查看所有
  npm ls package-name     #查看某安装包的具体信息
  ```

- **更新**

  ```sh
  npm update package-name
  ```

  **顺序**

  先到npm模块仓库查询最新版本（提供registry查询服务）==> 返回json对象(包含该模块所有版本信息；含有dist.tarball属性，属性值为该压缩包网址，访问下载源码) ==> 查询本地版本（若本地版本不存在或远程版本较新，则安装更新）
  npm install和npm update都是以此方式安装模块

- **搜索安装包**

  ```sh
  npm seach package-name
  ```

摘自：[npm和gem](https://blog.csdn.net/u011099640/article/details/53083845)



#### 命令摘录

- npm version：除了返回npm的版本外，还可以返回其他关于包的信息，比如：你正在使用的node.js的版本，openSSL或者V8的版本
- npm list：显示当前npm所有的安装的包及其相关信息（比如：版本号）

- npm install系列

  - **npm install** **<font color=FF0000>=</font>** **npm i**。在git clone项目的时候，项目文件中并没有 node_modules文件夹，项目的依赖文件可能很大。直接执行，<font color=FF0000>npm会根据package.json配置文件中的依赖配置下载安装</font>。
  
  - **-global** **<font color=FF0000>=</font>** **-g**，全局安装，安装后的包位于系统预设目录下
  - **--save** **<font color=FF0000>=</font>** **-S**，<font color=FF0000>安装的包将写入package.json里面的dependencies</font>，<mark>dependencies：生产环境需要依赖的库</mark>
  - **--save-dev** **<font color=FF0000>=</font>** **-D**，<font color=FF0000>安装的包将写入packege.json里面的devDependencies</font>，<mark>devdependencies：只有开发环境下需要依赖的库</mark>

另外：在package name后面添加@版本号，可以安装指定版本号的包

摘自：[npm install说明](https://www.jianshu.com/p/b3e407942ac5)

**补充：**

- npm info packageName：查看包的信息，及其历史版本的信息等
- npm config ls / npm config list：查看所有Node环境配置

- npm config set cache cache-path：设置缓存
- npm config set prefix prefix-path：设置路径

#### npm config相关使用

```sh
npm config set <key> <value> [-g|--global]
npm config get <key>
npm config delete <key>
npm config list [-l] [--json]
npm config edit
npm get <key>
npm set <key> <value> [-g|--global]
```

**Description**

npm gets its config settings from the command line, environmentvariables, `npmrc` files, and in some cases, the `package.json` file.

See npmrc for more information about the npmrc files.

See config for a more thorough discussion of the mechanisms involved.

The `npm config` command can be used to update and edit the contents of the user and global npmrc files.

**Sub-commands**

Config supports the following sub-commands:

- set：Sets the config key to the value. If value is omitted, then it sets it to "true".

```bash
npm config set key value
```

- `get`：Echo the config value to stdout.

```bash
npm config get key
```

- `list`：Show all the config settings. Use `-l` to also show defaults. Use `--json` to show the settings in json format.

```bash
npm config list
```

- `delete`：Deletes the key from all configuration files.

```bash
npm config delete key
```

- `edit`：Opens the config file in an editor. Use the `--global` flag to edit the global config.

```bash
npm config edit
```

摘自：[npm-config](https://docs.npmjs.com/cli/v6/commands/npm-config)



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

在现代新建一个 JS 相关的项目往往都是从 `package.json` 文件开始的，不过这个文件里需要的字段实在是太多了，正常人都记不住，所以 <font color=FF0000>npm 官方提供了 **`npm init`** 命令帮助我们快速初始化 **`package.json`** 文件</font>。执行之后会有一个交互式的命令行让你输入需要的字段值，当然如果你想直接使用默认值，也可以使用 **`npm init -y`** 来更加快速初始化的；其中：**`-y`** 表示所有的选项都是允许的。

随着技术的快速发展，发现初始化 package.json 已经无法满足大家的需求了，<font color=FF0000>越来越多的项目需要进行整个项目的初始化。脚手架工具应运而生</font>，除了有通用的脚手架工具 yeoman, sao 之外，<font color=FF0000>很多项目也会开发针对自己项目的脚手架工具，例如 vue-cli, create-react-app 以及专门用来初始化 ThinkJS 项目的脚手架工具 think-cli</font>。

摘自：[你不知道的 npm init](https://zhuanlan.zhihu.com/p/45151808)

**npm init功能：**创建一个 package.json 文件 / 可用于设置新的或现有的 npm 软件包。

摘自：[npm init 全方位解读](https://juejin.im/post/6844903960831082503)



#### 关于package-lock.json

npm 创建了一个 package-lock.json，这个文件就是用来**锁定全部直接依赖和间接依赖的精确版本号**，或者说提供了关于 node_modules 目录的精确描述，从而确保在这个项目中开发的所有人都能有完全一致的 npm 依赖。

摘自：[一杯茶的时间，上手 Node.js](https://zhuanlan.zhihu.com/p/97413574)



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

- 使用官方镜像

  ```sh
  npm config set registry https://registry.npmjs.org
  ```

- 通过cnpm

  ```sh
  npm install -g cnpm --registry=https://registry.npm.taobao.org
  ```

- 临时使用淘宝镜像

  ```sh
  npm --registry https://registry.npm.taobao.org install express
  ```

摘自：[npm换源](https://www.jianshu.com/p/f311a3a155ff)



#### npm scripts

**以下内容摘自：[npm scripts 使用指南](http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html)**

npm 允许在package.json文件里面，使用scripts字段定义脚本命令。

```js
{
  // ...
  "scripts": {
    "build": "node build.js"
  }
}
```

上面代码是package.json文件的一个片段，里面的scripts字段是一个对象。它的每一个属性，对应一段脚本。比如，build命令对应的脚本是node build.js。

命令行下使用npm run命令，就可以执行这段脚本。

```sh
$ npm run build
# 等同于执行
$ node build.js
```

这些定义在package.json里面的脚本，就称为 npm 脚本。它的优点很多。

- 项目的相关脚本，可以集中在一个地方。
- 不同项目的脚本命令，只要功能相同，就可以有同样的对外接口。用户不需要知道怎么测试你的项目，只要运行npm run test即可。
- 可以利用 npm 提供的很多辅助功能。

查看当前项目的所有 npm 脚本命令，可以使用不带任何参数的`npm run`命令。

```bash
$ npm run
```

- **原理：**

  npm 脚本的原理非常简单。<font color=FF0000>每当执行npm run，就会自动新建一个 Shell，在这个 Shell 里面执行指定的脚本命令</font>。因此，只要是 Shell（一般是 Bash）可以运行的命令，就可以写在 npm 脚本里面。

  比较特别的是，npm run新建的这个 Shell，会将当前目录的node_modules/.bin子目录加入PATH变量，执行结束后，再将PATH变量恢复原样。这意味着，当前目录的node_modules/.bin子目录里面的所有脚本，都可以直接用脚本名调用，而不必加上路径。比如，当前项目的依赖里面有 Mocha，只要直接写mocha test就可以了。

	 ```javascript
	"test": "mocha test"
	 ```
	
	而不用写成下面这样。
	
	```sh
	"test": "./node_modules/.bin/mocha test"
	```
	
	<font color=FF0000>由于 npm 脚本的唯一要求就是可以在 Shell 执行，因此它不一定是 Node 脚本，任何可执行文件都可以写在里面。</font>
	
	npm 脚本的退出码，也遵守 Shell 脚本规则。如果退出码不是**`0`**，npm 就认为这个脚本执行失败。
	
- **通配符**

  由于 npm 脚本就是 Shell 脚本，因为可以使用 Shell 通配符。

  ```sh
  "lint": "jshint *.js"
  "lint": "jshint **/*.js"
  ```

- **传参**

  向 npm 脚本传入参数，要使用 `--` 标明。

  ```javascript
  "lint": "jshint **.js"
  ```

  向上面的 `npm run lint` 命令传入参数，必须写成下面这样。

  ```bash
  $ npm run lint --  --reporter checkstyle > checkstyle.xml
  ```

- **执行顺序**

  如果 npm 脚本里面需要执行多个任务，那么需要明确它们的执行顺序。

  如果是<font color=FF0000>**并行执行**（即同时的平行执行），可以使用 **`&`** 符号</font>。

  ```bash
  $ npm run script1.js & npm run script2.js
  ```

  如果是<font color=FF0000>**继发执行**（即只有前一个任务成功，才执行下一个任务），可以使用 **`&&`** 符号</font>。

  ```bash
  $ npm run script1.js && npm run script2.js
  ```

  这两个符号是 Bash 的功能。此外，还可以使用 node 的任务管理模块：[script-runner](https://github.com/paulpflug/script-runner)、[npm-run-all](https://github.com/mysticatea/npm-run-all)、[redrun](https://github.com/coderaiser/redrun)。

- **默认值**

  一般来说，npm 脚本由用户提供。但是，npm 对两个脚本提供了默认值。也就是说，这两个脚本不用定义，就可以直接使用。
  
  ```javascript
  "start": "node server.js"，
"install": "node-gyp rebuild"
  ```
  
  上面代码中，`npm run start`的默认值是`node server.js`，前提是项目根目录下有`server.js`这个脚本；`npm run install`的默认值是`node-gyp rebuild`，前提是项目根目录下有`binding.gyp`文件。
  
- **钩子**

  <font color=FF0000>npm 脚本有 `pre` 和 `post` 两个钩子</font>。举例来说，`build` 脚本命令的钩子就是 `prebuild` 和 `postbuild` 。

  ```js
  "prebuild": "echo I run before the build script",
  "build": "cross-env NODE_ENV=production webpack",
  "postbuild": "echo I run after the build script"
  ```

  <font color=FF0000>用户执行 **`npm run build`** 的时候，会自动按照下面的顺序执行。</font>

  ```bash
  npm run prebuild && npm run build && npm run postbuild
  ```

  **npm 默认提供下面这些钩子**

  - prepublish，postpublish
  - preinstall，postinstall
  - preuninstall，postuninstall
  - preversion，postversion
  - pretest，posttest
  - prestop，poststop
  - prestart，poststart
  - prerestart，postrestart

  <font color=FF0000>自定义的脚本命令也可以加上 `pre` 和 `post` 钩子。比如， `myscript` 这个脚本命令，也有 `premyscript` 和 `postmyscript` 钩子。不过，双重的 `pre` 和 `post` 无效，比如 `prepretest` 和 `postposttest` 是无效的。</font>

  npm 提供一个 `npm_lifecycle_event` 变量，返回当前正在运行的脚本名称，比如 `pretest` 、`test` 、`posttest` 等等

- **简写形式**

  四个常用的 npm 脚本有简写形式。

  - `npm start` 是 `npm run start` 的简写
  - `npm stop` 是 `npm run stop` 的简写
  - `npm test` 是 `npm run test` 的简写
  - `npm restart` 是 `npm run stop && npm run restart && npm run start` 的简写

  `npm start`、`npm stop` 和 `npm restart` 都比较好理解，而`npm restart`是一个复合命令，实际上会执行三个脚本命令：`stop`、`restart`、`start`。具体的执行顺序如下。

  - prerestart
  - prestop
  - stop
  - poststop
  - restart
  - prestart
  - start
  - poststart
  - postrestart

- **变量**

  npm 脚本有一个非常强大的功能，就是可以使用 npm 的内部变量。

  首先，<font color=FF0000>通过 **`npm_package_`** 前缀，npm 脚本可以拿到package.json里面的字段</font>。比如，下面是一个package.json。

  ```json
  {
    "name": "foo", 
    "version": "1.2.5",
    "scripts": {
      "view": "node view.js"
    }
  }
  ```

  那么，<font color=FF0000>变量**`npm_package_name`** 返回 **`foo`**，变量**`npm_package_version`**返回**`1.2.5`**</font>。

  ```js
  // view.js
  console.log(process.env.npm_package_name); // foo
  console.log(process.env.npm_package_version); // 1.2.5
  ```

  上面代码中，我们通过环境变量`process.env` 对象，拿到`package.json` 的字段值。如果是 Bash 脚本，可以用`$npm_package_name`和`$npm_package_version` 取到这两个值。

  `npm_package_` 前缀也支持嵌套的 `package.json` 字段。

  ```json
  "repository": {
    "type": "git",
    "url": "xxx"
  },
  scripts: {
    "view": "echo $npm_package_repository_type"
  }
  ```

  上面代码中，`repository` 字段的 `type` 属性，可以通过 `npm_package_repository_type` 取到。

  另外一个例子：

  ```json
  "scripts": {
    "install": "foo.js"
  }
  ```

  上面代码中，`npm_package_scripts_install` 变量的值等于 `foo.js`。

  npm 脚本还可以通过 `npm_config_` 前缀，拿到 npm 的配置变量，即 `npm config get xxx` 命令返回的值。比如，当前模块的发行标签，可以通过`npm_config_tag`取到。

  ```js
  "view": "echo $npm_config_tag",
  ```

  注意，`package.json` 里面的 `config` 对象，可以被环境变量覆盖。

  ```json
  { 
    "name" : "foo",
    "config" : { "port" : "8080" },
    "scripts" : { "start" : "node server.js" }
  }
  ```

  上面代码中，`npm_package_config_port` 变量返回的是`8080`。这个值可以用下面的方法覆盖。

  ```bash
  $ npm config set foo:port 80
  ```

  最后，`env` 命令可以列出所有环境变量。

  ```js
  "env": "env"
  ```

- **常用脚本示例**

  ```json
  // 删除目录
  "clean": "rimraf dist/*",
  
  // 本地搭建一个 HTTP 服务
  "serve": "http-server -p 9090 dist/",
  
  // 打开浏览器
  "open:dev": "opener http://localhost:9090",
  
  // 实时刷新
   "livereload": "live-reload --port 9091 dist/",
  
  // 构建 HTML 文件
  "build:html": "jade index.jade > dist/index.html",
  
  // 只要 CSS 文件有变动，就重新执行构建
  "watch:css": "watch 'npm run build:css' assets/styles/",
  
  // 只要 HTML 文件有变动，就重新执行构建
  "watch:html": "watch 'npm run build:html' assets/html",
  
  // 部署到 Amazon S3
  "deploy:prod": "s3-cli sync ./dist/ s3://example-com/prod-site/",
  
  // 构建 favicon
  "build:favicon": "node scripts/favicon.js",
  ```



#### npx笔记

摘自：[阮一峰 - npx 使用教程](https://www.ruanyifeng.com/blog/2019/02/npx.html)

npm 从5.2版开始，增加了 npx 命令。

**解决的问题**

<font color=FF0000>npx 想要解决的主要问题，就是调用项目内部安装的模块</font>。比如，项目内部安装了测试工具 Mocha。

一般来说，调用 Mocha ，只能在项目脚本和 package.json 的scripts字段里面， 如果想在命令行下调用，必须像下面这样。

```bash
# 项目的根目录下执行
$ node-modules/.bin/mocha --version
```

<font color=FF0000>npx 就是想解决这个问题，让项目内部安装的模块用起来更方便，只要像下面这样调用就行了。</font>

```bash
$ npx mocha --version
```

<font color=FF0000>npx 的原理很简单，就是运行的时候，会到node_modules/.bin路径和环境变量$PATH里面，检查命令是否存在</font>。

由于 npx 会检查环境变量$PATH，所以系统命令也可以调用。

```bash
# 等同于 ls
$ npx ls
```

注意，Bash 内置的命令不在$PATH里面，所以不能用。比如，cd是 Bash 命令，因此就不能用npx cd。





#### Node相关笔记

以下内容摘自：[一杯茶的时间，上手 Node.js](https://zhuanlan.zhihu.com/p/97413574)

- Node（或者说 Node.js，两者是等价的）是 JavaScript 的一种**运行环境**。同时：**浏览器也是 JavaScript 的运行环境**，两个运行环境的区别：

  <img src="https://i.loli.net/2020/12/21/Ppjc5Vu4TbaFHCS.jpg" style="zoom: 25%;" />

- 运行 Node 代码通常有两种方式：1）在 REPL 中交互式输入和运行；2）将代码写入 JS 文件，并用 Node 执行。

  > **补充**
  > REPL 的全称是 Read Eval Print Loop（读取-执行-输出-循环），通常可以理解为**交互式解释器**，你可以输入任何表达式或语句，然后就会立刻执行并返回结果。<mark>如果你用过 Python 的 REPL 一定会觉得很熟悉。</mark>

  类似于Python，对于js文件也可通过如下命令使其在终端执行：

  ```sh
  node script.js
  ```

- 在浏览器中，我们有 document 和 window 等全局对象；而<font color=FF0000> Node 只包含 ECMAScript 和 V8，不包含 BOM 和 DOM，因此 Node 中不存在 document 和 window；取而代之，Node 专属的全局对象是 process</font>。

  **JavaScript 全局对象的分类**

  1. 浏览器专属，例如 `window`、`alert` 等等；
  2. <font color=FF0000>Node 专属，例如 `process`、`Buffer`、`__dirname`、`__filename` 等等；</font>
  3. <font color=FF0000>浏览器和 Node 共有，但是**实现方式不同**</font>，例如 `console`（第一节中已提到）、`setTimeout`、`setInterval` 等；
  4. <font color=FF0000>浏览器和 Node 共有，并且属于 **ECMAScript 语言定义**的一部分</font>，例如 `Date`、`String`、`Promise` 等；

  <img src="https://i.loli.net/2020/12/21/cWpCzIl3Nao6RAG.jpg" style="zoom: 30%;" />

- ##### **Node 专属全局对象解析**

  - **process：**<font color=FF0000>process 全局对象可以说是 Node.js 的灵魂，它是管理当前 Node.js 进程状态的对象，提供了与操作系统的简单接口。</font>

    在REPL中可以输入`process`查看process中包含哪些熟悉

  - **Buffer：**Buffer 全局对象让 JavaScript 也能够轻松地处理二进制数据流，结合 Node 的流接口（Stream），能够实现高效的二进制文件处理。

  - **\_\_filename 和 \_\_dirname：**分别代表当前所运行 Node 脚本的文件路径和所在目录路径。（\_\_filename 和 \_\_dirname 只能在 Node 脚本文件中使用，在 REPL 中是没有定义的。）

* 了解一下 Node 具体是怎样实现模块机制的。具体而言：Node 引入了三个新的全局对象（Node专属）：**require**、**exports**和**module**。

  - **require：**<font color=FF0000>require 用于导入其他 Node 模块</font>，其参数接受一个字符串代表模块的名称或路径，通常被称为模块标识符。具体有以下三种形式：

    - 直接写模块名称，通常是核心模块或第三方文件模块，例如 `os`、`express` 等
    - 模块的相对路径，指向项目中其他 Node 模块，例如 `./utils`
    - 模块的绝对路径<font color=FF0000>（**不推荐！**）</font>，例如 `/home/xxx/MyProject/utils`

    示例如下：

    ```js
    // 导入内置库或第三方模块
    const os = require('os');
    const express = require('express');
    
    // 通过相对路径导入其他模块
    const utils = require('./utils');
    
    // 通过绝对路径导入其他模块
    const utils = require('/home/xxx/MyProject/utils');
    ```

    > **提示：**在通过路径导入模块时，通常省略文件名中的 `.js` 后缀。

  - **exports：**导出Node模块。另外：exports对象本质上就是`module.exports`的补充，示例如下：

    ```js
    // myModule.js
    function add(a, b) {
      return a + b;
    }
    
    // 导出函数 add
    exports.add = add;  //有空研究一下：这里前一个add是否可以起别名？
    ```

    通过将 add 函数添加到 exports 对象中，外面的模块就可以通过以下代码使用这个函数。在 myModule.js 旁边创建一个 main.js，代码如下：

    ```js
    // main.js
    const myModule = require('./myModule');
    
    // 调用 myModule.js 中的 add 函数
    myModule.add(1, 2);
    ```

    > 提示：如果你熟悉 ECMAScript 6 中的解构赋值，那么可以用更优雅的方式获取 add 函数：
    >
    > ```js
    > const { add } = require('./myModule');
    > ```

  - **module：** 模块对象



## Express.js

**Express.js产生前的痛点：**

- 直接用内置的 http 模块去开发服务器有以下明显的弊端：需要写很多底层代码——例如手动指定 HTTP 状态码和头部字段，最终返回内容。如果我们需要开发更复杂的功能，涉及到多种状态码和头部信息（例如用户鉴权），这样的手动管理模式非常不方便
- 没有专门的路由机制——路由是服务器最重要的功能之一，通过路由才能根据客户端的不同请求 URL 及 HTTP 方法来返回相应内容。但是上面这段代码只能在 http.createServer 的回调函数中通过判断请求 req 的内容才能实现路由功能，搭建大型应用时力不从心

**由此就引出了 Express 对内置 http 的两大封装和改进：**

- 更强大的<font color=FF0000>请求（Request）</font>和<font color=FF0000>响应（Response）</font>对象，添加了很多实用方法
- 灵活方便的路由的定义与解析，能够很方便地进行代码拆分



#### **Request和Require对象**

-  **Request 请求对象：**通常我们习惯用 req 变量来表示。下面列举一些 req 上比较重要的成员

  - **req.body：**客户端<font color=FF0000>请求体的数据</font>，可能是表单或 JSON 数据
  - **req.params：**<font color=FF0000>请求 URI 中的路径参数</font>
  - **req.query：**<font color=FF0000>请求 URI 中的查询参数</font>
  - **req.cookies：**客户端的<font color=FF0000> cookies</font>

- **Response 响应对象：**通常用 res 变量来表示，可以<font color=FF0000>执行一系列响应操作</font>，如下示例：

  ```js
  // 发送一串 HTML 代码
  res.send('HTML String');
  
  // 发送一个文件
  res.sendFile('file.zip');
  
  // 渲染一个模板引擎并发送
  res.render('index');
  ```

  Response 对象上的操作非常丰富，并且还可以链式调用：

  ```js
  // 设置状态码为 404，并返回 Page Not Found 字符串
  res.status(404).send('Page Not Found');
  ```



#### 路由机制

<mark>客户端（包括 Web 前端、移动端等等）向服务器发起请求时包括两个元素：**路径**（URI）以及 **HTTP 请求方法**（包括 GET、POST 等等）</mark>。<font color=FF0000>路径和请求方法合起来一般被称为 API 端点（Endpoint）</font>。而<font color=FF0000>服务器根据客户端访问的端点选择相应处理逻辑的机制就叫做路由</font>。

在 Express 中，定义路由只需按下面这样的形式：

```js
app.METHOD(PATH, HANDLER)
```

其中：

- **app** 就是一个 express 服务器对象
- **METHOD** 可以是<font color=FF0000>任何小写的 HTTP 请求方法，包括 get、post、put、delete 等等</font>
- **PATH** 是客户端访问的 URI，例如 / 或 /about
- **HANDLER** 是<font color=FF0000>路由被触发时的回调函数，在函数中可以执行相应的业务逻辑</font>



#### 中间件

中间件并不是 Express 独有的概念。相反，它是一种广为使用的软件工程概念；是指将具体的业务逻辑和底层逻辑解耦的组件。换句话说，中间件就是能够适用多个应用场景、可复用性良好的代码。

Express 的<font color=FF0000>**简化版**中间件流程</font>如下图所示：

<img src="https://i.loli.net/2020/12/22/RSK6rnJpkjZ93FN.jpg" style="zoom:27%;" />

首先客户端向服务器发起请求，然后服务器依次执行每个中间件，最后到达路由，选择相应的逻辑来执行。

**另外需要注意的是：**

- <font color=FF0000>中间件是**按顺序执行**的</font>，<font color=FF0000>因此在配置中间件时顺序非常重要，不能弄错</font>
- <font color=FF0000>中间件在执行内部逻辑的时候可以选择将请求传递给下一个中间件，**也可以直接返回用户响应**</font>

**在 Express 中，中间件就是一个函数：**

```js
function someMiddleware(req, res, next) {
  // 自定义逻辑
  next();
}
```

三个参数中，req 和 res 就是前面提到的 Request 请求对象和 Response 响应对象；而 next 函数则用来触发下一个中间件的执行。

**在 Express 使用中间件有两种方式：<font color=FF0000>全局中间件</font>和<font color=FF0000>路由中间件</font>**

- **全局中间件：**<font color=FF0000>通过 app.use 函数就可以注册中间件</font>，并且此中间件会在用户发起任何请求都可能会执行。如下示例：

  ```js
  app.use(middlewareName)
  ```

- **路由中间件：**通过<font color=FF0000>在路由定义时注册中间件，此中间件只会在用户访问该路由对应的 URI 时执行</font>。如下示例：

  ```js
  app.get('/middleware', MiddlewareName, (req, res) => {
    res.send('Hello World');
  });
  ```

  