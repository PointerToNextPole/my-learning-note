# CSS权威指南（第四版）阅读笔记



#### P10 - P11：摘抄

**在CSS 中，<font color=FF0000>元素通常有两种形式：置换元素和⾮置换元素</font>。**

- **置换元素**
  	<font color=FF0000>置换元素（replaced element）指用来置换元素内容的部分不由文档内容直接表示</font>。

  ​	在HTML中，最常见的置换元素要数`img`，它的内容由文档之外的图像文件替换。其实，通过下面这个简单的例子可以看出，img元素没有内容：

  ```html
  <img src=＂howdy.gif＂>
  ```

  ​	这段标记只包含一个元素名和一个属性。如不指向外部内容（这里通过src属性指定一个图像），这个元素什么也表示不了。如果指向的图像文件存在，文档会把那个图像显示出来；否则，浏览器什么也不显示，或者显示“图像损坏”占位图。
   	`input`元素类似，根据类型的不同，会替换成单选按钮、复选框或文本输入框。

- 非置换元素
  HTML元素大部分是非置换元素（nonreplaced element），即元素的内容由用户代理（通常是浏览器）在元素自身生成的框中显示。

  例如，`<span>hi there<span>`是非置换元素，用户代理会显示“hi there”文本。

  <font color=FF0000>段落（p）、标题（h1～h5）、单元格（table中的th和td）、列表（list）</font>，以及HTML中<font color=FF0000>其他几乎所有元素</font>都是非置换元素。



#### **P12 - P13**：摘抄

<img src="https://i.loli.net/2021/02/09/OnIoZDwK4CEyzBi.png" style="zoom: 40%;" />

<img src="https://i.loli.net/2021/02/09/HsR7vZkoJEyWqNr.png" style="zoom:40%;" />



#### P14：总结

在html文件中放入xml也是可以的，不过不会有任何效果。示例如下：

```html
<book>
    <maintitle>Cascading Style Sheets: The Definitive Guide</maintitle>
    <subtitle>Third Edition</subtitle>
    <author>Eric A. Meyer</author>
    <publisher>O'Reilly and Associates</publisher>
    <pubdate>November 2006</pubdate>
    <isbn type="print">978-0-596-52733-4</isbn>
</book>
<book>
    <maintitle>CSS Pocket Reference</maintitle>
    <subtitle>Third Edition</subtitle>
    <author>Eric A. Meyer</author>
    <publisher>O'Reilly and Associates</publisher>
    <pubdate>October 2007</pubdate>
    <isbn type="print">978-0-596-51505-8</isbn>
</book>
```

在浏览器中的效果：

<img src="https://i.loli.net/2021/02/09/1oJLyIT2O7esz8n.png" style="zoom:50%;" />

不过可以添加如下代码使其有样式：

```css
book, maintitle, subtitle, author, isbn {display: block;}
publisher, pubdate {display: inline;}
```

效果如下：

<img src="https://i.loli.net/2021/02/09/hGdDUfxa1iHkcB8.png" style="zoom:50%;" />

不过，这些都是基本的样式效果。



#### P16 - P19

**link标签**

```html
<link rel="stylesheet" type="text/css" href="sheet1.css" media="all">
```

- link标签少有关注，但它完全是有效的标签，在HTML规范中已经存在多年，一直等待重用。它的基本作用是把其他文档与当前文档关联起来。<font color=FF0000>CSS使用它链接应用到文档上的样式表</font>。为了正确加载外部样式表，<font color=FF0000>link标签必须放在head元素中，不能放在其他元素中</font>。
  Web浏览器遇到link标签时，会查找并加载指定的样式表，使用样式表中的样式渲染HTML文档。

  类似的还有@import，<font color=FF0000>＠import声明必须放在所在样式表的开头，此外别无限制</font>。

- rel是“relation”（关系）的简称，这里指定的关系是 stylesheet。type属性的值始终为text/css，说明通过link标签加载的数据类型。这样Web浏览器才知道加载的样式表是CSS样式表，然后确定如何处理加载的数据。以后可能会出现其他样式语言，因此最好声明使用的是哪种语言。

- href属性，它的值是样式表的URL，可以是绝对地址，也可以是相对地址，具体由需求而定。前例使用的是相对URL.此外，也可以使用绝对URL，例如 URL  url， http:/I meyerweb. com/sheetI.css.

- media属性，它的值是一个或多个媒体描述符（media descriptor），指明媒体的类型和具有的功能（比如screen projection等）。多个媒体描述符以逗号分开候选样式表

