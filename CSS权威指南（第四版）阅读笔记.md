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

<font color=FF0000>这些伪类的顺序可不是随意的，通常推荐的顺序是“link - visited - hover - active”，不过后来改成了“link - visited - focus - hover - active”</font>（解释原因见：P118）



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



#### P94

下面的示例为获得焦点的电子邮件地址输入框设定背景图，一个在输入的地址无效时显示，一个在输入的地址有效时显示。

```html
<style>
input[type="email"]:focus {
	background-position: 100% 50%;
	background-repeat: no-repeat;
}
input[type="email"]:focus:invalid {
	background-image: url(warning.jpg);
}
input[type="email"]:focus:valid {
	background-image: url(checkmark.jpg);
}
</style>

<input type="email">
```

效果如下：

<img src="https://i.loli.net/2021/02/14/L7nVlFkXPe9mQpG.png" style="zoom:50%;" />



#### P96 :target 伪类

URL中有个片段标识符（fragment identifier），它所指向的文档片段（在CSS中）称为目标（target）。URL片段标识符指向的目标元素可以使用 `:target`伪类特别装饰。即便不知道“片段标识符”这个术语，你肯定也见过。 比如下面这URL：

http://www.w3.org/TR/css3-selectors/#target-pseudo

<font color=FF0000>这个URL中的 target-pseudo 部分就是片段标识符，由 `#` 符号标记。如果对应的页面（`http://www.w3.org/TR/css3-selectors/` ）中有ID为target-pseudo的元素，那个元素就是片段标识符的目标。</font>

借助 `:target` 伪类，我们可以突出显示文档中的任何目标元素，或者为不同的目标元素定义不同的样式，例如作为目标的标题使用一个样式，作为目标的表格使用一个样式等。

<font color=FF0000>`:target` 伪类定义的样式在两种情况下不会应用：</font>

- 页面的URL中没有片段标识符。

- 页面的URL中有片段标识符，但是文档中没有与之匹配的元素。



#### P97 :lang伪类

如果<font color=FF0000>想根据文本使用的语言选择元素，可以使用 `:lang()` 伪类</font>。在匹配方式上，<font color=FF0000>`:lang()` 伪类与 `|=` 属性选择符类似。 </font>假设想让使用法语编写的元素倾斜显示，可以编写下述规则中的任何一个：

```css
*:lang(fr) {font-style: italic;}
*[lang|="fr"] {font-style: italic;}
```

伪类选择符与属性选择符之间的主要区别是语言信息有多个来源，有时可能来自元素自身之外。 <font color=FF0000>对属性选择符来说，元素自身必须有lang属性才能匹配</font>。<font color=FF0000>而 `:lang` 伪类能匹配设定了语言的元素的后代</font>。Selectors Level3 是这样规定的：

> 在HTML中，语言可以通过lang属性判断，也可以通过 meta元素和协议（例如HTTP首部）判断。XML使用 xml:lang属性，此外还可能有文档语言专用的方法。

<font color=FF0000>`:lang` 伪类可以使用各种信息，而 `|=` 属性选择符只能用于标记中有lang 属性的元素。因此，**伪类比属性选择符更可靠**，多数情況下是装饰特定语言的理想之选。</font>



#### P98 - P100 :not伪类

<font color=FF0000>选择不满足条件的元素，可以使用 Selectors Level3引入的否定伪类 `:not()` </font>。这个伪类与其他选择符不太一样，而且自身有一些限制。 

`:not()` 伪类依附在元素上，括号中是简单的选择符。<font color=FF0000>根据W3C的定义，简单的选择符指：一个类型选择符（元素）、通用选择符（*），属性选择符（css定义的、或自定义的属性），类选择符（class）、ID选择符（id）<font size="5"> **或**</font> 伪类</font>。

基本上，简单选择符是指没有祖辈 - 后代关系的选择符。

<font color=FF0000>注意定义中的<font size=5>**“或”**</font>，它的意思是 `:not()` 伪类中只能使用其中一个选择符</font>。 不能使用群组选择符，也不能使用连结符，因此不能使用后代选择符，因为后代选择符中分隔元素的空格是连结符。

严格来说，`:not()`伪类的括号中可以使用通用选择符，但这么做意义不大。毕竟，`p:not(*)` 的意思是“选择不是元素的 p元素”。试想，怎么可能存在不是元素的元素。` p:not(p)`与之类似，也选择不了任何元素。 此外，还有`p:not(div)`这样的选择符，即选择不是div元素的p元素，这相当于选择所有p元素。 可见，没有什么缘由这样做。

- <font color=FF0000>**否定伪类不能嵌套**，因此`p:not(:not(p))`是无效的，将被忽略</font>。逻辑上，这与直接使用p没有区别，所以没必要这么做。 <font color=FF0000>此外，在括号中不能引用伪元素，因为伪元素不是简单选择符</font>。

- <font color=FF0000>否定伪类可以串在一起（串联），作用相当于“也不是”</font>。例如，你可能想选择 class为link，但既不是列表项目也不是段落的元素：

