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

#### **jQuery 语法**

jQuery 语法是通过选取 HTML 元素，并对选取的元素执行某些操作。

基础语法：

```js
$(selector).action(){
  //...
}
```

- 美元符号定义 jQuery
- 选择符（selector）"查询"和"查找" HTML 元素
- jQuery 的 action() 执行对元素的操作

**补充：**jQuery 使用的语法是 XPath 与 CSS 选择器语法的组合



#### CSS选择器

jQuery 选择器允许您对 HTML 元素组或单个元素进行操作。

jQuery 选择器基于元素的 id、类（class）、类型、属性、属性值等"查找"（或选择）HTML 元素。 它基于已经存在的 [CSS 选择器](https://www.runoob.com/cssref/css-selectors.html)，除此之外，它还有一些自定义的选择器。

- **元素选择器**

  ```js
  $("p")
  ```

- **#id 选择器**

  ```js
  $("#test")
  ```

- **.class 选择器**

  ```js
  $(".test")
  ```

**更多示例：**

| 语法                                               | 描述                                                         |
| :------------------------------------------------- | :----------------------------------------------------------- |
| $("*")                                             | 选取所有元素                                                 |
| <font color=FF0000>**$(this)**</font>              | 选取<font color=FF0000>当前 HTML 元素</font>                 |
| $("p.intro")                                       | 选取 class 为 intro 的 \<p> 元素                             |
| <font color=FF0000>**$("p:first")**</font>         | 选取<font color=FF0000>第一个 \<p> 元素</font>               |
| $("ul li:first")                                   | 选取第一个 \<ul> 元素的第一个 \<li> 元素                     |
| $("ul li:first-child")                             | 选取每个 \<ul> 元素的第一个 \<li> 元素                       |
| <font color=FF0000>**$("[href]")**</font>          | 选取<font color=FF0000>带有 href 属性的元素</font>           |
| $("a[target<font color=FF0000>=</font>'_blank']")  | 选取所有 target 属性值<font color=FF0000>等于</font> "_blank" 的 \<a> 元素 |
| $("a[target<font color=FF0000>!=</font>'_blank']") | 选取所有 target 属性值<font color=FF0000>不等于</font> "_blank" 的 \<a> 元素 |



#### jQuery 事件（action）

**示例：**

```js
$("p").click(function(){
    // 动作触发后执行的代码!!
});
```



**所有用于处理事件的 jQuery 方法：**

| 方法                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [bind()](https://www.runoob.com/jquery/event-bind.html)      | 向元素添加事件处理程序                                       |
| [<font color=FF0000>blur()</font>](https://www.runoob.com/jquery/event-blur.html) | 添加/触发失去焦点事件                                        |
| [change()](https://www.runoob.com/jquery/event-change.html)  | 添加/触发 change 事件                                        |
| [<font color=FF0000>click()</font>](https://www.runoob.com/jquery/event-click.html) | <font color=FF0000>当按钮点击事件被触发时</font>，添加/触发 click 事件 |
| [<font color=FF0000>dblclick()</font>](https://www.runoob.com/jquery/event-dblclick.html) | <font color=FF0000>双击元素时</font>，添加/触发 double click 事件 |
| [delegate()](https://www.runoob.com/jquery/event-delegate.html) | 向匹配元素的当前或未来的子元素添加处理程序                   |
| [die()](https://www.runoob.com/jquery/event-die.html)        | 在版本 1.9 中被移除。移除所有通过 live() 方法添加的事件处理程序 |
| [error()](https://www.runoob.com/jquery/event-error.html)    | 在版本 1.8 中被废弃。添加/触发 error 事件                    |
| [event.currentTarget](https://www.runoob.com/jquery/jq-event-currenttarget.html) | 在事件冒泡阶段内的当前 DOM 元素                              |
| [event.data](https://www.runoob.com/jquery/event-data.html)  | 包含当前执行的处理程序被绑定时传递到事件方法的可选数据       |
| [event.delegateTarget](https://www.runoob.com/jquery/event-delegatetarget.html) | 返回当前调用的 jQuery 事件处理程序所添加的元素               |
| [event.isDefaultPrevented()](https://www.runoob.com/jquery/event-isdefaultprevented.html) | 返回指定的 event 对象上是否调用了 event.preventDefault()     |
| [event.isImmediatePropagationStopped()](https://www.runoob.com/jquery/event-isimmediatepropagationstopped.html) | 返回指定的 event 对象上是否调用了 event.stopImmediatePropagation() |
| [event.isPropagationStopped()](https://www.runoob.com/jquery/event-ispropagationstopped.html) | 返回指定的 event 对象上是否调用了 event.stopPropagation()    |
| [event.namespace](https://www.runoob.com/jquery/event-namespace.html) | 返回当事件被触发时指定的命名空间                             |
| [event.pageX](https://www.runoob.com/jquery/event-pagex.html) | 返回相对于文档左边缘的鼠标位置                               |
| [event.pageY](https://www.runoob.com/jquery/event-pagey.html) | 返回相对于文档上边缘的鼠标位置                               |
| [event.preventDefault()](https://www.runoob.com/jquery/event-preventdefault.html) | 阻止事件的默认行为                                           |
| [event.relatedTarget](https://www.runoob.com/jquery/jq-event-relatedtarget.html) | 返回当鼠标移动时哪个元素进入或退出                           |
| [event.result](https://www.runoob.com/jquery/event-result.html) | 包含由被指定事件触发的事件处理程序返回的最后一个值           |
| [event.stopImmediatePropagation()](https://www.runoob.com/jquery/event-stopimmediatepropagation.html) | 阻止其他事件处理程序被调用                                   |
| [event.stopPropagation()](https://www.runoob.com/jquery/event-stoppropagation.html) | 阻止事件向上冒泡到 DOM 树，阻止任何父处理程序被事件通知      |
| [event.target](https://www.runoob.com/jquery/jq-event-target.html) | 返回哪个 DOM 元素触发事件                                    |
| [event.timeStamp](https://www.runoob.com/jquery/jq-event-timestamp.html) | 返回从 1970 年 1 月 1 日到事件被触发时的毫秒数               |
| [event.type](https://www.runoob.com/jquery/jq-event-type.html) | 返回哪种事件类型被触发                                       |
| [event.which](https://www.runoob.com/jquery/event-which.html) | 返回指定事件上哪个键盘键或鼠标按钮被按下                     |
| [event.metaKey](https://www.runoob.com/jquery/event_metakey.html) | 事件触发时 META 键是否被按下                                 |
| [<font color=FF0000>focus()</font>](https://www.runoob.com/jquery/event-focus.html) | 添加/触发 focus 事件                                         |
| [focusin()](https://www.runoob.com/jquery/event-focusin.html) | 添加事件处理程序到 focusin 事件                              |
| [focusout()](https://www.runoob.com/jquery/event-focusout.html) | 添加事件处理程序到 focusout 事件                             |
| [<font color=FF0000>hover()</font>](https://www.runoob.com/jquery/event-hover.html) | 添加两个事件处理程序到 hover 事件                            |
| [keydown()](https://www.runoob.com/jquery/event-keydown.html) | 添加/触发 keydown 事件                                       |
| [keypress()](https://www.runoob.com/jquery/event-keypress.html) | 添加/触发 keypress 事件                                      |
| [keyup()](https://www.runoob.com/jquery/event-keyup.html)    | 添加/触发 keyup 事件                                         |
| [live()](https://www.runoob.com/jquery/event-live.html)      | 在版本 1.9 中被移除。添加一个或多个事件处理程序到当前或未来的被选元素 |
| [load()](https://www.runoob.com/jquery/event-load.html)      | 在版本 1.8 中被废弃。添加一个事件处理程序到 load 事件        |
| [<font color=FF0000>mousedown()</font>](https://www.runoob.com/jquery/event-mousedown.html) | <font color=FF0000>鼠标指针移动到元素上方，并按下鼠标按键时</font>，添加/触发 mousedown 事件 |
| [<font color=FF0000>mouseenter()</font>](https://www.runoob.com/jquery/event-mouseenter.html) | <font color=FF0000>鼠标指针穿过元素时</font>，添加/触发 mouseenter 事件 |
| [<font color=FF0000>mouseleave()</font>](https://www.runoob.com/jquery/event-mouseleave.html) | <font color=FF0000>鼠标指针离开元素时</font>，添加/触发 mouseleave 事件 |
| [mousemove()](https://www.runoob.com/jquery/event-mousemove.html) | 添加/触发 mousemove 事件                                     |
| [mouseout()](https://www.runoob.com/jquery/event-mouseout.html) | 添加/触发 mouseout 事件                                      |
| [mouseover()](https://www.runoob.com/jquery/event-mouseover.html) | 添加/触发 mouseover 事件                                     |
| [<font color=FF0000>mouseup()</font>](https://www.runoob.com/jquery/event-mouseup.html) | <font color=FF0000>在元素上松开鼠标按钮时</font>，添加/触发 mouseup 事件 |
| [off()](https://www.runoob.com/jquery/event-off.html)        | 移除通过 on() 方法添加的事件处理程序                         |
| [on()](https://www.runoob.com/jquery/event-on.html)          | 向元素添加事件处理程序                                       |
| [one()](https://www.runoob.com/jquery/event-one.html)        | 向被选元素添加一个或多个事件处理程序。该处理程序只能被每个元素触发一次 |
| [$.proxy()](https://www.runoob.com/jquery/event-proxy.html)  | 接受一个已有的函数，并返回一个带特定上下文的新的函数         |
| [ready()](https://www.runoob.com/jquery/event-ready.html)    | 规定当 DOM 完全加载时要执行的函数                            |
| [resize()](https://www.runoob.com/jquery/event-resize.html)  | 添加/触发 resize 事件                                        |
| [scroll()](https://www.runoob.com/jquery/event-scroll.html)  | 添加/触发 scroll 事件                                        |
| [select()](https://www.runoob.com/jquery/event-select.html)  | 添加/触发 select 事件                                        |
| [submit()](https://www.runoob.com/jquery/event-submit.html)  | 添加/触发 submit 事件                                        |
| [toggle()](https://www.runoob.com/jquery/event-toggle.html)  | 在版本 1.9 中被移除。添加 click 事件之间要切换的两个或多个函数 |
| [trigger()](https://www.runoob.com/jquery/event-trigger.html) | 触发绑定到被选元素的所有事件                                 |
| [triggerHandler()](https://www.runoob.com/jquery/event-triggerhandler.html) | 触发绑定到被选元素的指定事件上的所有函数                     |
| [unbind()](https://www.runoob.com/jquery/event-unbind.html)  | 从被选元素上移除添加的事件处理程序                           |
| [undelegate()](https://www.runoob.com/jquery/event-undelegate.html) | 从现在或未来的被选元素上移除事件处理程序                     |
| [unload()](https://www.runoob.com/jquery/event-unload.html)  | 在版本 1.8 中被废弃。添加事件处理程序到 unload 事件          |
| [contextmenu()](https://www.runoob.com/jquery/event-contextmenu.html) | 添加事件处理程序到 contextmenu 事件                          |
| [$.holdReady()](https://www.runoob.com/jquery/event-holdready.html) | 用于暂停或恢复.ready() 事件的执行                            |

摘自：[jQuery 参考手册 - jQuery 事件 方法](https://www.runoob.com/jquery/jquery-ref-events.html)



#### jQuery 效果- 隐藏和显示

- **hide() 和 show()**

  **语法：**

  ```javascript
  $(selector).hide(speed,easing_function,callback);
  $(selector).show(speed,easing_function,callback);
  ```

  - 可选的 <font color=FF0000>**speed**</font> 参数规定<font color=FF0000>隐藏/显示的速度</font>，可以取以下值：<font color=FF0000>"slow"、"fast" 或毫秒</font>。

  - 可选的<font color=FF0000>**easing_function**</font>参数规定<font color=FF0000>过渡使用哪种缓动函数</font>，jQuery自身提供"linear" 和 "swing"，其他可以使用相关的插件，比如：[jQuery Easing 插件](http://gsgd.co.uk/sandbox/jquery/easing/)
  - 可选的 <font color=FF0000>**callback** </font>参数是<font color=FF0000>隐藏或显示完成后所执行的函数名称</font>。

- **toggle()**：可以<font color=FF0000>使用 toggle() 方法来切换 hide() 和 show() 方法</font>。

  **语法：**

  ```js
  $(selector).toggle(speed,easing_function,callback);
  ```

  参数与hide()、show()一致



#### jQuery 效果 - 淡入淡出

通过 jQuery，您可以实现元素的淡入淡出效果。

jQuery 拥有下面四种 fade 方法：

- **fadeIn()**：用于<font color=FF0000>淡入**已隐藏**的元素</font>。**语法:**

  ```js
  $(selector).fadeIn(speed,callback);
  ```

  - 可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。.

  - 可选的 callback 参数是 fading 完成后所执行的函数名称。

- **fadeOut()**：用于<font color=FF0000>淡出**可见**元素</font>。**语法:**

  ```js
  $(selector).fadeOut(speed,callback);
  ```

- **fadeToggle()**：可以<font color=FF0000>在 fadeIn() 与 fadeOut() 方法之间进行切换</font>。

  - 如果元素已淡出，则 fadeToggle() 会向元素添加淡入效果。

  - 如果元素已淡入，则 fadeToggle() 会向元素添加淡出效果。

  **语法:**

  ```js
  $(selector).fadeToggle(speed,callback);
  ```

- **fadeTo()**：允许渐变为给定的不透明度（值介于 0 与 1 之间），**语法:**

  ```js
  $(selector).fadeTo(speed,opacity,callback);
  ```

  其中：必需的 opacity 参数将淡入淡出效果设置为给定的不透明度（值介于 0 与 1 之间）。



#### jQuery 效果 - 滑动

jQuery 拥有以下滑动方法：

- **slideDown()**：用于<font color=FF0000>向下滑动元素</font>。**语法:**

  ```js
  $(selector).slideDown(speed,callback);
  ```

- **slideUp()**：用于向上滑动元素。**语法:**

  ```js
  $(selector).slideUp(speed,callback);
  ```

- **slideToggle()**：可以在 slideDown() 与 slideUp() 方法之间进行切换。

  如果元素向下滑动，则 slideToggle() 可向上滑动它们。

  如果元素向上滑动，则 slideToggle() 可向下滑动它们。

  **语法：**

  ```js
  $(selector).slideToggle(speed,callback);
  ```



#### jQuery 效果- 动画

jQuery animate() 方法允许您创建自定义的动画。**语法：**

```js
$(selector).animate({params},speed,callback);
```

- **params**：**<font color=FF0000>必需</font>**，参数定义形成动画的 CSS 属性。

- **speed**：<font color=FF0000>可选</font>， 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
- **callback**：<font color=FF0000>可选</font>，参数是动画完成后所执行的函数名称。

**示例：**

```js
$("button").click(function(){
  $("div").animate({left:'250px'});
});
```



#### jQuery 停止动画

jQuery stop() 方法用于<font color=FF0000>在动画或效果完成前对它们进行停止</font>。

stop() 方法**<font color=FF0000>适用于所有 jQuery 效果函数</font>**，包括滑动、淡入淡出和自定义动画。

**语法:**

```js
$(selector).stop(stopAll,goToEnd);
```

- **stopAll：**<font color=FF0000>可选</font>，参数规定<font color=FF0000>是否应该清除动画队列。默认是 false，即仅停止活动的动画，允许任何排入队列的动画向后执行</font>。

- **goToEnd：**<font color=FF0000>可选</font>，参数规定<font color=FF0000>是否立即完成当前动画。默认是 false</font>。

因此，默认地，stop() 会清除在被选元素上指定的当前动画。



#### jQuery - 链(Chaining)

通过 jQuery，可以把动作/方法链接在一起。

Chaining 允许我们<font color=FF0000>在一条语句中运行多个 jQuery 方法（在相同的元素上）</font>。



#### jQuery - 获取内容和属性（DOM 操作）

**三个简单实用的用于 DOM 操作的 jQuery 方法（<font color=FF0000>获取内容</font>）：**

- **text()**： <font color=FF0000>设置（有参）</font>或<font color=FF0000>返回（无参）</font>所选元素的<font color=FF0000>**文本内容**</font>
- **html()**： <font color=FF0000>设置（有参）</font>或<font color=FF0000>返回（无参）</font>所选元素的<font color=FF0000>**内容**</font>（包括 HTML 标记）
- **val()**： <font color=FF0000>设置（有参）</font>或<font color=FF0000>返回（无参）</font><font color=FF0000>表单字段的值</font>

**示例：**

```js
$("#btn1").click(function(){
  alert("Text: " + $("#test").text());
});
```

**获取属性值 - attr()**

jQuery attr() 方法用于<font color=FF0000>设置（有参）</font>或<font color=FF0000>返回（无参）</font><font color=FF0000>属性值</font>。示例：

```js
$("button").click(function(){
  alert($("#runoob").attr("href"));
});
```



#### jQuery - 设置内容和属性

- text()、html() 以及 val()<font color=FF0000>拥有回调函数</font>。

  <font color=FF0000>回调函数有两个参数</font>：被选元素列表中当前元素的下标，以及原始（旧的）值。<font color=FF0000>然后以函数新值返回您希望使用的字符串。</font>示例：

  ```js
  $("#btn1").click(function(){
      $("#test1").text(function(i,origText){
          return "旧文本: " + origText + " 新文本: Hello world! (index: " + i + ")"; 
      });
  });
  ```

- **attr()**：设置属性

  - jQuery attr() 方法也<font color=FF0000>用于设置/改变属性值</font>。**示例：**

    ```js
    //演示如何改变（设置）链接中 href 属性的值
    $("button").click(function(){
      $("#runoob").attr("href","http://www.runoob.com/jquery");
    });
    ```

  - attr() 方法也允许您<font color=FF0000>同时设置多个属性</font>。**示例：**

    ```js
    //演示如何同时设置 href 和 title 属性
    $("button").click(function(){    
      $("#runoob").attr({
        "href" : "http://www.runoob.com/jquery",
        "title" : "jQuery 教程"
      });
    });
    ```

  - jQuery 方法 attr()，也提供回调函数。<font color=FF0000>回调函数有**两个参数**</font>：<font color=FF0000>被选元素列表中当前元素的下标，以及原始（旧的）值</font>。然后以函数新值返回您希望使用的字符串。**示例：**

    ```js
    $("button").click(function(){
      $("#runoob").attr("href", function(i,origValue){
        return origValue + "/jquery"; 
      });
    });
    ```



#### jQuery - 添加元素

用于添加新内容的四个 jQuery 方法：

- **append()：** 在被选元素的<font color=FF0000>结尾</font>插入内容（<font color=FF0000>仍然在该元素的内部）</font>，**示例：**

  ```js
  $("p").append("追加文本");
  ```

- **prepend()：** 在被选元素的<font color=FF0000>开头</font>插入内容，**示例：**

  ```js
  $("p").prepend("在开头追加文本");
  ```

- **after()：** 在<font color=FF0000>**被选元素之后**</font>插入内容，**示例：**

  ```js
  $("img").after("在后面添加文本");
  ```

- **before()：** 在<font color=FF0000>**被选元素之前**</font>插入内容，**示例：**

  ```js
  $("img").before("在前面添加文本");
  ```

**补充：**四个方法插入多个元素亦可。示例：

  ```js
  function appendText()
  {
      var txt1="<p>文本。</p>";              // 使用 HTML 标签创建文本
      var txt2=$("<p></p>").text("文本。");  // 使用 jQuery 创建文本
      var txt3=document.createElement("p");
      txt3.innerHTML="文本。";               // 使用 DOM 创建文本 text with DOM
      $("body").append(txt1,txt2,txt3);        // 追加新元素
  }
  ```



#### jQuery - 删除元素

如需删除元素和内容，一般可使用以下两个 jQuery 方法：

- **remove()** ：删除<font color=FF0000>被选元素（**及其**子元素）</font>，示例：

  ```js
  $("#div1").remove();
  ```

  jQuery remove() 方法<font color=FF0000>也可接受一个参数，允许您**对被删元素进行过滤**</font>。

  ```js
  //删除 class="italic" 的所有 <p> 元素
  $("p").remove(".italic");
  ```

- **empty()** ：从被选元素<font color=FF0000>中的删除子元素</font>，示例：

  ```js
  $("#div1").empty();
  ```



#### jQuery - 获取并设置 CSS 类

jQuery 拥有若干进行 CSS 操作的方法

- **addClass()**： 向被选元素<font color=FF0000>添加一个或**多个类**</font>，当然，在添加类时，您也<font color=FF0000>可以**选取多个元素**</font>，也可以<font color=FF0000>**在方法中规定多个类**</font>。示例：

  ```js
  $("button").click(function(){
    // 选取多个元素           在方法中规定多个类
    $("body div:first").addClass("important blue");
  });
  ```

- **removeClass()**： 从被选元素<font color=FF0000>删除一个或**多个类**</font>，与addClass()类似，<font color=FF0000>可以**选取多个元素**</font>。**示例：**

  ```js
  $("button").click(function(){
    $("h1,h2,p").removeClass("blue");
  });
  ```

- **toggleClass()**： 对被选元素进行<font color=FF0000>添加 / 删除类的切换</font>操作。与addClass()类似，<font color=FF0000>可以**选取多个元素**</font>（可以用来实现比如电灯这种只有两种状态的东西）。**示例：**

  ```js
  $("button").click(function(){
    $("h1,h2,p").toggleClass("blue");
  });
  ```

- **css()**：<font color=FF0000> 设置或返回</font>一个或多个样式属性

  - **<font color=FF0000>返回</font> CSS 属性**，语法如下：

    ```js
  css("propertyname");
    ```

    示例如下：

    ```js
    //返回首个匹配元素的 background-color 值
    $("p").css("background-color");
    ```
  
  - **<font color=FF0000>设置</font> CSS 属性**，语法如下：
  
    ```js
  css("propertyname","value");
    ```
  
    示例如下：
  
    ```js
    //为所有匹配元素设置 background-color 值：
    $("p").css("background-color","yellow");
    ```
  
  - <font color=FF0000>设置多个 CSS 属性</font>，语法如下：
  
    ```js
    css({"propertyname":"value","propertyname":"value",...});
    ```
  
    示例如下：
  
    ```js
    //为所有匹配元素设置 background-color 和 font-size：
    $("p").css({"background-color":"yellow","font-size":"200%"});
    ```



#### jQuery 尺寸

<img src="https://i.loli.net/2020/08/27/ewxm9doRMDiPrbV.jpg" style="zoom: 90%;" />

jQuery 提供多个处理尺寸的重要方法：

- **width()**：<font color=FF0000>设置（有参）</font>或<font color=FF0000>返回（无参）</font>元素的宽度（<font color=FF0000>**不包括**内边距、边框或外边距</font>）
- **height()**：<font color=FF0000>设置（有参）</font>或<font color=FF0000>返回（无参）</font>元素的高度（<font color=FF0000>**不包括**内边距、边框或外边距</font>）
- **innerWidth()**：返回元素的宽度（包括内边距）
- **innerHeight()**：返回元素的高度（包括内边距）
- **outerWidth()**：返回元素的宽度（包括内边距和边框）
- **outerHeight()**：返回元素的高度（包括内边距和边框）



#### jQuery 遍历

https://www.runoob.com/jquery/jquery-traversing.html



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