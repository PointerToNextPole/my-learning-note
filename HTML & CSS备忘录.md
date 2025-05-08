# HTML & CSS备忘录



## HTML



#### Lorem Ipsum

Lorem ipsum，简称为 Lipsum，是指一篇常用于排版设计领域的拉丁文文章，主要的目的为测试文章或文字在不同字型、版型下看起来的效果。中文的类似用法则称为乱数假文、随机假文。

摘自：[wiki - Lorem Ipsum](https://zh.wikipedia.org/wiki/Lorem_ipsum)

另外，在 html 页面中同样可以作为测试字使用。



#### HTML5 & CSS3

2017年看 《Head First HTML 与 CSS》，其中有说：“即使 HTML5 随未来发展有添加新特性，HTML 也只会叫‘新 HTML5’ ”

另外，今天（2022/9/2）在 [http/4会是什么样的？ - 紫云飞的回答 - 知乎](https://www.zhihu.com/question/536812181/answer/2521339835) 看到了类似的概念：

> 多年以来，“HTML5” 这个词更多是一个非规范的叫法，尤其是现在都已经 2022 年了，基本上我只能在国内的招聘 JD 里看到（不够专业）。现在的 HTML 规范早已经没有版本号了，更加不会有 HTML6 。
>
> CSS 也同样早已没有版本号，CSS3 也是个非规范叫法，基本只能在招聘 JD 里看到，十年前我翻译的文章：[\[译\]根本没有"CSS4" - 紫云飞 - 博客园](https://www.cnblogs.com/ziyunfei/archive/2012/12/11/2813263.html)



#### 缩写与全称

**`<hr>`** 是 horizontal rule 的缩写



#### `<label>`

HTML `<label>` 元素（标签）表示用户界面中某个元素的说明。

##### 将一个 `<label>` 和一个 `<input>` 元素相关联主要有这些优点

- 标签文本不仅与其相应的文本输入元素在视觉上相关联，程序中也是如此。 这意味着，当用户聚焦到这个表单输入元素时，屏幕阅读器可以:读出标签，让使用辅助技术的用户更容易理解应输入什么数据。
- 你可以点击关联的标签来聚焦或者激活这个输入元素，就像直接点击输入元素一样。这扩大了元素的可点击区域，让包括使用触屏设备在内的用户更容易激活这个元素。

<font color=FF0000>将一个 `<label>` 和一个 `<input>` 元素匹配在一起，你需要给 `<input>` 一个 `id` 属性。而 `<label>` 需要一个 `for` 属性，其值和 `<input>` 的 `id` 一样。</font>

<font color=LightSeaGreen>另外，你也可以将 `<input>` 直接放在 `<label>` 里，此时则不需要 `for` 和 `id` 属性，因为关联已隐含存在：</font>

```html
<label>Do you like peas?
  <input type="checkbox" name="peas">
</label>
```

##### 其他使用事项

- 关联标签的表单控件称为这个标签的已关联标签的控件。<font color=FF0000>一个 `<input>` 可以与多个标签相关联</font>。

- 点击或者轻触 ( tap ) 与表单控件相关联的 `<label>` 也可以触发关联控件的 click 事件。

##### 属性

该元素包含 全局属性。

- <font color=FF0000>**`for`** ：即和 `<label>` 元素在同一文档中的 可关联标签的元素 的 `id`</font>。 <font color=LightSeaGreen>文档中第一个 `id` 值与 `<label>` 元素 `for` 属性值相同的元素，如果可关联标签 ( labelable ) ，则为已关联标签的控件，其标签就是这个 `<label>` 元素</font>。如果这个元素不可关联标签，则 `for` 属性没有效果。如果文档中还有其他元素的 `id` 值也和 `for` 属性相同，`for` 属性对这些元素也没有影响。

  > ⚠️ `<label>` 元素可同时有一个 `for` 属性和一个子代控件元素，只是 `for` 属性需要指向这个控件元素

- **`form`** ：<font color=FF0000>表示与 `label` 元素关联的 `<form>` 元素（即它的表单拥有者）</font>。**如果声明了该属性，其值应是同一文档中 `<form>` 元素的 `id` 。因此你可以将 `label` 元素放在文档的任何位置，而不仅作为 `<form>` 元素的后代**

摘自：[MDN - `<label>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/label)



#### `<button>`

HTML `<button>` 元素表示一个可点击的按钮，可以用在表单或文档其它需要使用简单标准按钮的地方。 <font color=LightSeaGreen>默认情况下，HTML按钮的显示样式接近于 user agent 所在的宿主系统平台（用户操作系统）的按钮</font>， 但你可以使用 CSS 来改变按钮的样貌。

##### 属性

- **autofocus** ：**HTML5**，一个布尔属性，用于<font color=FF0000>指定当页面加载时按钮必须有输入焦点</font>，除非用户重写，例如通过不同控件键入。只有一个表单关联元素可以指定该属性。

- **autocomplete** ：仅 Firefox允许，略

- **disabled** ：此布尔属性<font color=FF0000>表示用户不能与button交互</font>。如果属性值未指定，button 继承包含元素，例如 `<fieldset>` ；如果没有设置 `disabled` 属性的包含元素，button 将可交互。

  不像其它浏览器，Firefox 默认在页面加载时持续禁用 Button 的动态状态。使用 `autocomplete` 属性可控制此特性。

- **form** ：**HTML5**，<font color=FF0000>表示 button 元素关联的 form 元素（它的表单拥有者）</font>。<font color=fuchsia>**此属性值必须为同一文档中的一个 `<form>` 元素的 `id` 属性**</font>。如果此属性未指定，`<button>` 元素必须是 form元素的后代。利用此属性，你可以将 `<button>` 元素放置在文档内的任何位置，而不仅仅是作为他们 form 元素的后代。

- **formaction** ：**HTML5**，<font color=FF0000>表示程序处理 button 提交信息的 URI</font>。<font color=LightSeaGreen>如果指定了，将重写 button 表单拥有者（即 form ）的 `action` 属性</font>。

- **formenctype** ：**HTML5**，<font color=FF0000>如果 button 是 **submit**类型，**此属性值指定提交表单到服务器的内容类型**</font>。可选值：

  - **`application/x-www-form-urlencoded`** ：未指定时的<font color=FF0000>**默认值**</font>。
  - **`multipart/form-data`** ：<font color=FF0000>如果使用 type 属性的 `<input>` 元素设置文件</font>，使用此值。
  - **`text/plain`** ：如果指定此属性，它将重写 button 的表单拥有者的 `enctype` 属性。

- **formmethod** ：**HTML5**，<font color=FF0000>如果 button 是 **submit** 类型，**此属性指定浏览器提交表单使用的 HTTP 方法**</font>。可选值:

  - **post** ：来自表单的数据被包含在表单内容中，被发送到服务器。
  - **get** ：来自表单的数据以 `?` 作为分隔符被附加到 form 的 URI 属性中，得到的 URI 被发送到服务器。当表单没有副作用，且仅包含 ASCII 字符时使用这种方法。

  <font color=FF0000>如果指定了，此属性会重写 button 拥有者（即 form）的 `method` 属性</font>。

- **formnovalidate** ：**HTML5**，<font color=FF0000>如果 button 是 **submit** 类型，**此布尔属性指定当表单被提交时不需要验证**</font>。如果指定了，它会重写 button 拥有者的 novalidate 属性。

  - **formtarget** ：**HTML5**，<font color=FF0000>如果 button 是 **submit** 类型，此属性指定一个名称或关键字，表示接收提交的表单后在哪里显示响应</font>。这是一个浏览上下文（例如 tab，window 或内联框架）的名称或关键字。如果指定了，它会重写 button 拥有者的 `target`  属性。关键字如下：
  - **`_self`** ：在同一个浏览上下文中加载响应作为当前的。未指定时此值为默认值。
  - **`_blank`** ：在一个新的不知名浏览上下文中加载响应。
  - **`_parent`** ：在当前浏览上下文父级中加载响应。如果没有父级的，此选项将按 `_self` 执行。
  - **`_top`** ：在顶级浏览上下文（即当前浏览上下文的祖先，且没有父级）中架加载响应。如果没有顶级的，此选项将按 `_self` 执行。

- **name** ：<font color=FF0000>button 的名称</font>，<font color=fuchsia>**与表单数据一起提交**</font>。

- **type** ：<font color=FF0000>button 的类型</font>。可选值：

  - **submit** ：<font color=FF0000>此按钮将表单数据提交给服务器</font>。如果未指定属性，或者属性动态更改为空值或无效值，则<font color=FF0000>此值为默认值</font>。
  - **reset** ：<font color=FF0000>此按钮重置所有组件为初始值</font>。
  - **button** ：<font color=FF0000>此按钮没有默认行为</font>。它可以有与元素事件相关的客户端脚本，当事件出现时可触发。
  - **menu** ：此按钮打开一个由指定 `<menu>` 元素进行定义的弹出菜单。

- **value：**<font color=FF0000>button 的初始值</font>。它定义的值与表单数据的提交按钮相关联。<font color=FF0000>当表单中的数据被提交时，这个值便以参数的形式被递送至服务器</font>。

摘自：[MDN - `<button>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/button)



#### `<a>`

> 💡 a 是 anchor 的缩写。

HTML `<a>` 元素（或称锚元素）可以通过它的 href 属性创建通向其他网页、文件、同一页面内的位置、电子邮件地址或任何其他 URL 的超链接。`<a>` 中的内容应该应该指明链接的意图。<font color=LightSeaGreen>如果存在 href 属性，当 `<a>` 元素聚焦时按下回车键就会激活它</font>。

##### 属性

该元素的属性包含全局属性

###### download

HTML5，<font color=FF0000>此属性指示**浏览器下载 URL** 而不是导航到它</font>，因此将提示用户将其保存为本地文件。<font color=FF0000>如果属性有一个值，那么此值将在下载保存过程中作为预填充的文件名</font>（<font color=LightSeaGreen>如果用户需要，仍然可以更改文件名</font>）。此属性对允许的值没有限制，但是 / 和 \ 会被转换为下划线。大多数文件系统限制了文件名中的标点符号，故此，浏览器将相应地调整建议的文件名。

- 此属性<font color=FF0000>**仅适用于同源 URL**</font>。
- <font color=LightSeaGreen>尽管 HTTP URL 需要位于同一源中</font>，<font color=FF0000>但是**可以使用 `blob: URL` 和 `data: URL`** ，以方便用户下载使用 JavaScript 生成的内容</font>（例如使用在线绘图 Web 应用程序创建的照片）。
- 如果 HTTP 头中的 Content-Disposition 属性赋予了一个不同于此属性的文件名，HTTP 头属性优先于此属性
- 如果 HTTP 头属性 Content-Disposition 被设置为 inline（即 Content-Disposition='inline' ），那么 Firefox 优先考虑 HTTP 头 Content-Dispositiondownload 属性。

###### href

href 包含超链接指向的 URL 或 URL 片段。

<font color=FF0000>URL 片段是哈希标记 ( # ) 前面的名称</font>，哈希标记（即：锚点链接）指定当前文档中的内部目标位置（ HTML 元素的 ID ）。<font color=FF0000>URL 不限于基于 Web ( HTTP ) 的文档，也可以使用浏览器支持的任何协议</font>。<font color=LightSeaGreen>例如，在大多数浏览器中正常工作的 `file:` 、`ftp:` 和 `mailto:`</font>

**注意：**<font color=LightSeaGreen>可以使用 `href="#top"` 或者 `href="#"` 链接返回到页面顶部</font>。<font color=FF0000>这种行为是 HTML5 的特性</font>。

###### hreflang

该属性<font color=FF0000>用于指定链接文档的人类语言</font>。其<font color=LightSeaGreen>仅提供建议，并没有内置的功能</font>。hreflang 允许的值取决于 HTML5 BCP47。

###### ping

包含一个以空格分隔的 url 列表，<font color=LightSeaGreen>当跟随超链接时，将由浏览器(在后台)发送带有正文 PING 的 POST 请求</font>。<font color=LightSeaGreen>通常用于跟踪</font>。

###### referrerpolicy

<font color=FF0000>**实验性质**</font>，<font color=FF0000>表明在获取URL时发送哪个提交者</font> ( referrer ):

- **no-referrer：**<font color=FF0000>表示 Referer: 头将不会被发送</font>。
- **no-referrer-when-downgrade：**<font color=FF0000>表示当从使用HTTPS的页面导航到不使用 TLS(HTTPS)的来源 时不会发送 Referer: 头</font>。<font color=FF0000>**如果没有指定策略，这将是用户代理的 <font size=4>默认行为</font>**</font>。
- **origin：**<font color=FF0000>表示 referrer将会是页面的来源</font>，大致为这样的组合：主机和端口（不包含具体的路径和参数的信息）。
- **origin-when-cross-origin：**表示导航到其它源将会限制为这种组合：主机 + 端口，而导航到相同的源将会只包含 referrer 的路径
- **strict-origin-when-cross-origin**
- **unsafe-url：**表示 referrer将会包含源和路径 ( domain + path )（但是不包含密码或用户名的片段）。这种情况是不安全的，因为它可能会将安全的 URLs 数据泄露给不安全的源。

###### rel

该属性<font color=LightSeaGreen>指定了目标对象到链接对象的关系</font>。该值是空格分隔的列表类型值。

###### target

该属性<font color=FF0000>**指定在何处显示链接的资源**</font>。 <font color=FF0000>取值为标签 ( tab )，窗口( window )，或框架 ( iframe ) 等浏览上下文的名称或其他关键词</font>。以下关键字具有特殊的意义:

- **`_self`** ：<font color=FF0000>当前页面加载</font>，即当前的响应到同一HTML 4 frame（或HTML5浏览上下文）。<font color=FF0000>**此值是默认的**，如果没有指定属性的话</font>。
- **`_blank`** ：<font color=FF0000>新窗口打开</font>，即到一个<font color=LightSeaGreen>新的未命名</font>的HTML4窗口或HTML5浏览器上下文
- **`_parent`** ：<font color=LightSeaGreen>加载响应到当前框架的HTML4父框架或当前的HTML5浏览上下文的父浏览上下文</font>。<font color=FF0000>如果没有parent框架或者浏览上下文，此选项的行为方式与 `_self` 相同</font>。
- **`_top`** ：HTML4中：加载的响应成完整的，原来的窗口，取消所有其它frame。 HTML5中：加载响应进入顶层浏览上下文（即，浏览上下文，它是当前的一个的祖先，并且没有parent）。如果没有parent框架或者浏览上下文，此选项的行为方式相同 `_self`

> ⚠️ 在 `<a>` 元素上使用 `target="_blank"` 隐式提供了与使用 `rel="noopener"` 相同的 rel 行为，即不会设置 `window.opener`

###### type

该属性指定在一个 MIME type 链接目标的形式的媒体类型。其仅提供建议，并没有内置的功能。

##### 已废除的属性

- **charset**（ HTML5 已废除）
- **coords**（ HTML4 有效，HTML5 已废除）
- **name**（ HTML4 有效，HTML5 已废除）
- **rev**（ HTML4 有效，HTML5 已废除）
- **sharp**（ HTML4 有效，HTML5 已废除）

##### `<a>` 的作用

###### 链接到外部地址

```html
<a href="http://www.mozilla.com/">External Link</a>
```

###### 链接到本页的某个部分

> 👀 即作为 锚点

```html
<a href="#属性">Description of Same-Page Links</a>
```

###### 创建一个可点击的图片

类似的使得用 \<a>包裹 div，使得整个 div 都可以点击跳转

```html
<a href="https://developer.mozilla.org/en-US/">
  <img src="https://mdn.mozillademos.org/files/6851/mdn_logo.png" alt="MDN logo" />
</a>
```

###### 创建一个 email 链接

这是常见的创建按钮或链接，将用户的电子邮件程序打开，让他们发送新邮件。这是通过使用一个 mailto 链接完成的。这里有一个简单的例子：

```html
<a href="mailto:nowhere@mozilla.org">Send email to nowhere</a>
```

###### 创建电话链接

提供电话链接有助于用户查看连接到手机的网络文档和笔记本电脑。

```html
<a href="tel:+491570156">+49 157 0156</a>
```

###### 发短信

```html
<a href="sms:"13764567708">13764567708</a>
```

> 💡 发现这里的无论是 mailto scheme（见下面的补充）、tel scheme 以及 sms scheme 都和 url scheme 有些类似；可以说：URL Scheme 是一种自定义的协议
>
> <font color=dodgerBlue>关于 mailto scheme 这个说法的来源：</font>
>
> > **mailto** <font color=red>is a Uniform Resource Identifier (URI) **scheme**</font> for email addresses. It is used to produce hyperlinks on websites that allow users to send an email to a specific address directly from an HTML document, without having to copy it and entering it into an email client.
> >
> > 摘自：[wikipedia - mailto](https://en.wikipedia.org/wiki/Mailto)
>
> 另外，可以看下 [MDN - `Navigator.registerProtocolHandler()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Navigator/registerProtocolHandler) 和 [一言难尽的registerProtocolHandler()方法](https://www.zhangxinxu.com/wordpress/2023/08/js-registerprotocolhandler/) 其中介绍了 浏览器内置的 scheme（如下），以及用使用 `Navigator.registerProtocolHandler()` 自定义 scheme（感觉和 URL Scheme 大差不差）
>
> **浏览器内置 scheme （白名单中的协议标记）：**
>
> > `bitcoin`、 `geo`、 `im`、 `irc`、 `ircs`、 `magnet`、 `mailto`、 `mms`、 `news`、 `nntp`、 `openpgp4fpr`、 `sip`、 `sms`、 `smsto`、 `ssh`、 `tel`、 `urn`、 `webcal`、 `wtai`、 `xmpp`
> >
> > 摘自：[MDN - `Navigator.registerProtocolHandler()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Navigator/registerProtocolHandler)

###### 使用 `download` 属性将 `<canvas>` 保存为 PNG 格式

有点没搞懂，这里略，详见引用链接

摘自：[MDN - `<a>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a)



#### `<pre>`

HTML `<pre>` 元素表示预定义格式文本。<font color=FF0000>在该元素中的文本通常按照原文件中的编排，以等宽字体的形式展现出来，文本中的空白符（比如空格和换行符）都会显示出来</font>。（<font color=LightSeaGreen>紧跟在 `<pre>` 开始标签后的换行符也会被省略</font>）

摘自：[MDN - `<pre>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/pre)



#### form

HTML `<form>` 元素表示文档中的一个区域，此区域包含交互控件，用于向 Web 服务器提交信息。

<font color=FF0000>可以用 **`:valid`** 和 **`:invalid`** CSS 伪类来设置 `<form>` 元素的样式，此时样式的表现取决于表单中的 elements 是否有效</font>。

##### 属性

此元素拥有 全局属性。

###### `accept`

🗑️ 一个逗号分隔的列表，包括服务器能接受的内容类型。

<font color=LightSeaGreen>此属性已在 HTML5 中被移除并且不再被使用</font>。<font color=FF0000>作为替代，可以使用 `<input type=file>` 元素中的 `accept` 属性</font>

###### `accept-charset`

<font color=LightSeaGreen>一个空格分隔或逗号分隔的列表</font>，此<font color=FF0000>列表包括了服务器支持的字符编码</font>。<font color=FF0000>浏览器**以这些编码被列举的顺序使用它们**</font>。<font color=FF0000>默认值是一个保留字符串 "UNKNOWN"</font>。此字符串指的是，和包含此表单元素的文档相同的编码。
<font color=LightSeaGreen>在之前版本的 HTML 中，不同的字符编码可以用空格或逗号分隔</font>。<font color=FF0000>在 HTML5 中，**只有空格可以允许作为分隔符**</font>

###### `autocapitalize`

iOS Safari 特有，略

###### `autocomplete`

用于指示 input 元素是否能够拥有一个默认值，<font color=LightSeaGreen>此默认值是由浏览器自动补全的</font>。此设定可以被属于此表单的子元素的 `autocomplete` 属性覆盖。 可能的值有：

- **`off`** ：<font color=FF0000>浏览器可能不会自动补全条目</font>（在疑似登录表单中，浏览器倾向于忽略该值，而提供密码自动填充功能，参见 自动填充属性和登录）
- **`on`** ：<font color=FF0000>浏览器可自动补全条目</font>

###### `name`

<font color=FF0000>表单的名称</font>。<font color=LightSeaGreen>HTML4 中不推荐（应使用 id）</font>。<font color=FF0000>**在 HTML5 中，该值必须是所有表单中独一无二的，而且不能是空字符串**</font>。

###### `rel`

根据 value 创建一个超链接或 Creates a hyperlink or annotation depending on the value

##### 关于提交表单的属性

###### `action`

<font color=FF0000>处理表单提交的 URL</font>。

这个值可被 `<button>`、`<input type="submit">` 或 `<input type="image">` 元素上的 `formaction` 属性覆盖

###### `enctype`

<font color=FF0000>当 `method` 属性值为 **post** 时，**`enctype` 就是将表单的内容提交给服务器的 MIME 类型**</font> 。可能的取值有：

- **`application/x-www-form-urlencoded`** ：<font color=FF0000 size=4>未指定属性时的 **默认值**</font>。
- **`multipart/form-data`** ：<font color=LightSeaGreen>当表单包含 `type=file` 的 `<input>` 元素时使用此值</font>。
- **`text/plain`** ：<font color=LightSeaGreen>出现于 HTML5，用于调试</font>。

这个值可被 `<button>`、`<input type="submit">` 或 `<input type="image">` 元素上的 `formenctype` 属性覆盖。

###### method

浏览器使用哪种 HTTP 方式来提交 表单. 可能的值有：

- **`post`** ：指的是 HTTP POST 方法；<font color=FF0000>表单数据会包含在表单体内然后发送给服务器</font>.
- **`get`** ：指的是 HTTP GET 方法；<font color=LightSeaGreen>表单数据会附加在 `action` 属性的 URL 中，并以 '?' 作为分隔符</font>，没有副作用 时使用这个方法。
- **`dialog`** ：<font color=LightSeaGreen>如果表单在 `<dialog>` 元素中，提交时关闭对话框</font>。

此值可以被 `<button>` 、`<input type="submit">` 或 `<input type="image">` 元素中的 `formmethod` 属性覆盖

###### `novalidate`

<font color=FF0000>此布尔值属性表示提交表单时不需要验证表单</font>。 <font color=FF0000>如果没有声明该属性 （因此表单需要通过验证）</font>。

该属性可以被表单中的 `<button>`、`<input type="submit">` 或 `<input type="image">` 元素中的 `formnovalidate` 属性覆盖。

###### `target`

<font color=LightSeaGreen>表示在提交表单之后，在哪里显示响应信息</font>。在 HTML4 中, 这是一个 frame 的名字/关键字对。<font color=FF0000>在 HTML5 里，这是一个浏览上下文 的名字/关键字（如标签页、窗口或 iframe）</font>。下述关键字有特别含义：

- **`_self`** ：<font color=FF0000>**默认值**</font>。<font color=FF0000>在相同浏览上下文中加载</font>。
- **`_blank`** ：<font color=FF0000>在新的未命名的浏览上下文中加载</font>。
- **`_parent`** ：在当前上下文的父级浏览上下文中加载，<font color=FF0000>如果没有父级，则与 `_self` 表现一致</font>。
- **`_top`** ：在最顶级的浏览上下文中（即当前上下文的一个没有父级的祖先浏览上下文），<font color=FF0000>如果没有父级，则与 `_self` 表现一致</font>。

此值可以被 `<button>`、 `<input type="submit">` 或 `<input type="image">` 元素中的 `formtarget` 属性覆盖

摘自：[MDN - form](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/form)

##### 表单相关伪类

###### `:valid`

`:valid` CSS 伪类 <font color=FF0000>表示内容验证正确的 `<input>` 或其他 `<form>` 元素</font>。这<font color=FF0000>能简单地将校验字段展示为一种能让用户辨别出其输入数据的正确性的样式</font>。

```css
/* Selects any valid <input> */
 input:valid {
   background-color: powderblue;
}
```

该伪类对于高亮正确字段是很有用的。

摘自：[MDN - `:valid`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:valid)

##### `:invalid`

`:invalid` CSS 伪类 <font color=FF0000>表示任意内容未通过验证的 `<input>` 或其他 `<form>` 元素 </font>

```css
/* 可选定任意无效的 <input> */
input:invalid {
  background-color: pink;
}
```

这个伪类对于突出显示用户的字段错误非常有用。

摘自：[MDN - `:invalid`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:invalid)



#### `<dialog>`

HTML `<dialog>` 元素<font color=FF0000>表示一个对话框或其他交互式组件</font>，例如一个检查器或者窗口。

##### 属性

这个元素包含了全局属性。但是 `tabindex` 特性不能被使用在 `<dialog>` 元素上。

- **`open`** ：<font color=FF0000>指示这个对话框是激活的和能互动的</font>。<font color=LightSeaGreen>当这个 open 特性没有被设置，对话框不应该显示给用户</font>。

##### 使用备注

- <font color=FF0000>`<form>` 元素可在此对话框中使用，但需要指定属性 `method="dialog"`</font>。当提交表单时，对话框的 returnValue 属性将会等于表单中被使用的提交按钮的 value 。
- `::backdrop` CSS 伪元素可用于更改 `<dialog>` 背景元素样式，例如在对话框被打开激活时，调暗背景中不可访问的内容。仅当使用 `HTMLDialogElement.showModal()` 显示对话框时才会绘制 backdrop 背景。

摘自：[MDN - `<dialog>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/dialog)



#### `<fieldset>`

HTML `<fieldset>` 元素用于对表单中的控制元素<font color=FF0000>进行分组</font>（也包括 label 元素）。

`<fieldset>` 元素将一个HTML表单的一部分组成一组，<font color=FF0000>内置了一个 `<legend>` 元素作为 fieldset 的标题</font>。这个元素有几个属性，最值得注意的是 form，其可以包含同一页面的 `<form>` 元素的 id，以使 <font color=FF0000>`<fieldset>` 成为这个 `<form>` 的一部分</font>，即使 `<fieldset>` 不在其内。还有 `disabled` 属性，可将 `<fieldset>` 及其所有内容设置为不可用。

##### 属性

<font color=FF0000>这个元素包含所有全局属性</font>。

- **`disabled`** ：如果设置了这个 bool 值属性，`<fieldset>` 的所有子代表单控件也会继承这个属性。<font color=FF0000>这意味着它们不可编辑，也不会随着 `<form>` 一起提交</font>。它们也不会接收到任何浏览器事件，如鼠标点击或与聚焦相关的事件。默认情况下，浏览器会将这样的控件展示为灰色。注意，`<legend>` 中的表单元素不会被禁用。
- **`form`** ：将该值<font color=FF0000>设为一个 `<form>` 元素的 `id` 属性值以将 `<fieldset>` 设置成这个 `<form>` 的一部分</font>。      
- **`name`** ：元素分组 ( fieldset ) 的名称

> ⚠️ fieldset 的标题由第一个 `<legend>` 子元素确定。

摘自：[MDN - `<fieldset>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/fieldset)

#### `<legend>`

HTML `<legend>` 元素用于表示其父元素 `<fieldset>` 的内容标题。

摘自：[MDN - `<legend>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/legend)



#### `<meta>`

HTML `<meta>` 元素表示那些不能由其它 HTML 元相关 ( meta-related ) 元素 ( `<base>` 、`<link>` 、`<script>` 、`<style>` 或 `<title>` ) 之一表示的任何元数据信息。

|                  | 描述                                                         |
| :--------------- | ------------------------------------------------------------ |
| 内容分类         | 元数据内容，如果 `itemprop` 属性存在：流数据，表述内容       |
| 允许的内容       | 无，这是一个 空元素                                          |
| 标签省略         | 因为这是一个 void 元素，必须有开始标签而闭合标签可以省略     |
| 允许的父元素     | `<meta charset>` ， `<meta http-equiv>: <head>` 元素. 如果 http-equiv 不是编码声明, 它也可以放在 `<noscript>` 元素内，它本身在 `<head>` 元素内部 |
| 默认无障碍语义   | 没有相应的语义                                               |
| 允许的无障碍语义 | 没有允许的语义                                               |
| DOM 接口         | `HTMLMetaElement`                                            |

##### meta 元素定义的元数据的类型包括以下几种

- 如果<font color=FF0000>设置了 `name` 属性</font>，meta 元素<font color=FF0000>提供的是文档级别 ( document-level ) 的元数据</font>，应用于整个页面。

- 如果 <font color=dodgerBlue>**设置了 `http-equiv` 属性**</font>，**meta 元素则<font color=FF0000>是 <font color=fuchsia>编译指令</font></font>**，提供的信息与类似命名的 HTTP 头部相同。

- 如果<font color=FF0000>设置了 `charset` 属性</font>，meta 元素<font color=FF0000>是一个字符集声明</font>，<font color=FF0000>告诉文档使用哪种字符编码</font>。

- 如果<font color=FF0000>设置了 `itemprop` 属性</font>，meta 元素<font color=FF0000>提供用户定义的元数据</font>。

##### 属性

> ⚠️ 全局属性 name 在 `<meta>` 元素中具有特殊的语义；另外，<font color=FF0000>  在 **同一个 `<meta>` 标签中**，`name`、`http-equiv` 或者 `charset` 三者中 **任何一个属性存在** 时，**`itemprop` 属性不能被使用** </font>。

###### `charset`

这个属性<font color=FF0000>声明了文档的字符编码</font>。<font color=dodgerBlue>如果使用了这个属性</font>，<font color=FF0000>其值必须是</font>与 ASCII 大小写无关 ( ASCII case-insensitive ) 的 <font color=FF0000>"utf-8"</font>

###### `content`

此属性包含 `http-equiv` 或 `name` 属性的值，具体取决于所使用的值。

###### `http-equiv`

属性定义了一个编译指示指令。这个属性叫做 `http-equiv(alent)` 是因为<font color=FF0000> 所有允许的值都是特定 HTTP 头部的名称</font>，如下：

- **`content-security-policy`** ：它允许页面作者<font color=FF0000> 定义当前页的内容策略</font>。 内容策略<font color=FF0000> 主要指定允许的服务器源和脚本端点，这有助于防止跨站点脚本攻击</font>。

- **`content-type`** ：<font color=FF0000> 如果使用这个属性，其值必须是 "`text/html; charset=utf-8`"</font>。

  > ⚠️ 该属性只能用于 MIME type 为 `text/html` 的文档，不能用于 MIME 类型为 XML 的文档。

- **`default-style`** ：设置默认 CSS 样式表组的名称。

- **`x-ua-compatible`** ：<font color=FF0000>如果指定，则 `content` 属性必须具有值 "IE=edge" </font>。用户代理必须忽略此指示。

  > x-ua-compatible 一般是用来做 IE 适配的。
  >
  > 另外，还可以是 "IE=edge,chrome=1"
  >
  > IE=edge 告诉浏览器，以当前浏览器支持的最新版本来渲染，IE9 就以 IE9 版本来渲染。
  > chrome=1 告诉浏览器，如果当前IE浏览器安装了Google Chrome Frame 插件，就以 chrome 内核来渲染页面。
  > 像上图这种两者都存在的情况：如果有 chrome 插件，就以 chrome 内核渲染，如果没有，就以当前浏览器支持的最高版本渲染。
  > 另外，这个属性支持的范围是IE8 ～ IE11
  >
  > 你可能注意到了，如果在我们的 http 头部中也设置了这个属性，并且和 meta 中设置的有冲突，那么哪一个优先呢？ 答案是：开发者偏好（ meta 元素）优先于Web服务器设置（ HTTP 头）。
  >
  > 摘自：[作为前端，你必须要知道的meta标签知识](https://juejin.cn/post/7089271039842058253)

- **`refresh`** ：这个属性指定：
  
  - 如果 <font color=FF0000> `content` **只包含**一个正整数，则为 **重新载入页面的时间间隔（秒）** </font>；
  - 如果 <font color=FF0000>`content` 包含一个正整数，并且后面跟着字符串 ';url=' 和一个合法的 URL</font>，则是<font color=FF0000> **重定向到指定链接**的时间间隔（秒）</font>
  
- **`x-dns-prefetch-control`**

  > 一般来说，HTML 页面中的 a 标签会自动启用 DNS 提前解析来提升网站性能，但是在使用 https 协议的网站中失效了，我们可以设置：
  >
  > ```html
  > <meta http-equiv="x-dns-prefetch-control" content="on">
  > ```
  >
  > 来打开 dns 对 a 标签的提前解析
  >
  > 摘自：[作为前端，你必须要知道的meta标签知识](https://juejin.cn/post/7089271039842058253)
  
  > 💡 还有一个 `dns-prefetch` ，详见 [[#dns-prefetch 补充]] 。区别是： `x-dns-prefetch-control` 是 `http-equiv` 中的值，`dns-prefetch` 是 `rel` 中的值。另外，可以看下 [[计算机网络#X-DNS-Prefetch-Control#示例]]

###### `name`

<font color=FF0000>`name` 和 `content` 属性可以一起使用，以名-值对的方式给文档提供元数据</font>，其中 `name` 作为元数据的名称，`content` 作为元数据的值。

> 💡 补充：
>
> ###### 主题颜色控制
>
> ```html
> <meta name="theme-color" content="#4285f4">
> ```
>
> `<meta name="theme-color">` 可以用作 “主题颜色控制”。
>
> > ###### theme-color
> >
> > `<meta>` 中 `name` 属性的值为 `theme-color` 时，用户的浏览器将根据所设定的建议颜色来改变用户界面。倘若设置了该值，则 `content` 属性必须包含有效的 CSS `<color>` 值。
> >
> > 下图展示了上方所示的 `<meta>` 元素对于 Android 端 Chrome 浏览器造成的影响。
> >
> > <img src="https://s2.loli.net/2025/02/25/z628P9v3IsQ5ExH.png" alt="示 theme-color 对浏览器的影响" style="zoom:60%;" />
> >
> > 你可以用 media 来查询一个媒体类型，如果条件符合则使用对应颜色。比如：
> >
> > ```html
> > <meta name="theme-color" media="(prefers-color-scheme: light)" content="white" />
> > <meta name="theme-color" media="(prefers-color-scheme: dark)" content="black" />
> > ```
> >
> > 摘自：[MDN - `theme-color`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta/name/theme-color)
>
> ###### iOS 全屏模式
>
> ```html
> <meta name="apple-mobile-web-app-capable" content="yes">
> ```
>
> 学习自：[HTML中不为人知的细节小知识](https://juejin.cn/post/7469992360517271603)

###### `media`

The `media` attribute defines which media the theme color defined in the `content` attribute should be applied to. Its value is a [media query](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_media_queries/Using_media_queries), which defaults to `all` if the attribute is missing. This attribute is only relevant when the element's [`name`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta/name) attribute is set to [`theme-color`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta/name/theme-color). Otherwise, it has no effect, and should not be included.

See [standard metadata names](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta/name) for details about the set of standard metadata names defined in the HTML specification.

摘自：[MDN - `<meta>`：文档级元数据元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta) 、[MDN en - `<meta>`: The metadata element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta)

##### `<link rel="canonical">`

###### 什么是权威内容标签

<font color=lightSeaGreen>权威内容标签是一种 HTML 代码片段</font>，主要<font color=red>用于定义重复、近似重复或类似页面的主要版本</font>。换句话说，<font color=fuchsia>如果不同的网址上出现了相同或相似的内容，则可以使用权威内容标签去指定一个应该被索引的主要版本</font>。

###### 权威内容标签长的形式

权威内容标签使用简单一致的语法，放置在网页的 `<head>` 部分中：

```html
<link rel="canonical" href="https://example.com/sample-page/" />
```

说得明白点，每个部分的含义如下：

- **`link rel="canonical"`** ：带有此标记的链接是此网页的主（权威）版本。

- **`href="https://example.com/sample-page/"`** ：<font color=fuchsia>可以在这个网址找到权威版本</font>。

###### 为什么权威内容标签对于  SEO  来说如此重要

<font color=red>Google 并不喜欢重复的内容，因为这让它们很难抉择</font>：

1. 要索引哪一个页面版本？（它们只会索引一个！）
2. 哪一个页面版本可在相关查询中拔得头筹？
3. 是否应该在一个页面上整合出一个“链接权益”，或者在多个页面版本之间进行拆分？

再说<font color=red>过多的重复内容也会影响你的“抓取配额”</font>。<font color=fuchsia>这意味着 Google 最终可能会放弃浪费时间，抓取同一页面的多个版本，而不是发掘出你网站上的其他重要内容</font>。

> ###### 💡 关于抓取配额的真相？
>
> 当然，应该尽可能避免让 Google 浪费时间去抓取重复的内容。但是，Google 表示，这对大多数网站来说只是小菜一碟。
>
> > 如果新的网页往往会在发布当天被抓取，那么抓取配额不是网站管理员需要关注的东西。同样，如果一个站点的网址少于几千个，那么大多数时候它都会被有效抓取。

<font color=red>权威内容标签解决了所有这些问题</font>。<font color=fuchsia>它们能让 Google 知道你想索引和排名哪个页面版本，以及在哪里巩固 “链接权益”的位置</font>。

摘自：[权威内容标签—简明入门指南](https://ahrefs.com/blog/zh/canonical-tags/) 这里只是摘抄了概念部分，后面还有更多篇幅介绍了使用与使用技巧（以后做 SEO 的话可以继续回来看）。另外，也可以看下 [rel=canonical: the ultimate guide](https://yoast.com/rel-canonical/)

##### `http-equiv`

`http-equiv` 顾名思义，<font color=FF0000>**相当于 http 的文件头作用**</font>，它可以向浏览器传回一些有用的信息，以帮助正确和精确地显示网页内容，与之对应的属性值为 `content`，`content` 中的内容其实就是各个参数的变量值。

> 💡 <font color=FF0000>`http-equiv` 的 equiv 是 equivalent （译为：同等物 ）的缩写，所以 `http-equiv` 即 “ http 同等物”</font>

> equiv does stand for equivalent. <font color=FF0000>It's **equivalent to the HTTP response header**</font>. For example, these two are the same
>
> ```html
> <META http-equiv="content-type" content="text/html; charset=UTF-8">
> ```
>
> ```http
> Content-Type: text/html; charset=utf-8
> ```
>
> The HTTP header should be used over the `http-equiv` meta tag though.
>
> 摘自：[http-equiv - what's equiv stand for?](https://stackoverflow.com/questions/33967395/http-equiv-whats-equiv-stand-for)

meta 标签的 `http-equiv` 属性语法格式是：`<meta http-equiv="参数" content="参数变量值">` ；其中 `http-equiv` 属性主要有以下几种参数：

###### `Expires`

期限。可以用于设定网页的到期时间。一旦网页过期，必须到服务器上重新传输。必须使用 GMT 的时间格式。用法：

```html
<meta http-equiv="expires" content="Wed, 20 Jun 2007 22:33:00 GMT">
```

###### `Pragma`

cache 模式。是用于设定禁止浏览器从本地机的缓存中调阅页面内容，设定后一旦离开网页就无法从 Cache 中再调出。这样设定，访问者将无法脱机浏览。用法：

```html
<meta http-equiv="Pragma" content="no-cache">
```

###### `Refresh`

刷新。自动刷新并指向新页面。用法：

```html
<meta http-equiv="Refresh" content="2；URL=http://www.net.cn/">
```

> 其中的2是指停留 2秒钟后自动刷新到 URL 网址。

- **Set-Cookie（ cookie 设定）：**如果网页过期，那么存盘的cookie将被删除。 注意：必须使用GMT的时间格式。用法：

  ```html
  <meta http-equiv="Set-Cookie" content="cookievalue=xxx;expires=Wednesday, 20-Jun-2007 22:33:00 GMT; path=/">
  ```

- **Window-target（显示窗口的设定）：**强制页面在当前窗口以独立页面显示。注意：用来防止别人在框架里调用自己的页面。用法

  ```html
  <meta http-equiv="Window-target" content="_top">
  ```

###### `content-Type`

显示字符集的设定，设定页面使用的字符集。用法：

```html
<meta http-equiv="content-Type" content="text/html; charset=gb2312">
```

###### `Pics-label`

网页等级评定。在 IE 的 internet 选项中有一项内容设置，可以防止浏览一些受限制的网站，而网站的限制级别就是通过 meta 属性来设置的。用法：

```html
<meta http-equiv="Pics-label" contect="">  
```

###### Page_Enter、Page_Exit

**Page_Enter：**设定进入页面时的特殊效果

```html
<meta http-equiv="Page-Enter" contect="revealTrans(duration=1.0,transtion=12)">
```

**Page_Exit：**设定离开页面时的特殊效果

```html
<meta http-equiv="Page-Exit" contect="revealTrans(duration=1.0,transtion=12)">
```

Duration 的值为网页动态过渡的时间，单位为秒。

Transition 是过渡方式，它的值为 0 到 23 ，分别对应 24 种过渡方式。如下表：

|                          |                          |                          |                          |
| ------------------------ | ------------------------ | ------------------------ | ------------------------ |
| 0：盒状收缩              | 1：盒状放射              | 2：圆形收缩              | 3：圆形放射              |
| 4：由下往上              | 5：由上往下              | 6：从左至右              | 7：从右至左              |
| 8：垂直百叶窗            | 9：水平百叶窗            | 10：水平格状百叶窗       | 11：垂直格状百叶窗       |
| 12：随意溶解             | 13：从左右两端向中间展开 | 14：从中间向左右两端展开 | 15：从上下两端向中间展开 |
| 16：从中间向上下两端展开 | 17：从右上角向左下角展开 | 18：从右下角向左上角展开 | 19：从左上角向右下角展开 |
| 20：从左下角向右上角展开 | 21：水平线状展开         | 22：垂直线状展开         | 23：随机产生一种过渡方式 |

###### `cache-control`

清除缓存。再访问这个网站要重新下载

```html
<meta http-equiv="cache-control" content="no-cache">
```

###### `expires`

设定网页的到期时间：

```html
<meta http-equiv="expires" content="0">
```

###### `keywords`

关键字，给搜索引擎用的

```html
<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
```

###### `description`

页面描述

```html
<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
```

摘自：[Meta Http-equiv属性详解](https://cnbin.github.io/blog/2016/02/03/http-equivshu-xing-xiang-jie/)

##### `format-detection`

`format-detection` （是 meta 标签内的属性）翻译成中文是“格式检测”，是用来检测 html 里的一些格式的，meta 中的 `format-detection` 属性主要有以下几个设置：

```html
<meta name="format-detection" content="telephone=no">
<meta name="format-detection" content="email=no">
<meta name="format-detection" content="adress=no">
```

也可以连写：

```html
<meta name="format-detection" content="telephone=no,email=no,adress=no">
```

###### 解释

- `telephone` ：你明明写的一串数字没加链接样式，而 iPhone 会自动把你这个文字加链接样式、并且点击这个数字还会自动拨号！想去掉这个拨号链接该如何操作呢？这时 meta 又该大显神通了，代码如下：
  - **`telephone=no`** ：就<font color=FF0000> 禁止</font>了把数字转化为拨号链接
  - **`telephone=yes`** ：就<font color=FF0000> 开启</font>了把数字转化为拨号链接，要开启转化功能，这个 meta 就不用写了，在默认是情况下就是开启
  
- `email` ：告诉设备不识别邮箱，点击之后不自动发送
  - **`email=no`** ：禁止作为邮箱地址
  
  - **`email=yes`** ：就开启了把文字默认为邮箱地址，这个 meta 就不用写了,在默认是情况下就是开启！
  
- `adress`
  - `adress=no` ：禁止跳转至地图
  - `adress=yes` ：就开启了点击地址直接跳转至地图的功能,在默认是情况下就是开启

摘自：[meta的format-detection属性](https://www.jianshu.com/p/82a85a53d5b4)

> 👀  打电话、发送邮件可以通过 [[#创建电话链接]] 和 [[#创建一个 email 链接]] 实现



##### `<meta>` name-content 键值对的补充

###### robots

表示爬虫对此页面的处理行为，或者说，应当遵守的规则，是用来做搜索引擎抓取的。

###### `content` 可以是

- all：搜索引擎将索引此网页，并继续通过此网页的链接索引文件将被检索
- none：搜索引擎讲忽略此网页
- index：搜索引擎索引此网页
- follow：搜索引擎继续通过此网页的链接索引搜索其它的网页

###### renderer

用来指定双核浏览器的渲染方式。比如 360浏览器，我们可以通过这个设置来指定 360浏览器的渲染方式

```html
<!-- 默认 webkit 内核 -->
<meta name="renderer" content="webkit">
<!-- 默认 IE 兼容模式 -->
<meta name="renderer" content="ie-comp">
<!-- 默认 IE 标准模式 -->
<meta name="renderer" content="ie-stand">
```

摘自：[作为前端，你必须要知道的meta标签知识](https://juejin.cn/post/7089271039842058253)



#### `<base>`

<font color=FF0000> HTML `<base>` 元素 指定用于一个文档中包含的所有相对 URL 的根 URL。一份（文档）中只能有一个 `<base>` 元素</font>。

一个文档的基本 URL，可以通过使用 `document.baseURI` 的 JS 脚本查询。如果文档不包含 `<base>` 元素，baseURI 默认为 `document.location.href` 。

| 属性             | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| 内容类别         | 元数据内容。                                                 |
| 合法的内容       | 无，它是一个 empty element                                   |
| 标签省略         | <font color=FF0000> 该标签不能有结束标签</font>。            |
| 合法的父级       | <font color=FF0000> 任何不带有任何其他 `<base>` 元素的 `<head>` 元素</font> |
| 合法的 ARIA 角色 | 无                                                           |
| DOM 接口         | `HTMLBaseElement`                                            |

**示例：**

```html
<base href="http://www.example.com/">
<base target="_blank">
<base target="_top" href="http://www.example.com/">
```

**属性**

<font color=FF0000> **如果指定了以下任一属性，这个元素必须在其他任何属性是 URL 的元素之前**</font>。例如：`<link>` 的 href 属性。

###### `href`

<font color=LightSeaGreen>用于文档中相对 URL 地址的基础 URL。允许绝对和相对URL</font>。

###### `target`

<font color=LightSeaGreen>默认浏览上下文的关键字或作者定义的名称</font>，<font color=LightSeaGreen>当没有明确目标的链接 `<a>` 或表单 `<form>` 导致导航被激活时显示其结果</font>。该属性值定位到浏览上下文（例如选项卡，窗口或内联框 `<iframe>` ）。

<font color=dodgerBlue>**以下的关键字指定特殊的意思：**</font>

- **`_self`** ：载入结果到当前浏览上下文中。（该值是元素的默认值）。
- **`_blank`** ：载入结果到一个新的未命名的浏览上下文。
- **`_parent`** ：载入结果到父级浏览上下文（如果当前页是内联框）。如果没有父级结构，该选项的行为和 `_self` 一样。
- **`_top`** ：载入结果到顶级浏览上下文（该浏览上下文是当前上下文的最顶级上下文）。如果没有父级，该选项的行为和 `_self` 一样。

##### 使用说明

###### 多个 `<base>` 元素

<font color=LightSeaGreen>如果指定了多个 `<base>` 元素</font>，<font color=FF0000> 只会使用第一个 `href` 和 `target` 值，其余都会被忽略</font>。

###### 页内锚

指向文档中某个片段的链接，例如 `<a href="#some-id">` 用 `<base>` 解析，触发对带有附加片段的基本 URL 的 HTTP 请求。

例如：给定 `<base href="https://example.com">`

以及此链接 `<a href="#anchor">Anker</a>`

链接指向 https://example.com/#anchor

摘自：[MDN - `<base>`：文档根 URL 元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/base)



#### HTML5 新属性

| 属性         | 值                                                     | 描述                                                |
| :----------- | :----------------------------------------------------- | :-------------------------------------------------- |
| `charset`    | character_set                                          | 定义文档的字符编码。                                |
| `content`    | text                                                   | 定义与 `http-equiv` 或 `name` 属性相关的元信息。    |
| `http-equiv` | content-type default-style refresh                     | 把 `content` 属性关联到 HTTP 头部。                 |
| `name`       | application-name author description generator keywords | 把 `content` 属性关联到一个名称。                   |
| `scheme`     | format/URI                                             | HTML5不支持。 定义用于翻译 `content` 属性值的格式。 |

#### 表单属性

- **`maxlength`** / **`minlength`** ：用于 `<input>` 和 `<textarea>` ，作用是限制输入最多的和最少的字符数量
- **`autocomplete`** ：可用于以文本或数字值作为输入的 `<input>` 元素 ， `<textarea>` 元素， `<select>` 元素, 和 `<form>` 元素。 `autocomplete` 允许 web开发人员指定，如果有任何权限 user agent 必须提供填写表单字段值的自动帮助，并为浏览器提供关于字段中所期望的信息类型的指导。<font color=LightSeaGreen>建议值的来源通常取决于浏览器。 通常，值来自用户输入的过去值，但它们也可能来自预先配置的值。 </font>

#### data-*（数据属性）

HTML5 是具有扩展性的设计，它初衷是数据应与特定的元素相关联，但不需要任何定义。<font color=FF0000>`data-*` 属性允许我们在标准内于 HTML 元素中存储额外的信息</font>，而不需要使用类似于 `classList` ，标准外属性，DOM 额外属性或是 setUserData 之类的技巧。

##### HTML 语法

语法非常简单。所有在元素上以 `data-` 开头的属性为数据属性。比如说你有一篇文章，而你又想要存储一些不需要显示在浏览器上的额外信息。请使用 data 属性：

```html
<article
  id="electriccars"
  data-columns="3"
  data-index-number="12314"
  data-parent="cars"
>
	...
</article>
```

##### JavaScript 访问

在外部使用 JavaScript 去访问这些属性的值同样非常简单。你<font color=FF0000>可以使用 `getAttribute()` 配合它们完整的 HTML 名称去读取它们</font>，<font color=FF0000>但**标准定义了一个更简单的方法：DOMStringMap 你可以使用 dataset 读取到数据**</font>。

为了使用 dataset 对象去获取到数据属性，需要获取属性名中 **`data-`** 之后的部分(要注意的是：<font color=FF0000>破折号连接的名称需要改写为骆驼拼写法</font>（如 "index-number" 转换为 "indexNumber" )。

```js
var article = document.querySelector('#electriccars');

article.dataset.columns // "3"
article.dataset.indexNumber // "12314"
article.dataset.parent // "cars"
```

##### CSS 访问

> ⚠️ data 设定为 HTML 属性，他们同样能被 CSS 访问。比如你可以通过 generated content 使用函数 `attr()` 来显示 `data-parent` 的内容：

```css
article::before {
  content: attr(data-parent);
}
```

你也同样可以在 CSS 中使用属性选择器根据 data 来改变样式：

```css
article[data-columns='3'] {
  width: 400px;
}
article[data-columns='4'] {
  width: 600px;
}
```

摘自：[MDN - 使用数据属性](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Howto/Use_data_attributes)

> 💡 补充
>
> `data-*` 属性用于存储页面或应用程序的<font color=FF0000>私有自定义数据</font>。
>
> `data-*` 属性赋予我们在所有 HTML 元素上嵌入自定义 data 属性的能力。
>
> **`data-*` 属性包括两部分：**
>
> - <font color=FF0000>属性名不应该包含任何大写字母</font>，并且在前缀 "data-" 之后必须有至少一个字符
> - 属性值可以是任意字符串
>
> 摘自：[w3school - HTML data-* 属性](https://www.w3school.com.cn/tags/att_global_data.asp)
>



#### CDATA

**CDATA**，意为 character data，是标记语言 SGML 与 XML，<font color=FF0000>表示文档的特定部分是普通的字符数据</font>，而不是非字符数据或有特定、限定结构的字符数据。

**摘自：**[wiki - CDATA](https://zh.wikipedia.org/wiki/CDATA)

<font color=FF0000>所有 XML 文档中的文本均会被解析器解析，除了 CDATA 区段 ( CDATA section ) 中的文本会被解析器忽略。</font>

CDATA 的形式如下：`<![CDATA[ 文本内容 ]]>` 。

CDATA 的文本内容中不能出现字符串 `]]>`。另外，CDATA 不能嵌套。

XML 实例：在 CDATA 标记中的信息被解析器原封不动地传给应用程序，并且不解析该段信息中的任何控制标记。 CDATA 区域是由 `<![CDATA[` 为开始标记，以 `]]>` 为结束标记，注意 CDATA 为大写。

**摘自：**[XML 中的 ﹤!\[CDATA\[]\]>，及其解析](https://blog.csdn.net/qq_33266987/article/details/54288051)

#### PCDATA

PCDATA 指的是<font color=FF0000>被解析的字符数据</font> ( Parsed Character Data ) 。

XML 解析器通常会解析 XML 文档中所有的文本。

<font color=FF0000>当某个 XML 元素被解析时，其标签之间的文本也会被解析：</font>

```xml
<message>此文本也会被解析</message>
```

解析器之所以这么做是因为 XML 元素可包含其他元素，就像这个例子中，其中的 `<name>` 元素包含着另外的两个元素（ first 和 last ）：

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



#### `<img>`

`<img>` HTML 元素将一张图像嵌入文档。

##### 示例

```html
<img
  class="fit-picture"
  src="/media/cc0-images/grapefruit-slice-332-332.jpg"
  alt="Grapefruit slice atop a pile of other slices"
/>
```

<font color=dodgerBlue>上面的例子展示了 `<img>` 元素的用法：</font>

- `src` 属性是 **必须的**，它包含了你想嵌入的图片的路径。
- `alt` 属性包含一条对图像的文本描述，这<font color=LightSeaGreen>不是强制性的</font>，但<font color=red>对无障碍而言，它**难以置信地有用**</font>——<font color=LightSeaGreen>屏幕阅读器会将这些描述读给需要使用阅读器的使用者听，让他们知道图像的含义</font>。如果由于某种原因无法加载图像，普通浏览器也会在页面上显示 `alt` 属性中的备用文本：例如，网络错误、内容被屏蔽或链接过期。

<font color=dodgerBlue>**还有很多其他属性，可以实现各种不同的目的：**</font>

- Referrer / CORS 控制，保证安全与隐私：详见 `crossorigin` 和 `referrerpolicy` 属性。
- 使用 `width` 和 `height` 设置图像的固有尺寸（intrinsic size）：这将设置图像应占用的空间，以确保图像被加载之前页面的布局是稳定的。
- 使用 `sizes` 和 `srcset` 设置响应式图像

##### 支持的图像格式

HTML 标准并没有给出需要支持的图像格式的列表，因此每个用户代理支持一组不同的格式。

> 👀 [网页浏览器图像格式指南](https://developer.mozilla.org/zh-CN/docs/Web/Media/Formats/Image_types)提供了有关图像格式及 Web 浏览器支持的综合信息。本节只是一个总结！

<font color=dodgerBlue>Web 最常用的图像格式是：</font>

- [APNG（动态可移植网络图形）](https://developer.mozilla.org/zh-CN/docs/Web/Media/Formats/Image_types#apng_animated_portable_network_graphics)——无损动画序列的不错选择（GIF 性能较差）。
- [AVIF（AV1 图像文件格式）](https://developer.mozilla.org/zh-CN/docs/Web/Media/Formats/Image_types#avif_image)——静态图像或动画的不错选择，其性能较好。
- [GIF（图像互换格式）](https://developer.mozilla.org/zh-CN/docs/Web/Media/Formats/Image_types#gif_graphics_interchange_format)——*简单*图像和动画的不错选择。
- [JPEG（联合图像专家组）](https://developer.mozilla.org/zh-CN/docs/Web/Media/Formats/Image_types#jpeg_joint_photographic_experts_group_image)——有损压缩静态图像的不错选择（目前最流行的格式）。
- [PNG（便携式网络图形）](https://developer.mozilla.org/zh-CN/docs/Web/Media/Formats/Image_types#png_portable_network_graphics)——对于无损压缩静态图像而言是不错的选择（质量略好于 JPEG）。
- [SVG（可缩放矢量图形）](https://developer.mozilla.org/zh-CN/docs/Web/Media/Formats/Image_types#svg_scalable_vector_graphics)——矢量图像格式。用于必须以不同尺寸准确描绘的图像。
- [WebP（网络图片格式）](https://developer.mozilla.org/zh-CN/docs/Web/Media/Formats/Image_types#webp_image)——图像和动画的绝佳选择。

推荐使用诸如 [WebP](https://developer.mozilla.org/zh-CN/docs/Web/Media/Formats/Image_types#webp_image) 和 [AVIF](https://developer.mozilla.org/zh-CN/docs/Web/Media/Formats/Image_types#avif_image) 等图像格式，因为它们在静态图像和动画的性能均比 PNG、JPEG、JIF 好得多。WebP 得到了广泛的支持，而 AVIF 则缺乏 Safari 的支持。

对于必须以不同尺寸准确绘制的图像，则仍然推荐使用 SVG 格式。

##### 图像加载错误

如果在加载或渲染图像时发生错误，且设置了至少一个 `onerror` 事件处理器来处理 `error` 事件，那么设置的事件处理器就会被调用。这样的错误可能发生在各种不同的情况下，包括：

- `src` 属性的属性值为空（`""`）或者 `null`。
- `src` 属性的 URL 和用户正在浏览的页面的 URL 完全相同。
- 图像出于某些原因损坏了，从而无法加载。
- <font color=red>图像的元数据被破坏了，无法检索它的分辨率/宽高，而且在 `<img>` 元素的属性中没有指定宽度/高度</font>。
- <font color=red>用户代理尚未支持该图片所用的格式</font>。

##### 属性

此元素支持全局属性 

###### `src`

图像的 URL，这个属性对 `<img>` 元素来说是必需的。<font color=red>在支持 `srcset` 的浏览器中，`src` 被当做拥有一个像素密度的描述符 `1x` 的候选图像处理</font>，除非一个图像拥有这个像素密度描述符已经被在 `srcset` 或者 `srcset` 包含 `w` 描述符中定义了。

###### `alt`

定义了图像的备用文本描述。

如果把这个属性设置为空字符串 ( `alt=""` ) ，则表明该图像*不是* 内容的关键部分（这是一种装饰或者一个追踪像素点），非可视化浏览器在渲染的时候可能会忽略它。而且，如果图片加载失败，可视化浏览器会隐藏表示图片损坏的图标。

将图像复制并粘贴为文本，或是将图像的链接保存为浏览器书签时，也会用到此属性。

###### `crossorigin`

这个枚举属性表明是否必须使用 CORS 完成相关图像的抓取。[启用 CORS 的图像](https://developer.mozilla.org/zh-CN/docs/Web/HTML/CORS_enabled_image)可以在 `<canvas>` 元素中重复使用，而不会被标记为“[污染](https://developer.mozilla.org/zh-CN/docs/Web/HTML/CORS_enabled_image#安全性和“被污染”的_canvas)（tainted）”。

如果*未指定*  `crossorigin` 属性，则会发送不启用 CORS 的请求（不会携带 `Origin` 请求标头），且浏览器会将图像标记为“被污染”并拒绝对图像数据的访问，阻止其在 `<canvas>` 元素中的使用。

如果 *指定* 了 `crossorigin` 属性，则会发送启用 CORS 的请求（会携带 `Origin` 请求标头）；但是如果服务器不选择允许源站点对图像数据进行跨源访问（不发送任何 `Access-Control-Allow-Origin` 响应标头，或发送的 `Access-Control-Allow-Origin` 标头中不包含源站点），浏览器则会阻止图像的加载，并在开发者工具的控制台中记录 CORS 错误。

<font color=dodgerBlue>**允许的值有：**</font>

- `anonymous` ：发送忽略凭据的跨源请求（即，不携带 cookie、[X.509 证书](https://datatracker.ietf.org/doc/html/rfc5280) 或 `Authorization` 请求标头）。
- `use-credentials ` ：发送携带凭据的跨源请求（比如 cookie、X.509 证书和 `Authorization` 请求标头）。如果服务器不选择与源站共享凭据（通过返回 `Access-Control-Allow-Credentials: true` 响应标头） ，则浏览器会将图像标记为被污染且限制对其图像数据的访问。

如果属性是无效值，浏览器默认将其当做 `anonymous` 关键字。

###### `decoding`

为浏览器提供图像解码方式上的提示。允许的值：

- `sync` ：同步解码图像，实现与其他内容互斥的原子渲染。
- `async` ：异步解码图像，以减少其他内容的渲染延迟。
- `auto` ：默认值：不指定解码方式，由浏览器决定哪一种对用户来说是最合适的。

###### `fetchpriority`

🧪 提供获取图像时要使用的相对的优先级提示。允许的值：

- `high` ：表示其获取优先级相对其他图像要高。
- `low` ：表示其获取优先级相对其他图像要低。
- `auto `：默认值：表示自动确定其相对其他图像的获取优先级。

> 💡可以看下 [前端加载优化小技巧 —— 使用fetchpriority=high属性为页面最大资源的加载提速](https://juejin.cn/post/7134684645228347400) 作为补充

###### `height`

图像的固有高度，以像素为单位。必须是没有单位的整数值。

> 💡 **备注：**同时包括 `height` 和 `width` 使浏览器在加载图像之前计算图像的长宽比。此长宽比用于保留显示图像所需的空间，减少甚至防止在下载图像并将其绘制到屏幕上时布局的偏移。减少布局偏移是良好用户体验和 Web 性能的主要组成部分。

###### `width`

图像的宽度，以像素为单位。必须是没有单位的整数。

###### `srcset`

以逗号分隔的一个或多个字符串列表表明一系列用户代理使用的可能的图像。每一个字符串由以下组成：

1. 指向图像的 URL。
2. 可选地，再加一个空格之后，附加以下的其一：
   - 一个宽度描述符（一个正整数，后面紧跟 `w` 符号）。该整数宽度除以 `sizes` 属性给出的资源（source）大小来计算得到有效的像素密度，即换算成和 x 描述符等价的值。
   - 一个像素密度描述符（一个正浮点数，后面紧跟 `x` 符号）。

如果没有指定源描述符，那它会被指定为默认的 `1x`。

在相同的 `srcset` 属性中混合使用宽度描述符和像素密度描述符时，会导致该值无效。重复的描述符（比如，两个源在相同的 `srcset` 两个源都是 `2x`）也是无效的。

用户代理自行决定选择任何可用的来源。这位它们提供了一个很大的选择余地，可以根据用户偏好或带宽条件等因素来进行选择。

###### `loading`

指示浏览器应当如何加载该图像。允许的值：

- `eager` ：立即加载图像，不管它是否在可视视口 ( visible viewport ) 之外（默认值）。
- `lazy` ：延迟加载图像，直到它和视口接近到一个计算得到的距离（由浏览器定义）。目的是在需要图像之前，避免加载图像所需要的网络和存储带宽。这通常会提高大多数典型用场景中内容的性能。

###### `referrerpolicy`

一个字符串，指示在获取资源时使用的来源地址（referrer）：

- `no-referrer`：不会发送 `Referer` 标头。
- `no-referrer-when-downgrade`：若未使用 TLS（HTTPS）导航到源站，则不发送 `Referer` 标头。
- `origin`：发送到源站的来源地址将被限制为：协议、主机 和 端口
- `origin-when-cross-origin`：发送到其他来源的来源地址会被限制为协议、主机和端口。同源导航仍将包含路径。
- `same-origin`：仅同源请求发送来源地址，而跨源请求则不包含来源地址信息。
- `strict-origin`：仅在协议安全级别保持不变 ( HTTPS→HTTPS ) 的情况下将文档的来源作为来源地址发送。而在目标的安全性降低 ( HTTPS→HTTP ) 时则不发送来源地址。
- `strict-origin-when-cross-origin`（默认值）：执行同源请求时发送完整的 URL，且仅在协议安全级别保持不变（HTTPS→HTTPS）时发送来源 ( origin ) ，在目标安全性降低（HTTPS→HTTP）时则不发送来源。
- `unsafe-url`：来源地址包括来源 ( origin ) *和* 路径（path）（但不包括[片段](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLAnchorElement/hash)、密码或用户名）。**这个值是不安全的**，因为它将来源和路径从受 TLS 保护的资源泄露到不安全的来源。

###### `sizes`

表示资源大小的、以逗号隔开的一个或多个字符串。每一个资源大小包括：

1. 一个 [媒体条件](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media_Queries/Using_media_queries#语法)。最后一项一定是被忽略的。
2. 一个资源大小的值。

媒体条件描述 *视口* 的属性，而不是*图像*的属性。例如，如果 *视口* 不高于 500px，则建议使用 1000px 宽的资源：`(max-height: 500px) 1000px`。

资源尺寸的值被用来指定图像的预期尺寸。当 `srcset` 中的资源使用了宽度描述符 `w` 时，用户代理会使用当前图像大小来选择 `srcset` 中合适的一个图像 URL。被选中的尺寸影响图像的[显示大小](https://developer.mozilla.org/en-US/docs/Glossary/Intrinsic_Size)（如果没有影响大小的 CSS 样式被应用的话）。如果没有设置 `srcset` 属性，或者没有属性值，那么 `sizes` 属性也将不起作用。

###### `ismap`

这个布尔属性表示图像是否是[服务器端图像映射](https://en.wikipedia.org/wiki/Image_map#Server-side)的一部分。如果是，那么点击图片的精准坐标将会被发送到服务器。

> 💡 **备注：** 只有在 `<img>` 元素是一个拥有有效 `href` 属性的 `<a>` 元素的后代元素的情况下，这个属性才会被允许使用。

###### `usemap`

与元素相关联的 [图像映射( image map )](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/map)的部分 URL（以 `#` 开始的部分）。

> 💡 **备注：** 如果 `<img>` 元素是 `<a>` 或 `<button>` 元素的后代元素则不能使用这个属性。

摘自：[MDN - `<img>`：图像嵌入元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img)



#### `<picture>`

HTML `<picture>` 元素 <font color=FF0000> 通过包含零或多个 `<source>` 元素和一个 `<img>` 元素来为不同的显示/设备场景提供图像版本</font>。<font color=LightSeaGreen>浏览器会选择最匹配的子 `<source>` 元素</font>，<font color=FF0000> 如果没有匹配的，就选择 `<img>` 元素的 src 属性中的 URL</font>。然后，所选图像呈现在 `<img>` 元素占据的空间中。

##### 示例

```html
<picture>
  <source srcset="/media/cc0-images/surfer-240-200.jpg" media="(min-width: 800px)">
  <img src="/media/cc0-images/painted-hand-298-332.jpg" alt="" />
</picture>
```

要决定加载哪个 URL，<font color=FF0000> user agent 检查每个 `<source>` 的 `srcset` 、`media` 和 `type` 属性</font>，<font color=LightSeaGreen>来选择最匹配页面当前布局、显示设备特征等的兼容图像</font>。

| 属性              | 描述                                                         |
| :---------------- | ------------------------------------------------------------ |
| 内容分类          | 流内容，表述内容，嵌入内容。                                 |
| 允许的内容        | 零或多个 `<source>` 元素，以及紧随其后的一个 `<img>` 元素，可以混合一些脚本支持的元素。 |
| 标签省略          | <font color=FF0000> 不允许，开始标签和结束标签都不能省略</font>。 |
| 允许的父元素      | 任何可以包含嵌入内容的元素。                                 |
| 允许的 ARIA roles | 无                                                           |
| DOM 接口          | `HTMLPictureElement`                                         |

你可以使用 `object-position` 属性调整元素框架内图像的位置，用 `object-fit` 属性控制图片如何调整大小来适应框架。另外，<font color=FF0000> **是在子 `<img>` 元素上使用这些属性，不是 `<picture>` 元素**</font>。

##### 属性

###### `media` 属性

`media` 属性<font color=FF0000>允许你提供一个用于给用户代理作为选择 `<source>` 元素的依据的媒体条件 ( media condition )</font>（类似于媒体查询）。如果这个媒体条件匹配结果为 `false` ，那么这个 `<source>` 元素会被跳过。

###### `type` 属性

`type` 属性<font color=FF0000>允许你为 `<source>` 元素的 `srcset` 属性指向的资源指定一个 MIME 类型</font>。如果用户代理不支持指定的类型，那么这个 `<source>` 元素会被跳过。

摘自：[MDN - `<picture>`：picture 元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/picture)



#### source

HTML `<source>` 元素为 `<picture>` , `<audio>` 或者 `<video>` 元素指定多个媒体资源。这是一个空元素。它通常用于以不同浏览器支持的多种格式提供相同的媒体内容。

摘自：[MDN - Source](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/source)



#### CORS 设置属性

在 HTML5 中，一些 HTML 元素提供了对 CORS 的支持， <font color=FF0000>例如 `<audio>` 、`<img>` 、`<link>` 、`<script>` 和 `<video>` 均有一个跨域属性 ( crossOrigin property )，它允许你配置元素获取数据的 CORS 请求</font>。 在 “媒体元素” 上被使用的 <font color=FF0000>`crossorigin` 内容属性是一个 CORS 设置属性</font>。

<font color=dodgerBlue>这些属性是枚举的，并具有以下可能的值：</font>

- `anonymous` ：请求使用了 CORS 标头，且证书标志被设置为 `'same-origin'`。没有通过 cookies、客户端 SSL 证书或 HTTP 认证交换**用户凭据**，除非目的地是同一来源。

- `use-credentials` ：请求使用了 CORS 标头，且证书标志被设置为 `'include'`。总是包含**用户凭据**。

- `""` ：将属性名称设置为空值，如 `crossorigin` 或 `crossorigin=""`，与设置为 `anonymous` 的效果一样。

<font color=red>不合法的关键字或空字符串会视为 `anonymous` 关键字</font>。

默认情况下（即未指定该属性时），CORS 根本不会使用。用户代理不会要求对资源进行完全访问的许可，在跨源请求的情况下，将根据相关元素的类型进行某些限制：

| 元素                    | 限制                                                         |
| ----------------------- | ------------------------------------------------------------ |
| `img`、`audio`、`video` | 当资源被放置在 `<canvas>` 中时，元素会标记为[*被污染的*](https://developer.mozilla.org/zh-CN/docs/Web/HTML/CORS_enabled_image#安全性和“被污染”的_canvas)。 |
| `script`                | 对错误日志 `window.onerror` 的访问将会被限制。               |
| `link`                  | 使用了不合适的 `crossorigin` 标头的请求可能会被丢弃。        |

##### 示例

###### 使用 `crossorigin` 的 `<script>` 元素

你可以使用下面的 `<script>` 元素告诉浏览器执行来自 `https://example.com/example-framework.js` 的脚本且不发送用户凭据。

```html
<script src="https://example.com/example-framework.js" crossorigin="anonymous"></script>
```

###### 带有用户凭据的 Web 清单

在获取需要用户凭据的 manifest 时，属性值必须设置为 `use-credentials`。即使是同源的情况。

```html
<link rel="manifest" href="/app.webmanifest" crossorigin="use-credentials">
```

摘自：[MDN - HTML 属性：crossorigin](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Attributes/crossorigin)



#### AppCache 的相关的离线缓存

> ⚠️ AppCache 已过时，请使用 Service Worker 实现功能

HTML5 的离线存储是基于一个新建的 `.appcache` 文件的缓存机制，<font color=FF0000>通过 `.appcache` 文件上的 **解析清单** 离线存储资源</font>，这些资源就会像 cookie 一样被存储了下来。之后当网络在处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示。使用方法如下： 

1. 创建一个和 html 同名的 manifest 文件，然后在页面头部加入 manifest 属性：

   ```html
   <html lang="en" manifest="index.manifest">
   ```

2. 在 cache.manifest 文件中编写需要离线存储的资源：

   ```
   CACHE MANIFEST
       #v0.11
       CACHE:
       js/app.js
       css/style.css
       NETWORK:
       resourse/logo.png
       FALLBACK:
       / /offline.html
   ```

   - **CACHE** ：表示需要离线存储的资源列表，由于包含 manifest 文件的页面将被自动离线存储，所以不需要把页面自身也列出来。

   - **NETWORK** ：表示在它下面列出来的资源只有在在线的情况下才能访问，他们不会被离线存储，所以在离线情况下无法使用这些资源。不过，如果在 CACHE 和 NETWORK 中有一个相同的资源，那么这个资源还是会被离线存储，也就是说 CACHE 的优先级更高。

   - **FALLBACK** ：表示如果访问第一个资源失败，那么就使用第二个资源来替换他，比如上面这个文件表示的就是如果访问根目录下任何一个资源失败了，那么就去访问 offline.html 。

3. 在离线状态时，操作 `window.applicationCache` 进行离线缓存的操作。

##### 浏览器是如何对 HTML5 的离线储存资源进行管理和加载

- <font color=dodgerBlue>**在线的情况下**</font>，浏览器发现 html 头部有 manifest 属性，它会请求 manifest 文件，如果是第一次访问页面 ，那么浏览器就会根据 manifest 文件的内容下载相应的资源并且进行离线存储。如果已经访问过页面并且资源已经进行离线存储了，那么浏览器就会使用离线的资源加载页面，然后浏览器会对比新的 manifest 文件与旧的 manifest 文件，如果文件没有发生改变，就不做任何操作，如果文件改变了，就会重新下载文件中的资源并进行离线存储。

- <font color=dodgerBlue>**离线的情况下**</font>，浏览器会直接使用离线存储的资源。

摘自：[「2021」高频前端面试题汇总之HTML篇](https://juejin.cn/post/6905294475539513352)



#### HTML 语义化

**HTML 语义化是什么？**

语义化是指<font color=FF0000>根据内容的结构化（内容语义化），选择合适的标签（代码语义化）</font>，<font color=LightSeaGreen>便于开发者阅读和写出更优雅的代码的同时，让浏览器的爬虫和机器很好的解析</font>。

**为什么要语义化？**

- 有利于 SEO，有助于爬虫抓取更多的有效信息，爬虫是依赖于标签来确定上下文和各个关键字的权重。
- 语义化的 HTML 在没有 CSS 的情况下也能呈现较好的内容结构与代码结构
- 方便其他设备的解析。支持读屏软件，根据文章可以自动生成目录
- 可读性好，结构更加清晰， 便于团队开发和维护

摘自：[HTML语义化](https://segmentfault.com/a/1190000005626375)



#### HTML5 全局属性

<font color=LightSeaGreen>全局属性是**所有 HTML 元素共有的属性**，它们可以用于所有元素，即使属性可能对某些元素不起作用</font>

<font color=FF0000>我们可以在所有的 HTML 元素上指定全局属性，甚至是在标准里没有指定的元素</font>。<font color=LightSeaGreen>这意味着任何非标准元素仍必须能够应用这些属性，即使使用这些元素意味着文档不再是 html5 兼容的</font>。例如，虽然 `<foo>` 不是一个有效的 HTML 元素，但是 html5 兼容的浏览器隐藏了标记为 `<foo hidden>...</foo>` 的内容。

**除了基本的 HTML 全局属性之外，还存在以下全局属性:**

- **`xml:lang` 和 `xml:base`** —— <font color=LightSeaGreen>两者都是从 XHTML 规范继承，但为了兼容性而被保留的</font>。
- **多重 `aria-*` 属性**，<font color=LightSeaGreen>用于改善可访问性</font>。
- **事件处理程序 属性：**`on+事件名称`。👀 由于过多，这里略；详见原文。

##### 全局属性列表

###### `accesskey`

<font color=LightSeaGreen>提供了为当前元素生成键盘快捷键的提示</font>。这个属性由空格分隔的字符列表组成。浏览器应该使用在计算机键盘布局上存在的第一个。

###### `autocapitalize`

<font color=FF0000>**控制用户的文本输入是否和如何自动大写**</font>，它可以有以下的值：

- **`off` or `none`**，没有应用自动大写（<font color=FF0000>所有字母都默认为小写字母</font>）。
- **`on` or `sentences`**，<font color=FF0000>每个 ***句子*** 的第一个字母默认为大写字母；所有其他字母都默认为小写字母</font>。
- **`words`**，<font color=FF0000>每个 ***单词*** 的第一个字母默认为大写字母；所有其他字母都默认为小写字母</font>。
- **`characters`**，<font color=FF0000>所有的字母都应该默认为大写</font>。

###### `class`

<font color=FF0000>一个以空格分隔的元素的类名 ( classes )</font> <font color=fuchsia>**列表**</font>，它允许 CSS 和 Javascript 通过类选择器  ( class selectors ) 或 DOM 方法 ( `document.getElementsByClassName()` ) 来选择和访问特定的元素。

###### `contenteditable`

一个<font color=FF0000>枚举属性</font> ( enumerated attribute )，<font color=FF0000>表示元素是否可被用户编辑</font>。 如果可以，浏览器会调整元素的部件 ( widget ) 以允许编辑。

- **`true` 或者空字符串**，表明元素是可被编辑的；
- **`false`**，表明元素不能被编辑。

###### `contextmenu`

`<menu>` 的 `id` ，作为该元素的上下文菜单（<font color=FF0000>已经不被支持，将从所有浏览器中删除</font>）。

###### `data-*`

<font color=FF0000>一类自定义数据属性</font>，<font color=LightSeaGreen>它赋予我们在所有 HTML 元素上嵌入自定义数据属性的能力</font>，并可以通过脚本（一般指 JavaScript ） 与 HTML 之间进行专有数据的交换。所有这些自定义数据属性都可以通过所属元素的 `HTMLElement` 接口来访问。 <font color=FF0000>**`HTMLElement.dataset` 属性可以访问它们**</font>。

###### `dir`

<font color=FF0000>一个指示元素中 **文本方向** 的枚举属性</font>。它的取值如下：

- **`ltr`** , 指<font color=FF0000>从左到右</font>，用于那种从左向右书写的语言（比如英语）；
- **`rtl`** , 指<font color=FF0000>从右到左</font>，用于那种从右向左书写的语言（比如阿拉伯语）；
- **`auto`** , 指<font color=FF0000>由用户代理决定方向</font>。它在解析元素中字符时会运用一个基本算法，直到发现一个具有强方向性的字符，然后将这一方向应用于整个元素。

###### `draggable`

一种枚举属性，<font color=FF0000>指示是否可以 使用 Drag and Drop API 拖动元素</font>。它可以有以下的值：

- **`true`** ：<font color=FF0000>表明元素可能被拖动</font>
- **`false`** ：<font color=FF0000>表明元素可能不会被拖动</font>

###### `dropzone`

🧪 枚举属性，<font color=FF0000>指示可以使用 Drag and Drop API 在元素上**拖拽哪些类型的内容**</font>。 它可以具有以下值：

- **`copy`**，表示 drop 将创建被拖动元素的副本
- **`move`**，表示拖动的元素将移动到此新位置。
- **`link`**，将创建一个指向拖动数据的链接。

###### `exportparts`

🧪 Used to transitively export shadow parts from a nested shadow tree into a containing light tree.

###### `hidden`

布尔属性，<font color=FF0000>表示该元素尚未或不再相关</font>。例如，它可用于隐藏在登录过程完成之前无法使用的页面元素。浏览器不会呈现此类元素。不得使用此属性隐藏可合法显示的内容

###### `id`

定义唯一标识符 ( ID )，<font color=FF0000>该标识符在整个文档中必须是唯一的</font>。 其目的是在链接（使用片段标识符），脚本或样式（使用 CSS ）时标识元素。

###### `inputmode`

<font color=FF0000>向浏览器提供有关在编辑此元素或其内容时 <font color=fuchsia>**要使用的虚拟键盘配置类型**</font> 的提示</font>。主要用于 `<input>` 元素，但在 `contenteditable` 模式下可用于任何元素。

> 它可以是以下值：
>
> - **`"none"`** ：无虚拟键盘。在应用程序或者站点需要实现自己的键盘输入控件时很有用。
> - **`"text"`** ：（<font color=FF0000>**默认值**</font>）使用用户本地区域设置的标准文本输入键盘。
> - **`"decimal"`** ：小数输入键盘，包含数字和分隔符（通常是“ . ”或者“ , ”），设备可能也可能不显示减号键。
> - **`"numeric"`** ：数字输入键盘，所需要的就是 0 到 9 的数字，设备可能也可能不显示减号键。
> - **`"tel"`** ：电话输入键盘，包含 0 到 9 的数字、星号 ( * ) 和井号 ( # ) 键。表单输入里面的电话输入通常应该使用 `<input type="tel">`
> - **`"search"`** ：为搜索输入优化的虚拟键盘，比如，返回键可能被重新标记为“搜索”，也可能还有其他的优化。
> - **`"email"`** ：为邮件地址输入优化的虚拟键盘，通常包含"@"符号和其他优化。表单里面的邮件地址输入应该使用 `<input type="email">` 。
> - **`"url"`** ：为网址输入优化的虚拟键盘，比如，“/” 键会更加明显、历史记录访问等。表单里面的网址输入通常应该使用 `<input type="url">`
>
> 摘自：[MDN - inputmode](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/inputmode)

###### `enterkeyhint`

Hints what action label (or icon) to present for the enter key on virtual keyboards.

> The **`enterkeyhint`** <font color=lightSeaGreen>global attribute</font> is an <font color=red>**enumerated attribute**</font>（👀 说明只能是规定的值，其他值都是不合法的，也不会生效） defining what action label (or icon) to present for the enter key on virtual keyboards.
>
> ##### Description
>
> Form controls (such as `<textarea>` or `input` elements) or elements using `contenteditable` can specify an `inputmode` attribute to control what kind of virtual keyboard will be used. <font color=dodgerBlue>To further improve the user's experience</font>, <font color=LightSeaGreen>the enter key can be customized specifically by providing an `enterkeyhint` attribute indicating how the enter key should be labeled</font> (or which icon should be shown). The enter key usually represents what the user should do next; typical actions are: sending text, inserting a new line, or searching.
>
> If no `enterkeyhint` attribute is provided, the user agent might use contextual information from the `inputmode`, `type`, or [`pattern`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#pattern) attributes to display a suitable enter key label (or icon).
>
> ###### Values
>
> The <font color=red>`enterkeyhint` attribute is an enumerated attribute</font> and <font color=red>**only accepts the following values**</font>:
>
> | Value                     | Description                                                  | Example label (depends on user agent and user language) |
> | :------------------------ | :----------------------------------------------------------- | :------------------------------------------------------ |
> | `enterkeyhint="enter"`    | Typically inserting a new line.                              | ↵                                                       |
> | `enterkeyhint="done"`     | Typically meaning there is nothing more to input and the input method editor (IME) will be closed. | Done                                                    |
> | `enterkeyhint="go"`       | Typically meaning to take the user to the target of the text they typed. | Open                                                    |
> | `enterkeyhint="next"`     | Typically taking the user to the next field that will accept text. | Next                                                    |
> | `enterkeyhint="previous"` | Typically taking the user to the previous field that will accept text. | Previous                                                |
> | `enterkeyhint="search"`   | Typically taking the user to the results of searching for the text they have typed. | Search                                                  |
> | `enterkeyhint="send"`     | Typically delivering the text to its target.                 | Send                                                    |
>
> 摘自：[MDN US - enterkeyhint](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/enterkeyhint)

> 💡  `enterkeyhint` 和 `inputmode` 相当类似，区别如下：
>
> <img src="https://s2.loli.net/2023/05/11/ylCTZh4IHtcoOmr.png" alt="image-20230511162624422.png" style="zoom:45%;" />
>
> 💡 **实践补充**
>
> 在开发时发现一个问题：vant 中的 `van-search` 输入的虚拟键盘，enter 键显示的是 return，而不是 搜索 / “search”；显然不太符合需求，便想通过 `inputmode` 之类的属性实现。
>
> 经过实践发现：`inputmode="search"` 的 enter 显示的是 “前往” / “go”，而 `enterkeyinput="search"` enter 显示的是 “搜索” / “search” ，显然 `enterkeyinput="search"` 更优些。
>
> 另外，发现：同时使用 `inputmode="search"` 和 `enterkeyinput="search"` ，enter 展示的是 “前往” / “go” ；可见：`inputmode` 的优先级更高。

###### `is`

<font color=FF0000>允许您指定标准 HTML 元素</font>（👀 即，自定义组件）<font color=FF0000>应该**像已注册的自定义内置元素一样**<font color=fuchsia>**（便于语义化和 SEO）**</font></font>

只有在当前文档中已成功定义 ( defined ) 指定的自定义元素名称并且扩展了要应用的元素类型时，才能使用此属性。示例如下：

```js
// Create a class for the element
class WordCount extends HTMLParagraphElement {
  constructor() {
    // Always call super first in constructor
    super();
    
    // Constructor contents ommitted for brevity
    ...
  }
}

// Define the new element
customElements.define('word-count', WordCount, { extends: 'p' });
```

```html
<p is="word-count"></p>
```

###### `itemid`

<font color=FF0000>**项** 的唯一全局标识符</font>。

###### `itemprop`

<font color=FF0000>用于向项添加属性</font>。 每个 HTML 元素都可以指定一个 `itemprop` 属性，其中一个 `itemprop` 由一个名称和值对组成。

> 💡 `itemprop` 可以用于 `meta` 标签，参见 [[#\<meta>]]

###### `itemref`

只有不是具有 `itemscope` 属性的元素的后代，它的属性才可以与使用 `itemref` 项目相关联。它提供了元素 ID 列表（而不是 `itemids` ）以及文档中其他位置的其他属性。

###### `itemscope`

`itemscope`（通常）与 `itemtype` 一起使用，以指定包含在关于特定项目代码块中的 HTML 。 `itemscope` 创建 Item 并定义与之关联的 `itemtype` 的范围。 `itemtype` 是描述项及其属性上下文的词汇表（例如 schema.org ）的有效 URL 。

###### `itemtype`

指定将用于在数据结构中定义 `itemprops`（项属性）的词汇表的 URL。`itemscope` 用于设置数据结构中按 itemtype 设置的词汇表的生效范围。

###### `lang`

<font color=FF0000>帮助定义元素的语言：不可编辑元素所在的语言，或者应该由用户编写的可编辑元素的语言</font>。该属性包含一个“语言标记”(由用连字符分隔的“语言子标记”组成)，格式在 Tags for Identifying Languages ( BCP47 ) 中定义。`xml:lang` 优先于它。

###### `part`

🧪 元素的部件名称的空格分隔列表。Part 名称允许 CSS 通过 `::part()` 伪元素选择和设置阴影关联树中的特定元素

###### `popover`

Used to <font color=red>designate an element as a popover element</font> (see [Popover API](https://developer.mozilla.org/en-US/docs/Web/API/Popover_API) ). <font color=red>**Popover elements are hidden via `display: none` until opened via an invoking/control element**</font> (i.e. a `<button>` or `<input type="button">` with a `popovertarget` attribute) or a `HTMLElement.showPopover()` call.

###### `role`

Roles define the semantic meaning of content, allowing screen readers and other tools to present and support interaction with an object in a way that is consistent with user expectations of that type of object. `roles` are added to HTML elements using `role="role_type"` , where `role_type` is the name of a role in the ARIA specification.

###### `slot`

<font color=FF0000>**将 shadow DOM 阴影关联树中的一个沟槽分配给一个元素**</font>：具有 `slot` 属性的元素被分配给由 `<slot>` 元素创建的插槽，其 `name` 属性的值与 `slot` 属性的值匹配。

###### `spellcheck`

🧪 枚举属性，<font color=FF0000>定义**是否可以检查元素是否存在拼写错误**</font>。它可能具有以下值：

- **`true`** ：<font color=FF0000>表示如果可能，应检查元素是否存在拼写错误</font>；
- **`false`** ：表示<font color=FF0000>不应检查元素的拼写错误</font>。

###### `style`

<font color=FF0000>含要应用于元素的CSS样式声明</font>。 请注意，建议在单独的文件中定义样式。 该属性和 `<style>` 元素主要用于快速样式化，例如用于测试目的。

###### `tabindex`

整数属性，<font color=FF0000>指示元素是否可以获取输入焦点（可聚焦），是否应该参与顺序键盘导航</font>，如果是，则表示哪个位置。它可能需要几个值：

- **负值** ：<font color=FF0000>表示该元素应该是可聚焦的</font>，<font color=fuchsia>但**不应通过顺序键盘导航到达**</font>;
- **0** ：<font color=FF0000>表示元素应通过顺序键盘导航可聚焦和可到达</font>，但其相对顺序由平台约定定义;
- **正值** ：意味着<font color=FF0000>元素应该可以通过顺序键盘导航进行聚焦和访问；元素聚焦的顺序是 `tabindex` 的增加值</font>。如果多个元素共享相同的 `tabindex` ，则它们的相对顺序遵循它们在文档中的相对位置。

###### `title`

<font color=FF0000>包含表示与其所属元素相关信息的文本</font>。 这些信息通常可以作为提示呈现给用户，但不是必须的。

###### `translate`

🧪 枚举属性，<font color=FF0000>用于指定**在页面本地化时**是否转换元素的属性值及其 `Text` 节点子节点的值，或者是否保持它们不变</font>。它可以具有以下值：

- <font color=FF0000>**空字符串和`"yes" `**</font>，<font color=FF0000>表示元素将被翻译</font>。
- **`"no"`** ，表示该元素不会被翻译。

摘自：[MDN - 全局属性](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes)



#### HTML5 语义化元素

##### HTML5 语义化标签

| 标签                                     | 描述                                                       |
| ---------------------------------------- | ---------------------------------------------------------- |
| `<article>`                              | 页面独立的内容区域。                                       |
| `<aside>`                                | 页面的侧边栏内容。                                         |
| `<bdi>`                                  | 允许您设置一段文本，使其脱离其父元素的文本方向设置。       |
| `<command>`                              | 命令按钮，比如单选按钮、复选框或按钮                       |
| `<details>`                              | 用于描述文档或文档某个部分的细节                           |
| `<dialog>`                               | 对话框，比如提示框                                         |
| `<summary>`                              | 标签包含 `details` 元素的标题                              |
| `<figure>`                               | 规定独立的流内容（图像、图表、照片、代码等等）。           |
| <font color=FF0000>`<figcaption>`</font> | <font color=FF0000>`<figure>` 元素的标题</font>            |
| `<footer>`                               | section 或 document 的页脚。                               |
| <font color=FF0000>`<header>`</font>     | <font color=FF0000>文档的头部区域</font>                   |
| `<mark>`                                 | 带有记号的文本。                                           |
| `<meter>`                                | 度量衡。仅用于已知最大和最小值的度量。                     |
| <font color=FF0000>`<nav>`</font>        | <font color=FF0000>导航链接的部分。</font>                 |
| `<progress>`                             | 任何类型的任务的进度。                                     |
| `<ruby>`                                 | ruby 注释（中文注音或字符）。                              |
| `<rt>`                                   | 字符（中文注音或字符）的解释或发音。                       |
| `<rp>`                                   | 在 ruby 注释中使用，不支持 ruby 元素的浏览器所显示的内容。 |
| `<section>`                              | 文档中的节（section、区段）。                              |
| `<time>`                                 | 日期或时间。                                               |
| `<wbr>`                                  | 规定在文本中的何处适合添加换行符。                         |

##### 语义化标签的使用

###### `<title></title>` 页面主要内容

`<title>` 标签的特点是简短、描述性、唯一，用于提升搜索引擎排名。

搜索引擎会把 `title` 作为判断页面主要内容的指标，有效的 `title` 应该包含几个与页面内容密切相关的关键字，建议将 `title` 的核心内容写在前 60 个字符。

###### `<header></header>` 页眉

HTML5 规范描述为“一组解释性或导航型性的条目”，通常有网站标志、主导航、全站链接以及搜索框。

###### `<nav></nav>` 导航

页面的导航链接区域，用于定义页面的主要导航部分。

导航通常使用 `<ul>` 无序列表。若是面包屑链接，则使用 `<ol>` 有序列表。

HTML5 规范不推荐对辅助页脚链接使用 `nav`，除非页脚再次显示顶级全局导航、或者是招聘信息等重要链接。

###### `<main></main>` 主要内容

网站页面的主要内容，并且<font color=FF0000>一个页面只能使用一次 `<main>` 标签</font>。

若是 web 应用，则包含其主要功能。

###### `<article></article>` 文章标记

表示的是一个文档、页面、应用或是网站中的一个独立的容器。

HTML5 规范声明 `<article>` 标签适用于自包含的窗口小部件:股票行情，计算器，钟表，天气窗口小部件等。

`<article>` 这个标签可以嵌套使用，但是他们必须是部分与整体的关系。

###### `<section></section>` 区块

一组相似主题的内容，一般会有一个标题。

实现比如文章的章节，标签式对话框中的各种标签页等功能。

###### `<aside></aside>` 侧边栏

表示一部分内容与页面的主体并没有较大的关系，并且可以独立存在。

实现比如升式引用、侧边栏、相关文章的链接、广告、友情链接等功能。

###### `<footer></footer>` 页脚

和 `<header>` 标签对应，可以实现比如附录、索引、版权页、许可协议等功能。

###### `<cite></cite>` 引用

表示它所包含的文本对某个参考文献的引用，比如书籍或者杂志的标题。

按照惯例，引用的文本将以斜体显示。

用 `<cite>` 标签把指向其他文档的引用分离出来，尤其是分离那些传统媒体中的文档，如书籍、杂志、期刊，等等。

###### `<blockquote></blockquote>` 块引用

`<blockquote>` 与 `</blockquote>` 之间的所有文本都会从常规文本中分离出来，经常会在左、右两边进行缩进（增加外边距），而且有时会使用斜体。也就是说，块引用拥有它们自己的空间。

###### `<q></q>` 短的引用

浏览器经常在引用的内容周围添加引号。

根据 HTML 4.01 规范，`q` 元素应当使用分界引号来呈现，就是说，`q` 元素包含的文本必须以引号来开始和结束。

###### `<time></time>` 日期或时间

如果未定义 `datetime` 属性，则必须在元素的内容中规定日期或时间。

###### `<abbr></abbr>` 简称或缩写

通过对缩写进行标记，您能够为浏览器、拼写检查和搜索引擎提供有用的信息。

可以在 `<abbr>` 标签中使用全局的 `title` 属性，这样就能够在鼠标指针移动到 `<abbr>` 元素上时显示出简称/缩写的完整版本。

###### `<dfn></dfn>` 特殊术语或短语的定义

现在流行的浏览器通常用斜体来显示 `<dfn>` 中的文本。

与其他许多基于内容的样式和物理样式标签一样，`<dfn>` 标签尽量少用为妙。

###### `<del></del>` 删除的文本

和 `<ins>` 标签配合使用，来描述文档中的更新和修正。

###### `<ins></ins>` 插入文本

###### `<code></code>` 源代码

用于表示计算机源代码或者其他机器可以阅读的文本内容。

###### `<pre></pre>` 预格式化的文本

被包围在 `pre` 元素中的文本通常会保留空格和换行符。而文本也会呈现为等宽字体。

若使用 `<pre>` 标签来定义计算机源代码，比如 HTML 源代码，则使用符号实体来表示特殊字符，比如 `&lt;` 代表 "<"，`&gt;` 代表 ">"，`&amp;` 代表 "&"。

可以导致段落断开的标签（例如标题、`<p>` 和 `<address>` 标签）绝不能包含在 `<pre>` 所定义的块里。尽管有些浏览器会把段落结束标签解释为简单地换行，但是这种行为在所有浏览器上并不都是一样的。

pre 元素中允许的文本可以包括物理样式和基于内容的样式变化，还有链接、图像和水平分隔线。

##### 补充

<img src="https://i.loli.net/2021/02/23/Y6qnmiPTJdOjz1Q.jpg" alt="img" style="zoom:70%;" />

摘自：[runoob - HTML5 语义元素](https://www.runoob.com/html/html5-semantic-elements.html) / [前端面试题-HTML语义化标签](https://segmentfault.com/a/1190000013901244)



#### `<article>`

HTML `<article>` 元素表示文档、页面、应用或网站中的独立结构，其意在成为可独立分配的或可复用的结构，如在发布中，它可能是论坛帖子、杂志或新闻文章、博客、用户提交的评论、交互式组件，或者其他独立的内容项目。

**使用说明**

- 每个 `<article>` ，通常包括标题（ `<h1>` - `<h6>` 元素）作为 `<article>` 元素的子元素。
- 当 `<article>` 元素嵌套使用时，则该元素代表与外层元素有关的文章。例如，代表博客评论的 `<article>` 元素可嵌套在代表博客文章的 `<article>` 元素中。

- `<article>` 元素的作者信息可通过 `<address>` 元素提供，但是不适用于嵌套的 `<article>` 元素。
- `<article>` 元素的发布日期和时间可通过 `<time>` 元素的 `pubdate` 属性表示。
- 可以使用 `<time>` 元素的 `datetime` 属性来描述 `<article>` 元素的发布日期和时间。请注意 `<time>` 的 `pubdate` 属性不再是 W3C HTML5 标准。

摘自：[MDN - `<article>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/article)



#### `<figure>`

HTML `<figure>` 元素代表一段独立的内容，<font color=FF0000>经常与说明（caption）`<figcaption>` 配合使用，并且作为一个独立的引用单元</font>。当它属于主内容流 ( main flow ) 时，它的位置独立于主体。这个标签经常是在主文中引用的图片，插图，表格，代码段等等，当这部分转移到附录中或者其他页面时不会影响到主体。

**效果：**

<img src="https://i.loli.net/2020/11/17/Y2VJ586S1XEKQkN.png" alt="DeAnsA.png" style="zoom:40%;" />

##### **使用说明**

- 通常，`<figure>` 是图像，插图，图表，代码片段等，在文档的主流程中引用，但可以移动到文档的另一部分或附录而不影响主流程。
- 作为 sectioning root，`<figure>` 元素的内容轮廓将从文档的主要轮廓中排除。
- 通过在其中插入 `<figcaption>`（作为第一个或最后一个子元素），可以将标题与 `<figure>` 元素相关联。图中找到的第一个 `<figcaption>` 元素显示为图的标题。

摘自：[MDN - `<figure>`：可附标题内容元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/figure)



#### `<section>`

HTML `<section>` 元素<font color=FF0000>表示一个包含在 HTML 文档中的独立部分</font>，它没有更具体的语义元素来表示，一般来说会有包含一个标题。

##### **使用说明**

- 一般通过是否包含一个标题 ( `<h1>`-`<h6>` element) 作为子节点 来 辨识每一个 `<section>` 。
- 如果 `<section>` 元素的内容可以单独在多个媒体上发表，应该使用 `<article>` 而不是 `<section>`。
- 不要把 `<section>` 元素作为一个普通的容器来使用，这是本应该是 `<div>` 的用法（特别是当片段 ( the sectioning ) 仅仅是为了美化样式的时候）。 一般来说，一个 `<section>` 应该出现在文档大纲中。

摘自：[MDN - `<section>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/section)



#### `<progress>` 进度指示元素

HTML 中的 `<progress>` 元素<font color=FF0000>用来显示一项任务的完成进度</font>。<font color=LightSeaGreen>虽然规范中没有规定该元素具体如何显示，浏览器开发商可以自己决定，但通常情况下，该元素都显示为一个进度条形式</font>。如下示例：

<img src="https://i.loli.net/2021/02/23/6a4NvX5LlUoCQId.png" alt="https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/progress" style="zoom:40%;" />

##### 属性

和其他的 HTML 元素一样，该元素具有全局属性。

###### `max`

该属性描述了这个 `progress` 元素所表示的任务一共需要完成多少工作.

###### `value`

该属性用来指定该进度条已完成的工作量。如果没有 `value` 属性，则该进度条的进度为“不确定”，也就是说,进度条不会显示任何进度，你无法估计当前的工作会在何时完成（比如在下载一个未知大小的文件时，下载对话框中的进度条就是这样的）。

摘自：[MDN - `<progress>`：进度指示元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/progress)



#### `<address>`

HTML `<address>` 元素 表示其中的 HTML <font color=FF0000>提供了某个人或某个组织（等等）的联系信息</font>。

##### 用法说明

- <font color=LightSeaGreen>当表示一个和联系信息无关的任意的地址时，请改用 `<p>` 元素而不是 `<address>` 元素</font>。
- <font color=LightSeaGreen>这个元素不能包含除联系信息之外的任何信息</font>，比如出版日期（这应当被包含在 `<time>` 元素之中）。
- 通常，`<address>` 元素可以放在 `<footer>` 元素之中（如果存在的话）。

摘自：[MDN - `<address>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/address)



#### `<input>`

如果想要输入的值位于中间 / 右边，只需要设置 `text-align: center / right` 即可，同时，`placeholder` 也会跟着居中 / 居右

##### input 输入事件

onfocus ( focus )  -> 键盘输入 -> onkeydown ( keydown )  -> onkeypress ( keypress )  -> onkeyup ( keyup )  -> oninput ( input )  -> 失去焦点 -> onchange ( change )  -> onblur ( blur )

摘自：[input输入框事件](https://www.jianshu.com/p/4517117abd8e)

##### 修改输入框光标的颜色

修改输入框光标的颜色：可以使用 `caret-color` 属性

##### `<input type="hidden">`

<font color=FF0000>`hidden` 类型的 `<input>` 元素允许 Web 开发者存放一些用户不可见、不可改的数据，在用户提交表单时，这些数据会一并发送出</font>。比如，正被请求或编辑的内容的 ID，或是一个唯一的安全令牌。这些隐藏的 `<input>` 元素在渲染完成的页面中完全不可见，而且没有方法可以使它重新变为可见。

###### 使用场景

隐藏的 `<input>` 可以用于任何有需要提交给服务器的、用户无法查看或编辑的数据的地方。让我们看一些说明其用法的例子吧。

- 跟踪被编辑的内容：隐藏输入的最常见用途之一是当被编辑的表单提交时，保持跟踪数据库数据的更新。
  
- 改善网站安全性：隐藏输入表单还用于存储和提交安全令牌或机密信息，以提高网站的安全性。基本思路是，如果用户填写敏感表格，例如在其银行网站上将某笔款项转入另一个帐户的表格，他们将被提供的密钥和证明他们就是他们所说的真实身份，并且他们使用正确的表单来提交转移请求。

摘自：[MDN - `<input type="hidden">`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Input/hidden)

###### 补充

隐藏域在页面中对于用户是不可见的，在表单中插入隐藏域的目的在于收集或发送信息，以利于被处理表单的程序所使用 （隐藏只是在网页页面上面不显示输入框，但是虽然隐藏了，还是具有 form 传值功能。一般用来传值，而不必让用户看到）



##### `<input>` 的 `type` 属性可选值

| Type                                   | 描述                                                         | Spec  |
| :------------------------------------- | :----------------------------------------------------------- | :---- |
| button                                 | 没有默认行为的按钮，上面显示 `value` 属性的值，默认为空。    |       |
| checkbox                               | 复选框，可设为选中或未选中。                                 |       |
| color                                  | 用于指定颜色的控件；在支持的浏览器中，激活时会打开取色器。   | HTML5 |
| date                                   | 输入日期的控件（年、月、日，不包括时间）。在支持的浏览器激活时打开日期选择器或年月日的数字滚轮。 | HTML5 |
| datetime-local                         | 输入日期和时间的控件，不包括时区。在支持的浏览器激活时打开日期选择器或年月日的数字滚轮。 | HTML5 |
| email                                  | 编辑邮箱地址的区域。类似 text 输入，但在支持的浏览器和带有动态键盘的设备上会有确认参数和相应的键盘。 |       |
| file                                   | 让用户选择文件的控件。使用 `accept` 属性规定控件能选择的文件类型。 |       |
| <font color=FF0000>**hidden**</font>   | 不显示的控件，其值仍会提交到服务器。举个例子，右边就是一个隐形的控件。 |       |
| image                                  | 带图像的 submit 按钮。显示的图像由 src 属性规定。如果 src 缺失，alt 属性就会显示。 |       |
| month                                  | 输入年和月的控件，没有时区。                                 | HTML5 |
| <font color=FF0000>**number**</font>   | 用于输入数字的控件。如果支持的话，会显示滚动按钮并提供缺省验证（即只能输入数字）。拥有动态键盘的设备上会显示数字键盘。 |       |
| password                               | 单行的文本区域，<font color=LightSeaGreen>其值会被遮盖</font>。如果站点不安全，会警告用户。 |       |
| radio                                  | 单选按钮，允许在多个拥有相同 name 值的选项中选中其中一个。   |       |
| range                                  | 此控件用于输入不需要精确的数字。控件是一个范围组件，默认值为正中间的值。同时使用 htmlattrdefmin  和 htmlattrdefmax 来规定值的范围。 | HTML5 |
| reset                                  | 此按钮将表单的所有内容重置为默认值。不推荐。                 |       |
| search                                 | 用于搜索字符串的单行文字区域。输入文本中的换行会被自动去除。在支持的浏览器中可能有一个删除按钮，用于清除整个区域。拥有动态键盘的设备上的回车图标会变成搜索图标。 | HTML5 |
| submit                                 | 用于提交表单的按钮。                                         |       |
| tel                                    | 用于输入电话号码的控件。拥有动态键盘的设备上会显示电话数字键盘。 | HTML5 |
| text                                   | 默认值。单行的文本区域，输入中的换行会被自动去除。           |       |
| time                                   | 用于输入时间的控件，不包括时区。                             | HTML5 |
| url                                    | 用于输入 URL 的控件。类似 text 输入，但有验证参数，在支持动态键盘的设备上有相应的键盘。 | HTML5 |
| week                                   | 用于输入以年和周数组成的日期，不带时区。                     |       |
| <font color=FF0000>**废弃的值**</font> |                                                              |       |
| datetime                               | 用于输入基于 UTC 时区的日期和时间（时、分、秒及秒的小数部分）。 |       |

##### `<input>` 元素的属性包括 全局 HTML 属性 和 以下属性

| 属性                                    | 相关的 type                      | 描述                                                         |
| :-------------------------------------- | :------------------------------- | :----------------------------------------------------------- |
| accept                                  | file                             | 用于规定文件上传控件中期望的文件类型                         |
| alt                                     | image                            | image `type` 的 `alt` 属性，是可访问性的要求。               |
| autocomplete                            | 所有                             | 用于表单的自动填充功能                                       |
| autofocus                               | 所有                             | 页面加载时自动聚焦到此表单控件                               |
| capture                                 | file                             | 文件上传控件中媒体拍摄的方式                                 |
| checked                                 | radio, checkbox                  | 用于控制控件是否被选中                                       |
| dirname                                 | text, search                     | 表单区域的一个名字，用于在提交表单时发送元素的方向性         |
| disabled                                | 所有                             | 表单控件是否被禁用                                           |
| form                                    | 所有                             | 将控件和一个 form 元素联系在一起                             |
| formaction                              | image, submit                    | 用于提交表单的 URL                                           |
| formenctype                             | image, submit                    | 表单数据集的编码方式，用于表单提交                           |
| formmethod                              | image, submit                    | 用于表单提交的 HTTP 方法                                     |
| formnovalidate                          | image, submit                    | 提交表单时绕过对表单控件的验证                               |
| formtarget                              | image, submit                    | 表单提交的浏览上下文                                         |
| height                                  | image                            | 和 `<img>` 的 height 属性相同；垂直方向                      |
| list                                    | 绝大部分                         | 自动填充选项的 `<datalist>` 的id值                           |
| max                                     | 数字 type                        | 最大值                                                       |
| maxlength                               | password, search, tel, text, url | value 的最大长度（最多字符数目）                             |
| min                                     | 数字 type                        | 最小值                                                       |
| minlength                               | password, search, tel, text, url | value 的最小长度（最少字符数目）                             |
| multiple                                | email, file                      | 布尔值。 是否允许多个值                                      |
| name                                    | 所有                             | input表单控件的名字。以名字/值对的形式随表单一起提交 <br />关于这个字段可以看下 [React doc - Updating Objects in State # Using a single event handler for multiple fields](https://react.dev/learn/updating-objects-in-state#using-a-single-event-handler-for-multiple-fields) 中 `name` 的使用 |
| pattern                                 | password, text, tel              | 匹配有效 value 的模式 ( pattern )                            |
| placeholder                             | password, search, tel, text, url | 当表单控件为空时，控件中显示的内容                           |
| readonly                                | 绝大部分                         | 布尔值。存在时表示控件的值不可编辑                           |
| <font color=FF0000> **required**</font> | 绝大部分                         | 布尔值。<font color=FF0000> **表示此值为必填项或者提交表单前必须先检查该值**</font> |
| size                                    | email, password, tel, text       | 控件的大小                                                   |
| src                                     | image                            | 和 `<img>` 的 src 属性一样；图像资源的地址                   |
| step                                    | 数字type                         | 有效的递增值                                                 |
| type                                    | 所有                             | input 表单控件的 type                                        |
| value                                   | 所有                             | 表单控件的值。以名字/值对的形式随表单一起提交                |
| width                                   | image                            | 与 `<img>` 的 width 属性一样                                 |

> 💡  还有非标准属性 `webkitdirectory`
>
> > ###### `webkitdirectory`
> >
> > 如果出现布尔属性 `webkitdirectory`，表示在文件选择器界面中用户只能选择目录。更多细节和示例见 [`HTMLInputElement.webkitdirectory`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLInputElement/webkitdirectory)。
> >
> > 尽管 `webkitdirectory` 最初仅为基于 Webkit 的浏览器实现，它还在 Microsoft Edge 和 Firefox 50 及其后版本中可用。然而，尽管它有相对广泛的支持，它仍然是非标准的。除非别无选择，否则不要使用它。
>
> 摘自：[MDN - `<input type="file">`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input/file)
>
> 更详细的见下面：

##### HTMLInputElement：webkitdirectory 属性

**`HTMLInputElement.webkitdirectory`** 是一个反应了 HTML 属性 [`webkitdirectory`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input/file#webkitdirectory) 的属性，<font color=red>其指示 `<input>` 元素应该让用户选择文件目录而非文件</font>。在选择文件目录后，该目录及其整个内容层次结构将包含在所选项目集内。可以使用 [`webkitEntries` (en-US)](https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement/webkitEntries) 属性获取选定的文件系统条目。

###### 示例

这个示例提供了一个目录选择器，它允许用户选择一个或多个目录。当触发 [`change`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/change_event) 事件时，将生成并显示所选目录层次结构中包含的所有文件的列表。

```html
<input type="file" id="filepicker" name="fileList" webkitdirectory multiple />
<ul id="listing"></ul>
```

```js
document.getElementById("filepicker").addEventListener(
  "change",
  (event) => {
    let output = document.getElementById("listing");
    for (const file of event.target.files) {
      let item = document.createElement("li");
      item.textContent = file.webkitRelativePath;
      output.appendChild(item);
    }
  },
  false,
);
```

摘自：[MDN - HTMLInputElement：webkitdirectory 属性](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLInputElement/webkitdirectory)

##### `<textarea>` 完全去除边框

```css
textarea{
  border: none;
  resize: none;
  cursor: pointer;
}
```

摘自：[textarea 完全去除边框](https://blog.csdn.net/qq_32849999/article/details/102978744)



#### 锚点

锚点是网页制作中超级链接的一种，又叫命名锚记。命名锚记像一个迅速定位器一样是一种页面内的超级链接，运用相当普遍。英文名为：anchor

##### 声明示例如下

```html
<a name="top">这里是TOP部分</a>
<a name="content">这里是CONTENT部分</a>
<a name="foot">这里是FOOT部分</a>
```

<font color=FF0000>可以使用 id 属性来替代 name 属性，命名锚同样有效</font>。

##### 锚点的访问有两种方法

- 一种是利用超链接标签 `<a></a>` 制作锚点链接，主要用于页面内的锚点访问

  ```html
  <a href="#top">点击我链接到TOP</a>
  <a href="#content">点击我链接到CONTENT</a>
  <a href="#foot">点击我链接到FOOT</a>
  ```

- 另一种方式是<font color=FF0000>直接在页面地址后面加锚点标记，主要用于不同页面之间的锚点访问</font>

  <font color=LightSeaGreen>假如本页面的地址是 `http://文件路径/index.html`，要访问 foot 锚点只要访问如下链接即可：`http://文件路径/index.html#foot`</font>

摘自：[html中的锚点介绍和使用](https://blog.csdn.net/yangkai_hudong/article/details/14163483)



#### `<mark>`

##### 概要

HTML标记文本元素 ( `<mark>` ) 表示为引用或符号目的而标记或突出显示的文本，这是由于标记的段落在封闭上下文中的相关性或重要性造成的。

这个 HTML mark 标签代表突出显示的文字,例如可以为了标记特定上下文中的文本而使用这个标签。举个例子，它可以用来显示搜索引擎搜索后关键词。

| 属性                 | 内容                                                  |
| :------------------- | ----------------------------------------------------- |
| Content categories   | Flow content, phrasing content, palpable content.     |
| Permitted content    | Phrasing content.                                     |
| Tag omission         | None, both the starting and ending tag are mandatory. |
| Permitted parents    | Any element that accepts phrasing content.            |
| Implicit ARIA role   | No corresponding role                                 |
| Permitted ARIA roles | Any                                                   |
| DOM interface        | HTMLElement                                           |

##### 属性

这个元素只包含了 全局属性

##### 使用说明

<font color=dodgerBlue>`<mark>` 元素的典型使用场景包括：</font>

- 当用在引用 ( `<q>` 、`<blockquote>` ) 中时，通常用来显示有特殊意义的文本，但不在原材料中标记出来；或者是用来显示特殊审查的材料，即使原作者不认为它特别重要。
- 另外，`<mark>` 元素还用来显示与用户当前活动相关的一部分文档内容。例如，它可能被用于显示匹配搜索结果中的单词。
- 不要为了语法高亮而用 `<mark>` 元素；你应该用 `<strong>` 元素结合适当的 CSS 来实现这个目的（语法高亮）。

> 💡 不要把 `<mark>` 元素和 `<strong>` 元素搞混淆；`<strong>` 元素用来表示文本在上下文的重要性的， 而 `<mark>` 元素是用来表示上下文的关联性的

摘自：[MDN - `<mark>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/mark)

> 💡 `<mark>` 标签 在 markdown 中是有效的，同时，在 html 中也是有效的。



#### `tabindex`

`tabindex` 全局属性 指示其元素是否可以聚焦，以及它是否/在何处参与顺序键盘导航（通常使用Tab键，因此得名）。<font color=FF0000>（可以在下面“ 摘自的url ”按下<font size=4>⇥</font>看下效果）</font>

<font color=dodgerBlue>**它接受一个整数作为值，具有不同的结果，具体取决于整数的值：**</font>

- **`tabindex=负值`**（通常是 `tabindex="-1"` ），表示<font color=FF0000>元素是可聚焦的</font>，但是<font color=red>**不能** 通过键盘导航来访问到该元素</font>，用 JS 做页面小组件内部键盘导航的时候非常有用。
- **`tabindex="0"`** ，表示元素<font color=FF0000>是可聚焦的</font>，并且<font color=red>**可以** 通过键盘导航来聚焦到该元素</font>，它的相对顺序是当前处于的 DOM 结构来决定的。
- **`tabindex=正值`** ，表示<font color=FF0000>元素是可聚焦的</font>，并且 <font color=red>**可以** 通过键盘导航来访问到该元素</font>；<font color=fuchsia>它的相对顺序按照 tabindex 的数值递增而滞后获焦</font>。<font color=FF0000>如果多个元素拥有相同的 tabindex，它们的相对顺序按照他们在当前 DOM中的先后顺序决定</font>。

根据键盘序列导航的顺序，值为 0 、非法值、或者没有 `tabindex` 值的元素应该放置在 tabindex 值为正值的元素后面。

摘自：[MDN - tabindex](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/tabindex)



#### `<link>`

HTML 外部资源链接元素 ( `<link>` ) <font color=red>**规定了当前文档与外部资源的关系**</font>。<font color=lightSeaGreen>该元素 **最常用于链接样式表**，此外 **也可以被用来创建站点图标**</font>（ 比如 PC 端的 “favicon” 图标和移动设备上用以显示在主屏幕的图标 ）

要链接一个外部的样式表，你需要像这样在你的 `<head>` 中包含一个 `<link>` 元素：

```html
<link href="main.css" rel="stylesheet">
```

<font color=FF0000>在这个简单的例子中，使用了 **`href` 属性设置外部资源的路径**，并设置 `rel` 属性的值为 “stylesheet”（样式表）</font>。<font color=lightSeaGreen>**`rel` 表示 “关系 ( relationship ) ”**</font>，它可能是 `<link>` 元素其中一个关键的特性——属性值表示 `<link>` 项的链接方式与包含它的文档之间的关系。可以在 [链接类型 ( rel )](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Link_types) 中看到很多不同类型的关系。

这里有一些你经常遇到的其它类型。例如，<font color=dodgerBlue>这里是一个网站图标的链接</font>：

```html
<link rel="icon" href="favicon.ico">
```

<font color=dodgerBlue>还有一些其它的与图标相关的 `rel` 值</font>，主要用于表示不同移动平台上特殊的图标类型，例如：

```html
<link
  rel="apple-touch-icon-precomposed"
  sizes="114x114"
  href="apple-icon-114.png"
  type="image/png"
/>
```

<font color=LightSeaGreen>`sizes` 属性表示图标大小，`type` 属性包含了链接资源的 MIME 类型</font>。<font color=lightSeaGreen>**这些属性为浏览器选择最合适的图标提供了有用的提示**</font>。

你<font color=dodgerBlue>**也可以提供一个媒体类型，或者在 `media` 属性内部进行查询**</font>；<font color=lightSeaGreen>这种资源将只在满足媒体条件的情况下才被加载进来</font>。例如：

```html
<link href="print.css" rel="stylesheet" media="print">
<link href="mobile.css" rel="stylesheet" media="screen and (max-width: 600px)">
```

`<link>` 也加入了一些新的有意思的性能和安全特性。举例如下：

```html
<link
  rel="preload"
  href="myFont.woff2"
  as="font"
  type="font/woff2"
  crossorigin="anonymous"
>
```

<font color=lightSeaGreen>**将 `rel` 设定为 `preload` ，表示浏览器应该预加载该资源**</font>。<font color=FF0000>`as` 属性表示获取特定的内容类</font>。`crossorigin` 属性表示该资源是否应该使用一个 CORS 请求来获取。

> 💡 crossorigin 相关内容可参考 [[#CORS 设置属性]]

###### `<link>` 其他用法的注解

- `<link>` 元素可以出现在 `<head>` 元素或者 `<body>` 元素中，（能否出现在 `<body>` 中）具体取决于它是否有一个 **body-ok** 的[链接类型](https://html.spec.whatwg.org/multipage/links.html#body-ok)。例如，`stylesheet` 链接类型是 body-ok 的，因此 `<link rel="stylesheet">` 允许出现在 body 中。然而，<font color=dodgerBlue>这不是一种好的可遵循的实践方式</font>；<font color=red>更合理的方式是，将你的 `<link>` 元素从你的 body 内容中分离出来，将其放在 `<head>` 中</font>。

  > 👀 上述可以理解为：`<link>` 一定可以放在 `<head>` 中，部分可以放在 `<body>` 中；不过更推荐放在 `<head>` 中

- 当使用 `<link>` 为网站创建一个 favicon 时，你的网站使用内容安全策略 ( Content Security Policy, CSP ) 来增强它的安全性，这种策略适用于 favicon。如果你遇到 favicon 未加载的问题，验证 `Content-Security-Policy` 头的 `img-src` 没有在阻止对它的访问。

- HTML 和 XHTML 规范为 `<link>` 元素定义了一些事件处理器 ( event handler ) ，但是对于它们的使用方法不明确

- 在 XHTML 1.0 下，例如`<link>`的空元素需要一个尾斜杠：`<link />`。

- WebTV 支持 `rel` 使用 `next` 值，用于在一个 document series 中预加载下一页。

##### 属性

这个元素可以使用「全局属性」

###### `as`

<font color=FF0000>该属性 **仅在 `<link>` 元素设置了 `rel="preload"` 或者 `rel="prefetch"` 时才能使用**</font>。它规定了 `<link>` 元素加载的内容的类型，<font color=LightSeaGreen>对于内容的优先级、请求匹配、正确的 **内容安全策略 ( CSP )** 的选择以及正确的 `Accept` 请求头的设置，**这个属性是必需的**</font>。

**可选值有：**audio、document、embed、fetch、font、image、object、script、style、track、video、worker 

> 👀 这里原本是一个表格，这里略；详见原文
>
> 💡 另外，可以参考下面的 [[#preload 补充#What types of content can be preloaded?]]

###### `crossorigin`

此 <font color=FF0000>**枚举属性** 指定在加载相关资源时是否必须使用 CORS</font> 。启用 CORS 的图片 可以在 `<canvas>` 元素中重复使用，并避免其被污染

> 💡 `crossorigin` 相关内容可参考 [[#CORS 设置属性]]。
>
> 这里原文说了可选值 `"anonymous"` 和 `"use-credentials"`。为避免重复，这里略；详见原文

当不设置此属性时，资源将会不使用 CORS 加载（即不发送 `Origin` HTTP 头），这将阻止其在 `<canvas>` 元素中进行使用。若设置了非法的值，则视为使用 `anonymous`

###### `disabled`

<font color=red>仅对于 `rel="stylesheet"`</font> ，`disabled` 的 Boolean 属性指示是否应加载所描述的样式表并将其应用于文档。

<font color=dodgerBlue>如果在加载 HTML 时在 HTML 中指定了 Disabled</font>，则<font color=red>在页面加载期间不会加载样式表</font>。相反，<font color=dodgerBlue>如果禁用属性更改为 false 或删除</font>时，<font color=red>样式表将按需加载</font>。但是，一旦加载样式表，对 Disabled 属性的值所做的更改将不再与`StyleSheet.disabled` 属性的值有任何关系。相反，更改此属性的值只是启用和禁用应用于文档的样式表表单。这与 StyleSheet 的 disable 属性不同；将其更改为 true 会将样式表从文档的 `document.styleSheets` 列表中删除，并且在切换回 false 时不会自动重新加载样式表。

###### `href`

此属性<font color=FF0000>指定被链接资源的 URL</font>。 URL 可以是绝对的，也可以是相对的。

###### `hreflang`

此属性<font color=lightSeaGreen>指明了被链接资源的语言</font>。其意义仅供参考。

###### `importance`

🧪<font color=LightSeaGreen>指示资源的相对重要性</font>。优先级提示使用以下值委托：

- **`auto`** : 表示 **没有偏好**。浏览器可以使用其自己的启发式方法来确定资源的优先级。
- **`high`** : 向浏览器指示资源具有高优先级。
- **`low`** : 向浏览器指示资源的优先级较低。

> 💡 只有存在 **`rel="preload"`** 或 **`rel="prefetch"`** 时，`importance` 属性才能用于 `<link>` 元素。

###### `integrity`

🧪 包含行内元数据，它是一个你用浏览器获取的资源文件的哈希值，以 base64 编码的方式的加密，这样<font color=FF0000>用户能用它来验证一个获取到的资源，在传送时未被非法篡改</font>

###### `media`

这个属性 <font color=FF0000>**规定了外部资源适用的媒体类型**</font>。它的值必须是"媒体查询"。这个属性使得用户代理能选择最适合设备运行的媒体类型

###### `referrerpolicy`

🧪 一个字符串，<font color=FF0000>指示在获取资源时使用哪个引荐来源网址</font>

- `'no-referrer'` 表示 `Referer` 标头将不会发送。
- `'no-referrer-when-downgrade'` 的原始位置时不会发送任何 `Referer` 标头。如果未指定其他政策，这是用户代理的默认行为。
- `'origin'` 意味着引荐来源网址将是页面的来源，大致是方案，主机和端口。
- `'origin-when-cross-origin'` 这意味着导航到其他来源将仅限于方案，主机和端口，而在同一来源上导航将包括引荐来源网址的路径。
- `'unsafe-url'` 意味着引荐来源网址将包含来源和路径（但不包括片段，密码或用户名）。这种情况是不安全的，因为它可能会将来源和路径从受 TLS 保护的资源泄漏到不安全的来源。

###### `rel`

<font color=FF0000>**此属性命名链接文档与当前文档的关系**</font>。 该属性必须是链接类型值的用空格分隔的列表。

###### `sizes`

这个属性<font color=FF0000>定义了包含相应资源的可视化媒体中的 icons 的大小</font>。它只有在 `rel` 包含 icon 的 link 类型值。它可能有如下的规则：

- `any` 表示图标可以按矢量格式缩放到任意大小，例如 `image/svg+xml`。
- 一个由空白符分隔的尺寸列表。每一个都以`<width in pixels>x<height in pixels>`或 `<width in pixels>X<height in pixels>给出。`尺寸列表中的每一个尺寸都必须包含在资源里。

> 💡 这里没有说清楚，摘抄一下 [HTML中link标签的那些属性](https://juejin.cn/post/7229126088227487800) 中的内容
>
> > `sizes`：当使用`link`标签链接到多个尺寸的图标时，可以使用`sizes`属性指定图标的大小。这对于根据设备显示不同大小图标的情况很有用。示例：
> >
> > ```html
> > <link rel="icon" href="icon-48x48.png" sizes="48x48">
> > <link rel="icon" href="icon-96x96.png" sizes="96x96">
> > ```

###### `title`

属性在 `<link>` 元素上有特殊的语义。当用于 `<link rel="stylesheet">` 时，它定义了一个首选样式表或备用样式表。

###### `type`

这个属性被<font color=FF0000>**用于定义链接的内容的 MIME 类型**</font>。这个<font color=LightSeaGreen>属性的值应该是像 `text/html`，`text/css` 等 **MIME 类型**</font>。这个属性常用的用法是定义链接的样式表，最常用的值是表明了 CSS 的 `text/css`

##### 非标准属性 ⚠️

- **`methods`** ：此属性的值提供有关可能在对象上执行的功能的信息
- **`prefetch`** ：此属性<font color=FF0000>标识下一个导航可能需要的资源，用户代理应检索该资源</font>。这允许用户代理在将来请求资源时更快地做出响应
- **`target`** ：定义具有已定义链接关系或将显示任何链接资源的呈现的框架或窗口名称。

##### 已淘汰的属性 🗑

- **`charset`** ：此属性定义链接资源的字符编码。
- **`rev`** ：此属性的值显示了 `href` 属性所定义的当前文档与链接文档的关系

摘自：[MDN - `<link>`：外部资源链接元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/link#attr-as)



#### HTML 属性 `rel`

**`rel`** 属性<font color=red>**定义了所链接的资源与当前文档的关系**</font>，<font color=fuchsia>在 `<a>`、`<area>` 和 `<link>` 元素上有效</font>。支持的值取决于拥有该属性的元素。

关系的类型是由 `rel` 属性的值给出的，<font color=dodgerBlue>如果存在的话</font>，<font color=red>它的值必须是一组无序的、唯一的、用空格隔开的关键字</font>。与不表达语义的 `class` 名称不同，<font color=lightSeaGreen>**`rel` 属性必须使用对机器和人类都有语义的标记**</font>。目前关于 `rel` 属性的可能值的注册表是 [IANA 链接关系注册表](https://www.iana.org/assignments/link-relations/link-relations.xhtml)、[HTML 现行标准](https://html.spec.whatwg.org/multipage/links.html#linkTypes) 和 microformats wiki 中可自由编辑的 [existing-rel-values 页面](https://microformats.org/wiki/existing-rel-values)（根据现行标准的[建议](https://html.spec.whatwg.org/multipage/links.html#other-link-types)）。<font color=dodgerBlue>如果使用一个不存在于上述三个来源之一的 `rel` 属性</font>，一些 HTML 验证器（如 [W3C Markup Validation Service](https://validator.w3.org/)）会产生一个警告。

下表列出了一些最重要的现有关键词。在一个以空格分隔的值内的每个关键词在该值内都应该是唯一的。

| `rel` 值        | 描述                                                         | `<link>` | `a` 和 `<area>` | `<form>` |
| :-------------- | :----------------------------------------------------------- | :------- | :-------------- | :------- |
| `alternate`     | <font color=red>当前文档的替代描述</font>                    | 链接     | 链接            | 不允许   |
| `author`        | 当前文档或文章的作者。                                       | 链接     | 链接            | 不允许   |
| `bookmark`      | <font color=lightSeaGreen>到最近祖先章节的永久链接</font>    | 不允许   | 链接            | 不允许   |
| `canonical`     | <font color=red>当前文档的首要 URL</font>                    | 链接     | 不允许          | 不允许   |
| `dns-prefetch`  | 告知浏览器为目标资源的来源预先执行 DNS 解析。                | 外部资源 | 不允许          | 不允许   |
| `external`      | <font color=red>引用的文档与当前的文档不属于同一个站点</font> | 不允许   | 注解            | 注解     |
| `help`          | 链接到上下文相关的帮助。                                     | 链接     | 链接            | 链接     |
| `icon`          | 代表当前文档的图标。                                         | 外部资源 | 不允许          | 不允许   |
| `license`       | 表示当前文档的主要内容由被引用文件描述的版权许可所涵盖。     | 链接     | 链接            | 链接     |
| `manifest`      | Web 应用清单                                                 | 链接     | 不允许          | 不允许   |
| `me`            | <font color=lightSeaGreen>表示当前文档代表拥有链接内容的人</font> | 链接     | 链接            | 不允许   |
| `modulepreload` | <font color=lightSeaGreen>**告知浏览器预先获取该脚本，并将其存储在文档的模块映射中，以便稍后评估**</font>。也可以一同获取该模块的依赖关系。 | 外部资源 | 不允许          | 不允许   |
| `next`          | <font color=red>表示当前文档是一个系列的一部分，被引用的文档是该系列中的下一个文档</font>。 👀 后面还有 `prev` | 链接     | 链接            | 链接     |
| `nofollow`      | 表示当前文档的原作者或出版商不认可被引用的文件               | 不允许   | 注解            | 注解     |
| `noopener`      | <font color=red>创建一个顶级浏览上下文</font>。如果该超链接一开始就会创建其中之一，则该浏览上下文不是一个辅助浏览上下文（即有一个适当的 `target` 属性值）。 | 不允许   | 不允许          | 注解     |
| `noreferrer`    | <font color=red>不会包含 `Referer` 标头</font>。和 `noopener` 效果类似。 | 不允许   | 注解            | 注解     |
| `opener`        | 如果超链接会创建一个非辅助浏览上下文的顶级浏览上下文（即以“`_blank`”作为 `target` 属性值），则创建一个辅助浏览上下文。 | 不允许   | 注解            | 注解     |
| `pingback`      | 给出处理当前文档 pingback 的 pingback 服务器的地址。         | 外部资源 | 不允许          | 不允许   |
| `preconnect`    | <font color=red>指定用户代理应 **预先连接** 到目标资源的来源</font>。 | 外部资源 | 不允许          | 不允许   |
| `prefetch`      | 指定用户代理应预先获取并缓存目标资源，因为后续的导航可能需要它。 | 外部资源 | 不允许          | 不允许   |
| `preload`       | 指定用户代理必须根据 [`as`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/link#attr-as) 属性给出的潜在目的地（以及与相应目的地相关的优先级），为当前导航预先获取和缓存目标资源。 | 外部资源 | 不允许          | 不允许   |
| `prerender`     | 指定用户代理应预先获取目标资源，并以有助于在未来提供更快的响应的方式处理它。 | 外部资源 | 不允许          | 不允许   |
| `prev`          | 表示当前文档是系列的一部分，被引用的文档是该系列中的上一个文档。👀 前面还有 `next` | 链接     | 链接            | 链接     |
| `search`        | 给出一个资源的链接，可以用来搜索当前文件及其相关页面。       | 链接     | 链接            | 链接     |
| `stylesheet`    | 导入样式表。                                                 | 外部资源 | 不允许          | 不允许   |
| `tag`           | 给出一个适用于当前文档的标签（由给定地址识别）。             | 不允许   | 链接            | 链接     |

`rel` 属性与 `<link>`、`<a>`、`<area>` 和 `<form>` 元素有关，但有些值只与这些元素的子集有关。像所有的 HTML 关键字属性值一样，这些值是不区分大小写的。

`rel` 属性没有默认值。如果该属性被省略，或者该属性中没有一个值被支持，那么除了两者之间有一个超链接之外，文档与目标资源没有特别的关系。在这种情况下，在 `<link>` 和 `<form>` 元素上，如果 `rel` 属性不存在，没有关键词，或者如果不是上述一个或多个空格分隔的关键词，那么该元素就不会创建任何链接。`<a>` 和 `<area>` 仍将创建链接，但没有定义关系。

##### 值

###### `alternate`

表示当前文档的另一种方式。对 `<link>`、`<a>` 和 `<area>` 有效，<font color=red>其含义取决于其他属性的值</font>。

- 在 `<link>` 上使用 `stylesheet` 关键字，会创建一个替代样式表。

  ```html
  <!-- 一个永久样式表 -->
  <link rel="stylesheet" href="default.css" />
  <!-- 替代样式表 -->
  <link
    rel="alternate stylesheet"
    href="highcontrast.css"
    title="High contrast"
  />
  ```

- <font color=dodgerBlue>`hreflang` 属性与文档所使用语言不同时</font>，<font color=lightSeaGreen>表示该页面的一个翻译</font>。

  > 👀 当 hreflang 与当前文档使用的语言不同时，用另一个翻译的页面来 `alternate`

- `type` 属性值为 `"application/rss+xml"` 或 `"application/atom+xml"` 会创建一个 syndication feed 的参照链接。

  ```html
  <link
    rel="alternate"
    type="application/atom+xml"
    href="posts.xml"
    title="Blog"
  />
  ```

- 否则，它将创建一个超链接，引用当前文档的另一种表述，其性质由 `hreflang` 和 `type` 属性赋予。

  - 如果一起给出 `hreflang` 和 `alternate`，并且 `hreflang` 的值与当前文档的语言不同，则表明引用的文档是一个翻译。
  - 如果一起给出 `type` 和 `alternate`，它表示被引用的文件是一种替代格式（如 PDF）。
  - `hreflang` 和 `type` 属性可以与 `alternate` 一同给出。

  ```html
  <link
    rel="alternate"
    href="/fr/html/print"
    hreflang="fr"
    type="text/html"
    media="print"
    title="French HTML (for printing)"
  />
  <link
    rel="alternate"
    href="/fr/pdf"
    hreflang="fr"
    type="application/pdf"
    title="French PDF"
  />
  ```

###### `author`

表示被引用的文档提供了关于当前文档或文章作者的进一步信息。与 `<link>`、`<a>` 和 `<area>` 元素有关。

`<a>` 和 `<area>` 元素表示链接的文档（或 `mailto:`）提供了最近的祖先 `<article>` 元素的作者信息，如果无 article 元素就是整个文档的作者信息。

`<link>` 元素代表了整个文档的作者信息。

> 💡 **备注：** 由于历史原因，废弃的属性值 `rev="made"` 被视为 `rel="author"`。

###### `bookmark`

与 `<a>` 和 `<area>` 元素的 `rel` 属性值相关。如果有的话，给最近的祖先 `<article>` 元素提供一个固定链接。如果没有祖先 `<article>` 元素，则给出链接元素与之联系最紧密的部分的固定链接。

###### `canonical`

对 `<link>` 元素有效，它<font color=red>定义了当前文档的首选 URL，这**有助于搜索引擎减少重复内容**</font>。

###### `dns-prefetch`

在 `<body>` 和 `<head>` 元素内与 `<link>` 元素相关。它告诉浏览器为目标资源的来源预先执行 DNS 解析。对于用户可能需要的资源来说，它有助于减少延迟，从而提高用户访问资源时的性能，因为浏览器会预先对指定资源的来源进行 DNS 解析。

###### `external`

与 `<form>`、`<a>` 和 `<area>` 元素相关，它<font color=red>表示引用的文档不是当前网站的一部分</font>。这可以与属性选择器一起使用，使外部链接的样式向用户表明他们将离开当前网站。

###### `help`

与 `<form>`、`<link>`、`<a>` 和 `<area>` 元素相关，`help` 关键字表示链接到的内容提供上下文敏感的帮助，为定义超链接的元素的父元素及其子元素提供信息。当在 `<link>` 中使用时，针对整个文档。当与 `<a>` 和 `<area>` 一起包含并支持这种使用方法时，默认的 `cursor` 将是 `help` 而不是 `pointer`。

###### `icon`

对 `<link>` 元素有效，链接的资源代表了当前文档的图标，这是一种在用户界面上代表页面资源的方法。

`icon` 值最常见的用途是网站图标：

```html
<link rel="icon" href="favicon.ico" />
```

<font color=dodgerBlue>**如果有多个 `<link rel="icon">`**</font>，<font color=red>浏览器会使用它们的 `media`、`type` 和 `sizes` 属性来选择最合适的图标</font>。<font color=dodgerBlue>如果几个图标同样合适</font>，则<font color=red>使用最后一个</font>。如果后来发现最合适的图标不合适，例如使用了不支持的格式，浏览器就会继续选择下一个最合适的，以此类推。

###### `license`

在 `<a>`、`<area>`、`<form>`、 `<link>` 元素上有效，`license` 值表示该超链接指向描述许可信息的文件；当前文件的主要内容被引用文件描述的版权许可所覆盖。如果不在 `<head>` 元素内，标准并不区分适用于文档特定部分的超链接还是适用于整个文档的超链接。只有页面上的数据可以表明这一点。

```html
<link rel="license" href="#license" />
```

###### `manifest`

代表 [Web 应用清单](https://developer.mozilla.org/zh-CN/docs/Web/Manifest)。需要使用 CORS 协议进行跨源获取。

###### `modulepreload`

<font color=red>对于提高性能很有用</font>，并且<font color=red>与文档中的 `<link>` 元素相关</font>，设置 `rel="modulepreload"` 告诉浏览器预先获取脚本（和依赖关系）并存储在文档的模块映射中，以便以后评估。 <font color=red>`modulepreload` 链接可以确保网络抓取时，模块映射中的模块已经准备好（但没有评估）</font>，然后才一定需要它。参见 [`modulepreload`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel/modulepreload)。

###### `next`

与 `<form>`、`<link>`、`<a>` 和 `<area>` 元素相关，`next` 值表示当前文档是一个系列的一部分，引用文档是该系列的下一个文档。当包含在 `<link>` 中时，浏览器可能会假定将要获取那个文档，并将其作为一个资源提示。

###### `nofollow`

与 `<form>`、`<a>` 和 `<area>` 元素相关， `nofollow` 关键字告诉搜索引擎蜘蛛忽略链接关系。nofollow 关系可能表明当前文档的所有者并不认可被引用的文档。它经常被搜索引擎优化者包括在内，假装他们的链接农场不是垃圾页面。

###### `noopener`

与 `<form>`、`<a>` 和 `<area>` 元素相关，如果超链接一开始就会创建其中之一（即有一个适当的 `target`属性值），则会创建一个顶级浏览环境，而不是一个辅助浏览环境。换句话说，它使链接的行为如同 `window.opener`是空的，并且 `target="_parent"` 被设置。

这与 `opener` 具有的含义相反。

###### `noreferrer`

与 `<form>`、`<a>` 和 `<area>` 元素相关，包括这个值使得 referrer 未知（不会包含 `Referer` 标头），并创建一个顶级的浏览上下文，就像 `noopener` 也被设置一样。

###### `opener`

如果超链接会创建一个非辅助浏览上下文的顶级浏览上下文（即以“`_blank`”作为 `target` 属性值），则创建一个辅助浏览上下文。与 `noopener` 作用相反。

###### `pingback`

给出处理当前文档的 pingback 的 pingback 服务器地址。详见 [Pingback 规范](https://www.hixie.ch/specs/pingback/pingback)。

###### `preconnect`

向浏览器提供提示，<font color=red>建议它提前打开与链接网站的连接，而不透露任何私人信息或下载任何内容</font>，以便在跟踪链接时能更快地获取链接内容。

###### `prefetch`

指定用户代理应预先获取并缓存目标资源，因为后续导航可能需要该资源。

###### `preload`

指定用户代理必须根据 `as` 属性给出的潜在目的地（以及与相应目的地相关的优先级），为当前导航预先获取和缓存目标资源。

###### `prerender`

指定用户代理应抢先获取目标资源，并以有助于在未来提供更快的响应的方式对其进行处理，例如，获取其子资源或执行一些渲染。

###### `prev`

与 `next` 关键字类似，与 `<form>`、`<link>`、`<a>` 和 `<area>` 元素相关，`prev` 值表示当前文档是一个系列的一部分，而链接引用该系列中的一个先前文档就是被引用的文档。

备注：同义词 `previous` 并不正确，不应被使用。

###### `search`

与 `<form>`、`<link>`、`<a>` 和 `<area>` 元素相关，`search` 关键字表示该超链接引用一个文档，其界面是专门为在当前文档、站点和相关资源中搜索而设计的，提供一个可以用来搜索的资源链接。

如果 `type` 属性被设置为 `application/opensearchdescription+xml`，则该资源是一个 [OpenSearch](https://developer.mozilla.org/en-US/docs/Web/OpenSearch) 插件，可以很容易地添加到一些浏览器（如 Firefox 或 Internet Explorer）的界面中。

###### `stylesheet`

<font color=red>对 `<link>` 元素有效，它导入一个外部资源作为样式表使用</font>。<font color=red>`text/css` 的样式表不需要 `type` 属性，因为这是该属性的默认值</font>。如果它不是 `text/css` 类型的样式表，最好是声明这个类型。

虽然这个属性将链接定义为一个样式表，但与其他属性的交互以及 rel 值中的其他关键术语会影响样式表是否被下载和/或使用。

当与 `alternate` 关键字一起使用时，它定义了一个替代的样式表。在这种情况下，包括一个非空的 `title`。

如果媒体与 `media` 属性的值不匹配，外部样式表将不会被使用，甚至不会下载。

需要使用 CORS 协议进行跨源获取。

###### `tag`

对 `<a>` 和 `<area>` 元素有效，它给出了一个适用于当前文档的标签（由给定地址标识）。标签值表示该链接指向一个描述适用于其所在文档的标签的文档。这种链接类型不是指标签云中的标签，因为这些标签适用于一组页面，而 `rel` 属性的 `tag` 值是针对单个文档。

##### 非标准值

###### `apple-touch-icon`

指定 iOS 设备上的网络应用的图标。

摘自：[MDN - HTML 属性：`rel`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Attributes/rel)



#### Link preload

The `preload` value of the `<link>` element's ***`rel`*** attribute <font color=red>lets you **declare fetch requests in the HTML's `<head>`**</font> , <font color=FF0000>specifying **resources** that **your page will need very soon**</font>, which <font color=FF0000>you want to **start loading early** in the page lifecycle, <font color=fuchsia>**before browsers' main rendering machinery kicks in**</font></font>（在浏览器的主要渲染机制启动之前）. This <font color=FF0000>**ensures they are available earlier** and are <font color=fuchsia>**less likely to block the page's render**</font></font>, improving performance.

##### The basics

You <font color=LightSeaGreen>most commonly use `<link>` to load a CSS file</font> to style your page with:

```css
<link rel="stylesheet" href="styles/main.css">
```

Here however, we will use a `rel` value of `preload`, which turns `<link>` into a preloader for any resource we want. You will <font color=FF0000>**also need to specify**</font> :

- The path to the resource in the `href` attribute.

- The <font color=FF0000>type of resource in the `as` attribute</font>.

  <font color=dodgerBlue>**Using `as`** to specify the type of content to be preloaded **allows the browser to**</font>:

  - Prioritize resource loading more accurately.
  
  - <font color=FF0000>**Store in the cache for future requests**, reusing the resource if appropriate</font>.
  
    > 💡 这里的内容，可以参考 [[#Link prefetch#资源正在被预载时点击了某个链接会发生什么？]]
  
  - <font color=FF0000>Apply the correct **content security policy** ( CSP ) to the resource</font>.
  
  - <font color=FF0000>Set the correct ***`Accept` request headers*** for it</font>. 即：设置对的 `Accept` 请求头
  
  > 💡 这些在 [[#`<link>`#属性#as]] 中有提及。

```html
<link rel="preload" href="style.css" as="style">
<link rel="preload" href="main.js" as="script">
```

##### What types of content can be preloaded?

<font color=dodgerBlue>Many different content types can be preloaded. **The possible as attribute values are**</font>:

- audio: Audio file, as typically used in \<audio>
- document: An HTML document intended to be embedded by a \<frame> or \<iframe>
- embed: A resource to be embedded inside an \<embed> element
- fetch: Resource to be accessed by a fetch or XHR request, such as an ArrayBuffer or JSON file
- font: Font file
- image: Image file
- object: A resource to be embedded inside an \<object> element
- script: JavaScript file
- style: CSS stylesheet
- track: WebVTT file
- worker: A JavaScript web worker or shared worker
- video: Video file, as typically used in \<video>

##### prefetch 简介 与 对比

`<link rel="prefetch">` has been supported in browsers for a long time, but it is <font color=LightSeaGreen>intended for prefetching resources</font> that <font color=red>**will be used in the next navigation/page load**</font> (e.g. when you go to the next page). This is fine, but isn't useful for the current page! <font color=dodgerBlue>In addition</font>, <font color=FF0000>**browsers will give prefetch resources a lower priority than preload** ones</font> — <font color=LightSeaGreen>the current page is more important than the next</font>.

摘自：[MDN - Link types: preload](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types/preload#scripting_and_preloads)

#### Link prefetch

关键字 `prefetch` 作为元素 `<link>`  的属性 `rel` 的值，是为了提示浏览器：用户未来的浏览 有可能需要加载目标资源，所以浏览器有可能通过事先获取和缓存对应资源，优化用户体验。

摘自：[MDN - 链接类型：prefetch](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types/prefetch)

##### prefetching hints

浏览器会 <font color=red>**查找 关系类型 ( relation type 即：`rel` ) 为 `next` 或 `prefetch`**</font> 的 HTML `<link>`  或 HTTP `Link:` header。

下面是一个使用 `<link>` 标签的例子：

```js
<link rel="prefetch" href="/images/big.jpeg">
```

同样效果，使用 HTTP `Link:` header 的例子：

```http
Link: </images/big.jpeg>; rel=prefetch
```

`Link:` header 也可以通过使用 HTML meta 标签定义在 HTML 文档中：

```html
<meta http-equiv="Link" content="</images/big.jpeg>; rel=prefetch">
```

##### 资源正在被预载时点击了某个链接会发生什么？

<font color=dodgerBlue>当用户点击一个连接，或 开始任何形式的页面加载时</font>，<font color=FF0000>**预取操作将被停止且任何预取提示将被丢弃**</font>。<font color=FF0000>**如果一个预取文档只下载了一部分，那么 <font color=fuchsia>这部分文档将被保存在缓存中</font>，供服务端发送一个 `Accept-Ranges: bytes` 的返回头**</font>。这个返回头通常是由网络服务器在返回静态内容时生成的。当用户真正访问这个已经（部分）预载过的文档时，该文档的剩余部分将被通过一个 HTTP byte-range 的请求获取

摘自：[MDN - Link prefetching FAQ](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Link_prefetching_FAQ)

##### DNS Prefetching

<font color=dodgerBlue>**Domain lookups** can be slow, especially with network latency on mobile phones</font>. They are most relevant when there are a plethora of links to external websites that may be clicked on, like search engine results, <font color=FF0000>**DNS prefetching resolves domain names in advance** thereby **speeding up load times by reducing the time associated with domain lookup at request time**</font>.

```html
<link rel="dns-prefetch" href="https://example.com/">
```

> 👀 这部分内容，可以参考 [[#Link dns-prefetch]] ，有更详细的中文说明。

##### Link prefetching

Link prefetching is a **performance optimization** technique that works by assuming which links the user is likely to click, then downloading the content of those links. <font color=lightSeaGreen>If the user decides to click on one of the links, then the page will be rendered instantly as the content has already been downloaded</font>.

The prefetch hints are sent in HTTP headers:

```http
Link: ; rel=dns-prefetch,
      ; as=script; rel=preload,
      ; rel=prerender,
      ; as=style; rel=preload
```

摘自：[MDN US - Prefetch](https://developer.mozilla.org/en-US/docs/Glossary/Prefetch)

#### Link preconnect

The `preconnect` keyword for the **`rel` attribute of the `<link>` element** is <font color=FF0000>a hint to browsers</font> that the <font color=red>**user is likely to need resources from the target resource's origin**</font>, and therefore the **browser can likely improve the user experience** by <font color=fuchsia>**preemptively**</font> （先发制人地）<font color=fuchsia>initiating a **connection** to that origin</font>. 👀 这里的 connection 也就说明了 preconnect 的作用

```html
<link rel="preconnect" href="https://example.com">
```

摘自：[MDN US - Link types: preconnect](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types/preconnect)

> 👀 建议在 “没有很多第三方域连接” 时，`dns-prefetch` 与 `preconnect`（预连接）提示配对；原因见： [[#Link dns-prefetch#最佳实践]] 第三点。

#### Link prerender

The `prerender` keyword for the `rel` attribute of the \<link> element is a <font color=FF0000>hint to browsers that the user might need the target resource for the next navigation</font>, and <font color=FF0000>therefore the browser can likely improve the user experience by **preemptively**</font>（预先） <font color=FF0000>**fetching and processing the resource**</font> — for example, by <font color=lightSeaGreen>fetching its subresources or **performing some rendering in the background offscreen**</font>.

摘自：[MDN US - Link types: prerender](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types/prerender)

<font color=dodgerBlue>With prerendering</font>, <font color=FF0000>**the content is prefetched** and **<font size=4>then</font> rendered in the background**</font> by the browser as if the **content had been rendered into an invisible separate tab**. <font color=lightSeaGreen>When the user **navigates to the prerendered content**</font>, the <font color=FF0000>**current content is replaced by the prerendered content instantly**</font>.

```html
<link rel="prerender" href="https://example.com/content/to/prerender">
```

摘自：[MDN US - Prerender](https://developer.mozilla.org/en-US/docs/Glossary/prerender)

#### Link subresource

已经废弃，略

#### Link dns-prefetch

`DNS-prefetch`（ DNS 预获取 ）是尝试 **在请求资源之前解析域名**。这可能是后面要加载的文件，也可能是用户尝试打开的链接目标

##### 为什么要使用 `dns-prefetch` ？

<font color=lightSeaGreen>当浏览器从（第三方）服务器请求资源时，必须先将该跨域域名解析为 IP 地址，然后浏览器才能发出请求；此过程称为 DNS 解析</font>。<font color=FF0000>DNS 缓存可以帮助减少此延迟，而 DNS 解析可以导致请求增加明显的延迟</font>。对于打开了与许多第三方的连接的网站，此延迟可能会大大降低加载性能。

`dns-prefetch` 可帮助开发人员掩盖 DNS 解析延迟。 HTML `<link>` 元素 通过 `dns-prefetch` 的 `rel` 属性值 提供此功能。然后在 `href` 属性中指要跨域的域名：

```html
<link rel="dns-prefetch" href="https://fonts.googleapis.com/"> 
```

##### 最佳实践

请记住以下三点：

**首先**：**dns-prefetch <font color=fuchsia>仅对 *跨域* 域上的 DNS 查找有效</font>**，<font color=fuchsia>因此请避免使用它来指向您的站点或域</font>。<font color=LightSeaGreen>这是因为，到浏览器看到提示时，您站点域背后的 IP 已经被解析</font>。

>  💡 参考 [[#DNS Prefetching的两三事 笔记#总结]] 第五点

**其次**：可以通过使用 HTTP 链接字段将 `dns-prefetch`（以及其他资源提示）指定为 HTTP 标头：

```http
Link: <https://fonts.gstatic.com/>; rel=dns-prefetch
```

**第三**：<font color=fuchsia>**考虑将 `dns-prefetch` 与 `preconnect`（预连接）提示配对**</font>。尽管 `dns-prefetch` 仅执行 DNS 查找，但 `preconnect` 会建立与服务器的连接。如果站点是通过 HTTPS 服务的，则此过程包括 DNS 解析，建立 TCP 连接以及执行 TLS 握手。<font color=FF0000>**将两者结合起来可提供进一步减少跨域请求的感知延迟的机会**</font>。您可以安全地将它们一起使用，如下所示：

```html
<link rel="preconnect" href="https://fonts.gstatic.com/" crossorigin>
<link rel="dns-prefetch" href="https://fonts.gstatic.com/">
```

> ⚠️ 不过需要注意：<font color=FF0000>如果页面需要建立与许多第三方域的连接，则将它们 ***预先连接*** 会适得其反</font>。<font size=4>**`preconnect` 提示最好仅用于最关键的连接**</font>。对于其他的，只需使用 `<link rel="dns-prefetch">` 即可节省 ***第一步的时间** ~ **DNS 查找***

摘自：[MDN - dns-prefetch](https://developer.mozilla.org/zh-CN/docs/Web/Performance/dns-prefetch)

##### DNS Prefetching的两三事 笔记

###### 移动端痛点

既然要解析就会损耗时间，<font color=red>**对于前端特别是移动端而言，分秒必争**</font>，这个时间大家也想省去，所以浏览器厂商 Chrome 最先搞了这个新功能

###### Chrome 文档注意点

> 💡 文档地址：[The Chromium Projects - DNS Prefetching](https://www.chromium.org/developers/design-documents/dns-prefetching/)

> The most obvious example where DNS prefetching can help is when a user is looking at a page with many links to various domains, such as a search results page.

所以这个功能有个默认加载条件，所有的 a 标签的 href 都会自动去启用 DNS Prefetching，也就是说，<font color=fuchsia>网页的 a 标签 href 带的域名，是不需要在 head 里面加上 link 手动设置的</font>。

> **DNS Prefetch Control**
>
> By default, Chromium does not prefetch host names in hyperlinks that appear in HTTPS pages. <font color=fuchsia>This restriction helps **prevent an eavesdropper from inferring the host names of hyperlinks** that appear in HTTPS pages based on DNS prefetch traffic</font>. The one exception is that Chromium may periodically re-resolve the domain of the HTTPS page itself.

###### 总结

1. DNS Prefetching 是提前加载域名解析的，省去了解析时间。
2. `a` 标签的 href 是可以在 chrome，firefox 包括高版本的 IE 起作用，但是在 HTTPS 下面不起作用，需要 meta http-equiv 来强制开启功能
3. 这是 DNS 的提前解析，并不是 css，js 之类的文件缓存，不要混淆了两个不同的概念。
4. 如果直接做了 js 的重定向，或者在服务端做了重定向，没有在link里面手动设置，是不起作用的。
5. 这个对于什么样的网站更有作用呢--- 类似 taobao 这种网站，你的网页引用了大量很多其他域名的资源，<font color=lightSeaGreen>如果你的网站，基本所有的资源都在你本域名下，那么这个基本没有什么作用</font>。因为 <font color=red>DNS Chrome 在访问你的网站就帮你缓存了</font>。

![img](https://pic2.zhimg.com/v2-6072dcba45249eb25bd9cc7a9fe9af85_b.jpg)

###### 深入 Chrome 实现底层

**浏览器实现方式**

<font color=fuchsia>Chromium 直接启动了 8个完全异步的线程来做这个预解析</font>，每个线程都处理一个队列，等待域名响应，最终操作系统会响应一个 DNS 解析给线程，然后线程剔除队列，开始下一个。因为有 8 个同时，基本上最多一两个会阻塞，其他都很快执行完。所以从上也可以看出直接修改本机的 hosts 会影响浏览器的解析。也算一种简单的翻墙方式。以下命令可以查看队列状态

```url
about:histograms/DNS.PrefetchQueue
```

**浏览器启动**

<font color=red>Chrome 浏览器启动的时候，就会自动的快速解析浏览器最近一次启动时记录的 domin 的前 10 个</font>。<font color=fuchsia>所以一般说来你经常访问的网址打开的速度就没有DNS解析的延迟，更快</font>

> **Browser Startup**
>
> <font color=red>Chromium automatically remembers the first 10 domains that were resolved the last time the Chromium was started</font>, and <font color=red>automatically starts to resolve these names **very** early in the startup process</font>. As a result, the domains for a user's home page(s), along with any embedded domains (or anything the user "always" visits just after startup), are generally resolved before much of Chromium has ever loaded. When Chromium finally starts to try to load and render those pages, there is typically no DNS induced latency, and the application effectively "starts up" (becoming usable) faster. <font color=fuchsia>Average startup savings are 200ms or more, with common acceleration over 1 second</font>.

###### 功能的有效性

1. 如果本地就有缓存，那么解析大概是 0 ~ 1ms，如果去路由器查找大概是 15ms ，如果当地的服务器，一些常见的域名可能需要 150ms 左右，那么不常见的可能要 1S 以上。
2. DNS 解析的包很小，因为 DNS 是分层协议的，不需要跟 http 协议一样，一个 UDP 的包就ok，大概100bytes，快速。
3. <font color=dodgerBlue>本机的 DNS 缓存是有限，例如 XP 大概 50 到 200 个域名，所以Chrome这里做了优化</font>，会<font color=red>根据你的网站访问频率，来保证你常用的网站的DNS都能被缓存住</font>。

摘自：[DNS Prefetching的两三事 - 于秋的文章 - 知乎](https://zhuanlan.zhihu.com/p/22362198)



#### `<script>`

HTML `<script>` 元素用于<font color=FF0000> 嵌入或引用可执行脚本</font>。这通常用作嵌入或者指向 JavaScript 代码。`<script>` 元素也能在其他语言中使用，比如 WebGL 的 GLSL 着色器语言。

| 属性             | 描述                                                         |
| :--------------- | ------------------------------------------------------------ |
| 内容分类         | 元数据内容, 流式元素, 短语元素                               |
| 可用内容         | 动态脚本，如 `text/javascript`                               |
| 标签省略         | <font color=FF0000> 不允许，开始标签和结束标签都不能省略。</font> |
| 可用父元素       | 一些元素可以接受元数据内容, 或则是一些元素可以接受短语元素。 |
| 隐含的 ARIA 角色 | 没有对应的角色                                               |
| 允许的 ARIA 角色 | 不允许任何角色                                               |
| DOM接口          | HTMLScriptElement                                            |

**属性**

该元素包含全局属性。

###### `async`

对于<font color=FF0000>普通脚本</font>，如果存在 `async` 属性，那么<font color=FF0000>普通脚本会被并行请求，并尽快解析和执行</font>。

对于<font color=FF0000>模块脚本</font>，如果存在 `async` 属性，那么<font color=FF0000>脚本**及其所有依赖**都会在延缓队列中执行，因此它们会被并行请求，并尽快解析和执行</font>。

<font color=fuchsia>**该属性能够消除解析阻塞的 Javascript。解析阻塞的 Javascript 会导致浏览器必须加载并且执行脚本，之后才能继续解析**</font>。defer 在这一点上也有类似的作用。

这<font color=FF0000>是个**布尔属性**</font>：布尔属性的存在意味着 true 值，布尔属性的缺失意味着 false 值。

###### `crossorigin`

那些没有通过标准 CORS 检查的正常 `script` 元素传递最少的信息到 `window.onerror` 。<font color=FF0000>可以使用本属性来使那些将静态资源放在另外一个域名的站点打印错误信息</font>。参考 CORS 设置属性了解对有效参数的更具描述性的解释

```html
<script src="" crossorigin="anonymous"></script>
```

###### `defer`

这个 布尔属性 被设定用来通知浏览器该脚本将在文档完成解析后，触发 `DOMContentLoaded` 事件前执行。

有 `defer` 属性的脚本会阻止 `DOMContentLoaded` 事件，直到脚本被加载并且解析完成。

> ⚠️ 如果缺少 `src` 属性（即内嵌脚本），该属性不应被使用，因为这种情况下它不起作用。
>
> `defer` 属性对模块脚本没有作用 —— 他们默认 `defer`。

###### `integrity`

包含用户代理可用于验证已提取资源是否已无意外操作的内联元数据。参见 [Subresource Integrity](https://developer.mozilla.org/zh-CN/docs/Web/Security/Subresource_Integrity)。

###### `nomodule`

这个布尔属性被设置来标明这个脚本在支持 ES2015 modules 的浏览器中不执行。 — 实际上，这可用于在不支持模块化JavaScript的旧浏览器中提供回退脚本。

###### `nonce`

A cryptographic nonce (number used once) to whitelist inline scripts in a [script-src Content-Security-Policy (en-US)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/script-src). The server must generate a unique nonce value each time it transmits a policy. It is critical to provide a nonce that cannot be guessed as bypassing a resource's policy is otherwise trivial.

###### `referrerpolicy`

Indicates which [referrer](https://developer.mozilla.org/en-US/docs/Web/API/Document/referrer) to send when fetching the script, or resources fetched by the script:

- `no-referrer` : The `Referer` header will not be sent.
- `no-referrer-when-downgrade` : The `Referer` header will not be sent to [origin](https://developer.mozilla.org/en-US/docs/Glossary/Origin)s without TLS (HTTPS).
- `origin` : The sent referrer will be limited to the origin of the referring page: its [scheme](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL), [host](https://developer.mozilla.org/en-US/docs/Glossary/Host), and [port](https://developer.mozilla.org/en-US/docs/Glossary/Port).
- `origin-when-cross-origin` : The referrer sent to other origins will be limited to the scheme, the host, and the port. Navigations on the same origin will still include the path.
- `same-origin` : A referrer will be sent for same origin, but cross-origin requests will contain no referrer information.
- `strict-origin` : Only send the origin of the document as the referrer when the protocol security level stays the same (HTTPS→HTTPS), but don't send it to a less secure destination (HTTPS→HTTP).
- `strict-origin-when-cross-origin` (default) : Send a full URL when performing a same-origin request, only send the origin when the protocol security level stays the same (HTTPS→HTTPS), and send no header to a less secure destination (HTTPS→HTTP).
- `unsafe-url` : The referrer will include the origin *and* the path (but not the [fragment](https://developer.mozilla.org/en-US/docs/Web/API/HTMLAnchorElement/hash), [password](https://developer.mozilla.org/en-US/docs/Web/API/HTMLAnchorElement/password), or [username](https://developer.mozilla.org/en-US/docs/Web/API/HTMLAnchorElement/username)). **This value is unsafe**, because it leaks origins and paths from TLS-protected resources to insecure origins.

###### `src`

这个属性<font color=FF0000> 定义引用外部脚本的 URI</font>，这可以用来代替直接在文档中嵌入脚本。指定了 `src` 属性的 `script` 元素标签内不应该再有嵌入的脚本。

###### `type`

该属性定义 `script` 元素<font color=FF0000> 包含或 `src` 引用的脚本语言</font>。<font color=FF0000> 属性的值为 MIME 类型；支持的MIME类型包括 `text/javascript`，`text/ecmascript` , `application/javascript`  和 `application/ecmascript`</font>。<font color=LightSeaGreen>如果没有定义这个属性，脚本会被视作 JavaScript</font>。

<font color=dodgerBlue>如果 MIME 类型不是 JavaScript 类型（上述支持的类型）</font>，<font color=FF0000> 则该元素所包含的内容**会被当作数据块** 而 **不会被浏览器执行** </font>。

<font color=FF0000> **如果 type 属性为 module，代码会被当作 JavaScript 模块**。</font>🧪

在Firefox中可以通过定义 `type=application/javascript;version=1.8` 来使用如 let 声明这类的 JS 高版本中的先进特性。 但请注意这是个非标准功能，其他浏览器，特别是基于 Chrome 的浏览器可能会不支持。

###### `text`

和 `textContent` 属性类似，本属性用于设置元素的文本内容。但和 `textContent`  不一样的是，本属性在节点插入到 DOM 之后，此属性被解析为可执行代码。

**过时的属性**

- charset
- language

摘自：[MDN - `<script>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/script)

##### 关于 JSONP 的补充

在使用 `<script src="url">` 时，在最后加上 `?callback=cbFn`（如果原链接有 `?foo=bar` ，则在最后加上`&callback=cbFn` ；另外，这里的 callback 是定死的，不能替换），将会执行 cbFn 方法（前提是 cbFn 已定义好），示例如下：

```html
<script>
	function init () {
	  var map = new BMapGL.Map('map')
	  var point = new BMapGL.Point(116.404, 39.915)
	  map.centerAndZoom(point, 10)
	  // 可使用滚轮将地图放大缩小
	  map.enableScrollWheelZoom(true)
	}
	
	window.onload = function () {
	  var script = document.createElement('script')
	  script.src = 'https://api.map.baidu.com/api?v=1.0&type=webgl&ak=hDdCF04FamUm94EiU6j0ASAewm5PKoE4&callback=init'
	  document.body.appendChild(script)
	}
</script>
```

这里的 cbFn 即为 init 函数。这个也可以作为 JSONP 的原理。另一个示例如下：

```html
<script>
  window.callback = function (data) {
    console.log(data)
  }
</script>
<script src="http://localhost:8081/index.js"></script>
```

其中：index.js 的内容为

```json
callback: {
  foo: 'bar'
}
```

最后将会打印出 `{foo: 'bar'}`

另外，在 [现代JS教程 - Fetch：跨源请求 # 使用 script](https://zh.javascript.info/fetch-crossorigin#shi-yong-script) 中也有类似示例。



#### `<script type="importmap">`

`<script>` 元素的 `type` 属性的 `importmap` 值表示元素的主体包含一个导入映射。

导入映射 ( import map ) 是一个 JSON 对象，其允许开发者在导入 JavaScript 模块时，控制浏览器如何解析模块标识符。它<font color=red>提供了在 `import` 语句或 `import()` 运算符中用作模块标识符的文本</font>，其会在解析标识符时与要替换的文本之间建立映射。JSON 对象必须符合 [[#导入映射 JSON 表示]] 格式。

导入映射 <font color=red>用于解析 **静态** 和 **动态** 导入中的模块标识符</font>，因此必须在使用映射表中声明的标识符导入模块的任何 `<script>` 元素之前声明和处理。注意，<font color=red>导入映射仅适用于在 `import` 语句或 `import()` 运算符 中的模块标识符</font>；它<font color=red>不适用于 `<script>` 元素的 `src` 属性中指定的路径</font>。

##### 语法

```html
<script type="importmap">
  // 定义导入的 JSON 对象
</script>
```

<font color=red>不得指定 `src`、`async`、`nomodule`、`defer`、`crossorigin`、`integrity` 和 `referrerpolicy` 属性</font>。

<font color=red>**仅处理文档中具有内联定义的第一个导入映射**</font>；<font color=red>忽略任何额外的导入映射和外部导入映射</font>。

###### 异常

- `TypeError` ：导入映射的定义不是 JSON 对象、`importmap` 键已定义但它的值不是 JSON 对象或 `scopes` 键已定义但它的值不是 JSON 对象。

对于导入映射 JSON 不符合 [[#导入映射 JSON 表示]] 模式的其他情况，浏览器会生成控制台警告。

<font color=red>如果 script 元素中的 `type="importmap"` 没有被处理，则会触发 `error` 事件</font>。这是可能发生的，例如，<font color=LightSeaGreen>在处理导入模块时模块已经开始加载，或页面中定义了多个导入映射</font>。

##### 描述

当导入 JavaScript 模块时，`import` 语句和 `import()` 运算符 都有一个“模块标识符”，其指示要导入的模块。<font color=red>**浏览器必须能够将此标识符解析为绝对路径，才能导入模块**</font>。

例如，以下语句从模块标识符 `"./modules/shapes/square.js"` 导入元素，其是一个相对于文档的基础 URL 路径，而模块标识符 `"https://example.com/shapes/circle.js"` 是一个绝对路径。

```js
import { name as squareName, draw } from "./modules/shapes/square.js";
import { name as circleName } from "https://example.com/shapes/circle.js";
```

导入映射允许开发者在模块标识中指定（几乎）他们想要的任意文本；映射提供了一个相应的值，该值在解析模块标识符时替换文本。

###### 裸模块

下面的导入映射定义了一个 `imports` 键，该键具有属性为 `square` 和 `circle` 的“模块标识符映射”。

```html
<script type="importmap">
  {
    "imports": {
      "square": "./module/shapes/square.js",
      "circle": "https://example.com/shapes/circle.js"
    }
  }
</script>
```

使用导入映射，我们可以导入以上相同的模块，但在我们的模块标识符要使用“裸模块 ( bare module )”。

```js
import { name as squareName, draw } from "square";
import { name as circleName } from "circle";
```

###### 映射路径前缀

模块标识符映射键也可用于重新映射模块标识符中的路径前缀。请注意，在这种情况下，属性和映射路径都必须有一个尾随的正斜杠（`/`）。

```html
<script type="importmap">
  {
    "imports": {
      "shapes/": "./module/shapes/",
      "othershapes/": "https://example.com/modules/shapes/"
    }
  }
</script>
```

然后我们可以这样导入 circle 模块。

```js
import { name as squareName, draw } from "shapes/circle.js";
```

> 💡 从上面的示例看来，相当于给 js 文件 的前缀路径起了别名，起到简化代码的作用。

###### 模块标识符映射键中的路径

<font color=dodgerBlue>模块标识符键不必是单个单词名称（“裸模块的名称”）</font>。它们<font color=red>也可以包含路径分隔符或者以路径分隔符结尾，或者是绝对 URL，或者以 `/`、`./` 或 `../` 开始的相对 URL</font>。

```json
{
  "imports": {
    "modules/shapes/": "./module/src/shapes/",
    "modules/square": "./module/src/other/shapes/square.js",
    "https://example.com/modules/square.js": "./module/src/other/shapes/square.js",
    "../modules/shapes/": "/modules/shapes/"
  }
}
```

如果模块标识符映射中对应几个可能匹配的模块标识符键，则将选择最具体的键（即具有较长路径/值的键）。

在匹配之前，`./Foo/../js/app.js` 的模块说明符将解析为 `./js/app.js`。这意味着，即使 `./js/app.js` 的模块标识符键与模块标识符不完全相同，它们也是匹配的。

###### 作用域模块标识符映射

你<font color=red>可以使用 `scopes` 键提供映射</font>，仅当导入模块的脚本包含特定的 URL 路径时才使用。仅当导入模块的脚本包含一个指定的 URL 路径时，你才可以使用 `scopes` 键去提供映射。如果加载的脚本 URL 匹配提供的路径，则将使用与作用域相关联的映射。这允许根据正在导入的代码使用不同版本的模块。

例如，仅有加载的模块包含以下路径的 URL：“/modules/customshapes/”，映射才会使用作用域映射。

```html
<script type="importmap">
  {
    "imports": {
      "square": "./module/shapes/square.js"
    },
    "scopes": {
      "/modules/customshapes/": {
        "square": "https://example.com/modules/shapes/square.js"
      }
    }
  }
</script>
```

如果多个作用域与引用 URL 匹配，则使用最具体的作用域路径（具有最长名称的作用域键名称）。如果没有匹配的标识符，浏览器将会回落到下一个更具体的作用域路径，以此类推，最后会回落到 `imports` 键的模块标识符映射。

##### 导入映射 JSON 表示

以下是导入映射 JSON 表示的“正式”定义。

导入映射必须是有效的 JSON 对象，最多可以定义两个可选键：`imports` 和 `scopes`。每个键值必须是对象，可以是空。

- `imports` 可选

  该值是 模块标识符映射，它提供可能在 `import` 语句或 `import()` 运算符中出现的模块标识符文本，其会在解析时与替换它的文本之间建立映射。

  如果没有 `scopes` 路径 URL 匹配，或者如果匹配 `scopes` 路径中的模块标识符映射不包含与模块标识符匹配的键，这将是搜索匹配模块标识符的回退映射。

  `<module specifier map>` 

  “模块标识符映射” 是一个有效的 JSON 对象，其中 *键* 是在导入模块时，模块标识符可能存在的文本，并且相应的 *值* 时模块标识符解析为地址时将替换词文本的 URL 或路径。

  <font color=dodgerBlue>模块标识符映射 JSON 对象有以下要求：</font>

  - 没有键可以是空的。

  - 所有的值必须是字符串。定义有效的绝对 URL 或者以 `/`、`./` 或 `../` 开始的相对 URL。

  - 如果一个键以 `/` 结尾，那么相应的值也必须以 `/` 结尾。带有尾随的 `/` 键可以用作映射（或重新映射）模块地址的前缀。

  - 对象属性的顺序无关紧要：如果多个键可以匹配模块标识符，则使用最具体的键（换句话说，标识符“olive/branch/”将在“olive/”之前匹配）。

- `scopes` 可选

  作用域定义了特定于路径的[模块标识符映射](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/script/type/importmap#module_specifier_map)，这允许映射的选择取决于导入模块的代码路径。

  作用域对象是一个有效的 JSON 对象，其每个属性都是一个 `<scope key>`（URL 路径），并且相应的值是一个 `<module specifier map>`。如果导入模块脚本的 URL 匹配 `<scope key>` 路径，则首先检查与该键关联的 `<module specifier map>` 值是否匹配标识符符。如果有多个匹配的作用域键，则首先检查与最具体/嵌套的作用域路径关联的值是否匹配模块标识符。如果任何匹配的作用域模块标识符映射中没有匹配的模块标识符键，则使用 `imports` 中，备用的模块标识符映射。请注意，作用域不会改变地址的解析方式；相对地址总是解析为导入映射的基础 URL。

摘自：[MDN - `<script type="importmap">`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/script/type/importmap)



#### `<iframe>`

HTML 内联框架元素 ( `<iframe>` ) 表示嵌套的 浏览上下文 ( browsing context ）。它能够将另一个 HTML 页面嵌入到当前页面中。

每个嵌入的浏览上下文 ( embedded browsing context ) 都有自己的会话历史记录 ( session history ) 和 DOM树。<font color=FF0000> 包含嵌入内容的浏览上下文称为父级浏览上下文。顶级浏览上下文（没有父级）通常是由 Window 对象表示的浏览器窗口</font>。

##### 属性

该元素包含全局属性。

###### **`allow`** 

用于为 `<iframe>` 指定其特征策略。

###### **`allowfullscreen`** 

设置为 true 时，可以通过调用 `<iframe>` 的 `requestFullscreen()` 方法激活全屏模式。

> <font color=LightSeaGreen>这是一个历史遗留属性，已经被重新定义为 `allow="fullscreen"` 。</font>

###### **`allowpaymentrequest`** 

设置为 true 时，跨域的 `<iframe>` 就可以调用 Payment Request API。

> <font color=LightSeaGreen>这是一个历史遗留属性，已经被重新定义为 allow="payment"。</font>

###### `csp`

🧪 对嵌入的资源配置内容安全策略。 

###### **`width`** 

以 CSS 像素格式 HTML5，或以像素格式 HTML 4.01，或以百分比格式指定的 frame 的宽度。默认值是 300。

###### height

以 CSS 像素格式 HTML5，或像素格式 HTML 4.01，或百分比格式指定 frame 的高度。默认值为 150。

###### `importance` 🧪

表示 `<iframe>` 的 `src` 属性指定的资源的加载优先级。允许的值有：

- **`auto` ( <font color=FF0000>default</font> )**：不指定优先级。浏览器根据自身情况决定资源的加载顺序
- **`high`** ：资源的加载优先级较高
- **`low`** ：资源的加载优先级较低

###### `name`

用于定位嵌入的浏览上下文的名称。该名称可以用作 `<a>` 标签与 `<form>` 标签的 `target` 属性值，也可以用作 `<input>` 标签和 `<button>` 标签的 `formtarget` 属性值，还可以用作 `window.open()` 方法的 `windowName` 参数值

###### `referrerpolicy`

表示在获取 iframe 资源时如何发送 referrer 首部：

- **`no-referrer`** ：不发送 Referer 首部。
- **`no-referrer-when-downgrade` ( default )** ：向不受 TLS ( HTTPS ) 保护的 origin 发送请求时，不发送 Referer 首部。
- **`origin`** ：referrer 首部中仅包含来源页面的源。换言之，仅包含来源页面的 scheme, host, 以及 port。
- **`origin-when-cross-origin`** ：发起跨域请求时，仅在 `referrer` 中包含来源页面的源。发起同源请求时，仍然会在 `referrer` 中包含来源页面在服务器上的路径信息。
- **`same-origin`** ：对于 same origin （同源）请求，发送 `referrer` 首部，否则不发送。
- **`strict-origin`** ：仅当被请求页面和来源页面具有相同的协议安全等级时才发送 `referrer` 首部（比如从采用 HTTPS 协议的页面请求另一个采用 HTTPS 协议的页面）。如果被请求页面的协议安全等级较低，则不会发送 `referrer` 首部（比如从采用 HTTPS 协议的页面请求采用 HTTP 协议的页面）。
- **`strict-origin-when-cross-origin`** ：当发起同源请求时，在 `referrer` 首部中包含完整的 URL。当被请求页面与来源页面不同源但是有相同协议安全等级时（比如 HTTPS → HTTPS），在 referrer 首部中仅包含来源页面的源。当被请求页面的协议安全等级较低时（比如 HTTPS → HTTP），不发送 referrer 首部。
- **`unsafe-url`** ：始终在 `referrer` 首部中包含源以及路径 （但不包括 fragment，密码，或用户名）。这个值是不安全的, 因为这样做会暴露受 TLS 保护的资源的源和路径信息。

###### `sandbox`

该属性对呈现在 iframe 框架中的内容启用一些额外的限制条件。属性值可以为空字符串（这种情况下会启用所有限制），也可以是用空格分隔的一系列指定的字符串。有效的值有：

- **`allow-downloads-without-user-activation`** ：🧪 允许在没有征求用户同意的情况下下载文件.
- **`allow-forms`** ：允许嵌入的浏览上下文提交表单。如果没有使用该关键字，则无法提交表单。
- **`allow-modals`** ：允许嵌入的浏览上下文打开模态窗口。
- **`allow-orientation-lock`** ：允许嵌入的浏览上下文锁定屏幕方向（译者注：比如智能手机、平板电脑的水平朝向或垂直朝向）。
- **`allow-pointer-lock`** ：允许嵌入的浏览上下文使用 Pointer Lock API.
- **`allow-popups`** ：允许弹窗（例如 `window.open` , `target="_blank"` , `showModalDialog` ）。如果没有使用该关键字，相应的功能将自动被禁用。
- **`allow-popups-to-escape-sandbox`** ：允许沙箱化的文档打开新窗口，并且新窗口不会继承沙箱标记。例如，安全地沙箱化一个广告页面，而不会在广告链接到的新页面中启用相同的限制条件。
- **`allow-presentation`** ：允许嵌入的浏览上下文开始一个 presentation session。
- **`allow-same-origin`** ：如果没有使用该关键字，嵌入的浏览上下文将被视为来自一个独立的源，这将使 same-origin policy 同源检查失败。
- **`allow-scripts`** ：允许嵌入的浏览上下文运行脚本（但不能创建弹窗）。如果没有使用该关键字，就无法运行脚本。
- **`allow-storage-access-by-user-activation`** ：🧪 允许嵌入的浏览上下文通过 Storage Access API 使用父级浏览上下文的存储功能。
- **`allow-top-navigation`** ：允许嵌入的浏览上下文导航（加载）内容到顶级的浏览上下文。
- **`allow-top-navigation-by-user-activation`** ：允许嵌入的浏览上下文在经过用户允许后导航（加载）内容到顶级的浏览上下文。

###### `src`

被嵌套的页面的 URL 地址。使用 `about:blank` 值可以嵌入一个遵从同源策略的空白页。在 Firefox （version 65及更高版本）、基于 Chromium 的浏览器、Safari / iOS 中使用代码移除 `iframe` 的 `src` 属性（例如通过 `Element.removeAttribute()` ）会导致 `about:blank` 被载入 frame。

###### `srcdoc`

HTML5 only 该属性是一段HTML代码，这些代码会被渲染到 iframe 中。如果浏览器不支持 `srcdoc` 属性，则会渲染 src 属性表示的内容。

**已废弃的属性**

`align` 、`frameborder` 、`longdesc`  、`marginheight` 、`marginwidth` 、`scrolling `

摘自：[MDN - `<iframe>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe)

更多 iframe相关，可参考：[Iframe 有什么好处，有什么坏处？国内还有哪些知名网站仍用Iframe，为什么？有哪些原来用的现在抛弃了？又是为什么？ - 知乎](https://www.zhihu.com/question/20653055)



#### `<embed>`

HTML `<embed>` 元素将外部内容嵌入文档中的指定位置。此内容由外部应用程序或其他交互式内容源（如浏览器插件）提供。

##### 示例

<img src="https://s2.loli.net/2022/07/30/rNOGSQIn4FelLKR.png" alt="image-20220730160829752" style="zoom:50%;" />

> ⚠️ **备注：**这篇文档仅定义该元素在 HTML5 中定义的部分，不包含该元素之前的声明内容和非标准的实现。

请记住，大多数现代浏览器已经弃用并取消了对浏览器插件的支持，所以<font color=red>如果您希望您的网站可以在普通用户的浏览器上运行，那么**依靠 `<embed>` 通常是不明智的**</font>。

| [Content categories](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Content_categories) | [Flow content](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Content_categories#flow_content), [phrasing content](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Content_categories#phrasing_content), embedded content, interactive content, palpable content. |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| Permitted content                                            | None, it is an [empty element](https://developer.mozilla.org/zh-CN/docs/Glossary/Empty_element). |
| Tag omission                                                 | Must have a start tag, and must not have an end tag.         |
| Permitted parents                                            | Any element that accepts embedded content.                   |
| Permitted ARIA roles                                         | `application`, `document`, `img`, `presentation`             |
| DOM interface                                                | `HTMLEmbedElement`                                           |

##### 属性

这个元素的属性包括 全局属性。

- `height` ：资源显示的高度，<font color=fuchsia>**in [CSS pixels](https://drafts.csswg.org/css-values/#px). -- (Absolute values only. [NO percentages](https://html.spec.whatwg.org/multipage/embedded-content.html#dimension-attributes))**</font>
- `src` ：被嵌套的资源的 URL。
- `type` ：用于选择插件实例化的 MIME 类型。
- `width` ：资源显示的宽度，<font color=fuchsia>**in [CSS pixels](https://drafts.csswg.org/css-values/#px). -- (Absolute values only. [NO percentages](https://html.spec.whatwg.org/multipage/embedded-content.html#dimension-attributes))**</font>

摘自：[MDN - \<embed>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/embed)

##### 自己写的示例

```html
<embed
  class="pdf-preview"
  src="./Downloads/cheatsheet-a5.pdf"
  type="application/pdf"
/>

<style>
  .pdf-preview {
      width: 100vw;
      height: 100vh;
  }
</style>
```

在 Chrome 效果如下：

<img src="https://s2.loli.net/2022/07/30/yPLYexQdWmMknST.png" alt="image-20220730154912282" style="zoom:30%;" />

这个 pdf 预览组件 是 Chrome 自带的，Edge 和 Safari 都有自己的实现；这里就不截图了。

> ⚠️ 另外，需要注意的的是：虽然 `<embed>` 标签提供了 `width` 和 `height` 两个属性；但是，这两个属性只接收数字，且单位只能是 px；这样就导致很不灵活，建议还是使用 css 设置，如上面的代码。



#### `<ruby>`

The **`<ruby>`** HTML element represents small annotations that are rendered above, below, or next to base text, <font color=lightSeaGreen>usually used for showing the pronunciation of East Asian characters</font>. It can also be used for annotating other kinds of text, but this usage is less common.

The term *ruby* originated as [a unit of measurement used by typesetters](https://en.wikipedia.org/wiki/Agate_(typography)), representing the smallest size that text can be printed on newsprint while remaining legible.

##### 效果如下

<img src="https://s2.loli.net/2023/08/30/SaFyZj2CzvEVidw.png" alt="image-20230830175944284" style="zoom: 45%;" />

> 💡 发现即使去掉 `<rp>` 也可以正常展示，便去搜了下 `<rp>` 的作用：
>
> > HTML `<rp>` 元素用于为那些不能使用 `<ruby>` 元素展示 ruby 注解的浏览器，提供随后的圆括号。
> >
> > 摘自：[MDN - `<rp>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/rp)

##### 属性

| Property                                                     | Description                                                  |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| [Content categories](https://developer.mozilla.org/en-US/docs/Web/HTML/Content_categories) | [Flow content](https://developer.mozilla.org/en-US/docs/Web/HTML/Content_categories#flow_content), [phrasing content](https://developer.mozilla.org/en-US/docs/Web/HTML/Content_categories#phrasing_content), palpable content. |
| Permitted content                                            | [Phrasing content](https://developer.mozilla.org/en-US/docs/Web/HTML/Content_categories#phrasing_content). |
| Tag omission                                                 | None, both the starting and ending tag are mandatory.        |
| Permitted parents                                            | Any element that accepts [phrasing content](https://developer.mozilla.org/en-US/docs/Web/HTML/Content_categories#phrasing_content). |
| Implicit ARIA role                                           | [No corresponding role](https://www.w3.org/TR/html-aria/#dfn-no-corresponding-role) |
| Permitted ARIA roles                                         | Any                                                          |
| DOM interface                                                | `HTMLElement`                                                |

##### Attributes

This element only includes the global attributes.

摘自：[MDN - `<ruby>`: The Ruby Annotation element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ruby)



#### `manifest`

> ⚠️ <font color=FF0000>已从标准中移除</font>

`manifest`  属性是 HTML5 中的新属性。

##### 定义和用法

`manifest` 属性<font color=FF0000>规定文档的缓存 manifest 的位置</font>。（💡 manifest文件的后缀名必须为 **`.appcache`** ）

HTML5 引入了应用程序缓存，<font color=LightSeaGreen>这意味着 Web 应用程序可以被缓存，然后在无互联网连接的时候进行访问</font>。

###### 应用程序缓存使得应用程序有三个优点

- **离线浏览：**用户可以在离线时使用应用程序
- **快速：**缓存的资源可以更快地加载
- **减少服务器加载：**浏览器只从服务器上下载已更新 / 已更改的资源

`manifest` 属性<font color=FF0000>应该被 Web 应用程序中您想要缓存的每个页面包含</font>。

manifest 文件<font color=FF0000>是一个简单的文本文件，列举出了浏览器用于离线访问而缓存的资源</font>。

##### 语法

```html
<html manifest="URL">
```

###### 属性

- URL：文档的缓存 manifest 的地址。可能的值：
  - **绝对 URL** ：指向另一个网站（比如 `href="http://www.example.com/demo.appcache"` ）
  - **相对 URL** ：指向网站内的一个文件（比如 `href="demo.appcache"` ）

##### 示例

```html
<html manifest="demo.appcache">
```

摘自：[RUNOOB - HTML \<html> manifest 属性](https://www.runoob.com/tags/att-html-manifest.html)

manifest 文件是简单的文本文件，它告知浏览器被缓存的内容（以及不缓存的内容）。

##### manifest 文件可分为三个部分

- **CACHE MANIFEST** ：在此标题下列出的文件将在首次下载后进行缓存
- **NETWORK** ：在此标题下列出的文件需要与服务器的连接，且不会被缓存
- **FALLBACK** ：在此标题下列出的文件规定当页面无法访问时的回退页面（比如 404 页面）

摘自：[RUNOOB - HTML5 应用程序缓存](https://www.runoob.com/html/html5-app-cache.html)

***



## CSS

#### user agent stylesheet

user agent stylesheet 是 UA（一般理解为 浏览器）内置的 基本元素样式，不同的 UA 有不同的设计。

以 div 和 span 为例，看起来它们分别是 `display: block` 和 `display: inline` 的，但是，如果给他们都加上 `display: initial` ，即将其变成原始（未受到 user agent stylesheet 影响的 `display` 属性），可以发现它们在浏览器 DevTools - Element - Computed 中都变成了 `display: inline`  ，所以从纯粹 CSS 的角度（不受 浏览器 / user agent stylesheet 影响）来讲，它们是相同的；而在浏览器的角度来讲，它们是不同的

**不同浏览器的 user agent stylesheet 链接如下：**

- Gecko (Firefox): https://searchfox.org/mozilla-central/source/layout/style/res/html.css

- Chromium (Chrome): https://chromium.googlesource.com/chromium/src/third_party/+/master/blink/renderer/core/html/resources/html.css

- WebKit (Safari): https://trac.webkit.org/browser/trunk/Source/WebCore/css/html.css

学习自：CodingStartup 的 [div===span？是，也不是](https://www.bilibili.com/video/BV19h411q7d8)

> 💡 **补充：**浏览器中的 基本元素 是 shadow dom，无法看到其中的内部的样式；可以通过打开 DevTools 的 show user agent shadow DOM，在 DevTools 中便可以看到 这些内部样式。

**Chrome 开启 show user agent shadow DOM 方法：**

<img src="https://s2.loli.net/2022/02/12/OaGd6fB15TqovE2.png" alt="image-20220212162137972" style="zoom: 48%;" />

学习自：[stack overflow - How to show CSS Styles of Shadow Dom in Chrome DevTools](https://stackoverflow.com/questions/19316610/how-to-show-css-styles-of-shadow-dom-in-chrome-devtools)



#### width

**`width`** 属性用于设置元素的宽度。`width` 默认设置[内容区域](https://developer.mozilla.org/zh-CN/docs/CSS/CSS_box_model/Introduction_to_the_CSS_box_model#content-area)的宽度，但如果 `box-sizing` 属性被设置为 `border-box`，就转而设置[边框区域](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_box_model/Introduction_to_the_CSS_box_model#border-area)的宽度。

<font color=fuchsia>**`min-width` 和 `max-width` 属性的优先级高于 `width`**</font>。

##### 语法

```css
/* <length> values */
width: 300px;
width: 25em;

/* <percentage> value */
width: 75%;

/* Keyword values */
width: max-content;
width: min-content;
width: fit-content(20em); /* ⭐️ */
width: auto;

/* Global values */
width: inherit;
width: initial;
width: unset;
```

`width` 属性也指定为：

- 下面关键字值之一：`min-content`，`max-content`，`fit-content`，`auto`。
- 一个长度值 `<length>` 或者百分比值 `<percentage>`。

###### 值

- `<length>` ：使用绝对值定义宽度。
- `<percentage>` ：使用外层元素的容纳区块宽度（the containing block's width）的百分比定义宽度。
- `auto` ：浏览器将会为指定的元素计算并选择一个宽度。
- `max-content` 🧪 ：元素内容固有的（intrinsic）合适宽度。
- `min-content` 🧪 ：元素内容固有的最小宽度。

- <font color=fuchsia>**`fit-content`**</font> 🧪

  取以下两种值中的较大值：

  - 固有的最小宽度
  - 固有首选宽度（max-content）和可用宽度（available）两者中的较小值

  可表示为：`min(max-content, max(min-content, <length-percentage>))`

摘自：[MDN - width](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width#max-content)



#### padding margin border

##### 示图

注意下面的名词（marign-top...）

![](https://i.loli.net/2020/08/15/86cKqSQWjda2nOH.gif)

W3C 组织建议把所有网页上的对像都放在一个盒 (box) 中，设计师可以通过创建定义来控制这个盒的属性，这些对像包括段落、列表、标题、图片以及层。<font color=dodgerBlue>盒模型主要定义四个区域</font>：内容 (content)、<font color=FF0000>内边距(padding)</font>、<font color=FF0000>边框(border)</font> 和<font color=FF0000>外边距(margin)</font>。

**进一步用图说明：**

![](https://i.loli.net/2020/08/15/WhgXSk4sBYTCvmq.jpg)

- margin：层的边框以外留的空白

- background-color：背景颜色

- background-image：背景图片

- padding：层的边框到层的内容之间的空白 （填充）

- border：边框 

- content：内容

##### margin

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

###### 补充

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

##### Padding

包括 padding-top、padding-right、padding-bottom、padding-left，**控制块级元素内部**，content 与 border 之间的距离。代码和margin类似。

##### margin 折叠

当你想让两个元素的 content 在垂直方向 ( vertically ) 分隔时，既可以选择 padding-top / bottom，也可以选择 margin-top / bottom，在此建议你尽量使用 padding-top / bottom 来达到你的目的，这是因为 css 中存在 Collapsing margins（折叠的 margins ）的现象。

Collapsing margins : margins 折叠现象只存在于临近或有从属关系的元素，垂直方向的 margin 中。

**详细说明如下：**

- 如果只提供一个，将用于全部的四条边；
- 如果提供两个，第一个用于上－下，第二个用于左－右；
- 如果提供三个，第一个用于上，第二个用于左－右，第三个用于下；
- 如果提供全部四个参数值，将按上－右－下－左的顺序作用于四边。

```css
body { padding: 36px;} /* 对象四边的补丁边距均为36px */
body { padding: 36px 24px; } /* 上下两边的补丁边距为36px，左右两边的补丁边距为24px */
body { padding: 36px 24px 18px; } /*上、下两边的补丁边距分别为36px 18px，左右两边的补丁边距为24px*/
body { padding: 36px 24px 18px 12px; } /* 上、右、下、左补丁边距分别为36px、24px、18px、12px */
```

摘自：[CSS padding margin border属性详解](https://www.cnblogs.com/linjiqin/p/3556497.html)

##### margin 和 padding 的百分比参照物

当 margin 、padding 取值为百分比时，<font color=fuchsia size=4>百分比的值是以 **父元素的 width** 为参考</font>

> ⚠️ 上面这点非常重要，最近实现类似功能时，发现忘记了

摘自：[2023年CSS自适应正方形必须拿下🏆](https://juejin.cn/post/7204485623461691450)

> 💡 关于上面的文章，介绍了 `aspect-ratio: width-ratio / height-ratio` 的的方法，通过 `aspect-ratio: 1/1` 来实现。不过，兼容性不太好：
>
> <img src="https://s2.loli.net/2023/03/30/bnWGNR86zu1TxoD.png" alt="image-20230330015514387" style="zoom:50%;" />
>
> 所以，可以考虑通过：
>
> ```css
> .aspect-ratio-realize {
>   width: width-val;
>   height: 0;
>   padding-bottom: aspect-ratio-val;
> }
> ```
>
> 实现。在这里，为了实现正方形， `aspect-ratio-val` 为 100%。
>
> ⚠️ 这的注意的是：如果上面的 `.aspect-ratio-realize` 是作为 `img` 标签直接使用的话 ( `img.aspect-ratio-realize` )，图片将只会按照原尺寸展示，无论是否加上 `object-fit: contain` 。这时候，需要将 `.aspect-ratio-realize` 元素作为 `img` 标签的外层使用。示例如下：
>
> ```scss
> .img-wrap { // 原本的 `.aspect-ratio-realize`
>   position: relative;
>   width: width-val;
>   height: 0;
>   padding-bottom: aspect-ratio-val;
>   
>   .img {
>      position: absolute;
>      width: 100%;
>      height: 100%;
>      top: 0;
>      left: 0;
>      object-fit: cover;
>      object-position: center;
>   }
> }
> ```
>
> 参考自 [2023年CSS自适应正方形必须拿下🏆](https://juejin.cn/post/7204485623461691450)

##### cheatsheet

![](https://i.loli.net/2020/10/06/rKV25lxOgMSGjmL.png)



#### 盒子模型相关

##### 边距塌陷

给两个盒子同时设置外边距，他们最终的距离可能不是两者外边距之和。

这种现象会发生在相邻盒子间或父子盒子间，当他们都设置了边距时。

- 如果都是正数，则取最大值。
- 如果相同，则取其任一。
- 如果正负都有，取最大正数与最小负数之和。
- 如果都是负数，则取两者中最小的

另外，<font color=FF0000>设置元素为 BFC 可以解决边距塌陷的问题。</font>

##### 块格式化上下文

**块格式化上下文**（Block Formatting Context , BFC） 是Web页面的可视CSS 渲染的一部分，是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域。

###### 下列方式会创建块格式化上下文

- 根元素 `<html>`
- <font color=FF0000>浮动元素（元素的 float 不是 none）</font>
- <font color=FF0000>绝对定位元素（元素的 position 为 absolute 或 fixed）</font>
- <font color=FF0000>行内块元素（元素的 display 为 inline-block）</font>
- 表格单元格（元素的 display 为 table-cell，HTML表格单元格默认为该值）
- 表格标题（元素的 display 为 table-caption，HTML表格标题默认为该值）
- 匿名表格单元格元素（元素的 display 为 table、table-row、 table-row-group、table-header-group、table-footer-group（分别是HTML table、row、tbody、thead、tfoot 的默认属性）或 inline-table）
- <font color=FF0000>overflow 计算值( Computed )不为 visible 的块元素</font>
- display 值为 flow-root 的元素
- contain 值为 layout、content 或 paint 的元素
- 弹性元素（display 为 flex 或 inline-flex 元素的直接子元素）
- 网格元素（display 为 grid 或 inline-grid 元素的直接子元素）
- 多列容器（元素的 column-count 或 column-width 不为 auto，包括 column-count 为 1）
- column-span 为 all 的元素始终会创建一个新的BFC，即使该元素没有包裹在一个多列容器中（标准变更，Chrome bug）。

摘自：[MDN - 块格式化上下文](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)


##### 格式化上下文( formatting contexts )

格式化上下文有几种类型，包括：

- <font color=FF0000>**块**</font>格式化上下文 block formatting contexts，即BFC
- <font color=FF0000>**内联**</font>格式化上下文 inline formatting contexts，即 <font color=FF0000 size=4>**IFC**</font>
- <font color=FF0000>**灵活**</font>格式化上下文 flex formatting contexts。

<font color=FF0000>页面上的所有内容都是格式化上下文 formatting context 的一部分</font>，<font color=FF0000>或者是一个以特定方式显示的区域</font>。块格式上下文（BFC）将根据块布局规则布局子元素，<font color=FF0000>**灵活格式上下文 flex formatting context 将其子元素布局为灵活项flex items等**</font>。每个格式上下文在其上下文中都有特定的布局规则

<font color=FF0000>文档最外层元素使用块布局规则或称为初始块格式上下文</font>。<mark>这意味着 \<html> 元素块中的每个元素都是按照正常流程遵循块和内联布局规则进行布局的</mark>。\<html> 元素不是唯一能够创建块格式上下文的元素。默认为块布局的任何元素也会为其后代元素创建块格式上下文。此外，还有一些CSS属性可以使元素创建一个BFC（这里略，详见其他地方）

摘自：[MDN - Introduction to formatting contexts 格式化上下文简介](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flow_Layout/Intro_to_formatting_contexts)

##### 行内格式化上下文 IFC（Inline formatting context）

行内格式化上下文是一个网页的渲染结果的一部分。其中，<font color=FF0000>**各行内框 ( inline boxes ) 一个接一个地排列**</font>，其排列顺序根据书写模式（writing-mode）的设置来决定：

- 对于<font color=FF0000>水平书写模式</font>（由 writing-mode 属性控制），<font color=FF0000>各个框从左边开始水平地排列</font>

- 对于<font color=FF0000>垂直书写模式，各个框从顶部开始水平地排列</font>

摘自：[MDN - 行内格式化上下文（Inline formatting context）](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Inline_formatting_context)

补充：感觉上面的介绍好像没看懂，参考：[MDN - Introduction to formatting contexts 格式化上下文简介](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flow_Layout/Intro_to_formatting_contexts) 中的介绍：

<font color=FF0000>内联格式上下文**存在于其他格式上下文中**，可以将其视为段落的上下文</font>。段落创建了一个内联格式上下文，<font color=FF0000>其中在文本中使用诸如 \<strong>、\<a> 或 \<span> 元素等内容</font>。

box model 不完全适用于参与内联格式上下文。在水平书写模式行中，水平填充、边框和边距将应用于元素，并左右移动文本。但是，元素上方和下方边距将不适用。应用垂直填充和边框可能会在内容的上方和下方重叠，因为在内联格式上下文中，填充和边框不会将行框撑开。

![image-20220115194847850](https://s2.loli.net/2022/01/15/acflMQ3Xz7NCKkP.png)

摘自：[MDN - Introduction to formatting contexts 格式化上下文简介](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flow_Layout/Intro_to_formatting_contexts)

> 💡 在学习 标准流（标准文档流）和 BFC 的同时，发现了这样一篇文章：[CSS 布局的本质是什么](https://zhuanlan.zhihu.com/p/395050907) 摘自部分以做补充：

根据操作系统不同，会有不同的界面的开发方式。安卓、ios、windows 等都有各自的创建 ui 的库，但是更底层的绘图库却是有标准的：跨平台的绘图 api 接口标准 OpenGL 以及 windows 下的 DirectX。

现在开发 web 应用并不会直接基于 dom api，而是会选择某一个前端框架，比如 vue、react、angular 等。<font color=FF0000>这些框架实现了组件的功能，也就是对页面做的逻辑的拆分，把相同功能的 html、css、js 聚合在一起，使之可以复用</font>。并且<font color=FF0000>提供了 mvvm 的功能，自动做数据到具体 dom 的映射，而不再需要开发者手动操作 dom</font>。
<font color=FF0000>前端框架做的事情相当于是 web 应用的逻辑层，最终的渲染和交互还是通过 dom api</font>，但是 <font color=FF0000>用户不需要直接操作，而是在逻辑层描述组件和数据，由前端框架完成数据到 dom 的自动映射</font>。

（盒子模型）盒与盒之间也是有区别的，有的盒可以在同一行显示，有的则是独占一行，而且对内容的位置的计算方式也不一样。于是 <font color=fuchsia>**提供了 display 样式来设置盒类型**</font>（👀 这种说法，之前没听说过；虽然感觉就是 formatting context 概念那一套），比如 block、inline、inline-block、flex、table-cell、grid 等，分别<font size=4>设置成不同的盒类型，就会使用不同的计算规则</font>。

- **block** 的元素会独占一行、可以设置内容的宽高，具体计算规则叫做 BFC。
- **inline** 的元素宽高由内容撑开不可设置，不会独占一行，具体计算规则叫做 IFC。
- **flex** 的子元素可以自动计算空白部分，由 flex 样式指定分配比例，具体计算规则叫做 <font color=FF0000>**FFC**</font>。
- **grid** 的子元素则是可以拆分成多个行列来计算位置，具体计算规则叫 <font color=FF0000>**GFC**</font>。

这些都是不同盒类型的布局计算规则。

> 💡 在 stack overflow 有这样一个问题：[How many CSS formatting contexts are there?](https://stackoverflow.com/questions/16908438/how-many-css-formatting-contexts-are-there) 同样解释了这个问题：
>
> In general, a "formatting context" is simply an area in which descendant boxes of a certain kind (e.g. block, inline, flex-item) are laid out (or formatted) in normal flow.
>
> In CSS2.1, there are only two kinds of formatting context: block and inline. Both of these are described as appropriate in [section 9.4](http://www.w3.org/TR/CSS21/visuren.html#normal-flow). There is no such thing as a table formatting context, at least not as defined by CSS2.1; instead it simply says that [a table box establishes a block formatting context](http://www.w3.org/TR/CSS21/tables.html#model), however its contents are laid out in a tabular fashion.
>
> Other types of formatting context are defined in their respective CSS3 modules, so there may not be an exhaustive list. That said, some examples include:
>
> - [Flexbox](http://www.w3.org/TR/css3-flexbox): flex containers establish flex formatting contexts.
> - [Grid Layout](http://www.w3.org/TR/css3-grid-layout): grid containers establish grid formatting contexts.

摘自：[CSS 布局的本质是什么](https://zhuanlan.zhihu.com/p/395050907)



#### id & class

##### id选择器

- id 选择器可以 <font color=lightSeaGreen>为标有特定 id 的 HTML 元素指定特定的样式</font>。

- HTML 元素以 `id` 属性来设置 id 选择器，**<font color=FF0000>CSS 中 id 选择器以 "#" 来定义</font>**。

- <font color=red>`id` 属性不要以数字开头</font>，数字开头的 `id` 在 Mozilla/Firefox 浏览器中不起作用。
  
  示例如下：
  
  ```html
  <!DOCTYPE html>
  <html>
    <head>
    <meta charset="utf-8"> 
    <title>菜鸟教程(runoob.com)</title> 
    <style>
    #para1 {
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

##### class 选择器

- class 选择器用于描述一组元素的样式，<font color=FF0000>class 选择器有别于id选择器，**class 可以在多个元素中使用**</font>。

- class 选择器在HTML中以class属性表示，在 CSS 中，<font color=FF0000>**类选择器以一个点"."号显示**</font>



#### 多重样式（继承）优先级

（**内联样式）Inline style > （内部样式）Internal style sheet >（外部样式）External style sheet > 浏览器默认样式**

> 👀 类似于就近原则



#### float

`float` CSS 属性指定一个元素应沿其容器的左侧或右侧放置，允许文本和内联元素环绕它。<font color=FF0000>该元素从网页的正常流动（normal flow 文档流、正常流）中移除</font>，尽管仍然保持部分的流动性（与绝对定位相反）。

###### 可选值

- **`left`** ：表明元素必须浮动在其所在的块容器左侧的关键字。
- **`right`** ：表明元素必须浮动在其所在的块容器右侧的关键字。
- **`none`** ：<font color=red>表明元素不进行浮动的关键字</font>。
- **`inline-start`** ：关键字，<font color=LightSeaGreen>表明元素必须浮动在其所在块容器的开始一侧，在 ltr 脚本中是左侧，在 rtl 脚本中是右侧</font>
- **`inline-end`** ：关键字，<font color=lightSeaGreen>表明元素必须浮动在其所在块容器的结束一侧，在 ltr 脚本中是右侧，在 rtl 脚本中是左侧</font>

> 💡 补充
>
> ##### `inline-start` 和 `inline-end` 的兼容性
>
> 在 [MDN - float 的 **演示**](https://developer.mozilla.org/zh-CN/docs/Web/CSS/float#%E5%B0%9D%E8%AF%95%E4%B8%80%E4%B8%8B) 中，发现 `inline-start` 和 `inline-end` 旁边都打了 ❌ 的标志，在文档中搜索了下发现兼容性很差；还搜了下 [Can I use](https://caniuse.com/?search=float%3A%20inline-) 中相关兼容性，发现在 Chrome 和 Edge 等 Chromium 内核的浏览器中，这两者在现在和可规划的将来都属于完全不兼容的状态；如下截图：
>
> <img src="https://s2.loli.net/2023/06/10/teQzufDwUc1Mlno.png" alt="image-20230610150132884" style="zoom:50%;" />
>
> 甚至感觉可以彻底忘掉这两个属性了...

**由于 `float` 意味着使用块布局，它<font color=fuchsia>在某些情况下修改 `display` 值的计算值</font>：**

| 指定值               | 计算值      |
| :------------------- | :---------- |
| `inline`             | `block`     |
| `inline-block`       | `block`     |
| `inline-table`       | `table`     |
| `table-row`          | `block`     |
| `table-row-group`    | `block`     |
| `table-column`       | `block`     |
| `table-column-group` | `block`     |
| `table-cell`         | `block`     |
| `table-caption`      | `block`     |
| `table-header-group` | `block`     |
| `table-footer-group` | `block`     |
| `inline-flex`        | `flex`      |
| `inline-grid`        | `grid`      |
| *other*              | *unchanged* |

> 👀 上面的表格中没有 `flex` 和 `grid`  ，也就是说：<font color=fuchsia>`float` 无法修改 `flex` 和 `grid`</font>
>
> 💡 **关于上面表格的总结：**
>
> float 可以修改除 flex 和 grid 以外 `display` 中 <font color=fuchsia>部分</font> 的值，比如修改 `inline-flex` 为 `flex` ，修改 `inline-grid` 为 `grid` ，还会将 **其他** 受影响的值修改为 `block` 。

摘自：[MDN - `float`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/float)

##### 个人项目体会补充

###### 问题

float 和 `margin-left: auto` & `margin-right: auto` 的关系：在 flexbox 中，float 是失效的（无法影响或者修改 `display: flex`）；这时如果想实现 `float` 的效果，该怎么处理？

###### 方法

可以使用 `margin-left: auto` 替代 `float: right` ， `margin-right: auto` 替代 `float: left`（这里使用绝对定位似乎也可以实现）。跟进一步：如果 flexbox 使用了`flex-wrap: wrap` 且 右侧没有空间了，那只能换行，且放到下一行的右侧。这种情况下，是用绝对定位也不行了（无法放到下一行，只能放到该行末尾，会出现覆盖的情况）；这时使用 `margin-right: auto` 依然是有效的。原理是：使用 `auto` <font color=FF0000>会将所有剩余空间都占满</font>；类似的可参考《CSS权威指南》



#### clear

`clear` CSS 属<font color=red>性指定一个元素是否必须移动（清除浮动后）到在它之前的浮动元素下面</font>。`clear` 属性适用于浮动和非浮动元素。

> 💡 上面的说法有点抽象，参考原文的效果示例：
>
> <img src="https://s2.loli.net/2023/06/10/TSqbIcjRJPnGNKg.png" alt="image-20230610160253704" style="zoom:42%;" />
>
> <img src="https://s2.loli.net/2023/06/10/3RmZny9AYiaXPQt.png" alt="image-20230610160340503" style="zoom:42%;" />

<font color=dodgerBlue>当应用于 **非浮动块** 时</font>，它 <font color=fuchsia>**将非浮动块的边框边界移动到所有相关浮动元素外边界的下方**</font>。这个非浮动块的顶部外边距会折叠。

另一方面，两个浮动元素的垂直外边距将不会折叠。<font color=dodgerBlue>当应用于 **浮动元素** 时</font>，它将底部元素的外边界边缘移动到所有相关的浮动元素外边界边缘的下方。这会影响后面浮动元素的布局，因为后面的浮动元素的位置无法高于它之前的元素。

要被清除的相关浮动元素指的是在相同块级格式化上下文中的前置浮动。

##### 语法

```css
/* Keyword values */
clear: none;
clear: left;
clear: right;
clear: both;
clear: inline-start;
clear: inline-end;

/* Global values */
clear: inherit;
clear: initial;
clear: unset;
```

###### 值

- **`none`** ：元素*不会*向下移动清除之前的浮动。
- **`left`** ：元素被向下移动用于清除之前的左浮动。
- **`right`** ：元素被向下移动用于清除之前的右浮动。
- **`both`** ：元素被向下移动用于清除之前的左右浮动。
- **`inline-start`** ：该关键字表示该元素向下移动以清除其包含块的起始侧上的浮动。即在某个区域的左侧浮动或右侧浮动
- **`inline-end`** ：该关键字表示该元素向下移动以清除其包含块的末端的浮点，即在某个区域的右侧浮动或左侧浮动

> 💡 **补充**
>
> 类似 [[#`inline-start` 和 `inline-end` 的兼容性]] ，`clear: inline-*` 在 Chrome & Edge 等 Chromium 内核浏览器完全不兼容性；如下图：
>
> <img src="https://s2.loli.net/2023/06/10/Y9W4vFTnN3J72Oz.png" alt="image-20230610155537382" style="zoom:50%;" />

##### 示例补充

<font color=dodgerBlue>**如果一个元素**</font>（👀 <font color=lightSeaGreen>按照当前示例的说法，这个元素有 `id` 属性 container</font>）<font color=dodgerBlue>**里只有浮动元素**</font>，<font color=fuchsia>那它的高度会是 0</font>（👀 即塌陷）。如果你想要它自适应即包含所有浮动元素，那你需要清除它的子元素。一种方法叫做 clearfix ，即 `clear` 一个不浮动的 `::after` 伪元素（👀 即给该元素加一个 `::after` 伪元素）。

```css
#container::after {
  content: "";
  display: block;
  clear: both;
}
```

摘自：[MDN - `clear`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clear)



#### CSS Cursor

| 值                                    | 描述                                                         |
| :------------------------------------ | :----------------------------------------------------------- |
| url                                   | 需使用的自定义光标的图片URL。注释：请在此列表的末端始终定义一种普通的光标，以防没有由 URL 定义的可用光标。 |
| <font color=FF0000>**default**</font> | <font color=FF0000>**默认**</font>光标（通常是一个箭头）     |
| <font color=FF0000>auto</font>        | <font color=FF0000>默认</font>。浏览器设置的光标。           |
| crosshair                             | 光标呈现为十字线。                                           |
| <font color=FF0000>pointer</font>     | 光标呈现为指示链接的指针（一只手）                           |
| move                                  | 此光标指示某对象可被移动。                                   |
| e-resize                              | 此光标指示矩形框的边缘可被向右（东）移动。                   |
| ne-resize                             | 此光标指示矩形框的边缘可被向上及向右移动（北/东）。          |
| nw-resize                             | 此光标指示矩形框的边缘可被向上及向左移动（北/西）。          |
| n-resize                              | 此光标指示矩形框的边缘可被向上（北）移动。                   |
| se-resize                             | 此光标指示矩形框的边缘可被向下及向右移动（南/东）。          |
| sw-resize                             | 此光标指示矩形框的边缘可被向下及向左移动（南/西）。          |
| s-resize                              | 此光标指示矩形框的边缘可被向下移动（南）。                   |
| w-resize                              | 此光标指示矩形框的边缘可被向左移动（西）。                   |
| text                                  | 此光标指示文本。                                             |
| <font color=FF0000>wait</font>        | 此光标指示程序正忙（通常是一只表或沙漏）。                   |
| help                                  | 此光标指示可用的帮助（通常是一个问号或一个气球）。           |
| <font color=FF0000>not-allowed</font> | 不能执行，禁用                                               |
| zoom-in                               | 指示可被放大                                                 |
| zoom-out                              | 指示可被缩小                                                 |

另外这里的内容可以参考：[MDN - Cursor](https://developer.mozilla.org/zh-CN/docs/Web/CSS/cursor)，这里内容更加全面

> 💡 这里有三个补充：all-scroll、row-resize、col-resize，用于拖动 分割线，自定义元素的宽高。详见上面的 MDN - Cursor 以及 CodingStartup 的视频：[[JS] 实现可调侧栏](https://www.bilibili.com/video/BV1L54y197vj)



#### CSS 背景

#### background-color

##### 概览

CSS 属性中的 **background-color** 会设置元素的背景色，<font color=red>属性的值为颜色值 **或 关键字"transparent"**二者选其一</font>

| 属性           | 值                                                           |
| :------------- | ------------------------------------------------------------ |
| 初始值         | <font color=fuchsia>`transparent`</font>                     |
| 适用元素       | all elements. It also applies to `::first-letter` and `::first-line`. |
| 是否是继承属性 | 否                                                           |
| 计算值         | computed color                                               |
| Animation type | a [`<color>`](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value#interpolation) |

> ⚠️ `background-color` 的初始值是 `transparent` ，这点是完全没有想到的

##### 语法

```css
/* Keyword values */
background-color: red;

/* Hexadecimal value */
background-color: #bbff00;

/* RGB value */
background-color: rgb(255, 255, 128);

/* HSLA value */
background-color: hsla(50, 33%, 25%, 0.75);

/* Special keyword values */
background-color: currentColor;
background-color: transparent;

/* Global values */
background-color: inherit;
background-color: initial;
background-color: unset;
```

`background-color` 属性只能使用 `<color>` 值。

###### 取值

`<color>` ：一个描述背景统一颜色的 CSS `<color>` 值。<font color=dodgerBlue>**即使一个或几个的 `background-image` 被定义**</font>，<font color=red>**如果图像是不透明的，通过透明度该颜色也能影响到渲染**</font>。在 <font color=lightSeaGreen>**CSS 中，`transparent` 是一种颜色**</font>。

摘自：[MDN - `background-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-color)

#### background-repeat

**`background-repeat`** CSS 属性<font color=red>定义背景图像的重复方式</font>。<font color=lightSeaGreen>背景图像可以沿着水平轴，垂直轴，两个轴重复，或者根本不重复</font>。

默认情况下，重复的图像被剪裁为元素的大小，但<font color=red>它们可以 **缩放**（使用 `round` ）或者 **均匀地分布**（使用 `space` ）</font>

##### 语法

```css
/* 单值语法 */
background-repeat: repeat-x;
background-repeat: repeat-y;
background-repeat: repeat;
background-repeat: space;
background-repeat: round;
background-repeat: no-repeat;

/* 双值语法：水平 horizontal | 垂直 vertical */
background-repeat: repeat space;
background-repeat: repeat repeat;
background-repeat: round space;
background-repeat: no-repeat round;

background-repeat: inherit;
```

###### 值

`<repeat-style>`

<font color=dodgerBlue>单值语法</font>是<font color=lightSeaGreen>**完整的双值语法的简写**</font>：

| **单值**    | **等价于双值**        |
| :---------- | :-------------------- |
| `repeat-x`  | `repeat no-repeat`    |
| `repeat-y`  | `no-repeat repeat`    |
| `repeat`    | `repeat repeat`       |
| `space`     | `space space`         |
| `round`     | `round round`         |
| `no-repeat` | `no-repeat no-repeat` |

在<font color=dodgerBlue>双值语法</font>中，<font color=dodgerBlue>第一个值</font>表示<font color=lightSeaGreen>水平重复行为</font>，<font color=dodgerBlue>第二个值</font>表示<font color=lightSeaGreen>垂直重复行为</font>。

<font color=dodgerBlue>下面是关于每一个值是怎么工作的具体说明</font>：

| 值          | 描述                                                         |
| :---------- | :----------------------------------------------------------- |
| `repeat`    | 图像会按需重复来覆盖整个背景图片所在的区域。<font color=red>最后一个图像会被裁剪，如果它的大小不合适的话</font> |
| `space`     | <font color=red>图像会**尽可能得重复**，但是**不会裁剪**</font>。第一个和最后一个图像会被固定在元素 ( element ) 的相应的边上，同时空白会均匀地分布在图像之间。`background-position` 属性会被忽视，除非只有一个图像能被无裁剪地显示。只在一种情况下裁剪会发生，那就是图像太大了以至于没有足够的空间来完整显示一个图像。 |
| `round`     | 随着允许的空间在尺寸上的增长，<font color=red>被重复的图像将会伸展（没有空隙）</font>， 直到有足够的空间来添加一个图像。当下一个图像被添加后，所有的当前的图像会被压缩来腾出空间。<br/>例如，一个图像原始大小是 260px，重复三次之后，可能会被伸展到 300px，直到另一个图像被加进来。这样他们就可能被压缩到 225px。<br/>译者注：关键是浏览器怎么计算什么时候应该添加一个图像进来，而不是继续伸展。 |
| `no-repeat` | 图像不会被重复（因为背景图像所在的区域将可能没有完全被覆盖）。那个没有被重复的背景图像的位置是由 `background-position` 属性来决定。 |

摘自：[MDN - `background-repeat`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-repeat)

#### background-size

`background-size` 设置背景图片大小。图片可以保有其原有的尺寸，或者拉伸到新的尺寸，或者在保持其原有比例的同时缩放到元素的可用空间的尺寸。

⚠️ 注意：<font color=lightSeaGreen>没有被背景图片覆盖的背景区域仍然会显示用 `background-color` 属性设置的背景颜色</font>。此外，如果背景图片设置了透明或者半透明属性，衬在背景图片后面的背景色也会显示出来。

| 属性                          | 值                                                           |
| :---------------------------- | ------------------------------------------------------------ |
| <font color=red>初始值</font> | `auto auto`                                                  |
| 适用元素                      | all elements. It also applies to `::first-letter` and `::first-line`. |
| 是否是继承属性                | 否                                                           |
| Percentages                   | relative to the background positioning area                  |
| 计算值                        | as specified, but with relative lengths converted into absolute lengths |
| Animation type                | a repeatable list of                                         |

##### 语法

```css
/* Keyword values */
background-size: cover;
background-size: contain;

/* One-value syntax */
/* the width of the image (height becomes 'auto') */
background-size: 50%;
background-size: 3.2em;
background-size: 12px;
background-size: auto;

/* Two-value syntax */
/* first value: width of the image, second value: height */
background-size: 50% auto;
background-size: 3em 25%;
background-size: auto 6px;
background-size: auto auto;

/* Multiple backgrounds */
background-size: auto, auto; /* Not to be confused with `auto auto` */
background-size: 50%, 25%, 25%;
background-size: 6px, auto, contain;

/* Global values */
background-size: inherit;
background-size: initial;
background-size: revert;
background-size: revert-layer;
background-size: unset;
```

<font color=dodgerBlue>单张图片的背景大小可以使用以下三种方法中的一种来规定</font>：

- 使用关键词 `contain`
- 使用关键词 `cover`
- 设定宽度和高度值

当通过宽度和高度值来设定尺寸时，你可以提供一或者两个数值：

- 如果仅有一个数值被给定，这个数值将作为宽度值大小，高度值将被设定为 `auto`。
- 如果有两个数值被给定，第一个将作为宽度值大小，第二个作为高度值大小。

每个值可以是 `<length>` ，是 `<percentage>` 或者 `auto` .

###### 属性值

- **`<length>`** ：`<length>` 值，指定背景图片大小，不能为负值。
- **`<percentage>`** ：`<percentage>` 值，指定背景图片相对背景区 ( background positioning area ) 的百分比。背景区由 `background-origin` 设置，默认为盒模型的内容区与内边距，也可设置为只有内容区，或者还包括边框。如果 `attachment` 为 `fixed`，背景区为浏览器可视区（即视口），不包括滚动条。不能为负值。
- **`auto`** ：以背景图片的比例缩放背景图片。
- **`cover`** ：缩放背景图片以完全覆盖背景区，可能背景图片部分看不见。和 `contain` 值相反，`cover` 值尽可能大的缩放背景图像并保持图像的宽高比例（图像不会被压扁）。该背景图以它的全部宽或者高覆盖所在容器。<font color=red>当容器和背景图大小不同时，背景图的 左/右 或者 上/下 部分会被裁剪</font>。
- **`contain`** ：<font color=red>缩放背景图片以完全装入背景区，可能背景区部分空白</font>。`contain` 尽可能的缩放背景并保持图像的宽高比例（图像不会被压缩）。该背景图会填充所在的容器。当背景图和容器的大小的不同时，容器的空白区域（上/下或者左/右）会显示由 `background-color` 设置的背景颜色。

位图一定有固有尺寸与固有比例，矢量图可能两者都有，也可能只有一个。渐变视为只有固有尺寸或者只有固有比例的图片。

摘自：[MDN - `background-size`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-size)

#### background-attachment

**`background-attachment`** CSS 属性<font color=red>决定背景图像的位置是在视口内固定，或者随着包含它的区块滚动</font>。

> 💡 原文这里有可交互的示例，建议看一下

##### 语法

```css
/* 关键 属性值 */
background-attachment: scroll;
background-attachment: fixed;
background-attachment: local;

/* 全局 属性值 */
background-attachment: inherit;
background-attachment: initial;
background-attachment: unset;
```

###### 取值

- `fixed` ：此关键属性值表示 <font color=red>背景相对于 **viewport** 固定</font>。<font color=red>**即使一个元素拥有滚动机制，背景也不会随着元素的内容滚动**</font>。
- `local` ：此关键属性值表示 <font color=red>背景相对于 **元素的内容** 固定</font>。<font color=dodgerBlue>如果一个元素拥有滚动机制</font>，<font color=red>背景将会随着元素的内容滚动</font>，并且<font color=red>背景的绘制区域和定位区域是相对于可滚动的区域</font>而不是包含他们的边框。
- `scroll` ：此关键属性值表示 <font color=red>背景相对于 **元素本身** 固定</font>，而<font color=red>**不是随着它的内容滚动**</font>（对元素边框是有效的）

摘自：[MDN - background-attachment](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-attachment)

#### background-position

**`background-position`** CSS 属性 <font color=red>为每一个背景图片设置初始位置</font>。这个位置是 <font color=red>相对于由 `background-origin` 定义的位置图层的</font>。

##### 语法

```css
/* Keyword values */
background-position: top;
background-position: bottom;
background-position: left;
background-position: right;
background-position: center; /* 👀 */

/* <percentage> values */
background-position: 25% 75%;

/* <length> values */
background-position: 0 0;
background-position: 1cm 2cm;
background-position: 10ch 8em;

/* Multiple images */
background-position: 0 0, center; /* 👀 */

/* Edge offsets values */
background-position: bottom 10px right 20px;
background-position: right 3em bottom 10px;
background-position: bottom 10px right;
background-position: top right 10px;

/* Global values */
background-position: inherit;
background-position: initial;
background-position: revert;
background-position: unset;
```

`background-position` 属性被指定为一个或多个 `<position>` 值，用逗号隔开。

###### 值

`<position>` ：一个 `<position>` 定义一组 x/y 坐标（相对于一个元素盒子模型的边界），来放置背景图 ( item )。它<font color=dodgerBlue>可以使用一到四个值进行定义</font>。如果使用两个非关键字值，第一个值表示水平位置，第二个值表示垂直位置。<font color=dodgerBlue>**如果仅指定一个值**</font>，则<font color=red>**第二个值默认是 `center`**</font>。<font color=dodgerBlue>**如果使用三个或四个值**</font>，则<font color=red>长度百分比值是前面关键字值的偏移量</font>。

**一个值的语法：** 值可能是：

- 关键字 `center`，用来居中背景图片。
- 关键字 `top`、`left`、`bottom`、`right` 中的一个。用来指定把这个背景图 ( item ) 放在哪一个边界。另一个维度被设置成 50% ，所以这个背景图 ( item ) 被放在指定边界的中间位置。
- `<length>` 或 `<percentage>`。指定相对于左边界的 x 坐标，y 坐标被设置成 50%。

**两个值的语法：** 一个定义 x 坐标，另一个定义 y 坐标。每个值可以是：

- 关键字 `top`、`left`、`bottom`、`right` 中的一个。如果这里给出 `left` 或 `right`，那么这个值定义 x 轴位置，另一个值定义 y 轴位置。如果这里给出 `top` 或 `bottom`，那么这个值定义 y 轴位置，另一个值定义 x 轴位置。
- `<length>` 或 `<percentage>`。如果另一个值是 `left` 或 `right` ，则该值定义相对于顶部边界的 Y。如果另一个值是 `top` 或 `bottom`，则该值定义相对于左边界的 X。如果两个值都是 `<length>` 或 `<percentage>` 值，则第一个定义 X，第二个定义 Y。

注意：如果一个值是 `top` 或 `bottom`，那么另一个值不可能是 `top` 或 `bottom`。如果一个值是 `left` 或 `right`，那么另一个值不可能是 `left` 或 `right`。也就是说，例如，`top top` 和 `left right` 是无效的。

排序：配对关键字时，位置并不重要，因为浏览器可以重新排序，写成 `top left` 或 `left top` 其产生的效果是相同的。使用 `<length>` 或 `<percentage>` 与关键字配对时顺序非常重要，定义 X 的值放在前面，然后是定义 Y 的值， `right 20px` 和 `20px right`的效果是不相同的，前者有效但后者无效。`left 20%` 或 `20% bottom` 是有效的，因为 X 和 Y 值已明确定义且位置正确。

默认值是 `left top` 或者 `0% 0%`。

**三个值的语法：** 两个值是关键字值，第三个是前面值的偏移量：

- 第一个值是关键字 `top`、`left`、`bottom`、`right`，或者 `center`。如果设置为 `left`或 `right`，则定义了 X。如果设置为 `top` 或 `bottom`，则定义了 Y，另一个关键字值定义了 X。
- `<length>` 或 `<percentage>`，如果是第二个值，则是第一个值的偏移量。如果是第三个值，则是第二个值的偏移量。
- 单个长度或百分比值是其前面的关键字值的偏移量。一个关键字与两个 `length` 或 `<percentage>` 值的组合无效。

**四个值的语法：** 第一个和第三个值是定义 X 和 Y 的关键字值。第二个和第四个值是前面 X 和 Y 关键字值的偏移量：

- 第一个值和第三个值是关键字值 `top`、`left`、`bottom`、 `right` 之一。如果设置为 `left` 或 `right`，则定义了 X。如果设置为 `top` 或 `bottom`，则定义了 Y，另一个关键字值定义了 X。
- 第二个和第四个值是 `<length>` 或 `<percentage>`。第二个值是第一个关键字的偏移量。第四个值是第二个关键字的偏移量。

摘自：[MDN - `background-position`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-position)

#### background-origin

`background-origin` <font color=red>指定背景图片`background-image` 属性的原点位置的背景相对区域</font>。

⚠️ 注意：<font color=red>**当使用 `background-attachment` 为 fixed 时，该属性将被忽略不起作用**</font>。

##### 语法

```css
/* Keyword values */
background-origin: border-box;
background-origin: padding-box;
background-origin: content-box;

/* Global values */
background-origin: inherit;
background-origin: initial;
background-origin: revert;
background-origin: revert-layer;
background-origin: unset;
```

###### 属性值

- **`border-box`** ：背景图片的摆放以 border 区域为参考
- **`padding-box`** ：背景图片的摆放以 padding 区域为参考
- **`content-box`** ：背景图片的摆放以 content 区域为参考

| 属性                          | 值                                                           |
| :---------------------------- | ------------------------------------------------------------ |
| <font color=red>初始值</font> | `padding-box`                                                |
| 适用元素                      | all elements. It also applies to `::first-letter` and `::first-line`. |
| 是否是继承属性                | 否                                                           |
| 计算值                        | as specified                                                 |
| Animation type                | a repeatable list of                                         |

摘自：[MDN - `background-origin`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-origin)

#### background-clip

> 👀 感觉 background-clip 属性有点类似于 box-sizing 的作用。不过，text 属性不一样，需要注意下，在某些场景下很有用。

`background-clip` 设置元素的背景（背景图片或颜色）是否延伸到边框、内边距盒子、内容盒子下面。

> 👀 原文有各属性的效果展示，由于没法搬过来；详见 原文。

如果没有设置背景图片( `background-image` ) 或背景颜色 ( `background-color` )，那么这个属性只有在边框 ( `border` ) 被设置为非固实 ( soild )、透明或半透明时才能看到视觉效果（与 `border-style` 或 `border-image` 有关），否则，本属性产生的样式变化会被边框覆盖。

##### 语法

```css
/* Keyword values */
background-clip: border-box;
background-clip: padding-box;
background-clip: content-box;
background-clip: text;

/* Global values */
background-clip: inherit;
background-clip: initial;
background-clip: unset;
```

###### 值

- `border-box` ：背景延伸至边框外沿（但是在边框下层）。

  > 💡 经过实验，`border-box` 是默认值

- `padding-box` ：背景延伸至内边距 ( `padding` ) 外沿。不会绘制到边框处。

- `content-box` ：背景被裁剪至内容区 ( content box ) 外沿。

- `text` ：🧪 背景被裁剪成文字的前景色。

  > 👀 这个要注意下 ⚠️

摘自：[MDN - background-clip](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-clip)

#### background-blend-mode

##### 概述

`background-blend-mode` CSS 属性 <font color=red>定义该元素的 **背景图片以及背景色 如何混合**</font>。

混合模式应该按 `background-image` CSS 属性同样的顺序定义。如果混合模式数量与背景图像的数量不相等，它会被截取至相等的数量。

> 👀 这里的意思是如果 `background-image` 对应的值有图片值有多个，那么 ``background-blend-mode` 的值也 “应该”（不强制） 有多个

| 初始值         | `normal`                                                     |
| :------------- | ------------------------------------------------------------ |
| 适用元素       | All elements. In SVG, it applies to container elements, graphics elements, and graphics referencing elements.. It also applies to `::first-letter` and `::first-line`. |
| 是否是继承属性 | 否                                                           |
| 计算值         | as specified                                                 |
| Animation type | Not animatable                                               |

##### 语法

```css
/* One value */
background-blend-mode: normal;

/* Two values, one per background */
background-blend-mode: darken, luminosity;

/* Global values */
background-blend-mode: inherit;
background-blend-mode: initial;
background-blend-mode: revert;
background-blend-mode: revert-layer;
background-blend-mode: unset;
```

###### 值

`<blend-mode>` ：一个定义混合的模式，可以有多个值，用逗号间隔。

> 💡 这里的 `<blend-mode>` 见 [[#`<blend-mode>`]] 或 [MDN - `<blend-mode>`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/blend-mode)

摘自：[MDN - background-blend-mode](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-blend-mode)



#### mix-blend-mode

> 👀 感觉 [MDN - `mix-blend-mode`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/mix-blend-mode) 对于 `mix-blend-mode` 各个可选值并没有详细的讲解，不过可选值就是 `<blend-mode>` ，MDN 是有专门的页面的 [MDN - `<blend-mode>`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/blend-mode)；可见 [[#`blend-mode`]]

**`mix-blend-mode`** CSS 属性<font color=red>描述了元素的内容应该与元素的直系父元素的内容和元素的背景如何混合</font>。

##### 语法

```css
mix-blend-mode: <blend-mode>;

mix-blend-mode: initial;
mix-blend-mode: inherit;
mix-blend-mode: unset;
```

###### 值

`<blend-mode>` ：表示应该应用的混合模式。

##### 形式定义

| 初始值                                                       | `normal`       |
| :----------------------------------------------------------- | -------------- |
| 适用元素                                                     | all elements   |
| 是否是继承属性                                               | 否             |
| 计算值                                                       | as specified   |
| Animation type                                               | Not animatable |
| Creates [stacking context](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_positioned_layout/Understanding_z-index/Stacking_context) | yes            |

摘自：[MDN - `mix-blend-mode`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/mix-blend-mode)

> 💡 关于 `mix-blend-mode` 的使用，可以看下：[视频文字特效【渡一教育】](https://www.bilibili.com/video/BV1mQ4y1p7Dh) ，其中介绍了 `mix-blend-mode: screen` 的一个使用场景；下面做一些笔记

##### 关于 `mix-blend-mode: screen`  的使用

`mix-blend-mode` 可以理解为一种算法，针对每一个像素点，将其自身的颜色和背后的颜色混合以产生新的颜色，至于如何混合，就要看选择的混合算法（可以理解为 `f( color , color2 ) = new_color`），即 `mix-blend-mode` 对应的值。

其中，`screen` 是滤色算法（滤色模式）。公式如下：

$f(\ rgb(R_1, G_1, B_1),\quad rgb(R_2, G_2, B_2)\ ) = \\rgb(255-(255-R_1)*(255-R_2)/255,\quad 255-(255-G_1)*(255-G_2)/255,\quad 255-(255-B_1)*(255-B_2)/255)$

根据如上公式：如果其中一个 RGB 为 白色 `rgb(255, 255, 255)` ，则最后的颜色一定为白色；如果其中一个 RGB 为黑色 `rgb(0, 0, 0)`  最后颜色一定为另一个颜色

##### 关于 `mix-blend-mode: difference` 的使用

`difference` 即是将当前元素的颜色的 R、G、B 与父元素的颜色的 R、G、B 分别作差，产生新的颜色，作为颜色展示效果

学习自：[实现文字智能适配背景【渡一教育】](https://www.bilibili.com/video/BV11N4y1e7Lc)



#### `<blend-mode>`

`<blend-mode>` <font color=fuchisa>**是一种 CSS 数据类型**</font>，<font color=FF0000>用于描述当元素重叠时，颜色应当如何呈现</font>。它<font color=FF0000>被用于 `background-blend-mode` 和 `mix-blend-mode` 属性</font>。

> 💡 `<blend-mode>` 不是一种标签，而是一个数据类型，类似于 `<length>`，具体介绍见：[MDN - CSS 基本数据类型](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Types) 

当层重叠时，混合模式是计算像素最终颜色值的方法，每种混合模式采用前景和背景的颜色值，执行其计算并返回最终的颜色值。最终的可见层是对混合层中的每个重叠像素执行混合模式计算的结果。

##### 属性值

- `normal` ：最终颜色永远是顶层颜色，无论底层颜色是什么。 其效果类似于两张不透明的纸重叠 ( overlapping ) 在一起
- `multiply` ：最终颜色为顶层颜色与底层颜色相乘的结果。 如果叠加黑色层，则最终层必为黑色层，叠加白色层不会造成变化。 其效果类似于在透明薄膜上重叠印刷的两个图像。
- `screen` ：最终的颜色是**反转顶层颜色和底层颜色，将反转后的两个颜色相乘，再反转相加得到的和**得到的结果。 黑色层不会造成变化，白色层导致白色最终层。 其效果类似于（被投影仪）投射到投影屏幕上的两个图像。
- `overlay` ：如果底层颜色比顶层颜色深，则最终颜色是 `multiply` 的结果，如果底层颜色比顶层颜色浅，则最终颜色是 `screen` 的结果。 此混合模式相当于顶层与底层互换后的 `hard-light`。
- `darken` ：最终颜色是由每个颜色通道下，顶底两层颜色中的最暗值所组成的颜色。
- `lighten` ：最终颜色是每个颜色通道下，顶底两层颜色中的最亮值所组成的颜色。
- `color-dodge` ：最终颜色是将底部颜色除以顶部颜色的反色的结果。 黑色前景不会造成变化。前景如果是背景的反色，会得到白色（fully lit color，完全亮起的颜色，应当为白色）。 此混合模式类似于 `screen`，但是，前景只需要和背景的反色一样亮，最终图像就会变为全白。
- `color-burn` ：最终颜色是**反转底部颜色，将反转后的值除以顶部颜色，再反转除以后的值**得到的结果。 白色的前景不会导致变化，前景如果是背景的反色，会得到黑色。 此混合模式类似于 `multiply`，但是，前景只需要和背景的反色一样暗，最终图像就会变为全黑。
- `hard-light` ：如果顶层颜色比底层颜色深，则最终颜色是 `multiply` 的结果，如果顶层颜色比底层颜色浅，则最终颜色是 `screen` 的结果。 此混合模式相当于顶层与底层互换后的 `overlay`。 其效果类似于在背景层上（用前景层）打出一片_刺眼_的聚光灯。
- `soft-light` ：最终颜色类似于 `hard-light` 的结果，但更加柔和一些。 此混合模式的表现类似 `hard-light`。 其效果类似于在背景层上（用前景层）打出一片_发散_的聚光灯。
- ``difference`` ：最终颜色是 两种颜色中较浅的颜色 减去 两种颜色中较深的颜色 得到的结果。 黑色层不会造成变化，而白色层会反转另一层的颜色。
- `exclusion` ：最终颜色类似于 `difference`，但对比度更低一些。 和 `difference` 相同，黑色层不会造成变化，而而白色层会反转另一层的颜色。
- `hue` ：最终颜色由顶部颜色的_色调_和底部颜色的_饱和度_与_亮度_组成。
- `saturation` ：最终颜色由顶部颜色的_色调_和底部颜色的_饱和度_与_发光度_组成。 饱和度为零的纯灰色背景层不会造成变化。
- `color` ：最终颜色由顶部颜色的_色调_ 与_饱和度_和底部颜色的_亮度_组成。 此效果保留了灰度级别，可用于为前景着色。（The effect preserves gray levels and can be used to colorize the foreground.）
- `luminosity` ：最终颜色由顶部颜色的亮度和底部颜色的色调和饱和度组成。此混合模式相当于顶层与底层互换后的 `color`。

摘自：[MDN - `<blend-mode>`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/blend-mode)



#### font

`font` 属性可以用来<font color=LightSeaGreen>作为 `font-style` ，`font-variant` ， `font-weight` ， `font-size` ， `line-height` 和 `font-family` 属性的简写</font>，或将元素的字体设置为系统字体。

<font color=red>与任何简写属性一样，任何未指定的值都将设置为其对应的初始值</font>（可能覆盖先前使用非简写属性设置的值）。<font color=red>虽然不能通过 font 直接设置，但是[`font-stretch`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-stretch)，[`font-size-adjust`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-size-adjust) 和 [`font-kerning`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-kerning) 也会重置为初始值</font>。

##### 语法

可以将`font`属性指定为单个关键字，它将选择系统字体，或者作为字体相关的属性的简写。

如果将 `font` 指定为系统关键字，则它必须是以下之一：[`caption`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font#caption), [`icon`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font#icon), [`menu`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font#menu), [`message-box`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font#message-box), [`small-caption`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font#small-caption), [`status-bar`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font#status-bar)。

<font color=dodgerBlue>**如果 `font` 字体相关的属性的简写：**</font>

- <font color=fuchsia>必须包含以下值：`<font-size>` ，`<font-family>`</font>

- 可以选择性包含以下值：`<font-style>` ，`<font-variant>` ， `<font-weight>` ， `<line-height>`

- `font-style` ， `font-variant` 和 `font-weight` 必须在 `font-size` 之前
- 在 CSS 2.1 中 `font-variant` 只可以是 `normal` 和 `small-caps`
- <font color=fuchsia>**`line-height` 必须跟在 `font-size` 后面，由 "/" 分隔，例如 "`16px/3`"**</font>
- `font-family` 必须最后指定

摘自：[MDN - font](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font)

#### font-style

**`font-style`** CSS 属性允许你选择 `font-family` 字体下的 `italic` 或 `oblique` 样式。

**Italic** 字体一般是现实生活中的草书，相比无样式的字体，通常会占用较少的水平空间，而 **oblique** 字体一般只是常规字形的倾斜版本。如果当前字体没有对应的斜体，那么斜体 ( italic ) 和倾斜体 ( oblique ) 都会通过人工倾斜常规字体的字形来模拟（使用 [`font-synthesis`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-synthesis) 对此进行控制）。

##### 语法

```css
font-style: normal;
font-style: italic;
font-style: oblique;
font-style: oblique 10deg;

/* Global values */
font-style: inherit;
font-style: initial;
font-style: unset;
```

`font-style` 属性被指定为从下面的取值列表中的单独一个关键字，<font color=red>如果关键字是 `oblique`，则可附加一个可选的角度</font>。

###### 值

- `normal` ：选择 `font-family` 的常规字体。
- `italic` ：选择斜体，如果当前字体没有可用的斜体版本，会选用倾斜体 ( `oblique` ) 替代。

- `oblique` `<angle>` ：Selects a font classified as `oblique` , and additionally specifies an angle for the slant of the text. If one or more oblique faces are available in the chosen font family, the one that most closely matches the specified angle is chosen. If no oblique faces are available, the browser will synthesize an oblique version of the font by slanting a normal face by the specified amount. Valid values are degree values of `-90deg` to `90deg` inclusive. If an angle is not specified, an angle of 14 degrees is used. Positive values are slanted to the end of the line, while negative values are slanted towards the beginning.

  In general, for a requested angle of 14 degrees or greater, larger angles are preferred; otherwise, smaller angles are preferred (see the spec's [font matching section](https://drafts.csswg.org/css-fonts-4/#font-matching-algorithm) for the precise algorithm).

摘自：[MDN - font-style](https://developer.mozilla.org/en-US/docs/Web/CSS/font-style)



#### font-variant

// TODO

摘自：[MDN - font-variant](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-variant)



#### font-display

`font-display` 属性<font color=red>决定了一个 `@font-face` **在不同的下载时间和可用时间下** 是如何展示的</font>。

##### 字体显示时间轴

<font color=red>字体显示时间线基于一个计时器</font>，该计时器在用户代理尝试使用给定下载字体的那一刻开始。<font color=red>**时间线分为三个时间段**，在这三个时间段中指定使用字体的元素的渲染行为</font>。

- 字体阻塞周期 ：如果未加载字体，任何试图使用它的元素都必须渲染不可见的后备字体。如果在此期间字体已成功加载，则正常使用它。
- 字体交换周期 ：如果未加载字体，任何尝试使用它的元素都必须呈现后备字体。如果在此期间字体已成功加载，则正常使用它。
- 字体失败周期 ：如果未加载字体，用户代理将其视为导致正常字体回退的失败加载。

| 属性            | 值           |
| :-------------- | ------------ |
| Related at-rule | `@font-face` |
| 初始值          | `auto`       |
| 计算值          | as specified |

##### 语法

```css
/* 关键字值 */
font-display: auto;
font-display: block;
font-display: swap;
font-display: fallback;
font-display: optional;
```

###### 属性值

- `auto` ：字体显示策略由用户代理定义。

- `block` ：为字体提供一个短暂的阻塞周期和无限的交换周期。

- `swap` ：为字体提供一个非常小的阻塞周期和无限的交换周期。

  > 💡 **关于 `swap` 的补充：**
  >
  > <font color=dodgerBlue>字体通常是大文件，需要一段时间才能加载，一些浏览器直到下载完字体后才呈现文本</font>
  >
  > <font color=fuchsia>`font-display: swap` 告诉浏览器默认使用系统字体进行渲染，当自定义字体下载完成之后再进行替换</font>
  >
  > ```css
  > @font-face {
  > font-family: 'Pacifico';
  > font-style: normal;
  > font-weight: 400;
  > src: local('Pacifico Regular'), local('Pacifico-Regular'), url(https://fonts.gstatic.com/xxx.woff2) format('woff2');
  > font-display: swap;
  > }
  > ```
  >
  > > ⚠️ 这里的 [`src`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face/src) CSS 属性中使用了 `local()` 函数，这是之前所忽略的，值得注意下；另外，`format()` 也是有印象，但是很不熟的。这里做下摘抄：
  > >
  > > > - `<url> [ format( <string># ) ]?` : Specifies an external reference consisting of a `<url>()`, followed by an optional hint using the 
  > > >
  > > >   **`format()`** function to describe the format of the font resource referenced by that URL. The format hint contains a comma-separated list of format strings that denote well-known font formats. If a user agent doesn't support the specified formats, it skips downloading the font resource. If no format hints are supplied, the font resource is always downloaded.
  > > >
  > > > - `<font-face-name>` : Specifies the name of a locally-installed font face using the 
  > > >
  > > >   **`local()`** function, which uniquely identifies a single font face within a larger family. The name can optionally be enclosed in quotes.
  > > >
  > > > 摘自：[MDN - `src`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face/src)
  >
  > 另外，你<font color=red>可以使用 `<link rel="preload">` 更早的加载字体文件</font>。
  >
  > 摘自：[解读新一代 Web 性能体验和质量指标](https://mp.weixin.qq.com/s/L90rB5JmJAOhmn7E2m8EDg)

- `fallback` ：为字体提供一个非常小的阻塞周期和短暂的交换周期。

- `optional ` ：为字体提供一个非常小的阻塞周期，并且没有交换周期。

##### 示例

```css
@ font-face {
  font-family: ExampleFont;
  src: url（/path/to/fonts/examplefont.woff）format（'woff'），
       url（/path/to/fonts/examplefont.eot）format（'eot'）;
  font-weight: 400;
  font-style: normal;
  font-display: fallback;
}
```

摘自：[MDN - `font-display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face/font-display)



#### 字体相关补充

- <font color=FF0000>**line-height CSS 属性**</font>用于设置多行元素的空间量，如多行文本的间距。<font color=FF0000>**对于块级元素，它指定元素行盒（line boxes）的最小高度。对于非替代的 inline 元素，它用于计算行盒（line box）的高度**</font>。

- **letter-spacing** 用于设置文本字符的间距表现。

##### 用 em 来设置字体大小

为了避免 Internet Explorer 中无法调整文本的问题，许多开发者使用 em 单位代替像素。em 的尺寸单位由 W3C 建议。1em 和当前字体大小相等。在浏览器中默认的文字大小是 16px。因此，1em 的默认大小是 16px。可以通过下面这个公式将像素转换为em：px/16=em



#### text-indent

text-indent 属性能定义一个块元素首行文本内容之前的缩进量。

##### 语法

```css
/* <length> 长度值 */
text-indent: 3mm;
text-indent: 40px;

/* <percentage>百分比值取决于其包含块（containing block）的宽度*/
text-indent: 15%;

/* 关键字 */
text-indent: 5em each-line;
text-indent: 5em hanging;
text-indent: 5em hanging each-line;

/* 全局值 */
text-indent: inherit;
text-indent: initial;
text-indent: unset;
```

###### 可选值

- `<length>` ：`使用固定的<length>值来指定文本的缩进。允许使用负值。查阅可能`的 [`<length>`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/length) 单位。
- `<percentage>` ：使用包含块宽度的百分比作为缩进。
- `each-line` ：🧪 文本缩进会影响第一行，以及使用 `<br>` 强制断行后的第一行。
- `hanging` ：🧪 该值会对所有的行进行反转缩进：除了第一行之外的所有的行都会被缩进，看起来就像第一行设置了一个负的缩进值

摘自：[MDN - text-indent](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-indent)



#### CSS链接

##### 四个链接状态

- **`a:link`** ：正常，未访问过的链接
- **`a:visited`** ：用户已访问过的链接
- **`a:hover`** ：当用户鼠标放在链接上时
- **`a:active`** ：链接被点击的那一刻

当设置为若干链路状态的样式，也有一些顺序规则：

- a:hover 必须跟在 a:link 和 a:visited 后面
- a:active 必须跟在 a:hover后面



#### CSS 轮廓

轮廓（outline）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

轮廓（outline）属性指定元素轮廓的样式、颜色和宽度。

<img src="https://s1.ax1x.com/2020/08/24/drTiRO.jpg" style="zoom:80%;" />



#### 分组选择器 & 嵌套选择器

##### 分组选择器

为了尽量减少代码，你可以使用分组选择器。

每个选择器用逗号分隔。

##### 嵌套选择器

在下面的例子设置了三个样式：

- **`p{ }`** : 为所有 `p` 元素指定一个样式。
- **`.marked{ }`** : 为所有 `class="marked"` 的元素指定一个样式。
- **`.marked p{ }`** : 为所有 `class="marked"` 元素内的 `p` 元素指定一个样式。
- **`p.marked{ }`** : 为所有 `class="marked"` 的 `p` 元素指定一个样式。



#### display:none & visibility:hidden

隐藏一个元素可以通过把display属性设置为"none"，或把visibility属性设置为 "hidden" 。这两种方法会产生不同的结果。

- visibility:hidden 可以隐藏某个元素，但<mark>隐藏的元素仍需占用与未隐藏之前一样的空间</mark>。也就是说，该元素虽然被隐藏了，但<mark>仍然会影响布局</mark>。

- display:none 可以隐藏某个元素，<mark>且隐藏的元素不会占用任何空间</mark>。也就是说，该元素不但被隐藏了，而且<font color=FF0000>该元素原本占用的空间也会从页面布局中消失</font>。



#### 块和内联元素

- **块元素**是一个元素，占用了全部宽度，<font color=FF0000>在前后都是换行符</font>。
  
  每个块级元素都从新的一行开始，并且其后的元素也另起一行。（很霸道，一个块级元素独占一行）
  
  <mark>元素的高度、宽度、行高以及顶和底边距都<font color=FF0000>**可设置**</font></mark>。
  
  <font color=FF0000>元素宽度在不设置的情况下，**是它本身父容器的100%**（和父元素的宽度一致），除非设定一个宽度</font>。
  
  **块元素有：\<h1>，\<p>，\<div>**

- **内联元素**只需要必要的宽度，<font color=FF0000>不强制换行</font>。
  
  和其他元素都在一行上；
  
  <mark><font color=FF0000>**元素的高度、宽度及顶部和底部边距不可设置**</font></mark>；
  
  <font color=FF0000>元素的宽度就是它包含的文字或图片的宽度，不可改变。</font>
  
  **内联元素有：\<span>，\<a>**

**使得元素变成块元素和内联元素**

- `li { display: inline }`：把列表项显示为内联元素
- `span { display: block }`：把span元素作为块元素

##### 补充：inline-block

<font color=dodgerBlue>简单来说</font>：就是将<font color=FF0000>**对象**呈现为 **inline** 对象</font>，但是<font color=FF0000>**对象的内容**作为 **block** 对象</font>呈现。之后的内联对象会被排列在同一行内。比如我们可以给一个 link（ a 元素）inline-block 属性值，使其既具有 block 的宽度高度特性又具有 inline 的同行特性。

内联块状元素 ( inline-block ) 就是同时具备内联元素、块状元素的特点。

<font color=dodgerBlue>inline-block 元素特点：</font>

1、和其他元素都在一行上

2、<font color=FF0000>元素的高度、宽度、行高以及顶和底边距都可设置</font>。

<font color=dodgerBlue> 另外</font>：IE（低版本 IE ）本来是不支持 inline-block 的，所以在 IE 中对内联元素使用 display: inline-block，理论上 IE 是不识别的，但使用 display: inline-block 在 IE 下会触发 layout，从而使内联元素拥有了 display: inline-block 属性的表象。

摘自：[block，inline和inline-block概念和区别](https://www.cnblogs.com/KeithWang/p/3139517.html)  [CSS： inline、block和inline-block的区别](https://www.cnblogs.com/adongyo/p/11290826.html)



#### CSS Position

元素可以使用的顶部，底部，左侧和右侧属性定位。然而，这些属性无法工作，除非是先设定 position 属性。他们也有不同的工作方式，这取决于定位方法。

##### position 属性的五个值

- **static**：HTML 元素的<font color=fuchsia>**默认值**</font>，即<font color=red>没有定位，遵循正常的文档流对象</font>。
  
  浏览器会按照源码的顺序，决定每个元素的位置，这称为 “正常的页面流” ( normal flow ) 。每个块级元素占据自己的区块 ( block )，元素与元素之间不产生重叠，这个位置就是元素的默认位置。
  
  > ⚠️ <font color=red>static 定位所导致的元素位置，是浏览器自主决定的</font>；所以 <font color=red>top、bottom、left、right 这四个属性无效</font>。

- **relative**：相对定位元素的定位是相对其正常位置。<font color=FF0000>**（相对于自身定位）**</font>
  
  relative 表示：<font color=red>相对于默认位置（即 static 时的位置）进行偏移</font>，即定位基点是元素的默认位置。
  
  它<font color=lightSeaGreen>必须搭配 top、bottom、left、right 这四个属性一起使用，用来指定偏移的方向和距离</font>。

- **fixed**：<font color=red>元素的位置相对于浏览器窗口是固定位置</font>。
  
  它如果搭配 top、bottom、left、right 这四个属性一起使用，表示元素的初始位置是基于视口计算的，否则初始位置就是元素的默认位置。

- **absolute**：<font color=fuchsia>**绝对定位的元素的位置相对于 <font size=4>最近的已定位的</font> 父元素**</font>（即：父元素带有 position 属性（一般是 relative ）。<font color=fuchsia>如果没有，absolute将相对于 body 移动）</font>，如果元素没有已定位的父元素，那么它的位置相对于 \<html>
  
  absolute 表示：相对于上级元素（一般是父元素）进行偏移，即 <font color=red>定位基点是父元素</font>。
  
  <font color=dodgerblue>它有一个重要的限制条件：</font><font color=red>定位基点（一般是父元素）不能是 static 定位，否则定位基点就会变成整个网页的根元素html</font>。另外，<font color=LightSeaGreen>absolute定位也必须搭配 top、bottom、left、right 这四个属性一起使用</font>。

- **sticky**：sticky 英文字面意思是粘，粘贴，所以可以把它称之为粘性定位。
  
  `position: sticky` <font color=fuchsia>基于用户的滚动位置来定位</font>。粘性定位的元素是依赖于用户的滚动，在 `position: relative` 与 `position: fixed` 定位之间切换。
  
  <font color=dodgerBlue>使用场景：</font>比如，网页的搜索工具栏；初始加载时在自己的默认位置（ relative 定位），页面向下滚动时，工具栏变成固定位置，始终停留在页面头部（ fixed 定位）。
  
  <font color=dodgerBlue>sticky **生效的前提**是：</font><font color=FF0000>必须搭配 top、bottom、left、right 这四个属性一起使用，不能省略</font>，否则等同于 relative 定位，不产生 “动态固定” 的效果。原因是这四个属性用来定义"偏移距离"，浏览器把它当作 sticky 的生效门槛。
  
  <font color=dodgerBlue>运行的具体规则是：</font><font color=fuchsia>当页面滚动，父元素开始脱离视口时（即部分不可见），只要与 sticky 元素的距离达到生效门槛，relative 定位自动切换为 fixed 定位</font>；<font color=FF0000>等到父元素完全脱离视口时（即完全不可见），fixed 定位自动切换回 relative 定位。</font>
  
  > ⚠️ 注意：除了已被淘汰的 IE 以外，其他浏览器目前都支持 sticky。但是，<font color=red>Safari 浏览器需要加上浏览器前缀 `-webkit-`</font>
  
  > 补充：可以看下 [黏性定位【渡一教育】](https://www.bilibili.com/video/BV1gG411v7n5)，如下是做的笔记

    一个元素的黏性布局位置会受到如下两个东西的影响：
    - 该元素的包含块 containing block，一般是它父元素的内容盒（可以简单理解为父元素），虽然也有其他情况
    - 该元素的 **最近可滚动祖先**：向上层寻找 `overflow` 属性为 `scroll` 或者为 `auto` 的祖先（只要不是 `visible`），如果找不到即可能为视口 viewport 。一般情况下，该元素和一般元素没什么区别，但是，一旦该元素和它 “最近可滚动祖先” 的位置达到了设置的位置的时候，它的位置将会被固定下来；同时，这种位置的固定不会影响其他元素的排列：虽然 sticky 生效时，该元素被固定，但是不会因为该元素被固定，其他元素占据目标元素的位置（好像有一个 placeholder 占住了原先在 “最近可滚动祖先” 中的为止一样）。当“最近可滚动祖先” 脱离视口时，sticky 也会失效，目标元素也会消失

部分摘自：[阮一峰的网络日志 - CSS 定位详解](https://www.ruanyifeng.com/blog/2019/11/css-position.html)

> 👀 这部分内容可以参考 [[前端面试点总结#定位]]

#### inset

CSS 属性 **`inset`** 为<font color=red>**简写属性**，对应于 `top`、`right`、`bottom` 和 `left` 属性</font>。其与 `margin` 简写属性具有相同的多值语法。

> 👀 后面还有更多内容，不过感觉没必要摘抄

##### 示例

```html
<div>
  <span class="exampleText">示例文本</span>
</div>

<style>
div {
  background-color: yellow;
  width: 150px;
  height: 120px;
  position: relative;
}

.exampleText {
  writing-mode: sideways-rl;
  position: absolute;
  inset: 20px 40px 30px 10px;
  background-color: #c8c800;
}
</style>
```

摘自：[MDN - inset](https://developer.mozilla.org/zh-CN/docs/Web/CSS/inset)



#### 重叠的元素

元素的定位与文档流无关，所以它们可以覆盖页面上的其它元素。<font color=red>z-index 属性指定了一个元素的堆叠顺序（哪个元素应该放在前面，或后面）</font>。一个元素可以有正数或负数的堆叠顺序，<font color=lightSeaGreen>具有更高堆叠顺序的元素总是在较低的堆叠顺序元素的前面</font>。



#### CSS Float

CSS 的 Float（浮动），会使元素向左或向右移动，其周围的元素也会重新排列。

<font color=red>Float（浮动），**往往是用于图像**</font>，但它在布局时一样非常有用。

```css
float: right;
```

##### 清除浮动 - 使用 clear

元素浮动之后，周围的元素会重新排列；为了避免这种情况，使用 clear 属性

**示例：**

```css
.text_line {
    clear: both;
}
```



#### 水平 & 垂直对齐

##### 文本居中对齐

```css
text-align: center;
```

##### 居中对齐

```css
margin: auto;
```

另外，需要注意的是：对于行内块级元素“inline-block” ，想要用`margin: auto;` 实现居中对齐，需要加上`display: block`。比如：`<button>`

> 💡 这里居中对齐是 `margin: auto` 相当于 `margin: auto auto auto auto` ，但是为什么 垂直方向没有居中？同时，使用 flex box / gridbox 也就可以了，为什么？
>
> 对于这个问题： `margin[-(left, right)]: auto` 的 居中 或 居左 居右，《css权威指南》中有说。但是，没提到垂直方向的。在看了 [为什么「margin:auto」可以让块级元素水平居中？ - 杜瑶的回答 - 知乎](https://www.zhihu.com/question/21644198/answer/18895538)（如下图）感觉有点理解了。我的理解是：html 默认的渲染方向 ( writing-mode ) 是水平的，无法参考垂直的。而是用来 flexbox / gridbox 就不得不参考垂直的了。
>
> <img src="https://s2.loli.net/2022/09/05/waQs3Fby8PldNjE.png" alt="image-20220905174516397" style="zoom:55%;" />

##### 文本垂直居中

- 对于一行文本，可以使用 line-height === height，来实现 文本垂直居中。
- 对于多行文本，可以通过使用 `vertical-align: middle; display: table-cell` 实现。

##### 元素水平垂直居中对齐

见 [CSS 拷问：水平垂直居中方法你会几种？](https://liuyib.github.io/2020/04/07/css-h-and-v-center/) 其中答案相当全面，除了 flexbox、gridbox、position+transform、margin: auto 等写法；还有<font color=dodgerBlue>**值得注意的是如下的写法**</font>：

> ```html
> <div class="outer">
>   <div class="inner"></div>
> </div>
> 
> <style>
>   .outer {
>     position: relative;
>     width: 400px; height: 400px; background-color: lightblue;
>   }
>   .inner {
>     position: absolute;
>     top: 0; left: 0; right: 0; bottom: 0; // 👀
>     margin: auto;                         // 👀
>     width: 200px; height: 200px; background-color: lightcoral;
>   }
> </style>
> ```
>
> 该方案的原理是：使用了 CSS 中的定位属性（`absolute`、`fixed` 等）后，如果 `left` 设置了具体值，没有设置 `right` 和 `width`，那么就会自动计算，把剩余的空间分配给 `right` 和 `width`。如果 `left`、`right` 和 `width` 都设置了具体值，并且没有占满横向空间，那么剩余空间就处于待分配状态，此时设置 `margin: auto;` 意味着把剩余的空间分配给 `margin`，并且左右均分，所以就实现了水平居中，垂直方向同理。
>
> <font color=dodgerblue>但是要知道该方法的副作用：</font>
>
> - <font color=red>`left: 0; right: 0;` 相当于 `width: 100%;`</font>
> - <font color=red>`top: 0; bottom: 0;` 相当于 `height: 100%;`</font>
>
> **缺点**：<font color=red>需要固定居中元素的宽高，否则其宽高会被设为 `100%`（副作用）</font>。



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



#### CSS 伪类 ( Pseudo-classes )

添加到选择器的关键字，<mark>指定要选择的元素的特殊状态</mark>。

例如，`:hover `可被用于在用户将鼠标悬停在按钮上时改变按钮的颜色。

```css
/* 所有用户指针悬停的按钮 */
button:hover {
  color: blue;
}
```

伪类连同伪元素一起，他们允许你不仅仅是根据文档 DOM 树中的内容对元素应用样式，而且还允许你根据诸如像导航历史这样的外部因素来应用样式（例如 `:visited`），同样的，可以根据内容的状态（例如在一些表单元素上的 `:checked`），或者鼠标的位置（例如 `:hover` 让你知道是否鼠标在一个元素上悬浮）来应用样式。

##### 语法

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

下例 `::first-line` 伪元素可改变段落首行文字的样式。

```css
/* 每一个 <p> 元素的第一行。 */
p::first-line {
  color: blue;
  text-transform: uppercase;
}
```

##### 语法

```css
selector::pseudo-element {
  property: value;
}
```

**所有CSS伪类/元素**

| 选择器              | 示例              | 示例说明                                          |
| :------------------ | :---------------- | :------------------------------------------------ |
| `:link`             | `a:link`          | 选择所有未访问链接                                |
| `:visited`          | `a:visited`       | 选择所有访问过的链接                              |
| `:active`           | `a:active`        | 选择正在活动链接                                  |
| `:hover`            | `a:hover`         | 把鼠标放在链接上的状态                            |
| `:focus`            | `input:focus`     | 选择元素输入后具有焦点                            |
| `::first-letter`    | `p::first-letter` | 选择每个 `<p>` 元素的第一个字母                   |
| `::first-line`      | `p::first-line`   | 选择每个 `<p>` 元素的第一行                       |
| `::first-child`     | `p::first-child`  | 选择器匹配属于任意元素的第一个子元素的 `<p>` 元素 |
| `::before`          | `p::before`       | 在每个 `<p>`元素之前插入内容                      |
| `::after`           | `p::after`        | 在每个 `<p>`元素之后插入内容                      |
| `:lang(*language*)` | `p:lang(it)`      | 为 `<p>` 元素的 `lang` 属性选择一个开始值         |

以上部分摘自：[MDN - 伪元素](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-elements) ，不过上面的摘录很不全面。在前面 [MDN 的链接](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-elements) 中，可以找到更多被整理好的伪类，不过 MDN 并没有将其展示在文档中，而是排列在文档左边的目录中...（我也是在搜索 `:focus-within` 时找到的，可以通过搜索该伪类，其找到所在位置） 

> 💡 相当值得⚠️注意的点是：
>
> `input` 元素是不支持 `::before` 和 `::after` 的，加上了也不会按照预期显示。
>
> 另外，可以参考 [为什么input不支持伪元素(:after,:before)？ - 知乎](https://www.zhihu.com/question/21296044)

##### `:focus-within`

<font color=red>**`:focus-within`** CSS 伪类表示当元素或其任意后代元素被聚焦时，将匹配该元素</font>。换言之，它表示 `:focus` 伪类匹配到该元素自身或它的后代时，该伪类生效（这也包括 shadow 树中的后代元素）。

摘自：[MDN - `:focus-within`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:focus-within)



#### CSS 图像透明度

CSS3中属性的透明度是 `opacity`，示例：

```css
opacity: .4;
```



#### filter

**filter** CSS 属性<font color=FF0000>将模糊或颜色偏移等图形效果应用于元素</font>。<font color=FF0000>滤镜通常用于调整图像，背景和边框的渲染。</font>

<font color=dodgerBlue>CSS 标准里包含了一些已实现预定义效果的函数</font>。你也可以参考一个 SVG 滤镜，通过一个 URL 链接到 SVG 滤镜元素 ( SVG filter element )

##### 示例

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

/* Multiple filters */
filter: contrast(175%) brightness(3%);

/* Use no filter */
filter: none;

/* Global values */
filter: inherit;
filter: initial;
filter: unset;
```

摘自：[MDN - filter](https://developer.mozilla.org/zh-CN/docs/Web/CSS/filter)

> 👀 由于MDN 中说的并不是很清楚，所以又去阅读了：[CSS filter 有哪些神奇用途](https://segmentfault.com/a/1190000040058430)

##### 基本概念

CSS filter 属性将模糊或颜色偏移等图形效果应用于元素形成滤镜，<font color=FF0000>滤镜通常用于调整图像，背景和边框的渲染</font>。它的<font color=FF0000>值可以为 **filter 函数** `<filter-function>`</font>（👀 如下面的代码 第一行）<font color=FF0000> 或 **使用 url 添加的 svg 滤镜**</font>（ 👀 如下面的代码 第二行）。

```css
filter: <filter-function> [<filter-function>]* | none
filter: url(file.svg#filter-element-id)
```

> 👀 无论是摘抄的文章，还是 [MDN - filter](https://developer.mozilla.org/zh-CN/docs/Web/CSS/filter) 中都没有 [MDN - backdrop-filter](https://developer.mozilla.org/zh-CN/docs/Web/CSS/backdrop-filter) 说的清楚（下面有引用，也可见 [[#backdrop-filter#语法]]）。这两者 `filter` 是作用于前面的区域，`backdrop-filter` 作用于后面区域。

###### 可选值

- **`none`** ：没有应用于背景的滤镜。
- **`<filter-function-list>`** ：一个以空格分隔的滤镜函数 ( `<filter-function>` ) 或是要应用到背景上的 SVG 滤镜。

\<filter-function> 可以用于 filter 和 backdrop-filter（👀 详见下面 [[#backdrop-filter]]） 属性。它的数据类型由下列过滤器函数之一指定。每个函数需要一个参数，如果参数无效，则滤镜不会生效。以下是对滤镜函数含义的解释：

- **blur()：**<font color=FF0000>模糊</font>图像
- **brightness() ：**让图像<font color=FF0000>更明亮或更暗淡</font>
- **contrast()：**<font color=FF0000>增加或减少</font>图像的<font color=FF0000>对比度</font>
- **drop-shadow()：**在<font color=FF0000>图像后方应用投影</font>
- **grayscale()：**将图像<font color=FF0000>转为灰度图</font>
- **hue-rotate()：**<font color=FF0000>改变</font>图像的<font color=FF0000>整体色调</font>
- **invert()：**<font color=FF0000>反转</font>图像<font color=FF0000>颜色</font>
- **opacity()：**<font color=FF0000>改变</font>图像<font color=FF0000>透明度</font>
- **saturate()：**<font color=FF0000>**超饱和**</font> 或 <font color=FF0000>**去饱和** 输入的图像</font>
- **sepia()：**将图像<font color=FF0000>转为棕褐色</font>

摘自：[CSS filter 有哪些神奇用途](https://segmentfault.com/a/1190000040058430) 另外，该博客中还有相关使用示例的介绍，这里由于篇幅省略；可以看看，讲得很清楚。



####  backdrop-filter

backdrop-filter CSS 属性可以让你 <font color=FF0000>为一个 元素 <font size=4>**后面区域**</font> 添加图形效果</font>（如模糊或颜色偏移）。因为它适用于元素背后的所有元素，为了看到效果，必须使元素或其背景至少部分透明。

##### 语法

可选值

- **`none`** ：没有应用于背景的滤镜。
- **`<filter-function-list>`** ：一个以空格分隔的滤镜函数 ( `<filter-function>` ) 或是要应用到背景上的 SVG 滤镜。

##### 形式化定义

| 初始值         | `none`                                                       |
| :------------- | ------------------------------------------------------------ |
| 适用元素       | all elements; In SVG, it applies to container elements excluding the [`defs`](https://developer.mozilla.org/zh-CN/docs/Web/SVG/Element/defs) element and all graphics elements |
| 是否是继承属性 | 否                                                           |
| 计算值         | as specified                                                 |
| Animation type | a [filter function list](https://developer.mozilla.org/en-US/docs/Web/CSS/filter#interpolation) |

摘自：[MDN - backdrop-filter](https://developer.mozilla.org/zh-CN/docs/Web/CSS/backdrop-filter)



#### CSS 媒体类型

**媒介类型(Media Types)允许你定义以何种媒介来提交文档。文档可以被显示在显示器、纸媒介或者听觉浏览器等等。**

##### @media 规则

@media 规则使你有能力在相同的样式表中，使用不同的样式规则来针对不同的媒介。

| 媒体类型       | 描述                          |
|:---------- |:--------------------------- |
| all        | 用于所有的媒体设备。                  |
| aural      | 用于语音和音频合成器。                 |
| braille    | 用于盲人用点字法触觉回馈设备。             |
| embossed   | 用于分页的盲人用点字法打印机。             |
| handheld   | 用于小的手持的设备。                  |
| print      | 用于打印机。                      |
| projection | 用于方案展示，比如幻灯片。               |
| screen     | 用于电脑显示器。                    |
| tty        | 用于使用固定密度字母栅格的媒体，比如电传打字机和终端。 |
| tv         | 用于电视机类型的设备。                 |



#### 属性选择器

CSS **属性选择器**<font color=red>通过已经存在的属性名或属性值匹配元素</font>。

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



#### table-layout

table-layout CSS属性定义了用于布局表格单元格，行和列的算法。

- **auto：**<font color=FF0000>（默认值）大多数浏览器采用自动表格布局算法对表格布局。表格及单元格的宽度取决于其包含的内容</font>。

- **fixed：**<font color=FF0000>表格和列的宽度通过表格的宽度来设置，某一列的宽度仅由该列首行的单元格决定</font>。在当前列中，该单元格所在行之后的行并不会影响整个列宽。

  使用 “fixed” 布局方式时，整个表格可以在其首行被下载后就被解析和渲染。这样对于 “automatic” 自动布局方式来说可以加速渲染，但是其后的单元格内容并不会自适应当前列宽。任何一个包含溢出内容的单元格可以使用 overflow  属性控制是否允许内容溢出。

摘自：[MDN - table-layout](https://developer.mozilla.org/zh-CN/docs/Web/CSS/table-layout)



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



#### CSS3 模块

- 选择器
- 盒模型
- 背景和边框
- 文字特效
- 2D/3D转换
- 动画
- 多列布局
- 用户界面



#### box-shadow

盒阴影。可以在同一个元素上设置多个阴影效果，并用逗号将他们分隔开。该属性 <font color=FF0000>可设置的值包括阴影的 **X轴偏移量**、**Y轴偏移量**、**模糊半径**、**扩散半径** 和 **颜色**</font>。

##### 向元素添加单个 box-shadow 效果时使用以下规则

- 当给出两个、三个或四个 \<length> 值时
  - 如果只给出两个值, 那么这两个值将会被当作 \<offset-x> \<offset-y> 来解释。
  - 如果给出了第三个值, 那么第三个值将会被当作 \<blur-radius>解释。
  - 如果给出了第四个值, 那么第四个值将会被当作 \<spread-radius>来解释。
- 可选，inset关键字。
- 可选，\<color>值。

若要对同一个元素添加多个阴影效果，请使用逗号将每个阴影规则分隔开。

##### 取值

###### inset

<font color=FF0000>如果没有指定inset，默认阴影在边框外，即阴影向外扩散</font>。 <font color=FF0000>使用 inset 关键字会使得阴影落在盒子内部，这样看起来就像是内容被压低了</font>。 此时阴影会在边框之内 (即使是透明边框）、背景之上、内容之下。

###### `<offset-x>` `<offset-y>`

这是头两个 `<length>` 值，用来设置阴影偏移量。x , y 是按照数学二维坐标系来计算的，只不过 y 垂直方向向下。 `<offset-x>` 设置水平偏移量，<font color=LightSeaGreen>正值影则位于元素右边，负值阴影则位于元素左边</font>。 `<offset-y>` 设置垂直偏移量，<font color=LightSeaGreen>正值阴影则位于元素下方，负值阴影则位于元素上方</font>。可用单位请查看 `<length>` 。

如果两者都是0，那么阴影位于元素后面。这时如果设置了\<blur-radius> 或\<spread-radius> 则有模糊效果。需要考虑 inset 

###### `<blur-radius>`  阴影模糊半径 

这是第三个 `<length>` 值。值越大，模糊面积越大，阴影就越大越淡。 不能为负值。默认为 0，此时阴影边缘锐利。本规范不包括如何计算模糊半径的精确算法，但是，它详细说明如下：

> 对于长而直的阴影边缘，它会创建一个过渡颜色用于模糊 以阴影边缘为中心、模糊半径为半径的局域，过渡颜色的范围在完整的阴影颜色到它最外面的终点的透明之间。 （译者注：对此有兴趣的可以了解下数字图像处理的模糊算法。）

###### `<spread-radius>` 阴影扩散半径

这是第四个 `<length>` 值。取正值时，阴影扩大；取负值时，阴影收缩。默认为 0，此时阴影与元素同样大。需要考虑 inset 

###### `<color>` 阴影颜色

相关事项查看 `<length>` 。如果没有指定，则由浏览器决定——通常是color的值，不过目前 Safari 取透明。

##### box-shadow 工具

MDN 提供了一个在线工具： [Box-shadow generator](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Backgrounds_and_Borders/Box-shadow_generator)  ，可以以可视化的方式编辑，并生成 CSS 代码。

摘自：[MDN - box-shadow](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-shadow)

> 💡 text-shadow 用法和 box-shadow 一样，是用来对于文字进行设置阴影

#### border-image

边界图片

> 👀 下面是一点补充

#### border-collapse

`border-collapse` CSS 属性是用来决定表格的边框是分开的还是合并的。在分隔模式下，相邻的单元格都拥有独立的边框。在合并模式下，相邻单元格共享边框。

- 合并 ( *collapsed*  ) 模式下，表格中相邻单元格共享边框。在这种模式下，CSS 属性 `border-style` 的值 inset 表现为槽，值 outset 表现为脊。

- 分隔 ( *separated* ) 模式是 HTML 表格的传统模式。相邻单元格都拥有不同的边框。边框之间的距离是通过 CSS 属性 `border-spacing` 来确定的。

##### 语法

```css
/* Keyword values */
border-collapse: collapse;
border-collapse: separate;

/* Global values */
border-collapse: inherit;
border-collapse: initial;
border-collapse: unset;
```

`border-collapse` 的属性值被定义为一个单独的关键词，可为下面两个值中的一个。

###### 值

- `collapse`：相邻的单元格共用同一条边框（采用 collapsed-border 表格渲染模型）。
- `separate`：默认值。每个单元格拥有独立的边框（采用 separated-border 表格渲染模型）。

摘自：[MDN - border-collapse](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-collapse)



#### CSS 基本数据类型

<font color=FF0000>CSS 基本数据类型是 组件值类型( component value type ) 的一种</font>。用于定义 CSS属性和函数可以接受的变量（关键字和单位）的种类。

数据类型由放置在不等式符号 "<" 和 ">" 之间的关键字表示：

- \<angle>
- \<basic-shape>
- \<blend-mode>
- \<color>
- \<custom-ident>
- \<filter-function>
- \<flex>
- \<frequency>
- \<gradient>
- \<image>
- \<integer>
- \<length>
- \<number>
- \<percentage>
- \<position>
- \<ratio>
- \<resolution>
- \<shape-box>
- \<single-transition-timing-function>
- \<string>
- \<time>
- \<transform-function>
- \<url>

摘自：[MDN - CSS 基本数据类型](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Types)



#### clip-path

clip-path CSS 属性<font color=FF0000>使用裁剪方式创建元素的可显示区域</font>。区域内的部分显示，区域外的隐藏。

##### 取值

- **\<clip-source>：**用 \<url> 表示剪切元素的路径

- **\<basic-shape>：**一种形状，其大小和位置由<几何盒>值定义。如果没有指定几何框，则边框将用作参考框

  **补充：**

  <font color=FF0000>\<basic-shape>是一种表现基础图形的CSS数据类型，作用于clip-path 与 shape-outside 属性中。</font>

  \<basic-shape>数据类型可以由函数得到。<mark>当构建一个图形时，运用了\<basic-shape>值的属性就会定义一个相关的盒模型。</mark><font color=FF0000>基础图形使用的坐标系统即设置相关的盒模型**左上角顶点为原点**，该坐标轴的x轴正方向为右、y轴的正方向为下。所有以百分比定义的长度将通过相关盒模型与使用的维度重定义。</font>

  \<basic-shape>数据类型可以由下列<font color=FF0000>**图形函数**</font>得到：

  - **inset()：**以下函数定义了一个插进的长方形。

    ```css
    inset( <shape-arg>{1,4} [round <border-radius>]? )
    ```

    上式的**前四个参数**分别代表了插进的长方形与相关盒模型的上，右，下与左边界和顶点的偏移量。这些参数遵循边际速记语法（the syntax of the margin shorthand），所以给予一个、两个、或四个值都能设置四个偏移量。<font color=FF0000>（解释：即矩形到上边、右边、左边、下边到外容器的上边、右边、左边、下边的距离）</font>

    可选参数\<border-radius>用于定义插进长方形顶点的圆弧角度，该参数同上遵循边际速记语法（the syntax of the margin shorthand），给予一个、两个、或四个值都能设置四个偏移量。

    如果一对插进图形在通过堆叠产生的高于当前使用维度的维度中（例如，左右插进图像相叠75%）将会定义一个包围不了任何区域的图形。这种情况会在元素中产生一个空白且平坦的区域。

  - **circle()：**

    ```css
    circle( [<shape-radius>]? [at <position>]? )
    ```

    \<shape-radius> 参数代表了圆形的半径(r)，不接受负数作为该参数的值。一个以百分比表示的值将以公式 sqrt(width^2^ +height^2^) / sqrt(2)计算，其中width与height为相关盒模型的宽与高。

    \<position> 参数定义了圆心的位置。省缺值为盒模型的中心。

  - **ellipse()：**

    ```css
    ellipse( [<shape-radius>{2}]? [at <position>]? )
    ```

    \<shape-radius> 参数代表了 r~x~ 与 r~y~，<font color=FF0000>其中 r~x~ 代表了x轴方向的半径， r~y~代表了y轴方向的半径</font>。该参数不接受负数值。以百分比表示的长度将把盒模型的宽与高作为参照，宽作为 r~x~的参照值，高作为 r~y~ 的参照值。

    \<font color=FF0000>\<position>参数定义了椭圆形圆心的位子。其省缺值为盒模型的中心。</font>

  - **polygon()：**

    ```css
    polygon( [<fill-rule>,]? [<shape-arg> <shape-arg>]# )=
    ```

    \<fill-rule> 代表了填充规则（ filling rule ），即，如何填充该多边形。 可选值为 nonzero 和 evenodd。 该参数的省缺值为 nonzero。

    每一对在列表中的参数都代表了多边形顶点的坐标， x~i~ 与 y~i~ ，i代表顶点的编号，即，第i个顶点。（每个逗号分隔一个点，顺时针形成一个多边形）

  - **path()：**实验属性

    ```css
    path( [<fill-rule>,]? <string>)
    ```

    可选的 \<fill-rule> 表示 filling rule 填充规则. 可选 nonzero （非零环绕规则）和 evenodd （奇偶规则） . 默认是（Default value when omitted） nonzero.

    参数 \<string> 是用引号包含的 SVG Path 字符串

  摘自：[MDN - \<basic-shape>](https://developer.mozilla.org/zh-CN/docs/Web/CSS/basic-shape)

- **\<geometry-box>：**如果同 \<basic-shape> 一起声明，它将为基本形状提供相应的参考框盒。通过自定义，它将利用确定的盒子边缘包括任何形状边角（比如说，被 border-radius 定义的剪切路径）。几何框盒可以有以下的值中的一个：

  - **margin-box：**使用 margin box 作为引用框。
  - **border-box：**使用 border box 作为引用框。
  - **padding-box：**使用 padding box 作为引用框。
  - **content-box：**使用 content box 作为引用框。
  - **fill-box：**利用对象边界框作为引用框。
  - **stroke-box：**使用笔触边界框（stroke bounding box）作为引用框
  - **view-box：**使用最近的 SVG 视口（viewport）作为引用框。如果viewBox 属性被指定来为元素创建 SVG 视口，引用框将会被定位在坐标系的原点，引用框位于由 viewBox 属性建立的坐标系的原点，引用框的尺寸用来设置 viewBox 属性的宽高值。

- **none：**不创建的剪切路径。

摘自：[MDN - clip-path](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clip-path)



#### CSS3 渐变

CSS3 渐变（gradients）可以让你在两个或多个指定的颜色之间显示平稳的过渡。

CSS3 定义了两种类型的渐变 ( gradients )：

##### 线性渐变 ( Linear Gradients )

向下/向上/向左/向右/对角方向

###### 语法

```css
background-image: linear-gradient(direction, color-stop1, color-stop2, ...);
```

**可以使用预定义方向**（to bottom、to top、to right、to left、to bottom right，等等）

###### 示例

```css
/*演示了从左上角开始（到右下角）的线性渐变。起点是红色，慢慢过渡到黄色：*/
#grad {
  height: 200px;
  background-image: linear-gradient(to bottom right, red, yellow);
}
```

##### 使用角度

如果你想要在渐变的方向上做更多的控制，你可以定义一个角度，而不用预定义方向（to bottom、to top、to right、to left、to bottom right，等等）。

##### 语法

```css
background-image: linear-gradient(angle, color-stop1, color-stop2);
```

角度是指水平线和渐变线之间的角度，逆时针方向计算。换句话说，0deg 将创建一个从下到上的渐变，90deg 将创建一个从左到右的渐变。

<img src="https://s2.loli.net/2023/05/06/Cm8ynsJVozONS1G.jpg" alt="img" style="zoom:50%;" />

###### 使用多个颜色节点

示例：

```css
/*带有多个颜色节点的从上到下的线性渐变*/
#grad {
  background-image: linear-gradient(red, yellow, green);
}
```

###### 使用透明度 ( transparent )

`rgba()` 函数中的最后一个参数可以是从 0 到 1 的值，它定义了颜色的透明度：0 表示完全透明，1 表示完全不透明。

```css
#grad {
  background-image: linear-gradient(to right, rgba(255,0,0,0), rgba(255,0,0,1));
}
```

###### 径向渐变 ( Radial Gradients )

由它们的中心定义

径向渐变由它的中心定义。

为了创建一个径向渐变，你也必须至少定义两种颜色节点。颜色节点即你想要呈现平稳过渡的颜色。同时，你也可以指定渐变的中心、形状（圆形或椭圆形）、大小。默认情况下，渐变的中心是 center（表示在中心点），渐变的形状是 ellipse（表示椭圆形），渐变的大小是 farthest-corner（表示到最远的角落）。



#### CSS3 文本属性

| 属性                | 描述                                                    | CSS  |
| :------------------ | :------------------------------------------------------ | :--: |
| hanging-punctuation | 规定标点字符是否位于线框之外。                          |  3   |
| punctuation-trim    | 规定是否对标点字符进行修剪。                            |  3   |
| text-align-last     | 设置如何对齐最后一行或紧挨着强制换行符之前的行。        |  3   |
| text-emphasis       | 向元素的文本应用重点标记以及重点标记的前景色。          |  3   |
| text-justify        | 规定当 text-align 设置为 "justify" 时所使用的对齐方法。 |  3   |
| text-outline        | 规定文本的轮廓。                                        |  3   |
| text-overflow       | 规定当文本溢出包含元素时发生的事情。                    |  3   |
| text-shadow         | 向文本添加阴影。                                        |  3   |
| text-wrap           | 规定文本的换行规则。                                    |  3   |
| word-break          | 规定非中日韩文本的换行规则。                            |  3   |
| word-wrap           | 允许对长的不可分割的单词进行分割并换行到下一行。        |  3   |

#### word-break

 CSS 属性 word-break 指定了怎样在单词内断行。

**可选值**

- **normal：**使用默认的断行规则。
- **break-all：**<font color=FF0000>对于**非CJK (CJK 指中文/日文/韩文) 文本**，可在**任意字符间断行**</font>。
- **keep-all：**CJK 文本不断行。 Non-CJK 文本表现同 normal。
- **break-word ：（<font color=FF0000>废弃</font>）**他的效果是word-break: normal 和 overflow-wrap: anywhere  的合，不论 overflow-wrap的值是多少。

摘自：[MDN - word-break](https://developer.mozilla.org/zh-CN/docs/Web/CSS/word-break)

#### overflow-wrap

CSS 属性 overflow-wrap 是用来说明当一个不能被分开的字符串太长而不能填充其包裹盒时，为防止其溢出，浏览器是否允许这样的单词中断换行。

**值**

- **normal：**行只能在正常的单词断点处中断。（例如两个单词之间的空格）。
- **break-word：**表示如果行内没有多余的地方容纳该单词到结尾，则那些正常的不能被分割的单词会被强制分割换行。

<font color=fuchsia>**与 word-break 相比，overflow-wrap 仅在无法将整个单词放在自己的行，而不会溢出的情况下才会产生中断**</font>。

> 💡 备注：<font color=FF0000>`word-wrap` 属性原本属于微软的一个私有属性，在 CSS3 现在的文本规范草案中已经被重名为 `overflow-wrap`</font> 。 `word-wrap` 现在被当作 `overflow-wrap` 的 “别名”。 稳定的谷歌 Chrome 和 Opera 浏览器版本支持这种新语法。

摘自：[MDN - overflow-wrap](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow-wrap)



#### CSS3 字体

您所选择的字体在新的 **CSS3** 版本有关于 **`@font-face`** 规则描述。您"自己的"的字体是在 **CSS3 `@font-face`** 规则中定义的。

##### 示例

```css
@font-face
{
    font-family: myFirstFont;
    src: url(sansation_light.woff);
}
```

所有的字体描述和里面的 `@font-face` 规则定义：

| 描述符           | 值                                                                                                                                                    | 描述                                        |
|:-------------:|:---------------------------------------------------------------------------------------------------------------------------------------------------- |:-----------------------------------------:|
| font-family   | *name*                                                                                                                                               | 必需。规定字体的名称。                               |
| src           | *URL*                                                                                                                                                | 必需。定义字体文件的 URL。                           |
| font-stretch  | normal<br/>condensed<br/>ultra-condensed<br/>extra-condensed<br/>semi-condensed<br/>expanded<br/>semi-expanded<br/>extra-expanded<br/>ultra-expanded | 可选。定义如何拉伸字体。默认是 "normal"。                 |
| font-style    | normalitalicoblique                                                                                                                                  | 可选。定义字体的样式。默认是 "normal"。                  |
| font-weight   | normal<br/>bold<br/>100<br/>200<br/>300<br/>400<br/>500<br/>600<br/>700<br/>800<br/>900                                                              | 可选。定义字体的粗细。默认是 "normal"。                  |
| unicode-range | *unicode-range*                                                                                                                                      | 可选。定义字体支持的 UNICODE 字符范围。默认是 "U+0-10FFFF"。 |



#### text-orientation

text-orientation CSS 属性<font color=FF0000>设定行中字符的方向</font>。但<font color=FF0000>它仅影响纵向模式（当 `writing-mode` 的值不是`horizontal-tb` ）下的文本</font>。此属性在控制使用竖排文字的语言的显示上很有作用，也可以用来构建垂直的表格头。

##### 关键值

- **mixed** ：<font color=FF0000>默认值</font>。<font color=FF0000>顺时针旋转水平书写的字符90°</font>，将垂直书写的文字自然布局。
- **upright** ：<font color=FF0000>将水平书写的字符自然布局（直排）</font>，包括垂直书写的文字（as well as the glyphs for vertical scripts）。注意这个关键字会导致所有字符被视为从左到右，也就是 direction 被强制设为 ltr。
- **sideways** ：<font color=FF0000>所有字符被布局为与水平方式一样，但是整行文本被顺时针旋转90°</font>。
- **sideways-right** ：处于兼容目的，sideways 的别名。
- **use-glyph-orientation** ：对于 SVG 元素，这个关键字导致使用已弃用的 SVG 属性 glyph-orientation-vertical 和 glyph-orientation-horizontal。

##### 全局值

- inherit
- initial
- unset

摘自：[MDN - text-orientation](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-orientation)

#### writing-mode

<font color=FF0000>writing-mode 属性定义了文本水平或垂直排布以及在块级元素中文本的行进方向</font>。为整个文档设置时，应在根元素上设置它（对于 HTML 文档应该在 html 元素上设置）
<font color=FF0000>此属性指定块流动方向，即块级容器堆叠的方向，以及行内内容在块级容器中的流动方向</font>。因此，它也确定块级内容的顺序。

##### 关键值

- **horizontal-tb** ：对于左对齐 ( ltr ) 脚本，内容从左到右水平流动。对于右对齐 ( rtr ) 脚本，内容从右到左水平流动。下一水平行位于上一行下方。
- **vertical-rl** ：对于左对齐 ( ltr ) 脚本，内容从上到下垂直流动，下一垂直行位于上一行左侧。对于右对齐 ( rtr ) 脚本，内容从下到上垂直流动，下一垂直行位于上一行右侧。
- **vertical-lr：** 对于左对齐 ( ltr ) 脚本，内容从上到下垂直流动，下一垂直行位于上一行右侧。对于右对齐 ( rtr ) 脚本，内容从下到上垂直流动，下一垂直行位于上一行左侧。
- **sideways-rl** ：🧪 对于左对齐 ( ltr ) 脚本，内容从下到上垂直流动。对于右对齐 ( rtr ) 脚本，内容从上到下垂直流动。所有字形（即使是垂直脚本中的字形）都朝向右侧。
- **sideways-lr** ：🧪 对于左对齐 ( ltr ) 脚本，内容从上到下垂直流动。对于右对齐 ( rtr ) 脚本，内容从下到上垂直流动。所有字形（即使是垂直脚本中的字形）都朝向左侧。

##### 全局值

- inherit
- initial
- unset

摘自：[MDN - writing-mode](https://developer.mozilla.org/zh-CN/docs/Web/CSS/writing-mode)

> 👀 `text-orientation` 一般与 `writing-mode` 配合使用，同时一般使用 `writing-mode` 大概率要使用 `text-orientation`。如下示例：

```html
<p class="text">foo 123 你好！</p>
<style>
.text {
  writing-mode: vertical-rl;
}
</style>
```

效果如下，同时也是不合适的：

<img src="https://s2.loli.net/2022/02/08/fgdnSolbc5GTC9E.png" alt="image-20220208144748538" style="zoom:50%;" />

如果加上 `text-orientation` 即：

```css
.text {
  writing-mode: vertical-rl;
  text-orientation: upright;
}
```

效果如下，这显然是比较合适的：

<img src="https://s2.loli.net/2022/02/08/5qkHnlWOLwDIVuZ.png" alt="image-20220208145023666" style="zoom:50%;" />



#### direction

CSS 属性 `direction` 用来<font color=red>设置文本、表列水平溢出的方向</font>。 `rtl` 表示从右到左 (类似希伯来语或阿拉伯语)， `ltr` 表示从左到右（类似英语等大部分语言）

值得注意的是文本方向通常由文档定义（比如：[dir - html 全局属性](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/dir) ）而不是通过直接使用 direction 属性定义。

<font color=red>该属性设置可以设置块级元素文本的基本方向</font>，<font color=fuchsia>也可以设置由通过 `unicode-bidi`属性创建的嵌入元素的方向</font>。与此同时，它还可以设置文本、块级元素的默认对齐方式 ，以及表行中的单元格的流动方向。

与 HTML 中的 `dir` 属性不同，`direction` 属性不会从表列继承到表单元格，因为 CSS 继承遵从文档流，而表单元格位于行内部，但不在列内部。

`direction` 属性和 `unicode-bidi` 属性不受 `all` 属性影响。

##### 语法

```css
/* Keyword values */
direction: ltr;
direction: rtl;

/* Global values */
direction: inherit;
direction: initial;
direction: unset;
```

##### 取值

- `ltr` ：默认属性。可设置文本和其他元素的默认方向是从左到右。
- `rtl ` ：可设置文本和其他元素的默认方向是从右到左。

摘自：[MDN - direction](https://developer.mozilla.org/zh-CN/docs/Web/CSS/direction)

#### unicode-bidi

CSS `unicode-bidi` 属性，<font color=fuchsia>和 `direction` 属性，决定如何处理文档中的双书写方向文本</font> ( bidirectional text )。比如，如果一块内容同时包含有从左到右书写和从右到左书写的文本，那么用户代理 ( the user-agent ) 会使用复杂的 Unicode 算法来决定如何显示文本。<font color=red>`unicode-bidi` 属性会覆盖此算法，允许开发人员控制文本嵌入 ( text embedding )</font>。

`unicode-bidi` 与 `direction` 是仅有的两个不受 `all` 简写影响的属性。

> ⚠️ 此属性是文档类型定义 ( Document Type Definition , DTD ) 的设计者专用的。Web 设计者与其他类似的人员不应覆盖此属性

```css
/* 关键字值 */
unicode-bidi: normal;
unicode-bidi: embed;
unicode-bidi: isolate;
unicode-bidi: bidi-override;
unicode-bidi: isolate-override;
unicode-bidi: plaintext;
/* 全局值 */
unicode-bidi: inherit;
unicode-bidi: initial;
unicode-bidi: unset;
```

##### 语法

- `normal` ：对双向算法，此元素不提供额外的嵌入级别。对于内联元素，隐式的重新排序在元素的边界上起作用
- `embed` ：对于内联元素，该值会为双向算法打开一个额外的嵌入级别。嵌入级别的方向是由 `direction` 属性给出的
- `bidi-override` ：对于内联元素，该值会创建一个覆盖；对于块容器元素，该值将为不在另一个块容器元素内的内联级别的后代创建一个覆盖。这意味着在元素内部，根据 `direction` 属性，重新排序是严格按照顺序排列的；双向算法的隐式部分被忽略。
- `isolate` ：这个关键字表示计算元素容器的方向时，不考虑这个元素的内容。因此，这个元素就从它的兄弟姐妹中分离出来了。当应用它的双向分辨算法的时候，它的容器元素将其视为一个或多个 `U+FFFC Object Replacement Character`，即像 image 一样。
- `isolate-override `：<font color=red>这个关键字将 `isolate` 关键字的隔离行为应用于周围的内容，并将 `bidi-override` 关键字的覆盖行为应用于内部内容</font>。
- `plaintext` ：这个关键字在计算元素方向的时候，不考虑父元素的双向状态，也不考虑 `direction` 属性的值。它是使用 Unicode 双向算法的 P2 和 P3 规则计算的。 这个值允许按照 Unicode 双向算法显示已经格式化的数据。

摘自：[MDN - unicode-bidi](https://developer.mozilla.org/zh-CN/docs/Web/CSS/unicode-bidi)

#### direction 和 unicode-bidi 示例

##### 代码

```html
<p>hello world</p>

<style>
  p {
    width: 70px;
    direction: rtl;
    unicode-bidi: bidi-override;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
</style>
```

##### 效果

<img src="https://s2.loli.net/2022/09/06/UGRPvoIAdBcp42M.png" alt="image-20220906171852089" style="zoom:70%;" />

另外，如果没有指定宽度，文字将会居右：

<img src="https://s2.loli.net/2022/09/06/trLU9q2vSmjcE4e.png" alt="image-20220906172141115" style="zoom:60%;" />



#### all

CSS <font color=red>`all` [简写属性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Shorthand_properties)</font> <font color=fuchsia>将除了 `unicode-bidi` 与 `direction` 之外的所有属性重设至其初始值，或继承值</font>。

##### 语法

```css
/* Global values */
all: initial
all: inherit
all: unset

/* CSS Cascading and Inheritance Level 4 */
all: revert;
```

`all` 属性被作为 CSS 全局关键词的其中之一。不过需要注意的是，`unicode-bidi` 与 `direction` 这两个属性是不受 `all` 影响的

###### 取值

- `initial` ：该关键字代表改变该元素或其父元素的所有属性<font color=red>至初始值</font>。
- `inherit` ：该关键字代表改变该元素或其父元素的所有属性的值<font color=red>至他们的父元素属性的值</font>。
- `unset` ：该关键字代表<font color=dodgerblue>如果该元素的属性的值是可继承的</font>，则<font color=red>改变该元素或该元素的父元素的所有属性的值为他们父元素的属性值</font>，<font color=dodgerBlue>反之</font> 则<font color=red>改变为初始值</font>。
- `revert` ：指定依赖于声明所属的样式表原点的行为：
  - [User-agent origin](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Cascade#user-agent_stylesheets) 相当于 `unset`
  - [User origin](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Cascade#user_stylesheets) 将层叠回滚到用户代理级别，以便计算指定的值，就好像没有为该元素指定作者级别或用户级别规则。
  - [Author origin](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Cascade#author_stylesheets) 将层叠回滚到用户级别，以便计算指定的值，就好像没有为元素指定作者级规则。出于`revert`的目的，“作者”原点包括“覆盖”和“动画”原点。

摘自：[MDN - all](https://developer.mozilla.org/zh-CN/docs/Web/CSS/all)

#### initial、unset 和 revert

- `initial` ：将某一个 CSS 属性 变 / 恢复 为它（W3C 标准中）的默认值（各种 CSS 属性的默认值有很多，靠记忆是记不住的，也是没必要的；可以通过 `initial` 使它恢复为默认值）；注意不是 UA StyleSheet 中的默认直
- `unset` ：清除浏览器默认样式 ( UA Stylesheet )，让 CSS 属性可以继承的继承，不能继承的使用默认值
- `revert` ：让属性恢复为浏览器的默认样式

> ⚠️ 注意区分 W3C 的默认值和 UA 默认样式 的区别

学习自：[initial、unset、revert【渡一教育】](https://www.bilibili.com/video/BV1DH4y1C7ox)

#### CSS3 2D 转换

**浏览器支持**

表格中的数字表示支持该属性的第一个浏览器版本号。

紧跟在 -webkit-, -ms- 或 -moz- 前的数字为支持该前缀属性的第一个浏览器版本号。

| 属性                                  | Chrome            | Edge          | Firefox        | Safari       | Opera                            |
|:----------------------------------- | ----------------- | ------------- | -------------- | ------------ | -------------------------------- |
| transform                           | 36.0 4.0 -webkit- | 10.0 9.0 -ms- | 16.0 3.5 -moz- | 3.2 -webkit- | 23.0 15.0 -webkit- 12.1 10.5 -o- |
| transform-origin (two-value syntax) | 36.0 4.0 -webkit- | 10.0 9.0 -ms- | 16.0 3.5 -moz- | 3.2 -webkit- | 23.0 15.0 -webkit- 12.1 10.5 -o- |

Internet Explorer 10, Firefox, 和 Opera支持transform 属性.

Chrome 和 Safari 要求前缀 -webkit- 版本.

**注意：** Internet Explorer 9 要求前缀 -ms- 版本.

##### 2D变换方法

###### translate()

根据左(X轴)和顶部(Y轴)位置给定的参数，从当前元素<font color=FF0000>位置移动</font>。**示例：**

```css
div
{
    transform: translate(50px,100px);
    -ms-transform: translate(50px,100px); /* IE 9 */
    -webkit-transform: translate(50px,100px); /* Safari and Chrome */
}
```

###### rotate()

在一个给定度数<font color=FF0000>顺时针旋转</font>的元素。<mark>负值是允许的，这样是元素逆时针旋转</mark>。示例：

```css
div
{
    transform: rotate(30deg);
    -ms-transform: rotate(30deg); /* IE 9 */
    -webkit-transform: rotate(30deg); /* Safari and Chrome */
}
```

###### scale()

<font color=FF0000>元素增加或减少的大小</font>，取决于宽度（X轴）和高度（Y轴）的参数。示例：

```css
/*scale(2,3)转变宽度为原来的大小的2倍，和其原始大小3倍的高度。*/
div
{
  -ms-transform:scale(2,3); /* IE 9 */
    -webkit-transform: scale(2,3); /* Safari */
    transform: scale(2,3); /* 标准语法 */
}
```

> 💡 补充：<font color=red size=4>**使用 scale 可突破 chrome 字体 12px 的限制**</font>
>
> 思路：先将 font-size 设置为 targetFontSize 的 $1 / scaleVal$ 倍；比如 想要 font-size 设置为 10px，即：targetFontSize 为 10px，可将 scaleVal 设置为 0.5（即：`transform: scale(0.5)` ），此时 font-size 便可先设置为 20px。示例如下：
>
> ```html
> <div class="container">
>   <p class="scale">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Libero fuga rerum enim quae labore expedita quis iste incidunt sequi. Deserunt.</p>
> </div>
> 
> <style>
> .container {
>   width: 1000px;
>   height: 100px;
>   background: lightblue;
> }
> .scale {
>   font-size: 20px;
>   transform: scale(0.5) translate(-50%, -50%);
> }
> </style>
> ```
>
> ⚠️ 注意：<font color=fuchsia>使用 scale 之后，字体会出现位移；所以，需要使用 `translate(-50%, -50%)` 来抵消位移</font>。同时，因为上面的效果仅仅是缩放，会发现字体虽然缩放成功，但是段落宽度缩小了 scaleNum ，所以可以让 `width` 变成先前的 $1/scaleNum$ 倍以抵消效果。

###### skew()

定义了一个元素在二维平面上的倾斜转换。

**语法**

```css
transform: skew(<angle> [,<angle>]);
```

包含两个参数值，分别表示X轴和Y轴倾斜的角度，如果第二个参数为空，则默认为0，参数为负表示向相反方向倾斜。

**另外：**

- **skewX(\<angle>);**  表示只在X轴(水平方向)倾斜。

- **skewY(\<angle>);** 表示只在Y轴(垂直方向)倾斜。

**示例：**

```css
div {
    transform: skew(30deg,20deg);
    -ms-transform: skew(30deg,20deg); /* IE 9 */
    -webkit-transform: skew(30deg,20deg); /* Safari and Chrome */
}
```

###### matrix()

matrix()方法将2D变换方法合并成一个。matrix 方法有六个参数，包含旋转，缩放，移动（平移）和倾斜功能。

<font color=FF0000>`matrix(a, b, c, d, tx, ty)` 是 `matrix3d(a, b, 0, 0, c, d, 0, 0, 0, 0, 1, 0, tx, ty, 0, 1)` 的简写</font>

**总结：2D 转换方法**

| 函数                                        | 描述                                             |
|:----------------------------------------- |:---------------------------------------------- |
| matrix(*n*, *n*, *n*, *n*, *n*, *n*)      | 定义 2D 转换，使用六个值的矩阵。                             |
| translate(*x*, *y*)                       | 定义 2D 转换，沿着 X 和 Y 轴移动元素。                       |
| <font color=FF0000>translateX</font>(*n*) | 定义 2D 转换，<font color=FF0000>沿着 X 轴移动元素</font>。 |
| <font color=FF0000>translateY</font>(*n*) | 定义 2D 转换，<font color=FF0000>沿着 Y 轴移动元素</font>。 |
| scale(*x*, *y*)                           | 定义 2D 缩放转换，改变元素的宽度和高度。                         |
| scaleX(*n*)                               | 定义 2D 缩放转换，改变元素的宽度。                            |
| scaleY(*n*)                               | 定义 2D 缩放转换，改变元素的高度。                            |
| rotate(*angle*)                           | 定义 2D 旋转，在参数中规定角度。                             |
| skew(*x-angle*, *y-angle*)                | 定义 2D 倾斜转换，沿着 X 和 Y 轴。                         |
| skewX(*angle*)                            | 定义 2D 倾斜转换，沿着 X 轴。                             |
| skewY(*angle*)                            | 定义 2D 倾斜转换，沿着 Y 轴。                             |



#### transform

CSS transform 属性<font color=FF0000>允许你旋转，缩放，倾斜或平移给定元素</font>。这是通过修改CSS视觉格式化模型的坐标空间来实现的。

摘自：[MDN - transform](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform)



#### transform-origin

transform-origin CSS 属性让你<font color=FF0000>更改一个元素变形的原点</font>。<font color=FF0000>默认的转换原点是 center</font>

transform-origin <font color=FF0000>属性可以使用一个，两个或三个值来指定</font>，其中<font color=FF0000>**每个值都表示一个偏移量**</font>。 <font color=LightSeaGreen>没有明确定义的偏移将重置为其对应的初始值</font>

如果定义了两个或更多值并且没有值的关键字，或者唯一使用的关键字是 center，则第一个值表示水平偏移量，第二个值表示垂直偏移量

- **一个值：**必须是 `<length>`，`<percentage>`，<font color=FF0000>或</font> `left` , `center` , `right` , `top` , `bottom` 关键字中的一个。

- **两个值：**

  - 其中一个必须是 `<length>`，`<percentage>`，<font color=FF0000>或</font> `left` , `center` , <font color=FF0000>`right`</font> 关键字中的一个。
  - 另一个必须是 `<length>`，`<percentage>`，<font color=FF0000>或</font> `top` , `center` , <font color=FF0000>`bottom`</font>关键字中的一个。

- **三个值：**

  - 前两个值和只有两个值时的用法相同。
  - <font color=FF0000>**第三个值必须是 `<length>`**</font>。<font color=FF0000>它始终代表Z轴偏移量</font>。

##### 值

- **x-offset：**定义变形中心距离盒模型的左侧的 `<length>` 或`<percentage>` 偏移值。
- **offset-keyword：** `left` ，`right`，`top` ，`bottom` 或 `center` 中之一，定义相对应的变形中心偏移。
- **y-offset：**定义变形中心距离盒模型的顶的 `<length>` 或 `<percentage>` 偏移值。
- **x-offset-keyword：**`left`，`right`或 `center` 中之一，定义相对应的变形中心偏移。
- **y-offset-keyword：**`top`，`bottom` 或 `center` 中之一，定义相对应的变形中心偏移。
- **z-offset：**定义变形中心距离用户视线（z=0处）的 `<length>`（不能是 `<percentage>`）偏移值。

**关键字是方便的简写方法，等同于以下\<percentage>值：**

| keyword | value |
| :------ | :---: |
| left    |  0%   |
| center  |  50%  |
| right   | 100%  |
| top     |  0%   |
| bottom  | 100%  |

摘自：[MDN - transform-origin](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-origin)

> 💡发现了 transform-origin 的一个用法：使得文字呈现（圆）弧形。具体教程见：[css-tricks - Set Text on a Circle](https://css-tricks.com/set-text-on-a-circle/)
>
> 另外，也可以使用 svg 实现类似的效果，教程见：[css-tricks - Curved Text Along a Path](https://css-tricks.com/snippets/svg/curved-text-along-path/) ；以及示例：[codepen - Circle: Text Path](https://codepen.io/tylersticka/pen/ExxjyxO)



#### CSS 3D转换

**浏览器支持**

表格中的数字表示支持该属性的第一个浏览器版本号。

紧跟在 `-webkit-` , `-ms-` 或 `-moz-` 前的数字为支持该前缀属性的第一个浏览器版本号。

| 属性                                    | Chrome             | Edge | Firefox         | Safari       | Opera              |
|:------------------------------------- | ------------------ | ---- | --------------- | ------------ | ------------------ |
| transform                             | 36.0 12.0 -webkit- | 10.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |
| transform-origin (three-value syntax) | 36.0 12.0 -webkit- | 10.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |
| transform-style                       | 36.0 12.0 -webkit- | 11.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |
| perspective                           | 36.0 12.0 -webkit- | 10.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |
| perspective-origin                    | 36.0 12.0 -webkit- | 10.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |
| backface-visibility                   | 36.0 12.0 -webkit- | 10.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |

**转换属性**

| 属性                | 描述                                 | CSS  |
| :------------------ | :----------------------------------- | :--- |
| transform           | 向元素应用 2D 或 3D 转换。           | 3    |
| transform-origin    | 允许你改变被转换元素的位置。         | 3    |
| transform-style     | 规定被嵌套元素如何在 3D 空间中显示。 | 3    |
| perspective         | 规定 3D 元素的透视效果。             | 3    |
| perspective-origin  | 规定 3D 元素的底部位置。             | 3    |
| backface-visibility | 定义元素在不面对屏幕时是否可见。     | 3    |

**3D 转换方法**

| 函数                                                                                        | 描述                         |
|:----------------------------------------------------------------------------------------- |:-------------------------- |
| matrix3d(*n*, *n*, *n*, *n*, *n*, *n*,  *n*, *n*, *n*, *n*, *n*, *n*, *n*, *n*, *n*, *n*) | 定义 3D 转换，使用 16 个值的 4x4 矩阵。 |
| translate3d(*x*, *y* ,*z*)                                                                | 定义 3D 转化。                  |
| translateX(*x*)                                                                           | 定义 3D 转化，仅使用用于 X 轴的值。      |
| translateY(*y*)                                                                           | 定义 3D 转化，仅使用用于 Y 轴的值。      |
| translateZ(*z*)                                                                           | 定义 3D 转化，仅使用用于 Z 轴的值。      |
| scale3d(*x*, *y* ,*z*)                                                                    | 定义 3D 缩放转换。                |
| scaleX(*x*)                                                                               | 定义 3D 缩放转换，通过给定一个 X 轴的值。   |
| scaleY(*y*)                                                                               | 定义 3D 缩放转换，通过给定一个 Y 轴的值。   |
| scaleZ(*z*)                                                                               | 定义 3D 缩放转换，通过给定一个 Z 轴的值。   |
| rotate3d(*x*, *y*, *z*, *angle*)                                                          | 定义 3D 旋转。                  |
| rotateX(*angle*)                                                                          | 定义沿 X 轴的 3D 旋转。            |
| rotateY(*angle*)                                                                          | 定义沿 Y 轴的 3D 旋转。            |
| rotateZ(*angle*)                                                                          | 定义沿 Z 轴的 3D 旋转。            |
| perspective(*n*)                                                                          | 定义 3D 转换元素的透视视图。           |

#### translateZ()

The `translateZ()` [CSS function](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Functions) <font color=red>repositions</font>（重新定位）<font color=red>an element along the z-axis in 3D space</font>, i.e., <font color=red>closer to or farther away from the viewer</font>. Its result is a `<transform-function>` data type.

This transformation is defined by a `<length>` which <font color=red>specifies how far inward or outward the element or elements move</font>.

In the above interactive examples, `perspective: 550px;` (to create a 3D space) and [`transform-style: preserve-3d`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-style) ; (so the children, the 6 sides of the cube, are also positioned in the 3D space), have been set on the cube.

> ⚠️ **Note:** <font color=red>`translateZ(tz)` is equivalent to `translate3d(0, 0, tz)`</font> .

##### Syntax

```css
translateZ(tz)
```

##### Values

- `tz` : A `<length>` representing the z-component of the translating vector. A positive value moves the element towards the viewer, and a negative value farther away.

| Cartesian coordinates on ℝ^2                                 | Homogeneous coordinates on ℝℙ^2                              | Cartesian coordinates on ℝ^3 | Homogeneous coordinates on ℝℙ^3 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :--------------------------- | :------------------------------ |
| This transformation applies to the 3D space and can't be represented on the plane. | A translation is not a linear transformation in ℝ^3 and can't be represented using a Cartesian-coordinate matrix. | ( 10000100001t0001 )         |                                 |

摘自：[MDN US - translateZ()](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/translateZ)



#### transform-style

> 🧪 ：这是一个实验中的功能

CSS 属性 `transform-style` 设置元素的子元素是位于 3D 空间中还是平面中。

如果选择平面( flat ) ，元素的子元素将不会有 3D 的遮挡关系。

由于这个属性不会被继承，因此必须为元素的所有非叶子子元素设置它。

##### 语法

```css
/* Keyword values */
transform-style: flat;
transform-style: preserve-3d;

/* Global values */
transform-style: inherit;
transform-style: initial;
transform-style: unset;
```

##### 值

- `flat` ：默认值，设置元素的子元素位于该元素的平面中。
- `preserve-3d` ：指示元素的子元素应位于 3D 空间中。

摘自：[MDN - transform-style](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-style)



#### CSS3 过渡（transition）

添加某种效果是元素从一种样式逐渐改变为另一种的效果，无需使用Flash动画或JavaScript。

要实现这一点，必须规定两项内容：

- 指定要添加效果的CSS属性
- 指定效果的持续时间。

**示例**

```css
/*应用于宽度属性的过渡效果，时长为 2 秒：*/
div {
    transition: width 2s;
    -webkit-transition: width 2s; /* Safari */
}
```

**过渡属性**

| 属性                       | 描述                                         | CSS  |
| :------------------------- | :------------------------------------------- | :--- |
| transition                 | 简写属性，用于在一个属性中设置四个过渡属性。 | 3    |
| transition-property        | 规定应用过渡的 CSS 属性的名称。              | 3    |
| transition-duration        | 定义过渡效果花费的时间。默认是 0。           | 3    |
| transition-timing-function | 规定过渡效果的时间曲线。默认是 "ease"。      | 3    |
| transition-delay           | 规定过渡效果何时开始。默认是 0。             | 3    |



#### CSS3 动画

CSS3 可以创建动画，它可以取代许多网页动画图像、Flash 动画和 JavaScript 实现的效果。

要创建 CSS3 动画，你需要了解 `@keyframes` 规则。

- @keyframes 规则是创建动画。

- @keyframes 规则内<mark>指定一个 CSS 样式和动画将逐步从目前的样式更改为新的样式</mark>。

当在 **@keyframes** 创建动画，<mark><font color=FF0000>把它绑定到一个选择器</font>，否则动画不会有任何效果</mark>。指定至少这两个CSS3的动画属性绑定向一个选择器：

- 规定动画的名称
- 规定动画的时长（<mark>必须定义动画的名称和动画的持续时间</mark>。如果省略的持续时间，动画将无法运行，因为默认值是0。）

**CSS3动画是什么？**

动画是使元素从一种样式逐渐变化为另一种样式的效果。您可以改变任意多的样式任意多的次数。

<mark>请用百分比来规定变化发生的时间，或用关键词 "from" 和 "to"，等同于 0% 和 100%</mark>。

0% 是动画的开始，100% 是动画的完成。<mark>为了得到最佳的浏览器支持，您应该始终定义 0% 和 100% 选择器</mark>。

###### `@keyframes` 规则和所有动画属性

| 属性                      | 描述                                                         | CSS  |
| :------------------------ | :----------------------------------------------------------- | :--- |
| `@keyframes`              | 规定动画。                                                   | 3    |
| animation                 | 所有动画属性的简写属性。                                     | 3    |
| animation-name            | 规定 `@keyframes` 动画的名称。                               | 3    |
| animation-duration        | 规定动画完成一个周期所花费的秒或毫秒。默认是 0。             | 3    |
| animation-timing-function | 规定动画的速度曲线。默认是 "ease"。                          | 3    |
| animation-fill-mode       | 规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式。 | 3    |
| animation-delay           | 规定动画何时开始。默认是 0。                                 | 3    |
| animation-iteration-count | 规定动画被播放的次数。默认是 1。                             | 3    |
| animation-direction       | 规定动画是否在下一周期逆向地播放。默认是 "normal"。          | 3    |
| animation-play-state      | 规定动画是否正在运行或暂停。默认是 "running"。               | 3    |



#### CSS3 多列

##### column-count

指定了需要分割的列数。示例：

```css
/*将 <div> 元素中的文本分为 3 列*/
div {
    -webkit-column-count: 3; /* Chrome, Safari, Opera */
    -moz-column-count: 3; /* Firefox */
    column-count: 3;
}
```

##### column-gap

指定了列与列间的间隙。示例：

```css
/*指定了列与列间的间隙为 40 像素*/
div {
    -webkit-column-gap: 40px; /* Chrome, Safari, Opera */
    -moz-column-gap: 40px; /* Firefox */
    column-gap: 40px;
}
```

##### column-rule-style

指定了列与列间的边框样式。示例：

```css
div {
    -webkit-column-rule-style: solid; /* Chrome, Safari, Opera */
    -moz-column-rule-style: solid; /* Firefox */
    column-rule-style: solid;
}
```

##### column-rule-width

指定了两列的边框厚度。示例：

```css
div {
    -webkit-column-rule-width: 1px; /* Chrome, Safari, Opera */
    -moz-column-rule-width: 1px; /* Firefox */
    column-rule-width: 1px;
}
```

##### column-rule-color

指定了两列的边框颜色。示例：

```css
div {
    -webkit-column-rule-color: lightblue; /* Chrome, Safari, Opera */
    -moz-column-rule-color: lightblue; /* Firefox */
    column-rule-color: lightblue;
}
```

##### column-rule

是 column-rule-* 所有属性的简写。示例：

```css
/*设置了列直接的边框的厚度，样式及颜色*/
div {
    -webkit-column-rule: 1px solid lightblue; /* Chrome, Safari, Opera */
    -moz-column-rule: 1px solid lightblue; /* Firefox */
    column-rule: 1px solid lightblue;
}
```

##### column-span

指定元素跨越多少列。示例：

```css
/*指定 <h2> 元素跨越所有列*/
h2 {
    -webkit-column-span: all; /* Chrome, Safari, Opera */
    column-span: all;
}
```

##### column-width

指定了列的宽度。示例：

```css
div {
    -webkit-column-width: 100px; /* Chrome, Safari, Opera */
    column-width: 100px;
}
```

##### CSS3 的多列属性

| 属性              | 描述                                      |
| :---------------- | :---------------------------------------- |
| column-count      | 指定元素应该被分割的列数。                |
| column-fill       | 指定如何填充列                            |
| column-gap        | 指定列与列之间的间隙                      |
| column-rule       | 所有 column-rule-* 属性的简写             |
| column-rule-color | 指定两列间边框的颜色                      |
| column-rule-style | 指定两列间边框的样式                      |
| column-rule-width | 指定两列间边框的厚度                      |
| column-span       | 指定元素要跨越多少列                      |
| column-width      | 指定列的宽度                              |
| columns           | column-width 与 column-count 的简写属性。 |



#### CSS3 用户界面

##### resize

<font color=FF0000>指定一个元素是否应该由用户去调整大小（比如用户改变窗口的大小）</font>。示例：

```css
div {
  resize: both;
  overflow: auto;
}
```

**取值：**` none | both | horizontal | vertical | block | inline `

- **none：** 元素不能被用户缩放。
- **both：** 允许用户在水平和垂直方向上调整元素的大小。
- **horizontal：** 允许用户在水平方向上调整元素的大小。
- **vertical：** 允许用户在垂直方向上调整元素的大小。
- **block**
- **inline**

> **注意 **⚠️：<font color=FF0000>如果一个 block 元素的 overflow 属性被设置成了 visible ，那么 resize 属性对该元素无效</font>。

##### box-sizing

以确切的方式定义适应某个区域的具体内容。示例：

```css
div {
  box-sizing:border-box;
  -moz-box-sizing:border-box; /* Firefox */
  width:50%;
  float:left;
}
```

在 CSS 盒子模型的默认定义里，你对一个元素所设置的 width 与 height 只会应用到这个元素的内容区。如果这个元素有任何的 border 或 padding ，绘制到屏幕上时的盒子宽度和高度会加上设置的边框和内边距值。这意味着当你调整一个元素的宽度和高度时需要时刻注意到这个元素的边框和内边距。当我们实现响应式布局时，这个特点尤其烦人。

<font color=dodgerBlue>box-sizing 属性可以被用来调整这些表现：</font>

- **content-box：**是默认值。如果你设置一个元素的宽为100px，那么这个元素的内容区会有100px 宽，并且任何边框和内边距的宽度都会被增加到最后绘制出来的元素宽度中。
- **border-box：**告诉浏览器：你想要设置的边框和内边距的值是包含在width内的。也就是说，如果你将一个元素的width设为100px，那么这100px会包含它的border和padding，内容区的实际宽度是width减去(border + padding)的值。大多数情况下，这使得我们更容易地设定一个元素的宽高。
- **inherit：**继承 父元素 box-sizing属性的值

另外：border-box不包含margin

摘自：[MDN - box-sizing](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-sizing)

##### box-sizing 的补充

如果上面没有看懂的话，可以看看下面的

盒子模型中盒子宽度和高度的计算方式会根据<font color=FF0000>盒子的类型的不同而不同</font>，而盒子类型是根据 box-sizing 的属性设置的。

- **content-box**：是默认值，CSS 属性中的 width 和 height 设置的是内容区域的宽高，而盒子的宽高则需要加上内边距 ( padding ) 和宽高 ( width / height )
- **border-box**：CSS 中 width 和 height 的值分别是 content 宽高 + padding + border 值的和

摘自：[动画解释 CSS 盒子模型、布局方式、box-sizing、替换元素和边距塌陷，CSS 布局不再难！](https://www.bilibili.com/video/BV13V411z7do)

content-box：标准盒模型，CSS 定义的宽高只包含 content 的宽高

border-box：IE 盒模型，CSS 定义的宽高包括了content，padding 和 border

摘自：[学会使用box-sizing布局](https://www.jianshu.com/p/e2eb0d8c9de6)

##### outline-offset

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

| 属性           | 说明                                           | CSS  |
| :------------- | :--------------------------------------------- | :--- |
| appearance     | 允许您使一个元素的外观像一个标准的用户界面元素 | 3    |
| box-sizing     | 允许你以适应区域而用某种方式定义某些元素       | 3    |
| icon           | 为创作者提供了将元素设置为图标等价物的能力。   | 3    |
| nav-down       | 指定在何处使用箭头向下导航键时进行导航         | 3    |
| nav-index      | 指定一个元素的Tab的顺序                        | 3    |
| nav-left       | 指定在何处使用左侧的箭头导航键进行导航         | 3    |
| nav-right      | 指定在何处使用右侧的箭头导航键进行导航         | 3    |
| nav-up         | 指定在何处使用箭头向上导航键时进行导航         | 3    |
| outline-offset | 外轮廓修饰并绘制超出边框的边缘                 | 3    |
| resize         | 指定一个元素是否是由用户调整大小               | 3    |



#### CSS 框大小

##### 不使用 CSS3 box-sizing 属性

默认情况下，元素的宽度与高度计算方式如下：

- width（宽） + padding（内边距） + border（边框） = 元素实际宽度

- height（高） + padding（内边距） + border（边框） = 元素实际高度

这就意味着我们在设置元素的 width/height 时，元素真实展示的高度与宽度会更大（因为元素的边框与内边距也会计算在 width/height 中）

##### 使用 CSS3 box-sizing 属性

CSS3 box-sizing 属性在一个元素的 width 和 height 中包含 padding（内边距） 和 border（边框）

如果在元素上设置了 `box-sizing: border-box` ，则 padding（内边距）和 border（边框） 也包含在 width 和 height 中



#### @规则 ( at-rule )

一个 `@rule` 是一个CSS 语句，以 at 符号开头，'@' ( U+0040 COMMERCIAL AT )，后跟一个标识符，并包括直到下一个分号的所有内容， ';' (U+003B SEMICOLON)，或下一个 CSS 块，以先到者为准。

**下面是一些 @规则, 由它们的标示符指定，每种规则都有不同的语法：**

- **@charset：**<font color=FF0000>定义样式表使用的字符集</font>.
- **@import：**<font color=FF0000>告诉 CSS 引擎引入一个外部样式表</font>.
- **@namespace：**告诉 CSS 引擎必须考虑XML命名空间。
- <font color=FF0000>**嵌套 @规则**， 是嵌套语句的子集，**不仅可以作为样式表里的一个语句，也可以用在条件规则组里**</font>：
  - **@media：**如果满足媒介查询的条件则条件规则组里的规则生效。
  - **@page：**描述打印文档时布局的变化.
  - **@font-face：**描述将下载的外部的字体。 
  - **@keyframes：**描述 CSS 动画的中间步骤 . 
  - **@supports：**如果满足给定条件则条件规则组里的规则生效。 
  - **@document：**如果文档样式表满足给定条件则条件规则组里的规则生效。 (<mark>推延至 CSS Level 4 规范</mark>)

摘自：[MDN - @规则](https://developer.mozilla.org/zh-CN/docs/Web/CSS/At-rule)

#### @charset

##### 概述

 @charset CSS @规则  <font color=FF0000>指定样式表中使用的字符编码</font>。它<font color=FF0000 size=4>**必须是样式表中的第一个元素**</font>，<font color=FF0000>而 <font size=4>**前面不得有任何字符**</font></font>。因为它不是一个嵌套语句，所以不能在 @规则条件组 中使用。<font color=FF0000>**如果有多个 @charset @规则被声明，只有第一个会被使用**</font>，而且<font color=FF0000>不能在HTML元素或HTML页面的字符集相关 \<style> 元素内的样式属性内使用</font>。

此 @规则 在某些 CSS 属性中使用非 ASCII 字符时非常有用，例如 content。

在样式表中有多种方法去声明字符编码，浏览器会按照以下顺序尝试下边的方法（一旦找到就停止并得出结果）：

1. 文件的开头的 Unicode byte-order 字符值。
2. 由Content-Type：HTTP header 中的 charset 属性给出的值或用于提供样式表的协议中的等效值。
3. CSS @规则 @charset。
4. 使用参考文档定义的字符编码： \<link> 元素的 charset 属性。 该方法在 HTML5 标准中已废除，无法使用。
5. 假设文档是 UTF-8。

##### 语法

```css
@charset "UTF-8";
@charset "iso-8859-15";
```

- **charset：**它是一个 \<string> 表示字符编码被使用。<font color=FF0000>它必须是在被 IANA-registry 声明过的 web-safe 字符编码中的一个</font>，<font color=FF0000 size=4>**还必须被双引号包围, 遵循一个空格字符 (U+0020)，并且立即以分号结束**</font>。 如果有多个相关的编码名字，只有被标记为 preferred  的那个才会被使用。

##### 例子

```css
@charset "UTF-8";
@charset "utf-8"; /*大小写不敏感*/
/* 设置css的编码格式为Unicode UTF-8 */
@charset 'iso-8859-15'; /* 无效的, 使用了错误的引号 */
@charset 'UTF-8';       /* 无效的, 使用了错误的引号 */
@charset  "UTF-8";      /* 无效的, 多于一个空格 */
 @charset "UTF-8";      /* 无效的, 在at-rule之前多了一个空格 */
@charset UTF-8;         /* Invalid, without ' or ", the charset is not a CSS <string> */
```

摘自：[MDN - @charset](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@charset)

#### @import

**@import**是 CSS @规则，<font color=FF0000>用于从其他样式表导入样式规则</font>。<font color=FF0000>这些规则必须先于所有其他类型的规则</font>，@charset 规则除外： 因为它不是一个嵌套语句，@import不能在条件组的规则中使用。

因此，<mark>用户代理可以避免为不支持的媒体类型检索资源，作者可以指定依赖媒体的@import规则</mark>。这些条件导入在URI之后指定逗号分隔的媒体查询。在没有任何媒体查询的情况下，导入是无条件的。指定所有的媒体具有相同的效果。

**语法**

```css
@import url;
@import url list-of-media-queries;
```

- **url：**<font color=FF0000>是一个表示要引入资源位置的 \<string> 或者 \<uri></font> 。 <mark>这个 URL 可以是绝对路径或者相对路径</mark>。 要注意的是这个 URL 不需要指明一个文件； 可以只指明包名，然后合适的文件会被自动选择 (e.g. chrome://communicator/skin/)。
- **list-of-media-queries：**<font color=FF0000>是一个**逗号分隔**的 媒体查询 条件列表，决定通过URL引入的 CSS 规则 在什么条件下应用</font>。如果浏览器不支持列表中的任何一条媒体查询条件，就不会引入URL指明的CSS文件。

摘自：[MDN - @import](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@import)

#### @supports

**@supports** CSS的 @规则。您<font color=FF0000>可以**指定依赖于浏览器中的一个或多个特定的CSS功能的支持声明**</font>。这<font color=FF0000>被称为特性查询</font>。<font color=LightSeaGreen>该规则可以放在代码的顶层，也可以嵌套在任何其他条件组规则中</font>。

```css
/* 支持 grid布局 */
@supports (display: grid) {
  div {
    display: grid;
  }
}
/* 不支持 grid布局 */
@supports not (display: grid) {
  div {
    float: right;
  }
}
```

<font color=FF0000>**在 JavaScript 中，可以通过 CSS 对象模型接口 CSSSupportsRule 来访问 @supports**</font>。

##### 语法

@supports @规则 <font color=FF0000>**由一组样式声明和一条支持条件构成**</font>。<font color=FF0000>支**持条件由一条或多条使用 逻辑与（and）、逻辑或（or）、逻辑非（not）结合的名称-值对（name-value pair）组成**</font>。<font color=FF0000>**可以使用圆括号调整操作符的优先级**</font>。

###### 声明语法

最基本的支持条件就是 CSS 声明，也就是<font color=FF0000>一个 CSS 属性后跟一个值，中间用冒号分开</font>。<font color=FF0000>如果 transform-origin 的实现语法认为 `5% 5%  ` 是有效的值，则下面的表达式会返回 true</font>。

```css
@supports (transform-origin: 5% 5%) {}
```

###### 函数语法

第二种基本支持条件是支持函数，几乎所有浏览器都支持这种语法，但函数本身仍在标准化进程中。

###### selector() 🧪

测试浏览器是否支持经过测试的选择器语法。如果浏览器支持子组合器，则以下示例返回true：

```css
@supports selector(A > B) {}
```

###### not 操作符

将 <font color=FF0000>not 操作符放在任何表达式之前就能否定一条表达式</font>。<mark>如果 transform-origin 的实现语法认为 10em 10em 10em 是无效的，则下面的表达式会返回 true</mark>。

```css
@supports not (transform-origin: 10em 10em 10em) {}
```

<font color=FF0000>和其他操作符一样，not 操作符可以应用在任意复杂度的表达式上</font>。下面的几个例子中都是合法的表达式：

```css
@supports not (not (transform-origin: 2px)) {}
@supports (display: grid) and (not (display: inline-grid)) {}
```

<mark>**注意：**如果 not 操作符位于表达式的最外层，则没有必要使用圆括号将它括起来。但如果要将该表达式与其他表达式连接起来使用，比如 and 和 or，则需要外面的圆括号</mark>。

###### and 操作符

and 操作符用来将两个原始的表达式做逻辑与后生成一个新的表达式，如果两个原始表达式的值都为真，则生成的表达式也为真。在下例中，当且仅当两个原始表达式同时为真时，整个表达式才为真：

```css
@supports (display: table-cell) and (display: list-item) {}
```

可以将多个合取词并置而不需要更多的括号。以下两者都是等效的：

```css
@supports (display: table-cell) and (display: list-item) and (display:run-in) {}
@supports (display: table-cell) and ((display: list-item) and (display:run-in)) {}
```

###### or 操作符

or 操作符用来将两个原始的表达式做逻辑或后生成一个新的表达式，如果两个原始表达式的值有一个或者都为真，则生成的表达式也为真。在下例中，当两个原始表达式中至少有一个为真时，整个表达式才为真：

```css
@supports (transform-style: preserve) or (-moz-transform-style: preserve) {}
```

可以将多个析取词并置而不需要更多的括号。以下两者都是等效的：

```css
@supports (transform-style: preserve) or (-moz-transform-style: preserve) or
          (-o-transform-style: preserve) or (-webkit-transform-style: preserve) {}

@supports (transform-style: preserve-3d) or ((-moz-transform-style: preserve-3d) or
          ((-o-transform-style: preserve-3d) or (-webkit-transform-style: preserve-3d))) {}
```

> ⚠️ 在使用 and 和 or 操作符时，如果是为了定义多个表达式的执行顺序，则必须使用圆括号。如果不这样做的话，该条件就是无效的，会导致整个 @规则 失效。

摘自：[MDN - @supports](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@support)



#### @layer

CSS @规则中的 <font color=red>`@layer` 声明了一个级联层</font>，<font color=red>同一层内的规则将级联在一起</font>，这给予了开发者对层叠机制的更多控制。

```css
@layer utilities {
  /* 创建一个名为 utilities 的级联层 */
}
```

##### 形式语法

```css
@layer = 
  @layer <layer-name>? { <stylesheet> } |
  @layer <layer-name># ;

<layer-name> = <ident> [ '.' <ident> ]*  
```

##### 语法

`@layer` @规则 可以通过三种方式其一来创建级联层。

###### 第一种

创建一个块级的 @规则，其中包含作用于该层内部的 CSS 规则。

```css
@layer utilities {
  .padding-sm { padding: .5rem; }
  .padding-lg { padding: .8rem; }
}
```

###### 第二种

一个级联层同样可以通过 `@import` 来创建，规则存在于被引入的样式表内：

```css
@import(utilities.css) layer(utilities);
```

###### 第三种

你也可以创建带命名的级联层，但不指定任何样式。例如，单一的命名层：

```css
@layer utilities
```

或者，<font color=red>多个命名层也可以被同时定义</font>。例如：

```css
@layer theme, layout, utilities
```

这一做法很有用，因为<font color=dodgerBlue>层最初被指定的顺序决定了它是否有优先级</font>。<font color=red>**对于声明而言，如果同一声明在多个级联层中被指定，最后一层中的将优先于其他层**</font>。因此，在上面的例子中，如果 `theme` 层和 `utilities` 层中存在冲突的规则，那么 `utilities` 层中的将优先被应用。

<font color=fuchsia>即使 `utilities` 层中规则的优先级低于 `theme` 层中的</font>，该规则仍会被应用。一旦级联层顺序建立之后，优先级和出现顺序都会被忽略。<font color=red>这将使创建 CSS 选择器变得更加简单</font>，因为你<font color=red>不需要确保每一个选择器都有足够高的优先级来覆盖其他冲突的规则</font>，你<font color=red>只需要确保它们出现在一个顺序更靠后的级联层中</font>。

> 💡 **备注：** 在已经声明级联层的名字后，它们的顺序随即被确立，你<font color=red>可以重复声明某级联层的名字来向其添加 CSS 规则</font>。这些样式将被附加到该层的末尾，且级联层之间的顺序不会改变。

<font color=red>其他不属于任何一级联层的样式将被集中到同一匿名层，并置于所有层的后部</font>，这意味着任何在层外声明的样式都会覆盖在层内声明的样式。

##### 嵌套层

<font color=red>级联层允许嵌套</font>，例如：

```css
@layer framework {
  @layer layout { /* ... */ }
}
```

向 `layout` 层内部的 `framework` 层附加规则，只需用 `.` 连接这两层。

```css
@layer framework.layout {
  p { margin-block: 1rem; }
}
```

##### 匿名层

如果创建了一个级联层但并未指定名字，例如：

```css
@layer {
  p { margin-block: 1rem; }
}
```

那么则称为创建了一个匿名层。除<font color=red>创建后无法向其添加规则</font>外，<font color=lightSeaGreen>该层和其他命名层功能一致</font>。

摘自：[MDN - @layer](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@layer)



#### !important

!important主要用于<font color=FF0000>**提升**指定样式规则的**优先级**</font>。**示例：**

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



#### CSS3 盒子模型

<mark>弹性盒子</mark>是 CSS3 的<mark>一种新的布局模式</mark>。

CSS3 弹性盒 ( Flexible Box 或 flexbox )，是一种当页面需要<font color=FF0000>适应不同的屏幕大小以及设备类型时确保元素拥有恰当的行为的布局方式</font>。

引入弹性盒布局模型的目的是提供一种更加有效的方式来<mark>对一个容器中的子元素进行排列、对齐和分配空白空间</mark>。

<font color=FF0000>**弹性盒子**</font>由**<font color=FF0000>弹性容器(Flex container)</font>**和**<font color=FF0000>弹性子元素(Flex item)</font>**组成。

弹性容器<font color=FF0000>通过设置 display 属性的值</font>为 <font color=FF0000>**flex**</font> 或 **<font color=FF0000>inline-flex</font>** 将其定义为弹性容器。

弹性容器内包含了<font color=FF0000>一个</font>或<font color=FF0000>多个</font>弹性子元素。弹性子元素<font color=FF0000>通常在弹性盒子内一行显示</font>。<font color=FF0000>**默认情况每个容器只有一行**</font>。

> ⚠️ 设为 Flex 布局以后，子元素的 float、clear 和 vertical-align 属性将失效。

#### Flex容器（父容器）

##### flex-direction

flex-direction 属性指定了弹性子元素在父容器中的位置

###### 语法

```css
flex-direction: row | row-reverse | column | column-reverse
```

- **row**：横向从左到右排列（左对齐），默认的排列方式。
- **row-reverse**：反转横向排列（右对齐，从后往前排，最后一项排在最前面）。
- **column**：纵向排列。
- **column-reverse**：反转纵向排列，从后往前排，最后一项排在最上面。

##### justify-content 属性

内容对齐（justify-content）属性应用在弹性容器上，把弹性项沿着弹性容器的主轴线（main axis）对齐。

###### 语法

```css
justify-content: flex-start | flex-end | center | space-between | space-around
```

- **flex-start**：弹性项目<font color=FF0000>向**行头**紧挨着填充</font>。这个<font color=FF0000>是默认值</font>。<font color=FF0000>第一个弹性项的main-start外边距边线被放置在该行的**main-start边线**</font>，而后续弹性项依次平齐摆放
- **flex-end**：弹性项目<font color=FF0000>向**行尾**紧挨着填充</font>。<font color=FF0000>第一个弹性项的main-end外边距边线被放置在该行的**main-end边线**</font>，而后续弹性项依次平齐摆放。
- **center**：弹性项目居中紧挨着填充。（<mark>如果剩余的自由空间是负的，则弹性项目将在两个方向上同时溢出</mark>）。
- **space-between**：弹性项目<mark>平均分布在该行</mark>上。<mark>如果剩余<font color=FF0000>空间为负</font>或者<font color=FF0000>只有一个弹性项</font>，则该值<font color=FF0000>等同于flex-start</font></mark>。否则，第1个弹性项的外边距和行的main-start边线对齐，而最后1个弹性项的外边距和行的main-end边线对齐，然后剩余的弹性项分布在该行上，相邻项目的间隔相等。
- **space-around**：弹性项目<mark>平均分布在该行</mark>上，<font color=FF0000>两边留有一半的间隔空间</font>。<mark>如果剩余空间为负或者只有一个弹性项，则该值等同于center</mark>。否则，弹性项目沿该行分布，且<font color=FF0000>彼此间隔相等（比如是20px）</font>，同时<font color=FF0000>首尾两边和弹性容器之间留有一半的间隔（1/2*20px=10px）</font>。
- **space-evenly：**flex项都沿着主轴均匀分布在指定的对齐容器中。相邻flex项之间的间距，主轴起始位置到第一个flex项的间距，主轴结束位置到最后一个flex项的间距，都完全一样。
- **stretch：**均匀排列每个元素'auto'-sized 的元素会被拉伸以适应容器的大小
- **safe：**与对齐关键字一起使用，如果选定的关键字会导致元素溢出容器造成数据丢失，那么将会使用 start 代替它。
- **unsafe**

##### align-items 属性

align-items<mark>设置或检索弹性盒子元素在<font color=FF0000>侧轴（纵轴）方向上的对齐方式</font></mark>。

###### 语法

```css
align-items: flex-start | flex-end | center | baseline | stretch
```

- **flex-start**：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴<font color=FF0000>起始</font>边界。
- **flex-end**：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴<font color=FF0000>结束</font>边界。
- **center**：弹性盒子元素在该行的侧轴（纵轴）上居中放置。（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度）。
- **baseline**：如弹性盒子元素的行内轴与侧轴为同一条，则该值与'flex-start'等效。其它情况下，该值<font color=FF0000>将与基线对齐</font>。
- **stretch**：<font color=FF0000>默认值</font>，如果指定侧轴大小的属性值为'auto'，则其值会使项目的边距盒的尺寸尽可能接近所在行的尺寸，但同时会遵照'min/max-width/height'属性的限制。

##### flex-wrap 属性

**flex-wrap** 属性用于指定弹性盒子的子元素换行方式。

###### 语法

```css
flex-wrap: nowrap|wrap|wrap-reverse|initial|inherit;
```

- **nowrap**： <font color=FF0000>默认</font>， 弹性容器为<font color=FF0000>单行</font>。该情况下<mark>弹性子项**可能会**溢出容器</mark>。
- **wrap**： 弹性容器为<font color=FF0000>多行</font>。该情况下<mark>弹性子项溢出的部分会被放置到新行</mark>，子项内部会发生断行
- **wrap-reverse**：反转 wrap 排列。

##### align-content 属性

align-content 属性用于<font color=FF0000>修改 flex-wrap属性的行为</font>。类似于align-items, 但它不是设置弹性子元素的对齐，而是<font color=FF0000>**设置各个行的对齐**</font>。

###### 语法

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

##### order

<font color=FF0000>（容易被忽略）</font>定义项目的<font color=FF0000>排列顺序</font>。数值越小，排列越靠前，默认为0，可以为负值。]

```css
.item {
    order: <integer>;
}
```

##### flex-grow

定义项目的<font color=FF0000>**放大比例**</font>，<font color=FF0000>**默认为0**</font>，<font color=FF0000>即如果存在剩余空间，也不放大</font>。

<font color=FF0000>如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍</font>。

```css
.item {
  flex-grow: <number>; /* default 0 */
}
```

##### flex-shrink

定义了项目的<font color=FF0000>**缩小比例**</font>，<font color=FF0000>**默认为1**</font>，即如果空间不足，该项目将缩小。

<font color=FF0000>如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小</font>。

```css
.item {
  flex-shrink: <number>; /* default 1 */
}
```

##### flex-basis

定义了在<font color=FF0000>**flex-basis给上面两个属性分配多余空间之前, 计算项目是否有多余空间, 默认值为 auto, 即项目本身的大小**</font>。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

```css
.item {
  flex-basis: <length> | auto; /* default auto */
}
```

##### flex

flex 属性是 flex-grow、 flex-shrink 和 flex-basis的简写，<font color=FF0000>默认值为0 1 auto。后两个属性可选</font>。

###### 语法

```css
.item {
    1 | initial | none | inherit |  [ flex-grow ] || [ flex-shrink ] || [ flex-basis ]
}
```

- auto: 计算值为 1 1 auto
- initial: 计算值为 0 1 auto
- none：计算值为 0 0 auto
- inherit：从父元素继承

##### align-self

<font color=FF0000>允许单个项目有与其他项目不一样的对齐方式</font>，设置弹性元素自身在侧轴（纵轴）方向上的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

###### **语法**

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

> 💡 **补充**

- flex: 1; === flex: 1 1 (0 / 任意数字+任意长度单位); <font color=FF0000>（就是代表均匀分配元素）</font>
  
  <font color=FF0000>补充：flex: 1也可以表示占满剩余空间（相对于后面70%的，见后面 --> ）</font>，同时相对应的：`flex: 1 1 70%`表示占据70%的空间

- flex: initial; === flex: 0 1 auto;

- flex: none; === flex: 0 0 auto;

摘自：[flex:1 到底代表什么?](https://zhuanlan.zhihu.com/p/136223806)

place-content 是 align-content 和 justify-content 的简写属性；而 <font color=FF0000>place-items 是 align-items 和 justify-items 的简写属性</font>。即：

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

##### flex 和 inline-flex 的区别

- flex： 将对象作为<font color=FF0000>弹性伸缩盒</font>显示
- inline-flex：将对象作为<font color=FF0000>**内联**块级弹性伸缩盒</font>显示

摘自：[display：flex和display: inline-flex区别](https://www.jianshu.com/p/4d596708f61b)

![](https://i.loli.net/2020/08/25/A3Qhi7IHJotxzMr.png)



#### CSS3 多媒体查询

使用 @media 查询，你<font color=FF0000>可以针对不同的媒体类型定义不同的样式</font>。

CSS3 的多媒体查询继承了 CSS2 多媒体类型的所有思想： 取代了查找设备的类型，CSS3 根据设置自适应显示。

媒体查询可用于检测很多事情，例如：

- viewport(视窗) 的宽度与高度
- 设备的宽度与高度
- 朝向 (智能手机横屏，竖屏) 
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

- **not** ：not是用来<font color=FF0000>排除掉某些特定的设备</font>的，比如 @media not print（非打印设备）。
  
  > 💡 **补充**
  >
  > 用于否定媒体查询，<font color=LightSeaGreen>如果不满足这个条件则返回 true，否则返回false</font>。 如果出现在以逗号分隔的查询列表中，它将仅否定应用了该查询的特定查询。 如果使用not运算符，则还必须指定媒体类型。
  >
  > 摘自：[MDN - 使用媒体查询](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Media_queries)
  
- **only** ：用来<font color=FF0000>定某种特别的媒体类型</font>。对于支持 Media Queries 的移动设备来说，如果存在 only 关键字，移动设备的 Web 浏览器会忽略 only 关键字并直接根据后面的表达式应用样式文件。对于不支持 Media Queries 的设备但能够读取 Media Type 类型的 Web 浏览器，遇到 only 关键字时会忽略这个样式文件。

- **all** ：<font color=FF0000>所有设备</font>

- **and** ：and 操作符用于 <font color=LightSeaGreen>将多个媒体查询规则组合成单条媒体查询</font>，<font color=FF0000>当每个查询规则都为真时则该条媒体查询为真</font>，它还用于将媒体功能与媒体类型结合在一起。

- **,** （逗号）：逗号 用于<font color=LightSeaGreen>将多个媒体查询合并为一个规则</font>。 逗号分隔列表中的每个查询都与其他查询分开处理。 因此，<font color=LightSeaGreen>如果列表中的任何查询为 true ，则整个 media 语句均返回 true</font>。 换句话说，<font color=FF0000>列表的行为类似于逻辑或 `or` 运算符</font>。

以上部分内容摘自：[MDN - 使用媒体查询](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Media_queries)

**CSS3 多媒体类型**

| 值      | 描述               |
|:------ |:---------------- |
| all    | 用于所有多媒体类型设备      |
| print  | 用于打印机            |
| screen | 用于电脑屏幕，平板，智能手机等。 |
| speech | 用于屏幕阅读器          |

更多相关媒体类型 / 媒体功能参见：[Runoob - CSS3 @media 查询](https://www.runoob.com/cssref/css3-pr-mediaquery.html)

##### 关于的像素密度补充

CSS 中有关于 “像素密度” 的媒体查询，如下示例：

```css
@media only screen and (min-device-pixel-ratio: 1.5) {
  #my-image { background: (high.png); }
}
```

即上面的 min-device-pixel-ratio 即是 一种条件。

摘自：[「2021」高频前端面试题汇总之CSS篇](https://juejin.cn/post/6905539198107942919)

另外，关于 device-pixel-ratio 搜了一下 MDN，只搜到 [-webkit-device-pixel-ratio](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media/-webkit-device-pixel-ratio) ，其中还提到： device-pixel-ratio 和 [resolution](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/resolution) 是等价的（写法不同）。

> **`-webkit-device-pixel-ratio`** is a range value meaning the prefixed **`-webkit-min-device-pixel-ratio`** and **`-webkit-max-device-pixel-ratio`** are also available.
>
> ```css
> @media (-webkit-min-device-pixel-ratio: 2) { ... }
> /* is equivalent to */
> @media (min-resolution: 2dppx) { ... }
> 
> /* And likewise */
> @media (-webkit-max-device-pixel-ratio: 2) { ... }
> /* is equivalent to */
> @media (max-resolution: 2dppx) { ... }
> ```

在搜素的过程中，顺便还搜到了媒体查询条件 [image-resolution](https://developer.mozilla.org/en-US/docs/Web/CSS/image-resolution) ，这里略。



#### @container-query

// TODO 

和 `@media` 使用一样，不过使用场景不同。入门参考：[最新：CSS Container Queries 容器查询，响应式布局利器](https://www.bilibili.com/video/BV1oo4y1C7nJ)



#### 响应式 Web 设计 - Viewport

##### Viewport

Viewport 是<font color=FF0000>用户网页的可视区域</font>，翻译为中文可以叫做"<font color=FF0000>视区</font>"。

手机浏览器是<font color=FF0000>把页面放在一个虚拟的"窗口"（viewport）中</font>，<font color=FF0000>**通常这个虚拟的"窗口"（viewport）比屏幕宽**</font>，这样就不用把每个网页挤到很小的窗口中（这样会破坏没有针对手机浏览器优化的网页的布局），用户可以通过平移和缩放来看网页的不同部分。

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

##### 方向：横屏/竖屏

结合 CSS 媒体查询，<mark>可以创建适应不同设备的方向（<font color=FF0000>横屏landscape</font>、<font color=FF0000>竖屏portrait</font> 等）的布局</mark>。

##### 语法

```css
orientation：portrait | landscape
```

- **portrait：**指定输出设备中的页面可见区域高度大于或等于宽度
- **landscape：** 除portrait值情况外，都是landscape

##### 示例

```css
@media only screen and (orientation: landscape) {
    body {
        background-color: lightblue;
    }
}
```

##### 使用指点设备

作为四级规范的一部分，hover媒体特征被引入了进来。这种特征意味着你<mark>可以测试用户是否能在一个元素上悬浮</mark>，这也<mark>基本就是说他们正在使用某种指点设备，因为触摸屏和键盘导航是没法实现悬浮的</mark>。

###### 示例

```css
@media (hover: hover) {
    body {
        color: rebeccapurple;
    }
}
```

##### 断点

引入媒体查询的点就叫做 **断点**。

摘自：[MDN - 使用媒体查询](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Media_queries)



#### prefers-color-scheme

`prefers-color-scheme` CSS <font color=FF0000>**媒体特性 **</font> 用于<font color=FF0000>检测用户是否有将系统的主题色设置为亮色或者暗色</font>。

**语法**

- **no-preference：**表示系统未得知用户在这方面的选项。在布尔值上下文中，其执行结果为 false。
- **light：**表示用户已告知系统他们选择使用浅色主题的界面。
- **dark：**表示用户已告知系统他们选择使用暗色主题的界面。

> 译者注：“未得知”、“已告知”等用语，英文原文如此。
> **“未得知”** 可理解为：浏览器的宿主系统不支持设置主题色，或者支持主题色并默认为/被设为了未设置/无偏好
> **“已告知”** 为：浏览器的宿主系统支持设置主题色，且被设置为了亮色或者暗色。

目前，<font color=LightSeaGreen>若结果为 no-preference，无法通过此媒体特性获知宿主系统是否支持设置主题色，或者用户是否主动将其设置为无偏好</font>。出于隐私保护等方面的考虑，用户或用户代理也可能在一些情况下在浏览器内部将其设置为 no-preference。

##### 示例

```css
@media (prefers-color-scheme: dark) {
  .day.dark-scheme   { background:  #333; color: white; }
  .night.dark-scheme { background: black; color:  #ddd; }
}
@media (prefers-color-scheme: light) {
  .day.light-scheme   { background: white; color:  #555; }
  .night.light-scheme { background:  #eee; color: black; }
}
```

摘自：[MDN - prefers-color-scheme](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media/prefers-color-scheme)



#### 总结 / 文档：CSS 选择器

| 选择器                                        | 例子                  | 例子描述                                                     | CSS版本 |
| :-------------------------------------------- | :-------------------- | :----------------------------------------------------------- | :-----: |
| .*class*                                      | .intro                | 选择 class="intro" 的所有元素。                              |    1    |
| #*id*                                         | \#firstname           | 选择 id="firstname" 的所有元素。                             |    1    |
| *                                             | *                     | 选择所有元素。                                               |    2    |
| *element*                                     | p                     | 选择所有 \<p> 元素。                                         |    1    |
| *element*,*element*                           | div,p                 | 选择所有\<div> 元素和所有\<p> 元素。                         |    1    |
| <font color=FF0000>*element* *element*</font> | div p                 | <font color=FF0000>选择\<div>元素**内部**的所有\<p>元素。</font> |    1    |
| *element*>*element*                           | div>p                 | 选择<font color=FF0000>父元素</font>为\<div>元素的所有\<p>元素。 |    2    |
| *element*+*element*                           | div+p                 | 选择<font color=FF0000>紧接在\<div>元素之后</font>的所有\<p>元素。 |    2    |
| *attribute*                                   | [target]              | 选择带有 target 属性所有元素。                               |    2    |
| [*attribute*=*value*]                         | [target=_blank]       | 选择 target="_blank" 的所有元素。                            |    2    |
| [*attribute*~=*value*]                        | [title~=flower]       | 选择 title 属性包含单词 "flower" 的所有元素。                |    2    |
| [*attribute*\|=*value*\]                      | [lang\|=en]           | 选择 lang 属性值以 "en" 开头的所有元素。                     |    2    |
| :link                                         | a:link                | 选择所有未被访问的链接。                                     |    1    |
| :visited                                      | a:visited             | 选择所有已被访问的链接。                                     |    1    |
| :active                                       | a:active              | 选择活动链接。                                               |    1    |
| :hover                                        | a:hover               | 选择鼠标指针位于其上的链接。                                 |    1    |
| :focus                                        | input:focus           | 选择获得焦点的 input 元素。                                  |    2    |
| :first-letter                                 | p:first-letter        | 选择每个\<p>元素的首字母。                                   |    1    |
| :first-line                                   | p:first-line          | 选择每个\<p>元素的首行。                                     |    1    |
| :first-child                                  | p:first-child         | 选择属于父元素的第一个子元素的每个\<p>元素。                 |    2    |
| :before                                       | p:before              | 在每个\<p>元素的内容之前插入内容。                           |    2    |
| :after                                        | p:after               | 在每个\<p>元素的内容之后插入内容。                           |    2    |
| :lang(*language*)                             | p:lang(it)            | 选择带有以 "it" 开头的 lang 属性值的每个\<p>元素。           |    2    |
| *element1*~*element2*                         | p~ul                  | 选择前面有\<p> 元素的每个\<ul>元素。                         |    3    |
| *attribute*^=*value*\]                        | a[src^="https"]       | 选择其 src 属性值以 "https" 开头的每个\<a>元素。             |    3    |
| *attribute*$=*value*\]                        | a[src$=".pdf"]        | 选择其 src 属性以 ".pdf" 结尾的所有\<a>元素。                |    3    |
| [*attribute**=*value*\]                       | a[src*="abc"]         | 选择其 src 属性中包含 "abc" 子串的每个\<a>元素。             |    3    |
| :first-of-type                                | p:first-of-type       | 选择属于其父元素的首个\<p> 元素的每个\<p>元素。              |    3    |
| :last-of-type                                 | p:last-of-type        | 选择属于其父元素的最后\<p> 元素的每个\<p>元素。              |    3    |
| :only-of-type                                 | p:only-of-type        | 选择属于其父元素唯一的\<p> 元素的每个\<p>元素。              |    3    |
| :only-child                                   | p:only-child          | 选择属于其父元素的唯一子元素的每个\<p>元素。                 |    3    |
| :nth-child(*n*)                               | p:nth-child(2)        | 选择属于其父元素的第二个子元素的每个\<p>元素。               |    3    |
| :nth-last-child(*n*)                          | p:nth-last-child(2)   | 同上，从最后一个子元素开始计数。                             |    3    |
| :nth-of-type(*n*)                             | p:nth-of-type(2)      | 选择属于其父元素第二个\<p>元素的每个\<p> 元素。              |    3    |
| :nth-last-of-type(*n*)                        | p:nth-last-of-type(2) | 同上，但是从最后一个子元素开始计数。                         |    3    |
| :last-child                                   | p:last-child          | 选择属于其父元素最后一个子元素每个\<p>元素。                 |    3    |
| :root                                         | :root                 | 选择文档的根元素。                                           |    3    |
| :empty                                        | p:empty               | 选择<font color=FF0000>没有子元素</font>的每个\<p>元素（<font color=FF0000>包括文本节点</font>）。 |    3    |
| :target                                       | \#news:target         | 选择当前活动的 \#news 元素。                                 |    3    |
| :enabled                                      | input:enabled         | 选择每个启用的\<input>元素。                                 |    3    |
| :disabled                                     | input:disabled        | 选择每个禁用的\<input>元素                                   |    3    |
| :checked                                      | input:checked         | 选择每个被选中的\<input>元素。                               |    3    |
| :not(*selector*)                              | :not(p)               | 选择非\<p>元素的每个元素。                                   |    3    |
| ::selection                                   | ::selection           | 选择被用户选取的元素部分。                                   |    3    |

摘自：[runoob - CSS 选择器](https://www.runoob.com/cssref/css-selectors.html)

![img](https://s2.loli.net/2022/01/16/yafVOHADUo5QsmM.png)

摘自：[2021年你可能不知道的 CSS 特性](https://zhuanlan.zhihu.com/p/376238191)

##### 补充

关于 `nth-of-type` ：CSS 伪类是<font color=FF0000>针对具有一组兄弟节点的标签, 用 n 来筛选出在一组兄弟节点的位置</font>。示例如下。<font color=FF0000>n从1开始</font>

```css
/* 在每组兄弟元素中选择第四个 <p> 元素 */
p:nth-of-type(4n) {
  color: lime;
}
```



#### :nth-child

**:nth-child(an+b)** 这个 CSS 伪类首先找到所有当前元素的兄弟元素，然后按照位置先后顺序从1开始排序，选择的结果为CSS伪类:nth-child括号中表达式（an+b）匹配到的元素集合（n=0，1，2，3...）。

##### 示例

- `0n+3` 或简单的 `3` 匹配第三个元素。
- `1n+0` 或简单的 `n` 匹配每个元素。（兼容性提醒：在 Android 浏览器 4.3 以下的版本 `n` 和 `1n` 的匹配方式不一致。`1n` 和 `1n+0` 是一致的，可根据喜好任选其一来使用。）
- `2n+0` 或简单的 `2n` 匹配位置为 2、4、6、8...的元素（n=0时，2n+0=0，第0个元素不存在，因为是从1开始排序)。你可以使用关键字 **even** 来替换此表达式。
- `2n+1` 匹配位置为 1、3、5、7...的元素。你可以使用关键字 **odd** 来替换此表达式。
- `3n+4` 匹配位置为 4、7、10、13...的元素。

a 和 b 都必须为整数，并且元素的第一个子元素的下标为 1。换言之就是，该伪类匹配所有下标在集合 { an + b; n = 0, 1, 2, ...} 中的子元素。另外需要特别注意的是，*an* 必须写在 *b* 的前面，不能写成 *b+an* 的形式。

- **tr:nth-child(2n+1)：**表示HTML表格中的奇数行。
- **tr:nth-child(odd)：**表示HTML表格中的奇数行。
- **tr:nth-child(2n)：**表示HTML表格中的偶数行。
- **tr:nth-child(even)：**表示HTML表格中的偶数行。
- **span:nth-child(0n+1)：**表示子元素中第一个且为span的元素，与 :first-child 选择器作用相同。
- **span:nth-child(1)：**表示父元素中子元素为第一的并且名字为span的标签被选中
- **span:nth-child(-n+3)：**匹配前三个子元素中的span元素。

摘自：[MDN - nth-child](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-child)



#### placeholoder 加上颜色

##### 方法一

用 `::placeholder`（<font color=FF0000>比较简洁，但这是一个实验功能，兼容性比较差</font>）

伪元素 `::placeholder` 可以选择一个表单元素的占位文本，它允许开发者和设计师自定义占位文本的样式。

###### 示例

```css
::placeholder {
  color: red;
  font-size: 1.5em;
}
```

##### 方法二

不同浏览器分别解决

```css
/* - Chrome ≤56,
   - Safari 5-10.0
   - iOS Safari 4.2-10.2
   - Opera 15-43
   - Opera Mobile >12
   - Android Browser 2.1-4.4.4
   - Samsung Internet
   - UC Browser for Android
   - QQ Browser */
::-webkit-input-placeholder {
    color: #ccc;
    font-weight: 400;
}

/* Firefox 4-18 */
:-moz-placeholder {
    color: #ccc;
    font-weight: 400;
}

/* Firefox 19-50 */
::-moz-placeholder {
    color: #ccc;
    font-weight: 400;
}

/* - Internet Explorer 10–11
   - Internet Explorer Mobile 10-11 */
:-ms-input-placeholder {
    color: #ccc !important;
    font-weight: 400 !important;
}

/* Edge (also supports ::-webkit-input-placeholder) */
::-ms-input-placeholder {
    color: #ccc;
    font-weight: 400;
}

/* CSS Working Draft */
::placeholder {
    color: #ccc;
    font-weight: 400;
}
```

摘自：[MDN - ::placeholder](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::placeholder)  [如何修改placeholder样式](https://juejin.cn/post/6844903697135173645)

#### :placeholder-shown

> 🧪 ：这是一个实验性质的功能

`:placeholder-shown` CSS 伪类 在 `<input>` 或 `<textarea>` 元素显示 placeholder text 时生效。示例如下：

```css
/* 选择所有显示占位符(placeholder)的元素 */
:placeholder-shown {
  border: 2px solid silver;
}
```

摘自：[MDN - :placeholder-shown](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:placeholder-shown)



#### div实现“可输入”

可以使用 contenteditable 属性。示例如下：

```html
<div class='input' contenteditable placeholder='请输入文字' />

<style>
    .input{
        width:200px;
        height:24px;
        line-height:24px;
        font-size:14px;
        padding:5px 8px;
        border:1px solid #ddd;
    }
    .input:empty::before {
        content: attr(placeholder);
    }
</style>
```

摘自：[div模拟input实现输入框](https://blog.csdn.net/qq_36671474/article/details/68064132)

#### contenteditable

<font color=FF0000>一个HTML元素的 `contenteditable` 属性被设置为 `true` 时，`document.execCommand()` 方法便可使用</font>。通过该方法，你可以运行相关commands 来操作可编辑区域的内容。其中大多数命令都会影响文档的选择，例如，给文本提供一个样式(加粗，倾斜等)、插入新元素(如增加一个链接)、影响一整行文本(如缩进排版)。当使用contentEditable后，调用execCommand()方法将影响当前处于活动状态的可编辑元素

摘自：[MDN - Content Editable](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/Editable_content)

**<font size='4'>document.execCommand</font>**

> ⚠️已废弃，详见 [CanIuse-execCommand](https://caniuse.com/?search=execCommand)

当一个 HTML 文档切换到设计模式时，document 暴露 execCommand 方法，该方法允许运行命令来操纵可编辑内容区域的元素。

大多数命令影响 document 的 selection（粗体，斜体等），当其他命令插入新元素（添加链接）或影响整行（缩进）。当使用 `contentEditable` 时，调用 `execCommand()` 将影响当前活动的可编辑元素。

##### 语法

```js
bool = document.execCommand(aCommandName, aShowDefaultUI, aValueArgument)
```

###### 返回值

一个 Boolean ，如果是 `false` 则表示操作不被支持或未被启用。

###### 参数

- **aCommandName：**一个 DOMString ，命令的名称。可用命令列表请参阅 [命令](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/execCommand#%E5%91%BD%E4%BB%A4) 。
- **aShowDefaultUI：**一个 Boolean， 是否展示用户界面，一般为 false。Mozilla 没有实现。
- **aValueArgument：**一些命令（例如 insertImage ）需要额外的参数（ insertImage 需要提供插入 image 的url ），默认为 null。

###### 命令

由于过长：略，见原文

摘自：[MDN - document.execCommand](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/execCommand)

##### 补充

`contenteditable` 有一个类似功能的 js 属性为：`Document.designMode`

> `document.designMode` 控制整个文档是否可编辑。有效值为 "on" 和 "off" 。
>
> 摘自：[MDN - Document.designMode](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/designMode)



#### calc()

alac() 此 CSS 函数允许在声明 CSS 属性值时执行一些计算。它可以用在如下场合：\<length>、\<frequency>, \<angle>、\<time>、\<percentage>、\<number>、 \<integer>。

##### 语法

```css
property: calc(expression)
```

##### 示例

```css
width: calc(100% - 80px);
```

此 `calc()` 函数用一个表达式作为它的参数，用这个表达式的结果作为值。<font color=FF0000>这个表达式可以是任何如下操作符的组合</font>，采用标准操作符处理法则的简单表达式。

- **+**：加法。
- **-** ：减法。
- *****：乘法，乘数中至少有一个是 `<number>`
- **/**：除法，除数（/右面的数）必须是 `<number>`

表达式中的运算对象可以使用任意 `<length>` 值。如果你愿意，你<font color=FF0000>可以在一个表达式中混用这类值的不同单位</font>。在需要时，你<font color=FF0000>还可以使用小括号来建立计算顺序</font>。

##### 备注

- <font color=FF0000>**+ 和 - 运算符的两边必须要有空白字符**</font>。比如，`calc(50% -8px)` 会被解析成为一个无效的表达式，解析结果是：一个百分比 后跟一个负数长度值。而加有空白字符的、有效的表达式 `calc(8px + -50%)` 会被解析成为：一个长度 后跟一个加号 再跟一个负百分比。
- <mark>***** 和 **/** 这两个运算符前后不需要空白字符，但如果考虑到统一性，仍然推荐加上空白符</mark>。
- 用 0 作除数会使 HTML 解析器抛出异常。
- 涉及自动布局和固定布局的表格中的表列、表列组、表行、表行组和表单元格的宽度和高度百分比的数学表达式，`auto` 可视为已指定。
- `calc()` 函数支持嵌套，但支持的方式是：把被嵌套的 `calc()` 函数全当成普通的括号（译者注：所以，函数内直接用括号就好了）

摘自：[MDN - calc()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/calc)

#### attr()

CSS表达式 attr() 用来<font color=FF0000>获取选择到的元素的某一HTML属性值，并用于其样式（CSS）</font>。它也可以用于伪元素，属性值采用伪元素所依附的元素。

<font color=FF0000>**理论上：**</font>**attr() 表达式可以用于任何CSS属性**。

<font color=FF0000>**注意：**attr() **理论上**能用于所有的CSS属性，<font color=FF0000>**但目前支持的仅有伪元素的 content 属性**</font>，其他的属性和高级特性目前是实验性的</font>

摘自：[MDN - attr](https://developer.mozilla.org/zh-CN/docs/Web/CSS/attr())

#### var()

**概述**
var()函数可以<font color=FF0000>代替元素中任何属性中的值的任何部分</font>。var()函数不能作为属性名、选择器或者其他除了属性值之外的值。（这样做通常会产生无效的语法或者一个没有关联到变量的值。）

**语法**

```css
var( <custom-property-name> , <declaration-value>? )
```

方法的<font color=FF0000>第一个参数</font>是<font color=FF0000>要替换的自定义属性的名称</font>。函数的<font color=FF0000>可选第二个参数</font> <font color=FF0000>用作回退值</font>。<mark>如果第一个参数引用的自定义属性无效，则该函数将使用第二个值</mark>。

**注意：**<font color=FF0000>自定义属性的回退值允许使用逗号</font>。例如， `var(--foo, red, blue)` 将 `red, blue`同时指定为回退值；即是说<mark>任何在第一个逗号之后到函数结尾前的值都会被考虑为回退值</mark>。

###### 值

- **\<custom-property-name> 自定义属性名：**在实际应用中它被定义为以两个破折号开始的任何有效标识符。 自定义属性仅供作者和用户使用; CSS 将永远不会给他们超出这里表达的意义。
- **\<declaration-value> 声明值（后备值）：**回退值被用来在自定义属性值无效的情况下保证函数有值。回退值可以包含任何字符，但是部分有特殊含义的字符除外，例如换行符、不匹配的右括号（如`)`、 `]`或`}`）、感叹号以及顶层分号（不被任何非`var()`的括号包裹的分号，例如`var(--bg-color, --bs;color)`是不合法的，而`var(--bg-color, --value(bs;color))`是合法的）。

摘自：[MDN - var()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/var())

#### CSS自定义属性

自定义属性（有时候也被称作 CSS变量或者级联变量）是由CSS作者定义的，它包含的值可以在整个文档中重复使用。由自定义属性标记设定值（比如： `--main-color: black;`），<font color=FF0000>**由 `var()` 函数来获取值**</font>（比如： `color: var(--main-color);` ）

> 💡 自定义属性名是大小写敏感的，`--my-color` 和 `--My-color` 会被认为是两个不同的自定义属性。

摘自：[MDN - 使用CSS自定义属性（变量）](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Using_CSS_custom_properties)

今年( 2017 ) 三月，微软宣布 Edge 浏览器将支持 CSS 变量。这个重要的 CSS 新功能，所有主要浏览器已经都支持了。

声明变量的时候，变量名前面要加两根连词线（ `--` ）。

```css
body {
  --foo: #7F583F;
  --bar: #F7EFD2;
}
```

上面代码中，body 选择器里面声明了两个变量：`--foo` 和 `--bar` 。

<font color=LightSeaGreen>它们与 color、font-size 等正式属性没有什么不同，只是 **没有默认含义**</font>。所以 <font color=FF0000>CSS 变量 ( CSS variable ) 也叫做 “ CSS 自定义属性” ( CSS custom properties )</font> 。因为变量与自定义的 CSS 属性其实是一回事。

你可能会问，<font color=dodgerBlue>为什么选择两根连词线( `--` )表示变量</font>？因为 <font color=lightSeaGreen>`$foo` 被 Sass 用掉了，`@foo` 被 Less 用掉了</font>。为了不产生冲突，官方的 CSS 变量就改用两根连词线了。

<font color=dodgerBlue>**各种值都可以放入 CSS 变量**：</font>

```css
:root{
  --main-color: #4d4e53;
  --main-bg: rgb(255, 255, 255);
  --logo-border-color: rebeccapurple;

  --header-height: 68px;
  --content-padding: 10px 20px;

  --base-line-height: 1.428571429;
  --transition-duration: .35s;
  --external-link: "external link";
  --margin-top: calc(2vh + 20px);
}
```

<font color=FF0000>**变量名大小写敏感**</font>，`--header-color` 和 `--Header-Color`是两个不同变量。

##### var() 函数用于读取变量

```css
a {
  color: var(--foo);
  text-decoration-color: var(--bar);
}
```

`var()` 函数 <font color=fuchsia>**还可以使用第二个参数**</font>，<font color=red>**表示变量的默认值**。如果该变量不存在，就会使用这个默认值</font>。

```css
color: var(--foo, #7F583F);
```

第二个参数不处理内部的逗号或空格，都视作参数的一部分。

```css
var(--font-stack, "Roboto", "Helvetica");
var(--pad, 10px 15px 20px);
```

<font color=FF0000>`var()` 函数还可以用在变量的声明</font>。

```css
:root {
  --primary-color: red;
  --logo-text: var(--primary-color);
}
```

> 👀 上面的等价于 `:root { --logo-text: red }` 

<font color=FF0000>**注意，变量值只能用作属性值，不能用作属性名**</font>。

```css
.foo {
  --side: margin-top;
  /* 无效 */
  var(--side): 20px;
}
```

上面代码中，变量 `--side` 用作属性名，这是无效的。

##### 变量值的类型

<font color=FF0000>如果变量值是一个字符串，**可以与其他字符串拼接**</font>。

> ⚠️ 是直接拼接，不需要加上其他连接符号

```css
--bar: 'hello';
--foo: var(--bar)' world';
```

利用这一点，可以 debug。

```css
body::after {
  /* 就是将变量的键值对通过 after伪元素，打印出来。*/
  content: '--screen-category : 'var(--screen-category);
}
```

<font color=FF0000 size=4>**如果变量值是数值，不能与数值单位直接连用**</font>。

```css
.foo {
  --gap: 20;
  /* 无效 */
  margin-top: var(--gap)px;
}
```

<font color=FF0000>**上面代码中，数值与单位直接写在一起，这是无效的。必须使用 calc() 函数，将它们连接**</font>

```css
.foo {
  --gap: 20;
  margin-top: calc(var(--gap) * 1px);
}
```
> 💡 在开发中，由于忘记了这个写法，写成了
> ```css
> .trunk-icon {
>     --icon-width: 30px;
>     width: var(--icon-with);
> }
> ```
> 导致在开发环境 icon 显示正常，线上测试环境 icon 不显示的情况。改成如下代码后，线上测试环境 icon 显示也正常的情况：
> 
> ```css
> .trunk-icon {
>     --icon-width: 30;
>     width: calc(var(--icon-width) * 1px)
> }
> ```

<font color=FF0000>**如果变量值带有单位，就不能写成字符串**</font>

```css
/* 无效 */
.foo {
  --foo: '20px'; /* 如下，不需要加上引号 */
  font-size: var(--foo);
}

/* 有效 */
.foo {
  --foo: 20px;
  font-size: var(--foo);
}
```

##### 作用域

同一个 CSS 变量，可以在多个选择器内声明。读取的时候，优先级最高的声明生效（**注：**即，特指度）。这与 CSS 的 “层叠” ( cascade ) 规则是一致的。下面是一个[例子](http://jsbin.com/buwahixoqo/edit?html,css,output)。

```html
<style>
  :root { --color: blue; }
  div { --color: green; }
  #alert { --color: red; }
  * { color: var(--color); }
</style>

<p>蓝色</p>
<div>绿色</div>
<div id="alert">红色</div>
```

上面代码中，三个选择器都声明了 `--color` 变量。不同元素读取这个变量的时候，会采用优先级最高的规则，因此三段文字的颜色是不一样的。这就是说，变量的作用域就是它所在的选择器的有效范围。

```css
body { --foo: #7F583F; }
.content { --bar: #F7EFD2; }
```

上面代码中，变量 `--foo` 的作用域是 body 选择器的生效范围，`--bar` 的作用域是 `.content` 选择器的生效范围。

由于这个原因，全局的变量通常放在根元素 `:root` 里面，确保任何选择器都可以读取它们。

```css
:root { --main-color: #06c; }
```

##### 响应式布局

CSS 是动态的，页面的任何变化，都会导致采用的规则变化。<font color=FF0000>利用这个特点，可以在响应式布局的 `media` 命令里面声明变量，使得不同的屏幕宽度有不同的变量值</font>。

```css
body {
  --primary: #7F583F;
  --secondary: #F7EFD2;
}

a {
  color: var(--primary);
  text-decoration-color: var(--secondary);
}

@media screen and (min-width: 768px) {
  body {
    --primary:  #F7EFD2;
    --secondary: #7F583F;
  }
}
```

**兼容性处理**

对于不支持 CSS 变量的浏览器，<font color=dodgerBlue>可以采用下面的写法</font>。

```css
a {
  color: #7F583F;
  color: var(--primary);
}
```

<font color=FF0000>**也可以使用 `@support` 命令进行检测**</font>。

```css
@supports ( (--a: 0)) { 
  /* supported */
}
@supports ( not (--a: 0)) {
  /* not supported */
}
```

##### JavaScript 操作

<font color=FF0000>**JavaScript 也可以检测浏览器是否支持 CSS 变量**</font>。（注意下面的用法）

```css
const isSupported =
  window.CSS &&
  window.CSS.supports &&
  window.CSS.supports('--a', 0);

if (isSupported) {
  /* supported */
} else {
  /* not supported */
}
```

###### JavaScript 操作 CSS 变量的写法如下

```js
// ⭐️ 设置变量
document.body.style.setProperty('--primary', '#7F583F');

// 读取变量
document.body.style.getPropertyValue('--primary').trim();
// '#7F583F'

// 删除变量
document.body.style.removeProperty('--primary');
```

> 💡 示例可参考 https://www.zhangxinxu.com/study/201811/js-set-css-var.php

这意味着，JavaScript 可以将任意值存入样式表。下面是一个监听事件的例子，事件信息被存入 CSS 变量。

```css
const docStyle = document.documentElement.style;

document.addEventListener('mousemove', (e) => {
  docStyle.setProperty('--mouse-x', e.clientX);
  docStyle.setProperty('--mouse-y', e.clientY);
});
```

<font color=FF0000>那些对 CSS 无用的信息，也可以放入 CSS 变量</font>。

```js
--foo: if(x > 5) this.width = 10;
```

<font color=FF0000>上面代码中，`--foo` 的值在 CSS 里面是无效语句，但是可以被 JavaScript 读取。这意味着，可以把样式设置写在 CSS 变量中，让 JavaScript 读取</font>。

所以，CSS 变量提供了 JavaScript 与 CSS 通信的一种途径。

摘自：[阮一峰 - CSS 变量教程](https://www.ruanyifeng.com/blog/2017/05/css-variables.html)

##### 实战中的补充

<font color=FF0000 size=4>**CSS 变量具有继承性**</font>，即：在 ***父节点*** 定义的 CSS 变量，在***子节点*** 中依然可以使用。另外，在使用 `node.style.setProperty` 等方法时，node 不必是 CSS 变量定义的所在的节点，也可以是需要修改样式的（子）节点；如下示例：

```html
<div id="wrap">
  <div id="child" onclick="handleClick"/>
</div>

<style>
  #wrap { --width: 100px; --bg-color: lightblue; }
  #child { width: var(--width); height: var(--width); background: var(--bg-color); }
</style>

<script>
  // node 是 child，不是 wrap，虽然 --width 在 wrap 定义
  handleClick() { child.style.setProperty('--width', '200px') }
</script>
```



#### counter()

CSS 函数 `counter()` ，返回一个代表计数器的当前值的字符串。它通常和伪元素搭配使用，但是理论上可以在支持 `<string>` 值的任何地方使用。

```css
/* 简单使用 */
counter(计数器名称);

/* 更改计数器显示 */
counter(countername, upper-roman)
```

摘自：[MDN - counter()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/counter)

#### clamp()

`clamp()` 函数的作用是<font color=FF0000>把一个值限制在一个上限和下限之间，当这个值超过最小值和最大值的范围时，在最小值和最大值之间选择一个值使用</font>。<font color=lightSeaGreen>它接收三个参数：最小值、**首选值**、最大值</font>。 `clamp()` 被用在 `<length>`、`<frequency>`、`<angle>`、`<time>`、`<percentage>`、`<number>`、`<integer>` 中都是被允许的。

`clamp(MIN, VAL, MAX)` 其实就是表示 `max(MIN, min(VAL, MAX) )`

##### 语法

clamp() 函数接收三个用逗号分隔的表达式作为参数，按最小值、首选值、最大值的顺序排列。

- <font color=FF0000>当首选值比最小值要小时，则使用最小值</font>。
- 当首选值介于最小值和最大值之间时，用首选值。
- <font color=FF0000>当首选值比最大值要大时，则使用最大值</font>。

摘自：[MDN - clamp()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clamp())



#### CSS 函数

根据 w3cplus 中可以划分为以下几类：

- 属性函数：attr()；

- 背景图片函数：linear-gradient()、radial-gradient()、conic-gradient()、repeating-linear-gradient()、repeating-radial-gradient()、repeating-conic-gradient()、image-set()、image()、url()、element()；

- 颜色函数：rgb()、rgba()、hsl()、hsla()、hwb()、color-mod()；

- 图形函数：circle()、ellipse()、inset()、polygon()、path()

- 滤镜函数：blur()、brightness()、contrast()、drop-shadow()、grayscale()、hue-rotate()、invert()、opacity()、saturate()、sepia()；

- 转换函数：matrix()、matrix3d()、perspective()、rotate()、rotate3d()、rotateX()、rotateY()、rotateZ()、scale()、scale3d()、scaleX()、scaleY()、scaleZ()、skew()、skewX()、skewY()、translate()、translateX()、translateY()、translateZ()、translate3d()；

- 数学函数：calc()、min()、max()、mixmax()、repeat()；

- 缓动函数：cubic-bezier()、steps()；

- 其他函数：counter()、counters()、toggle()、var()、 symbols()。

摘自：[css函数竟然有86个！！！css函数大全](https://blog.csdn.net/MFWSCQ/article/details/89530967)

##### 鱼骨图

![](https://www.w3cplus.com/sites/default/files/blogs/2020/2005/css-function-5.svg)

MDN 也有 总结：[MDN - CSS Functional Notation](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Functions) // TODO 有必要看下

##### env()

// TODO 。另外，注意下其中的 `safe-area-inset-*` ，`env(safe-area-insert-*)` 用来表示屏幕四角是弧形的高度，这在 iOS 环境下较为常用。

摘自：[MDN - env()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/env())

##### url()

url() 不再局限于 background-image 属性中，还可以用于：

```css
background-image: url("https://mdn.mozillademos.org/files/16761/star.gif");
list-style-image: url('../images/bullet.jpg');
content: url("pdficon.jpg");
cursor: url(mycursor.cur);
border-image-source: url(/media/diamonds.png);
src: url('fantasticfont.woff');
offset-path: url(#path);
mask-image: url("masks.svg#mask1");
@font-face {
    font-family: "Open Sans";
    src: url("/fonts/OpenSans-Regular-webfont.woff2") format("woff2"),
        url("/fonts/OpenSans-Regular-webfont.woff") format("woff");
}
```

简单地说，在CSS中可以使用url()函数来引用相应的资源，有点类似于HTML中的 src，href 属性。

##### attr()

原则上说，attr() 能运用于所有的 CSS 属性；但<font color=red>目前仅能服务于 CSS 的伪元素 `::before` 和 `::after` 的 content 属性</font>

摘自：[图解CSS: CSS中的函数](https://www.w3cplus.com/css/css-functions-guide.html)



#### mask-image

mask-image CSS属性用于设置元素上遮罩层的图像。是一个实验性的属性。

##### 可选值

- none：默认值，透明的黑色图像层，也就是<font color=FF0000>没有遮罩层</font>。
- \<mask-source>：\<mask> 或 CSS 图像的 url
- \<image>：图片作为遮罩层

##### 示例

```css
/* Keyword value */
mask-image: none;

/* <mask-source> value */
mask-image: url(masks.svg#mask1);

/* <image> values */
mask-image: linear-gradient(rgba(0, 0, 0, 1.0), transparent);
mask-image: image( url(mask.png), skyblue );

/* Multiple values */
mask-image: image(url(mask.png), skyblue), linear-gradient(rgba(0, 0, 0, 1.0), transparent);

/* Global values */
mask-image: inherit;
mask-image: initial;
mask-image: unset;
```

> 👀 注：之所以想要了解这个 CSS 属性，是因为在看 [MDN - Performance](https://developer.mozilla.org/zh-CN/docs/Web/API/Performance) 看到了如下图的内容；但是复制到笔记时，笔记上并没有出现 “⚠️” 标志，有的只是 “Non-Standard” ，这让我感到好奇。
>
> <img src="https://s2.loli.net/2022/05/22/Zy8OuP1RLIEVFJB.png" alt="image-20220522235008322" style="zoom:50%;" />

摘自：[MDN - mask-image](https://developer.mozilla.org/zh-CN/docs/Web/CSS/mask-image)



#### 如何设置页面全局背景图（可随着屏幕大小作裁切）

```css
html{
    width: 100%;
    height: 100%;
    overflow-x:hidden; 
    overflow-y:hidden;
}

body{
    width:100%;
    min-height: 100%; 
    background-image: url(../images/houtai/loginBj@2x.png);
    background-position: center 0;
    background-repeat: no-repeat;
    background-attachment: fixed;
    background-size: cover;
    -webkit-background-size: cover;
    -o-background-size: cover;
    -moz-background-size: cover;
    -ms-background-size: cover; 
}
```

[背景图片的适配问题](https://blog.csdn.net/weixin_43612234/article/details/85417227)



#### list-style集合

list-style CSS 属性是一个简写对属性集合，包括list-style-type, list-style-image, 和 list-style-position。

- **list-style-type：**

  CSS 属性 list-style-type 可以<font color=FF0000>设置列表元素的 marker（比如圆点、符号、或者自定义计数器样式）</font>。

  其中符号包含unicode编码，示例：

  ```css
  list-style-type: "\1F44D"; // thumbs up sign
  ```

  摘自：[MDN - list-style-type](https://developer.mozilla.org/zh-CN/docs/Web/CSS/list-style-type)

- **list-style-image：**

  list-style-image 属性用来指定一个<font color=FF0000>能用来作为列表元素标记的图片</font>。

  **数值**

  - \<url>：用来作为标记的图片的地址。
  - none：说明没有图片被用作标记。如果这个值被设定，那么 list-style-type 中定义的值会被取代。

  摘自：[MDN - list-style-image](https://developer.mozilla.org/zh-CN/docs/Web/CSS/list-style-image)

- **list-style-position：**

  list-style-position 属性<font color=FF0000>指定标记框在主体块框中的位置</font>。

  **数值：**

  - outside：标记盒在主块盒的外面。
  - inside：标记盒是主要块盒中的第一个行内盒，处于元素的内容流之后。

  摘自：[MDN - list-style-position](https://developer.mozilla.org/zh-CN/docs/Web/CSS/list-style-position)

摘自：[MDN - list-style](https://developer.mozilla.org/zh-CN/docs/Web/CSS/list-style) 



#### :target 伪类

:target CSS 伪类 代表一个唯一的页面元素（目标元素），其 id 与当前 <font color=FF0000>URL片段</font>（section，也可以被称为锚点？）匹配 

**解释起来比较麻烦，直接看如下示例，做下实验：**

```html
<h3>Table of Contents</h3>
<ol>
 <li><a href="#p1">Jump to the first paragraph!</a></li>
 <li><a href="#p2">Jump to the second paragraph!</a></li>
 <li><a href="#nowhere">This link goes nowhere, because the target doesn't exist.</a></li>
</ol>

<h3>My Fun Article</h3>
<p id="p1">You can target <i>this paragraph</i> using a URL fragment. Click on the link above to try out!</p>
<p id="p2">This is <i>another paragraph</i>, also accessible from the links above. Isn't that delightful?</p>

<!-- 样式 -->
<style>
p:target {
  background-color: gold;
}

/* 在目标元素中增加一个伪元素*/
p:target::before {
  font: 70% sans-serif;
  content: "►";
  color: limegreen;
  margin-right: .25em;
}

/*在目标元素中使用italic样式*/
p:target i {
  color: red;
}
</style>
```

被选中的列表项会导致下方的文字高亮

摘自：[MDN - :target](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:target) ，同样的示例也可以参考：[简书 - target伪类](https://www.jianshu.com/p/487a6f38036d)



#### ::target-text

配合 `#:~:text=content` 使用，用于分享 <font color=FF0000>带有 *锚点* 的 url</font> ，设置 content 内容的样式，从让阅读者轻松定位 content 部分。

详见：[基于文字的URL锚点定位与::target-text样式设置](https://www.zhangxinxu.com/wordpress/2022/06/url-anchor-target-text/)



#### :is() 伪类选择器

> 🧪 ：这是一个实验中的功能

`:is()` CSS <font color=FF0000>**伪类函数** 将选择器列表作为参数</font>，并选择该列表中任意一个选择器可以选择的元素。这对于以更紧凑的形式编写大型选择器非常有用。

注意，<font color=red>许多浏览器通过一个 **更旧的**、**带前缀** 的伪类 `:any()` 来支持这个功能，包括旧版本的 Chrome、Firefox 和 Safari</font>。这与 `:is()` 的工作方式完全相同，只是它需要厂商前缀，不支持复杂的选择器。

摘自：[MDN - :is() (:matches(), :any())](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:is)

##### 示例

不用 `:is` 伪类的写法：

```css
li a, artcile a, section a {
  color: #000000;
}
h1 a, h2 a, h3 a {
  color: blue;
}
```

使用 `:is` 伪类的写法：

```css
:is(li, article, p) a {
  color: #999999;
}
:is(h1, h2, h3) a {
  color: green;
}
```

<font color=dodgerblue>`:is()` 也可以用于各种选择器的组合中，例如子节点选择器、邻居节点选择器等</font>，下边的代码展示了选择子节点的方式：

```css
:is(article, p) :is(h2, li) a {
  color: #ff3344;
}
```

展开就等于：

```css
article h2 a, article li a, p h2 a, p li a {
  color: #ff3344;
}
```

**与其它伪类选择器结合**

假设有一个需求，当一个文章所有的 h1 - h6 标题，hover 的时候，在后面加上一个 "#"，如果使用传统的方式，我们会写成：

```css
h1:hover::after, h2:hover::after, h3:hover::after,
h4:hover::after, h5:hover::after, h6:hover::after {
  content: "#";
}
```

如果使用 `:is()` 伪类选择器，则可以写成：

```css
:is(h1, h2, h3, h4, h5, h6):hover::after {
  content: "#";
}
```

摘自：[CSS :is() 伪类选择器使用指南](https://zxuqian.cn/css-is-pseudo-class-selector/)

#### :where()

`:where()` <font color=FF0000>**CSS 伪类函数** 接收 ***选择器列表*** 作为它的参数</font>，将会 选择 所有能被 该选择器列表 中任何一条规则选中的元素。

```css
/* Selects any paragraph inside a header, main or footer element that is being hovered */
:where(header, main, footer) p:hover {
  color: red;
  cursor: pointer;
}

/* The above is equivalent to the following */
header p:hover,
main p:hover,
footer p:hover {
  color: red;
  cursor: pointer;
}
```

<font color=dodgerBlue>`:where()` 和 `:is()` 的不同之处在于</font>：<font color=fuchsia>`:where()` 的优先级总是为 0</font> ，但是 `:is()` 的优先级是由 <font color=fuchsia>**它的选择器列表中优先级最高的选择器决定的**</font>

摘自：[MDN - :where()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:where) 、[MDN US - :where()](https://developer.mozilla.org/en-US/docs/Web/CSS/:where)

#### :indeterminate

`:indeterminate` <font color=FF0000>CSS 伪类</font> 表示状态不确定的表单元素

表单选择框元素 ( checkbox ) 状态样式如下：

<img src="https://i.loli.net/2021/02/23/gOs3ADKmN6GWerE.png" alt="https://i2.wp.com/css-tricks.com/wp-content/uploads/2016/12/indeterminate-checkboxes.png?w=1430&ssl=1" style="zoom: 50%;" />

`:indeterminate` 可作用的对象有：

- `<input type="checkbox">` 元素，其 indeterminate 属性被 JavaScript设置为 true 。
- `<input type="radio">` 元素, 表单中拥有相同 name值的所有单选按钮都未被选中时。
- 处于不确定状态的 `<progress>` 元素

摘自：[MDN - :indeterminate](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:indeterminate) / [css-tricks - :indeterminate](https://css-tricks.com/almanac/selectors/i/indeterminate/)



#### :host

:host CSS伪类<font color=FF0000>选择包含其内部使用的CSS的shadow DOM的根元素</font>。换句话说，这<font color=FF0000>允许你从其shadow DOM中选择一个自定义元素</font>。

**注意：**<font color=FF0000>在shadow DOM之外使用时，这没有任何效果</font>。

**示例：**

```css
/* Selects a shadow root host */
:host {
  font-weight: bold;
}
```

摘自：[MDN - :host](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:host)

#### :host()

CSS 伪类函数 :host() <font color=FF0000>选择包含使用这段 CSS 的 Shadow DOM 的影子宿主</font>（这样就可以从 Shadow DOM 中选择包括它的自定义元素）——<font color=FF0000>但前提是该函数的参数与选择的阴影宿主相匹配</font>。

最简单的用法是仅将类名放在某些自定义元素实例上，然后将相关的类选择器作为函数参数包含在内。<font color=FF0000>不能将它与后代选择器表达式一起使用，以**仅选择特定祖先内部的自定义元素的实例**。这是 :host-context() 的作用</font>。

**注意：**<font color=FF0000 size=4>**在shadow DOM之外使用时，这没有任何效果**</font>。

**示例：**

```css
/* 选择阴影根元素，仅当它与选择器参数匹配 */
:host(.special-custom-element) {
  font-weight: bold;
}
```

摘自：[MDN - :host()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:host())

#### :host-context()

:host-context() CSS 伪类的作用是选择shadow DOM 中shadow host，这个伪类内可以写关于该shadow host的CSS规则。 (我们可以从 shadow DOM 中选择一个自定义元素 custom element) — 但前提是，在DOM 层级中，括号中的选择器参数必须和shadow host 的祖先相匹配。

**注意：**<font color=FF0000 size=4>**该伪类的css样式在 shadow DOM 之外的元素上是没有效果的**</font>

典型的使用方法是后代选择器表达式。例如 h1， <font color=LightSeaGreen>只选择在\<h1> 内的自定义元素的实例</font>。

```css
/* 选择了一个 shadow root host, 当且仅当这个
 shadow root host 是括号中选择器参数(h1)的后代 */
:host-context(h1) {
  font-weight: bold;
}

:host-context(main article) {
  font-weight: bold;
}
```

摘自：[MDN - :host-context()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:host-context())



#### outline

<font color=red>CSS 的 outline 属性是在一条声明中设置多个轮廓属性的简写属性</font> ， 例如 outline-style, outline-width 和 outline-color。 

**border 和 outline：**border 和 outline 很类似，但有如下区别：

- outline<font color=FF0000>不占据空间</font>，绘制于元素内容周围。
- 根据规范，outline通常是矩形，但也可以是非矩形的。

摘自：[MDN - outline](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline)

补充：页面上，输入框如果被聚焦，会出现 outline，这时候可以使用 `outline: none` 使其消失



#### user-select

CSS 属性 user-select <font color=FF0000>控制用户能否选中文本</font>。除了文本框内，它对被载入为 chrome 的内容没有影响。

##### 形式化语法

```css
user-select: auto | text | none | contain | all
```

##### 语法

- **none：**<font color=FF0000>元素及其子元素的文本不可选中</font>。 请注意这个Selection 对象可以包含这些元素。 从Firefox 21开始， none 表现的像 -moz-none，因此可以使用 -moz-user-select: text 在子元素上重新启用选择。
- **auto：**<font color=FF0000>auto 的具体取值取决于一系列条件</font>，具体如下：
  - 在 ::before 和 ::after 伪元素上，采用的属性值是 none
  - 如果元素是可编辑元素，则采用的属性值是 contain
  - 否则，如果此元素的父元素的 user-select 采用的属性值为 all，则该元素采用的属性值也为 all
  - 否则，如果此元素的父元素的 user-select 采用的属性值为 none，则该元素采用的属性值也为 none
  - 否则，采用的属性值为 text
- **text：** <font color=LightSeaGreen>用户可以选择文本</font>
- **all：**在一个HTML编辑器中，当双击子元素或者上下文时，那么包含该子元素的最顶层元素也会被选中。
- **contain：**允许在元素内选择；但是，选区将被限制在该元素的边界之内。
- **element**（IE 专有别名）：与 contain 相同，但仅在 Internet Explorer 中受支持。

> ⚠️ ：CSS UI 4 已将 user-select 的 element 属性值重命名为 contain。

摘自：[MDN - user-select](https://developer.mozilla.org/zh-CN/docs/Web/CSS/user-select)



#### touch-action

CSS属性 `touch-action` 用于 <font color=FF0000>设置触摸屏用户如何操纵元素的区域</font>（<font color=FF0000>例如，浏览器内置的缩放功能</font>）。

```css
/* Keyword values */
touch-action: auto;
touch-action: none;
touch-action: pan-x;
touch-action: pan-left;
touch-action: pan-right;
touch-action: pan-y;
touch-action: pan-up;
touch-action: pan-down;
touch-action: pinch-zoom;
touch-action: manipulation;

/* Global values */
touch-action: inherit;
touch-action: initial;
touch-action: unset;
```

<font color=FF0000>默认情况下，平移（滚动）和缩放手势 **由浏览器专门处理**</font>。 <font color=fuchsia>使用 `pointer_events` 的应用程序将 在浏览器开始处理触摸手势时收到一个 `pointercancel` 事件</font>。 通过明确指定浏览器应该处理哪些手势，应用程序可以在 pointermove 和 pointerup 监听器中为其余的手势提供自己的行为。 使用 Touch_events 的应用程序通过调用 preventDefault() 禁用浏览器处理手势，但也应使用触摸操作确保浏览器在调用任何事件侦听器之前，了解应用程序的意图。

当手势开始时，浏览器与触摸的元素及其所有祖先的触摸动作值相交直到一个实现手势（换句话说，第一个包含滚动元素）的触摸动作值。 这意味着在实践中，触摸动作通常仅适用于具有某些自定义行为的单个元素，而无需在该元素的任何后代上明确指定触摸动作。 手势开始之后，触摸动作值的更改将不会对当前手势的行为产生任何影响。

##### 语法

**touch-action 属性可以被指定为：**

- 任何一个关键字 auto、none、manipulation，或

- 零或任何一个关键字 pan-x、pan-left、pan-right，加零或任何一个关键字 pan-y、pan-up、pan-down，加可选关键字 pinch-zoom.

##### 值

- **auto：**<font color=FF0000>当触控事件发生在元素上时，由浏览器来决定进行哪些操作</font>，比如对viewport进行平滑、缩放等。

- **none：**<font color=FF0000>当触控事件发生在元素上时，不进行任何操作</font>

- **pan-x：**<font color=FF0000>启用单指水平平移手势</font>。<mark>可以与 **pan-y 、pan-up、pan-down** 和／或 **pinch-zoom** 组合使用</mark>。

- **pan-y：**<font color=FF0000>启用单指垂直平移手势</font>。<mark>可以与 **pan-x 、pan-left 、pan-right** 和／或 **pinch-zoom** 组合使用</mark>。

- **manipulation：**<font color=FF0000>浏览器只允许进行滚动和持续缩放操作</font>。任何其它被auto值支持的行为不被支持。启用平移和缩小缩放手势，但禁用其他非标准手势，例如双击以进行缩放。 禁用双击可缩放功能可减少浏览器在用户点击屏幕时延迟生成点击事件的需要。 这是“**pan-x pan-y pinch-zoom**”（为了兼容性本身仍然有效）的别名。

- **pan-left, pan-right, pan-up, pan-down：🧪** <font color=FF0000>启用以指定方向滚动开始的单指手势</font>。 一旦滚动开始，方向可能仍然相反。 请注意，滚动“向上”（**pan-up**）意味着用户正在将其手指向下拖动到屏幕表面上，同样 **pan-left** 表示用户将其手指向右拖动。 多个方向可以组合，除非有更简单的表示（例如，“**pan-left pan-right**”无效，因为“**pan-x**”更简单，而“**pan-left pan-down**”有效）。

- **pinch-zoom：**<font color=FF0000>启用多手指平移和缩放页面</font>。 这可以与任何平移值组合。

摘自：[MDN - touch-action](https://developer.mozilla.org/zh-CN/docs/Web/CSS/touch-action)



#### autocomplete

默认情况下，浏览器会记录用户网页上提交的输入框的信息。这使得浏览器能够提供自动补全（在用户开始输入的时候给用户提供可能的内容）和自动填充（在加载的时候预先填充某些字段）功能。

这些功能通常是默认启用的，但可能涉及用户的隐私，因此浏览器允许用户禁用这些功能。

要禁用的表单自动填充，你可以将 autocomplete 的属性设置为 "off"：**autocomplete = "off"**

##### 设置 autocomplete="off" 会有两种效果：

- 这会告诉浏览器，不要为了以后在类似表单上自动填充而保存用户输入的数据。但浏览器不一定遵守。
- 这会阻止浏览器缓存会话历史记录中的数据。若表单数据缓存于会话历史记录，用户提交表单后，再点击返回按钮返回之前的表单页面，则会显示用户之前输入的数据。

##### 使用 autocomplete="new-password" 阻止自动填充

如果你定义了一个用户管理页面，其中用户可以为其他人指定新的密码，因此你<font color=fuchsia>想阻止密码字段的自动填充，你可以使用 `autocomplete="new-password"` </font>。

<font color=red>这只是一个提示，浏览器不一定要遵守</font>。但现代浏览器都已停止在设置了 `autocomplete="new-password"` 的 `<input>` 元素上使用自动填充。👀 注：这个没有试过，即没有验证过，只是在帮助同时解决问题时查到。

> **`new-password`**：新密码。 创建新帐户或更改密码时，应将其用于“输入新密码”或“确认新密码”字段，而不是通常出现的“输入当前密码”字段。 浏览器可以使用它来避免意外填写现有密码，并在创建安全密码时提供帮助
>
> 摘自：[MDN - HTML attribute: autocomplete](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Attributes/autocomplete)

摘自：[MDN - 如何关闭表单自动填充](https://developer.mozilla.org/zh-CN/docs/Web/Security/Securing_your_site/Turning_off_form_autocompletion)



#### 实现图片切换效果的两种方法

##### 用单纯的CSS

更优雅。但当hover事件发生时，似乎只能对本级元素进行修改

```html
<img src="icon_rise@2x.png" alt="" id="img" />

<style>
  #img:hover {
    content: url(icon_discharge@2x.png)
  }
</style>
```

参考自：[CSS小技巧之替换图片(content)](https://blog.csdn.net/qq_45745643/article/details/108697191)

##### JS

更灵活。当hover事件发生时，可以对任意位置的元素进行修改

```html
<img src="icon_rise@2x.png" alt="" id="img" onmouseover="changeImg()" onmouseout="resotreImg()">

<script>
  function changeImg() {
    let e = document.querySelector('#img')
    e.src = 'icon_discharge@2x.png'
  }

  function restoreImg() {
    let e = document.querySelector('#img')
    e.src = 'icon_rise@2x.png'
  }
</script>
```



#### content

content 属性与 `::before` 及 `::after` 伪元素配合使用，来插入生成内容。

**Content属性值**

|                    值                     |                             说明                             |
| :---------------------------------------: | :----------------------------------------------------------: |
|                   none                    |                设置Content，如果指定成Nothing                |
|                  normal                   | 设置content，如果指定的话，正常，默认是"none"（该是nothing） |
|                  counter                  |                        设定计数器内容                        |
| <font color=FF0000>attr(attribute)</font> |  <font color=FF0000>设置Content作为选择器的属性之一</font>   |
|                 *string*                  |                  设置Content到你指定的文本                   |
|                open-quote                 |                    设置Content是开口引号                     |
|                close-quote                |                    设置Content是闭合引号                     |
|               no-open-quote               |                 如果指定，移除内容的开始引号                 |
|              no-close-quote               |                 如果指定，移除内容的闭合引号                 |
|    <font color=FF0000>url(url)</font>     | <font color=FF0000>设置某种媒体（图像，声音，视频等内容）</font> |
|                  inherit                  |           指定的content属性的值，应该从父元素继承            |

摘自：[CSS content 属性](https://www.runoob.com/cssref/pr-gen-content.html)



#### width

width 属性用于设置元素的宽度。width 默认设置内容区域的宽度，但 <font color=LightSeaGreen>如果 box-sizing 属性被设置为 border-box，就转而设置边框区域的宽度</font>。

<font color=FF0000>min-width 和 max-width 属性的优先级高于 width</font>。

**width 属性也指定为：**

- 下面关键字值之一：min-content，max-content，fit-content，auto。
- 一个长度值 \<length> 或者百分比值 \<percentage>。

**值**

- **\<length>：**使用绝对值定义宽度。

- **\<percentage>：**使用外层元素的容纳区块宽度（the containing block's width）的百分比定义宽度。

- **auto：** <font color=LightSeaGreen>浏览器将会为指定的元素计算并选择一个宽度</font>。

- **max-content：🧪 **<font color=FF0000>元素内容固有的（intrinsic）合适宽度</font>。

- **min-content：🧪 **<font color=FF0000>元素内容固有的最小宽度</font>。

- **fit-content：🧪 **取以下两种值中的较大值：

  - 固有的最小宽度
  - 固有首选宽度（max-content）和可用宽度（available）两者中的较小值

  可表示为：min(max-content, max(min-content, \<length-percentage>))

摘自：[MDN - width](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width)

参考：[理解CSS3 max/min-content及fit-content等width值](https://www.zhangxinxu.com/wordpress/2016/05/css3-width-max-contnet-min-content-fit-content/)

#### `<length>`

长度 `<length>` 是用于表示距离尺寸的 CSS 数据类型。许多 CSS 属性会用到长度，比如 `width` 、`margin`、`padding` 、`font-size` 、`border-width` 和 `text-shadow` 。

##### 语法

`<length>` 数据类型由一个 `<number>` 和一个长度单位构成。 与所有 CSS 维度一样，单位的字面值与数字之间没有空格。 数字为 0 时，长度单位是可选的。

> 👀 由于这里单位较多，这里略；只摘录有必要的，其他见引用的链接

- vmin：视口高度 vw 和宽度 vh 两者之间的最小值。
- vmax：视口高度 vw 和宽度 vh 两者之间的最大值。

摘自：[MDN - `<length>`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/length)

> 💡 知乎上有这样一个问题：[css样式的百分比都相对于谁？ - 知乎](https://www.zhihu.com/question/36079531) 感觉很有价值，推荐阅读

#### `<color>`

CSS 数据类型 `<color>` 表示一种 [标准 RGB 色彩空间 ( sRGB color space )](http://en.wikipedia.org/wiki/SRGB)的颜色。一个颜色可以包括一个 alpha  通道透明度值，来表明颜色如何与它的背景色[混合 ( composite )](https://www.w3.org/TR/2003/REC-SVG11-20030114/masking.html#SimpleAlphaBlending)。

<font color=dodgerBlue>一个 `<color>` 可以以如下方式定义：</font>

- 使用一个关键字（比如 `blue` 或 `transparent` ）
- 使用 [RGB 立体坐标 ( RGB cubic-coordinate )](http://en.wikipedia.org/wiki/RGB_color_model#Geometric_representation)系统（以“#”加十六进制或者 `rgb()` 和 `rgba()` 函数表达式的形式）
- 使用 [HSL  圆柱坐标 ( HSL cylindrical-coordinate )](http://en.wikipedia.org/wiki/HSL_and_HSV)系统（以 `hsl()` 和 `hsla()` 函数表达式的形式）

> 💡 **备注：** 本文章详细描述了`<color>`数据类型。如要了解更多关于在 HTML 中使用颜色的信息，请参阅[使用 CSS 为 HTML 元素应用颜色](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_colors/Applying_color)。

##### 语法

`<color>` 可以以以下方式指定

###### 颜色关键字

颜色关键字 ( color keywords ) 是不区分大小写的标识符，它表示一个具体的颜色，例如 `red`、`blue`、`brown` 或者 `lightseagreen` 。尽管名称或多或少描述了分别的颜色，但必定是人工的，其后没有严格的标准。

<font color=dodgerBlue>在使用关键字时有几个需要留意的注意事项：</font>

- <font color=red>除了通常的 16 个 HTML 基本颜色，其他的不能被用于 HTML</font>。HTML 将通过一个特定的计算程序转换这些未知的值，这将导致成为完全不同的颜色。这些关键字应只被用于 SVG 和 CSS。
- 未知的关键字会让 CSS 属性无效。无效的属性将被忽略，该颜色将没有作用。这是一个和 HTML 相比不同的行为。
- 未使用关键字定义的颜色在 CSS 中有任意的透明度，它们是单实色。
- <font color=dodgerBlue>一些关键字表示同样的颜色：</font>
  - `darkgray` / `darkgrey`
  - `darkslategray` / `darkslategrey`
  - `dimgray` / `dimgrey`
  - `lightgray` / `lightgrey`
  - `lightslategray` / `lightslategrey`
  - `gray` / `grey`
  - `slategray` / `slategrey`
- 虽然关键字的名称取自常见的 X11 颜色名，然而由于生产商为具体的硬件所做的定制，颜色可能与 X11 系统上相应的颜色有所偏差。

###### `transparent` 关键字

`transparent` 关键字表示一个完全透明的颜色，即该颜色看上去将是背景色。从技术上说，它是带有阿尔法通道为最小值的黑色，<font color=red>**是 `rgba(0, 0, 0, 0)` 的简写**</font>。

###### `currentColor` 关键字

`currentColor` 关键字<font color=red>代表**原始的 `color` 属性的计算值**</font>。它<font color=red>允许让 **继承自属性** 或 **子元素的属性** 颜色属性以默认值不再继承</font>。

> 👀 没太看懂这句话，不过结合下面示例，有点明白了：currentColor 不仅可以在 ( 设置 color 的元素）的子元素中使用，也可以在元素自身中使用；具体结合下面示例。

它也能用于那些继承了元素的 `color` 属性计算值的属性，相当于在这些元素上使用 `inherit` 关键字，如果这些元素有该关键字的话。

**示例如下**

```html
<!-- 👀 注意这里的 border 的 curentcolor -->
<div style="color: blue; border: 1px dashed currentcolor;">
  The color of this text is blue.
  <div style="background: currentcolor; height: 9px;"></div>
  This block is surrounded by a blue border.
</div>
```

<img src="https://s2.loli.net/2023/07/08/jAJHmSMKck1Y4tp.png" alt="image-20230708142627482" style="zoom:50%;" />

> 💡 有点好奇的发现一个问题，对上面的代码做了如下修改：
>
> ```diff
> - <div style="color: blue; border: 1px dashed currentcolor;">
> + <div style="color: currentcolor; border: 1px dashed blue;">
>     The color of this text is blue.
>     <div style="background: currentcolor; height: 9px;"></div>
>     This block is surrounded by a blue border.
>   </div>
> ```
>
> 样式变成了如下，也就是说：元素本身 currentColor 生效了，但是子元素没有生效
>
> <img src="https://s2.loli.net/2023/07/08/N5trRmQAFxyv3qZ.png" alt="image-20230708143328104" style="zoom:50%;" />

###### RGB 颜色

颜色可以使用红 - 绿 - 蓝 ( red-green-blue (RGB) ) 模式的两种方式被定义：

**语法**

RGB 颜色可以通过以`#`为前缀的十六进制字符和函数 ( `rgb()`、`rgba()` ) 标记表示。

> 💡 **备注：** 在 CSS 颜色标准 4 中，`rgba()` 是 `rgb()` 的别称。在实行第 4 级标准的浏览器中，它们接受相同的参数，作用效果也相同。

- 十六进制符号：`#RRGGBB[AA]` 。`R`（红）、`G`（绿）、`B` （蓝）和`A` （alpha）是十六进制字符（0–9、A–F）。`A` 是可选的。比如，`#ff0000` 等价于 `#ff0000ff` 。
- 十六进制符号：`#RGB[A]` 。`R`（红）、`G`（绿）、`B` （蓝）和 `A`  ( alpha ) 是十六进制字符（0–9、A–F）。`A`是可选的。三位数符号 ( `#RGB` )是六位数形式 ( `#RRGGBB` ) 的减缩版。比如，<font color=lightSeaGreen>**`#f09` 和 `#ff0099`表示同一颜色**</font>。类似地，<font color=red>四位数符号 ( `#RGBA` ) 是八位数形式 ( `#RRGGBBAA` ) 的减缩版</font>。比如，`#0f38` 和 `#00ff3388` 表示相同颜色。
- 函数符： `rgb[a](R, G, B[, A])` 。`R`（红）、`G`（绿）、`B` （蓝）可以是 `<number>`（数字），或者`<percentage>`（百分比），255 相当于 100%。`A` ( alpha ) 可以是 `0` 到 `1` 之间的数字，或者百分比，数字`1`相当于`100%`（完全不透明）。
- 函数符：`rgb[a](R G B[ / A])` 。CSS 颜色级别 4 支持用空格分开的值。

###### HSL 颜色

颜色也可以使用 `hsl()` 函数符被定义为色相 - 饱和度 - 亮度 ( Hue-saturation-lightness ) 模式。<font color=lightSeaGreen>HSL 相比 RGB 的优点是更加直观</font>：你可以估算你想要的颜色，然后微调。它也更易于创建相称的颜色集合。（通过保持相同的色相并改变亮度/暗度和饱和度）。

Many designers find HSL more intuitive than RGB, since it allows hue, saturation, and lightness to each be adjusted independently. HSL can also make it easier to create a set of matching colors (such as when you want multiple shades of a single hue).

**语法**

HSL colors are expressed through the functional `hsl()` and `hsla()` notations.

> 💡 **备注：** As of CSS Colors Level 4, `hsla()` is an alias for `hsl()`. In browsers that implement the Level 4 standard, they accept the same parameters and behave the same way.

- Functional notation: `hsl[a](H, S, L[, A])`

  `H` (hue) is an `<angle>` of the color circle given in `deg`s, `rad`s, `grad`s, or `turn`s in [CSS Color Module Level 4](https://drafts.csswg.org/css-color/#the-hsl-notation). When written as a unitless `<number>`, it is interpreted as degrees, as specified in [CSS Color Module Level 3](https://drafts.csswg.org/css-color-3/#hsl-color). By definition, red=0deg=360deg, with the other colors spread around the circle, so green=120deg, blue=240deg, etc. As an `<angle>`, it implicitly wraps around such that -120deg=240deg, 480deg=120deg, -1turn=1turn, etc. `S` (saturation) and `L`(lightness) are percentages. `100%` **saturation** is completely saturated, while `0%` is completely unsaturated (gray). `100%` **lightness** is white, `0%` lightness is black, and `50%` lightness is “normal.” `A` (alpha) can be a `<number>` between `0` and `1`, or a `<percentage>`, where the number `1` corresponds to `100%` (full opacity).

- Functional notation: `hsl[a](H S L[ / A])`

  CSS Colors Level 4 adds support for space-separated values in the functional notation.

> 👀 这里下面还有内容，由于没怎么看懂，所以略。

##### 校验某段字符串是否是有效的颜色

> 👀 这串代码来自 [MDN - `<color>` # 颜色值检测器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/color_value#颜色值检测器) 中的 playground

```js
function validTextColor(stringToTest) {
  if (stringToTest === "") return false;
  if (stringToTest === "inherit") return false;
  if (stringToTest === "transparent") return false;

  const image = document.createElement("img");
  
  image.style.color = "rgb(0, 0, 0)";
  image.style.color = stringToTest;
  if (image.style.color !== "rgb(0, 0, 0)") return true;
  
  image.style.color = "rgb(255, 255, 255)";
  image.style.color = stringToTest;
  return image.style.color !== "rgb(255, 255, 255)";
}
```

感觉主要是在通过检测 “给 img 实例添加 color 是否生效” 来判断该 color 是否为有效的颜色。有种“爱迪生用水测量电灯泡体积”的感觉。

摘自：[MDN - `<color>`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/color_value)



#### accent-color

CSS 属性 **`accent-color`** 为某些元素所生成的用户界面控件设置了强调色。

支持 `accent-color` 属性的浏览器目前将其应用于下列 HTML 元素：

- `<input type="checkbox">`
- `<input type="radio">`
- `<input type="range">`
- `<progress>`

##### 效果示例

> 👀 看一下效果示例，就知道这个属性是干什么的了。这个类似于 vant 中 radio 和 checkbox 中的 `color` prop，虽然这些组件是模拟的

<img src="https://s2.loli.net/2023/08/29/3Em7C4RX9WBuMJF.png" alt="image-20230829100024460" style="zoom:40%;" />

##### 语法

```css
/* 关键字值 */
accent-color: auto;

/* <color> 值 */
accent-color: darkred;
accent-color: #5729e9;
accent-color: rgb(0 200 0);
accent-color: hsl(228 4% 24%);

/* 全局值 */
accent-color: inherit;
accent-color: initial;
accent-color: revert;
accent-color: revert-layer;
accent-color: unset;
```

###### 取值

- `auto` ：表示用户代理所选颜色，应匹配平台的强调色（若有）。
- `<color>` ：指定用作强调色的颜色。

##### 形式定义

| 初始值         | `auto`                                                       |
| :------------- | ------------------------------------------------------------ |
| 适用元素       | all elements                                                 |
| 是否是继承属性 | yes                                                          |
| 计算值         | `auto` is computed as specified and `<color>` values are computed as defined for the `color` property. |
| Animation type | by computed value type                                       |

摘自：[MDN - accent-color](https://developer.mozilla.org/zh-CN/docs/Web/CSS/accent-color)



#### object-fit

<font color=FF0000>object-fit CSS 属性**指定 <font size=4>可替换元素</font>**</font>（这个名词，隔壁的《CSS权威指南阅读笔记》有解释，见 [[CSS权威指南（第四版）阅读笔记#置换元素]] 。另外，值得补充的是：可替换元素的外观是 CSS 所难以控制的，比如 `<img>` 的内容是有 `src` 控制的）<font color=FF0000>的内容应该如何适应到其使用的高度和宽度确定的框。</font>
<font color=dodgerBlue>您可以通过使用 object-position 属性来切换被替换元素的内容对象在元素框内的对齐方式。</font>

##### 取值

- **`contain`** ：<font color=FF0000>被替换的内容将被缩放，以在填充元素的内容框时保持其宽高比</font>。 整个对象在填充盒子的同时保留其长宽比，因此如果宽高比与框的宽高比不匹配，该对象将被添加“黑边”。
- **`cover`** ：被替换的内容在<font color=FF0000>保持其宽高比的同时填充元素的整个内容框</font>。如果对象的宽高比与内容框不相匹配，该对象将被剪裁以适应内容框。
- **`fill`** ：<font color=FF0000>（默认）</font><font color=FF0000>被替换的内容正好填充元素的内容框</font>。整个对象将完全填充此框。<font color=FF0000>如果对象的宽高比与内容框不相匹配，那么 <font size=4>**该对象将被拉伸以适应内容框**</font></font>。
- **`none`** ：被替换的内容将<font color=FF0000>保持其原有的尺寸</font>。
- **`scale-down`** ：<font color=FF0000>内容的尺寸与 `none` 或 `contain` 中的一个相同，取决于它们两个之间谁得到的对象尺寸会更小一些</font>。

摘自：[MDN - object-fit](https://developer.mozilla.org/zh-CN/docs/Web/CSS/object-fit)

> 💡 经过实践发现：`object-fit` 可以设置图片的展示效果。这在之前是使用 `background-image` + `background-size` 去实现的，现在发现 `object-fit` 同样可以。

另外，参考文章：[张鑫旭 - 半深入理解CSS3 object-position/object-fit属性](https://www.zhangxinxu.com/wordpress/2015/03/css3-object-position-object-fit/)

#### object-position

CSS 属性 object-position 规定了 <font color=FF0000 size=4>**可替换元素**</font> 的内容，在这里我们称其为对象（即 object-position 中的 object），在其内容框中的位置。可替换元素的内容框中未被对象所覆盖的部分，则会显示该元素的背景（background）。

摘自：[MDN - object-position](https://developer.mozilla.org/zh-CN/docs/Web/CSS/object-position)



#### pointer-events

pointer-events 是 CSS3 的一个属性，<font color=FF0000>指定在什么情况下元素可以成为鼠标事件的 target</font>（包括鼠标的样式）

<font color=dodgerblue>pointer-events 属性有很多值</font>，但是<font color=FF0000>对于浏览器来说，只有 auto 和 none 两个值可用</font>，<font color=lightSeaGreen>其它的几个是针对 SVG 的（本身这个属性就来自于 SVG 技术）</font>。

- **auto（默认）：**效果和没有定义 pointer-events 属性相同（即默认），<font color=FF0000>鼠标不会穿透当前层</font>。在 SVG 中，该值和 visiblePainted 的效果相同
- **none：**<font color=FF0000>元素永远不会成为鼠标事件的 target（目标）。但是，当其后代元素的 pointer-events 属性指定其他值时，鼠标事件可以指向后代元素，在这种情况下，鼠标事件将在捕获或冒泡阶段触发父元素的事件侦听器。</font>

摘自：[非常有用的pointer-events属性](https://www.cnblogs.com/kunmomo/p/11752669.html)  👀 该博客还有一些 pointer-events 的使用示例，比如地图工具栏，很详细地说明了使用场景



#### will-change

CSS 属性 will-change <font color=red>为 web 开发者提供了一种</font> <font color=fuchsia>告知浏览器该元素会有 **哪些变化的方法**</font>（👀 will-change 的值 *可以是* CSS 属性，比如 `will-change: filter` ，当然，在这种情况下，CSS 样式中要包含 filter 属性。另外，参考下 [[#用好这个属性并不是很容易]] 中的 “这个属性是用来让页面开发者告知浏览器哪些属性可能会变化的”），这样 <font color=red>浏览器可以在元素属性真正发生变化之前提前做好对应的优化准备工作</font>。 <font color=lightSeaGreen>这种优化可以将一部分复杂的计算工作提前准备好，使页面的反应更为快速灵敏</font>。

##### 用好这个属性并不是很容易

- **不要将 will-change 应用到太多元素上**：浏览器已经尽力尝试去优化一切可以优化的东西了。有一些更强力的优化，如果与 will-change 结合在一起的话，有可能会消耗很多机器资源，如果过度使用的话，可能导致页面响应缓慢或者消耗非常多的资源。
- **有节制地使用**：<font color=fuchsia> **通常**，当元素恢复到初始状态时，**浏览器会丢弃掉之前做的优化工作**</font>。但是<font color=FF0000> **如果直接在样式表中显式声明了 will-change 属性，则表示目标元素可能会经常变化，浏览器会将优化工作保存得比之前更久**</font>（👀 即：浏览器将对应的优化工作一直保存在内存中）。所以最佳实践是当元素变化之前和之后通过脚本来切换 will-change 的值。
- <font color=FF0000> **不要过早应用 will-change 优化：**</font>如果你的页面在性能方面没什么问题，则不要添加 will-change 属性来榨取一丁点的速度。 <font color=FF0000> will-change 的设计初衷是作为最后的优化手段，用来尝试解决现有的性能问题。它**不应该被用来预防性能问题**</font>。<font color=lightSeaGreen>过度使用 will-change 会导致大量的内存占用，并会导致更复杂的渲染过程，因为浏览器会试图准备可能存在的变化过程。这会导致更严重的性能问题</font>。
- **给它足够的工作时间：**<font color=fuchsia> 这个属性是用来 让页面开发者告知浏览器 <font size=4>**哪些属性可能会变化的**</font></font>（👀 这句话是重点 ⭐️） 。然后<font color=red>浏览器可以选择在变化发生前提前去做一些优化工作。所以给浏览器一点时间去真正做这些优化工作是非常重要的</font>。使用时需要尝试去找到一些方法提前一定时间获知元素可能发生的变化，然后为它加上 will-change 属性。

| 属性           | 值           |
| :------------- | ------------ |
| 初始值         | auto         |
| 适用元素       | all elements |
| 是否是继承属性 | 否           |
| 计算值         | as specified |
| Animation type | discrete     |

##### 语法

```css
auto | <animateable-feature>
where 
<animateable-feature> = scroll-position | contents | <custom-ident>
```

###### 取值

- **auto：**表示没有特别指定哪些属性会变化，<font color=FF0000> **浏览器需要自己去猜**，然后使用浏览器经常使用的一些常规方法优化</font>。
- **\<animateable-feature> 可以是以下值**：
  - **scroll-position** ：表示开发者希望在<font color=FF0000> 不久后 **改变滚动条的位置**，或者使之产生动画</font>。
  - **contents** ：表示开发者希望在<font color=FF0000> 不久后 **改变元素内容中的某些东西**，或者使它们产生动画</font>。
  - **\<custom-ident>** ：表示开发者希望在<font color=FF0000> 不久后改变指定的属性名或者使之产生动画</font>。如果属性名是简写，则代表所有与之对应的简写或者全写的属性。

```css
will-change: auto
will-change: scroll-position
will-change: contents
will-change: transform        // Example of <custom-ident>
will-change: opacity          // Example of <custom-ident>
will-change: left, top        // Example of two <animateable-feature>

will-change: unset
will-change: initial
will-change: inherit
```

摘自：[MDN - will-change](https://developer.mozilla.org/zh-CN/docs/Web/CSS/will-change)

> 👀 will-change 可以用于 防止 回流和重绘，相关见 [[前端面试点总结#减少 回流 和 重绘 的操作]] 中的 “使用硬件 ( GPU ) 加速”



#### text-decoration

text-decoration 这个 CSS 属性是用于设置文本的修饰线外观的（下划线、上划线、贯穿线/删除线  或 闪烁）它是 text-decoration-line, text-decoration-color, text-decoration-style, 和新出现的 text-decoration-thickness 属性的缩写。

文本修饰属性会延伸到子元素。这意味着如果祖先元素指定了文本修饰属性，子元素则不能将其删除。

##### 属性

- **text-decoration-line：**文本修饰的位置, 如下划线underline，删除线line-through
- **text-decoration-color：**文本修饰的颜色
- **text-decoration-style：**文本修饰的样式, 如波浪线wavy实线solid虚线dashed
- **text-decoration-thickness：**文本修饰线的粗细

摘自：[MDN - text-decoration](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration)



#### text-transform

The **`text-transform`** CSS property <font color=red>specifies how to capitalize an element's text</font>. It can be <font color=LightSeaGreen>used to make text appear in all-uppercase or all-lowercase</font>, or <font color=LightSeaGreen>with each word capitalized</font>. It also can help improve legibility for ruby.

> 👀 这里省略掉了一些 “考虑特定于语言的案例映射规则” 和 “在其他一些其他特定的情况下，映射规则不被任何浏览器考虑在内”，因为暂时用不到；这里略。详见原链接。

| 值             | 属性                                                         |
| :------------- | ------------------------------------------------------------ |
| 初始值         | `none`                                                       |
| 适用元素       | all elements. It also applies to `::first-letter` and `::first-line`. |
| 是否是继承属性 | yes                                                          |
| 计算值         | as specified                                                 |
| Animation type | discrete                                                     |

##### 语法

```css
/* Keyword values */
text-transform: capitalize;
text-transform: uppercase;
text-transform: lowercase;
text-transform: none;
text-transform: full-width;

/* Global values */
text-transform: inherit;
text-transform: initial;
text-transform: unset;
```

###### 值

- `capitalize`：这个关键字<font color=red>强制每个单词的首字母转换为大写</font>。其他的字符保留不变（它们写在元素里的文本保留原始大小写）。字母是 Unicode 字符集或者数字里定义的字符 🧪；因此单词开头的任何标点符号或者特殊符号将会被忽略。

  > Authors should not expect `capitalize` to follow language-specific titlecasing conventions (such as skipping articles in English).

- `uppercase`：这个关键字<font color=red>强制所有字符被转换为大写</font>。

- `lowercase`：这个关键字<font color=red>强制所有字符被转换为小写</font>。

- `none`：这个关键字<font color=red>阻止所有字符的大小写被转换</font>。

- `full-width` ：🧪 这个关键字强制字符 — 主要是表意字符和拉丁文字 — 书写进一个方形里，并允许它们按照一般的东亚文字（比如中文或日文）对齐。

摘自：[MDN US - text-transform](https://developer.mozilla.org/en-US/docs/Web/CSS/text-transform)、[MDN - text-transform](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-transform)



#### -webkit-text-stroke

>  👀 一般用来实现文字描边。参见 [49 个在工作中常用且容易遗忘的 CSS 样式清单整理 ](https://www.cnblogs.com/CIBud/p/15004924.html) 第41条

##### 摘要

**`-webkit-text-stroke`** CSS属性为文本字符指定了宽 和 颜色 . 它是 [`-webkit-text-stroke-width`](https://developer.mozilla.org/en-US/docs/Web/CSS/-webkit-text-stroke-width) 和 [`-webkit-text-stroke-color`](https://developer.mozilla.org/en-US/docs/Web/CSS/-webkit-text-stroke-color) 属性的缩写。

##### 语法

```css
/* 宽度和颜色属性 */
-webkit-text-stroke: 4px navy;

/* 全局属性 */
-webkit-text-stroke: inherit;
-webkit-text-stroke: initial;
-webkit-text-stroke: unset;
```

##### 值

- ###### `<length>` ：文本宽。
- `<color>` ：文本颜色。

##### 常规用法

```css
/* 设置宽度和颜色 */
-webkit-text-stroke: <length> <color>;

/* 默认设置 */
-webkit-text-stroke: inherit/initial/unset;
```

摘自：[MDN - -webkit-text-stroke](https://developer.mozilla.org/zh-CN/docs/Web/CSS/-webkit-text-stroke)



#### white-space

**white-space CSS 属性是用来设置如何处理元素中的 空白（即：`␣`）。**

**可取值**

- **normal：**<font color=FF0000>连续的空白符会被合并</font>，<font color=FF0000>换行符会被当作空白符来处理</font>。换行在填充「行框盒子(line boxes)」时是必要。
- **nowrap：**和 normal 一样，连续的空白符会被合并。但文本内的换行无效。
- **pre：**<font color=FF0000>连续的空白符会被保留。在遇到换行符或者\</br>元素时才会换行</font>。 
- **pre-wrap：**<font color=FF0000>连续的空白符会被保留</font>。在遇到换行符或者\</br>元素，或者需要为了填充「行框盒子(line boxes)」时才会换行。
- **pre-line：**<font color=FF0000>连续的空白符会被合并</font>。在遇到换行符或者\</br>元素，或者需要为了填充「行框盒子(line boxes)」时会换行。
- **break-spaces：**与 pre-wrap的行为相同，除了：
  - 任何保留的空白序列总是占用空间，包括在行尾。
  - 每个保留的空格字符后都存在换行机会，包括空格字符之间。
  - 这样保留的空间占用空间而不会挂起，从而影响盒子的固有尺寸（最小内容大小和最大内容大小）。

**下面的表格总结了各种 white-space 值的行为：**

|              | 换行符 | 空格和制表符 | 文字换行 | 行尾空格 |
| :----------: | :----: | :----------: | :------: | :------: |
|    normal    |  合并  |     合并     |   换行   |   删除   |
|    nowrap    |  合并  |     合并     |  不换行  |   删除   |
|     pre      |  保留  |     保留     |  不换行  |   保留   |
|   pre-wrap   |  保留  |     保留     |   换行   |   挂起   |
|   pre-line   |  保留  |     合并     |   换行   |   删除   |
| break-spaces |  保留  |     保留     |   换行   |   换行   |

摘自：[MDN - white-space](https://developer.mozilla.org/zh-CN/docs/Web/CSS/white-space)



#### text-align

text-align CSS属性定义行内内容（例如文字）<font color=FF0000>如何相对它的块父元素对齐</font>。text-align 并不控制块元素自己的对齐，只控制它的行内内容的对齐。

##### 可选值

- **start：**如果内容方向是左至右（ltr），则等于left，反之（rtl）则为right。
- **end：**如果内容方向是左至右（ltr），则等于right，反之（rtl）则为left。
- **left：**行内内容向左侧边对齐。
- **right：**行内内容向右侧边对齐。
- **center：**行内内容居中。
- **\<string>：**第一个出现的该（单字符）字符串被用来对齐。跟随的关键字定义对齐的方向。例如，可用于让数字值根据小数点对齐。
- **justify：**<font color=FF0000>文字向两侧对齐，对最后一行无效。</font>
- **justify-all：**和justify一致，但是强制使最后一行两端对齐。
- **match-parent：** 和inherit类似，区别在于start和end的值根据父元素的direction确定，并被替换为恰当的left或right。

摘自：[MDN - text-align](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-align)

#### text-align-last

CSS 属性 text-align-last  <font color=FF0000>描述的是一段文本中最后一行在被强制换行之前的对齐规则</font>。

##### 属性值

- **auto：**每一行的对齐规则由 text-align 的值来确定，当 text-align 的值是 justify，text-align-last 的表现和设置了 start 的表现是一样的，即如果文本的展示方向是从左到右，则最后一行左侧对齐与内容盒子。
  译者注：经测试，当 text-align 的值为 right，并且 text-align-last 设置为 auto 时，文本最后一行的对齐方式相当于 text-align-last 被设置为 right 时的效果。<font color=FF0000>即 text-align-last 设置为 auto 后的表现跟 text-align 的设置有关</font>。

- **start：**与 direction 的设置有关。
  - 如果文本展示方向是从左到右，起点在左侧，则是左对齐；
  - 如果文本展示方向是从右到左，起点在右侧，则是右对齐。
  - 如果没有设置 direction ，则按照浏览器文本的默认显示方向来确定。

- **end：**与 direction 的设置有关。
  - 如果文本展示方向是从左到右，末尾在右侧，则是右对齐；
  - 如果文本展示方向是从右到左，末尾在左侧，则是左对齐。
  - 如果没有设置 direction ，则按照浏览器文本的默认显示方向来确定。
- **left：**最后一行文字与内容盒子的左侧对齐
- **right：**最后一行文字与内容盒子的右侧对齐
- **center：**最后一行文字与内容盒子居中对齐
- **justify：**最后一行文字的开头与内容盒子的左侧对齐，末尾与右侧对齐。

摘自：[MDN - text-align-last](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-align-last)



#### vertical-align

CSS 的属性 vertical-align <font color=FF0000>用来指定 <font size=4>**行内元素 ( inline )**</font> 或 <font size=4>**表格单元格 ( table-cell )**</font> 元素的垂直对齐方式</font>。

##### vertical-align属性可被用于两种环境

- 使 <font color=FF0000>**行内元素盒模型**</font> 与其 <font color=FF0000>**行内元素容器**</font> 垂直对齐。例如，用于垂直对齐一行文本内的图片

  <img src="https://i.loli.net/2021/08/20/qbGZoYeHary5Aus.png" alt="image-20210820110216620" style="zoom: 33%;" />

- 垂直对齐表格单元内容:

  <img src="https://i.loli.net/2021/08/20/uYdcp1AFb2vHUqK.png" alt="image-20210820110341093" style="zoom: 33%;" />

**注意：**<font color=FF0000>**vertical-align 只对行内元素、表格单元格元素生效**：不能用它垂直对齐块级元素</font>。

##### vertical-align 属性指定为下面列出的值之一

- **行内元素的值**
  - **<font color=FF0000 size=4>相对父元素</font> 的值：**这些值使元素相对其父元素垂直对齐：
    - **baseline：**<font color=FF0000>使元素的基线与父元素的基线对齐</font>。HTML规范没有详细说明部分可替换元素的基线，如 \<textarea> ，这意味着<font color=FF0000>这些元素使用此值的表现 **因浏览器而异**</font>。
    - **sub：**使<font color=FF0000>元素的 **基线** 与父元素的 <font color=FF0000>**下标基线**</font> 对齐</font>
    - <font color=FF0000>**super：**</font>使<font color=FF0000>元素的 **基线** 与父元素的 <font color=FF0000>**上标基线**</font> 对齐</font>
    - **text-top：**使<font color=FF0000>元素的 **顶部** 与父元素的 **字体顶部** 对齐</font>
    - **text-bottom：**使元素的 <font color=FF0000>**底部**</font> 与父元素的 <font color=FF0000>**字体底部**</font> 对齐。
    - **middle：**使元素的 <font color=FF0000>中部</font>与 父元素的 <font color=FF0000>**基线** **加上** 父元素 **x-height**（译注：x高度）的 **一半**</font>对齐。
    - **\<length>：**使元素的 <font color=FF0000>**基线**</font> 对齐到 父元素的 <font color=FF0000>**基线之上** </font>的 <font color=FF0000>**给定长度**</font>。可以是负数。
    - **\<percentage>：**使元素的 <font color=FF0000>**基线**</font> 对齐到父元素的<font color=FF0000>**基线之上**</font>的 <font color=FF0000>**给定百分比**</font>，该百分比是line-height属性的百分比。可以是负数。
  - **<font color=FF0000 size=4>相对行</font> 的值：**下列值使元素相对整行垂直对齐：
    - **top：**使 <font color=FF0000>**元素及其后代元素** 的 **顶部**</font> 与 <font color=FF0000>**整行的顶部**</font> 对齐。
    - **bottom：**使 <font color=FF0000>**元素及其后代元素的底部**</font> 与 <font color=FF0000>**整行的底部**</font> 对齐。


  没有基线的元素，使用外边距的下边缘替代。

- **表格单元格的值**

  - **baseline（以及 sub, super, text-top, text-bottom, \<length>, \<percentage>）：**使单元格的基线，与该行中所有以基线对齐的其它单元格的基线对齐。
  - **top：**使单元格内边距的上边缘与该行顶部对齐。
  - **middle：**使单元格内边距盒模型在该行内居中对齐。
  - **bottom：**使单元格内边距的下边缘与该行底部对齐。

  可以是负数。

摘自：[MDN - vertical-align](https://developer.mozilla.org/zh-CN/docs/Web/CSS/vertical-align)



#### text-overflow

<font color=FF0000>这个属性只对那些在**块级元素**溢出的内容有效</font>，但是必须要与块级元素内联(inline)方向一致 <font color=LightSeaGreen>（举个反例：内容在盒子的下方溢出。此时就不会生效）</font>。文本可能在以下情况下溢出：当其因为某种原因而无法换行(例子：设置了**`white-space:nowrap`**)，或者一个单词因为太长而不能合理地被安置(fit)。

##### 一般可选值（被一般浏览器实现的）

- **clip：**此为默认值。这个关键字的意思是"在内容区域的极限处截断文本"，因此在字符的中间可能会发生截断。<font color=dodgerBlue>如果你的目标浏览器支持</font>  `text-overflow: ''`， <font color=LightSeaGreen>为了能在两个字符过渡处截断，你可以使用一个空字符串值</font> (`''`)  <font color=LightSeaGreen>作为 text-overflow 属性的值。</font>
- **ellipsis：**这个关键字的意思是 “用一个省略号 ( `'…'` , U+2026 HORIZONTAL ELLIPSIS ) 来表示被截断的文本”。这个省略号被添加在内容区域中，因此会减少显示的文本。<font color=FF0000>如果空间太小到连省略号都容纳不下，那么这个省略号也会被截断</font>。
- **\<string>：**\<string>用来表示被截断的文本。字符串内容将被添加在内容区域中，所以会减少显示出的文本。如果空间太小到连省略号都容纳不下，那么这个字符串也会被截断。

摘自：[MDN - text-overflow](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-overflow)



#### -webkit-overflow-scrolling

> ⚠️ **非标准:** 该特性是非标准的，请尽量不要在生产环境中使用它！

##### 概述

**`-webkit-overflow-scrolling`** 属性<font color=red>控制元素在移动设备上是否使用滚动回弹效果</font>

##### 值

- **auto**：使用普通滚动，当手指从触摸屏上移开，滚动会立即停止。

- **touch**：<font color=red>使用具有回弹效果的滚动，当手指从触摸屏上移开，内容会继续保持一段时间的滚动效果</font>。继续滚动的速度和持续的时间和滚动手势的强烈程度成正比。<font color=fuchsia>同时也会创建一个新的堆栈上下文</font>。

##### 规范

尚未有相关规范。另在 Apple 提供的 [Safari CSS 参考文档](https://developer.apple.com/library/safari/documentation/AppleApplications/Reference/SafariCSSRef/Articles/StandardCSSProperties.html#//apple_ref/css/property/-webkit-overflow-scrolling) 中有所提及。

摘自：[MDN - -webkit-overflow-scrolling](https://developer.mozilla.org/zh-CN/docs/Web/CSS/-webkit-overflow-scrolling)



#### scroll-snap-type

> 🧪：这是一个实验中的功能

scroll-snap-type CSS 属性定义在滚动容器中的一个临时点（snap point）如何被严格的执行。

此属性不能用来指定任何精确的动画或者物理运动效果来执行临时点，而是交给用户代理来处理。

```css
/* 关键值 */
scroll-snap-type: none;
scroll-snap-type: x;
scroll-snap-type: y;
scroll-snap-type: block;
scroll-snap-type: inline;
scroll-snap-type: both;

/* 可选 mandatory | proximity*/
scroll-snap-type: x mandatory;
scroll-snap-type: y proximity;
scroll-snap-type: both mandatory;
/* etc */

/* 全局值 */
scroll-snap-type: inherit;
scroll-snap-type: initial;
scroll-snap-type: unset;
```

**语法**

- **取值**

  - **none：**当这个滚动容器的可视的 viewport 是滚动的，它必须忽略临时点。

  - **x：**滚动容器只捕捉其水平轴上的捕捉位置。

  - **y：**滚动容器只捕捉其垂直轴上的捕捉位置。

  - **block：**滚动容器仅捕捉到其块轴上的捕捉位置。

  - **inline：**滚动容器仅捕捉到其内联轴上的捕捉位置。

  - **both：**滚动容器会独立捕捉到其两个轴上的位置（可能会捕捉到每个轴上的不同元素）

  - **mandatory：**如果它当前没有被滚动，这个滚动容器的可视视图将静止在临时点上。意思是当滚动动作结束，如果可能，它会临时在那个点上。如果内容被添加、移动、删除或者重置大小，滚动偏移将被调整为保持静止在临时点上。

  - **proximity：**如果它当前没有被滚动，这个滚动容器的可视视图将基于基于用户代理滚动的参数去到临时点上。如果内容被添加、移动、删除或者重置大小，滚动偏移将被调整为保持静止在临时点上。

- **正式语法**

  ```css
  none | [ x | y | block | inline | both ] [ mandatory | proximity ]?
  ```

摘自：[MDN - scroll-snap-type](https://developer.mozilla.org/zh-CN/docs/Web/CSS/scroll-snap-type)



#### 滚动条相关

<font size=4>**CSS滚动条选择器**</font> 🧪

**描述：**这个 ::-webkit-scrollbar CSS伪类选择器影响了一个元素的滚动条的样式。::-webkit-scrollbar 仅仅在支持WebKit的浏览器（例如，谷歌Chrome, 苹果Safari）可以使用。

**你可以使用以下伪元素选择器去修改各式webkit浏览器的滚动条样式：**

- **::-webkit-scrollbar**：整个滚动条.
- **::-webkit-scrollbar-button**：<font color=FF0000>滚动条上的按钮（上下箭头）</font>
- **::-webkit-scrollbar-thumb**：<font color=FF0000>滚动条上的滚动滑块</font>
- **::-webkit-scrollbar-track**：<font color=FF0000>滚动条轨道</font>
- **::-webkit-scrollbar-track-piece**：<font color=FF0000>滚动条没有滑块的轨道部分</font>
- **::-webkit-scrollbar-corner**：当同时有垂直滚动条和水平滚动条时交汇的部分.
- **::-webkit-resizer**：某些元素的corner部分的部分样式(例:textarea的可拖动按钮).

摘自：[MDN - ::-webkit-scrollbar](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::-webkit-scrollbar)

<font size=4>**scrollbar-color 🧪**</font> 

scrollbar-color：该CSS属性设置 滚动条轨道 和 拇指 的颜色。

- track（轨道）是指滚动条，其一般是固定的而不管滚动位置的背景。

- thumb（拇指）是指滚动条通常漂浮在轨道的顶部上的移动部分（即：滚动条）。

**语法**

```css
/* Keyword values */
scrollbar-color: auto | dark | light;

/* <color> values */
scrollbar-color: rebeccapurple green;   /* Two valid colors. The first applies to the thumb of the scrollbar, the second to the track. */

/* Global values */
scrollbar-color: inherit | initial | unset;
```

值：\<scrollbar-color>：定义滚动条的颜色。

| 值                | 描述                                                         |
| ----------------- | ------------------------------------------------------------ |
| auto              | 在没有任何其他相关滚动条颜色属性的情况下，滚动条的轨道部分默认平台渲染。（注：即默认值） |
| dark              | 显示黑色滚动条，可以是平台提供的滚动条的深色变体，也可以是带深色的自定义滚动条。 |
| light             | 显示一个轻量滚动条，可以是平台提供的滚动条的轻微变体，也可以是带有浅色的自定义滚动条。 |
| \<color> \<color> | 将第一种颜色应用于滚动条拇指，第二种颜色应用于滚动条轨道。   |

摘自：[MDN - scrollbar-color](https://developer.mozilla.org/zh-CN/docs/Web/CSS/scrollbar-color)

<font size=4>**scrollbar-width 🧪**</font>

**语法**

```css
/* Keyword values */
scrollbar-width: auto | thin | none;

/* Global values */
scrollbar-width: inherit | initial | unset;
```

**值：** **\<scrollbar-width>** 将滚动条的宽度定义为数值宽度或者预定义宽度，当被定义为预定义宽度时，则必须为下列值之一：

| 值   | 描述                                                   |
| ---- | ------------------------------------------------------ |
| auto | 系统默认的滚动条宽度                                   |
| thin | 系统提供的瘦滚动条宽度，或者比默认滚动条宽度更窄的宽度 |
| none | 不显示滚动条，但是该元素依然可以滚动                   |

摘自：[MDN - scrollbar-width](https://developer.mozilla.org/zh-CN/docs/Web/CSS/scrollbar-width)

另外，参考视频：[使用纯 CSS 自定义滚动条样式](https://www.bilibili.com/video/BV1bS4y1V7k7)



#### ARIA

Accessible Rich Internet Applications **(ARIA)** 是<font color=FF0000>能够让残障人士更加便利的访问 Web 内容和使用 Web 应用（特别是那些由JavaScript 开发的）的一套机制</font>。

ARIA是对超文本标记语言（HTML ）的补充，以便在没有其他机制的情况下，使得应用程序中常用的交互和小部件可以传递给辅助交互技术。例如，ARIA支持HTML4中的可访问导航地标、JavaScript小部件、表单提示和错误消息、实时内容更新等。

ARIA 是一组特殊的易用性属性，可以添加到任意标签上，尤其适用于 HTML。<font color=FF0000>role 属性定义了对象的通用类型（例如文章、警告，或幻灯片）</font>。额外的 ARIA 属性提供了其他有用的特性，例如表单的描述或进度条的当前值。

摘自：[MDN - ARIA](https://developer.mozilla.org/zh-CN/docs/Web/Accessibility/ARIA)



#### 浏览器引擎前缀

<font color=FF0000> 浏览器厂商们有时会给实验性的或者非标准的 CSS 属性和 JavaScript API 添加前缀</font>，这样开发者就可以用这些新的特性进行试验，同时（理论上）防止他们的试验代码被依赖，从而在标准化过程中破坏 web 开发者的代码。<mark>开发者应该等到浏览器行为标准化之后再使用未加前缀的属性</mark>。

浏览器厂商们正在努力停止使用前缀来表示实验性质的代码的行为。Web开发者一直在生产环境的网站上使用这些实验性代码，这使得浏览器厂商更难保证浏览器兼容性和处理新特性；这也伤害了更小众的浏览器，它们被迫添加其他浏览器前缀以加载热门网站。

现在的趋势是将实验性功能添加在需要用户自行设置偏好或标记（flag）的地方，同时编写一个更小规模的规范，以更快达到稳定状态。

**CSS 前缀**
主流浏览器引擎前缀:

- **-webkit- ：**（谷歌，Safari，新版Opera浏览器，以及几乎所有iOS系统中的浏览器（包括 iOS 系统中的火狐浏览器）；基本上所有基于WebKit 内核的浏览器）

  - **补充：**WebKit是一种用来让网页浏览器绘制网页的排版引擎。它被用于Apple Safari。其分支Blink被用于基于Chromium的网页浏览器，如Opera与Google Chrome。

    摘自：[wiki - WebKit](https://zh.wikipedia.org/wiki/WebKit)

- **-moz- ：**（火狐浏览器）

- **-o- ：**（旧版Opera浏览器）

- **-ms- ：**（IE浏览器 和 Edge浏览器）

**API 前缀**

- 接口前缀：略
- 属性和方法前缀：略

摘自：[MDN - 浏览器引擎前缀](https://developer.mozilla.org/zh-CN/docs/Glossary/Vendor_Prefix)



#### \<svg>

可以参考：[[CSS] 打勾动画](https://www.bilibili.com/video/BV1ME411F7BG)，只是简单入门



## CSS 组件样式实现

##### 实现 自定义的复选框

```html
<input class="switch" type="checkbox" id="connect" />
<label for="connect"></label>

<style>
  .switch{   /*隐藏复选框，通过label来响应点击*/
   position: absolute; /*不能使用 display：none，那样会把它从键盘tab键切换焦点的队列中删除*/
   clip: rect(0,0,0,0)    
}
.switch + label {   /*设置未选中状态的label样式*/
   display: inline-block;
   height: 15px;
   width: 30px;
   border-radius: 10px;
   background-color: #eee;
   position: relative;
}
.switch + label:before {  /*可滑动的小按钮为label:before伪类*/
   content: '';
   display: inline-block;
   position: absolute;
   height: 15px;
   width: 15px;
   border-radius: 50%;
   background-color: #fff;
   right: 0;
   top: 0;
   box-shadow: 0 0 2px #666;
}
.switch:checked + label {  /*设置选中状态的label样式*/
   background-color: yellowgreen;
}
.switch:checked + label:before {
   left: 0;
}
</style>
```

###### 效果如下

<img src="https://i.loli.net/2021/09/22/WQNPyUenGYwxcvu.png" style="zoom:50%;" />


摘自：[CSS的N个编码技巧](https://juejin.cn/post/6924206099193135111) 另外，上面还有更多样式的技巧，推荐复习。



##### 给 SVG 图片添加颜色

主要就是使用 `fill` 属性。

参考：https://codepen.io/chriscoyier/pen/lcDBd



##### 纯 CSS 实现优惠券透明圆形镂空打孔效果

学习自：[纯 CSS 实现优惠券透明圆形镂空打孔效果](https://www.lervor.com/archives/72/) 。另外，感觉原文中代码内容不够清晰，给人一种 magic number 的感觉；这里做了变量抽取的改动，代码可读性和实现原理都清晰了很多

```html
<body>
  <div class="coupon">
    <div class="coupon-top"></div>
    <div class="coupon-middle">
      <div></div>
    </div>
    <div class="coupon-bottom"></div>
  </div>
</body>
```

```css
html {
  --gap-radius: 30;
  --cross-fill-radius: 300;
  --gap-add-cross-fill: calc(var(--gap-radius) + var(--cross-fill-radius));
}

body {
  background: #939393;
}

.coupon {
  width: 590px;
  height: 370px;
  border-radius: 16px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.coupon-top,
.coupon-bottom {
  background: #FFFFFF;
}

.coupon-top {
  flex: 1;
}

.coupon-bottom {
  height: 120px;
  padding: 0 38px;
}

.coupon-middle {
  height: 64px;
  position: relative;
  overflow: hidden;
}

.coupon-middle div {
  /* 中间虚线 */
  position: absolute;
  left: 36px;
  right: 36px;
  top: 29px;
  border-top: 1px dashed #E6E6E6;
  z-index: 9;
}


.coupon-middle::before,
.coupon-middle::after {
  content: '';
  border: calc(var(--cross-fill-radius) * 1px) solid #FFFFFF;
  position: absolute;
  width: calc(var(--gap-radius) * 2px);
  height: calc(var(--gap-radius) * 2px);
  border-radius: 50%;
  top: 50%;
  margin-top: calc(var(--gap-add-cross-fill) * -1px);
}

.coupon-middle::before {
  left: calc(var(--gap-add-cross-fill) * -1px);
}

.coupon-middle::after {
  right: calc(var(--gap-add-cross-fill) * -1px);
}
```



##### 一次性密码输入框

参见文章 [Single-digit inputs with one element (OTP)](https://css-tip.com/single-digit-inputs/)



## 经验总结

###### 管理好CSS有助于提高项目性能

说到CSS我们是势必要说到两个概念：**重绘 & 重排**

- **重绘：**重绘是指<mark>当 DOM 元素的属性发生变化 (如 color) 时</mark>, 浏览器会通知render 重新描绘相应的元素, 此过程称为重绘。
- **重排：**重排是指<mark style="background: aqua">某些元素变化涉及元素布局 (如width)</mark>，浏览器则抛弃原有属性, 重新计算，此过程称为重排。（重排一定会重绘，重绘不一定重排）。

<font color=FF0000>页面渲染的一般过程为 <font size=4>**JS > CSS > 计算样式 > 布局 > 绘制 > 渲染层合并**</font> 。而在这个过程中，**重排和重绘是整个环节中最为耗时的两环，从重绘和重排的概念上看，重排比重绘更加的消耗性能**</font>，所以我们尽量避免着这两个环节。从性能方面考虑,最理想的渲染流水线是没有布局和绘制环节的,只需要做渲染层的合并即可。

###### 如何更好的写 CSS & HTML

- 首先定义项目的基准样式：如重置样式、公用样式变量、兼容性处理等，且最好用less / sass / stylus 等来写我们的 css
- 把项目的公共布局及样式抽离出来：如公用的头部，公用的尾部，公用的tab等
- 避免样式重复赋值，避免样式重叠：如避免在业务或者组件里面写全局样式，样式层级不要过深
- 用好z-index，position

摘自：[我们应该如何写好HTML&CSS](https://juejin.cn/post/6854573211548549127)

**注：**关于 *回流* 和 *重绘* 在 [[前端面试点总结#回流与重绘的原理]] 以及后面的部分 有详细的说明。



#### CSS 命名规范

**BEM ( Block, Element, Modifier )**

BEM 规范试图将整个用户界面分解成一个个小的可重复使用的 <font color=FF0000>**组件**</font>

- **B 意为「区块」 ( Block )**。在实际中，这里「区块」<font color=FF0000>可以表示一个网站导航、页眉、页脚或者其他一些设计区块</font>。命名示例：

  ```css
  .stick-man { ... }
  ```

- **E 代表着「元素」 ( Element )**。整体的区块设计往往并不是孤立的，元素是组件的一部分。它们可视作子组件，也就是父组件的组成部分。如果使用 BEM 命名规范的话，这些 <font color=FF0000>元素的类名都可以通过在 **两条下划线 ( `__` )** 后加上元素名称来产生</font>

  ```css
  .stick-man__head { ... }
  .stick-man__arms { ... }
  .stick-man__feet { ... }
  ```

- **M 代表修饰符 ( Modifiers )。**在现实场景里，这可能是一个 red 或者 blue 的按钮。这就是 组件当中的限定修饰。

  如果使用 BEM 的话，这些<font color=FF0000>修饰符的类名都可以通过在 **两条连字符 ( `--` )** 后加上元素名来产生</font>。示例：

  ```css
  .stick-man--blue { ... }
  .stick-man--red { ... }
  ```

摘自：[[译] 这些 CSS 命名规范将省下你大把调试时间](https://juejin.cn/post/6844903556420632583)

**什么时候应该用 BEM 格式**

- 使用 BEM 的诀窍是，<font color=FF0000>你要知道什么时候哪些东西是应该写成 BEM 格式的</font>
- <font color=FF0000>并不是每个地方都应该使用 BEM 命名方式。当需要明确关联性的模块关系时，应当使用 BEM 格式</font>
- 比如只是一条公共的单独的样式，就没有使用 BEM 格式的意义

**在 CSS 预处理器中使用 BEM 格式**

- BEM 的一个槽点是，命名方式长而难看，书写不雅。相比 BEM 格式带来的便利来说，我们应客观看待。
- 而且，一般来说会 使用 LESS/SASS 等预处理器语言来编写 CSS，利用其语言特性书写起来要简单很多。

**以 LESS 为例：**

```less
.article {
    max-width: 1200px;
    &__body {
        padding: 20px;
    }
    &__button {
        padding: 5px 8px;
        &--primary {background: blue;}
        &--success {background: green;}
    }
}
```

摘自：[CSS — BEM 命名规范](https://juejin.cn/post/6844903672162304013)

另外，知乎帖子：[如何看待 CSS 中 BEM 的命名方式？ - 知乎](https://www.zhihu.com/question/21935157) 可以参考阅读，由于比较忙所以暂时没读

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

| 注释类型                                                   | 句法或可能取值                                                                                                                                                                                         |
|:------------------------------------------------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| 标准 HTML 注释                                             | \<!-- Comment content  --\>                                                                                                                                                                     |
| <font color=FF0000>**上层显示**</font>（downlevel-hidden）   | <font color=red>\<!--</font><font color=blue>[if expression]</font><font color=FF0000>></font> *HTML* <font color=FF0000><!</font><font color=blue>[endif]</font><font color=FF0000>--\></font> |
| <font color=FF0000>**下层隐藏**</font>（downlevel-revealed） | <font color=FF0000>\<!</font>[if *expression*]<font color=FF0000>></font> *HTML* <font color=FF0000><!</font>[endif]<font color=FF0000>\></font>                                                |

于每个条件注释之中的句法块内的 HTML 表示任意的 HTML 内容块，包括脚本。<font color=FF0000>两种条件注释均使用条件**表达式**以指示注释块内的内容应该被解析还是被忽略</font>。条件表达式由特性，操作符，和/或决定于其特性的值组成。

下表展示了支持的特性并描述了每种特性支持的值。

| 项目             | 示例                    | 说明                                                                                                                                 |
|:--------------:|:---------------------:|:----------------------------------------------------------------------------------------------------------------------------------:|
| IE             | [if IE]               | 字符串 "IE" 是一种对应于用以浏览网页的 Internet Explorer 的版本的一种*特性*。                                                                               |
| value          | [if IE 7]             | 一个对应于浏览器版本的<font color=FF0000>整数</font>或<font color=FF0000>浮点数</font>。<font color=FF0000>返回一个布尔值<，版本号和浏览器版本相匹配时为 true</font>       |
| WindowsEdition | [if WindowsEdition]   | 适用于 Windows 7 上的 Internet Explorer 8。字符串 "WindowsEdition" 是一种对应于用以浏览该网页的 Microsoft Windows 版本的*特性*。                                |
| value          | [if WindowsEdition 1] | 一个对应于用以浏览该网页的 Windows 的*版本*的整数。返回一个布尔值，数值和使用的版本相匹配时为真 true。关于所支持的值和它们所描述的版本的更多信息，参见GetProductInfo 函数的 *pdwReturnedProductType* 参数。 |
| true           | [if true]             | 永远等价于 true.                                                                                                                        |
| false          | [if false]            | 永远等价于 false.                                                                                                                       |

可用于创造条件注释的算符如下表：

| 项目  | 示例                       | 说明                                                   |
|:---:|:------------------------:|:----------------------------------------------------:|
| !   | [if !IE]                 | NOT 运算符。这被放在 *特性*, *算符*, 或者 *子表达式* 的前面以反转该表达式的布尔值含义。 |
| lt  | [if lt IE 5.5]           | 小于运算符。第一项小于第二项时返回 true。                              |
| lte | [if lte IE 6]            | 小于或等于运算符。第一项小于或等于第二项时返回 true。                        |
| gt  | [if gt IE 5]             | 大于运算符。第一项大于第二项时返回 true。                              |
| gte | [if gte IE 7]            | 大于或等于运算符。第一项大于或等于第二项时返回 true。                        |
| ( ) | [if !(IE 7)]             | 子表达式运算符。用以连接布尔算符以创造更加复杂的表达式。                         |
| &   | [if (gt IE 5)&(lt IE 7)] | AND 运算符。所有子表达式为真时返回 true。                            |
| \|  | [if (IE 6)\|(IE 7)]      | OR 运算符。子表达式任意一个为真时返回 true。                           |

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
  
  接下来下载[less.js](https://github.com/less/less.js/archive/master.zip)并将script标记在 head 元素中:
  
  ```html
  <head>
  	<link rel="stylesheet" type="text/less" href="styles.less"/>
  	<script src="less.js" type="text/javascript"></script>
  </head>
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

| Scss                                                                      | Sass                                                                      |
|:-------------------------------------------------------------------------:|:-------------------------------------------------------------------------:|
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

使用 `&` ，将会引用父选择器。示例：

##### 示例一

scss 代码：

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

##### 示例二

scss 代码：

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

##### 原本

```scss
body{
  body-family: Helvetica, Arial, sans-serif;
  body-size: 15px;
  body-weight: normal;
}
```

##### 经过属性嵌套后

```scss
body{
  font: {
      family: Helvetica, Arial, sans-serif;
      size: 15px;
      weight: normal;
  }
}
```

##### 甚至

原本

```scss
.nav{
  border: 1px solid #000;
  border-left: 0;
  border-right: 0;
}
```

经过属性嵌套后

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
type-of(5px)                             //"number"

type-of(hello)                      //"string"
type-of("hello")                     //"string"

type-of(1px solid #000)   //"list"

type-of(#ff0000)                    //"color"
type-of(red)                             //"color"
type-of(rgb(255, 0, 0))   //"color"
type-of(hsl(0, 100%, 50%))//"color"
```

#### number

`8 / 2`结果是： `8/2`，原因是有类似用法：`font: 16px/1.8 serif`，如果想要得到的结果为数字，则要将表达式放入括号：`(8 / 2)`

```scss
5px + 5px     //10px
5px - 2                //3px
5px * 2             //10px
5px * 2px         //10px*px
(10px / 2px)  //5
10px / 2px        //10px/2px
10px / 2            //10px/2
(10px / 2)         //5px
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
  hello + world                //helloworld
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
hello * world                     将会报错：Error: Undefined operation "hello * world".
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
  - **[rgb(\$red,\$green,​\$blue)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#rgb-instance_method)**：根据**红**、**绿**、**蓝**三个值创建一个颜色；
  - **[rgba(\$red,​\$green,​\$blue,\$alpha)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#rgba-instance_method)**：根据**红**、**绿**、**蓝**和**透明度**值创建一个颜色；
  - **[red($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#red-instance_method)**：从一个颜色中获取其中**红色**值；
  - **[green($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#green-instance_method)**：从一个颜色中获取其中**绿色**值；
  - **[blue($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#blue-instance_method)**：从一个颜色中获取其中**蓝色**值；
  - **[mix(\$color-1,​\$color-2,[$weight])](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#mix-instance_method)**：把两种颜色混合在一起。
  
- **HSL颜色函数**
  - **[hsl(\$hue,\$saturation,\$lightness)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#hsl-instance_method)**：通过色相（hue）、饱和度(saturation)和亮度（lightness）的值创建一个颜色；
  - **[hsla(\$hue,​\$saturation,$\lightness,\$alpha)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#hsla-instance_method)**：通过色相（hue）、饱和度(saturation)、亮度（lightness）和透明（alpha）的值创建一个颜色；
  - **[hue($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#hue-instance_method)**：从一个颜色中获取色相（hue）值；
  - **[saturation($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#saturation-instance_method)**：从一个颜色中获取饱和度（saturation）值；
  - **[lightness($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#lightness-instance_method)**：从一个颜色中获取亮度（lightness）值；
  - **[adjust-hue(\$color,​\$degrees)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#adjust_hue-instance_method)**：通过改变一个颜色的色相值，创建一个新的颜色；
  - **[lighten(\$color,​\$amount)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#lighten-instance_method)**：通过改变颜色的亮度值，让颜色变亮，创建一个新的颜色；
  - **[darken(\$color,$amount)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#darken-instance_method)**：通过改变颜色的亮度值，让颜色变暗，创建一个新的颜色；
  - **[saturate(\$color,$amount)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#saturate-instance_method)**：通过改变颜色的饱和度值，让颜色更饱和，从而创建一个新的颜色
  - **[desaturate(\$color,$amount)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#desaturate-instance_method)**：通过改变颜色的饱和度值，让颜色更少的饱和，从而创建出一个新的颜色；
  - **[grayscale($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#grayscale-instance_method)**：将一个颜色变成灰色，相当于`desaturate($color,100%)`;
  - **[complement($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#complement-instance_method)**：返回一个补充色，相当于`adjust-hue($color,180deg)`;
  - **[invert($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#invert-instance_method)**：反回一个反相色，红、绿、蓝色值倒过来，而透明度不变。
  
- **Opacity函数**
  - **[alpha($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#alpha-instance_method) /[opacity($color)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#opacity-instance_method)**：获取颜色透明度值；
  - **[rgba(\$color, $alpha)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#rgba-instance_method)**：改变颜色的透明度值；
  - **[opacify(\$color, $amount)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#opacify-instance_method) / [fade-in(\$color, $amount)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#fade_in-instance_method)**：使颜色<font color=FF0000>更不透明</font>；
  - **[transparentize(\$color, $amount)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#transparentize-instance_method) / [fade-out(\$color, $amount)](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#fade_out-instance_method)**：使颜色<font color=FF0000>更加透明</font>。

摘自：[Sass中文网 - Sass基础——颜色函数](https://www.sass.hk/skill/sass25.html)

#### 列表函数

- **length($list)：**返回`$list`长度（如果不是list,返回1）
- **nth(\$list,\$index)：**返回`$list`中第`$index`列表项值（如果索引值不在列表范围内，将会报错）
  - **nth(list, element)：**获取list中element的索引
- **index(\$list,​\$value)：**返回`$value`在`$list`中的位置
- **append(\$list,\$value[,\$separator])：**使用`$separator`分隔符将`$value`列表项添加到`$list`最后（如果没有显式指定`$separator`分隔符，会以当前分隔符分隔）
- **join(\$list-1, \$list-2[,\$separator]):**使用`$separator`分隔符将`$list-2`附加到`$list-1`（如果没有显式指定分隔符，将对`$list-1`中的分隔符）
- **zip(\*$lists):**将多个`$list`组合在一起成为一个多维列表。如果列表源长度并不是所有都相同，结果列表长度将以最短的一个为准
- **reject(\$list,\$value)：**这是Compass中的一个函数，将`$value`值从`$list`中删除
- **compact(\*$args)：**Compass函数，返回一个删除非真值的新列表

摘自：[Sass中文网 - 理解Sass的list](https://www.sass.hk/skill/sass31.html)

#### Map

定义：

```scss
$map: (key1: value1, key2: value2[, key3: value3])
```

取值：

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

#### Boolean

包含 and or not运算符

#### interpolation

可以让我们将一个值<font color=FF0000>（变量）</font>插入到另一个值<font color=FF0000>（常量）</font>中，使用方式：

```scss
#{$variable}
```

**使用场景，示例：**

```scss
$version: "0.0.1";
/*当前版本号是：#{$version}*/
```

```scss
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



## Emmet 使用

#### 语法

##### Climb-up : ^

表示 `^` 后面的元素是上一层级的。同时， `^` 可以连用（比如 `^^` ），表示上几个层级（下面有示例）

`div+div>p>span+em^bq` 按下 Tab 后生成：

```html
<div></div>
<div>
    <p><span></span><em></em></p>
    <blockquote></blockquote>
</div>
```

`div+div>p>span+em^bq` 和 `div+div>(p>span+em)+bq` 是等价的；毕竟 `div+div>p>span+em^bq` 等价于 `div+div>p>(span+em)^bq` ，括号再往上一层。

`div+div>p>span+em^^bq`  再往上一层，按下 Tab 生成：

```html
<div></div>
<div>
    <p><span></span><em></em></p>
</div>
<blockquote></blockquote>
```

##### Item numbering: $

`h$[title=item$]{Header $}*3` 按下 Tab：

```html
<h1 title="item1">Header 1</h1>
<h2 title="item2">Header 2</h2>
<h3 title="item3">Header 3</h3>
```

`ul>li.item$$$*5` 按下 Tab：

```html
<ul>
    <li class="item001"></li>
    <li class="item002"></li>
    <li class="item003"></li>
    <li class="item004"></li>
    <li class="item005"></li>
</ul>
```

其中 `$$$` 表示三位数，

`ul>li.item$@-*5` 按下 Tab：

```html
<ul>
    <li class="item5"></li>
    <li class="item4"></li>
    <li class="item3"></li>
    <li class="item2"></li>
    <li class="item1"></li>
</ul>
```

其中 `-` 表示倒序

`ul>li.item$@3*5` 按下 Tab：

```html
<ul>
    <li class="item3"></li>
    <li class="item4"></li>
    <li class="item5"></li>
    <li class="item6"></li>
    <li class="item7"></li>
</ul>
```

其中 `@` 表示 数字开始处

##### ID and CLASS attributes

`p.class1.class2.class3` 按下 Tab：

```html
<p class="class1 class2 class3"></p>
```

##### Text: {}

`p>{Click }+a{here}+{ to continue}` 按下 Tab：

```html
<p>Click <a href="">here</a> to continue</p>
```

##### Implicit tag names

有一些 标签 内的子标签是固定的，不是什么标签都可以作为子标签的。另外，emmet 默认标签是 \<div>，而在前面所说的标签内，不指定标签，生成的子标签将不会是 div，而是固定的那些子标签

`em>.class` 按下 Tab：

```html
<em><span class="class"></span></em>
```

`ul>.class` 按下 Tab：

```html
<ul>
  <li class="class"></li>
</ul>
```

`table>.row>.col` 按下 Tab：

```html
<table>
    <tr class="row">
        <td class="col"></td>
    </tr>
</table>
```

另外，可以使用 `implicitTag+` 可以生成完整的代码，比如上面 的 `table>.row>.col`  可用 `table+` 实现同样效果，详见下面 [[#HTML#table+]]

#### HTML

##### a:link

```html
<a href="http://"></a>
```

##### a:mail

```html
<a href="mailto:"></a>
```

##### link:css

```html
<link rel="stylesheet" href="style.css" />
```

##### form

```html
<form action=""></form>
```

##### form:get

```html
<form action="" method="get"></form>
```

类似的有 `form:post` 

##### input / inp

```html
<input type="text" />
```

##### input:text, input:t

```html
<input type="text" name="" id="" />
```

类似的 `input:email`、`input:time`、`input:tel` ...

##### html:xml

```html
<html xmlns="http://www.w3.org/1999/xhtml"></html>
```

##### button:submit, button:s, btn:s

```html
<button type="submit"></button>
```

类似的还有 `button:reset, button:r, btn:r` 

##### button:disabled, button:d, btn:d

```html
<button disabled="disabled"></button>
```

##### dlg

```html
<dialog></dialog>
```

##### table+

Alias of `table>tr>td`

```html
<table>
    <tr>
        <td></td>
    </tr>
</table>
```

类似的还有：`ol+` 、`ul+`、`dl+`、`map+`、 `colgroup+, colg+` 、`tr+`、`select+`、`optgroup+, optg+`、`pic+`

#### CSS

##### Color

`c` : `color:#000;`

`c:r` :  `color: rgb(0, 0, 0);`

`c:ra` : `color: rgba(0, 0, 0, .5);`

`op` : `opacity: ;`

##### 太多了，遇到了再去查，再来记录

#### XSL

暂时用不到，略。