```css
*.link:not(li):not(p) {font-style: italic;}
```

这个规则的意思是，“选择class 属性值中包含link这个词，但不是li或p的所有元素”。



#### P100 - P101 伪元素选择符

伪元素与伪类很像，为了实现特定的效果，它在文档中插入虚构的元素<font color=FF0000>CSS2定义了四个基本的伪元素</font>，分别用于<font color=FF0000>装饰元素的首字母</font>，<font color=FF0000>首行</font>，以及<font color=FF0000>创建和装饰“前置”和“后置”内容</font>。CSS2之后的规范又定义了其他伪元素（例如 `::marker`），我们将在相关的章节中探讨。 这一节介绍CSS2 定义的那四个，因为它们由来已久，借此机会还可以讨论伪元素的行为。

伪类使用一个冒号，而伪元素使用一对冒号，例如 `::first-line`。 这么做是为了把伪元素与伪类区分开。 <mark>一开始并不是这样的，在CSS2中，这两种选择符都使用一个冒号。因此，为了向后兼容，浏览器也接受使用单个冒号的伪元素选择符。 但是，不要因为这样就懈怠。 为了确保你编写的 CSS在未来还能继续使用，应该使用正确的冒号个数，毕竟我们无法预知浏览器什么时候不再接受单个冒号的伪元素选择符</mark>。

注意，<font color=FF0000><font size="4">**所有伪元素只能出现在选择符的最后**</font>。p::first-line em是无效的，因为伪元素在选择符的主词前面（主词是选择符中的最后一个元素）。这也表明一个选择符中只能有一个伪元素</font>，不过在CSS以后的版本中可能会取消这一限制。



#### P101 - P103

-  `::first-letter` 伪元素用于<font color=FF0000>装饰**任何非行内元素**的首字母</font>，或者开头的标点符号和首字母（如果文本以标点符号开头）
- `::first-line` 用于装饰元素的<font color=FF0000>首行文本</font>

对 `::first-letter`和 `::first-line` 的限制

目前，`::first-letter` 和 `::first-line`伪元素只能应用到块级元素上，例如标题或段落，不能应用到行内元素上，例如超链接。`::first-line` 和 `::first-letter` 样式中可以使用的CSS属性也有限制，见下表：

允许伪元素使用的属性

|  ::first-letter  |   ::first-line   |
| :--------------: | :--------------: |
|   所有字体属性   |   所有字体属性   |
|   所有背景属性   |   所有背景属性   |
| 所有文本装饰属性 |  所有外边距属性  |
| 所有行内排版属性 |  所有内边距属性  |
| 所有行内布局属性 |   所有边框属性   |
|   所有边框属性   | 所有文本装饰属性 |
|    box-shadow    | 所有行内排版属性 |
|      color       |      color       |
|     opacity      |     opacity      |



#### P103 - P104

使用CSS 可以插入生成的内容（generated content），生成的这些内容可以直接使用 `::before` 和 `::after` 伪元素装饰。示例如下：

```css
h2::before {content: "]]"; color: silver;}
body::after {content: "The End.";}
```



#### P105

继承（inheritance）是指把一个元素的某些属性值传给其后代的机制。 <font color=FF0000>确定应该把哪些值应用到元素上时，用户代理不仅要考虑继承，还要考虑声明的特指度（specificity），以及声明的来源。这个过程称为层叠（cascade）</font>。



#### P105 特指度

我们可以使用多种不同的方法选择元素。 实际上，同一个元素可能会被两个或多个规则选择，而且每个规则的选择符不尽相同。以下面三对规则为例，假设每一对规则匹配相同的元素：

```css
h1 {color: red;}
body h1 {color: green;}

h2.grape {color: purple;}
h2 {color: silver;}

html > body table tr[id="totals"] td ul > li {color: maroon;}
li#answer {color: navy;}
```

每对规则中只有一个能胜出，因为<font color=FF0000>匹配的元素只能显示为其中一个颜色</font>。 那么我们如何知道哪个规则胜出呢？

<mark>答案隐藏在每个选择符的特指度中。<font color=FF0000>**用户代理会计算每个规则中选择符的特指度，然后将其依附到规则中的每个声明上**。 **如果两个或多个属性声明有冲突，特指度最高的声明胜出**</font>。</mark>

选择符的特指度由选择符本身的组成部分决定。 一个特指度值由四部分构成，例如0, 0, 0, 0选择符的特指度通过下述规则确定：

- 选择符中的每个 <font color=FF0000>ID 属性</font>值加<font color=FF0000>0, 1, 0, 0</font>。
- 选择符中的每个<font color=FF0000>类属性值</font>、<font color=FF0000>属性选择</font>或<font color=FF0000>伪类</font>加<font color=FF0000>0, 0, 1, 0</font>。
- 选择符中的每个<font color=FF0000>元素</font>和<font color=FF0000>伪元素</font>加<font color=FF0000>0, 0, 0, 1</font>。伪类到底有没有特指度在CSS2中表述的有些自相矛盾，不过<mark>CSS2.1明确指出，伪元素有特指度</mark>。
- <font color=FF0000>连结符和通用选择符不增加特指度</font>。

