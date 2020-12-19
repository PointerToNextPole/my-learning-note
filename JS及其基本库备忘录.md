`JS及其基本库备忘录



## JavaScript

#### JavaScript基础

DOM (**D**ocument **O**bject **M**odel)（文档对象模型）是用于访问 HTML 元素的正式 W3C 标准。

- 直接写入 HTML 输出流

  ```js
  document.write("<h1>这是一个标题</h1>");
  ```

- 改变 HTML 内容

  ```js
  x=document.getElementById("demo");  //查找元素
  x.innerHTML="Hello JavaScript";    //改变内容
  ```

- 改变 HTML 图像

  ```js
  element=document.getElementById('myimage');
  element.src="/images/pic_bulboff.gif";
  ```

- 改变 HTML 样式

  ```js
  x=document.getElementById("demo")  //找到元素 
  x.style.color="#ff0000";           //改变样式
  ```

  

#### JavaScript 变量

**js命名规则**

- 变量<font color=FF0000>必须以字母开头</font>
- 变量也能以 $ 和 _ 符号开头（不过我们不推荐这么做）
- 变量名称<font color=FF0000>对大小写敏感</font>（y 和 Y 是不同的变量）



#### JavaScript 数据类型

- **<font color=FF0000>值类型(基本类型)</font>**：字符串（String）、数字(Number)、布尔(Boolean)、对空（Null）、未定义（Undefined）、Symbol（Symbol 是 ES6 引入了一种新的原始数据类型，表示独一无二的值。））

- **<font color=FF0000>引用数据类型</font>**：对象(Object)、数组(Array)、函数(Function)。
- 对象数据类型：Object、Date、Array
- 2 个不包含任何值的数据类型：null、undefined

**Undefined 和 Null**

Undefined 这个值表示变量不含有值。

可以通过将变量的值设置为 null 来清空变量。



#### JavaScript 事件

一些常见的HTML事件的列表:

| 事件        | 描述                         |
| :---------- | :--------------------------- |
| onchange    | HTML 元素改变                |
| onclick     | 用户点击 HTML 元素           |
| onmouseover | 用户在一个HTML元素上移动鼠标 |
| onmouseout  | 用户从一个HTML元素上移开鼠标 |
| onkeydown   | 用户按下键盘按键             |
| onload      | 浏览器已完成页面的加载       |

更多事件列表: [JavaScript 参考手册 - HTML DOM 事件](https://www.runoob.com/jsref/dom-obj-event.html)。



#### JavaScript 字符串

**字符串中可以使用转义字符转义的特殊字符：**

| 代码 |    输出     |
| :--: | :---------: |
|  \'  |   单引号    |
|  \"  |   双引号    |
|  \\  |   反斜杠    |
|  \n  |    换行     |
|  \r  |    回车     |
|  \t  | tab(制表符) |
|  \b  |   退格符    |
|  \f  |   换页符    |

<font color=FF0000>**字符串属性**</font>

|                 属性                 |                             描述                             |
| :----------------------------------: | :----------------------------------------------------------: |
|             constructor              | 返回创建<font color=FF0000>字符串</font>属性的函数（下面有更多解释） |
| <font color=FF0000>**length**</font> |                     **返回字符串的长度**                     |
|            **prototype**             |                  允许您向对象添加属性和方法                  |

**字符串方法**

| 方法                                   | 描述                                                         |
| :------------------------------------- | :----------------------------------------------------------- |
| charAt()                               | 返回指定索引位置的字符                                       |
| charCodeAt()                           | 返回指定索引位置字符的 Unicode 值                            |
| <font color=FF0000>**concat()**</font> | 连接（合并）两个或多个字符串，返回连接后的字符串             |
| fromCharCode()                         | 将 Unicode 转换为字符串                                      |
| <font color=FF0000>indexOf()</font>    | 返回字符串中检索指定字符第一次出现的位置                     |
| lastIndexOf()                          | 返回字符串中检索指定字符最后一次出现的位置                   |
| localeCompare()                        | 用本地特定的顺序来比较两个字符串                             |
| match()                                | 找到一个或多个正则表达式的匹配                               |
| <font color=FF0000>replace()</font>    | 替换与正则表达式匹配的子串                                   |
| search()                               | 检索与正则表达式相匹配的值                                   |
| slice()                                | 提取字符串的片断，并在新的字符串中返回被提取的部分           |
| split()                                | 把字符串分割为子字符串数组                                   |
| substr()                               | 从起始索引号提取字符串中指定数目的字符                       |
| substring()                            | 提取字符串中两个指定的索引号之间的字符                       |
| toLocaleLowerCase()                    | 根据主机的语言环境把字符串转换为小写，只有几种语言（如土耳其语）具有地方特有的大小写映射 |
| toLocaleUpperCase()                    | 根据主机的语言环境把字符串转换为大写，只有几种语言（如土耳其语）具有地方特有的大小写映射 |
| toLowerCase()                          | 把字符串转换为小写                                           |
| toString()                             | 返回字符串对象值                                             |
| toUpperCase()                          | 把字符串转换为大写                                           |
| trim()                                 | 移除字符串首尾空白                                           |
| valueOf()                              | 返回某个字符串对象的原始值                                   |

更多方法实例可以参见：[JavaScript String 对象](https://www.runoob.com/jsref/jsref-obj-string.html)。



#### JavaScript 比较 和 逻辑运算符

- **===** ：绝对等于（值和类型均相等）

- **!==**： <font color=FF0000>不绝对等于</font>（<font color=FF0000>**值和类型有一个不相等，或两个都不相等**</font>）



#### For 循环

- 一般for循环

- for-in循环，for-in 循环实际是为循环”enumerable“对象而设计的。示例：

  ```js
  for(elem in elems){
    //code
  }
  ```

  <font color=FF0000>不推荐用 for-in 来循环一个**数组**，因为，不像对象，数组的`index`跟普通的对象属性不一样，是重要的数值序列指标</font>。

- forEach循环（ JavaScript5 引入）

  forEach() 方法用于调用<font color=FF0000>数组</font>的每个元素，并将元素传递给回调函数。
  
  **语法：**
  
  ```js
  array.forEach(function(currentValue, index, arr), thisValue)
  ```
  
  **参数：**
  
  - **function(currentValue, index, arr)**：<font color=FF0000>必需</font>。 数组中每个元素需要调用的函数。
    - **currentValue**	必需。当前元素
    - **index**	可选。当前元素的索引值。
    - **arr**	可选。当前元素所属的数组对象。
  - **thisValue**：<font color=FF0000>可选</font>。传递给函数的值一般用 "this" 值。
  
  **示例：**
  
  ```js
  myArray.forEach(function (value) {
    console.log(value);
  });
  ```
  
  写法简单了许多，但<mark>也有短处：你不能中断循环(使用`break`语句或使用`continue`语句。</mark>（但是支持return）
  
- for-of循环

  ```js
  for (var value of myArray) {
    console.log(value);
  }
  ```

  它既比传统的 for 循环简洁，同时弥补了 forEach 和 for-in 循环的短板。

  //todo  for-of的具体使用参考下面的文章。

摘自：[JavaScript里的循环方法：forEach，for-in，for-of](https://www.webhek.com/post/javascript-loop-foreach-for-in-for-of.html)



#### JavaScript typeof, null, 和 undefined

可以使用 typeof 操作符来检测变量的数据类型。**实例**

```js
typeof "John"        // 返回 string
typeof 3.14         // 返回 number
typeof false         // 返回 boolean
typeof [1,2,3,4]       // 返回 object，这里要注意：数组是一种特殊的对象类型。
typeof {name:'John', age:34} // 返回 object
```

<font color=FF0000>**undefined 和 null 的区别**</font>

```js
typeof undefined             // undefined
typeof null                  // object
null === undefined           // false
null == undefined            // true
```

更多类型

```js
typeof NaN                    // 返回 number
typeof new Date()             // 返回 object
typeof function () {}         // 返回 function
typeof myCar                  // 返回 undefined (如果 myCar 没有声明)
typeof null                   // 返回 object
```



#### JavaScript 类型转换

**constructor 属性**：返回所有 JavaScript 变量的构造函数。

```js
"John".constructor                 // 返回函数 String()  { [native code] }
(3.14).constructor                 // 返回函数 Number()  { [native code] }
false.constructor                  // 返回函数 Boolean() { [native code] }
[1,2,3,4].constructor              // 返回函数 Array()   { [native code] }
{name:'John', age:34}.constructor  // 返回函数 Object()  { [native code] }
new Date().constructor             // 返回函数 Date()    { [native code] }
function () {}.constructor         // 返回函数 Function(){ [native code] }
```

**转换为字符串**

- **<font color=FF0000>全局方法</font>** **String()** 可以将数字转换为字符串，<font color=FF0000>该方法（全局方法）可用于任何类型的数字，字母，变量，表达式</font>。示例：

  ```js
  String(x)         // 将变量 x 转换为字符串并返回
  String(123)       // 将数字 123 转换为字符串并返回
  String(100 + 23)  // 将数字表达式转换为字符串并返回
  ```

- Number / Boolean / Date 的方法 **toString()** 也是有同样的效果，示例：

  ```js
  x.toString()
  (123).toString()
  (100 + 23).toString()
  ```

- 更多数字转换为字符串的方法：

  | 方法                                    | 描述                                                         |
  | :-------------------------------------- | :----------------------------------------------------------- |
  | toExponential()                         | 把对象的值转换为指数计数法。                                 |
  | <font color=FF0000>**toFixed()**</font> | 把数字转换为字符串，结果的小数点后有<font color=FF0000>通过**四舍五入**指定位数的数字</font>。 |
  | toPrecision()                           | 把数字格式化为指定的长度。                                   |

- 关于日期转换为字符串的函数：

  | 方法              | 描述                                        |
  | :---------------- | :------------------------------------------ |
  | getDate()         | 从 Date 对象返回一个月中的某一天 (1 ~ 31)。 |
  | getDay()          | 从 Date 对象返回一周中的某一天 (0 ~ 6)。    |
  | getFullYear()     | 从 Date 对象以四位数字返回年份。            |
  | getHours()        | 返回 Date 对象的小时 (0 ~ 23)。             |
  | getMilliseconds() | 返回 Date 对象的毫秒(0 ~ 999)。             |
  | getMinutes()      | 返回 Date 对象的分钟 (0 ~ 59)。             |
  | getMonth()        | 从 Date 对象返回月份 (0 ~ 11)。             |
  | getSeconds()      | 返回 Date 对象的秒数 (0 ~ 59)。             |
  | getTime()         | 返回 1970 年 1 月 1 日至今的毫秒数。        |

  更多请参考：[Date 方法](https://www.runoob.com/jsref/jsref-obj-date.html) 

**将字符串转换为数字**

<font color=FF0000>**全局方法**</font> <font color=FF0000>**Number()** 可以将字符串转换为数字。</font>

- 字符串包含数字(如 "3.14") 转换为数字 (如 3.14).

- 空字符串转换为 0。

- 其他的字符串会转换为 NaN (不是个数字)。

示例如下：

```js
Number("3.14")    // 返回 3.14
Number(" ")       // 返回 0
Number("")        // 返回 0
Number("99 88")   // 返回 NaN
```

在 [Number 方法](https://www.runoob.com/jsref/jsref-obj-number.html) 中，你可以查看到更多关于字符串转为数字的方法，比如：

| 方法         | 描述                               |
| :----------- | :--------------------------------- |
| parseFloat() | 解析一个字符串，并返回一个浮点数。 |
| parseInt()   | 解析一个字符串，并返回一个整数。   |

**一元运算符 +**

**Operator +** 可用于将变量转换为数字。示例： 

```js
var y = "5";   // y 是一个字符串
var x = + y;   // x 是一个数字
```

<font color=FF0000>如果变量不能转换，它仍然会是一个数字，但值为 NaN</font> (不是一个数字)，示例：

```js
var y = "John";   // y 是一个字符串
var x = + y;      // x 是一个数字 (NaN)
```

**将布尔值转换为数字**

全局方法 **Number()** 可将布尔值转换为数字。

```js
Number(false)   // 返回 0
Number(true)   // 返回 1
```

**将日期转换为数字**

全局方法 **Number()** 可将日期转换为数字，示例：

```js
d = new Date();
Number(d)     // 返回 1404568027739
```

日期方法 **getTime()** 也有相同的效果。

```js
d = new Date();
d.getTime()    // 返回 1404568027739
```



#### instanceof

instanceof 运算符用于<font color=FF0000>检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上</font>。返回值为 true / false

示例如下：

```js
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}
const auto = new Car('Honda', 'Accord', 1998);

