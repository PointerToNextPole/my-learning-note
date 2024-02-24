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

在 [Rust 1.59](https://course.rs/appendix/rust-versions/1.59.html) 版本后，我们可以在赋值语句的左式中使用元组、切片和结构体模式了。

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

这种使用方式跟之前的 `let` 保持了一致性，但是 `let` 会重新绑定，而这里仅仅是对之前绑定的变量进行再赋值。

需要注意的是，使用 `+=` 的赋值语句还不支持解构式赋值。

> 这里用到了模式匹配的一些语法，如果大家看不懂没关系，可以在学完模式匹配章节后，再回头来看。



## 其他笔记



#### 智能指针

可以参考：[如何理解智能指针？ - 胡昊的回答 - 知乎](https://www.zhihu.com/question/20368881/answer/14918675) ，相当易懂。另外，需要注意的是 由于是 2012 年的文章，里面还在使用 boost 库的版本，尤其是没有提到 `unique_ptr` 。所以可以看下 C++ Primer 的 第 12 章“动态内存”。
