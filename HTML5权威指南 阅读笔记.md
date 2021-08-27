# HTML5权威指南 阅读笔记



#### P4

HTML5不仅仅是HTML规范的最新版本，它还是一系列用来制作现代富Web内容的相关技术的总称。



#### P5

HTML5的一大改进就是支持在浏览器中直接播放视频和音频文件（也就是说不借助于插件）。这是W3C对插件风靡现象的一种反应。原生（native)多媒体支持再结合其他HTML特性可望大有作为。



#### P6

HTML5引入了一些用来分开元素的含义和内容呈现方式的特性和规则。这是HTML5中的一个重要概念



#### P12

元素名不区分大小写。\<CODE>、\<code>甚至\<CoDe>都会被浏览器视为code元素的开始标签。一般来说，应该认定某种大小写格式并且贯彻始终。近年来更常见的风格是全部使用小写。



#### P13

hr 表示主题的改变。



#### P14

有些元素只能使用一个标签表示，在其中放置任何内容都不符合HTML规范。这类元素称为虚元素（void element)，hr就是这样一个元素。它是一种组织性元素，用来表示内容中段落级别的终止。虚元素有两种表示法。第一种只使用开始标签，如\<hr>；第二种表示法在此基础上加了一个斜杠符号，其形式与空元素一致，如\<hr />



#### P17 使用自定义属性

用户可自定义属性，这种属性必须以data-开头，如下示例：

```html
Enter your name: <input disabled-"true" data-creator-"adam"data-purpose-"collection">
```


这种属性的恰当名称是作者定义属性，有时也称扩展属性。不过我更喜欢使用自定义属性这个常见得多的名称。

自定义属性是对HTML4中“浏览器应当忽略不认识的属性”这种广泛应用的技巧的正式规定。在这类属性名称之前添加前缀 `data-` 是为了避免与HTML的未来版本中可能增加的属性名冲突。自定义属性与CSS和JavaScript结合起来很有用。



#### P17 - P18

用于处理HTML文档的各种软件有一个共同的名称叫做用户代理（user agent)。浏览器是最流行的用户代理，但不是唯一的一种。非浏览器类用户代理现在还很少，以后可能会多起来。



#### P18

HTML文档的外层结构由两个元素确定：DOCTYPE和html。DOCTYPE元素让浏览器明白其处理的是HTML文档。这是用布尔属性HTML表达的：\<IDOCTYPE HTML>。紧跟着DOCTYPE元素的是html元素的开始标签。它告诉浏览器：自此直到html结束标签，所有元素
内容都应作为HTML处理。



#### P20

<font color=FF0000>HTML5规范将元素分为三大类：元数据元素（metadata element)、流元素（flow element)和短语元素（phrasing element)</font>
元数据元素用来构建HTML文档的基本结构，以及就如何处理文档向浏览器提供信息和指示。
另外两种元素略有不同，它们的用途是确定一个元素合法的父元素和子元素范围。短语元素是HTML的基本成分。流元素是短语元素的超集。这就是说，所有短语元素都是流元素，但并非所有流元素都是短语元素。



#### P21

每种元素都能规定自己的属性，这种属性称为局部属性（local attribute)。属性还有另一种类型：全局属性（global attribute).它们用来配置所有元素共有的行为。全局属性可以用在任何一个元素身上，不过这不一定会带来有意义或有用的行为改变。



#### P21-P22

使用accesskey属性可以设定一个或几个用来选择页面上的元素的快捷键。

```html
<form>
	Name: <input type="text" name="name" accesskey="n"/>
	<p/>
	Password: <input type="password" name="password" accesskey="p"/>
	<p/>
	<input type="submit"value="Log In"accesskey="s"/>
</form>
```

此例为三个input元素添加了accesskey属性。其目的是让网页或网站的熟客可以使用快捷键访问经常用到的元素。用来触发accesskey机制的按键组合因平台而异，在Windows系统上是同时按下Alt键和accesskey属性值对应的键。下图展示了accesskey属性的效果。用户可以按Alt+n将键盘焦点转移到第一个input元素，在此输入姓名。接下来按 `Alt+p` 将焦点转到第二个input元素，在此输入密码。然后按 `Alt+s`，这等于按下Log In按钮以提交表单。

<img src="https://i.loli.net/2021/02/22/r7natgOxoX92LjK.png" style="zoom:30%;" />



#### P22

class属性用来将元素<font color=FF0000>归类</font>。这样做通常是为了能够找出文档中的某一类元素或为某一类元素应用CSS样式。

一个元素可以被归入多个类别，为此在class属性值中提供多个用空格分隔的类名即可。



#### P25

contextmenu属性用来为元素设定快捷菜单。这种菜单会在受到触发的时候（例如，Windows用户用鼠标右击时）弹出来。不过到现在（2021-2-22）尚无支持这个属性的浏览器。



