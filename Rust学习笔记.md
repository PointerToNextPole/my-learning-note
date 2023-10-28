# Rust 学习笔记



## 《Rust 语言圣经》笔记



#### 认识 cargo

##### 创建项目

```sh
$ cargo new hello_world
```

上面的命令使用 `cargo new` 创建一个项目，项目名是 `world_hello` ，<font color=lightSeaGreen>该项目的结构和配置文件都是由 `cargo` 生成，意味着**我们的项目被 `cargo` 所管理**</font>。

可以看下创建的目录结构：

<img src="https://s2.loli.net/2023/10/27/Jj7Vcr2YvHGkCFM.png" alt="image-20231027161606056" style="zoom:50%;" />

早期的 `cargo` 在创建项目时，必须添加 `--bin` 的参数，如下所示：

```sh
$ cargo new world_hello --bin
```

现在的版本，已经无需此参数，<font color=red>`cargo` 默认就创建 `bin` 类型的项目</font>。<font color=dodgerBlue>Rust 项目主要分为两个类型：`bin` 和 `lib`</font>，前者是一个可运行的项目，后者是一个依赖库项目。

##### 运行项目

有两种方式可以运行项目：

1. `cargo run`
2. 手动编译和运行项目

###### `cargo run`

效果如下：

<img src="https://s2.loli.net/2023/10/27/PnCcxZL3kuOroe4.png" alt="image-20231027162343490" style="zoom:50%;" />

另外，会发现项目中多了一个 target 文件夹：

<img src="https://s2.loli.net/2023/10/27/keHJ5NywoT9LbZ2.png" alt="image-20231027162712140" style="zoom:50%;" />

上述代码，`cargo run` 首先对项目进行编译( 👀 Compiling )，然后再运行，因此它实际上等同于运行了两个指令，下面我们手动试一下编译和运行项目：

**编译**

<img src="https://s2.loli.net/2023/10/27/TjI2MdFACpcqtHh.png" alt="image-20231027162910983" style="zoom:50%;" />

**运行**

<img src="https://s2.loli.net/2023/10/27/Mgt9DzKZrx5j4lc.png" alt="image-20231027163026842" style="zoom:50%;" />

在调用的时候，路径 `./target/debug/world_hello` 中有一个 `debug` 字段，<font color=red>这里我们运行的是 `debug` 模式</font>，<font color=dodgerBlue>在这种模式下</font>，<font color=red>**代码的编译速度会非常快**</font>，<font color=dodgerBlue>可是福兮祸所伏</font>，<font color=red>**运行速度就慢了**</font>。原因是：<font color=lightSeaGreen>在 `debug` 模式下，Rust 编译器不会做任何的优化，只为了尽快的编译完成，让你的开发流程更加顺畅</font>。

想要高性能的代码，添加 `--release` 来编译：

- `cargo run --release`
- `cargo build --release`

> 👀 试了下：使用 `cargo run --release` 似乎也是可以的（至少不会报错）

##### cargo check

<font color=dodgerBlue>当项目大了后，`cargo run` 和 `cargo build` 不可避免的会变慢，那么有没有更快的方式来验证代码的正确性呢</font>？<font color=red>可以使用 `cargo check`</font> 。

`cargo check` 是我们在代码开发过程中最常用的命令，<font color=dodgerBlue>它的作用很简单</font>：<font color=red>快速的检查一下代码能否编译通过</font>。因此该命令速度会非常快，能节省大量的编译时间。

<img src="https://s2.loli.net/2023/10/27/zBTZQtdJxl9vSND.png" alt="image-20231027164343634" style="zoom:50%;" />