**下面给出几个规则中选择符的特指度**

```css
h1 {color: red;} 												/* specificity = 0,0,0,1 */
p em {color: purple;} 									/* specificity = 0,0,0,2 */
.grape {color: purple;} 								/* specificity = 0,0,1,0 */
*.bright {color: yellow;} 							/* specificity = 0,0,1,0 */
p.bright em.dark {color: maroon;} 			/* specificity = 0,0,2,2 */
#id216 {color: blue;} 									/* specificity = 0,1,0,0 */
div#sidebar *[href] {color: silver;} 		/* specificity = 0,1,1,1 */
```

上面冲突示例的结果：

```css
h1 {color: red;} 																								/* 0,0,0,1 */
body h1 {color: green;}																				 	/* 0,0,0,2 (winner)*/

h2.grape {color: purple;} 																			/* 0,0,1,1 (winner) */
h2 {color: silver;} 																						/* 0,0,0,1 */

html > body table tr[id="totals"] td ul > li {color: maroon;} 	/* 0,0,1,7 */
li#answer {color: navy;} 																				/* 0,1,0,1(winner) */
```

<font color=FF0000 size="4">**特指度从左向右比较**</font>



#### P09 ID 和属性选择符的特指度

<font color=FF0000>ID选择符</font> 和 <font color=FF0000>选择id属性的属性选择符</font> 之间在特指度上是有区别的，这一点一定要注意。来看前述示例中的第三对规则：

```css
html > body table tr[id="totals"] td ul > li {color: maroon;} /* 0,0,1,7 */
li#answer {color: navy;} 																			/* 0,1,0,1 (wins) */
```

第二个规则中的ID 选择符（#answer）为选择符的总特指度贡献0, 1, 0, 0。然而，第一个规则中的属性选择符（ [id="totals"] ）为总特指度贡献0, 0, 1, 0。 因此，对下述规则来说，id为meadow的元素将显示为绿色：

```css
#meadow {color: green;} /* 0,1,0,0 */
*[id="meadow"] {color: red;} /* 0,0,1,0 */
```



#### P109 行内样式的特指度

<font color=FF0000>目前见到的特指度都以零开头</font>，因此你可能在想，那一位为什么要存在呢？存在必定有用。<font color=FF0000>那一位是为行内样式声明保留的，行内样式声明的特指度比其他声明都高</font>。 



#### P110 importance

有时某个声明可能非常重要，超过其他所有声明，CSS称之为重要声明（important declaration，原因显而易见）。这种声明要在声明末尾的分号之前插入 `!important` ，例如：

```css
p.dark {color: #333 !important; background: white;}
```

这里，颜色值 `#333` 使用 `!important` 标记，而背景色 `white` 没有。 如果想把两个声明都标记为重要的，每个声明中都要插入 `!important`:

```css
p.dark {color: #333 !important; background: white !important;}
```

`!important` 的位置必须正确，否则声明将失效。`!important` 始终放在声明末尾的分号之前对值为多个关键字的属性（例如font）来说，`!important` 的位置尤其重要：

```css
p.light {color: yellow; font: smaller Times, serif !important;}
```

如果把 `!important` 放在font 声明的其他位置，整个声明都将失效，而且整个样式都不会应用到元素上。

<font color=FF0000>带有 `!important` 的声明对特指度没有影响，但是会与不重要的声明分开处理。 其实，所有带 `!important` 的声明会放在一起，而特指度冲突就在这个范围内解决</font>。同样，非重要的声明作为一个整体，其中的冲突使用特指度解决。 因此，<font color=FF0000>重要声明和非重要声明冲突时，重要声明始终胜出</font>。



#### P112

祖先的属性值会沿着树状图向下传播到后代元素，直到没有后代为止。 <font color=FF0000>属性值绝不向上传播，即元素的样式绝不传给祖辈元素</font>。

在HTML中，**<font color=FF0000>向上传播规则有个例外</font>**：<mark>应用到body 元素上的背景样式会传给 html元素。html是文档的根元素，用于定义渲染文档的画布。<font color=FF0000>这一例外仅发生在为body元素定义了背景而没有为html元素定义背景的情况下</font>。</mark>



#### P112 - P113 不继承

很多属性是不继承的，这通常是为了避免得到意外的结果。例如，<font color=FF0000>border属性就不继承</font>。如果继承边框，文档将变得杂乱无章，除非编写样式时付出额外的精力去掉继承的边框。

此外，<font color=FF0000>基于同样的原因，**多数**盒模型属性也不继承，包括外边距、内边距、背景和边框</font>。试想，你肯定不想让段落中的所有链接都从父元素那里继承30像素的左外边距。其次，<font color=FF0000 size="5">**继承的值没有特指度，连零都没有（在和零比较时，零胜出）**</font>。这看起来似乎只是理论上的不同，但是等你了解到继承的值没有特指度将产生怎样的结果时，便会发现这种差距绝不能忽视（如下示例）。

