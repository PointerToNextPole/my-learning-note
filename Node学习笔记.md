# Node学习笔记



## 《从零开发Web Server博客项目  前端晋升全栈工程师必备》笔记



##### nvm 使用命令

- **nvm list：**查看当前所有的node版本
- **nvm install v10.13.0：**安装指定的版本
- **nvm use-delete-prefix 10.13.0：**切换到指定的版本



##### common.js 中的导出和导入

```js
// 导出 foo.js
foo() { ... }
bar() { ... }
// 单个
module.exports = foo
// 多个的情况，要用对象导出
module.exports = {
  foo,
  bar
}
       
// 导入 foo.js
// 单个
const foo = require('foo.js')
// 多个导入
const {foo, bar} = require('foo.js')
```



##### Node 代码的调试

在 package.json 中，默认（使用 `npm init -y ` ）main属性对应的值为 "index.js"，表示：主文件是 "index.js"。如果要在 VSCode中调试 (debug) 项目时，必须要有 "index.js" 这个文件（当然，这个名字可以自定义，当然实际文件名也要跟着变化）

**补充：**

模块引入方法require()在引入包时，会优先检查这个字段，并将其作为包中其余模块的入口。如果不存在这个字段，require()方法会查找包目录下的index.js、index.node、index.json文件作为默认入口

摘自：《深入浅出Node.js》P35



#### Node 处理 http 请求

##### get 请求 和 querystring（所有的 url参数）

- get请求，即客户端要向server端获取数据，如查询博客列表 
- 通过querystring来传递数据，如a.html?a=100&b=200
- 浏览器直接访问，就发送get请求

- 代码示例：

  ```js
  const http = require('http')
  const querystring = require('querystring')
  
  const server = http.createServer((req, res) => {
    console.log(req.method) // GET
    const url = req.url // 获取请求的完整 url
    req.query = querystring.parse(url.split('?')[1]) // 解析querystring
    console.log('query: ', req.query) // 控制台打印 query
    res.end(JSON.stringify(req.query)) // 将querystring 返回
  })
  
  server.listen(8000)
  console.log("OK")
  ```

##### post 请求 和 post-data

- post请求，即客户端要向服务端传递数据，如新建博客

- 通过post data传递数据

- 浏览器无法直接模拟，需要手写js，或者使用postman

- 示例代码：

  另外，需要在postman中去模拟post请求

  ```js
  const http = require('http')
  
  const server = http.createServer((req, res) => {
    if(req.method === 'POST') {
      console.log('content-type: ', req.headers['content-type'])
      
      // 接收数据，采用数据流的方式接收
      let postData = ''
      req.on('data', chunk => {
        postData += chunk.toString()
      })
      req.on('end', () => {
        console.log('postData: ', postData)
        res.end('hello world!')
      })
    }
  })
  
  server.listen(8000)
  console.log('OK!')
  ```

##### 路由（接口地址 api/foo/bar ）



可以使用 nodemon 来监控你项目中文件的变动，并自动重启服务。即：热部署工具



## coderwhy Node.js 学习笔记

##### 浏览器引擎

- **Gecko：**早期被Netscape和Mozilla Firefox浏览器使用;

- **Trident：**微软开发，被IE4-IE11浏览器使用，但是Edge浏览器已经转向Blink
- **Webkit：**苹果基于KHTML开发、开源的，用于Safari，Google Chrome之前也在使用; 
- **Blink：**是Webkit的一个分支，Google开发，目前应用于Google Chrome、Edge、Opera等
- 等等…

事实上，我们经常说的浏览器内核指的是浏览器的排版引擎：排版引擎( layout engine )，也称为浏览器引擎( browser engine)、页面渲染引擎( rendering engine )或样版引擎。



##### 渲染引擎工作原理

关键渲染路径

<img src="https://i.loli.net/2021/10/27/GIKUCpTzsqESofe.png" alt="image-20211027232236491" style="zoom: 75%;" />

##### 常见的 JS 引擎

-  **SpiderMonkey：**第一款JavaScript引擎，由Brendan Eich开发（也就是JavaScript作者）
- **Chakra：**微软开发，用于IT浏览器
- **JavaScriptCore：**WebKit中的JavaScript引擎， Apple公司开发
- **V8：**Google开发的强大JavaScript引擎，也帮助Chrome从众多浏览器中脱颖而出



##### JS引擎和浏览器内核之间的关系

这里我们先以WebKit为例，WebKit事实上由两部分组成的：

- **WebCore：**负责HTML解析、布局、渲染等等相关的工作
- **JavaScriptCore（ JS 引擎）：**解析、执行JavaScript代码

所以JS引擎是浏览器内核的一部分。



##### V8 引擎处理过程

<img src="https://i.loli.net/2021/10/28/E4pUv58nlmfdK6V.png" alt="image-20211028000541086" style="zoom: 67%;" />

V8引擎本身的源码非常复杂，大概有超过100w行C++代码,但是我们可以简单了解一下它执行JavaScript代码的原理：

- Parse模块会将JavaScript代码转换成AST (抽象语法树)这是因为解释器并不直接认识JavaScript代码
  - 如果函数没有被调用，那么是不会被转换成AST的
  - Parse的V8官方文档：https://v8.dev/blog/scanner

- Ignition是一个解释器，会将AST转换成 ByteCode（字节码）
  - 同时会收集TurboFan优化所需要的信息（比如函数参数的类型信息,有了类型才能进行真实的运算）
  - 如果函数只调用一次，Ignition会执行解释执行ByteCode
  - Ignition的V8官方文档：https://v8.dev/blog/ignition-interpreter

- TurboFan是一个编译器，可以将字节码编译为CPU可以直接执行的机器码
  - 如果一个函数被多次调用，那么就会被标记为热点函数，那么就会经过TurboFan转换成优化的机器码,提高代码的执行性能
  - 但是，机器码实际上也会被还原为ByteCode，这是因为如果后续执行函数的过程中,类型发生了变化（比如sum函数原来执行的是number类型,后来执行变成了string类型），之前优化的机器码并不能正确的处理运算,就会逆向的转换成字节码
  - TurboFan的V8官方文档：https://v8.dev/blog/turbofan-jit
- 上面是JavaScript代码的执行过程，事实上V8的内存回收也是其强大的另外一个原因，不过这里暂时先不展开讨论：
  - Orinoco模块，负责垃圾回收，将程序中不需要的内存回收
  - Orinoco的V8官方文档：https://v8.dev/blog/trash-talk



##### 回顾：官方对Node.js的定义

Node.js 是一个基于 V8 JavaScript 引擎的 JavaScript 运行时环境。

> Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine.

也就是说 Node.js 基于 V8 引擎来执行 JavaScript 的代码，但是不仅仅只有 V8 引擎：

- 前面我们知道V8可以嵌入到任何C ++应用程序中，无论是Chrome还是Node.js，事实上都是嵌入了v8引擎来执行JavaScript代码
- 但是在Chrome浏览器中,还需要解析、渲染HTML，CSs等相关渲染引擎，另外还需要提供支持浏览器操作的API、浏览器自己的事件循环等
- 另外，在Node.js中我们也需要进行一些额外的操作,比如文件系统读/写、网络IO、加密、压缩解压文件等操作；



##### 浏览器和 Node 的区别

<img src="https://i.loli.net/2021/10/28/qFDoY35ETyCAOBw.png" alt="image-20211028004205435" style="zoom:40%;" />

##### Node 架构图

- 我们编写的JavaScript代码会经过V8引擎，再通过Nodejs的Bindings，将任务放到Libuv的事件循环中
- libuv ( Unicorn Velociraptor-独角伶盗龙)是使用C语言编写的库;
- libuv提供了事件循环、文件系统读写、网络IO、线程池等等内容;

<img src="https://i.loli.net/2021/10/28/VIel4XGCSb2qaR8.png" alt="image-20211028004526334" style="zoom:50%;" />



##### Node 管理工具

- NVM：https://github.com/nvm-sh/nvm
- N：https://github.com/tj/n
  - **n lts：**安装最新的lts版本
  - **n latest：**安装最新的current版本
  - **n：**查看所有版本，并且可以进行切换版本



##### REPL

- REPL是 Read-Eval-Print Loop的简称，翻译为"读取-求值-输出"循环
- REPL 一个简单的、交互的编程环境



##### 给Node传递参数

 在 Node 中的 全局对象 process 对象中，存在一个 argv 数组 ( argument vector )，其中存放的都是字符串。其中第一个元素是：node的安装路径，第二个元素是当前脚本的路径。第三个元素才是用户传递的参数。

即：使用 `process.argv[i]`

##### 为什么叫 argv ？

在C/C++程序中的main函数中,实际上可以获取到两个参数：

- argc : argument counter的缩写,传递参数的个数
- argv : argument vector的缩写,传入的具体参数。
  - vector 翻译过来是矢量的意思，在程序中表示的是一种数据结构。
  - 在C++、Java 中都有这种数据结构,是一种数组结构;
  - 在JavaScript 中也是一个数组,里面存储一些参数信息;

**补充：**process对象中的内容：

显示方式：在Node的REPL 中，输入 `process.` 再按下两次 Tab 键