- <font color=FF0000>此外还有候选样式表（alternate stylesheet），定义方式为把rel属性的值设为 alternate stylesheet当用户自己选择，文档才会使用候选样式表渲染</font>。
  <font color=FF0000>如果浏览器支持候选样式表，会使用link元素 title属性的值生成候选样式列表</font>。对下面的示例来说:

  ```html
  <link rel=＂stylesheet＂type=＂text/css＂ href=＂sheet1. css＂ title=＂Default＂>
  <link rel=＂alternate stylesheet＂type=textcs  href=＂bigtext. css＂ title=＂Big Text＂>
  <link rel=＂alternate stylesheet＂ type=＂text/css＂href=＂zany.css＂ title＂Crazy colors！＂>
  ```

  浏览器默认使用第一个样式表（这里是名为“Default”的样式表），此外用户还可以自选择想使用的样式表。

  <font color=FF0000>属性为rel的 stylesheet的link元素设定标题，也就是将其定为**首选样式表**（preferred stylesheet）</font>。这意味着，首选样式表优先于候选样式表，显示文档时默认使用。而选择候选样式表之后，首选样式表就不使用了。<font color=FF0000>如果不为样式表设定标题，那它就是**永久样式表**（ persistent stylesheet），始终用于显示文档</font>。



#### P20

style元素也是一种引入样式表的方式，直接写在文档中：

```html
<style type=＂text/css＂>...</style>
```

style 元素应该始终设定 type 属性。对CSS 文档来说，正确的值是＂text/css＂，这与link元素是一样的。

开始和结束style标签之间的样式称为文档样式表（document stylesheet）或嵌入式样式表（embedded stylesheet，因为这种样式表内嵌在文档中）。style元素可以直接包含应用到文档上的样式，也可以通过＠import指令引入外部样式表.



#### P21 - P22

链接的外部样式表中也有的＠import指令：

```html
＠import url(sheet2.css)
```

与link一样，Web 浏览器遇到＠import 指令时会加载外部样式表，使用其中的样式渲染HTML 文档。 二者之间唯一的主要区别在于句法和指令的位置。 可以看出，<font color=FF0000>＠import指令在style 元素内部，而且必须放在其他CSS规则前面，否则不会起作用</font>。 比如下面的例子：

```html
<style type=＂text/css＂>
	＠import url（styles.css); //＠import 放在开头
	h1｛color: gray;｝
</style>
```

与link一样，<mark>一个文档中可以有多个＠import语句</mark>。然而，不同的是：＠import指令导人的每个样式表都会使用，无法指定候选样式表。 

与link类似，<font color=FF0000>＠import指令也可以显示导入的样式表应用于何种媒体</font>。 方法是在样式表的URL后面提供媒体描述符：

```html
＠import url（sheet2.css）all；
＠import url（blueworld.css）screen；
＠import url（zany.css）projection，print；
```

如果一个外部样式表需要用到另一个外部样式表中的样式，＠import指令的作用就体现出来了。 我们知道，<font color=FF0000>外部样式表不能包含任何文档标记，也就是不能使用link元素，但是可以使用＠import指令</font>。因此，外部样式表中可能包含下述内容：

```html
＠import url（http://example.org/library/layout.css）；
＠import url（basic-text.css）；
＠import url（printer.css）print；
body ｛color:red；｝
h1 ｛color:blue；｝
```



#### P23

除了body元素之外的标签（如head或title），所有HTML标签都能设定style属性.

通常不建议使用 style 属性。 而且，除了 HTML，XML很少使用这个属性。 倘若使用style属性，CSS 的很多重要优点都不复存在了，例如集中管理样式，控制整个文档或网站中所有文档的外观。 在很多方面，行内样式都不比font标签好多少，不过却提供了一定的灵活性。



#### P24

一个规则由两个基本部分构成：选择符（selector）和声明块（declaration block）.声明块由一个或多个声明组成，而一个声明包含一个属性（property）和对应的值（value）。一个样式表由一系列规则构成。 下图展示的是一个规则的各个部分。

<img src="https://i.loli.net/2021/02/09/xOQsGcUPZ5mNIyM.png" style="zoom: 40%;" />



#### P28

创作人员通过媒体查询（media query）定义浏览器在何种媒体环境中使用指定的样式表。过去，实现这一机制的方法是通过link元素或style元素的 media 属性设定媒体类型，或者为＠import或＠media 指令提供媒体描述符，媒体查询更进一步，允许创作人员通过媒体描述符根据指定媒体类型的特性选择样式表。

媒体查询可以在下述几个地方使用：

- link元素的media属性：\<link type="text/css" href="frobozz.css" media="screen,print">
- style元素的media 属性：\<style type="text/css" media="screen,print">...\</style>

- ＠import 声明的媒体描述符部分：@import url(frobozz.css)screen,print;

- ＠media 声明的媒体描述符部分：@media screen,print {...}

媒体查询可以是简单的媒体类型，也可以是复杂的媒体类型和特性的组合。







