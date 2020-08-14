





# Web相关备忘录



### <font color=FF0000>编写web程序时出现的问题</font>

- 出现如下情况：

  <img src="https://s1.ax1x.com/2020/07/05/US9DqP.png" style="zoom: 45%;" />

  其中<font color=FF0000>**一种解释原因**</font>（无法确认是所有）是：当前访问的页面不存在（物理上不存在） / 当前程序能访问到的中的资源中没有该资源（资源存在，但程序找不到）。



### <font color=FF0000>80端口和8080端口</font>

<font color=FF0000>80是**http协议的默认端口**</font>，是在输入网站的时候其实浏览器（非IE）已经帮你输入协议了，所以你输入http://baidu.com，其实是访问http://baidu.com:80，而<font color=FF0000>8080，一般用于webcahe</font>，<font color=FF0000>一般是用来连接代理的</font>。完全不一样的两个，比如linux服务器里apache默认跑80端口，而apache-tomcat默认跑8080端口，其实端口没有实际意义只是一个接口，主要是看服务的监听端口，如果baidu的服务器监听的81端口，那么你直接输入就不行了就要输入http://baidu.com:81这样才能正常访问

摘自：[80端口跟8080端口有什么具体区别？ - 死神的沙漏的回答 - 知乎](https://www.zhihu.com/question/26698345/answer/33709330)



## <font color=FF0000>URL</font>

URL中 `%十六进制数字` 代表的是十六进制的ASCII码



### <font color=FF0000>HTML的转义字符</font>

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



## <font color=FF0000>SOAP & REST</font>

#### SOAP

SOAP：简单对象访问协议（Simple Object Access Protocol）是一种<mark>基于 XML 的协议</mark>，可以和现存的许多因特网协议和格式结合使用，包括超文本传输协议（HTTP），简单邮件传输协议（SMTP），多用途网际邮件扩充协议（MIME），基于“通用”传输协议是 SOAP的一个优点。它还支持从消息系统到远程过程调用（Remote Procedure Call，RPC）等大量的应用程序。SOAP提供了一系列的标准，如WSRM（WS-Reliable Messaging）形式化契约确保可靠性与安全性，确保异步处理与调用；WS-Security、WS-Transactions和WS-Coordination等标准提供了上下文信息与对话状态管理

***

#### Rest

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

**<font color=FF0000>最常见的一种设计错误，就是URI包含动词</font>。**因为"资源"表示一种实体，所以应该是名词，URI不应该有动词，动词应该放在HTTP协议中。

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



## <font color=FF0000>POJO</font>

POJO（Plain Ordinary Java Object）即普通Java类，具有一部分getter/setter方法的那种类就可以称作POJO。

实际意义就是普通的JavaBeans（简单的实体类），特点就是<mark>支持业务逻辑的<font color=FF0000>协助类</font></mark>。
POJO类的作用是方便程序员使用数据库中的数据表，对于程序员来说，可以很方便的将POJO类当作对象来进行使用，也可以方便的调用其get，set方法。<font color=FF0000>但**不允许有业务方法**，也不能携带有connection之类的方法，即**不包含业务逻辑或持久逻辑等**</font>。

<mark>当一个Pojo可序列化，有一个无参的构造函数，使用getter和setter方法来访问属性时，他就是一个JavaBean</mark>。

摘自：[POJO和JavaBean的区别](https://blog.csdn.net/tiantangdizhibuxiang/article/details/81784873)

#### entity、bo、vo、po、dto、pojo的定义与区别

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



## <font color=FF0000>OAuth</font>

**OAuth（Open Authorization）**。OAuth协议为用户资源的授权提供了一个安全的、开放而又简易的标准。与以往的授权方式不同之处是OAuth的授权不会使第三方触及到用户的帐号信息（如用户名与密码），即<mark>第三方无需使用用户的用户名与密码就可以申请获得该用户资源的授权</mark>，因此OAuth是安全的。

摘自：[百度百科 - OAuth](https://baike.baidu.com/item/oAuth)

类似的可参考：[OAuth 2.0 的一个简单解释](http://www.ruanyifeng.com/blog/2019/04/oauth_design.html)



## <font color=FF0000>JSP</font>

**JSP的本质其实就是Servlet**。只是JSP当初设计的目的是为了简化Servlet输出HTML代码。

**JSP已经很少用了，被下面两种技术替代**

- 模板引擎（freemarker、Thymeleaf、Velocity），用法其实跟JSP差不太多，只是它们的性能会更好
- 前后端分离，后端只需要返回JSON给前端，页面完全不需要后端管

摘自：[2020年了，不学jsp后学什么代替？ - Java3y的回答 - 知乎 ](https://www.zhihu.com/question/382065572/answer/1102140696)



### <font color=FF0000>Web容器的作用域</font>

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



### <font color=FF0000>Session和Cookie</font>

会话（Session）：指用户登录网站后的一系列动作，比如浏览商品添加到购物车并购买。会话跟踪是 Web 程序中常用的技术，用来**跟踪用户的整个会话**。

由于HTTP协议是<font color=FF0000>无状态的协议</font>，所以服务端需要记录用户的状态时，就需要用某种机制来识具体的用户。由于HTTP协议无状态，所以并不知道是哪个用户操作的，所以服务端要为特定的用户创建了特定的Session，用用于标识这个用户。

**只使用session的痛点**：<font color=FF0000>Session 的致命弱点是不容易在多台服务器之间共享</font>，所以这也限制了 Session 的使用。这就需要用cookie

**<font color=FF0000>Cookie</font> 通过<font color=FF0000>在客户端记录</font>信息<font color=FF0000>确定用户身份</font>**，**<font color=FF0000>Session</font> 通过<font color=FF0000>在服务器端记录</font>信息<font color=FF0000>确定用户身份</font>**。

session 的运行依赖 session id，而 <font color=FF0000>session id 是存在 cookie 中的</font>，也就是说，如果浏览器禁用了 cookie ，同时 session 也会失效（但是可以通过其它方式实现，比如在 url 中传递 session_id）

**<mark style=background-color:aqua><font color=FF0000>如果客户端的浏览器禁用了 Cookie 怎么办？</font></mark>**一般这种情况下，会使用一种叫做<font color=FF0000>URL重写</font>的技术来<font color=FF0000>进行会话跟踪</font>，即每次HTTP交互，<font color=FF0000>URL后面都会被附加上一个诸如 sid=xxxxx 这样的参数，服务端据此来识别用户</font>。



### <font color=FF0000>双向绑定</font>

<font color=FF0000>单向绑定</font>就是把Model绑定到View，当我们用JavaScript代码更新Model时，View就会自动更新。

<font color=FF0000>双向绑定</font>就是如果用户更新了View，Model的数据也自动被更新了，这种情况就是双向绑定。