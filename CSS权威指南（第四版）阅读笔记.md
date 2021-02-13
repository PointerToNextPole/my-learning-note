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



#### P31

<font color=FF0000>媒体查询中可使用的逻辑关键字有两个：</font>

- **and**

  连接的两个或多个媒体特性必须同时满足条件，整个查询得到的结果才是真值。例如，（color）and （orientation:landscape）and（min-device-width:800px）表示三个条件都要满足：当媒体环境是彩色的、横向放着，而且设备的屏幕宽至少为800像素，才会应用样株式表。

- **not**

  对整个查询取反。 假如所有条件都为真，那样式表不会应用到文档上。 例如，not（color）and（orientation:landscape）and（min-device-width:800px）表示三个条目都满足时，整个语句得到的结果与之相反。 因此，当媒体环境是彩色的、横向放着，而且设备的屏幕宽至少为800像素，样式表不会应用到文档上。 除此之外的情况下，都将应用样式表。

注意，<font color=FF0000>not 关键字只能在媒体查询的开头使用</font>。<mark>写为这样是无效的：（color）and not（min-device-width:800px）。如果真这样写，媒体查询将被忽略</mark>。 还要注意，太旧的浏览器不支持媒体查询，因此会跳过媒体描述符以 not开头的样式表。

<font color=FF0000>媒体设备不支持OR关键字。 不过，分隔多个媒体查询的逗号相当于OR</font>。 例如，screen，print 的意思是，“为屏幕或印刷媒体时应用样式＂；而screen and（max-color:2）or（monochrome）是无效的，会被忽略，正确的写法是 screen and（max-color:2）screen and（monochrome）。

<font color=FF0000>此外还有一个only关键词，**专门用于保证向后兼容**（是的，这是真的）</font>。

- **only**

  在<font color=FF0000>**不支持**媒体查询的旧浏览器中</font><font color=000FF0>隐藏样式表</font>。<mark> 例如，如果想在所有媒体中应用一个样式表，但是只在支持媒体查询的浏览器中应用，可以这样写：`＠import url(new.css) only all`。在支持媒体查询的浏览器中，only关键字被忽略，样式表会应用到文档上。而在不支持媒体查询的浏览器中，媒体类型为 only all，而这是无效的，因此不会应用样式表</mark>。 <font color=FF0000>注意，only关键字只能用在媒体查询的开头</font>。



#### P32

下面列出所有可用的描述符（截至2017年年末）:

<img src="https://i.loli.net/2021/02/09/rXNkB176FcLzytp.png" style="zoom:45%;" />

此外，还有来由两种新增的值

- \<ratio>
- \<resolution>



#### P32

**特性查询**

2015~2016年间，CSS新增了一个功能：<font color=FF0000>根据用户代理是否支持特定的CSS 属性及其值来应用一段样式：这个功能称为**特性查询**（feature query）</font>。

特性查询在结构上与媒体查询很像。 假设我们想在用户代理支持color 属性时 （显然是支持的）为元素设定颜色，可以这样写：

```css
＠supports(color:black){
  body {color:black;}
  h1 {color:purple;}
  h2 {color:navy;}
}
```

上述代码的意思其实是：<font color=FF0000>如果你能识别并处理 color: black这样的属性和值组合，那就应用这段样式；否则，跳过这段样式</font>。如果用户代理不支持＠supports，整段样式都会跳过.

特性查询是渐进增强样式的完美方式。比如说你想在浮动布局之外增加栅格布局，可以保留现有的布局方式，在样式表中添加下面这段样株式：

```css
＠supports(display: grid){
  section #main{
    display: grid;
  }
	/* 去掉旧布局的样式*/
	/* 栅格布局的样式*/
｝
```

这段样式在支持栅格布局的浏览器中应用，（它会覆盖旧的页面布局，然后应用通过栅格实现的新布局。 不支持栅格布局的旧浏览器很可能也不支持＠supports，因此会跳过整段样式，就像没出现过一样。

***

与媒体查询一样，（特性查询也支持使用逻辑运算微假如想在用户代理同时支持栅格布局和CSS 形状时应用一段样式，可以这样写：

```css
＠supports(display:grid) and (shape-outside:circle()) {
	/*栅格和形状样株式*/
}
```

这与下述写法是等效的：

```css
＠supports(display:grid)
	＠supports(shape-outside:circle()) {
		/* 栅格和形状样式 */
	}
}
```



#### P52

<font color=FF0000>**在类选择符和ID选择符之间选择（区别）**</font>