![image-20211028224427394](https://i.loli.net/2021/10/28/5sLNoKgI6WnXqtw.png)



##### Node 程序的输出

- **console.log()**
- **console.clear()：**将控制台中的内容清空
- **console.trace()：**打印函数调用栈



##### Node 中常见的全局对象

所有的全局对象可见：[官方文档 - Globals](https://nodejs.org/dist/latest-v17.x/docs/api/globals.html)

但是 有些对象只能被称为 “特殊的全局变量”，为什么称之为特殊的全局对象？

- 这些全局对象实际上是模块中的变量,只是每个模块都有,看来像是全局变量
- 在命令行交互中是不可以使用的

- 包括:\_\_dirname、\_\_filename、exports、module、require()

  其中 \_\_dirname 表示当前文件 所在目录的名称（绝对路径）；\_\_filename 表示当前文件的名称（包括路径）

还是以  \_\_dirname 为例，他只能在模块中使用，且每个模块都会使用这个；但是，如果在 REPL 中使用 \_\_dirname 将会报错，因为没有定义。所以，他不能被称为 “真正的全局对象”

**常见的全局对象**

- **process 对象：**process <font color=FF0000>提供了Node进程中相关的信息</font>：
  - 比如Node的运行环境、参数信息、当前使用的Node版本信息 等
  - 后面在项目中，我也会讲解，如何将一些环境变量读取到 process 的 env 中
  
- **console 对象**

- **定时器对象：**setTimeout、setInterval、setImmediate。对应的有：clearTimeout、clearInterval、clearImmediate 
  - 对于定时器的补充：process.nextTick
  
- **global**

  global 中的内容（显示方式：在Node的REPL 中，输入 `global.` 再按下两次 Tab 键）：

  ![image-20211028224901221](https://i.loli.net/2021/10/28/EdA9cZi6IS8fP3h.png)

  global 中放了很多东西，可以让用户很方便的获取到这些东西（<font color=FF0000>类似于浏览器中的 window</font>）



在CommonJS出现之前，是<font color=FF0000>可以通过 IIFE 解决</font> 没有模块化带来的问题的。

```js
var module = ;(function (){
  var foo = 1
  var bar = 2
  
  return {
  	foo,
  	bar
	}
})()
```

**但是，带来的新的问题是**：

- 第一,我必须记得每一个模块中返回对象的命名,才能在其他模块使用过程中正确的使用;
- 第二,代码写起来混乱不堪,每个文件中的代码都需要包裹在一个匿名函数中来编写;
- 第三,在没有合适的规范情况下,每个人、每个公司都可能会任意命名、甚至出现模块名称相同的情况

**所以，我们会发现,虽然实现了模块化,但是我们的实现过于简单,并且是没有规范的。**

- 我们需要制定一定的规范来约束每个人都按照这个规范去编写模块化的代码;
- 这个规范中应该包括核心功能:模块本身可以导出暴露的属性,模块又可以导入自己需要的属性;
- JavaScript社区为了解决上面的问题,涌现出一系列好用的规范,接下来我们就学习具有代表性的一些规范。



##### CJS 导入导出

Node中实现CJS的本质是 <font color=FF0000>引用赋值</font>（详见下面的 exports.foo 中的内容），可以通过分别在 prod.js 和 user.js 中使用 setTimeout 以验证（比如，先在 user.js 中打印；而后一秒钟之后 prod.js 中内容改变；再在user.js 中打印。甚至可以反过来，user.js 中改变引入的值，并让 prod.js 打印，查看是否改变（结果是 “改变了”））

- **exports.foo**

  ```js
  // prod.js
  
  let foo = 'foo'
  let bar = 'bar'
  exports.foo = foo
  exports.bar = bar
  ```

  ```js
  // user.js
  
  // 注意：这个语句等价于 prod = exports
  const prod = require('./prod.js')
  console.log(prod.foo, prod.bar)
  
  // 由于 prod 就是 exports，是一个对象，所以可以使用结构赋值
  const {foo,  bar} = require('./prod.js')
  console.log(foo, bar)

- **module.exports = foo**

  <font color=FF0000>**module.exports和exports有什么关系或者区别呢？**</font>

  追根溯源，通过维基百科中对CommonJS规范的解析：CommonJS中是没有module.exports的概念的。但是<font color=FF0000>为了实现模块的导出, Node中使用的是 **Module的类**，**每一个模块都是Module的一个实例**，也就是 module</font>（即：每一个模块都有这个 module 属性）。（module 是一个“特殊的”全局对象）

  - 所以在Node中真正用于导出的其实根本不是exports ,而是module.exports ;

  - 因为module才是导出的真正实现者;

  如果 <font color=FF0000 size=4>**不手动**</font>给module.exports 赋值，则node帮用户做了 module.exports = exports，也就是说：被导出的对象、exports、module.exports 这三个对象是一样的（引用赋值）

  <font color=FF0000 size=4>**如果手动给 module.exports 赋值，module.exports 将重新开辟一个空间，将赋的值放入（深拷贝），调用者使用新开辟的空间中的数据，将不会影响被调用者的数据**</font>



#### require 的细节

```js
require(X)
```

- **如果，X是一个核心模块：**比如path、http，则，直接返回核心模块，并且停止查找

- **如果：X是以 ./ 或 ../ 或 / (根目录) 开头的**

  - 第一步：将当做一个文件在对应的目录下查找;
    - 如果有后缀名，按照后缀名的格式查找对应的文件
    - 如果没有后缀名，会按照如下顺序：
      1. 直接查找文件X
      2. 查找Xjs文件
      3. 查找X.json文件
      4. 查找X.node文件
  
- 第二步：没有找到对应的文件，将X作为一个目录
    - 查找目录下面的index文件
      1. 查找X/index.js文件
      2. 查找X/index.json文件
      3. 查找X/index.node文件
  - 如果没有找到，那么报错: not found

- **直接是一个x (没有路径)，并且x不是一个核心模块**

  则会从当前目录中的node_modules 文件夹中寻找，如果找不到，则前往上一层级的node_modules 文件夹寻找；知道在根目录中的node_modules 文件夹中也没有找到，则会报错：not found。

  查找文件路径顺序如下：

  ![image-20211029100330552](https://i.loli.net/2021/10/29/7fRgNsjvSGEcrz5.png)
  
  也就是 module.path （数组）中的路径，按先后顺序一一查找，如果都找不到，则报错。



#### Module 中的内容

![image-20211031163719703](https://i.loli.net/2021/10/31/xhSQNr7dWs6ty9Z.png)

- id：文件的id，如果是当前的文件，则为 `.`
- path：当前文件所在地路径
- exports：导出的属性
- parent：父模块
- filename：文件名
- loaded：记录当前模块是否被加载过
- children：子模块
- paths：导入模块查找路径



<font color=FF0000>**CJS加载过程是同步的**</font>。也就是说：相比 usr.js 中的代码，prod.js 中的代码会优先执行。

另外，CJS中引入的文件只会加载一次，因为是有缓存的；在同一个项目中，如果第二次被加载，则直接从缓存中取得。这也是Module中loaded属性的作用。



**如果有循环引入,那么加载顺序是什么?**

如果出现下图模块的引用关系，那么加载顺序是什么呢？

- 这个其实是一种数据结构：图结构
- 图结构在遍历的过程中,有深度优先搜索( DFS, depth first search )和广度优先搜索( BFS, breadth first search);
- Node采用的是深度优先算法：main-> aaa -> ccc-> ddd-> eee ->bb

<img src="https://i.loli.net/2021/10/31/a1WkKDRzw847GAb.png" alt="image-20211031165246012" style="zoom:50%;" />



**AMD**

AMD主要是应用于浏览器的一种模块化规范：

- AMD是Asynchronous Module Definition (异步模块定义）的缩写;
- 它采用的是异步加载模块;
- 事实上AMD的规范还要早于CommonJs ,但是CommonJS目前依然在被使用,而AMD使用的较少了;

我们提到过,规范只是定义代码的应该如何去编写,只有有了具体的实现才能被应用：

- AMD实现的比较常用的库是 require.js 和 curl.js;

**CMD** 略，直接看视频。

**ES Module**

ES Module和CommonJS的模块化有一些不同之处：

- 一方面它<mark>使用了import和export关键字</mark>

- 另一方面它<font color=FF0000>采用编译期的静态分析，并且也加入了动态引用的方式</font>

ES Module模块采用export和import关键字来实现模块化：

- export负责将模块内的内容导出;

- import负责从其他模块导入内容

另外，<font color=FF0000>ESM 将自动采用严格模式 （ 自动加上 use strict ） </font>

ESM exports 的三种用法

- **export + 定义**

  ```js
  export const foo = 'bar'
  ```

- **export + {}**

  ```js
  export {foo, bar, baz}
  ```

  注意：在 {} 中统一导出，但是 <font color=FF0000>{} 不是一个对象</font>，也就是说：<mark>export {foo: 'bar'} 是会报错的</mark>。<font color=FF0000>{} 中放置到处的变量的引用用列表</font>。另外，export 对应的 {} 也不是对象。

- **在 export + {} 中 可以给变量起别名 ( as )**

  ```js
  const foo = 'bar'
  export {
  	foo as bar
  }
  ```

  在import 时，必须用别名；而不是本来的名字。



##### ESM export 的三种用法

- `import {} from file_path`

  ```js
  import {foo, bar, baz} from './prod.js'
  ```

  这里必须要加扩展名

- 导出变量可以起别名

  ```js
  import {foo as bar} from './prod.js'
  ```

- 使用 * 并起别名

  ```js
  import * as foobar from './prod.js'
  console.log(foobar.foo)
  ```



**可以将 import 和 export 一起使用：**

```js
// middleware.js
export {foo, bar} from './prod.js'
```

这样写等价于：

```js
// middleware.js
import {foo, bar} from './prod.js'
export {foo, bar}
```

实际使用时：

```js
export {foo as fooAlias} from './prod.js'
```

**这样做的用途：**

- 在开发和封装一个功能库时,通常我们希望将暴露的所有接口放到一个文件中;
- 这样方便指定统一的接口规范,也方便阅读;
- 这个时候,我们就可以使用export和import结合使用;
- 一般用于第三方库的导出，便于用户使用。



##### export default的使用

在export 中使用 default，在导出时，是可以不命名的：

```js
// prod.js
export default function () {
  ...
}
```

在导入时，用户可以自定义导入模块的名字：

```js
// usr.js
import myFn from './prod.js'
```

另外，export default 在一个模块中只能使用一次。



`import foo from 'foo.js'` 这种模式的引入，不是一个函数（真正是函数的是 import() ）。同时，他也在parsing 阶段就已经确定了依赖关系（parse -> AST -> bytecode ->  执行）；这是静态导入。

而在 ES2020 时提供了 import() 支持动态导入（解析时加载），import() 是一个函数。

```js
if( bool ) {
  import('foo')
}
```

<font color=FF0000 size=4>**import() 是异步执行的，本质上返回的是一个 promise**</font>

另外，CJS中的require() 是一个函数，同样支持 动态导入



##### ES Module 加载过程

ES Module加载 js文件的过程是编译（解析）时加载的,并且是异步的：

- 编译时（解析）时加载，意味着 import不能和运行时相关的内容放在一起使用（不能动态导入）
- 比如from后面的路径需要动态获取

- 比如不能将import放到if等语句的代码块中

- 所以我们有时候也称ES Module是静态解析的，而不是动态或者运行时解析的;

异步的意味着：JS 引擎在遇到import时会去获取这个js文件，但是这个获取的过程是异步的,并不会阻塞主线程继续执行

- 也就是说设置了type=module的代码，相当于在script标签上也加上了async属性（ \<script src="..." async>\</scirpt>）

- 如果我们后面有普通的script标签以及对应的代码,那么ES Module对应的js文件和代码不会阻塞它们的执行

 **ES Module通过export导出的是变量本身的引用：**

- export在导出一个变量时，js引擎会解析这个语法，<font color=FF0000>并且**创建模块环境记录( module environment record )**</font>

  <img src="https://i.loli.net/2021/10/31/vfpWgbUmhcz4PB7.png" alt="image-20211031204115126" style="zoom: 50%;" />

- <font color=FF0000>**模块环境记录会和变量进行绑定( binding)，并且这个绑定是实时的**</font>

- 而在导入的地方,我们是可以实时的获取到绑定的最新值的

  <img src="https://i.loli.net/2021/10/31/zfVsIYrblWCOgnP.png" alt="image-20211031204356938" style="zoom:50%;" />

**所以，如果在导出的模块中修改了变化，那么导入的地方可以实时获取最新的变量**

注意：在导入的地方不可以修改变量，因为它只是被绑定到了这个变量上（其实是一个常量），即：在 模块环境记录 中的定义就是一个 const 类型。但是，被引用处的 变量被修改（比如 name被修改），模块环境记录中对应的记录（const name = 'why'）会被删除，并且将修改后的记录添加上去( const name = 'aaaa')；所以不会报错。

<img src="https://i.loli.net/2021/10/31/zX1Qy8PZHurJkVC.png" alt="image-20211031203455534" style="zoom:50%;" />

思考:如果bar.js 中导出的是一个对象，那么main.js中是否可以修改对象中的属性呢?

答案是可以的：因为他们指向同一块内存空间（即，绑定的是引用 / 地址，只要绑定的地址不改变，内容可以随便改变）



##### CJS 和 ESM的交互

**通常情况下，CommonJS不能加载ES Module**（模块用 ESM 导出，CJS 导入）

- 因为CommonJS 是同步加载的，但是ES Module必须经过静态分析等（而且ESM是异步的，可能ESM的内容还没有变成ByteCode，这是CJS无法执行），(ESM) 无法在这个时候执行JavaScript代码。见下图：ESM加载在第一二阶段之间，CJS执行在三四阶段之间，所以无法使用ESM。

  ```mermaid
  flowchart LR
      parse -. ESM加载 .-> AST
      AST --> ByteCode
      ByteCode -. CJS加载 .-> 代码运行
  ```

- 但是这个并非绝对的，某些平台在实现的时候可以对代码进行针对性的解析，也可能会支持

- Node当中是不支持的

**多数情况下, ES Module可以加载CommonJs**（模块用CJS导出，ESM导入）

- ES Module在加载CommonJS时，会将其 module.exports 导出的内容作为default导出方式来使用
- 这个依然需要看具体的实现,比如webpack中是支持的、Node最新的Current版本也是支持的



**Node中常见内置模块**（**除了可以使用 CJS导入内置模块外，还可以使用 ESM导入内置模块**）

- **path：**用于对路径和文件进行处理，提供了很多好用的方法。

  **用法举例：**linux / macOS 中 路径中分隔符使用的是 /，而win中有一些系统，路径分隔符使用的是 \\，所以路径最好不要简单的由字符串写死 (只用 / 或 \\ ) 表示，这样会造成不兼容；所以需要使用path模块（使用 path.resolve() ）

  **path 的其他方法：**

  - **path.dirname(filePath)：**获取文件路径

  - **path.basename(filePath)：**获取文件名 + 扩展名

  - **path.extname(filePath)：**获取文件扩展名

    ```js
    const path = require('path')
    
    const filePath = '/User/foo/bar.txt'
    
    console.log(path.dirname(filePath)) // /User/foo
    console.log(path.basename(filePath)) // bar.txt
    console.log(path.extname(filePath)) // .txt
    ```

  - **path.join()：**拼接路径。除了会做不同系统之间路径分隔符 的处理外，就是单纯的拼接。

    ```js
    const path = require('path')
    
    const basePath = 'User/foo'
    const fileName = 'bar.txt'
    const filePath = path.join(basePath, fileName) // /User/foo/bar.txt
    ```

  - **path.resolve()：**路径拼接。<font color=FF0000>**和path.join()的区别：path.resolve() 会判断拼接路径字符中，是否有以：/ 或 ./ 或 ../ 开头的 路径**</font>，如果有：则会分析/ 或 ./ 或 ../ 的路径，并在前面加上 当前系统的完整路径（绝对路径）。这个用的最多

  **补充：** 

  POSIX 可移植操作系统接口 (Portable Operating System Interface) 

  - Linux 和 macOS 都实现了 POSIX 接口
  - Windows 部分系统 实现了 POSIX接口

- **fs ( file system )：**文件系统。

  借助于Node帮我们封装的文件系统，我们可以在任何的操作系统 ( window、 macOS、Linux ) 上面直接去操作文件。这也是Node可以开发服务器的一大原因，也是它可以成为前端自动化脚本等热门工具的原因。

  **fs API很多，这些API大多数都提供三种操作方式:**

  - **同步操作文件：**代码会被阻塞，不会继续执行;
  - **异步回调函数操作文件：**代码不会被阻塞，需要传入回调函数，当获取到结果时,回调函数被执行
  - **异步Promise操作文件：**代码不会被阻塞，通过fs.promises调用方法操作，会返回一个Promise，可以通过then、catch进行处理

  **以读取 文件信息为例：**

  - **fs.statSync()**：同步，会阻塞后面的代码 

    ```js
    const fs = require('fs')
    
    const filePath = '...'
    const info = fs.stat(filePath)
    ```

    结果示例：

    ```
    Stats {
      dev: 16777220,
      mode: 33188,
      nlink: 1,
      uid: 501,
      gid: 20,
      rdev: 0,
      blksize: 4096,
      ino: 95266175,
      size: 105,
      blocks: 8,
      atimeMs: 1635671491044.592,
      mtimeMs: 1635671466458.197,
      ctimeMs: 1635671466458.197,
      birthtimeMs: 1635671374416.326,
      atime: 2021-10-31T09:11:31.045Z,
      mtime: 2021-10-31T09:11:06.458Z,
      ctime: 2021-10-31T09:11:06.458Z,
      birthtime: 2021-10-31T09:09:34.416Z
    }
    ```

    **另外，这里的info除了上面这些属性，还有如下方法：**

    - **info.isFile()：**当前读取的是 是否是文件
    - **info.isDirectory()：**当前读取的是 是否是文件夹

  - **fs.stat：**异步，采用Node底层的异步IO；不会阻塞后面的代码。

    ```js
    const fs = require('fs')
    
    const filePath = '...'
    fs.stat(filePath, (err, info) => {
      if(err) {
        console.log(err)
        return
      }
      console.log(info)
    })
    ```

  - **fs.promises.stat()**：使用promise

    ```js
    const fs = require('fs')
    
    const filePath = '...'
    fs.promises,stat(filePath).then(info => {
      console.log(info)
    }).catch(err => {
      console.log(err)
    })
    ```

  其他类似的方法，也是和这类似，有三种方法：同步、异步、promise的异步

  **文件描述符：**

  **文件描述符( File descriptors ) 是什么？**

  - 在POSIX系统上，对于每个进程，内核都维护着一张当前打开着的文件和资源的表格。
  - <font color=FF0000>每个打开的文件都分配了一个称为文件描述符的简单的数字标识符</font>。
  - 在系统层，所有文件系统操作都使用这些文件描述符来标识和跟踪每个特定的文件。
  - Windows 系统使用了一个虽然不同但概念上类似的机制来跟踪资源。

  **为了简化用户的工作，<font color=FF0000>Node.js 抽象出操作系统之间的特定差异</font>，并为所有打开的文件分配一个数字型的文件描述符。**

  通过文件描述符去读取文件信息：

  ```js
  const fs = require('fs')
  
  fs.open('./foo.txt', (err, fd) => {
    if(err) {
      console.log(err)
      return
    }
    
    // 通过文件描述符去获取文件信息
    // 这时候就不需要使用文件名称了，因为文件名称以及映射到文件描述符了
    fs.fstat(fd, (err, info) => {
      console.log(info)
    })
  })
  ```

  **文件的读写**

  ```js
  const fs = require('fs')
  
  const content = 'foobar'
  // 覆盖写入（先删除，再写入）
  fs.writeFile('./foo.txt', content, err => {
    console.log(err)
  })
  ```

  上面的示例是“覆盖性写入”。

  语法：

  ```js
  fs.writeFile(file, data[, options], callback)
  ```

  **其中options 是可选变量，包含encoding、flag、mode、signal**

  - **flag：**\<string> See support of file system flags. Default: 'w'.

    - **w：**打开文件<font color=FF0000>写入</font>，是 <font color=FF0000>**写入的默认值**</font>
    - **w+：**打开文件进行读写，如果<font color=FF0000>不存在则创建文件</font>
    - **r+：**打开文件进行读写，如果<font color=FF0000>不存在那么抛出异常</font>
    - **r：**打开文件读取，是<font color=FF0000>**读取时的默认值**</font>
    - **a：**打开要写入的文件，<font color=FF0000>将 ( IO ) 流放在文件末尾</font>。如果不存在则创建文件。a 表示 appending
    - **a+：**打开文件以进行读写，将 ( IO ) 流放在文件末尾。如果不存在则创建文件

    示例：

    ```js
    fs.writeFile('./foo.txt', {flag: "a"}, content, err => {
      console.log(err)
    })
    ```

  - **encoding：**\<string> | \<null> Default: 'utf8'

  - **mode：**\<integer> Default: 0o666

  - **signal：**\<AbortSignal> allows aborting an in-progress writeFile

  **文件读取：**

  ```js
  // 这里的data 是一个 buffer 对象。
  // 如果不指定 encoding的结果。
  fs.readFile('./foo.txt', (err, data) => {
    console.log(data) // 打印内容：<Buffer e4 bd a0 e5 a5 bd ef bc 8c e4 b8 96 e7 95 8c 0a>
  })
  
  // 指定 encoding
  fs.readFile('./foo.txt', { encoding: "utf-8" },(err, data) => {
    console.log(data) // 打印内容：你好，世界
  })
  ```

  **文件夹的操作：**

  - **创建文件夹**

    ```js
    const dirname = './foo'
    // 判断文件夹是否存在，如果不存在，则：
    if( !fs.existsSync(dirname) ) {
      // mkdir 创建文件夹
      fs.mkdir(dirname, err => {
        console.log(err)
      })
    }
    ```

  - **读取文件夹中的所有文件**

    ```js
    fs.readdir('./foo', (err, files) => {
      console.log(files) // 打印出 文件数组 ['foo', 'bar']
    })
    ```

    但是，会有一个问题：上面的代码只会读取一层，也就是说：即使读取的文件夹中还有文件夹，子文件夹中的文件不会被读取。

    这里需要判断是否是文件夹，除了可以用 上面说的 info.isDirectory() 外，还可以使用 fs.readdir() 中的options：withFileType。设置 withFileType 为 true 后，files中的元素 file 会多一个 方法：file.isDirectory()，以判断是否为文件夹。

    **代码示例如下：**

    ```js
    function getFiles(dirname) {
      fs.readdir(dir, {withFileType: true}, (err, files) => {
        for(let file of files) {
          if(file.isDirectory()) {
            // 递归，同时，用path.resolve() 拼接新的 文件夹路径
            const filePath = path.resolve(dirname, file.name)
            getFiles(filePath)
          } else {
            console.log(file.name)
          }
        }
      })
    }
    ```

    **补充：**options 中共有两个参数：

    1. **encoding：**\<string> Default: 'utf8' 

    2. **withFileTypes：** \<boolean> Default: false。如果为 true，files数组中的内容会变成对象（如下），而不是一个字符串

       ```json
       Dirent { name: '.DS_Store', [Symbol(type)]: 1 }, // 为1，则为文件
       Dirent { name: 'C++', [Symbol(type)]: 2 },       // 为2，则为文件夹
       ```

  - **文件夹的重命名**

    ```js
    fs.rename("./foo", "./bar", err => {
      console.log(err)
    })
    ```

- **events 模块**

  **Node中的核心AP都是基于异步事件驱动的：**

  - 在这个体系中，某些对象（ 发射器 Emitters ）发出某一个事件
  - 我们可以监听这个事件（ 监听器 Listeners )，并且传入的回调函数，这个回调函数会在监听到事件时调用

  <font color=FF0000>**发出事件和监听事件都是通过 EventEmitter 类来完成的,它们都属于 events对象。**</font>

  - **emitter.on( eventName, listener )：**监听事件，也可以使用 addListener;
  - **emitter.off( eventName, listener )：**移除事件监听，也可以使用removeListener;
  - **emitter.emit( eventName [, ...args] )：**发出事件，可以携带一些参数

  示例：

  ```js
  // EventEmitter 是一个类，类似于 Vue React 中事件总线( EventBus )，这些都是借鉴 Node的
  const EventEmitter = require("events")
  
  // 创建 发射器
  const emitter = new EventEmitter();
  
  // 监听一个事件（注册监听事件）。
  // 可以用 on() 或 addListener()，addListener 更早定义，addListener 是 on 的别名( alias )
  emitter.on('click', (args) => {
    console.log('监听click事件', args)
  })
  // 另外，可以在回调函数中加入多个参数。
  emitter.on('click', (arg1, arg2, arg3) => {...} )
  // 也可以将 (args) => { console.log('监听click事件', args) } 写成变量，这样便于 取消监听
  const listener = (args) => { console.log('监听click事件', args) }
  // 取消监听
  emitter.off('click', listener)
  
  // 如果只想让事件被监听一次，可以使用 emitter.once()
  emitter.once('click', (args) => {...} )
  
  // 提高优先级，即使放在后面的，也会第一个执行。同时，也有 prependOnceListener
  emitter.prependListener('click', (args) => {...})
  
  // 移除所有监听器 或者 加上可选的 eventName，移除特定的所有监听器
  emitter.removeAllListeners( [eventName] )
  
  // 发出（触发）一个事件
  emitter.emit('click')
  ```

  **其他相关信息的获取**

  ```js
  // 获取注册的事件（on 定义的事件）
  // 返回一个数组，数组内容是上面的注册的所有事件
  const eventList = emitter.eventNames()
  console.log(eventList) // ['click', 'tap']
  
  // 获取注册的事件对应的 回调函数
  const eventCbList = emitter.listener('click') 
  // [ [Function], [Function: fnName] ]，其中第一个表示：回调函数是不是一个变量（在emitter.on 中直接定义）；第二个表示：回调函数是一个定义的“变量？”（虽然使用const 定义的）
  
  // 获取事件对应的回调函数个数
  const eventCbCount = emitter.listenerCount('click') // 2
  ```



#### Node 包管理工具 npm

##### 配置文件

每一个项目都会有一个对应的配置文件，无论是前端项目还是后端项目：

- 这个配置文件会<font color=FF0000>记录着你项目的名称、版本号、项目描述等</font>
- 也会<font color=FF0000>记录着你项目所依赖的其他库的信息和依赖库的版本号</font>

package.json 中 main属性是 程序的入口，

在 npm scripts 中，npm run start 和 npm start 是等价的，同样的还有：test、stop、restart 命令。

##### dependencies 属性

- dependencies属性是指定<font color=FF0000>无论开发环境还是生成环境都需要依赖的包</font>
- 通常是我们项目实际开发用到的一些库模块;

##### devDependencies 属性

<font color=FF0000>一些包在生成环境是不需要的，比如webpack、babel 等</font>。这个时候我们会通过 npm install webpack --save-dev，将它安装到 devDependencies 属性中。

即使有的包只在devDependencies定义，但是在生产环境打包时，如果要用到，还是会被引入、打包。devDependencies 属性的意义是：开发共享出去的工具时，对方需不需要安装 该依赖，取决于 devDependencies。

另外，<mark>在开发服务器时，打包生产环境，可以通过 npm install --production 来安装项目依赖；这时，devDependencies 中的依赖将不会安装</mark>。而<font color=FF0000> npm install 是会安装 package.json 中**所有的依赖**</font>。

**package.json 安装依赖版本中出现类似于：^2.0.3 或者 ~2.0.3，这是什么意思？**

npm包 通常要遵从 semver 的版本规范（[官方文档](https://semver.org/lang/zh-CN/)）

- **版本格式：主版本号.次版本号.修订号，版本号递增规则如下：**
  - **主版本号( major )：**当你<font color=FF0000>做了不兼容的 API 修改</font>
  - **次版本号( minor )：**当你<font color=FF0000>做了向下兼容的**功能性新增**</font>
  - **修订号( patch )：**当你<font color=FF0000>做了向下兼容的问题修正</font>

- **^ 和 ~ 的含义：**
  - **^major.minor.patch：**表示 major 是保持不变的，minor 和 patch 永远安装最新的版本
  - **~major.minor.patch：**表示major 和 minor 保持不变，patch 永远安装最新的版本

**package.json 中其他常见的属性：**

- **engines属性**
  - engines 属性用于<font color=FF0000>指定Node和NPM的版本号</font>
  - 在安装的过程中，会先检查对应的引擎版本，如果不符合就会报错
  - 事实上也可以指定所在的操作系统 "os" : ["darwin", "linux"] ,只是很少用到
- **browserslist属性**
  - 用于<font color=FF0000>配置打包后的 JavaScript 浏览器的兼容情况</font>
  - 否则我们需要手动的添加 polyfills 来让支持某些语法
  - 也就是说它是为 webpack 等打包工具服务的一个属性（这里不是详细讲解webpack等工具的工作原理，所以不再给出详情）



**npm install 分为两种：**全局安装( global ) 和 局部安装( local )

npm全局安装的包都是一些工具包，比如：yarn、webpack等。<font color=FF0000>全局安装 类似 axios 的包 是没有意义的；如果在项目中没有本店安装 axios，而想 `const axios = require('axios')` 是拿不到 axios 的，因为 全局安装的路径 不在 module.path 的数组中</font>。



#### npm install 的原理

![image-20211101175159270](https://i.loli.net/2021/11/01/LhPGwR6UC1poWXZ.png)

由上图可知：npm install 从仓库中取得的包是压缩包，需要解压到 node_modules文件夹中。

在流程图的最上面一行（没有 lock文件的情况），在包文件被解压到 node_modules 文件夹之后，会生成 package-lock.json 文件。package-lock.json 是检查依赖一致性的，比如：axios依赖了一个东西（package-lock.json中有记录），如果和package-lock.json 中该依赖的版本不一致，则表示不一致，这时会重新构建依赖关系（走第一行的流程）

package-lock.json 会优先从 缓存文件中获取 依赖包；如果找不到，则先下载，在把下载好的依赖包放入缓存文件

##### 更多详解

npm install会检测是有package-lock.json文件:

- **没有lock文件**
  - 分析依赖关系，这是因为我们可能包会依赖其他的包，并且多个包之间会产生相同依赖的情况
  - 从registry仓库中下载压缩包（如果我们设置了镜像，那么会从镜像服务器下载压缩包）
  - 获取到压缩包后会对压缩包进行缓存（从npm5开始有的）
  - 将压缩包解压到项目的 node-modules 文件夹中（前面我们讲过，require的查找顺序会在该包下面查找）
- **有lock文件**
  - 检测lock中包的版本是否和 package.json 中一致（会按照semver版本规范检测）
  - 不一致，那么会重新构建依赖关系,直接会走顶层的流程
  - 一致的情况下，会去优先查找缓存
  - 没有找到，会从registry仓库下载，直接走顶层流程;
  - 查找到,会获取缓存中的压缩文件，并且将压缩文件解压到node_modules文件夹中

**package-lock.json 文件中的内容介绍：**

```json
{
  "name": "tmp",
  "version": "1.0.0",
  "lockfileVersion": 2,
  "requires": true, // 后面的dependencies 中，使用requires来管理子依赖（详见下面的 dependencies -> axios -> requires）
  "packages": {
    "": {
      "name": "tmp",
      "version": "1.0.0",
      "license": "ISC",
      "dependencies": {
        "axios": "^0.24.0"
      }
    },
    "node_modules/axios": {
      "version": "0.24.0",
      "resolved": "https://registry.npmmirror.com/axios/download/axios-0.24.0.tgz?cache=0&sync_timestamp=1635213986012&other_urls=https%3A%2F%2Fregistry.npmmirror.com%2Faxios%2Fdownload%2Faxios-0.24.0.tgz",
      "integrity": "sha1-gE5voeS5xSiFAd2d/1anoJQNINY=",
      "license": "MIT",
      "dependencies": {
        "follow-redirects": "^1.14.4"
      }
    },
    "node_modules/follow-redirects": {
      "version": "1.14.4",
      "resolved": "https://registry.nlark.com/follow-redirects/download/follow-redirects-1.14.4.tgz?cache=0&sync_timestamp=1631622129411&other_urls=https%3A%2F%2Fregistry.nlark.com%2Ffollow-redirects%2Fdownload%2Ffollow-redirects-1.14.4.tgz",
      "integrity": "sha1-g4/fSKi73XnlLuUfsclOPtmLk3k=",
      "engines": {
        "node": ">=4.0"
      }
    }
  },
  "dependencies": {
    "axios": {
      "version": "0.24.0", // 真实安装的版本
      "resolved": "https://registry.npmmirror.com/axios/download/axios-0.24.0.tgz?cache=0&sync_timestamp=1635213986012&other_urls=https%3A%2F%2Fregistry.npmmirror.com%2Faxios%2Fdownload%2Faxios-0.24.0.tgz", // 安装的版本，在服务器上所在的位置。
      "integrity": "sha1-gE5voeS5xSiFAd2d/1anoJQNINY=",
      "requires": {
        "follow-redirects": "^1.14.4"
      }
    },
    "follow-redirects": {
      "version": "1.14.4",
      "resolved": "https://registry.nlark.com/follow-redirects/download/follow-redirects-1.14.4.tgz?cache=0&sync_timestamp=1631622129411&other_urls=https%3A%2F%2Fregistry.nlark.com%2Ffollow-redirects%2Fdownload%2Ffollow-redirects-1.14.4.tgz",
      "integrity": "sha1-g4/fSKi73XnlLuUfsclOPtmLk3k="
    }
  }
}
```

键入命令 `npm config get cache` 可查看 缓存所在位置（一般在 /Users/userName/.npm ），进入 .npm 文件夹，会发现 有一个 _cacache 文件夹，其中有 content-v2、index-v5、tmp 三个文件夹，index-v5 是索引，content-v2 是内容；根据索引，去取内容中的东西。

另外，无论是团队中协作，还是将代码上传到 GitHub中，最后将 package-lock.json 和 package.json 一起上传。



##### 安装工具包附带的依赖

在安装 axios 时，axios 也会将自己的依赖 follow-redirectives 在项目中下载下来，打开node_modules中axios 文件夹中的 package.json，可以看到 devDependencies 中有大量的依赖，这是它开发时所使用的依赖，而这不是我们需要关心的。dependencies 中只有一个 follow-redirectives，需要被下载。所以，在安装包时，如果它的 package.json 中 dependencies 中有依赖，npm 会将其下载下来。



##### npm 的其他命令

- **npm rebuild：**强制重新 build，比如将项目从 windows 拷贝到macOS 上，依赖需要重新编译，这时就要使用 npm rebuild。另外，切换 node 版本时，也最好执行一下。
- **npm cache clean：**清除缓存



##### yarn的使用

| npm                                     | Yarn                          |
| --------------------------------------- | ----------------------------- |
| npm install                             | yarn install                  |
| npm install [package]                   | yarn add [package]            |
| npm install --save [package]            | yarn add [package]            |
| npm install --save-dev [package]        | yarn add [package] [--dev/-D] |
| npm rebuild                             | yarn install --force          |
| npm uninstall [package]                 | yarn remove [package]         |
| npm uninstall --save [package]          | yarn remove [package]         |
| npm uninstall --save-dev [package]      | yarn remove [package]         |
| npm uninstall --save-optional [package] | yarn remove [package]         |
| npm cache clean                         | yarn cache clean              |
| rm -rf node_modules && npm install      | yarn upgrade                  |



##### npx 工具

如果全局安装 webpack，且在项目中本地安装webpack，在项目根目录使用 webpack，依然使用的是全局安装的 webpack，如果要使用本地安装的 webpack，需要输入 `./node_modules/.bin/webpack --version`，这是非常麻烦的，所以这时可以使用 npx webpack

另外，在 npm scripts 中 设置的script，会优先调用局部安装的 工具，比如 webpack，比如：

```json
"scripts": {
  "webpackVersion": "webpack --version"
}
```

npm run webpackVersion 时，会优先查找局部安装的 webpack，并调用。



##### 自定义终端指令

步骤：

- npm init 创建一个 package.json，其中必须要有 main作为入口文件；比如入口文件为：index.js

- 创建 index.js 文件，并写代码

  ```js
  #!/usr/bin/env node
  
  console.log('why')
  ```

  **第一行的代码表示：**<font size=4>**#!**</font> <font color=FF0000>是一个固定写法，也是一个指令，被叫做 hashbang / shebang；可以在 hashbang 后面配置一个环境，以执行当前的文件</font>；后面的是让系统去 /user/bin/env 文件中，去找node去执行。
  另外，上面也可以写绝对路径找到 node，但是为了跨系统间兼容，还是写上面的代码比较好。

- 在package.json 中加上 bin，以给指令命名，并指定 指令对应的执行文件。

  ```json
  "bin": {
    "why": "index.js"
  }
  ```

- 键入命令：`npm link`，将package.json 中的 bin 和 系统的环境变量 生成一个 “**软链接**”，将 `why` 作为终端命令配置到环境变量中

这时，可以输入命令 why，即可打印出 ‘why’。<font color=FF0000>**另外，这个字定义的指令，是全局可以调用的；尤其是在当前项目的外层**</font>。

除了可以键入命令 why，打印出 'why' 之外，一般而言还应该可以传递其他参数（命令选项），比如 `why --version` 。这时，需要（可以）使用 [commander](https://github.com/tj/commander.js) 这个工具库。

npm install commander 之后，在 入口文件 index.js 写入：

```js
#!/usr/bin/env node

const program = require('commander')

// 设置显示的值
program.version('1.0.0')
// 告诉命令行后去解析指令
program.parse(process.agrv)
```

此时，键入 why --version 将会打印出 1.0.0。

另外，此时键入 why --help，有打印结果：

```js
Usage: why [options]

Options:
  -V, --version  output the version number
  -h, --help     display help for command
```

这些内容是 program 中自带的，同时，这些东西也可以自定义的（见下面）。

还有：此时的 版本号 1.0.0 是写死的；而版本号肯定是会变的；这时候就要读取 package.json 中 version 字段。修改代码如下：

```js
#!/usr/bin/env node

const program = require('commander')

const version = require('./package.json').version
program.version( version )
program.parse( process.agrv )
```

上面的命令是可以自定义的。

- **比如：添加 why -v，以实现 why --version 一样的效果。**

  两种方法：

  - ```js
    program.version(version, '-v, --version')
    ```

    后面第二个参数，可以添加两个“参数”？如果添加了第三个，第三个写了也没有用。另外，这时候 why -V 命令会失效（被覆盖？）

  - 解决方法：多次写几遍

    ```js
    program.version(version, '-v')
    program.version(version, '-V, --version')
    ```

    还可以写更多遍。

  - 另外，如果只写了一个，比如 `program.version(version, '-v')`，那么 --version 和 -V都将无法使用。

- **比如：添加一条 键入 why --help 中输出的 Options：**

  <font color=FF0000>**注意，这里的options的意思是：可选参数，在option中定义的可选参数，在当前工具下（当前是why），都可以使用；并且可选参数的内容（\<...> 中的内容，比如说 \<dest>）保存在 program 对象中，即program.dest（关于 <...> 后面有说到）**</font>

  ```js
  // 添加一条 自定义的 options
  program.option('-w --why', 'a why cli')
  ```

  这是键入 why --help，输出：

  ```
  Usage: why [options]
  
  Options:
    -V, --version  output the version number
    -w --why       a why cli
    -h, --help     display help for command
  ```

  而仅仅这样作用不大，因为一般 可选指令后面一般带参数；所以，可以这样写：

  ```js
  // 这里的 <dest> 是一个变量，便于脚本获取参数对应的文件夹，下面也有打印这个用户输入的命令相关的参数。
  program.option('-d --dest <dest>', 'a destinatioin folder, e.g -d /src/components')
  
  program.parse(process.argv)
  
  // 获取所有<> 中定义的变量。
  const opts = program.opts()
  console.log(opts.dest)
  ```

  键入：`why -d /src/component`，这时候会打印出 `/src/components`（由console.log打印）。另外，如果使用了 -d，而没有输入 dest，将会报错。

- 监听指令：可以使用 program.on() 函数。

  ```js
  // 监听 --help 指令，一旦 --help 执行，就会触发后面的回调函数。
  program.on('--help', () => {
    console.log('-----------')
    console.log('you had called help command')
  })
  ```

由于index.js 中内容很多，所以需要做一个拆分：在项目的根目录中，创建lib 文件夹（一般工具库的开发的代码不放在src中，而是放在lib 中），lib 文件夹下 创建两个文件夹： core（核心文件）和 utils（服务工具）。在core下创建 help.js 将help 相关的代码都放到 help.js 中。

```js
// help.js
const program = require('commander')

const helpOptions = () => {
  program.option('-w --why', 'a why cli')
  program.option('-d --dest <dest>', 'a destinatino folder, e.g /src/components')
  program.option('-f --framework <framework>', 'your framework')
 
  program.on('--help', () => {
    console.log('------------------')
    console.log('you had called --help command')
  })
}
module.exports = helpOptions
```

在index.js 中引入：

```js
#!/usr/bin/env node

const program = require('commander')
const helpOptions = require('./lib/core/help.js')

const version = require('./package.json').version
program.version(version)
// 帮助和可选信息
helpOptions()

program.parse(process.agrv)

const opts = program.opts()
console.log(opts.dest)
```



由于做脚手架的内容有点多，无法完全记录，代码需要重构也不方便记录；所以直接见项目，项目中有笔记。



Node 中 util 模块中有 promisify 方法，可以将不支持 promise 的回调函数，转化为一个 promise。

用法：util.promisify(callbackFn) 返回一个函数：调用该函数，将会返回一个 promise



Vue-cli 中 比如 `vue create foo` 时，让用户选择使用 vue2还是vue3，用不用 vue-router vuex...的选择是通过 [inquirer.js](https://github.com/SBoudrias/Inquirer.js) 这个工具实现的。



ejs 文件是 Node 的模版引擎，同时也可以用作 脚手架生成前端文件的大概框架。



#### Node 中的 Buffer

Node 中 Buffer 是处理二进制数据的。

Buffer 相当于是一个字节的数组，数组中的每一项对于一个字节的大小

##### 创建Buffer

###### 方法一

```js
const msg = 'hello world'
const buffer = new Buffer(msg)
console.log(buffer) 
```

这是一个已经过时的方法，警告如下：

```
(node:81005) [DEP0005] DeprecationWarning: Buffer() is deprecated due to security and usability issues. Please use the Buffer.alloc(), Buffer.allocUnsafe(), or Buffer.from() methods instead.
// (Use `node --trace-deprecation ...` to show where the warning was created)
```

###### 方法二

```js
const buffer = Buffer.from(msg)
console.log(buffer) // <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64>

const msg2 = '你好啊'
const buffer2 = Buffer.from(msg2)
console.log(buffer2) // <Buffer e4 bd a0 e5 a5 bd e5 95 8a> 默认使用 UTF-8编码，想要修改编码
console.log(buffer2.toString('utf8')) // '你好啊'，toString() 用于解码。默认使用 UTF-8解码，所以这里 UTF8 可以不写。
// 另外，使用什么格式进行编码，就使用什么格式解码；否则，会出现乱码。
```

###### 方法三

Buffer.alloc(size, fill[, encoding])，略

<font color=FF0000>**Buffer 是一个数组，所以可以通过数组索引的方式修改其中的元素（内容）**</font>

在创建Buffer时，不会频繁地向操作系统申请内存，他会默认先申请一个 8 * 1024 个字节大小的内容，即 8kb。以后再创建Buffer，就向后面偏移。



在Node中，图片（二进制文件）的处理是比较麻烦的，可以使用 [**sharp**](https://github.com/lovell/sharp) 这个工具库。



#### 事件循环

**事件循环可以理解为：用户编写的 JS代码 和 浏览器 或者 Node（因为都有事件循环） 之间的桥梁**

- **浏览器的事件循环 **是一个我们编写的JavaScript代码和 <font color=FF0000>**浏览器API调用**</font>（setTimeout / AJAX / 监听事件等）的一个桥梁，桥梁之间他们<font color=FF0000>通过回调函数进行沟通</font>
- **Node的事件循环** 是一个我们编写的 JavaScript代码和 <font color=FF0000>**系统调用**</font>（file system, network等）之间的一个桥梁，桥梁之间他们<font color=FF0000>通过回调函数进行沟通的</font>

我们经常会说JavaScript是单线程的，但是<font color=FF0000>JavaScript的线程应该有自己的容器进程</font>：浏览器或者Node。

**浏览器是一个进程吗，它里面只有一个线程吗?**

- 目前多数的浏览器其实都是多进程的，当我们打开一个tab页面时就会开启一个新的进程，这是为了防止一个页面卡死而造成所有页面无法响应，整个浏览器需要强制退出
- 每个进程中又有很多的线程，其中包括执行JavaScript代码的线程

**但是 JavaScript 的代码执行是在一个单独的线程中执行的：**

- 这就意味着 JavaScript 的代码，在同一个时刻只能做一件事
- 如果这件事是非常耗时的，就意味着当前的线程就会被阻塞



**事件循环中并非只维护着一个队列，事实上是有两个队列:**

- 宏任务队列 ( macrotask queue ) : ajax、setTimeout、setinterval、DOM监听、UI Rendering 等
- 微任务队列 ( microtask queue ) : 
  - **Promise的then回调：**（<font color=FF0000 size=4>**在promise.then回调 前面的代码 还算同步代码**</font>）
  - **async / await：**
    - <font color=FF0000>**把await关键字后面执行的代码，看作是包裹在 (resolve, reject) => { ... } 中的代码**</font>
    - <font color=FF0000>**await 的下一条语句，可以看做 .then( res => { ... } ) 中的代码 ）**</font>
  - Mutation Observer API
  - queueMicrotask等

**事件循环对于两个队列的优先级是怎么样的?**

1. main script中的代码优先执行（编写的顶层script代码）

2. 在执行任何一个宏任务之前（不是队列，是一个宏任务），都会先查看微任务队列中是否有任务需要执行

   - 也就是 宏任务 执行之前，必须保证 微任务 队列是空的;

   - 如果不为空，那么就优先执行微任务队列中的任务（回调）

##### 事件循环面试题

```js
setTimeout(function () {
  console.log("set1");
  new Promise(function (resolve) {
    resolve();
  }).then(function () {
    new Promise(function (resolve) {
      resolve();
    }).then(function () {
      console.log("then4");
    });
    console.log("then2");
  });
});

new Promise(function (resolve) {
  console.log("pr1"); // 这里由于还没到promise.then() 所以还是同步代码，所以会第一个执行
  resolve();
}).then(function () {
  console.log("then1");
});

setTimeout(function () {
  console.log("set2");
});

console.log(2);

queueMicrotask(() => {
  console.log("queueMicrotask1")
});

new Promise(function (resolve) {
  resolve();
}).then(function () {
  console.log("then3");
});

// pr1
// 2
// then1
// queuemicrotask1
// then3
// set1
// then2
// then4
// set2
```



##### Node 中事件循环

浏览器中的EventLoop是根据HTML5定义的规范来实现的，不同的浏览器可能会有不同的实现；而<font color=FF0000>Node中是由 libuv实现的</font>。

我们来看在很早就给大家展示的Node架构图：

- 我们会发现libuv中主要维护了一个 EventLoop 和 worker threads（线程池）
- EventLoop负责调用系统的一些其他操作:文件的 I/O、Network、child-processes 等

libuv 是一个跨平台 的专注于 异步IO 的库

<img src="https://i.loli.net/2021/10/28/VIel4XGCSb2qaR8.png" alt="image-20211028004526334" style="zoom:50%;" />



查看Node 源码，其中 V8 和 libuv 都是放在 deps ( dependences ) 文件夹下的



##### 阻塞IO 和 非阻塞IO

**操作系统通常为我们提供了两种调用方式：阻塞式调用 和 非阻塞式调用：**

- **阻塞式调用：**调用结果返回之前，当前线程处于阻塞态（阻塞态 CPU是不会分配时间片的） ，调用线程只有在得到调用结果之后才会继续执行
- **非阻塞式调用：**<mark>调用执行之后，当前线程不会停止执行，只需要过一段时间来检查一下有没有结果返回即可</mark>。

**在开发中的很多耗时操作，都可以基于这样的非阻塞式调用**

- 比如网络请求本身使用了 Socket 通信，而 Socket本身提供了select模型， 可以进行非阻塞方式的工作
- 比如文件读写的IO操作，我们可以使用操作系统提供的基于事件的回调机制

**但是<font color=FF0000>非阻塞IO也会存在一定的问题</font>：**我们并没有获取到需要读取（我们以读取为例）的结果

- 那么就意味着为了可以知道是否读取到了完整的数据，我们需要频繁的去确定读取到的数据是否是完整的
- 这个过程我们称之为轮询 ( poll ) 操作

**那么这个轮询的工作由谁来完成呢？**

<mark>如果我们的主线程频繁的去进行轮询的工作，那么必然会大大降低性能</mark>。并且开发中我们可能不只是一个文件的读写，可能是多个文件；而且可能是多个功能：网络的IO、数据库的IO、子进程调用

<font color=FF0000> **libuv提供了一个线程池( Thread Pool )**</font>

- 线程池会负责所有相关的操作，并且会通过轮询或者其他的方式等待结果
- <font color=FF0000>当获取到结果时，就 <font size=4>**可以将对应的回调放到事件循环（某一个事件队列）中**</font></font>
- 事件循环就可以负责接管后续的回调工作，告知 JavaScript 应用程序执行对应的回调函数



##### 阻塞和非阻塞，同步和异步的区别

- **阻塞和非阻塞是对于<font color=FF0000>被调用者</font>来说的：**

  被调用者是 <font color=FF0000>系统调用</font>，<font color=FF0000>操作系统为我们提供了 阻塞调用 和 非阻塞调用</font>

- **同步和异步是对于<font color=FF0000>调用者</font>来说的：**

  调用者是 用户的程序

  - 如果程序在发起调用之后，<font color=FF0000>不会进行其他任何的操作，只是等待结果</font>，这个过程就称之为<font color=FF0000>同步调用</font>
  - 如果程序在发起调用之后，并<font color=FF0000>不会等待结果，继续完成其他的工作，等到有回调时再去执行</font>，这个过程就是<font color=FF0000>异步调用</font>



##### Node 中的事件循环

我们最前面就强调过：事件循环像是一个桥梁，是连接着应用程序的 JavaScript 和 系统调用 之间的通道：

- 无论是我们的文件IO、数据库、网络IO、定时器、子进程，在完成对应的操作后，都会将对应的结果和回调函数放到事件循环（任务队列）中
- 事件循环会不断的从任务队列中取出对应的事件（回调函数）来执行

**Node中一次完整的事件循环 Tick 范围很多阶段：**

- **定时器( Timers )：**本阶段执行已经被 setTimeout() 和 setInterval() 的调度回调函数。
- **待定回调( Pending Callback )：**对某些系统操作（如TCP错误类型）执行回调，比如 TCP连接时接收到 ECONNREFUSED
- **idle, prepare：**仅系统内部使用。
- **轮询( Poll )：**检索新的 I/O 事件，执行与 I/O 相关的回调
- **检测 ( Detect )：**setImmediate() 回调函数在这里执行。
- **关闭的回调函数：**一些关闭的回调函数，如: socket.on('close', ...)

<img src="https://i.loli.net/2021/11/05/mAuznb6RgIc3FVB.png" alt="image-20211105144738923" style="zoom:45%;" />

图源：[Node.js官方文档 - Guide - The Node.js Event Loop, Timers, and process.nextTick()](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)

##### Node 中的 宏任务 和 微任务

- **宏任务(macrotask )：**setTimeout、setInterval、I/O事件、setimmediate、close事件;
- **微任务(microtask)：**Promise的then回调、process.nextTick、queueMicrotask

**但是：Node 中的事件循环 不仅仅是 微任务队列 和 宏任务队列**

**一次事件循环 被称为 一个Tick，一次Tick 需要 <font color=FF0000 size=4>依次执行</font>：**

- **next tick 队列：**process.nexTick 
- **other 队列：**Promise.then、queueMircotask
- **timer 队列：**setTimeout、setInterval
- **poll 队列：**I/O 事件（占用时间最长）
- **check 队列：** setImmediate 
- **close 队列：** close 事件

```js
async function async1() {
  console.log('async1 start')
  await async2()
  console.log('async1 end')
}

async function async2() {
  console.log('async2')
}

console.log('script start')

setTimeout(function () {
  console.log('setTimeout0')
}, 0)

setTimeout(function () {
  console.log('setTimeout2')
}, 300)

setImmediate(() => console.log('setImmediate'));

process.nextTick(() => console.log('nextTick1'));

async1();

process.nextTick(() => console.log('nextTick2'));

new Promise(function (resolve) {
  console.log('promise1')
  resolve();
  console.log('promise2')
}).then(function () {
  console.log('promise3')
})

console.log('script end')

// script start
// async1 start
// async2
// promise1
// promise2
// script end
// nextTick1
// nextTick2
// async1 end
//  promise3
// setTimeout0
// setImmediate
// setTimeout2
```

如下代码，谁先执行？

```js
setTimeout(() => {
  console.log('setTimeout')
}, 0) // 注意，这里设置的是0

setImmediate(() => {
  console.log('setImmediate')
})
```

虽然，setTimeout 属于 timer，早于 setImmediate所属于的check，应该是 setTimeout -> setImmediate。但是，答案是都有可能。

因为setTimeout 会立即执行，而setTimeout 会将回调函数，放在一个地方暂存（存放时间为 setTimeout设置的事件，这里是0），而继续向下执行。这个流程，哪怕setTimeout设置为0，也是需要耗费时间的（一般认为至少4ms），这里将这个时间设为time1。而在这期间，事件循环队列 会进行初始化，初始化也是需要消耗时间的；这里这个时间设为time2。

- **如果 time1 < time2：**则事件循环队列初始化完毕后，setTimeout已经在 timer 队列中了，则会执行，其次执行 setImmediate
- **如果 time1 > time2：**则事件循环队列初始化完毕后，setTimeout还没在 timer 队列中，则会先执行setImmediate，等setTimeout被放进timer 队列中时，再执行。



#### Node中的 Stream

可以这样理解流：

- 是连续字节的一种表现形式和抽象概念
- 流应该是<font color=FF0000>可读</font>的，也是<font color=FF0000>可写</font>的

可以直接通过 readFile 或者 writeFile 方式读写文件，为什么还需要流呢？

- 直接读写文件的方式，虽然简单，但是无法控制一些细节的操作；比如从什么位置开始读、读到什么位置、一次性读取多少个字节；读到某个位置后，暂停读取，某个时刻恢复读取等等
- 这个文件非常大，比如一个视频文件，一次性全部读取并不合适

**事实上Node中很多对象是基于流实现的：**

- http模块的 Request 和 Response 对象
- process.stdout 对象

<font color=FF0000>**所有的流Stream 都是 EventEmitter 的实例**</font>

##### Node.js 中有四种基本流类型

- **Writable：**可以向其<font color=FF0000>写入数据的流</font>（ 例如 fs.createWriteStream()  ）
- **Readable：**可以从中<font color=FF0000>读取数据的流</font>（例如 fs.createReadStream() ）
- **Duplex：**<font color=FF0000>同时为Readable和的流Writable</font>（例如 net.Socket ）
- **Transform：**Duplex可以在写入和读取数据时修改或转换数据的流（ 例如 zlib.createDeflate() )

##### 读取 Stream 代码示例

```js
const fs = require('fs')

const reader = fs.createReadStream('./foo.txt', {
  start: 3, // 开始读的位置
  end: 6, // 结束读的位置
  highWaterMark: 2, // 每次读取多少数据，这里是两个字节   
})

// createReadStream 没有回调函数返回读取数据，只能通过监听事件来获取数据。这里的data 是读取一次触发的事件
// 另外，还有 open事件（文件被打开） 和 close事件（文件被关闭）
reader.on('data', (data) => {
  console.log(data)
  
  // 读取暂停
  reader.pause()
  
  // 一秒后继续读取
  setTimeout(() => {
    reader.resume()
  }, 1000)
})
```

##### 写入Stream 代码示例

```js
const writer = fs.createWriteStream('./bar.txt', {
  flags: 'a',
  start: 4,
})

writer.writer('你好啊', (err) => {
	if(err) {
	  console.log(err)
    return
  }
  console.log('写入成功')
})

// 关闭文件
writer.close()
// 另外，也可以使用writer.end() 以关闭，同时，end方法可以传入数据。
writer.end('hello world')

writer.on('close', () => {
  console.log('文件已关闭')
})
```

##### pipe 方法

###### 传统的写法

```js
// 读取文件，并写入
fs.readFile('./bar.txt', (err, data) => {
  fs.write('./baz.txt', data, (err) => {
    console.log(err)
  })
})
```

###### 用stream的方法（更加优雅）

```js
const reader = fs.createReadStream('./foo.txt')
const writer = fs.createWriteStream('./bar.txt')

reader.pipe(writer)
```



#### http开发

**Web服务器是：**当应用程序（客户端）需要某一个资源时，可以向一台服务器，通过Http请求获取到这个资源；提供资源的这个服务器就是一个Web服务器

##### http 模块代码示例

```js
const http = require('http')

const server = http.createServer((req, res) => {
	// 这里的res 继承自 stream，这里的res.end() 表示向流中添加数据，并关闭。
  // 等价于：res.write('hello world'); res.close()
  res.end('hello world')
})

// 第二个参数为 hostname：在哪台主机上。
// 第三个是启动成功的回调函数
server.listen(8888, 'localhost', () => {
  console.log('服务器启动成功')
})
// 如果在这里console.log('服务器启动成功')，其实是无效的，因为：这里是同步代码，无法判断服务器是否启动成功
```

另外，关于 server.listen() 也是可以挂在 server 定义后面的：

```js
const server = http.createServer((req, res) => {
  // ...
}).listen(8000, () => {
  console.log('服务器启动成功')
})
```

想要在开发时，可以实现 热替换，可以使用nodemon工具；在安装nodemon 之后，运行项目，需要输入命令：`nodemon yourEnter.js`

<font color=FF0000>在同一个入口文件中，可以创建多个 server</font>；多次通过 http.createServer() 创建server 即可。

另外，还可以通过更加原生的方法，创建服务器：

```js
const server = new http.Server((req, res) => {
  res.end('hello world')
})
server.listen(8888)
```

http.server() 这种方法和 createServer() 效果是一摸一样的，而且在源码中 createServer的实现就是 return http.server()

##### server.listen() 的参数

- server.listen() 中如果不设置一个端口号，也是可以的；这将会随机使用一个端口号。另外，可以通过 `serverName.address().port` 获取到端口号。
- 主机host，通常可以传入 localhost、127.0.0.1、0.0.0.0 ；同时，<font color=FF0000>默认是 0.0.0.0</font>
  - **localhost：**<font color=FF0000>本质上是一个域名，通常情况下会被解析成 127.0.0.1</font>
  - **127.0.0.1：**<font color=FF0000>回环地址</font> ( Loop Back Address )，表达的意思其实是：<font color=FF0000>我们主机自己发出去的包，直接被自己接收</font>
    - 正常的数据库包经常 应用层-传输层-网络层-数据链路层-物理层;
    - 而<font color=FF0000>回环地址，是**在网络层直接就被获取到了，是不会经常数据链路层和物理层**的</font>
    - 比如我们监听127.0.0.1时，在同一个网段下的主机中，通过 ip地址是不能访问的
  - **0.0.0.0**
    - 监听IPV4上所有的地址，再根据端口找到不同的应用程序
    - 比如我们监听0.0.0.0时，<font color=FF0000>在同一个网段下的主机中，通过ip地址是可以访问的</font>

##### request 对象

request 对象中 封装 了客户端给我们服务器传递过来的所有信息，重要的有：

- **req.url：**请求的地址
- **req.method：**请求方式
- **req.headers：**请求头

req.url 获取的内容是 请求处理protocol 和 site name 之外的完整地址，包含path query 以及 hash；当用户只想获得 path的时候，需要使用 url 模块；比如使用 url.parse( req.url ) 将会获得 url对象，其中包含各种信息；同时也有 想要的 pathname。另外，这里还可以使用解构赋值：

```js
const { pathname, query } = url.parse(req.url)
```

另外，这里的query 是 通过 & 连接的，无法直接转化对象，所以可以使用 queryString 进行解析。

```js
const qs = require('queryString')

const { query } = url.parse(req.url)
const {username, password} = qs.parse(query)
```

在原生的Node req 对象中，没有 body这个成员（虽然第三方的框架，会在req中添加body这个成员）。在Node 中获取 body的 方法：

```js
// body 中的数据是通过流形式传入的，所以可以通过 data 事件，回调获取数据
// 另外，由于传过来的是流，所以是二进制的；可以通过 toString() 进行解码，也可以通过设置req.setEncoding
// 方法一
req.on('data', (data) => {
  console.log(data.toString())
})

// 方法二
req.setEncoding('utf-8')
req.on('data', (data) => {
  console.log(data)
})
```

**req.headers 中的内容是 http request header：**

- 如果 accept-encoding 中包含 gzip编码，可以配置webpack，在打包时 对 js、css 文件进行压缩，将其压缩为gz 文件，同时，浏览器在加载时，可以直接将其解压并解析。

- Node 中，keep-alive 默认保持5秒钟，5秒钟后如果没有请求，则关闭当前会话。设个keepAliveTime是可以设置的，方法是：server.keepAliveTimeout( microSecNum )，默认值为 5000



##### reponse 对象

###### 响应结果

```js
res.write('hello world')
res.end('bye')
```

###### 响应状态码

设置状态码 的方法：

- 直接给 statusCode 属性赋值

  ```js
  res.statusCode = 200
  ```

- 与 header 一起设置

  ```js
  res.writeHead(503, {
    // header 的其他内容。这个对象是可选的
  })
  ```

- **响应header**

  设置 header 的方法：

  - setHeader：

    ```js
    res.setHeader('Content-Type', 'text/plain;charset=utf8')
    ```

  - writeHeader：

    ```js
    res.writeHead(404, {
      "Content-Type": "application/json"
    })
    ```



##### Node http 发送请求

Node 的 http 模块，不仅仅可以接收与处理浏览器的请求，也可以发送 http 的请求；比如作为中间层，比如做请求转发？

###### 发送 GET请求

```js
const http = require('http')

http.get('http://localhost:8000', (res) => {
  // 监听 服务器 返回的数据
  res.on('data', (data) => {
    console.log(data)
  })
})
```

###### 发送 POST请求

另外，http原生没有post方法，所以需要其他方法实现：

```js
// 另外，在使用 request 发送请求时，如果不告诉Node请求的内容配置 已经结束了，否则是不会发送请求的。
// 所以，需要接收 http.request 生成的变量，并告诉它要请求的内容的配置 已经结束（end）了，这时候才会发送请求。通知结束，需要使用 req.end()。而 http.get() 会自动做这件事。
const req = http.request({
  method: 'post',
  hostname: 'localhost',
  port: 8888,
}, (res) => {
  res.on('data', (data) => {
    console.log(data.toString())
  })
  
  res.on('end', () => {
    console.log('获得了请所有结果，关闭')
  })
})

req.end()
```

另外，如果在Node 中发送 网络请求，一般（更多情况下）是不会通过 原生方法去写的（太麻烦）；而是 会使用 Axios，<font color=FF0000>在Node 环境中，Axios会使用 Node的 http模块（进行封装）</font>，在浏览器环境中，使用 XMLHttpRequest。



##### Node 原生 文件上传

常见错误的方法：

```js
const server = http.createServer((req, res) => {
  const url = res.url
  const { path } = res.parse(url)
  if ( path === '/upload' && req.method === 'POST' ) {
    
    // 设置接收的编码，因为这里的默认编码是UTF-8；如果没有这行代码，图片即使数据正确，图片也会打不开。
    req.setEncoding('binary')
    
    const fileWriter = fs.createWriteStream('./foo.png', {flag: 'a+'})
    // 错误的开始。
    // 因为，这里是POST发送数据，发送的数据不一定只有图片，可能还有其他内容，比如请求body的内容，甚至还有提交的其他信息。
    // 这里将接收到的数据一股脑的作为图片数据存储，所以会导致图片数据被破坏，文件结构被破坏，导致图片错误。还有其他方式，只要思路是相同的，都是错误的。
    req.on('data', (data) => {
      fileWriter.write(data)
    })
    req.on('end', () => {
      res.end('文件上传成功')
    })
  }
})
```

将上面的data 的内容打印出来，会发现除了图片的内容，还有其他信息，包括：

- boundary：分隔符，用于：分隔多个文件、表单项。内容大致是 --------------XXXXX（开始分隔符？） 后面跟数据，后面再跟 -------XXXXX（结束分隔符？）。另外，同一个表单项的分隔符的 XXXXX 是一样的？（不确定）
- Content-Disposition
- name：指定的字段 键值对 中的键
- filename：上传文件（在客户端上真实）的名称
- filename*=UTF-8：上传文件名称（上面的filename）对应的UTF-8编码形式
- Content-Type
- 另外，Content-Type后面还有两个空行（\r\n\r\n，回车(\r)+换行(\n) 两次），作为分隔。

正确的做法，和上面类似，不过要专门截取数据，精准地提取出 图片内容。具体需要做的是：去掉 Content-Type，以及 （结束分隔符？）---------XXXXX。

这里需要使用 querystring 的 parse() 方法。qs.parse() 的声明：querystring.parse( str [, sep [, eq[, options] ] ] )

##### 其中

- str： 是需要被解析为对象的内容
- sep：是分隔为单个键值对字符串的参考值（比如，str经过sep的分割，被分为多个："key:value" ），默认值是 "&"
- eq：界定键值对的区分，默认值 是 "="；options 是 选项。

```js
const qs = require('querystring')

// 省略的内容，和上面的一样

req.setEncoding('binary')

req.on('end', () => {
	// 处理 body，拿到Content-Type的对象
  const payload = qs.parse(body, '\r\n', ': ')
  const type = payload['Content-Type'] // 拿到上传文件的 格式，比如 image/png
  
  // 开始从 image/png 的位置进行截取
  const typeIndex = body.indexOf(type) // type在body 中的位置
  const typeLength = type.lenth // type的长度
  const imageData = body.subString(typeIndex + typeLength)
  
  // 将 中间的空格去掉
  imageData = imageData.replace(/^\s\s*/, '') 
  // 不过，现在imageData 后面还是有 boundary，需要处理。
  
  // 将最后的 boundary 去掉
  // 获取 boundary 的内容
  const bounday = req.headers['content-type'].split(';')[1].split('=')[1]
  // 去掉boundary，获取真正的 imageData
  imageData = imageData.substring(0, imageData.indexOf(`--${boundary}--`))
  
  // 写入图片
  fs.writeFile('./foo.png', imageData, {encoding: 'binary'}, (err) => {
    res.end('文件上传成功')
  })
})
```



#### web框架（express、koa）

已经有了使用http内置模块来搭建Web服务器，为什么还要使用框架?

- <mark>原生http在进行很多处理时，会较为复杂</mark>
- <font color=FF0000>有URL判断、Method判断、参数处理、逻辑代码处理等，都需要我们自己来处理和封装</font>。并且所有的内容都放在一起，会非常的混乱

目前在Node中比较流行的Web服务器框架是express、koa。

express早于koa出现,并且在Node社区中迅速流行起来：我们可以基于express快速、方便的开发自己的Web服务器；并且可以通过一些实用工具和中间件来扩展自己功能。Express 整个框架的核心就是中间件，理解了中间件其他一切都非常简单（中间件可以理解为：回调函数）

##### express的使用过程有两种方式

- **方式一：**通过express提供的脚手架，直接创建一个应用的骨架。需要安装 express-generator

  1. 安装脚手架：npm install express-generator -g

  2. 创建项目：express express-demo

  3. 安装依赖：npm install

  4. 启动项目：node bin/www 或者 npm start（这个命令是express 官方推荐的）

- **方式二：**从零搭建自己的express应用结构

  1. npm init -y

  2. npm install express

  3. ```js
     // index.js
     const express = require('express')
     
     // express 其实是一个函数：createApplication，返回值是一个函数：app
     const app = express()
     
     // 监听默认路径
     app.get('/', (req, res, next) => {
       res.end('hello express GET Method')
     })
     app.post('/', (req, res, next) => {
       res.end('hello express POST Method')
     })
     // 监听 login
     app.post('/login', (req, res, next) => {
       res.end('welcome back')
     })
     
     // 开启监听
     app.listen(8000, () => {
       console.log('express is running')
     })
     ```



#### 中间件

Express是一个路由和中间件的Web框架，它本身的功能非常少。Express应用程序 <font color=FF0000>本质上是一系列中间件函数的调用</font>

##### 中间件是什么

- 中间件的本质是传递给 express 的一个回调函数
- **这个回调函数接受三个参数：**
  - 请求对象：request对象
  - 响应对象：response对象
  - <font color=FF0000>next函数</font>：<font color=FF0000>在express 中定义的用于执行下一个中间件的函数</font>

##### 中间件中可以执行哪些任务

- 执行任何代码
- 更改请求( request )和响应( response )对象
- 结束请求 ( res.end() ) - 响应周期（返回数据）
- 调用栈中的下一个中间件( next() )

![image-20211106171729596](https://i.loli.net/2021/11/06/FhXK7QbHN1OxzRm.png)

在中间件 ( req, res, next) => { ... } 中（这个函数确实是一个中间件），如果不调用 res.end()，就必须要调用 next()，否则这个请求响应的生命周期将一直不会结束，即：请求被挂起。

##### 如何将一个中间件应用到我们的应用程序中？

- express主要提供了两种方式：app.use / router.use 和 app.methods / router.methods
- 可以是 app，也可以是router
- methods指的是常用的请求方式，比如：app.get / app.post / app.all

##### 应用中间件示例

###### 最普通的中间件

```js
const express = require('express')

const app = express()

// 编写普通的中间件
// 用use 注册一个中间件（回调函数）
// 因为没有设置路径和请求方法，所以这个中间件，可以被任意的路径和方法请求执行
app.use((req, res, next) => {
  console.log('注册了一个最普通的中间件')
  // 如果第二个中间件加了res.end()，那么这里res.end()就不能加上，因为生命周期已经结束(end)了，还在调用res.end()，是不被允许的。一般 end都是放到最后一个执行的中间件中。
  //res.end('end')
  
  // 如果这里没有next()，下面定义的第二个中间件将不会执行。因为中间件默认（不加next()的情况）只会访问第一个配置上的中间价
  // 如果加上了 next()，将会查找下一个可以执行（可以匹配上）的中间件。
  // 第三个中间件同样
  next()
})

// 这里注册了第二个中间件，如果上面的中间件没有调用 next()，这个定义的中间件将不会执行。
app.use((req, res, next) => {
  console.log('注册了一个最普通的中间件2')
  next()
  res.end('end')
})

app.listen(8000, () => {
  console.log('普通的中间件启动成功')
})
```

###### 路径匹配中间件

```js
app.use('/home', (req, res, next) => {
  console.log('home middleware')
  res.end（'home middleware')
})
```

###### 路径和方法匹配的中间件

```js
app.get('/home', (req, res, next) => {
  console.log('path and method middleware')
})
```

###### 连续注册中间件

```js
app.get('/home', (req, res, next) => {
  console.log('path and method middleware')
  next()
}, (req, res, next) => {
  console.log('path and method middleware')
  next()
}, (req, res, next) => {
  console.log('path and method middleware')
  next()
})
```

###### 中间件json 解析

```js
app.post('/login', (req, res, next) => {
  req.on('data', (data) => {
    console.log(data.tostring)
  })
  req.on('end', () => {
    res.end('welcome')
  })
})

app.post('/products', (req, res, next) => {
  req.on('data', (data) => {
    console.log(data.tostring)
  })
  req.on('end', () => {
    res.end('upload products')
  })
})

//... 假设还有更多解析JSON的中间件，会发现一个问题，在每个中间件中都要处理JSON，这是非常麻烦的。
// 这时候，可以在最上面加上一个专门解析 JSON的中间件。
```

```js
app.use((req, res, next) => {
  if(req.headers['content-type'] === 'application/json') {
    // 专门解析json
    req.on('data', (data) => {
			const info = JSON.parse(data.toString())
			// 因为next()中不能添加参数（传递解析结果），所以这里用res.body放置解析结果，方便后面的中间件获取 解析结果
      res.body = info
    })
    req.on('end', () => {
      next()
    })
  } else {
    next()
  }
})
// 如果用户使用的格式不是 json，可以编写其他的中间件，进行处理

// 另外，有一个工具叫做：body-parser，可以作为解析的中间件，body-parser是express中的一个内置函数（从4.16.x 开始。另外，3.x中也是内置的，4.x中被移除，直到4.16.x 成为内置函数）。使用方法如下：
app.use(express.json()) 
// 类似的，对于urlencoded格式，有 express.urlencoded()。另外，urlencoded方法有参数：{extened: (true | false)}；如果为true，则使用第三方的库qs；如果为false，则使用node内置模块querystring

app.post('/login', (req, res, next) => {
```

如果想要设置一个解析 formdata格式 数据的中间件（使用formdata格式的目的，主要是上传文件），除了自己写，可以使用 [multer](https://github.com/expressjs/multer)工具库，这是express官方开发的，但是没有内置到 express中，需要手动安装。

```js
const multer = require('multer')

const upload = multer() // 相当于创建了一个multer对象
app.use(upload.any()) // 设置中间件，另外，根据官方文档：不建议将 upload.any() 作为全局中间件使用；所以，这里的代码，不建议这样写；建议直接放在中间件中（见下面 /login 的示例）。
```

###### 使用 multer 保存上传的文件

```js
const path = require('path')

const	storage = multer.diskStorage({
  // 存放文件的文件夹
  destination: (req, file, callback) => {
    callback(null, './upload/')
  },
  // 文件名
  filename: (req, file, callback) => {
    // Date.now() 时间戳动态设置文件名，保证不会重复
    // path.extname(...) 获取文件扩展名
    callback(null, Date.now() + path.extname(file.originalname))
  }
})

const upload = multer({
  // 设置文件保存的位置，另外，设置dest，上传的文件将没有后缀名，因为它不知道。否则，需要使用storage
  // dest: './upload/'
  storage
})

app.post('/login', upload.any(), (req, res. next) => {
  console.log(req.body)
  res.end('用户登录成功')
})

// 如果上传单个文件，则设置为 upload.single；若为多个，则为 upload.array
app.post('/upload', upload.single('file'), (req, res, next) => {
  res.end('文件上传成功')
})
```



##### express保存日志信息

如果我们希望将请求日志记录下来，那么可以使用express官网开发的第三方库: [morgan](https://github.com/expressjs/morgan)，使用示例：

```js
const fs = require('fs')

const morgan = require('morgan')

// 创建 写入流，并设置写入的日志文件，和写入模式
const writerStream = fs.createWriteStream('./log/access.log', {
  flags: 'a+'
})
// combined所对应的参数：是设置保存日志的格式
app.use(morgan('combined', {stream: writerStream}))

// 作为测试的中间件，访问该中间件，看看是否会添加日志
app.get('/home', (req, res, next) => {
  res.end('hello world')
})
```



##### 客户端传递到服务器参数的方法常见的是5种

- 通过get请求中的URL的params（类似于 Vue-router中动态路由）
- 通过get请求中的URL的query
- 通过post请求中的body的json格式（中间件中已经使用过）
- 通过post请求中的body的x-www-form-urlencoded格式（中间件使用过）
- 通过post请求中的form-data格式（中间件中使用过）

###### params 参数的处理

```js
app.get('/products/:id', (req, res, next) => {
  console.log(req.params) // req.params 是一个对象，同时，路径可以是/products/:id/:name，所以params可能有多个成员
})
```

###### query 参数的处理

```js
app.get('/login', (req, res, next) => {
	// req.query 是一个对象，所以这里不需要 querystring的解析
  console.log(req.query)
})
```



##### express的response响应数据和状态

res.end() 中的返回的内容必须是 字符串。如果想要返回json格式，需要先进行 JSON.stringify()：

```js
app.get('/login', (req, res, next) => {
	res.type("application/json")
  res.end(JSON.stringify({name: 'foo', age: 18}))
})
```

这种方式是比较麻烦的，也是不推荐的。还有更好的方法：

```js
res.json({name: 'foo', age: 18})
res.json(['abc', 'def', 'ghi'])
```

想要返回状态码：

```js
res.status(204)
```



##### express的路由

如果我们将所有的代码逻辑都写在app中，那么app会变得越来越复杂：

- 一方面完整的Web服务器包含非常多的处理逻辑
- 另一方面有些处理逻辑其实是一个整体，我们应该将它们放在一起：比如对users相关的处理
  - 获取用户列表
  - 获取一个用户信息
  - 创建一个新的用户
  - 删除一个用户
  - 更新一个用户

**我们可以使用 Express.Router来创建一个路由处理程序：**

- <font color=FF0000>一个 Router 实例拥有完整的中间件 和 路由系统</font>
- 因此，它<font color=FF0000>也被称为迷你应用程序 ( mini-app )</font>

示例：

- 定义Router：创建一个 routers 文件夹，并创建 users.js 文件

  ```js
  // users.js 关于users的操作，get / post / delete 都在这个文件中
  const express = require('express')
  
  const router = express.Router()
  
  // 这里省略的 /users/ 路由 由调用处（见使用 Router处代码）中添加
  router.get('/', (req, res, next) => {
    res.json(['foo', 'bar', 'baz'])
  })
  
  
  router.post('/:id', (req, res, next) => {
    res.json(`${req.params.id}用户信息`)
  })
  
  router.post('/', (req, res, next) => {
    res.json('create user success')
  })
  
  module.exports = router
  ```

- 使用 Router：

  ```js
  const userRouter = require('./routers/users.js')
  
  const app = express()
  
  // 添加路由路径，给定义处省略的路由添加路径
  app.use('/users', userRouter)
  
  app.listen(8000, () => {
    console.log('...')
  })
  ```

在源码的实现中：无论是 app.use()、还是 app.get() ... 实际上都是在调用路由（通过 lazyRouter() 创建一个路由，这个路由一般称为：主路由）



##### express 作为静态服务器

```js
const express = require('express')

const app = express()

// 只需要添加一行代码即可！
// express.static() 中传递的是：想要将哪一个文件夹，作为静态资源的根目录
app.use(express.static('./build'))

app.listen(8000, () => {
  console.log('静态资源服启动成功')
})
```

express 确实可以作为静态服务器，但是选择nginx 是更好的选择



##### express 的错误处理

比如在 login 接口中，用户提交用户名和密码，数据库查询发现用户不存在；或者 在register 接口中，用户提交用户名和密码，数据查询发现用户已存在。<font color=FF0000>这里的判断逻辑是非常相似的</font>，所以，<font color=FF0000>对于错误处理，可以专门做一个 中间件</font>。

```js
const express = require('express');

const app = express();

const USERNAME_DOES_NOT_EXISTS = "USERNAME_DOES_NOT_EXISTS";
const USERNAME_ALREADY_EXISTS = "USERNAME_ALREADY_EXISTS";

app.post('/login', (req, res, next) => {
  // 加入在数据中查询用户名时, 发现不存在
  const isLogin = false;
  if (isLogin) { res.json("user login success~");} 
  else {
    // res.type(400);
    // res.json("username does not exists~")
    next(new Error(USERNAME_DOES_NOT_EXISTS));
  }
})

app.post('/register', (req, res, next) => {
  // 加入在数据中查询用户名时, 发现不存在
  const isExists = true;
  if (!isExists) { res.json("user register success~"); } 
  else {
    // res.type(400);
    // res.json("username already exists~")
    next(new Error(USERNAME_ALREADY_EXISTS));
  }
});

// 错误处理中间件，回调函数有四个参数。
app.use((err, req, res, next) => {
  let status = 400;  let message = "";
  console.log(err.message);

  switch(err.message) {
    case USERNAME_DOES_NOT_EXISTS:
      message = "username does not exists~";
      break;
    case USERNAME_ALREADY_EXISTS:
      message = "USERNAME_ALREADY_EXISTS~"
      break;
    default: 
      message = "NOT FOUND~"
  }

  res.status(status);
  res.json({
    errCode: status,
    errMessage: message
  })
})

app.listen(8000, () => {
  console.log("路由服务器启动成功~");
});

```



##### express 源码阅读

// 需要重看...



### koa

Koa官方的介绍:

> koa : next generation web framework for node.js;

**事实上，koa是express同一个团队开发的一个新的Web框架：**

- 目前团队的核心开发者 TJ 的主要精力也在维护Koa，express已经交给团队维护了;
- Koa旨在为Web应用程序和API提供更小、更丰富和更强大的能力
- 相对于express具有更强的异步处理能力（后续我们再对比）
- Koa的核心代码只有1600+行，是一个更加轻量级的框架，我们可以根据需要安装和使用中间件

##### 示例

###### 没有中间件

```js
// express中导入的是一个createApplication的函数
// 而在Koa中导入的是一个叫做Application 的类，所以这里的Koa最好首字母大写
const Koa = require('koa')

const app = new Koa()

// 目前这里 koa 没有设置任何中间件，如果进行访问，将会主动返回 Not Found；而在 Express中是直接挂起，没有返回
// 不同于 express，express 中如果不主动的去掉用 res.end()，这个中间件将会请求一直挂起；导致客户端一直在请求。所以客户端需要设置一个超时时间，如果超时，则客户端主动将请求关闭。
// koa 在所有的中间件都执行完后，依然没有返回结果，它会主动返回一个Not Found

app.listen(8888, () => {
  console.log('服务器已启动')
})
```

###### 写上中间件

```js
// 第一个参数是 ctx，即context，上下文；其中的内容有：ctx.request、ctx.response
// 第二个参数是 dispatch，为了习惯写作：next
// 这时候，再访问，还是会返回 Not Found。是因为上面所说：koa 在所有的中间件都执行完后，依然没有返回结果，它会主动返回一个Not Found。这里没有返回结果。
app.use((ctx, next) => {
  console.log('中间件被执行')
  // 这里同样可以使用 next()，让下一个中间件执行。
})

// 带上返回的中间件，返回的内容通过 ctx.response.body 返回。
// body 中传递的内容可以是 字符串、对象、数组、buffer...
app.use((ctx, next) => {
  ctx.response.body = 'hello world'
})
```

**koa 通过创建的app对象，注册中间件只能通过use方法：**

- **Koa 并没有提供 methods 的方式来注册中间件**：没有提供类似 app.get()、app.post() 等方法，也没有提供一个app.use() 中注册多个中间件的方法（express中是可以的）
- **也没有提供path中间件来匹配路径**：没有 app.use('/path', () => {...} ) 的用法。

**也就是说，koa 中的用法，似乎更接近原生Node http的用法**。示例如下：

```js
app.use((ctx, next) => {
  if(ctx.request.url === '/login') {
    if(ctx.request.method === 'POST') {
      ctx.response.body = 'login success'
    }
  } else {
    ctx.response.body = 'other request'
  }
  
  // 如果这里加上了 一个 ctx.response.body = '...'的返回，将会返回 '...' 中的内容，而不是上面的 login success / other require。甚至这样的用法在 express 中会报错，因为这类似于 res.end() 之后再 res.end()
  ctx.response.body = 'foobar'
})
```

这样的方法写中间件，接近原生Node http，但是确实需要一个一个写中间件，太复杂，而且麻烦；所以可以通过路由的方式写中间件。

另外，由于koa是轻量级的框架，所以 路由需要使用 第三方的，最常用的 koa路由是：[koa-router](https://github.com/ZijianHe/koa-router)。在项目中安装即可。和express那边一样，创建一个 router文件夹，专门放置 router。

```js
// user.js

// 这里和koa的引入一样，引入了一个类
const Router = require('koa-router')

// 这里的 prefix 表示路由的前缀，后面在写中间件时，就不需要再加了
const router = new Router({prefix: '/users'})

// koa-router 是支持 methods方法和路径 来注册中间件的
// 这里由于上面加了前缀 /users，所以就不需要加上再写 /users 了
router.get('/', (ctx, next) => {
  ctx.response.body = 'user list'
})

// 路由导出
module.exports = router
```

路由导出了之后需要在使用的地方导入：

```js
// index.js
const Koa = require('koa')
// 引入 user路由
const userRouter = require('./router/user')

const app = new Koa()
// 注册 引入的userRouter
app.use(userRouter.routes())
// 由于 userRouter 中只设置了 get 方法，并没有设置post，如果用户使用post方法进行请求，这时会返回 Not Found，这对于用户是不太友好的，毕竟用户不知道发生了什么。这时可以使用下面koa-router 提供的方法，如果用户请求了没有定义的方法，会返回 "Method Not Allow"（对于put、delete、patch方法，并返回状态码：405）、"Not implement"（对于link、copy、lock等方法，并且会返回501） 等提示
app.use(userRouter.allowMethds())

app.listen(8000, () => {
  console.log('koa server is running')
})
```

##### koa 参数解析

在（原生的）koa 中，可以通过 ctx.request.url 获取路径，通过 ctx.request.query 获取请求的query参数；但是无法获取到 params。这时就需要通过路由 koa-router 获取。

通过koa-router 路由获取params 示例：

```js
const userRouter = new Router({prefix: '/users'})

userRouter.get('/:id', (ctx, next) => {
  console.log(ctx.request.params)
})
```

对于 post方法传来的 json数据，（原生的）koa无法通过 ctx.request.body 等方法获取到 上传的json数据。还是要通过第三方工具获取，比较常用的是 [koa-bodyparser](https://github.com/koajs/bodyparser)，代码示例如下：

```js
const bodyParser = require('koa-bodyparse')

// 注册 bodyParser
app.use(bodyParser())

// 下面的代码和之前一样
app.use((ctx, next) => 
  // 这里的解析是无感的、自动的，无论是传入 json数据还是urlencoded数据，都是可以自动解析（键值对的格式会变成对象）。另外，对于formdata类型的数据，无法进行解析
  console.log(ctx.request.body)
})
```

对于 formdata 数据的解析，还是要使用另一种第三方工具：[koa-multer](https://github.com/koajs/multer)，和express 中的 multer 同名，只不过加上了 koa的前缀。代码示例如下：

```js
const multer = require('koa-multer')

// 如果是文件上传，可以给multer()函数中加上上传文件的存储路径
const upload = multer()

// 不推荐全局注册upload.any()，会带来意外的安全风险。还是期望在路由中这样使用：Router.post('/login', upload.any(), (ctx, next) => {})
// 所以，下面这行代码只是为了演示，不建议使用。
app.use(upload.any())

app.use((ctx, next) => {
	// 注意：这里不是 ctx.require了，而是 ctx.req了；解析后body数据放在这里。
  cosole.log(ctx.req.body)
})
```

##### koa处理上传文件，比如上传头像

示例如下：

```js
// upload.js 专门处理上传的路由
const Router = require('koa-router')
const multer = require('koa-multer')

const uploadRouter = new Router({prefix: '/upload'})

const storage = multer.diskStorage({
  destination: (...) => {...}, // 和上面的express multer一样，略
  filename: (...) => {...} 
})

// 同样，如果没有storage，而指定了 dest，上传后的文件是没有后缀名的。和express一样，需要使用 storage
const upload = multer({
  // dest: './uploads/'
  storage
})

uploadRouter.post('/avator', upload.single('avator'), (ctx, next) => {
  console.log(ctx.req.file)
  ctx.request.body = '上传头像成功'
})
```



##### koa中数据的响应

**输出结果：body将响应主体设置为以下之一：**

- string：字符串数据

  ```js
  ctx.response.body = 'hello world'
  ```

- Buffer：Buffer数据

- Stream：流数据

- Object 或 Array：对象或者数组

  ```js
  ctx.response.body = {
    foo: 'bar',
    baz: 'quz'
  }
  ctx.response.body = ['foo', 'bar', 'baz']
  ```

- null：不输出任何内容

- 如果response.status 尚未设置，Koa会自动将状态设置为200或204

  ```js
  ctx.status = 204
  ```

另外，除了 ctx.response.body的写法，<font color=FF0000>还有简洁的写法：ctx.body = '...'</font>；这样写，本质上还是在执行ctx.response.body； 这是通过代理实现的。ctx.request 也一样。



##### koa部署静态资源

需要使用第三方库：[koa-static](https://github.com/koajs/static)

```js
// 不推荐使用static作为变量名，因为很有可能是关键字；所以这里是staticAsset
const staticAsset = require('koa-static')

// 注册 koa-static，并制定静态资源的位置。
app.use(staticAsset('./build'))
```



##### koa错误处理

koa 给出了多种错误处理的方式，常用的有：

```js
app.use((ctx, next) => {
  const isLogin = false
  if(!isLogin) {
    // 由于，一般中间件是写在路由中的，无法直接获取到app对象（除非导出）；但是，可以通过ctx.app获取到app
    // 触发一个错误
    ctx.app.emit('error', new Error('您还没有登录'), ctx)
  }
})

app.on('error', (err, ctx) => {
  // 这里用 switch比较多，这里略
  ctx.status = 401
  ctx.body = err.message
})
```



##### koa 和 express 的区别

###### 从架构设计上来说

- express是完整和强大的,其中帮助我们内置了非常多好用的功能;
- koa是简洁和自由的,它只包含最核心的功能,并不会对我们使用其他中间件进行任何的限制。
  - 甚至是在app中连最基本的get, post都没有给我们提供;
  - 我们需要通过自己或者路由来判断请求方式或者其他功能;
- 因为express和koa框架他们的核心其实都是中间件：
  - 但是他们的中间件事实上，它们的中间件的执行机制是不同的：express中的next() 是同步代码实现的，所以如果下一个中间件有异步操作，则不会等待其执行完成（如果使用 Node promisify将其变成一个promise，是否可以改变？）。而koa 中的next() 是通过promise 实现的，所以下一个中间件有异步处理，如果加上promise 或者 async / await 就会等待。



##### 洋葱模型

**两层理解含义：**

- **中间件代码的处理过层：**从中间层1执行 -> 中间层2执行 -> ... 中间层n-1执行 -> 中间层n执行 -> 中间层n-1执行 -> ... -> 中间层2执行 -> 中间层1执行 -> 结束；就像是下图经过洋葱一样。

  另外，洋葱模型一般指koa的，其实express也是有洋葱模型的，但是只针对于同步代码；而koa 对于异步代码也可以。

- **response 返回 body执行**

<img src="https://i.loli.net/2021/11/07/zfsRkQwWNKIvdr7.png" alt="image-20211107151535567" style="zoom:40%;" />



#### mysql

- 将mysql路径写入环境变量

  ```sh
  export PATH=$PATH:/usr/local/mysql/bin
  ```

- 登录：

  ```sh
  mysql -uroot -pYourPassword
  ```

- 离开mysql 命令行：

  ```sql
  quit
  ```

  

##### MySQL默认的数据库

- **infomation-schema：**信息数据库,其中包括MySQL在维护的其他数据库、表、列、访问权限等信息
- **performance-schema：**性能数据库,记录着MySQL Server数据库引擎在运行过程中的一些资源消耗相关的信息；
- **mysql：**用于存储数据库管理者的用户信息、权限信息以及一些日志信息等
- **sys：**相当于是一个简易版的performance-schema ,将性能数据库中的数据汇总成更容易理解的形式



##### 在Navicat中，表有这样属性

- **字符集：**即存储数据的编码，默认使用 utf8mb4，不建议使用直接使用utf8，因为utf8无法存储emoji
- **排序规则：**在select中 orderby prop <font color=FF0000>asc / desc </font> 中的正序倒序的排序规则。
  - **ai / as**
    - ai：排序时候不区分轻重音
    - as：排序时候区分轻重音
  - **ci / cs**
    - ci：不区分大小写
    - cs：区分大小写

这些属性在DDL 中也会有体现。

另外，数据库中是不会直接存储二进制文件的，比如图片、视频，一般这些东西都会存一个url在数据库，而数据存在文件系统中。



##### 常见的SQL语句我们可以分成四类

- **DDL** ( Data Definition Language )：数据定义语言。可以通过DDL语句对数据库或者表进行:创建、删除、修改等操作

  - 显示所有数据库：

    ```sql
    show databases;
    ```

  - 创建数据库：

    ```sql
    create database youDatabaseName;
    # 创建一个不存在的dbName数据库
    create database if not exists dbName;
    # 创建一个不存在的dbName数据库，且编码为 utf8mb4，排序规则为 utf8mb4_0900_ai_ci
    create database if not exists dbName default character utf8mb4 collate utf8mb4_0900_ai_ci;
    ```

  - 删除数据库：

    ```sql
    # 删除数据库，如果它存在的话；如果不加 if exists，且不存在，会报错
    drop database if exists XX;
    ```

  - 修改数据库属性：

    ```sql
    # 修改数据库 编码 和排序规则
    alter database dbName character set = utf8 collate = utf8mb4_0900_ai_ci
    ```

  - 进入某一个数据库 / 切换的目标数据库：

    ```sql
    use databaseName;
    ```

  - 查看当前使用的数据库：

    ```sql
    select database();
    ```

  - 显示当前数据库所有的表：

    ```sql
    show tables
    ```

  - 创建表：

    ```sql
    create table if not exists `tblName` (
    	# ...
    )
    ```

  - 补充：果想要设置 创建时间 和 修改时间的默认值，可以这样。

    ```sql
    create table if not exists `tblName` {
    	createTime timestamp default current_timestamp; # 默认值当前时间
    	updateTime timestamp default current_timestamp
      						on update current_timestamp; # 添加记录时，记录当前时间；在更新记录时，记录修改的当前时间
    }
    ```

    另外，不仅仅这个可以使用在创建表的语句中，还可以用在 修改表结构的语句中。

  - 删除表：

    ```sql
    drop table if exists `tblName`;
    ```

  - 查看表结构：

    ```sql
    desc tblName
    ```

  - 查看创建表时，写的命令：

    ```sql
    show create table `tblName`
    ```

    在查看结果时，会发现结果和当时写的sql代码不一样；这是因为mysql 会自动加上一些遗漏的信息

  - 修改表名：

    ```sql
    alter table `oldTblName` rename to `newTblName`
    ```

  - 添加新列：

    ```sql
    alter table `tblName` add `modifiedTime` timestamp
    ```

  - 修改字段名

    ```sql
    alter table `tblName` change `oldField` `newField` varchar(20)
    ```

  - 修改字段类型：

    ```sql
    alter table `tblName` modify `FieldName` varchar(20)
    ```

  - 删除某个字段：

    ```sql
    alter table `tblName` drop `dropedField`
    ```

  - 根据一张表结构去创建另一张表（借鉴的表只会复制结构，不会复制其中内容）

    ```sql
    create table `userLike` like `user`
    ```

  - 根据另一张表中的内容，去创建一张新的表。这张表只会复制内容，不会复制表结构（比如约束之类）

    ```sql
    create table `userCopy` as (select * from `user`)
    ```

  - 在已经定义好的表中添加外键

    ```sql
    # 如果这个字段不存在的话，先添加字段
    alter table `tblName` add `brand_id` int;
    # 将添加到字段设置外键
    alter table `tblName` add foreign key(brand_id) references brand(id)
    ```

  - **外键存在时更新和删除数据：**

    当一个字段作为另一张表的外键存在时，使用正常的sql语句，是无法修改这个字段的内容的。

    如果用户确实希望可以更新？这时<font color=FF0000>需要修改on delete（删除时）或者on update（更新时）的值</font>。这两个的叫做Action

    **我们可以给更新或者删除时设置几个值：**

    - **RESTRICT**（<font color=FF0000>**默认属性**</font>）：<font color=FF0000>当更新或删除某个记录时，会检查该记录是否有关联的外键记录，有的话，会报错的， 不允许更新或删除</font>
    - **NO ACTION：**和RESTRICT是一致的，是在SQL标准中定义的
    - **CASCADE：**
    - <font color=FF0000>当更新或删除某个记录时，会检查该记录是否有关联的外键记录，有的话</font>：
      - **更新：**那么会更新对应的记录
      - **删除：**那么关联的记录会被一起删除掉
    - **SET NULL：**当更新或删除某个记录时，会检查该记录是否有关联的外键记录，有的话，将对应的值设置为 NULL

    **所以如果需要更新删改数据，需要写如下sql语句（也似乎没有更好的、更方便的解决方法了）：**

    ```sql
    # 查看创建表时的sql语句
    show create table `products`
    # 根据名称将外键删除
    alter table `products` drop foreign key foreignKeyName
    # 重新添加外键约束，并设置on update、on delete这两个Action的策略。推荐：on update 级联修改，on delete 不联动
    alter table `products` add foreign key (brand_id) references brand(id)
    																						      on update cascade
    																						      on delete restrict
    # 这时候就可以进行更新外键了
    update `brand` set `id` = 100 where `id` = 1; # 这时，修改外键所在的表上的这个值，对应的值会级联修改
    ```

- **DML** ( Data Manipulation Language )：数据操作语言。可以通过DML语句对表进行：添加、删除、修改等操作

  - 添加记录：

    ```sql
    # 添加记录内容和 表结构一样
    insert into `user` values (110, 'why', '020-110110', '2020-10-20', '2020-11-11');
    # 添加记录内容和 表结构不一样（只添加指定字段）
    insert into `user` (name, telephone, 'createTime', 'updateTime')
    						values ('kobe', '000-111111', '2020-10-10', '2030-10-20')
    ```

  - 删除数据：

    ```sql
    delete from `user` where id = '100'
    ```

  - 更新数据：

    ```sql
    update `user` set name = 'foo', telephone = '010-1111' where id = 115
    ```

- **DQL** ( Data Query Language )：数据查询语言。可以通过DQL从数据库中查询记录

  select 语句语法：

  ```sql
  SELECT select_expr [, select_expr]...
            [FROM table_references]
            [WHERE where_condition]
            [ORDER BY expr [ASC | DESC]]
            [LIMIT {[offset,] row_count | row_count OFFSET offset}]
            [GROUP BY expr]
            [HAVING where_condition]
  ```

  另外，这个只是老师的总结。在mysql的官网可以看到更详细（也有很多会用不到、或很少很少用到）的语法，参见：https://dev.mysql.com/doc/refman/8.0/en/select.html

  - 查询中给字段起别名，另外，这里的 as 关键字是可以省略的

    ```sql
    select foo as bar, baz as quz from `tblName`
    ```

  - 不等于可以用 `!=` 也可以用 `<>`

    ```sql
    select * from `tblName` where num != 100
    select * from `tblName` where num <> 100
    ```

  - 逻辑运算语句：

    ```sql
    # 大于 1000 且小于 200
    select * from `tblName` where price > 1000 and price < 2000;
    select * from `tblName` where price > 1000 && price < 2000
    # between and 是“左右都闭” 的
    select * from `tblName` where price between 1000 and 2000
    ```

  - 查询某一个记录中的字段为null，用 `is null` 或者 `= null`

    ```sql
    select * from `tblName` where url is null;
    select * from `tblName` where url = null
    
    # 反过来，不为 null
    select * from `tblName` where url is not null;
    select * from `tblName` where url != null
    ```

    另外，也可以使用 `<=>` 来判断是否为null

    详见：[stackoverflow - What is this operator <=> in MySQL?](https://stackoverflow.com/questions/21927117/what-is-this-operator-in-mysql)

  - 模糊查询 使用 like关键字，和 `_`（表示一个字符）、`%`（表示零个 到 多个字符）

    ```sql
    select * from `tblName` where brand like '_p%';
    ```

  - in 关键字 表示取多个值中的其中一个即可

    ```sql
    select * from `tblName` where brand in ('apple', 'mi', 'oneplus')
    ```

  - 结果排序：order by。先根据第一个进行排序，如果第一个相同，则根据第二个，以此类推。

    ```sql
    select * from `products` order by price ASC, score DESC;
    ```

  - 分页查询：

    语法：  `[ LIMIT {[offset,] row_count | row_count OFFSET offset }]`

    ```sql
    # 下面两种写法的效果是一样的。
    # LIMIT limit_num OFFSET offset_num
    select * from `tblName` limit 20 offset 0;
    # LIMIT offset_num limit_num
    select * from `tblName` limit 0 20;
    ```

  - 使用 聚合函数，默认结果的字段名是 `聚合函数名(字段)`，这时可以用 重命名：

    ```sql
    select sum(count) as sum from `tblName`
    # 省略 as
    select sum(count) sum from `tblName`
    
    # 计算个数
    select count(*) from `tblName` where brand = 'apple'
    # 计算不同价格 的数量
    select count(distinct price) from `tblName`
    ```

    另外，聚合函数默认将所有数据看作一个分组，所以如果在查询字段中添加 非聚合函数的字段，将会报错。示例如下：

    ```sql
    # 是会报错的，不能加上brand
    select brand, sum(count) as sum from `tblName`
    ```

    而下面的 分组中可以，但是也仅仅是根据分组的那个字段。

  - 分组：

    ```sql
    # 以brand 建立分组，并分别计算一些数据
    select brand, avg(price), count(*), avg(score) from `products` group by brand
    # 下面的语句是会报错的，因为sql不知道price是什么，而brand是分组的凭证，所以sql知道
    select price, avg(price), count(*), avg(score) from `products` group by brand
    ```

  - 在分组中添加 条件，需要使用 having。之所以不能继续使用 where的原因：

    - 首先 where 不能放到 group by后面，会造成语法错误
    - 其次，分组也有可能不知道 where的 字段名

    **having 是对分组之后的结果进行筛选的。**

    ```sql
    select brand, avg(price) avgPrice, count(*), avg(score) from `products` 
    							group by brand having avgPrice > 2000
    ```

  - **SQL Join：**左连接、右连接、内连接、全连接

    ![image-20211108213109203](https://i.loli.net/2021/11/08/TWFLxsQhkm6G8vV.png)
    
    -  **左连接：**即是以左边的（join 关键字左侧的表）为主，简单的、没有筛选的左连接是左边的表所有数据都是可以显示的。
    
      ```sql
      # 全包含的左连接，即包含交集
      select * from `products` left join `brand` on products.brand_id = brand.id
      # 不包含交集
      select * from `products` left join `brand` on products.brand_id = brand.id where brand_id is null
      ```
    
      另外，left join 实际上 可以是 left outer join，不过 outer 可以省略
    
    - **右连接：**以右边的表为主。其他和左连接一样。
    
      ```sql
      # 全包含的右连接，即包含交集
      select * from `products` right join `brand` on products.brand_id = brand.id
      # 不包含交集
      select * from `products` right join `brand` on products.brand_id = brand.id where brand.id is null
      ```
    
    - **内连接：**<font color=FF0000>**join**</font> / inner join / cross join
    
      ```sql
      select * from `products` join `brand` on products.brand_id = brand.id
      ```
    
    - **全连接：**mysql 中没有直接实现全连接，可以使用联合 (union) 来实现全连接
    
      ```sql
      # 这个代码是错误的
      select * from `products` full join `brand` on products.brand_id = brand.id
      
      # 联合，完整的并集
      (select * from `products` left join `brand` on products.brand_id = brand.id) 
      union
      (select * from `products` right join `brand` on products.brand_id = brand.id)
      ```
    
      

- **DCL** ( Data Control Language )：数据控制语言。对数据库、表格的权限进行相关访问控制操作



**MySQL支持的数据类型**

MySQL支持的数据类型有：<font color=FF0000>**数字类型、日期、时间类型、字符串（字符和字节）类型、空间类型 和 JSON数据类型**</font>。

- **数字类型：**

  -  **整数数字类型：**INTEGER、INT、SMALLINT、TINYINT、MEDIUMINT、BIGINT

    | Type      | Storage (Bytes) | Minimum Value Signed | Minimum Value Unsigned | Maximum Value Signed | Maximum Value Unsigned |
    | :-------- | :-------------- | :------------------- | :--------------------- | :------------------- | :--------------------- |
    | TINYINT   | 1               | -128                 | 0                      | 127                  | 255                    |
    | SMALLINT  | 2               | -32768               | 0                      | 32767                | 65535                  |
    | MEDIUMINT | 3               | -8388608             | 0                      | 8388607              | 16777215               |
    | INT       | 4               | -2147483648          | 0                      | 2147483647           | 4294967295             |
    | BIGINT    | 8               | -2^63^               | 0                      | 2^63^-1              | 2^64^-1                |

  - **浮点数字类型：** FLOAT、DOUBLE（FLOAT是4个字节, DOUBLE是8个字节）
  - **精确数字类型：**DECIMAL、NUMERIC （ DECIMAL 是 NUMERIC的实现形式 ），以DECIMAL 为例，高度为两位小数，则为：DECIMAL(10, 2)：有10位，有2个小数点

- **日期类型：**

  - **YEAR：**以 "YYYY" 格式显示值，范围1901 到 2155，和0000
  - **DATE：**用于具有日期部分但没有时间部分的值：
    - DATE以格式 "YYY-MM-DD" 显示值
    - 支持的范围是"1000-01-01" 到 "9999-12-31"
  - **DATETIME：**用于包含日期和时间部分的值
    - DATETIME以格式YYYY-MM-DD hh.mm:ss显示值;
    - 支持的范围是1000-01-01 00:00:00到9999-12-31 23:59:59
  - **TIMESTAMP：**被用于同时包含日期和时间部分的值：
    - TIMESTAMP以格式 "YYYY-MM-DD hh:mm:ss" 显示值
    - 但是它的范围是UTC的时间范围："1970-01-01 00:00:01" 到 "2038-01-19 03:14:07"
    - <font color=FF0000>在 DATETIME 和 TIMESTAMP 都可以保存目标的时间时，优先选择 TIMESTAMP</font>
  - 另外：DATETM或TIMESTAMP值可以包括在高达微秒（6位）精度的后小数秒一部分
    - 比如 DATETIME 表示的范围可以是 "1000-01-01 00:0:0.000000" 到 "9999-12-31 23:59:59.999999"

- **字符串类型：**

  - CHAR类型：在创建表时为<font color=FF0000>固定长度</font>，<font color=FF0000>长度可以是0到255之间的任何值</font>。在被查询时，会删除后面的空格
  - VARCHAR类型：是<font color=FF0000>可变长度的字符串</font>，<font color=FF0000>长度可以指定为0到65535之间的值</font>。在被查询时，不会删除后面的空格
  - BINARY 和 VARBINARY类型：用于存储二进制字符串,存储的是字节字符串
  - BLOB：用于存储大的二进制类型
  - TEXT：存储大的字符串类型



**主键 Primary key**

一张表中，我们为了区分每一条记录的唯一性，必须有一个字段是永远不会重复，并且不会为空的，这个字段我们通常会
将它设置为主键：

- <font color=FF0000>主键是表中唯一的索引</font>
- 并且必须是NOT NULL的，如果没有设置NOT NULL，那么MySQL也会隐式的设置为NOT NULL
- 主键也可以是多列索引，PRIMARY KEY ( key-part , ...)，我们一般称之为联合主键
- **建议：**<font color=FF0000>开发中主键字段应该是和业务无关的，尽量不要使用业务字段来作为主键</font>

**唯一：UNIQUE**

- 某些字段在开发中我们希望是唯一的，不会重复的；比如手机号码、身份证号码等，这个字段我们可以使用UNIQUE来约束
- 使用UNIQUE约束的字段在表中必须是不同的
- 对于所有引擎，<font color=FF0000>UNIQUE索引 允许NULL包含的列具有多个值NULL</font>

**不能为空：NOT NULL**

- 某些字段我们要求用户必须插入值，不可以为空，这个时候我们可以使用NOT NULL来约束

**默认值：DEFAULT**

- 某些字段我们希望在没有设置值时给予一个默认值，这个时候我们可以使用DEFAULT来完成 

**自增：AUTOINCREMENT**

- 某些字段我们希望不设置值时可以进行递增，比如用户的id，这个时候可以使用AUTOINCREMENT来完成



**将联合查询的数据转为对象**

- 将联合查询的数据转成对象（一对多的情况）：

  可以使用 json_object() 方法：

  ```mysql
  select products.id id, products.title title, products.price price,
  			 json_object('id', brand.id, 'name', brand.id, 'website', brand.website) brand
  from 'products'
  left join `brand` on products.brand_id = brand.id
  ```

- 将查询到的多条数据，组织成对象，放到数组中（多对多的情况）：

   [Node学习笔记.md](Node学习笔记.md) 这里要使用分组，另外，除了上面的 json_object() 方法，还需要 json_arrayagg() 方法

  ```mysql
  select
  			stu.id stu.name stu.age
  			json_arrayagg(json_object('id', cs.id, 'name', cs.name, 'price', cs.price))
  from `student` stu
  join `students_select_courses` ssc on stu.id = ssc.student_id
  join `courses` cs on ssc.course_id = cs.id
  group by stu.id
  ```



**mysql2： 类似于mysql驱动**

真实开发中肯定是通过代码来完成所有的操作的。如何可以在Node的代码中执行SQL语句的，这里我们可以借助于两个库：

- mysql：最早的Node连接MySQL的数据库驱动
- mysql2：在mysql的基础之上,进行了很多的优化、改进

目前相对来说，更偏向于使用mysql2，mysql2 兼容 mysql 的 API，并且提供了一些附加功能

- **更快 / 更好的性能**

- **Prepared Statement（预处理语句，编译语句）**

  - **提高性能：**将创建的语句模块发送给MySQL，然后MySQL编译（解析、优化、转换）语句模块，并且存储它但是不执行，之后我们在真正执行时会给提供实际的参数才会执行；就算多次执行，也只会编译一次，所以性能是更高的

    **补充：**如果再次执行该语句，它将会从LRU ( Least Recently Used ) Cache 中获取获取,省略了编译 statement 的时间来提高性能。

  - **防止SQL注入：**之后传入的值不会像模块引擎那样就编译，那么一些SQL注入的内容不会被执行； `or 1 = 1` 不会被执行 

- **支持Promise，所以我们可以使用 async 和 await 语法**

- 等等...

**mysql2 简单代码示例：**

```js
const mysql = require('mysql2')

// 创建数据库连接
const connection = mysql.createConnection({
  host: 'localhost',
  port: '3306', // 另外，默认就是3306
  database: 'myDbName',
  user: 'user'
  password: 'password'
})

// 执行sql 语句
const statement = `
	select * from products
`
connection.query(statement, (err, results, fields) => {
  console.log(results)
  // 终止连接，如果在终止时发生错误，是可以通过connection.on('error', () => {}) 监听到的。
  // 另外，也可以使用 connection.destory() 销毁连接。不过一般不会主动销毁连接，而是通过连接池来实现。
  // connection.end(); 
})
```

**mysql2预处理语句：**

```js
const mysql = require('mysql2')
const connection = mysql.createConnection({...})
                                           
const statement = `select * from products where > ? and score > ?`;

// 数组是传递上面问号的内容
connection.execute(statement, [6000, 7], (err, results) => {
  console.log(results)
})
```



**连接池**

前面我们是创建了一个连接 ( connection )，但是如果我们有多个请求的话，该连接很有可能正在被占用，那么我们是否需要每次一个请求都去创建一个新的连接呢？

- **事实上，mysql2给我们提供了连接池(connection pools )**
- 连接池可以在需要的时候自动创建连接，并且创建的连接不会被销毁，会放到连接池中，后续可以继续使用
- 我们可以在创建连接池的时候设置LMIT，也就是最大创建个数

**连接池的使用**

```js
const mysql = require('mysql2')

// 创建连接池
const connections = mysql.createPool({
  host: 'localhost',
  port: '3306',
  database: 'myDb',
  user: 'user',
  password: 'password',
  connectionLimit: 10
})

const statement = `select * from products where > ? and score > ?`;
// 使用了连接池，不过这样的写法是不太好的，因为回调函数会带来回调地狱
connections.execute(statement, [6000, 7], (err, results) => {
  console.log(results)
})
// 还是推荐使用 promise；正好，connections有promise 方法
connections.promise().execute(statement, [6000, 7]).then(([results]) => {
  console.log(results)
}).catch(err => {
  console.log(err)
})
```



**ORM**

**对象关系映射**（**Object Relational Mapping**，简称**ORM**，或**O/RM**，或**O/R mapping**），是一种程序 设计的方案：从效果上来讲，它提供了一个可在编程语言中，使用 虚拟对象数据库 的效果;

Node 中最常用的 ORM工具是：[Sequelize](https://github.com/sequelize/sequelize)

- Sequelize是用于Postgres，MySQL，MariaDB，SQLite和Microsoft SQL Server的基于Node.js 的 ORM；它支持非常多的功能

- 如果希望将Sequelize和MySQL一起使用，那么我们需要先安装两个东西：
  - mysql2：sequelize在操作mysql时使用的是mysql2
  - sequelize：使用它来让对象映射到表中

简单使用示例：

```js
const { Sequelize } = require('sequelize')

const sequelize = new Sequelize('dbName', 'userName', 'password', {
  host: 'localhost',
  dialect: 'mysql'
})

sequelize.authenticate().then(() => {
  console.log('连接数据库成功')
}).catch(err => {
  console.log('连接数据库失败', err)
})
```

**Sequelize 查询一张表**（一对一操作）

```js
// 这里的Op 表示 Option
const { Sequelize, Model, DataTypes, Op } = require('sequelize')
const sequelize = new Sequelize('dbName', 'userName', 'password', { host: 'localhost', dialect: 'mysql'})

class Product extends Model {}
Product.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  title: {
    type: DataTypes.STRING,
    allowNotNull: false
  },
  price: DataTypes.DOUBLE,
  score: DataTypes.DOUBLE,
  // ....
}, {
  tableName: 'products' // 将对象绑定表
  // 为了设计规范，Sequelize 在sql插叙结果上会默认加上 createdAt 和 updateAt 字段，这里将这两个设为false，让其不再显示。
  createdAt: false, 
  updatedAt: false,
  sequelize
})

async function queryProducts() {
  // 查询数据：select * from products where price >= 5000
  const queryResult = await Product.finAll({
    where: {
      price: {
        [op.gte]: 5000
      }
    }
  })
  
  // 插入数据：
	const insertResult = await Product.create({
    title: '三星',
    price: 8888,
    score: 5.5
  })
  
  // 更新数据
  const updateResult = await Product.update({
    price: 3688
  }, {
    where: {
      id: 1
    }
  })
}

queryProducts()
```

**一对多操作：**

```js
// Brand 表
class Brand extends Model {}
Brand.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  name: {
    type: DataTypes.STRING,
    allowNotNull: false
  },
  website: DataTyes.STRING,
  phoneRank: DataTypes.INTEGER
}, {
  tableName: 'brand',
  createdAt: false,
  updatedAt: false,
  sequelize
})

