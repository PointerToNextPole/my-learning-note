# Web相关备忘录



#### unicode相关

<font color=FF0000>Unicode 是一种字符集标准，用于对来自世界上不同语言、文字系统和符号进行编号和字符定义</font>。<mark>通过给每个字符分配一个编号，程序员可以创建字符编码，让计算机在同一个文件或程序中存储、处理和传输任何语言组合</mark>。

<mark>在 Unicode 定义之前，在同一数据中混合使用不同的语言是很困难的，而且容易出错</mark>。例如，一个字符集存储的是日文字符，而另一个字符集存储的是阿拉伯字母。如果没有明确标明数据的哪些部分属于哪个字符集，其他程序和计算机就会错误地显示文本，或者在处理过程中损坏文本。如果你曾经见过像 (“”) 被替换为胡言乱语 Ã‚Â£，那么你就已经看到过这个被称为  Mojibake 的问题。

<font color=FF0000 size=4>**网络上最常见的 Unicode 字符编码是UTF-8**</font>。还存在一些其他编码，如 UTF-16或过时的 UCS-2，但推荐使用 UTF-8。

摘自：[MDN - Unicode](https://developer.mozilla.org/zh-CN/docs/Glossary/Unicode)

<font color=FF0000 size=4>**阅读前注意：**</font>下面这篇文章，写于2014年，部分内容或已过时

1. <font size=4>**Unicode是什么？**</font>

   Unicode源于一个很简单的想法：将全世界所有的字符包含在一个集合里，计算机只要支持这一个字符集，就能显示所有的字符，再也不会有乱码了。

   <mark>它从0开始，为每个符号指定一个编号</mark>，<font color=FF0000>这叫做"码点"（code point）</font>。比如，码点0的符号就是null（表示所有二进制位都是0）。

   ```
   U+0000 = null
   ```

   上式中，U+表示 <font color=FF0000>**紧跟在后面的十六进制数是Unicode的码点**</font>。

   <mark>目前（2014年），Unicode的最新版本是7.0版，一共收入了109449个符号，其中的中日韩文字为74500个</mark>。可以近似认为，全世界现有的符号当中，三分之二以上来自东亚文字。比如，中文"好"的码点是十六进制的597D。

   <font color=FF0000>这么多符号，Unicode不是一次性定义的，而是<font size=4>**分区定义**</font></font>。<font color=FF0000>每个区可以存放65536个（216）字符，称为一个平面（plane）</font>。<font color=FF0000>目前，一共有17个（2^5^）平面，也就是说，整个Unicode字符集的大小现在是2^21^</font>。

   <font color=FF0000><font size=4>**最前面的65536个字符位**</font>，称为<font size=4>**基本平面（Basic Multilingual Plane，缩写BMP）**</font></font>，<font color=FF0000>它的码点范围是从0一直到2^16^-1，写成16进制就是从U+0000到U+FFFF</font>。<mark>**所有最常见的字符都放在这个平面**，这是Unicode最先定义和公布的一个平面</mark>。

   <font color=FF0000><font size=4>**剩下的字符**</font>都放在<font size=4>**辅助平面（Supplementary Multilingual Plane 缩写SMP）**</font></font>，<mark>码点范围从U+010000一直到U+10FFFF</mark>。

2. <font size=4>**UTF-32与UTF-8**</font>

   Unicode只规定了每个字符的码点，到底用什么样的字节序表示这个码点，就涉及到<font color=FF0000 size=4>**编码方法**</font>。

   <font color=FF0000>最直观</font>的编码方法是，<font color=FF0000>每个码点使用四个字节表示</font>，字节内容一一对应码点。<font color=FF0000>这种编码方法就叫做UTF-32</font>。比如，码点0就用四个字节的0表示，码点597D就在前面加两个字节的0。

   ```
   U+0000 = 0x0000 0000
   U+597D = 0x0000 597D
   ```

   UTF-32的<font color=0000FF>优点</font>在于，<font color=0000FF>转换规则简单直观，查找效率高</font>。<font color=FF0000>缺点</font>在于<font color=FF0000>浪费空间</font>，同样内容的英语文本，它会比ASCII编码大四倍。<mark>这个缺点很致命，导致实际上没有人使用这种编码方法</mark>，<font color=FF0000>HTML 5标准就明文规定，网页不得编码成UTF-32</font>。

   人们真正需要的是一种节省空间的编码方法，这导致了UTF-8的诞生。**UTF-8是一种<font size=4>变长的编码方法，字符长度从1个字节到4个字节不等</font>。**<font color=FF0000>越是常用的字符，字节越短，最前面的128个字符，只使用1个字节表示，与ASCII码完全相同</font>。

   | 编号范围            | 字节 |
   | ------------------- | ---- |
   | 0x0000 - 0x007F     | 1    |
   | 0x0080 - 0x07FF     | 2    |
   | 0x0800 - 0xFFFF     | 3    |
   | 0x010000 - 0x10FFFF | 4    |

   由于UTF-8这种节省空间的特性，导致它成为互联网上最常见的网页编码。

3. <font size=4>**UTF-16简介**</font>

   UTF-16编码<mark>介于UTF-32与UTF-8之间</mark>，<font color=FF0000>**同时结合了定长和变长两种编码方法的特点**</font>。

   它的编码规则很简单：<font color=FF0000>基本平面的字符占用2个字节，辅助平面的字符占用4个字节</font>。<mark>**也就是说，UTF-16的编码长度要么是2个字节（U+0000到U+FFFF），要么是4个字节（U+010000到U+10FFFF）。**</mark>

   <mark style=background-color:aqua>于是就有一个问题，当我们遇到两个字节，怎么看出它本身是一个字符，还是需要跟其他两个字节放在一起解读？</mark>

   说来很巧妙，我也不知道是不是故意的设计，<font color=FF0000 size=4>**在基本平面内，从U+D800到U+DFFF是一个空段**（共2^12^个），即这些码点不对应任何字符。因此，这个空段可以用来映射辅助平面的字符</font>。

   具体来说，<font color=FF0000>辅助平面的字符位共有2^20^个，也就是说，对应这些字符至少需要20个二进制位</font>。<font color=FF0000>UTF-16将这20位拆成两半</font>，<font color=FF0000 size=4>**前10位映射在U+D800到U+DBFF（空间大小2^10^），称为高位（H），后10位映射在U+DC00到U+DFFF（空间大小2^10^），称为低位（L）**</font>。这意味着，一个辅助平面的字符，被拆成两个基本平面的字符表示。

   **所以，当我们遇到两个字节，发现它的码点在U+D800到U+DBFF之间，就可以断定，紧跟在后面的两个字节的码点，应该在U+DC00到U+DFFF之间，这四个字节必须放在一起解读。**

4. <font size=4>**UTF-16的转码公式**</font>

   Unicode码点转成UTF-16的时候，首先区分这是基本平面字符，还是辅助平面字符。如果是前者，直接将码点转为对应的十六进制形式，长度为两字节。

   ```
   U+597D = 0x597D
   ```

   如果是辅助平面字符，Unicode 3.0版给出了转码公式。

   ```js
   H = Math.floor((c - 0x10000) / 0x400) + 0xD800
   L = (c - 0x10000) % 0x400 + 0xDC00
   ```

   以字符![img](https://i.loli.net/2021/08/23/S9ch8BUR1Q3iWVz.png)为例，它是一个辅助平面字符，码点为U+1D306，将其转为UTF-16的计算过程如下。

   ```js
   H = Math.floor((0x1D306-0x10000)/0x400)+0xD800 = 0xD834
   L = (0x1D306-0x10000) % 0x400+0xDC00 = 0xDF06
   ```

   所以，字符![img](https://i.loli.net/2021/08/23/S9ch8BUR1Q3iWVz.png)的UTF-16编码就是0xD834 DF06，长度为四个字节。

5. <font size=4>**JavaScript使用哪一种编码？**</font>

   JavaScript语言采用Unicode字符集，但是只支持一种编码方法。这种编码既不是UTF-16，也不是UTF-8，更不是UTF-32。上面那些编码方法，JavaScript都不用。<font color=FF0000 size=4>**JavaScript用的是UCS-2！**</font>

摘自：[阮一峰 - Unicode与JavaScript详解](http://www.ruanyifeng.com/blog/2014/12/unicode.html)





#### <font color=FF0000>编写web程序时出现的问题</font>

- 出现如下情况：

  <img src="https://s1.ax1x.com/2020/07/05/US9DqP.png" style="zoom: 45%;" />

  其中<font color=FF0000>**一种解释原因**</font>（无法确认是所有）是：当前访问的页面不存在（物理上不存在） / 当前程序能访问到的中的资源中没有该资源（资源存在，但程序找不到）。



#### <font color=FF0000>80端口和8080端口</font>

<font color=FF0000>80是**http协议的默认端口**</font>，是在输入网站的时候其实浏览器（非IE）已经帮你输入协议了，所以你输入http://baidu.com，其实是访问http://baidu.com:80，而<font color=FF0000>8080，一般用于webcahe</font>，<font color=FF0000>一般是用来连接代理的</font>。完全不一样的两个，比如linux服务器里apache默认跑80端口，而apache-tomcat默认跑8080端口，其实端口没有实际意义只是一个接口，主要是看服务的监听端口，如果baidu的服务器监听的81端口，那么你直接输入就不行了就要输入http://baidu.com:81这样才能正常访问

摘自：[80端口跟8080端口有什么具体区别？ - 死神的沙漏的回答 - 知乎](https://www.zhihu.com/question/26698345/answer/33709330)



#### <font color=FF0000>URL</font>

URL中 `%十六进制数字` 代表的是十六进制的ASCII码



#### <font color=FF0000>HTML的转义字符</font>

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



#### <font color=FF0000>SOAP & REST</font>

##### SOAP

SOAP：简单对象访问协议（Simple Object Access Protocol）是一种<mark>基于 XML 的协议</mark>，可以和现存的许多因特网协议和格式结合使用，包括超文本传输协议（HTTP），简单邮件传输协议（SMTP），多用途网际邮件扩充协议（MIME），基于“通用”传输协议是 SOAP的一个优点。它还支持从消息系统到远程过程调用（Remote Procedure Call，RPC）等大量的应用程序。SOAP提供了一系列的标准，如WSRM（WS-Reliable Messaging）形式化契约确保可靠性与安全性，确保异步处理与调用；WS-Security、WS-Transactions和WS-Coordination等标准提供了上下文信息与对话状态管理

***

##### Rest

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

**RESTful的<font color=FF0000>五个约束</font>**<mark>（如果一个系统满足了下面所列出的五条约束，那么该系统就被称为是RESTful的）</mark>

- **使用客户/服务器模型（C/S）**：客户和服务器之间通过一个统一的接口来互相通讯。
- **层次化的系统**：在一个REST系统中，客户端并不会固定地与一个服务器打交道。
- **无状态**：在一个REST系统中，服务端并不会保存有关客户的任何状态。也就是说，客户端自身负责用户状态的维持，并在每次发送请求时都需要提供足够的信息。
- **可缓存**：REST系统需要能够恰当地缓存请求，以尽量减少服务端和客户端之间的信息传输，以提高性能。
- <font color=FF0000>**统一的接口**</font>：一个REST系统<mark>需要使用一个统一的接口来完成子系统之间以及服务与用户之间的交互</mark>。这使得REST系统中的各个子系统可以独自完成演化。

**与REST密切相关的两个名词：<font color=FF0000>资源</font>和<font color=FF0000>状态</font>**

可以说，<font color=FF0000>**资源**</font>是REST系统的核心概念。<mark>所有的设计都会以资源为中心</mark>，包括如何对资源进行添加，更新，查找以及修改等。而资源本身则拥有一系列<font color=FF0000>**状态**</font>：<mark>在每次对资源进行添加 ，删除或修改的时候，资源就将从一个状态转移到另外一个状态。</mark>

摘自：[简书文章：rest](https://www.jianshu.com/p/90d757e925d1)

**Rest架构的主要原则**

- <font color=FF0000>网络上的所有事物都被抽象为资源</font>
- <font color=FF0000>每个资源都有一个唯一的资源标识符</font>
- 同一个资源具有多种表现形式(xml, json等)
- 对资源的各种操作不会改变资源标识符
- 所有的操作都是无状态的
- 符合REST原则的架构方式即可称为RESTful

摘自：[什么是 REST 以及 RESTful?](http://blog.bhusk.com/articles/2018/05/24/1527167962745)

优雅的REST风格的<font color=FF0000>资源URL</font>不希望带有.html或.do等后缀

摘自：[视图和视图解析器](https://www.cnblogs.com/xuweiweiwoaini/p/11852405.html)

***

#### REST和SOAP区别

<font color=FF0000>在SOAP模式把HTTP作为一种通信协议，而不是应用协议</font>。<mark>所以http中的表头，错误信息等全部无视。实际上http有 put get post delete等方法</mark>。
<font color=FF0000>REST 则不然，HTTP method中的 POST GET PUT DELETE 都是与请求方法对应的</font>，rest真正实现了http的五层结构。REST 提交的请求中，代理服务器可以通过请求方式直接判断请求动作是要进行什么操作。

摘自：[REST 和 SOAP 的区别理解](https://blog.csdn.net/carlsunnnn/article/details/80570872)



#### <font color=FF0000>POJO</font>

POJO（Plain Ordinary Java Object）即普通Java类，具有一部分getter/setter方法的那种类就可以称作POJO。

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



#### <font color=FF0000>OAuth</font>

**OAuth（Open Authorization）**。OAuth协议为用户资源的授权提供了一个安全的、开放而又简易的标准。与以往的授权方式不同之处是OAuth的授权不会使第三方触及到用户的帐号信息（如用户名与密码），即<mark>第三方无需使用用户的用户名与密码就可以申请获得该用户资源的授权</mark>，因此OAuth是安全的。

摘自：[百度百科 - OAuth](https://baike.baidu.com/item/oAuth)

类似的可参考：[OAuth 2.0 的一个简单解释](http://www.ruanyifeng.com/blog/2019/04/oauth_design.html)



#### <font color=FF0000>JSP</font>

**JSP的本质其实就是Servlet**。只是JSP当初设计的目的是为了简化Servlet输出HTML代码。

**JSP已经很少用了，被下面两种技术替代**

- 模板引擎（freemarker、Thymeleaf、Velocity），用法其实跟JSP差不太多，只是它们的性能会更好
- 前后端分离，后端只需要返回JSON给前端，页面完全不需要后端管

摘自：[2020年了，不学jsp后学什么代替？ - Java3y的回答 - 知乎 ](https://www.zhihu.com/question/382065572/answer/1102140696)

**JSP九大内置对象**

- **输出输入对象**

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

- **通信控制对象**

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

- **错误处理对象**

  - **exception对象**

    **所在包：**运行时的异常,java.lang.Throwable

    被调用的错误页面的结果,只有在错误页面中才可使用,

    即在页面指令中设置:<%@page isErrorPage=“true”%>

摘自：[JSP 9 大内置对象详解](https://www.cnblogs.com/kelin1314/archive/2011/03/03/1969578.html)



#### <font color=FF0000>Tomcat</font>

**启动tomcat**

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

**关闭tomcat**

在 Windows 上，可以通过执行下面的命令来停止 Tomcat：

```
C:\apache-tomcat-5.5.29\bin\shutdown
```

在 Unix（Solaris、Linux 等） 上，可以通过执行下面的命令来停止 Tomcat：

```
/usr/local/apache-tomcat-5.5.29/bin/shutdown.sh
```



#### <font color=FF0000>Servlet的生命周期</font>

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

#### <font color=FF0000>实现Servlet</font>

Servlet 是服务 HTTP 请求并实现 javax.servlet.Servlet 接口的 Java 类。<mark>Web 应用程序开发人员通常编写 Servlet 来扩展 javax.servlet.http.HttpServlet，并<font color=FF0000>实现 Servlet 接口的抽象类</font>专门用来处理 HTTP 请求</mark>。

#### <font color=FF0000>Get和Post方法</font>

- **GET 方法**

  GET 方法向页面请求发送已编码的用户信息。<font color=FF0000>页面和已编码的信息中间用 ? 字符分隔</font>，如下所示：

  ```
  http://www.test.com/hello?key1=value1&key2=value2
  ```

  GET 方法是<font color=FF0000>默认的从浏览器向 Web 服务器传递信息的方法</font>，它会产生一个很长的字符串，出现在浏览器的地址栏中。如果您要向服务器传递的是密码或其他的敏感信息，请不要使用 GET 方法。<mark>GET 方法有大小限制：请求字符串中最多只能有 1024 个字符</mark>。

  这些信息使用 QUERY_STRING 头传递，并可以通过 QUERY_STRING 环境变量访问，Servlet 使用 **doGet()** 方法处理这种类型的请求。

- **POST 方法**

  另一个向后台程序传递信息的比较可靠的方法是 POST 方法。POST 方法打包信息的方式与 GET 方法基本相同，但是 <font color=FF0000>POST 方法不是把信息作为 URL 中 ? 字符后的文本字符串进行发送，而是把这些信息作为一个单独的消息。消息以标准输出的形式传到后台程序，您可以解析和使用这些标准输出</font>。Servlet 使用 doPost() 方法处理这种类型的请求。

#### <font color=FF0000>使用 Servlet 读取表单数据</font>

Servlet 处理表单数据，这些数据会根据不同的情况使用不同的方法自动解析：

- **getParameter()：**您<font color=FF0000>可以调用 request.getParameter() 方法来获取表单参数的值</font>。
- **getParameterValues()：**如果<font color=FF0000>参数出现一次以上，则调用该方法，并返回多个值</font>，例如复选框。
- **getParameterNames()：**如果您想要<font color=FF0000>得到当前请求中的所有参数的完整列表</font>，则调用该方法。

#### <font color=FF0000>Servlet的Http请求</font>

当浏览器请求网页时，它会向 Web 服务器发送特定信息，这些信息不能被直接读取，因为<font color=FF0000>这些信息是作为 HTTP **请求的头**的一部分进行传输的</font>。

**浏览器端的重要头信息**

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

**读取 HTTP 头的方法**

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

#### <font color=FF0000>Servlet的Http响应</font>

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
| 23   | **void setStatus(int sc)**<br> 为该响应设置状态码。              |

#### <font color=FF0000>Servlet http状态码</font>

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

#### <font color=FF0000>Servlet 过滤器</font>

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

#### <font color=FF0000>Servlet Cookie 处理</font>

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

#### <font color=FF0000>Servlet Session 跟踪</font>

HTTP 是一种<font color=FF0000>**"无状态"**</font>协议，<mark>这意味着每次客户端检索网页时，客户端打开一个单独的连接到 Web 服务器，服务器会自动不保留之前客户端请求的任何记录</mark>。

#### **有以下三种方式来维持 Web 客户端和 Web 服务器之间的 session 会话：**

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

**HttpSession 对象**

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

#### <font color=FF0000>Servlet 网页重定向</font>

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

#### <font color=FF0000>Servlet 自动刷新页面</font>

Java Servlet 提供了一个机制，使得网页会在给定的时间间隔自动刷新。

刷新网页的最简单的方式是使用响应对象的方法 **setIntHeader()**。以下是这种方法的定义：

```java
public void setIntHeader(String header, int headerValue)
```

此方法把头信息 "Refresh" 连同一个表示时间间隔的整数值（以秒为单位）发送回浏览器。

以上关于Servlet的内容摘自：[RUNOOB - Servlet 教程](https://www.runoob.com/servlet/servlet-tutorial.html)



#### <font color=FF0000>Web容器的作用域</font>

几乎所有web应用容器都提供了四种类似Map的结构：**application / session / request / page**，Jsp或者Servlet通过向着这四个对象放入数据，从而实现Jsp和Servlet之间数据的共享。

- **application：应用**

  <font color=FF0000>作用范围在服务器一开始执行服务，到服务器关闭为止</font>。Application 的范围最、停留的时间也最久，所以使用时要特别注意不然可能会造成服务器负载越来越重的情况。只要将数据存入application对象，数据的范围范围 (Scope) 就为Application。

  具有application范围的对象被绑定到<font color=FF0000>javax.servlet.ServletContext</font>中。在Web应用程序运行期间，所有的页面都可以访问在这个范围内的对象。

- **session：会话**

  <font color=FF0000>HTTP会话</font>开始到结束这段时间。Session 的<font color=FF0000>作用范围为一段用户持续和服务器所连接的时间</font>，但与服务器断线，这个属性就无效了。

  Session 的开始时刻比较容易判断，它从浏览器发出第一个HTTP请求即可认为会话开始。但结束时刻就不好判断了，因为浏览器关闭时并不会通知服务器，所以只能通过如下这种方法判断：如果一定的时间内客户端没有反应，则认为会话结束。Tomcat的默认值为120分钟，但这个值也可以通过HttpSession的setMaxInactiveInterval()方法来设置。

  具有session范围的对象被绑定到<font color=FF0000>javax.servlet.http.HttpSession</font>对象中。

  另外：<font color=FF0000>Session 的致命弱点是不容易在多台服务器之间共享</font>，所以这也限制了 Session 的使用。这就需要用cookie解决问题

- **request：一次请求**

  HTTP请求开始到结束这段时间。Request 的<font color=FF0000>范围是指在一JSP 网页发出请求到另一个JSP 网页之间</font>，否则这个属性就失效。一个HTTP请求的处理可能需要多个Servlet合作，而这几个Servlet之间可以通过某种方式传递信息，但这个信息在请求结束后就无效了。
  具有request范围的对象被绑定到<font color=FF0000>javax.servlet.ServletRequest</font>对象中。


  要注意的是，因为请求对象对于每一个客户请求都是不同的，所以对于每一个新的请求，都要重新创建和删除这个范围内的对象。

- **page：当前页面**

  作用范围：<font color=FF0000>当前页面从打开到关闭这段时间，它只能在同一个页面中有效</font>。

  具有page范围的对象被绑定到<font color=FF0000>javax.servlet.jsp.PageContext</font>对象中。



#### <font color=FF0000>Session和Cookie</font>

会话（Session）：指用户登录网站后的一系列动作，比如浏览商品添加到购物车并购买。会话跟踪是 Web 程序中常用的技术，用来**跟踪用户的整个会话**。

由于HTTP协议是<font color=FF0000>无状态的协议</font>，所以服务端需要记录用户的状态时，就需要用某种机制来识具体的用户。由于HTTP协议无状态，所以并不知道是哪个用户操作的，所以服务端要为特定的用户创建了特定的Session，用用于标识这个用户。

**只使用session的痛点**：<font color=FF0000>Session 的致命弱点是不容易在多台服务器之间共享</font>，所以这也限制了 Session 的使用。这就需要用cookie

#### <mark>**<font color=FF0000>Cookie</font> 通过<font color=FF0000>在客户端记录</font>信息<font color=FF0000>确定用户身份</font>**，**<font color=FF0000>Session</font> 通过<font color=FF0000>在服务器端记录</font>信息<font color=FF0000>确定用户身份</font>**。</mark>

#### **<mark>session 的运行依赖 session id，而 <font color=FF0000>session id 是存在 cookie 中的</font>，也就是说，<font color=FF0000>如果浏览器禁用了 cookie ，同时 session 也会失效</font></mark>**（但是可以通过其它方式实现，比如在 url 中传递 session_id）所以说：**session 是基于 cookie 实现的，session 存储在服务器端，sessionId 会被存储到客户端的cookie 中**

**<mark style=background-color:aqua><font color=FF0000>如果客户端的浏览器禁用了 Cookie 怎么办？</font></mark>**一般这种情况下，会使用一种叫做<font color=FF0000>URL重写</font>的技术来<font color=FF0000>进行会话跟踪</font>，即每次HTTP交互，<font color=FF0000>URL后面都会被附加上一个诸如 sid=xxxxx 这样的参数，服务端据此来识别用户</font>。

##### **补充：cookie的重要属性**

| 属性       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| name=value | 键值对，设置Cookie的名称及相对应的值，都必须是字符串类型<br/>如果值为Unicode字符，需要为字符编码。<br/>如果值为1 -进制数据，则需要使用BASE64编码。 |
| domain     | 指定cookie所属域名,默认是当前域名                            |
| path       | 指定cookie在哪个路径(路由)下生效， 默认是''/'。<br/>如果设置为/abc ，则只有/abc 下的路由可以访问到该cookie,如: /abc/read |
| maxAge     | cookie失效的时间，单位秒。如果为整数，则该cookie在maxAge秒后失效。如果为负数，该cookie为临时cookie，关闭浏览器即失效，浏览器也不会以任何形式保存该cookie。如果为0,表示删除该cookie。 默认为-1。另外，这个方法比expires好用。 |
| expires    | 过期时间，在设置的某个时间点后该cookie就会失效。<br/>一般浏览器的cookie都是默认储存的，当关闭浏览器结束这个会话的时候，这个cookie也就会被删除 |
| secure     | 该cookie是否仅被使用安全协议传输。安全协议有HTTPS, SSL等，在网络上传输数据之前先将数据加密。默认为false。<br/>当secure值为true时，cookie 在HTTP中是无效，在HTTPS中才有效。 |
| httpOnly   | 如果给某个cookie设置了httpOnly属性,则无法通过JS脚本读取到该cookie的信息，但还是能通过Application中手动修改cookie,所以只是在-定程度上可以防止XSS攻击，不是绝对的安全 |

##### session 认证流程

- 用户<font color=FF0000>**第一次请求服务器**</font>的时候，服务器根据用户提交的相关信息，创建对应的 Session
- 请求返回时将此 Session 的唯一标识信息 SessionID 返回给浏览器
- <mark>浏览器接收到服务器返回的 SessionID 信息后，会将此信息存入到 Cookie 中，同时 Cookie 记录此 SessionID 属于哪个域名</mark>
- 当用户<font color=FF0000>**第二次访问服务器**</font>的时候，<mark>请求会自动判断此域名下是否存在 Cookie 信息，如果存在自动将 Cookie 信息也发送给服务端，服务端会从 Cookie 中获取 SessionID，再根据 SessionID 查找对应的 Session 信息，如果没有找到说明用户没有登录或者登录失效，如果找到 Session 证明用户已经登录可执行后面操作</mark>。

根据以上流程可知，**SessionID 是连接 Cookie 和 Session 的一道桥梁**，大部分系统也是根据此原理来验证用户登录状态。

##### Cookie 和 Session 的区别

- **安全性：** Session 比 Cookie 安全，Session 是存储在服务器端的，Cookie 是存储在客户端的。
- **存取值的类型不同**：<mark>Cookie 只支持存字符串数据，想要设置其他类型的数据，需要将其转换成字符串，Session 可以存任意数据类型</mark>
- **有效期不同：** Cookie 可设置为长时间保持，比如我们经常使用的默认登录功能，<font color=FF0000>Session 一般失效时间较短，客户端关闭（默认情况下）或者 Session 超时都会失效</font>。
- **存储大小不同：** 单个 Cookie 保存的数据不能超过 4K，Session 可存储数据远高于 Cookie，但是当访问量过多，会占用过多的服务器资源。


部分摘自：[傻傻分不清之 Cookie、Session、Token、JWT](https://juejin.im/post/6844904034181070861)，<font color=FF0000>后面还有Token和JWT的内容，建议阅读</font>


#### <font color=FF0000>redirect & forward</font>

**Forward**：直接转发方式。客户端和浏览器只发出一次请求，Servlet、HTML、JSP或其它信息资源，由第二个信息资源响应该请求，在请求对象request中，保存的对象对于每个信息资源是共享的。

**Redirect**：间接转发方式。实际是两次HTTP请求，服务器端在响应第一次请求的时候，让浏览器再向另外一个URL发出请求，从而达到转发的目的。

**举个通俗的例子：**

直接转发就相当于：“A找B借钱，B说没有，B去找C借，借到借不到都会把消息传递给A”；

间接转发就相当于："A找B借钱，B说没有，让A去找C借"。

##### **具体阐述：**

- 间接转发方式，有时也叫<font color=FF0000>重定向</font>，它<font color=FF0000>一般用于避免用户的非正常访问</font>。<mark>例如：用户在没有登录的情况下访问后台资源，Servlet可以将该HTTP请求重定向到登录页面，让用户登录以后再访问</mark>。在Servlet中，通过调用response对象的SendRedirect()方法，告诉浏览器重定向访问指定的URL，示例代码如下：　

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

**技巧：**

其实，通过浏览器就可以观察到服务器端使用了那种请求转发方式，当单击某一个超链接时，浏览器的地址栏会出现当前请求的地址，如果服务器端响应完成以后，<mark>发现地址栏的地址变了，则证明是间接的请求转发</mark>。相反，<mark>如果地址没有发生变化，则代表的是直接请求转发或者没有转发</mark>。

**问：直接转发和间接转发的原理及区别是什么？**

答：Forward和Redirect代表了两种请求转发方式：直接转发和间接转发。对应到代码里，分别是RequestDispatcher类的forward()方法和HttpServletRequest类的sendRedirect()方法。

　　对于间接方式，服务器端在响应第一次请求的时候，让浏览器再向另外一个URL发出请求，从而达到转发的目的。它本质上是两次HTTP请求，对应两个request对象。

　　对于直接方式，客户端浏览器只发出一次请求，Servlet把请求转发给Servlet、HTML、JSP或其它信息资源，由第2个信息资源响应该请求，两个信息资源共享同一个request对象。

摘自：[JAVA常见面试题之Forward和Redirect的区别](https://www.cnblogs.com/selene/p/4518246.html)



#### <font color=FF0000>HTTP</font>

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



#### <font color=FF0000>双向绑定</font>

<font color=FF0000>单向绑定</font>就是把Model绑定到View，当我们用JavaScript代码更新Model时，View就会自动更新。

<font color=FF0000>双向绑定</font>就是如果用户更新了View，Model的数据也自动被更新了，这种情况就是双向绑定。



#### <font color=FF0000>面包屑导航</font>

页面路径（英语：Breadcrumb或Breadcrumb Trail/Navigation），又称面包屑导航，是在用户界面中的一种导航辅助。<mark>它是用户一个在程序或文件中确定和转移他们位置的一种方法</mark>。面包屑这个词来自糖果屋这个童话故事；故事中，汉赛尔与葛丽特企图依靠洒下的面包屑找到回家的路。

**页面路径**通常在页面顶部水平出现，一般会位于标题或页头的下方。<mark>它们提供给用户返回之前任何一个页面的链接</mark>（这些链接也是能到达当前页面的路径），<mark>在层级架构中通常是这个页面的父级页面</mark>。页面路径提供给用户回溯到网站首页或入口页面的一条路径，通常是以大于号（>）出现，尽管一些设计是其他的符号（如»）。

它们绝大部分看起来就像这样：首页>分类页>次级分类页 或者 首页>>分类页>>次级分类页。

**一共有三种类型的网站面包屑导航。**

1. 路径型(Path)：路径型面包屑是一个动态显示用户到达页面经过的途径。
2. 位置型(Location)：位置型面包屑是固定的，显示了页面在网站结构中的位置。
3. 属性型(Attribute)：属性型面包屑给出的当前页面的分类信息。

摘自：[维基百科 - 面包屑导航]([https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%8C%85%E5%B1%91%E5%AF%BC%E8%88%AA](https://zh.wikipedia.org/wiki/面包屑导航))



#### <font color=FF0000>浏览器对象模型（BOM）</font>

浏览器对象模型(BOM)指的是<font color=FF0000>由Web浏览器暴露的所有对象组成的表示模型</font>。<font color=FF0000>BOM</font>与DOM不同，其<font color=FF0000>既没有标准的实现</font>，<font color=FF0000>也没有严格的定义, 所以浏览器厂商可以自由地实现BOM</font>。

作为显示文档的窗口, 浏览器程序将其视为对象的分层集合。当浏览器分析文档时, 它将创建一个对象的集合, 以定义文档, 并详细说明它应如何显示。浏览器创建的对象称为文档对象。它是浏览器使用的更大的对象集合的一部分。此浏览器对象集合统称为浏览器对象模型或BOM。

BOM层次结构的顶层是窗口对象, 它包含有关显示文档的窗口的信息。某些窗口对象本身就是描述文档和相关信息的对象。

摘自：[维基百科 - 浏览器对象模型](https://zh.wikipedia.org/wiki/%E6%B5%8F%E8%A7%88%E5%99%A8%E5%AF%B9%E8%B1%A1%E6%A8%A1%E5%9E%8B)



#### <font color=FF0000>单页富应用程序（SPA）</font>

单页应用（英语：single-page application，缩写SPA）是<font color=FF0000>一种网络应用程序或网站的**模型**</font>，<font color=FF0000>它通过**动态重写当前页面**来与用户交互</font>，<font color=FF0000>而非传统的从服务器重新加载整个新页面</font>。<mark>这种方法避免了页面之间切换打断用户体验，使应用程序更像一个桌面应用程序</mark>。在单页应用中，所有必要的代码（HTML、JavaScript和CSS）都通过单个页面的加载而检索，或者根据需要（通常是为响应用户操作）动态装载适当的资源并添加到页面。尽管可以用位置散列或HTML5历史API来提供应用程序中单独逻辑页面的感知和导航能力，但页面在过程中的任何时间点都不会重新加载，也不会将控制转移到其他页面。与单页应用的交互通常涉及到与网页服务器后端的动态通信。

摘自：[维基百科 - 单页应用](https://zh.wikipedia.org/wiki/%E5%8D%95%E9%A1%B5%E5%BA%94%E7%94%A8)

简单的说 SPA 就是<font color=FF0000>一个WEB项目只有一个 HTML 页面</font>，<font color=FF0000>一旦页面加载完成，SPA 不会因为用户的操作而进行页面的重新加载或跳转</font>。 取而代之的是<font color=FF0000>利用 JS 动态的变换 HTML 的内容，从而来模拟多个视图间跳转</font>。

摘自：[「前端进阶」彻底弄懂前端路由](https://juejin.im/post/6844903890278694919)



#### <font color=FF0000>**服务端渲染（SSR）和 客户端渲染（CSR）**</font>

- **服务端渲染概念**

  解释一：服务端在返回 html 之前，在特定的区域，符号里用数据填充，再给客户端，客户端只负责解析 HTML 。

  解释二：服务端渲染的模式下，<font color=FF0000>当用户第一次请求页面时，由服务器把需要的组件或页面渲染成 HTML 字符串，然后把它返回给客户端</font>。<mark>客户端拿到手的，是可以直接渲染然后呈现给用户的 HTML 内容</mark>，不需要为了生成 DOM 内容自己再去跑一遍 JS 代码。使用服务端渲染的网站，可以说是 “所见即所得”，页面上呈现的内容，我们在 html 源文件里也能找到。

  **利弊**

  - **好处:**  首屏渲染快、利于 SEO、可以生成缓存片段，生成静态化文件、节能（对比客户端渲染的耗电）

  - **坏处:**  用户体验较差、不容易维护，通常前端改了部分 html 或者 css，后端也需要修改。

- **客户端渲染概念**

  解释一：客户端渲染模式下，<font color=FF0000>**服务端把渲染的静态文件给到客户端**</font>，<font color=FF0000>客户端拿到服务端发送过来的文件自己跑一遍 js，根据 JS 运行结果，生成相应 DOM，然后渲染给用户</font>。

  解释二：html 仅仅作为静态文件，客户端在请求时，服务端不做任何处理，<font color=FF0000>直接以原文件的形式返回给客户端客户端</font>，然后根据 html 上的 JavaScript，生成 DOM 插入 html。

  `延伸`：前端渲染的方式起源于 JavaScript 的兴起，ajax 的大热更是让前端渲染更加成熟，前端渲染真正意义上的实现了前后端分离，前端只专注于 UI 的开发，后端只专注于逻辑的开发，前后端交互只通过约定好的 API 来交互，后端提供 json 数据，前端循环 json 生成 DOM 插入到页面中去。

  **利弊**

  - **好处：** 网络传输数据量小、减少了服务器压力、前后端分离、局部刷新，无需每次请求完整页面、交互好可实现各种效果

  - **坏处：**不利于 SEO、爬虫看不到完整的程序源码、首屏渲染慢（渲染前需要下载一堆 js 和 css 等）

摘自：[服务端渲染（SSR)](https://juejin.im/post/6844903731075481608)



#### <font color=FF0000>前端路由和后端路由</font>

- **后端路由: **对于普通的网站，<font color=FF0000>所有的超链接都是URL地址</font>，<font color=FF0000>所有的URL地址都对应服务器上对应的资源</font>
- **前端路由: **对于<font color=FF0000>单页面应用程序</font>来说，主要<font color=FF0000>通过URL中的hash（#号）来实现不同页面之间的切换（与锚点相似）</font>。同时，hash有一个特点: <font color=FF0000>HTTP请求中不会包含hash相关的内容</font>；所以，单页面程序中的页面跳转主要用hash实现
- 在单页面应用程序中，'<font color=FF0000>**这种通过hash改变来切换页面的方式，称作前端路由**</font>（ 区别于后端路由）



#### CORS

CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）。

<font color=FF0000>它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。</font>

**补充：**

跨源资源共享 (CORS) （或通俗地译为跨域资源共享）是一种基于HTTP 头的机制，该机制通过允许服务器标示除了它自己以外的其它origin（域，协议和端口），这样浏览器可以访问加载这些资源

出于安全性，浏览器限制脚本内发起的跨源HTTP请求。跨源域资源共享（ CORS ）机制允许 Web 应用服务器进行跨源访问控制，从而使跨源数据传输得以<font color=FF0000>安全进行</font>。现代浏览器支持在 API 容器中（例如 XMLHttpRequest 或 Fetch ）使用 CORS，以降低跨源 HTTP 请求所带来的风险。



**什么情况下需要 CORS ？**
这份 [cross-origin sharing standard](http://www.w3.org/TR/cors/) 允许在下列场景中使用跨站点 HTTP 请求：

- 前文提到的由 XMLHttpRequest 或 Fetch 发起的跨源 HTTP 请求。
- Web 字体 (CSS 中通过 @font-face 使用跨源字体资源)，因此，网站就可以发布 TrueType 字体资源，并只允许已授权网站进行跨站调用。
- WebGL 贴图

- 使用 drawImage 将 Images/video 画面绘制到 canvas



<font color=FF0000>CORS需要浏览器和服务器同时支持</font>。目前，所有浏览器都支持该功能，IE浏览器不能低于IE10。

<font color=FF0000>整个CORS通信过程，都是浏览器自动完成，不需要用户参与</font>。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。

因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。



**浏览器将CORS请求分成两类：简单请求（simple request）和非简单请求（not-so-simple request）。**

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
  - DPR
  - Downlink
  - Save-Data
  - Viewport-Width
  - Width
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

  一旦服务器通过了"预检"请求，以后每次浏览器正常的CORS请求，就都跟简单请求一样，会有一个Origin头信息字段。服务器的回应，也都会有一个Access-Control-Allow-Origin头信息字段。
  **补充：**

  规范要求，对那些可能对服务器数据产生副作用的 HTTP 请求方法（特别是 GET 以外的 HTTP 请求，或者搭配某些 MIME 类型的 POST 请求），<mark>浏览器必须首先使用 OPTIONS 方法发起一个预检请求（preflight request），从而获知服务端是否允许该跨源请求。服务器确认允许之后，才发起实际的 HTTP 请求</mark>。在预检请求的返回中，服务器端也可以通知客户端，是否需要携带身份凭证（包括 Cookies 和 HTTP 认证相关数据）。
  CORS请求失败会产生错误，但是为了安全，在JavaScript代码层面是无法获知到底具体是哪里出了问题。你只能查看浏览器的控制台以得知具体是哪里出现了错误。

摘自：[跨域资源共享 CORS 详解](https://www.ruanyifeng.com/blog/2016/04/cors.html)  [MDN - 跨源资源共享（CORS）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)(另外，这其中还有很多没有看懂的，需要继续阅读)



#### MIME

**MIME 简介**

<font color=FF0000>MIME, Mutipurpose Internet Mail Extensions，多用途 Internet 邮箱扩展</font>。<font color=FF0000>MIME 是描述消息内容类型的 internet 标准</font>。<mark>在创建之初，是为了在发送电子邮件时附加多媒体数据，让邮件客户程序根据其类型进行处理</mark>。<font color=FF0000>现在 MIME TYPE 被 HTTP 协议支持后，使得HTTP能够传输各种各样的文件</font>。

**浏览器与 MIME-TYPE**

浏览器通过 MIME TYE,也就是该资源的媒体类型，来决定以什么形式显示数据。

<mark>媒体类型通常是通过 HTTP 协议，由 Web 服务器请求头中的 Content-Type 来告知浏览器数据类型的</mark>，比如：

```http
Content-Type: text/HTML
```

表示内容是 text/HTML 类型，也就是超文本文件。注意，必须是 "text/HTML" 而不是 "HTML/text".因为 MIME 是经过 ietf 组织协商，以 RFC 的形式发布在网上的。

**自定义的类型**

需要注意的是：<font color=FF0000>**只有一些在互联网上获得广泛应用的格式才会获得一个 MIME Type**</font>，<font color=FF0000>如果是某个客户端**自己定义的格式**，**一般只能以 application/x- 开头**</font>。

Internet 中有一个专门组织来对 MIME 标准进行修订，但是由于 Internet 发展过快，很多应用程序便使用在类别中以 x- 开头的方法标识这个类别还没有成为标准，例如 x-gzip,x-tar等。

其实是不是标准无关紧要，只要客户端和服务器都能识别这个格式就可以了。在 app 端会使用自定义标准来保证数据安全。

MIME类型与文档的后缀相关，因此服务器使用文档的后缀来区分不同文件的 MIME 类型，服务器中必须规定文件后缀和MIME类型之间的对应关系。而客户端从服务器上接收数据的时候，它只是从服务器接收数据流，并不了解文档的名字，因此服务器需要使用附加信息来告诉客户程序数据的 MIME 类型。服务器将首先发送以下两行 MIME 标识信息，这个信息并不是真正的数据文件的一部分。

```http
Context-type: text/html

```

注意，第二行为一个空格，这是必须的，使用这个空行的目的是将 MIME 信息与真正的数据内容分离开。

**MIME TYPE语法 及常见分类**

<font color=FF0000>通用结构：`type/subtype`</font>
<font color=FF0000>MIME 类型对大小写不敏感，但是通常传统写法是小写。</font>
 *分类*

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

<font size=4>**补充：**</font>

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



#### Base64

**Base64索引表**

| 数值 | <mark>字符</mark> | 数值 | <mark>字符</mark> | 数值 | <mark>字符</mark> | 数值 | <makr>字符</mark> |
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

**具体来说，转换方式可以分为四步。**

- **第一步**，<font color=FF0000>**将每三个字节作为一组**，一共是24个二进制位</font>。
- **第二步**，<font color=FF0000>**将这24个二进制位分为四组**，每个组有6个二进制位</font>。
- **第三步**，<font color=FF0000>在每组前面加两个00，扩展成32个二进制位，即四个字节</font>。
- **第四步**，<font color=FF0000>**根据索引表**，得到扩展后的每个字节的对应符号</font>，这就是Base64的编码值。

因为，Base64将三个字节转化成四个字节，因此Base64编码后的文本，会比原文本大出三分之一左右。

**举一个具体的实例，演示英语单词Man如何转成Base64编码。**

![image-20210823164828151](https://i.loli.net/2021/08/23/nfjBv3gHG7YpwP5.png)

- **第一步**，"M"、"a"、"n"的ASCII值分别是77、97、110，对应的二进制值是01001101、01100001、01101110，将它们连成一个24位的二进制字符串010011010110000101101110。

- **第二步**，将这个24位的二进制字符串分成4组，每组6个二进制位：010011、010110、000101、101110。

- **第三步**，在每组前面加两个00，扩展成32个二进制位，即四个字节：00010011、00010110、00000101、00101110。它们的十进制值分别是19、22、5、46。

- 第四步，根据上表，得到每个值对应Base64编码，即T、W、F、u。

<font size=4>**如果字节数不足三，则这样处理：**</font>

- **二个字节的情况：**将这<font color=FF0000>**二个字节**</font>的一共16个二进制位，按照上面的规则，<font color=FF0000>**转成三组**</font>，<font color=FF0000>最后一组除了前面加两个0以外，**后面也要加两个0**</font>。这样得到一个三位的Base64编码，<font color=FF0000>**再在末尾补上<font size=4>一个"="号</font>**</font>。

  比如，"Ma"这个字符串是两个字节，可以转化成三组00010011、00010110、00010000以后，对应Base64值分别为T、W、E，再补上一个"="号，因此<font color=FF0000>"Ma"的Base64编码就是TWE=</font>。

- **一个字节的情况：**将这<font color=FF0000>**一个字节**</font>的8个二进制位，按照上面的规则<font color=FF0000>**转成二组**</font>，最后一组除了前面加二个0以外，<font color=FF0000>**后面再加4个0**</font>。这样得到一个二位的Base64编码，<font color=FF0000>**再在末尾补上<font size=4>两个"="号</font>**</font>。

比如，"M"这个字母是一个字节，可以转化为二组00010011、00010000，对应的Base64值分别为T、Q，再补上二个"="号，因此"M"的Base64编码就是TQ==。

摘自：[阮一峰 - Base64笔记](https://www.ruanyifeng.com/blog/2008/06/base64.html)

另外，也可以看视频：[原理到实现 | 一个视频完全掌握base64编码](https://www.bilibili.com/video/BV1Wt4y1q7dH)





#### 浏览器指纹

通过Cookie来跟踪识别你的身份已经不是什么新技术了，也已经有了很多反跟踪的方法。但是现在存在一种方法，无需广告商为你设置标识，只需读取你的浏览器信息，就可以确定你的身份。这种方法称为“**浏览器指纹**”。

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
