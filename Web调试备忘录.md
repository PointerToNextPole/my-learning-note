# Web 调试笔记



##### 一些资料

关于简单的调试，可以看下 [使用sources进行断点调试【渡一教育】](https://www.bilibili.com/video/BV19N41147nS) 感觉相当易懂



## 神光调试小册笔记

[前端调试通关秘籍](https://juejin.cn/book/7070324244772716556/section) // TODO 首发买的，有空一定看... 👀



## devTools 使用



##### Cheat Sheet

![console](https://s2.loli.net/2022/09/26/KtPYa862LcXqs4k.png)

摘自：https://github.com/wangzhengya/cheatsheet



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
> 👀 注：这条不重要，但是有点实用。

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

> 👀 注：另外，也可以参考 [Chrome Developers - Console Utilities API reference](https://developer.chrome.com/docs/devtools/console/utilities/) ，因为 MS Edge 的有一个总结的表格，所以摘抄的 MS Edge 的文档。

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

> 👀 注：具体使用参见 [Chrome Developments - Watch JavaScript values in real-time with Live Expressions](https://developer.chrome.com/docs/devtools/console/live-expressions/)
>
> <img src="https://s2.loli.net/2022/08/27/5QLiXdD7vseaEH2.png" alt="Typing document.activeElement into the Live Expression text box." style="zoom:30%;" />

Logpoints 则是一种特殊的断点。我们可以在开发者工具的 Sources tool 中右键点击 JavaScript 中的任意一行并设置 logpoint。系统会提示我们输入想要记录表达式，之后即可在该代码行运行时通过 console 获取它的值。所以从技术上讲，我们完全可以在 web 的任意位置上插入 `console.log()` 。

> 👀 注：具体使用参见 [Chrome Developments - What's New In DevTools (Chrome 73) # Logpoints](https://developer.chrome.com/blog/new-in-devtools-73/#logpoints)
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