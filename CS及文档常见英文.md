# CS 及 文档书籍 常见英文

### 通用

- **synchronous**： 同步

- **asynchronous**：异步

- **compatibility**：兼容性。compat：兼容 ( verb )

- **shortcut**：快捷方式，近道

- **out of the box**：开箱即用

- **built-in**：内置

- **addon**：插件。来自 “node-gyp is a cross-platform command-line tool written in Node.js for compiling native addon modules for Node.js”

- **on the fly**：即时，在运行时（热更新）。参考：[如何优雅的翻译 on the fly ？ - 知乎](https://www.zhihu.com/question/21136587)

- **pros and cons**：利弊

- **overhead**：开销（和性能相关）

- **from scratch**：从零开始

- **preflight**：预检。一般情况下的含义：预检请求 ( preflight request )。不过，在 antfu 的 UnoCSS 相关博客 [Reimagine Atomic CSS](https://antfu.me/posts/reimagine-atomic-css#scoping) 中 [scoping](https://antfu.me/posts/reimagine-atomic-css#scoping) 部分 发现了有 “样式预检” 的含义，翻译在 [重新构想原子化 CSS - CSS 作用域](https://antfu.me/posts/reimagine-atomic-css-zh#css-%E4%BD%9C%E7%94%A8%E5%9F%9F) 中

- **semver**：( Semantic version ) 语义化版本控制规范

- **instantiate**：实例化 ( verb )

- **nest**：嵌套，一般用形容词 nested ，嵌套的

- **recursion**：递归

- **i.e.** : *i.e.* is an abbreviation for the phrase ***id est***, which means **"that is"** .

- **retrieve**：检索

- **decoupled**：解耦的

- **critical**：关键的。一般见：关键渲染路径 ( Critical Rendering Path ) 。虽然更常见的翻译是 “批评性的”

- **gotcha** ：在计算机编程领域中是指在系统或程序、程序设计语言中，合法有效，但是会误解意思的构造，程式容易造成错误，或是一些易于使用但其结果不如期望的构造。字面上是 got you 的简写，常用于口语，**直译为： “逮着你了”、“捉弄到你了 ”、“你中计了” 、“骗到你了”**。

- **tricky**：困难的，棘手的

- **prune**：剪枝。这是一个一般性概念，可以用于 机器学习，数据库 以及  树形数据结构，也是前端构建 Tree Shaking 的概念。

  补充：git 有 prune 命令，用于清除 “不可达” 或 “孤儿 ( orphaned ) ” 的对象；详见：https://www.atlassian.com/git/tutorials/git-prune ；这里略。npm 也有 prune 命令：`npm prune [[<@scope>/]<pkg>...]` ，详见 [npm docs - npm-prune](https://docs.npmjs.com/cli/v8/commands/npm-prune)。同时 docker 也有，详见 [docker docs - Prune unused Docker objects](https://docs.docker.com/config/pruning/)

- **utilize**：利用

- **under the hood**：在引擎盖下（指内部实现）

- **wildcard**：通配符

- **spec**：规格，细则。abbr of specification

- **tackle**：解决

- **yield**：产出

- **mutation**：变异

- **Imperative programming**：命令式编程

- **arithmetic**：算术

- **in lieu in**：替代

- **omit**：删除、省略

- **summation**：总和。sum 的完整形式？

- **interchangable**：可交换的

- **precedent**：先例

- **walkthrough**：演练

- **casting function**：转型函数。关于翻译，在 [[JS及其相关库备忘录#包装类#JS 中的“原始值包装类型” ( Primitive wrapper types )]] 中有过说明

- **DRY**：即：Don't Repeat Yourself 。不重复（原则）

- **verbose**：冗长的、啰嗦的

- **redundant**：冗余的

- **shorthand**：速记

- **practice**：实践，比如 Best practice。感觉可以理解为“做法”，不过字典上没“做法”这种意思

- **unite**：使联合

- **wellspring**：源泉，来源

- **tweak**：调整

- **terse**：简短的

- **polymorphism**：多态性

- **bandaid**：绷带，创口贴

- **big word**：大词，又长又艰深的词汇，表达严肃或重要概念的字眼

- **modular**：模块化的

- **patch**：修复( verb )，补丁( noun )

- **hierarchy**：层级

- **Let's say**：比如说... ，一般用于开头开始话题

- **incur**：招致，发生

- **prioritize ( ... over ... )**：优先考虑

- **setup**：设置 ( noun )

- **set up**：设置 ( verb )

- **granular**：颗粒状的。more granular：更细粒度的。granul：颗粒。

- **fine-grained**：细粒度的

- **scenario**：场景。来自：” The `publicPath` configuration option can be quite useful in a variety of **scenarios** ” 。更普遍的意思是：脚本，假想

- **neat**：整洁的。引申为：**简单的**。来自：“ There are a few use cases in real applications where this feature becomes especially **neat** ”

- **dedicated**：专门的（来自：“ In such cases, you'll have to move the public path assignment to its own **dedicated** module and then import it on top of your entry.js ” )。更普遍的意思是：投入的

- **misconception**：误解 ( noun )，错误观念。

- **overlap**：重叠

- **workaround**：解决方法，变通方法

- **concurrency**：并发，并发数。**concurrent**：并发的

- **simultaneous**：同时（发生）的

- **compatible**：和睦相处的

- **schema**：设计，架构，概要

- **fallback**：后退，注意和 rollback（回滚）的区别

- **recipient**：收件人（在 网络 http 场景中出现），来自：https://datatracker.ietf.org/doc/html/rfc7231#section-4.3.7

- **interface** <font size=4>*vs*</font> **port**：interface 接口，比如 后端接口；port 端口，80 端口

- **ship**：提供 ( verb ) 。来自：“ This is an example for a package that has optimizations for production and development usage with runtime detection for `process.env` and also **ships** a CommonJs and ESM version ”

- **address**：关联 ( verb )，来自 “Each module available addresses a specific aspect of service worker development.” ，查阅谷歌翻译和百度翻译 都找不到相关的意思，应该是当前语境的引申义。另外，说一下快遗忘的含义：尝试解决

- **accommodate**：为 ... 提供住宿，这也是高中学习时普遍的含义。这里要强调的是，也可以翻译为：**顾及、适应、兼容**；如下： “ Workbox aims to make using service workers as easy as possible, while allowing the flexibility to **accommodate** complex application requirements where needed.”

- **house**：收容 ( verb )。来自：“ This repo **houses** two bundling libraries: a *modern Vite plugin* and a *legacy Rollup plugin* . ” ，这里翻译成“包含”更妥帖些。

- **anonymous**：匿名的。anonymity：匿名 ( noun )。deanonymize：匿名化

- **boilerplate**：样板

- **directive**：指令，命令

- **stale**：陈旧的，不过这里要说的“不新鲜的”，用于 HTTP 的 Cache-Control 中，比如 max-stale 选项

- **latency**：延时。high-latency 高延时。

- **underpin**：支撑，构成 … 的基础。来自： “ Because promises also **underpin** async and await , ... ”

- **as-is**：照原样

- **at the cost of**：以什么为代价。

- **fine tune**：微调

- **[prefix]-agnostic**：... 无关的。比如 framework-agnostic 译为 “框架无关的”。

- **profile**：剖析 ( verb )。来自 “ It is especially useful in the case of early prototyping and **profiling**.  ”

- **scaffold**：脚手架

- **negate**：否定。**negated**：否定的。参考记忆，可以根据 negative

- **as of**：截止，从 ... 开始


- **histogram**：直方图
- **omnibox**：地址栏
- **ultimate**：一般的含义是 “最终的”。不过这里要注意的是，还有 “**最后的**” 的意思。来自：“The `Compiler` is **ultimately** a function which performs bare minimum functionality to keep a lifecycle running.”


#### 特殊字符

因为 Google 是不支持 特殊字符 搜索的，想要带上 特殊字符 进行搜索，就必须要加上 字符的英文；所以这些就非常有必要记录

##### 括号 Bracket

- **() / 圆括号**：Round brackets / parentheses
- **\[] / 方括号**：Square brackets / brackets
- **{} / 花括号**： / 大括号：Curly brackets / braces
- **<> / 尖括号**：Angle brackets / chevrons

参考自：[wikipedia - Bracket](https://en.wikipedia.org/wiki/Bracket)

- **星号 `*`**：asterisk。在非正式的交流中也会被称为 “star” （比如 [stack overflow - Meaning of a double star (**) in a file path](https://stackoverflow.com/questions/46547540/meaning-of-a-double-star-in-a-file-path) ），不过，在 Google 搜索中 还是要使用 asterisk，star 没有用

- **连接号**：dash，是一种统称。详见：[wikipedia - 连接号](https://zh.wikipedia.org/wiki/%E8%BF%9E%E6%8E%A5%E5%8F%B7)
  - en dash：表示范围
  - em dash：表示预期转折
- **连字符 `-`**：hyphen。用于连接单词，将不同单词连接变成一个单词。

- **分号 `;`** ：semicolon

- **斜杠**
  - forward slash: `/`
  - backslash :  `\`
