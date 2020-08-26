# HTML & CSS备忘录



//TODO

```HTML
<input type="hidden" id="">
```





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

详细说明如下：

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

- visibility:hidden可以隐藏某个元素，但<mark>隐藏的元素仍需占用与未隐藏之前一样的空间</mark>。也就是说，该元素虽然被隐藏了，但仍然会影响布局。

- display:none可以隐藏某个元素，<mark>且隐藏的元素不会占用任何空间</mark>。也就是说，该元素不但被隐藏了，而且该元素原本占用的空间也会从页面布局中消失。



#### 块和内联元素

- **块元素**是一个元素，占用了全部宽度，<font color=FF0000>在前后都是换行符</font>。

  块元素有：\<h1>，\<p>，\<div>

- **内联元素**只需要必要的宽度，<font color=FF0000>不强制换行</font>。

  内联元素有：\<span>，\<a>

**使得元素变成块元素和内联元素**

- `li {display:inline;}`：把列表项显示为内联元素
- `span {display:block;}`：把span元素作为块元素

#### **补充：inline-block**

**简单来说：**就是<mark>将<font color=FF0000>**对象**呈现为 **inline** 对象</font>，但是<font color=FF0000>**对象的内容**作为 **block** 对象</font>呈现</mark>。之后的内联对象会被排列在同一行内。比如我们可以给一个 link（a 元素）inline-block 属性值，使其既具有 block 的宽度高度特性又具有 inline 的同行特性。

**另外：**IE（低版本 IE）本来是不支持 inline-block 的，所以在 IE 中对内联元素使用 display:inline-block，理论上 IE 是不识别的，但使用 display:inline-block 在 IE 下会触发 layout，从而使内联元素拥有了 display:inline-block 属性的表象。