```html
<style>
* {color: gray;}
h1#page-title {color: black;}
</style>

<h1 id="page-title">Meerkat <em>Central</em></h1>
<p>Welcome to the best place on the web for meerkat information!</p>
```

<img src="https://i.loli.net/2021/02/16/zMrgjALTJi6b2xQ.png" style="zoom: 33%;" />

<font color=FF0000>因为通用选择符应用于全部元素，而且特指度为零，所以它声明的颜色gray 击败继承的颜色black</font>（<font color=FF0000>由于继承的值根本没有特指度</font>）。因此，em元素渲染为灰色，而不是黑色。

<font color=FF0000>这个例子充分体现了滥用通用选择符的潜在危险</font>。 <font color=FF0000>通用选择符能匹配任何元素，它往往会终结继承。 这个问题有变通解决方法，但是通常最好从一开始就不滥用通用选择符</font>。



#### P115 CSS层叠规则

**CSS 的层叠规则如下：**

- 找到匹配特定元素的所有规则。

- <font color=FF0000>按显式权重排序应用到特定元素上的所有声明</font>。以 `!important` 标记的规则比没有这一标记的权重高。
- 按来源排序应用到特定元素上的所有声明。 <mark>声明有三个来源：创作人员、 读者和用户代理。 正常情况下，创作人员编写的样式击败读者提供的样式； 读者样式中以 `!important` 标记的声明比其他样式权重高，包括创作人员编写的样式中以 `!important` 标记的声明； 创作人员和读者样式覆盖用户代理的默认样式</mark>。
- 按特指度排序应用到特定元素上的所有声明。 特指度高的声明具有较高的权重。
- <font color=FF0000>**按声明的前后位置排序**应用到特定元素上的所有声明。 样式表或文档中<font size="4">**靠后的声明权重较高**</font>。**导入的样式表中的声明放在当前样式表中所有声明的前面**</font>。



#### P115

如果两个规则应用到同一个元素上，而其中一个以！important标记，那么有此标记的规则胜出：

```html
<style>
  p {color: gray !important;}
</style>

<p style="color: black;">Well, <em>hello</em> there!</p>
```

虽然<font color=FF0000 size="4">**有个颜色由段落的 style 属性提供，但是 `!important` 标记的规则依然胜出，因此段落将显示为灰色。em元素会继承灰色**</font>。



#### P115 - P116

<font color=FF0000>如果显式权重相同，就要考虑规则的来源</font>。如果匹配元素的两个样式权重相同，而一个在创作人员编写的样式表中，另一个在读者提供的样式表中，那么元素将使用创作人员编写的样式表中的样式。 例如，假设下面两个样式分别来自指定的位置：

```css
p em {color: black;} /* author's style sheet */
p em {color: yellow;} /* reader's style sheet */
```

此时，段落中的强调文本将显示为黑色，而不是黄色，因为同样的权重下，创作人员编写的样式战胜读者提供的样式。<font color=FF0000>然而，如果两个规则都用 `!important` 标记，那情况就变了：</font>

```css
p em {color: black !important;} /* author's style sheet */
p em {color: yellow !important;} /* reader's style sheet */
```

<font color=FF0000>现在，段落中的强调文本将显示为黄色，而不是黑色（即：读者编写的样式胜出）</font>。



#### P116

在声明的权重上，基本要考虑五个方面。下面按<font color=FF0000>权重**从高到低**列出</font>：

- <font color=FF0000>读者提供的样式中以 `!important` 标记的声明</font>。

- 创作人员编写的样式中以 `!important` 标记的声明。

- 创作人员编写的常规声明。

- 读者提供的常规声明。

- 用户代理的默认声明。

创作人员通常只需考虑前四点，因为创作人员编写的任何样式都会覆盖用户代理的默认样株式。



#### P118

因为样式在样式表中的前后位置对效果有影响，所以通常才推荐按照一定的顺序编写链接的样式。 链接样式的推荐顺序是“link-visited-focus-hover-active”（LVFHA），如下所示：

```css
a:active {color: orange;}
a:focus {color: green;}
a:hover {color: red;}
a:link {color: blue;}
a:visited {color: purple;}
```

读过本章的内容之后我们知道，<font color=FF0000>这些选择符的特指度相等，都是0, 0, 1, 0。 因为它们的显式权重、来源和特指度都一样，所以最后一个匹配的规则将胜出</font>。 <mark>单击或激活（例如使用键盘）未访问的链接时，匹配其中四个规则，:link，:focus，:hover和 :active。因此，这四个规则中的最后一个胜出</mark>。对LVFHA顺序来说，:active 将胜出，这通常正是创作人员想要的。



#### P119 - P120 CSS之外的表现提示