console.log(auto instanceof Car);
// expected output: true

console.log(auto instanceof Object);
// expected output: true
```





#### JS获取对象的结构：constructor

//TODO



#### JavaScript 正则表达式

正则表达式语法：

```
/正则表达式主体/修饰符(可选)
```

示例：

```js
var patt = /runoob/i
```

- **/runoob/i** 是一个正则表达式。
  - **runoob** 是一个**正则表达式主体** (用于检索)。
  - **i** 是一个**修饰符** (搜索不区分大小写)。

在 JavaScript 中，正则表达式通常用于两个字符串方法 : **search() 和 replace()**。

- **search() 方法** 用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串，并<font color=FF0000>返回子串的起始位置</font>。

- **replace() 方法** 用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。

**正则表达式修饰符**

**修饰符** 可以在全局搜索中不区分大小写:

| 修饰符 | 描述                                                     |
| :----- | :------------------------------------------------------- |
| i      | 执行对大小写不敏感的匹配。                               |
| g      | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。 |
| m      | 执行多行匹配。                                           |

**使用 RegExp 对象**

在 JavaScript 中，RegExp 对象是一个预定义了属性和方法的正则表达式对象。

- **test()** 方法<font color=FF0000>用于检测一个字符串是否匹配某个模式</font>，如果字符串中含有匹配的文本，则返回 true，否则返回 false。

- **exec()** 方法是一个正则表达式方法。exec() 方法用于检索字符串中的正则表达式的匹配。

  <font color=FF0000>**该函数返回一个数组**，其中存放匹配的结果</font>。<font color=FF0000>如果未找到匹配，则返回值为 null</font>。



#### throw

throw出去的异常将会被catch会捕捉到，放（拼接）在catch的参数中。



#### JavaScript 变量提升

JavaScript 中，函数及变量的声明都将被提升到函数的最顶部。变量可以先使用后声明，也就是变量可以先使用再声明。这就是"hoisting(变量提升)"。

变量提升：函数声明和变量声明总是会被解释器悄悄地被<font color=FF0000>"提升"</font>到方法体的最顶部（不是真的被提升，下面还有说明）。

另外，要注意的是：<font color=FF0000>JavaScript **只有声明的变量会提升，初始化的不会**</font>。示例如下：

```js
var x = 5; // 初始化 x

elem = document.getElementById("demo"); // 查找元素
elem.innerHTML = x + " " + y;           // 显示 x 和 y

var y = 7; // 初始化 y
```

结果为：x 为：5，y 为：undefined

**由上现象可知：**对于大多数程序员来说并不知道 JavaScript 变量提升。如果程序员不能很好的理解变量提升，他们写的程序就容易出现一些问题。为了避免这些问题，通常我们在每个作用域开始前声明这些变量，这也是正常的 JavaScript 解析步骤，易于我们理解。



#### JavaScript 严格模式(use strict)

"use strict" 指令在 JavaScript 1.8.5 (ECMAScript5) 中新增。它不是一条语句，但是一个字面量表达式，在 JavaScript 旧版本中会被忽略。<mark>"use strict" 的目的是指定代码在严格条件下执行</mark>。

**为什么使用严格模式:**

- 消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为;

- 消除代码运行的一些不安全之处，保证代码运行的安全；
- 提高编译器效率，增加运行速度；
- 为未来新版本的Javascript做好铺垫。

"严格模式"体现了Javascript更合理、更安全、更严谨的发展方向，包括IE 10在内的主流浏览器，都已经支持它，许多大项目已经开始全面拥抱它。

另一方面，同样的代码，在"严格模式"中，可能会有不一样的运行结果；一些在"正常模式"下可以运行的语句，在"严格模式"下将不能运行。掌握这些内容，有助于更细致深入地理解Javascript，让你变成一个更好的程序员。



**严格模式的限制**

- 不允许使用未声明的变量
- 不允许删除变量或对象，或函数（使用delete）
- 不允许变量重名
- 不允许使用八进制
- 不允许使用转义字符
- 不允许对只读属性赋值
- 不允许对一个使用getter方法读取的属性进行赋值
- 不允许删除一个不允许删除的属性
- 变量名不能使用 "eval" 字符串
- 变量名不能使用 "eval" 字符串
- 由于一些安全原因，在作用域 eval() 创建的变量不能被调用
- 禁止this关键字指向全局对象

限制的示例请看：[JavaScript 严格模式(use strict)](https://www.runoob.com/js/js-strict.html)

**为了向将来Javascript的新版本过渡，严格模式新增了一些保留关键字：**

- implements
- interface
- let
- package
- private
- protected
- public
- static
- yield



#### JavaScript 使用误区

```js
var x = 10;
var y = "10";
if (x == y)
```

<font color=FF0000>结果：返回true</font>

在严格的比较运算中，<font color=FF0000>=== 为恒等计算符，同时**检查表达式的值**与**类型**</font>，以下 if 条件语句返回 false：

```js
var x = 10;
var y = "10";
if (x === y)
```

**数组易错点：**

- JavaScript 不支持使用名字来索引数组，只允许使用数字索引。

- 在 JavaScript 中, **对象** 使用 **名字作为索引**。

**Undefined 不是 Null**

在 JavaScript 中, **null** 用于对象, **undefined** 用于变量，属性和方法。

对象只有被定义才有可能为 null，否则为 undefined。

**程序块作用域**

在每个代码块中 JavaScript 不会创建一个新的作用域，<font color=FF0000>一般各个代码块的作用域都是全局的</font>。示例如下：

```js
// 以下代码的的变量 i 返回 10，而不是 undefined：
for (var i = 0; i < 10; i++) {
  // some code
}
return i;
```



#### **JavaScript 表单**

HTML 表单验证可以通过 JavaScript 来完成。



#### HTML 约束验证

HTML5 新增了 HTML 表单的验证方式：约束验证（constraint validation）。约束验证是表单被提交时浏览器用来实现验证的一种算法。

**HTML 约束验证基于：**

- **HTML 输入属性**

  | 属性     | 描述                     |
  | :------- | :----------------------- |
  | disabled | 规定输入的元素不可用     |
  | max      | 规定输入元素的最大值     |
  | min      | 规定输入元素的最小值     |
  | pattern  | 规定输入元素值的模式     |
  | required | 规定输入元素字段是必需的 |
  | type     | 规定输入元素的类型       |

  完整列表，请查看 [HTML 输入属性](https://www.runoob.com/html/html5-form-attributes.html)。

- **CSS 伪类选择器**

  | 选择器    | 描述                                    |
  | :-------- | :-------------------------------------- |
  | :disabled | 选取属性为 "disabled" 属性的 input 元素 |
  | :invalid  | 选取无效的 input 元素                   |
  | :optional | 选择没有"required"属性的 input 元素     |
  | :required | 选择有"required"属性的 input 元素       |
  | :valid    | 选取有效值的 input 元素                 |

  完整列表，请查看 [CSS 伪类](https://www.runoob.com/css/css-pseudo-classes.html)。

- **DOM 属性和方法**



#### JavaScript 验证 API

示例：

```html
<p>输入数字并点击验证按钮:</p>
<!-- 注意这里的type=“number” -->
<input id="id1" type="number" min="100" max="300" required>
<button onclick="myFunction()">验证</button>

<p>如果输入的数字小于 100 或大于300，会提示错误信息。</p>

<p id="demo"></p>

