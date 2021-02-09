\*斜体\*



#### **在markdown中可以使用\<mark>\</mark>来时让文字背景高亮，效果如下**
<mark>在markdown中可以使用\<mark>\</mark>来时让文字背景高亮，效果如下</mark>
\<mark>标签默认背景颜色为黄色，如果想要其他颜色，可以使用style属性，style=background-color:colorName

**示例如下：**
<mark style=background-color:red>\<mark>标签默认背景颜色为黄色，如果想要其他颜色，可以使用style属性，style=background-color:colorName</mark>
若要换成其他颜色，使用其他<mark><font size=3 color=FF0000>**颜色关键字(Color Keywords)**</font></mark>即可：其他颜色关键字可见：**https://www.html.cn/book/css/appendix/color-keywords.htm#basic**
再者：如果要同时设置背景色和字体颜色，需要使用分号（ **;** ）分隔两个设置，示例如下：
<mark style=color:red;background-color:lime>如果要同时设置背景色和字体颜色，需要使用分号（ **;** ）分隔两个设置</mark>
虽然上面的\<mark style=background-color:>\<font>...\</font>\</mark>也可以实现类似的效果



#### **在markdown中添加<mark>代码块</mark>，可以在\```之后加上语言名称，即可实现对应语言的语法高亮。示例如下**
  ```java
   @AutoConfigurationPackage
   @Import({EnableAutoConfigurationImportSelector.class})
   public @interface EnableAutoConfiguration {...}
  ```
更多支持的语言，见链接：[**markdown代码块支持的语言**](https://www.jianshu.com/p/1f223eb78ad8)，以及补充：[**markdown 代码块(```code)支持的语言列表**](https://www.jianshu.com/p/d32a3328489c)

同时：若要标注<mark>**段落中的代码**</mark>，使用<mark> **\`** </mark>即可，示例如下：`print('hello wolrd')`



#### **若要用markdown生成多级列表，方法如下：在'-'前按下Tab即可；效果如下**
- 一级列表1
  - 二级列表1
  - 二级列表2
  - 二级列表3
- 一级列表2



#### **类似的，若要生成有序多级列表，方法如下：在写下一级有序列表时前缩进4个空格（按下Tab）即可；效果如下**

1. 一级列表1
   1. 二级列表1
   2. 二级列表2
   3. 二级列表3
2. 一级列表2



#### 使用markdown生成页内跳转方法：

定义一个锚(id)：\<span id="jump">跳转到的地方\</span>
使用markdown语法：\[点击跳转\](#jump)

#### **类似的markdown插入跳转的方法：**
\[链接文字\](链接网址 "Optinal title")
<font color=FF0000>**示例如下：**</font>
**This is an [example link](http://www.baidu.com/ "With a Title").** 



#### **markdown插入图片**

**有两种方法：**

- markdown特有格式：<font color=FF0000>**\!\[Alt text](address "optional title")**</font>
  - Alt text: 当按下Alt时显示的文字，用于描述图片的关键词，可以不写
  - address: 就是地址，可以写绝对路径和相对路径
  - optional title: 鼠标悬置于图片上会出现的标题文字，可以不写

<mark>同时若要对图片**进行缩放**，在**VS Code**和**Typora**中似乎只能使用<mark>HTML的格式：**\<img src="图片地址" alt="Alt文本" style="zoom: 缩放大小%;" />**</mark>

至于其他markdown编辑器似乎可用`![]()`，见知乎问题：[**markdown中插入图片怎么定义图片的大小或比例？**](https://www.zhihu.com/question/23378396)

至于更多markdown中插入图片的问题，比如：[**markdown多张图片并排显示**](https://www.cnblogs.com/jaycethanks/p/12201959.html)，遇到再说。



#### **markdown插入表格**

<font color=FF0000>**模板如下：**</font>
<font color=FF0000 size=4>\|</font>title1<font color=FF0000 size=4>\|</font>title2<font color=FF0000 size=4>\|</font>...<font color=FF0000 size=4>\|</font>
<font color=FF0000 size=4>\|-\|-\|-\|-\|</font>
<font color=FF0000 size=4>|</font>1-1<font color=FF0000 size=4>|</font>1-2<font color=FF0000 size=4>|</font>...<font color=FF0000 size=4>|</font>
<font color=FF0000 size=4>|</font>2-1<font color=FF0000 size=4>|</font>2-2<font color=FF0000 size=4>|</font>...<font color=FF0000 size=4>|</font>
<font color=FF0000 size=4>|</font>...<font color=FF0000 size=4>|</font>...<font color=FF0000 size=4>|</font>...<font color=FF0000 size=4>|</font>
效果如下：

|title1|title2|...|
|-|-|-|
|1-1|1-2|...|
|2-1|2-2|...|
|...|...|...|



#### **Markdown插入分割线**

使用\***以产生分割线，如下：

***



#### markdown中使标题居中

```html
<div align='center' ><font size='40'>实习总结报告</font></div>
```

效果如下：

<div align='center' ><font size='40'>实习总结报告</font></div>

