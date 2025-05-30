# Web 相关备忘录



## 编码相关



#### 编码通用概念

escape 就有 “转义” 的意思，所以 JS 中有编码的 `escape()` 方法也是很自然的了；虽然，该方法已经废弃，不推荐使用。

> 💡 补充： 键盘中 `ESC` 键 也是 escape 的意思。



### unicode 相关

<font color=FF0000>Unicode 是一种字符集标准，用于对来自世界上不同语言、文字系统和符号进行编号和字符定义</font>。<font color=LightSeaGreen>通过给每个字符分配一个编号，程序员可以创建字符编码，让计算机在同一个文件或程序中存储、处理和传输任何语言组合</font>。

<font color=LightSeaGreen>在 Unicode 定义之前，在同一数据中混合使用不同的语言是很困难的，而且容易出错</font>。例如，一个字符集存储的是日文字符，而另一个字符集存储的是阿拉伯字母。如果没有明确标明数据的哪些部分属于哪个字符集，其他程序和计算机就会错误地显示文本，或者在处理过程中损坏文本。如果你曾经见过像  ( `“”` )被替换为胡言乱语 `Ã‚Â£` ，那么你就已经看到过这个被称为  [Mojibake](https://zh.wikipedia.org/wiki/Mojibake) 的问题。

<font color=fuchsia>**网络上最常见的 Unicode 字符编码是 UTF-8**</font>。还存在一些其他编码，如 UTF-16 或过时的 UCS-2，但推荐使用 UTF-8

摘自：[MDN - Unicode](https://developer.mozilla.org/zh-CN/docs/Glossary/Unicode)

#### 阮一峰文章《Unicode与JavaScript详解》笔记

> ⚠️ 由于这篇文章，写于2014年，所以下面部分内容 *或* 已过时

##### Unicode 是什么

Unicode源于一个很简单的想法：将全世界所有的字符包含在一个集合里，计算机只要支持这一个字符集，就能显示所有的字符，再也不会有乱码了。

> 👀 关于 Unicode 的介绍，可以看下 [[正则表达式#Unicode 属性 `\p{…}`]] 中的内容；其中说明了 Unicode 中类别/分类相关的内容

<font color=LightSeaGreen>它从 0 开始，为每个符号指定一个编号</font>，<font color=FF0000>这叫做"码点" ( code point ) </font>。比如，码点0 的符号就是 null（表示所有二进制位都是 0 ）。

```
U+0000 = null
```

上式中，U+ 表示 <font color=FF0000>**紧跟在后面的十六进制数是 Unicode 的码点**</font>。

<font color=LightSeaGreen>目前（2014年），Unicode 的最新版本是 7.0版，一共收入了 109449 个符号，其中的中日韩文字为 74500 个</font>。可以近似认为，全世界现有的符号当中，三分之二以上来自东亚文字。比如，中文"好"的码点是十六进制的 597D。

<font color=FF0000>这么多符号，Unicode 不是一次性定义的，而是 <font size=4>**分区定义**</font></font>。<font color=FF0000>每个区可以存放 65536 个 ( $2^{16}$ ) 字符，称为一个平面 ( plane ) </font>。<font color=FF0000>目前，一共有 17 个 ( $2^5$ ) 平面，也就是说，整个 Unicode 字符集的大小现在是 $2^{21}$</font>。

<font color=FF0000><font size=4>**最前面的 65536 个字符位**</font>，称为<font size=4>**基本平面（Basic Multilingual Plane，缩写 BMP ）**</font></font>，<font color=FF0000>它的码点范围是从 0 一直到 $2^{16}-1$ ，写成 16进制就是从 U+0000 到 U+FFFF </font>。<font color=LightSeaGreen>**所有最常见的字符都放在这个平面**，这是 Unicode 最先定义和公布的一个平面</font>。

<font color=FF0000><font size=4>**剩下的字符**</font> 都放在 <font size=4>**辅助平面 ( Supplementary Multilingual Plane, SMP ) **</font></font>，<font color=LightSeaGreen>码点范围从 U+010000 一直到 U+10FFFF</font> 。

##### UTF-32 与 UTF-8

Unicode 只规定了每个字符的码点，到底用什么样的字节序表示这个码点，就涉及到 <font color=FF0000 size=4>**编码方法**</font>。

<font color=FF0000>最直观</font>的编码方法是，<font color=FF0000>每个码点使用四个字节表示</font>，字节内容一一对应码点。<font color=FF0000>这种编码方法就叫做 UTF-32</font>。比如，码点0 就用四个字节的 0 表示，码点 597D 就在前面加两个字节的 0。

```
U+0000 = 0x0000 0000
U+597D = 0x0000 597D
```

<font color=dodgerBlue>UTF-32 的**优点**在于</font>，<font color=red>转换规则简单直观，查找效率高</font>。<font color=dodgerBlue>**缺点**</font>在于<font color=FF0000>浪费空间</font>，同样内容的英语文本，它会比 ASCII 编码大四倍。<font color=LightSeaGreen>这个缺点很致命，导致实际上没有人使用这种编码方法</font>，<font color=FF0000>HTML5 标准就明文规定，网页不得编码成 UTF-32</font>。

人们真正需要的是一种节省空间的编码方法，这导致了 UTF-8 的诞生。**UTF-8 是一种 <font size=4>变长的编码方法，字符长度从 1 个字节到 4 个字节不等</font>。**<font color=FF0000>越是常用的字符，字节越短，最前面的 128 个字符，只使用 1 个字节表示，与 ASCII 码完全相同</font>。

| 编号范围            | 字节 |
| ------------------- | ---- |
| 0x0000 - 0x007F     | 1    |
| 0x0080 - 0x07FF     | 2    |
| 0x0800 - 0xFFFF     | 3    |
| 0x010000 - 0x10FFFF | 4    |

由于UTF-8这种节省空间的特性，导致它成为互联网上最常见的网页编码。

##### UTF-16 简介

UTF-16编码<mark>介于UTF-32与UTF-8之间</mark>，<font color=FF0000>**同时结合了定长和变长两种编码方法的特点**</font>。

它的编码规则很简单：<font color=FF0000>基本平面的字符占用2个字节，辅助平面的字符占用4个字节</font>。<mark>**也就是说，UTF-16 的编码长度要么是 2 个字节 ( U+0000 ~ U+FFFF )，要么是4个字节 ( U+010000 ~ U+10FFFF )。**</mark>

<mark style=background-color:aqua>于是就有一个问题，当我们遇到两个字节，怎么看出它本身是一个字符，还是需要跟其他两个字节放在一起解读？</mark>

说来很巧妙，我也不知道是不是故意的设计，<font color=FF0000 size=4>**在基本平面内，从 U+D800 到 U+DFFF 是一个空段**（共2^12^个），即这些码点不对应任何字符。因此，这个空段可以用来映射辅助平面的字符</font>。

具体来说，<font color=FF0000>辅助平面的字符位共有 2^20^ 个，也就是说，对应这些字符至少需要 20 个二进制位</font>。<font color=FF0000>UTF-16 将这 20 位拆成两半</font>，<font color=FF0000 size=4>**前10位映射在 U+D800 到 U+DBFF （空间大小 2^10^），称为高位 ( H )，后 10 位映射在 U+DC00 到 U+DFFF（空间大小 2^10^），称为低位 ( L )**</font>。这意味着，一个辅助平面的字符，被拆成两个基本平面的字符表示。

**所以，当我们遇到两个字节，发现它的码点在 U+D800 到 U+DBFF 之间，就可以断定，紧跟在后面的两个字节的码点，应该在 U+DC00 到 U+DFFF 之间，这四个字节必须放在一起解读。**

##### UTF-16 的转码公式

Unicode 码点转成 UTF-16 的时候，首先区分这是基本平面字符，还是辅助平面字符。如果是前者，直接将码点转为对应的十六进制形式，长度为两字节。

```
U+597D = 0x597D
```

如果是辅助平面字符，Unicode 3.0 版给出了转码公式。

```js
H = Math.floor((c - 0x10000) / 0x400) + 0xD800
L = (c - 0x10000) % 0x400 + 0xDC00
```

以字符![img](https://i.loli.net/2021/08/23/S9ch8BUR1Q3iWVz.png)为例，它是一个辅助平面字符，码点为 U+1D306 ，将其转为UTF-16的计算过程如下。

```js
H = Math.floor((0x1D306-0x10000)/0x400)+0xD800 = 0xD834
L = (0x1D306-0x10000) % 0x400+0xDC00 = 0xDF06
```

所以，字符![img](https://i.loli.net/2021/08/23/S9ch8BUR1Q3iWVz.png)的UTF-16编码就是0xD834 DF06，长度为四个字节。

##### JavaScript 使用哪一种编码

JavaScript 语言采用 Unicode 字符集，但是只支持一种编码方法。这种编码既不是 UTF-16，也不是 UTF-8，更不是 UTF-32。上面那些编码方法，JavaScript都不用。<font color=FF0000 size=4>**JavaScript 用的是 UCS-2！**</font>

摘自：[阮一峰 - Unicode与JavaScript详解](http://www.ruanyifeng.com/blog/2014/12/unicode.html) 另外，可以参考文章：[淦，为什么 "𠮷𠮷𠮷".length !== 3](https://juejin.cn/post/7025400771982131236)

#### 阮一峰文章《字符编码笔记：ASCII，Unicode 和 UTF-8》笔记

##### UTF-8

UTF-8 最大的一个特点，就是<font color=FF0000> 它是一种变长的编码方式</font>。它<font color=FF0000> 可以使用1~4个字节表示一个符号</font>，根据不同的符号而变化字节长度。

###### UTF-8 的编码规则很简单，只有二条

- <font color=FF0000>对于 **单字节** 的符号，**字节的第一位设为0**（和下面的多字节的对应，用 多少个1 标示是多少个字节）</font>，后面7位为这个符号的 Unicode 码。因此对于英语字母，UTF-8 编码和 ASCII 码是相同的。

- <font color=FF0000> 对于 <font size=4>**n字节**</font> 的符号（n > 1），<font size=4>**第一个字节的前n位都设为1**，**第n + 1位设为0**</font></font>，<font color=FF0000> 后面字节（即剩余的字节） 的前两位一律设为10</font>。<mark>剩下的没有提及的二进制位，全部为这个符号的 Unicode 码</mark>。

下表总结了编码规则，字母`x`表示可用编码的位。

| Unicode符号范围 (十六进制) | UTF-8编码方式（二进制）                                      |
| -------------------------- | ------------------------------------------------------------ |
| 0000 0000 ～ 0000 007F     | 0xxxxxxx                                                     |
| 0000 0080 ～ 0000 07FF     | <font color=FF0000>110</font>xxxxx <font color=FF0000>10</font>xxxxxx |
| 0000 0800 ～ 0000 FFFF     | <font color=FF0000>1110</font>xxxx <font color=FF0000>10</font>xxxxxx <font color=FF0000>10</font>xxxxxx |
| 0001 0000 ～ 0010 FFFF     | <font color=FF0000>11110</font>xxx <font color=FF0000>10</font>xxxxxx <font color=FF0000>10</font>xxxxxx <font color=FF0000>10</font>xxxxxx |

跟据上表，解读 UTF-8 编码非常简单。如果一个字节的第一位是0，则这个字节单独就是一个字符；如果第一位是1，则连续有多少个1，就表示当前字符占用多少个字节。

##### 下面，还是以汉字严为例，演示如何实现 UTF-8 编码

“严”字 的 Unicode 是 4E25 (<mark style="background:LightSlateGray">100</mark><mark style="background:fuchsia">111000</mark><mark style="background:aqua">100101</mark>)，<font color=FF0000>**根据上表**</font>，可以<font color=FF0000>发现 4E25 处在第三行的范围内 (0000 0800 - 0000 FFFF)（即：确定字节数）</font>，<mark>因此 “严” 的 UTF-8 编码需要三个字节，即格式是1110xxxx 10xxxxxx 10xxxxxx</mark>。然后，<font color=FF0000>**从 “严” 的最后一个二进制位开始，依次 <font size=4>从后向前填入</font> 格式中的x，<font size=4>多出的位补0</font>**</font> 。这样就得到了，严的 UTF-8 编码是 <mark>1110</mark><mark style="background:red">0</mark><mark style="background:LightSlateGray">100</mark> <mark>10</mark><mark style="background:fuchsia">111000</mark> <mark>10</mark><mark style="background:aqua">100101</mark>（其中mark为<mark style="background:red">红色</mark>的 0，是补上的 0），转换成十六进制就是 E4B8A5。

##### Little endian 和 Big endian

UCS-2 格式可以存储 Unicode 码（码点不超过 `0xFFFF `）。以汉字 `严` 为例，Unicode 码是 `4E25`，需要用两个字节存储，一个字节是 `4E`，另一个字节是 `25`。存储的时候，`4E` 在前，`25` 在后，这就是 Big endian 方式；`25`在前，`4E` 在后，这是 Little endian 方式。

###### 出现一个问题：计算机怎么知道某一个文件到底采用哪一种方式编码？

Unicode 规范定义，每一个文件的最前面分别加入一个表示编码顺序的字符，这个字符的名字叫做 “零宽度非换行空格” ( zero width no-break space ) ，用 `FEFF` 表示。这正好是两个字节，而且 `FF` 比 `FE` 大 `1`。

<font color=fuchsia>**如果一个文本文件的头两个字节是 `FE FF` ，就表示该文件采用大头方式；如果头两个字节是 `FF FE` ，就表示该文件采用小头方式**</font>。

摘自：[阮一峰 - 字符编码笔记：ASCII，Unicode 和 UTF-8](https://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html)

#### 💡 补充

##### 特殊 （Unicode区段）

> 此条目介绍的是<font color=dodgerBlue>收录缺字字符的 Unicode 区段</font>

特殊字符是 Unicode 的一个简短的区段，分配在基本多文种平面的最末端，位于 U+FFF0-FFFF。在这 16 个码位中，有 5 个是从 Unicode 3.0 开始分配的。

- U+FFF9 ：行间注解锚，标志着注解文本的开始。
- U+FFFA ： 行间注解分隔符，标记注解字符的开始。
- U+FFFB ：行间注解终结者，标志着注解块的结束。
- U+FFFC ￼ ：OBJECT REPLACEMENT CHARACTER，在文本中为另一个未指定的对象提供占位符，例如在一个复合文件中。
- U+FFFD � ：REPLACEMENT CHARACTER（替换字符），用于替换一个未知的、不被认可的或无法表示的字符。
- U+FFFE <非字符-FFFE> 不是一个字符。
- U+FFFF <非字符-FFFF> 不是一个字符。

FFFE 和 FFFF 不是通常意义上的未分配字符，但不是 Unicode 字符。它们可以用来猜测一个文本的编码方案，因为根据定义，任何包含这些的文本都不是一个正确编码的 Unicode 文本。Unicode 的 U+FEFF BYTE ORDER MARK 字符可以插在 Unicode 文本的开头，以表示它的字节性：一个程序在阅读这样的文本并遇到 0xFFFE 时，就会知道它应该为后面的所有字符转换字节顺序。

摘自：[wikipedia - 特殊 (Unicode区段)](https://zh.wikipedia.org/wiki/%E7%89%B9%E6%AE%8A_(Unicode%E5%8D%80%E6%AE%B5))



### HTML 编码

##### 痛点

不是每一个 Unicode 字符都能直接在 HTML 语言里面显示

- 不是每个 Unicode 字符都可以打印出来，有些没有可打印形式，比如换行符的码点是十进制的10（十六进制的A），就没有对应的字面形式。

- 小于号（<）和大于号（>）用来定义 HTML 标签，其他需要用到这两个符号的场合，必须防止它们被解释成标签。

- 由于 Unicode 字符太多，无法找到一种输入法，可以直接输入所有这些字符。换言之，没有一种键盘，有办法输入所有符号。

- 网页不允许混合使用多种编码，如果使用 UTF-8 编码的同时，又想插入其他编码的字符，就会很困难。

HTML 为了解决上面这些问题，<font color=FF0000> **允许使用 Unicode 码点表示字符**</font>，浏览器会自动将码点转成对应的字符。

字符的码点表示法是 &#N;（十进制，N代表码点）或者 &#xN;（十六进制，N代表码点），比如，字符a可以写成a（十进制）或者a（十六进制），字符中可以写成中（十进制）或者中（十六进制），浏览器会自动转换它们。

```html
<p>hello</p>
<!-- 等同于 -->
<p>&#104;&#101;&#108;&#108;&#111;</p>
<!-- 等同于 -->
<p>&#x68;&#x65;&#x6c;&#x6c;&#x6f;</p>
```

> 👀 这里 `&#` 是用来表示 unicode 的

> 使用 HTML，可以直接通过 Unicode 展示字符实体，形式是在 `&#x` 后面加上对应 CodePoint 的十六进制。或者 &#后面加 CodePoint 的十进制数字。
>
> 摘自：[硬核基础编码篇（二）ascii、unicode 和 utf-8](https://juejin.cn/post/7022893476464066596)

> ⚠️ <font color=FF0000> HTML 标签本身不能使用码点表示，否则浏览器会认为这是所要显示的文本内容，而不是标签</font>。比如，`<p>` 一旦写成 `<\&#112;>` 或者 `&#60;&#112;&#62;`，浏览器就不再认为这是标签了，而会当作文本内容将其显示为 `<p>`。

##### 字符的实体表示法

> 💡 更准确的描述是 “字符实体”，可见 [wikipedia - 字符实体引用](https://zh.wikipedia.org/wiki/%E5%AD%97%E7%AC%A6%E5%AE%9E%E4%BD%93%E5%BC%95%E7%94%A8)

<font color=LightSeaGreen>数字表示法的不方便之处，在于必须知道每个字符的码点，很难记忆。为了能够快速输入，HTML 为一些特殊字符，规定了容易记忆的名字，允许通过名字来表示它们，这称为<font color=FF0000> **实体表示法**</font> ( entity )</font>。

实体的写法是 `&name;`，其中的 `name` 是字符的名字。下面是其中一些特殊字符，及其对应的实体。

- <：`&lt;`
- \> :  `&gt;`
- " ：`&quot;`
- ' ：`&apos;`
- & ：`&amp;`
- © ：`&copy;`
- \# ：`&num;`
- § ：`&sect;`
- ¥ ：`&yen;`
- $ ：`&dollar;`
- £ ：`&pound;`
- ¢ ：`&cent;`
- % ：`&percnt;`
- \* ：`&ast;`
- @ ：`&commat;`
- ^ ：`&Hat;`
- ± ：`&plusmn;`
- 空格 ：`&nbsp;`

字符的数字表示法和实体表示法，都可以表示正常情况无法输入的字符，逃脱了浏览器的限制，所以英语里面称为“escape”，中文翻译为“字符的转义”。

摘自：[HTML 字符编码](https://wangdoc.com/html/encode.html)

#### HTML的转义字符

| 特殊符号           | 命名实体   | 十进制编码 | 特殊符号              | 命名实体    | 十进制编码 |
| ------------------ | ---------- | ---------- | --------------------- | ----------- | ---------- |
| <mark>**Α**</mark> | \&Alpha;   | \&#913;    | <mark>**Β**</mark>    | \&Beta;     | \&#914;    |
| <mark>**Γ**</mark> | \&Gamma;   | \&#915;    | <mark>**Δ**</mark>    | \&Delta;    | \&#916;    |
| <mark>**Ε**</mark> | \&Epsilon; | \&#917;    | <mark>**Ζ**</mark>    | \&Zeta;     | \&#918;    |
| <mark>**Η**</mark> | \&Eta;     | \&#919;    | <mark>**Θ**</mark>    | \&Theta;    | \&#920;    |
| <mark>**Ι**</mark> | \&Iota;    | \&#921;    | <mark>**Κ**</mark>    | \&Kappa;    | \&#922;    |
| <mark>**Λ**</mark> | \&Lambda;  | \&#923;    | <mark>**Μ**</mark>    | \&Mu;       | \&#924;    |
| <mark>**Ν**</mark> | \&Nu;      | \&#925;    | <mark>**Ξ**</mark>    | \&Xi;       | \&#926;    |
| <mark>**Ο**</mark> | \&Omicron; | \&#927;    | <mark>**Π**</mark>    | \&Pi;       | \&#928;    |
| <mark>**Ρ**</mark> | \&Rho;     | \&#929;    | <mark>**Σ**</mark>    | \&Sigma;    | \&#931;    |
| <mark>**Τ**</mark> | \&Tau;     | \&#932;    | <mark>**Υ**</mark>    | \&Upsilon;  | \&#933;    |
| <mark>**Φ**</mark> | \&Phi;     | \&#934;    | <mark>**Χ**</mark>    | \&Chi;      | \&#935;    |
| <mark>**Ψ**</mark> | \&Psi;     | \&#936;    | <mark>**Ω**</mark>    | \&Omega;    | \&#937;    |
| <mark>**α**</mark> | \&alpha;   | \&#945;    | <mark>**β**</mark>    | \&beta;     | \&#946;    |
| <mark>**γ**</mark> | \&gamma;   | \&#947;    | <mark>**δ**</mark>    | \&delta;    | \&#948;    |
| <mark>**ε**</mark> | \&epsilon; | \&#949;    | <mark>**ζ**</mark>    | \&zeta;     | \&#950;    |
| <mark>**η**</mark> | \&eta;     | \&#951;    | <mark>**θ**</mark>    | \&theta;    | \&#952;    |
| <mark>**ι**</mark> | \&iota;    | \&#953;    | <mark>**κ**</mark>    | \&kappa;    | \&#954;    |
| <mark>**λ**</mark> | \&lambda;  | \&#955;    | <mark>**μ**</mark>    | \&mu;       | \&#956;    |
| <mark>**ν**</mark> | \&nu;      | \&#957;    | <mark>**ξ**</mark>    | \&xi;       | \&#958;    |
| <mark>**ο**</mark> | \&omicron; | \&#959;    | <mark>**π**</mark>    | \&pi;       | \&#960;    |
| <mark>**ρ**</mark> | \&rho;     | \&#961;    | <mark>**ς**</mark>    | \&sigmaf;   | \&#962;    |
| <mark>**σ**</mark> | \&sigma;   | \&#963;    | <mark>**τ**</mark>    | \&tau;      | \&#964;    |
| <mark>**υ**</mark> | \&upsilon; | \&#965;    | <mark>**φ**</mark>    | \&phi;      | \&#966;    |
| <mark>**χ**</mark> | \&chi;     | \&#967;    | <mark>**ψ**</mark>    | \&psi;      | \&#968;    |
| <mark>**ω**</mark> | \&omega;   | \&#969;    | <mark>**ϑ**</mark>    | \&thetasym; | \&#977;    |
| <mark>**ϒ**</mark> | \&upsih;   | \&#978;    | <mark>**ϖ**</mark>    | \&piv;      | \&#982;    |
| <mark>**•**</mark> | \&bull;    | \&#8226;   | <mark>**…**</mark>    | \&hellip;   | \&#8230;   |
| <mark>**′**</mark> | \&prime;   | \&#8242;   | <mark>**″**</mark>    | \&Prime;    | \&#8243;   |
| <mark>**‾**</mark> | \&oline;   | \&#8254;   | <mark>**⁄**</mark>    | \&frasl;    | \&#8260;   |
| <mark>**℘**</mark> | \&weierp;  | \&#8472;   | <mark>**ℑ**</mark>    | \&image;    | \&#8465;   |
| <mark>**ℜ**</mark> | \&real;    | \&#8476;   | <mark>**™**</mark>    | \&trade;    | \&#8482;   |
| <mark>**ℵ**</mark> | \&alefsym; | \&#8501;   | <mark>**←**</mark>    | \&larr;     | \&#8592;   |
| <mark>**↑**</mark> | \&uarr;    | \&#8593;   | <mark>**→**</mark>    | \&rarr;     | \&#8594;   |
| <mark>**↓**</mark> | \&darr;    | \&#8595;   | <mark>**↔**</mark>    | \&harr;     | \&#8596;   |
| <mark>**↵**</mark> | \&crarr;   | \&#8629;   | <mark>**⇐**</mark>    | \&lArr;     | \&#8656;   |
| <mark>**⇑**</mark> | \&uArr;    | \&#8657;   | <mark>**⇒**</mark>    | \&rArr;     | \&#8658;   |
| <mark>**⇓**</mark> | \&dArr;    | \&#8659;   | <mark>**⇔**</mark>    | \&hArr;     | \&#8660;   |
| <mark>**∀**</mark> | \&forall;  | \&#8704;   | <mark>**∂**</mark>    | \&part;     | \&#8706;   |
| <mark>**∃**</mark> | \&exist;   | \&#8707;   | <mark>**∅**</mark>    | \&empty;    | \&#8709;   |
| <mark>**∇**</mark> | \&nabla;   | \&#8711;   | <mark>**∈**</mark>    | \&isin;     | \&#8712;   |
| <mark>**∉**</mark> | \&notin;   | \&#8713;   | <mark>**∋**</mark>    | \&ni;       | \&#8715;   |
| <mark>**∏**</mark> | \&prod;    | \&#8719;   | <mark>**∑**</mark>    | \&sum;      | \&#8722;   |
| <mark>**−**</mark> | \&minus;   | \&#8722;   | <mark>**∗**</mark>    | \&lowast;   | \&#8727;   |
| <mark>**√**</mark> | \&radic;   | \&#8730;   | <mark>**∝**</mark>    | \&prop;     | \&#8733;   |
| <mark>**∞**</mark> | \&infin;   | \&#8734;   | <mark>**∠**</mark>    | \&ang;      | \&#8736;   |
| <mark>**∧**</mark> | \&and;     | \&#8869;   | <mark>**∨**</mark>    | \&or;       | \&#8870;   |
| <mark>**∩**</mark> | \&cap;     | \&#8745;   | <mark>**∪**</mark>    | \&cup;      | \&#8746;   |
| <mark>**∫**</mark> | \&int;     | \&#8747;   | <mark>**∴**</mark>    | \&there4;   | \&#8756;   |
| <mark>**∼**</mark> | \&sim;     | \&#8764;   | <mark>**≅**</mark>    | \&cong;     | \&#8773;   |
| <mark>**≈**</mark> | \&asymp;   | \&#8773;   | <mark>**≠**</mark>    | \&ne;       | \&#8800;   |
| <mark>**≡**</mark> | \&equiv;   | \&#8801;   | <mark>**≤**</mark>    | \&le;       | \&#8804;   |
| <mark>**≥**</mark> | \&ge;      | \&#8805;   | <mark>**⊂**</mark>    | \&sub;      | \&#8834;   |
| <mark>**⊃**</mark> | \&sup;     | \&#8835;   | <mark>**⊄**</mark>    | \&nsub;     | \&#8836;   |
| <mark>**⊆**</mark> | \&sube;    | \&#8838;   | <mark>**⊇**</mark>    | \&supe;     | \&#8839;   |
| <mark>**⊕**</mark> | \&oplus;   | \&#8853;   | <mark>**⊗**</mark>    | \&otimes;   | \&#8855;   |
| <mark>**⊥**</mark> | \&perp;    | \&#8869;   | <mark>**⋅**</mark>    | \&sdot;     | \&#8901;   |
| <mark>**⌈**</mark> | \&lceil;   | \&#8968;   | <mark>**⌉**</mark>    | \&rceil;    | \&#8969;   |
| <mark>**⌊**</mark> | \&lfloor;  | \&#8970;   | <mark>**⌋**</mark>    | \&rfloor;   | \&#8971;   |
| <mark>**◊**</mark> | \&loz;     | \&#9674;   | <mark>**♠**</mark>    | \&spades;   | \&#9824;   |
| <mark>**♣**</mark> | \&clubs;   | \&#9827;   | <mark>**♥**</mark>    | \&hearts;   | \&#9829;   |
| <mark>**♦**</mark> | \&diams;   | \&#9830;   | <mark>**空格**</mark> | \&nbsp;     | \&#160;    |
| <mark>**¡**</mark> | \&iexcl;   | \&#161;    | <mark>**¢**</mark>    | \&cent;     | \&#162;    |
| <mark>**£**</mark> | \&pound;   | \&#163;    | <mark>**¤**</mark>    | \&curren;   | \&#164;    |
| <mark>**¥**</mark> | \&yen;     | \&#165;    | <mark>**¦**</mark>    | \&brvbar;   | \&#166;    |
| <mark>**§**</mark> | \&sect;    | \&#167;    | <mark>**¨**</mark>    | \&uml;      | \&#168;    |
| <mark>**©**</mark> | \&copy;    | \&#169;    | <mark>**ª**</mark>    | \&ordf;     | \&#170;    |
| <mark>**«**</mark> | \&laquo;   | \&#171;    | <mark>**¬**</mark>    | \&not;      | \&#172;    |
| <mark>**­**</mark> | \&shy;     | \&#173;    | <mark>**®**</mark>    | \&reg;      | \&#174;    |
| <mark>**¯**</mark> | \&macr;    | \&#175;    | <mark>**°**</mark>    | \&deg;      | \&#176;    |
| <mark>**±**</mark> | \&plusmn;  | \&#177;    | <mark>**²**</mark>    | \&sup2;     | \&#178;    |
| <mark>**³**</mark> | \&sup3;    | \&#179;    | <mark>**´**</mark>    | \&acute;    | \&#180;    |
| <mark>**µ**</mark> | \&micro;   | \&#181;    |                       |             |            |

| HTML 原代码 | 显示结果 | 描述                   |
| ----------- | -------- | ---------------------- |
| \&lt;       | <        | 小于号或显示标记       |
| \&gt;       | >        | 大于号或显示标记       |
| \&amp;      | &        | 可用于显示其它特殊字符 |
| \&quot;     | “        | 引号                   |
| \&reg;      | ®        | 已注册                 |
| \&copy;     | ©        | 版权                   |
| \&trade;    | ™        | 商标                   |
| \&ensp;     |          | 半个空白位             |
| \&emsp;     |          | 一个空白位             |
| \&nbsp;     |          | 不断行的空白           |

| 符号               | 源代码     | 符号               | 源代码    | 符号               | 源代码     | 符号               | 源代码    | 符号               | 源代码     |
| ------------------ | ---------- | ------------------ | --------- | ------------------ | ---------- | ------------------ | --------- | ------------------ | ---------- |
| <mark>**´**</mark> | \&acute;   | <mark>**©**</mark> | \&copy;   | <mark>**>**</mark> | \&gt;      | <mark>**µ**</mark> | \&micro;  | <mark>**®**</mark> | \&reg;     |
| <mark>**&**</mark> | \&amp;     | <mark>**°**</mark> | \&deg;    | <mark>**¡**</mark> | \&iexcl;   | <mark>** **</mark> | \&nbsp;   | <mark>**»**</mark> | \&raquo;   |
| <mark>**¦**</mark> | \&brvbar;  | <mark>**÷**</mark> | \&divide; | <mark>**¿**</mark> | \&iquest;  | <mark>**¬**</mark> | \&not;    | <mark>**§**</mark> | \&sect;    |
| <mark>**•**</mark> | \&bull;    | <mark>**½**</mark> | \&frac12; | <mark>**«**</mark> | \&laquo;   | <mark>**¶**</mark> | \&para;   | <mark>**¨**</mark> | \&uml;     |
| <mark>**¸**</mark> | \&cedil;   | <mark>**¼**</mark> | \&frac14; | <mark>**<**</mark> | \&lt;      | <mark>**±**</mark> | \&plusmn; | <mark>**×**</mark> | \&times;   |
| <mark>**¢**</mark> | \&cent;    | <mark>**¾**</mark> | \&frac34; | <mark>**¯**</mark> | \&macr;    | <mark>**“**</mark> | \&quot;   | <mark>**™**</mark> | \&trade;   |
| <mark>**€**</mark> | \&euro;    | <mark>**£**</mark> | \&pound;  | <mark>**¥**</mark> | \&yen;     |                    |           |                    |            |
| <mark>**„**</mark> | \&bdquo;   | <mark>**…**</mark> | \&hellip; | <mark>**·**</mark> | \&middot;  | <mark>**›**</mark> | \&rsaquo; | <mark>**ª**</mark> | \&ordf;    |
| <mark>**ˆ**</mark> | \&circ;    | <mark>**“**</mark> | \&ldquo;  | <mark>**—**</mark> | \&mdash;   | <mark>**’**</mark> | \&rsquo;  | <mark>**º**</mark> | \&ordm;    |
| <mark>**†**</mark> | \&dagger;  | <mark>**‹**</mark> | \&lsaquo; | <mark>**–**</mark> | \&ndash;   | <mark>**‚**</mark> | \&sbquo;  | <mark>**”**</mark> | \&rdquo;   |
| <mark>**‡**</mark> | \&Dagger;  | <mark>**‘**</mark> | \&lsquo;  | <mark>**‰**</mark> | \&permil;  | <mark>** **</mark> | \&shy;    | <mark>**˜**</mark> | \&tilde;   |
| <mark>**≈**</mark> | \&asymp;   | <mark>**⁄**</mark> | \&frasl;  | <mark>**←**</mark> | \&larr;    | <mark>**∂**</mark> | \&part;   | <mark>**♠**</mark> | \&spades;  |
| <mark>**∩**</mark> | \&cap;     | <mark>**≥**</mark> | \&ge;     | <mark>**≤**</mark> | \&le;      | <mark>**″**</mark> | \&Prime;  | <mark>**∑**</mark> | \&sum;     |
| <mark>**♣**</mark> | \&clubs;   | <mark>**↔**</mark> | \&harr;   | <mark>**◊**</mark> | \&loz;     | <mark>**′**</mark> | \&prime;  | <mark>**↑**</mark> | \&uarr;    |
| <mark>**↓**</mark> | \&darr;    | <mark>**♥**</mark> | \&hearts; | <mark>**−**</mark> | \&minus;   | <mark>**∏**</mark> | \&prod;   | <mark>**‍**</mark>  | \&zwj;     |
| <mark>**♦**</mark> | \&diams;   | <mark>**∞**</mark> | \&infin;  | <mark>**≠**</mark> | \&ne;      | <mark>**√**</mark> | \&radic;  | <mark>**‌**</mark>  | \&zwnj;    |
| <mark>**≡**</mark> | \&equiv;   | <mark>**∫**</mark> | \&int;    | <mark>**‾**</mark> | \&oline;   | <mark>**→**</mark> | \&rarr;   |                    |            |
| <mark>**α**</mark> | \&alpha;   | <mark>**η**</mark> | \&eta;    | <mark>**μ**</mark> | \&mu;      | <mark>**π**</mark> | \&pi;     | <mark>**θ**</mark> | \&theta;   |
| <mark>**β**</mark> | \&beta;    | <mark>**γ**</mark> | \&gamma;  | <mark>**ν**</mark> | \&nu;      | <mark>**ψ**</mark> | \&psi;    | <mark>**υ**</mark> | \&upsilon; |
| <mark>**χ**</mark> | \&chi;     | <mark>**ι**</mark> | \&iota;   | <mark>**ω**</mark> | \&omega;   | <mark>**ρ**</mark> | \&rho;    | <mark>**ξ**</mark> | \&xi;      |
| <mark>**δ**</mark> | \&delta;   | <mark>**κ**</mark> | \&kappa;  | <mark>**ο**</mark> | \&omicron; | <mark>**σ**</mark> | \&sigma;  | <mark>**ζ**</mark> | \&zeta;    |
| <mark>**ε**</mark> | \&epsilon; | <mark>**λ**</mark> | \&lambda; | <mark>**φ**</mark> | \&phi;     | <mark>**τ**</mark> | \&tau;    | <mark>** **</mark> | \          |
| <mark>**Α**</mark> | \&Alpha;   | <mark>**Η**</mark> | \&Eta;    | <mark>**Μ**</mark> | \&Mu;      | <mark>**Π**</mark> | \&Pi;     | <mark>**Θ**</mark> | \&Theta;   |
| <mark>**Β**</mark> | \&Beta;    | <mark>**Γ**</mark> | \&Gamma;  | <mark>**Ν**</mark> | \&Nu;      | <mark>**Ψ**</mark> | \&Psi;    | <mark>**Υ**</mark> | \&Upsilon; |
| <mark>**Χ**</mark> | \&Chi;     | <mark>**Ι**</mark> | \&Iota;   | <mark>**Ω**</mark> | \&Omega;   | <mark>**Ρ**</mark> | \&Rho;    | <mark>**Ξ**</mark> | \&Xi;      |
| <mark>**Δ**</mark> | \&Delta;   | <mark>**Κ**</mark> | \&Kappa;  | <mark>**Ο**</mark> | \&Omicron; | <mark>**Σ**</mark> | \&Sigma;  | <mark>**Ζ**</mark> | \&Zeta;    |
| <mark>**Ε**</mark> | \&Epsilon; | <mark>**Λ**</mark> | \&Lambda; | <mark>**Φ**</mark> | \&Phi;     | <mark>**Τ**</mark> | \&Tau;    | <mark>**ς**</mark> | \&sigmaf;  |

摘自：[Html 特殊符号](https://www.cnblogs.com/knowledgesea/p/3210703.html)



### 百分号编码 / URL编码

百分号编码（英语：Percent-encoding ），<font color=FF0000> 又称：URL编码（URL encoding）是特定上下文的统一资源定位符 （URL）的编码机制</font>，实际上也适用于统一资源标志符 ( URI ) 的编码。<font color=LightSeaGreen>也用于为 application/x-www-form-urlencoded（的）MIME 准备数据</font>，因为它用于通过HTTP 的请求操作 ( request ) 提交 HTML表单数据。

##### URI的字符类型

URI 所允许的字符分作 **保留** 与 **未保留** 。<font color=FF0000> **保留**字符 是 **那些具有特殊含义的字符**</font>，例如：斜线字符用于URL（或URI）不同部分的分界符；<font color=FF0000> **未保留**字符 没有这些特殊含义</font>。<font color=FF0000> 百分号编码 把保留字符表示为特殊字符序列</font>。上述情形随 URI 与 URI 的不同版本规格会有轻微的变化。

RFC 3986 section 2.2 <font color=dodgerBlue>保留字符</font> （ 2005年1月 ），如下：

| !    | *    | '    | (    | )    | ;    | :    | @    | &    | =    | +    | $    | ,    | /    | ?    | #    | [    | ]    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |

RFC 3986 section 2.3 <font color=dodgerBlue>未保留字符</font> (2005年1月)，如下：

| A    | B    | C    | D    | E    | F    | G    | H    | I    | J    | K    | L    | M    | N    | O    | P    | Q    | R    | S    | T    | U    | V    | W    | X    | Y    | Z    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| a    | b    | c    | d    | e    | f    | g    | h    | i    | j    | k    | l    | m    | n    | o    | p    | q    | r    | s    | t    | u    | v    | w    | x    | y    | z    |
| 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | -    | _    | .    | ~    |      |      |      |      |      |      |      |      |      |      |      |      |

<font color=FF0000> URI中的其它字符（比如汉字）必须用百分号编码</font>。

##### 对保留字符的百分号编码

如果一个 保留字符 在特定上下文中具有特殊含义（ 称作 "reserved purpose" ），且 URI 中必须使用该字符用于其它目的，那么该字符必须百分号编码。<font color=red>百分号编码一个保留字符</font>，<font color=dodgerBlue>**首先**</font> 需要 <font color=fuchsia>把该字符的 ASCII 的值表示为 <font size=4>**两个16进制的数字**</font></font>，<font color=dodgerBlue>**然后**</font> <font color=red>在其前面放置转义字符 ("`%`")</font> ，置入 URI 中的相应位置。（ 对于 非ASCII 字符，需要转换为 UTF-8 字节序，然后每个字节按照上述方式表示）

例如，"`/`" ， 如果用作 URI 的路径成份的分界符，则是具有特殊含义的保留字符。如果该字符需要出现在 URI 一个路径成分的内部，则三字符序列 "`%2F`" 或 "`%2f`" 就用于代替原本的 "`/`" 出现在该 URI 路径成分的内部.

| !    | #    | $    | &    | '    | (    | )    | *    | +    | ,    | /    | :    | ;    | =    | ?    | @    | [    | ]    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| %21  | %23  | %24  | %26  | %27  | %28  | %29  | %2A  | %2B  | %2C  | %2F  | %3A  | %3B  | %3D  | %3F  | %40  | %5B  | %5D  |

在特定上下文中没有特殊含义的保留字符也可以被百分号编码，在语义上与不百分号编码的该字符没有差别。

在 URI 的 “查询” 成分（ `?` 字符后的部分）中， 例如 "`/`" 仍然是保留字符但是没有特殊含义，除非一个特定的 URI 有其它规定。该 `/` 字符在没有特殊含义时不需要百分号编码。

如果保留字符具有特殊含义，那么该保留字符用百分号编码的 URI 与该保留字符仅用其自身表示的 URI 具有不同的语义。

摘自：[wiki - 百分号编码](https://zh.wikipedia.org/wiki/%E7%99%BE%E5%88%86%E5%8F%B7%E7%BC%96%E7%A0%81)

<font color=red>不同的操作系统、不同的浏览器、不同的网页字符集，如果让浏览器自动进行 URL编码 会导致编码结果完全不同</font>，<font color=fuchsia>因为使用了不同的编码方式</font>（详见引用链接）。

为了预防这种问题，应该使用 Javascript 先对 URL 编码，然后再向服务器提交，不要给浏览器插手的机会。**有三种方法：**

- **escape()** ：不推荐使用
- **encodeURI()** ：只能整体编码
- **encodeURIComponent()** ：与 `encodeURI()` 的区别是，它用于对URL的组成部分进行个别编码，而不用于对整个URL进行编码。

> 👀 补充：由于这篇文章2010年的，很多东西与当前（2021年）不符了。在Chrome中浏览器自动的URL编码采用 UTF-8编码方式。

摘自：[阮一峰 - 关于URL编码](https://www.ruanyifeng.com/blog/2010/02/url_encoding.html)

百分号编码又叫做URL编码，<mark>是一种编码机制</mark>，<font color=FF0000>只要用于URI（包含 URL 和 URN ）编码中</font>。

##### URL 是什么

 URL ( Uniform Resource Locator ) ，统一资源定位符，<font color=FF0000>是地址的别名</font>。<font color=FF0000>包含了关于文件储存位置和浏览器应该如何处理他的信息</font>。互联网上的每一个文件都有唯一的 URL

##### URL 分为三个部分

- 第一部分模式

- 第二部分文件所在的主机名称

- 第三部分路径（目录+文件名）

一般来说，URL只能使用英文字母、阿拉伯数字和某些标点符号，不能使用其他的文字和符号

URL只能使用 ASCII 字符集来通过网络进行发送。由于URL经常包含ASCII 码之外的字符，所以必须将 URL 转换为有效的ASCII 码的格式

<mark>URL编码通常会使用 % 后面跟随两个十六进制数字来替换 非ASCII 字符</mark>；

<font color=FF0000>URL不能包含空格。所以进行编码时经常使用 + 来替换空格</font>；

##### 为什么会需要编码

那是因为这样东西不适合传输。<font color=FF0000>原因可能有很多种：大小过大，包含隐私数据</font>。<font color=FF0000>对于URL而言，之所以进行编码是因为URL中有一些字符会引起歧义</font>。

**例如：**URL 参数字符串中使用键值对这样的形式来传参，键值对之间使用了 & 符号分隔。

如果 value字符串中包含了 @ 或 & 等字符，那么一定会造成接收URL的服务器解析错误，因此就需要对造成歧义的 @ 和 & 进行转义，对其进行编码。

还有，如果URL的字符集使用的ASCII 码，不是 Unicode，这就意味着不可以在 URL 中包含任何的非 ASCII 码，例如：中文，否则客户端浏览器和服务器设定的字符集不同的情况下，输入的中文就会出现乱码。

百分号编码（ URL 编码）会对 URL 不允许出现的字符或者其他特殊情况的允许的字符进行编码，对于被编码的字符，最终会转为百分号 % 开头，后面跟这昂个十六进制数字的形式。例如：空格 ( SP) 是不允许的字符，在 ACSII 码中对应的的二进制值是 00100000 ，最终转换为 %20

<font color=FF0000>URL编码的原则就是**使用安全的字符**（没有特殊用途或者特殊意义的可打印字符）**去掉那些不安全的字符**，**以保证内容的正常显示**</font>。

##### 需要编码的字符

RFC3986文档中规定，URL中只需要包含英文字母（a\~zA\~Z）、数字（0~9）、（- \_ . ~）四个特殊字符以及所有保留字符。

URL经常包含ASCII 码之外的字符，所以必须将 URL 转换为有效的ASCII 码的格式。

有一些字符需要经过编码才不会引起URL语义的转变



### Base64

##### Base64索引表

| 数值 | <mark>字符</mark> | 数值 | <mark>字符</mark> | 数值 | <mark>字符</mark> | 数值 | <mark>字符</mark> |
| :--: | :---------------: | :--: | :---------------: | :--: | :---------------: | :--: | :---------------: |
|  0   |         A         |  16  |         Q         |  32  |         g         |  48  |         w         |
|  1   |         B         |  17  |         R         |  33  |         h         |  49  |         x         |
|  2   |         C         |  18  |         S         |  34  |         i         |  50  |         y         |
|  3   |         D         |  19  |         T         |  35  |         j         |  51  |         z         |
|  4   |         E         |  20  |         U         |  36  |         k         |  52  |         0         |
|  5   |         F         |  21  |         V         |  37  |         l         |  53  |         1         |
|  6   |         G         |  22  |         W         |  38  |         m         |  54  |         2         |
|  7   |         H         |  23  |         X         |  39  |         n         |  55  |         3         |
|  8   |         I         |  24  |         Y         |  40  |         o         |  56  |         4         |
|  9   |         J         |  25  |         Z         |  41  |         p         |  57  |         5         |
|  10  |         K         |  26  |         a         |  42  |         q         |  58  |         6         |
|  11  |         L         |  27  |         b         |  43  |         r         |  59  |         7         |
|  12  |         M         |  28  |         c         |  44  |         s         |  60  |         8         |
|  13  |         N         |  29  |         d         |  45  |         t         |  61  |         9         |
|  14  |         O         |  30  |         e         |  46  |         u         |  62  |         +         |
|  15  |         P         |  31  |         f         |  47  |         v         |  63  |         /         |

摘自：[wiki - Base64](https://zh.wikipedia.org/wiki/Base64)

所谓Base64，就是说选出64个字符----小写字母a-z、大写字母A-Z、数字0-9、符号"+"、"/"（再加上作为垫字的"="，实际上是65个字符）----作为一个基本字符集。然后，其他所有符号都转换成这个字符集中的字符。

##### 具体来说，转换方式可以分为四步

- **第一步**，<font color=FF0000>**将每三个字节作为一组**，一共是24个二进制位</font>。
- **第二步**，<font color=FF0000>**将这24个二进制位分为四组**，每个组有6个二进制位</font>。
- **第三步**，<font color=FF0000>在每组前面加两个00，扩展成32个二进制位，即四个字节</font>。
- **第四步**，<font color=FF0000>**根据索引表**，得到扩展后的每个字节的对应符号</font>，这就是Base64的编码值。

因为，Base64 将三个字节转化成四个字节，因此 Base64 编码后的文本，会比原文本大出三分之一左右。

##### 举一个具体的实例，演示英语单词Man如何转成Base64编码

![image-20210823164828151](https://i.loli.net/2021/08/23/nfjBv3gHG7YpwP5.png)

- **第一步**，"M"、"a"、"n"的ASCII值分别是77、97、110，对应的二进制值是01001101、01100001、01101110，将它们连成一个24位的二进制字符串010011010110000101101110。

- **第二步**，将这个24位的二进制字符串分成4组，每组6个二进制位：010011、010110、000101、101110。

- **第三步**，在每组前面加两个00，扩展成32个二进制位，即四个字节：00010011、00010110、00000101、00101110。它们的十进制值分别是19、22、5、46。

- 第四步，根据上表，得到每个值对应Base64编码，即T、W、F、u。

##### 如果字节数不足三，则这样处理

- **二个字节的情况：**将这<font color=FF0000>**二个字节**</font>的一共16个二进制位，按照上面的规则，<font color=FF0000>**转成三组**</font>，<font color=FF0000>最后一组除了前面加两个0以外，**后面也要加两个0**</font>。这样得到一个三位的Base64编码，<font color=FF0000>**再在末尾补上<font size=4>一个"="号</font>**</font>。

  比如，"Ma"这个字符串是两个字节，可以转化成三组00010011、00010110、00010000以后，对应Base64值分别为T、W、E，再补上一个"="号，因此<font color=FF0000>"Ma"的Base64编码就是TWE=</font>。

- **一个字节的情况：**将这<font color=FF0000>**一个字节**</font>的8个二进制位，按照上面的规则<font color=FF0000>**转成二组**</font>，最后一组除了前面加二个0以外，<font color=FF0000>**后面再加4个0**</font>。这样得到一个二位的Base64编码，<font color=FF0000>**再在末尾补上<font size=4>两个"="号</font>**</font>。

比如，"M"这个字母是一个字节，可以转化为二组00010011、00010000，对应的Base64值分别为T、Q，再补上二个"="号，因此"M"的Base64编码就是TQ==。

摘自：[阮一峰 - Base64笔记](https://www.ruanyifeng.com/blog/2008/06/base64.html)

另外，也可以看视频：[原理到实现 | 一个视频完全掌握base64编码](https://www.bilibili.com/video/BV1Wt4y1q7dH)

##### 为什么要使用 Base64 ？

我们知道在计算机中的字节共有 256 个组合，对应就是 ascii 码，而 <font color=LightSeaGreen>ascii 码的 128～255 之间的值是不可见字符</font>。而<font color=FF0000> 在网络上交换数据时，比如说从 A地 传到 B地，往往要经过多个路由设备，由于 <font size=4>**不同的设备对字符的处理方式有一些不同，这样那些不可见字符就有可能被处理错误，这是不利于传输的**</font></font>。**所以就先把数据先做一个 Base64 编码，统统变成可见字符，这样出错的可能性就大降低了**。👀 更直白易懂的说法是：Base64主要在做兼容不同规格的网路设备，如下：

> 某些二进制值，在一些硬件上，比如在不同的路由器，老电脑上，表示的意义不一样，做的处理也不一样。同样，一些老的软件，网络协议（👀 详见下面引用）也有类似的问题。
>
> > SMTP协议，等很多比较老的协议只支持纯文本的
> >
> > 摘自：[为什么要使用base64编码，有哪些情景需求？ - Ted Zyzsdy的回答 - 知乎](https://www.zhihu.com/question/36306744/answer/67312483)
>
> 摘自：[为什么要使用base64编码，有哪些情景需求？ - Wang Kai的回答 - 知乎](https://www.zhihu.com/question/36306744/answer/673975520)

对证书来说，特别是根证书，一般都是作 Base64 编码的，因为它要在网上被许多人下载（👀 感觉因为：面向大量用户，大量的设备，对兼容性起到了要求；需要 base64 提供支持）。电子邮件的附件一般也作 Base64 编码的，因为一个附件数据往往是有不可见字符的（ 👀 关于“不可见字符” 的说法，更具体的是：**控制字符** 和 **二进制流** （图片视频文件））

摘自：[为什么要使用base64编码，有哪些情景需求？ - wuxinliulei的回答 - 知乎](https://www.zhihu.com/question/36306744/answer/71626823)




### 其他补充

#### # 前端开发中的大小写敏感问题

// TODO

摘自：[前端开发中的大小写敏感问题](https://zhuanlan.zhihu.com/p/545504140)

## 浏览器运行相关

#### 浏览器访问网站的工作流程

- 输入 URL 之后，浏览器会进行 DNS查找，找到 URL 所在服务器的域名

  > 💡 在进行 DNS 解析 之前，浏览器会先判断是否需要重定向和缓存检查，缓存涉及到强缓存和协商缓存。
  >
  > 具体参考 [[前端面试点总结#在浏览器中输入 URL 并且按下回车之后发生了什么]] 中的第二步

- 找到服务器之后，浏览器会通过 TCP 握手与服务器建立连接。如果是基于 HTTPS 的连接，会多一步<font color=FF0000 size=4> **TLS握手**</font>，建立加密的隧道，保证数据不被监听和篡改。

- 在建立连接之后，浏览器会发送 HTTP / HTTPS 请求，一般服务器的响应就是 html 的网页代码。

  - 浏览器在接收服务器响应时，由于 TCP 的慢启动 ( slow start ) 的机制，浏览器会先收到前 14kb 的数据；后面才会慢慢增加传输速度（ 这里的 14kb 衍生出了“首屏优化，首屏的资源要小于 14kb ”的限制 ）

    > 💡 关于 14kb 相关的内容，在 Google Chrome 的网站 web.dev 中有 [文章](https://web.dev/i18n/zh/extract-critical-css/) 提及，也说明了 “为什么是 14KB ”：
    >
    > > 新的 [TCP](https://hpbn.co/building-blocks-of-tcp/) 连接无法立即利用客户端和服务器之间的全部可用带宽，这些连接会经过[慢启动](https://hpbn.co/building-blocks-of-tcp/#slow-start)以避免数据量超过连接的承载能力。在这个过程中，服务器会先开始传输少量数据，如果数据以完美的状态到达客户端，那么下一次往返中数据量会加倍。<font color=FF0000>对于大多数服务器，第一次往返最多可以传输 10 个数据包或大约 14 KB</font>。
    > >
    > > > 💡 关于 TCP 数据包的大小：
    > > >
    > > > > TCP 数据包的最大大小为`1500 byte` 这个最大值不是由 TCP 规范设置的，它来自[以太网标准](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Ethernet_frame)）
    > > > >
    > > > > 每个 TCP 数据包在其标头中使用 40 个字节，16 个字节用于 IP，另外 24 个字节用于 TCP
    > > > >
    > > > > 每个 TCP 数据包剩下1460 字节，`10 x 1460 = 14600 bytes` ( 👀 14.25kB )或大约 14kB
    > > > >
    > > > > 摘自：[为什么开发的网页不应该大于 14KB？ - 艾康麦icon-meh的回答 - 知乎](https://www.zhihu.com/question/597403140/answer/3004518358) 
    > > >
    > > > 另外，文章中提及了 14kB 可以是压缩后（比如 gzip）的数据，原数据可能有 50kB。另外，[为什么开发的网页不应该大于 14KB？ - NaHCO3的回答 - 知乎](https://www.zhihu.com/question/597403140/answer/3009158641) 的评论区还提及：考虑缓存策略，减少请求数据，也是一种对 14kB 的优化
    > >
    > > 摘自：[提取关键 CSS (Critical CSS)](https://web.dev/i18n/zh/extract-critical-css/)

- 在收到 html 文件之后，浏览器开始渲染网页，共有五个步骤，被称为：<font color=FF0000 size=4> **关键渲染路径**</font>


##### 关键渲染路径

###### 构建 DOM ( Document Object Model ) 树

- 浏览器解析 HTML、构建DOM 树是顺序执行的（从上到下），并且<font color=fuchsia>只有一个 **主线程** 负责解析</font>（ 👀 防止冲突）

- <font color=red>如果在解析的过程中遇到 `<script>` 标签，浏览器会加载 JS 文件，并执行里面的代码</font>；这时候 <font color=fuchsia>**主线程会暂停解析 HTML，直到 JS 代码执行完毕，才会继续**</font>（👀 因为 script 会产生/修改 DOM 元素）

- 对于<font color=fuchsia>**图片、CSS文件、rel 属性设置为 `defer` 、`async` 的 `<script>` 标签**</font>，将<font color=fuchsia>不会影响主线程</font>；而是<font color=fuchsia>会异步的加载</font>

- 浏览器有一个 <font color=FF0000>**预扫描线程**</font> ( Pre Scanner / Preload Scanner ？) ，会扫描 HTML 代码，预先下载 CSS文件、字体、JS代码

  > 👀 和 `rel="preload"` / `rel="prefetch"` 相关？

<img src="https://i.loli.net/2021/10/19/k1zobO8ZBEnAVcx.png" alt="image-20211019182116485" style="zoom:40%;" />

###### 构建 CSSOM ( CSS Object Model ) 树

<img src="https://i.loli.net/2021/10/19/Lzy9VXM2JuYEeIF.png" alt="image-20211019180750859" style="zoom: 40%;" />

CSSOM树 是 CSS 在浏览器中的对象表示，也是树状结构

###### 合并 DOM 树 和 CSSOM 树（形成渲染树）

浏览器会从 DOM 的根节点开始，合并 CSSOM 中的样式到 DOM 中的每个节点，形成一棵渲染树 ( Render Tree )，<font color=fuchsia>渲染树的节点被称为 ***渲染对象***</font>

<img src="https://i.loli.net/2021/10/19/oAuSFHbZWYXcajT.png" alt="image-20211019182315113" style="zoom:40%;" />

###### 布局 Layout

生成渲染树之后，<font color=fuchsia>浏览器会根据样式计算每个可见节点的宽高和位置等，对每个节点进行布局规划</font>。<font color=fuchsia>**对于像图片这样的节点**</font>（置换元素？）<font color=fuchsia>**如果没有指定宽高，浏览器会先忽略它的大小**</font>（如下图）

<img src="https://i.loli.net/2021/10/19/qV5uGPlobEwmk2M.png" alt="image-20211019182426724" style="zoom:40%;" />

在图片加载完成之后，浏览器会根据图片的宽高和位置，<font color=FF0000 size=4> **再次计算受影响的节点的大小和位置**</font>，这个过程被称为<font color=FF0000 size=4> **回流 (reflow)**</font>。如下图：

<img src="https://i.loli.net/2021/10/19/mPIbkxRulrdTKCE.png" alt="image-20211019182604357" style="zoom:40%;" />

可见节点即：没有设置display: none 的节点。

###### 绘制 Paint

在第一次布局完成之后，浏览器会真正将节点和节点的样式绘制到屏幕上。这个过程要求十分快速，否则会影响动画和交互的性能。如果之前发生了回流，浏览器还会发生<font color=FF0000> **重绘**</font>，将变化的布局重新绘制到屏幕上。在绘制期间，也有可能会有 <font color=FF0000> 组合</font>发生。在渲染节点时，可能会产生新的图层，比如：\<video />、opacity、will-change、transform 等属性的节点，浏览器需要将这些图层组合起来，按正确的堆叠顺序渲染。同样，回流和重绘操作也会引发重新组合操作

###### 合成 Composite

<font color=FF0000>**由于页面的各部分可能被绘制到多层，由此它们需要按正确顺序绘制到屏幕上，以便正确渲染页面**</font>。这一步往往不会被提及。

##### 总结

上面 5 步（加上 composite 是 6 步）完成之后，设置 rel 为 refer / async 的 \<script> 标签中的 JS 文件，开始加载并执行。完成之后，整个网页便加载完成了

摘自：[浏览器的工作原理是什么?](https://www.bilibili.com/video/BV1Dh411J71c)



#### 关键渲染路径 (Critical Rendering Path / CRP)

关键渲染路径<font color=FF0000> 是浏览器将 HTML，CSS 和 JavaScript 转换为屏幕上的像素所经历的**步骤序列**</font>。优化关键渲染路径可提高渲染性能。<font color=FF0000>关键渲染路径 **包含了 文档对象模型 ( DOM )，CSS 对象模型 ( CSSOM )，渲染树和布局**</font>。

#####  概述如下

- 在解析 HTML 时会创建文档对象模型 ( DOM )。HTML 可以请求 JavaScript，而 JavaScript  反过来，又可以更改 DOM。

- HTML 包含（样式）或请求样式，依次来构建 CSS 对象模型。
- 浏览器引擎将两者结合起来以创建 渲染树。
- 布局确定页面上所有内容的大小和位置。
- 确定布局后，将像素绘制到屏幕上。

##### 理解 CRP

Web 性能包含了服务器请求和响应、加载、执行脚本、渲染、布局和绘制每个像素到屏幕上。

网页请求从 HTML 文件请求开始。服务器返回 HTML -- 响应头和数据。然后浏览器开始解析 HTML，转换收到的数据为 DOM 树。<font color=FF0000> 浏览器 <font size=4>**每次发现外部资源就初始化请求**</font>，无论是样式、脚本或者嵌入的图片引用。<font size=4>**有时请求会阻塞，这意味着解析剩下的 HTML 会被终止直到重要的资源被处理**</font></font>。浏览器接着解析 HTML，发请求和构造 DOM 直到文件结尾 ，这时开始构造 CSS对象模型。等到 DOM 和 CSSOM 完成之后，浏览器构造渲染树，计算所有可见内容的样式。一旦渲染树完成布局开始，定义所有渲染树元素的位置和大小。完成之后，页面被渲染完成，或者说是绘制到屏幕上。                                                                                                         

##### 文本对象模型 ( DOM )

<font color=FF0000> DOM构建是增量的</font>。 <font color=fuchsia>**HTML 响应变成令牌 ( token )**</font>（ 👀 这里的 token 和 鉴权的 Token 没有关系，与编译原理中的分词 ( Tokenization ) 概念相关），<font color=red>**令牌变成节点**</font>，而节点又变成 DOM 树。<font color=FF0000>  单个 DOM节点以 **startTag令牌 开始，以 endTag令牌 结束**。 **节点包含有关 HTML 元素的所有相关信息。 该信息是使用令牌描述的**</font>。 节点根据令牌层次结构连接到 DOM树中。 如果另一组 startTag 和 endTag 令牌位于一组 startTag 和 endTag 之间，则您在节点内有一个节点，这就是我们定义DOM树层次结构的方式。

节点数量越多，关键渲染路径中的后续事件将花费的时间就越长。 测一下吧！ 几个额外的节点不会有什么区别，但“DIV癖”（divitis）可能会导致问题。

##### CSS 对象模型 ( CSSOM )

DOM 包含页面所有的内容。CSSOM 包含了页面所有的样式，也就是如何展示 DOM 的信息。 CSSOM 跟 DOM 很像，但是不同。 <font color=red>DOM 构造是增量的，CSSOM 却不是</font>。 <font color=fuchsia>CSS 是渲染阻塞的：浏览器会阻塞页面渲染直到它接收和执行了所有的 CSS</font>。CSS 是渲染阻塞是因为规则可以被覆盖，所以内容不能被渲染直到 CSSOM 的完成。

CSS 有其自身的规则集合用来定义标识。注意 CSS 中的 C 代表的是 “层叠” ( Cascading )。CSS 规则是级联的。随着解析器转换标识为节点，节点的后代继承了样式。像处理 HTML 那样的增量处理功能没有被应用到 CSS 上，因为后续规则可能被之前的所覆盖。CSS 对象模型随着 CSS 的解析而被构建，但是直到完成都不能被用来构建渲染树，因为样式将会被之后的解析所覆盖而不应该被渲染到屏幕上。

<font color=dodgerblue>从选择器性能的角度，更少的特定选择器是比更多的要快</font>。例如，`.foo {}` 是比 `.bar .foo {}` 更快的因为当浏览器发现  `.foo` ，接下来必须沿着 DOM 向上走来（👀 自底向上）检查 `.foo` 是不是有一个祖先 `.bar`。越是具体的标签浏览器就需要更多的工作，但这样的弊端未必值得优化

如果你测量过解析 CSS 的时间，你将会被浏览器实在地快所震惊。更具体的规则更昂贵因为它必须遍历更多的 DOM 树节点，但这所带来的额外的消耗通常很小。先测量一下。然后按需优化。特定化或许不是你的低垂的果实。在 CSS 中选择器的性能优化，提升仅仅是毫秒级的。有其他一些方式来优化 CSS，例如压缩和使用媒体查询来异步处理 CSS 为非阻塞的请求。

##### 渲染树

渲染树包括了内容和样式：DOM 和 CSSOM 树结合为渲染树。<font color=fuchsia> 为了构造渲染树，浏览器检查每个节点，**从 DOM 树的根节点开始，并且决定哪些 CSS 规则被添加**</font>。

<font color=FF0000> 渲染树只包含了可见内容。头部（通常）不包含任何可见信息，因此不会被包含在渲染树种</font>。如果有元素上有 `display: none;`，它本身和其后代都不会出现在渲染树中。

##### 布局

一旦渲染树被构建，布局变成了可能。<font color=lightSeaGreen>布局取决于屏幕的尺寸。布局这个步骤决定了在哪里和如何在页面上放置元素，决定了每个元素的宽和高，以及他们之间的相关性</font>。

什么是一个元素的宽？块级元素，根据定义，默认有父级宽度的 100%。一个宽度 50% 的元素，将占据父级宽度的一半。除非另外定义，body 有 100% 的宽，意味着它占据视窗的 100%。设备的宽度影响布局。 

<font color=dodgerblue>视窗的元标签定义了布局视窗的宽度，从而影响布局</font>。没有的话，浏览器使用视窗的默认宽度，默认全屏浏览器通常是 960px。在默认情况下像你的手机浏览器的全屏浏览器，<font color=lightSeaGreen>通过设置 `<meta name="viewport" content="width=device-width">` ，宽度将会是设备的宽度而不是默认的视窗宽度</font>。设备宽度当用户在横向和纵向模式旋转他们的手机时将会改变。布局发生在每次设备旋转或浏览器缩放时。

布局性能受 DOM 影响：节点数越多，布局就需要更长的时间。布局将会变成瓶颈，如果期间需要滚动或者其他动画将会导致迟滞。20ms 的延迟在加载或者方向改变时或许还可以接受，但在动画或滚动时就会迟滞。任何渲染树改变的时候，像添加节点、改变内容或者在一个节点更新盒模型样式的时候布局就会发生。

为了减小布局事件的频率和时长，批量更新或者避免改动盒模型属性。

##### 绘制

最后一步是将像素绘制在屏幕上。 一旦渲染树创建并且布局完成，像素就可以被绘制在屏幕上。加载时，整个屏幕被绘制出来。之后， <font color=red>只有受影响的屏幕区域会被重绘，**浏览器被优化为只重绘需要绘制的最小区域**</font>。绘制时间取决于何种类型的更新被附加在渲染树上。绘制是一个非常快的过程，所以聚焦在提升性能时这大概不是最有效的部分，重点要记住的是当测量一个动画帧需要的时间需要考虑到布局和重绘时间。添加到节点的样式会增加渲染时间，但是移除样式增加的 0.001ms 或许不能让你的优化物有所值。记住先测量。然后你可决定它的优化优先级。

##### 合成 Composite

略。详见上面。

##### 优化 CRP

<font color=FF0000> 提升页面加载速度需要通过**被加载资源的优先级**、**控制它们加载的顺序**和**减小这些资源的体积**</font>。性能提示包含：

- 通过异步重要资源的下载来减小请求数量
- 优化必须的请求数量和每个请求的文件体积
- 通过<font color=FF0000> **区分关键资源的优先级来优化被加载关键资源的顺序**</font>，来缩短关键路径长度。

摘自：[MDN - 关键渲染路径](https://developer.mozilla.org/zh-CN/docs/Web/Performance/Critical_rendering_path)

#### 渲染树补充

**关键渲染路径中的令牌 ( Token )**

HTML中 尖括号 里的文本，具有特殊含义，属于标记。<font color=FF0000> **每当遇到一个标记，浏览器会发出一个令牌**</font>。**这些令牌包括**：

- 文档类型 DOCTYPE
- 开始标签 start tag
- 结束标签 end tag
- 注释 comment
- 字符 character
- 文件结束 end-of-file

<font color=fuchsia> 整个流程都由令牌解析器来完成，当令牌解析器在执行这一流程时，有另一个流程正在消耗这些令牌。并按照某种规则将它们转换为节点对象。</font>

<font color=FF0000> 令牌解析器发出了 **起始令牌** 和 **结束令牌**，这样就可以显示节点之间的关系</font>，比如 StartTag：head 令牌出现在EndTag：HTML令牌前面，表示  head令牌是HTML令牌的子级。

摘自：[笔记](https://www.jianshu.com/p/219a9462ff90)

当我们请求某个 URL 以后，浏览器获得响应的数据并将所有的标记转换到我们在屏幕上所看到的 HTML，这中间发生了什么？

- 浏览器从磁盘或网络中读取 HTML 原始字节，并根据文件的指定编码将它们转成字符。
- 当遇到 HTML **标记**时，浏览器会发出一个令牌，生成诸如 StartTag: HTML StartTag:head Tag: meta EndTag: head 这样的令牌 ( Token ) ，整个浏览由令牌生成器来完成。
- 在令牌生成的同时，另一个流程会同时消耗这些令牌并转换成 HTML head 这些节点对象，起始和结束令牌表明了节点之间的关系
- 当所有的令牌消耗完以后就转换成了DOM（文档对象模型）

<img src="https://s2.loli.net/2024/08/19/56aF8GoOmdilftk.png" alt="ToDOM" style="zoom:80%;" />

摘自：[详解 CRP：如何最大化提升首屏渲染速度](https://juejin.cn/post/6844903757038223367)

#### 渲染树 其他补充

尽管不同的渲染引擎渲染流程不同，但是都需要解析 HTML 和 CSS 用于生成渲染树。前端开发接触最多的渲染引擎是 WebKit（以及在其基础上派生的 Blink ），接下来本文会以 Webkit 为基础介绍渲染树。

![The Compositing Forest](https://s2.loli.net/2022/02/08/oQWuJlNCZEjtheA.jpg)

图片来自 [GPU Accelerated Compositing in Chrome](https://www.chromium.org/developers/design-documents/gpu-accelerated-compositing-in-chrome/)

上图中，除了我们熟悉的 DOM 树外，还有 RenderObject 树，RenderLayer 树，GraphicsLayer 树，它们共同构成了 “***渲染森林***”。

##### RenderObject 渲染对象

RenderObject 保存了绘制 DOM 节点所需要的各种信息，与 DOM 树对应，RenderObject 也构成了一颗树。但是RenderObject 的树与 DOM 节点并不是一一对应关系（ 👀 比如 `display: none` 的节点，不会生成渲染对象）。《Webkit 技术内幕》指出，如果满足下列条件，则会创建一个 RenderObject ：

- DOM 树中的 document 节点
- DOM 树中的可见节点（ Webkit 不会为非可视节点创建 RenderObject 节点）
- 为了处理需要，Webkit 建立匿名的 RenderObject 节点，如表示块元素的 RenderBlock（ RenderObject 的子类）节点

<font color=FF0000>将 DOM 节点绘制在页面上，除了要知道渲染节点的信息外，**还需要各渲染节点的层级**</font>（ 👀 可参考 composite ）。浏览器提供了 RenderLayer 来定义渲染层级

##### RenderLayer 渲染层级

<font color=FF0000>RenderLayer 是浏览器基于 RenderObject 创建的</font>。RenderLayer 最初是用来生成层叠上下文 ( stacking context )，以<font color=FF0000>**保证页面元素按照正确的层级展示**</font>。同样的， RenderObject 和 RenderLayer 也不是一一对应的；<font color=FF0000>**RenderObject 如果满足以下条件，则会创建对应的 RenderLayer**</font> （ [GPU Accelerated Compositing in Chrome](https://www.chromium.org/developers/design-documents/gpu-accelerated-compositing-in-chrome/)）：

- 文档的根节点
- 具有明确 CSS 定位信息的节点（如 relative，absolute 或者 transform ）
- 透明节点
- 有 overflow，mask 或者 reflection 属性的节点
- 有 filter 属性的节点
- 有 3D Context 或者加速的 2D Context 的 Canvas 节点
- 对应 Video 元素的节点。

<font color=FF0000>**可以将每一个 RenderLayer 想象成一个图层**。渲染就是在每个 RenderLayer 图层上，将 RenderObject 绘制出来</font>。这个过程可以使用 CPU 绘制，这就是软件绘图。但是软件绘图是无法处理 3D 的绘图上下文，每一层的 RenderObject 中都不能包含使用 3D 绘图的节点，例如有 3D Contex 的 Canvas 节点，也不能支持 CSS 3D 变化属性。此外，<font color=FF0000>页面动画中，每次元素尺寸或者位置变动，都要重新去构造 RenderLayer 树，**触发 Layout 及其后续的渲染流水线。这样会导致页面帧率的下降，造成视觉上的卡顿**。**所以现代浏览器引入了由 GPU 完成的硬件加速绘图**</font>。

<font color=FF0000>在获得了每一层的信息后，需要将其合并到同一个图像上，这个过程就是合成 ( Compositing ) </font>，使用了合成技术的称之为合成化渲染。

在软件渲染中，实际上是不需要合成的，因为软件渲染是按照从前到后的顺序在同一个内存空间完成每一层的绘制。在现代浏览器尤其是移动端设备中，使用 GPU 完成的硬件加速绘图更为常见。由 GPU 完成的硬件加速绘图需要合成，而合成都是使用 GPU 完成的，这整个过程称之为硬件加速的合成化渲染。
现代浏览器中，并不是所有的绘图都需要使用 GPU 来完成，《Webkit 技术内幕》中指出：

> 对于常见的 2D 绘图操作，使用 GPU 来绘图不一定比使用 CPU 绘图在性能上有优势，例如绘制文字、点、线等，原因是 CPU 的使用缓存机制有效减少了重复绘制的开销而不需要 GPU 并行性。

##### GraphicsLayer

<font color=FF0000>为了节省 GPU 的内存资源，Webkit 并不会为每个 RenderLayer 分配一个对应的后端存储。而是，**按照一定的规则，将一些 RenderLayer 组合在一起，形成一个有后端存储的新层，用于之后的合成，称之为 合成层**</font>。

<font color=FF0000>**合成层中，存储空间使用 GraphicsLayer 表示**</font>。对于<font color=FF0000>一个 RenderLayer 对象，**如果没有单独提升为合成层，则使用其父对象的合成层**</font>。如果一个 <font color=dodgerBlue>RenderLayer 具有以下几个特征之一</font> ( [GPU Accelerated Compositing in Chrome](https://www.chromium.org/developers/design-documents/gpu-accelerated-compositing-in-chrome/) ) ，<font color=dodgerBlue>则其 **具有自己的合成层**</font>：

- 有 3D 或者透视变换的 CSS 属性
- 包含使用硬件加速的视频加码技术的 Video 元素
- 有 3D Contex 或者加速的 2D Context 的 Canvas 元素（ 👀 普通的 2D Context 不会提升为合成层）
- 有 opacity、transform 改变的动画
- 使用了硬件加速的 CSS filter 技术
- 后代包含一个合成层
- Overlap 重叠：有一个 Z 坐标比自己小的兄弟节点，且该节点是一个合成层

对于 Overlap 重叠造成的合成层提升，[Compositing in Blink / WebCore: From WebCore::RenderLayer to cc:Layer](https://docs.google.com/presentation/d/1dDE5u76ZBIKmsqkWi2apx3BqV8HOcNf4xxBdyNywZR8/edit#slide=id.p) 给出了三幅图片：

![img](https://s2.loli.net/2022/05/23/vy7NAo3ijdCwMzY.jpg)

图 1 中，顶部的绿色矩形和底部的蓝色矩形是兄弟节点，蓝色矩形因为某种原因被提升为合成层。如果绿色矩形不进行合成层提升的话，它将和父节点共用一个合成层。这就导致在渲染时，绿色矩形位于蓝色矩形的底部，出现渲染出错（图 2)。所以如果发生重叠，绿色矩形也需要被提升为合成层。

对于合成层的提升条件，[无线性能优化：Composite](https://fed.taobao.org/blog/taofed/do71ct/performance-composite/) 中有更详细的介绍。结合 RenderLayer 和 GraphicsLayer 的创建条件，可以看出动画（尺寸、位置、样式等改变）元素更容易创建 RenderLayer ，进而提升为合成层（这里要注意，并不是所有的 CSS 动画元素都会被提升为合成层，这个会在后续的渲染流水线中介绍）。这种设计使浏览器可以更好使用 GPU 的能力，给用户带来流畅的动画体验。

> 👀 有点多，等有空了在看... // TODO 

摘自：[从浏览器渲染原理谈动画性能优化](https://segmentfault.com/a/1190000041295744)



## web 开发

#### 编写web程序时出现的问题

- 出现如下情况：

  <img src="https://s1.ax1x.com/2020/07/05/US9DqP.png" style="zoom: 45%;" />

  其中<font color=FF0000>**一种解释原因**</font>（无法确认是所有）是：当前访问的页面不存在（物理上不存在） / 当前程序能访问到的中的资源中没有该资源（资源存在，但程序找不到）。



#### 80端口和8080端口

<font color=FF0000>80是**http协议的默认端口**</font>，是在输入网站的时候其实浏览器（非IE）已经帮你输入协议了，所以你输入http://baidu.com，其实是访问http://baidu.com:80，而<font color=FF0000>8080，一般用于webcahe</font>，<font color=FF0000>一般是用来连接代理的</font>。完全不一样的两个，比如linux服务器里apache默认跑80端口，而apache-tomcat默认跑8080端口，其实端口没有实际意义只是一个接口，主要是看服务的监听端口，如果baidu的服务器监听的81端口，那么你直接输入就不行了就要输入http://baidu.com:81这样才能正常访问

摘自：[80端口跟8080端口有什么具体区别？ - 死神的沙漏的回答 - 知乎](https://www.zhihu.com/question/26698345/answer/33709330)



#### SOAP & REST

##### SOAP

> 👀 该 **API 协议** 已经基本被淘汰，类似的应该还有 WSDL；可见[让你的职业生涯少走弯路【让编程再次伟大#4】- 11:52](https://youtu.be/CLnsE0E3cXo?t=712)

SOAP：简单对象访问协议（Simple Object Access Protocol）是一种<mark>基于 XML 的协议</mark>，可以和现存的许多因特网协议和格式结合使用，包括超文本传输协议（HTTP），简单邮件传输协议（SMTP），多用途网际邮件扩充协议（MIME），基于“通用”传输协议是 SOAP的一个优点。它还支持从消息系统到远程过程调用（Remote Procedure Call，RPC）等大量的应用程序。SOAP提供了一系列的标准，如WSRM（WS-Reliable Messaging）形式化契约确保可靠性与安全性，确保异步处理与调用；WS-Security、WS-Transactions和WS-Coordination等标准提供了上下文信息与对话状态管理

##### REST

**REST（Representational State Transfer）表现层状态转化**，如果一个架构符合REST原则，就称它为RESTful架构。

要理解RESTful架构，最好的方法就是去理解Representational State Transfer这个词组到底是什么意思，它的每一个词代表了什么涵义。

- **资源（Resources）**

  REST的名称"表现层状态转化"中，省略了主语。<font color=FF0000>"表现层"其实指的是"资源"（Resources）的"表现层"。</font>

  **所谓"资源"，就是网络上的一个实体，或者说是网络上的一个具体信息。**它可以是一段文本、一张图片、一首歌曲、一种服务，总之就是一个具体的实在。<mark>你可以用一个URI（统一资源定位符）指向它，每种资源对应一个特定的URI。要获取这个资源，访问它的URI就可以，因此URI就成了每一个资源的地址或独一无二的识别符</mark>。所谓"上网"，就是与互联网上一系列的"资源"互动，调用它的URI。

- **表现层（Representation）**

  "资源"是一种信息实体，它可以有多种外在表现形式。**<mark>我们把"资源"具体呈现出来的形式，叫做它的"表现层"（Representation）</mark>。**

  比如，文本可以用txt格式表现，也可以用HTML格式、XML格式、JSON格式表现，甚至可以采用二进制格式；图片可以用JPG格式表现，也可以用PNG格式表现。

  URI只代表资源的实体，不代表它的形式。<mark>严格地说，有些网址最后的".html"后缀名是不必要的，因为这个后缀名表示格式，属于"表现层"范畴，而URI应该只代表"资源"的位置</mark>。<font color=FF0000>它的具体表现形式，应该在HTTP请求的头信息中用Accept和Content-Type字段指定，这两个字段才是对"表现层"的描述</font>。

- **状态转化（State Transfer）**

  访问一个网站，就代表了客户端和服务器的一个互动过程。在这个过程中，势必涉及到数据和状态的变化。

  <font color=FF0000>**互联网通信协议HTTP协议，是一个无状态协议**。这意味着，所有的状态都保存在服务器端</font>。因此，**如果客户端想要操作服务器，必须通过某种手段，让服务器端发生"状态转化"（State Transfer）。而这种转化是建立在表现层之上的，所以就是"表现层状态转化"。**

  客户端用到的手段，只能是HTTP协议。具体来说，就是HTTP协议里面，四个表示操作方式的动词：GET、POST、PUT、DELETE。它们分别对应四种基本操作：<font color=FF0000>**GET用来获取资源，POST用来新建资源（也可以用于更新资源），PUT用来更新资源，DELETE用来删除资源。**</font>

**RESTful架构有一些典型的<font color=FF0000>设计误区</font>**

**<font color=FF0000>最常见的一种设计错误，就是URI包含动词</font>。**因为"资源"表示一种实体，<font color=FF0000>所以应该是名词</font>，URI不应该有动词，动词应该放在HTTP协议中。

举例来说：某个URI是/posts/show/1，其中show是动词，这个URI就设计错了，<mark>正确的写法应该是/posts/1，然后用GET方法表示show</mark>。

如果某些动作是HTTP动词表示不了的，你就应该把动作做成一种资源。比如网上汇款，从账户1向账户2汇款500元，<mark>错误的URI是：</mark>

> 　　POST /accounts/1/transfer/500/to/2

<mark>正确的写法</mark>是**把动词transfer改成名词transaction**，资源不能是动词，但是可以是一种服务：

> 　　POST /transaction HTTP/1.1
> 　　Host: 127.0.0.1
> 　　
> 　　from=1&to=2&amount=500.00

**另一个设计误区，就是在URI中加入版本号**：

> 　　http://www.example.com/app/1.0/foo
>
> 　　http://www.example.com/app/1.1/foo
>
> 　　http://www.example.com/app/2.0/foo

因为不同的版本，可以理解成同一种资源的不同表现形式，所以应该采用同一个URI。<mark>版本号可以在HTTP请求头信息的Accept字段中进行区分</mark>（参见[Versioning REST Services](http://www.informit.com/articles/article.aspx?p=1566460)）：

> 　　Accept: vnd.example-com.foo+json; version=1.0
>
> 　　Accept: vnd.example-com.foo+json; version=1.1
>
> 　　Accept: vnd.example-com.foo+json; version=2.0

摘自：[阮一峰：理解RESTful架构](http://www.ruanyifeng.com/blog/2011/09/restful.html)

**RESTful 的<font color=FF0000>五个约束</font>**<mark>（如果一个系统满足了下面所列出的五条约束，那么该系统就被称为是RESTful的）</mark>

- **使用客户/服务器模型（C/S）**：客户和服务器之间通过一个统一的接口来互相通讯。
- **层次化的系统**：在一个REST系统中，客户端并不会固定地与一个服务器打交道。
- **无状态**：在一个REST系统中，服务端并不会保存有关客户的任何状态。也就是说，客户端自身负责用户状态的维持，并在每次发送请求时都需要提供足够的信息。
- **可缓存**：REST系统需要能够恰当地缓存请求，以尽量减少服务端和客户端之间的信息传输，以提高性能。
- <font color=FF0000>**统一的接口**</font>：一个REST系统<mark>需要使用一个统一的接口来完成子系统之间以及服务与用户之间的交互</mark>。这使得REST系统中的各个子系统可以独自完成演化。

**与REST密切相关的两个名词：<font color=FF0000>资源</font>和<font color=FF0000>状态</font>**

可以说，<font color=FF0000>**资源**</font>是REST系统的核心概念。<mark>所有的设计都会以资源为中心</mark>，包括如何对资源进行添加，更新，查找以及修改等。而资源本身则拥有一系列<font color=FF0000>**状态**</font>：<mark>在每次对资源进行添加 ，删除或修改的时候，资源就将从一个状态转移到另外一个状态。</mark>

摘自：[简书文章：rest](https://www.jianshu.com/p/90d757e925d1)

**REST 架构的主要原则**

- <font color=FF0000>网络上的所有事物都被抽象为资源</font>
- <font color=FF0000>每个资源都有一个唯一的资源标识符</font>
- 同一个资源具有多种表现形式(xml, json等)
- 对资源的各种操作不会改变资源标识符
- 所有的操作都是无状态的
- 符合REST原则的架构方式即可称为RESTful

摘自：[什么是 REST 以及 RESTful?](http://blog.bhusk.com/articles/2018/05/24/1527167962745)

优雅的REST风格的<font color=FF0000>资源URL</font>不希望带有.html或.do等后缀

摘自：[视图和视图解析器](https://www.cnblogs.com/xuweiweiwoaini/p/11852405.html)



#### REST 和 SOAP 区别

<font color=FF0000>在 SOAP 模式把 HTTP  作为一种通信协议，而不是应用协议</font>。所以 http 中的表头，错误信息等全部无视。实际上 HTTP 有 put、get、post、delete 等方法。
<font color=FF0000>REST 则不然，HTTP method 中的 POST GET PUT DELETE 都是与请求方法对应的</font>，rest真正实现了http的五层结构。REST 提交的请求中，代理服务器可以通过请求方式直接判断请求动作是要进行什么操作。

摘自：[REST 和 SOAP 的区别理解](https://blog.csdn.net/carlsunnnn/article/details/80570872)



#### POJO

POJO（Plain Ordinary Java Object）即普通 Java 类，具有一部分 getter / setter 方法的那种类就可以称作 POJO。

实际意义就是普通的JavaBeans（简单的实体类），特点就是<mark>支持业务逻辑的<font color=FF0000>协助类</font></mark>。
POJO类的作用是方便程序员使用数据库中的数据表，对于程序员来说，可以很方便的将POJO类当作对象来进行使用，也可以方便的调用其get，set方法。<font color=FF0000>但**不允许有业务方法**，也不能携带有connection之类的方法，即**不包含业务逻辑或持久逻辑等**</font>。

<mark>当一个Pojo可序列化，有一个无参的构造函数，使用getter和setter方法来访问属性时，他就是一个JavaBean</mark>。

摘自：[POJO和JavaBean的区别](https://blog.csdn.net/tiantangdizhibuxiang/article/details/81784873)

##### entity、bo、vo、po、dto、pojo的定义与区别

- **Entity：**最常用实体类，<font color=FF0000>基本和数据表一一对应，一个实体一张表</font>。
- **Bo (business object)：**即<font color=FF0000>业务对象</font>，Bo就是<mark>把<font color=FF0000>**业务逻辑**</font>封装为一个对象</mark>，这个对象可以包括一个或多个其它的对象。通过调用Dao方法，结合Po或Vo进行业务操作。
- **Vo (value object)：**即<font color=FF0000>值对象</font>，通常用于<font color=FF0000>**业务层**之间的数据传递</font>，由new创建，由GC回收。
- **Po (persistant object)：**即<font color=FF0000>持久层对象</font>，<font color=FF0000>对应数据库中**表的字段**</font>，数据库表中的记录在java对象中的显示状态，最形象的理解就是一个PO就是数据库中的一条记录。
- **Dto (data transfer object)：**即<font color=FF0000>数据传输对象</font>。是<font color=FF0000>一种设计模式之间传输数据的软件应用系统</font>，数据传输目标往往是数据访问对象从数据库中检索数据。数据传输对象与数据交互对象或数据访问对象之间的差异是一个以不具任何行为除了存储和检索的数据（访问和存取器）。简而言之，就是<font color=FF0000>接口之间传递的数据封装</font>（用于前后端数传输），<mark>比如用于Json和类（DTO）的转化之中</mark>。
- **pojo (plian ordinary java object)：**简单无规则java对象，纯的传统意义的java对象，最基本的Java Bean只有属性加上属性的get和set方法。可以额转化为Po、Dto、Vo；<mark>比如POJO在<font color=FF0000>传输过程中就是DTO</font></mark>
- **Dao (data access object)：**即<font color=FF0000>数据访问对象</font>。是sun的一个标准j2ee设计模式的接口之一，负责持久层的操作。主要<font color=FF0000>用来封装**对数据的访问**</font>，注意，是对数据的访问，不是对数据库的访问

所以实际项目中，一般都是这样应用的：
 控制层(controller-action)，业务层/服务层( bo-manager-service)，实体层(po-entity)，dao(dao)，视图对象(Vo-)，视图层(view-jsp/html)

摘自：[entity、bo、vo、po、dto、pojo如何理解和区分？](https://www.jianshu.com/p/b934b0d72602)



#### OAuth

**OAuth（Open Authorization）**。OAuth协议为用户资源的授权提供了一个安全的、开放而又简易的标准。与以往的授权方式不同之处是OAuth的授权不会使第三方触及到用户的帐号信息（如用户名与密码），即<mark>第三方无需使用用户的用户名与密码就可以申请获得该用户资源的授权</mark>，因此OAuth是安全的。

摘自：[百度百科 - OAuth](https://baike.baidu.com/item/oAuth)

类似的可参考：[OAuth 2.0 的一个简单解释](http://www.ruanyifeng.com/blog/2019/04/oauth_design.html)



#### SSO 单点登录

##### 定义

SSO（ Single Sign On 单点登录 ），它的定义是：<font color=FF0000>在多个应用系统中，用户只需要登录一次，就可以访问所有相互信任的应用系统</font>。比如：在 pan.baidu.com 登录，可以在 map.baidu.com 中也登录了。

在不同域名的情况下（详见下面 [[#不同域名下实现单点登录]] 认证中心实现），SSO 一般都需要一个独立的认证中心 ( passport )，子系统的登录均得通过 passport，子系统本身将不参与登录操作。

<font color=FF0000>当一个系统成功登录以后，passport 将会颁发一个令牌给各个子系统，子系统可以拿着令牌会获取各自的受保护资源</font>。**为了减少频繁认证，各个子系统在被 passport 授权以后，<font color=FF0000>会建立一个局部会话，在一定时间内可以无需再次向 passport 发起认证</font>**

##### 同域名下实现单点登录

> Domain、Path、Name 三者决定一个 Cookie

Cookie 的 Domin 属性设置为当前域的父域，并且父域的 Cookie 会被子域所共享。Path 属性默认为 web 应用的上下文路径。

利用 Cookie 的这个特点，只需要<font color=FF0000>将 Cookie 的 Domain 属性设置为父域的域名（主域名），同时将 Cookie 的 Path 属性设置为根路径，将 Session ID（ 或 Token ）保存到父域中；这样所有的子域应用就都可以访问到这个 Cookie</font>

不过**这要求应用系统的域名需建立在一个共同的主域名之下**，如 pan.baidu.com 和 map.baidu.com，它们都建立在 baidu.com 这个主域名之下，那么它们就可以通过这种方式来实现单点登录。

##### 不同域名下实现单点登录

###### 认证中心 ( CAS ) 实现

<font color=dodgerBlue>在不同域的情况下，Cookie 是不共享的</font>，<font color=fuchsia>**可以部署一个 认证中心 ，用于专门处理登录请求的独立的 Web 服务**</font>

用户统一在认证中心进行登录，登录成功后，<font color=FF0000>认证中心记录用户的登录状态，并将 Token 写入 Cookie</font>

> ⚠️ 这个 Cookie 是认证中心的，应用系统是访问不到的

> 👀 下面的流程有点费解，这里摘抄一下易懂的版本：
>
> > 不同域名下的单点登录：CAS流程：用户登录子系统时未登录，跳转到 SSO 登录界面，成功登录后，SSO 生成一个 ST ( service ticket )。<font color=fuchsia>用户登录不同的域名时，都会跳转到 SSO</font>，然后 <font color=fuchsia>SSO 带着 ST 返回到不同的子域名</font>，<font color=red>**子域名中发出请求验证 ST 的正确性（防止篡改请求）**</font>。<font color=red>验证通过后即可完成不同的业务</font>。
> >
> > 摘自：[前端实现单点登录（SSO）](https://juejin.cn/post/7282692430117748755)

应用系统检查当前请求有没有 Token，如果没有，说明用户在当前系统中尚未登录，那么就将页面跳转至认证中心。

由于这个操作会将认证中心的 Cookie 自动带过去，因此，认证中心能够根据 Cookie 知道用户是否已经登录过了。如果认证中心发现用户尚未登录，则返回登录页面，等待用户登录。如果发现用户已经登录过了，就不会让用户再次登录了；<font color=FF0000>会跳转回目标 URL ，并**在跳转前生成一个 Token，拼接在目标 URL 的后面**，回传给目标应用系统</font>。

<font color=FF0000>应用系统拿到 Token 之后，还需要向认证中心确认下 Token 的合法性，防止用户伪造</font>。确认无误后，应用系统记录用户的登录状态，并将 Token 写入 Cookie，然后给本次访问放行。（注意这个 Cookie 是当前应用系统的）当用户再次访问当前应用系统时，就会自动带上这个 Token，应用系统验证 Token 发现用户已登录，于是就不会有认证中心什么事了。

<font color=FF0000>此种实现方式相对复杂，**支持跨域，扩展性好，是单点登录的标准做法**</font>

###### 前端控制 实现

可以选择<font color=FF0000>将 Session ID （或 Token ）保存到浏览器的 LocalStorage 中</font>，让<font color=FF0000>前端在每次向后端发送请求时，主动将 LocalStorage 的数据 ( 👀 Session ID? ) 传递给服务端</font>。

这些都是由前端来控制的，后端需要做的仅仅是在用户登录成功后，将 Session ID （或 Token ）放在响应体中传递给前端

单点登录完全可以在前端实现。前端拿到 Session ID （或 Token ）后，除了将它写入自己的 LocalStorage 中之外，还可以通过特殊手段将它写入多个其他域下的 LocalStorage 中。

**关键代码如下：**

```javascript
// 获取 token
var token = result.data.token;
 
// 动态创建一个不可见的 iframe，在 iframe 中加载一个跨域 HTML
var iframe = document.createElement("iframe");
iframe.src = "http://app1.com/localstorage.html";
document.body.append(iframe);

// 使用 postMessage() 方法将 token 传递给 iframe
setTimeout(function () {
    iframe.contentWindow.postMessage(token, "http://app1.com");
}, 4000);
setTimeout(function () {
    iframe.remove();
}, 6000);
 
// 在这个iframe所加载的HTML中绑定一个事件监听器，当事件被触发时，把接收到的token数据写入localStorage
window.addEventListener('message', function (event) {
    localStorage.setItem('token', event.data)
}, false);
```

前端通过 iframe + postMessage() 方式，将同一份 Token 写入到了多个域下的 LocalStorage 中，前端每次在向后端发送请求之前，都会主动从 LocalStorage 中读取 Token 并在请求中携带，这样就实现了同一份 Token 被多个域所共享

<font color=FF0000>此种实现方式完全由前端控制，几乎不需要后端参与，同样支持跨域</font>

##### 认证中心实现流程图

![img](https://s2.loli.net/2022/05/25/Fx8IVRk3nC2bezN.png)

- 用户访问 系统1 的受保护资源，系统1 发现用户未登录，跳转至 SSO 认证中心，并将自己的地址作为参数
- SSO 认证中心发现用户未登录，将用户引导至登录页面
- 用户输入用户名密码提交登录申请
- <font color=FF0000>SSO 认证中心校验用户信息，创建用户与 SSO 认证中心之间的会话，称为 <font size=4>***全局会话***</font>，同时创建授权令牌</font>
- <font color=FF0000>**SSO 认证中心带着令牌跳转会最初的请求地址（系统1）**</font>
- <font color=FF0000>系统1 拿到令牌，去 SSO 认证中心校验令牌是否有效</font>
- SSO 认证中心校验令牌，返回有效，注册 系统1
- <font color=FF0000>系统1 使用该令牌创建与用户的会话，称为 <font size=4>***局部会话***</font>，返回受保护资源</font>
- 用户访问 系统2 的受保护资源
- 系统2 发现用户未登录，跳转至 SSO 认证中心，并将自己的地址作为参数
- <font color=FF0000>SSO 认证中心发现用户已登录，跳转回 系统2 的地址，并附上令牌</font>
- 系统2 拿到令牌，去 SSO 认证中心校验令牌是否有效
- SSO 认证中心校验令牌，返回有效，注册 系统2
- 系统2 使用该令牌创建与用户的局部会话，返回受保护资源

用户登录成功之后，会与 SSO  认证中心及各个子系统建立会话，用户与 SSO  认证中心建立的会话称为全局会话

用户与各个子系统建立的会话称为局部会话，局部会话建立之后，用户访问子系统受保护资源将不再通过 SSO  认证中心

###### 全局会话与局部会话有如下约束关系

- 局部会话存在，全局会话一定存在
- 全局会话存在，局部会话不一定存在
- 全局会话销毁，局部会话必须销毁

摘自：[面试官：说说什么是单点登录？如何实现?](https://segmentfault.com/a/1190000039712911)



#### JSP

**JSP的本质其实就是 Servlet**。只是 JSP 当初设计的目的是为了简化 Servlet 输出 HTML 代码。

**JSP已经很少用了，被下面两种技术替代**

- 模板引擎（freemarker、Thymeleaf、Velocity），用法其实跟JSP差不太多，只是它们的性能会更好
- 前后端分离，后端只需要返回JSON给前端，页面完全不需要后端管

摘自：[2020年了，不学jsp后学什么代替？ - Java3y的回答 - 知乎 ](https://www.zhihu.com/question/382065572/answer/1102140696)

##### JSP九大内置对象

###### 输出输入对象

- **request对象：** 向客户端请求数据

  **所在包：**请求信息 javax.servlet.http.HttpServletrequest

  | 方法名               | 说明                                                         |
  | -------------------- | ------------------------------------------------------------ |
  | isUserInRole         | 判断认证后的用户是否属于某一成员组                           |
  | getAttribute         | 获取指定属性的值,如该属性值不存在返回Null                    |
  | getAttributeNames    | 获取所有属性名的集合                                         |
  | getCookies           | 获取所有Cookie对象                                           |
  | getCharacterEncoding | 获取请求的字符编码方式                                       |
  | getContentLength     | 返回请求正文的长度,如不确定返回-1                            |
  | getHeader            | 获取指定名字报头值                                           |
  | getHeaders           | 获取指定名字报头的所有值,一个枚举                            |
  | getHeaderNames       | 获取所有报头的名字,一个枚举                                  |
  | getInputStream       | 返回请求输入流,获取请求中的数据                              |
  | getMethod            | 获取客户端向[服务器](http://www.23book.net/Server/Index.htm)端传送数据的方法 |
  | getParameter         | 获取指定名字参数值                                           |
  | getParameterNames    | 获取所有参数的名字,一个枚举                                  |
  | getParameterValues   | 获取指定名字参数的所有值                                     |
  | getProtocol          | 获取客户端向[服务器](http://www.23book.net/Server/Index.htm)端传送数据的协议名称 |
  | getQueryString       | 获取以get方法向[服务器](http://www.23book.net/Server/Index.htm)传送的查询字符串 |
  | getRequestURI        | 获取发出请求字符串的客户端地址                               |
  | getRemoteAddr        | 获取客户端的IP地址                                           |
  | getRemoteHost        | 获取客户端的名字                                             |
  | getSession           | 获取和请求相关的会话                                         |
  | getServerName        | 获取[服务器](http://www.23book.net/Server/Index.htm)的名字   |
  | getServerPath        | 获取客户端请求文件的路径                                     |
  | getServerPort        | 获取[服务器](http://www.23book.net/Server/Index.htm)的端口号 |
  | removeAttribute      | 删除请求中的一个属性                                         |
  | setAttribute         | 设置指定名字参数值                                           |

- **response对象：**封装了jsp产生的响应,然后被发送到客户端以响应客户的请求

  **所在包：**响应 javax.servlet.http.HttpServletResponse

  | 方法名          | 说明                               |
  | --------------- | ---------------------------------- |
  | addCookie       | 添加一个Cookie对象                 |
  | addHeader       | 添加Http文件指定名字头信息         |
  | containsHeader  | 判断指定名字Http文件头信息是否存在 |
  | encodeURL       | 使用sessionid封装URL               |
  | flushBuffer     | 强制把当前缓冲区内容发送到客户端   |
  | getBufferSize   | 返回缓冲区大小                     |
  | getOutputStream | 返回到客户端的输出流对象           |
  | sendError       | 向客户端发送错误信息               |
  | sendRedirect    | 把响应发送到另一个位置进行处理     |
  | setContentType  | 设置响应的MIME类型                 |
  | setHeader       | 设置指定名字的Http文件头信息       |

- **out对象：**向客户端输出数据

  **所在包：**数据流 javax.servlet.jsp.jspWriter

  | 方法名         | 说明                              |
  | -------------- | --------------------------------- |
  | print或println | 输出数据                          |
  | newLine        | 输出换行字符                      |
  | flush          | 输出缓冲区数据                    |
  | close          | 关闭输出流                        |
  | clear          | 清除缓冲区中数据,但不输出到客户端 |
  | clearBuffer    | 清除缓冲区中数据,输出到客户端     |
  | getBufferSize  | 获得缓冲区大小                    |
  | getRemaining   | 获得缓冲区中没有被占用的空间      |
  | isAutoFlush    | 是否为自动输出                    |

###### 通信控制对象

- **pageContext对象：**

  **所在包：**页面上下文 javax.servlet.jsp.PageContext)

  | 方法名            | 说明                                 |
  | ----------------- | ------------------------------------ |
  | forward           | 重定向到另一页面或Servlet组件        |
  | getAttribute      | 获取某范围中指定名字的属性值         |
  | findAttribute     | 按范围搜索指定名字的属性             |
  | removeAttribute   | 删除某范围中指定名字的属性           |
  | setAttribute      | 设定某范围中指定名字的属性值         |
  | getException      | 返回当前异常对象                     |
  | getRequest        | 返回当前请求对象                     |
  | getResponse       | 返回当前响应对象                     |
  | getServletConfig  | 返回当前页面的ServletConfig对象      |
  | getServletContext | 返回所有页面共享的ServletContext对象 |
  | getSession        | 返回当前页面的会话对象               |

- **session对象：**

  **所在包：**会话 javax.servlet.http.HttpSession

  | 方法名                 | 说明                                    |
  | ---------------------- | --------------------------------------- |
  | getAttribute           | 获取指定名字的属性                      |
  | getAttributeNames      | 获取session中全部属性名字,一个枚举      |
  | getCreationTime        | 返回session的创建时间                   |
  | getId                  | 获取会话标识符                          |
  | getLastAccessedTime    | 返回最后发送请求的时间                  |
  | getMaxInactiveInterval | 返回session对象的生存时间单位千分之一秒 |
  | invalidate             | 销毁session对象                         |
  | isNew                  | 每个请求是否会产生新的session对象       |
  | removeAttribute        | 删除指定名字的属性                      |
  | setAttribute           | 设定指定名字的属性值                    |

- **application对象：**

  **所在包：**应用程序 javax.servlet.ServletContext)

  | 方法名            | 说明                                  |
  | ----------------- | ------------------------------------- |
  | getAttribute      | 获取应用对象中指定名字的属性值        |
  | getAttributeNames | 获取应用对象中所有属性的名字,一个枚举 |
  | getInitParameter  | 返回应用对象中指定名字的初始参数值    |
  | getServletInfo    | 返回Servlet编译器中当前版本信息       |
  | setAttribute      | 设置应用对象中指定名字的属性值        |

- **Servlet对象：**

  - **page对象：**

    **所在包：**当前JSP的实例,java.lang.object

    它代表JSP被编译成Servlet,可以使用它来调用Servlet类中所定义的方法

  - **config对象：**

    **所在包：**Servlet的配置信息 javax.servlet.ServletConfig

    | 方法名                | 说明                                 |
    | --------------------- | ------------------------------------ |
    | getServletContext     | 返回所执行的Servlet的环境对象        |
    | getServletName        | 返回所执行的Servlet的名字            |
    | getInitParameter      | 返回指定名字的初始参数值             |
    | getInitParameterNames | 返回该JSP中所有的初始参数名,一个枚举 |


###### 错误处理对象

- **exception对象**

  **所在包：**运行时的异常,java.lang.Throwable

  被调用的错误页面的结果,只有在错误页面中才可使用,

  即在页面指令中设置：`<%@page isErrorPage=“true”%>`

摘自：[JSP 9 大内置对象详解](https://www.cnblogs.com/kelin1314/archive/2011/03/03/1969578.html)



#### Tomcat

##### 启动tomcat

在 Windows 上，可以通过执行下面的命令来启动 Tomcat：

```sh
%CATALINA_HOME%\bin\startup.bat
```

 或者

```shell
C:\apache-tomcat-5.5.29\bin\startup.bat
```

在 Unix（Solaris、Linux 等） 上，可以通过执行下面的命令来启动 Tomcat：

```sh
$CATALINA_HOME/bin/startup.sh
```

 或者

```
/usr/local/apache-tomcat-5.5.29/bin/startup.sh
```

**关闭 tomcat**

在 Windows 上，可以通过执行下面的命令来停止 Tomcat：

```
C:\apache-tomcat-5.5.29\bin\shutdown
```

在 Unix（Solaris、Linux 等） 上，可以通过执行下面的命令来停止 Tomcat：

```
/usr/local/apache-tomcat-5.5.29/bin/shutdown.sh
```



#### Servlet 的生命周期

Servlet 生命周期可被定义为从创建直到毁灭的整个过程。以下是 Servlet 遵循的过程：

- Servlet 通过调用<font color=FF0000> **init ()** </font>方法进行<font color=FF0000>初始化</font>。

  <mark>init 方法被设计成只调用一次。它在第一次创建 Servlet 时被调用，在后续每次用户请求时不再调用</mark>。

  同时，Servlet 创建于用户第一次调用对应于该 Servlet 的 URL 时，但是您也可以指定 Servlet 在服务器第一次启动时被加载。

  <mark>当用户调用一个 Servlet 时，就会创建一个 Servlet 实例，每一个用户请求都会产生一个新的线程，适当的时候移交给 doGet 或 doPost 方法</mark>。init() 方法简单地创建或加载一些数据，这些数据将被用于 Servlet 的整个生命周期。

  init 方法的定义如下：

  ```java
  public void init() throws ServletException {
    // 初始化代码...
  }
  ```

- Servlet 调用 **<font color=FF0000>service()</font>** 方法来<font color=FF0000>处理客户端的请求</font>。

  service() 方法是执行实际任务的主要方法。<mark>Servlet 容器（即 Web 服务器）调用 service() 方法来处理来自客户端（浏览器）的请求，并把格式化的响应写回给客户端</mark>。

  <mark>每次服务器接收到一个 Servlet 请求时，服务器会产生一个新的线程并调用服务</mark>。service() 方法检查 HTTP 请求类型（GET、POST、PUT、DELETE 等），并在适当的时候调用 doGet、doPost、doPut，doDelete 等方法。

  下面是该方法的特征：

  ```java
  public void service(ServletRequest request, 
                      ServletResponse response) 
        throws ServletException, IOException{
  }
  ```

  service() 方法由容器调用，service 方法在适当的时候调用 doGet、doPost、doPut、doDelete 等方法。所以，<font color=FF0000>您不用对 service() 方法做任何动作，您只需要根据来自客户端的请求类型来重写 doGet() 或 doPost() 即可</font>。

  doGet() 和 doPost() 方法是每次服务请求中最常用的方法。下面是这两种方法的特征。

  - **doGet() 方法**

    <font color=FF0000>GET 请求来自于一个 URL 的正常请求</font>，或者来自于一个未指定 METHOD 的 HTML 表单，它由 doGet() 方法处理。

    ```java
    public void doGet(HttpServletRequest request,
                      HttpServletResponse response)
        throws ServletException, IOException {
        // Servlet 代码
    }
    ```

  - **doPost() 方法**

    <font color=FF0000>POST 请求来自于一个特别指定了 METHOD 为 POST 的 **HTML 表单**</font>，它由 doPost() 方法处理。

    ```java
    public void doPost(HttpServletRequest request,
                       HttpServletResponse response)
        throws ServletException, IOException {
        // Servlet 代码
    }
    ```

- Servlet 通过调用 <font color=FF0000>**destroy()** </font>方法<font color=FF0000>终止（结束）</font>。

  <font color=FF0000>destroy() 方法只会被调用一次，在 Servlet 生命周期结束时被调用</font>。<mark>destroy() 方法可以让您的 Servlet 关闭数据库连接、停止后台线程、把 Cookie 列表或点击计数器写入到磁盘，并执行其他类似的清理活动</mark>。

  在调用 destroy() 方法之后，servlet 对象被标记为垃圾回收。destroy 方法定义如下所示：

  ```java
    public void destroy() {
      // 终止化代码...
    }
  ```

- 最后，Servlet 是由<font color=FF0000> JVM 的垃圾回收器进行垃圾回收的</font>。



#### 实现Servlet

Servlet 是服务 HTTP 请求并实现 javax.servlet.Servlet 接口的 Java 类。<mark>Web 应用程序开发人员通常编写 Servlet 来扩展 javax.servlet.http.HttpServlet，并<font color=FF0000>实现 Servlet 接口的抽象类</font>专门用来处理 HTTP 请求</mark>。



#### Get 和 Post 方法

##### GET 方法

GET 方法向页面请求发送已编码的用户信息。<font color=FF0000>页面和已编码的信息中间用 `?` 字符分隔</font>，如下所示：

```
http://www.test.com/hello?key1=value1&key2=value2
```

GET 方法是<font color=FF0000>默认的从浏览器向 Web 服务器传递信息的方法</font>，它会产生一个很长的字符串，出现在浏览器的地址栏中。如果您要向服务器传递的是密码或其他的敏感信息，请不要使用 GET 方法。<mark>GET 方法有大小限制：请求字符串中最多只能有 1024 个字符</mark>。

这些信息使用 QUERY_STRING 头传递，并可以通过 QUERY_STRING 环境变量访问，Servlet 使用 **doGet()** 方法处理这种类型的请求。

##### POST 方法

另一个向后台程序传递信息的比较可靠的方法是 POST 方法。POST 方法打包信息的方式与 GET 方法基本相同，但是 <font color=FF0000>POST 方法不是把信息作为 URL 中 ? 字符后的文本字符串进行发送，而是把这些信息作为一个单独的消息。消息以标准输出的形式传到后台程序，您可以解析和使用这些标准输出</font>。Servlet 使用 doPost() 方法处理这种类型的请求。



#### 使用 Servlet 读取表单数据

Servlet 处理表单数据，这些数据会根据不同的情况使用不同的方法自动解析：

- **getParameter()：**您<font color=FF0000>可以调用 request.getParameter() 方法来获取表单参数的值</font>。
- **getParameterValues()：**如果<font color=FF0000>参数出现一次以上，则调用该方法，并返回多个值</font>，例如复选框。
- **getParameterNames()：**如果您想要<font color=FF0000>得到当前请求中的所有参数的完整列表</font>，则调用该方法。



#### Servlet 的 Http 请求

当浏览器请求网页时，它会向 Web 服务器发送特定信息，这些信息不能被直接读取，因为<font color=FF0000>这些信息是作为 HTTP **请求的头**的一部分进行传输的</font>。

##### 浏览器端的重要头信息

| 头信息              | 描述                                                         |
| :------------------ | :----------------------------------------------------------- |
| Accept              | 这个头信息指定浏览器或其他客户端可以处理的 MIME 类型。值 **image/png** 或 **image/jpeg** 是最常见的两种可能值。 |
| Accept-Charset      | 这个头信息指定浏览器可以用来显示信息的字符集。例如 ISO-8859-1。 |
| Accept-Encoding     | 这个头信息指定浏览器知道如何处理的编码类型。值 **gzip** 或 **compress** 是最常见的两种可能值。 |
| Accept-Language     | 这个头信息指定客户端的首选语言，在这种情况下，Servlet 会产生多种语言的结果。例如，en、en-us、ru 等。 |
| Authorization       | 这个头信息用于客户端在访问受密码保护的网页时识别自己的身份。 |
| Connection          | 这个头信息指示客户端是否可以处理持久 HTTP 连接。持久连接允许客户端或其他浏览器通过单个请求来检索多个文件。值 **Keep-Alive** 意味着使用了持续连接。 |
| Content-Length      | 这个头信息只适用于 POST 请求，并给出 POST 数据的大小（以字节为单位）。 |
| Cookie              | 这个头信息把之前发送到浏览器的 cookies 返回到服务器。        |
| Host                | 这个头信息指定原始的 URL 中的主机和端口。                    |
| If-Modified-Since   | 这个头信息表示只有当页面在指定的日期后已更改时，客户端想要的页面。如果没有新的结果可以使用，服务器会发送一个 304 代码，表示 **Not Modified** 头信息。 |
| If-Unmodified-Since | 这个头信息是 If-Modified-Since 的对立面，它指定只有当文档早于指定日期时，操作才会成功。 |
| Referer             | 这个头信息指示所指向的 Web 页的 URL。例如，如果您在网页 1，点击一个链接到网页 2，当浏览器请求网页 2 时，网页 1 的 URL 就会包含在 Referer 头信息中。 |
| User-Agent          | 这个头信息识别发出请求的浏览器或其他客户端，并可以向不同类型的浏览器返回不同的内容。 |

##### 读取 HTTP 头的方法

下面的方法可用在 Servlet 程序中读取 HTTP 头。这些方法通过<font color=FF0000> HttpServletRequest</font> 对象可用。

| 序号 | 方法 & 描述                                                  |
| :--- | :----------------------------------------------------------- |
| 1    | **Cookie[] getCookies()**<br/> 返回一个数组，包含客户端发送该请求的所有的 Cookie 对象。 |
| 2    | **Enumeration getAttributeNames()**<br> 返回一个枚举，包含提供给该请求可用的属性名称。 |
| 3    | **Enumeration getHeaderNames()**<br> 返回一个枚举，包含在该请求中包含的所有的头名。 |
| 4    | **Enumeration getParameterNames()**<br> 返回一个 String 对象的枚举，包含在该请求中包含的参数的名称。 |
| 5    | **HttpSession getSession()**<br> 返回与该请求关联的当前 session 会话，或者如果请求没有 session 会话，则创建一个。 |
| 6    | **HttpSession getSession(boolean create)**<br> 返回与该请求关联的当前 HttpSession，或者如果没有当前会话，且创建是真的，则返回一个新的 session 会话。 |
| 7    | **Locale getLocale()**<br> 基于 Accept-Language 头，返回客户端接受内容的首选的区域设置。 |
| 8    | **Object getAttribute(String name)**<br> 以对象形式返回已命名属性的值，如果没有给定名称的属性存在，则返回 null。 |
| 9    | **ServletInputStream getInputStream()**<br> 使用 ServletInputStream，以二进制数据形式检索请求的主体。 |
| 10   | **String getAuthType()**<br> 返回用于保护 Servlet 的身份验证方案的名称，例如，"BASIC" 或 "SSL"，如果JSP没有受到保护则返回 null。 |
| 11   | **String getCharacterEncoding()**<br> 返回请求主体中使用的字符编码的名称。 |
| 12   | **String getContentType()**<br> 返回请求主体的 MIME 类型，如果不知道类型则返回 null。 |
| 13   | **String getContextPath()**<br> 返回指示请求上下文的请求 URI 部分。 |
| 14   | **String getHeader(String name)**<br> 以字符串形式返回指定的请求头的值。 |
| 15   | **String getMethod()**<br> 返回请求的 HTTP 方法的名称，例如，GET、POST 或 PUT。 |
| 16   | **String getParameter(String name)**<br> 以字符串形式返回请求参数的值，或者如果参数不存在则返回 null。 |
| 17   | **String getPathInfo()**<br> 当请求发出时，返回与客户端发送的 URL 相关的任何额外的路径信息。 |
| 18   | **String getProtocol()**<br> 返回请求协议的名称和版本。          |
| 19   | **String getQueryString()**<br> 返回包含在路径后的请求 URL 中的查询字符串。 |
| 20   | **String getRemoteAddr()**<br> 返回发送请求的客户端的互联网协议（IP）地址。 |
| 21   | **String getRemoteHost()**<br> 返回发送请求的客户端的完全限定名称。 |
| 22   | **String getRemoteUser()**<br> 如果用户已通过身份验证，则返回发出请求的登录用户，或者如果用户未通过身份验证，则返回 null。 |
| 23   | **String getRequestURI()**<br> 从协议名称直到 HTTP 请求的第一行的查询字符串中，返回该请求的 URL 的一部分。 |
| 24   | **String getRequestedSessionId()**<br> 返回由客户端指定的 session 会话 ID。 |
| 25   | **String getServletPath()**<br> 返回调用 JSP 的请求的 URL 的一部分。 |
| 26   | **String[] getParameterValues(String name)**<br> 返回一个字符串对象的数组，包含所有给定的请求参数的值，如果参数不存在则返回 null。 |
| 27   | **boolean isSecure()**<br> 返回一个布尔值，指示请求是否使用安全通道，如 HTTPS。 |
| 28   | **int getContentLength()**<br> 以字节为单位返回请求主体的长度，并提供输入流，或者如果长度未知则返回 -1。 |
| 29   | **int getIntHeader(String name)**<br> 返回指定的请求头的值为一个 int 值。 |
| 30   | **int getServerPort()**<br> 返回接收到这个请求的端口号。         |
| 31   | **int getParameterMap()**<br> 将参数封装成 Map 类型。            |

#### Servlet 的 Http 响应

当一个 Web 服务器响应一个 HTTP 请求时，响应通常包括一个状态行、一些响应报头、一个空行和文档。

状态行包括 HTTP 版本（在本例中为 HTTP/1.1）、一个状态码（在本例中为 200）和一个对应于状态码的短消息（在本例中为 OK）。

下表总结了从 Web 服务器端返回到浏览器的最有用的 HTTP 1.1 响应报头，您会在 Web 编程中频繁地使用它们：

| 头信息              | 描述                                                         |
| :------------------ | :----------------------------------------------------------- |
| Allow               | 这个头信息指定服务器支持的请求方法（GET、POST 等）。         |
| Cache-Control       | 这个头信息指定响应文档在何种情况下可以安全地缓存。可能的值有：**public、private** 或 **no-cache** 等。Public 意味着文档是可缓存，Private 意味着文档是单个用户私用文档，且只能存储在私有（非共享）缓存中，no-cache 意味着文档不应被缓存。 |
| Connection          | 这个头信息指示浏览器是否使用持久 HTTP 连接。值 **close** 指示浏览器不使用持久 HTTP 连接，值 **keep-alive** 意味着使用持久连接。 |
| Content-Disposition | 这个头信息可以让您请求浏览器要求用户以给定名称的文件把响应保存到磁盘。 |
| Content-Encoding    | 在传输过程中，这个头信息指定页面的编码方式。                 |
| Content-Language    | 这个头信息表示文档编写所使用的语言。例如，en、en-us、ru 等。 |
| Content-Length      | 这个头信息指示响应中的字节数。只有当浏览器使用持久（keep-alive）HTTP 连接时才需要这些信息。 |
| Content-Type        | 这个头信息提供了响应文档的 MIME（Multipurpose Internet Mail Extension）类型。 |
| Expires             | 这个头信息指定内容过期的时间，在这之后内容不再被缓存。       |
| Last-Modified       | 这个头信息指示文档的最后修改时间。然后，客户端可以缓存文件，并在以后的请求中通过 **If-Modified-Since** 请求头信息提供一个日期。 |
| Location            | 这个头信息应被包含在所有的带有状态码的响应中。在 300s 内，这会通知浏览器文档的地址。浏览器会自动重新连接到这个位置，并获取新的文档。 |
| Refresh             | 这个头信息指定浏览器应该如何尽快请求更新的页面。您可以指定页面刷新的秒数。 |
| Retry-After         | 这个头信息可以与 503（Service Unavailable 服务不可用）响应配合使用，这会告诉客户端多久就可以重复它的请求。 |
| Set-Cookie          | 这个头信息指定一个与页面关联的 cookie。                      |

**设置 HTTP 响应报头的方法**

下面的方法可用于在 Servlet 程序中设置 HTTP 响应报头。这些方法通过HttpServletResponse对象可用。

| 序号 | 方法 & 描述                                                  |
| :--- | :----------------------------------------------------------- |
| 1    | **String encodeRedirectURL(String url)**<br> 为 sendRedirect 方法中使用的指定的 URL 进行编码，或者如果编码不是必需的，则返回 URL 未改变。 |
| 2    | **String encodeURL(String url)**<br> 对包含 session 会话 ID 的指定 URL 进行编码，或者如果编码不是必需的，则返回 URL 未改变。 |
| 3    | **boolean containsHeader(String name)**<br> 返回一个布尔值，指示是否已经设置已命名的响应报头。 |
| 4    | **boolean isCommitted()**<br> 返回一个布尔值，指示响应是否已经提交。 |
| 5    | **void addCookie(Cookie cookie)**<br> 把指定的 cookie 添加到响应。 |
| 6    | **void addDateHeader(String name, long date)**<br> 添加一个带有给定的名称和日期值的响应报头。 |
| 7    | **void addHeader(String name, String value)**<br> 添加一个带有给定的名称和值的响应报头。 |
| 8    | **void addIntHeader(String name, int value)**<br> 添加一个带有给定的名称和整数值的响应报头。 |
| 9    | **void flushBuffer()**<br> 强制任何在缓冲区中的内容被写入到客户端。 |
| 10   | **void reset()**<br> 清除缓冲区中存在的任何数据，包括状态码和头。 |
| 11   | **void resetBuffer()**<br> 清除响应中基础缓冲区的内容，不清除状态码和头。 |
| 12   | **void sendError(int sc)**<br> 使用指定的状态码发送错误响应到客户端，并清除缓冲区。 |
| 13   | **void sendError(int sc, String msg)**<br> 使用指定的状态发送错误响应到客户端。 |
| 14   | **void sendRedirect(String location)**<br> 使用指定的重定向位置 URL 发送临时重定向响应到客户端。 |
| 15   | **void setBufferSize(int size)**<br> 为响应主体设置首选的缓冲区大小。 |
| 16   | **void setCharacterEncoding(String charset)**<br> 设置被发送到客户端的响应的字符编码（MIME 字符集）例如，UTF-8。 |
| 17   | **void setContentLength(int len)**<br> 设置在 HTTP Servlet 响应中的内容主体的长度，该方法设置 HTTP Content-Length 头。 |
| 18   | **void setContentType(String type)**<br> 如果响应还未被提交，设置被发送到客户端的响应的内容类型。 |
| 19   | **void setDateHeader(String name, long date)**<br> 设置一个带有给定的名称和日期值的响应报头。 |
| 20   | **void setHeader(String name, String value)**<br> 设置一个带有给定的名称和值的响应报头。 |
| 21   | **void setIntHeader(String name, int value)**<br> 设置一个带有给定的名称和整数值的响应报头。 |
| 22   | **void setLocale(Locale loc)**<br> 如果响应还未被提交，设置响应的区域。 |
| 23   | **void setStatus(int sc)**<br> 为该响应设置状态码。          |

#### Servlet http状态码

HTTP 请求和 HTTP 响应消息的格式是类似的，结构如下：

- 初始状态行 + 回车换行符（回车+换行）
- 零个或多个标题行+回车换行符
- 一个空白行，即回车换行符
- 一个可选的消息主体，比如文件、查询数据或查询输出

**服务器的响应头如下所示：**

```http
HTTP/1.1 200 OK
Content-Type: text/html
Header2: ...
...
HeaderN: ...
  (Blank Line)
<!doctype ...>
<html>
<head>...</head>
<body>
...
</body>
</html>
```

**设置 HTTP 状态代码的方法**

下面的方法可用于在 Servlet 程序中设置 HTTP 状态码。这些方法通过 HttpServletResponse 对象可用。

| 序号 | 方法 & 描述                                                  |
| :--- | :----------------------------------------------------------- |
| 1    | **public void setStatus ( int statusCode )**<br> 该方法设置一个任意的状态码。setStatus 方法接受一个 int（状态码）作为参数。如果您的响应包含了一个特殊的状态码和文档，请确保在使用 *PrintWriter* 实际返回任何内容之前调用 setStatus。 |
| 2    | **public void sendRedirect(String url)**<br> 该方法生成一个 302 响应，连同一个带有新文档 URL 的 *Location* 头。 |
| 3    | **public void sendError(int code, String message)**<br> 该方法发送一个状态码（通常为 404），连同一个在 HTML 文档内部自动格式化并发送到客户端的短消息。 |

#### Servlet 过滤器

Servlet 过滤器是可用于 Servlet 编程的 Java 类，可以实现以下<font color=FF0000>目的</font>：

- 在<font color=FF0000>客户端的请求访问后端资源之前</font>，拦截这些请求。
- 在<font color=FF0000>服务器的响应发送回客户端之前</font>，处理这些响应。

总结：拦截请求和处理响应

**根据规范建议的各种类型的过滤器：**

- 身份验证过滤器（Authentication Filters）。
- 数据压缩过滤器（Data compression Filters）。
- 加密过滤器（Encryption Filters）。
- 触发资源访问事件过滤器。
- 图像转换过滤器（Image Conversion Filters）。
- 日志记录和审核过滤器（Logging and Auditing Filters）。
- MIME-TYPE 链过滤器（MIME-TYPE Chain Filters）。
- 标记化过滤器（Tokenizing Filters）。
- XSL/T 过滤器（XSL/T Filters），转换 XML 内容。

过滤器通过 Web 部署描述符（<font color=FF0000>web.xml</font>）中的 XML 标签来声明，然后映射到您的应用程序的部署描述符中的 Servlet 名称或 URL 模式。

当 Web 容器启动 Web 应用程序时，它会为您在部署描述符中声明的每一个过滤器创建一个实例。

Filter的执行顺序与在web.xml配置文件中的配置顺序一致，一般把Filter配置在所有的Servlet之前。

**Servlet 过滤器方法**

过滤器是一个实现了 javax.servlet.Filter 接口的 Java 类。javax.servlet.Filter 接口定义了三个方法：

| 序号 | 方法 & 描述                                                  |
| :--- | :----------------------------------------------------------- |
| 1    | **public void doFilter (ServletRequest, ServletResponse, FilterChain)**<br> 该方法完成实际的过滤操作，当客户端请求方法与过滤器设置匹配的URL时，Servlet容器将先调用过滤器的doFilter方法。FilterChain用户访问后续过滤器。 |
| 2    | **public void init(FilterConfig filterConfig)**<br> web 应用程序启动时，web 服务器将创建Filter 的实例对象，并调用其init方法，读取web.xml配置，完成对象的初始化功能，从而为后续的用户请求作好拦截的准备工作（filter对象只会创建一次，init方法也只会执行一次）。开发人员通过init方法的参数，可获得代表当前filter配置信息的FilterConfig对象。 |
| 3    | **public void destroy()**<br> Servlet容器在销毁过滤器实例前调用该方法，在该方法中释放Servlet过滤器占用的资源。 |

后面还有示例，由于繁琐且使用较少，这里不再赘述：[Servlet 编写过滤器](https://www.runoob.com/servlet/servlet-writing-filters.html)

#### Servlet Cookie 处理

Cookie 是存储在客户端计算机上的文本文件，并保留了各种跟踪信息。Java Servlet 显然支持 HTTP Cookie。

**识别返回用户包括三个步骤：**

- 服务器脚本向浏览器发送一组 Cookie。例如：姓名、年龄或识别号码等。
- 浏览器将这些信息存储在本地计算机上，以备将来使用。
- <mark>当下一次浏览器向 Web 服务器发送任何请求时，<font color=FF0000>浏览器会把这些 Cookie 信息发送到服务器，服务器将使用这些信息来识别用户。</font></mark>

**Cookie 剖析**

Cookie 通常设置在 HTTP 头信息中（虽然 JavaScript 也可以直接在浏览器上设置一个 Cookie）。设置 Cookie 的 Servlet 会发送如下的头信息：

```http
HTTP/1.1 200 OK
Date: Fri, 04 Feb 2000 21:03:38 GMT
Server: Apache/1.3.9 (UNIX) PHP/4.0b3
Set-Cookie: name=xyz; expires=Friday, 04-Feb-07 22:03:38 GMT; 
                 path=/; domain=runoob.com
Connection: close
Content-Type: text/html
```

<font color=FF0000>Set-Cookie 头包含了一个名称值对、一个 GMT 日期、一个路径和一个域</font>。名称和值会被 URL 编码。expires 字段是一个指令，告诉浏览器在给定的时间和日期之后"忘记"该 Cookie。

**Servlet Cookie 方法**

| 序号 | 方法 & 描述                                                  |
| :--- | :----------------------------------------------------------- |
| 1    | **public void setDomain(String pattern)**<br> 该方法设置 cookie 适用的域，例如 runoob.com。 |
| 2    | **public String getDomain()**<br> 该方法获取 cookie 适用的域，例如 runoob.com。 |
| 3    | **public void setMaxAge(int expiry)**<br> 该方法设置 cookie 过期的时间（以秒为单位）。如果不这样设置，cookie 只会在当前 session 会话中持续有效。 |
| 4    | **public int getMaxAge()**<br> 该方法返回 cookie 的最大生存周期（以秒为单位），默认情况下，-1 表示 cookie 将持续下去，直到浏览器关闭。 |
| 5    | **public String getName()**<br> 该方法返回 cookie 的名称。名称在创建后不能改变。 |
| 6    | **public void setValue(String newValue)**<br> 该方法设置与 cookie 关联的值。 |
| 7    | **public String getValue()**<br> 该方法获取与 cookie 关联的值。  |
| 8    | **public void setPath(String uri)**<br> 该方法设置 cookie 适用的路径。如果您不指定路径，与当前页面相同目录下的（包括子目录下的）所有 URL 都会返回 cookie。 |
| 9    | **public String getPath()**<br> 该方法获取 cookie 适用的路径。   |
| 10   | **public void setSecure(boolean flag)**<br> 该方法设置布尔值，表示 cookie 是否应该只在加密的（即 SSL）连接上发送。 |
| 11   | **public void setComment(String purpose)**<br> 设置cookie的注释。该注释在浏览器向用户呈现 cookie 时非常有用。 |
| 12   | **public String getComment()**<br> 获取 cookie 的注释，如果 cookie 没有注释则返回 null。 |



#### Servlet Session 跟踪

HTTP 是一种<font color=FF0000>**"无状态"**</font>协议，<mark>这意味着每次客户端检索网页时，客户端打开一个单独的连接到 Web 服务器，服务器会自动不保留之前客户端请求的任何记录</mark>。

##### 有三种方式来维持 Web 客户端和 Web 服务器之间的 session 会话

- **Cookies**

  一个 Web 服务器可以分配一个唯一的 session 会话 ID 作为每个 Web 客户端的 cookie，对于客户端的后续请求可以使用接收到的 cookie 来识别。

  <mark>这可能不是一个有效的方法，因为很多浏览器不支持 cookie，所以我们建议不要使用这种方式来维持 session 会话</mark>。

- **隐藏的表单字段**

  <font color=FF0000>一个 Web 服务器可以发送一个隐藏的 HTML 表单字段，以及一个唯一的 session 会话 ID</font>，如下所示：

  ```html
  <input type="hidden" name="sessionid" value="12345">
  ```

  该条目意味着，<font color=FF0000>当表单被提交时，指定的名称和值会被自动包含在 GET 或 POST 数据中。每次当 Web 浏览器发送回请求时，session_id 值可以用于保持不同的 Web 浏览器的跟踪</font>。

  这可能是一种保持 session 会话跟踪的有效方式，但是<font color=FF0000>点击常规的超文本链接（\<a href...>）不会导致表单提交，因此隐藏的表单字段也不支持常规的 session 会话跟踪</font>。

- <font color=FF0000>**URL 重写**</font>

  <font color=FF0000>您可以在每个 URL 末尾追加一些额外的数据来标识 session 会话，服务器会把该 session 会话标识符与已存储的有关 session 会话的数据相关联</font>。

  例如，`http://w3cschool.cc/file.htm;sessionid=12345`，session 会话标识符被附加为 sessionid=12345，标识符可被 Web 服务器访问以识别客户端。

  <font color=FF0000>URL 重写是一种更好的维持 session 会话的方式，它在浏览器不支持 cookie 时能够很好地工作，但是它的缺点是会动态生成每个 URL 来为页面分配一个 session 会话 ID，即使是在很简单的静态 HTML 页面中也会如此</font>。

##### HttpSession 对象

除了上述的三种方式，Servlet 还提供了 <font color=FF0000>HttpSession 接口</font>，<font color=FF0000>该接口提供了一种跨多个页面请求或访问网站时**识别用户以及存储有关用户信息的方式**</font>。

Servlet 容器使用这个接口来创建一个 HTTP 客户端和 HTTP 服务器之间的 session 会话。会话持续一个指定的时间段，跨多个连接或页面请求。

您会通过调用 HttpServletRequest 的公共方法 **getSession()** 来获取 HttpSession 对象，如下所示：

```java
HttpSession session = request.getSession();
```

你需要在向客户端发送任何文档内容之前调用 *request.getSession()*。下面总结了 HttpSession 对象中可用的几个重要的方法：

| 序号 | 方法 & 描述                                                  |
| :--- | :----------------------------------------------------------- |
| 1    | **public Object getAttribute(String name)** <br>该方法返回在该 session 会话中具有指定名称的对象，如果没有指定名称的对象，则返回 null。 |
| 2    | **public Enumeration getAttributeNames()**<br> 该方法返回 String 对象的枚举，String 对象包含所有绑定到该 session 会话的对象的名称。 |
| 3    | **public long getCreationTime()**<br> 该方法返回该 session 会话被创建的时间，自格林尼治标准时间 1970 年 1 月 1 日午夜算起，以毫秒为单位。 |
| 4    | **public String getId()**<br> 该方法返回一个包含分配给该 session 会话的唯一标识符的字符串。 |
| 5    | **public long getLastAccessedTime()**<br> 该方法返回客户端最后一次发送与该 session 会话相关的请求的时间自格林尼治标准时间 1970 年 1 月 1 日午夜算起，以毫秒为单位。 |
| 6    | **public int getMaxInactiveInterval()**<br> 该方法返回 Servlet 容器在客户端访问时保持 session 会话打开的最大时间间隔，以秒为单位。 |
| 7    | **public void invalidate()**<br> 该方法指示该 session 会话无效，并解除绑定到它上面的任何对象。 |
| 8    | **public boolean isNew()**<br> 如果客户端还不知道该 session 会话，或者如果客户选择不参入该 session 会话，则该方法返回 true。 |
| 9    | **public void removeAttribute(String name)**<br> 该方法将从该 session 会话移除指定名称的对象。 |
| 10   | **public void setAttribute(String name, Object value)**<br> 该方法使用指定的名称绑定一个对象到该 session 会话。 |
| 11   | **public void setMaxInactiveInterval(int interval)**<br> 该方法在 Servlet 容器指示该 session 会话无效之前，指定客户端请求之间的时间，以秒为单位。 |



#### Servlet 网页重定向

重定向请求到另一个网页的最简单的方式是使用 response 对象的 sendRedirect() 方法。下面是该方法的定义：

```java
public void HttpServletResponse.sendRedirect(String location) throws IOException 
```

该方法把响应连同状态码和新的网页位置发送回浏览器。您也可以通过把 setStatus() 和 setHeader() 方法一起使用来达到同样的效果：

```java
String site = "http://www.runoob.com" ;
response.setStatus(response.SC_MOVED_TEMPORARILY);
response.setHeader("Location", site); 
```



#### Servlet 自动刷新页面

Java Servlet 提供了一个机制，使得网页会在给定的时间间隔自动刷新。

刷新网页的最简单的方式是使用响应对象的方法 **setIntHeader()**。以下是这种方法的定义：

```java
public void setIntHeader(String header, int headerValue)
```

此方法把头信息 "Refresh" 连同一个表示时间间隔的整数值（以秒为单位）发送回浏览器。

以上关于Servlet的内容摘自：[RUNOOB - Servlet 教程](https://www.runoob.com/servlet/servlet-tutorial.html)



#### Web 容器的作用域

几乎所有web应用容器都提供了四种类似Map的结构：**application / session / request / page**，Jsp或者Servlet通过向着这四个对象放入数据，从而实现Jsp和Servlet之间数据的共享。

##### application：应用

<font color=FF0000>作用范围在服务器一开始执行服务，到服务器关闭为止</font>。Application 的范围最、停留的时间也最久，所以使用时要特别注意不然可能会造成服务器负载越来越重的情况。只要将数据存入application对象，数据的范围范围 (Scope) 就为Application。

具有application范围的对象被绑定到<font color=FF0000>javax.servlet.ServletContext</font>中。在Web应用程序运行期间，所有的页面都可以访问在这个范围内的对象。

##### session：会话

<font color=FF0000>HTTP会话</font>开始到结束这段时间。Session 的<font color=FF0000>作用范围为一段用户持续和服务器所连接的时间</font>，但与服务器断线，这个属性就无效了。

Session 的开始时刻比较容易判断，它从浏览器发出第一个HTTP请求即可认为会话开始。但结束时刻就不好判断了，因为浏览器关闭时并不会通知服务器，所以只能通过如下这种方法判断：如果一定的时间内客户端没有反应，则认为会话结束。Tomcat的默认值为120分钟，但这个值也可以通过HttpSession的setMaxInactiveInterval()方法来设置。

具有session范围的对象被绑定到<font color=FF0000>javax.servlet.http.HttpSession</font>对象中。

另外：<font color=FF0000>Session 的致命弱点是不容易在多台服务器之间共享</font>，所以这也限制了 Session 的使用。这就需要用cookie解决问题

##### request：一次请求

HTTP请求开始到结束这段时间。Request 的<font color=FF0000>范围是指在一JSP 网页发出请求到另一个JSP 网页之间</font>，否则这个属性就失效。一个HTTP请求的处理可能需要多个Servlet合作，而这几个Servlet之间可以通过某种方式传递信息，但这个信息在请求结束后就无效了。
具有request范围的对象被绑定到<font color=FF0000>javax.servlet.ServletRequest</font>对象中。


  要注意的是，因为请求对象对于每一个客户请求都是不同的，所以对于每一个新的请求，都要重新创建和删除这个范围内的对象。

##### page：当前页面

作用范围：<font color=FF0000>当前页面从打开到关闭这段时间，它只能在同一个页面中有效</font>。

具有page范围的对象被绑定到<font color=FF0000>javax.servlet.jsp.PageContext</font>对象中。



#### Session 和 Cookie

会话 ( Session ) ：指用户登录网站后的一系列动作，比如浏览商品添加到购物车并购买。会话跟踪是 Web 程序中常用的技术，用来**跟踪用户的整个会话**。

由于 HTTP 协议是<font color=FF0000>无状态的协议</font>，所以服务端需要记录用户的状态时，就需要用某种机制来识具体的用户。由于 HTTP 协议无状态，并不知道是哪个用户操作的，所以服务端要为特定的用户创建了特定的 Session，用用于标识这个用户。

**只使用 Session 的痛点**：<font color=FF0000>Session 的致命弱点是不容易在多台服务器之间共享</font>，所以这也限制了 Session 的使用。这就需要用 Cookie

**<font color=FF0000>Cookie</font> 通过<font color=FF0000>在客户端记录</font>信息<font color=FF0000>确定用户身份</font>**，**<font color=FF0000>Session</font> 通过<font color=FF0000>在服务器端记录</font>信息<font color=FF0000>确定用户身份</font>**。

**session 的运行依赖 session id，而 <font color=FF0000>session id 是存在 cookie 中的</font>，也就是说，<font color=FF0000>如果浏览器禁用了 cookie ，同时 session 也会失效</font>**（但是可以通过其它方式实现，比如在 url 中传递 session_id）所以说：**session 是基于 cookie 实现的，session 存储在服务器端，sessionId 会被存储到客户端的cookie 中**

**<font color=FF0000>如果客户端的浏览器禁用了 Cookie 怎么办？</font>**一般这种情况下，会使用一种叫做<font color=FF0000>URL重写</font>的技术来<font color=FF0000>进行会话跟踪</font>，即每次HTTP交互，<font color=FF0000>URL后面都会被附加上一个诸如 sid=xxxxx 这样的参数，服务端据此来识别用户</font>。

##### cookie的重要属性

| 属性       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| name=value | 键值对，设置Cookie的名称及相对应的值，都必须是字符串类型<br/>如果值为Unicode字符，需要为字符编码。<br/>如果值为1 -进制数据，则需要使用BASE64编码。 |
| domain     | 指定cookie所属域名，默认是当前域名                           |
| path       | 指定cookie在哪个路径(路由)下生效， 默认是''/'。<br/>如果设置为/abc ，则只有/abc 下的路由可以访问到该cookie,如: /abc/read |
| maxAge     | cookie失效的时间，单位秒。如果为整数，则该cookie在maxAge秒后失效。如果为负数，该cookie为临时cookie，关闭浏览器即失效，浏览器也不会以任何形式保存该cookie。如果为0,表示删除该cookie。 默认为-1。另外，这个方法比expires好用。 |
| expires    | 过期时间，在设置的某个时间点后该cookie就会失效。<br/>一般浏览器的cookie都是默认储存的，当关闭浏览器结束这个会话的时候，这个cookie也就会被删除 |
| secure     | 该cookie是否仅被使用安全协议传输。安全协议有HTTPS, SSL等，在网络上传输数据之前先将数据加密。默认为false。<br/>当secure值为true时，cookie 在HTTP中是无效，在HTTPS中才有效。 |
| httpOnly   | 如果给某个cookie设置了httpOnly属性,则无法通过JS脚本读取到该cookie的信息，但还是能通过Application中手动修改cookie,所以只是在-定程度上可以防止XSS攻击，不是绝对的安全 |

##### Session 认证流程

- 用户<font color=dodgerBlue>第一次请求服务器</font>的时候，服务器根据用户提交的相关信息，创建对应的 Session
- 请求返回时将此 Session 的唯一标识信息 SessionID 返回给浏览器
- <font color=lightSeaGreen>浏览器接收到服务器返回的 SessionID 信息后，会将此信息存入到 Cookie 中，同时 Cookie 记录此 SessionID 属于哪个域名</font>
- 当用户<font color=dodgerBlue>第二次访问服务器</font>的时候，<font color=lightSeaGreen>请求会自动判断此域名下是否存在 Cookie 信息，如果存在自动将 Cookie 信息也发送给服务端，服务端会从 Cookie 中获取 SessionID，再根据 SessionID 查找对应的 Session 信息，如果没有找到说明用户没有登录或者登录失效，如果找到 Session 证明用户已经登录可执行后面操作</font>。

根据以上流程可知，**SessionID 是连接 Cookie 和 Session 的一道桥梁**，大部分系统也是根据此原理来验证用户登录状态。

##### Cookie 和 Session 的区别

- **安全性：** Session 比 Cookie 安全，Session 是存储在服务器端的，Cookie 是存储在客户端的。
- **存取值的类型不同**：<font color=lightSeaGreen>Cookie 只支持存字符串数据，想要设置其他类型的数据，需要将其转换成字符串，Session 可以存任意数据类型</font>
- **有效期不同：** Cookie 可设置为长时间保持，比如我们经常使用的默认登录功能，<font color=FF0000>Session 一般失效时间较短，客户端关闭（默认情况下）或者 Session 超时都会失效</font>。
- **存储大小不同：** <font color=FF0000>单个 Cookie 保存的数据不能超过 4K</font>，<font color=FF0000>**Session 可存储数据远高于 Cookie**</font>，但是当访问量过多，会占用过多的服务器资源。


部分摘自：[傻傻分不清之 Cookie、Session、Token、JWT](https://juejin.im/post/6844904034181070861)，<font color=FF0000>后面还有Token和JWT的内容，建议阅读</font>



#### JWT

##### CheatSheet

<img src="https://s2.loli.net/2023/10/15/XCgYSE3bIe6Mu2V.jpg" alt="" style="zoom:45%;" />



#### redirect & forward

**Forward**：直接转发方式。客户端和浏览器只发出一次请求，Servlet、HTML、JSP 或其它信息资源，由第二个信息资源响应该请求，在请求对象request中，保存的对象对于每个信息资源是共享的。

**Redirect**：间接转发方式。实际是两次HTTP请求，服务器端在响应第一次请求的时候，让浏览器再向另外一个URL发出请求，从而达到转发的目的。

**举个通俗的例子：**

直接转发就相当于：“A找B借钱，B说没有，B去找C借，借到借不到都会把消息传递给A”；

间接转发就相当于："A找B借钱，B说没有，让A去找C借"。

##### **具体阐述：**

- 间接转发方式，有时也叫<font color=FF0000>重定向</font>，它<font color=FF0000>一般用于避免用户的非正常访问</font>。<font color=lightSeaGreen>例如：用户在没有登录的情况下访问后台资源，Servlet可以将该HTTP请求重定向到登录页面，让用户登录以后再访问</font>。在Servlet中，通过调用response对象的SendRedirect()方法，告诉浏览器重定向访问指定的URL，示例代码如下：　

  ```java
  //Servlet中处理get请求的方法
  public void doGet(HttpServletRequest request,HttpServletResponse response){
  //请求重定向到另外的资源
      response.sendRedirect("资源的URL");
  }
  ```

  间接转发请求的过程如下：

  1. 浏览器向Servlet1发出访问请求；
  2. Servlet1调用sendRedirect()方法，将浏览器重定向到Servlet2；
  3. 浏览器向servlet2发出请求；
  4. 最终由Servlet2做出响应。

- **直接转发方式**用的更多一些，一般说的请求转发指的就是直接转发方式。Web应用程序大多会有一个控制器。由控制器来控制请求应该转发给那个信息资源。然后由这些信息资源处理请求，处理完以后还可能转发给另外的信息资源来返回给用户，这个过程就是经典的MVC模式。

  javax.serlvet.RequestDispatcher接口是请求转发器必须实现的接口，由Web容器为Servlet提供实现该接口的对象，通过调用该接口的forward()方法到达请求转发的目的，示例代码如下：

  ```java
  //Servlet里处理get请求的方法
  public void doGet(HttpServletRequest request , HttpServletResponse response){
     //获取请求转发器对象，该转发器的指向通过getRequestDisPatcher()的参数设置
     RequestDispatcher requestDispatcher =request.getRequestDispatcher("资源的URL");
     //调用forward()方法，转发请求      
     requestDispatcher.forward(request,response);    
  }
  ```

  直接转发请求的过程如下：

  1. 浏览器向Servlet1发出访问请求；
  2. Servlet1调用forward()方法，在服务器端将请求转发给Servlet2；
  3. 最终由Servlet2做出响应。

##### 技巧

其实，通过浏览器就可以观察到服务器端使用了那种请求转发方式，当单击某一个超链接时，浏览器的地址栏会出现当前请求的地址，如果服务器端响应完成以后，<mark>发现地址栏的地址变了，则证明是间接的请求转发</mark>。相反，<mark>如果地址没有发生变化，则代表的是直接请求转发或者没有转发</mark>。

**问：直接转发和间接转发的原理及区别是什么？**

答：Forward和Redirect代表了两种请求转发方式：直接转发和间接转发。对应到代码里，分别是RequestDispatcher类的forward()方法和HttpServletRequest类的sendRedirect()方法。

　　对于间接方式，服务器端在响应第一次请求的时候，让浏览器再向另外一个URL发出请求，从而达到转发的目的。它本质上是两次HTTP请求，对应两个request对象。

　　对于直接方式，客户端浏览器只发出一次请求，Servlet把请求转发给Servlet、HTML、JSP或其它信息资源，由第2个信息资源响应该请求，两个信息资源共享同一个request对象。

摘自：[JAVA常见面试题之Forward和Redirect的区别](https://www.cnblogs.com/selene/p/4518246.html)



#### HTTP

**用户单击鼠标后所发生的事件按顺序如下（以访问清华大学的网站为例）:**

1. 浏览器分析链接指向页面的URL（http://www.tsinghua.edu.cn/chn/index.htm）。

2. 浏览器向DNS请求解析www.tsinghua.edu.cn的IP地址。

3. 域名系统DNS解析出清华大学服务器的IP地址。

4. 浏览器与该服务器建立TCP连接（默认端口号为80）。

5. 浏览器发出HTTP请求: GET /chn/index.htm。

6. 服务器通过HTTP响应把文件index.htm发送给浏览器。

7. TCP连接释放。

8. 浏览器解释文件index.htm， 并将Web页显示给用户 。

**常见应用层协议小结**

|  应用程序   | 使用协议 | 熟知端口号 |
| :---------: | :------: | :--------: |
| FTP数据连接 |   TCP    |     20     |
| FTP控制连接 |   TCP    |     21     |
|   TELNET    |   TCP    |     23     |
|    SMTP     |   TCP    |     25     |
|     DNS     |   UDP    |     53     |
|    TFTP     |   UDP    |     69     |
|    HTTP     |   TCP    |     80     |
|    POP3     |   TCP    |    110     |
|    SNMP     |   UDP    |    161     |

摘自：《王道计算机网络》



#### 双向绑定

<font color=FF0000>**单向绑定**</font>：就是把 Model 绑定到 View，当我们用 JavaScript 代码更新 Model 时，View 就会自动更新。

<font color=FF0000>**双向绑定**</font>：就是如果用户更新了 View，Model 的数据也自动被更新了，这种情况就是双向绑定。



#### 面包屑导航

页面路径（英语：Breadcrumb或Breadcrumb Trail/Navigation），又称面包屑导航，是在用户界面中的一种导航辅助。<mark>它是用户一个在程序或文件中确定和转移他们位置的一种方法</mark>。面包屑这个词来自糖果屋这个童话故事；故事中，汉赛尔与葛丽特企图依靠洒下的面包屑找到回家的路。

**页面路径**通常在页面顶部水平出现，一般会位于标题或页头的下方。<mark>它们提供给用户返回之前任何一个页面的链接</mark>（这些链接也是能到达当前页面的路径），<mark>在层级架构中通常是这个页面的父级页面</mark>。页面路径提供给用户回溯到网站首页或入口页面的一条路径，通常是以大于号（>）出现，尽管一些设计是其他的符号（如»）。

它们绝大部分看起来就像这样：首页>分类页>次级分类页 或者 首页>>分类页>>次级分类页。

**一共有三种类型的网站面包屑导航。**

1. 路径型 ( Path )：路径型面包屑是一个动态显示用户到达页面经过的途径。
2. 位置型 ( Location )：位置型面包屑是固定的，显示了页面在网站结构中的位置。
3. 属性型 ( Attribute )：属性型面包屑给出的当前页面的分类信息。

摘自：[维基百科 - 面包屑导航]([https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%8C%85%E5%B1%91%E5%AF%BC%E8%88%AA](https://zh.wikipedia.org/wiki/面包屑导航))



#### 浏览器对象模型 ( BOM )

浏览器对象模型 ( BOM ) 指的是<font color=FF0000>由 Web 浏览器暴露的所有对象组成的表示模型</font>。<font color=FF0000>BOM </font>与 DOM 不同，其<font color=FF0000>既没有标准的实现</font>，<font color=FF0000>也没有严格的定义, 所以浏览器厂商可以自由地实现 BOM</font>。

作为显示文档的窗口，浏览器程序将其视为对象的分层集合。当浏览器分析文档时, 它将创建一个对象的集合，以定义文档，并详细说明它应如何显示。浏览器创建的对象称为文档对象。它是浏览器使用的更大的对象集合的一部分。此浏览器对象集合统称为浏览器对象模型或 BOM。

BOM 层次结构的顶层是窗口对象, 它包含有关显示文档的窗口的信息。某些窗口对象本身就是描述文档和相关信息的对象。

摘自：[维基百科 - 浏览器对象模型](https://zh.wikipedia.org/wiki/%E6%B5%8F%E8%A7%88%E5%99%A8%E5%AF%B9%E8%B1%A1%E6%A8%A1%E5%9E%8B)



#### Static Generated Sites Vs. SSR

##### 前言 & 背景

<font color=LightSeaGreen>JavaScript currently enables you to **build three types of applications**</font>: single-page applications ( SPAs ), <font size=4><font color=FF0000>**pre-rendered**</font> **or** <font color=FF0000>static-generated sites</font></font>, and server-side-rendered applications. <font color=dodgerBlue>SPAs come with many [challenges](https://en.wikipedia.org/wiki/Single-page_application#Challenges_with_the_SPA_model), one of which is search engine optimization (SEO)</font>. <font color=FF0000>**Possible solutions** are to make use of</font> a [**static-site generator**](https://www.smashingmagazine.com/2020/07/differences-static-generated-sites-server-side-rendered-apps/#static-site-generator) or [**server-side rendering**](https://www.smashingmagazine.com/2020/07/differences-static-generated-sites-server-side-rendered-apps/#server-side-rendering) (SSR).

We’ll look at what static generation is, as well as static generation is, as well as <font color=FF0000>**frameworks** that help us create static-generated sites, such as **Gatsby** and **VuePress**</font>. We’ll learn what a server-side-rendered application is, as well as learn about <font color=FF0000>**frameworks** for creating one, such as **Next.js** and **Nuxt.js**</font>. 

##### SSG ( Static-Site Generator ) 是什么

<font color=FF0000>A **static-site generator (SSG) is a <font size=4>software application</font>**</font> that <font color=FF0000>creates HTML pages from templates or components and a given content source</font>. <font color=fuchsia>**Give it some text files and content, and the generator will give you back a complete website**</font>（👀 这句话是重点：只需提供静态内容，比如 markdown 文件及图片，就可以生成一个网站）; this <font color=FF0000>completed website is referred to as a static-generated site</font>. This means that <font color=FF0000>the website’s pages are generated **at build time**</font>, and <font color=FF0000>their contents do not change unless you add new content or components and then **rebuild**</font> — you have to rebuild the website if you want it to be updated with the new content.

下图：*How static-site generation works*

<img src="https://s2.loli.net/2022/07/11/VzSuanfv2YNjegB.png" alt="img" style="zoom:50%;" />

<font color=LightSeaGreen>This approach is good for building applications whose content does not change often</font>. So, you wouldn’t necessarily use it for a website that has to be modified according to the user or one that has a lot of user-generated content. However, <font color=FF0000>a blog or personal website would be an ideal use</font>. Let’s look at some advantages of static-generated sites.

##### SSG 优点

- **Speed**：Because all of your website’s pages and content will be generated at build time, you <font color=FF0000>**do not have to worry about API calls to a server for content**, which will make your website very fast</font>.
- **Deployment**：Once your static site has been generated, you will be left with static files. Hence, it <font color=FF0000>can be easily deployed to a platform</font> such as [Netlify](https://www.netlify.com/).
- **Security**：A static-generated site solely comprises（译：包含） static files, with no database for an attacker to exploit by injecting malicious（译：怀有恶意的） code. So, vulnerability to a cyber attack is minimal.
- **Version control**：You can use version control software ( such as Git ) to manage and track changes to your content. This comes in handy（译：派上用场） when you want to roll back changes made to the content.

##### SSG 缺点

- **If the content changes too quickly, it can be hard to keep up**.
- <font color=FF0000>To update content, you have to rebuild the website</font>.
- <font color=FF0000>**The build time increases according to the size of the application**</font>（👀 这一点没有想到，毕竟 这是正常现象吧... 难道 SSR 可以解决？ ）.

##### 常见的 SSG 应用 Gatsby & VuePress

文中介绍了 [Gatsby](https://github.com/gatsbyjs/gatsby) 和 [VuePress](https://github.com/vuejs/vuepress) 简单的创建项目和使用，这些内容不是重点，略。同时，这些内容，在其 GitHub 主页中 Get Started 中都有介绍... 另外，在阅读这部分的内容时，我还顺带体验了下新出的 [VitePress](https://github.com/vuejs/vitepress) ；另外，VuePress 使用 Vite 开发 [第二版](https://github.com/vuepress/vuepress-next) 了

##### 什么是 SSR

Server-side rendering ( SSR ) is <font color=FF0000>the process of rendering web pages on a server and passing them to the browser ( client-side )</font> , instead of rendering them in the browser. <font color=FF0000>**SSR sends a fully rendered page to the client**</font> ; the client’s JavaScript bundle takes over and enables the SPA framework to operate.

This means that if your application is server-side rendered, the content is fetched from the server and passed to the browser to be displayed to the user. <mark>**Client-side rendering is different**</mark>: The <font color=FF0000>user would have to **navigate to the page before the browser fetches data from the server**</font>, meaning that the <font color=FF0000>**user would have to wait for some seconds** to pass before the browser is served with the content for that page</font>. Applications that have SSR enabled are called server-side-rendered applications.

下图：*How server-side rendering works*

<img src="https://s2.loli.net/2022/07/11/CMYwdWahyHAxecV.png" alt="image-20220711110546200" style="zoom: 40%;" />

<font color=FF0000>This approach **is good if you’re building a complex application that requires user interaction**</font>（👀 这应该是相较于 SSG）, that relies on a database, or whose content changes often. If the content changes often, then users would need to see the updates right away. **The approach is also good for applications** that tailor content（译：定制内容） according to who is viewing it and that store user data such as email addresses and user preferences, while also attending to SEO. An example would be a large e-commerce or social media platform. Let’s look at some of the advantages of SSR for your applications.

##### SSR 优点

- The <font color=FF0000>content is up to date because it is fetched on the go</font>（**译**：随时随地。👀 毕竟资源都在本地？）.
- The <font color=FF0000>website loads quickly</font> because the browser fetches content from the server before rendering it for the user.
- Because the JavaScript is rendered server-side, <font color=FF0000>the user’s device has little bearing</font>（译：没什么影响） <font color=FF0000>on the loading time of the page, making for better performance</font>.

##### SSR 缺点

- More API calls to the server are made, because they’re made per request.
- The website cannot be deployed to a static content delivery network (CDN). 
 > 👀 这个可以通过 ESR 解决，ESR 相关见 [[#ESR 边缘渲染]]

##### 常见的 SSR 框架 Next.js & Nuxt.js

文中介绍了 [Next.js](https://github.com/vercel/next.js) 和 [Nuxt.js](https://github.com/nuxt/framework) （👀 文档中介绍的还是 Nuxt2，这里了链接是 Nuxt3 ） 简单的创建项目和使用，这些内容不是重点，略。同时，这些内容，在其 GitHub 主页中 Get Started 中都有介绍...

##### SSG 和 SSR 的不同之处

| Static-Site Generation                                       | Server-Side Rendering                                        |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Can easily be deployed to a static CDN                       | Cannot be deployed to a static CDN（👀 可通过 ESR 改进）      |
| Content and pages are generated at build time                | <font color=FF0000>Content and pages are generated per request</font> |
| <font color=FF0000>**Content can get stale**</font>（**译：**不新鲜） <font color=FF0000>**quickly**</font> （👀 需打包更新） | <font color=FF0000>Content is always up to date</font>       |
| <font color=FF0000>Fewer API calls</font>, because it only makes them at build time | <font color=FF0000>**Makes API calls each time a new page is visited**</font> |

摘自：[Differences Between Static Generated Sites And Server-Side Rendered Apps](https://www.smashingmagazine.com/2020/07/differences-static-generated-sites-server-side-rendered-apps/)



#### 服务端渲染 ( SSR ) 和 客户端渲染 ( CSR )

##### CSR 客户端渲染概念

解释一：客户端渲染模式下，<font color=FF0000>**服务端把渲染的静态文件给到客户端**</font>，<font color=FF0000>客户端拿到服务端发送过来的文件自己跑一遍 JS ，根据 JS 运行结果，生成相应 DOM，然后渲染给用户</font>。

解释二：html 仅仅作为静态文件，客户端在请求时，服务端不做任何处理，<font color=FF0000>直接以原文件的形式返回给客户端客户端</font>，然后根据 html 上的 JavaScript，生成 DOM 插入 html。

延伸：前端渲染的方式起源于 JavaScript 的兴起，ajax 的大热更是让前端渲染更加成熟，前端渲染真正意义上的实现了前后端分离，前端只专注于 UI 的开发，后端只专注于逻辑的开发，前后端交互只通过约定好的 API 来交互，后端提供 json 数据，前端循环 json 生成 DOM 插入到页面中去。

###### 利弊

- **好处** ： 网络传输数据量小、减少了服务器压力、前后端分离、局部刷新，无需每次请求完整页面、交互好可实现各种效果

- **坏处** ：<font color=FF0000>**不利于 SEO**</font>。<font color=FF0000>爬虫看不到完整的程序源码、首屏渲染慢</font>（渲染前需要下载一堆 js 和 css 等）

##### SSR 服务端渲染概念

解释一：服务端在返回 html 之前，在特定的区域，符号里用数据填充，再给客户端，客户端只负责解析 HTML 。

解释二：服务端渲染的模式下，<font color=FF0000>当用户第一次请求页面时，由服务器把需要的组件或页面渲染成 HTML 字符串，然后把它返回给客户端</font>。<font color=LightSeaGreen>客户端拿到手的，是可以直接渲染然后呈现给用户的 HTML 内容</font>，不需要为了生成 DOM 内容自己再去跑一遍 JS 代码。使用服务端渲染的网站，可以说是 “所见即所得”，页面上呈现的内容，我们在 html 源文件里也能找到。

###### 利弊

- **好处** ：<font color=FF0000 size=4>**首屏渲染快**</font>（ FCP 优化）、<font color=FF0000>**利于 SEO**</font>、可以生成缓存片段，生成静态化文件、节能（对比客户端渲染的耗电）

- **坏处** ：用户体验较差、不容易维护，通常前端改了部分 html 或者 css，后端也需要修改。

摘自：[服务端渲染（SSR)](https://juejin.im/post/6844903731075481608)

##### 发展历程介绍

模版引擎 SSR 时代，略 ...

**CSR ( Client Side Rendering ) 时代**

有了 Ajax 技术后，再加上通过 CDN 缓存静态资源之后，前端 SPA + CSR 渲染有了飞跃式的发展，这种模式前端处理所有逻辑、内容填充和路由，数据加载部分通过 Ajax 从后端获取，因此很好的解决了前后端分工开发的问题。其具体请求时间线可参见下图：

<img src="https://s2.loli.net/2022/07/10/12onN9Yj6WirCyM.png" alt="img" style="zoom: 60%;" />

**SSR 时代 ( Node )**

随着 Node 引领的全栈技术的发展，前端又回到了当初的 SSR 路上，只不过这次的回归是一次螺旋式的上升。首先是 <font color=FF0000>前后端全是 JS 语法，大部分代码都是可复用的</font> ；其次是 SEO 场景友好，服务端渲染好后直接返回最终的 HTML ，<font color=FF0000>减少了白屏等待时间，过多异步请求的导致的性能问题也可下放到服务端解决</font>；也能有效避免多次的数据获取、内容填充，浏览器只绑定相关的 JS 逻辑、事件即可。其具体请求时间线：

<img src="https://s2.loli.net/2022/07/10/c13tChWzjb6xRIS.png" alt="img" style="zoom:60%;" />

ESR ( Edge Side Rendering ) 时代，略。见下面 [[#ESR 边缘渲染]]

摘自：[边缘渲染提速](https://zhuanlan.zhihu.com/p/406527954)

##### 服务端渲染的 优点 总结

- **更好的 SEO：** 因为 SPA 页面的内容是通过 Ajax 获取，而搜索引擎爬取工具并不会等待 Ajax 异步完成后再抓取页面内容，所以在 SPA 中是抓取不到页面通过 Ajax 获取到的内容；而 SSR 是直接由服务端返回已经渲染好的页面（数据已经包含在页面中），所以搜索引擎爬取工具可以抓取渲染好的页面；
- **更快的内容到达时间（首屏加载更快）：**<font color=FF0000>**SPA 会等待所有 Vue 编译后的 js 文件都下载完成后，才开始进行页面的渲染**；文件下载等需要一定的时间等，所以首屏渲染需要一定的时间；**SSR 直接由服务端渲染好页面直接返回显示**</font>，无需等待下载 js 文件及再去渲染等，所以 SSR 有更快的内容到达时间；

##### 服务端渲染的 缺点 总结

- **更多的开发条件限制：**例如，<font color=FF0000>服务端渲染 <font size=4>**只支持 beforCreate 和 created 两个钩子函数**</font></font>，这会导致一些外部扩展库需要特殊处理，才能在服务端渲染应用程序中运行。并且与可以部署在任何静态文件服务器上的完全静态单页面应用程序 SPA 不同，<font color=FF0000>**服务端渲染应用程序，<font size=4>需要处于 Node.js server 运行环境</font>**</font>
- **更多的服务器负载：**<font color=FF0000>在 Node.js 中渲染完整的应用程序，显然会比仅仅提供静态文件的  server 更加大量占用 CPU 资源</font>（ CPU-intensive - CPU 密集)，因此如果你预料在高流量环境 ( high traffic ) 下使用，请准备相应的服务器负载，并明智地采用缓存策略。

摘自：[Vue高频面试题](https://juejin.cn/post/6844904153924239374)



#### NSR ( Native Side Rendering )

##### ChatGPT 的解释

本地端渲染 ( Native side rendering ) 是指在移动应用程序的本地代码层面进行渲染和图形处理的技术。它<font color=red>通常与传统的基于网络的渲染方式（例如 Web视图 和 WebView ）相对</font>。

<font color=LightSeaGreen>在传统的移动应用开发中，UI 和渲染通常由操作系统提供的默认组件或 Web 视图来处理</font>。这些组件使用操作系统的原生渲染引擎来呈现用户界面，并通过网络协议加载和显示远程网页内容。

然而，<font color=red>随着移动设备的性能和图形处理能力的增强，以及用户对更高效、更灵活的用户界面的需求，本地端渲染成为一种常见的选择</font>。本地端渲染通过使用应用程序的本地代码和图形库，直接在设备上执行渲染和图形处理操作。

<font color=dodgerBlue>本地端渲染的优势包括：</font>

1. 性能优化：本地端渲染可以更好地利用设备的硬件资源，例如GPU（图形处理器）来加速图形渲染和动画效果，提供更流畅的用户体验。
2. 离线功能：本地端渲染可以使应用程序具备离线功能，因为应用程序可以在设备上存储和处理数据，并在没有网络连接时继续提供功能。
3. 自定义用户界面：本地端渲染提供了更大的自由度和灵活性，开发者可以根据应用程序的需求自定义用户界面，实现更复杂的布局和交互效果。
4. 安全性：本地端渲染可以提供更高的安全性，因为敏感数据和业务逻辑可以在设备上本地处理，减少了与远程服务器的数据传输和处理。

常见的本地端渲染技术包括使用原生开发语言（如 Java 或 Kotlin 用于 Android，Objective-C 或 Swift 用于 iOS ）结合图形库（如 OpenGL ES 或 Metal ）进行渲染，或使用跨平台框架（如 React Native、Flutter 或 Unity ）来实现本地端渲染。

总而言之，本地端渲染是一种通过移动应用程序的本地代码和图形库来实现渲染和图形处理的技术，可以提供更好的性能、离线功能、自定义用户界面和安全性。

<img src="https://s2.loli.net/2023/05/16/jtlSV3pZYg5sFHW.png" alt="image-20230516092554797" style="zoom:45%;" />



#### ESR 边缘渲染

随着边缘计算的发展，<font color=FF0000>由于 CDN 节点距离用户更近，有更短网络延时的优势</font>；我们 <font color=FF0000>可以将页面进行 <font size=4>**动静拆分**</font>，将 **静态内容** 缓存在 CDN 先快速返回给用户</font>，然后 <font color=FF0000 size=4>**在 CDN 节点上发起** **动态内容** 的请求</font>（👀 是 CDN 发起对静态内容的请求，不是客户端 ），之后 <font color=FF0000 size=4>将 **动态内容** 与 **静态部分** **以流的形式进行拼接**</font>，从而<font color=FF0000>进一步提高了用户的首屏加载时间</font>，尤其在边缘地区或者弱网环境也有能拥有很好的用户体验，此外还<font color=FF0000>减少原先 SSR 服务器压力</font>。

<img src="https://img-blog.csdnimg.cn/b4c2d46ba41944afa183e9d02b8d43ea.png" alt="img" style="zoom:60%;" />

##### 原理和优势

上面也提到：ESR 就是借助边缘计算能力，将返回的内容进行静态、动态部分拆分，并以流的形式返回。静态部分依托 CDN 的缓存能力，优先返回给用户，随后在 CDN 节点上继续发起动态数据请求，并 拼接在静态部分之后，继续流式返回。因此，优势也是显而易见：

- **TTFB ( Time To First Byte ) 很短**：因为静态内容在 CDN 缓存住了，会很快的返回给用户。

- **FP ( First Paint ) 很短**：因为在静态内容返回后，已经可以开始 HTML 的解析，以及  JS、CSS 的下载和执行

- **FMP ( First Meaningful Paint ) 很短**：<font color=FF0000>因为动态内容的请求是在 CDN 发起，相比于客户端与服务端直连，请求减少了 **TCP连接** 和 **网络传输** 开销</font>；而且由于动态部分是以 chunked 形式流式返回，FMP 就会很短，比如搜索网站的第一个搜索结果就会首先绘制出来

##### 应用场景举例

**场景一：将 SSR服务 直接部署在边缘节点，中心服务提供数据接口**

<img src="https://s2.loli.net/2022/07/10/Cm4kdXfWZcat5ET.png" alt="img" style="zoom:60%;" />

**场景二：边缘服务读取缓存的 静态部分HTML ，中心服务提供 动态HTML**

SSR服务 部署在中心，边缘流式返回 HTML内容（利用 HTTP `Transfer-Encoding: chunked` 分块传输机制），需要分离静态与动态部分，具体流程如下图。

- **边缘服务**：请求 静态HTML 并返回，同时请求中心 SSR服务，获取动态内容并返回
- **SSR服务**：去除 静态HTML，把动态部分返回给边缘服务

![img](https://s2.loli.net/2022/07/10/Gd26bf1l9WYLeAV.png)

摘自：[边缘渲染提速 - 青墨书晚风的文章 - 知乎](https://zhuanlan.zhihu.com/p/406527954)



#### 无头CMS ( Headless content management system )

##### CMS 是什么

> A content management system ( CMS ) is <font color=FF0000>**computer software** used to **manage** the creation and modification of digital content</font> (content management). A CMS is typically used for <font color=0000FF>enterprise content management</font> ( ECM ) and <font color=fuchsia>web content management</font> ( WCM ).
>
> <font color=0000FF>ECM</font> typically supports multiple users in a collaborative environment by integrating document management, digital asset management, and record retention.
>
> Alternatively, <font color=fuchsia>WCM</font> is the collaborative authoring for websites and <mark>may include text and embed graphics, photos, video, audio, maps, and program code</mark> that display content and interact with the user. <mark>ECM typically includes a WCM function</mark>. CMS is a web template to create your own website
>
> ##### Structure
>
> <mark style="background: lightskyblue">A CMS typically has **two major components**</mark> : a <font color=FF0000>**content management application**</font> ( CMA ) , <font color=FF0000>as the front-end user interface</font> that allows a user, even with limited expertise（**译**：即使用户专业知识有限（即：不是程序员））, to add, modify, and remove content from a website without the intervention（**译**：干预） of a [webmaster](https://en.wikipedia.org/wiki/Webmaster) ; and a <font color=FF0000>**content delivery application**</font> ( CDA ) , that compiles the content and updates the website.
>
> ##### Installation type
>
> <mark style="background: lightskyblue">There are **two types of CMS installation**</mark> : <font color=FF0000>**on-premises**</font>（译：现场）and <font color=fuchsia>**cloud-based**</font>. On-premises installation means that the <font color=FF0000>CMS software can be installed on the server</font>. This approach is usually taken by businesses that want flexibility in their setup. Notable **CMSs which can be installed on-premises are [Wordpress.org](https://en.wikipedia.org/wiki/WordPress), [Drupal](https://en.wikipedia.org/wiki/Drupal), [Joomla](https://en.wikipedia.org/wiki/Joomla), [ModX](https://en.wikipedia.org/wiki/Modxcms) and others**.
>
> The <font color=fuchsia>**cloud-based**</font> CMS is hosted on the vendor environment（译：供应商环境）. With <font color=fuchsia>this approach the CMS software cannot be modified for the customer</font>. Examples of notable **cloud-based CMSs are [SquareSpace](https://en.wikipedia.org/wiki/Squarespace), [Wordpress.com](https://en.wikipedia.org/wiki/WordPress.com), [Webflow](https://en.wikipedia.org/wiki/Webflow) and [WIX](https://en.wikipedia.org/wiki/Wix.com)** .
>
> 摘自：[wikipedia - Content management system](https://en.wikipedia.org/wiki/Content_management_system)

##### 无头CMS 是什么

**总述**

A **headless Content Management System**, or **headless CMS**, is a <font color=FF0000>**back-end-only** content management system</font> that <font color=FF0000>acts primarily **as a content repository**</font>. A headless CMS <font color=fuchsia>makes content accessible <font size=4>**via an API**</font> for display on any device <font size=4>**without a built-in , front-end or presentation layer**</font></font>. <font color=FF0000>The term **“headless” comes from the concept** of chopping the **“head” ( the front end )** off the “body” ( the back end )</font> .

**详细介绍**

Whereas a <mark>traditional CMS typically **combines the content and presentation layers**</mark> of a website, a headless CMS comprises（译：包含） just the content component and focuses entirely on the administrative interface for content creators, the <font color=FF0000>facilitation</font>（译：促进） <font color=FF0000>of content workflows and collaboration</font>, and the organization of content into taxonomies（译：分类法，这里是复数）. As such, a headless CMS must be combined with a separate presentation layer to handle design, site structure and templates. That combination generally relies on stateless or loosely coupled APIs.

One <mark style="background: lightskyblue">**advantage**</mark> of this decoupled（译：解耦的） approach is that <font color=fuchsia>content can be sent via APIs to multiple display types</font>, like mobile and Internet of Things (IoT) devices, alongside a website. A <mark style="background: aqua">**disadvantage**</mark>, however, is the <font color=FF0000>requirement to **maintain two separate systems for a single site**, which can **require more resources**</font>.

**Cloud-first headless CMSes** are those that were also built with a multitenant（译：多租户） cloud model at their core and whose vendors promote **software as a service** ( **SaaS** ) , <font color=FF0000>promising high availability, scalability, and full management of security, upgrades, and hotfixes on behalf of clients</font>. Similar to how headless CMSes focus on creating content in the backend to be displayed on frontends via APIs, headless commerce（译：贸易） uses the same setup to separate backend product management and navigation from the frontend of a website or other display types, like IoT.

Headless CMS is similar to but distinct from the use of widgets or plugins on a site, like adding an Uber Eats menu or online ordering plugin to a restaurant website.

摘自：[wikipedia - Headless content management system](https://en.wikipedia.org/wiki/Headless_content_management_system)



#### OSS

**对象存储**（ Object storage ) 是一种 计算机数据存储 架构，它<font color=FF0000>**将数据作为对象进行管理**</font>，与其他存储架构不同（如 文件系统 将数据作为文件层次结构进行管理，而[块存储](https://zh.wikipedia.org/wiki/块_(数据存储))则将数据作为扇区和轨道内的块进行管理）。**每个对象 通常包括：数据本身、数量不等的 元数据 和 一个 [全局唯一的标识符](https://zh.wikipedia.org/wiki/通用唯一识别码)** 。对象存储可以在多个层面实现，包括设备层面（对象存储设备）、系统层面和接口层面。在每一种情况下，对象存储都试图实现其他存储架构所不具备的能力，如可由应用程序直接编程的接口，可跨越多个物理硬件实例的命名空间，以及数据管理功能，如[数据复制](https://zh.wikipedia.org/w/index.php?title=数据复制&action=edit&redlink=1)和对象级粒度的数据分发。

<font color=FF0000>对象存储系统允许保留大量的 **非结构化数据**</font>。对象存储的用途包括：在 Facebook 上存储照片，在 Spotify 上存储歌曲，或在在线协作服务（如Dropbox）中存储文件。

摘自：[wikipedia - 对象存储](https://zh.wikipedia.org/zh-cn/%E5%AF%B9%E8%B1%A1%E5%AD%98%E5%82%A8)

##### 阿里云文档的补充

阿里云 对象存储OSS ( Object Storage Service ) 是一款<font color=FF0000>海量、安全、低成本、高可靠 的 云存储服务</font>，可提供 99.9999999999%（12个9）的<font color=FF0000>数据持久性</font>，99.995% 的<font color=FF0000>数据可用性</font>。<font color=FF0000>多种存储类型供选择</font>，全面优化存储成本。

数据存储到 阿里云OSS 以后，您<font color=FF0000>可以选择 *标准存储 ( Standard )* 作为移动应用、大型网站、图片分享 或 热点音视频的主要存储方式</font>；也可以选择成本更低、存储期限更长的 低频访问存储 ( Infrequent Access) 、归档存储 ( Archive )、冷归档存储 ( Cold Archive ) 作为不经常访问数据的存储方式。

##### OSS 相关概念

- **存储类型 ( Storage Class )**：<font color=FF0000>OSS 提供标准、低频访问、归档、冷归档四种存储类型</font>，全面覆盖从热到冷的各种数据存储场景。其中<mark>**标准存储类型**</mark> 提供高持久、高可用、高性能的对象存储服务，能够支持频繁的数据访问；**低频访问存储类型** 适合长期保存不经常访问的数据（平均每月访问频率 1 到 2 次），存储单价低于标准类型；**归档存储类型** 适合需要长期保存（建议半年以上）的归档数据；**冷归档存储** 适合需要超长时间存放的极冷数据。更多信息，请参见[存储类型介绍](https://help.aliyun.com/document_detail/51374.htm#concept-fcn-3xt-tdb)。
- <font color=FF0000>**存储空间 ( Bucket )**</font>：存储空间是您 <font color=FF0000>**用于存储对象 ( Object ) 的容器**</font>，<font color=FF0000>所有的对象都必须隶属于某个存储空间</font>。存储空间具有各种配置属性，包括：地域、访问权限、存储类型等。您可以根据实际需求，创建不同类型的存储空间来存储不同的数据。
- **对象 ( Object )**：<font color=FF0000>**对象是 OSS 存储数据的 基本单元**，也被称为 OSS 的文件</font>。对象 <font color=FF0000>由 元信息 ( Object Meta )、用户数据 ( Data ) 和 文件名 ( Key ) 组成</font>。对象由存储空间内部唯一的 Key 来标识。对象元信息 是一组键值对，表示了对象的一些属性，例如最后修改时间、大小等信息，同时您也可以在元信息中存储一些自定义的信息。
- **地域 ( Region )**：<font color=FF0000>**地域表示 OSS 的数据中心所在物理位置**</font>。您可以根据费用、请求来源等选择合适的地域创建 Bucket。
- **访问域名 ( Endpoint )**：Endpoint <font color=FF0000>**表示 OSS 对外服务的访问域名**</font>。OSS 以 HTTP RESTful API 的形式对外提供服务，当访问不同地域的时候，需要不同的域名。通过内网和外网访问同一个地域所需要的域名也是不同的。更多信息，请参见[各个Region对应的Endpoint](https://help.aliyun.com/document_detail/31837.htm#concept-zt4-cvy-5db)。
- <font color=FF0000>**访问密钥 ( AccessKey )**</font>：AccessKey <font color=FF0000>**简称 AK**</font> ，指的<font color=FF0000>是 **访问身份验证中用到的 AccessKey ID 和 AccessKey Secret**</font> 。<font color=FF0000>OSS 通过使用 AccessKey ID 和 AccessKey Secret 对称加密的方法来 **验证某个请求的发送者身份**</font>。<font color=fuchsia>**AccessKey ID 用于标识用户**</font>；<font color=fuchsia>**AccessKey Secret 是用户用于 加密签名字符串 和 OSS 用来验证签名字符串的密钥，必须保密**</font>。关于获取 AccessKey 的方法，请参见 [获取AccessKey](https://help.aliyun.com/document_detail/53045.htm#task968)。

##### OSS 常见操作

- **创建 Bucket**：<font color=FF0000>在上传文件 ( Object ) 到 OSS 之前，您需要创建一个用于存储文件的 Bucket</font>。<font color=FF0000>Bucket 具有各种配置属性，包括地域、访问权限以及其他元数据</font>。创建 Bucket 的具体操作，请参见 [创建存储空间](https://help.aliyun.com/document_detail/31842.htm#concept-ntj-wx1-5db)。
- **上传文件**：Bucket 创建完成后，您可以通过多种方式上传不同大小的文件。有关上传文件的具体操作，请参见[上传文件](https://help.aliyun.com/document_detail/31886.htm#concept-zx1-4p4-tdb)。
- **下载文件**：文件上传完成后，您可以将文件下载至浏览器默认路径或本地指定路径。有关下载文件的具体操作，请参见[下载文件](https://help.aliyun.com/document_detail/31887.htm#task-2038465)。
- **列举文件**：当您的 Bucket 内存储了大量的文件后，您可以选择列举 Bucket 内的全部或部分文件。有关列举文件的具体操作，请参见[列举文件](https://help.aliyun.com/document_detail/31860.htm#concept-uzd-syy-5db)。
- **删除文件**：当您不再需要保留上传的文件时，您可以手动删除单个或多个文件，也可以通过配置生命周期规则自动删除单个或多个文件。有关删除文件的具体操作，请参见[删除文件](https://help.aliyun.com/document_detail/31862.htm#concept-g42-bhd-5db)。

##### OSS 重要特性

- **版本控制**：版本控制是 <font color=FF0000>针对存储空间 ( Bucket ) 级别的数据保护功能</font>。开启版本控制后，<font color=FF0000>针对数据的覆盖和删除操作将会以历史版本的形式保存下来</font>。您在错误覆盖或者删除文件 ( Object ) 后，<font color=FF0000>能够将 Bucket 中存储的 Object 恢复至任意时刻的历史版本</font>。有关版本控制的更多信息，请参见[版本控制介绍](https://help.aliyun.com/document_detail/109695.htm#concept-jdg-4rx-bgb)。

- **Bucket Policy**：<font color=FF0000>Bucket 拥有者可 **通过 Bucket Policy 授权不同用户以何种权限访问指定的 OSS 资源**</font>。例如您需要进行跨账号或对匿名用户授权访问或管理整个 Bucket 或 Bucket 内的部分资源，或者需要对同账号下的不同 RAM 用户授予访问或管理 Bucket 资源的不同权限，例如只读、读写或完全控制的权限等。有关配置 Bucket Policy 的操作步骤，请参见[通过Bucket Policy授权用户访问指定资源](https://help.aliyun.com/document_detail/85111.htm#concept-ahc-tx4-j2b)。

- **跨区域复制**：跨区域复制 ( Cross-Region Replication ) 是跨不同 OSS 数据中心（地域）的 Bucket 自动、异步（近实时）复制Object，它会将 Object 的创建、更新和删除等操作从源存储空间复制到不同区域的目标存储空间。跨区域复制功能满足 Bucket 跨区域容灾或用户数据复制的需求。有关跨区域复制的更多信息，请参见[跨区域复制](https://help.aliyun.com/document_detail/31864.htm#concept-zjp-31z-5db)。

- **数据加密**

  - **服务器端加密**：<font color=FF0000>**上传文件** 时，OSS 对收到的文件进行加密，再将得到的加密文件持久化保存</font>；<font color=fuchsia>**下载文件** 时，OSS 自动将加密文件解密后返回给用户，并在返回的 HTTP 请求 Header 中，声明该文件进行了服务器端加密</font>。有关服务器端加密的更多信息，请参见 [服务器端加密](https://help.aliyun.com/document_detail/31871.htm#concept-lqm-fkd-5db)。

  - **客户端加密**：<font color=FF0000>将文件上传到 OSS 之前在本地进行加密</font>。有关客户端加密的更多信息，请参见 [客户端加密](https://help.aliyun.com/document_detail/73332.htm#concept-2323737)。

##### OSS 使用方式

###### OSS 提供多种灵活的上传、下载和管理方式

- **通过<font color=FF0000>控制台管理</font> OSS**：OSS 提供了 Web 服务页面，您可以登录[OSS控制台](https://oss.console.aliyun.com/overview)管理您的 OSS 资源。更多信息，请参见 [控制台用户指南](https://help.aliyun.com/document_detail/31893.htm#task-zx1-4p4-tdb)
- **通过 <font color=FF0000>API 或 SDK 管理</font> OSS**：OSS 提供 RESTful API 和各种语言的 SDK 开发包，方便您快速进行二次开发。更多信息，请参见[OSS API参考](https://help.aliyun.com/document_detail/31948.htm#reference-wrz-l2q-tdb)和[OSS SDK参考](https://help.aliyun.com/document_detail/52834.htm#concept-dcn-tp1-kfb)。
- **通过<font color=FF0000>工具管理</font> OSS**：OSS 提供图形化管理工具 ossbrowser、命令行管理工具 ossutil、FTP 管理工具 ossftp 等各种类型的管理工具。更多信息，请参见 [OSS常用工具](https://help.aliyun.com/document_detail/44075.htm#concept-owg-knn-vdb)。
- **通过<font color=FF0000>云存储网关管理</font> OSS**：OSS 的存储空间内部是扁平的，没有文件系统的目录等概念，所有的对象都直接隶属于其对应的存储空间。如果您想要像使用本地文件夹和磁盘那样来使用 OSS 存储服务，可以通过配置云存储网关来实现。更多信息，请参见[云存储网关产品详情页面](https://www.alibabacloud.com/product/cloud-storage-gateway)。

摘自：[什么是对象存储OSS](https://help.aliyun.com/document_detail/31817.html)



#### CDN 加速 VS OSS 传输加速 

阿里云 对象存储 OSS 以 海量、安全、低成本、高可靠 等特点 已经 成为用户存储 静态资源 和 文件 的首要选择，实际使用中 面向全球 各地用户访问 OSS 资源时，访问速度会受到 客户端网络、OSS 的下行带宽、Bucket 地域、访问链路长 等限制出现访问慢的情况。<font color=dodgerBlue>以下主要介绍 CDN 加速 OSS 和 OSS 传输加速的加速方式</font>：

##### 实现原理

**具体实现加速的原理如下：**

- **CDN 加速 OSS** ：是建立并覆盖在承载网之上，由 <mark>遍布全球的边缘节点服务器群组成的分布式网络</mark>。阿里云 CDN 能分担源站压力，<mark>避免网络拥塞，确保在不同区域、不同场景下加速网站内容的分发，提高资源访问速度</mark>。由 CDN 全球广泛分布的边缘节点缓存 OSS 存储的静态数据，从而实现客户端从边缘节点直接获取数据的方式来实现访问的加速。
- **OSS 传输加速**：<mark>利用全球分布的云机房，将全球各地用户对您存储空间 ( Bucket ) 的访问</mark>，<font color=FF0000>**经过 智能路由 解析至就近的接入点，使用 优化后的网络及协议，为云存储互联网的上传、下载提供端到端的加速方案**</font>。

##### 资源加速场景介绍

OSS传输加速 是 针对 OSS 的链路加速，使用 OSS 传输加速后支持 OSS 提供的任意特性。CDN 通过全球边缘节点缓存 OSS 资源，加速同时可降低带宽成本。<font color=FF0000>**OSS传输加速 和 CDN加速 完全是两个不同的产品**</font>，且应对的场景不同，详情请参见 [CDN应用场景](https://help.aliyun.com/document_detail/109895.htm) 和 [传输加速场景](https://help.aliyun.com/document_detail/131312.htm)。

- 如果您的业务是 <font color=FF0000>第三方数据源加速</font>，推荐您 <font color=fuchsia>使用 CDN 加速</font>。
- 如果您的 <font color=FF0000>OSS 资源需要进行 **多次下载** 的操作</font>，并且 <font color=FF0000>**不要求数据强一致性**</font>，推荐您 <font color=fuchsia>**使用 CDN 加速**</font>。
- 如果您的 <font color=FF0000>OSS 资源需要 **加速下载**</font>，并且 <font color=FF0000>**访问量少**</font>，推荐您 <font color=FF0000>**使用 OSS 传输加速**</font>。
- 如果您的<font color=FF0000> OSS 资源需要 **进行多次下载的操作**</font>，并且 <font color=FF0000>**要求数据强一致性**</font>，推荐您 <font color=FF0000>**使用 OSS 传输加速**</font>。
- 如果您的 <font color=FF0000>业务存储的是 **动态资源**</font>，且 <font color=FF0000>**数据更新频繁**</font>，推荐您 <font color=FF0000>**使用 OSS 传输加速**</font>。
- 如果您的 <font color=FF0000>业务存储的是 **静态资源**</font>，且 <font color=FF0000>**更新少**</font>，推荐您 <font color=fuchsia>**使用 CDN 加速**</font>。

##### CDN加速 和 OSS传输加速 的对比

| **加速方式** | **实现方法**                                                 | **应用场景**                                                 | **优点**                                                     | **缺点**                                                     |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| CDN加速OSS   | 通过全球分布的边缘节点缓存数据来实现加速。                   | 网站或应用中小文件大文件的下载视音频点播                     | CDN 边缘节点全球分布，数量多。CDN 节点提供的服务带宽量大。   | <font color=FF0000>对于访问量大的资源，命中率高，访问量小的资源命中率低，节点未缓存的情况下，还是需要回源访问</font>，回源依赖实时的公网回源链路。CDN 静态资源的访问，对于上传、删除等动态请求加速效果不明显。 |
| OSS传输加速  | 实现的是：<font color=FF0000>**客户端到 OSS 服务端之间链路优化来实现的加速功能**</font>，实际每次资源的请求还是从 OSS 来进行获取。 | 远距离数据传输加速 GB、TB 级大文件上传和下载非静态、非热点数据下载加速 | OSS 存储节点全球主要区域分布。远距离以及大文件的上传和下载加速。 | <font color=FF0000>**所有的访问都是回源到 OSS 访问**</font>，占用 OSS 的服务带宽。同一区域大量用户集中访问资源的情况下，效果没有 CDN 加速效果好。<font color=FF0000>只能使用 HTTPS 方式访问</font>。 |

摘自：[CDN加速和OSS传输加速的区别](https://help.aliyun.com/document_detail/305994.html)



#### 基本式JPEG 和 渐进式JPEG

##### 基本式 ( Baseline ) JPEG

这种类型的 JPEG 文件存储方式是按从上到下的扫描方式，把每一行顺序的保存在 JPEG 文件中。打开这个文件显示它的内容时，数据将按照存储时的顺序从上到下一行一行的被显示出来，直到所有的数据都被读完，就完成了整张图片的显示。如果文件较大或者网络下载速度较慢，那么就会看到图片被一行行加载的效果，这种格式的 JPEG 没有什么优点；<font color=dodgerBlue>因此，一般都推荐使用 Progressive JPEG</font>。

##### 渐进式 ( Progressive ) JPEG

<font color=dodgerBlue>和 Baseline 一遍扫描不同</font>，<font color=red>Progressive JPEG 文件 **包含多次扫描**，这些扫描顺寻的存储在 JPEG 文件中。打开文件过程中，会先显示整个图片的模糊轮廓，随着扫描次数的增加，图片变得越来越清晰</font>。这种格式的主要优点是在网络较慢的情况下，可以看到图片的轮廓知道正在加载的图片大概是什么。在一些网站打开较大图片时，你就会注意到这种技术。

![](https://s2.loli.net/2023/02/25/jS5QBNPC3UGgLVI.gif)

渐进式图片带来的好处是可以让用户在没有下载完图片就可以看到最终图像的大致轮廓，一定程度上可以提升用户体验。（瀑布流的网站建议还是使用标准型的）

![img](https://s2.loli.net/2023/02/25/ARcjbmSpJ3l5ir1.jpg)

另外渐进式的图片的大小并不会和基本的图片大小相差很多，有时候可能会比基本图片更小。渐进式的图片的缺点就是吃用户的 CPU 和内存，不过对于现在的电脑来说这点图片的计算并不算什么。

##### 图片如何保存为Progressive JPEG

###### PhotoShop

在PS中有“存储为web所用格式”，打开后选择“连续”就是渐进式JPEG。

###### Linux

检测是否为 progressive jpeg ： `identify -verbose filename.jpg | grep Interlace` ；如果输出 None 说明不是 progressive jpeg；如果输出 Plane 说明是 progressive jpeg 。

将 basic jpeg 转换成 progressive jpeg ：`convert infile.jpg -interlace Plane outfile.jpg`

> 👀 下面还有使用 Python、php、jpegtran、C# 等工具转换为响应式图片的介绍，这里略；详见原文

> 💡知乎问题：[medium.com 是如何让图片加载时从模糊到清晰的？ - 知乎](https://www.zhihu.com/question/40757342) 也有讨论。其中有提到，medium 实现渐进式图片方法的博文：[How Medium does progressive image loading](https://jmperezperez.com/blog/medium-image-progressive-loading-placeholder/)

学习自：[使用渐进式JPEG来提升用户体验](https://www.biaodianfu.com/progressive-jpeg.html)




#### 前端路由 VS 后端路由

- **后端路由: **对于普通的网站，<font color=FF0000>所有的超链接都是 URL 地址</font>，<font color=FF0000>所有的URL地址都对应服务器上对应的资源</font>
- **前端路由: **对于<font color=FF0000>单页面应用程序</font>来说，主要<font color=FF0000>通过 URL 中的 hash ( # ) 来实现不同页面之间的切换（与锚点相似）</font>。同时，hash 有一个特点: <font color=FF0000>HTTP 请求中不会包含 hash 相关的内容</font>；所以，单页面程序中的页面跳转主要用 hash 实现
- 在单页面应用程序中，<font color=FF0000>**这种通过 hash 改变来切换页面的方式，称作前端路由**</font>（ 区别于后端路由）



#### CORS

CORS 是一个 W3C 标准，全称是 “跨域资源共享” ( Cross-origin resource sharing ) 。

<font color=FF0000>它允许浏览器向跨源服务器，发出 XMLHttpRequest 请求，从而克服了 AJAX 只能同源使用的限制。</font>

##### 补充

跨源资源共享 CORS （或通俗地译为跨域资源共享）是一种基于 HTTP 头的机制，该机制通过允许服务器标示除了它自己以外的其它origin（域，协议和端口），这样浏览器可以访问加载这些资源

出于安全性，浏览器限制脚本内发起的跨源HTTP请求。跨源域资源共享（ CORS ）机制允许 Web 应用服务器进行跨源访问控制，从而使跨源数据传输得以<font color=FF0000>安全进行</font>。现代浏览器支持在 API 容器中（例如 XMLHttpRequest 或 Fetch ）使用 CORS，以降低跨源 HTTP 请求所带来的风险。

##### 什么情况下需要 CORS ？

这份 [cross-origin sharing standard](http://www.w3.org/TR/cors/) 允许在下列场景中使用跨站点 HTTP 请求：

- 前文提到的由 XMLHttpRequest 或 Fetch 发起的跨源 HTTP 请求。
- Web 字体 ( CSS 中通过 `@font-face` 使用跨源字体资源)，因此，网站就可以发布 TrueType 字体资源，并只允许已授权网站进行跨站调用。
- WebGL 贴图

- 使用 drawImage（方法）将 Images/video 画面绘制到 canvas



<font color=FF0000>CORS 需要浏览器和服务器同时支持</font>。目前，所有浏览器都支持该功能，IE 浏览器不能低于 IE10。

<font color=FF0000>整个 CORS 通信过程，都是浏览器自动完成，不需要用户参与</font>。对于开发者来说，CORS 通信与同源的 AJAX 通信没有差别，代码完全一样。浏览器一旦发现 AJAX 请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。

因此，实现 CORS 通信的关键是服务器。只要服务器实现了CORS 接口，就可以跨源通信。

##### 简单请求 & 非简单请求

**浏览器将CORS请求分成两类：简单请求 ( simple request ) 和 非简单请求 ( not-so-simple request )。**

只要<font color=FF0000>同时满足以下两大条件，就属于简单请求</font>。

- 请求方法是以下三种方法之一：
  - HEAD
  - GET
  - POST
- 除了被用户代理自动设置的首部字段（例如 Connection，User-Agent）和在 Fetch 规范中定义为 禁用首部名称的其他首部，允许人为设置的字段为 Fetch 规范定义的 对 CORS 安全的首部字段集合该集合为：
  - Accept
  - Accept-Language
  - Content-Language
  - Content-Type的值仅限于下列三者之一：
    - text/plain
    - multipart/form-data
    - application/x-www-form-urlencoded
- 请求中的任意XMLHttpRequestUpload 对象均没有注册任何事件监听器；XMLHttpRequestUpload 对象可以使用 XMLHttpRequest.upload 属性访问。
- 请求中没有使用 ReadableStream 对象。

**凡是<font color=FF0000>不同时满足上面两个条件，就属于非简单请求</font>。**

**具体讲解：**

- **简单请求**

  对于简单请求，浏览器直接发出CORS请求。具体来说，就是在头信息之中，增加一个**`Origin`**字段。`Origin`字段用来说明，本次请求来自哪个源（协议 + 域名 + 端口）。服务器根据这个值，决定是否同意这次请求。

- **非简单请求**

  非简单请求是那种对服务器有特殊要求的请求，比如请求方法是PUT或DELETE，或者Content-Type字段的类型是application/json。

  <mark>非简单请求的CORS请求，会<font color=FF0000>在正式通信之前，增加一次HTTP查询请求，称为**"预检"请求**（preflight）</font></mark>。

  浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些HTTP动词和头信息字段。只有得到肯定答复，浏览器才会发出正式的XMLHttpRequest请求，否则就报错

  <font color=FF0000>一旦服务器通过了"预检"请求，以后每次浏览器正常的CORS请求，就都跟简单请求一样</font>，会有一个Origin头信息字段。服务器的回应，也都会有一个Access-Control-Allow-Origin头信息字段。
  **补充：**

  规范要求，对那些可能对服务器数据产生副作用的 HTTP 请求方法（特别是 GET 以外的 HTTP 请求，或者搭配某些 MIME 类型的 POST 请求），<mark>浏览器必须首先使用 OPTIONS 方法发起一个预检请求（preflight request），从而获知服务端是否允许该跨源请求。服务器确认允许之后，才发起实际的 HTTP 请求</mark>。在预检请求的返回中，服务器端也可以通知客户端，是否需要携带身份凭证（包括 Cookies 和 HTTP 认证相关数据）。
  CORS请求失败会产生错误，但是为了安全，在JavaScript代码层面是无法获知到底具体是哪里出了问题。你只能查看浏览器的控制台以得知具体是哪里出现了错误。

摘自：[跨域资源共享 CORS 详解](https://www.ruanyifeng.com/blog/2016/04/cors.html)  [MDN - 跨源资源共享（CORS）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)(另外，这其中还有很多没有看懂的，需要继续阅读)



#### SRI ( Subresource Integrity )

**子资源完整性** ( SRI ) 是允许<font color=FF0000>浏览器检查其获得的资源（例如从 CDN 获得的）是否被篡改</font>的一项安全特性。它<font color=FF0000>通过验证 **获取文件的哈希值是否和你提供的哈希值一样** 来判断资源是否被篡改</font>。

<font color=lightSeaGreen>使用 **内容分发网络** ( CDNs ) 在多个站点之间共享脚本和样式表等文件可以提高站点性能并节省带宽。然而，使用 CDN 也存在风险，如果攻击者获得对 CDN 的控制权，则可以将任意恶意内容注入到 CDN 上的文件中（或完全替换掉文件），因此可能潜在地攻击所有从该 CDN 获取文件的站点</font>。子资源完整性通过确保 Web 应用程序获得的文件未经第三方注入或其他任何形式的修改来降低这种攻击的风险。

<font color=dodgerBlue>需要注意的是</font>：SRI 并不能规避所有的风险

##### 如何使用 SRI

将使用 base64 编码过后的文件哈希值写入你所引用的 \<script> 或 \<link> 标签的 integrity 属性值中即可启用子资源完整性功能。

integrity 值分成两个部分，第一部分指定哈希值的生成算法（目前支持 sha256、sha384 及 sha512），第二部分是经过 base64 编码的实际哈希值，两者之间通过一个短横（-）分割。

###### 生成 SRI 哈希的工具

你可以用 openssl 在命令行中执行如下命令来生成 SRI 哈希值：

```sh
cat FILENAME.js | openssl dgst -sha384 -binary | openssl enc -base64 -A         
```

或者用 **shasum** 在命令行中执行：

```sh
shasum -b -a 384 FILENAME.js | xxd -r -p | base64
```

###### 内容安全策略及子资源完整性

你可以根据内容安全策略来配置你的服务器使得指定类型的文件遵守 SRI。这是通过在 CSP 头部添加 require-sri-for 指令实现的：

```http
Content-Security-Policy: require-sri-for script;
```

这条指令规定了所有 JavaScript 都要有 **integrity** 属性，且通过验证才能被加载。

你也可以指定所有样式表也要通过 SRI 验证：

```http
Content-Security-Policy: require-sri-for style;
```

你也可以对两者都加上验证。

摘自：[MDN - Subresource Integrity](https://developer.mozilla.org/zh-CN/docs/Web/Security/Subresource_Integrity)



#### MIME

##### MIME 简介

<font color=FF0000>MIME, Mutipurpose Internet Mail Extensions，多用途 Internet 邮箱扩展</font>。<font color=FF0000>MIME 是描述消息内容类型的 internet 标准</font>。<mark>在创建之初，是为了在发送电子邮件时附加多媒体数据，让邮件客户程序根据其类型进行处理</mark>。<font color=FF0000>现在 MIME TYPE 被 HTTP 协议支持后，使得HTTP能够传输各种各样的文件</font>。

##### 浏览器与 MIME-TYPE

浏览器通过 MIME TYPE,也就是该资源的媒体类型，来决定以什么形式显示数据。

<font color=lightSeaGreen>媒体类型通常是通过 HTTP 协议，由 Web 服务器请求头中的 Content-Type 来告知浏览器数据类型的</font>，比如：

```http
Content-Type: text/HTML
```

表示内容是 text/HTML 类型，也就是超文本文件。注意，必须是 "text/HTML" 而不是 "HTML/text".因为 MIME 是经过 ietf 组织协商，以 RFC 的形式发布在网上的。

##### 自定义的类型

需要注意的是：<font color=FF0000>**只有一些在互联网上获得广泛应用的格式才会获得一个 MIME Type**</font>，<font color=FF0000>如果是某个客户端**自己定义的格式**，**一般只能以 application/x- 开头**</font>。

Internet 中有一个专门组织来对 MIME 标准进行修订，但是由于 Internet 发展过快，很多应用程序便使用在类别中以 x- 开头的方法标识这个类别还没有成为标准，例如 x-gzip,x-tar等。

其实是不是标准无关紧要，只要客户端和服务器都能识别这个格式就可以了。在 app 端会使用自定义标准来保证数据安全。

MIME类型与文档的后缀相关，因此服务器使用文档的后缀来区分不同文件的 MIME 类型，服务器中必须规定文件后缀和MIME类型之间的对应关系。而客户端从服务器上接收数据的时候，它只是从服务器接收数据流，并不了解文档的名字，因此服务器需要使用附加信息来告诉客户程序数据的 MIME 类型。服务器将首先发送以下两行 MIME 标识信息，这个信息并不是真正的数据文件的一部分。

```http
Context-type: text/html
```

注意，第二行为一个空格，这是必须的，使用这个空行的目的是将 MIME 信息与真正的数据内容分离开。

##### MIME TYPE语法 及常见分类

<font color=FF0000>通用结构：`type/subtype`</font>

<font color=FF0000>MIME 类型对大小写不敏感，但是通常传统写法是小写。</font>
 
 ###### 分类

| 分类                                  | 描述                                           | 典型类型                                                     |
| ------------------------------------- | ---------------------------------------------- | ------------------------------------------------------------ |
| <font color=FF0000>text</font>        | <font color=FF0000>表明是普通文本</font>       | text/plain, text/html, text/css, text/javascript             |
| image                                 | 表示是某种图像，不包括视频文件，但是包括动态图 | image/gif /image/png, image/jpeg, image/bmp, image/webp      |
| audio                                 | 音频文件                                       | audio/midi, audio/mpeg, audio/webm, audio/ogg, audio/wav,    |
| video                                 | 表示某种视频文件                               | video/webm, video/ogg                                        |
| <font color=FF0000>application</font> | <font color=FF0000>表示某种二进制数据</font>   | application/octet-stream,/pkcs12, application/vnd.mspowerpoint, application/xhtml+xml, application/xml,  application/pdf,application/json |

<font color=FF0000>对于 text 文件类型若是没有特定的 subtype，就使用 text/plain</font>；类似的，<font color=FF0000>二进制文件如果没有特定或已知的 subtype，就使用 application/octet-stream</font>.

**重要的 MIME 类型**

- **text/plain：**<font color=FF0000>文本文件默认值，意思是未知的文本文件，浏览器认为是可以直接展示的</font>。

- **text/css：**<font color=FF0000>任何一个 CSS 文件想要在网页上被解释执行就必须设为 text/css 文件</font>。<mark>如果服务器将 MIME 类型设置为 text/plain 或 application/octet-stream 发送，这种情况下，文件并不能被浏览器识别为 CSS 文件并且会被直接忽略</mark>。

- **text/html：**所有的 HTML 内容都应该使用这种格式。

摘自：[网络：什么是 MIME TYPE?](https://www.jianshu.com/p/24c5433ce31b)

##### MDN 补充

互联网号码分配机构（IANA）是负责跟踪所有官方MIME类型的官方机构

**重要：**<font color=FF0000>浏览器通常使用MIME类型（而不是文件扩展名）来确定如何处理URL，因此Web服务器在响应头中添加正确的MIME类型非常重要</font>。<font color=FF0000>如果配置不正确，浏览器可能会曲解文件内容，网站将无法正常工作，并且下载的文件也会被错误处理</font>。

摘自：[MDN - MIME 类型](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_types)

<font size=4>**补充二：**</font>

阮一峰的《MIME笔记》<font color=FF0000>（提示，由于文章是2008年写的，所以应该会有些内容是过时的，或不完整的）</font>

MIME是对传统电子邮件的一个扩展，现在已经成为电子邮件实际上的标准。

<mark>传统的电子邮件是1982年定下技术规范的，文件是RFC 822</mark>。

<font color=FF0000>它的一个重要特点，就是规定电子邮件只能使用ASCII字符。这导致了三个结果</font>：<font color=FF0000>1）非英语字符都不能在电子邮件中使用；2）电子邮件中不能插入二进制文件（如图片）；3）电子邮件不能有附件</font>。

这实际上无法接受的，因此到了1992年，工程师们决定扩展电子邮件的技术规范，提出一系列补充规范，这就是MIME的由来。

**MIME对传统电子邮件的扩展，表现在它在信件头部分添加了几条语句，主要有三条。**

- **第一条是：**

  ```http
  MIME-Version: 1.0
  ```

  这条语句是必须的，而且1.0这个版本值是不变的，即使MIME本身已经升级了好几次。

  有了这条语句，收信端就知道这封信使用了MIME规范。

- **第二条语句是：**

  ```http
  Content-Type: text/plain; charset="ISO-8859-1"
  ```

  <font color=FF0000>这一行是极端重要的，它表明传递的信息类型和采用的编码</font>。

  <font color=FF0000>Content-Type表明信息类型，<font size=4>**缺省值为" text/plain"**</font>。**它包含了主要类型（primary type）和次要类型（subtype）两个部分**，两者之间用"/"分割</font>。主要类型有9种，分别是application、audio、example、image、message、model、multipart、text、video。

  <font color=FF0000 size=4>**如果信息的主要类型是"text"，那么还必须指明编码类型"charset"，缺省值是ASCII**</font>，其他可能值有"ISO-8859-1"、"UTF-8"、"GB2312"等等。

  整个Content-Type这一行，不仅使用在电子邮件，后来也被移植到了HTTP协议中，所以现在只要是在网上传播的HTTP信息，都带有Content-Type头，以表明信息类型。

- <mark>电子邮件的传统格式不支持非ASCII编码和二进制数据，因此**MIME规定了第三条语句**：</mark>

  ```http
  Content-transfer-encoding: base64
  ```

  这条语句指明了编码转换的方式。Content-transfer-encoding的值有5种----"7bit"、"8bit"、"binary"、"quoted-printable"和"base64"----其中"7bit"是缺省值，即不用转化的ASCII字符。真正常用是"quoted-printable"和"base64"两种

  <font color=FF0000>**补充：**不知道为什么Content-transfer-encoding这个属性，在mdn中没有搜到，在搜索引擎中搜索结果也很少</font>

摘自：[阮一峰 - MIME笔记](https://www.ruanyifeng.com/blog/2008/06/mime.html)



#### HTML 和 XML 的关系

<font color=FF0000>当初 HTML 是 SGML 的一个应用</font>（ 意思是 SGML 可以有许多应用，比如 docbook 最初也是 SGML 的一个应用），而 <font color=FF0000>XML 是简化了的 SGML 并用来取代 SGML 的</font>。而 <font color=FF0000>XHTML 就是 HTML 从 SGML 转用 XML 语法的结果</font>。所以它们是有关系，但不是同个层面的东西。

注意，<mark>今天的 HTML 已经不是当初意义上的 HTML 了</mark>（包含了当初的 html / xhtml / dom 等许多标准和api，而不是单单一个语言词汇集的定义），所以关系又弱了一层。

摘自：[xml跟html有关系吗？ - 贺师俊的回答 - 知乎](https://www.zhihu.com/question/28317309/answer/40400863)



#### 浏览器指纹

通过 Cookie 来跟踪识别你的身份已经不是什么新技术了，也已经有了很多反跟踪的方法。但是现在存在一种方法，无需广告商为你设置标识，只需读取你的浏览器信息，就可以确定你的身份。这种方法称为 “**浏览器指纹**”。

每个人的浏览器似乎都是一样的，但实际上，几乎每个浏览器提供的信息都是不同的。

**浏览器能提供以下一些信息：**

- 是否开启了Cookie
- 超级Cookie限制
- 触屏支持
- “请勿跟踪”
- 平台
- 时区
- 屏幕大小和颜色深度
- 系统字体
- 用户代理
- 语言
- WebGL指纹
- HTTP_ACCEPT头
- 浏览器插件
- Canvas指纹

将所有这些信息结合起来，就成为了我这个浏览器独一无二的指纹。

摘自：[你是如何被广告跟踪的？](https://zhuanlan.zhihu.com/p/34591096)



#### 单页面应用（SPA）

单页应用（英语：single-page application，缩写SPA）是一种网络应用程序或网站的模型，它<font color=FF0000> 通过动态重写当前页面来与用户交互</font>，而非传统的从服务器重新加载整个新页面。<font color=FF0000> 这种方法避免了页面之间切换打断用户体验，使应用程序更像一个桌面应用程序</font>。在单页应用中，<font color=FF0000> 所有必要的代码（HTML、JavaScript和CSS）都通过单个页面的加载而检索，或者根据需要（通常是为响应用户操作）动态装载适当的资源并添加到页面</font>。尽管可以用位置散列或HTML5历史API来提供应用程序中单独逻辑页面的感知和导航能力，但<font color=FF0000> 页面在过程中的任何时间点都不会重新加载，也不会将控制转移到其他页面</font>。与单页应用的交互通常涉及到与网页服务器后端的动态通信。

摘自：[wiki - 单页应用](https://zh.wikipedia.org/wiki/单页应用)

简单的说 SPA 就是<font color=FF0000>一个WEB项目只有一个 HTML 页面</font>，<font color=FF0000>一旦页面加载完成，SPA 不会因为用户的操作而进行页面的重新加载或跳转</font>。 取而代之的是<font color=FF0000>利用 JS 动态的变换 HTML 的内容，从而来模拟多个视图间跳转</font>。

摘自：[「前端进阶」彻底弄懂前端路由](https://juejin.im/post/6844903890278694919)


#### Comet （web技术）

Comet是一种用于 <font color=FF0000> web的推送技术，能使服务器实时地将更新的信息传送到客户端，而无须客户端发出请求</font>，目前<font color=FF0000> **有两种实现方式，长轮询和iframe流**</font>。

**实现方式**

- **长轮询：**长轮询是<font color=FF0000> 在打开一条连接以后保持，等待服务器推送来数据再关闭的方式</font>。

- **iframe流：**iframe流方式是<font color=FF0000> **在页面中插入一个隐藏的iframe**，利用其src属性在服务器和客户端之间建立一条长链接</font>，<font color=FF0000> 服务器向iframe传输数据</font>（通常是HTML，内有负责插入信息的javascript），<font color=FF0000> 来实时更新页面</font>。 <mark>iframe流方式的优点是浏览器兼容好</mark>，Google公司在一些产品中使用了iframe流，如Google Talk。

在HTML5标准中，定义了客户端和服务器通讯的WebSocket方式，在得到浏览器支持以后，<font color=FF0000 size=4> **WebSocket将会取代Comet成为服务器推送的方法**</font>，目前Google Chrome、Firefox、Opera、Safari等主流版本均支持，Internet Explorer从10开始支持。

摘自：[wiki - Comet (web技术)](https://zh.wikipedia.org/wiki/Comet_(web%E6%8A%80%E6%9C%AF))



#### WebDAV（基于Web的分布式编写和版本控制）

**基于Web的分布式编写和版本控制**（WebDAV）<font color=FF0000> 是超文本传输协议（HTTP）的扩展</font>，<font color=FF0000> 有利于用户间**协同编辑**和**管理存储**在万维网服务器文档</font>。WebDAV由互联网工程任务组的工作组在RFC 4918中定义。

<font color=FF0000> **WebDAV协议为用户在服务器上创建、更改和移动文档提供了一个框架**</font>。WebDAV协议最重要的功能包括<mark style="background:aqua">**维护作者或修改日期的属性**</mark>、<mark>**命名空间管理**</mark>、<mark style="background:fuchsia">**集合和覆盖保护**</mark>。

- <mark style="background:aqua">**维护属性**</mark>包括创建、删除和查询文件信息等。
- <mark>**命名空间管理**</mark>处理在服务器名称空间内复制和移动网页的能力。
- <mark style="background:fuchsia">**集合**</mark>（Collections）处理各种资源的创建、删除和列举。<mark style="background:fuchsia">**覆盖保护**</mark>处理与锁定文件相关的方面。

许多现代操作系统为WebDAV提供了内置的客户端支持。

<font size=4>**实现**</font>

WebDAV 扩展了 request 方法所允许的标准 HTTP谓词 和 HTTP头。增加的谓词包括：

- COPY：将资源从一个URI复制到另一个URI
- LOCK：锁定一个资源。WebDAV支持共享锁和互斥锁。
- MKCOL：创建集合（即目录）
- MOVE：将资源从一个URI移动到另一个URI
- PROPFIND：从Web资源中检索以XML格式存储的属性。它也被重载，以允许一个检索远程系统的集合结构（也叫目录层次结构）。
- PROPPATCH：在单个原子性动作中更改和删除资源的多个属性
- UNLOCK：解除资源的锁定

摘自：[wiki - 基于Web的分布式编写和版本控制](https://zh.wikipedia.org/wiki/%E5%9F%BA%E4%BA%8EWeb%E7%9A%84%E5%88%86%E5%B8%83%E5%BC%8F%E7%BC%96%E5%86%99%E5%92%8C%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6)



#### 混合开发

##### 混合开发流派

- **H5 加壳派：**以 Ionic 和 Uni-App 为代表，基于 WebView（ Android 下是 WebView，iOS 下是 WKWebView ）控件加载H5页面，同时通过框架预先实现的一些能力，实现对设备摄像头、文件系统等设备能力的调用。

- **JS Run 原生派：**以 RN 和 Weex为代表，使用 JS 进行编写，在运行时映射成原生控件运行。

- **自成一派：**以 Flutter 为代表，这货直接自己弄了个引擎和运行时，自带体系（UI组件和渲染器），除了设备能力外UI方面全权Handle。

| 技术类型     | UI渲染          | 性能 | 开发效率 | 代表框架       |
| ------------ | --------------- | ---- | -------- | -------------- |
| H5加壳派     | WebView渲染     | 一般 | 高       | Ionic、Uni-App |
| JS Run原生派 | 原生控件渲染    | 好   | 中       | RN、Weex       |
| 自成一派     | 调用系统API渲染 | 好   | 高       | Flutter        |



#### 埋点 & 监控

##### 为什么需要埋点

当我们在分析复盘一个产品是否成功的时候，不同的角色考虑的方向是不同的：

- **站在产品的视角**，经常会问如下几个问题：1. 产品有没有用户使用。2. 用户用得怎么样。3. 系统会不会经常出现异常。4. 如何更好地满足用户需求服务用户。

- **站在技术的视角**，经常会问如下几个问题：1. 系统出现异常的频率如何。2. 异常出现后如何快速进行定位追踪。3. 如何分析解决问题

- **站在老板的视角**，问题可能又会变为：1. 我的存量用户多少，未来还有多少潜力。2. 多少用户在系统内进行了消费。

当在回答了上述问题之后，埋点&监控 便跃然纸上。因为<font color=FF0000>要回答以上问题，只有通过对系统进行数据分析的方式才能弄清楚</font>。

其实无论是埋点亦或是监控，二者并不是独立存在，而是相互依存的关系。

<img src="https://s2.loli.net/2022/07/14/o14ykFx8A9XilpK.png" alt="img" style="zoom:48%;" />

##### 埋点 & 监控 能做什么

<font color=FF0000>从单个页面的常规数据角度出发我们可以通过埋点获取：**访问次数 ( UV/PV )**、**地域数据 ( IP )**、**在线时长**、**区域点击次数** 等数据</font>。

当我们将这些单点数据按照特定的纬度进行数据聚合，就可以获得全流程视角下的数据如：**用户留存率/流转率、用户转化率、用户访问深度** 等数据。

而在埋点数据进行上报的同时，我们也可以 **同步收集页面基础数据/接口相关数据** 如：<font color=fuchsia>**页面加载/渲染时长、页面异常、请求接口**</font> 等数据（👀  这些则是 **前端监控** 的功能）。

<font color=dodgerBlue>**对于前端监控来说，大致可以分成三个方向：数据监控、性能监控、异常监控：**</font>

- **数据监控**：<font color=FF0000>数据监控即通过数据分析用户行为</font>，**常见的监控数据包括：PV/UV、页面停留时长、通过什么入口进入、在页面触发了什么行为等**。统计这些数据就是为了清楚用户来源，拓宽产品的推广渠道；了解用户在页面停留的时间情况，针对停留较短的页面进行分析改进。也就是我们常说的：who(uuid)、when(time)、from where(referrer)、where(x, y)、what(自定义拓展数据) 串成的用户行为路径。

- **性能监控**：<font color=FF0000>性能监控主要是针对前端进行监控，比如 **不同用户在不同地区使用不同机型下的 首屏加载时间、页面的白屏时间、静态资源下载时间** 等数据</font>。通过针对这些性能数据进行监控，可以大概反映前端性能的好坏，根据性能监测的结果可以进一步的去优化前端性能。

- **异常监控**：<font color=FF0000>**前端代码在执行过程中也可能会发生异常**，因此 需要引入异常监控，例如 **sentry** 等工具及时的上报异常情况</font>，可以避免线上故障的发上。常见的异常包括：Javascript 的异常监控、css 的异常监控等。

  > 👀 关于异常监控，这里摘抄了 [前端异常的捕获与处理 # 六、异常上报](https://segmentfault.com/a/1190000039264963) 中的内容作为补充
  >
  > <font color=dodgerBlue>即使我们前端开发完成后，会有一系列的 Web 应用的上线前的验证，如自测、QA 测试、code review 等，以确保应用能在生产上没有事故</font>。
  >
  > 但是事与愿违，很多时候我们都会接到客户反馈的一些线上问题，这些问题有时候可能是你自己代码的问题。这样的问题一般能够在测试环境重现，我们很快的能定位到问题关键位置。但是，很多时候有一些问题，我们在测试中并未发现，可是在线上却有部分人出现了，问题确确实实存在的，这个时候我们测试环境又不能重现，还有一些偶现的生产的偶现问题，这些问题都很难定位到问题的原因，让我们前端工程师头疼不已。
  >
  > 而我们不可能每次都远程给用户解决问题，或者让用户按 F12 打开浏览器控制台把错误信息截图给我们吧。这时候，我们不得不借助一些工具来解决这一系列令人头疼的问题。
  >
  > 前端错误监控日志系统就应用而生。当前端代码在生产运行中出现错误的时候，第一时间传递给监控系统，从而第一时间定位并且解决问题。
  >
  > <font color=red>有很多成熟的方案可供选择： ARMS、fundebug、BadJS、Sentry</font>。<font color=LightSeaGreen>政采云当前使用的是 Sentry 的开源版本，并结合业务进行一些改造</font>：
  >
  > - 与构建系统结合，构建项目时自动生成 Sentry 项目，注入 Sentry 脚本
  > - 客服端注入 Sentry 客户端脚本后，按项目、页面等不同粒度配置告警事件的过滤规则
  > - 对接钉钉消息系统，将告警消息推送到订阅群
  > - 过滤接口错误和优化 Promise 错误上报信息

##### 目前采政云 埋点方案

目前公司已经存在一套埋点 SDK 在运行，使用的是代码埋点方案，其埋点上报数据可大致分为三类：页面进入、事件触发、页面离开。

- **页面进入 ( pageIn )：**进入页面时，同步推送页面基础信息如：当前页面的来源页面、操作系统、浏览器、页面 url，发生时间等
- **事件触发 ( Event )**：触发事件时，同步推送事件类型（ click、hover 等 ）、鼠标位置、附加业务参数等
- **页面离开 ( pageOut )**：离开页面时，同步推送发生时间、页面 url 等

##### 后续演进方向

在现有 SDK 的基础上我们可以发现，目前的埋点 SDK 只上报了一些用户的基础信息数据，在性能数据和异常数据的上报上还存在可拓展的空间。

- **性能数据上报：**在获取用户基础数据的同时，后续可以通过 `window.performance` API 获取前端性能数据，在第一次进入页面时随 pageIn 一起将页面初始性能数据进行上报。
  > 👀 这也是 **性能优化** 获得相关数据，最普遍的方法了

- **接口数据上报**：除了上报性能数据外，我们也 <font color=FF0000>可将页面内所发的所有请求通过重写 `XMLHttpRequest` 进行劫持打标上报，即在当前页面下的所有请求 header 上默认加上当前页面 ID，将各个请求与当前页面的 pageId 进行绑定</font>。

  通过该类数据可以进行统计分析出某一页面的请求量、请求异常等情况判断出页面级别的请求健康度；后期甚至可与 Yapi 接口系统打通，若出现异常情况可直接将实际请求参数与文档上的请求参数进行对比，排除异常是由于请求参数错误造成的。

摘自：[浅谈前端埋点&监控](https://www.zoo.team/article/monitor)

##### GIF 埋点 的技术方案

> 👀《为什么大厂前端监控都在用GIF做埋点？》笔记

<font color=dodgerBlue>现在常见的埋点上报方法有三种：手动埋点、可视化埋点、无埋点</font>

###### 手动埋点

手动埋点，<font color=red>也叫代码埋点</font>，即 <font color=fuchsia>纯手动写代码，**调用埋点 SDK 的函数**</font>，<font color=fuchsia>**在 需要埋点的业务逻辑功能位置 调用接口**，上报埋点数据</font>，像 “友盟”、“百度统计” 等第三方数据统计服务商大都采用这种方案。手动埋点<font color=red>让使用者可以方便地设置自定义属性、自定义事件</font>；所以<font color=red>当你需要深入下钻，并精细化自定义分析时，比较适合使用手动埋点</font>。

<font color=dodgerBlue>手动埋点的缺陷</font>就是：<font color=red>项目工程量大，需要埋点的位置太多</font>，而且<font color=red>需要产品开发运营之间相互反复沟通，容易出现手动差错，如果错误，重新埋点的成本也很高</font>。

###### 可视化埋点

<font color=fuchsia>通过 **可视化交互** 的手段，代替上述的代码埋点</font>。<font color=red>将业务代码和埋点代码分离，**提供一个可视化交互的页面**，输入为业务代码</font>，通过这个可视化系统，<font color=red>可以在业务代码中自定义的增加埋点事件等等</font>，<font color=fuchsia>**最后输出的代码耦合了业务代码和埋点代码**</font>。

<font color=dodgerBlue>可视化埋点的缺陷</font> 就是<font color=red>可以埋点的控件有限，不能手动定制</font>。

###### 无埋点

无埋点则是 <font color=fuchsia>前端自动**采集全部事件**，上报埋点数据</font>，由<font color=red>后端来过滤和计算出有用的数据</font>。<font color=dodgerBlue>**优点**</font>是 前端只要一次加载埋点脚本，<font color=dodgerBlue>**缺点**</font>是 <font color=red>流量和采集的数据过于庞大，服务器性能压力大</font>。

###### 使用 GIF 上报的原因

向服务器端上报数据，<font color=LightSeaGreen>可以通过请求接口</font>，<font color=fuchsia>**请求**普通文件</font>，或者 <font color=fuchsia>**请求** 图片资源</font>的方式进行。<font color=lightSeaGreen>只要能上报数据，无论是请求 GIF 文件还是请求 js文件 或者是 调用页面接口，服务器端其实并不关心具体的上报方式</font>。那为什么使用了请求 GIF 图片的方式上报数据呢（👀 准确的说是 “借用图片的 `src` 属性来发起请求”，至于 src 指向的图片是否存在是无所谓的）？

- **防止跨域**

  <font color=red>一般而言，打点域名都不是当前域名，所以所有的接口请求都会构成跨域</font>。而跨域请求很容易出现由于配置不当被浏览器拦截并报错，这是不能接受的。但<font color=red>图片的 src 属性并不会跨域，并且同样可以发起请求</font>。（排除接口上报）

- **防止阻塞页面加载，影响用户体验**

  <font color=lightSeaGreen>通常，**创建资源节点后只有将对象注入到浏览器 DOM树 后，浏览器才会实际发送资源请求**</font>。反复操作 DOM 不仅会引发性能问题，而且载入 js/css 资源还会阻塞页面渲染，影响用户体验。

  <font color=dodgerblue>但**图片请求例外**</font>。<font color=fuchsia>构造图片打点不仅不用插入 DOM，只要在 js 中 new Image 对象就能发起请求</font>，<font color=fuchsia>还没有阻塞问题</font>，<font color=fuchsia>**在没有 js 的浏览器环境中也能通过 img 标签正常打点**</font>，这是其他类型的资源请求所做不到的。（排除文件方式）

- **相比 PNG/JPG ，GIF 的体积最小**

  > 👀 以上都只说了 “使用 image” 作为发送数据的载体的原因，下面说为什么使用 GIF：

  最小的 BMP 文件需要74个字节，PNG 需要 67个字节，而合法的 GIF，只需要 43个字节。同样的响应，GIF 可以比 BMP 节约 41% 的流量，比 PNG 节约 35% 的流量。

  **并且大多采用的是1x1像素的透明GIF来上报**

  1x1像素是最小的合法图片。而且，因为是通过图片打点，所以图片最好是透明的，这样一来不会影响页面本身展示效果，二者表示图片透明只要使用一个二进制位标记图片是透明色即可，不用存储色彩空间数据，可以节约体积。

摘自：[为什么大厂前端监控都在用GIF做埋点？](https://juejin.cn/post/7065123244881215518)

> 👀 阅读评论区发现：使用 GIF 作为载体进行埋点，是很老的技术了；也似乎有点过时...

> 💡 在评论区还看见了 《JS高程 第四版》中原理类似的内容，摘录下来作为辅助阅读：
>
> > Web 应用程序开发中的一个常见做法是建立中心化的错误日志存储和跟踪系统。数据库和服务器错 误正常写到日志中并按照常用 API 加以分类。对复杂的 Web 应用程序而言，最好也把 JavaScript 错误发 送回服务器记录下来。这样做可以把错误记录到与服务器相同的系统，只要把它们归类到前端错误即可。 使用相同的系统可以进行相同的分析，而不用考虑错误来源。
> >
> > 要建立 JavaScript 错误日志系统，首先需要在服务器上有页面或入口可以处理错误数据。该页面只要从查询字符串中取得错误数据，然后把它们保存到错误日志中即可。比如，该页面可以使用如下代码：
> >
> > ```js
> > function logError(sev, msg) {
> >    let img = new Image(), 
> >       encodedSev = encodeURIComponent(sev),
> >       encodedMsg = encodeURIComponent(msg);
> >       img.src = 'log.php?sev=${encodedSev}&msg=${encodedMsg}';
> > }
> > ```
> >
> > `logError()` 函数接收两个参数：严重程度和错误消息。严重程度可以是数值或字符串，具体取决于使用的日志系统。这里使用 Image 对象发送请求主要是从灵活性方面考虑的。
> >
> > - <font color=red>所有浏览器都支持 Image 对象，即使不支持 XMLHttpRequest 对象也一样</font>。
> > - 不受跨域规则限制。通常，接收错误消息的应该是多个服务器中的一个，而 XMLHttpRequest 此时就比较麻烦。
> > - 记录错误的过程很少出错。大多数 Ajax 通信借助 JavaScript 库的包装来处理。如果这个库本身 出错，而你又要利用它记录错误，那么显然错误消息永远不会发给服务器。
> >
> > 摘自：《JS高程 第四版》- 21.2.7 把错误记录到服务器中 P677



#### H5 唤端技术

唤端技术也称之为 `deep link` 技术。不同平台的实现方式有些不同，一般常见的有这几种，分别是：

- URL Scheme（通用）
- Universal Link （iOS）
- App Link、Chrome Intents（android）

##### URL Scheme

URL Schemes 有两个单词：

- **URL：**我们都很清楚，http://www.apple.com 就是个 URL，我们也叫它链接或网址
- **Schemes：**表示的是一个 URL 中的一个位置——<font color=lightSeaGreen>最初始的位置，即 `://` 之前的那段字符</font>。比如 `https://www.apple.com` 这个网址的 Schemes 是 https

> 💡 补充：
>
> <font color=dodgerBlue>URL Scheme 组成</font>
>
> ```
> [scheme:][//authority][path][?query][#fragment]
> ```

根据我们上面对 URL Schemes 的使用，我们可以很轻易地理解，在以本地应用为主的 iOS 上，我们可以像定位一个网页一样，用一种特殊的 URL 来定位一个应用甚至应用里某个具体的功能。而定位这个应用的，就应该这个应用的 URL 的 Schemes 部分，也就是开头儿那部分。比如短信，就是 `sms:`

你可以完全按照理解一个网页的 URL （也就是它的网址）的方式来理解一个 iOS 应用的 URL，拿苹果的网站和 iOS 上的微信来做个简单对比：

|                   | 网页（苹果）                           | iOS 应用（微信）                |
| :---------------- | :------------------------------------- | :------------------------------ |
| 网站首页/打开应用 | `http://www.apple.com`                 | `weixin://`                     |
| 子页面/具体功能   | `http://www.apple.com/mac/`（Mac页面） | `weixin://dl/moments`（朋友圈） |

摘自：[URL Schemes 使用详解](https://sspai.com/post/31500)

###### URL Schemes 从功能上分为了 4 种

1. **基础 URL Schemes** ：用于启动应用，比如：`drafts4://`
2. **复杂 URL Schemes** ：用于直接打开应用的具体功能，比如：`drafts4://dictate`
3. **变形 URL Schemes** ：用于输入内容，含有第三方应用的特殊语法，比如：在 Launch Center Pro 里用这一条 `drafts4://create?text=[prompt]`
4. **x-callback-URL** ：前面的 URL Schemes 只能执行一个动作，而 `x-callback-URL` 可以根据根据前一段 URL Schemes 的执行情况决定进一步的行动。

摘自：[入门 iOS 自动化：读懂 URL Schemes](https://sspai.com/post/44591)

###### URL Schemes 在应用中的 “注册”

- macOS 在 <font color=FF0000>`Info.plist`</font> 文件中。
- Windows 通过往<font color=FF0000>注册表</font>的 HKCR ( HKEY_CALSSES_ROOT ) 目录下添加一条记录来完成该协议的注册。

摘自：[pc网页端调起客户端应用的那些事--electron](https://blog.csdn.net/dengdongxia/article/details/105906975) 该链接中还有开发相关的设置，这里略，需要用了再看。

###### URL Schemes 的优缺点

**优点**

- 兼容性好，无论安卓或者 iOS 都能支持，是目前最常用的方式
- 开发简单

**缺点**

- 只能通过固定协议格式的链接来实现跳转，而且<font color=FF0000>打开H5页面时，会出现一个提示框：“是否打开XXX”。用户确认了才会跳转到 App 中，增加了用户流程</font>；可能会导致用户流失

- 容易被屏蔽，app 很轻松就可以拦截掉通过 URL Scheme 发起的跳转。<font color=LightSeaGreen>微信、QQ 等禁用了 URL Scheme 打开 App，但是它们都各自维护着一个白名单，如果 Scheme 不在该白名单内，那么就不能在他们的 App 内打开这个 App</font>（如果被封锁了那么用户只能通过右上角浏览器内打开 App）
- 无法准确判断是否唤起成功，因为本质上这种方式就是打开一个链接，并且还不是普通的 http 链接，所以如果用户没有安装对应的 APP，那么尝试跳转后在浏览器中会没有任何反应，通过定时器来引导用户跳到应用商店，但这个定时器的时间又没有准确值，不同手机的唤端时间也不同，我们只能大概的估计一下它的时间来实现，一般设为3000ms左右比较合适；
- 有 URL Scheme 劫持风险。比如有一个 app 也向系统注册了 `zhihu://` 这个 scheme ，唤起流量可能就会被劫持到这个 app 里

摘自：[什么是Deeplink？以及Deeplink的原理-阿里云开发者社区](https://developer.aliyun.com/article/780283) 、[H5如何实现唤起APP](https://juejin.cn/post/7097784616961966094)

###### URL Scheme 列表

- [URL Scheme 分享](https://st3376519.huoban.com/share/1985010/VGi2N5Vf0C1MVnHCVWiBc8L9g15c9VGJbMGcFrb6/172707/list)
- [【基础知识】现在很火的app上的deeplink技术，到底是什么？](https://cloud.tencent.com/developer/article/1049347) 在最后的附录中

###### 判断 URL Scheme 是否成功唤起

当用户唤起APP失败时，我们希望可以引导用户去进行下载。那怎么才能知道当前 APP 是否成功唤起呢？

可以监听当前页面的 `visibilitychange` 事件，如果页面隐藏，则表示唤端成功；否则唤端失败，跳转到应用商店。

```vue
<template>
  <div class="open_app">
    <div class="open_btn" @click="open">打开腾讯微博</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      timer: null
    }
  },
  mounted() {
    this.watchVisibility()
  },
  methods: {
    watchVisibility() {
       window.addEventListener('visibilitychange', () => {
         // 监听页面 visibility
         if(document.hidden) {
           // 如果页面隐藏了，则表示唤起成功，这时候需要清除下载定时器
           clearTimeout(this.timer)
         }
       })
    },
    open() {
      this.timer = setTimeout(() => {
        // 没找到腾讯微博的下载页，这里暂时以淘宝下载页代替
        window.location.href = 'http://apps.apple.com/cn/app/id387682726'
      }, 3000)
      window.location.href = 'TencentWeibo://'
    }
  }
}
</script>
```

> 👀 这里源代码有点问题，总之没看懂 `visibilitychange` 事件是如何开始监听的；所以做了部分改动

##### Universal Link

Universal Link 是 iOS 9 中新增的功能，可以直接通过 `https` 协议的链接来打开 APP。 相比 URL Scheme 的优点是使用`https`协议；<font color=LightSeaGreen>如果没有唤端成功，那么就会直接打开这个网页，不再需要判断是否唤起成功</font>。并且使用 Universal Link，<font color=red>不会再弹出是否打开的弹出</font>；对用户来说，唤端的效率更高了。

###### 原理

- 在 APP 中注册自己要支持的域名
- 在自己域名的根目录下配置一个 `apple-app-site-association` 文件即可（具体的配置前端同学不用关注，只需与 iOS 同学确认好支持的域名即可）

###### 打开方式

```js
openByUniversal () {
  // 打开知乎问题页
  window.location.href = 'https://oia.zhihu.com/questions/64966868'
  // oia.zhihu.com
},
```

###### 适用性

- 相对 URL Scheme，有一个较大优点是它唤端时没有弹窗提示是否打开，提升用户体验，可以减少一部分用户流失
- <font color=red>无需关心用户是否安装对应的 APP</font>。对于没有安装的用户，点击链接就会直接打开对应的页面，因为它也是 http 协议的路径，这样也能一定程度解决 URL Scheme 无法准确判断唤端失败的问题；
- 只能够在 iOS 上使用
- 只能由用户主动触发

##### App Link & Chrome Intents

2015 年的 Google I/O 大会上，Android M 宣布了一个新特性：App Links 让用户在点击一个普通 web 链接的时候可以打开指定 APP 的指定页面，<font color=LightSeaGreen>前提是这个 APP 已经安装并且经过了验证</font>，否则会显示一个打开确认选项的弹出框，只支持 Android M 以上系统。

App Links 的最大的作用就是可以避免从页面唤醒 App 时出现的选择浏览器选项框，前提是必须注册相应的Scheme，就可以实现直接打开关联的 App

###### 缺点

- App links 在国内的支持还不够，部分安卓浏览器并不支持跳转至App，而是直接在浏览器上打开对应页面
- 系统询问是否打开对应App时，假如用户选择“取消”并且选中了“记住此操作”，那么用户以后就无法再跳转App

###### Chrome Intents

- Chrome Intent 是 Android 设备上 Chrome 浏览器中 URI 方案的深层链接替代品。
- 如果 APP 已安装，则通过配置的 URI SCHEME 打开 APP。
- 如果 APP 未安装，配置了 fallback url 的跳转 fallback url，没有配置的则跳转应用市场。

这两种方案在国内的应用都比较少。

##### 方案对比

|                   | URL Scheme  | Universal Link | App Link       |
| ----------------- | ----------- | -------------- | -------------- |
| <ios9             | 支持        | 不支持         | 不支持         |
| >=ios9            | 支持        | 支持           | 不支持         |
| <android6         | 支持        | 不支持         | 不支持         |
| >=android6        | 支持        | 不支持         | 支持           |
| 是否需要HTTPS     | 不需要      | 需要           | 需要           |
| 是否需要客户端    | 需要        | 需要           | 需要           |
| 无对应APP时的现象 | 报错/无反应 | 跳到对应的页面 | 跳到对应的页面 |

###### URI Scheme

- URL Scheme 的兼容性最高，但使用体验相对较差
- 当要被唤起的 App 没有安装时，这个链接就会出错，页面无反应
- 当注册有多个 Scheme 相同的时候，没有办法区分
- 不支持从其他 App 中的 UIWebView 中跳转到目标 APP， 所以 iOS 和 Android 都出现了自己的独有解决方案

###### Universal Link

- 已经安装 APP，直接唤起 APP；APP 没有安装，则跳去对应的 web link
- Universal Link 是从服务器上查询是哪个APP需要被打开，所以不会存在冲突问题
- Universal Link 支持从其他 App 中的 UIWebView 中跳转到目标 App
- 缺点在于会记住用户的选择：在用户点击了 Universal Link 之后，iOS 会去检测用户最近一次是选择了直接打开 App 还是打开网站。一旦用户点击了这个选项，他就会通过 safiri 打开你的网站。并且在之后的操作中，默认一直延续这个选择，除非用户从你的 webpage 上通过点击 Smart App Banner 上的 OPEN 按钮来打开。

###### App link

- 优点与 Universal Link 类似
- 缺点在于国内的支持相对较差，在有的浏览器或者手机 ROM 中并不能链接至 APP，而是在浏览器中打开了对应的链接。
- 在询问是否用 APP 打开对应的链接时，如果选择了“取消”并且“记住选择”被勾上，那么下次你再次想链接至 APP 时就不会有任何反应

摘自：[H5如何实现唤起APP](https://juejin.cn/post/7097784616961966094)

##### deep link 携带信息

deep link 强大的地方是<font color=FF0000>**能携带信息**</font>。虽然 URL Schemes 也可以用 query 携带信息；但是：你点击了我生成的deeplink 去下载客户端 然后安装、注册 这么多步骤 等你登录的时候 客户端能上报 deeplink 的信息 知道你点了谁的；而这时 URL Schemes 的 query 肯定丢失了

学习自：微信群 codingstartup 群友



#### SEO 

##### TDK

可以参考：[SEO优化中的TDK三大标签是什么？SEO标签讲解](https://www.boxuegu.com/news/367.html)

##### structured data

##### sitemap



#### Open graph

> 👀 open graph ( og ) 是一个 fb 推出的协议，在 twitter 中也被称为 twitter card。

##### What is Open Graph?

**[Open Graph](https://ogp.me/) 🔗 is <font color=dodgerBlue>an internet protocol</font>** that was <font color=LightSeaGreen>originally created by [Facebook](http://fbdevwiki.com/wiki/Open_Graph_protocol)</font> to <font color=red>standardize the use of metadata within a webpage</font> to represent the content of a page.

Within it, you <font color=lightSeaGreen>can provide details as simple as the title of a page or as specific as the duration of a video</font>. These pieces all fit together to form a representation of each individual page of the internet.

##### Why do I need it?

Content on the internet is typically created with at least one goal in mind -- to share it with others. This might not necessarily matter if you’re just sending it to one friend, but <font color=dodgerBlue>if you want to share it or want it to be shared on any social network or app that utilizes rich previews</font>, you’ll want that preview to be as effective as possible.

<img src="https://s2.loli.net/2023/03/20/MDh6merZ5VOYQXb.png" alt="" style="zoom:42%;" />

> 👀 这里图片在原文中就是一个 open graph 展示，由于无法展示，所以做了截图

This <font color=lightSeaGreen>will help encourage people to check out your content and inevitably click through to your content</font>.

##### Starting with the basics of open graph

The <font color=dodgerBlue>**four basic open graph tags**</font> that are <font color=red>**required for each page**</font> are <font color=red>`og:title` , `og:type` , `og:image` and `og:url`</font> . These tags should be unique for each page you serve, <font color=lightSeaGreen>meaning your homepage’s tags should all be different from your blog post article’s page</font>.

<img src="https://s2.loli.net/2023/03/20/fbBKrnLX327IETe.jpg" style="zoom: 45%;" />

While it should be pretty straightforward, here’s a breakdown of what each of the tags mean:

- **`og:title`** : The title of your page. This is typically the same as your webpage's `<title>` tag unless you’d like to present it differently.
- **`og:type`** : <font color=lightSeaGreen>The “type” of website you have</font>. I’ll explain more in the next section, though a generic “type” is “website”.
- **`og:image`** : This should be a link to an image that you’d like to represent your content. It <font color=LightSeaGreen>should be a high resolution image</font> that the social networks will use in their feeds.
- **`og:url`** : This should be the URL of the current page.

When placing a tag on your website, you should place it in the `<head>` along with any other metadata. The tag used will be a `<meta>` tag and should look like this pattern:

```html
<meta property=“[NAME]” content=“[VALUE]” />
```

So if I were to create a set four basic open graph tags for my website, [colbyfayock.com](https://colbyfayock.com/), it might look like:

```html
<meta property="og:title" content="Colby Fayock - A UX Designer &amp; Front-end Developer Blog" />
<meta property="og:type" content="website" />
<meta property="og:image" content="/static/website-social-card-44070c4a901df708aa1563ac4bbe595a.jpg" />
<meta property="og:url" content="https://www.colbyfayock.com" />
```

##### Website open graph type

<font color=dodgerBlue>The open graph protocol has a few variations of the “**type**” of website it supports</font>. This <font color=red>includes types like website, article, or video</font>.

When setting up your open graph tags, you’ll <font color=lightSeaGreen>want to have an idea of **which type will make more sense for your website**</font>. <font color=dodgerBlue>If your page is focused on a single video</font>, it probably makes sense to use the type “video”. <font color=dodgerBlue>If it’s a general website with no specific vertical,</font> you would probably just want to use the type “website”.

Similar to the others, this is unique for each page. So if your homepage is "website,” you could always have another page of type “video”.

So if I were to create an open graph type for my website, it might look like the following on my homepage:

```html
<!-- colbyfayock.com -->
<meta property=“og:type” content=“profile” />
```

When navigating to a blog post, it would look like:

```html
<!-- https://www.colbyfayock.com/2020/03/anyone-can-map-inspiration-and-an-introduction-to-the-world-of-mapping/ -->
<meta property=“og:type” content=“article” />
```

You can find the most common open graph website types on the open graph webpage: https://ogp.me/#types

##### Some other open graph tags that are worth adding

Though you’ll generally be okay with the basics, <font color=dodgerBlue>here are a few more that would be worth adding</font>:

- **`og:description`** : A description of your page. Similarly to `og:title` , this may be the same as your website’s `<meta type=“description”>` tag, unless you’d like to present it differently.
- **`og:locale`** : If you <font color=LightSeaGreen>want to localize your tags</font>, it would probably make sense to include locale. The format is `language_TERRITORY` , where <font color=LightSeaGreen>the default is `en_US`</font> .
- **`og:site_name`** : The name of the overall website your content is on. If you're on a blog post page, you might have a `title` using that blog post’s title, where the `site_name` would be the name of your blog.
- **`og:video`** : Have a video that supports your content? Here’s a chance to include it. Add a link to your video using this tag.

##### Twitter and other social media networks using open graph

Most of the social networks adhere to the basics of open graph standards, <font color=dodgerBlue>but a few of them also include their own extension to help customize the look and feel within their ecosystem</font>.

<font color=dodgerBlue>Twitter for instance</font>, **allows you to specify `twitter:card`** , which is the type of “card” you can use when they show your website. At this time, <font color=dodgerBlue>**their card types include**</font>:

- summary
- summary_large_image
- app
- player

This will help you choose how your links are used in their feed. If you choose `summary_large_image` for instance, Twitter will show your links with big high resolution images as long as you’re providing it in the in the `og:image` tag.

<font color=dodgerBlue>**Here are some quick references to the documentation of how to use open graph tags with some of the social media sites**</font>:

- Twitter: https://developer.twitter.com/en/docs/tweets/optimize-with-cards/guides/getting-started
- Facebook: https://developers.facebook.com/docs/sharing/webmasters/
- Pinterest: https://developers.pinterest.com/docs/rich-pins/overview/?
- LinkedIn: https://www.linkedin.com/help/linkedin/answer/46687/making-your-website-shareable-on-linkedin?lang=en

##### Images in open graph

While adding your image as `og:image` should often be enough, sometimes it can be challenging to get your image to show up correctly. If you seem to be running into trouble,<font color=LightSeaGreen> the open graph standard includes a few image tags such as `og:image:url` vs `og:image:secure_url` as well as the `og:image:width` and `og:image:height`</font>.

Try to make sure you’re following all of the [notes and examples in the open graph documentation](https://ogp.me/#structured). Additionally, some of the social networks have image requirements. [Twitter for instance requires](https://developer.twitter.com/en/docs/tweets/optimize-with-cards/overview/summary-card-with-large-image) <font color=LightSeaGreen>a ratio of 2:1 with a minimum size of 300x157 and a maximum size of 4096x4096 that’s under 5MB and of JPG, PNG, WEBP or GIF format</font>.

If you’re stuck, test your tags using the social media network’s tools to see if you can find the issue.

##### Testing your open graph tags

Luckily, our favorite social networks also <font color=red>**provide tools to help us debug our tags**</font>. Once you make sure that your tags are actually showing up in the source code of your website, you’ll be able to preview how your website will look in the feed.

- Twitter: https://cards-dev.twitter.com/validator
- Facebook: https://developers.facebook.com/tools/debug/
- Pinterest: https://developers.pinterest.com/tools/url-debugger/

##### Example

If you’re simply looking for an example to get started, here’s what you should end up with when setting up your tags for [a blog post](https://www.colbyfayock.com/2020/03/anyone-can-map-inspiration-and-an-introduction-to-the-world-of-mapping/):

```html
<meta property="og:site_name" content="Colby Fayock" />
<meta property=“og:title” content=“Anyone Can Map! Inspiration and an introduction to the world of mapping - Colby Fayock" />
<meta property="og:description" content="Chef Gusteau was a visionary who created food experiences for the world to enjoy. How can we take his lessons and apply them to the world of…" />
<meta property="og:url" content="https://www.colbyfayock.com/2020/03/anyone-can-map-inspiration-and-an-introduction-to-the-world-of-mapping/" />
<meta property="og:type" content="article" />
<meta property="article:publisher" content="https://www.colbyfayock.com" />
<meta property="article:section" content="Coding" />
<meta property="article:tag" content="Coding" />
<meta property="og:image" content="https://res.cloudinary.com/fay/image/upload/w_1280,h_640,c_fill,q_auto,f_auto/w_860,c_fit,co_rgb:232129,g_west,x_80,y_-60,l_text:Source%20Sans%20Pro_70_line_spacing_-10_semibold:Anyone%20Can%20Map!%20Inspiration%20and%20an%20introduction%20to%20the%20world%20of%20mapping/blog-social-card-1.1" />
<meta property="og:image:secure_url" content="https://res.cloudinary.com/fay/image/upload/w_1280,h_640,c_fill,q_auto,f_auto/w_860,c_fit,co_rgb:232129,g_west,x_80,y_-60,l_text:Source%20Sans%20Pro_70_line_spacing_-10_semibold:Anyone%20Can%20Map!%20Inspiration%20and%20an%20introduction%20to%20the%20world%20of%20mapping/blog-social-card-1.1" />
<meta property="og:image:width" content="1280" />
<meta property="og:image:height" content="640" />
<meta property="twitter:card" content="summary_large_image" />
<meta property="twitter:image" content="https://res.cloudinary.com/fay/image/upload/w_1280,h_640,c_fill,q_auto,f_auto/w_860,c_fit,co_rgb:232129,g_west,x_80,y_-60,l_text:Source%20Sans%20Pro_70_line_spacing_-10_semibold:Anyone%20Can%20Map!%20Inspiration%20and%20an%20introduction%20to%20the%20world%20of%20mapping/blog-social-card-1.1" />
<meta property="twitter:site" content="@colbyfayock" />
```

摘自：[What is Open Graph and how can I use it for my website?](https://www.freecodecamp.org/news/what-is-open-graph-and-how-can-i-use-it-for-my-website/)



#### 序列帧

Apple 官网喜欢用用户滚动页面 实现 播放视频（动画）效果，其中关键技术：序列帧。

如果是播放视频的效果：是使用 canvas，在canvas中渲染图片，并根据页面的滚动 切换图片，形成播放效果。

学习自：[仿苹果 AirPods-Pro 产品页黑人动画之序列帧](https://zhuanlan.zhihu.com/p/89895329)

如果是动画（视频似乎也可以）的效果，可以使用 Steven lei 的 [trigger](https://github.com/triggerjs/trigger)；类似的，也可以使用 gsap 的 scrollTrigger 结合 timeline

了解自：codingstartup群 群友



#### JSBridge

<font color=FF0000>**JSBridge 是一种 JS 实现的 Bridge，连接着桥两端的 Native（注：这里的 Native 是指移动端原生应用） 和 H5**</font>。它<font color=FF0000>在 APP 内方便地让 Native 调用 JS，JS 调用 Native ，是双向通信的通道</font>。JSBridge <font color=LightSeaGreen>**主要提供了 JS 调用 Native 代码的能力**，实现原生功能如查看本地相册、打开摄像头、指纹支付等</font>。

> 💡 双向通道的含义是：
>
> - **JS 向 Native 发送消息：**调用相关功能、<font color=FF0000>通知 Native 当前 JS 的相关状态</font>等。
> - **Native 向 JS 发送消息 :** 回溯调用结果、消息推送、<font color=FF0000>通知 JS 当前 Native 的状态</font>等。
>
> 摘自：[JSBridge的原理](https://juejin.cn/post/6844903585268891662)

##### H5 与 Native 对比

| name        | H5                                                | Native                                             |
| :---------- | :------------------------------------------------ | :------------------------------------------------- |
| 稳定性      | 调用系统浏览器内核，稳定性较差                    | 使用原生内核，更加稳定                             |
| 灵活性      | 版本迭代快，上线灵活                              | 迭代慢，需要应用商店审核，上线速度受限制           |
| 受网速 影响 | 较大                                              | 较小                                               |
| 流畅度      | 有时加载慢，给用户“卡顿”的感觉                    | 加载速度快，更加流畅                               |
| 用户体验    | 功能受浏览器限制，体验有时较差                    | 原生系统 api 丰富，能实现的功能较多，体验较好      |
| 可移植性    | 兼容跨平台跨系统，如 PC 与 移动端，iOS 与 Android | 可移植性较低，对于 iOS 和 Android 需要维护两套代码 |

##### JSBridge 的双向通信原理

- **JS 调用 Native：**<font color=dodgerBlue>JS 调用 Native 的**实现方式**较多</font>，主要有拦截 URL Scheme 、重写 prompt 、注入 API 等方法
  - **拦截 URL Scheme：**Android 和 iOS 都可以通过拦截 URL Scheme 并解析 scheme 来决定是否进行对应的 Native 代码逻辑处理
  - **重写 prompt 等原生 JS 方法**
    - Android 4.2 之前注入对象的接口是 addJavascriptInterface ，但是由于安全原因慢慢不被使用。一般会通过修改浏览器的部分 Window 对象的方法来完成操作。<font color=LightSeaGreen>主要是拦截 alert、confirm、prompt、console.log 四个方法，分别被 Webview 的 onJsAlert、onJsConfirm、onConsoleMessage、onJsPrompt 监听</font>。
    - iOS 由于安全机制， <font color=LightSeaGreen>WKWebView 对 alert、confirm、prompt 等方法做了拦截，如果通过此方式进行 Native 与 JS 交互，需要实现 WKWebView 的三个 WKUIDelegate 代理方法</font>。
  - **注入 API：**<font color=FF0000>基于 Webview 提供的能力</font>，我们<font color=FF0000>可以向 Window 上注入对象或方法</font>。<font color=FF0000>JS 通过这个对象或方法进行调用时，执行对应的逻辑操作，可以直接调用 Native 的方法</font>。使用该方式时，JS 需要等到 Native 执行完对应的逻辑后才能进行回调里面的操作。
- **Native 调用 JS：**Native 调用 JS 比较简单，只要 <font color=FF0000>H5 将 JS 方法暴露在 Window 上给 Native 调用即可</font>。

##### JSBridge 的使用

###### 如何引用

- **由 H5 引用：**在我司移动端初期版本时采用的是该方式，采用本地引入 npm 包的方式进行调用。这种方式可以确定 JSBridge 是存在的，可直接调用 Native 方法。但是如果后期 Bridge 的实现方式改变，双方需要做更多的兼容，维护成本高
- **由 Native 注入：**这是当前我司移动端选用的方式。在考虑到后期业务需要的情况下，进行了重新设计，选用 Native 注入的方式来引用 JSBridge。这样有利于保持 API 与 Native 的一致性，但是缺点是在 Native 注入的方法和时机都受限，JS 调用 Native 之前需要先判断 JSBridge 是否注入成功

###### 使用规范

见链接，略

摘自：[小白必看，JSBridge 初探](https://www.zoo.team/article/jsbridge)

// TODO 阅读文章 [JSBridge的原理](https://juejin.cn/post/6844903585268891662)



#### JNI

可以参考文章：[Android JNI(一)——NDK与JNI基础](https://www.jianshu.com/p/87ce6f565d37) 感觉介绍得不错

由于，暂时用不到，这里暂时不精读&做笔记



#### PWA

在看 webpack 中 PWA 的内容，由于 webpack 作为 bundler 下的 PWA 是通过 workbox 作为支持的。就在知乎上搜了 workbox，发现了如下问题与回答，引发了我的思考...

> ***弱网环境下前端如何优化？***
>
> 没有什么骚操作，Google推广的PWA就是为你准备的。
>
> 你有三个选项：HTTP Cache，Service Worker 和 Cache Storage API，Workbox。
>
> 如果你的网站支持HTTPS，推荐用Workbox。如果用的是前端三大框架之一，都有现成的方案。
>
> 摘自：[弱网环境下前端如何优化？ - 陈龙的回答 - 知乎](https://www.zhihu.com/question/306385245/answer/560086505)

另外，大量关于 PWA 的内容被记录在 [[webpack学习笔记]] 那边，这里暂时不做记录。



#### Chrome 使用

在 [[linux与macOS备忘录#Chrome相关]] 中有一些 Chrome 快捷键

`chrome://chrome-urls/` 中列出了 Chrome 相关的所有“内置页”，下面列出重要的（虽然不全...）：

- `chrome://inspect/`：调试相关，这也是 Node 程序调试，一个非常方便的方法

  > 现在 Node 调试简单多了，不需要安装任何额外的包，直接 `node --inspect yourScript.js`，然后打开 `chrome://inspects`即可。
  >
  > 摘自：[Node 调试指南 —— Inspector 协议](https://zhuanlan.zhihu.com/p/30264842) 另外，该文章说明了 Node 在浏览器中调试的方法；另外，也可参考官方文档： [Node Doc - Guides - Debugging Guide](https://nodejs.org/en/docs/guides/debugging-getting-started/)

- `chrome://flags/`：实验功能配置

- `chrome://downloads/`：下载列表

- `chrome://extensions/`：扩展插件配置

- `chrome://settings/`：设置

- `chrome://version/`：当前版本信息

- `chrome://user-actions/`：侦听用户行为

- `chrome://whats-new/`：新特性

- `chrome://network-errors/`：网络错误列表

- `chrome://system/`：系统信息

- `chrome://bookmarks/`：书签

- `chrome://device-log/`：设备日志（USB 之类）

- `chrome://components/`：组件信息与更新

- `chrome://tracing/`：追踪 Chrome 各个任务运行。另外，trace 工具是一个 计算机术语，用于监控程序运行

- `chrome://newtab/`：新标签页

- `chrome://print/`：调用打印组件

- `chrome://predictors/`：浏览器的地址栏 ( Omnibox ) 的预测器，可以根据用户输入提前做 pre-fetch、pre-render、preload；该链接显示相关预测的信息。相关内容可参考 [Mac 平台的 Chrome 比 Safari 性能更好吗？为什么用的人那么多？ - 李熠的回答 - 知乎](https://www.zhihu.com/question/24541628/answer/28168760)

- `chrome://omnibox/`：地址栏设置

  ###### 关于 Omnibox

  omnibox 本意不是 “地址栏”，它只是：Chrome 的地址栏代号是 Omnibox。详见下面：

  > Chrome 的地址栏代号是 Omnibox，omni 是万能、全能的词根，Omnibox 就是全能盒子的意思。在 2008 年 Chrome 发布的时候，其它浏览器都是在一个长的地址栏右侧有个短的搜索框，Omnibox 把搜索和网址输入合二为一是当时一个大的创新。
  >
  > Omnibox 是个很复杂的东西，远比我们用户想的要复杂，Chrome 就自带一个给他们的开发者调试用的页面，chrome://omnibox：
  >
  > ![img](https://pic3.zhimg.com/v2-ba1a9d1c59e5e2103fdbfac92aab4999_r.jpg)
  >
  > 摘自：[Chrome 是怎么判断地址栏输入的东西是不是网址? - 紫云飞的回答 - 知乎](https://www.zhihu.com/question/560616439/answer/2722866208)

- `chrome://crashes/`：浏览器崩溃历史

- `chrome://net-internals/`：可以在其中找到有关代理，DNS，缓存，HTTP限制等信息的页面。

  > 你们想看看自己Chrome里有关网络的一切？请在地址栏里输入：`chrome://net-internals` 。什么DNS、Cache、Prerender（上面说的预先加载的页面）、目前可用的 Socket 都一览无遗。
  >
  > 摘自： [Mac 平台的 Chrome 比 Safari 性能更好吗？为什么用的人那么多？ - 李熠的回答 - 知乎](https://www.zhihu.com/question/24541628/answer/28168760)

- `chrome://safe-browsing/`：浏览器安全设置？

- `chrome://serviceworker-internals/`：浏览器内注册的所有 service worker，以及配置

- `chrome://gcm-internals/`：消息推送服务

- `chrome://indexeddb-internals/`：html5 内部存储

- `chrome://sync-internals/`：同步记录

- `chrome://quota-internals/`：显示磁盘详细可用空间以及各个网站的使用配额

- `chrome://translate-internals/`：内部翻译器

- `chrome://memory-internals/`：内存记录

- `chrome://signin-internals/`：The “chrome://signin-internals” webpage is a special URL in chromium that displays a summary of all signin and authentication related information in Google Chrome. It is useful in the triage and pinpointing of errors that could potentially be authentication related.

  摘自：[The Chromium Projects - about-signin-internals](https://www.chromium.org/developers/about-signin-internals/)

另外还在 [DNS Prefetching的两三事 - 于秋的文章 - 知乎](https://zhuanlan.zhihu.com/p/22362198) 发现了 `chrome://histograms/` 以及它下面的 `chrome://histograms/dns` 

部分参考自：[我所了解的chrome ](https://www.cnblogs.com/liyunhua/p/4531964.html) 、[你可能不知道的chrome隐藏技巧 - 伯衡君的文章 - 知乎](https://zhuanlan.zhihu.com/p/99527398)

另外，[The Chromium Projects](https://www.chromium.org/chromium-projects/) 中有不少相关内容，可参考。



#### Atomic CSS

> 👀 如果要了解定义，看下面 **这句话** 就够了

Atomic CSS is the <font color=red>approach to CSS architecture</font> that <font color=fuchsia>favors small, **single-purpose classes with names based on visual function**</font>（ 👀 之所以被称为 function，是因为它接受自定义的选项/参数；见下面 [[#Programmatic]] 的示例代码 ）.

The term “Atomic CSS” was coined（创造（新词汇）） by Thierry Koblenz in his foundational article [“Challenging CSS Best Practices”](https://www.smashingmagazine.com/2013/10/challenging-css-best-practices-atomic-approach/) in October 2013.

Some people started referring to this approach as “Functional CSS” some time later. Though there have been cases in the past where Functional CSS has been used to describe something else , today the terms Functional CSS and Atomic CSS are used interchangeably（可交换的）.

##### Programmatic

The Programmatic approach to Atomic CSS involves using a build tool to automatically generate styles based on what it finds in the HTML.

For example, given this:

```html
<!-- Programmatic Atomic CSS Example -->
<div class="Bgc(#0280ae) C(#fff) P(20px)">Lorem ipsum</div>
```

The following CSS would be generated:

```css
.Bgc\(#0280ae\) { background-color: #0280ae; }
.C\(#fff\) { color: #fff; }
.P\(20px\) { padding: 20px; }
```

##### Longhand/Shorthand

The longhand style favors more readable class names (see [Expressive CSS](http://johnpolacek.github.io/expressive-css/) and [Solid](http://solid.buzzfeed.com/)), while shorthand favors brevity（简洁）(see [Tachyons](http://tachyons.io/) and [Basscss](http://basscss.com/)).

```css
/* Shorthand Atomic CSS Examples */
.bg-blue { background-color: #357edd; } 
.f1 { font-size: 3rem; }
.ma0 { margin: 0; }

/* Longhand Atomic CSS Examples */
.bgr-blue { background-color: #357edd; }
.background-blue  { background-color: #357edd; }
.backgroundcolor-blue  { background-color: #357edd; }
.text-h1 { font-size: 3rem; }
.text-3rem { font-size: 3rem; }
.text-huge { font-size: 3rem; }
.fontsize-1 { font-size: 3rem; }
.marg-0 { margin: 0; }
.margin-0 { margin: 0; }

/* Programmatic Shorthand */
Bgc(#357edd) { background-color: #357edd; }

/* Programmatic Longhand */
bgrBlue(#357edd) { background-color: #357edd; }
backgroundBlue(#357edd) { background-color: #357edd; }
backgroundColorBlue(#357edd) { background-color: #357edd; }
```

摘自：[John Polacek - Let’s Define Exactly What Atomic CSS is](https://css-tricks.com/lets-define-exactly-atomic-css/)

另外也可以参见 [antfu - Reimagine Atomic CSS](https://antfu.me/posts/reimagine-atomic-css) 中开头的内容



#### 无障碍辅助功能

“无障碍辅助功能” 被称为 “a11y” ，即 Accessibility 的简化写法。