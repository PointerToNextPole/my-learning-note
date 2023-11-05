# Nuxt 备忘录



## 文档笔记



#### Introduction

<font color=red>Nuxt uses conventions and an **opinionated** directory structure</font> to automate repetitive tasks and allow developers to focus on pushing features. The configuration file can still customize and override its default behaviors.

- **File-based routing:** define routes based on the structure of your [`pages/` directory](https://nuxt.com/docs/guide/directory-structure/pages). This can make it easier to organize your application and avoid the need for manual route configuration.
- **Code splitting:** Nuxt automatically splits your code into smaller chunks, which can help reduce the initial load time of your application.
- **Server-side rendering out of the box:** Nuxt comes with built-in SSR capabilities, so you don't have to set up a separate server yourself.
- **Auto-imports:** write Vue composables and components in their respective directories and use them without having to import them with the benefits of tree-shaking and optimized JS bundles.
- **Data-fetching utilities:** Nuxt provides composables to handle SSR-compatible data fetching as well as different strategies.
- **Zero-config TypeScript support:** write type-safe code without having to learn TypeScript with our auto-generated types and `tsconfig.json`
- **Configured build tools:** we use [Vite](https://vitejs.dev/) by default to support hot module replacement (HMR) in development and bundling your code for production with best-practices baked-in.

Nuxt takes care of these and provides both frontend and backend functionality so you can focus on what matters: **creating your web application**.



## 工作经验



#### 如何添加 webpack 插件

> ⚠️ 这里摘抄的是第三方文档 nuxtjs.cn 的，而且 Nuxt 版本是 2.14.5 ，而不是 3.x

可以在 `nuxt.config.js` 中添加 webpack 插件：

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

摘自：[nuxtjs.cn - 常见问题 - Webpack 插件 # 如何添加 Webpack 插件？](https://www.nuxtjs.cn/faq/webpack-plugins) 。另外，在  Nuxt v2 的文档中找到了对应的部分：[Nuxt 2 - Learn - Configuration # Add webpack plugins](https://v2.nuxt.com/docs/features/configuration/#add-webpack-plugins) ；不过，在 Nuxt v3 的 [Docs - Configuration]() 部分并没有找到相应内容...