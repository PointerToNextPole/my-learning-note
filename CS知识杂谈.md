# CS 知识杂谈



> ⚠️ **由于水平不够没有时间仔细研究或只是一时好奇，这里很多只是摘取了概念，也只是挖坑；如需深入理解，还需进一步查资料与添加笔记**



#### 元编程
元编程一言以蔽之，就是用<font color=FF0000>代码生成（操纵）代码</font>。

<font color=FF0000>常见的开发语言均能做到元编程</font>，Lisp 这货就不用多说了，C 的 Marco，<font color=FF0000>C++ 的 Template，Java的Annotation</font>，C# 的 Attribute、Reflection、CodeDom 和 IL Emitter，各种脚本语言（如 js、python）的 eval，甚至连 Unix/Linux 的 shell 脚本也能。

元编程常见的<font color=FF0000>**应用场景**</font>很多，扩展（重构）语法、开发 DSL、生成代码、根据特定场景自动选择代码优化、解决一些正交的架构设计问题、AOP等等。

摘自：[什么是元编程以及元语言？ - 猫杀的回答 - 知乎](https://www.zhihu.com/question/22572900/answer/21828721)



#### 图灵机

图灵机（Turing Machine）是图灵在1936年发表的 "On Computable Numbers, with an Application to the Entscheidungsproblem"（《论可计算数及其在判定性问题上的应用》）中提出的数学模型。既然是数学模型，它就并非一个实体概念，而是架空的一个想法。在文章中图灵描述了它是什么，并且证明了：<font color=LightSeaGreen>只要图灵机可以被实现，就可以用来解决任何可计算问题。</font>

**图灵机的结构包括以下几个部分：**

> - 一条无限长的<font color=FF0000>纸带</font>（tape），纸带被分成一个个相邻的格子（square），每个格子都可以写上至多一个字符（symbol）。
> - 一个<font color=FF0000>字符表</font>（alphabet），<font color=LightSeaGreen>即字符的集合，它包含纸带上可能出现的所有字符。</font>其中包含一个特殊的空白字符（blank），意思是此格子没有任何字符。
> - 一个读写头（head），可理解为指向其中一个格子的指针。它可以读取/擦除/写入当前格子的内容，此外也可以每次向左/右移动一个格子。
> - 一个状态寄存器（state register），它追踪着每一步运算过程中，整个机器所处的状态（运行/终止）。当这个状态从运行变为终止，则运算结束，机器停机并交回控制权。如果你了解有限状态机，它便对应着有限状态机里的状态。
> - 一个有限的指令集（instructions table），它记录着读写头在特定情况下应该执行的行为。可以想象读写头随身有一本操作指南，里面记录着很多条类似于“当你身处编号53的格子并看到其内容为0时，擦除，改写为1，并向右移一格。此外，令下一状态为运行。”这样的命令。其实某种意义上，这个指令集就对应着程序员所写下的程序了。
>
> <img src="https://s2.loli.net/2022/08/27/Pb5VAnuNgGQ8TCE.png" alt="图灵机结构" title="图灵机结构" style="zoom: 67%;" />
>
> 在计算开始前，纸带可以是完全空白，也可以在某些格子里预先就有写上部分字符作为输入。运算开始时，读写头从某一位置开始，严格按照此刻的配置（configuration），即：
> - 当前所处位置
> - 当前格子内容
>
> 来一步步的对照着指令集去进行操作，直到状态变为停止，运算结束。而后纸带上留下的信息，即字符的序列（比如类似“...011001...”）便作为输出，由人来解码为自然语言。
>
> 要重申一下，以上只是图灵机模型的内容，而非具体的实现。所谓的纸带和读写头都只是图灵提出的抽象概念。为便于理解打一个比方。算盘虽然不是图灵机（因为它没有无限长的纸带，即无限的存储空间），但它的行为与图灵机一致。每一串算珠都是纸带上的一格，一串算珠上展示的数字便记录着当前格中的字符（可以是空白，可以是 12345 ）。人类的手即是读写头，可以更改每串算珠的状态。算盘的运行遵循人脑中的算法，当算法结束，算盘停机。
>
> 摘自：[什么是图灵完备？ - Ran C的回答 - 知乎](https://www.zhihu.com/question/20115374/answer/288346717)



#### 图灵完备

在可计算性理论里，如果一系列操作数据的规则（如指令集、编程语言、细胞自动机）<font color=FF0000>可以用来模拟单带图灵机</font>，那么它是图灵完备的。

摘自：[图灵完备-百度百科](https://baike.baidu.com/item/%E5%9B%BE%E7%81%B5%E5%AE%8C%E5%A4%87/4634934?fr=aladdin)

图灵完备 ( Turing Completeness ) 是指<font color=LightSeaGreen>机器执行任何其他可编程计算机能够执行计算的能力</font>。

图灵完备也意味着你的语言可以做到能够用图灵机能做到的所有事情，可以解决所有的可计算问题。

摘自：[图灵完备是什么？](https://www.jianshu.com/p/b8b492ea547d)



#### 编程范式

编程范型、编程范式或程序设计法 ( Programming paradigm)，（范即模范、典范之意，范式即模式、方法），是一类典型的编程风格，是指从事软件工程的一类典型的风格（可以对照方法学）。

摘自：[编程范型-百度百科](https://baike.baidu.com/item/%E7%BC%96%E7%A8%8B%E8%8C%83%E5%9E%8B/1475451?fromtitle=%E7%BC%96%E7%A8%8B%E8%8C%83%E5%BC%8F&fromid=23696164&fr=aladdin)



#### 行为树

行为树是一种形式化的图形建模语言，主要用于系统和软件工程。 行为树采用明确定义的符号来明确表示数百甚至数千种自然语言需求，这些需求通常用于表达大规模软件集成系统的利益相关者需求。

摘自：[行为树-百度百科](https://baike.baidu.com/item/%E8%A1%8C%E4%B8%BA%E6%A0%91/22735785?fr=aladdin)



#### UUID

**UUID** 是指 Universally Unique Identifier，翻译为中文是**通用唯一识别码**，<font color=LightSeaGreen>UUID 的目的是让分布式系统中的所有元素都能有唯一的识别信息</font>。如此一来，每个人都可以创建不与其它人冲突的 UUID，就不需考虑数据库创建时的名称重复问题。

##### 定义

UUID 是<font color=LightSeaGreen>由一组32位数的16进制数字所构成</font>，是故 UUID 理论上的总数为1632=2128，约等于3.4 x 10123。

##### 格式

UUID 的十六个八位字节被表示为 32个十六进制数字，<font color=LightSeaGreen>以连字号分隔的五组来显示，形式为 8-4-4-4-12</font>，总共有 36个字符（即三十二个英数字母和四个连字号）。例如：

```
123e4567-e89b-12d3-a456-426655440000
xxxxxxxx-xxxx-Mxxx-Nxxx-xxxxxxxxxxxx
```

数字 `M` 的四位表示 UUID 版本，当前规范有 5个版本，M可选值为 `1, 2, 3, 4, 5` ；

数字 `N` 的一至四个最高有效位表示 UUID 变体 ( variant )，有固定的两位 `10xx` 因此只可能取值 `8, 9, a, b`

UUID 版本通过 M 表示，当前规范有5个版本，M可选值为`1, 2, 3, 4, 5`。这5个版本使用不同算法，利用不同的信息来产生UUID，各版本有各自优势，适用于不同情景。具体使用的信息

- version 1, date-time & MAC address
- version 2, date-time & group/user id
- version 3, MD5 hash & namespace
- version 4, pseudo-random number
- version 5, SHA-1 hash & namespace

使用较多的是版本1和版本4，其中版本1使用当前时间戳和MAC地址信息。版本4使用(伪)随机数信息，128bit中，除去版本确定的4bit和variant确定的2bit，其它122bit全部由(伪)随机数信息确定。

因为时间戳和随机数的唯一性，版本1和版本4总是生成唯一的标识符。若希望对给定的一个字符串总是能生成相同的 UUID，使用版本3或版本5。

摘自：[什么是 UUID](https://www.jianshu.com/p/da6dae36c290)



#### foo & bar

foo & bar 是类似于”哑变元“，<font color=LightSeaGreen>我们可以把 foo 理解成张三李四，或者"某某"</font>，但真正了解 foo 及 foobar 等词的含义，还是能使我们更好的理解英文文档，避免产生不必要的歧义。

术语 foobar , foo , bar , baz 和 qux 经常在计算机编程或计算机相关的文档中 <font color=LightSeaGreen>被用作**占位符**的名字</font>。<font color=LightSeaGreen>当变量，函数，或命令本身不太重要的时候， foobar , foo , bar , baz 和 qux 就被用来充当这些实体的名字</font>，这样做的目的仅仅是阐述一个概念，说明一个想法。这些术语本身相对于使用的场景来说没有任何意义。Foobar经常被单独使用；而当需要多个实体举例的时候，foo，bar，和baz则经常被按顺序使用。

摘自：[你所不知道的“foo”和“bar”](https://cloud.tencent.com/developer/article/1360988)



#### 哑元

哑元，即虚拟变量 ( Dummy Variables ) 又称虚设变量、名义变量或哑变量，用以反映质的属性的一个人工变量，是量化了的自变量，通常取值为0或1。<font color=LightSeaGreen>引入哑变量可使线形回归模型变得更复杂，但对问题描述更简明，一个方程能达到两个方程的作用，而且接近现实</font>。

摘自：[百度百科 - 虚拟变量]([https://baike.baidu.com/item/%E8%99%9A%E6%8B%9F%E5%8F%98%E9%87%8F/8262721?fr=aladdin](https://baike.baidu.com/item/虚拟变量/8262721?fr=aladdin))



#### Mixin

**Mixin是面向对象程序设计语言中的类**，提供了方法的实现。其他类可以访问mixin类的方法而不必成为其子类。Mixin有时被称作"included"而不是"inherited"。mixin为使用它的class提供额外的功能，但 <font color=dodgerBlue>自身却不单独使用（不能单独生成实例对象，属于抽象类）</font>。因为有以上限制，Mixin类通常作为功能模块使用，在需要该功能时“混入”，而且不会使类的关系变得复杂。用户与 Mixin 不是 “is-a” 的关系，而是 “-able” 关系

<font color=FF0000>Mixin 有利于代码复用 又 **避免了多重继承的复杂**</font>。使用 Mixin 享有单一继承的单纯性和多重继承的共有性。接口与 mixin 相同的地方是都可以多继承，不同的地方在于 mixin 是带实现的。Mixin 也可以看作是带实现的 interface 。这种设计模式实现了依赖反转原则。

Mixin 实质上是利用语言特性（比如 Ruby 的 include 语法、Python 的多重继承）来更简洁地实现组合模式。

摘自：[wikipedia - Mixin](https://zh.wikipedia.org/wiki/Mixin)	[知乎 - Mixin是什么概念?](https://www.zhihu.com/question/20778853)



#### ABI

In [computer software](https://en.wikipedia.org/wiki/Computer_software), an <font color=FF0000>**application binary interface** (**ABI**) is an [interface](https://en.wikipedia.org/wiki/Interface_(computing)) between two binary program modules</font>（译：二进制程序模块）. Often, one of these modules is a [library](https://en.wikipedia.org/wiki/Library_(computing)) or [operating system](https://en.wikipedia.org/wiki/Operating_system) facility（译：提供的服务）, and the other is a program that is being run by a user.

An <font color=fuchsia>***ABI***</font> defines how data structures or computational routines（译：例程。相关内容，详见下面 [[#routine 例程]]） are accessed in [machine code](https://en.wikipedia.org/wiki/Machine_code), which is a <font color=fuchsia>**low-level**</font>, hardware-dependent format. In contrast, an [***API***](https://en.wikipedia.org/wiki/Application_programming_interface) defines this access in [source code](https://en.wikipedia.org/wiki/Source_code), which is a relatively <font color=DeepSkyBlue >**high-level**</font>, hardware-independent, often [human-readable](https://en.wikipedia.org/wiki/Human-readable) format. A common aspect of an ABI is the [calling convention](https://en.wikipedia.org/wiki/Calling_convention), which <font color=FF0000>determines how data is provided as input to, or read as output from</font>, computational routines. Examples of this are the [x86 calling conventions](https://en.wikipedia.org/wiki/X86_calling_conventions).

Adhering to an ABI (which may or may not be officially standardized) is usually the job of a [compiler](https://en.wikipedia.org/wiki/Compiler), operating system, or library author. However, <font color=FF0000>an application programmer may **have to deal with an ABI directly** when **writing a program in a mix of programming languages**</font> , or even compiling a program written in the same language with different compilers. （译：如果撰写一个程序使用多种语言，或使用多种编译器编译统一语言的程序，就必须直接处理 ABI ）

摘自：[wiki - ABI](https://en.wikipedia.org/wiki/Application_binary_interface)

#### 例程 routine

> 什么是 “子程序 ( routine ) ” ？子程序 是为实现一个特定的目的而编写的一个可被调用的 方法 ( method ) 或 过程 ( procedure )。例如 C++ 中的函数 ( function )， Java 中的方法 ( method )，或 Microsoft Visual Basic 中的函数过程 ( function procedure ) 或 子过程 ( sub procedure )。对于某些使用方式，C 和 C++ 中的 宏 ( macro ) 也可认为是子程序。
>
> 摘自：《代码大全（第二版）》chapter 7 - P162

如果细分起来：function 是有返回值的 routine，procedure（过程）是没有返回值的 routine，method 是作为 class 的成员的 routine，甚至 C++ 中重载了的运算符也算是 routine。👀 这部分也可以参考 [[#Side Effect 副作用]] 中的内容

摘自：[CC2e 术语：把 routine 译为“子程序”的理由](https://blog.csdn.net/techcrunch/article/details/1961970)

#### 协程 coroutine

###### 一些前置概念

许多编程语言使用术语 “函数” ( Function ) 或 “方法” ( Method ) 或 “过程” ( Procedure ) 或 “子程序” ( Subroutine )来表示可以被其他代码调用的一段代码。<font color=lightSeaGreen>一些比较古老的编程语言，比如 Fortran，会使用 Subroutine 这个词</font>；在 Python 中，主要使用 “函数” ( Function ) 或 “方法” ( Method ) 这两个概念。

<font color=dodgerBlue>**大多数编程语言都有所谓的 “子程序” ( Subroutine ) 调用模型**</font>。<font color=lightSeaGreen>父函数遇到一个子程序，开始调用该函数，进入到该函数的开头，然后一直执行，直到函数的结尾（或遇到 `return` 语句），返回到调用该函数的地方</font>。所有对该函数的调用都遵循上面的这个流程：进入函数，从头开始，直到 `return` 结尾，返回父函数调用处。这种调用模型我们再熟悉不过了。

<font color=dodgerBlue>此外，**还有另一种代码执行模型，称为 “协程” ( Coroutine ) 调用模型**</font>。在这个调用模型中，<font color=fuchsia>被调用者可以 “让出”控制权而不是把整个函数都返回</font>。当这个被调用的协程 “让出” 控制权时，调用他的父函数会立即返回到当初调用协程的的位置。但未来再次调用协程时，不会从头开始，而是从上一次该协程执行停止的地方继续执行。控制权在父函数和协程之间来回跳转，如下图所示：

![](https://s2.loli.net/2025/04/09/uQzWIEPin39a8Se.webp)

摘自：[python asyncio的设计晦涩难懂，一点也不python，是做毁了吗？ - PP鲁的回答 - 知乎](https://www.zhihu.com/question/451397804/answer/3123427382)

##### 概念

协程 ( Coroutine ) 是一种程序组件，它<font color=red>比线程更为轻量级</font>，<font color=red>**用于处理并发编程**，特别典型的应用场景为异步编程</font>。<font color=dodgerBlue>与传统的线程相比</font>，协程<font color=red>主要运行在单个线程内</font>，但<font color=red>能在特定的时刻挂起和恢复执行</font>，这种特性使得协程非常适合进行I/O密集型和高并发的任务。

###### 协程的工作原理

协程的关键点在于它的执行可以被挂起并在稍后某个时刻从挂起处继续执行，而<font color=red>这种挂起并不会阻塞程序中的其它协程</font>。这是通过异步编程实现的，当一个协程在等待I/O操作（如网络请求、文件读写等）完成时，它会被挂起，而程序的控制权返回到事件循环中，事件循环继续处理其他协程的执行，直到I/O操作完成，原来的协程再恢复执行。

###### 与线程的区别

- **资源消耗**：<font color=dodgerBlue>创建和销毁线程的代价相对较高</font>，<font color=red>协程因为是在单线程内切换，所以资源消耗更低</font>。
- **切换开销**：<font color=dodgerBlue>**线程的切换涉及到操作系统的调度，上下文切换的成本相对较高**</font>。<font color=red>**协程的切换是由程序自身控制**，而不是操作系统，因此切换开销小</font>。
- **数据共享**：<font color=dodgerBlue>线程间的数据共享需要考虑同步问题，可能涉及锁，这可能带来死锁等问题</font>。协程由于运行在单线程中，自然避免了这些问题，但数据共享的管理需用户自行控制。

###### 编程语言的支持

许多现代编程语言都内置了对协程的支持，或通过库的形式提供支持。例如：

- **Python**：通过 asyncio 库和 async/await 语法提供对协程的支持。
- **JavaScript**：在 ES6 中，通过 Generator函数及 Promise对象支持异步编程，而 async/await 在 ES2017被正式引入。
- **Go**：语言层面支持轻量级线程( goroutine )；虽然不是传统意义上的协程，但具有类似的使用场景和优点
- **C#**：通过 async 和 await 关键字支持异步编程模式。

###### 实际应用

协程在网络编程、多任务异步处理、并发任务等领域有广泛应用。例如，协程可以用于 Web 服务器来处理数万个并发连接，对于爬虫程序来进行高效的数据抓取等场景也非常适合使用协程。

###### 总结

协程提供了一种高效的方法来执行多任务，尤其是I/O密集型任务。它<font color=red>允许单线程中的多个任务交错执行，通过任务之间的合理切换，大幅提高应用程序的性能与资源利用率</font>。理解并正确使用协程，能在开发高效和响应迅速的程序中发挥重要作用。

<img src="https://s2.loli.net/2024/08/01/fZPUQxJzO5itYXF.png" alt="image-20240801122639745" style="zoom:50%;" />



##### 有栈协程 vs 无栈协程

| 特色                                                 | 有栈协程                                                     | 无栈协程                                                     |
| :--------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 可以挂起的位置                                       | 任意函数                                                     | 仅有显式标注了 async 的函数                                  |
| 让出执行权时机                                       | 任意位置                                                     | 仅可显式 yield                                               |
| 上下文存放于(至少需要当前函数的栈，寄存器和重入位置) | 由运行时管理的协程栈                                         | 对象或闭包                                                   |
| 如何挂起                                             | 运行时暂停当前函数的执行                                     | 把普通函数编译成可重入函数，挂起时直接结束当前函数           |
| 如何恢复                                             | 由运行时从协程栈中提取局部变量和挂起位置                     | 重新调用函数，函数内从 continuation 中提取 label 并找到相应位置 |
| 额外内存开销                                         | 每个协程都需要独立的栈空间，可以设计成动态大小来减少开销 接近于零 |                                                              |
| 需要                                                 | 运行时支持                                                   | 编译器/预处理器支持                                          |
| 已有代码                                             | 可能不改写                                                   | 需要改写                                                     |

摘自：[用 TypeScript 看无栈协程](https://shockwave.me/8260120b7806/)



#### 不同语言间互相调用 & SharedMemory & FFI & RPC

> ⚠️ 下面内容摘自：[不同语言为什么不能相互调用 ？ - 酱紫君的回答 - 知乎](https://www.zhihu.com/question/353354060/answer/2554554876) 。此文章循序渐进的说明 “不同语言（程序）间互相调用”，直至 讲到 目前的最佳实践 RPC，相当适合了解 RPC，及其背景

不同语言能相互调用，常用的有 SM、FFI、IPC、MQ、RPC 等方式

##### sharedMemory

<font color=FF0000>效率最高的叫共享内存</font>。

我开辟一块内存给你，我往里面写数据, 写完你给算，你改下互斥锁的状态，我就当你算完了。

访问共享内存区域和访问进程独有的内存区域一样快，并不需要通过系统调用或者其它需要切入内核的过程来完成；同时还能避免对数据的各种不必要的复制，好处非常多。

**缺点**：<font color=FF0000>资源和状态管理比较复杂</font>，还有要是<font color=FF0000>对面崩了容易把你也给带崩</font>。但是话说回来，我给你改的你才能改，我没给你改的，你不能瞎改，你硬要改，那叫外挂。

##### FFI ( Foreign Function Interface )

语言交互接口 FFI 是<font color=FF0000>**最常见的相互调用方式**</font>。

计算机的运算，最底层都是以二进制的形式存在。所有的语言在编译后，都会以二进制的形式去执行指令，或者从字节码动态翻译出指令执行。

那么我<font color=FF0000>把输入输出的空间准备好</font>，然后<font color=FF0000>直接调用别的语言生成的二进制</font>去跑不就行了吗？这就是 FFI，<font color=FF0000>**FFI 能更好的管理二进制资源的生命周期**</font>。

管生命周期的那个作为宿主语言，所以 FFI 中一般高级语言做主导，低级语言被调用，而且还能封装成二进制库，比如 lib、dll、so 之类的给不同语言用（👀 比如 ffmpeg ？）。

**缺点**：二进制毕竟太底层了，没有大家一致认可的调用约定也是不可能互通的，要遵守一些基本准则。调用约定、类型表示 和 名称修饰这三者，构成了二进制接口 ( Application Binary Interface )。

但是, 世界上这么多 CPU，这么多语言，每个类型系统都不一样，想想也没法统一；不能统一就会有混乱, 混乱乘以发展时间就是屎山的大小。你用 node 肯定被 node-gyp 恶心过，用 Java 肯定被 JNI 恶心过。在时间面前，再好的约定也会变得混乱，最后把你埋葬在屎山之中

> 💡 JS 生态就有 FFI 插件：[node-ffi](https://github.com/node-ffi/node-ffi)

##### RPC ( Remote Procedure Call )

那我不管别人怎么规定了，我就按自己的规定来，你们都给我转成这个格式交互（👀 适配器模式，中间层？）。

比如我用 javascript，那你们和我交互都发 json，岂不美哉？

<font color=dodgerBlue>缺点：</font>只有一点点微小的代价：<font color=FF0000>发出时有一个序列化开销，收到时还有一个反序列化开销，对面也有这两个开销</font>。

虽然名义上是远程调用，但是也常常开两个端口本地往本地发。比如 LSP 就基于 JSON-RPC ，本地往本地发（💡 LSP 即，语言服务协议，这也是 VS Code 可以支持编写多语言的技术原理）。

当然 json 的 serde（👀 进行 “序列化” 和“反序列化”操作的程序，似乎是 [GitHub - serde-rs/json](https://github.com/serde-rs/json) ） 太慢了，计算机就该用二进制说话，所以 <font color=FF0000>谷歌的 **grpc** 使用 **Protocol Buffers** 这种二进制格式作为交换格式</font>。

> 💡 ProtoBuf 是一种 IDL ( Interactive Data Language ) 接口描述语言。

一般的应用远远达不到要优化掉 serde 开销这个地步，所以现在就慢慢演变到传 json 搞定一切了。

摘自：[不同语言为什么不能相互调用 ？ - 酱紫君的回答 - 知乎](https://www.zhihu.com/question/353354060/answer/2554554876) 



#### RPC

##### Wikipedia 中的解释

分布式计算中，远端程序呼叫 ( Remote Procedure Call, RPC ) 是一个计算机通信协议。该协议 <font color=FF0000>允许 运行于一台计算机的程序 调用 另一个地址空间</font>（通常为一个开放网络的一台计算机）<font color=FF0000>的子程序</font>，而 <font color=FF0000>程序员就像调用本地程序一样，无需额外地为这个交互作用编程（无需关注细节）</font>。RPC 是一种 服务器-客户端 ( Client/Server ) 模式，经典实现是一个通过 **发送请求-接受回应** 进行信息交互的系统。

如果涉及的软件采用面向对象编程，那么远程过程调用亦可称作远端呼叫或远端方法呼叫，例：Java RMI 。

<font color=FF0000>RPC 是一种 **进程间通信** 的模式，程序分布在不同的地址空间里</font>。如果 <font color=dodgerBlue>**在同一主机里**</font>，RPC 可以通过不同的虚拟地址空间（即便使用相同的物理地址）进行通讯；而 <font color=dodgerBlue>**在不同的主机间**</font>，则通过不同的物理地址进行交互。许多技术（通常是不兼容）都是基于这种概念而实现的。

摘自：[wikipedia - 远程过程调用](https://zh.wikipedia.org/zh-cn/%E9%81%A0%E7%A8%8B%E9%81%8E%E7%A8%8B%E8%AA%BF%E7%94%A8)

##### 知乎问题 “谁能用通俗的语言解释一下什么是 RPC 框架？”中的讨论

RPC 是指远程过程调用，也就是说两台服务器 A 和 B，一个应用部署在 A服务器 上，想要调用 B服务器 上应用提供的函数/方法，由于不在一个内存空间，不能直接调用，需要通过网络来表达调用的语义和传达调用的数据。

###### 需要解决如下问题

- 首先，<font color=FF0000>**要解决通讯的问题**</font>。主要是通过在客户端和服务器之间建立 TCP 连接，远程过程调用的所有交换的数据都在这个连接里传输。连接可以是按需连接，调用结束后就断掉，也可以是长连接，多个远程过程调用共享同一个连接。

- 第二，要解决寻址的问题。也就是说，<font color=FF0000>**A服务器上的应用怎么告诉底层的 RPC 框架，如何连接到 B服务器（如主机 或 IP地址）以及特定的端口，<font size=4>方法的名称名称是什么</font>**，这样才能完成调用</font>。比如基于 Web 服务协议栈的 RPC，就要提供一个 endpoint URI ，或者是从 UDDI 服务上查找。如果是 RMI 调用的话，还需要一个 RMI Registry 来注册服务的地址。

- 第三，<font color=FF0000>**当 A服务器 上的应用发起远程过程调用时，方法的参数需要通过底层的网络协议如 TCP 传递到 B服务器**</font>，由于网络协议是基于二进制的，内存中的参数的值要序列化成二进制的形式，也就是<font color=FF0000>**序列化 ( Serialize ) 或编组 ( marshal )**</font> ，通过寻址和传输将序列化的二进制发送给 B服务器。

  > 👀 关于 Marshal ，可见 [[#Marshal]]

- 第四，B服务器收到请求后，需要对参数进行反序列化（序列化的逆操作），<font color=FF0000>**恢复为内存中的表达方式**</font>，然后<font color=FF0000>**找到对应的方法**（寻址的一部分）进行本地调用</font>，然后得到返回值。

- 第五，<font color=FF0000>**返回值还要发送回 A服务器 上的应用**</font>（👀 这点差点忘了... 类似函数调用，往往有返回值；这里自然要发送），也<font color=FF0000>要经过序列化的方式发送</font>；A服务器 接到后，再反序列化，恢复为内存中的表达方式，交给 A服务器 上的应用

<img src="https://s2.loli.net/2022/07/12/VqJ8LAHpDnUjxyG.png" alt="img" style="zoom:50%;" />

摘自：[谁能用通俗的语言解释一下什么是 RPC 框架？ - 用心阁的回答 - 知乎](https://www.zhihu.com/question/25536695/answer/36197244)

RPC 就是要像调用本地的函数一样去调远程函数。在研究 RPC 前，我们先看看本地调用是怎么调的。假设我们要调用函数 Multiply 来计算 `lvalue * rvalue` 的结果:

```cpp
1 int Multiply(int l, int r) {
2    int y = l * r;
3    return y;
4 }
5 
6 int lvalue = 10;
7 int rvalue = 20;
8 int l_times_r = Multiply(lvalue, rvalue);
```

那么在第8行时，我们实际上执行了以下操作：

1. 将 lvalue 和 rvalue 的值压栈
2. 进入Multiply函数，取出栈中的值10 和 20，将其赋予 l 和 r
3. 执行第2行代码，计算 l * r ，并将结果存在 y
4. 将 y 的值压栈，然后从Multiply返回
5. 第8行，从栈中取出返回值 200 ，并赋值给 l_times_r

以上 5步就是执行本地调用的过程（作者注：以上步骤只是为了说明原理。事实上编译器经常会做优化，对于参数和返回值少的情况会直接将其存放在寄存器，而不需要压栈弹栈的过程，甚至都不需要调用 call，而直接做 inline操作。仅就原理来说，这5步是没有问题的）

<font size=4>**远程过程调用带来的新问题**</font>

在远程调用时，我们需要执行的函数体是在远程的机器上的，也就是说，Multiply 是在另一个进程中执行的。这就带来了几个新问题：

1. **Call ID 映射**：我们<font color=fuchsia>**怎么告诉远程机器我们要调用 Multiply，而不是 Add 或者 FooBar 呢**</font>？在 <font color=FF0000>**本地调用**中，函数体是直接通过函数指针来指定的，我们调用 Multiply，编译器就自动帮我们调用它相应的函数指针</font>。但是<font color=fuchsia>**在远程调用中，函数指针是不行的，因为两个进程的地址空间是完全不一样的**</font>。所以，<font color=fuchsia size=4>**在 RPC 中，所有的函数都必须有自己的一个 ID** ；**这个 ID 在所有进程中都是唯一确定的**</font>。<font color=fuchsia size=4>**客户端在做远程过程调用时，必须附上这个ID**</font>。然后，我们 <font color=fuchsia size=4>**还需要在客户端和服务端分别维护一个 `{ 函数 <--> Call ID }` 的对应表**</font>。两者的表不一定需要完全相同，但相同的函数对应的 Call ID 必须相同。当客户端需要进行远程调用时，它就查一下这个表，找出相应的 Call ID，然后把它传给服务端，服务端也通过查表，来确定客户端需要调用的函数，然后执行相应函数的代码。
2. **序列化和反序列化**：客户端怎么把参数值传给远程的函数呢？在本地调用中，我们只需要把参数压到栈里，然后让函数自己去栈里读就行。但是在远程过程调用时，客户端跟服务端是不同的进程，不能通过内存来传递参数。甚至有时候客户端和服务端使用的都不是同一种语言（比如服务端用 C++，客户端用 Java 或者 Python）。这时候就需要客户端把参数先转成一个字节流，传给服务端后，再把字节流转成自己能读取的格式。这个过程叫 序列化和反序列化。同理，从服务端返回的值也需要序列化反序列化的过程。
3. **网络传输**：远程调用往往用在网络上，客户端和服务端是通过网络连接的。所有的数据都需要通过网络传输，因此就需要有一个网络传输层。网络传输层需要把 Call ID 和序列化后的参数字节流传给服务端，然后再把序列化后的调用结果传回客户端。只要能完成这两者的，都可以作为传输层使用。因此，它所使用的协议其实是不限的，能完成传输就行。<font color=FF0000>尽管大部分 RPC 框架都使用 TCP 协议，但其实 UDP 也可以，而 <font size=4>**gRPC**</font> 干脆就 <font size=4>**用了 HTTP2**</font></font> 。Java 的 Netty 也属于这层的东西。

有了这三个机制，就能实现 RPC 了；具体过程如下：

```cpp
// Client端 
// int l_times_r = Call(ServerAddr, Multiply, lvalue, rvalue)
1. 将这个调用映射为Call ID。这里假设用最简单的字符串当Call ID的方法
2. 将Call ID，lvalue和rvalue序列化。可以直接将它们的值以二进制形式打包
3. 把2中得到的数据包发送给ServerAddr，这需要使用网络传输层
4. 等待服务器返回结果
5. 如果服务器调用成功，那么就将结果反序列化，并赋给l_times_r

// Server端
1. 在本地维护一个Call ID到函数指针的映射call_id_map，可以用std::map<std::string, std::function<>>
2. 等待请求
3. 得到一个请求后，将其数据包反序列化，得到Call ID
4. 通过在call_id_map中查找，得到相应的函数指针
5. 将lvalue和rvalue反序列化后，在本地调用Multiply函数，得到结果
6. 将结果序列化后通过网络返回给Client
```

所以要实现一个 RPC 框架，其实只需要按以上流程实现就基本完成了。

其中：

- Call ID 映射可以直接使用函数字符串，也可以使用整数ID。<font color=FF0000>映射表一般就是一个哈希表</font>。
- 序列化反序列化可以自己写，也可以使用 Protobuf 或者 FlatBuffers 之类的。
- 网络传输库可以自己写 socket，或者用asio，ZeroMQ，Netty 之类。

摘自：[谁能用通俗的语言解释一下什么是 RPC 框架？ - 洪春涛的回答 - 知乎](https://www.zhihu.com/question/25536695/answer/221638079)

##### RPC 和 HTTP 的关系

RPC 只是对底层协议的封装，其实对具体的通信协议是啥并没有太多要求。

实际上 application layer 是可以有不止一层的，比如说 <font color=FF0000>RPC 可以直接建立在 TCP 之上</font>（👀 比如 Socket ），<font color=FF0000>也可以建立在 HTTP 协议之上；对于 RPC 来说，这都是一样的，只要把通讯的内容塞进不同的报文理好了</font>。

其实 <font color=FF0000>HTTP 是最常用的承载 RPC 的通信协议之一</font>。而且我们可以在 HTTP 上传输 XML 和 JSON 这样的文本协议，也可以是 protobuf 和 thrift 这样的二进制协议，这都不是问题。

摘自：[既然有 HTTP 请求，为什么还要用 RPC 调用？ - 詹姆斯.通的回答 - 知乎](https://www.zhihu.com/question/41609070/answer/496122169)

**HTTP 和 RPC 不是对等的概念**：<font color=FF0000>RPC 是一个完整的 远程调用 <font size=4>**方案**</font></font> ，它 <font color=fuchsia size=4>包括了：**接口规范 + 序列化反序列化规范 + 通信协议** 等</font>。而 <font color=FF0000>**HTTP 只是一个通信协议**</font>，工作在 OSI 的第七层，**不是一个完整的远程调用方案**。

所以，应该拉平为一个对等的概念。例如，HTTP + Restful规范 + 序列化与反序列化，构成一个完整的远程调用方案，再和 RPC 进行比较。而单纯的 HTTP，只是一个通信协议，自然无法和 RPC 比较（👀 注： HTTP 要加上 接口规范、序列化反序列化等 的远程调用方案  才能和 采用 RPC 的远程调用方案  对等比较 / 有比较的意义）。

这就像是牛 ( HTTP ) 不能和马车 ( RPC ) 比较。要想比较，就应该将牛补齐为牛车，然后和马车比较。

> 👀 后面是  **HTTP 加上接口规范、序列化反序列化等 的远程调用方案** （比如 HTTP + Restful + JSON）和 **采用 RPC 的远程调用方案** 进行比较的内容，由于暂时用不到，略。

摘自：[既然有 HTTP 请求，为什么还要用 RPC 调用？ - 易哥的回答 - 知乎](https://www.zhihu.com/question/41609070/answer/1030913797)

<font color=FF0000>RPC 在1984 年就被人用来做 **分布式系统的通信**</font> ，Java 在 1.1版本提供了 Java 版本的 RPC 框架 ( RMI ) ，而 <font color=FF0000>HTTP 协议在 1990 年才开始作为主流协议出现，而且 HTTP 发明的场景是用于 web 架构，而不是分布式系统间通信</font>；这导致了在很长一段时间内，HTTP 都是浏览器程序和后端 web 系统通信用的东西，上面的文档格式都是 HTML（非常啰嗦），没有人会把 HTTP 作为分布式系统通信的协议。

在很长一段时间内，RPC 才是正统。随着前端技术的发展，AJAX 技术 和 JSON文档 在前端界逐渐成为主流，HTTP调用 才摆脱 HTML，开始使用 JSON 这一相对简洁的文档格式，为后面用于系统间调用定下基础。最后随着 RESTFUL 思潮的兴起，越来越多系统考虑用 HTTP 来提供服务，但这时候，RPC 已经是各种大型分布式调用的标配了。

摘自：[既然有 HTTP 请求，为什么还要用 RPC 调用？ - 周迪超的回答 - 知乎](https://www.zhihu.com/question/41609070/answer/1040163258)

**<font color=FF0000>按照调用方式</font>来看，RPC 有四种模式：**

- RR ( Request-Response ) 模式，又叫请求响应模式，指每个调用都要有具体的返回结果信息。
- Oneway 模式，又叫单向调用模式，调用即返回，没有响应的信息。
- Future 模式，又叫异步模式，返回拿到一个 Future 对象，然后执行完获取到返回结果信息。
- Callback 模式，又叫回调模式，处理完请求以后，将处理结果信息作为参数传递给回调函数进行处理。

这四种调用模式中，前两种最常见，后两种一般是 RR 和 Oneway 方式的包装，所以从本质上看：<font color=FF0000>RPC 一般对于客户端的来说是一种同步的远程服务调用技术</font>；与其相对应的，一般来说 <font color=FF0000>MQ 恰恰是一种异步的调用技术</font>。

摘自：[任务队列和消息队列，rpc的区别？ - kimmking的回答 - 知乎](https://www.zhihu.com/question/265988880/answer/747819369)



#### 消息队列 MQ

在计算机科学中，消息队列 ( Message queue ) 是一种 <font color=FF0000>**进程间通信** 或 **同一进程的不同线程间** 的 <font color=dodgerBlue>**通信方式**</font></font>，软件的队列用来处理一系列的输入，通常是来自用户。<font color=FF0000>消息队列提供了 <font size=4>**异步的通信协议**</font></font>，每一个队列中的纪录包含详细说明的资料，包含发生的时间，输入设备的种类，以及特定的输入参数，也就是说：<font color=FF0000>消息的发送者和接收者不需要同时与消息队列交互</font>（👀 “不需要同时” 说明了这是异步的）。<font color=fuchsia>**消息会保存在队列中，直到接收者取回它**</font>。

一个 WIMP ( 👀 Window, Icon, Menu, Pointer ) 环境像是 Microsoft Windows，借由优先的某些形式（通常是事件的时间或是重要性的顺序）来存储用户产生的事件到一个 事件队列 中。系统把每个事件从事件队列中传递给目标的应用程序。

<font color=FF0000>**消息队列常常保存在 链表结构 中。拥有权限的进程可以向消息队列中写入或读取消息**</font>。

消息队列本身是异步的，<font color=FF0000>它允许接收者在消息发送很长时间后再取回消息，**这和大多数通信协议是不同的**</font>。例如 WWW 中使用的 <font color=LightSeaGreen>HTTP 协议（ HTTP/2 之前）是同步的，因为客户端在发出请求后必须等待服务器回应</font>。然而，很多情况下我们需要异步的通信协议。比如，一个进程通知另一个进程发生了一个事件，但不需要等待回应。但<font color=FF0000>消息队列的异步特点，**也造成了一个缺点**，就是 **接收者必须轮询消息队列，才能收到最近的消息**</font>。

<font color=FF0000>**和信号 ( signal ) 相比，消息队列能够传递更多的信息**</font>。<font color=fuchsia>**与管道 ( pipeline ) 相比，消息队列提供了有格式的数据**</font>，这可以减少开发人员的工作量。但<font color=FF0000>**消息队列仍然有大小限制**</font>。

消息队列除了可以当不同线程或进程间的缓冲外，更可以透过消息队列当前消息数量来侦测接收线程或进程性能是否有问题。

摘自：[wikipedia - 消息队列](https://zh.wikipedia.org/zh-cn/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97)

##### 知乎问题 “任务队列和消息队列，rpc 的区别？” 的讨论

RPC 是调用模式，MQ 是实现模式。

RPC 指的是远程调用，也就是说，调用的函数不在同一内存空间内。这里强调的是远程，具体怎么实现的远程，并不强求。很多时候，RPC 喜欢把自己包装成本地函数，让调用尽可能的方便，早期的实现，经常把它做成同步的。

<font color=FF0000>MQ 是 **网络中的 <font size=4>通用队列</font>**</font>，<font color=fuchsia>它 <font size=4>**不在乎存储的数据格式，可以是数据，可以是任务**</font>，它只是提供了一个存放的场所，规定了存取的方式</font>。<font color=FF0000>可以用它 发布消息，分发任务，**也可做 RPC 实现**。它是 **通用**，**异步**，和 **解耦** 的</font>。

**二者侧重点也不一样**：<font color=red>**RPC 侧重两端，MQ 侧重中间**，算是不同层面的东西</font>。

摘自：[任务队列和消息队列，rpc的区别？ - ze ran的回答 - 知乎](https://www.zhihu.com/question/265988880/answer/301986369)

消息队列 ( MQ ) 是一种能实现 <font color=FF0000>生产者 到 消费者 **单向通信**</font> 的通信模型（👀 半双工？），一般来说是指实现这个模型的中间件，比如 RabbitMQ

摘自：[任务队列和消息队列，rpc的区别？ - 灵剑的回答 - 知乎](https://www.zhihu.com/question/265988880/answer/301580895)

异步的远程调用，如果能同时存在很多个请求，该如何处理呢？进一步地，由于不能立即拿到处理结果，假若需要考虑失败策略，重试次数等，应该怎么设计呢？
如果有 N 个不同系统相互之间都有 RPC 调用，这时候整个系统环境就是一个很大的网状结构，依赖关系有 N*(N-1)/2 个（👀 **注**：有点类似于 网络中的 “网状拓扑”）。任何一个系统出问题，都会影响剩下 N-1个系统，怎么降低这种耦合呢？如下图：

<img src="https://s2.loli.net/2022/07/13/L5b3PVNAG8iu7f4.jpg" style="zoom:80%;" />

基于这些问题，我们发展出来了消息队列 ( MQ ) 技术，<font color=FF0000>所有的处理请求先作为一个消息发送到 MQ（一般我们叫做 broker ），接着处理消息的系统从 MQ 拿到消息并进行处理</font>（👀 有点类似于 网络中的 “星型拓扑”）。这样就实现了各个系统间的解耦，同时可以把失败策略、重试等作为一个机制，对各个应用透明，直接在 MQ 与各调用方的应用接口层面实现即可。如下图：

<img src="https://s2.loli.net/2022/07/13/urjEPvw39TVF5fD.jpg" alt="img" style="zoom:80%;" />

一般来说，我们把发送消息的系统称为消息生产者 ( message producer )，接受处理消息的系统称为消息消费者 ( message consumer)。

**根据消息处理的特点，我们又可以总结两种消息模式：**

- **点对点模式** ( Point to Point / PTP ) ：一个生产者发送的每一个消息，都只能有一个消费者能消费，看起来消息就像从一个点传递到了另外一个点。
- **发布订阅模式** ( Publish-Subscribe / PubSub ) ：一个生产者发送的每一个消息，都会发送到所有订阅了此队列的消费者，这样对这个消息感兴趣的系统都可以拿到这个消息。

通过这两种消息模式的灵活应用以及功能扩展，我们可以实现各种具体的消息应用场景，比如高并发下的订单异步处理，海量日志数据的分析处理等等。

摘自：[任务队列和消息队列，rpc的区别？ - kimmking的回答 - 知乎](https://www.zhihu.com/question/265988880/answer/747819369)

**消息队列具有四大好处：**

- **解耦**：每个成员不必受其他成员影响，可以更独立自主，只通过一个简单的容器来联系
- **提速**：只发送消息，其他不问
- **广播**：只需发送一次，有多个接收人
- **削峰**：发送消息频率不固定，不太可能出现密集发送的情况

**有如下成本（特性）：**

- 使用 消息队列 会带来成本，除了性能外，还有安全成本
- 生产者不需要从消费者处获得反馈
- 发送方/接收方 的数据会存在暂时的不一致性

学习自：[消息队列的使用场景是怎样的？ - 祁达方的回答 - 知乎](https://www.zhihu.com/question/34243607/answer/140732170)



#### 并发编程

##### 并发编程一些实践方案

- 加锁

- 通信优于同步 源于 CSP ( Communicating Sequential Processes )

  Golang 语言内的 Channel 是这一思想的典型体现

- Actor

- 无锁编程

- 函数式编程：利用了 FP 数据不可变性特性

  > 👀 难怪《黑客与画家》作者在书中提了一堆 LISP 实现 Web应用的优点...

学习自：[计算机核心思想系列【2】| 高性能程序的关键：并发](https://mp.weixin.qq.com/s/PywoV8cFzNmqCPpT8hFAng) 及其视频 [计算机核心思想系列【2】| 高性能&高复杂度程序的关键：并发](https://mp.weixin.qq.com/s/PywoV8cFzNmqCPpT8hFAng)



#### Pattern Matching

<img src="https://s2.loli.net/2024/04/09/5QltTeEnd2CSrx4.png" alt="image-20240409235143301" style="zoom:50%;" />

// TODO





#### Marshal

##### 背景

在看文章 [在业务系统中寻找技术含量](https://xargin.com/seeking-for-high-tech-in-business-system-dev/) 时看到这样一句话：

> 如果嫌 nginx 那样的配置格式麻烦，用 json 可以表达任意形式的数据结构、逻辑流程、映射方式。这里你可以有些疑惑，我们可以认为：
>
> - 代码和 AST 之间是可以互相转换的
> - AST 可以被 *Marshal 为 json*
> - 所以 json 从原理上可以表达一切程序逻辑

除了对这句话感觉颇有收获外，也对 Marshal 是什么有点好奇。

##### 是什么

Google 了下 “Marshal json” ，发现主要和 Go 相关，主要是因为 Go 的 package [json](https://pkg.go.dev/encoding/json) 中包含 [`Marshal`](https://pkg.go.dev/encoding/json#Marshal) 和 [`Unmarshal`](https://pkg.go.dev/encoding/json#Unmarshal) 函数。

另外，问了下 ChatGPT，得到如下信息：

<img src="https://s2.loli.net/2023/05/30/cPeSr4Z8RXy1QoU.png" alt="image-20230530162605279" style="zoom:46%;" />

<img src="https://s2.loli.net/2023/05/30/IHVOMNmWLeRG6jP.png" alt="image-20230530170550942" style="zoom:46%;" />

总结：Marshal 是比 Serialization 更宽泛的概念。



#### 自旋锁

自旋锁是计算机科学用于 **多线程同步** 的一种锁，<font color=FF0000>**线程反复检查锁变量是否可用**</font>。由于线程在这一过程中保持执行，因此是一种忙等待。一旦获取了自旋锁，线程会一直保持该锁，直至显式释放自旋锁。

自旋锁避免了进程上下文的调度开销，因此对于线程只会阻塞很短时间的场合是有效的。因此操作系统的实现在很多地方往往用自旋锁。Windows 操作系统提供的 轻型[读写锁](https://zh.m.wikipedia.org/wiki/读写锁) ( SRW Lock ) 内部就用了自旋锁。显然，<font color=FF0000>**单核 CPU 不适于使用自旋锁**，这里的单核 CPU 指的是 **单核单线程** 的 CPU</font>，因为，在同一时间只有一个线程是处在运行状态，假设运行 线程A 发现无法获取锁，只能等待解锁，但因为 A 自身不挂起，所以那个持有锁的 线程B 没有办法进入运行状态，只能等到操作系统分给A的时间片用完，才能有机会被调度。这种情况下使用自旋锁的代价很高。

获取、释放自旋锁，实际上是读写自旋锁的存储内存或寄存器。因此这种读写操作必须是[原子的](https://zh.m.wikipedia.org/wiki/原子操作)。通常用 [test-and-set](https://zh.m.wikipedia.org/wiki/Test-and-set) 等原子操作来实现

摘自：[wikipedia - 自旋锁](https://zh.wikipedia.org/zh-sg/%E8%87%AA%E6%97%8B%E9%94%81)



#### 竞态

<img src="https://s2.loli.net/2024/04/05/G6BjliXquYzIcVF.png" alt="image-20240405142127407" style="zoom:50%;" />

> 👀 个人感觉：竞态主要和 OS 以及 分布式 相关。另外，在《Designing Data-Intensive Application》中搜索 “race conditions” 可以搜索到相当多的关联项，有空可以看下这本名著

// TODO





#### 规格继承和实现继承

// TODO



#### 常见配置文件语言

- INI
- XML
- JSON
- YAML

具体的可以阅读：[常见配置文件语言: INI, XML, JSON与YAML]([https://dhpo.github.io/2018/02/03/%E5%B8%B8%E8%A7%81%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6-XML-JSON-INI%E4%B8%8EYAML/](https://dhpo.github.io/2018/02/03/常见配置文件-XML-JSON-INI与YAML/))



#### DSL ( Domain-Specific Language )

//TODO



#### 方法链

//todo



#### 回调函数

// TODO



#### 命名规则

##### 驼峰式(Camel Case)

- 小驼峰（lower camel case）：第一个单词首字母小写，其余单词首字母大写

  示例：firstName、userPhone

- 大驼峰（upper camel case, Pascal Case）：单词首字母都大写

  示例：FirstName、UserPhone

##### Snake Case

单词之间使用下划线 `_` 连接，示例：first_name、user_phone

##### kebab Case ( spinal-case, Train-Case, Lisp-case )

单词之间使用下划线 `-` 连接，示例：first-name、user-phone



#### 计算机名词的区别

##### argument 和 parament 的区别

- argument：实参
- parament：形参

##### 函数和方法的区别

- 函数（function）是指一段可以直接被其名称调用的代码块，它可以传入一些参数进行处理并返回一些数据，所有传入函数的数据都是被明确定义。
- 方法指的是一段<font color=FF0000>被它关联的对象</font>通过它的名字调用的代码块。

> 💡 总结：方法就是面向对象版的函数

摘自：[方法和函数的区别](https://blog.csdn.net/notsaltedfish/article/details/75174556)

##### class: Helper VS Utility

There are many naming styles to use. I would suggest <font color=FF0000>**Utils** just because its **more common**</font>.

A **Utility** class is understood to <font color=FF0000>**only have static methods and be stateless**</font>. You <font color=FF0000>would not create an instance of such a class</font>.

A **Helper** <font color=FF0000>can be a **utility** class</font> or <font color=FF0000>it can be **stateful** or **require an instance be created**</font>. I would avoid this if possible.

摘自：[What are the differences between Helper and Utility classes?](https://stackoverflow.com/questions/12192050/what-are-the-differences-between-helper-and-utility-classes)

##### expression VS statement

// TODO 虽然这比较明显，不过还是建议做下笔记 https://stackoverflow.com/questions/4728073/what-is-the-difference-between-an-expression-and-a-statement-in-python

##### status vs state

state 一般表示可以互相转换的状态，status 一般表示不会互相转换的状态

学习自：https://x.com/changwei1006/status/1885987370958495869 。另外，知乎问题 [程序代码中，怎么区分status和state？ - 知乎](https://www.zhihu.com/question/21994784) 可作参考



#### let 关键字

##### 背景

在了解 Chris Lattner 创业推出的 mojo 语言时（[如何看待 Modular 提出的 mojo 语言？ - Nugine的回答 - 知乎](https://www.zhihu.com/question/598832787/answer/3013077756)），看到这样一句话：

> `let` 声明不可变变量，`var` 声明可变变量

这引起了我的好奇，毕竟这和 JS 不太一样。便去问了下 ChatGPT。

> 💡 不过， Rust 的变量也是默认不可变，即：
>
> ```rust
> let x = 5 // 不可变的变量
> x = 6 // 错误: cannot assign twice to immutable variable `x`
> 
> let mut y = 6
> y = 7 // 正常
> ```
>
> 所以，mojo 这样的设计没毛病？

##### 询问 ChatGPT 的结果

<img src="https://s2.loli.net/2023/05/06/eaHYR4xo6G7P23U.png" alt="image-20230506003753515" style="zoom:47%;" />

> 👀 上面的 JS 中 `let` “被用作声明一个不可变变量” 显然是错的

<img src="https://s2.loli.net/2023/05/06/s8BeMURytcuLdbV.png" alt="image-20230506003907981" style="zoom:47%;" />



#### RAII

##### 概念

**RAII：资源获取即初始化 (Resource Acquisition Is Initialization)**

RAII 是一种在面向对象编程（尤其是在 C++ 中）广泛使用的编程范型和资源管理技术。它的<font color=red>核心思想是将 **资源的生命周期 **与 **对象的生命周期** 绑定起来</font>。

###### 工作原理

1. **资源获取 (Acquisition)** ：<font color=red>在对象的**构造函数 (Constructor)** 中获取资源</font>（例如分配内存、打开文件、锁定互斥量、建立网络连接等）。当对象成功创建时，意味着资源也已经成功获取。
2. **资源释放 (Release)** ：<font color=red>在对象的**析构函数 (Destructor)** 中释放资源</font>（例如释放内存、关闭文件、解锁互斥量、关闭网络连接等）。对象的析构函数在对象生命周期结束时（例如对象离开其作用域或被 `delete` 时）会被**自动调用**。

###### 为什么需要 RAII？解决了什么问题？

<font color=dodgerBlue>在没有 RAII 的情况下，程序员**需要手动管理资源**</font>：

- 申请资源后，必须记得在所有可能的代码路径（包括正常执行路径和异常处理路径）中释放它。
- 这很容易出错，导致 **资源泄漏**（例如内存泄漏、文件句柄未关闭）或**死锁**（忘记释放锁）。
- 当代码中存在复杂的条件分支或异常处理时，手动确保资源总能被正确释放变得非常困难和繁琐。

RAII 通过将资源释放逻辑封装在对象的析构函数中，利用语言本身的机制（对象生命周期管理和析构函数的自动调用）来**自动化**资源管理，从而解决了这些问题。

###### RAII 的主要优点

1. **自动化资源管理** ：<font color=red>极大地减少了资源泄漏的可能性</font>，因为资源的释放是自动发生的，程序员不需要显式编写释放代码。
2. **异常安全 (Exception Safety)** ：<font color=dodgerBlue>这是 RAII 最重要的优点之一</font>。<font color=dodgerBlue>即使在资源获取和释放之间发生了异常，导致程序控制流跳转</font>，<font color=red>**对象的析构函数仍然会被（在栈回溯过程中）自动调用，从而保证资源能够被正确释放**</font>。这大大简化了编写健壮、异常安全的代码。
3. **代码简洁清晰** ：将资源管理的逻辑（获取和释放）封装在专门的类中，使得使用资源的代码更加简洁，专注于业务逻辑，提高了代码的可读性和可维护性。
4. **封装性** ：资源及其管理逻辑被封装在一个类中，符合面向对象的设计原则。

###### 典型示例 (C++)

C++ 标准库大量应用了 RAII 思想：

- **智能指针 (Smart Pointers)** ：`std::unique_ptr`, `std::shared_ptr`。它们在构造时获取指向动态分配内存的所有权，并在析构时自动释放内存。

  ```cpp
  #include <memory>
  #include <iostream>
  
  void process_data() {
      std::unique_ptr<int> ptr(new int(10)); // 获取资源 (动态分配内存)
      std::cout << "Value: " << *ptr << std::endl;
      // 当 ptr 离开作用域时，其析构函数会自动调用 delete ptr; 释放内存
      // 即使在处理过程中发生异常，内存也会被释放
  }
  ```

- **文件流 (File Streams)** ：`std::fstream`, `std::ifstream`, `std::ofstream`。它们在构造时打开文件，在析构时自动关闭文件。

  ```cpp
  #include <fstream>
  #include <string>
  #include <iostream>
  
  void write_to_file(const std::string& filename, const std::string& data) {
      std::ofstream file(filename); // 获取资源 (打开文件)
      if (file.is_open()) {
          file << data;
          // 当 file 对象离开作用域时，其析构函数会自动关闭文件
      } else {
          std::cerr << "Failed to open file: " << filename << std::endl;
      }
  }
  ```

- **锁守卫 (Lock Guards)** ： `std::lock_guard`, `std::unique_lock`。它们在构造时锁定互斥量 (mutex)，在析构时自动解锁互斥量。

  ```cpp
  #include <mutex>
  #include <thread>
  
  std::mutex mtx;
  
  void critical_section() {
      std::lock_guard<std::mutex> lock(mtx); // 获取资源 (锁定互斥量)
      // ... 访问共享资源 ...
      // 当 lock 对象离开作用域时，其析构函数会自动解锁互斥量
  }
  ```

###### 其他语言中的类似概念

虽然 RAII 这个术语主要与 C++ 相关，但其他语言也有类似的机制来确保资源的确定性释放：

- **Python** ： `with` 语句和上下文管理器协议 (`__enter__`, `__exit__`)。
- **C#** ： `using` 语句和 `IDisposable` 接口。
- **Java** ： `try-with-resources` 语句和 `AutoCloseable` 接口。
- **Rust** ：所有权系统和 `Drop` trait。

这些机制虽然实现细节不同，但目标都是一致的：将资源的生命周期与某个作用域或对象的生命周期绑定，以实现自动、可靠的资源释放。

###### 总结

RAII 是一种强大而优雅的资源管理技术，通过将资源的生命周期与对象的生命周期相关联，利用构造函数获取资源、析构函数释放资源的方式，实现了资源的自动化管理和异常安全，是编写健壮、可靠的 C++ 代码（以及利用类似机制的其他语言代码）的关键实践之一。

摘自：https://g.co/gemini/share/c6eba16129b5



#### 卫语句

> 👀 想把这个词记录到笔记是看到了文章 [Return 的高级用法，你真的掌握了吗？（Python 篇）](https://zhuanlan.zhihu.com/p/1928881704648569563) 中有提及：
>
> > Python 推崇 “尽早失败” ( fail fast ) 的编程思维。当你处理异常、特殊参数或边界条件时，建议先用 return 早早结束函数执行，而不是一层层 if 嵌套。
> >
> > 不推荐写法（略）
> >
> > 推荐写法（卫语句模式）：
> >
> > ```python
> > def process_order(user, product):
> >     if not user:
> >         return "用户未登录"  # 卫语句1，立即退出无效路径
> >     if not user.is_vip:
> >         return "非VIP用户"  # 卫语句2
> >     if product.stock <= 0:
> >         return "库存不足"    # 卫语句3
> >     if user.balance < product.price:
> >         return "余额不足"    # 卫语句4
> >     
> >     return "下单成功"  # 主逻辑清晰展现在最后
> > ```
> >
> > **这是一种卫语句模式 ( Guard Clause )**，它**就像门口的保安：**当函数开始执行时，卫语句会**先检查所有可能出错的条件**；一旦发现危险情况（如无效参数、权限不足），**立即用 return 终止流程**；只有通过所有检查的“安全请求”才能进入主逻辑**。**
> >
> > **那么卫语句模式的作用是什么呢？**
> >
> > 它将边界判断集中放在函数开头，主逻辑自然展开。在 Django /。Flask 中的视图函数、表单验证、接口返回判断中。**这种写法能显著提升代码可读性。**
>
> 不过，该名词在之前也有多次听过，不过只是有些耳熟而已... 有点忘了

// TODO



#### 鸭子类型

鸭子类型(duck typing)<font color=red>在程序设计中是**动态类型**的一种风格</font>。<font color=fuchsia>在这种风格中，**一个对象有效的语义，不是由继承自特定的类或实现特定的接口，而是由“当前方法和属性的集合”决定**</font>。这个概念的名字来源于由詹姆斯·惠特科姆·莱利提出的<font color=red>**鸭子测试**</font>，<font color=dodgerBlue>“鸭子测试”可以这样表述</font>：

> “当看到一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子，那么这只鸟就可以被称为鸭子。”

<font color=fuchsia>**在鸭子类型中，关注点在于对象的行为，能做什么；而不是关注对象所属的类型**</font>。例如，在不使用鸭子类型的语言中，我们可以编写一个函数，它接受一个类型为“鸭子”的对象，并调用它的“走”和“叫”方法。<font color=red>在使用鸭子类型的语言中，这样的一个函数</font> <font color=fuchsia>**可以接受一个任意类型的对象**</font>，<font color=red>并调用它的“走”和“叫”方法</font>。<font color=fuchsia>**如果这些需要被调用的方法不存在，那么将引发一个运行时错误**</font>。任何拥有这样的正确的“走”和“叫”方法的对象都可被函数接受的这种行为引出了以上表述，这种决定类型的方式因此得名。

<font color=red>鸭子类型通常得益于 “不” 测试方法和函数中参数的类型，而是依赖文档、清晰的代码和测试来确保正确使用</font>。

在常规类型中，我们能否在一个特定场景中使用某个对象取决于这个对象的类型，而在鸭子类型中，则取决于这个对象是否具有某种属性或者方法——即<font color=fuchsia>只要具备特定的属性或方法，能通过鸭子测试，就可以使用</font>。

<font color=fuchsia>鸭子类型在 **不使用继承 的情况下使用了 多态**</font>

##### 批评

关于鸭子类型常常被引用的一个批评是它要求程序员在任何时候都必须很好地理解他/她正在编写的代码。在一个强静态类型的、使用了类型继承树和参数类型检查的语言中，给一个类提供未预测的对象类型更为困难。例如，在Python 中，你可以创建一个称为 `Wine` 的类，并在其中需要实现 `press` 方法。然而，一个称为 `Trousers` 的类可能也实现 `press()` 方法。为了避免奇怪的、难以检测的错误，开发者在使用鸭子类型时需要意识到每一个`press` 方法的可能使用，即使在语义上和他/她所正在编写工作的代码没有任何关系。

本质上，问题是：“如果它走起来像鸭子并且叫起来像鸭子”，它也可以是一只正在模仿鸭子的龙。尽管它们可以模仿鸭子，但也许你不总是想让龙进入池塘。

摘自：[wikipedia - 鸭子类型](https://zh.wikipedia.org/wiki/%E9%B8%AD%E5%AD%90%E7%B1%BB%E5%9E%8B)



#### 计算机名词简易解释

###### TICK（滴答）

是计算机的计时器的单位

**脚手架：**脚手架就是别人用构建工具帮你搭好了原始项目，你可以在不懂构建工具的情况下进行前端开发。不过这就是初级前端的基本工作，给我一个环境，让我安心的写业务代码。学习自：[一小时的时间，上手 Webpack](https://zhuanlan.zhihu.com/p/114286243)



#### 编程命名

##### 命名词汇 Container 和 Wrapper 的区别

In programming languages the word <font color=FF0000>**container**</font> is generally used for structures that can <font color=FF0000>contain more than one element</font>.

A <font color=FF0000>**wrapper**</font> instead is something that <font color=FF0000>wraps around a single object</font> to provide more functionalities and interfaces to it.

摘自：[stack overflow - CSS Language Speak: Container vs Wrapper?](https://stackoverflow.com/questions/4059163/css-language-speak-container-vs-wrapper)



#### 重写方法和重载方法

##### 重写 (Override)

是子类对父类的允许访问的方法的实现过程进行重新编写, 返回值和形参都不能改变。即外壳不变，核心重写！

##### 重载 ( Overload )

是在一个类里面，方法名字相同，而参数不同。返回类型可以相同也可以不同。



#### 一等公民

第一类物件（英语：First-class object）在电脑科学中指可以在执行期创造并作为参数传递给其他函数或存入一个变数的实体。将一个实体变为第一类物件的过程叫做“物件化” ( Reification )。

<font color=LightSeaGreen>“第一类物件”这一名称最早由克里斯托弗·斯特雷奇在1960年代发明，原称“第一类公民”（First-class citizen），意指函数可作为电脑语言中的第一类公民</font>。英文中也称 “First-class entity” 或 “First-class value”。

第一类物件不一定是物件导向程式设计所指的物件，而可以指任何程式中的实体。<font color=FF0000> **一般第一类物件所特有的特性为**</font>：

- 可以被存入变数或其他结构
- 可以被作为参数传递给其他函数
- 可以被作为函数的返回值
- <font color=FF0000>**可以在执行期创造，而无需完全在设计期全部写出**</font>
- <font color=FF0000>即使没有被系结至某一名称，也可以存在</font>

<font color=LightSeaGreen>绝大多数语言中，数值与基础型别都是第一类物件</font>，然而不同语言中对函数的区别很大，例如C语言与C++中的函数不是第一类物件，因为在这些语言中函数不能在执行期创造，而必须在设计时全部写好。相比之下，<font color=FF0000> Scheme中的函数是第一类物件，因为可以用lambda表达式来创造匿名函数并作为第一类物件来操作</font>。

摘自：[wiki - 第一类对象](https://zh.wikipedia.org/wiki/%E7%AC%AC%E4%B8%80%E9%A1%9E%E7%89%A9%E4%BB%B6)

一等公民表示：用户<font color=FF0000 size=4> **可以通过这个元素（被称为一等公民的元素）来完成该语言当中的所有其他元素能进行的所有操作**</font>。

**在 SICP 中也有提到 *一等公民* ：**

- <font color=FF0000> 可以用变量命名</font>。即：函数表达式

- <font color=FF0000> 可以作为参数传递给过程</font>。即：高阶函数

  > 💡 关于 “高阶函数” 的补充
  >
  > A higher-order function is a function that **takes one or more functions as arguments**, <font color=red>returns a function as its result</font>, or both.
  >
  > 摘自：[What Is Functional Programming?](https://www.sitepoint.com/what-is-functional-programming/)

- <font color=FF0000> 可以作为程序的结果返回</font>。即：闭包

- <font color=FF0000> 可以包含在数据结构中</font>。即：嵌套声明，可以放在对象和数组中

> 原文：
>
> In general, programming languages impose restrictions on the ways in which computational elements can be manipulated. Elements with the fewest restrictions are said to have first-class status. Some of the “rights and privileges” of first-class elements are:
>
> - They may be named by variables. 
> - They may be passed as arguments to procedures. 
> - They may be returned as the results of procedures. 
> - They may be included in data structures

另外，<font color=FF0000> **声明提升 也是由于一等公民产生的**</font>

**二等公民：**只能作为参数传递，但不能返回，也不能赋值给变量

**三等公民：**作为参数传递都不可以

<font color=FF0000> **JS 中没有实现 二等公民、三等公民 这些概念**</font>

摘自：[JavaScript中的函数是一等公民，是什么意思？](https://www.bilibili.com/video/BV1RD4y1X7qv)

其他参考内容：[如何理解在 JavaScript 中 "函数是第一等公民" 这句话? - justjavac的回答 - 知乎](https://www.zhihu.com/question/67652709/answer/255460149)



#### shim 和 ployfill 的 区别

- A **shim** is any piece of code that performs interception of an API call and provides a layer of abstraction. <font color=FF0000>It isn't necessarily restricted to a web application or HTML5/CSS3</font>.
- A **polyfill** <font color=FF0000 size=4>is a *type of shim*</font> that retrofits legacy browsers with modern HTML5/CSS3 features <font color=FF0000>usually using Javascript or Flash</font>.

摘自：[What is the difference between a shim and a polyfill?](https://stackoverflow.com/questions/6599815/what-is-the-difference-between-a-shim-and-a-polyfill)

其他参考： [zhihu - shim和polyfill有什么区别?](https://www.zhihu.com/question/22129715)、[程序员黑话辞典之 Polyfill/Shim | 程序员黄玄 Vol.005](https://www.bilibili.com/video/BV12q4y1E7Gg)



#### 字节码和机器码

##### 机器码

机器码 ( machine code )，学名 “机器语言指令” ，有时也被称为 “原生码”  ( Native Code )，是 <font color=red>CPU 可直接解读的数据</font>（👀 CPU 直接面对和执行的数据）；运行速度也是最快的代码。

##### 字节码

字节码 ( byte code ) 是一种包含执行程序、由一序列 “OP代码（操作码）/ 数据对” 组成的二进制文件。 <font color=red>字节码是一种 “中间码” ，它比机器码更抽象，需要 “直译器转译” 后才能成为机器码的中间代码</font>。

<font color=red>通常情况下它是已经经过编译，但与特定机器码无关</font>。字节码通常不像源码一样可以让人阅读，而是编码后的数值常量、引用、指令等构成的序列。

<font color=red>字节码主要为了实现特定软件运行和软件环境、与硬件环境无关</font>。字节码的实现方式是通过编译器和虚拟机器。编译器将源码编译成字节码，特定平台上的虚拟机器将字节码转译为可以直接执行的指令。

<font color=red>字节码的典型应用为 Java bytecode</font> ，<font color=LightSeaGreen>字节码在运行时通过 JVM 做一次转换生成机器指令，因此能够更好的跨平台运行</font>。

摘自：[机器码(machine code)和字节码(byte code)是什么？](https://juejin.cn/post/6844903943714111501)

##### 在 V8 中的使用

V8 引擎诞生的使命就是要在速度和内存回收上进行革命。JavaScriptCore 的架构是采用生成字节码的方式，然后执行字节码。<font color=LightSeaGreen>Google 觉得 JavaScriptCore 这套架构不行，生成字节码会浪费时间，不如直接生成机器码快</font>。所以 V8 在前期的架构设计上是非常激进的，采用了直接编译成机器码的方式。后期的实践证明 Google 的这套架构速度是有改善，但是同时也造成了「内存消耗问题」

摘自：[字节码和机器码的区别](https://blog.51cto.com/zhangchiworkos/4941089)

> 💡 补充
>
> > 影响 Bun 性能的第二个因素是使用了 Apple 的 JavaScriptCore 引擎。事实证明，JavaScriptCore 比 V8 更快。差异相对较小，但绝对存在。
> > 摘自：[TS、Vue、React、SSR、Node、Deno、Bun：回顾2022，展望2023](https://mp.weixin.qq.com/s/wC9Wq7FJ_sKg1I-z8AozCQ)
>
> 👀 似乎也验证了 wwdc 上说 Safari 是最快的。

#### 闭包 ( Closure ) 

> **// TODO**
>
> 下面的链接包含大量的知识（不仅仅是 js 中的闭包，凡是实现 FP 的语言应该都有闭包），建议阅读与记录

摘自：[设计闭包（Closure）的初衷是为了解决什么问题？ - 知乎](https://www.zhihu.com/question/51402215)



#### Side Effect 副作用

##### 什么是副作用

<font color=red>**副作用 ( side-effect ) 是指让一个函数变得不再纯净 ( pure ) 的东西**</font>。

<font color=fuchsia>**一个纯净的函数，无论何时何地 ( any time any wherer ) 执行，都会得到稳定的结果**</font>（👀 即，是 “幂等” 的），这对保障程序的稳定性和性能都有极大的帮助。<font color=FF0000>反过来：如果一个函数不能 any time any wherer 得到稳定的结果，那这个函数就不是纯净的，就是有副作用了</font>。

<font color=dodgerBlue>常见副作用包括</font>：<font color=red>**对外部可变数据或变量的修改，外部接口的调用尤其是IO ，异常的抛出**</font>。因为<font color=FF0000>它们都会让函数的执行不再稳定，要么会导致函数输出变化，要么导致函数报错</font>。 <font color=dodgerBlue>举一些例子</font>：

- **对外部可变数据或变量的修改** ：全局变量 / 闭包变量 / dom对象 / bom对象 的读写操作

- **外部接口的调用尤其是 IO** ：dom对象 / bom对象的方法调用，xhr / fetch这样的网络IO，console / LocalStorage 这样的 磁盘IO

  > 👀 关于 console 为什么是不纯的，可以参考下 [WeakSet的垃圾回收问题【渡一教育】](https://www.bilibili.com/video/BV1Ux4y147nx) ，从 GC 的角度说明了 console 为什么会带来异常：DevTool 的 Console 存在引用，会影响回收

- **异常的抛出** ：函数中的某些代码可能会抛出异常或者执行出错

通俗的来讲就是：不能相信除了自己以外的任何人。

> 💡 <font color=red>实际上，在 FP 中 function 指的是纯净的函数，对于有副作用的函数称之为 procedure</font>。👀 关于 function 和 producer 参见 [[#routine 例程]]

##### 副作用是有害的

副作用让我们的程序变得不稳定，举个例子：

```js
window.a = 0
function doWork() {
	return window.a + 1
}
```

这个 `doWork` 就是一个副作用函数的代表，它依赖了外部的可变数据变得不再稳定；因为谁也不知道什么时候会在另外的地方对 `window.a` 做了修改。

在我们的业务代码中有很多这样的“稳定”的函数，我们总是“愿意相信”它们不会出问题，然而这样的函数事实上就是不稳定的；因为它依赖了外部的可变数据。其实一些 lib 比如 webpack 要比我们理性的多，它们会不带任何主观因素的把这样的函数视为带有副作用的函数

<font color=dodgerBlue>我们经常有一个误解是：**我们总认为我们以为的稳定的函数是纯净的**，实际上并非如此</font>（💡 下面的组合可以总结为：“纯洁” 和 “函数稳定” 是充分不必要的）：

- 纯 -> 稳定 ✅
- 不纯 -> 不稳定 ✅
- 稳定 -> 纯 ❌
- 不稳定 -> 不纯 ✅

<font color=dodgerBlue>稳定不一定是纯的，但是不纯一定是不稳定的</font>。<font color=red>因此，判定我们的前端程序是否真的稳定，它的依据不是表面上是否稳定，而是内在是否纯净</font>。<font color=fuchsia>**根据 FP 的定义，纯净的函数只适合用来做计算**</font>；<font color=red>但是计算密集型工作正好不是前端程序擅长的工作</font>。相反，<font color=fuchsia>**前端程序几乎都是不纯净的**，因为像 IO、DOM、BOM等我们极度依赖的平台接口，全部都是具有副作用的</font>。

所以，前端程序或者说任何具有实际应用价值的程序几乎都是不稳定的，我们一定要认识到这一点，只有这样我们才能理解 RxJS / Redux-saga / Redux-thunk 这些东西存在的意义。

##### 副作用是必须的

<font color=fuchsia>只有完全孤立的系统才不会有副作用，但是试想一下如果系统是完全孤立的，我们又如何去使用它呢？如果一个系统跟外界没有交互，那它的存在又有什么意义呢？</font>因此，<font color=dodgerBlue>副作用对任何有实际应用意义的系统而言都是必须的</font>。

副作用就像燃气一样，如果我们对其放任不顾它就会变成散布于我们空间中的毒气，但是一旦给予它合适的管制，它就能成为推动我们空间前进的燃料。

##### 实际上这是两件事

前面分别讲述了：

- 副作用是有害的，我们要 **限制副作用**
- 副作用是必须的，我们要 **处理副作用**

实际上这是两件不相关的事，前者关注的是副作用产生之前，而后者关注的是副作用产生之后。

也因此分别对应了不同的表现形态：

- 对于限制副作用，它催生了 FP 层面的 Monad / AlgebraicEffect 和 库层面 的 Rx / Elm / Flux / Redux / Saga 这些东西，它们主要是用来限制副作用产生的
- 对于处理副作用，则体现在了不同前端框架对已经产生的副作用的处理策略上

所以，当在再看到一些关于FP或者关于副作用的言论出现的时候，我们就要分清楚了：

- 如果是基础库作者发表的，那么他们一般是在说如何限制副作用
- 如果是前端视图库作者发表的，那么他们一般是在说如何处理副作用

摘自：[再谈副作用](https://juejin.cn/post/6905234297360220174)

##### wikipedia 中的定义 补充

In computer science, <font color=fuchsia>**an operation, function or expression**</font> is said to <font color=dodgerBlue>have a **side effect** if it modifies some state variable value(s) outside its local environment</font>, which is to say if it has any observable effect other than its primary effect of returning a value to the invoker of the operation. Example <font color=fuchsia>side effects include **modifying a non-local variable**, **modifying a static local variable**, **modifying a mutable argument passed by reference**, **performing I/O** or **calling other functions with side-effects**</font>. In the presence of side effects, a program's behaviour may depend on history; that is, the order of evaluation matters. Understanding and debugging a function with side effects requires knowledge about the context and its possible histories.

Side effects play an important role in the design and analysis of programming languages. The degree to which side effects are used depends on the programming paradigm. For example, imperative programming（命令式编程） is commonly used to produce side effects, to update a system's state. By contrast, declarative programming is commonly used to report on the state of system, without side effects.

<font color=dodgerBlue>**Functional programming aims to minimize or eliminate side effects**</font>. <font color=fuchsia>The lack of side effects makes it easier to do formal verification of a program</font>. The functional language *Haskell* eliminates side effects such as I/O and other stateful computations by replacing them with [monadic](https://en.wikipedia.org/wiki/Monad_(functional_programming)) actions. <font color=lightSeaGreen>Functional languages such as *Standard ML*, *Scheme* and *Scala* do not restrict side effects, but it is customary for programmers to avoid them</font>.

摘自：[wikipedia - Side effect (computer science)](https://en.wikipedia.org/wiki/Side_effect_(computer_science))



#### Docstring

##### wiki 介绍

In programming, a **docstring** is a <font color=lightSeaGreen>string literal specified in source code that is used, like a comment, to document a specific segment of code</font>. <font color=dodgerBlue>Unlike conventional source code comments, or even specifically formatted comments like docblocks</font>, <font color=red>docstrings are **not stripped from the source tree when it is parsed**</font> and are <font color=red>**retained throughout the runtime of the program**</font>. This allows the programmer to inspect these comments at run time, for instance as an interactive help system, or as metadata.

Languages that support docstrings include Python, Lisp, Elixir, Clojure, Gherkin, Julia and Haskell.

摘自：[wikipedia - Docstring](https://en.wikipedia.org/wiki/Docstring)



#### ASCII 码知识补充

ASCII 编码只用了 128 位（2 的 7 次方），为什么 ASCII 编码不使用 8 位呢？可以收纳更多字符？

原因是最初设计 ACSII (1967 年) 的主要目的是在电传打字机上使用，那时候还没有字节的概念，同时当时寄存器也比较昂贵，编码设计需要尽可能精简，用 7 位编码就完全能够满足美国编码的需求。后来 IBM 也将 ASCII 扩展到了 8 位，新增了一些字符，[EASCII](https://zh.wikipedia.org/wiki/EASCII)

A ( 1000001 ) 恰好排在第 64 + 1 位，a ( 1100001 ) 排在了 64 + 32 + 1 的位置，这样看起来字母的编码是有意而为之，好处是<font color=FF0000 size=4>**当需要切换大小写是只需要做执行一次 `^32`**</font>。如下以 js 为例：

```js
const XOR_NUM = 32

const lowCaseChar_b = 'b'
const upperCaseChar_B = String.fromCharCode(lowCaseChar_b.charCodeAt()^XOR_NUM)
console.log(upperCaseChar_B) // B

const upperCaseChar_Z = 'Z'
const lowerCaseChar_z = String.fromCharCode(upperCaseChar_Z.charCodeAt()^XOR_NUM)
console.log(lowerCaseChar_z) // z
```



#### 银行家舍入

银行家舍入法 是由 IEEE-754 标准规定的浮点数取整算法，大部分的编程软件都使用的是这种方法。 所谓银行家舍入法，其实质是一种<font color=FF0000>四舍六入五取偶</font>（又称 四舍六入五留双）<font color=FF0000>法</font>。

“四舍六入五成双”，也即“4舍 6入 5凑偶”，这里 “四”是指 ≤4 时舍去，"六"是指 ≥6 时进上。<font color=dodgerBlue>“五”指的是 根据 5 后面的数字来定</font>：当 <font color=FF0000>5 后有数时，舍 5 入 1</font>；<font color=dodgerBlue>当 5 后无有效数字时，需要分两种情况来讲</font>：<font color=fuchsia>**5 前为奇数，舍 5 入 1**</font> ；<font color=fuchsia>**5 前为偶数，舍 5 不进（ 0 是偶数）**</font>。

摘自：[百度百科 - 银行家舍入](https://baike.baidu.com/item/%E9%93%B6%E8%A1%8C%E5%AE%B6%E8%88%8D%E5%85%A5/4781630)

> 💡 补充
>
> 另外，根据 [toFixed的结果可能会欺骗你【渡一教育】](https://www.bilibili.com/video/BV14r421b759) 的说法，JS 中 `Number.prototype.toFixed` 并没有完全使用 “银行家舍入” / “银行家算法”。另外，问 Copilot Chat 二次确认了下，得到如下回复：
>
> <img src="https://s2.loli.net/2024/04/05/QBjczlh6KeWOZ8A.png" alt="image-20240405162832728" style="zoom:50%;" />
> 
> 另外，视频中也给出了 `toFixed` 的运行结果在小数点最后一位为 5 的情况下，行为不符合预期；比如 `1.45.toFixed(1) === '1.4'` ，而 `2.45.toFixed(1) === '2.5'` 的原因：由于二进制的表示存在误差，1.45 在二进制的表示时，相比 1.5 更接近 1.4 (`1.45.toPrecision(20) === '1.4499999999999999556'`) ；而 2.45 在二进制的表示时，相比 2.4 更接近 2.5 ( `2.45.toPrecision(20) === '2.4500000000000001776'` )



#### Bootstrap 

// TODO https://zh.wikipedia.org/wiki/%E5%95%9F%E5%8B%95%E7%A8%8B%E5%BC%8F



#### nonce

number once // TODO https://en.wikipedia.org/wiki/Cryptographic_nonce



#### 关注点分离

// TODO



#### 数字英文表示

在上计算机课程（尤其是计算机组成）的时候，了解到了 double、triple 和 quad 分别表示 二、三、四，然而它还有更多：

2 = double , 3 = triple , 4 = quadruple , 5 = quintuple , 6 = sextuple , 7 = septuple , 8 = octuple

学习自：[2=double,3=triple,what's for 4,5,6,e.t.c.](https://www.proz.com/kudoz/english/other/526192-2%3Ddouble3%3Dtriplewhats-for-456etc.html)



#### CVE & CWE

// TODO

可参考 https://g.co/gemini/share/01d70c01a371



#### 抽象与架构设计

可以看下 [程序或架构设计的抽象能力 - RMosaicDCFan的文章 - 知乎](https://zhuanlan.zhihu.com/p/1919853703936082787)



#### 领域驱动设计 (DDD)

可以通过 https://g.co/gemini/share/5e7798fc640b 了解下 DDD，以及 Golang 在 DDD 下的项目结构

另外，可以看下 [想给面试官吹DDD吗？项目像搭积木一样运行：领域驱动设计（DDD）让复杂业务变简单](https://juejin.cn/post/7519812174899478582) ，介绍了一些概念，感觉作为入门还不错