摘自：[block，inline和inline-block概念和区别](https://www.cnblogs.com/KeithWang/p/3139517.html)



#### CSS Position

元素可以使用的顶部，底部，左侧和右侧属性定位。然而，这些属性无法工作，除非是先设定position属性。他们也有不同的工作方式，这取决于定位方法。

position 属性的五个值：

- **static**：HTML 元素的默认值，即没有定位，遵循正常的文档流对象。
- **relative**：相对定位元素的定位是相对其正常位置。
- **fixed**：<mark>元素的位置相对于浏览器窗口是固定位置</mark>。
- **absolute**：<mark>绝对定位的元素的位置相对于最近的已定位父元素</mark>，如果元素没有已定位的父元素，那么它的位置相对于\<html>:
- **sticky**：sticky 英文字面意思是粘，粘贴，所以可以把它称之为粘性定位。**position: sticky;** 基于用户的滚动位置来定位。粘性定位的元素是依赖于用户的滚动，在 **position:relative** 与 **position:fixed** 定位之间切换。

**重叠的元素**

元素的定位与文档流无关，所以它们可以覆盖页面上的其它元素。<mark>z-index属性指定了一个元素的堆叠顺序（哪个元素应该放在前面，或后面）</mark>。一个元素可以有正数或负数的堆叠顺序，<mark>具有更高堆叠顺序的元素总是在较低的堆叠顺序元素的前面</mark>。



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

- 图片居中对齐

  ```css
  margin: auto;
  ```



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

| 函数                                 | 描述                                     |
| :----------------------------------- | :--------------------------------------- |
| matrix(*n*, *n*, *n*, *n*, *n*, *n*) | 定义 2D 转换，使用六个值的矩阵。         |
| translate(*x*, *y*)                  | 定义 2D 转换，沿着 X 和 Y 轴移动元素。   |
| translateX(*n*)                      | 定义 2D 转换，沿着 X 轴移动元素。        |
| translateY(*n*)                      | 定义 2D 转换，沿着 Y 轴移动元素。        |
| scale(*x*, *y*)                      | 定义 2D 缩放转换，改变元素的宽度和高度。 |
| scaleX(*n*)                          | 定义 2D 缩放转换，改变元素的宽度。       |
| scaleY(*n*)                          | 定义 2D 缩放转换，改变元素的高度。       |
| rotate(*angle*)                      | 定义 2D 旋转，在参数中规定角度。         |
| skew(*x-angle*, *y-angle*)           | 定义 2D 倾斜转换，沿着 X 和 Y 轴。       |
| skewX(*angle*)                       | 定义 2D 倾斜转换，沿着 X 轴。            |
| skewY(*angle*)                       | 定义 2D 倾斜转换，沿着 Y 轴。            |



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



### CSS3 盒子模型

<mark>弹性盒子</mark>是 CSS3 的<mark>一种新的布局模式</mark>。

CSS3 弹性盒（ Flexible Box 或 flexbox），是一种当页面需要<font color=FF0000>适应不同的屏幕大小以及设备类型时确保元素拥有恰当的行为的布局方式</font>。

引入弹性盒布局模型的目的是提供一种更加有效的方式来<mark>对一个容器中的子元素进行排列、对齐和分配空白空间</mark>。

<font color=FF0000>**弹性盒子**</font>由**<font color=FF0000>弹性容器(Flex container)</font>**和**<font color=FF0000>弹性子元素(Flex item)</font>**组成。

弹性容器<font color=FF0000>通过设置 display 属性的值</font>为 <font color=FF0000>**flex**</font> 或 **<font color=FF0000>inline-flex</font>**将其定义为弹性容器。

弹性容器内包含了<font color=FF0000>一个</font>或<font color=FF0000>多个</font>弹性子元素。弹性子元素<font color=FF0000>通常在弹性盒子内一行显示</font>。<font color=FF0000>**默认情况每个容器只有一行**</font>。

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

- order：排序指定

  **语法：**

  ```css
  order: <integer>
  ```

  \<integer>：用整数值来定义排列顺序，<font color=FF0000>数值小的排在前面</font>。可以为负值。

- **align-self**

  align-self属性用于设置弹性元素自身在侧轴（纵轴）方向上的对齐方式。

  **语法**

  ```css
  align-self: auto | flex-start | flex-end | center | baseline | stretch
  ```

  - **auto**：<font color=FF0000>默认值</font>，如果'align-self'的值为'auto'，则其计算值为元素的父元素的'align-items'值，<font color=FF0000>如果其没有父元素，则计算值为'stretch'</font>。
  - **flex-start**：弹性盒子元素的<font color=FF0000>侧轴（纵轴）</font>起始位置的边界紧靠住该行的侧轴<font color=FF0000>起始边界</font>。
  - **flex-end**：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴<font color=FF0000>结束边界</font>。
  - **center**：弹性盒子元素在该行的侧轴（纵轴）上<font color=FF0000>居中放置</font>。（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度）。
  - **baseline**：如弹性盒子元素的行内轴<font color=FF0000>与侧轴为同一条</font>，则该值与'flex-start'等效。其它情况下，该值将与基线对齐。
  - **stretch**：如果指定侧轴大小的属性值为'auto'，则其值会使项目的边距盒的尺寸尽可能接近所在行的尺寸，但同时会遵照'min/max-width/height'属性的限制。

- **flex**

  flex属性用于指定弹性子元素如何分配空间。

  语法

  ```css
  flex: auto | initial | none | inherit |  [ flex-grow ] || [ flex-shrink ] || [ flex-basis ]
  ```

  - auto: 计算值为 1 1 auto
  - initial: 计算值为 0 1 auto
  - none：计算值为 0 0 auto
  - inherit：从父元素继承
  - [ flex-grow ]：定义弹性盒子元素的扩展比率。
  - [ flex-shrink ]：定义弹性盒子元素的收缩比率。
  - [ flex-basis ]：定义弹性盒子元素的默认基准值。

**补充：示图一张**

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



#### 总结 / 文档：CSS 选择器

| 选择器                                                       | 例子                  | 例子描述                                           | CSS  |
| :----------------------------------------------------------- | :-------------------- | :------------------------------------------------- | :--- |
| [.*class*](https://www.w3school.com.cn/cssref/selector_class.asp) | .intro                | 选择 class="intro" 的所有元素。                    | 1    |
| [#*id*](https://www.w3school.com.cn/cssref/selector_id.asp)  | #firstname            | 选择 id="firstname" 的所有元素。                   | 1    |
| [*](https://www.w3school.com.cn/cssref/selector_all.asp)     | *                     | 选择所有元素。                                     | 2    |
| [*element*](https://www.w3school.com.cn/cssref/selector_element.asp) | p                     | 选择所有 \<p> 元素。                               | 1    |
| [*element*,*element*](https://www.w3school.com.cn/cssref/selector_element_comma.asp) | div,p                 | 选择所有\<div> 元素和所有\<p> 元素。               | 1    |
| [*element* *element*](https://www.w3school.com.cn/cssref/selector_element_element.asp) | div p                 | 选择\<div>元素内部的所有\<p>元素。                 | 1    |
| [*element*>*element*](https://www.w3school.com.cn/cssref/selector_element_gt.asp) | div>p                 | 选择父元素为\<div>元素的所有\<p>元素。             | 2    |
| [*element*+*element*](https://www.w3school.com.cn/cssref/selector_element_plus.asp) | div+p                 | 选择紧接在\<div>元素之后的所有\<p>元素。           | 2    |
| [[*attribute*\]](https://www.w3school.com.cn/cssref/selector_attribute.asp) | [target]              | 选择带有 target 属性所有元素。                     | 2    |
| [[*attribute*=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value.asp) | [target=_blank]       | 选择 target="_blank" 的所有元素。                  | 2    |
| [[*attribute*~=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value_contain.asp) | [title~=flower]       | 选择 title 属性包含单词 "flower" 的所有元素。      | 2    |
| [[*attribute*\|=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value_start.asp) | [lang\|=en]           | 选择 lang 属性值以 "en" 开头的所有元素。           | 2    |
| [:link](https://www.w3school.com.cn/cssref/selector_link.asp) | a:link                | 选择所有未被访问的链接。                           | 1    |
| [:visited](https://www.w3school.com.cn/cssref/selector_visited.asp) | a:visited             | 选择所有已被访问的链接。                           | 1    |
| [:active](https://www.w3school.com.cn/cssref/selector_active.asp) | a:active              | 选择活动链接。                                     | 1    |
| [:hover](https://www.w3school.com.cn/cssref/selector_hover.asp) | a:hover               | 选择鼠标指针位于其上的链接。                       | 1    |
| [:focus](https://www.w3school.com.cn/cssref/selector_focus.asp) | input:focus           | 选择获得焦点的 input 元素。                        | 2    |
| [:first-letter](https://www.w3school.com.cn/cssref/selector_first-letter.asp) | p:first-letter        | 选择每个\<p>元素的首字母。                         | 1    |
| [:first-line](https://www.w3school.com.cn/cssref/selector_first-line.asp) | p:first-line          | 选择每个\<p>元素的首行。                           | 1    |
| [:first-child](https://www.w3school.com.cn/cssref/selector_first-child.asp) | p:first-child         | 选择属于父元素的第一个子元素的每个\<p>元素。       | 2    |
| [:before](https://www.w3school.com.cn/cssref/selector_before.asp) | p:before              | 在每个\<p>元素的内容之前插入内容。                 | 2    |
| [:after](https://www.w3school.com.cn/cssref/selector_after.asp) | p:after               | 在每个\<p>元素的内容之后插入内容。                 | 2    |
| [:lang(*language*)](https://www.w3school.com.cn/cssref/selector_lang.asp) | p:lang(it)            | 选择带有以 "it" 开头的 lang 属性值的每个\<p>元素。 | 2    |
| [*element1*~*element2*](https://www.w3school.com.cn/cssref/selector_gen_sibling.asp) | p~ul                  | 选择前面有\<p> 元素的每个\<ul>元素。               | 3    |
| [[*attribute*^=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_begin.asp) | a[src^="https"]       | 选择其 src 属性值以 "https" 开头的每个\<a>元素。   | 3    |
| [[*attribute*$=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_end.asp) | a[src$=".pdf"]        | 选择其 src 属性以 ".pdf" 结尾的所有\<a>元素。      | 3    |
| [[*attribute**=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_contain.asp) | a[src*="abc"]         | 选择其 src 属性中包含 "abc" 子串的每个\<a>元素。   | 3    |
| [:first-of-type](https://www.w3school.com.cn/cssref/selector_first-of-type.asp) | p:first-of-type       | 选择属于其父元素的首个\<p> 元素的每个\<p>元素。    | 3    |
| [:last-of-type](https://www.w3school.com.cn/cssref/selector_last-of-type.asp) | p:last-of-type        | 选择属于其父元素的最后\<p> 元素的每个\<p>元素。    | 3    |
| [:only-of-type](https://www.w3school.com.cn/cssref/selector_only-of-type.asp) | p:only-of-type        | 选择属于其父元素唯一的\<p> 元素的每个\<p>元素。    | 3    |
| [:only-child](https://www.w3school.com.cn/cssref/selector_only-child.asp) | p:only-child          | 选择属于其父元素的唯一子元素的每个\<p>元素。       | 3    |
| [:nth-child(*n*)](https://www.w3school.com.cn/cssref/selector_nth-child.asp) | p:nth-child(2)        | 选择属于其父元素的第二个子元素的每个\<p>元素。     | 3    |
| [:nth-last-child(*n*)](https://www.w3school.com.cn/cssref/selector_nth-last-child.asp) | p:nth-last-child(2)   | 同上，从最后一个子元素开始计数。                   | 3    |
| [:nth-of-type(*n*)](https://www.w3school.com.cn/cssref/selector_nth-of-type.asp) | p:nth-of-type(2)      | 选择属于其父元素第二个\<p>元素的每个\<p> 元素。    | 3    |
| [:nth-last-of-type(*n*)](https://www.w3school.com.cn/cssref/selector_nth-last-of-type.asp) | p:nth-last-of-type(2) | 同上，但是从最后一个子元素开始计数。               | 3    |
| [:last-child](https://www.w3school.com.cn/cssref/selector_last-child.asp) | p:last-child          | 选择属于其父元素最后一个子元素每个\<p>元素。       | 3    |
| [:root](https://www.w3school.com.cn/cssref/selector_root.asp) | :root                 | 选择文档的根元素。                                 | 3    |
| [:empty](https://www.w3school.com.cn/cssref/selector_empty.asp) | p:empty               | 选择没有子元素的每个\<p>元素（包括文本节点）。     | 3    |
| [:target](https://www.w3school.com.cn/cssref/selector_target.asp) | #news:target          | 选择当前活动的 #news 元素。                        | 3    |
| [:enabled](https://www.w3school.com.cn/cssref/selector_enabled.asp) | input:enabled         | 选择每个启用的\<input>元素。                       | 3    |
| [:disabled](https://www.w3school.com.cn/cssref/selector_disabled.asp) | input:disabled        | 选择每个禁用的\<input>元素                         | 3    |
| [:checked](https://www.w3school.com.cn/cssref/selector_checked.asp) | input:checked         | 选择每个被选中的\<input>元素。                     | 3    |
| [:not(*selector*)](https://www.w3school.com.cn/cssref/selector_not.asp) | :not(p)               | 选择非\<p>元素的每个元素。                         | 3    |
| [::selection](https://www.w3school.com.cn/cssref/selector_selection.asp) | ::selection           | 选择被用户选取的元素部分。                         | 3    |

摘自：[runoob - CSS 选择器](https://www.runoob.com/cssref/css-selectors.html)