// Product 表
class Product extends Model {}
Product.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  title: {
    type: DataTypes.STRING,
    allowNotNull: false
  },
  price: DataTypes.DOUBLE,
  score: DataTypes.DOUBLE,
	brandId: {
    field: 'brand_id',
    type: DataTypes.INTEGER,
		// 外键约束
    references: {
      model: Brand,
      key: 'id'
    }
  }
}, {
  tableName: 'products' // 将对象绑定表
  // 为了设计规范，Sequelize 在sql插叙结果上会默认加上 createdAt 和 updatedAt 字段，这里将这两个设为false，让其不再显示。
  createdAt: false, 
  updatedAt: false,
  sequelize
})

// 将两张表联系在一起
Product.belongsTo(Brand, {
  foreignKey: 'brandId'
})

async function queryProducts() {
  const result = Product.findAll({
    // 进行联合查询
    include: {
      model: Brand
    }
  })
}
```

多对多操作：

```js
// Student
class Student extends Model {}
Student.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  name: {
    type: DataTypes.STRING,
    allowNotNull: false
  },
  age: DataTypes.INTEGER,
}, {
  tableName: 'student',
  createdAt: false,
  updatedAt: false,
  sequelize
})
// 课程
class Course extends Model {}
Course.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  name: {
    type: DataTypes.STRING,
    allowNotNull: false
  },
  price: DataTypes.DOUBLE
}, {
  tableName: 'course',
  createdAt: false,
  updatedAt: false,
  sequelize
})
// 学生课程表
class StudentCourse extends Model {}
StudentCourse.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  studentId: {
    type: DataTypes.INTEGER,
    references: {
      model: Student,
 	    key: 'id'
    },
    field: 'student_id'
  },
  courseId: {
    type: DataTypes.INTEGER,
    references: {
      model: Course,
      key: 'id'
    },
    field: 'course_id'
  }
}, {
  tableName: 'studentCourse',
  createdAt: false,
  updatedAt: false,
  sequelize
})
// 创建表之间的关系
Student.belongsToMany(Course, {
  through: StudentCourse,
  foreignKey: 'student_id',
  otherKey: 'course_id'
})
Course.belongsToMany(Student, {
  through: StudentCourse,
  foreignKey: 'course_id',
  otherKey: 'student_id'
})

