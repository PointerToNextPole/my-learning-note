# Web 调试与性能优化笔记



##### 一些资料

- 关于简单的调试，可以看下 [使用sources进行断点调试【渡一教育】](https://www.bilibili.com/video/BV19N41147nS) 感觉相当易懂

- 关于 performance 选项卡的使用，可以看下 [如何发现前端性能瓶颈【渡一教育】](https://www.bilibili.com/video/BV1aP41147FA) ，感觉视频的教程形式，要比神光调试小册相关部分更易理解些。// TODO 看神光调试小册做笔记时，记得重看一遍，做好笔记。

  另外，还有 [network选项卡【渡一教育】](https://www.bilibili.com/video/BV1Dy4y1N734) ，虽然新学到的东西并不多，但是还是感觉 Initator 和 Timing 选项卡中的内容是之前完全没注意到的，而且有点意思；尤其是 Timing
  
- [67 Weird Debugging Tricks Your Browser Doesn't Want You to Know](https://alan.norbauer.com/articles/browser-debugging-tricks)



#### VS Code 调试单个 TS 文件

##### 预先准备

- 配置好 ts、node、ts-node 等环境

- 安装好 vsc 的 [TypeScript Debugger](https://marketplace.visualstudio.com/items?itemName=kakumei.ts-debug) 调试插件

  <img src="https://s2.loli.net/2024/12/04/76qluPm8wQi3e5U.png" alt="image-20241204235107085" style="zoom:40%;" />

##### 创建 `launch.json`

<img src="/Users/yan/Library/Application Support/typora-user-images/image-20241204235316143.png" alt="image-20241204235316143" style="zoom:50%;" />

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



## 神光调试小册笔记

[前端调试通关秘籍](https://juejin.cn/book/7070324244772716556/section) // TODO 看了些，有空体系性地做下笔记



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
- is：过滤某种状态的请求，比如 from cache 从缓存拿的，比如 running 还在运行的
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



## devTools 使用



##### Cheat Sheet

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

| Function                                             | Description                                                  |
| :--------------------------------------------------- | :----------------------------------------------------------- |
| <font color=red>**\$_**</font>                       | Returns the value of the most recently evaluated expression. |
| <font color=fuchsia>**\$0 - \$4**</font>             | <font color=fuchsia>Returns a recently selected element or JavaScript object</font>. |
| <font color=red>$(selector)</font>                   | <font color=red>Query selector</font>; returns the reference to the <font color=red>first DOM element</font> with the specified CSS selector, <font color=red>like `document.querySelector()`</font> . 👀 下面有补充 [[#Query selector]] |
| <font color=red>\$\$(selector, **startNode**)</font> | <font color=red>Query selector all</font>; returns an <font color=red>array of elements</font> that match the specified CSS selector, <font color=red>like `document.querySelectorAll() `</font> . |
| $x(path, startNode)                                  | Returns an <font color=red>array of DOM elements</font> that <font color=red>match the specified XPath expression</font>. |
| clear()                                              | Clears the console of its history.                           |
| <font color=fuchsia>copy(object)</font>              | <font color=fuchsia>Copies a string</font> representation of the specified object <font color=fuchsia>to the clipboard</font>. |
| debug(function)                                      | When the specified function is called, the debugger is invoked and breaks inside the function on the Sources panel. |
| dir(object)                                          | Displays an object-style listing of all of the properties for the specified object, <font color=red>like `console.dir()`</font>. |
| dirxml(object)                                       | <font color=red>Prints an XML representation of the specified object</font>, as displayed in the **Elements** tool, <font color=red>like `console.dirxml()`</font> . |
| inspect(object/function)                             | <font color=red>Opens and selects</font> the <font color=fuchsia>specified DOM element in the **Elements** tool</font>, <font color=fuchsia>or the specified JavaScript heap object in the **Memory** tool</font>. |
| <font color=red>getEventListeners(object)</font>     | <font color=red>Returns the event listeners</font> that are <font color=fuchsia>registered on the specified object</font>. 👀[[#getEventListeners]] |
| <font color=red>keys(object)</font>                  | Returns an <font color=red>array containing the names of the properties belonging to the specified object</font>. 👀 和 `Object.keys()` 作用一样 |
| monitor(function)                                    | Logs a message to the console that indicates the function name, along with the arguments passed to the function as part of a request. |
| monitorEvents(object, events)                        | When one of the specified events occurs on the specified object, the event object is logged to the console. |
| profile(name)                                        | <font color=red>Starts a JavaScript CPU **profiling session** with an optional name</font>. |
| profileEnd(name)                                     | <font color=red>Completes a JavaScript CPU profiling session and displays the results in the **Memory** tool</font>. |
| queryObjects(Constructor)                            | Returns an array of the objects that were created by the specified constructor. |
| table(data, columns)                                 | Logs object data, formatted as a table with column headings, for the specified data object. |
| undebug(function)                                    | <font color=red>Stops the debug</font> of the specified function, so that when the function is requested, the debugger is no longer invoked. |
| unmonitor(function)                                  | Stops the monitoring of the specified function.              |
| unmonitorEvents(object, events)                      | Stops monitoring events for the specified object and events. |
| values(object)                                       | Returns an array containing the values of all properties belonging to the specified object. 👀 和 `Object.values()` 作用一样 |

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



## 其他技巧



#### 《微软产品经理：你不能不知道的6个Web开发者工具》笔记

##### 使用 ES6 对象写法，显示打印值对应的信息

可能很多朋友在使用“console.log()”时，都仅仅忙于记录下具体值、却忘记为其添加来源。例如，在使用以下代码时，我们会得到一份数字清单，但却并不清楚这份清单的含义。

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

##### 使用 console.assert()

`console.assert()` 它只会在满足特定条件时记录一条消息（👀 `console.assert(assertion, obj1[, obj2, ... ])` ，只在 `assertion` 为 `false` 的情况下，打印后面的内容 ）。对于这类需求，以往大家可能更习惯于编写包含 `console.log()` 的 “ if 语句”；但这里推荐大家直接使用 `assert()` ，这样更有利于后续清理调试代码。

##### 使用 Console Utilities API

详见 [[#Console Utilities API reference]]，这里略。

##### 无需源代码即可记录：Live Expressions 与 Logpoints 

`console.log()` 的正确使用方式，当然是放置在代码中希望获取信息的位置；但我们也可以使用它深入了解自己无法访问或变更的代码。<font color=fuchsia>Live Expressions 就是一种无需变更代码即可记录信息的好办法</font>。<font color=red>它们能够以惊人的速度记录不断变化的值，但又不会给 Console 带来太大压力、拖慢运行速度</font>。

> 👀 具体使用参见 [Chrome Developments - Watch JavaScript values in real-time with Live Expressions](https://developer.chrome.com/docs/devtools/console/live-expressions/)
>
> <img src="https://s2.loli.net/2022/08/27/5QLiXdD7vseaEH2.png" alt="Typing document.activeElement into the Live Expression text box." style="zoom:30%;" />

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

按下 ⌘ + ⇧ + P 可以唤出 DevTools 的 Command 搜索框，输入与 DevTools 的自动联想，选择想要的命令。下面是一些常见的命令：

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

> 👀 // TODO https://juejin.cn/post/6844903702445162509 其中还有 `Whistle ` 配合 `Proxifier` 的使用，以及利用 Mac safari 的 DevTools 作为调试工具的演示；很有必要看一下w