文档除了CSS 之外可能包含<font color=FF0000>表现提示（presentational hint）</font>，例如font 元素。 这种表现提示的特指度为o，而且认为出现在创作人员编写的样式表的开头。 表现提示将被创作人员编写的样式或读者提供的样式覆盖，但是不会被用户代理的默认样式覆盖。 css3把CSS 外部的表现提示视作用户代理默认样式表的一部分，而且假定出现在默认样式表的最后（不过，写作本书时，规范没有这么表述）。



#### P121

接受关键字的属性， 所取的关键字必须在那个属性允许使用的关键字范围之内。 <mark>如果两个属性使用相同的关键字，在不同的属性中相同的关键字可能具有不一样的行为。 例如，letter-spacing属性的 normal关键字与font-style属性的 normal关键字就相差很大</mark>。



#### P122 - P123 全局关键字

CSS3定义了几个“全局”关键字，规范中的每个属性都能使用：**inherit，initial和unset.**

- **inherit：**<font color=FF0000>关键字inherit把元素某个属性的值设为与父元素同一属性的值一样</font>。也就是说，<font color=FF0000>这个关键字强制继承，即便是在通常情况下不继承时。 很多时候无需这么做，因为多数属性会自动继承</font>。

  <font color=FF0000>使用inherit 还能把通常情况下不从父元素继承的属性值强制拉过来</font>。比如说，border属性不会继承（这是正确的行为）。如果想让span 继承父元素的边框，只需使用 `span { border: inherit; }`

- **initial：**<font color=FF0000>关键字initial 把属性的值设为预定义的初始值，相当于“重设”值</font>。例如，font-weight属性的默认值是 normal。因此，font-weight: initial的作用与font-weight: normal一样。

- **unset：**<font color=FF0000>关键字 unset是inherit和initial的通用替身</font>。<font color=FF0000>对继承的属性来说，unset的作用与inherit一样；对不继承的属性来说，unset的作用与initial一样</font>。



#### P123 all

<font color=FF0000>inherit、initial、unset这三个全局关键字在所有属性中都可以使用</font>。<font color=FF0000>有个特殊的属性只接受这几个全局关键字：all</font>。

<font color=FF0000>all表示除 direction和 unicode-bidi 之外的所有属性</font>。 因此，如果为一个元素声明 `all: inherit`，意思是除direction和unicode-bidi之外的所有属性都从父元素上继承值。

- 取值：inherit |initial |unset

- 初始值：参见各属性



#### P125 字符串

如果字符串值中有换行,可以转义换行符。CSS 会去掉换行符,就像从未换行一样。因此，下面两个字符串在CSS看来是一样的：

```
"This is the right place \
for a newline."
"This is the right place for a newline."
```

如果真想在字符串中插入一个换行符,在需要换行的地方使用Unicode字符\A：

```
"This is a better place \Afor a newline."
```



#### P128

按照某些属性的定义，取值范围外的数字会被设为与所用值最接近的数（这种行为一般称为 clamping）.



#### P128 弹性值

弹性值（fraction value或flex value）是\<number>后跟fr。因此，单位弹性值是1fr。这个概念由栅格布局引入，用于把布局中不受限制的空间分成几部分。



#### P128

**长度单位分两种：绝对长度单位和<font color=FF0000>相对长度单位</font>**



#### P129 **绝对长度单位：**

#### **绝对长度单位有<font color=FF0000>七个</font>**

- 英寸（in）
- 厘米（cm）
- 毫米（mm）
- 四分之一毫米（q）
- 点（pt）：点是一个标准的印刷度量单位，<mark>1英寸有72个点</mark>
- 派卡（pc）：派卡（pica）也是印刷术语，<mark>1派卡等于12点，1英寸等于6派卡</mark>
- 像素（px）：像素是屏幕上的小点，不过CSS定义的像素较为抽象。<mark>在CSS中，1像素所占的尺寸够1英寸中放下96像素。</mark> 



#### P131 **分辨率单位**

随着媒体查询和响应式设计的出现，为了描述显示器的分辨率，出现了三个新单位。

- **点每英寸（dpi）：**在长为1英寸的范围内能显示的点数。 可以指打印机输出的点，也可以指 LED 屏幕等设备上的物理像素点，或者电子墨水屏（例如Kindle用的）上的图像单元。
- **点每厘米（dpcm）**
- **点每像素单位（dppx）：**CSS中每个px单位显示的点数。从CSS3起，1dppx相当于 96dpi，因为CSS就是按照这个换算比例定义像素单位的。不过，这个比例在CSS未来的版本中可能会变0010A



#### P132 相对长度单位

**<font color=FF0000>相对长度单位中的“相对”是指其长度是相对其他东西而言的</font>。相对长度的实际值（或绝对值）<font color=FF0000>根据不受其控制的因素而变</font>，例如屏幕分辨率，视区宽度、用户的偏好设置等。**比外，有些相对单位的实际尺寸始终相对当前元素，因此在元素之间也有差异。



#### P132 - P133 em和ex单位

按CSS的定义，1em等于元素的 font-size属性值。如果元素的font-size为14像素，那么对那个元素来说，1em就等于14像素。