async function queryProducts() {
  const result = await Student.findAll({
    include: {
      model: course
    }
  })
}
```



## Node 补充



##### Node 可以做什么

![596181630397925_.pic_hd](https://i.loli.net/2021/09/10/9Ig4SoB6b3zxnDT.png)



#### 《一杯茶的时间，上手 Node.js》笔记

Node（或者说 Node.js，两者是等价的）是 JavaScript 的一种**运行环境**。同时：**浏览器也是 JavaScript 的运行环境**，两个运行环境的区别：

<img src="https://i.loli.net/2020/12/21/Ppjc5Vu4TbaFHCS.jpg" style="zoom: 25%;" />

运行 Node 代码通常有两种方式：1）在 REPL 中交互式输入和运行；2）将代码写入 JS 文件，并用 Node 执行。

> **补充**
> REPL 的全称是 Read Eval Print Loop（读取-执行-输出-循环），通常可以理解为**交互式解释器**，你可以输入任何表达式或语句，然后就会立刻执行并返回结果。<font color=lightSeaGreen>如果你用过 Python 的 REPL 一定会觉得很熟悉。</font>

类似于Python，对于js文件也可通过如下命令使其在终端执行：

```sh
$ node script.js
```

在浏览器中，我们有 document 和 window 等全局对象；而<font color=FF0000> Node 只包含 ECMAScript 和 V8，不包含 BOM 和 DOM，因此 Node 中不存在 document 和 window；取而代之，Node 专属的全局对象是 process</font>。

**JavaScript 全局对象的分类**

1. 浏览器专属，例如 `window`、`alert` 等等；
2. <font color=FF0000>Node 专属，例如 `process`、`Buffer`、`__dirname`、`__filename` 等等；</font>
3. <font color=FF0000>浏览器和 Node 共有，但是**实现方式不同**</font>，例如 `console`（第一节中已提到）、`setTimeout`、`setInterval` 等；
4. <font color=FF0000>浏览器和 Node 共有，并且属于 **ECMAScript 语言定义**的一部分</font>，例如 `Date`、`String`、`Promise` 等；

<img src="https://i.loli.net/2020/12/21/cWpCzIl3Nao6RAG.jpg" style="zoom: 30%;" />

##### Node 专属全局对象解析

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

摘自：[一杯茶的时间，上手 Node.js](https://zhuanlan.zhihu.com/p/97413574)



#### Node 查看 V8 信息

> 👀 注：之所以想要了解 查看 Node 查看 V8 版本的方法，除了好奇之外，也是看见了 antfu 给 vscode 提的一个 issue 的截图（如下图），在提 issue 时很有必要给出自己运行环境的信息，这是一个 “减少不必要沟通” 的好习惯：
>
> <img src="https://s2.loli.net/2022/09/20/32JyPTvCenM6OSW.png" alt="image-20220920003722224" style="zoom: 33%;" />
>
> 链接：https://github.com/microsoft/vscode/issues/95937

##### 查看 V8 版本

###### 方法一

使用 `node -p process.versions.v8` ，结果如下：

<img src="https://s2.loli.net/2022/09/19/JyaBrAxI36VeqZ8.png" alt="image-20220919234454543" style="zoom:60%;" />

> 👀 注：`-p` 完整写法是 `--print` ，作用“应该”是 print “script” ，参见 [Node Doc - cli # `-p` , `--print` "script"](https://nodejs.org/api/cli.html#-p---print-script)

###### 方法二

使用 `npm version` ，结果如下：

<img src="https://s2.loli.net/2022/09/19/uJnsrRBOd5FbxKo.png" alt="image-20220919234857310" style="zoom:50%;" />

> 👀 注：顺带看了下 `node -p process.versions` 的结果，和 `npm version` 的运行结果一样。

##### 查看 v8 提供的选项

```sh
$ node --v8-options
```

打印结果相当多，这里略。不过可以使用 grep 对结果进行筛选：

```sh
# 查询 v8 引擎中 harmony（ES6）功能
$ node --v8-options | grep -e '--harmony'
```

摘自：[楊傑文的資訊技術手札 - NodeJS - 查看 v8 引擎的版本編號及支援特性](http://chiehwenyang.logdown.com/posts/2016/10/28/1048713)

##### Chrome 查看 V8 版本

在 Chrome 地址栏 输入并访问 `chrome://version/`  ，会显示 chrome 的一些信息，其中也包含 V8 的信息：

