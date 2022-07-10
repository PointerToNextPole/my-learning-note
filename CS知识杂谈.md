# CS 知识杂谈



> ⚠️ **由于水平不够没有时间仔细研究或只是一时好奇，这里很多只是摘取了概念，也只是挖坑；如需深入理解，还需进一步查资料与添加笔记**



#### **元编程**
元编程一言以蔽之，就是用<font color=FF0000>代码生成（操纵）代码</font>。

<font color=FF0000>常见的开发语言均能做到元编程</font>，Lisp 这货就不用多说了，C 的 Marco，<font color=FF0000>C++ 的 Template，Java的Annotation</font>，C# 的 Attribute、Reflection、CodeDom和 IL Emitter，各种脚本语言（如 js、python）的eval，甚至连 Unix/Linux 的 shell 脚本也能。

元编程常见的<font color=FF0000>**应用场景**</font>很多，扩展（重构）语法、开发DSL、生成代码、根据特定场景自动选择代码优化、解决一些正交的架构设计问题、AOP等等。
摘自：[什么是元编程以及元语言？ - 猫杀的回答 - 知乎](https://www.zhihu.com/question/22572900/answer/21828721)



#### 图灵机

图灵机（Turing Machine）是图灵在1936年发表的 "On Computable Numbers, with an Application to the Entscheidungsproblem"（《论可计算数及其在判定性问题上的应用》）中提出的数学模型。既然是数学模型，它就并非一个实体概念，而是架空的一个想法。在文章中图灵描述了它是什么，并且证明了：<mark>只要图灵机可以被实现，就可以用来解决任何可计算问题。</mark></br>

**图灵机的结构包括以下几个部分：**

> - 一条无限长的<font color=FF0000>纸带</font>（tape），纸带被分成一个个相邻的格子（square），每个格子都可以写上至多一个字符（symbol）。
> - 一个<font color=FF0000>字符表</font>（alphabet），<mark>即字符的集合，它包含纸带上可能出现的所有字符。</mark>其中包含一个特殊的空白字符（blank），意思是此格子没有任何字符。
> - 一个读写头（head），可理解为指向其中一个格子的指针。它可以读取/擦除/写入当前格子的内容，此外也可以每次向左/右移动一个格子。
> - 一个状态寄存器（state register），它追踪着每一步运算过程中，整个机器所处的状态（运行/终止）。当这个状态从运行变为终止，则运算结束，机器停机并交回控制权。如果你了解有限状态机，它便对应着有限状态机里的状态。
> - 一个有限的指令集（instructions table），它记录着读写头在特定情况下应该执行的行为。可以想象读写头随身有一本操作指南，里面记录着很多条类似于“当你身处编号53的格子并看到其内容为0时，擦除，改写为1，并向右移一格。此外，令下一状态为运行。”这样的命令。其实某种意义上，这个指令集就对应着程序员所写下的程序了。
> ![图灵机结构](https://pic1.zhimg.com/80/v2-6d57f9001041416d43e886f14fd43f84_1440w.jpg "图灵机结构")</br>
> 
> 在计算开始前，纸带可以是完全空白，也可以在某些格子里预先就有写上部分字符作为输入。运算开始时，读写头从某一位置开始，严格按照此刻的配置（configuration），即：
> - 当前所处位置
> - 当前格子内容</br>
> 
> 来一步步的对照着指令集去进行操作，直到状态变为停止，运算结束。而后纸带上留下的信息，即字符的序列（比如类似“...011001...”）便作为输出，由人来解码为自然语言。</br>
> 
>要重申一下，以上只是图灵机模型的内容，而非具体的实现。所谓的纸带和读写头都只是图灵提出的抽象概念。为便于理解打一个比方。算盘虽然不是图灵机（因为它没有无限长的纸带，即无限的存储空间），但它的行为与图灵机一致。每一串算珠都是纸带上的一格，一串算珠上展示的数字便记录着当前格中的字符（可以是空白，可以是 12345 ）。人类的手即是读写头，可以更改每串算珠的状态。算盘的运行遵循人脑中的算法，当算法结束，算盘停机。</br>
> 摘自：[什么是图灵完备？ - Ran C的回答 - 知乎](https://www.zhihu.com/question/20115374/answer/288346717)



#### 图灵完备

在可计算性理论里，如果一系列操作数据的规则（如指令集、编程语言、细胞自动机）<font color=FF0000>可以用来模拟单带图灵机</font>，那么它是图灵完备的。

摘自：[图灵完备-百度百科](https://baike.baidu.com/item/%E5%9B%BE%E7%81%B5%E5%AE%8C%E5%A4%87/4634934?fr=aladdin)

图灵完备 ( Turing Completeness ) 是指<mark>机器执行任何其他可编程计算机能够执行计算的能力</mark>。

图灵完备也意味着你的语言可以做到能够用图灵机能做到的所有事情，可以解决所有的可计算问题。

摘自：[图灵完备是什么？](https://www.jianshu.com/p/b8b492ea547d)



### 编程范式

编程范型、编程范式或程序设计法（Programming paradigm），（范即模范、典范之意，范式即模式、方法），是一类典型的编程风格，是指从事软件工程的一类典型的风格（可以对照方法学）。

摘自：[编程范型-百度百科](https://baike.baidu.com/item/%E7%BC%96%E7%A8%8B%E8%8C%83%E5%9E%8B/1475451?fromtitle=%E7%BC%96%E7%A8%8B%E8%8C%83%E5%BC%8F&fromid=23696164&fr=aladdin)



### 行为树

行为树是一种形式化的图形建模语言，主要用于系统和软件工程。 行为树采用明确定义的符号来明确表示数百甚至数千种自然语言需求，这些需求通常用于表达大规模软件集成系统的利益相关者需求。

摘自：[行为树-百度百科](https://baike.baidu.com/item/%E8%A1%8C%E4%B8%BA%E6%A0%91/22735785?fr=aladdin)



#### UUID

**UUID** 是指 Universally Unique Identifier，翻译为中文是**通用唯一识别码**，<mark>UUID 的目的是让分布式系统中的所有元素都能有唯一的识别信息</mark>。如此一来，每个人都可以创建不与其它人冲突的 UUID，就不需考虑数据库创建时的名称重复问题。

##### 定义

UUID 是<mark>由一组32位数的16进制数字所构成</mark>，是故 UUID 理论上的总数为1632=2128，约等于3.4 x 10123。

##### 格式

UUID 的十六个八位字节被表示为 32个十六进制数字，<mark>以连字号分隔的五组来显示，形式为 8-4-4-4-12</mark>，总共有 36个字符（即三十二个英数字母和四个连字号）。例如：

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

摘自：[**什么是 UUID**](https://www.jianshu.com/p/da6dae36c290)



#### foo & bar

foo & bar 是类似于”哑变元“，<mark>我们可以把 foo 理解成张三李四, 或者"某某"</mark>，但真正了解 foo 及 foobar 等词的含义，还是能使我们更好的理解英文文档，避免产生不必要的歧义。

术语 foobar , foo , bar , baz 和 qux 经常在计算机编程或计算机相关的文档中<mark>被用作**占位符**的名字</mark>。<mark>当变量，函数，或命令本身不太重要的时候， foobar , foo , bar ,baz 和 qux 就被用来充当这些实体的名字</mark>，这样做的目的仅仅是阐述一个概念，说明一个想法。这些术语本身相对于使用的场景来说没有任何意义。Foobar经常被单独使用；而当需要多个实体举例的时候，foo，bar，和baz则经常被按顺序使用。

摘自：[**你所不知道的“foo”和“bar”**](https://cloud.tencent.com/developer/article/1360988)



#### 哑元

哑元，即虚拟变量 ( Dummy Variables) 又称虚设变量、名义变量或哑变量，用以反映质的属性的一个人工变量，是量化了的自变量，通常取值为0或1。<mark>引入哑变量可使线形回归模型变得更复杂，但对问题描述更简明，一个方程能达到两个方程的作用，而且接近现实</mark>。

摘自：[**百度百科 - 虚拟变量**]([https://baike.baidu.com/item/%E8%99%9A%E6%8B%9F%E5%8F%98%E9%87%8F/8262721?fr=aladdin](https://baike.baidu.com/item/虚拟变量/8262721?fr=aladdin))



#### Mixin

**Mixin是面向对象程序设计语言中的类**，提供了方法的实现。其他类可以访问mixin类的方法而不必成为其子类。Mixin有时被称作"included"而不是"inherited"。mixin为使用它的class提供额外的功能，但<mark>自身却不单独使用（不能单独生成实例对象，属于抽象类）</mark>。因为有以上限制，Mixin类通常作为功能模块使用，在需要该功能时“混入”，而且不会使类的关系变得复杂。用户与Mixin不是“is-a”的关系，而是“-able”关系

<mark>Mixin有利于代码复用<font color=FF0000>又避免了多重继承的复杂</font></mark>。使用Mixin享有单一继承的单纯性和多重继承的共有性。接口与mixin相同的地方是都可以多继承，不同的地方在于mixin是带实现的。Mixin也可以看作是带实现的interface。这种设计模式实现了依赖反转原则。

Mixin 实质上是利用语言特性（比如 Ruby 的 include 语法、Python 的多重继承）来更简洁地实现组合模式。

摘自：[wikipedia - Mixin](https://zh.wikipedia.org/wiki/Mixin)	[知乎 - Mixin是什么概念?](https://www.zhihu.com/question/20778853)



#### ABI

In [computer software](https://en.wikipedia.org/wiki/Computer_software), an <font color=FF0000>**application binary interface** (**ABI**) is an [interface](https://en.wikipedia.org/wiki/Interface_(computing)) between two binary program modules</font>（译：二进制程序模块）. Often, one of these modules is a [library](https://en.wikipedia.org/wiki/Library_(computing)) or [operating system](https://en.wikipedia.org/wiki/Operating_system) facility（译：提供的服务）, and the other is a program that is being run by a user.

An <font color=fuchsia>***ABI***</font> defines how data structures or computational routines（译：例程。相关内容，详见下面 [[#routine 例程]]） are accessed in [machine code](https://en.wikipedia.org/wiki/Machine_code), which is a <font color=fuchsia>**low-level**</font>, hardware-dependent format. In contrast, an [***API***](https://en.wikipedia.org/wiki/Application_programming_interface) defines this access in [source code](https://en.wikipedia.org/wiki/Source_code), which is a relatively <font color=DeepSkyBlue >**high-level**</font>, hardware-independent, often [human-readable](https://en.wikipedia.org/wiki/Human-readable) format. A common aspect of an ABI is the [calling convention](https://en.wikipedia.org/wiki/Calling_convention), which <font color=FF0000>determines how data is provided as input to, or read as output from</font>, computational routines. Examples of this are the [x86 calling conventions](https://en.wikipedia.org/wiki/X86_calling_conventions).

Adhering to an ABI (which may or may not be officially standardized) is usually the job of a [compiler](https://en.wikipedia.org/wiki/Compiler), operating system, or library author. However, <font color=FF0000>an application programmer may **have to deal with an ABI directly** when **writing a program in a mix of programming languages**</font> , or even compiling a program written in the same language with different compilers. （译：如果撰写一个程序使用多种语言，或使用多种编译器编译统一语言的程序，就必须直接处理 ABI ）

摘自：[wiki - ABI](https://en.wikipedia.org/wiki/Application_binary_interface)

##### routine 例程

> 什么是“子程序 ( routine ) ” ？子程序 是为实现一个特定的目的而编写的一个可被调用的 方法 ( method ) 或 过程 ( procedure )。例如C++ 中的函数 ( function )， Java 中的方法 ( method )，或 Microsoft Visual Basic 中的函数过程 ( function procedure ) 或 子过程 ( sub procedure )。对于某些使用方式，C 和 C++ 中的 宏 ( macro ) 也可认为是子程序。
>
> 摘自：《代码大全（第二版）》chapter 7 - P162

如果细分起来：function 是有返回值的 routine，procedure（过程）是没有返回值的 routine，method 是作为 class 的成员的 routine，甚至 C++ 中重载了的运算符也算是 routine。

摘自：[CC2e 术语：把 routine 译为“子程序”的理由](https://blog.csdn.net/techcrunch/article/details/1961970)



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

管生命周期的那个作为宿主语言，所以 FFI 中一般高级语言做主导，低级语言被调用，而且还能封装成二进制库，比如 lib、dll、so 之类的给不同语言用（注：比如 ffmpeg ？）。

**缺点**：二进制毕竟太底层了，没有大家一致认可的调用约定也是不可能互通的，要遵守一些基本准则。调用约定、类型表示 和 名称修饰这三者，构成了二进制接口 ( Application Binary Interface )。

但是, 世界上这么多 CPU，这么多语言，每个类型系统都不一样，想想也没法统一；不能统一就会有混乱, 混乱乘以发展时间就是屎山的大小。你用 node 肯定被 node-gyp 恶心过，用 Java 肯定被 JNI 恶心过。在时间面前，再好的约定也会变得混乱，最后把你埋葬在屎山之中

##### RPC ( Remote Procedure Call )

那我不管别人怎么规定了，我就按自己的规定来，你们都给我转成这个格式交互（**注：**适配器模式，中间层？）。

比如我用 javascript，那你们和我交互都发 json，岂不美哉？

**缺点**：只有一点点微小的代价：<font color=FF0000>发出时有一个序列化开销，收到时还有一个反序列化开销，对面也有这两个开销</font>。

虽然名义上是远程调用，但是也常常开两个端口本地往本地发。比如 LSP 就基于 JSON-RPC ，本地往本地发（**注：**LSP 即，语言服务协议，这也是 VS Code 可以支持编写多语言的技术原理）。

当然 json 的 serde（**注：**进行 “序列化” 和“反序列化”操作的程序，似乎是 [GitHub - serde-rs/json](https://github.com/serde-rs/json) ） 太慢了，计算机就该用二进制说话，所以 <font color=FF0000>谷歌的 **grpc** 使用 Protocol Buffers 这种二进制格式作为交换格式</font>。

一般的应用远远达不到要优化掉 serde 开销这个地步，所以现在就慢慢演变到传 json 搞定一切了。

摘自：[不同语言为什么不能相互调用 ？ - 酱紫君的回答 - 知乎](https://www.zhihu.com/question/353354060/answer/2554554876) 



#### RPC

分布式计算中，远端程序呼叫 ( Remote Procedure Call, RPC ) 是一个计算机通信协议。该协议 <font color=FF0000>允许 运行于一台计算机的程序 调用 另一个地址空间</font>（通常为一个开放网络的一台计算机）<font color=FF0000>的子程序</font>，而 <font color=FF0000>程序员就像调用本地程序一样，无需额外地为这个交互作用编程（无需关注细节）</font>。RPC 是一种 服务器-客户端 ( Client/Server ) 模式，经典实现是一个通过 **发送请求-接受回应** 进行信息交互的系统。

如果涉及的软件采用面向对象编程，那么远程过程调用亦可称作远端呼叫或远端方法呼叫，例：Java RMI 。

<font color=FF0000>RPC 是一种 **进程间通信** 的模式，程序分布在不同的地址空间里</font>。如果<mark style="background: aqua">在同一主机里</mark>，RPC 可以通过不同的虚拟地址空间（即便使用相同的物理地址）进行通讯；而<mark>在不同的主机间</mark>，则通过不同的物理地址进行交互。许多技术（通常是不兼容）都是基于这种概念而实现的。

摘自：[wikipedia - 远程过程调用](https://zh.wikipedia.org/zh-cn/%E9%81%A0%E7%A8%8B%E9%81%8E%E7%A8%8B%E8%AA%BF%E7%94%A8)



#### 规格继承和实现继承





#### 常见配置文件语言

- INI
- XML
- JSON
- YAML

具体的可以阅读：[常见配置文件语言: INI, XML, JSON与YAML]([https://dhpo.github.io/2018/02/03/%E5%B8%B8%E8%A7%81%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6-XML-JSON-INI%E4%B8%8EYAML/](https://dhpo.github.io/2018/02/03/常见配置文件-XML-JSON-INI与YAML/))



//TODO

#### DSL ( Domain-Specific Language )



#### argument 和 parament 的区别

- argument：实参
- parament：形参



//todo

#### 方法链



//todo

#### 回调函数





#### 命名规则

- **驼峰式(Camel Case)**

  - 小驼峰（lower camel case）：第一个单词首字母小写，其余单词首字母大写

    示例：firstName、userPhone

  - 大驼峰（upper camel case, Pascal Case）：单词首字母都大写

    示例：FirstName、UserPhone

- **Snake Case**：单词之间使用下划线 `_` 连接

  示例：first_name、user_phone

- **kebab Case ( spinal-case, Train-Case, Lisp-case )**：单词之间使用下划线 `-` 连接

  示例：first-name、user-phone



#### 函数和方法的区别

- 函数（function）是指一段可以直接被其名称调用的代码块，它可以传入一些参数进行处理并返回一些数据，所有传入函数的数据都是被明确定义。
- 方法指的是一段<font color=FF0000>被它关联的对象</font>通过它的名字调用的代码块。

**总结：方法就是面向对象版的函数**

摘自：[方法和函数的区别](https://blog.csdn.net/notsaltedfish/article/details/75174556)



#### 鸭子类型

鸭子类型（英语：duck typing）在程序设计中是动态类型的一种风格。在这种风格中，一个对象有效的语义，不是由继承自特定的类或实现特定的接口，而是由"当前方法和属性的集合"决定。

> 这个概念的名字来源于由詹姆斯·惠特科姆·莱利提出的鸭子测试（见下面的“历史”章节），“鸭子测试”可以这样表述：“当看到一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子，那么这只鸟就可以被称为鸭子。”

在鸭子类型中，关注点在于对象的行为，能作什么；而不是关注对象所属的类型。例如，在不使用鸭子类型的语言中，我们可以编写一个函数，它接受一个类型为"鸭子"的对象，并调用它的"走"和"叫"方法。在使用鸭子类型的语言中，这样的一个函数可以接受一个任意类型的对象，并调用它的"走"和"叫"方法。如果这些需要被调用的方法不存在，那么将引发一个运行时错误。任何拥有这样的正确的"走"和"叫"方法的对象都可被函数接受的这种行为引出了以上表述，这种决定类型的方式因此得名。

摘自：[wiki - 鸭子类型](https://zh.wikipedia.org/wiki/%E9%B8%AD%E5%AD%90%E7%B1%BB%E5%9E%8B)



**TICK（滴答）**：是计算机的计时器的单位



**脚手架：**脚手架就是别人用构建工具帮你搭好了原始项目，你可以在不懂构建工具的情况下进行前端开发。不过这就是初级前端的基本工作，给我一个环境，让我安心的写业务代码。

摘自：[一小时的时间，上手 Webpack](https://zhuanlan.zhihu.com/p/114286243)



#### 编程命名

##### 命名词汇 Container 和 Wrapper 的区别

In programming languages the word <font color=FF0000>**container**</font> is generally used for structures that can <font color=FF0000>contain more than one element</font>.

A <font color=FF0000>**wrapper**</font> instead is something that <font color=FF0000>wraps around a single object</font> to provide more functionalities and interfaces to it.

摘自：[stack overflow - CSS Language Speak: Container vs Wrapper?](https://stackoverflow.com/questions/4059163/css-language-speak-container-vs-wrapper)



#### 重写方法和重载方法

- 重写 (Override) ：是子类对父类的允许访问的方法的实现过程进行重新编写, 返回值和形参都不能改变。即外壳不变，核心重写！
- 重载 (overload) ：是在一个类里面，方法名字相同，而参数不同。返回类型可以相同也可以不同。



#### 一等公民

第一类物件（英语：First-class object）在电脑科学中指可以在执行期创造并作为参数传递给其他函数或存入一个变数的实体。将一个实体变为第一类物件的过程叫做“物件化”（Reification）。

<mark>“第一类物件”这一名称最早由克里斯托弗·斯特雷奇在1960年代发明，原称“第一类公民”（First-class citizen），意指函数可作为电脑语言中的第一类公民</mark>。英文中也称“First-class entity”或“First-class value”。

第一类物件不一定是物件导向程式设计所指的物件，而可以指任何程式中的实体。<font color=FF0000> **一般第一类物件所特有的特性为**</font>：

- 可以被存入变数或其他结构
- 可以被作为参数传递给其他函数
- 可以被作为函数的返回值
- <font color=FF0000>**可以在执行期创造，而无需完全在设计期全部写出**</font>
- <font color=FF0000>即使没有被系结至某一名称，也可以存在</font>

<mark>绝大多数语言中，数值与基础型别都是第一类物件</mark>，然而不同语言中对函数的区别很大，例如C语言与C++中的函数不是第一类物件，因为在这些语言中函数不能在执行期创造，而必须在设计时全部写好。相比之下，<font color=FF0000> Scheme中的函数是第一类物件，因为可以用lambda表达式来创造匿名函数并作为第一类物件来操作</font>。

摘自：[wiki - 第一类对象](https://zh.wikipedia.org/wiki/%E7%AC%AC%E4%B8%80%E9%A1%9E%E7%89%A9%E4%BB%B6)

一等公民表示：用户<font color=FF0000 size=4> **可以通过这个元素（被称为一等公民的元素）来完成该语言当中的所有其他元素能进行的所有操作**</font>。

**在 SICP 中也有提到 *一等公民* ：**

- <font color=FF0000> 可以用变量命名</font>。即：函数表达式
- <font color=FF0000> 可以作为参数传递给过程</font>。即：高阶函数
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



#### 闭包 ( Closure ) 

<font color=FF0000 size=4>**// TODO 下面的链接包含大量的知识（不仅仅是 js 中的闭包，凡是实现 FP 的语言应该都有闭包），建议阅读与记录。**</font>

摘自：[设计闭包（Closure）的初衷是为了解决什么问题？ - 知乎](https://www.zhihu.com/question/51402215)



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



#### Bootstrap 

// TODO https://zh.wikipedia.org/wiki/%E5%95%9F%E5%8B%95%E7%A8%8B%E5%BC%8F



#### nonce

number once // TODO https://en.wikipedia.org/wiki/Cryptographic_nonce
