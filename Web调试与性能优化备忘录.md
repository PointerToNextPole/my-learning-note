# Web 调试与性能优化笔记



##### 一些资料

- 关于简单的调试，可以看下 [使用sources进行断点调试【渡一教育】](https://www.bilibili.com/video/BV19N41147nS) 感觉相当易懂

- 关于 performance 选项卡的使用，可以看下 [如何发现前端性能瓶颈【渡一教育】](https://www.bilibili.com/video/BV1aP41147FA) ，感觉视频的教程形式，要比神光调试小册相关部分更易理解些。// TODO 看神光调试小册做笔记时，记得重看一遍，做好笔记。

  另外，还有 [network选项卡【渡一教育】](https://www.bilibili.com/video/BV1Dy4y1N734) ，虽然新学到的东西并不多，但是还是感觉 Initator 和 Timing 选项卡中的内容是之前完全没注意到的，而且有点意思；尤其是 Timing
  
  > 👀  25/07/20 [使用chrome调试工具解决问题【渡一教育】](https://www.bilibili.com/video/BV18tM7ziEYv) 感觉也不错，其中有些选项卡比如 Layout 、DOM Breakpoints 之前几乎没什么用过，也不清楚怎么使用，有必要重新看下
  
- [67 Weird Debugging Tricks Your Browser Doesn't Want You to Know](https://alan.norbauer.com/articles/browser-debugging-tricks)
- [面试官：一个接口使用postman这些测试很快，但是页面加载很慢怎么回事 😤😤😤](https://juejin.cn/post/7539817416609382427)



## 调试方法与技巧

#### VS Code 调试单个 TS 文件

##### 预先准备

- 配置好 ts、node、ts-node 等环境

- 安装好 vsc 的 [TypeScript Debugger](https://marketplace.visualstudio.com/items?itemName=kakumei.ts-debug) 调试插件

  <img src="https://s2.loli.net/2024/12/04/76qluPm8wQi3e5U.png" alt="image-20241204235107085" style="zoom:40%;" />

##### 创建 `launch.json`

<img src="https://s2.loli.net/2025/01/09/zfi8JjSt6bOHnUW.png" alt="image-20241204235316143" style="zoom:50%;" />

在 “Run and Debug” 面板下，点击 “create a launch.json file”，并选择 debugger 中的 “TS Debug”

<img src="https://s2.loli.net/2024/12/04/OXUxNikFd32VnIr.png" alt="image-20241204235457289" style="zoom:50%;" />

##### 修改 `launch.json`

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "ts-node",
      "type": "node",
      "request": "launch",
      "args": ["${relativeFile}"],
      "runtimeArgs": ["-r", "/usr/local/lib/node_modules/ts-node/register"],
      "cwd": "${workspaceRoot}",
      "protocol": "inspector",
      "internalConsoleOptions": "openOnSessionStart",
      "program": "${workspaceFolder}/debugTarget.ts"
    }
  ]
}
```

其中值得注意的是：`runtimeArgs` 中的 `ts-node/register` 路径是绝对路径，并且随着电脑的不同而不同。可以考虑通过 `which ts-node` 找到找到 “alias” （软链接？），并通过查看文件信息找到源文件地址，再定位到对应 `ts-node` 文件夹下的 `register` 文件夹

<img src="https://s2.loli.net/2024/12/05/odpQUCwfmGrPRtv.png" alt="image-20241205001811653" style="zoom:50%;" />

##### 打断点与开始 debug

<img src="https://s2.loli.net/2024/12/04/vOmkZBxDYhrltjS.png" alt="image-20241204235946478" style="zoom:50%;" />

学习自：[Mac上vscode调试typescript(单文件)](https://blog.csdn.net/qq_43478653/article/details/138563080)

##### VS Code 调试单个 `.mts` 文件

在一个 `lc` 文件夹中统一存放和编写 leetcode 题目时，发现不同 ts 文件会共用一个上下文，当不同的 `.ts` 文件中出现同名变量时，将会报错。这时候不得不使用 `.mts` 扩展名，不过，再 debug 会发现无法解析，无法理解类型标注了，会对哪怕最简单的类型标注报错。

搜了下解决方案，找到了 [Debugging mts file in VSCode (Typescript 4.5)](https://stackoverflow.com/questions/70661161/debugging-mts-file-in-vscode-typescript-4-5) ，按照其说法：在 `.vscode/launch.json` 的 `configuration` 下加上 `"pauseForSourceMap": true` 即可正常 debug `.mts` 文件；但是，加上之后，还是不行。于是按照问题的描述，借鉴问题提出者的配置，做了配置；在 `tsconfig.json` 的基础上，加上了 `tsconfig.debug.json` ，结果可以了，代码如下：

```json
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "noEmit": false,
    "sourceMap": true,
    "outDir": "lib"
  }
}
```

> 💡**补充**
>
> ###### `.mts` 和 `.module.ts` 之间的关系
>
> <img src="https://s2.loli.net/2024/12/05/sZemj2IVCAFgPOo.png" alt="image-20241205163217638" style="zoom: 50%;" />



#### 《一些你可能不知道的奇葩调试技巧》笔记

##### 条件断点

条件断点是一种高级的调试技巧，它允许我们为某个特定的代码行设置断点，但这个断点只有在满足某个特定条件时才会触发。

我们可以在想要调试的地方右键，选择 `Add conditional breakpoint`

<img src="https://s2.loli.net/2025/06/01/tsbgK4LxmYZ1lH5.png" alt="图片" style="zoom: 50%;" />

然后在条件中输入断点的生效条件，例如我们可以让它在这个位置只打印日志不进行暂停：

<img src="https://s2.loli.net/2025/06/01/G4t165kBpjxf8Rd.png" alt="图片" style="zoom:60%;" />

<font color=dodgerBlue>还有一些你可能会使用到的调试条件：</font>

仅在使用 2 个参数调用当前函数时断点：`arguments.callee.length === 2` ，如果这个函数有多个可选的重载的时候会很有用。

<font color=red>页面加载后 7 秒才断点：`performance.now() > 7000`</font>，当你想要设置断点，但只想在初始页面加载后断点执行时会很有用。

> 👀 这点没想到，不过想了下：Sources 面板中监听的变量是响应式的，所以虽然出人意料，但是也是细想之下合理的。

同理，我们还可以实现更精准一点的时间控制：如果在接下来的 7 秒内命中断点，则不要断点执行，而是在之后随时断点：

```js
window.baseline = window.baseline || Date.now(), (Date.now() - window.baseline) > 7000
```

>  👀 这个表达式还是挺风骚的：用到了自定义全局变量、赋值返回、或运算短路，逗号运算符 ...

也可以根据计算的 `CSS` 值断点，例如，仅当文档正文具有红色背景色时才暂停执行：

```js
window.getComputedStyle(document.body).backgroundColor === "rgb(255, 0, 0)"
```

> 👀 试了下，这里的代码还是有点问题的：虽然 `getComputedStyle` 返回的样式 `backgroundColor` 确实是使用 `rgb()` 函数的，但是本质上还是字符串，甚至按照原文的 `rgb(255,0,0)` 都不能使其相等并进入断点，必须要使用空格，更不用说如果使用 `red` 做比较

我们还可以用它来追踪函数的调用堆栈：比如说，你有一个函数用于显示加载动画，还有一个函数用于隐藏它，但在你的代码中的某个地方，你调用了展示动画的方法，但没有相应的隐藏动画的调用。你要如何找到这个没有配对的展示动画方法的调用源头呢？你可以在展示动画方法的条件断点中使用 `console.trace` 来运行代码，找到对应展示动画方法的最后一个栈追踪，点击调用源就可以跳转到对应的代码位置：

<img src="https://s2.loli.net/2025/06/01/PJOmkLrVbcCoKgx.gif" style="zoom:70%;" />

甚至可以利用条件断点来帮助我们对函数进行性能分析，我们只需要在函数前后插入：`console.time` 和 `console.timeend`：

<img src="https://s2.loli.net/2025/06/01/dMA9Ba3ER5HLzQO.gif" alt="图片" style="zoom:70%;" />

##### 记录 DOM 的快照

> 👀 个人感觉：除了使用 `copy` 这个 Console API 的 函数 外，“记录快照” 这个功能没什么作用？

获取当前状态下 DOM 的快照：

```js
copy(document.documentElement.outerHTML);
```

每秒记录一次 DOM 快照，并打印到控制台：

```js
doms = [];
setInterval(() => {
  const domStr = document.documentElement.outerHTML;
  console.log("snapshotting DOM: ", domStr);
  doms.push(domStr);
}, 1000);
```

##### 监控网页中获得焦点的元素

```js
(function () {
  let last = document.activeElement;
  setInterval(() => {
    if (document.activeElement !== last) {
      last = document.activeElement;
      console.log("Focus changed to: ", last);
    }
  }, 1700);
})();
```

<img src="https://s2.loli.net/2025/06/02/RyCrdOv9BF2lzoI.gif" alt="图片" style="zoom:65%;" />

##### 找到所有加粗的元素

```js
const isBold = (e) => {
  let w = window.getComputedStyle(e).fontWeight;
  return w === "bold" || w === "700";
};
Array.from(document.querySelectorAll("*")).filter(isBold);
```

##### 调试当前选择的元素

`$0` 控制台中的内容是对元素检查器中当前选定元素的自动引用。

例如 ，我们可以检查当前所选元素的事件侦听器：`getEventListeners($0)`：

<img src="https://s2.loli.net/2025/06/02/r4WFy7ZkQPub1K3.png" style="zoom:70%;" />

> 👀 这个函数也是 Console API 的

调试所选元素的所有事件：`monitorEvents($0)`

调试所选元素的特定事件：`monitorEvents($0, ["control", "key"])`

##### 调用并调试函数

> ⚠️ 这个用法感觉很不错，也是之前几乎没听过的

在想要查找问题并进行详细调试时，一个简单的技巧就是先调用一下 `debugger` 命令。例如，假设我们有以下形式的函数：

```js
function fn() {
  /* 某些代码 */
}
```

<font color=red>**可以在自己的控制台里这样操作：**</font>

```js
debugger; fn(1);
```

<font color=red>然后点击 `Step into next function call`，就能对 `fn` 函数的具体实现进行调试了。</font>

<font color=lightSeaGreen>**这个技巧在你不想找到函数 `fn` 的详细定义并手动设置断点，或者当这个 `fn` 函数是动态绑定到某个函数上，你又不清楚具体源头在哪里时，尤其好用**</font>。

在 `Chrome` 浏览器里，<font color=red>甚至可以在命令行里直接使用 `debug(fn)` 命令</font>，这样每次运行 `fn` 函数时，调试器都会暂停在这个函数的执行过程中，方便你查看和排查问题。

> 👀 `debug` 函数，也是 Console API，另外，感觉有点像是 watch函数的意味了。另外，可以发现的是这个用法和 `monitor` 函数的功能类似（见 [[#使用 `monitor()` 函数]]），不过，`debug` 用来 <font color=red>**监听**</font> 函数的执行，`monitor` 用来 <font color=red>**记录**</font> 变量的变化（因为<font color=red>它并不会产生断点</font>）

##### 在 URL 更改时暂停执行

> 👀 虽然也可以使用路由守卫实现？不过，这个思路还是很有启发性质

想要在单页应用改变 `URL`（比如发生路由跳转）之前暂停执行，你可以使用以下代码：

```js
const dbg = () => {
  debugger;
};
history.pushState = dbg;
history.replaceState = dbg;
window.onhashchange = dbg;
window.onpopstate = dbg;
```

但是<font color=red>这个方法不能处理当代码直接调用 `window.location.replace/assign` 的情况</font>，因为页面会在赋值后立即卸载，所以没有什么可以调试的。<font color=dodgerBlue>如果你仍然想要看到这些重定向的来源</font>（并在重定向时调试你的状态），<font color=dodgerBlue>在 `Chrome` 中，你可以这样调试相关的方法：</font>

```js
debug(window.location.replace);
debug(window.location.assign);
```

##### 调试属性读取

如果你有一个对象，想知道它的属性什么时候会被读取，可以在对象的 `getter` 中调用 `debugger`。例如，将 `{ configOption : true }` 转换为

```js
{ 
  get configOption() { 
    debugger; return true; 
  }
}
```

当你将一些配置选项传递给某个地方，并且想要看到它们如何被使用时，这个技巧非常有用。

##### 使用 `copy()` 函数

`Chrome` 和 `Firefox` 浏览器都支持使用 `console API` 的 `copy()` 函数，<font color=red>可以**直接将浏览器中的有趣信息复制到你的剪贴板，且不会有任何字符串截断**</font>，<font color=dodgerBlue>下面是一些你可能想要复制的有趣信息</font>：

- 当前 `DOM` 的快照： `copy(document.documentElement.outerHTML)`
- 资源的元数据（例如：图像）： `copy(performance.getEntriesByType("resource"))`
- 大型格式化的 `JSON` 块： `copy(JSON.parse(blob))`
- 你的 `localStorage` 的数据转储： `copy(localStorage)`
- 等等

这个技巧可以在你需要将一些数据信息复制到剪贴板，以便你在其他地方使用或者进行分析的时候使用。

##### 使用 `monitor()` 函数

你可以使用 `Chrome` 的 `monitor` 命令行方法来轻松追踪所有对类方法的调用。比如，给定一个 `People`类：

```js
class People {
  eat(count) {
    /* ... */
  }
}
```

如果我们想要知道所有对 `People` 类所有实例的调用，将以下代码粘贴到命令行：

```js
var p = People.prototype;
Object.getOwnPropertyNames(p).forEach((k) => monitor(p[k]));
```

然后你将在控制台获得输出：`function eat called with arguments: 2`

<font color=red>如果你希望在任何方法调用时暂停执行，而不仅仅是打印到控制台，可以使用 `debug` 而不是 `monitor`</font>。

<font color=dodgerBlue>如果你不知道类名，但你有一个实例，可以这样操作：</font>

```js
var p = instance.constructor.prototype;
Object.getOwnPropertyNames(p).forEach((k) => monitor(p[k]));
```

##### 绕过反调试

有时打开网页的 `Devtools` 你会发现可能会一直循环进入到一个 `debugger` 中，导致没法正常调试。

这可能就是网站给是增加的一点反调试的手段：

<img src="https://s2.loli.net/2025/06/02/P42Jt3wOYqNxhcZ.png" alt="图片" style="zoom:60%;" />

但这个绕过非常简单， 你只需要右键 `debugger` 的位置，点击 `Never pause here` ，就不会在这里进入断点了：

<img src="https://s2.loli.net/2025/06/02/4BebSDArm537wCc.png" alt="图片" style="zoom:55%;" />





## 神光调试小册笔记

[前端调试通关秘籍](https://juejin.cn/book/7070324244772716556/section) // TODO 看了些，有空体系性地做下笔记



#### 调试代码会遇到的 9 种 JS 作用域

> 👀 基本都是理论知识，主体内容略，这里只摘抄了总结部分，并做了点补充，感觉也够了

JS 总共有 9 种作用域，我们通过调试的方式来分析了下：

- **Global 作用域**： 全局作用域，在浏览器环境下就是 window，在 node 环境下是 global

- **Local 作用域**：本地作用域，<font color=red>或者叫 **函数作用域**</font>

  <img src="https://s2.loli.net/2025/06/02/39O8UqmjK15ARwW.png" alt="" style="zoom:40%;" />

  > 👀 另外，自己试了下：除了 `function` 定义的函数中的变量（哪怕是 ES6 新增的 let 和 const ），使用箭头函数定义的函数，也一样
  >
  > <img src="https://s2.loli.net/2025/06/02/Hw9Z4AzsNOjo578.png" alt="image-20250602201956049" style="zoom:45%;" />

- **Block 作用域**：块级作用域，ES6 新增。<font color=red>**if、while、for 等语句都会生成 Block 作用域**</font>

  <img src="https://s2.loli.net/2025/06/02/fgVZY4GDJIOoybs.png" style="zoom:45%;" />

  > 👀 另外，注意到：和上面的 Local 作用域一样，在 Block 作用域中，let 和 const 定义的变量都在 Block 作用域中（而不是 Script 作用域中）

- **Script 作用域**：let、const 声明的全局变量会保存在 Script 作用域，这些变量可以直接访问，但却不能通过 `window.xx` 访问

  > 👀 同样符合常识的是：直接使用的变量 和 var 声明的变量在 global 作用域中

- **Module 作用域**：ES Module 模块运行的时候会生成 Module 作用域，而 CommonJS 模块运行时严格来说也是函数作用域，因为 Node 执行它的时候会包一层函数，算是比较特殊的函数作用域，有 module、exports、require 等变量

  > 同样的代码，在 node 环境下就没有了 Script 作用域，但是多了一个 Local 作用域：
  >
  > <img src="https://s2.loli.net/2025/06/02/GDTYP1mK6Cc8ur4.png" style="zoom:45%;" />
  >
  > 这个 Local 作用域还有 module、exports、require 等变量，这个叫做模块作用域。

- **Catch Block 作用域**： catch 语句的作用域可以访问错误对象

  > Catch 语句也会生成一个特殊的作用域，Catch Block 作用域，特点是能访问错误对象：
  >
  > <img src="https://s2.loli.net/2025/06/02/bFi9DCj6QOzMTYZ.png" style="zoom: 45%;" />
  >
  > 在 node 里也是一样，只不过还有一层模块作用域：
  >
  > <img src="https://s2.loli.net/2025/06/02/2ICHdyK4l5iVBQN.png" style="zoom:45%;" />
  >
  > <font color=dodgerBlue>那 finally 语句呢？</font>这个就没啥特殊的了，就是 Block 作用域：
  >
  > <img src="https://s2.loli.net/2025/06/02/H5tzN8yxvXeSbcL.png" alt="" style="zoom:45%;" />
  >
  > <img src="https://s2.loli.net/2025/06/02/i1CEaf6TAPbhvXL.png" style="zoom:45%;" />

- **With Block 作用域**：with 语句会把传入的对象的值放到单独的作用域里，这样 with 语句里就可以直接访问了

  > <img src="https://s2.loli.net/2025/06/02/4r1M72cpUaIluen.png" style="zoom:50%;" />
  >
  > 这个 with 语句里的作用是什么？with 语句里的作用域就是这个对象：
  >
  > <img src="https://s2.loli.net/2025/06/02/XuCefbVKxPT6pRv.png" style="zoom:45%;" />
  >
  > 换成普通的对象更明显一些：
  >
  > <img src="https://s2.loli.net/2025/06/02/PfrcqFhBvG6ZLkA.png" style="zoom:50%;" />
  >
  > 可以直接访问 with 对象的值，就是因为形成了一个 With Block 作用域；当然，<font color=red>里面再声明的变量还是在 Block 作用域里</font>。

- **Closure 作用域**：函数返回函数的时候，会把用到的外部变量保存在 Closure 作用域里，这样再执行的时候该有的变量都有，这就是闭包。eval 的闭包比较特殊，会把所有变量都保存到 Closure 作用域

  > ```js
  > function fun() {
  >     const a = 1;
  >     const b = 2;
  >     return function () {
  >         const c = 2;
  > 
  >         console.log(a, c);
  >         debugger;
  >     };
  > }
  > 
  > const f = fun();
  > f();
  > ```
  >
  > 那闭包的变量怎么保存的？通过 node 可以看到：
  >
  > <img src="https://s2.loli.net/2025/06/02/OP92Xq6abKFl7wL.png" style="zoom:45%;" />
  >
  > 通过 Closure 作用域保存了变量 a 的值（👀 原因和闭包概念有关，这里略）
  >
  > 然后执行的时候就会恢复这个 Closure 作用域：
  >
  > <img src="https://s2.loli.net/2025/06/02/AVL4GeN5jEw3ztX.png" alt="20250602_205622.png" style="zoom:45%;" />
  >
  > <font color=dodgerBlue>Closure 作用域也可以多层</font>，比如这样：
  >
  > ```js
  > function fun() {
  >     const a = 1;
  >     const b = 2;
  >     return function () {
  >         const c = 2;
  >         const d = 4;
  > 
  >         return function () {
  >             const e = 5;
  >             console.log(a, c, e);
  >         };
  >     };
  > }
  > 
  > const f = fun()();
  > f();
  > ```
  >
  > 用到的外部变量分别在两个作用域里，那就会生成两个 Closure 作用域：
  >
  > <img src="https://s2.loli.net/2025/06/02/LVhElGFozIyCTWn.png" style="zoom:45%;" />
  >
  > 只留下用到的作用域的变量 a、c

- **Eval 作用域**：eval 代码声明的变量会保存在 Eval 作用域

上面这些都是调试得出的，是 JS 引擎执行代码时的真实作用域。



#### 打断点的 7 种方式

##### 背景

有的时候，我们并不知道应该在哪里打断点：

- 比如代码抛了异常，你想打个断点看看异常出现的原因，但你并不知道异常在哪里发生的。

  > 👀 对应异常断点

- 比如 dom 被某个地方修改了，你想打个断点看看怎么修改的，但你并不知道是哪段代码修改了它。

  > 👀 对应 DOM 断点

- 比如有的断点你想只在满足某个条件的时候断住，不满足条件就不需要断住。

  > 👀 对应条件断点

##### TL;DR

这里提前做下总结，除了一般的打断点，文中提到的断点一共 6 种，如下：

- 异常断点
- 条件断点
- LogPoint
- DOM 断点
- Event Listener 断点
- url 请求断点

##### 异常断点

代码抛了异常，你想知道在哪抛的，这时候就可以用异常断点。

比如这样一段代码：

```js
function add(a, b) {
    throw Error('add');
    return a + b;    
}

console.log(add(1, 2));
```

add 函数里抛了个异常，你想在异常处断住，这时候就可以加个异常断点：

用 node 的调试来跑：

<img src="https://s2.loli.net/2025/06/02/GBrEIyvMWkohXnS.png" alt="img" style="zoom:45%;" />

勾选 Uncaught Exceptions：

<img src="https://s2.loli.net/2025/06/02/BabVDtSprIR9215.png" alt="img" style="zoom:50%;" />

它可以在没有被处理的错误或者 Promise 的 reject 处断住。

上面那个 Caught Exception 是在被 catch 处理的异常出断住。

Uncaught Exceptions 更常用一些。

<img src="https://s2.loli.net/2025/06/02/bGrwEZFLY3vOscI.webp" style="zoom: 40%;" />

当然，网页调试也可以用异常断点：

<img src="https://s2.loli.net/2025/06/02/blCQfwmU4KYNMjV.webp" alt="" style="zoom:40%;" >

##### 条件断点

> 👀 讲的有点基础了，这里略。不过可以看下 [[#《一些你可能不知道的奇葩调试技巧》笔记#条件断点]] 中的内容，其中介绍了一些让人感觉 “这也行？” 的用法

##### LogPoint

略

##### DOM 断点

<font color=dodgerBlue>后面几种断点是网页里专用的，也是在特定场景下很有用的断点类型。</font>

用 create-react-app 创建的 react 项目，有这样一个组件：

<img src="https://s2.loli.net/2025/06/02/NGIy34TYiQadE9j.png" style="zoom:53%;" />

如果想知道 setState 之后是怎么修改 DOM 的，想在 DOM 被修改的时候断住，应该怎么做呢？

找到源码打断点么？不熟悉源码的话，你根本不知道在哪里打断点。

<font color=dodgerBlue>这时候就可以用 DOM 断点了：</font>

先创建一个 chrome 调试配置，把网页跑起来：

<img src="https://s2.loli.net/2025/06/02/kHe1XlwCrD6pibg.png" alt="" style="zoom:45%;" >

然后打开 Chrome DevTools，在 root 的节点上加一个 DOM 断点：

<img src="https://s2.loli.net/2025/06/02/7rP5z96QecZmb32.png" style="zoom:45%;" />

<font color=dodgerBlue>有三种类型：</font>子树修改的时候断住、属性修改的时候断住、节点删除的时候断住。

我们选择第一种，然后刷新页面。

<img src="https://s2.loli.net/2025/06/02/slKPnhMrJ8w24ey.webp" style="zoom:40%;" />

这时候你会发现代码在修改 DOM 的地方断住了，<font color=lightSeaGreen>这就是 React 源码里最终操作 DOM 的地方</font>，看下调用栈就知道 setState 之后是如何更新 DOM 的了。

当然这只是一种用途，特定场景下，DOM 断点是很有用的。

##### Event Listener 断点

之前我们想调试事件发生之后的处理逻辑，需要找到事件监听器，然后打个断点。

但<font color=dodgerBlue>如果你不知道哪里处理的这个事件呢？</font>这时候就可以用事件断点了

打开 sources 面板，就可以找到事件断点，有各种类型的事件：

<img src="https://s2.loli.net/2025/06/02/mjJWo7a4XUhwlK2.png" style="zoom:45%;" />

比如这样一段代码：

<img src="https://s2.loli.net/2025/06/02/qkEQ8nm3aNWPM6H.png" alt="" style="zoom:45%;" />

你找不到哪里处理的点击事件，那就可以加一个 click 的事件断点：

<img src="https://s2.loli.net/2025/06/02/nSugBTszhaIW7td.png" alt="" style="zoom:60%;" />

这时当你点击元素的时候，代码就会在事件处理器断住：

<img src="https://s2.loli.net/2025/06/02/wdrzPTAfQKy6BUk.webp" style="zoom:50%;" />

当然，<font color=dodgerBlue>因为 React 是合成事件</font>，也就是事件绑定在某个元素上自己做的分发，<font color=lightSeaGreen>所以这里是在源码处理事件的地方断住的</font>。<font color=red>用 Vue 就可以直接在事件处理函数处断住</font>。

##### url 请求断点

当你想在某个请求发送的时候断住，但你不知道在哪里发的，这时候就可以用 url 请求断点

比如这样一段代码，你想在发送 url 包含 guang 的请求的时候断住，就可以使用 url 请求断点：

<img src="https://s2.loli.net/2025/06/02/N2aIumJsGyCe4j5.png" alt="" style="zoom:70%;" />

不输入内容就是在任何请求处断住，你可以可以输入内容，那会在 url 包含该内容的请求处断住：

<img src="https://s2.loli.net/2025/06/02/Fkm9LT4Jg5zqGae.png" alt="PNG_image.png" style="zoom:70%;" />

效果如下：

<img src="https://s2.loli.net/2025/06/02/btD32CLMPhUBwuE.png" alt="PNG_image.png" style="zoom:30%;" />

这在调试网络请求的代码的时候，是很有用的。



#### Chrome DevTools 小功能集锦

##### 定位到源码

想知道某个请求是从哪里发出的，可以打开 Network 面板，在每个网络请求的 initiator 部分可以看到发请求代码的调用栈，点击可以快速定位到对应代码。

<img src="https://s2.loli.net/2024/11/26/iyLWnzqHcowtkYA.png" style="zoom:40%;" />

##### 元素定位到创建的源码

想知道某个元素的创建流程，可以通过 Elements 面板选中某个元素，点击 Stack Trace，就会展示出元素创建流程的调用栈；<font color=red>这可以帮你理清前端框架的运行流程</font>

<img src="https://s2.loli.net/2024/11/26/ouI6daQw2XWxfsE.png" style="zoom:40%;" />

<font color=dodgerBlue>这个功能是实验性的，需要手动开启</font>：在 settings 的 expriments 功能里，勾选 “Capture node creation stacks”

<img src="https://s2.loli.net/2024/11/26/duBH8fEm7CvyNhW.png" style="zoom:50%;" />

##### group by folder

<font color=dodgerBlue>网页加载的文件 **默认是按照域名和目录组织的**</font>，找文件时一层层找起来比较麻烦。可以切换为平铺的，会按照 js、css、图片的顺序列出来，找某个文件就容易多了：

<img src="https://s2.loli.net/2024/11/26/bJnNV341EHL8sAG.png" style="zoom:45%;" />

##### Network 自定义展示列

Network 是可以修改展示的列的

<img src="https://s2.loli.net/2024/11/26/vZlEABOVCxe9PD1.png" style="zoom:45%;" />

除此以外，还可以自定义展示的响应 header：

<img src="https://s2.loli.net/2024/11/26/bCM3fDqdJxe5Yz2.png" style="zoom:45%;" />

<img src="https://s2.loli.net/2024/11/26/QkzJ8KOWXc7A2Sp.png" alt="img" style="zoom:50%;" />

比较常用的有 Cache-Control ，可以快速查看每个文件的缓存策略：

<img src="https://s2.loli.net/2024/11/26/LFWgPnk79lUoRJ1.png" style="zoom:50%;" />

##### 代码手动关联 sourcemap

sources 面板可以右键点击 “add soruce map”，输入 sourcemap 的 url 就可以关联上 sourcemap，<font color=red>当调试 **线上代码** 的时候可以用这种方式关联源码</font>。

<img src="https://s2.loli.net/2024/11/26/YxzecUKpBVfDLau.png" style="zoom:45%;" />

##### filter 筛选器筛选内容

> 💡 虽然这里在说 Network 面板，但是这里的部分规则对于 Console 也是适用的，比如 url

<font color=dodgerBlue>输入 **`mime-type:`**</font>（👀 注意冒号），<font color=lightSeaGreen>**Chrome DevTools 就会列出当前网页的请求的所有 mime type**</font>，选择某一种，就会过滤出那种 mime type 的请求。

比如过滤 mp4 请求：

<img src="https://s2.loli.net/2024/11/26/WKdH29uVPgiFCEO.png" style="zoom:45%;" />

过滤 webp 请求：

<img src="https://s2.loli.net/2024/11/26/EgCSGlwThDB2HNJ.png" style="zoom:45%;" />

或者<font color=red>不根据 mime-type，根据资源的大致分类来过滤</font>：

输入 `resource-type:`，会展示出所有的资源分类，包括 document、stylesheet、image 等：

<img src="https://s2.loli.net/2024/11/26/5RPIOiYeBTgnzLG.png" style="zoom:50%;" />

其实这就是 Network 的这部分：

<img src="https://s2.loli.net/2024/11/26/SLp5U3rK8nhmHCo.png" style="zoom:50%;" />

值得注意的是：<font color=red>上面的选项按钮是可以通过 **按住 command 键实现多选** 的</font>

<font color=dodgerBlue>除了资源类型外，**还可以根据状态码过滤**：</font>

<img src="https://s2.loli.net/2024/11/26/xOv3FMKHYLNTwGi.png" style="zoom:50%;" />

<font color=red>**状态码 0 代表被删除或取消的请求**</font>，网络请求是可以被取消的，这种就可以通过状态码 0 来过滤。

此外还可以<font color=dodgerBlue>**根据资源的大小来过滤**</font>：

通过 larger-than 指定 100、300k、2M 等大小的限制，就可以过滤出大小大于这个值的请求。

<img src="https://s2.loli.net/2024/11/26/hvWyAbHP1g36eEX.png" style="zoom:50%;" />

> 👀 这里 100 的默认单位是 字节 bytes
>
> <img src="https://s2.loli.net/2024/11/27/IOrbHFuD4MBYvpE.png" alt="image-20241127001712015" style="zoom:48%;" />
>
> 另外，没有类似 smaller-than 的选项
>
> <img src="https://s2.loli.net/2024/11/27/TqGlhYHs9uL5wXx.png" alt="image-20241127002009335" style="zoom:48%;" />

还可以根据请求方式，是 GET、POST 等来过滤：

<img src="https://s2.loli.net/2024/11/26/bqmnFfa3TP7gD5N.png" style="zoom:50%;" />

根据是否包含某个响应 header 来过滤：

<img src="https://s2.loli.net/2024/11/26/WpRPigr6zOsCFxB.png" alt="img" style="zoom:50%;" />

`has-response-header:Set-Cookie` 过滤出来的就是有设置 cookie 的响应的请求

`has-response-header:access-control-allow-origin` 过滤出来的就是支持跨域的请求

根据<font color=dodgerBlue>**是否包含某个 cookie 来过滤**</font>：

<img src="https://s2.loli.net/2024/11/26/uV4grCZAO3oM85U.png" style="zoom:50%;" />

##### 常用的过滤器主要有这些

- has-response-header：过滤响应包含某个 header 的请求

- method：根据 GET、POST 等请求方式过滤请求

- domain: 根据域名过滤

- status-code：过滤响应码是 xxx 的请求，比如 404、500 等

- larger-than：过滤大小超过多少的请求，比如 100k，1M

- mime-type：过滤某种 mime 类型的请求，比如 png、mp4、json、html 等

- is：<font color=red>过滤某种状态的请求，比如 from cache 从缓存拿的，比如 running 还在运行的</font>

  > 👀 这个有点意思，另外，在神光的另一篇文章 [面试官：你懂 HTTP 缓存，那说下浏览器强制刷新是怎么实现的？](https://juejin.cn/post/7163506251304239135) 中也有提及：
  >
  > > 可以通过 is 过滤器来过滤 from-cache 的请求，也就是所有直接拿了强缓存的请求。
  >
  > 不过，感觉这里侧重点是“强缓存”的判定方法。
  >
  > 另外，在 Network 面板的搜索框试了下 `is:` 能带出的可选项只有四个，分别是： `form-cache` 、 `running` 、`service-worker-initiated` 、`service-worker-intercepted`

- resource-type：根据请求分类来过滤，比如 document 文档请求，stylesheet 样式请求、fetch 请求，xhr 请求，preflight 预检请求

- cookie-name：过滤带有某个名字的 cookie 的请求

当然，这些不需要记，输入一个 `-` 就会提示所有的过滤器：

<img src="https://s2.loli.net/2024/11/26/QOWDcgeoXfFTHaV.png" style="zoom:50%;" />

但是<font color=red>这个减号之后要去掉，它**是非的意思**</font>，这和右边的 invert 选项功能一样。

> 👀 辅助记忆：`git invert`

而且，<font color=red>**这些过滤器都可以组合，只要中间加个空格就行**</font>。

> ##### 💡 补充
>
> 除了上面所说，还有一个 More filters 的选项，其中 “Hide extension URLs” 的选项，感觉不错
>
> <img src="https://s2.loli.net/2024/11/27/WRrUmBMiLo6wGyV.png" alt="image-20241127001221545" style="zoom:50%;" />

##### 这些过滤器不支持根据内容过滤

确实，过滤器不支持这个，但是可以自己搜：

<img src="https://s2.loli.net/2024/11/27/J8wEpOzBceb6gTo.png" style="zoom:50%;" />

##### remove event listeners

Element 面板选中元素可以看到这个元素和它的父元素的所有事件监听器：

<img src="https://s2.loli.net/2024/11/27/WCXymhda15SeH7R.png" style="zoom:45%;" />

可以手动 remove。

比如你想看下拉菜单的样式，但是鼠标一移开就消失了

<img src="https://s2.loli.net/2024/11/27/BXOJD64VKbZpFqy.png" alt="image-20241127091708371" style="zoom:50%;" />

这时候你可以删掉这个按钮的 mouseleave 事件的监听器：

<img src="https://s2.loli.net/2024/11/27/4pwjTbiQDm7aCcl.png" alt="image-20241127091940616" style="zoom:50%;" />

这样移开鼠标也不会消失了。



## DevTools 与 Debug



#### DevTools 组成

##### Sources 面板

V8 的 debug 里把当前的函数作用域叫做 Local，上层的函数作用域叫做 Closure，比如：

<img src="https://s2.loli.net/2025/04/08/Ga7Xe3yFMim5Ovo.png" alt="img" style="zoom:75%;" />

它仅仅是个称呼而已，这里的 Closure 和 Local 也完全可以都换成 Function 字样，比如：

<img src="https://s2.loli.net/2025/04/08/zbsYJX9qEm3VjhQ.png" alt="img" style="zoom:70%;" />

这里的内容在 [[JS 机制与原理#V8 debug 中的 Local 和 Closure]] 中有也摘抄

摘自：[前端初学js萌新，想问关于let的问题? - 紫云飞的回答 - 知乎](https://www.zhihu.com/question/663627652/answer/3588547851)



#### Debug 技巧

##### 断点回溯

在之前的调试中，经常出现 “不小心将某个自己想要调试的代码（断点）遗漏了、错过调试了”，这时候往往只能重新开启调试，最多将一些已经确认无误的断点清理掉。实际上：DevTools 是可以通过 “将当前执行代码所在的执行栈中某一个帧 ( Frame ) 代码” 重新执行的方式，实现重新调试目标代码的功能。

具体实现方式是：在 DevTools 的 Sources 选项的 Call Stack 下选择目标代码所在、且最近的 Frame，右键选择 “Restart frame” ，即可。

<img src="https://s2.loli.net/2025/01/09/3jznkJbGpPZ8vUh.png" alt="image-20250109231205133" style="zoom:50%;" />

不过，这种方法并不是无脑执行的，如上代码，如果每次 **执行完** L9 `val++` 之后，重新对 `called` 这个 Frame 运行 “Restart frame” ，此时会发现作为入参对 val 会一直 +1 ，而不是开始时的等于 1。

帧重启不会重置参数。换句话说，重启不会在调用函数时恢复初始状态。而只是将执行指针移到函数的开头。

至于<font color=dodgerBlue>解决方式是</font>：在 Scope 下双击相应值以进行修改，并设置所需的值。

![](https://s2.loli.net/2025/01/09/dx7G6PiVD3YSp8Q.png)

学习自：[JavaScript 调试参考文档](https://blog.csdn.net/gtlbtnq9mr3/article/details/140811875)



#### Cheat Sheet

![console](https://s2.loli.net/2022/09/26/KtPYa862LcXqs4k.png)

摘自：https://github.com/wangzhengya/cheatsheet



#### 一些快捷键

| **操作类型**                         | **快捷键/说明**                                              |
| ------------------------------------ | ------------------------------------------------------------ |
| 打开调试模式                         | `F12`，`Ctrl + Shift + I` ，Mac 为 ` ⌘ + ⌥ + I`              |
| 切换调试工具位置                     | `ctrl + shift + D` ，Mac 为 `⌘ + shift + D`                  |
| 切换 DevTools 的面板标签             | `ctrl + [ / ]` 左右切换调试工具面板，Mac 为 `⌘ + [ / ]`      |
| Console 保存堆栈信息 ( Stack trace ) | 报错信息可以右键另存为文件，保存完整堆栈信息                 |
| 存储为全局变量 Store as global       | Console 右键把一个对象设置为全局变量，自动命名为`temp*`  。Element 右键把一个元素设置全局变量 |
| `h` 快速隐藏、显示该元素             | 选中元素，按下 `h` 快速隐藏、显示该元素。                    |
| 元素：移动元素                       | 鼠标拖动到任意位置上下移动 `ctrl + ↑ / ↓`  ，Mac 为 `⌘ + ↑ / ↓` |

学习自：[脱发秘籍：前端Chrome调试技巧最全汇总](https://juejin.cn/post/7248118049584316472)



#### Console Utilities API

<font color=dodgerBlue>The Console Utilities API contains a collection of convenience functions for performing common tasks</font> , such as:

- Selecting and inspecting DOM elements.
- Displaying data in a readable format.
- Stopping and starting the profiler.
- <font color=red>Monitoring DOM events</font>.

These commands only work by entering them directly into the DevTools **Console** ; you can't call these commands from scripts.

#####  Summary

| Function                                           | Description                                                  |
| :------------------------------------------------- | :----------------------------------------------------------- |
| <font color=red>`$_`</font>                        | Returns the value of the most recently evaluated expression. |
| <font color=fuchsia>**`$0` - `$4`**</font>         | <font color=fuchsia>Returns a recently selected element or JavaScript object</font>. |
| <font color=red>`$(selector)`</font>               | <font color=red>Query selector</font>; returns the reference to the <font color=red>first DOM element</font> with the specified CSS selector, <font color=red>like `document.querySelector()`</font> . 👀 下面有补充 [[#Query selector]] |
| <font color=red>`$(selector, startNode)`</font>    | <font color=red>Query selector all</font>; returns an <font color=red>array of elements</font> that match the specified CSS selector, <font color=red>like `document.querySelectorAll() `</font> . |
| `$x(path, startNode)`                              | Returns an <font color=red>array of DOM elements</font> that <font color=red>match the specified XPath expression</font>. |
| `clear()`                                          | Clears the console of its history.                           |
| <font color=fuchsia>`copy(object)`</font>          | <font color=fuchsia>Copies a string</font> representation of the specified object <font color=fuchsia>to the clipboard</font>. |
| <font color=fuchsia>`debug(function)`</font>       | When the specified function is called, the debugger is invoked and breaks inside the function on the Sources panel. |
| `dir(object)`                                      | Displays an object-style listing of all of the properties for the specified object, <font color=red>like `console.dir()`</font>. |
| `dirxml(object)`                                   | <font color=red>Prints an XML representation of the specified object</font>, as displayed in the **Elements** tool, <font color=red>like `console.dirxml()`</font> . |
| `inspect(object/function)`                         | <font color=red>Opens and selects</font> the <font color=fuchsia>specified DOM element in the **Elements** tool</font>, <font color=fuchsia>or the specified JavaScript heap object in the **Memory** tool</font>. |
| <font color=red>`getEventListeners(object)`</font> | <font color=red>Returns the event listeners</font> that are <font color=fuchsia>registered on the specified object</font>. 👀[[#getEventListeners]] |
| <font color=red>`keys(object)`</font>              | Returns an <font color=red>array containing the names of the properties belonging to the specified object</font>. 👀 和 `Object.keys()` 作用一样 |
| `monitor(function)`                                | Logs a message to the console that indicates the function name, along with the arguments passed to the function as part of a request. |
| `monitorEvents(object, events)`                    | When one of the specified events occurs on the specified object, the event object is logged to the console. |
| `profile(name)`                                    | <font color=red>Starts a JavaScript CPU **profiling session** with an optional name</font>. |
| `profileEnd(name)`                                 | <font color=red>Completes a JavaScript CPU profiling session and displays the results in the **Memory** tool</font>. |
| `queryObjects(Constructor)`                        | Returns an array of the objects that were created by the specified constructor. |
| `table(data, columns)`                             | Logs object data, formatted as a table with column headings, for the specified data object. |
| `undebug(function)`                                | <font color=red>Stops the debug</font> of the specified function, so that when the function is requested, the debugger is no longer invoked. |
| `unmonitor(function)`                              | Stops the monitoring of the specified function.              |
| `unmonitorEvents(object, events)`                  | Stops monitoring events for the specified object and events. |
| `values(object)`                                   | Returns an array containing the values of all properties belonging to the specified object. 👀 和 `Object.values()` 作用一样 |

##### Query selector

###### Syntax

```js
$(selector, [startNode])
```

This function also supports a second parameter, <font color=fuchsia>`startNode`</font>, that <font color=red>specifies an element or node from which to search for elements</font>. <font color=fuchsia>The default value of the parameter is `document`</font> . 👀 这和 `$$(selector, [startNode])` 一样

> ⚠️ Note : <font color=dodgerBlue>If you are using a library such as jQuery that uses `$`</font> , <font color=fuchsia>the functionality is overwritten</font>, and `$` corresponds to the implementation from that library.

##### Query selector all

和 [[#Query selector]] 没太大区别。

> ⚠️ Note : Press `Shift` + `Enter` in the **Console** to <font color=red>start a new line without running the script</font>.
>
> 👀 这条不重要，但是有点实用。

##### XPath

Returns an array of DOM elements that <font color=red>match the specified XPath expression</font>. 👀 没学过 XPath，注意下

###### Example

```js
$x("//p")
```

#####  inspect

<font color=red>Opens and selects</font> the <font color=fuchsia>specified DOM element in the **Elements** tool</font>, <font color=fuchsia>or the specified ***JavaScript heap object*** in the **Memory** tool</font>.

- For a DOM element, this function opens and selects the specified DOM element in the **Elements** tool.
- For a JavaScript heap object, this function opens the specified JavaScript heap object in the **Memory** tool.

###### Syntax

```js
inspect(object | function)
```

###### Example

In the following example, the `document.body` opens in the **Elements** tool:

```js
inspect(document.body);
```

When passing a function to inspect, the function opens the webpage in the **Sources** tool for you to inspect.

##### debug

<font color=red>When the specified function is called</font>, the <font color=fuchsia>debugger is invoked and breaks inside the function on the Sources panel</font>.

After the debugger is paused, you can then step through the code and debug it.

###### Syntax

```js
debug(function)
```

###### Example

```js
debug("debug");
```

<font color=red>Use `undebug(function)` to stop breaking on the function, or use the UI to turn off all breakpoints</font>.

##### getEventListeners

Returns the event listeners that are registered on the specified object.

The return value is an object that contains an array for each registered event type ( such as `click` or `keydown` ). The members of each array are objects that describe the listener registered for each type.

###### Syntax

```js
getEventListeners(object)
```

###### Example

In the following example, <font color=red>all of the event listeners that are registered on the `document` object are listed</font>:

```js
getEventListeners(document);
```

##### monitor

Logs a message to the console that indicates the function name, along with the arguments passed to the function as part of a request.

###### Syntax

```js
monitor(function)
```

###### Example

```js
function sum(x, y) { return x + y; }
monitor(sum);
```

<img src="https://s2.loli.net/2022/08/26/lEIGvedprghHBay.png" alt="image-20220826205742454" style="zoom:50%;" />

##### monitorEvents

When <font color=red>one of the specified events occurs on the specified object</font>, the event object is logged to the console.

You <font color=red>can specify a single event to monitor, **an array of events**</font>, <font color=fuchsia>or one of the generic events types that are mapped to a predefined collection of events</font>.

###### Syntax

```js
monitorEvents(object[, events]) // 👀 之所以是 events，是因为是可以第二个参数是“事件数组”
```

###### Example

The following code monitors all resize events on the window object:

```js
monitorEvents(window, "resize"); // 👀 修改窗口大小，和放大缩小比例
```

<img src="https://s2.loli.net/2022/08/26/9ixRm28EsFfnZlM.png" alt="image-20220826210033603" style="zoom:55%;" />

The following code defines an array to monitor both `resize` and `scroll` events on the window object:

```js
monitorEvents(window, ["resize", "scroll"]);
```

###### Specifying an event type

You <font color=red>can also specify one of the available types of events</font>, strings that map to predefined sets of events. The following table shows the available event types and the associated event mappings:

| Event type | Corresponding mapped events                                  |
| :--------- | :----------------------------------------------------------- |
| mouse      | "click", "dblclick", "mousedown", "mousemove", "mouseout", "mouseover", "mouseup", "mousewheel" |
| key        | "keydown", "keypress", "keyup", "textInput"                  |
| touch      | "touchcancel", "touchend", "touchmove", "touchstart"         |
| Ctrl       | "blur", "change", "focus", "reset", "resize", "scroll", "select", "submit", "zoom" |

###### Example ( event type )

In the following code, the `key` event type corresponding to `key` events on an input text field are currently selected in the **Elements** tool:

```js
monitorEvents(window, "key");
```

<img src="https://s2.loli.net/2022/08/26/JxphGekbYr1c4Tq.png" alt="image-20220826211307103" style="zoom:50%;" />

##### profile

<font color=fuchsia>Starts a JavaScript CPU profiling session with an optional name</font>.

To complete the profile and display the results in the **Memory** tool, call `profileEnd()` .

######  Syntax

```js
profile([name])
```

###### Example

To start profiling, call `profile() ` :

```js
profile("My profile")
```

To stop profiling and display the results in the **Memory** tool , call `profileEnd()` .

Profiles can also be nested:

```js
profile('A');
profile('B');
profileEnd('A');
profileEnd('B');
```

The result is the same, regardless of the order. The result appears as a Heap Snapshot in the **Memory** tool, with grouped profiles:

<img src="https://s2.loli.net/2022/08/26/jryiB7M2ZglOou6.png" alt="https://docs.microsoft.com/en-us/microsoft-edge/devtools-guide-chromium/media/console-memory-multiple-cpu-profiles.msft.png" style="zoom: 45%;" />

> ⚠️ Note : Multiple CPU profiles can operate at the same time, and you aren't required to close-out each profile in creation order.

##### queryObjects

<font color=red>Returns an array of the objects that were **created by the specified constructor**</font>.

<font color=fuchsia>The scope of `queryObjects()` is the currently selected runtime context in the **Console**</font>.

###### Syntax

```js
queryObjects(Constructor)
```

###### Example

- `queryObjects(promise)` returns all instances of `Promise`.
- `queryObjects(HTMLElement)` returns all HTML elements.
- `queryObjects(functionName)` returns all objects that were instantiated using `new functionName()`.

摘自：[MS Edge - Dev Tools Guide Chromium - Console tool utility functions and selectors](https://docs.microsoft.com/en-us/microsoft-edge/devtools-guide-chromium/console/utilities)

> 👀 另外，也可以参考 [Chrome Developers - Console Utilities API reference](https://developer.chrome.com/docs/devtools/console/utilities/) ，因为 MS Edge 的有一个总结的表格，所以摘抄的 MS Edge 的文档。

##### Copy

> 👀 在写上面笔记的时候，没注意注意到 `copy` 函数，直到看到文章 [11+ chrome 高级调试技巧，学会效率直接提升 666%](https://juejin.cn/post/7085135692568723492) 

要将控制台中打印的对象发给别人，可以通过 `JSON.stringify(target, null, 2)` 实现（ 👀 这也是上文中说的），但是更简单的方法是使用 `copy(target)` 。

> 👀 另外，可以使用 `copy` 记录 DOM 的快照；这个可以看下 [[#记录 DOM 的快照]]



## 性能优化

##### 资料

- [我在性能团队的这两年](https://zhuanlan.zhihu.com/p/16545147271) 作者介绍了一些他在飞书文档团队进行的性能优化的事项，这种规模的团队，无论是实践经验还是团队内部的流程步骤，都是非常有参考性的



## 抓包与代理

##### 资料

- [多代理混战？用 PAC（Proxy Auto-Config） 优雅切换代理场景](https://juejin.cn/post/7524812644717428782)



## 其他技巧



#### 《微软产品经理：你不能不知道的6个Web开发者工具》笔记

##### 使用 ES6 对象写法，显示打印值对应的信息

可能很多朋友在使用 `console.log()` 时，都仅仅忙于记录下具体值、却忘记为其添加来源。例如，在使用以下代码时，我们会得到一份数字清单，但却并不清楚这份清单的含义。

```js
console.log(width)
console.log(height)
```

解决这个问题的简单方法，就是把要记录的内容用大括号括起来。这样 Console 就会同时记录下值与名称。

```js
console.log({width})
console.log({height})
```

<img src="https://s2.loli.net/2022/08/26/ZT5iLaI1DResEcN.png" alt="图片" style="zoom: 60%;" />

##### 使用 `console.assert()`

`console.assert()` 它只会在满足特定条件时记录一条消息（👀 `console.assert(assertion, obj1[, obj2, ... ])` ，只在 `assertion` 为 `false` 的情况下，打印后面的内容 ）。对于这类需求，以往大家可能更习惯于编写包含 `console.log()` 的 “ if 语句”；但这里推荐大家直接使用 `assert()` ，这样更有利于后续清理调试代码。

##### 使用 Console Utilities API

详见 [[#Console Utilities API reference]]，这里略。

##### 无需源代码即可记录：Live Expressions 与 Logpoints 

`console.log()` 的正确使用方式，当然是放置在代码中希望获取信息的位置；但我们也可以使用它深入了解自己无法访问或变更的代码。<font color=fuchsia>Live Expressions 就是一种无需变更代码即可记录信息的好办法</font>。<font color=red>它们能够以惊人的速度记录不断变化的值，但又不会给 Console 带来太大压力、拖慢运行速度</font>。

> 👀 具体使用参见 [Chrome Developments - Watch JavaScript values in real-time with Live Expressions](https://developer.chrome.com/docs/devtools/console/live-expressions/)
>
> <img src="https://s2.loli.net/2022/08/27/5QLiXdD7vseaEH2.png" alt="Typing document.activeElement into the Live Expression text box." style="zoom:30%;" />
>
> 另外，可以注意下这里的 `document.activeElement` 属性

Logpoints 则是一种特殊的断点。我们可以在开发者工具的 Sources tool 中右键点击 JavaScript 中的任意一行并设置 logpoint。系统会提示我们输入想要记录表达式，之后即可在该代码行运行时通过 console 获取它的值。所以从技术上讲，我们完全可以在 web 的任意位置上插入 `console.log()` 。

> 👀 具体使用参见 [Chrome Developments - What's New In DevTools (Chrome 73) # Logpoints](https://developer.chrome.com/blog/new-in-devtools-73/#logpoints)
>
> <img src="https://s2.loli.net/2022/08/27/CrJMBcVseOSY3KN.png" alt="Adding a Logpoint" style="zoom:33%;" />

##### 将代码注入至任意站点：Snippets 与 Overrides

开发者工具中的 Snippets 是一种针对当前网站运行脚本的方式。我们可以在这些脚本中使用 Console Utilities，进而编写并存储那些需要在 Console 中执行的高复杂度 DOM 操作脚本。大家可以使用 snippets 编辑器或者命令菜单，在当前文档的窗口上下文内运行脚本。如果是使用命令菜单的情况，请注意在命令开头使用！并输入要运行的代码段名称。

>  👀 具体使用参见 [Chrome Developers - Run Snippets of JavaScript](https://developer.chrome.com/docs/devtools/javascript/snippets/) ，下图说明了Snippets 的位置；下面的 Overrides 也在同样的位置
>
> <img src="https://s2.loli.net/2022/08/27/cAYqebWRENOrs4x.png" alt="How the page looks before running the Snippet." style="zoom:30%;" />

Overrides 的作用是为远程脚本存储一份本地副本，并在页面加载时执行覆盖。例如，如果我们的整个应用程序构建过程太过缓慢，但又希望随时尝试一点新鲜设计，那么 overrides 就能发挥作用了。另外，这款工具还能在无需浏览器扩展的前提下，替换掉第三方网站中那些烦人的脚本。

> 👀 具体使用参见 [Chrome Developments - What's New In DevTools (Chrome 65) # Local Overrides](https://developer.chrome.com/blog/new-in-devtools-65/#overrides)
>
> <img src="https://s2.loli.net/2022/08/27/pFJuvjHinEUKMP8.gif" alt="img" style="zoom:45%;" />

摘自：[微软产品经理：你不能不知道的6个Web开发者工具](https://mp.weixin.qq.com/s/FMfrl28EoMaasZ5sXsHnjQ)



#### 《11+ chrome高级调试技巧，学会效率直接提升666%》笔记

##### `$i` 直接在控制台安装 npm 包

你遇到过这个场景吗？有时候想使用比如 `dayjs`  或者 `lodash` 的某个 `API`，但是又不想去官网查，如果可以在控制台直接试出来就好了

[Console Importer](https://chrome.google.com/webstore/detail/console-importer/hgajpakhafplebkdljleajgbpdmplhie/related?utm_source=chrome-ntp-icon) 就是这么一个插件，用来在控制台直接安装 `npm` 包。

1. 安装 `Console Importer` 插件

2. `$i('name')` 安装npm包

3. 如下使用：

   <img src="https://s2.loli.net/2022/11/07/dIW8RTBxrqpMNbw.png" alt="image-20221107212336477" style="zoom:50%;" />

##### ⌘ + ⇧ + P 执行 Command 系列

按下 ⌘ + ⇧ + P 可以唤出 DevTools 的 Command 搜索框，输入与 DevTools 的自动联想，选择想要的命令。

> ⚠️ 值得注意的是：对于内容较长的网页，会出现截图无法截全的问题，问了 Gemini 2.5 Pro 🔗 https://g.co/gemini/share/48984f0f7c12 ，并没有直接解决方案方案；只能通过第三方插件或自动化测试的工具来实现截图。我优先选择了 OneNote Web Clipper 导出到 OneNote 中，至于缺点就是清晰度稍差些

下面是一些常见的命令：

###### 截取全屏

输入 `Capture full size screenshot` 按下回车，可实现截取全屏（更确切的说法是“截取当前网页，所有可浏览的部分”）

###### 截取节点

在 Elements 中 选择截取的目标节点，并在 Command 中输入`Capture node screenshot` 并回车，可实现对目标节点进行截取

###### 自由选择截取部分

输入 `Capture area screenshot` ，便可以像截图软件一样，自由选择截取部分。

##### 切换 DevTools 明暗主题

`Switch to dark theme` 或者 `Switch to light theme` 进行主题切换

##### 重新请求接口

重新请求接口，不一定需要刷新界面，也可以在 “ Network ” 中选择想要重新请求的接口，右键选择 “ Replay XHR ”，如下：

<img src="https://s2.loli.net/2022/11/08/cUjLHhsABYbQplN.png" alt="image-20221108013804733" style="zoom:50%;" />

##### 在控制台快速发起请求

和上面类似，不过是选择 “ Copy ” 以及下面的 “ Copy as fetch ”，这时以 fetch 形式实现的 xhr 请求边在系统剪切板中了，可以在 DevTools 中的 Console 进行粘贴与运行

<img src="https://s2.loli.net/2022/11/23/D8Ti16BcLzxmMwO.png" alt="image-20221108013914275" style="zoom:50%;" />

学习自：[11+ chrome高级调试技巧，学会效率直接提升666%](https://juejin.cn/post/7085135692568723492)



#### Chrome DevTools 的 Network 用法



摘自：[Chrome DevTools 的 Network 还能这么用？](https://juejin.cn/post/7159519090229887013)



#### 使用 HTTP proxy

##### DevTools Network 存在的问题

###### 重定向前的请求细节看不到

很多实现 OAuth 相关服务的网站在登录完成后，会跳转到 redirect url 并且带着一个 code，而这时有些网站会拿 code 去交换 access_token，然后再带着 access_token 跳转到下一个页面。如果 code 交换 access_token 这一步有问题，该怎么 debug 呢？

<font color=red>Chrome DevTools 在跳转到其他页面时，默认会把 console 和 network 的东西都清空</font>。<font color=lightSeaGreen>有一个选项叫做“Preserve log” ，把它勾起来以后看似问题就解决了</font>，<font color=red>**但其实没有**</font>。

可以随便找一个网页，打开 DevTools 并且把保留 log 勾起来，然后执行以下代码：

```js
fetch('https://httpbin.org/user-agent')
  .then(()=> window.location ='https://example.com')
```

当跳转完成以后，<font color=red>**虽然 Network 那边确实可以看到这个请求，但点进去以后只会看到 “Failed to load response data” **</font>：

<img src="https://s2.loli.net/2025/05/30/DFaoGd3CEpzlZg2.png" alt="看不到請求" style="zoom:75%;" />

这个问题从 2012 年就有人反馈了，好不容易等了十几年，2023 年底时说这个在 2024 的 roadmap 上，但目前依然没有任何动静：DevTools: XHR (and other resources) content not available after navigation。

<font color=dodgerBlue>这个问题的本质是</font>：<font color=red>**一旦页面跳转，旧页面的 Network 请求数据就会被浏览器丢弃。因此即使请求已经发送，DevTools 也无法保留或查看 Response**</font>。

总之呢，在这个情境之下，看不到 response 基本上没办法 debug，很不方便。

###### WebSocket 连接握手失败找不到原因

虽然我们平常在用 WebSocket 时，只需要一行代码就可以建立连接，但背后其实是分了两步。

第一步会发出一个 HTTP Upgrade 请求，完成以后才切换到 WebSocket 连接。虽然大多数情况下第一步都会成功，那如果第一步失败会怎样呢？

我们可以请 AI 写一个很简单的 demo 出来：

- 写一个 nodejs websocket server，然后用一个 nginx 挡在前面
- nginx 的作用是当 url 含有？debug 的时候要返回 500 错误
- 当 websocket 连接后会往 client 自动发送 hello 的 message
- 最后要包装成可以用 docker compose 跑起来

等 AI 生成完之后用 docker 跑起来，一样随便开个网页建立连接，会发现带有 debug 的那个连接请求，你只知道失败了，却完全不知道原因：

<img src="https://s2.loli.net/2025/05/30/bhDlvZ3gGeMkIcz.png" alt="看不到原因" style="zoom:75%;" />

这个错误信息甚至跟你随便连一个没开的端口一样，完全不知道为什么会失败，这样也很难跟后端说问题在哪里。

其实遇到 WebSocket 握手失败，也可以尝试用其他方式辅助调试，比如用 `curl -i --include` 手动模拟 HTTP Upgrade 请求，检查是不是被后端或代理服务器拦截。这在无法获取浏览器详细错误信息时，是个不错的替代方案。

以上是两个我有印象的范例，但实际开发中应该碰过更多更多，基本上都是只靠 DevTools 来看 Network 没办法解决的问题，要么是看不到，要么看到的东西不太对。

摘自：[人人都需要一個 HTTP proxy 來 debug](https://blog.huli.tw/2025/04/23/everyone-need-a-http-proxy-to-debug/) ，以及简中版本 [人人都需要一个 HTTP proxy 来 debug](https://mp.weixin.qq.com/s/8c9mbxnGxyUcmVzO7zicVw)



#### iOS 模拟器调试 WebView

##### 打开模拟器

打开模拟器至少有两种方法

###### 方法一

spotlight 中输入 “simulator” ，可以直接运行

###### 方法二

Xcode 中 “Xcode” -> “Open Developer Tool” -> “Simulator”

<img src="https://s2.loli.net/2022/09/21/pAaUwdYtOeVCh4T.png" alt="image-20220921234836028" style="zoom:50%;" />

##### 创建新的模拟器与打开模拟器

如下

<img src="https://s2.loli.net/2022/09/21/JdDfCsMr6gAqPiI.png" alt="image-20220921235233136" style="zoom:50%;" />

<img src="https://s2.loli.net/2022/09/21/eUgK7LMOIBZHbvD.png" alt="image-20220921235038192" style="zoom:50%;" />

有一个问题是：只能使用 已经下载的 OS 版本，在这里没法添加新指定的 OS版本 ( OS Version )，感觉有点反人类... 方法见下面

##### 添加指定的 OS 版本

OS 版本必须要在 Xcode 中下载，点击 Xcode 中 “Xcode” -> “window” -> “Devices and Simulators”

<img src="https://s2.loli.net/2022/09/21/grcubENiMp1KXAR.png" alt="image-20220921235659086" style="zoom:40%;" />

进入如下页面，点击 “+” 号

<img src="https://s2.loli.net/2022/09/22/qCkOn5QJfM1zupG.png" alt="image-20220922000009823" style="zoom:40%;" />

选择 “Download more simulator runtimes...”

<img src="https://s2.loli.net/2022/09/22/YjkV7RU5XMDlyqG.png" alt="image-20220922000229461" style="zoom:40%;" />

从而进行选择：

<img src="https://s2.loli.net/2022/09/22/F9CKBc6vynwue7L.png" alt="image-20220922000410775" style="zoom:45%;" />

<img src="https://s2.loli.net/2022/09/22/UwFfnTBe6ucLApm.png" alt="image-20220922000539319" style="zoom:45%;" />

##### 创建与打开 Simulator

<img src="https://s2.loli.net/2022/09/22/gQiobnVB5KpwZXh.png" alt="image-20220922000810475" style="zoom:45%;" />

##### 使用 Simulator

<img src="https://s2.loli.net/2022/09/22/1TamIy639VkRltU.png" alt="image-20220922000957059" style="zoom:35%;" />

需要注意的是：虽然是模拟器，但还是不像 Docker，Simulator 可以访问本机 Mac 的端口，也就是说：本机 Mac 上 web dev server 启动的项目也是可以直接在 模拟器上访问到的；这就不需要打包上传到服务器，再访问了；这样很方便。

> 👀 // TODO https://juejin.cn/post/6844903702445162509 其中还有 `Whistle` 配合 `Proxifier` 的使用，以及利用 Mac safari 的 DevTools 作为调试工具的演示；很有必要看一下w