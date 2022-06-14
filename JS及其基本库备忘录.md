## JavaScript

#### JavaScript基础

DOM ( **D**ocument **O**bject **M**odel )（文档对象模型）是用于访问 HTML 元素的正式 W3C 标准。

##### 直接写入 HTML 输出流

```js
document.write("<h1>这是一个标题</h1>");
```

##### 改变 HTML 内容

```js
x = document.getElementById("demo"); // 查找元素
x.innerHTML = "Hello JavaScript";    // 改变内容
```

##### 改变 HTML 图像

```js
element = document.getElementById('myimage');
element.src = "/images/pic_bulboff.gif";
```

##### 改变 HTML 样式

```js
x=document.getElementById("demo")  //找到元素 
x.style.color = "#ff0000";           //改变样式
```

##### 设置 HTML 样式

```js
element.setAttribute(attributeName, attributeValue);
```

##### outerHTML 的补充

除了 innerHTML 外，还有 outerHTML ：element  DOM 接口的 outerHTML 属性获取描述元素（包括其后代）的序列化 HTML 片段。它也可以设置为用从给定字符串解析的节点替换元素。

**语法**

```js
let content = element.outerHTML;
```

返回时，内容包含描述元素及其后代的序列化 HTML 片段

```js
element.outerHTML = content;
```

将元素替换为通过使用元素的父作为片段解析算法的上下文节点解析字符串内容生成的节点

摘自：[MDN - element.outerHTML](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/outerHTML)



#### JavaScript 变量

##### JS 命名规则

- 变量<font color=FF0000>必须以字母开头</font>
- 变量也能以 $ 和 _ 符号开头（不过我们不推荐这么做）
- 变量名称<font color=FF0000>对大小写敏感</font>（ y 和 Y 是不同的变量）

#### JavaScript 数据类型

- **<font color=FF0000>值类型(基本类型)</font>**：字符串 ( String )、数字(Number)、布尔(Boolean)、对空(Null)、未定义 ( Undefined )、Symbol ( Symbol 是 ES6 引入了一种新的原始数据类型，表示独一无二的值）

- **<font color=FF0000>引用数据类型</font>**：对象 ( Object )、数组 ( Array )、函数 ( Function ) 

- 对象数据类型：Object、Date、Array

- 2 个不包含任何值的数据类型：null、undefined

##### Undefined 和 Null

Undefined 这个值表示变量不含有值。

可以通过将变量的值设置为 null 来清空变量。

##### 补充

`var foo = 10` 只是定义了一个变量，可以通过 window.foo 访问；但它无法被 delete 删除掉（因为它不是 window 的属性，能打印出来似乎是 JS 设计的问题）

不加上 var / let / const 的 `bar = 20`，则是在 window 对象下加了一个bar属性（可以打印出 window.bar ），且可以被 delete 删除掉（ `delete bar` 返回 true ）

学习自：[再学JavaScript ES(6-11)全版本语法大全](https://coding.imooc.com/class/444.html)



#### JavaScript 事件

##### 一些常见的HTML事件的列表

| 事件        | 描述                           |
| :---------- | :----------------------------- |
| onchange    | HTML 元素改变                  |
| onclick     | 用户点击 HTML 元素             |
| onmouseover | 用户在一个 HTML 元素上移动鼠标 |
| onmouseout  | 用户从一个 HTML 元素上移开鼠标 |
| onkeydown   | 用户按下键盘按键               |
| onload      | 浏览器已完成页面的加载         |

更多事件列表：[JavaScript 参考手册 - HTML DOM 事件](https://www.runoob.com/jsref/dom-obj-event.html)。

##### input 输入事件

onfocus ( focus ) -> 键盘输入 -> onkeydown ( keydown )  -> onkeypress ( keypress )  -> onkeyup ( keyup )  -> oninput ( input )  -> 失去焦点 -> onchange ( change ) -> onblur ( blur )

摘自：[input输入框事件](https://www.jianshu.com/p/4517117abd8e)

#### JavaScript 字符串

**字符串中可以使用转义字符转义的特殊字符：**

| 代码  | 输出       |
|:---:|:--------:|
| \'  | 单引号      |
| \"  | 双引号      |
| \\  | 反斜杠      |
| \n  | 换行       |
| \r  | 回车       |
| \t  | tab(制表符) |
| \b  | 退格符      |
| \f  | 换页符      |

<font color=FF0000>**字符串属性**</font>

| 属性                                   | 描述                                              |
|:------------------------------------:|:-----------------------------------------------:|
| constructor                          | 返回创建<font color=FF0000>字符串</font>属性的函数（下面有更多解释） |
| <font color=FF0000>**length**</font> | **返回字符串的长度**                                    |
| **prototype**                        | 允许您向对象添加属性和方法                                   |

**字符串方法**

| 方法                                        | 描述                                                         |
| :------------------------------------------ | :----------------------------------------------------------- |
| charAt()                                    | 返回指定索引位置的字符                                       |
| charCodeAt()                                | 返回指定索引位置字符的 Unicode 值                            |
| <font color=FF0000>**concat()**</font>      | 连接（合并）两个或多个字符串，返回连接后的字符串             |
| fromCharCode()                              | 将 Unicode 转换为字符串                                      |
| <font color=FF0000>indexOf()</font>         | 返回字符串中检索指定字符第一次出现的位置                     |
| <font color=FF0000>**lastIndexOf()**</font> | 返回字符串中检索指定字符最后一次出现的位置                   |
| localeCompare()                             | 用本地特定的顺序来比较两个字符串                             |
| <font color=FF0000>match()</font>           | 找到一个或多个正则表达式的匹配                               |
| <font color=FF0000>replace()</font>         | 替换与正则表达式匹配的子串                                   |
| <font color=FF0000>search()</font>          | 检索与正则表达式相匹配的值                                   |
| slice()                                     | 提取字符串的片断，并在新的字符串中返回被提取的部分           |
| split()                                     | 把字符串分割为子字符串数组                                   |
| substr()                                    | 从起始索引号提取字符串中指定数目的字符                       |
| substring()                                 | 提取字符串中两个指定的索引号之间的字符                       |
| toLocaleLowerCase()                         | 根据主机的语言环境把字符串转换为小写，只有几种语言（如土耳其语）具有地方特有的大小写映射 |
| toLocaleUpperCase()                         | 根据主机的语言环境把字符串转换为大写，只有几种语言（如土耳其语）具有地方特有的大小写映射 |
| toLowerCase()                               | 把字符串转换为小写                                           |
| toString()                                  | 返回字符串对象值                                             |
| toUpperCase()                               | 把字符串转换为大写                                           |
| trim()                                      | 移除字符串首尾空白                                           |
| valueOf()                                   | 返回某个字符串对象的原始值                                   |

更多方法实例可以参见：[JavaScript String 对象](https://www.runoob.com/jsref/jsref-obj-string.html)。



#### 模版字符串

##### 标签函数

标签函数的语法是函数名后面直接带一个模板字符串，并从模板字符串中的插值表达式中获取参数，举个例子。

定义一个 greet 函数接收三个参数。

```js
function greet(arg1, arg2, arg3){
  console.log(arg1);
  console.log(arg2);
  console.log(arg3);
}
```

下面两句代码等价

```js
// 普通函数
greet(["I'm ", ". I'm ", " years old."], name, age)

// tag 函数
greet`I'm ${name}. I'm ${age} years old.`、

// 最终输出
[ 'I\'m ', '. I\'m ', ' years old.' ]
Alfred
47
```

标签函数的第一个参数是被嵌入表达式分隔的文本的数组。第二个及后面的参数是嵌入表达式的内容。

摘自：[ES6 - 标签函数](https://zhuanlan.zhihu.com/p/31687266)

#### String.raw()

在标签函数的第一个参数中，存在一个特殊的属性raw ，我们可以通过它来访问模板字符串的原始字符串，而不经过特殊字符的替换。

另外，使用String.raw() 方法创建原始字符串和使用默认模板函数和字符串连接创建是一样的。

摘自：[MDN - 模板字符串](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Template_literals)

String.raw() 是一个模板字符串的标签函数，<font color=FF0000>是用来获取一个模板字符串的原始字符串的</font>，比如说：占位符（例如 ${foo}）会被处理为它所代表的其他字符串，而转义字符（例如 \n）不会。

```js
String.raw(callSite, ...substitutions)
String.raw`templateString`
```

##### 参数

- **callSite：**一个模板字符串的“调用点对象”。类似{ raw: ['foo', 'bar', 'baz'] }。
- **...substitutions：**任意个可选的参数，表示任意个内插表达式对应的值。
- **templateString：**模板字符串，可包含占位符（${...}）。

**返回值：**给定模板字符串的原始字符串。

摘自：[MDN - String.raw()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/raw)



#### String.prototype.repeat()

##### 语法

```js
str.repeat(count)
```

##### 参数

count：介于 0 和 +Infinity 之间的整数。表示在新构造的字符串中重复了多少遍原字符串。

**返回值：**包含指定字符串的指定数量副本的新字符串。

##### 异常

- RangeError: 重复次数不能为负数。
- RangeError: 重复次数必须小于 infinity，且长度不会大于最长的字符串。

摘自：[MDN - String.prototype.repeat()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/repeat)



#### String.prototype.startsWith()

startsWith() 方法用来判断当前字符串是否以另外一个给定的子字符串开头，并根据判断结果返回 true 或 false。

##### 语法

```js
str.startsWith(searchString[, position])
```

##### 参数

- **searchString：**要搜索的子字符串。
- **position：**<font color=FF0000>可选</font>，在 str 中搜索 searchString 的开始位置，<font color=FF0000>默认值为 0</font>。

**返回值：**如果在字符串的开头找到了给定的字符则返回true；否则返回false。

**描述：**这个方法能够让你确定一个字符串是否以另一个字符串开头。<font color=FF0000>这个方法区分大小写</font>。

摘自：[MDN - tring.prototype.startsWith()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/startsWith)

##### 补充

和正则中的 `^` 符号一致

#### String.prototype.endsWith()

endsWith() 方法用来判断当前字符串是否是以另外一个给定的子字符串“结尾”的，根据判断结果返回 true 或 false。

##### 语法

```js
str.endsWith(searchString[, length])
```

##### 参数

- **searchString：**要搜索的子字符串。
- **length：**<font color=FF0000>可选</font>，作为 str 的长度。<font color=FF0000>默认值为 str.length</font>。

**返回值：**如果传入的子字符串在搜索字符串的末尾则返回true；否则将返回 false。

**描述：**这个方法帮助你确定一个字符串是否在另一个字符串的末尾。<font color=FF0000>这个方法是大小写敏感的</font>。

摘自：[MDN - String.prototype.endsWith()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/endsWith)

##### 补充

和正则中的 `$` 符号一致



#### String.prototype.padStart()

padStart() 方法用另一个字符串填充当前字符串(如果需要的话，会重复多次)，以便产生的字符串达到给定的长度。从当前字符串的左侧开始填充。

##### 语法

```js
str.padStart(targetLength [, padString])
```

##### 参数

- **targetLength：**当前字符串需要填充到的目标长度。<font color=FF0000>如果这个数值小于当前字符串的长度，则返回当前字符串本身</font>。
- **padString：**<font color=FF0000>可选</font>，填充字符串。<font color=FF0000>如果字符串太长，使填充后的字符串长度超过了目标长度，则只保留（padString的）最左侧的部分，其他部分会被截断</font>。<font color=FF0000>**此参数的默认值为 " "**</font>（U+0020）。

**返回值：**在原字符串开头填充指定的填充字符串直到目标长度所形成的新字符串。

摘自：[MDN - String.prototype.padStart()]()

#### String.prototype.padEnd()

padEnd()  方法会用一个字符串填充当前字符串（如果需要的话则重复填充），返回填充后达到指定长度的字符串。从当前字符串的末尾（右侧）开始填充。

**语法**

```js
str.padEnd(targetLength [, padString])
```

##### 参数

- **targetLength：**当前字符串需要填充到的目标长度。<font color=FF0000>如果这个数值小于当前字符串的长度，则返回当前字符串本身</font>。
- **padString：**<font color=FF0000>可选</font>，填充字符串。<font color=FF0000>如果字符串太长，使填充后的字符串长度超过了目标长度，则只保留（padString的）最左侧的部分，其他部分会被截断。**此参数的缺省值为 " "（U+0020）**</font>。

**返回值：**在原字符串末尾填充指定的填充字符串直到目标长度所形成的新字符串。

摘自：[MDN - String.prototype.padEnd()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/padEnd)



#### String.prototype.trimStart()

trimStart() 方法从字符串的开头删除空格。<font color=FF0000>trimLeft() 是此方法的别名</font>。

##### 语法

```js
str.trimStart();
str.trimLeft();
```

##### 描述

trimStart() / trimLeft()方法移除原字符串右端的连续空白符并返回，trimStart() / trimLeft()方法并不会直接修改原字符串本身。

##### 别名

<mark>为了与 String.prototype.padStart 等函数保持一致，标准方法名称为trimStart。 但是，出于 Web 兼容性原因，trimLeft 仍然是 trimStart 的别名</mark>。在某些引擎中，这意味着：

```js
String.prototype.trimLeft.name === "trimStart";
```

摘自：[MDN - String.prototype.trimStart()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/trimStart)

#### String.prototype.trimEnd()

trimEnd() 方法从一个字符串的末端移除空白字符。<font color=FF0000>trimRight() 是这个方法的别名。</font>

##### 语法

```js
str.trimEnd();
str.trimRight();
```

##### 描述

trimEnd() / trimRight()方法移除原字符串右端的连续空白符并返回，<font color=FF0000>trimEnd() / trimRight()方法并不会直接修改原字符串本身</font>。

##### 别名

为了与 String.prototype.padEnd 等函数保持一致，标准方法名称为 trimEnd。 但是，出于 Web 兼容性原因，trimRight 仍然是 trimEnd 的别名。 在某些引擎中，这意味着：

```js
String.prototype.trimRight.name === "trimEnd";
```

摘自：[MDN - String.prototype.trimEnd()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/trimEnd)

#### String.prototype.localeCompare()

localeCompare() 方法 <font color=FF0000>返回一个数字</font> 来 <font color=FF0000>指示一个**参考字符串是否在排序顺序前面或之后或与给定字符串相同**</font>。
新的 locales 和 options 参数能让应用程序定制函数的行为，即指定用来排序的语言。 locales 和 options 参数完全取决于实现，在旧的实现中忽略这两个参数。

##### 语法

```js
referenceStr.localeCompare(compareString[, locales[, options]])
```

参数等内容由于过于庞杂，这里略；详见链接

摘自：[MDN - String.prototype.localeCompare()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare)



#### Unicode 相关的 字符串

<font color=FF0000>**在 ECMAScript 6 出现以前， JavaScript 字符串 一直基于 16 位字符编码 ( UTF-16 ) 进行构建**</font>（<mark>**注：**由此可知：UTF-16 和“代理对”不完全等价</mark>。详细可以参见：[[#Surrogate Pair 是 UTF-16 中用于扩展字符而使用的编码方式，是一种采用四个字节（两个 UTF-16 编码）来表示一个字符，称作 代理对]]）。<mark style="background: aqua">每 16 位的序列是一个**编码单元（code unit )**，代表一个 字符。 length、charAt() 等字符串属性和方法都是基于这种编码单元构造的</mark>。 当然， 在过去 16位足以包含任何字符， 直到 Unicode 引入扩展字符集， 编码规则才不得不进行变更 。

##### UTF-16 码位

Unicode 的目标是为全世界每一个字符提供全球唯一的标识符。 如果我们把字符长度限制在 16位 ， 码位数量将不足以表示如此多的字符。 <mark>这里说的“全球唯一的标识符”， 又被称作 <font color=FF0000 size=4>**码位 ( code point )**</font>， 是从 0 开始的数值</mark>（**注：**这里的 code point 和下面的各种兼容 unicode 的方法的*方法名* 相关）。 而表示字符的这些数值或码位， 我们称之为字符编码 ( character encode )。字符编码必须将码位编码为内部一致的编码单元。对于 UTF-16 来说 ， 码位可由多种编码单 元表示。

<font color=FF0000>在 UTF-16 中，**前 2^16^ 个码位**均 以 16 位的编码单元表示，**这个范围被称作 *基本多文种平面*** ( BMP, Basic Multilingual Plane )</font> 。<font color=FF0000>超出这个范围的码位则要归属于某个***辅助平面*** ( supplementary plane )</font> ， 其中 的码位仅用 16 位就无法表示了。<font color=FF0000 size=4>为此，**UTF-16 引入了代理对 ( surrogate pair )**，其**规定用两个 16 位编码单元表示一个码位**</font>。 这也就是说， <mark>字符串里的字符有两种， 一种是由一个编码单元 16 位表示的 BMP 字符， 另一种是由两个编码单元 32 位表示的辅助平面字符</mark>。 在 ECMAScript 5 中 ， 所有字符串 的操作都基于 16 位编码单元。 如果采用同样的方式处理包含代理对的 UTF-16 编码字符 ，得到的结果可能与预期不符。

摘自：深入理解ES6 - 字符串和正则表达式 P14 - P15

#### String.prototype.codePointAt()

codePointAt() 方法<font color=FF0000>返回 一个 **Unicode 编码点值的非负整数**</font>。

##### 语法

```js
str.codePointAt(pos)
```

##### 参数

- **pos：**这个字符串中需要转码的元素的位置。

**返回值：**返回值是在字符串中的给定索引的编码单元体现的数字，<font color=FF0000>如果在索引处没找到元素则返回 undefined</font> 。

##### 描述

如果在指定的位置没有元素则返回 undefined 。<font color=FF0000>如果在索引处开始没有 UTF-16 代理对，将直接返回在那个索引处的编码单元</font>。

##### Surrogate Pair 是 UTF-16 中用于扩展字符而使用的编码方式，是一种采用四个字节（两个 UTF-16 编码）来表示一个字符，称作*代理对*

> 代理对也就是 UTF-16 （的）<font color=FF0000>扩展字符</font>
>
> 摘自：[现代JS教程 - Iterable object（可迭代对象）- 字符串是可迭代的](https://zh.javascript.info/iterable#zi-fu-chuan-shi-ke-die-dai-de)

##### 示例

```js
'ABC'.codePointAt(1);          // 66
'\uD800\uDC00'.codePointAt(0); // 65536
'XYZ'.codePointAt(42);         // undefined
```

摘自：[MDN - String.prototype.codePointAt()]()

> <font color=FF0000>对于 BMP 字符集中的字符， codePointAt() 方法的返回值与 charCodeAt() 方法的相同</font>（**注：**这里不是 charAt()，注意不要混淆）， 而对于非 BMP 字符集来说返回值则不同
>
> 摘自：深入理解ES6 - 字符串和正则表达式 P16

#### String.fromCodePoint()

String.fromCodePoint() **静态方法** <font color=FF0000>返回 使用指定的代码点序列创建的字符串</font>。

##### 语法

```js
String.fromCodePoint(num1[, ...[, numN]])
```

##### 参数

- **num1, ... , num*N* ：**一串 Unicode 编码位置，即“代码点”。

**返回值：**使用指定的 Unicode 编码位置创建的字符串。

##### 异常

- **RangeError：**如果传入无效的 Unicode 编码，将会抛出一个RangeError（例如："RangeError: NaN is not a valid code point"）。

**说明：**该方法返回一个字符串，而不是一个 String 对象。因为 fromCodePoint() 是 String 的一个静态方法，所以只能通过 String.fromCodePoint() 这样的方式来使用，不能在你创建的 String 对象实例上直接调用。

摘自：[MDN - String.fromCodePoint()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/fromCodePoint)

> ECMAScript 通常会面向同一个操作提供正反两种方法。你可以使用 codePointAt() 方法在字符串中检索一个字符的码位，也可以使用 String.fromCodePoint() 方法根据指定的码位生成一个字符（串）（<mark>**注：**即 **codePointAt() 和 fromCodePoint() 是一对相反的方法**。另外，类似的还有 Object.entries() 和 Object.fromEntries()</mark> ）。举个例子：
>
> ```js
> console.log( String.fromCodePoint(134071) ) // 𠮷
> ```
>
> 可以将 String.fromCodePoint() 看成是更完 整版的 String.fromCharCode() 。 同样，对于 BMP 中的所有字符，这两个方法的执行结果相同。 只有传递非 BMP 的码位作为参数时， 二者执行结果才有可能 不同。
>
> 摘自：深入理解ES6 - 字符串和正则表达式 P17

#### String.prototype.normalize()

normalize() 方法会按照指定的一种 Unicode 正规形式将当前字符串正规化。（如果该值不是字符串，则首先将其转换为一个字符串）。

##### 语法

```js
str.normalize( [form] )
```

##### 参数

- **form：**<font color=FF0000>可选</font>。四种 Unicode 正规形式 ( Unicode Normalization Form ) "NFC"、"NFD"、"NFKC"，或 "NFKD" 其中的一个，默认值为 "NFC"。

  > - **"NFC"：**默认选项，以标准等价方式分解,然后以标准等价方式重组。
  > - **"NFD"：**以标准等价方式分解
  > - **"NFKC"：**以兼容等价方式分解
  > - **"NFKD"：**以兼容等价方式分解,然后以标准等价方式重组
  >
  > 摘自：深入理解ES6 - 字符串和正则表达式 P17

**返回值：**含有给定字符串的 Unicode 规范化形式的字符串。

##### 可能出现的异常

- **RangeError：**如果给 form 传入了上述四个字符串以外的参数，则会抛出 RangeError 异常。

摘自：[MDN - String.prototype.normalize()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/normalize)

##### String.prototype.normalize() 的补充：normalize 的使用场景

> 在许多语言中，都有一些由基本字符组成的符号，在其上方/下方有一个标记。
>
> 例如，字母 `a` 可以是 `àáâäãåā` 的基本字符。最常见的“复合”字符在 UTF-16 表中都有自己的代码。但不是全部，因为可能的组合太多。为了支持任意组合，UTF-16 允许我们使用多个 Unicode 字符：基本字符紧跟“装饰”它的一个或多个“标记”字符。
>
> 例如，如果我们 `S` 后跟有特殊的 “dot above” 字符（代码 `\u0307`），则显示 Ṡ。
>
> ```js
> console.log( 'S\u0307' ); // Ṡ
> ```
>
> 如果我们需要在字母上方（或下方）添加额外的标记 —— 没问题，只需要添加必要的标记字符即可。
>
> 例如，如果我们追加一个字符 “dot below”（代码 `\u0323` ），那么我们将得到“S 上面和下面都有点”的字符：`Ṩ`。例如：
>
> ```javascript
> console.log( 'S\u0307\u0323' ); // Ṩ
> ```
>
> 这在提供良好灵活性的同时，也<font color=FF0000>存在一个有趣的问题：**两个视觉上看起来相同的字符，可以用不同的 Unicode 组合表示**</font>。例如
>
> ```js
> let s1 = 'S\u0307\u0323'; // Ṩ，S + 上点 + 下点
> let s2 = 'S\u0323\u0307'; // Ṩ，S + 下点 + 上点
> 
> console.log( `s1: ${s1}, s2: ${s2}` );
> console.log( s1 == s2 ); // false，尽管字符看起来相同（?!）
> ```
>
> 为了解决这个问题，有一个 “Unicode 规范化”算法，它将每个字符串都转化成单个“通用”格式。它由 str.normalize() 实现。
>
> ```js
> console.log( "S\u0307\u0323".normalize() == "S\u0323\u0307".normalize() ); // true
> ```
>
> 摘自：[现代JS教程 - 字符串 - 变音符号与规范化](https://zh.javascript.info/string#bian-yin-fu-hao-yu-gui-fan-hua)

##### 其他关于 UTF-16代理对字符串的补充

<font color=FF0000 size=4>**字符串迭代器能够识别代理对**</font>

> ##### for...of
>
> 数组和字符串是使用最广泛的内建可迭代对象。对于一个字符串，`for..of` 遍历它的每个字符；<font color=FF0000>对于代理对 ( surrogate pairs )，它也能正常工作</font>！
>
> ```js
> let str = '𝒳😂';
> for (let char of str) {
>     console.log( char ); // 𝒳，然后是 😂
> }
> ```
>
> ##### Array.from()
>
> 全局方法 Array.from 可以接受一个可迭代或类数组的值，并从中获取一个“真正的”数组。然后我们就可以对其调用数组方法了。<font color=FF0000>**Array.from() 对于代理对也是支持的**</font>：
>
> ```js
> let str = '𝒳😂';
> let chars = Array.from(str); // 将 str 拆分为字符数组
>                                                               
> console.log(chars[0]); // 𝒳
> console.log(chars[1]); // 😂
> console.log(chars.length); // 2
> ```
>
> 与 str.split 方法不同，它依赖于字符串的可迭代特性（注：这是 ES6 的 Symbol.iterator 的特性）。因此，就像 for..of 一样，可以正确地处理代理对。
>
> **注：**个人感觉，ES6 中新增的字符串相关的方法，都是支持代理对的；感觉这也是在情理之中的。
>
> 摘自：[现代JS教程 - Iterable object（可迭代对象）](https://zh.javascript.info/iterable)



#### JavaScript 比较 和 逻辑运算

- **===** ：绝对等于（值和类型均相等）

- **!==**： <font color=FF0000>不绝对等于</font>（<font color=FF0000>**值和类型有一个不相等，或两个都不相等**</font>）

#### For 循环

for-in 循环，<font color=FF0000>for-in 循环实际是为循环 ”enumerable“ 对象而设计的</font>（**注：**for in 是用来遍历，对象中 enumerable 数据属性为 true 的属性）。示例：

```js
for ( elem in elems ){ // code}
```

<font color=FF0000>不推荐用 for-in 来循环一个**数组**，因为，不像对象，数组的`index`跟普通的对象属性不一样，是重要的数值序列指标</font>。

##### forEach 循环（ JavaScript5 引入）

forEach() 方法用于调用<font color=FF0000>数组</font>的每个元素，并将元素传递给回调函数。

**语法：**

```js
array.forEach(function(currentValue, index, arr), thisValue)
```

**参数：**

- **function(currentValue, index, arr)**：<font color=FF0000>必需</font>。 数组中每个元素需要调用的函数。
  - **currentValue**    必需。当前元素
  - **index**    可选。当前元素的索引值。
  - **arr**    可选。当前元素所属的数组对象。
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

#### for ... of

for ... of语句<font color=FF0000>在可迭代对象（包括 Array，Map，Set，String，TypedArray，arguments 对象等等）上创建一个迭代循环</font>，调用自定义迭代钩子，并为每个不同属性的值执行语句

**补充：**

for of 循环获取index值，将Array.entries()将其包装为可迭代对象，再遍历，示例如下：

```js
for (const [index, val] of arr.entries()) {
  // ...
}
```

摘自：[for of 循环获取index值](https://blog.csdn.net/Hero_rong/article/details/109536906)

<font size=4>**对于对象的遍历**</font>

遍历对象有如下方法：

- **使用for in**

  ```js
  for (const key in obj) {
    console.log(key, obj[key])
  }
  ```

- **使用Object.keys()**

  ```js
  Object.keys(obj).forEach(key => {
    console.log(key, obj[key])
  })
  ```

- **使用Object.getOwnPropertyNames()**

  ```js
  Object.getOwnPropertyNames(obj).forEach(key => {
    console.log(key, obj[key])
  })
  ```

- **使用Reflect.ownKeys()**

  ```js
  Reflect.ownKeys(obj).forEach(key => {
    console.log(key, obj[key])
  })
  ```


**区别：**

- Reflect.ownKeys() 和 Object.getOwnPropertyNames() 可以获取 enumerable 为 false 的数据，而 Object.keys() 和 for...of 不可以。

- Object.getOwnPropertyNames() 不能获取Symbol，需要通过Object.getOwnPropertySymbols() 获取；而Reflect.ownKeys() 都可以拿到。

  补充：经 codingstartup群友的补充：Reflect.ownKeys()的遍历是有序的，按照你添加的顺序：

  - 首先遍历所有数值键，按照数值升序排列
  - 其次遍历所有字符串键，按照加入时间升序排列
  - 最后遍历所有 Symbol 键，按照加入时间升序排列

##### 《现代 JS 教程》中的相关内容

> -   Object.getOwnPropertyNames(obj) 返回非 Symbol 键。
> -   Object.getOwnPropertySymbols(obj) 返回 Symbol 键。
> -   Object.keys/values() 返回带有 enumerable 标志的非 Symbol 键/值
> -   for..in 循环遍历所有带有 enumerable 标志的非 Symbol 键，以及原型对象的键。
>
> 摘自：[现代 JS 教程 - Proxy 和 Reflect](https://zh.javascript.info/proxy)



#### **for ... of 和 for ... in 的区别**

无论是 for...in 还是 for...of 语句都是迭代一些东西。它们之间的主要区别在于它们的迭代方式。

- for...in 语句以任意顺序迭代对象的可枚举属性。
- for...of 语句遍历可迭代对象定义要迭代的数据。

摘自：[MDN - for ... of](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of)

**更多：**

- 推荐<font color=FF0000>在循环对象属性的时候，使用 for...in </font>，<font color=0000FF>在遍历数组的时候的时候使用 for...of</font>。
- **<font color=FF0000>for...in 循环出的是 key</font>，<font color=0000FF>for...of 循环出的是value</font>（ <font color=FF0000>for in是遍历键名</font>，<font color=0000FF>for of是遍历键值</font>）**
- 注意，<font color=0000FF>for...of 是 ES6 新引入的特性</font>。修复了 ES5 引入的for...in的不足
- <font color=FF0000>for...of 不能循环普通的对象，需要通过和 Object.keys() 搭配使用</font>
- for...of 兼容性还不够，移动端安卓微信浏览器貌似不支持，苹果的可以；web端 IE 支持也不够，chrome 可以。

摘自：[javascript总for of和for in的区别？](https://segmentfault.com/q/1010000006658882)

**补充：**

- for of <font color=FF0000 size=4>**遍历的是拥有 iterator 属性（注：即 Symbol.iterator ）的对象**</font>，出来的是属性值；

- for in 遍历对象的属性，出来的是属性名，<font color=FF0000 size=4>**包括继承的属性**</font>，<font color=FF0000>**可枚举的属性**</font>（ enumerable 为 true 的属性），<font color=FF0000>**不包括 symbol**</font>

  **注：**如果不想要 for in 打印出 父类的属性，可以用 hasOwnProperty 方法，判断是否为本对象的属性。

摘自：[峰华前端工程师动态 的 评论区 -PILOT- 的评论](https://t.bilibili.com/524677482676016409)

#### forEach 和 map 方法有什么区别

这方法都是用来遍历数组的，两者区别如下：

- **forEach()：**会针对每一个元素执行提供的函数，对数据的操作<font color=FF0000>**会改变原数组**，该方法**没有返回值**</font>
- **map()：**<font color=FF0000>**不会改变原数组的值**，**返回一个新数组**</font>，新数组中的值为原数组调用函数处理之后的值


摘自：[「2021」高频前端面试题汇总之JavaScript篇（上）](https://juejin.cn/post/6940945178899251230)



#### JS中的实例方法和静态方法定义
下面对象和数组的方法中，带有prototype的属于实例方法（作用在对象上），不带prototype的是静态方法（作用在类上）



#### Array.prototype.keys()

 keys() 方法返回一个包含数组中每个索引键的Array Iterator对象。示例如下：

```js
const array1 = ['a', 'b', 'c'];
const iterator = array1.keys();

for (const key of iterator) { console.log(key); }
```

- **语法**

  ```js
  arr.keys()
  ```

- **返回值：**一个新的 Array 迭代器对象。

- **示例：**<font color=FF0000>索引迭代器会包含那些没有对应元素的索引</font>

  ```js
  var arr = ["a", , "c"];
  var sparseKeys = Object.keys(arr);
  var denseKeys = [...arr.keys()];
  console.log(sparseKeys); // ['0', '2'] 注：使用 Object.keys() 不包含
  console.log(denseKeys);  // [0, 1, 2] 注：使用 Array.prototype.keys() 是包含的
  ```

摘自：[MDN - Array.prototype.keys()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/keys)

#### Array.prototype.values()

<font color=FF0000>values() 方法**返回一个新的 Array Iterator 对象**，**该对象包含数组每个索引的值**</font>

**示例：**

```js
let arr = ['w', 'y', 'k', 'o', 'p'];
let eArr = arr.values();

console.log(eArr); // Object [Array Iterator] {}

for (let letter of eArr) {
  console.log(letter);
} //"w" "y "k" "o" "p"
```

<font color=FF0000>**Array.prototype.values** 是 **Array.prototype[Symbol.iterator]** 的默认实现。</font>代码如下：

```js
Array.prototype.values === Array.prototype[Symbol.iterator]  // true 
```

返回的可迭代对象可以使用 .next() 迭代，示例如下：

```js
var arr = ['a', 'b', 'c', 'd', 'e'];
var iterator = arr.values();
iterator.next();  // Object { value: "a", done: false }
iterator.next().value;         // "b"
iterator.next()["value"];      // "c"
iterator.next();               // Object { value: "d", done: false }
iterator.next();               // Object { value: "e", done: false }
iterator.next();               // Object { value: undefined, done: true }
iterator.next().value;         // undefined
```

摘自：[MDN - Array.prototype.values()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/values)

#### Array.prototoype.entries()

entries() 方法 <font color=FF0000 size=4>**返回一个新的Array Iterator对象**</font>（不是一个数组，Array.isArray() === false ），该对象包含数组中每个索引的键/值对。示例如下：

```js
const array1 = ['a', 'b', 'c'];
const iterator1 = array1.entries();

console.log(iterator1.next().value); // expected output: Array [0, "a"]
console.log(iterator1.next().value); // expected output: Array [1, "b"]
```

**语法**

```js
arr.entries()
```

**返回值**

一个新的 Array 迭代器对象。Array Iterator是对象，它的原型（\__proto__: Array Iterator）上有一个next方法，可用用于遍历迭代器取得原数组的 [ key, value ] 。

摘自：[MDN - Array.prototype.entries()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/entries)

#### Array.prototype\[@@iterator]()

@@iterator<font color=FF0000>（`@@` 是 `Symbol.` 的简写；即：`Symbol.iterator`为`@@iterator`） </font>属性和 Array.prototype.values() 属性的初始值是同一个函数对象。

- **语法**

  ```js
  arr[Symbol.iterator]()
  ```

- **返回值**
  数组的 iterator 方法，<font color=FF0000>默认情况下，与 values() 返回值相同</font>， arr[Symbol.iterator] 则会返回 values() 函数。

摘自：[MDN - Array.prototype[@@iterator]()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/@@iterator)



#### Array.prototype.reverse()

reverse() 方法将数组中元素的位置颠倒，并返回该数组。数组的第一个元素会变成最后一个，数组的最后一个元素变成第一个。该方法会改变原数组。

- **语法：**

  ```js
  arr.reverse()
  ```

- **返回值：**颠倒后的数组。

- **描述：**

  reverse 方法颠倒数组中元素的位置，改变了数组，并返回该数组的引用。

  <font color=FF0000> reverse方法是特意类化的</font>；<font color=FF0000> 此方法可被 called 或 applied于类似数组对象</font>。对象如果不包含反映一系列连续的、基于零的数值属性中的最后一个长度的属性，则该对象可能不会以任何有意义的方式运行。

- **示例：**

  **颠倒类数组中的元素**
  下例<mark>创造了一个类数组对象 a, 包含3个元素和一个 length 属性, 然后颠倒这个类数组对象</mark>。  reverse() 的调用返回一个颠倒后的类数组对象 a的引用。

  ```js
  const a = {0: 1, 1: 2, 2: 3, length: 3};
  console.log(a); // {0: 1, 1: 2, 2: 3, length: 3}
  Array.prototype.reverse.call(a); //这里用apply也一样
  console.log(a); // {0: 3, 1: 2, 2: 1, length: 3}
  ```

摘自：[MDN - Array.prototype.reverse()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)



#### Array.prototype.find()

find() 方法<font color=FF0000>返回数组中满足提供的测试函数的第一个元素的值，否则返回 undefined</font>。

**语法**

```js
arr.find(callback[, thisArg])
```

**参数**

- **callback：**在数组每一项上执行的函数，接收 3 个参数：
  - element：当前遍历到的元素。
  - index：可选，当前遍历到的索引。
  - array：可选，数组本身。
- **thisArg：**可选，<font color=FF0000>执行回调时用作this 的对象</font>。

**返回值：**数组中第一个满足所提供测试函数的元素的值，否则返回 undefined。

**示例：**

```js
const array1 = [5, 12, 8, 130, 44];
const found = array1.find(element => element > 10);
console.log(found); // expected output: 12
```

而 indIndex() 方法，它返回数组中找到的元素的索引，而不是其值。

摘自：[MDN - Array.prototype.find()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/find)


#### Array.prototype.reduce()

**reduce()** 方法 <font color=FF0000>对数组中的每个元素 **执行一个自定义的 reducer 函数（升序执行）**</font>，<font color=FF0000>每一次运行 **reducer** 会将先前元素的计算结果</font>（**注：**说成“上一次的计算结果”，感觉更容易理解些）<font color=FF0000>作为参数传入</font>，<font color=FF0000>**最后将其结果汇总为单个返回值**（**注：**非常重要！**上一次的计算结果必须要返回**）</font>，真是因为要返回，如果写了没有 {}包裹、也没有 return 的箭头函数时，不要用 push，使用 concat。

第一次执行回调函数时，不存在 “上一次的计算结果”。如果需要回调函数从数组索引为 0 的元素开始执行，则需要传递初始值。<mark>否则，数组索引为 0 的元素将被作为初始值 *initialValue*</mark>，<font color=FF0000>**迭代器将从第二个元素开始执行（索引为 1 而不是 0）**</font>。

示例如下：

```js
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));  // expected output: 10

// 5 + 1 + 2 + 3 + 4，其中5为初始值（initialValue）
console.log(array1.reduce(reducer, 5));  // expected output: 15
```

**reducer** 逐个遍历数组元素，每一步都将当前元素的值与上一步的计算结果相加（上一步的计算结果是当前元素之前 所有元素的总和）——直到没有更多的元素被相加。

**reducer 函数接收4个参数：**

1. Accumulator ( acc ) ：累计器（累加器）。**注：**2022 / 4 / 8 MDN 文档改变了，这个参数被称为 previousValue；不过，毕竟 reduce 不仅仅有 “累加的” 功能。

   要看之前的版本可以看：https://web.archive.org/web/20220314073057/https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce，可以发现 2022 / 3 / 14 这里还没改动...

2. Current Value ( cur ) ：当前值

3. Current Index ( idx ) ：当前索引

4. Source Array ( src ) ：源数组

##### 语法

```js
array.reduce( callback( accumulator, currentValue [, index [, array]]) [, initialValue] )
```

**参数**

- **callback：**执行数组中每个值（如果没有提供 initialValue，则第一个值除外）的函数，包含四个参数：
  - **accumulator：**累计器累计回调的返回值；它是上一次调用回调时返回的累积值，或 initialValue（见于下方）。
  
  - **currentValue：**数组中正在处理的元素。在第一次调用时，若指定了初始值 `initialValue`，其值则为数组索引为 0 的元素 array[0]，否则为 array[1]。
  
  - **index：** <font color=FF0000>可选</font>，数组中正在处理的当前元素的索引。 如果提供了initialValue，则起始索引号为0，否则从索引1起始。
  
  - **array：**<font color=FF0000>可选</font>，调用reduce()的数组
  
- **initialValue：**<font color=FF0000>可选</font>，作为第一次调用 callback函数时的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。 在没有初始值的空数组上调用 reduce 将报错。

**返回值：**函数累计处理的结果

##### 异常

**TypeError：**数组为空且初始值 initialValue 未提供。



摘自：[MDN - Array.prototype.reduce()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

#### Array.prototype.reduceRight()

reduceRight() 方法接受一个函数作为累加器（accumulator）和数组的每个值（<font color=FF0000>从右到左</font>）将其减少为单个值。

**异常使用**

- 如果数组为空，但提供了 initialValue 参数，或如果数组中只有一个元素，且没有提供 initialValue 参数，将会直接返回 initialValue 参数或数组中的那一个元素。这两种情况下，都不会调用 callback 函数。

- 如果数组为空，且没有提供 initialValue 参数，则会抛出一个 TypeError 错误。

最终，首次调用时的情况可汇总为此表：

|              数组内元素数量               | 是否提供 initialValue |                             结果                             |
| :---------------------------------------: | :-------------------: | :----------------------------------------------------------: |
|             <mark>> 1</mark>              |        未提供         | accumulator 为数组中（下略）最后一个元素 currentValue 为倒数第二个元素 |
|              <mark>>1</mark>              |         提供          |   accumulator 为 initialValue currentValue 为最后一个元素    |
|                    = 1                    |        未提供         |                 直接返回数组中的唯一一个元素                 |
| <mark style="background: aqua">= 0</mark> |         提供          |                    直接返回 initialValue                     |
| <mark style="background: aqua">=0</mark>  |        未提供         |                     抛出 TypeError 错误                      |

**个人补充：**<font color=FF0000>该函数有作为迭代器的作用（从右向左迭代），否则，仅仅作为累加器的功能，Array.prototype.reduce()同样可以做到</font>。示例如下：

```js
const array1 = [[0, 1], [2, 3], [4, 5]].reduceRight(
  (accumulator, currentValue) => accumulator.concat(currentValue)
);

console.log(array1); // expected output: Array [4, 5, 2, 3, 0, 1]
```

摘自：[MDN - Array.prototype.reduceRight()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight)



#### Array.prototype.filter()

**filter()** 方法创建一个新数组, 其包含通过所提供函数实现的测试（即满足条件）的所有元素。 

- **示例如下：**
  
  ```js
  const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];
  const result = words.filter(word => word.length > 6);
  console.log(result);
  // expected output: Array ["exuberant", "destruction", "present"]
  ```

- **语法**
  
  ```js
  var newArray = arr.filter(callback(element[, index[, array]])[, thisArg])
  ```

- **参数**
  
  - callback：用来测试数组的每个元素的函数。返回 true 表示该元素通过测试，保留该元素，false 则不保留。它接受以下三个参数：
    - element：数组中当前正在处理的元素。
    - index：（可选）正在处理的元素在数组中的索引。
    - array：（可选）调用了 filter 的数组本身。
  - thisArg：（可选）<font color=FF0000>**执行 callback 时，用于 this 的值**。</font>

- **返回值**
  一个<font color=FF0000>新的</font>、由通过测试的元素组成的数组，<font color=FF0000>如果没有任何数组元素通过测试，则返回空数组</font>。

如果为 filter 提供一个 thisArg 参数，则它会被作为 callback 被调用时的 this 值。否则，callback 的 this 值在非严格模式下将是全局对象，严格模式下为 undefined

filter 遍历的元素范围在第一次调用 callback 之前就已经确定了。在调用 filter 之后被添加到数组中的元素不会被 filter 遍历到。如果已经存在的元素被改变了，则他们传入 callback 的值是 filter 遍历到它们那一刻的值。被删除或从来未被赋值的元素不会被遍历到

摘自：[MDN - Array.prototype.filter()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)



#### Array.prototype.map()

**map()** 方法创建一个新数组，其结果是该数组中的每个元素是调用一次提供的函数后的返回值。

```js
const array1 = [1, 4, 9, 16];

// pass a function to map
const map1 = array1.map(x => x * 2);
```

**Array.prototype.map() 可以起到和 forEach类似的作用**。注：map方法 在编程中一个常用的场景是：“挑出对象数组中的部分属性形成一个新的数组，甚至是对跳出的属性进行属性名修改”；不过，在编程中发现：map 这种形成新数组的方法，对其中引用类型的属性只是拷贝了引用，不是深拷贝。

**语法**

```js
var new_array = arr.map(callback(currentValue[, index[, array]])[, thisArg])
```

**参数**

- callback：生成新数组元素的函数，使用三个参数：
  - currentValue：callback 数组中正在处理的当前元素。
  - index可选：callback 数组中正在处理的当前元素的索引。
  - array可选：map 方法调用的数组。
  - thisArg可选：执行 callback 函数时值被用作this。
- 返回值：一个由原数组每个元素执行回调函数的结果组成的新数组。

即，如上代码可以这样写：

```js
const map1 = array1.map((x, index, arr) => {
  console.log(index); // 0, 1, 2, 3
  console.log(arr); //[1, 4, 9, 16]
  return x * 2
});
```

**其他：**

- 因为 map 生成一个新数组，当你不打算使用返回的新数组却使用 map 是违背设计初衷的，请用forEach或者for-of替代。你不该使用map: (A) 你不打算使用返回的新数组，或 (B) 你没有从回调函数中返回值。

- map() 中可以直接写函数，示例如下：

  ```js
  var numbers = [1, 4, 9];
  var roots = numbers.map(Math.sqrt); // roots的值为[1, 2, 3], numbers的值仍为[1, 4, 9]
  ```

摘自：[MDN - Array.prototype.map()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

##### 关于上面的三个函数式编程的补充

> map 是<font color=FF0000>**映射**</font>、reduce 是<font color=FF0000>聚合</font>，filter 是筛选。
>
> 摘自：[python如何理解map，reduce，filter？ - 朱卫军的回答 - 知乎](https://www.zhihu.com/question/265648060/answer/2196619912)
>
> 函数式编程，<font color=FF0000>把算子和数据都交给这几个函数来进行批量执行</font>。
>
> 摘自：[python如何理解map，reduce，filter？ - gashero的回答 - 知乎](https://www.zhihu.com/question/265648060/answer/2418365397)



#### Array.prototype.sort()

sort() 方法用原地算法对数组的元素进行排序，并返回数组。<font color=FF0000>**默认排序顺序是在将元素转换为字符串**，然后**比较它们的UTF-16代码单元值序列**（字典序？？）时构建的</font>

同时和C++的**比较器**一样，sort函数可以自定义比较函数。

- **语法**
  
  ```js
  arr.sort([compareFunction])
  ```

- **参数**
  
  - compareFunction： 可选，用来指定按某种顺序进行排列的函数。如果省略，元素按照转换为的字符串的各个字符的Unicode位点进行排序。
    - firstEl：第一个用于比较的元素。
    - secondEl：第二个用于比较的元素。

- **返回值**
  排序后的数组。请注意，数组已原地排序，并且不进行复制。

摘自：[MDN - Array.prototype.sort()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)



#### Array.prototype.slice() 切片

slice() 方法<font color=FF0000>**返回一个新的数组对象**</font>，这一对象是一个<font color=FF0000>由 begin 和 end 决定的原数组的**浅拷贝**</font>（包括 begin，不包括end）。<font color=FF0000>原始数组不会被改变</font>。

- **语法**
  
  ```js
  arr.slice([begin[, end]])
  ```

- **参数**
  
  - **begin** <font color=FF0000>可选</font>，提取起始处的索引（从 0 开始），从该索引开始提取原数组元素。
    - <font color=FF0000>如果该参数为负数，则表示从原数组中的倒数第几个元素开始提取</font>（**注：**注意和 substring() 的区别），slice(-2) 表示提取原数组中的倒数第二个元素到最后一个元素（包含最后一个元素）。
    - <font color=FF0000>如果省略 begin，则 slice 从索引 0 开始。</font>
    - <font color=FF0000>如果 begin 超出原数组的索引范围，则会返回空数组。</font>
  - **end** <font color=FF0000>可选</font>
    - 提取终止处的索引（从 0 开始），在该索引处结束提取原数组元素。slice 会提取原数组中索引从 begin 到 end 的所有元素（包含 begin，但不包含 end）。
    - slice(1,4) 会提取原数组中从第二个元素开始一直到第四个元素的所有元素 （索引为 1, 2, 3的元素）。
      <font color=FF0000>如果该参数为负数， 则它表示在原数组中的倒数第几个元素结束抽取</font>。 **slice(-2,-1) 表示抽取了原数组中的倒数第二个元素到最后一个元素（不包含最后一个元素，也就是只有倒数第二个元素）**。
    - <font color=FF0000>如果 end 被省略，则 slice 会一直提取到原数组末尾。</font>
    - <font color=FF0000>如果 end 大于数组的长度，slice 也会一直提取到原数组末尾。</font>

- **返回值**
  一个含有被提取元素的新数组。

**描述**
<font color=FF0000>slice 不会修改原数组，只会返回一个浅复制了原数组中的元素的一个新数组</font>。原数组的元素会按照下述规则拷贝：

- 如果该元素是个对象引用 （不是实际的对象），slice 会拷贝这个对象引用到新的数组里。两个对象引用都引用了同一个对象。如果被引用的对象发生改变，则新的和原来的数组中的这个元素也会发生改变。
- 对于字符串、数字及布尔值来说（不是 String、Number 或者 Boolean 对象），slice 会拷贝这些值到新的数组里。在别的数组里修改这些字符串或数字或是布尔值，将不会影响另一个数组。

如果向两个数组任一中添加了新元素，则另一个不会受到影响。

摘自：[MDN - Array.prototype.slice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

#### String.prototype.substring()

substring() 方法<font color=FF0000>返回一个字符串在开始索引到结束索引之间的一个子集</font>，<font color=FF0000>**或** 从开始索引直到字符串的末尾的一个子集</font>。

**语法**

```js
str.substring(indexStart[, indexEnd])
```

**参数**

- **indexStart：**需要截取的第一个字符的索引，该索引位置的字符作为返回的字符串的首字母。
- **indexEnd：**<font color=FF0000>**可选**</font>。一个 0 到字符串长度之间的整数，以该数字为索引的字符不包含在截取的字符串内。

**返回值：**包含给定字符串的指定部分的新字符串。

**描述**
substring 提取从 indexStart 到 indexEnd（不包括）之间的字符。特别地：

- <font color=FF0000>如果 indexStart 等于 indexEnd，substring 返回一个空字符串</font>。
- 如果省略 indexEnd，substring 提取字符一直到字符串末尾。
- <font color=FF0000>**如果任一参数小于 0 或为 NaN，则被当作 0**</font>。
- <font color=FF0000>如果任一参数大于 stringName.length，则被当作 stringName.length</font>。
- <font color=FF0000>**如果 indexStart 大于 indexEnd，则 substring 的执行效果就像两个参数调换了一样**</font>。

下面还有关于关于“描述”更详细的示例，这里略

摘自：[MDN - String.prototype.substring()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substring)

#### String.prototype.substr()

⚠️<font color=FF0000 size=4>**警告：**</font>尽管 String.prototype.substr(…) 没有严格被废弃 (as in "removed from the Web standards")，但<font color=FF0000>它被认作是遗留的函数并且可以的话应该避免使用</font> 。<font color=FF0000>**它并非 JavaScript核心语言的一部分，未来将可能会被移除掉**。如果可以的话，使用 substring() 替代它</font>。

**substr()** 方法返回一个字符串中从指定位置开始到指定字符数的字符。

**语法**

```js
str.substr(start[, length])
```

**参数**

- **start：**开始提取字符的位置。<font color=FF0000>如果为负值，则被看作 strLength + start</font>，其中 strLength 为字符串的长度（例如，如果 start 为 -3，则被看作 strLength + (-3) ）。
- **length：**<font color=FF0000>**可选**</font>。<font color=FF0000>提取的字符数</font>。

**描述**

start 是一个字符的索引。substr 从 start 位置开始提取字符，提取 length 个字符（或直到字符串的末尾）。

- 如果 <font color=FF0000>start 为正值，且大于或等于字符串的长度</font>，则 substr 返回一个空字符串。

- 如果 <font color=FF0000>start 为负值</font>，则 substr 把它作为从字符串末尾开始的一个字符索引（**注：**即从 start + length 开始提取 ）。

  如果 start 为负值且 abs(start) 大于字符串的长度，则 substr 使用 0 作为开始提取的索引。注意负的 start 参数不被 MS JScript 支持

- 如果 <font color=FF0000>**length 为 0 或负值**</font>，则 substr 返回一个空字符串。如果忽略 length，则 substr 提取字符，直到字符串末尾。

摘自：[MDN - String.prototype.substr()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substr)

##### 三种方法的总结

| 方法                  | 选择方式……                                    | 负值参数          |
| :-------------------- | :-------------------------------------------- | :---------------- |
| slice(start, end)     | 从 start 到 end（不含 end）                   | 允许              |
| substring(start, end) | start 与 end 之间（包括 start，但不包括 end） | 负值代表 0        |
| substr(start, length) | 从 start 开始获取长为 length 的字符串         | 允许 start 为负数 |

摘自：[现代JS教程 - 字符串 - 获取子字符串](https://zh.javascript.info/string#huo-qu-zi-zi-fu-chuan)



#### Array.prototype.splice()

splice() 方法<font color=FF0000>通过**删除或替换现有元素**或者**原地添加新的元素**来修改数组</font>，并以数组形式返回被修改的内容。此方法会改变原数组。

- **语法**

  ```js
  array.splice(start[, deleteCount[, item1[, item2[, ...]]]])
  ```

- **参数**
  - <font color=FF0000>**start：**指定修改的开始位置（从0计数）</font>。如果超出了数组的长度，则从数组末尾开始添加内容；如果是负值，则表示从数组末位开始的第几位（从-1计数，这意味着-n是倒数第n个元素并且等价于array.length-n）；如果负数的绝对值大于数组的长度，则表示开始位置为第0位。
  - <font color=FF0000>**deleteCount ：**（可选）整数，表示**要移除的数组元素的个数**</font>。
    - 如果 deleteCount 大于 start 之后的元素的总数，则从 start 后面的元素都将被删除（含第 start 位）。
    - 如果 deleteCount 被省略了，或者它的值大于等于array.length - start(也就是说，如果它大于或者等于start之后的所有元素的数量)，那么start之后数组的所有元素都会被删除。
    - 如果 deleteCount 是 0 或者负数，则不移除元素。这种情况下，至少应添加一个新元素。
  - **item1, item2, ... ：**（可选）要添加进数组的元素,从start 位置开始。如果不指定，则 splice() 将只删除数组元素。
- **返回值：**由被删除的元素组成的一个数组。如果只删除了一个元素，则返回只包含一个元素的数组。<font color=FF0000>如果没有删除元素，则返回空数组</font>。

- **描述：**如果添加进数组的元素个数不等于被删除的元素个数，数组的长度会发生相应的改变。

摘自：[MDN - Array.prototype.splice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)



#### Array.prototype.concat()

 concat() 方法用于合并两个或多个数组。<font color=FF0000>此方法不会更改现有数组，而是返回一个新数组。</font>

摘自：[MDN - Array.prototype.concat()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)



#### Array.prototype.fill()

fill() 方法用一个固定值填充一个数组中从起始索引到终止索引内的全部元素。不包括终止索引。

- **语法**

  ```js
  arr.fill(value[, start[, end]])
  ```

- **参数**
  - value：用来填充数组元素的值。
  - start：可选，起始索引，<font color=FF0000>默认值为0</font>。
  - end 可选，终止索引，<font color=FF0000>默认值为 this.length</font>。
- **返回值：**修改后的数组。
- **描述：**<font color=FF0000>**[start, end)是左闭右开**</font>。<font color=FF0000>如果 start 是个负数</font>, 则开始索引会被自动计算成为 length+start, 其中 length 是 this 对象的 length 属性值。<font color=FF0000>如果 end 是个负数</font>, 则结束索引会被自动计算成为 length+end。

摘自：[MDN - Array.prototype.fill()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)



#### Array.prototype.copyWithin()

copyWithin() 方法<font color=FF0000>浅复制</font>数组的一部分到同一数组中的另一个位置，并返回它，<font color=FF0000>不会改变原数组的长度</font>。

- **语法**

  ```js
  arr.copyWithin(target[, start[, end]])
  ```

- **参数**
  - **target：**0 为基底的索引，复制序列到该位置。<font color=FF0000>如果是负数，target 将从末尾开始计算</font>（<mark>**自我补充：**<font color=FF0000>**经过实验**</font>，如果target为负数，且范围在 `[-arr.length, -1]`，target会变成target + arr.length；而如果target为负数，且小于 -arr.length，则不作任何操作</mark>）。<font color=FF0000>如果 target 大于等于 arr.length，将会不发生拷贝</font>。如果 target 在 start 之后，复制的序列将被修改以符合 arr.length。
  - **start：**0 为基底的索引，开始复制元素的起始位置。如果是负数，start 将从末尾开始计算。如果 start 被忽略，copyWithin 将会从0开始复制。<font color=FF0000>即：默认为0</font>。而如果start为负数，且小于 -arr.length，则不作任何操作
  - **end：**0 为基底的索引，开始复制元素的结束位置。copyWithin 将会拷贝到该位置，但<font color=FF0000>不包括 end 这个位置的元素</font>。如果是负数， end 将从末尾开始计算。如果 end 被忽略，copyWithin 方法将会一直复制至数组结尾（默认为 arr.length）。<font color=FF0000>即：默认为arr.length</font>。而如果end为负数，且小于 -arr.length，则不作任何操作
  - **自我补充：**<font color=FF0000>经过实验</font>，[start, end)是左闭右开。同时，会将索引从target开始到target+(end-start-1)的值，变成arr[start]到arr[end]的值。如果start >= end，则不进行操作
- **返回值：**改变后的数组。

- **描述：**
  - 如果 start 为负，则其指定的索引位置等同于 length+start，length 为数组的长度。end 也是如此。
  - <font color=FF0000>copyWithin 函数被设计为通用式的，其不要求其 this 值必须是一个数组对象</font>。

摘自：[MDN - Array.prototype.copyWithin()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/copyWithin)



#### Array.prototype.includes()

includes() 方法用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回false。

- **语法**

  ```js
  arr.includes(valueToFind[, fromIndex])
  ```

- **参数**
  - valueToFind：需要查找的元素值。**注意：**使用 includes()比较字符串和字符时是<font color=FF0000>区分大小写</font>。
  - fromIndex：<font color=FF0000>**可选**</font>，<font color=FF0000>从fromIndex 索引处开始查找 valueToFind</font>。<mark>如果为负值，则按升序从 array.length + fromIndex 的索引开始搜</mark> （即使从末尾开始往前跳 fromIndex 的绝对值个索引，然后往后搜寻）。<font color=FF0000>默认为 0</font>。

- **注意：**

  - NaN != NaN，且`NaN in [NaN] == false`；但是 `[1, 2, NaN].includes(NaN) === true`
  - <mark>如果 fromIndex 大于等于数组的长度，则会返回 false，且该数组不会被搜索</mark>。

  - 如果 fromIndex 为负值，计算出的索引将作为开始搜索searchElement的位置。<font color=FF0000>如果计算出的索引小于 0，则整个数组都会被搜索</font>。
  - <font color=FF0000>**includes() 方法有意设计为通用方法。它不要求this值是数组对象，所以它可以被用于其他类型的对象 (比如类数组对象)**。</font>

摘自：[MDN - Array.prototype.includes()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)



#### Array.from()

Array.from() 方法<font color=FF0000>从一个 类数组 （比如带有length属性，而没有数组的方法（比如push））或 可迭代对象 创建一个新的，浅拷贝的数组实例</font>。

**语法**

```js
Array.from(arrayLike[, mapFn[, thisArg]])
```

**参数**

- arrayLike：想要转换成数组的<font color=FF0000>**伪数组对象** 或 **可迭代对象**</font>。
- mapFn： <font color=FF0000>可选</font>，如果指定了该参数，新数组中的每个元素会执行该回调函数。
- thisArg：<font color=FF0000>可选</font>，可选参数，执行回调函数 mapFn 时 this 对象。

**返回值：**一个新的数组实例。

**Array.from() 可以通过以下方式来创建数组对象：**

- 伪数组对象（拥有一个 length 属性和若干索引属性的任意对象）
- 可迭代对象（可以获取对象中的元素,如 Map和 Set 等）

摘自：[MDN - Array.from()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/from)

**示例如下：**

```js
let arrayLike = {
  0: "Hello",
  1: "World",
  length: 2
};
const genArr = Array.from(arrayLike)
console.log(Array.isArray(genArr)) // true
```

**补充：**

Array.from() 可以通过 Array.prototype.slice( arrayLikeVarible ) 实现。

学习自：[MDN - Array.prototype.slice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)



#### Array.of()

Array.of() 方法<font color=FF0000>创建一个具有可变数量参数的新数组实例</font>，而不考虑参数的数量或类型。

 **Array.of() 和 Array 构造函数之间的区别**在于处理整数参数：<font color=FF0000>Array.of(7) 创建一个具有单个元素 7 的数组</font>，<font color=FF0000>而 Array(7) 创建一个长度为7的空数组</font>（注意：<font color=FF0000>这是指一个有7个空位(empty)的数组</font>，而不是由7个undefined组成的数组）。

- **语法**

  ```js
  Array.of(element0[, element1[, ...[, elementN]]])
  ```

- **参数**

  elementN：任意个参数，将按顺序成为返回数组中的元素。

- **返回值：**新的 Array 实例。

摘自：[MDN - Array.of()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/of)



#### Array.prototype.flat()

flat() 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。

**语法：**

```js
var newArray = arr.flat([depth])
```

**参数：**

- depth 可选，指定要提取嵌套数组的结构深度，默认值为 1。

**返回值：**一个包含将数组与子数组中所有元素的新数组。

**示例：**

```js
var arr2 = [1, 2, [3, 4, [5, 6]]];
arr2.flat(); // [1, 2, 3, 4, [5, 6]]

var arr3 = [1, 2, [3, 4, [5, 6]]];
arr3.flat(2); // [1, 2, 3, 4, 5, 6]

//使用 Infinity，可展开任意深度的嵌套数组
var arr4 = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]];
arr4.flat(Infinity); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

摘自：[MDN - Array.prototype.flat()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)

#### Array.prototype.flatMap()

<font color=FF0000>flatMap() 方法首先使用映射函数映射每个元素，**然后将结果压缩成一个新数组**</font>。<font color=FF0000>它与 map 连着深度值为1的 flat 几乎相同</font>，但 flatMap 通常在合并成一种方法的效率稍微高一些。

- **语法**

  ```js
  var new_array = arr.flatMap(function callback(currentValue[, index[, array]]) {
      // return element for new_array
  }[, thisArg])
  ```

- **参数（与Array.prototype.map()一致）**

  - **callback：**可以生成一个新数组中的元素的函数，可以传入三个参数：
    - **currentValue：**当前正在数组中处理的元素
    - **index：**<font color=FF0000>可选</font>，数组中正在处理的当前元素的索引。
    - **array：**<font color=FF0000>可选</font>，被调用的 map 数组
  - **thisArg：**<font color=FF0000>可选</font>，执行 callback 函数时 使用的this 值。

- **返回值：** 一个新的数组，其中每个元素都是回调函数的结果，并且结构深度 depth 值为1。

摘自：[MDN - Array.prototype.flatMap()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap)

**flatMap 举例：**

现在给你一个需求

```js
let arr = ["科比 詹姆斯 安东尼", "利拉德 罗斯 麦科勒姆"];
```

将上面数组转为

```js
[ '科比', '詹姆斯', '安东尼', '利拉德', '罗斯', '麦科勒姆' ]
```

第一时间想到map + flat

```js
console.log(arr.map(x => x.split(" ")).flat());
// [ '科比', '詹姆斯', '安东尼', '利拉德', '罗斯', '麦科勒姆' ]
```

<font color=FF0000>**flatMap 就是 flat + map**</font>，一个方法顶两个

```js
console.log(arr.flatMap(x => x.split(" ")));
// [ '科比', '詹姆斯', '安东尼', '利拉德', '罗斯', '麦科勒姆' ]
```

摘自：[基础很好？总结了38个ES6-ES12的开发技巧，倒要看看你能拿几分？🐶](https://juejin.cn/post/6995334897065787422)



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
  | <font color=FF0000>**toFixed()**</font> | <font color=FF0000>把数字转换为**字符串**</font>，结果的小数点后有<font color=FF0000>通过**四舍五入**指定位数的数字</font>。 |
  | toPrecision()                           | 把数字格式化为指定的长度。                                   |

- 关于日期转换为字符串的函数：
  
  | 方法                | 描述                            |
  |:----------------- |:----------------------------- |
  | getDate()         | 从 Date 对象返回一个月中的某一天 (1 ~ 31)。 |
  | getDay()          | 从 Date 对象返回一周中的某一天 (0 ~ 6)。   |
  | getFullYear()     | 从 Date 对象以四位数字返回年份。           |
  | getHours()        | 返回 Date 对象的小时 (0 ~ 23)。       |
  | getMilliseconds() | 返回 Date 对象的毫秒(0 ~ 999)。       |
  | getMinutes()      | 返回 Date 对象的分钟 (0 ~ 59)。       |
  | getMonth()        | 从 Date 对象返回月份 (0 ~ 11)。       |
  | getSeconds()      | 返回 Date 对象的秒数 (0 ~ 59)。       |
  | getTime()         | 返回 1970 年 1 月 1 日至今的毫秒数。      |
  
  更多请参考：[Date 方法](https://www.runoob.com/jsref/jsref-obj-date.html) 

<font size=4>**补充：**</font>

**将时间戳转换为‘YYYY-MM-DD HH:mm:ss’形式：**

```js
function time(time = +new Date()) {
    var date = new Date(time + 8 * 3600 * 1000); // 增加8小时
    return date.toJSON().substr(0, 19).replace('T', ' ');
}
time(); // "2018-08-09 18:25:54"
```

摘自：[一行js代码实现时间戳转时间格式](https://segmentfault.com/a/1190000015992232)



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

| 方法           | 描述                |
|:------------ |:----------------- |
| parseFloat() | 解析一个字符串，并返回一个浮点数。 |
| parseInt()   | 解析一个字符串，并返回一个整数。  |

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

console.log(auto instanceof Car); // expected output: true
console.log(auto instanceof Object); // expected output: true
```

摘自：[MDN - instanceof](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/instanceof)

**补充：**obj instanceof Class 算法的执行过程大致如下：

1. <font color=FF0000>**如果这儿有静态方法 Symbol.hasInstance，那就直接调用这个方法**</font>。例如：

   ```js
   // 设置 instanceOf 检查，并假设具有 canEat 属性的都是 animal
   class Animal {
     static [Symbol.hasInstance](obj) { if (obj.canEat) return true; }
   }
   let obj = { canEat: true };
   
   console.log(obj instanceof Animal); // true：Animal[Symbol.hasInstance](obj) 被调用
   ```

2. <font color=FF0000>**大多数 class 没有 Symbol.hasInstance**</font>。在这种情况下，标准的逻辑是：使用 obj instanceOf Class 检查 Class.prototype 是否等于 obj 的原型链中的原型之一。换句话说就是，一个接一个地比较。

摘自：[现代JS教程 - 类检查："instanceof"](https://zh.javascript.info/instanceof)

#### JS获取对象的结构：constructor

**constructor** 属性返回所有 JavaScript 变量的构造函数。示例如下：

```js
"John".constructor                 // 返回函数 String()  { [native code] }
(3.14).constructor                 // 返回函数 Number()  { [native code] }
false.constructor                  // 返回函数 Boolean() { [native code] }
[1,2,3,4].constructor              // 返回函数 Array()   { [native code] }
{name:'John', age:34}.constructor  // 返回函数 Object()  { [native code] }
new Date().constructor             // 返回函数 Date()    { [native code] }
function () {}.constructor         // 返回函数 Function(){ [native code] }
```

#### JavaScript 正则表达式

正则表达式语法：

```js
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

| 修饰符 |                           描述                           |
| :----: | :------------------------------------------------------: |
|   i    |                执行对大小写不敏感的匹配。                |
|   g    | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。 |
|   m    |                      执行多行匹配。                      |

**使用 RegExp 对象**

在 JavaScript 中，RegExp 对象是一个预定义了属性和方法的正则表达式对象。

- **test()** 方法<font color=FF0000>用于检测一个字符串是否匹配某个模式</font>，如果字符串中含有匹配的文本，则返回 true，否则返回 false。

  **RegExp.prototype.test()**

  test() 方法执行一个检索，用来<font color=FF0000>查看正则表达式与指定的字符串是否匹配</font>。<font color=FF0000>返回 true 或 false</font>。

  - **语法**

    ```js
    regexObj.test(str)
    ```

  - **参数**

    - str：用来与正则表达式匹配的字符串

  - **返回值：**如果正则表达式与指定的字符串匹配 ，返回true；否则false。

  - **描述**
    <mark>当你想要知道一个正则表达式是否与指定的字符串匹配时，就可以使用 test()（类似于 String.prototype.search() 方法）</mark>，<font color=FF0000>差别在于test返回一个布尔值，而 search 返回索引</font><mark>（如果找到）或者-1（如果没找到）</mark>；<mark>若想知道更多信息（然而执行比较慢）</mark>，<font color=FF0000>可使用exec() 方法（类似于 String.prototype.match() 方法）</font>。 和 exec() (或者组合使用),一样，在相同的全局正则表达式实例上多次调用test将会越过之前的匹配。

  摘自：[MDN - RegExp.prototype.test()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test)

- **exec()** 方法是一个正则表达式方法。exec() 方法用于检索字符串中的正则表达式的匹配。
  
  <font color=FF0000>**该函数返回一个数组**，其中存放匹配的结果</font>。<font color=FF0000>如果未找到匹配，则返回值为 null</font>。

<font size=4>**补充**</font>

**以上内容均是ES5的内容，ES6中又进行了补充：**

- **y修饰符（粘连修饰符）：**修饰符g是每次匹配剩余的字符串，而修饰符y是从剩余的<font color=FF0000>第一个字符</font>开始匹配，如果找不到，则继续从头开始找
- **u修饰符：**用unicode的模式去匹配，使用u修饰符可以突破\u0000-\uffff的范围

**ES9再次进行了补充：**

- **dotAll（s修饰符）：**正则表达式中的`.`用于匹配任意单个字符，但是不能用于匹配 多字符的unicode 和 行终结符、行分隔符（\n，\r、\u{2028}、\u{2029}）、段终结符。ES9中加入了dotAll，即s修饰符，`.`加上`s`即可匹配所有的单个字符

  **RegExp.prototype.dotAll**

  dotAll 属性<font color=FF0000>表明是否在正则表达式中一起使用"s"修饰符</font>（<mark>引入/s修饰符，使得.可以匹配任意单个字符</mark>）。dotAll 是一个<font color=FF0000>只读的属性</font>，属于单个正则表达式实例。

  | RegExp.prototype.dotAll 属性的属性特性： | 值    |
  | :--------------------------------------- | ----- |
  | writable                                 | false |
  | enumerable                               | false |
  | configurable                             | true  |

  **描述**

  <font color=FF0000 size=4>**如果使用了"s"修饰符，dotAll 的值将返回Boolean类型的true，否则将返回false**</font>。<mark>"s"修饰符表示，特殊字符"."应另外匹配字符串中的下述行终结符</mark>（line terminator characters），否则将会失配：

  - U+000A 换行符（"\n"）
  - U+000D 回车符（"\r"）
  - U+2028 行分隔符（line separator）
  - U+2029 段分隔符（paragraph separator）

  这实际上意味着"."将会匹配任意的单个Unicode Basic Multilingual Plane (BMP)字符。若要使其与astral字符（大于\uFFFF的Unicode字符）匹配，你应当使用"u"（Unicode）修饰符。一起使用这两个修饰符，"."将无一例外地匹配任意Unicode字符。

  摘自：[MDN - RegExp.prototype.dotAll](https://developer.mozilla.org/zh-CN/docs/orphaned/Web/JavaScript/Reference/Global_Objects/RegExp/dotAll)

- **具名组匹配：**

- **后行断言：**



#### RegExp(正则表达式)

RegExp 对象用于将文本与一个模式匹配。

字面量和构造函数，有两种方法可以创建一个 RegExp 对象：一种是字面量，另一种是构造函数。

- **字面量：**由斜杠(/)包围而不是引号包围。
- **构造函数的字符串参数：**由引号而不是斜杠包围。

**以下三种表达式都会创建相同的正则表达式：**

```js
let regexp = '/ab+c/i'; //字面量形式
let regexpObj = new RegExp('ab+c', 'i'); // 首个参数为字符串模式的构造函数
let regexpObj2 = new RegExp(/ab+c/, 'i'); // 首个参数为常规字面量的构造函数
```

当表达式被赋值时，<font color=FF0000><font size=4>**字面量形式提供**</font>正则表达式的编译（compilation）状态</font>，当正则表达式保持为常量时使用字面量。<font color=FF0000>例如当你在循环中使用字面量构造一个正则表达式时，**正则表达式不会在每一次迭代中都被重新编译**（recompiled）</font>。

而正则表达式对象的<font color=FF0000><font size=4>**构造函数**</font>，如 new RegExp('ab+c') 提供了正则表达式运行时编译（runtime compilation）</font>。<mark style=background-color:hotpink>**如果你知道正则表达式模式将会改变，或者你事先不知道什么模式，而是从另一个来源获取，如用户输入，这些情况都可以使用构造函数**</mark>。

**Perl-like RegExp 属性**
请注意，RegExp属性有长名称和短名称（类似Perl）。两个名称总是引用同一个值（<font color=FF0000>**Perl是JavaScript为其正则表达式建模的编程语言**</font>）

- **构造函数**
  - **RegExp()：**创建一个新的 RegExp 对象。
- **静态属性**
  - **get RegExp[@@species]：**该构造函数<font color=FF0000>用于创建派生对象</font>。
  - **RegExp.lastIndex：**<font color=FF0000>该**索引**表示从哪里开始下一个匹配</font>
- **实例属性**
  - **RegExp.prototype.flags：**含有 RegExp 对象 <font color=FF0000>flags 的字符串</font>。（即修饰符）
  - **RegExp.prototype.dotAll：**<font color=FF0000>"."是否要匹配新行（newlines）</font>。
  - **RegExp.prototype.global：**针对字符串中所有可能的匹配项测试正则表达式，还是仅针对第一个匹配项。
  - **RegExp.prototype.ignoreCase：**匹配文本的时候是否忽略大小写。
  - **RegExp.prototype.multiline：**是否进行多行搜索。
  - **RegExp.prototype.source：**正则表达式的文本。
  - **RegExp.prototype.sticky：**搜索是否是 sticky。
  - **RegExp.prototype.unicode：**Unicode 功能是否开启。
- **实例方法**
  - **RegExp.prototype.compile()：**🗑<font color=FF0000> 运行脚本的期间（重新）编译正则表达式</font>。（已废除）
  - **RegExp.prototype.exec()：**<font color=FF0000>在该字符串中执行匹配项的搜索</font>。
  - **RegExp.prototype.test()：**该正则<font color=FF0000>在字符串里是否有匹配</font>。
  - **RegExp.prototype\[@@match]()：**对给定字符串执行匹配并返回匹配结果。
  - **RegExp.prototype\[@@matchAll]()：**对给定字符串执行匹配，返回所有匹配结果。
  - **RegExp.prototype\[@@replace]()：**给定新的子串，替换所有匹配结果。
  - **RegExp.prototype\[@@search]()：**在给定字符串中搜索匹配项，并返回在字符串中找到字符索引。
  - **RegExp.prototype\[@@split]()：**通过<font color=FF0000> 将给定字符串拆分为子字符串</font>，<font color=FF0000> 并返回字符串形成的数组</font>。
  - **RegExp.prototype.toString()：**返回表示指定对象的字符串。<font color=FF0000> **重写Object.prototype.toString()方法**</font>。

摘自：[MDN - RegExp(正则表达式)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)

#### String.prototype.match()

match() 方法<font color=FF0000> 检索返回一个字符串匹配正则表达式的结果</font>。

**语法**

```js
str.match(regexp)
```

**参数**

- **regexp：**一个正则表达式对象。<font color=FF0000> 如果传入一个**非正则表达式对象**，则会隐式地使用 new RegExp(obj) 将其转换为一个 RegExp</font>。<mark>如果你<font color=FF0000> 没有给出任何参数并直接使用match() 方法</font> ，你将会得到一 个包含空字符串的 Array ：[""] </mark>。

**返回值**

- **如果<font color=FF0000> 使用g</font>标志**，则将<font color=FF0000> 返回与完整正则表达式匹配的所有结果</font>，但<font color=FF0000> 不会返回捕获组</font>。

- **如果<font color=FF0000> 未使用g</font>标志**，则<font color=FF0000> **仅返回第一个完整匹配及其相关的捕获组（Array）**</font>(这里的Array和matchAll形成了对比）。 在这种情况下，返回的项目将具有如下所述的其他属性。

  **附加属性：**如上所述，匹配的结果包含如下所述的附加特性。

  - **groups:** <font color=FF0000> 一个捕获组数组</font> 或 <font color=FF0000> undefined（如果没有定义命名捕获组）</font>。
  - **index:** <font color=FF0000> 匹配的结果的开始位置</font>
  - **input:** <font color=FF0000> 搜索的字符串</font>.

一个Array，其内容取决于global（g）标志的存在与否，如果未找到匹配则为null。

<mark style=background-color:hotpink>**描述：**如果正则表达式不包含 g 标志，str.match() 将返回与 RegExp.exec() 相同的结果</mark>。

**比较RegExp的方法：**

- 如果你需要知道<font color=FF0000> 一个字符串是否与一个正则表达式匹配 RegExp</font> ，<font color=FF0000> 可使用 RegExp.test() </font>。

- 如果你<font color=FF0000> 只是需要第一个匹配结果</font>，你<font color=FF0000> 也可以使用 RegExp.exec() </font>。

- 如果你<font color=FF0000> 想要获得捕获组</font>，<font color=FF0000> 并且设置了全局标志</font>，你<font color=FF0000> 需要用 RegExp.exec()  或者  String.prototype.matchAll()</font>

摘自：[MDN - String.prototype.match()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/match)



#### throw

throw 出去的异常将会被 catch 捕捉到，放（拼接）在 catch 的参数中。

#### Error

通过 Error 的构造器可以创建一个错误对象。当运行时错误产生时，Error 的实例对象会被抛出。Error 对象也可用于用户自定义的异常的基础对象。

##### 语法

```js
new Error([message[, fileName[,lineNumber]]])
```

**参数**

- **message：**可选。人类可阅读的错误描述信息。
- **fileName：**⚠️可选。被创建的Error对象的fileName属性值。默认是调用Error构造器代码所在的文件 的名字。
- **lineNumber：**⚠️可选。被创建的Error对象的lineNumber属性值。默认是调用Error构造器代码所在的文件的行号。

##### 作为函数使用

当像函数一样使用 Error 时：如果没有 new，它将返回一个 Error 对象。所以，<font color=FF0000>仅仅调用 Error 产生的结果与通过 new 关键字构造 Error 对象生成的结果相同</font>。 

```js
// this:
const x = Error('I was created using a function call!');
// has the same functionality as this:
const y = new Error('I was constructed via the "new" keyword!');
```

##### Error 类型

除了通用的 Error 构造函数外，<font color=FF0000>JavaScript 还有 6 个其他类型的错误构造函数</font>。

- **EvalError：**创建一个 error 实例，表示错误的原因：与 eval()  有关。
- **InternalError⚠️：**创建一个代表 Javascript 引擎内部错误的异常抛出的实例。 如: "递归太多".
- **RangeError：**创建一个 error 实例，表示错误的原因：数值变量或参数超出其有效范围。
- **ReferenceError：**创建一个 error 实例，表示错误的原因：无效引用。
- **SyntaxError：**创建一个 error 实例，表示错误的原因：eval() 在解析代码的过程中发生的语法错误。
- **TypeError：**创建一个error 实例，表示错误的原因：变量或参数不属于有效类型。
- **URIError：**创建一个error 实例，表示错误的原因：给 encodeURI() 或  decodeURI() 传递的参数无效。

另外，稳重还提到了 **AggregateError**：

> **AggregateError** 当多个错误需要包装在一个错误中时，该对象表示一个错误。
>
> ##### 语法
>
> ```js
> new AggregateError(errors[, message])
> ```
>
> **参数**
>
> - **errors：**错误的描述，默认为空。
> - **message：**可选，AggregateError 错误的提示信息。
>
> 摘自：[MDN - AggregateError](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/AggregateError)

摘自：[MDN - Error](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Error)



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

<font size=4>**补充：**</font>

在定义类时，如果想要定义的属性为变量（即属性名可以修改），可以使用中括号包裹（即计算属性）



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

#### HTML 约束验证

HTML5 新增了 HTML 表单的验证方式：约束验证（constraint validation）。约束验证是表单被提交时浏览器用来实现验证的一种算法。

**HTML 约束验证基于：**

- **HTML 输入属性**
  
  | 属性       | 描述           |
  |:-------- |:------------ |
  | disabled | 规定输入的元素不可用   |
  | max      | 规定输入元素的最大值   |
  | min      | 规定输入元素的最小值   |
  | pattern  | 规定输入元素值的模式   |
  | required | 规定输入元素字段是必需的 |
  | type     | 规定输入元素的类型    |
  
  完整列表，请查看 [HTML 输入属性](https://www.runoob.com/html/html5-form-attributes.html)。

- **CSS 伪类选择器**
  
  | 选择器       | 描述                            |
  |:--------- |:----------------------------- |
  | :disabled | 选取属性为 "disabled" 属性的 input 元素 |
  | :invalid  | 选取无效的 input 元素                |
  | :optional | 选择没有"required"属性的 input 元素    |
  | :required | 选择有"required"属性的 input 元素     |
  | :valid    | 选取有效值的 input 元素               |
  
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
| setCustomValidity() | 设置 input 元素的 validationMessage 属性，用于自定义错误提示信息的方法。使用 setCustomValidity 设置了自定义提示后，validity.customError 就会变成true，则 checkValidity 总是会返回false。如果要重新判断需要取消自定义提示，方式如下：<br>setCustomValidity('')<br>setCustomValidity(null)<br>setCustomValidity(undefined) |

**约束验证 DOM 属性**

| 属性                | 描述                     |
|:----------------- |:---------------------- |
| validity          | 布尔属性值，返回 input 输入值是否合法 |
| validationMessage | 浏览器错误提示信息              |
| willValidate      | 指定 input 是否需要验证        |

**Validity 属性**

input 元素的 **validity 属性**包含一系列关于 validity 数据属性:

| 属性              | 描述                                     |
|:--------------- |:-------------------------------------- |
| customError     | 设置为 true, 如果设置了自定义的 validity 信息。       |
| patternMismatch | 设置为 true, 如果元素的值不匹配它的模式属性。             |
| rangeOverflow   | 设置为 true, 如果元素的值大于设置的最大值。              |
| rangeUnderflow  | 设置为 true, 如果元素的值小于它的最小值。               |
| stepMismatch    | 设置为 true, 如果元素的值不是按照规定的 step 属性设置。     |
| tooLong         | 设置为 true, 如果元素的值超过了 maxLength 属性设置的长度。 |
| typeMismatch    | 设置为 true, 如果元素的值不是预期相匹配的类型。            |
| valueMissing    | 设置为 true，如果元素 (required 属性) 没有值。       |
| valid           | 设置为 true，如果元素的值是合法的。                   |

#### JavaScript关键字

##### JavaScript 保留关键字

Javascript 的保留关键字不可以用作变量、标签或者函数名。有些保留关键字是作为 Javascript 以后扩展使用。

|          |           |            |           |              |
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

##### JavaScript this关键字

<font color=FF0000>JavaScript 中 this 不是固定不变的，它会随着执行环境的改变而改变</font>。

- 在**对象方法**中，this 表示**该方法所属的对象**。
- 如果**单独使用**，this 表示**全局对象**（在严格模式下也是这样）。
- 在**函数**中，this 表示**全局对象**。
- 在**函数**中，在**严格模式**下，**this 是未定义的 **(undefined)。
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

| 函数             | 描述                                           |
| :--------------- | :--------------------------------------------- |
| JSON.parse()     | 用于将一个 JSON 字符串转换为 JavaScript 对象。 |
| JSON.stringify() | 用于将 JavaScript 值转换为 JSON 字符串。       |

#### JSON.stringify()

JSON.stringify() 方法将一个 JavaScript 对象或值转换为 JSON 字符串。<font color=FF0000>如果**指定了一个 replacer 函数**</font>，则<font color=FF0000>**可以选择性地 替换值**</font>；<font color=FF0000>**指定的 replacer 是数组**</font>，则<font color=FF0000>**可选择性地 仅包含数组指定的属性**</font>

##### 语法

```
JSON.stringify(value[, replacer [, space]])
```

**参数**

- **value**：将要序列化成 一个 JSON 字符串的值。
- **replacer**：<mark>可选</mark>
  - 如果该参数是一个<font color=FF0000>函数</font>，则在序列化过程中，被序列化的值的每个属性（对象的 key ）都会经过该函数的转换和处理
  - 如果该参数是一个<font color=FF0000>数组</font>，则只有包含在这个数组中的属性名才会被序列化到最终的 JSON 字符串中
  - 如果该参数 <font color=FF0000>为 null 或 未提供</font>，则对象所有的属性都会被序列化。
- **space**：<mark>可选</mark>。<font color=FF0000>指定缩进用的空白字符串，用于美化输出</font> ( pretty-print )
  - 如果参数是个<font color=FF0000>数字</font>，它代表有多少的空格，上限为 10。该值若小于 1，则意味着没有空格
  - 如果该参数为<font color=FF0000>字符串</font>（当字符串长度超过 10 个字母，取其前 10 个字母），该字符串将被作为空格
  - 如果该参数 <font color=FF0000>为 null 或 没有提供</font>，将没有空格。

**返回值**：一个表示给定值的 JSON 字符串。

**异常**

- 当在循环引用时会抛出异常 `TypeError ("cyclic object value")`（循环对象值）
- 当尝试去转换 BigInt 类型的值会抛出 `TypeError ("BigInt value can't be serialized in JSON")` <font color=FF0000>**BigInt 值不能 JSON 序列化**</font>

##### JSON.stringify() 将值转换为相应的 JSON 格式

- <font color=FF0000>转换值如果有 toJSON() 方法，该方法定义什么值将被序列化</font>
- **非数组对象的属性** <font color=FF0000>不能保证以特定的顺序</font>出现在序列化后的字符串中。
- **布尔值、数字、字符串的包装对象** 在序列化过程中会自动转换成对应的原始值。
- **undefined、任意的函数 以及 symbol 值**，在序列化过程中会被忽略（出现在非数组对象的属性值中时）或 被转换成 null（出现在数组中时）。函数、undefined 被 **单独转换** 时，会返回 undefined。
- 对 **包含循环引用的对象** 执行此方法，会抛出错误。
- <font color=FF0000>所有**以 symbol 为属性键的属性** 都会被完全忽略掉</font>，**即便 replacer 参数中强制指定包含了它们**。
- <font color=FF0000>Date 日期调用了 toJSON() 将其转换为了 string 字符串</font>（ 同 Date.toISOString() ），因此<font color=FF0000>会被当做字符串处理</font>
- <font color=FF0000>NaN 和 Infinity 格式的数值 及 null 都会被当做 null</font>
- 其他类型的对象，包括 Map / Set / WeakMap / WeakSet，仅<font color=FF0000>会序列化可枚举的属性</font>。

摘自：[MDN - JSON.stringify()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)



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

JavaScript 中的<font color=FF0000>异步操作函数往往通过**回调函数**来实现异步任务的结果处理</font>。补充：可以说，回调函数 (callback) 是 JavaScript异步编程的基础。

<font color=FF0000>回调函数</font>就是一个函数，它是在我们启动一个异步任务的时候就告诉它：等你完成了这个任务之后要干什么。这样一来主线程几乎不用关心异步任务的状态了，他自己会善始善终。

示例：

```js
setTimeout(function () {
    document.getElementById("demo").innerHTML="RUNOOB!";
}, 3000);
```

这个函数执行之后会产生一个子线程，子线程会等待 3 秒，然后执行回调函数 "print"，在命令行输出 "RUNOOB!"。

#### Promise

Promise 是一个 ECMAScript 6 提供的类，目的是更加优雅地书写复杂的异步任务。<font color=FF0000>本质上 Promise 是一个函数返回的对象，对象用于表示一个**异步操作**的最终完成 （或失败）及其结果值</font>。

**补充：**一个 Promise 对象代表一个在这个 promise 被创建出来时不一定已知的值。它让您能够把异步操作最终的成功返回值或者失败原因和相应的处理程序关联起来。 这样使得异步方法可以像同步方法那样返回值：<font color=FF0000>异步方法并不会立即返回最终的值，而是会返回一个 promise，以便在未来某个时候把值交给使用者。</font>

**一个 Promise <font color=FF0000><font size=4>必然</font> 处于以下几种状态之一</font>：**

- **待定（pending）：**<font color=FF0000>初始状态</font>，<font color=FF0000>既没有被兑现，也没有被拒绝</font>。
- **已兑现（fulfilled）：**意味着<font color=FF0000>操作成功完成</font>。
- **已拒绝（rejected）：** 意味着<font color=FF0000>操作失败</font>。

<font color=FF0000>待定（pending）状态的 Promise 对象 **要么** 会通过一个值被兑现（fulfilled），**要么** 会通过一个原因（错误）被拒绝（rejected）</font>。

**另外：** 如果一个 promise 已经被兑现（fulfilled）或被拒绝（rejected），那么我们也可以说它处于已敲定（settled）状态

<font size=4>**构造 Promise**</font>

新建一个 Promise 对象：

```js
new Promise(function (resolve, reject) {
    // code
});
```

Promise 构造函数只有一个参数，是一个函数（即上面的function），这个函数在构造之后会直接被异步运行，所以我们称之为<font color=FF0000>起始函数</font>。起始函数<font color=FF0000>包含两个参数 resolve 和 reject</font>。其中，<font color=FF0000>resolve 和 reject 都是函数</font>，其中<font color=FF0000>调用 resolve 代表一切正常</font>，<font color=FF0000>reject 是出现异常时所调用的</font>

Promise 类有 **`.then()`** **` .catch()`** 和 **`.finally()`** 三个方法，<font color=FF0000>这三个方法的参数都是一个函数</font>

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

<font color=FF0000>**任何不是 throw 的终止都会创建一个"已决议 ( resolved )"状态，而以 throw 终止则会创建一个"已拒绝 ( rejected )"状态。**</font>

另外：下面的代码，可用更好的的方法替代：

```js
const myPromise =
  (new Promise(myExecutorFunc))
  .then(handleFulfilledA,handleRejectedA)
  .then(handleFulfilledB,handleRejectedB)
  .then(handleFulfilledC,handleRejectedC);
```

**替代方案：**（注：这样就不用写多个 handleRejected 了）

```js
const myPromise =
  (new Promise(myExecutorFunc))
  .then(handleFulfilledA)
  .then(handleFulfilledB)
  .then(handleFulfilledC)
  .catch(handleRejectedAny);
```

**在使用 Promise 时，会有以下约定：**

- 在本轮 事件循环 运行完成之前，回调函数是不会被调用的。
- 即使异步操作已经完成（成功或失败），在这之后通过 then() 添加的回调函数也会被调用。
- <font color=FF0000>通过**多次调用 then()** 可以**添加多个回调函数**，它们会按照插入顺序进行执行</font>。

<font size=4>**Promise 很棒的一点就是链式调用（chaining）：**</font>

<font color=FF0000>连续执行两个或者多个异步操作是一个常见的需求，在上一个操作执行成功之后，开始下一个的操作，并带着上一步操作所返回的结果</font>。我们可以通过创造一个 **Promise 链**来实现这种需求。<font color=FF0000>then() 函数会返回一个**和原来不同的新的 Promise**</font>。

> **注意：**执行的函数一定要有返回值，否则，callback 将无法获取上一个 Promise 的结果（如果使用箭头函数，() => x 比 () => { return x; } 更简洁一些，但后一种保留 return 的写法才支持使用多个语句）。

**补充：**因为 <font color=FF0000>Promise.prototype.then 和 Promise.prototype.catch 方法 **返回的是 promise**， 所以它们可以被链式调用</font>。

![原链接：https://mdn.mozillademos.org/files/8633/promises.png](https://i.loli.net/2021/01/05/WDzh3PxfvlRNqCp.png)

链式调用中的 promise 们就像俄罗斯套娃一样，是嵌套起来的，但又像是一个栈，每个都必须从顶端被弹出。链式调用中的第一个 promise 是嵌套最深的一个，也将是第一个被弹出的。

<font size=4>**构造函数：**</font>

**Promise()：**<font color=FF0000>创建一个新的 Promise 对象</font>。该构造函数主要用于包装还没有添加 promise 支持的函数。

<font size=4>**组合：**</font>

<font color=FF0000>Promise.all(iterable) 和 Promise.race(iterable) 是并行运行异步操作的两个组合式工具</font>。

我们可以发起并行操作，然后等多个操作全部结束后进行下一步操作，如下：

```js
Promise.all([func1(), func2(), func3()])
.then(([result1, result2, result3]) => { /* use result1, result2 and result3 */ });
```

可以使用一些聪明的 JavaScript 写法实现<font color=FF0000>时序组合</font>：

```js
[func1, func2, func3].reduce((p, f) => p.then(f), Promise.resolve())
.then(result3 => { /* use result3 */ });
```

<font size=4>**静态方法**</font>

- <font size=4>**Promise.all(iterable)：**</font><font color=FF0000>这个方法返回一个新的 promise对象：**该promise对象 在 iterable参数对象里（的） <font size=4>所有 promise对象都成功的时候才会触发成功</font>**，**一旦有任何一个 iterable 里面的 promise 对象失败则立即触发该promise对象的失败**</font>。（<mark>注：感觉说成 “当所有的 promise 都 fulfilled 才会 返回 promise” 更容易理解些</mark>）
  
  这个新的 promise 对象在 <font color=FF0000>**触发成功状态**</font> 以后，<font color=FF0000>会把一个包含 iterable 里所有 promise 返回值的数组作为成功回调的返回值</font>，<font color=FF0000 size=4>**顺序跟iterable的顺序保持一致**</font>； **如果这个新的 promise对象 <font color=FF0000>触发了失败状态</font>，<font color=FF0000>它会把 iterable 里第一个触发失败的 promise对象的错误信息作为它的失败错误信息</font>**。Promise.all方法常被用于处理多个promise对象的状态集合。（可以参考jQuery.when方法---译者注）

- <font size=4>**Promise.allSettled(iterable)：**</font>等到所有promises都 **已敲定（settled）**（<font color=FF0000>每个promise都已兑现（fulfilled）或已拒绝（rejected）</font>）。 **返回一个 promise**，该 promise 在所有 promise 完成后完成。<font color=FF0000>并带有一个对象数组，每个对象对应每个promise的结果</font>。

  **补充：**摘自：[MDN - Promise.allSettled()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)

  <font color=FF0000>Promise.allSettled() 方法返回一个 在所有给定的promise 都已经 fulfilled 或 rejected 后的 promise，**并带有一个对象数组，每个对象表示对应的 promise 结果**。</font>

  <mark style="background: aqua">当您有多个彼此不依赖的异步任务成功完成时，或者您总是想知道每个promise的结果时，通常使用它。</mark>

  <mark>相比之下，Promise.all() 更适合彼此相互依赖或者在其中任何一个reject时立即结束。</mark>

  - **语法**

    ```js
    Promise.allSettled(iterable);
    ```

  - **参数**

    **iterable：**一个可迭代的对象，例如Array，其中<font color=FF0000>每个成员都是 Promise</font>。

  - **返回值**
    一旦所指定的 promises 集合中每一个 promise 已经完成，<font color=FF0000>无论是成功的达成或被拒绝，未决议的 Promise将被异步完成</font>。那时，所返回的 promise 的处理器将传入一个数组作为输入，该数组包含原始 promises 集中每个 promise 的结果。

    <mark>对于每个结果对象，都有一个 status 字符串。如果它的值为 fulfilled，则结果对象上存在一个 value 。如果值为 rejected，则存在一个 reason 。value（或 reason ）反映了每个 promise 决议（或拒绝）的值</mark>。

  - **示例：**

    ```js
    const promise1 = Promise.resolve(3);
    const promise2 = new Promise((resolve, reject) => setTimeout(reject, 100, 'foo'));
    const promises = [promise1, promise2];
    
    Promise.allSettled(promises).
      then((results) => results.forEach((result) => console.log(result.status)));
    // expected output:
    // "fulfilled"
    // "rejected"
    ```
    
  - **群友写的简单实现：**

    ```js
    const promiseAllSettled = (arr) => {
      const res = []
      let count = 0
      return new Promise((resolve, reject) => {
        arr.forEach((item, index) => {
          item().then(r => Promise.resolve(r))
          .catch(e => Promise.resolve(e))
          .then(r => {
            res[index] = r
            if(++count === arr.length) { resolve(res) }
          })
        })
      })
    }
    ```

- <font size=4>**Promise.any(iterable)：**</font>接收一个Promise对象的集合，<font color=FF0000>当其中的一个 promise **成功**，**就返回那个成功的promise的值**</font>。（**注：**注意和 Promise.race 的区别。另外，有点类似于 Array.prototype.some() ）

- <font size=4>**Promise.race(iterable)：**</font>当 iterable参数 里的 <font color=FF0000>任意一个子promise被**成功或失败**后</font>，父promise 马上也会用 子promise 的成功返回值 或 失败详情作为参数调用 父promise绑定的相应句柄，并返回该promise对象<mark>（注：这里说的让人困惑，看下面的详解）</mark>。

  **补充：**摘自 [MDN - Promise.race()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/race)

  Promise.race(iterable) 方法返回一个 promise，<font color=FF0000 size=4>**一旦迭代器中的 某个promise解决或拒绝，返回的 promise就会解决或拒绝**</font>。（**注：**可以理解为：任意一个promise settled了，直接返回这个promise的状态( fulfilled / rejected )作为整个 iterable 的结果）

  - **语法：**

    ```js
    Promise.race(iterable);
    ```

  - **参数：**
    iterable：可迭代对象，类似Array。详见 iterable。

  - **返回值：**一个待定的 Promise 只要给定的迭代中的一个promise解决或拒绝，就采用第一个promise的值作为它的值，从而异步地解析或拒绝（一旦堆栈为空）。

  - **描述：**race 函数返回一个 Promise，它将与第一个传递的 promise 相同的完成方式被完成。它可以是完成（ resolves ），也可以是失败（rejects），这要取决于第一个完成的方式是两个中的哪个。

    <font color=FF0000>如果传的迭代是空的，则返回的 promise 将永远等待</font>。<font color=FF0000>如果迭代包含一个或多个非承诺值和/或已解决/拒绝的承诺，则 **Promise.race 将解析为迭代中找到的第一个值**</font>。

  - **示例：**

    ```js
    const promise1 = new Promise((resolve, reject) => {
      setTimeout(resolve, 500, 'one');
    });
    const promise2 = new Promise((resolve, reject) => {
      setTimeout(resolve, 100, 'two');
    });
    
    Promise.race([promise1, promise2]).then((value) => {
      console.log(value); // Both resolve, but promise2 is faster
    });
    // expected output: "two"
    ```

- <font size=4>**Promise.reject(reason)：**</font><font color=FF0000>返回一个**状态为失败**的Promise对象</font>，并将给定的失败信息传递给对应的处理方法

- <font size=4>**Promise.resolve(value)：**</font><font color=FF0000>返回一个状态由给定value、决定的Promise对象</font>。<mark>如果该值是thenable（即，带有then方法的对象），返回的Promise对象的最终状态由then方法执行决定</mark>；否则的话（该value为空，基本类型或者不带then方法的对象），返回的 Promise对象状态为fulfilled，并且将 该value传递给对应的 then方法。<mark>通常而言，如果您不知道一个值是否是Promise对象，使用 Promise.resolve(value) 来返回一个Promise对象，这样就能将该value以Promise对象形式使用</mark>。

<font size=4>**Promise 原型**</font>

- **实例属性：**
  
  **Promise.prototype.constructor** 返回被创建的实例函数.  默认为 Promise 函数.

- **实例方法**
  
  - **Promise.prototype.catch(onRejected)：**<font color=FF0000>添加一个拒绝 ( rejection ) 回调到当前 promise，返回一个新的 promise</font>。当这个回调函数被调用，新 promise 将以它的返回值来resolve，否则如果当前promise 进入fulfilled状态，则以当前promise的完成结果作为新promise 的完成结果。
  - **Promise.prototype.then(onFulfilled, onRejected)：**<font color=FF0000>添加解决 ( fulfillment ) 和拒绝 ( rejection ）回调到当前 promise</font>，返回一个新的 promise，将以回调的返回值来resolve。
  - **Promise.prototype.finally(onFinally)：**添加一个事件处理回调于当前 promise对象，并且在原promise对象解析完毕后，返回一个新的promise对象。回调会在当前promise运行完毕后被调用，无论当前promise的状态是完成(fulfilled)还是失败(rejected)

<font size=4>**创建Promise**</font>

<mark>Promise 对象是由关键字 new 及其构造函数来创建的</mark>。<font color=FF0000>该构造函数会把一个叫做“处理器函数”（executor function）的函数作为它的参数</font>。<mark>这个“处理器函数”接受两个函数——resolve 和 reject ——作为其参数</mark>。当异步任务顺利完成且返回结果值时，会调用 resolve 函数；而当异步任务失败且返回失败原因（通常是一个错误对象）时，会调用 reject 函数。

```js
const myFirstPromise = new Promise((resolve, reject) => {
  // ?做一些异步操作，最终会调用下面两者之一:
  //
  //   resolve(someValue); // fulfilled
  // ?或
  //   reject("failure reason"); // rejected
});
```

<font color=FF0000>想要某个函数拥有promise功能，只需让其返回一个promise即可。</font>

```js
function myAsyncFunction(url) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.onload = () => resolve(xhr.responseText);
    xhr.onerror = () => reject(xhr.statusText);
    xhr.send();
  });
};
```

**以上补充内容摘自：**[MDN - 使用 Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises)  和  [MDN - Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise) 另外，还有一些东西，在两篇MDN教程中都没有找到，应该是学习自：慕课网课程：[再学JavaScript ES(6-11)全版本语法大全](https://coding.imooc.com/class/444.html)

以下补充摘自：[ECMAScript 6 入门 - Promise 对象](https://es6.ruanyifeng.com/#docs/promise)

**Promise也有一些缺点**

- 无法取消Promise，一旦新建它就会立即执行，无法中途取消。
- 如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部。
- 当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

如果某些事件不断地反复发生，一般来说，使用 Stream 模式（注：是Node 的 Stream）是比部署Promise更好的选择。

**( Promise的 ) 基本用法：**
Promise构造函数接受一个函数作为参数，该函数的两个参数分别是 <font color=FF0000>resolve 和 reject</font>。<font color=FF0000>它们是两个函数，<font size=4>**由 JavaScript 引擎提供，不用自己部署**</font></font>。
**resolve 函数的作用是：**<font color=FF0000>将Promise对象的状态从“未完成”变为“成功”</font>（即从 pending 变为 resolved），<font color=FF0000>在异步操作成功时调用，并 <font size=4>**将异步操作的结果，作为参数传递出去**</font></font>。**reject函数的作用是：**<font color=FF0000>将Promise对象的状态从“未完成”变为“失败”</font>（即从 pending 变为 rejected），<font color=FF0000>在异步操作失败时调用，并 <font size=4>**将异步操作报出的错误，作为参数传递出去**</font></font>。示例如下：

```js
// 异步加载图片的例子
function loadImageAsync(url) {
  return new Promise(function(resolve, reject) {
    const image = new Image();

    // 成功了就 用resolve传递 image
    image.onload = function() {
      resolve(image);
    };
    // 失败了就 **抛出异常**
    image.onerror = function() {
      reject(new Error('Could not load image at ' + url));
    };

    image.src = url;
  });
}
```

Promise实例生成以后，<font color=FF0000>可以用 then 方法分别指定 resolved状态 和 rejected状态 的回调函数</font>。（<mark>**注：感觉这里MDN 没有 阮一峰讲的清楚**</mark>）

```js
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```

then方法可以<font color=FF0000>接受两个回调函数作为参数</font>。<font color=FF0000>**第一个回调函数 是Promise对象的状态变为 <font size=4>resolved时调用</font> **</font>，<font color=FF0000>**第二个回调函数是Promise对象的状态变为 <font size=4>rejected时调用</font>**</font>。<font color=FF0000>**这两个函数都是可选的，不一定要提供**</font>。它们都接受Promise对象传出的值作为参数。

**resolve 函数的参数除了正常的值以外，<font color=FF0000>还可能是另一个 Promise 实例</font>**，比如像下面这样。

```js
const p1 = new Promise(function (resolve, reject) {
  // ...
});
const p2 = new Promise(function (resolve, reject) {
  // resolve 传递 一个 promise 实例 p1
  resolve(p1);
})
```

注意，<font color=FF0000>这时p1的状态就会传递给p2，也就是说，**p1的状态决定了p2的状态**</font>。<font color=FF0000>如果**p1的状态是 pending**，那么 <font size=4>**p2 的回调函数就会等待 p1的状态改变**</font>；如果 **p1 的状态已经是 resolved 或者 rejected**，那么 <font size=4>**p2 的回调函数将会立刻执行**</font></font>。如下示例：

```js
const p1 = new Promise(function (resolve, reject) {
  setTimeout(() => reject(new Error('fail')), 3000)
})

const p2 = new Promise(function (resolve, reject) {
  setTimeout(() => resolve(p1), 1000)
})

p2.then(result => console.log(result))
  .catch(error => console.log(error))
// 结果： Error: fail （注：在Node上运行该脚本，会在打印结果后报错；但在chrome控制台上没有报错）；
```

<font color=FF0000 size=4>**注意，调用 resolve 或 reject 并不会终结 Promise 的参数函数的执行**</font>。示例如下：

```javascript
new Promise((resolve, reject) => {
  resolve(1);
  console.log(2);
}).then(r => {
  console.log(r); // 注：这里接收到了 resolve 传出的1
});
// 2 注：这里2 是同步代码
// 1 注：这里1 是微任务
```

<mark>（注：和上面 p1 的状态 会影响 p2 一样）</mark> 采用链式的then，可以指定一组<font color=FF0000 size=4>**按照次序调用的回调函数**</font>（<mark>注，这样是在表明：返回promise 实例的 promise.then 调用链，是按顺序执行的</mark>）。这时，<font color=FF0000>**前一个回调函数，有可能返回的还是一个 Promise对象（即有异步操作），这时后一个回调函数，就会等待该 Promise 对象的状态发生变化，才会被调用**</font>

<font size=4>**Promise.prototype.catch()**</font>
<font color=FF0000>**Promise.prototype.catch() 方法是 .then(null, rejection) 或 .then(undefined, rejection) 的别名**</font>，用于指定发生错误时的回调函数。
如果<font color=FF0000>异步操作抛出错误，状态就会变为 rejected，就会调用 catch() 方法指定的回调函数，处理这个错误</font>。另外，<font color=FF0000>then() 方法指定的回调函数种，如果运行中抛出错误，也会被catch()方法捕获</font>。
**如果 Promise 状态已经变成 resolved，再抛出错误是无效的**；示例如下：

```js
const promise = new Promise(function(resolve, reject) {
  resolve('ok');
  throw new Error('test');
});
promise.then(function(value) { console.log(value) })
		   .catch(function(error) { console.log(error) });
// ok
```
Promise 在resolve语句后面，再抛出错误，不会被捕获，等于没抛出。因为 <font color=FF0000>**Promise 的状态一旦改变，就永久保持该状态，不会再变**</font>。
<font color=FF0000>**Promise 对象的错误具有 “冒泡” 性质，会一直向后传递，直到被捕获为止**</font>。也就是说，错误总是会被下一个catch语句捕获。（<font color=FF0000>注：promise.then() 也一样，如果上面的代码出了错误，也会去 promise.then() 中寻找 onRejected的函数（一般是第二个回调函数）</font>）

```js
getJSON('/post/1.json').then(function(post) {
  return getJSON(post.commentURL);
}).then(function(comments) {
  // some code
}).catch(function(error) {
  // 处理前面三个Promise产生的错误
});
```
上面代码中，一共有三个 Promise 对象：一个由 getJSON() 产生，两个由 then() 产生。<font color=FF0000>它们之中任何一个抛出的错误，都会被最后一个 catch() 捕获</font>。
<font color=FF0000>**一般来说，不要在then()方法里面定义 Reject 状态的回调函数（即 then 的第二个参数），总是使用 catch 方法**</font>。使用 catch方法 的写法可以捕获前面 then方法 执行中的错误，也<font color=FF0000>更接近同步的写法 ( try / catch )</font>
跟传统的 try / catch 代码块不同的是，如果没有使用 catch() 方法指定错误处理的回调函数，Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应。
```js
const someAsyncThing = function() {
  return new Promise(function(resolve, reject) {
    // 下面一行会报错，因为x没有声明
    resolve(x + 2);
  });
};

someAsyncThing().then(function() { console.log('everything is great'); });

setTimeout(() => { console.log(123) }, 2000);
// Uncaught (in promise) ReferenceError: x is not defined
// 123
```
上面代码中，someAsyncThing()函数产生的 Promise 对象，内部有语法错误。<font color=FF0000>浏览器运行到这一行，会打印出错误提示ReferenceError: x is not defined，但是不会退出进程、终止脚本执行，2 秒之后还是会输出123</font>。这就是说，<font color=FF0000 size=4>**Promise 内部的错误不会影响到 Promise 外部的代码，通俗的说法就是 “Promise 会吃掉错误”**</font>。
在catch中也有可能会抛出错误，这时候，如果在当前catch 后面继续 catch捕获异常的话；这时候，会报错。

<font size=4>**Promise.prototype.finally()**</font> ES2018
<mark>不管 promise 最后的状态，在执行完 then 或 catch 指定的回调函数以后，都会执行 finally 方法指定的回调函数</mark>。
<font color=FF0000>**finally方法 的回调函数不接受任何参数，这意味着 <font size=4>（finally方法）没有办法知道，前面的 Promise 状态到底是 fulfilled 还是 rejected</font>**</font>。这表明：<mark>finally方法 里面的操作，应该是与状态无关的，不依赖于 Promise 的执行结果</mark>。
<font color=FF0000>**finally本质上是then方法的特例：**</font>

```js
promise.finally(() => {
  // 语句
});

// 等同于
promise.then(result => {
    // 语句
    return result;
  },
  error => {
    // 语句
    throw error;
  }
);
```
上面代码中，如果不使用finally方法，同样的语句需要为成功和失败两种情况各写一次。有了 finally 方法，则只需要写一次。
**它的 <font color=FF0000>实现</font> 也很简单：**

```js
Promise.prototype.finally = function (callback) {
  let P = this.constructor;
  return this.then(
    value => P.resolve(callback()).then(() => value),
    reason => P.resolve(callback()).then(() => { throw reason })
  );
};
```
<font size=4>**Promise.race()**</font>
该方法返回的新的 Promise 实例，<font color=FF0000>一旦发生状态变更，状态总是 fulfilled，不会变成 rejected</font>。状态变成 fulfilled 后，它的回调函数会接收到一个数组作为参数，该数组的每个成员对应前面数组的每个 Promise 对象。

<font size=4>**Promise.resolve()**</font>
有时需要将现有对象转为 Promise 对象，Promise.resolve() 方法就起到这个作用。比如 Promise.all( iterator ) 的 iterator 如果为 非promise的数组，会将这些数组成员 通过 Promise.resolve() 转成 Promise，再进一步处理。
**Promise.resolve()等价于下面的写法：**

```js
Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))
```
**Promise.resolve()方法的参数分成四种情况：**
1. **参数是一个 Promise 实例：**如果<font color=FF0000>参数是 Promise 实例</font>，那么 Promise.resolve <font color=FF0000>将不做任何修改、原封不动地返回这个实例</font>。

2. **参数是一个thenable对象：**<font color=FF0000>thenable 对象指的是具有then方法的对象</font>，比如下面这个对象。

```js
let thenable = {
  then: function(resolve, reject) { resolve(42); }
};
```
<font color=FF0000>Promise.resolve() 方法会将这个对象转为 Promise 对象，**然后就立即执行thenable对象的then()方法**</font>。
```javascript
let thenable = {
  then: function(resolve, reject) { resolve(42); }
};

let p1 = Promise.resolve(thenable);
p1.then(function (value) { console.log(value); }); // 42
```
上面代码中，thenable 对象的 then() 方法执行后，对象p1的状态就变为 resolved，从而立即执行最后那个 then() 方法指定的回调函数，输出42。

- <font color=FF0000>**参数不是具有then()方法的对象，或根本就不是对象：**</font><font color=FF0000>如果参数是一个原始值，或者是一个不具有then()方法的对象</font>，则Promise.resolve() 方法 <font color=FF0000 size=4>**返回一个新的 Promise 对象，状态为 resolved**</font>。
  ```js
  const p = Promise.resolve('Hello');
  
  p.then(function (s) { console.log(s) }); // Hello
  ```
  上面代码生成一个新的 Promise 对象的 实例p。<font color=FF0000>由于字符串Hello不属于异步操作（判断方法是字符串对象不具有 then 方法）</font>，返回 Promise 实例的状态从一生成就是resolved，所以回调函数会立即执行。Promise.resolve() 方法的参数，会同时传给回调函数。
- **不带有任何参数：**<font color=FF0000>Promise.resolve() 方法允许调用时不带参数，直接返回一个 resolved 状态的 Promise 对象</font>。

  所以，<mark>如果希望得到一个 Promise 对象，比较方便的方法就是直接调用 Promise.resolve() 方法</mark>。

  ```javascript
  const p = Promise.resolve();
  
  p.then(function () { ... });
  ```

<font size=4>**Promise.reject()**</font>
Promise.reject(reason) 方法也会<font color=FF0000>返回一个新的 Promise 实例，该**实例的状态为rejected**</font>。

```javascript
const p = Promise.reject('出错了');
// 等同于
const p = new Promise((resolve, reject) => reject('出错了'))

p.then(null, function (s) { console.log(s) }); // 出错了
```

上面代码生成一个 Promise 对象的实例p，状态为 rejected，回调函数会立即执行。<font color=FF0000>**Promise.reject() 方法的参数，会原封不动地作为reject的理由，变成后续方法的参数**</font>。

```javascript
Promise.reject('出错了')
.catch(e => { console.log(e === '出错了') }) // true
```

上面代码中，Promise.reject() 方法的参数是一个字符串，后面 catch() 方法的参数e就是这个字符串。

<font size=4>**Promise.try()**</font> （<font color=FF0000>**注：目前还只是提案**</font>）

实际开发中，经常遇到一种情况：<mark>不知道或者不想区分，函数f是同步函数还是异步操作，但是想用 Promise 来处理它</mark>。因为<mark>这样就可以不管 f 是否包含异步操作，都用 then 方法指定下一步流程，用 catch 方法处理f抛出的错误</mark>。一般就会采用下面的写法。 

```js
Promise.resolve().then(f)
```

上面的写法 <font color=FF0000>**有一个缺点，就是如果 f 是同步函数，那么它会在本轮事件循环的末尾执行**</font>。

那么有没有一种方法，让同步函数同步执行，异步函数异步执行，并且让它们具有统一的 API 呢？回答是可以的，并且还有两种写法。

- 第一种写法是用async函数来写

  ```js
  const f = () => console.log('now');
  (async () => f())();
  console.log('next');
  // now
  // next
  ```

- 第二种写法是使用 new Promise()

  ```javascript
  const f = () => console.log('now');
  (
    () => new Promise(
      resolve => resolve(f())
    )
  )();
  console.log('next');
  // now
  // next
  ```

摘自：[ECMAScript 6 入门 - Promise 对象](https://es6.ruanyifeng.com/#docs/promise)

##### 关于如何从 Promise 中取值的补充

打印一个 promise 对象，发现有三个内部属性 \[[Prototype]]、\[[PromiseState]]、\[[PromiseResult]]。其中 \[[PromiseState]] 表示Promise的状态 pending / fulfilled / rejected。\[[PromiseResult]] 表示promise中包含的数据。而js无法访问内部属性，但是可以通过 then 的回掉函数获得：
```js
const promise = Promise.resolve(1)
promise.then(console.log)          // 1
promise.then(_ => console.log(_))  // 1
```



#### **异步函数**

异步函数 ( async function ) 是 <font color=FF0000>ECMAScript 2017</font> ( ECMA-262 ) 标准的规范，几乎被所有浏览器所支持，除了 Internet Explorer。<mark>它们是基于promises的语法糖，使异步代码更易于编写和阅读</mark>；<font color=FF0000>**同时，<font size=4>async/await 也是 generator 的语法糖</font>**</font>，关于 generator 实现异步同步执行，可以参考 [[JS 机制与原理#Generator 的自动执行#关于 generator 实现异步原理的总结]]

**示例：**

```js
async function asyncFunc() {
    await print(1000, "First");
    await print(4000, "Second");
    await print(3000, "Third");
}
asyncFunc();
```

<mark>异步函数 <font color=FF0000>**async function 中可以使用 await 指令**</font>，<font color=FF0000>**await** 指令后**必须**跟着一个 **Promise**</font>，异步函数会在这个 Promise 运行中暂停，直到其运行结束再继续运行</mark>。

**示例：**

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

##### **更多补充：**

- **async关键字：**
  
  我们使用 async 关键字，把它放在函数声明之前，使其成为 async function。异步函数是一个知道怎样使用 await 关键字调用异步代码的函数。
  
  ```js
  async function hello() { return "Hello" };
  hello();
  ```
  
  现在<font color=FF0000>调用该函数会返回一个 promise</font>。这是异步函数的特征之一 —— <font color=FF0000>**它保证函数的返回值为 promise**</font>（自动）。

- **await关键字：**
  
  当 await 关键字与异步函数一起使用时，它的真正优势就变得明显了 —— 事实上， <font color=FF0000>await 只在异步函数里面才起作用</font>。它可以放在任何异步的，基于 promise 的函数之前。<font color=FF0000>它会暂停代码在该行上，直到 promise 完成，然后返回结果值</font>。在暂停的同时，其他正在等待执行的代码就有机会执行了。

摘自：[async和await:让异步编程更简单](https://developer.mozilla.org/zh-CN/docs/learn/JavaScript/%E5%BC%82%E6%AD%A5/Async_await)

> 在函数前面的 “async” 这个单词表达了一个简单的事情：即这个函数总是返回一个 promise。其他值将自动被包装在一个 resolved 的 promise 中。
>
> 关键字 await 让 JavaScript 引擎等待直到 promise 完成（settle）并返回结果。
>
> 注：经过实验表明：await 似乎只有在 返回一个 settled 状态的 Promise，才会生效（等其完成）
>
> ```js
> async function wait() {
>   await new Promise(resolve => setTimeout(resolve, 1000));
>   console.log('await work')
> }
> wait() // await 生效了
> 
> async function noWait() {
>   await setTimeout(() => console.log(1), 1000)
>   console.log(2)
> }
> noWait() // 未生效
> ```
>
> 摘自：[现代JS教程 - Async/await](https://zh.javascript.info/async-await)



#### for await...of

for await...of 语句创建一个循环，<font color=FF0000>该循环遍历异步可迭代对象以及同步可迭代对象</font>，<mark>包括: 内置的 String，Array，类似数组对象 (例如 arguments 或 NodeList)，TypedArray, Map, Set 和用户定义的异步/同步迭代器</mark>。它使用对象的每个不同属性的值调用要执行的语句来调用自定义迭代钩子。

<mark>类似于 await 运算符一样，该语句只能在一个async function 内部使用</mark>。

<font color=FF0000>**注意：**for await...of 不适用于不是异步可迭代的异步迭代器</font>。

**语法**

```JS
for await (variable of iterable) {
  statement
}
```

- **variable：**在每次迭代中，将不同属性的值分配给变量。变量有可能以const, let, 或者 var来声明。
- **iterable：**被迭代枚举其属性的对象。<font color=FF0000>与 for...of 相比，这里的对象可以返回 Promise，如果是这样，那么 variable 将是 Promise 所包含的值，否则是值本身</font>。

**示例**

**迭代异步生成器** 

<font color=FF0000>异步生成器已经实现了异步迭代器协议</font>, 所以可以用 for await...of循环。

```js
async function* asyncGenerator() {
  var i = 0;
  while (i < 3) {
    yield i++;
  }
}

(async function() {
  for await (num of asyncGenerator()) {
    console.log(num);
  }
})(); // 结果：0 1 2
```

摘自：[MDN - for await...of](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for-await...of)



#### Generator

生成器对象是由一个 [generator function](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/function*)（注：为 function*） 返回的,并且它符合可迭代协议和迭代器协议。

方法

- **Generator.prototype.next()：**返回一个由 yield 表达式生成的值。

  > next() 方法返回一个包含属性 done 和 value 的对象。该方法也可以通过接受一个参数用以向生成器传值。
  >
  > - **语法**
  >
  >   ```js
  >   gen.next(value)
  >   ```
  >
  > - **参数**
  >   **value：**向生成器传递的值.
  >
  > - **返回值：**返回的对象包含两个属性
  >
  >   - **done (布尔类型)：**如果迭代器超过迭代序列的末尾，则值为 true。 在这种情况下，value可选地指定迭代器的返回值。
  >     如果迭代器能够生成序列中的下一个值，则值为false。 这相当于没有完全指定done属性。
  >   - **value：**迭代器返回的任意的Javascript值。当 done 的值为 true 时可以忽略该值。
  >
  > - **示例：**使用 next() 方法；下面的例子展示了一个简单的生成器, 以及调用 next 后方法的返回值:
  >
  >   ```js
  >   function* gen() { yield 1; yield 2; yield 3; }
  >                                                                                                                               
  >   var g = gen(); // "Generator { }" 注：这里调用 gen() 返回了一个为名为 g 的 Generator 对象
  >   g.next();      // "Object { value: 1, done: false }"
  >   g.next();      // "Object { value: 2, done: false }"
  >   g.next();      // "Object { value: 3, done: false }"
  >   g.next();      // "Object { value: undefined, done: true }"
  >   ```
  >
  >   **向生成器传值**
  >
  >   在此示例中，使用值调用next。 <font color=FF0000>请注意，第一次调用没有记录任何内容，因为生成器最初没有产生任何结果</font>。
  >
  >   ```js
  >   function* gen() {
  >     while(true) {
  >       var value = yield null;
  >       console.log(value);
  >     }
  >   }
  >                                                                                                                               
  >   var g = gen();
  >   g.next(1); // "{ value: null, done: false }"
  >   g.next(2); // 2
  >   // "{ value: null, done: false }"
  >   ```
  >
  > 摘自：[MDN - Generator.prototype.next()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator/next)

- **Generator.prototype.return()：**返回给定的值并结束生成器。

  > **语法：**
  >
  > ```js
  > gen.return(value) // 注意，不是在 generator 函数中调用 return
  > ```
  >
  > - **参数**
  >   **value：**需要返回的值
  > - **返回值：**返回该函数参数中给定的值
  >
  > **示例：**
  >
  > ```js
  > function* gen() { yield 1; yield 2; yield 3; }
  > var g = gen();
  > 
  > g.next();        // { value: 1, done: false }
  > g.return("foo"); // { value: "foo", done: true } 注：这里 done 变成了 true。另外，如果return中没有带参数，则 value 为 undefined；如果带了参数，则 value 为参数的值
  > g.next();        // { value: undefined, done: true } 这里再调用 next()，done 始终都是 true。
  > ```
  >
  > 如果对已经处于 “完成” 状态 的生成器调用 return(value)，则生成器将保持在“完成”状态。<mark>如果没有提供参数，则返回对象的 value 属性与示例最后的 .next() 方法相同</mark>。如果提供了参数，则参数将被设置为返回对象的 value 属性的值。
  >
  > 摘自：[MDN - Generator.prototype.return()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator/return)

- **Generator.prototype.throw()：**向生成器抛出一个错误。

  > throw() 方法<font color=FF0000>用来向生成器抛出异常</font>，并恢复生成器的执行，返回带有 done 及 value 两个属性的对象。
  >
  > **语法：**
  >
  > ```js
  > gen.throw(exception)
  > ```
  >
  > - **参数：** exception：用于抛出的异常。 使用 Error 的实例对调试非常有帮助。**注：**如下示例，exception 是 Error 对象
  > - **返回值：**带有两个属性的对象：
  >   - **done (boolean)：**如果迭代器已经返回了迭代序列的末尾，则值为 true。在这种情况下，可以指定迭代器 value 的返回值。 如果迭代能够继续生产在序列中的下一个值，则值为 false。 这相当与不指定 done 属性的值。
  >   - **value：**迭代器返回的任何 JavaScript 值。当 done 是 true 的时候可以省略。
  >
  > **示例：**
  >
  > 下面的例子展示了一个简单的生成器并使用 throw方法向该生成器抛出一个异常，该异常通常可以通过 try...catch 块进行捕获
  >
  > ```js
  > function* gen() {
  >   while(true) {
  >     try { yield 42; } 
  >     catch(e) { console.log("Error caught!"); }
  >   }
  > }
  > 
  > var g = gen();
  > g.next(); // { value: 42, done: false }
  > g.throw(new Error("Something went wrong")); // "Error caught!"
  > // g.next() { value: undefined, done: true} 这行代码是我自己添加测试的，发现 throw调用后，done 就直接变成 true 了
  > ```
  >
  > 摘自：[MDN - Generator.prototype.throw()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator/throw)

摘自：[MDN - Generator](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator)

#### GeneratorFunction

GeneratorFunction构造器<font color=FF0000>生成新的生成器函数对象</font>。在JavaScript中，<font color=FF0000>生成器函数实际上都是GeneratorFunction的实例对象</font>。

注意，GeneratorFunction并不是一个全局对象。它可以通过下面的代码获取。

```js
Object.getPrototypeOf(function*(){}).constructor
```

- **语法**

  ```js
  new GeneratorFunction ([arg1[, arg2[, ...argN]],] functionBody)
  ```

- **参数**

  - **arg1, arg2, ... argN：**函数使用的名称作为形式参数名称。每个必须是一个字符串，对应于一个有效的JavaScript标识符或这样的字符串的列表，用逗号分隔；如“x”，“theValue”或“a,b”。
  - **functionBody：**一个包含多条表示JavaScript函数体语句的字符串。

摘自：[MDN - GeneratorFunction](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/GeneratorFunction)

#### yield

yield 关键字用来<font color=FF0000>暂停和恢复一个生成器函数（ function* 或遗留的生成器函数 ）</font>。

**语法**

```js
[rv] = yield [expression];
```

- **expression：**定义通过迭代器协议从生成器函数返回的值。如果省略，则返回undefined。
- **rv：**返回传递给生成器的next()方法的可选值，以恢复其执行。

**描述**
<font color=FF0000>yield 关键字使生成器函数执行暂停</font>，yield 关键字 <font color=FF0000>**后面的表达式的值返回给生成器的调用者**</font>。<font color=FF0000>**它可以被认为是一个基于生成器的版本的return 关键字**</font>。

<font color=FF0000>yield关键字实际返回一个 **IteratorResult** 对象</font>，它有两个属性，value 和 done。value 属性是对 yield 表达式求值的结果，而 done 是 false，表示生成器函数尚未完全完成。

一旦遇到 yield 表达式，生成器的代码将被暂停运行，直到生成器的 next() 方法被调用。每次调用生成器的 next() 方法时，生成器都会恢复执行，直到达到以下某个值：

- **yield：**导致生成器再次暂停并返回生成器的新值。 下一次调用 next() 时，在 yield 之后紧接着的语句继续执行。
- **throw：**用于从生成器中抛出异常。这让生成器完全停止执行，并在调用者中继续执行，正如通常情况下抛出异常一样。
- **到达生成器函数的结尾：**在这种情况下，生成器的执行结束，并且 IteratorResult 给调用者返回 undefined 并且 done 为  true。
- **到达return 语句：**在这种情况下，生成器的执行结束，并将 IteratorResult 返回给调用者，其值是由 return 语句指定的，并且 done 为 true。

<font color=FF0000>**如果将参数传递给生成器的 next() 方法，则该值将成为生成器当前yield操作返回的值。**</font>

在生成器的代码路径中的 yield 运算符，以及通过将其传递给 Generator.prototype.next() 指定新的起始值的能力之间，生成器提供了强大的控制力。

摘自：[MDN - yield](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/yield)

**补充：**yield 和迭代器 iterator 很类似，不过 yield 是构造一个 generator，每有一个 yield 停下来，等待再次调用，从停下来的地方继续执行。而 iterator是通过 next() 继续执行。另外，因为 generator 内部自己实现了 next() 方法，所以，可以用 generator 来实现 iterator

#### yield*

 yield* 表达式<font color=FF0000>用于委托给另一个generator 或可迭代对象</font>。

**语法**

```js
 yield* [[expression]];
```

expression：返回一个可迭代对象的表达式。

**描述：**yield* 表达式迭代操作数，并产生它返回的每个值。yield* 表达式本身的值是当迭代器关闭时返回的值（即done为true时）。

**示例：**

- **委托给其他生成器：**以下代码中，<font color=FF0000>g1() yield 出去的每个值都会在 g2() 的 next() 方法中返回</font>，就像那些 yield 语句是写在 g2() 里一样

  ```js
  function* g1() { yield 2; yield 3; yield 4; }
  function* g2() { yield 1; yield* g1(); yield 5; } // 注意这里的 yield* g1()
  
  var iterator = g2();
  console.log(iterator.next()); // { value: 1, done: false }
  console.log(iterator.next()); // { value: 2, done: false }
  console.log(iterator.next()); // { value: 3, done: false }
  console.log(iterator.next()); // { value: 4, done: false }
  console.log(iterator.next()); // { value: 5, done: false } 注意，执行完g1后，g2继续执行
  console.log(iterator.next()); // { value: undefined, done: true }
  ```

- **委托给其他可迭代对象：**除了生成器对象这一种可迭代对象，<font color=FF0000 size=4>yield* 还可以 yield 其它任意的**可迭代对象**</font>，比如说 <font color=FF0000 size=4>**数组、字符串、arguments 对象**等等</font>。

  ```js
  function* g3() { yield* [1, 2]; yield* "34"; yield* arguments; } 
  
  var iterator = g3(5, 6);
  console.log(iterator.next()); // { value: 1, done: false }
  console.log(iterator.next()); // { value: 2, done: false }
  console.log(iterator.next()); // { value: "3", done: false }
  console.log(iterator.next()); // { value: "4", done: false }
  console.log(iterator.next()); // { value: 5, done: false }
  console.log(iterator.next()); // { value: 6, done: false }
  console.log(iterator.next()); // { value: undefined, done: true }
  ```

- **yield* 表达式的值：**<font color=FF0000>**yield* 是一个表达式，不是语句，所以它会有自己的值**</font>。

  ```js
  function* g4() { yield* [1, 2, 3]; return "foo"; }
  var result;
  function* g5() { result = yield* g4(); }
  
  var iterator = g5();
  console.log(iterator.next()); // { value: 1, done: false }
  console.log(iterator.next()); // { value: 2, done: false }
  console.log(iterator.next()); // { value: 3, done: false }
  console.log(iterator.next()); // { value: undefined, done: true } 此时 g4()返回了 { value: "foo", done: true }
  console.log(result);          // "foo"
  ```

摘自：[MDN - yield*](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/yield*)



#### iterator

**<font color=FF0000>原生</font>具备Iterator接口的数据结构**

- Array
- Map
- Set
- String
- TypedArray
- 函数的arguments对象
- Nodelist对象

**对于自定义的结构，需要满足迭代协议（两个）：**

- **可迭代协议：**<font color=FF0000>**当前类对戏中包含 Symbol.iterator**</font>

- **迭代器协议：**迭代器必须符合：

  ```js
  return { next() { return {value, done} } }
  ```

  即：返回一个对象，对象中包含 next() 函数，对象中必须有函数的值（value）和done的值

<font size=4>**补充：**</font>

<font size=4>**迭代协议**</font>

作为 ECMAScript 2015 的一组补充规范，迭代协议并不是新的内置实现或语法，而是协议。这些协议可以被任何遵循某些约定的对象来实现。 <font color=FF0000>迭代协议具体分为两个协议：可迭代协议和迭代器协议。</font>

- **可迭代协议**

  <font color=FF0000>可迭代协议允许 JavaScript 对象定义或定制它们的迭代行为，例如，在一个 for..of 结构中，哪些值可以被遍历到</font>。一些内置类型同时是内置可迭代对象，并且有默认的迭代行为，比如 Array 或者 Map，而其他内置类型则不是（比如 Object)）。

  <font color=FF0000>要成为可迭代对象， 一个对象必须实现 @@iterator 方法。这意味着对象（或者它原型链上的某个对象）必须有一个键为 @@iterator 的属性</font>，可通过常量 Symbol.iterator 访问该属性：

  | 属性              | 值                                                     |
  | :---------------- | :----------------------------------------------------- |
  | [Symbol.iterator] | 一个无参数的函数，其返回值为一个符合迭代器协议的对象。 |

  <font color=FF0000>当一个对象需要被迭代的时候（比如被置入一个 for...of 循环时），首先，会不带参数调用它的 @@iterator 方法，然后使用此方法返回的迭代器获得要迭代的值</font>。
  值得注意的是调用此零个参数函数时，它将作为对可迭代对象的方法进行调用。 因此，在函数内部，this关键字可用于访问可迭代对象的属性，以决定在迭代过程中提供什么。

  <font color=FF0000>此函数可以是普通函数，也可以是生成器函数，以便在调用时返回迭代器对象。 在此生成器函数的内部，可以使用yield提供每个条目</font>。

- **迭代器协议**
  迭代器协议定义了产生一系列值（无论是有限个还是无限个）的标准方式。当值为有限个时，所有的值都被迭代完毕后，则会返回一个默认返回值。
  只有实现了一个拥有以下语义（semantic）的 next() 方法，一个对象才能成为迭代器：

  | 属性 | 值                                                           |
  | :--- | :----------------------------------------------------------- |
  | next | 一个无参数函数，返回一个应当拥有以下两个属性的对象：<br/><font color=FF0000>**done（boolean）：**</font>如果迭代器可以产生序列中的下一个值，则为 false。（这等价于没有指定 done 这个属性。）<br>如果迭代器已将序列迭代完毕，则为 true。这种情况下，value 是可选的，如果它依然存在，即为迭代结束之后的默认返回值。<br /><font color=FF0000>**value：**</font>迭代器返回的任何 JavaScript 值。done 为 true 时可省略。<br /><font color=FF0000>next() 方法必须返回一个对象，该对象应当有两个属性： done 和 value</font>，<mark>如果返回了一个非对象值（比如 false 或 undefined），则会抛出一个 TypeError 异常</mark>（"iterator.next() returned a non-object value"）。 |

  一些内置的语法结构，比如 展开运算符 其内部实现也使用了同样的迭代协议。

  摘自：[MDN - 迭代协议](https://developer.mozilla.org/zh-CN/docs/orphaned/Web/JavaScript/Reference/Iteration_protocols)



#### Fetch

Fetch API 提供了一个 JavaScript 接口， 提供了对 Request 和 Response （以及其他与网络请求有关的）对象的通用定义；用于访问和操纵 HTTP 管道的一些具体部分，例如请求和响应。它还提供了一个全局 fetch() 方法，该方法提供了一种简单，合理的方式来跨网络异步获取资源。

一个基本的 fetch 请求设置起来很简单。<font color=FF0000>fetch() 必须接受一个参数——资源的路径</font>。无论请求成功与否，它都返回一个 Promise 对象，resolve 对应请求的 Response。

看看下面的代码：

```js
fetch('http://example.com/movies.json')
  .then(function(response) {
    return response.json();
  })
  .then(function(myJson) {
    console.log(myJson);
  });
```

这里我们通过网络获取一个 JSON 文件并将其打印到控制台。最简单的用法是只提供一个参数用来指明想 fetch() 到的资源路径，然后返回一个包含响应结果的promise（一个 Response 对象）。

<font color=FF0000>也可以传一个可选的第二个参数 init</font>

**fetch 语法如下：**

**语法**

```js
Promise<Response> fetch(input[, init]);
```

**参数**

- **?input**

  定义要获取的资源。这可能是：一个 USVString 字符串，包含要获取资源的 URL。一些浏览器会接受 blob: 和 data: 作为 schemes.一个 Request 对象。

- **init 可选**

  一个配置项对象，包括所有对请求的设置。可选的参数有：

  - **method：** 请求使用的方法，如 GET、POST。
  
  - **headers：** <font color=FF0000>请求的头信息</font>，形式为 Headers 的对象或包含 ByteString 值的对象字面量。
  
  - **body：**<font color=FF0000>**请求的 body 信息**，可能是 一个 Blob、BufferSource、FormData、URLSearchParams 或者 USVString 对象</font>。注意 GET 或 HEAD 方法的请求不能包含 body 信息。
  
  - **mode：** 请求的模式，如 cors、 no-cors 或者 same-origin。
  
    > mode 选项是一种安全措施，可以防止偶发的跨源请求：
    >
    > - **"cors"：**默认值，允许跨源请求，如 [Fetch：跨源请求](https://zh.javascript.info/fetch-crossorigin) 一章所述，
    > - **"same-origin"：**禁止跨源请求，
    > - **"no-cors" ：**只允许简单的跨源请求。
    >
    > 当 fetch 的 URL 来自于第三方，并且我们想要一个“断电开关”来限制跨源能力时，此选项可能很有用。
    >
    > 摘自：[现代JS教程 - Fetch API - mode](https://zh.javascript.info/fetch-api#mode)
  
  - **credentials：** 请求的 credentials，如 omit、same-origin 或者 include。为了在当前域名内自动发送 cookie ， 必须提供这个选项， 从 Chrome 50 开始， 这个属性也可以接受 FederatedCredential 实例或是一个 PasswordCredential 实例。
  
    > credentials 选项<font color=FF0000>指定 fetch 是否应该 **随请求发送 cookie** 和 HTTP-Authorization header</font>。
    >
    > - **"same-origin"**：<font color=FF0000>默认值</font>，<font color=FF0000>对于跨源请求不发送</font>，
    > - **"include"：**<font color=FF0000>总是发送</font>，需要来自跨源服务器的 Accept-Control-Allow-Credentials，才能使 JavaScript 能够访问响应，详细内容在 [Fetch：跨源请求](https://zh.javascript.info/fetch-crossorigin) 一章有详细介绍
    > - **"omit"**：<font color=FF0000>不发送</font>，即使对于同源请求。
    >
    > 摘自：[现代JS教程 - Fetch API - credentials](https://zh.javascript.info/fetch-api#credentials)
  
  - **cache：**  请求的 cache 模式: default、 no-store、 reload 、 no-cache、 force-cache或者 only-if-cached 。
  
    > 默认情况下，fetch 请求使用标准的 HTTP 缓存。就是说，它遵从 Expires，Cache-Control header，发送 If-Modified-Since，等。就像常规的 HTTP 请求那样。
    >
    > 下面还有具体选项的含义，详见链接
    >
    > 摘自：[现代JS教程 - Fetch API - cache](https://zh.javascript.info/fetch-api#cache)
  
  - **redirect：** 可用的 redirect 模式： follow （自动重定向），error （如果产生重定向将自动终止并且抛出一个错误）, 或者 manual （手动处理重定向）。 在Chrome中默认使用follow（Chrome 47之前的默认值是manual）。
  
  - **referrer：** 一个 USVString 可以是 no-referrer、client或一个 URL。默认是 client。
  
  - **referrerPolicy：** 指定了 HTTP 头部 referer 字段的值。可能为以下值之一： no-referrer、 no-referrer-when-downgrade、 origin、 origin-when-cross-origin、 unsafe-url 。
  
  - **integrity：** 包括请求的  subresource integrity 值 （ 例如： sha256-BpfBw7ivV8q2jLiT13fxDYAe2tJllusRSZ273h2nFSE=）。

**返回值**

一个 Promise，resolve 时回传 Response 对象。

摘自：[MDN - Fetch API](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API)  [MDN - 使用 Fetch](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch#%E6%94%AF%E6%8C%81%E7%9A%84%E8%AF%B7%E6%B1%82%E5%8F%82%E6%95%B0)  [WorkerOrGlobalScope.fetch()](https://developer.mozilla.org/zh-CN/docs/Web/API/WindowOrWorkerGlobalScope/fetch)

##### Fetch 的补充：

**请注意，fetch 规范与 jQuery.ajax() 主要有以下的不同：**

- <font color=FF0000>**当接收到一个代表错误的 HTTP 状态码时，从 fetch() 返回的 Promise 不会被标记为 reject**</font> ，<font color=FF0000>即使响应的 HTTP 状态码是 404 或 500 。相反，它会将 Promise 状态标记为 resolve</font>（如果响应的 HTTP 状态码不在 200 - 299 的范围内，则设置 resolve 返回值的 ok 属性为 false ），<font color=FF0000 size=4>**仅当网络故障时或请求被阻止时，才会标记为 reject**</font>。
- <font color=FF0000>**fetch 不会发送跨域 cookies，除非你使用了 credentials 的初始化选项**</font>。（自2018 年 8 月以后，默认的 credentials 政策变更为 same-origin ）

**自定义请求对象**

除了传给 `fetch()` 一个资源的地址，你还可以通过使用 [`Request()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Request/Request "Request()") 构造函数来创建一个 request 对象，然后再作为参数传给 `fetch()`。示例如下：

```js
const myHeaders = new Headers();

const myRequest = new Request('flowers.jpg', {
  method: 'GET',
  headers: myHeaders,
  mode: 'cors',
  cache: 'default',
});

fetch(myRequest)
  .then(response => response.blob())
  .then(myBlob => { myImage.src = URL.createObjectURL(myBlob); });
```

<font color=FF0000>`Request()` 和 `fetch()` 接受同样的参数</font>。你甚至可以传入一个已存在的 request 对象来创造一个拷贝：

```js
const anotherRequest = new Request(myRequest, myInit);
```

**使用 Headers 接口**

使用 [`Headers`](https://developer.mozilla.org/zh-CN/docs/Web/API/Headers) 的接口，你可以通过 [`Headers()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Headers/Headers "Headers()") 构造函数来创建一个你自己的 headers 对象。一个 headers 对象是一个简单的多键值对：

```js
const content = 'Hello World';
const myHeaders = new Headers();
myHeaders.append('Content-Type', 'text/plain');
myHeaders.append('Content-Length', content.length.toString());
myHeaders.append('X-Custom-Header', 'ProcessThisImmediately');
```

也可以传入一个多维数组或者对象字面量：

```js
const myHeaders = new Headers({
  'Content-Type': 'text/plain',
  'Content-Length': content.length.toString(),
  'X-Custom-Header': 'ProcessThisImmediately'
});
```

它的内容可以被获取：

```js
console.log(myHeaders.has('Content-Type')); // true
console.log(myHeaders.has('Set-Cookie')); // false
myHeaders.set('Content-Type', 'text/html');
myHeaders.append('X-Custom-Header', 'AnotherValue');

console.log(myHeaders.get('Content-Length')); // 11
console.log(myHeaders.get('X-Custom-Header')); // ['ProcessThisImmediately', 'AnotherValue']

myHeaders.delete('X-Custom-Header');
console.log(myHeaders.get('X-Custom-Header')); // null
```

虽然一些操作只能在 ServiceWorkers 中使用，但是它提供了更方便的操作 Headers 的 API。

**Response 对象**

如上所述，Response 实例是在 fetch() 处理完 promise 之后返回的。

**会用到的最常见的 response 属性有：**
- **Response.status：**整数（默认值为 200）为response的状态码。
- **Response.statusText：**字符串（默认值为 ""），该值与 HTTP 状态码消息对应。 注意：HTTP/2 不支持状态消息
- **Response.ok：**该属性是来检查 response 的状态是否在 200 - 299（包括200 和 299）这个范围内。该属性返回一个布尔值。

##### Body

不管是请求还是响应都能够包含 body 对象。body 也可以是以下任意类型的实例。

- ArrayBuffer
- ArrayBufferView (Uint8Array等)
- Blob/File
- string
- URLSearchParams
- FormData

<font color=FF0000>**Body 类定义了以下方法（这些方法都被 Request 和 Response所实现）以获取 body 内容**</font>。这些方法都会返回一个被解析后的 Promise 对象和数据。

- Request.arrayBuffer() / Response.arrayBuffer()
- Request.blob()  / Response.blob()
- Request.formData()  / Response.formData()
- Request.json()  / Response.json()
- Request.text()  / Response.text()

相比于XHR，这些方法让非文本化数据的使用更加简单。

摘自：[MDN - 使用 Fetch](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch) 



#### 宏任务和微任务

**宏任务有哪些**
- \<script>标签中的运行代码（注：<font color=FF0000 size=4>**整个script标签是个宏任务**</font>）
- 事件触发的回调函数，例如DOM Events、I/O、requestAnimationFrame
- setTimeout、setInterval的回调函数

**微任务有哪些**

- **promises：**Promise.then、Promise.catch、Promise.finally
- **MutationObserver**
- **queueMicrotask**
- **process.nextTick**：Node独有

摘自：[执行机制 - 宏任务和微任务分别有哪些 #34](https://github.com/logan70/Blog/issues/34)

**注：**这里不全，可以参考 [[前端面试点总结#宏任务和微任务]]



**补充：**queueMicrotask()

为了允许第三方库、框架、polyfills 能使用微任务，Window 暴露了 queueMicrotask() 方法，而 Worker 接口则通过WindowOrWorkerGlobalScope mixin 提供了同名的 queueMicrotask() 方法。

<font color=FF0000>如果一个微任务通过调用 queueMicrotask()，向队列中加入了更多的微任务，则 **那些新加入的微任务 会早于下一个任务运行**</font>（<mark>注：这里的任务是task，即宏任务</mark>） 。这是因为事件循环会持续调用微任务直至队列中没有留存的，即使是在有更多微任务持续被加入的情况下

虽然在过去要使得入列微任务成为可能有可用的技巧（比如创建一个立即 resolve 的 promise），但新加入的 queueMicrotask() 方法增加了一种标准的方式，<font color=FF0000>**可以安全的引入微任务而避免使用额外的技巧**</font>。<mark>通过引入 queueMicrotask()，由晦涩地使用 promise 去创建微任务而带来的风险就可以被避免了</mark>。举例来说，当使用 promise 创建微任务时，由回调抛出的异常被报告为 rejected promises 而不是标准异常。同时，创建和销毁 promise 带来了事件和内存方面的额外开销，这是正确入列微任务的函数应该避免的。

**何时使用微任务：**

<mark>如果可能的话，大部分开发者并不应该过多的使用微任务</mark>。**使用微任务的最主要原因简单归纳为：**<font color=FF0000>确保任务顺序的一致性，即便当结果或数据是同步可用的，也要同时减少操作中用户可感知到的延迟而带来的风险</font>。

摘自：[MDN - 在 JavaScript 中通过 queueMicrotask() 使用微任务](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_DOM_API/Microtask_guide)

示例如下：

```js
const fn = () => console.log('queueMicrotask fn')
queueMicrotask(fn) // 自动变成一个微任务，并执行，打印出 queueMicrotask fn

// 另外，queueMicrotask的优先级没有 promise高；加入如下代码：
async function asyncFn() {
  await console.log(1)
}
asyncFn() // 1 比 queueMicrotask fn 先打印
```



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

<font color=FF0000>**补充：**</font>我们<font color=FF0000> 使用 **函数字面量 **创建函数时 const adder = function(a, b) { return a + b; }，其实等价于 const adder = **new Function("a", "b", "return a + b");**</font>

有些属性是 Function 的，而有些属性则是 Function 从父类继承过来的（确切的说是原型链上的）。那么 Function 的父类是谁呢？是 Object。而 Object 的父类又是谁呢？答案是，是 Function。

这就是 javascript 中鸡生蛋、蛋生鸡的难题。关于这个话题就可以讨论一天一夜了，我就不展开了，只放一张图：

<img src="https://i.loli.net/2021/10/21/IxpsZcEq8tr7fXD.jpg" style="zoom: 80%;" />

摘自：[如何理解在 JavaScript 中 "函数是第一等公民" 这句话? - justjavac的回答 - 知乎](https://www.zhihu.com/question/67652709/answer/255460149)

##### 函数提升（Hoisting）

提升（Hoisting）是 JavaScript 默认<font color=FF0000>将当前作用域提升到前面去的的行为</font>。

函数可以在声明之前调用：

```js
myFunction(5);

function myFunction(y) {
    return y * y;
}
```

##### 关于函数提升的补充

> 这种行为（注：函数提升）仅适用于函数声明，而不适用于我们将函数分配给变量的函数表达式，例如 let say = function(name)...
>
> 摘自：[现代JS教程 - 变量作用域，闭包](https://zh.javascript.info/closure)

#### Function.length

length 属性<font color=FF0000>指明函数的形参个数</font>。示例如下：

```js
function func1() {}
console.log(func1.length); // expected output: 0

function func2(a, b) {}
console.log(func2.length); // expected output: 2

const arrownFn = (a, b, c) => {}
console.log(arrowFn.length) // 3

function argFn = function(...args) {}
console.log(argFn.length) // 0
```

| Function.length 属性的属性特性： |       |
| :------------------------------- | ----- |
| writable                         | false |
| enumerable                       | false |
| configurable                     | true  |

**描述**

length 是函数对象的一个属性值，指该函数期望传入的参数数量，即形参的个数。<font color=FF0000>形参的数量不包括剩余参数个数</font>，仅包括第一个具有默认值之前的参数个数。与之对比的是，arguments.length 是函数被调用时实际传参的个数（**注：**即 Function.length 和 arguments。length 对应）。

**Function 构造器的属性：**<font color=FF0000>Function 构造器本身也是个 Function。他的 length 属性值为 1</font> 。该属性 Writable: false, Enumerable: false, Configurable: true

**Function.prototype 对象的属性：** <mark>Function.prototype 对象的 length 属性值为 0 </mark>。

摘自：[MDN - Function.length](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/length)

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

**补充：**

- <font color=FF0000>**箭头函数中的this指向的是它外层对象的this**</font>。this指向定义时所在的对象，而不是调用时所在的对象

- 箭头函数不能作为构造函数

- 箭头函数不能使用arguments对象，但是可以使用rest运算符

- 对于对象中方法的定义，如果使用箭头函数，this的指向会有问题。不过ES6中可以这样定义

  ```js
  let obj = {
    fnName() { ... }
  }
  ```

  

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

#### Arguments 对象

arguments 是一个对应于传递给函数的参数的<font color=FF0000>类数组对象</font>。

<font color=FF0000>arguments对象是所有（<font size=4>非箭头</font>）函数中都可用的局部变量</font>。你<font color=FF0000>可以使用arguments对象在函数中引用函数的参数</font> ( 比如：arguments[0] )。此对象包含传递给函数的每个参数，第一个参数在索引0处。

<font color=FF0000 size=4>**arguments 对象不是一个 Array 。它类似于Array，但除了 length 属性和索引元素之外没有任何 Array 属性**</font>。例如，它没有 pop / push 方法。但是它可以被转换为一个真正的Array：

```js
var args = Array.prototype.slice.call(arguments);
var args = [].slice.call(arguments);
var args = [].splice.call(arguments, 0)
var args = [].concat.apply([], arguments)

// ES2015
const args = Array.from(arguments);
const args = [...arguments];
```

补充：arguments对象是一个包含length属性的对象。如下示例：

```js
function fn() {
  console.log(arguments)
}

fn(1, 2, 3, 4) 
// [Arguments] { '0': 1, '1': 2, '2': 3, '3': 4 }，但是这种键值对的形式，可以通过argument['index']获取到值
// 在浏览器控制台的效果如下图
```

![image-20220104204631259](https://s2.loli.net/2022/01/04/hNHnBVwWcZdM1GD.png)

**属性**

- **arguments.callee：**指向参数所属的当前执行的函数。

  > **注：**在严格模式下，第5版 ECMAScript (ES5) 禁止使用 arguments.callee()
  >
  > 详见：[MDN - arguments.callee](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arguments/callee)

- **arguments.caller：**🗑 指向调用当前函数的函数。

- **arguments.length：**传递给函数的参数数量。

- **arguments[@@iterator]：**返回一个新的Array 迭代器 对象，该对象包含参数中每个索引的值。

摘自：[MDN - Arguments 对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arguments)



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



#### Function.prototype.call()

call() 方法<font color=FF0000>使用一个指定的 this 值</font>和<font color=FF0000>单独给出的一个或多个参数</font>来调用一个函数。

**注意：**该方法的语法和作用与 apply() 方法类似，只有一个区别，就是 call() 方法接受的是一个参数列表，而 apply() 方法接受的是一个包含多个参数的数组。

- **语法**

  ```js
  function.call(thisArg, arg1, arg2, ...)
  ```

- **参数**

  - **thisArg：**<font color=FF0000>**可选的**</font>。在 function 函数运行时使用的 this 值。请注意，this可能不是该方法看到的实际值：<font color=FF0000>如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象</font>，原始值会被包装。
  - **arg1, arg2, ...：**指定的参数列表。

- **返回值：**<font color=FF0000>使用调用者提供的 this 值和参数调用该函数的返回值</font>。若该方法没有返回值，则返回 undefined。

- **描述：**
  call() 允许为不同的对象分配和调用属于一个对象的函数/方法。

  call() 提供新的 this 值给当前调用的函数/方法。你<font color=FF0000>可以使用 call 来实现继承</font>：写一个方法，然后让另外一个新的对象来继承它（而不是在新对象中再写一次这个方法）。

- **其他：**

  **使用 call 方法调用函数并且不指定第一个参数（argument）**

  在下面的例子中，我们调用了 display 方法，但并没有传递它的第一个参数。<font color=FF0000>如果没有传递第一个参数，this 的值将会被绑定为全局对象</font>。

  ```js
  var sData = 'Wisen';
  
  function display() {
    console.log('sData value is %s ', this.sData);
  }
  
  display.call();  // sData value is Wisen
  ```

摘自：[MDN - Function.prototype.call()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)

#### Function.prototype.apply()

apply() 方法<font color=FF0000>调用一个具有给定this值的函数</font>，以及<font color=FF0000>以一个数组（或类数组对象）的形式提供的参数</font>。

**注意：**call() 方法的作用和 apply() 方法类似，区别就是 call() 方法接受的是参数列表，而 apply() 方法接受的是一个参数数组。

- **语法**

  ```js
  func.apply(thisArg, [argsArray])
  ```

- **参数**

  - **thisArg：**<font color=FF0000>必选</font>。在 func 函数运行时使用的 this 值。请注意，this可能不是该方法看到的实际值：如果这个函数处于非严格模式下，则<font color=FF0000>指定为 null 或 undefined 时会自动替换为指向全局对象，原始值会被包装</font>。
  - **argsArray：**<font color=FF0000>可选</font>。<font color=FF0000>一个数组或者类数组对象</font>，其中的数组元素将作为单独的参数传给 func 函数。如果该参数的值为 null 或 undefined，则表示不需要传入任何参数。从 ECMAScript 5 开始可以使用类数组对象。

- **返回值：**调用有指定this值和参数的函数的结果。

- **描述：**

  apply <font color=FF0000>可以使用数组字面量</font> ( array literal )，如 fun.apply(this, ['eat', 'bananas'])，或<font color=FF0000>数组对象</font>， 如  fun.apply(this, new Array('eat', 'bananas'))。

  你<font color=FF0000>也可以使用 arguments 对象作为 argsArray 参数</font>。 arguments 是一个函数的局部变量。 它可以被用作被调用对象的所有未指定的参数。 这样，你在使用 apply函数的时候就不需要知道被调用对象的所有参数。 你可以使用 arguments 来把所有的参数传递给被调用对象。 被调用对象接下来就负责处理这些参数。

- **示例：** 声明apply的功能可以通过 Function.prototype.call() 加上解构赋值来实现，当然这是ES6的写法。这里的示例都是ES5的

  **用 apply 将数组各项添加到另一个数组**

  ```js
  var array = ['a', 'b'];
  var elements = [0, 1, 2];
  array.push.apply(array, elements);
  console.info(array); // ["a", "b", 0, 1, 2]
  ```
  
  **注：**需要注意的是，由上例发现：在 apply 函数内传入的 argsArray，在处理时，不是以一个整个数组传入，而是会被展开一个一个元素传入函数，如下示例：
  
  ```js
  // 示例1
  function test(argArr) { console.log(argArr) }
  test.apply({}, [1, 2, 3]) // 1。结果只打印了 1
  
  // 示例2
  functionn test2() { console.log(arguments) }
  test2.apply({}, [1, 2, 3]) // { '0': 1, '1': 2, '2': 3 }。如果传入的整个数组，则应该打印 { '0': [1, 2, 3] }
  ```

  - **使用apply和内置函数**，用Math.max/Math.min求得数组中的最大/小值。注意下面的

    ```js
    /* 找出数组中最大/小的数字 */
    var numbers = [5, 6, 2, 3, 7];
    
    /* 使用Math.min/Math.max以及apply 函数时的代码 */
    var max = Math.max.apply(null, numbers); /* 基本等同于 Math.max(numbers[0], ...) 或 Math.max(5, 6, ..) */
    var min = Math.min.apply(null, numbers);
    ```

    **注意：**<font color=FF0000>如果按上面方式调用apply，有超出JavaScript引擎参数长度上限的风险</font>。<mark>一个方法传入过多参数（比如一万个）时的后果在不同JavaScript 引擎中表现不同。（JavaScriptCore引擎中有被硬编码的 参数个数上限：65536）。这是因为此限制（实际上也是任何用到超大栈空间的行为的自然表现）是不明确的</mark>。一些引擎会抛出异常，更糟糕的是其他引擎会直接限制传入到方法的参数个数，导致参数丢失。比如：假设某个引擎的方法参数上限为4（实际上限当然要高得多）, 这种情况下，上面的代码执行后, 真正被传递到 apply的参数为 5, 6, 2, 3 ，而不是完整的数组。

    <font color=FF0000>如果你的参数数组可能非常大，那么推荐使用下面这种混合策略：将数组切块后循环传入目标方法</font>：

    ```js
    function minOfArray(arr) {
      var min = Infinity;
      var QUANTUM = 32768;
    
      for (var i = 0, len = arr.length; i < len; i += QUANTUM) {
        var submin = Math.min.apply(null, arr.slice(i, Math.min(i + QUANTUM, len)));
        min = Math.min(submin, min);
      }
    
      return min;
    }
    
    var min = minOfArray([5, 6, 2, 3, 7]);
    ```

  - **使用apply来链接构造器** 

    可以使用apply来链接一个对象构造器。下面示例中会创建一个全局Function 对象的construct方法 ，来使你能够在构造器中使用一个类数组对象而非参数列表

    ```js
    Function.prototype.construct = function (aArgs) {
      var oNew = Object.create(this.prototype);
      this.apply(oNew, aArgs);
      return oNew;
    };
    ```

    因为Object.create() 是“相对”比较新的技术(ES5.1)，还有一些更老的技术实现（比如 object.\__proto__、闭包、Function构造器），由于看的时候水平不够，这里略。等水平够了，再来看

摘自：[MDN - Function.prototype.apply()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)

#### Function.prototype.bind()

bind() 方法<font color=FF0000>创建一个新的函数</font>，<mark>在 bind() 被调用时，这个新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用</mark>。

- **语法**

  ```js
  function.bind(thisArg[, arg1[, arg2[, ...]]])
  ```

- **参数**

  - **thisArg：**调用绑定函数时作为 this 参数传递给目标函数的值。 <font color=FF0000 size=4>**如果使用 new 运算符构造绑定函数，则忽略该值（注：bind的优先级没有new高，详细讲解可以参见：[[前端面试点总结#this的绑定（学习自 coderwhy 的文章）]] 中 “this 绑定操作”的优先级）**</font>。当<font color=FF0000>使用 bind 在 setTimeout 中创建一个函数（作为回调提供）时，作为 thisArg 传递的任何原始值都将转换为 object</font>。如果 <font color=FF0000>bind 函数的参数列表为空，或者thisArg是null或undefined，执行作用域的 this 将被视为新函数的 thisArg</font>。

  - **arg1, arg2, ...：**<font color=FF0000>当目标函数被调用时，被 <font size=4>**预置入**</font> 绑定函数的参数列表中的参数</font>。这些参数 偏函数中 会用到。

- **返回值：**返回一个原函数的拷贝，并拥有指定的 this 值和初始参数。

- **描述**

  <font color=FF0000>bind() 函数会创建一个新的绑定函数</font>（bound function，BF）。<font color=FF0000>绑定函数是一个 exotic function object</font>（怪异函数对象，ECMAScript 2015 中的术语），<font color=FF0000>它包装了原函数对象</font>。<font color=FF0000>调用绑定函数通常会导致执行包装函数</font>。

  **绑定函数具有以下内部属性：**

  - **[[BoundTargetFunction]]：**<font color=FF0000>包装的函数对象</font>
  - **[[BoundThis]]：**<font color=FF0000>在调用包装函数时始终作为 this 值传递的值</font>
  - **[[BoundArguments]]：**<font color=FF0000>列表</font>，在对包装函数做任何调用都会优先用列表元素填充参数列表。
  - **[[Call]]：**<font color=FF0000>执行与此对象关联的代码</font>。通过函数调用表达式调用。内部方法的参数是一个this值和一个包含通过调用表达式传递给函数的参数的列表。

  <font color=FF0000>当调用绑定函数时，它调用 [[BoundTargetFunction]] 上的内部方法 [[Call]]</font>，就像这样 <font color=FF0000>Call(boundThis, args)</font>。其中，<font color=FF0000>boundThis 是 [[BoundThis]]</font>，<font color=FF0000>args 是 [[BoundArguments]] 加上通过函数调用传入的参数列表</font>。

  绑定函数也可以使用 new 运算符构造，它会表现为目标函数已经被构建完毕了似的。提供的 this 值会被忽略，但前置参数仍会提供给模拟函数。

- **示例**

  - **创建绑定函数**

    <font color=FF0000>bind() 最简单的用法是创建一个函数，<font size=4>**不论怎么调用，这个函数都有同样的 this 值**</font></font> 。JavaScript新手经常犯的一个错误是将一个方法从对象中拿出来，然后再调用，期望方法中的 this 是原来的对象（比如在回调中传入这个方法）；<mark>前面说的这个问题就是：下面代码中的retireveX() 调用的结果为9，而不是81；原因是在全局作用域调用，且没有做绑定；所以，此时的this为window</mark>。如果不做特殊处理的话，一般会丢失原来的对象。基于这个函数，用原始的对象创建一个绑定函数，巧妙地解决了这个问题；<mark>即：下面的boundGetX</mark>：

    ```js
    this.x = 9;    // 在浏览器中，this 指向全局的 "window" 对象
    var module = {
      x: 81,
      getX: function() { return this.x; }
    };
    
    module.getX(); // 81
    
    var retrieveX = module.getX;
    retrieveX();
    // 返回 9，因为函数是在全局作用域中调用的
    
    // 创建一个新函数，把 'this' 绑定到 module 对象
    // 新手可能会将全局变量 x 与 module 的属性 x 混淆
    var boundGetX = retrieveX.bind(module);
    boundGetX(); // 81
    ```

  - **偏函数**

    <font color=FF0000>bind() 的另一个最简单的用法是 **使一个函数拥有预设的初始参数**</font>。只要<mark>将这些参数（如果有的话）作为 bind() 的参数写在 this 后面；当绑定函数被调用时，这些参数会被插入到目标函数的参数列表的开始位置，传递给绑定函数的参数会跟在它们后面</mark>。

    ```js
    function list() {
      return Array.prototype.slice.call(arguments);
    }
    
    function addArguments(arg1, arg2) { return arg1 + arg2 }
    
    // 直接使用这些函数
    var list1 = list(1, 2, 3); // [1, 2, 3]
    var result1 = addArguments(1, 2); // 3
    
    // 创建一个函数，它拥有预设参数列表。
    var leadingThirtysevenList = list.bind(null, 37);
    
    // 创建一个函数，它拥有预设的第一个参数
    var addThirtySeven = addArguments.bind(null, 37);
    
    var list2 = leadingThirtysevenList(); // [37]
    
    var list3 = leadingThirtysevenList(1, 2, 3); // [37, 1, 2, 3]
    
    var result2 = addThirtySeven(5); // 37 + 5 = 42
    
    var result3 = addThirtySeven(5, 10); // 37 + 5 = 42 ，第二个参数被忽略
    ```

  - **配合 setTimeout**

    在默认情况下，使用 window.setTimeout() 时，this 关键字会指向 window （或 global）对象。<font color=FF0000>当类的方法中需要 this 指向类的实例时，你可能需要显式地把 this 绑定到回调函数，就不会丢失该实例的引用</font>。

    ```js
    function LateBloomer() {
      this.petalCount = Math.ceil(Math.random() * 12) + 1;
    }
    
    // 在 1 秒钟后声明 bloom
    LateBloomer.prototype.bloom = function() {
      /* 注：这里的写法，在类的方法的 最外层、或类的方法中的不改变this指向的函数内层，this默认指向类的实例；所以这里bind(this) 指向的就是类的实例 */
      window.setTimeout(this.declare.bind(this), 1000);
    };
    
    LateBloomer.prototype.declare = function() {
      console.log('I am a beautiful flower with ' +
        this.petalCount + ' petals!');
    };
    
    var flower = new LateBloomer();
    flower.bloom();  // 一秒钟后, 调用 'declare' 方法
    ```

摘自：[MDN - Function.prototype.bind()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)

**补充：**偏函数

**什么时候会创建一个偏函数？**

好处是我们可以创建一个具有可读性高的名字（double，triple）的独立函数。我们可以使用它，并且不必每次都提供一个参数，因为参数是被绑定了的。

另一方面，当我们有一个非常通用的函数，并希望有一个通用型更低的该函数的变体时，偏函数会非常有用。

例如，我们有一个函数 send(from, to, text)。然后，在一个 user 对象的内部，我们可能希望对它使用 send 的偏函数变体：从当前 user 发送 sendTo(to, text)。

摘自：[js现代教程 - 函数绑定](https://zh.javascript.info/bind)

#### new 运算符

new 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。

- **语法**

  ```js
  new constructor[([arguments])]
  ```

- **参数**

  - **constructor：**一个指定对象实例的类型的类或函数。
  - **arguments：**一个用于被 constructor 调用的参数列表。

- **描述：** 

  **new 关键字会进行如下的操作：**

  1. <font color=FF0000>创建一个空的简单JavaScript对象（即{} ）</font>
  2. 为步骤1 <font color=FF0000 size=4>**新创建的对象添加属性 \__proto__，将该属性链接至构造函数的原型对象** ⭐️⭐️⭐️</font>
  3. 将步骤1 <font color=FF0000 size=4>**新创建的对象作为this的上下文**</font>
  4. 如果该函数没有返回对象，则返回this

  **当代码 new Foo(...) 执行时，会发生以下事情：**

  1. <font color=FF0000 size=4>**一个继承自 Foo.prototype 的新对象被创建**</font>

  2. 使用指定的参数调用构造函数 Foo，并将 this 绑定到新创建的对象。<font color=FF0000>new Foo 等同于 new Foo()，也就是没有指定参数列表，Foo 不带任何参数调用的情况</font>。如下示例

     ```js
     function Foo() { this.x = 0, this.y = 0 };
     const foo = new Foo // 这里没加括号
     console.log(foo) // Foo { x: 0, y: 0 }
     ```

  3. <font color=FF0000>由构造函数返回的对象就是 new 表达式的结果</font>。如果构造函数没有显式返回一个对象，则使用步骤1创建的对象。（一般情况下，构造函数不返回值，但是用户可以选择主动返回对象，来覆盖正常的对象创建步骤）

  始终可以对已定义的对象添加新的属性。例如，car1.color = "black" 语句给 car1 添加了一个新的属性 color，并给这个属性赋值 "black"。但是，<mark>这不会影响任何其他对象</mark>。要<font color=FF0000>将新属性添加到相同类型的所有对象，你必须将该属性添加到 Car 对象类型的定义中</font>。

  <font color=FF0000>可以使用 Function.prototype 属性将共享属性添加到以前定义的对象类型</font>。这<font color=FF0000>定义了一个由该函数创建的所有对象共享的属性，而不仅仅是对象类型的其中一个实例</font>。

  下面的代码将一个值为 null 的 color 属性添加到 car 类型的所有对象，然后仅在实例对象 car1 中用字符串 "black" 覆盖该值：

  ```js
  function Car() {}
  car1 = new Car();
  car2 = new Car();
  
  console.log(car1.color);    // undefined
  
  // 添加 “共享属性”
  Car.prototype.color = "original color";
  console.log(car1.color);    // original color
  
  car1.color = 'black';
  console.log(car1.color);   // black
  
  console.log(car1.__proto__.color) //original color
  console.log(car2.__proto__.color) //original color
  console.log(car1.color)  // black
  console.log(car2.color) // original color
  ```

  **注意：**如果<font color=FF0000>没有使用 new 运算符， 构造函数会像其他的常规函数一样被调用， 并不会创建一个对象</font>。在这种情况下， this 的指向也是不一样的。

摘自：[MDN - new 运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new)

#### Object.prototype.constructor

<font color=FF0000>**返回**创建实例对象的 Object **构造函数的引用**</font>。注意，此属性的值是对函数本身的引用，而不是一个包含函数名称的字符串。对原始类型来说，如1，true和"test"，该值只可读。

- **描述：**<font color=FF0000>所有对象都会从它的原型上继承一个 constructor 属性</font>

  ```js
  var o = {};
  o.constructor === Object; // true
  
  var o = new Object;
  o.constructor === Object; // true
  
  var a = [];
  a.constructor === Array; // true
  
  var a = new Array;
  a.constructor === Array // true
  
  var n = new Number(3);
  n.constructor === Number; // true
  ```

- **示例：**

  基本类型对象的 constructor 属性的值是可以修改的，<font color=FF0000>只有boolean number string 类型 在修改Object.prototype.constructor 后Object.prototype.constructor 不受影响（因为创建他们的是只读的原生构造函数 (native constructors)）</font>；其他JS类型（包括Boolean Number String）都是会被修改成功。

  另外，只修改constructor，不能修改instanceof 关系。通过instanceof 返回的还是false。

  这也说明了依赖一个对象的 constructor 属性并不安全。

  示例太长了，略。详见链接地址：[MDN - Object.prototype.constructor - 改变对象的 constructor](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor#example_changing_the_constructor_of_an_object)

摘自：[MDN - Object.prototype.constructor](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor)

#### Function

<font color=FF0000>每个 JavaScript 函数实际上都是一个 Function 对象</font>。运行 `  (function(){}).constructor === Function  ` 的结果为 true；便可以得到这个结论。

- **构造函数：<font color=FF0000>即 new Function()</font>**

  Function 构造函数创建一个新的 Function 对象。<font color=FF0000>直接调用此构造函数可用动态创建函数</font>，但会遇到和 eval 类似的的安全问题和（相对较小的）性能问题。然而，与 eval 不同的是，Function 创建的函数只能在全局作用域中运行。示例如下：

  ```js
  const sum = new Function('a', 'b', 'return a + b');
  ```

- **语法**

  ```js
  new Function ([arg1[, arg2[, ...argN]],] functionBody)
  ```

- **参数**

  - **arg1, arg2, ... argN：**被函数使用的参数的名称必须是合法命名的。参数名称是一个有效的JavaScript标识符的字符串，或者一个用逗号分隔的有效字符串的列表;例如“×”，“theValue”，或“a,b”。
  - **functionBody：**一个含有包括函数定义的 JavaScript 语句的字符串。

- **描述：**

  使用 Function 构造器生成的 Function 对象是在函数创建时解析的。这<font color=FF0000>比你使用函数声明或者函数表达式并在你的代码中调用 <font size=4>更为低效</font>，因为使用后者创建的函数是跟其他代码一起解析的</font>。

  所有被传递到构造函数中的参数，都将被视为将被创建的函数的参数，并且是相同的标示符名称和传递顺序。

  以调用函数的方式调用 Function 的构造函数（而不是使用 new 关键字) 跟以构造函数来调用是一样的。

- **属性和方法：**全局的 Function 对象没有自己的属性和方法，但是，因为它本身也是一个函数，所以它也会通过原型链从自己的原型链 Function.prototype 上继承一些属性和方法。

摘自：[MDN - Function](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function)

#### New Function 补充

以下三种声明的含义相同：

```js
new Function('a', 'b', 'return a + b'); // 基础语法
new Function('a,b', 'return a + b'); // 逗号分隔
new Function('a , b', 'return a + b'); // 逗号和空格分隔
```

摘自：[js现代教程 - "new Function" 语法](https://zh.javascript.info/new-function)

#### 内部属性 ( internal property )

ECMA-262定义了内部才用的特性是为了实现JavaScript引擎用的，因此在JS中不能直接访问它们。内部属性的特征是 通过[[]]包裹。

描述符对象就是一种内部属性，包含数据描述符 和 存取描述符：[[Configurable]] [[Enumerable]] [[writable]] [[value]] [[Get]] [[Set]]

摘自：[JS属性-------内部属性（数据属性和访问器属性）](https://blog.csdn.net/baidu_36065997/article/details/79110331)

// TODO

摘自：[不为人知的javascript的内部属性](https://segmentfault.com/a/1190000012842459)

**内部属性还有：**[[ConstructorKind]] 用于构造器中，[[HomeObject]] 只用于super（方法在内部的 [[HomeObject]] 属性中记住了它们的类/对象。这就是 super 如何解析父方法的）



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
  var x = document.getElementsByClassName("intro");
  ```

**补充：**

#### **Element.classList**

<font color=FF0000>Element.classList 是一个只读属性，返回一个元素的类属性的实时 DOMTokenList 集合</font>。
相比将 element.className 作为以空格分隔的字符串来使用，classList 是一种更方便的访问元素的类列表的方法。

- **语法**
  
  ```js
  const elementClasses = elementNodeReference.classList;
  ```

- **返回值**
  elementClasses 是一个 DOMTokenList 表示  elementNodeReference 的类属性 。如果类属性未设置或为空，那么 elementClasses.length 返回 0。<font color=FF0000>element.classList 本身是只读的，但是你可以使用 add(className) 和 remove(className) 方法修改它</font>。
  
  - **add(className) /  remove(className)：**甚至其中的add() / remove()可以放多组类值，甚至可以通过使用展开语法添加或移除多个类值。示例如下：
    
    ```js
    // 添加或移除多个类值
    div.classList.add("foo", "bar", "baz");
    div.classList.remove("foo", "bar", "baz");
    
    // 使用展开语法添加或移除多个类值
    const cls = ["foo", "bar"];
    div.classList.add(...cls);
    div.classList.remove(...cls);
    ```
  
  - **item(index)：**返回元素中索引值对应的类名。索引值从 0 开始。如果索引值在区间范围外则返回 null
  
  - **toggle(className)：**如果该类值已存在，则移除它（并返回true），否则添加它（并返回false）。另外，该函数还可添加触发条件：`toggle(className, conditionExpression)`若条件满足则添加/ 移除该类值。
    
    ```js
    // 如果 visible 类值已存在，则移除它，否则添加它
    div.classList.toggle("visible");
    
    // add/remove visible, depending on test conditional, i less than 10
    div.classList.toggle("visible", i < 10 );
    ```
  
    <font size=4>**补充：**</font>
    
    **DOMTokenList.toggle()**
    
    DOMTokenList 接口的 toggle() 方法从列表中删除一个给定的标记 并返回 false 。 如果标记 不存在，则添加并且函数返回 true。
    
    - **语法**
    
      ```js
      tokenList.toggle(token, force);
      ```
    
    - 参数列表
    
      - **token：**标记列表中你想探查并切换的 DOMString .
      - **force：**可选，一个Boolean值, 设置后会将方法变成单向操作. 如设置为false, 则会删除标记列表中匹配的给定标记，且不会再度添加. 如设置为 true, 则将在标记列表中添加给定标记，且不会再度删除。
    
    - **返回值：**该方法返回一个Boolean (en-US)值 — 如给定标记不存在于列表中返回false , 标记存在则返回true 。
    
    摘自：[MDN - DOMTokenList.toggle()](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMTokenList/toggle)
    
  - **contains(className)：**判断是否存在该类值
  
  - **replace(replacedClassName, replacingClassName)：**将replacedClassName替换为replacingClassName。
    
    ```js
    // 将类值 "foo" 替换成 "bar"
    div.classList.replace("foo", "bar");
    ```

摘自：[MDN - Element.classList](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/classList)

#### JavaScript HTML DOM - 改变 HTML

如需改变 HTML 元素的属性，请使用这个语法：

```js
document.getElementById(id).attribute = new-attribute
```

#### JavaScript HTML DOM - 改变CSS

如需改变 HTML 元素的样式，请使用这个语法：

```js
document.getElementById(id).style.property = new-style
```

**使用事件**

HTML DOM 允许我们通过触发事件来执行代码。比如以下事件：

- 元素被点击。
- 页面加载完成。
- 输入框被修改。
- ……

示例如下：

```html
<!-- 注意：这里onclick中的js代码被引号""包含 -->
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

<font size=4>**补充：**</font><font size=4>**CSSStyleDeclaration.getPropertyValue()**</font>

CSSStyleDeclaration.getPropertyValue() 接口返回一个 DOMString ，其中包含请求的 CSS 属性的值。

- **语法**

  ```js
  var value = style.getPropertyValue(property);
  ```

- **参数**
  property 是一个 DOMString，是需要查询的CSS 属性名称。

- **返回值**
  value 是 DOMString ，包含查找属性的值。若对应属性没有设置，则返回空字符串。

- **示例**
  下述示例使用一个CSS选择器规则查询对应的margin 属性的值：

  ```js
  var declaration = document.styleSheets[0].cssRules[0].style;
  var value = declaration.getPropertyValue('margin'); // "1px 2px"
  ```

摘自：[MDN - CSSStyleDeclaration.getPropertyValue()](https://developer.mozilla.org/zh-CN/docs/Web/API/CSSStyleDeclaration/getPropertyValue)

#### CSSConditionRule

CSSConditionRule 对象表示单个条件 CSS 规则, 由条件和语句块组成。继承至 CSSGroupingRule.

<font color=FF0000>从它派生的两个对象 : CSSMediaRule and CSSSupportsRule</font>.

**语法**
该语法使用所描述的WebIDL 格式：

```web-idl
interface CSSConditionRule : CSSGroupingRule {
    attribute DOMString conditionText;
}
```

摘自：[MDN - CSSConditionRule](https://developer.mozilla.org/zh-CN/docs/Web/API/CSSConditionRule)

#### CSSSupportsRule

该 CSSSupportsRule <font color=FF0000>**接口**</font> <font color=FF0000>描述了代表一个 CSS 对象 @supports at-rule。它实现了 CSSConditionRule 接口</font>，因此 CSSRule指定规则 和CSSGroupingRule 用一个类型值接口 12 (CSSRule.SUPPORTS\_RULE)。

**语法**
该语法使用所描述的WebIDL 格式：

```web-idl
interface CSSSupportsRule : CSSConditionRule {}
```

摘自：[MDN - CSSSupportsRule](https://developer.mozilla.org/zh-CN/docs/Web/API/CSSSupportsRule)

#### CSSMediaRule

CSSMediaRule 是一个<font color=FF0000>表示单个CSS @media 规则的接口。它实现了 CSSConditionRule 接口</font>，因此也是 CSSGroupingRule，也相当于CSSRule中类型值为 4 的规则类型（即CSSRule.MEDIA_RULE）。

**语法**

下列语法是使用 WebIDL 格式描述的。

```web-idl
interface CSSMediaRule : CSSConditionRule {
    readonly attribute MediaList media;
}
```

摘自：[MDN - CSSMediaRule](https://developer.mozilla.org/zh-CN/docs/Web/API/CSSMediaRule)

#### Window.matchMedia()

Window 的 matchMedia() 方法 <font color=FF0000>返回一个新的 MediaQueryList 对象</font>，<font color=FF0000>表示指定的媒体查询字符串解析后的结果</font>。返回的 MediaQueryList 可被用于判定 Document 是否匹配媒体查询，或者监控一个 document 来判定它匹配了或者停止匹配了此媒体查询。

- **语法：**

  ```js
  mqList = window.matchMedia(mediaQueryString)
  ```

  - **参数：**

     **mediaQueryString：**一个被用于媒体查询解析的字符串。

  - **返回值：**一个用来媒体查询的新的 MediaQueryList 对象

- **使用说明：**
  您可以使用返回的媒体查询来执行即时检查和事件驱动检查，以查看文档是否与媒体查询匹配。

  要执行一次瞬时检查以查看文档是否与媒体查询匹配，请查看 matches 属性（注：这里的 matches 属性 应该是 mql 中的。具体参见下面的示例）的值，当 document 满足媒体查询条件的时候将会返回 true。

- **示例：**

  此示例运行媒体查询 (max-width: 600px) 并在 \<span> 中显示 MediaQueryList 的 matches 属性值。如果视口的宽度小于或等于 600像素，则输出将为 true，而如果窗口的宽度大于此宽度，则将输出 false。

  JavaScript 代码只需将要匹配的媒体查询字符串传递到 matchMedia() 进行编译，然后设置 \<span> 的 innerText为matches 属性结果的值，以便它表明此 document 在此刻页面加载完成时是否与媒体查询所匹配。

  ```html
  <span class="mq-value"></span>
  <script>
  	let mql = window.matchMedia('(max-width: 600px)');
  	document.querySelector(".mq-value").innerText = mql.matches;
  </script>
  ```

  **注：**对mql 进行打印，结果如下；可以看出 MediaQueryList 的结构：

  ![image-20220212150611427](https://s2.loli.net/2022/02/12/p19cOE83JwjZKnt.png)

摘自：[MDN - Window.matchMedia()](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/matchMedia)



#### JavaScript HTML DOM EventListener

**addEventListener() 方法**

- addEventListener() 方法用于<font color=FF0000>向指定元素添加事件句柄</font>。
- addEventListener() 方法<font color=FF0000>添加的事件句柄**不会覆盖已存在的事件句柄**</font>。
- 你<font color=FF0000>可以**向一个元素添加多个事件句柄**</font>。
- 你可以<font color=FF0000>向同个元素添加多个同类型的事件句柄</font>，如：两个 "click" 事件。
- 你可以<font color=FF0000>向**任何 DOM 对象**添加事件监听</font>，不仅仅是 HTML 元素。如： window 对象。
- addEventListener() 方法可以更简单的控制事件（<font color=FF0000>冒泡与捕获</font>）。

<font size=4>**补充：**</font>

**EventTarget.addEventListener()（摘自MDN）**

EventTarget.addEventListener() 方法<font color=FF0000>将指定的监听器 <font size=4>**注册到 EventTarget 上**</font></font>，<mark>当该对象触发指定的事件时，指定的回调函数就会被执行</mark>。 <mark>**事件目标可以是一个文档上的元素 Element, Document和Window或者任何其他支持事件的对象 (比如 XMLHttpRequest)**</mark>。

addEventListener()的 <font color=FF0000>**工作原理**是将实现EventListener的函数或对象添加到调用它的EventTarget上的指定事件类型的事件侦听器列表中</font>。

**语法：**

```js
target.addEventListener(type, listener, options);
target.addEventListener(type, listener, useCapture);
```

**参数：**

- **type：**表示<font color=FF0000>监听事件类型</font>的字符串。

- **listener：**当所监听的事件类型触发时，会接收到一个事件通知（实现了 Event 接口的对象）对象。<font color=FF0000>listener 必须是一个实现了 EventListener 接口的对象，或者是一个函数</font>。

- **options：<font color=FF0000>可选</font>**，一个指定有关 listener 属性的<font color=FF0000>可选参数对象</font>。可用的选项如下：
  - **capture:**  Boolean，表示 listener 会在该类型的事件<font color=FF0000>捕获阶段传播到该 EventTarget 时触发</font>。（<font color=FF0000>默认为false</font>）
  - **once:**  Boolean，<font color=FF0000>表示 listener 在添加之后最多只调用一次</font>。<mark>如果是 true， listener 会在其被调用之后自动移除</mark>。
  - **passive:**  Boolean，<font color=FF0000>设置为true时，表示 listener 永远不会调用 preventDefault()</font>。<mark>如果 listener 仍然调用了这个函数，客户端将会忽略它并抛出一个控制台警告</mark>。
  - **signal：**AbortSignal，该 AbortSignal 的 abort() 方法被调用时，监听器会被移除。

- **useCapture： <font color=FF0000>可选</font>**，Boolean，<font color=FF0000>在DOM树中，**注册了listener的元素， 是否要先于它下面的EventTarget，调用该listener**</font>。 <font color=FF0000 size=4>**当useCapture（设为true）时，沿着DOM树向上冒泡的事件，不会触发listener**</font>。当一个元素嵌套了另一个元素，并且两个元素都对同一事件注册了一个处理函数时，所发生的事件冒泡和事件捕获是两种不同的事件传播方式。事件传播模式决定了元素以哪个顺序接收事件。 **如果没有指定， <font color=FF0000>useCapture 默认为 false</font> **。 


**备注：**
为什么要使用 addEventListener? addEventListener() 是 W3C DOM 规范中提供的注册事件监听器的方法。它的优点包括：

- 它<font color=FF0000>允许给一个事件注册多个监听器</font>。 特别是在使用AJAX库，JavaScript模块，或其他需要第三方库/插件的代码。
- 它<font color=FF0000>提供了一种更精细的手段控制 listener 的触发阶段</font>。（即可以选择捕获或者冒泡）。
- 它对任何 DOM 元素都是有效的，而不仅仅只对 HTML 元素有效。

摘自：[MDN - EventTarget.addEventListener()](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener)



**removeEventListener()方法**

<font color=FF0000>删除使用 EventTarget.addEventListener() 方法添加的事件</font>。使用事件类型，事件侦听器函数本身，以及可能影响匹配过程的各种可选择的选项的组合来标识要删除的事件侦听器。

**语法：**

```js
target.removeEventListener(type, listener[, options]);
target.removeEventListener(type, listener[, useCapture]);
```

**参数**

- **type：**一个字符串，表示需要移除的事件类型，如 "click"。

- **listener：**<font color=FF0000>需要从目标事件移除的 EventListener 函数</font>。

- **options：**<mark style=background-color:hotpink>可选</mark>，一个指定事件侦听器特征的可选对象。可选项有：

  - **capture：**一个 Boolean 表示这个类型的事件将会被派遣到已经注册的侦听器，然后再派遣到DOM树中它下面的任何 EventTarget。

- **useCapture：**<mark style=background-color:hotpink>可选</mark>，指定需要移除的 EventListener 函数是否为捕获监听器。如果无此参数，默认值为 false。

  如果同一个监听事件分别为“事件捕获”和“事件冒泡”注册了一次，这两次事件需要分别移除。两者不会互相干扰。移除捕获监听器不会影响非捕获版本的相同监听器，反之亦然。

**返回值：**undefined

摘自：[MDN - EventTarget.removeEventListener](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/removeEventListener)



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

- **事件冒泡：**微软提出了名为 事件冒泡(event bubbling)的事件流。事件冒泡可以形象地比喻为把一颗石头投入水中，泡泡会一直从水底冒出水面。也就是说，事件会从最内层的元素开始发生，一直向上传播，直到document对象。
  
  因此在事件冒泡的概念下在p元素上发生click事件的顺序应该是**`p -> div -> body -> html -> document`**
  
- **事件捕获：**网景提出另一种事件流名为 事件捕获(event capturing)。与事件冒泡相反，事件会从最外层开始发生，直到最具体的元素。
  
  因此在事件捕获的概念下在p元素上发生click事件的顺序应该是**`document -> html -> body -> div -> p`**

摘自：[JS中事件冒泡与捕获](https://segmentfault.com/a/1190000005654451) 具体更多可以参考：[Javascript info -- 冒泡和捕获](https://zh.javascript.info/bubbling-and-capturing)



#### event.stopPropagation

<font color=FF0000>**阻止**捕获和冒泡阶段中**当前事件的进一步传播**。</font>

但是，它不能防止任何默认行为的发生； 例如，对链接的点击仍会被处理。

如果要停止这些行为，请参见 preventDefault 方法，它可以阻止事件触发后默认动作的发生。

摘自：[MDN - event.stopPropagation](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/stopPropagation)

#### event.preventDefault

Event 接口的 preventDefault()方法，告诉user agent：如果此事件（自定义的事件）没有被显式处理，它默认的动作也不应该照常执行。

此事件还是继续传播，除非碰到事件侦听器调用stopPropagation() 或stopImmediatePropagation()，才停止传播。

**备注：**

在事件流的任何阶段调用 preventDefault() 都会取消事件，这意味着任何通常被该实现触发并作为结果的默认行为都不会发生。

你可以使用 Event.cancelable 来检查该事件是否支持取消。为一个不支持cancelable的事件调用preventDefault()将没有效果。

摘自：[MDN - event.preventDefault](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/preventDefault)

#### event.defaultPrevented

**概述：**返回一个布尔值，表明当前事件是否调用了 event.preventDefault()方法。

**语法**

```js
bool = event.defaultPrevented
```

摘自：[MDN - event.defaultPrevented](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/defaultPrevented)



####  HTML DOM 元素 (节点)

- **appendChild(node)：**创建新的 HTML 元素 (节点)，用于<font color=FF0000>添加新元素到尾部</font>
  
  ```js
  element.appendChild(aChild)
  ```
  
  **参数**
  
  - **aChild：**要追加给父节点（通常为一个元素）的节点。
  - **返回值：**<font color=FF0000>返回追加后的子节点 （aChild）</font>，<mark>除非 aChild 是一个文档片段（DocumentFragment），这种情况下将返回空文档片段（DocumentFragment）</mark>。
  
  要创建新的 HTML 元素 (节点)需要先创建一个元素`，然后在已存在的元素中添加它。
  
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
  
  **补充：**
  
  <mark>如果将被插入的节点已经存在于当前文档的文档树中，那么 appendChild() 只会将它从原先的位置移动到新的位置（不需要事先移除要移动的节点）</mark>。 这意味着，一个节点不可能同时出现在文档的不同位置。所以，如果某个节点已经拥有父节点，在被传递给此方法后，它首先会被移除，再被插入到新的位置。<mark style="background: aqua">若要保留已在文档中的节点，可以先使用  Node.cloneNode() 方法来为它创建一个副本，再将副本附加到目标父节点下。请注意，用 cloneNode 制作的副本不会自动保持同步</mark>。
  
  部分摘自：[MDN - Node.appendChild](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/appendChild)
  
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

#### document.getElementById()

Document的方法 getElementById()返回一个匹配特定 ID的元素。由于<font color=FF0000>元素的 ID 在大部分情况下要求是独一无二的</font>，这个方法自然而然地成为了一个高效查找特定元素的方法。
<font color=FF0000>如果需要查找到那些没有ID 的元素，你可以考虑通过CSS选择器使用 querySelector()（见下面）。</font>

- **语法**
  
  ```js
  var element = document.getElementById(id);
  ```

- **参数**
  
  - element是一个 Element 对象。如果当前文档中拥有特定ID的元素不存在则返回null.
  - <font color=FF0000>id是大小写敏感的字符串</font>，代表了所要查找的元素的唯一ID.

- **返回值**
  
  - 返回一个匹配到 ID 的 DOM Element 对象。<font color=FF0000>若在当前 Document 下没有找到，则返回 null</font>。

#### document.getElementsByClassName()

<font color=FF0000>返回一个包含了所有指定类名的子元素的**类数组对象**</font>。 <mark>当在document对象上调用时，会搜索整个DOM文档，包含根节点</mark>。<mark style="background: aqua">**你也可以在任意元素上调用getElementsByClassName() 方法，它将返回的是以当前元素为根节点，所有指定类名的子元素。**</mark>

**语法**

```js
var elements = document.getElementsByClassName(names); // 或者
var elements = rootElement.getElementsByClassName(names);
```

- elements 是一个<font color=FF0000>实时集合</font>，包含了找到的所有元素。
- names 是一个字符串，表示要匹配的类名列表；类名通过空格分隔
- getElementsByClassName 可以在任何元素上调用，不仅仅是 document。 调用这个方法的元素将作为本次查找的根元素.

**示例**

- 获取所有 class 为 'test' 的元素：

  ```js
  document.getElementsByClassName('test');
  ```

- 获取所有 class <font color=FF0000>同时包括 'red' 和 'test' 的元素</font>：

  ```js
  document.getElementsByClassName('red test');
  ```

- <font color=FF0000>在id 为'main'的元素的子节点中，获取所有class为'test'的元素</font>：

  ```js
  document.getElementById('main').getElementsByClassName('test');
  ```

- 我们还可以对任意的  HTMLCollection 使用 Array.prototype 的方法，调用时传递  HTMLCollection 作为方法的参数。这里我们将查找到所有class为 'test' 的 div 元素:

  ```js
  var testElements = document.getElementsByClassName('test');
  var testDivs = Array.prototype.filter.call(testElements, function(testElement){
      return testElement.nodeName === 'DIV';
  });
  ```

摘自：[MDN - Document.getElementsByClassName()](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementsByClassName)



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

#### **补充**：document.querySelector() 和document.querySelectorAll()

**document.querySelector()**：文档对象模型Document引用的querySelector()方法<font color=FF0000>返回文档中与指定选择器或选择器组匹配的**第一个 HTMLElement对象**</font>。 <font color=FF0000>如果找不到匹配项，则返回null</font>。( 匹配是使用<font color=FF0000>**深度优先先序遍历**</font>，从文档标记中的第一个元素开始，并按子节点的顺序依次遍历。)

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

<font size=4>**补充：**</font>

<font color=FF0000>**在ES<font size=5>5</font>**</font>中，类的定义如下：

```js
function Foo(member1, ...) {
  this.member1 = member1
  ...
}
```

想要在类中定义成员函数：

```js
Foo.prototype.fnName = function() {
  ...
}
```

定义静态属性：

```js
Foo.staticMember = initVal
```

定义静态方法：

```js
Foo.staticFunc = function() {
  ....
}
```

**定义子类：<font color=FF0000>组合式继承（不仅继承属性，也继承方法；同时都继承无法一步到位，既需要继承属性，也需要继承方法）</font>**

```js
function Bar(member1, member2, ...) {
  Foo(this, member1) // 父类构造函数，只能继承属性
  this.member2 = member2
}
// 继承方法
Bar.prototype = new Foo()
Bar.prototype.constuctor = Foo
```



#### JavaScript Number 对象

**<font color=FF0000>所有 JavaScript 数字均为 64 位</font>**

JavaScript 不是类型语言。与许多其他编程语言不同，<mark>JavaScript 不定义不同类型的数字，比如整数、短、长、浮点等等</mark>。

在JavaScript中，<mark>数字不分为整数类型和浮点型类型，<font color=FF0000>**所有的数字都是由 浮点型类型**</font></mark>。JavaScript 采用 IEEE 754 标准定义的 64 位浮点格式表示数字，它能表示最大值为 ±1.7976931348623157e+308，最小值为 ±5e-324。

| 值 (aka Fraction/Mantissa) | 指数                | Sign       |
|:-------------------------:|:-----------------:|:----------:|
| 52 bits (0 - 51)          | 11 bits (50 - 62) | 1 bit (63) |

**精度**

- 整数（不使用小数点或指数计数法）最多为 15 位。
  
  - 超过15位的一律变成100,000,000,000,000,000（15个0）

- 小数的最大位数是 17

**八进制和十六进制**

要加上前缀 `0` 和 `0x`

默认情况下，JavaScript 数字为十进制显示。但是你可以使用 toString() 方法 输出16进制、8进制、2进制。

示例：

```js
var myNumber = 128;
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

<font size=4>**补充：**</font>

isNaN() 方法是 ES5 的方法，实际上是 window.isNaN() 的简写；而在 ES6中添加了 Number.isNaN()，之所以添加，是为了避免 window 下挂载的方法过多，而 Number.isNaN() 是一种更优 / 更合适的设计 / 模块化设计

同样的还有 Number.parseInt() / Number.parseFloat() / Number.isFinite()



**Number 属性**

| 属性                     | 描述                                                         |
| :----------------------- | :----------------------------------------------------------- |
| Number.MAX_VALUE         | 最大值                                                       |
| Number.MIN_VALUE         | 最小值                                                       |
| Number.NaN               | 非数字                                                       |
| Number.NEGATIVE_INFINITY | 负无穷，在溢出时返回                                         |
| Number.POSITIVE_INFINITY | 正无穷，在溢出时返回                                         |
| Number.EPSILON           | 表示 1 和比最接近 1 且大于 1 的最小 Number 之间的差别        |
| Number.MIN_SAFE_INTEGER  | 最小安全整数。 -(2^53^ - 1)<br>属性同下                      |
| Number.MAX_SAFE_INTEGER  | 最大安全整数。 2^53^ - 1<br>属性描述符：writable: false, enumerable: false, configurable: false<br>Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2 将得到 true的结果，而这在数学上是错误的<br>由于 MAX_SAFE_INTEGER 是Number的一个静态属性 |

**数字方法**

| 方法                   | 描述                                                         |
| :--------------------- | :----------------------------------------------------------- |
| Number.parseFloat()    | 将字符串转换成浮点数，和全局方法 [parseFloat()](https://www.runoob.com/jsref/jsref-parsefloat.html) 作用一致。 |
| Number.parseInt()      | 将字符串转换成整型数字，和全局方法 [parseInt()](https://www.runoob.com/jsref/jsref-parseint.html) 作用一致。 |
| Number.isFinite()      | 判断传递的参数是否为有限数字。                               |
| Number.isInteger()     | 判断传递的参数是否为整数。                                   |
| Number.isNaN()         | 判断传递的参数是否为 isNaN()。                               |
| Number.isSafeInteger() | 判断传递的参数是否为安全整数。<br>安全整数范围为 -(2^53^ - 1)到 2^53^ - 1之间的整数，包含 -(2^53^ - 1)和 2^53^ - 1。 |

**数字类型原型上的一些方法**

| 方法            | 描述                                                         |
| :-------------- | :----------------------------------------------------------- |
| toExponential() | 返回一个数字的指数形式的字符串，如：1.23e+2                  |
| toFixed()       | 返回指定小数位数的表示形式。<br />var a=123; b=a.toFixed(2); // b="123.00" |
| toPrecision()   | 返回一个指定精度的数字。如下例子中，a=123 中，3会由于精度限制消失：<br />var a=123; b=a.toPrecision(2); // b="1.2e+2" |

#### Number.prototype.toString()

toString() 方法返回指定 Number 对象的字符串表示形式。

**语法**

```js
numObj.toString([radix])
```

**参数**

- **radix：**<font color=FF0000>指定要用于数字到字符串的转换的基数（从2到36）</font>。如果未指定 radix 参数，则默认值为 10。

**异常信息**

- **RangeError：**<font color=FF0000>如果 toString() 的 radix 参数不在 2 到 36 之间，将会抛出一个 RangeError</font>。

**描述**
<font color=FF0000>Number 对象覆盖了 Object 对象上的 toString() 方法，它不是继承的 Object.prototype.toString()</font>。对于 Number 对象，toString() 方法以指定的基数返回该对象的字符串表示。

如果转换的基数大于10，则会使用字母来表示大于9的数字，比如基数为16的情况，则使用a到f的字母来表示10到15。

如果基数没有指定，则使用 10。

<font color=FF0000>**如果对象是负数，则会保留负号**</font>。即使radix是2时也是如此：返回的字符串包含一个负号（-）前缀和正数的二进制表示，不是 数值的二进制补码。

<font color=FF0000>**进行数字到字符串的转换时，建议用小括号将要转换的目标括起来，防止出错**</font>。

**示例：**

```js
console.log((-10).toString(2));   // 输出 '-1010'
console.log((-0xff).toString(2)); // 输出 '-11111111'
```

摘自：[MDN - Number.prototype.toString()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toString)

#### Math.sign()

Math.sign() 函数返回一个数字的符号, 指示数字是正数，负数还是零。

因为 sign 是 Math 的一个静态方法。<font color=FF0000>此函数共有5种返回值, 分别是 1, -1, 0, -0, NaN</font>。代表的各是正数, 负数, 正零, 负零, NaN。

摘自：[MDN - Math.sign()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/sign)

#### Math.cbrt()

Math.cbrt() 函数返回任意数字的立方根

cbrt 是 "cube root" 的缩写, 意思是立方根

摘自：[MDN - Math.cbrt()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/cbrt)

<font size=4>**补充：**</font>

**进制前缀**

- **二进制：**0B / 0b
- **八进制：** 0 / 0o / 0O
- **十六进制：** 0x / 0X



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



#### JS时间戳与日期之间的转换

- **将时间戳转换成日期格式**

  ```js
  var date = new Date(timeStamp); //获取一个时间对象
  
  date.getFullYear();  // 获取完整的年份(4位,1970)
  date.getMonth();  // 获取月份(0-11,0代表1月,用的时候记得加上1)
  date.getDate();  // 获取日(1-31)
  date.getTime();  // 获取时间(从1970.1.1开始的毫秒数)
  date.getHours();  // 获取小时数(0-23)
  date.getMinutes();  // 获取分钟数(0-59)
  date.getSeconds();  // 获取秒数(0-59)
  ```

- **将日期格式转换成时间戳**

  ```js
  var timeStr = '2014-04-23 18:55:49:123';
  var date = new Date(timeStr); //传入一个时间格式，如果不传入就是获取现在的时间了，这样做不兼容火狐。
  // 可以这样做
  var date = new Date(timeStr.replace(/-/g, '/'));
  
  // 有三种方式获取
  time1 = date.getTime();  // 1398250549123
  time2 = date.valueOf(); // 1398250549123
  time3 = Date.parse(date); // 1398250549000 
  ```

  **三种获取的区别：**

  第一、第二种：会精确到毫秒；第三种：只能精确到秒，毫秒将用0来代替。比如上面代码输出的结果(一眼就能看出区别)：

  摘自：[js时间戳与日期格式之间的互转](https://segmentfault.com/a/1190000000481753)



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
  
  另外，Array.isArray() 是 ES6的语法，如果在ES5中，可以用 `arrVariable instanceof Array`  替代。



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

<font size=4>**补充：**</font>

**Math.trunc()**
Math.trunc() 方法会将数字的小数部分去掉，只保留整数部分。

- **语法**

  ```js
  Math.trunc(value)
  ```

- **参数**：value 任意数字
- **返回值：**给定数字的整数部分

因为 trunc() 是 Math 对象的静态方法，你必须用 Math.trunc() 来使用，而不是调用你创建的 Math 对象的一个实例方法（Math 没有构造函数）

摘自：[MDN - Math.trunc()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/trunc)



#### delete

如果delete删除成功，则返回true；若删除失败（比如不存在该属性），则返回false

delete只能删除对象的属性，而不能删除变量；如下：

```js
let a = 10;
delete a; // 会返回false
console.log(a); // 10
```



#### JavaScript Window - 浏览器对象模型

浏览器对象模型（**B**rowser **O**bject **M**odel (BOM)）尚无正式标准。但所有浏览器都支持 window 对象。它表示浏览器窗口。

由于现代浏览器已经（几乎）实现了 JavaScript 交互性方面的相同方法和属性，因此常被认为是 BOM 的方法和属性。

<font color=FF0000>所有 JavaScript 全局对象、函数以及变量均自动成为 window 对象的成员</font>。全局变量是 window 对象的属性，全局函数是 window 对象的方法；甚至 HTML DOM 的 document 也是 window 对象的属性之一

**其他一些 Window 方法**

- window.open() ：打开新窗口
- window.close() ：关闭当前窗口
- window.moveTo() ：移动当前窗口
- window.resizeTo() ：调整当前窗口的尺寸，设置绝对尺寸
- window.resizeBy()：调整当前窗口的尺寸，改变相对尺寸

<font size=4>**补充：**</font>屏幕 / 窗口 / 元素  的  高度 / 宽度

- **<font color=FF0000>整个屏幕</font> 逻辑分辨率上的 宽高度**
  
  - **window.screen.height：**屏幕高度 （整个屏幕 逻辑分辨率上的 高度）
  - **window.screen.width：**屏幕宽度（整个屏幕 逻辑分辨率上的 宽度）
  
- **获取屏幕<font color=FF0000>可工作区域的</font>高度和宽度（<font color=FF0000>去掉状态栏</font>）**

  ​	<font color=FF0000>**只去除了上面的状态栏，打开控制台不受影响**</font>。<font color=FF0000>**是一个固定值，不受浏览器窗口大小而改变**</font>

  ​	即：<mark>window.screen.availHeight = window.screen.height - 浏览器上面状态栏的高度</mark>

  - **window.screen.availHeight：**可视区域<font color=FF0000>去除状态栏高度 </font>
  - **window.screen.availWidth：**可视区域<font color=FF0000>去除状态栏宽度</font>

- 上面的window.screen.availHeight / window.screen.availWidth不受浏览器窗口大小而改变，而<font color=FF0000>window.outerHeight / window.outerWidth**受影响**</font>

  - <mark style="background: aqua">**window.outerHeight ：**</mark>浏览器<font color=FF0000>**当前可见区域**（<font size=4>**包含**</font>上方工具栏 / 下方的控制台）</font>的高度
  - <mark style="background: fuchsia">**window.outerWidth ：**</mark>浏览器<font color=FF0000>**当前可见区域**（<font size=4>**包含**</font>控制台）</font>的宽度

- **滚动条卷上去的高度和向右卷的宽度**

  - document.body.scrollTop
  - document.body.scrollLeft

- **网页可见区域的高度和宽度**

  去除浏览器上面的状态栏和控制台高度剩下的可以显示网页内容的高度。同理：如果控制台在左/右侧，则去除浏览器左/右控制台宽度剩下的可以显示网页内容的宽度

  - **不加边线**

    - <mark style="background: lime">**document.body.clientHeight：**</mark> 可见高度，包括被滚动条隐藏的部分；如果页面长到有滚动条，则clientHeight值会比screen.height长
    - <mark style="background: DodgerBlue">**document.body.clientWidth：**</mark> 同上，不过把高度换成宽度

  - **网页可见区域的高度和宽度（加边线）**

    - **document.body.offsetHeight：**body总高度；该属性和<mark style="background: lime">document.body.clientHeight</mark>相对应。

    - **document.body.offsetWidth：**body总宽度；该属性和<mark style="background: DodgerBlue">document.body.clientWidth</mark>相对应：

    - **window.innerHeight：**包含滚动条的高度，浏览器<font color=FF0000>**当前可见区域**、<font size=4>**不包含**</font>上方工具栏 / 下方控制台</font>的高度

      该属性和<mark style="background: aqua">window.outerHeight</mark>相对应：工具栏高度 [ + 控制台高度]  = <mark style="background: aqua">window.outerHeight</mark> - window.innerHeight

    - **window.innerWidth：**包含滚动条的宽度，浏览器<font color=FF0000>**当前可见区域**、<font size=4>**不包含**</font>可能存在的控制台</font>的宽度

      该属性和<mark style="background: fuchsia">window.outerWidth</mark>相对应： 控制台宽度 = <mark style="background: fuchsia">window.outerHeight</mark> - window.innerHeight

    - document.documentElement.clientHeight

    - document.documentElement.clientWidth    //去除滚动条的宽度

- **一般元素**（上面 **`document.body.*`** 有的，一般元素也会有；而**`window.*`** 有的，一般元素没有）

  - **Element.clientHeight：**<font color=FF0000>只读属性</font>，<font color=FF0000>对于没有定义CSS或者内联布局盒子的元素为0</font>，否则，它是元素内部的高度（单位像素）；<font color=FF0000>Element.clientHeight包含内边距</font>，但不包括水平滚动条、边框和外边距。

    clientHeight 可以通过 CSS height + CSS padding - 水平滚动条高度 (如果存在)来计算。

    <mark>Element.clientHeight<font color=FF0000>会将获取的值四舍五入取整数</font></mark>。 如果你需要小数结果, 请使用 element.getBoundingClientRect().

  - **Element.clientWidth：**<font color=FF0000>只读属性</font>，<font color=FF0000>内联元素以及没有 CSS 样式的元素的 clientWidth 属性值为 0</font>。Element.clientWidth 属性表示元素的内部宽度，以像素计。<font color=FF0000>Element.clientWidth包括内边距 padding</font>，但不包括边框 border、外边距 margin 和垂直滚动条（如果有的话）。

    <mark>该属性值<font color=FF0000>会被四舍五入为一个整数</font></mark>。如果你需要一个小数值，可使用 element.getBoundingClientRect()。

  - **HTMLElement.offsetHeight：**<font color=FF0000>只读属性</font>，它返回该元素的像素高度，<font color=FF0000>高度包含该元素的垂直内边距和<font size=4>**边框**</font></font>，<mark>且是一个整数</mark>。

    通常，元素的 offsetHeight 是一种元素CSS高度的衡量标准，<font color=FF0000>包括元素的**边框**、内边距和**元素的水平滚动条**（如果存在且渲染的话），不包含 `:before` 或 `:after` 等伪类元素的高度</font>。

    如果元素被隐藏（例如 元素或者元素的祖先之一的元素的style.display被设置为none），则返回0

  - **HTMLElement.offsetWidth：**<font color=FF0000>只读属性</font>，返回一个元素的布局宽度。一个典型的（译者注：各浏览器的offsetWidth可能有所不同）offsetWidth是测量<font color=FF0000>包含元素的边框（border）、水平线上的内边距（padding）、竖直方向滚动条（scrollbar）（如果存在的话）、以及CSS设置的宽度(width)的值</font>。

  - **Element.scrollHeight：**

  - **Element.scrollWidth：**

  - **Element.scrollTop：**Element.scrollTop 属性可以<font color=FF0000>**获取或设置**</font>一个元素的内容垂直滚动的像素数。

    一个元素的 <font color=FF0000>scrollTop 值是这个元素的内容顶部（卷起来的）到它的视口可见内容（的顶部）的距离的度量</font>。当一个元素的内容没有产生垂直方向的滚动条，那么它的 scrollTop 值为0。

    **scrollTop <font color=FF0000>可以被设置为任何整数值</font>，同时注意：**

    - 如果一个元素不能被滚动（例如，它没有溢出，或者这个元素有一个"non-scrollable"属性）， scrollTop将被设置为0。
    - 设置scrollTop的值小于0，scrollTop 被设为0
    - 如果设置了超出这个容器可滚动的值, scrollTop 会被设为最大值。

  - **Element.scrollLeft：**可以<font color=FF0000>**读取或设置**</font>元素滚动条到元素左边的距离。

    注意<mark>如果这个元素的内容排列方向（direction） 是rtl (right-to-left) ，那么滚动条会位于最右侧（内容开始处），并且scrollLeft值为0</mark>。<font color=FF0000>此时，当你从右到左拖动滚动条时，scrollLeft会从0变为负数</font>。

    **（和scrollTop一样）<font color=FF0000>scrollLeft 可以是任意整数，然而：</font>**

    - 如果元素不能滚动（比如：元素没有溢出），那么 scrollLeft 的值是 0。
    - 如果给 scrollLeft 设置的值小于0，那么 scrollLeft 的值将变为 0。
    - 如果给 scrollLeft 设置的值大于元素内容最大宽度，那么 scrollLeft 的值将被设为元素最大宽度。

摘自：[js获取各种高度](https://juejin.cn/post/6844904112111239176) / [JS 获取屏幕、浏览器、页面的高度宽度](https://segmentfault.com/a/1190000010443608) / [MDN - Element.clientHeight](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/clientHeight) / [MDN - Element.clientWidth](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/clientWidth) / [MDN - HTMLElement.offsetHeight](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetHeight) / 

<font size=4>**补充：**</font>

<img src="https://i.loli.net/2021/08/01/klzN3KsWC4niVAg.png" alt="image-20210801191948841" style="zoom:50%;" />

摘自：[现代JS教程 - 元素大小和滚动- 几何](https://zh.javascript.info/size-and-scroll#ji-he)

#### Element.getBoundingClientRect()

Element.getBoundingClientRect() 方法<font color=FF0000>返回元素的 大小 及其 相对于视口 ( viewport ) 的位置</font>（**注：**视口即页面窗口）。

如果是 <font color=FF0000>**标准盒子模型**</font>，<font color=FF0000>元素的尺寸等于 `width/height` + `padding` + `border-width` 的总和</font>。如果 <font color=fuchsia>**`box-sizing: border-box`**</font> ，<font color=fuchsia>元素的的尺寸等于 `width/height` </font>。

##### 语法

```js
domRect = element.getBoundingClientRect();
```

返回值是一个 DOMRect 对象，这个对象是由该元素的 getClientRects() 方法返回的一组矩形的集合，就是该元素的 CSS 边框大小。<font color=FF0000>返回的结果是包含完整元素的最小矩形，并且拥有 <font size=4>**left、top、right、bottom、x、y、width 和 height**</font> 这几个以像素为单位的 **只读属性** 用于描述整个边框</font>。**除了 width 和 height 以外的属性是<font color=FF0000>相对于视图窗口的左上角来计算的</font>**。

> **注：**需要注意的是 ⚠️：经过实验发现，这里<font color=FF0000>计算 top 和 bottom 值的 ***上界*** 是 ***整个页面（文档）*** 的最上面，而不是 ***当前可视区域*** 的最上面。所以，y 和 height 的值是可以大于 100vh 的</font>。
>
> 上面说的都是 ***竖向滚动*** 的情况，类似的，如果是 <font color=FF0000>***横向滚动*** 的话，x 和 width 的值也是可以大于 100vw 的</font>（TODO，竖向滚动的情况经过了代码测试，但横向滚动的情况没有经过代码测试；不知道为什么 onscroll 和 addEventListener('scroll', ...) 都不能监听到 横向滚动的事件；应该是我代码写的有问题？）

摘自：[MDN - Element.getBoundingClientRect()](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getBoundingClientRect)

##### Element.getBoundingClientRect() 《现代 JS 教程》中的图示

<img src="https://i.loli.net/2021/08/01/4MFxHdhp67APyz1.png" alt="image-20210801201125660" style="zoom:50%;" />

摘自：[现代JS教程 - 坐标 - 元素坐标：getBoundingClientRect](https://zh.javascript.info/coordinates#yuan-su-zuo-biao-getboundingclientrect)

#### Window.resizeBy()

**概述：**调整窗口大小。

**语法：**

```js
window.resizeBy(xDelta, yDelta)
```

**参数：**

- xDelta：为窗口水平方向变化的像素值。
- yDelta：为窗口垂直方向变化的像素值 。

**示例：**

```js
// 缩小窗口
window.resizeBy(-200, -200);
```

**备注：**该方法相对于窗口当前大小来调整该窗口的大小。要以绝对大小方式调整窗口的大小，可使用 [window.resizeTo](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/resizeTo)。

摘自：[MDN - window.resizeBy](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/resizeBy)



#### JavaScript Window Location

window.location 对象用于<font color=FF0000>获得当前页面的地址 (URL)</font>，<font color=FF0000>并把浏览器重定向到新的页面</font>。<font color=FF0000>在编写时**可不使用 window 这个前缀**</font>。

**相关属性**

- **location.hostname**     返回 web 主机的域名
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

**补充：**
<font color=FF0000>Navigator <font size=4>**接口**</font>表示 **用户代理的状态和标识**</font>。 它允许脚本查询它和注册自己进行一些活动。可以使用只读的 window.navigator 属性检索navigator对象。

链接处还有一些属性和方法，这里略。

另外，也可以通过 在 浏览器 devtools terminal 上输入 navigator（ node环境没有 navigator ），查看属性和方法。

摘自：[MDN - Navigator](https://developer.mozilla.org/zh-CN/docs/Web/API/Navigator)

#### Navigator.sendBeacon()

navigator.sendBeacon() 方法可用于通过 HTTP POST 将少量数据异步传输到 Web 服务器。它主要用于将统计数据发送到 Web 服务器，同时避免了用传统技术（如：XMLHttpRequest）发送分析数据的一些问题。

##### 语法

```js
navigator.sendBeacon(url[, data]);
```

##### 参数

- **url：**url 参数表明 data 将要被发送到的网络地址。
- **data：**可选，data 参数是将要发送的 ArrayBuffer、ArrayBufferView、Blob、DOMString、FormData 或 URLSearchParams 类型的数据。

##### 描述

这个方法 <font color=FF0000>**主要用于满足统计和诊断代码的需要，这些代码通常尝试在卸载 ( unload ) 文档之前向 web 服务器发送数据**</font>。<font color=FF0000>过早的发送数据可能导致错过收集数据的机会</font>。然而，<mark>对于开发者来说保证在文档卸载期间发送数据一直是一个困难</mark>。<mark>因为用户代理通常会忽略在 unload 事件处理器中产生的异步 XMLHttpRequest </mark>。

过去，为了解决这个问题， 统计和诊断代码通常要在： 1) 发起一个同步 XMLHttpRequest 来发送数据。2) 创建一个 \<img> 元素并设置 src，大部分用户代理会延迟卸载（unload）文档以加载图像。3) 创建一个几秒的 no-op 循环。

上述的所有方法都会迫使用户代理延迟卸载文档，并使得下一个导航出现的更晚。下一个页面对于这种较差的载入表现无能为力。

<mark>**这就是 sendBeacon() 方法存在的意义**</mark>。<font color=FF0000>使用 sendBeacon() 方法会使用户代理在有机会时异步地向服务器发送数据，同时，**不会延迟页面的卸载或影响下一导航的载入性能**</font>，这意味着：

- 数据发送是可靠的
- 数据异步传输
- 不影响下一导航的载入

##### 在会话结束时发送统计数据

网站通常 <font color=FF0000>在用户完成页面浏览后向服务器发送分析或诊断数据</font>，**<font color=FF0000>最可靠的方法是在</font> [visibilitychange](https://developer.mozilla.org/en-US/docs/Web/API/Document/visibilitychange_event) <font color=FF0000>事件发生时发送数据</font>**：

```js
document.addEventListener('visibilitychange', function logData() {
  if (document.visibilityState === 'hidden') {
    navigator.sendBeacon('/log', analyticsData);
  }
});
```

##### 避免使用 unload 和 beforeunload

<mark><font size=4>**过去**</font>，许多网站使用 unload 或 beforeunload 事件以在会话结束时发送统计数据</mark>。然而<font color=FF0000>**这是不可靠的**</font>，<font color=FF0000>**在许多情况下（尤其是移动设备）浏览器不会产生 unload、beforeunload 或 pagehide 事件**</font>。下面列出了一种不触发上述事件的情况：

1. 用户加载了网页并与其交互。
2. 完成浏览后，用户切换到了其它应用程序，而不是关闭选项卡。
3. 随后，用户通过手机的应用管理器关闭了浏览器应用。

此外，`unload` 事件与现代浏览器实现的往返缓存（[bfcache](https://web.dev/bfcache/)）不兼容。

##### 使用 pagehide 作为回退

可使用 pagehide 事件来代替部分浏览器未实现的 visibilitychange 事件。和 beforeunload 与 unload 事件类似，这一事件不会被可靠地触发（特别是在移动设备上），但它与 bfcache 兼容。

摘自：[MDN - Navigator.sendBeacon()](https://developer.mozilla.org/zh-CN/docs/Web/API/Navigator/sendBeacon)

#### Clipboard & Navigator.clipboard

Clipboard 接口实现了 Clipboard API，如果用户授予了相应的权限，就能提供系统剪贴板的读写访问。在 Web 应用程序中，Clipboard API 可用于实现剪切、复制和粘贴功能。

如果用户没有适时使用 Permissions API 授予相应权限和"clipboard-read" 或 "clipboard-write" 权限，调用 Clipboard 对象的方法不会成功。**注意 ⚠️：**实际上，现在浏览器对于访问剪贴板权限的索取各有不同，在章节 [剪贴板可用性](https://developer.mozilla.org/zh-CN/docs/Web/API/Clipboard#剪贴板可用性) 查看更多细节。

<font color=FF0000>**系统剪贴板暴露于全局属性 Navigator.clipboard 之中**</font>

<font color=FF0000 size=4>**所有** 剪贴板 API 方法都是 **异步** 的</font>；它们 <font color=FF0000 size=4>**返回一个 Promise 对象**</font>，在剪贴板访问完成后被执行。如果剪贴板访问被拒绝，promise 也会被拒绝。

##### 方法

<font color=FF0000>**Clipboard 继承自 EventTarget 接口**</font>，因此拥有它的方法。

- **read()：**从剪贴板读取数据（比如图片），<font color=FF0000>返回一个 Promise 对象</font>。When the data has been retrieved, the promise is resolved with a DataTransfer object that provides the data。
- **readText()：**从操作系统读取文本；returns a Promise which is resolved with a DOMString containing the clipboard's text once it's available。
- **write()：**写入任意数据至操作系统剪贴板。This asynchronous operation signals that it's finished by resolving the returned Promise。
- **writeText()：**写入文本至操作系统剪贴板。returning a Promise which is resolved once the text is fully copied into the clipboard。

摘自：[MDN - Clipboard](https://developer.mozilla.org/zh-CN/docs/Web/API/Clipboard)

#### Navigator.permissions 🧪

**permissions** 是 **Navigator** 读属性，返回一个可用于查询或更新某些APIs（由 Permissions API 覆盖）的权限状态的对象。

##### 语法

```js
permissionsObj = globalObj.navigator.permissions
```

##### 返回值

一个 Permissions 对象。

##### 示例

```js
navigator.permissions.query({name:'geolocation'}).then(function(result) {
  if (result.state === 'granted') {
    showMap();
  } else if (result.state === 'prompt') {
    showButtonToEnableMap();
  }
  // 如果被拒绝，请不要做任何操作。
});
```

摘自：[MDN - Navigator.permissions](https://developer.mozilla.org/zh-CN/docs/Web/API/Navigator/permissions)



#### JavaScript 计时事件

我们可以<font color=FF0000>在一个设定的时间间隔之后来执行代码</font>。我们称之为计时事件。

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

  **补充：**如下内容摘自 [MDN - window.setInterval](https://developer.mozilla.org/zh-CN/docs/Web/API/WindowOrWorkerGlobalScope/setInterval)
  
  WindowOrWorkerGlobalScope 的 setInterval() 方法重复调用一个函数或执行一个代码段，在每次调用之间具有固定的时间延迟。
  
  在窗口和工作接口上提供的setInterval()方法重复调用函数或执行代码片段，每次调用之间有固定的时间延迟。<font color=FF0000>它**返回一个时间间隔ID**，**该ID唯一地标识时间间隔**，因此您可以稍后通过调用 clearInterval() 来删除它</font>。这个方法是由 WindowOrWorkerGlobalScope mixin 定义的。
  
  - **语法**
  
    ```js
    var intervalID = scope.setInterval(func, delay, [arg1, arg2, ...]);
    var intervalID = scope.setInterval(code, delay);
    ```
  
    **注：值得注意的是，由上面的语法可知：<font color=FF0000>setInterval() 除了接收两个参数外，是可以接收更多参数的</font>。**
  
  - **参数**
  
    - **func：**要重复调用的函数。 每经过指定 延迟 毫秒后执行的函数 。<font color=FF0000>该函数不接受任何参数，也没有返回值</font>。
    - <font color=FF0000>**code：**</font><mark>这个语法是<font color=FF0000>**可选**</font>的，你可以传递一个字符串来代替一个函数对象，你传递的字符串会被编译然后每个delay毫秒时间内执行一次。<font color=FF0000>这个语法因为存在安全风险所以不被推荐使用</font></mark>。
    - **delay：**是每次延迟的毫秒数（一秒等于1000毫秒），函数的每次调用会在该延迟之后发生。<font color=FF0000>和setTimeout一样，实际的延迟时间可能会稍长一点</font>。这个时间计算单位是毫秒（千分之一秒），这个定时器会使指定方法或者代码段执行的时候进行时间延迟。<font color=FF0000>如果这个参数值小于10，则默认使用值为10。请注意，真正延迟时间或许更长</font>
    - <font color=FF0000>**arg1, ..., argN：**</font><font color=FF0000>可选</font>，**当定时器过期的时候，将被传递给func指定函数的附加参数。**
  
  - **返回值：**<font color=FF0000>此返回值 intervalID 是一个非零数值，用来标识通过 setInterval() 创建的计时器，这个值可以用来作为clearInterval()的参数来清除对应的计时器 </font>。
  
  <font color=FF0000>值得注意的是</font>，<font color=FF0000 size=4>**setInterval() 和 setTimeout() 共享同一个 ID池，并且 clearInterval() 和 clearTimeout() 在技术上是可互换使用的**</font>。但是，我们必须去匹配 clearInterval() 和 clearTimeout() 对应的 id，以避免代码杂乱无章，增强代码的可维护性。
  
  **补充：**经过测试，setInterval(function, intervalTime) 第一次执行function是在intervalTime之后。另外，返回的id是一个 对象。
  
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
  myVar = window.setTimeout(function, milliseconds);
  ```

  注：setTimeout 有部分内容还是挺重要 和 易错的，还是要看下；不过，由于setTimeout 的内容和 setInterval 过于相似，所以省略；<font color=FF0000>这里只说重点摘要</font>，详见上面 setInterval 中的内容，或者直接看 [MDN - window.setTimeout](https://developer.mozilla.org/zh-CN/docs/Web/API/setTimeout)
  
  - setTimeout 可以传入超过两个参数，多于两个参数是：可选的附加参数，一旦定时器到期，它们会作为参数传递给function
  - setTimeout()和setInterval()共用一个编号池，技术上，clearTimeout()和 clearInterval() 可以互换。但是，为了避免混淆，不要混用取消定时函数
  
- **clearTimeout()：** 方法用于<font color=FF0000>停止执行setTimeout()方法的函数代码</font>。
  
  **语法**
  
  ```js
  window.clearTimeout(timeoutVariable)
  ```
  
  类似clearInterval()，使用clearTimeout() 方法, 你必须在创建超时方法中（setTimeout）使用全局变量



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

<font size=4>**补充：**</font>

获取并设置与当前文档相关联的 cookie。可以把它当成一个 getter and setter。

- **语法**

  读取所有可从此位置访问的Cookie

  ```js
  allCookies = document.cookie;
  ```

  在上面的代码中，allCookies被赋值为一个字符串，该字符串包含所有的Cookie，<mark>每条cookie以"分号和空格(; )"分隔(即, key=value 键值对)</mark>。

  写一个新 cookie

  ```js
  document.cookie = newCookie;
  ```

  newCookie是一个键值对形式的字符串。需要注意的是，<font color=FF0000>用这个方法一次只能对一个cookie进行设置或更新</font>。

  - **以下可选的cookie属性值可以跟在键值对后，用来具体化对cookie的设定/更新，使用分号以作分隔**：
    - **;path=path** (例如 '/', '/mydir') <font color=FF0000>如果没有定义，默认为当前文档位置的路径</font>。
    - **;domain=domain** (例如 'example.com'， 'subdomain.example.com') <font color=FF0000>如果没有定义，默认为当前文档位置的路径的域名部分</font>。与早期规范相反的是，在域名前面加 . 符将会被忽视，因为浏览器也许会拒绝设置这样的cookie。如果指定了一个域，那么子域也包含在内。
    - **;max-age=max-age-in-seconds** (例如一年为60\*60\*24\*365)
    - **;expires=date-in-GMTString-format** <font color=FF0000>如果没有定义，cookie会在对话结束时过期</font>
    - **;secure**<font color=FF0000>** (cookie只通过https协议传输)**</font>
  - **cookie的值字符串可以用encodeURIComponent()来保证它不包含任何逗号、分号或空格(cookie值中禁止使用这些值)**

- **Cookie的其他方法**

  - **写入cookie**

    **语法：**

    ```js
    docCookies.setItem(name, value[, end[, path[, domain[, secure]]]])
    ```

    **描述：**创建或覆盖一个cookie

    **参数**

    - **name** (<mark>必要</mark>)：要<font color=FF0000>创建或覆盖的cookie的名字 (string)</font>。
    - **value** (<mark>必要</mark>)：<font color=FF0000>cookie的值 (string)</font>。
    - **end** (可选)：最大生命的秒数max-age-in-seconds (一年为31536e3， <font color=FF0000>永不过期的cookie为Infinity</font>) ，或者过期时间的GMTString格式或Date对象; 如果没有定义则会在会话结束时过期 (number – 有限的或 Infinity – string, Date object or null)。
    - **path** (可选)：例如 '/', '/mydir'。 如果没有定义，默认为当前文档位置的路径。(string or null)。路径必须为绝对路径（参见 RFC 2965）。关于如何在这个参数使用相对路径的方法请参见这段。
    - **domain** (可选)：例如 'example.com'， '.example.com' (包括所有子域名), 'subdomain.example.com'。如果没有定义，默认为当前文档位置的路径的域名部分 (string或null)。
    - <font color=FF0000>**secure**</font> (可选)：<font color=FF0000>cookie只会被https传输 (boolean或null)</font>。

  - **获得cookie**

    **语法**

    ```js
    docCookies.getItem(name)
    ```

    **描述：**读取一个cookie。<font color=FF0000>如果cookie不存在返回null</font>。

    **参数**

    - name：读取的cookie名 (string).

  - **移除cookie**

    **语法：**

    ```js
    docCookies.removeItem(name[, path],domain)
    ```

    **描述：**删除一个cookie。

    **参数**

    - **name：**要移除的cookie名(string).
    - **path** (可选)：例如 '/', '/mydir'。 <mark>如果没有定义，默认为当前文档位置的路径</mark>。(string or null)。路径必须为绝对路径（参见 RFC 2965）。关于如何在这个参数使用相对路径的方法请参见这段。
    - domain (可选)：例如 'example.com'， '.example.com' (包括所有子域名), 'subdomain.example.com'。<mark>如果没有定义，默认为当前文档位置的路径的域名部分</mark> (string或null)。

  - **检测cookie**
    **语法**

    ```js
    docCookies.hasItem(name)
    ```

    **描述：**检查一个cookie是否存在

    **参数**

    - **name：**要检查的cookie名 (string).

  - **获得所有cookie的列表**
    **语法**

    ```js
    docCookies.keys()
    ```

    **描述：**<font color=FF0000>返回一个这个路径所有可读的cookie的数组</font>。

摘自：[MDN - Document.cookie](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/cookie)



#### 原型链

js以原型链的形式，保证函数或对象中的方法、属性可以让向下传递，按照面向对象的说法，这就是继承。而<font color=FF0000>js通过原型链才得以实现函数或对象的继承</font>。

![image-20211230145726601](https://s2.loli.net/2021/12/30/ka9luJN7sgcw68f.png)

#### \__proto__

<font color=FF0000>\__proto__ 被认为是过时且不推荐使用的（deprecated）</font>，这里的不推荐使用是指 JavaScript 规范中规定，proto 必须仅在浏览器环境下才能得到支持。

**现代的方法有：**

- **Object.create(proto,  [descriptors]) ：** 利用给定的 proto 作为 [[Prototype]] 和可选的属性描述来创建一个空对象。
- **Object.getPrototypeOf(obj) ：**<font color=FF0000>返回对象 obj 的 [[Prototype]]</font>
- **Object.setPrototypeOf(obj, proto) ：<font color=FF0000>**将对象 obj 的 [[Prototype]] 设置为 proto</font>

应该使用这些方法来代替 \__proto__。

**为什么不建议使用\__proto__ ？**

如果我们尝试在其中存储 用户提供的 键（例如：一个用户输入的字典），我们可以发现一个有趣的小故障：所有的键都正常工作，除了 "\__proto__"。如下示例：

```js
let obj = {};

let key = prompt("What's the key?", "__proto__");
obj[key] = "some value";

alert(obj[key]); // 如果对于 默认值 __proto__ 不做修改，则打印：[object Object]，并不是 "some value"！
```

这里如果用户输入 __proto__，那么赋值会被忽略！因为：<font color=FF0000 size=4>**\__proto__ 属性很特别：它必须是对象或者 null；字符串不能成为 prototype**</font>

在这里，后果并没有很严重。但是在其他情况下，我们可能会对对象进行赋值操作，然后原型可能就被更改了。结果，可能会导致完全意想不到的结果。最可怕的是：通常开发者完全不会考虑到这一点。这让此类 bug 很难被发现，甚至变成漏洞，尤其是在 JavaScript 被用在服务端的时候。另外，上述的字典可以使用 Map来实现，这样就不会出现上述的\__proto__的问题。

<font color=FF0000 size=4>**\__proto__ 不是一个对象的属性，只是 Object.prototype 的访问器属性：**</font>

<img src="https://s2.loli.net/2021/12/31/sI4O5fTJ1zRiodc.png" alt="image-20211231110323159" style="zoom:50%;" />

因此，如果 obj.\__proto__ 被读取或者赋值，那么对应的 getter/setter 会被从它的原型中调用，它会 set/get [[Prototype]]。

就像在本部分教程的开头所说的那样：\__proto__ 是一种访问 [[Prototype]] 的方式，而不是 [[prototype]] 本身。

另外：上面的问题，除了用Map 还可以使用 如下方法：

```js
let obj = Object.create(null);

let key = prompt("What's the key?", "__proto__");
obj[key] = "some value";

alert(obj[key]); // "some value"
```

Object.create(null) 创建了一个空对象，这个对象没有原型（[[Prototype]] 是 null）。因此，它没有继承 \__proto__ 的 getter/setter 方法。现在，它被作为正常的数据属性进行处理，因此上面的这个示例能够正常工作

**总结：使用 \__proto__ 不安全。**

摘自：[原型方法，没有 _\_proto__ 的对象](https://zh.javascript.info/prototype-methods)

**补充：**

如下示例：

```js
function Foo() {}
const foo = new Foo()

Object.getPrototypeOf(foo) === foo.__proto__ === Foo.prototype
//                         true              true
```

尤其是：<font color=FF0000 size=4>**foo.\__proto__ === Foo.prototype**</font>

参考自：[js继承实现之Object.create](https://segmentfault.com/a/1190000014592412)



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

变量提升是 Javascript中执行上下文 （特别是创建和执行阶段）工作方式的一种认识。在 ECMAScript® 2015 Language Specification 之前的JavaScript文档中找不到变量提升（Hoisting）这个词。不过，需要注意的是，开始时，这个概念可能比较难理解，甚至恼人。

例如，从概念的字面意义上说，“变量提升”意味着变量和函数的声明会在物理层面移动到代码的最前面，但这么说并不准确。实际上变量和函数声明在代码里的位置是不会动的，而是在编译阶段被放入内存中。

<font color=FF0000>JavaScript 在执行任何代码段之前，将函数声明放入内存中的优点之一是，**你可以在声明一个函数之前使用该函数**。</font>

摘自：[MDN web docs - Hoisting（变量提升）](https://developer.mozilla.org/zh-CN/docs/Glossary/Hoisting)



#### falsy

**falsy 值 (虚值) 是<font color=FF0000>在 Boolean 上下文中认定为 false 的值</font>。**

JavaScript 在需要用到布尔类型值的上下文中使用强制类型转换(Type Conversion )将值转换为布尔值，例如条件语句和循环语句。

**在 JavaScript 中只有 8 个 falsy 值。**

| false      | false 关键字                                                                                                                                                                  |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0          | 数值 zero                                                                                                                                                                    |
| -0         | 数值 负 zero                                                                                                                                                                  |
| 0n         | 当 BigInt作为布尔值使用时, 遵从其作为数值的规则. 0n 是 falsy 值                                                                                                                            |
| "", '', `` |这是一个空字符串 (字符串的长度为零)。 JavaScript 中的字符串可用双引号 "", 单引号 '', 或 模板字面量``定义。|
| null       | null - 缺少值                                                                                                                                                                 |
| undefined  | undefined - 原始值                                                                                                                                                            |
| NaN        | NaN - 非数值                                                                                                                                                                  |

摘自：[MDN官方文档 - Falsy](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy)



#### window.open()

```javascript
let windowObjectReference = window.open(strUrl, strWindowName, [strWindowFeatures]);
```

- **WindowObjectReference：**打开的新窗口对象的引用。如果调用失败，返回值会是 null 。如果父子窗口满足“同源策略”，你可以通过这个引用访问新窗口的属性或方法。
- **strUrl：**<font color=FF0000>新窗口需要载入的url地址</font>。strUrl可以是 web 上的html页面，<font color=FF0000>也可以是图片文件或者其他任何浏览器支持的文件格式</font>。注意：<mark>如果strUrl 是一个空值，那么打开的窗口将会是带有默认工具栏的空白窗口（加载 about:blank ）</mark>。
- **strWindowName：**新窗口的名称。该字符串可以用来作为超链接 \<a> 或表单 \<form> 元素的目标属性值。字符串中不能含有空白字符。注意：strWindowName 并不是新窗口的标题。
- **strWindowFeatures：**<font color=FF0000>**可选参数**</font>。是一个字符串值，<font color=FF0000>这个值列出了将要打开的窗口的一些特性（窗口功能和工具栏）</font> 。 字符串中不能包含任何空白字符，特性之间用逗号分隔开。

**补充：**

- open() 方法，是创建一个新的浏览器窗口对象。
- 如果已经存在以 strWindowName 为名称的窗口，则不再打开一个新窗口，而是把 strUrl 加载到这个窗口中。在这种情况下，方法的返回值是这个已经打开的窗口，并忽略参数 strWindowFeatures 。
- 位置尺寸特征常用属性：<mark>left / top / height / width / screenX / screenY / centerscreen / outerHeight / outerWidth / innerHeight / innerWidth</mark>（还有其他省略）

摘自：[MDN - Window.open()](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/open)

#### window.opener

返回打开当前窗口的那个窗口的引用，例如：在window A中打开了window B，B.opener 返回 A.

- **语法**

  ```js
  var objRef = window.opener;
  ```

- **备注：**<mark>如果当前窗口是由另一个窗口打开的，window.opener保留了那个窗口的引用</mark>。<font color=FF0000>如果当前窗口不是由其他窗口打开的, 则该属性返回 null.</font>

摘自：[MDN - window.opener](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/opener)



#### window.postMessage

<font color=FF0000>**window.postMessage() 方法可以安全地实现跨源通信**</font>。通常，<mark>对于两个不同页面的脚本，只有当执行它们的页面位于具有相同的协议（通常为 https ），端口号（ 443 为 https 的默认值 ），以及主机（两个页面的模数 Document.domain 设置为相同的值）时，这两个脚本才能相互通信</mark><font color=FF0000>**（同源限制）**</font>。window.postMessage() 方法提供了一种受控机制来规避此限制，只要正确使用，这种方法就很安全

从广义上讲，<font color=FF0000>一个窗口可以获得对另一个窗口的引用</font>（比如 <font color=FF0000>**targetWindow = window.opener**</font> ），<font color=FF0000>然后在窗口上调用 targetWindow.postMessage() 方法分发一个  MessageEvent 消息</font>。接收消息的窗口可以根据需要自由处理此事件。<font color=FF0000>传递给 window.postMessage() 的参数（比如 message ）将通过消息事件对象暴露给接收消息的窗口</font>。

##### 语法

```js
otherWindow.postMessage(message, targetOrigin, [transfer]);
```

- **otherWindow：**<font color=FF0000>其他窗口的一个引用</font>，比如 iframe 的 contentWindow 属性、执行 window.open 返回的窗口对象、或者是命名过或数值索引的 window.frames 。

- **message：**<font color=FF0000>将要发送到其他 window的数据</font>。<font color=FF0000>它将会被 结构化克隆算法 ( structedClone ) 序列化</font>。这意味着你可以不受什么限制的将数据对象安全的传送给目标窗口而无需自己序列化

- **targetOrigin：**<font color=FF0000>**通过窗口的origin属性来指定哪些窗口能接收到消息事件**</font>，<font color=FF0000>其值可以是字符串 "\*"（表示无限制）或者一个 URI</font>。在发送消息的时候，如果目标窗口的协议、主机地址或端口这三者的 任意一项不匹配 targetOrigin 提供的值，那么消息就不会被发送；只有三者完全匹配，消息才会被发送（同源限制）。

  这个机制用来控制消息可以发送到哪些窗口。例如，当用 postMessage 传送密码时，这个参数就显得尤为重要，必须保证它的值与这条包含密码的信息的预期接受者的 origin 属性完全一致，来防止密码被恶意的第三方截获。如果你明确的知道消息应该发送到哪个窗口，那么请始终提供一个有确切值的 targetOrigin，而不是 \*。不提供确切的目标将导致数据泄露到任何对数据感兴趣的恶意站点

- **transfer：** **可选**，是一串和 message 同时传递的 Transferable 对象. 这些对象的所有权将被转移给消息的接收方，而发送一方将不再保有所有权。

**执行如下代码, 其他window可以监听分发的message:**

```js
window.addEventListener("message", (event) => {
  if (event.origin !== "http://example.org:8080")
    return;
  // ...
}, false);
```

 **message 的属性有:**

- **data：**从其他 window 中传递过来的对象。
- **origin：**<font color=FF0000>调用 postMessage  时消息发送方窗口的 origin . **这个字符串由 协议、“://“、域名、“ : 端口号”拼接而成**</font>。例如 “https://example.org (隐含端口 443)”、“http://example.net (隐含端口 80)”、“http://example.com:8080”。请注意，这个origin不能保证是该窗口的当前或未来origin，因为postMessage被调用后可能被导航到不同的位置。
- **source：**<font color=FF0000 size=4>**对发送消息的窗口对象的引用**</font>；<mark style=background-color:hotpink>您可以使用此来在具有不同origin的两个窗口之间建立双向通信</mark>。

**安全问题**

<font color=FF0000>如果您不希望从其他网站接收 message，请不要为 message 事件添加任何事件侦听器。这是一个完全万无一失的方式来避免安全问题</font>。

<font color=FF0000>如果您确实希望从其他网站接收 message，请始终使用 origin 和 source 属性验证发件人的身份</font>。 任何窗口（包括例如http://evil.example.com）都可以向任何其他窗口发送消息，并且您不能保证未知发件人不会发送恶意消息。 但是，验证身份后，您仍然应该始终验证接收到的消息的语法。 否则，您信任只发送受信任邮件的网站中的安全漏洞可能会在您的网站中打开跨网站脚本漏洞。

<font color=FF0000>当您使用postMessage将数据发送到其他窗口时，始终指定精确的目标 origin，而不是\*</font>。 恶意网站可以在您不知情的情况下更改窗口的位置，因此它可以拦截使用postMessage发送的数据。

摘自：[MDN - window.postMessage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)



#### window.close()

Window.close() 方法用于：关闭当前窗口或某个指定的窗口。

<font color=FF0000>该方法只能由 Window.open() 方法打开的窗口的 window 对象来调用</font>。如果一个窗口不是由脚本打开的，那么，在调用该方法时，JavaScript 控制台会出现类似下面的错误：不能使用脚本关闭一个不是由脚本打开的窗口。或 Scripts may not close windows that were not opened by script. 。

<mark>同时也要注意，对于由 HTMLIFrameElement.contentWindow 返回的 Window 对象，close() 也没有效果</mark>。

摘自：[MDN web docs - window.close()](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/close)



#### window.frames

<font color=FF0000>返回当前窗口，一个类数组对象，列出了当前窗口的所有直接子窗口</font>

##### 语法

```js
frameList = window.frames;
```

- frameList 是一个 frame 对象的集合，它<font color=FF0000>类似一个数组，有 length 属性且可以使用索引 ( [i] ) 来访问</font>
- <font color=FF0000>**`frameList === window` 计算结果为 true**</font>
- 在 <font color=FF0000>window.frames 类数组中的 **每一项都代表了窗口对应给定对象的 \<frame> 或 \<iframe>的内容**</font>，而不是 (i)frame DOM元素（即window.frames[0] 与 document.getElementsByTagName("iframe")[0].contentWindow 是相同的）。

摘自：[MDN - Window.frames](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/frames)



#### window.location.reload()

- **语法：**
  
  ```javascript
  object.reload(forcedReload);
  ```

- **参数：**
  forcedReload 可选，该参数要求为 布尔 类型，当取值为 true 时，将强制浏览器从服务器重新获取当前页面资源，而不是从浏览器的缓存中读取，如果取值为 false 或不传该参数时，浏览器则可能会从缓存中读取当前页面。

摘自：[MDN web docs - Location.reload()](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/reload)



#### alert()、confirm()、prompt()

- **alert()**：alert 方法有一个参数，即希望对用户显示的文本字符串。该字符串不是 HTML 格式。该消息框提供了一个“确定”按钮让用户关闭该消息框，并且该消息框是模式对话框，也就是说，用户必须先关闭该消息框然后才能继续进行操作。
  
  <img src="https://s1.ax1x.com/2020/07/29/aZWwng.png" style="zoom:50%;" />

- **confirm()**：使用确认消息框可向用户问一个“是-或-否”问题，并且用户可以选择单击“确定”按钮或者单击“取消”按钮。<font color=FF0000>confirm 方法的返回值为 true 或 false</font>。该消息框也是模式对话框：用户必须在响应该对话框（单击一个按钮）将其关闭后，才能进行下一步操作。
  
  <img src="https://s1.ax1x.com/2020/07/29/aZhEz6.png" style="zoom:50%;" />

- **prompt()**：提示消息框提供了一个文本字段，用户可以在此字段输入一个答案来响应您的提示。该消息框有一个“确定”按钮和一个“取消”按钮。如果您提供了一个辅助字符串参数，则提示消息框将在文本字段显示该辅助字符串作为默认响应。否则，默认文本为 ""。

  <img src="https://s1.ax1x.com/2020/07/29/aZhJQf.png" style="zoom:50%;" />

  **补充：**

  语法：

  ```js
  result = window.prompt(text, value);
  ```

  - **result：** 用来<font color=FF0000>存储用户输入文字的字符串</font>，或者是 null。
  - **text：** 用来<font color=FF0000>提示用户输入文字的字符串</font>，如果没有任何提示内容，该参数<font color=FF0000>可以省略不写</font>。
  - **value：** 文本输入框中的<font color=FF0000>默认值</font>，该参数也<font color=FF0000>可以省略不写</font>。不过在 Internet Explorer 7 和 8 中，省略该参数会导致输入框中显示默认值"undefined"。

  摘自：[MDN - window.prompt](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/prompt)

摘自：[**JS alert()、confirm()、prompt()的区别**](https://www.imooc.com/article/6751)



#### Console

Console 对象可以接入浏览器控制台（如：Firefox 的 Web Console），在不同浏览器上它的实现细节可能是不一样的.

Console 对象可以从任何全局对象中访问到，如 Window，WorkerGlobalScope 以及控制台属性中的特殊变量。它被定义为 Window.console，而且可直接通过 console 调用。

**方法**

- **Console.assert()：**判断第一个参数是否为真，false 的话抛出异常并且在控制台输出相应信息。
- **Console.clear()：**清空控制台，并输出 Console was cleared。
- **Console.count()：**以参数为标识记录调用的次数，调用时在控制台打印标识以及调用次数。
- **Console.countReset()：**重置指定标签的计数器值。
- **Console.debug()：**在控制台打印一条 "debug" 级别的消息。
- **Console.dir()：**显示一个由特定的 Javascript 对象列表组成的可交互列表。这个列表可以使用三角形隐藏和显示来审查子对象的内容。
- **Console.dirxml()：**打印 XML/HTML 元素表示的指定对象，否则显示 JavaScript 对象视图。
- **Console.error()：**<font color=FF0000>**常用**</font>，打印一条错误信息，使用方法可以参考 string substitution。
- **Console.exception()：** error() 方法的别称。
- **Console.group()：**创建一个新的内联 group, 后续所有打印内容将会以子层级的形式展示。调用 groupEnd()来闭合组。
- **Console.groupCollapsed()：**创建一个新的内联 group。使用方法和 group() 相同，不同的是，groupCollapsed() 方法打印出来的内容默认是折叠的。调用groupEnd()来闭合组。
- **Console.groupEnd()：**闭合当前内联 group。
- **Console.info()：**<font color=FF0000>**常用**</font>，打印资讯类说明信息，使用方法可以参考 string substitution。
- **Console.log()：**<font color=FF0000>**常用**</font>，打印内容的通用方法，使用方法可以参考 string substitution。
- **Console.profile() ：**er's built-in profiler (for example, the Firefox performance tool). You can specify an optional name for the profile.
- **Console.profileEnd() ：**Stops the profiler. You can see the resulting profile in the browser's performance tool (for example, the Firefox performance tool).
- **Console.table()：**将列表型的数据打印成表格。
- **Console.time()：**启动一个以入参作为特定名称的计时器，在显示页面中可同时运行的计时器上限为10,000.
- **Console.timeEnd()：**结束特定的 计时器 并以豪秒打印其从开始到结束所用的时间。
- **Console.timeLog()：**打印特定 计时器 所运行的时间。
- **Console.timeStamp() ：**添加一个标记到浏览器的 Timeline 或 Waterfall 工具。
- **Console.trace()：**输出一个 stack trace。
- **Console.warn()：**常用，打印一个警告信息，可以使用 string substitution 和额外的参数。

##### **使用字符串替换** （注：这里感觉用“格式化输出“表述，似乎更好些）

可以在传递给 console 的方法的时候使用下面的字符以期进行参数的替换。

| Substitution string | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| %o or %O            | 打印 JavaScript 对象。在审阅器点击对象名字可展开更多对象的信息。 |
| %d or %i            | 打印整数。支持数字格式化。例如, console.log("Foo %.2d", 1.1) 会输出有先导 0 的两位有效数字: Foo 01。 |
| %s                  | 打印字符串。                                                 |
| %f                  | 打印浮点数。支持格式化，比如 console.log("Foo %.2f", 1.1) 会输出两位小数: Foo 1.10 |

##### 为控制台定义样式

可以使用 <font color=FF0000 size=4>**%c**</font> 为打印内容定义样式：

```js
console.log("This is %cMy stylish message", "color: yellow; font-style: italic; background-color: blue;padding: 2px"); // 另外，注意 %c 的位置
```

指令前的文本不会受到影响，但指令后的文本将会使用参数中声明的 CSS 样式。

![img](https://mdn.mozillademos.org/files/12527/CSS-styling.png)

%c 语法可用的属性如下 (至少在 Firefox 中是这样，别的浏览器会有诸多不同)：

- background 与其全写版本。
- border 与其全写版本。
- border-radius
- box-decoration-break
- box-shadow
- clear 和 float
- color
- cursor
- display
- font 与其全写版本。
- line-height
- margin
- outline 与其全写版本。
- padding
- text-transform 这类 text-* 属性 
- white-space
- word-spacing 和 word-break
- writing-mode

注意：控制台信息的默认行为与行内元素相似。为了应用 padding, margin 这类效果，你应当这样设置display: inline-block.。

摘自：[MDN web docs - Console](https://developer.mozilla.org/zh-CN/docs/Web/API/Console)





#### JS存储对象（Storage）

作为 Web Storage API 的接口，**`Storage`** <mark>提供了访问特定域名下的<font color=FF0000>**会话存储**</font>（sessionStorage）或<font color=FF0000>**本地存储**</font>（localStorage）的功能</mark>，例如，可以添加、修改或删除存储的数据项。

如果你想要操作一个域名的会话存储，可以使用 Window.sessionStorage；如果想要操作一个域名的本地存储，可以使用 Window.localStorage。

- 属性
  - **Storage.length：** 只读，返回一个整数，表示存储在 Storage 对象中的数据项数量。
- **方法**
  - **Storage.key()：**该方法接受一个数值 n 作为参数，并返回存储中的第 n 个键名。
  - **Storage.getItem()：**该方法接受一个键名作为参数，返回键名对应的值。
  - **Storage.setItem()：**该方法接受一个键名和值作为参数，将会把键值对添加到存储中，如果键名存在，则更新其对应的值。
  - **Storage.removeItem()：**该方法接受一个键名作为参数，并把该键名从存储中删除。
  - **Storage.clear()：**<font color=FF0000>调用该方法会清空存储中的所有键名</font>。

摘自：[MDN web docs - Storage](https://developer.mozilla.org/zh-CN/docs/Web/API/Storage)



#### DOMString

DOMString 是一个UTF-16字符串。由于JavaScript已经使用了这样的字符串（即：已经使用了String），所以DOMString 直接映射到 一个String。

将 null传递给接受DOMString的方法或参数时通常会把其字符串化为“null”。

摘自：[MDN - DOMString](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)



#### Document.createElement()

在 HTML 文档中，Document.createElement() 方法用于<font color=FF0000>创建一个由标签名称 tagName 指定的 HTML 元素</font>。<mark>如果用户代理无法识别 tagName，则会生成一个未知 HTML 元素 HTMLUnknownElement</mark>。

**语法**

```js
var element = document.createElement(tagName[, options]);
```

**参数**

- **tagName：**指定要创建元素类型的字符串,创建元素时的 nodeName 使用 tagName 的值为初始化，该方法不允许使用限定名称(如:"html:a")，<font color=FF0000>在 HTML 文档上调用 createElement() 方法创建元素之前会将tagName 转化成小写</font>，在 Firefox、Opera 和 Chrome 内核中，createElement(null) 等同于 createElement("null")
- **options：**可选，一个可选的参数 ElementCreationOptions 是包含一个属性名为 is 的对象，该对象的值是用 customElements.define() 方法定义过的一个自定义元素的标签名。为了向前兼容较老版本的 Custom Elements specification, 有一些浏览器会允许你传一个值为自定义元素的标签名的字符串作为该参数的值。可以参考本页下方的 Web component example Google 的 Extending native HTML elements 文档仔细了解如何使用该参数。

**Web component 示例**
以下示例片段取自我们的 expanding-list-web-component 示例(实时查看)。 在这个案例中， 我们的自定义元素继承了以 \<ul> 元素为代表的 HTMLUListElement (en-US).

```js
// 为新元素创建一个类
class ExpandingList extends HTMLUListElement {
  constructor() {
    // Always call super first in constructor
    super();
		// constructor definition left out for brevity
		...
  }
}
// 定义新元素
customElements.define('expanding-list', ExpandingList, { extends: "ul" });
```


如果我们想以函数的方式创建此元素的实例，则可以使用以下方式调用：

```js
let expandingList = document.createElement('ul', { is : 'expanding-list' })
```

新元素将被赋予is属性，其值为自定义元素的标签名称。

摘自：[MDN - Document.createElement()](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createElement)



#### Document.createTextNode()

创建一个新的文本节点。这个方法可以用来转义 HTML 字符。

语法

```js
var text = document.createTextNode(data);
```

- text 是一个文本节点。
- data 是一个字符串，包含了要放入文本节点的内容。

摘自：[MDN - Document.createTextNode()](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createTextNode)



#### Element.getAttribute()

getAttribute() 返回元素上一个指定的属性值。如果指定的属性不存在，则返回  null 或 "" （空字符串）。

##### 语法

```js
let attribute = element.getAttribute(attributeName);
```
- attribute 是一个包含 attributeName 属性值的字符串。
- attributeName 是你想要获取的属性值的属性名称。

摘自：[MDN - Element.getAttribute()](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getAttribute)

#### Element.setAttribute()

设置指定元素上的某个属性值。<font color=FF0000>如果属性已经存在，则更新该值；否则，使用指定的名称和值添加一个新的属性</font>。

要获取某个属性当前的值，可使用 getAttribute()；要删除某个属性，可使用 removeAttribute()。

##### 语法

```js
element.setAttribute(name, value);
```

**参数**

- name：表示属性名称的字符串。A DOMString specifying the name of the attribute whose value is to be set. The attribute name is automatically converted to all lower-case when setAttribute() is called on an HTML element in an HTML document.
- value：属性的值/新值。A DOMString 包含了分配给这个属性的值. 任何非字符串的值都会被自动转换为字符串

摘自：[MDN - Element.setAttribute()](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/setAttribute)

#### Element.removeAttribute()

元素方法 removeAttribute() 从指定的元素中删除一个属性。

##### 语法

```js
element.removeAttribute(attrName);
```

**参数**

- **属性名：**DOMString指定要从元素中移除的属性的名称。如果指定的属性不存在，则removeAttribute()返回，但不会生成错误。
- **返回值：**IE 返回boolean类型值，其他返回undefined

摘自：[MDN - Element.removeAttribute()](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/removeAttribute)



#### Node.parentElement

返回当前节点的父元素节点，如果该元素没有父节点，或者父节点不是一个 DOM 元素，则返回 null。

语法

```js
parentElement = node.parentElement
```

parentElement 是当前节点的父元素。它永远是一个 DOM 元素 对象，或者 null。

摘自：[MDN - Node.parentElement](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/parentElement)



#### Node.nodeType

只读属性 **Node.nodeType** 表示的是该节点的类型。

**描述**

nodeType 属性可用来区分不同类型的节点，比如 元素, 文本 和 注释。

**语法**

```js
var type = node.nodeType;
```

返回一个整数，其代表的是节点类型。

**节点类型常量**

| 常量                             | 值   | 描述                                                         |
| :------------------------------- | :--- | :----------------------------------------------------------- |
| Node.ELEMENT_NODE                | 1    | 一个 元素 节点，例如 \<p> 和 \<div>。                        |
| Node.TEXT_NODE                   | 3    | Element 或者 Attr 中实际的 文字                              |
| Node.CDATA_SECTION_NODE          | 4    | 一个 CDATASection，例如 <!CDATA[[ … ]]>。                    |
| Node.PROCESSING_INSTRUCTION_NODE | 7    | 一个用于XML文档的 ProcessingInstruction (en-US) ，例如 \<?xml-stylesheet ... ?> 声明。 |
| Node.COMMENT_NODE                | 8    | 一个 Comment 节点。                                          |
| Node.DOCUMENT_NODE               | 9    | 一个 Document 节点。                                         |
| Node.DOCUMENT_TYPE_NODE          | 10   | 描述文档类型的 DocumentType 节点。例如 \<!DOCTYPE html> 就是用于 HTML5 的。 |
| Node.DOCUMENT_FRAGMENT_NODE      | `11` | 一个 DocumentFragment 节点                                   |

另外，还有一些“已弃用的节点类型常量”，不要使用；这里省略（详见引用文档）

摘自：[MDN - Node.nodeType](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/nodeType)



#### Node.cloneNode

Node.cloneNode() 方法返回调用该方法的节点的一个副本。

**语法**

```js
var dupNode = node.cloneNode(deep);
```

- **node：**将要被克隆的节点

- **dupNode：**克隆生成的副本节点

- **deep：** 可选，Boolean，<font color=FF0000>**是否采用深度克隆**</font>，<mark>如果为true，则该节点的所有后代节点也都会被克隆，如果为false，则只克隆该节点本身</mark>。
- 如果<font color=FF0000>deep参数设为false</font>, 则<font color=FF0000>不克隆它的任何子节点</font>。<font color=FF0000>该节点所包含的所有文本也不会被克隆</font>，因为文本本身也是一个或多个的Text节点。
  
- 如果<font color=FF0000>deep参数设为true</font>, 则<font color=FF0000>会复制整棵DOM子树</font>(包括那些可能存在的Text子节点).对于空结点(例如\<img>和\<input>元素)，则deep参数无论设为true还是设为false,都没有关系,但是仍然需要为它指定一个值.

摘自：[MDN - Node.cloneNode](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/cloneNode)



#### Node.contains

Node.contains()返回的是一个布尔值，来表示传入的节点是否为该节点的后代节点。

**语法**

```js
node.contains( otherNode )
```

- node 是否包含otherNode节点.

- otherNode 是否是node的后代节点.

如果 otherNode 是 node 的后代节点或是 node 节点本身.则返回true , 否则返回 false.

摘自：[MDN - Node.contains](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/contains)



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



#### EventTarget / Node / Element 

**EventTarget**

EventTarget 是一个 DOM 接口，由可以接收事件、并且可以创建侦听器的对象实现。

Element，document 和 window 是最常见的 event targets ，但是其他对象也可以作为 event targets，比如 XMLHttpRequest，AudioNode，AudioContext  等等。

**构造函数**
**EventTarget()：**创建一个新的 EventTarget 对象实例。
**方法**

- **EventTarget.addEventListener()：**在EventTarget上注册特定事件类型的事件处理程序。
- **EventTarget.removeEventListener()：**EventTarget中删除事件侦听器。
- **EventTarget.dispatchEvent()：**将事件分派到此EventTarget。

摘自：[MDN - EventTarget](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget)

**Node**

Node 是一个接口，各种类型的 DOM API 对象会从这个接口继承。它允许我们使用相似的方式对待这些不同类型的对象；比如, 继承同一组方法，或者用同样的方式测试。

以下接口都从 Node 继承其方法和属性：

Document, Element, Attr, CharacterData (which Text, Comment, and CDATASection inherit), ProcessingInstruction (en-US), DocumentFragment, DocumentType, Notation, Entity, EntityReference
在方法和属性不相关的特定情况下，这些接口可能返回 null。它们可能会抛出异常 - 例如，当将子节点添加到不允许子节点存在的节点时。

Node从其父类型 EventTarget继承属性。

摘自：[MDN - Node](https://developer.mozilla.org/zh-CN/docs/Web/API/Node)

**Element**

<font color=FF0000>**Element 是一个通用性非常强的基类，所有 Document 对象下的对象都继承自它**</font>。<font color=FF0000>这个接口描述了所有相同种类的元素所普遍具有的方法和属性。一些接口继承自 Element 并且增加了一些额外功能的接口描述了具体的行为</font>。例如， HTMLElement 接口是所有 HTML 元素的基本接口，而 SVGElement 接口是所有 SVG 元素的基础。大多数功能是在这个类的更深层级（hierarchy）的接口中被进一步制定的。

<font color=FF0000>所有属性继承自它的祖先接口 Node，并且扩展了 Node 的父接口 EventTarget</font>，并且从以下部分继承了属性：ParentNode、ChildNode、NonDocumentTypeChildNode，和 Animatable。

摘自：[MDN - Element](https://developer.mozilla.org/zh-CN/docs/Web/API/Element)

**HTMLElement**

HTMLElement 接口表示所有的 HTML 元素。一些HTML元素直接实现了HTMLElement接口，其它的间接实现HTMLElement接口。

**属性：**略，见摘抄网页

**方法：**从父元素继承的方法, Element。略，见摘抄网页

摘自：[MDN - HTMLElement](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement)

<font size=4>**补充：**</font>

<img src="https://i.loli.net/2021/07/27/WKpYb5TPe4xjCfR.png" alt="image-20210727084849123" style="zoom: 33%;" />

我们考虑一下 \<input> 元素的 DOM 对象。它属于 HTMLInputElement 类。

它获取属性和方法，并将其作为下列类（按继承顺序列出）的叠加：

- **HTMLInputElement** — 该类提供特定于输入的属性，
- **HTMLElement** — 它提供了通用（common）的 HTML 元素方法（以及 getter 和 setter）
- **Element** — 提供通用（generic）元素方法，
- **Node** — 提供通用 DOM 节点属性，
- **EventTarget** — 为事件（包括事件本身）提供支持，
- ……最后，它继承自 Object，因为像 hasOwnProperty 这样的“普通对象”方法也是可用的。

摘自：[现代JavaScript教程 - 节点属性：type，tag 和 content](https://zh.javascript.info/basic-dom-node-properties)



#### Observer 综述

##### 什么是浏览器的观察器

<font color=FF0000>**针对一些不是由用户直接触发的事件**</font>：比如，DOM 元素从不可见到可见、DOM大小、属性的改变和子节点个数的修改等；<font color=FF0000>浏览器提供特定的 API 去监控这些变化，这些 API 就是浏览器的观察器</font>。

##### 浏览器的观察器有哪些

<font color=FF0000>**浏览器的观察器共有 5 种**</font> ：IntersectionObserver（交叉观察器）、MutationObserver（变化观察器）、ResizeObserver（大小观察器）、PerformanceObserver（性能观察器）、ReportingObserver（报告观察器） 

摘自：[一起认识下浏览器的5种观察器](https://mp.weixin.qq.com/s/0Kj0cUYrQtx7Bh2VuZ-pGQ)

#### MutationObserver

<font color=FF0000>**MutationObserver 接口提供了监视对 *DOM 树* 所做更改的能力**</font>。它被设计为旧的Mutation Events功能的替代品，**该功能是DOM3 Events规范的一部分**。

##### 构造函数

- **MutationObserver()：**<font color=FF0000>创建并返回</font>一个新的 MutationObserver 它会在指定的DOM发生变化时被调用。

##### 方法

- **disconnect()：**<font color=FF0000>阻止 MutationObserver 实例继续接收的通知，直到再次调用其observe()方法</font>，该观察者对象包含的回调函数都不会再被调用。
- **observe()：**<font color=FF0000>配置MutationObserver在DOM更改匹配给定选项时</font>，通过其回调函数开始接收通知。
- **takeRecords()：**<font color=FF0000>从MutationObserver的通知队列中删除所有待处理的通知</font>，并将它们返回到MutationRecord对象的新Array中。

##### 使用示例

```js
// 选择需要观察变动的节点
const targetNode = document.getElementById('some-id');

// 观察器的配置（需要观察什么变动）
const config = { attributes: true, childList: true, subtree: true };

// 当观察到变动时执行的回调函数
const callback = function(mutationsList, observer) {
    // Use traditional 'for loops' for IE 11
    for(let mutation of mutationsList) {
        if (mutation.type === 'childList') {
            console.log('A child node has been added or removed.');
        }
        else if (mutation.type === 'attributes') {
            console.log('The ' + mutation.attributeName + ' attribute was modified.');
        }
    }
};

// 创建一个观察器实例并传入回调函数
const observer = new MutationObserver(callback);

// 以上述配置开始观察目标节点
observer.observe(targetNode, config);

// 之后，可停止观察
observer.disconnect();
```

摘自：[MDN - MutationObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)

#### MutationObserver.MutationObserver()

<font color=FF0000>DOM 规范中的 MutationObserver() 构造函数</font>——是 MutationObserver 接口内容的一部分——<font color=FF0000>**创建并返回一个新的观察器**</font>，它会在触发指定 DOM 事件时，调用指定的回调函数。<mark>MutationObserver 对 DOM 的观察不会立即启动；而必须先调用 observe() 方法来确定，要监听哪一部分的 DOM 以及要响应哪些更改</mark>。

- **语法**

  ```js
  var observer = new MutationObserver(callback);
  ```

- **参数**

  - **callback：**一个回调函数，<font color=FF0000>每当被指定的节点或子树以及配置项有Dom变动时会被调用</font>。<font color=FF0000>**回调函数拥有两个参数**</font>：一个是<font color=FF0000>**描述所有被触发改动的 MutationRecord 对象数组**</font>，另一个是<font color=FF0000>**调用该函数的MutationObserver 对象**</font>。

- **返回值：**一个新的、包含监听 DOM 变化回调函数的 MutationObserver 对象。

MutationObserver()的具体示例：可以看上面的MutationObserver中的使用示例

摘自：[MDN - MutationObserver.MutationObserver()](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver/MutationObserver)

#### MutationObserver.observe()

MutationObserver的 observe() 方法<font color=FF0000>**配置**</font>了 MutationObserver 对象的回调方法以开始接收与给定选项匹配的DOM变化的通知。根据配置，观察者会观察 DOM 树中的单个 Node，也可能会观察被指定节点的部分或者所有的子孙节点。

- **语法**

  ```js
  mutationObserver.observe(target[, options])
  ```

- **参数**

  - **target：**<font color=FF0000>DOM树中的一个要观察变化的DOM Node</font> (可能是一个Element) , 或者是被观察的子节点树的根节点。
  - **options：**<font color=FF0000>**可选**</font>，<font color=FF0000>一个可选的**MutationObserverInit** 对象</font>（<font size=4>**详见下面**</font>），此对象的配置项描述了DOM的哪些变化应该提供给当前观察者的callback。

- **返回值：**undefined。

- **异常**

  - **TypeError：**以下任一情况都会抛出异常:
    - **配置选项使得实际上不会监视任何内容**（例如，如果 MutationObserverInit.childList，MutationObserverInit.attributes 和 MutationObserverInit.characterData 都为 false）。
    - attributes 选项为 false（表示不监视属性更改）但是attributeOldValue 为 true 并且/或者 attributeFilter 配置存在。
    - characterDataOldValue 选项为 true 但是 MutationObserverInit.characterData 为 false（表示不跟踪字符更改）。

摘自：[MDN - MutationObserver.observe()](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver/observe)

#### MutationObserverInit

MutationObserverInit 字典描述了 MutationObserver 的配置。因此，它主要被用作 MutationObserver.observe() 方法的参数类型。

**属性**：当调用 observe() 方法时，**<font color=FF0000 size=4>childList，attributes 或者 characterData 三个属性之中，至少有一个必须为 true</font>，否则会抛出 TypeError 异常**。

- **attributeFilter：可选**，<font color=FF0000>要监视的特定属性名称的数组</font>。<mark>如果未包含此属性，则对所有属性的更改都会触发变动通知</mark>。<font color=FF0000>无默认值</font>。

- **attributeOldValue：可选**，当监视节点的属性改动时，将此属性设为 true 将记录任何有改动的属性的上一个值。有关观察属性更改和值记录的详细信息。<font color=FF0000>无默认值</font>。

  **即：如果为 true，则将特性的旧值和新值都传递给回调，否则只传新值（需要 attributes 选项）**

- **attributes：可选**，设为 true 以观察受监视元素的属性值变更。<font color=FF0000>默认值为 false</font>。

- **characterData：可选**，设为 true 以监视指定目标节点或子节点树中节点所包含的字符数据的变化。<font color=FF0000>无默认值</font>。

- **characterDataOldValue：可选**，设为 true 以在文本在受监视节点上发生更改时记录节点文本的先前值。<font color=FF0000>无默认值</font>。

- **childList：可选**，设为 true 以监视目标节点（如果 subtree 为 true，则包含子孙节点）添加或删除新的子节点。<font color=FF0000>默认值为 false</font>。

- **subtree：可选**，设为 true 以将监视范围扩展至目标节点整个节点树中的所有节点。MutationObserverInit 的其他值也会作用于此子树下的所有节点，而不仅仅只作用于目标节点。<font color=FF0000>默认值为 false</font>。

摘自：[MDN - MutationObserverInit](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserverInit)

#### MutationObserver.disconnect()

MutationObserver 的 <font color=FF0000>disconnect() 方法告诉观察者停止观察变动</font>。<font color=FF0000> **可以通过调用其observe()方法来重用观察者**</font>。

- **语法**

  ```js
  mutationObserver.disconnect()
  ```

- **参数：**无

- **返回值：**undefined

- **使用说明：**<font color=FF0000>如果被观察的元素被从DOM中移除，然后被浏览器的垃圾回收机制释放，此MutationObserver将同样被删除</font>。

摘自：[MDN - MutationObserver.disconnect()](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver/disconnect)

#### MutationObserver.takeRecords()

MutationObserver 的 takeRecords() 方法<font color=FF0000>**返回已检测到但尚未由观察者的回调函数处理的所有匹配DOM更改的列表，使变更队列保持为空**</font>。 此方法<font color=FF0000>**最常见的使用场景**是<font size=4>**在断开观察者之前立即获取所有未处理的更改记录**</font>，以便在停止观察者时可以处理任何未处理的更改</font>。

- **语法**

  ```js
  mutationRecords = mutationObserver.takeRecords()
  ```

- **参数：**无

- **返回值：**<font color=FF0000>返回一个MutationRecord 对象列表</font>，<mark>每个对象都描述了应用于DOM树某部分的一次改动</mark>。
- **注意：**<font color=FF0000>调用takeRecords()后，已发生但未传递给回调的变更队列将保留为空</font>。

摘自：[MDN - MutationObserver.takeRecords()](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver/takeRecords)

#### MutationRecord

<font color=FF0000>每个 MutationRecord 都代表一个独立的 DOM 变化</font>，**在每次随 DOM 变化调用 MutationObserver 的回调函数时，一个相应的 MutationRecord 会被作为参数，传递给回调函数**。

摘自：[MDN - MutationRecord](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationRecord)

**MutationRecord 对象具有以下属性：**

- **type：**变动类型，以下类型之一：
  - "attributes"：特性被修改了，
  - "characterData"：数据被修改了，用于文本节点，
  - "childList"：添加/删除了子元素。
- **target：**更改发生在何处："attributes" 所在的元素，或 "characterData" 所在的文本节点，或 "childList" 变动所在的元素，
- **addedNodes / removedNodes：**添加/删除的节点，
- **previousSibling / nextSibling：**添加/删除的节点的上一个/下一个兄弟节点，
- **attributeName / attributeNamespace：**被更改的特性的名称/命名空间（用于 XML），
- **oldValue：**之前的值，仅适用于特性或文本更改，如果设置了相应选项 attributeOldValue/characterDataOldValue。

摘自：[现代 JavaScript 教程 - DOM 变动观察器（Mutation observer）](https://zh.javascript.info/mutation-observer)

<font size=4>**补充：**</font>

以下内容摘自 [MutationObserver 和 IntersectionObserver](https://juejin.cn/post/6999950594207121444)。另外，这篇文章还有更详细的介绍（也比 MDN 中的容易理解），推荐阅读。由于只是简单了解，同时 当前的工作中完全找不到使用场景，所以，这里略。

MutationObserver 可以用来监听 DOM 的任何变化，比如子元素、属性和文本内容的变化。

<mark>概念上，它很接近事件，可以理解为 DOM 发生变动就会触发 Mutation Observer 事件</mark>。但是，<font color=FF0000>它与事件有一个本质不同</font>：<font color=FF0000>**事件是同步触发**</font>，也就是说，<font color=FF0000>**DOM 的变动立刻会触发相应的事件**</font>；而 <font color=FF0000>Mutation Observer 则是 <font size=4>**异步触发**</font></font>，<font color=FF0000>DOM 发生变化并不会马上触发</font>，而是<font color=FF0000 size=4>**要等到当前所有 DOM 操作都结束才触发**</font>，执行时机有点类似于宏任务。

<mark>这样设计是为了应对 DOM 变动频繁的特点</mark>。<mark>如果不这么做，当文档中连续插入 1000 个 \<p> 元素，就会连续触发 1000 个插入事件并执行每个事件的回调函数，这很可能造成浏览器的卡顿</mark>。而 Mutation Observer 完全不同，只在 1000 个段落都插入结束后才会触发，而且只触发一次。

**综上所述，Mutation Observer 有以下特点：**

- 它等待所有脚本任务完成后，才会触发（宏任务）。
- 它把所有 DOM 变动记录封装成一个数组进行处理，而不是单独处理每个 DOM 变动。
- 它既可以观察 DOM 的所有类型变动，也可以指定只观察某一类型的变动。

#### Intersection Observer 

> **注：**intersection 译为 交叉、相交，这可用于辅助记忆。

IntersectionObserver 接口（<mark>从属于 Intersection Observer API </mark>）<font color=FF0000>提供了一种 <font size=4>**异步 观察 目标元素 与 其祖先元素 或 顶级文档视窗 ( viewport ) 交叉状态 的方法**</font></font>。<mark>祖先元素与视窗 ( viewport ) 被称为根 ( root ) </mark> **注：**这里 root 使用 下面有提及；另外，这里的 root 是一个相对概念，只表示祖先节点，不是绝对的根节点。

<font color=FF0000>当一个 IntersectionObserver 对象被创建时，其被配置为监听根中一段给定比例的可见区域</font>。<font color=FF0000 size=4>**一旦 IntersectionObserver 被创建，则无法更改其配置**</font>，所以 **一个给定的观察者对象** 只能用来监听可见区域的  **特定变化值**；然而，你 <font color=FF0000>可以在同一个观察者对象中配置监听多个目标元素</font>。

##### 构造器

**IntersectionObserver.IntersectionObserver()：**创建一个新的 IntersectionObserver 对象，<font color=FF0000>当其监听到 **目标元素的可见部分穿过了一个或多个阈** ( thresholds ) 时，**会执行指定的回调函数**</font>。

> #### IntersectionObserver.IntersectionObserver()
>
> IntersectionObserver() 构造器创建并返回一个 IntersectionObserver 对象。 如果指定 rootMargin 则会检查其是否符合语法规定，检查阈值以确保全部在 0.0 到 1.0 之间，并且阈值列表会按升序排列。如果阈值列表为空，则默认为一个 [0.0] 的数组。
>
> ##### 语法
>
> ```js
> var observer = new IntersectionObserver(callback[, options]);
> ```
>
> ##### 参数
>
> - **callback**：<font color=FF0000>当元素可见比例超过指定阈值后，会调用一个回调函数</font>，此回调函数接受两个参数：
>   - **entries**：<font color=FF0000>一个 IntersectionObserverEntry 对象的 **数组**</font>，每个被触发的阈值，都或多或少与指定阈值有偏差。
>   - **observer**：被调用的 IntersectionObserver 实例
> - **options**：可选，一个可以用来配置 observer 实例的对象。如果 options 未指定，observer 实例默认使用文档视口作为 root，并且没有 margin，阈值为 0%（意味着即使一像素的改变都会触发回调函数）。你可以指定以下配置：
>   - **root**：<font color=FF0000>监听元素的祖先元素 Element 对象，其边界盒将被视作视口</font>。目标在根的可见区域的的任何不可见部分都会被视为不可见
>   - **rootMargin**：一个<font color=FF0000>在计算交叉值时添加至根的边界盒 ( [bounding_box](https://developer.mozilla.org/en-US/docs/Glossary/bounding_box) ) 中的一组偏移量</font>，类型为字符串 ( string ) ，可以有效的缩小或扩大根的判定范围从而满足计算需要。语法大致和 CSS 中的 margin 属性等同。默认值是 "0px 0px 0px 0px"
>   - **threshold**：<font color=FF0000>规定了一个监听目标与边界盒交叉区域的比例值，可以是 **一个具体的数值** 或是 **一组 0.0 到 1.0 之间的数组**</font>。若指定值为 0.0，则意味着监听元素即使与根有 1 像素交叉，此元素也会被视为可见。若指定值为 1.0，则意味着整个元素都在可见范围内时才算可见。阈值的默认值为 0.0
>
> ##### 返回值
>
> 一个可以使用规定阈值监听目标元素可见部分与 root 交叉状况的新的 IntersectionObserver 实例。调用自身的 observe() 方法开始使用规定的阈值监听指定目标。
>
> ##### 异常
>
> - **SyntaxError**：指定的 rootMargin 不存在。
> - **RangeError**：一个或多个阈值超出了 0.0 到 1.0 的范围。
>
> 摘自：[MDN - IntersectionObserver.IntersectionObserver()](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver/IntersectionObserver)

##### 属性

- **IntersectionObserver.root：**<font color=FF0000>只读</font>，<font color=FF0000>所 **监听对象的具体祖先元素** ( element )</font>。<font color=FF0000>如果 **未传入值 或 值为 null**，则 **默认使用顶级文档的视窗**</font>
- **IntersectionObserver.rootMargin：**<font color=FF0000>只读</font>，计算交叉时 添加到根 ( root ) 边界盒 bounding box 的 矩形<font color=FF0000>偏移量</font>， <font color=FF0000>可以有效的缩小 或 扩大根的判定范围从而满足计算需要</font>。此属性返回的值可能与调用构造函数时指定的值不同，因此可能需要更改该值，以匹配内部要求。<font color=FF0000>所有的偏移量均可用像素 ( pixel / px ) 或百分比 ( percentage / % ) 来表达</font>，<mark>默认值为 "0px 0px 0px 0px"</mark>。
- **IntersectionObserver.thresholds：**<font color=FF0000>只读</font>，<font color=FF0000>一个包含阈值的列表，按升序排列，**列表中的每个阈值都是监听对象的交叉区域与边界区域的比率**</font>。<font color=FF0000 size=4>**当监听对象的任何阈值被越过时，*都* 会生成一个通知 ( Notification )**</font>。<mark>如果构造器未传入值, 则默认值为 0</mark>。

##### 方法

- **IntersectionObserver.disconnect()：**使 IntersectionObserver 对象 <font color=FF0000>停止监听</font>工作。
- **IntersectionObserver.observe()：**使 IntersectionObserver <font color=FF0000>开始监听</font>一个目标元素。
- **IntersectionObserver.takeRecords()：**<font color=FF0000>返回所有观察目标的 IntersectionObserverEntry 对象数组</font>。
- **IntersectionObserver.unobserve()：**使 IntersectionObserver <font color=FF0000>停止监听特定目标元素</font>。

##### 示例

```js
var intersectionObserver = new IntersectionObserver(function(entries) {
  // If intersectionRatio is 0, the target is out of view and we do not need to do anything.
  if (entries[0].intersectionRatio <= 0) return;

  loadItems(10);
  console.log('Loaded new items');
});
// start observing
intersectionObserver.observe(document.querySelector('.scrollerFooter'));
```

**注：**上面代码提到了 IntersectionObserverEntry.intersectionRatio

> The `IntersectionObserverEntry` interface's <font color=FF0000>read-only</font> **`intersectionRatio`** property <font color=FF0000>tells you **how much of the target element is currently visible within the root's intersection ratio**, **as a value between 0.0 and 1.0**</font>
>
> 摘自：[MDN - IntersectionObserverEntry.intersectionRatio](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserverEntry/intersectionRatio)

摘自：[MDN - Intersection Observer](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver)

##### 补充

以下内容摘自  [MutationObserver 和 IntersectionObserver](https://juejin.cn/post/6999950594207121444)。另外，这篇文章还有更详细的介绍（也比 MDN 中的容易理解），推荐阅读。由于只是简单了解，同时 当前的工作中完全找不到使用场景，所以，这里略。

**痛点：**<mark>以往我们 **实现图片懒加载** 往往是通过监听存放图片的滚动容器的滚动事件（ 即：scroll 事件）或者 整个页面的滚动事件，在滚动事件触发的时候调用图片元素的 getBoundingClientRect() 函数来进行可见性比对</mark>。<font color=FF0000>这种方式是在事件触发的时候同步进行，如果运算量过大极有可能会导致主线程阻塞，从而页面卡顿</font>。延伸开来，我们急迫地需要一个高性能的元素可见性变化解决方案，所以 IntersectionObserver 诞生了。（**注：**也就是说，<font color=FF0000>**IntersectionObserver 可用来实现图片懒加载**</font>）

**概述：**<font color=FF0000>这个 API 可以 <font size=4>**观察目标元素与视口或指定根元素产生的交叉区的变化**</font>，所以这个 API 也叫做“交叉观察器”</font>。<mark>它和 MutationObserver 一样都是 <font color=FF0000>**异步的**</font>，不随着目标元素的滚动 同步触发</mark>。<font color=FF0000>发明者规定，IntersectionObserver 的实现，应该采用 requestIdleCallback() 的方式，即只有线程空闲下来，才会执行观察器</font>。这意味着，<mark>这个观察器的优先级非常低，只在其他任务执行完，浏览器有了空闲才会执行</mark>。**注：**异步 + 低优先级 所以高性能（似乎也可以称为：不会影响性能...）

摘自：[MutationObserver 和 IntersectionObserver](https://juejin.cn/post/6999950594207121444) 。文章中还有 root、rootmargin、threshold 的 介绍，内容比 MDN 清楚、具体很多，这里略。

在 CodingStartup 的教程 [【CSS】动态划线效果｜使用IntersectionObserver](https://www.bilibili.com/video/BV1xa4y1a7J9) 有关于 IntersectionObserver 的使用；相关代码如下：

```js
let observer = new IntersectionObserver((entries, observer) => {
  entries.forEach(entry => {
    // 如果 监听的目标元素 和 root 相交，则...
    if(entry.isIntersecting) {
      entry.target.classList.add('animation')
      observer.unobserve(entry.target)
    }
  })
})

// 设置监听的目标元素
document.querySelectorAll('mark').forEach(mark => { observer.observe(mark) })
```

##### 关于 IntersectionObserverEntry.isIntersecting

> The `IntersectionObserverEntry` interface's <font color=FF0000>read-only</font> **`isIntersecting`** property is a <font color=FF0000>Boolean value</font> which is <font color=FF0000>`true` if the target element intersects with the intersection observer's root</font>. If this is `true`, then, the `IntersectionObserverEntry` describes a transition into a state of intersection; if it's `false`, then you know the transition is from intersecting to not-intersecting.
>
> 摘自：[MDN - IntersectionObserverEntry.isIntersecting](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserverEntry/isIntersecting)

##### 图示

| <img src="https://s2.loli.net/2022/06/13/5ENmXiyIGuxBrt8.gif" alt="图片" style="zoom:50%;" /> | <img src="https://s2.loli.net/2022/06/13/kPItm6xRZFfSvGr.gif" alt="图片" style="zoom:50%;" /> |
| :----------------------------------------------------------: | :----------------------------------------------------------: |

摘自：[一起认识下浏览器的5种观察器](https://mp.weixin.qq.com/s/0Kj0cUYrQtx7Bh2VuZ-pGQ)

##### Intersection Observer 的使用场景

- **图片的懒加载**

  **注：**应该也包含上面 codingstartup 的 示例：当动画可见（在 视口 viewport 中）时，才会触发。对于定时触发的动画也可以通过 intersection observer 可见时运行，不可见时停止。

- 无限滚动

- <font color=FF0000>**网页广告的曝光量统计**</font>

  > 很多时候，广告图片不一定需要全部展示才算被用户看到，有时候图片只展示了 60% 时，主要信息已经被用户看到，这种情况其实是可以算作一次曝光量的统计。为了实现这种广告的曝光量的精确统计，我们可以创建交叉管理器，观察到广告目标元素的交叉比例 intersectionRatio 达到 0.6 时，判定广告的 曝光量 +1
  >
  > 摘自：[一起认识下浏览器的5种观察器](https://mp.weixin.qq.com/s/0Kj0cUYrQtx7Bh2VuZ-pGQ)



#### ResizeObserver 🧪

> ##### Resize Observer API
>
> Resize Observer API 提供了一种 <font color=FF0000>**高性能的机制**</font>，通过该机制，<font color=FF0000>代码可以监视元素的大小更改，并且 **每次大小更改** 时都会向观察者传递通知</font>
>
> ##### 背景 / 痛点
>
> 存在大量的<font color=FF0000>响应式设计</font>（以及其他相关）技术，它们<font color=FF0000>可以响应元素大小的变化</font>；但是以前，它们的实现常常很笨拙或者说生硬。
>
> 举个例子：当视口更改大小时， 媒介查询 / window.matchMedia 非常适合在特定点更新布局，但是如果要响应于特定元素的大小更改而更改布局，该元素又不是外部容器时，该怎么办？
>
> 为此，<mark>一种 **有限的解决方案** 是监听对适当事件的更改，该事件会提示您对更改大小感兴趣的元素</mark>（例如 window resize event ），<mark>然后找出该元素之后新的尺寸或其他功能</mark>；例如，使用 Element.getBoundingClientRect 或者 Window.getComputedStyle 来调整大小。 <font color=FF0000>**这样的解决方案仅适用于有限的场景，对性能不利**（不断调用上述方法会导致性能严重下降），并且在不更改浏览器窗口大小的情况下通常不起作用</font>。
>
> <font color=FF0000>Resize Observer API 提供了一种解决此类问题的解决方案，此外，它还**使您能够轻松观察和响应元素内容或边框的大小变化**，并 **以高效的方式做出响应**</font>。 它为 Web 平台中经常讨论的缺少 element queries 提供了 JavaScript 解决方案。
>
> 用法很简单，并且与其他观察者（例如 Performance Observer 或者 Intersection Observer ）几乎相同，您可以使用 ResizeObserver() 构造函数创建一个新的 ResizeObserver ，然后使用 ResizeObserver.observe() 使其寻找特定元素大小的更改。 <font color=FF0000>**每次更改大小时，构造函数中设置的回调函数便会运行**，从而提供对新维度的访问权限，并允许您根据需要执行任何操作</font>。
>
> 摘自：[MDN - Resize Observer API](https://developer.mozilla.org/zh-CN/docs/Web/API/Resize_Observer_API)

ResizeObserver 接口 <font color=FF0000>可以监听到 Element 的 <font size=4>**内容区域**</font>（**注：**Mutation observer 是监听 DOM树 的改变）</font> **或** <font color=0000FF>SVGElement的边界框</font> <font size=4>**改变**</font>。<font color=FF0000>内容区域则需要减去内边距 padding</font>。（有关内容区域、内边距资料见 [盒子模型](https://developer.mozilla.org/docs/Learn/CSS/Introduction_to_CSS/Box_model)）

<font color=FF0000>ResizeObserver **避免了在自身回调中调整大小，从而触发的无限回调和循环依赖**</font>。它 <font color=FF0000>仅通过在后续帧中处理DOM中更深层次的元素来实现这一点</font>。<mark>如果（浏览器）遵循规范，**只会在 绘制前或布局后触发调用**</mark>。

##### 构造器

**ResizeObserver()：**创建并返回一个 ResizeObserver 对象。

##### 方法

- **ResizeObserver.disconnect()：**<font color=FF0000>取消和结束 目标**对象上所有 **对 Element 或 SVGElement 观察</font>
- **ResizeObserver.observe()：**<font color=FF0000>开始观察</font>指定的 Element 或 SVGElement
- **ResizeObserver.unobserve()：**<font color=FF0000>结束观察</font>指定的 Element 或 SVGElement

##### 示例

以下示例通过观察 box 的宽度变化从而改变其边框圆角半径。**注：**相关使用和 上面的 intersectionObserver 相当类似

```js
const resizeObserver = new ResizeObserver(entries => {
  for (let entry of entries) {
    entry.target.style.borderRadius = Math.max(0, 250 - entry.contentRect.width) + 'px';
  }
});

resizeObserver.observe(document.querySelector('.box:nth-child(2)'));
```

摘自：[MDN - ResizeObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/ResizeObserver)

#### ResizeObserverEntry

The `ResizeObserverEntry` interface represents the object passed to the `ResizeObserver()` constructor's callback function, which <font color=FF0000>allows you to access the new dimensions （尺寸） of the `Element` or `SVGElement` being observed</font>.

##### Properties

- **ResizeObserverEntry.borderBoxSize**: <mark>Read only</mark>, An object containing the  <font color=FF0000>new **border box** size</font> of the observed element when the callback is run. **注：**注意 和 contentBoxSize 的区别

- **ResizeObserverEntry.contentBoxSize**: <mark>Read only</mark>, An object containing the <font color=FF0000>new **content box** size</font> of the observed element when the callback is run.

- **ResizeObserverEntry.devicePixelContentBoxSize**: <mark>Read only</mark>, An object containing the <font color=FF0000>new content box size in device pixels</font> of the observed element when the callback is run.

- **ResizeObserverEntry.contentRect**: <mark>Read only</mark>, <font color=FF0000>**A `DOMRectReadOnly` object**</font> （**注：** `getBoundingClientRect` 返回值是一个 `DOMRect` ，而 `DOMRectReadOnly` 继承自 `DOMRect` ）<font color=FF0000>**containing the new size of the observed element**</font> when the callback is run. 

  Note that <mark>this is better supported than the above two properties</mark>, but it is left over from an earlier implementation of the Resize Observer API, is still included in the spec for web compat reasons, and may be deprecated in future versions.

- **ResizeObserverEntry.target**: <mark>Read only</mark>, <font color=FF0000>A reference to the `Element` or `SVGElement` being observed</font>.

##### Methods

None.

摘自：[MDN US - ResizeObserverEntry](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserverEntry)



#### PerformanceObserver 性能监测对象

PerformanceObserver <font color=FF0000>用于监测性能度量事件</font>，<mark>在浏览器的性能时间轴记录下一个新的 performance entries  的时候将会被通知</mark> 。
注意：<mark>此特性在 <font color=FF0000>**Web Worker**</font> 中可用</mark>。**注：**performance entries 是 Performance API 中的内容，详见下面 [[#Performance API]]

##### 构造函数

**PerformanceObserver()：**<font color=FF0000>创建并返回一个新的 PerformanceObserver 对象</font>。

##### 方法

- **PerformanceObserver.observe()：**指定监测的 entry types 的集合（**注：**即，数组）。 当 performance entry 被记录并且是指定的 entryTypes 之一的时候，性能观察者对象的回调函数会被调用。

  > entry types 即：PerformanceEntry.entryType。包含 frame/navigation、resource、mark、measure、paint。
  >
  > 参考自：[MDN - PerformanceEntry.entryType](https://developer.mozilla.org/zh-CN/docs/Web/API/PerformanceEntry/entryType)

- **PerformanceObserver.disconnect()：**性能监测回调 <font color=FF0000>停止接收 性能条目</font>。

##### 示例

```js
function perf_observer(list, observer) {
   // 处理 "measure" 事件
}
var observer2 = new PerformanceObserver(perf_observer);
observer2.observe( { entryTypes: ["measure"] } )
```

摘自：[MDN - 性能监测对象](https://developer.mozilla.org/zh-CN/docs/Web/API/PerformanceObserver)

#### ReportingObserver

// TODO

摘自：[MDN - ReportingObserver](https://developer.mozilla.org/en-US/docs/Web/API/ReportingObserver)

另外，「神说要有光」的文章 [浏览器的 5 种 Observer，你用过几种？](https://mp.weixin.qq.com/s/-fLZe164aWAaPJ55iZ_m2w) 推荐阅读。// TODO



#### Performance API

> **注：**当时注意到 Performance API，是因为“首屏渲染优化”，就按习惯搜起了 MDN ...看到后面发现， 自己不应该在不了解背景的情况下，一上来就看 API，这样很容易看懵，而且看了没怎么用也就忘了；所以，如果之后想要复习这部分的内容，可以先看看后面的 [[#Performance Timeline 介绍]] 了解下背景。

High Resolution Time 标准定义了 Performance 接口，该接口支持应用程序中客户端的延时测量。<font color=FF0000>Performance 接口被认为是高采样率的</font>，因为其<font color=FF0000>精确度可达千分之一毫秒</font>（受硬件或软件限制）。这些接口支持许多使用情形，包括计算帧速率（在动画中可能很重要）和基准测试（例如加载资源的时间）。

由于平台的系统时钟会受到各种时滞（例如 NTP 调整）的影响，该接口支持单调时钟，即一直增加的时钟。 鉴于这个原因， <font color=FF0000>**Performance API 定义了 DOMHighResTimeStamp 类型**</font>，而不是使用 Date.now() 接口。

> Date.now() 方法只适用于日期时间相关操作，而且是不要求计时精度的操作
>
> 摘自：《 JS 高级程序设计》第四版 P644 ：20.10 计时 API

##### DOMHighResTimeStamp

DOMHighResTimeStamp 类型，顾名思义，表示高采样率的时间戳。 此类型是 double ，由性能接口使用。 该值可以是离散时间戳，也可以是两个离散时间戳之间的时间间隔。

DOMHighResTimeStamp 的单位是毫秒，应精确到 5 µs（微秒）（**注：**单位是毫秒，但是有小数）。 但是，如果浏览器无法提供精确到 5 微秒的时间数值（例如由于硬件或软件限制），则浏览器可以将该值表示为精确到毫秒的时间（以毫秒为单位）。

##### 方法

**Performance 接口具有两个方法**（**注：**感觉有点不对，毕竟下面 [[#Performance#方法]] 有很多方法...）

- **now() 方法**：返回一个 DOMHighResTimeStamp，其值取决于 navigation start 和 作用域。如果作用域是 window ，则值是创建 ***浏览器上下文*** 的时间；如果作用域是 worker，则值是创建 worker 的时间。

  > **注：**在 Chrome 和 Safari 中测试，now 方法返回的均是当前标签页 ( Tab ) 打开的时间（单位是毫秒）。所以，这里所说的 ***浏览器上下文***  <mark>**感觉应该**</mark> 和 ***标签页*** 一一对应，即：打开一个 ***标签页***，生成一个 ***浏览器上下文***

- **toJSON() 方法**：返回 Performance 对象的序列化结果，包含可以被序列化的属性。

##### 属性

**Performance 接口具有两个属性**（**注：**还是感觉有点不对，毕竟下面 [[#Performance#属性]] 有很多属性...）

- **timing 属性**：返回一个 PerformanceTiming 对象，其中包含与延时相关的性能信息，例如导航开始的时间，重定向的开始时间和结束时间，响应的开始时间和结束时间等。

- **navigation 属性**：返回一个 PerformanceNavigation 对象，该对象表示在给定浏览上下文中发生的导航类型，例如从历史记录导航到的页面，通过跟随链接导航到的页面等

##### 接口

- **Performance**：提供方法和属性，包含给定页面与计时相关的性能信息

- **PerformanceEntry**：提供方法和属性，将单个性能指标封装为性能时间轴的一部分

- **PerformanceFrameTiming**：提供方法和属性，包含有关<font color=FF0000>浏览器事件循环的 帧计时数据</font>

- **PerformanceMark**：条目类型为 "mark" 的 PerformanceEntry 抽象接口，该类型的条目通过调用 mark() 将命名的DOMHighResTimeStamp mark 添加到浏览器的性能时间轴来创建

  > **注：**这里的 mark() ，即 Performance.mark()

- **PerformanceMeasure**：条目类型为 "measure" 的 PerformanceEntry 抽象接口，该类型的条目通过调用 measure() 在浏览器的性能时间轴的两个标记之间添加一个命名的 DOMHighResTimeStamp measure 来创建

  > **注：**这里的 measure() ，即 Performance.measure()

- **PerformanceNavigationTiming**：提供方法和属性，用于 <font color=FF0000>存储和检索</font> 有关 <font color=FF0000>浏览器文档导航事件 的高采样率时间戳或其他指标</font>。

  > **注：**即 Navigation Timing API
  >
  > 另外，PerformanceResourceTiming 继承自 PerformanceEntry
  >
  > ```mermaid
  > classDiagram
  > direction LR
  > PerformanceEntry <|-- PerformanceResourceTiming : Inheritance
  > ```
  >
  > 摘自：[MDN - PerformanceResourceTiming](https://developer.mozilla.org/zh-CN/docs/Web/API/PerformanceResourceTiming)

- **PerformanceObserver**：提供方法和属性，用于观察性能测量事件，并在浏览器的性能时间轴中记录新的 PerformanceEntry 时进行通知

  > **注：**可以参考上面的 [[#PerformanceObserver 性能监测对象]]

- **PerformanceResourceTiming**：提供方法和属性，用于<font color=FF0000>检索和分析</font> 有关 <font color=FF0000>应用程序 **资源加载** 的详细网络计时数据</font>

  > **注：**即 Resource Timing API

摘自：[MDN - Performance API](https://developer.mozilla.org/zh-CN/docs/Web/API/Performance_API)

#### Performance

Performance 接口<font color=FF0000>**可以获取到当前页面中与性能相关的信息**</font>。它<font color=FF0000>是 High Resolution Time API 的一部分</font>，同时也融合了 Performance Timeline API、Navigation Timing API、 User Timing API 和 Resource Timing API。

> ##### Performance 接口由多个 API 构成
>
> - High Resolution Time API
>
> - Performance Timeline API
> - Navigation Timing API
> - User Timing API
> - Resource Timing API
> - Paint Timing API
>
> 摘自：《 JS 高级程序设计》第四版 P644 ：20.10 计时 API

<font color=FF0000>该类型的对象可以通过调用只读属性 Window.performance 来获得</font>。

> **注意 ⚠️：**除了以下指出的情况外，<font color=FF0000>该接口及其成员在 Web Worker 中可用</font>。此外，还需注意：performance 的创建和衡量都是同一环境下的。即，<mark>如果你在主线程（或者其他 worker ）中创建了一个 performance ，那么它在另外的 worker 线程中是不可用的；反之亦然</mark>。

##### 属性

**Performance 接口没有继承任何属性**

- **Performance.navigation：**🗑 已废弃， 只读。PerformanceNavigation 对象<font color=FF0000>提供了在指定的时间段里发生的操作相关信息</font>。包括页面是加载还是刷新、发生了多少次重定向等等。<font color=FF0000>**workers 中不可用**</font>

- **Performance.timing**：🗑 已废弃，只读。PerformanceTiming 对象<font color=FF0000>**包含延迟相关的性能信息**</font>。<font color=FF0000>**workers 中不可用**</font>。

  > 在 Navigation Timing Level 2 specification 中已经被废弃，请<font color=FF0000>使用 Performance.timeOrigin 替代</font>。
  >
  > 摘自：[MDN - Performance.timing](https://developer.mozilla.org/zh-CN/docs/Web/API/Performance/timing)

- **performance.memory**：⚠️ 非标准。是 Chrome 添加的一个非标准扩展，这个属性<font color=FF0000>提供了一个可以获取到基本内存使用情况的对象</font>。**不应该**使用这个非标准的 API 。

- **Performance.timeOrigin**：⚠️ 非标准，只读。返回性能测量开始时的时间的<font color=FF0000>高精度时间戳</font>。

  > **注：**经测试，返回的 *时间戳* 精确到毫秒，和 Date.now() 同一个层级。另外，既然是 *时间戳*，所以必然是绝对时间，而不是相对时间。

##### 事件处理程序

- **Performance.onresourcetimingbufferfull**：一个回调的 EventTarget ，当触发 resourcetimingbufferfull 事件的时候会被调用

##### 方法

**Performance 接口没有继承任何方法**

- **Performance.clearMarks()**：将给定的 mark 从浏览器的性能输入缓冲区中移除。

- **Performance.clearMeasures()**：将给定的 measure 从浏览器的性能输入缓冲区中移除。

- **Performance.clearResourceTimings()**：从浏览器的 *性能数据缓冲区* 中移除所有 entryType 是 "resource" 的 performance entries

- <font color=FF0000>**Performance.getEntries()**</font>：基于给定的 filter 返回一个 PerformanceEntry 对象的列表。

  > **注：**filter 是 getEntries 方法传入的参数，语法如下。另外，相对后面的 getEntriesByName 和 getEntriesByType 方法，getEntries 方法传入 filter 要更灵活，泛用性更强些。
  >
  > ```js
  > // 取全部
  > entries = window.performance.getEntries();
  > entries = window.performance.getEntries(PerformanceEntryFilterOptions);
  > 
  > // 取特定
  > entries = performance.getEntries({name: "entry_name", entryType: "mark"});
  > ```
  >
  > 摘自：[MDN - Performance.getEntries()](https://developer.mozilla.org/zh-CN/docs/Web/API/Performance/getEntries)

- <font color=FF0000>**Performance.getEntriesByName()**</font>：基于给定的 name 和 entry type 返回一个 PerformanceEntry 对象的列表。

- <font color=FF0000>**Performance.getEntriesByType()**</font>：基于给定的 entry type 返回一个 PerformanceEntry 对象的列表

- <font color=FF0000>**Performance.mark()**</font>：根据给出 name 值，<font color=FF0000>在 ***浏览器的性能输入缓冲区*** 中创建一个相关的 timestamp</font>

  > **注：** name 是 mark 方法中指定的参数
  >
  > ```js
  > performance.mark(name);
  > ```
  >
  > 其中 name 是 一个表示标记名称的 DOMString
  >
  > 摘自：[MDN - Performance.mark()](https://developer.mozilla.org/zh-CN/docs/Web/API/Performance/mark)

- **Performance.measure()**：在浏览器的指定 start mark 和 end mark 间的性能输入缓冲区中创建一个指定的 timestamp

- <font color=FF0000>**Performance.now()**</font>：<font color=FF0000>返回一个表示 **从性能测量时刻开始经过的毫秒数** DOMHighResTimeStamp</font>

- **Performance.setResourceTimingBufferSize()**：将浏览器的资源 timing 缓冲区的大小设置为 "resource" type performance entry 对象的指定数量

- **Performance.toJSON()**：其是一个 JSON 格式转化器，返回 Performance 对象的 JSON 对象

摘自：[MDN - Performance](https://developer.mozilla.org/zh-CN/docs/Web/API/Performance)

#### PerformanceTiming

PerformanceTiming 接口是 <font color=FF0000>**为保持向后兼容性而保留的传统接口**</font>（**注：**即 PerformanceTiming 接口 已经被废弃，现在因为兼容而保存），并且<font color=FF0000>提供 **在加载和使用当前页面期间发生的各种事件的性能计时信息**</font>。可以通过 **只读属性** window.performance.timing 获得实现该接口的一个对象。

##### 属性

PerformanceTiming 接口不包含任何继承属性

- **PerformanceTiming.navigationStart**：只读，是一个无符号 long long 型的毫秒数。表征了<font color=FF0000>从 **同一个浏览器上下文** 的上一个文档卸载 ( unload ) 结束时的 UNIX 时间戳</font>。<mark>如果没有上一个文档，这个值会和 PerformanceTiming.fetchStart 相同</mark>。

- **PerformanceTiming.unloadEventStart**：只读，是一个无符号 long long 型的毫秒数。表征了 <font color=FF0000>unload 事件抛出时的 UNIX 时间戳</font>。如果没有上一个文档，或者 之前的文档或所需的重定向之一不是同一来源，这个值会返回 0

- **PerformanceTiming.unloadEventEnd**：只读，是一个无符号long long 型的毫秒数。表征了 <font color=FF0000>unload 事件处理完成时的 UNIX 时间戳</font>。如果没有上一个文档，或者 之前的文档或所需的重定向之一不是同一来源，这个值会返回 0

- **PerformanceTiming.redirectStart**：只读，是一个无符号 long long 型的毫秒数。表征了 <font color=FF0000>**第一个 HTTP *重定向开始* 时** 的 UNIX 时间戳</font>。如果没有重定向，或者重定向中的一个不同源，这个值会返回 0

- **PerformanceTiming.redirectEnd**：只读，是一个无符号 long long 型的毫秒数。表征了 <font color=FF0000>**最后一个 HTTP *重定向完成* 时**（也就是说是 HTTP 响应的最后一个比特直接被收到的时间）的 UNIX 时间戳</font>。如果没有重定向，或者重定向中的一个不同源，这个值会返回 0

- **PerformanceTiming.fetchStart**：只读，是一个无符号 long long 型的毫秒数。表征了 浏览器 <font color=FF0000>**准备好 使用 HTTP 请求** 来获取 ( fetch ) 文档的 UNIX 时间戳</font>。这个时间点会在检查任何应用缓存之前。

- **PerformanceTiming.domainLookupStart**：只读，是一个无符号 long long 型的毫秒数。表征了 <font color=FF0000>**域名查询开始的 UNIX 时间戳**</font>。如果使用了持续连接 ( persistent connection，**注：**感觉可以理解为 “长连接” Connection: Keep-Alive )，或者这个信息存储到了缓存或者本地资源上，这个值将和 PerformanceTiming.fetchStart 一致

- **PerformanceTiming.domainLookupEnd**：只读，是一个无符号 long long 型的毫秒数。表征了 <font color=FF0000>**域名查询结束的 UNIX 时间戳**</font>。如果使用了持续连接 ( persistent connection )，或者这个信息存储到了缓存或者本地资源上，这个值将和 PerformanceTiming.fetchStart 一致

- **PerformanceTiming.connectStart**：只读，是一个无符号 long long 型的毫秒数。返回HTTP请求开始向服务器发送时的Unix毫秒时间戳。如果使用持久连接 ( persistent connection )，则返回值等同于 fetchStart 属性的值

- **PerformanceTiming.connectEnd**：只读，是一个无符号 long long 型的毫秒数。返回浏览器与服务器之间的连接建立时的 Unix 毫秒时间戳。如果建立的是持久连接，则返回值等同于 fetchStart 属性的值。连接建立指的是所有握手和认证过程全部结束

  - **PerformanceTiming.secureConnectionStart**：只读，是一个无符号 long long 型的毫秒数。返回 <font color=FF0000>浏览器与服务器开始 **安全链接**（**注：**即 https ） 的握手时的 Unix 毫秒时间戳</font>。如果当前网页不要求安全连接，则返回 0

  关于 https 的说法来自：

  > ##### secureConnectionStart
  >
  > <font color=FF0000>This attribute is **optional**</font>. User agents that don't have this attribute available must set it as undefined. <font color=FF0000>When this attribute is available, if the scheme [URL] of the current page is "**https**"</font>, this attribute must return the time immediately before the user agent starts the handshake process to secure the current connection. If this attribute is available but HTTPS is not used, this attribute must return zero.
  >
  > 摘自：[w3.org - Navigation Timing Level 2 - 8. Obsolete - 8.1 The PerformanceTiming interface - secureConnectionStart](https://www.w3.org/TR/navigation-timing-2/#dom-performancetiming-secureconnectionstart)

- **PerformanceTiming.requestStart**：只读，是一个无符号 long long 型的毫秒数。返回浏览器向服务器发出 HTTP 请求时（或开始读取本地缓存时）的 Unix 毫秒时间戳

- **PerformanceTiming.responseStart**：只读，是一个无符号 long long 型的毫秒数。返回浏览器从服务器收到（或从本地缓存读取）第一个字节时的 Unix 毫秒时间戳。如果传输层在开始请求之后失败并且连接被重开，该属性将会被数制成新的请求的相对应的发起时间。

- **PerformanceTiming.responseEnd**：只读，是一个无符号 long long 型的毫秒数。返回浏览器从服务器收到（或从本地缓存读取，或从本地资源读取）最后一个字节时（如果在此之前 HTTP 连接已经关闭，则返回关闭时）的 Unix 毫秒时间戳

- **PerformanceTiming.domLoading**：只读，是一个无符号long long 型的毫秒数。返回 <font color=FF0000>当前网页 DOM 结构开始解析</font>时（即 <mark style="background: aqua"><font size=4>**Document.readyState 属性变为 “loading”**</font></mark>、相应的 readystatechange 事件触发时）的 Unix 毫秒时间戳

- **PerformanceTiming.domInteractive**：只读，是一个无符号 long long 型的毫秒数。返回 <font color=FF0000>**当前网页 DOM 结构结束解析、开始加载内嵌资源时**</font>（即 ]<mark style="background: aqua"><font  size=4>**Document.readyState 属性变为 “interactive”**</font></mark>、相应的 readystatechange 事件触发时）的 Unix 毫秒时间戳

- **PerformanceTiming.domContentLoadedEventStart**：只读，是一个无符号 long long 型的毫秒数。返回当解析器发送 DOMContentLoaded 事件，即所有需要被执行的脚本已经被解析时的 Unix 毫秒时间戳

- **PerformanceTiming.domContentLoadedEventEnd**：只读，是一个无符号 long long 型的毫秒数。返回 <font color=FF0000>当所有需要立即执行的脚本已经被执行</font>（不论执行顺序。**注：**即， 非 async 的脚本 ）时的 Unix 毫秒时间戳

- **PerformanceTiming.domComplete**：只读，是一个无符号 long long 型的毫秒数。返回 <font color=FF0000>**当前文档解析完成**</font>，即 <mark style="background: aqua"><font size=4>**Document.readyState 变为 ”complete“**</font></mark> ，且相对应的 readystatechange 被触发时的 Unix 毫秒时间戳

- **PerformanceTiming.loadEventStart**：只读，是一个无符号 long long 型的毫秒数。返回该文档下，<font color=FF0000>load 事件 **被发送** 时的 Unix 毫秒时间戳</font>。如果这个事件还未被发送，它的值将会是 0

- **PerformanceTiming.loadEventEnd**：只读，是一个无符号 long long 型的毫秒数。返回当 load 事件结束，即 <font color=FF0000>**加载事件完成时** 的 Unix 毫秒时间戳</font>。如果这个事件还未被发送，或者尚未完成，它的值将会是 0

##### 辅助记忆图

![img](https://s2.loli.net/2022/05/26/gNJI6i1hBrdA2Y3.png)

摘自：[MDN - PerformanceTiming](https://developer.mozilla.org/zh-CN/docs/Web/API/PerformanceTiming)

#### User Timing API

User Timing 接口允许开发者在浏览器性能时间线中创建针对特定应用的 ***时间戳*** 。<font color=FF0000>有两种自定义时间测量事件类型：mark 事件类型 和 measure 事件类型</font>。

mark 事件可以指定任意的名字并且可以在放在应用的任何位置。<font color=FF0000>measure 事件也可以指定为任意的名字，但是需要放在两个 mark 之间，所以它实际上是两个 mark 的中间点</font>。

摘自：[MDN - 自定义时间测量API](https://developer.mozilla.org/zh-CN/docs/Web/API/User_Timing_API)

#### performanceEntry

PerformanceEntry 对象代表了 performance 时间列表中的单个 metric 数据。每一个 performance entry 都可以在应用运行过程中通过手动构建 mark 或者 measure （ 例如调用 mark() 方法 ） 生成。此外，Performance Entries 在资源加载的时候，也会被动生成（ 例如：图片、script、CSS 等资源加载）

> **注：**一点总结
>
> performanceEntry 通过 getEntries( filter )、getEntriesByName( name, entryType )、getEntriesByType( enterType ) 检索，通过 mark( name ) 创建，clearMarks( name ) 移除。

##### 属性

- **PerformanceEntry.name**：只读，DOMString 该 performance entry 的名字
- **PerformanceEntry.entryType**：只读，DOMString 代表所上报的 performance metric 的 entryType 类型，例如 "mark"。可通过 [entryType](https://developer.mozilla.org/zh-CN/docs/Web/API/PerformanceEntry/entryType) 查阅完整的 entryType type 类型
- **PerformanceEntry.startTime**：只读，DOMHighResTimeStamp  此为 metric 上报时的时间
- **PerformanceEntry.duration**：只读，DOMHighResTimeStamp 该事件的耗时

摘自：[MDN - PerformanceEntry](https://developer.mozilla.org/zh-CN/docs/Web/API/PerformanceEntry)

#### Performance.measure()

 measure() 方法在浏览器性能记录缓存中创建了 DOMHighResTimeStamp 来记录两个特殊标志位（通常称为 ***开始标志*** 和 ***结束标志*** ）。 被命名的 DOMHighResTimeStamp 称为一次测量 ( measure )

##### 语法

```js
measure(measureName)
measure(measureName, MeasureOptions)
measure(measureName, startMark)
measure(measureName, startMark, endMark)
```

参数：略，详见：[MDN - performance.measure() - en-US](https://developer.mozilla.org/en-US/docs/Web/API/Performance/measure) 。不过，值得说一下的是：startMark 和 endMark 也是可以 performance.timing 中的值，详见： [[[#PerformanceTiming]]]

摘自：[MDN - performance.measure()](https://developer.mozilla.org/en-US/docs/Web/API/Performance/measure) 不过，中文版的内容和美版不一样，少了很多内容，建议看：[MDN - performance.measure() - en-US](https://developer.mozilla.org/en-US/docs/Web/API/Performance/measure) 



#### Performance Timeline 介绍

**Performance Timeline** ，在 w3c 中有两个版本的规范，这里基于 **第二版本** 介绍。

**在一个 web 页面的生命周期之内**，基于 Performance Timeline 可以得到哪些性能指标，主要分为以下三大类：

- **navigation-timing**：navigation of the document
- **resource-timing**：页面资源
- **user-timing**：开发者自定义的一些监控，主要是 mark 和 measure

如下示例：

```html
<img id="image0" src="https://www.w3.org/Icons/w3c_main.png" />

<script>
function init() {
  performance.mark("startWork");
  setTimeout(() => {
    performance.mark("endWork");
    measurePerf();
  }, 2000);
}
function measurePerf() {
  performance
    .getEntries()
    .map(entry => JSON.stringify(entry, null, 2))
    .forEach(json => console.log(json));
}
</script>
```

引入了一张 外部图片，mark 可以理解为标记了一个时间点，getEntries 得到所有的性能数据，最后输出：

```json
{ "name": "", "entryType": "navigation", "startTime": 0, "duration": 50.07500003557652 }
{ "name": "https://www.w3.org/Icons/w3c_main.png", "entryType": "resource" }
{ "name": "startWork", "entryType": "mark", "startTime": 49.990000028628856, "duration": 0 }
{ "name": "first-paint", "entryType": "paint", "startTime": 94.83499999623746, "duration": 0 }
{ "name": "first-contentful-paint", "entryType": "paint", "startTime": 94.83499999623746, "duration": 0 }
{ "name": "endWork", "entryType": "mark", "startTime": 2050.5150000099093, "duration": 0 }
```

> **注：**上面的打印经过了压缩，实际打印结果自行运行）

注意其中，entryType 有  navigation、resource、mark、paint（其中可见：name 有 first-paint ( FP ) 和 first-contentful-paint ( FCP ) ）。另外，FP 表示：<font color=FF0000>页面上 **第一个像素落点** 的时候</font>；FCP 表示：<font color=FF0000>页面上 **开始有内容绘制** 的时候</font>

另外，这里的打印只是 “当时得到的性能指标”。如果想要 **持续的** 获得 “性能指标”，可以通过 *定时器* 进行 “轮询”，不过太低效；好的解决方式是：<font color=FF0000>**使用 PerformanceObserver**</font>。它是浏览器内部对 Performance 实现的 观察者模式，<font color=FF0000>当有性能数据产生时，会主动通知</font>

##### PerformanceObserver 监听 first-paint、first-contentful-paint 示例

```js
const observer = new PerformanceObserver(list => {
    list.getEntries().forEach(entry => {
        console.log('entry对象', entry);
    });
});

// 观察的类型
observer.observe({ entryTypes: ['paint'] });
```

关于 entryTypes，可以取如下值：

- frame：event-loop 时的每一帧
- navigation：导航
- resource：资源
- mark：打点，得到一个时间戳
- measure：在两个 mark 产生的点之间的测量
- paint：绘制
- longtask：（好像只有 chrome 支持）任何在浏览器中执行超过 50 ms 的任务，都是 long task

##### navigation entry 对象里能拿到相关的数据

![img](https://s2.loli.net/2022/05/26/gNJI6i1hBrdA2Y3.png)

> 原图片来自：https://www.w3.org/TR/navigation-timing-2/#processing-model

上图完整地描述了一个 ***页面*** 呈现的完整流程。 拿到每个时间点可以进行分析每个区间的时间耗费。

```js
let t = entry

console.log('DNS 查询耗时 ：' + ( t.domainLookupEnd - t.domainLookupStart).toFixed(0) )
console.log('TCP 链接耗时 ：' + ( t.connectEnd - t.connectStart).toFixed(0) )
console.log('request 请求耗时 ：' + ( t.responseEnd - t.responseStart).toFixed(0) )
console.log('解析 dom 树耗时 ：' + ( t.domComplete - t.domInteractive).toFixed(0) )
console.log('白屏时间 ：' + ( t.responseStart - t.navigationStart).toFixed(0) )
console.log('domready 时间 ：' + ( t.domContentLoadedEventEnd - t.navigationStart).toFixed(0) )
console.log('onload 时间 ：' + ( t.loadEventEnd - t.navigationStart).toFixed(0) )
```

##### resource entry 对象里能拿到相关的数据

<img src="https://s2.loli.net/2022/05/26/v2ehLcIqB6z8VKQ.png" alt="img" style="zoom:55%;" />

摘自：[你的页面为什么慢，Performance Timeline 简介](https://juejin.cn/post/6844904020109164552)





#### JS 表达式

如果一个“字符串”可以被赋值给一个变量（类似于“右值”），便可以称为 ***JS 表达式***



#### 栈内存和堆内存

对于JS中的基本数据类型，如 String、Number、Boolean、Undefined、Null 是存在于栈内存中的，在栈内存中储存变量名及相应的值。而Object、Array、Function 存在于堆内存中，在堆内存中储存变量名及引用位置。

> **注：**这部分的内容可以参考 [[JS 机制与原理#JS 中变量的存储]] 以及 [[JS 机制与原理#常见垃圾回收算法与 JavaScript 中垃圾回收原理#JavaScript 垃圾回收]] 中的相关内容。



#### js判断数组中是否存在某值

- **array.indexOf(el)：**判断数组中是否存在某个值，如果存在返回数组元素的下标，否则返回-1

- **array.includes(searchElement[, startIndex])：**判断一个数组是否包含一个指定的值，如果存在返回 true，否则返回false。
  
  **参数如下：**
  
  - **searchElement：**需要查找的元素值
  - **startIndex：**从该索引处开始查找 searchElement。<font color=FF0000>如果为负值，则按升序从 array.length + startIndex 的索引开始搜索</font>。默认为 0

- **array.find(callback[, thisArg])：**<font color=FF0000>返回数组中满足条件的第一个元素的值</font>，如果没有，返回undefined
  
  **参数如下：**
  
  - **callback**
    - element 当前遍历到的元素。
    - index 当前遍历到的索引。
    - array 数组本身
  - **thisArg**（可选）：指定 callback 的 this 参数

- **array.findIndex(callback[, thisArg])：**<font color=FF0000>返回数组中满足条件的第一个元素的索引（下标）, 如果没有找到，返回-1</font>
  
  **参数如下：**
  
  - **callback**
    - element 当前遍历到的元素。
    - index 当前遍历到的索引。
    - array 数组本身。
  - **thisArg（可选）：**指定 callback 的 this 参数。

摘自：[js判断数组中是否存在某个值](https://segmentfault.com/a/1190000014202195)



#### Blob

<font color=FF0000>**Blob 对象表示一个不可变、原始数据的类文件对象**</font>。<font color=FF0000>它的数据可以按 **文本** 或 **二进制** 的格式进行 **读取**，也可以转换成 **ReadableStream** 来用于数据 **操作**</font>。 

Blob 表示的不一定是 JavaScript 原生格式的数据。<font color=FF0000>**File 接口基于Blob**，继承了 blob 的功能并将其扩展使其支持用户系统上的文件</font>。

要从其他非blob对象和数据构造一个 Blob，请使用 Blob() 构造函数。要创建一个 blob 数据的子集 blob，请使用 slice() 方法（**注：**类似于切片）。要获取用户文件系统上的文件对应的 Blob 对象，请参阅 File 文档。

- **构造函数**

  **Blob(blobParts[, options])**：返回一个新创建的 Blob 对象，其内容由参数中给定的数组串联组成。

  > **参数**
  >
  > - **array**（即上面的 blobParts，这里的 blobParts 必须是数组） <font color=FF0000>是一个由 **ArrayBuffer、ArrayBufferView、Blob、DOMString** 等对象构成的 Array</font> ，<font color=FF0000>或者其他类似对象的混合体</font>，它将会被放进 Blob。<font color=FF0000>**DOMStrings 会被编码为 UTF-8**</font>。
  > - **options** 是一个 <font color=FF0000>**可选的 BlobPropertyBag 字典**</font>，它可能会指定如下两个属性：
  >   - **type：**<font color=FF0000>**默认值为 ""**</font>，它代表了将会被放入到 blob 中的<font color=FF0000>**数组内容**</font>（注：即对应了上面的Array） 的 <font color=FF0000 size=4>**MIME 类型**</font>。
  >   - **endings：👎 ** <font color=FF0000>默认值为 "transparent"，用于指定包含行结束符\n的字符串如何被写入</font>。 它是以下两个值中的一个："native"，代表行结束符会被更改为适合宿主操作系统文件系统的换行符，或者 "transparent"，代表会保持blob中保存的结束符不变
  >
  > 摘自：[MDN - Blob()](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob/Blob)

- **属性：**

  - **Blob.size：**<font color=FF0000>只读</font>，Blob 对象中所包含数据的大小（字节）。
  - **Blob.type：**<font color=FF0000>只读</font>，一个字符串，<font color=FF0000>表明该 Blob 对象所包含数据的 MIME 类型</font>。如果类型未知，则该值为空字符串。

- **方法：**

  - **Blob.slice([start[, end[, contentType]]])**：返回一个新的 Blob 对象，包含了源 Blob 对象中指定范围内的数据。

    > **参数**
    >
    > - **start：**可选，这个参数代表 Blob 里的下标，表示第一个会被会被拷贝进新的 Blob 的字节的起始位置。如果你传入的是一个负数，那么这个偏移量将会从数据的末尾从后到前开始计算。举例来说，-10 将会是  Blob 的倒数第十个字节。它的<font color=FF0000>**默认值是0**</font>， <mark>如果你传入的start的长度大于源 Blob 的长度，那么返回的将会是一个长度为 0 并且不包含任何数据的一个 Blob 对象</mark>
    > - **end：**可选，这个参数代表的是 Blob 的一个下标，这个下标 -1 的对应的字节将会是被拷贝进新的Blob 的最后一个字节。如果你传入了一个负数，那么这个偏移量将会从数据的末尾从后到前开始计算。举例来说， -10 将会是 Blob 的倒数第十个字节。它的<font color=FF0000>默认值就是它的原始长度 ( size )</font>
    > - **contentType：**可选，<font color=FF0000>**给新的 Blob 赋予一个新的文档类型**</font>。这将会把它的 type 属性设为被传入的值。它的<font color=FF0000>默认值是一个空的字符串</font>。
    >
    > 摘自：[MDN - Blob.slice](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob/slice)

  - **Blob.stream()**：<font color=FF0000>返回一个能读取 blob 内容的 **ReadableStream**</font>。

  - **Blob.text()**：<font color=FF0000>返回一个 **Promise**</font> 且<font color=FF0000>包含 blob 所有内容的 **UTF-8 格式**的 USVString</font>。
  - **Blob.arrayBuffer()**：<font color=FF0000>返回一个 **Promise**</font> 且<font color=FF0000>包含 blob 所有内容的**二进制格式**的 ArrayBuffer</font>

摘自：[MDN - Blob](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob) 

##### Blob 在《现代JS教程》的补充

> Blob 由一个可选的字符串 type（通常是 MIME 类型）和 blobParts 组成 —— 一系列其他 Blob 对象，字符串和 BufferSource。
>
> ![](https://zh.javascript.info/article/blob/blob.svg)
>
> 摘自：[现代JS教程 - Blob](https://zh.javascript.info/blob)



#### FileReader

<font color=FF0000>FileReader 对象</font>允许Web应用程序异步读取存储在用户计算机上的文件（或原始数据缓冲区）的内容，<font color=FF0000>使用 File 或 Blob 对象指定要读取的文件或数据</font>。
其中File对象<font color=FF0000>可以是来自用户在一个`<input>`元素上选择文件后返回的FileList对象</font>，也可以来自拖放操作生成的 DataTransfer对象，还可以是来自在一个HTMLCanvasElement上执行mozGetAsFile()方法后返回结果。

**重要提示：** <mark>FileReader仅用于以安全的方式从用户（远程）系统读取文件内容，它不能用于从文件系统中按路径名简单地读取文件。 <font color=FF0000>要在JavaScript中按路径名读取文件，应使用标准Ajax解决方案进行服务器端文件读取，如果读取跨域，则使用CORS权限。</font></mark>

**构造函数**

- **FileReader()：**返回一个新构造的FileReader。

有关详细信息和示例，请参阅[如何在web应用程序中使用文件](https://developer.mozilla.org/zh-CN/Using_files_from_web_applications)。

**属性**

- **FileReader.error：** 只读，一个DOMException，表示在读取文件时发生的错误 。

- <font color=FF0000>**FileReader.readyState：** </font>只读，<font color=FF0000>表示FileReader状态的数字</font>。取值如下：
  
  | 常量名     | 值   | 描述          |
  | ------- | --- | ----------- |
  | EMPTY   | 0   | 还没有加载任何数据.  |
  | LOADING | 1   | 数据正在被加载.    |
  | DONE    | 2   | 已完成全部的读取请求. |

- <font color=FF0000>**FileReader.result：**</font>只读，<font color=FF0000>文件的内容</font>。该属性仅在读取操作完成后才有效，数据的格式取决于使用哪个方法来启动读取操作。

**事件处理**

- FileReader.onabort：处理abort事件。该事件在读取操作被中断时触发。
- FileReader.onerror：处理error事件。该事件在读取操作发生错误时触发。
- FileReader.onload：<font color=FF0000>处理load事件</font>。该事件<font color=FF0000>在读取操作完成时触发</font>。
- FileReader.onloadstart：<font color=FF0000>处理loadstart事件</font>。该事件<font color=FF0000>在读取操作开始时触发</font>。
- FileReader.onloadend：处理loadend事件。该事件在读取操作结束时（要么成功，要么失败）触发。
- FileReader.onprogress：处理progress事件。该事件在读取Blob时触发。

**方法**

- FileReader.abort()：中止读取操作。在返回时，readyState属性为DONE。
- FileReader.readAsArrayBuffer()：开始读取指定的 Blob中的内容, 一旦完成, result 属性中保存的将是被读取文件的 ArrayBuffer 数据对象.
- FileReader.readAsBinaryString()：开始读取指定的Blob中的内容。一旦完成，result属性中将包含所读取文件的原始二进制数据。
- FileReader.readAsDataURL()：开始读取指定的Blob中的内容。一旦完成，result属性中将包含一个data: URL格式的Base64字符串以表示所读取文件的内容。
- FileReader.readAsText()：开始读取指定的Blob中的内容。一旦完成，result属性中将包含一个字符串以表示所读取的文件内容。

摘自：[MDN - FileReader](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader)



#### URL

<font color=FF0000>**URL接口 用于解析，构造，规范化和编码 URLs**（统一资源定位器）</font>。 它通过<mark>提供允许您轻松阅读和修改URL组件的属性</mark>来工作。 <mark>通常，通过在调用URL的构造函数时将URL指定为字符串或提供相对URL和基本URL来创建新的URL对象</mark>。 然后，您可以轻松读取URL的已解析组成部分或对URL进行更改。

<font color=FF0000>**如果浏览器尚不支持URL()构造函数**</font>，则可以使用Window中的<font color=FF0000>Window.URL</font>属性。 确保检查您的任何目标浏览器是否要求对此添加前缀。

- **构造器**
  **new URL()：**创建并返回一个URL对象，该URL对象引用使用绝对URL字符串，相对URL字符串和基本URL字符串指定的URL。
  
- **属性**
  - **hash：**<font color=FF0000>包含'#'的USVString</font>，后跟URL的片段标识符。
  - **host：**一个USVString，其中包含域（即主机名），后跟（如果指定了端口）“：”和URL的端口。
  - **hostname：**包含 URL 域名的 USVString。
  - **href：**包含完整 URL 的 USVString。
  - **origin：**只读，<mark>返回一个包含协议名、域名和端口号</mark>的 USVString。
  - **password：**包含在域名前面指定的密码的  USVString 。
  - **pathname：**以 '/' 起头紧跟着 URL 文件路径的 DOMString。
  - **port：**<mark>包含 URL 端口号的 USVString</mark>。
  - **protocol：**包含 URL 协议名的 USVString，<font color=FF0000>末尾带 ':'</font>。
  - **search：**一个USVString ，指示URL的参数字符串； 如果提供了任何参数，则<font color=FF0000>此字符串包括所有参数，并**以开头的“？”开头 字符**</font>
  - <font color=FF0000>**searchParams：**</font>只读，
    - <font color=FF0000>**URLSearchParams：**</font>对象，可用于访问search中找到的各个查询参数。
  - **username：**包含在域名前面指定的用户名的 USVString。
  
- **方法**
  - **toString()：**返回包含整个 URL 的 USVString。 它是 URL.href 的同义词，尽管它不能用于修改值。
  - **toJSON()：**返回包含整个 URL 的 USVString。 它返回与 href 属性相同的字符串。
  
- **静态方法**
  
  - **createObjectURL()：**返回一个 DOMString ，包含一个唯一的 blob 链接（该链接协议为以 `blob:`，后跟唯一标识浏览器中的对象的掩码）。
  
    > URL.createObjectURL() 静态方法 <font color=FF0000>会创建一个 DOMString</font>，<font color=FF0000>其中包含一个表示参数中给出的对象的 URL</font>。**这个 URL 的<font color=FF0000>生命周期和创建它的窗口中的 document 绑定</font>**。这个新的 URL 对象表示指定的 File 对象或 Blob 对象。
    >
    > **注意⚠️：**此特性<font color=FF0000>在 Web Worker 中可用</font>。<font color=FF0000>在 Service Worker 中**不可用**</font>，因为它有可能导致内存泄漏。
    >
    > **语法：**
    >
    > ```js
    > objectURL = URL.createObjectURL(object);
    > ```
    >
    > - **参数：**
    >
    >   **object：**<font color=FF0000>用于创建 URL 的 File 对象、Blob 对象或者 MediaSource 对象</font>。
    >
    > - **返回值：**一个 DOMString 包含了一个对象 URL，该 URL 可用于指定源 object 的内容。
    >
    > **附注**
    >
    > - **内存管理：**在每次调用 createObjectURL() 方法时，都会创建一个新的 URL 对象，即使你已经用相同的对象作为参数创建过。<font color=FF0000>当不再需要这些 URL 对象时，每个对象必须通过调用 URL.revokeObjectURL() 方法来释放</font>。<font color=FF0000>浏览器在 document 卸载的时候，会自动释放它们</font>；**但为了获得最佳性能和内存使用状况，你应该在安全的时机主动释放掉它们**
    > - 使用相对URLs 作为媒体流：在旧版本的媒体资源规范中，添加流作为 \<video> 元素需要创建一个关于 MediaStream的对象URL。<mark>现已没必要这样做了，浏览器已经移除了该操作的支持</mark>。
    >
    > 补充内容摘自：[MDN - URL.createObjectURL()](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/createObjectURL)
  
    **revokeObjectURL()：**销毁之前使用 URL.createObjectURL() 方法创建的URL实例。
  
    > URL.revokeObjectURL() 静态方法用来<font color=FF0000>释放一个之前已经存在的、通过调用 URL.createObjectURL() 创建的 URL 对象</font>。当你结束使用某个 URL 对象之后，应该通过调用这个方法来让浏览器知道不用在内存中继续保留对这个文件的引用了。
    >
    > **语法：**
    >
    > ```js
    > window.URL.revokeObjectURL(objectURL);
    > ```
    >
    > - **参数：**
    >   **objectURL：**一个 DOMString，表示通过调用 URL.createObjectURL() 方法产生的 URL 对象。
    > - **返回值：**undefined
    >
    > 内容摘自：[MDN - URL.revokeObjectURL()](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/revokeObjectURL)

摘自：[MDN - URL](https://developer.mozilla.org/zh-CN/docs/Web/API/URL)

#### URLSearchParams

URLSearchParams <font color=FF0000>**接口** **定义了一些实用的方法**来处理 URL 的查询字符串</font>。

一个实现了 URLSearchParams 的对象可以直接用在 for...of 结构中，例如下面两行是相等的：

```js
for (const [key, value] of mySearchParams) {}
for (const [key, value] of mySearchParams.entries()) {}
```

注意：此特性在 Web Worker 中可用

- **构造函数**

  **URLSearchParams()：**返回一个 URLSearchParams 对象。

- **方法**
  该接口不继承任何属性。

  - **URLSearchParams.append()：**插入一个指定的键/值对作为新的搜索参数。
  - **URLSearchParams.delete()：**从搜索参数列表里删除指定的搜索参数及其对应的值。
  - **URLSearchParams.entries()：**返回一个iterator可以遍历所有键/值对的对象。
  - **URLSearchParams.get()：**获取指定搜索参数的第一个值。
  - **URLSearchParams.getAll()：**获取指定搜索参数的所有值，返回是一个数组。
  - **URLSearchParams.has()：**返回 Boolean 判断是否存在此搜索参数。
  - **URLSearchParams.keys()：**返回iterator 此对象包含了键/值对的所有键名。
  - **URLSearchParams.set()：**设置一个搜索参数的新值，假如原来有多个值将删除其他所有的值。
  - **URLSearchParams.sort()：**按键名排序。
  - **URLSearchParams.toString()：**返回搜索参数组成的字符串，可直接使用在URL上。
  - **URLSearchParams.values()：**返回iterator 此对象包含了键/值对的所有值。

摘自：[MDN - URLSearchParams](https://developer.mozilla.org/zh-CN/docs/Web/API/URLSearchParams)

#### URL.searchParams

URL 接口的 searchParams 属性<font color=FF0000>返回一个 URLSearchParams 对象</font>，<font color=FF0000>这个对象包含当前 URL 中解码后的 GET 查询参数</font>。

- **语法**

  ```js
  const urlSearchParams = url.searchParams
  ```

- **属性值：**一个 URLSearchParams 对象。

- 例子
  如果你的 URL 是 `https://example.com/?name=Jonathan&age=18` ，你可以这样解析 URL，然后得到 name 和 age 的值。

  ```js
  let params = (new URL(document.location)).searchParams;
  let name = params.get('name'); // is the string "Jonathan Smith".
  let age = parseInt(params.get('age')); // is the number 18
  ```

摘自：[MDN - URL.searchParams](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/searchParams)

#### encodeURI()

encodeURI()  函数通过将特定字符的每个实例替换为一个、两个、三或四转义序列来对统一资源标识符 (URI) 进行编码 (该字符的 UTF-8 编码仅为四转义序列)由两个 "代理" 字符组成)。

- **语法**

  ```js
  encodeURI(URI)
  ```

- **参数**

  - **URI：**一个完整的URI.

- **返回值：** 一个新字符串, 表示提供的字符串编码为统一资源标识符 (URI)。

- **描述**
  假定一个URI是完整的URI，那么无需对那些保留的并且在URI中有特殊意思的字符进行编码。

  ```
  http://username:password@www.example.com:80/path/to/file.php?foo=316&bar=this+has+spaces#anchor
  ```

  <font color=FF0000>encodeURI 会替换所有的字符</font>，<font color=FF0000>但不包括以下字符</font>，即使它们具有适当的UTF-8转义序列：

  | 类型         | 包含                                          |
  | ------------ | --------------------------------------------- |
  | 保留字符     | `;` `,` `/` `?` `:` `@` `&` `=` `+` `$`       |
  | 非转义的字符 | 字母 数字 `-` `_` `.` `!` `~` `*` `'` `(` `)` |
  | 数字符号     | `#`                                           |

  请注意，<font color=FF0000>encodeURI 自身无法产生能适用于HTTP GET 或 POST 请求的URI</font>，例如对于 XMLHTTPRequests，<font color=FF0000>因为 "&", "+", 和 "=" 不会被编码，然而在 GET 和 POST 请求中它们是特殊字符</font>。然而<font color=FF0000>encodeURIComponent这个方法会对这些字符编码</font>。

  另外，<font color=FF0000>**如果试图编码一个非高-低位完整的代理字符，将会抛出一个 URIError 错误**</font>，例如：

  ```js
  // 编码高-低位完整字符 ok
  console.log(encodeURI('\uD800\uDFFF'));
  
  // 编码单独的高位字符抛出 "Uncaught URIError: URI malformed"
  console.log(encodeURI('\uD800'));
  
  // 编码单独的低位字符抛出 "Uncaught URIError: URI malformed"
  console.log(encodeURI('\uDFFF'));
  ```

  并且需要注意，如果URL需要遵循较新的RFC3986标准，那么方括号是被保留的(给IPv6)，因此对于那些没有被编码的URL部分(例如主机)，可以使用下面的代码：

  ```js
  function fixedEncodeURI (str) {
      return encodeURI(str).replace(/%5B/g, '[').replace(/%5D/g, ']');
  }
  ```

摘自：[MDN - encodeURI()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURI)

#### encodeURIComponent()

encodeURIComponent()函数通过将一个，两个，三个或四个表示字符的UTF-8编码的转义序列替换某些字符的每个实例来编码 URI （对于由两个“代理”字符组成的字符而言，将仅是四个转义序列） 。

- **语法**

  ```js
  encodeURIComponent(str);
  ```

- **参数**

  - str：String. URI 的组成部分。

- **返回值：**原字串作为URI组成部分被被编码后的新字符串。

- **描述**
  <font color=FF0000>encodeURIComponent 转义除了如下所示外的所有字符</font>：A-Z a-z 0-9 - _ . ! ~ * ' ( )

摘自：[MDN - encodeURIComponent()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent)

#### decodeURI()

decodeURI() 函数能解码由encodeURI 创建或其它流程得到的统一资源标识符（URI）。

- **语法**

  ```js
  decodeURI(encodedURI)
  ```

- **参数**

  - **encodedURI：**一个完整的编码过的 URI

- **返回值：**返回一个给定编码统一资源标识符(URI)的未编码版本的新字符串。

- **异常：**当encodedURI 包含无效字符序列时，引发URIError（“格式错误的URI序列”）异常。

- **描述：**将已编码 URI 中所有能识别的转义序列转换成原字符，但不能解码那些不会被 encodeURI 编码的内容（例如 "#"）。

摘自：[MDN - decodeURI()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/decodeURI)

#### decodeURIComponent()

decodeURIComponent() 方法<font color=FF0000>用于解码由 encodeURIComponent 方法 或者 其它类似方法编码的部分统一资源标识符（URI）</font>。

- **语法**

  ```js
  decodeURIComponent(encodedURI)
  ```

- **参数**

  - encodedURI：编码后的部分 URI

- **返回值：**<font color=FF0000>一个解码后的统一资源标识符（URI）字符串</font>，处理前的URI经过了给定格式的编码。

- **异常：**<font color=FF0000>当该方法使用不当时，将会抛出一个URIError（“格式错误的URI序列”）异常</font>。

- **描述：**<font color=FF0000>**将已编码 URI 中所有能识别的<font size=4>转义序列转换成原字符</font>**</font>。

- **示例**

  - 解码一个西里尔字母的URL

    ```js
    decodeURIComponent("JavaScript_%D1%88%D0%B5%D0%BB%D0%BB%D1%8B");
    // "JavaScript_шеллы"
    ```

  - 捕捉异常

    ```js
    try {
      var a = decodeURIComponent('%E0%A4%A');
    } catch(e) {
      console.error(e);
    }
    // URIError: malformed URI sequence
    ```

  摘自：[MDN - decodeURIComponent()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/decodeURIComponent)

#### TextDecoder



#### TextEncoder

TextEncoder <font color=FF0000>接受代码点流作为输入</font>，<font color=FF0000>并提供 UTF-8 字节流作为输出</font>。

**例子**

```js
const encoder = new TextEncoder()
const view = encoder.encode('€')
console.log(view); // Uint8Array(3) [226, 130, 172]
```

- **构造器**

  - **TextEncoder()：**返回一个新构造的 TextEncoder，<font color=FF0000>它默认使用 UTF-8 编码将代码点流转换成字节<流</font>。

- **属性**

  TextEncoder 接口不继承任何属性。

  - **TextEncoder.encoding：**<font color=FF0000>**只读**</font>，<font color=FF0000>**总是**返回 "utf-8"</font>。

- **方法**
  TextEncoder 接口不继承任何方法。
  - **TextEncoder.encode()：**接受一个 USVString 作为输入，<font color=FF0000>返回一个包含文本的 **Uint8Array**，其中的文本使用 UTF-8 编码</font>。
  - **TextEncoder.prototype.encodeInto()：**接受一个 USVString 作为输入、一个Uint8Array 作为输出目标，<font color=FF0000>**返回一个指示编码进度的目录（dictionary）对象**</font>。此方法的性能可能会比更早出现的 encode() 方法好一些。

摘自：[MDN - TextEncoder](https://developer.mozilla.org/zh-CN/docs/Web/API/TextEncoder)

#### TextDecoder

TextDecoder 接口<font color=FF0000>表示一个文本解码器</font>，<font color=FF0000>**一个解码器只支持一种特定文本编码**</font>，例如 utf-8、iso-8859-2、koi8、cp1261，gbk 等等。解码器将字节流作为输入，并提供代码点流作为输出。

**构造函数**

- **TextDecoder()：**返回一个新构造的 TextDecoder，它使用参数中指定的解码方法生成代码点流。

  **语法：**

  ```js
  decoder = new TextDecoder(utfLabel, options);
  ```

  **参数：**

  - utfLabel：选填，是一个DOMString包含编码名称，默认为utf-8；可以是任意合法的编码名称
  - options：选填，是带有属性的TextDecoderOptions字典
    - fatal：一个布尔标志，表明 TextDecoder.decode() 方法在发现编码错误时是否必须抛出带有“EncodingError”值的 DOMException。 默认为false。

  摘自：[MDN - TextDecoder()](https://developer.mozilla.org/en-US/docs/Web/API/TextDecoder/TextDecoder)

**属性**

- **TextDecoder：**接口不继承任何属性。
- **TextDecoder.prototype.encoding：**只读，<font color=FF0000>DOMString所包含的解码器的名称</font>，表示TextDecoder所使用的解码方法的字符串。
- **TextDecoder.prototype.fatal：**只读，布尔值，Boolean，<font color=FF0000>是否显示致命错误</font>。
- **TextDecoder.prototype.ignoreBOM：**只读，布尔值，Boolean，<font color=FF0000>是否忽略 BOM（byte order marker）标记</font>。

**方法：**TextDecoder 接口不继承任何方法。

- **TextDecoder.prototype.decode()：**<font color=FF0000>返回一个DOMString，其中包含使用特定 TextDecoder 对象的方法解码的文本</font>。

摘自：[MDN - TextDecoder](https://developer.mozilla.org/zh-CN/docs/Web/API/TextDecoder)



#### Event.target

触发事件的对象 (某个DOM元素) 的引用。当事件处理程序在事件的冒泡或捕获阶段被调用时，它与event.currentTarget不同。

摘自：[MDN - Event.target](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/target)

#### Event.currentTarget

Event 接口的只读属性 currentTarget 表示的，<font color=FF0000 size=4> **标识是当事件沿着 DOM 触发时事件的当前目标** </font>。它总是指向事件绑定的元素，而 Event.target 则是事件触发的元素。

摘自：[MDN - event.currentTarget](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/currentTarget)

#### 两者对比示例
##### Event.target 示例
```html
<div id="cntr">
  <div id="wrapper">
    <button id="btn">点击</button>
  </div>
</div>

<script>
  cntr.addEventListener('click', (evt) => { console.log(evt.target) }) // <button id="btn">点击</button>
  wrapper.addEventListener('click', (evt) => { console.log(evt.target) }) // <button id="btn">点击</button>
  btn.addEventListener('click', (evt) => { console.log(evt.target) }) // <button id="btn">点击</button>
</script>
```

##### Event.currentTarget 示例
```html
<div id="cntr">
  <div id="wrapper">
    <button id="btn">点击</button>
  </div>
</div>

<script>
  cntr.addEventListener('click', (evt) => { console.log(evt.currentTarget) })
  // <div id="cntr"><div id="wrapper"><button id="btn">点击</button></div></div>
  wrapper.addEventListener('click', (evt) => { console.log(evt.currentTarget) }) 
  // <div id="wrapper"><button id="btn">点击</button></div>
  btn.addEventListener('click', (evt) => { console.log(evt.currentTarget) }) // <button id="btn">点击</button>
</script>
```



#### JS获取div高度

对于一个元素而言，<font color=FF0000>默认的style对象是没有高度的（style.height=""）。可以说style是它的外部属性，但一个对象应该有一个高度（自身属性）</font>。补充：style属性只能获取元素标签 style 属性里的值

如果这时想要获取该元素的高度，可以使用如下方法

**有两种方法：**

- offsetHeight 、clientHeight

- getComputedStyle

**offsetHeight 和 clientHeight 的区别：**

<font color=FF0000>offsetHeight的值包括 元素内容 + 内边距 + 边框</font>，而 <font color=FF0000>clientHeight 的值等于 元素内容 + 内边距</font>。区别就在于有没有边框~

**getComputedStyle**

<font color=FF0000>getComputedStyle 方法获取的是最终应用在元素上的所有 CSS 属性对象（即使没有 CSS 代码，也会把默认的祖宗八代都显示出来）</font>；这和<font color=FF0000> `style` 属性只能获取**内联样式**的行为形成了鲜明的对比</font>。除此之外，<font color=FF0000>**getComputedStyle 是只读的**</font>，但是 style 能文能武，可读可写，我们也可以利用它动态设置元素的高度。示例如下：

```js
window.getComputedStyle(div);
```

如果想要高度，可以使用 getPropertyValue 方法来帮忙 getPropertyValue 方法可以获取 CSS 样式申明对象上的属性值。示例如下：

```js
console.log(window.getComputedStyle(div).getPropertyValue('height')); // 200px
```

摘自：[JS获取div高度的方法](https://www.jianshu.com/p/58c12245c2cc)

**补充：Window.getComputedStyle() **

Window.getComputedStyle() 方法<font color=FF0000>返回一个对象</font>，<font color=FF0000>该对象在应用活动样式表并解析这些值可能包含的任何基本计算后报告元素的所有 CSS 属性的值</font>。 私有的 CSS 属性值可以通过对象提供的 API 或通过简单地使用 CSS 属性名称进行索引来访问。

**语法**

```js
let style = window.getComputedStyle(element, [pseudoElt]);
```

- **element：**用于获取计算样式的 Element。
- **pseudoElt：** 可选，指定一个要匹配的伪元素的字符串。必须对普通元素省略（或 null ）。

返回的 style 是一个实时的 CSSStyleDeclaration 对象，当元素的样式更改时，它会自动更新本身。

从 getComputedStyle 返回的对象是只读的，可以用于检查元素的样式

摘自：[MDN - Window.getComputedStyle()](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/getComputedStyle)



#### Object.create()

Object.create() 方法<font color=FF0000>创建一个新对象</font>，<font color=FF0000>**使用现有的对象来提供新创建的对象的 \__proto__**</font>

**示例如下：**

```js
const person = {
  isHuman: false,
  printIntroduction: function() {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  }
};

const me = Object.create(person);

me.name = 'Matthew'; // "name" is a property set on "me", but not on "person"
me.isHuman = true; // inherited properties can be overwritten

me.printIntroduction(); // expected output: "My name is Matthew. Am I human? true"
```

**语法：**

```js
Object.create(proto，[propertiesObject])
```

**参数**

- **proto：**新创建对象的原型对象。

- **propertiesObject：**可选。需要传入一个对象（注意：也可以是多个对象），<font color=FF0000>该对象的属性类型参照 **Object.defineProperties()** 的第二个参数</font>。如果该参数被指定且不为 undefined，该传入对象的自有可枚举属性(即其自身定义的属性，而不是其原型链上的枚举属性)将为新创建的对象添加指定的属性值和对应的属性描述符。

  相关示例参见：[MDN - Object.create - 使用 Object.create 的 propertyObject参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create#使用_object.create_的_propertyobject参数) 另外，关于Object.create()的最后也有摘录部分

**返回值：**一个新对象，带着指定的原型对象和属性。

**注意：**<font color=FF0000>如果proto参数不是 null 或非原始包装对象，则抛出一个 TypeError 异常</font>。

**示例：用 Object.create实现类式继承**，演示如何使用Object.create() 来实现类式继承。这是一个所有版本JavaScript都支持的单继承。

```js
// Shape - 父类(superclass)
function Shape() {
  this.x = 0;
  this.y = 0;
}

// 父类的方法
Shape.prototype.move = function(x, y) {
  this.x += x;
  this.y += y;
  console.info('Shape moved.');
};

// Rectangle - 子类(subclass)
function Rectangle() {
  Shape.call(this); // call super constructor. 这里的作用是调用Shape的构造函数，获取x和y这两个成员属性；以备下面调用
}

// 子类继承父类
Rectangle.prototype = Object.create(Shape.prototype);
Rectangle.prototype.constructor = Rectangle; // 上面使用call调用Shape的构造函数，在这里被赋值

var rect = new Rectangle();

console.log('Is rect an instance of Rectangle?', rect instanceof Rectangle); // true
console.log('Is rect an instance of Shape?', rect instanceof Shape); // true
rect.move(1, 1); // 'Shape moved.'
```

如果你希望能继承到多个对象，则可以使用混入的方式。

```js
function MyClass() {
   SuperClass.call(this);
   OtherSuperClass.call(this);
}

// 继承一个类
MyClass.prototype = Object.create(SuperClass.prototype);
// 混合其它
Object.assign(MyClass.prototype, OtherSuperClass.prototype);
// 重新指定constructor
MyClass.prototype.constructor = MyClass; // 这里的构造函数还是上面使用call创建的

MyClass.prototype.myMethod = function() { // do a thing };
```

<font color=FF0000 size=4>**Object.assign 会把  OtherSuperClass 原型上的函数拷贝到 MyClass原型上**</font>，使 MyClass 的所有实例都可用 OtherSuperClass 的方法。Object.assign 是在 ES2015 引入的，且可用 polyfilled。要支持旧浏览器的话，可用使用 jQuery.extend() 或者 \_.assign()。

**使用 Object.create 的 propertyObject 参数（这里只摘录了一部分）**

```js
var o；

// 自己的话：下面这示例很好的说明了 数据属性 和 访问器 的区别
o = Object.create(Object.prototype, {
  // foo会成为所创建对象的数据属性
  foo: {
    writable:true,
    configurable:true,
    value: "hello"
  },
  // bar会成为所创建对象的访问器属性
  bar: {
    configurable: false,
    get: function() { return 10 },
    set: function(value) { console.log("Setting `o.bar` to", value); }
  }
});

function Constructor(){}
o = new Constructor();
// 上面的一句就相当于:
o = Object.create(Constructor.prototype);
// 当然,如果在Constructor函数中有一些初始化代码,Object.create不能执行那些代码
```

摘自：[MDN - Object.create()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

**补充：**

如下示例：

```js
function Foo() {}
const fooCp = Object.create(Foo.prototype)

fooCp.__proto__ === Object.getPrototypeOf(fooCp) === Foo.prototype
//              true                             true
```

> 其实new 是一种语法糖，真正new干了两件事，第一是在构造函数中第一句加上 this = Object.create(Parent.prototype)， 第二件事是最后结束的时候 return this; 你在构造函数里面写这两句，实例化对象的时候就不用new了

参考自：[js继承实现之Object.create](https://segmentfault.com/a/1190000014592412)



#### Object.defineProperty()

**Object.defineProperty()** 方法会<font color=FF0000>直接在一个对象上**定义一个新属性**，或者 <font size=4>**修改一个对象**的现有属性</font> </font>，<font color=FF0000>并返回此对象</font>。

（这是一个静态方法）应当直接在 Object 构造器对象上调用此方法，而不是在任意一个 Object 类型的实例上调用

- **语法：**

  ```js
  Object.defineProperty(obj, prop, descriptor)
  ```

- **参数：**

  - obj：要定义属性的对象。
  - prop：<font color=FF0000>要定义或修改的属性的名称或 Symbol </font>。
  - <font color=FF0000>descriptor：要定义或修改的属性描述符（writable / enumerable / configurable）</font>。

- **返回值：**被传递给函数的对象。

**注意：**在ES6中，由于 Symbol类型的特殊性，用Symbol类型的值来做对象的key与常规的定义或修改不同，而Object.defineProperty 是定义key为Symbol的属性的方法之一。

默认情况下，<font color=FF0000>**使用 Object.defineProperty() 添加的属性值是不可修改（immutable）的（默认writable是false）**</font>。（同时也是在打印中不显示的，因为enumerable 默认为 false）

**使用示例：**

```js
// 在对象中添加一个设置了存取描述符属性的示例
var bValue = 38;
Object.defineProperty(o, "b", {
  // 使用了方法名称缩写（ES2015 特性）
  // 下面两个缩写等价于：
  // get : function() { return bValue; },
  // set : function(newValue) { bValue = newValue; },
  get() { return bValue; },
  set(newValue) { bValue = newValue; },
  enumerable : true,
  configurable : true
});
```

**另外，等价关系：**

```js
var o = {};

o.a = 1;
// 等同于：
Object.defineProperty(o, "a", {
  value: 1,
  writable: true,
  configurable: true,
  enumerable: true
});

// 另一方面，
Object.defineProperty(o, "a", { value : 1 });
// 等同于：
Object.defineProperty(o, "a", {
  value: 1,
  writable: false,
  configurable: false,
  enumerable: false
}); // 因为这里的都是默认值均为 false
```

**属性描述符**

对象里目前存在的属性描述符**有两种主要形式**：<mark>**数据描述符**</mark> 和 <mark style="background:aqua">**存取描述符**</mark>。<mark>数据描述符</mark>是一个具有值的属性，该值可以是可写的，也可以是不可写的。<mark style="background:aqua">存取描述符</mark>是由 getter 函数和 setter 函数所描述的属性。<font color=FF0000>一个描述符只能是这两者其中之一；不能同时是两者。</font>
这两种描述符都是对象。它们共享以下可选键值（默认值是指在使用 Object.defineProperty() 定义属性时的默认值）

- **configurable：**<font color=FF0000>当且仅当该属性的 configurable 键值为 true 时，该属性的**描述符才能够被改变**</font>，同时该属性也能从对应的对象上被删除。<font color=FF0000 size=4>**默认为 false**</font>。(补充：<font color=FF0000>configurable 特性表示对象的属性是否可以被删除</font>，以及除 value 和 writable 特性外的其他特性是否可以被修改。)
- **enumerable：**<font color=FF0000>当且仅当该属性的 enumerable 键值为 true 时，该属性**才会出现在对象的枚举属性中**</font>。<font color=FF0000 size=4>**默认为 false**</font>。(补充：<font color=FF0000>enumerable 定义了对象的属性是否可以在 for...in 循环和 Object.keys() 中被枚举。</font>)

- **value：**该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。<font color=FF0000 size=4>**默认为 undefined**</font>。
- **writable：**<font color=FF0000>当且仅当该属性的 writable 键值为 true 时，**属性的值**，也就是上面的 value，**才能被赋值运算符改变**</font>。<font color=FF0000 size=4>**默认为 false**</font>。

<mark style="background:aqua">存取描述符</mark>还具有以下可选键值：

- **get：**属性的 getter 函数，如果没有 getter，则为 undefined。<font color=FF0000>**当访问该属性时，会调用此函数**</font>。<mark style="background:fuchsia">执行时不传入任何参数，但是会传入 this 对象（由于继承关系，这里的this并不一定是定义该属性的对象）</mark>。该函数的返回值会被用作属性的值。<font color=FF0000>默认为 undefined</font>。
- **set：**属性的 setter 函数，如果没有 setter，则为 undefined。<font color=FF0000>**当属性值被修改时，会调用此函数**</font>。<mark style="background:fuchsia">该方法接受一个参数（也就是被赋予的新值），会传入赋值时的 this 对象</mark>。<font color=FF0000>默认为 undefined</font>。

|            | configurable | enumerable | value  | writable | get    | set    |
| ---------- | ------------ | ---------- | ------ | -------- | ------ | ------ |
| 数据描述符 | 可以         | 可以       | 可以   | 可以     | 不可以 | 不可以 |
| 存取描述符 | 可以         | 可以       | 不可以 | 不可以   | 可以   | 可以   |

如果一个描述符不具有 value、writable、get 和 set 中的任意一个键，那么它将被认为是一个数据描述符。如果一个描述符同时拥有 value 或 writable 和 get 或 set 键，则会产生一个异常。

<font color=FF0000>**以上内容总结：**因为 get set 和 value writable 功能是类似的（重复的，甚至可以说是：冲突的），所以不能同时存在。</font>

**继承属性**

如果访问者的属性是被继承的，它的 get 和 set 方法会在子对象的属性被访问或者修改时被调用。如果这些方法用一个变量存值，该值会被所有对象共享。示例如下：

```js
function myclass() {}

var value;
Object.defineProperty(myclass.prototype, "x", {
  get() { return value; },
  set(x) { value = x; }
});

var a = new myclass();
var b = new myclass();
a.x = 1;
console.log(b.x); // 1
```

如果一个不可写的属性被继承，它仍然可以防止修改对象的属性。

摘自：[MDN - Object.defineProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

#### Object.defineProperties()

**Object.defineProperties()** 方法直接在一个对象上定义新的属性或修改现有属性，并返回该对象。

- **语法**

  ```js
  Object.defineProperties(obj, props)
  ```

- **参数**

  - **obj：**在其上定义或修改属性的对象。
  - **props：**要定义其可枚举属性或修改的属性描述符的对象。对象中存在的属性描述符主要有两种：数据描述符和访问器描述符（更多详情，请参阅Object.defineProperty()）。描述符具有以下键：
    - **configurable：**true 只有该属性描述符的类型可以被改变并且该属性可以从对应对象中删除。默认为 false
    - **enumerable：**true 只有在枚举相应对象上的属性时该属性显现。默认为 false
    - **value：**与属性关联的值。可以是任何有效的JavaScript值（数字，对象，函数等）。默认为 undefined.
    - **writable：**true只有与该属性相关联的值被assignment operator改变时。默认为 false
    - **get：**作为该属性的 getter 函数，如果没有 getter 则为undefined。函数返回值将被用作属性的值。默认为 undefined
    - **set：**作为属性的 setter 函数，如果没有 setter 则为undefined。函数将仅接受参数赋值给该属性的新值。默认为 
    - **undefined**
  - **返回值：**传递给函数的对象。

摘自：[MDN - Object.defineProperties()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties)



#### Object.preventExtensions()

Object.preventExtensions()方法让一个对象变的不可扩展，也就是永远不能再添加新的属性。

- **语法**

  ```js
  Object.preventExtensions(obj)
  ```

- 参数

  obj：将要变得不可扩展的对象。

- **返回值：**已经不可扩展的对象。

- **描述**
  <font color=FF0000>如果一个对象可以添加新的属性，则这个对象是可扩展的</font>。<font color=FF0000>Object.preventExtensions()将对象标记为不再可扩展</font>，这样它将永远不会具有它被标记为不可扩展时持有的属性之外的属性。注意，<font color=FF0000>一般来说，不可扩展对象的属性可能仍然可被删除</font>。<font color=FF0000>**尝试将新属性添加到不可扩展对象将静默失败或抛出TypeError**</font>（最常见的情况是strict mode (en-US)中，但不排除其他情况）。

  Object.preventExtensions()仅阻止添加自身的属性。但其对象类型的原型依然可以添加新的属性。
  该方法使得目标对象的 [[prototype]]  不可变；任何重新赋值 [[prototype]] 操作都会抛出 TypeError 。这种行为只针对内部的 [[prototype]] 属性， 目标对象的其它属性将保持可变。

摘自：[MDN - Object.preventExtensions(obj)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions)

#### Object.isExtensible()

Object.isExtensible() 方法判断一个对象是否是可扩展的（是否可以在它上面添加新的属性）。

- **语法**

  ```js
  Object.isExtensible(obj)
  ```

- **参数**
  obj：需要检测的对象

- **返回值：**表示给定对象是否可扩展的一个Boolean 。

- **描述**
  默认情况下，对象是可扩展的：即可以为他们添加新的属性。以及它们的 __proto__ 属性可以被更改。<font color=FF0000>Object.preventExtensions，Object.seal 或 Object.freeze 方法都可以标记一个对象为不可扩展（non-extensible）</font>。

摘自：[MDN - Object.isExtensible()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/isExtensible)



#### Object.keys()

Object.keys() 方法会<font color=FF0000>**返回一个由一个给定对象的自身可枚举属性组成的数组**</font>，<mark>数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致 </mark>。

**需要注意的示例：**

```js
var arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']
```

在ES5里，如果Object.keys()的参数不是对象（而是一个原始值），那么它会抛出 TypeError。<font color=FF0000>在ES2015中，非对象的参数将被强制转换为一个对象</font>。代码示例如下：

```js
Object.keys("foo"); // TypeError: "foo" is not an object (ES5 code)
Object.keys("foo"); // ["0", "1", "2"] (ES2015 code)
```

摘自：[MDN - Object.keys()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)

<font size=4>**补充：**</font>如果Object里面有其他不属于属性的东西，Object.keys() 不会拿到这些东西。

参考自：[JS现代教程 - LocalStorage，sessionStorage - 遍历键](https://zh.javascript.info/localstorage#bian-li-jian)

#### Object.values()

Object.values()方法<font color=FF0000>返回一个给定对象自身的所有可枚举属性值的数组</font>，值的顺序与使用for...in循环的顺序相同 ( 区别在于 for-in 循环枚举原型链中的属性 )。

**描述：**Object.values()返回一个数组，其元素是在对象上找到的可枚举属性值。属性的顺序与通过手动循环对象的属性值所给出的顺序相同。

**需要注意的示例：**

- ```js
  // when we use numeric keys, the value returned in a numerical order according to the keys
  // 当我们用数字作为键，返回的值（数组）是根据键进行排序的结果
  const an_obj = { 100: 'a', 2: 'b', 7: 'c' };
  console.log(Object.values(an_obj)); // ['b', 'c', 'a']
  ```

- ```js
  console.log(Object.values('foo')); // ['f', 'o', 'o']
  ```

摘自：[MDN - Object.values()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/values)



#### Object.entries()

Object.entries() 方法<font color=FF0000>返回一个给定对象自身<font size=4>**可枚举属性**</font>的<font size=4> **键值对数组** </font></font>，其排列与使用 for...in 循环遍历该对象时返回的顺序一致（区别在于 <font color=FF0000 size=4>**for-in 循环还会枚举原型链中的属性**</font>）。

**需要注意示例如下：**

- ```js
  const obj = { foo: 'bar', baz: 42 };
  console.log(Object.entries(obj)); // [ ['foo', 'bar'], ['baz', 42] ]
  ```

- ```js
  // array like object with random key ordering
  const anObj = { 100: 'a', 2: 'b', 7: 'c' };
  console.log(Object.entries(anObj)); // [ ['2', 'b'], ['7', 'c'], ['100', 'a'] ]
  ```

- ```js
  // non-object argument will be coerced to an object
  console.log(Object.entries('foo')); // [ ['0', 'f'], ['1', 'o'], ['2', 'o'] ]
  ```

**将Object转换为Map**

new Map() 构造函数接受<font color=FF0000>一个可迭代的entries</font>。借助Object.entries方法你可以很容易的将Object转换为Map:

```js
var obj = { foo: "bar", baz: 42 };
var map = new Map(Object.entries(obj));
console.log(map); // Map { foo: "bar", baz: 42 }
```

摘自：[MDN - Object.entries()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)

#### Object.fromEntries()

 Object.fromEntries() 方法<font color=FF0000>把键值对列表转换为一个对象</font>。

- **参数**

  <font color=FF0000>**iterable**</font>：<font color=FF0000>类似 Array 、 Map 或者其它**实现了可迭代协议的可迭代对象**</font>。

- **返回值**
  一个由该迭代对象条目提供对应属性的新对象。
- **描述**
  Object.fromEntries() 方法接收一个键值对的列表参数，并返回一个带有这些键值对的新对象。<mark>这个迭代参数应该<font color=FF0000>是一个能够实现@@iterator方法的的对象</font>，返回一个迭代器对象。它生成一个具有两个元素的类数组的对象，第一个元素是将用作属性键的值，第二个元素是与该属性键关联的值。</mark>

<font color=FF0000>Object.fromEntries() 执行与 Object.entries 互逆的操作。</font>

**示例：**

-  Map 转化为 Object

  ```js
  const map = new Map([ ['foo', 'bar'], ['baz', 42] ]);
  const obj = Object.fromEntries(map);
  console.log(obj); // { foo: "bar", baz: 42 }
  ```

- Array 转化为 Object

  ```js
  const arr = [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ];
  const obj = Object.fromEntries(arr);
  console.log(obj); // { 0: "a", 1: "b", 2: "c" }
  ```

摘自：[MDN - Object.fromEntries()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/fromEntries)



#### Object.assign()

Object.assign() 方法用于<font color=FF0000>将所有可枚举属性的值</font>从一个**或多个源对象**分配到目标对象。它将返回目标对象。

- **语法**

  ```js
  Object.assign(target, ...sources)
  ```

  <font color=FF0000>自我补充：</font>之所以sources前面加了展开运算符，是因为可能有多个源对象

- **参数**
  
  - **target：**目标对象。
  - **sources：**源对象。
  
- **返回值：**目标对象。

- **描述**
  <font color=FF0000 size=4>如果目标对象中的属性具有相同的键，则属性将被源对象中的属性 **覆盖**</font>。后面的源对象的属性将类似地覆盖前面的源对象的属性。

  <font color=FF0000 size=4>**Object.assign 方法只会拷贝源对象自身的并且可枚举的属性到目标对象**</font>。该方法使用源对象的 [[Get]] 和目标对象的 [[Set]]，所以它会调用相关 getter 和 setter。<font color=FF0000>因此，它分配属性，而不仅仅是复制或定义新的属性</font>。<font color=FF0000>如果合并源包含getter，这可能使其不适合将新属性合并到原型中</font>。为了将属性定义（包括其可枚举性）复制到原型，应使用Object.getOwnPropertyDescriptor()和Object.defineProperty() 。

  <font color=FF0000>String类型和 **Symbol 类型**的属性都会被拷贝。</font>

  <font color=FF0000>在出现错误的情况下，例如，**如果属性不可写，会引发TypeError**</font>，如果在引发错误之前添加了任何属性，则可以更改target对象。

  <font color=FF0000>注意，Object.assign **不会** 在那些source对象值为 null 或 undefined 的时候抛出错误</font>。
  
- **深拷贝问题：**

  针对深拷贝，需要使用其他办法，因为 Object.assign()拷贝的是（可枚举）属性值。假如源值是一个对象的引用，它仅仅会复制其引用值。
  
- <font color=FF0000>**继承属性和不可枚举属性是不能拷贝的**</font>

  ```js
  const obj = Object.create({foo: 1}, { // foo 是个继承属性。
      bar: { 
        value: 2  // bar 是个不可枚举属性。
      },
      baz: {
          value: 3,
          enumerable: true  // baz 是个自身可枚举属性。
      }
  });
  
  const copy = Object.assign({}, obj);
  console.log(copy); // { baz: 3 }
  ```

- 原始类型会被包装为对象

  ```js
  const v1 = "abc";
  const v2 = true;
  const v3 = 10;
  const v4 = Symbol("foo")
  
  const obj = Object.assign({}, v1, null, v2, undefined, v3, v4);
  // 原始类型会被包装，null 和 undefined 会被忽略。
  // 注意，只有字符串的包装对象才可能有自身可枚举属性。
  console.log(obj); // { "0": "a", "1": "b", "2": "c" }
  ```


摘自：[MDN - Object.assign()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

补充：如果要实现深拷贝，除了自己手写实现外；还可以使用：

- lodash 的 deepClone
- JSON.parse( JSON.toString() )
- structuredClone() ，**注：**这是一个新特性



#### Object.is()

Object.is() 方法判断两个值是否为同一个值。

- **语法**

  ```js
  Object.is(value1, value2);
  ```

  - **参数**
    - value1：被比较的第一个值。
    - value2：被比较的第二个值。

  - **返回值：**一个 Boolean 类型标示两个参数是否是同一个值。

- **描述**

  Object.is() 方法判断两个值是否为同一个值。<font color=FF0000>如果满足以下条件则两个值相等</font>：

  - 都是 undefined<font color=FF0000>（undefined === undefined 本身就成立）</font>
  - 都是 null <font color=FF0000>（null === null 本身就成立）</font>
  - 都是 true 或 false
  - 都是相同长度的字符串且相同字符按相同顺序排列
  - 都是相同对象（意味着每个对象有同一个引用），<font color=FF0000>注：这里两个本来也全等( === )</font>
  - 都是数字且
    - 都是 +0
    - 都是 -0
    - <font color=FF0000>都是 NaN（注：NaN != NaN，但是Object.is(NaN, NaN) == true ）</font>
    - 或都是非零而且非 NaN 且为同一个值

  与== 运算不同。  <font color=FF0000>== 运算符在判断相等前对两边的变量（如果它们不是同一类型）进行强制转换（这种行为的结果会将 "" == false 判断为 true）</font>， <font color=FF0000>而 **Object.is不会强制转换两边的值**</font>。

  与\=== 运算也不相同， <font color=FF0000>\=== 运算符 (也包括 == 运算符) 将数字 -0 和 +0 视为相等（ **注：根据上面的判断规则，Object.is(+0, =0) 是 false** ），而将Number.NaN 与NaN视为不相等</font>

摘自：[MDN - Object.is()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/is)



#### Object.prototype.valueOf()

valueOf() 方法返回指定对象的原始值。

- **语法：**

  ```js
  object.valueOf()
  ```

- **返回值：**返回值为该对象的原始值。

- **描述：**
  <font color=FF0000>JavaScript 调用 valueOf 方法将对象转换为原始值</font>。你<mark>很少需要自己调用 valueOf 方法</mark>：当遇到要预期的原始值的对象时，<font color=FF0000 size=4>**JavaScript 会自动调用它**</font>。

  默认情况下，valueOf 方法由 Object 后面的每个对象继承（注：这里的说法有点问题，应该是 <font color=FF0000>继承Object 的对象，会继承 valueOf 方法</font>。原文：the valueOf method is inherited by every object descended from Object.）。 <font color=FF0000>每个内置的核心对象都会覆盖此方法以返回适当的值</font>。如果对象没有原始值，则 valueOf 将返回对象本身。

  <font color=FF0000>JavaScript 的许多内置对象都重写了该函数，以实现更适合自身的功能需要</font>。因此，不同类型对象的 valueOf() 方法的返回值和返回值类型均可能不同。

  **不同类型对象的valueOf()方法的返回值**

  | **对象** | **返回值**                                                   |
  | :------- | :----------------------------------------------------------- |
  | Array    | 返回数组对象本身                                             |
  | Boolean  | 布尔值                                                       |
  | Date     | 存储的时间是 <font color=FF0000>从 1970 年 1 月 1 日午夜开始计的毫秒数 UTC</font>。注：即时间戳 |
  | Function | <mark>函数本身</mark>                                        |
  | Number   | 数字值。                                                     |
  | Object   | 对象本身。这是 <font color=FF0000>默认情况</font>            |
  | String   | 字符串值                                                     |
  |          | Math 和 Error 对象没有 valueOf 方法                          |

  你可以在自己的代码中使用 valueOf 将内置对象转换为原始值。<font color=FF0000>**创建自定义对象时，可以覆盖Object.prototype.valueOf() 来调用自定义方法（注：这里可以理解为 override ），而不是默认 Object 方法**</font>。

- **覆盖自定义对象的 valueOf方法**

  你可以创建一个取代 valueOf 方法的函数，你的 <font color=FF0000>方法必须不能传入参数</font>。

  假设你有个对象叫 MyNumberType，而你想为它创建一个 valueOf 方法。下面的代码为 valueOf 方法赋予了一个自定义函数：

  ```js
  MyNumberType.prototype.valueOf = function() { return customPrimitiveValue; };
  ```

  <mark>有了这样的一个方法，下一次每当 MyNumberType 要被转换为原始类型值时，JavaScript 在此之前会自动调用自定义的 valueOf 方法</mark>。

  <mark>valueOf 方法一般都会被 JavaScript 自动调用，但你也可以像下面代码那样自己调用</mark>：

  ```js
  myNumberType.valueOf()
  ```

- **示例：**

  ```js
  // Array：返回数组对象本身
  var array = ["ABC", true, 12, -5];
  console.log(array.valueOf() === array);   // true
  
  // Date：当前时间距1970年1月1日午夜的毫秒数
  var date = new Date(2013, 7, 18, 23, 11, 59, 230);
  console.log(date.valueOf());   // 1376838719230
  
  // Number：返回数字值
  var num =  15.26540;
  console.log(num.valueOf());   // 15.2654
  
  // boolean：返回布尔值 true 或 false
  var bool = true;
  console.log(bool.valueOf() === bool);   // true
  
  // new 一个 Boolean对象
  var newBool = new Boolean(true);
  // valueOf()返回的是true，两者的值相等
  console.log(newBool.valueOf() == newBool);   // true
  // 但是不全等，两者类型不相等，前者是boolean类型，后者是object类型
  console.log(newBool.valueOf() === newBool);   // false
  
  // Function：返回函数本身
  function foo(){}
  console.log( foo.valueOf() === foo );   // true
  var foo2 =  new Function("x", "y", "return x + y;");
  console.log( foo2.valueOf() );
  /* ƒ anonymous(x,y) { return x + y;} */
  
  // Object：返回对象本身
  var obj = {name: "张三", age: 18};
  console.log( obj.valueOf() === obj );   // true
  
  // String：返回字符串值
  var str = "http://www.xyz.com";
  console.log( str.valueOf() === str );   // true
  
  // new一个字符串对象
  var str2 = new String("http://www.xyz.com");
  // 两者的值相等，但不全等，因为类型不同，前者为string类型，后者为object类型
  console.log( str2.valueOf() === str2 );   // false
  ```

摘自：[MDN - Object.prototype.valueOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf)

#### Object.prototype.toString()

toString() 方法返回一个表示该对象的字符串

- **语法：**

  ```js
  obj.toString()
  ```

- **返回值：**一个表示该对象的字符串。

- **描述：**<font color=FF0000>**每个对象都有一个 toString() 方法，当该对象被表示为一个文本值时，或者一个对象以预期的字符串方式引用时自动调用**</font>。<font color=FF0000>默认情况下，toString() 方法被每个 Object 对象继承</font>。如果此方法在自定义对象中未被覆盖，toString() 返回 "[object type]"，其中 type 是对象的类型。以下代码说明了这一点：

  ```js
  var o = new Object();
  o.toString(); // 返回 [object Object]
  ```

- **覆盖默认的 toString 方法：**可以自定义一个方法，来取代默认的 toString() 方法。该 toString() 方法不能传入参数，并且必须返回一个字符串。自定义的 toString() 方法可以是任何我们需要的值，但如果它附带有关对象的信息，它将变得非常有用。

  下面的示例略，大致是：对一个对象使用 toString 方法只会打印 [object Object]；可以通过覆盖 Object.prototype.toString() 方法来自定义打印内容。

- **使用 toString() 检测对象类型：**可以通过 toString() 来获取每个对象的类型。为了每个对象都能通过 Object.prototype.toString() 来检测，需要以 Function.prototype.call() 或者 Function.prototype.apply() 的形式来调用，传递要检查的对象作为第一个参数，称为 thisArg。

  ```js
  var toString = Object.prototype.toString;
  
  toString.call(new Date); // [object Date]
  toString.call(new String); // [object String]
  toString.call(Math); // [object Math]
  
  //Since JavaScript 1.8.5
  toString.call(undefined); // [object Undefined]
  toString.call(null); // [object Null]
  ```

  摘自：[MDN - Object.prototype.toString()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)

##### 关于 typeof 判断类型的不足 & 解决方法的补充

**注，上面没有说痛点是什么，这里补充下：**<font color=FF0000>在 JavaScript 里使用 typeof 判断数据类型，只能区分基本类型</font>，即：number、string、undefined、boolean、object。<font color=FF0000>对于null、array、function、object 来说，使用 typeof 都会统一返回 object 字符串。要想区分对象、数组、函数、单纯使用 typeof 是不行的</font>。

在 JS 中，可以通过 Object.prototype.toString() 方法，判断某个对象之属于哪种内置类型（注：这也是判断内置类型 <font color=FF0000>最靠谱的方法</font>）。分为 null、string、boolean、number、undefined、array、function、object、date、math。（注：error 也可以）

另外，上面说的不全，还包含：RegExp、JSON、Arguments。示例如下：

```js
// 正则表达式
var reg = /[hbc]at/gi;
Object.prototype.toString.call(reg); // "[object RegExp]"

var isNativeJSON = window.JSON && Object.prototype.toString.call(JSON);
console.log(isNativeJSON);// 输出结果为”[object JSON]”说明JSON是原生的，否则不是

function argToString() { return Object.prototype.toString.call(arguments) }
console.log(argToString()) // [object Arguments]
```

摘自：[浅谈 Object.prototype.toString.call() 方法](https://www.jianshu.com/p/585926ae62cc)

使用  ***.constructor.name** 可以实现类似的功能（**注：**当然泛用性没有 toString 好，比如 null、undefined 不可用，arguments 的结果还是 Object。另外，这里的原理是： constructor 是一个函数，Function 有 name 这个属性 ），示例如下：

```js
const reg = /abc/
console.log(Object.getPrototypeOf(reg).constructor.name) // 'RegExp'
```

上面说了 JS 的数据类型，不知道具体有哪些，可以参考：[MDN - JavaScript 标准内置对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects)，有时间做一下笔记。

另外，还可以使用 instanceof 来解决问题，具体参见 [[前端面试点总结#typeof 和 instanceof 的实现原理]] 中的内容

##### Object.prototype.toString 执行步骤的补充

<font color=FF0000 size=4>**在 ECMAScript 5 中，Object.prototype.toString() 被调用时，会进行如下步骤：**</font>

- 如果 this 是 undefined ，返回 [object Undefined] ；
- 如果 this 是 null ， 返回 [object Null] ；
- 令 `O` 为以 this 作为参数调用 ToObject 的结果；
- <font color=FF0000>令 class 为 `O` 的内部属性 [[Class]] 的值</font>；
- 返回三个字符串 `"[object", class, 以及"]"` 拼接而成的字符串。

注，也就是说，在 ES5 中，toString 是通过 内部属性 [[class]] 获取的结果

<font color=FF0000 size=4>**在 ES6，调用 Object.prototype.toString 时，会进行如下步骤：**</font>

- 如果 this 是 undefined ，返回 '[object Undefined]' ;
- 如果 this 是 null , 返回 '[object Null]' ；
- 令 O 为以 this 作为参数调用 ToObject 的结果；
- 令 isArray 为 IsArray(O) ；
- ReturnIfAbrupt(isArray) （如果 isArray 不是一个正常值，比如抛出一个错误，中断执行）；
- 如果 isArray 为 true ， 令 builtinTag 为 'Array' ;
- else ，如果 O is an exotic String object ， 令 builtinTag 为 'String' ；
- else ，如果 O 含有 [[ParameterMap]] internal slot, ， 令 builtinTag 为 'Arguments' ； 
- else ，如果 O 含有 [[Call]] internal method ， 令 builtinTag 为 Function ；
- else ，如果 O 含有 [[ErrorData]] internal slot ， 令 builtinTag 为 Error ；
- else ，如果 O 含有 [[BooleanData]] internal slot ， 令 builtinTag 为 Boolean ；
- else ，如果 O 含有 [[NumberData]] internal slot ， 令 builtinTag 为 Number ；
- else ，如果 O 含有 [[DateValue]] internal slot ， 令 builtinTag 为 Date ；
- else ，如果 O 含有 [[RegExpMatcher]] internal slot ， 令 builtinTag 为 RegExp ；
- else ， 令 builtinTag 为 Object ；
- 令 tag 为 Get(O, @@toStringTag) 的返回值（ Get(O, @@toStringTag) 方法，既是在 O 是一个对象，并且具有 @@toStringTag 属性时，返回 O[Symbol.toStringTag] ）；
- ReturnIfAbrupt(tag) ，如果 tag 是正常值，继续执行下一步；
- 如果 Type(tag) 不是一个字符串，let tag be builtinTag ；
- 返回由三个字符串 `"[object", tag, and "]"` 拼接而成的一个字符串。

在ES6里，之前的 [[Class]] 不再使用，取而代之的是一系列的 internal slot，有一个比较完整的解释：

> Internal slots correspond to internal state that is associated with objects and used by various ECMAScript specification algorithms. Internal slots are not object properties and they are not inherited. Depending upon the specific internal slot specification, such state may consist of values of any ECMAScript language type or of specific ECMAScript specification type values

大概的意思是：Internal slots 对应于与对象相关联并由各种ECMAScript规范算法使用的内部状态，它们没有对象属性，也不能被继承，根据具体的 Internal slot 规范，这种状态可以由任何ECMAScript语言类型或特定ECMAScript规范类型值的值组成。

**注：**上面 ES6 toString 的 执行过程中包含了大量的 内部属性，在学习 内部属性 时可以分别了解下。

摘自：[谈谈 Object.prototype.toString](https://juejin.cn/post/6844903477940846600)

#### 对象的原始值转换 和 valueOf toString

**背景：**当对象相加 obj1 + obj2，相减 obj1 - obj2，或者使用 alert(obj) 打印时会发生什么？在这种情况下，对象会被自动转换为原始值，然后执行操作。

这里要提到：Symbol.toPrimitive 中的 hint。

> The hint is a type of the primitive the object is going to be converted to.
>
> 摘自：[Understanding JavaScript type conversions](https://dev.to/antonmelnyk/understanding-javascript-type-conversions-43n)

即：hint 是对象将要 转换成为的 基本类型，且 hint 的值，只有三种："number"、"string"、"defalut"

- **"string"：**对象到字符串的转换，当我们对期望一个字符串的对象执行操作时，如 "alert"：

  ```js
  // 输出
  alert(obj);
  
  // 将对象作为属性键
  anotherObj[obj] = 123;
  ```

- **"number"：**对象到数字的转换，例如当我们进行数学运算

  ```js
  // 显式转换
  let num = Number(obj);
  
  // 数学运算（除了二元加法）
  let n = +obj; // 一元加法
  let delta = date1 - date2;
  
  // 小于/大于的比较
  let greater = user1 > user2;
  ```

- **"default"：**在少数情况下发生，当运算符“不确定”期望值的类型时。

  例如，二元加法 `+` 可用于字符串（连接），也可以用于数字（相加），所以字符串和数字这两种类型都可以。因此，当二元加法得到对象类型的参数时，它将依据 "default" hint 来对其进行转换。

  此外，如果对象被用于与字符串、数字或 symbol 进行 `==` 比较，这时到底应该进行哪种转换也不是很明确，因此使用 "default" hint。

  ```js
  // 二元加法使用默认 hint
  let total = obj1 + obj2;
  
  // obj == number 使用默认 hint
  if (user == 1) { ... };
  ```

  像 `<` 和 `>` 这样的小于/大于比较运算符，也可以同时用于字符串和数字。不过，它们使用 “number” hint，而不是 “default”。这是历史原因。

**为了进行转换，JavaScript 尝试查找并调用三个对象方法：**

1. 调用 obj\[Symbol.toPrimitive](hint) —— 带有 symbol 键 Symbol.toPrimitive（系统 symbol）的方法，如果这个方法存在的话，
2. 否则，如果 hint 是 "string" —— <font color=FF0000>尝试 obj.toString() 和 obj.valueOf()</font>，无论哪个存在。
3. 否则，如果 hint 是 "number" 或 "default" —— <font color=FF0000>尝试 obj.valueOf() 和 obj.toString()</font>，无论哪个存在。

注：注意 hint 是 "string" 和 "number" / "default" 这两种情况下 toString() valueOf() 的顺序，下面也会说到。

<font size=4>**toString / valueOf**</font>

方法 toString 和 valueOf 来自上古时代（注：根据 MDN 相关页面的规范，这两个方法最早均来自 ES1）。它们不是 symbol（那时候还没有 symbol 这个概念），而是 “常规的” 字符串命名的方法。它们提供了一种可选的“老派”的实现转换的方法。

<mark>**如果没有 Symbol.toPrimitive**，那么 JavaScript 将尝试找到它们，并且按照下面的顺序进行尝试</mark>：

- **对于 "string" hint：**<font color=FF0000>toString -> valueOf</font>
- **其他情况**（注：即 "number" / "defalut" ）：<font color=FF0000>valueOf -> toString</font>

这些方法必须返回一个原始值。如果 toString 或 valueOf 返回了一个对象，那么返回值会被忽略（和这里没有方法的时候相同）。

**默认情况下，普通对象具有 toString 和 valueOf 方法：**

- <font color=FF0000>toString 方法返回一个字符串 "[object Object]"</font>。
- <font color=FF0000>valueOf 方法返回对象自身</font>。

示例如下：

```js
let user = {name: "John"};

alert(user); // [object Object]
alert(user.valueOf() === user); // true
```

让我们实现一下这些方法（注：即在对象中 定义（也可以认为是覆盖 override） valueOf() 和 toString() ）。例如，这里的 user 执行和前面提到的那个 user 一样的操作，使用 toString 和 valueOf 的组合（而不是 Symbol.toPrimitive）：

```js
let user = {
  name: "John",
  money: 1000,

  // 对于 hint="string"
  toString() { return `{name: "${this.name}"}`; },

  // 对于 hint="number" 或 "default"
  valueOf() { return this.money; }
};

alert(user); // toString -> {name: "John"}
alert(+user); // valueOf -> 1000
alert(user + 500); // valueOf -> 1500
```

<font size=4>**返回类型**</font>

关于所有原始转换方法，有一个重要的点需要知道，就是它们不一定会返回 "hint" 的原始值。<font color=FF0000>没有限制 toString() 是否返回字符串</font>，<font color=FF0000>或 Symbol.toPrimitive 方法是否为 hint "number" 返回数字</font>。<font color=FF0000>**唯一强制性的事情是：这些方法必须返回一个原始值，而不是对象**</font>。

> **历史原因**
>
> <font color=FF0000>由于历史原因，如果 toString 或 valueOf 返回一个对象，则不会出现 error，但是这种值会被忽略</font>（就像这种方法根本不存在）。<font color=FF0000>这是因为在 JavaScript 语言发展初期，没有很好的 “error” 的概念</font>（注：Error 确实是在 ES1 时代就已经有了）。
>
> 相反，<font color=FF0000>Symbol.toPrimitive **必须** 返回一个原始值，否则就会出现 error</font>。

<font size=4>**进一步的转换**</font>

**如果我们将对象作为参数传递，则会出现两个阶段：**

1. 对象被转换为原始值（通过前面我们描述的规则）。
2. <font color=FF0000 size=4>**如果生成的原始值的类型不正确，则继续进行转换**</font>。

例如：

```javascript
let obj = {
  // toString 在没有其他方法的情况下处理所有转换
  toString() { return "2"; }
};

alert(obj * 2); // 4，对象被转换为原始值字符串 "2"，之后它被乘法转换为数字 2。
```

1. 乘法 `obj * 2` 首先将对象转换为原始值（字符串 “2”）。
2. 之后 `"2" * 2` 变为 `2 * 2`（字符串被转换为数字）。

<mark>二元加法在同样的情况下会将其连接成字符串，因为它更愿意接受字符串</mark>：

```js
let obj = {
  toString() { return "2"; }
};

alert(obj + 2); // 22（"2" + 2）被转换为原始值字符串 => 级联
```

摘自：[现代JS教程 - 对象 — 原始值转换](https://zh.javascript.info/object-toprimitive#symboltoprimitive)



#### JS判断对象是否为空

判断 Object.keys(obj).length 是否等于 0

#### JS判断对象是否包含某个key

有两种方法：

- 'key' in obj：返回一个bool值（这是类似于Python的写法）
- **obj.hasOwnProperty("key")：**返回一个bool值

摘自：[JS中判断对象是否包含某个key的方法](https://blog.csdn.net/sinat_34241861/article/details/107347288)

#### Object.prototype.hasOwnProperty()

hasOwnProperty() 方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性

**语法**

```js
obj.hasOwnProperty(prop)
```

- **参数** 

  prop：要检测的属性的 String 字符串形式表示的名称，或者 Symbol。

- 返回值：用来判断某个对象是否含有指定的属性的布尔值 Boolean。

所有继承了 Object 的对象都会继承到 hasOwnProperty 方法。这个方法可以用来检测一个对象是否含有特定的自身属性；<font color=FF0000>和 in 运算符不同，该方法**会忽略**掉那些从原型链上继承到的属性</font>。

**备注：**即使属性的值是 null 或 undefined，只要属性存在，hasOwnProperty 依旧会返回 true。

摘自：[MDN - Object.prototype.hasOwnProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)

##### 《红宝书》中的关于 hasOwnProperty 的补充

hasOwnProperty() 方法（可以）<font color=FF0000>用于 **确定** 某个属性是在实例上，还是在原型对象上</font>。这个方法是继承自 Object 的，会<font color=FF0000>在（该）属性存在于 **调用它的对象实例上时**，**返回 true**</font>

**摘自：**《JS 高程 第四版》P229



#### Object.prototype.isPrototypeOf()

<font color=FF0000>isPrototypeOf() 方法用于测试一个对象是否存在于另一个对象的**原型链**上</font>。

<font color=FF0000>isPrototypeOf() 与 instanceof 运算符不同</font>。<mark>在表达式 "object instanceof AFunction"中，object 的原型链是针对 AFunction.prototype 进行检查的，而不是针对 AFunction 本身</mark>。

- **语法**

  ```js
  prototypeObj.isPrototypeOf(object)
  ```

- **参数**
  object：在该对象的原型链上搜寻

- 返回值

  Boolean：表示调用对象是否在另一个对象的原型链上。

- **报错**
  TypeError：如果 prototypeObj 为 undefined 或 null，会抛出 TypeError。

摘自：[MDN - Object.prototype.isPrototypeOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/isPrototypeOf)

#### Object.getPrototypeOf()

<font color=FF0000>Object.getPrototypeOf() 方法返回指定对象的原型（**内部[[Prototype]]属性的值**）</font>。

- **语法**

  ```js
  Object.getPrototypeOf(object)
  ```

- **参数**
  obj：要返回其原型的对象。

- **返回值：**给定对象的原型。如果没有继承属性，则返回 null 。

摘自：[MDN - Object.getPrototypeOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getPrototypeOf)

#### Object.setPrototypeOf()

<font color=FF0000>Object.setPrototypeOf() 方法设置一个指定的对象的原型 ( 即, 内部[[Prototype]]属性）到另一个对象或  null。</font>

<font size=4 color=FF0000>**警告:** </font>由于现代 JavaScript 引擎优化属性访问所带来的特性的关系，<font color=FF0000>更改对象的 [[Prototype]]在各个浏览器和 JavaScript 引擎上都是一个很慢的操作。其在更改继承的性能上的影响是微妙而又广泛的，这不仅仅限于 obj.\__proto__ = ... 语句上的时间花费，而且可能会延伸到任何代码，那些可以访问任何[[Prototype]]已被更改的对象的代码</font>。<mark>如果你关心性能，你应该避免设置一个对象的 [[Prototype]]。相反，你应该使用 Object.create()来创建带有你想要的[[Prototype]]的新对象</mark>。

- **语法**

  ```js
  Object.setPrototypeOf(obj, prototype)
  ```

- **参数**

  - **obj：**要设置其原型的对象。.
  - **prototype：**该对象的新原型(一个对象 或 null).

- **描述：**

  - 如果对象的[[Prototype]]被修改成不可扩展(通过 Object.isExtensible()查看)，就会抛出 TypeError异常。
  - 如果prototype参数不是一个对象或者null(例如，数字，字符串，boolean，或者 undefined)，则什么都不做。
  - 否则，该方法将obj的[[Prototype]]修改为新的值。

  Object.setPrototypeOf() 是ECMAScript 6最新草案中的方法，相对于 Object.prototype.\__proto__  ，它被认为是修改对象原型更合适的方法

摘自：[MDN - Object.setPrototypeOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/setPrototypeOf)



#### Object.getOwnPropertyDescriptor()

Object.getOwnPropertyDescriptor() 方法 <font color=FF0000>返回指定对象上一个 **自有属性** 对应的属性描述符</font>。（自有属性指的是<font color=FF0000>直接赋予该对象的属性，不需要从原型链上进行查找的属性</font>）

- **语法：**

  ```js
  Object.getOwnPropertyDescriptor(obj, prop)
  ```

- **参数：**

  - **obj：**需要查找的目标对象
  - **prop：**目标对象内属性名称。**注：**prop需要 引号包裹

- **返回值：**<mark>如果指定的属性存在于对象上，则返回其属性描述符对象（property descriptor），否则返回 undefined</mark>。

摘自：[MDN - Object.getOwnPropertyDescriptor()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)

#### Object.getOwnPropertyDescriptors()

Object.getOwnPropertyDescriptors() 方法用来获取一个对象的所有自身属性的描述符。

- **语法**

  ```js
  Object.getOwnPropertyDescriptors(obj)
  ```

- **参数**

  obj：任意对象

- **返回值：**所指定对象的所有自身属性的描述符，如果没有任何自身属性，则返回空对象。

摘自：[MDN - Object.getOwnPropertyDescriptors()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors)



#### Object.getOwnPropertyNames()

Object.getOwnPropertyNames() 方法返回一个由指定对象的所有自身属性的属性名（<font color=FF0000>包括不可枚举属性，但 <font size=4>**不包括Symbol值作为名称的属性**</font></font>）组成的数组。

- **语法：**

  ```js
  Object.getOwnPropertyNames(obj)
  ```

- **参数**
  **obj：**一个对象，其自身的可枚举和不可枚举属性的名称被返回。

- **返回值：**在给定对象上找到的自身属性对应的字符串数组。

- **描述：**Object.getOwnPropertyNames() 返回一个数组，该数组对元素是 obj自身拥有的枚举或不可枚举属性名称字符串。 数组中枚举属性的顺序与通过 for...in 循环（或 Object.keys）迭代该对象属性时一致。数组中不可枚举属性的顺序未定义。

- **特殊示例：**

  ```js
  const arr = [1, 2, 3]
  console.log(Object.getOwnPropertyNames(arr)) // [ '0', '1', '2', 'length' ]
  ```

  注意：离这里是会打印出 length的，用Reflect.ownKeys() 也是，但是用 Object.keys() 不会

摘自：[MDN - Object.getOwnPropertyNames()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyNames)



#### Object.getOwnPropertySymbols()

Object.getOwnPropertySymbols() 方法<font color=FF0000>返回一个给定对象自身的所有 Symbol 属性的数组</font>。

- **语法**

  ```js
  Object.getOwnPropertySymbols(obj)
  ```

- **参数**
  obj：要返回 Symbol 属性的对象。

- **返回值：**在给定对象自身上找到的所有 Symbol 属性的数组。

- **描述**
  与Object.getOwnPropertyNames()类似，您可以将给定对象的所有符号属性作为 Symbol 数组获取。 请注意，Object.getOwnPropertyNames()本身不包含对象的 Symbol 属性，只包含字符串属性。

  因为所有的对象在初始化的时候不会包含任何的 Symbol，除非你在对象上赋值了 Symbol 否则Object.getOwnPropertySymbols()只会返回一个空的数组。

- 示例：

  ```js
  var obj = {};
  var a = Symbol("a");
  var b = Symbol.for("b");
  
  obj[a] = "localSymbol";
  obj[b] = "globalSymbol";
  
  var objectSymbols = Object.getOwnPropertySymbols(obj);
  
  console.log(objectSymbols.length); // 2
  console.log(objectSymbols)         // [Symbol(a), Symbol(b)]
  console.log(objectSymbols[0])      // Symbol(a)
  ```

摘自：[MDN - Object.getOwnPropertySymbols()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols)



#### new.target

new.target 属性允许你<font color=FF0000> **检测函数或构造方法是否是通过 new 运算符被调用的**</font>。<font color=FF0000> 在通过 new 运算符被初始化的函数或构造方法中，**new.target 返回一个指向构造方法或函数的引用**</font>。<mark>在普通的函数调用中，new.target 的值是<font color=FF0000> undefined</font></mark>。

##### 语法

```js
new.target
```

##### 描述 

new.target 语法由一个关键字 "new"，一个点，和一个属性名 "target" 组成。<font color=FF0000> 通常 "new." 的作用是提供属性访问的上下文，但 **这里 "new." 其实不是一个真正的对象**</font>。不过在构造方法调用中，new.target 指向被 new 调用的构造函数，所以 "new." 成为了一个虚拟上下文。

new.target 属性适用于所有函数访问的元属性。<font color=FF0000>在箭头函数中，new.target 指向最近的外层函数的 new.target</font>

##### 示例

- **函数调用中的 new.target**
  在普通的函数调用中（和作为构造函数来调用相对），new.target 的值是 undefined。这使得你可以检测一个函数是否是作为构造函数通过 new 被调用的。

  ```js
  function Foo() {
    if (!new.target) throw "Foo() must be called with new";
    console.log("Foo instantiated with new");
  }
  
  Foo(); // throws "Foo() must be called with new"
  new Foo(); // logs "Foo instantiated with new"
  ```
  
- **构造方法中的 new.target**

  在类的构造方法中，new.target 指向直接被 new 执行的构造函数。并且当一个父类构造方法在子类构造方法中被调用时，情况与之相同。

  ```js
  class A {
    constructor() { console.log(new.target.name); }
  }
  class B extends A { constructor() { super(); } }
  
  var a = new A(); // logs "A"
  var b = new B(); // logs "B"
  
  class C { constructor() { console.log(new.target); } }
  class D extends C { constructor() { super(); } }
  
  var c = new C(); // logs class C{constructor(){console.log(new.target);}}
  var d = new D(); // logs class D extends C{constructor(){super();}}
  ```
  
  从上面类 C 和 D 的例子可以看出来，new.target 指向的是初始化类的类定义。比如当 D 通过 new 初始化的时候，打印出了 D 的类定义，C 的例子与之类似，打印出的是 C 的类定义。

摘自：[MDN - new.target](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new.target)

##### 视频《ES6中的难点--new.target是什么鬼东西？》的补充

new.target 是 ES6 新增的

class 定义的类( `class ClassName { ...  }` )，是无法通过 “非 new” 的方式调用的。必须要 `new ClassName()`，否则会报错。那么 如何让 `function ClassName() {}` 定义的类，也必须要通过 new 调用（否则报错）？可以通过判断 new.target。

有点类似于 Event.target （而不是 Event.currentTarget ），在继承中，哪怕父类中有 new.target，new.target 指向的也会是 当前 new 的对象，如下示例：

```js
class Parent {
  constructor(name) {
    console.log(new.target)   // super() 调用的
    this.name = name
  }
}

class Child extends Parent {
  constructor() {
    super()                  // class Child extends Parent {...}
    console.log(new.target)  // class Child extends Parent {...}
  }
}

new Child()
```

类似的，new.target 可以用来实现（不可实例化的）抽象类
```js
class AbstractClass {
  constructor() {
    if(new.target === AbstractClass) {
      throw(new Error('抽象类不能示例化'))
    }
  }
}
```

学习自：[【全网首发:更新完】ES6中的难点--new.target是什么鬼东西？](https://www.bilibili.com/video/BV1CT4y1o7qL)



#### Reflect

**Reflect** 是一个<font color=FF0000>**内置的对象**，它提供拦截 JavaScript 操作的方法</font>。<font color=FF0000>Reflect 不是一个函数对象，因此它是不可构造的</font>。

与大多数全局对象不同，Reflect 并非一个构造函数，所以<font color=FF0000>不能通过 new 运算符对其进行调用</font>，或者将Reflect对象作为一个函数来调用。<font color=FF0000>Reflect的所有属性和方法都是静态的</font>（<font color=FF0000>就像Math对象</font>）。

##### 《现代 JS 教程》中的 Reflect

由于 MDN 中 没有说清楚 Reflect 的作用，所以摘抄《现代 JS 教程》中的内容：

> Reflect 是一个内建对象，可简化 Proxy 的创建。
>
> <font color=FF0000>Proxy 中定义的内部方法</font>（注：即那些「内部属性」，详见原文中的表格），<font color=FF0000>例如 \[[Get]] 和 \[[Set]] 等，都只是规范性的，不能直接调用</font>。
>
> <font color=FF0000>Reflect 对象（**注：**的存在？）使调用这些内部方法成为了可能</font>。<font color=FF0000>它的方法</font>（**注：**即 Reflect 调用这些 “内部方法” 的方法）<font color=FF0000>是内部方法的 **最小包装**</font>。
>
> <font color=FF0000>**对于每个可被 Proxy 捕获的内部方法，在 Reflect 中都有一个对应的方法，其名称和参数与 Proxy 捕捉器相同**</font>。所以，我们可以使用 Reflect 来将操作转发给原始对象。
>
> **以下是执行相同操作和 Reflect 调用的示例：**
>
> | 操作              | Reflect 调用                      | 内部方法       |
> | :---------------- | :-------------------------------- | :------------- |
> | obj[prop]         | Reflect.get(obj, prop)            | \[[Get]]       |
> | obj[prop] = value | Reflect.set(obj, prop, value)     | \[[Set]]       |
> | delete obj[prop]  | Reflect.deleteProperty(obj, prop) | \[[Delete]]    |
> | new F(value)      | Reflect.construct(F, value)       | \[[Construct]] |
> | …                 | …                                 | …              |
>
> **代码示例：**
>
> ```js
> let user = {};
> Reflect.set(user, 'name', 'John');
> 
> alert(user.name); // John
> ```
>
> 在下面这个示例中，捕捉器 get 和 set 均透明地（好像它们都不存在一样）将读取/写入操作转发到对象，并显示一条消息：
>
> ```js
> let user = { name: "John", };
> 
> user = new Proxy(user, {
>   get(target, prop, receiver) {
>     alert(`GET ${prop}`);
>     return Reflect.get(target, prop, receiver); // (1)
>   },
>   set(target, prop, val, receiver) {
>     alert(`SET ${prop}=${val}`);
>     return Reflect.set(target, prop, val, receiver); // (2)
>   }
> });
> 
> let name = user.name; // 显示 "GET name"
> user.name = "Pete"; // 显示 "SET name=Pete"
> ```
>
> 摘自：[现代 JS 教程 - Proxy 和 Reflect - Reflect](https://zh.javascript.info/proxy#reflect)

##### Vue3 官方文档中的补充

> 使用 Proxy 的一个难点是 this 绑定。我们希望任何方法都绑定到这个 Proxy，而不是目标对象，这样我们也可以拦截它们。值得庆幸的是，ES6 引入了另一个名为 Reflect 的新特性，它允许我们以最小的代价消除了这个问题。
>
> 摘自：[Vue3 官方文档 - 深入响应性原理](https://v3.cn.vuejs.org/guide/reactivity.html)

**静态方法**

- **Reflect.<font color=FF0000>apply</font>(target, thisArgument, argumentsList)：**<font color=FF0000>对一个函数进行调用操作，同时可以传入一个数组作为调用参数</font>。和 Function.prototype.apply() 功能类似。

- **Reflect.construct(target, argumentsList[, newTarget])：**对构造函数进行 new 操作，相当于执行 new target(...args)。

- **Reflect.<font color=FF0000>defineProperty</font>(target, propertyKey, attributes)：**<font color=FF0000>和 Object.defineProperty() 类似</font>。如果设置成功就会返回 true。

  <font color=FF0000>**补充：**</font><mark>**不同于Object.defineProperty()，不同的是该方法返回的是布尔值，而不需要你像以前那样去捕捉异常(因为Object.defineProperty 是在执行出错的时候直接抛错的)，<font color=FF0000>下面类似的也一样</font>**</mark>

  如果target不是 Object，抛出一个 TypeError。

- **Reflect.<font color=FF0000>deleteProperty</font>(target, propertyKey)：**作为函数的delete操作符，<font color=FF0000>相当于执行 delete target[name]</font>。

- **Reflect.get(target, propertyKey[, receiver])：**获取对象身上某个属性的值，类似于 target[name]。

- **Reflect.getOwnPropertyDescriptor(target, propertyKey)：**类似于 Object.getOwnPropertyDescriptor()。如果对象中存在该属性，则返回对应的属性描述符,  否则返回 undefined.

- **Reflect.getPrototypeOf(target)：**类似于 Object.getPrototypeOf()。

- **Reflect.<font color=FF0000>has</font>(target, propertyKey)：**<font color=FF0000>判断一个对象是否存在某个属性，和 in 运算符 的功能完全相同</font>。

- **Reflect.isExtensible(target)：**类似于 Object.isExtensible().

- **Reflect.<font color=FF0000>ownKeys</font>(target)：**<font color=FF0000>返回一个包含所有自身属性（不包含继承属性）的数组</font>。(<mark>类似于 Object.keys() ，但不会受enumerable影响</mark>).

- **Reflect.preventExtenions(target)：**类似于 Object.preventExtensions()。返回一个Boolean。

- **Reflect.<font color=FF0000>set</font>(target, propertyKey, value[, receiver])：**<font color=FF0000>将值分配给属性的函数（即：为这个对象添加一个新的属性）</font>。返回一个Boolean，如果更新成功，则返回true。

- **Reflect.setPrototypeOf(target, prototype)：**设置对象原型的函数. 返回一个 Boolean， 如果更新成功，则返回true。

摘自：[MDN - Reflect](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect) 另外，可以补充阅读：[JS的Reflect学习和应用](https://zhuanlan.zhihu.com/p/92700557)，该文章对上面的静态方法，做了示例。

##### 使用 Reflect 的好处

- **将 Object 属于语言内部的方法转移到 Reflect 上**

  如上面 ES6 中：将 window.isNaN() 转移到 Number 下，成为 Number.isNaN() ，使之设计更合理。<font color=FF0000>在 ES5 设计初期，将一些方法放到 Object 下，这样的设计并不合理。而在 ES6 中，将其放到 Reflect 下（ 比如 Object.defineProperty() 变成 Reflect.defineProperty() ），这样设计会更加规范</font>

- **修改某些Object方法的返回结果，让其变得更合理**

  还是以 Object.defineProperty() 为例，如果当前有些属性无法定义，则会抛出异常，需要使用try...catch。而在 Reflect.defineProperty()中，会返回一个boolean值，如果出现异常则会返回一个false

- **让 Object 操作变成函数行为**

  Object 的一些操作都是命令式的，如判断 Object 中是否含有 assign 这一方法，使用： ` assign in Object ` ，返回 boolean 值；而在 Reflect 中是函数式的：`Reflect.has(Object, 'assign')` 返回 boolean 值，这样更加合理

- <font color=FF0000>**Reflect 对象的方法与 Proxy 对象的方法一对应**</font>

  在 Proxy 下，使用 Object 的方法，可以使用 Reflect 替代，并且使用方法和 Proxy 一样。

##### CodingStartup 群群友关于 Proxy 和 Reflect 的补充

> proxy 是代理行为，reflect 是注入行为

#### Reflect.ownKeys()

<font color=FF0000>静态方法</font> Reflect.ownKeys() <font color=FF0000>返回一个**由目标对象自身的属性键组成的数组**</font>。

**语法：**

```js
Reflect.ownKeys(target)
```

- 参数

  **target：**获取自身属性键的目标对象。

- 返回值：由目标对象的自身属性键组成的 Array。 

- 异常：<font color=FF0000>如果目标不是 Object，抛出一个 TypeError</font>。

**描述：**Reflect.ownKeys 方法返回一个由目标对象自身的属性键组成的数组。它的返回值等同于Object.getOwnPropertyNames(target).concat(Object.getOwnPropertySymbols(target))

**示例：**

```js
const obj = {
  foo: 1,
  bar: 2
}
console.log(Reflect.ownKeys(obj)); // [ 'foo', 'bar' ]

const arr = [1]
console.log(Reflect.ownKeys(arr)) // [ '0', 'length' ]
```

摘自：[MDN - Reflect.ownKeys()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys)

**个人补充：**Reflect.ownKeys() 可以获取 enumerable 为 false 的数据，而 Object.keys() 和 for...of 不可以



#### Symbol

<font color=FF0000>**symbol 是一种基本数据类型**</font> （primitive data type）。<font color=FF0000>Symbol() 函数会返回 symbol 类型的值，该类型具有静态属性和静态方法</font>。它的静态属性会暴露几个内建的成员对象；它的静态方法会暴露全局的 symbol 注册，且类似于内建对象类，但<font color=FF0000>作为构造函数来说它并不完整，因为它不支持语法："new Symbol()"</font>（注：下面有示例代码）。

每个从 Symbol() 返回的 symbol值都是唯一的。<font color=FF0000>**一个 symbol值能作为对象属性的标识符**</font>（**注：**示例如下）；这是该数据类型仅有的目的

> ```js
> var obj = {};
> var a = Symbol("a");
> var b = Symbol.for("b");
> 
> obj[a] = "localSymbol";
> obj[b] = "globalSymbol";
> ```
>
> 摘自：[MDN - Object.getOwnPropertySymbols()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols)

上面是在初始化之后赋值，也可以<font color=FF0000>使用“计算属性”在初始化时赋值</font>：

> 当一个 Symbol 包装器对象作为一个属性的键时，这个对象将被强制转换为它包装过的 symbol 值：
>
> ```js
> var sym = Symbol("foo");
> var obj = {[sym]: 1};
> obj[sym];            // 1
> obj[Object(sym)];    // still 1
> ```
>
> 摘自：[MDN - Symbol - Symbol 包装器对象作为属性的键](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol#symbol_包装器对象作为属性的键)

**语法**

```js
Symbol([description])
```

**参数**

- **description：**可选，字符串类型。对 symbol 的描述，可用于调试但不是访问 symbol 本身。

**描述**

直接使用 Symbol() 创建新的 symbol 类型，并<font color=FF0000>用一个可选的字符串作为其描述</font>。

```js
var sym1 = Symbol();
var sym2 = Symbol('foo');
var sym3 = Symbol('foo');
```

上面的代码创建了三个新的symbol类型。 注意，Symbol("foo") 不会强制将字符串 “foo” 转换成symbol类型。它每次都会创建一个新的 symbol类型：

```js
Symbol("foo") === Symbol("foo"); // false
```

**下面带有 new 运算符的语法将抛出 TypeError 错误：**

```js
var sym = new Symbol(); // TypeError
```

这会阻止创建一个显式的 Symbol 包装器对象而不是一个 Symbol 值。<font color=FF0000>**围绕原始数据类型创建一个显式包装器对象从 ECMAScript 6 开始不再被支持**</font>。 然而，<mark>现有的原始包装器对象，如 `new Boolean`、`new String` 以及 `new Number`，因为遗留原因仍可被创建</mark>。

##### 全局共享的 Symbol

上面<mark>使用 Symbol() 函数的语法，<font color=FF0000 size=4>**不会**</font>在你的整个代码库中创建一个可用的全局的 symbol 类型</mark>。 <font color=FF0000>要创建跨文件可用的 symbol，甚至跨域（每个都有它自己的全局作用域），**使用 Symbol.for() 方法和  Symbol.keyFor() 方法从全局的 symbol注册表设置和取得 symbol**</font>。

##### 在对象中查找 Symbol 属性

Object.getOwnPropertySymbols() 方法让你在查找一个给定对象的符号属性时返回一个 symbol 类型的数组。注意，每个初始化的对象都是没有自己的 symbol 属性的，因此这个数组可能为空，除非你已经在对象上设置了 symbol 属性。

**属性**

- **Symbol.length：**长度属性，值为0。
- **Symbol.prototype：**symbol 构造函数的原型。

**方法**

- **Symbol.for(key)：**<font color=FF0000>使用给定的 key 搜索现有的 symbol</font>，如果<mark>找到则返回该 symbol；否则将使用给定的 key 在全局 symbol注册表中创建一个新的 symbol</mark>。
- **Symbol.keyFor(sym)：**<font color=FF0000>从全局 symbol注册表中，为给定的 symbol检索一个共享的 symbol key</font>。

<font size=4>**Symbol 原型：**</font>所有 Symbols 继承自 Symbol.prototype

**实例属性**

- **Symbol.prototype.description：**一个只读的字符串，意为对该 Symbol 对象的描述

**实例方法**

- **Symbol.prototype.toSource：**返回该 Symbol 对象的源代码。该方法重写了 Object.prototype.toSource 方法
- **Symbol.prototype.toString：**返回一个包含着该 Symbol 对象描述的字符串。该方法重写了 Object.prototype.toString 方法
- **Symbol.prototype.valueOf：**返回该 Symbol 对象。该方法重写了 Symbol.prototype.valueOf 方法
- **Symbol.prototype[@@toPrimitive]：**返回该 Symbol 对象。

##### **其他细节**

- ```js
  typeof Symbol.iterator === 'symbol'
  ```

- **Symbols 与 JSON.stringify()**

  当<font color=FF0000 size=4>**使用 JSON.stringify() 时，以 symbol 值作为键的属性会被完全忽略**</font>：

  ```js
  JSON.stringify({[Symbol("foo")]: "foo"}); // '{}'
  ```

摘自：[MDN - Symbol](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)

#### 关于标识符的补充

 JS 对象的标识符可以是 string、number、symbol 这三种类型的。

> <mark>一个对象的属性名可以是任何有效的 JavaScript 字符串，<font color=FF0000>或者可以被转换为字符串的任何类型，包括空字符串</font></mark>。然而，<font color=FF0000>一个属性的名称如果不是一个有效的 JavaScript 标识符</font> ( JavaScript identifier )（例如，一个由空格或连字符，或者以数字开头的属性名），<font color=FF0000>就只能通过方括号标记访问</font>。<font color=FF0000>**这个标记法在属性名称是动态判定（属性名只有到运行时才能判定）时非常有用**</font>（**注：**这里和 JS 的计算属性值非常类似，甚至感觉就是一个东西）。
>
> ```js
> // 同时创建四个变量，用逗号分隔
> var myObj = new Object(),
>     str = "myString",
>     rand = Math.random(),
>     obj = new Object();
> 
> myObj.type              = "Dot syntax";
> myObj["date created"]   = "String with space";
> myObj[str]              = "String value";
> myObj[rand]             = "Random Number";
> myObj[obj]              = "Object";
> myObj[""]               = "Even an empty string";
> 
> console.log(myObj); // { type: 'Dot syntax', 'date created': 'String with space', myString: 'String value', '0.38817180392201656': 'Random Number', '[object Object]': 'Object', '': 'Even an empty string' }
> ```
>
> 请注意，方括号中的所有键都将转换为字符串类型，因为 JavaScript 中的对象只能使用 String 类型作为键类型。 例如，在上面的代码中，当将键obj添加到myObj时，JavaScript将调用obj.toString()方法，并将此结果字符串用作新键。
>
> 摘自：[MDN - 使用对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Working_with_Objects)

学习自：[几个一看就会的 TypeScript 小技巧](https://juejin.cn/post/7077536309804859428)

#### symbol 的 11个 内置值

>  除了定义自己使用的 Symbol 值以外，ES6 还提供了 11 个内置的 Symbol 值，指向语言内部使用的方法。
>
> 摘自：[阮一峰 ECMAScript 6 (ES6) 标准入门教程 第三版 - Symbol - 8. 内置的 Symbol 值](https://es6.ruanyifeng.com/#docs/symbol#内置的-Symbol-值)
>
> 这些内置的Symbol值被称为well-known symbols（常用内置符号）。另外，<font color=FF0000 size=4>**`Symbol.` 可以简写为 `@@`**</font>
>
> 摘自：[Detailed Overview of Well-known Symbols](https://dmitripavlutin.com/detailed-overview-of-well-known-symbols/)

在 说明 Symbol 各值之前，说一些共性：Symbol 值的对象属性描述符 (writable、enumerable、configurable) 均为 false：

| Symbol值的属性特性： | 值    |
| :------------------- | ----- |
| writable             | false |
| enumerable           | false |
| configurable         | false |

<font size=4>**Symbol.iterator**</font>

Symbol.iterator 为每一个对象定义了默认的迭代器。该迭代器可以被 for...of 循环使用。

当需要对一个对象进行迭代时（比如开始用于一个 for...of 循环中），它的 @@iterator 方法都会在不传参情况下被调用，返回的迭代器用于获取要迭代的值。

<font color=FF0000>一些内置类型拥有默认的迭代器行为，其他类型（如 Object）则没有</font>。下表中的内置类型拥有默认的 @@iterator 方法：Array.prototype\[@@iterator]()、TypedArray.prototype\[@@iterator]()、String.prototype\[@@iterator]()、Map.prototype\[@@iterator]()、Set.prototype\[@@iterator]()

摘自：[MDN - Symbol.iterator](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/iterator)

<font size=4>**Symbol.asyncIterator**</font>

Symbol.asyncIterator 符号指定了一个对象的默认异步迭代器。<font color=FF0000>如果一个对象设置了这个属性，它就是异步可迭代对象，可用于 for await...of 循环</font>。

**描述：**Symbol.asyncIterator 是一个用于访问对象的 @@asyncIterator 方法的 <font color=FF0000>内建符号</font>。<font color=FF0000>一个异步可迭代对象 <font size=4>**必须**</font> 要有Symbol.asyncIterator属性</font>。

**内建异步可迭代对象：**<font color=FF0000>目前没有默认设定了 [Symbol.asyncIterator] 属性的JavaScript内建的对象</font>。不过，<mark>WHATWG（网页超文本应用技术工作小组）Streams会被设定为第一批异步可迭代对象，[Symbol.asyncIterator] 最近已在设计规范中落地</mark>。

摘自：[MDN - Symbol.asyncIterator](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/asyncIterator)

<font size=4>**Symbol.species**</font>

Symbol.species 是个 “函数值属性” ，其被构造函数用以创建派生对象。

**描述：**species 访问器属性 <font color=FF0000 size=4>**允许 子类覆盖对象的默认构造函数**</font>。**注：**具体使用参见 下面补充的 get Array[@@species]

摘自：[MDN - Symbol.species](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/species)

##### 补充：get Array[@@species] 

<font color=FF0000>**Array[@@species] 访问器属性**</font> <font color=FF0000>返回 Array 的 **构造函数**</font>。

- **语法**

  ```js
  Array[Symbol.species]
  ```

- **返回值：**<font color=FF0000>Array 的构造函数</font>。

- **描述：**species 访问器属性返回 Array 对象的默认构造函数。<font color=FF0000>子类的构造函数可能会覆盖并改变构造函数的赋值</font>。

- **示例：**species 属性返回默认构造函数，它用于 Array 对象的构造函数 Array：

  ```js
  Array[Symbol.species]; // function Array()
  ```

  在继承类的对象中（例如你自定义的数组 MyArray ），MyArray 的 species 属性返回的是 MyArray 这个构造函数. 然而你可能想要覆盖它，以便在你继承的对象 MyArray 中返回父类的构造函数 Array :

  ```js
  class MyArray extends Array {
    // 重写 MyArray 的 species 属性到父类 Array 的构造函数
    static get [Symbol.species]() { return Array; }
  }
  ```

  **注：**这里的示例使用场景，参看 [现代JS教程 - 扩展内建类](https://zh.javascript.info/extend-natives)

摘自：[MDN - get Array[@@species]](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/@@species)

**类似的还有：**get Map[@@species]、get Set[@@species]、get TypedArray[@@species]、get ArrayBuffer[@@species]、get RegExp[@@species]

<font size=4>**Symbol.match**</font>

Symbol.match <font color=FF0000>指定了 匹配的是 正则表达式 而不是 字符串</font>。<font color=FF0000>**String.prototype.match() 方法会调用此函数**</font>。示例如下：

```js
const regexp1 = /foo/;
// console.log('/foo/'.startsWith(regexp1));
// expected output (Chrome): Error: First argument to String.prototype.startsWith must not be a regular expression
// expected output (Firefox): Error: Invalid type: first can't be a Regular Expression
// expected output (Safari): Error: Argument to String.prototype.startsWith cannot be a RegExp

regexp1[Symbol.match] = false; // 注：经过测试发现，如果regexp1[Symbol.match]不设置为false（默认值 或 设置为true），下面运行时均会报错

console.log('/foo/'.startsWith(regexp1)); // expected output: true
console.log('/baz/'.endsWith(regexp1)); // expected output: false
```

**描述：**此函数还用于标识对象是否具有正则表达式的行为。<font color=FF0000>比如， String.prototype.startsWith()，String.prototype.endsWith() 和 String.prototype.includes() **这些方法会检查其第一个参数是否是正则表达式，是正则表达式就抛出一个TypeError**</font>。现在，<font color=FF0000>如果 match symbol 设置为 false（ <font size=4>**或者一个 假值 falsy**</font> ）</font>，就表示该对象不打算用作正则表达式对象。

摘自：[MDN - Symbol.match](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/match)

<font size=4>**Symbol.replace**</font>

Symbol.replace 这个属性 <font color=FF0000>指定了当一个字符串替换所匹配字符串时所调用的方法</font>。String.prototype.replace() 方法会调用此方法。示例如下：

```js
class Replace1 {
  constructor(value) { this.value = value; }
  [Symbol.replace](string) { return `s/${string}/${this.value}/g`; }
}

console.log('foo'.replace(new Replace1('bar'))); // expected output: "s/foo/bar/g" 注：'bar'外面封装了一层new Replace1
```

更多信息， 详见 RegExp.prototype\[@@replace]() 和 String.prototype.replace()。

摘自：[MDN - Symbol.replace](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/replace)

<font size=4>**Symbol.split**</font>

Symbol.split <font color=FF0000>指向 一个 <font size=4>**正则表达式的索引处**</font> 分割字符串的方法</font>。 这个方法<font color=FF0000>通过 String.prototype.split() 调用</font>。<mark>注：这里表达有点问题，感觉应该是 String.prototype.split() 会调用 Symbol.split</mark>

```js
class Split1 {
  constructor(value) { this.value = value; }
  [Symbol.split](string) {
    const index = string.indexOf(this.value);
    return `${this.value}${string.substr(0, index)}/${string.substr(index + this.value.length)}`;
  }
}

console.log('foobar'.split(new Split1('foo'))); // expected output: "foo/bar"
```

详情请参阅RegExp.prototype\[@@split]() 和String.prototype.split().

摘自：[MDN - Symbol.split](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/split)

<font size=4>**Symbol.toStringTag**</font>

Symbol.toStringTag 是一个内置 symbol，它 <font color=FF0000>通常作为对象的属性键使用</font>，<font color=FF0000>对应的属性值应该为字符串类型</font>，<font color=FF0000>这个字符串用来表示该对象的自定义类型标签</font>，通常只有内置的 Object.prototype.toString() 方法会去读取这个标签并把它包含在自己的返回值里。(<mark>注，这里说成：使用 Object.prototype.toString() 方法时，会自动调用 Symbol.toStringTag，似乎更好；否则太让人费解了... </mark>)

**描述：**<font color=FF0000 size=4>**许多内置的 JavaScript 对象类型即便没有 toStringTag 属性，也能被 toString() 方法识别并返回特定的类型标签**</font>，比如，另外，注意下面的用法，使用 call 确实很巧妙

```js
Object.prototype.toString.call('foo');     // "[object String]"
Object.prototype.toString.call([1, 2]);    // "[object Array]"
Object.prototype.toString.call(3);         // "[object Number]"
Object.prototype.toString.call(true);      // "[object Boolean]"
Object.prototype.toString.call(undefined); // "[object Undefined]"
Object.prototype.toString.call(null);      // "[object Null]"
// ... and more
```

<font color=FF0000>另外一些对象类型则不然，toString() 方法能识别它们是因为引擎为它们设置好了 toStringTag 标签</font>：

```js
Object.prototype.toString.call(new Map());         // "[object Map]"
Object.prototype.toString.call(function* () {});   // "[object GeneratorFunction]"
Object.prototype.toString.call(Promise.resolve()); // "[object Promise]"
// ... and more
```

但你自己创建的类（注：不是内建类）不会有这份特殊待遇，toString() 找不到 toStringTag 属性时只好返回默认的 Object 标签：

```js
class ValidatorClass {}

Object.prototype.toString.call(new ValidatorClass()); // "[object Object]"
```

加上 toStringTag 属性，你的类也会有自定义的类型标签了：

```js
class ValidatorClass {
  get [Symbol.toStringTag]() { return "Validator"; } // 注：注意这里的 get 方法，get [Symbol.species]()也有用到
}

Object.prototype.toString.call(new ValidatorClass()); // "[object Validator]"
```

摘自：[MDN - Symbol.toStringTag](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/toStringTag)

注：这里说的不太清楚，可以参见 [阮一峰的 ECMAScript 6 入门 - Symbol - Symbol.toStringTag](https://es6.ruanyifeng.com/#docs/symbol#Symbol-toStringTag)

对象的 Symbol.toStringTag 属性，指向一个方法。在该对象上面调用 Object.prototype.toString 方法时，如果这个属性存在，它的返回值会出现在 toString 方法返回的字符串之中，表示对象的类型。也就是说，这个属性可以用来定制 [object Object] 或 [object Array] 中 object 后面的那个字符串。

```javascript
// 例一
({[Symbol.toStringTag]: 'Foo'}.toString()) // "[object Foo]"

// 例二
class Collection {
  get [Symbol.toStringTag]() { return 'xxx'; }
}
let x = new Collection();
Object.prototype.toString.call(x) // "[object xxx]"
```

ES6 新增内置对象的 Symbol.toStringTag 属性值如下。

- **JSON[Symbol.toStringTag]：**'JSON'
- **Math[Symbol.toStringTag]：**'Math'
- **Module 对象 M[Symbol.toStringTag]：**'Module'
- **ArrayBuffer.prototype[Symbol.toStringTag]：**'ArrayBuffer'
- **DataView.prototype[Symbol.toStringTag]：**'DataView'
- **Map.prototype[Symbol.toStringTag]：**'Map'
- **Promise.prototype[Symbol.toStringTag]：**'Promise'
- **Set.prototype[Symbol.toStringTag]：**'Set'
- **%TypedArray%.prototype[Symbol.toStringTag]：**'Uint8Array'等
- **WeakMap.prototype[Symbol.toStringTag]：**'WeakMap'
- **WeakSet.prototype[Symbol.toStringTag]：**'WeakSet'
- **%MapIteratorPrototype%[Symbol.toStringTag]：**'Map Iterator'
- **%SetIteratorPrototype%[Symbol.toStringTag]：**'Set Iterator'
- **%StringIteratorPrototype%[Symbol.toStringTag]：**'String Iterator'
- **Symbol.prototype[Symbol.toStringTag]：**'Symbol'
- **Generator.prototype[Symbol.toStringTag]：**'Generator'
- **GeneratorFunction.prototype[Symbol.toStringTag]：**'GeneratorFunction'
- 注，上面类似的还有：
  -  **globalThis[Symbol.toStringTag] /  window[Symbol.toStringTag]：**'global' / 'window'
  - **XMLHttpRequest.prototype[Symbol.toStringTag]：**'XMLHttpRequest'

摘自： [阮一峰的 ECMAScript 6 入门 - Symbol - Symbol.toStringTag](https://es6.ruanyifeng.com/#docs/symbol#Symbol-toStringTag)

**补充：** toString 和 typeof、instanceof 的对比

| 操作        | 用于                                                       | 返回值       |
| :---------- | :--------------------------------------------------------- | :----------- |
| typeof      | 原始数据类型                                               | string       |
| {}.toString | 原始数据类型，内建对象，包含 Symbol.toStringTag 属性的对象 | string       |
| instanceof  | 对象                                                       | true / false |

正如我们所看到的，<font color=FF0000>从技术上讲，toString 是一种“更高级的” typeof</font>。

toString 就类似于“磕了药似的 typeof”，不仅能检查原始数据类型，而且适用于内建对象，更可贵的是还支持自定义。所以，如果我们想要获取内建对象的类型，并希望把该信息以字符串的形式返回，而不只是检查类型的话，我们可以用 {}.toString.call 替代 instanceof

摘自：[现代JS教程 - 类检查："instanceof"](https://zh.javascript.info/instanceof)

<font size=4>**Symbol.toPrimitive**</font>

Symbol.toPrimitive 是一个内置的 Symbol 值，它 是作为对象的函数值属性存在的，<font color=FF0000 size=4>**当一个对象转换为对应的原始值时，会调用此函数**</font>。示例如下：

```js
const object1 = {
  [Symbol.toPrimitive](hint) {
    if (hint === 'number') { return 42; }
    return null;
  }
};
console.log(+object1); // expected output: 42
```

**描述：**<font color=FF0000 size=4>**在 Symbol.toPrimitive 属性（用作函数值）的帮助下，一个对象可被转换为原始值**</font>。<font color=FF0000>**该函数被调用时，会被传递一个字符串参数 hint ，表示要转换到的原始值的预期类型**</font>。<font color=FF0000>hint 参数的取值是 "number"、"string" 和 "default" 中的任意一个</font>。注：这里的 hint 是：

> a type of the primitive the object is going to be converted to.
>
> 摘自：[Understanding JavaScript type conversions](https://dev.to/antonmelnyk/understanding-javascript-type-conversions-43n)

更多的，可以参见上面 Object.prototype.valueOf() 和 Object.prototype.toString() 的内容。

**示例：**下面的例子展示了， Symbol.toPrimitive 属性是如何干扰一个对象转换为原始值时输出的结果的。

```js
// 一个没有提供 Symbol.toPrimitive 属性的对象，参与运算时的输出结果
var obj1 = {};
console.log(+obj1);     // NaN
console.log(`${obj1}`); // "[object Object]"
console.log(obj1 + ""); // "[object Object]"

// 接下面声明一个对象，手动赋予了 Symbol.toPrimitive 属性，再来查看输出结果
var obj2 = {
  [Symbol.toPrimitive](hint) {
    if (hint == "number") { return 10; }
    if (hint == "string") { return "hello"; }
    return true; }
};
console.log(+obj2);     // 10      -- hint 参数值是 "number"
console.log(`${obj2}`); // "hello" -- hint 参数值是 "string"
console.log(obj2 + ""); // "true"  -- hint 参数值是 "default"
```

摘自：[MDN - Symbol.toPrimitive](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/toPrimitive) 另外，关于对象的初始值转换可以参看：[现代JS教程 - 对象 — 原始值转换](https://zh.javascript.info/object-toprimitive)，另外，其中也有关于 hint 的三种类型（number、string、default）使用更易懂的解释

<font size=4>**Symbol.isConcatSpreadable**</font>

内置的 Symbol.isConcatSpreadable 符号<font color=FF0000>用于配置某对象作为 Array.prototype.concat() 方法的参数时是否展开其数组元素</font>。示例如下，注意下面numeric的 Symbol.isConcatSpreadable 属性值 被设置为 false 后，再使用 concat 的效果。

```js
const alpha = ['a', 'b', 'c'];
const numeric = [1, 2, 3];
let alphaNumeric = alpha.concat(numeric);

console.log(alphaNumeric); // expected output: Array ["a", "b", "c", 1, 2, 3]

numeric[Symbol.isConcatSpreadable] = false;
alphaNumeric = alpha.concat(numeric);

console.log(alphaNumeric); // expected output: Array ["a", "b", "c", Array [1, 2, 3]]
```

**描述：**@@isConcatSpreadable 符号 (Symbol.isConcatSpreadable) <font color=FF0000>可以直接定义为 对象属性 或 继承而来，它是 **布尔类型**</font>。它 <font color=FF0000>可以控制 数组 或 类似数组 (array-like) 的对象的行为</font>：

- 对于数组对象，默认情况下：用于concat时，会按数组元素展开然后进行连接（数组元素作为新数组的元素）。重置Symbol.isConcatSpreadable 可以改变默认行为。

- 对于类似数组的对象，用于 concat 时，该对象整体作为新数组的元素，重置 Symbol.isConcatSpreadable 可改变默认行为。如下：

  ```js
  var x = [1, 2, 3];
  var fakeArray = {
    [Symbol.isConcatSpreadable]: true,
    0: "hello",
    1: "world",
    length: 2,
  }
  
  x.concat(fakeArray); // [1, 2, 3, "hello", "world"]
  ```

摘自：[MDN - Symbol.isConcatSpreadable](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/isConcatSpreadable)

<font size=4>**Symbol.hasInstance**</font>

Symbol.hasInstance <font color=FF0000>用于判断某对象是否为某构造器的实例</font>。因此你可以用它自定义 instanceof 操作符在某个类上的行为。

注：自己的理解，Symbol.hasInstance 可以用来自定义 instanceof 运算符的行为。

摘自：[MDN - Symbol.hasInstance](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/hasInstance)

对象的 Symbol.hasInstance属性，指向一个内部方法。<font color=FF0000>当其他对象使用 instanceof 运算符，判断是否为该对象的实例时，会调用这个方法</font>。<mark>比如，foo instanceof Foo 在语言内部，实际调用的是 Foo\[Symbol.hasInstance](foo)</mark>。

摘自：[JavaScript ES6 Symbol.hasInstance的理解](https://www.cnblogs.com/waitforyou/p/7080591.html)

<font size=4>**Symbol.unscopables**</font>

Symbol.unscopables 指 <font color=FF0000>用于指定对象值，其对象自身和继承的从关联对象的 with 环境绑定中排除的属性名称</font>。示例如下：

```js
const object1 = { property1: 42 };

object1[Symbol.unscopables] = { property1: true }; // 注：经测试，当property1的值为真值(truthy)时，下面with的运行会报错

with (object1) {
  console.log(property1); // expected output: Error: property1 is not defined
}
```

**描述**
可以在任何对象上定义 @@unscopables symbol ( Symbol.unscopables )，<font color=FF0000>用于排除属性名称并与 with 环境绑定在一起作为词法变量公开</font>。 <mark>请注意，如果使用 Strict mode，语句将不可用，并且可能也不需要 symbol</mark>。

在 unscopables 对象上设置属性为 true（<mark>注：根据 Symbol.match 中的特性，如果值为 falsy，则表示该对象不打算用作正则表达式对象。类似的，也经过测试，发现这里属性值为 truthy，则... </mark>），将使其 unscopable 并且因此该属性也将不会在词法环境变量中出现。 如果设置属性为 false ，则将使其可 scopable 并且该属性会出现在词法环境变量中。

**示例：**下列的代码可兼容 ES5 及以下版本。然而，在 ES6 或其后续版本中，Array.prototype.keys() 方法才会出现。意味着内部 with 环境“关键字” 存在该方法，但变量中不会存在。 也就是说，当 unscopables symbol 被展示时，内置的unscopables设置是由 Array.prototype[@@unscopables\] 展示并实现的， 一些 Array 的方法 将作为 scoped 放入 with语句中。

```js
var keys = [];
with(Array.prototype) { keys.push("something"); } // 注：如果不加上这行代码，似乎代码也可以运行...

Object.keys(Array.prototype[Symbol.unscopables]);
// ['copyWithin', 'entries', 'fill', 'find', 'findIndex', 'flat', 'flatMap', 'includes', 'keys', 'values', 'at']
```

注：可以尝试下打印 `Array.prototype[Symbol.unscopables]` 的结果：

```js
[Object: null prototype] {
  copyWithin: true,
  entries: true,
  fill: true,
  find: true,
  findIndex: true,
  flat: true,
  flatMap: true,
  includes: true,
  keys: true,
  values: true,
  at: true
}
```

所以上面的 `Object.keys(Array.prototype[Symbol.unscopables])` 的结果，也不意外。

**也可以为你自己的对象设置 unscopables：**

```js
var obj = { foo: 1, bar: 2 };
obj[Symbol.unscopables] = { foo: false, bar: true }; // 注：即在这里设置

with(obj) {
  console.log(foo); // 1
  console.log(bar); // ReferenceError: bar is not defined
}
```

摘自：[MDN - Symbol.unscopables](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/unscopables)



#### Symbol.for()

Symbol.for(key) 方法会根据给定的键 key，来从运行时的 symbol 注册表中找到对应的 symbol，<font color=FF0000>如果找到了，则返回它</font>，<font color=FF0000>否则，新建一个与该键关联的 symbol，并放入全局 symbol 注册表中</font>。

- **语法**

  ```js
  Symbol.for(key);
  ```

- **参数**
  **key：**一个字符串，<font color=FF0000>作为 symbol 注册表中与某 symbol 关联的键（同时也会作为该 symbol 的描述）</font>。
- **返回值：**<font color=FF0000>返回由给定的 key 找到的 symbol，否则就是返回新创建的 symbol</font>。

- **描述：**

  和 <font color=FF0000>Symbol() 不同的是，（ Symbol() ）用 Symbol.for() 方法创建的的 symbol 会被放入一个全局 symbol 注册表中</font>。<mark>Symbol.for() 并不是每次都会创建一个新的 symbol，它会首先检查给定的 key 是否已经在注册表中了。假如是，则会直接返回上次存储的那个。否则，它会再新建一个</mark>。

  **全局 symbol 注册表**

  symbol 注册表中的记录结构：

  | 字段名     | 字段值                          |
  | :--------- | :------------------------------ |
  | [[key]]    | 一个字符串，用来标识每个 symbol |
  | [[symbol]] | 存储的 symbol 值                |

- **示例：**

  ```js
  Symbol.for("foo"); // 创建一个 symbol 并放入 symbol 注册表中，键为 "foo"
  Symbol.for("foo"); // 从 symbol 注册表中读取键为"foo"的 symbol
  
  Symbol.for("bar") === Symbol.for("bar"); // true，证明了上面说的
  Symbol("bar") === Symbol("bar"); // false，Symbol() 函数每次都会返回新的一个 symbol
  
  var sym = Symbol.for("mario");
  sym.toString(); // "Symbol(mario)"，mario 既是该 symbol 在 symbol 注册表中的键名，又是该 symbol 自身的描述字符串
  ```

  为了防止冲突，最好给你要放入 symbol 注册表中的 symbol 带上键前缀。

  ```js
  Symbol.for("mdn.foo");
  Symbol.for("mdn.bar");
  ```

摘自：[MDN - Symbol.for()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/for)

#### Symbol.keyFor()

Symbol.keyFor(sym) 方法用来获取全局symbol 注册表中与某个 symbol 关联的键。

- **语法**

  ```js
  Symbol.keyFor(sym);
  ```

- **参数**
  sym：必选参数，需要查找键值的某个 Symbol 。
- **返回值：**<mark>如果全局注册表中查找到该symbol，则返回该symbol的key值，返回值为字符串类型。否则返回undefined</mark>

摘自：[MDN - Symbol.keyFor()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/keyFor)

#### Symbol.prototype.description（ES2019）

description 是一个<font color=FF0000>只读属性</font>，它会返回 Symbol 对象的可选描述的字符串。

**示例：**

```js
console.log(Symbol('desc').description); // expected output: "desc"

console.log(Symbol.iterator.description); // expected output: "Symbol.iterator"

console.log(Symbol.for('foo').description); // expected output: "foo"
```

**语法**

```js
Symbol('myDescription').description;
Symbol.iterator.description;
Symbol.for('foo').description;
```

**描述**
Symbol 对象可以通过一个可选的描述创建，可用于调试，但不能用于访问 symbol 本身。Symbol.prototype.description 属性可以用于读取该描述。<mark>与 Symbol.prototype.toString() 不同的是它不会包含 "Symbol()" 的字符串</mark>。

摘自：[MDN - Symbol.prototype.description](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/description)



#### Proxy

Proxy 对象用于创建一个对象的代理，从而实现基本操作的拦截和自定义（如属性查找、赋值、枚举、函数调用等）。

##### 术语

- <font color=FF0000>**handler：**包含捕捉器 ( trap ) 的占位符对象，可译为处理器对象。</font>
- <font color=FF0000>**traps：**提供属性访问的方法。这类似于操作系统中捕获器的概念。</font>
- **target：**被 Proxy 代理虚拟化的对象。它常被作为代理的存储后端。根据目标验证关于对象不可扩展性或不可配置属性的不变量（保持不变的语义）。

##### 语法

```js
const p = new Proxy(target, handler)
```

##### 参数

- **target：**<font color=FF0000>要使用 Proxy 包装的目标对象</font>（<mark>可以是任何类型的对象，包括原生数组，函数，甚至另一个代理</mark>）。
- **handler：**一个通常以函数作为属性的对象，各属性中的函数分别定义了在执行各种操作时代理 p 的行为。

##### 方法

- **Proxy.revocable()：**<font color=FF0000>创建一个可撤销的 Proxy 对象</font>。

  《现代 JS 教程》的补充

  > 一个 **可撤销** 的代理是可以被禁用的代理。
  >
  > 假设我们有一个资源，并且想随时关闭对该资源的访问。我们可以做的是将它包装成可一个撤销的代理，没有任何捕捉器。这样的代理会将操作转发给对象，并且我们可以随时将其禁用。
  >
  > 语法为：
  >
  > ```js
  > let {proxy, revoke} = Proxy.revocable(target, handler)
  > ```
  >
  > 该调用返回一个带有 proxy 和 revoke 函数的对象以将其禁用。
  >
  > 摘自：[现代 JS 教程 - Proxy 和 Reflect - 可撤销 Proxy](https://zh.javascript.info/proxy#ke-che-xiao-proxy)

##### handler 对象的方法

handler 对象是一个容纳一批特定属性的占位符对象。它包含有 Proxy 的各个捕获器（trap）。所有的捕捉器是可选的。如果没有定义某个捕捉器，那么就会保留源对象的默认行为。

- **handler.getPrototypeOf()：**Object.getPrototypeOf 方法的捕捉器。

- **handler.setPrototypeOf()：**Object.setPrototypeOf 方法的捕捉器。

- **handler.isExtensible()：**Object.isExtensible 方法的捕捉器。

- **handler.preventExtensions()：**Object.preventExtensions 方法的捕捉器。

- **handler.getOwnPropertyDescriptor()：**Object.getOwnPropertyDescriptor 方法的捕捉器。

- **handler.defineProperty()：**Object.defineProperty 方法的捕捉器。

- <font color=FF0000>**handler.has()：**in 操作符的捕捉器。</font>

- <font color=FF0000>**handler.get()：**属性读取操作的捕捉器。</font>

- <font color=FF0000>**handler.set()：**属性设置操作的捕捉器。</font>

- <font color=FF0000>**handler.deleteProperty()：**delete 操作符的捕捉器。</font>

  该方法会拦截以下操作:

  - 删除属性：delete proxy[foo] 和 delete proxy.foo
  - Reflect.deleteProperty()

- **handler.ownKeys()：**Object.getOwnPropertyNames 方法和 Object.getOwnPropertySymbols 方法的捕捉器。

- **handler.apply()：**函数调用操作的捕捉器。

- <font color=FF0000>**handler.construct()：**new 操作符的捕捉器。</font>

<font color=FF0000>一些不标准的捕捉器已经被废弃并且移除了。</font>

**基础示例**

- 在以下简单的例子中，当对象中不存在属性名时，默认返回值为 37。下面的代码以此展示了 `get` handler 的使用场景。

  ```js
  const handler = {
      get: function(obj, prop) {
          return prop in obj ? obj[prop] : 37;
      }
  };
  
  const p = new Proxy({}, handler);
  p.a = 1;
  p.b = undefined;
  
  console.log(p.a, p.b);      // 1, undefined
  console.log('c' in p, p.c); // false, 37
  ```

- **无操作转发代理**

  在以下例子中，我们使用了一个原生 JavaScript 对象，代理会将所有应用到它的操作转发到这个对象上。

  ```js
  let target = {};
  let p = new Proxy(target, {});
  
  p.a = 37;   // 操作转发到目标
  
  console.log(target.a);    // 37. 操作已经被正确地转发
  ```

摘自：[MDN - Proxy](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

**补充：**

| 内部方法               | Handler 方法             | 何时触发                                                     |
| :--------------------- | :----------------------- | :----------------------------------------------------------- |
| \[[Get]]               | get                      | 读取属性                                                     |
| \[[Set]]               | set                      | 写入属性                                                     |
| \[[HasProperty]]       | has                      | in 操作符                                                    |
| \[[Delete]]            | deleteProperty           | delete 操作符                                                |
| \[[Call]]              | apply                    | 函数调用                                                     |
| \[[Construct]]         | construct                | new 操作符                                                   |
| \[[GetPrototypeOf]]    | getPrototypeOf           | Object.getPrototypeOf                                        |
| \[[SetPrototypeOf]]    | setPrototypeOf           | Object.setPrototypeOf                                        |
| \[[IsExtensible]]      | isExtensible             | Object.isExtensible                                          |
| \[[PreventExtensions]] | preventExtensions        | Object.preventExtensions                                     |
| \[[DefineOwnProperty]] | defineProperty           | Object.defineProperty, Object.defineProperties               |
| \[[GetOwnProperty]]    | getOwnPropertyDescriptor | Object.getOwnPropertyDescriptor, for..in, Object.keys/values/entries |
| \[[OwnPropertyKeys]]   | ownKeys                  | Object.getOwnPropertyNames, Object.getOwnPropertySymbols, for..in, Object/keys/values/entries |

摘自：[现代JavaScript教程 - Proxy 和 Reflect](https://zh.javascript.info/proxy)



#### instanceof

instanceof 运算符用于检测构造函数的 prototype 属性（即： `constructor.prototype ` ）是否出现在某个实例对象( object )的原型链上

- **语法**

  ```js
  object instanceof constructor
  ```

- **参数**

  - object：某个实例对象
  - constructor：某个构造函数

摘自：[MDN - instanceof](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/instanceof)



#### extends

extends 关键字用于类声明或者类表达式中，以创建一个类，该类是另一个类的子类。

- **语法：**

  ```js
  class ChildClass extends ParentClass { ... }
  ```

- **描述：**extends 关键字用来创建一个普通类或者内建对象的子类。继承的 `.prototype` 必须是一个 Object 或者 null 。

摘自：[MDN - extends](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes/extends)



#### super

super 关键字<font color=FF0000>用于访问和调用一个对象的父对象上的函数</font>。super.prop 和 super[expr] 表达式在类和对象字面量任何方法定义中都是有效的。

- **语法**

  ```js
  super([arguments]);  // 调用 父对象/父类 的构造函数
  
  super.functionOnParent([arguments]);  // 调用 父对象/父类 上的方法
  ```

- **描述：**<font color=FF0000>在构造函数中使用时，super 关键字将单独出现，并且必须在使用 this 关键字之前使用</font>。<font color=FF0000>super 关键字也可以用来调用父对象上的函数</font>。

摘自：[MDN - super](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/super)



#### with

> <mark style=background-color:aqua>**不建议使用`with`语句，因为它可能是混淆错误和兼容性问题的根源**</mark>

with语句 扩展一个语句的作用域链。

- **语法：**

  ```js
  with (expression) {
      statement
  }
  ```

  - **expression：**将给定的表达式添加到在评估语句时使用的作用域链上。表达式周围的括号是必需的。
  - **statement：**任何语句。要执行多个语句，请使用一个块语句 ({ ... })对这些语句进行分组。

- 描述
  JavaScript查找某个未使用命名空间的变量时，会通过作用域链来查找，作用域链是跟执行代码的context或者包含这个变量的函数有关。'with'语句将某个对象添加到作用域链的顶部，如果在statement中有某个未使用命名空间的变量，跟作用域链中的某个属性同名，则这个变量将指向这个属性值。如果沒有同名的属性，则将拋出ReferenceError异常。



#### Set

Set 对象允许你存储任何类型的唯一值，无论是原始值或者是对象引用。

<font color=FF0000>NaN和undefined都可以被存储在Set 中， NaN之间被视为相同的值（NaN被认为是相同的，尽管 NaN !== NaN）。</font>

- **构造方法：**
  **Set()：**创建一个新的Set对象。

  <mark>补充：set() 可以添加Array或者String（也是一种字符的Array）作为参数</mark>

- **静态属性：**
  **get Set[@@species]：**构造函数用来创建派生对象.

- **实例属性：**
  <font color=FF0000>**Set.prototype.size：**返回 Set 对象中的值的个数</font>

- **实例方法：**

  - **Set.prototype.<font color=FF0000>add</font>(value)：**<font color=FF0000>在Set对象尾部添加一个元素</font>。返回该Set对象。
  - **Set.prototype.<font color=FF0000>clear()</font>：**<font color=FF0000>移除Set对象内的所有元素</font>。
  - **Set.prototype.<font color=FF0000>delete(value)</font>：**<font color=FF0000>移除Set中与这个值相等的元素</font>，<font color=FF0000>返回Set.prototype.has(value)在这个操作前会返回的值</font>（即如果该元素存在，返回true，否则返回false）。Set.prototype.has(value)在此后会返回false。
  - **Set.prototype.entries()：**返回一个新的迭代器对象，该对象包含Set对象中的按插入顺序排列的所有元素的值的[value, value]数组。为了使这个方法和Map对象保持相似， 每个值的键和值相等。
  - **Set.prototype.forEach(callbackFn[, thisArg])：**按照插入顺序，为Set对象中的每一个值调用一次callBackFn。如果提供了thisArg参数，回调中的this会是这个参数。
  - **Set.prototype.<font color=FF0000>has(value)</font>：**<font color=FF0000>返回一个布尔值，表示该值在Set中存在与否</font>。
  - **Set.prototype.keys()：**与values()方法相同，返回一个新的迭代器对象，该对象包含Set对象中的按插入顺序排列的所有元素的值。
  - **Set.prototype.values()：**返回一个新的迭代器对象，该对象包含Set对象中的按插入顺序排列的所有元素的值。
  - **Set.prototype\[@@iterator]()：**返回一个新的迭代器对象，该对象包含Set对象中的按插入顺序排列的所有元素的值。

摘自：[MDN - Set](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)

#### weakset

weakset只能放入对象，同时对于对象是弱引用，当对象被回收时，不会考虑是否被weakset引用

weakset不可遍历



#### DocumentFragment 文档片段 / 文档碎片

DocumentFragment，<font color=FF0000>文档片段 **接口**</font>，一个<font color=FF0000>没有父对象的最小文档对象</font>。它被作为一个轻量版的 Document 使用，就像标准的document 一样，存储由节点（nodes）组成的文档结构。<font color=FF0000>与 document 相比，最大的区别是DocumentFragment **不是真实 DOM 树的一部分，它的变化不会触发 DOM 树的重新渲染，且不会导致性能等问题**。</font>

最常用的方法是使用文档片段作为参数（例如，任何 Node 接口类似 Node.appendChild 和 Node.insertBefore 的方法），这种情况下被添加（append）或被插入（inserted）的是片段的所有子节点, 而非片段本身。<font color=FF0000>因为所有的节点会被一次插入到文档中，而这个操作仅发生一个重渲染的操作，而不是每个节点分别被插入到文档中，因为后者会发生多次重渲染的操作。</font>

该接口在 Web 组件（Web components）中也非常有用：\<template> 元素在其 HTMLTemplateElement.content 属性中包含了一个 DocumentFragment。

<font color=FF0000>可以使用document.createDocumentFragment 方法或者构造函数来创建一个空的 DocumentFragment。</font>

**属性：**该接口没有特殊的属性，其<font color=FF0000 size=4>**属性都继承自 Node**</font>。并补充了 Node 接口中的属性。原文这里下面列举了几个属性，这里略。

**构造函数：DocumentFragment()** 返回一个空的 DocumentFragment 对象。

**方法：**<font color=FF0000>该接口继承 Node 的全部方法</font>，并实现了 ParentNode 接口中的方法。

- **DocumentFragment.querySelector()**：返回在DocumentFragment中以文档顺序排列的第一个符合指定选择器的Element节点。
- **DocumentFragment.querySelectorAll()**：返回在DocumentFragment中所有的符合指定选择器的Element节点组成的NodeList数组。
- **DocumentFragment.getElementById()**：返回在DocumentFragment中以文档顺序排列的第一个符合指定ID选择器的Element节点。与Document.getElementById()作用相同。

**示例如下：**

```html
<ul id="list"></ul>

<script>
  const list = document.querySelector('#list');
  const fruits = ['Apple', 'Orange', 'Banana', 'Melon'];
  
  const fragment = document.createDocumentFragment();
  
  fruits.forEach(fruit => {
    const li = document.createElement('li');
    li.innerHTML = fruit;
    fragment.appendChild(li);
  });
  
  list.appendChild(fragment);
</script>
```

摘自：[MDN - DocumentFragment](https://developer.mozilla.org/zh-CN/docs/Web/API/DocumentFragment)



#### history api

DOM window 对象通过 history 对象提供了对浏览器的会话历史的访问。<mark>它暴露了很多有用的方法和属性，允许你在用户浏览历史中向前和向后跳转</mark>，同时——从HTML5开始——提供了对<font color=FF0000>history栈</font>中内容的操作。

##### **使用 back(),  forward()和  go() 方法来完成在用户历史记录中向后和向前的跳转。**

- **window.history.back()：**在history中向后跳转，这和用户点击浏览器回退按钮的效果相同。
- **window.history.forward()：**在history中向前跳转（如同用户点击了前进按钮）
- **window.history.go(n)：**可以用 go() 方法载入到会话历史中的某一特定页面， 通过与当前页面相对位置来标志 (<font color=FF0000>当前页面的相对位置标志为0</font>)
  - **window.history.go(-1)：**向后移动一个页面，等同于调用 back()
  - **window.history.go(1)：**向前移动一个页面, 等同于调用了 forward()

HTML5引入了 <mark style="background: fuchsia">**history.pushState()** </mark>和<mark style="background: aqua"> **history.replaceState()**</mark> 方法，它们分别可以<mark style="background: fuchsia">**添加**</mark>和<mark style="background: aqua">修改</mark>历史记录条目。<mark>这些方法通常与window.onpopstate 配合使用</mark>。
使用 history.pushState() 可以改变referrer，它在用户发送 XMLHttpRequest 请求时在HTTP头部使用，改变state后创建的 XMLHttpRequest 对象的referrer都会被改变。因为referrer是标识创建  XMLHttpRequest 对象时 this 所代表的window对象中document的URL。

##### history.pushState()

**用法：**history.pushState() 方法向当前浏览器会话的历史堆栈中添加一个状态（state）。

**语法**

```js
history.pushState(state, title[, url])
```

**参数**

- **state：**状态对象是一个JavaScript对象，它与pushState()创建的新历史记录条目相关联。 每当用户导航到新状态时，都会触发popstate 事件，并且该事件的状态属性包含历史记录条目的状态对象的副本。<mark>状态对象可以是任何可以序列化的对象</mark>。 
- **title：**当前大多数浏览器都忽略此参数，尽管将来可能会使用它。 在此处传递空字符串应该可以防止将来对方法的更改。 或者，您可以为要移动的状态传递简短的标题。
- **url：** <font color=FF0000>可选</font>，新历史记录条目的URL由此参数指定。 请注意，浏览器不会在调用pushState() 之后尝试加载此URL，但可能会稍后尝试加载URL，例如在用户重新启动浏览器之后。 <font color=FF0000>新的URL**不必是绝对的**。 如果是相对的，则相对于当前URL进行解析</font>。 新网址必须与当前网址相同 origin； 否则，pushState()将引发异常。 如果未指定此参数，则将其设置为文档的当前URL。

摘自：[MDN - History.pushState()](https://developer.mozilla.org/zh-CN/docs/Web/API/History/pushState)

##### history.replaceState()

replaceState()方法使用状态对象（state objects）, title 和 URL 作为参数，<font color=FF0000> 修改当前历史记录实体</font>，如果你想更新当前的state对象或者当前历史实体的URL来响应用户的的动作的话这个方法将会非常有用。

**语法**

```js
history.replaceState(stateObj, title[, url]);
```

**参数**

- **stateObj：**状态对象是一个JavaScript对象，它与传递给 replaceState 方法的历史记录实体相关联.
- **title：**<mark>大部分浏览器忽略这个参数</mark>, 将来可能有用. 在此处传递空字符串应该可以防止将来对方法的更改。或者，您可以为该状态传递简短标题
- **url：**可选，历史记录实体的URL. 新的URL跟当前的URL必须是同源; 否则 replaceState 抛出一个异常.

摘自：[MDN - History.replaceState()](https://developer.mozilla.org/zh-CN/docs/Web/API/History/replaceState)



#### window事件

- <font size=4>**load：**</font>当整个页面及所有依赖资源如样式表和图片都已完成加载时，将触发load事件。

  事件处理程序属性：onload

  摘自：[MDN - load](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/load_event)

  **补充：GlobalEventHandlers.onload**

  GlobalEventHandlers mixin 的 onload 属性是一个事件处理程序<font color=FF0000>用于处理Window, XMLHttpRequest, \<img> 等元素的加载事件，当资源已加载时被触发</font>。

  **语法**

  ```js
  window.onload = funcRef;
  ```

  当 window load事件触发时，funcRef 方法会被调用；funcRef 是窗口加载事件触发时调用的处理函数。

  摘自：[MDN web docs - GlobalEventHandlers.onload](https://developer.mozilla.org/zh-CN/docs/Web/API/GlobalEventHandlers/onload)

- <font size=4>**unload：**</font>当文档或一个子资源正在被卸载时, 触发 unload事件。

  事件处理程序属性：onunload

  <font color=FF0000>**unload时间在beforeunload 和 pagehide 事件之后出发**</font>

  请注意：unload事件也遵循文档树:父iframe会在子iframe卸载前卸载

  摘自：[MDN - unload](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/unload_event)

- <font size=4>**beforeunload：**</font>当浏览器窗口关闭或者刷新时，会触发beforeunload事件。<font color=FF0000>当前页面不会直接关闭，可以点击确定按钮关闭或刷新，也可以取消关闭或刷新</font>。

  事件处理程序属性：onbeforeunload

  事件使网页能够触发一个确认对话框，询问用户是否真的要离开该页面。如果用户确认，浏览器将导航到新页面，否则导航将会取消。

  摘自：[MDN - Window: beforeunload event](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/beforeunload_event)

- <font size=4>**pagehide：**</font>当浏览器在显示与会话历史记录不同的页面的过程中隐藏当前页面时, pagehide(页面隐藏)事件会被发送到一个Window 。例如，<font color=FF0000>当用户单击浏览器的**“后退”**按钮时，当前页面在显示上一页之前会收到一个pagehide(页面隐藏)事件</font>。

  事件处理程序属性：onpagehide

  摘自：[MDN - Window: 页面隐藏事件(pagehide event)](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/pagehide_event)

- <font size=4>**pageshow：**</font>当一条<font color=FF0000>会话历史记录被执行的时候将会触发页面显示(pageshow)事件</font>。<font color=FF0000>这包括了后退/前进按钮操作，同时也会在onload 事件触发后初始化页面时触发</font>

  事件处理程序属性：onpageshow

  摘自：[MDN - pageshow]()

- <font size=4>**popstate：**</font>浏览器在被点击“后退”或者“前进"按钮后触发

  **补充：**

  事件处理程序属性：onpopstate

  <font color=FF0000>当活动历史记录条目更改时，将触发 popstate 事件</font>。如果被激活的历史记录条目是通过对 history.pushState() 的调用创建的，或者受到对 history.replaceState() 的调用的影响，popstate事件的state属性包含历史条目的状态对象的副本。

  <font color=FF0000>需要注意的是<font size=4>**调用 history.pushState() 或 history.replaceState() 不会触发popstate事件**</font></font>。<font color=FF0000>只有在做出浏览器动作时，才会触发该事件</font>，如用户点击浏览器的回退按钮（或者在Javascript代码中调用history.back() 或者 history.forward() 方法）

  摘自：[MDN - popstate](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/popstate_event)

- <font size=4>**error：**</font><font color=FF0000>当一个资源加载失败或无法使用时，会在Element对象上触发error事件</font>。<mark>例如当脚本执行错误、或图片无法找到或图片无效时</mark>。

  不可以冒泡，也不可以取消（preventDefault）

  事件处理程序属性：onerror

  摘自：[MDN - Window: error event](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/error_event)

  **补充：GlobalEventHandlers.onerror**

  混合事件 GlobalEventHandlers 的 onerror 属性是用于处理 error 的事件

  Error事件的事件处理程序，在各种目标对象的不同类型错误被触发：

  - <font color=FF0000>当JavaScript运行时错误（包括语法错误）发生时</font>，window会触发一个ErrorEvent接口的error事件，并执行window.onerror()。
  - <font color=FF0000>当一项资源（如\<img>或\<script>）加载失败，加载资源的元素会触发一个Event接口的error事件</font>，并执行该元素上的onerror()处理函数。这些error事件不会向上冒泡到window，不过（至少在Firefox中）能被单一的window.addEventListener捕获。

  加载一个全局的error事件处理函数可用于自动收集错误报告。

  **语法**

  <font color=FF0000>由于历史原因，window.onerror和element.onerror接受不同的参数</font>。

  - **window.onerror**

    ```js
    window.onerror = function(message, source, lineno, colno, error) { ... }
    ```
    函数参数：

    - message：错误信息（字符串）。可用于HTML onerror=""处理程序中的event。
    - source：发生错误的脚本URL（字符串）
    - lineno：发生错误的行号（数字）
    - colno：发生错误的列号（数字）
    - error：Error对象（对象）

    若该函数返回true，则阻止执行默认事件处理函数。

  - **window.addEventListener('error')**

    ```js
    window.addEventListener('error', function(event) { ... })
    ```

    ErrorEvent 类型的event包含有关事件和错误的所有信息。

  - **element.onerror**

    ```js
    element.onerror = function(event) { ... }
    ```

    element.onerror使用单一Event参数的函数作为其处理函数。

- <font size=4>**resize：**</font>文档视图调整大小时会触发 **resize** 事件。

- <font size=4>**mousemove：**</font>当指针设备( 通常指鼠标 )在元素上移动时, mousemove 事件被触发。

  可以冒泡，也可以取消（preventDefault）

  事件处理程序属性：onmousemove

  摘自：[MDN - mousemove](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/mousemove_event)

- <font size=4>**mousedown：**</font>mousedown 事件在指针设备按钮按下时触发。

  可以冒泡，也可以取消（preventDefault）

  事件处理程序属性：onmousedown

  摘自：[MDN - mousedown](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/mousedown_event)

- <font size=4>**mouseup：**</font>当指针在元素中时， mouseup事件在指针设备（如鼠标或触摸板）按钮放开时触发。mouseup 事件与mousedown 事件相反。

  可以冒泡，也可以取消（preventDefault）

  事件处理程序属性：onmouseup

  摘自：[MDN - mouseup](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/mouseup_event)

- <font size=4>**mouseover：**</font>当一个指针设备（比如鼠标或触控板）的光标被移动到一个元素上或这个元素的子元素上，这时mouseover事件将触发。

  可以冒泡，也可以取消（preventDefault）

  事件处理程序属性：onmouseover

  摘自：[MDN - Element: mouseover event](https://developer.mozilla.org/en-US/docs/Web/API/Element/mouseover_event)

- <font size=4>**mouseout：**</font>当移动指针设备（通常是鼠标），使指针不再包含在这个元素或其子元素中时，mouseout 事件被触发。当指针从一个元素移入其子元素时，mouseout 也会被触发，因为子元素遮盖了父元素的可视区域。

  可以冒泡，也可以取消（preventDefault）

  事件处理程序属性：onmouseout

  摘自：[MDN - Element: mouseout 事件](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/mouseout_event)

- <font size=4>**mouseenter：**</font>当定点设备（通常指鼠标）移动到元素上时就会触发 mouseenter 事件

  类似 mouseover，它们两者之间的差别是 mouseenter 不会冒泡（bubble），也就是说当指针从它的子层物理空间移到它的物理空间上时不会触发

  | ![](https://developer.mozilla.org/@api/deki/files/5908/=mouseenter.png) |    ![](https://i.loli.net/2021/07/19/tivKCq5axQ3mBfH.png)    |
  | :----------------------------------------------------------: | :----------------------------------------------------------: |
  | (mouseenter事件)触发时，会向层次结构的每个元素发送一个mouseenter事件。当指针到达文本时，此处将4个事件发送到层次结构的四个元素。 | 一个单独的mouseover事件被发送到DOM树的最深层元素，然后它将层次结构向上冒泡，直到它被处理程序取消或到达根目录。 |

  对于深层次结构，发送的mouseenter事件数量可能非常大并且会导致严重的性能问题。在这种情况下，最好是监听鼠标悬停事件。（可使用chrome开发者工具选项卡中的Performance进行性能测试）

  结合其对称事件, mouseleave, mouseenter DOM事件的行为方式与CSS  :hover 伪类非常相似。

  <font color=FF0000>**不可以**冒泡，也**不可以**取消（preventDefault</font>

  事件处理程序属性：onmousenter

  摘自：[MDN - mouseenter](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/mouseenter_event)

- <font size=4>**mouseleave：**</font>指点设备（通常是鼠标）的指针移出某个元素时，会触发mouseleave事件。

  <font color=FF0000>mouseleave  和 mouseout 是相似的，但是两者的不同在于**mouseleave 不会冒泡而mouseout 会冒泡**。</font><mark>这意味着当指针离开元素及其所有后代时，会触发mouseleave，而当指针离开元素或离开元素的后代（即使指针仍在元素内）时，会触发mouseout。</mark>

  | ![mouseenter.png](https://developer.mozilla.org/@api/deki/files/5910/=mouseleave.png) | ![mouseover.png](https://developer.mozilla.org/@api/deki/files/5911/=mouseout.png) |
  | :----------------------------------------------------------: | :----------------------------------------------------------: |
  | 当离开它们时，一个mouseleave事件被发送到层次结构的每个元素。当指针从文本移动到这里表示的最外面的div之外的区域时，这里4个事件会发送到层次结构的四个元素。 | 一个单一的鼠标事件mouseout被发送到DOM树最深的元素，然后它冒泡层次，直到它被处理程序取消或到达根。 |

  <font color=FF0000>**不可以**冒泡，也**不可以**取消（preventDefault）</font>

  事件处理程序属性：onmouseleave

  摘自：[MDN - mouseleave](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/mouseleave_event)

- <font size=4>**dragstart：**</font><font color=FF0000>当用户开始拖动一个元素或者一个选择文本的时候 dragstart 事件就会触发</font>。

  可以冒泡，可以取消。事件目标对象：Document、Element。事件接口：DragEvent

  另外，该事件有一些属性；由于有不少，这里略。
  
  摘自：[MDN - dragstart](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/dragstart_event)

- <font size=4>**dragsend：**</font><font color=FF0000>拖放事件在拖放操作结束时触发</font>（通过<font color=FF0000>释放鼠标按钮或单击escape键</font>）

  可以冒泡，不可以取消。事件目标对象：Document、Element。事件接口：DragEvent

  另外，该事件有一些属性；由于有不少，这里略。

  摘自：[MDN - dragstart](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/dragstart_event)

- <font size=4>**rejectionhandled：**</font><font color=FF0000>当 Promise 被 rejected 且 **有rejection处理器** 时，会在 <font size=4>**全局触发**</font> rejectionhandled 事件</font>（通常是发生在window 下，但是也可能发生在 Worker 中）。应用于调试一般应用回退。当 Promise 被 rejected 且没有 rejection 处理器处理时会触发 unhandledrejection 事件。这两个事件协同工作

  不可以冒泡，不可以取消

  摘自：[MDN - Window: rejectionhandled event](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/rejectionhandled_event)

- <font size=4>**unhandledrejection：**</font><font color=FF0000>当Promise 被 reject 且 **没有 reject 处理器** 的时候，会触发 unhandledrejection 事件（注：和rejectionhandled 一样，在全局作用域触发 ）</font>；这可能发生在 window 下，但也可能发生在 Worker 中。 这对于调试回退错误处理非常有用。

  不可以冒泡，可以取消（preventDefault）

  事件处理程序属性：WindowEventHandlers.onunhandledrejection

  摘自：[MDN - unhandledrejection](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/unhandledrejection_event)

- <font size=4>**transitionend：**</font><font color=FF0000>transitionend 事件会在 CSS transition 结束后触发</font>。<mark>当transition完成前移除transition时，比如移除css的transition-property 属性，事件将不会被触发.如在transition完成前设置  display 为"none"，事件同样不会被触发</mark>。

  不可以冒泡，可以取消（preventDefault）

  事件处理器属性：ontransitionend

  transitionend<font color=FF0000> **事件是双向触发的**</font>：<mark>当完成到转换状态的过渡，以及完全恢复到默认或非转换状态时都会触发</mark>。 如果没有过渡延迟或持续时间，即两者的值都为 0s 或者都未声明， 则不发生过渡，并且任何过渡事件都不会触发。如果触发了 transitioncancel 事件，则transitionend 事件不会触发。

  摘自：[MDN - transitionend](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/transitionend_event)

- <font size=4>**transitioncancel：**</font>transitioncancel事件会在CSS transition被取消时触发

  可以冒泡，不可以取消（preventDefault）

  摘自：[MDN - HTMLElement: transitioncancel event](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/transitioncancel_event)

- <font size=4>**animationstart：**</font><font color=FF0000>animationstart 事件会在 CSS 动画开始时触发</font>。 <mark>如果有 animation-delay 延时，事件会在延迟时效过后立即触发</mark>。为负数的延时时长会致使事件被触发时事件的 elapsedTime 属性值等于该时长的绝对值（并且，相应地，动画将直接播放该时长绝对值之后的动画）。

  可以冒泡，不可取消

  摘自：[MDN - animationstart](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/animationstart_event)

- <font size=4>**animationend：**</font><font color=FF0000>animationend 事件会在一个 CSS 动画完成时触发</font>（不包括完成前就已终止的情况，例如元素变得不可见或者动画从元素中移除）。

  可以冒泡，不可取消

  摘自：[MDN - animationend](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/animationend_event)

- <font size=4>**contextmenu：**</font>contextmenu 事件<mark>会在用户尝试打开上下文菜单时被触发</mark>。该事件<font color=FF0000>**通常在鼠标点击右键或者按下键盘上的菜单键时被触发**</font>，如果使用菜单键，该上下文菜单会被展示 到所聚焦元素的左下角，但是如果该元素是一棵DOM树的话，上下文菜单便会展示在当前这一行的左下角。

  可以冒泡，也可以取消（preventDefault）

  事件处理器属性：oncontextmenu

  摘自：[MDN - Element: contextmenu event](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/contextmenu_event)

- <font size=4>**input：**</font>当一个 \<input>, \<select>, 或 \<textarea> 元素的 value 被修改时，会触发 input 事件。

  可以冒泡，不可以取消。

  接口：InputEvent；事件处理器属性：GlobalEventHandlers.oninput

  <font color=FF0000 size=4>**input 事件也适用于启用了 contenteditable 的元素，以及开启了 designMode 的任意元素**</font>。<font color=FF0000>在contenteditable 和 designMode 的情况下，事件的 target 为当前正在编辑的宿主</font>。<font color=FF0000>如果这些属性应用于多个元素上，当前正在编辑的宿主为最近的父节点不可编辑的祖先元素</font>。
  <mark>对于 type=checkbox 或 type=radio 的 input 元素，每当用户切换控件（通过触摸、鼠标或键盘）时（HTML5规范），input 事件都应该触发。然而，历史事实并非如此。请检查兼容性，或使用 change 事件代替这些类型的元素</mark>。

  摘自：[MDN - input](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/input_event)

- <font size=4>**copy：**</font>当用户通过浏览器的用户界面启动复制操作时，将触发 copy 事件。

  可以冒泡，可以取消（preventDefault）

  事件处理器属性：oncopy

  摘自：[MDN - Window: copy event](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/copy_event)

- <font size=4>**cut：**</font>cut 事件在将选中内容从文档中删除并将其添加到剪贴板后触发。<mark>如果用户尝试对不可编辑内容执行剪切操作，则cut事件仍会触发，但事件对象不包含任何数据</mark>。

  可以冒泡，可以取消（preventDefault）

  接口：ClipboardEvent，事件处理器属性：oncut

  摘自：[MDN - cut](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/cut_event)

- <font size=4>**paste：**</font>当用户在浏览器用户界面发起“粘贴”操作时，会触发paste事件。

  可以冒泡，可以取消（preventDefault）

  接口：ClipboardEvent，事件处理属性：onpaste

  如果光标位于可编辑的上下文中（例如，在 \<textarea> 或者 contenteditable 属性设置为 true的元素），则默认操作是将剪贴板的内容插入光标所在位置的文档中。

  <mark>事件处理程序可以通过调用事件的 clipboardData 属性上的 getData()访问剪贴板内容。</mark>

  摘自：[MDN - Element: paste事件](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/paste_event)

- <font size=4>**submit：**</font>当表单提交的时候触发submit事件

  **注意：<font color=FF0000>submit事件只能作用于form元素，不能作用于button或者\<input type="submit"></font>**

  可以冒泡，可以取消（尽管它被指定为一个简单事件时是不可撤销的）

  事件处理器属性：GlobalEventHandlers.onsubmit

  摘自：[MDN - submit](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLFormElement/submit_event)

  <font size=4>**补充：**</font>

  submit 事件触发后默认会<font color=FF0000> **重载页面** </font>

  摘自：[Vue3官方文档 - 事件处理 - 事件修饰符](https://v3.cn.vuejs.org/guide/events.html#事件修饰符)

- <font size=4>**DOMContentLoaded：**</font>当纯HTML被完全加载以及<font color=FF0000>**（开始）**</font>解析时，DOMContentLoaded 事件会被触发，而<font color=FF0000>不必等待样式表，图片或者子框架完成加载</font>。

  JavaScript 的同步模式（非 async ）会导致 DOM 解析暂停。如果你想在用户请求页面时，首先尽可能先解析 DOM，此时你可以使用 JavaScript 异步模式 ( async )，并且优化样式表的加载。在通常模式的加载过程中，样式表的加载会与 DOM 解析并行，从而迟缓主要 HTML 文档的加载。

  摘自：[MDN - Document: DOMContentLoaded 事件](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/DOMContentLoaded_event)

  **注：**感觉 MDN 的解释并不易懂，摘抄 《现代 JS 教程》中的内容：
  > - DOMContentLoaded：浏览器已完全加载 HTML，并构建了 DOM 树，但像 \<img> 和样式表之类的外部资源可能尚未加载完成。
  > - load：浏览器不仅加载完成了 HTML，还加载完成了所有外部资源：图片，样式等。
  >
  > 摘自：[现代 JS 教程 - 页面生命周期：DOMContentLoaded，load，beforeunload，unload](https://zh.javascript.info/onload-ondomcontentloaded)
- <font size=4>**readystatechange：**</font>当文档的 readyState 属性发生改变时，会触发 readystatechange 事件。

  不可以冒泡，不可以取消

  接口：Event，事件处理属性：onreadystatechange

  摘自：[MDN - Document: readystatechange 事件](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/readystatechange_event)

- <font size=4>**focusin：**</font>当一个元素将要被聚焦时，focusin事件会被触发。focus事件和focusin事件的主要区别在于：focusin会冒泡，而focus不会。

  focusin的反事件是focusout

  可以冒泡，不可以取消

  摘自：[MDN - Element: focusin event](https://developer.mozilla.org/en-US/docs/Web/API/Element/focusin_event)

- <font size=4>**focusout：**</font>当一个元素将要失焦时，focusout 事件会被触发。blur 事件和 focusout 事件的主要区别在于：focusout 会冒泡，而blur不会。

  focusout 的反事件是 focusin

  可以冒泡，不可以取消

  摘自：[MDN - Element: focusout event](https://developer.mozilla.org/en-US/docs/Web/API/Element/focusout_event)

- <font size=4>**scroll：**</font>文档视图或者一个元素在滚动时，会触发元素的scroll事件。

  可以冒泡，不可以取消

  摘自：[MDN - scroll](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/scroll_event)

- <font size=4>**wheel：**</font>当滚动鼠标滚轮或操作其它类似输入设备时会触发**滚轮事件**。滚轮事件替换了已被弃用的非标准mousewheel事件。

  可以冒泡，可以取消

  摘自：[MDN - Element: wheel event](https://developer.mozilla.org/en-US/docs/Web/API/Element/wheel_event)

- <font size=4>**hashchange：**</font>当URL的片段标识符更改时，将触发**hashchange**事件 (跟在＃符号后面的URL部分，包括＃符号)

  可以冒泡，不可以取消

  摘自：[MDN - Window: hashchange event](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/hashchange_event)

  **注：**这个事件是 vue-router hash模式的原理

<font size=4>**补充：**</font>

- **pageshow和load区别：**
  
  pageshow 事件类似于 load 事件，<font color=FF0000>**load 事件在页面第一次加载时触发， pageshow 事件在每次加载页面时触发**，即 load 事件在页面从浏览器缓存中读取时不触发</font>。
  
  <mark>一般情况下，移动端浏览器会将当前已访问页面存入缓存中，缓存中保存着页面数据，DOM和js的状态，前进和后退操作时直接从浏览器缓存中读取页面内容，而不进行页面刷新，所以监听前进和后退操作时可用pageshow事件</mark>。
  
  <mark style="background: aqua">**触发时间：**load先触发，pageshow后触发。</mark>
  
- **pageshide和unload区别：**
  
  <font color=FF0000>pagehide 事件类似于 unload 事件，在用户离开网页时触发（如点击一个链接、刷新页面、提交表单、关闭浏览器、前进、后退等）</font>。
  
  页面缓存：pagehide触发可以缓存页面，但unload 事件触发后无法缓存。
  
  <mark style="background: aqua">**触发时间：**pagehide先触发，unload后触发。</mark>

摘自：[pageshow 和 pagehide](https://blog.csdn.net/yihanzhi/article/details/88244913)

<font size=4>**补充：**</font>

更多JS事件可以参考MDN的 [事件参考](https://developer.mozilla.org/zh-CN/docs/Web/Events)，里面列出的事件比较全面。



#### Event

Event 接口表示在 DOM 中出现的事件。

- <font color=FF0000>一些事件是由用户触发的</font>，例如鼠标或键盘事件；

- 而<font color=FF0000>其他事件常由 API 生成</font>，例如指示动画已经完成运行的事件，视频已被暂停等等。

- <font color=FF0000>事件也可以通过脚本代码触发</font>，例如对元素调用 HTMLElement.click() 方法，
- <font color=FF0000>或者定义一些自定义事件</font>，<mark>再使用EventTarget.dispatchEvent() 方法将自定义事件派发往指定的目标（target）</mark>。

**构造器**

- **Event()：**创建并返回一个 Event 对象。

属性

- **Event.bubbles：**只读，一个布尔值，<font color=FF0000>用来表示该事件是否会在 DOM 中冒泡</font>。
- **Event.cancelBubble：**<font color=FF0000>Event.stopPropagation() 的历史别名</font>。在事件处理器函数返回之前，将此属性的值设置为 true，亦可阻止事件继续冒泡。
- **Event.cancelable：**只读，一个布尔值，<font color=FF0000>**表示事件是否可以取消**</font>。
- **Event.composed：**只读，一个布尔值，<font color=FF0000>**表示事件是否可以穿过 Shadow DOM 和常规 DOM 之间的隔阂进行冒泡**</font>。
- **Event.currentTarget：**只读，对事件当前注册的目标的引用。这是一个当前计划将事件发送到的对象。它是有可能在重定向的过程中被改变的。
- **Event.deepPath：**👎一个由事件流所经过的 DOM 节点组成的数组。
- **Event.defaultPrevented：**只读，一个布尔值，<font color=FF0000>表示 event.preventDefault() 方法是否取消了事件的默认行为</font>。
- **Event.eventPhase：**<font color=FF0000>**只读，表示事件流正被处理到了哪个阶段**</font>。
- **Event.explicitOriginalTarget：**👎只读，事件的明确（explicit）原始目标（Mozilla 专有属性）。
- **Event.originalTarget：**👎只读，重设目标前的事件原始目标 （Mozilla 专有属性）。
- **Event.returnValue：**旧版 Internet Explorer 引入的一个非标准历史属性，为保证依赖此属性的网页正常运作，此属性最终被收入规范。可用 Event.preventDefault() 与 Event.defaultPrevented 代替，但由于已进入规范，也可以使用此属性。
- **Event.srcElement：**👎旧版 Internet Explorer 对 Event.target 的非标准别称。出于兼容原因，一些其他浏览器也支持此别称。
- **Event.target：**只读，<font color=FF0000 size=4>**对事件原始目标的引用**</font>，这里的原始目标指最初派发（dispatch）事件时指定的目标。
- **Event.timeStamp：**只读，<font color=FF0000>**事件创建时的时间戳**</font>（精度为毫秒）。按照规范，这个时间戳是 Unix 纪元起经过的毫秒数，但实际上，在不同的浏览器中，对此时间戳的定义也有所不同。另外，规范正在将其修改为 DOMHighResTimeStamp。
- **Event.type：**只读，<font color=FF0000 size=4>**事件的类型**</font>，不区分大小写。
- **Event.isTrusted：**只读，表示事件是由浏览器（例如用户点击）发起的（<font color=FF0000>返回true</font>），还是由脚本（使用事件创建方法，例如 Event.initEvent）发出的（<font color=FF0000>返回false</font>）。

**方法**

- **Event.createEvent()：**🗑创建一个新事件，如果使用此方法创建事件，则必须调用其自身的 initEvent() 方法，对其进行初始化。
- **Event.composedPath()：**<font color=FF0000>返回事件的路径（将在该对象上调用监听器）</font>。<mark style=background-color:hotpink>如果阴影根节点 (shadow root) 创建时 ShadowRoot.mode 值为 closed，那么路径不会包括该根节点下阴影树 (shadow tree) 的节点</mark>。
- **event.initEvent：**🗑为通过 Event.createEvent() (en-US) 创建的事件初始化。<font color=FF0000>**该方法对已经被派发的事件无效**</font>。
- **event.preventDefault：**<font color=FF0000>取消事件（**如果该事件可取消**）</font>。
- **event.stopImmediatePropagation：**<font color=FF0000>对这个特定的事件而言，没有其他监听器被调用。这个事件既不会添加到相同的元素上，也不会添加到以后将要遍历的元素上</font>（例如在捕获阶段）。
- **event.stopPropagation：**<font color=FF0000>停止冒泡，阻止事件在 DOM 中继续冒泡</font>。

摘自：[MDN - Event](https://developer.mozilla.org/zh-CN/docs/Web/API/Event)



#### Event()

Event() 构造函数, 创建一个新的事件对象 Event。

- **语法**

  ```js
  event = new Event(typeArg, eventInit);
  ```

- **参数**
  
  - **typeArg：**是DOMString 类型，<font color=FF0000>表示所创建事件的名称</font>。
  - **eventInit：**可选，是 EventInit 类型的字典，接受以下字段:
    - <font color=FF0000>**bubbles：**</font>可选，Boolean类型，<font color=FF0000>默认值为 false</font>，<font color=FF0000>表示该事件是否冒泡</font>。
    - <font color=FF0000>**cancelable：**</font>可选，Boolean类型，<font color=FF0000>默认值为 false</font>， <font color=FF0000>表示该事件能否被取消</font>。
    - **composed：**可选，Boolean类型，<font color=FF0000>默认值为 false</font>，<font color=FF0000>指示事件是否会在影子DOM根节点之外触发侦听器</font>。

摘自：[MDN - Event()](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/Event)

#### EventTarget.dispatchEvent

<font color=FF0000>向一个指定的事件目标**<font size=4> 派发 </font>一个事件**,  并 <font size=4>以合适的顺序**同步调用**</font> 目标元素相关的事件处理函数</font>。标准事件处理规则（包括事件捕获和可选的冒泡过程）同样适用于通过手动的使用dispatchEvent()方法派发的事件。

- **语法**

  ```js
  cancelled = !target.dispatchEvent(event)
  ```

- **参数**

  - **event：**是要被派发的**事件对象**。
  - **target：**<font color=FF0000>被用来初始化 事件 和 决定将会触发 目标</font>

- **返回值：**当该事件是可取消的（cancelable为true），并且至少一个该事件的 事件处理方法 调用了Event.preventDefault()，则返回值为false；否则返回true。

<mark>如果该被派发的事件的事件类型( event's type )在方法调用之前没有被经过初始化被指定</mark>，<mark>就会抛出一个 UNSPECIFIED_EVENT_TYPE_ERR 异常</mark>，<mark style=background-color:aqua>或者如果事件类型是null或一个空字符串. event handler 就会抛出未捕获的异常</mark>； 这些 event handlers 运行在一个嵌套的调用栈中： 他们会阻塞调用直到他们处理完毕，但是异常不会冒泡。

**注意**
与浏览器原生事件不同，<mark>原生事件是由DOM派发的，并通过event loop异步调用事件处理程序，而 <font color=FF0000>**dispatchEvent() 则是同步调用事件处理程序**</font></mark>。在调用dispatchEvent()后，所有监听该事件的事件处理程序将在代码继续前执行并返回。

dispatchEvent()是create-init-dispatch过程的最后一步，用于将事件调度到实现的事件模型中。可以使用Event构造函数来创建事件。

摘自：[MDN - EventTarget.dispatchEvent](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/dispatchEvent)

#### CustomEvent

CustomEvent 事件是<font color=FF0000>由程序创建的</font>，<font color=FF0000>**可以有任意自定义功能的事件**</font>。

- **构造函数**
  - **CustomEvent()：**<font color=FF0000>创建一个自定义事件</font>。

- **属性**
  - **CustomEvent.detail：**只读，任何时间初始化时传入的数据
- **方法**
  - **CustomEvent.initCustomEvent() ：**<font color=FF0000 size=4>**已废弃**</font>。初始化一个 CustomEvent 对象。如果事件已经被触发，这个方法将不会起任何作用。

摘自：[MDN - CustomEvent](https://developer.mozilla.org/zh-CN/docs/Web/API/CustomEvent)



#### Event.eventPhase

表示事件流当前处于哪一个阶段。

**语法**

```js
var phase = event.eventPhase;
```

返回一个代表当前执行阶段的 整数值

**常量：**事件阶段常量
下面这些值表示了事件流当前执行的阶段。

| 常量                                            | 值   | 描述                                                         |
| :---------------------------------------------- | :--- | :----------------------------------------------------------- |
| Event.NONE                                      | 0    | 这个时间，没有事件正在被处理                                 |
| Event.<font color=FF0000>CAPTURING</font>_PHASE | 1    | 事件正在被目标元素的祖先对象处理. 这个处理过程从Window开始，然后Document, 然后是HTMLHtmlElement, 一直这样，直到目标元素的父元素。 通过EventTarget.addEventListener() 注册为捕获模式的Event listeners 被调用。 |
| Event.<font color=FF0000>AT_TARGET</font>       | 2    | 事件对象已经抵达the event's target. 为这个阶段注册的事件监听被调用。 如果 Event.bubbles 的值为false, 对事件对象的处理在这个阶段后就会结束. |
| Event.<font color=FF0000>BUBBLING</font>_PHASE  | 3    | 事件对象逆向向上传播回目标元素的祖先元素, 从父亲元素开始，并且最终到达包含元素 Window. 这就是冒泡，并且只有Event.bubbles 值为true的时候才会发生。 为这个阶段注册的Event listeners 在这个过程中被触发. |

摘自：[MDN - Event.eventPhase](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/eventPhase)



#### Event.isTrusted

Event 接口的 isTrusted 属性是一个只读属性，它是一个布尔值（Boolean）。<font color=FF0000>当事件是由用户行为生成的时候，这个属性的值为 true</font> ，而<font color=FF0000>**当事件是由脚本创建、修改、通过 EventTarget.dispatchEvent() 派发的时候，这个属性的值为 false （即开发者通过Event构造函数自定义的，为false）**</font>。

摘自：[MDN - Event.isTrusted](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/isTrusted)



#### MouseEvent.relatedTarget

只读属性MouseEvent.relatedTarget 是鼠标事件的次要目标（如果存在），它包括：

| 事件名称   | target                   | relatedTarget            |
| ---------- | ------------------------ | ------------------------ |
| focusin    | EventTarget 获取焦点     | EventTarget 失去焦点     |
| focusout   | EventTarget 失去焦点     | The EventTarget 获取焦点 |
| mouseenter | 指针设备进入EventTarget  | 指针设备离开EventTarget  |
| mouseleave | 指针设备离开 EventTarget | 指针设备进入 EventTarget |
| mouseout   | 指针设备离开 EventTarget | The EventTarget          |
| mouseover  | 指针设备进入 EventTarget | 指针设备离开 EventTarget |
| dragenter  | 指针设备进入 EventTarget | 指针设备离开 EventTarget |
| dragexit   | 指针设备离开 EventTarget | 指针设备进入 EventTarget |

如果事件没有次要目标，relatedTarget 将返回 null.

- **语法**

  ```js
  var target = instanceOfMouseEvent.relatedTarget
  ```

- **返回值：** EventTarget 对象或者 null.

摘自：[MDN - MouseEvent.relatedTarget](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent/relatedTarget)



#### MouseEvent.button

MouseEvent.button是只读属性，它返回一个值，代表用户按下并触发了事件的鼠标按键。

这个属性<font color=FF0000><font size=4>**只能**</font>够表明在触发事件的单个或多个按键按下或释放过程中哪些按键被按下了</font>。

用户可能会改变鼠标按键的配置，<font color=FF0000>因此当一个事件的MouseEvent.button值为0时，它可能不是由物理上设备最左边的按键触发的。但是对于一个标准按键布局的鼠标来说就会是左键</font>。

- **语法**

  ```js
  var buttonPressed = instanceOfMouseEvent.button
  ```

- **返回值**
  一个数值，代表按下的鼠标按键：
  - **0：**<font color=FF0000>主按键</font>，通常指鼠标左键或默认值（译者注：如document.getElementById('a').click()这样触发就会是默认值）
  - **1：**辅助按键，通常指鼠标滚轮中键
  - **2：**<font color=FF0000>次按键</font>，通常指鼠标右键
  - **3：**第四个按钮，通常指浏览器后退按钮
  - **4：**第五个按钮，通常指浏览器的前进按钮

  对于配置为左手使用的鼠标，按键操作将正好相反。此种情况下，从右至左读取值。

摘自：[MDN - MouseEvent.button](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent/button)

#### MouseEvent.buttons（注意与MouseEvent.button的区别）

只读属性MouseEvent.buttons指示事件触发时哪些鼠标按键被按下。
每一个按键都用一个给定的数（见下文）表示。<font color=FF0000>如果同时多个按键被按下，buttons 的值为各键对应值做与计算（+）后的值</font>。<mark>例如，如果右键（2）和滚轮键（4）被同时按下，buttons 的值为 2 + 4 = 6</mark>。

**注意：**属性 MouseEvent.button 和 MouseEvent.buttons 是不同的。<font color=FF0000>MouseEvent.buttons 可指示任意鼠标事件中鼠标的按键情况，而 MouseEvent.button 只能保证在由按下和释放一个或多个按键时触发的事件中获得正确的值</font>。

- **语法**

  ```js
  var buttonPressed = instanceOfMouseEvent.buttons
  ```

- **返回值**
  <font color=FF0000>一个数字，用来标识鼠标按下的一个或者多个按键。如果按下的键为多个，则值等于所有按键对应数值进行或(|)运算的结果。</font>
  - 0  : 没有按键或者是没有初始化
  - 1  : 鼠标左键
  - 2  : 鼠标右键
  - 4  : 鼠标滚轮或者是中键
  - 8  : 第四按键 (通常是“浏览器后退”按键)
  - 16 : 第五按键 (通常是“浏览器前进”)

摘自：[MDN - MouseEvent.buttons](https://developer.mozilla.org/zh-CN/docs/Web/api/MouseEvent/buttons)



#### MouseEvent.pageX

 **pageX** 是一个由MouseEvent接口返回的相对于整个文档的x（水平）坐标以像素为单位的只读属性。

这个属性将基于文档的边缘，考虑任何页面的水平方向上的滚动。举个例子，如果页面向右滚动 200px 并出现了滚动条，这部分在窗口之外，然后鼠标点击距离窗口左边 100px 的位置，pageX 所返回的值将是 300。

 起初这个属性被定义为长整型。 CSSOM 视图模块将它重新定位为双浮点数类型。请参阅浏览器兼容性部分了解详情。



#### ClipboardEvent 🧪

ClipboardEvent <font color=FF0000>接口描述了**与修改剪切板相关信息**的事件</font>，这些<font color=FF0000>事件包括 剪切cut，复制copy 和 粘贴paste 事件</font>。

**构造函数 ClipboardEvent()：**用给定的参数创建了一个 ClipboardEvent 事件。

**属性：**同样是从其父类  Event 继承的属性。

**ClipboardEvent.clipboardData：**只读，<font color=FF0000>是一个 DataTransfer 对象</font>，它<font color=FF0000>**包含了由用户发起的 cut  、 copy 和 paste 操作影响的数据**</font>， <font color=FF0000>以及它的 **MIME 类型**</font>。MDN 链接：[MDN - ClipboardEvent.clipboardData](https://developer.mozilla.org/zh-CN/docs/Web/API/ClipboardEvent/clipboardData)

摘自：[ClipboardEvent](https://developer.mozilla.org/zh-CN/docs/Web/API/ClipboardEvent)

#### DataTransfer

DataTransfer 对象用于保存拖动并放下（drag and drop，**注：**html 拖放相关见下面）过程中的数据。它可以保存一项或多项数据，这些数据项可以是一种或者多种数据类型。这个对象可以从所有拖动事件 drag events 的 dataTransfer 属性上获取。

// TODO

摘自：[MDN - DataTransfer](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransfer)

#### HTML 拖放

参考视频：[使用原生 JS Drag&Drop API 实现可视化拖拽和文件拖放操作](https://www.bilibili.com/video/BV1i5411E7hk)



#### Event Loop 和 JS 引擎、渲染引擎的关系

<font color=FF0000 size=4>**js引擎**</font>

- **组成**

  **js 引擎包括 parser、解释器、gc 再加一个 JIT 编译器这几部分：**

  - **parser：**负责<font color=FF0000>把 javascript 源码转成 AST</font>

  - **interpeter：**解释器，<font color=FF0000>负责转换 AST 成字节码</font>，并解释执行

  - **JIT compiler：**对执行时的热点函数进行编译，<font color=FF0000>把字节码转成机器码</font>，<font color=FF0000>之后可以直接执行机器码</font>

  - **gc (garbage collector)：**<font color=FF0000>垃圾回收器</font>，<mark>清理堆内存中不再使用的对象</mark>

- **编译流水线**

  一般的 JS 引擎的 **编译流水线** 是 parse 源码成 AST，之后 AST 转为字节码，解释执行字节码。运行时会收集函数执行的频率，对于到达了一定阈值的热点代码，会把对应的字节码转成机器码（JIT），然后直接执行。这就是 js 代码能够生效的流程。

  <img src="https://s2.loli.net/2022/01/10/LjWasQ7mJUHYDh1.png" alt="https://mmbiz.qpic.cn/mmbiz_png/zPh0erYjkib1W6VwQ03NU6clT1alBexqetjkCauOPwGqjos8Vo6TQBN7E4pR72cCjDiby3G2yG001kSyvb7xxqvA/640.png" style="zoom: 80%;" />

<font color=FF0000 size=4>**渲染引擎**</font>

渲染时会把 html、css 分别用 parser 解析成 dom 和 cssom<mark>（注：注意下面的图，html是通过xml praser prase的）</mark>，然后合并到一起，并计算布局样式成绝对的坐标，生成渲染树，之后把渲染树的内容复制到显存就可以由显卡来完成渲染。

<img src="https://s2.loli.net/2022/01/10/LxPVIXyBjYeSW8o.png" alt="https://mmbiz.qpic.cn/mmbiz_png/zPh0erYjkib1W6VwQ03NU6clT1alBexqess0cic5sUraDQTQGEqLQCcZ2kNIPIuqf4JAC3VQrGDON8spQpDicsqlw/640.png" style="zoom:80%;" />

每一次渲染流程叫做一帧，浏览器会有一个帧率（比如一秒 60帧）来刷新。

<font size=4>**如何结合 <font color=FF0000>JS 引擎</font>和<font color=FF0000>渲染引擎</font>**</font>

<mark>不管是 JS 引擎、还是渲染引擎，都比较傻（纯粹），JS 引擎只会不断执行 JS 代码，渲染引擎也是只会布局和渲染</mark>。但是<font color=FF0000>要完成一个完整的网页应用，这两者都需要</font>。怎么综合两者呢？有两种思路：多线程 和 单线程

- **多线程：**分为多个线程，<font color=FF0000>主线程用来操作 ui 和渲染，其他线程用来执行一些任务</font>（不能多个线程同时修改 ui，顺序没法控制）。

  - **安卓 ui 架构：**安卓就是这样的架构，<font color=FF0000>在**主线程里面完成 ui 的更新，事件的绑定**</font>，其他逻辑可以放到别的线程，然后完成以后在消息队列中放一个消息，主线程不断循环的取消息来执行。
  - **electron ui 架构：**开发过 electron 应用的同学会知道，<mark>electron 中分为了主进程和渲染进程，window 相关的操作只能在主线程，由渲染进程向主进程发消息</mark>。

  从上面两个案例我们可以总结出，**所有的 ui 系统的设计，如果使用了多线程（进程）的架构，<font color=FF0000>基本都是 ui 只能在一个线程（进程）中操作，由别的线程（进程）来发消息到这边来更新</font>，如果多个线程，会有一个消息队列和 looper。消息队列的生产者是各个子线程（进程），消费者是主线程（进程）。**

  而且，<mark>不只是 ui 架构是这样，后端也大量运用了消息队列的概念，后端因为不同服务负载能力不一样，所以中间会加一个消息队列来异步处理消息</mark>，<font color=FF0000>**和前端客户端的 ui 架构不同的是，后端的消息队列中间件会有多个消费者、多个队列，而 ui 系统的消息队列只有一个队列，一个消费者（主线程、主进程）**</font>

  **在一个线程做 ui 操作，其他线程做逻辑计算的架构很普遍，会需要一个消息队列来做异步消息处理。** 网页中后来有了 web worker，也是这种架构的实现，但是最开始并不是这样的。

- **单线程：**因为 javascript 最开始只是被设计用来做表单处理，那么就不会有特别大的计算量，就没有采用多线程架构，而是在一个线程内进行 dom 操作和逻辑计算，渲染和 JS 执行相互阻塞。（后来加了 web worker，但不是主流）

  我们知道，<font color=FF0000>JS 引擎只知道执行 JS，渲染引擎只知道渲染，它们两个并不知道彼此，该怎么配合呢？答案就是 event loop</font>

  **宿主环境**

  <font color=FF0000>JS 引擎并 <font size=4>**不提供 event loop**</font></font>（可能很多同学以为 event loop 是 JS 引擎提供的，其实不是），它<font color=FF0000>是宿主环境为了集合渲染和 JS 执行，也为了处理 JS 执行时的高优先级任务而设计的机制</font>。

  **宿主环境有浏览器、node、跨端引擎等，<font color=FF0000>不同的宿主环境有一些区别</font>：**

  - **注入的全局 api 不同**

    - node 会注入一些全局的 require api，同时提供 fs、os 等内置模块

    - 浏览器会注入 w3c 标准的 api

    - 跨端引擎会注入设备的 api，同时会注入一套操作 ui 的 api（可能是对标 w3c 的 api 也可能不是）

  - **event loop 的实现不同**

    上文说过，event loop 是宿主环境提供了，不同的宿主环境有不同的需要调度的任务，所以也会有不同的设计：

    - 浏览器里面主要是调度渲染和 JS 执行，还有 worker
    - node 里面主要是调度各种 io
    - 跨端引擎也是调度渲染和 JS 执行

    **这里我们只关心浏览器里面的 event loop**

<font size=4>**浏览器的event loop**</font>

<font color=FF0000>浏览器里面执行一个 JS 任务就是一个 event loop</font>，每个 loop 结束会检查下是否需要渲染，是否需要处理 worker 的消息，通过这种每次 loop 结束都 check 的方式来综合渲染、JS 执行、worker 等，让它们都能在一个线程内得到执行（渲染其实是在别的线程，但是会和 JS 线程相互阻塞）。

<img src="https://s2.loli.net/2022/01/10/gV2Lz746MtZFhkc.png" alt="https://mmbiz.qpic.cn/mmbiz_png/zPh0erYjkib1W6VwQ03NU6clT1alBexqeVCVsRm613URKK4hNsTJZeW0Hb41o8a2JOibUk5hz6ibvzTceQibzQ7beQ/640.png" style="zoom:80%;" />

<font color=FF0000>这样就解决了渲染、JS 执行、worker 这三者的调度问题</font>。但是这样有没有问题？

我们会在任务队列中不断的放新的任务，这样<font color=FF0000>如果有更高优的任务是不是要等所有任务都执行完才能被执行。如果是“急事”呢？</font>所以这样还不行，<font color=FF0000 size=4>**要给 event loop 加上“急事”处理的快速通道，这就是微任务 micro tasks**</font>。

**micro tasks**

<img src="https://s2.loli.net/2022/01/10/TtEvVNhMSod7pxZ.png" alt="https://mmbiz.qpic.cn/mmbiz_png/zPh0erYjkib1W6VwQ03NU6clT1alBexqeIjfMdbbdArgJU4z4DRxqjxJ7zrs1FekxRqZ1HTSzcaYiacibej19JW9w/640.png" style="zoom:75%;" />

任务还是每次取一个执行，执行完检查下要不要渲染，处理下 worker 消息，但是<font color=FF0000>也给高优先级的“急事”加入了插队机制，会在执行完任务之后，把所有的急事 ( micro task ) 全部处理完</font>。这样，event loop 貌似就挺完美的了，每次都会检查是否要渲染，也能更快的处理 JS 的“急事”。

<font size=4>**requestAnimationFrame**</font>

<font color=FF0000>JS 执行完，开始渲染之前会有一个生命周期，就是 **requestAnimationFrame**</font>，**在这里面做一些计算最合适了，能保证一定是在渲染之前做的计算**。

<img src="https://s2.loli.net/2022/01/10/em9rhbSQ8NgYp5q.png" alt="https://mmbiz.qpic.cn/mmbiz_png/zPh0erYjkib1W6VwQ03NU6clT1alBexqeQuwS6YRPXUoU0EObFMI90MIhj4IxIuian0EF2SGdLYxITdytL1mo8hQ/640.png" style="zoom: 50%;" />

如果有人问 requestAnimationFrame 是宏任务还是微任务，就可以告诉他：**requestAnimationFrame 是每次 loop 结束发现需要渲染，<font color=FF0000>在渲染之前执行的一个回调函数，不是宏微任务</font>**

<font size=4>**event loop 的问题**</font>

上文聊过，虽然后面加入了 worker，但是主流的方式还是 JS 计算和渲染相互阻塞，这样就导致了一个问题：

<font color=FF0000>**每一帧的计算和渲染是有固定频率的，如果 JS 执行时间过长，超过了一帧的刷新时间，那么就会导致渲染延迟，甚至掉帧（因为上一帧的数据还没渲染到界面就被覆盖成新的数据了），给用户的感受就是“界面卡了”**</font>

<mark>什么情况会导致帧刷新拖延甚至帧数据被覆盖（丢帧）呢？</mark> <font color=FF0000>**每个 loop 在 check 渲染之前的每一个阶段都有可能**</font>，也就是 <mark style=background:fuchsia>**task、microtask、requestAnimationFrame、requestIdleCallback 都有可能导致阻塞了 check**</mark>，<font color=FF0000>这样等到了 check 的时候发现要渲染了，再去渲染的时候就晚了</font>。

<font color=FF0000>所以主线程 JS 代码不要做太多的计算</font>（不像安卓会很自然的起一个线程来做），<font color=FF0000>要做拆分</font>，<font color=FF0000>这也是为啥 ui 框架要做计算的 fiber 化</font>，就是因为处理交互的时候，<mark>不能让计算阻塞了渲染，要递归改循环，通过链表来做计算的暂停恢复</mark>（注：这里说的是react的fiber架构，父子兄弟用链表关联起来）。

除了 JS 代码本身要注意之外，如果浏览器能够提供 API 就是在每帧间隔来执行，那样岂不是就不会阻塞了，所以后来有了 requestIdleCallback

<font size=4>**requestIdleCallback**</font>

requestIdleCallback <font color=FF0000>会在每次 check 结束发现距离下一帧的刷新还有时间，就执行一下这个</font>；<font color=FF0000>如果时间不够，就下一帧再说</font>（注：这里应该可以理解为 响应优先）。<font color=FF0000>**如果每一帧都没时间呢，那也不行，所以提供了 timeout 的参数可以指定最长的等待时间，如果一直没时间执行这个逻辑，那么就算拖延了帧渲染也要执行**</font>。

<img src="https://s2.loli.net/2022/01/10/wiDYB9khmbAK1ZC.png" alt="https://mmbiz.qpic.cn/mmbiz_png/zPh0erYjkib1W6VwQ03NU6clT1alBexqeuPodh2TAHEibkTP2gu3jibstYww60jafMmSbxHtqMFYcjWE9TAN1WjMQ/640.png" style="zoom:67%;" />

这个 api 对于前端框架来说太需要了，框架就是希望计算不阻塞渲染，也就是在每一帧的间隔时间（idle时间）做计算，但是<mark>这个 api 毕竟是最近加的，有兼容问题，所以 react 自己实现了类似 idle callback 的 fiber 机制，在执行逻辑之前判断一下离下一帧刷新还有多久，来判断是否执行逻辑。</mark>

<font size=4>**总结**</font>
总之，浏览器里有 JS 引擎做 JS 代码的执行，利用注入的浏览器 API 完成功能，有渲染引擎做页面渲染，两者都比较纯粹，需要一个调度的方式，就是 event loop。
event loop 实现了 task 和 急事处理机制 microtask，而且每次 loop 结束会 check 是否要渲染，渲染前会有 requestAnimationFrames 生命周期。
帧刷新不能被拖延否则会卡顿甚至掉帧，所以就需要 JS 代码里面不要做过多计算，于是有了 requestIdleCallback 的 api，希望在每次 check 完发现还有时间就执行，没时间就不执行（这个deadline的时间也作为参数让 js 代码自己判断），为了避免一直没时间，还提供了 timeout 参数强制执行。
防止计算时间过长导致渲染掉帧是 ui 框架一直关注的问题，就是怎么不阻塞渲染，让逻辑能够拆成帧间隔时间内能够执行完的小块。浏览器提供了 idelcallback 的 api，很多 ui 框架也通过递归改循环然后记录状态等方式实现了计算量的拆分，目的只有一个：loop 内的逻辑执行不能阻塞 check，也就是不能阻塞渲染引擎做帧刷新。所以不管是 JS 代码宏微任务、 requestAnimationCallback、requestIdleCallback 都不能计算时间太长。这个问题是前端开发的持续性阵痛。

摘自：[Event Loop 和 JS 引擎、渲染引擎的关系](https://mp.weixin.qq.com/s/1PCQMgrXt4bPYtW-uVZgHQ)

#### window.requestAnimationFrame

window.requestAnimationFrame() <font color=FF0000>告诉浏览器</font>：你<font color=FF0000>希望执行一个动画，并且**要求浏览器在下次 <font size=4>*重绘*</font> 之前，调用指定的回调函数更新动画**</font>。该方法<font color=FF0000>需要传入一个 <font size=4>**回调函数**</font> 作为参数，该 <font size=4>**回调函数会在浏览器下一次重绘之前执行**</font></font>

> 注意：<font color=FF0000>若你想在浏览器下次重绘之前继续更新下一帧动画，那么 **回调函数自身必须再次调用 window.requestAnimationFrame()**</font>

当你准备更新动画时你应该调用此方法。这将使浏览器在下一次重绘之前 调用你传入给该方法的动画函数（即你的回调函数）。<font color=FF0000>回调函数执行次数通常是 每秒 60 次</font>，但<font color=FF0000>在大多数遵循 W3C 建议的浏览器中，回调函数执行次数通常与浏览器屏幕刷新次数相匹配</font>。<mark>为了提高性能和电池寿命，因此在大多数浏览器里，当 requestAnimationFrame() 运行在后台标签页 或者 隐藏的 \<iframe> 里时，requestAnimationFrame() 会被暂停调用以提升性能和电池寿命</mark>。

回调函数会被传入 DOMHighResTimeStamp 参数，DOMHighResTimeStamp 指示当前被 requestAnimationFrame() 排序的回调函数被触发的时间。在同一个帧中的多个回调函数，它们每一个都会接受到一个相同的时间戳，即使在计算上一个回调函数的工作负载期间已经消耗了一些时间。该时间戳是一个十进制数，单位毫秒，最小精度为 1ms。

**语法**

```js
window.requestAnimationFrame(callback);
```

##### 参数

- **callback：**下一次重绘之前更新动画帧所调用的函数（即上面所说的回调函数）。该回调函数会被传入 DOMHighResTimeStamp 参数，该参数与 performance.now() 的返回值相同，它表示 requestAnimationFrame() 开始去执行回调函数的时刻。

**返回值：**<font color=FF0000>一个 long 整数，请求 ID ，是回调列表中唯一的标识</font>。是个非零值，没别的意义。你<font color=FF0000>可以传这个值给 window.cancelAnimationFrame() 以取消回调函数</font>。

摘自：[MDN - window.requestAnimationFrame](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestAnimationFrame)

> **使用 requesAnimationFrame 比 setTimeout / setInterval 好在哪里？**
>
> - **CPU 节能：**
>
>   使用 setInterval 实现的动画，当页面被隐藏或最小化时，setInterval 仍然在后台执行动画任务，由于此时页面处于不可见或不可用状态，刷新动画是没有意义的，完全是浪费CPU资源。
>
>   而 RequestAnimationFrame 则完全不同，当页面处理未激活的状态下，该页面的屏幕刷新任务也会被系统暂停，因此跟着系统走的 RequestAnimationFrame 也会停止渲染，当页面被激活时，动画就从上次停留的地方继续执行，有效节省了 CPU 开销。
>
> - **函数<font color=FF0000>节流</font>**：在高频率事件（ resize, scroll 等）中，为了防止在一个刷新间隔内发生多次函数执行，<font color=FF0000>RequestAnimationFrame 可保证每个刷新间隔内，函数只被执行一次</font>。
>
> - **减少 DOM 操作**：<font color=FF0000>requestAnimationFrame **会把每一帧中的所有 DOM 操作集中起来**，**在一次重绘或回流中完成**</font>；并且重绘或回流的时间间隔会紧紧跟随浏览器的刷新频率，一般来说，这个频率为每秒60帧。
>
> 摘自：[「2021」高频前端面试题汇总之CSS篇](https://juejin.cn/post/6905539198107942919)

#### window.requestIdleCallback 🧪

> 注意：这是一个实验中的功能

window.requestIdleCallback() 方法<font color=FF0000>插入一个函数</font>，<font color=FF0000>这个函数将在浏览器空闲时期被调用</font>。这<font color=FF0000>使开发者能够在主事件循环上执行后台和低优先级工作，而不会影响延迟关键事件</font>，如动画和输入响应。函数一般会按先进先调用的顺序执行，然而，如果回调函数指定了执行超时时间timeout，则有可能为了在超时前执行函数而打乱执行顺序。

你可以在空闲回调函数中调用 requestIdleCallback()，以便在下一次通过事件循环之前调度另一个回调。

> 强烈建议使用 timeout 选项进行必要的工作，否则可能会在触发回调之前经过几秒钟。

##### 语法

```js
var handle = window.requestIdleCallback(callback[, options])
```

**返回值**：一个ID，可以把它传入 Window.cancelIdleCallback() 方法来结束回调。

**参数**

- **callback：**<font color=FF0000>一个在事件循环空闲时即将被调用的函数的引用</font>。<font color=FF0000>函数会接收到一个名为 IdleDeadline 的参数</font>，<mark>这个参数可以获取当前空闲时间以及回调是否在超时时间前已经执行的状态</mark>
- **options：**可选，包括可选的配置参数。具有如下属性：
  - **timeout：**<font color=FF0000>如果指定了timeout，并且有一个正值，而回调在timeout毫秒过后还没有被调用，那么回调任务将放入事件循环中排队</font>（注：可以理解为 强制执行），即使这样做有可能对性能产生负面影响

摘自：[MDN - requestIdleCallback](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestIdleCallback)

##### 垃圾回收的补充

#### FinalizationRegistry

FinalizationRegistry 对象<font color=FF0000>可以让你在对象被垃圾回收时请求一个回调</font>。

**描述**
FinalizationRegistry 提供了这样的一种方法：<font color=FF0000>当一个在 **注册表中注册的对象被回收时**，请求在某个时间点上调用一个清理回调</font>。（清理回调有时被称为 finalizer ）。

摘自：[MDN - FinalizationRegistry](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/FinalizationRegistry)



#### Window.scrollY

返回文档在垂直方向已滚动的像素值。

语法

```js
var y = window.scrollY;
```

y 是文档从顶部开始滚动过的像素值。

**示例**

```js
// 保证刚好滚动到第二页
if (window.scrollY) {
  window.scroll(0, 0);  // 重置滚动位置为文档的左上角
}

window.scrollByPages(1);
```

摘自：[MDN - Window.scrollY](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/scrollY)



#### Base64

在 JavaScript 中，有两个函数被分别用来处理解码和编码 base64 字符串：

- **atob()**
- **btoa()**

<font color=FF0000>atob() 函数能够解码通过base-64编码的字符串数据</font>。相反地，<font color=FF0000>btoa() 函数能够从二进制数据“字符串”创建一个base-64编码的ASCII字符串。</font>注：a是ASCII的意思，b是base64的意思

atob() 和 btoa() 均使用字符串。

摘自：[MDN - Base64的编码与解码](https://developer.mozilla.org/zh-CN/docs/Glossary/Base64)

#### WindowOrWorkerGlobalScope.btoa()

WindowOrWorkerGlobalScope.btoa() <font color=FF0000>从 String 对象中创建一个 base-64 编码的 ASCII 字符串</font>，其中字符串中的每个字符都被视为一个二进制数据字节。

**备注：**由于这个函数将每个字符视为二进制数据的字节，而不管实际组成字符的字节数是多少，<font color=FF0000>所以如果任何字符的码位超出 0x00 ~ 0xFF 这个范围，则会引发 InvalidCharacterError 异常</font>。

- **语法**

  ```js
  let encodedData = window.btoa(stringToEncode);
  ```

- **参数**
  - **stringToEncode：**一个字符串，<font color=FF0000>其字符分别表示要编码为 **ASCII 的二进制数据的单个字节**</font>。
- **返回值：**<font color=FF0000>一个包含 stringToEncode 的 <font size=4>**Base64**</font> 表示的字符串</font>。

- **备注：**<mark>你可以使用此方法对可能导致通信问题的数据进行编码，传输，然后使用 atob() 方法再次解码数据</mark>。例如，可以编码控制字符，包括 ASCII 值为 0 到 31 的字符。

摘自：[MDN - WindowOrWorkerGlobalScope.btoa()](https://developer.mozilla.org/zh-CN/docs/Web/API/WindowOrWorkerGlobalScope/btoa)

#### WindowOrWorkerGlobalScope.atob()

WindowOrWorkerGlobalScope.atob() <font color=FF0000>对经过 base-64 编码的字符串进行**解码**</font>。你可以使用 window.btoa() 方法来编码一个可能在传输过程中出现问题的数据，并且在接受数据之后，使用 atob() 方法再将数据解码。例如：你可以编码、传输和解码操作各种字符，比如 0-31 的 ASCII 码值。

- **语法**

  ```js
  var decodedData = scope.atob(encodedData);
  ```

- **异常：**<font color=FF0000>如果传入字符串不是有效的 base64 字符串，比如其长度不是 4 的倍数，则抛出DOMException</font>。

摘自：[MDN - WindowOrWorkerGlobalScope.atob()](https://developer.mozilla.org/zh-CN/docs/Web/API/WindowOrWorkerGlobalScope/atob)



#### Document.forms

[Document.]<font color=FF0000>forms</font>（Document可以省略掉） <font color=FF0000>返回当前文档中的 \<form>元素的一个集合(HTMLCollection)</font>。

摘自：[MDN - Document.forms](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/forms)

#### HTML<font color=FF0000>Form</font>Element.elements

elements 返回一个 HTMLFormControlsCollection其中包含FORM的所有控件。需要注意的是，其中不包括type等于image的input 元素。

你可以通过 name 或 id来访问对应的控件。

摘自：[MDN - HTMLFormElement.elements](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLFormElement/elements)



#### KeyboardEvent.repeat

KeyboardEvent.repeat是一个只读属性，返回一个布尔值Boolean，如果按键被一直按住，返回值为true。

摘自：[MDN - KeyboardEvent.repeat](https://developer.mozilla.org/zh-CN/docs/Web/API/KeyboardEvent/repeat)



#### document.readyState

Document.readyState 属性描述了document 的加载状态。

当该属性值发生变化时，会在 document 对象上触发 readystatechange  事件。

- **语法**

  ```js
  var string = document.readyState;
  ```

- **值：**一个文档的 readyState 可以是以下之一

  - **loading（正在加载）：**document 仍在加载。
  - **interactive（可交互）：**文档已被解析，"正在加载"状态结束（DOMElementLoaded），但是诸如图像，样式表和框架之类的子资源仍在加载。
  - **complete（完成）：**文档和所有子资源已完成加载。表示 load 状态的事件即将被触发。

摘自：[MDN - document.readyState](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/readyState)



#### PageTransitionEvent.persisted

只读属性persisted代表一个页面是否从缓存中加载的，<font color=FF0000>**类型：boolean**</font>

**示例：**

```js
window.addEventListener('pageshow', function(event) {
  if (event.persisted) {
    console.log('Page was loaded from cache.');
  }
});
```

摘自：[MDN - PageTransitionEvent.persisted](https://developer.mozilla.org/zh-CN/docs/Web/API/PageTransitionEvent/persisted)



#### FormData

<font color=FF0000>FormData 接口提供了一种表示表单数据的键值对 key / value 的构造方式，并且可以轻松的将数据通过XMLHttpRequest.send() 方法发送出去，本接口和此方法都相当简单直接。</font>如果送出时的编码类型被设为 "multipart/form-data"，它会使用和表单一样的格式。

实现了 FormData 接口的对象可以直接在for...of结构中使用，而不需要调用entries() : for (var p of myFormData) 的作用和 for (var p of myFormData.entries()) 是相同的。

**构造函数**

```js
FormData() // 创建一个新的 FormData 对象。
```

**方法**

- **FormData.append()：**<font color=FF0000>向 FormData 中添加新的属性值</font>，FormData 对应的属性值存在也不会覆盖原值，而是新增一个值，如果属性不存在则新增一项属性值。
- **FormData.delete()：**<font color=FF0000>从 FormData 对象里面删除一个键值对</font>。
- **FormData.entries()：**返回一个包含所有键值对的iterator对象。
- **FormData.get()：**<font color=FF0000>返回在 FormData 对象中与给定键关联的第一个值</font>。
- **FormData.getAll()：**返回一个包含 FormData 对象中与给定键关联的所有值的数组。
- **FormData.has()：**返回一个布尔值表明 FormData 对象是否包含某些键。
- **FormData.keys()：**返回一个包含所有键的iterator对象。
- **FormData.set()：**给 FormData 设置属性值，如果FormData 对应的属性值存在则覆盖原值，否则新增一项属性值。
- **FormData.values()：**返回一个包含所有值的iterator对象。

摘自：[MDN - FormData](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData)



#### 逗号操作符

逗号操作符 对它的每个操作数求值（从左到右），并 <font color=FF0000>**返回最后一个操作数的值**</font>。

```js
let x = 1;
x = (x++, x);
console.log(x); // expected output: 2

x = (2, 3);
console.log(x); // expected output: 3
```

- **语法**

  ```js
  expr1, expr2, expr3...
  ```

- **参数**

  - **expr1, expr2, expr3...：**任一表达式

- **描述：**当你想要在<mark>期望一个表达式的位置包含多个表达式时，可以使用逗号操作符</mark>。<font color=FF0000>这个操作符最常用的一种情况是：for 循环中提供多个参数</font>。

摘自：[MDN - 逗号操作符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comma_Operator)

#### 尾后逗号

<mark>尾后逗号 （有时叫做“终止逗号”）在向 JavaScript 代码添加元素、参数、属性时十分有用</mark>。<font color=FF0000>如果你想要添加新的属性，并且上一行已经使用了尾后逗号，你可以仅仅添加新的一行，而不需要修改上一行。这使得版本控制的代码比较（diff）更加清晰，代码编辑过程中遇到的麻烦更少</font>。

JavaScript 一开始就支持数组字面量中的尾后逗号，随后向对象字面量（ECMAScript 5）中添加了尾后逗号。最近<font color=FF0000>（ECMAScript 2017），又将其添加到**函数参数**中</font>。

但是，<font color=FF0000>**JSON 不允许尾后逗号**</font>。对象中的尾后逗号仅仅在 ECMAScript 5 中引入。由于 JSON 基于 ES5 之前的语法， JSON 中并不允许尾后逗号。

JavaScript 忽略数组中的尾后逗号。<font color=FF0000>**如果使用了多于一个尾后逗号，会产生省略**</font>（elision，或者间隙 hole）。 <mark>带有间隙的数组叫做稀疏数组（sparse 紧凑数组 dense array 没有省略/间隙）</mark>。 例如，当使用 Array.prototype.forEach() 或 Array.prototype.map() 迭代数组时，会跳过数组间隙。

```js
var arr = [1, 2, 3,,,];
arr.length; // 5
```

仅仅包含逗号的函数参数定义或者函数调用会抛出 SyntaxError。 而且，当使用剩余参数的时候，并不支持尾后逗号：

```js
function f(,) {} // SyntaxError: missing formal parameter
(,) => {};       // SyntaxError: expected expression, got ','
f(,)             // SyntaxError: expected expression, got ','

function f(...p,) {} // SyntaxError: parameter after rest parameter
(...p,) => {}        // SyntaxError: expected closing parenthesis, got ','
```

摘自：[MDN - 尾后逗号](https://developer.mozilla.org/zh-CN/docs/orphaned/Web/JavaScript/Reference/Trailing_commas)



#### BigInt (ES2020)

BigInt 是一种内置对象，它<font color=FF0000>提供了一种方法来表示大于 2^53^ - 1 的整数</font>。这原本是 Javascript中可以用 Number 表示的最大数字。<font color=FF0000>BigInt 可以表示任意大的整数。</font>

可以用在一个整数字面量后面加 n 的方式定义一个 BigInt ，如：10n，或者调用函数BigInt()。

```js
const theBiggestInt = 9007199254740991n;
const alsoHuge = BigInt(9007199254740991); // ↪ 9007199254740991n
const hugeString = BigInt("9007199254740991"); // ↪ 9007199254740991n
const hugeHex = BigInt("0x1fffffffffffff"); // ↪ 9007199254740991n
const hugeBin = BigInt("0b11111111111111111111111111111111111111111111111111111"); // ↪ 9007199254740991n
```

它在某些方面类似于 Number ，但是也有几个关键的不同点：<font color=FF0000>**不能用于 Math 对象中的方法；不能和任何 Number 实例混合运算，两者必须转换成同一种类型**。在两种类型来回转换时要小心，因为 BigInt 变量在转换成 Number 变量时可能会丢失精度</font>。

<mark>使用 typeof 测试时， BigInt 对象返回 "bigint" </mark>

**运算：**

以下操作符可以和 BigInt 一起使用： 

- <font color=FF0000>+、*、-、**、% </font>
- <font color=FF0000>除 >>> （无符号右移）之外的 位操作 也可以支持</font>。因为 BigInt 都是有符号的， >>> （无符号右移）不能用于 BigInt。
- 为了兼容 asm.js ，<font color=FF0000>BigInt 不支持单目 (+) 运算符</font>。

**比较**

BigInt 和 Number 不是严格相等的（!==），但是宽松相等的（\==）。

```js
0n === 0// ↪ false
0n == 0// ↪ true
```

Number 和 BigInt 可以进行比较。

```js
1n < 2 // ↪ true
2n > 1// ↪ true
2 > 2 // ↪ false
2n > 2 // ↪ false
2n >= 2 // ↪ true
```

两者也可以混在一个数组内并排序。

```js
const mixed = [4n, 6, -12n, 10, 4, 0, 0n]; // ↪  [4n, 6, -12n, 10, 4, 0, 0n]
mixed.sort(); // ↪ [-12n, 0, 0n, 10, 4n, 4, 6]
```

**构造器**

- BigInt()：创建BigInt 对象。

**静态方法**

- **BigInt.asIntN()：**将 BigInt 值转换为一个 -2^width-1^ 与 2^width-1^-1 之间的有符号整数。

  - **语法**

    ```js
    BigInt.asIntN(width, bigint);
    ```

  - **参数**

    - width：可存储整数的位数。
    - bigint：要存储在指定位数上的整数。

  - **返回值：**bigint 模(modulo) 2^width^ 作为有符号整数的值。

  摘自：[MDN - BigInt.asIntN()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/BigInt/asIntN)

- **BigInt.asUintN()：**将一个 BigInt 值转换为 0 与 2^width^-1 之间的无符号整数。

  - **语法**

    ```js
    BigInt.asUintN(width, bigint);
    ```

  - **参数**

    - width：可存储整数的位数。
    - bigint： 要存储在指定位数上的整数。

  - **返回值：**bigint 模(modulo) 2^width^ 作为无符号整数的值。

  摘自：[MDN - BigInt.asUintN()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/BigInt/asUintN)

**实例方法**

- **BigInt.prototype.toLocaleString()：**返回此数字的 language-sensitive 形式的字符串。覆盖 Object.prototype.toLocaleString()  方法。
- **BigInt.prototype.toString()：**返回以指定基数(base)表示指定数字的字符串。覆盖 Object.prototype.toString() 方法。
- **BigInt.prototype.valueOf()：**返回指定对象的基元值。 覆盖 Object.prototype.valueOf() 方法。

摘自：[MDN - BigInt](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/BigInt)



#### globalThis（ES2020）

全局属性 globalThis 包含全局的 this 值，类似于全局对象（global object）。

**globalThis 属性的属性特性：**

- **writable：**true
- **enumerable：**false
- **configurable：**true

**描述**
**在以前，从不同的 JavaScript 环境中获取全局对象需要不同的语句**。<font color=FF0000>在 Web 中，可以通过 window、self 或者 frames 取到全局对象，但是在 Web Workers 中，只有 self 可以。在 Node.js 中，它们都无法获取，必须使用 global</font>。

<font color=FF0000>globalThis 提供了一个标准的方式**来获取不同环境下的全局 this  对象（也就是全局对象自身）**</font>。不像 window 或者 self 这些属性，它确保可以在有无窗口的各种环境下正常工作。所以，<font color=FF0000>你可以安心的使用 globalThis，不必担心它的运行环境</font>。为便于记忆，你只需要记住，全局作用域中的 this 就是 globalThis。

摘自：[MDN - globalThis](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/globalThis)



#### 可选链操作符（ES2020）

可选链操作符( ?. )允许读取位于连接对象链深处的属性的值，而不必明确验证链中的每个引用是否有效。

?. 操作符的功能类似于 . 链式操作符，不同之处在于，<font color=FF0000>在引用为空(nullish ) （null 或者 undefined）的情况下不会引起错误，该表达式短路返回值是 undefined</font>。与函数调用一起使用时，如果给定的函数不存在，则返回 undefined。

当尝试访问可能不存在的对象属性时，<font color=FF0000>可选链操作符将会使表达式更短、更简明</font>。在探索一个对象的内容时，如果不能确定哪些属性必定存在，可选链操作符也是很有帮助的。

**语法**

```js
obj?.prop
obj?.[expr]
arr?.[index]
func?.(args)
```

摘自：[MDN - 可选链操作符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Optional_chaining)

#### 空值合并运算符（ES2020）

空值合并操作符 ( ?? ) 是一个逻辑操作符，<font color=FF0000>**当 <font size=4>左侧</font> 的操作数为 null 或者 undefined 时**</font>，<font color=FF0000>返回其右侧操作数，否则返回左侧操作数</font>。

与逻辑或操作符（||）不同，逻辑或操作符会在左侧操作数为假值时返回右侧操作数。也就是说，<font color=FF0000>如果使用 || 来为某些变量设置默认值，可能会遇到意料之外的行为。比如为假值（例如，'' 或 0）时</font>。<mark>注：使用 || 会出现 falsy值 的问题，而其中部分 falsy值 是不想它被 隐式转换 的</mark>

**使用场景：**函数中，你不清楚对方传不传该参数，这时候可以在函数体中使用 `let foo = paramFoo ?? 'default foo'`以附上默认值

**短路：**与 OR 和 AND 逻辑操作符相似，当左表达式不为 null 或 undefined 时，不会对右表达式进行求值。（可以通过在右侧放置一个带有打印功能的函数，以验证）

<font color=FF0000>**不能与 AND 或 OR 操作符共用：**</font>将 ?? 直接与 AND（&&）和 OR（||）操作符组合使用是不可取的。（译者注：应当是因为空值合并操作符和其他逻辑操作符之间的运算优先级/运算顺序是未定义的）这种情况下会抛出 SyntaxError 。

```js
null || undefined ?? "foo"; // 抛出 SyntaxError
true || undefined ?? "foo"; // 抛出 SyntaxError
```

摘自：[MDN - 空值合并运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)

补充：null == undefined，另外null经过隐式转换 只等于 undefined（另外，null === null），通过这点可以辅助记忆。

#### Logical OR assignment (||=) 

逻辑或赋值（x ||= y）运算仅在 x 为虚值（即：falsy ）时赋值。

- 示例：

  ```js
  const a = { duration: 50, title: '' };
  
  a.duration ||= 10; // 这里的 a.duration 为 50
  console.log(a.duration); // expected output: 50
  
  a.title ||= 'title is empty.'; // 这里的 a.title 为 ''
  console.log(a.title); // expected output: "title is empty"
  ```

- **语法**

  ```js
  expr1 ||= expr2
  ```

- **描述：**短路运算
  <font color=FF0000>**逻辑或**</font> 的运算方法如下所示：

  ```js
  x || y; // 当 x 为真值时，返回 x；当 y 为真值时，返回 y
  ```

  <font color=FF0000>**逻辑或运算**</font> 的短路逻辑：<font color=FF0000>**当且仅当**第一个操作数尚未确定结果（不是真值）时，才会评估第二个操作数</font>。逻辑或赋值运算有 <font color=FF0000>**短路逻辑**</font>，这意味着，它仅在左侧为虚值时执行赋值。换句话说，x ||= y 等同于：

  ```js
  x || (x = y);
  ```

  但不等同于以下总是执行赋值的语句：

  ```js
  x = x || y;
  ```

  请注意，这与数学逻辑和按位赋值运算不同。

摘自：[MDN - Logical OR assignment (||=)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Logical_OR_assignment)

#### Logical AND assignment (&&=)

逻辑与赋值  (x &&= y) 操作符 只在 x 为 truthy 时赋值

- 示例：

  ```js
  let a = 1;
  let b = 0;
  
  a &&= 2;
  console.log(a); // expected output: 2
  
  b &&= 2;
  console.log(b); // expected output: 0
  ```

- 语法

  ```js
  expr1 &&= expr2
  ```

摘自：[MDN - Logical AND assignment (&&=)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Logical_AND_assignment)



#### ArrayBuffer

ArrayBuffer 对象用来表示<font color=FF0000>通用的、固定长度</font>的<font color=FF0000>原始二进制数据缓冲区</font>。

你<font color=FF0000>**不能直接操作 ArrayBuffer 的内容，而是要通过类型数组对象或 DataView 对象来操作**</font>，它们会将缓冲区中的数据表示为特定的格式，并通过这些格式来读写缓冲区的内容。

- **语法**

  ```js
  new ArrayBuffer(length)
  ```

- **参数**
  length：要创建的 ArrayBuffer 的大小，单位为字节。

- **返回值：**一个指定大小的 ArrayBuffer 对象，其内容被初始化为 0。

- **异常：**如果 length 大于 Number.MAX_SAFE_INTEGER（>= 2 ** 53）或为负数，则抛出一个  RangeError  异常。

- **属性**
  - **ArrayBuffer.length：**ArrayBuffer 构造函数的 length 属性，其值为1。
  - **ArrayBuffer.prototype.byteLength：**只读属性，表示 ArrayBuffer 的byte的大小，在ArrayBuffer构造完成时生成，不可改变。
  - **get ArrayBuffer[@@species]：**返回 ArrayBuffer 的构造函数。
  - **ArrayBuffer.prototype：**通过 ArrayBuffer 的原型对象可以为所有 ArrayBuffer 对象添加属性。
- **方法**
  - **ArrayBuffer.isView(arg)：**如果参数是 ArrayBuffer 的视图实例则返回 true，例如 类型数组对象 或 DataView 对象；否则返回 false。
  - **ArrayBuffer.transfer(oldBuffer [, newByteLength]) ：**返回一个新的 ArrayBuffer 对象，其内容取自 oldBuffer 中的数据，并且根据 newByteLength 的大小对数据进行截取或补 0。

摘自：[MDN - ArrayBuffer](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)



#### TypedArray

一个类型化数组（TypedArray）对象描述了一个底层的二进制数据缓冲区（binary data buffer）的一个类数组视图（view）。

事实上，没有名为 TypedArray 的全局属性，也没有一个名为 TypedArray 的构造函数。相反，有许多不同的全局属性，它们的值是特定元素类型的类型化数组构造函数

- **语法：**

  ```js
  // 下面代码是语法格式，不能直接运行，
  // TypedArray 关键字需要替换为底部列出的构造函数。
  new TypedArray(); // ES2017中新增
  new TypedArray(length);
  new TypedArray(typedArray);
  new TypedArray(object);
  new TypedArray(buffer [, byteOffset [, length]]);
  
  // TypedArray 指的是以下的其中之一：
  Int8Array();
  Uint8Array();
  Uint8ClampedArray();
  Int16Array();
  Uint16Array();
  Int32Array();
  Uint32Array();
  Float32Array();
  Float64Array();
  ```

- **参数**

  - **length：**当传入 length 参数时，一个内部的数组缓冲区会被创建在内存中，该缓存区的大小（类型化数组中 byteLength 属性的值）是传入的 length 乘以数组中每个元素的字节数（BYTES_PER_ELEMENT），每个元素的值都为0。(译者注：每个元素的字节数是由具体的构造函数决定的，比如 Int16Array() 的每个元素的字节数为 2，Int32Array() 的每个元素的字节数为 4)
  - **typedArray：**当传入一个任意类型化数组对象作为 typedArray 参数时（比如 Int32Array），typedArray 会被复制到一个新的类型数组中。typedArray 中的每个值在被复制到新的数组之前，会被转化为相应类型的构造函数。新的生成的类型化数组对象将会有跟传入的数组相同的长度（译者注：比如原来的类型化数组的 length==2，那么新生成的数组的 length 也是 2，只是数组中的每一项进行了转化）。
  - **object：**当传入一个 object 作为参数时，就像通过 TypedArray.from() 方法创建一个新的类型化数组一样。
  - **buffer, byteOffset, length：**当传入一个 buffer 参数，或者再另外加上可选参数 byteOffset 和 length 时，一个新的类型化数组视图将会被创建，并可用于呈现传入的 ArrayBuffer 实例。byteOffset 和length 参数指定了类型化数组视图将要暴露的内存范围。如果两者都未传入，那么整个buffer 都会被呈现；如果仅仅忽略 length，那么 buffer 中偏移了 byteOffset 后剩下的 buffer 将会被呈现。





#### 模块化规范

- CommonJS: Node.js服务器创建的模块系统，局限于服务端
- AMD: 通过require.js实现，异步模块定义
- UMD：建议作为通用的模块系统，它和AMD和CommonJS都兼容
- CMD: sea.js
- export / import：ES6



#### ES6 工厂函数




#### import

// TODO

摘自：[MDN - import](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/import)



#### import.meta

import.meta是一个<font color=FF0000>给 JavaScript  模块暴露特定上下文的元数据属性的对象</font>。<font color=FF0000>它包含了这个模块的信息，比如说这个模块的 URL</font>。

##### 语法

```js
import.meta
```

##### 描述

import.meta 对象由一个关键字 "import"，一个点符号和一个 meta 属性名组成。通常情况下 "import." 是作为一个属性访问的上下文，但是在这里 "import" 不是一个真正的对象。（**注：** 这里的说法，有点类似于 [[#new.target]] ）

import.meta 对象是由 ECMAScript 实现的，它带有一个 null 的原型对象。这个对象可以扩展，并且它的属性都是 “可写”，“可配置” 和 “可枚举” 的。

##### 示例

这里有一个 my-module.mjs 模块

```html
<script type="module" src="my-module.mjs"></script>
```

你可以通过 import.meta 对象获取这个模块的元数据信息。

```js
console.log(import.meta); // { url: "file:///home/user/my-module.mjs" }
```

它返回一个带有 url 属性的对象，指明模块的基本 URL。也可以是外部脚本的 URL，还可以是内联脚本所属文档的 URL 。

摘自：[MDN - import.meta](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/import.meta)



#### document.documentElement

Document.documentElement 是一个会返回文档对象（document）的<font color=FF0000>根元素的只读属性</font>（<font color=FF0000>如HTML文档的 \<html> 元素</font>）。

对于任何非空 HTML 文档，调用 document.documentElement 总是会返回一个 \<html> 元素，且它一定是该文档的根元素。借助这个只读属性，能方便地获取到任意文档的根元素。

摘自：[MDN - document.documentElement](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/documentElement)

#### Element.firstElementChild

Element.firstElementChild <font color=FF0000>只读属性</font>，返回对象的第一个子 <font color=FF0000>**元素**</font>, 如果没有子元素，则为null。

他的属性最初是在element遍历纯接口中定义的。由于这个接口包含两组不同的属性，一个针对具有子元素的Node，一个针对子元素的属性，因此它们被移动到两个单独的纯接口中，Element和ChildNode。在本例中，firstElementChild移动到Element。这是一个相当技术性的更改，不应该影响兼容性。

摘自：[MDN - Element.firstElementChild](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/firstElementChild)

#### HTMLInputElement

HTMLInputElement 接口提供了特定的属性和方法（继承自常规的HTML元素接口）用于管理输入元素的布局和外观。

```mermaid
classDiagram
HTMLInputElement --|> HTMLElement : Inheritance
HTMLElement --|> Element : Inheritance
Element --|> Node : Inheritance
Node --|> EventTarget : Inheritance
```

摘自：[MDN - HTMLInputElement](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLInputElement)



#### Element.closest()

Element.closest() 方法用来获取：<font color=FF0000>匹配特定选择器且离当前元素最近的祖先元素（也可以是当前元素本身）。如果匹配不到，则返回 null</font>

**语法**

```js
var closestElement = targetElement.closest(selectors);
```

- **参数**
  selectors 是指定的选择器，比如 "p:hover, .toto + q"。
- **返回值**
  elt 是查询到的祖先元素，也可能是 null。
- **异常：**如果传入的选择器不合法，则抛出 SyntaxError 异常。

示例见下面链接

摘自：[MDN - Element.closest()](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/closest)



#### element.insertAdjacentHTML

insertAdjacentHTML() 方法<font color=FF0000>将指定的文本解析为 Element 元素</font>，并<font color=FF0000>将结果节点插入到DOM树中的指定位置</font>。<mark style=background-color:hotpink>它不会重新解析它正在使用的元素，因此它不会破坏元素内的现有元素。这避免了额外的序列化步骤，**使其比直接使用innerHTML操作更快**</mark>。

**语法**

```js
element.insertAdjacentHTML(position, text);
```

- **position：**一个 DOMString，表示插入内容相对于元素的位置，并且必须是以下字符串之一：
  - **beforebegin：**<font color=FF0000>元素自身的前面</font>。
  - **afterbegin：**插入元素<font color=FF0000>内部</font>的<font color=FF0000>第一个子节点之前</font>。
  - **beforeend：**插入元素<font color=FF0000>内部</font>的<font color=FF0000>最后一个子节点之后</font>。
  - **afterend：**<font color=FF0000>元素自身的后面</font>。
- **text：**是<font color=FF0000>**要被解析为HTML或XML元素**</font>，并插入到DOM树中的 DOMString。

摘自：[MDN - element.insertAdjacentHTML](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/insertAdjacentHTML)



#### Element.attachShadow()

<font color=FF0000>Element.attachShadow() 方法**给指定的元素挂载一个Shadow DOM**，并且**返回对 ShadowRoot 的引用**</font>。

**可以被挂载的shadow DOM元素**

要注意的是，<font color=FF0000>不是每一种类型的元素都可以附加到shadow root（影子根）下面</font>。出于安全考虑，一些元素不能使用 shadow DOM（例如\<a>），以及许多其他的元素。下面是一个可以挂载 shadow root 的元素列表：

**任何带有有效的名称且可独立存在的（autonomous）自定义元素**

- \<article>
- \<aside>
- \<blockquote>
- \<body>
- \<div>
- \<footer>
- \<h1> \<h2> \<h3> \<h4> \<h5> \<h6>
- \<header>
- \<main>
- \<nav>
- \<p>
- \<section>
- \<span>

**语法**

```js
var shadowroot = element.attachShadow(shadowRootInit);
```

**参数**

- **shadowRootInit：**一个 ShadowRootInit 字典，包括下列字段：

  - **mode 模式：**指定 Shadow DOM 树封装模式的字符串，可以是以下值：

    - **open：**shadow root元素允许<font color=FF0000>从js外部访问根节点</font>，例如使用 Element.shadowRoot:

      ```js
      element.shadowRoot; // 返回一个ShadowRoot对象
      ```

    - **closed：<font color=FF0000>拒绝从js外部访问关闭的shadow root节点</font>

      ```js
      element.shadowRoot; // 返回null
      ```

  - **delegatesFocus：**焦点委托

    一个布尔值, <font color=FF0000>当设置为 true 时, 指定减轻自定义元素的聚焦性能问题行为</font>.
    当shadow DOM中不可聚焦的部分被点击时, 让第一个可聚焦的部分成为焦点, 并且shadow host（影子主机）将提供所有可用的 :focus 样式.

**返回值：**<font color=FF0000>返回一个 ShadowRoot 对象或者 **null**</font>。

**异常**

| 异常                          | 说明                                                         |
| :---------------------------- | :----------------------------------------------------------- |
| InvalidStateError无效状态错误 | 您添加的元素已经是一个shadow host（影子主机）.               |
| NotSupportedError不被支持错误 | 您应该添加 HTML 元素的命名空间之外的shadow root, 或者这个元素不能有其他shadow挂载到它上面 (见上). |

摘自：[MDN - Element.attachShadow()](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/attachShadow)

#### ShadowRoot.host

ShadowRoot的host属性是只读的，它返回一个对 ShadowRoot 附加到的 DOM 元素的引用

**语法：**

```js
const someElement = shadowRoot.host
```

**返回值：**一个DOM元素

**示例：**

```js
let customElem = document.querySelector('my-shadow-dom-element');
let shadow = customElem.shadowRoot;
  ...

// return the original host element some time later
let hostElem = shadow.host;
```

摘自：[MDN - ShadowRoot.host](https://developer.mozilla.org/en-US/docs/Web/API/ShadowRoot/host)



#### Intl

**Intl 对象** 是 ECMAScript <font color=FF0000>**国际化 API 的一个命名空间**</font>，它<font color=FF0000>**提供了精确的字符串对比、数字格式化，和日期时间格式化**</font>。Collator、NumberFormat 和 DateTimeFormat 对象的 **构造函数** 是 Intl 对象的属性。

##### 属性

- **Intl.Collator**：collators 的 <font color=FF0000>构造函数</font>，用于启用对语言敏感的字符串比较的对象。
- **Intl.DateTimeFormat**：用于启用语言敏感的日期和时间格式的对象的构造函数。
- **Intl.ListFormat**：Constructor for objects that enable language-sensitive list formatting.
- **Intl.NumberFormat**：用于启用语言敏感数字格式的对象的构造函数。
- **Intl.PluralRules**：用于启用多种敏感格式和多种语言语言规则的对象的构造函数。
- **Intl.RelativeTimeFormat**：Constructor for objects that enable language-sensitive relative time formatting

##### 方法

- **Intl.getCanonicalLocales()**：返回规范区域名称 ( canonical locale name )

// TODO 后面还有更多内容，没有细究。

摘自：[MDN - Intl](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Intl)

****

## 工程设计

#### 对象固化

在框架封装，继承的时候，如果你做好了一个对象给别人使用，为了不让使用者拿过来后任意涂改，比如他把你的对象的一个方法改成了指向另一个函数，或者干脆删除了这个方法，我们需要把这个对象固化。

<mark>在 JavaScript里我们可以在三个层级上固化一个对象</mark>。<font color=FF0000> 从宽到严它们依次是：防止扩展、密封、冻结</font>。

![VnF0SS](https://i.loli.net/2021/08/30/84v2l1ZkMCXO6eg.png)

摘自：[js固化对象](https://zhhlwd.gitee.io/posts/js%E5%9B%BA%E5%8C%96%E5%AF%B9%E8%B1%A1.html)



#### 一些 worker 的总概

除了专用 worker 之外，还有一些其他种类的 worker ：

- **Shared Workers** 可被不同的窗体的多个脚本运行，例如 IFrames 等，只要这些 workers 处于同一主域。共享worker 比专用 worker 稍微复杂一点 — 脚本必须通过活动端口进行通讯。
- **Service Workers** 一般作为web应用程序、浏览器和网络（如果可用）之间的代理服务。他们旨在（除开其他方面）创建有效的离线体验，拦截网络请求，以及根据网络是否可用采取合适的行动，更新驻留在服务器上的资源。他们还将允许访问推送通知和后台同步API。
- **Chrome Workers** 是一种仅适用于firefox的worker。如果您正在开发附加组件，希望在扩展程序中使用worker且可以访问 js-ctypes，那么可以使用Chrome Workers。
- **音频 Workers** 可以在网络worker上下文中直接完成脚本化音频处理

摘自：[MDN - Web Workers API](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API)

#### web worker

Worker 接口是 Web Workers API 的一部分，指的是一种可由脚本创建的后台任务，任务执行中可以向其创建者收发信息。<font color=FF0000>要创建一个 Worker ，只须调用 Worker( URL ) 构造函数，函数参数 URL 为指定的脚本</font>。

<font color=FF0000>Worker 也可以创建新的 Worker，当然，所有 Worker 必须与其创建者<font size=4>**同源**</font></font>。

需要注意的是，<font color=FF0000>不是所有函数和构造函数（或者说…类）都可以在 Worker 中使用</font>。具体参考页面 [Worker 所支持的函数和类](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API/Functions_and_classes_available_to_workers)。Worker 可以使用 XMLHttpRequest 发送请求，但是请求的 responseXML 与 channel 两个属性值始终返回 null （fetch 仍可正常使用，没有类似的限制）。 

- **构造函数**
  Worker()：创建一个专用Web worker，它只执行URL指定的脚本。<font color=FF0000> Blob URL 作为参数亦可</font>。

  > 如果 此URL有一个无效的语句，或者违反同源策略，一个 SECURITY_ERR 类型的DOMException被抛出。
  >
  > **语法**
  >
  > ```js
  > const myWorker = new Worker(aURL[, options]);
  > ```
  >
  > **参数**
  >
  > - **aURL：**是一个DOMString 表示worker 将执行的脚本的URL。它必须遵守同源策略。
  > - **options：** 可选，包含可在创建对象实例时设置的选项属性的对象。可用属性如下:
  >   - **type：**用以指定 worker 类型的  DOMString 值. 该值可以是 classic 或 module. 如果未指定，将使用默认值 classic.
  >   - **credentials：**用以指定 worker 凭证的 DOMString 值.该值可以是 omit, same-origin，或 include.。如果未指定，或者 type 是 classic，将使用默认值 omit (不要求凭证)。
  >   - **name：**在 DedicatedWorkerGlobalScope 的情况下，用来表示 worker 的 scope 的一个 DOMString 值，主要用于调试目的。
  >
  > **异常**
  >
  > - 当 document 不被允许启动 worker 的时候，将抛出一个 SecurityError 异常。例如：如果提供的 aURL 有语法错误，或者与同源策略相冲突（跨域访问）。
  > - 如果 worker 的 MIME 类型不正确，将抛出一个 NetworkError 异常。<font color=FF0000>worker 的 MIME 类型必须是 text/javascript</font>。
  > - 如果 aURL 无法被解析（格式错误），将抛出一个 SyntaxError 异常。
  >
  > 摘自：[MDN - Worker()](https://developer.mozilla.org/zh-CN/docs/Web/API/Worker/Worker)

- **属性：**<font color=FF0000>继承父对象EventTarget 的属性，以及实现对象 AbstractWorker 的属性</font>。

- **事件句柄：**
  - **AbstractWorker.onerror：**当 ErrorEvent 类型的事件冒泡到 worker 时，事件监听函数 EventListener 被调用。它继承于 AbstractWorker
  - **Worker.onmessage：**当 MessageEvent 类型的事件冒泡到 worker 时，事件监听函数 EventListener 被调用.  例如，一个消息通过 DedicatedWorkerGlobalScope.postMessage，从执行者发送到父页面对象，消息保存在事件对象的 data 属性中.
  - **Worker.onmessageerror：**当messageerror 类型的事件发生时，对应的 event handler 代码被调用。
  
- **方法：**<font color=FF0000>继承父对象EventTarget 的方法，以及实现对象 AbstractWorker 的方法</font>。
  - **Worker.postMessage()：**<font color=FF0000>发送一条消息到最近的外层对象</font>，消息可由任何 JavaScript 对象组成。
  
    > **注：**需要指出的是，要将 Worker.postMessage 和 window.postMessage 区分开；同时，在 Worker 中使用 postMessage，应该都是 Worker.postMessage。
  
  - **Worker.terminate()：**<font color=FF0000>立即终止 worker</font>。<mark>该方法不会给 worker 留下任何完成操作的机会；就是简单的立即停止</mark>。Service Woker 不支持这个方法。

摘自：[MDN - Worker](https://developer.mozilla.org/zh-CN/docs/Web/API/Worker)

##### worker上下文对象 补充

在专用workers的情况下，DedicatedWorkerGlobalScope 对象代表了worker的上下文（专用workers是指标准worker仅在单一脚本中被使用；共享worker的上下文是 SharedWorkerGlobalScope对象）。一个专用worker仅仅能被首次生成它的脚本使用，而共享worker可以同时被多个脚本使用。

摘自：[MDN - 使用 Web Workers](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API/Using_web_workers) 由于该文章内容很多，且还有很多细节的东西暂时用不到，所以这里略。

#### service worker

Service workers <font color=FF0000>本质上充当 Web 应用程序、浏览器与网络（可用时）之间的代理服务器</font>（<mark> **注：** 即 Proxy，感觉也可以理解为拦截层？ </mark>）。<font color=FF0000>这个 API 旨在创建有效的离线体验</font>（**注：** 读到这里想到了 PWA，查了下便搜到了这篇文章：[通过 Service workers 让 PWA 离线工作](https://developer.mozilla.org/zh-CN/docs/Web/Progressive_web_apps/Offline_Service_workers) 另外，自己的笔记 [[webpack学习笔记#PWA打包]] 部分也有提及：server work 是 pwa 实现的原理。另外，下面也有对这篇文章做摘抄：[[#service worker 补充]] ），<font color=FF0000>它**会拦截网络请求并根据网络是否可用来采取适当的动作、更新来自服务器的的资源**</font>。它还提供入口以推送通知和访问后台同步 API。

<font size=4>**Service worker 的概念和用法**</font>

Service worker 是一个<font color=FF0000>注册在 **指定源和路径下** 的事件驱动worker</font>。它<font color=FF0000>采用 JavaScript 控制关联的页面或者网站，<font size=4> **拦截并修改访问和资源请求，细粒度地缓存资源** </font> </font>。你<font color=FF0000>可以完全控制应用在特定情形（最常见的情形是网络不可用）下的表现</font>（ **注：** <mark>根据语义，service worker 可以控制“在线和离线”两种情况下的“网络请求和资源获取”</mark>）。

<font color=FF0000>Service worker 运行在worker上下文，因此它<font size=4> 不能访问DOM</font></font>。相对于驱动应用的 主JavaScript线程，它运行在其他线程中，所以不会造成阻塞。<font color=FF0000> **它设计为完全异步** ，同步API（如 XHR 和localStorage ）不能在service worker中使用</font>（**注：**xhr 存在同步模式，虽然默认是异步；但是这样说没有错...）。

<font color=FF0000>**出于安全考量，Service workers 只能由 HTTPS 承载**</font>，毕竟修改网络请求的能力暴露给中间人攻击会非常危险

**注意：**

- Service workers 之所以优于以前同类尝试（如 AppCache ），是因为它们（ **注：** 即 AppCache 之类的同类尝试 ）无法支持当操作出错时终止操作（ **注：** 即 Service worker 可以）。<font color=FF0000>Service workers可以更细致地控制每一件事情</font>。
- <font color=FF0000> **Service workers 大量使用 Promise** </font>，因为通常它们会等待响应后继续，并根据响应返回一个成功或者失败的操作。Promise 非常适合这种场景。

下面还有 service worker 使用过相关的内容，如：注册、下载、安装和激活 还有相关接口，由于暂时用不到，这里略
摘自：[MDN - Service Worker API](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API)

##### service worker 补充

<font color=FF0000>Service Worker 是浏览器和网络之间的虚拟代理</font>。 它们终于解决了前端开发人员多年来一直在努力解决的一些问题，其中最值得关注的是，<font color=FF0000>解决了如何正确缓存网站资源并使其在离线时可用的问题</font>。

<mark>Service Worker 运行在 一个与页面 JavaScript 主线程独立的 线程上（ **注：** 即与 js主线程 无关），并且无权访问 DOM 结构</mark>。这引入了一种与传统 Web 编程不同的方式：它的 API 是非阻塞的，并且可以在不同的上下文之间发送和接收信息。<font color=FF0000>您可分配给 Service Worker 一些任务，并通过 <font size=4>基于 Promise 的方法</font> 在任务完成时收到结果</font>。

它<font color=FF0000>不仅仅提供离线功能，还可以做包括处理通知、在单独的线程上执行繁重的计算等事务</font>。Service workers 非常强大，因为他们可以控制网络请求、修改网络请求、返回缓存的自定义响应，或者合成响应。

**离线优先**
“离线优先”或“缓存优先”模式是向用户提供内容的最流行策略。如果资源已缓存且可脱机使用，就在尝试从服务器下载资源之前先将其返回；如果它已经不在缓存中，就下载并缓存以备将来使用。

补充内容摘自：[MDN - 通过 Service workers 让 PWA 离线工作](https://developer.mozilla.org/zh-CN/docs/Web/Progressive_web_apps/Offline_Service_workers)

#####  [科普] Service Worker 入门指南 中的补充

// TODO 

文章简要介绍了 Service Worker 的特点，以及生命周期、运行流程、代码逻辑（ 包含：register  install activate  fetch skipWaiting ），以及一些 API，和 应用场景。有时间做一下笔记。

#### SharedWorker

SharedWorker 接口代表一种特定类型的 worker，可以从几个浏览上下文中访问，例如几个窗口、iframe 或其他 worker。它们实现一个不同于普通 worker 的接口，具有不同的全局作用域, SharedWorkerGlobalScope

**注意 ⚠️：**如果要使 SharedWorker 连接到多个不同的页面，这些页面必须是同源的（相同的协议、host 以及端口）。

##### 构造函数

**SharedWorker()：**创建一个执行指定 url 脚本的共享 web worker。

##### 属性

继承自其父类 EventTarget，并实现 AbstractWorker 中的属性 。

- AbstractWorker.onerror：一个 EventListener，当 ErrorEvent 类型的 error 冒泡到 worker 时触发。
- SharedWorker.port 只读，返回一个 MessagePort 对象，该对象可以用来进行通信和对共享 worker 进行控制。

##### 方法

继承自其父类 EventTarget，并实现 AbstractWorker 中的方法 。

##### 示例：略

摘自：[MDN - SharedWorker](https://developer.mozilla.org/zh-CN/docs/Web/API/SharedWorker)

#### 政采云文章《Web Worker》中的 web worker

内容：讲述了 一般 worker、shared worker、service worker 的功能和使用，浅显易懂。

#### // TODO

摘自：[Web Worker](https://juejin.cn/post/7091068088975622175)

***


## JS 场景实践案例

- 只能输入和粘贴汉字 
  
  ```html
  <input onkeyup="value=value.replace(/[^\u4E00-\u9FA5]/g,'')" onbeforepaste="clipboardData.setData('text',clipboardData.getData('text').replace(/[^\u4E00-\u9FA5]/g,''))" /> 
  ```

- 只能输入和粘贴数字 
  
  ```html
  <input onkeyup="this.value=this.value.replace(/\D/g,'')" 	onafterpaste="this.value=this.value.replace(/\D/g,'')" />
  ```

- 数字脚本 
  
  ```html
  <input onkeyup="if(/\D/.test(this.value)){alert('只能输入数字');this.value='';}" />
  ```

- 只能输入数字和英文 
  
  ```html
  <input onkeyup="value=value.replace(/[\W]/g,'') " onbeforepaste="clipboardData.setData('text',clipboardData.getData('text').replace(/[^\d]/g,''))">
  ```

- 简易禁止输入汉字：输入法不转换，但可粘贴上
  
  ```html
  <input style="ime-mode:disabled">
  ```

- 输入数字和小数点 
  
  ```html
  <input onkeyup="value=value.replace(/[^\d{1,}\.\d{1,}|\d{1,}]/g,'')" />
  ```

- 只能数字和"-",例如在输入时间的时候可以用到 
  
  ```html
  <input onkeyup="value=value.replace(/[^\w&=]|_/ig,'')" onblur="value=value.replace(/[^\w&-]|_/ig,'')" /> 
  ```

摘自：[限制input输入类型(多种方法实现)](https://www.cnblogs.com/eaysun/p/5490603.html)

**补充：**

- 限制输入框输入的只能是金额（非负，小数点后最多两位，自动纠错）

  ```js
  onkeyup="value=value.replace(/[^\d{1,}\.\d{1,}|\d{1,}]/g,'').replace('.','$#$').replace(/\./g,'').replace('$#$','.').replace(/\.{2,}/g,'.').replace(/^(\-)*(\d+)\.(\d\d).*$/,'$1$2.$3')"
  ```

  使用示例如下：

  ```js
  <el-input       onkeyup="value=value.replace(/[^\d{1,}\.\d{1,}|\d{1,}]/g,'').replace('.','$#$').replace(/\./g,'').replace('$#$','.').replace(/\.{2,}/g,'.').replace(/^(\-)*(\d+)\.(\d\d).*$/,'$1$2.$3')" 
  />
  ```

  **注意：**该表达式似乎无法限制最后一个字符的输入，需要将结果使用parseFloat处理

- 限制输入框输入的只能是数量（非负整数）

  ```js
  onkeyup="value=value.replace(/[^\d]/g,'')"
  ```



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

<font color=FF0000>JSONP ( JSON with Padding )</font> 是 <font color=FF0000>json 的一种"使用模式"</font>，<font color=FF0000>可以让网页从**别的域名**（网站）那获取资料，即跨域读取数据。</font>

为什么我们从不同的域（网站）访问数据需要一个特殊的技术( JSONP )呢？这是因为同源策略。

同源策略，它是由 Netscape 提出的一个著名的安全策略，现在所有支持 JavaScript 的浏览器都会使用这个策略。

**补充：**

<font color=FF0000 size=4>**jsonp是一种借助于\<script>标签发送跨域请求的方式**</font>

关于 jsonp 使用的示例，可以参考 [[HTML & CSS备忘录#关于 JSONP 的补充]]


#### JSX

JSX是Javascript和XML结合的一种格式。React发明了JSX，利用HTML语法来创建虚拟DOM。<mark>当遇到\<，JSX就当HTML解析，遇到{就当JavaScript解析。</mark>

#### JS异步执行的运行机制

1. 所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。

2. 主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。

3. 一旦"执行栈"中的所有同步任务执行完毕（同步任务执行完毕），系统就会读取"任务队列"，看看里面有哪些事件（看看哪些异步事件执行完毕，是否产生什么结果）。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。

4. 主线程不断重复上面的第三步。

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
variable = new XMLHttpRequest();
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

<font color=FF0000>**XMLHttpRequest 对象如果要用于 AJAX 的话，其 open() 方法的 async 参数必须设置为 true**</font>：

```js
xmlhttp.open("GET","ajax_test.html",true);
```

<mark>对于 web 开发人员来说，发送异步请求是一个巨大的进步。很多在服务器执行的任务都相当费时。AJAX 出现之前，这可能会引起应用程序挂起或停止</mark>。**<font color=FF0000>通过 AJAX，JavaScript 无需等待服务器的响应</font>**，而是：

- 在等待服务器响应时执行其他脚本
- 当响应就绪后对响应进行处理

<font color=FF0000>当使用 async=true 时，请规定在响应处于 onreadystatechange 事件中的就绪状态时执行的函数</font>，如下示例：

```js
xmlhttp.onreadystatechange=function()
{
    if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
        document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    }
}
xmlhttp.open("GET","/try/ajax/ajax_info.txt",true);
xmlhttp.send();
```

<mark><font color=FF0000>不推荐使用</font> async=false，但是对于一些小型的请求，也是可以的。</mark>

请记住，JavaScript 会等到服务器响应就绪才继续执行。如果服务器繁忙或缓慢，应用程序会挂起或停止。

注意：当您使用 async=false 时，请不要编写 onreadystatechange 函数 - 把代码放到 send() 语句后面即可

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

#### **XMLHttpRequest**

XMLHttpRequest（XHR）对象用于与服务器交互。通过 XMLHttpRequest 可以在不刷新页面的情况下请求特定 URL，获取数据。这允许网页在不影响用户操作的情况下，更新页面的局部内容。XMLHttpRequest 在 AJAX 编程中被大量使用。

```mermaid
classDiagram
XMLHttpRequest --|> XMLHttpRequestEventTarget : Inheritance
XMLHttpRequestEventTarget --|> EventTarget : Inheritance
```

尽管名称如此，XMLHttpRequest 可以用于获取任何类型的数据，而不仅仅是 XML。<font color=FF0000>它甚至支持 HTTP 以外的协议（包括 file:// 和 FTP），尽管可能受到更多出于安全等原因的限制</font>。

**构造函数：**XMLHttpRequest()

该构造函数用于初始化一个 XMLHttpRequest 实例对象。在调用下列任何其他方法之前，必须先调用该构造函数，或通过其他方式，得到一个实例对象。

摘自：[MDN - XMLHttpRequest](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest)

<font size=4>**补充：**</font>

**老版本（Level 1）的XMLHttpRequest对象有以下几个缺点：**

  * <font color=FF0000>只支持文本数据的传送，无法用来读取和上传二进制文件</font>。
  * <font color=FF0000>传送和接收数据时，没有进度信息，只能提示有没有完成。</font>
　　* <font color=FF0000>受到"同域限制"（Same Origin Policy），只能向同一域名的服务器请求数据</font>。

**新版本（Level 2）的XMLHttpRequest对象，针对老版本的缺点，做出了大幅改进。**

  * **可以设置HTTP请求的时限**

    有时，ajax操作很耗时，而且无法预知要花多少时间。如果网速很慢，用户可能要等很久。

    新版本的XMLHttpRequest对象，增加了timeout属性，可以设置HTTP请求的时限。

    ```js
    xhr.timeout = 3000;
    ```

    上面的语句，将最长等待时间设为3000毫秒。过了这个时限，就自动停止HTTP请求。与之配套的还有一个timeout事件，用来指定回调函数。

    ```js
    xhr.ontimeout = function(event){
    　　alert('请求超时！');
    }
    ```

  * **可以使用FormData对象管理表单数据**

    ajax操作往往用来传递表单数据。为了方便表单处理，HTML 5新增了一个FormData对象，可以模拟表单。

  * <font color=FF0000>**可以上传文件**</font>

    新版XMLHttpRequest对象，不仅可以发送文本信息，还可以上传文件。

　　* <font color=FF0000>**可以请求不同域名下的数据（跨域请求）**</font>

  * <font color=FF0000>**可以获取服务器端的二进制数据**</font>

    老版本的XMLHttpRequest对象，只能从服务器取回文本数据（<mark>否则它的名字就不用XML起首了</mark>），新版则可以取回二进制数据。

    这里又分成两种做法：

    - **改写数据的MIMEType（较老）：**

      将服务器返回的二进制数据伪装成文本数据，并且告诉浏览器这是用户自定义的字符集。

      ```js
      xhr.overrideMimeType("text/plain; charset=x-user-defined");
      ```

      然后，用responseText属性接收服务器返回的二进制数据。

      ```js
      var binStr = xhr.responseText;
      ```

      由于这时，浏览器把它当做文本数据，所以还必须再一个个字节地还原成二进制数据。

      ```js
      for (var i = 0, len = binStr.length; i < len; ++i) {
      　　var c = binStr.charCodeAt(i);
      　　var byte = c & 0xff;
      }
      ```

      最后一行的位运算"c & 0xff"，表示在每个字符的两个字节之中，只保留后一个字节，将前一个字节扔掉。原因是浏览器解读字符的时候，会把字符自动解读成Unicode的0xF700-0xF7ff区段。

    - **responseType属性（较新）：**

      从服务器取回二进制数据，较新的方法是使用新增的responseType属性。如果服务器返回文本数据，这个属性的值是"TEXT"，这是默认值。较新的浏览器还支持其他值，也就是说，可以接收其他格式的数据。

      你可以把responseType设为blob，表示服务器传回的是二进制对象。

      ```js
      var xhr = new XMLHttpRequest();
      xhr.open('GET', '/path/to/image.png');
      xhr.responseType = 'blob';
      ```

      接收数据的时候，用浏览器自带的Blob对象即可。

      ```js
      var blob = new Blob([xhr.response], {type: 'image/png'});
      ```

      注意，是读取xhr.response，而不是xhr.responseText。

      你还可以将responseType设为arraybuffer，把二进制数据装在一个数组里。

      ```js
      var xhr = new XMLHttpRequest();
      xhr.open('GET', '/path/to/image.png');
      xhr.responseType = "arraybuffer";
      ```

      接收数据的时候，需要遍历这个数组。

      ```js
      var arrayBuffer = xhr.response;
      if (arrayBuffer) {
      　　var byteArray = new Uint8Array(arrayBuffer);
      　　for (var i = 0; i < byteArray.byteLength; i++) {
      　　　　// do something
      　　}
      }
      ```
      
      **补充：**
      
      XMLHttpRequest 属性 responseType <font color=FF0000>是一个枚举字符串值</font>，<font color=FF0000>用于**指定响应中包含的数据类型**</font>。它还<font color=FF0000>允许更改响应类型</font>。如果<mark>将 responseType 的值设置为空字符串</mark>，则<font color=FF0000>会使用 text 作为默认值</font>。
      
      **可选值**
      
      - **""：**空的 responseType 字符串 <font color=FF0000 size=4>与默认类型 "text" 相同</font>。
      - **"arraybuffer"：**response 是一个包含二进制数据的 JavaScript ArrayBuffer。
      - **"blob"：**response 是一个包含二进制数据的 Blob 对象。
      - **"document"：**response 是一个 HTML Document 或 XML XMLDocument，根据接收到的数据的 MIME 类型而定。
      - **"json"：**response 是通过将接收到的数据内容解析为 JSON 而创建的 JavaScript 对象。
      - **"text"：**response 是 <font color=FF0000>DOMString 对象中的文本</font>。
      - **"ms-stream"：**👎 略
      
      以上内容摘自：[MDN - XMLHttpRequest.responseType](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/responseType)

  * **可以获得数据传输的进度信息**

    新版本的XMLHttpRequest对象，传送数据的时候，有一个progress事件，用来返回进度信息。
    它分成上传和下载两种情况。下载的progress事件属于XMLHttpRequest对象，上传的progress事件属于XMLHttpRequest.upload对象。
    我们先定义progress事件的回调函数：

    ```js
    xhr.onprogress = updateProgress;
    xhr.upload.onprogress = updateProgress;
    ```

    然后，在回调函数里面，使用这个事件的一些属性。

    ```js
    function updateProgress(event) {
    　　if (event.lengthComputable) {
    　　　　var percentComplete = event.loaded / event.total;
    　　}
    }
    ```

    上面的代码中，event.total是需要传输的总字节，event.loaded是已经传输的字节。如果event.lengthComputable不为真，则event.total等于0。

    与progress事件相关的，还有其他五个事件，可以分别指定回调函数：

    - **load事件：**传输成功完成。
    - **abort事件：**传输被用户取消。
    - **error事件：**传输中出现错误。
    - **loadstart事件：**传输开始。
    - **loadEnd事件：**传输结束，但是不知道成功还是失败。

**摘自：**[XMLHttpRequest Level 2 使用指南](http://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html)

XMLHttpRequest一开始只是微软浏览器提供的一个接口，后来各大浏览器纷纷效仿也提供了这个接口，再后来W3C对它进行了标准化，提出了XMLHttpRequest标准。<font color=FF0000>XMLHttpRequest标准又分为Level 1和Level 2</font>。

从上面的解释中可以知道：<font color=FF0000>ajax是一种技术方案，但并不是一种新技术。它依赖的是现有的CSS/HTML/Javascript，而其中最核心的依赖是浏览器提供的XMLHttpRequest对象</font>，是这个对象使得浏览器可以发出HTTP请求与接收HTTP响应。

**一些知识点**

- **如何设置request header：**

  在发送Ajax请求（实质是一个HTTP请求）时，我们可能需要设置一些请求头部信息，比如content-type、connection、cookie、accept-xxx等。xhr提供了setRequestHeader来允许我们修改请求 header。

  ```js
  void setRequestHeader(DOMString header, DOMString value);
  ```

  **注意点：**

  - 方法的第一个参数 header 大小写不敏感，即可以写成content-type，也可以写成Content-Type，甚至写成content-Type;

  - <font color=FF0000>Content-Type的默认值与具体发送的数据类型有关</font>

  - <font color=FF0000>**setRequestHeader必须在open()方法之后，send()方法之前调用，否则会抛错**</font>

  - <font color=FF0000>setRequestHeader可以调用多次，最终的值不会采用覆盖override的方式，而是采用追加append的方式</font>

- **如何获取response header：**
  xhr提供了2个用来获取响应头部的方法：getAllResponseHeaders和getResponseHeader。前者是获取 response 中的所有header 字段，后者只是获取某个指定 header 字段的值。另外，getResponseHeader(header)的header参数不区分大小写。

<font color=FF0000>**// TODO 未看完**</font>

**摘自：**[你真的会使用XMLHttpRequest吗？](https://segmentfault.com/a/1190000004322487)

#### XMLHttpRequest.statusText

<mark>只读属性</mark> XMLHttpRequest.statusText 返回了XMLHttpRequest 请求中由服务器返回的一个DOMString 类型的文本信息，这则信息中也<font color=FF0000>包含了响应的数字状态码</font>。

不同于使用一个数字来指示的状态码 <font color=FF0000>XMLHTTPRequest.status</font>，这个属性<font color=FF0000>包含了返回状态对应的文本信息</font>，<font color=FF0000>例如"OK"或是"Not Found"</font>。如果请求的状态readyState的值为"UNSENT"或者"OPENED"，则这个属性的值将会是一个空字符串。
如果服务器未明确指定一个状态文本信息，则statusText的值将会被自动赋值为"OK"。

摘自：[MDN - XMLHttpRequest.statusText](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/statusText)

#### XMLHttpRequest.withCredentials

XMLHttpRequest.withCredentials  属性<font color=FF0000>是一个Boolean类型</font>，它<font color=FF0000>指示了是否该使用类似cookies,authorization headers(头部授权)或者TLS客户端证书这一类资格证书来<font size=4>**创建一个跨站点访问控制**</font>（cross-site Access-Control）请求</font>。<font color=FF0000>在**同一个站点**下使用withCredentials属性是**无效的**</font>。

此外，这个指示<font color=FF0000>也会被用做响应中cookies 被忽视的标示。**默认值是false**</font>。

如果在发送来自其他域的XMLHttpRequest请求之前，未设置withCredentials 为true，那么就不能为它自己的域设置cookie值。而通过设置withCredentials 为true获得的第三方cookies，将会依旧享受同源策略，因此不能被通过document.cookie或者从头部相应请求的脚本等访问。

**注意：**同域下的XmlHttpRequest 响应，不论其Access-Control- header 设置什么值，都无法为它自身站点设置cookie值，<font color=FF0000>除非它在请求之前将withCredentials 设为true</font>。

摘自：[MDN - XMLHttpRequest.withCredentials](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/withCredentials)

#### XMLHttpRequest.upload

XMLHttpRequest.upload 属性<font color=FF0000>返回一个 XMLHttpRequestUpload对象，用来表示上传的进度</font>。<font color=FF0000>这个对象是不透明的，但是作为一个XMLHttpRequestEventTarget</font>，<font color=FF0000>可以通过对其绑定事件来追踪它的进度</font>。

**可以被绑定在upload对象上的事件监听器如下：**

| 事件        | 相应属性的信息类型                                 |
| ----------- | -------------------------------------------------- |
| onloadstart | <font color=FF0000>**获取开始**</font>             |
| onprogress  | <font color=FF0000>**数据传输进行中**</font>       |
| onabort     | 获取操作终止                                       |
| onerror     | 获取失败                                           |
| onload      | 获取成功                                           |
| ontimeout   | 获取操作在用户规定的时间内未完成                   |
| onloadend   | <font color=FF0000>获取完成（不论成功与否）</font> |

摘自：[MDN - XMLHttpRequest.upload](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/upload)

#### XMLHttpRequestEventTarget.onprogress

XMLHttpRequestEventTarget.onprogress 是<font color=FF0000>在 XMLHttpRequest 完成之前周期性调用的函数</font>。

- **语法**

  ```js
  XMLHttpRequest.onprogress = callback;
  ```

- **值**

  - **callback：**在请求完成之前周期性调用的函数。

- **事件**

  - **event.loaded：**<font color=FF0000>已传输的数据量</font>
  - **event.total：**<font color=FF0000>总共的数据量</font>

```js
XMLHttpRequest.onprogress = function (event) {
  event.loaded;
  event.total;
};
```

摘自：[MDN - XMLHttpRequestEventTarget.onprogress](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget/onprogress)

#### ProgressEvent

ProgressEvent 接口是<font color=FF0000>测量如 HTTP 请求</font>（一个XMLHttpRequest，<mark>或者一个 \<img>，\<audio>，\<video>，\<style> 或 \<link> 等底层资源的加载</mark>）<font color=FF0000>等底层流程进度的事件</font>。

```mermaid
classDiagram
ProgressEvent --|> Event : Inheritance
```

**构造方法**

- **ProgressEvent()：**用给定的参数<font color=FF0000>创建一个 ProgressEvent 事件</font>。

**属性**
**同时继承它的父元素 Event 的属性。**

- **ProgressEvent.lengthComputable：**只读，是一个 Boolean标志，<mark>表示底层流程将需要完成的总工作量和已经完成的工作量是否可以计算</mark>。换句话说，<font color=FF0000>它告诉我们进度是否可以被测量</font>。
- **ProgressEvent.loaded：**只读，是一个 <font color=FF0000>unsigned long long 类型数据</font>，<font color=FF0000>表示底层流程已经执行的工作总量</font>。<mark>可以用这个属性和 ProgressEvent.total 计算工作完成比例</mark>。当使用 HTTP 下载资源，它只表示内容本身的部分，不包括首部和其它开销。
- **ProgressEvent.total：**只读，是一个 <font color=FF0000>unsigned long long 类型数据</font>，<font color=FF0000>表示正在执行的底层流程的工作总量</font>。当使用 HTTP 下载资源，它只表示内容本身的部分，a不包括首部和其它开销。

**方法**
同时继承它的父元素 Event 的方法。

- **ProgressEvent.initProgressEvent()：**🗑👎使用被弃用的 Document.createEvent("ProgressEvent") 方法，来初始化一个已经创建好的 ProgressEvent。

摘自：[MDN - ProgressEvent](https://developer.mozilla.org/zh-CN/docs/Web/API/ProgressEvent)

#### AbortController
AbortController 接口表示一个控制器对象，<font color=FF0000>允许你根据需要 **中止一个或多个 Web 请求**</font>。

你可以使用 AbortController.AbortController() 构造函数创建一个新的 AbortController。使用 AbortSignal 对象可以完成与 DOM 请求的通信。

- **构造函数**
  
   AbortController.AbortController()：创建一个新的 AbortController 对象实例。
   
- **属性**
  
   AbortController.signal <font color=FF0000>只读</font>，返回一个 AbortSignal 对象实例，它可以用来 with/abort 一个 Web(网络)请求。
   
- **方法**
  
   AbortController.abort()：中止一个尚未完成的 Web（网络）请求。这<font color=FF0000>能够中止 fetch 请求及任何响应体的消费和流</font>。
   
   > 当一个请求被终止，它的  readyState 将被置为 XMLHttpRequest.UNSENT (0)，并且请求的 status 置为 0。
   >
   > 摘自：[MDN - XMLHttpRequest.abort()](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/abort)

摘自：[MDN - AbortController](https://developer.mozilla.org/zh-CN/docs/Web/API/AbortController)，AbortController 的相关作用 在 [[web开发工具#请求已经发出去了，如何取消掉这个已经发出去的请求]] 中有详细说明。



#### CanvasRenderingContext2D

##### 绘制矩形

- **CanvasRenderingContext2D.clearRect()**：设置指定矩形区域内（以 ***点 ( x, y )*** 为起点，范围是 ( width, height ) ）<font color=FF0000>所有像素变成透明，并擦除之前绘制的所有内容</font>。
- **CanvasRenderingContext2D.fillRect()：**绘制填充矩形，矩形的起点在 (x, y) 位置，矩形的尺寸是 width 和 height。<font color=FF0000>填充样式由当前的 fillStyle 决定</font>
- **CanvasRenderingContext2D.strokeRect()**：在 canvas 中，<font color=FF0000>**使用当前的笔触样式**，描绘一个起点在 (x, y) 、宽度为 w 、高度为 h 的矩形</font>

##### 绘制文本

- **CanvasRenderingContext2D.fillText()**：<font color=FF0000>在 ( x, y ) 位置绘制 ***填充文本***</font>。详见 [[#CanvasRenderingContext2D.fillText()]]
- **CanvasRenderingContext2D.strokeText()**：<font color=FF0000>在 ( x, y ) 位置绘制 ***描边文本***</font>。详见 [[#CanvasRenderingContext2D.strokeText()]]
- **CanvasRenderingContext2D.measureText()**：返回 TextMetrics 对象。

##### 线型

 ***方法*** 和 ***属性*** 控制如何绘制线

- **CanvasRenderingContext2D.lineWidth**：<font color=FF0000>线的 **宽度**</font>。默认 1.0
- **CanvasRenderingContext2D.lineCap**：<font color=FF0000>***线末端*** 的类型</font>。 允许的值： butt（默认），round，square.
- **CanvasRenderingContext2D.lineJoin**：定义<font color=FF0000>两线相交 ***拐点*** 的类型</font>。允许的值：round，bevel，miter（默认）
- **CanvasRenderingContext2D.miterLimit**：斜接面限制比例。默认 10。**注：**有点没看懂，详见：[MDN - CanvasRenderingContext2D.miterLimit](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/miterLimit)
- **CanvasRenderingContext2D.getLineDash()**：<font color=FF0000>返回当前线段样式的数组</font>，数组包含一组数量为偶数的非负数数字
- **CanvasRenderingContext2D.setLineDash()**：<font color=FF0000>设置当前的线段样式</font>
- **CanvasRenderingContext2D.lineDashOffset**：<font color=FF0000>描述在哪里开始绘制线段</font>

##### 文本样式

属性控制如何设计文本

- **CanvasRenderingContext2D.font**：字体设置。 默认值 10px sans-serif。
- **CanvasRenderingContext2D.textAlign**：<font color=FF0000>***文本对齐*** 设置</font>。 允许的值： start（默认），end，left，right 或 center
- **CanvasRenderingContext2D.textBaseline**：<font color=FF0000>***基线对齐*** 设置</font>。 允许的值： top，hanging，middle，alphabetic（默认），ideographic，bottom
- **CanvasRenderingContext2D.direction**：<font color=FF0000>文本的 ***方向***</font>。 允许的值： ltr， rtl，inherit（默认）

##### 填充和描边样式

填充设计用于图形内部的颜色和样式，描边设计用于图形的边线。

- **CanvasRenderingContext2D.fillStyle**：<font color=FF0000>图形 **内部**</font> 的颜色和样式。 默认 `#000` （黑色）
- **CanvasRenderingContext2D.strokeStyle**：<font color=FF0000>图形 **边线**</font> 的颜色和样式。 默认 `#000`（黑色）

##### 渐变和图案

- **CanvasRenderingContext2D.createLinearGradient()**：创建一个沿着参数坐标指定的线的 ***线性渐变***。
- **CanvasRenderingContext2D.createRadialGradient()**：创建一个沿着参数坐标指定的线的 ***放射性性渐变***。
- **CanvasRenderingContext2D.createPattern()**：使用指定的图片 ( CanvasImageSource ) 创建图案。通过 repetition 变量指定的方向上重复源图片。此方法返回 CanvasPattern对象。

##### 阴影

- **CanvasRenderingContext2D.shadowBlur**：描述模糊效果。 默认 0
- **CanvasRenderingContext2D.shadowColor**：阴影的颜色。 默认 fully-transparent black
- **CanvasRenderingContext2D.shadowOffsetX**：阴影水平方向的偏移量。 默认 0
- **CanvasRenderingContext2D.shadowOffsetY**：阴影垂直方向的偏移量。 默认 0

##### 路径

用来操作对象的路径

- **CanvasRenderingContext2D.beginPath()**：<font color=FF0000>清空子路径列表开始一个新的路径</font>。当你想创建一个新的路径时，调用此方法。
- **CanvasRenderingContext2D.closePath()**：使笔点返回到当前子路径的起始点。它尝试从当前点到起始点绘制一条直线。如果图形已经是封闭的或者只有一个点，那么此方法不会做任何操作。
- **CanvasRenderingContext2D.moveTo()**：<font color=FF0000>将一个新的子路径的起始点移动到 (x，y) 坐标</font>
- **CanvasRenderingContext2D.lineTo()**：<font color=FF0000>使用直线连接子路径的最后的点到 ( x, y ) 坐标</font>
- **CanvasRenderingContext2D.bezierCurveTo()**：添加一个3次贝赛尔曲线路径。该方法需要三个点。 第一、第二个点是控制点，第三个点是结束点。起始点是当前路径的最后一个点，绘制贝赛尔曲线前，可以通过调用 moveTo() 进行修改。
- **CanvasRenderingContext2D.quadraticCurveTo()**：添加一个2次贝赛尔曲线路径。
- **CanvasRenderingContext2D.arc()**：绘制一段圆弧路径， 圆弧路径的圆心在 (x, y) 位置，半径为 r ，根据anticlockwise （默认为顺时针）指定的方向从 startAngle 开始绘制，到 endAngle 结束。
- **CanvasRenderingContext2D.arcTo()**：根据控制点和半径绘制圆弧路径，使用当前的描点(前一个moveTo或lineTo等函数的止点)。根据当前描点与给定的控制点1连接的直线，和控制点1与控制点2连接的直线，作为使用指定半径的圆的切线，画出两条切线之间的弧线路径。
- **CanvasRenderingContext2D.ellipse()**：🧪实验性，添加一个椭圆路径，椭圆的圆心在 ( x, y ) 位置，半径分别是 radiusX 和 radiusY ，按照 anticlockwise （默认顺时针）指定的方向，从 startAngle  开始绘制，到 endAngle 结束。
- **CanvasRenderingContext2D.rect()**：创建一个矩形路径，矩形的起点位置是 ( x, y ) ，尺寸为 width 和 height。

##### 绘制路径

- CanvasRenderingContext2D.fill()：使用当前的样式填充子路径。
- CanvasRenderingContext2D.stroke()：使用当前的样式描边子路径。
- CanvasRenderingContext2D.drawFocusIfNeeded()：如果给定的元素获取了焦点，那么此方法会在当前的路径绘制一个焦点。
- CanvasRenderingContext2D.scrollPathIntoView()：将当前或给定的路径滚动到窗口。
- CanvasRenderingContext2D.clip()：从当前路径创建一个剪切路径。在  clip() 调用之后，绘制的所有信息只会出现在剪切路径内部。例如： 参见 Canvas教程中的 剪切路径 。
- CanvasRenderingContext2D.isPointInPath()：判断当前路径是否包含检测点。
- CanvasRenderingContext2D.isPointInStroke()：判断检测点是否在路径的描边线上。

##### 变换

在 CanvasRenderingContext2D 渲染背景中的对象会有一个当前的变换矩阵，一些方法可以对其进行控制。当创建当前的默认路径，绘制文本、图形和Path2D对象的时候，会应用此变换矩阵。下面列出的方法保持历史和兼容性的原因，是为了SVGMatrix对象现在能够应用于大部分 API ，将来会被替换。

- **CanvasRenderingContext2D.currentTransform**：<font color=FF0000>当前的变换矩阵</font> ( SVGMatrix 对象)
- **CanvasRenderingContext2D.rotate()**：在变换矩阵中增加 <font color=FF0000>***旋转***</font>，角度变量表示一个顺时针旋转角度并且用弧度表示。
- **CanvasRenderingContext2D.scale()**：根据 x 水平方向和 y 垂直方向，为canvas 单位添加 <font color=FF0000>***缩放***</font> 变换。
- **CanvasRenderingContext2D.translate()**：通过在网格中移动 canvas 和 canvas 原点 x 水平方向、原点 y 垂直方向，添加 <font color=FF0000>***平移***</font> 变换
- **CanvasRenderingContext2D.transform()**：使用方法参数描述的矩阵多次叠加当前的变换矩阵。
- **CanvasRenderingContext2D.setTransform()**：重新设置当前的变换为单位矩阵，并使用同样的变量调用 transform() 方法。
- **CanvasRenderingContext2D.resetTransform()**：🧪实验性，使用单位矩阵重新设置当前的变换。

##### 合成

- CanvasRenderingContext2D.globalAlpha：在合成到 canvas 之前，设置图形和图像透明度的值。默认 1.0 (不透明)。
- CanvasRenderingContext2D.globalCompositeOperation：通过 globalAlpha 应用，设置如何在已经存在的位图上绘制图形和图像

##### 绘制图像

- **CanvasRenderingContext2D.drawImage()**：<font color=FF0000>绘制指定的图片</font>。该方法有多种格式，提供了很大的使用灵活性。详见 [[#CanvasRenderingContext2D.drawImage()]]

##### 像素控制

参见 ImageData 对象

- **CanvasRenderingContext2D.createImageData()**：<font color=FF0000>使用指定的尺寸，创建一个新的、空白的 ImageData 对象</font>。所有的像素在新对象中都是透明的。
- **CanvasRenderingContext2D.getImageData()**：<font color=FF0000>返回一个 ImageData 对象</font>，用来描述 canvas 区域隐含的像素数据，这个区域通过矩形表示，起始点为 ( sx, sy) 、宽为 sw、高为 sh
- **CanvasRenderingContext2D.putImageData()**：<font color=FF0000>将数据从已有的 ImageData 绘制到位图上</font>。 如果提供了脏矩形，只能绘制矩形的像素

##### 图像平滑

- CanvasRenderingContext2D.imageSmoothingEnabled：🧪实验性，图像平滑的方式；如果禁用，缩放时，图像不会被平滑处理

##### canvas 状态

CanvasRenderingContext2D渲染环境包含了多种绘图的样式状态（属性有线的样式、填充样式、阴影样式、文本样式）。下面的方法会帮助你使用这些状态：

- CanvasRenderingContext2D.save()：使用栈保存当前的绘画样式状态，你可以使用 restore() 恢复任何改变。
- CanvasRenderingContext2D.restore()：恢复到最近的绘制样式状态，此状态是通过 save() 保存到”状态栈“中最新的元素。
- CanvasRenderingContext2D.canvas：对 HTMLCanvasElement 只读的反向引用。如果和 \<canvas> 元素没有联系，可能为null。

##### 点击区域

- CanvasRenderingContext2D.addHitRegion()：🧪实验性，给 canvas 添加点击区域。
- CanvasRenderingContext2D.removeHitRegion()：🧪实验性，从 canvas 中删除指定 id  的点击区域。
- CanvasRenderingContext2D.clearHitRegions()：🧪实验性，从 canvas 中删除所有的点击区域。

摘自：[MDN - CanvasRenderingContext2D](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D)

#### CanvasRenderingContext2D.fillText()

CanvasRenderingContext2D.fillText() 是 <font color=FF0000>Canvas 2D API 在 (x, y) 位置填充文本的方法</font>。如果选项的第四个参数提供了最大宽度，文本会进行缩放以适应最大宽度

##### 语法

```js
void ctx.fillText( text, x, y, [maxWidth] )
```

##### 参数

- **text：**使用当前的 font, textAlign, textBaseline 和 direction 值对文本进行渲染。
- **x：**文本起点的 x 轴坐标。
- **y：**文本起点的 y 轴坐标。
- **maxWidth：**可选，<font color=FF0000>绘制的最大宽度</font>。如果指定了值，并且经过计算字符串的值比最大宽度还要宽；字体为了适应会水平缩放（如果通过水平缩放当前字体，可以进行有效的或者合理可读的处理）或者使用小号的字体。

##### 示例

```html
<canvas id="canvas"></canvas>
```

```js
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

ctx.font = "48px serif";
ctx.fillText("Hello world", 50, 100);
```

摘自：[MDN - CanvasRenderingContext2D.fillText()](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/fillText)

#### CanvasRenderingContext2D.strokeText()

CanvasRenderingContext2D.strokeText() 是 Canvas 2D API 在给定的 (x, y) 位置绘制文本的方法

**注：**该方法详细内容略。因为无论 ***参数*** 还是 ***效果*** 都和 CanvasRenderingContext2D.fillText() 没大的区别。唯一的区别是 <font color=FF0000>**fillText() 是填充文本**</font>（和一般见到的文本的一样，详见 [MDN - CanvasRenderingContext2D.fillText()](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/fillText) 中的示例），而 <font color=FF0000>**strokeText() 是 描边文本**</font>；效果见 [MDN - CanvasRenderingContext2D.strokeText()](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/strokeText)

#### CanvasRenderingContext2D.drawImage()

Canvas 2D API 中的 CanvasRenderingContext2D.drawImage() 方法提供了多种方式在Canvas上绘制图像。

##### 语法

```js
void ctx.drawImage(image, dx, dy);
void ctx.drawImage(image, dx, dy, dWidth, dHeight);
void ctx.drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
```

##### 参数

![](https://s2.loli.net/2022/05/30/Yy71La3VtzRehrN.jpg)

- **image**：<font color=FF0000>绘制到上下文的元素</font>。允许任何的 canvas 图像源 ( CanvasImageSource )，例如：CSSImageValue，HTMLImageElement，SVGImageElement，HTMLVideoElement，HTMLCanvasElement，ImageBitmap 或者 OffscreenCanvas
- **sx**：( source ) 可选，需要绘制到目标上下文中的，image 的矩形（裁剪）选择框的左上角 X 轴坐标。
- **sy**：( source ) 可选，需要绘制到目标上下文中的，image 的矩形（裁剪）选择框的左上角 Y 轴坐标。
- **sWidth**：可选，需要绘制到目标上下文中的，image 的矩形（裁剪）选择框的宽度。如果不说明，整个矩形（裁剪）从坐标的 sx 和 sy 开始，到 image 的右下角结束。
- **sHeight**：可选，需要绘制到目标上下文中的，image 的矩形（裁剪）选择框的高度。
- **dx**：( destination ) image 的左上角在目标 canvas上 X 轴坐标。
- **dy**：( destination ) image 的左上角在目标 canvas上 Y 轴坐标。
- **dWidth**：可选，image 在目标 canvas 上绘制的宽度。 允许对绘制的 image 进行缩放。如果不说明， 在绘制时，image 宽度不会缩放
- **dHeight**：可选，image 在目标 canvas 上绘制的高度。 允许对绘制的 image 进行缩放。 如果不说明， 在绘制时，image 高度不会缩放

##### 抛出异常

- INDEX_SIZE_ERR：如果 canvas 或者图像矩形区域的宽度或高度为0
- INVALID_STATE_ERR：图像没有数据。
- TYPE_MISMATCH_ERR：提供的原始元素不支持。
- NS_ERROR_NOT_AVAILABLE：图像尚未加载。使用 .complete === true 和 .onload确定何时准备就绪

摘自：[MDN - CanvasRenderingContext2D.drawImage()](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/drawImage)




## 代码规范

#### js 加分号

在阅读教程( [前端面试之彻底搞懂this指向](https://mp.weixin.qq.com/s/hYm0JgBI25grNG_2sCRlTA) )时发现了这样一个问题：

```js
function foo() {
  console.log(this);
}

var obj1 = {
  name: "obj1",
  foo: foo
}; 

var obj2 = {
  name: "obj2"
}

obj1.foo(); // obj1对象
(obj2.foo = obj1.foo)();  // window
```

把倒数第二行注释掉，再运行代码会报错；百思不得其解，最后才发现在obj2定义对象最后加上 `;` 才解决问题；不加上逗号：无法区分，会被认为是 ` var obj2 = {name: "obj2"}(obj2.foo = obj1.foo)()`。

> 没有应该不应该，只有你自己喜欢不喜欢。JavaScript 语法长得 C-like 不代表它本质上和 C 是一类语言，所有直觉性的 “当然应该加分号” 都是保守的、未经深入思考的草率结论。后来新设计的语言里可选分号的多得去了，光是 “可以加分号但是大家都不加” 的语言就有：Go, Scala, Ruby, Python, Swift, Groovy...
>
> 至于说 “很难总结什么时候加不加”，其实真的很简单。<font color=FF0000 size=4>**真正会导致上下行解析出问题的 token 有 5 个：括号，方括号，正则开头的斜杠( / )，加号，减号**</font>。我还从没见过实际代码中用正则、加号、减号作为行首的情况，所以总结下来就是一句话：一行开头是括号或者方括号的时候加上分号就可以了，其他时候全部不需要。其实即使是这两种情况，在实际代码中也颇为少见。
>
> 摘自：[JavaScript 语句后应该加分号么？ - 尤雨溪的回答](https://www.zhihu.com/question/20298345/answer/49551142)
>
> 参考阅读：[JavaScript 语句后应该加分号么？ - 贺师俊的回答 - 知乎](https://www.zhihu.com/question/20298345/answer/14670020) 后面的这篇文章相当于是前面hax文章的简化版[写javascript时要不要省略分号？ - 对角另一面的文章 - 知乎](https://zhuanlan.zhihu.com/p/22998282)



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

//todo

`.toggleClass()`

`.data()`

#### Collapse：折叠插件

# //todo