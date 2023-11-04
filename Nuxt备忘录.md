# Nuxt 备忘录



## 文档阅读



## 工作经验



#### 如何添加 webpack 插件

> ⚠️ 这里摘抄的文档是第三方 nuxtjs.cn 的，而且 Nuxt 版本是 2.14.5 不是 3.x

可以在 `nuxt.config.js` 中添加 Webpack 插件：

```js
const webpack = require('webpack')

module.exports = {
  build: {
    plugins: [
      new webpack.ProvidePlugin({
        $: 'jquery',
        _: 'lodash'
        // ...etc.
      })
    ]
  }
}
```

摘自：[nuxtjs.cn - 常见问题 - Webpack 插件 # 如何添加 Webpack 插件？](https://www.nuxtjs.cn/faq/webpack-plugins) 。另外，在  Nuxt v2 的文档中找到了对应的部分：[Nuxt 2 - Learn - Configuration # Add webpack plugins](https://v2.nuxt.com/docs/features/configuration/#add-webpack-plugins) ；不过，在 Nuxt v3 的 Docs - Configuration 部分并没有找到相应内容...