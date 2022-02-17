# Vue.js经验与总结



#### **如何在 v-html 渲染成功之后执行 一些自定义的语句？**

在给v-html的赋值函数里，赋值成功后执行 this.$nextTick(function () {自己的方法}

该方法在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

摘自：[Vue v-html渲染成功后执行函数](https://blog.csdn.net/Songlinlin_/article/details/100771070)

