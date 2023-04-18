# JS 相关库备忘录



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

##### 元素选择器

```js
$("p")
```

##### \#id 选择器

```js
$("#test")
```

##### .class 选择器

```js
$(".test")
```

##### 更多示例

| 语法                                                 | 描述                                                              |
|:-------------------------------------------------- |:--------------------------------------------------------------- |
| $("*")                                             | 选取所有元素                                                          |
| <font color=FF0000>**$(this)**</font>              | 选取<font color=FF0000>当前 HTML 元素</font>                          |
| $("p.intro")                                       | 选取 class 为 intro 的 \<p> 元素                                      |
| <font color=FF0000>**$("p:first")**</font>         | 选取<font color=FF0000>第一个 \<p> 元素</font>                         |
| $("ul li:first")                                   | 选取第一个 \<ul> 元素的第一个 \<li> 元素                                     |
| $("ul li:first-child")                             | 选取每个 \<ul> 元素的第一个 \<li> 元素                                      |
| <font color=FF0000>**$("[href]")**</font>          | 选取<font color=FF0000>带有 href 属性的元素</font>                       |
| $("a[target<font color=FF0000>=</font>'_blank']")  | 选取所有 target 属性值<font color=FF0000>等于</font> "_blank" 的 \<a> 元素  |
| $("a[target<font color=FF0000>!=</font>'_blank']") | 选取所有 target 属性值<font color=FF0000>不等于</font> "_blank" 的 \<a> 元素 |

#### jQuery 事件（action）

**示例：**

```js
$("p").click(function(){
    // 动作触发后执行的代码!!
});
```

**所有用于处理事件的 jQuery 方法：**