<font color=FF0000>**理论上**，1em 等于所用字体中小写字母m的宽度</font>。其实，“em”这个名称就是由此而来的。<font color=FF0000>这是一个古老的排版术语。然而，CSS没有采用这个定义</font>。

<font color=FF0000>ex指所用字体中小写字母x的高度</font>。 因此，如果两个段落的字号都是24点，但是使用的字体不同，那么ex的值也不一样。 这是因为不同字体中的x高度有所不同



#### P134 rem单位

与em单位类似，rem也基于声明的字号。二者之间的区别是（很微小），em相对当前元素的字号计算，而 rem 始终相对根元素计算。在HTML中，根元素是html。因此，`font-size: 1rem;` 声明把元素的字号设为与文档根元素的字号一样大。

<font color=FF0000>rem 的实际作用相当于重设字号</font>：不管祖辈元素把字号设为多大，`font-size: 1rem;` 都会把字号还原成根元素设定的大小。通常，<font color=FF0000>1rem是用户的默认字号</font>，除非你（或用户）为根元素设定了其他字号。



#### P134 ch单位

CSS3 新增了一个有趣的单位：ch。这个单位基本上可以理解为“一个字符”。CSS3规范是这样定义的：

> 等于渲染时所用字体中“0”（零，U+0030）字形的进距。

<font color=FF0000>“进距”（advance measure）其实是CSS自造的，对应于字体排印中的“进宽”（advancewidth）</font>。CSS使用“距”是因为有些文字不是从右向左或从左向右写的，而是从上到下或从下到上写的，而此时应该用“进高”而非“进宽”。简单起见，本节采用“进宽”一说。

抛开繁杂的细节，<font color=FF0000>**简单来说**，<font size="4">**字形的进宽指一个字形的起点到下一个字形的起点之间的距离**</font>。 一般情况下，这段距离等于字形本身的宽度加上侧边的间距（**间距可以为正也可以为负**）</font>。



#### P136 视区相关的单位

<font color=FF0000>CSS3 还新增了三个与视区尺寸相关的单位</font>。 <mark>这些单位根据视区的尺寸计算，比如浏览器窗口、可打印区域，移动设备的显示屏等</mark>。

- 视区电度单位（vw）

  这个单位<font color=FF0000>根据视区的宽度计算，然后除以100</font>。因此，如果视区的宽度是937像素，那么1vw等于9.37px。如果视区的宽度有变，例如把浏览器窗口拉宽或缩窄，vw的值随之改变。

- 视区高度单位（vh）

  这个单位<font color=FF0000>根据视区的高度计算，然后除以100</font>。因此，如果视区的高度是650像素，那么1vh等于6.5px。如果视区的高度有变，例如把浏览器窗口拉高或缩矮，vh的值随之改变。

- 视区尺寸最小值单位（vmin）

  <font color=FF0000>这个单位等于视区宽度或高度的1/100，始终取宽度和高度中较小的那个</font>。 因此，如果一个视区的宽度为937像素，高度为650像素，那么1vmin等于6.5px.

- 视区尺寸最大值单位（vmax）

  <font color=FF0000>这个单位等于视区宽度或高度的1/100，始终取宽度和高度中较大的那个</font>。 因此，如果一个视区的宽度为937像素，高度为650像素，那么1vmax等于9.37px。

<mark>这些单位特别适合用于创建全视区界面，例如移动设备的界面，因为元素是根据视区的尺寸而变化的，与文档树中的任何元素都没关系</mark>。 因此，轻易就能填满整个视区，或者至少填满视区的大部分，而不用担心特定情况下视区的具体尺寸。



#### P138 计算值

为方便你做数学计算，CSS 提供了`calc()` 值。括号中可以使用简单的数学算式。<mark>允许使用的运算符有+（加）、-（减）、*（乘）、/（除），以及括号</mark>。另外，<font color=FF0000>`calc()` 不允许做指数运算</font>。

<mark style="background: fuchsia">注意：允许在 `calc()` 中使用括号似乎是浏览器提供的便利措施，因为 `calc()` 的句法定义中没有提到括号。因此，浏览器可能会继续支持，不过使用时要小心</mark>。

<font color=FF0000>在允许使用这些值的地方，都能使用` calc()`：\<length>、\<frequency>、\<angle>、\<time>、\<percentage>、\<number>和\<integer></font>。这些单位类型也都能在 `calc()` 中使用，不过有些限制要注意。

基本的限制是，calc（）会检查括号中各个值的类型，确保是兼容的。 检查的方式如下：

- <font color=FF0000>`+` 和 `-` 号两侧的值必须使用相同的单位类型</font>，或者是 \<number> 和 \<integer>（此时结果为一个\<numbe>值）。因此，`5 + 2.7` 是有效的，结果为7.7；而 `5em + 2.7` 无效，因为一边有长度单位，一边没有。<font color=FF0000>注意，`5em + 20px` 是有效的，因为 em 和 px 都是长度单位</font>。

