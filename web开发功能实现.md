# web开发功能实现



#### 文件上传





#### 虚拟列表

可以参考文章：[剖析无限滚动虚拟列表的实现原理](https://lkangd.com/post/virtual-infinite-scroll/)



#### 全屏展示

> 💡 关于全屏功能的实现，可以使用 [GitHub - screenfull](https://github.com/sindresorhus/screenfull#screenfull) 或者 [React use - `useFullscreen`](https://streamich.github.io/react-use/?path=/story/ui-usefullscreen--docs) 和 [VueUse - `useFullscreen`](https://vueuse.org/core/useFullscreen/#usefullscreen) ，不必自己实现

##### 实现页面全屏展示

```js
function fullScreen() {  
  const el = document.documentElement
  const rfs = 
    el.requestFullScreen || 
    el.webkitRequestFullScreen || 
    el.mozRequestFullScreen || 
    el.msRequestFullscreen
  if(typeof rfs != "undefined" && rfs) {
      rfs.call(el)
  }
}
```

> 👀 需要注意的是：经过测试，直接调用 `fullScreen` 函数，甚至是 dispatchEvent，都是无法实现“自动”页面全屏的。必须需要用户手动点击元素，触发使得页面全屏；这里应该涉及到权限。

##### 退出全屏

```js
function exitScreen() {
  if (document.exitFullscreen) document.exitFullscreen()
  else if (document.mozCancelFullScreen) document.mozCancelFullScreen()
  else if (document.webkitCancelFullScreen) document.webkitCancelFullScreen()
  else if (document.msExitFullscreen) document.msExitFullscreen()
  
  if (typeof cfs != "undefined" && cfs) cfs.call(el)
}
```

摘自：[js小众且好用的技巧【api】](https://juejin.cn/post/7229515080487370812)