<img src="https://s2.loli.net/2022/09/20/D5aQCo9fbE1PFck.png" alt="image-20220920003308551" style="zoom:50%;" />





## npm

**NPM**（node package manager），通常称为node包管理器。顾名思义，它的主要功能就是管理node包，包括：安装、卸载、更新、查看、搜索、发布等。



#### 基本使用

##### 安装

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

##### 卸载

```sh
npm uninstall package-name
```

##### 查看已安装的包

```sh
npm ls      						#查看所有
npm ls package-name     #查看某安装包的具体信息
```

##### 更新

```sh
npm update package-name
```

##### 顺序

先到npm模块仓库查询最新版本（提供registry查询服务）==> 返回json对象(包含该模块所有版本信息；含有dist.tarball属性，属性值为该压缩包网址，访问下载源码) ==> 查询本地版本（若本地版本不存在或远程版本较新，则安装更新）
npm install和npm update都是以此方式安装模块

##### 搜索安装包

```sh
npm seach package-name
```

摘自：[npm和gem](https://blog.csdn.net/u011099640/article/details/53083845)



#### 命令解释

- `npm version` ：除了返回 npm 的版本外，还返回其他关于包的信息，比如：你正在使用的node.js的版本，openSSL或者V8的版本
- `npm list` ：显示当前npm所有的安装的包及其相关信息（比如：版本号）

- `npm install` 系列
  - **npm install** **<font color=FF0000>=</font>** **npm i**。在git clone项目的时候，项目文件中并没有 node_modules文件夹，项目的依赖文件可能很大。直接执行，<font color=FF0000>npm会根据package.json配置文件中的依赖配置下载安装</font>。
  
  - **-global** **<font color=FF0000>=</font>** **-g**，全局安装，安装后的包位于系统预设目录下
  - **--save** **<font color=FF0000>=</font>** **-S**，<font color=FF0000>安装的包将写入package.json里面的dependencies</font>，<mark>dependencies：生产环境需要依赖的库</mark>
  - **--save-dev** **<font color=FF0000>=</font>** **-D**，<font color=FF0000>安装的包将写入packege.json里面的devDependencies</font>，<mark>devdependencies：只有开发环境下需要依赖的库</mark>

另外：在package name后面添加@版本号，可以安装指定版本号的包

摘自：[npm install说明](https://www.jianshu.com/p/b3e407942ac5)

##### 补充

- `npm info packageName` ：查看包的信息，及其历史版本的信息等（在你想要安装特定版本的npm包，且不确定是否存在时，可以使用该命令）
- `npm config ls` / `npm config list` ：查看所有Node环境配置
- `npm config get cache` ：查看缓存路径
- `npm config set cache cache-path` ：设置缓存
- `npm config set prefix prefix-path`：设置路径



#### npm config 相关使用

```sh
npm config set <key> <value> [-g|--global]
npm config get <key>
npm config delete <key>
npm config list [-l] [--json]
npm config edit
npm get <key>
npm set <key> <value> [-g|--global]
```

##### Description

npm gets its config settings from the command line, environmentvariables, `npmrc` files, and in some cases, the `package.json` file.

See npmrc for more information about the npmrc files.

See config for a more thorough discussion of the mechanisms involved.

The `npm config` command can be used to update and edit the contents of the user and global npmrc files.

##### Sub-commands

Config supports the following sub-commands:

- **set：**Sets the config key to the value. If value is omitted, then it sets it to "true".

  ```sh
  npm config set key value
  ```

- **get：**Echo the config value to stdout.

  ```sh
  npm config get key
  ```

- **list：**Show all the config settings. Use `-l` to also show defaults. Use `--json` to show the settings in json format.

  ```sh
  npm config list
  ```

- **delete：**Deletes the key from all configuration files.

  ```sh
  npm config delete key
  ```

- **edit：**Opens the config file in an editor. Use the `--global` flag to edit the global config.

  ```sh
  npm config edit
  ```

摘自：[npm-config](https://docs.npmjs.com/cli/v6/commands/npm-config)



#### --save 系列 options

npm install takes 3 exclusive, optional flags which save or update the package version in your main package.json:

npm install 提供了3种独立的、可选的用于保存和更新在你主要的package.json的包版本的标记

- `-S` , `--save` : Package will appear in your dependencies.

  `-S` 是 `--save` 的缩写，package将会出现在你的dependencies中（自动把模块和版本号添加到dependencies部分（生产环境））

- `-D` , `--save-dev` : Package will appear in your devDependencies.

  `-D` 是 `--save-dev` 的缩写，package将会出现在你的devDependencies中（自动把模块和版本号添加到devDependencies部分（开发环境））

- `-O` , `--save-optional` : Package will appear in your optionalDependencies.

  `-O` 是 `--save-optinal` 的缩写，package将会出现在你的optionalDependencies中

摘自：[npm 安装参数中的 --save-dev 是什么意思](https://segmentfault.com/q/1010000000403629)



#### npm init

在现代新建一个 JS 相关的项目往往都是从 `package.json` 文件开始的，不过这个文件里需要的字段实在是太多了，正常人都记不住，所以 <font color=FF0000>npm 官方提供了 **`npm init`** 命令帮助我们快速初始化 **`package.json`** 文件</font>。执行之后会有一个交互式的命令行让你输入需要的字段值，当然如果你想直接使用默认值，也可以使用 **`npm init -y`** 来更加快速初始化的；其中：**`-y`** 表示所有的选项都是允许的 ( yes )。

随着技术的快速发展，发现初始化 package.json 已经无法满足大家的需求了，<font color=FF0000>越来越多的项目需要进行整个项目的初始化。脚手架工具应运而生</font>，除了有通用的脚手架工具 yeoman, sao 之外，<font color=FF0000>很多项目也会开发针对自己项目的脚手架工具，例如 vue-cli, create-react-app 以及专门用来初始化 ThinkJS 项目的脚手架工具 think-cli</font>。

摘自：[你不知道的 npm init](https://zhuanlan.zhihu.com/p/45151808)

**npm init功能：**创建一个 package.json 文件 / 可用于设置新的或现有的 npm 软件包。

摘自：[npm init 全方位解读](https://juejin.im/post/6844903960831082503)



#### package-lock.json 相关

npm 创建了一个 package-lock.json，这个文件就是用来**锁定全部直接依赖和间接依赖的精确版本号**，或者说提供了关于 node_modules 目录的精确描述，从而确保在这个项目中开发的所有人都能有完全一致的 npm 依赖。

摘自：[一杯茶的时间，上手 Node.js](https://zhuanlan.zhihu.com/p/97413574)



#### 查看全局安装 的位置

```sh
npm root -g
```

运行结果：

<img src="https://s1.ax1x.com/2020/10/28/B1tKUO.png" style="zoom:75%;" />



#### 镜像设置

##### 查看npm默认镜像地址

```sh
npm config get registry
```

##### 将npm默认镜像地址修改为淘宝

```sh
npm config set registry https://registry.npm.taobao.org
```

##### 使用官方镜像

```sh
npm config set registry https://registry.npmjs.org
```

##### 通过cnpm

```sh
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

##### 临时使用淘宝镜像

```sh
npm --registry https://registry.npm.taobao.org install express
```

摘自：[npm换源](https://www.jianshu.com/p/f311a3a155ff)



#### 发布 npm 包

- **编写模块**

- **初始化包描述信息**
  
  `package.json` 文件的内容尽管相对较多，但是实际发布一个包时并不需要一行一行编写。NPM提供的 `npm init` 命令会帮助你生成`package.json` 文件，NPM 通过提问式的交互逐个填入选项，最后生成预览的包描述文件。如果你满意，输入 yes，此时会在目录下得到 `package.json` 文件。
  
- **注册包仓库账号**
  
  为了维护包，NPM 必须要使用仓库账号才允许将包发布到仓库中。<font color=FF0000>**注册账号的命令是 `npm adduser`**</font> 。这也是一个提问式的交互过程，按顺序进行即可：
  
  ```bash
  $ npm adduser
  Username: (jacksontian)
  Email: (shyvo1987@gmail.com)
  ```
  
- **上传包**
  
  <font color=FF0000>**上传包的命令是 `npm publish <folder>`**</font>。在刚刚创建的 package.json 文件所在的目录下，执行 `npm publish .` 开始上传包，相关代码如下：
  
  ```bash
  $ npm publish .
  npm http PUT http://registry.npmjs.org/hello_test_jackson
  npm http 201 http://registry.npmjs.org/hello_test_jackson
  npm http GET http://registry.npmjs.org/hello_test_jackson
  npm http 200 http://registry.npmjs.org/hello_test_jackson
  npm http PUT http://registry.npmjs.org/hello_test_jackson/0.0.1/-tag/latest
  npm http 201 http://registry.npmjs.org/hello_test_jackson/0.0.1/-tag/latest
  npm http GET http://registry.npmjs.org/hello_test_jackson
  npm http 200 http://registry.npmjs.org/hello_test_jackson
  npm http PUT http://registry.npmjs.org/hello_test_jackson/-/hello_test_jackson-0.0.1.tgz/-rev/2-2d64e0946b866878bb252f182070c1d5
  npm http 201 http://registry.npmjs.org/hello_test_jackson/-/hello_test_jackson-0.0.1.tgz/-rev/2-2d64e0946b866878bb252f182070c1d5
  + hello_test_jackson@0.0.1
  ```
  
  在这个过程中，NPM会将目录打包为一个存档文件，然后上传到官方源仓库中
  
- **安装包** ：`npm install`

- **管理包权限**

  通常，一个包只有一个人拥有权限进行发布。如果需要多人进行发布，可以使用 `npm owner` 命令帮助你管理包的所有者：

  ```bash
  $ npm owner ls eventproxy
  npm http GET https://registry.npmjs.org/eventproxy
  npm http 200 https://registry.npmjs.org/eventproxy
  jacksontian <shyvo1987@gmail.com>
  ```

  使用这个命令，也可以添加包的拥有者，删除一个包的拥有者：

  ```bash
  npm owner ls <package name>
  npm owner add <user> <package name>
  npm owner rm <user> <package name>
  ```

**分析包**

在使用 NPM 的过程中，或许你不能确认当前目录下能否通过 require() 顺利引入想要的包，<font color=FF0000>这时可以执行 npm ls 分析包</font>。

这个命令可以为你分析出当前路径下能够通过模块路径找到的所有包，并生成依赖树，如下：

```bash
$ npm ls
/Users/jacksontian
├─┬ connect@2.0.3
│ ├── crc@0.1.0
│ ├── debug@0.6.0
│ ├── formidable@1.0.9
│ ├── mime@1.2.4
│ └── qs@0.4.2
├── hello_test_jackson@0.0.1
└── urllib@0.2.3
```

摘自：《深入浅出Node.js》P40



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

<font color=FF0000> 命令行下使用 npm run 命令，就可以执行这段脚本</font>。

```sh
npm run build
# 等同于执行
node build.js
```

<mark style="background:aqua">这些定义在 package.json 里面的脚本，就称为 npm 脚本。它的优点很多</mark>。

- <mark>项目的相关脚本，可以集中在一个地方</mark>。
- 不同项目的脚本命令，只要功能相同，就可以有同样的对外接口。用户不需要知道怎么测试你的项目，只要运行 npm run test 即可。
- <mark>可以利用 npm 提供的很多辅助功能</mark>。

<font color=FF0000> 查看当前项目的所有 npm 脚本命令，可以使用不带任何参数的 npm run 命令</font>。

```bash
npm run
```

**原理：**

npm 脚本的原理非常简单。<font color=FF0000>**每当执行npm run，就会自动新建一个 Shell，在这个 Shell 里面执行指定的脚本命令**</font>。因此，<font color=FF0000> 只要是 Shell（一般是 Bash）可以运行的命令，就可以写在 npm 脚本里面</font>。

<mark>比较特别的是，npm run 新建的这个 Shell，会将当前目录的 node_modules/.bin 子目录加入 PATH 变量，执行结束后，再将 PATH 变量恢复原样</mark>。这意味着，<font color=FF0000> 当前目录的 node_modules/.bin 子目录里面的所有脚本，都可以直接用脚本名调用，而不必加上路径</font>。比如，当前项目的依赖里面有 Mocha，只要直接写 mocha test 就可以了。

 ```javascript
"test": "mocha test"
 ```

而不用写成下面这样。

```sh
"test": "./node_modules/.bin/mocha test"
```

<font color=FF0000>由于 npm 脚本的唯一要求就是可以在 Shell 执行，因此它不一定是 Node 脚本，任何可执行文件都可以写在里面。</font>

<font color=FF0000> npm 脚本的退出码，也遵守 Shell 脚本规则。如果退出码不是 **0**，npm 就认为这个脚本执行失败</font>。

**通配符**

由于 npm 脚本就是 Shell 脚本，因为可以使用 Shell 通配符。

```json
"lint": "jshint *.js"
"lint": "jshint **/*.js"
```

**传参**

向 npm 脚本传入参数，要使用 `--` 标明。

```json
"lint": "jshint **.js"
```

向上面的 npm run lint 命令传入参数，必须写成下面这样。

```bash
npm run lint --  --reporter checkstyle > checkstyle.xml
```

**执行顺序**

<font color=FF0000> **如果 npm 脚本里面需要执行多个任务，那么需要明确它们的执行顺序**</font>。

如果是 <font color=FF0000>**并行执行**（即同时的平行执行），可以使用 **`&`** 符号</font>。

```bash
npm run script1.js & npm run script2.js
```

如果是<font color=FF0000>**继发执行**（即只有前一个任务成功，才执行下一个任务），可以使用 **`&&`** 符号</font>。

```bash
npm run script1.js && npm run script2.js
```

这两个符号是 Bash 的功能。此外，还可以使用 node 的任务管理模块：[script-runner](https://github.com/paulpflug/script-runner)、[npm-run-all](https://github.com/mysticatea/npm-run-all)、[redrun](https://github.com/coderaiser/redrun)。

**默认值**

一般来说，npm 脚本由用户提供。但是，<font color=FF0000> npm 对两个脚本提供了**默认值**</font>。也就是说，<font color=FF0000> 这两个脚本不用定义，就可以直接使用</font>。

```json
"start": "node server.js"，
"install": "node-gyp rebuild"
```

上面代码中，<mark>npm run start 的默认值是 node server.js，前提是项目根目录下有 server.js 这个脚本；npm run install 的默认值是 node-gyp rebuild，前提是项目根目录下有 binding.gyp 文件</mark>。

**钩子**

<font color=FF0000>**npm 脚本有 pre 和 post 两个钩子**</font>（即：在脚本运行前执行和在脚本执行完成后执行）。<font color=FF0000> 举例来说，build 脚本命令的钩子就是 prebuild 和 postbuild </font>。

```json
"prebuild": "echo I run before the build script",
"build": "cross-env NODE_ENV=production webpack",
"postbuild": "echo I run after the build script"
```

<font color=FF0000>用户执行 **npm run build** 的时候，会自动按照下面的顺序执行。</font>

```bash
npm run prebuild && npm run build && npm run postbuild
```

**npm 默认提供下面这些钩子**

- pre<font color=0000FF>publish</font>，post<font color=0000FF>publish</font>
- pre<font color=0000FF>install</font>，post<font color=0000FF>install</font>
- pre<font color=0000FF>uninstall</font>，post<font color=0000FF>uninstall</font>
- pre<font color=0000FF>version</font>，post<font color=0000FF>version</font>
- pre<font color=0000FF>test</font>，post<font color=0000FF>test</font>
- pre<font color=0000FF>stop</font>，post<font color=0000FF>stop</font>
- pre<font color=0000FF>start</font>，post<font color=0000FF>start</font>
- pre<font color=0000FF>restart</font>，post<font color=0000FF>restart</font>

<font color=FF0000> <font size=4>**自定义的脚本命令也可以加上 pre 和 post 钩子**</font>。比如， myscript 这个脚本命令，也有 pre**myscript** 和 post**myscript** 钩子。不过，**双重的 pre 和 post 无效**，比如 prepretest 和 postposttest 是无效的</font>。

npm 提供一个 npm_lifecycle_event 变量，返回当前正在运行的脚本名称，比如 pretest 、test 、posttest 等等

**简写形式**

<mark style="background:aqua">四个常用的 npm 脚本有简写形式：</mark>

- npm start 是 npm run start 的简写
- npm stop 是 npm run stop 的简写
- npm test 是 npm run test 的简写
- npm restart 是 npm run stop && npm run restart && npm run start 的简写

npm start、npm stop 和 npm restart 都比较好理解，而 npm restart是一个复合命令，实际上会执行三个脚本命令：stop、restart、start。具体的执行顺序如下。

1. prerestart
2. prestop
3. stop
4. poststop
5. restart
6. prestart
7. startpoststart
8. postrestart

**变量**

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

那么，<font color=FF0000>变量 **`npm_package_name`** 返回 **foo**，变量 **npm_package_version** 返回 **1.2.5**</font>。

```js
// view.js
console.log(process.env.npm_package_name); // foo
console.log(process.env.npm_package_version); // 1.2.5
```

上面代码中，我们通过环境变量 process.env 对象，拿到 package.json 的字段值。如果是 Bash 脚本，可以用 \$npm_package_name和\$npm_package_version 取到这两个值。

`npm_package_` 前缀也支持嵌套的 package.json 字段。

```json
"repository": {
  "type": "git",
  "url": "xxx"
},
scripts: {
  "view": "echo $npm_package_repository_type"
}
```

上面代码中，repository 字段的 type 属性，可以通过 npm_package_repository_type 取到。

另外一个例子：

```json
"scripts": {
  "install": "foo.js"
}
```

上面代码中，npm_package_scripts_install 变量的值等于 foo.js。

npm 脚本还可以通过 npm_config_ 前缀，拿到 npm 的配置变量，即 npm config get xxx 命令返回的值。比如，当前模块的发行标签，可以通过npm_config_tag取到。

```js
"view": "echo $npm_config_tag",
```

注意，package.json 里面的 config 对象，可以被环境变量覆盖。

```json
{ 
  "name" : "foo",
  "config" : { "port" : "8080" },
  "scripts" : { "start" : "node server.js" }
}
```

上面代码中，npm_package_config_port 变量返回的是 8080。这个值可以用下面的方法覆盖。

```bash
$ npm config set foo:port 80
```

最后，env 命令可以列出所有环境变量。

```js
"env": "env"
```

**常用脚本示例**

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

npm 从 @5.2 开始，增加了 npx 命令。

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

<font color=FF0000 size=4>**npx 的原理很简单，就是运行的时候，会到node_modules/.bin路径和环境变量$PATH里面，检查命令是否存在**</font>。

由于 npx 会检查环境变量 \$PATH，所以系统命令也可以调用。

```bash
# 等同于 ls
$ npx ls
```

<font color=FF0000> **注意，Bash 内置的命令不在$PATH里面，所以不能用。比如，cd是 Bash 命令，因此就不能用 npx cd**</font>。

**补充：**

直到这里我还是不太理解npx的作用，不过在看了《再学JavaScript ES(6-11)全版本语法大全》的11-2后明白了：在没有全局安装工具时，是无法直接用工具命令去使用它的（比如如果没有全局安装webpack，无法在项目中直接使用 `webpack xxx`，而且由于在一台机器内开发多个项目，且webpack版本未必一致时，webpack不建议全局安装）；这时可以使用npx，输入命令 `npx 工具命令 xxx`（比如：`npx webpack xxx`）



#### node-gyp

##### GitHub README.md 介绍

node-gyp is a cross-platform command-line tool written in Node.js <font color=FF0000>for **compiling native addon**</font>（插件） <font color=red>modules for Node.js</font>. It contains a vendored copy of the gyp-next project that was previously used by the Chromium team, extended to support the development of Node.js native addons.

Note that node-gyp is not used to build Node.js itself.

##### 特性

- The same build commands work on any of the supported platforms
- Supports the targeting of different versions of Node.js

Github地址：https://github.com/nodejs/node-gyp

##### node-gyp 的作用 

> node下的 gyp 
>
> Google 使用过很多处理平台无关（即跨平台）的项目构建系统，比如Scons，CMake。<font color=FF0000> 在实际使用中这些并不能满足需求。开发复杂的应用程序时，在Mac上Xcode更加适合，而 Windows上 Visual Studio 更是无二之选</font>。👀 注：CMake 是 更高级（层级、抽象程度）的 make，同时也支持跨平台（ make 不支持跨平台）。
>
> <font color=FF0000>gyp 是为 Chromium 项目创建的项目生成工具，可以从平台无关的配置生成平台相关的 Visual Studio、Xcode、Makefile 的项目文件。这样一来我们就不需要花额外的时间处理每个平台不同的项目配置以及项目之间的依赖关系</font>。
>
> 摘自：[node-gyp的作用是什么? - FreezeSoul的回答 - 知乎](https://www.zhihu.com/question/36291768/answer/109978115)

> 编译 C++ 扩展用的。
> 长久以来 linux 的二进制分发一直是巨坑，<font color=FF0000> npm 为了方便干脆就直接源码分发，用户装的时候再现场编译</font>。不过对另一些人二进制分发就比源码分发简单多了，所以还有个 node-pre-gyp 来干二进制扩展的分发。
>
> 摘自：[node-gyp的作用是什么? - Belleve的回答 - 知乎](https://www.zhihu.com/question/36291768/answer/70682907)

> GYP工具，即 “Generate Your Projects” 短句的缩写。它的好处在于，可以帮助你生成各个平台下的项目文件，比如Windows下的VisualStudio 解决方案文件（.sln）、Mac下的XCode项目配置文件以及Scons工具。在这个基础上，再动用各自平台下的编译器编译项目。这大大减少了跨平台模块在项目组织上的精力投入。
>
> 摘自：《深入浅出Node.js》P28



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


摘自：[一杯茶的时间，上手 Express 框架开发](https://zhuanlan.zhihu.com/p/98019249)