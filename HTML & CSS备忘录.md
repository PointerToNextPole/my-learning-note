# HTML & CSS备忘录



## HTML

**\<hr\>**是horizontal rule的缩写



#### \<label>

HTML \<label> 元素（标签）表示用户界面中某个元素的说明。

**将一个 \<label> 和一个 \<input> 元素相关联主要有这些优点：**

- 标签文本不仅与其相应的文本输入元素在视觉上相关联，程序中也是如此。 这意味着，当用户聚焦到这个表单输入元素时，屏幕阅读器可以读出标签，让使用辅助技术的用户更容易理解应输入什么数据。
- 你可以点击关联的标签来聚焦或者激活这个输入元素，就像直接点击输入元素一样。这扩大了元素的可点击区域，让包括使用触屏设备在内的用户更容易激活这个元素。

<font color=FF0000>将一个 \<label> 和一个 \<input> 元素匹配在一起，你需要给 \<input> 一个 id 属性。而 \<label> 需要一个 for 属性，其值和  \<input> 的 id 一样。</font>

<mark>另外，你也可以将 \<input> 直接放在 \<label> 里，此时则不需要 for 和 id 属性，因为关联已隐含存在：</mark>

```html
<label>Do you like peas?
  <input type="checkbox" name="peas">
</label>
```

**其他使用事项：**

- 关联标签的表单控件称为这个标签的已关联标签的控件。<font color=FF0000>一个 input 可以与多个标签相关联</font>。

- 点击或者轻触（tap）与表单控件相关联的 \<label> 也可以触发关联控件的 click 事件。

**属性**
该元素包含 全局属性。

- <font color=FF0000>**for：**即和 \<label> 元素在同一文档中的 可关联标签的元素 的 id</font>。 <mark>文档中第一个 id 值与 \<label> 元素 for 属性值相同的元素，如果可关联标签（labelable），则为已关联标签的控件，其标签就是这个 \<label> 元素</mark>。如果这个元素不可关联标签，则 for 属性没有效果。如果文档中还有其他元素的 id 值也和 for 属性相同，for 属性对这些元素也没有影响。
  注意：\<label> 元素可同时有一个 for 属性和一个子代控件元素，只是 for 属性需要指向这个控件元素。
- **form：**<font color=FF0000>表示与 label 元素关联的 \<form> 元素（即它的表单拥有者）</font>。**如果声明了该属性，其值应是同一文档中 \<form> 元素的 id。因此你可以将 label 元素放在文档的任何位置，而不仅作为 \<form> 元素的后代**。

摘自：[MDN - \<label>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/label)



#### \<button>

HTML \<button> 元素表示一个可点击的按钮，可以用在表单或文档其它需要使用简单标准按钮的地方。 <mark>默认情况下，HTML按钮的显示样式接近于 user agent 所在的宿主系统平台（用户操作系统）的按钮</mark>， 但你可以使用 CSS 来改变按钮的样貌。

**属性**

- **autofocus：**<mark style=background-color:hotpink>HTML5</mark>，一个布尔属性，用于<font color=FF0000>指定当页面加载时按钮必须有输入焦点</font>，除非用户重写，例如通过不同控件键入。只有一个表单关联元素可以指定该属性。

- **autocomplete：**仅Firefox允许，略

- **disabled：**此布尔属性<font color=FF0000>表示用户不能与button交互</font>。如果属性值未指定，button继承包含元素，例如\<fieldset>；如果没有设置disabled属性的包含元素，button将可交互。

  不像其它浏览器，Firefox默认在页面加载时持续禁用Button的动态状态。使用autocomplete属性可控制此特性。

- **form：**<mark style=background-color:hotpink>HTML5</mark>，<font color=FF0000>表示button元素关联的form元素（它的表单拥有者）</font>。<font color=FF0000 size=4>**此属性值必须为同一文档中的一个\<form>元素的id属性**</font>。如果此属性未指定，\<button>元素必须是form元素的后代。利用此属性，你可以将\<button>元素放置在文档内的任何位置，而不仅仅是作为他们form元素的后代。

- **formaction：**<mark style=background-color:hotpink>HTML5</mark>，<font color=FF0000>表示程序处理button提交信息的URI</font>。<mark>如果指定了，将重写button表单拥有者（即form）的action属性</mark>。

- **formenctype：**<mark style=background-color:hotpink>HTML5</mark>，<font color=FF0000>如果button是**submit**类型，**此属性值指定提交表单到服务器的内容类型**</font>。可选值：

  - **application/x-www-form-urlencoded：**未指定时的<font color=FF0000>**默认值**</font>。
  - **multipart/form-data：**<font color=FF0000>如果使用type属性的\<input>元素设置文件</font>，使用此值。
  - **text/plain：**如果指定此属性，它将重写button的表单拥有者的enctype属性。

- **formmethod：**<mark style=background-color:hotpink>HTML5</mark>，<font color=FF0000>如果button是**submit**类型，**此属性指定浏览器提交表单使用的HTTP方法**</font>。可选值:

  - **post：**来自表单的数据被包含在表单内容中，被发送到服务器。
  - **get：**来自表单的数据以'?'作为分隔符被附加到form的URI属性中，得到的URI被发送到服务器。当表单没有副作用，且仅包含ASCII字符时使用这种方法。

  <font color=FF0000>如果指定了，此属性会重写button拥有者（即form）的method属性</font>。

- **formnovalidate：**<mark style=background-color:hotpink>HTML5</mark>，<font color=FF0000>如果button是**submit**类型，**此布尔属性指定当表单被提交时不需要验证**</font>。如果指定了，它会重写button拥有者的novalidate属性。

- **formtarget：**<mark style=background-color:hotpink>HTML5</mark>，<font color=FF0000>如果button是**submit**类型，此属性指定一个名称或关键字，表示接收提交的表单后在哪里显示响应</font>。这是一个浏览上下文（例如tab，window或内联框架）的名称或关键字。如果指定了，它会重写button拥有者的target 属性。关键字如下：

  - **\_self：**在同一个浏览上下文中加载响应作为当前的。未指定时此值为默认值。
  - **\_blank：**在一个新的不知名浏览上下文中加载响应。
  - **\_parent：**在当前浏览上下文父级中加载响应。如果没有父级的，此选项将按\_self执行。
  - **\_top：**在顶级浏览上下文（即当前浏览上下文的祖先，且没有父级）中架加载响应。如果没有顶级的，此选项将按\_self执行。

- **name：**<font color=FF0000>button的名称</font>，<font color=FF0000 size=4>**与表单数据一起提交**</font>。

- **type：**<font color=FF0000>button的类型</font>。可选值：

  - **submit:**  <font color=FF0000>此按钮将表单数据提交给服务器</font>。如果未指定属性，或者属性动态更改为空值或无效值，则<font color=FF0000>此值为默认值</font>。
  - **reset:**  <font color=FF0000>此按钮重置所有组件为初始值</font>。
  - **button:** <font color=FF0000>此按钮没有默认行为</font>。它可以有与元素事件相关的客户端脚本，当事件出现时可触发。
  - **menu:** 此按钮打开一个由指定\<menu>元素进行定义的弹出菜单。

- **value：**<font color=FF0000>button的初始值</font>。它定义的值与表单数据的提交按钮相关联。<font color=FF0000>当表单中的数据被提交时，这个值便以参数的形式被递送至服务器</font>。

摘自：[MDN - \<button>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/button)



#### \<a>

<font color=FF0000>**a是anchor的缩写。**</font>

HTML \<a> 元素（或称锚元素）可以通过它的 href 属性创建通向其他网页、文件、同一页面内的位置、电子邮件地址或任何其他 URL 的超链接。\<a> 中的内容应该应该指明链接的意图。<mark>如果存在 href 属性，当 \<a> 元素聚焦时按下回车键就会激活它</mark>。

**属性**
**该元素的属性包含全局属性**

- **download：**<mark>HTML5</mark>，<font color=FF0000>此属性指示**浏览器下载 URL** 而不是导航到它</font>，因此将提示用户将其保存为本地文件。<font color=FF0000>如果属性有一个值，那么此值将在下载保存过程中作为预填充的文件名</font>（<mark>如果用户需要，仍然可以更改文件名</mark>）。此属性对允许的值没有限制，但是 / 和 \ 会被转换为下划线。大多数文件系统限制了文件名中的标点符号，故此，浏览器将相应地调整建议的文件名。

  **注意:**

  - 此属性<font color=FF0000>**仅适用于同源 URL**</font>。
  - <mark>尽管 HTTP URL 需要位于同一源中</mark>，<font color=FF0000>但是**可以使用 blob: URL 和 data: URL** ，以方便用户下载使用 JavaScript 生成的内容</font>（例如使用在线绘图 Web 应用程序创建的照片）。
  - 如果 HTTP 头中的 Content-Disposition 属性赋予了一个不同于此属性的文件名，HTTP 头属性优先于此属性。
  - 如果 HTTP 头属性 Content-Disposition 被设置为inline（即 Content-Disposition='inline'），那么 Firefox 优先考虑 HTTP 头 Content-Dispositiondownload 属性。

- **href：**包含超链接指向的 URL 或 URL 片段。
  <font color=FF0000>URL 片段是哈希标记（#）前面的名称</font>，哈希标记（即：锚点链接）指定当前文档中的内部目标位置（HTML 元素的 ID）。<font color=FF0000>URL 不限于基于 Web（HTTP）的文档，也可以使用浏览器支持的任何协议</font>。<mark>例如，在大多数浏览器中正常工作的file:、ftp:和mailto：</mark>。

  **注意：**<mark>可以使用 href="#top" 或者 href="#" 链接返回到页面顶部</mark>。<font color=FF0000>这种行为是 HTML5 的特性</font>。

- **hreflang：**该属性<font color=FF0000>用于指定链接文档的人类语言</font>。其<mark>仅提供建议，并没有内置的功能</mark>。hreflang 允许的值取决于HTML5 BCP47。

- **ping：**包含一个以空格分隔的url列表，<mark>当跟随超链接时，将由浏览器(在后台)发送带有正文 PING 的 POST 请求</mark>。<mark>通常用于跟踪</mark>。

- **referrerpolicy：**<font color=FF0000>**实验性质**</font>，<font color=FF0000>表明在获取URL时发送哪个提交者</font>（referrer）:

  - **no-referrer：**<font color=FF0000>表示 Referer: 头将不会被发送</font>。
  - **no-referrer-when-downgrade：**<font color=FF0000>表示当从使用HTTPS的页面导航到不使用 TLS(HTTPS)的来源 时不会发送 Referer: 头</font>。<font color=FF0000>**如果没有指定策略，这将是用户代理的<font size=4>默认行为</font>**</font>。
  - **origin：**<font color=FF0000>表示 referrer将会是页面的来源</font>，大致为这样的组合：主机和端口（不包含具体的路径和参数的信息）。
  - **origin-when-cross-origin：**表示导航到其它源将会限制为这种组合：主机 + 端口，而导航到相同的源将会只包含 referrer的路径。
  - **strict-origin-when-cross-origin**
  - **unsafe-url：**表示 referrer将会包含源和路径（domain + path）（但是不包含密码或用户名的片段）。这种情况是不安全的，因为它可能会将安全的URLs数据泄露给不安全的源。

- **rel：**该属性<mark>指定了目标对象到链接对象的关系</mark>。该值是空格分隔的列表类型值。

- **target：**该属性<font color=FF0000>**指定在何处显示链接的资源**</font>。 <font color=FF0000>取值为标签（tab），窗口（window），或框架（iframe）等浏览上下文的名称或其他关键词</font>。以下关键字具有特殊的意义:

  - **\_self：**<font color=FF0000>当前页面加载</font>，即当前的响应到同一HTML 4 frame（或HTML5浏览上下文）。<font color=FF0000>**此值是默认的**，如果没有指定属性的话</font>。
  - **\_blank：**<font color=FF0000>新窗口打开</font>，即到一个<mark>新的未命名</mark>的HTML4窗口或HTML5浏览器上下文
  - **\_parent：**<mark>加载响应到当前框架的HTML4父框架或当前的HTML5浏览上下文的父浏览上下文</mark>。<font color=FF0000>如果没有parent框架或者浏览上下文，此选项的行为方式与 \_self 相同</font>。
  - **\_top：**HTML4中：加载的响应成完整的，原来的窗口，取消所有其它frame。 HTML5中：加载响应进入顶层浏览上下文（即，浏览上下文，它是当前的一个的祖先，并且没有parent）。如果没有parent框架或者浏览上下文，此选项的行为方式相同\_self

  **注意：**在 \<a> 元素上使用 target="_blank" 隐式提供了与使用 rel="noopener" 相同的 rel 行为，即不会设置 window.opener。

- **type：**该属性指定在一个 MIME type 链接目标的形式的媒体类型。其仅提供建议，并没有内置的功能。

**已废除的属性**

- **charset**（HTML5已废除）
- **coords**（HTML4有效，HTML5已废除）
- **name**（HTML4有效，HTML5已废除）
- **rev**（HTML4有效，HTML5已废除）
- **sharp**（HTML4有效，HTML5已废除）

摘自：[MDN - \<a>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a)



#### \<pre>

HTML \<pre> 元素表示预定义格式文本。<font color=FF0000>在该元素中的文本通常按照原文件中的编排，以等宽字体的形式展现出来，文本中的空白符（比如空格和换行符）都会显示出来</font>。(<mark>紧跟在 \<pre> 开始标签后的换行符也会被省略</mark>)

摘自：[MDN - \<pre>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/pre)



#### form

HTML \<form> 元素表示文档中的一个区域，此区域包含交互控件，用于向 Web 服务器提交信息。

<font color=FF0000>可以用 **:valid** 和 **:invalid** CSS 伪类来设置 \<form> 元素的样式，此时样式的表现取决于表单中的 elements 是否有效</font>。

**属性**
此元素拥有 全局属性。

- **accept：**<font color=FF0000>**（已废弃）**</font>一个逗号分隔的列表，包括服务器能接受的内容类型。
  <mark>此属性已在 HTML5 中被移除并且不再被使用</mark>。<font color=FF0000>作为替代，可以使用 \<input type=file> 元素中的 accept 属性</font>。
- **accept-charset：**<mark>一个空格分隔或逗号分隔的列表</mark>，此<font color=FF0000>列表包括了服务器支持的字符编码</font>。<font color=FF0000>浏览器**以这些编码被列举的顺序使用它们**</font>。<font color=FF0000>默认值是一个保留字符串 "UNKNOWN"</font>。此字符串指的是，和包含此表单元素的文档相同的编码。
  <mark>在之前版本的 HTML 中，不同的字符编码可以用空格或逗号分隔</mark>。<font color=FF0000>在 HTML5 中，**只有空格可以允许作为分隔符**</font>。
- **autocapitalize：**iOS Safari特有，略
- **autocomplete：**用于指示 input 元素是否能够拥有一个默认值，<mark>此默认值是由浏览器自动补全的</mark>。此设定可以被属于此表单的子元素的 autocomplete 属性覆盖。 可能的值有：
  - **off：**<font color=FF0000>浏览器可能不会自动补全条目</font>（在疑似登录表单中，浏览器倾向于忽略该值，而提供密码自动填充功能，参见 自动填充属性和登录）
  - **on：**<font color=FF0000>浏览器可自动补全条目</font>
- **name：**<font color=FF0000>表单的名称</font>。<mark>HTML 4中不推荐（应使用 id）</mark>。<font color=FF0000>**在 HTML 5 中，该值必须是所有表单中独一无二的，而且不能是空字符串**</font>。
- **rel：**根据 value 创建一个超链接或Creates a hyperlink or annotation depending on the value

**关于提交表单的属性**

- **action：**<font color=FF0000>处理表单提交的 URL</font>。<mark style=background-color:hotpink>这个值可被 \<button>、\<input type="submit"> 或 \<input type="image"> 元素上的 **formaction 属性覆盖**</mark>。

- **enctype：**<font color=FF0000>当 method 属性值为 **post** 时，**enctype 就是将表单的内容提交给服务器的 MIME 类型**</font> 。可能的取值有：

  - **application/x-www-form-urlencoded：**<font color=FF0000 size=4>未指定属性时的**默认值**</font>。
  - **multipart/form-data：**<mark>当表单包含 type=file 的 \<input> 元素时使用此值</mark>。
  - **text/plain：**<mark>出现于 HTML5，用于调试</mark>。

  <mark style=background-color:hotpink>这个值可被 \<button>、\<input type="submit"> 或 \<input type="image"> 元素上的 **formenctype** 属性覆盖</mark>。