- 如前所示，<font color=FF0000>类（class）可以赋予任意个元素，warning 这个类名赋子了p 元素和 span 元素</font>，比外可以赋予更多的元素。而ID就不同了，<font color=FF0000>**在一个HTML 文档中，一个ID 能且只能使用一次**</font>。因此，如果文档中有个元素的 id 属性值为lead-para，其他元素的id 属性就不能再设为这个值.

  <font color=FF0000>实际上，浏览器不一定总会检查 HTML 中的 ID 是不是唯一的。也就是说，如果HTML 文档中的多个元素具有相同的 ID 属性，相同的样式可能会应用到每个元素上。这是不正确的行为，但却可能发生</font>。 文档中出现多个相同的 ID 值还不利于DOM 脚本编程，因为getElementById()等函数预期只有一个 ID 属性为指定值的元素。

  与类选择符不同，ID选择符不能串在一起使用，因为 ID属性的值不能是以空格分隔的列表。

- <font color=FF0000>类选择符（class）和 ID 选择符之间的另一个区别是，用户代理判断该把哪个样式应用到元素时，ID选择符的权重更高</font>。

此外还要注意，<font color=FF0000>类选择符和 ID 选择符可能是区分大小写的， 这取决于文档语言</font>。 <font color=FF0000>**根据 HTML规范，类和ID 的值是区分大小写的，因此类选择符和ID选择符的大小写必须与文档中的一致**</font>。 因此，对于下述 CSS 和 HTML 而言，元素中的文本不会显示为粗体：

```css
p.criticalInfo {font-weight:bold;}
```

```html
<p class=＂criticalinfo＂>Don't look down.</p>
```

因为字母i的大小写不一样，所以上例中的选择符无法匹配元素。



#### P53 属性选择符

不管是类选择符还是ID 选择符，我们选择的其实都是属性的值。 前两节使用的句法专门针对 HTML，XHTML，SVG和MathML 文档（截至写作本书时）。<mark>在其他标记语言中，这样编写的类选择符和ID选择符可能无法使用（class和id属性或许根本不存在）。为了解决这个问题，CSS2 引入了属性选择符（attribute selector），根据属性及其值选择元素</mark>。 <font color=FF0000>属性选择符大致可以分为四类： 简单属性选择符</font>（simple attribute selectors）、 <font color=FF0000>精准属性值选择符</font>（ exact attribute value selectors）、<font color=FF0000>部分匹配属性值选择符</font>（partial-match attribute value selectors）和<font color=FF0000>起始值属性选择符</font>（leading-value attribute selectors）。



#### P53 - P54 简单属性选择符

如果想选择具有某个属性的元素，而不管属性的值是什么，可以使用简单属性选择符。列如，若想选择县有class 属性（可以包含任何值）的所有h1元素，把文本设为银色。

可以这样写：

```css
h1[class] {
  color: silver;
}
```

对下面的标记来说：

```html
<h1 class=＂hoopla＂>Hello</h1>
<h1>Serenity</h1>
<h1 class=＂fancy＂>Fooling</h1>
```

效果如下：

<img src="https://i.loli.net/2021/02/09/tyRUwG7MXInQBW9.png" style="zoom:40%;" />

此外，还可以基于多个属性选择。 为此，要把多个属性选择符串在一起。 例如，若<mark>想让同时具有href和title属性的 HTML 超链接显示为粗体</mark>，可以这样写：

```css
a[href][title] {font-weight:bold;}
```

对下述标记来说，第一个链接会显示为粗体，而第二个和第三个链接都不会显示为粗体：

```html
<a href="http://www.w3.org/" title="W3C Home">W3C</a><br/>
<a href="http://www.webstandards.org">Standards Info</a><br/>
<a title="Not a link">dead.letter</a>
```



#### P55 - P56 精准属性值选择符

此外，还可以进一步缩小范围，只选择属性为特定值的元素。 比如说我们想把指向 Web服务器上某个文档的超链接显示为粗体，可以这么写：

```css
a[href="http://www.css-discuss.org/about.html"]{
  font-weight: bold;
}
```

这个规则把 `href` 属性的值为 `http://www.css-discuss.orglabout.html` 的a元素显示为粗体。任何变化，即便没有 `www.` 部分，或者换成安全协议 `https`，都无法匹配。

可以为任何元素指定任何属性和值的组合。 然而，如果属性和值的组合在文档中未出现，那么选择符不匹配任何元素。 同样，XML语言也很适合使用这种方式应用样式。

与选择属性时一样，可以把多个属性和值选择符串在一起。例如，若想把 href 属性的值为 http://www.w3.org/，而且title属性的值为W3CHome的HTML超链接显示为两倍字号，可以这样写：

```css
a[href="http://www.w3.org/"][title="W3C Home"]{
	font-size: 200%;
}
```



