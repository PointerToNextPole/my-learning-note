# TODO Note



[新一波JavaScript Web框架](https://mp.weixin.qq.com/s/2MrTEz_YXIxsqc1z69mJNA)（ 原文：[The new wave of Javascript web frameworks](https://frontendmastery.com/posts/the-new-wave-of-javascript-web-frameworks/) ）其中花了大量篇幅讲了 React 和 React 生态，以及 React 的一些问题。由于阅读时 React 没学多少，看得一知半解，有必要再次阅读。

另外其中有一些点，值得研究与笔记：

> 不知从何时起，我们觉得，让这些文件变成动态，会非常酷。于是我们有了像 <font color=fuchsia>CGI</font> 这样的技术，<font color=red>使我们能够根据请求提供不同的内容</font>。然后，我们有了像 Perl 这样的表达式语言来编写这些脚本。它对最初针对 Web 开发的 PHP 产生了影响。PHP 的创新之处在于将 HTML 直接连接到后端代码。这使得以编程方式创建嵌入动态值的文件变得容易了。
>
> 👀 可以看下：[万法归宗——CGI](https://zhuanlan.zhihu.com/p/25013398)

> 当虚拟 DOM 和真实 DOM 之间发生协调时，大型交互式应用程序会对用户的输入失去响应。像<font color=fuchsia>“长任务”</font>这样的术语开始出现了。
>
> 这导致了 React 在 2017 年被重新编写，为<font color=fuchsia>并发模式</font>奠定了基础。

> 在 React 中，虚拟 DOM 的运行时成本是无法避免的。<font color=fuchsia>并发模式</font>是一个解决问题的方法

> <font color=fuchsia>Svelte</font> 开创了预编译方法的先河，消除了我们在运行时看到的复杂性和开销。
>
> <font color=fuchsia>Svelte 完全避免了使用虚拟 DOM</font>，因此不会受到编写 Javascript 的不可变风格的约束，这种风格可以用来做更新状态之类的事情

> <font color=fuchsia>Solid 有一个直接的和可预测的反应性模型</font>，其灵感来自 Knockout。像 React 一样，它也避免了使用模板来简化函数的可组合性。
>
> 而 React 采取的是不断重新渲染世界的方法。<font color=fuchsia>Solid 只渲染一次</font>，并在不增加虚拟 DOM 开支的情况下，使用精简的反应性系统进行细粒度的更新。

> 受 PHP 的启发，<font color=fuchsia>Next</font> 开始简化创建静态页面推送到 CDN 的过程。它还解决了在 React 应用程序中使用 SSR 的棘手问题。

> <font color=fuchsia>Remix</font> 在 React 生态系统中带来了渐进增强的回归。
>
> 从技术角度来看，<font color=fuchsia>Remix 是 React Router 的编译器</font>，和其他新兴的元框架一样，是一个边缘兼容运行时。它通过嵌套布局和数据获取 API，解决了 Facebook 通过 Relay 大规模解决的相同挑战。

> Qwik 这个框架是关于尽量减少不必要的 Javascript。虽然它的 API 看起来像 React，但它的方法与其他元框架不同，因为<font color=fuchsia>它专注于水化过程</font>。