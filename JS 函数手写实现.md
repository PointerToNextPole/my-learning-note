# JS 函数手写实现



#### bind函数

```js
/**
 * 实现要求：
 * 1.改变this指向
 * 2.第一个参数是this的值，后面的参数是 函数接收参数 的值
 * 3.返回值不变
 */
Function.prototype.myBind = function() {
  const self = this // 谁调用myBind，this就指向谁
  
  const args = Array.prototype.slice.call(arguments)
  const thisVal = args.shift()
  
  // 返回匿名函数
  return function() {
    return self.apply(thisVal, arg) // 返回执行原函数
  }
}
```

摘自：[快速掌握 JS 面试题之『作用域和闭包』 - 手写bind函数](https://www.bilibili.com/video/BV1Kv411778c?p=4)



#### promise 实现

```js

```





参考资料：

[测试一下前端基本功--简单的源码重写？](https://www.bilibili.com/video/BV1dS4y1X7pn) 

[前端面试出场率奇高的18个手写代码，原来代码还可以这么写？！！ - 爱前端不爱恋爱的文章 - 知乎](https://zhuanlan.zhihu.com/p/256195603)