<script>
function myFunction() {
    var inpObj = document.getElementById("id1");
    if (inpObj.checkValidity() == false) {
        document.getElementById("demo").innerHTML = inpObj.validationMessage;
    } else {
        document.getElementById("demo").innerHTML = "输入正确";
    }
}
</script>
```

**约束验证 DOM 方法**

| Property            | Description                                                  |
| :------------------ | :----------------------------------------------------------- |
| checkValidity()     | 如果 input 元素中的数据是合法的返回 true，否则返回 false。   |
| setCustomValidity() | 设置 input 元素的 validationMessage 属性，用于自定义错误提示信息的方法。使用 setCustomValidity 设置了自定义提示后，validity.customError 就会变成true，则 checkValidity 总是会返回false。如果要重新判断需要取消自定义提示，方式如下：<br>`setCustomValidity('')`<br>`setCustomValidity(null) `<br>`setCustomValidity(undefined)` |

**约束验证 DOM 属性**

| 属性              | 描述                                  |
| :---------------- | :------------------------------------ |
| validity          | 布尔属性值，返回 input 输入值是否合法 |
| validationMessage | 浏览器错误提示信息                    |
| willValidate      | 指定 input 是否需要验证               |

**Validity 属性**

input 元素的 **validity 属性**包含一系列关于 validity 数据属性:

| 属性            | 描述                                                       |
| :-------------- | :--------------------------------------------------------- |
| customError     | 设置为 true, 如果设置了自定义的 validity 信息。            |
| patternMismatch | 设置为 true, 如果元素的值不匹配它的模式属性。              |
| rangeOverflow   | 设置为 true, 如果元素的值大于设置的最大值。                |
| rangeUnderflow  | 设置为 true, 如果元素的值小于它的最小值。                  |
| stepMismatch    | 设置为 true, 如果元素的值不是按照规定的 step 属性设置。    |
| tooLong         | 设置为 true, 如果元素的值超过了 maxLength 属性设置的长度。 |
| typeMismatch    | 设置为 true, 如果元素的值不是预期相匹配的类型。            |
| valueMissing    | 设置为 true，如果元素 (required 属性) 没有值。             |
| valid           | 设置为 true，如果元素的值是合法的。                        |



#### JavaScript关键字

## JavaScript 保留关键字

Javascript 的保留关键字不可以用作变量、标签或者函数名。有些保留关键字是作为 Javascript 以后扩展使用。


|					 |					 |						|						|							 |
| -------- | --------- | ---------- | --------- | ------------ |
| abstract | arguments | boolean    | break     | byte         |
| case     | catch     | char       | class*    | const        |
| continue | debugger  | default    | delete    | do           |
| double   | else      | enum*      | eval      | export*      |
| extends* | false     | final      | finally   | float        |
| for      | function  | goto       | if        | implements   |
| import*  | in        | instanceof | int       | interface    |
| let      | long      | native     | new       | null         |
| package  | private   | protected  | public    | return       |
| short    | static    | super*     | switch    | synchronized |
| this     | throw     | throws     | transient | true         |
| try      | typeof    | var        | void      | volatile     |
| while    | with      | yield      |           |              |



#### JavaScript this关键字

<font color=FF0000>JavaScript 中 this 不是固定不变的，它会随着执行环境的改变而改变</font>。

- 在**对象方法**中，this 表示**该方法所属的对象**。
- 如果**单独使用**，this 表示**全局对象**（在严格模式下也是这样）。
- 在**函数**中，this 表示**全局对象**。
- 在**函数**中，在**严格模式**下，**this 是未定义的**(undefined)。
- 在**事件**中，this 表示**接收事件的元素**。
- 类似 call() 和 apply() 方法可以将 this 引用到任何对象。



#### JavaScript let

ES2015(ES6) 新增加了两个重要的 JavaScript 关键字: **let** 和 **const**。

- **let** 声明的变量只在 let 命令所在的代码块内有效。

- **const** 声明一个只读的常量，一旦声明，常量的值就不能改变。

而在 ES6 之前，JavaScript 只有两种作用域： **全局变量** 与 **函数内的局部变量**。需要注意的是：

```js
function myFunction() {
    var carName = "Volvo";   // 局部作用域
}
```

这里的carName<font color=FF0000>**确实是**局部变量</font>，且是**函数内的局部变量**

**全局变量**

在函数体外或代码块外使用 **var** 和 **let** 关键字声明的变量也有点类似。它们的作用域都是 **全局的**:

```js
var x = 2;       // 全局作用域
let x = 2;       // 全局作用域
```

**HTML 代码中使用全局变量**

在 JavaScript 中, 全局作用域是针对 JavaScript 环境。<font color=FF0000>在 HTML 中, 全局作用域是针对 window 对象</font>。

- 使用 **var** 关键字声明的全局作用域变量属于 window 对象

- 使用 **let** 关键字声明的全局作用域变量不属于 window 对象（如果调用就会是undefined）

**重置变量**

- 使用 **var** 关键字声明的变量在任何地方都可以修改：

  ```js
  var x = 2;  // x 为 2
  var x = 3;  // 现在 x 为 3
  ```

- 在相同的<font color=FF0000>作用域</font>或<font color=FF0000>块级作用域</font>中，<font color=FF0000>不能使用 **let** 关键字来重置 **var** 关键字声明的变量</font>：

  ```js
  var x = 2;       // 合法
  let x = 3;       // 不合法
  
  {
      var x = 4;   // 合法
      let x = 5   // 不合法
  }
  ```

- 在相同的作用域或块级作用域中，<font color=FF0000>不能使用 **var** 关键字来重置 **let** 关键字声明的变量</font>：

  ```js
  let x = 2;       // 合法
  var x = 3;       // 不合法
  
  {
      let x = 4;   // 合法
      var x = 5;   // 不合法
  }
  ```

- **let** 关键字在不同作用域，或不同块级作用域中是可以重新声明赋值的

  ```js
  let x = 2;       // 合法
  
  {
      let x = 3;   // 合法
  }
  
  {
      let x = 4;   // 合法
  }
  ```

**变量提升**

- var 关键字定义的变量可以在使用后声明，也就是变量可以先使用再声明
- let 关键字定义的变量则不可以在使用后声明，也就是变量需要先声明再使用。



#### JavaScript const

const 用于声明一个或多个常量，<font color=FF0000>**声明时必须进行初始化**</font>，且初始化后值不可再修改：

`const`定义常量与使用`let` 定义的变量<font color=FF0000>相似</font>：

- 二者都是块级作用域
- 都不能和它所在作用域内的其他变量或函数拥有相同的名称

两者还有以下两点<font color=FF0000>**区别**</font>：

- <font color=FF0000>`const`声明的常量必须初始化，而`let`声明的变量不用</font>
- <font color=FF0000>const 定义常量的值不能通过再赋值修改，也不能再次声明。而 let 定义的变量值可以修改</font>。

**const关键字没有变量提升**：const 关键字定义的变量则不可以在使用后声明，也就是变量需要先声明再使用。

**const并非真正的常量**：**const 的本质：** <font color=FF0000>const 定义的变量并非常量，并非不可变，它定义了一个常量引用一个值</font>。使用 const 定义的对象或者数组，其实是可变的。下面的代码并不会报错：

```js
// 创建常量对象
const car = {type:"Fiat", model:"500", color:"white"};
// 修改属性:
car.color = "red";
// 添加属性
car.owner = "Johnson";
// 显示属性
document.getElementById("demo").innerHTML = "Car owner is " + car.owner; 
```

但是我们不能对常量对象重新赋值：

```js
const car = {type:"Fiat", model:"500", color:"white"};
car = {type:"Volvo", model:"EX60", color:"red"};    // 错误
```

**重置变量**

- 使用 **var** 关键字声明的变量在任何地方都可以修改

- 在<font color=FF0000>相同的作用域或块级作用域</font>中，<font color=FF0000>不能使用</font> **const** 关键字来重置 **var**、**let**、**const**关键字声明的变量

- **const** 关键字在<font color=FF0000>不同作用域</font>，或<font color=FF0000>不同块级作用域</font>中是<font color=FF0000>可以重新声明赋值</font>的，示例：

  ```js
  const x = 2;       // 合法
  
  {
      const x = 3;   // 合法
  }
  
  {
      const x = 4;   // 合法
  }
  ```



#### JavaScript JSON

JSON（**J**ava**S**cript **O**bject **N**otation） 是用于存储和传输数据的格式。通常用于服务端向网页传递数据 。

**JSON 字符串转换为 JavaScript 对象**

通常我们从服务器中读取 JSON 数据，并在网页中显示数据。

简单起见，我们网页中直接设置 JSON 字符串：

- 首先，创建 JavaScript 字符串，字符串为 JSON 格式的数据：

  ```js
  var text = '{ "sites" : [' +
  '{ "name":"Runoob" , "url":"www.runoob.com" },' +
  '{ "name":"Google" , "url":"www.google.com" },' +
  '{ "name":"Taobao" , "url":"www.taobao.com" } ]}';
  ```

- 然后，<font color=FF0000>使用 JavaScript 内置函数 `JSON.parse()` 将字符串转换为 JavaScript 对象</font>

  ```js
  var obj = JSON.parse(text);
  ```

- 最后，在你的页面中使用新的 JavaScript 对象

**相关函数**

| 函数                                                         | 描述                                           |
| :----------------------------------------------------------- | :--------------------------------------------- |
| [JSON.parse()](https://www.runoob.com/js/javascript-json-parse.html) | 用于将一个 JSON 字符串转换为 JavaScript 对象。 |
| [JSON.stringify()](https://www.runoob.com/js/javascript-json-stringify.html) | 用于将 JavaScript 值转换为 JSON 字符串。       |



#### javascript:void(0) 含义

**javascript:void(0)** 中最关键的是 **void** 关键字， <font color=FF0000>**void**</font> 是 JavaScript 中非常重要的关键字，<font color=FF0000>该操作符**指定要计算一个表达式但是不返回值**</font>

语法格式如下：

```js
void func()
javascript:void func()
```

或者

```js
void(func())
javascript:void(func())
```

下面的代码创建了一个超级链接，当用户点击以后不会发生任何事。示例：

```html
<a href="javascript:void(0)">单击此处什么也不会发生</a>
```

以下实例中，在用户点击链接后显示警告信息：

```html
<p>点击以下链接查看结果：</p>
<a href="javascript:void(alert('Warning!!!'))">点我!</a>
```

**href="#"与href="javascript:void(0)"的区别**

- <font color=FF0000>**`#`** 包含了一个位置信息</font>，默认的锚是**`#top`** 也就是网页的上端。

  在页面很长的时候会使用 **#** 来定位页面的具体位置，格式为：**# + id**。

- javascript:void(0)，仅仅<font color=FF0000>表示一个死链接</font>。如果你要定义一个死链接请使用 javascript:void(0) 。



#### JavaScript 异步编程

异步（Asynchronous, async）是与同步（Synchronous, sync）相对的概念。

在我们学习的传统单线程编程中，程序的运行是同步的（同步不意味着所有步骤同时运行，而是指<font color=FF0000>步骤在一个控制流序列中**按顺序执行**</font>）。而<font color=FF0000>异步的概念则是不保证同步的概念</font>，也就是说，一个<font color=FF0000>异步过程的执行将不再与原有的序列有顺序关系。</font>

<mark>**简单来理解就是：同步按你的代码顺序执行，异步不按照代码顺序执行，异步的执行效果更高**</mark>

JavaScript 中的<font color=FF0000>异步操作函数往往通过**回调函数**来实现异步任务的结果处理</font>。

<font color=FF0000>回调函数</font>就是一个函数，它是在我们启动一个异步任务的时候就告诉它：等你完成了这个任务之后要干什么。这样一来主线程几乎不用关心异步任务的状态了，他自己会善始善终。

示例：

```js
setTimeout(function () {
    document.getElementById("demo").innerHTML="RUNOOB!";
}, 3000);
```

这个函数执行之后会产生一个子线程，子线程会等待 3 秒，然后执行回调函数 "print"，在命令行输出 "RUNOOB!"。



#### JavaScript Promise

Promise 是一个 ECMAScript 6 提供的类，目的是更加优雅地书写复杂的异步任务。

**构造 Promise**
现在我们新建一个 Promise 对象：

```js
new Promise(function (resolve, reject) {
    // code
});
```

Promise 构造函数只有一个参数，是一个函数（即上面的function），这个函数在构造之后会直接被异步运行，所以我们称之为<font color=FF0000>起始函数</font>。起始函数<font color=FF0000>包含两个参数 resolve 和 reject</font>。其中，<font color=FF0000>resolve 和 reject 都是函数</font>，其中<font color=FF0000>调用 resolve 代表一切正常</font>，<font color=FF0000>reject 是出现异常时所调用的</font>

Promise 类有 `.then()` ` .catch()` 和 `.finally()` 三个方法，<font color=FF0000>这三个方法的参数都是一个函数</font>

- **.then()** ：可以将参数中的函数<font color=FF0000>添加到当前 Promise 的正常执行序列</font>

- **.catch()** ：则是设定 Promise 的<font color=FF0000>异常处理序列</font>

- **.finally()** ：是在 <font color=FF0000>Promise 执行的最后一定会执行的序列</font>

示例：

```js
new Promise(function (resolve, reject) {
    var a = 0;
    var b = 1;
    if (b == 0) reject("Diveide zero");
    else resolve(a / b);
}).then(function (value) {
    console.log("a / b = " + value);
}).catch(function (err) {
    console.log(err);
}).finally(function () {
    console.log("End");
});
```

**异步函数**

异步函数（async function）是 <font color=FF0000>ECMAScript 2017</font> (ECMA-262) 标准的规范，几乎被所有浏览器所支持，除了 Internet Explorer。

示例：

```js
async function asyncFunc() {
    await print(1000, "First");
    await print(4000, "Second");
    await print(3000, "Third");
}
asyncFunc();
```

<font color=FF0000>异步函数 async function 中可以使用 await 指令，await 指令后必须跟着一个 Promise，异步函数会在这个 Promise 运行中暂停，直到其运行结束再继续运行</font>。

示例：

```js
async function asyncFunc() {
    let value = await new Promise(
        function (resolve, reject) {
            resolve("Return value");
        }
    );
    console.log(value);
}
asyncFunc();
```

//todo 这里没有完全看懂，需要以后再了解....



#### JavaScript 代码规范

代码规范通常包括以下几个方面:

- 变量和函数的命名规则
  - 变量名推荐使用驼峰法来命名**camelCase**
- 空格，缩进，注释的使用规则
  - 通常运算符 ( = + - * / ) 前后需要添加空格
  - 通常使用 4 个空格符号来缩进代码块
- 其他常用规范
  - 复杂语句的通用规则:
    - 将左花括号放在第一行的结尾。
    - <font color=FF0000>左花括号前添加一空格</font>
    - <font color=FF0000>将右花括号独立放在一行</font>
    - 不要以分号结束一个复杂的声明。
  - 对象定义的规则
    - 将左花括号与类名放在同一行。
    - 冒号与属性值间有个空格。
    - 字符串使用双引号，数字不需要。
    - 最后一个属性-值对后面不要添加逗号。
    - 将右花括号独立放在一行，并以分号作为结束符号。



#### JavaScript函数

