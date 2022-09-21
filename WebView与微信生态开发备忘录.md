# webView & 微信生态开发备忘录



#### 判断是否在“小程序”环境中

可以根据字段 `window.__wxjs_environment`，如果是一个对象，则在小程序中；否则应该是 undefined



#### 安装和使用 微信官方 js-sdk

##### 安装

```sh
npm install weixin-js-sdk
```

##### 使用

```js
import wx from 'weixin-js-sdk'
```



#### webview 网页 跳转到小程序中

```js
wx.miniProgram.navigateTo({ url: urlStr })
```



#### webView 移动端调试工具

一般使用 vconsole

##### 添加 vconsole

###### npm 安装 和使用（推荐）

```sh
$ npm install vconsole
```

```js
import VConsole from 'vconsole';

const vConsole = new VConsole();
// or init with options
const vConsole = new VConsole({ theme: 'dark' });

// remove it when you finish debugging
vConsole.destroy();
```

###### CDN 使用

```html
<script src="https://unpkg.com/vconsole@latest/dist/vconsole.min.js"></script>
<script>
  // VConsole will be exported to `window.VConsole` by default.
  var vConsole = new window.VConsole();
</script>
```

##### 其他替代品

背景：在公司的 iOS 12 系统的 iPhone 6S 测试机上使用 vconsole 时，发现 Network 无法良好的工作，访问接口的请求没有展示出（可能是 CDN 安装的原因？），感觉不对劲；便搜索 vconsole 的替代品，找到了 [eruda](https://github.com/liriliri/eruda) ，最后确实 eruda 是正常的。

类似的还有：[weinre](http://people.apache.org/~pmuellr/weinre/) ⚠️ 相当有名，但是废弃了... ，[spy-debugger](https://github.com/wuchangming/spy-debugger)



#### 微信开发者工具使用

##### Clear Cache

出现了这样的问题：明明自己打测试包，使用 http-server 运行，访问的始终是生产环境的接口；不知如何处理，甚至删掉重新打包之后，依然如此。经过同事指点，点击调试工具右上方的 “ Clear Cache ” 简单粗暴的 “ Clear All ” 即可解决... 



#### 问题与解决方法

##### ios端微信h5页面上下滑动时卡顿、页面缺失

**问题详情描述：**在ios端，上下滑动页面时，如果页面高度超出了一屏，就会出现明显的卡顿，页面有部分内容显示不全的情况，例如下图，右图是正常页面，边是ios上下滑动后，卡顿导致如左图下面部分丢失。

<img src="https://s2.loli.net/2022/07/15/f1GU59VgKhwxJnW.webp" alt="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/8/6/16c649f7c602c727~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp" style="zoom:30%;" />

**出现原因分析：**

Safari 对于 `overflow-scrolling` 用了原生控件来实现。对于有 `-webkit-overflow-scrolling` 的网页，会创建一个 UIScrollView ，提供 子layer 给渲染模块使用。【有待考证】

**解决办法：**只需要在公共样式加入下面这行代码

```css
*{
  -webkit-overflow-scrolling: touch;
}
```

但是，这个属性是有 bug 的，比如如果你的页面中有设置了绝对定位的节点，那么该节点的显示会错乱，当然还有会有其他的一些bug。

##### 安卓弹出的键盘遮盖文本框

**问题详情描述：**安卓微信 H5 弹出软键盘后挡住 input 输入框，如下左图是期待唤起键盘的时候样子，右边是实际唤起键盘的样子。

<img src="https://s2.loli.net/2022/07/15/cohsKg2xJR4Gt76.webp" alt="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/8/6/16c64a040001376c~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp" style="zoom: 30%;" />

**解决办法**：给 input 和 textarea 标签添加 focus 事件，如下，先判断是不是安卓手机下的操作，当然，可以不用判断机型，Document 对象属性和方法，setTimeout 延时 0.5 秒，因为调用安卓键盘有一点迟钝，导致如果不延时处理的话，滚动就失效了

```js
changefocus() {
  let u = navigator.userAgent, app = navigator.appVersion;
  let isAndroid = u.indexOf('Android') > -1 || u.indexOf('Linux') > -1;
  if(isAndroid){
    setTimeout(function() {
     document.activeElement.scrollIntoViewIfNeeded();
     document.activeElement.scrollIntoView();
    }, 500);       
  }
},
```

以上内容摘自：[微信H5页面前端开发，大多数人都会遇到的几个兼容性坑](https://juejin.cn/post/6844903907139780616)



#### iOS 模拟器调试 WebView

##### 打开模拟器

打开模拟器至少有两种方法

###### 方法一

spotlight 中输入 “simulator” ，可以直接运行

###### 方法二

Xcode 中 “Xcode” -> “Open Developer Tool” -> “Simulator”

<img src="https://s2.loli.net/2022/09/21/pAaUwdYtOeVCh4T.png" alt="image-20220921234836028" style="zoom:50%;" />

##### 创建新的模拟器与打开模拟器

如下

<img src="https://s2.loli.net/2022/09/21/JdDfCsMr6gAqPiI.png" alt="image-20220921235233136" style="zoom:50%;" />

<img src="https://s2.loli.net/2022/09/21/eUgK7LMOIBZHbvD.png" alt="image-20220921235038192" style="zoom:50%;" />

有一个问题是：只能使用 已经下载的 OS 版本，在这里没法添加新指定的 OS版本 ( OS Version )，感觉有点反人类... 方法见下面

##### 添加指定的 OS 版本

OS 版本必须要在 Xcode 中下载，点击 Xcode 中 “Xcode” -> “window” -> “Devices and Simulators”

<img src="https://s2.loli.net/2022/09/21/grcubENiMp1KXAR.png" alt="image-20220921235659086" style="zoom:40%;" />

进入如下页面，点击 “+” 号

<img src="https://s2.loli.net/2022/09/22/qCkOn5QJfM1zupG.png" alt="image-20220922000009823" style="zoom:40%;" />

选择 “Download more simulator runtimes...”

<img src="https://s2.loli.net/2022/09/22/YjkV7RU5XMDlyqG.png" alt="image-20220922000229461" style="zoom:40%;" />

从而进行选择：

<img src="https://s2.loli.net/2022/09/22/F9CKBc6vynwue7L.png" alt="image-20220922000410775" style="zoom:45%;" />

<img src="https://s2.loli.net/2022/09/22/UwFfnTBe6ucLApm.png" alt="image-20220922000539319" style="zoom:45%;" />

##### 创建与打开 Simulator

<img src="https://s2.loli.net/2022/09/22/gQiobnVB5KpwZXh.png" alt="image-20220922000810475" style="zoom:45%;" />

##### 使用 Simulator

<img src="https://s2.loli.net/2022/09/22/1TamIy639VkRltU.png" alt="image-20220922000957059" style="zoom:35%;" />
