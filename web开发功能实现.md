# web开发功能实现



#### 文件上传





#### 虚拟列表

可以参考文章：[剖析无限滚动虚拟列表的实现原理](https://lkangd.com/post/virtual-infinite-scroll/)



#### 视频自动播放

```html
<video src="srcUrl" autoplay></video>

<script>
  const video = document.querySelector('video')
  video.play()
</script>
```

如上两种方法：使用 `autoplay` 和 `video.play()` 都没有成功，甚至会出现如下错误：

> Uncaught (in promise) DOMException: play() failed because the user didn't interact with the document first.

这需要用户先与当前页面交互后，才能触发播放；这里便涉及到浏览器的 “自动播放策略”（动机是改善用户体验，显然浏览器厂商不希望自动播放机制被网页开发者滥用 ），这里只说 Chrome 的，其他浏览器也差不多

浏览器的 “自动播放策略” 是从 Chrome 66 开始生效的，浏览器并非不允许网页自动播放，它需要满足一些条件：

- 始终允许静音（没有声音）的视频自动播放
- 在如下情况下（满足任意一个），带声音的视频自动播放会被允许
  - 用户已经和当前网页进行了交互（通过 click、tap 事件）
  - 在桌面设备上，用户的媒体参与度指数已经超过阈值，就可以实现自动播放；这意味着用户之前播放过有声音的视频
  - 用户已经将网站添加到移动设备的主屏幕，或者在桌面上安装了 PWA
- 顶部帧 ( top frame ) （如果有自动播放的能力）可以将自动播放权限委派给 iframe，以允许其自动播放声音

##### 媒体参与度 ( MEI , Media Engagement Index )

媒体参与度 ( MEI ) 用于衡量个人在网站 ( domain ) 中使用多媒体的倾向

它是一个数字，可以通过 `chrome://media-engagement/` 查看

媒体参与度数值越高，浏览器会认为：用户对该网站中的多媒体更感兴趣；所以允许自动播放视频

对于开发者而言：

1. 媒体参与度的计算规则无法通过技术手段修改
2. 媒体参与度的计算规则在不同版本的浏览器中可能会不同

###### 关于媒体参与度 开发者的最佳实践

1. 方案一：互动后播放

   先尝试自动播放，若发生异常，则引导用户进行互动操作，然后在进行播放。示例如下：

   ```html
   <div class="video-container">
     <video src="./test.mp4" />
     <div class="modal">
       <button class="btn">开始播放</button>
     </div>
   </div>
   
   <script>
     const video = document.querySelector('video')
     const modal = document.querySelector('modal')
     const btn = document.querySelector('.btn')
   
     async function play() {
       try {
         await video.play() // play() 返回一个 Promise
         modal.style.display = 'none'
         btn.removeEventListener('click', play)
       } catch (err) { // 这里 err，可能有很多原因，这里假定就是自动播放失败
         modal.style.display = 'flex'
         btn.addEventListener('click', play)
       }
     }
     
     play()
   </script>
   ```

2. 方案二：互动后出声（这也是更推荐，企业使用最多的方案，比如抖音）

   先静音播放，然后根据是否能自动播放来决定是否取消静音。如果

   1. 能自动播放，则取消静音
   2. 不能自动播放，引导用户进行互动操作后取消静音

   代码示例如下：

   ```html
   <div class="video-container">
     <video src="./test.mp4" />
     <div class="modal">
       <button class="btn">开始声音</button>
     </div>
   </div>
   
   <script>
     const video = document.querySelector('video')
     const modal = document.querySelector('modal')
     const btn = document.querySelector('.btn')
   
     async function play() {
       video.muted = true // 静音
       video.play()
       const ctx = new AudioContext()
       const canAutoPaly = ctx.state === 'running'
       ctx.close()
       
       if (canAutoPaly) {
         video.muted = false
         modal.style.display = 'none'
         btn.removeEventListener('click', play)
       } else {
         modal.style.display = 'flex'
         btn.addEventListener('click', play)
       }
     }
   </script>
   ```

学习自：[自动播放策略【渡一教育】](https://www.bilibili.com/video/BV1Gw411C745)



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