语法：

```js
function functionName(parameters) {
  //code
}
```

函数声明后不会立即执行，会在我们需要的时候调用到。

**函数表达式**

JavaScript 函数可以通过一个表达式定义。函数表达式可以存储在变量中，示例：

```js
var x = function (a, b) {return a * b};
```

<font color=FF0000>在函数表达式存储在变量后，变量也可作为一个函数使用</font>，示例：

```js
var x = function (a, b) {return a * b};
var z = x(4, 3);
```

<font color=FF0000>这个函数实际上是一个 **匿名函数** (函数没有名称)。函数存储在变量中，不需要函数名称，通常通过变量名来调用</font>。

**Function() 构造函数**

函数同样可以通过内置的 JavaScript 函数构造器（Function()）定义。示例：

```js
var myFunction = new Function("a", "b", "return a * b");
var x = myFunction(4, 3);
```

不过没有必要，可以写成：

```js
var myFunction = function (a, b) {return a * b};
var x = myFunction(4, 3);
```

**函数提升（Hoisting）**

提升（Hoisting）是 JavaScript 默认<font color=FF0000>将当前作用域提升到前面去的的行为</font>。

函数可以在声明之前调用：

```js
myFunction(5);

function myFunction(y) {
    return y * y;
}
```

**自调用函数**

- **函数表达式可以 "自调用"。自调用表达式会自动调用。**

- <font color=FF0000>如果表达式后面紧跟 () ，则会自动调用。</font>

- <font color=FF0000>不能自调用声明的函数。</font>

通过添加括号，来说明它是一个函数表达式：

```js
(function () {
  var x = "Hello!!";   // 我将调用自己
})();
```

**函数是对象**

在 JavaScript 中使用 **typeof** 操作符判断函数类型将返回 "function" 。

但是JavaScript 函数描述为一个对象更加准确。<font color=FF0000>JavaScript 函数有 **属性** 和 **方法**</font>。<font color=FF0000>arguments.length 属性返回函数调用过程接收到的参数个数</font>：

```js
function myFunction(a, b) {
  return arguments.length;
}
```

toString() 方法将函数作为一个字符串返回:

```js
function myFunction(a, b) {
    return a * b;
}
var txt = myFunction.toString();
```

**箭头函数**

ES6 新增了箭头函数。箭头函数表达式的语法比普通函数表达式更简洁。

```js
(param1, param2, …, paramN) => { expression }

(param1, param2, …, paramN) => expression(单一)
//相当于：(param1, param2, …, paramN) =>{ return expression; }
```

当<font color=FF0000>只有一个参数</font>时，圆括号是可选的：

```js
(单一参数) => {expression}
单一参数 => {expression}
```

<font color=FF0000>没有参数</font>的函数应该写成一对圆括号:

```js
() => {expression}
```

示例：

```js
const x = (x, y) => x * y;
```

另外：箭头函数是不能提升的，所以需要在使用之前定义。



#### JavaScript 函数参数

JavaScript 函数对参数的值没有进行任何的检查。

**函数显式参数(Parameters)与隐式参数(Arguments)**

- 函数显式参数在函数定义时列出。
- 函数隐式参数在函数调用时传递给函数真正的值。

**参数规则**

- JavaScript 函数<font color=FF0000>定义显式参数时没有指定数据类型</font>。

- JavaScript 函数<font color=FF0000>对隐式参数没有进行类型检测</font>。

- JavaScript 函数<font color=FF0000>对隐式参数的个数没有进行检测</font>。

**默认参数**

- **ES5中：**<font color=FF0000>如果函数在调用时未提供隐式参数，参数会默认设置为： **undefined**</font>。有时这是可以接受的，但是建议最好为参数设置一个默认值，比如说0。

  示例：

  ```js
  //方法一
  function myFunction(x, y) {
      if (y === undefined) {
            y = 0;
      } 
  }
  
  //方法二，更简单的方式
  function myFunction(x, y) {
      y = y || 0;
  }
  ```

- **ES6中：** <font color=FF0000>支持函数带有默认参数</font>

  示例：

  ```js
  function myFunction(x, y = 10){
    //code
  }
  ```

**arguments 对象**

JavaScript 函数有个<font color=FF0000>内置的对象 arguments 对象</font>。<font color=FF0000>argument 对象包含了函数调用的参数数组</font>。

示例：

```js
x = findMax(1, 123, 500, 115, 44, 88);
 
function findMax() {
    var i, max = arguments[0];
    
    if(arguments.length < 2) return max;
 
    for (i = 0; i < arguments.length; i++) {
        if (arguments[i] > max) {
            max = arguments[i];
        }
    }
    return max;
}
```



### JavaScript 函数调用

<font color=FF0000>JavaScript 函数有 **4 种**调用方式</font>。每种方式的不同在于 **this** 的初始化。

- **作为一个函数调用**

  <font color=FF0000>函数不属于任何对象。但是在 JavaScript 中它始终是默认的全局对象</font>。而在 <mark>HTML 中默认的全局对象是 HTML 页面本身</mark>，所以函数是属于 HTML 页面。

  this对象：当<font color=FF0000>函数没有被自身的对象调用时 **this** 的值就会变成全局对象</font>。在 web 浏览器中全局对象是浏览器窗口（window 对象）。

- **函数作为方法调用**

  在 JavaScript 中你可以将函数定义为对象的方法。这里的**this**对象是当前方法所在的对象

- **使用构造函数调用函数**

  如果函数调用前使用了 **new** 关键字, 则是调用了构造函数。

  这看起来就像创建了新的函数，但实际上 <font color=FF0000>**JavaScript 函数是重新创建的对象**</font>。示例：

  ```js
  // 构造函数:
  function myFunction(arg1, arg2) {
      this.firstName = arg1;
      this.lastName  = arg2;
  }
   
  // This creates a new object
  var x = new myFunction("John","Doe");
  x.firstName;                             // 返回 "John"
  ```

- **作为函数方法调用函数**

  在 JavaScript 中, <font color=FF0000>函数是对象。JavaScript **函数有它的属性和方法**</font>。

  <font color=FF0000>**call()** 和 **apply()** 是**预定义**的函数方法</font>。 <font color=FF0000>两个方法可用于调用函数</font>，<font color=FF0000>两个方法的**第一个参数必须是对象本身**</font>。
  示例：

  ```js
  function myFunction(a, b) {
      return a * b;
  }
  myObject = myFunction.call(myObject, 10, 2);     // 返回 20
  ```

  两个方法都使用了对象本身作为第一个参数。 两者的区别在于第二个参数： 

  - **apply**<font color=FF0000>传入的是一个参数数组</font>，也就是将多个参数组合成为一个数组传入

  - **call**则作为call的参数传入（从第二个参数开始）。
  
  在 JavaScript <font color=FF0000>严格模式(strict mode)下</font>, 在<font color=FF0000>调用函数时第一个参数会成为 **this** 的值</font>， 即使该参数不是一个对象。
  
  在 JavaScript <font color=FF0000>非严格模式(non-strict mode)下</font>, <font color=FF0000>如果第一个参数的值是 null 或 undefined</font>, 它<font color=FF0000>将使用全局对象替代</font>。
  
  注意：通过 call() 或 apply() 方法你可以设置 **this** 的值, 且作为已存在对象的新方法调用。



#### JavaScript 闭包

私有变量可以用到闭包。

所有函数都能访问全局变量。 实际上，<font color=FF0000>在 JavaScript 中，**所有函数都能访问它们上一层的作用域**</font>。同时，<font color=FF0000>JavaScript 支持嵌套函数。嵌套函数可以访问上一层的函数变量</font>。

示例：

```js
function add() {
    var counter = 0;
    function plus() {counter += 1;}
    plus();    
    return counter; 
}
//调用
add(); // counter = 1
add(); // counter = 2
add(); // counter = 3
```

同时由于<font color=FF0000>函数可以自我调用</font>，所以闭包可以这样：

```js
var add = (function () {
    var counter = 0;
    return function () {return counter += 1;}
})();
 
add(); // counter = 1
add(); // counter = 2
add(); // counter = 3
```



#### JavaScript HTML DOM

通过 HTML DOM，可访问 JavaScript HTML 文档的所有元素。

网页被加载时，浏览器会创建页面的文档对象模型（DOM / Document Object Model）。

**查找 HTML 元素有三种方法**

- 通过 id 找到 HTML 元素

  ```js
  var x=document.getElementById("intro");
  ```

- 通过标签名找到 HTML 元素

  ```js
  var y=x.getElementsByTagName("p");
  ```

- 通过类名找到 HTML 元素

  ```js
  var x=document.getElementsByClassName("intro");
  ```

  

#### JavaScript HTML DOM - 改变 HTML

如需改变 HTML 元素的属性，请使用这个语法：

```js
document.getElementById(id).attribute=new-attribute
```



#### JavaScript HTML DOM - 改变CSS

如需改变 HTML 元素的样式，请使用这个语法：

```js
document.getElementById(id).style.property=new-style
```

**使用事件**

HTML DOM 允许我们通过触发事件来执行代码。比如以下事件：

- 元素被点击。
- 页面加载完成。
- 输入框被修改。
- ……

示例如下：

```html
<!--注意：这里onclick中的js代码被引号""包含-->
<button type="button" 
        onclick="document.getElementById('id1').style.color='red'">
  点我!</button>
```

与上面代码类似的：引号中的js代码可以被封装成一个函数，而在引号中只调用函数即可。

```html
<button onclick="displayDate()">点这里</button>
<script>
	function displayDate(){
		document.getElementById("demo").innerHTML=Date();
	}
</script>
```

同时，这里的onclick事件也可以被修改为：

- **onload 和 onunload 事件：**onload 和 onunload 事件会在用户进入或离开页面时被触发。<font color=FF0000>onload 和 onunload 事件可用于处理 cookie</font>。
- **onchange 事件：**<font color=FF0000>onchange 事件常结合**对输入字段的验证**来使用</font>。
- **onmouseover 和 onmouseout 事件：**onmouseover 和 onmouseout 事件可用于在用户的鼠标移至 HTML 元素上方或移出元素时触发函数。
- **onmousedown、onmouseup 以及 onclick 事件：**onmousedown, onmouseup 以及 onclick 构成了鼠标点击事件的所有部分。首先当点击鼠标按钮时，会触发 onmousedown 事件，当释放鼠标按钮时，会触发 onmouseup 事件，最后，当完成鼠标点击时，会触发 onclick 事件。



#### JavaScript HTML DOM EventListener

**addEventListener() 方法**

- addEventListener() 方法用于<font color=FF0000>向指定元素添加事件句柄</font>。
- addEventListener() 方法<font color=FF0000>添加的事件句柄**不会覆盖已存在的事件句柄**</font>。
- 你<font color=FF0000>可以**向一个元素添加多个事件句柄**</font>。
- 你可以<font color=FF0000>向同个元素添加多个同类型的事件句柄</font>，如：两个 "click" 事件。
- 你可以<font color=FF0000>向**任何 DOM 对象**添加事件监听</font>，不仅仅是 HTML 元素。如： window 对象。
- addEventListener() 方法可以更简单的控制事件（<font color=FF0000>冒泡与捕获</font>）。

**语法：**

```js
element.addEventListener(event, function, useCapture);
```

- **event：**<font color=FF0000>事件的类型</font> 
- **function：**<font color=FF0000>事件触发后调用的函数</font>。
- **useCapture：**是个布尔值<font color=FF0000>用于描述事件是冒泡还是捕获</font>。该参数是<font color=FF0000>可选</font>的。
  - 如果为 false（默认值），则在冒泡阶段设置处理程序。
  - 如果为 true，则在捕获阶段设置处理程序。



**removeEventListener()方法**

语法：

```js
element.removeElementListener(event, function);
```



