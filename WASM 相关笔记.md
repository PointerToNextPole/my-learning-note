# WASM 相关笔记



#### 相关教程

[WebAssembly 官网](https://webassembly.org/)

[WebAssembly 中文网|Wasm 中文文档](http://webassembly.org.cn/)



#### 《WebAssembly 完全入门——了解 wasm 的前世今身》笔记

##### 什么时候使用 WebAssembly？

说了这么多，我到底什么时候该使用它呢？总结下来，大部分情况分两个点。1) 对性能有很高要求的 App/Module/游戏 2) 在 Web 中使用C/C++/Rust/Go 的库 举个简单的例子。如果你要实现的 Web 版本的 Ins 或者 Facebook， 你想要提高效率。那么就可以把其中对图片进行压缩、解压缩、处理的工具，用 C++ 实现，然后再编译回 WebAssembly。

##### WebAssembly 的几个开发工具

- [AssemblyScript](https://github.com/AssemblyScript/assemblyscript)：支持直接将 TypeScript 编译成 WebAssembly 。这对于很多前端同学来说，入门的门槛还是很低的。
- [Emscripten](https://github.com/emscripten-core/emscripten)：可以说是 WebAssembly 的灵魂工具不为过，上面说了很多编译，这个就是那个编译器（👀 命令是 `mscc`。命令格式类似于 `gcc`，生成可以在浏览器上运行的 `.wasm` 文件 ）。将其他的高级语言，编译成WebAssembly。
- [WABT](https://github.com/WebAssembly/wabt)：是个将 WebAssembly 在字节码和文本格式相互转换的一个工具，方便开发者去理解这个wasm到底是在做什么事。

> 👀 注：本文还介绍了一些 wasm 出现的背景，和 Emscripten 编译器  `mscc` 命令 相关的内容（暂时用不到，这里略）。
>
> 不过也做了一点总结，WASM 有点点类似于 FFI，不过，WASM本质上是“运行在现代浏览器上的虚拟机”，参见：[蚂蚁集团 Wasm 编译器虚拟机基础能力建设 - PDF](https://gw.alipayobjects.com/os/bmw-prod/8bf7483e-baaa-4119-bd4b-210aeea2d632.pdf) 和 [WebAssembly 虚拟机是什么？为什么应该使用它？](https://learnblockchain.cn/article/3486) // TODO 这两篇文章也建议读下。

摘自：[WebAssembly完全入门——了解wasm的前世今身 - SH的全栈笔记的文章 - 知乎](https://zhuanlan.zhihu.com/p/68048524)

