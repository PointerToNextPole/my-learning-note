# HTML & CSS备忘录



## HTML

**\<hr\>**是horizontal rule的缩写



#### \<meta>

HTML \<meta> 元素表示那些不能由其它 HTML 元相关（meta-related）元素（(\<base>、\<link>, \<script>、\<style> 或 \<title>）之一表示的任何元数据信息。

##### **meta 元素定义的元数据的类型包括以下几种：**

- 如果设置了 name 属性，meta 元素提供的是文档级别（document-level）的元数据，应用于整个页面。

- 如果设置了 http-equiv 属性，meta 元素则是编译指令，提供的信息与类似命名的HTTP头部相同。

- 如果设置了 charset 属性，meta 元素是一个字符集声明，告诉文档使用哪种字符编码。

- 如果设置了 itemprop 属性，meta 元素提供用户定义的元数据。

##### **属性**

- **charset：**这个属性声明了文档的字符编码。如果使用了这个属性，其值必须是与ASCII大小写无关（ASCII case-insensitive）的"utf-8"。

- **content：**此属性包含http-equiv 或name 属性的值，具体取决于所使用的值。

- **http-equiv：**

  属性定义了一个编译指示指令。这个属性叫做 **http-equiv**(alent) 是因为所有允许的值都是特定HTTP头部的名称，如下：

  - **content-security-policy：** 它允许页面作者定义当前页的内容策略。 内容策略主要指定允许的服务器源和脚本端点，这有助于防止跨站点脚本攻击。
  - **content-type：** 如果使用这个属性，其值必须是"text/html; charset=utf-8"。注意：该属性只能用于 MIME type 为 text/html 的文档，不能用于MIME类型为XML的文档。
  - **default-style：**设置默认 CSS 样式表组的名称。
  - **x-ua-compatible：** 如果指定，则 content 属性必须具有值 "IE=edge"。用户代理必须忽略此指示。
  - **refresh：** 这个属性指定：
    - 如果 content 只包含一个正整数，则为重新载入页面的时间间隔(秒)；
    - 如果 content 包含一个正整数，并且后面跟着字符串 ';url=' 和一个合法的 URL，则是重定向到指定链接的时间间隔(秒)

**HTML5 新属性**

| 属性       | 值                                                     | 描述                                              |
| :--------- | :----------------------------------------------------- | :------------------------------------------------ |
| charset    | *character_set*                                        | 定义文档的字符编码。                              |
| content    | *text*                                                 | 定义与 http-equiv 或 name 属性相关的元信息。      |
| http-equiv | content-type default-style refresh                     | 把 content 属性关联到 HTTP 头部。                 |
| name       | application-name author description generator keywords | 把 content 属性关联到一个名称。                   |
| scheme     | *format/URI*                                           | HTML5不支持。 定义用于翻译 content 属性值的格式。 |



#### 表单属性

- **maxlength / minlength：**用于\<input>和\<textarea>，作用是限制输入最多的和最少的字符数量

- **autocomplete：**可用于以文本或数字值作为输入的 \<input> 元素 ， \<textarea> 元素, \<select> 元素, 和\<form> 元素。 autocomplete 允许web开发人员指定，如果有任何权限 user agent 必须提供填写表单字段值的自动帮助，并为浏览器提供关于字段中所期望的信息类型的指导。<mark>建议值的来源通常取决于浏览器。 通常，值来自用户输入的过去值，但它们也可能来自预先配置的值。 </mark>



#### CDATA

**CDATA**，意为character data，是标记语言SGML与XML，<font color=FF0000>表示文档的特定部分是普通的字符数据</font>，而不是非字符数据或有特定、限定结构的字符数据。

**摘自：**[wiki - CDATA](https://zh.wikipedia.org/wiki/CDATA)

<font color=FF0000>所有 XML 文档中的文本均会被解析器解析，除了 CDATA 区段（CDATA section）中的文本会被解析器忽略。</font>

CDATA 的形式如下：\<![CDATA[ 文本内容 ]]> 。

CDATA 的文本内容中不能出现字符串 `]]>`。另外，CDATA 不能嵌套。

XML 实例：在 CDATA 标记中的信息被解析器原封不动地传给应用程序，并且不解析该段信息中的任何控制标记。 CDATA 区域是由 `<![CDATA[` 为开始标记，以 `]]>` 为结束标记，注意 CDATA 为大写。

**摘自：**[XML 中的 ﹤!\[CDATA\[]\]>，及其解析](https://blog.csdn.net/qq_33266987/article/details/54288051)

#### PCDATA

PCDATA 指的是<font color=FF0000>被解析的字符数据</font>（Parsed Character Data）。

XML 解析器通常会解析 XML 文档中所有的文本。

<font color=FF0000>当某个 XML 元素被解析时，其标签之间的文本也会被解析：</font>

```xml
<message>此文本也会被解析</message>
```

解析器之所以这么做是因为 XML 元素可包含其他元素，就像这个例子中，其中的 <name> 元素包含着另外的两个元素(first 和 last)：

```xml
<name><first>Bill</first><last>Gates</last></name>
```

而解析器会把它分解为像这样的子元素：

```xml
<name>
   <first>Bill</first>
   <last>Gates</last>
</name>
```

摘自：[W3school - XML CDATA](https://www.w3school.com.cn/xml/xml_cdata.asp)



#### \<figure>

**HTML `<figure>` 元素**代表一段独立的内容，<font color=FF0000>经常与说明（caption）\<figcaption>配合使用，并且作为一个独立的引用单元</font>。当它属于主内容流（main flow）时，它的位置独立于主体。这个标签经常是在主文中引用的图片，插图，表格，代码段等等，当这部分转移到附录中或者其他页面时不会影响到主体。

**效果：**

<img src="https://i.loli.net/2020/11/17/Y2VJ586S1XEKQkN.png" alt="DeAnsA.png" style="zoom:40%;" />

##### **使用说明**

- 通常，`<figure>`是图像，插图，图表，代码片段等，在文档的主流程中引用，但可以移动到文档的另一部分或附录而不影响主流程。
- 作为sectioning root，\<figure>元素的内容轮廓将从文档的主要轮廓中排除。
- 通过在其中插入\<figcaption>（作为第一个或最后一个子元素），可以将标题与`<figure>`元素相关联。图中找到的第一个`<figcaption>`元素显示为图的标题。

摘自：[MDN - \<figure>：可附标题内容元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/figure)



#### \<section>

HTML \<section>元素<font color=FF0000>表示一个包含在HTML文档中的独立部分</font>，它没有更具体的语义元素来表示，一般来说会有包含一个标题。

##### **使用说明**

- 一般通过是否包含一个标题 (\<h1>-\<h6> element) 作为子节点 来 辨识每一个\<section>。
- 如果 \<section> 元素的内容可以单独在多个媒体上发表，应该使用 \<article> 而不是 \<section>。
- 不要把\<section>元素作为一个普通的容器来使用，这是本应该是\<div>的用法（特别是当片段（the sectioning ）仅仅是为了美化样式的时候）。 一般来说，一个 \<section> 应该出现在文档大纲中。

摘自：[MDN - \<section>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/section)



#### box-sizing





#### \<textarea\>完全去除边框：

```css
textarea{
  border: none;
  resize: none;
  cursor: pointer;
}
```

摘自：[textarea 完全去除边框](https://blog.csdn.net/qq_32849999/article/details/102978744)



//TODO

```HTML
<input type="hidden" id="">
```



## CSS

### padding margin border的区别

**示图：注意下面的名词（marign-top...）**

![](https://i.loli.net/2020/08/15/86cKqSQWjda2nOH.gif)

W3C 组织建议把所有网页上的对像都放在一个盒 (box) 中，设计师可以通过创建定义来控制这个盒的属性，这些对像包括段落、列表、标题、图片以及层。<mark>盒模型主要定义四个区域：内容 (content)、<font color=FF0000>内边距(padding)</font>、<font color=FF0000>边框(border)</font> 和<font color=FF0000>外边距(margin)</font></mark>。

**进一步用图说明：**

![](https://i.loli.net/2020/08/15/WhgXSk4sBYTCvmq.jpg)

- margin：层的边框以外留的空白

- background-color：背景颜色

- background-image：背景图片

- padding：层的边框到层的内容之间的空白 （填充）

- border：边框 

- content：内容

#### **margin**

包括 margin-top、margin-right、margin-bottom、margin-left，**控制<font color=FF0000>块级元素</font>之间的距离**，它们是透明不可见的。根据上、 右、下、左的**<font color=FF0000>顺时针规则</font>**，可以写为 

```css
margin: 40px 40px 40px 40px;
```

当上下、左右 margin 值分别一致, 可简写为:

```css
margin: 40px 40px;
```

前一个 40px 代表上下 margin 值，后一个 40px 代表左右 margin 值。
当上下左右 margin 值均一致，可简写为:

```css
margin: 40px;
```

补充：
```css
{
  /*第一个数字表示上下的间距，第二个数字表示左右的间距*/
  /*另外，若为值0，单位pixel不写也可以*/
  margin: 10px 0;
}
/*等价于*/
{
	margin-top: 10px;
	margin-bottom: 10px;
}
```

#### **Padding**

包括 padding-top、padding-right、padding-bottom、padding-left，**控制块级元素内部**，content 与 border 之间的距离。代码和margin类似。

#### 补充

当你想让两个元素的 content 在垂直方向 (vertically) 分隔时，既可以选择 padding-top/bottom，也可以选择 margin-top/bottom，在此建议你尽量使用 padding-top / bottom 来达到你的目的，这是因为 css 中存在 Collapsing margins(折叠的 margins) 的现象。

Collapsing margins: margins 折叠现象只存在于临近或有从属关系的元素，垂直方向的 margin 中。

**详细说明如下：**

- 如果只提供一个，将用于全部的四条边；
- 如果提供两个，第一个用于上－下，第二个用于左－右；
- 如果提供三个，第一个用于上，第二个用于左－右，第三个用于下；
- 如果提供全部四个参数值，将按上－右－下－左的顺序作用于四边。

```css
body { padding: 36px;} //对象四边的补丁边距均为36px 
body { padding: 36px 24px; } //上下两边的补丁边距为36px，左右两边的补丁边距为24px 
body { padding: 36px 24px 18px; } //上、下两边的补丁边距分别为36px、18px，左右两边的补丁边距为24px 
body { padding: 36px 24px 18px 12px; } //上、右、下、左补丁边距分别为36px、24px、18px、12px
```

摘自：[CSS padding margin border属性详解](https://www.cnblogs.com/linjiqin/p/3556497.html)

**补充：**

![](https://i.loli.net/2020/10/06/rKV25lxOgMSGjmL.png)



#### id & class

- **id选择器**

  - id 选择器可以<mark>为标有特定 id 的 HTML 元素指定特定的样式</mark>。

  - HTML元素以id属性来设置id选择器，**<font color=FF0000>CSS 中 id 选择器以 "#" 来定义</font>**。

  - <mark>ID属性不要以数字开头</mark>，数字开头的ID在 Mozilla/Firefox 浏览器中不起作用。

    示例如下：

    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="utf-8"> 
    <title>菜鸟教程(runoob.com)</title> 
    <style>
    #para1
    {
    	text-align:center;
    	color:red;
    } 
    </style>
    </head>
    
    <body>
    <p id="para1">Hello World!</p>
    <p>这个段落不受该样式的影响。</p>
    </body>
    </html>
    ```

- **class 选择器**

  - class 选择器用于描述一组元素的样式，<font color=FF0000>class 选择器有别于id选择器，**class可以在多个元素中使用**</font>。

  - class 选择器在HTML中以class属性表示，在 CSS 中，<font color=FF0000>**类选择器以一个点"."号显示**</font>



#### 多重样式（继承）优先级

**内联样式）Inline style > （内部样式）Internal style sheet >（外部样式）External style sheet > 浏览器默认样式**（类似于就近原则）



#### CSS Cursor

| 值                                | 描述                                                         |
| :-------------------------------- | :----------------------------------------------------------- |
| *url*                             | 需使用的自定义光标的 URL。注释：请在此列表的末端始终定义一种普通的光标，以防没有由 URL 定义的可用光标。 |
| <font color=FF0000>default</font> | <font color=FF0000>默认</font>光标（通常是一个箭头）         |
| <font color=FF0000>auto</font>    | <font color=FF0000>默认</font>。浏览器设置的光标。           |
| crosshair                         | 光标呈现为十字线。                                           |
| pointer                           | 光标呈现为指示链接的指针（一只手）                           |
| move                              | 此光标指示某对象可被移动。                                   |
| e-resize                          | 此光标指示矩形框的边缘可被向右（东）移动。                   |
| ne-resize                         | 此光标指示矩形框的边缘可被向上及向右移动（北/东）。          |
| nw-resize                         | 此光标指示矩形框的边缘可被向上及向左移动（北/西）。          |
| n-resize                          | 此光标指示矩形框的边缘可被向上（北）移动。                   |
| se-resize                         | 此光标指示矩形框的边缘可被向下及向右移动（南/东）。          |
| sw-resize                         | 此光标指示矩形框的边缘可被向下及向左移动（南/西）。          |
| s-resize                          | 此光标指示矩形框的边缘可被向下移动（南）。                   |
| w-resize                          | 此光标指示矩形框的边缘可被向左移动（西）。                   |
| text                              | 此光标指示文本。                                             |
| wait                              | 此光标指示程序正忙（通常是一只表或沙漏）。                   |
| help                              | 此光标指示可用的帮助（通常是一个问号或一个气球）。           |

另外这里的内容可以参考：[MDN - Cursor](https://developer.mozilla.org/zh-CN/docs/Web/CSS/cursor)，这里内容更加全面



### CSS背景

- #### background-color

  定义了元素的背景颜色。CSS中，颜色值通常以以下方式定义:

  - 十六进制 - 如："#ff0000"
  - RGB - 如："rgb(255,0,0)"
  - 颜色名称 - 如："red"

- #### background-image

  描述了元素的背景图像。示例：

  ```css
  body {
  	background-image:url('paper.gif');
  }
  ```

- #### background-repeat

  属性定义背景图像的重复方式。背景图像<mark>可以沿着水平轴，垂直轴，两个轴重复，或者根本不重复</mark>。

  |  **单值**   | **等价于双值（XY两个方向）** |
  | :---------: | :--------------------------: |
  | `repeat-x`  |      `repeat no-repeat`      |
  | `repeat-y`  |      `no-repeat repeat`      |
  |  `repeat`   |       `repeat repeat`        |
  |   `space`   |        `space space`         |
  |   `round`   |        `round round`         |
  | `no-repeat` |    `no-repeat no-repeat`     |

  在双值语法中, 第一个值表示<mark>水平重复行为</mark>, 第二个值表示<mark>垂直重复行为</mark>. 下面是关于每一个值是怎么工作的具体说明:

  |  `repeat`   | 图像会按需重复来覆盖整个背景图片所在的区域. 最后一个图像会被裁剪, 如果它的大小不合适的话. |
  | :---------: | :----------------------------------------------------------: |
  |   `space`   | 图像会尽可能得重复, 但是不会裁剪. 第一个和最后一个图像会被固定在元素(element)的相应的边上, 同时空白会均匀地分布在图像之间. [`background-position`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-position)属性会被忽视, 除非只有一个图像能被无裁剪地显示. 只在一种情况下裁剪会发生, 那就是图像太大了以至于没有足够的空间来完整显示一个图像. |
  |   `round`   | 随着允许的空间在尺寸上的增长, 被重复的图像将会伸展(没有空隙), 直到有足够的空间来添加一个图像. 当下一个图像被添加后, 所有的当前的图像会被压缩来腾出空间. 例如, 一个图像原始大小是260px, 重复三次之后, 可能会被伸展到300px, 直到另一个图像被加进来. 这样他们就可能被压缩到225px.译者注: 关键是浏览器怎么计算什么时候应该添加一个图像进来, 而不是继续伸展. |
  | `no-repeat` | 图像不会被重复(因为背景图像所在的区域将可能没有完全被覆盖). 那个没有被重复的背景图像的位置是由[`background-position`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-position)属性来决定. |

  以上关于background-repeat摘自：[MDN - background-repeat](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-repeat)

- #### **background-attachment**

  决定背景图像的位置是在视口内固定，或者随着包含它的区块滚动。

  - 
    fixed：此关键属性值表示背景相对于视口固定。即使一个元素拥有滚动机制，背景也不会随着元素的内容滚动。
  
- local：此关键属性值表示背景相对于元素的内容固定。如果一个元素拥有滚动机制，背景将会随着元素的内容滚动， 并且背景的绘制区域和定位区域是相对于可滚动的区域而不是包含他们的边框。
  
- scroll：此关键属性值表示背景相对于元素本身固定， 而不是随着它的内容滚动（对元素边框是有效的）。
  
  以上关于background-attachment的内容摘自：[MDN - background-attachment](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-attachment)
  
- #### background-position

  background-position 为每一个背景图片设置初始位置。 这个位置是相对于由 background-origin 定义的位置图层的。

  示例：

  ```css
  /* Keyword values */
  background-position: top;
  background-position: bottom;
  background-position: left;
  background-position: right;
  background-position: center;
  
  /* <percentage> values */
  background-position: 25% 75%;
  
  /* <length> values */
  background-position: 0 0;
  background-position: 1cm 2cm;
  background-position: 10ch 8em;
  
  /* Multiple images */
  background-position: 0 0, center;
  
  /* Edge offsets values */
  background-position: bottom 10px right 20px;
  background-position: right 3em bottom 10px;
  background-position: bottom 10px right;
  background-position: top right 10px;
  
  /* Global values */
  background-position: inherit;
  background-position: initial;
  background-position: unset; 
  ```

  以上关于background-position的内容摘自：[MDN - background-position](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-position)



#### CSS字体

font 属性可以用来作为 font-style, font-variant, font-weight, font-size, line-height 和 font-family 属性的简写，或将元素的字体设置为系统字体。

补充：<mark><font color=FF0000>**line-height CSS 属性**</font>用于设置多行元素的空间量，如多行文本的间距</mark>。<font color=FF0000>**对于块级元素，它指定元素行盒（line boxes）的最小高度。对于非替代的 inline 元素，它用于计算行盒（line box）的高度**</font>。

**用em来设置字体大小**

为了避免Internet Explorer 中无法调整文本的问题，许多开发者使用 em 单位代替像素。em的尺寸单位由W3C建议。1em和当前字体大小相等。在浏览器中默认的文字大小是16px。因此，1em的默认大小是16px。可以通过下面这个公式将像素转换为em：px/16=em



#### CSS链接

四个链接状态：

- **a:link** - 正常，未访问过的链接
- **a:visited** - 用户已访问过的链接
- **a:hover** - 当用户鼠标放在链接上时
- **a:active** - 链接被点击的那一刻

当设置为若干链路状态的样式，也有一些顺序规则：

- a:hover 必须跟在 a:link 和 a:visited后面
- a:active 必须跟在 a:hover后面



#### CSS轮廓

轮廓（outline）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

轮廓（outline）属性指定元素轮廓的样式、颜色和宽度。

<img src="https://s1.ax1x.com/2020/08/24/drTiRO.jpg" style="zoom:80%;" />



#### 分组选择器 & 嵌套选择器

- **分组选择器**

  为了尽量减少代码，你可以使用分组选择器。

  每个选择器用逗号分隔。

- **嵌套选择器**

  在下面的例子设置了三个样式：

  - **p{ }**: 为所有 **p** 元素指定一个样式。
  - **.marked{ }**: 为所有 **class="marked"** 的元素指定一个样式。
  - <mark>**.marked p{ }**: 为所有 **class="marked"** 元素内的 **p** 元素指定一个样式</mark>。
  - <mark>**p.marked{ }**: 为所有 **class="marked"** 的 **p** 元素指定一个样式</mark>。



#### display:none & visibility:hidden

隐藏一个元素可以通过把display属性设置为"none"，或把visibility属性设置为"hidden"。这两种方法会产生不同的结果。

- visibility:hidden可以隐藏某个元素，但<mark>隐藏的元素仍需占用与未隐藏之前一样的空间</mark>。也就是说，该元素虽然被隐藏了，但<mark>仍然会影响布局</mark>。

- display:none可以隐藏某个元素，<mark>且隐藏的元素不会占用任何空间</mark>。也就是说，该元素不但被隐藏了，而且<font color=FF0000>该元素原本占用的空间也会从页面布局中消失</font>。



#### 块和内联元素

- **块元素**是一个元素，占用了全部宽度，<font color=FF0000>在前后都是换行符</font>。

  每个块级元素都从新的一行开始，并且其后的元素也另起一行。（很霸道，一个块级元素独占一行）

  <mark>元素的高度、宽度、行高以及顶和底边距都<font color=FF0000>**可设置**</font></mark>。

  <font color=FF0000>元素宽度在不设置的情况下，**是它本身父容器的100%**（和父元素的宽度一致），除非设定一个宽度</font>。

  **块元素有：\<h1>，\<p>，\<div>**

- **内联元素**只需要必要的宽度，<font color=FF0000>不强制换行</font>。

  和其他元素都在一行上；
  
  <mark>元素的高度、宽度及顶部和底部边距<font color=FF0000>不可设置</font></mark>；
  
  <font color=FF0000>元素的宽度就是它包含的文字或图片的宽度，不可改变。</font>
  
  **内联元素有：\<span>，\<a>**

**使得元素变成块元素和内联元素**

- `li {display:inline;}`：把列表项显示为内联元素
- `span {display:block;}`：把span元素作为块元素

#### **补充：inline-block**

**简单来说：**就是<mark>将<font color=FF0000>**对象**呈现为 **inline** 对象</font>，但是<font color=FF0000>**对象的内容**作为 **block** 对象</font>呈现</mark>。之后的内联对象会被排列在同一行内。比如我们可以给一个 link（a 元素）inline-block 属性值，使其既具有 block 的宽度高度特性又具有 inline 的同行特性。

内联块状元素（inline-block）就是同时具备内联元素、块状元素的特点。

inline-block 元素特点：

1、和其他元素都在一行上；

2、元素的高度、宽度、行高以及顶和底边距都可设置。

**另外：**IE（低版本 IE）本来是不支持 inline-block 的，所以在 IE 中对内联元素使用 display:inline-block，理论上 IE 是不识别的，但使用 display:inline-block 在 IE 下会触发 layout，从而使内联元素拥有了 display:inline-block 属性的表象。

摘自：[block，inline和inline-block概念和区别](https://www.cnblogs.com/KeithWang/p/3139517.html)  [CSS： inline、block和inline-block的区别](https://www.cnblogs.com/adongyo/p/11290826.html)



#### CSS Position

元素可以使用的顶部，底部，左侧和右侧属性定位。然而，这些属性无法工作，除非是先设定position属性。他们也有不同的工作方式，这取决于定位方法。

position 属性的五个值：

- **static**：HTML 元素的<font color=FF0000>默认值</font>，即没有定位，遵循正常的文档流对象。

  浏览器会按照源码的顺序，决定每个元素的位置，这称为“正常的页面流”（normal flow）。每个块级元素占据自己的区块（block），元素与元素之间不产生重叠，这个位置就是元素的默认位置。

  <mark>注意，static定位所导致的元素位置，是浏览器自主决定的，所以这时top、bottom、left、right这四个属性无效。</mark>

- **relative**：相对定位元素的定位是相对其正常位置。

  relative表示，相对于默认位置（即static时的位置）进行偏移，即定位基点是元素的默认位置。

  它<mark>必须搭配top、bottom、left、right这四个属性一起使用，用来指定偏移的方向和距离</mark>。

- **fixed**：<mark>元素的位置相对于浏览器窗口是固定位置</mark>。

  它如果搭配`top`、`bottom`、`left`、`right`这四个属性一起使用，表示元素的初始位置是基于视口计算的，否则初始位置就是元素的默认位置。

- **absolute**：<font color=FF0000>**绝对定位的元素的位置相对于最近的<mark>已定位</mark>父元素**</font>，如果元素没有已定位的父元素，那么它的位置相对于\<html>

  absolute表示，相对于上级元素（一般是父元素）进行偏移，即定位基点是父元素。

  <font color=FF0000>它有一个重要的限制条件：定位基点（一般是父元素）不能是static定位，否则定位基点就会变成整个网页的根元素html</font>。另外，<mark>absolute定位也必须搭配top、bottom、left、right这四个属性一起使用</mark>。

- **sticky**：sticky 英文字面意思是粘，粘贴，所以可以把它称之为粘性定位。**position: sticky;** 基于用户的滚动位置来定位。粘性定位的元素是依赖于用户的滚动，在 **position:relative** 与 **position:fixed** 定位之间切换。

  **使用场景**：<font color=FF0000>网页的搜索工具栏，初始加载时在自己的默认位置（relative定位），页面向下滚动时，工具栏变成固定位置，始终停留在页面头部（fixed定位）</font>。

  <mark>sticky<font color=FF0000>**生效的前提**</font>是，<font color=FF0000>必须搭配top、bottom、left、right这四个属性一起使用，不能省略</font>，否则等同于relative定位，不产生"动态固定"的效果</mark>。原因是这四个属性用来定义"偏移距离"，浏览器把它当作sticky的生效门槛。

  它的<font color=FF0000>**具体规则**</font>是，<font color=FF0000>当页面滚动，父元素开始脱离视口时（即部分不可见），只要与sticky元素的距离达到生效门槛，relative定位自动切换为fixed定位</font>；<font color=FF0000>等到父元素完全脱离视口时（即完全不可见），fixed定位自动切换回relative定位。</font>

  注意，除了已被淘汰的 IE 以外，其他浏览器目前都支持`sticky`。但是，Safari 浏览器需要加上浏览器前缀`-webkit-`。

  部分摘自：[阮一峰的网络日志 - CSS 定位详解](https://www.ruanyifeng.com/blog/2019/11/css-position.html)

**重叠的元素**

元素的定位与文档流无关，所以它们可以覆盖页面上的其它元素。<mark>z-index属性指定了一个元素的堆叠顺序（哪个元素应该放在前面，或后面）</mark>。一个元素可以有正数或负数的堆叠顺序，<font color=FF0000>具有更高堆叠顺序的元素总是在较低的堆叠顺序元素的前面</font>。



#### CSS 布局 - Overflow

CSS overflow 属性<mark>可以控制内容溢出元素框时在对应的元素区间内添加滚动条</mark>。

overflow属性有以下值：

|   值    |                             描述                             |
| :-----: | :----------------------------------------------------------: |
| visible |         默认值。内容不会被修剪，会呈现在元素框之外。         |
| hidden  |            内容会被修剪，并且其余内容是不可见的。            |
| scroll  | <mark>内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容</mark>。 |
|  auto   |   如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。   |
| inherit |           规定应该从父元素继承 overflow 属性的值。           |

**注意：**overflow 属性只工作于指定高度的块元素上。



#### CSS Float

CSS 的 Float（浮动），会使元素向左或向右移动，其周围的元素也会重新排列。

<mark>Float（浮动），**往往是用于图像**</mark>，但它在布局时一样非常有用。

```css
float: right;
```

**清除浮动 - 使用 clear**

元素浮动之后，周围的元素会重新排列，为了避免这种情况，使用 clear 属性。

示例：

```css
.text_line
{
    clear:both;
}
```



#### 水平 & 垂直对齐

- 文本居中对齐

  ```css
  text-align: center;
  ```

- 居中对齐

  ```css
  margin: auto;
  ```
  
  另外，需要注意的是：对于行内块级元素（display:inline-block），想要用`margin: auto;`实现居中对齐，需要加上`display: block`。比如：\<button>



#### CSS 组合选择符

在 CSS3 中包含了四种组合方式：

- **后代选择器**（以空格` `分隔）

  后代选择器用于选取某元素的后代元素，示例：

  ```css
  /*选取所有 <p> 元素插入到 <div> 元素中*/
  div p
  {
    background-color:yellow;
  }
  ```

- **子元素选择器**（以大于`>`号分隔）

  后代选择器用于选取某元素的后代元素，示例：

  ```css
  /*选择了<div>元素中所有直接子元素 <p> */
  div>p
  {
    background-color:yellow;
  }
  ```

- **相邻兄弟选择器**（以加号`+`分隔）

  相邻兄弟选择器（Adjacent sibling selector）可选择紧接在另一元素后的元素，且二者有相同父元素。如果需要选择紧接在另一个元素后的元素，而且二者有相同的父元素，可以使用相邻兄弟选择器

  ```css
  /*选取了所有位于 <div> 元素后的第一个 <p> 元素*/
  div+p
  {
    background-color:yellow;
  }
  ```

- **普通兄弟选择器**（以破折号`~`分隔）

  后续兄弟选择器选取所有指定元素之后的相邻兄弟元素。

  ```css
  /*选取了所有 <div> 元素之后的所有相邻兄弟元素 <p> */
  div~p
  {
    background-color:yellow;
  }
  ```



#### CSS 伪类(Pseudo-classes)

添加到选择器的关键字，<mark>指定要选择的元素的特殊状态</mark>。

例如，`:hover `可被用于在用户将鼠标悬停在按钮上时改变按钮的颜色。

```css
/* 所有用户指针悬停的按钮 */
button:hover {
  color: blue;
}
```

伪类连同伪元素一起，他们允许你不仅仅是根据文档 DOM 树中的内容对元素应用样式，而且还允许你根据诸如像导航历史这样的外部因素来应用样式（例如 [`:visited`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:visited)），同样的，可以根据内容的状态（例如在一些表单元素上的 [`:checked`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:checked)），或者鼠标的位置（例如 [`:hover`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:hover) 让你知道是否鼠标在一个元素上悬浮）来应用样式。

**语法：**

- 伪类的语法：

  ```css
  selector:pseudo-class {
    property: value;
  }
  ```

- CSS类也可以使用伪类：

  ```css
  selector.class:pseudo-class {
    property:value;
  }
  ```

摘自：[MDN - 伪类](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-classes)



#### 伪元素

伪元素是一个附加至选择器末的关键词，允许你对被选择元素的特定部分修改样式。

下例 [`::first-line`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::first-line) 伪元素可改变段落首行文字的样式。

```css
/* 每一个 <p> 元素的第一行。 */
p::first-line {
  color: blue;
  text-transform: uppercase;
}
```

**语法**

```css
selector::pseudo-element {
  property: value;
}
```

**所有CSS伪类/元素**

| 选择器                                                       | 示例           | 示例说明                                         |
| :----------------------------------------------------------- | :------------- | :----------------------------------------------- |
| [:link](https://www.runoob.com/cssref/sel-link.html)         | a:link         | 选择所有未访问链接                               |
| [:visited](https://www.runoob.com/cssref/sel-visited.html)   | a:visited      | 选择所有访问过的链接                             |
| [:active](https://www.runoob.com/cssref/sel-active.html)     | a:active       | 选择正在活动链接                                 |
| [:hover](https://www.runoob.com/cssref/sel-hover.html)       | a:hover        | 把鼠标放在链接上的状态                           |
| [:focus](https://www.runoob.com/cssref/sel-focus.html)       | input:focus    | 选择元素输入后具有焦点                           |
| [:first-letter](https://www.runoob.com/cssref/sel-firstletter.html) | p:first-letter | 选择每个\<p> 元素的第一个字母                    |
| [:first-line](https://www.runoob.com/cssref/sel-firstline.html) | p:first-line   | 选择每个\<p> 元素的第一行                        |
| [:first-child](https://www.runoob.com/cssref/sel-firstchild.html) | p:first-child  | 选择器匹配属于任意元素的第一个子元素的 \<p> 元素 |
| [:before](https://www.runoob.com/cssref/sel-before.html)     | p:before       | 在每个\<p>元素之前插入内容                       |
| [:after](https://www.runoob.com/cssref/sel-after.html)       | p:after        | 在每个\<p>元素之后插入内容                       |
| [:lang(*language*)](https://www.runoob.com/cssref/sel-lang.html) | p:lang(it)     | 为\<p>元素的lang属性选择一个开始值               |

以上部分摘自：[MDN - 伪元素](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-elements)



#### CSS 图像透明度

CSS3中属性的透明度是 **opacity**，示例：

```css
opacity:0.4;
```



#### filter

**filter** CSS属性<font color=FF0000>将模糊或颜色偏移等图形效果应用于元素</font>。<font color=FF0000>滤镜通常用于调整图像，背景和边框的渲染。</font>

示例：

```css
/* URL to SVG filter */
filter: url("filters.svg#filter-id");

/* <filter-function> values */
filter: blur(5px);
filter: brightness(0.4);
filter: contrast(200%);
filter: drop-shadow(16px 16px 20px blue);
filter: grayscale(50%);
filter: hue-rotate(90deg);
filter: invert(75%);
filter: opacity(25%);
filter: saturate(30%);
filter: sepia(60%);
```

摘自：[MDN - filter](https://developer.mozilla.org/zh-CN/docs/Web/CSS/filter)

##### blur()：是filter的一种

blur() CSS 方法将高斯模糊应用于输出图片. 结果为 \<filter-function>。

**语法：**

```css
blur(radius)
```

**值：**

**radius：**<font color=FF0000>模糊的半径，值为\<length></font>。 它定义了高斯函数的标准偏差值，即屏幕上有多少像素相互融合; 因此，较大的值会产生更多模糊。 值为0会使输入保持不变。 该值为空则为0。

摘自：[MDN - blur()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/filter-function/blur())







#### CSS 媒体类型

**媒介类型(Media Types)允许你定义以何种媒介来提交文档。文档可以被显示在显示器、纸媒介或者听觉浏览器等等。**

**@media规则**
@media 规则使你有能力在相同的样式表中，使用不同的样式规则来针对不同的媒介。

| 媒体类型   | 描述                                                   |
| :--------- | :----------------------------------------------------- |
| all        | 用于所有的媒体设备。                                   |
| aural      | 用于语音和音频合成器。                                 |
| braille    | 用于盲人用点字法触觉回馈设备。                         |
| embossed   | 用于分页的盲人用点字法打印机。                         |
| handheld   | 用于小的手持的设备。                                   |
| print      | 用于打印机。                                           |
| projection | 用于方案展示，比如幻灯片。                             |
| screen     | 用于电脑显示器。                                       |
| tty        | 用于使用固定密度字母栅格的媒体，比如电传打字机和终端。 |
| tv         | 用于电视机类型的设备。                                 |



#### 属性选择器

CSS **属性选择器**<mark>通过已经存在的属性名或属性值匹配元素</mark>。

示例：

```css
/* 存在title属性的<a> 元素 */
a[title] {
  color: purple;
}

/* 存在href属性并且属性值匹配"https://example.org"的<a> 元素 */
a[href="https://example.org"] {
  color: green;
}

/* 存在href属性并且属性值包含"example"的<a> 元素 */
a[href*="example"] {
  font-size: 2em;
}

/* 存在href属性并且属性值结尾是".org"的<a> 元素 */
a[href$=".org"] {
  font-style: italic;
}

/* 存在class属性并且属性值包含"logo"的<a>元素 */
a[class~="logo"] {
  padding:
}
```



#### CSS计数器

本质上CSS计数器是<font color=FF0000>由CSS维护的变量</font>，<mark>这些变量可能根据CSS规则增加以跟踪使用次数</mark>。这允许你根据文档位置来调整内容表现。 CSS计数器是CSS2.1中自动计数编号部分的实现。

<mark>计数器的值通过使用<font color=FF0000>counter-reset</font> （计数器重置为0）和 <font color=FF0000>counter-increment</font> （增加计数器值）操作，在 <font color=FF0000>content 上</font>应用 <font color=FF0000>counter() 或 counters()</font>函数来<font color=FF0000>显示在页面上</font></mark>。

**使用计数器，示例：**

<font color=FF0000>使用CSS计数器之前，必须重置一个值，默认是0</font>。使用counter()函数来给元素增加计数器。 下面的CSS给每个h3元素的前面增加了 "Section <计算器值>:"。

```html
<head>
	<style>
		body {
		  counter-reset: section;           /* 重置计数器成0 */
		}
		h3:before {
		  counter-increment: section;      /* 增加计数器值 */
		  content: "Section " counter(section) ": "; /* 显示计数器 */
		}
	</style>
</head>

<body>
	<h3>Introduction</h3>
	<h3>Body</h3>
	<h3>Conclusion</h3>
</body>
```

**效果：**

<img src="https://i.loli.net/2020/08/26/VZXTbwyaWf98o4U.png" style="zoom: 45%;" />

**计数器嵌套（比如：1.1, 1.2, ...），示例：**

CSS计数器对创建有序列表特别有用，因为在子元素中会自动创建一个CSS计数器的实例。使用 counters() 函数，在不同级别的嵌套计数器之间可以插入字符串。

```html
<head>
  <style>
    ol {
      counter-reset: section;                /* 为每个ol元素创建新的计数器实例 */
  		list-style-type: none;
    }
		li:before {
  		counter-increment: section;            /* 只增加计数器的当前实例 */
  		content: counters(section, ".") " ";   /* 为所有计数器实例增加以“.”分隔的值 */
		}
  </style>
</head>

<body>
<ol>
    <li>item</li>          <!-- 1     -->
    <li>item               <!-- 2     -->
        <ol>
            <li>item</li>      <!-- 2.1   -->
            <li>item</li>      <!-- 2.2   -->
            <li>item           <!-- 2.3   -->
                <ol>
                    <li>item</li>  <!-- 2.3.1 -->
                    <li>item</li>  <!-- 2.3.2 -->
                </ol>
                <ol>
                    <li>item</li>  <!-- 2.3.1 -->
                    <li>item</li>  <!-- 2.3.2 -->
                    <li>item</li>  <!-- 2.3.3 -->
                </ol>
            </li>
            <li>item</li>      <!-- 2.4   -->
        </ol>
    </li>
    <li>item</li>          <!-- 3     -->
    <li>item</li>          <!-- 4     -->
</ol>
<ol>
    <li>item</li>          <!-- 1     -->
    <li>item</li>          <!-- 2     -->
</ol>
</body>
```

**效果：**

<img src="https://i.loli.net/2020/08/26/IyzNwb8Plva7gAD.png" style="zoom:50%;" />

摘自：[MDN - 使用CSS计数器](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Counters)



#### CSS 实例链接

这里有非常多的情况及对应实例：https://www.runoob.com/css/css-examples.html



#### **CSS3 模块**

- 选择器
- 盒模型
- 背景和边框
- 文字特效
- 2D/3D转换
- 动画
- 多列布局
- 用户界面



#### CSS边框

- **border-radius**：圆角

  示例：

  ```css
  border-radius:25px;
  ```

- **box-shadow**：盒阴影

  可以在同一个元素上设置多个阴影效果，并用逗号将他们分隔开。该属性<mark>可设置的值包括阴影的X轴偏移量、Y轴偏移量、模糊半径、扩散半径和颜色</mark>。

  **取值**

  - `inset`

    如果没有指定`inset`，默认阴影在边框外，即阴影向外扩散。 使用 `inset` 关键字会使得阴影落在盒子内部，这样看起来就像是内容被压低了。 此时阴影会在边框之内 (即使是透明边框）、背景之上、内容之下。

  - `<offset-x>` `<offset-y>`

    这是头两个 [`<length>`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/length) 值，用来设置阴影偏移量。x,y 是按照数学二维坐标系来计算的，只不过y垂直方向向下。 `<offset-x>` 设置水平偏移量，正值阴影则位于元素右边，负值阴影则位于元素左边。 `<offset-y>` 设置垂直偏移量，正值阴影则位于元素下方，负值阴影则位于元素上方。可用单位请查看 [`<length>`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/length) 。

    如果两者都是0，那么阴影位于元素后面。这时如果设置了`<blur-radius>` 或`<spread-radius>` 则有模糊效果。需要考虑 `inset` 

  - `<blur-radius>`

    这是第三个 [`<length>`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/length) 值。值越大，模糊面积越大，阴影就越大越淡。 不能为负值。默认为0，此时阴影边缘锐利。本规范不包括如何计算模糊半径的精确算法，但是，它详细说明如下：

  > 对于长而直的阴影边缘，它会创建一个过渡颜色用于模糊 以阴影边缘为中心、模糊半径为半径的局域，过渡颜色的范围在完整的阴影颜色到它最外面的终点的透明之间。 （译者注：对此有兴趣的可以了解下数字图像处理的模糊算法。）

  - `<spread-radius>`

    这是第四个 [`<length>`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/length) 值。取正值时，阴影扩大；取负值时，阴影收缩。默认为0，此时阴影与元素同样大。需要考虑 `inset` 

  - `<color>`

    相关事项查看 [`<length>`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/color_value) 。如果没有指定，则由浏览器决定——通常是[`color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/color)的值，不过目前Safari取透明。

  相关摘自：[MDN - box-shadow](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-shadow)

- **border-image**：边界图片



#### CSS3 背景

**背景属性：**

- background-image
- background-size
- background-origin
- background-clip



#### CSS3 渐变

CSS3 渐变（gradients）可以让你在两个或多个指定的颜色之间显示平稳的过渡。

CSS3 定义了两种类型的渐变（gradients）：

- **线性渐变（Linear Gradients）**- 向下/向上/向左/向右/对角方向

  - **语法**

    ```css
    background-image: linear-gradient(direction, color-stop1, color-stop2, ...);
    ```

  - 可以使用<mark>预定义方向</mark>（to bottom、to top、to right、to left、to bottom right，等等）

    **示例：**

    ```css
    /*演示了从左上角开始（到右下角）的线性渐变。起点是红色，慢慢过渡到黄色：*/
    #grad {
      height: 200px;
      background-image: linear-gradient(to bottom right, red, yellow);
    }
    ```

  - **使用角度**

    如果你想要在渐变的方向上做更多的控制，你可以定义一个角度，而不用预定义方向（to bottom、to top、to right、to left、to bottom right，等等）。

    **语法**

    ```css
    background-image: linear-gradient(angle, color-stop1, color-stop2);
    ```

    角度是指水平线和渐变线之间的角度，逆时针方向计算。换句话说，<mark>0deg</mark> 将创建一个<mark>从下到上的渐变</mark>，<mark>90deg</mark> 将创建一个<mark>从左到右的渐变</mark>。

    <img src="https://i.loli.net/2020/08/25/GB2mqyRKZiTP8zS.jpg" style="zoom:50%;" />

  - **使用多个颜色节点**，示例：

    ```css
    /*带有多个颜色节点的从上到下的线性渐变*/
    #grad {
      background-image: linear-gradient(red, yellow, green);
    }
    ```

  - **使用透明度**（transparent）

    rgba() 函数中的最后一个参数可以是从 0 到 1 的值，它定义了颜色的透明度：0 表示完全透明，1 表示完全不透明。

    ```css
    #grad {
      background-image: linear-gradient(to right, rgba(255,0,0,0), rgba(255,0,0,1));
    }
    ```

- **径向渐变（Radial Gradients）**- 由它们的中心定义

  径向渐变由它的中心定义。

  为了创建一个径向渐变，你也必须至少定义两种颜色节点。颜色节点即你想要呈现平稳过渡的颜色。同时，你也可以指定渐变的中心、形状（圆形或椭圆形）、大小。默认情况下，渐变的中心是 center（表示在中心点），渐变的形状是 ellipse（表示椭圆形），渐变的大小是 farthest-corner（表示到最远的角落）。



#### CSS3 文本属性

| 属性                                                         | 描述                                                    | CSS  |
| :----------------------------------------------------------- | :------------------------------------------------------ | :--: |
| [hanging-punctuation](https://www.runoob.com/cssref/css3-pr-hanging-punctuation.html) | 规定标点字符是否位于线框之外。                          |  3   |
| [punctuation-trim](https://www.runoob.com/cssref/css3-pr-punctuation-trim.html) | 规定是否对标点字符进行修剪。                            |  3   |
| [text-align-last](https://www.runoob.com/cssref/css3-pr-text-align-last.html) | 设置如何对齐最后一行或紧挨着强制换行符之前的行。        |  3   |
| [text-emphasis](https://www.runoob.com/css3/css3-pr-text-emphasis.html) | 向元素的文本应用重点标记以及重点标记的前景色。          |  3   |
| [text-justify](https://www.runoob.com/cssref/css3-pr-text-justify.html) | 规定当 text-align 设置为 "justify" 时所使用的对齐方法。 |  3   |
| [text-outline](https://www.runoob.com/cssref/css3-pr-text-outline.html) | 规定文本的轮廓。                                        |  3   |
| [text-overflow](https://www.runoob.com/cssref/css3-pr-text-overflow.html) | 规定当文本溢出包含元素时发生的事情。                    |  3   |
| [text-shadow](https://www.runoob.com/cssref/css3-pr-text-shadow.html) | 向文本添加阴影。                                        |  3   |
| [text-wrap](https://www.runoob.com/cssref/css3-pr-text-wrap.html) | 规定文本的换行规则。                                    |  3   |
| [word-break](https://www.runoob.com/cssref/css3-pr-word-break.html) | 规定非中日韩文本的换行规则。                            |  3   |
| [word-wrap](https://www.runoob.com/cssref/css3-pr-word-wrap.html) | 允许对长的不可分割的单词进行分割并换行到下一行。        |  3   |

 

#### CSS3 字体

您所选择的字体在新的 **CSS3** 版本有关于 **@font-face** 规则描述。您"自己的"的字体是在 **CSS3 @font-face** 规则中定义的。

**示例：**

```css
@font-face
{
    font-family: myFirstFont;
    src: url(sansation_light.woff);
}
```

所有的字体描述和里面的@font-face规则定义：

|    描述符     | 值                                                           |                             描述                             |
| :-----------: | :----------------------------------------------------------- | :----------------------------------------------------------: |
|  font-family  | *name*                                                       |                    必需。规定字体的名称。                    |
|      src      | *URL*                                                        |                  必需。定义字体文件的 URL。                  |
| font-stretch  | normal<br/>condensed<br/>ultra-condensed<br/>extra-condensed<br/>semi-condensed<br/>expanded<br/>semi-expanded<br/>extra-expanded<br/>ultra-expanded |          可选。定义如何拉伸字体。默认是 "normal"。           |
|  font-style   | normalitalicoblique                                          |           可选。定义字体的样式。默认是 "normal"。            |
|  font-weight  | normal<br/>bold<br/>100<br/>200<br/>300<br/>400<br/>500<br/>600<br/>700<br/>800<br/>900 |           可选。定义字体的粗细。默认是 "normal"。            |
| unicode-range | *unicode-range*                                              | 可选。定义字体支持的 UNICODE 字符范围。默认是 "U+0-10FFFF"。 |



#### CSS3 2D 转换

**浏览器支持**

表格中的数字表示支持该属性的第一个浏览器版本号。

紧跟在 -webkit-, -ms- 或 -moz- 前的数字为支持该前缀属性的第一个浏览器版本号。

| 属性                                | Chrome            | Edge          | Firefox        | Safari       | Opera                            |
| :---------------------------------- | ----------------- | ------------- | -------------- | ------------ | -------------------------------- |
| transform                           | 36.0 4.0 -webkit- | 10.0 9.0 -ms- | 16.0 3.5 -moz- | 3.2 -webkit- | 23.0 15.0 -webkit- 12.1 10.5 -o- |
| transform-origin (two-value syntax) | 36.0 4.0 -webkit- | 10.0 9.0 -ms- | 16.0 3.5 -moz- | 3.2 -webkit- | 23.0 15.0 -webkit- 12.1 10.5 -o- |

Internet Explorer 10, Firefox, 和 Opera支持transform 属性.

Chrome 和 Safari 要求前缀 -webkit- 版本.

**注意：** Internet Explorer 9 要求前缀 -ms- 版本.



**2D变换方法：**

- **translate()**：根据左(X轴)和顶部(Y轴)位置给定的参数，从当前元素<font color=FF0000>位置移动</font>。**示例：**

  ```css
  div
  {
  	transform: translate(50px,100px);
  	-ms-transform: translate(50px,100px); /* IE 9 */
  	-webkit-transform: translate(50px,100px); /* Safari and Chrome */
  }
  ```

- **rotate()**：在一个给定度数<font color=FF0000>顺时针旋转</font>的元素。<mark>负值是允许的，这样是元素逆时针旋转</mark>。示例：

  ```css
  div
  {
  	transform: rotate(30deg);
  	-ms-transform: rotate(30deg); /* IE 9 */
  	-webkit-transform: rotate(30deg); /* Safari and Chrome */
  }
  ```

- **scale()**：<font color=FF0000>元素增加或减少的大小</font>，取决于宽度（X轴）和高度（Y轴）的参数。示例：

  ```css
  /*scale(2,3)转变宽度为原来的大小的2倍，和其原始大小3倍的高度。*/
  div
  {
    -ms-transform:scale(2,3); /* IE 9 */
  	-webkit-transform: scale(2,3); /* Safari */
  	transform: scale(2,3); /* 标准语法 */
  }
  ```

- **skew()**

  定义了一个元素在二维平面上的倾斜转换。

  **语法**

  ```css
  transform:skew(<angle> [,<angle>]);
  ```

  包含两个参数值，分别表示X轴和Y轴倾斜的角度，如果第二个参数为空，则默认为0，参数为负表示向相反方向倾斜。

  **另外：**

  - `skewX(<angle>);`  表示只在X轴(水平方向)倾斜。

  - `skewY(<angle>); ` 表示只在Y轴(垂直方向)倾斜。

  **示例：**

  ```css
  div
  {
  	transform: skew(30deg,20deg);
  	-ms-transform: skew(30deg,20deg); /* IE 9 */
  	-webkit-transform: skew(30deg,20deg); /* Safari and Chrome */
  }
  ```

- **matrix()**

  matrix()方法将2D变换方法合并成一个。matrix 方法有六个参数，包含旋转，缩放，移动（平移）和倾斜功能。

  <mark>`matrix(a, b, c, d, tx, ty)` 是 `matrix3d(a, b, 0, 0, c, d, 0, 0, 0, 0, 1, 0, tx, ty, 0, 1)` 的简写。</mark>

**总结：2D 转换方法**

| 函数                                      | 描述                                                        |
| :---------------------------------------- | :---------------------------------------------------------- |
| matrix(*n*, *n*, *n*, *n*, *n*, *n*)      | 定义 2D 转换，使用六个值的矩阵。                            |
| translate(*x*, *y*)                       | 定义 2D 转换，沿着 X 和 Y 轴移动元素。                      |
| <font color=FF0000>translateX</font>(*n*) | 定义 2D 转换，<font color=FF0000>沿着 X 轴移动元素</font>。 |
| <font color=FF0000>translateY</font>(*n*) | 定义 2D 转换，<font color=FF0000>沿着 Y 轴移动元素</font>。 |
| scale(*x*, *y*)                           | 定义 2D 缩放转换，改变元素的宽度和高度。                    |
| scaleX(*n*)                               | 定义 2D 缩放转换，改变元素的宽度。                          |
| scaleY(*n*)                               | 定义 2D 缩放转换，改变元素的高度。                          |
| rotate(*angle*)                           | 定义 2D 旋转，在参数中规定角度。                            |
| skew(*x-angle*, *y-angle*)                | 定义 2D 倾斜转换，沿着 X 和 Y 轴。                          |
| skewX(*angle*)                            | 定义 2D 倾斜转换，沿着 X 轴。                               |
| skewY(*angle*)                            | 定义 2D 倾斜转换，沿着 Y 轴。                               |



#### CSS 3D转换

**浏览器支持**

表格中的数字表示支持该属性的第一个浏览器版本号。

紧跟在 -webkit-, -ms- 或 -moz- 前的数字为支持该前缀属性的第一个浏览器版本号。

| 属性                                  | Chrome             | Edge | Firefox         | Safari       | Opera              |
| :------------------------------------ | ------------------ | ---- | --------------- | ------------ | ------------------ |
| transform                             | 36.0 12.0 -webkit- | 10.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |
| transform-origin (three-value syntax) | 36.0 12.0 -webkit- | 10.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |
| transform-style                       | 36.0 12.0 -webkit- | 11.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |
| perspective                           | 36.0 12.0 -webkit- | 10.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |
| perspective-origin                    | 36.0 12.0 -webkit- | 10.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |
| backface-visibility                   | 36.0 12.0 -webkit- | 10.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |

**转换属性**

| 属性                                                         | 描述                                 | CSS  |
| :----------------------------------------------------------- | :----------------------------------- | :--- |
| [transform](https://www.runoob.com/cssref/css3-pr-transform.html) | 向元素应用 2D 或 3D 转换。           | 3    |
| [transform-origin](https://www.runoob.com/cssref/css3-pr-transform-origin.html) | 允许你改变被转换元素的位置。         | 3    |
| [transform-style](https://www.runoob.com/cssref/css3-pr-transform-style.html) | 规定被嵌套元素如何在 3D 空间中显示。 | 3    |
| [perspective](https://www.runoob.com/cssref/css3-pr-perspective.html) | 规定 3D 元素的透视效果。             | 3    |
| [perspective-origin](https://www.runoob.com/cssref/css3-pr-perspective-origin.html) | 规定 3D 元素的底部位置。             | 3    |
| [backface-visibility](https://www.runoob.com/cssref/css3-pr-backface-visibility.html) | 定义元素在不面对屏幕时是否可见。     | 3    |

**3D 转换方法**

| 函数                                                         | 描述                                      |
| :----------------------------------------------------------- | :---------------------------------------- |
| matrix3d(*n*, *n*, *n*, *n*, *n*, *n*,  *n*, *n*, *n*, *n*, *n*, *n*, *n*, *n*, *n*, *n*) | 定义 3D 转换，使用 16 个值的 4x4 矩阵。   |
| translate3d(*x*, *y* ,*z*)                                   | 定义 3D 转化。                            |
| translateX(*x*)                                              | 定义 3D 转化，仅使用用于 X 轴的值。       |
| translateY(*y*)                                              | 定义 3D 转化，仅使用用于 Y 轴的值。       |
| translateZ(*z*)                                              | 定义 3D 转化，仅使用用于 Z 轴的值。       |
| scale3d(*x*, *y* ,*z*)                                       | 定义 3D 缩放转换。                        |
| scaleX(*x*)                                                  | 定义 3D 缩放转换，通过给定一个 X 轴的值。 |
| scaleY(*y*)                                                  | 定义 3D 缩放转换，通过给定一个 Y 轴的值。 |
| scaleZ(*z*)                                                  | 定义 3D 缩放转换，通过给定一个 Z 轴的值。 |
| rotate3d(*x*, *y*, *z*, *angle*)                             | 定义 3D 旋转。                            |
| rotateX(*angle*)                                             | 定义沿 X 轴的 3D 旋转。                   |
| rotateY(*angle*)                                             | 定义沿 Y 轴的 3D 旋转。                   |
| rotateZ(*angle*)                                             | 定义沿 Z 轴的 3D 旋转。                   |
| perspective(*n*)                                             | 定义 3D 转换元素的透视视图。              |



#### CSS3 过渡（transition）

添加某种效果是元素从一种样式逐渐改变为另一种的效果，无需使用Flash动画或JavaScript。

要实现这一点，必须规定两项内容：

- 指定要添加效果的CSS属性
- 指定效果的持续时间。

**示例**

```css
/*应用于宽度属性的过渡效果，时长为 2 秒：*/
div
{
    transition: width 2s;
    -webkit-transition: width 2s; /* Safari */
}
```

**过渡属性**

| 属性                                                         | 描述                                         | CSS  |
| :----------------------------------------------------------- | :------------------------------------------- | :--- |
| [transition](https://www.runoob.com/cssref/css3-pr-transition.html) | 简写属性，用于在一个属性中设置四个过渡属性。 | 3    |
| [transition-property](https://www.runoob.com/cssref/css3-pr-transition-property.html) | 规定应用过渡的 CSS 属性的名称。              | 3    |
| [transition-duration](https://www.runoob.com/cssref/css3-pr-transition-duration.html) | 定义过渡效果花费的时间。默认是 0。           | 3    |
| [transition-timing-function](https://www.runoob.com/cssref/css3-pr-transition-timing-function.html) | 规定过渡效果的时间曲线。默认是 "ease"。      | 3    |
| [transition-delay](https://www.runoob.com/cssref/css3-pr-transition-delay.html) | 规定过渡效果何时开始。默认是 0。             | 3    |



#### CSS3 动画

CSS3 可以创建动画，它可以取代许多网页动画图像、Flash 动画和 JavaScript 实现的效果。

要创建 CSS3 动画，你需要了解 <font color=FF0000>@keyframes</font> 规则。

- @keyframes 规则是创建动画。

- @keyframes 规则内<mark>指定一个 CSS 样式和动画将逐步从目前的样式更改为新的样式</mark>。

当在 **@keyframes** 创建动画，<mark><font color=FF0000>把它绑定到一个选择器</font>，否则动画不会有任何效果</mark>。指定至少这两个CSS3的动画属性绑定向一个选择器：

- 规定动画的名称
- 规定动画的时长（<mark>必须定义动画的名称和动画的持续时间</mark>。如果省略的持续时间，动画将无法运行，因为默认值是0。）

**CSS3动画是什么？**

动画是使元素从一种样式逐渐变化为另一种样式的效果。您可以改变任意多的样式任意多的次数。

<mark>请用百分比来规定变化发生的时间，或用关键词 "from" 和 "to"，等同于 0% 和 100%</mark>。

0% 是动画的开始，100% 是动画的完成。<mark>为了得到最佳的浏览器支持，您应该始终定义 0% 和 100% 选择器</mark>。

**@keyframes 规则和所有动画属性：**

| 属性                                                         | 描述                                                         | CSS  |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :--- |
| [@keyframes](https://www.runoob.com/cssref/css3-pr-animation-keyframes.html) | 规定动画。                                                   | 3    |
| [animation](https://www.runoob.com/cssref/css3-pr-animation.html) | 所有动画属性的简写属性。                                     | 3    |
| [animation-name](https://www.runoob.com/cssref/css3-pr-animation-name.html) | 规定 @keyframes 动画的名称。                                 | 3    |
| [animation-duration](https://www.runoob.com/cssref/css3-pr-animation-duration.html) | 规定动画完成一个周期所花费的秒或毫秒。默认是 0。             | 3    |
| [animation-timing-function](https://www.runoob.com/cssref/css3-pr-animation-timing-function.html) | 规定动画的速度曲线。默认是 "ease"。                          | 3    |
| [animation-fill-mode](https://www.runoob.com/cssref/css3-pr-animation-fill-mode.html) | 规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式。 | 3    |
| [animation-delay](https://www.runoob.com/cssref/css3-pr-animation-delay.html) | 规定动画何时开始。默认是 0。                                 | 3    |
| [animation-iteration-count](https://www.runoob.com/cssref/css3-pr-animation-iteration-count.html) | 规定动画被播放的次数。默认是 1。                             | 3    |
| [animation-direction](https://www.runoob.com/cssref/css3-pr-animation-direction.html) | 规定动画是否在下一周期逆向地播放。默认是 "normal"。          | 3    |
| [animation-play-state](https://www.runoob.com/cssref/css3-pr-animation-play-state.html) | 规定动画是否正在运行或暂停。默认是 "running"。               | 3    |



#### CSS3 多列

- **column-count**：指定了需要分割的列数。示例：

  ```css
  /*将 <div> 元素中的文本分为 3 列*/
  div {
      -webkit-column-count: 3; /* Chrome, Safari, Opera */
      -moz-column-count: 3; /* Firefox */
      column-count: 3;
  }
  ```

- **column-gap**：指定了列与列间的间隙。示例：

  ```css
  /*指定了列与列间的间隙为 40 像素*/
  div {
      -webkit-column-gap: 40px; /* Chrome, Safari, Opera */
      -moz-column-gap: 40px; /* Firefox */
      column-gap: 40px;
  }
  ```

- **column-rule-style**：指定了列与列间的边框样式。示例：

  ```css
  div {
      -webkit-column-rule-style: solid; /* Chrome, Safari, Opera */
      -moz-column-rule-style: solid; /* Firefox */
      column-rule-style: solid;
  }
  ```

- **column-rule-width**：指定了两列的边框厚度。示例：

  ```css
  div {
      -webkit-column-rule-width: 1px; /* Chrome, Safari, Opera */
      -moz-column-rule-width: 1px; /* Firefox */
      column-rule-width: 1px;
  }
  ```

- **column-rule-color**：指定了两列的边框颜色。示例：

  ```css
  div {
      -webkit-column-rule-color: lightblue; /* Chrome, Safari, Opera */
      -moz-column-rule-color: lightblue; /* Firefox */
      column-rule-color: lightblue;
  }
  ```

- **column-rule**：是 column-rule-* 所有属性的简写。示例：

  ```css
  /*设置了列直接的边框的厚度，样式及颜色*/
  div {
      -webkit-column-rule: 1px solid lightblue; /* Chrome, Safari, Opera */
      -moz-column-rule: 1px solid lightblue; /* Firefox */
      column-rule: 1px solid lightblue;
  }
  ```

- **column-span**：指定元素跨越多少列。示例：

  ```css
  /*指定 <h2> 元素跨越所有列*/
  h2 {
      -webkit-column-span: all; /* Chrome, Safari, Opera */
      column-span: all;
  }
  ```

- **column-width**：指定了列的宽度。示例：

  ```css
  div {
      -webkit-column-width: 100px; /* Chrome, Safari, Opera */
      column-width: 100px;
  }
  ```

 **CSS3 的多列属性：**

| 属性                                                         | 描述                                      |
| :----------------------------------------------------------- | :---------------------------------------- |
| [column-count](https://www.runoob.com/cssref/css3-pr-column-count.html) | 指定元素应该被分割的列数。                |
| [column-fill](https://www.runoob.com/cssref/css3-pr-column-fill.html) | 指定如何填充列                            |
| [column-gap](https://www.runoob.com/cssref/css3-pr-column-gap.html) | 指定列与列之间的间隙                      |
| [column-rule](https://www.runoob.com/cssref/css3-pr-column-rule.html) | 所有 column-rule-* 属性的简写             |
| [column-rule-color](https://www.runoob.com/cssref/css3-pr-column-rule-color.html) | 指定两列间边框的颜色                      |
| [column-rule-style](https://www.runoob.com/cssref/css3-pr-column-rule-style.html) | 指定两列间边框的样式                      |
| [column-rule-width](https://www.runoob.com/cssref/css3-pr-column-rule-width.html) | 指定两列间边框的厚度                      |
| [column-span](https://www.runoob.com/cssref/css3-pr-column-span.html) | 指定元素要跨越多少列                      |
| [column-width](https://www.runoob.com/cssref/css3-pr-column-width.html) | 指定列的宽度                              |
| [columns](https://www.runoob.com/cssref/css3-pr-columns.html) | column-width 与 column-count 的简写属性。 |



#### CSS3 用户界面

- **resize**：指定一个元素是否应该由用户去调整大小（比如用户改变窗口的大小）。示例：

  ```css
  div
  {
      resize:both;
      overflow:auto;
  }
  ```

  **取值**

  - **none** 元素不能被用户缩放。
  - **both** 允许用户在水平和垂直方向上调整元素的大小。
  - **horizontal** 允许用户在水平方向上调整元素的大小。
  - **vertical** 允许用户在垂直方向上调整元素的大小。

  - **block**
  - **inline**

- **box-sizing**：以确切的方式定义适应某个区域的具体内容。示例：

  ```css
  div
  {
      box-sizing:border-box;
      -moz-box-sizing:border-box; /* Firefox */
      width:50%;
      float:left;
  }
  ```

  这里没有看懂，建议看[MDN - box-sizing](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-sizing)

- **outline-offset**

  对轮廓进行偏移，并在超出边框边缘的位置绘制轮廓。

  轮廓与边框有两点不同：

  - 轮廓不占用空间
  - 轮廓可能是非矩形

  示例：

  ```css
  /*规定边框边缘之外 15 像素处的轮廓*/
  div
  {
      border:2px solid black;
      outline:2px solid red;
      outline-offset:15px;
  }
  ```

**新的用户界面特性**

| 属性                                                         | 说明                                           | CSS  |
| :----------------------------------------------------------- | :--------------------------------------------- | :--- |
| [appearance](https://www.runoob.com/cssref/css3-pr-appearance.html) | 允许您使一个元素的外观像一个标准的用户界面元素 | 3    |
| [box-sizing](https://www.runoob.com/cssref/css3-pr-box-sizing.html) | 允许你以适应区域而用某种方式定义某些元素       | 3    |
| [icon](https://www.runoob.com/cssref/css3-pr-icon.html)      | 为创作者提供了将元素设置为图标等价物的能力。   | 3    |
| [nav-down](https://www.runoob.com/cssref/css3-pr-nav-down.html) | 指定在何处使用箭头向下导航键时进行导航         | 3    |
| [nav-index](https://www.runoob.com/cssref/css3-pr-nav-index.html) | 指定一个元素的Tab的顺序                        | 3    |
| [nav-left](https://www.runoob.com/cssref/css3-pr-nav-left.html) | 指定在何处使用左侧的箭头导航键进行导航         | 3    |
| [nav-right](https://www.runoob.com/cssref/css3-pr-nav-right.html) | 指定在何处使用右侧的箭头导航键进行导航         | 3    |
| [nav-up](https://www.runoob.com/cssref/css3-pr-nav-up.html)  | 指定在何处使用箭头向上导航键时进行导航         | 3    |
| [outline-offset](https://www.runoob.com/cssref/css3-pr-outline-offset.html) | 外轮廓修饰并绘制超出边框的边缘                 | 3    |
| [resize](https://www.runoob.com/cssref/css3-pr-resize.html)  | 指定一个元素是否是由用户调整大小               | 3    |



#### CSS3 图片

定位图片上的文本

- **.topleft**：左上角
- **.topright**：右上角
- **.center**：居中
- **.bottomleft**：左下角
- **.bottomright**：右下角



#### CSS 框大小

- **不使用 CSS3 box-sizing 属性**
  默认情况下，元素的宽度与高度计算方式如下：

  - width(宽) + padding(内边距) + border(边框) = 元素实际宽度

  - height(高) + padding(内边距) + border(边框) = 元素实际高度

  这就意味着我们在设置元素的 width/height 时，元素真实展示的高度与宽度会更大(因为元素的边框与内边距也会计算在 width/height 中)。

- <font color=FF0000>**使用 CSS3 box-sizing 属性**</font>

  CSS3 `box-sizing` 属性在一个元素的 width 和 height 中包含 padding(内边距) 和 border(边框)。

  <mark>如果在元素上设置了 `box-sizing: border-box;` 则 padding(内边距) 和 border(边框) 也包含在 width 和 height 中</mark>



#### @import

**@import**是CSS@规则，<font color=FF0000>用于从其他样式表导入样式规则</font>。这些规则必须先于所有其他类型的规则，@charset 规则除外： 因为它不是一个嵌套语句，@import不能在条件组的规则中使用。

因此，<mark>用户代理可以避免为不支持的媒体类型检索资源，作者可以指定依赖媒体的@import规则</mark>。这些条件导入在URI之后指定逗号分隔的媒体查询。在没有任何媒体查询的情况下，导入是无条件的。指定所有的媒体具有相同的效果。

**语法**

```css
@import url;
@import url list-of-media-queries;
```

- **url：**<font color=FF0000>是一个表示要引入资源位置的 \<string> 或者 \<uri></font> 。 <mark>这个 URL 可以是绝对路径或者相对路径</mark>。 要注意的是这个 URL 不需要指明一个文件； 可以只指明包名，然后合适的文件会被自动选择 (e.g. chrome://communicator/skin/)。
- **list-of-media-queries：**<font color=FF0000>是一个**逗号分隔**的 媒体查询 条件列表，决定通过URL引入的 CSS 规则 在什么条件下应用</font>。如果浏览器不支持列表中的任何一条媒体查询条件，就不会引入URL指明的CSS文件。

摘自：[MDN - @import](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@import)



#### !important

`!important`主要用于<font color=FF0000>**提升**指定样式规则的**优先级**</font>。**示例：**

```css
div{
		color:red!important;
		color:blue;
}
```

因为后面的blue会覆盖前面的red，结果应该显示蓝色字，但是<font color=FF0000>因为我们在red上加了**!important**，提升了red的优先级</font>，所以结果显示为红色字。

同时：如果同一个样式，<font color=FF0000>**不是**定义在同一个大括号内，!important**也是可以正常发挥作用**的</font>。**示例如下：**

```css
div{
		color:red!mportant;
}
div{
		color:blue;
}
```

<font color=FF0000>在所有的浏览器中都会显示**红色字**。</font>

摘自：[css !important的使用](https://www.jianshu.com/p/3d1b4a58c3ef)



### CSS3 盒子模型

<mark>弹性盒子</mark>是 CSS3 的<mark>一种新的布局模式</mark>。

CSS3 弹性盒（ Flexible Box 或 flexbox），是一种当页面需要<font color=FF0000>适应不同的屏幕大小以及设备类型时确保元素拥有恰当的行为的布局方式</font>。

引入弹性盒布局模型的目的是提供一种更加有效的方式来<mark>对一个容器中的子元素进行排列、对齐和分配空白空间</mark>。

<font color=FF0000>**弹性盒子**</font>由**<font color=FF0000>弹性容器(Flex container)</font>**和**<font color=FF0000>弹性子元素(Flex item)</font>**组成。

弹性容器<font color=FF0000>通过设置 display 属性的值</font>为 <font color=FF0000>**flex**</font> 或 **<font color=FF0000>inline-flex</font>**将其定义为弹性容器。

弹性容器内包含了<font color=FF0000>一个</font>或<font color=FF0000>多个</font>弹性子元素。弹性子元素<font color=FF0000>通常在弹性盒子内一行显示</font>。<font color=FF0000>**默认情况每个容器只有一行**</font>。

<font color=FF0000 size=4>**注意，设为 Flex 布局以后，子元素的float、clear和vertical-align属性将失效。**</font>

#### Flex容器（父容器）

- **flex-direction**

  flex-direction 属性指定了弹性子元素在父容器中的位置

  **语法**

  ```css
  flex-direction: row | row-reverse | column | column-reverse
  ```

  - **row**：横向从左到右排列（左对齐），默认的排列方式。
  - **row-reverse**：反转横向排列（右对齐，从后往前排，最后一项排在最前面）。
  - **column**：纵向排列。
  - **column-reverse**：反转纵向排列，从后往前排，最后一项排在最上面。

- **justify-content 属性**

  内容对齐（justify-content）属性应用在弹性容器上，把弹性项沿着弹性容器的主轴线（main axis）对齐。

  **语法**

  ```css
  justify-content: flex-start | flex-end | center | space-between | space-around
  ```

  - **flex-start**：弹性项目<font color=FF0000>向**行头**紧挨着填充</font>。这个<font color=FF0000>是默认值</font>。<font color=FF0000>第一个弹性项的main-start外边距边线被放置在该行的**main-start边线**</font>，而后续弹性项依次平齐摆放
  - **flex-end**：弹性项目<font color=FF0000>向**行尾**紧挨着填充</font>。<font color=FF0000>第一个弹性项的main-end外边距边线被放置在该行的**main-end边线**</font>，而后续弹性项依次平齐摆放。
  - **center**：弹性项目居中紧挨着填充。（<mark>如果剩余的自由空间是负的，则弹性项目将在两个方向上同时溢出</mark>）。
  - **space-between**：弹性项目<mark>平均分布在该行</mark>上。<mark>如果剩余<font color=FF0000>空间为负</font>或者<font color=FF0000>只有一个弹性项</font>，则该值<font color=FF0000>等同于flex-start</font></mark>。否则，第1个弹性项的外边距和行的main-start边线对齐，而最后1个弹性项的外边距和行的main-end边线对齐，然后剩余的弹性项分布在该行上，相邻项目的间隔相等。
  - **space-around**：弹性项目<mark>平均分布在该行</mark>上，<font color=FF0000>两边留有一半的间隔空间</font>。<mark>如果剩余空间为负或者只有一个弹性项，则该值等同于center</mark>。否则，弹性项目沿该行分布，且<font color=FF0000>彼此间隔相等（比如是20px）</font>，同时<font color=FF0000>首尾两边和弹性容器之间留有一半的间隔（1/2*20px=10px）</font>。

- **align-items 属性**

  align-items<mark>设置或检索弹性盒子元素在<font color=FF0000>侧轴（纵轴）方向上的对齐方式</font></mark>。

  **语法**

  ```css
  align-items: flex-start | flex-end | center | baseline | stretch
  ```

  - **flex-start**：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴<font color=FF0000>起始</font>边界。
  - **flex-end**：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴<font color=FF0000>结束</font>边界。
  - **center**：弹性盒子元素在该行的侧轴（纵轴）上居中放置。（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度）。
  - **baseline**：如弹性盒子元素的行内轴与侧轴为同一条，则该值与'flex-start'等效。其它情况下，该值<font color=FF0000>将与基线对齐</font>。
  - **stretch**：<font color=FF0000>默认值</font>，如果指定侧轴大小的属性值为'auto'，则其值会使项目的边距盒的尺寸尽可能接近所在行的尺寸，但同时会遵照'min/max-width/height'属性的限制。

- **flex-wrap 属性**

  **flex-wrap** 属性用于指定弹性盒子的子元素换行方式。

  **语法**

  ```css
  flex-wrap: nowrap|wrap|wrap-reverse|initial|inherit;
  ```

  - **nowrap**： <font color=FF0000>默认</font>， 弹性容器为<font color=FF0000>单行</font>。该情况下<mark>弹性子项**可能会**溢出容器</mark>。
  - **wrap**： 弹性容器为<font color=FF0000>多行</font>。该情况下<mark>弹性子项溢出的部分会被放置到新行</mark>，子项内部会发生断行
  - **wrap-reverse**：反转 wrap 排列。

- **align-content 属性**

  align-content 属性用于<font color=FF0000>修改 flex-wrap属性的行为</font>。类似于align-items, 但它不是设置弹性子元素的对齐，而是<font color=FF0000>**设置各个行的对齐**</font>。

  **语法**

  ```css
  align-content: flex-start | flex-end | center | space-between | space-around | stretch
  ```

  - **stretch** - <font color=FF0000>默认</font>。各行将会<font color=FF0000>伸展以占用剩余的空间</font>。
  - **flex-start** - 各行向弹性盒容器的<font color=FF0000>起始位置堆叠</font>。
  - **flex-end** - 各行向弹性盒容器的<font color=FF0000>结束位置堆叠</font>。
  - **center** -各行向弹性盒容器的<font color=FF0000>中间位置堆叠</font>。
  - **space-between** -各行在弹性盒容器中<font color=FF0000>平均分布</font>。
  - **space-around** - 各行在弹性盒容器中<font color=FF0000>平均分布</font>，<font color=FF0000>两端保留子元素与子元素之间间距大小的一半</font>。

#### Flex项目（子项目）

以下6个属性设置在项目上

- **order**：定义项目的<font color=FF0000>排列顺序</font>。数值越小，排列越靠前，默认为0，可以为负值。

  ```css
  .item {
  	order: <integer>;
  }
  ```

- **flex-grow**定义项目的<font color=FF0000>**放大比例**</font>，<font color=FF0000>**默认为0**</font>，即如果存在剩余空间，也不放大。

  <font color=FF0000>如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍</font>。

  ```css
  .item {
    flex-grow: <number>; /* default 0 */
  }
  ```

- **flex-shrink**：定义了项目的<font color=FF0000>**缩小比例**</font>，<font color=FF0000>**默认为1**</font>，即如果空间不足，该项目将缩小。

  <font color=FF0000>如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小</font>。

  ```css
  .item {
    flex-shrink: <number>; /* default 1 */
  }
  ```

- **flex-basis**：定义了在<font color=FF0000>**flex-basis给上面两个属性分配多余空间之前, 计算项目是否有多余空间, 默认值为 auto, 即项目本身的大小**</font>。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

  ```css
  .item {
    flex-basis: <length> | auto; /* default auto */
  }
  ```

- **flex**：flex属性是flex-grow, flex-shrink 和 flex-basis的简写，<font color=FF0000>默认值为0 1 auto。后两个属性可选</font>。

  **语法**

  ```css
  .item {
  	1	qI'm 	1qI'm 	q	qq	q221qq1 | initial | none | inherit |  [ flex-grow ] || [ flex-shrink ] || [ flex-basis ]
  }
  ```

  - auto: 计算值为 1 1 auto
  - initial: 计算值为 0 1 auto
  - none：计算值为 0 0 auto
  - inherit：从父元素继承

- **align-self**：<font color=FF0000>允许单个项目有与其他项目不一样的对齐方式</font>，设置弹性元素自身在侧轴（纵轴）方向上的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

  **语法**

  ```css
  .item {
  	align-self: auto | flex-start | flex-end | center | baseline | stretch
  }
  ```

  - **auto**：<font color=FF0000>默认值</font>，如果'align-self'的值为'auto'，则其计算值为元素的父元素的'align-items'值，<font color=FF0000>如果其没有父元素，则计算值为'stretch'</font>。

  - **flex-start**：弹性盒子元素的<font color=FF0000>侧轴（纵轴）</font>起始位置的边界紧靠住该行的侧轴<font color=FF0000>起始边界</font>。
  - **flex-end**：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴<font color=FF0000>结束边界</font>。
  - **center**：弹性盒子元素在该行的侧轴（纵轴）上<font color=FF0000>居中放置</font>。（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度）。
  - **baseline**：如弹性盒子元素的行内轴<font color=FF0000>与侧轴为同一条</font>，则该值与'flex-start'等效。其它情况下，该值将与基线对齐。
  - **stretch**：如果指定侧轴大小的属性值为'auto'，则其值会使项目的边距盒的尺寸尽可能接近所在行的尺寸，但同时会遵照'min/max-width/height'属性的限制。

以上摘自：[RUNOOB - CSS3 弹性盒子(Flex Box)](https://www.runoob.com/css3/css3-flexbox.html)  [阮一峰的网络日志  - Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)  

具体实例：[阮一峰的网络日志  - Flex 布局教程：实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)



**补充：**

- flex: 1; === flex: 1 1 (0 / 任意数字+任意长度单位); <font color=FF0000>（就是代表均匀分配元素）</font>
- flex: initial; === flex: 0 1 auto;
- flex: none; === flex: 0 0 auto;

​			摘自：[flex:1 到底代表什么?](https://zhuanlan.zhihu.com/p/136223806)

-  place-content 是 align-content 和 justify-content 的简写属性；而 place-items 是 align-items 和 justify-items 的简写属性。即：

  ```css
  .flex__container {
      place-content: center;
      place-items: center;
  }
  /*等效于：*/
  .flex__container {
      align-content: center;
      justify-content: center;
  
      align-items: center;
      justify-items: center;
  }
  ```

  摘自：[收藏！40 个 CSS 布局技巧](https://zhuanlan.zhihu.com/p/161822219)

- flex和inline-flex的区别

  - flex： 将对象作为<font color=FF0000>弹性伸缩盒</font>显示
  - inline-flex：将对象作为<font color=FF0000>**内联**块级弹性伸缩盒</font>显示

  摘自：[display：flex和display: inline-flex区别](https://www.jianshu.com/p/4d596708f61b)

![](https://i.loli.net/2020/08/25/A3Qhi7IHJotxzMr.png)



#### CSS3 多媒体查询

<mark>使用 @media 查询，你<font color=FF0000>可以针对不同的媒体类型定义不同的样式</font></mark>。

CSS3 的多媒体查询继承了 CSS2 多媒体类型的所有思想： 取代了查找设备的类型，CSS3 根据设置自适应显示。

媒体查询可用于检测很多事情，例如：

- viewport(视窗) 的宽度与高度
- 设备的宽度与高度
- 朝向 (智能手机横屏，竖屏) 。
- 分辨率

**多媒体查询语法**

多媒体查询由多种媒体组成，可以包含一个或多个表达式，表达式根据条件是否成立返回 true 或 false。

```css
@media not|only mediatype and (expressions) {    
  /*CSS 代码...; */
}
```

如果指定的多媒体类型匹配设备类型则查询结果返回 true，文档会在匹配的设备上显示指定样式效果。

除非你使用了 not 或 only 操作符，否则所有的样式会适应在所有设备上显示效果。

- **not:** not是用来<font color=FF0000>排除掉某些特定的设备</font>的，比如 @media not print（非打印设备）。

  **补充**（摘自：[MDN - 使用媒体查询](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Media_queries)）：用于否定媒体查询，<mark>如果不满足这个条件则返回true，否则返回false</mark>。 如果出现在以逗号分隔的查询列表中，它将仅否定应用了该查询的特定查询。 如果使用not运算符，则还必须指定媒体类型。

- **only:** 用来<font color=FF0000>定某种特别的媒体类型</font>。对于支持Media Queries的移动设备来说，如果存在only关键字，移动设备的Web浏览器会忽略only关键字并直接根据后面的表达式应用样式文件。对于不支持Media Queries的设备但能够读取Media Type类型的Web浏览器，遇到only关键字时会忽略这个样式文件。

- **all:** <font color=FF0000>所有设备</font>

- **and:** and 操作符用于<mark>将多个媒体查询规则组合成单条媒体查询</mark>，<font color=FF0000>当每个查询规则都为真时则该条媒体查询为真</font>，它还用于将媒体功能与媒体类型结合在一起。

- **, (逗号): **逗号<mark>用于将多个媒体查询合并为一个规则</mark>。 逗号分隔列表中的每个查询都与其他查询分开处理。 因此，<mark>如果列表中的任何查询为true，则整个media语句均返回true</mark>。 换句话说，<font color=FF0000>列表的行为类似于逻辑或`or`运算符</font>。

以上部分内容摘自：[MDN - 使用媒体查询](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Media_queries)

**CSS3 多媒体类型**

| 值     | 描述                             |
| :----- | :------------------------------- |
| all    | 用于所有多媒体类型设备           |
| print  | 用于打印机                       |
| screen | 用于电脑屏幕，平板，智能手机等。 |
| speech | 用于屏幕阅读器                   |

更多相关媒体类型 / 媒体功能参见：[Runoob - CSS3 @media 查询](https://www.runoob.com/cssref/css3-pr-mediaquery.html)



#### 响应式 Web 设计 - Viewport

**Viewport** 

Viewport 是<font color=FF0000>用户网页的可视区域</font>，翻译为中文可以叫做"<font color=FF0000>视区</font>"。

<mark>手机浏览器是<font color=FF0000>把页面放在一个虚拟的"窗口"（viewport）中</font>，<font color=FF0000>**通常这个虚拟的"窗口"（viewport）比屏幕宽**</font>，这样就不用把每个网页挤到很小的窗口中</mark>（这样会破坏没有针对手机浏览器优化的网页的布局），用户可以通过平移和缩放来看网页的不同部分。

**设置 Viewport**

一个常用的针对移动网页优化过的页面的 viewport meta 标签大致如下：

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

- **width**：<font color=FF0000>控制 viewport 的大小</font>，可以指定的一个值，如 600，或者特殊的值，如 device-width 为设备的宽度（单位为缩放为 100% 时的 CSS 的像素）。
- **height**：<font color=FF0000>和 width 相对应，指定高度</font>。
- **initial-scale**：<mark>初始缩放比例</mark>，也即是当<mark>页面第一次 load 的时候缩放比例</mark>。
- **maximum-scale**：<mark>允许用户缩放到的最大比例</mark>。
- **minimum-scale**：<mark>允许用户缩放到的最小比例</mark>。
- **user-scalable**：<mark>用户是否可以手动缩放</mark>。



#### 响应式 Web 设计 - 网格视图

**网格视图：**很多网页都是基于网格设计的，这说明<mark>网页是按列来布局的</mark>。

创建一个响应式网格视图：首先<mark>确保所有的 HTML 元素都有 **box-sizing** 属性且设置为 **border-box**</mark>。确保边距和边框包含在元素的宽度和高度间。

添加如下代码：

```css
* {
    box-sizing: border-box;
}
```

12 列的网格系统可以更好的控制响应式网页。

首先我们可以计算每列的百分比: 100% / 12 列 = 8.33%。在每列中指定 class，<font color=FF0000> **class="col-"** 用于定义**每列有几个 span** </font>：

```css
.col-1 {width: 8.33%;}
.col-2 {width: 16.66%;}
.col-3 {width: 25%;}
.col-4 {width: 33.33%;}
.col-5 {width: 41.66%;}
.col-6 {width: 50%;}
.col-7 {width: 58.33%;}
.col-8 {width: 66.66%;}
.col-9 {width: 75%;}
.col-10 {width: 83.33%;}
.col-11 {width: 91.66%;}
.col-12 {width: 100%;}
```



#### 响应式 Web 设计 - 媒体查询

**方向：横屏/竖屏**

结合CSS媒体查询，<mark>可以创建适应不同设备的方向(<font color=FF0000>横屏landscape</font>、<font color=FF0000>竖屏portrait</font>等)的布局</mark>。

**语法：**

```css
orientation：portrait | landscape
```

- **portrait：**指定输出设备中的页面可见区域高度大于或等于宽度
- **landscape：** 除portrait值情况外，都是landscape

**示例：**

```css
@media only screen and (orientation: landscape) {
    body {
        background-color: lightblue;
    }
}
```

**使用指点设备**

作为四级规范的一部分，hover媒体特征被引入了进来。这种特征意味着你<mark>可以测试用户是否能在一个元素上悬浮</mark>，这也<mark>基本就是说他们正在使用某种指点设备，因为触摸屏和键盘导航是没法实现悬浮的</mark>。

示例如下：

```css
@media (hover: hover) {
    body {
        color: rebeccapurple;
    }
}
```

**断点**

引入媒体查询的点就叫做**断点**。

以上部分内容摘自：[MDN - 使用媒体查询](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Media_queries)



#### 响应式 Web 设计 - 图片

**图片**

- **width：**如果 width 属性设置为 100%，图片会根据上下范围实现响应式功能。但是在大屏幕下，图片会比它的原始图片大

- **max-width**：如果 max-width 属性设置为 100%, 图片永远不会大于其原始大小：

**背景图片**

背景图片可以响应调整大小或缩放。以下是三个不同的方法：

-  如果 background-size 属性设置为 <font color=FF0000>"contain"</font>, 背景图片将<font color=FF0000>按比例自适应内容区域</font>。
- 如果 background-size 属性设置为 <font color=FF0000>"100% 100%"</font> ，背景图片将<font color=FF0000>延展覆盖整个区域</font>

- 如果 background-size 属性设置为 <font color=FF0000>"cover"</font>，则会把背景图像扩展至足够大，以使<font color=FF0000>背景图像完全覆盖背景区域</font>。注意该属性保持了图片的比例因此<font color=FF0000>背景图像的某些部分无法显示在背景定位区域中</font>。

**HTML5 \<picture> 元素**

HTML5 的 `<picture>` 元素<font color=FF0000>可以设置多张图片</font>。

`<picture>` 元素类似于 `<video>` 和 `<audio>` 元素。可以设备不同的资源，第一个设置的资源为首选使用的，示例：

```css
<picture>
  <source srcset="img_smallflower.jpg" media="(max-width: 400px)">
  <source srcset="img_flowers.jpg">
  <img src="img_flowers.jpg" alt="Flowers">
</picture>
```

`srcset` 属性的必须的，定义了图片资源。



#### calc()方法

alac() 此 CSS 函数允许在声明 CSS 属性值时执行一些计算。它可以用在如下场合：\<length>、\<frequency>, \<angle>、\<time>、\<percentage>、\<number>、 \<integer>。

**语法：**

```css
property: calc(expression)
```

**示例：**

```css
width: calc(100% - 80px);
```

此 calc()函数用一个表达式作为它的参数，用这个表达式的结果作为值。<font color=FF0000>这个表达式可以是任何如下操作符的组合</font>，采用标准操作符处理法则的简单表达式。

- **+**：加法。
- **-** ：减法。
- *****：乘法，乘数中至少有一个是\<number>
- **/**：除法，除数（/右面的数）必须是\<number>

表达式中的运算对象可以使用任意 \<length> 值。如果你愿意，你<font color=FF0000>可以在一个表达式中混用这类值的不同单位</font>。在需要时，你<font color=FF0000>还可以使用小括号来建立计算顺序</font>。

**备注**

- <font color=FF0000>**+ 和 - 运算符的两边必须要有空白字符**</font>。比如，`calc(50% -8px)` 会被解析成为一个无效的表达式，解析结果是：一个百分比 后跟一个负数长度值。而加有空白字符的、有效的表达式 `calc(8px + -50%)` 会被解析成为：一个长度 后跟一个加号 再跟一个负百分比。
- <mark>***** 和 **/** 这两个运算符前后不需要空白字符，但如果考虑到统一性，仍然推荐加上空白符</mark>。
- 用 0 作除数会使 HTML 解析器抛出异常。
- 涉及自动布局和固定布局的表格中的表列、表列组、表行、表行组和表单元格的宽度和高度百分比的数学表达式，`auto` 可视为已指定。
- `calc()` 函数支持嵌套，但支持的方式是：把被嵌套的 `calc()` 函数全当成普通的括号。（译者注：所以，函数内直接用括号就好了。）

摘自：[MDN - calc()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/calc)



#### 总结 / 文档：CSS 选择器

| 选择器                                                       | 例子                  | 例子描述                                                     | CSS版本 |
| :----------------------------------------------------------- | :-------------------- | :----------------------------------------------------------- | :-----: |
| [.*class*](https://www.w3school.com.cn/cssref/selector_class.asp) | .intro                | 选择 class="intro" 的所有元素。                              |    1    |
| [#*id*](https://www.w3school.com.cn/cssref/selector_id.asp)  | #firstname            | 选择 id="firstname" 的所有元素。                             |    1    |
| [*](https://www.w3school.com.cn/cssref/selector_all.asp)     | *                     | 选择所有元素。                                               |    2    |
| [*element*](https://www.w3school.com.cn/cssref/selector_element.asp) | p                     | 选择所有 \<p> 元素。                                         |    1    |
| [*element*,*element*](https://www.w3school.com.cn/cssref/selector_element_comma.asp) | div,p                 | 选择所有\<div> 元素和所有\<p> 元素。                         |    1    |
| [<font color=FF0000>*element* *element*</font>](https://www.w3school.com.cn/cssref/selector_element_element.asp) | div p                 | <font color=FF0000>选择\<div>元素**内部**的所有\<p>元素。</font> |    1    |
| [*element*>*element*](https://www.w3school.com.cn/cssref/selector_element_gt.asp) | div>p                 | 选择父元素为\<div>元素的所有\<p>元素。                       |    2    |
| [*element*+*element*](https://www.w3school.com.cn/cssref/selector_element_plus.asp) | div+p                 | 选择紧接在\<div>元素之后的所有\<p>元素。                     |    2    |
| [[*attribute*\]](https://www.w3school.com.cn/cssref/selector_attribute.asp) | [target]              | 选择带有 target 属性所有元素。                               |    2    |
| [[*attribute*=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value.asp) | [target=_blank]       | 选择 target="_blank" 的所有元素。                            |    2    |
| [[*attribute*~=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value_contain.asp) | [title~=flower]       | 选择 title 属性包含单词 "flower" 的所有元素。                |    2    |
| [[*attribute*\|=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value_start.asp) | [lang\|=en]           | 选择 lang 属性值以 "en" 开头的所有元素。                     |    2    |
| [:link](https://www.w3school.com.cn/cssref/selector_link.asp) | a:link                | 选择所有未被访问的链接。                                     |    1    |
| [:visited](https://www.w3school.com.cn/cssref/selector_visited.asp) | a:visited             | 选择所有已被访问的链接。                                     |    1    |
| [:active](https://www.w3school.com.cn/cssref/selector_active.asp) | a:active              | 选择活动链接。                                               |    1    |
| [:hover](https://www.w3school.com.cn/cssref/selector_hover.asp) | a:hover               | 选择鼠标指针位于其上的链接。                                 |    1    |
| [:focus](https://www.w3school.com.cn/cssref/selector_focus.asp) | input:focus           | 选择获得焦点的 input 元素。                                  |    2    |
| [:first-letter](https://www.w3school.com.cn/cssref/selector_first-letter.asp) | p:first-letter        | 选择每个\<p>元素的首字母。                                   |    1    |
| [:first-line](https://www.w3school.com.cn/cssref/selector_first-line.asp) | p:first-line          | 选择每个\<p>元素的首行。                                     |    1    |
| [:first-child](https://www.w3school.com.cn/cssref/selector_first-child.asp) | p:first-child         | 选择属于父元素的第一个子元素的每个\<p>元素。                 |    2    |
| [:before](https://www.w3school.com.cn/cssref/selector_before.asp) | p:before              | 在每个\<p>元素的内容之前插入内容。                           |    2    |
| [:after](https://www.w3school.com.cn/cssref/selector_after.asp) | p:after               | 在每个\<p>元素的内容之后插入内容。                           |    2    |
| [:lang(*language*)](https://www.w3school.com.cn/cssref/selector_lang.asp) | p:lang(it)            | 选择带有以 "it" 开头的 lang 属性值的每个\<p>元素。           |    2    |
| [*element1*~*element2*](https://www.w3school.com.cn/cssref/selector_gen_sibling.asp) | p~ul                  | 选择前面有\<p> 元素的每个\<ul>元素。                         |    3    |
| [[*attribute*^=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_begin.asp) | a[src^="https"]       | 选择其 src 属性值以 "https" 开头的每个\<a>元素。             |    3    |
| [[*attribute*$=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_end.asp) | a[src$=".pdf"]        | 选择其 src 属性以 ".pdf" 结尾的所有\<a>元素。                |    3    |
| [[*attribute**=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_contain.asp) | a[src*="abc"]         | 选择其 src 属性中包含 "abc" 子串的每个\<a>元素。             |    3    |
| [:first-of-type](https://www.w3school.com.cn/cssref/selector_first-of-type.asp) | p:first-of-type       | 选择属于其父元素的首个\<p> 元素的每个\<p>元素。              |    3    |
| [:last-of-type](https://www.w3school.com.cn/cssref/selector_last-of-type.asp) | p:last-of-type        | 选择属于其父元素的最后\<p> 元素的每个\<p>元素。              |    3    |
| [:only-of-type](https://www.w3school.com.cn/cssref/selector_only-of-type.asp) | p:only-of-type        | 选择属于其父元素唯一的\<p> 元素的每个\<p>元素。              |    3    |
| [:only-child](https://www.w3school.com.cn/cssref/selector_only-child.asp) | p:only-child          | 选择属于其父元素的唯一子元素的每个\<p>元素。                 |    3    |
| [:nth-child(*n*)](https://www.w3school.com.cn/cssref/selector_nth-child.asp) | p:nth-child(2)        | 选择属于其父元素的第二个子元素的每个\<p>元素。               |    3    |
| [:nth-last-child(*n*)](https://www.w3school.com.cn/cssref/selector_nth-last-child.asp) | p:nth-last-child(2)   | 同上，从最后一个子元素开始计数。                             |    3    |
| [:nth-of-type(*n*)](https://www.w3school.com.cn/cssref/selector_nth-of-type.asp) | p:nth-of-type(2)      | 选择属于其父元素第二个\<p>元素的每个\<p> 元素。              |    3    |
| [:nth-last-of-type(*n*)](https://www.w3school.com.cn/cssref/selector_nth-last-of-type.asp) | p:nth-last-of-type(2) | 同上，但是从最后一个子元素开始计数。                         |    3    |
| [:last-child](https://www.w3school.com.cn/cssref/selector_last-child.asp) | p:last-child          | 选择属于其父元素最后一个子元素每个\<p>元素。                 |    3    |
| [:root](https://www.w3school.com.cn/cssref/selector_root.asp) | :root                 | 选择文档的根元素。                                           |    3    |
| [:empty](https://www.w3school.com.cn/cssref/selector_empty.asp) | p:empty               | 选择没有子元素的每个\<p>元素（包括文本节点）。               |    3    |
| [:target](https://www.w3school.com.cn/cssref/selector_target.asp) | #news:target          | 选择当前活动的 #news 元素。                                  |    3    |
| [:enabled](https://www.w3school.com.cn/cssref/selector_enabled.asp) | input:enabled         | 选择每个启用的\<input>元素。                                 |    3    |
| [:disabled](https://www.w3school.com.cn/cssref/selector_disabled.asp) | input:disabled        | 选择每个禁用的\<input>元素                                   |    3    |
| [:checked](https://www.w3school.com.cn/cssref/selector_checked.asp) | input:checked         | 选择每个被选中的\<input>元素。                               |    3    |
| [:not(*selector*)](https://www.w3school.com.cn/cssref/selector_not.asp) | :not(p)               | 选择非\<p>元素的每个元素。                                   |    3    |
| [::selection](https://www.w3school.com.cn/cssref/selector_selection.asp) | ::selection           | 选择被用户选取的元素部分。                                   |    3    |

摘自：[runoob - CSS 选择器](https://www.runoob.com/cssref/css-selectors.html)

***



## web兼容

#### CSS Hack

面对浏览器诸多的兼容性问题，经常需要通过修改CSS样式来调试，其中用的最多的就是CSS Hack。所谓CSS Hack就是针对不同的浏览器书写不同的CSS样式，通过使用某个浏览器单独识别的样式代码，控制该浏览器的显示效果。

摘自：[常用的hack技巧！](https://www.jianshu.com/p/a65486c56d19)



#### 条件注释

条件注释（conditional comment）是<font color=FF0000>于HTML源码中被 Microsoft Internet Explorer 有条件解释的语句</font>。条件注释<font color=FF0000>可被用来向 Internet Explorer 提供及隐藏代码</font>。

条件注释<font color=FF0000>最初于微软的 Internet Explorer 5浏览器中出现，并且直至 Internet Explorer 9 均支持</font>。<mark>微软已宣布于 Internet Explorer 10 中以标准模式处理页面 - 如 HTML5 - 时停止支持，但是旧版网页使用这种技术（于兼容性视图）将继续有效</mark>。JScript 条件注释于 Internet Explorer 4 中被引进，而在 Internet Explorer 10 中继续受支持，无论于标准模式或者兼容性模式之中，但在 Windows 应用商店应用程序中不受支持。

##### **语法：**

有两种“条件注释”：<font color=FF0000>**下层显示 (downlevel revealed)**</font>和<font color=FF0000>**下层隐藏(downlevel hidden)**</font>。

|                           注释类型                           |                        句法或可能取值                        |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                        标准 HTML 注释                        |                 \<!-- Comment content  --\>                  |
|  <font color=FF0000>**上层显示**</font>（downlevel-hidden）  | <font color=red>\<!--</font><font color=blue>[if expression]</font><font color=FF0000>></font> *HTML* <font color=FF0000><!</font><font color=blue>[endif]</font><font color=FF0000>--\></font> |
| <font color=FF0000>**下层隐藏**</font>（downlevel-revealed） | <font color=FF0000>\<!</font>[if *expression*]<font color=FF0000>></font> *HTML* <font color=FF0000><!</font>[endif]<font color=FF0000>\></font> |

于每个条件注释之中的句法块内的 HTML 表示任意的 HTML 内容块，包括脚本。<font color=FF0000>两种条件注释均使用条件**表达式**以指示注释块内的内容应该被解析还是被忽略</font>。条件表达式由特性，操作符，和/或决定于其特性的值组成。

下表展示了支持的特性并描述了每种特性支持的值。

|      项目      |         示例          |                             说明                             |
| :------------: | :-------------------: | :----------------------------------------------------------: |
|       IE       |        [if IE]        | 字符串 "IE" 是一种对应于用以浏览网页的 Internet Explorer 的版本的一种*特性*。 |
|     value      |       [if IE 7]       | 一个对应于浏览器版本的<font color=FF0000>整数</font>或<font color=FF0000>浮点数</font>。<font color=FF0000>返回一个布尔值<，版本号和浏览器版本相匹配时为 true</font> |
| WindowsEdition |  [if WindowsEdition]  | 适用于 Windows 7 上的 Internet Explorer 8。字符串 "WindowsEdition" 是一种对应于用以浏览该网页的 Microsoft Windows 版本的*特性*。 |
|     value      | [if WindowsEdition 1] | 一个对应于用以浏览该网页的 Windows 的*版本*的整数。返回一个布尔值，数值和使用的版本相匹配时为真 true。关于所支持的值和它们所描述的版本的更多信息，参见GetProductInfo 函数的 *pdwReturnedProductType* 参数。 |
|      true      |       [if true]       |                       永远等价于 true.                       |
|     false      |      [if false]       |                      永远等价于 false.                       |

可用于创造条件注释的算符如下表：

| 项目 |           示例           |                             说明                             |
| :--: | :----------------------: | :----------------------------------------------------------: |
|  !   |         [if !IE]         | NOT 运算符。这被放在 *特性*, *算符*, 或者 *子表达式* 的前面以反转该表达式的布尔值含义。 |
|  lt  |      [if lt IE 5.5]      |          小于运算符。第一项小于第二项时返回 true。           |
| lte  |      [if lte IE 6]       |    小于或等于运算符。第一项小于或等于第二项时返回 true。     |
|  gt  |       [if gt IE 5]       |          大于运算符。第一项大于第二项时返回 true。           |
| gte  |      [if gte IE 7]       |    大于或等于运算符。第一项大于或等于第二项时返回 true。     |
| ( )  |       [if !(IE 7)]       |   子表达式运算符。用以连接布尔算符以创造更加复杂的表达式。   |
|  &   | [if (gt IE 5)&(lt IE 7)] |          AND 运算符。所有子表达式为真时返回 true。           |
|  \|  |   [if (IE 6)\|(IE 7)]    |         OR 运算符。子表达式任意一个为真时返回 true。         |

- **<font color=FF0000>下层隐藏</font>的条件注释**

  如下是两个“下层隐藏”条件注释的示例。

  - 示例中的指令将会让 IE 8 读取指定的 CSS 文件，而 IE 7 或者其它版本的 IE 将会忽略它。非 IE 的浏览器同样会把它忽略因为它看起来像一条标准的 HTML 注释

    ```html
    <!--[if IE 8]>
    <link href="ie8only.css" rel="stylesheet">
    <![endif]-->
    ```

  - 示例里的标记将会让 IE 5 至 7 读取其内的 CSS 样式。通过对这种标记的不同的使用你也可以挑出 IE 6, IE 5 或者比指定版本更新（大）或更旧（小）版本的 IE。

    ```html
    <!--[if lte IE 7]>
    <style>
    	/* CSS here */
    </style>
    <![endif]-->
    ```

- **<font color=FF0000>下层显示</font>的条件注释**

  如下是一个“下层显示”条件“注释”的示例，<font color=FF0000>它除了误导向的名字之外，根本*不是一个 (X)HTML* 注释，使用默认的微软语法</font>：

  ```html
  <![if !IE]>
  <link href="non-ie.css" rel="stylesheet">
  <![endif]>
  ```

  这个示例展示了应该<font color=FF0000>仅对非 IE 浏览器暴露的内容</font>，由于该条件对 IE 为假（并且因此该内容被忽略），而这些标签自身在非 IE 浏览器中是无法识别的（并因此被忽略）。这不是有效的 HTML 或 XHTML。

  那如果想要符合W3C标准要如何写呢？答案如下：
  
  ```html
  <!--[if !IE]>-->
  <link href="non-ie.css" rel="stylesheet">
  <!--<![endif]-->
  ```

示例：

```html
<!--[if !IE]>-->
<script type="text/javascript" src="<%=Page.ResolveClientUrl("~/Scripts/jquery-2.1.4.min.js")  %>"></script>
<!--<![endif]-->

<!--[if lt IE 9]>
    <script type="text/javascript" src="<%=Page.ResolveClientUrl("~/Scripts/jquery-1.12.4.min.js")%>"></script>
<![endif]-->
```

当使用IE5-IE9时，正常识别条件注释，跳过 **!IE** 的部分，读取 **lt IE 9** 里面的内容，当使用其他版本时忽略条件注释，读取2.1.4版本jQuery

摘自：[维基百科 - 条件注释](https://zh.wikipedia.org/wiki/%E6%9D%A1%E4%BB%B6%E6%B3%A8%E9%87%8A)  [条件注释的两种形式——下层隐藏与下层显示](https://zhuanlan.zhihu.com/p/150115755)

**使用示例：**

```html
<!--[if !IE]><!--> 除IE外都可识别 <!--<![endif]-->
<!--[if IE]> 所有的IE可识别 <![endif]-->
<!--[if IE 6]> 仅IE6可识别 <![endif]-->
<!--[if lt IE 6]> IE6以及IE6以下版本可识别 <![endif]-->
<!--[if gte IE 6]> IE6以及IE6以上版本可识别 <![endif]-->
<!--[if IE 7]> 仅IE7可识别 <![endif]-->
<!--[if lt IE 7]> IE7以及IE7以下版本可识别 <![endif]-->
<!--[if gte IE 7]> IE7以及IE7以上版本可识别 <![endif]-->
<!--[if IE 8]> 仅IE8可识别 <![endif]-->
<!--[if IE 9]> 仅IE9可识别 <![endif]-->
```

摘自：[条件注释判断浏览器<!--[if !IE]><!--[if IE]><!--[if lt IE 6]><!--[if gte IE 6]>](https://www.cnblogs.com/dtdxrk/archive/2012/03/06/2381868.html)



#### flex兼容

Flexbox布局的语法经过几年发生了很大的变化，也给Flexbox的使用带来一定的局限性，因为语法版本众多，浏览器支持不一致，致使Flexbox布局使用不多。

Flexbox布局主要有三种语法版本：

2009版本，我们称之为老版本，使用的是“display:box”或者“display:inline-box”；

2011版本，我们称之为混合版本，使用的是“display:flexbox”或者“display:inline-flexbox”；

2013版本，也就是最新语法版本，使用的是“display:flex”或者“display:inline-flex”。

https://www.jianshu.com/p/654cd19da3e7



#### 兼容IE8的方法：

- **HTML5标签兼容方案：html5shiv.js**

  GitHub地址：https://github.com/aFarkas/html5shiv/

  IE8不支持HTML5的新标签，如\<header>、\<nav>等标签在IE8无法渲染。html5shiv.js可帮助IE6-8浏览器兼容HTML5语义化标签。

  **使用方法：**在页面中引用html5shiv.js文件。必须添加在页面的\<head>元素内，因为IE浏览器必须在元素解析前知道这个元素，所以这个js文件不能在页面底部引用。

- **CSS3媒体查询兼容方案：Respond.js**

  GitHub地址：https://github.com/scottjehl/Respond

  IE8不支持CSS媒体查询，对响应式设计大大不利。Respond.js可帮助IE6-8兼容“min/max-width”媒体查询条件。

  使用方法：在页面中所有css文件的引用位置之后引用Respond.js。而且Respond.js的引用得越早，用户看到页面闪烁的机会越小。

- **CSS3字体单位“rem”兼容方案：rem.js**

  GitHub地址：https://github.com/chuckcarpenter/REM-unit-polyfill

  CSS3引入了新的字体大小单位rem，与em的“相对于其父元素来设置字体大小”的功能不同，rem是相对于根元素\<html>的字体大小比率单位，成了目前主流的单位之一。IE9+开始支持，IE8就只能通过引入js库来支持了。

  使用方法：在页面中引用rem.js文件。需要引用在页脚，也就是\<body>末尾，在所有css文件引用和DOM元素之后。

- **CSS3“background-size”属性的“cover”和“contain”属性值兼容方案：background-size polyfill**

  GitHub地址：https://github.com/louisremi/background-size-polyfill

  “background-size”是CSS3新引入的属性，其中有两个属性值非常常用，分别为“cover”和“contain”。“cover”可以把背景图像扩展至足够大，以使背景图像完全覆盖背景区域，背景图像的某些部分也许无法显示在背景定位区域中。“contain”可以把图像图像扩展至最大尺寸，以使其宽度和高度完全适应内容区域。IE8同样不支持，非常不方便。这时可以引用“background-size polyfill”库来兼容。

  使用方法：与以上几个库不同，“background-size polyfill”的代码文件需要在css中引用。在所有用到这两个“background-size”属性值的地方，加一行“-ms-behavior”属性：

  ```css
  .selector { 
      background-size: cover;
      /* 以下相对路径是相对于文档，而非css文件！ */
      /* 使用绝对路径可以避免混淆 */
      -ms-behavior: url(/backgroundsize.min.htc);
  }
  ```

- **JS数组的forEach方法兼容方案：自行实现**

  IE8的数组对象没有forEach方法，晕。所以自行声明即可，代码如下：

  ```js
  if ( !Array.prototype.forEach ) {
      Array.prototype.forEach = function forEach( callback, thisArg ) {
          var T, k;
          if ( this == null ) {
              throw new TypeError( "this is null or not defined" );
          }
          var O = Object(this);
          var len = O.length >>> 0;
          if ( typeof callback !== "function" ) {
              throw new TypeError( callback + " is not a function" );
          }
          if ( arguments.length > 1 ) {
              T = thisArg;
          }
          k = 0;
          while( k < len ) {
              var kValue;
              if ( k in O ) {
                  kValue = O[ k ];
                  callback.call( T, kValue, k, O );
              }
              k++;
          }
      };
  }
  ```

- **SVG图形兼容方案：优雅降级**

  参考文章：http://www.zhangxinxu.com/wordpress/2013/09/svg-fallbacks/

  对于svg图形是真的无法直接兼容了，因此使用优雅降级，在IE8下显示替代的jpg、png或gif图片。有三种比较实用的方法：一是用js修改\<img>的src属性，这里省略；二是用HTML的hack实现优雅降级，类似于如下代码：

  ```html
  <svg width="96" height="96">
    <image xlink:href="svg.svg" src="svg.png" width="96" height="96" />
  </svg>
  ```

  支持\<svg>标签的浏览器会显示svg.svg，老版本浏览器会无视\<svg>标签，渲染\<image>标签，从而显示svg.png。

  此外，还有一种比较巧妙的方法：

  ```html
  <img src="image.svg" onerror="this.src='image.png'">
  ```

  此法有弊端：当image.png出现问题无法载入时，会陷入死循环。

- **Canvas兼容方案：Excanvas.js**

  下载地址：http://code.google.com/p/explorercanvas/downloads/list

  Canvas的功能非常强大，兼容IE8的工作也很繁巨。可能有很大一部分情况要用优雅降级，但是一些情况下可以使用Google出的Excanvas.js库。它是利用IE支持的VML对象来模拟Canvas的绘图的，有些情况下可用，但无法穷尽Canvas的所有功能。

  使用方法：在页面中引用Excanvas.js文件，最好在\<head>标签中。

  具体注意事项可以参考文章：http://rockyuse.iteye.com/blog/1618298

- **Canvas+WebGL兼容方案：优雅降级**

  最近WebGL库——Three.js越来越流行了，但它只支持IE11+，IE8的兼容好像无解……所以只能优雅降级，但是效果肯定大打折扣。

摘自：[前端页面兼容ie8解决方法](https://www.cnblogs.com/vickya/p/8033944.html)



***

## Less

#### **less的特点：**

- 变量：就像写其他语言一样，免于多处修改。
- 混合：class 之间的轻松引入和继承。
- 嵌套：选择器之间的嵌套使你的 less 非常简洁。
- 函数&运算：就像 js 一样，对 less 变量的操控更灵活。

摘自：[浅谈css预处理器，Sass、Less和Stylus](https://zhuanlan.zhihu.com/p/23382462)



#### 将Less编译成css的方法

- **使用node.js编译（推荐）**

  ```sh
  lessc compiled.less > output.css
  ```

- **浏览器使用**

  在浏览器中使用Less.js是开始开发的最简单方法，而且使用较少的开发也很方便，但是在生产中，<mark>当性能和可靠性非常重要时，建议使用Node.js或许多可用的第三方工具之一进行预编译</mark>。

  第一将你写的less样式表.通过link链进HTML并且将rel属性设置为stylesheet/less

  ```html
  <link rel="stylesheet/less" type="text/css" href="styles.less"/>
  <!-- 下面这种写法也行，自行选择 -->
  <link rel="stylesheet" type="text/less" href="styles.less"/>
  ```

  接下来下载[less.js](https://github.com/less/less.js/archive/master.zip)并将script标记在`head`元素中:

  ```
  <head>
  <link rel="stylesheet" type="text/less" href="styles.less"/>
  <script src="less.js" type="text/javascript"></script>
  </head>
  复制代码
  ```

- **3.下载koala软件编译less**

以上摘自：[less的几种编译成css的方法](https://juejin.im/post/6844903891922845710)



#### **注释**

```less
// 不会被编译到css文件中，相当于：给开发人员看的注释
/* 会被编译到css文件中，相当于：给客户看的注释 */
```



#### **声明**

- 声明变量，示例：

  ```less
  @color:pink; //声明变量
  background: @color; //使用声明的变量
  ```

- 声明属性（比如margin），示例：

  ```less
  @m:margin; //声明属性
  @{m}: 0;     //使用声明的属性
  ```

- 声明选择器，示例：

  ```less
  @selector: #wrap;  //声明选择器
  @{selector}{       //使用声明的选择器
    //...
  }
  ```

- 类似的还有URL，同样使用`@{url}`



#### 延迟加载

如果定义变量两次，将搜索并使用当前范围内变量的上一次定义。此方法类似于CSS本身，<font color=FF0000>其中值是从定义中的最后一个属性提取的</font>。

摘自：[Less 变量延迟加载范围](https://www.w3cschool.cn/less/less_lazy_loading_scope.html)

**原因：**会等到less中的所有文本都被“读完”，然后再将less中的变量替换成css



#### less中的&

内层选择器前面的 & 符号就表示<font color=FF0000>对父选择器的引用</font>。

在一个内层选择器的前面，如果<mark>没有 & 符号，则它<font color=FF0000>被解析为父选择器的后代</font></mark>；如果<mark>有 & 符号，它就<font color=FF0000>被解析为父元素自身或父元素的伪类</font></mark>。同时用在选择器中的&还可以反转嵌套的顺序并且可以应用到多个类名上

摘自：[less 的 & 详解](https://www.jianshu.com/p/127b0974cfc3)



#### less的混合

混合就是将一系列属性从一个规则集引入另一个规则集的方式

1. 普通混合
2. 不带输出的混合
3. 带参数的混合
4. 带参数并且有默认值的混合
5. 带多个参数的混合
6. 命名参数
7. 匹配模式
8. arguments变量



#### less计算

不同于css，less中是可以进行表达式计算的

示例：

```less
#wrap .foo{
  width: (100 + 100px);
}
```

另外，由示例发现：less的表达式只需其中一方带单位即可

**补充：**

CSS出了一个calc()方法，不过只适合用于长度的计算。



#### less继承



#### less避免编译

```less
~"foo"
```



***



## Sass

**Sass 与 Scss的区别**

SCSS（Sassy CSS） 语法使用 `.scss` 文件扩展名。<mark>除了极少部分的例外， 它是 CSS 的超集</mark>，也就是说 **所有有效的 CSS 也同样都是有效的 SCSS** 。 由于其与 CSS 的相似性，它是最容易上手的语法， 也是最流行的语法。

<mark>缩进语法（类似于Python的缩进）</mark>是 Sass 的原始语法，因此它使用文件 扩展名 `.sass` 。由于这个扩展名的原因，这种语法有时直接被称为 “Sass"。 <font color=FF0000>缩进语法支持与 SCSS 相同的所有特性，但是它使用 缩进而不是花括号和分号来描述文档的格式</font>。

摘自：https://sass.bootcss.com/documentation/syntax

**Scss和Sass<font color=FF0000>代码写法上</font>的区别，如图：**

|                             Scss                             |                             Sass                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| <img src="https://s1.ax1x.com/2020/08/27/d5iUIS.png" style="zoom:40%;" /> | <img src="https://s1.ax1x.com/2020/08/27/d5iIMR.png" style="zoom:48%;" /> |



#### 将sass文件编译成css文件

```sh
sass compiled_file.sass:output_file.css
```

**使用sass自动监视sass文件的变化，<font color=FF0000>自动编译</font>（类似于热部署），避免每次都需要输入命令，这时只需加上参数`--watch`即可**

```sh
sass --watch comiled_file_folder:output_folder
```



#### **输出css文件的样式**

- **nested**：嵌套<font color=FF0000>（默认）</font>
- **compact**：紧凑
- **expanded**：扩展
- **compressed**：压缩

**由上面的编译成css文件，结合输出文件的格式，于是有参数 `--style`，示例：**

```sh
sass --watch comiled_file_folder:output_folder --style compact
```



#### 启用sass命令行

```sh
sass -i
```



#### Scss变量

```scss
$attribute: value
```

示例：

```scss
$primary-color: #1269b5;
```

甚至可以在其他声明中使用变量，示例：

```scss
$primary-border: 1px solid $primary-color;
```

另外：在创建变量名时候，对于单词间连接，可以使用`-`，也可以使用`_`；但是最好做到风格统一



#### 嵌套时调用父选择器

使用`&`，将会引用父选择器。示例：

- scss代码

   ```scss
    &:hover{
    	background-color: #0d2f7e;
    	color: #fff;
    }
   ```

    编译css，结果：

    ```css
    .nav ul a:hover {
      background-color: #0d2f7e;
    	color: #fff;
    }
    ```

- scss代码

   ```scss
     & &-text{
       font-size: 15px;
     }
   ```

    编译css，结果：

   ```css
   .nav .nav-test {
     font-size: 15px;
   }
   ```



#### 属性嵌套

示例：

- 原本：

  ```scss
  body{
    body-family: Helvetica, Arial, sans-serif;
    body-size: 15px;
    body-weight: normal;
  }
  ```

- 经过属性嵌套后：

  ```scss
  body{
    font: {
    	family: Helvetica, Arial, sans-serif;
    	size: 15px;
    	weight: normal;
    }
  }
  ```

**甚至：**

- 原本：

  ```scss
  .nav{
    border: 1px solid #000;
    border-left: 0;
    border-right: 0;
  }
  ```

- 经过属性嵌套后：

  ```scss
  .nav{
    border: 1px solid #000 {
      left: 0;
      right: 0;
      }
  }
  ```

  

#### mixin

<font color=FF0000>**定义**mixin</font>，语法：

```scss
@mixin mixin-name (para1, para2, ...){
  //样式
}
```

<font color=FF0000>**调用**mixin</font>，语法：

```scss
@include: mixin-name;
```



#### 继承 @extend

示例：

```scss
.alert{
  padding: 15px;
}

//alert-info继承.alert
.alert-info { 
	@extend: .alert;
  background-color: #d9edf7;
}
```

另外需要注意的是：<font color=FF0000>与父类相关的选择器中的样式，也会被子类继承下来</font>。示例：

```scss
.alert{
  padding: 15px;
}

.alert a{
  font-weight: bold;
}

#alert-info也会继承alert下面的a
.alert-info { 
	@extend: .alert;
  background-color: #d9edf7;
}
```



#### @import

CSS本身就有@import的功能，在一个css文件中，使用@import可以将其他的css文件包含进来。不过，每次使用@import，浏览器都会发出一次新的Http请求，下载被导入的css文件，这样的话会消耗服务器的资源。sass<font color=FF0000>扩展</font>了css的@import

SASS会以文件为单位进行编译，<font color=FF0000>如果不想某个scss文件单独编译，可以将其处理为partial文件，也即在文件名前加上一个下划线，比如`_base.css`</font>



而在引入时，无需加上下划线，也不需要填写文件类型；示例：

```scss
@import "base";
```



#### comment

jquery有三种注释：

- **/*\*/** <mark>不会出现在compressed模式编译的css文件中</mark>
- **//**     不会显示在编译后的css文件中
- **/*! */** <font color=FF0000>强制输出的注释内容</font>



#### Data Type

```scss
type-of(variable) //查看变量类型

type-of(5)                //"number"
type-of(5px) 							//"number"

type-of(hello)  					//"string"
type-of("hello") 					//"string"

type-of(1px solid #000)   //"list"

type-of(#ff0000)					//"color"
type-of(red) 							//"color"
type-of(rgb(255, 0, 0))   //"color"
type-of(hsl(0, 100%, 50%))//"color"
```



#### number

`8 / 2`结果是： `8/2`，原因是有类似用法：`font: 16px/1.8 serif`，如果想要得到的结果为数字，则要将表达式放入括号：`(8 / 2)`

```scss
5px + 5px     //10px
5px - 2				//3px
5px * 2 			//10px
5px * 2px 		//10px*px
(10px / 2px)  //5
10px / 2px		//10px/2px
10px / 2			//10px/2
(10px / 2) 		//5px
```



#### 数字函数

- **abs()** 绝对值
- **round()**：四舍五入
- **ceil()**：向上取整
- **floor()**：向下取整
- **percentage()**：转化为百分比

- max()
- min()



#### 字符串

sass中的字符串有两种，带引号和不带引号两种。

其中带引号的，可以加上空格；不带引号的不能加上空格。

- 带引号的字符串加上（+）不带引号的字符串返回带引号的字符串

  ```scss
  "hello" + world    //"helloworld"
  ```

- 不带引号的字符串加上（+）带引号的字符串返回不带引号的字符串

  ```scss
  hello + "world"     //helloworld
  ```

- 不带引号的字符串加上（+）不带引号的字符串返回不带引号的字符串

  ```scss
  hello + world				//helloworld
  ```

**另外：**

```scss
hello - world          //"hello-world"
```

```scss
hello / world          //"hello/world"
//之所以存在是因为，有意义
```

```scss
hello * world					 将会报错：Error: Undefined operation "hello * world".
//字符串的乘法没有意义，
```



#### 字符串函数

- **to-upper-case()：**

- **to-lower-case()：**
- **str-length()：**得到字符串的长度

- **str-index()：**获取字符串的位置，**示例：**

  ```scss
  str-index(str, sub_str)
  ```

- **str-insert()：**插入字符串，示例：

  ```scss
  str-insert(inserted_str, insert_str, insert_index)
  ```



#### 颜色函数

- **RGB颜色函数**
  - **[rgb($red,$green,$blue)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#rgb-instance_method)**：根据**红**、**绿**、**蓝**三个值创建一个颜色；
  - **[rgba($red,$green,$blue,$alpha)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#rgba-instance_method)**：根据**红**、**绿**、**蓝**和**透明度**值创建一个颜色；
  - **[red($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#red-instance_method)**：从一个颜色中获取其中**红色**值；
  - **[green($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#green-instance_method)**：从一个颜色中获取其中**绿色**值；
  - **[blue($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#blue-instance_method)**：从一个颜色中获取其中**蓝色**值；
  - **[mix($color-1,$color-2,[$weight])](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#mix-instance_method)**：把两种颜色混合在一起。

- **HSL颜色函数**
  - **[hsl($hue,$saturation,$lightness)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#hsl-instance_method)**：通过色相（hue）、饱和度(saturation)和亮度（lightness）的值创建一个颜色；
  - **[hsla($hue,$saturation,$lightness,$alpha)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#hsla-instance_method)**：通过色相（hue）、饱和度(saturation)、亮度（lightness）和透明（alpha）的值创建一个颜色；
  - **[hue($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#hue-instance_method)**：从一个颜色中获取色相（hue）值；
  - **[saturation($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#saturation-instance_method)**：从一个颜色中获取饱和度（saturation）值；
  - **[lightness($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#lightness-instance_method)**：从一个颜色中获取亮度（lightness）值；
  - **[adjust-hue($color,$degrees)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#adjust_hue-instance_method)**：通过改变一个颜色的色相值，创建一个新的颜色；
  - **[lighten($color,$amount)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#lighten-instance_method)**：通过改变颜色的亮度值，让颜色变亮，创建一个新的颜色；
  - **[darken($color,$amount)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#darken-instance_method)**：通过改变颜色的亮度值，让颜色变暗，创建一个新的颜色；
  - **[saturate($color,$amount)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#saturate-instance_method)**：通过改变颜色的饱和度值，让颜色更饱和，从而创建一个新的颜色
  - **[desaturate($color,$amount)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#desaturate-instance_method)**：通过改变颜色的饱和度值，让颜色更少的饱和，从而创建出一个新的颜色；
  - **[grayscale($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#grayscale-instance_method)**：将一个颜色变成灰色，相当于`desaturate($color,100%)`;
  - **[complement($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#complement-instance_method)**：返回一个补充色，相当于`adjust-hue($color,180deg)`;
  - **[invert($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#invert-instance_method)**：反回一个反相色，红、绿、蓝色值倒过来，而透明度不变。

- **Opacity函数**
  - **[alpha($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#alpha-instance_method) /[opacity($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#opacity-instance_method)**：获取颜色透明度值；
  - **[rgba($color, $alpha)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#rgba-instance_method)**：改变颜色的透明度值；
  - **[opacify($color, $amount)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#opacify-instance_method) / [fade-in($color, $amount)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#fade_in-instance_method)**：使颜色<font color=FF0000>更不透明</font>；
  - **[transparentize($color, $amount)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#transparentize-instance_method) / [fade-out($color, $amount)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#fade_out-instance_method)**：使颜色<font color=FF0000>更加透明</font>。

摘自：[Sass中文网 - Sass基础——颜色函数](https://www.sass.hk/skill/sass25.html)



#### 列表函数

- **length($list)：**返回`$list`长度（如果不是list,返回1）
- **nth($list,$index)：**返回`$list`中第`$index`列表项值（如果索引值不在列表范围内，将会报错）
  - **nth(list, element)：**获取list中element的索引
- **index($list,$value)：**返回`$value`在`$list`中的位置
- **append($list,$value[,$separator])：**使用`$separator`分隔符将`$value`列表项添加到`$list`最后（如果没有显式指定`$separator`分隔符，会以当前分隔符分隔）
- **join($list-1,$list-2[,$separator]):**使用`$separator`分隔符将`$list-2`附加到`$list-1`（如果没有显式指定分隔符，将对`$list-1`中的分隔符）
- **zip(\*$lists):**将多个`$list`组合在一起成为一个多维列表。如果列表源长度并不是所有都相同，结果列表长度将以最短的一个为准
- **reject($list,$value)：**这是Compass中的一个函数，将`$value`值从`$list`中删除
- **compact(\*$args)：**Compass函数，返回一个删除非真值的新列表

摘自：[Sass中文网 - 理解Sass的list](https://www.sass.hk/skill/sass31.html)



#### Map

- 定义：

  ```scss
  $map: (key1: value1, key2: value2[, key3: value3])
  ```

- 取值：

  - 由key获取一个value

      ```scss
    map-get($map-name, map-key)
    ```

  - 获取全部的value

    ```scss
    map-values($map-name)
    ```

- 获取map的全部key，返回一个列表

  ```scss
  map-keys($map-name)
  ```

- 判断map中是否含有指定的key，返回true / false

  ```scss
  map-has-key($map-name, key-name)
  ```

- 合并map

  ```scss
  map-merge($map1, $map2)
  ```

- map移除一个键值对

  ```scss
  map-remove($map-name, key1[, key2])
  ```



####  Boolean

包含 and or not运算符



#### interpolation

可以让我们将一个值<font color=FF0000>（变量）</font>插入到另一个值<font color=FF0000>（常量）</font>中，使用方式：

```scss
#{$variable}
```

**使用场景，示例：**

- ```scss
  $version: "0.0.1";
  /*当前版本号是：#{$version}*/
  ```

- ```scss
  $name: "info";
  $attr: "border";
  
  .alert-#{$name}{
    #{$attr}-color: #ccc;
  }
  ```



#### @if

**语法：**

```scss
@if condition {
  /*todo*/
}@else if condition {
  /*todo*/
}@else {
  /*todo*/
}
```



#### @for

**语法：**

```scss
@for $var from <开始值> [through | to] <结束值> {
  /*todo*/
}
```

<font color=FF0000>**其中：through相当于`≤`，to相当于`<`**</font>



#### @each

@each用于循环list（类似于foreach），语法：

```scss
@each $var in $list{
  /*todo*/
}
```



#### @while

语法：

```scss
@while condition {
  /*todo*/
}
```



#### 自定义函数

语法：

```scss
@function func-name (param1, param2[, param3, ...]){
  /*todo*/
}
```



#### 警告与错误

```scss
@warn "warning info";
```



## bootstrap

```html
<!--悬浮向左-->
<span class="push-left"></span>
<!--悬浮向右-->
<span class=“push-right”></span>
```

