# webpack 学习笔记



#### webpack 存在的必要

webpack是一种构建工具工具。那，为什么需要构建或者说编译呢？因为像es6、less及sass、模板语法、vue指令及jsx在浏览器中是无法直接执行的，必须经过构建这一个操作才能保证项目运行，所以前端构建打包很重要。除了这些，前端构建还能解决一些web应用性能问题，比如：依赖打包、资源嵌入、文件压缩及hash指纹等。具体的我不再展开，总之前端构建工程化已经是趋势。



##### webpack 解析 ES6

需要掌握一个新的概念，loaders：所谓 loaders ，就是说把原本 webpack 不支持加载的文件或者文件内容通过 loaders 进行加载解析，实现应用的目的。<mark>这里讲解 ES6 解析，原生支持 JS 解析，但是不能解析 ES6，需要 babel-loader ，而 babel-loader 又依赖 babel</mark>

##### webpack 加载 css、less 等样式文件

css-loader 用于加载 css 文件并生成 commonjs 对象，style-loader 用于将样式通过 style 标签插入到 \<head> 中

##### webpack 加载图片

图片在 webpack 中的打包使用 file-loader 或 url-loader

摘自：[一小时的时间，上手 Webpack](https://zhuanlan.zhihu.com/p/114286243)



#### 开发环境与生产环境的区别

##### 开发环境

- NODE_ENV 为 development
- 启用模块热更新 ( hot module replacement )
- 额外的 webpack-dev-server 配置项，API Proxy 配置项
- 输出 SourceMap

##### 生产环境

- NODE_ENV 为 production
- 将 React、jQuery 等常用库设置为 external，直接采用 CDN 线上的版本
- 样式源文件（如 css、less、scss 等）需要通过 ExtractTextPlugin 独立抽取成 css 文件
- 启用 post-css
- 启用 optimize-minimize（如 uglify 等）
- 中大型的商业网站生产环境下，是绝对不能有 console.log() 的，所以要为 babel 配置 Remove console transform

> 这里需要说明的是因为开发环境下启用了 hot module replacement，为了让样式源文件的修改也同样能被热替换，不能使用 ExtractTextPlugin，而转为随 JS Bundle 一起输出。

摘自：[为什么我们要做三份 Webpack 配置文件](https://zhuanlan.zhihu.com/p/29161762)



### webpack 基础概念

#### webpack 文档 concept 的介绍

##### 总述

At its core, **webpack** is a *static module bundler* for modern JavaScript applications. When webpack processes your application, <font color=FF0000>**it internally <font size=4>builds a [dependency graph](https://webpack.js.org/concepts/dependency-graph/)</font> from one or more *entry points***</font> and **then** <font color=FF0000>**combines every module your project needs into one or more *bundles***</font>, which are static assets to serve your content from.

<mark>Since version 4.0.0, **webpack does not require a configuration file** to bundle your project</mark>. <font size=4>**Nevertheless**</font>, <font color=FF0000>**it is [incredibly configurable](https://webpack.js.org/configuration) to better fit your needs**</font>.

##### Entry

An **entry point** indicates <font color=FF0000>**which module webpack should use to begin building out its internal [dependency graph](https://webpack.js.org/concepts/dependency-graph/)**</font>. Webpack will <font color=FF0000>figure out which other modules and libraries that **entry point depends on** (directly and indirectly)</font>.

<font color=FF0000>**By default its value is `./src/index.js`**</font> , but you can <font color=FF0000>specify a different (or multiple entry points) by setting an `entry` property</font> in the webpack configuration. For example:

```js
// webpack.config.js

module.exports = {
  entry: './path/to/my/entry/file.js',
};
```

##### Output

The **output** property **tells webpack where to emit the *bundles* it creates** and **how to name these files**. <font color=FF0000>**It defaults to `./dist/main.js` for the main output file**</font> and <font color=FF0000>**to the `./dist` folder for any other generated file**</font>.

You can configure this part of the process by specifying an `output` field in your configuration:

```js
// webpack.config.js

const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js',
  },
};
```

In the example above, we use the <font color=FF0000>`output.filename`</font> and the <font color=fuchsia>`output.path` </font> properties to tell webpack <font color=FF0000>**the name of our bundle**</font> and <font color=fuchsia>**where we want it to be emitted to**</font>. 

##### Loaders

<mark style="background: lightpink">**Out of the box**</mark> , <font color=FF0000 size=4>webpack **only understands JavaScript and JSON** files</font>（译文：webpack 只能理解 JavaScript 和 JSON 文件，<mark style="background: lightpink">**这是 webpack 开箱可用的自带能力**</mark>）. <font color=FF0000 size=4>**Loaders** allow webpack to **process other types of files** and **convert them into valid [modules](https://webpack.js.org/concepts/modules)** that **can be consumed by your application** and **added to the dependency graph**</font>.

At a high level, **loaders** have two properties in your webpack configuration:

1. The **`test` property** <font color=FF0000>identifies which file or files should be transformed</font>.
2. The **`use` property** <font color=FF0000>indicates which loader should be used to do the transforming</font>.

```js
// webpack.config.js
const path = require('path');

module.exports = {
  output: {
    filename: 'my-first-webpack.bundle.js',
  },
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
};
```

The configuration above has defined a `rules` property for a single module with two required properties: `test` and `use`. This tells webpack's compiler the following:

> *"Hey webpack compiler, when you come across a path that resolves to a '.txt' file inside of a* `require()`*/*`import` *statement,* **use** *the* `raw-loader` *to transform it before you add it to the bundle."*

###### 文档 API 部分的补充

Loaders are transformations that are applied to the source code of a module. <font color=fuchsia>**They are written as functions** that **accept source code as a parameter** and **return a new version of that code with transformations applied**</font>.

摘自：[webpack doc - API - Introduction - Loaders](https://webpack.js.org/api/#loaders)

##### Plugins

While loaders are used to transform certain types of modules, <font color=FF0000>plugins can **be leveraged to perform a wider range of tasks** like bundle optimization, asset management and injection of environment variables</font>.

<font color=FF0000>In order to use a plugin, **you need to `require()` it and add it to the `plugins` array**</font>. Most plugins are customizable through options. Since you can use a plugin multiple times in a configuration for different purposes, <font color=FF0000>**you need to create an instance of it by calling it with the `new` operator**</font>.

```js
// webpack.config.js

const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack'); //to access built-in plugins

module.exports = {
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
  plugins: [new HtmlWebpackPlugin({ template: './src/index.html' })],
};
```

In the example above, <font color=FF0000>the `html-webpack-plugin` **generates an HTML file for your application** and **automatically injects all your generated bundles into this file**</font>.

> **Tip 💡**: There are many plugins that webpack provides out of the box! Check out the [list of plugins](https://webpack.js.org/plugins).

###### 文档 API 部分的补充

<font color=red>The plugin <font size=4>**interface**</font> **allows users to tap directly into the compilation process**</font>. <font color=fuchsia>Plugins can <font size=4>**register handlers on lifecycle hooks**</font> that <font size=4>**run at different points throughout a compilation**</font></font>. <font color=red>**When <font size=4>each hook</font> is executed, the plugin will <font size=4>have full access to the current state of the compilation</font>**</font>.

> 👀 注：上面这段话很重要，虽然不难懂；但为了强调，这里摘抄下中文文档中的 [翻译](https://www.webpackjs.com/api/#plugin)：
>
> 插件接口可以帮助用户直接触及到编译过程 ( compilation process )。 插件可以将处理函数 ( handler ) 注册到编译过程中的不同事件点上运行的生命周期钩子函数上。 当执行每个钩子时， 插件能够完全访问到编译 ( compilation ) 的当前状态。

摘自：[webpack doc - API - Introduction - Plugins](https://webpack.js.org/api/#plugins)

##### Mode

By setting the <font color=FF0000>`mode` parameter to either `development`, `production` or `none`</font>, you can <font color=FF0000 size=4>enable **webpack's built-in optimizations**</font> that correspond to each environment. <font color=FF0000>**The default value is `production`**</font>.

```javascript
module.exports = {
  mode: 'production',
};
```

##### Browser Compatibility

Webpack supports all browsers that are [ES5-compliant](https://kangax.github.io/compat-table/es5/) (<font color=FF0000>**IE8 and below are not supported**</font>). Webpack needs `Promise` for `import()` and `require.ensure()` . <mark>If you want to support older browsers, you will need to **load a polyfill before using these expressions**</mark> .

摘自：[webpack 文档 - Concepts](https://webpack.js.org/concepts)

#### webpack 文档 Guide 的笔记

##### webpack 与插件

You might be wondering <font color=FF0000>**how webpack and its plugins seem to "know" what files are being generated**</font>（在这之前讲了）. **The answer is** <font color=FF0000>in the manifest that webpack keeps to track how all the **modules map** to the output bundles</font>. If you're interested in managing webpack's [`output`](https://webpack.js.org/configuration/output) in other ways, the manifest would be a good place to start.

The manifest data can be extracted into a json file for consumption using the [`WebpackManifestPlugin`](https://github.com/shellscape/webpack-manifest-plugin).

摘自：[webpack 文档 - Guide - Output Management - The Manifest](https://webpack.js.org/guides/output-management/#the-manifest)



#### 关于 webpack.config.js

webpack 默认使用 webpack.config.js 作为配置文件。可以使用 `--config` 选项，以 `--config youWantConfigFile` 的形式选择想要的配置文件。如上内容，总结自下面的话：

> <font color=FF0000>If a `webpack.config.js` is present, the `webpack` command picks it up **by default**</font>. We <font color=FF0000>use the **`--config` option** here only to show that you can pass a configuration of any name</font>. This will be useful for more complex configurations that need to be split into multiple files.
>
> 摘自：[webpack 文档 - Guides - Getting Started - Using a Configuration](https://webpack.js.org/guides/getting-started/#using-a-configuration)



#### 依赖管理 Dependency Management

<font color=fuchsia>**A context is created if your request contains expressions**</font>（👀 **注**：request 含义见下面引用）, so the **exact** module is not known on compile time（**译**：因为在编译时(compile time)并不清楚 **具体** 导入哪个模块。👀 **注**：所以在运行时确定）.

> **Request**: 指在 require/import 语句中的表达式，如在 *require("./template/" + name + ".ejs")* 中的请求是 *"./template/" + name + ".ejs"* 。
>
> 摘自：[webpack - 术语表](https://webpack.docschina.org/glossary/#r)

<mark style="background: lightskyblue">Example</mark>, given we have the following folder structure including `.ejs` files:

```bash
example_directory
│
└───template
│   │   table.ejs
│   │   table-row.ejs
│   │
│   └───directory
│       │   another.ejs
```

When following `require()` call is evaluated :

```javascript
require('./template/' + name + '.ejs');
```

<font color=fuchsia>Webpack parses the `require()` call and **extracts some information**</font>:

```js
Directory: ./template
Regular expression: /^.*\.ejs$/ // 这里是用正则表示，以供后面匹配。
```

<font color=FF0000>**A context module is generated**</font>. It contains references to **all modules in that directory** that can be required with a request matching the <font color=FF0000>regular expression</font>（**译**：它包含 **目录下的所有模块** 的引用，如果一个 request 符合正则表达式，就能 require 进来）. <font color=FF0000>The context module contains a map which translates requests to module ids</font>.

示例 map：

```json
{
  "./table.ejs": 42,
  "./table-row.ejs": 43,
  "./directory/another.ejs": 44
}
```

The <font color=FF0000>context module also contains some runtime logic to access the map</font>.

This means dynamic requires are supported but will cause all matching modules to be included in the bundle.

##### require.context()

You <font color=FF0000>can **create your own context with the `require.context()` function**</font>.

It allows you to pass in <mark>a directory to search</mark> , <mark style="background: aqua">a flag indicating whether subdirectories should be searched too</mark>, and <mark style="background: lightpink">a regular expression to match files against</mark>.

Webpack parses for `require.context()` in the code while building（👀 **注**：即 compile time）.

<mark style="background: lightskyblue">**The syntax is as follows:**</mark>

```js
require.context(
  directory,                  // 目标文件夹
  (useSubdirectories = true), // 是否深度查找，默认是 true
  (regExp = /^\.\/.*$/),      // 查找模式（正则）
  (mode = 'sync')             // 同步异步，默认是同步
);
```

<mark style="background: lightskyblue">**Examples:**</mark>

```javascript
require.context('./test', false, /\.test\.js$/);
// a context with files from the "test" directory that can be required with a request ending with `.test.js`.
```

```js
require.context('../', true, /\.stories\.js$/);
// a context with all files in the parent folder and descending folders ending with `.stories.js`.
```

##### context module API

A **context module** <font color=FF0000>**exports a ( require ) function** that **takes one argument : the request**</font>.

<font color=dodgerBlue>**The exported function has 3 properties : `resolve` , `keys` , `id`**</font> .（👀 **注**：这里有点费解，不过想到 函数（ JS 中函数是一个对象）有 name、length 等属性，也就理解了...）

- `resolve` is a **function** and <font color=FF0000>returns the **module id** of the parsed request</font>（**译**：返回 request 被解析后得到的模块 id ）.

- `keys` is a **function** that <font color=FF0000>returns **an array of all possible requests** that the context module can handle</font>（**译**：所有可能被此 context module 处理的请求的数组）.

  This can <font color=FF0000>be useful if you want to require all files in a directory or matching a pattern</font> , Example :

  ```js
  function importAll(r) {
    r.keys().forEach(r);
  }
  
  importAll(require.context('../components/', true, /\.js$/));
  ```

  ```javascript
  const cache = {};
  
  function importAll(r) {
    r.keys().forEach((key) => (cache[key] = r(key)));
  }
  
  importAll(require.context('../components/', true, /\.js$/));
  // At build-time cache will be populated with all required modules.
  ```

- `id` is the **module id of the context module**. This <font color=FF0000>may be useful for `module.hot.accept`</font> .

摘自：[webpack 文档 - Guides - Dependency Management](https://webpack.js.org/guides/dependency-management/)

##### require.context() 在实际项目中的使用

###### 用来在组件内引入多个组件

```js
// 从 @/components/home 目录下加载所有 .vue 后缀的组件
const files = require.context('@/components/home', false, /\.vue$/);
const components = {};
 
// 遍历 files 对象，构建 components 键值
files.keys().forEach(key => {
    components[key.replace(/(\.\/|\.vue)/g, '')] = files(key).default
});

export default {
    // ...
    components,
}
```

###### 在 main.js 中引入大量公共组件

```js
import Vue from 'vue'

const requireComponents = require.context('../views/components', true, /\.vue/)
// 遍历出每个组件的路径
requireComponents.keys().forEach(fileName => {
  // 组件实例
  const reqCom = requireComponents(fileName)
  // 截取路径作为组件名
  const reqComName =reqCom.name|| fileName.replace(/\.\/(.*)\.vue/,'$1')
  // 组件挂载
  Vue.component(reqComName, reqCom.default || reqCom)
})
```

##### 用在 vuex 中加载 module 或加载多个 api 接口

```js
/**
 * The file enables `@/store/index.js` to import all vuex modules
 * in a one-shot manner. There should not be any reason to edit this file.
 */
const files = require.context('.', false, /\.js$/)
const modules = {}
 
files.keys().forEach(key => {
  if (key === './index.js') return
  modules[key.replace(/(\.\/|\.js)/g, '')] = files(key).default
})
 
export default modules
```

摘自：[require.context()的用法详解](https://blog.csdn.net/pinbolei/article/details/115620728)



### webpack 文档 深层概念

#### \__webpack_require__

知道 `__webpack_require__` 是在 webpack 文档的 [Concept - The Manifest - Manifest](https://webpack.js.org/concepts/manifest/#manifest) 部分

> **No matter which module syntax you have chosen**, those <font color=FF0000>import or require statements have now become `__webpack_require__` methods</font> that <font color=FF0000>point to module identifiers</font>

感觉有点重要



### 慕课网课程《从基础到实战 手把手带你掌握新版 Webpack4.0 》笔记

**`npx webpack entryFile`** 命令的作用是：使用 webpack 工具去解析（似乎只是替换文件引入的语法 ( import / require ) ，将模块合并打包，webpack 本身不懂 ES6 -> ES5 的转换）一个项目，这时候项目的文件夹下会产生一个 dist 目录，在该目录下会产生一个 main.js 的文件（ main.js 是 webpack 默认配置定义的打包名称，关于 webpack 配置见下面）。



webpack 本质上是一个模块打包工具，同时支持 ES Module / CommonJS / CMD / ADM 的导入方式

**官方文档如下：**

> In contrast to Node.js modules, webpack modules can express their dependencies in a variety of ways. A few examples are:
>
> - An **ES2015** import statement
> - A **CommonJS** require() statement
> - An **AMD** define and require statement
> - An **@import** statement inside of a css/sass/less file.
> - An image url in a stylesheet **url(...)** or HTML **\<img src=...>** file.
>
> 
>
> Webpack supports the following module types **natively**:
>
> - ECMAScript modules
> - CommonJS modules
> - AMD modules
> - Assets
> - WebAssembly modules
>
> 摘自：https://webpack.js.org/concepts/modules/#supported-module-types



<font color=FF0000>**CommonJS 是 Node 的模块引入规范**</font>，其中引入和导出的语法如下

```js
//引入
var module = require('modulePath')

//导出
module.exports = module
```



使用 webpack 的同时，必须要安装 webpack-cli 。webpack-cli 的作用是让你可以在命令行中使用 webpack 命令，如果你不安装，将无法使用 `webpack` 或 `npx webpack` 命令。



webpack 是基于 Node 开发的模块打包工具，所以本质上是由 Node 实现的。同时，由于更高版本的 webpack，会使用高版本 Node 的一些特性；可以大大提高 webpack 打包的速率。

通常不建议使用全局安装包 ( `npm install -g package` ) ，比如 webpack ；因为这样对于依赖不同版本包的不同项目，会造成依赖冲突，所以推荐使用在项目中通过 `-D` 形式安装。

以webpack为例，如果没有全局安装 webpack，这时使用 **webpack -v** 命令是没有结果的，这时可以使用 **`npx webpack -v`**，<font color=FF0000> 将会运行运行在当前目录下的 webpack 安装包</font>。同样，不仅仅可以使用 `-v` 命令，还有其他命令。

<font size=4>**补充：**</font> 使用全局安装的 webpack，打包命令如下：**`webpack entryFileName`**。所以，后面才会出现项目本地安装时打包的命令： npx webpack entryFileName



`npm init` 命令，帮助我们以 Node 规范的形式创建一个项目，或者创建一个 Node 的包文件。此时会生成一个 package.json 的文件。

可以在命令的后面添加 `-y` ，让所有选项均为默认。

当前的 package.json 文件内容如下：

```json
{
  "name": "yan",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

这时，可以在 package.json 中加上 `"private": true` 表示代码是私有的，不会被发布到 npm 的线上仓库中。

另外，main 的作用是：指定 便于外部项目使用的、向外暴露的文件



webpack 的（默认）配置文件的名称为 webpack.config.js。即：即使用户不自定义配置文件，webpack 也会使用默认的配置文件



#### webpack 安装 文档补充

##### Prerequisites 前提条件

Before we begin, make sure you have a fresh version （译：新版本）of [Node.js](https://nodejs.org/en/) installed. The current Long Term Support (LTS) release is an ideal starting point. You <font color=FF0000>may run into a variety of issues with the older versions as **they may be missing functionality webpack and/or its related packages require**</font>.

##### Local Installation 项目层级安装

If you're <font color=FF0000>using webpack v4 or later</font> and <font color=FF0000>want to call `webpack` from the command line</font>, you'll also need to install the [CLI](https://webpack.js.org/api/cli/).

<font color=fuchsia>Installing locally is what we recommend for most projects</font>. This makes it easier to upgrade projects individually **when breaking changes are introduced**.

> 💡 **Tip**  : <font color=fuchsia>To run the local installation of webpack you can **access its binary version as `node_modules/.bin/webpack`**</font> . Alternatively, <font color=FF0000>if you are using npm v5.2.0 or greater, you can **run `npx webpack` to do it**</font>.

##### Global Installation 全局安装

> ⚠️ **Warning** : Note that <font color=FF0000>this is **not a recommended practice**</font>. Installing globally <font color=FF0000>locks you down to a specific version of webpack</font> and could fail in projects that use a different version.

##### Bleeding Edge 最新体验版本

If you are enthusiastic about using the latest that webpack has to offer, <font color=FF0000>you can install beta versions or even **directly from the webpack repository** using the following commands</font>:

```bash
npm install --save-dev webpack@next
# or a specific tag/branch
npm install --save-dev webpack/webpack#<tagname/branchname>
```

👀 **注：**只见过 `libName@libVersion` ，没见过 `libName#<tagName>` 以及 `libname#<branchName>` ，值得注意。

摘自：[webpack doc - Guide - Installation](https://webpack.js.org/guides/installation)



#### webpack.config.js 配置文件的格式

webpack.config.js 使用 CommonJS 的语法，语法格式如下：

```js
// 这是引入node的一个核心模块path，与下面的path一起使用。
// 如果这个没有引入，将没有下面的 path.resolve() 方法
const path = require('path')

module.exports = {
  // 项目打包，开始打包的文件入口，实际上相当于entry: {main: './src/index.js'}, 的简写
  // 如果不配置该项，命令 `npx webpack [entryFileName]` 是必须要加上文件名（入口文件名）的；
  // 配置了这项，则不需要了，可以直接使用 `npx webpack`了
  entry: './src/index.js',
  output: {
    // 项目打包输出文件的名称，默认为main.js,
    filename: 'bundle.js',
    // path是定义：项目打包输出文件夹路径，即打包输出的文件在path/filename下。
    // __dirname的意思是webpack.config.js所在了文件夹路径。
    // 这行代码的意思是输出文件为 bundle/bundle.js，虽然一般默认为dist
    // **另外**，这里path对应的值必须是绝对路径，所以需要使用 path.resolve() 做拼接，来获取绝对路径
    path: path.resolve(__dirname, 'bundle'),
  }
  // mode 指定打包的模式。这里指定的值是production，此时打包出的文件将会被压缩（变成min格式）
  // 除了production外，还可以选择development选项，此时代码不会被压缩
  // 另外，如果不指定 mode，mode的默认值是production。但是不指定，webpack会警告。
  mode: 'production',
}
```

**注：**dist 是 distribution 的缩写

#### Entry 文档补充

##### 总述

Entry 有两种写法 ：

- **“Single Entry (Shorthand) Syntax”  译为 “单个入口（简写）语法”**：`entry: string | [string]`
- **“Object Syntax” 译为 “对象语法”** ： `entry: { <entryChunkName> string | [string] } | {}`

 前者是后者的简写。另外， `[string]` 是 *字符串数组* 。

##### 单个入口（简写）语法

**用法：**`entry: string | [string]` 。

`entry: './path/to/my/entry/file.js'` 是 “单个入口（简写）语法”，是一种简写；它的完整的写法（“对象语法”）是：

```javascript
entry: {
  main: './path/to/my/entry/file.js',
},
```

另外，这里 main 是 entryChunkName，下面有说明

**除了 指定 字符串 作为入口路径外，还可以使用 字符串数组：** 

> We can also <font color=FF0000>pass an array of file paths to the `entry` property which creates what is known as a **"multi-main entry"**</font>. This is <font color=FF0000>useful when you would like to **inject multiple dependent files together** and **graph their dependencies into one "chunk"**</font>.
>
> ```js
> // webpack.config.js
> 
> module.exports = {
>     entry: ['./src/file_1.js', './src/file_2.js'],
>     output: {
>       filename: 'bundle.js',
>     },
> };
> ```
>
> Single Entry Syntax is a great choice when you are looking to <font color=FF0000>quickly set up a webpack configuration for an application or tool<font color=FF0000> with one entry point (i.e. a library). However, <font color=FF0000>there is **not much flexibility** in **extending or scaling your configuration with this syntax**</font>.

##### 对象语法

> ```js
> // webpack.config.js
> 
> module.exports = {
>     entry: {
>        app: './src/app.js',
>        adminApp: './src/adminApp.js',
>     },
> };
> ```
>
> The object syntax is more verbose（啰嗦）. However, this is <font color=FF0000>the **most scalable way**</font> （可伸缩 / 可扩展性）<font color=FF0000>of defining entry / entries in your application</font>.
>
> > **"Scalable webpack configurations"** are ones that can be reused and combined with other partial configurations. This is a popular technique used to separate concerns（分离关注点） by environment, build target, and runtime. They are then merged using specialized tools like [webpack-merge](https://github.com/survivejs/webpack-merge).
> >
> > 译文：**“webpack 配置的可扩展”** 是指，这些配置可以重复使用，并且可以与其他配置组合使用。这是一种流行的技术，用于将关注点从环境 (environment)、构建目标(build target)、运行时(runtime)中分离。然后使用专门的工具（如 [webpack-merge](https://github.com/survivejs/webpack-merge)）将它们合并起来。
>
> ##### EntryDescription object （入口描述对象）
>
> An object of entry point description. You can <font color=FF0000>specify the following properties</font>:
>
> - **dependOn**: The entry points that <font color=FF0000>**the current entry point depends on**</font>. They <font color=FF0000>**must be loaded before this entry point is loaded**</font>.   
>
>   当前入口所依赖的入口。它们必须在该入口被加载前被加载。
>
> - **filename**: Specifies the name of each output file on disk.  
>
>   <font color=FF0000>指定要 <font size=4>**输出**</font> 的文件名称</font>。
>
> - **import:** Module(s) that are loaded upon startup（启动）.
>
>   <font color=FF0000>启动时需加载的模块</font>。
>
> - **library**: Specify [library options](https://webpack.js.org/configuration/output/#outputlibrary) to bundle a library from current entry. 
>
>   <font color=FF0000>**指定 library 选项，为当前 entry 构建一个 library**</font>。更多相关可见 [[#创建 library ( Authoring Libraries )#Expose the Library]]
>
> - **runtime**: <font color=FF0000>**The name of the runtime chunk**</font>. <font color=FF0000>When set, a new runtime chunk will be created</font>. It can be set to `false` to avoid a new runtime chunk since webpack 5.43.0.  
>
>   （指定）<font color=FF0000>**运行时 chunk 的名字**</font>（这也解释了下面为什么说：runtime 不能指向 “已存在的入口名称”）。<font color=FF0000>如果设置了，就会创建一个新的运行时 chunk</font>。在 webpack 5.43.0 之后可将其设为 `false` 以避免一个新的运行时 chunk。
>
> - **publicPath**: Specify a public URL address for the output files of this entry when they are referenced in a browser. Also, see [output.publicPath](https://webpack.js.org/configuration/output/#outputpublicpath).  
>
>   当该入口的输出文件在浏览器中被引用时，为它们指定一个公共 URL 地址。同样，可见 output.publicPath
>
> ##### 示例
>
> ```js
> module.exports = {
>   entry: {
>     a2: 'dependingfile.js',
>     b2: {
>       dependOn: 'a2',
>       import: './src/app.js',
>     },
>   },
> };
> ```
>
> ##### 注意点
>
> - <font color=FF0000>**`runtime` and `dependOn`** should not be used together on **a single entry**</font>, so the <mark>following config is invalid and would **throw an error**</mark>:
>
>   ```js
>   module.exports = {
>     entry: {
>       a2: './a',
>       b2: {
>         runtime: 'x2',
>         dependOn: 'a2',
>         import: './b',
>       },
>     },
>   };
>   ```
>
> - <font color=FF0000>**`runtime`** must not point to an **existing entry point name**</font>, for example the <mark>below config would **throw an error**</mark>:
>
>   ```js
>   module.exports = {
>     entry: {
>       a1: './a',
>       b1: {
>         runtime: 'a1', // a1 在上面已定义
>         import: './b',
>       },
>     },
>   };
>   ```
>
> - <font color=FF0000>**`dependOn`** must not be circular</font>（dependOn 不能循环引用）, the following example again would throw an error:
>
>   ```js
>   module.exports = {
>     entry: {
>       a3: {
>         import: './a',
>         dependOn: 'b3',
>       },
>       b3: {
>         import: './b',
>         dependOn: 'a3',
>       },
>     },
>   };
>   ```

##### 常见场景：分离 app（ 应用程序 ） 和 vendor （第三方库） 入口

这也是 “多入口打包”。如下示例配置：

```js
// wepback.config.js
module.exports = {
  entry: {
    main: './src/app.js',
    vendor: './src/vendor.js',
  },
};
```

```js
// webpack.prod.js
module.exports = {
  output: {
    filename: '[name].[contenthash].bundle.js',
  },
};
```

```js
// webpack.dev.js
module.exports = {
  output: {
    filename: '[name].bundle.js',
  },
};
```

这样做的原因是： <font color=FF0000>**可以在 `vendor.js` 中存入 未做修改的 必要 “库”( library ) 或文件**</font>（例如 Bootstrap， jQuery， 图片等），然后<font color=FF0000>**将它们打包在一起成为单独的 chunk**</font>。内容哈希保持不变，这使浏览器可以独立地缓存它们，从而减少了加载时间。

> ##### 注意
>
> 在 webpack < 4 的版本中，<mark>通常将 **vendor 作为一个单独的入口起点** 添加到 entry 选项中</mark>，以将其编译为一个单独的文件（与 `CommonsChunkPlugin` 结合使用）。
>
> 而<font color=FF0000>在 webpack 4 中不鼓励这样做</font>。而是<font color=FF0000>使用 `optimization.splitChunks` 选项，将 vendor 和 app （应用程序） 模块分开，并为其创建一个单独的文件</font>。不要 为 vendor 或其他不是执行起点创建 entry。

##### 常见场景：多页面应用程序 (MPA)

```js
// webpack.config.js
module.exports = {
  entry: {
    pageOne: './src/pageOne/index.js',
    pageTwo: './src/pageTwo/index.js',
    pageThree: './src/pageThree/index.js',
  },
};
```

这是在告诉 webpack，我们需要三个分离的依赖图 ( dependency graphs )。

这样做的原因是： 在多页面应用程序中，<font color=FF0000>server 会拉取一个新的 HTML 文档给你的客户端。页面重新加载此新文档，并且资源被重新下载</font>。然而，<font color=FF0000>**这给了我们特殊的机会去做很多事**</font>，例如使用 `optimization.splitChunks` 为页面间共享的应用程序代码创建 bundle。由于入口起点数量的增多，多页应用能够复用多个入口起点之间的大量代码/模块，从而可以极大地从这些技术中受益。

总结与摘抄自：[webpack 文档 - concept - Entry Points](https://webpack.js.org/concepts/entry-points/)

#### Output 文档补充

output 配置选项 是告诉 webpack： 如何向硬盘写入编译文件。注意 ⚠️：<font color=FF0000>即使可以存在多个 `entry` 起点，但 <font size=4>**只能指定一个 `output` 配置**</font></font>（**注意** ⚠️：是只能指定一个配置，而不是文件）。另外，下面的 [[#targets#Multiple Targets]] 中 想要根据不同的 target mode（比如 'node' 和 'web' ）打两个包，有两个不同的 output，可以写两个同形的配置对象，详见下面

<font color=FF0000>使用 output 配置选项 **至少** 要传入 一个 包含 `output.filename` 的对象</font>：

```js
// webpack.config.js
module.exports = {
  output: {
    filename: 'bundle.js',
  },
};
```

此配置将一个单独的 `bundle.js` 文件输出到 `dist` 目录中。

##### 多打包入口

如果配置中创建出多于一个 "chunk"（例如，使用 **多个入口起点** 或 使用像 CommonsChunkPlugin 这样的插件），则应该使用 [占位符(substitutions)](https://webpack.docschina.org/configuration/output#output-filename) 来确保每个文件具有唯一的名称：

```js
module.exports = {
  entry: {
    app: './src/app.js',
    search: './src/search.js',
  },
  output: {
    filename: '[name].js', // [name] 即是一种“占位符”
    path: __dirname + '/dist',
  },
};

// 写入到硬盘：./dist/app.js, ./dist/search.js
```

##### output 中的 publicPath

以下是对资源使用 CDN 和 hash 的复杂示例：

```js
module.exports = {
  output: {
    path: '/home/proj/cdn/assets/[fullhash]',
    publicPath: 'https://cdn.example.com/assets/[fullhash]/',
  },
};
```

如果在编译时，不知道最终输出文件的 `publicPath` 是什么地址，则可以将其留空，并且在运行时通过入口起点文件中的 `__webpack_public_path__` 动态设置。

```js
// 应用程序入口的其余部分
__webpack_public_path__ = myRuntimePublicPath;
```

摘自：[webpack 文档 - output](https://webpack.js.org/concepts/output/) 注：下面还有 文档 Guides 部分的 Public Path，见 [[#文档 Guides 中的 public path]]

##### output 中的 initial chunk 和 non-initial chunk

在 [webpack 文档 - concept - Under The Hood - Output](https://webpack.js.org/concepts/under-the-hood/#Output) 中还有 `inital` chunk files （与 output.filename 相关）以及 `non-initial` chunk files （与 output.chunkFilename 相关 ）的 内容，值得注意



#### 文档 Guides 的 public path

// TODO

DefinePlugin：https://webpack.js.org/plugins/define-plugin/ ，还有 https://segmentfault.com/a/1190000017217915 也看下



#### Targets

##### 总述

Because <font color=FF0000>JavaScript can be written for both server and browser</font>, **webpack offers multiple deployment *targets* that you can set in your webpack [configuration](https://webpack.js.org/configuration)**.

##### 用法

To set the `target` property, you set the target value in your webpack config:

```javascript
// webpack.config.js
module.exports = {
  target: 'node',
};
```

In the example above, **using `node` webpack will compile for usage in a Node.js-like environment** (<font color=FF0000>uses Node.js `require` to load chunks and not touch any built in modules like `fs` or `path` )</font>.

Each *target* has a variety of deployment/environment specific additions, support to fit its needs. See what [targets are available](https://webpack.js.org/configuration/target/).

**注：**更多 target 的可选值，可见：[webpack 文档 - configuration - Target](https://webpack.js.org/configuration/target) ，其中包含了很多可选值

##### Multiple Targets

Although <font color=FF0000>webpack does **not** support multiple strings being passed into the `target` property</font>, you can create an isomorphic（同形的）library by bundling two separate configurations（即：构造两个配置对象）:

```javascript
// webpack.config.js
const path = require('path');
const serverConfig = {
  target: 'node',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'lib.node.js',
  },
  //…
};

const clientConfig = {
  target: 'web', // <=== can be omitted as default is 'web'
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'lib.js',
  },
  //…
};

module.exports = [serverConfig, clientConfig];
```

The example above will create a `lib.js` and `lib.node.js` file in your `dist` folder.

摘自：[webpack 文档 - Guides - Targets](https://webpack.js.org/concepts/targets/)



`npx webpack [entryFileName]` 命令默认使用配置文件是 webpack.config.js 。如果项目中没有 webpack.config.js 同时运行 `npx webpack [entryFileName]` 将会报错，解决方法是：**`npx webpack [entryFileName] --config myConfig.js`**，从而指定一个 webpack 的配置文件



如果习惯于 Vue 或 React 的 `npm run` 命令，可以在 package.json 下的 scripts 项设置

```json
// package.json 文件下
"scripts": {
  "bundle": "webpack"
}
```

它的意思是：当你执行 bundle（自定义的，即也可以换成 start ）命令，它实际执行的是 webpack 打包。这时输入 **`npm run bundle`** 一样可以起到 **`npm webpack`** 打包的效果。这里涉及到的是：**npm scripts**。定义了npm scripts ，使用其他定义的命令只需要用 **`npm run yourScripts`**即可。npm scripts 的作用是，为了避免总是在 CLI 中输入大量的配置，而对命令做出的快捷方式。

> Given it's not particularly fun to run a local copy of webpack from the CLI, we can set up a little shortcut.
>
> 摘自：https://webpack.js.org/guides/getting-started/#npm-scripts

同时，这里使用了 npm webpack，而不是 npx webpack；这是因为如果配置了 npm scripts ，npm 会首先到 node_modules 文件夹下查找是否安装了 webpack，如果找到则直接使用。

推荐阅读：https://webpack.js.org/guides/getting-started/



#### Loader

webpack 默认只会对 js 文件打包，如果想要对其他类型的文件打包，需要在 webpack.config.js 文件中进行设置：

以打包 jpg 图片为例：

```js
module.exports = {
  // 当 webpack 不知道某个模块该如何处理时，回到这边来找配置
	module: {
    // 表示规则，这是一个数组；包含很多规则（rule），每个规则的结构都是类似的
	  rules: [{
      // 当前是告诉webpack，如何处理jpg文件格式的文件
	    test: /\.jpg$/,
      // 告诉webpack，该使用哪一个loader
      // 另外，use也可以是一个数组；比如要处理CSS，要用到多个Loader（css-loader, style-loader）时（关于CSS的设置，见下面）
	    use: {
        // file-loader需要npm install安装，file-loader将会把你的文件（比如说图片）
        // **原封不动的搬到** 你打包的文件（比如dist），并按照你意愿修改文件的名称，配置文件名称，见下面。
        // 如果不配置文件名称，则 webpack 会(默认以hash的方式)重命名文件
	  		loader: 'file-loader'
	  	}
	  }]
	}
}
```

<font color=FF0000>**在 webpack 5中，file-loader、raw-loader、url-loader 被 资源模块（asset module）所取缔**</font>。



<mark>同样，如果需要打包 vue 格式的文件，使用 loader 就需要使用 vue-loader </mark>，关于 vue-loader 可以去 [官网](https://vue-loader.vuejs.org/zh/)  进一步学习

**设置打包后图片的名字，**在 webpack.config.js 文件中设置：

```js
module: {
  rules: [{
    test: /\.(jpg|png|gif)$/, // 多种文件格式的写法，这是一个正则表达式
    use: {
      // 如何处理上面的文件格式，使用哪个loader
  		loader: 'file-loader',
      // 添加额外参数的配置项，options有很多参数，
      // 更多参数详见 https://v4.webpack.docschina.org/loaders/file-loader/#%E9%80%89%E9%A1%B9
      options: {
      	// 这里设置为 [name] 表示打包生成后的名字和之前的一样
        // 这里设置为 [ext] 标示 表示文件名后缀，这里表示和原文件格式一样
        // 在默认情况下：打包后的文件会变成：hash + 原文件的扩展名，即：[contenthash].[ext]
        // 这种使用 [foo] 配置的语法，被称为placeholder，即占位符
        // 更多占位符如[hash]，详见：https://v4.webpack.docschina.org/loaders/file-loader/#placeholders
      	name: '[name].[ext]', 
        // 将文件打包到自定义的文件夹下，这里是dist文件夹下的image文件夹
      	outputPath: 'images/',
    	}
  	}
  }]
}
```

**url-loader**

也可以使用 **url-loader**，它可以实现 file-loader 一样的功能，<font color=FF0000> 同时配置项也非常相似</font>；不过，并不会将图片打包到dist文件夹下；<font color=FF0000> **url-loader 会把图片转变成 Base64（或其他）的 <font size=4>Data URI</font>，写入到打包出的 js 文件中**</font> <mark>（类似于 C++ 中的内联）</mark>。不过这样会带来问题：如果图片很大，该 js 文件将会因此很大，请求 js 文件的时间将会很长，于是页面将会很长时间内没有反应。于是需要进行一个新的设置 **limit**：

```js
use: {
	loader: 'url-loader',
  options: {
  	name: '[name].[ext]', 
  	outputPath: 'images/',
    // 这里的单位是字节，如果文件超过2048字节，那么该文件将不会内联到js文件中
    limit: 2048,
	}
}
```

**补充：**

如果不想使用默认的 base64 的格式，可以通过 encoding 参数进行配置；可选参数包含： "utf8", "utf16le", "latin1", "base64", "hex", "ascii" , "binary" , "ucs2"

另外，如果文件超过 limit 的限制，默认处理的 loader 是 file-loader，但是也可以通过 fallback 参数进行 自定义。示例如下：

```js
options: {
  fallback: require.resolve('responsive-loader'),
},
```



**补充：**

| loader      | 介绍                                                         |
| ----------- | ------------------------------------------------------------ |
| raw-loader  | 加载文件原始内容 ( utf-8 ) 。直接返回 JSON.stringify 后的内容。<br>官方文档中的描述是：A loader for webpack that allows importing files as a String |
| val-loader  | 将代码作为模块执行，并将 exports 转为 JS 代码。字符串形式定义的代码作为模块执行，返回对应的模块 |
| file-loader | 将文件发送到输出文件夹，并返回（相对）URL                    |
| url-loader  | 像 file loader 一样工作，但如果文件小于限制，可以返回 data URL |

摘自：[Webpack的几种文件loader介绍](https://yanyinhong.github.io/2017/07/04/Webpack-file-loader-set-introduction/)



#### 对于样式文件 ( CSS ) 的打包

```js
module: {
  rules: {
    test: /\.(css|scss)$/,
    use: ['style-loader', 'css-loader', 'postcss-loader', 'sass-loader']
  }
}
```

- **css-loader：**<font color=FF0000> 会分析所有 CSS 文件的关系，最后将这些CSS文件合并为一个CSS文件</font>
- **sass-loader：**如果需要打包到文件中包含 scss / sass，则需要使用 sass-loader，于是use变成了 `['style-loader', 'css-loader', 'sass-loader']`。同时，想要使用 sass-loader，还需要安装 node-sass。注：node-sass 非常容易出问题，推荐使用 dart-sass
- **style-loader：**<font color=FF0000> 在得到 css-loader 输出的内容后，会把生成的 css 文件挂载到页面的 header 部分；即：生成一个 `<style>...</style>` 并插入</font>
- **postcss-loader：**见下面。

<font color=FF0000>**在webpack中，loader的使用是<font size=4>有先后顺序</font>的，分别是：<font size=4>从下到上，从右到左</font>。**</font> 所以，上面关于 sass 的讲解，是 sass-loader 先解析 sass，交给 css-loader 分析所有 css 关系，并整合成一个文件；最后，style-loader 挂载到 header 上



如果想要针对不同的浏览器进行适配（需要写前缀，如 `webkit-` ），<font color=FF0000> **这时需要使用 postcss-loader**</font>，

想要对 postcss-loader 进行配置，需要添加并配置 postcss.config.js 配置文件，可以使用 autoprefixer 插件

```js
// 当前文件是：postcss.config.js
module.exports = {
  // 配置可使用的插件。plugins除了可以是数组外，也可以是对象，即使有多个，也可以以'foo': {}形式设置
  plugins: [
    require('autoprefixer')
  ]
}
```

同时，也可以使用 browerslist 设置对浏览器的支持：

```json
// package.json
"browserslist": [
  "> 1%", // 支持市场份额大于1%的浏览器
  "last 2 versions", // 并且支持这些浏览器到上两个版本
] 
```

更多配置项见 GitHub：https://github.com/browserslist/browserslist



如果想要对 css-loader （或者某一个 loader ）增加配置项，则需要将字符串配对象的形式，将其改成对象形式：

```js
// 当前文件webpack.config.js。
// 这里use省略掉了上层配置项 module 和 rules
use: [
  'style-loader', 
  {
    loader: 'css-loader',
    options: {
      // 由于loader的使用是有先后顺序的：从下到上，从右到左；
      // 且如果SCSS文件中出现嵌套引用，由于已经执行到了css-loader部分，
      // 将无法再使用sass-loader，这时候就需要使用importLoader，使之再按照顺序重新执行一遍
      // 这样：无论是在js文件中引入scss文件，还是在scss文件中再引入其他scss文件，在打包时都按照顺序执行
      importLoaders: 2,
      // 使用模块化的CSS，避免全局样式干扰
      modules: true,
    }
  }, 
  'postcss-loader'
  'sass-loader',
]
```

如果使用模块化 CSS，那么在文件中导入 CSS 模块时，需要将 `import 'cssPath'`，改为 `import style from 'cssPath'` 。如果想要使用 模块 CSS 中的样式，要使用 style.className。

同时，如果没有模块化的 CSS，会导致引入的图片样式变成全局作用的，这很不利于样式的控制；所以我们需要模块化的 CSS 。在webpack 中启用模块化 CSS，只需要在 options 中添加 `modules: true` 即可（如上的代码）

##### css-loader 知识补充

> css-loader 默认的哈希算法是 `[hash:base64]` ，这会将 （CSS 类）`.title` 编译成 `._3zyde4l1yATCOkgn-DBWEL` 这样的字符串
>
> 摘自：[阮一峰 - CSS Modules 用法教程](http://www.ruanyifeng.com/blog/2016/06/css_modules.html)

类似的说法，也可以在 css-loader 的 GitHub readme 中找到：

> ##### `localIdentName`
>
> Type:
>
> ```
> type localIdentName = string;
> ```
>
> Default: `'[hash:base64]'`
>
> 摘自：[GitHub - css-loader - readme - localIdentName](https://github.com/webpack-contrib/css-loader#localidentname)



##### style-loader 的 options

| 名称              | 类型               | 默认值    | 描述                                             |
| :---------------- | :----------------- | :-------- | :----------------------------------------------- |
| injectType        | {String}           | styleTag  | 配置把 styles 插入到 DOM 中的方式                |
| attributes        | {Object}           | {}        | 添加自定义属性到插入的标签中                     |
| insert            | {String\|Function} | head      | 在指定的位置插入标签                             |
| styleTagTransform | {String\|Function} | undefined | 当将 'style' 标签插入到 DOM 中时，转换标签和 css |
| base              | {Number}           | true      | 基于 (DLLPlugin) 设置 module ID                  |
| esModule          | {Boolean}          | false     | 使用 ES modules 语法                             |

摘自：[webpack官方文档 - style-loader - options](https://webpack.docschina.org/loaders/style-loader/#options)

##### css-loader 的 options

| 名称          | 类型                      | 默认值           | 描述                                                |
| :------------ | :------------------------ | :--------------- | :-------------------------------------------------- |
| url           | {Boolean\|Function}       | true             | 启用/禁用 url() / image-set() 函数处理              |
| import        | {Boolean\|Function}       | true             | 启用/禁用 @import 规则进行处理                      |
| modules       | {Boolean\|String\|Object} | {auto: true}     | 启用/禁用 CSS 模块及其配置                          |
| sourceMap     | {Boolean}                 | compiler.devtool | 启用/禁用生成 SourceMap                             |
| importLoaders | {Number}                  | 0                | 启用/禁用或者设置在 css-loader 前应用的 loader 数量 |
| esModule      | {Boolean}                 | true             | 使用 ES 模块语法                                    |

摘自：[webpack官方文档 - css-loader - options](https://webpack.docschina.org/loaders/css-loader/#options)



**webpack打包字体**（eot / ttf / svg 格式），可以使用 file-loader（V4的官方文档中使用的是file-loader）

```js
{
  test: /\.(eot|ttf|svg)/,
  use: {
    loader: 'file-loader',
  }
}
```



##### 补充：自定义 JSON 模块 parser

通过使用 自定义 parser 替代特定的 webpack loader，可以将任何 toml、yaml 或 json5 文件作为 JSON 模块导入。

示例如下：

- 先安装 toml，yamljs 和 json5 的 packages：

  ```sh
  npm install toml yamljs json5 --save-dev
  ```

- 配置webpack.config.js，分为 分别引入 模块 和 分别配置解析器（parser）

  ```js
  const toml = require('toml');
  const yaml = require('yamljs');
  const json5 = require('json5');
  
  module: {
    rules: [
      {
        test: /\.toml$/i,
        type: 'json',
        parser: {
          parse: toml.parse,
        },
      },
      {
        test: /\.yaml$/i,
        type: 'json',
        parser: {
          parse: yaml.parse,
        },
      },
      {
        test: /\.json5$/i,
        type: 'json',
        parser: {
          parse: json5.parse,
        },
      },
    ]
  }
  ```

  

#### Asset Modules

资源模块(asset module)是一种模块类型，它允许使用资源文件（字体，图标等）而无需配置额外 loader。

在 webpack 5 之前，通常使用：

- **raw-loader** 将文件导入为字符串
- **url-loader** 将文件作为 data URI 内联到 bundle 中
- **file-loader** 将文件发送到输出目录

资源模块类型(asset module type)，通过添加 4 种新的模块类型，来替换所有这些 loader：

- **asset/resource：**发送一个单独的文件并导出 URL。<font color=FF0000> 之前通过使用 file-loader 实现</font>。

  ```js
  module: {
    rules: [
      {
        test: /\.png/,
        type: 'asset/resource'
      }
    ]
  },
  ```

  默认情况下，asset/resource 模块以 \[hash]\[ext][query] 文件名发送到输出目录。可以通过在 webpack 配置中设置 output.assetModuleFilename 来修改此模板字符串。示例如下：

  ```js
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
    assetModuleFilename: 'images/[hash][ext][query]'
  },
  ```

  但是，这是针对所有文件的配置，可以自定义输出文件名的方式，将某些资源发送到指定目录。这里使用 rules.generator.filename

  ```js
  rules: [
    {
      test: /\.png/,
      type: 'asset/resource'
    },
    {
      test: /\.html/,
      type: 'asset/resource',
      generator: {
        filename: 'static/[hash][ext][query]'
      }
    }
  ]
  ```

  使用此配置，所有 html 文件都将被发送到输出目录中的 static 目录中。

- **asset/inline：**导出一个资源的 data URL。<font color=FF0000> 之前通过使用 url-loader 实现</font>。

  webpack 输出的 data URL，默认是呈现为使用 Base64 算法编码的文件内容。如果想要自定义 编码算法，示例如下：

  ```js
  rules: [
    {
      test: /\.svg/,
      type: 'asset/inline',
      generator: {
        dataUrl: content => {
          content = content.toString();
          return svgToMiniDataURI(content);
        }
      }
    }
  ]
  ```

  现在，所有 .svg 文件都将通过 mini-svg-data-uri 包进行编码。

- **asset/source：**导出资源的源代码。<font color=FF0000> 之前通过使用 raw-loader 实现</font>。

- **asset：**在导出一个 data URI 和发送一个单独的文件之间自动选择。<font color=FF0000> 之前通过使用 url-loader，并且配置资源体积限制实现</font>。

  webpack 按照默认条件，自动地在 resource 和 inline 之间进行选择：小于 8kb 的文件，将会视为 inline 模块类型，否则会被视为 resource 模块类型。可以通过在 webpack 配置的 module rule 层级中，设置 Rule.parser.dataUrlCondition.maxSize 选项来修改此条件

  ```js
  rules: [
    {
      test: /\.txt/,
      type: 'asset',
      parser: {
        dataUrlCondition: {
          maxSize: 4 * 1024 // 4kb
        }
      }
    }
  ]
  ```

  还可以 指定一个函数（详见：https://webpack.js.org/guides/asset-modules/#:~:text=Also%20you%20can-,specify%20a%20function,-to%20decide%20to） 来决定是否 inline 模块



##### 想要在Webpack V5中继续使用 被废弃的 assets loader

当在 webpack 5 中使用旧的 assets loader（如 file-loader/url-loader/raw-loader 等）和 asset 模块时，你可能想停止当前 asset 模块的处理，并再次启动处理，这可能会导致 asset 重复，你可以通过将 asset 模块的类型设置为 'javascript/auto' 来解决。示例如下：

```js
module.exports = {
  module: {
   rules: [
      {
        test: /\.(png|jpg|gif)$/i,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192,
            }
          },
        ],
        type: 'javascript/auto'
      },
   ]
  },
}
```

如需从 asset loader 中排除来自新 URL 处理的 asset，请添加 `dependency: { not: ['url'] }` 到 loader 配置中。



#### Loader 文档补充

在你的应用程序中，有两种使用 loader 的方式：配置方式 和 内联方式

##### 配置方式（推荐）

**配置方式 ** 在 **webpack.config.js** 文件中指定 loader。即：使用 module.rules 配置。

loader 按照 从右到左（或从下到上）地取值 ( evaluate ) / 执行 ( execute )

##### 内联方式

内联方式 在每个 `import` 语句中显式指定 loader

可以在 `import` 语句 或 任何与 *"import" 方法同等的引用方式* 中指定 loader。<font color=FF0000>使用 `!` 将资源中的 loader 分开</font>。每个部分都会相对于当前目录解析。

通过为内联 `import` 语句添加前缀，可以覆盖 (overload) 配置 中的所有 loader, preLoader 和 postLoader：

- 使用 `!` 前缀，将禁用所有已配置的 normal loader（普通 loader）

  ```js
  import Styles from '!style-loader!css-loader?modules!./styles.css';
  ```

- 使用 `!!` 前缀，将禁用所有已配置的 loader（preLoader, loader, postLoader）

  ```js
  import Styles from '!!style-loader!css-loader?modules!./styles.css';
  ```

- 使用 `-!` 前缀，将禁用所有已配置的 preLoader 和 loader，但是不禁用 postLoaders

  ```js
  import Styles from '-!style-loader!css-loader?modules!./styles.css';
  ```

选项可以传递查询参数，例如 `?key=value&foo=bar`，或者一个 JSON 对象，例如 `?{"key":"value","foo":"bar"}`。

注意 ⚠️：在 webpack v4 版本可以通过 CLI 使用 loader，但是在 webpack v5 中被弃用。

##### loader 特性

- <font color=FF0000>**loader 支持链式调用**</font>。链中的每个 loader 会将转换应用在已处理过的资源上。一组链式的 loader 将按照相反的顺序执行。链中的第一个 loader 将其结果（也就是应用过转换后的资源）传递给下一个 loader，依此类推。最后，链中的最后一个 loader，返回 webpack 所期望的 JavaScript。
- <font color=FF0000>loader 可以是同步的，也可以是异步的</font>。
- loader 运行在 Node.js 中，并且能够执行任何操作。
- <font color=FF0000>loader 可以通过 `options` 对象配置</font>（仍然支持使用 `query` 参数来设置选项，但是这种方式已被废弃）。
- 除了<font color=FF0000>**常见的通过 `package.json` 的 `main` 来将一个 npm 模块导出为 loader**</font>，还可以在 module.rules 中使用 `loader` 字段直接引用一个模块。
- <font color=FF0000>插件 ( plugin ) 可以为 loader 带来更多特性</font>。
- loader 能够产生额外的任意文件。

通过 loader 的预处理函数，可以自定义输出。用户现在可以更加灵活地引入细粒度逻辑，例如：压缩、打包、语言转译（或编译）和 [更多其他特性](https://webpack.js.org/loaders)。

摘自：[webpack 文档 - Loaders](https://webpack.js.org/concepts/loaders/#example) 



#### 使用 plugins 插件

**plugin 的作用：**plugin 可以在<font color=FF0000> webpack 运行到某个时刻的时候 </font>，自动地帮你做一些事情

##### htmlWebpackPlugin

每次打包项目时，入口的 html 文件 ( index.html ) 都要<font color=FF0000>手动构建</font>，很麻烦；这时就需要 **htmlWebpackPlugin** 。代码示例如下：

```js
var HtmlWebpackPlugin = require('html-webpack-plugin');
var path = require('path');

module.exports = {
  entry: 'index.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'index_bundle.js'
  },
  plugins: [
    // 生成一个html-webpack-plugin的实例
    new HtmlWebpackPlugin({
    // 这里可以配置一个模版，自动生成的index.html会根据这个模版来生成
    // 之所以需要模版，是因为虽然html-webpack-plugins会手动导入打包生成的文件
    // 但是与打包无关的部分内容仍需要添加，比如<head>中的meta属性、无需打包的script、引入的js文件中调用的html的dom
    template: 'src/index.html',
  })]
};
```

**htmlWebpackPlugin 的作用：** htmlWebpackPlugin 会在打包结束后，<font color=FF0000> 自动生成一个 html 文件</font>， 并<font color=FF0000>把打包生成的 js 自动引入到这个 html 文件中</font>。该插件在打包后执行。

<font size=4>**补充：**</font>html-webpack-plugin 的 GitHub 地址：https://github.com/jantimon/html-webpack-plugin，由于 webpack 官方问答中介绍的比较少，更多的介绍与配置可以看这个：

> <font color=FF0000>HtmlWebpackPlugin</font> 插件除了可以帮助我们简化 HTML 文件的创建，<font color=FF0000>**也可以压缩 HTML 文件**</font>。
>
> ```js
> // webpack.config.js
> const HtmlWebpackPlugin = require("html-webpack-plugin");
> 
> module.exports = {
> 	plugins: [new HtmlWebpackPlugin()],
> };
> ```
>
> 如果不添加任何配置的话（如上，就是一个 `new HtmlWebpackPlugin()` ），会生成一个默认 index.html 文件，并自动注入所有的 chunk 和压缩。也可以通过自定义配置参数，以下几个是常见的参数：
>
> - **template**：模板的路径，默认会去寻找 `src/index.ejs` 是否存在
> - **filename**：输出文件的名称，默认为 index.html
> - **inject**：是否将资源注入到模版中，默认为 true
> - **minify**：压缩参数。在生产模式下 ( production ) ，默认为 true ；否则，默认为 false
>
> <font color=FF0000>**如果 minify 为 true**</font>，生成的 HTML 将使用 [html-minifier-terser](https://github.com/terser/html-minifier-terser) 和以下选项进行压缩：
>
> ```js
> {
>   collapseWhitespace: true,
>   keepClosingSlash: true,
>   removeComments: true,
>   removeRedundantAttributes: true,
>   removeScriptTypeAttributes: true,
>   removeStyleLinkTypeAttributes: true,
>   useShortDoctype: true
> }
> ```
>
> 摘自：[webpack 完全指南：代码压缩](https://juejin.cn/post/7031115698633965582)

##### clean-webpack-plugin 插件

在打包之后生成了打包后输出的 JS 文件，如果这时<font color=FF0000> 修改了配置文件</font> ( webpack.config.js ) <font color=FF0000> 中的打包输出js的文件名称</font>，再进行打包，这时会发现目标文件夹中有两个打包输出的js文件（分别是配置修改前生成的 JS 文件，和配置修改后生成的 JS 文件）。可以<font color=FF0000> 使用 clean-webpack-plugin 在生成新的 JS 文件时，清除掉旧的 JS 文件</font>。代码示例如下：

```js
plugins: [
  // 指定作用的文件夹，将会打包前先删除掉目标文件夹下所有的文件
  new CleanWebpackPlugin(['dist']),
]
```

该插件在打包前执行

**补充：** webpack V5 中添加了 output.clean 选项，用 boolean 值 控制；可用来替代 clean-webpack-plugin

> As you might have noticed over the past guides and code example, our `/dist` folder has become quite cluttered （烂七八糟的）. Webpack will generate the files and put them in the `/dist` folder for you, but it doesn't keep track of which files are actually in use by your project.
>
> <font color=FF0000>In general it's good practice to clean the `/dist` folder before each build</font>, so that only used files will be generated. Let's take care of that with **[`output.clean`](https://webpack.js.org/configuration/output/#outputclean)** option.
>
> ```diff
> // webpack.config.js
> module.exports = {
>   output: {
>     filename: '[name].bundle.js',
>     path: path.resolve(__dirname, 'dist'),
> +   clean: true
>   }
> }
> ```
>
> 摘自：[webpack - Guide - Output Management - Cleaning up the /dist folder](https://webpack.js.org/guides/output-management/#cleaning-up-the-dist-folder)



#### Plugin 文档补充

##### 总述

<font color=FF0000 size=4>**Plugins** are the [backbone](https://github.com/webpack/tapable) </font>（支柱、骨干）<font color=FF0000 size=4>of webpack</font>. <font color=FF0000>Webpack itself is built on the **same plugin system** that you use in your webpack configuration</font>!

They also <font color=FF0000>serve the purpose of doing **anything else** that a loader cannot do</font>. Webpack provides many such plugins out of the box （开箱即用插件，可理解为：webpack 内置了一些 plugins）.

##### 原理

A webpack plugin is a JavaScript object that has an `apply` method. This `apply` method is <font color=FF0000 size=4>**called by the webpack compiler**, giving access to the **entire** compilation lifecycle</font>. 感觉设计上类似于 Vue plugins 的 install 方法。示例如下：

```js
// ConsoleLogOnBuildWebpackPlugin.js
const pluginName = 'ConsoleLogOnBuildWebpackPlugin';

class ConsoleLogOnBuildWebpackPlugin {
  apply(compiler) {
    compiler.hooks.run.tap(pluginName, (compilation) => {
      console.log('The webpack build process is starting!');
    });
  }
}

module.exports = ConsoleLogOnBuildWebpackPlugin;
```

The first parameter of the `tap` method of the compiler hook should be **a camelized version of the plugin name**. It is advisable to use a constant for this so it can be reused in all hooks.

##### 用法

Since **plugins** can take arguments / options, you must pass a `new` instance to the `plugins` property in your webpack configuration.

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack'); //to access built-in plugins

module.exports = {
  // ...
   plugins: [
    new webpack.ProgressPlugin(),
    new HtmlWebpackPlugin({ template: './src/index.html' }),
  ],
}
```

##### Node API 方式

在使用 Node API 时，还可以通过配置中的 `plugins` 属性传入插件。

```js
// some-node-script.js

const webpack = require('webpack'); // 访问 webpack 运行时(runtime)
const configuration = require('./webpack.config.js');

let compiler = webpack(configuration); // 注：注意这里的写法，以及下面 compiler 的使用

new webpack.ProgressPlugin().apply(compiler);

compiler.run(function (err, stats) {
  // ...
});
```

摘自：[webpack 文档 - Plugins](https://webpack.js.org/concepts/plugins/)



#### Entry 和 Output 的配置

entry 和 output 的文件名，默认都是 main.js 。在 entry 配置项中配置 **输入文件的文件名**，可以写成字符串 'entryFileName' ，也可以写成对象，对象的键。示例如下：

另外，<font color=FF0000>**如果想要对一个文件打包两次（即获得两个打包结果），可以进行如下配置**</font>：

```js
module.exports = {
  entry: {
    main: './src/index.js',
    sub: './src/index.js',
  },
  output: {
    // 上面是打包两次，所以如果这里只设置一个文件名的话会报错（比如设置为 bundle.js ），所以下面使用这种方式 
    // **这里的 name ，对应着entry的键**；在这里是'main'和'sub'，打包生成的文件名也就是main.js和sub.js
    filename: '[name].js',
    path: path.resolve(__dirname, 'bundle') 
  }
}
```

如果想要在打包输出文件前添加一个前缀（比如一个网址路径），可以在 output 中加上 publicPath。关于 publicPath，在 Vue CLI 的配置中也有 publicPath，且作用相同（毕竟 Vue CLI 是 webpack 的 简易化封装）

> 默认情况下，Vue CLI 会假设你的应用是被部署在一个域名的根路径上（**注：**默认值为 `'/'` ），例如： `https://www.my-app.com/` 。如果应用被部署在一个子路径上，你就需要用这个选项指定这个子路径。例如，如果你的应用被部署在 `https://www.my-app.com/my-app/` ，则设置 `publicPath` 为 `/my-app/`
>
> 摘自：[Vue CLI 官方文档  - vue.config.js - publicPath](https://cli.vuejs.org/zh/config/#publicpath) 。

```js
output: {
  publicPath: 'http://cdn.com.cn',
  // 剩下的和之前一样
  filename: '[name].js',
  path: path.resolve(__dirname, 'bundle') 
}
```

这时，所有的输出文件会变成：**`http://cdn.com.cn/bundle/*.js`**



// TODO 

阅读output相关：https://webpack.js.org/configuration/output/  

尽量读完Entry相关：https://webpack.js.org/configuration/entry-context/

HtmlWebpackPlugin相关：https://webpack.js.org/plugins/html-webpack-plugin/ 

https://github.com/jantimon/html-webpack-plugin



#### SourceMap 配置

sourceMap 是一个<font color=FF0000>映射关系</font>。它知道 <font color=FF0000> 打包输出的文件的代码行</font> 与对应的 <font color=FF0000> 被打包的源代码中代码行</font> 的映射关系；**便于在打包代码出现错误时候，可以指向源代码的<font color=FF0000>某一行某一列（即精确到字符）</font>出现的问题**。<font color=FF0000>使用sourceMap打包速度会变慢（尤其是大型项目）</font>。

如何启用sourceMap？只需要使用 **`devtool: source-map`** 即可。同时，启用sourceMap后，在dist文件夹下会出现一个 main.js.map 的映射对应关系文件，这个文件实际上是一个<mark>VLQ的编码集合</mark>

**devtool的选项中包含各种修饰符，比如 inline、cheap、module**，说明如下：

- **devtool: 'inline-source-map'**：main.js.map文件将不会存在在dist文件夹中，main.js.map的内容实际上会被包含在打包输出的js文件中。

  > map文件不会出现在打包之后的文件夹中，<font color=FF0000>这实际上是 inline 起到的作用</font>。另外，该映射关系（即 map 文件中的内容）会以 base64 的格式 存放在打包后的 js 文件中。
  >
  > 学习自：[配置SourceMap【Webpack】](https://www.bilibili.com/video/BV1h5411G7tm)

- **devtool: cheap-source-map**：如果不加 cheap，将会精确到源码的<font color=FF0000>某一行某一列（即精确到字符）</font>，而使用cheap，将会精确到代码的某一行，不会精确到某个字符，这样大大提高了打包效率。

  同时，cheap还有一个功能：<font color=FF0000>**如果添加了cheap，那么打包时只会管业务代码的映射**</font>，<font color=FF0000>对于其他的代码（比如 loader、第三方模块）将不会关心</font>。（和下面的 module 形成了对照）

- **devtool: cheap-module-source-map**： 如果想要对于上面所说的<font color=FF0000> 其他的非业务代码（比如loader、第三方模块）也会做映射</font>，监控其中的错误，<font color=FF0000>可以加上 **module**</font>

- **detvool: 'eval'**：<font color=FF0000>是打包执行效率最快的</font>（除了为 'none' 外），使用 eval() 函数来生成。虽然它同样可以定位错误的源代码；不过，不是记录对应关系；而是将错误直接记录在打包输出的js文件中，不过对于复杂的项目，使用 eval 可能提示错误并不全面 / 不准确。

- **devtool: 'none'** ：表示省略 devtool 选项 - 不生成 sourceMap。

**总结：**

| 关键字     | 含义                                            |
| ---------- | ----------------------------------------------- |
| eval       | 使用 eval 包裹代码                              |
| source-map | 生成.map文件                                    |
| cheap      | 不包含列信息，也不包括loader的sourcemap         |
| module     | <font color=FF0000>包括loader的sourcemap</font> |
| inline     | 将 .map 作为 DataURL 嵌入，不单独生成 .map文件  |

摘自：[webpack——devtool配置及sourceMap的选择](https://blog.csdn.net/zwkkkk1/article/details/88758726)

##### 最佳实践

- 在<font color=FF0000>开发环境</font>中，建议使用 **cheap-module-eval-source-map** ，这样提示出的错误是比较全的，同时打包速度也很快
- 在<font color=FF0000>生产环境</font>中，一般是没有必要使用devtool的。但是如果还是想要查看错误，建议使用 **cheap-module-source-map**，这样提示效果会更好一些

##### 浏览器中使用 source map 的补充

需要启用 Enable JavaScript source maps 和 Enable CSS source maps

<img src="https://s2.loli.net/2022/05/08/2BEgKHUd1RLZzIe.png" alt="image-20220309012608227" style="zoom:47%;" />

学习自：[深入浅出之 Source Map](https://juejin.cn/post/7023537118454480904)

##### 关于 js 中 source map 原理的补充

背景 以及 source map 是什么

> <mark>在生产环境中，代码一般是以编译、压缩后的形态存在，对生产友好，但是调试时候定位的错误只能定位到编译压缩后的代码位置，此时的代码对人类的阅读很不友好，可能变量名被缩短失去语义，甚至是经过编译的，生产代码与开发代码已经没法一一对应</mark>。
>
> 为了解决代码之间对应关系，人们设计了 source map 这种数据格式来。source map 就像一个索引表，将生产代码和开发代码联系起来，这样开发调试时就可以清晰的定位到开发代码中了。
>
> **前端的编译处理过程包括但不限于**
>
> - 转译器 / Transpilers ( Babel )
> - 编译器 / Compilers ( TypeScript，CoffeeScript，Webassembly )
> - 压缩 / Minifiers  ( UglifyJS )
>
> 这些都是可生成 source map 的操作。有了 source map，使得我们调试线上产品时，能直接看到开发环境的代码
>

<font size=4>**Source map 的格式**</font>

打开Source map文件，它大概是这个样子：

```js
{
  version : 3,
  file: "out.js",
  sourceRoot : "",
  sources: ["foo.js", "bar.js"],
  names: ["src", "maps", "are", "fun"],
  mappings: "AAgBC,SAAQ,CAAEA"
}
```

整个文件就是一个JavaScript对象，可以被解释器读取。它主要有以下几个属性：

- version：Source map的版本，目前为3。
- file：转换后的文件名。
- sourceRoot：转换前的文件所在的目录。如果与转换前的文件在同一目录，该项为空。
- **sources：**<font color=FF0000>**转换前** 的文件</font>。该项是一个数组，表示<font color=FF0000>可能存在多个文件合并</font>。
- names：<font color=FF0000>**转换前** 的所有变量名和属性名</font>。
- **mappings：**<font color=FF0000>记录位置信息的字符串</font>

<font color=FF0000 size=4>**mappings 属性**</font>

记录两个文件的各个位置是如何一一对应的，关键就是 map 文件的 mappings 属性。这是一个很长的字符串，它分成三层（**注：**这里阮一峰说的很不清楚。<mark>这里的“三层分类”的“层次”是粒度越来越细的，先将 mappings 分为“行”，再将“行”分为“位置”，最后“位置”是用 VLQ 描述的</mark>）

- 第一层是<font color=FF0000>**行对应**</font>，以分号`;` 表示，<font color=FF0000>**每个分号对应转换后源码的一行**</font>。所以，第一个分号前的内容，就对应源码的第一行，以此类推
- 第二层是<font color=FF0000>**位置对应**</font>，以逗号`,` 表示，<font color=FF0000>**每个逗号对应转换后源码的一个位置**</font>。所以，第一个逗号前的内容，就对应该行源码的第一个位置，以此类推。
- 第三层是<font color=FF0000>**位置转换**</font>，<font color=FF0000>以 VLQ 编码表示，代表该位置对应的转换前的源码位置</font>。

举例来说，假定mappings属性的内容如下：

```js
mappings:"AAAAA,BBBBB;CCCCC"
```

就表示：<font color=FF0000>转换后的源码分成两行，第一行有两个位置，第二行有一个位置</font>。**注：**因为分号将 mapping 分成了两段，所以两行；分号前面的内容被逗号分开，所以第一行有两个位置

<font color=FF0000 size=4>**位置对应的原理**</font>

<font color=FF0000>每个位置使用五位  ，表示五个字段</font>（**注：**根据上面，位置通过逗号分隔；同样上面的示例也是 AAAAA BBBBB 有五位）。从左边算起：

　　- 第一位：表示这个位置在（<font color=FF0000>**转换后**的代码的）的第几列</font>
　　- 第二位：表示这个位置属于 sources 属性中的哪一个文件（**注：**根据上面所说，source 是 **转换前** 的文件）
　　- 第三位：<mark style="background: aqua">表示<font color=FF0000>这个位置属于 **转换前** 代码的**第几行**</font></mark>
　　- 第四位：<mark style="background: aqua">表示<font color=FF0000>这个位置属于 **转换前** 代码的**第几列**</font></mark>（**注：**行和列。所以，第三第四位可以连起来看）
　　- 第五位：表示这个位置属于 names 属性中的哪一个变量（**注：**根据上面所说，names 是 **转换前** 的文件名）

**注：**<mark>辅助记忆：第一二两位是转换后的位置信息，三四五位是转换前的位置信息</mark>

有几点需要说明。首先，<font color=FF0000>**所有的值都是以 0 作为基数的**</font>。其次，<font color=FF0000>第五位不是必需的</font>，<mark>如果该位置没有对应names属性中的变量，可以省略第五位</mark>。再次，<font color=FF0000>每一位都采用VLQ编码表示；**由于VLQ编码是变长的，所以每一位可以由多个字符构成**</font>。

如果某个位置是AAAAA，由于A在VLQ编码中表示0，因此这个位置的五个位实际上都是0。它的意思是，该位置在转换后代码的第0列，对应sources属性中第0个文件，属于转换前代码的第0行第0列，对应names属性中的第0个变量。

**VLQ 编码表示数值**

这种编码最早用于 MIDI 文件，后来被多种格式采用。它的特点就是可以非常精简地表示很大的数值。

<font color=FF0000>**VLQ编码是变长的：**如果（整）数值在 -15到 +15之间（含两个端点），用一个字符表示；超出这个范围，就需要用多个字符表示</font>。它规定，每个字符使用6个两进制位，正好可以借用 Base64 编码的字符表。

![img](https://s2.loli.net/2022/03/09/ucvlDPgopVZkted.png)

<font color=FF0000>**在这6个位中，左边的第一位（最高位）表示是否“连续”( continuation )**</font>。如果<font color=FF0000>是1</font>，代表<mark>这６个位后面的6个位也属于同一个数</mark>；如果<mark>是0</mark>，表示<mark>该数值到这6个位结束</mark>。**注：**辅助记忆：1 为 truthy，所以表示连续；0 表示 falsy，所以表示不连续。

<font color=FF0000>这6个位中的 **右边最后一位（最低位）** 的含义</font>，<font color=FF0000>取决于这6个位是否是某个数值的VLQ编码的第一个字符</font>。**如果是的，这个位代表"符号"(sign)** ，0为正，1为负（Source map的符号固定为0）；如果不是，这个位没有特殊含义，被算作数值的一部分。**注：**辅助记忆：二进制表示中符号位 0 为正，1为负。

```
Continuation 是否连续
|　　　　　Sign 符号位
|　　　　　|
V　　　　　V
１０１０１１
```

**VLQ编码：实例**

下面看一个例子，如何对数值16进行VLQ编码。

- 第一步，将16改写成二进制形式10000。
- 第二步，<mark>在最右边补充符号位</mark>。因为16大于0，所以符号位为0，整个数变成100000。
- 第三步，<font color=FF0000>从右边的最低位开始，将整个数每隔5位，进行分段</font>（**注：**分段的目的是为了加上“连续位”，见第五步），即变成1和00000两段。<font color=FF0000>**如果最高位所在的段不足5位，则前面补0**，因此两段变成00001和00000</font>。
- 第四步，<font color=FF0000 size=4>**将两段的顺序倒过来，即00000和00001**</font>。
- 第五步，在每一段的最前面添加一个"连续位"，除了最后一段为0，其他都为1，即变成100000和000001。
- 第六步，将每一段转成Base 64编码。

查表可知，100000为g，000001为B。因此，数值16的VLQ编码为gB

摘自：[阮一峰 - JavaScript Source Map 详解](http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html)

另外，本文没有提及 sourcemap 中 .js.map 文件中字段间的关系，和这些字段对记录”转换前“和”转换后“代码变化的作用；还有 .js.map 文件 记录变化的原理，都没有说。这些内容，详见 [D-kylin - note - Base64 VLQ](https://github.com/D-kylin/note/blob/master/VLQ编码.md)

还有，令人感觉有些奇怪的是：在 [MDN - http - SourceMap](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/SourceMap) 中说到：SourceMap 是一个HTTP 响应头，使浏览器能够重建原始的资源然后显示在调试器里。这个，从没有听说过...



#### webpack-dev-server

##### 介绍 webpack-dev-server

webpack-dev-server可以用来实现<font color=FF0000>热部署</font>，即修改源代码，不需要在输入 `npm run yourScript` 或者 `npx webpack *` 命令，即可自动完成打包。

**方法有三种：**

- 在 package.json 的 npm scripts 中进行如下设置：

  ```js
  "scripts": {
    "watch": "webpack --watch"
  }
  ```

  然后输入 `npm run watch` 以使用。

- **使用 webpack-dev-server（<font color=FF0000>最推荐</font>）**

  上面的已经简单实现了，不过这还不够好，希望可以实现自动打包、自动打开浏览器、在代码改变后自动刷新浏览器、还可以模拟服务器上的特性。示例如下：

  ```js
  //在webpack.config.js配置文件中，与entry同一层级
  devServer: {
    // 服务器要启在哪一个文件夹下。需要注意的是：webpack@5中，将contentBase 去掉了；使用static以替换？
    contentBase: './dist',
  	// 自动打开一个新的 Tab，并访问配置的url
    open: true,
    // 设置端口号
    port: 8080,
  }
  ```

  ```json
  // 在package.json配置文件中的scripts下：
  "scripts": {
    "start": "webpack-dev-server",
  }
  ```

  同时，需要安装 webpack-dev-server。

  这时启动项目只需要输入 npm run start 即可

  另外：如其名，webpack-dev-server 只在 development 环境中需要被使用（配置），production 环境不需要使用（配置）

  <font color=FF0000>**使用 webpack-dev-server 以启用模拟服务器的原因：在服务器上启动项目，可以使用http协议，而在本地手动打开项目，虽然项目也能运行，但是当前只在 file 协议下，file 协议无法使用 ajax 等 web 服务**</font>

  在 React / Vue 的脚手架配置 中都会有 Proxy 这项配置，这是在做跨域的接口模拟时的接口代理。之所以都有 Proxy，是因为 React / Vue 的底层都使用了 webpack 的 devServer

- 自己手动实现 webpack-dev-server，在 package.json 配置文件中的 scripts（如下示例）。然后自己使用 node 编写 server.js 代码（非常复杂，不推荐）

  ```json
  "scripts": {
    "server": "node server.js"
  }
  ```

#### 开发环境 文档补充

> ##### 痛点与需求
>
> It quickly becomes a hassle（麻烦） to manually run `npm run build` <mark>every time you want to compile your code</mark>.
>
> ##### 解决方法
>
> There are a couple of different options available in webpack that help you automatically compile your code whenever it changes:
>
> 1. webpack's [**Watch Mode**](https://webpack.js.org/configuration/watch/#watch)
>
>    > You can <font color=FF0000>instruct webpack to **"watch" all files within your dependency graph for changes**</font>. <font color=FF0000>**If one of these files is updated, the code will be recompiled**</font> so you don't have to run the full build manually.
>    >
>    > ```json
>    > // package.json
>    > {
>    >   "scripts": {
>    >       "watch": "webpack --watch"
>    >    }
>    > }
>    > ```
>    >
>    > Now run `npm run watch` from the command line and see how webpack compiles your code. You can see that it doesn't exit the command line because the script is currently watching your files.
>    >
>    > **The only downside（缺点）** is that <font color=FF0000>**you have to refresh your browser in order to see the changes**</font>（即：不支持 HMR ）. It would be much nicer if that would happen automatically as well
>    >
>    > 摘自：[webpack 文档 - Guide - Development - Using Watch Mode](https://webpack.js.org/guides/development/#using-watch-mode)
>
> 2. [**webpack-dev-server**](https://github.com/webpack/webpack-dev-server)
>
>    > The webpack-dev-server <font color=FF0000>provides you with a **rudimentary （基本的） web server**</font> and <font color=FF0000>the **ability to use live reloading**</font>
>    >
>    > ```js
>    > module.exports = {
>    >     devServer: {
>    >       static: './dist'
>    >     },
>    >     optimization: {
>    >       runtimeChunk: 'single'
>    >     }
>    > }
>    > ```
>    >
>    > This tells `webpack-dev-server` to serve the files from the `dist` directory on `localhost:8080` （默认端口）.
>    >
>    > The <font color=FF0000>**`optimization.runtimeChunk: 'single'` was added**</font> because <font color=FF0000>in this example we have more than one entrypoint on a single HTML page</font>. Without this, we could get into trouble described [here](https://bundlers.tooling.report/code-splitting/multi-entry/). Read the [Code Splitting](https://webpack.js.org/guides/code-splitting/) chapter for more details.
>    >
>    > `webpack-dev-server` serves bundled files from the directory defined in [`output.path`](https://webpack.js.org/configuration/output/#outputpath), i.e., files will be available under `http://[devServer.host]:[devServer.port]/[output.publicPath]/[output.filename]`.
>    >
>    > ##### 添加相关 npm script
>    >
>    > ```json
>    > "scripts": {
>    >     "start": "webpack serve --open"
>    > }
>    > ```
>    >
>    > Now we can run `npm start` from the command line and we will see <font color=FF0000>our browser automatically loading up our page</font>. If you now change any of the source files and save them, the web server will automatically reload after the code has been compiled.
>    >
>    > 摘自：[webpack 文档 - Guide - Development - Using webpack-dev-middleware](https://webpack.js.org/guides/development/#using-webpack-dev-middleware)
>
> 3. [**webpack-dev-middleware**](https://github.com/webpack/webpack-dev-middleware)
>
>    > <font color=FF0000>`webpack-dev-middleware` is a wrapper</font> that will <font color=FF0000>emit files processed by webpack to a server</font>. <font color=FF0000>**This is used in `webpack-dev-server` internally**</font>, however <font color=FF0000>it's available as a separate package to allow more custom setups if desired</font>. We'll take a look at <mark>an example that combines</mark> `webpack-dev-middleware` <mark>with an express server</mark>.
>    >
>    > 摘自：[webpack 文档 - Guide - Development - Using webpack-dev-middleware](https://webpack.js.org/guides/development/#using-webpack-dev-middleware)
>
> <font color=FF0000>In most cases, you probably would want to use `webpack-dev-server`</font> , but let's explore all of the above options.
>
> 摘自：[webpack 文档 - Guide - Development - Choosing a Development Tool](https://webpack.js.org/guides/development/#choosing-a-development-tool)

另外，相关介绍也在 [[Vue3 + TS 学习笔记#搭建本地服务器]] 以及后面的部分，中有说明



//TODO 阅读：

https://webpack.js.org/configuration/devtool/  

https://webpack.js.org/configuration/dev-server/



#### Hot Module ReplaceMent  ( HMR ) 热模块更新 

使用 webpack-dev-server 之后将不会生成 dist 目录，原因是：webpack-dev-server 虽然也会对项目进行打包，但是<font color=FF0000>打包的结果会放在内存中</font>。这是 webpack-dev-server 隐藏的特性。

热模块更新的含义是：修改代码后，重新打包，但是不会刷新页面（刷新页面会让 在代码被修改之前 在页面上操作的数据 / 样式还原）

```js
// webpack.config.js 文件下
devServer: {
  // 开启Hot Module ReplaceMent（HMR） 热模块更新 功能
  hot: true,
  // 即便是HMR的功能没有生效，也不会让浏览器自动刷新。注意：这个配置，**没必要加上**
  hotOnly: true,
}
```

<font color=FF0000> 开启 HMR 功能还需要使用 HotModuleReplacementPlugin 插件</font>，引入 HotModuleReplacementPlugin 插件的方法如下：

```js
plugins: [
  new webpack.HotModuleReplacementPlugin()
]
```

**热模块更新的作用：**

- 方便调试 CSS 代码

- 开启<font color=FF0000> 局部刷新</font>，即修改某一部分的代码，项目会自动刷新，但是只会刷新修改的部分，其他部分不会刷新。

  - CSS-Loader是内部就已经实现了局部刷新功能，所以不需要开发者做任何处理（自动）
  - React / Vue-Loader 也是内部就已经实现局部刷新功能

  **JS 开启局部刷新的方法：**

  ```js
  if(module.hot) {
    moduele.hot.accept('./number', () => {
      foo()  //	进行需要执行局部刷新的代码
    })
  }
  ```

#### HMR Guides 笔记

It（👀 **注**：即 HMR ） allows all kinds of modules to be updated at runtime without the need for a full refresh（不需要全量刷新）.

Since **`webpack-dev-server` v4.0.0** , <font color=FF0000>Hot Module Replacement is enabled by default</font> .  👀 **注**：这应该指的是 “只使用 `devServer.hot: true` 使得 HMR 生效 ”，另外，下面有手动设置： [[#manual entry points for HMR]]

> 💡 **Tip** : If you took the route of <font color=FF0000>using `webpack-dev-middleware` instead of `webpack-dev-server`</font> , please <font color=fuchsia>use the **`webpack-hot-middleware`** package to **enable HMR** on **your custom server or application**</font> .

##### manual entry points for HMR

👀 注：由于 Guides 是一个 从零到一 一步一步搭建 webpack 配置的教程，所以 Guides 的配置是有上下文的，这里也不例外；但放在摘抄中会显得很奇怪，所以这里做了一些省略。

```js
// webpack.config.js
module.exports = {
  entry: {
    app: './src/index.js',
    // Runtime code for hot module replacement
		hot: 'webpack/hot/dev-server.js',
		// Dev server client for web socket transport, hot and live reload logic
		client: 'webpack-dev-server/client/index.js?hot=true&live-reload=true',
  },
  devServer: {
    // ...
    // Dev server client for web socket transport, hot and live reload logic
		hot: false,
		client: false,
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: 'Hot Module Replacement',
    }),
    // Plugin for hot module replacement
    new webpack.HotModuleReplacementPlugin(),
  ],
  // ...
}
```

```diff
// index.js
+ if (module.hot) {
+   module.hot.accept('./print.js', function() {
+     console.log('Accepting the updated printMe module!');
+     printMe();
+   })
+ }
```

> 👀 **注**：上面 `client: 'webpack-dev-server/client/index.js?hot=true&live-reload=true'` 的用法，可以参考 [[#Loader 文档补充#内联方式]] 中内联方式的方法。
>
> 另外，上面 `module.hot.accept` 的用途，参见 [[Vue3 + TS 学习笔记#如何使用 HMR？]]

##### 通过 Node.js API 而不使用 devServer 选项

> 👀 **注**：应该可以理解为 自己写一个 server ？

When <font color=red>using Webpack Dev Server **with the Node.js API**</font> , don't put the <font color=dodgerBlue>**dev server options**</font> on the webpack configuration object. Instead , <font color=FF0000>**pass them as a second parameter upon creation**</font>（**译**：在创建时， 将其 ( dev server option ) 作为第二个参数传递）. For example:

```js
new WebpackDevServer(options, compiler)
```

To enable HMR , you also <font color=dodgerBlue>**need to modify your webpack configuration object to include the HMR entry points**</font>. Here's a small example of how that might look:

```js
// dev-server.js 注意文件名
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack');
const webpackDevServer = require('webpack-dev-server');

const config = {
  mode: 'development',
  entry: [
    // Runtime code for hot module replacement
    'webpack/hot/dev-server.js',
    // Dev server client for web socket transport, hot and live reload logic
    'webpack-dev-server/client/index.js?hot=true&live-reload=true',
    // Your entry
    './src/index.js',
  ],
  devtool: 'inline-source-map',
  plugins: [
    // Plugin for hot module replacement
    new webpack.HotModuleReplacementPlugin(),
    new HtmlWebpackPlugin({
      title: 'Hot Module Replacement',
    }),
  ],
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
    clean: true,
  },
};
const compiler = webpack(config); // 通过 webpack(config) 生成了一个 compiler 实例

// `hot` and `client` options are disabled because we added them manually
const server = new webpackDevServer({ hot: false, client: false }, compiler);

(async () => {
  await server.start();
  console.log('dev server is running');
})();
```

> 👀 **注**：上面 `'webpack-dev-server/client/index.js?hot=true&live-reload=true'` 的用法，可以参考 [[#Loader 文档补充#内联方式]] 中内联方式的方法

See the [full documentation of `webpack-dev-server` Node.js API](https://webpack.js.org/api/webpack-dev-server/).

> 💡 **Tip** : If you're [using `webpack-dev-middleware`](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) , **check out the [`webpack-hot-middleware`](https://github.com/webpack-contrib/webpack-hot-middleware) package to enable HMR** on your custom dev server.

##### 问题 ( Gotchas )

<font color=dodgerBlue>Hot Module Replacement can be **tricky**</font>（困难的，棘手的）. To show this , let's go back to our working example. If you go ahead and click the button on the example page , you will realize <font color=FF0000>the console is printing the old `printMe` function</font>.

This is happening because <font color=FF0000>the button's `onclick` event handler is **still bound to the original `printMe` function**</font>.

<font color=fuchsia>To make this work with HMR we need to update that **binding to the new `printMe` function using `module.hot.accept`**</font>

```diff
// index.js

if (module.hot) {
    module.hot.accept('./print.js', function() {
    console.log('Accepting the updated printMe module!');
+   document.body.removeChild(element);
+   element = component(); // Re-render the "component" to update the click handler
+   document.body.appendChild(element);
  })
}
```

This is only one example, but <font color=FF0000>**there are many others that can easily trip people up**</font>（绊倒）. Luckily, <font color=fuchsia>there are a lot of loaders out there (some of which are mentioned below) that will make hot module replacement much easier</font>.

> 👀 注：这部分看得有点懵，可以参考 [[Vue3 + TS 学习笔记#如何使用 HMR？]]

##### HMR with Stylesheets

Hot Module Replacement with CSS is actually fairly straightforward（👀 注：“极其简单”更好理解） <font color=FF0000>with the **help** of the `style-loader`</font> . <font color=fuchsia>This loader **uses `module.hot.accept` behind the scenes to patch `<style>` tags when CSS dependencies are updated**</font>.

> 👀 注：下面是实践的代码，安装 style-loader 和 css-loader；并在 webpack.config.js 的 `module.rules` 中配置 style-loader、css-loader，略；详见原文。

Change the style on `body` to `background: red;` and you <font color=FF0000>should immediately see the page's background color change without a full refresh</font>.

```diff
  // styles.css
  body {
-   background: blue;
+   background: red;
  }
```

##### Other Code and Frameworks

There are many other loaders and examples out in the community to make HMR interact smoothly with a variety of frameworks and libraries...

- [React Hot Loader](https://github.com/gaearon/react-hot-loader) : Tweak react components **in real time**.
- [Vue Loader](https://github.com/vuejs/vue-loader) : <font color=FF0000>This **loader** supports HMR for vue components</font> **out of the box** .
- [Elm Hot webpack Loader](https://github.com/klazuka/elm-hot-webpack-loader) : Supports HMR for the Elm programming language.
- [Angular HMR](https://github.com/gdi2290/angular-hmr) : <font color=FF0000>No loader necessary!</font> A small change to your main NgModule file is all that's required to have full control over the HMR APIs.
- [Svelte Loader](https://github.com/sveltejs/svelte-loader) : This loader supports HMR for Svelte components out of the box.

摘自：[webpack 文档 - Guides - Hot Module Replacement](https://webpack.js.org/guides/hot-module-replacement)



#### HMR Concepts

##### 在应用程序中

The **following steps** allow modules to **be swapped in and out** （置换） of an application :

- The <mark style="background: aqua">**application**</mark> <font color=FF0000>asks the HMR</font> <mark style="background: lightpink">**runtime**</mark> <font color=FF0000>to check for updates</font>.
- The <mark style="background: lightpink">**runtime**</mark> <font color=FF0000>**asynchronously**</font>（异步） <font color=FF0000>downloads the updates</font> and **notifies the <mark style="background: aqua">application</mark>**.
- The <mark style="background: aqua">**application**</mark> then <font color=FF0000>asks the <mark style="background: lightpink">**runtime**</mark> to apply the updates</font>.
- The <mark style="background: lightpink">**runtime**</mark> <font color=FF0000>**synchronously**</font>（注意：是同步） <font color=FF0000>applies the updates</font>.

You can set up HMR so that <font color=FF0000>**this process happens automatically**</font>, or you can choose to require user interaction for updates to occur.

##### 在编译器中

In addition to normal assets, <font color=FF0000>the **compiler** needs to **emit an "update"** to allow updating from the previous version to the new version</font>. The **"update" consists of two parts**:

1. The updated manifest ( JSON )
2. One or more updated chunks ( JavaScript )

The **manifest contains** the <font color=FF0000>new compilation hash</font> and <font color=FF0000>a list of all updated chunks</font>. **Each of these chunks contains the new code for all updated modules** ( <font color=FF0000>**or**</font> **a flag indicating that the module was removed**).

The compiler ensures that module IDs and chunk IDs are consistent between these builds（没搞懂是什么意思，感觉是：多次打包 ids 相同，即：幂等性？）. It typically stores these IDs in memory (e.g. with [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) ), but it's also possible to store them in a JSON file.

##### 在模块中

HMR is an <font color=FF0000>**opt-in**</font> （可选的） <font color=FF0000>**feature**</font> that <font color=FF0000>only affects modules containing HMR code</font>. One example would be <font color=FF0000>patching styling through the style-loader In order for patching to work</font>, the <font color=FF0000 size=4>**style-loader implements the HMR interface**</font>; when <font color=FF0000>it receives an update through HMR</font>, it replaces the old styles with the new ones.

Similarly, <mark>**when implementing the HMR interface in a module, you can describe what should happen when the module is updated**</mark>. However, in most cases, it's not mandatory to write HMR code in every module. <font color=FF0000 size=4>**If a module has no HMR handlers, the update bubbles up**</font>. This means that <font color=FF0000>a <font size=4>**single handler**</font> can update a <font size=4>**complete module tree**</font></font>. <font color=FF0000 size=4>**If a single module from the tree is updated, the entire set of dependencies is reloaded**</font>.

##### 在运行时环境中

For the module system runtime, additional code is emitted to track （追踪） module `parents` and `children`. On the management side, <font color=FF0000>the runtime supports two methods:</font> <font color=fuchsia>**`check`**</font> and <font color=DeepSkyBlue>**`apply`**</font>.

A <font color=fuchsia size=4>**`check`**</font> <font color=FF0000>makes an HTTP request to the update manifest</font>. If this request fails, there is no update available. <font color=FF0000>If it succeeds, **the list of updated chunks is compared to the list of currently loaded chunks**</font>. For each loaded chunk, the corresponding update chunk is downloaded. <font color=FF0000>All module updates are stored in the <font size=4>**runtime**</font></font>. <mark>When all update chunks have been **downloaded** and are **ready to be applied**</mark>, the <font color=FF0000>**runtime switches into the `ready` state**</font>.

The <font color=DeepSkyBlue size=4>**`apply`**</font> method <font color=FF0000>**flags all updated modules as invalid**</font>. <font color=FF0000>**For each invalid module**, there **needs** to be an update handler in the module or in its parent(s)</font> （对于每个 invalid module ，都需要在模块中有一个 update handler，或者在此模块的父级模块中有 update handler）. **Otherwise**, the <font color=FF0000>invalid flag bubbles up and invalidates parent(s) as well</font> （否则，会进行无效标记冒泡，并且父级也会被标记为无效）. <font color=FF0000>Each bubble continues until the app's **entry point** or **a module with an update handler is reached** (whichever comes first)</font>. <font color=FF0000 size=4>**If it bubbles up from an entry point, the process fails**</font>. 

Afterwards, all invalid modules are disposed （处理） (via the dispose handler) and unloaded. The current hash is then updated and all `accept` handlers are called. The runtime switches back to the `idle` state and everything continues as normal.

摘自：[webpack 文档 - Concept - Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/)

//TODO 阅读：

https://webpack.js.org/api/hot-module-replacement/（ HMR 除了accept 方法外还有什么方法）

https://webpack.js.org/plugins/hot-module-replacement-plugin/



#### 使用 Babel 处理 ES6 语法的代码

在 webpack 中使用 babel 需要安装 babel-loader（ babel 和 webpack 之间通信的桥梁） 、 @babel-core（核心模块）还有@babel/preset-env（作为语法转换）。另外对于 Promise 这种新的方法，还要安装 @babel/polyfill

```js
//webpack.config.js文件下
module: {
  rules: {
    test: /\.js$/,
    // 对于node_modules文件夹下的js文件不翻译，这项操作可以很好地提升打包速度；
    // 相反地，可以使用include，表示只对某些代码应用；比如：include: path.resolve(__dirname, '../src')
    exclude: /node_modules/,
    loader: "babel-loader",
    options: {
      "presets": [["@babel/preset-env", {
        // 对babel适配的浏览器做限定
        targets: {
          chrome: "67", // 只针对chrome 67以上的浏览器做适配
        }
        // 打包只添加自己使用的 polyfill，以减小打包后的文件体积；即：**按需加载**，使用 useBuiltIns
        useBuiltIns: "usage"
      }]]
    }
  }
}
```

**babel/polyfill：**用于对于一些<font color=FF0000>更低版本</font>的浏览器，提供ES6支持。在 **import "@babel/polyfill"** 时默认将所有的适配内容都放入输出的js文件中，这样会导致js文件臃肿，所以可以使用 **useBuiltIns: "usage"** （见上面）以设置放入输出 js 文件的，只有业务代码中被使用的那部分（<font color=FF0000>**按需打包**</font>）。

> 👀 注：这里的 useBuiltIns 参考 [webpack doc - guides - shimming](https://webpack.js.org/guides/shimming) 中的 [Node Built-Ins](https://webpack.js.org/guides/shimming/#further-optimizations) ，意思是 “使用内置” ( use-built-in(s) )，其中 s 表示复数

不过，babel/polyfill会通过全局变量注入方法，会污染全局环境；所以只推荐在业务代码中使用。如果写的是框架 / 库文件，则建议使用 **@babel/plugin-transform-runtime**，建议参考：https://babeljs.io/docs/en/babel-plugin-transform-runtime

如果 webpack.config.js 中babel的配置项过长，可以将babel配置项的内容放到 babel的配置文件 **.babelrc** 中。同时这些配置会自动生效，不用手动导入 webpack.config.js 中

另外，在 .babelrc 文件下，使用 useBuiltIns 且他的值为 'usage'，不需要手动引入 polyfill（比如 babel-polyfill），会自动引入 这些 polyfill

##### babel-plugin-import

[babel-plugin-import](https://github.com/umijs/babel-plugin-import) 可以被用来实现按需引入，比如 element-ui（element-plus 是官方推荐的是 antfu 的 [unplugin-vue-components](https://github.com/antfu/unplugin-vue-components) 和 [unplugin-auto-import](https://github.com/antfu/unplugin-auto-import) ）、antd、antd-mobile、lodash、material-ui



#### webpack对React项目的打包

webpack 打包 React 需要安装、使用 @babel/preset-react

```js
options: {
  "presets": [
    ["@babel/preset-env", {
    	targets: {
      	chrome: "67", //只针对chrome 67以下的浏览器
    	}
    	useBuiltIns: "usage"
  		}
    ],
    "@babel/preset-react"
  ]
}
```

同样presets调用的顺序也是从下到上，从右到左



#### Tree Shaking

对于 ES Module 引入时候，默认会将业务代码中所有ES6代码打包引入（不管这个代码块是否被调用），这样就没有做到<font color=FF0000>按需引入 / 按需打包 </font>。对于这个痛点，可以使用 <font color=FF0000>**Tree Shaking** 解决</font>。Tree Shaking <font color=FF0000>只支持</font> ES Module 代码的引入（即 import module，是 ES6 的特性；不支持 commonJS ）。代码使用如下：

```js
//webpack.config.js
module.exports = {
  ...
  // 在开发环境下加上 optimization
	optimization: {
	  usedExports: true
	}
}
```

```js
//package.json，最外层

// sideEffects中指定不使用Tree Shaking的模块
// Tree Shaking 的机制是：检查模块的导入和导出，存在导出了，而没有导入的，会被Tree Shaking掉。而这有时是不对的：
// 比如这里的*.css，之所以对所有的css文件进行不使用Tree Shaking，是因为css文件会导出任何内容，所以使用Tree Shaking容易出问题
// 如果不存在这类模块，则直接写false
{
  "sideEffects": ['*.css'],
}
```

另外：即使不使用的那些代码，在<font color=FF0000>开发环境</font>的打包中，那些不用的代码将不会被删掉，而是告知你只使用了哪些代码，便于你开发。而在生产环境的打包中，将会直接删掉那些不用的代码。

<font size=4>**补充：**</font>在生产环境的打包中，Tree Shaking 是自动生效的，即：你不需要写 `optimization.usedExports: true ` 配置项，不过 `package.json` 中的 sideEffects 还是要写的。

##### 《现代 JS 教程》中的关于 tree-shaking 的内容
> 删除未使用的导出 ( “tree-shaking” )
>
> 摘自：[现代 JS 教程 - 模块 (Module) 简介 - 构建工具](https://zh.javascript.info/modules-intro#gou-jian-gong-ju)

《现代 JS 教程》中关于 tree-shaking 的定义，相当直接易懂。

#### Tree Shaking 文档补充

> 👀 注：下面的摘抄中会出现不少的 pure，有一部分的原因是： “ *v4 beta 版时叫 `pure module` , 后来改成了 `sideEffects`* ”
>
> 学习自：[Webpack 中的 sideEffects 到底该怎么用？ - kuitos的文章 - 知乎](https://zhuanlan.zhihu.com/p/40052192)

*Tree shaking* is a term commonly used in the JavaScript context for dead-code elimination（消除）. <font color=FF0000>**It relies on the [static structure](http://exploringjs.com/es6/ch_modules.html#static-module-structure) of ES2015 module syntax , i.e. `import` and `export`**</font> . The name and concept have <font color=FF0000>**been popularized by the ES2015 module bundler [rollup](https://github.com/rollup/rollup)**</font> .

The **webpack 2** release came with built-in support for ES2015 modules (alias *harmony modules*) as well as <font color=FF0000>unused module export detection</font> . The new **webpack 4** release expands on this capability with <font color=fuchsia>a way to **provide hints to the compiler via the `"sideEffects"` `package.json` property** to denote</font>（表示） which files in your project are "pure" and therefore safe to prune（剪枝） if unused.

##### Tree Shaking 下 webpack.config.js 中的配置

```diff
  // webpack.config.js

  module.exports = {
    // ...
+   mode: 'development',
+   optimization: {
+     usedExports: true,
+   },
  }
```

但是仅仅加上这些还是不够的，没有使用的代码在打包时候，依然会被加入；所以，还要加上 sideEffects

##### 将文件标记为 side-effect-free (无副作用)

In a 100% ESM module world, identifying side effects is straightforward. However, <font color=FF0000>we aren't there quite yet</font> , so in the mean time <font color=fuchsia>it's necessary to provide hints to webpack's compiler on the "pureness" of your code</font>.

> **译**：在一个纯粹的 ESM 模块世界中，很容易识别出哪些文件有副作用。然而，我们的项目无法达到这种纯度（无法 100% 用 ESM ），所以，此时有必要提示 webpack compiler 哪些代码是“纯粹部分”。

The way this is accomplished is the `"sideEffects"` package.json property（**译**：通过 package.json 的 `"sideEffects"` 属性，来实现这种方式）.

```json
// package.json
{
  "name": "your-project",
  "sideEffects": false
}
```

All the code noted above does not contain **side effects** , so <font color=FF0000>we can **mark the property as `false`**</font> <font color=fuchsia>**to inform webpack that it can safely prune unused exports**</font> .

> **译**：如果所有代码都不包含副作用，我们就可以简单地将该属性标记为 `false` ，告知 webpack 它可以安全地删除未用到的 export

> 💡 **Tip** : A "side effect" is defined as code that performs a special behavior when imported, other than exposing one or more exports. An example of this are polyfills, which affect the global scope and usually do not provide an export.

<font color=FF0000>If your code did have some side effects</font> though , an array can be provided instead :

```json
{
  "name": "your-project",
  "sideEffects": ["./src/some-side-effectful-file.js"]
}
```

The array accepts simple glob patterns（简单的全局模式） to the relevant files . <font color=FF0000>It uses [glob-to-regexp](https://github.com/fitzgen/glob-to-regexp) under the hood</font>（**译**：在引擎盖下，即：在内部实现中...） <font color=fuchsia>**( Supports : `*` , `**` , `{a,b}` , `[a-z]` )**</font> . <font color=FF0000>Patterns like `*.css` , which do not include a `/` , will be treated like `**/*.css`</font> . 

> 👀 **注**：`*` 和 `**` 都是 *nix 下通用的 通配符，其中 `**` 可以简单理解为 “深度搜索”。具体看下面的摘抄：
>
> > It's almost the same as the single asterisk but may consist of *multiple* directory levels.
> >
> > In other words, while `/x/*/y` will match entries like:
> >
> > ```none
> > /x/a/y
> > /x/b/y
> > ```
> >
> > and so on ( with only one directory level in the wildcard section ) , the double asterisk `/x/**/y` will *also* match things like:
> >
> > ```none
> > /x/any/number/of/levels/y
> > ```
> >
> > with the concept of "any number of levels" also including zero (in other words, `/x/**/y` will match `/x/y` as one of its choices).
> >
> > 摘自：https://stackoverflow.com/a/32604736/13496313
>
> 这种设计不仅仅是 webpack 所独有的，grunt、gulp 中也有（参考 https://stackoverflow.com/a/32604753/13496313 ）。这种设计就是来自于 *nix，参考 [What do double-asterisk (**) wildcards mean?](https://stackoverflow.com/questions/28176590/what-do-double-asterisk-wildcards-mean)  比如：在命令行中 想要搜索 “当前目录（嵌套）下所有的 `.jpg` 文件”，可以通过 `find **/*.jpg` 命令进行查找。

> 💡 **Tip** : Note that <font color=FF0000>any imported file is subject to tree shaking</font> . This means <font color=fuchsia>if you use something like `css-loader` in your project and import a CSS file , **it needs to be added to the side effect list**</font> so it will not be unintentionally dropped in production mode（**译**：以免在生产模式中无意中将它删除 ） :

```json
{
  "name": "your-project",
  "sideEffects": ["./src/some-side-effectful-file.js", "*.css"]
}
```

Finally , `"sideEffects"` can <font color=FF0000>**also** be set</font> from the [`module.rules` configuration option](https://webpack.js.org/configuration/module/#modulerules).

#####  `usedExports` ( Tree Shaking ) 和 `sideEffects` 的区别

The <font color=dodgerBlue>**`sideEffects` and `usedExports`**</font> ( more known as tree shaking ) <font color=dodgerBlue>**optimizations are two different things**</font> .

<font color=fuchsia>**`sideEffects` is much more effective**</font> since <font color=FF0000>it allows to skip whole modules/files and the complete subtree</font> . 👀 **注**：感觉因为 `sideEffects` 是开发者配置的，所以只要匹配开发者的配置，则直接跳过；而 `usedExports` 是由 webpack 运行决定。

<font color=fuchsia>**`usedExports` relies on [terser](https://github.com/terser-js/terser)**</font>（👀 见下面的“注”） <font color=fuchsia>**to detect side effects in statements**</font> . It is a <font color=FF0000>difficult task in JavaScript</font> and <font color=FF0000>**not as effective as straightforward `sideEffects` flag**</font> . <font color=fuchsia>It also **can't skip subtree/<font size=4>dependencies</font>**</font> since the spec（规范） says that side effects need to be evaluated. While exporting function works fine , React's Higher Order Components ( HOC ) are problematic in this regard（**译**：尽管导出函数能运作如常，但 React 框架的高阶组件在这种情况下是会出问题的）.

> 👀 注：terser 是一种 JS Parser。另外，terser 以及基于它的 terser-webpack-plugin 已经集成到 webpack 中，分别可以在  webpack@5 的 package.json `devDependencies` 和 `dependencies` 中找到

> 👀 注：这里接下来举了个 React 的例子，挺长，且和概念没有太强的关联；再加上当前我对 React 不太了解... 这里略；详见原文。

Terser actually tries to figure it（ it 是例子中的问题）out, but it doesn't know for sure in many cases. This doesn't mean that terser is not doing its job well because it can't figure it out. <font color=fuchsia>It's too difficult to determine it reliably in a dynamic language like JavaScript</font>.

But <font color=dodgerBlue>**we can help terser by using the <font size=4>`/*#__PURE__*/`</font> annotation**</font>. <font color=fuchsia>**It flags a statement as side-effect-free**</font>. So a small change would make it possible to tree-shake the code :

```javascript
var Button$1 = /*#__PURE__*/ withAppProvider()(Button);
```

<font color=FF0000>This would allow to remove this piece of code</font> . But there are still questions with the imports which need to be included/evaluated because they could contain side effects（**译**：但仍然会有一些引入的问题，需要对其进行评估，因为它们产生了副作用）.

<font color=dodgerBlue>**To tackle（解决） this , we use the [`"sideEffects"`](https://webpack.js.org/guides/tree-shaking/#mark-the-file-as-side-effect-free) property in `package.json`**</font> .

It's <font color=FF0000>**similar to `/*#__PURE__*/`**</font> but on a module level instead of a statement level（**译**：作用于模块的层面，而不是代码语句的层面（这是在说 `/*#__PURE__*/` 这种））. <font color=dodgerBlue>**It says ( `"sideEffects"` property )**</font> : "<mark>If no direct export from a module flagged with no-sideEffects is used</mark>（**译**：如果被标记为无副作用的模块没有被直接导出使用）, <mark>the bundler can **skip evaluating the module for side effects**</mark>." .

##### Mark a function call as side-effect-free

It is possible to tell webpack that a function call is side-effect-free ( pure ) by using the `/*#__PURE__*/` annotation. <font color=FF0000>It **can be put in front of**</font> <font color=fuchsia size=4>**function calls**</font>（👀 注：这里是**函数调用**，下面的示例也是这样用的） <font color=FF0000>to mark them as side-effect-free</font>. Arguments passed to the function are not being marked by the annotation and may need to be marked individually. <font color=FF0000>When the initial value in a variable declaration of an unused variable is considered as side-effect-free ( pure ) , it is getting marked as dead code, not executed and dropped by the minimizer</font>. <font color=fuchsia>**This behavior is enabled when [`optimization.innerGraph`](https://webpack.js.org/configuration/optimization/#optimizationinnergraph) is set to `true`**</font> .

```js
// file.js
/*#__PURE__*/ double(55);
```

##### Minify the Output

So we've cued up our "dead code" to be dropped by using the `import` and `export` syntax, <font color=FF0000>but we **still need to drop it from the bundle**</font>. To do that , set the `mode` configuration option to `production`.

```js
// webpack.config.js
module.exports = {
  mode: 'production'
}
```

> 💡 **Tip** : Note that the `--optimize-minimize` flag（命令行 `--optimize-minimize` 标记） can be used to enable `TerserPlugin` as well.
>
> 💡 **Tip** : <font color=fuchsia size=4>**`ModuleConcatenationPlugin` is needed for the tree shaking to work**</font>. It is added by `mode: 'production'` （👀 注：在生产模式下，ModuleConcatenationPlugin 默认自动被引入 ）. <font color=FF0000>If you are not using it</font> （👀 注：没有用生产模式）, <font color=FF0000>**remember to add the [`ModuleConcatenationPlugin`](https://webpack.js.org/plugins/module-concatenation-plugin/) manually**</font> .

##### 总结

What we've learned is that in order to take advantage of *tree shaking* , <font color=dodgerBlue>you must</font>...

- <font color=FF0000>Use ES2015 module syntax</font> ( i.e. `import` and `export` ).
- <font color=FF0000>**Ensure no compilers transform your ES2015 module syntax into CommonJS modules**</font> (this is the default behavior of the popular Babel preset @babel/preset-env - see the [documentation](https://babeljs.io/docs/en/babel-preset-env#modules) for more details).
- <font color=FF0000>Add a `"sideEffects"` property</font> to your project's `package.json` file.
- <font color=fuchsia>**Use the `production` `mode`**</font> configuration option to enable [various optimizations](https://webpack.js.org/configuration/mode/#usage) including minification and tree shaking.

摘自：[webpack 文档 - Guide - Tree Shaking](https://webpack.js.org/guides/tree-shaking/)

##### 文章《 Webpack 中的 sideEffects 到底该怎么用？》中的补充

其实 webpack 里的 `sideEffects: false` 的意思并不是我这个模块真的没有副作用，而只是为了在摇树时告诉 webpack：**我这个包在设计的时候就是期望没有副作用的，即使他打完包后是有副作用的，webpack 同学你摇树时放心的当成无副作用包摇就好啦！**

也就是说，<font color=fuchsia>**只要你的包不是用来做 polyfill 或 shim 之类的事情，就尽管放心的给他加上 `sideEffects: false` 吧！**</font>

摘自：[Webpack 中的 sideEffects 到底该怎么用？ - kuitos的文章 - 知乎](https://zhuanlan.zhihu.com/p/40052192)

上面提到了 ModuleConcatenationPlugin，便也看了下文档。

##### ModuleConcatenationPlugin

In the past, one of webpack’s trade-offs when bundling was that <font color=dodgerBlue>**each module in your bundle would be wrapped in individual function closures**</font>. <font color=red>These wrapper functions made it slower for your JavaScript to execute in the browser</font>. In comparison , <font color=dodgerBlue>tools like Closure Compiler and RollupJS **‘hoist’ or concatenate the scope of all your modules into one closure**</font> and <font color=red>**allow for your code to have a faster execution time in the browser**</font> .

<font color=dodgerBlue>This plugin will enable the same concatenation behavior in webpack</font>. <font color=fuchsia>By default this plugin is already enabled in production `mode` and disabled otherwise</font>. If you need to override the production `mode` optimization, set the [`optimization.concatenateModules` option](https://webpack.js.org/configuration/optimization/#optimizationconcatenatemodules) to `false`. <mark>To enable concatenation behavior in other modes</mark> , you can add `ModuleConcatenationPlugin` manually or use the `optimization.concatenateModules` option :

```js
new webpack.optimize.ModuleConcatenationPlugin();
```

> 💡 <font color=fuchsia>This concatenation behavior is called **“scope hoisting”** .</font>
>
> Scope hoisting is specifically a feature made possible by ECMAScript Module syntax. Because of this webpack may fallback to normal bundling based on what kind of modules you are using, and other conditions.

> ⚠️ **Warning** : Keep in mind that this plugin will only be applied to [ES6 modules](https://webpack.js.org/api/module-methods/#es6-recommended) processed directly by webpack. When using a transpiler, you'll need to disable module processing (e.g. the [`modules`](https://babeljs.io/docs/en/babel-preset-env#modules) option in Babel).

👀 注：文档下面还有一些内容，没怎么看懂...略。

摘自：[webpack doc - Plugins - ModuleConcatenationPlugin](https://webpack.js.org/plugins/module-concatenation-plugin/)




#### Development 模式 和 Production 模式的区分打包

##### development 模式 和 production 模式的部分区别

- 在development模式下，sourceMap是非常全的，可以快速定位代码的问题；而production模式下，sourceMap会简洁很多（没有development环境下那么重要了）。
- 开发环境下，代码不需要做压缩；而生产环境中，代码需要被压缩

由于 Development 和 Production 是两种模式，所以不建议它们共享一个 webpack.config.js，在打包时还需要分别修改配置，容易造成遗漏和BUG，所以建议两种模式分别使用两个不同的配置文件，比如：webpack.dev.conf.js 和 webpack.prod.conf.js

由于之前都在说development模式下的配置，这里说一下改成 production 模式需要改哪些地方：

```js
// webpack.prod.js
module.exports = {
  // mode 要改成production
  mode: 'production',
  // devtool 取值中去掉 eval
  devtool: 'cheap-module-source-map',
  // devServer 直接去掉，因为不需要手动生成服务器了。
  
  // html插件中的HotModuleReplacementPlugin，可以去掉了
  
  //由于生产环境自动Tree Shaking，所以 optimization 可以去掉了
}
```

相对应的需要在 package.json 分别配置 dev 环境和 prod 环境的 npm scripts，使用 --config 进行切换不同的配置文件

```json
"scripts": {
  // 后面的--config webpack.dev.conf.js表示选择哪个配置文件
  "dev": "webpack-dev-server --config webpack.dev.conf.js",
	// build是指打包上线的代码，即production
  "build": "webpack --config webpack.prod.conf.js ",
}
```

<font color=FF0000> **由于 webpack.dev.conf.js 和 webpack.prod.conf.js 中存在很多相同的配置，所以，可以添加一个 webpack.common.conf.js 用于存放公共配置**</font>。同时，<font color=FF0000> 想要合并两个配置文件，还需要安装插件：webpack-merge</font>；且 dev / prod的配置中也需要进行修改。

以 webpack.dev.conf.js 为例：

```js
const merge = require('webpack-merge')
const commonConfig = require('./webpack.common.conf.js')

// 以development环境的配置，作为变量
const devConfig = {
  // ...
}

module.exports = merge(commonConfig, devConfig)
```

由于：配置文件被放到 build 文件夹下，所以：输出文件 ( output ) 的路径 也要进行改动；同时，如果使用了 CleanWebpackPlugin ，路径同样要修改，不过，由于 CleanWebpackPlugin 默认当前文件 ( webpack.dev.config.js ) 的所在的文件夹是根目录，所以不能直接修改路径，如下：

```js
new CleanWebpackPlugin(['../dist'])
```

这会让 webpack 到根路径下去找。所以需要另外配置：

```js
plugins: [
  new CleanWebpackPlugin(['/dist'], {
    // 设置根目录的路径，是当前文件夹的上一层级
    root: path.resolve(__dirname, '../')
  })
]
```

另外，<font color=FF0000> 可以**新建一个build文件夹**</font>，<mark>将 webpack.dev.conf.js 、webpack.prod.conf.js 以及 webpack.common.conf.js 三个配置文件放入其中</mark>。不过这时 package.json 需要修改对应的配置文件的路径

```json
"scripts": {
  "dev": "webpack-dev-server --config ./build/webpack.dev.conf.js", //后面的--config webpack.dev.conf.js表示选择哪个配置文件
  "build": "webpack --config ./build/webpack.prod.conf.js ", //build是指打包上线的代码，即production
}
```



#### Production 文档补充

<font color=dodgerBlue>The **goals** of *development* and *production* builds **differ greatly**</font>. **In *development***, we <font color=red>want strong source mapping and a localhost server with live reloading or hot module replacement</font>. **In *production***, <font color=fuchsia>our goals shift to a focus on minified bundles, lighter weight source maps, and optimized assets to improve load time</font>. With this logical separation at hand, we typically <font color=dodgerBlue>recommend writing **separate webpack configurations** for each environment</font>.

While we will separate the *production* and *development* specific bits out , note that <font color=red>we'll still maintain a "common"</font> （公共的）<font color=red>configuration to keep things DRY</font>（Don't Repeat Yourself，即：不重复（原则）） . In order to merge these configurations together, we'll use a utility called [`webpack-merge`](https://github.com/survivejs/webpack-merge) . With the "common" configuration in place, we won't have to duplicate code within the environment-specific configurations.

Let's start by installing `webpack-merge` and splitting out the bits we've already worked on in previous guides:

```bash
npm install --save-dev webpack-merge
```

##### 文件以及配置更改

```diff
- |- webpack.config.js
+ |- webpack.common.js
+ |- webpack.dev.js
+ |- webpack.prod.js
```

```js
// webpack.common.js
module.exports = {
  entry: { app: './src/index.js', },
  plugins: [
    new HtmlWebpackPlugin({ title: 'Production', }),
  ],
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
    clean: true,
  },
};
```

```js
// webpack.dev.js
const { merge } = require('webpack-merge');
const common = require('./webpack.common.js');

module.exports = merge(common, {
  mode: 'development',
  devtool: 'inline-source-map',
  devServer: {
    static: './dist',
  },
});
```

```js
// webpack.prod.js
const { merge } = require('webpack-merge');
const common = require('./webpack.common.js');

module.exports = merge(common, {
  mode: 'production',
});
```

In `webpack.common.js` , we now have setup our `entry` and `output` configuration and we've included any plugins that are required for both environments. In `webpack.dev.js` , we've set `mode` to `development`. Also, we've added the recommended `devtool` for that environment (strong source mapping) , as well as our `devServer` configuration. Finally, <font color=fuchsia>in `webpack.prod.js` , `mode` is set to `production` which loads `TerserPlugin`</font> （👀 注：就是 terser-webpack-plugin ）, <font color=fuchsia>which was first introduced by the [tree shaking](https://webpack.js.org/guides/tree-shaking/) guide</font> .

##### 对应的 NPM Scripts 修改

Now, let's modify our npm scripts to use the new configuration files. For the `start` script, which runs `webpack-dev-server`, we will use `webpack.dev.js`, and for the `build` script, which runs `webpack` to create a production build, we will use `webpack.prod.js` :

```diff
// package.json
  {
    "scripts": {
-     "start": "webpack serve --open",
+     "start": "webpack serve --open --config webpack.dev.js",
-     "build": "webpack"
+     "build": "webpack --config webpack.prod.js"
    },
  }
```

##### 指定 Mode

<font color=dodgerblue>Many libraries will key off</font>（👀 注：这里的意思是 “关联”） <font color=dodgerblue>the **`process.env.NODE_ENV`** variable to **determine what should be included in the library**</font> . For example, <font color=red>when `process.env.NODE_ENV` **is not set to `'production'`** some libraries may **add additional logging** and testing to make debugging easier</font>. However, with <font color=fuchsia>`process.env.NODE_ENV` set to `'production'` they might drop or add significant portions of code to optimize how things run for your actual users</font>. **Since webpack v4, specifying `mode`**（通过下面指定 mode 的方式） **automatically configures [`DefinePlugin`](https://webpack.js.org/plugins/define-plugin) for you**：

```js
// webpack.prod.js
module.exports = merge(common, {
   mode: 'production',
 });
```

> 💡 **Tip** : Technically , <font color=red>`NODE_ENV` is a system environment variable that Node.js exposes into running scripts</font>. It is used by convention to determine dev-vs-prod behavior by server tools , build scripts, and client-side libraries. <font color=fuchsia>Contrary to expectations , `process.env.NODE_ENV` is not set to `'production'` **within** the build script `webpack.config.js`</font> , see [#2537](https://github.com/webpack/webpack/issues/2537) . Thus, conditionals like `process.env.NODE_ENV === 'production' ? '[name].[contenthash].bundle.js' : '[name].bundle.js'`  within webpack configurations do not work as expected.

If you're using a library like `react` , <font color=red>you should actually see a significant drop in bundle size after adding `DefinePlugin`</font> . Also, note that any of our local `/src` code can key off（关联） of this as well, so the following check would be valid :

```diff
// src/index.js

+ if (process.env.NODE_ENV !== 'production') {
+   console.log('Looks like we are in development mode!');
+ }
```

##### 压缩 ( Minification )

Webpack v4+ will minify your code by default in production mode.

Note that <font color=dodgerBlue>**while**</font> the `TerserPlugin` is a great place to start for minification and being used by default, <font color=fuchsia>there are other options out there</font> : [`ClosureWebpackPlugin`](https://github.com/webpack-contrib/closure-webpack-plugin)

<font color=dodgerBlue>If you decide to try another minification plugin</font>, make sure your new choice also drops dead code as described in the [tree shaking](https://webpack.js.org/guides/tree-shaking) guide and provide it as the `optimization.minimizer`.

##### 源码映射 ( Source Mapping )

<font color=red>We encourage you to have source maps enabled in production</font>, as <font color=fuchsia>they are useful for debugging as well as running benchmark tests</font>. That said, you should choose one with a fairly quick build speed that's recommended for production use ( see [`devtool`](https://webpack.js.org/configuration/devtool) ). For this guide, we'll use the `source-map` option in the *production* as opposed to the `inline-source-map` we used in the *development* :

```diff
  module.exports = merge(common, {
    mode: 'production',
+   devtool: 'source-map',
  });
```

> 💡 **Tip**：<font color=red>Avoid `inline-***` and `eval-***` use in production</font> as they can <font color=red>increase bundle size</font> and <font color=red>reduce the overall performance</font> .

##### 压缩 CSS ( Minimize CSS )

It is crucial to minimize your CSS for production. Please see the [Minimizing for Production](https://webpack.js.org/plugins/mini-css-extract-plugin/#minimizing-for-production) section.

##### 命令行选项 ( CLI Alternatives )

<font color=dodgerBlue>Many of the options described above can be set as command line arguments</font>. For example , <font color=red>`optimization.minimize` can be set with `--optimization-minimize`</font> , and <font color=fuchsia>`mode` can be set with `--mode`</font> . Run <font color=red>`npx webpack --help=verbose` **for a full list of CLI arguments**</font>.  👀 **注**：运行该命令需要安装 webpack 和 webpack-cli 作为依赖。另外，https://webpack.js.org/api/cli/ 也有一样的内容

<font color=dodgerBlue>**While**</font> these shorthand（速记） methods are useful , <font color=fuchsia>we recommend setting these options in a webpack configuration file for **more configurability**</font>.

摘自：[webpack doc - Guides - Production](https://webpack.js.org/guides/production/)



#### Code Splitting 代码分割

为了看见打包生成的内容，我们不能使用 webpack-dev-server 了，这时我们可以使用一个新的 npm scripts：

```json
"scripts": {
  "dev-build": "webpack --config ./build/webpack.dev.conf.js"
}
```

**代码分割解决的问题：**

- 如果没有代码分割，所有的js文件将会被打包成一个文件。首先是文件很大，加载需要花费很长时间；而<font color=FF0000> 浏览器可以并行加载资源</font>，<font color=FF0000> 如果进行代码分割，相对而言，加载多个文件的速度 要比只加载一个大的文件要快</font>。

- <font color=FF0000> 打包的代码分为依赖代码（代码库）和业务代码</font>，我们开发几乎只需要动业务代码。而且<font color=FF0000> 浏览器是有缓存的</font>，当页面上业务代码的逻辑上发生变化时，只需要加载改动的业务逻辑代码即可。

代码分割的功能在 webpack 出现之前就已经存在，但是 webpack 的代码分割功能让其智能化，只需要配置即可。

配置代码分割的 代码如下：

```js
// webpack.common.conf.js下
optimization: {
  splitChunks: {
    // 表示无论是同步还是异步的代码都会进行代码分割（详见下面的更多介绍）
    chunks: 'all'
  }
}
```

webpack对于同步性质代码的打包时：会对代码进行分析，把该提取出来的文件（比如某个库）提取出来，进行单独的存放，自动进行分割。对于异步加载的代码（比如使用 Ajax 获取的代码），会单独拿出来，同样进行分割。

对于异步加载的代码，在打包时 webpack 会根据代码分割产生的 id 的值，来作为异步加载模块打包后的文件名。这时候可以使用 magic comment 魔法注释，在 `import( asyncCode )` 的 import 中使用 `/* webpackChunkName: "you_ordered_name" */`，即：

```js
import( /*webpackChunkName:"you_ordered_name"*/ asyncCode)
```

此时，名字会变成 <font color=FF0000> vendors~</font>you_ordered_name，如果想要去掉 vendors~，可以添加配置 cacheGroups ，如下：

```js
// webpack.common.conf.js下
optimization: {
  splitChunks: {
    chunks: 'all',
    cacheGroups: {
      vendors: false,
      default: false
    }
  }
}
```

##### webpackChunkName 的补充

> ##### Chunks
>
> Chunks come in two forms:
>
> - <font color=fuchsia>**`initial`**</font> is the <font color=FF0000>main chunk for the entry point</font>. This chunk contains all the modules and their dependencies that you specify for an entry point.
> - <font color=fuchsia>**`non-initial`**</font> is a chunk that <font color=FF0000>**may be lazy-loaded**</font>. It may appear when [dynamic import](https://webpack.js.org/guides/code-splitting/#dynamic-imports) or [SplitChunksPlugin](https://webpack.js.org/plugins/split-chunks-plugin/) is being used.
>
> <font color=FF0000>**By default**</font>, <font color=FF0000>**there is no name for `non-initial` chunks** so that a unique ID is used instead of a name</font>（根据原文下面 output 部分的内容：这里的 ID 应该就是 `[id]` 占位符？ ）. When using dynamic import we <font color=FF0000>may specify a chunk name explicitly by using a **"magic" comment**</font>
>
> ```react
> import(/* webpackChunkName: "app" */ './app.jsx').then(
>   (App) => { ReactDOM.render(<App />, root); }
> );
> ```
>
> 摘自：[webpack 文档 - concept - Under The Hood - Chunks](https://webpack.js.org/concepts/under-the-hood/#chunks)

##### splitchunks 的配置项

splitChunks有很多配置项，默认的配置项（即 即使splitChunk对象为空，也会生效），如下：

```js
module.exports = {
  //...
  optimization: {
    splitChunks: {
      // 配置为 async 表示：做代码分割时，只对异步代码生效，即：只对异步代码进行代码分割；
      // 也可以选择 all，表示对 异步 和 同步 的代码都进行分割；但是仅仅只是用all 并不能生效，
      // 还要配置 cacheGroups，如下面 cacheGroups.defaultVendors的配置。
      // 如果 配置为 all，则不会去看下面的 cacheGroups.defaultVendors的配置
      chunks: 'async',
      // 设置引入 模块或包 的大小，大于多少个字节，则做代码分割。
      // 另外，满足了minSize，可要判断是否满足下面的cacheGroups中的判断
      // 比如下面 defaultVendors要是代码从node_modules中引入的，而如果引入的代码小于minSize，
      // 但不是node_modules中来的，则还是无法进行代码分割
      minSize: 20000,
      // 在 webpack 5 中引入了 splitChunks.minRemainingSize 选项，
      // 通过 **确保拆分后剩余的最小 chunk 体积超过限制来避免大小为零的模块**。 
      // 'development' 模式 中默认为 0。对于其他情况，splitChunks.minRemainingSize 默认为 splitChunks.minSize 的值，
      // 因此除需要深度控制的极少数情况外，不需要手动指定它。
      minRemainingSize: 0,
      // 当一个代码，被使用了多少次时，我们对它进行代码分割。
      // 这里 “被使用” 的定义是：一个代码被一个 **chunk** 使用了，则被称为使用一次。
      // 这里的配置是 minChunks，默认为 1。即哪怕只被一个chunk使用了，也会进行代码分割
      minChunks: 1,
      // 加载网页时，最多可以同时加载的模块数。如果超过，则不会进行代码分割
      maxAsyncRequests: 30,
      // 网站首页 / 入口文件进行加载时，最多可以加载的文件数量，如果超过，则不会进行代码分割
      maxInitialRequests: 30,
      enforceSizeThreshold: 50000,
      // 缓存组。设置打包后分割的代码，会被放到哪里。
      cacheGroups: {
        // 默认组。默认分割的代码是从 node_modules 来的
        defaultVendors: {
          // 判断是从哪里引入的。如果符合test的要求，则进行下面的策略。
          // 这里是通过 npm 安装的、放在 node_modules 中代码，则进行代码分割
          test: /[\\/]node_modules[\\/]/,
          // 设置如果同一个文件可以满足两个组，则被哪个组设置的优先级选项。priority越大优先级越高，则同时满足的文件被其处理。
          priority: -10,
          // 如果可以模块已经被打包过了，再打包时该如何处理。设置为true，则忽略。
          reuseExistingChunk: true,
        },
        // 对于不满足 defaultVendors 要求的文件的代码分割进行设置，这里同样可以设置filename
        // 当前的这个default 组没有test，即：所有不满足defaultVendors的文件，都可以满足 default
        default: {
          // 在splitChunks中有，见上
          minChunks: 2,
          priority: -20, // 见上
          reuseExistingChunk: true, // 见上
        },
      },
    },
  },
};
```

这样生成的代码分割文件名为：vendors~entryFileName.js，其中vendors表示符合 defaultVendors组的要求，所以以vendors 为前缀；entryFileName 表示这段代码的入口文件为entryFileName.js（这里入口文件的配置需要webpack.common.conf.js 中的 entry 去配置）。

如何想要修改代码分割后的文件名，可以使用 filename 参数；比如这里的 vendors.js ：

```js
// 默认组
defaultVendors: {
  // ...
  filename: 'vendors.js'
}
```

splitChunks的其他的配置（即：非默认配置）

- **maxSize：**对代码分割的代码，判断其大小（单位为字节），是否有必要做二次分割。依据就是 maxSize，如果大于 maxSize 则进行二次代码分割。

  不过，对于 lodash库之类的库，即使其大小为 1MB左右（可能会大于 maxSize），但还是 <font color=FF0000 size=4> **有可能**</font> 无法被分割

- **automaticNameDelimiter：**代码分割生成文件时，文件名中间的分割符。默认值是 '\~'，比如生成的文件名为：vendors\~main.js

- **name：**拆分 chunk 的名称。默认值为 false，设为 false 将保持 chunk 的相同名称，因此不会不必要地更改名称。这是生产环境下构建的建议值。



**Chunk：**在打包的代码分割之后，生成的每一个块都是一个chunk

**chunkFileName**

```js
// webpack.common.conf.js
output: {
  // entry 中定义的入口文件会直接与 filename 相关，会影响 filename，作为打包后的入口文件
  filename: '[name].js'
  // 异步加载、间接生成的文件，使用chunkFileName 作为命名方式 
  chunkFileName: '[name].chunk.js'
}
```

#### Code Splitting 补充

##### 总述

<mark>Code splitting is **one of the most compelling** （引人入胜的）**features** of webpack</mark>. <font color=FF0000>This feature **allows you to split your code into various bundles** which can then **be loaded on demand or in parallel**</font> （按需加载 和 并行加载）. It can be used to achieve smaller bundles and <font color=FF0000>**control resource load prioritization**</font>（控制资源加载优先级）which, <mark>if used correctly, can have a major impact on load time</mark>.

**There are <font color=FF0000>three general approaches</font> to code splitting <font color=FF0000>available</font>:**

- **Entry Points**: Manually split code using [`entry`](https://webpack.js.org/configuration/entry-context) configuration. 
- **Prevent Duplication**: Use [Entry dependencies](https://webpack.js.org/configuration/entry-context/#dependencies) or [`SplitChunksPlugin`](https://webpack.js.org/plugins/split-chunks-plugin/) to dedupe （去重） and split chunks.
- **Dynamic Imports**: Split code via inline function calls within modules.

##### Entry Points

This is <font color=FF0000>by far the easiest and most intuitive way to split code</font>. However, it is more manual and has some pitfalls（陷阱） we will go over.

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.js',
    another: './src/another-module.js',
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```

**As mentioned there are some pitfalls to this approach:**

- If there are any <font color=FF0000>duplicated modules between entry chunks</font>, they <font color=FF0000>will be included in both bundles</font>.

  如上示例中 （代码略，详见原链接）：index.js 和 another-module.js 中都引入了 `lodash` ，那么，lodash 会引入两次；这显然是不应该的。解决办法见下面，即：使用 dependOn，写成：`dependOn: youDefinedSharedChunk`

- It isn't as flexible and <font color=FF0000>can't be used to dynamically split code **with the core application logic**</font>（不能动态地将核心应用程序逻辑中的代码拆分出来）.

##### Prevent Duplication

- **Entry dependencies**

  The [`dependOn` option](https://webpack.js.org/configuration/entry-context/#dependencies) allows to share the modules between the chunks:

  ```js
  const path = require('path');
  
  module.exports = {
    mode: 'development',
    entry: {
      index: {
        import: './src/index.js',
        dependOn: 'shared', // dependOn 指向了下面定义的 shared: 'lodash'
      },
      another: {
        import: './src/another-module.js',
        dependOn: 'shared', // dependOn 指向了下面定义的 shared: 'lodash'
      },
      shared: 'lodash', // **注意这里**，定义 shared: 'lodash' 处
    },
    output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist'),
    },
    optimization: {
      runtimeChunk: 'single', // 多入口打包引入当个 html 页面
    },
  };
  ```

  如上配置，打包之后的结果：除了 index.bundle.js、another.bundle.js 和 <font color=FF0000>**shared.bundle.js**</font> ，<font color=FF0000>**还会有 runtime.bundle.js**</font>

  As you can see there's another `runtime.bundle.js` file generated besides `shared.bundle.js`, `index.bundle.js` and `another.bundle.js`.

  <mark>Although using multiple entry points per page is allowed in webpack</mark>, it <font color=FF0000>should **be avoided** when possible in favor of an **entry point with multiple imports**</font>: `entry: { page: ['./analytics', './app'] }`. This <font color=FF0000>**results in** a better optimization and consistent execution order when using `async` script tags</font>.

- **SplitChunksPlugin**

  The [**`SplitChunksPlugin`**](https://webpack.js.org/plugins/split-chunks-plugin/) <font color=FF0000>allows us to extract common dependencies into an existing entry chunk </font>（已有的入口 chunk ）<font color=FF0000>or an entirely new chunk</font>. Let's use this to de-duplicate the `lodash` dependency from the previous example:

  ```diff
    const path = require('path');
  
    module.exports = {
      mode: 'development',
      entry: {
        index: './src/index.js',
        another: './src/another-module.js',
      },
      output: {
        filename: '[name].bundle.js',
        path: path.resolve(__dirname, 'dist'),
      },
  +   optimization: {
  +     splitChunks: {
  +       chunks: 'all',
  +     },
  +   },
    };
  ```

  With the [`optimization.splitChunks`](https://webpack.js.org/plugins/split-chunks-plugin/#optimizationsplitchunks) configuration option in place, we should <font color=FF0000>now see **the duplicate dependency removed from our `index.bundle.js` and `another.bundle.js`**</font> （ **译：**`index.bundle.js` 和 `another.bundle.js` 中 <font color=FF0000>**已经移除了重复的依赖模块**</font>）. The plugin should notice that we've separated `lodash` out to a separate chunk and remove the dead weight from our main bundle（**译：** 需要注意的是，插件将 `lodash` 分离到单独的 chunk，并且将其从 main bundle 中移除，减轻了大小）.

另外，还可以使用 [`mini-css-extract-plugin`](https://webpack.js.org/plugins/mini-css-extract-plugin)，Useful for <font color=FF0000>splitting CSS out from the main application</font>.

##### Dynamic Imports

<mark>**Two similar techniques** are supported by webpack when it comes to dynamic code splitting</mark>. **The first and recommended approach** is to <font color=FF0000>use the `import()` syntax</font> that conforms（符合） to the [ECMAScript proposal](https://github.com/tc39/proposal-dynamic-import) for dynamic imports. The legacy, webpack-specific approach is to use [`require.ensure`](https://webpack.js.org/api/module-methods/#requireensure) （**译：**第二种，则是 webpack 的遗留功能，使用 webpack 特定的 require.ensure）. 

>  ⚠️ **Warning**：<font color=FF0000>**`import()` calls use [promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) internally**</font>. If you use `import()` with older browsers (e.g., IE 11), remember to shim `Promise` using a polyfill such as [es6-promise](https://github.com/stefanpenner/es6-promise) or [promise-polyfill](https://github.com/taylorhakes/promise-polyfill).

##### Prefetching / Preloading modules

<mark style="background: aqua">Webpack 4.6.0+ adds support for **prefetching** and **preloading**.</mark>

Using these inline directives while declaring your imports allows webpack to output（输出） “Resource Hint” which tells the browser that for:

- **prefetch**: resource is probably needed for some <font color=FF0000>navigation **in the future**</font>
- **preload**: resource will also be needed <font color=FF0000>during the **current** navigation</font>

An example of this is having a `HomePage` component, which renders a `LoginButton` component which then <font color=FF0000>on demand loads a `LoginModal` component after being clicked</font>.

```js
/* LoginButton.js */

//...
import(/* webpackPrefetch: true */ './path/to/LoginModal.js');
```

This <font color=FF0000>will **result in `<link rel="prefetch" href="login-modal-chunk.js">` being appended in the head of the page**</font>, which will <font color=FF0000>instruct the browser to prefetch in idle time the `login-modal-chunk.js` file</font>.

> 💡 **Tip**: webpack will <font color=FF0000>add the prefetch hint **once the parent chunk has been loaded**</font>.

<mark>**Preload** directive has **a bunch of differences** compared to **prefetch**:</mark>

- A <font color=FF0000>**preloaded chunk starts loading in <font size=4>parallel</font> to the parent chunk**</font>. A prefetched chunk starts after the parent chunk finishes loading.
- A preloaded chunk has medium priority and is <font color=FF0000>instantly downloaded</font>. A prefetched chunk is <font color=FF0000>downloaded while the browser is idle</font>.
- A preloaded chunk should be <font color=FF0000>instantly requested by the parent chunk</font>. A prefetched chunk <font color=FF0000>can be used anytime in the future</font>.
- <font color=FF0000>Browser support is different</font>.

> 💡 **Tip**: <font color=FF0000>Using `webpackPreload` incorrectly can actually hurt performance</font>, so be careful when using it.

<mark>Sometimes you need to have your own control over preload</mark>. For example, <font color=FF0000>preload of any dynamic import **can be done via async script**</font>. This can be useful in case of streaming server side rendering （即 SSR ）.

```js
const lazyComp = () =>
  import('DynamicComponent').catch((error) => {
    // Do something with the error.
    // For example, we can retry the request in case of any net error
  });
```

If the script loading will fail before webpack starts loading of that script by itself (webpack just creates a script tag to load its code, if that script is not on a page), that catch handler won't start till [chunkLoadTimeout](https://webpack.js.org/configuration/output/#outputchunkloadtimeout) is not passed. This behavior can be unexpected. But it's explainable — webpack can not throw any error, cause webpack doesn't know, that script failed. Webpack will add onerror handler to the script right after the error has happen.

**<font color=FF0000>To prevent such problem</font> you can add your own onerror handler**, which removes the script in case of any error:

```html
<script
  src="https://example.com/dist/dynamicComponent.js"
  async
  onerror="this.remove()"
></script>
```

<font color=FF0000>In that case, errored script will be removed</font>. Webpack will create its own script and any error will be processed without any timeouts.

##### Bundle Analysis

<font color=FF0000>**Once you start splitting your code**, it can be useful to analyze the output to check where modules have ended up</font>. The [official analyze tool](https://github.com/webpack/analyse) is a good place to start. There are some other community-supported options out there as well:

- [webpack-chart](https://alexkuz.github.io/webpack-chart/): <font color=FF0000>Interactive pie chart</font> for webpack stats.
- [webpack-visualizer](https://chrisbateman.github.io/webpack-visualizer/): Visualize and analyze your bundles to <font color=FF0000>see which modules are taking up space and which might be duplicates</font>.
- [webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer): A plugin and CLI utility that <font color=FF0000>represents bundle content as a convenient **interactive zoomable treemap**</font>.
- [webpack bundle optimize helper](https://webpack.jakoblind.no/optimize): This tool will analyze your bundle and <font color=FF0000>**give you actionable suggestions** on what to improve to reduce your bundle size</font>.
- [bundle-stats](https://github.com/bundle-stats/bundle-stats): <font color=FF0000>Generate a bundle report</font> (bundle size, assets, modules) and compare the results between different builds.

摘自：[webpack 文档 - Guide - Code Splitting](https://webpack.js.org/guides/code-splitting/)



#### 懒加载 ( Lazy Loading ) 文档

##### 总述

Lazy, or "on demand", loading is a great way to optimize your site or application. <font color=dodgerBlue>This practice</font>（译为 “实践” 比较好，同样的 “best practice ”） <font color=dodgerBlue>essentially involves splitting your code at logical breakpoints, and then loading it once the user has done something that requires, or will require, a new block of code</font>. <font color=fuchsia>This **speeds up the initial load of the application** and **lightens its overall weight as some blocks may never even be loaded**</font>.

##### 代码实践

```diff
// src/index.js

+ import _ from 'lodash';
+
- async function getComponent() {
+ function component() {
    const element = document.createElement('div');
-   const _ = await import(/* webpackChunkName: "lodash" */ 'lodash');
+   const button = document.createElement('button');
+   const br = document.createElement('br');

+   button.innerHTML = 'Click me and look at the console!';
    element.innerHTML = _.join(['Hello', 'webpack'], ' ');
+   element.appendChild(br);
+   element.appendChild(button);
+
+   // Note that because a network request is involved, some indication
+   // of loading would need to be shown in a production-level site/app.
+   button.onclick = e => import(/* webpackChunkName: "print" */ './print').then(module => {
+     const print = module.default; // 👀 default 见这里
+
+     print();
+   });

    return element;
  }

- getComponent().then(component => {
-   document.body.appendChild(component);
- });
+ document.body.appendChild(component());
```

> ⚠️ **Warning** : Note that <font color=fuchsia>when using `import()` on ES6 modules you **must reference the `.default` property**</font> （注：`.default` 见上面的代码 ）**as** <font color=fuchsia>**it's the actual `module` object** that will be returned when the promise is resolved</font>.
>
> 译：注意当调用 ES6 模块的 `import()` 方法（引入模块）时，必须指向模块的 `.default` 值，因为它才是 promise 被处理后返回的实际的 `module` 对象。

摘自：[webpack doc - Guides - Lazy Loading](https://webpack.js.org/guides/lazy-loading/)



#### 打包分析

[webpack/analyse](https://github.com/webpack/analyse) 是 webpack分析工具。使用命令如下：

```sh
webpack --profile --json > stats.json
```

它会生成一个status.json文件，用于 记录对于打包过程的描述。也可以将其 上传到 https://webpack.github.io/analyse/ 中，进行分析。可以查看生成的文件的信息，以及它们之间的关系。除了上面的网站，还有其他的网站工具，如：https://alexkuz.github.io/webpack-chart/，更多工具详见：https://webpack.js.org/guides/code-splitting/#bundle-analysis

另外，命令中的配置项（ --profile ）也可以添加到 其他 npm scripts 中

##### 代码利用率

在 Chrome 的控制台中，按下**⇧ + ⌘ + P**，会出现命令菜单 ( Command Menu )，此时输入 coverage，可以查看代码的利用率，越高越好。开启效果图如下：

![image-20210916164550767](https://i.loli.net/2021/09/16/GaP61kAXmCSpqIf.png)

##### preloading 和 prefetching

懒加载（ lazy loading，即：使用 ES6 的 import，要使用了才去获取）会出现一个问题，用了再去获取、加载，响应速度可能会很慢。这时候就要用到 preloading 和 prefetching。同样 preloading 和 prefetching 可以通过 magic comments 设置（**注：**更多 magic comments 设置参见：[webpack - Module Methods](https://webpack.js.org/api/module-methods/) ）。

```js
// 摘自：https://webpack.js.org/api/module-methods/#magic-comments
import(
  /* ... 注：还有设置被省略，没有摘抄 */
  /* webpackPrefetch: true */
  /* webpackPreload: true */
  `./locale/${language}`
);
```

- **prefetch**（预获取）：将来某些导航下可能需要的资源
- **preload**（预加载）：当前导航下可能需要资源

**与 prefetch 指令相比，preload 指令有许多不同之处：**

- preload chunk 会在父 chunk 加载时，以并行方式开始加载。prefetch chunk 会在父 chunk 加载结束后开始加载。
- preload chunk 具有中等优先级，并立即下载。prefetch chunk 在浏览器闲置时下载。
- preload chunk 会在父 chunk 中立即请求，用于当下时刻。prefetch chunk 会用于未来的某个时刻。
- 浏览器支持程度不同



#### CSS 代码分割

CSS在打包时默认被添加到JS文件中，可以使用 MiniCssExtractPlugin 对 CSS 进行 <font color=FF0000>代码分割，**让它成为一个单独的文件**</font>。这个插件适合在线上环境的配置文件上使用。另外，这个插件需要 npm install 安装。

> <font color=FF0000>**代码分割，只是把 CSS 代码提取出来，不是压缩 CSS 代码**</font>；压缩 CSS 代码还是需要使用 [optimize-css-assets-webpack-plugin](https://github.com/NMFR/optimize-css-assets-webpack-plugin)。另外，根据下面文章的说法：使用 optimize-css-assets-webpack-plugin 会导致压缩 JS 失效，所以需要额外引入一个压缩 JS 的插件，比如：uglifyjs-webpack-plugin
>
> **注：**再次强调 ⚠️ ，optimize-css-assets-webpack-plugin 是 webpack@4 压缩 CSS 代码的方案，webpack@5 中推荐使用 [css-minimizer-webpack-plugin](https://github.com/NMFR/optimize-css-assets-webpack-plugin) ，这也是 optimize-css-assets-webpack-plugin GitHub readme 中的说法
>
> 学习自：[重构之路：webpack打包体积优化（超详细）](https://juejin.cn/post/6844903781377785863)

```js
// webpack.prod.conf.js
const MiniCssExtractPlugin = require('mini-css-extract-plugin')

const prodConfig = {
  mode: 'productioin',
  devtool: 'cheap-module-source-map',
  module: {
    rules: [{
      test: /\.scss$/,
      // 这里使用 MiniCssExtractPlugin 的解释在下面的 plugins 配置项那边
      use: [
        MiniCssExtractPlugin.loader,
        {
          loader: 'css-loder',
          options: {
            importLoader: 2
          }
        },
        'sass-loader',
        'postcss-loader'
      ]
    }, {
      test: /\.css$/,
      use: [MiniCssExtractPlugin.loader, 'css-loader', 'postcss-loader']
    }]
  }
  plugins: [
    // 由于该插件必须要使用自己的 loader 处理CSS，需要替换掉 style-loader
	  // 所以需要将之前公用的 webpack.common.conf.js中css-loader 相关的删除掉。
    // 分别在webpack.dev.conf.js和webpack.prod.conf.js中单独配置。
    // webpack.dev.conf.js使用之前（webpack.common.conf.js）的配置，
    // webpack.prod.conf.js 需要加上MiniCssExtractPlugin的loader
    new MiniCssExtractPlugin({
  		// 直接被入口文件 引入
  		filename: '[name].css',
  		// 间接被引入，被其他文件（非入口文件）引入
  		chunkFileName: '[name].chunk.css'
		})
  ]
}
```

如果想要对 CSS 代码进行代码压缩 和 合并，可以使用 optimize-css-assets-webpack-plugin（在webpack@5 中 官方推荐的是 css-minimizer-webpack-plugin，更多可以看文档 ），由于这里讲了optimize-css-assets-webpack-plugin，所以这里进行介绍：

```js
//webpack.prod.conf.js
optimization: {
  minimizer: [new OptimizeCSSAssetsPlugin({})]
}
```

webpack 可能会对打包后的结果做出性能警告，既可以解决问题；可以使用

```js
// webpack.common.conf.js
module.exports = {
	performance: false
}
```

以忽略警告。



#### 浏览器缓存

浏览器是有缓存的，可以利用缓存，避免不必要的加载。于是，需要对 webpack 配置进行修改

对于开发环境，不需要处理；对于生产环境，需要进行配置

```js
// webpack.prod.conf.js
output: {
	// contenthash是关键
	filename: '[name].[contenthash].js',
	chunkFilename: '[name].[contenthash].js'
}
```

由于使用了 contenthash，会对代码的内容进行 hash 处理，如果代码发生改变，则 hash 变化；如果代码没有发生变化，则 hash 值不变。同样，由于缓存的存在，hash 值变化，文件名变化会重新加载；hash 值不变，文件名不变，由于缓存，则不会重新加载。

如果使用的webpack版本较老，会出现如下问题：即是代码没有修改，两次打包 contenthash 的值是不一样的。这时候可以通过配置解决这一问题：

```js
// webpack.common.conf.js
optimization: {
  runtimeChunk: {
    name: 'runtime'
  }
}
```

这时候问题解决了，但是打包出的文件中会多一个 runtime.[contenthash].js  的文件。之所以contenthash会不同，这是由于 业务逻辑 和 代码库 之间是有关联的，在webpack中将这些（描述）关联的代码，叫做 manifest，manifest 存在于 业务代码打包的结果、也存在于 代码库的打包结果的；在旧版的webpack中，每次打包时 manifest 会有变化；所以即使没有改动代码，由于manifest的变化，业务代码打包的结果、代码库的打包结果可能就变了；这就导致 contenthash 的不同。而使用 runtimeChunk 的配置将这些 manifest 抽离出来，放到了 runtime.[contenthash].js 的文件中；则不会再对 业务逻辑 和 代码库的代码产生影响。

而新版本的webpack不会有类似的问题。

#### Cache 文档补充

##### 总述

So we're using webpack to bundle our modular（模块化的） application which yields（生成） a deployable `/dist` directory. Once the contents of `/dist` have been deployed to a server, clients ( typically browsers ) will hit that server to grab the site and its assets. <mark>The last step can be time consuming, which is why browsers use a technique called [caching](https://en.wikipedia.org/wiki/Cache_(computing))</mark>. <font color=FF0000>This allows sites to load faster with less unnecessary network traffic</font>. However, it can also cause headaches when you need new code to be picked up.

##### 提取引导模板(extracting boilerplate)

<mark>As we learned in [code splitting](https://webpack.js.org/guides/code-splitting), the [`SplitChunksPlugin`](https://webpack.js.org/plugins/split-chunks-plugin/) can be used to split modules out into separate bundles</mark>. <font color=FF0000>Webpack provides an optimization feature to split runtime code into a separate chunk using the [`optimization.runtimeChunk`](https://webpack.js.org/configuration/optimization/#optimizationruntimechunk) option</font>. Set it to `single` to create a single runtime bundle for all chunks:

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  plugins: [
    new HtmlWebpackPlugin({
    title: 'Caching',
    }),
  ],
  output: {
    filename: '[name].[contenthash].js', // 这里 [contenthash] 原文上面有讲，由于之前的笔记也有说过，这里略
    path: path.resolve(__dirname, 'dist'),
    clean: true,
  },
  optimization: {
    runtimeChunk: 'single',
  },
};
```

It's also good practice to <font color=FF0000>extract third-party libraries, such as `lodash` or `react`, to a <font size=4>**separate `vendor` chunk**</font> as they are less likely to change than our local source code</font>（ 将第三方库 ( library )（例如 `lodash` 或 `react` ）**提取到单独的 `vendor` chunk 文件中**，是比较推荐的做法，这是因为，它们很少像本地的源代码那样频繁修改 ）. This step will allow clients to request even less from the server to stay up to date. <font color=FF0000>This can be done by using the [`cacheGroups`](https://webpack.js.org/plugins/split-chunks-plugin/#splitchunkscachegroups) option of the [`SplitChunksPlugin`](https://webpack.js.org/plugins/split-chunks-plugin/)</font> demonstrated in [Example 2 of SplitChunksPlugin](https://webpack.js.org/plugins/split-chunks-plugin/#split-chunks-example-2). Lets <font color=FF0000>**add `optimization.splitChunks` with `cacheGroups`**</font> with next params and build:

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  plugins: [
    new HtmlWebpackPlugin({
      title: 'Caching',
    }),
  ],
  output: {
    filename: '[name].[contenthash].js',
    path: path.resolve(__dirname, 'dist'),
    clean: true,
  },
  optimization: {
    runtimeChunk: 'single',
    splitChunks: {
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all',
        },
      },
    },
  },
};
```

Let's run another build to see our new `vendor` bundle:

```bash
...
                          Asset       Size  Chunks             Chunk Names
runtime.cc17ae2a94ec771e9221.js   1.42 KiB       0  [emitted]  runtime
vendors.a42c3ca0d742766d7a28.js   69.4 KiB       1  [emitted]  vendors
   main.abf44fedb7d11d4312d7.js  240 bytes       2  [emitted]  main
                     index.html  353 bytes          [emitted]
...
```

**注：**注意这里的打包结果，会和下面 [[#模块标识符 Module Identifiers]] 的打包结果比对

We can now see that our <font color=FF0000>`main` bundle does not contain `vendor` code from `node_modules` directory</font> and is down in size to `240 bytes`（ `main` 不再含有来自 `node_modules` 目录的 `vendor` 代码，并且体积减少到 240 bytes）!

##### 模块标识符 Module Identifiers

（原文中添加了新的文件，将其引入打包入口 index.js 中，再次打包）Running another build, we <font color=FF0000>would expect only our `main` bundle's hash to change</font>, however...

```bash
...
                           Asset       Size  Chunks                    Chunk Names
  runtime.1400d5af64fc1b7b3a45.js    5.85 kB      0  [emitted]         runtime
  vendor.a7561fb0e9a071baadb9.js     541 kB       1  [emitted]  [big]  vendor
    main.b746e3eb72875af2caa9.js    1.22 kB       2  [emitted]         main
                      index.html  352 bytes          [emitted]
...
```

... we can see that all three have （发现三个输出结果的 contenthash 都改变了 ）. <font color=FF0000>This is because **each [`module.id`](https://webpack.js.org/api/module-variables/#moduleid-commonjs) is incremented based on resolving order by default**</font>（每个 module.id 会默认地基于解析顺序 ( resolve order ) 进行增量）. Meaning <font color=FF0000>when the order of resolving is changed, the IDs will be changed as well</font>. <mark style="background: aqua">To recap（简要概括）</mark>:

- The `main` bundle changed because of its new content
- <font color=FF0000>The `vendor` bundle changed because its `module.id` was changed</font>.
- And, <font color=FF0000>the `runtime` bundle changed because it now contains a reference to a new module</font>.

The first and last are expected, it's the `vendor` hash we want to fix. Let's use [`optimization.moduleIds`](https://webpack.js.org/configuration/optimization/#optimizationmoduleids) with `'deterministic'` option:

```diff
  const path = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');

  module.exports = {
    entry: './src/index.js',
    plugins: [
      new HtmlWebpackPlugin({
        title: 'Caching',
      }),
    ],
    output: {
      filename: '[name].[contenthash].js',
      path: path.resolve(__dirname, 'dist'),
      clean: true,
    },
    optimization: {
+     moduleIds: 'deterministic',
      runtimeChunk: 'single',
      splitChunks: {
        cacheGroups: {
          vendor: {
            test: /[\\/]node_modules[\\/]/,
            name: 'vendors',
            chunks: 'all',
          },
        },
      },
    },
  };
```

Now, despite any new local dependencies, <font color=FF0000>our `vendor` hash should stay consistent between builds</font>（幂等）:

```bash
...
                          Asset       Size  Chunks             Chunk Names
   main.216e852f60c8829c2289.js  340 bytes       0  [emitted]  main
vendors.55e79e5927a639d21a1b.js   69.5 KiB       1  [emitted]  vendors
runtime.725a1a51ede5ae0cfde0.js   1.42 KiB       2  [emitted]  runtime
                     index.html  353 bytes          [emitted]
Entrypoint main = runtime.725a1a51ede5ae0cfde0.js vendors.55e79e5927a639d21a1b.js main.216e852f60c8829c2289.js
...
```

摘自：[webpack 文档 - Guide - Caching](https://webpack.js.org/guides/caching)



#### ES Module 文档摘抄

##### Flagging modules as ESM

**By default** webpack will automatically detect whether a file is an ESM or a different module system.

<font color=dodgerBlue>Node.js established a way of explicitly setting the module type of files by using a property in the `package.json`</font> . <font color=fuchsia>Setting `"type": "module"` in a package.json does **force** all files below this package.json to be ECMAScript Modules</font>. Setting `"type": "commonjs"` will instead force them to be CommonJS Modules.

```json
{
  "type": "module"
}
```

<font color=fuchsia>In addition to that , files can set the module type by **using `.mjs` or `.cjs` extension**</font>. <font color=red>`.mjs` will force them to be ESM , `.cjs` force them to be CommonJs</font>.

<font color=red>In DataURIs using the `text/javascript` or `application/javascript` mime type will **also force** module type to **ESM**</font> .

In addition to the module format , <font color=fuchsia>flagging modules as ESM also affect the resolving logic , interop logic and the available symbols in modules</font>.

<font color=dodgerBlue>Imports in ESM are **resolved more strictly**</font>. Relative requests must include a filename and file extension ( e.g. `*.js` or `*.mjs` ) unless you have the behaviour disabled with [`fullySpecified=false`](https://webpack.js.org/configuration/module/#resolvefullyspecified) .

> 💡 **Tip** : Requests to packages e.g. `import "lodash"` are still supported. （👀 注：这点之前没什么印象...）

Only the "default" export can be imported from non-ESM. Named exports are not available.

（👀 注：应该加上 “在 ESM” 中）<font color=dodgerBlue>CommonJs Syntax is not available</font> : `require` , `module` , `exports` , `__filename` , `__dirname `.

> 💡 **Tip** : <font color=fuchsia>HMR can be used with [`import.meta.webpackHot`](https://webpack.js.org/api/module-variables/#importmetawebpackhot) instead of [`module.hot`](https://webpack.js.org/api/module-variables/#modulehot-webpack-specific)</font> .

摘自：[webpack doc - Guides - ECMAScript Modules](https://webpack.js.org/guides/ecma-script-modules/)



#### shimming 垫片

shim 类似于 polyfill，但两者不等价。下面关于 shim 进行举例：

由于ES Module的存在：在一个文件a.js中引入了一个库，比如jquery或者lodash，如果a.js中还引入了b.js，且b.js中使用了jquery或者lodash，但是在自己的module中没有引入，这是代码运行会报错，因为找不到jqury或者lodash。这时候，可以使用 shim 垫片的形式 将jquery的$，或者lodash的_，作为一个全局定义。配置如下：

```js
// webpack.common.js
const webpack = require('webpack')

module.exports = {
  plugins: [
    new webpack.providePlugin({
      $: 'jquery',
      _: 'lodash'
    })
  ]
}
```

这时候，b.js 在打包执行时，发现了 \$ 或者 \_，会自动在b.js中引入 jquery（import \$ from 'jquery'）lodash（import _ from 'lodash'）

甚至上面的全局定义这一这样设置：

```js
_join: ['lodash', 'join']
```

这时：`_join()` 将会等价于 lodash.join() 。

**Shim 垫片甚至可以实现如下功能：**在Module 中，this 始终指向模块自身，而使用imports-loader，可以将this指向变成window。配置如下：

```js
// webpack.common.conf.js
rules: [{
  test: /\.js$/,
  exclude: /node_modules/,
  user: [{
    loader: 'babel-loader'
  }, {
    loader: 'imports-loader?this=>window'
  }]
}]
```



#### Shimming 文档补充

##### 总述

The `webpack` compiler can understand modules written as ES2015 modules, CommonJS or AMD . However, <font color=dodgerBlue>some third party libraries may expect global dependencies ( e.g. `$` for `jQuery` )</font> . <font color=dodgerBlue>The libraries might also create **globals**</font>（全局变量） <font color=dodgerBlue>which need to be exported</font>. These **"broken modules"**（不符合规范的模块） are <mark style="background: lightpink">**one instance where *shimming* comes into play**</mark> .

> ⚠️ **Warning** : <font color=fuchsia>**We don't recommend using globals!**</font> The whole concept behind webpack is to allow more modular（模块化的） front-end development. This means <font color=red>writing isolated modules that are **well contained**</font>（良好的封闭性） and <font color=red>do not rely on hidden dependencies</font> (e.g. globals). Please use these features only when necessary.

<mark style="background: lightpink">**Another instance where *shimming***</mark> can be useful is <font color=fuchsia>when you want to **polyfill browser** functionality to support more users</font>. In this case, you may only want to deliver those polyfills to <font color=red>the browsers that need **patching**</font>（修补） ( i.e. load them on demand ).

##### 全局引入

we wanted to instead <font color=red>provide this</font>（前面有省略，根据上下文 this 是指 lodash ） <font color=red>**as a global** throughout our application</font> . **To do this , we can use [`ProvidePlugin`](https://webpack.js.org/plugins/provide-plugin)** .

<font color=dodgerblue>The `ProvidePlugin` **makes a package available as a variable in every module** compiled through webpack</font>. <font color=fuchsia>If webpack sees that variable used , it will **include the given package in the final bundle**</font>. Let's go ahead by removing the `import` statement for `lodash` and instead provide it via the plugin:

```diff
// src/index.js
-import _ from 'lodash';
-
 function component() {
   const element = document.createElement('div');

-  // Lodash, now imported by this script
   element.innerHTML = _.join(['Hello', 'webpack'], ' ');
   return element;
 }

 document.body.appendChild(component());
```

```diff
 const path = require('path');
+const webpack = require('webpack');

 module.exports = {
   entry: './src/index.js',
   output: {
     filename: 'main.js',
     path: path.resolve(__dirname, 'dist'),
   },
+  plugins: [
+    new webpack.ProvidePlugin({
+      _: 'lodash',
+    }),
+  ],
 };
```

What we've essentially done here is tell webpack...

> <font color=red>If you **encounter at least one instance** of the variable `_`</font> , include the `lodash` package and provide it to the modules that need it.

<font color=dodgerBlue>We can also use the `ProvidePlugin` to **expose a single export of a module**</font>（暴露某个模块中单个导出值，示例见下面）<font color=dodgerBlue>by **configuring it with an "array path"**</font> ( <font color=fuchsia>**e.g. `[module, child, ...children?]`**</font> ) . So let's imagine we <font color=dodgerblue>**only** wanted to provide the `join` method from `lodash`</font> wherever it's invoked:

```diff
 // src/index.js
 function component() {
   const element = document.createElement('div');

-  element.innerHTML = _.join(['Hello', 'webpack'], ' ');
+  element.innerHTML = join(['Hello', 'webpack'], ' ');

   return element;
 }

 document.body.appendChild(component());
```

```diff
 const path = require('path');
 const webpack = require('webpack');

 module.exports = {
   entry: './src/index.js',
   output: {
     filename: 'main.js',
     path: path.resolve(__dirname, 'dist'),
   },
   plugins: [
     new webpack.ProvidePlugin({
-      _: 'lodash',
+      join: ['lodash', 'join'],
     }),
   ],
 };
```

This would <font color=fuchsia>go nicely with Tree Shaking</font> as <font color=fuchsia>**the rest of the `lodash` library should get dropped**</font>.

##### 细粒度 shimming

<font color=dodgerBlue>Some legacy modules rely on `this` being the `window` object</font>（一些传统的模块依赖的 `this` <font color=red>指向的是</font> `window` 对象） . <font color=red>This becomes a problem when the module is executed **in a CommonJS context where `this` is equal to `module.exports`**</font> . In this case <font color=fuchsia>**you can override `this` using the [`imports-loader`](https://webpack.js.org/loaders/imports-loader/)**</font> :

```js
// webpack.config.js
module.exports = {
  module: {
    rules: [
      {
        test: require.resolve('./src/index.js'), // 匹配上 require.resolve 返回的路径，则使用 loader
        use: 'imports-loader?wrapper=window',
      },
    ],
  },
  // ...
}
```

> 👀 注：上面的 require.resolve 方法是 Node module 的方法：
>
> > ```js
> > require.resolve(request[, options])
> > ```
> >
> > - **request** : `<string>` , The module path to resolve.
> > - **options** :  `<Object>`
> >   - **paths** : `<string[]>` , <font color=red>Paths to resolve module location from</font>. If present, <font color=fuchsia>these paths are used **instead of the default resolution paths**</font> , with the exception of `GLOBAL_FOLDERS` like `$HOME/.node_modules` , which are always included. Each of these paths is used as a starting point for the module resolution algorithm, meaning that the `node_modules` hierarchy（层级） is checked from this location.
> > - **Returns** : `string`
> >
> > Use the internal `require()` machinery to look up the location of a module, but <mark>rather than loading the module, just return the resolved filename</mark>.
> >
> > <font color=fuchsia>If the module can not be found, **a `MODULE_NOT_FOUND` error is thrown**</font>.
> >
> > 摘自：[Node doc - modules - require.resolve(request[, options])](https://nodejs.org/api/modules.html#requireresolverequest-options)
>
> 另外，require.resolve 的用法，还有返回值，都和 path.resolve 相当类似；这里说一下区别：
>
> 根据 [require.resolve和path.resolve](https://blog.csdn.net/wu_xianqiang/article/details/121783008) 中的说法：在“相对路径” 下这两者基本没什么区别。
>
> >  👀 注：自己实践时发现三点区别：
> >
> > 1. 如果 `require.resolve` 中的 `request` 参数是一个文件夹，比如 `./` ，则会搜索当前文件夹下的 index.js 等文件，找到则返回结果，找不到则报错；而 path.resolve 只会返回文件夹路径字符串
> > 2. path.resolve 找不到不会报错，而是把路径（哪怕是不存在的）返回
> > 3. path.resolve 的语法是：``path.resolve([...paths])`` ，和 require.resolve 不一样
>
> 而在非相对路径下，两者查找路径是不同的：`require.resolve` 默认从 当前项目下( local )的 node_module 文件夹下开始搜索，不会搜索 global（自己测试过）；比如 `require.resolve('webpack')`。而 `path.resolve` 是 从当前目录出发找模块。
>
> 学习与启发自：[require.resolve和path.resolve](https://blog.csdn.net/wu_xianqiang/article/details/121783008) 

##### Global Exports

Let's say <mark>a library creates a global variable</mark> that <mark>it expects its consumers to use</mark>. （👀 注：这里省略一些内容，包括下面的前半句）

you may encounter a dated library you'd like to use that contains similar code to what's shown above（ 👀 注：“above” 的内容被省略，见原文）. In this case, <mark>we can use</mark> [`exports-loader`](https://webpack.js.org/loaders/exports-loader/) , <mark>to export that global variable as a normal module export</mark>. For instance, in order to export `file` as `file` and `helpers.parse` as `parse` :

```diff
 module.exports = {
   entry: './src/index.js',
   module: {
     rules: [
       {
         test: require.resolve('./src/index.js'),
         use: 'imports-loader?wrapper=window',
       },
+      {
+        test: require.resolve('./src/globals.js'),
+        use: 'exports-loader?type=commonjs&exports=file,multiple|helpers.parse|parse',
+      },
     ],
   },
 };
```

> 👀 注：上面的写法应该是 resourceQuery，相关的内容可以参考 [webpack doc - cfg - module # Rule.resourceQuery](https://webpack.js.org/configuration/module/#ruleresourcequery) ；另外，[webpack doc - guides - asset modules # Replacing Inline Loader Syntax](https://webpack.js.org/guides/asset-modules/#replacing-inline-loader-syntax) 中也有提及（也更详细点）

Now from within our entry script (i.e. `src/index.js` ) , <font color=red>**we could use `const { file, parse } = require('./globals.js');` and all should work smoothly**</font>.

##### Loading Polyfills

<font color=red>**There's a lot of ways to load polyfills**</font>. For example, to include the [`babel-polyfill`](https://babeljs.io/docs/en/babel-polyfill/) we might: `npm install --save babel-polyfill` and <font color=red>`import` it so as to include it in our main bundle</font>:

```js
// src/index.js 这也是入口文件
import 'babel-import'

// ...
```

> 💡 Tip : Note that <font color=red>we aren't binding the `import` to a variable</font>. This is because <font color=fuchsia>polyfills simply run on their own, prior to the rest of the code base</font>, allowing us to then assume certain native functionality exists.
>
> 译：注意，我们没有将 `import` 绑定到某个变量。这是因为 polyfill 直接基于自身执行，并且是在基础代码执行之前，这样通过这些预置，我们就可以假定已经具有某些原生功能。

Note that <font color=fuchsia>this approach **prioritizes correctness over bundle size**</font>（👀 注：即 bundle 体积很大）. <font color=fuchsia size=4>**To be safe and robust**, polyfills/shims must run **before all other code**</font>, and <font color=red>thus either need to load synchronously</font>（ 👀 注：同步是为了保证先执行？）, **or**, <font color=red>all app code needs to load after all polyfills/shims load</font>. <font color=dodgerBlue>There are many **misconceptions** in the community</font>, as well, <mark>that modern browsers "don't need" polyfills, or that polyfills/shims merely serve to add missing features</mark>（👀 注：为保证之后阅读断章取义，前面高亮的内容是错的） - in fact, <font color=fuchsia>they often *repair broken implementations*, **even in the most modern of browsers**</font>. <mark style="background: lightpink">The **best practice** thus remains to unconditionally and synchronously load all polyfills/shims, despite the bundle size cost this incurs</mark>（👀 注：这是总结）.

If you feel that you have mitigated（减轻，这里理解为“打消”） these concerns（顾虑） and wish to incur the risk of brokenness（希望承受损坏的风险）, <font color=dodgerBlue>here's one way you might do it</font> : Let's move our `import` to a new file and add the [`whatwg-fetch`](https://github.com/github/fetch) polyfill: `npm install --save whatwg-fetch` :

```diff
// src/index.js
-import 'babel-polyfill';

// ...
```

```diff
 // project
 webpack-demo
  |- package.json
  |- package-lock.json
  |- webpack.config.js
  |- /dist
  |- /src
    |- index.js
    |- globals.js
+   |- polyfills.js
  |- /node_modules
```

```js
// src/polyfill.js
import 'babel-polyfill';
import 'whatwg-fetch';
```

```js
// webpack.config.js
module.exports = {
  entry: {
    polyfills: './src/polyfills',
    index: './src/index.js',
  },
  output: {
    filename: '[name].bundle.js', // 因为多入口了
    path: path.resolve(__dirname, 'dist'),
  },
}
```



摘自：[webpack doc - Guides - Shimming](https://webpack.js.org/guides/shimming/)

// TODO [ProvidePlugin](https://webpack.js.org/plugins/provide-plugin/) 笔记





#### 环境变量

在webpack打包过程中，可以通过 环境变量 来控制 是打 开发环境 还是生产环境的包。主要应用在 npm scripts中，可以通过类似 --env.production=foo，控制打那种环境的包。

不过，这时 webpack.dev.conf.js 和 webpack.prod.conf.js 要引入 webpack.common.conf.js，从而便于控制

```js
// webpack.common.js

import devConfig = require('/webpack.dev.conf.js')
import prodConfig = require('/webpack.prod.conf.js')
import merge = require('webpack-merge')

module.exports = (env) => {
  if(env && env.productioin) {
    // 生产环境
    return merge(commonConfig, prodConfig)
  } else {
    // 开发环境
    return merge(commonConfig, devConfig)
  }
}
```

如果想要打包生产环境的，npm scripts 要这样修改，添加一个--env.production 使得 env存在，且env.production 也存在：

```js
scripts: {
	"build": "webpack --env.production --config ./build/webpack.common.conf.js"
}
```

同时还可以这样设置：--env production，此时 上面的代码，要变成 

```js
module.exports = (production) => {
  if(production) {
    ...
  } else {...}
}
```

还可以设置：--env.productoin=foo，这时：

```js
module.exports = (env) => {
  if(env && env.production === 'foo') {
    ...
  } else {...}
}
```



#### 对于代码库 Library 的打包

##### 在使用 代码库 时有多种引入方式

**ES Module：**

```js
import library from 'library'
```

**commonJS：**

```js
const library = require('library')
```

**AMD：**

```js
require(['library'], function() { ... })
```

**\<scriprt /\>：**

```html
<script src='library.js'></script>
```

同时希望：引入 library 之后，可以使用 library.member

##### 配置如下

```js
output: {
  path: path.reslove(__dirname, 'dist'),
  filename: 'library.js,
  // 用于适配 <script> ，让library 挂载到 library 的全局变量上，即在全局变量上添加一个 library 变量，在页面上可以访问 library 变量；亦即，打包好的库通过 'library' 的名字暴露出去。
  libray: 'library',
  // umd 表示无论使用那种引入方式（ESM CommonJS AMD）都可以（但是 <script> 不行），u表示universal
  libraryTarget: 'umd'
}
```

library 和 libraryTarget 是可以配合使用的，如果将 libraryTarget 配置为 'this'（如下），library 将会被挂载到 this 下（this.library），同时 library 也是可以访问的。同样libraryTarget 也可以配置为 'window'

```js
output: {
  path: path.reslove(__dirname, 'dist'),
  filename: 'library.js,
  libray: 'library',
  libraryTarget: 'this'
}
```

当我们的 library 中使用了第三方的库文件（比如 lodash ），可能会出现：用户已经在自己的项目中使用（打包）了 lodash，而我们的library 时又引入（打包）了一次 lodash。用户的代码中会出现两份 lodash的代码。可以做如下配置：

```js
// webpack.config.js
module.exports = {
  // 表示在打包中，如果遇到 lodash库，则忽略掉 lodash
  externals: ['lodash']
}
```

externals 可以配置为数组和对象。

```js
// webpack.config.js
module.exports = {
  externals: {
    lodash: {
      // 如果 我的library 以 commonjs的形式被引入，lodash 被加载时，名字必须要被叫做 lodash
      commonjs: 'lodash',
      // 通过全局的<script> 标签引入，则<script> 标签必须在全局注入一个'_'的全局变量
      root: '_'
    }
  }
}
```

**注：**上面相关 外置化 第三方库，下面有 webpack 文档 的补充 [[#Externalize Lodash]]



想要把代码给别人使用，需要配置 package.json

```json
// package.json
{
  "main": "./dist/library.js"
}
```

如果想要发布自己写的 library 到 npm 上，可以先去 [npm 官网](https://npmjs.com) 注册

注册好后，可以在命令行，输入

```sh
# 添加用户名和密码
npm adduser [--registry=url] [--scope=@orgname] [--auth-type=legacy]

# 发布
npm publish [<tarball>|<folder>] [--tag <tag>] [--access <public|restricted>] [--otp otpcode] [--dry-run]
```

另外，npm的库名称 必须要是独一无二的，不能与别人冲突的。要修改 npm 包的名字同样是通过 package.json 去修改：

```json
// package.json
{
  name: 'foooooo'
}
```



#### 创建 library ( Authoring Libraries )

##### 总述

Aside from applications, <font color=FF0000>webpack can also be used to bundle JavaScript libraries</font>.

##### Expose the Library

So far everything should be the same as bundling an application, and here comes the different part – <font color=FF0000>we need to **expose exports** from the entry point through [`output.library`](https://webpack.js.org/configuration/output/#outputlibrary) option</font>.

```diff
  const path = require('path');

  module.exports = {
    entry: './src/index.js',
    output: {
      path: path.resolve(__dirname, 'dist'),
      filename: 'webpack-numbers.js',
+     library: "webpackNumbers",
    },
  };
```

However <font color=FF0000>it only works when it's referenced through script tag</font>（比如 CDN 引入）, it <font color=FF0000>**<font size=4>can't</font> be used in other environments like CommonJS, AMD, Node.js, etc**</font>.

Let's update the `output.library` option with its `type` set to [`'umd'`](https://webpack.js.org/configuration/output/#type-umd) :

```diff
 const path = require('path');

 module.exports = {
   entry: './src/index.js',
   output: {
     path: path.resolve(__dirname, 'dist'),
     filename: 'webpack-numbers.js',
-    library: 'webpackNumbers',
+    library: {
+      name: 'webpackNumbers',
+      type: 'umd',
+    },
   },
 };
```

**注：**UMD ( Universal Module Definition ) : UMD patterns for JavaScript modules that work everywhere.

##### Externalize Lodash

**注：**实际上不仅仅是 lodash，而是所有的第三库。

Now, if you run `npx webpack` , you will find that a largish（相当大的） bundle is created. <mark>If you inspect the file, you'll see that lodash has been bundled along with your code</mark>. <font color=FF0000>In this case, we'd prefer to treat `lodash` as a ***peer dependency*** </font>. Meaning that <font color=FF0000>**the consumer should already have `lodash` installed**</font> （**注：**感觉这里可以理解为：用户（库的使用者，自身也是程序员）在自己的项目中也引入了 lodash，可能会造成多次安装 ）. <font color=FF0000>Hence you would want to **give up control of this external library to the consumer of your library**</font>.

This can be done using the [`externals`](https://webpack.js.org/configuration/externals/) configuration:

```diff
  const path = require('path');

  module.exports = {
    entry: './src/index.js',
    output: {
      path: path.resolve(__dirname, 'dist'),
      filename: 'webpack-numbers.js',
      library: {
        name: "webpackNumbers",
        type: "umd"
      },
    },
+   externals: {
+     lodash: {
+       commonjs: 'lodash',
+       commonjs2: 'lodash',
+       amd: 'lodash',
+       root: '_',
+     },
+   },
  };
```

This means that <font color=FF0000>your library expects a dependency named `lodash` to **be available in the consumer's environment**</font>.

##### External Limitations

For libraries that use several files from a dependency（从一个依赖中调用多个文件）:

```js
import A from 'library/one';
import B from 'library/two';

// ...
```

You won't be able to exclude them from the bundle by specifying `library` in the externals. You'll either need to exclude them one by one or by using a regular expression.

```js
module.exports = {
  //...
  externals: [
    'library/one',
    'library/two',
    // Everything that starts with "library/"
    /^library\/.+$/,
  ],
};
```

##### Final Steps（发布）

Optimize your output for production by following the steps mentioned in the [production guide](https://webpack.js.org/guides/production). Let's also add the path to your generated bundle as the package's `main` field in with the `package.json`

```json
// package.json
{
  "main": "dist/webpack-numbers.js",
}
```

Or, to add it <font color=FF0000>**as a standard module**</font> as per [this guide](https://github.com/dherman/defense-of-dot-js/blob/master/proposal.md#typical-usage):

```json
// package.json
{
  "module": "src/index.js",
}
```

<font color=FF0000>**The key `main` refers to the [standard from `package.json`](https://docs.npmjs.com/files/package.json#main) **</font>, and `module` to [a](https://github.com/dherman/defense-of-dot-js/blob/master/proposal.md) [proposal](https://github.com/rollup/rollup/wiki/pkg.module) to allow the JavaScript ecosystem upgrade to use ES2015 modules without breaking backwards compatibility （ `module` 是参照 一个提案，此提案允许 JavaScript 生态系统升级使用 ES2015 模块，而不会破坏向后兼容性）.

> **Warning ⚠️ :** The `module` property should point to a script that utilizes ES2015 module syntax <font color=FF0000>but no other syntax features that aren't yet supported by browsers or node</font>. This enables webpack to parse the module syntax itself, allowing for lighter bundles via [tree shaking](https://webpack.js.org/guides/tree-shaking/) if users are only consuming certain parts of the library.
>
> **译文：**`module` 属性应指向一个使用 ES2015 模块语法的脚本，但不包括浏览器或 Node.js 尚不支持的其他语法特性。这使得 webpack 本身就可以解析模块语法，如果用户只用到 library 的某些部分，则允许通过 tree shaking 打包更轻量的包。

**注：**看了下 Vue.js GitHub 的 [package.json](https://github.com/vuejs/vue/blob/main/package.json)，发现它 同时包含 main 和 module 的配置

**Now you can [publish it as an npm package](https://docs.npmjs.com/getting-started/publishing-npm-packages)** and find it at [unpkg.com](https://unpkg.com/#/) to distribute it to your users.

> **Tip💡** : To expose stylesheets associated with your library （感觉是 样式库？）, **the [`MiniCssExtractPlugin`](https://webpack.js.org/plugins/mini-css-extract-plugin) should be used**. Users can then consume and load these as they would any other stylesheet.



#### PWA 打包

使用 workbox-webpack-plugin 插件可以提供 PWA 的打包。同时，只有需要上线的代码需要考虑是否断网的问题，所以 PWA 插件只用在 production 环境下。

需要先安装（npm install），然后，配置如下：

```js
// webpack.prod.conf.js
const WorkboxPlugin = require('workbox-webpack-plugin')

module.exports = {
  ...
  plugins: [
    // sw 的含义是 Service worker，这也是 PWA 使用的底层技术原理
    new WorkboxPlugin.GenerateSW({
      clientsClaim: true,
      skipWaiting: true
    })
  ]
}
```

打包之后，在dist 文件夹下会出现 service-worker.js 和 precache-manifest.contenthash.js 两个文件，这两个文件使得 service worker 正常生效，使项目支持 PWA。service-worker 可以理解为另类的缓存，让页面即使在 断网 或者 服务器崩溃的 情况下，也能正常显示内容。

只有 service-worker.js 和 precache-manifest.[contenthash].js 两个文件还不够，还要在业务代码内添加代码

```js
// 业务代码

// 判断浏览器是否支持 service worker
if ( 'serviceWorker' in navigator ) {
  window.addEventListener('load', () => {
    // 注册Service workers
    navigator.serviceWorker.register('/service-worker.js')
      .then(registration => {
        // do something，浏览器有Service workers，则成功注册
    }).catch(error => {
      // do somethiing，失败处理
    })
  })
}
```

service worker 会在在浏览器中注册与保留，在开发其他项目时，有时会使得开发出现问题。这时候，可以在控制台的Application 的 Service workers下 取消  service worker 的注册，如下图：

<img src="https://i.loli.net/2021/09/17/DgULye3h7IxnJ1M.png" alt="image-20210917175053748" style="zoom: 80%;" />

另外，Chrome Developer 的 workbox-webpack-plugin 的文档见 https://developer.chrome.com/docs/workbox/modules/workbox-webpack-plugin/

#### 文档 Guides 的补充

// TODO https://developer.chrome.com/docs/workbox/what-is-workbox/



在 wepback 中 使用的插件是 [workbox-webpack-plugin](https://github.com/GoogleChrome/workbox/tree/v6/packages/workbox-webpack-plugin) ，基于 webpack 的 vue-cli 也是；而在 基于 Vite 作为 bundler 的开发环境中，PWA插件 [vite-plugin-pwa](https://github.com/antfu/vite-plugin-pwa)，使用的是 更底层的 [workbox-build](https://github.com/GoogleChrome/workbox/tree/v6/packages/workbox-build) ( workbox-webpack-plugin 依赖 workbox-build ) 和 [workbox-window](https://github.com/GoogleChrome/workbox/tree/v6/packages/workbox-window) ；详见 [vite-plugin-pwa - package.json](https://github.com/antfu/vite-plugin-pwa/blob/main/package.json)



#### TypeScript 的 webpack 相关

在使用 TS 之前，要先安装 ts 和 ts-loader

webpack配置如下：

```js
// webpack.config.js
const path = require('path')

module.exports = {
  mode: 'production',
  entry: './src/index.tsx',
  module: {
    rules: [{
      test: [{
        test: /\.tsx?$/,
        user: 'ts-loader',
        exclude: /node_modules/
      }]
    }]
  }
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
}
```

但是单单配置 webpack.config.js 是不够的，还需要添加一个 tsconfig.json 的配置文件：

```json
// tsconfig.json
{
  "compilerOptions": {
    // 如果在webpack 中已经配置过output，则添加不添加这个配置项都可以
    "outDir": "./dist",
    // 模块引入方式，这里 es6 表示 es module 的引入方式
    "module": "es6",
    // 目标文件类型，即 将TS 代码转换成什么形式的代码
    "target": "es5",
    // 是否允许 TS 的语法中引入JS 的模块
    "allowJs": true
  }
}
```

TS 会对不规范的代码进行报错，但是不会对 第三方的库（比如 lodash）的函数的错误使用，进行报错。（以 lodash 为例）这时可以使用 @type/lodash 模块，这是一个类型文件。这时 TS 会正确识别 lodash 中的函数，使用哪些参数，一旦使用错误，将会报错，与错误信息。

想要知道哪些工具（代码库）有这种 类型文件，可以去 www.typescriptlang.org/dt/search 去搜索。

#### TS 文档补充





####  webpackDevServer 请求转发

如果要对请求转发进行配置，要对 devServer 的 proxy 进行处理。示例如下：

```js
// webpack.config.js

// **只有在开发环境下，devServer 才会生效**，devServer在生产环境会被忽略掉
devServer: {
  proxy: {
    // 当请求 /react/api 路径下面的 接口（默认会被请求本地localhost的8080端口）时，将其代理转发到 http://www.dell-lee.com 这台服务器上
    // 对于简单的请求转发，可以使用字符串的形式：'/react/api': 'http://www.dell-lee.com'
    // 如果有多个请求路径，这样设置：context: ['/auth', '/api'], target: 'http://www.dell-lee.com'
    // 另外，如果想要对根目录进行 请求转发，不能直接写 '/'，需要在devServer 下面一级 配置 index: ''
    '/react/api': {
      // 设置 转发目标
      target: 'http://www.dell-lee.com',
      // 如果转发的目标是一个 https 的网址，需要配置secure
      secure: false,
      // 将 header.json 接口的请求，转发到 demo.json 接口上，这样做是为了不修改 业务代码中的接口，
      // 而通过配置 将其转发，从而实现修改。这样做是为了避免修改业务代码。
      // 同时，由于 devServer 只在开发环境下生效，生产环境下 接口还是会请求header.json 接口
      pathRewrite: {
        'header.json': 'demo.json'
        // {'^/api', ''} 如果配置是这样，表示以 /api **开头的** 接口都 干掉（拦截掉？抛弃掉？）
      },
      bypass: function (req, res, proxyOptions) {
        	// 如果请求地址是一个 html 下的一个地址的话，则返回 index.html 路径下的内容，
        	// 如果最后返回return false，表示跳过转发，不做代理
          if (req.headers.accept.indexOf('html') !== -1) {
            console.log('Skipping proxy for browser request.');
            return '/index.html';
          }
      },
      // 有些网站回对 origin 有限制（为了防止爬虫），在做接口代理时会拿不到接口的内容，
      // 这时候可以设置 changeOrigin，可以突破对 origin 的限制。建议默认加上
      changeOrign: true,
      // 可以配置请求头的信息
      headers: {
        // 修改请求头
        host: 'www.dell-lee.com',
        // 修改 cookie
        cookie: '...',
      }
    }，
  }
}
```

devServer使用了 [http-proxy-middleware](https://github.com/chimurai/http-proxy-middleware)，可详细了解

关于使用devServer解决跨域的问题，还可以参考：[webpack开发配置API代理解决跨域问题-devServer](https://segmentfault.com/a/1190000016199721)



#### WebpackDevServer 单页面应用路由

当你在开发时，访问 localhost:8080/foo 的页面时，后端（当前是webpackDevServer）会以为访问的是后端 foo 页面，所以页面会出现 Cannot GET /foo。这是前端做 SPA 时常会出现的问题，可以通过webpack 配置解决：

```js
// webpack.config.js
module.exports = {
  devServer: {
    historyApiFallback: true
  }
}
```

这时可以解决问题。这个配置的原理是：当后端发现 没有 /foo 的页面时，都会将 对 /foo页面的请求转换为<font color=FF0000> 对根路径</font>的请求。

**historyApiFallback 还有更多用法：**

```js
devServer: {
  historyApiFallback: {
    // 这个historyApiFallback.rewrite 和 pathRewrite 类似
    // 按照这样的写法：historyApiFallback: true 等价于 rewrites: [{from: /\.*\/}, to: '/index.html'] 任意的 from 到 index.html
    rewrites: [{
      // 当访问的是abc.html，访问的将转换为index.html
      from: /abc.html/, 
      to: '/index.html' 
    }]
  }
}
```

historyApiFallback 的底层 是由 [connect-history-api-fallback](https://github.com/bripkens/connect-history-api-fallback)  实现。在这里可以发现 rewrite 的 to 可以以 函数方式实现。

同样，由于 historyApiFallback 是 devServer 中的配置，仅仅在 开发环境中有效，在生产环境中无效，这时 在生产环境中，前端无法直接解决该问题；需要后端在服务器（Apache、Nginx）上 进行配置



#### ESLint 在 webpack 中的配置

ESLint 在要使用，需要先安装 npm install；同时还需要 做一个配置文件，通过 npx eslint --init 生成。此时 项目的文件夹中会多出一个 .eslintrc.js 的文件。

在 ESLint 初始化好了之后，如果想要，查看某个文件 / 文件夹是否满足 ESLint 的规范，输入命令：npx eslint src ；此时，如果代码不符合规范，将会把不符合规范的代码信息输出。

```js
// .eslintrc.js

module.exports = {
  // 默认使用 airbnb 公司的 ESLint 规范
  "extends": "airbnb",
  // 指定解析器，便于 解析 eslint 默认不认识的代码；从而给出更准确的信息与提示。
  "parser": "babel-eslint",
	// 自定义规则，可以覆盖 Airbnb 规范中的规则
  "rules": {
    // 配置项
    "react/prefer-stateless-function": 0,
  }
}
```

同时，还有更好的方法是：在编辑器中使用 ESLint 的插件，如果代码不符合规范，将会自动的标红和报错。但是无法保证每个 开发人员的编辑器中都有 这个 插件。所以这时 <font color=FF0000> 可以</font>（为什么说是 “可以”，因为有更好的 git hook 和 eslint 的结合；见下面） 在webpack 中配置 eslint。

在配置之前，需要先安装 eslint-loader。然后进行配置：

```js
// webpack.config.js
module: {
  rules: [{
    test: /\.js$/,
    exclude: /node_modules/,
    // 另外，由于 use 的顺序是 由后到前的，所以 eslint-loader 先处理，babel-loader 后处理。
    // 因为只有先判断代码是否规范（eslint-loader），而后再打包（babel-loader）
    // 另外，eslint-loader 也可以写在 babel-loader 前面，不过，要在eslint-loader的配置对象中加一个配置：force: 'pre'，表示强制先执行
    use: ['babel-loader', 'eslint-loader']
  }]
}
```

可以让 ESLint 的报错信息显示在 浏览器页面上，从而让 问题 变得更加直观（容易被看见）。可以在 devServer 中配置 overlay: true。

```js
// webpack.config.js
devServer: {
  overlay: true
}
```

效果如下图：

<img src="https://i.loli.net/2021/09/18/8s4j3XCfmEl7Lr6.png" alt="image-20210918162352519" style="zoom: 40%;" />

类似的：Vue CLI 中自动配置了 webpack 的 overlay，所以自动实现了。

**更多的关于 eslint-loader 的配置：**

- **fix：**如果配置 fix: true 则有些小的问题，eslint 将会自动帮你修复。

  ```js
  // webpack.config.js
  use: ['babel-loader', {
    loader: 'eslint-loader',
    options: {
      fixed: true
    }
  }]
  ```

- **cache：**将会降低 在打包时对于性能的损耗。（因为做了缓存？）

最后，一般在公司，不会使用 eslint-loader，而是使用 git hook，在 git 提交前，会对代码（src的代码）进行 eslint 的检测（npx eslint src），如果存在代码不符合规范，而禁止提交。



#### webpack 如何提升打包速度

- **跟上技术的迭代（使用新版本的webpack、Node、npm、yarn）**
  - 每一个新版本的webpack都会对打包速度进行优化
  - webpack 是基于 Node 之上的，使用新版本的 Node，则 Node 的运行效率会得到提升，反过来提升 webpack 的运行速度
  - 安装新版本的 npm 和 yarn，模块之间如果相互引用，新的包管理工具可以更快地帮助我们分析包依赖，或者做包的引入；间接提升 webpack 的打包速度
- **在尽可能少的模块上应用 Loader**

- **plugin 尽可能精简 且 确保可靠**
  - 比如在 开发环境下，没有必要使用对代码进行压缩；因为不需要考虑用户的加载速度问题，所以没必要做代码压缩，同时可以减少打包时间。
  - 另外，尽可能使用 webpack 官方网站上推荐的插件，这些插件的性能往往经过了官方网站的测试，是比较快的。而使用自己写的，或者不知名的第三方插件时，往往性能得不到保证，会造成打包速度变慢。

#### 打包性能优化 文档补充

##### 通用（ dev 和 prod 皆可 ）的最佳实践

###### Stay Up to Date 保持更新

<font color=FF0000>Use the latest webpack version</font>. We are always making performance improvements.  

<font color=FF0000>Staying up-to-date with **Node.js** can also help with performance</font>. On top of this, <font color=FF0000>keeping your package manager ( e.g. `npm` or `yarn` ) up-to-date can also help</font>. Newer versions create more efficient module trees and increase resolving speed.

###### Loaders

Apply loaders to the minimal number of modules necessary（ loader 中使用 includes、exclude ） .

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        include: path.resolve(__dirname, 'src'),
        loader: 'babel-loader',
      },
    ],
  },
};
```

###### Bootstrap 启动程序

**注：**Bootstrap 即：引导程序、启动程序

**Each additional loader/plugin has a bootup time**. <font color=FF0000>Try to use **as few tools as possible**</font>.

###### Resolving

**The following steps can increase resolving speed:**

- Minimize the number of items in `resolve.modules` , `resolve.extensions` , `resolve.mainFiles` , `resolve.descriptionFiles`

  , <font color=FF0000>as they increase the number of filesystem calls</font>.

- Set `resolve.symlinks: false` if you don't use symlinks (e.g. `npm link` or `yarn link` ). **注：**symlink 即 “软连接”

- Set `resolve.cacheWithContext: false` if you use custom resolving plugins , that are not context specific.

###### Dlls

Use the `DllPlugin` to move code that is changed less often into a separate compilation（ 可参考 [[#使用 DllPlugin 提高打包速度]]，即对第三方库进行单独编译，动态链接）. This will improve the application's compilation speed , <mark>although it does increase complexity of the build process</mark>.

###### Smaller = Faster

<font color=FF0000>Decrease the total size of the compilation to increase build performance</font>. Try to keep chunks small.

- Use fewer/smaller libraries.
- Use the `SplitChunksPlugin` in Multi-Page Applications.
- Use the `SplitChunksPlugin` in `async` mode in Multi-Page Applications.
- Remove unused code. **注：**Code Spliting ？
- Only compile the part of the code you are currently developing on.

###### Worker Pool

The `thread-loader` can be used to offload expensive loaders to a worker pool（译文：`thread-loader` 可以将非常消耗资源的 loader 分流给一个 worker pool）.

> **Warning ⚠️** : <font color=FF0000>Don't use too many workers</font> , as there is a boot overhead（启动开销） for the Node.js runtime and the loader. Minimize the module transfers between worker and main process. IPC is expensive.

###### Persistent cache

Use [`cache`](https://webpack.js.org/configuration/cache) option in webpack configuration. Clear cache directory on `"postinstall"` in `package.json`. **注：**没搞懂

###### Progress plugin

It is possible to <font color=FF0000>shorten build times by removing `ProgressPlugin`</font> from webpack's configuration. Keep in mind, `ProgressPlugin` might not provide as much value for fast builds as well , so make sure you are leveraging the benefits of using it.

##### 开发环境

###### Incremental Builds 增量编译

Use webpack's watch mode. Don't use other tools to watch your files and invoke webpack. The built-in watch mode will keep track of timestamps and passes this information to the compilation for cache invalidation.

In some setups, watching falls back to polling mode. With many watched files , this can cause a lot of CPU load. In these cases, you can increase the polling interval with `watchOptions.poll` .

###### Compile in Memory

<font color=FF0000>The following utilities **improve performance by compiling and serving assets in memory**</font> rather than writing to disk:

- webpack-dev-server
- webpack-hot-middleware
- webpack-dev-middleware

###### stats.toJson speed

<mark>Webpack 4 outputs a large amount of data with its `stats.toJson()` by default</mark>（ **注：**stats 是 statistics 的简写 ）. Avoid retrieving portions of the `stats` object unless necessary in the incremental step. `webpack-dev-server` after v3.1.3 contained a substantial performance fix to minimize the amount of data retrieved from the `stats` object per incremental build step.

###### Devtool

Be aware of the performance differences between the different `devtool` settings.

- <font color=FF0000>`"eval"` has the best performance</font>, but <font color=FF0000>doesn't assist you for transpiled code</font>.
- The `cheap-source-map` variants are more performant if you can live with the slightly worse mapping quality.
- Use a `eval-source-map` variant for incremental builds.

> 💡 **Tip** : <font color=FF0000>**In most cases , `eval-cheap-module-source-map` is the best option**</font>.

###### Avoid Production Specific Tooling

<font color=FF0000>Certain utilities, plugins, and loaders **only make sense** when building **for production**</font>. For example, it usually <font color=FF0000>doesn't make sense to minify and mangle</font>（压碎） <font color=FF0000>your code with the `TerserPlugin`</font> （就是 Terser-webpack-plugin ）<font color=red>while in development</font>. These tools should typically be excluded in development :

- TerserPlugin
- `[fullhash]` / `[chunkhash]` / `[contenthash]`
- AggressiveSplittingPlugin
- AggressiveMergingPlugin
- ModuleConcatenationPlugin

###### Minimal Entry Chunk

Webpack only emits updated chunks to the filesystem. For some configuration options, ( HMR , `[name]` / `[chunkhash]` / `[contenthash]` in `output.chunkFilename` , `[fullhash]` ) the entry chunk is invalidated in addition to the changed chunks.

Make sure the entry chunk is cheap to emit by keeping it small. The following configuration creates an additional chunk for the runtime code, so it's cheap to generate:

```js
module.exports = {
  // ...
  optimization: {
    runtimeChunk: true,
  },
};
```

###### Avoid Extra Optimization Steps

Webpack does extra algorithmic work to optimize the output for size and load performance. These optimizations are performant for smaller codebases, but can be costly in larger ones:

```js
module.exports = {
  // ...
  optimization: {
    removeAvailableModules: false,
    removeEmptyChunks: false,
    splitChunks: false,
  },
};
```

###### Output Without Path Info

<mark>Webpack has the ability to generate path info in the output bundle</mark>. However, this puts garbage collection pressure（造成垃圾回收的压力） on projects that bundle thousands of modules. Turn this off in the `options.output.pathinfo` setting :

```js
module.exports = {
  // ...
  output: {
    pathinfo: false,
  },
};
```

###### Node.js Versions 8.9.10-9.11.1

<font color=FF0000>There was a [performance regression](https://github.com/nodejs/node/issues/19769) in Node.js versions 8.9.10 - 9.11.1 in the ES2015 `Map` and `Set` implementations</font>. <font color=FF0000>Webpack uses those data structures</font> （即 Map 和 Set ）<font color=FF0000>liberally</font>（大量地）, so this regression affects compile times.

Earlier and later Node.js versions are not affected.

###### TypeScript Loader

To improve the build time when using `ts-loader` , use the `transpileOnly` loader option . On its own, this option turns off type checking . To gain type checking again , use the [`ForkTsCheckerWebpackPlugin`](https://www.npmjs.com/package/fork-ts-checker-webpack-plugin). This speeds up TypeScript type checking and ESLint linting by moving each to a separate process.

```js
module.exports = {
  // ...
  test: /\.tsx?$/,
  use: [
    {
      loader: 'ts-loader',
      options: {
        transpileOnly: true,
      },
    },
  ],
};
```

##### 生产环境

> ⚠️ **Warning** : **Don't sacrifice the quality of your application for small performance gains!** Keep in mind that optimization quality is, in most cases, more important than build performance.

##### Specific Tooling Issues 工具相关问题

The following tools have certain problems that can degrade build performance（降低构建性能） :

###### Babel

Minimize the number of preset/plugins

###### TypeScript

- Use the `fork-ts-checker-webpack-plugin` for typechecking <font color=FF0000>**in a separate process**</font>.
- <font color=FF0000>Configure loaders to skip typechecking</font>.
- Use the `ts-loader` in `happyPackMode: true` / `transpileOnly: true`.

###### Sass

<font color=FF0000>**`node-sass` has a bug**</font> which blocks threads from the Node.js thread pool. <font color=FF0000>When using it with the `thread-loader` set `workerParallelJobs: 2` </font>.

摘自：[webpack 文档 - Guide - Build Performance](https://webpack.js.org/guides/build-performance)



#### resolve 参数合理配置

有的时候，在使用 ES Module 的引用 ( import ) 文件时，我们不想写扩展名，比如：

```js
import foo from './foo'
```

这时候，webpack 是不知道 foo 文件到底是 什么扩展名的，便会报错。这时，可以在webpack 中配置：

```js
// webpack.config.js
resolve: {
  extensions: ['.js', '.jsx', 'json']
}
```

webpack 将会一次以 extensions 中的扩展名，去分别匹配 / 寻找 这些文件 ( 'foo.js'、'foo.jsx'、'foo.json' )，找到之后，便成功引入。

<font color=FF0000 size=4>**但是，**</font>虽然方便，但是不建议滥用；比如将 css、jpg 之类扩展名放进 extensions 数组中，webpack 在进行查找时会调用 Node 中的文件查找模块，还是比较耗费性能的，如果过多使用，性能消耗还是很大的。所以，建议：只有逻辑代码 ( 'js'、'jsx'、'ts'、'tsx', 'vue' ) 之类的扩展名，放入 extensions 数组中。



还是在 ESM 中，有的时候，由于引入的文件 ( foo.js ) 所在的文件夹 ( fooFolder ) 下只有这一个文件，此时我们甚至不想写文件名。

```js
import 'foo' from './fooFolder/'
```

这也是可以的，只要配置 resolve：

```js
// webpack.config.js
resolve: {
  mainFiles: ['index', 'foo']
}
```

这时如果没有指定文件名，webpack 将会 依据 mainFiles 数组中的 文件名，去文件夹下寻找 对应的文件。另外，如果这项不配置，webpack 会默认地 自动寻找名为 index 的文件。

同样，如果配置 mainFiles 会带来性能上的问题。按需配置，不可滥用。



还是在ESM中，引入的文件可以使用别名（比如 foobar），但是通过 resolve.alias 依然可以让 webpack 找到别名所指向的文件 ( foo )：

```js
// webpack.config.js
resolve: {
  alias: {
    foobar: path.resolve(__dirname, '../src/foo')
  }
}
```

这项配置在 webpack 中使用还是很常见的。比如在 Vue 中，@ 表示 src 文件夹 



#### resolve 选项 和 模块解析 补充

##### 总述

**A resolver is a library** which <font color=FF0000>helps in locating a module by its absolute path</font>. A module can be required as a dependency from another module

```js
import foo from 'path/to/module';
// or
require('path/to/module');
```

The dependency module can be from the application code or a third-party library. The <font color=FF0000>resolver helps webpack find the module code that needs to be included in the bundle for every such `require` / `import` statement</font>. webpack uses [enhanced-resolve](https://github.com/webpack/enhanced-resolve) to resolve file paths while bundling modules.

##### webpack 中解析规则

使用 enchanced-resolved ，webpack 能解析三种文件路径：

- **绝对路径**

  ```js
  import '/home/me/file';
  import 'C:\\Users\\me\\file';
  ```

- **相对路径**

  ```js
  import '../src/file1';
  import './file2';
  ```

- **模块路径**

  ```js
  import 'module';
  import 'module/lib/file';
  ```
  
  <font color=FF0000>**Modules are searched for inside all directories specified in [`resolve.modules`](https://webpack.js.org/configuration/resolve/#resolvemodules)**</font>（注意 ⚠️：这个 resolve.modules 和 “配置解析文件的 loader” 的 module 选项 不是一个东西）. You can replace the original module path by an alternate path by creating an alias for it using the [`resolve.alias`](https://webpack.js.org/configuration/resolve/#resolvealias) configuration option.（比如 vue 项目中常见的 @ -> src ）
  
  - If the package contains a `package.json` file, then fields specified in [`resolve.exportsFields`](https://webpack.js.org/configuration/resolve/#resolveexportsfields) configuration options are looked up in order, and the first such field in `package.json` determines the available exports from the package according to the [package exports guideline](https://webpack.js.org/guides/package-exports/).
  
    译文：如果 package 中包含 `package.json` 文件，那么在 [`resolve.exportsFields`](https://webpack.docschina.org/configuration/resolve/#resolveexportsfields) 配置选项中指定的字段会被依次查找，`package.json` 中的第一个字段会根据 [package 导出指南](https://webpack.docschina.org/guides/package-exports/) 确定 package 中可用的 export。
  
  Once the path is resolved based on the above rule（感觉可以理解为：如果 目标文件 在上述规则中，即可被成功解析）, <font color=FF0000>**the resolver checks to see if the path points to a file or a directory**</font>. <mark style="background: aqua">If the path points to a file</mark>:
  
  - <font color=FF0000>If the path has a file extension</font>, then the file is bundled straightaway.
  - **Otherwise**, <font color=FF0000>the file extension is resolved using the [`resolve.extensions`](https://webpack.js.org/configuration/resolve/#resolveextensions) option</font>, which tells the resolver which extensions are acceptable for resolution e.g. `.js`, `.jsx`.
  
  <mark style="background: aqua">If the path points to a folder</mark>, then the following steps are taken to find the right file with the right extension:
  
  - If <font color=FF0000>**the** folder contains a `package.json` file</font>, then fields specified in [`resolve.mainFields`](https://webpack.js.org/configuration/resolve/#resolvemainfields) configuration option are looked up in order, and <font color=FF0000>the first such field in `package.json` determines the file path</font>.
  - If there is <font color=FF0000>no `package.json`</font> <font size=4>**or**</font> if the <font color=FF0000>[`resolve.mainFields`](https://webpack.js.org/configuration/resolve/#resolvemainfields) do not return a valid path</font>, <font color=FF0000>**file names specified in the [`resolve.mainFiles`](https://webpack.js.org/configuration/resolve/#resolvemainfiles) configuration option are looked for in order**</font>, to see if a matching filename exists in the imported/required directory.
  - The file extension is then resolved in a similar way using the [`resolve.extensions`](https://webpack.js.org/configuration/resolve/#resolveextensions) option.
  
  Webpack provides reasonable [defaults](https://webpack.js.org/configuration/resolve) for these options depending on your build target.

resolveLoader 拥有类似的规则，不过适用于 解析 loader

摘自：[webpack 文档 - Concept - Module Resoluation](https://webpack.js.org/concepts/module-resolution/)



#### 使用 DllPlugin 提高打包速度

在 webpack <font color=FF0000> 每次</font>打包时，<font color=FF0000> 都会</font>对第三方模块进行代码分析；而第三方模块是不会变的，所以没有必要每次都对第三方模块进行代码分析。这时我们希望：<font color=FF0000>**第三方模块只在第一次打包时，做代码分析，之后直接用第一次分析好的结果即可**</font>。

这时，可以在 build 文件夹（ webpack.config.js 所在文件夹）下，创建一个 webpack.dll.js

```js
// webpack.dll.js
const path = require('path')

module.exports = {
  mode: 'production',
  // 打包入口
  entry: {
    // 配置引入的第三方模块
    vendors: ['react', 'react-dom', 'lodash']
  },
  output: {
    // 再次提醒：这里的 name 对应的 entry 中的 vendors
    filename: '[name].dll.js',
    // 配置第三方模块打包的目标路径
    path: path.resolve(__dirname, '../dll'),
    // 同样，对应 entry 中的 vendors；这里的作用是，通过全局变量，将其暴露出去。
    library: '[name]'
  }
}
```

同时，可以创建一个 npm scripts：

```js
// package.json
"scirpts": {
  "build:dll": "webpack --config ./build/webpack.dll.js"
}
```

将打包的 dll 文件，对其生成一个全局变量（比如上面 library: [name] 生成的 vendors ），并且在 index.html 上引入（ \<script src="vendors.dll.js" \>），需要使用插件：add-asset-html-webpack-plugin

```js
// webpack.common.conf.js

const path = require('path')
const addAssetHtmlWebpackPlugin = require('add-asset-html-webpack-plugin')

module.exports = {
  plugins: [
    new addAssetHtmlWebpackPlugin({
      filepath: path.resolve(__dirname, '../dll/vendors.dll.js')
    })
  ]
}
```

这时，虽然第三方模块只打包一次，这个想法已经实现了；但是第三方模块打包生成的内容，在 webpack 打包的过程中没有去使用它（在打包时，业务代码还是在使用 node_modules 中的代码模块；而没有使用 vendors.dll.js 中打包好的的第三方模块）。所以，这时我们要 在引入第三方模块时，去使用 dll 文件引入。

这时，要在 webpack.dll.js 中做一个映射：

```js
// webpack.dll.js

const webpack = require('webpack')

// 之前有的 webpack.dll.js 中的配置，省略

module.export = {
  plugins: [
    new webpack.DllPlugin({
      name: '[name]',
      // 用 DllPlugin 分析代码库，将第三方模块之间的映射关系放到名为 [name].manifest.json 的文件下
      path: path.resolve(__dirname, '../dll/[name].manifest.json')
    })
  ]
}
```

运行 npm run build:dll，此时 dll 文件夹下会出现一个名为 vendors.manifest.json 的映射文件。所以在webpack 打包时，可以结合 之前生成的 全局变量 和 manifest映射关系 来对代码进行分析。这时候，需要使用 webpack.DllReference ，在 webpack.common.conf.js中配置：

```js
// webpack.common.conf.js

module.exports = {
  plugins: [
    // 之前写的 addAssetHtmlWebpackPlugin 配置省略
    new webpack.DllReferencePlugin({
      manifest: path.reslove(__dirname, '../dll/vendors.manifest.json')
    })
  ]
}
```

在 webpack 打包，引入第三方的模块时， 会到 vendors.manifest.json 中寻找映射关系。如果能找到映射关系，则 webpack 知道 没有必要从 node_modules 中打包进来了，直接去 vendors.dll.js 中拿即可（原理上，会从全局变量那边拿到，那里存储了打包生成的第三方模块）；如果没有找到映射关系，则还会从 node_modules 中拿到模块，去打包。

<font size=4>**Dllplugin使用的一些优化**</font>

对于 entry 中 自定义的 vendors 数组太长，是可以拆分的，比如按照模块的关系进行拆分：

```js
// webpack.dll.js
module.exports = {
  entry: {
    vendors: ['loadash'],
    react: ['react', 'react-dom']
  }
}
```

这时候运行 npm run build:dll 将会在 dll 文件夹下 会多出 react.dll.js 和 react.manifest.json 两个文件。这时，webpack.common.conf.js 也要修改，在 addAssetHtmlWebpackPlugin 和 DllReferencePlugin 中添加配置 react.manifest.json 

```js
// webpack.common.conf.js

module.exports = {
  plugins: [
		new addAssetHtmlWebpackPlugin({
      filepath: path.resolve(__dirname, '../dll/vendors.dll.js')
    }),
    // 新增的 react.dll.js 配置
    new addAssetHtmlWebpackPlugin({
    	filepath: path.resolve(__dirname, '../dll/react.dll.js')
    }), 
    new webpack.DllReferencePlugin({
      manifest: path.reslove(__dirname, '../dll/vendors.manifest.json')
    }),
    // 新增的 react.manifest.json 配置
    new webpack.DllReferencePlugin({
      manifest: path.resolve(__dirname, '../dll/react.manifest.json')
    })
  ]
}
```

<font color=FF0000 size=4> **但是**</font>，在大型项目中，引入的第三方模块会很多，同样 webpack.dll.js 中 entry 的入口 数组也很多，如果一个一个 在 webpack.common.conf.js 中 配置 addAssetHtmlWebpackPlugin 和 DllReferencePlugin，将会非常麻烦；可以使用脚本动态的添加配置，将原先的 plugins 数组删掉，在 module.exports 上面添加变量 const plugins，使用 Node 的 FS 模块在 dll 文件夹下，自动、动态地进行匹配（正则） 和 添加：

```js
// webpack.common.conf.js

const fs = require('fs')

// plugins 的初始值是非 addAssetHtmlWebpackPlugin 和 DllReferencePlugin 的配置
const plugins = [
  new HtmlWebpackPlugin({
    template: 'scr/index.html'
  }),
  new CleanWebpackPlugin(['dist'], {
    root: path.resolve(__dirname, '../')
  })
]

// 用 Node 的 fs 模块读取 dll 文件夹下的内容，返回的 files 变量 是一个数组，里面是 dll 文件夹下的文件名
const files = fs.readdirSync(path.resolve(__dirname, '../dll'))
files.forEach(file => {
  // 正则匹配
  if(/.*\.dll.js/.test(file)) {
    plugins.push(new AddAssetHtmlWebpackPlugin({
      filepath: path.resolve(__dirname, '../dll', file)
    }))
  }
  if(/.*\.manifest.json/.test(file)) {
    plugins.push(new webpack.DllReferencePlugin({
      manifest: path.resolve(__dirname, '../dll', file)
    }))
  }
})

module.exports = {
  // ES6 语法，等价于 plugins: plugins
  plugins,
}
```

这样写，entry 中即使有很多个 数组，webpack 会自动的做好引入。

以上，现在只需要在第一次打包时，运行 npm run build:dll 分析第三方模块中的代码，并做好全局变量暴露 和 对应关系；后面的打包只需要运行 npm run build 即可 很好地加快打包速度。



#### webpack 打包优化的其他点

- 控制包的大小
  - 对于引用但没有使用的第三方模块，使用 Tree-Shaking；设置直接不去引用它。这样便可以控制打包的大小，从而提高打包速度
  - 通过插件，对代码进行拆分，将大模块拆分成小模块，进行打包处理。
- webpack 默认通过Node来进行打包，所以它是一个单线程的打包工具。所以我们可以通过使用 thread-loader, parallel-webpack, happypack 进行多进程打包
  - thread-loader 和 happypack 可以用到 Node 中的多进程，利用多个 CPU 对项目进行打包。使用几个 CPU，可以通过做项目自己总结。
  - 在做多页打包时，还可以借助 happypack，从而对多个页面<font color=FF0000> 一起</font>进行打包。不必页面等待另一个页面打包好了，再打包了。
- 合理使用 sourceMap，关于sourceMap 信息全面与否，与打包速度之间进行权衡。
- 结合 stats 分析打包结果。
- 开发环境内存编译（内存速度快）
- 开发环境的无用插件剔除（比如代码压缩，这时没有意义的，而且会增加打包时间）



#### 多页面打包配置

截止现在，我们都是在对单页面（即dist 文件见下只有一个 html ，Vue 和 React 都是 SPA）进行打包。但是在一些特殊场景下，比如：兼容 jquery、zepto 的老的项目，要对多页面进行打包。所以下面是多页面的打包配置：

```js
// webpack.config.js

module.exports = {
  entry: {
    // 之前 单页面应用时的首页
    main: './src/index.js',
    // 现在，多页面应用添加的页面
    list: './src/list.js'
  }
}
```

这时候 dist 文件夹下 出现了main.js 和 list.js 打包之后 生成的两个js文件，但是 html 还是只有一个，且这个 html 页面（index.html）中将两个生成的 js 文件都引入了，而我们只想只引入一个（即：应该生成两个 html 页面，同时，每个html 页面各引入一个 打包生成的 js 文件）。这时要对 HtmlWebpackPlugin 的配置进行修改。

```js
// webpack.config.js

// 这是使用 DllPlugin 之后，将plugins作为变量抽离出来的版本
const plugins = [
  new HtmlWebpackPlguin({
    // 模版文件 配置不变
    template: 'src/index.html',
    // 添加 filename 参数，以生成两个 html 文件
    filename: 'index.html',
    // 指定该 html 文件需要引入的打包生成的文件有哪些；这样设置，index.html 将引入 main.js，而不会引入 list.js 文件
    // 这里的 runtime 是运行时代码，是必须要引入的。
    chunks: ['runtime', 'vendors', 'main']
  }),
  new HtmlWebpackPlugin({
    template: 'src/index.html',
    // 添加 filename 参数，以生成两个 html 文件
    filename: 'list.html',
    // list.html 将引入 list.js，而不会引入 main.js 文件
    chunks: ['runtime', 'vendors', 'list']
  })
]
```

如果多页面有很多的话，每有一个页面，就需要添加一个 HtmlWebpackPlugin，这样将非常麻烦。和DllPlugin 那边一样，这些东西可以通过代码自动添加

```js
// webpack.common.conf.js

// 抽离出来的 plugins
const plugins = []
...

const makePlugins = (configs) => {
  // plguins 变量的初始化
  const plugins = [
    new CleanWebpackPlugin(['dist', {
      root: path.resolve(__dirname, '../')
    }])
  ]
  
  Object.keys(configs.entry).forEach(item => {
    plugins.push(
      new HtmlWebpackPlugin({
      	template: 'src/index.html',
	      filename: `${item}.html'`,
  	    chunks: ['runtime', 'vendors', item]
    	})
    )
  })
  
  // 处理DllPlugin这部分的的代码。这部分代码完全照搬的，原理见上面 DllPlugin
  const files = fs.readdirSync(path.resolve(__dirname, '../dll'))
	files.forEach(file => {
	  if(/.*\.dll.js/.test(file)) {
  	  plugins.push(new AddAssetHtmlWebpackPlugin({
    	  filepath: path.resolve(__dirname, '../dll', file)
    	}))
  	}
  	if(/.*\.manifest.json/.test(file)) {
    	plugins.push(new webpack.DllReferencePlugin({
      	manifest: path.resolve(__dirname, '../dll', file)
	    }))
  	}
	})
  
  return plugins
}

// 将配置抽离出来，变成变量
const configs = {
  entry: { ... },
  resolve: { ... },
  module: { ... },
  optimization: { ... },
  preformance: false,
  output: { ... }
}

configs.plugins = makePlugins(configs)
           
module.exports = configs
```



#### webpack 中的 CSP

<font color=FF0000>Webpack is capable of adding a `nonce` to all scripts that it loads</font> （**译文：**Webpack 能够为其加载的所有脚本添加 `nonce` 。nonce 即 number once，是加密通信只能使用一次的（随机）数字 ）. <font color=FF0000>To activate this feature, **set a `__webpack_nonce__` variable and include it in your entry script**</font> . <font color=fuchsia>A unique hash-based `nonce` will then be generated and provided for each unique page view</font> ( this is why `__webpack_nonce__` is specified in the entry file and not in the configuration). Please note that <font color=FF0000>the `__webpack_nonce__` **should always be a base64-encoded string**</font>.

##### Examples

In the entry file:

```js
// ...
__webpack_nonce__ = 'c29tZSBjb29sIHN0cmluZyB3aWxsIHBvcCB1cCAxMjM=';
// ...
```

##### Enabling CSP

Please note that <font color=FF0000>**CSPs are not enabled by default**</font>. A corresponding header `Content-Security-Policy` or meta tag `<meta http-equiv="Content-Security-Policy" ...>` needs to be sent with the document to instruct the browser to enable the CSP. Here's <mark>an example of what a CSP header including a CDN white-listed URL might look like</mark>:

```http
Content-Security-Policy: default-src 'self'; script-src 'self'
https://trusted.cdn.com;
```

##### Trusted Types

Webpack is also capable of using Trusted Types to load dynamically constructed scripts, to adhere to CSP [`require-trusted-types-for`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/require-trusted-types-for) directive restrictions. See [`output.trustedTypes`](https://webpack.js.org/configuration/output/#outputtrustedtypes) configuration option. （**译文：**webpack 还能够使用 Trusted Types 来加载动态构建的脚本，遵守 CSP require-trusted-types-for 指令的限制。可查看 output.trustedTypes 配置项 ）**注：**没怎么看懂

注：[webpack 文档 - Guide - Content Security Policies](https://webpack.js.org/guides/csp/)



#### 开发 使用 Vagrant

##### 总述

If you <font color=FF0000>have a more advanced project</font> and <font color=FF0000>use [Vagrant](https://www.vagrantup.com/) to run your development environment in a Virtual Machine</font>（译文：使用 Vagrant 来实现在虚拟机 ( Virtual Machine ) 上运行你的开发环境）, you'll often want to also <font color=FF0000>**run webpack in the VM**</font>.

**注：**Vagrant 是一个开发的虚拟环境，类似于 docker。由于目前没有使用到，所以一扫而过，暂时略。不过，下面说了 webpack-dev-server 的部分原理：

`webpack-dev-server` will <font color=FF0000>**include a script**</font> in your bundle that <font color=FF0000 size=4>**connects to a WebSocket** to **reload when a change in any of your files occurs**</font>

摘自：[webpack doc - Guides - Development Vagrant](https://webpack.js.org/guides/development-vagrant/)







#### 如何编写一个 Loader

在webapck 中打包一个类型文件或者是一个模块时，这时 Loader 开始发生作用。

实现一个 简单的 Loader 很简单，比如：修改（替换）<font color=FF0000> 源代码</font>中某一个特定的字符串；关于这个功能，实现一下：

在项目中创建一个 Loader 文件夹，在文件夹下创建一个 replaceLoader.js

```js
// replaceLoader.js

// 这里注意，函数中要使用 this，webpack 在使用 Loader 时会对 this 进行一些变更，变更之后才能用 this 中的一些方法
// 而这里如果用箭头函数，this 指向将会有问题，将无法调用方法；所以不能在用箭头函数替代这里的 function
// 这里的参数 source 是引入文件的 **源代码**，这里 Loader 对它进行一些处理，最后将其返回回去。
module.exports = function(source) {
  // 将代码中的 foo 替换成 foobar
  return source.replace('foo', 'foobar')
}
```

在 webpack 中使用自定义的 Loader：

```js
// webpack.config.js

const path = require('path')

module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js'
  },
  module: {
    rules: [{
      test: /\.js/,
      use: [path.resolve(__dirname, './loader/replaceLoader.js')]
    }]
  },
  output: {
    path: path.resolve(__dirname, 'dist')
    filename: '[name].js'
  }
}
```

假设 有index.js，内容为：

```js
console.log('foo bar')
```

打包之后的结果 main.js 文件中，将会被改成：console.log('foobar bar')。



在上面webpack.config.js 中use 的使用比较简单，还可以 在use 数组中使用对象：

```js
// webpack.config.js
rules: {
  test: /\.js/,
  use: [{
    loader: path.resolve(__dirname, './loader/replaceLoader.js'),
    // 配置的options 中的内容，可以被 replaceLoader 通过 this.query 接收到
    options: {
      name: 'hello'
    }
  }]
}
```

这里面的 options 将可以被 replaceLoader 通过 this.query 接收到：

```js
module.exports = function() {
  return source.replace('foo', this.query.name)
}
```

这时候打包，代码将会被变成： console.log('hello bar')

由于options 可以传的东西 很多，甚至很奇怪，所以官方推荐使用 loader-utils，可以使用 loader-utils 的 getOptions 方法来获取内容<font size=4 color=FF0000>**（在webpack 5中，官方实现了this.getOptions(schema)，用来替代 loader-utils 的 getOptions方法 ）**</font>



在Loader API 中有一个 this.callback，语法如下：

```js
this.callback(
  err: Error | null,
  content: string | Buffer, // 需要回传的内容，相当于 return
  sourceMap?: SourceMap, // 可选，sourceMap
  meta?: any // 可选
);
```

这是一个回调，第二个参数可以实现 return 的功能，但是 加上其他参数，可以实现比 return 多的功能



在 Loader 中如果要实现异步，如果直接使用，会报错；这时，需要使用 this.async 方法。示例如下：

 ```js
 module.exports = function(source) {
   
   // 声明存在异步
   const callback = this.async()
   setTimeout(() => {
     const result = source.replace('foo', this.options.name)
     // 这里调用的实际上是 this.callback
     callback(null, result)
   }, 1000)
 }
 ```

如果不想要在 use 的 loader 中使用 path.resolve(__dirname, 'yourLoader')，而直接 使用 'yourLoader'，需要做下配置：

```js
// webpack.config.js

module.exports = {
  resolveLoader: {
    // './loaders' 表示自己代码中编写的 loaders
    // 这个配置的意思是：当使用 loader 时，会先去 node_modules 目录下去寻找，如果没有会去 loaders 目录下寻找
    modules: ['node_modules', './loaders']
  },
  module: {
    rules: [{
      test: /.\js/,
      user: [{
        // 这里即可以不使用 path.resolve 了
        loader: 'replaceLoader'
      }]
    }]
  }
}
```

自己写 Loader 的一些场景：

- 不需要直接修改源代码，可以在 Loader 中通过对 source 的修改，即可完成一些补充。

  比如没有写try...catch的代码加上 try...catch。

- 根据不同的场景 / 环境，对同一个参数进行修改。

  比如国际化，对同一个 标记，根据不同的语言，进行不同的对应



#### 如何编写一个 Plugin

plugin 是在打包时，对于打包的某些时刻，帮助我们做一些事情。

如果查看 webpack 的源代码，可以发现 webpack 中 80% 的代码都是 plugin，可以说 plugin 是 webpack 的灵魂。

下面，自己做一个 plugin：在打包完后，在dist文件夹下生成一个版权文件。在项目中创建一个 plugins 文件夹，文件夹下创建一个 copyright-webpack-plugin.js 文件。

```js
// copyright-webpack-plugin.js

class CopyRightWebpackPlugin {
  // options 用于接收 生成对象时添加的对象参数
  constructor(options) {
    
  }
  
  // compiler 是 webpack 的一个实例，该实例中存储了webpack 中存储了webpack中各种配置文件，打包过程
  apply( compiler ) {
    // emit 是一个 compiler.hooks，同时也是一个异步的钩子
    // compilation 存放的是与这次打包的所有内容，cb 是 callback
    // 如果调用的是 tapAsync 函数，必须要在函数最后调用一下 callback
    compiler.hooks.emit.tapAsync('CopyrightWebpackPlugin', (compilation, cb) => {
			// compilation 中有一个 assets 属性，表示打包内容有哪些
      // 这里的 copyright.txt 表示：向compilation.assets 中添加一个 copyright.txt 文件
      compilation.assets['copyright.txt'] = {
        // 指定生成 copyright.txt 文件的内容
        source: function() {
          return 'copyright by foobar'
        },
        // 表示文件大小
        size: function() {
          return 19
        }
      }
      cb()
    })
    // 上面的代码，就已经让添加 copyright 的功能实现了
    // 下面的代码，是在补充一个 同步勾子的使用，与 copyright 的功能没有关系
    
    // compiler.compile 是一个同步的钩子，使用 tap 方法；另外，参数没必要传 callback 了
    compiler.hooks.compile.tap('CopyrightWebpackPlugin', compilation => {
      ...
    })
  }
}

module.exports = CopyrightWebpackPlugin;
```

在 webpack.config.js 中使用该插件：

```js
// webpack.config.js
const CopyrightWebpackPlugin = require('./plugins/copyright-webpack-plugin')

module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js'
  },
  plugins: [
    // 由上面的定义可知，之所以使用插件时，要用 new，是因为它就是一个类
    // 插件中可以添加对象，插件的实现类通过构造器（constructor）的 options 对象获取
    new CopyrightWebpackPlugin({})
  ],
  outpath: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js'
  }
}
```

打开webpack 官方文档，compiler hooks 相关 https://webpack.js.org/api/compiler-hooks/，发现 hooks 有很多种；具体见 文档（前面的链接）。

在写 webpack plugin 时，可以使用 Node 的调试工具 来查看代码中的内容（debug），可以写一条 npm scripts：

```json
// package.json

"scripts": {
  // 运行 node node_modules/webpack/bin/webpack.js 和 运行 npx webpack 命令，干的事情是一样的
  // 添加 node 参数 --inspect --inspect-brk。
  // --inspect 表示要开启node的调试工具，--inspect-brk 表示在运行 webpack 时，
  // 在第一行上打一个断点。这时运行 webpack，进入浏览器，点击Node图标，则可以进入 debug 模式。
  // 如果想要对其他行做 debug 时，可以在源代码上想要打断点的代码前一行，加上 debugger;
  "debug": "node --inspect --inspect-brk node_modules/webpack/bin/webpack.js"
}
```



#### bundler 源码编写

babel/parser 可以分析代码文件，了解文件的信息（比如 AST）。在实现 bundler时，可以借助 babel/parser 来获得 AST

babel/traverse 可以帮助了解文件中 代码的引入，加入代码中有很多引入的话，都可以很好的显示出来。

由于 import / export 是 ES6 Module 的语法，所以还需要借助 babel/core 中的 transformFromAst 方法，将AST转化成可以运行的代码；在该方法的第三个参数是options，还需要 babel/preset-env 。

此时，便将入口文件的所有代码转换为浏览器可以识别的ES5，并得到了它所引入的所有文件



脚手架工具可以理解为是一个 Node 程序，帮助用户初始化项目，包含了该项目的工程目录，和这个项目打包的配置

module.exports 中的 bail 配置的作用是，一旦打包出现错误，则停止。



---

### webpack 5 特性

#### 模块联邦

##### 《模块联邦浅析》 摘抄

###### 业务场景

假设公司有个业务集群，<font color=FF0000>**公共业务组件库升级了，希望能够尽可能少得影响业务线，仅仅在基础组件库版本升级即可全业务线升级**</font>，那么<font color=FF0000>**可以考虑使用模块联邦来实现**</font>。

**模块联邦 和 利用 npm 发包来实现的方案的区别在于**：<font color=FF0000>npm 发布的组件库从 1.0.1 升级到 1.0.2 的时候，必须要把业务线项目重新构建，打包，发布才能使用到最新的特性</font>；而<font color=fuchsia>**模块联邦可以实现实时动态更新而无需打包业务线项目**</font>。

###### 效果

大致的原型图如下：

<img src="https://s2.loli.net/2022/07/16/8evp1dVaJNBTow3.png" alt="img" style="zoom: 90%;" />

我们看到：project1 的 home 页的 specialItem，project2 的 about 页的 searchItem 组件被用于 project2 的 home 中， project2 的 about 直接用的 project1 的 about 页。

[项目](https://github.com/AshesOfHistory/vue3-cli-module-federation-demo) 运行示意效果图如下

![img](https://s2.loli.net/2022/07/16/iIejHmBqwfWcL7N.jpg)

###### 配置

**app-exposes 的 vue.config.js 配置：**

<img src="https://s2.loli.net/2022/07/16/DocwUb2igXVIlqf.png" alt="img" style="zoom: 33%;" />

**app-general 的 vue.config.js 配置：**

<img src="https://s2.loli.net/2022/07/16/A892pwVxSbYDXTL.png" alt="img" style="zoom:33%;" />

可以看到，总体上我们用到了 webpack 原生的插件 ModuleFederationPlugin 来实现模块联邦的效果的。

在首页中，我们异步引用的 app-exposes 提供的 SearchItem 以及 SpecialItem 组件。

<img src="https://s2.loli.net/2022/07/16/GwlRXTObmUK13BV.png" alt="img" style="zoom:33%;" />

在 about 页面的路由配置中，我们直接引入的远程连接的 AboutView 页面。

<img src="https://s2.loli.net/2022/07/16/TRE3X9OsJ2NSIPj.png" alt="img" style="zoom:33%;" />

###### 联邦模块的原理分析

<font color=FF0000>联邦模块有两个主要概念：**Host**（消费其他 Remote）和 **Remote**（被 Host 消费）</font>。 <font color=FF0000>**每个项目可以是 Host 也可以是 Remote，也可以两个都是**</font>。可以通过 webpack 配置来区分，可以参考[例子](https://github.com/module-federation/module-federation-examples/tree/master/bi-directional)。

- 作为 Host 需要配置 remote 列表和 shared 模块
- 作为 Remote 需要配置项目名 ( name ) ，打包方式 ( library )，打包后的文件名 ( filename )，提供的模块 ( exposes )，和 Host 共享的模块 ( shared )。

###### ModuleFederationPlugin 的原理

源码中 ModuleFederationPlugin 主流程 主要做了三件事：

- 通过参数是否配置 shared 来判断是否使用共享依赖 SharePlugin 模块。
- 通过参数是否配置 exposes 来判断是否使用公开 ContainerPlugin 模块。
- 通过参数是否配置 remotes 来判断是否使用 ContainerReferencePlugin 引用模块。

###### **webpack5 模块联邦对异步模块加载的处理**

- 下载并执行 remoteEntry.js，挂载入口点对象到 window.app-exposes，他有两个函数属性，init 和 get。init 方法用于初始化作用域对象 initScope，get 方法用于下载 moduleMap 中导出的远程模块。
- 加载 app-exposes 到本地模块。
- 创建 app-exposes.init 的执行环境，收集依赖到共享作用域对象 shareScope。
- 执行 app-exposes.init，初始化 initScope。
- 用户 import 远程模块时调用 app-exposes.get(moduleName) 通过 Jsonp 懒加载远程模块，然后缓存在全局对象 window['webpackChunk' + appName]。
- 通过 webpack_require 读取缓存中的模块，执行用户回调。

###### 使用场景

目前模块联邦已经在微前端领域发挥了巨大的作用，也起到 webpack 能够越来越强大。

利用模块联邦强大的跨应用级模块共享能力，我们<font color=FF0000>可以搭建一个非业务的中台搭建系统，实现 app 级别的低代码搭建平台</font>，这与市场上常见页面级低代码搭建不同，<font color=FF0000>能够实现系统级能力复用的同时降低维护成本</font>。后续比如说 sso 单点登录，页面跳转，埋点，异常捕获等都可以考虑抽象封装成系统内置的方法到里面。

摘自：[模块联邦浅析](https://www.zoo.team/article/webpack-modular)

##### 《精读《Webpack5 新特性 - 模块联邦》》摘抄

**Webpack5 模块联邦让 Webpack 达到了线上 Runtime 的效果**，<font color=FF0000>**让代码直接在项目间利用 CDN 直接共享**</font>，不再需要本地安装 Npm 包、构建再发布了（ 👀 **注**：可以理解为修改公共组件之后，让使用组件的项目 不需要重新打包？）！

我们知道 <font color=FF0000>Webpack 可以通过 DLL</font> （👀 **注**：参考 [[#Dlls]]）<font color=FF0000>或者 Externals</font>（👀 **注**：参考 [[#Externalize Lodash]]） <font color=FF0000>做代码共享时 Common Chunk</font>（👀 **注**：即一个 lib 在项目中（抽离出来）只保留一份，不论它被项目直接依赖，还是作为依赖的依赖... ），但不同应用和项目间这个任务就变得困难了，我们几乎无法在项目之间做到按需热插拔。

模块联邦是 Webpack5 新内置的一个重要功能，可以让跨应用间真正做到模块共享。

###### ModuleFederationPlugin 的参数

<font color=FF0000>模块联邦本身是一个普通的 Webpack 插件 `ModuleFederationPlugin`</font> ，插件有几个重要参数：

- **name**： 当前应用名称，需要全局唯一。

- **remotes**： 可以将其他项目的 `name` 映射到当前项目中。

- **exposes**： 表示导出的模块，只有在此申明的模块才可以作为远程依赖被使用。

- **shared**：是非常重要的参数，制定了这个参数，可以让远程加载的模块对应依赖改为使用本地项目的 React 或 ReactDOM

###### 总结

模块联邦为更大型的前端应用提供了开箱解决方案，并已经作为 Webpack5 官方模块内置，可以说是继 Externals 后最终的运行时代码复用解决方案。

摘自：[精读《Webpack5 新特性 - 模块联邦》](https://zhuanlan.zhihu.com/p/115403616)

##### 官方文档摘抄

###### 背景：微前端概念（与不足）

Multiple separate builds should form a single application（**译**：多个独立的构建（子程序）可以组成一个（作为整体的）应用程序）. <font color=FF0000>These separate builds should not have dependencies between each other</font>, so <font color=FF0000>they can be developed and deployed individually</font>.

<font color=FF0000>This is often known as <font size=4>**Micro-Frontends**</font>, **but is not limited to that**</font>. 👀 **注**：照这么说有点后端“微服务”的意思了

###### 模块联邦底层概念

We distinguish between <font color=FF0000>**local**</font>（**译**：本地模块） <font color=FF0000>**and remote modules**</font>. Local modules are normal modules which are part of the current build. <font color=FF0000>Remote modules are modules that are</font> <font color=fuchsia>**not part of the current build**</font> and <font color=fuchsia>**loaded from a so-called <font size=4>*container*</font> at the runtime**</font>.

<font color=FF0000>**Loading remote modules** is considered an **asynchronous operation**</font>. When using a remote module these asynchronous operations will <font color=FF0000>be placed in the next chunk loading operation(s) that is between the remote module and the entrypoint</font>（**译**：当使用远程模块时，这些异步操作将被放置在远程模块和入口之间的下一个 chunk 的加载操作中）. It's not possible to use a remote module without a chunk loading operation（**译**：如果没有 chunk 加载操作，就不能使用远程模块）.

A chunk loading operation is usually an `import()` call, but older constructs like `require.ensure` or `require([...])` are supported as well. <font color=FF0000>A container is created through a container entry, which exposes asynchronous access to the specific modules</font>. <mark style="background: lightskyblue">**The exposed access is separated into two steps**</mark>:

1. loading the module ( asynchronous )
2. evaluating the module ( synchronous )

Step 1 will be done during the chunk loading. Step 2 will be done during the module evaluation interleaved（**译**：交错地） with other ( local and remote ) modules. This way, <font color=FF0000>evaluation order is unaffected by converting a module from local to remote or the other way around</font>（**译**：执行顺序不受模块从本地转换为远程或从远程转为本地的影响。👀 **注**：即，当前“构建”既可能有“本地模块”，也有可能有“远端模块”）.

It is possible to nest a container. Containers can use modules from other containers. Circular dependencies between containers are also possible.

###### 模块联邦高级概念

<font color=fuchsia>**Each build acts as a container**</font> and <font color=fuchsia>also **consumes other builds as containers**</font>（**译**：每个构建都充当一个容器，也可将其他构建作为容器）. This way each build is able to access any other exposed module by loading it from its container.

**Shared modules** are modules that are both overridable（**译**：可重写的） and provided as overrides to nested container. They usually point to the same module in each build, e.g. the same library.

The `packageName` option allows setting a package name to look for a `requiredVersion`（**译**：packageName 选项允许通过设置包名来查找所需的版本）. <font color=fuchsia>It is automatically inferred for the module requests by default</font> , <font color=FF0000>**set `requiredVersion` to `false` when automatic infer should be disabled**</font>.

###### 构建块 ( Building blocks )

- **ContainerPlugin** (<font color=FF0000>low level</font>) : This plugin <font color=fuchsia>**creates an additional container entry** with the specified exposed modules</font>.

- **ContainerReferencePlugin** (<font color=FF0000>low level</font>) : This plugin <font color=fuchsia>**adds specific references to containers as externals**</font>（**译**：将特定的引用添加到作为外部资源的容器中） and <font color=fuchsia>allows to import remote modules from these containers</font>. It also calls the `override` API of these containers to provide overrides to them. Local overrides (via `__webpack_override__` or `override` API when build is also a container) and specified overrides are provided to all referenced containers.

- <font color=fuchsia size=4>**ModuleFederationPlugin**</font> (high level) : [`ModuleFederationPlugin`](https://webpack.js.org/plugins/module-federation-plugin) <font color=FF0000>combines `ContainerPlugin` and `ContainerReferencePlugin`</font> .

摘自：[webpack 文档 - Concepts - Module Federation](https://webpack.js.org/concepts/module-federation) 以及 中文版 [印记中国 webpack 文档 - 概念 - Module Federation](https://webpack.docschina.org/concepts/module-federation/) 👀 **注**：后面还有内容，但看不下去了... 感觉官方文档写得太唐突了，深入但不浅出... 或许因为它被放在 Concepts 中了吧...

***

### webpack 插件介绍

##### webpack-bundle-analyzer

🔗 : https://github.com/webpack-contrib/webpack-bundle-analyzer

生成webpack打包后，包的组成的可视化页面

![webpack bundle analyzer zoomable treemap](https://i.loli.net/2021/11/02/5tOlRDib9svUoTp.gif)

类似的工具（不是webpack插件，而是网页应用）还有：[analyse tool](https://webpack.github.io/analyse/) 、 [webpack-visualizer](https://chrisbateman.github.io/webpack-visualizer/)



##### webpack-chain

🔗 : https://github.com/neutrinojs/webpack-chain

可以使用链式编程，在webpack.config.js中进行链式配置



##### mini-css-extract-plugin

🔗 : https://github.com/webpack-contrib/mini-css-extract-plugin

> This plugin extracts CSS into separate files. It creates a CSS file per JS file which contains CSS. It supports On-Demand-Loading of CSS and SourceMaps.
>
> https://github.com/webpack-contrib/mini-css-extract-plugin - README

CSS代码分割，在打包时，将css代码分为多个文件；并给出生成文件的命名规则



##### compression-webpack-plugin

🔗 : https://github.com/webpack-contrib/compression-webpack-plugin

> Prepare compressed versions of assets to serve them with Content-Encoding
>
> https://github.com/webpack-contrib/compression-webpack-plugin - READEME

该插件可以被用来将包进行压缩，默认使用 gzip 格式。



##### terser-webpack-plugin

🔗 : https://github.com/webpack-contrib/terser-webpack-plugin

也被称为 terserPlugin ，尤其在 webpack.config.js 之类的配置中，以 terserPlugin 的名字引入 ( `const terserPlugin = require('terser-webpack-plugin')` ) 。

基于 terser（一种 JS parser ）用来最小化 js 代码 ( Tree-shaking ) ，减小生产包的大小。

👀 补充：在 webpack@5 项目中 terser-webpack-plugin 被默认集成（可以在 [webpack](https://github.com/webpack/webpack) package.json 的 `dependencies` 中找到它），因为webpack 是通过它实现了 Tree-shaking，且 Tree-shaking 功能在 Production 模式下默认开启。

类似的 还有 [uglifyjs-webpack-plugin](https://github.com/webpack-contrib/uglifyjs-webpack-plugin)，它是默认集成在 webpack@4 的生产环境中的，不过已经废弃。

它们都是基于[ UglifyJS](https://github.com/mishoo/UglifyJS)



### webpack 相关问题



#### webpack 的构建流程是什么

- **初始化参数：**<font color=FF0000>解析 webpack 配置参数</font>，合并 命令行（参数）传入和 webpack.config.js 文件配置的参数，形成最后的配置结果
- **开始编译：**上一步得到的<font color=FF0000>参数初始化 compiler 对象</font>，注册所有配置的插件，插件 监听 webpack 构建生命周期的事件节点，做出相应的反应，执行对象的 run 方法开始执行编译；
- **确定入口：**<font color=FF0000>从配置的entry入口，开始解析文件构建AST语法树，找出依赖，递归下去</font>；
- **编译模块：**<font color=FF0000>递归中根据文件类型和 loader 配置</font>，调用所有配置的 loader 对文件进行转换，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理
- **完成模块编译并输出：**<font color=FF0000>递归结束后，得到每个文件结果</font>，包含每个模块以及他们之间的依赖关系，根据 entry 或 分包 配置生成代码块chunk
- **输出完成：**输出所有的chunk到文件系统

#### webpack 的热更新原理

其实是自己开启了 express 应用，<font color=FF0000>添加了对 webpack 编译的监听，添加了和浏览器的 **websocket 长连接**</font>，<font color=FF0000>当文件变化触发 webpack 进行编译并完成后，会通过 sokcet 消息告诉浏览器准备刷新</font>。而为了减少刷新的代价，就是不用刷新网页，而是刷新某个模块，webpack-dev-server 可以支持热更新，<font color=FF0000>通过生成 文件的 hash 值来比对需要更新的模块</font>，浏览器再进行热替换。

#### webpack 打包的 hash 码是如何生成的

- **webpack生态中存在多种计算hash的方式**

  - **hash：**<font color=FF0000>**每次 webpack 编译中生成的 hash值**</font>，所有使用这种方式的文件hash都相同。<font color=FF0000>每次构建都会使 webpack 计算新的 hash</font>

  - **chunkhash：**<font color=FF0000>**基于 入口文件 及其 关联的 chunk 形成**</font>，某个文件的改动只会影响与它有关联的 chunk 的 hash值，不会影响其他文件 contenthash 根据文件内容创建

  - **contenthash：**<font color=FF0000>**当文件内容发生变化时，contenthash发生变化**</font>

- **避免相同随机值：**webpack 在计算 hash 后分割 chunk。产生相同随机值可能是因为这些文件属于同一个 chunk，可以将某个文件提到独立的 chunk（如放入entry）

#### Webpack 如何用 localStorage 离线缓存静态资源

- 在配置 webpack 时，我们可使用 html-webpack-plugin 来注入一段脚本到 html， 来实现第三方或者公用资源的静态化存储。

  **做法：**在 html 中注入一段标识，例如 <% HtmlWebpackPlugin.options.loading.html %>，在 html-webpack-plugin 中即可通过配置 html 属性，将 script 注入进去。

- 通过配置 webpack-manifest-plugin，生成 manifest.json 文件用来对比 js 资源的差异，做到是否替换；当然，也要写缓存 script。

- 在我们做 Cl / CD的时候，也可以通过编辑文件流来实现静态化脚本的注入，来降低服务器的压力，提高性能

- 可以通过自定义 plugin 或者 html-webpack-plugin 等周期函数，动态注入前端静态化存储 script

#### webpack 常见的plugin有哪些？

- ProvidePlugin：自动加载模块（相当于定义一个全局变量），使用时不需要 require 和 import

- <font color=FF0000>**html-webpack-plugin**</font>：可以根据模板自动生成html代码，并自动引用 css 和 js 文件

- ~~extract-text-webpack-plugin~~ 将js文件中引用的样式单独抽离成css文件。

  **注：**<font color=FF0000>该 plugin 已经 deprecated，推荐使用 **mini-css-extract-plugin**</font>

- DefinePlugin 编译时配置全局变量，这对开发模式和发布模式的构建允许不同的行为非常有用。

- <font color=FF0000>**HotModuleReplacementPlugin**</font>：热更新必备

- <font color=FF0000>optimize-css-assets-webpack-plugin</font>：不同组件中重复的css可以快速去重。

  **注：**在 webpack@5 中，推荐使用 css-minimizer-webpack-plugin

- <font color=FF0000>webpack-bundle-analyzer</font>：一个webpack的bundle文件分析工具，将bundle文件以可交互缩放的treemap的形式展示。

- <font color=FF0000>**compression-webpack-plugin**</font>：生产环境可采用 gzip 压缩JS和CSS

- happypack：通过多进程模型，来加速代码构建

- <font color=FF0000>**clean-webpack-plugin**</font>：清理每次打包下没有使用的文件

- speed-measure-webpack-plugin：可以看到每个Loader和Plugin执行耗时（整个打包耗时、每个 Plugin和 Loader 耗时）

#### webpack 插件如何实现

- webpack 本质是一个事件流机制，核心模块：tapable(Sync + Async)Hooks 构造出 === Compiler(编译) + Compiletion(创建bundles)
- compiler对象代表了完整的webpack环境配置。这个对象在启动webpack时被一次性建立，并配置好所有可操作的设置，包括options、loader和plugin。当在webpack环境中应用一插件时，插件将收到此compiler对象的引用。可以使用它来访问webpack的主环境
- compilation对象代表了一次资源版本构建。当运行webpack开发环境中间件时，每当检测到一个文件变化，就会创建一个新的compilation,从而生成一个新的编译资源。一个compilation对象表现了当前的模块资源、编译生成资源、变化的文件、以及被跟踪依赖的状态的信息。compilation对象也提供了很多关键时机的回调，以供插件做自定义处理时选择使用
- 创建一个插件函数，在其prototype上定义apply方法，指定一个webpack自身的事件钩子
- 函数内部处理webpack内部实例的特定数据
- 处理完成后，调用webpack提供的回调函数

#### webpack有哪些常⻅的Loader

- file-loader：把⽂件输出到⼀个⽂件夹中，在代码中通过相对 URL 去引⽤输出的⽂件
- url-loader：和 file-loader 类似，但是能在⽂件很⼩的情况下以 base64 的⽅式把⽂件内容注⼊到代码中去
- source-map-loader：加载额外的 Source Map ⽂件，以⽅便断点调试
- image-loader：加载并且压缩图⽚⽂件
- babel-loader：把 ES6 转换成 ES5
- css-loader：加载 CSS，⽀持模块化、压缩、⽂件导⼊等特性
- style-loader：把 CSS 代码注⼊到 JavaScript 中，通过 DOM 操作去加载 CSS。
- eslint-loader：通过 ESLint 检查 JavaScript 代码

#### webpack如何实现持久化缓存

- 服务端设置http缓存头（cache-control）
- 打包依赖和运行时到不同的chunk，即作为splitChunk,因为他们几乎是不变的
- 延迟加载：使用 import() 方式，可以动态加载的文件分到独立的chunk,以得到自己的chunkhash
- 保持hash值的稳定：编译过程和文件内通的更改尽量不影响其他文件hash的计算，对于低版本webpack生成的增量数字id不稳定问题，可用hashedModuleIdsPlugin基于文件路径生成解决

#### 如何⽤webpack来优化前端性能？

⽤webpack优化前端性能是指优化webpack的输出结果，让打包的最终结果在浏览器运⾏快速⾼效。

- 压缩代码：删除多余的代码、注释、简化代码的写法等等⽅式。可以利⽤webpack的 UglifyJsPlugin 和 ParallelUglifyPlugin 来压缩JS⽂件， 利⽤ mini-css-extract-plugin 来压缩css
- 利⽤CDN加速: 在构建过程中，将引⽤的静态资源路径修改为CDN上对应的路径。可以利⽤webpack对于 output 参数和各loader的 publicPath 参数来修改资源路径
- Tree Shaking: 将代码中永远不会⾛到的⽚段删除掉。可以通过在启动webpack时追加参数 --optimize-minimize 来实现
- Code Splitting: 将代码按路由维度或者组件分块(chunk),这样做到按需加载,同时可以充分利⽤浏览器缓存
  提取公共第三⽅库: SplitChunksPlugin插件来进⾏公共模块抽取,利⽤浏览器缓存可以⻓期缓存这些⽆需频繁变动的公共代码

#### webpack treeShaking机制的原理

- treeShaking 也叫摇树优化，是一种通过移除多于代码，来优化打包体积的，生产环境默认开启。
- 可以在代码不运行的状态下，分析出不需要的代码；
- 利用es6模块的规范
  - ES6 Module引入进行静态分析，故而编译的时候正确判断到底加载了那些模块
  - 静态分析程序流，判断那些模块和变量未被使用或者引用，进而删除对应代码

摘自：[webpack 十连问你能接住几题(附详解)](https://mp.weixin.qq.com/s/_QzAVr92WFQmHWJnwxDymQ)