#### P57 根据部分属性值选择

有时，我们想根据属性值的一部分选择元素，而不是完整的值。CSS 为这种情况提供』多种选择，以不同的方式匹配属性值的子串。这些方式的概述见下表

<p align='center'><font size=4>使用属性选择符匹配子串表</font></p>

|      形式       |                             说明                             |
| :-------------: | :----------------------------------------------------------: |
| [foo \|= "bar"] | 选择的元素<font color=FF0000>有foo属性</font>，且<font color=FF0000>其值以bar和一个英文破折号（U+002D）开头</font>， <font color=FF0000>或者值就是bar本身</font><br>这个规则选择foo属性的值为bar，或者以bar-开头的元素 |
| [foo ~= "bar"]  | 选择的元素<font color=FF0000>有foo属性</font>，且其值是<font color=FF0000>包含bar这个词的**一组词**</font><br>这个规则选择一组以空格分隔的属性 |
| [foo *= "bar"]  | 选择的元素<font color=FF0000>有foo属性</font>，且<font color=FF0000>其值包含子串bar</font> |
| [foo ^= "bar"]  | 选择的元素<font color=FF0000>有foo属性</font>，且<font color=FF0000>其值以bar开头</font> |
| [foo $= "bar"]  | 选择的元素<font color=FF0000>有foo属性</font>，且<font color=FF0000>其值以bar结尾</font> |



#### P70 选择<font color=FF0000>紧邻</font>同胞元素，`+`

为了正常工作，CSS要求两个元素的顺序与“原始顺序”一样。 在前面的示例中，0元素后面是ul元素。因此，可以使用ol+ ul选择后一个元素，但是不能使用相同的句法选择前一个元素。 若想让 `ul + ol` 成功匹配，有序列表必须紧跟在无序列表后面。注意，两个元素之间的文本不影响紧邻同胞连结符的作用。以下述标记片段为例

```html
<div>
    <ol>
        <li>List item 1</li>
        <li>List item 1</li>
        <li>List item 1</li>
    </ol>
    This is some text that is part of the 'div'.
    <ul>
        <li>A list item</li>
        <li>Another list item</li>
        <li>Yet another list item</li>
    </ul>
</div>
```

即使两个列表之间有文本，仍然不妨碍使用 `ol + ul` 匹配第二个列表。 这是<font color=FF0000>因为**处在中间的文本不算是同胞元素，而是父元素div的一部分**。 如果把那段文本放在p元素中，`ol + ul` 就无法匹配第二个列表</font>。此时，可以写成`ol + p + ul` 。



#### P71 选择后续同胞（兄弟结点）

Select入一个新的同胞连结符，名为一般同胞连结符（general siblingcombinator）这个连结符使用波浪号（~）表示，<font color=FF0000>选择**一个元素后面**同属一个父元素的另一个元素</font>。

举个例子。若想让h2后面与它同属一个父元素的ol元素的文本倾斜，可以编写 `h2 ~ ol {font-style:italic;}`。<font color=FF0000>两个元素不一定非得是紧邻同胞，不过是的话也能匹配</font>。



#### P71 伪类选择符

讲到伪类选择符（pseudo-class selector），事情就变得有趣了。 <font color=FF0000>利用这种选择符可以为文档中不一定真实存在的结构指定样式，或者为某些元素 （甚至文档本身） 的特定状态赋予幽灵类</font>。



#### P73

深人讨论之前，<font color=FF0000>对伪类要明确一点：伪类始终指代所依附的元素。 </font>这听起来有点奇怪，但是又理所当然，不是吗？之所以强调这一点，是因为有几个结构伪类容易让人误以为是描述符，认为指代的是后代元素。

下面笔者举个例子。2003年，我的第一个孩子出生时，我（像你一样）在网上宣布了这个好消息。有些人对我表示了祝贺，还用CSS 开起了玩笑，比如 <font color=FF0000>#ericmeyer:first-child</font>选择符。 <font color=FF0000>这个选择符的问题是，**它选择的是我，而不是我的女儿**，而且我必须是爸妈的第一个孩子才行（碰巧我是）。若想正确选择我的第一个孩子，选择符应该是 `#ericmeyer > :first-child`</font>。



#### P74 选择空元素

使用 `:empty` 伪类<font color=FF0000>可以选择没有任何子代的元素</font>，<font color=FF0000>**甚至连文本节点都没有（包括文本和空白）**</font>。CMS 经常生成没有任何内容的空元素，此时便可以使用这个伪类。例如，`p:empty{ display: none; }`能禁止显示空段落。