- **method：**浏览器使用哪种 HTTP 方式来提交 表单. 可能的值有：

  - **post：**指的是 HTTP POST 方法；<font color=FF0000>表单数据会包含在表单体内然后发送给服务器</font>.
  - **get：**指的是 HTTP GET 方法；<mark>表单数据会附加在 action 属性的 URL 中，并以 '?' 作为分隔符</mark>，没有副作用 时使用这个方法。
  - **dialog：**<mark>如果表单在 \<dialog> 元素中，提交时关闭对话框</mark>。

  <mark style=background-color:hotpink>此值可以被 \<button>、\<input type="submit"> 或 \<input type="image"> 元素中的 **formmethod** 属性覆盖</mark>。

- **novalidate：**<font color=FF0000>此布尔值属性表示提交表单时不需要验证表单</font>。 <font color=FF0000>如果没有声明该属性 （因此表单需要通过验证）</font>。

  <mark style=background-color:hotpink>该属性可以被表单中的 \<button>、\<input type="submit"> 或 \<input type="image"> 元素中的 **formnovalidate** 属性覆盖</mark>。

- **target：**<mark>表示在提交表单之后，在哪里显示响应信息</mark>。在 HTML 4 中, 这是一个 frame 的名字/关键字对。<font color=FF0000>在 HTML5 里，这是一个浏览上下文 的名字/关键字（如标签页、窗口或 iframe）</font>。下述关键字有特别含义：

  - **_self：**<font color=FF0000>**默认值**</font>。<font color=FF0000>在相同浏览上下文中加载</font>。
  - **_blank：**<font color=FF0000>在新的未命名的浏览上下文中加载</font>。
  - **_parent：**在当前上下文的父级浏览上下文中加载，<font color=FF0000>如果没有父级，则与 _self 表现一致</font>。
  - **_top：**在最顶级的浏览上下文中（即当前上下文的一个没有父级的祖先浏览上下文），<font color=FF0000>如果没有父级，则与 _self 表现一致</font>。

  <mark style=background-color:hotpink>此值可以被 \<button>、 \<input type="submit"> 或 \<input type="image"> 元素中的 formtarget 属性覆盖</mark>。

摘自：[MDN - form](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/form)

#### 表单相关伪类

- **:valid**

  :valid CSS 伪类 <font color=FF0000>表示内容验证正确的 \<input> 或其他 \<form> 元素</font>。这<font color=FF0000>能简单地将校验字段展示为一种能让用户辨别出其输入数据的正确性的样式</font>。

  ```css
  /* Selects any valid <input> */
   input:valid {
     background-color: powderblue;
  }
  ```

  该伪类对于高亮正确字段是很有用的。

  摘自：[MDN - :valid](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:valid)

- **:invalid**

  :invalid CSS 伪类 <font color=FF0000>表示任意内容未通过验证的 \<input> 或其他 \<form> 元素 </font>

  ```css
  /* 可选定任意无效的<input> */
  input:invalid {
    background-color: pink;
  }
  ```

  这个伪类对于突出显示用户的字段错误非常有用。

  摘自：[MDN - :invalid](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:invalid)



#### \<dialog>

HTML \<dialog> 元素<font color=FF0000>表示一个对话框或其他交互式组件</font>，例如一个检查器或者窗口。

**属性**
这个元素包含了全局属性。但是 tabindex 特性不能被使用在 \<dialog> 元素上。

- **open：**<font color=FF0000>指示这个对话框是激活的和能互动的</font>。<mark>当这个 open 特性没有被设置，对话框不应该显示给用户</mark>。

**使用备注**

- <font color=FF0000>\<form>元素可在此对话框中使用，但需要指定属性 method="dialog"</font>。当提交表单时，对话框的 returnValue 属性将会等于表单中被使用的提交按钮的 value 。
- ::backdrop CSS 伪元素可用于更改 \<dialog> 背景元素样式，例如在对话框被打开激活时，调暗背景中不可访问的内容。仅当使用  HTMLDialogElement.showModal() (en-US)  显示对话框时才会绘制 backdrop 背景。

摘自：[MDN - \<dialog>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/dialog)



#### \<fieldset>

HTML \<fieldset> 元素用于对表单中的控制元素<font color=FF0000>进行分组</font>（也包括 label 元素）。

\<fieldset> 元素将一个HTML表单的一部分组成一组，<font color=FF0000>内置了一个 \<legend> 元素作为 fieldset 的标题</font>。这个元素有几个属性，最值得注意的是 form，其可以包含同一页面的 \<form> 元素的 id，以使 <font color=FF0000>\<fieldset> 成为这个 \<form> 的一部分</font>，即使 \<fieldset> 不在其内。还有 disabled 属性，可将 \<fieldset> 及其所有内容设置为不可用。

**属性：**<font color=FF0000>这个元素包含所有全局属性</font>。

- **disabled：**如果设置了这个 bool 值属性, \<fieldset> 的所有子代表单控件也会继承这个属性。<font color=FF0000>这意味着它们不可编辑，也不会随着 \<form> 一起提交</font>。它们也不会接收到任何浏览器事件，如鼠标点击或与聚焦相关的事件。默认情况下，浏览器会将这样的控件展示为灰色。注意，\<legend> 中的表单元素不会被禁用。
- **form：**将该值<font color=FF0000>设为一个 \<form> 元素的 id 属性值以将 \<fieldset> 设置成这个 \<form> 的一部分</font>。      
- **name：**元素分组（fieldset）的名称

注意：fieldset 的标题由第一个 \<legend> 子元素确定。

摘自：[MDN - \<fieldset>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/fieldset)

#### \<legend>

HTML \<legend> 元素用于表示其父元素 \<fieldset> 的内容标题。

摘自：[MDN - \<legend>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/legend)



#### \<meta>