> Rust 虽然编译速度还行，但是还是不能与 Go 语言相提并论，因为 Rust 需要做很多复杂的编译优化和语言特性解析，甚至连如何优化编译速度都成了一门学问：[优化编译速度](https://course.rs/profiling/compiler/speed-up.html)。

##### Cargo.toml 和 Cargo.lock

`Cargo.toml` 和 `Cargo.lock` 是 `cargo` 的核心文件，它的所有活动均基于此二者。

> 👀 感觉和 npm 项目中的 `package.json` 和 `package-lock.json` 类似，问了下 Claude ，得到了肯定的结果：
>
> <img src="https://s2.loli.net/2023/10/27/rLfaqOmgZXliCGd.png" alt="image-20231027165230070" style="zoom:45%;" />

- `Cargo.toml` 是 `cargo` 特有的**项目数据描述文件**。它存储了项目的所有元配置信息，如果 Rust 开发者希望 Rust 项目能够按照期望的方式进行构建、测试和运行，那么，必须按照合理的方式构建 `Cargo.toml`。
- `Cargo.lock` 文件是 `cargo` 工具根据同一项目的 `toml` 文件生成的**项目依赖详细清单**，因此我们一般不用修改它，只需要对着 `Cargo.toml` 文件撸就行了。

> 什么情况下该把 `Cargo.lock` 上传到 git 仓库里？很简单，当你的项目是一个可运行的程序时，就上传 `Cargo.lock`，如果是一个依赖库项目，那么请把它添加到 `.gitignore` 中。

###### package 配置段落

`package` 中记录了项目的描述信息，典型的如下：

```toml
[package]
name = "world_hello"
version = "0.1.0"
edition = "2021"
```

`name` 字段定义了项目名称，`version` 字段定义当前版本，新项目默认是 `0.1.0`，`edition` 字段定义了我们使用的 Rust 大版本。因为本书很新（不仅仅是现在新，未来也将及时修订，跟得上 Rust 的小步伐），所以使用的是 `Rust edition 2021` 大版本，详情见 [Rust 版本详解](https://course.rs/appendix/rust-version.html)

###### 定义项目依赖

使用 `cargo` 工具的最大优势就在于，<font color=lightSeaGreen>能够对该项目的各种依赖项进行方便、统一和灵活的管理</font>。

<font color=dodgerBlue>在 `Cargo.toml` 中，**主要通过各种依赖段落来描述该项目的各种依赖项**：</font>

- 基于 Rust 官方仓库 `crates.io`，通过版本说明来描述
- 基于项目源代码的 git 仓库地址，通过 URL 来描述
- 基于本地项目的绝对路径或者相对路径，通过类 Unix 模式的路径来描述

<font color=dodgerBlue>这三种形式具体写法如下：</font>

```toml
[dependencies]
rand = "0.3"
hammer = { version = "0.5.0" }
color = { git = "https://github.com/bjz/color-rs" }
geometry = { path = "crates/geometry" }
```

相信聪明的读者已经能看懂该如何引入外部依赖库，这里就不再赘述。详细的说明参见此章：[Cargo 依赖管理](https://course.rs/cargo/reference/specify-deps.html)，但是不建议大家现在去看，只要按照目录浏览，拨云见日指日可待。



#### 不仅仅是 Hello world

##### 多国语言的"世界，你好"

```rust
fn greet_world() {
    let southern_germany = "Grüß Gott!";
    let chinese = "世界，你好";
    let english = "World, hello";
    let regions = [southern_germany, chinese, english];
    for region in regions.iter() {
        println!("{}", &region);
    }
}

fn main() {
    greet_world();
}
```

运行结果：

<img src="https://s2.loli.net/2023/10/27/7lsj8HyXvWEbiIz.png" alt="image-20231027170617662" style="zoom:50%;" />

<font color=dodgerBlue>花点时间来看看上面的代码：</font>

首先，<font color=red>Rust 原生支持 UTF-8 编码的字符串</font>，这意味着你可以很容易的使用世界各国文字作为字符串内容。

其次，<font color=dodgerBlue>关注下 `println` 后面的 `!`</font>，如果你有 Ruby 编程经验，那么你可能会认为这是解构操作符，但是<font color=red>在 Rust 中，这是 `宏` 操作符</font>，你目前可以认为宏是一种特殊类型函数。

对于 <font color=dodgerBlue>`println`</font> 来说，我们<font color=lightSeaGreen>没有使用其它语言惯用的 `%s`、`%d` 来做输出占位符，而是使用 `{}`</font>，因为 <font color=red>Rust 在底层帮我们做了大量工作，会自动识别输出数据的类型，例如当前例子，会识别为 `String` 类型</font>。

最后，和其它语言不同，<font color=fuchsia>Rust 的**集合类型**不能直接进行循环，需要变成迭代器</font>（<font color=red>这里是通过 `.iter()` 方法</font>），<font color=fuchsia>才能用于迭代循环</font>。在目前来看，你会觉得这一点好像挺麻烦，不急，以后就知道这么做的好处所在。

> 实际上这段代码可以简写，<font color=dodgerBlue>在 2021 edition 及以后</font>，<font color=red>支持直接写 `for region in regions`</font>，原因会在迭代器章节的开头提到，是<font color=red>**因为 for 隐式地将 regions 转换成迭代器**</font>。
>
> > 👀 试了下，去掉 `.iter()` 运行结果是符合预期运行的

##### Rust 语言初印象

```rust
fn main() {
   let penguin_data = "\
   common name,length (cm)
   Little penguin,33
   Yellow-eyed penguin,65
   Fiordland penguin,60
   Invalid,data
   ";

   let records = penguin_data.lines();

   for (i, record) in records.enumerate() {
     if i == 0 || record.trim().len() == 0 {
       continue;
     }

     // 声明一个 fields 变量，类型是 Vec
     // Vec 是 vector 的缩写，是一个可伸缩的集合类型，可以认为是一个动态数组
     // <_> 表示 Vec 中的元素类型由编译器自行推断，在很多场景下，都会帮我们省却不少功夫
     let fields: Vec<_> = record
       .split(',')
       .map(|field| field.trim())
       .collect();
     if cfg!(debug_assertions) {
       // 输出到标准错误输出
       eprintln!("debug: {:?} -> {:?}", record, fields);
     }

     let name = fields[0];
     // 1. 尝试把 fields[1] 的值转换为 f32 类型的浮点数，如果成功，则把 f32 值赋给 length 变量
     //
     // 2. if let 是一个匹配表达式，用来从=右边的结果中，匹配出 length 的值：
     //   1）当=右边的表达式执行成功，则会返回一个 Ok(f32) 的类型，若失败，则会返回一个 Err(e) 类型，if let 的作用就是仅匹配 Ok 也就是成功的情况，如果是错误，就直接忽略
     //   2）同时 if let 还会做一次解构匹配，通过 Ok(length) 去匹配右边的 Ok(f32)，最终把相应的 f32 值赋给 length
     //
     // 3. 当然你也可以忽略成功的情况，用 if let Err(e) = fields[1].parse::<f32>() {...}匹配出错误，然后打印出来，但是没啥卵用
     if let Ok(length) = fields[1].parse::<f32>() {
         // 输出到标准输出
         println!("{}, {}cm", name, length);
     }
   }
 }
```

上面代码中，<font color=dodgerBlue>值得注意的 Rust 特性有</font>：

- 控制流：`for` 和 `continue` 连在一起使用，实现循环控制。

- 方法语法：由于 <font color=red>Rust 没有继承</font>，因此 <font color=lightSeaGreen>Rust 不是传统意义上的面向对象语言</font>，但是它却从 `OO` 语言那里偷师了方法的使用 `record.trim()`，`record.split(',')` 等。

- 高阶函数编程：<font color=red>函数可以作为参数也能作为返回值，例如 `.map(|field| field.trim())`</font>，这里 `map` 方法中<font color=red>使用闭包函数作为参数</font>，也可以称呼为 `匿名函数`、`lambda 函数`。

- 类型标注：`if let Ok(length) = fields[1].parse::<f32>()`，通过 `::<f32>` 的使用，告诉编译器 `length` 是一个 `f32` 类型的浮点数。这种类型标注不是很常用，但是在编译器无法推断出你的数据类型时，就很有用了。

  > 💡 关于这里 `::<f32>` 的用法，开始以为是 “类型断言”，问了下 Cluade，告诉我不是：
  >
  > <img src="https://s2.loli.net/2023/10/27/ij1JdeQPsLxraXV.png" alt="image-20231027174616276" style="zoom:48%;" />

- <font color=fuchsia>条件编译</font>：`if cfg!(debug_assertions)`，<font color=red>说明紧跟其后的输出（打印）只在 `debug` 模式下生效</font>。

  > 👀 运行结果如下：多了 debug 相关的输出
  >
  > <img src="https://s2.loli.net/2023/10/28/Z3MwWGOEAmk4cCg.png" alt="image-20231028234229333" style="zoom:50%;" />
  >
  > 如果想要 debug 相关输出消失，可以使用 `--release` 参数。另外，以 `cargo run --release` 编译时间会很长，在当前的 Mac 上有 27.75s ...

- 隐式返回：Rust 提供了 `return` 关键字用于函数返回，但<font color=lightSeaGreen>是在很多时候，我们可以省略它</font>。因为 Rust 是 [**基于表达式的语言**](https://course.rs/basic/base-type/statement-expression.html)。







## 其他笔记



#### 智能指针

可以参考：[如何理解智能指针？ - 胡昊的回答 - 知乎](https://www.zhihu.com/question/20368881/answer/14918675) ，相当易懂。另外，需要注意的是 由于是 2012 年的文章，里面还在使用 boost 库的版本，尤其是没有提到 `unique_ptr` 。所以可以看下 C++ Primer 的 第 12 章“动态内存”。

