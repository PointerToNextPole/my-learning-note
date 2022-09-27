# webView & 微信生态开发备忘录



#### WebView 和 小程序 联动



##### webview 网页 跳转到小程序中

```js
wx.miniProgram.navigateTo({ url: urlStr })
```

同时，小程序内部的页面跳转也是使用 `wx.navigateTo()`



##### 判断当前环境是否在 小程序 中

可以根据字段 `window.__wxjs_environment`，如果是一个对象，则在小程序中；否则应该是 undefined

具体可以参考 [微信官方文档 - 小程序 - 组件 - web-view](https://developers.weixin.qq.com/miniprogram/dev/component/web-view.html)



##### 安装和使用 微信官方 js-sdk

###### 安装

```sh
npm install weixin-js-sdk
```

###### 使用

```js
import wx from 'weixin-js-sdk'
```



#### 小程序生命周期

##### 页面 Page 实例的生命周期

<img src="https://s2.loli.net/2022/09/23/iXBRNuCZklsQbrL.png" alt="img" style="zoom:75%;" />

摘自：[微信官方文档 - 小程序 - 框架接口 / 页面 / 页面生命周期](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page-life-cycle.html)

##### App 生命周期回调函数

###### onLoad(Object query)

页面加载时触发。一个页面只会调用一次，<font color=red>可以在 onLoad 的参数中获取打开当前页面路径中的参数</font>。

**参数：**

| 名称  | 类型   | 说明                     |
| :---- | :----- | :----------------------- |
| query | Object | 打开当前页面路径中的参数 |

###### onShow()

页面显示 / 切入前台时触发。

###### onReady()

页面初次渲染完成时触发。一个页面只会调用一次，代表页面已经准备妥当，可以和视图层进行交互。

注意：对界面内容进行设置的 API 如 [wx.setNavigationBarTitle](https://developers.weixin.qq.com/miniprogram/dev/api/ui/navigation-bar/wx.setNavigationBarTitle.html) ，请在 `onReady` 之后进行。详见[生命周期](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page-life-cycle.html)

###### onHide()

页面隐藏/切入后台时触发。 如 [wx.navigateTo](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.navigateTo.html) 或底部 `tab` 切换到其他页面，小程序切入后台等。

###### onUnload()

页面卸载时触发。如 [wx.redirectTo](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.redirectTo.html) 或 [wx.navigateBack](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.navigateBack.html) 到其他页面时。

摘自：[微信官方文档 - 小程序 - 框架接口 / 页面 / Page](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html)



#### Page 对象

注册小程序中的一个页面。接受一个 `Object` 类型参数，其指定页面的初始数据、生命周期回调、事件处理函数等。

##### 参数

| 属性                                                         | 类型         | 默认值 | 必填 | 说明                                                         |
| :----------------------------------------------------------- | :----------- | :----- | :--- | :----------------------------------------------------------- |
| [data](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#data) | Object       |        |      | 页面的初始数据                                               |
| options                                                      | Object       |        |      | 页面的组件选项，同 [`Component` 构造器](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Component.html) 中的 `options` ，需要基础库版本 [2.10.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [behaviors](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/behaviors.html) | String Array |        |      | 类似于 mixins 和 traits 的组件间代码复用机制，参见 [behaviors](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/behaviors.html)，需要基础库版本 [2.9.2](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [onLoad](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onLoad-Object-query) | function     |        |      | 生命周期回调—监听页面加载                                    |
| [onShow](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onShow) | function     |        |      | 生命周期回调—监听页面显示                                    |
| [onReady](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onReady) | function     |        |      | 生命周期回调—监听页面初次渲染完成                            |
| [onHide](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onHide) | function     |        |      | 生命周期回调—监听页面隐藏                                    |
| [onUnload](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onUnload) | function     |        |      | 生命周期回调—监听页面卸载                                    |
| [onPullDownRefresh](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onPullDownRefresh) | function     |        |      | 监听用户下拉动作                                             |
| [onReachBottom](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onReachBottom) | function     |        |      | 页面上拉触底事件的处理函数                                   |
| [onShareAppMessage](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onShareAppMessage-Object-object) | function     |        |      | 用户点击右上角转发                                           |
| [onShareTimeline](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onShareTimeline) | function     |        |      | 用户点击右上角转发到朋友圈                                   |
| [onAddToFavorites](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onAddToFavorites-Object-object) | function     |        |      | 用户点击右上角收藏                                           |
| [onPageScroll](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onPageScroll-Object-object) | function     |        |      | 页面滚动触发事件的处理函数                                   |
| [onResize](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onResize-Object-object) | function     |        |      | 页面尺寸改变时触发，详见 [响应显示区域变化](https://developers.weixin.qq.com/miniprogram/dev/framework/view/resizable.html#在手机上启用屏幕旋转支持) |
| [onTabItemTap](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onTabItemTap-Object-object) | function     |        |      | 当前是 tab 页时，点击 tab 时触发                             |
| [onSaveExitState](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onSaveExitState) | function     |        |      | 页面销毁前保留状态回调                                       |
| 其他                                                         | any          |        |      | 开发者可以添加任意的函数或数据到 `Object` 参数中，在页面的函数中用 `this` 可以访问。**这部分属性会在页面实例创建时进行一次深拷贝**。 |

摘自：[微信官方文档 - 小程序 - 框架接口 / 页面 / Page](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html)



#### 页面路由

在小程序中所有页面的路由全部由框架进行管理：

##### 页面栈

框架以栈的形式维护了当前的所有页面。 当发生路由切换的时候，页面栈的表现如下：

| 路由方式   | 页面栈表现                        |
| :--------- | :-------------------------------- |
| 初始化     | 新页面入栈                        |
| 打开新页面 | 新页面入栈                        |
| 页面重定向 | 当前页面出栈，新页面入栈          |
| 页面返回   | 页面不断出栈，直到目标返回页      |
| Tab 切换   | 页面全部出栈，只留下新的 Tab 页面 |
| 重加载     | 页面全部出栈，只留下新的页面      |

开发者可以使用 `getCurrentPages()` 函数获取当前页面栈。

##### 路由方式

对于路由的触发方式以及页面生命周期函数如下：

| 路由方式   | 触发时机                                                     | 路由前页面 | 路由后页面         |
| :--------- | :----------------------------------------------------------- | :--------- | :----------------- |
| 初始化     | 小程序打开的第一个页面                                       |            | onLoad, onShow     |
| 打开新页面 | 调用 API [wx.navigateTo](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.navigateTo.html) 使用组件 [`<navigator open-type="navigateTo"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html) | onHide     | onLoad, onShow     |
| 页面重定向 | 调用 API [wx.redirectTo](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.redirectTo.html) 使用组件 [`<navigator open-type="redirectTo"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html) | onUnload   | onLoad, onShow     |
| 页面返回   | 调用 API [wx.navigateBack](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.navigateBack.html) 使用组件[`<navigator open-type="navigateBack">`](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html) 用户按左上角返回按钮 | onUnload   | onShow             |
| Tab 切换   | 调用 API [wx.switchTab](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.switchTab.html) 使用组件 [`<navigator open-type="switchTab"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html) 用户切换 Tab |            | 各种情况请参考下表 |
| 重启动     | 调用 API [wx.reLaunch](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.reLaunch.html) 使用组件 [`<navigator open-type="reLaunch"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html) | onUnload   | onLoad, onShow     |

摘自：[微信官方文档 - 小程序 - 框架接口 / 页面 / 页面路由](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/route.html)



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