- <font color=FF0000>计算的两个值中必须有一个是 \<number>（注意，它包括整数）</font>。因此，2.5rem * 2和2 * 2.5rem 都是有效的，结果均为 5rem。而`2.5rem * 2rem` 是无效的，因为这样得到的结果是 5rem<sup>2</sup>，这是面积单位，而不是长度单位了。

- <font color=FF0000>`/` 计算的两个值中右边的那个必须是\<number></font>。左边是\<integer>时，结果为一个\<number>值；杏则，结果的单位与左边的值一样。 因此，`30em / 2.75`是有效的，而`30 / 2.75em`是无效的。

- 此外，<font color=FF0000>任何情况下都不能除以零</font>。`30px / 0` 这样的算式很明显，但有时并不显而易见。

此外还有一个小限制要注意，即**<font color=FF0000> + 和 - 运算符的两侧必须有空白，\* 和 / 没有这一限制，这样有助于避免混淆负数</font>**

除此之外，<font color=FF0000>规范要求用户代理至少要支持 `calc()` 中的算式可以使用20个算子</font>，<mark>算子可以是数字、百分数，也可以是大小量（长度）。<font color=FF0000>超过这一限制的算式应该视为无效的</font></mark>。



#### P139 属性值

在一些CSS 属性中，可以使用样式对应的元素上的 HTML 属性值。 方法是使用 `attr()` 表达式。

例如，使用生成的内容时，可以插入任何属性的值。如下示例：

```css
p::before {content: "[" attr(id) "]";}
```

这个表达式把id 属性的值放在括号里，加在有id属性的段落前面。 

```html
<p id="leadoff">This is the first paragraph.</p>
<p>This is the second paragraph.</p>
<p id="conclusion">This is the third paragraph.</p>
```

<img src="https://i.loli.net/2021/02/17/iPpJ27CbEBMyh9e.png" style="zoom:40%;" />

<font color=FF0000>**理论上**，只要在表达式中指定值的类型，`attr()` 可以获取任何属性的值</font>。



#### P140 - P141 颜色

怎样设定页面中使用的颜色？在HTML中，<font color=FF0000>有两个选择：可以使用数量不多的颜色名，例如red或purple，或者使用有点晦涩的十六进制代码</font>。CSS也支持这两种方法， 不过还提供了其他我觉得更直观的方法。

如果只想使用基本的颜色，最简单的方法是使用颜色的名称。 CSS 把这种颜色称为具名颜色（named color）。早期，CSS中有16个基本的颜色关键字，对应HTML4.01定义的那16个颜色。这些具名颜色见下：

aqua     gray 	 	 navy 		silver

black     green 		olive 		teal

blue 	  lime 	  	purple    white

fuchsia  maroon 	red 		yellow

截至2017年年末，最新的CSS颜色规范包含这16个具名颜色，不过它们属于一个更大的列表，一共有148个颜色关键字。 



#### P141 函数式RGB颜色

有两种颜色值使用函数式RGB 表示法，而不使用十六进制表示法。这种颜色值的一般句法是 rgb（color），其中color是三个值，可以是百分数或整数。百分数的取值范围是0~100％，整数的取值范围是0~255。

<font color=FF0000>一个颜色值中**不能混用**整数和百分数</font>

<font color=FF0000>百分数表示法中可以使用小数</font>， 有时，<font color=FF0000>基于特定的原因，你可能想把红色通道指定为25.5％，绿色通道指定为40％，蓝色通道指定为98.6％</font>:

```css
h2 {color: rgb(25.5%,40%,98.6%);}
```

<font color=FF0000>不支持小数的用户代理应该把小数**归整到最近的整数**，得到 `rgb(26％, 40％, 99％)` </font>。使用三个整数时，各个通道只能使用整数。

<font color=FF0000>不管使用哪种表示法，超出取值范围的数将“裁剪”为最近的边界值</font>。 例如，大于100％或小于0％的值将变成 100％和0％。



#### P143 RGBa

从CSS3起，上述两种函数式RGB 表示法发展成了函数式RGBa表示法。 这种表示法在RGB 的三个通道后面增加了一个 alpha值，即“red-green-blue-alpha＂，简称RGBa。这里的alpha指alpha通道，用于衡量不透明度。

毫无疑问，alpha 值始终是0-1范围内的实数。 超出范围的值要么被忽略， 要么被重置为最近的有效值。<font color=FF0000>alpha值不能使用 \<percentage>表示，即便数值一样也不行</font>。



#### P145 十六进制RGB值

如果每个十六进制数中的内个数字相等，（CSS 允许使用简短表示法。一般的句法是#RGB:

```css
h1 {color: #000;} /* set H1s to black */
h2 {color: #666;} /* set H2s to dark gray */
h3 {color: #FFF;} /* set H3s to white */
```

可以看出，每个颜色值中只有三个数字。可是，00和FF之间的十六进制数需要两个数字，这里总共只有三个数字，这样怎么行呢？

