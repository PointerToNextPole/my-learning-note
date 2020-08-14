# JS及其基本库备忘录



#### onclick对元素绑定js方法

```html
<div onclick="method-name()">
</div>
```



#### JS中的调试

JavaScript中使用`debugger`语句用于debug，示例：

```javascript
function potentiallyBuggyCode() {
    debugger;
    // do potentially buggy stuff to examine, step through, etc.
}
```

在浏览器控制台进入sources下，查看源码并在浏览器中进行debug

<img src="https://s1.ax1x.com/2020/07/29/aZvHwn.png" style="zoom: 30%;" />

摘自：[MDN web docs - debugger](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/debugger)



#### var和let

- <font color=FF0000>在 ES6 之前，我们都是用 var 来声明变量，而且 JS 只有函数作用域和全局作用域，没有块级作用域，所以`{}`限定不了 var 声明变量的访问范围。</font>

  ```javascript
  { 
    var i = 9;
  } 
  console.log(i);  // 9
  ```

  ES6 新增了`let`命令，用来声明局部变量。它的用法类似于`var`，<font color=FF0000>但是所声明的变量，只在`let`命令所在的代码块内有效，而且有暂时性死区的约束</font>

  ```javascript
  { 
    let i = 9;     // i变量只在 花括号内有效！！！
  } 
  console.log(i);  // Uncaught ReferenceError: i is not defined
  ```

  由于上述特性，`let`非常适合用于 `for`循环内部的块级作用域。

- 用`let`声明的变量，**<font color=FF0000>不存在变量提升</font>**（相关概念见下方）。而且<font color=FF0000>要求必须 等`let`声明语句执行完之后，变量才能使用，不然会报`Uncaught ReferenceError`错误。</font>示例：

  ```javascript
  console.log(aicoder);    // 错误：Uncaught ReferenceError ...
  let aicoder = 'aicoder.com';
  // 这里就可以安全使用aicoder
  ```

  > ES6 明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。
  > 总之，在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。

- **let变量不能重复声明**

  let<font color=FF0000>不允许在相同作用域内，重复声明同一个变量。否则报错</font>：`Uncaught SyntaxError: Identifier 'XXX' has already been declared`

  ```javascript
  let a = 0;
  let a = 'sss';
  // Uncaught SyntaxError: Identifier 'a' has already been declared
  ```