**补充：事件传递有两种方式：冒泡与捕获**

事件冒泡和事件捕获分别由微软和网景公司提出，这两个概念都是为了解决页面中事件流（事件发生顺序）的问题。

考虑下面这段代码：

```html
<div id="outer">
    <p id="inner">Click me!</p>
</div>
```

上面的代码当中一个div元素当中有一个p子元素，<font color=FF0000>如果两个元素都有一个click的处理函数</font>，那么我们怎么才能知道哪一个函数会首先被触发呢？
为了解决这个问题微软和网景提出了两种几乎完全相反的概念。

- 事件冒泡

  微软提出了名为`事件冒泡(event bubbling)`的事件流。事件冒泡可以形象地比喻为把一颗石头投入水中，泡泡会一直从水底冒出水面。也就是说，事件会从最内层的元素开始发生，一直向上传播，直到document对象。

  因此在事件冒泡的概念下在p元素上发生click事件的顺序应该是**`p -> div -> body -> html -> document`**

- 事件捕获

  网景提出另一种事件流名为`事件捕获(event capturing)`。与事件冒泡相反，事件会从最外层开始发生，直到最具体的元素。

  因此在事件捕获的概念下在p元素上发生click事件的顺序应该是**`document -> html -> body -> div -> p`**

摘自：[JS中事件冒泡与捕获](https://segmentfault.com/a/1190000005654451) 具体更多可以参考：[Javascript info -- 冒泡和捕获](https://zh.javascript.info/bubbling-and-capturing)



#### JavaScript HTML DOM 元素 (节点)

- **appendChild(node)：**创建新的 HTML 元素 (节点)，用于<font color=FF0000>添加新元素到尾部</font>

  要创建新的 HTML 元素 (节点)需要先创建一个元素，然后在已存在的元素中添加它。

  示例：

  ```js
  //创建<p> 元素
  var para = document.createElement("p");
  //<p> 元素创建一个新的文本节点
  var node = document.createTextNode("这是一个新的段落。");
  //将文本节点添加到 <p> 元素中：
  para.appendChild(node);
   
  var element = document.getElementById("div1");
  element.appendChild(para);
  ```

- **insertBefore(node)：**将新元素添加到开始位置

- **removeChild(node)：**移除一个元素
- **replaceChild(node)：**替换一个元素



#### JavaScript HTML DOM 集合(Collection)

getElementsByTagName() 方法返回 [HTMLCollection](https://www.runoob.com/jsref/dom-htmlcollection.html) 对象。

```js
// 获取文档所有的 <p> 元素（哪怕是嵌套内层的）
var x = document.getElementsByTagName("p");
// 访问第二个 <p> 元素可以是以下代码:
y = x[1];
```

**另外，HTMLCollection 不是一个数组！**

HTMLCollection 看起来可能是一个数组，但其实不是。

你可以像数组一样，使用索引来获取元素。

所以，HTMLCollection 无法使用数组的方法： valueOf(), pop(), push(), 或 join() 。



#### JavaScript HTML DOM 节点列表

**NodeList** 对象是一个从文档中获取的<font color=FF0000>节点列表</font> (集合) 。

所有浏览器的 **childNodes** 属性返回的是 NodeList 对象。

<font color=FF0000>大部分浏览器的 **querySelectorAll()** 返回 NodeList 对象</font>。

**HTMLCollection 与 NodeList 的区别**

- [HTMLCollection](https://www.runoob.com/js/js-htmldom-collections.html) 是 HTML 元素的集合。NodeList 是一个文档节点的集合。
- 不同：
  - **HTMLCollection** 元素可以通过 name，id 或索引来获取。
  - **NodeList** <font color=FF0000>只能</font>通过索引来获取。
  - 只有 NodeList 对象有包含属性节点和文本节点。

- 类似：
  - NodeList 与 HTMLCollection 都与数组对象有点类似，可以使用索引 (0, 1, 2, 3, 4, ...) 来获取元素。
  - NodeList 与 HTMLCollection 都有 length 属性。

#### **补充：**document.querySelector()和document.querySelectorAll()

**document.querySelector()**：文档对象模型Document引用的querySelector()方法<font color=FF0000>返回文档中与指定选择器或选择器组匹配的**第一个 HTMLElement对象**</font>。 <font color=FF0000>如果找不到匹配项，则返回null</font>。( 匹配是使用<font color=FF0000>深度优先先序遍历</font>，从文档标记中的第一个元素开始，并按子节点的顺序依次遍历。)

**语法：**

```js
element = document.querySelector(selectors);
```

**参数：**selectors
包含一个或多个要匹配的选择器的 DOM字符串DOMString。 <font color=FF0000>该字符串必须是有效的CSS选择器字符串</font>；如果不是，则引发SYNTAX_ERR异常。

如果您需要与指定选择器匹配的所有元素的列表，则应该使用querySelectorAll() 。



#### JavaScript 对象

- **创建对象，语法如下：**

  ```js
  // 以构造函数形式来调用
  new Object([value])
  ```

  Object 构造函数，会根据给定的参数创建对象，具体有以下情况：

  - 如果给定值是 null 或 undefined，将会创建并返回一个空对象。
  - 如果传进去的是一个基本类型的值，则会构造其包装类型的对象。
  - 如果传进去的是引用类型的值，仍然会返回这个值，经他们复制的变量保有和源对象相同的引用地址。
  - 当以非构造函数形式被调用时，Object 的行为等同于 new Object()。

- **可以使用对象字面量来创建对象**，语法格式如下：

  ```js
  { name1 : value1, name2 : value2,...nameN : valueN }
  ```

  示例：

  ```js
  person={firstname:"John",lastname:"Doe",age:50,eyecolor:"blue"};
  ```

- **使用<font color=FF0000>对象构造器</font>**

  示例：

  ```js
  function person(firstname,lastname,age,eyecolor)
  {
      this.firstname=firstname;
      this.lastname=lastname;
      this.age=age;
      this.eyecolor=eyecolor;
  }
  ```

  一旦有了对象构造器，就可以创建新的对象实例，就像这样：

  ```js
  var myFather=new person("John","Doe",50,"blue");
  var myMother=new person("Sally","Rally",48,"green");
  ```

**JavaScript for...in 循环**

JavaScript for...in 语句循环遍历对象的属性。 for...in 循环中的代码块<font color=FF0000>将针对每个属性执行一次</font>。

**语法**

```js
for (variable in object){
	//code
}
```

示例：

```js
var person={fname:"John",lname:"Doe",age:25}; 
 
for (x in person){
    txt=txt + person[x];
}
```

**JavaScript 的对象是可变的**

对象是可变的，它们是通过<font color=FF0000>引用</font>来传递的。

以下实例的 person 对象不会创建副本：

```js
var x = person;  // 不会创建 person 的副本，是引用
```

<font color=FF0000>如果修改来 x ，person 的属性也会改变</font>，注意这里是引用，是浅拷贝

实例

```js
//x.age 和 person.age 都会改变
var person = {firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"}  var x = person; x.age = 10;           
```



#### JavaScript prototype（原型对象）

<font color=FF0000>所有的 JavaScript 对象都会从一个 prototype（原型对象）中**继承**属性和方法</font>。

- Date 对象从 Date.prototype 继承。
- Array 对象从 Array.prototype 继承。
- Person 对象从 Person.prototype 继承。

<font color=FF0000>**所有 JavaScript 中的对象都是位于原型链顶端的 Object 的实例。**</font>

<font color=FF0000>JavaScript 对象有一个指向一个原型对象的链。当试图访问一个对象的属性时，它不仅仅在该对象上搜寻，还会搜寻该对象的原型，以及该对象的原型的原型，依次层层向上搜索，直到找到一个名字匹配的属性或到达原型链的末尾</font>。

`Date` 对象，`Array` 对象， 以及 `Person` 对象从 `Object.prototype` 继承。

**添加属性和方法**

有的时候我们想要在所有已经存在的对象添加新的属性或方法。另外，有时候我们想要在对象的构造函数中添加属性或方法。

- 使用 prototype 属性就可以给对象的构造函数添加新的属性

- 当然我们也可以使用 prototype 属性就可以给对象的构造函数添加新的方法

示例：

```js
function Person(first, last, age, eyecolor) {  
  this.firstName = first;
  this.lastName = last;  
  this.age = age;  
  this.eyeColor = eyecolor; 
}  

//使用 prototype 属性就可以给对象的构造函数添加新的属性
Person.prototype.nationality = "English";

//使用 prototype 属性就可以给对象的构造函数添加新的方法
Person.prototype.name = function() {
  return this.firstName + " " + this.lastName;
};
```

#### Object.prototype.toString()

在 JavaScript 中, 想要判断某个对象值属于哪种内置类型, 最靠谱的做法就是通过 Object.prototype.toString 方法。示例如下：

```js
var arr = [];
console.log(Object.prototype.toString.call(arr))  //"[object Array]"
```





#### JavaScript Number 对象

**<font color=FF0000>所有 JavaScript 数字均为 64 位</font>**

JavaScript 不是类型语言。与许多其他编程语言不同，<mark>JavaScript 不定义不同类型的数字，比如整数、短、长、浮点等等</mark>。

在JavaScript中，<mark>数字不分为整数类型和浮点型类型，<font color=FF0000>**所有的数字都是由 浮点型类型**</font></mark>。JavaScript 采用 IEEE754 标准定义的 64 位浮点格式表示数字，它能表示最大值为 ±1.7976931348623157e+308，最小值为 ±5e-324。

| 值 (aka Fraction/Mantissa) |       指数        |    Sign    |
| :------------------------: | :---------------: | :--------: |
|      52 bits (0 - 51)      | 11 bits (50 - 62) | 1 bit (63) |

**精度**

- 整数（不使用小数点或指数计数法）最多为 15 位。
  - 超过15位的一律变成100,000,000,000,000,000（15个0）

- 小数的最大位数是 17

**八进制和十六进制**

要加上前缀`0`和`0x`

默认情况下，JavaScript 数字为十进制显示。但是你可以使用 toString() 方法 输出16进制、8进制、2进制。

示例：

```js
var myNumber=128;
myNumber.toString(16);  // 返回 80
myNumber.toString(8);  // 返回 200
myNumber.toString(2);  // 返回 10000000
```

**无穷大（Infinity）**

- <font color=FF0000>当数字运算结果超过了JavaScript所能表示的数字**上限**（溢出），结果为一个特殊的无穷大（infinity）值</font>，在JavaScript中以Infinity表示。
- 同样地，当负数的值超过了JavaScript所能表示的负数范围，结果为负无穷大，在JavaScript中以-Infinity表示。

**NaN - 非数字值**

NaN 属性是代表非数字值的特殊值。该属性用于指示某个值不是数字。可以把 Number 对象设置为该值，来指示其不是数字值。

你可以使用 <font color=FF0000>isNaN() 全局函数来判断一个值是否是 NaN 值</font>。

补充：无穷大是一个数字

**数字可以是数字或者对象**

示例如下：

```js
var x = 123;
var y = new Number(123);
typeof(x) // 返回 Number
typeof(y) // 返回 Object
```

**Number 属性**

| 属性                     | 描述                                                  |
| :----------------------- | :---------------------------------------------------- |
| Number.MAX_VALUE         | 最大值                                                |
| Number.MIN_VALUE         | 最小值                                                |
| Number.NaN               | 非数字                                                |
| Number.NEGATIVE_INFINITY | 负无穷，在溢出时返回                                  |
| Number.POSITIVE_INFINITY | 正无穷，在溢出时返回                                  |
| Number.EPSILON           | 表示 1 和比最接近 1 且大于 1 的最小 Number 之间的差别 |
| Number.MIN_SAFE_INTEGER  | 最小安全整数。                                        |
| Number.MAX_SAFE_INTEGER  | 最大安全整数。                                        |

**数字方法**

| 方法                   | 描述                                                         |
| :--------------------- | :----------------------------------------------------------- |
| Number.parseFloat()    | 将字符串转换成浮点数，和全局方法 [parseFloat()](https://www.runoob.com/jsref/jsref-parsefloat.html) 作用一致。 |
| Number.parseInt()      | 将字符串转换成整型数字，和全局方法 [parseInt()](https://www.runoob.com/jsref/jsref-parseint.html) 作用一致。 |
| Number.isFinite()      | 判断传递的参数是否为有限数字。                               |
| Number.isInteger()     | 判断传递的参数是否为整数。                                   |
| Number.isNaN()         | 判断传递的参数是否为 isNaN()。                               |
| Number.isSafeInteger() | 判断传递的参数是否为安全整数。                               |

**数字类型原型上的一些方法**

| 方法            | 描述                                                         |
| :-------------- | :----------------------------------------------------------- |
| toExponential() | 返回一个数字的指数形式的字符串，如：1.23e+2                  |
| toFixed()       | 返回指定小数位数的表示形式。<br>`var a=123; b=a.toFixed(2); // b="123.00"` |
| toPrecision()   | 返回一个指定精度的数字。如下例子中，a=123 中，3会由于精度限制消失：<br>`var a=123; b=a.toPrecision(2); // b="1.2e+2"` |



#### JavaScript Date（日期） 对象

Date 对象用于处理日期和时间。 可以通过 new 关键词来定义 Date 对象

<font color=FF0000>有四种方式初始化日期：</font>

- ```js
  new Date();
  ```

  示例：

  ```js
  var today = new Date()
  ```

- ```js
  new Date(value);
  ```

  示例：

  ```js
  var d2 = new Date(79,5,24)
  ```

- ```js
  new Date(dateString);
  ```

  示例：

  ```js
  var d1 = new Date("October 13, 1975 11:13:00")
  ```

- ```js
  new Date(year, monthIndex [, day [, hours [, minutes [, seconds [, milliseconds]]]]]);
  ```

  示例：

  ```js
  var d3 = new Date(79,5,24,11,33,0)
  ```

**设置日期**

通过使用针对日期对象的方法，我们可以很容易地对日期进行操作。

在下面的例子中，我们为日期对象设置了一个特定的日期 (2010 年 1 月 14 日)：

```js
var myDate=new Date();
myDate.setFullYear(2010,0,14);
```

在下面的例子中，我们将日期对象设置为 5 天后的日期：

```js
var myDate=new Date();
myDate.setDate(myDate.getDate()+5);
```

**注意:** <mark>如果增加天数会改变月份或者年份，那么日期对象会自动完成这种转换</mark>。





#### JavaScript Array（数组） 对象

创建一个数组，有三种方法。下面的代码定义了一个名为 myCars的数组对象：

- 常规方式：

  ```js
  var myCars=new Array();
  myCars[0]="Saab";   
  myCars[1]="Volvo";
  myCars[2]="BMW";
  ```

- 简洁方式:

  ```js
  var myCars=new Array("Saab","Volvo","BMW");
  ```

- 字面:

  ```js
  var myCars=["Saab","Volvo","BMW"];
  ```


补充：

- **Array.isArray()** ：用于确定传递的值是否是一个 Array。示例如下：

  ```js
  Array.isArray([1, 2, 3]);  // true
  Array.isArray({foo: 123});  // false
  Array.isArray("foobar");  // false
  Array.isArray(undefined);  // false
  ```

  - **语法：**Array.isArray(obj)

  - **参数：**obj 需要检测的值。

  - **返回值：**如果值是 Array，则为true; 否则为false。



#### JavaScript Math（算数） 对象

JavaScript 提供 8 种可被 Math 对象访问的算数值：

- Math.E
- Math.PI
- Math.SQRT2
- Math.SQRT1_2
- Math.LN2
- Math.LN10
- Math.LOG2E
- Math.LOG10E



#### JavaScript Window - 浏览器对象模型

浏览器对象模型 (BOM) 使 JavaScript 有能力与浏览器"对话"。

浏览器对象模型（**B**rowser **O**bject **M**odel (BOM)）尚无正式标准。但所有浏览器都支持 window 对象。它表示浏览器窗口。

由于现代浏览器已经（几乎）实现了 JavaScript 交互性方面的相同方法和属性，因此常被认为是 BOM 的方法和属性。

<font color=FF0000>所有 JavaScript 全局对象、函数以及变量均自动成为 window 对象的成员</font>。全局变量是 window 对象的属性，全局函数是 window 对象的方法；甚至 HTML DOM 的 document 也是 window 对象的属性之一

如下

```js
window.document.getElementById("header");
//等价于
document.getElementById("header");
```

**Window 尺寸**

有三种方法能够确定浏览器窗口的尺寸。

- 对于Internet Explorer、Chrome、Firefox、Opera 以及 Safari：
  - window.innerHeight - 浏览器窗口的内部高度(包括滚动条)
  - window.innerWidth - 浏览器窗口的内部宽度(包括滚动条)

- 对于 Internet Explorer 8、7、6、5：
  - document.documentElement.clientHeight
  - document.documentElement.clientWidth

- 或者
  - document.body.clientHeight
  - document.body.clientWidth

示例：

```js
// 计算浏览器窗口的高度和宽度
var width = window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth;
var height = window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight;
```

**其他一些 Window 方法**

- window.open() - 打开新窗口
- window.close() - 关闭当前窗口
- window.moveTo() - 移动当前窗口
- window.resizeTo() - 调整当前窗口的尺寸



#### JavaScript Window Screen

window.screen 对象包含有关用户屏幕的信息。<font color=FF0000>**window.screen**对象在编写时可以不使用 window 这个前缀</font>。

**一些属性：**

- **screen.availWidth** ： <font color=FF0000>可用</font>的屏幕宽度
- **screen.availHeight** ：<font color=FF0000>可用</font>的屏幕高度



#### JavaScript Window Location

window.location 对象用于<font color=FF0000>获得当前页面的地址 (URL)</font>，<font color=FF0000>并把浏览器重定向到新的页面</font>。<font color=FF0000>在编写时**可不使用 window 这个前缀**</font>。

**相关属性**

- **location.hostname** 	返回 web 主机的域名
- **location.pathname**    返回当前页面的路径和文件名
- **location.port**                返回 web 主机的端口 （80 或 443）
- **location.protocol**       返回所使用的 web 协议（http: 或 https:）
- **location.href**               返回当前页面的 URL
- **location.assign()**        方法加载新的文档。



#### JavaScript Window History

<font color=FF0000>window.history 对象包含浏览器的历史</font>。在编写时<font color=FF0000>可不使用 window 这个前缀</font>。

为了保护用户隐私，对 JavaScript 访问该对象的方法做出了限制。

**相关方法：**

- **history.back()：** 与在浏览器点击后退按钮相同
- **history.forward()：** 与在浏览器中点击向前按钮相同
- **history.go()**：载入到会话历史中的某一特定页面， 通过与当前页面相对位置来标志 (当前页面的相对位置标志为0)。

**补充：**

**HTML5** 引入了 <font color=FF0000>history.pushState()</font> 和 <font color=0000FF>history.replaceState()</font> 方法，它们分别可以<font color=FF0000>添加</font>和<font color=0000FF>修改</font>**历史记录条目**。这些方法通常与window.onpopstate 配合使用。

<mark>使用 history.pushState() 可以改变referrer，它在用户发送 XMLHttpRequest 请求时在HTTP头部使用，改变state后创建的 XMLHttpRequest 对象的referrer都会被改变</mark>。因为referrer是标识创建 XMLHttpRequest 对象时 this 所代表的window对象中document的URL。

摘自： [MDN - History API](https://developer.mozilla.org/zh-CN/docs/Web/API/History_API)



#### JavaScript Window Navigator

window.navigator 对象<font color=FF0000>包含有关访问者浏览器的信息</font>，在编写时可不使用 window 这个前缀

- **navigator.appCodeName：**    浏览器代号
- **navigator.appName：**             浏览器名称
- **navigator.appVersion：**          浏览器版本
- **navigator.cookieEnabled：**    启用Cookies
- **navigator.platform：**               硬件平台
- **navigator.userAgent：**             用户代理
- **navigator.systemLanguage：** 用户代理语言

**警告：**来自 navigator 对象的信息具有误导性，不应该被用于检测浏览器版本，这是因为：

- <font color=FF0000>navigator 数据可被浏览器使用者更改</font>
- <mark>一些浏览器对测试站点会识别错误</mark>
- <mark>浏览器无法报告晚于浏览器发布的新操作系统</mark>



#### JavaScript 计时事件

我们可以<font color=FF0000>在一个设定的时间间隔之后来执行代码</font>。我们称之为计时事件。

**两个关键方法：**

- **setInterval() ：** <font color=FF0000>间隔指定的毫秒数**不停地**执行指定的代码</font>。另外：**window.setInterval()** 方法可以不使用 window 前缀，直接使用函数 **setInterval()**。

  **语法**

  ```js
  window.setInterval(function, milliseconds);
  ```

  - 第一个参数是函数（function）

  - 第二个参数<font color=FF0000>间隔的**毫秒数**</font>

  **示例：**

  ```js
  // 每三秒弹出 "hello" 
  setInterval(function(){alert("Hello")},3000);
  ```

- **clearInterval()：** 用于停止 setInterval() 方法执行的函数代码

  语法：

  ```js
  window.clearInterval(intervalVariable)
  ```

  window.clearInterval()方法可以不使用window前缀，直接使用函数**clearInterval()**。

  <mark>要使用clearTimeout() 方法, 你<font color=FF0000>必须在创建超时方法中（setTimeout）使用全局变量</font></mark>

- **setTimeout()：** <font color=FF0000>在指定的毫秒数后执行指定代码</font>。

  **语法**

  ```js
  myVar= window.setTimeout(function, milliseconds);
  ```

- **clearTimeout()：** 方法用于<font color=FF0000>停止执行setTimeout()方法的函数代码</font>。

  **语法**

  ```js
  window.clearTimeout(timeoutVariable)
  ```

  类似clearInterval()，使用clearTimeout() 方法, 你必须在创建超时方法中（setTimeout）使用全局变量:



#### JavaScript Cookie

JavaScript 可以使用<font color=FF0000> **document.cookie** 属性来创建 、读取、及删除 cookie</font>。

**使用 JavaScript 创建Cookie**

- JavaScript 中，创建 cookie 如下所示：

  ```js
  document.cookie="username=John Doe";
  ```

- 您还<font color=FF0000>可以用expires参数，为 cookie 添加一个过期时间（以 UTC 或 GMT 时间）</font>。**<font color=FF0000>默认情况下，cookie 在浏览器关闭时删除</font>**：

  ```js
  document.cookie="username=John Doe; expires=Thu, 18 Dec 2043 12:00:00 GMT";
  ```

- 您可以<font color=FF0000>使用 path 参数</font><font color=FF0000>告诉浏览器 cookie 的路径</font>。默认情况下，cookie 属于当前页面。

  ```js
  document.cookie="username=John Doe; expires=Thu, 18 Dec 2043 12:00:00 GMT; path=/";
  ```

**使用 JavaScript 使用 Cookie**

- 在 JavaScript 中, 可以使用以下代码来读取 cookie：

  ```js
  var x = document.cookie;
  ```

  document.cookie 将以字符串的方式返回所有的 cookie，类型格式： cookie1=value; cookie2=value; cookie3=value;

- **使用 JavaScript 修改 Cookie**

  在 JavaScript 中，<font color=FF0000>修改 cookie 类似于创建 cookie（**即覆盖**）</font>，如下所示：

  ```js
  document.cookie="username=John Smith; expires=Thu, 18 Dec 2043 12:00:00 GMT; path=/";
  ```

- **使用 JavaScript 删除 Cookie**

  删除 cookie 非常简单。您只需要设置 expires 参数为以前的时间即可，如下所示

  ```js
  document.cookie = "username=; expires=Thu, 01 Jan 1970 00:00:00 GMT";
  ```

  <font color=FF0000>注意，当您删除时不必指定 cookie 的值。</font>



补充：[JavaScript 实例](https://www.runoob.com/js/js-examples.html)



#### 原型链

js以原型链的形式，保证函数或对象中的方法、属性可以让向下传递，按照面向对象的说法，这就是继承。而<font color=FF0000>js通过原型链才得以实现函数或对象的继承</font>。

//todo





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

GlobalEventHandlers mixin 的 onload 属性是一个事件处理程序用于处理Window, XMLHttpRequest, \<img> 等元素的加载事件，当资源已加载时被触发。

**语法**

```javascript
window.onload = funcRef;
```


当 window load事件触发时，funcRef 方法会被调用；funcRef 是窗口加载事件触发时调用的处理函数。

摘自：[MDN web docs - GlobalEventHandlers.onload](https://developer.mozilla.org/zh-CN/docs/Web/API/GlobalEventHandlers/onload)



#### Node.textContent

Node 接口的 **textContent** 属性表示一个节点及其后代的文本内容。用法：

```js
let text = someNode.textContent;
someOtherNode.textContent = string;
```

**textContent 的值取决于具体情况：**

- 如果<font color=FF0000>节点是一个 document</font>，<font color=FF0000>或者一个 DOCTYPE</font> ，则 textContent 返回 null。
- 如果节点是个 CDATA section、注释、processing instruction 或者 text node，textContent 返回节点内部的文本内容，例如 Node.nodeValue。
- 对于其他节点类型，textContent 将所有子节点的 textContent 合并后返回，除了注释和processing instructions。（如果该节点没有子节点的话，返回一个空字符串。）

**与 innerText 的区别：**

不要被 Node.textContent 和 HTMLElement.innerText 的区别搞混了。虽然名字看起来很相似，但有重要的不同之处：

- <font color=FF0000>textContent 会获取所有元素的内容，包括 \<script> 和 \<style> 元素</font>，然而<font color=0000FF> innerText 只展示给人看的元素</font>。
- <font color=FF0000>textContent会返回节点中的每一个元素</font>。相反，<font color=0000FF>innerText受 CSS 样式的影响，并且不会返回隐藏元素的文本</font>。
  - 此外，<font color=0000FF>由于 innerText 受 CSS 样式的影响，它会触发回流（ reflow ）去确保是最新的计算样式</font>。（回流在计算上可能会非常昂贵，因此应尽可能避免。）
- 与 textContent 不同的是, 在 Internet Explorer (小于和等于 11 的版本) 中对 innerText 进行修改， 不仅会移除当前元素的子节点，而且还会永久性地破坏所有后代文本节点。在之后不可能再次将节点再次插入到任何其他元素或同一元素中。

**与 innerHTML 的区别：**

正如其名称，<font style="color: fuchsia">Element.innerHTML 返回 HTML</font>。通常，为了在元素中检索或写入文本，人们使用 innerHTML。但是，textContent 通常具有更好的性能，因为文本不会被解析为HTML。

**此外，使用 textContent 可以防止 XSS 攻击。**

摘自：[MDN - Node.textContent](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/textContent)



#### JS forEach

**语法：**

```js
array1.forEach(element => do_something(element))
```



#### 列表操作

- **concat**：两个列表合并（有返回参数，为合并后的列表）

***



## JSON

JSON: **J**ava**S**cript **O**bject **N**otation(JavaScript 对象表示法)

JSON 独立于语言：JSON 使用 Javascript语法来描述数据对象，但是 JSON 仍然独立于语言和平台。JSON 解析器和 JSON 库支持许多不同的编程语言。 目前非常多的动态（PHP，JSP，.NET）编程语言都支持JSON。

**与 XML 相同之处**

- JSON 是纯文本
- JSON 具有"自我描述性"（人类可读）
- JSON 具有层级结构（值中存在值）
- JSON 可通过 JavaScript 进行解析
- JSON 数据可使用 AJAX 进行传输

**与 XML 不同之处**

- 没有结束标签
- 更短
- 读写的速度更快
- 能够使用内建的 JavaScript eval() 方法进行解析
- 使用数组
- 不使用保留字



#### JSON 语法

**JSON 语法规则**

JSON 语法是 JavaScript 对象表示语法的子集。

- 数据在名称/值对中
- 数据由逗号分隔
- 大括号保存对象
- 中括号保存数组

**JSON 名称/值对**

JSON 数据的书写格式是：名称/值对。名称/值对包括字段名称（在双引号中），后面写一个冒号，然后是值。

示例：

```json
"name" : "菜鸟教程"
```

**另外：JSON 文件**

- JSON 文件的文件类型是 ".json"
- JSON 文本的 MIME 类型是 "application/json"



#### JSON 值

JSON 值可以是：

- 数字（整数或浮点数）
- 字符串（在双引号中）
- 逻辑值（true 或 false）
- 数组（在中括号`[]`中）
- 对象（在大括号中）
- null



#### JSON 对象

示例：

```json
{ "name":"runoob", "alexa":10000, "site":null }
```

**访问对象值**

- 你可以使用点号<font color=FF0000 size=5>**`.`**</font>来访问对象的值：

  ```js
  var myObj = { "name":"runoob", "alexa":10000, "site":null };
  var x = myObj.name;
  ```

- 你也可以使用中括号<font color=FF0000 size=5>**`[]`**</font>来访问对象的值：

  ```js
  var myObj = { "name":"runoob", "alexa":10000, "site":null };
  var x = myObj["name"];
  ```

- 你可以使用 for-in 来循环对象的属性：

  **实例**

  ```js
  var myObj = { "name":"runoob", "alexa":10000, "site":null }; 
  for (x in myObj) {
    document.getElementById("demo").innerHTML += x + "<br>"; 
  }
  ```

- 循环对象

  - 可以使用 for-in 来循环对象的属性

    ```js
    var myObj = { "name":"runoob", "alexa":10000, "site":null };
    for (x in myObj) {
        document.getElementById("demo").innerHTML += x + "<br>";
    }
    //name alexa site
    ```

  - 在 for-in 循环对象的属性时，使用中括号（[]）来访问属性的值：

    ```js
    var myObj = { "name":"runoob", "alexa":10000, "site":null };
    for (x in myObj) {
        document.getElementById("demo").innerHTML += myObj[x] + "<br>";
    }
    ```

**嵌套 JSON 对象**

JSON 对象中可以包含另外一个 JSON 对象：

**实例**

```json
myObj = {    
  "name":"runoob",    
  "alexa":10000,    
  "sites": {        
    "site1":"www.runoob.com",        
    "site2":"m.runoob.com",        
    "site3":"c.runoob.com"    
  } 
}
```

你可以使用点号<font color=FF0000 size=5>**`.`**</font>或者中括号<font color=FF0000 size=5>**`[]`**</font>来访问嵌套的 JSON 对象。

```js
x = myObj.sites.site1;
// 或者
x = myObj.sites["site1"];
```

**修改值**

- 你可以使用点号<font color=FF0000 size=5>**`.`**</font>来修改 JSON 对象的值

  ```js
  myObj.sites.site1 = "www.google.com";
  ```

- 你可以使用中括号<font color=FF0000 size=5>**`[]`**</font>来修改 JSON 对象的值：

  ```js
  myObj.sites["site1"] = "www.google.com";
  ```

**删除对象属性**

我们可以使用 **delete** 关键字来删除 JSON 对象的属性

- 使用点号<font color=FF0000 size=5>**`.`**</font>

  ```js
  delete myObj.sites.site1;
  ```

- 使用中括号<font color=FF0000 size=5>**`[]`**</font>

  ```js
  delete myObj.sites["site1"]
  ```



#### JSON.parse()

JSON 通常用于与服务端交换数据。在接收服务器数据时一般是字符串。

**<font color=FF0000>我们可以使用 JSON.parse() 方法将数据转换为 JavaScript 对象。</font>**

**语法**

```js
JSON.parse(text[, reviver])
```

**参数说明：**

- **text：**<font color=FF0000>必需</font>， 一个有效的<font color=FF0000> JSON</font> 字符串。
- **reviver：** <font color=FF0000>可选</font>，一个<font color=FF0000>转换结果的函数</font>， <font color=FF0000>将为对象的每个成员调用此函数</font>。

**解析“函数”（解析字符串形式的函数）**

<font color=FF0000>JSON 不允许包含函数，但你可以将函数作为字符串存储，之后再将字符串转换为函数</font>。

示例：

```js
var text = '{ "name":"Runoob", "alexa":"function () {return 10000;}", "site":"www.runoob.com"}'; 
var obj = JSON.parse(text);
obj.alexa = eval("(" + obj.alexa + ")");  

document.getElementById("demo").innerHTML = obj.name + " Alexa 排名：" + obj.alexa();
```



#### JSON.stringify()

JSON 通常用于与服务端交换数据。在向服务器发送数据时一般是字符串。

我们<font color=FF0000>可以使用 JSON.stringify() 方法将 JavaScript 对象转换为字符串</font>。

**语法**

```js
JSON.stringify(value[, replacer[, space]])
```

**参数说明：**

- **value：**<font color=FF0000>必需</font>， 要转换的 JavaScript 值（<font color=FF0000>通常为对象或数组</font>）。

- **replacer：**<font color=FF0000>可选</font>。用于转换结果的函数或数组。

  如果 replacer 为<font color=FF0000>函数</font>，则 JSON.stringify 将调用该函数，并传入每个成员的键和值。使用返回值而不是原始值。如果此函数返回 undefined，则排除成员。根对象的键是一个空字符串：""。

  如果 replacer 是一个<font color=FF0000>数组</font>，则仅转换该数组中具有键值的成员。成员的转换顺序与键在数组中的顺序一样。当 value 参数也为数组时，将忽略 replacer 数组。

- **space：**<font color=FF0000>可选</font>，文本添加缩进、空格和换行符，如果 space 是一个数字，则返回值文本在每个级别缩进指定数目的空格，如果 space 大于 10，则文本缩进 10 个空格。space 也可以使用非数字，如：\t。

**解析<font color=FF0000>“函数”</font>**

JSON 不允许包含函数，JSON.stringify() 会删除 JavaScript 对象的函数，包括 key 和 value。

我们可以<font color=FF0000>在执行 JSON.stringify() 函数前将函数转换为字符串</font>来避免以上问题的发生：

**示例：**

```js
var obj = { "name":"Runoob", "alexa":function () {return 10000;}, "site":"www.runoob.com"}; 
obj.alexa = obj.alexa.toString(); 
var myJSON = JSON.stringify(obj);  

document.getElementById("demo").innerHTML = myJSON;
```



#### JSON 使用

**把 JSON 文本转换为 JavaScript 对象**

JSON<font color=FF0000> 最常见的用法</font>之一，是<font color=FF0000>从 web 服务器上读取 JSON 数据（作为文件或作为 HttpRequest）</font>，将 JSON 数据转换为 JavaScript 对象，然后在网页中使用该数据。

由于 JSON 语法是 JavaScript 语法的子集，<font color=FF0000>JavaScript 函数 eval() 可用于将 JSON 文本转换为 JavaScript 对象</font>。

eval() 函数使用的是 JavaScript 编译器，可解析 JSON 文本，然后生成 JavaScript 对象。<font color=FF0000>必须把文本包围在括号中，这样才能避免语法错误</font>，示例：

```js
var obj = eval ("(" + txt + ")");
```



#### JSONP 教程

<font color=FF0000>JSONP (JSON with Padding)</font> 是 <font color=FF0000>json 的一种"使用模式"</font>，<font color=FF0000>可以让网页从**别的域名**（网站）那获取资料，即跨域读取数据。</font>

为什么我们从不同的域（网站）访问数据需要一个特殊的技术( JSONP )呢？这是因为同源策略。

同源策略，它是由 Netscape 提出的一个著名的安全策略，现在所有支持 JavaScript 的浏览器都会使用这个策略。







***

## AJAX

AJAX（Asynchronous JavaScript and XML <font color=FF0000>异步的 JavaScript 和 XML</font>）是一种使用现有标准的新方法，用于创建快速动态网页的技术。

AJAX 最大的优点是在<font color=FF0000>不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容</font>。即：<font color=FF0000>通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新</font>；而传统的网页（不使用 AJAX）如果需要更新内容，必需重载整个网页面。

**AJAX 工作原理**

<img src="https://i.loli.net/2020/08/28/zmGg7KVBkWH5Zj2.png" style="zoom:85%;" />

AJAX是基于现有的Internet标准，并且联合使用它们：

- **XMLHttpRequest对象** (异步的与服务器交换数据)
- **JavaScript / DOM** (信息显示/交互)
- **CSS** (给数据定义样式)
- **XML** (作为转换数据的格式)



#### AJAX - XMLHttpRequest 对象

**XMLHttpRequest 对象**：所有现代浏览器（IE7+、Firefox、Chrome、Safari 以及 Opera）均支持 XMLHttpRequest 对象（IE5 和 IE6 使用 ActiveXObject）。<font color=FF0000>XMLHttpRequest 用于在后台与服务器交换数据</font>。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

**创建 XMLHttpRequest 对象**

**语法：**

```js
variable=new XMLHttpRequest();
```

老版本的 Internet Explorer （IE5 和 IE6）使用 ActiveX 对象：

```js
variable=new ActiveXObject("Microsoft.XMLHTTP");
```

为了应对所有的现代浏览器，包括 IE5 和 IE6，请<font color=FF0000>检查浏览器是否支持 XMLHttpRequest 对象</font>。如果支持，则创建 XMLHttpRequest 对象。如果不支持，则创建 ActiveXObject。代码示例如下：

```js
var xmlhttp;
if (window.XMLHttpRequest) {    
  //  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
  xmlhttp=new XMLHttpRequest();
} else {
  // IE6, IE5 浏览器执行代码
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP"); 
}
```



#### AJAX - 向服务器发送请求请求

如需将请求发送到服务器，<font color=FF0000>使用 XMLHttpRequest 对象的 open() 和 send() 方法</font>：

- **open(method, url, async)**：规定<font color=FF0000>请求的类型</font>、<font color=FF0000>URL</font> 以及<font color=FF0000>是否异步处理请求</font>。
  - **method**：请求的类型；GET 或 POST
  - **url**：文件在服务器上的位置
  - **async**：<font color=FF0000>true（异步）</font>或 <font color=FF0000>false（同步）</font>
- **send(string)**：将请求发送到服务器。
  - **string**：<font color=FF0000>仅用于 POST 请求</font>

**GET 还是 POST？**

<font color=FF0000>与 POST 相比，GET 更简单也更快</font>，并且在大部分情况下都能用。

然而，<font color=FF0000>在以下情况中，请使用 POST 请求</font>：

- 无法使用缓存文件（更新服务器上的文件或数据库）
- **向服务器发送大量数据**（<font color=FF0000>POST 没有数据量限制</font>）
- <font color=FF0000>**发送包含未知字符的用户输入时**</font>，POST 比 GET 更稳定也更可靠

**使用GET请求**

通过 GET 方法发送信息，请向 URL 添加信息，可以使用？，示例：

```js
xmlhttp.open("GET","/try/ajax/demo_get2.php?fname=Henry&lname=Ford",true); 
xmlhttp.send();
```

**使用POST请求**

如果<font color=FF0000>需要像 HTML 表单那样 POST 数据，请**使用 setRequestHeader() 来添加 HTTP 头**</font>。然后在 send() 方法中规定您希望发送的数据。示例如下：

```js
xmlhttp.open("POST","/try/ajax/demo_post2.php",true); 
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded"); 
xmlhttp.send("fname=Henry&lname=Ford");
```

setRequestHeader(header,value)：向请求添加 HTTP 头

- header: 规定头的名称
- value: 规定头的值

**异步 - True 或 False？**

XMLHttpRequest 对象如果要用于 AJAX 的话，其 open() 方法的 async 参数必须设置为 true：

```js
xmlhttp.open("GET","ajax_test.html",true);
```

<mark>对于 web 开发人员来说，发送异步请求是一个巨大的进步。很多在服务器执行的任务都相当费时。AJAX 出现之前，这可能会引起应用程序挂起或停止</mark>。**<font color=FF0000>通过 AJAX，JavaScript 无需等待服务器的响应</font>**，而是：

- 在等待服务器响应时执行其他脚本
- 当响应就绪后对响应进行处理

<mark><font color=FF0000>不推荐使用</font> async=false，但是对于一些小型的请求，也是可以的。</mark>



#### AJAX - 服务器 响应

如<font color=FF0000>需获得来自服务器的响应</font>，<font color=FF0000>请使用 XMLHttpRequest 对象的 responseText 或 responseXML 属性</font>。

- **response<font color=FF0000>Text</font>**：获得<font color=FF0000>字符串</font>形式的响应数据。

  如果来自服务器的响应<font color=FF0000>并非 XML</font>，请<font color=FF0000>使用 responseText </font>属性。

  responseText 属性返回字符串形式的响应，示例：

  ```js
  document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
  ```

- **response<font color=FF0000>XML</font>**：获得 <font color=FF0000>XML</font> 形式的响应数据。

  如果来自服务器的响应是 XML，而且需要作为 XML 对象进行解析，请使用 responseXML 属性



#### AJAX - onreadystatechange 事件

当请求被发送到服务器时，我们需要执行一些基于响应的任务。

每当 readyState 改变时（readyState 属性存有 XMLHttpRequest 的状态信息），就会触发 onreadystatechange 事件。

**XMLHttpRequest 对象的三个重要的属性：**

- **onreadystatechange**：存储函数（或函数名），<font color=FF0000>**每当 readyState 属性改变时，就会调用该函数**</font>。另外，<font color=FF0000>onreadystatechange 事件被触发 4 次（0 - 4）, 分别是： 0-1、1-2、2-3、3-4，对应着 readyState 的每个变化。</font>
- **readyState：**<font color=FF0000>**存有 XMLHttpRequest 的状态**</font>。从 0 到 4 发生变化。
  - 0: 请求未初始化
  - 1: 服务器连接已建立
  - 2: 请求已接收
  - 3: 请求处理中
  - 4: 请求已完成，且响应已就绪
- **status：**HTTP状态码
  - 200: "OK"
  - 404: 未找到页面

总结：[AJAX 实例](https://www.runoob.com/ajax/ajax-examples.html)，其中包含许多场景的实例，对于实际使用有帮助。

***



## ECMAScript

ES6及更高版本不能兼容所有浏览器，而ES5**相对来说**可以很好的兼容这些浏览器；我们在编写时，写ES6+，而使用Babel将ES6+的代码转变为ES5。同时，Babel会被配置在Webpack这样的构建工具当中，通过在Webpack中配置Babel就可以很方便的将ES6+的语法打包成ES5的语法。



#### NRM

由于npm速度并不快，所以可以考虑使用nrm以替代；安装命令：

```bash
npm install -g nrm
```

**NRM常用命令**

- 查看可选源: `nrm Is`
- 测试源响应时间: `nrm test taobao`
- 切换源: `nrm use taobao`
- 增加定制源: `nrm add imooc http://192.168.1.100:6666`

- 删除源: `nrm del imooc`



#### 构建imooc-es-cli的开发环境

（imooc-es-cli是讲师配置好的脚手架，初学就先使用讲师配置的脚手架）

- 初始化
  - 进入到要初始化项目的文件夹
  - 初始化命令：imooc-es-cli init
    - 需要输入新建项目的项目名称

- 进入项目：cd es-demo
- 安装依赖包：npm install
- 启动项目：npm run start



#### 文件夹与相关文件的作用

- build文件夹下是webpack的配置文件

- src文件夹下是源码，会被webpack打包编译

- static文件夹下的文件并不会被webpack打包和编译，会被直接复制到生产环境中，所以：需要编译的代码不要放在static文件夹下。

- package-lock.json和package.json文件是依赖的包管理的配置文件



#### ES6





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

其中某些框架也使用 $ 符号作为简写（就像 jQuery），如果您在用的两种不同的框架正在使用相同的简写符号，有可能导致脚本停止运行。<mark>jQuery 的团队考虑到了这个问题，并实现了 noConflict() 方法</mark>。示例如下：

```js
$.noConflict();
jQuery(document).ready(function(){
  jQuery("button").click(function(){
    jQuery("p").text("jQuery 仍然在工作!");
  });
});
```

如果你的 jQuery 代码块使用 $ 简写，并且您不愿意改变这个快捷方式，那么您可以把 $ 符号作为变量传递给 ready 方法。这样就可以在函数内使用 $ 符号了 - 而在函数外，依旧不得不使用 "jQuery"

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

| 名称                         | 值/描述                                                      |
| :--------------------------- | :----------------------------------------------------------- |
| async                        | 布尔值，表示请求是否异步处理。默认是 true。                  |
| beforeSend(*xhr*)            | 发送请求前运行的函数。                                       |
| cache                        | 布尔值，表示浏览器是否缓存被请求页面。默认是 true。          |
| complete(*xhr,status*)       | 请求完成时运行的函数（在请求成功或失败之后均调用，即在 success 和 error 函数之后）。 |
| contentType                  | 发送数据到服务器时所使用的内容类型。默认是："application/x-www-form-urlencoded"。 |
| context                      | 为所有 AJAX 相关的回调函数规定 "this" 值。                   |
| data                         | 规定要发送到服务器的数据。                                   |
| dataFilter(*data*,*type*)    | 用于处理 XMLHttpRequest 原始响应数据的函数。                 |
| dataType                     | 预期的服务器响应的数据类型。                                 |
| error(*xhr,status,error*)    | 如果请求失败要运行的函数。                                   |
| global                       | 布尔值，规定是否为请求触发全局 AJAX 事件处理程序。默认是 true。 |
| ifModified                   | 布尔值，规定是否仅在最后一次请求以来响应发生改变时才请求成功。默认是 false。 |
| jsonp                        | 在一个 jsonp 中重写回调函数的字符串。                        |
| jsonpCallback                | 在一个 jsonp 中规定回调函数的名称。                          |
| password                     | 规定在 HTTP 访问认证请求中使用的密码。                       |
| processData                  | 布尔值，规定通过请求发送的数据是否转换为查询字符串。默认是 true。 |
| scriptCharset                | 规定请求的字符集。                                           |
| success(*result,status,xhr*) | 当请求成功时运行的函数。                                     |
| timeout                      | 设置本地的请求超时时间（以毫秒计）。                         |
| traditional                  | 布尔值，规定是否使用参数序列化的传统样式。                   |
| type                         | 规定请求的类型（GET 或 POST）。                              |
| url                          | 规定发送请求的 URL。默认是当前页面。                         |
| username                     | 规定在 HTTP 访问认证请求中使用的用户名。                     |
| xhr                          | 用于创建 XMLHttpRequest 对象的函数。                         |

摘自：[jQuery ajax() 方法](https://www.runoob.com/jquery/ajax-ajax.html)



//todo

`.toggleClass()`

`.data()`







#### Collapse：折叠插件

## //todo