#### P26 dir

dir属性用来规定元素中文字的方向。其有效值有两个：1tr(用于从左到右的文字）和rtl（用于从右到左的文字）。



#### P26 draggable

draggable属性是HTML5支持拖放操作的方式之一，用来表示元素是否可被拖放。

dropzone属性是HTML5支持拖放操作的方式之一，与上述draggable属性搭配使用。



#### P28 id

id属性用来给元素分配一个唯一的标识符。

id属性还可以用来导航到文档中的特定位置。倘若有个名为example.html的文档中包含一个id属性值为myelement的元素，那么使用example.html#myelement这个URL即可直接导航至该元素。该URL的末尾部分（#加上元素id值）称为URL片段标识符（fragment identifier)。



#### P29 lang

lang属性用于说明元素内容使用的语言。如下示例：

```html
<p lang="en">Hello - how are you?</p>
<p lang="fr">Bonjour - comment êtes-vous?</p>
<p lang="es">Hola - icómo estás?</p>
```

lang属性值必须使用有效的ISO语言代码。关于如何声明语言的全面说明可参阅这个网址：http://tools.ietf.org/html/bcp47.不过要注意，语言是个复杂的技术性问题。
使用lang属性的目的是让浏览器调整其表达元素内容的方式。比如说，改变使用的引号，在使用了文字朗读器（或别的残障辅助技术）的情况下正确发音。
lang属性还可以用来选择指定语言的内容，以便只显示用户所选语言的内容或对其应用样式。



#### P29 spellcheck

spellcheck属性用来表明浏览器是否应该对元素的内容进行拼写检查。这个属性只有用在用户可以编辑的元素上时才有意义。

```html
<textarea spellcheck="true">This is some mispelled text</textarea>
```



#### P30 - P31 tabindex

<font color=FF0000>HTML页面上的键盘焦点可以通过按Tab键在各元素之间切换。用tabindex属性可以改变默认的转移顺序。</font>

```html
<label>Name: <input type="text" name="name" tabindex="1"/></label>
<p>
<label>City: <input type="text" name="city" tabindex="-1"/></label>
<p>
<label>Country: <input type="text" name="country" tabindex="2"/>x/label>
<p>
<input type="submit" tabindex="3"/>
```

tabindex值为1的元素会第一个被选中。用户按一下Tab键后，tabindex值为2的那个元素会被选中，依次类推。tabindex设置为－1的元素不会在用户按下Tab键后被选中。



#### P31 title<font color=FF0000>属性</font>

title属性提供了元素的额外信息。浏览器通常用这些东西显示工具提示。（类似于Alt）

```html
<a title="Apress Publishing" href="http://apress.com">Visit the Apress site</a>
```

<img src="https://i.loli.net/2021/02/22/dElVPSCJBKivD7Y.png" style="zoom:40%;" />



#### P38 @import

可以用＠import语句将样式从一个样式表导入另一个样式表。

<font color=FF0000>＠import语句必须位于样式表顶端，样式表自己的样式定义不能出现在它之前。</font>

＠import语句用得并不广泛。其中一个原因是不少人并不知道有这个东西；另一个原因则是<font color=FF0000>浏览器处理＠import语句的效率往往不如处理多个link元素并靠样式层叠解决问题</font>。



#### P39 @charset

在CSS样式表中可以出现在＠import语句之前的只有＠charset语句，后者用于声明样式表使用的字符编码。

如果样式表中未声明所使用的字符编码，那么浏览器将使用载入该样式表的HTML文档声明的编码。要是HTML文档也没有声明其编码，那么默认情况下使用的将是UTF-8



#### P40 - P41

除了三种定义样式的方式（元素内嵌、文档内嵌和外部样式表），样式还有另外两个来源。

- **浏览器样式**
  浏览器样式（更恰当的名称是用户代理样式）是元素尚未设置样式时浏览器应用在它身上的默认样式。这些样式因浏览器而略有差异，不过大体一致。
- **用户样式**
  大多数浏览器都允许用户定义自己的样式表。这类样式表中包含的样式称为用户样式。这个功能用的人不多，不过，那些要定义自己的样式表的人往往比较看重这一点，一个特别的原因是这可以让有生理不便的人更容易使用网页。



#### P42

明白了浏览器所要查看的所有样式来源之后，现在可以看看浏览器要显示元素时求索一个CSS属性值的次序。这个次序很明确：

1. 元素内嵌样式（用元素的全局属性style定义的样式）；
2. 文档内嵌样式（定义在style元素中的样式）；
3. 外部样式（用link元素导入的样式）；
4. 用户样式（用户定义的样式）；
5. 浏览器样式（浏览器应用的默认样式）。