摘自：[前端面试题：JS中的let和var的区别](https://www.cnblogs.com/fly_dragon/p/8669057.html)

#### 变量提升（Hoisting）

变量提升是 Javascript中执行上下文 （特别是创建和执行阶段）工作方式的一种认识。在 [ECMAScript® 2015 Language Specification](http://www.ecma-international.org/ecma-262/6.0/index.html) 之前的JavaScript文档中找不到变量提升（Hoisting）这个词。不过，需要注意的是，开始时，这个概念可能比较难理解，甚至恼人。

例如，从概念的字面意义上说，“变量提升”意味着变量和函数的声明会在物理层面移动到代码的最前面，但这么说并不准确。实际上变量和函数声明在代码里的位置是不会动的，而是在编译阶段被放入内存中。

<font color=FF0000>JavaScript 在执行任何代码段之前，将函数声明放入内存中的优点之一是，**你可以在声明一个函数之前使用该函数**。</font>

摘自：[MDN web docs - Hoisting（变量提升）](https://developer.mozilla.org/zh-CN/docs/Glossary/Hoisting)



#### window.open()

```javascript
let windowObjectReference = window.open(strUrl, strWindowName, [strWindowFeatures]);
```

- `WindowObjectReference`

  打开的新窗口对象的引用。如果调用失败，返回值会是 `null 。如果`父子窗口满足“[同源策略](https://developer.mozilla.org/cn/JavaScript的同源策略)”，你可以通过这个引用访问新窗口的属性或方法。

- `strUrl`

  新窗口需要载入的url地址。strUrl可以是web上的html页面也可以是图片文件或者其他任何浏览器支持的文件格式。注意：如果strUrl 是一个空值，那么打开的窗口将会是带有默认工具栏的空白窗口（加载`about:blank`）。

- `strWindowName`

  新窗口的名称。该字符串可以用来作为超链接 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a) 或表单 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/form) 元素的目标属性值。字符串中不能含有空白字符。注意：strWindowName 并不是新窗口的标题。

- `strWindowFeatures`

  可选参数。是一个字符串值，这个值列出了将要打开的窗口的一些特性(窗口功能和工具栏) 。 字符串中不能包含任何空白字符，特性之间用逗号分隔开。参考

摘自：[MDN web docs - Window.open()](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/open)

#### window.close()

`Window.close()` 方法用于：关闭当前窗口或某个指定的窗口。

<font color=FF0000>该方法只能由 [`Window.open()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/open) 方法打开的窗口的 `window` 对象来调用</font>。如果一个窗口不是由脚本打开的，那么，在调用该方法时，JavaScript 控制台会出现类似下面的错误：不能使用脚本关闭一个不是由脚本打开的窗口。或 `Scripts may not close windows that were not opened by script.` 。

同时也要注意，对于由 `HTMLIFrameElement.contentWindow` 返回的 [`Window`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window) 对象，`close()` 也没有效果。

摘自：[MDN web docs - window.close()](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/close)

#### window.location.reload()

- **语法：**

  ```javascript
  object.reload(forcedReload);
  ```

- **参数**
  `forcedReload` 可选，该参数要求为 布尔 类型，当取值为 true 时，将强制浏览器从服务器重新获取当前页面资源，而不是从浏览器的缓存中读取，如果取值为 false 或不传该参数时，浏览器则可能会从缓存中读取当前页面。

摘自：[MDN web docs - Location.reload()](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/reload)



#### alert()、confirm()、prompt()

- **alert()**：alert 方法有一个参数，即希望对用户显示的文本字符串。该字符串不是 HTML 格式。该消息框提供了一个“确定”按钮让用户关闭该消息框，并且该消息框是模式对话框，也就是说，用户必须先关闭该消息框然后才能继续进行操作。

  <img src="https://s1.ax1x.com/2020/07/29/aZWwng.png" style="zoom:50%;" />

- **confirm()**：使用确认消息框可向用户问一个“是-或-否”问题，并且用户可以选择单击“确定”按钮或者单击“取消”按钮。<font color=FF0000>confirm 方法的返回值为 true 或 false</font>。该消息框也是模式对话框：用户必须在响应该对话框（单击一个按钮）将其关闭后，才能进行下一步操作。

  <img src="https://s1.ax1x.com/2020/07/29/aZhEz6.png" style="zoom:50%;" />

- **prompt()**：提示消息框提供了一个文本字段，用户可以在此字段输入一个答案来响应您的提示。该消息框有一个“确定”按钮和一个“取消”按钮。如果您提供了一个辅助字符串参数，则提示消息框将在文本字段显示该辅助字符串作为默认响应。否则，默认文本为 ""。

  <img src="https://s1.ax1x.com/2020/07/29/aZhJQf.png" style="zoom:50%;" />

摘自：[**JS alert()、confirm()、prompt()的区别**](https://www.imooc.com/article/6751)



#### Console

Console 对象可以接入浏览器控制台（如：Firefox 的 Web Console），在不同浏览器上它的实现细节可能是不一样的.

`Console` 对象可以<font color=FF0000>从任何全局对象中访问到</font>，如 [`Window`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window)，[`WorkerGlobalScope`](https://developer.mozilla.org/zh-CN/docs/Web/API/WorkerGlobalScope) 以及控制台属性中的特殊变量。它被定义为 [`Window.console`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/console)，而且可直接通过 `console` 调用。

**方法**

- [`Console.assert()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/assert)

  判断第一个参数是否为真，`false` 的话抛出异常并且在控制台输出相应信息。

- [`Console.clear()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/clear)

  清空控制台，并输出 `Console was cleared`。

- [`Console.count()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/count)

  以参数为标识记录调用的次数，调用时在控制台打印标识以及调用次数。

- [`Console.countReset()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/countReset)

  重置指定标签的计数器值。

- [`Console.debug()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/debug)

  在控制台打印一条 `"debug"` 级别的消息。

- [`Console.dir()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/dir)

  显示一个由特定的 Javascript 对象列表组成的可交互列表。这个列表可以使用三角形隐藏和显示来审查子对象的内容。.

- [`Console.dirxml()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/dirxml)

  打印 XML/HTML 元素表示的指定对象，否则显示 JavaScript 对象视图。

- [`Console.error()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/error)：<font color=FF0000>**常用**</font>

  打印一条错误信息，使用方法可以参考 [string substitution](https://developer.mozilla.org/en-US/docs/Web/API/console#Using_string_substitutions)。

- [`Console.exception()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/exception) 

  `error()` 方法的别称。

- [`Console.group()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/group)

  创建一个新的内联 [group](https://developer.mozilla.org/en-US/docs/Web/API/console#Using_groups_in_the_console), 后续所有打印内容将会以子层级的形式展示。调用 `groupEnd()`来闭合组。

- [`Console.groupCollapsed()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/groupCollapsed)

  创建一个新的内联 [group](https://developer.mozilla.org/en-US/docs/Web/API/console#Using_groups_in_the_console)。使用方法和 `group()` 相同，不同的是，`groupCollapsed()` 方法打印出来的内容默认是折叠的。调用`groupEnd()`来闭合组。

- [`Console.groupEnd()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/groupEnd)

  闭合当前内联 [group](https://developer.mozilla.org/en-US/docs/Web/API/console#Using_groups_in_the_console)。

- [`Console.info()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/info)：<font color=FF0000>**常用**</font>

  打印资讯类说明信息，使用方法可以参考 [string substitution](https://developer.mozilla.org/en-US/docs/Web/API/console#Using_string_substitutions)。

- [`Console.log()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/log)：<font color=FF0000>**常用**</font>

  打印内容的通用方法，使用方法可以参考 [string substitution](https://developer.mozilla.org/en-US/docs/Web/API/console#Using_string_substitutions)。

- [`Console.profile()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/profile) 

  Starts the browser's built-in profiler (for example, the [Firefox performance tool](https://developer.mozilla.org/en-US/docs/Tools/Performance)). You can specify an optional name for the profile.

- [`Console.profileEnd()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/profileEnd) 

  Stops the profiler. You can see the resulting profile in the browser's performance tool (for example, the [Firefox performance tool](https://developer.mozilla.org/en-US/docs/Tools/Performance)).

- [`Console.table()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/table)

  将列表型的数据打印成表格。

- [`Console.time()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/time)

  启动一个以入参作为特定名称的[计时器](https://developer.mozilla.org/en-US/docs/Web/API/console#Timers)，在显示页面中可同时运行的计时器上限为10,000.

- [`Console.timeEnd()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/timeEnd)

  结束特定的 [计时器](https://developer.mozilla.org/en-US/docs/Web/API/console#Timers) 并以豪秒打印其从开始到结束所用的时间。

- [`Console.timeLog()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/timeLog)

  打印特定 [计时器](https://developer.mozilla.org/en-US/docs/Web/API/console#Timers) 所运行的时间。

- [`Console.timeStamp()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/timeStamp) 

  添加一个标记到浏览器的 [Timeline](https://developer.chrome.com/devtools/docs/timeline) 或 [Waterfall](https://developer.mozilla.org/en-US/docs/Tools/Performance/Waterfall) 工具。

- [`Console.trace()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/trace)

  输出一个 [stack trace](https://developer.mozilla.org/en-US/docs/Web/API/console#Stack_traces)。

- [`Console.warn()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/warn)：<font color=FF0000>**常用**</font>

  打印一个警告信息，可以使用 [string substitution](https://developer.mozilla.org/en-US/docs/Web/API/console#Using_string_substitutions) 和额外的参数。

摘自：[MDN web docs - Console](https://developer.mozilla.org/zh-CN/docs/Web/API/Console)



#### JS存储对象（Storage）

作为 Web Storage API 的接口，**`Storage`** <mark>提供了访问特定域名下的<font color=FF0000>**会话存储**</font>（sessionStorage）或<font color=FF0000>**本地存储**</font>（localStorage）的功能</mark>，例如，可以添加、修改或删除存储的数据项。

如果你想要操作一个域名的会话存储，可以使用 [`Window.sessionStorage`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/sessionStorage)；如果想要操作一个域名的本地存储，可以使用 [`Window.localStorage`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/localStorage)。

- **属性**
  **`Storage.length`** 只读，返回一个整数，表示存储在 Storage 对象中的数据项数量。
- **方法**
  - **`Storage.key()`：**该方法接受一个数值 n 作为参数，并返回存储中的第 n 个键名。
  - **`Storage.getItem()`：**该方法接受一个键名作为参数，返回键名对应的值。
  - **`Storage.setItem()`：**该方法接受一个键名和值作为参数，将会把键值对添加到存储中，如果键名存在，则更新其对应的值。
  - **`Storage.removeItem()`：**该方法接受一个键名作为参数，并把该键名从存储中删除。
  - **`Storage.clear()`：**调用该方法会清空存储中的所有键名。

摘自：[MDN web docs - Storage](https://developer.mozilla.org/zh-CN/docs/Web/API/Storage)



#### GlobalEventHandlers.onload

[`GlobalEventHandlers`](https://developer.mozilla.org/zh-CN/docs/Web/API/GlobalEventHandlers) mixin 的 `onload` 属性是一个事件处理程序用于处理[`Window`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window), [`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest), [`<img>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img) 等元素的加载事件，当资源已加载时被触发。

**语法**

```javascript
window.onload = funcRef;
```


当 window load事件触发时，funcRef 方法会被调用；funcRef 是窗口加载事件触发时调用的处理函数。

摘自：[MDN web docs - GlobalEventHandlers.onload](https://developer.mozilla.org/zh-CN/docs/Web/API/GlobalEventHandlers/onload)

***



## jQuery

//todo

`.toggleClass()`

`.data()`



***

## bootstrap

```html
<!--悬浮向左-->
<span class="push-left"></span>
<!--悬浮向右-->
<span class=“push-right”></span>
```



#### Collapse：折叠插件

//todo