| 方法                                                                                                              | 描述                                                                |
|:--------------------------------------------------------------------------------------------------------------- |:----------------------------------------------------------------- |
| [bind()](https://www.runoob.com/jquery/event-bind.html)                                                         | 向元素添加事件处理程序                                                       |
| [<font color=FF0000>blur()</font>](https://www.runoob.com/jquery/event-blur.html)                               | 添加/触发失去焦点事件                                                       |
| [change()](https://www.runoob.com/jquery/event-change.html)                                                     | 添加/触发 change 事件                                                   |
| [<font color=FF0000>click()</font>](https://www.runoob.com/jquery/event-click.html)                             | <font color=FF0000>当按钮点击事件被触发时</font>，添加/触发 click 事件              |
| [<font color=FF0000>dblclick()</font>](https://www.runoob.com/jquery/event-dblclick.html)                       | <font color=FF0000>双击元素时</font>，添加/触发 double click 事件             |
| [delegate()](https://www.runoob.com/jquery/event-delegate.html)                                                 | 向匹配元素的当前或未来的子元素添加处理程序                                             |
| [die()](https://www.runoob.com/jquery/event-die.html)                                                           | 在版本 1.9 中被移除。移除所有通过 live() 方法添加的事件处理程序                            |
| [error()](https://www.runoob.com/jquery/event-error.html)                                                       | 在版本 1.8 中被废弃。添加/触发 error 事件                                       |
| [event.currentTarget](https://www.runoob.com/jquery/jq-event-currenttarget.html)                                | 在事件冒泡阶段内的当前 DOM 元素                                                |
| [event.data](https://www.runoob.com/jquery/event-data.html)                                                     | 包含当前执行的处理程序被绑定时传递到事件方法的可选数据                                       |
| [event.delegateTarget](https://www.runoob.com/jquery/event-delegatetarget.html)                                 | 返回当前调用的 jQuery 事件处理程序所添加的元素                                       |
| [event.isDefaultPrevented()](https://www.runoob.com/jquery/event-isdefaultprevented.html)                       | 返回指定的 event 对象上是否调用了 event.preventDefault()                       |
| [event.isImmediatePropagationStopped()](https://www.runoob.com/jquery/event-isimmediatepropagationstopped.html) | 返回指定的 event 对象上是否调用了 event.stopImmediatePropagation()             |
| [event.isPropagationStopped()](https://www.runoob.com/jquery/event-ispropagationstopped.html)                   | 返回指定的 event 对象上是否调用了 event.stopPropagation()                      |
| [event.namespace](https://www.runoob.com/jquery/event-namespace.html)                                           | 返回当事件被触发时指定的命名空间                                                  |
| [event.pageX](https://www.runoob.com/jquery/event-pagex.html)                                                   | 返回相对于文档左边缘的鼠标位置                                                   |
| [event.pageY](https://www.runoob.com/jquery/event-pagey.html)                                                   | 返回相对于文档上边缘的鼠标位置                                                   |
| [event.preventDefault()](https://www.runoob.com/jquery/event-preventdefault.html)                               | 阻止事件的默认行为                                                         |
| [event.relatedTarget](https://www.runoob.com/jquery/jq-event-relatedtarget.html)                                | 返回当鼠标移动时哪个元素进入或退出                                                 |
| [event.result](https://www.runoob.com/jquery/event-result.html)                                                 | 包含由被指定事件触发的事件处理程序返回的最后一个值                                         |
| [event.stopImmediatePropagation()](https://www.runoob.com/jquery/event-stopimmediatepropagation.html)           | 阻止其他事件处理程序被调用                                                     |
| [event.stopPropagation()](https://www.runoob.com/jquery/event-stoppropagation.html)                             | 阻止事件向上冒泡到 DOM 树，阻止任何父处理程序被事件通知                                    |
| [event.target](https://www.runoob.com/jquery/jq-event-target.html)                                              | 返回哪个 DOM 元素触发事件                                                   |
| [event.timeStamp](https://www.runoob.com/jquery/jq-event-timestamp.html)                                        | 返回从 1970 年 1 月 1 日到事件被触发时的毫秒数                                     |
| [event.type](https://www.runoob.com/jquery/jq-event-type.html)                                                  | 返回哪种事件类型被触发                                                       |
| [event.which](https://www.runoob.com/jquery/event-which.html)                                                   | 返回指定事件上哪个键盘键或鼠标按钮被按下                                              |
| [event.metaKey](https://www.runoob.com/jquery/event_metakey.html)                                               | 事件触发时 META 键是否被按下                                                 |
| [<font color=FF0000>focus()</font>](https://www.runoob.com/jquery/event-focus.html)                             | 添加/触发 focus 事件                                                    |
| [focusin()](https://www.runoob.com/jquery/event-focusin.html)                                                   | 添加事件处理程序到 focusin 事件                                              |
| [focusout()](https://www.runoob.com/jquery/event-focusout.html)                                                 | 添加事件处理程序到 focusout 事件                                             |
| [<font color=FF0000>hover()</font>](https://www.runoob.com/jquery/event-hover.html)                             | 添加两个事件处理程序到 hover 事件                                              |
| [keydown()](https://www.runoob.com/jquery/event-keydown.html)                                                   | 添加/触发 keydown 事件                                                  |
| [keypress()](https://www.runoob.com/jquery/event-keypress.html)                                                 | 添加/触发 keypress 事件                                                 |
| [keyup()](https://www.runoob.com/jquery/event-keyup.html)                                                       | 添加/触发 keyup 事件                                                    |
| [live()](https://www.runoob.com/jquery/event-live.html)                                                         | 在版本 1.9 中被移除。添加一个或多个事件处理程序到当前或未来的被选元素                             |
| [load()](https://www.runoob.com/jquery/event-load.html)                                                         | 在版本 1.8 中被废弃。添加一个事件处理程序到 load 事件                                  |
| [<font color=FF0000>mousedown()</font>](https://www.runoob.com/jquery/event-mousedown.html)                     | <font color=FF0000>鼠标指针移动到元素上方，并按下鼠标按键时</font>，添加/触发 mousedown 事件 |
| [<font color=FF0000>mouseenter()</font>](https://www.runoob.com/jquery/event-mouseenter.html)                   | <font color=FF0000>鼠标指针穿过元素时</font>，添加/触发 mouseenter 事件           |
| [<font color=FF0000>mouseleave()</font>](https://www.runoob.com/jquery/event-mouseleave.html)                   | <font color=FF0000>鼠标指针离开元素时</font>，添加/触发 mouseleave 事件           |
| [mousemove()](https://www.runoob.com/jquery/event-mousemove.html)                                               | 添加/触发 mousemove 事件                                                |
| [mouseout()](https://www.runoob.com/jquery/event-mouseout.html)                                                 | 添加/触发 mouseout 事件                                                 |
| [mouseover()](https://www.runoob.com/jquery/event-mouseover.html)                                               | 添加/触发 mouseover 事件                                                |
| [<font color=FF0000>mouseup()</font>](https://www.runoob.com/jquery/event-mouseup.html)                         | <font color=FF0000>在元素上松开鼠标按钮时</font>，添加/触发 mouseup 事件            |
| [off()](https://www.runoob.com/jquery/event-off.html)                                                           | 移除通过 on() 方法添加的事件处理程序                                             |
| [on()](https://www.runoob.com/jquery/event-on.html)                                                             | 向元素添加事件处理程序                                                       |
| [one()](https://www.runoob.com/jquery/event-one.html)                                                           | 向被选元素添加一个或多个事件处理程序。该处理程序只能被每个元素触发一次                               |
| [$.proxy()](https://www.runoob.com/jquery/event-proxy.html)                                                     | 接受一个已有的函数，并返回一个带特定上下文的新的函数                                        |
| [ready()](https://www.runoob.com/jquery/event-ready.html)                                                       | 规定当 DOM 完全加载时要执行的函数                                               |
| [resize()](https://www.runoob.com/jquery/event-resize.html)                                                     | 添加/触发 resize 事件                                                   |
| [scroll()](https://www.runoob.com/jquery/event-scroll.html)                                                     | 添加/触发 scroll 事件                                                   |
| [select()](https://www.runoob.com/jquery/event-select.html)                                                     | 添加/触发 select 事件                                                   |
| [submit()](https://www.runoob.com/jquery/event-submit.html)                                                     | 添加/触发 submit 事件                                                   |
| [toggle()](https://www.runoob.com/jquery/event-toggle.html)                                                     | 在版本 1.9 中被移除。添加 click 事件之间要切换的两个或多个函数                             |
| [trigger()](https://www.runoob.com/jquery/event-trigger.html)                                                   | 触发绑定到被选元素的所有事件                                                    |
| [triggerHandler()](https://www.runoob.com/jquery/event-triggerhandler.html)                                     | 触发绑定到被选元素的指定事件上的所有函数                                              |
| [unbind()](https://www.runoob.com/jquery/event-unbind.html)                                                     | 从被选元素上移除添加的事件处理程序                                                 |
| [undelegate()](https://www.runoob.com/jquery/event-undelegate.html)                                             | 从现在或未来的被选元素上移除事件处理程序                                              |
| [unload()](https://www.runoob.com/jquery/event-unload.html)                                                     | 在版本 1.8 中被废弃。添加事件处理程序到 unload 事件                                  |
| [contextmenu()](https://www.runoob.com/jquery/event-contextmenu.html)                                           | 添加事件处理程序到 contextmenu 事件                                          |
| [$.holdReady()](https://www.runoob.com/jquery/event-holdready.html)                                             | 用于暂停或恢复.ready() 事件的执行                                             |

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

jQuery 遍历，意为"移动"，用于根据其相对于其他元素的关系来"查找"（或选取）HTML 元素。以某项选择开始，并沿着这个选择移动，直到抵达您期望的元素为止。

下图展示了一个家族树。通过 jQuery 遍历，您能够从被选（当前的）元素开始，轻松地在家族树中向上移动（祖先），向下移动（子孙），水平移动（同胞）。这种移动被称为<font color=FF0000>对 DOM 进行遍历</font>。

其中DOM中祖先的说法，类似于数据结构中树的说法。

#### jQuery 遍历 - 祖先

jquery用于向上遍历 DOM 树的方法：

- **parent()**：parent() 方法<font color=FF0000>返回被选元素的**直接**父元素</font>。该方法只会向上一级对 DOM 树进行遍历。**示例：**
  
  ```js
  //返回每个 \<span> 元素的直接父元素
  $(document).ready(function(){  
    $("span").parent(); 
  });
  ```

- **parents()**：返回被选元素的<font color=FF0000>所有祖先元素</font>，它一路向上直到文档的根元素 (\<html>)。示例：
  
  ```js
  //返回所有<span> 元素的所有祖先
  $(document).ready(function(){  
    $("span").parents(); 
  });
  ```

- **parentsUntil()**：parentsUntil() 方法返回<font color=FF0000>介于两个给定元素之间的所有祖先元素</font>。示例：
  
  ```js
  //返回介于 <span> 与 <div> 元素之间的所有祖先元素
  $(document).ready(function(){
    $("span").parentsUntil("div"); 
  });
  ```

#### jQuery 遍历 - 后代

通过 jQuery，能够向下遍历 DOM 树，以查找元素的后代。下面是两个用于向下遍历 DOM 树的 jQuery 方法：

- **children()**：返回被选元素的<mark style=background-color:silver><font color=FF0000>**所有**</font></mark> <mark style=background-color:aqua><font color=FF0000>**直接**</font></mark> <font color=FF0000>子元素</font>。该方法只会向下一级对 DOM 树进行遍历。**示例**
  
  ```js
  //返回每个 <div> 元素的所有直接子元素
  $(document).ready(function(){  
    $("div").children(); 
  });
  ```

- **find()**：find() 方法<font color=FF0000>返回被选元素的后代元素</font>，一路向下<font color=FF0000>直到最后一个后代</font>。**示例**
  
  ```js
  //返回属于 <div> 后代的所有 <span> 元素
  $(document).ready(function(){
    $("div").find("span"); 
  });
  ```

#### jQuery 遍历 - 同胞(siblings)

同胞拥有相同的父元素。通过 jQuery，能够在 DOM 树中<font color=FF0000>遍历元素的同胞元素</font>。

**在 DOM 树进行水平遍历的方法：**

- **siblings()**：<font color=FF0000>返回被选元素的所有同胞元素</font>。示例：
  
  ```js
  //返回 <h2> 的所有同胞元素
  $(document).ready(function(){  
    $("h2").siblings(); 
  });
  ```
  
  也可以<font color=FF0000>使用可选参数</font>来<font color=FF0000>过滤</font>对同胞元素的搜索
  
  ```js
  //返回属于 <h2> 的同胞元素的所有 <p> 元素
  $(document).ready(function(){  
    $("h2").siblings("p"); 
  });
  ```

- **next()**：<font color=FF0000>返回被选元素的**下一个同胞元素**</font>。该方法<font color=FF0000>只返回一个元素</font>。示例：
  
  ```js
  //返回 <h2> 的下一个同胞元素
  $(document).ready(function(){  
    $("h2").next(); 
  });
  ```

- **nextAll()**：返回<font color=FF0000>被选元素的所有跟随的同胞元素</font>（<mark>即被选元素<font color=FF0000>右边的</font>、<font color=FF0000>所有兄弟节点（**不包含兄弟节点的子节点**）</font></mark>）。示例：
  
  ```js
  //返回 <h2> 的所有跟随的同胞元素
  $(document).ready(function(){  
    $("h2").nextAll(); 
  });
  ```

- **nextUntil()**：返回<font color=FF0000>介于两个给定参数之间（不包含边界的两个元素）的所有跟随的同胞元素</font>。示例：
  
  ```js
  //返回介于 <h2> 与 <h6> 元素之间的所有同胞元素
  $(document).ready(function(){  
    $("h2").nextUntil("h6"); 
  });
  ```

- **prev()**：和next()类似，方向相反

- **prevAll()**：和nextAll()类似，方向相反

- **prevUntil()**：和nextUtil()类似，方向相反

#### jQuery 遍历- 过滤

- 三个最基本的过滤方法是：**first(), last() 和 eq()**，它们允许您<font color=FF0000>基于其在一组元素中的位置来**选择一个特定的元素**</font>。
  
  - **first()**：返回被选元素（被过滤返回的元素列表）的首个元素。示例：
    
    ```js
    //选取首个 <div> 元素内部的第一个 <p> 元素
    $(document).ready(function(){  
      $("div p").first(); 
    });
    ```
  
  - **last()**：返回被选元素的最后一个元素。示例：
    
    ```js
    //选择最后一个 <div> 元素中的最后一个 <p> 元素
    $(document).ready(function(){  
      $("div p").last(); 
    });
    ```
  
  - **eq()**：返回被选元素中<font color=FF0000>带有指定**索引号**的元素</font>。**<font color=FF0000>索引号从 0 开始</font>**，因此首个元素的索引号是 0 而不是 1。示例：
    
    ```js
    //选取第二个 <p> 元素（索引号 1）
    $(document).ready(function(){  
      $("p").eq(1); 
    });
    ```

- 其他过滤方法，比如 **filter() 和 not()** 允许您<font color=FF0000>选取匹配或不匹配某项指定标准的元素</font>。
  
  - **filter()**：允许<font color=FF0000>**自定义规定**一个标准，不匹配这个标准的元素会被从集合中删除，匹配的元素会被返回</font>。
    
    ```js
    //返回带有类名 "url" 的所有 <p> 元素
    $(document).ready(function(){  
      $("p").filter(".url"); 
    });
    ```
  
  - **not()**：返回不匹配标准的所有元素。<font color=FF0000>**not() 方法与 filter() 相反**</font>。
    
    ```js
    //返回不带有类名 "url" 的所有 <p> 元素
    $(document).ready(function(){  
      $("p").not(".url"); 
    });
    ```

#### AJAX

AJAX = 异步 JavaScript 和 XML（Asynchronous JavaScript and XML）。

- **jQuery load() **：load() 方法从服务器加载数据，并把返回的数据放入被选元素中。
  
  **语法:**
  
  ```js
  $(selector).load(URL, data, callback);
  ```
  
  - **URL：**<font color=FF0000>必需</font>，规定您希望加载的 URL资源；也可以把 jQuery 选择器添加到 URL 参数。
  
  - **data：**<font color=FF0000>可选</font>，<font color=FF0000>规定与请求一同发送的查询字符串键/值对集合</font>。
  
  - **callback：**<font color=FF0000>可选</font>，是 load() 方法完成后所执行的函数名称。
    
    回调函数可以设置不同的参数：
    
    - **responseTxt** - 包含调用成功时的结果内容
    - **statusTXT** - 包含调用的状态
    - **xhr** - 包含 XMLHttpRequest 对象

#### jQuery - AJAX get() 和 post() 方法

jQuery get() 和 post() 方法用于通过 HTTP GET 或 POST 请求从服务器请求数据。

- **GET** - 从指定的资源<font color=FF0000>**请求**</font>数据，基本上用于从服务器获得（取回）数据。另外，<font color=FF0000>GET 方法**可能返回**缓存数据</font>。
  
  **jQuery $.get() 方法**：通过 HTTP GET 请求从服务器上请求数据。语法：
  
  ```js
  $.get(URL,callback);
  ```
  
  - **URL**：<font color=FF0000>必需</font>，参数规定您<font color=FF0000>希望请求的 URL</font>。
  - **callback**：<font color=FF0000>可选</font>，参数是<font color=FF0000>**请求成功后**所执行的函数名</font>。

- **POST** - 向指定的资源<font color=FF0000>**提交**</font>要处理的数据，可用于从服务器获取数据。不过，<font color=FF0000>POST 方法**不会缓存**数据</font>，并且<font color=FF0000>**常用于连同请求一起发送数据**</font>。
  
  **jQuery $.post() 方法**：通过 HTTP POST 请求向服务器提交数据。**语法:**
  
  ```js
  $.post(URL,data,callback);
  ```
  
  - **URL**：<font color=FF0000>必需</font>，参数规定您希望请求的 URL。
  - **data**：可选，参数规定<font color=FF0000>连同请求发送的数据</font>。
  - **callback**：可选，参数是请求成功后所执行的函数名。

#### jQuery - noConflict() 方法

如果其他 JavaScript 框架也使用 $ 符号作为简写怎么办？

其中某些框架也使用 `$` 符号作为简写（就像 jQuery），如果您在用的两种不同的框架正在使用相同的简写符号，有可能导致脚本停止运行。<mark>jQuery 的团队考虑到了这个问题，并实现了 noConflict() 方法</mark>。示例如下：

```js
$.noConflict();
jQuery(document).ready(function(){
  jQuery("button").click(function(){
    jQuery("p").text("jQuery 仍然在工作!");
  });
});
```

如果你的 jQuery 代码块使用 `$` 简写，并且您不愿意改变这个快捷方式，那么您可以把 `$` 符号作为变量传递给 ready 方法。这样就可以在函数内使用 `$` 符号了 - 而在函数外，依旧不得不使用 "jQuery"

示例如下：（注意functoin中的$）

```js
$.noConflict();
jQuery(document).ready(function($){
  $("button").click(function(){
    $("p").text("jQuery 仍然在工作!");
  });
});
```

#### 总结

```js
$.ajax({name:value, name:value, ... })
```

该参数规定 AJAX 请求的一个或多个名称/值对。

下面的表格中列出了可能的名称/值：

| 名称                           | 值/描述                                                       |
|:---------------------------- |:---------------------------------------------------------- |
| async                        | 布尔值，表示请求是否异步处理。默认是 true。                                   |
| beforeSend(*xhr*)            | 发送请求前运行的函数。                                                |
| cache                        | 布尔值，表示浏览器是否缓存被请求页面。默认是 true。                               |
| complete(*xhr,status*)       | 请求完成时运行的函数（在请求成功或失败之后均调用，即在 success 和 error 函数之后）。         |
| contentType                  | 发送数据到服务器时所使用的内容类型。默认是："application/x-www-form-urlencoded"。 |
| context                      | 为所有 AJAX 相关的回调函数规定 "this" 值。                               |
| data                         | 规定要发送到服务器的数据。                                              |
| dataFilter(*data*,*type*)    | 用于处理 XMLHttpRequest 原始响应数据的函数。                             |
| dataType                     | 预期的服务器响应的数据类型。                                             |
| error(*xhr,status,error*)    | 如果请求失败要运行的函数。                                              |
| global                       | 布尔值，规定是否为请求触发全局 AJAX 事件处理程序。默认是 true。                      |
| ifModified                   | 布尔值，规定是否仅在最后一次请求以来响应发生改变时才请求成功。默认是 false。                  |
| jsonp                        | 在一个 jsonp 中重写回调函数的字符串。                                     |
| jsonpCallback                | 在一个 jsonp 中规定回调函数的名称。                                      |
| password                     | 规定在 HTTP 访问认证请求中使用的密码。                                     |
| processData                  | 布尔值，规定通过请求发送的数据是否转换为查询字符串。默认是 true。                        |
| scriptCharset                | 规定请求的字符集。                                                  |
| success(*result,status,xhr*) | 当请求成功时运行的函数。                                               |
| timeout                      | 设置本地的请求超时时间（以毫秒计）。                                         |
| traditional                  | 布尔值，规定是否使用参数序列化的传统样式。                                      |
| type                         | 规定请求的类型（GET 或 POST）。                                       |
| url                          | 规定发送请求的 URL。默认是当前页面。                                       |
| username                     | 规定在 HTTP 访问认证请求中使用的用户名。                                    |
| xhr                          | 用于创建 XMLHttpRequest 对象的函数。                                 |

摘自：[jQuery ajax() 方法](https://www.runoob.com/jquery/ajax-ajax.html)
