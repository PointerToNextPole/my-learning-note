# CS 及 文档书籍 常见英文



### 通用

- **synchronous** ： 同步

   **asynchronous** ：异步

- **compatibility** ：兼容性

  **forwards compatibility** ：向后兼容性

  **backcompat** ：向后兼容

  **compat**：兼容 ( verb )

- **shortcut** ：快捷方式，近道

- **out of the box** ：开箱即用

- **built-in** ：内置

- **addon** ：插件

   > node-gyp is a cross-platform command-line tool written in Node.js for compiling native addon modules for Node.js
   >
   > 摘自：[GitHub - node-gyp - README](https://github.com/nodejs/node-gyp)

- **on the fly** ：即时，在运行时（热更新）

  > 💡 参考：[如何优雅的翻译 on the fly ？ - 知乎](https://www.zhihu.com/question/21136587)

- **on the spot** ：当场、立即

- **on-going** ：正在进行中的

- **day-to-day** ：日常

- **pros and cons** ：利弊

- **pan and zoom** ：平移和缩放

   > 👀 之所以会放到这里，是因为搜了一下 pan 在这里有“平移” 的含义，但是一般字典中没有记录；可能是相关领域内约定俗成的说法吧

- **downside** ：缺点

- **overhead**：<font color=red>**开销**</font>（和性能相关）

- **vice versa** ：反之亦然

- **preflight**：预检

   > [!TIP]
   > 一般情况下的含义：预检请求 ( preflight request )。不过，在 antfu 的 UnoCSS 相关博客 [Reimagine Atomic CSS](https://antfu.me/posts/reimagine-atomic-css#scoping) 中 [scoping](https://antfu.me/posts/reimagine-atomic-css#scoping) 部分 发现了有 “样式预检” 的含义，翻译在 [重新构想原子化 CSS - CSS 作用域](https://antfu.me/posts/reimagine-atomic-css-zh#css-%E4%BD%9C%E7%94%A8%E5%9F%9F) 中

- **semver** ：( semantic version ) 语义化版本控制规范

- **semantical** ：语义的

- **instantiate** ：实例化 ( verb )

- **nest** ：嵌套，一般用形容词 nested ，嵌套的

- **recursion** ：递归

   **recursive** ：递归的

- **i.e.** : *i.e.* is an abbreviation for the phrase ***id est***, which means **"that is"** .

- **retrieve** ：检索，获取；所谓 CRUD 的 “R” 就是 retrieve，RAG ( Retrieval-Augmented Generation ) 的 R 也是

- **decoupled** ：解耦的，decouple ( verb )。

   **coupled** ：耦合的

- **critical**：关键的。一般见：关键渲染路径 ( Critical Rendering Path ) 。虽然更常见的翻译是 “批评性的”

- **gotcha** ：在计算机编程领域中是指在系统或程序、程序设计语言中，合法有效，但是会误解意思的构造，程式容易造成错误，或是一些易于使用但其结果不如期望的构造。字面上是 got you 的简写，常用于口语，**直译为： “逮着你了”、“捉弄到你了 ”、“你中计了” 、“骗到你了”**。

- **tricky**：困难的，棘手的

- **utilize**：利用

- **prune**：剪枝。这是一个一般性概念，可以用于 机器学习，数据库 以及  树形数据结构，也是前端构建 Tree Shaking 的概念。

  >[!TIP]
  >
  >Git 有 `prune` 命令，用于清除 “不可达” 或 “孤儿 ( orphaned ) ” 的对象；详见：https://www.atlassian.com/git/tutorials/git-prune ；这里略。npm 也有 prune 命令：`npm prune [[<@scope>/]<pkg>...]` ，详见 [npm docs - npm-prune](https://docs.npmjs.com/cli/v8/commands/npm-prune)。同时 docker 也有，详见 [docker docs - Prune unused Docker objects](https://docs.docker.com/config/pruning/)

- **under the hood**：在引擎盖下（指内部实现）

- **underlying**：内在的

- **internal** ：里面的，内部的

- **wildcard**：通配符

- **spec**：规格，细则。abbr of specification

   **specific** ：特性。👀 虽然但是，这个不应该遗忘

- **tackle**：解决

- **mutation**：变异

- **imperative programming**：命令式编程

   > [!TIP]
   >
   > 另外，React 也有 `useImperativeHandle` 的 hooks

- **arithmetic** ：算术

- **in lieu of** ：替代

- **omit** ：删除、省略、遗漏

   **omission** ：omit 的名词形式

- **summation**：总和。sum 的完整形式？

- **interchangable** ：可交换的

- **precedent** ：先例

   **precedence** ：优先地位，优先级

   > take precedence over

   **preced** ：先于

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

   **hierarchical** ：分登记的

- **Let's say**：比如说... ，一般用于开头开始话题

- **incur**：招致，发生

- **prioritize ( ... over ... )**：优先考虑

- **setup**：设置 ( noun ) 。**set up**：设置 ( verb )

- **granular**：颗粒状的

   **more granular** ：更细粒度的

   **granul** ：颗粒
   
   **granularity** ：粒度

- **fine-grained**：细粒度的

- **scenario**：**场景**。更普遍的意思是：脚本，假想

   > The `publicPath` configuration option can be quite useful in a variety of **scenarios** ” 。

- **neat** ：整洁的。引申为：**简单的**。来自：“ There are a few use cases in real applications where this feature becomes especially **neat** ”

- **dedicated**：**专门的**。虽然，更普遍的意思是：投入的

   > In such cases, you'll have to move the public path assignment to its own **dedicated** module and then import it on top of your entry.js ”

   **dedication** ：奉献

- **misconception**：误解 ( noun )，错误观念。

- **overlap**：重叠

- **workaround**：解决方法，变通方法

- **concurrency**：并发，并发数。

   **concurrent**：并发的

- **simultaneous**：同时（发生）的

- **compatible**：和睦相处的

- **schema**：设计，架构，概要

- **fallback**：后退

   > ⚠️ 注意和 rollback（回滚）的区别

- **recipient**：收件人（在 网络 http 场景中出现），来自：https://datatracker.ietf.org/doc/html/rfc7231#section-4.3.7

- **interface** <font size=4>*vs*</font> **port**：interface 接口，port 端口，80 端口

- **ship**：提供 ( verb ) 

   > This is an example for a package that has optimizations for production and development usage with runtime detection for `process.env` and also **ships** a CommonJs and ESM version

- **address**：关联 ( verb )

   > Each module available addresses a specific aspect of service worker development.” ，

   查阅谷歌翻译和百度翻译 都找不到相关的意思，应该是当前语境的引申义。另外，说一下快遗忘的含义：尝试解决

- **accommodate**：为 ... 提供住宿，这也是高中学习时普遍的含义。这里要强调的是，也可以翻译为：**顾及、适应、兼容**

   > Workbox aims to make using service workers as easy as possible, while allowing the flexibility to **accommodate** complex application requirements where needed.

- **house**：收容 ( verb )。

   > This repo **houses** two bundling libraries: a *modern Vite plugin* and a *legacy Rollup plugin* .
   >
   > 这里翻译成“包含”更妥帖些。

- **anonymous**：匿名的

   **anonymity** ：匿名 ( noun )

   **deanonymize** ：匿名化

- **boilerplate** ：样板

- **directive** ：指令，命令

- **stale** ：陈旧的，不过这里要说的“不新鲜的”，用于 HTTP 的 `Cache-Control` 中，比如 `max-stale` 选项

- **latency** ：延时。

   **high-latency** ： 高延时。

- **underpin** ：支撑，构成 … 的基础。

   > Because promises also **underpin** async and await , ...

- **as-is** ：照原样

- **at the cost of** ：以什么为代价。

- **fine tune** ：微调

- **[prefix]-agnostic** ：... 无关的。比如 framework-agnostic 译为 “框架无关的”。

- **[prefix]-prone** ：有 ... 倾向的。

  > Uno makes heavy use of regex for dynamic utilities, which feels error**-prone**. 
  >
  > 摘自：[TailwindCSS vs. UnoCSS](https://dev.to/mapleleaf/tailwindcss-vs-unocss-2a53)

- **profile **：剖析 ( verb )。

   > It is especially useful in the case of early prototyping and **profiling**.

- **scaffold** ：脚手架

- **negate** ：否定。**negated**：否定的。参考记忆，可以根据 negative

- **as of** ：截止，从 ... 开始

- **histogram** ：直方图

- **omnibox** ：地址栏。

  > 👀 百度翻译的结果是 “地址栏”，但 苹果和谷歌的翻译 都给出的都不是 “地址栏”，原因参见 [[Web相关#关于 Omnibox]]

- **ultimate** ：一般的含义是 “最终的，最后的”。不过这里要注意的是，还有 “基本的” 的意思。来自：“The `Compiler` is **ultimately** a function which performs bare minimum functionality to keep a lifecycle running.”

- **kickstart** ：启动

- **consumable** ：易于使用的

- **dispose **：处置

- **reversible** ：可逆转的。**irreversible**：不可逆转的

- **idle**：空闲的，闲置的

- **invocate**：调用 ( verb) 

   **invocation** ( noun )

- **conjunct**：结合 ( verb )

   **conjunction** ( noun )

- **trivial**：不重要的

   **not trivial** ：并非微不足道

- **deterministic**：确定性的

- **pseudo**：伪的

- **resort**：求助

   > In general, whenever you have to **resort** to writing Effects, keep an eye out for when you can extract a piece of functionality into a custom Hook with a more declarative and purpose-built API like `useData` above.
   >
   > 摘自：[React doc - You Might Not Need an Effect # Fetching data](https://react.dev/learn/you-might-not-need-an-effect#fetching-data)

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

- **correspond**：相一致，相当于，通信。

   **corresponding**：**相应的**，对应的，符合的

- **function**：**运转**，运行 ( verb )

- **story**：需求

   > The moral of the **story** is that there are a variety of ways to `hook` into the `compiler`, each one allowing your plugin to run as it sees fit.
   >
   > 摘自：[webpack doc - api - plugins # plugin-types](https://webpack.js.org/api/plugins/#plugin-types)

   扩展阅读：[敏捷模式中“故事”和“需求”的关系是什么？ - 知乎](https://www.zhihu.com/question/26996772)

- **mangle**：碾碎，撕裂。

   > 👀 引申 -> mangler：碾碎器？（代码压缩相关）

- **misc**：杂项

- **predicate**：谓词，比如 TS 中的 “type predicates”（ 类型谓词 ）

   > 👀 另外值得注意的是：不要和 predict 混淆了

- **terminology** ：术语

   **term** ：vt. 把…称为，把…叫做

- **coin**：verb 创造（新词语）

- **interchangeable**：可交换的

- **variation**：noun 变化

   **variate**：noun 变量，变数

   **vary**：verb 变化

   **variant** ：变体

   > **[Variants](https://windicss.org/utilities/general/variants.html)** allow you to apply some variations to your existing rules, like the `hover:` variant from Tailwind CSS.
   >
   > 摘自：[UnoCSS doc - config - Variants](https://unocss.dev/config/variants)

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

- **deviate** ：偏离。

   > 👀 一般搭配：deviate from

- **canonical** ：权威性的。比较常见的用在 `<link rel="canonical">` 和 CNAME record ( Canonical Name Record )
   **canonialization** ：规范化

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

- **territory** ：领域

- **explicit** ：明确的。cpp 关键字

- **operand** ：操作数

- **caveat** ：n. 警告，注意事项

- **concise** ：简明的

- **stay tuned (for)** ：敬请关注

- **instrument** ：一般的意思是乐器、仪器。不过，在 Vue3 源码中看到了 ArrayInstrumentations 数组检测器的东西；instrument 在计算机中还有 “检测” 的含义，尤其在 macOS 中有一个 instruments 的性能分析和可视化 应用。

- **baffled** ：感到困惑的

- **perspective** ：一般的意思是：态度。但也有 “角度”的意思，无论是 from someone's perspective ，还是 CSS 中的 perspective 属性。

- **coverage** ：覆盖率

- **revoke** ：撤销

   **revocable** ：可撤销的

   > 👀 `Proxy.revocable()`

   **provoke** ：激怒，激起、引起

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

   **portion** ：部分

   <img src="https://s2.loli.net/2023/12/17/5f9SMEh2snbzFt7.png" alt="image-20231217190626456" style="zoom:45%;" />

- **come in handy** ：派上用场

- **calibrate** ：校准

- **in time** ：及时

   **on time** ：准时

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

  另外，docker 中有 detached mode ，Git 中有 detached HEAD 分离头指针

  <img src="https://s2.loli.net/2024/06/01/mrfWn68x9a4dghI.png" alt="image-20240601150716505" style="zoom:45%;" />

- **pitfall** ：陷阱

- **outline** ：轮廓。outline 是一种 CSS 属性

- **simulate** ：模拟。simulator 则是模拟器

- **Palette** ：调色板

- **criterion** ：标准，准则

   **criteria** ：criterion 的复数

- **funnel** ：漏斗

- **hassle** ：困难

- **signify** ：表示

- **troubleshooting** ：排错，分析解决问题

- **trait** ：特征，特点

- **segregation** ：隔离。read / write segregation 读写隔离

- **squiggles** ：波浪线，波形

- **succinct** ：简明的

- **curved** ：弯曲的

   **curved line** ：曲线

   > 👀 curve 本身就有 “曲线” 的含义，比如 “Exponential curve”

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

  > A more arcane, but very ***ergonomic*** way to set a compiler setting is via compiler flag which are comments starting with `// @` .`
  > 摘自：[TS play handbook - Twoslash Annotations](https://www.typescriptlang.org/play?#handbook-14)

- **clause** ：**子句**，条款

   > You can use an `implements` clause to check that a class satisfies a particular `interface`.
   >
   > 摘自：[TS doc - Classes # `implements` Clauses](https://www.typescriptlang.org/docs/handbook/2/classes.html#implements-clauses)

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

- **self-contained** ：自包含、自给自足的

   > **Self contained:** KaTeX has no dependencies and can easily be bundled with your website resources.
   >
   > 摘自：[GitHub - KaTex - README](https://github.com/KaTeX/KaTeX)
   >
   > <img src="https://s2.loli.net/2024/03/31/MpZmh753flQEIdB.png" alt="image-20240331172014784" style="zoom:50%;" />

   **self-hosted** ：自托管的

   **self-lifting** ：自举。同样含义的还有 Bootstrapping

- **amplify** ：v. 增加，增强。
  > 👀 chatgpt 说：和 ample （充足的，足够的）有关系，自己感觉也是有关系的；不过语意上没太看出来...

- **speculate** ：v. 推测

   **speculation** ：n.

- **fait accompli** ：既成事实

- **illustration** ：插画。

   **illustrate** ：给 ... 加插图，阐述，说明。

- **globble** ：狼吞虎咽，吞咽

- **bespoke** ：定制的

- **synthesize** ：合成

- **suspicious** ：怀疑的。suspect ：嫌疑人

- **authoritative** ：权威的

- **imply** ：暗示

- **assume** ：假设，假定。类似的有 presume。

  <img src="https://s2.loli.net/2023/04/05/iET3t1bdynUZNDO.png" alt="image-20230405171953947" style="zoom:47%;" />


- **resume** ：v. 重新开始，继续。 n. 简历

- **mimic** ：v. / n. 模仿，adj. 会模仿的（人/动物）

- **gist** ：要点，主旨。这个单词挺常见的，比如 `gist.github.com`

- **essential** ：n. 必需品，要点；adj. 本质的，必不可少的

  **essence** ：本质

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

  **consequently** ：因此

- **derive** ：起源于...

- **evolve** ：演化

  > Even if it weren’t slow, as your code evolves, you will run into cases where the “chain” you wrote doesn’t fit the new requirements.
  >
  > 摘自：[React 文档 - You Might Not Need an Effect # Chains of computations](https://react.dev/learn/you-might-not-need-an-effect#chains-of-computations)

  > 👀 虽然但是，这词有点过于基础了

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

  > 👀 见到这个单词是在读 [MDN - `String.prototype.isWellFormed()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/isWellFormed) 中提及的 “lone surrogate” 。之前似乎从没有见过这个单词，查了下意思，想到了 alone 和 lonely；但 lone 确实没有半点印象，有点神奇...

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

- **flaw** ：缺陷。design flaw 设计缺陷

- **cascade** ：级联

- **impure** ：不纯的

- **synthetic** ：合成的

  > 👀 synthetic event 合成事件

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

  **port** ：v. 移植（软件）

  > We started with the intention of a JS to Rust *port*, but soon realized that in order to achieve the best possible performance, we have to prioritize writing code in a way that aligns with how Rust works.
  >
  > 摘自：[Rolldown - About Rolldown](https://rolldown.rs/about) 。另外，毕竟 Rolldown 还在早期阶段，文档变化的可能性很大，这里给出 [wayback machine 的当前( 2024/3/8 ) 的版本](https://web.archive.org/web/20240308083647/https://rolldown.rs/about)

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

  **outgoing** ：传出的

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
  
  **coerce** ：v

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
  >
  > 另外，CSS 也有 [`contain-intrinsic-size`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/contain-intrinsic-size) 属性

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

  <img src="https://s2.loli.net/2024/04/04/8tcbBKAv9ryLEYN.png" alt="image-20240404132006707" style="zoom:48%;" />

- **cumulative** ：累计的

  > 👀 见到这个词是来自 “Cumulative Layout Shift” 即 CLS 。另外，[`Array.prototype.reduce`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) 的第一个参数是 `accumulator` 也与之类似

- **in series** ：串联

  >  If your tasks do not use any timers or perform any I/O, they will actually be executed **in series**
  >
  > 摘自：[async doc - parallel](http://caolan.github.io/async/v3/docs.html#parallel)

- **recap** ：回顾，扼要重述

  > 💡 它是 recapitulate 的缩写

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

  **spin-off** ：拆分的

  **spin** ：旋转

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
  > 摘自：[Nuxt Github repo - README # Modules](https://github.com/nuxt/nuxt#modules)

- **clockwise** ：顺时针的

  **counterclockwise** ：逆时针的

  **counterclock** ：逆时针

- **polar coordinates** ：极坐标

- **metric** ：指标

- **workbench** ：工作台，在开发领域一般和代码编辑器相关

- **autowire** ：自动装配。一般见于 Spring Boot 的注解 `@autowired`

- **roam** ：漫游

- **demonstrate** ：演示（亦即 “demo” 的全称），证明

- **counterpoint** ：对位法

- **stimulate** ：促进，刺激

- **strength** ：优势

  > 👀 虽然但是，过于基础... 不过还是忘记了...

- **clickbait** ：点击诱饵

- **adhere** ：遵守

  > 👀 挺基础的，不该遗忘

- **the rule of thumb** ：经验法则

- **dangling** ：v. 悬挂，悬垂，悬荡 adj. 悬挂的，摇摆的。是 dangle 的现在分词

  > 💡 dangling pointer ：悬空指针

- **coexit** ：共存

- **hands-on** ：实践的。hands-on experience 实践经验

- **suboptimal** ：次优的

- **fidelity** ：n. 保真度、准确性、忠诚

  > Cloud environments at large scale can now be as high **fidelity** as you need them to be.
  >
  > 摘自：[Daytona 管网](https://www.daytona.io/)

- **premise** ：前提，假定

  **on-premise** ：本地部署

  > Stay locked behind firewalls and custom **on-premise** infrastructure.
  >
  > 摘自：[Daytona 管网](https://www.daytona.io/)

- **accommdate** ：容纳，为...提供住宿

- **tarball** ：压缩包

  <img src="https://s2.loli.net/2024/03/21/tcJDaEgofMYVGxI.png" alt="image-20240321233430412" style="zoom:50%;" />

- **pivot** ：支点，中心轴

  > 💡 提到这个词，一般想到 “快排” 中的变量名 `pivot`

  **pivotal point** ：中心点

- **cherry pick** ：择优挑选

  > 💡 虽说 Git 中有 `git cherry-pick` 这个命令，但是这个词并不是 Git 生造的；所以，没必要遇到这个词时，必将其与 Git 相关联；它也有本来的”择优挑选“的意思。比如如下句子：
  >
  > > You’ll see later that this allows us to **cherry-pick** only the bits we want to keep from `build` and toss the rest.
  > >
  > > 摘自：[Demystifying Docker for JavaScript](https://fly.io/javascript-journal/demystify-docker-js/)

- **tweening** ：补间动画

- **wrangle** ：操纵、处理

  > Fetch, cache, update, and **wrangle** all forms of async data in your TS/JS, React, Vue, Solid, Svelte & Angular applications all without touching any "global state".
  >
  > 摘自：[TanStack 官网](https://tanstack.com)
  >
  > <img src="https://s2.loli.net/2024/03/30/3DZQAdsoHhuLOyU.png" alt="image-20240330165133045" style="zoom: 45%;" />

  > 👀 搜了下词典软件，基本上都只说 “wrangle” 的意思是 “争吵”，并没有找到“操纵、处理”的含义。所以特此补充

- **thumbnail** ：缩略图

- **prepopulated** ：预先填充，预先加载

  > Search inputs are often prepopulated from the URL, and the user might navigate Back and Forward without touching the input.
  >
  > 摘自：[React doc - You Might Not Need an Effect # Fetching data](https://react.dev/learn/you-might-not-need-an-effect#fetching-data)

- **footprint** ：**(某物所占的)空间量，面积** ；足迹；脚印

  > HotKeys.js is an input capture library with some very special features, it is easy to pick up and use, has a reasonable **footprint** ([~6kB](https://bundlephobia.com/result?p=hotkeys-js)) (gzipped: **`2.8kB`**), and has no dependencies. It should not interfere with any JavaScript libraries or frameworks.
  >
  > 摘自：[Github - Hotkeys # README](https://github.com/jaywcjlove/hotkeys-js)

- **infix** ：中缀

- **dialect** ：方言

- **collation** ：整理；校对。<font color=red>排序规则</font>

  > DuckDB supports arbitrary and nested correlated subqueries, window functions, **collations**, complex types (arrays, structs), and more.
  >
  > 摘自：[duckdb github repo - README](https://github.com/duckdb/duckdb)

- **cat's pyjamas** ：令人赞叹的人或物

  > Requires Node 8 or above, because async and await are the **cat's pyjamas**.
  >
  > 摘自：[GitHub - degit - README](https://github.com/Rich-Harris/degit)

  <img src="https://s2.loli.net/2024/05/24/Bf8hC6dLDzoM94T.png" alt="image-20240524133441772" style="zoom:50%;" />

- **exponential** ：指数的

  **exponent** ：指数，幂

- **sharding upload** ：分片上传

  **sharde** ：碎片

- **span** ：跨度，**跨越**

  > The top layer is a specific layer that **spans** the entire width and height of the viewport and sits on top of all other layers displayed in a web document.
  >
  > 摘自：[MDN US - Top layer](https://developer.mozilla.org/en-US/docs/Glossary/Top_layer)

  **timespan** ：时间跨度

- **on-premises** ：就地，在现场

- **aggregate** ：聚合

- **idiomatic** ：惯用语的

- **interpreter** ：口译员、解释器

  > Python interpreter

- **soundness** ：健全、可靠（性）

  > **A Note on Soundness**
  >
  > TypeScript’s type system allows certain operations that can’t be known at compile-time to be safe. When a type system has this property, it is said to not be “sound”. The places where TypeScript allows unsound behavior were carefully considered, and throughout this document we’ll explain where these happen and the motivating scenarios behind them.
  >
  > 摘自：[typescript doc - Type Compatibility # A Note on Soundness](https://www.typescriptlang.org/docs/handbook/type-compatibility.html#a-note-on-soundness)

- **on-prem** ：本地

  > The examples here use Llama locally, in the cloud, and **on-prem**.
  >
  > 摘自：[GitHub - llama-recipes - README](https://github.com/meta-llama/llama-recipes)
  >
  > <img src="https://s2.loli.net/2024/11/19/D97RW3tUYXmAcNZ.png" alt="image-20241119094059569" style="zoom:50%;" />

- **columnar** ：柱状的

  > 👀 明显是 column 的变体

- **delta** ：**增量**（数学符号 $\Delta$），三角洲

  > The other new configuration option being added will further ensure that the right types of deltas are generated at `git push` time...
  >
  > 摘自：[How we shrunk our Javascript monorepo git size by 94%](https://www.jonathancreamer.com/how-we-shrunk-our-git-repo-size-by-94-percent/)

- **idempotency** ：幂等性

- **proactive** ：主动的

  > 💡 在编程范式和软件架构领域存在 proactive 和 reactive 两种**异步处理**方式
  > - **Proactive （主动）** ：通常指系统主动执行某些操作，比如预先加载数据、主动发起计算等等。
  > - **Reactive（响应式）** ：通常指系统对外部事件或变化做出响应，比如用户点击、数据更新、消息到达等等。

- **introspection** ：内省

  > Playwright waits for elements to be actionable prior to performing actions. It also has a rich set of **introspection** events.
  >
  > 摘自：[playwright 官网](https://playwright.dev/)

- **flaky**：不可靠的

  > The combination of the two eliminates the need for artificial timeouts - the primary cause of flaky tests.
  >
  > 摘自：[playwright 官网](https://playwright.dev/)

- **distro** ：发行版

- **sparse** ：稀疏的

  - **sparse matrix** ：稀疏矩阵
  - **sparse file** ：稀疏文件

- **capacitor** ：电容

- **facet** ：方面

- **entropy** ：熵

- **cohesive** ：内聚的，有结合力的、有凝聚力的

  > On the other hand, if you split up a cohesive piece of logic into separate Effects, the code may look “cleaner” but will be more difficult to maintain.
  >
  > 摘自：[React doc - Lifecycle of Reactive Effects # Each Effect represents a separate synchronization process](https://react.dev/learn/lifecycle-of-reactive-effects#each-effect-represents-a-separate-synchronization-process)
  
  **cohesion** ：凝聚力、连贯性
  > The entire suite of commands is built on a shared foundation to ensure cohesion and consistency.
  > 
  > 摘自：[Announcing Vite+](https://voidzero.dev/posts/announcing-vite-plus)

  **cohere** ：vi. 凝聚，连贯、一致；团结一致

- **numerator** ：（分数中的）分子

- **denominator** ：分母

- **stochastic** ：随机的

  > SGD 即 随机梯度下降 Stochastic Gradient Descent

- **nexus** ：联结

- **dichotomy** ：二分法

- **upsert** ：更新或插入、提升

  > 👀 和 insert 很像。在 [Github - tc39 - proposal-upsert](https://github.com/tc39/proposal-upsert) 中看到，用来描述 `getOrInsert` 这一行为

- **proberen** ：尝试

  **verhogen** ：增加、提高

  > 👀 这两个词就是 OS 领域中 PV 操作的缩写

- **semaphore** ：信号量

- **invent the wheel** ：造轮子

  > 👀 一直都不知道 “造轮子” 是翻译过来的...

- **exhaustive** ：穷尽的、彻底的、详尽的

  > exhaustive 在模式匹配 ( Pattern Matching ) 中，具有非常特殊且重要的含义。
  >
  > 它指的是 **模式匹配中的所有分支（cases/patterns）必须完全覆盖（cover）被匹配的那个值（或类型）的所有可能情况**。
  >
  > 它<font color=red>是一种**编译时保证**</font>，确保你的模式覆盖了被匹配类型的所有可能情况，从而**消除**因遗漏某些情况而导致的**运行时错误**。这是函数式编程语言和一些现代命令式语言中一个强大的安全特性。

- **bulk** ：**批量**，大部分。固定搭配：a bulk of

  > 在计算机科学和信息技术领域，“bulk” 通常用来形容**大规模、批量**的数据、操作或处理过程。它强调的是**数量多**、**整体性**，而不是针对单个、孤立的元素进行操作。使用 “bulk” 的主要目的通常是为了**提高效率、减少开销 (overhead) 和简化管理**。
  >
  > ###### 和 batch 的区别
  >
  > - Bulk （批量/大量）：更侧重于数量和规模。它<font color=red>强调的是一次性操作涉及的数据项非常多</font>。重点在于操作本身的大规模性。例如，BULK INSERT 强调的是一次性插入大量行。
  >
  > - Batch（批次/批处理）：更侧重于分组和处理模式。它<font color=red>强调的是将一组项目（数据、任务、事务）收集起来，然后作为一个单元进行处理</font>。重点在于这种“收集后统一处理”的机制，通常与处理的时间点（例如，非实时、周期性）有关。
  >
  > | **特征** | **Bulk**                                         | **Batch**                                         |
  > | -------- | ------------------------------------------------ | ------------------------------------------------- |
  > | 主要焦点 | 操作涉及的数量/规模很大                          | 分组处理的机制/模式，工作单元                     |
  > | 常见语境 | 数据库操作 (INSERT/UPDATE/DELETE), 数据传输, API | 系统架构 (批处理系统), 作业调度, 处理流程中的分组 |
  > | 强调点   | 单次操作的效率和吞吐量                           | 处理的时机 (非实时), 资源利用 (集中处理)          |
  > | 例子     | `BULK INSERT`, Bulk API, Bulk Data               | Batch Processing, Batch Job, a Batch of Records   |
  >
  > 在某些非正式的场合，或者当两者含义重叠时（例如，“批量更新” 可以说 Bulk Update 或 Batch Update），它们有时可以互换使用。但是，在描述系统架构（Batch Processing vs. Real-time Processing）或特定的数据库优化操作（BULK INSERT）时，使用正确的术语更为精确。
  
- **artifact** ：人工制品、工件

- **jank** ：劣质、不可靠、破旧，**不流畅**、**卡顿**（一般在前端 / UI 领域中使用）

  > “jank” 这个词在日常英语中确实不常用，它更像是一个非正式的、甚至有些俚语化的词汇，尤其在技术圈内比较流行。
  >
  > 在 **用户界面 (UI)、图形渲染和性能优化**相关的子领域中，是**相当常见**的一个术语
  >
  > 它主要的应用场景是用来描述**性能不佳导致的用户体验问题**，特别是视觉上的不流畅感。以下是一些具体的应用：
  >
  > 1. **描述 UI 性能问题 (Describing UI Performance Issues):** 这是最核心的应用。
  > 2. **性能分析与调试 (Performance Analysis & Debugging):** 开发者在讨论和诊断性能瓶颈时会使用这个词。
  > 3. **沟通术语 (Communication Term):** 在开发者、设计师、产品经理之间，"jank" 是一个简洁且被广泛理解的术语，用于快速传达“这个地方体验不好，不流畅，需要优化”的信息。
  >
  > 在编程领域，“jank” 主要是一个与**前端/UI性能**紧密相关的术语，特指那些破坏用户体验流畅感的**视觉卡顿和响应延迟**现象。
  >
  > 部分摘抄自：Gemini 2.5 Pro 的回答，🔗 https://g.co/gemini/share/1a70d9bf609c

- **fire and forget** ：发射后不管

  > 在 wikipedia 中含义是 “自主制导” 的武器，不过在 IT 领域也有使用
  >
  > 在 IT 领域，“Fire and forget” 的主要含义和应用场景包括：
  >
  > 1. **异步通信 (Asynchronous Communication)** ：是最常见的应用场景。发送方发出请求或消息后，不会阻塞等待响应，而是继续处理其他事务。例如：
  >
  >    - **消息队列 (Message Queues):** 生产者将消息放入队列后就完成了它的任务（fire and forget），消费者稍后会从队列中取出并处理消息。
  >
  >    - **日志记录 (Logging):** 应用程序记录日志事件时，通常只是将日志信息发送到日志系统（fire and forget），而不会等待日志系统确认写入成功。
  >
  >    - **发送通知 (Sending Notifications):** 如发送电子邮件或推送通知，系统发出通知请求后通常不会等待确认邮件是否送达或用户是否看到通知。
  >
  > 2. **网络协议 (Networking Protocols)** ：UDP
  >
  > 3. **软件设计模式 (Software Design Patterns)** ：在某些设计模式中，一个组件可能会触发另一个组件的操作，但并不关心操作的结果或何时完成。
  >
  > 详见 🔗 https://g.co/gemini/share/074cdf8ee75a
  >
  > <img src="https://s2.loli.net/2025/05/03/8VJLrtHKszSxvYT.png" alt="image-20250503101248798" style="zoom:33%;" />
  
- **benevolent** ：仁慈

  **BDFL** ：Benevolent Dictator For Life
  
- **project** ：投射

  > **Projects** each source value to an Observable which is merged in the output Observable, emitting values only from the most recently projected Observable.
  >
  > 摘自：[RxJS - switchMap](https://rxjs.dev/api/operators/switchMap)

- **subscript** ：下标

  > 👀 根据 [那些无解的计算机问题【让编程再次伟大#16】](https://www.youtube.com/watch?v=xPbhRDaWODg) 的说法：Dennis Ritchie 的 《The C Programming Language》以及直到最新的 C语言标准委员会的标准稿中都没有引入索引 “index” 的概念，而在数组中括号中的是 “subscript”，因为 C语言中 数组和地址的关系（数组的地址指向第一个元素），subscript 可以理解为 offset 内存地址偏移量 ( `array[subscript] = (*((array) + (subscript * type_size)))` )，这也方便将 C 语言编译为汇编。而现在一些新语言，会在数组开头塞入各种 metadata，导致数组的地址不再指向第一个元素；此时 subscript 不再适用，所以选择了索引 “index”，同时这种表达也更符合人的直觉，不过和机器底层偏离更大了。
  
- **marginal** ：边缘的

- **SOTA / SotA** ：State of the Art 指在特定任务中目前表现最好的方法或模型

  > 关于 SOTA 和 最佳事件的区别询问 Gemini 2.5Pro 的回答 🔗 https://g.co/gemini/share/57d5bfc67e81
  
- **footer-gun** ：地雷、陷阱

  > There are also other **foot-guns** we can run into if we start adding more clean-up logic to our finally block — for example, exceptions preventing other resources from being disposed.
  >
  > 摘自：[TS doc - handbook - release note -  TypeScript 5.2 # `using` Declarations and Explicit Resource Management](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html#using-declarations-and-explicit-resource-management)

  > 按照 [Gemini](https://g.co/gemini/share/1bb60ade9ae7) 的说法：
  >
  > >  "foot-gun" 特指那些**设计上或功能上本身可能没有问题，但却极易被用户（尤其是经验不足的用户）错误使用，从而导致严重负面后果的功能、特性或工具**。它强调的是一种“自我伤害”的可能性——这个工具给了你伤害自己的能力
  
- **backport** ：回溯

  > This is useful to enforce all your packages to use a single version of a dependency, or **backport** a fix.
  >
  > 摘自：[yarn doc - configuration - manifest # resolutions](https://yarnpkg.com/configuration/manifest#resolutions)
  
- **ciphertext** ：密文

- **cipher**：密码 / 密码算法

- **glossary** ：术语表

- **checksums** ：校验和

- **parity** ：一致性、对等

  > When the native codebase has reached sufficient **parity** with the current TypeScript, we’ll be releasing it as **TypeScript 7.0**.
  >
  > 摘自：[ms devlog - ts - typescript native port # Versioning Roadmap](https://devblogs.microsoft.com/typescript/typescript-native-port/#versioning-roadmap)

- **nomenclature** ：命名法、命名系统

  > For the sake of clarity, we’ll refer to them simply as TypeScript 6 (JS) and TypeScript 7 (native), since this will be the nomenclature for the foreseeable future.
  >
  > 摘自：[ms devlog - ts - typescript native port # Versioning Roadmap](https://devblogs.microsoft.com/typescript/typescript-native-port/#versioning-roadmap)
  
- **ordinal** ：序数

- **Human in the Loop (HiTL)** ：人机协同

  > Gemini Code Assist pairs developers with AI agents capable of performing a wide range of actions across the software development life cycle, with support for multiple file edits, full project context, built-in tools and integration with ecosystem tools following Model Context Protocol (MCP), all while incorporating **Human in the Loop (HiTL)** for needed oversight.
  >
  > 摘自：[Gemini Code Assist: AI-first coding in your natural language](https://codeassist.google/)
  
- **dongle** ：（电子）适配器

- **raster** ：光栅、栅格

  **rasterization** ：光栅化

- **compositor** ：合成器

  **compositor thread** ：合成线程、合成器线程
  
- **scavenge** ：搜寻

  > 👀 V8 Scavenger GC 算法

- **misalign** ：错位
- **middle-click** ：中键点击
  **command-click** ：按住 command 键点击
  > There’s a lot of awesome functionality built into linking elements like `<a>` and `<button>`. If you middle click or command-click on them they’ll open in new windows.
  > 
    > 摘自：[URL Design](https://warpspire.com/posts/url-design)
- **sample** : 样本、样品、<font color=red>采样</font>
- **orchestration** ：编排
- **hybrid** ：混合
- **regress** ：回归
  **regression testing** ：回归测试
- **supervision** ：监督
- **morph** ：变形、渐变
- **vertex** ：顶点
- **peripheral** ：周边的设备、周边的
- **modality** ：模态
  **multimodality** ：多模态
***


### 英文术语

- **埋点**：Event Tracking




### 特殊字符

> 👀 因为 Google 是不支持 特殊字符 搜索的，想要带上 特殊字符 进行搜索，就必须要加上 字符的英文；所以这些就非常有必要记录

##### 括号 Bracket

- **`()` / 圆括号**：Round brackets / parentheses
- **`[]` / 方括号**：Square brackets / brackets
- **`{}` / 花括号 / 大括号** ：Curly brackets / braces
- **`<>` / 尖括号**：Angle brackets / chevrons

参考自：[wikipedia - Bracket](https://en.wikipedia.org/wiki/Bracket)

- **星号 `*`**：asterisk。在非正式的交流中也会被称为 “star” （比如 [stack overflow - Meaning of a double star (**) in a file path](https://stackoverflow.com/questions/46547540/meaning-of-a-double-star-in-a-file-path) ），不过，在 Google 搜索中 还是要使用 asterisk，star 没有用

- **连接号**：dash，是一种统称。详见：[wikipedia - 连接号](https://zh.wikipedia.org/wiki/%E8%BF%9E%E6%8E%A5%E5%8F%B7)
  - en dash：表示范围
  - em dash：表示预期转折
  
- **连字符 `-`**：hyphen。用于连接单词，将不同单词连接变成一个单词。

- **分号 `;`** ：semicolon

- **斜杠**
  - 正斜杠  `/`：forward slash
  
    >  👀 另外，和 `/` 相关的缩写中：w/ 的意思是 with，A/C 是空调
  
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

| Symbol                               | How it is helpful                                            |
| :----------------------------------- | :----------------------------------------------------------- |
| Plus sign (`+`)                      | Search for things like blood type [ AB+ ] or for a Google+ page like [ +Chrome ] |
| "At" sign (@)                        | Find social tags like [ `@google` ] or [ `@ladygaga`]        |
| <font color=red>Ampersand</font> (&) | Find strongly connected ideas and phrases like [ Brothers & Sisters ] or [ A&E ] |
| Percent (`%`)                        | Search for a percent value like [ 40% of 80 ] or [ 10% of .1 ] |
| Dollar sign (`$`)                    | Indicate prices, so [ nikon 400 ] and [ nikon `$400` ] give different results |
| Hashtag/number sign (#)              | Search for trending topics indicated by hashtags like [ \#lifewithoutgoogle ] |
| Dash (-)                             | Indicate that words around it are strongly connected as in [ twelve-year-old dog ] and [ cross-reference ] |
| Underscore symbol (_)                | Connected two works like [ quick_sort ]. Your search results will find this pair of words either linked together (e.g., quicksort) or connected by an underscore (e.g., quick_sort). |

> [!TIP]
> 
> 关于 `#` 的 number sign 的含义，一直没注意，直到见到这个句子：
>
> > I actually think that this could increase the total # of developer jobs.
> >
> > 摘自：[The End of Front-End Development](https://www.joshwcomeau.com/blog/the-end-of-frontend-development/)

摘自：[Google搜索特殊字符的方法](https://blog.csdn.net/LongZh_CN/article/details/14453317)

##### 希腊字符

可以参考 [[Latex使用笔记#希腊字符]] 中的内容



### 前后缀

##### 前缀

- `co-` ：协同的、共同的...

  > Remote plugins run as **co-processes**, safely and asynchronously.
  >
  > 摘自：[neovim 官网](https://neovim.io/)

- `inter-` ：相互的

  > interop ：互操作
  >
  > interoperable ：互操作性

- `semi-` ：半

- `quasi-` ：准的，类似的

  <img src="https://s2.loli.net/2024/12/06/YDhadEMJRs4SWrT.png" alt="image-20241206135114843" style="zoom:50%;" />

- `sur-` ：在...之上、超越

  > 具体可见 Gemini 的介绍 https://aistudio.google.com/prompts/1SIcZZlg4cieytfvpYaAU8lNBnE93EsiN

  surreal ：超现实

- `con-` ：

  > ###### “con-”前缀的主要含义
  >
  > <font color=dodgerBlue>“con-”前缀在单词中主要有两种功能：</font>
  >
  > 1. **表示“共同、一起” (with, together)**：<font color=red>这是最常见的意思</font>
  >    - **Connect** （连接）：将事物联系**在一起**。
  >    - **Congregate** （聚集）：人群聚集**在一起**。
  >    - **Combine**（结合）：将多个事物组合**在一起**。
  >    - **Convene**（召集）：为了某个目的走到**一起**。
  > 2. **表示 “彻底、完全”（作为强调）**: <font color=dodgerBlue>有时</font>，<font color=lightSeaGreen>“con-” 不直接表示 “一起” ，而是用来加强词根的语气</font>，可以理解为 “彻底地” ( thoroughly )。
  >    - **Consolidate** （巩固）：**彻底**变得坚固。
  >    - **Confirm** （确认）：**彻底**使其坚定，即证实。
  >    - **Conclude** （结束、断定）：**彻底**地关闭或了结一件事。
  >    - **Convince** （说服）：**彻底**地征服（思想上），即使人信服。
  >
  > ###### “con-”前缀的拼写变体
  >
  > 为了使发音更加顺畅，“con-” 在与不同的字母开头的词根结合时，其拼写会发生相应的变化，这种现象被称为 “同化” ( assimilation )。
  >
  > - **com-**: 通常用在字母 **b, m, p** 前。
  >   - **Combine** (结合)，**Commit** (承诺)，**Compose** (组成)。
  > - **col-**: 用在字母 **l** 前。
  >   - **Collect** (收集)，**Collaborate** (合作)。
  > - **cor-**: 用在字母 **r** 前。
  >   - **Correct** (纠正)，**Correlate** (相互关联)。
  > - **co-**: 通常用在元音字母 (**a, e, i, o, u**) 或 **h** 前。
  >   - **Coexist** (共存)，**Cooperate** (合作)，**Cohere** (连贯)。
  >
  > 摘自：https://aistudio.google.com/prompts/1Krl_Wf1iOKHYQnvogj-6fwGP5FayUpSv

- `du-` / `duel-` ：两个的
  > The prefix _du-_ (found in "duel" and "dual") derives from Latin and means **"two"**. It implies a pairing, doubling, or, specifically in the case of "duel" (derived from _duellum_), a "one-on-one combat" or a fight between two parties.
  > 
  > 摘自：Google 搜索 “duel prefix meaning”
  

##### 后缀

- `-ish` ：用于表达数量或时间的近似值。以 "10ish" 为例， 它是一个口语化的用法，表示数量大约为10。 

  > This is an over-generalization, but over the past 10**ish** years, a lot of complexity has been moving from the server to the client.
  >
  > 摘自：[The End of Front-End Development](https://www.joshwcomeau.com/blog/the-end-of-frontend-development/)
  
- `-wise` ：在…方面

  > In particular, this is not a good idea, **performance-wise**, for hosting a site on a normal web server.
  >
  > 摘自：[GitHub - vite-plugin-singlefile](https://github.com/richardtallent/vite-plugin-singlefile)
  
- `-ate` ：

  > “-ate” 后缀主要有三种功能：构成动词、构成形容词，以及构成名词。
  >
  > - 构成动词 ( Verb-forming Suffix ) ：这是 "-ate" 最主要、最核心的功能。它通常加在名词或形容词之后，表示 “使成为…” 、“引起…活动” 或 “做…动作”。
  > - 构成形容词 (Adjective-forming Suffix) ：表示 “具有…性质的” 或 “充满…的” 。
  > - 构成名词 (Noun-forming Suffix) ：指代一类人或事物，或者表示某种职位、身份、化学物质等
  >
  > 摘自：https://aistudio.google.com/prompts/1Krl_Wf1iOKHYQnvogj-6fwGP5FayUpSv



### 单词缩写

> 💡 可以参考 [[linux与macOS备忘录#Linux 命令缩写由来|Linux 命令缩写由来]] 中的内容

- **pt. n** ：第 n 部分
- **Misc** ：杂项，其他。"Misc" 是 “miscellaneous” 的缩写
- **spec** ：规格 specification
- **ver.** ：版本 version
- **promo** ：推广/ 宣传片 promotion 
- **IMHO** ：依我拙见 In My Humble Opinion
- **biz** ：business



### 常见单词

- **knot** ：n.（用绳索等打的）结。v. 把…打成结（或扎牢）

  **unknot** ： 解开...的结

- **trial** ：n. **审判**，试验，试用；比赛;  v. 试验，**试用**。

  **trail** ：追踪，小径

- **top-notch** ：一流的

  **notch** ：缺口

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

- **embark** ：v. 上船，装船，开始

- **nod** ：点头

- **dominance** ：支配地位，支配、控制

- **ongoing** ：adj. 持续的。n. 进行，前进

- **workload** ：工作量

- **semester** ：学期

- **hype** ：n. 炒作。来自 hyperbole，但是似乎不是缩写？

  <img src="https://s2.loli.net/2023/05/15/cKtwg3T6ZHynzi5.png" alt="image-20230515140741048" style="zoom:45%;" />

- **woe** ：麻烦，悲哀

- **dread** ：v. 恐惧 n. 恐惧

  **dreaded** ：令人害怕的，可怕的

- **budget** ：预算

  > 👀 过于基础，不该忘的

- **exempt** ：豁免

- **scape** ：景观。Netscape 也就是网景

- **subtle** ：细微的，不易察觉的 ( subtle bug )

- **coupon** ：优惠券

- **acquisition** ：获得 。acquire 的名词

   > RAII ：资源获取即初始化 ( Resource Acquisition Is Initialization )

- **informed** ：明智的 👀 这个挺简单的，不该忘记的...

  > Developers and Open Source authors now have a massive amount of services offering free tiers, but it can be hard to find them all to make informed decisions.
  >
  > 摘自：[GitHub - free-for-dev - README](https://github.com/ripienaar/free-for-dev)

- **reserve** ：v. 保留，留存，储备；n. 保留

  > 👀 遇到的地方见下面 “over time”

- **over time** ：随时间推移

  > State is reserved only for interactivity, that is, data that changes over time. Since this is a static version of the app, you don’t need it.
  >
  > 摘自：[React doc - Thinking in React # Step 2: Build a static version in React](https://react.dev/learn/thinking-in-react#step-2-build-a-static-version-in-react)

- **conform** ：遵从，符合，符合

- **suffice** ：足够，足以

- **converse** ：adj. 相反的，逆的 vi. 谈话

- **conversion** ：转换

  > 👀 convert 的名词形式

- **legible** ：清晰的，清澈的

- **legitimate** ：合法的，**正当合理的**

  > All errors flagged by the linter are legitimate. There’s always a way to fix the code to not break the rules.
  >
  > 摘自：[react doc - Lifecycle of Reactive Effects # Recap](https://react.dev/learn/lifecycle-of-reactive-effects#recap)

- **minutiae** ：细枝末节，细节

  > They are built to take into account the code you write in your component, so you will get [inferred types](https://www.typescriptlang.org/docs/handbook/type-inference.html) a lot of the time and ideally do not need to handle the **minutiae** of providing the types.
  >
  > 摘自：[React doc - Using TypeScript # example hooks](https://react.dev/learn/typescript#example-hooks)

- **anatomy** ：解剖

  **dissect** ：解剖

- **premature** ：过早的

  > Don’t optimize prematurely!

  > This is a powerful way to avoid **<font color=lightSeaGreen>pre</font>mature** abstraction and the pain points that come with it.
  >
  > 摘自：[TailwindCSS doc - Reusing Styles](https://tailwindcss.com/docs/reusing-styles)

- **substitute** ：替代

- **rest assured** ( that ) ：请放心

- **paraphrase** ：释义

- **as a rule of thumb** ：根据经验

- **discern** ：鉴别

- **consolidate** ：v. （使）结成一体，合并，整合；（使）巩固；使加强

  > For these cases, you can consolidate all the state update logic outside your component in a single function, called a *reducer.*
  >
  > 摘自：[react doc - Extracting State Logic into a Reducer](https://react.dev/learn/extracting-state-logic-into-a-reducer)

- **upfront** ：提前，预先，一开始（看了下字典没有 “一开始” 的意思，这个属于引申意）

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

- **curricula** ：课程

- **proficient** ：精通的

- **clandestine** ：秘密的

- **prevalent** ：流行的

- **substantial** ：可观的、大量的

- **degraded** ：退化的

- **permissive** ：宽松的、放任的

  > Atuin is open source with a permissive license, and has a growing community.
  >
  > 摘自：[Atuin 官网](https://atuin.sh/)

- **leading-edge** ：前沿

- **lore** ：**学问**，传说

- **ritualistic** ：仪式性的

- **synthesizes** ：合成

- **grind** ：磨碎

- **plow** ：犁

  > The only other way to obtain the information you'll find in this handbook would be to **plow through** a mountain of books and a few hundred technical journals and then add a significant amount of real-world experience.
  >
  > 摘自：Code Complete 2nd Edition

- **nitty-gritty** ：细节

  > It gets into nitty-gritty construction details such as steps in building classes, ins and outs of using data and control structures, debugging, refactoring, and code-tuning techniques and strategies.
  >
  > 摘自：Code Complete 2nd Edition

- **fray** ：磨损

- **distilled** ：蒸馏的

- **swathe** ：捆、包裹

- **circumstance** ：条件、情况

- **applicable** ：恰当的，适用的

- **watershed** ：流域、转折点、分水岭

- **transcend** ：超越

- **esoteric** ：难懂的、深奥的

- **nuance** ：细微差别

- **journalism** ：新闻业

- **distinct** ：清晰的

- **hobble** ：蹒跚

- **comfy** ：舒适的

- **guardrail** ：护栏

- **brittle** ：易碎的

- **sparing** ：adj. 慎用、节俭的

  > Refs are an escape hatch that should be used sparingly
  >
  > 摘自：[react.dev - Manipulating the DOM with Refs # Accessing another component’s DOM nodes](https://react.dev/learn/manipulating-the-dom-with-refs#accessing-another-components-dom-nodes)

- **shortchanged** ：短缺，变短

- **irony** ：讽刺

- **in light of** ：鉴于

- **conceive** ：想出，构思

- **trumpet** ：鼓吹

- **oversee** ：监督、监视

- **inquiry** ：调查、询问、查询

- **mobilize** ：动员

- **resilience** ：韧性、恢复力

- **pill up** ：堆积

- **allergy** ：过敏

- **informal** ：轻松友好的

- **red tape** ：繁文缛节

- **bachelor** ：学士学位，单身汉

- **sucessor** ：继任者

- **from the ground of** ：从头开始

  > Since UnoCSS is built from the ground up, we are able to have a great overview of how atomic CSS has been designed with prior arts and abstracted into an elegant and powerful API.
  >
  > 摘自：[UnoCSS doc - Why UnoCSS? # Tailwind CSS](https://unocss.dev/guide/why#tailwind-css)

- **apples-to-apples** ：公平的比较

  > With quite different design goals, it's not really an apples-to-apples comparison with Tailwind CSS. But we will try to list a few differences.
  >
  > 摘自：[UnoCSS doc - Why UnoCSS? # Tailwind CSS](https://unocss.dev/guide/why#tailwind-css)

- **work around** ：解决方法

  > Another ways to **work around** the limitation of dynamically constructed utilities is that you can use an object that list all the combinations **statically**.
  >
  > 摘自：[UnoCSS doc - Extracting # Static List Combinations](https://unocss.dev/guide/extracting#static-list-combinations)

- **tragedy** ：悲剧

- **befall** ：降临，发生在（某人）身上

- **mandatory** ：强制的

- **exhaustive** ：彻底的

- **drastically** ：极大地、彻底地、剧烈地

  > The `DllPlugin` and `DllReferencePlugin` provide means to split bundles in a way that can drastically improve build time performance.
  >
  > 摘自：[webpack doc - plugin - dllplugin](https://webpack.js.org/plugins/dll-plugin)

- **adverse** ：不利的

- **monopoly** ：垄断

- **stagnation** ：停滞

  > We might run into a **monopoly** and **stagnation** situation, as we had with Internet Explorer 6. Please use this setting with caution.
  >
  > 摘自：[GitHub - browserslist - README](https://github.com/browserslist/browserslist)

- **market share** ：市场份额，市场占有率

  > Chinese QQ Browsers has more **market share** than Firefox and desktop Safari combined.
  >
  > 摘自：[GitHub - browserslist - README # Best Practices](https://github.com/browserslist/browserslist?tab=README-ov-file#best-practices)

- **depict** ：描述

- **conjuction** ：连词，结合

  **in conjuction with** ：与...相结合

- **trove** ：宝库

- **analogous** ：相似的

- **incentivize** ：vt. 激励，奖励

  **incentive** ：n. 激励，奖励

- **peddle** ：兜售

- **demystify** ：揭秘，使明白易懂

  <img src="https://s2.loli.net/2024/03/21/biaoU63cEM7pkqO.png" alt="image-20240321231852217" style="zoom:50%;" />

- **spoiler** ：**剧透**，阻流板

- **introverted** ：内向的

  > 💡 一下说法来自 GPT-4：
  >
  > 词源：来自拉丁语“intro-”（意为“向内”）和“vertere”（意为“转向”），字面上的意思是“转向内部”。

  **extroverted** ：外向的，introveted 的反义词
  
- **fright** ：n. 恐惧 -> v. frighten

- **overly** ：过于，过度

- **inlay** ：嵌入

- **analogy** ：类比

- **strain out** ：过滤掉

- **toss out** ：扔掉，摆脱

- **denote** ：表示

- **bullseye** ：靶心

- **nuance** ：细微差别

- **pertain** ：涉及、相关

  **pertaining** ：相关的
  
- **popularize** ：使普及

- **abide** ：遵守

- **idiom** ：成语

- **groundbreaking** ：开创性的

- **foster** ：促进

- **culminate** ：达到顶点

  <img src="https://s2.loli.net/2024/03/31/TNoLK7waycv3Pzx.png" alt="image-20240331211658667" style="zoom:50%;" />
  
- **clock in** ：打卡

- **arguable** ：可争论的

  **arguably** ：可以说

  <img src="https://s2.loli.net/2024/04/04/hUqm2tlJWLxkCQw.png" alt="image-20240404131636176" style="zoom:48%;" />

- **kick off** ：开始

- **avert** ：避免

- **opaque** ：不透明的

- **thence** ：从那里

  ![image-20240413171033582](https://s2.loli.net/2024/04/13/9chERBmHLP8AJy3.png)

- **rather** ：相当的

  > The user will get rather upset if you send their message at any other time or for any other reason.
  >
  > 摘自：[React doc - Separating Events from Effects # Event handlers run in response to specific interactions](https://react.dev/learn/separating-events-from-effects#event-handlers-run-in-response-to-specific-interactions)
  
- **resign** ：辞职

- **veil** ：面纱

  **unveil** ：揭示，揭幕
  
- **one-off** ：一次性的

  > You can create **one-off** `group-*` modifiers on the fly by providing your own selector as an [arbitrary value](https://tailwindcss.com/docs/adding-custom-styles#using-arbitrary-values) between square brackets:
  >
  > 摘自：[TailwindCSS doc - Handling Hover, Focus, and Other States # Arbitrary groups](https://tailwindcss.com/docs/hover-focus-and-other-states#arbitrary-groups)
  
- **evit** ：避免

  **in<font color=lightSeaGreen>evit</font>ably** ：不可避免地
  
- **dialogue** ：对话

- **craft** ：手艺

  **crafted** ：精心制作的

  > ⚠️ 注意不要和 draft “草稿” 混淆

- **breakage** ：破损

  > If it detects **breakages** of the rules, it will automatically skip over just those components or hooks, and continue safely compiling other code.
  >
  > 摘自：[React doc - React Compiler # What does the compiler do?](https://react.dev/learn/react-compiler#what-does-the-compiler-do)
  
- **sore** ：痛

  **sore point** ：痛点
  
- **overhaul** ：彻底检修

- **punctuation** ：标点符号

- **take this with a grain of salt** ：对此持谨慎态度

  > Note: At scale, there are performance reasons to prefer interfaces ([see official Microsoft notes on this](https://github.com/microsoft/TypeScript/wiki/Performance#preferring-interfaces-over-intersections)) but [**take this with a grain of salt**](https://news.ycombinator.com/item?id=25201887)
  >
  > 摘自：[React TypeScript Cheatsheet - Typing Component Props # More Advice](https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/basic_type_example/#more-advice)

- **nuanced** ：微妙，细微

  > It's a **nuanced** topic, don't get too hung up on it.
  >
  > 摘自：[React TypeScript Cheatsheet - Typing Component Props # More Advice](https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/basic_type_example/#more-advice)

- **grunt work** ：苦活

- **streamlined** ：简化的，流线型的

- **swell** ：膨胀

- **pile on** ：堆积

- **entangle** ：纠缠，缠住

- **overwhelm** ：压倒

- **unwieldy** ：笨手笨脚的

- **schematic** ：示意图

- **landscape** ：景观

- **myriad** ：无数，大量

- **inbound** ：传入，到达

  > In the `main.ts` example above, we simply start up our HTTP listener, which lets the application await **inbound** HTTP requests.
  >
  > 摘自：[nest doc - First steps # Setup](https://docs.nestjs.com/first-steps#setup)
  
- **enormous** ：巨大的

- **arduous** ：艰苦

- **nee** ：原名

  > Metadata-producing decorators (**nee.** "Annotations") need to be generally composable with mutating decorators.
  >
  > 摘自：[GitHub - reflect-metadata - README](https://github.com/rbuckton/reflect-metadata)
  >
  > <img src="https://s2.loli.net/2024/07/04/sVglmCRcKNI64FW.png" alt="image-20240704164136762" style="zoom:50%;" />
  
- **sanitize** ：消毒、使卫生

  > You may wish to **sanitize** user input instead of testing for whitespace, see [transform](https://github.com/yiminghe/async-validator#transform) for an example that would allow you to strip whitespace.
  >
  > 摘自：[GitHub - async-validator - README # whitespace](https://github.com/yiminghe/async-validator#whitespace)
  
- **cardinality** ：基数

  >  [`SCARD`](https://redis.io/docs/latest/commands/scard/) returns the size (a.k.a. cardinality) of a set.
  >
  > 摘自：[redis doc - Redis sets](https://redis.io/docs/latest/develop/data-types/sets/)
  
- **tenant** ：租户

- **distract** ：分散，分散注意力，使分心

- **conformant** ：符合要求，符合标准的

  > Oxc maintains its own AST and parser, which is by far the fastest and most **conformant** JavaScript and TypeScript (including JSX and TSX) parser written in Rust.
  >
  > 摘自：[GitHub - Oxc README](https://github.com/oxc-project/oxc)

- **abundance** ：丰富

  > While many existing JavaScript tools rely on [estree](https://github.com/estree/estree) as their AST specification, a notable drawback is its **abundance** of ambiguous nodes.
  >
  > 摘自：[GitHub - Oxc README](https://github.com/oxc-project/oxc)

- **ambiguity** ：模棱两可

  > This **ambiguity** often leads to confusion during development with estree.
  >
  > 摘自：[GitHub - Oxc README](https://github.com/oxc-project/oxc)
  
- **perceptions** ：感知

- **coalesce** ：合并

  > Nullish **coalescing** operator
  
- **intervent** ：干预

- **deception** ：欺骗

- **rival** ：竞争对手 👀 不该遗忘的

- **coalition** ：联盟

- **diminish** ：减少

- **veto** ：否决

- **take it with a grain of salt** ：将信将疑

- **stance** ：立场

- **malice** ：恶意

- **benevolent** ：仁慈的

- **divine** ：神圣的

- **ruthless** ：无情的

  **ruth** ：怜悯，悲哀
  
- **groundbreaking** ：开创性的

- **uncharted** ：未知的

- **intensity** ：强度

- **malform** ：畸形

  **malformed code** ：格式错误的代码

- **sophisticated** ：精致的

- **fatigue** ：疲劳，乏力

- **skitch** ：涂鸦

- **slang** ：俚语

- **colloquial** ：口语的

- **knob** ：旋钮

- **absurd** ：荒谬的

- **gymnastics** ：体操

  > type gymnastics
  
- **holistic** ：固定的、整体的

  > Most core web frameworks do not come with an opinionated way of fetching or updating data in a **holistic** way.
  >
  > 摘自：[TanStack Query doc - overview # Motivation](https://tanstack.com/query/latest/docs/framework/react/overview#motivation)
  
- **heritage** ：遗产

- **candid** ：坦率的、率直的

- **hatchle** ：孵化

- **reconcile** ：调和

- **identical** ：完全相同的

- **martial** ：军事的

- **laurels** ：桂冠、荣誉

- **retroactive** ：追溯的

- **meticulous** ：细致的，一丝不苟的

- **synonyms** ：同义词

- **curation** ：策展

- **merchandize** / **merchandise** ：v. 销或销售商品的行为。n. 商品、货物

- **abreast** ：并排

  **keep abreast of** ：跟上

- **preamble** ：序言

- **backlog** ：积压

- **pinnacle** ：顶峰

- **detour** ：绕道

- **devise** ：设计

- **advent** ：出现、到来、降临

  > <img src="https://s2.loli.net/2024/12/09/TfmrPu6HUznFsD4.png" alt="image-20241209165938332" style="zoom:50%;" />

- **sharded** ：分片

- **ingest** ：摄入

  > ClickHouse specialises in analytics workloads, and can support very high ingest rates through horizontal scaling and sharded storage.
  >
  > 摘自：[7 Databases in 7 Weeks for 2025](https://matt.blwt.io/post/7-databases-in-7-weeks-for-2025/)

- **semantics** ：语义学

- **mold** ：模具

- **commodity** ：商品

- **prelude** ：前奏

- **digress** ：离题、偏题

- **dispensable** ：非必需的、可有可无的

  **dispense** ：分配、分发

  **indispensable** ：不可或缺

- **unmet** ：未满足

- **piecemeal** ：**逐步的**，零碎的

  > This might be useful in a large multi-package repo that's migrating to a newer version of a dependency **piecemeal**.
  >
  > 摘自：[pnpm doc - Feature - Catalogs # Named Catalogs](https://pnpm.io/catalogs#named-catalogs)
  
- **proactive** ：积极主动的

- **sluggish** ：迟缓的、行动迟缓的

- **motto** ：座右铭

- **deposit** ：存款、押金

- **plenary** ：全体的

  **plenary meeting** ：全体会议
  
- **quirk** ：怪癖

- **decay** ：衰变、衰退

- **rhetoric** ：修辞学

- **tackle** ：对付

- **toxic** ：有毒的

  **toxicity** ：毒性

- **takeaway** ：外卖

  **key takeaway** ：重要信息、关键要点
  
- **prologue** ：序言

- **streamline** ：精简

- **on par with** ：与...相当

- **stealth** ：隐形

- **pierce** ：刺穿

  > Playwright selectors pierce shadow DOM and allow entering frames seamlessly.
  >
  > 摘自：[playwright 官网](https://playwright.dev/)
  
- **outright** ：完全

- **sneak** ：v. 偷偷摸摸、潜行

  **sneakily** ：adv. 偷偷的。形容词形式是：sneaky
  
- **notorious** ：臭名昭著的

  > There are other problems like calculations across Daylight Saving Time (DST) and historical calendar changes, which are notoriously difficult to work with.
  >
  > 摘自：[MDN - JavaScript Temporal is coming](https://developer.mozilla.org/en-US/blog/javascript-temporal-is-coming/)

- **approachable** ：**平易近人的**，和蔼可亲的

- **inclusive** ：包容的

- **steward** ：管家

- **inert** ：惰性

  > 👀 html 中存在 `inert` 全局属性
  >
  > > 在前端开发中，有时我们需要将某些元素暂时“隐藏”起来，从用户视角和交互上完全忽略。这时，新的 `inert` 属性就派上了用场。通过简单地设置元素的 `inert` 属性，整个元素及其子元素会从交互中消失。它们无法被点击、聚焦，也会从可访问性树中移除，甚至页面的“查找”功能也无法选中其中的文本。
  > >
  > > 摘自：[2025，重新认识 HTML！](https://mp.weixin.qq.com/s/QvM087jlWt9K1rTzXP_MiQ)
  
- **intake** ：摄入
   > 👀 类似含义的还有：ingest 表示摄入
   
- **obstruct** ：阻挡

- **metabolism** ：新陈代谢

- **likelihood** ：可能性

- **rejuvenation** ：复兴

- **viewpoint** ：观点

- **masonry** ：砖石工艺
  > 👀 CSS 中 `grid-template-rows` 有属性 `masonry` ，虽然兼容性非常差... Chrome 直至现在 25/3/16 完全不支持，Safari 还在 TP 阶段，Firefox 属于 Flag 功能
  
- **feasible** ：可行

- **whereby** ：凭此，以此方式

- **tinker** ：修补者、修补工

- **slate** ：石板

- **discrepancy** ：差异

- **by and large** ：总的来说

- **without further ado** ：不必多说、毋庸置疑

- **manifesto** ：宣言

- **vibe** ：气氛

- **autonomous** ：自主的

- **emerge** ：出现

   > 👀 相关词语 emergency ：突发事件
   
- **tenant** ：租户

   > 👀 可以引申出 multi-tenant ：多租户
   
- **vibrate** ：震动

- **setback** ：挫折

   > 👀 过于基础了...
   
- **deem** ：认为、视为

- **unagentic** ：无目的、**缺乏主动性**

   > this fact is a trap for competent but unagentic engineers.
   
- **fully fledged** ：羽衣丰满的

- **grasp** ：把握，掌握

- **iffy** ：不确定的

   > Some LLMs are a bit **iffy** when it comes to returning well formed JSON data, sometimes they skip a parentheses and sometimes they add some words in it, because that's what an LLM does.
   >
   > 摘自：[GitHub - json_repair](https://github.com/mangiucugna/json_repair)
   
- **intact** ：完整、完好无损的
   **left intact** ：保持不变
   
   > The `target` setting changes which JS features are downleveled and which are left intact.
   >
   > 摘自：[ts doc - tsconfig # target](https://www.typescriptlang.org/tsconfig/#target)
   
- **capitulate** ：屈服、投降

   > It roughly falls into the same space of tool as [git filter-branch](https://git-scm.com/docs/git-filter-branch) but without the **capitulation**-inducing poor [performance](https://public-inbox.org/git/CABPp-BGOz8nks0+Tdw5GyGqxeYR-3FF6FT5JcgVqZDYVRQ6qog@mail.gmail.com/), with far more capabilities, and with a design that scales usability-wise beyond trivial rewriting cases.
   >
   > 摘自：[GitHub - git-filter-repo](https://github.com/newren/git-filter-repo)
   
- **deport** ：驱逐

- **graveyard** ：墓地

- **routine** ：常规的

- **vague** ：模糊的

- **coordinated** ：协调的、组织有序的

- **magnitude** ：大小

- **guideline** ：准则、指南

- **necessitate** ：使成为必要、需要

- **quota** ：配额、限额

- **backoff** ：回退

   > exponential backoff ：指数回退

- **backlog** ：积压、积压的工作

- **excel (at)** ：突出、擅长...

- **radial** ：放射状的、径向

   > 👀 半径 radius 的形容词形式
   
- **symmetric** ：对称的

- **emulate** ：模仿

- **paralysis** ：瘫痪

- **offload** ：卸下、卸载

- **ovan** ：烤箱

- **metaphor** ：隐喻

- **dominate** ：主导、支配、统治

   > 👀 有点基础了...

- **sophisticated** ：精致、老于世故的、复杂的

   > But the modern approach goes beyond just replacing your HTTP library. You get **sophisticated** timeout and cancellation support built-in:
   >
   > 摘自：[Modern Node.js Patterns for 2025](https://kashw1n.com/blog/nodejs-2025/)
   
- **tribal** ：部落
   **tribal knowledge** ：部落知识
   > Tribal knowledge refers to **the undocumented, informal expertise and insights that exist within a specific group or organization**. It's often passed down through word-of-mouth, observation, or shared experiences, rather than through formal documentation or training. This knowledge can be valuable but also poses risks if not properly captured and shared.
   > 来源：Google 搜索自带 AI 概览
   > 
   > 这个问题组成 tribal knowledge（部落知识）问题，也就是每一个部落都拥有自己的知识，都不写下来，只能通过口口相传，基本上不可能被传播到部落以外。每一个部落都跟其它部落不一样，你不能举一反三。这是很多公司在快速增长过程中会遇到的痛点。
   > 
   > 摘自：[如何看待著名程序员 DHH 宣布不再使用 TypeScript 改回 JavaScript？ - Cat Chen的回答 - 知乎](https://www.zhihu.com/question/623259697/answer/3222950408)
   
- **surname** ：姓氏
- **de facto** ：事实上的
  **de jure** ：法律上的
- **escalate** ：逐步上升 / 增强 / 恶化
- **sphere** ：球体
- **vanity** ：虚荣、虚荣心
- **alpine** ：高山的
  > Alpine Linux
- **prelude** ：序曲、前奏
  **preliminary** ：初步
- **density** ：密度
  **dense** ：密集的
- **condescending** ：傲慢的、居高岭下的
- **freed up** ：腾出、释放出
- **ditto** ：同上
- **mesh** ：表格、网格（ three.js 相关）
- **fast-forward** ：快进
  > 👀 此外，git 的 merge 操作也有 fast-forward 模式
  
- **merch** ：商品
- **epilogue** ：尾声
- **latent** ：潜在的
  
  > latent space ：潜空间
  >
  > latent demand ：潜在需求
  
- **deliberate** ：深思熟虑的、蓄意的
  > Therefore, our team took **deliberate** steps to implement it without sacrificing stability or ecosystem compatibility.
  >
  > 摘自：[Vite 8 Beta: The Rolldown-powered Vite](https://vite.dev/blog/announcing-vite8-beta)
  
- **pinpoint** ：精准定位
  > With real-time feedback during builds, pinpoint issues in the build log and utilize robust test reporting, keeping your team on top of the CI/CD process.
  > 摘自：[JetBrains - TeamCity](https://www.jetbrains.com/teamcity/)
  
- **sculpt** ：雕刻、使...成形
- **epoch** ：纪元、年代
- **chronic** ：慢性的
  **chronic disease** ：慢性病
- **piecewise** ：分段的
  > `visualMap.type` 的可选值之一 `"piecewise"`
- **graspable** ：可感知的、可理解的
- **aesthetics** ：美学
- **polyglot** ：通晓多种语言的、万用语言者
- **sloppy** ：邋遢的
  > sloppy mode ：宽松模式（相较于严格模式 strict mode ）
- **overhang** ：悬垂
- **mandate** ：授权
- **steer** ：引导；操纵；方向盘