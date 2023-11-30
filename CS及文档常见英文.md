# CS 及 文档书籍 常见英文



### 通用

- **synchronous**： 同步

- **asynchronous**：异步

- **compatibility**：兼容性。**forwards compatibility**：向后兼容性

  **compat**：兼容 ( verb )

- **shortcut**：快捷方式，近道

- **out of the box**：开箱即用

- **built-in**：内置

- **addon**：插件

   > node-gyp is a cross-platform command-line tool written in Node.js for compiling native addon modules for Node.js

- **on the fly**：即时，在运行时（热更新）

  > 💡 参考：[如何优雅的翻译 on the fly ？ - 知乎](https://www.zhihu.com/question/21136587)

- **on the spot** ：当场、立即

- **on-going** ：正在进行中的

- **day-to-day** ：日常

- **pros and cons**：利弊

- **pan and zoom** ：平移和缩放

   > 👀 之所以会放到这里，是因为搜了一下 pan 在这里有“平移” 的含义，但是一般字典中没有记录；可能是相关领域内约定俗成的说法吧

- **downside** ：缺点

- **overhead**：<font color=red>**开销**</font>（和性能相关）

- **vice versa** ：反之亦然

- **preflight**：预检。一般情况下的含义：预检请求 ( preflight request )。不过，在 antfu 的 UnoCSS 相关博客 [Reimagine Atomic CSS](https://antfu.me/posts/reimagine-atomic-css#scoping) 中 [scoping](https://antfu.me/posts/reimagine-atomic-css#scoping) 部分 发现了有 “样式预检” 的含义，翻译在 [重新构想原子化 CSS - CSS 作用域](https://antfu.me/posts/reimagine-atomic-css-zh#css-%E4%BD%9C%E7%94%A8%E5%9F%9F) 中

- **semver**：( semantic version ) 语义化版本控制规范

- **semantical** ：语义的

- **instantiate**：实例化 ( verb )

- **nest**：嵌套，一般用形容词 nested ，嵌套的

- **recursion**：递归。**recursive**：递归的

- **i.e.** : *i.e.* is an abbreviation for the phrase ***id est***, which means **"that is"** .

- **retrieve**：检索，获取

- **decoupled**：解耦的，decouple ( verb )。coupled 耦合的

- **critical**：关键的。一般见：关键渲染路径 ( Critical Rendering Path ) 。虽然更常见的翻译是 “批评性的”

- **gotcha** ：在计算机编程领域中是指在系统或程序、程序设计语言中，合法有效，但是会误解意思的构造，程式容易造成错误，或是一些易于使用但其结果不如期望的构造。字面上是 got you 的简写，常用于口语，**直译为： “逮着你了”、“捉弄到你了 ”、“你中计了” 、“骗到你了”**。

- **tricky**：困难的，棘手的

- **prune**：剪枝。这是一个一般性概念，可以用于 机器学习，数据库 以及  树形数据结构，也是前端构建 Tree Shaking 的概念。

  >💡 git 有 prune 命令，用于清除 “不可达” 或 “孤儿 ( orphaned ) ” 的对象；详见：https://www.atlassian.com/git/tutorials/git-prune ；这里略。npm 也有 prune 命令：`npm prune [[<@scope>/]<pkg>...]` ，详见 [npm docs - npm-prune](https://docs.npmjs.com/cli/v8/commands/npm-prune)。同时 docker 也有，详见 [docker docs - Prune unused Docker objects](https://docs.docker.com/config/pruning/)

- **utilize**：利用

- **under the hood**：在引擎盖下（指内部实现）

- **underlying**：内在的

- **internal** ：里面的，内部的

- **wildcard**：通配符

- **spec**：规格，细则。abbr of specification
  
   **specific** ：特性。👀 虽然但是，这个不应该遗忘
   
- **tackle**：解决

- **mutation**：变异

- **Imperative programming**：命令式编程

- **arithmetic**：算术

- **in lieu of**：替代

- **omit**：删除、省略

- **summation**：总和。sum 的完整形式？

- **interchangable**：可交换的

- **precedent**：先例

- **walkthrough**：演练

- **casting function**：转型函数。👀 关于翻译，在 [[JavaScript备忘录#包装类#JS 中的“原始值包装类型” ( Primitive wrapper types )]] 中有过说明

- **DRY**：即：Don't Repeat Yourself 。不重复（原则）

- **verbose**：冗长的、啰嗦的

- **redundant**：冗余的

- **shorthand**：速记

- **practice**：实践。比如 Best practice ，感觉可以理解为“做法”，不过字典上没“做法”这种意思

- **unite**：使联合

- **unify**：统一

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

- **setup**：设置 ( noun ) 。**set up**：设置 ( verb )

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

- **[prefix]-prone** ：有 ... 倾向的。

  > Uno makes heavy use of regex for dynamic utilities, which feels error-<font color=LightSeaGreen>prone</font>. 
  >
  > 摘自：[TailwindCSS vs. UnoCSS](https://dev.to/mapleleaf/tailwindcss-vs-unocss-2a53)

- **profile**：剖析 ( verb )。来自 “ It is especially useful in the case of early prototyping and **profiling**.  ”

- **scaffold**：脚手架

- **negate**：否定。**negated**：否定的。参考记忆，可以根据 negative

- **as of**：截止，从 ... 开始

- **histogram**：直方图

- **omnibox**：地址栏。

  > 👀 百度翻译的结果是 “地址栏”，但 苹果和谷歌的翻译 都给出的都不是 “地址栏”，原因参见 [[Web相关#关于 Omnibox]]

- **ultimate**：一般的含义是 “最终的，最后的”。不过这里要注意的是，还有 “基本的” 的意思。来自：“The `Compiler` is **ultimately** a function which performs bare minimum functionality to keep a lifecycle running.”

- **kickstart** ：启动

- **consumable** ：易于使用的

- **dispose **：处置

- **reversible** ：可逆转的。**irreversible**：不可逆转的

- **idle**：空闲的，闲置的

- **invocate**：调用 ( verb) -> **invocation** ( noun )

- **conjunct**：结合 ( verb ) -> **conjunction** ( noun )

- **trivial**：不重要的

   **not trivial** ：并非微不足道

- **deterministic**：确定性的

- **pseudo**：伪的

- **resort**：求助

- **process**：一般理解为 “过程” ( noun )，这里强调的是“**处理**“ ( verb ) 的意思

- **proceed**：继续

- **pave**：铺路

- **invoke**：援引，提及，**调用**

- **enforce**：**执行**，强迫实施

- **opt out**：选择退出，选择排除

   **opt in** ：选择加入

- **subsequent**：随后的，后面的

- **interfere**：干涉

- **opaque**：不透明的

- **praxis**：实践

- **supersede**：取代

- **arbitrary**：**任意的**，武断的

- **assign**：**分配**，指派

- **introspect**：内省，**自我检查**

- **seal**：封闭。含义可以参考 `Object.seal()` 方法

- **patch**：修补，修改（比如 HTTP 的 `PATCH` 方法，用于对资源进行部分修改 ）

- **tie**：连接，联系

- **interpolate**：插话，**插值**。

- **constraint**：n. 约束，限制。常见的有 SQL 中的 `constraint` 关键字

  **constrain** ：vt. 强迫，限制

- **revive**：使复苏，使重新使用

- **respect**：**遵守**。了解自 “The callback must **respect** the type of the tap method used.”

- **as with**：如同，和 ... 一样

- **spread**：展开。比如 [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) 展开语法

- **unray**：一元的（一元运算）

- **ternary** ：三元的。Ternary expression 三元表达式

- **general purpose**：通用

- **factorize**：分解

- **correspond**：相一致，相当于，通信。**corresponding**：**相应的**，对应的，符合的

- **function**：**运转**，运行 ( verb )

- **story**：**需求**。了解自 “The moral of the **story** is that there are a variety of ways to `hook` into the `compiler`, each one allowing your plugin to run as it sees fit.” 。扩展阅读：[敏捷模式中“故事”和“需求”的关系是什么？ - 知乎](https://www.zhihu.com/question/26996772)

- **mangle**：碾碎，撕裂。引申 -> mangler：碾碎器？（代码压缩相关）

- **misc**：杂项

- **predicate**：谓词，比如 TS 中的 “type predicates”（ 类型谓词 ）

- **terminology** ：术语

   **term** ：vt. 把…称为，把…叫做

- **coin**：创造（新词语） ( verb )

- **interchangeable**：可交换的

- **variation**：变化 ( noun ) 。**variate**：变量，变数 ( noun )。**vary**：变化( verb )

- **longhand**：手写

- **shorthand**：速记

- **brevity**：简洁 ( noun )

- **on demand**：按需。👀 这个词 看英文能懂，但是中文想英文完全想不起来...

- **extent** ：程度

- **hierarchy** ：**层级**，等级制度

- **encapsulate** ：**封装**，概括

- **paradigm** ：范式

- **acquaint** ：熟悉的

- **parallelism** ：相似 ( noun )

- **Mutexes** ：互斥量

- **synchronization** ：**同步**，校准

- **notify** ：通知 ( verb) ，notification 的动词形式

- **notation** ：符号

- **notably** ：adv. 尤其，非常。

  **notable** ：adj. 值得注意的，显著的，重要的。n. 名人，重要人物

- **afterthought** ：回想 👀 虽然但是，不该猜不出意思

- **foreground** ：前景。在 [微信官方文档 - 小程序 - 框架接口 / 页面 / 页面生命周期](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page-life-cycle.html) 中看到的 “set to foreground ”，应该有 前一个页面的意思。另外，它的反义词是 background

- **foregone** ：预先确定的

- **mitigate** ：缓解

- **squash** ：压碎，压缩。这个意思一般和 `git rebase` 和 `git merge` 相关

- **in a nutshell** ：简而言之

- **recipes** ：食谱

- **characterize** ：描述、表征 ( verb )

- **peculiar** ：特有的、奇怪的

- **intricacy** ：错综复杂（的事物）

- **react** ：反应 ( verb ) 👀 这个不应该忘记的...

- **deviate** ：偏离。一般使用：deviate from

- **canonical** ：权威性的。比较常见的用在 `<link rel="canonical">` 和 CNAME record ( Canonical Name Record )

- **typo** ：打字错误

  **typographic** ：排版的

- **eavesdrop** ：窃听。eaves 屋檐。eavesdropper 窃听者。

  > 💡 在密码学领域，除了 Alice & Bob 这两个虚拟人物，还有 Eve，<font color=red>E</font>ve 就是 <font color=red>E</font>avesdropper
  >
  > 学习自：[为什么计算机科学如密码学喜欢用 Alice 和 Bob 举栗子？ - 刘巍然-学酥的回答 - 知乎](https://www.zhihu.com/question/63306763/answer/255496822) ，另外文章中还提及了人物 Mallory ，指代 Malicious Adversary “恶意攻击者”

- **throughput** ：吞吐量

- **apropos** ：关于。除了这个意思，这也是一个常用的 Linux 命令，用于根据描述搜索 Linux 帮助文档来找到想要的命令。

- **doodle** ：涂鸦

- **formula** ：公式

- **from scratch** ：从头做起，从零开始。

  **scratch** ：划痕

- **territory** ：领域。

- **resort** ：采取

- **explicit** ：明确的。cpp 关键字

- **operand** ：操作数

- **caveat** ：n. 警告，注意事项

- **concise** ：简明的

- **stay tuned (for)** ：敬请关注

- **instrument** ：一般的意思是乐器、仪器。不过，在 Vue3 源码中看到了 ArrayInstrumentations 数组检测器的东西；instrument 在计算机中还有 “检测” 的含义，尤其在 macOS 中有一个 instruments 的性能分析和可视化 应用。

- **baffled** ：感到困惑的

- **perspective** ：一般的意思是：态度。但也有 “角度”的意思，无论是 from someone's perspective ，还是 CSS 中的 perspective 属性。

- **coverage** ：覆盖率

- **revoke** ：撤销。

- **facilitate** ：促进

- **resilient** ：有弹性的

- **complement** ：v. 补足，补充； n. 补充物，补语。 

- **permalink** ：Permanent Link 的缩写。固定链接，永久链接

- **momentum** ：推进力

- **yield** ：出产，让步。这里比较重要的意思是 “让步”。毕竟 js / py 有 `yield` 关键字，本意就是“让步”。

  > 协程可以通过 `yield`（取其“让步”之义而非“出产”）来调用其它协程，接下来的每次协程被调用时，从协程上次 `yield` 返回的位置接着执行，通过 `yield` 方式转移执行权的协程之间不是调用者与被调用者的关系，而是彼此对称、平等的。
  >
  > 摘自：[wikipedia - 协程](https://zh.wikipedia.org/wiki/%E5%8D%8F%E7%A8%8B)
  >
  > 另外，在 [函数式编程的核心价值是什么？ - 张宏波的回答 - 知乎](https://www.zhihu.com/question/471098472/answer/2029186480) 的评论区看到了这样一个评论，感觉受到了不小的启发。这里做下补充：
  >
  > <img src="https://s2.loli.net/2023/05/12/hpL7nJcyGUgKt38.png" alt="image-20230512163155036" style="zoom:65%;" />

- **intermediary** ：中间的，中介的

- **bespoke** ：定制的

- **minimal**：最小的。缩写 min 很常见，全称有点难认。另外，max 的全称是 maximum

- **quo** ：现状。来自：

  > After years of relative stability, many are now beginning to question the status **quo**.
  >
  > 摘自：[stateofjs 2022](https://2022.stateofjs.com/en-US/)

- **respondents** ：受访者

- **proportion** ：比例

- **come in handy** ：派上用场

- **calibrate** ：校准

- **in time** ：及时。 **on time** ：准时

- **triage** ：分类诊断

- **equity** ：公平

- **malware** ：恶意软件。Malicious Software 的合成词

- **transpile** ：转译

- **catalog** ：目录

- **vendor** ：

  > It's a <font color=dodgerBlue>common convention</font> to <font color=red>put files coming from various third party sources (the "vendors") in a folder named that way</font>.
  >
  > You can use it as it makes it clearer what's "from the project" and what is a dependency you rely upon, but it is merely a convention, not an obligation.
  >
  > ***
  >
  > `/vendor` usually refers to a directory that contains third party plugins.
  >
  > 摘自：[stackoverflow - What does vendor mean in web file structure?](https://stackoverflow.com/questions/16865980/what-does-vendor-mean-in-web-file-structure)

- **layout** ：布局。这个词一般和 关键渲染路径 相关

- **forgery** ：伪造

- **nitpick** ：吹毛求疵。在阅读 Prettier 文档时，多次出现了单词 “nit”，比如：

  > People get very emotional around particular ways of writing code and nobody likes spending time writing and receiving <font color=lightSeaGreen>nits</font>.
  >
  > Our top reason was to stop wasting our time debating style <font color=LightSeaGreen>nits</font>.
  >
  > 摘自：[Prettier Doc - Why Prettier?](https://prettier.io/docs/en/why-prettier.html)

  查阅字典也并没有相关的意思，直到在文档更后面处看到 “nitpick”，所以 “nit” 应该可以被译为“细节”

- **trivial** ：不重要的

- **proportional** ：成比例的。disproportional 不成比例的

- **ramp up** ：加快

- **alleviate** ：减轻

- **rationale** ：根本原因

- **dominant** ：占主导地位的，显性的。显性性状 ( n )

- **heuristic** ：启发式

- **pragmatic** ：实用的

- **detach** ：使...分离。这个词是在 [Vue3 rfc - reactivity-effect-scope](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0041-reactivity-effect-scope.md) 中看见的：

  > It also provides the functionality to create "detached" effects from the component's `setup()` scope or user-defined scope.

  另外，docker 中也有 detached mode。

- **pitfall** ：陷阱

- **outline** ：轮廓。outline 是一种 CSS 属性

- **simulate** ：模拟。simulator 则是模拟器

- **Palette** ：调色板

- **criteria** ：标准

- **funnel** ：漏斗

- **hassle** ：困难

- **signify** ：表示

- **troubleshooting** ：排错，分析解决问题

- **trait** ：特征，特点

- **segregation** ：隔离。read / write segregation 读写隔离

- **precedence** ：优先地位，优先级

- **squiggles** ：波浪线，波形

- **succinct** ：简明的

- **curved** ：弯曲的，curved line 曲线

- **latency** ：延迟

- **comply** ：遵守

- **snapshot** ：快照

- **cover** ：报道、**介绍**、**讲述**。cover 的这种含义是在 [ts handbook - generic # Generic Classes](https://www.typescriptlang.org/docs/handbook/2/generics.html#generic-classes) 中看到的：

  > As we cover in [our section on classes](https://www.typescriptlang.org/docs/handbook/2/classes.html), a class has two sides to its type: the static side and the instance side. 

- **sort** ：种类，类别，分类。在编程 sort 意为“排序”，但是他还有其他意思。

  > TypeScript intentionally limits the sorts of expressions you can use `typeof` on.

- **introspected** ：内省的

- **retrieve** ：检索

- **probe** ：探测

- **plug** ：插头

- **concrete** ：具体的

- **concatenate** ：串联，连接。可以认为 concat 是 concatenate 的缩写

- **interpolated** ：插值的

- **eligible** ：有资格的

- **ergonomic** ：（符合）人体工程学的

  > A more arcane, but very ***ergonomic*** way to set a compiler setting is via compiler flag which are comments starting with `// @`.
  > 摘自：[TS play handbook - Twoslash Annotations](https://www.typescriptlang.org/play?#handbook-14)
  
- **clause** ：条款

- **hypothesis** ：假设

- **disclosure** ：n. 披露

  **disclose** ：v. 披露，泄露

- **obsolete** ：淘汰的

- **hop** ：跳

  > They'll hop onto SquareSpace, find a template they like, and spend $20/month.
  >
  > 摘自：[The End of Front-End Development](https://www.joshwcomeau.com/blog/the-end-of-frontend-development/)
  >
  > 💡 关于 hop：
  >
  > <img src="https://s2.loli.net/2023/04/03/FMxgbdEVJ3OuBNK.png" alt="image-20230403213515134" style="zoom:47%;" />

  **leap** ：跳跃
  
- **tweak** ：微调

- **hallucination** ：幻觉

- **vulnerability** ：漏洞

- **liable** ：有责任的

- **self-contained** ：自给自足的

- **amplify** ：v. 增加，增强。
  > 👀 chatgpt 说：和 ample （充足的，足够的）有关系，自己感觉也是有关系的；不过语意上没太看出来...
  
- **speculate** ：v. 推测。speculation ：n.

- **fait accompli** ：既成事实

- **illustration** ：插画。illustrate ：给 ... 加插图，阐述，说明。

- **globble** ：狼吞虎咽，吞咽

- **bespoke** ：定制的

- **synthesize** ：合成

- **suspicious** ：怀疑的。suspect ：嫌疑人

- **authoritative** ：权威的

- **imply** ：暗示

- **assume** ：假设，假定。类似的有 presume。

  <img src="https://s2.loli.net/2023/04/05/iET3t1bdynUZNDO.png" alt="image-20230405171953947" style="zoom:50%;" />


- **resume** ：v. 重新开始，继续。 n. 简历

- **mimic** ：v. / n. 模仿，adj. 会模仿的（人/动物）

- **gist** ：要点，主旨。这个单词挺常见的，比如 `gist.github.com`

- **essential** ：n. 必需品，要点；adj. 本质的，必不可少的

- **instruction** ：指令。👀 虽然但是，这个太基础了，不应该忘记的...

- **relevant** ：adj. 相关的

- **footnote** ：脚注


- **downside** ：缺点

- **downright** ：完全的，彻底的

- **comprise** ：由...构成，组成

- **occasional** ：偶尔的。👀 虽然但是，这有点过于基础了

- **invoked** ：被调用

- **extent** ：程度

- **consequence** ：后果。As a consequence ：因此 。👀 挺基础的，太久没接触，遗忘了

- **derive** ：起源于...

- **ambient** ：周围的

- **kinetic** ：运动的

- **censor** ：审查

- **spawn** ：生成，产卵，引起、导致。
  > 💡 spawn 是 Linux 的一个系统调用，也是 Node 中 child_process 模块的方法。理所当然的： [zx](https://github.com/google/zx) 中也有使用。
  >
  > <img src="https://s2.loli.net/2023/05/04/AE6BML3iJSTtoCc.png" style="zoom:47%;" />

- **daemon** ：守护进程，后台进程。

  > 💡 ChatGPT 的解释：
  >
  > <img src="https://s2.loli.net/2023/05/04/VpqUxysj7McCZrX.png" style="zoom: 47%;" />

- **cryptic** ：神秘的，含义隐晦的，**晦涩难懂的**。👀 这里强调的是 “晦涩难懂的”

  > ```ts
  > interface Array<T> {
  >     concat(...items: Array<T[] | T>): T[];
  >     reduce<U>(
  >       callback: (state: U, element: T, index: number, array: T[]) => U,
  >       firstState?: U
  >     ): U;
  >     // ···
  > }
  > ```
  >
  > You may think that this is **cryptic**.
  >
  > 摘自：Tackling TypeScript - “The essentials of TypeScript” - What you’ll learn

- **comprehensive** ：综合的，全面的。 👀 这个有点基础了，不该遗忘

  **comprehension** ：理解，理解力。另外，Python 中 list comprehension 译作 “列表推导式”

- **vault** ：保险箱，金库

- **lone** ：单独的，独自的，孤零零的。

  > 👀 见到这个单词是在读 [MDN - String.prototype.isWellFormed()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/isWellFormed) 中提及的 “lone surrogate” 。之前似乎从没有见过这个单词，查了下意思，想到了 alone 和 lonely；但 lone 确实没有半点印象，有点神奇...

- **counterpart** ：对应的人或物

- **ad-hoc** ：临时的，特别的

- **terminate** ：v. 终止

- **mutual** ：相互的 👀 挺基础的，不该忘记

- **factor** ：n. 因素，因子，因数，要素；（增或减的）数量，倍数；系数;

- **partition** ：n. 隔断、分割、<font color=red>**分治**</font> 。vt. 分割

- **contradict** ：v. 反驳、驳斥、批驳；相抵触、<font color=red>相矛盾</font>、相反。

  **contradiction** ：n. 矛盾

- **disguise** ：vt. 假扮、伪装 n. 伪装物，假扮、装扮、伪装

- **replicate** ：复制

- **flatter** ：奉承，使显得更漂亮（ 👀 感觉可以等价为 “优化” ？，如下标题 “ Flatter `deno.json` configuration “，显然翻译为 “优化” 更好些）

- **designate** ：vt. <font color=red>**命名**</font>，指定、选定，**指派**，**委任**（某人任某职）；标明、标示、指明

- **primes** ：质数，类似的也可以用 prime number

- **unpack** ：打开...取出东西，（网络 / 音视频领域？）**解包**

- **per se** ：本身

- **methodology** ：方法论

- **moot** ：无意义的，无关紧要的

- **sentinel** ：守卫

  > But the docs, plus the syntax required to use it, discourages them from being used. I like the clear, enforced <font color=LightSeaGreen>sentinel</font> of “this does not exist in the design system”.
  >
  > 摘自：[TailwindCSS vs. UnoCSS](https://dev.to/mapleleaf/tailwindcss-vs-unocss-2a53)

  > 💡 另外的一点吹毛求疵
  >
  > <img src="https://s2.loli.net/2023/05/09/GEYMZO4SdNXqrHs.png" style="zoom:45%;" />
  >
  > <img src="https://s2.loli.net/2023/05/09/Lj9QxHhmwbyGeV2.png" style="zoom:45%;" />

- **decompose** ：v. **分解**，腐烂

- **jargon** ：（某个特定领域或职业中使用的）术语

  > jargon 和 term 都有 “术语” 的意思，不过还是有所区别：
  >
  > <img src="https://s2.loli.net/2023/05/10/P6M3gnuNUhRmoSE.png" style="zoom:45%;" />

- **idempotency** ：幂等性

- **generalize** ：v. 概括，归纳

  **generalization** ：n. 泛化

- **flaw** ：缺陷

- **cascade** ：级联

- **impure** ：不纯的

- **synthetic** ：合成的

- **violate** ：vt. 违反，违规

  **violation** ：n. 违反

- **implicit** ：隐含的，含蓄的

- **consensus** ：共识

- **sequential** ：连续的，顺序的

  **sequent** ：adj. 继续的，连续的。n. 后果，接着发生的事，结果

- **strip** ：n. 带、条 v. **剥去**，除去

  > Things like `userState` and `useEffect` get stripped out.
  >
  > 摘自：[What Even Are React Server Components](https://www.viget.com/articles/what-even-are-react-server-components/)

- **portable** ：<font color=red>可移植的</font>，便携的

- **carousel** ：旋转木马。**image carousel** ：图片轮播

- **standby** ：待机

- **mitigate** ：缓和，缓解，减轻

- **policy** ：**策略**，政策。

  > Policies in place to aid background page performance
  >
  > 摘自：[MDN US - Page Visibility API](https://developer.mozilla.org/en-US/docs/Web/API/Page_Visibility_API) 
  >
  > 另外，类似的还有 Content Security Policy

- **throttle** ：vt. 掐死;使窒息;勒死 n. **节流阀**。另外，因为‘节流阀“的含义，引申出 ”节流“ 的含义

  > <img src="https://s2.loli.net/2023/05/21/capsQL24kFU5vT6.png" alt="image-20230521172230218" style="zoom:50%;" />

- **incoming** ：传入的

  > It’s a continuous loop executing incoming work. 
  >
  > 摘自：[Practical Guide To Not Blocking The Event Loop](https://www.bbss.dev/posts/eventloop/)

- **indefinite** ：adj. **无限期的**，模糊不清的，不明确的

  > In server contexts, one such request can block all others indefinitely.
  >
  > 摘自：[Practical Guide To Not Blocking The Event Loop](https://www.bbss.dev/posts/eventloop/)

- **monolithic** ：庞大而单一的，单体化的。monorepo 即 Monolithic Repository

- **geometry** ：几何学

- **spatial** ：adj. 空间的

  **spatially** ：adv. 空间上

- **viscosity** ：粘度

- **gamut** ：全范围

- **error-tolerant** ：容错的

- **coercion** ：强制，胁迫

  **type coercion** ：强制类型转换
  
- **tamper** ：篡改

- **optical** ：视觉的

- **congest** ：拥挤。 TCP 中 cwnd 即 congestion window

- **intrinsic** ：内在的，固有的，本身的

  **intrinsics** ：内部函数。

  > 💡 另外，Google 搜索 “intrinsics in js”，可以搜到如下内容：
  >
  > > **“Intrinsic”** is the way some authors refer to what other authors call **“built-in”**.
  > >
  > > Those data types/objects/classes are always there regardless of what environment you’re running in.
  > >
  > > JavaScript provides intrinsic (or “built-in”) objects. They are the Array, Boolean, Date, Error, Function, Global, JSON, Math, Number, Object, RegExp, and String objects.
  > >
  > > 摘自：[Develop a webpage using Intrinsic Java Functions](https://rahultamkhane.medium.com/develop-a-webpage-using-intrinsic-java-functions-36cb3b84826c)
  
- **MVP** ："Minimum Viable Product"，即 “最小可行产品”

  > This product is in an early stage, approximately 80% to MVP as of July 2023.
  >
  > 摘自：[GitHub - PythonMonkey](https://github.com/Distributive-Network/PythonMonkey/)
  
- **preemptive** ：先发制人的，抢先的

  >  Server Push is a performance technique aimed at reducing latency by loading resources **preemptively**, even before the client knows they will be needed.
  >
  > 摘自：[wikipedia - HTTP/2 Server Push](https://en.wikipedia.org/wiki/HTTP/2_Server_Push)
  
- **arity** ：表示一个函数或操作的参数数量，也称为参数个数或元数

  > 👀 这个词是在 lodash 官方文档的 [`_.curry`](https://lodash.com/docs/4.17.15#curry) 部分看到（如下），当然它是一个编程的通用概念
  >
  > ```js
  > _.curry(func, [arity=func.length])
  > ```

  如下是询问 ChatGPT 的结果：

  <img src="https://s2.loli.net/2023/08/26/e16lFatTNHkiOpm.png" alt="image-20230826145857953" style="zoom: 48%;" />
  
- **caret** ：插入符号

  > A **caret** (sometimes called a "text cursor") is an indicator displayed on the screen to indicate where text input will be inserted.
  >
  > 摘自：[MDN - Caret](https://developer.mozilla.org/en-US/docs/Glossary/Caret)
  
- **duplicated** ：重复的

  **dedupe** ：去重。即 de-dupe，而 “dupe” 正是来源于 “duplicated”
  
  > 👀 这个词第一次看见是来自 vite 的一个配置 [`reslove.dedupe`](https://cn.vitejs.dev/config/shared-options.html#resolve-dedupe) 
  
- **repetitive** ：重复的

- **cumulative** ：累计的

  > 👀 见到这个词是来自 “Cumulative Layout Shift” 即 CLS 。另外，[`Array.prototype.reduce`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) 的第一个参数是 `accumulator` 也与之类似
  
- **in series** ：串联

  >  If your tasks do not use any timers or perform any I/O, they will actually be executed **in series**
  >
  > 摘自：[async doc - parallel](http://caolan.github.io/async/v3/docs.html#parallel)
  
- **recap** ：回顾，扼要重述

- **top down** ：自上而下

  **bottom up** ：自下而上

  > You can either build “top down” by starting with building the components higher up in the hierarchy (like `FilterableProductTable`) or “bottom up” by working from components lower down (like `ProductRow`)
  >
  > 摘自：[React doc - Thinking in React # Step 2: Build a static version in React](https://react.dev/learn/thinking-in-react#step-2-build-a-static-version-in-react)
  
- **forward** ：转发

  > Some components forward all of their props to their children, like how this `Profile` does with `Avatar` .
  >
  > 摘自：[React doc - Passing Props to a Component # Forwarding props with the JSX spread syntax](https://react.dev/learn/passing-props-to-a-component#forwarding-props-with-the-jsx-spread-syntax)

- **restraint** ：约束，限制，克制

- **spinner** ：本意：旋转器。前端引申意：加载动画

  > When you press “Submit”, both the form and the button **become disabled,** and a spinner **appears.**
  >
  > 摘自：[React doc - Reacting to Input with State # How declarative UI compares to imperative](https://react.dev/learn/reacting-to-input-with-state#how-declarative-ui-compares-to-imperative)
  
- **exponential** ：指数的

- **name after** ：以...为名

- **Escape Hatch** ：逃生舱 / 应急方案

  > 👀 开始以为这个词没有那么特别，不过，还是问了下 Claude，得到如下回复：
  >
  > <img src="https://s2.loli.net/2023/10/07/oat8pKvWdgJYUiP.png" alt="image-20231007211051637" style="zoom:48%;" />
  
- **delimiter** ：分隔符

- **coordinator** ：协调员。

  > 👀 显然这个词和 “coordinate” 坐标/协调 相关

- **genesis** ：开端，初始

- **supercharge** ：增强，提升

  > Discover our [list of modules](https://nuxt.com/modules) to supercharge your Nuxt project, created by the Nuxt team and community.
  >
  > 摘自：[Nuxt Github repo - readme # Modules](https://github.com/nuxt/nuxt#modules)
***



### 英文术语

- **埋点**：Event Tracking




### 特殊字符

因为 Google 是不支持 特殊字符 搜索的，想要带上 特殊字符 进行搜索，就必须要加上 字符的英文；所以这些就非常有必要记录

##### 括号 Bracket

- **() / 圆括号**：Round brackets / parentheses
- **\[] / 方括号**：Square brackets / brackets
- **{} / 花括号 / 大括号** ：Curly brackets / braces
- **<> / 尖括号**：Angle brackets / chevrons

参考自：[wikipedia - Bracket](https://en.wikipedia.org/wiki/Bracket)

- **星号 `*`**：asterisk。在非正式的交流中也会被称为 “star” （比如 [stack overflow - Meaning of a double star (**) in a file path](https://stackoverflow.com/questions/46547540/meaning-of-a-double-star-in-a-file-path) ），不过，在 Google 搜索中 还是要使用 asterisk，star 没有用
- **连接号**：dash，是一种统称。详见：[wikipedia - 连接号](https://zh.wikipedia.org/wiki/%E8%BF%9E%E6%8E%A5%E5%8F%B7)
  - en dash：表示范围
  - em dash：表示预期转折
- **连字符 `-`**：hyphen。用于连接单词，将不同单词连接变成一个单词。
- **分号 `;`** ：semicolon
- **斜杠**
  - 正斜杠  `/`：forward slash 👀 其中 w/ 的意思是 with
  - 反斜杠  `\ ` ：backslash 
- **竖杠 `|`** ：一般称为 “Vertical bar”，但是在 CS 中更多称为 “pipe symbol”
- **引号**
  - 反引号 ``	`：backquote
  - 单引号 `'` ：single quote
  - 双引号 `"` ：double quote
- **下划线 `_`** ：underline，CS 中更倾向使用 **underscore**
- **逗号 `,`** ：comma
- **问号 `?`** ：question mark
- **冒号 `:`** ：colon
- **点号 `.`** ：dot
- **间隔号 `·`** ：interpunct
- **波浪号 `~`** ：tilde

##### Google 特殊字符搜索方法

| Symbol                  | How it is helpful                                            |
| :---------------------- | :----------------------------------------------------------- |
| Plus sign (+)           | Search for things like blood type [ AB+ ] or for a Google+ page like [ +Chrome ] |
| "At" sign (@)           | Find social tags like [ @google ] or [ @ladygaga]            |
| Ampersand (&)           | Find strongly connected ideas and phrases like [ Brothers & Sisters ] or [ A&E ] |
| Percent (%)             | Search for a percent value like [ 40% of 80 ] or [ 10% of .1 ] |
| Dollar sign (`$`)       | Indicate prices, so [ nikon 400 ] and [ nikon `$400` ] give different results |
| Hashtag/number sign (#) | Search for trending topics indicated by hashtags like [ \#lifewithoutgoogle ] |
| Dash (-)                | Indicate that words around it are strongly connected as in [ twelve-year-old dog ] and [ cross-reference ] |
| Underscore symbol (_)   | Connected two works like [ quick_sort ]. Your search results will find this pair of words either linked together (e.g., quicksort) or connected by an underscore (e.g., quick_sort). |

> 👀 关于 `#` 的 number sign 的含义，一直没注意，直到见到这个句子：
>
> > I actually think that this could increase the total # of developer jobs.
> >
> > 摘自：[The End of Front-End Development](https://www.joshwcomeau.com/blog/the-end-of-frontend-development/)

摘自：[Google搜索特殊字符的方法](https://blog.csdn.net/LongZh_CN/article/details/14453317)



#### 前后缀

##### 后缀

- `-ish` 后缀：用于表达数量或时间的近似值。以 "10ish" 为例， 它是一个口语化的用法，表示数量大约为10。 

  > This is an over-generalization, but over the past 10ish years, a lot of complexity has been moving from the server to the client.
  >
  > 摘自：[The End of Front-End Development](https://www.joshwcomeau.com/blog/the-end-of-frontend-development/)
  
- `co-` 前缀：协同的、共同的...

  > Remote plugins run as **co-processes**, safely and asynchronously.
  >
  > 摘自：[neovim 官网](https://neovim.io/)
  
- `inter` 前缀：相互的

  > interop ：互操作



### 单词缩写

> 💡 可以参考 [[linux与macOS备忘录#Linux 命令缩写由来|Linux 命令缩写由来]] 中的内容

- **pt. n** ：第 n 部分
- **Misc** ：杂项，其他。"Misc" 是 "miscellaneous" 的缩写
- **spec** ：规格 specification
- **ver** ：版本 version
- **promo** ：推广/ 宣传片。 promotion 



### 常见单词

- **knot** ：n.（用绳索等打的）结。v. 把…打成结（或扎牢）

  **unknot** ： 解开...的结

- **trail** ：n. **审判**，试验，试用；比赛;  v. 试验，试用。

- **top-notch** ：一流的

- **motivate** ：v. 激励，成为…的动机，是…的原因

  **motivated** ：adj. 有积极性的，充满热情的，**有动机的**，**有目的的**

- **incur** ：v. 招致

- **preceive** ：v. 感知

- **sentiment** ：情感

- **overtime** ：加班

- **commence** ：开始

- **departure** ：离开

- **messy** ：凌乱的

  **mess** ：混乱

- **obligated** ：有义务的

- **conviction** ：信念

  **convict** ：n. 罪犯;已决犯;服刑囚犯 v. 定罪;宣判…有罪

  <img src="https://s2.loli.net/2023/05/07/Fi1pkIUHhQZjPYc.png" alt="image-20230507173533552" style="zoom:45%;" />

- **cohesion** ：凝聚力

  **cohere** ：vi. 凝聚，连贯、一致；团结一致

- **surgery** ：手术

- **drift** ：v. 漂流 n. 偏航，逐渐变化（尤指向坏的方面）

- **subsidies** ：补助金

- **congress** ：国会，代表大会

- **orphan** ：孤儿

- **transient** ：adj. 转瞬即逝的、短暂的；n. 暂住某地的人、过往旅客、临时工

- **eventual** ：最后的

  **eventuality** ：可能发生的事

- **diligent** ：勤奋的

- **gravitate** ：被吸引到 ( 👀 感觉从 gravity ""地心引力"" 引申为 gravitate “被吸引到” 也是符合逻辑的 )

- **guesstimate** ：大概估计

- **portrait** ：肖像

- **frustrate** ：v. 阻挠、挫败、使沮丧

- **friction** ：摩擦 👀 有点基础了

- **tedious** ：单调乏味的

- **longevity** ：长寿

- **necessitate** ：vt. 使成为必要

- **embark** ：v. 上船;装船;开始

- **nod** ：点头

- **dominance** ：支配地位，支配、控制

- **ongoing** ：adj. 持续的。n. 进行，前进

- **workload** ：工作量

- **semester** ：学期

- **fidelity** ：n. 忠诚

- **hype** ：n. 炒作。来自 hyperbole，但是似乎不是缩写？

  <img src="https://s2.loli.net/2023/05/15/cKtwg3T6ZHynzi5.png" alt="image-20230515140741048" style="zoom:45%;" />

- **woe** ：麻烦，悲哀

- **dread** ：v. 恐惧 n. 恐惧

  **dreaded** ：令人害怕的，可怕的
  
- **budget** ：预算

- **exempt** ：豁免

- **scape** ：景观。Netscape 也就是网景

- **subtle** ：细微的，不易察觉的 ( subtle bug )

- **coupon** ：优惠券

- **acquisition** ：获得 。acquire 的名词

- **informed** ：明智的 👀 这个挺简单的，不该忘记的...

  > Developers and Open Source authors now have a massive amount of services offering free tiers, but it can be hard to find them all to make informed decisions.
  >
  > 摘自：[GitHub - free-for-dev - readme](https://github.com/ripienaar/free-for-dev)

- **reserve** ：v. 保留，留存，储备；n. 保留

  > 👀 遇到的地方见下面 “over time”

- **over time** ：随时间推移

  > State is reserved only for interactivity, that is, data that changes over time. Since this is a static version of the app, you don’t need it.
  >
  > 摘自：[React doc - Thinking in React # Step 2: Build a static version in React](https://react.dev/learn/thinking-in-react#step-2-build-a-static-version-in-react)
  
- **conform** ：遵从，符合，符合

- **suffice** ：足够，足以

- **converse** ：adj. 相反的，逆的 vi. 谈话

- **legible** ：清晰的，清澈的

- **resort** ：求助

- **minutiae** ：细枝末节，细节

  > They are built to take into account the code you write in your component, so you will get [inferred types](https://www.typescriptlang.org/docs/handbook/type-inference.html) a lot of the time and ideally do not need to handle the **minutiae** of providing the types.
  >
  > 摘自：[React doc - Using TypeScript # example hooks](https://react.dev/learn/typescript#example-hooks)
  
- **anatomy** ：解剖

- **premature** ：过早的

  > Don’t optimize prematurely!
  
- **substitute** ：替代

- **rest assured** ( that ) ：请放心

- **paraphrase** ：释义

- **as a rule of thumb** ：根据经验

- **discern** ：鉴别

- **consolidate** ：v. （使）结成一体，合并，整合；（使）巩固；使加强

  > For these cases, you can consolidate all the state update logic outside your component in a single function, called a *reducer.*
  >
  > 摘自：[react doc - Extracting State Logic into a Reducer](https://react.dev/learn/extracting-state-logic-into-a-reducer)

- **upfront** ：预先，一开始（看了下字典没有 “一开始” 的意思，这个属于引申意）

  > Generally, with `useState` you have to write less code upfront.
  >
  > 摘自：[react doc - Extracting State Logic into a Reducer # Comparing `useState` and `useReducer`](https://react.dev/learn/extracting-state-logic-into-a-reducer#comparing-usestate-and-usereducer)

- **bloat** ：膨胀

- **dozen** ：（一）打

- **slog** ：埋头苦干

- **declutter** ：清理

  **clutter** ：n. 杂乱 vt. 凌乱地塞满
  
- **retain** ：保持。👀 这个有些过于基础了

- **chronological** ：按时间先后顺序的

- **versatile** ：多才多艺

- **aesthetic** ：美学的
