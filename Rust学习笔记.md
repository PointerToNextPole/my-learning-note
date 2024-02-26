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



#### 下载依赖很慢或卡住？

在目前，大家还不需要自己搭建的镜像下载服务，因此只需知道下载依赖库的地址是 [crates.io](https://crates.io/)，是由 Rust 官方搭建的镜像下载和管理服务。

##### 修改 Rust 的下载镜像为国内的镜像地址

为了使用 `crates.io` 之外的注册服务，我们需要对 `$HOME/.cargo/config.toml` (`$CARGO_HOME` 下) 文件进行配置，添加新的服务提供商，<font color=dodgerBlue>有两种方式可以实现：增加新的镜像地址和覆盖默认的镜像地址</font>。

###### 新增镜像地址

**首先是在 `crates.io` 之外添加新的注册服务**，在 `$HOME/.cargo/config.toml` （如果文件不存在则手动创建一个）中添加以下内容：

```toml
[registries]
ustc = { index = "https://mirrors.ustc.edu.cn/crates.io-index/" }
```

<font color=red>这种方式只会新增一个新的镜像地址</font>，因此在引入依赖的时候，需要指定该地址，例如在项目中引入 `time` 包，你需要在 `Cargo.toml` 中使用以下方式引入:

```toml
[dependencies]
time = {  registry = "ustc" }
```

<font color=red>**在重新配置后，初次构建可能要较久的时间**</font>，因为要下载更新 `ustc` 注册服务的索引文件，由于文件比较大，需要等待较长的时间。

此处有两点需要注意：

1. cargo 1.68 版本开始支持稀疏索引，不再需要完整克隆 crates.io-index 仓库，可以加快获取包的速度，如：

   ```toml
   [source.ustc]
   registry = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"
   ```

2. cargo search 无法使用镜像

**科大镜像**

上面使用的是科大提供的注册服务，也是 Rust 最早期的注册服务

**字节跳动**

最大的优点就是不限速，当然，你的网速如果能跑到 1000Gbps，我们也可以认为它无情的限制了你，咳咳。

```toml
[source.crates-io]
replace-with = 'rsproxy'

[source.rsproxy]
registry = "https://rsproxy.cn/crates.io-index"

# 稀疏索引，要求 cargo >= 1.68
[source.rsproxy-sparse]
registry = "sparse+https://rsproxy.cn/index/"

[registries.rsproxy]
index = "https://rsproxy.cn/crates.io-index"

[net]
git-fetch-with-cli = true
```

###### 覆盖默认的镜像地址

<font color=red>我们更推荐第二种方式</font>，因为第一种方式在项目大了后，实在是很麻烦，全部修改后，万一以后不用这个镜像了，你又要全部修改成其它的。

<font color=red>而第二种方式，则不需要修改 `Cargo.toml` 文件</font>，**因为它是直接使用新注册服务来替代默认的 `crates.io`**。

在 `$HOME/.cargo/config.toml` 添加以下内容：

```toml
[source.crates-io]
replace-with = 'ustc'

[source.ustc]
registry = "git://mirrors.ustc.edu.cn/crates.io-index"
```

首先，创建一个新的镜像源 `[source.ustc]`，然后将默认的 `crates-io` 替换成新的镜像源: `replace-with = 'ustc'`。

只要这样配置后，以往需要去 `crates.io` 下载的包，会全部从科大的镜像地址下载

##### 下载卡住

下载卡住其实就一个原因：下载太慢了。根据经验来看，<font color=red>**卡住不动往往发生在更新索引时**</font>。<font color=red>毕竟 Rust 的包越来越多，索引也越来越大</font>，如果不使用国内镜像，卡住还蛮正常的；好在，我们也无需经常更新索引

###### Blocking waiting for file lock on package cache

不过这里有一个坑，需要大家注意，如果你同时打开了 VSCode 和命令行，然后修改了 `Cargo.toml`，此时 VSCode 的 rust-analyzer 插件会自动检测到依赖的变更，去下载新的依赖。

在 VSCode 下载的过程中( 特别是更新索引，可能会耗时很久)，假如你又在命令行中运行类似 `cargo run`或者 `cargo build` 的命令，就会提示一行有些看不太懂的内容：

```shell
$ cargo build
    Blocking waiting for file lock on package cache
    Blocking waiting for file lock on package cache
```

其实这个报错就是因为 VSCode 的下载太慢了，而且该下载构建还锁住了当前的项目，导致你无法在另一个地方再次进行构建。

<font color=dodgerBlue>解决办法也很简单：</font>

- 增加下载速度，见前面内容
- 耐心等待持有锁的用户构建完成
- 强行停止正在构建的进程，例如杀掉 IDE 使用的 rust-analyzer 插件进程，然后删除 `$HOME/.cargo/.package_cache` 目录



### Rust 基础入门

#### 变量绑定与解构

##### 为何要手动设置变量的可变性？

<font color=dodgerBlue>在其它大多数语言中，要么只支持声明可变的变量，要么只支持声明不可变的变量( 例如函数式语言 )</font>，前者为编程提供了灵活性，后者为编程提供了安全性，而 <font color=red>Rust 比较野，选择了两者我都要，既要灵活性又要安全性</font>。

**一切选择皆是权衡**，那么两者都要的权衡是什么呢？这就是 Rust 开发团队为我们做出的贡献，两者都要意味着 Rust 语言底层代码的实现复杂度大幅提升，因此 Salute to The Rust Team!

除了以上两个优点，<font color=dodgerBlue>还有一个很大的优点</font>，那就是运行性能上的提升，因为将本身无需改变的变量声明为不可变在运行期会避免一些多余的 `runtime` 检查。

##### 变量命名

在命名方面，和其它语言没有区别，不过当给变量命名时，需要遵循 [Rust 命名规范](https://course.rs/practice/naming.html)。

> Rust 语言有一些**关键字**（*keywords*），和其他语言一样，这些关键字都是被保留给 Rust 语言使用的，因此，它们不能被用作变量或函数的名称。在 [附录 A](https://course.rs/appendix/keywords.html) 中可找到关键字列表。

##### 变量绑定

在其它语言中，我们用 `var a = "hello world"` 的方式给 `a` 赋值，也就是把等式右边的 `"hello world"` 字符串赋值给变量 `a` ，而在 <font color=red>Rust 中，我们这样写： `let a = "hello world"`</font> ，同时<font color=fuchsia>给这个过程起了另一个名字：**变量绑定**</font>。

<font color=dodgerBlue>为何不用赋值而用绑定呢</font>（其实你<font color=lightSeaGreen>*也可以称之为赋值*</font>，<font color=fuchsia>但是绑定的含义更清晰准确</font>）？<font color=fuchsia>这里就涉及 Rust 最核心的原则——**所有权**</font>，<font color=dodgerBlue>简单来讲</font>，<font color=fuchsia>任何内存对象都是有主人的，而且一般情况下完全属于它的主人，绑定就是把这个对象绑定给一个变量，让这个变量成为它的主人</font>（聪明的读者应该能猜到，在这种情况下，<font color=fuchsia>**该对象之前的主人就会丧失对该对象的所有权**</font>），像极了我们的现实世界，不是吗？

##### 变量可变性

<font color=red>Rust 的变量在默认情况下是**不可变的**</font>。前文提到，这是 Rust 团队为我们精心设计的语言特性之一，让我们编写的代码更安全，性能也更好。当然你<font color=red>可以通过 `mut` 关键字让变量变为**可变的**</font>，让设计更灵活。

如果变量 `a` 不可变，那么一旦为它绑定值，就不能再修改 `a` 。比如如下代码：

```rust
fn main() {
    let x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```

使用 `cargo run` 运行它，迎面而来的是一条错误提示：

```console
$ cargo run
   Compiling variables v0.1.0 (file:///projects/variables)
error[E0384]: cannot assign twice to immutable variable `x`
 --> src/main.rs:4:5
  |
2 |     let x = 5;
  |         -
  |         |
  |         first assignment to `x`
  |         help: consider making this binding mutable: `mut x`
3 |     println!("The value of x is: {}", x);
4 |     x = 6;
  |     ^^^^^ cannot assign twice to immutable variable

error: aborting due to previous error
```

具体的错误原因是 `cannot assign twice to immutable variable x`（无法对不可变的变量进行重复赋值），因为我们想为不可变的 `x` 变量再次赋值。

这种错误是为了避免无法预期的错误发生在我们的变量上：<font color=red>一个变量往往被多处代码所使用，其中一部分代码假定该变量的值永远不会改变，而另外一部分代码却无情的改变了这个值</font>，<font color=lightSeaGreen>在实际开发过程中，这个错误是很难被发现的，**特别是在多线程编程中**</font>。

这种规则让我们的代码变得非常清晰，只有你想让你的变量改变时，它才能改变，这样就不会造成心智上的负担，也给别人阅读代码带来便利。

但是<font color=dodgerBlue>可变性也非常重要</font>，否则我们就要<font color=lightSeaGreen>像 ClojureScript 那样，每次要改变，就要重新生成一个对象，**在拥有大量对象的场景，性能会变得非常低下，内存拷贝的成本异常的高**</font>。

在 Rust 中，可变性很简单，只要在变量名前加一个 `mut` 即可，而且这种显式的声明方式还会给后来人传达这样的信息：嗯，这个变量在后面代码部分会发生改变。

> 💡 关于 `mut` 关键字，是否是修饰符
>
> <img src="https://s2.loli.net/2024/02/24/z1p9Vrq6YtLTMxK.png" alt="Snipaste_2024-02-24_16-26-59" style="zoom: 55%;" />

```rust
fn main() {
    let mut x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```

运行程序将得到下面结果：

```console
$ cargo run
   Compiling variables v0.1.0 (file:///projects/variables)
    Finished dev [unoptimized + debuginfo] target(s) in 0.30s
     Running `target/debug/variables`
The value of x is: 5
The value of x is: 6
```

选择可变还是不可变，更多的还是取决于使用场景，例如<font color=fuchsia>不可变可以带来安全性，但是丧失了灵活性和性能</font>（<font color=red>如果你要改变，就要重新创建一个新的变量，这里涉及到内存对象的再分配</font>）。而<font color=fuchsia>可变变量最大的好处就是使用上的灵活性和性能上的提升</font>。

例如，<font color=dodgerBlue>在使用大型数据结构或者热点代码路径（被大量频繁调用）的情形下</font>，<font color=red>在同一内存位置更新实例可能比复制并返回新分配的实例要更快</font>。<font color=dodgerBlue>使用较小的数据结构时</font>，<font color=red>通常创建新的实例并以更具函数式的风格来编写程序，可能会更容易理解，所以值得以较低的性能开销来确保代码清晰</font>。

##### 使用下划线开头忽略未使用的变量

如果你创建了一个变量却不在任何地方使用它，Rust 通常会给你一个警告，因为这可能会是个 BUG。但是有时创建一个不会被使用的变量是有用的，比如你正在设计原型或刚刚开始一个项目。这时**你希望告诉 Rust 不要警告未使用的变量，为此可以用下划线 `_` 作为变量名的开头**：

```rust
fn main() {
    let _x = 5;
    let y = 10;
}
```

使用 `cargo run` 运行下试试:

```shell
warning: unused variable: `y`
 --> src/main.rs:3:9
  |
3 |     let y = 10;
  |         ^ help: 如果 y 故意不被使用，请添加一个下划线前缀: `_y`
  |
  = note: `#[warn(unused_variables)]` on by default
```

可以看到，两个变量都是只有声明，没有使用，但是编译器却独独给出了 `y` 未被使用的警告，充分说明了 `_` 变量名前缀在这里发挥的作用。

<font color=dodgerBlue>值得注意的是</font>，<font color=red>这里编译器还很善意的给出了提示</font>（ Rust 的编译器非常强大，这里的提示只是小意思）: <font color=red>将 `y` 修改 `_y` 即可</font>。

更多关于 `_x` 的使用信息，请阅读后面的[模式匹配章节](https://course.rs/basic/match-pattern/all-patterns.html?highlight=_#使用下划线开头忽略未使用的变量)。

##### 变量解构

`let` 表达式不仅仅用于变量的绑定，还能进行复杂变量的解构：从一个相对复杂的变量中，匹配出该变量的一部分内容：

```rust
fn main() {
    let (a, mut b): (bool, bool) = (true, false);
    // a = true，不可变; b = false，可变
    println!("a = {:?}, b = {:?}", a, b);

    b = true;
    assert_eq!(a, b);
}
```

> 👀 注意这里类型标注的写法

###### 解构式赋值

在 [Rust 1.59](https://course.rs/appendix/rust-versions/1.59.html) 版本后，可以在赋值语句的左式中使用元组 ( tuple )、切片 ( slicing ) 和结构体 ( struct ) 模式了。

```rust
struct Struct {
    e: i32
}

fn main() {
    let (a, b, c, d, e);

    (a, b) = (1, 2);
    // _ 代表匹配一个值，但是我们不关心具体的值是什么，因此没有使用一个变量名而是使用了 _
    [c, .., d, _] = [1, 2, 3, 4, 5];
    Struct { e, .. } = Struct { e: 5 };

    assert_eq!([1, 2, 1, 4, 5], [a, b, c, d, e]);
}
```

这种使用方式跟之前的 `let` 保持了一致性，但是 <font color=fuchsia>**`let` 会重新绑定**</font>，而<font color=fuchsia>这里仅仅是对之前绑定的变量进行再赋值</font>

> 💡对于上面的解释，有点没看懂；于是问了下 GitHub Copilot，得到如下回复：
>
> <img src="https://s2.loli.net/2024/02/25/aARGOuNbTl2DPxs.png" alt="Snipaste_2024-02-24_16-26-39" style="zoom:50%;" />
>
> 算是有点懂了，不过看到 [[#变量遮蔽 ( shadowing )]] 才算真的搞懂是为什么

需要注意的是，使用 `+=` 的赋值语句还不支持解构式赋值。

##### 量和常量之间的差异

变量的值不能更改可能让你想起其他另一个很多语言都有的编程概念：**常量** ( *constant* ) 。与不可变变量一样，常量也是绑定到一个常量名且不允许更改的值，<font color=dodgerBlue>但是常量和变量之间存在一些差异</font>：

- <font color=red>常量不允许使用 `mut`</font> （👀 虽然这非常合理，但是还有必要注意一下。即：`const` 不可和 `mut` 一起使用）。**常量不仅仅默认不可变，而且自始至终不可变**，因为<font color=red>常量在编译完成后，已经确定它的值</font>。
- 常量使用 `const` 关键字而不是 `let` 关键字来声明，并且<font color=red>值的类型**必须**标注</font>。

下面是一个常量声明的例子，其常量名为 `MAX_POINTS`，值设置为 `100,000` 。（ <font color=red>Rust 常量的命名约定是全部字母都使用大写，并使用下划线分隔单词</font>，另外<font color=lightSeaGreen>对数字字面量可插入下划线以提高可读性</font>）：

```rust
const MAX_POINTS: u32 = 100_000;
```

常量可以在任意作用域内声明，包括全局作用域，在声明的作用域内，常量在程序运行的整个过程中都有效。对于需要在多处代码共享一个不可变的值时非常有用，例如游戏中允许玩家赚取的最大点数或光速。

> 在实际使用中，最好将程序中用到的硬编码值都声明为常量，对于代码后续的维护有莫大的帮助。如果将来需要更改硬编码的值，你也只需要在代码中更改一处即可。

##### 变量遮蔽 ( shadowing )

<font color=fuchsia>Rust 允许声明相同的变量名，在后面声明的变量会遮蔽掉前面声明的</font>，如下所示：

> 👀 这一点和 js 的 `let` 以及 `const` 很不一样 

```rust
fn main() {
    let x = 5;
    // 在main函数的作用域内对之前的x进行遮蔽
    let x = x + 1;

    {
        // 在当前的花括号作用域内，对之前的x进行遮蔽
        let x = x * 2;
        println!("The value of x in the inner scope is: {}", x);
    }

    println!("The value of x is: {}", x);
}
```

这个程序首先将数值 `5` 绑定到 `x`，然后通过重复使用 `let x =` 来遮蔽之前的 `x`，并取原来的值加上 `1`，所以 `x` 的值变成了 `6`。第三个 `let` 语句同样遮蔽前面的 `x`，取之前的值并乘上 `2`，得到的 `x` 最终值为 `12`。当运行此程序，将输出以下内容：

```console
$ cargo run
   Compiling variables v0.1.0 (file:///projects/variables)
   ...
The value of x in the inner scope is: 12
The value of x is: 6
```

这和 `mut` 变量的使用是不同的，<font color=red>第二个 `let` 生成了 **完全不同的新变量**</font>，<font color=fuchsia>**两个变量只是恰好拥有同样的名称**，涉及一次内存对象的再分配</font> ，而 <font color=dodgerBlue>`mut` 声明的变量</font>，<font color=red>可以修改同一个内存地址上的值，并不会发生内存对象的再分配，性能要更好</font>。

变量遮蔽的用处在于，如果你在某个作用域内无需再使用之前的变量（在被遮蔽后，无法再访问到之前的同名变量），就可以重复的使用变量名字，而不用绞尽脑汁去想更多的名字。

例如，假设有一个程序要统计一个空格字符串的空格数量：

```rust
// 字符串类型
let spaces = "   ";
// usize 数值类型
let spaces = spaces.len();
```

这种结构是允许的，因为第一个 `spaces` 变量是一个字符串类型，<font color=red>第二个 `spaces` 变量</font> **是一个全新的变量** 且和第一个具有相同的变量名，且是一个数值类型。所以变量遮蔽可以帮我们节省些脑细胞，不用去想如 `spaces_str` 和 `spaces_num` 此类的变量名（👀 有 **一点点类似于** 函数名重载）；相反我们可以重复使用更简单的 `spaces` 变量名。如果你不用 `let` ：

```rust
let mut spaces = "   ";
spaces = spaces.len();
```

运行一下，你就会发现编译器报错：

```console
$ cargo run
   Compiling variables v0.1.0 (file:///projects/variables)
error[E0308]: mismatched types
 --> src/main.rs:3:14
  |
3 |     spaces = spaces.len();
  |              ^^^^^^^^^^^^ expected `&str`, found `usize`

error: aborting due to previous error
```

显然，Rust 对类型的要求很严格，<font color=red>不允许将整数类型 `usize` **赋值** 给字符串类型</font>。`usize` 是一种 CPU 相关的整数类型。

> ⚠️ 注意这里对于 `mut` 对应的是对变量赋值，而“变量遮蔽”的则是新创建一个变量；前者有类型相同的约束，后者没有

#### 基本类型

Rust 每个值都有其确切的数据类型，<font color=dodgerBlue>总的来说可以分为两类</font>：基本类型和复合类型。 基本类型意味着它们往往是一个最小化原子类型，无法解构为其它类型(一般意义上来说)，由以下组成：

- 数值类型: 有符号整数 (`i8`, `i16`, `i32`, `i64`, `isize`)、 无符号整数 (`u8`, `u16`, `u32`, `u64`, `usize`) 、浮点数 (`f32`, `f64`)、<font color=red>以及有理数、复数</font>

- 字符串：字符串字面量和<font color=red>字符串切片 `&str`</font>

  > 💡 看完了这一整节，才发现：本节几乎没有介绍字符串类型，这里做一下补充：
  >
  > <img src="https://s2.loli.net/2024/02/26/cDkYfazgIX1rLRv.png" alt="image-20240226165302892" style="zoom:60%;" />
  >
  > <img src="https://s2.loli.net/2024/02/26/IuF8G6CDdq4mica.png" alt="image-20240226165346266" style="zoom:50%;" />

- 布尔类型： `true` 和 `false`

- 字符类型：<font color=red>表示**单个 Unicode 字符，存储为 4 个字节**</font>

- 单元类型：即 `()` ，其唯一的值也是 `()`

  > 👀 之前完全没有听过 “单元类型” ，就准备了解一下：
  >
  > <img src="https://s2.loli.net/2024/02/25/kloOMHVx9jsAqNu.png" alt="image-20240225011350388" style="zoom: 60%;" />
  >
  > 从上面的描述而言，感觉 “单元类型” 和 c-like 语言中的 `void` 类型很像（另外，可以明确的说：Rust 中没有 void 类型）
  >
  > <img src="https://s2.loli.net/2024/02/25/StbwXR6EjimgKZq.png" alt="image-20240225011604461" style="zoom:60%;" />

##### 类型推导与标注

与 Python、JavaScript 等动态语言不同，<font color=dodgerBlue>Rust 是一门静态类型语言</font>，也就是<font color=red>**编译器必须在编译期知道我们所有变量的类型**</font>，但<font color=lightSeaGreen>这不意味着你需要为每个变量指定类型</font>，因为 **<font color=dodgerBlue>Rust 编译器很聪明</font>，它<font color=red>可以根据变量的值和上下文中的使用方式来自动推导出变量的类型</font>**，同时<font color=dodgerBlue>编译器也不够聪明</font>，<font color=fuchsia>在某些情况下，它无法推导出变量类型，需要手动去给予一个类型标注</font>

来看段代码：

```rust
let guess = "42".parse().expect("Not a number!");
```

先忽略 `.parse().expect..` 部分，这段代码的目的是将字符串 `"42"` 进行解析，而编译器在这里无法推导出我们想要的类型：整数？浮点数？字符串？因此编译器会报错：

```console
$ cargo build
   Compiling no_type_annotations v0.1.0 (file:///projects/no_type_annotations)
error[E0282]: type annotations needed
 --> src/main.rs:2:9
  |
2 |     let guess = "42".parse().expect("Not a number!");
  |         ^^^^^ consider giving `guess` a type
```

因此我们需要提供给编译器更多的信息，例如给 `guess` 变量一个**显式的类型标注**：`let guess: i32 = "42".parse().expect("Not a number!");` 或者 `let guess = "42".parse::<i32>().expect("Not a number!");` 

#### 数值类型

Rust 使用一个相对传统的语法来创建整数（`1`，`2`，...）和浮点数（`1.0`，`1.1`，...）。整数、浮点数的运算和你在其它语言上见过的一致，都是通过常见的运算符来完成。

> 不仅仅是数值类型，<font color=red>Rust 也允许在复杂类型上定义运算符，例如在自定义类型上定义 `+` 运算符，这种行为被称为运算符重载</font>，Rust 具体支持的可重载运算符见[附录 B](https://course.rs/appendix/operators.html#运算符) 。

##### 整数类型

**整数**是没有小数部分的数字。之前使用过的 `i32` 类型，表示有符号的 32 位整数（ `i` 是英文单词 *integer*的首字母，与之相反的是 `u`，代表无符号 `unsigned` 类型）。下表显示了 Rust 中的内置的整数类型：

| 长度                              | 有符号类型 | 无符号类型 |
| --------------------------------- | ---------- | ---------- |
| 8 位                              | `i8`       | `u8`       |
| 16 位                             | `i16`      | `u16`      |
| 32 位                             | `i32`      | `u32`      |
| 64 位                             | `i64`      | `u64`      |
| 128 位                            | `i128`     | `u128`     |
| <font color=red>视架构而定</font> | `isize`    | `usize`    |

类型定义的形式统一为：`有无符号 + 类型大小(位数)`。**无符号数**表示数字只能取正数和0，而**有符号**则表示数字可以取正数、负数还有0。就像在纸上写数字一样：当要强调符号时，数字前面可以带上正号或负号；然而，当很明显确定数字为正数时，就不需要加上正号了。有符号数字以[补码](https://en.wikipedia.org/wiki/Two's_complement)形式存储。

<font color=lightSeaGreen>每个有符号类型规定的数字范围是 -($2^{n - 1}$) ~ $2^{n -
1} - 1$，其中 `n` 是该定义形式的位长度</font>。因此 `i8` 可存储数字范围是 -($2^7$) ~ $2^7 - 1$，即 -128 ~ 127。无符号类型可以存储的数字范围是 0 ~ $2^n - 1$，所以 `u8` 能够存储的数字为 0 ~ $2^8 - 1$，即 0 ~ 255。

此外，<font color=red>`isize` 和 `usize` 类型取决于程序运行的计算机 CPU 类型</font>： 若 CPU 是 32 位的，则这两个类型是 32 位的，同理，若 CPU 是 64 位，那么它们则是 64 位。

> 💡 关于 `isize` 和 `usize` 在 cpp 中的类型
>
> <img src="https://s2.loli.net/2024/02/25/NPLoB6Wzbqtdk98.png" alt="image-20240225174603926" style="zoom:60%;" />
>
> <img src="https://s2.loli.net/2024/02/25/MCp7NmRWhB3nd5v.png" alt="image-20240225174811372" style="zoom:60%;" />

<font color=dodgerBlue>整形字面量可以用下表的形式书写：</font>

| 数字字面量                                      | 示例          |
| ----------------------------------------------- | ------------- |
| 十进制                                          | `98_222`      |
| 十六进制                                        | `0xff`        |
| 八进制                                          | `0o77`        |
| 二进制                                          | `0b1111_0000` |
| 字节（<font color=red>**仅限于 `u8`**</font> ） | `b'A'`        |

这么多类型，有没有一个简单的使用准则？答案是肯定的， <font color=red>Rust 整型默认使用 `i32`</font>，<font color=lightSeaGreen>例如 `let i = 1`，那 `i` 就是 `i32` 类型</font>，因此<font color=fuchsia>你可以首选它，同时**该类型也往往是性能最好的**</font>。<font color=dodgerBlue>`isize` 和 `usize` 的主要应用场景</font>是<font color=red>用作集合的索引</font>。

###### 整型溢出

<font color=dodgerBlue>假设有一个 `u8` ，它可以存放从 0 到 255 的值</font>。那么<font color=red>当你将其修改为范围之外的值，比如 256，则会发生**整型溢出**</font>。<font color=dodgerBlue>关于这一行为 Rust 有一些有趣的规则</font>：<font color=fuchsia>当在 debug 模式编译时，Rust 会检查整型溢出</font>，若存在这些问题，则使程序在编译时 *panic*（崩溃，Rust 使用这个术语来表明程序因错误而退出）。

> 💡关于 panic 的补充：
>
> ![image-20240225175608471](https://s2.loli.net/2024/02/25/ZyoXQrUlJBAt2jT.png)
>
> ![image-20240225180546552](https://s2.loli.net/2024/02/25/thcHdyvj1ikBPfw.png)

<font color=dodgerBlue>**在当使用 `--release` 参数进行 release 模式构建时**</font>，<font color=red>**Rust **不**检测溢出**</font>。相反，<font color=dodgerBlue>当检测到整型溢出时</font>，<font color=fuchsia>Rust 会按照补码循环溢出 ( *two’s complement wrapping* ) 的规则处理</font>。<font color=dodgerBlue>简而言之</font>，<font color=red>大于该类型最大值的数值会被补码转换成该类型能够支持的对应数字的最小值</font>。比如<font color=lightSeaGreen>在 `u8` 的情况下，256 变成 0，257 变成 1，依此类推</font>。程序不会 *panic*，但是该变量的值可能不是你期望的值。<font color=red>依赖这种默认行为的代码都应该被认为是错误的代码</font>。

<font color=dodgerBlue>要显式处理可能的溢出，可以使用标准库针对原始数字类型提供的这些方法</font>：

- 使用 `wrapping_*` 方法在所有模式下都按照补码循环溢出规则处理，例如 `wrapping_add`
- 如果使用 `checked_*` 方法时<font color=red>发生溢出，则返回 `None` 值</font>
- 使用 `overflowing_*` 方法<font color=red>返回该值和一个指示是否存在溢出的布尔值</font>
- 使用 `saturating_*` 方法，可以<font color=red>限定计算后的结果不超过目标类型的最大值或低于最小值</font>，例如:

```rust
assert_eq!(100u8.saturating_add(1), 101);
assert_eq!(u8::MAX.saturating_add(127), u8::MAX);
```

下面是一个演示`wrapping_*`方法的示例：

```rust
fn main() {
    let a : u8 = 255;
    let b = a.wrapping_add(20);
    println!("{}", b);  // 19
}
```

##### 浮点类型

**浮点类型数字** 是带有小数点的数字，在 <font color=dodgerBlue>Rust 中浮点类型数字也有**两种基本类型**</font>：<font color=red> `f32` 和 `f64`</font>，分别为 32 位和 64 位大小。<font color=fuchsia>默认浮点类型是 `f64`</font>，在现代的 CPU 中它的速度与 `f32` 几乎相同，但精度更高。

下面是一个演示浮点数的示例：

```rust
fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}
```

浮点数根据 `IEEE-754` 标准实现。`f32` 类型是单精度浮点型，`f64` 为双精度。

###### 浮点数陷阱

浮点数由于底层格式的特殊性，导致了<font color=red>如果在使用浮点数时不够谨慎，就可能造成危险</font>，<font color=dodgerBlue>有两个原因</font>：

1. <font color=red>**浮点数往往是你想要数字的近似表达**</font> 浮点数类型是基于二进制实现的，但是我们想要计算的数字往往是基于十进制，例如 `0.1` 在二进制上并不存在精确的表达形式，但是在十进制上就存在。这种不匹配性导致一定的歧义性，更多的，虽然浮点数能代表真实的数值，但是<font color=lightSeaGreen>由于底层格式问题，它往往受限于定长的浮点数精度，如果你想要表达完全精准的真实数字，**只有使用无限精度的浮点数才行**</font>
2. **浮点数在某些特性上是反直觉的** 例如大家都会觉得浮点数可以进行比较，对吧？是的，<font color=dodgerBlue>它们确实可以使用 `>`，`>=` 等进行比较</font>，但是在某些场景下，这种直觉上的比较特性反而会害了你。因为 <font color=fuchsia>`f32` ， `f64` 上的比较运算**实现的是 `std::cmp::PartialEq` 特征**（类似其他语言的接口）</font>，但是<font color=red>并没有实现 `std::cmp::Eq` 特征</font>，但是后者在其它数值类型上都有定义，说了这么多，可能大家还是云里雾里，用一个例子来举例：

Rust 的 `HashMap` 数据结构，是一个 KV 类型的 Hash Map 实现，<font color=lightSeaGreen>它对于 `K` 没有特定类型的限制</font>，但是<font color=red>要求能用作 `K` 的类型必须实现了 `std::cmp::Eq` 特征，因此这意味着你无法使用浮点数作为 `HashMap` 的 `Key`，来存储键值对</font>，但是作为对比，Rust 的整数类型、字符串类型、布尔类型都实现了该特征，因此可以作为 `HashMap` 的 `Key`。

<font color=dodgerBlue>为了避免上面说的两个陷阱，你**需要遵守以下准则**</font>：

- 避免在浮点数上测试相等性
- 当结果在数学上可能存在未定义时，需要格外的小心

来看个小例子:

```rust
fn main() {
  // 断言0.1 + 0.2与0.3相等
  assert!(0.1 + 0.2 == 0.3);
}
```

你可能以为，这段代码没啥问题吧，实际上它会 *panic*（程序崩溃，抛出异常），因为二进制精度问题，导致了 0.1 + 0.2 并不严格等于 0.3，它们可能在小数点 N 位后存在误差。

那如果非要进行比较呢？可以考虑用这种方式 `(0.1_f64 + 0.2 - 0.3).abs() < 0.00001` ，具体小于多少，取决于你对精度的需求。

> 💡有点类似于 JS 中的 `Number.EPSILON` 的作用，可以参考 [[JS 机制与原理#IEEE 754 标准在 JS 数字存储 中的使用]] 中的内容。

讲到这里，相信大家基本已经明白了，为什么操作浮点数时要格外的小心，但是还不够，下面再来一段代码，直接震撼你的灵魂：

```rust
fn main() {
    let abc: (f32, f32, f32) = (0.1, 0.2, 0.3);
    let xyz: (f64, f64, f64) = (0.1, 0.2, 0.3);

    println!("abc (f32)");
    println!("   0.1 + 0.2: {:x}", (abc.0 + abc.1).to_bits());
    println!("         0.3: {:x}", (abc.2).to_bits());
    println!();

    println!("xyz (f64)");
    println!("   0.1 + 0.2: {:x}", (xyz.0 + xyz.1).to_bits());
    println!("         0.3: {:x}", (xyz.2).to_bits());
    println!();

    assert!(abc.0 + abc.1 == abc.2);
    assert!(xyz.0 + xyz.1 == xyz.2);
}
```

运行该程序，输出如下:

```rust
abc (f32)
   0.1 + 0.2: 3e99999a
         0.3: 3e99999a

xyz (f64)
   0.1 + 0.2: 3fd3333333333334
         0.3: 3fd3333333333333

thread 'main' panicked at 'assertion failed: xyz.0 + xyz.1 == xyz.2',
➥ch2-add-floats.rs.rs:14:5
note: run with `RUST_BACKTRACE=1` environment variable to display
➥a backtrace
```

仔细看，对 `f32` 类型做加法时，`0.1 + 0.2` 的结果是 `3e99999a`，`0.3` 也是 `3e99999a`，因此 `f32` 下的 `0.1 + 0.2 == 0.3` 通过测试，但是<font color=dodgerBlue>到了 `f64` 类型时，结果就不一样了</font>，<font color=lightSeaGreen>因为 `f64` 精度高很多，因此在小数点非常后面发生了一点微小的变化</font>，`0.1 + 0.2` 以 `4` 结尾，但是 `0.3` 以`3`结尾，这个细微区别导致 `f64` 下的测试失败了，并且抛出了异常。

###### NaN

对于数学上未定义的结果，例如对负数取平方根 `-42.1.sqrt()` ，会产生一个特殊的结果：Rust 的浮点数类型使用 `NaN` (not a number) 来处理这些情况。

**所有跟 `NaN` 交互的操作，都会返回一个 `NaN`**（💡 这和 [[现代 JavaScript 教程 阅读笔记#NaN]] 中所说 “NaN 是粘性的。任何对 NaN 的进一步操作都会返回 NaN”，意思是一样的），而且 `NaN` 不能用来比较，下面的代码会崩溃：

```rust
fn main() {
  let x = (-42.0_f32).sqrt();
  assert_eq!(x, x);
}
```

<font color=dodgerBlue>**出于防御性编程的考虑**</font>，<font color=red>可以使用 `is_nan()` 等方法</font>，可以用来判断一个数值是否是 `NaN` ：

```rust
fn main() {
    let x = (-42.0_f32).sqrt();
    if x.is_nan() {
        println!("未定义的数学行为")
    }
}
```

##### 数字运算

Rust 支持所有数字类型的基本数学运算：加法、减法、乘法、除法和取模运算。下面代码各使用一条 `let`语句来说明相应运算的用法：

```rust
fn main() {
    // 加法
    let sum = 5 + 10;
    // 减法
    let difference = 95.5 - 4.3;
    // 乘法
    let product = 4 * 30;
    // 除法
    let quotient = 56.7 / 32.2;
    // 求余
    let remainder = 43 % 5;
}
```

这些语句中的每个表达式都使用了数学运算符，并且计算结果为一个值，然后绑定到一个变量上。[附录 B](https://course.rs/appendix/operators.html#运算符)中给出了 Rust 提供的所有运算符的列表。

再来看一个综合性的示例：

```rust
fn main() {
  // 编译器会进行自动推导，给予twenty i32的类型
  let twenty = 20;
  // 类型标注
  let twenty_one: i32 = 21;
  // ⭐️ 通过类型后缀的方式进行类型标注：22是i32类型
  let twenty_two = 22i32;

  // 只有同样类型，才能运算
  let addition = twenty + twenty_one + twenty_two;
  println!("{} + {} + {} = {}", twenty, twenty_one, twenty_two, addition);

  // 对于较长的数字，可以用_进行分割，提升可读性
  let one_million: i64 = 1_000_000;
  println!("{}", one_million.pow(2));

  // 定义一个f32数组，其中42.0会自动被推导为f32类型
  let forty_twos = [
    42.0,
    42f32,
    42.0_f32,
  ];

  // 打印数组中第一个值，并控制小数位为2位
  println!("{:.2}", forty_twos[0]);
}
```

> 💡 补充
>
> 关于 `22i32` 这种 “类型后缀的方式进行类型标注” 的方法，在 cpp / java 中也有类似的，甚至感觉存在借鉴关系。另外，rust 做了更进一步改动：
>
> <img src="https://s2.loli.net/2024/02/25/y86cX9zFM1LKDfU.png" alt="image-20240225192459253" style="zoom:55%;" />
>
> 另外，顺带提及一下cpp 中的 “指定字面量类型”，如下摘自 C++ Primer E5 P37
>
> <img src="https://s2.loli.net/2024/02/25/AbQtT73qBafiroZ.png" alt="image-20240225193415080" style="zoom:50%;" />

##### 位运算

Rust 的位运算基本上和其他语言一样

| 运算符    | 说明                                                   |
| --------- | ------------------------------------------------------ |
| `&` 位与  | 相同位置均为 1 时则为 1，否则为 0                      |
| `|` 位或  | 相同位置只要有 1 时则为 1，否则为 0                    |
| `^` 异或  | 相同位置不相同则为 1，相同则为 0                       |
| `!` 位非  | 把位中的 0 和 1 相互取反，即 0 置为 1，1 置为 0        |
| `<<` 左移 | 所有位向左移动指定位数，右位补 0                       |
| `>>` 右移 | 所有位向右移动指定位数，带符号移动（正数补0，负数补1） |

```rust
fn main() {
    // 二进制为00000010
    let a:i32 = 2;
    // 二进制为00000011
    let b:i32 = 3;

    println!("(a & b) value is {}", a & b);
    println!("(a | b) value is {}", a | b);
    println!("(a ^ b) value is {}", a ^ b);
    println!("(!b) value is {} ", !b);
    println!("(a << b) value is {}", a << b);
    println!("(a >> b) value is {}", a >> b);

    let mut a = a;
    // 注意这些计算符除了!之外都可以加上=进行赋值 (因为!=要用来判断不等于)
    a <<= b;
    println!("(a << b) value is {}", a);
}
```

##### 序列 ( Range )

> 💡 应该是借鉴的 Python

Rust 提供了一个非常简洁的方式，用来生成连续的数值，例如 `1..5`，生成从 1 到 4 的连续数字，不包含 5 ；`1..=5`，生成从 1 到 5 的连续数字，包含 5，它的用途很简单，常常用于循环中：

```rust
for i in 1..=5 {
    println!("{}",i);
}
```

<font color=red>序列只允许用于 **数字** 或 **字符** 类型</font>，<font color=dodgerBlue>原因是</font>：<font color=red>它们可以连续</font>，同时编译器在编译期可以检查该序列是否为空，字符和数字值是 Rust 中仅有的可以用于判断是否为空的类型。如下是一个使用字符类型序列的例子：

```rust
for i in 'a'..='z' {
    println!("{}",i);
}
```

##### 使用 `as` 完成类型转换

Rust 中可以使用 `as` 来完成一个类型到另一个类型的转换，其<font color=dodgerBlue>最常用于</font> <font color=red>将原始类型转换为其他原始类型</font>，但是它<font color=fuchsia>也可以完成诸如 将指针转换为地址、地址转换为指针 以及 将指针转换为其他指针等功能</font>。

> 💡 `as` 即强制类型转换
>
> ![image-20240225194442523](https://s2.loli.net/2024/02/25/axnYbOuw2HKEyZv.png)

##### 有理数和复数

Rust 的标准库相比其它语言，准入门槛较高，因此有理数和复数并未包含在标准库中：

- 有理数和复数
- 任意大小的整数和任意精度的浮点数
- 固定精度的十进制小数，常用于货币相关的场景

好在社区已经开发出高质量的 Rust 数值库：[num](https://crates.io/crates/num)。

按照以下步骤来引入 `num` 库：

1. 创建新工程 `cargo new complex-num && cd complex-num`
2. 在 `Cargo.toml` 中的 `[dependencies]` 下添加一行 `num = "0.4.0"`
3. 将 `src/main.rs` 文件中的 `main` 函数替换为下面的代码
4. 运行 `cargo run`

```rust
use num::complex::Complex;

 fn main() {
   let a = Complex { re: 2.1, im: -1.2 };
   let b = Complex::new(11.1, 22.2);
   let result = a + b;

   println!("{} + {}i", result.re, result.im)
 }
```

> 💡 上面分别介绍了复数的两种定义方法
>
> 另外，上面 `re` 和 `im` 分别代表 “real part” 和 “imaginary part” ，即 “实部” 和 “虚部”

##### 总结

之前提到了过 Rust 的数值类型和运算跟其他语言较为相似，但是实际上，<font color=dodgerBlue>除了语法上的不同之外，还是存在一些差异点</font>：

- **Rust 拥有相当多的数值类型**。因此你需要熟悉这些类型所占用的字节数，这样就知道该类型允许的大小范围以及你选择的类型是否能表达负数
- **类型转换必须是显式的**。Rust 永远也不会偷偷把你的 16bit 整数转换成 32bit 整数
- <font color=red>**Rust 的数值上可以使用方法**</font>。例如你可以用以下方法来将 `13.14` 取整：`13.14_f32.round()`，在这里我们使用了类型后缀，因为编译器需要知道 `13.14` 的具体类型

#### 字符、布尔、单元类型

##### 字符类型 ( char )

```rust
fn main() {
    let c = 'z';
    let z = 'ℤ';
    let g = '国';
    let heart_eyed_cat = '😻';
}
```

Rust 的字符不仅仅是 `ASCII`，所有的 `Unicode` 值都可以作为 Rust 字符。`Unicode` 值的范围从 `U+0000 ~ U+D7FF` 和 `U+E000 ~ U+10FFFF`。

由于 `Unicode` 都是 4 个字节编码，因此字符类型也是占用 4 个字节：

```rust
fn main() {
    let x = '中';
    println!("字符'中'占用了{}字节的内存大小",std::mem::size_of_val(&x));
}
```

输出如下:

```shell
$ cargo run
   Compiling ...

字符'中'占用了4字节的内存大小
```

> ⚠️ 和一些语言不同，<font color=fuchsia>Rust 的字符只能用 `''` 来表示， `""` 是留给字符串的</font>。
>
> 👀 上面也说明了在 Rust 中，`""` 和 `''` 是不能混用的，两者有严格的区分。

##### 布尔 ( bool )

Rust 中的布尔类型有两个可能的值：`true` 和 `false`，布尔值占用内存的大小为 `1` 个字节：

```rust
fn main() {
    let t = true;

    let f: bool = false; // 使用类型标注，显式指定f的类型
    if f {
        println!("这是段毫无意义的代码");
    }
}
```

使用布尔类型的场景主要在于流程控制，例如上述代码的中的 `if` 就是其中之一。

##### 单元类型

单元类型就是 `()` ，对，你没看错，就是 `()` ，唯一的值也是 `()` ，一些读者读到这里可能就不愿意了，你也太敷衍了吧，管这叫类型？

只能说，再不起眼的东西，都有其用途，在目前为止的学习过程中，大家已经看到过很多次 `fn main()` 函数的使用吧？那么这个函数返回什么呢？

没错，<font color=red> `main` 函数就返回这个单元类型 `()`</font>，你不能说 `main` 函数无返回值，因为<font color=red>没有返回值的函数在 Rust 中是有单独的定义的</font>：<font color=fuchsia>发散函数 ( diverge function )，顾名思义，无法收敛的函数</font>。

例如常见的 `println!()` 的返回值也是单元类型 `()` 。

再比如，你<font color=fuchsia>可以用 `()` 作为 `map` 的值，表示我们不关注具体的值，只关注 `key`</font>。 这种用法和 Go 语言的 **`struct{}`** 类似，可以<font color=fuchsia>作为一个值用来占位，但是完全**不占用**任何内存</font>。

#### 语句和表达式

Rust 的函数体是由一系列语句组成，<font color=fuchsia>最后由一个表达式来返回值</font>，例如：

```rust
fn add_with_extra(x: i32, y: i32) -> i32 {
    let x = x + 1; // 语句
    let y = y + 5; // 语句
    x + y // 表达式
}
```

语句会执行一些操作但是不会返回一个值，而表达式会在求值后返回一个值，因此在上述函数体的三行代码中，前两行是语句，最后一行是表达式。

对于 Rust 语言而言，**这种基于语句 ( statement ) 和表达式 ( expression ) 的方式是非常重要的，你需要能明确的区分这两个概念**，但是对于很多其它语言而言，这两个往往无需区分。基于表达式是函数式语言的重要特征，**表达式总要返回值**。

##### 语句

```rust
let a = 8;
let b: Vec<f64> = Vec::new();
let (a, c) = ("hi", false);
```

以上都是语句，它们完成了一个具体的操作，但是并没有返回值，因此是语句。

由于 `let` 是语句，因此不能将 `let` 语句赋值给其它值，如下形式是错误的：

```rust
let b = (let a = 8);
```

错误如下：

```rust
error: expected expression, found statement (`let`) // 期望表达式，却发现`let`语句
 --> src/main.rs:2:13
  |
2 |     let b = let a = 8;
  |             ^^^^^^^^^
  |
  = note: variable declaration using `let` is a statement `let`是一条语句

error[E0658]: `let` expressions in this position are experimental
          // 下面的 `let` 用法目前是试验性的，在稳定版中尚不能使用
 --> src/main.rs:2:13
  |
2 |     let b = let a = 8;
  |             ^^^^^^^^^
  |
  = note: see issue #53667 <https://github.com/rust-lang/rust/issues/53667> for more information
  = help: you can write `matches!(<expr>, <pattern>)` instead of `let <pattern> = <expr>`
```

以上的错误告诉我们 `let` 是语句，不是表达式，因此它不返回值，也就不能给其它变量赋值。但是该错误还透漏了一个重要的信息， `let` 作为表达式已经是试验功能了，也许不久的将来，我们在 [`stable rust`](https://course.rs/appendix/rust-version.html)下可以这样使用。a

> 👀 试了下，在当前 ( 2024/2/25 ) ，cargo 1.76.0 的版本下 `let b = let a = 8;` 语句并不能成功运行，运行结果如下：
>
> ![image-20240225210432675](https://s2.loli.net/2024/02/25/Mer6aQI2q7LoR9P.png)

##### 表达式

表达式会进行求值，然后返回一个值。例如 `5 + 6`，在求值后，返回值 `11`，因此它就是一条表达式。

表达式可以成为语句的一部分，例如 `let y = 6` 中，`6` 就是一个表达式，它在求值后返回一个值 `6`（有些反直觉，但是确实是表达式）。

<font color=red>**调用一个函数是表达式**</font>，因为会返回一个值，调用宏也是表达式，用花括号包裹最终返回一个值的语句块也是表达式，总之，<font color=fuchsia>**能返回值，它就是表达式**</font>（👀 比如下面的 `let y = { let x = 3; x + 1 }`）：

```rust
fn main() {
    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {}", y);
}
```

上面使用一个语句块表达式将值赋给 `y` 变量，语句块长这样：

```rust
{
    let x = 3;
    x + 1
}
```

<font color=red>该语句块是表达式的原因是：它的最后一行是表达式，返回了 `x + 1` 的值</font>，<font color=fuchsia>注意 `x + 1` 不能以分号结尾，否则就会从表达式变成语句， **表达式不能包含分号**</font>。这一点非常重要，<font color=red>一旦你在表达式后加上分号，它就会变成一条语句，再也**不会**返回一个值</font>，请牢记！

最后，<font color=red>如果表达式不返回任何值，会隐式地返回一个 `()`</font> 。

```rust
fn main() {
    assert_eq!(ret_unit_type(), ())
}

fn ret_unit_type() {
    let x = 1;
    // if 语句块也是一个表达式，因此可以用于赋值，也可以直接返回
    // 类似三元运算符，在Rust里我们可以这样写
    let y = if x % 2 == 1 {
        "odd"
    } else {
        "even"
    };
    // 或者写成一行
    let z = if x % 2 == 1 { "odd" } else { "even" };
}
```

#### 函数

```rust
fn add(i: i32, j: i32) -> i32 {
   i + j
 }
```

该函数如此简单，但是又是如此的五脏俱全，声明函数的关键字 `fn` ，函数名 `add()`，参数 `i` 和 `j`，参数类型和返回值类型都是 `i32` 。

<img src="https://s2.loli.net/2024/02/25/RtgE1LyMul7dTbc.png" alt="img" style="zoom:80%;" />

##### 函数要点

- 函数名和变量名使用[蛇形命名法(snake case)](https://course.rs/practice/naming.html)，例如 `fn add_two() -> {}`

  > 💡 这点应该是借鉴了 Python
  >
  > ![image-20240225213407803](https://s2.loli.net/2024/02/25/WmqekD7r32M1Abf.png)

- 函数的位置可以随便放，Rust 不关心我们在哪里定义了函数，只要有定义即可

  > 💡 补充
  >
  > ![image-20240225213702443](https://s2.loli.net/2024/02/25/UPEYwAB75gmsHxK.png)

- 每个函数参数都需要标注类型

  > 💡补充，看到这里想到了“函数签名”；顺带想到了“函数重载”，也了解到：Rust 是不支持函数重载的
  >
  > <img src="https://s2.loli.net/2024/02/25/LBX6rKgfhAs7OiY.png" alt="image-20240225213917632" style="zoom:60%;" />

##### 函数参数

Rust 是强类型语言，因此需要你为每一个函数参数都标识出它的具体类型，例如：

```rust
fn main() {
    another_function(5, 6.1);
}

fn another_function(x: i32, y: f32) {
    println!("The value of x is: {}", x);
    println!("The value of y is: {}", y);
}
```

`another_function` 函数有两个参数，其中 `x` 是 `i32` 类型，`y` 是 `f32` 类型，然后在该函数内部，打印出这两个值。<font color=dodgerBlue>这里去掉 `x` 或者 `y` 的任何一个的类型，都会报错：</font>

```rust
fn main() {
    another_function(5, 6.1);
}

fn another_function(x: i32, y) {
    println!("The value of x is: {}", x);
    println!("The value of y is: {}", y);
}
```

错误如下：

```rust
error: expected one of `:`, `@`, or `|`, found `)`
 --> src/main.rs:5:30
  |
5 | fn another_function(x: i32, y) {
  |                              ^ expected one of `:`, `@`, or `|` // 期待以下符号之一 `:`, `@`, or `|`
  |
  = note: anonymous parameters are removed in the 2018 edition (see RFC 1685)
    // 匿名参数在 Rust 2018 edition 中就已经移除
help: if this is a parameter name, give it a type // 如果y是一个参数名，请给予它一个类型
  |
5 | fn another_function(x: i32, y: TypeName) {
  |                             ~~~~~~~~~~~
help: if this is a type, explicitly ignore the parameter name // 如果y是一个类型，请使用_忽略参数名
  |
5 | fn another_function(x: i32, _: y) {
  |                             ~~~~
```

##### 函数返回

在上一章节语句和表达式中，我们有提到，在 <font color=fuchsia size=4>**Rust 中函数就是表达式**</font>，因此我们可以把函数的返回值直接赋给调用者。

函数的返回值就是函数体最后一条表达式的返回值，当然我们<font color=fuchsia>也可以使用 `return` 提前返回</font>（👀 所以说：在 Rust 中也是存在 `return` 关键字的），下面的函数使用最后一条表达式来返回一个值：

```rust
fn plus_five(x:i32) -> i32 {
    x + 5
}

fn main() {
    let x = plus_five(5);

    println!("The value of x is: {}", x);
}
```

`x + 5` 是一条表达式，求值后，返回一个值，因为它是函数的最后一行，因此该表达式的值也是函数的返回值。

<font color=dodgerBlue>再来看两个重点：</font>

1. `let x = plus_five(5)` ，说明我们用一个函数的返回值来初始化 `x` 变量，因此侧面说明了在 Rust 中函数也是表达式，这种写法等同于 `let x = 5 + 5;`
2. `x + 5` 没有分号，因为它是一条表达式，这个在上一节中我们也有详细介绍

再来看一段代码，同时使用 `return` 和表达式作为返回值：

```rust
fn plus_or_minus(x: i32) -> i32 {
    if x > 5 {
        return x - 5
    }

    x + 5
}

fn main() {
    let x = plus_or_minus(5);

    println!("The value of x is: {}", x);
}
```

`plus_or_minus` 函数根据传入 `x` 的大小来决定是做加法还是减法，若 `x > 5` 则通过 `return` 提前返回 `x - 5` 的值，否则返回 `x + 5` 的值。

##### Rust 中的特殊返回类型

###### 无返回值`()`

对于 Rust 新手来说，有些返回类型很难理解，而且如果你想通过百度或者谷歌去搜索，都不好查询，因为这些符号太常见了，根本难以精确搜索到。

例如<font color=dodgerBlue>单元类型 `()`</font>，<font color=fuchsia>**是一个零长度的元组**</font>。它没啥作用，但是可以用来表达一个函数没有返回值：

- 函数没有返回值，那么返回一个 `()`
- 通过 `;` 结尾的语句返回一个 `()`

例如下面的 `report` 函数会隐式返回一个 `()`：

```rust
use std::fmt::Debug;

fn report<T: Debug>(item: T) {
  println!("{:?}", item);
}
```

与上面的函数返回值相同，但是下面的函数显式的返回了 `()`：

```rust
fn clear(text: &mut String) -> () {
  *text = String::from("");
}
```

在实际编程中，你会经常在错误提示中看到该 `()` 的身影出没，假如你的函数需要返回一个 `u32` 值，但是如果你不幸的以 `表达式;` 的语句形式作为函数的最后一行代码，就会报错：

```rust
fn add(x:u32,y:u32) -> u32 {
    x + y;
}
```

错误如下：

```rust
error[E0308]: mismatched types // 类型不匹配
 --> src/main.rs:6:24
  |
6 | fn add(x:u32,y:u32) -> u32 {
  |    ---                 ^^^ expected `u32`, found `()` // 期望返回u32,却返回()
  |    |
  |    implicitly returns `()` as its body has no tail or `return` expression
7 |     x + y;
  |          - help: consider removing this semicolon
```

还记得我们在[语句与表达式](https://course.rs/basic/base-type/statement-expression.html)中讲过的吗？只有表达式能返回值，而 `;` 结尾的是语句，在 Rust 中，一定要严格区分**表达式**和**语句**的区别，这个在其它语言中往往是被忽视的点。

###### 永不返回的发散函数 `!`

<font color=dodgerBlue>**当用 `!` 作函数返回类型的时候**</font>，<font color=fuchsia>表示该函数永不返回 ( diverge function )</font>，特别的，这种语法往往用做会导致程序崩溃的函数：

```rust
fn dead_end() -> ! {
  panic!("你已经到了穷途末路，崩溃吧！");ccccc
}
```

下面的函数创建了一个无限循环，该循环永不跳出，因此函数也永不返回：

```rust
fn forever() -> ! {
  loop {
    //...
  };
}
```



#### 所有权和借用

Rust 之所以能成为万众瞩目的语言，就是因为其<font color=red>内存安全性</font>。<font color=dodgerBlue>在以往</font>，<font color=red>内存安全几乎都是通过 GC 的方式实现</font>，但是 <font color=red>**GC 会引来性能、内存占用以及 Stop the world 等问题**，在高性能场景和系统编程上是不可接受的</font>，因此 Rust 采用了与 ( 不 ) 众 ( 咋 ) 不 ( 好 ) 同 ( 学 )的方式：**所有权系统**。

#### 所有权

所有的程序都必须和计算机内存打交道，<font color=lightSeaGreen>如何从内存中申请空间来存放程序的运行内容，如何在不需要的时候释放这些空间，成了重中之重</font>，也是所有编程语言设计的难点之一。在计算机语言不断演变过程中，<font color=dodgerBlue>出现了三种流派</font>：

- **垃圾回收机制 ( GC )**，在程序运行时不断寻找不再使用的内存，典型代表：Java、Go
- **手动管理内存的分配和释放**，在程序中，<font color=red>通过函数调用的方式来申请和释放内存</font>，典型代表：C++
- **通过所有权来管理内存**，<font color=red>编译器在编译时会根据一系列规则进行检查</font>

其中 Rust 选择了第三种，最妙的是，<font color=fuchsia>这种检查只发生在编译期</font>，因此<font color=red>对于程序运行期，不会有任何性能上的损失</font>

##### 一段不安全的代码

先来看看一段来自 C 语言的糟糕代码：

```c
int* foo() {
    int a;          // 变量 a 的作用域开始
    a = 100;
    char *c = "xyz"; // 变量 c 的作用域开始
    return &a;
}                   // 变量 a 和 c 的作用域结束
```

<font color=dodgerBlue>这段代码虽然可以编译通过，但是其实非常糟糕</font>；变量 `a` 和 `c` 都是局部变量，函数结束后将局部变量 `a` 的地址返回，但<font color=red>局部变量 `a` 存在栈中</font>，在离开作用域后，`a` 所申请的栈上内存都会被系统回收，从而造成了 “悬空指针” ( Dangling Pointer ) 的问题。这是一个非常典型的内存安全问题，虽然编译可以通过，但是运行的时候会出现错误, 很多编程语言都存在。

再来看变量 `c`，`c` 的值是常量字符串，<font color=red>存储于常量区</font>，可能这个函数我们只调用了一次，也可能我们不再会使用这个字符串，<font color=red>但 `"xyz"` 只有当整个程序结束后系统才能回收这片内存</font>。

所以内存安全问题，一直都是程序员非常头疼的问题，好在，在 Rust 中这些问题即将成为历史，因为 Rust 在编译的时候就可以帮助我们发现内存不安全的问题。



## 其他笔记



#### 智能指针

可以参考：[如何理解智能指针？ - 胡昊的回答 - 知乎](https://www.zhihu.com/question/20368881/answer/14918675) ，相当易懂。另外，需要注意的是 由于是 2012 年的文章，里面还在使用 boost 库的版本，尤其是没有提到 `unique_ptr` 。所以可以看下 C++ Primer 的 第 12 章“动态内存”。