其实，<font color=FF0000>浏览器会把每个数字复制成两个。 因此，#F00 变成了#FF0000，#6FA 变成了#66FFAA，#FFF变成了#FFFFFF，即白色（white）</font>。不是每个颜色都能像这样表示。比如说，中灰色就要写成标准的十六进制表示法，即#808080。 这个颜色不能使用简短表示法表示。与之最接近的是#888，它等同于 #888888。



#### P145 十六进制RGBa颜色

（截至2017年年末）有个新的十六进制表示法在后面添加一个十六进制值，表示 alpha通道的值。 

<mark>与不带 alpha 值的十六进制表示法一样，如果四个十六进制值中的两个数字都一样，可以简写。 因此，#663399AA 可以写成#639A</mark>。 



#### P146 HSL和HSLa颜色

<mark>CSS3新增了HSL表示法（不过与一般的颜色理论不同）</mark>。<font color=FF0000>HSD是Hue（色相）Saturation（饱和度）和Lightness（明度）的简称，其中色相是角度值，取值范围是0 ~ 360，饱和度是从0（无饱和度）~ 100（完全饱和）的百分数，明度是从0（全暗）~ 100（全明）的百分数</font>。

<font color=FF0000>饱和度衡量颜色的强度</font>。 <mark>饱和度为 0％时，不管色相角度为多少，得到的都是不太暗的灰色；饱和度为100％时，在明度一定时，色相最饱满</mark>。<font color=FF0000>明度定义颜色有多暗或多亮</font>。<mark>明度为0％时，不管色相和饱和度为多少，始终为黑色；而明度为100％时，得到的是白色</mark>。

RGB 有对应的 RGBa，类似地，<font color=FF0000>HSL有对应的HSLa</font>。<mark>HSLa在HSL的三个值后面加上一个alpha值，取值范围是0~1</mark>。 



#### P149 颜色关键字

<font color=FF0000>有两个特殊的关键字**可以在任何允许使用颜色值的地方使用**：transparent 和 currentColor</font>。

从名称可以看出，<font color=FF0000>transparent 表示完全透明的颜色。按照 CSS Color Module的定义，它与 `rgba(0, 0, 0, 0)` 等效</font>，而这就是transparent 计算得到的值。 <mark>这个关键字不常用于设定文本颜色，不过却是元素背景色的默认值。 此外，还可以用它为元素定义只占空间但不可见的边框，定义渐变时也常用</mark>，这些话题将在后面的章节中讨论。

<font color=FF0000>currentColor的意思是，“当前元素 color属性计算得到的值”</font>。 对下述规则来说：

```css
main {color: gray; border-color: currentColor;}
```

第一个声明把main元素的前景色设为gray，第二个声明使用 currentColor 复制 color属性计算得到的值（这里是 `rgb(50%, 50%,  50%)`，等同于gray），然后应用到 main元素的边框上。



#### P149 - P150 角度

讲完 HSL中的色相角度，现在比较适合讨论角度单位。 角度一般使用 \<angle> 表示，即一个 \<number> 后跟下列四个单位中的一个。

- deg：度数，完整的圆周是360度。

- grad：<font color=FF0000>百分度</font>（gradian，也叫grade或gon），<font color=FF0000>完整的圆周是400百分度</font>

- rad：弧度，完整的圆周是2π（近似于 6.28）.

- turn：<font color=FF0000>圈数，一个完整的圆周是一圈</font>。这个单位在旋转动画中最有用，比如说让一个元素旋转10圈就是10turn（可惜，复数形式turns 是无效的，将被忽略； 至少截至2017年年末是这样）.



#### P150 时间和频率

<font color=FF0000>属性的值为一段时间时，使用 \<time>表示，它是一个 \<number> 值后跟s（秒）或ms（毫秒）。时间值最常在过渡和动画中使用，用干定义持续时间或延迟时间</font>。

<mark>视听CSS 也能用到时间值，用于定义持续时间或延迟时间。 不过，写作本书时，对视听CSS的支持极其有限</mark>

<mark>视听CSS中还有一种值 \<frequency>它是一个 \<number> 值后跟Hz（赫兹）或kHz（千赫兹）</mark>。一同往常，这个单位的标识符不区分大小写，因此Hz和 hz是等效的。 



#### P151

2017年年末，CSS新增了一个特性。这个特性的术语是“自定义属性”（custom property），不过它的作用其实是在CSS 中创建变量。这个名称词不达意，它并不创建特殊的CSS属性（像color或font 之类的）。示例如下：

```css
html {
	--base-color: #639;
	--highlight-color: #AEA;
}
h1 {color: var(--base-color);}
h2 {color: var(--highlight-color);}
```

<font color=FF0000>自定义标识符以两个连字符开头（`--`）。调用的方法是使用 `var()` 值类型。 注意，这些名称是区分大小写的</font>，因此 `--main-color` 和 `--Main-color` 是完全不同的两个标识符。

这些自定义标识符通常被称为“CSS 变量”，这解释了为什么使用 `var()` 调用它们。