HTML \<meta> 元素表示那些不能由其它 HTML 元相关（meta-related）元素（(\<base>、\<link>, \<script>、\<style> 或 \<title>）之一表示的任何元数据信息。

##### **meta 元素定义的元数据的类型包括以下几种：**

- 如果<font color=FF0000>设置了 name 属性</font>，meta 元素<font color=FF0000>提供的是文档级别（document-level）的元数据</font>，应用于整个页面。

- 如果<font color=FF0000>设置了 http-equiv 属性</font>，meta 元素则<font color=FF0000>是编译指令</font>，提供的信息与类似命名的HTTP头部相同。

- 如果<font color=FF0000>设置了 charset 属性</font>，meta 元素<font color=FF0000>是一个字符集声明</font>，<font color=FF0000>告诉文档使用哪种字符编码</font>。

- 如果<font color=FF0000>设置了 itemprop 属性</font>，meta 元素<font color=FF0000>提供用户定义的元数据</font>。

##### **属性**

**注意：**全局属性 name 在 \<meta> 元素中具有特殊的语义；另外， 在同一个 \<meta> 标签中，name, http-equiv 或者 charset 三者中任何一个属性存在时，itemprop 属性不能被使用。

- **charset：**这个属性<font color=FF0000>声明了文档的字符编码</font>。<mark>如果使用了这个属性</mark>，<font color=FF0000>其值必须是</font>与ASCII大小写无关（ASCII case-insensitive）的<font color=FF0000>"utf-8"</font>。

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
  
- **name：**name 和 content 属性可以一起使用，以名-值对的方式给文档提供元数据，其中 name 作为元数据的名称，content 作为元数据的值。

摘自：[MDN - \<meta>：文档级元数据元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta)



#### **HTML5 新属性**

| 属性         | 值                                                      | 描述                               |
|:---------- |:------------------------------------------------------ |:-------------------------------- |
| charset    | *character_set*                                        | 定义文档的字符编码。                       |
| content    | *text*                                                 | 定义与 http-equiv 或 name 属性相关的元信息。  |
| http-equiv | content-type default-style refresh                     | 把 content 属性关联到 HTTP 头部。         |
| name       | application-name author description generator keywords | 把 content 属性关联到一个名称。             |
| scheme     | *format/URI*                                           | HTML5不支持。 定义用于翻译 content 属性值的格式。 |

#### 表单属性

- **maxlength / minlength：**用于\<input>和\<textarea>，作用是限制输入最多的和最少的字符数量
- **autocomplete：**可用于以文本或数字值作为输入的 \<input> 元素 ， \<textarea> 元素, \<select> 元素, 和\<form> 元素。 autocomplete 允许web开发人员指定，如果有任何权限 user agent 必须提供填写表单字段值的自动帮助，并为浏览器提供关于字段中所期望的信息类型的指导。<mark>建议值的来源通常取决于浏览器。 通常，值来自用户输入的过去值，但它们也可能来自预先配置的值。 </mark>

#### data-*（数据属性）

HTML5是具有扩展性的设计，它初衷是数据应与特定的元素相关联，但不需要任何定义。<font color=FF0000>data-* 属性允许我们在标准内于HTML元素中存储额外的信息</font>，而不需要使用类似于 classList，标准外属性，DOM额外属性或是 setUserData 之类的技巧。

- **HTML语法：**

  语法非常简单。所有在元素上以data-开头的属性为数据属性。比如说你有一篇文章，而你又想要存储一些不需要显示在浏览器上的额外信息。请使用data属性：

  ```html
  <article
    id="electriccars"
    data-columns="3"
    data-index-number="12314"
    data-parent="cars">
  	...
  </article>
  ```

- **JavaScript 访问**

  在外部使用JavaScript去访问这些属性的值同样非常简单。你<font color=FF0000>可以使用 getAttribute() 配合它们完整的HTML名称去读取它们</font>，<font color=FF0000>但**标准定义了一个更简单的方法：DOMStringMap你可以使用dataset读取到数据**</font>。

  为了使用dataset对象去获取到数据属性，需要获取属性名中 **`data-`** 之后的部分(要注意的是：<font color=FF0000>破折号连接的名称需要改写为骆驼拼写法</font>(如"index-number"转换为"indexNumber"))。

  ```js
  var article = document.querySelector('#electriccars');
  
  article.dataset.columns // "3"
  article.dataset.indexNumber // "12314"
  article.dataset.parent // "cars"
  ```

- **CSS 访问**
  注意：data设定为HTML属性，他们同样能被CSS访问。比如你可以通过generated content使用函数attr()来显示data-parent的内容：

  ```css
  article::before {
    content: attr(data-parent);
  }
  ```

  你也同样可以在CSS中使用属性选择器根据data来改变样式：

  ```css
  article[data-columns='3'] {
    width: 400px;
  }
  article[data-columns='4'] {
    width: 600px;
  }
  ```

摘自：[MDN - 使用数据属性](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Howto/Use_data_attributes)

**补充：**

- data-* 属性用于存储页面或应用程序的<font color=FF0000>私有自定义数据</font>。

- data-* 属性赋予我们在所有 HTML 元素上嵌入自定义 data 属性的能力。

- **data-* 属性包括两部分：**
  - <font color=FF0000>属性名不应该包含任何大写字母</font>，并且在前缀 "data-" 之后必须有至少一个字符
  - 属性值可以是任意字符串

摘自：[w3school - HTML data-* 属性](https://www.w3school.com.cn/tags/att_global_data.asp)



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



#### \<img>

**属性**

- src：是必须的，它包含了你想嵌入的图片的文件路径。
- alt： 属性包含一条对图像的文本描述，这<mark>不是强制性的</mark>，<mark style="background: fuchsia">但对可访问性而言，它难以置信地有用——屏幕阅读器会将这些描述读给需要使用阅读器的使用者听，让他们知道图像的含义</mark>。如果由于某种原因无法加载图像，普通浏览器也会在页面上显示alt 属性中的备用文本：例如，网络错误、内容被屏蔽或链接过期时。

- **还有很多其他属性，可以实现各种不同的目的：**

  - Referrer/CORS 控制，保证安全与隐私：详见 crossorigin 属性和 referrerpolicy 属性。

    - **crossorigin：**这个<mark>枚举属性表明是否必须使用 CORS 完成相关图像的抓取</mark>。启用 CORS 的图像 可以在 \<canvas> 元素中重复使用，而不会被污染（tainted）。允许的值有：

      - **anonymous：**执行一个跨域请求（比如，有 Origin HTTP header）。但是<font color=FF0000>**没有发送证书**</font>（比如，没有 cookie，没有 X.509 证书，没有 HTTP 基本授权认证）。如果服务器没有把证书给到源站（没有设置 Access-Control-Allow-Origin HTTP 头），图像会被污染，而且它的使用会被限制。
      - **use-credentials：**<font color=FF0000>一个有证书的跨域请求</font>（比如，有 Origin HTTP header）被发送 （比如，cookie, 一份证书，或者 HTTP 基本验证信息）。如果服务器没有给源站发送证书（通过 Access-Control-Allow-Credentials HTTP header），图像将会被污染，且它的使用会受限制。

      当用户并未显式使用本属性时，默认不使用 CORS 发起请求（例如，不会向服务器发送原有的HTTP 头部信息），可防止其在 \<canvas> 中的使用。如果无效，默认当做 anonymous 关键字生效。

    - **referrerpolicy** ：用于表明在获取资源时，使用什么样的referrer请求头

      - no-referrer: The Referer header will not be sent.
      - no-referrer-when-downgrade: No Referer header is sent when navigating to an origin without HTTPS. This is the default if no policy is otherwise specified.
      - origin: The Referer header will include the page's origin (scheme, host, and port (en-US)).
      - origin-when-cross-origin: Navigating to other origins will limit the included referral data to the scheme, host, and port, while navigating from the same origin will include the full path and query string.
      - unsafe-url: The Referer header will always include the origin, path and query string, but not the fragment, password, or username. This is unsafe because it can leak information from TLS-protected resources to insecure origins.

  - 使用 width、height 和 intrinsicsize 设置 原始分辨率 (en-US)：这将设置图像应占用的空间，以确保图像被加载之前页面的布局是稳定的。

    - <font color=FF0000>**width：**图像的宽度</font>，在 HTML5 中单位是 CSS 像素， 在 HTML 4 中可以是像素也可以是百分比。
    - <font color=FF0000>**height：**图像的高度</font>，在 HTML5 中的单位是 CSS 像素，在 HTML 4 中既可以是像素，也可以是百分比。可以只指定 width 和 height 中的一个值，浏览器会根据原始图像进行缩放。

  - 使用 sizes 和 srcset 设置响应式图像

    - sizes：表示资源大小的、以逗号隔开的一个或多个字符串。每一个资源大小包括：

      - 一个媒体条件。最后一项一定是被忽略的。
      - 一个资源尺寸的值。

    - srcset
      以逗号分隔的一个或多个字符串列表表明一系列用户代理使用的可能的图像。每一个字符串由以下组成：

      - 指向图像的 URL。
      - 可选地，再加一个空格之后，附加以下的其一：
        - 一个宽度描述符，这是一个正整数，后面紧跟 'w' 符号。该整数宽度除以sizes属性给出的资源（source）大小来计算得到有效的像素密度，即换算成和x描述符等价的值。
        - 一个像素密度描述符，这是一个正浮点数，后面紧跟 'x' 符号。

      如果没有指定源描述符，那它会被指定为默认的 1x。

      在相同的 srcset 属性中混合使用宽度描述符和像素密度描述符时，会导致该值无效。重复的描述符（比如，两个源在相同的 srcset 两个源都是 2x）也是无效的。

摘自：[MDN - \<img>：图像嵌入元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img)



#### HTML语义化

**HTML语义化是什么？**

语义化是指<font color=FF0000>根据内容的结构化（内容语义化），选择合适的标签（代码语义化）</font>，<mark>便于开发者阅读和写出更优雅的代码的同时，让浏览器的爬虫和机器很好的解析</mark>。

**为什么要语义化？**

- 有利于SEO，有助于爬虫抓取更多的有效信息，爬虫是依赖于标签来确定上下文和各个关键字的权重。
- 语义化的HTML在没有CSS的情况下也能呈现较好的内容结构与代码结构
- 方便其他设备的解析
- 便于团队开发和维护

以上摘自：[HTML语义化](https://segmentfault.com/a/1190000005626375)



#### HTML5全局属性

<mark>全局属性是**所有HTML元素共有的属性**; 它们可以用于所有元素，即使属性可能对某些元素不起作用</mark>

<font color=FF0000>我们可以在所有的HTML元素上指定全局属性，甚至是在标准里没有指定的元素</font>。<mark>这意味着任何非标准元素仍必须能够应用这些属性，即使使用这些元素意味着文档不再是html5兼容的</mark>。例如，虽然\<foo>不是一个有效的HTML元素，但是html5兼容的浏览器隐藏了标记为\<foo hidden>...\</foo>的内容。

**除了基本的HTML全局属性之外，还存在以下全局属性:**

- **xml:lang 和 xml:base** ——<mark>两者都是从XHTML规范继承，但为了兼容性而被保留的</mark>。
- **多重aria-*属性**，<mark>用于改善可访问性</mark>。
- **事件处理程序 属性：**on+事件名称。由于过多，这里略；详见原文

**全局属性列表**

- **accesskey：**<mark>提供了为当前元素生成键盘快捷键的提示</mark>。这个属性由空格分隔的字符列表组成。浏览器应该使用在计算机键盘布局上存在的第一个。

- **autocapitalize：**<font color=FF0000>**控制用户的文本输入是否和如何自动大写**</font>，它可以有以下的值：

  - **off or none**，没有应用自动大写（<font color=FF0000>所有字母都默认为小写字母</font>）。
  - **on or sentences**，<font color=FF0000>每个<font size=4>**句子**</font>的第一个字母默认为大写字母；所有其他字母都默认为小写字母</font>。
  - **words**，<font color=FF0000>每个<font size=4>**单词**</font>的第一个字母默认为大写字母；所有其他字母都默认为小写字母</font>。
  - **characters**，<font color=FF0000>所有的字母都应该默认为大写</font>。

- **class：**<font color=FF0000>一个以空格分隔的元素的类名（classes ）<font size=4>**列表**</font></font>，它允许 CSS 和 Javascript 通过类选择器 (class selectors) 或DOM方法( document.getElementsByClassName)来选择和访问特定的元素。

- **contenteditable：**一个<font color=FF0000>枚举属性</font>（enumerated attribute），<font color=FF0000>表示元素是否可被用户编辑</font>。 如果可以，浏览器会调整元素的部件（widget）以允许编辑。

  - **true 或者空字符串**，表明元素是可被编辑的；
  - **false**，表明元素不能被编辑。

- **contextmenu：**\<menu> 的id ，作为该元素的上下文菜单（<font color=FF0000>已经不被支持，将从所有浏览器中删除</font>）。

- **data-*：**<font color=FF0000>一类自定义数据属性</font>，<mark>它赋予我们在所有 HTML 元素上嵌入自定义数据属性的能力</mark>，并可以通过脚本(一般指JavaScript) 与 HTML 之间进行专有数据的交换。所有这些自定义数据属性都可以通过所属元素的 HTMLElement 接口来访问。  <font color=FF0000>**HTMLElement.dataset 属性可以访问它们**</font>。

- **dir：**<font color=FF0000>一个指示元素中**文本方向**的枚举属性</font>。它的取值如下：

  - **ltr**, 指<font color=FF0000>从左到右</font>，用于那种从左向右书写的语言（比如英语）；
  - **rtl**, 指<font color=FF0000>从右到左</font>，用于那种从右向左书写的语言（比如阿拉伯语）；
  - **auto**, 指<font color=FF0000>由用户代理决定方向</font>。它在解析元素中字符时会运用一个基本算法，直到发现一个具有强方向性的字符，然后将这一方向应用于整个元素。

- **draggable：**一种枚举属性，<font color=FF0000>指示是否可以 使用 Drag and Drop API 拖动元素</font>。它可以有以下的值：

  - **true**, 这<font color=FF0000>表明元素可能被拖动</font>
  - **false**, 这<font color=FF0000>表明元素可能不会被拖动</font>

- **dropzone：**<font color=FF0000>实验性质</font>。枚举属性，<font color=FF0000>指示可以使用 Drag and Drop API 在元素上**拖拽哪些类型的内容**</font>。 它可以具有以下值：

  - **copy**，表示drop将创建被拖动元素的副本
  - **move**，表示拖动的元素将移动到此新位置。
  - **link**，将创建一个指向拖动数据的链接。

- **exportparts ：**<font color=FF0000>实验性质</font>。Used to transitively export shadow parts from a nested shadow tree into a containing light tree.

- **hidden：**布尔属性，<font color=FF0000>表示该元素尚未或不再相关</font>。例如，它可用于隐藏在登录过程完成之前无法使用的页面元素。浏览器不会呈现此类元素。不得使用此属性隐藏可合法显示的内容

- **id：**定义唯一标识符（ID），<font color=FF0000>该标识符在整个文档中必须是唯一的</font>。 其目的是在链接（使用片段标识符），脚本或样式（使用CSS）时标识元素。

- **inputmode：**<font color=FF0000>向浏览器提供有关在编辑此元素或其内容时<font size=4>**要使用的虚拟键盘配置类型**</font>的提示</font>。主要用于 \<input>元素，但在contenteditable模式下可用于任何元素。它可以是以下值：

  - **none：**无虚拟键盘。在应用程序或者站点需要实现自己的键盘输入控件时很有用。
  - **text：**使用用户本地区域设置的标准文本输入键盘。（<font color=FF0000>**默认值**</font>）
  - **decimal：**小数输入键盘，包含数字和分隔符（通常是“ . ”或者“ , ”），设备可能也可能不显示减号键。
  - **numeric：**数字输入键盘，所需要的就是0到9的数字，设备可能也可能不显示减号键。
  - **tel：**电话输入键盘，包含0到9的数字、星号（*）和井号（#）键。表单输入里面的电话输入通常应该使用 \<input type="tel"> 
  - **search：**为搜索输入优化的虚拟键盘，比如，返回键可能被重新标记为“搜索”，也可能还有其他的优化。
  - **email：**为邮件地址输入优化的虚拟键盘，通常包含"@"符号和其他优化。表单里面的邮件地址输入应该使用 \<input type="email"> 。
  - **url：**为网址输入优化的虚拟键盘，比如，“/”键会更加明显、历史记录访问等。表单里面的网址输入通常应该使用 \<input type="url"> 。

  摘自：[MDN - inputmode](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/inputmode)

- **is：**允许您<font color=FF0000>指定标准HTML元素应该**像已注册的自定义内置元素一样**</font>。<font color=FF0000 size=4>**（便于语义化和SEO）**</font>

  只有在当前文档中已成功定义( defined )指定的自定义元素名称并且扩展了要应用的元素类型时，才能使用此属性。示例如下：

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

- **itemid：**<font color=FF0000><font size=4>**项**</font>的唯一全局标识符</font>。

- **itemprop：**<font color=FF0000>用于向项添加属性</font>。 每个HTML元素都可以指定一个itemprop属性，其中一个itemprop由一个名称和值对组成。

- **itemref：**只有不是具有itemscope属性的元素的后代，它的属性才可以与使用itemref项目相关联。它提供了元素ID列表（而不是itemids）以及文档中其他位置的其他属性。

- **itemscope：**itemscope（通常）与itemtype一起使用，以指定包含在关于特定项目代码块中的HTML。 itemscope创建Item并定义与之关联的itemtype的范围。 itemtype是描述项及其属性上下文的词汇表（例如schema.org）的有效URL。

- **itemtype：**指定将用于在数据结构中定义itemprops（项属性）的词汇表的URL。 itemscope用于设置数据结构中按itemtype设置的词汇表的生效范围。

- **lang：**<font color=FF0000>帮助定义元素的语言：不可编辑元素所在的语言，或者应该由用户编写的可编辑元素的语言</font>。该属性包含一个“语言标记”(由用连字符分隔的“语言子标记”组成)，格式在 Tags for Identifying Languages (BCP47) 中定义。xml:lang 优先于它。

- **part：**<font color=FF0000>实验性质</font>。元素的部件名称的空格分隔列表。Part名称允许CSS通过 ::part() 伪元素选择和设置阴影关联树中的特定元素。

- **slot：**<font color=FF0000>**将shadow DOM阴影关联树中的一个沟槽分配给一个元素**</font>：具有slot属性的元素被分配给由\<slot>元素创建的沟槽，其name属性的值与slot属性的值匹配。

- **spellcheck：**<font color=FF0000>实验性质</font>。枚举属性，<font color=FF0000>定义**是否可以检查元素是否存在拼写错误**</font>。它可能具有以下值：

  - **true**，<font color=FF0000>表示如果可能，应检查元素是否存在拼写错误</font>；
  - **false**，表示<font color=FF0000>不应检查元素的拼写错误</font>。

- **style：**<font color=FF0000>含要应用于元素的CSS样式声明</font>。 请注意，建议在单独的文件中定义样式。 该属性和\<style>元素主要用于快速样式化，例如用于测试目的。

- **tabindex：**整数属性，<font color=FF0000>指示元素是否可以获取输入焦点（可聚焦），是否应该参与顺序键盘导航</font>，如果是，则表示哪个位置。它可能需要几个值：

  - **负值：**<font color=FF0000>表示该元素应该是可聚焦的</font>，<font color=FF0000 size=4>但**不应通过顺序键盘导航到达**</font>;
  - **0：**<font color=FF0000>表示元素应通过顺序键盘导航可聚焦和可到达</font>，但其相对顺序由平台约定定义;
  - **正值：**意味着<font color=FF0000>元素应该可以通过顺序键盘导航进行聚焦和访问；元素聚焦的顺序是tabindex的增加值</font>。如果多个元素共享相同的tabindex，则它们的相对顺序遵循它们在文档中的相对位置。

- **title：**<font color=FF0000>包含表示与其所属元素相关信息的文本</font>。 这些信息通常可以作为提示呈现给用户，但不是必须的。

- **translate：**<font color=FF0000>实验性质</font>。枚举属性，<font color=FF0000>用于指定**在页面本地化时**是否转换元素的属性值及其Text 节点子节点的值，或者是否保持它们不变</font>。它可以具有以下值：

  - <font color=FF0000>**空字符串和"yes"**</font>，<font color=FF0000>表示元素将被翻译</font>。
  - **"no"**, 表示该元素不会被翻译。

摘自：[MDN - 全局属性](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes)



#### HTML5语义化元素（标签）

**HTML5语义化标签**

| 标签                                    | 描述                                                       |
| --------------------------------------- | ---------------------------------------------------------- |
| \<article>                              | 页面独立的内容区域。                                       |
| \<aside>                                | 页面的侧边栏内容。                                         |
| \<bdi>                                  | 允许您设置一段文本，使其脱离其父元素的文本方向设置。       |
| \<command>                              | 命令按钮，比如单选按钮、复选框或按钮                       |
| \<details>                              | 用于描述文档或文档某个部分的细节                           |
| \<dialog>                               | 对话框，比如提示框                                         |
| \<summary>                              | 标签包含 details 元素的标题                                |
| \<figure>                               | 规定独立的流内容（图像、图表、照片、代码等等）。           |
| <font color=FF0000>\<figcaption></font> | <font color=FF0000>\<figure> 元素的标题</font>             |
| \<footer>                               | section 或 document 的页脚。                               |
| <font color=FF0000>\<header></font>     | <font color=FF0000>文档的头部区域</font>                   |
| \<mark>                                 | 带有记号的文本。                                           |
| \<meter>                                | 度量衡。仅用于已知最大和最小值的度量。                     |
| <font color=FF0000>\<nav></font>        | <font color=FF0000>导航链接的部分。</font>                 |
| \<progress>                             | 任何类型的任务的进度。                                     |
| \<ruby>                                 | ruby 注释（中文注音或字符）。                              |
| \<rt>                                   | 字符（中文注音或字符）的解释或发音。                       |
| \<rp>                                   | 在 ruby 注释中使用，不支持 ruby 元素的浏览器所显示的内容。 |
| \<section>                              | 文档中的节（section、区段）。                              |
| \<time>                                 | 日期或时间。                                               |
| \<wbr>                                  | 规定在文本中的何处适合添加换行符。                         |

**语义化标签的使用**

- **\<title>\</title>** 页面主要内容
  - \<title> 标签的特点是简短、描述性、唯一，用于提升搜索引擎排名。

  - 搜索引擎会把 title 作为判断页面主要内容的指标，有效的 title 应该包含几个与页面内容密切相关的关键字，建议将 title 的核心内容写在前 60 个字符。

- **\<header>\</header>** 页眉
  - HTML5 规范描述为“一组解释性或导航型性的条目”，通常有网站标志、主导航、全站链接以及搜索框。

- **\<nav>\</nav>** 导航
- 页面的导航链接区域，用于定义页面的主要导航部分。
  - 导航通常使用 \<ul> 无序列表。若是面包屑链接，则使用 \<ol> 有序列表。
  
- HTML5 规范不推荐对辅助页脚链接使用 nav，除非页脚再次显示顶级全局导航、或者是招聘信息等重要链接。
  
- <font color=FF0000>**\<main>\</main>** 主要内容</font>
  - 网站页面的主要内容，并且<font color=FF0000>一个页面只能使用一次 \<main> 标签</font>。
  - 若是 web 应用，则包含其主要功能。

- **\<article>\</article>** 文章标记
  - 表示的是一个文档、页面、应用或是网站中的一个独立的容器。
  - HTML5 规范声明 \<article> 标签适用于自包含的窗口小部件:股票行情，计算器，钟表，天气窗口小部件等。
  - \<article>这个标签可以嵌套使用，但是他们必须是部分与整体的关系。

- **\<section>\</section>** 区块
  - 一组相似主题的内容，一般会有一个标题。
  - 实现比如文章的章节，标签式对话框中的各种标签页等功能。

- **\<aside>\</aside>** 侧边栏
  - 表示一部分内容与页面的主体并没有较大的关系，并且可以独立存在。
  - 实现比如升式引用、侧边栏、相关文章的链接、广告、友情链接等功能。

- **\<footer>\</footer>** 页脚
  - 和 \<header> 标签对应，可以实现比如附录、索引、版权页、许可协议等功能。

- **\<cite>\</cite>** 引用
  - 表示它所包含的文本对某个参考文献的引用，比如书籍或者杂志的标题。
  - 按照惯例，引用的文本将以斜体显示。
  - 用 \<cite> 标签把指向其他文档的引用分离出来，尤其是分离那些传统媒体中的文档，如书籍、杂志、期刊，等等。

- <font color=FF0000>**\<blockquote>\</blockquote>** 块引用</font>
  - \<blockquote> 与 \</blockquote> 之间的所有文本都会从常规文本中分离出来，经常会在左、右两边进行缩进（增加外边距），而且有时会使用斜体。也就是说，块引用拥有它们自己的空间。

- <font color=FF0000>**\<q>\</q>** 短的引用</font>
  - 浏览器经常在引用的内容周围添加引号。
  - 根据 HTML 4.01 规范，q 元素应当使用分界引号来呈现，就是说，q 元素包含的文本必须以引号来开始和结束。

- **\<time>\</time>** 日期或时间
  - 如果未定义 datetime 属性，则必须在元素的内容中规定日期或时间。

- **\<abbr>\</abbr>** 简称或缩写
  - 通过对缩写进行标记，您能够为浏览器、拼写检查和搜索引擎提供有用的信息。
  - 可以在 \<abbr> 标签中使用全局的 title 属性，这样就能够在鼠标指针移动到 \<abbr> 元素上时显示出简称/缩写的完整版本。

- **\<dfn>\</dfn>** 特殊术语或短语的定义
  - 现在流行的浏览器通常用斜体来显示 \<dfn> 中的文本。
  - 与其他许多基于内容的样式和物理样式标签一样，\<dfn> 标签尽量少用为妙。

- **\<del>\</del>** 删除的文本
  - 和 \<ins> 标签配合使用，来描述文档中的更新和修正。

- **\<ins>\</ins>** 插入文本
- **\<code>\</code>** 源代码
  - 用于表示计算机源代码或者其他机器可以阅读的文本内容。

- **\<pre>\</pre>** 预格式化的文本
  - 被包围在 pre 元素中的文本通常会保留空格和换行符。而文本也会呈现为等宽字体。
  - 若使用 \<pre> 标签来定义计算机源代码，比如 HTML 源代码，则使用符号实体来表示特殊字符，比如 "<" 代表 "<"，">" 代表 ">"，"&" 代表 "&"。
  - 可以导致段落断开的标签（例如标题、\<p> 和 \<address> 标签）绝不能包含在 \<pre> 所定义的块里。尽管有些浏览器会把段落结束标签解释为简单地换行，但是这种行为在所有浏览器上并不都是一样的。
  - pre 元素中允许的文本可以包括物理样式和基于内容的样式变化，还有链接、图像和水平分隔线。

**补充**

<img src="https://i.loli.net/2021/02/23/Y6qnmiPTJdOjz1Q.jpg" alt="img" style="zoom:70%;" />

摘自：[runoob - HTML5 语义元素](https://www.runoob.com/html/html5-semantic-elements.html) / [前端面试题-HTML语义化标签](https://segmentfault.com/a/1190000013901244)



#### \<article>

**HTML `<article>`**元素表示文档、页面、应用或网站中的独立结构，其意在成为可独立分配的或可复用的结构，如在发布中，它可能是论坛帖子、杂志或新闻文章、博客、用户提交的评论、交互式组件，或者其他独立的内容项目。

**使用说明**

- 每个\<article>，通常包括标题（\<h1> - \<h6>元素）作为\<article>元素的子元素。
- 当\<article>元素嵌套使用时，则该元素代表与外层元素有关的文章。例如，代表博客评论的\<article>元素可嵌套在代表博客文章的\<article>元素中。

- \<article>元素的作者信息可通过\<address>元素提供，但是不适用于嵌套的\<article>元素。
- \<article>元素的发布日期和时间可通过\<time>元素的pubdate属性表示。
- 可以使用\<time> 元素的datetime属性来描述\<article>元素的发布日期和时间。请注意\<time>的pubdate 属性不再是W3C HTML5标准。



#### \<figure>

**HTML `<figure>` 元素**代表一段独立的内容，<font color=FF0000>经常与说明（caption）\<figcaption>配合使用，并且作为一个独立的引用单元</font>。当它属于主内容流（main flow）时，它的位置独立于主体。这个标签经常是在主文中引用的图片，插图，表格，代码段等等，当这部分转移到附录中或者其他页面时不会影响到主体。

**效果：**

<img src="https://i.loli.net/2020/11/17/Y2VJ586S1XEKQkN.png" alt="DeAnsA.png" style="zoom:40%;" />

##### **使用说明**

- 通常，\<figure>是图像，插图，图表，代码片段等，在文档的主流程中引用，但可以移动到文档的另一部分或附录而不影响主流程。
- 作为sectioning root，\<figure>元素的内容轮廓将从文档的主要轮廓中排除。
- 通过在其中插入\<figcaption>（作为第一个或最后一个子元素），可以将标题与\<figure>元素相关联。图中找到的第一个\<figcaption>元素显示为图的标题。

摘自：[MDN - \<figure>：可附标题内容元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/figure)



#### \<section>

HTML \<section>元素<font color=FF0000>表示一个包含在HTML文档中的独立部分</font>，它没有更具体的语义元素来表示，一般来说会有包含一个标题。

##### **使用说明**

- 一般通过是否包含一个标题 (\<h1>-\<h6> element) 作为子节点 来 辨识每一个\<section>。
- 如果 \<section> 元素的内容可以单独在多个媒体上发表，应该使用 \<article> 而不是 \<section>。
- 不要把\<section>元素作为一个普通的容器来使用，这是本应该是\<div>的用法（特别是当片段（the sectioning ）仅仅是为了美化样式的时候）。 一般来说，一个 \<section> 应该出现在文档大纲中。

摘自：[MDN - \<section>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/section)



#### \<progress>：进度指示元素

**HTML**中的**`<progress>`**元素<font color=FF0000>用来显示一项任务的完成进度</font>。<mark>虽然规范中没有规定该元素具体如何显示，浏览器开发商可以自己决定，但通常情况下，该元素都显示为一个进度条形式</mark>。如下示例：

<img src="https://i.loli.net/2021/02/23/6a4NvX5LlUoCQId.png" alt="https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/progress" style="zoom:40%;" />

**属性：**和其他的HTML元素一样，该元素具有全局属性。

- **max：**该属性描述了这个progress元素所表示的任务一共需要完成多少工作.
- **value：**该属性用来指定该进度条已完成的工作量.如果没有value属性,则该进度条的进度为"不确定",也就是说,进度条不会显示任何进度,你无法估计当前的工作会在何时完成(比如在下载一个未知大小的文件时,下载对话框中的进度条就是这样的)。

摘自：[MDN - \<progress>：进度指示元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/progress)



#### \<address>

**HTML `<address>` 元素** 表示其中的 HTML <font color=FF0000>提供了某个人或某个组织（等等）的联系信息</font>。

**用法说明**

- <mark>当表示一个和联系信息无关的任意的地址时，请改用 \<p> 元素而不是 \<address> 元素</mark>。
- <mark>这个元素不能包含除联系信息之外的任何信息</mark>，比如出版日期（这应当被包含在 \<time> 元素之中）。
- 通常，\<address> 元素可以放在 \<footer> 元素之中（如果存在的话）。



#### \<input>

如果想要输入的值位于中间 / 右边，只需要设置text-align: center / right即可，同时，placeholder也会跟着居中 / 居右

- **input输入事件：**onfocus（focus） -> 键盘输入 -> onkeydown（keydown） -> onkeypress（keypress） -> onkeyup（keyup） -> oninput（input） -> 失去焦点 -> onchange（change） -> onblur（blur)
  
  摘自：[input输入框事件](https://www.jianshu.com/p/4517117abd8e)

- <mark>修改输入框光标的颜色：可以使用caret-color属性</mark>

- ##### **\<input type="hidden">**
  
  <font color=FF0000>`"hidden"` 类型的 `<input>` 元素允许 Web 开发者存放一些用户不可见、不可改的数据，在用户提交表单时，这些数据会一并发送出</font>。比如，正被请求或编辑的内容的 ID，或是一个唯一的安全令牌。这些隐藏的 `<input>`元素在渲染完成的页面中完全不可见，而且没有方法可以使它重新变为可见。
  
  **使用场景：**隐藏的 `<input>` 可以用于任何有需要提交给服务器的、用户无法查看或编辑的数据的地方。让我们看一些说明其用法的例子吧。
  
  - 跟踪被编辑的内容：
    
    隐藏输入的最常见用途之一是当被编辑的表单提交时，保持跟踪数据库数据的更新。
  
  - 改善网站安全性：
    隐藏输入表单还用于存储和提交安全令牌或机密信息，以提高网站的安全性。基本思路是，如果用户填写敏感表格，例如在其银行网站上将某笔款项转入另一个帐户的表格，他们将被提供的密钥和证明他们就是他们所说的真实身份，并且他们使用正确的表单来提交转移请求。
  
  摘自：[MDN - \<input type="hidden">](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Input/hidden)
  
  **补充：**
  
  隐藏域在页面中对于用户是不可见的，在表单中插入隐藏域的目的在于收集或发送信息，以利于被处理表单的程序所使用 （隐藏只是在网页页面上面不显示输入框，但是虽然隐藏了，还是具有form传值功能。一般用来传值，而不必让用户看到。）



| Type                                   | 描述                                                         | Spec  |
| :------------------------------------- | :----------------------------------------------------------- | :---- |
| button                                 | 没有默认行为的按钮，上面显示 value 属性的值，默认为空。      |       |
| checkbox                               | 复选框，可设为选中或未选中。                                 |       |
| color                                  | 用于指定颜色的控件；在支持的浏览器中，激活时会打开取色器。   | HTML5 |
| date                                   | 输入日期的控件（年、月、日，不包括时间）。在支持的浏览器激活时打开日期选择器或年月日的数字滚轮。 | HTML5 |
| datetime-local                         | 输入日期和时间的控件，不包括时区。在支持的浏览器激活时打开日期选择器或年月日的数字滚轮。 | HTML5 |
| email                                  | 编辑邮箱地址的区域。类似 text 输入，但在支持的浏览器和带有动态键盘的设备上会有确认参数和相应的键盘。 |       |
| file                                   | 让用户选择文件的控件。使用accept属性规定控件能选择的文件类型。 |       |
| hidden                                 | 不显示的控件，其值仍会提交到服务器。举个例子，右边就是一个隐形的控件。 |       |
| image                                  | 带图像的 submit 按钮。显示的图像由 src 属性规定。如果 src 缺失，alt 属性就会显示。 |       |
| month                                  | 输入年和月的控件，没有时区。                                 | HTML5 |
| number                                 | 用于输入数字的控件。如果支持的话，会显示滚动按钮并提供缺省验证（即只能输入数字）。拥有动态键盘的设备上会显示数字键盘。 |       |
| password                               | 单行的文本区域，其值会被遮盖。如果站点不安全，会警告用户。   |       |
| radio                                  | 单选按钮，允许在多个拥有相同 name 值的选项中选中其中一个。   |       |
| range                                  | 此控件用于输入不需要精确的数字。控件是一个范围组件，默认值为正中间的值。同时使用htmlattrdefmin  和 htmlattrdefmax来规定值的范围。 | HTML5 |
| reset                                  | 此按钮将表单的所有内容重置为默认值。不推荐。                 |       |
| search                                 | 用于搜索字符串的单行文字区域。输入文本中的换行会被自动去除。在支持的浏览器中可能有一个删除按钮，用于清除整个区域。拥有动态键盘的设备上的回车图标会变成搜索图标。 | HTML5 |
| submit                                 | 用于提交表单的按钮。                                         |       |
| tel                                    | 用于输入电话号码的控件。拥有动态键盘的设备上会显示电话数字键盘。 | HTML5 |
| text                                   | 默认值。单行的文本区域，输入中的换行会被自动去除。           |       |
| time                                   | 用于输入时间的控件，不包括时区。                             | HTML5 |
| url                                    | 用于输入 URL 的控件。类似 text 输入，但有验证参数，在支持动态键盘的设备上有相应的键盘。 | HTML5 |
| week                                   | 用于输入以年和周数组成的日期，不带时区。                     |       |
| <font color=FF0000>**废弃的值**</font> |                                                              |       |
| datetime                               | 用于输入基于UTC时区的日期和时间（时、分、秒及秒的小数部分）。 |       |



#### \<textarea\>完全去除边框：

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

- 声明示例如下：

  ```html
  <a name="top">这里是TOP部分</a>
  <a name="content">这里是CONTENT部分</a>
  <a name="foot">这里是FOOT部分</a>
  ```

  <font color=FF0000>可以使用 id 属性来替代 name 属性，命名锚同样有效</font>。

- **锚点的访问有两种方法**

  - 一种是利用超链接标签 `<a></a>` 制作锚点链接，主要用于页面内的锚点访问

    ```html
    <a href="#top">点击我链接到TOP</a>
    <a href="#content">点击我链接到CONTENT</a>
    <a href="#foot">点击我链接到FOOT</a>
    ```

  - 另一种方式是<font color=FF0000>直接在页面地址后面加锚点标记，主要用于不同页面之间的锚点访问</font>

    <mark>假如本页面的地址是 `http://文件路径/index.html`，要访问foot锚点只要访问如下链接即可：`http://文件路径/index.html#foot`</mark>

摘自：[html中的锚点介绍和使用](https://blog.csdn.net/yangkai_hudong/article/details/14163483)



#### tabindex

tabindex 全局属性 指示其元素是否可以聚焦，以及它是否/在何处参与顺序键盘导航（通常使用Tab键，因此得名）。<font color=FF0000>（可以在下面“ 摘自的url ”按下<font size=5>⇥</font>看下效果）</font>

**它接受一个整数作为值，具有不同的结果，具体取决于整数的值：**

- tabindex=<font color=FF0000>负值</font> (通常是tabindex=“-1”)，表示<font color=FF0000>元素是可聚焦的，但是不能通过键盘导航来访问到该元素</font>，用JS做页面小组件内部键盘导航的时候非常有用。
- tabindex="<font color=FF0000>0</font>" ，表示元素<font color=FF0000>是可聚焦的，并且可以通过键盘导航来聚焦到该元素</font>，它的相对顺序是当前处于的DOM结构来决定的。
- tabindex=<font color=FF0000>正值</font>，表示<font color=FF0000>元素是可聚焦的，并且可以通过键盘导航来访问到该元素；它的相对顺序按照tabindex 的数值递增而滞后获焦</font>。<font color=FF0000>如果多个元素拥有相同的 tabindex，它们的相对顺序按照他们在当前DOM中的先后顺序决定</font>。

根据键盘序列导航的顺序，值为 0 、非法值、或者没有 tabindex 值的元素应该放置在 tabindex 值为正值的元素后面。

摘自：[MDN - tabindex](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/tabindex)



#### \<script>

HTML \<script> 元素用于嵌入或引用可执行脚本。这通常用作嵌入或者指向 JavaScript 代码。\<script> 元素也能在其他语言中使用，比如 WebGL 的 GLSL 着色器语言。

**属性**
该元素包含全局属性。

- **async (HTML5)**

  对于普通脚本，如果存在 async 属性，那么普通脚本会被并行请求，并尽快解析和执行。
  对于模块脚本，如果存在 async 属性，那么脚本及其所有依赖都会在延缓队列中执行，因此它们会被并行请求，并尽快解析和执行。
  该属性能够消除解析阻塞的 Javascript。解析阻塞的 Javascript 会导致浏览器必须加载并且执行脚本，之后才能继续解析。defer 在这一点上也有类似的作用。
  这是个布尔属性：布尔属性的存在意味着 true 值，布尔属性的缺失意味着 false 值。
  关于浏览器支持请参见浏览器兼容性。另可参见文章asm.js的异步脚本。

- **crossorigin**

  那些没有通过标准CORS (en-US)检查的正常script 元素传递最少的信息到 window.onerror。可以使用本属性来使那些将静态资源放在另外一个域名的站点打印错误信息。参考 CORS 设置属性了解对有效参数的更具描述性的解释。

  ```html
  <script src="" crossorigin="anonymous"></script>
  ```

- **defer**
  这个布尔属性被设定用来通知浏览器该脚本将在文档完成解析后，触发 DOMContentLoaded (en-US) 事件前执行。
  有 defer 属性的脚本会阻止 DOMContentLoaded 事件，直到脚本被加载并且解析完成。

  > 如果缺少 src 属性（即内嵌脚本），该属性不应被使用，因为这种情况下它不起作用。
  >
  > defer 属性对模块脚本没有作用 —— 他们默认 defer。

- **integrity：**包含用户代理可用于验证已提取资源是否已无意外操作的内联元数据。参见 Subresource Integrity。

- **nomodule：**这个布尔属性被设置来标明这个脚本在支持 ES2015 modules 的浏览器中不执行。 — 实际上，这可用于在不支持模块化JavaScript的旧浏览器中提供回退脚本。

- **nonce**

- **referrerpolicy**

- **src：**这个属性定义引用外部脚本的URI，这可以用来代替直接在文档中嵌入脚本。指定了 src 属性的script元素标签内不应该再有嵌入的脚本。

- **type**
  该属性定义script元素包含或src引用的脚本语言。属性的值为MIME类型; 支持的MIME类型包括text/javascript, text/ecmascript, application/javascript, 和application/ecmascript。如果没有定义这个属性，脚本会被视作JavaScript。
  如果MIME类型不是JavaScript类型（上述支持的类型），则该元素所包含的内容会被当作数据块而不会被浏览器执行。
  如果type属性为module，代码会被当作JavaScript模块 。请参见ES6 in Depth: Modules
  在Firefox中可以通过定义type=application/javascript;version=1.8来使用如let声明这类的JS高版本中的先进特性。 但请注意这是个非标准功能，其他浏览器，特别是基于Chrome的浏览器可能会不支持。
  关于如何引入特殊编程语言，请参见这篇文章。

- **text**
  和 textContent 属性类似，本属性用于设置元素的文本内容。但和 textContent  不一样的是，本属性在节点插入到DOM之后，此属性被解析为可执行代码。

- **过时的属性**

  - charset
  - language

***



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

#### **补充：**

- **边距塌陷：**给两个盒子同时设置外边距，他们最终的距离可能不是两者外边距之和。
  
  这种现象会发生在相邻盒子间或父子盒子间，当他们都设置了边距时。
  
  - 如果都是正数，则取最大值。
  - 如果相同，则取其任一。
  - 如果正负都有，取最大正数与最小负数之和。
  - 如果都是负数，则取两者中最小的
  
  另外，<font color=FF0000>设置元素为BFC可以解决边距塌陷的问题。</font>

- **块格式化上下文**（Block Formatting Context，BFC） 是Web页面的可视CSS渲染的一部分，是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域。
  
  下列方式会创建块格式化上下文：
  
  - 根元素（\<html>）
  - 浮动元素（元素的 float 不是 none）
  - 绝对定位元素（元素的 position 为 absolute 或 fixed）
  - 行内块元素（元素的 display 为 inline-block）
  - 表格单元格（元素的 display 为 table-cell，HTML表格单元格默认为该值）
  - 表格标题（元素的 display 为 table-caption，HTML表格标题默认为该值）
  - 匿名表格单元格元素（元素的 display 为 table、table-row、 table-row-group、table-header-group、table-footer-group（分别是HTML table、row、tbody、thead、tfoot 的默认属性）或 inline-table）
  - overflow 计算值(Computed)不为 visible 的块元素
  - display 值为 flow-root 的元素
  - contain 值为 layout、content 或 paint 的元素
  - 弹性元素（display 为 flex 或 inline-flex 元素的直接子元素）
  - 网格元素（display 为 grid 或 inline-grid 元素的直接子元素）
  - 多列容器（元素的 column-count 或 column-width 不为 auto，包括 column-count 为 1）
  - column-span 为 all 的元素始终会创建一个新的BFC，即使该元素没有包裹在一个多列容器中（标准变更，Chrome bug）。
  
  摘自：[MDN - 块格式化上下文](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)

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

（**内联样式）Inline style > （内部样式）Internal style sheet >（外部样式）External style sheet > 浏览器默认样式**（类似于就近原则）



#### float

float CSS属性指定一个元素应沿其容器的左侧或右侧放置，允许文本和内联元素环绕它。<font color=FF0000>该元素从网页的正常流动(normal flow 文档流、正常流)中移除</font>，尽管仍然保持部分的流动性（与绝对定位相反）。

可选值

- **left：**表明元素必须浮动在其所在的块容器左侧的关键字。
- right：表明元素必须浮动在其所在的块容器右侧的关键字。
- **none：**<mark>表明元素不进行浮动的关键字</mark>。
- **inline-start：**<mark style="background: fuchsia">关键字，表明元素必须浮动在其所在块容器的开始一侧，在ltr脚本中是左侧，在rtl脚本中是右侧</mark>。
- **inline-end：**<mark style="background: fuchsia">关键字，表明元素必须浮动在其所在块容器的结束一侧，在ltr脚本中是右侧，在rtl脚本中是左侧</mark>。

**由于float意味着使用块布局，它在某些情况下修改display 值的计算值：**

| 指定值             | 计算值                                                       |
| :----------------- | :----------------------------------------------------------- |
| inline             | block                                                        |
| inline-block       | block                                                        |
| inline-table       | table                                                        |
| table-row          | block                                                        |
| table-row-group    | block                                                        |
| table-column       | block                                                        |
| table-column-group | block                                                        |
| table-cell         | block                                                        |
| table-caption      | block                                                        |
| table-header-group | block                                                        |
| table-footer-group | block                                                        |
| flex               | <font color=FF0000>flex, 但是float对这样的元素不起作用</font> |
| inline-flex        | inline-flex, 但是float对这样的元素不起作用                   |
| *other*            | *unchanged*                                                  |

摘自：[MDN - float](https://developer.mozilla.org/zh-CN/docs/Web/CSS/float)

**个人项目体会补充：**

float 和 `margin-left: auto` & `margin-right: auto`的关系：在flexbox中，float是失效的；这时如果想实现float的效果，该怎么处理？

可以使用 `margin-left: auto` 替代 `float: right` ， `margin-right: auto` 替代 `float: left`（这里使用绝对定位似乎也可以实现）。跟进一步：如果flexbox使用了`flex-wrap: wrap` 且 右侧没有空间了，那只能换行，且放到下一行的右侧。这种情况下，是用绝对定位也不行了（无法放到下一行，只能放到该行末尾，会出现覆盖的情况）；这时使用`margin-right: auto`依然是有效的。原理是：使用auto<font color=FF0000>会将所有剩余空间都占满</font>；类似的可参考《CSS权威指南》



#### CSS Cursor

| 值                                    | 描述                                                         |
| :------------------------------------ | :----------------------------------------------------------- |
| url                                   | 需使用的自定义光标的图片URL。注释：请在此列表的末端始终定义一种普通的光标，以防没有由 URL 定义的可用光标。 |
| <font color=FF0000>default</font>     | <font color=FF0000>默认</font>光标（通常是一个箭头）         |
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
      background-image: url('paper.gif');
  }
  ```

- #### background-repeat
  
  属性定义背景图像的重复方式。背景图像<mark>可以沿着水平轴，垂直轴，两个轴重复，或者根本不重复</mark>。
  
  | **单值**  | **等价于双值（XY两个方向）** |
  | :-------: | :--------------------------: |
  | repeat-x  |       repeat no-repeat       |
  | repeat-y  |       no-repeat repeat       |
  |  repeat   |        repeat repeat         |
  |   space   |         space space          |
  |   round   |         round round          |
  | no-repeat |     no-repeat no-repeat      |
  
  在双值语法中, 第一个值表示<mark>水平重复行为</mark>, 第二个值表示<mark>垂直重复行为</mark>. 下面是关于每一个值是怎么工作的具体说明：
  
  |    值     |                             说明                             |
  | :-------: | :----------------------------------------------------------: |
  |  repeat   | 图像会按需重复来覆盖整个背景图片所在的区域. 最后一个图像会被裁剪, 如果它的大小不合适的话. |
  |   space   | 图像会尽可能得重复, 但是不会裁剪. 第一个和最后一个图像会被固定在元素(element)的相应的边上, 同时空白会均匀地分布在图像之间. background-position属性会被忽视, 除非只有一个图像能被无裁剪地显示. 只在一种情况下裁剪会发生, 那就是图像太大了以至于没有足够的空间来完整显示一个图像. |
  |   round   | 随着允许的空间在尺寸上的增长, 被重复的图像将会伸展(没有空隙), 直到有足够的空间来添加一个图像. 当下一个图像被添加后, 所有的当前的图像会被压缩来腾出空间. 例如, 一个图像原始大小是260px, 重复三次之后, 可能会被伸展到300px, 直到另一个图像被加进来. 这样他们就可能被压缩到225px.译者注: 关键是浏览器怎么计算什么时候应该添加一个图像进来, 而不是继续伸展. |
  | no-repeat | 图像不会被重复(因为背景图像所在的区域将可能没有完全被覆盖). 那个没有被重复的背景图像的位置是由background-position属性来决定. |
  
  以上关于background-repeat摘自：[MDN - background-repeat](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-repeat)
  
- #### **background-attachment**
  
  决定背景图像的位置是在视口内固定，或者随着包含它的区块滚动。
  
  - fixed：此关键属性值表示背景相对于视口固定。即使一个元素拥有滚动机制，背景也不会随着元素的内容滚动。
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

**补充：**

- <mark><font color=FF0000>**line-height CSS 属性**</font>用于设置多行元素的空间量，如多行文本的间距</mark>。<font color=FF0000>**对于块级元素，它指定元素行盒（line boxes）的最小高度。对于非替代的 inline 元素，它用于计算行盒（line box）的高度**</font>。

- **letter-spacing：**用于设置文本字符的间距表现。

**用em来设置字体大小**

为了避免Internet Explorer 中无法调整文本的问题，许多开发者使用 em 单位代替像素。em的尺寸单位由W3C建议。1em和当前字体大小相等。在浏览器中默认的文字大小是16px。因此，1em的默认大小是16px。可以通过下面这个公式将像素转换为em：px/16=em

#### text-indent

text-indent 属性能定义一个块元素首行文本内容之前的缩进量。

摘自：[MDN - text-indent](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-indent)



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
  
  <mark><font color=FF0000>**元素的高度、宽度及顶部和底部边距不可设置**</font></mark>；
  
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

2、<font color=FF0000>元素的高度、宽度、行高以及顶和底边距都可设置</font>。

**另外：**IE（低版本 IE）本来是不支持 inline-block 的，所以在 IE 中对内联元素使用 display:inline-block，理论上 IE 是不识别的，但使用 display:inline-block 在 IE 下会触发 layout，从而使内联元素拥有了 display:inline-block 属性的表象。

摘自：[block，inline和inline-block概念和区别](https://www.cnblogs.com/KeithWang/p/3139517.html)  [CSS： inline、block和inline-block的区别](https://www.cnblogs.com/adongyo/p/11290826.html)



#### CSS Position

元素可以使用的顶部，底部，左侧和右侧属性定位。然而，这些属性无法工作，除非是先设定position属性。他们也有不同的工作方式，这取决于定位方法。

position 属性的五个值：

- **static**：HTML 元素的<font color=FF0000>默认值</font>，即没有定位，遵循正常的文档流对象。
  
  浏览器会按照源码的顺序，决定每个元素的位置，这称为“正常的页面流”（normal flow）。每个块级元素占据自己的区块（block），元素与元素之间不产生重叠，这个位置就是元素的默认位置。
  
  <mark>注意，static定位所导致的元素位置，是浏览器自主决定的，所以这时top、bottom、left、right这四个属性无效。</mark>

- **relative**：相对定位元素的定位是相对其正常位置。<font color=FF0000>**（相对于自身定位）**</font>
  
  relative表示，相对于默认位置（即static时的位置）进行偏移，即定位基点是元素的默认位置。
  
  它<mark>必须搭配top、bottom、left、right这四个属性一起使用，用来指定偏移的方向和距离</mark>。

- **fixed**：<mark>元素的位置相对于浏览器窗口是固定位置</mark>。
  
  它如果搭配`top`、`bottom`、`left`、`right`这四个属性一起使用，表示元素的初始位置是基于视口计算的，否则初始位置就是元素的默认位置。

- **absolute**：<font color=FF0000>**绝对定位的元素的位置相对于最近的<mark>已定位的</mark>父元素（即：父元素带有position属性（一般是relative）。如果没有，absolute将相对于body移动）**</font>，如果元素没有已定位的父元素，那么它的位置相对于\<html>
  
  absolute表示，相对于上级元素（一般是父元素）进行偏移，即定位基点是父元素。
  
  <font color=FF0000>它有一个重要的限制条件：定位基点（一般是父元素）不能是static定位，否则定位基点就会变成整个网页的根元素html</font>。另外，<mark>absolute定位也必须搭配top、bottom、left、right这四个属性一起使用</mark>。

- **sticky**：sticky 英文字面意思是粘，粘贴，所以可以把它称之为粘性定位。**position: sticky;** 基于用户的滚动位置来定位。粘性定位的元素是依赖于用户的滚动，在 **position:relative** 与 **position:fixed** 定位之间切换。
  
  **使用场景**：<font color=FF0000>网页的搜索工具栏，初始加载时在自己的默认位置（relative定位），页面向下滚动时，工具栏变成固定位置，始终停留在页面头部（fixed定位）</font>。
  
  <mark>sticky<font color=FF0000>**生效的前提**</font>是，<font color=FF0000>必须搭配top、bottom、left、right这四个属性一起使用，不能省略</font>，否则等同于relative定位，不产生"动态固定"的效果</mark>。原因是这四个属性用来定义"偏移距离"，浏览器把它当作sticky的生效门槛。
  
  它的<font color=FF0000>**具体规则**</font>是，<font color=FF0000>当页面滚动，父元素开始脱离视口时（即部分不可见），只要与sticky元素的距离达到生效门槛，relative定位自动切换为fixed定位</font>；<font color=FF0000>等到父元素完全脱离视口时（即完全不可见），fixed定位自动切换回relative定位。</font>
  
  注意，除了已被淘汰的 IE 以外，其他浏览器目前都支持`sticky`。但是，Safari 浏览器需要加上浏览器前缀`-webkit-`。
  
  部分摘自：[阮一峰的网络日志 - CSS 定位详解](https://www.ruanyifeng.com/blog/2019/11/css-position.html)



#### **重叠的元素**

元素的定位与文档流无关，所以它们可以覆盖页面上的其它元素。<mark>z-index属性指定了一个元素的堆叠顺序（哪个元素应该放在前面，或后面）</mark>。一个元素可以有正数或负数的堆叠顺序，<font color=FF0000>具有更高堆叠顺序的元素总是在较低的堆叠顺序元素的前面</font>。



#### CSS 布局 - Overflow

CSS overflow 属性<mark>可以控制内容溢出元素框时在对应的元素区间内添加滚动条</mark>。

overflow属性有以下值：

| 值       | 描述                                        |
|:-------:|:-----------------------------------------:|
| visible | 默认值。内容不会被修剪，会呈现在元素框之外。                    |
| hidden  | 内容会被修剪，并且其余内容是不可见的。                       |
| scroll  | <mark>内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容</mark>。 |
| auto    | 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。              |
| inherit | 规定应该从父元素继承 overflow 属性的值。                 |

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

| 选择器                                                                 | 示例             | 示例说明                        |
|:------------------------------------------------------------------- |:-------------- |:--------------------------- |
| [:link](https://www.runoob.com/cssref/sel-link.html)                | a:link         | 选择所有未访问链接                   |
| [:visited](https://www.runoob.com/cssref/sel-visited.html)          | a:visited      | 选择所有访问过的链接                  |
| [:active](https://www.runoob.com/cssref/sel-active.html)            | a:active       | 选择正在活动链接                    |
| [:hover](https://www.runoob.com/cssref/sel-hover.html)              | a:hover        | 把鼠标放在链接上的状态                 |
| [:focus](https://www.runoob.com/cssref/sel-focus.html)              | input:focus    | 选择元素输入后具有焦点                 |
| [:first-letter](https://www.runoob.com/cssref/sel-firstletter.html) | p:first-letter | 选择每个\<p> 元素的第一个字母           |
| [:first-line](https://www.runoob.com/cssref/sel-firstline.html)     | p:first-line   | 选择每个\<p> 元素的第一行             |
| [:first-child](https://www.runoob.com/cssref/sel-firstchild.html)   | p:first-child  | 选择器匹配属于任意元素的第一个子元素的 \<p> 元素 |
| [:before](https://www.runoob.com/cssref/sel-before.html)            | p:before       | 在每个\<p>元素之前插入内容             |
| [:after](https://www.runoob.com/cssref/sel-after.html)              | p:after        | 在每个\<p>元素之后插入内容             |
| [:lang(*language*)](https://www.runoob.com/cssref/sel-lang.html)    | p:lang(it)     | 为\<p>元素的lang属性选择一个开始值       |

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
  
  可以在同一个元素上设置多个阴影效果，并用逗号将他们分隔开。该属性<mark style="background: fuchsia">可设置的值包括阴影的X轴偏移量、Y轴偏移量、模糊半径、扩散半径和颜色</mark>。
  
  **取值**
  
  - **inset**
    
    如果没有指定inset，默认阴影在边框外，即阴影向外扩散。 使用 inset 关键字会使得阴影落在盒子内部，这样看起来就像是内容被压低了。 此时阴影会在边框之内 (即使是透明边框）、背景之上、内容之下。
  
  - **\<offset-x>** **\<offset-y>**
    
    这是头两个 \<length> 值，用来设置阴影偏移量。x,y 是按照数学二维坐标系来计算的，只不过y垂直方向向下。 \<offset-x> 设置水平偏移量，正值阴影则位于元素右边，负值阴影则位于元素左边。 \<offset-y> 设置垂直偏移量，正值阴影则位于元素下方，负值阴影则位于元素上方。可用单位请查看 \<length> 。
    
    如果两者都是0，那么阴影位于元素后面。这时如果设置了\<blur-radius> 或\<spread-radius> 则有模糊效果。需要考虑 inset 
  
  - **\<blur-radius> ** **阴影模糊半径** 
    
    这是第三个 \<length> 值。值越大，模糊面积越大，阴影就越大越淡。 不能为负值。默认为0，此时阴影边缘锐利。本规范不包括如何计算模糊半径的精确算法，但是，它详细说明如下：
  
  > 对于长而直的阴影边缘，它会创建一个过渡颜色用于模糊 以阴影边缘为中心、模糊半径为半径的局域，过渡颜色的范围在完整的阴影颜色到它最外面的终点的透明之间。 （译者注：对此有兴趣的可以了解下数字图像处理的模糊算法。）
  
  - **\<spread-radius>** **阴影扩散半径**
    
    这是第四个 \<length> 值。取正值时，阴影扩大；取负值时，阴影收缩。默认为0，此时阴影与元素同样大。需要考虑 inset 
  
  - **\<color>** **阴影颜色**
    
    相关事项查看 \<length> 。如果没有指定，则由浏览器决定——通常是color的值，不过目前Safari取透明。
  
  相关摘自：[MDN - box-shadow](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-shadow)

  ##### <mark>**补充：**text-shadow用法和box-shadow一样，是用来对于文字进行设置阴影</mark>
  
- **border-image**：边界图片



#### CSS3 背景

**背景属性：**

- **background-image**

- **background-size**

  background-size 设置背景图片大小。图片可以保有其原有的尺寸，或者拉伸到新的尺寸，或者在保持其原有比例的同时缩放到元素的可用空间的尺寸。

  单张图片的背景大小可以使用以下三种方法中的一种来规定：

  - 使用关键词 contain（以可满足的一边满足，另一边<font color=FF0000>空出</font>）
  - 使用关键词 cover（以短的一边满足，长的一边<font color=FF0000>裁剪</font>）
  - 设定宽度和高度值，可以是\<length>, 是 \<percentage>, 或者 auto.

  摘自：[MDN - background-size](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-size)

- **background-position**

  background-position 为每一个背景图片设置初始位置。 这个位置是相对于由 background-origin 定义的位置图层的。

  摘自：[MDN - background-position](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-position)

- **background-origin**

- **background-clip**

**补充：background-blend-mode**

**（MDN）**background-blend-mode CSS属性<font color=FF0000>定义该元素的背景图片，以及背景色如何混合</font>。

混合模式应该按background-image CSS属性同样的顺序定义。如果混合模式数量与背景图像的数量不相等，它会被截取至相等的数量。

**（RUNOOB）**background-blend-mode 属性<font color=FF0000>定义了背景层的混合模式（图片与颜色）</font>。

|     值      |             描述             |
| :---------: | :--------------------------: |
|   normal    | 默认值。设置正常的混合模式。 |
|  multiply   |        正片叠底模式。        |
|   screen    |          滤色模式。          |
|   overlay   |          叠加模式。          |
|   darken    |          变暗模式。          |
|   lighten   |          变亮模式。          |
| color-dodge |        颜色减淡模式。        |
| saturation  |         饱和度模式。         |
|    color    |          颜色模式。          |
| luminosity  |          亮度模式。          |

摘自：[MDN - background-blend-mode](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-blend-mode)、 [RUNOOB - CSS background-blend-mode 属性](https://www.runoob.com/cssref/pr-background-blend-mode.html)



#### clip-path

clip-path CSS 属性<font color=FF0000>使用裁剪方式创建元素的可显示区域</font>。区域内的部分显示，区域外的隐藏。

**取值**

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

| 属性                                                                                    | 描述                                      | CSS |
|:------------------------------------------------------------------------------------- |:--------------------------------------- |:---:|
| [hanging-punctuation](https://www.runoob.com/cssref/css3-pr-hanging-punctuation.html) | 规定标点字符是否位于线框之外。                         | 3   |
| [punctuation-trim](https://www.runoob.com/cssref/css3-pr-punctuation-trim.html)       | 规定是否对标点字符进行修剪。                          | 3   |
| [text-align-last](https://www.runoob.com/cssref/css3-pr-text-align-last.html)         | 设置如何对齐最后一行或紧挨着强制换行符之前的行。                | 3   |
| [text-emphasis](https://www.runoob.com/css3/css3-pr-text-emphasis.html)               | 向元素的文本应用重点标记以及重点标记的前景色。                 | 3   |
| [text-justify](https://www.runoob.com/cssref/css3-pr-text-justify.html)               | 规定当 text-align 设置为 "justify" 时所使用的对齐方法。 | 3   |
| [text-outline](https://www.runoob.com/cssref/css3-pr-text-outline.html)               | 规定文本的轮廓。                                | 3   |
| [text-overflow](https://www.runoob.com/cssref/css3-pr-text-overflow.html)             | 规定当文本溢出包含元素时发生的事情。                      | 3   |
| [text-shadow](https://www.runoob.com/cssref/css3-pr-text-shadow.html)                 | 向文本添加阴影。                                | 3   |
| [text-wrap](https://www.runoob.com/cssref/css3-pr-text-wrap.html)                     | 规定文本的换行规则。                              | 3   |
| [word-break](https://www.runoob.com/cssref/css3-pr-word-break.html)                   | 规定非中日韩文本的换行规则。                          | 3   |
| [word-wrap](https://www.runoob.com/cssref/css3-pr-word-wrap.html)                     | 允许对长的不可分割的单词进行分割并换行到下一行。                | 3   |

#### word-break

 CSS 属性 word-break 指定了怎样在单词内断行。

**可选值**

- **normal：**使用默认的断行规则。
- **break-all：**<font color=FF0000>对于**非CJK (CJK 指中文/日文/韩文) 文本**，可在任意字符间断行</font>。
- **keep-all：**CJK 文本不断行。 Non-CJK 文本表现同 normal。
- **break-word ：（<font color=FF0000>废弃</font>）**他的效果是word-break: normal 和 overflow-wrap: anywhere  的合，不论 overflow-wrap的值是多少。

摘自：[MDN - word-break](https://developer.mozilla.org/zh-CN/docs/Web/CSS/word-break)

#### overflow-wrap

CSS 属性 overflow-wrap 是用来说明当一个不能被分开的字符串太长而不能填充其包裹盒时，为防止其溢出，浏览器是否允许这样的单词中断换行。

**值**

- **normal：**行只能在正常的单词断点处中断。（例如两个单词之间的空格）。
- **break-word：**表示如果行内没有多余的地方容纳该单词到结尾，则那些正常的不能被分割的单词会被强制分割换行。

与word-break相比，overflow-wrap仅在无法将整个单词放在自己的行而不会溢出的情况下才会产生中断。

注：<font color=FF0000>word-wrap 属性原本属于微软的一个私有属性，在 CSS3 现在的文本规范草案中已经被重名为 overflow-wrap</font> 。 word-wrap 现在被当作 overflow-wrap 的 “别名”。 稳定的谷歌 Chrome 和 Opera 浏览器版本支持这种新语法。

摘自：[MDN - overflow-wrap](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow-wrap)



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

| 描述符           | 值                                                                                                                                                    | 描述                                        |
|:-------------:|:---------------------------------------------------------------------------------------------------------------------------------------------------- |:-----------------------------------------:|
| font-family   | *name*                                                                                                                                               | 必需。规定字体的名称。                               |
| src           | *URL*                                                                                                                                                | 必需。定义字体文件的 URL。                           |
| font-stretch  | normal<br/>condensed<br/>ultra-condensed<br/>extra-condensed<br/>semi-condensed<br/>expanded<br/>semi-expanded<br/>extra-expanded<br/>ultra-expanded | 可选。定义如何拉伸字体。默认是 "normal"。                 |
| font-style    | normalitalicoblique                                                                                                                                  | 可选。定义字体的样式。默认是 "normal"。                  |
| font-weight   | normal<br/>bold<br/>100<br/>200<br/>300<br/>400<br/>500<br/>600<br/>700<br/>800<br/>900                                                              | 可选。定义字体的粗细。默认是 "normal"。                  |
| unicode-range | *unicode-range*                                                                                                                                      | 可选。定义字体支持的 UNICODE 字符范围。默认是 "U+0-10FFFF"。 |



#### text-orientation

text-orientation CSS 属性<font color=FF0000>设定行中字符的方向</font>。但<font color=FF0000>它仅影响纵向模式（当 writing-mode 的值不是horizontal-tb）下的文本</font>。此属性在控制使用竖排文字的语言的显示上很有作用，也可以用来构建垂直的表格头。

- **关键值**
  - **mixed：**<font color=FF0000>默认值</font>。<font color=FF0000>顺时针旋转水平书写的字符90°</font>，将垂直书写的文字自然布局。
  - **upright：**<font color=FF0000>将水平书写的字符自然布局（直排）</font>，包括垂直书写的文字（as well as the glyphs for vertical scripts）。注意这个关键字会导致所有字符被视为从左到右，也就是 direction 被强制设为 ltr。
  - **sideways：**<font color=FF0000>所有字符被布局为与水平方式一样，但是整行文本被顺时针旋转90°</font>。
  - **sideways-right：**处于兼容目的，sideways 的别名。
  - **use-glyph-orientation：**对于SVG元素，这个关键字导致使用已弃用的SVG属性glyph-orientation-vertical 和 glyph-orientation-horizontal。

- **全局值**
  - inherit
  - initial
  - unset

摘自：[MDN - text-orientation](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-orientation)

#### writing-mode

<font color=FF0000>writing-mode 属性定义了文本水平或垂直排布以及在块级元素中文本的行进方向</font>。为整个文档设置时，应在根元素上设置它（对于 HTML 文档应该在 html 元素上设置）
<font color=FF0000>此属性指定块流动方向，即块级容器堆叠的方向，以及行内内容在块级容器中的流动方向</font>。因此，它也确定块级内容的顺序。

- **关键值**
  - **horizontal-tb：**  对于左对齐(ltr)脚本，内容从左到右水平流动。对于右对齐(rtr)脚本，内容从右到左水平流动。下一水平行位于上一行下方。
  - **vertical-rl：** 对于左对齐(ltr)脚本，内容从上到下垂直流动，下一垂直行位于上一行左侧。对于右对齐(rtr)脚本，内容从下到上垂直流动，下一垂直行位于上一行右侧。
  - **vertical-lr：** 对于左对齐(ltr)脚本，内容从上到下垂直流动，下一垂直行位于上一行右侧。对于右对齐(rtr)脚本，内容从下到上垂直流动，下一垂直行位于上一行左侧。
  - **sideways-rl：**（实验属性）对于左对齐(ltr)脚本，内容从下到上垂直流动。对于右对齐(rtr)脚本，内容从上到下垂直流动。所有字形（即使是垂直脚本中的字形）都朝向右侧。
  - **sideways-lr：**（实验属性）对于左对齐(ltr)脚本，内容从上到下垂直流动。对于右对齐(rtr)脚本，内容从下到上垂直流动。所有字形（即使是垂直脚本中的字形）都朝向左侧。
- **全局值**
  - inherit
  - initial
  - unset

摘自：[MDN - writing-mode](https://developer.mozilla.org/zh-CN/docs/Web/CSS/writing-mode)



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
  
- **matrix()**
  
  matrix()方法将2D变换方法合并成一个。matrix 方法有六个参数，包含旋转，缩放，移动（平移）和倾斜功能。
  
  <font color=FF0000>matrix(a, b, c, d, tx, ty) 是 matrix3d(a, b, 0, 0, c, d, 0, 0, 0, 0, 1, 0, tx, ty, 0, 1) 的简写</font>。

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

<font size=4>**补充：**</font>

**transform**
CSS transform 属性<font color=FF0000>允许你旋转，缩放，倾斜或平移给定元素</font>。这是通过修改CSS视觉格式化模型的坐标空间来实现的。

摘自：[MDN - transform](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform)

**transform-origin**

transform-origin CSS属性让你<font color=FF0000>更改一个元素变形的原点</font>。<font color=FF0000>默认的转换原点是 center</font>

transform-origin<font color=FF0000>属性可以使用一个，两个或三个值来指定</font>，其中<font color=FF0000>**每个值都表示一个偏移量**</font>。 <mark>没有明确定义的偏移将重置为其对应的初始值</mark>。

如果定义了两个或更多值并且没有值的关键字，或者唯一使用的关键字是center，则第一个值表示水平偏移量，第二个值表示垂直偏移量。

- **一个值：**

  必须是\<length>，\<percentage>，<font color=FF0000>或</font> left, center, right, top, bottom关键字中的一个。

- **两个值：**

  - 其中一个必须是\<length>，\<percentage>，<font color=FF0000>或</font> left, center, <font color=FF0000>right</font>关键字中的一个。
  - 另一个必须是\<length>，\<percentage>，<font color=FF0000>或</font> top, center, <font color=FF0000>bottom</font>关键字中的一个。

- **三个值：**

  - 前两个值和只有两个值时的用法相同。
  - <font color=FF0000>**第三个值必须是\<length>**</font>。<font color=FF0000>它始终代表Z轴偏移量</font>。

**值**

- **x-offset：**定义变形中心距离盒模型的左侧的\<length>或\<percentage>偏移值。
- **offset-keyword：**left，right，top，bottom或center中之一，定义相对应的变形中心偏移。
- **y-offset：**定义变形中心距离盒模型的顶的\<length>或\<percentage>偏移值。
- **x-offset-keyword：**left，right或center中之一，定义相对应的变形中心偏移。
- **y-offset-keyword：**top，bottom或center中之一，定义相对应的变形中心偏移。
- **z-offset：**定义变形中心距离用户视线（z=0处）的\<length>（不能是\<percentage>）偏移值。

**关键字是方便的简写方法，等同于以下\<percentage>值：**

| keyword | value |
| :------ | :---: |
| left    |  0%   |
| center  |  50%  |
| right   | 100%  |
| top     |  0%   |
| bottom  | 100%  |

摘自：[MDN - transform-origin](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-origin)



#### CSS 3D转换

**浏览器支持**

表格中的数字表示支持该属性的第一个浏览器版本号。

紧跟在 -webkit-, -ms- 或 -moz- 前的数字为支持该前缀属性的第一个浏览器版本号。

| 属性                                    | Chrome             | Edge | Firefox         | Safari       | Opera              |
|:------------------------------------- | ------------------ | ---- | --------------- | ------------ | ------------------ |
| transform                             | 36.0 12.0 -webkit- | 10.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |
| transform-origin (three-value syntax) | 36.0 12.0 -webkit- | 10.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |
| transform-style                       | 36.0 12.0 -webkit- | 11.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |
| perspective                           | 36.0 12.0 -webkit- | 10.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |
| perspective-origin                    | 36.0 12.0 -webkit- | 10.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |
| backface-visibility                   | 36.0 12.0 -webkit- | 10.0 | 16.0 10.0 -moz- | 4.0 -webkit- | 23.0 15.0 -webkit- |

**转换属性**

| 属性                                                                                    | 描述                   | CSS |
|:------------------------------------------------------------------------------------- |:-------------------- |:--- |
| [transform](https://www.runoob.com/cssref/css3-pr-transform.html)                     | 向元素应用 2D 或 3D 转换。    | 3   |
| [transform-origin](https://www.runoob.com/cssref/css3-pr-transform-origin.html)       | 允许你改变被转换元素的位置。       | 3   |
| [transform-style](https://www.runoob.com/cssref/css3-pr-transform-style.html)         | 规定被嵌套元素如何在 3D 空间中显示。 | 3   |
| [perspective](https://www.runoob.com/cssref/css3-pr-perspective.html)                 | 规定 3D 元素的透视效果。       | 3   |
| [perspective-origin](https://www.runoob.com/cssref/css3-pr-perspective-origin.html)   | 规定 3D 元素的底部位置。       | 3   |
| [backface-visibility](https://www.runoob.com/cssref/css3-pr-backface-visibility.html) | 定义元素在不面对屏幕时是否可见。     | 3   |

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

| 属性                                                                                                  | 描述                      | CSS |
|:--------------------------------------------------------------------------------------------------- |:----------------------- |:--- |
| [transition](https://www.runoob.com/cssref/css3-pr-transition.html)                                 | 简写属性，用于在一个属性中设置四个过渡属性。  | 3   |
| [transition-property](https://www.runoob.com/cssref/css3-pr-transition-property.html)               | 规定应用过渡的 CSS 属性的名称。      | 3   |
| [transition-duration](https://www.runoob.com/cssref/css3-pr-transition-duration.html)               | 定义过渡效果花费的时间。默认是 0。      | 3   |
| [transition-timing-function](https://www.runoob.com/cssref/css3-pr-transition-timing-function.html) | 规定过渡效果的时间曲线。默认是 "ease"。 | 3   |
| [transition-delay](https://www.runoob.com/cssref/css3-pr-transition-delay.html)                     | 规定过渡效果何时开始。默认是 0。       | 3   |



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

| 属性                                                                                                | 描述                                           | CSS |
|:------------------------------------------------------------------------------------------------- |:-------------------------------------------- |:--- |
| [@keyframes](https://www.runoob.com/cssref/css3-pr-animation-keyframes.html)                      | 规定动画。                                        | 3   |
| [animation](https://www.runoob.com/cssref/css3-pr-animation.html)                                 | 所有动画属性的简写属性。                                 | 3   |
| [animation-name](https://www.runoob.com/cssref/css3-pr-animation-name.html)                       | 规定 @keyframes 动画的名称。                         | 3   |
| [animation-duration](https://www.runoob.com/cssref/css3-pr-animation-duration.html)               | 规定动画完成一个周期所花费的秒或毫秒。默认是 0。                    | 3   |
| [animation-timing-function](https://www.runoob.com/cssref/css3-pr-animation-timing-function.html) | 规定动画的速度曲线。默认是 "ease"。                        | 3   |
| [animation-fill-mode](https://www.runoob.com/cssref/css3-pr-animation-fill-mode.html)             | 规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式。 | 3   |
| [animation-delay](https://www.runoob.com/cssref/css3-pr-animation-delay.html)                     | 规定动画何时开始。默认是 0。                              | 3   |
| [animation-iteration-count](https://www.runoob.com/cssref/css3-pr-animation-iteration-count.html) | 规定动画被播放的次数。默认是 1。                            | 3   |
| [animation-direction](https://www.runoob.com/cssref/css3-pr-animation-direction.html)             | 规定动画是否在下一周期逆向地播放。默认是 "normal"。               | 3   |
| [animation-play-state](https://www.runoob.com/cssref/css3-pr-animation-play-state.html)           | 规定动画是否正在运行或暂停。默认是 "running"。                 | 3   |



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

| 属性                                                                                | 描述                                 |
|:--------------------------------------------------------------------------------- |:---------------------------------- |
| [column-count](https://www.runoob.com/cssref/css3-pr-column-count.html)           | 指定元素应该被分割的列数。                      |
| [column-fill](https://www.runoob.com/cssref/css3-pr-column-fill.html)             | 指定如何填充列                            |
| [column-gap](https://www.runoob.com/cssref/css3-pr-column-gap.html)               | 指定列与列之间的间隙                         |
| [column-rule](https://www.runoob.com/cssref/css3-pr-column-rule.html)             | 所有 column-rule-* 属性的简写             |
| [column-rule-color](https://www.runoob.com/cssref/css3-pr-column-rule-color.html) | 指定两列间边框的颜色                         |
| [column-rule-style](https://www.runoob.com/cssref/css3-pr-column-rule-style.html) | 指定两列间边框的样式                         |
| [column-rule-width](https://www.runoob.com/cssref/css3-pr-column-rule-width.html) | 指定两列间边框的厚度                         |
| [column-span](https://www.runoob.com/cssref/css3-pr-column-span.html)             | 指定元素要跨越多少列                         |
| [column-width](https://www.runoob.com/cssref/css3-pr-column-width.html)           | 指定列的宽度                             |
| [columns](https://www.runoob.com/cssref/css3-pr-columns.html)                     | column-width 与 column-count 的简写属性。 |



#### CSS3 用户界面

- **resize**：<font color=FF0000>指定一个元素是否应该由用户去调整大小（比如用户改变窗口的大小）</font>。示例：
  
  ```css
  div {
      resize: both;
      overflow: auto;
  }
  ```
  
  **取值：**none | both | horizontal | vertical | block | inline
  
  - **none：** 元素不能被用户缩放。
  - **both：** 允许用户在水平和垂直方向上调整元素的大小。
  - **horizontal：** 允许用户在水平方向上调整元素的大小。
  - **vertical：** 允许用户在垂直方向上调整元素的大小。
  - **block**
  - **inline**
  
  <font color=FF0000>如果一个block元素的 overflow 属性被设置成了visible，那么resize属性对该元素无效</font>。
  
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
  
  在 CSS 盒子模型的默认定义里，你对一个元素所设置的 width 与 height 只会应用到这个元素的内容区。如果这个元素有任何的 border 或 padding ，绘制到屏幕上时的盒子宽度和高度会加上设置的边框和内边距值。这意味着当你调整一个元素的宽度和高度时需要时刻注意到这个元素的边框和内边距。当我们实现响应式布局时，这个特点尤其烦人。
  
  box-sizing 属性可以被用来调整这些表现:
  
  - **content-box：**是默认值。如果你设置一个元素的宽为100px，那么这个元素的内容区会有100px 宽，并且任何边框和内边距的宽度都会被增加到最后绘制出来的元素宽度中。
  - **border-box：**告诉浏览器：你想要设置的边框和内边距的值是包含在width内的。也就是说，如果你将一个元素的width设为100px，那么这100px会包含它的border和padding，内容区的实际宽度是width减去(border + padding)的值。大多数情况下，这使得我们更容易地设定一个元素的宽高。
  - **inherit：**继承 父元素 box-sizing属性的值
  
  另外：border-box不包含margin
  
  摘自：[MDN - box-sizing](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-sizing)
  
  **补充：**（如果上面没有看懂的话，可以看看下面的）
  
  盒子模型中盒子宽度和高度的计算方式会根据<font color=FF0000>盒子的类型的不同而不同</font>，而盒子类型是根据box-sizing的属性设置的。
  
  - box-sizing的默认值是content-box。css属性中的width和height设置的是内容区域的宽高，而盒子的宽高则需要加上内边距（padding）和宽高（width / height）
  - border-box：css中width和height的值分别是content宽高 + padding + border的值的和
  
  摘自：[动画解释 CSS 盒子模型、布局方式、box-sizing、替换元素和边距塌陷，CSS 布局不再难！](https://www.bilibili.com/video/BV13V411z7do)
  
  content-box：标准盒模型，CSS定义的宽高只包含content的宽高
  border-box：IE盒模型，CSS定义的宽高包括了content，padding和border
  
  摘自：[学会使用box-sizing布局](https://www.jianshu.com/p/e2eb0d8c9de6)

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

| 属性                                                                          | 说明                      | CSS |
|:--------------------------------------------------------------------------- |:----------------------- |:--- |
| [appearance](https://www.runoob.com/cssref/css3-pr-appearance.html)         | 允许您使一个元素的外观像一个标准的用户界面元素 | 3   |
| [box-sizing](https://www.runoob.com/cssref/css3-pr-box-sizing.html)         | 允许你以适应区域而用某种方式定义某些元素    | 3   |
| [icon](https://www.runoob.com/cssref/css3-pr-icon.html)                     | 为创作者提供了将元素设置为图标等价物的能力。  | 3   |
| [nav-down](https://www.runoob.com/cssref/css3-pr-nav-down.html)             | 指定在何处使用箭头向下导航键时进行导航     | 3   |
| [nav-index](https://www.runoob.com/cssref/css3-pr-nav-index.html)           | 指定一个元素的Tab的顺序           | 3   |
| [nav-left](https://www.runoob.com/cssref/css3-pr-nav-left.html)             | 指定在何处使用左侧的箭头导航键进行导航     | 3   |
| [nav-right](https://www.runoob.com/cssref/css3-pr-nav-right.html)           | 指定在何处使用右侧的箭头导航键进行导航     | 3   |
| [nav-up](https://www.runoob.com/cssref/css3-pr-nav-up.html)                 | 指定在何处使用箭头向上导航键时进行导航     | 3   |
| [outline-offset](https://www.runoob.com/cssref/css3-pr-outline-offset.html) | 外轮廓修饰并绘制超出边框的边缘         | 3   |
| [resize](https://www.runoob.com/cssref/css3-pr-resize.html)                 | 指定一个元素是否是由用户调整大小        | 3   |



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
  
  CSS3 box-sizing 属性在一个元素的 width 和 height 中包含 padding(内边距) 和 border(边框)。
  
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

- <font color=FF0000 size="5">**order**：</font><font color=FF0000>（容易被忽略）</font>定义项目的<font color=FF0000>排列顺序</font>。数值越小，排列越靠前，默认为0，可以为负值。]
  
  ```css
  .item {
      order: <integer>;
  }
  ```

- **flex-grow**定义项目的<font color=FF0000>**放大比例**</font>，<font color=FF0000>**默认为0**</font>，<font color=FF0000>即如果存在剩余空间，也不放大</font>。
  
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
      1 | initial | none | inherit |  [ flex-grow ] || [ flex-shrink ] || [ flex-basis ]
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
  
  <font color=FF0000>补充：flex: 1也可以表示占满剩余空间（相对于后面70%的，见后面 --> ）</font>，同时相对应的：`flex: 1 1 70%`表示占据70%的空间

- flex: initial; === flex: 0 1 auto;

- flex: none; === flex: 0 0 auto;

​            摘自：[flex:1 到底代表什么?](https://zhuanlan.zhihu.com/p/136223806)

- place-content 是 align-content 和 justify-content 的简写属性；而 <font color=FF0000>place-items 是 align-items 和 justify-items 的简写属性</font>。即：
  
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

| 值      | 描述               |
|:------ |:---------------- |
| all    | 用于所有多媒体类型设备      |
| print  | 用于打印机            |
| screen | 用于电脑屏幕，平板，智能手机等。 |
| speech | 用于屏幕阅读器          |

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

- 如果 background-size 属性设置为 <font color=FF0000>"contain"</font>, 背景图片将<font color=FF0000>按比例自适应内容区域</font>。

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
| [*element*>*element*](https://www.w3school.com.cn/cssref/selector_element_gt.asp) | div>p                 | 选择<font color=FF0000>父元素</font>为\<div>元素的所有\<p>元素。 |    2    |
| [*element*+*element*](https://www.w3school.com.cn/cssref/selector_element_plus.asp) | div+p                 | 选择<font color=FF0000>紧接在\<div>元素之后</font>的所有\<p>元素。 |    2    |
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
| [:empty](https://www.w3school.com.cn/cssref/selector_empty.asp) | p:empty               | 选择<font color=FF0000>没有子元素</font>的每个\<p>元素（<font color=FF0000>包括文本节点</font>）。 |    3    |
| [:target](https://www.w3school.com.cn/cssref/selector_target.asp) | #news:target          | 选择当前活动的 #news 元素。                                  |    3    |
| [:enabled](https://www.w3school.com.cn/cssref/selector_enabled.asp) | input:enabled         | 选择每个启用的\<input>元素。                                 |    3    |
| [:disabled](https://www.w3school.com.cn/cssref/selector_disabled.asp) | input:disabled        | 选择每个禁用的\<input>元素                                   |    3    |
| [:checked](https://www.w3school.com.cn/cssref/selector_checked.asp) | input:checked         | 选择每个被选中的\<input>元素。                               |    3    |
| [:not(*selector*)](https://www.w3school.com.cn/cssref/selector_not.asp) | :not(p)               | 选择非\<p>元素的每个元素。                                   |    3    |
| [::selection](https://www.w3school.com.cn/cssref/selector_selection.asp) | ::selection           | 选择被用户选取的元素部分。                                   |    3    |

摘自：[runoob - CSS 选择器](https://www.runoob.com/cssref/css-selectors.html)

![v2-18e482112459a3b01c95119bae5e2f4b_720w](https://i.loli.net/2021/05/31/hExIljRiqPX6kOy.jpg)

**补充：**

关于nth-of-type：CSS 伪类是<font color=FF0000>针对具有一组兄弟节点的标签, 用 n 来筛选出在一组兄弟节点的位置</font>。示例如下。<font color=FF0000>n从1开始</font>

```css
/* 在每组兄弟元素中选择第四个 <p> 元素 */
p:nth-of-type(4n) {
  color: lime;
}
```



#### 给placeholoder加上颜色

- 方法一：用`:: placeholder`（<font color=FF0000>比较简洁，但这是一个实验功能，兼容性比较差</font>）

  伪元素 ::placeholder可以选择一个表单元素的占位文本，它允许开发者和设计师自定义占位文本的样式。

  示例：

  ```css
  ::placeholder {
    color: red;
    font-size: 1.5em;
  }
  ```

- 方法二：不同浏览器分别解决

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



#### div实现“可输入”

可以使用contenteditable属性。示例如下：

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

**补充：<font size='4'>document.execCommand</font>（<font color=FF0000>注意：已废弃，详见[CanIuse-execCommand](https://caniuse.com/?search=execCommand)</font>）**

当一个HTML文档切换到设计模式时，document暴露 execCommand 方法，该方法允许运行命令来操纵可编辑内容区域的元素。

大多数命令影响document的 selection（粗体，斜体等），当其他命令插入新元素（添加链接）或影响整行（缩进）。当使用contentEditable时，调用 execCommand() 将影响当前活动的可编辑元素。

- **语法**

  ```js
  bool = document.execCommand(aCommandName, aShowDefaultUI, aValueArgument)
  ```

- **返回值**：一个 Boolean ，如果是 false 则表示操作不被支持或未被启用。

- **参数**
  - **aCommandName：**一个 DOMString ，命令的名称。可用命令列表请参阅 [命令](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/execCommand#%E5%91%BD%E4%BB%A4) 。
  - **aShowDefaultUI：**一个 Boolean， 是否展示用户界面，一般为 false。Mozilla 没有实现。
  - **aValueArgument：**一些命令（例如insertImage）需要额外的参数（insertImage需要提供插入image的url），默认为null。

- **命令**

  由于过长：略。地址： [命令](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/execCommand#%E5%91%BD%E4%BB%A4) 

摘自：[MDN - document.execCommand](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/execCommand)



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

**值**

- **\<custom-property-name> 自定义属性名：**在实际应用中它被定义为以两个破折号开始的任何有效标识符。 自定义属性仅供作者和用户使用; CSS 将永远不会给他们超出这里表达的意义。
- **\<declaration-value> 声明值（后备值）：**回退值被用来在自定义属性值无效的情况下保证函数有值。回退值可以包含任何字符，但是部分有特殊含义的字符除外，例如换行符、不匹配的右括号（如`)`、 `]`或`}`）、感叹号以及顶层分号（不被任何非`var()`的括号包裹的分号，例如`var(--bg-color, --bs;color)`是不合法的，而`var(--bg-color, --value(bs;color))`是合法的）。

摘自：[MDN - var()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/var())

**补充：**CSS自定义属性

自定义属性（有时候也被称作CSS变量或者级联变量）是由CSS作者定义的，它包含的值可以在整个文档中重复使用。由自定义属性标记设定值（比如： --main-color: black;），由var() 函数来获取值（比如： color: var(--main-color);）

注意：自定义属性名是大小写敏感的，--my-color 和 --My-color 会被认为是两个不同的自定义属性。

摘自：[MDN - 使用CSS自定义属性（变量）](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Using_CSS_custom_properties)

#### counter()

CSS 函数 counter()，返回一个代表计数器的当前值的字符串。它通常和伪元素搭配使用，但是理论上可以在支持\<string>值的任何地方使用。

```css
/* 简单使用 */
counter(计数器名称);

/* 更改计数器显示 */
counter(countername, upper-roman)
```

#### clamp()

clamp() 函数的作用是<font color=FF0000>把一个值限制在一个上限和下限之间，当这个值超过最小值和最大值的范围时，在最小值和最大值之间选择一个值使用</font>。<mark style="background: aqua"><font size=4>它接收三个参数：最小值、**首选值**、最大值</font></mark>。 clamp() 被用在 \<length>、\<frequency>、\<angle>、\<time>、\<percentage>、\<number>、\<integer> 中都是被允许的。

<mark>clamp(MIN, VAL, MAX) 其实就是表示 max(MIN, min(VAL, MAX))</mark>

**语法**
clamp() 函数接收三个用逗号分隔的表达式作为参数，按最小值、首选值、最大值的顺序排列。

- <font color=FF0000>当首选值比最小值要小时，则使用最小值</font>。
- 当首选值介于最小值和最大值之间时，用首选值。
- <font color=FF0000>当首选值比最大值要大时，则使用最大值</font>。

摘自：[MDN - clamp()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clamp())



#### **补充：CSS函数**

根据w3cplus中可以划分为以下几类：

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

**或者：**

![](https://www.w3cplus.com/sites/default/files/blogs/2020/2005/css-function-5.svg)

**url()**

url()不再局限于`background-image`属性中，还可以用于：

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

简单地说，在CSS中可以使用url()函数来引用相应的资源，有点类似于HTML中的src，href属性。

**attr()**

原则上说，attr()能运用于所有的CSS属性，但目前仅能服务于CSS的伪元素::before和::after的content属性。



原文: https://www.w3cplus.com/css/css-functions-guide.html 



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



#### :indeterminate

`:indeterminate` <font color=FF0000>CSS 伪类</font> 表示状态不确定的表单元素

表单选择框元素（checkbox）状态样式如下：

<img src="https://i.loli.net/2021/02/23/gOs3ADKmN6GWerE.png" alt="https://i2.wp.com/css-tricks.com/wp-content/uploads/2016/12/indeterminate-checkboxes.png?w=1430&ssl=1" style="zoom: 50%;" />

`:indeterminate`可作用的对象有：

- \<input type="checkbox"> 元素，其 indeterminate 属性被 JavaScript设置为 true 。
- \<input type="radio"> 元素, 表单中拥有相同 name值的所有单选按钮都未被选中时。
- 处于不确定状态的 \<progress> 元素

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

典型的使用方法是后代选择器表达式。例如 h1，<mark>只选择在\<h1> 内的自定义元素的实例</mark>。

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

<mark>CSS 的 outline 属性是在一条声明中设置多个轮廓属性的简写属性</mark> ， 例如 outline-style, outline-width 和 outline-color。 

**border 和 outline：**border 和 outline 很类似，但有如下区别：

- outline<font color=FF0000>不占据空间</font>，绘制于元素内容周围。
- 根据规范，outline通常是矩形，但也可以是非矩形的。

摘自：[MDN - outline](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline)



#### user-select

- CSS 属性 user-select 控制用户能否选中文本。除了文本框内，它对被载入为 chrome 的内容没有影响。

- **形式化语法**
  auto | text | none | contain | all
- **语法**
  - **none：**元素及其子元素的文本不可选中。 请注意这个Selection 对象可以包含这些元素。 从Firefox 21开始， none 表现的像 -moz-none，因此可以使用 -moz-user-select: text 在子元素上重新启用选择。
  - **auto：**auto 的具体取值取决于一系列条件，具体如下：
    - 在 ::before 和 ::after 伪元素上，采用的属性值是 none
    - 如果元素是可编辑元素，则采用的属性值是 contain
    - 否则，如果此元素的父元素的 user-select 采用的属性值为 all，则该元素采用的属性值也为 all
    - 否则，如果此元素的父元素的 user-select 采用的属性值为 none，则该元素采用的属性值也为 none
    - 否则，采用的属性值为 text
  - **text：**用户可以选择文本。
  - **all：**在一个HTML编辑器中，当双击子元素或者上下文时，那么包含该子元素的最顶层元素也会被选中。
  - **contain：**允许在元素内选择；但是，选区将被限制在该元素的边界之内。
  - **element**（IE 专有别名）：与 contain 相同，但仅在 Internet Explorer 中受支持。
    注意： CSS UI 4 已将 user-select 的 element 属性值重命名为 contain。

摘自：[MDN - user-select](https://developer.mozilla.org/zh-CN/docs/Web/CSS/user-select)



#### autocomplete

默认情况下，浏览器会记录用户网页上提交的输入框的信息。这使得浏览器能够提供自动补全（在用户开始输入的时候给用户提供可能的内容）和自动填充（在加载的时候预先填充某些字段）功能。

这些功能通常是默认启用的，但可能涉及用户的隐私，因此浏览器允许用户禁用这些功能。

要禁用的表单自动填充，你可以将 autocomplete 的属性设置为 "off"：`autocomplete = "off"`

设置 `autocomplete="off"` 会有两种效果：

- 这会告诉浏览器，不要为了以后在类似表单上自动填充而保存用户输入的数据。但浏览器不一定遵守。
- 这会阻止浏览器缓存会话历史记录中的数据。若表单数据缓存于会话历史记录，用户提交表单后，再点击返回按钮返回之前的表单页面，则会显示用户之前输入的数据。

摘自：[MDN - 如何关闭表单自动填充](https://developer.mozilla.org/zh-CN/docs/Web/Security/Securing_your_site/Turning_off_form_autocompletion)



#### :target伪类

:target CSS 伪类 代表一个唯一的页面元素(目标元素)，其id 与当前<font color=FF0000>URL片段</font>（section，也可以被称为锚点？）匹配 

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



#### 实现图片切换效果的两种方法，值得参考：

- 用单纯的CSS（更优雅。但当hover事件发生时，似乎只能对本级元素进行修改）

  ```html
  <img src="icon_rise@2x.png" alt="" id="img" />
  
  <style>
    #img:hover {
      content: url(icon_discharge@2x.png)
    }
  </style>
  ```

  参考自：[CSS小技巧之替换图片(content)](https://blog.csdn.net/qq_45745643/article/details/108697191)

- JS（更灵活。当hover事件发生时，可以对任意位置的元素进行修改）

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

|                     值                      |                             说明                             |
| :-----------------------------------------: | :----------------------------------------------------------: |
|                    none                     |                设置Content，如果指定成Nothing                |
|                   normal                    | 设置content，如果指定的话，正常，默认是"none"（该是nothing） |
|                   counter                   |                        设定计数器内容                        |
| <font color=FF0000>attr*(attribute)*</font> |  <font color=FF0000>设置Content作为选择器的属性之一</font>   |
|                  *string*                   |                  设置Content到你指定的文本                   |
|                 open-quote                  |                    设置Content是开口引号                     |
|                 close-quote                 |                    设置Content是闭合引号                     |
|                no-open-quote                |                 如果指定，移除内容的开始引号                 |
|               no-close-quote                |                 如果指定，移除内容的闭合引号                 |
|    <font color=FF0000>url(*url*)</font>     | <font color=FF0000>设置某种媒体（图像，声音，视频等内容）</font> |
|                   inherit                   |           指定的content属性的值，应该从父元素继承            |

摘自：[CSS content 属性](https://www.runoob.com/cssref/pr-gen-content.html)



#### object-fit

<font color=FF0000>object-fit CSS 属性**指定可替换元素**</font>（这个名词，隔壁的《CSS权威指南阅读笔记》有解释）<font color=FF0000>的内容应该如何适应到其使用的高度和宽度确定的框。</font>
<mark>您可以通过使用 object-position 属性来切换被替换元素的内容对象在元素框内的对齐方式。</mark>

**取值**

- **contain**
  被替换的内容将被缩放，以在填充元素的内容框时保持其宽高比。 整个对象在填充盒子的同时保留其长宽比，因此如果宽高比与框的宽高比不匹配，该对象将被添加“黑边”。
- **cover**
  被替换的内容在保持其宽高比的同时填充元素的整个内容框。如果对象的宽高比与内容框不相匹配，该对象将被剪裁以适应内容框。
- **fill<font color=FF0000>（默认）</font>**
  被替换的内容正好填充元素的内容框。整个对象将完全填充此框。<font color=FF0000>如果对象的宽高比与内容框不相匹配，那么该对象将被拉伸以适应内容框</font>。
- **none**
  被替换的内容将保持其原有的尺寸。
- **scale-down**
  内容的尺寸与 none 或 contain 中的一个相同，取决于它们两个之间谁得到的对象尺寸会更小一些。

摘自：[MDN - object-fit](https://developer.mozilla.org/zh-CN/docs/Web/CSS/object-fit)

#### object-position

CSS 属性 object-position 规定了<font color=FF0000>可替换元素</font>的内容，在这里我们称其为对象（即 object-position 中的 object），在其内容框中的位置。可替换元素的内容框中未被对象所覆盖的部分，则会显示该元素的背景（background）。

摘自：[MDN - object-position](https://developer.mozilla.org/zh-CN/docs/Web/CSS/object-position)



#### \<iframe>

HTML内联框架元素 (\<iframe>) 表示嵌套的browsing context。它能够将另一个HTML页面嵌入到当前页面中。



#### pointer-events

pointer-events是css3的一个属性，指定在什么情况下元素可以成为鼠标事件的target（包括鼠标的样式）

pointer-events属性有很多值，但是对于浏览器来说，只有auto和none两个值可用，其它的几个是针对SVG的(本身这个属性就来自于SVG技术)。

- **auto（默认）：**效果和没有定义pointer-events属性相同（即默认），<font color=FF0000>鼠标不会穿透当前层</font>。在SVG中，该值和visiblePainted的效果相同。
- **none：**<font color=FF0000>元素永远不会成为鼠标事件的target（目标）。但是，当其后代元素的pointer-events属性指定其他值时，鼠标事件可以指向后代元素，在这种情况下，鼠标事件将在捕获或冒泡阶段触发父元素的事件侦听器。</font>

摘自：[非常有用的pointer-events属性](https://www.cnblogs.com/kunmomo/p/11752669.html)



#### text-decoration

text-decoration 这个 CSS 属性是用于设置文本的修饰线外观的（下划线、上划线、贯穿线/删除线  或 闪烁）它是 text-decoration-line, text-decoration-color, text-decoration-style, 和新出现的 text-decoration-thickness 属性的缩写。

- **text-decoration-line：**文本修饰的位置, 如下划线underline，删除线line-through
- **text-decoration-color：**文本修饰的颜色
- **text-decoration-style：**文本修饰的样式, 如波浪线wavy实线solid虚线dashed
- **text-decoration-thickness：**文本修饰线的粗细

摘自：[MDN - text-decoration](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration)



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

**可选值：**

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

**属性值**

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

CSS 的属性 vertical-align <font color=FF0000>用来指定行内元素（inline）或表格单元格（table-cell）元素的垂直对齐方式</font>。

**vertical-align属性可被用于两种环境：**

- 使行内元素盒模型与其行内元素容器垂直对齐。例如，用于垂直对齐一行文本内的图片

  <img src="https://i.loli.net/2021/08/20/qbGZoYeHary5Aus.png" alt="image-20210820110216620" style="zoom: 33%;" />

- 垂直对齐表格单元内容:

  <img src="https://i.loli.net/2021/08/20/uYdcp1AFb2vHUqK.png" alt="image-20210820110341093" style="zoom: 33%;" />

**注意：**vertical-align 只对行内元素、表格单元格元素生效：不能用它垂直对齐块级元素。

**vertical-align 属性指定为下面列出的值之一。**

行内元素的值

- **相对父元素的值：**这些值使元素相对其父元素垂直对齐：

  - **baseline：**使元素的基线与父元素的基线对齐。HTML规范没有详细说明部分可替换元素的基线，如<textarea> ，这意味着这些元素使用此值的表现因浏览器而异。
  - **sub：**使元素的基线与父元素的下标基线对齐。
  - **super：**使元素的基线与父元素的上标基线对齐。
  - **text-top：**使元素的顶部与父元素的字体顶部对齐。
  - **text-bottom：**使元素的底部与父元素的字体底部对齐。
  - **middle：**使元素的中部与父元素的基线加上父元素x-height（译注：x高度）的一半对齐。
  - **\<length>：**使元素的基线对齐到父元素的基线之上的给定长度。可以是负数。
  - **\<percentage>：**使元素的基线对齐到父元素的基线之上的给定百分比，该百分比是line-height属性的百分比。可以是负数。
    相对行的值

- **下列值使元素相对整行垂直对齐：**

  - **top：**使元素及其后代元素的顶部与整行的顶部对齐。
  - **bottom：**使元素及其后代元素的底部与整行的底部对齐。

  没有基线的元素，使用外边距的下边缘替代。

- 表格单元格的值
  - **baseline (以及 sub, super, text-top, text-bottom, \<length>, \<percentage>)：**使单元格的基线，与该行中所有以基线对齐的其它单元格的基线对齐。
  - **top：**使单元格内边距的上边缘与该行顶部对齐。
  - **middle：**使单元格内边距盒模型在该行内居中对齐。
  - **bottom：**使单元格内边距的下边缘与该行底部对齐。
    可以是负数。

摘自：[MDN - vertical-align](https://developer.mozilla.org/zh-CN/docs/Web/CSS/vertical-align)



#### text-overflow

<font color=FF0000>这个属性只对那些在**块级元素**溢出的内容有效</font>，但是必须要与块级元素内联(inline)方向一致<mark>（举个反例：内容在盒子的下方溢出。此时就不会生效）</mark>。文本可能在以下情况下溢出：当其因为某种原因而无法换行(例子：设置了**`white-space:nowrap`**)，或者一个单词因为太长而不能合理地被安置(fit)。

**一般可选值（被一般浏览器实现的）**

- **clip：**此为默认值。这个关键字的意思是"在内容区域的极限处截断文本"，因此在字符的中间可能会发生截断。<mark>如果你的目标浏览器支持</mark> `text-overflow: ''`，<mark>为了能在两个字符过渡处截断，你可以使用一个空字符串值</mark> (`''`) <mark>作为 text-overflow 属性的值。</mark>
- **ellipsis：**这个关键字的意思是“用一个省略号 (`'…'`, U+2026 HORIZONTAL ELLIPSIS)来表示被截断的文本”。这个省略号被添加在内容区域中，因此会减少显示的文本。<font color=FF0000>如果空间太小到连省略号都容纳不下，那么这个省略号也会被截断</font>。
- **\<string>：**\<string>用来表示被截断的文本。字符串内容将被添加在内容区域中，所以会减少显示出的文本。如果空间太小到连省略号都容纳不下，那么这个字符串也会被截断。

摘自：[MDN - text-overflow](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-overflow)



#### ARIA

Accessible Rich Internet Applications **(ARIA)** 是<font color=FF0000>能够让残障人士更加便利的访问 Web 内容和使用 Web 应用（特别是那些由JavaScript 开发的）的一套机制</font>。

ARIA是对超文本标记语言（HTML ）的补充，以便在没有其他机制的情况下，使得应用程序中常用的交互和小部件可以传递给辅助交互技术。例如，ARIA支持HTML4中的可访问导航地标、JavaScript小部件、表单提示和错误消息、实时内容更新等。

ARIA 是一组特殊的易用性属性，可以添加到任意标签上，尤其适用于 HTML。<font color=FF0000>role 属性定义了对象的通用类型（例如文章、警告，或幻灯片）</font>。额外的 ARIA 属性提供了其他有用的特性，例如表单的描述或进度条的当前值。

摘自：[MDN - ARIA](https://developer.mozilla.org/zh-CN/docs/Web/Accessibility/ARIA)



## 经验总结

**管理好CSS有助于提高项目性能**

说到CSS我们是势必要说到两个概念：**重绘 & 重排**

- **重绘：**重绘是指<mark>当 DOM 元素的属性发生变化 (如 color) 时</mark>, 浏览器会通知render 重新描绘相应的元素, 此过程称为重绘。
- **重排：**重排是指<mark style="background: aqua">某些元素变化涉及元素布局 (如width)</mark>，浏览器则抛弃原有属性, 重新计算，此过程称为重排。（重排一定会重绘，重绘不一定重排）。

<font color=FF0000>页面渲染的一般过程为 <font size=4>**JS > CSS > 计算样式 > 布局 > 绘制 > 渲染层合并**</font> 。而在这个过程中，**重排和重绘是整个环节中最为耗时的两环，从重绘和重排的概念上看，重排比重绘更加的消耗性能**</font>，所以我们尽量避免着这两个环节。从性能方面考虑,最理想的渲染流水线是没有布局和绘制环节的,只需要做渲染层的合并即可。

**如何更好的写CSS&HTML**

- 首先定义项目的基准样式：如重置样式,公用样式变量,兼容性处理等,且最好用less/sass/stylus等来写我们的css
- <mark>把项目的公共布局及样式抽离出来：如公用的头部，公用的尾部，公用的tab等</mark>
- 避免样式重复赋值，避免样式重叠：如避免在业务或者组件里面写全局样式，样式层级不要过深
- 用好z-index，position

摘自：[我们应该如何写好HTML&CSS](https://juejin.cn/post/6854573211548549127)

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

#### Boolean

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