# Vue.js 经验与总结



### Vue2

##### 如何在 v-html 渲染成功之后执行 一些自定义的语句？

在给 `v-html` 的赋值函数里，赋值成功后执行 `this.$nextTick(function () {自己的方法}`

该方法在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

摘自：[Vue v-html渲染成功后执行函数](https://blog.csdn.net/Songlinlin_/article/details/100771070)



### Vue3

##### 在一个生命周期钩子中调用另一个生命周期钩子

在 [前端内存泄漏：你的JS代码在偷偷“吃”内存！](https://juejin.cn/post/7478520039411859519) 中看到如下代码

```js
import { onMounted, onUnmounted } from "vue";

onMounted(() => {
  const timer = setInterval(() => {
    console.log("Hello Vue3!");
  }, 1000);

  onUnmounted(() => clearInterval(timer));
});
```

感觉有些怪异，不过问了下 Gemini ( 🔗 https://g.co/gemini/share/a3adffd9f75a )，这种写法是可行的，同时也是没有任何危害的；虽然 Gemini 的说法是：并不推荐么这么做。但是，个人感觉：在保证创建和销毁逻辑放一起的同时，这样做不必暴露 `timer` 对象，增加了代码的安全性，在类似场景下这种写法也是值得推荐的