注意，为了能正确匹配，从解析的角度来看，元素必须真的为空，没有有空白、 可见内容或后代元素。对下面几个元素来说，只有第一个和最后一个能被p:empty 匹配：

```html
<p></p>

<p> </p>

<p>
</p>

<p><!—-a comment--></p>
```

<font color=FF0000>第二个和第三个段落不能被：empty 匹配，因为它们不是空的，而是分别有一个空格和一个换行符</font>。这两种空白都算文本节点，因此也就不是空的。<font color=FF0000>最后一个段落之所以能匹配，是因为注释不是内容，也不是空白</font>。然而，如果在注释的某一边加上一个空格或一个换行符，p:empty就无法匹配了。



#### P76 - P77

`:only-of-type` 匹配同胞中唯一的那种元素（那种元素只有一个），而 `:only-child` 只匹配完全没有同胞的元素（只有着一个元素）。

`:only-of-type` <font color=FF0000>指代的是元素，而不是其他任何东西</font>（就是标记，如div、p等）。 



#### P79

`:first-child` 和 `:last-child` 这两个伪类结合在一起的效果相当于 `:only-child。 下述两个规则选择的是相同的元素：

```css
p:only-child {color:red;}
p:first-child:last-child {background-color:red;}
```

这两个规则在一起把段落设为红底红字（<mark>这么做显然不好</mark>）。



#### P80 选择第一个和最后一个某种元素

除了选择一个元素中的第一个和最后一个子代（`:first-child` 、`:last-child`）之外，还可以选择一个元素中某种元素的第一个或最后一个。 （`:first-of-type`、`:last-of-type`）



#### P81

我们可以把伪类`:first-of-type`、 `last-of-type` 连在一起，达到 `:only-of-type` 的效果。下述两个规则选择的是相同的元素：

```css
table:only-of-type {color:red;}
table:first-of-type:last-of-type {background:red;}
```



#### P81

`:nth-child()` 伪类。 我们<font color=FF0000>可以在括号中填上</font>整数，<font color=FF0000>**甚至是简单的代数式**</font>，选择任何想选择的子元素。

括号中可以使用简单的代数式定义公式。代数式的形式为`an + b` 或 `an - b`，其中 `a` 和 `b` 是具体的整数，n 原封不动。而且，`b` 和 `-b`是可选的，如果不需要，可以不用。<font color=FF0000>这里的n表示0、1、2、3、4，一直到无穷大</font>。

正因为元素从1数起（元素的第一个子元素的下标为 1），要稍微转个圈才能推出 `:nth-child(2n)` 选择的是偶数位的子代，而 `:nth-child(2n+1)` 或 `:nth-child(2n-1)` 选择的是奇数位的子代义你可以选择记住，也可以使用两个特殊的关键字：`even` 和 `odd`。示例如下：

```css
tr:nth-child(odd) {background:silver;}
```

**补充：**摘自 [MDN - :nth-child](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-child)

`a` 和 `b` 都必须为整数，并且元素的第一个子元素的下标为 1。换言之就是，该伪类匹配所有下标在集合 { an + b; n = 0, 1, 2, ...} 中的子元素。<font color=FF0000>另外需要特别注意的是，`an` 必须写在 `b` 的前面，不能写成 `b+an` 的形式</font>



#### P84 - P85

`:nth-last-child()` 它的作用与 `:nth-child()` 一样，只不过是<font color=FF0000>从一组同胞的最后一个元素开始</font>，从后向前计算。

`:nth-child()` 和 `:nth-last-child()` 伪类有对应的 `:nth-of-type()` 和 `:nth-last-of-type()`。



#### P88 已访问链接的<font color=FF0000>隐私保护</font>

有超过十年的时间，已访问的链接可以使用任何可用的 CSS 属性装饰，与未访问链接无异。 然而，大约在2005年，有几个人通过示例揭露，通过视觉样式和简单的DOM 脚本可以判断用户是否访问过特定的页面。例如，对 `:visited { font-weight: bold;}`规则来说，脚本可以找出所有加粗的链接，告诉用户他们访问计哪些网站。 <font color=FF0000>更糟的是，已访问的网站可能会被服务器收集。 不使用脚本的话，还可以通过背景图像达到相同的效果</font>。

<mark>对你来说这可能不是什么严重的问题，但在有些国家，访问某些网站（反对党，未经批准的宗教组织、邪教或腐败网站等）可能招致牢狱之灾。 钓鱼网站还可以利用这一点查出用户访问过哪些在线银行</mark>。<font color=FF0000>鉴于此，相关方采取了两个措施。</font>

首先，<font color=FF0000>只能把**颜色相关的属性**应用到已访问的链接上，包括：color，background-color，column-rule-color，outline-color，border-color，以及各边的边框颜色属性（例如 border-top-color）。**除此之外的属性将被忽略**</font>。<font color=FF0000>此外，`:link` 定义的样式除了应用到未访问的链接上之外，也会应用到已访问的链接上，因此 `:link` 能“装饰所有超链接”，而不只是“装饰所有未访问的超链接”</font>。

其次，如果<font color=FF0000>通过 DOM 查询已访问链接的样式，返回的值跟未访问时一样</font>。 因此，如果把已访问链接的颜色设为紫色，而未访问链接的颜色设为蓝色，那么通过DOM 查询颜色时，返回的是蓝色，而不是紫色。

从2017年年末起，这一行为在所有浏览模式中都提供，而不仅限于“隐私浏览”模式。尽管只能使用有限的CSS 属性区分已访问链接和未访问链接，为了可用性和可访问性，我们还是要充分利用有限的属性把已访问的链接和未访问的链接区分开。



#### P86 - P89

|   伪类   |                             说明                             |
| :------: | :----------------------------------------------------------: |
|  :link   | 指代用作超链接的锚记（即具有href属性），而且<font color=FF0000>指向尚未访问的地址</font> |
| :visited | 指代<font color=FF0000>指向已访问地址的超链接</font>。 <mark>出于安全考虑，能应用到已访问链接上的样式十分有限</mark> |
|  :focus  | 指代<font color=FF0000>当前获得输入焦点的元素</font>，即可以接受键盘输入或以某种方式激活 |
|  :hover  | 指代<font color=FF0000>鼠标指针放置其上的元素</font>，<font color=FF0000>例如鼠标指针悬停在超链接上</font> |
| :active  | 指代<font color=FF0000>由用户输入激活的元素</font>，<mark>例如用户单击超链接时按下鼠标按键的那段时间</mark> |

很多网页都有类似下面的样式：

```css
a:link {color: navy;}
a:visited {color: gray;}
a:focus {color: orange;}
a:hover {color: red;}
a:active {color: yellow;}
```

<font color=FF0000>这些伪类的顺序可不是随意的，通常推荐的顺序是“link - visited - hover - active”，不过后来改成了“link - visited - focus - hover - active”</font>



#### P90 - P91

|      伪类      |                             说明                             |
| :------------: | :----------------------------------------------------------: |
|    :enabled    | 指代<font color=FF0000>启用</font>的用户界面元素（例如表单元素），即接受输入的元素 |
|   :disabled    | 指代<font color=FF0000>禁用</font>的用户界面元素（例如表单元素），即不接受输入的元素 |
|    :checked    |          指代由用户或文档默认选中的单选按钮或复选框          |
| :indeterminate | <font color=FF0000>指代既未选中也没有未选中的单选按钮或复选框； 这个状态只能由DOM脚本设定，不能由用户设定</font> |
|    :default    | 指代<font color=FF0000>默认选中的单选按钮，复选框或选项</font> |
|     :valid     | 指代<font color=FF0000>满足所有数据有效性语义</font>的输入框 |
|    :invalid    | 指代<font color=FF0000>不满足所有数据有效性语义</font>的输入框 |
|   :in-range    | 指代<font color=FF0000>输入的值在最小值和最大值之间的输入框</font> |
| :out-of-range  | 指代<font color=FF0000>输入的值小于控件允许的最小值或大于控件允许的最大值的输入框</font> |
|   :required    |       指代<font color=FF0000>必须输入</font>值的输入框       |
|   :optional    |     指代<font color=FF0000>无需一定输入</font>值的输入框     |
|  :read-write   |      指代<font color=FF0000>可由用户编辑</font>的输入框      |
|   :read-only   |     指代<font color=FF0000>不能由用户编辑</font>的输入框     |

Selectors Level3 为这种状态提供了`:checked` 伪类，但是不知为何，没有 `:unchecked` 伪类。<font color=FF0000>不过，可以使用否定伪类（`:not`）选择未被选中的复选框 `:input[type="checkbox"]:not(:checked)` 只有单选按钮和复选框才能被选中</font>

***

通过 CSS 装饰单选按钮和复选框能实现的效果十分有限。 然而，<font color=FF0000>使用这些伪类能做的事情却是无限的。 例如，可以结合 `:checked` 和紧邻同胞连结符装饰复选框和单选按钮的标注（label），如下所示（**非常有用**）：</font>

```html
<style>
	input[type="checkbox"]:checked + label {
		color: red;
		font-style: italic;
	}
</style>
<input id="chbx" type="checkbox"> <label for="chbx">I am a label</label>
```

