# Nuxt 备忘录



## 文档笔记



#### Introduction

##### Automation and Conventions

<font color=red>Nuxt uses conventions and an **opinionated** directory structure</font> to automate repetitive tasks and allow developers to focus on pushing features. <font color=lightSeaGreen>The configuration file can still customize and override its default behaviors</font>.

- <font color=fuchsia>**File-based routing:**</font> <font color=red>define routes based on the structure of your [**`pages/`** directory](https://nuxt.com/docs/guide/directory-structure/pages)</font>. This can make it easier to organize your application and avoid the need for manual route configuration.

  > 💡 参照 UmiJS 的说法：这种路由也可以被理解为“约定式路由”
  >
  > > 除配置式路由外，Umi 也支持约定式路由。约定式路由也叫文件路由，就是不需要手写配置，文件系统即路由，通过目录和文件及其命名分析出路由配置。
  > >
  > > **如果没有 routes 配置，Umi 会进入约定式路由模式**，然后分析 `src/pages` 目录拿到路由配置。
  > >
  > > 比如以下文件结构：
  > >
  > > ```bash
  > > .
  > >   └── pages
  > >     ├── index.tsx
  > >     └── users.tsx
  > > ```
  > >
  > > 会得到以下路由配置，
  > >
  > > ```js
  > > [
  > >   { path: '/', component: '@/pages/index' },
  > >   { path: '/users', component: '@/pages/users' },
  > > ]
  > > ```
  > >
  > > > 使用约定式路由时，约定 `src/pages` 下所有的 `(j|t)sx?` 文件即路由。如果你需要修改默认规则，可以使用 [conventionRoutes](https://umijs.org/docs/api/config#conventionroutes) 配置。
  > >
  > > 摘自：[UmiJS doc - 路由 # 约定式路由](https://umijs.org/docs/guides/routes#%E7%BA%A6%E5%AE%9A%E5%BC%8F%E8%B7%AF%E7%94%B1)

- **Code splitting:** Nuxt automatically splits your code into smaller chunks, which can help reduce the initial load time of your application.

- <font color=red>**Server-side rendering out of the box:**</font> <font color=red>Nuxt comes with built-in SSR capabilities</font>, so you <font color=lightSeaGreen>don't have to set up a separate server yourself</font>.

- **Auto-imports:** write Vue composables and components in their respective directories and use them without having to import them with the benefits of tree-shaking and optimized JS bundles.

- **Data-fetching utilities:** <font color=red>Nuxt provides composables to handle SSR-compatible data fetching</font> as well as different strategies.

  > 👀 前面几点都听过，没什么难度；但是上面这点是完全的 SSR 概念，没怎么听过

- **Zero-config TypeScript support:** write type-safe code without having to learn TypeScript with our auto-generated types and `tsconfig.json`

- **Configured build tools:** we <font color=red>use [Vite](https://vitejs.dev/) by default to support hot module replacement (HMR) in development</font> and bundling your code for production with best-practices baked-in.

Nuxt takes care of these and provides both frontend and backend functionality so you can focus on what matters: **creating your web application**.

##### Server-Side Rendering

Nuxt comes with built-in server-side rendering (SSR) capabilities by default, <font color=red>without having to configure a server yourself</font>, which has many benefits for web applications:

- <font color=red>**Faster initial page load time:**</font> Nuxt sends a fully rendered HTML page to the browser, <font color=red>which can be displayed immediately</font>. This can provide a faster perceived page load time and a <font color=lightSeaGreen>better user experience (UX), **especially on slower networks or devices**</font>.
- **Improved SEO:** search engines can better index SSR pages because the HTML content is available immediately, rather than requiring JavaScript to render the content on the client-side.
- <font color=red>**Better performance on low-powered devices:**</font> it <font color=lightSeaGreen>reduces the amount of JavaScript that needs to be downloaded and executed on the client-side</font>, which can be beneficial for low-powered devices that may struggle with processing heavy JavaScript applications.
- **Better accessibility:** the content is immediately available on the initial page load, <font color=red>improving accessibility for users</font> who rely on screen readers or other assistive technologies.
- **Easier caching:** pages can be cached on the server-side, which can further improve performance by reducing the amount of time it takes to generate and send the content to the client.

Overall, server-side rendering can provide a faster and more efficient user experience, as well as improve search engine optimization and accessibility.

As Nuxt is a versatile framework, it gives you the possibility to <font color=red>statically render your whole application to a static hosting with `nuxt generate`</font> , <font color=lightSeaGreen>**disable SSR globally with the `ssr: false` option**</font> or <font color=red>leverage hybrid rendering by setting up the `routeRules` option</font>.

##### Server engine

The Nuxt server engine [Nitro](https://nitro.unjs.io/) unlocks new full-stack capabilities.

<font color=dodgerBlue>In development</font>, it <font color=red>uses Rollup and Node.js workers for your server code and context isolation</font>. It also <font color=red>**generates your server API by reading files in `server/api/` and server middleware from `server/middleware/`**</font>.

<font color=dodgerBlue>In production</font>, <font color=fuchsia>Nitro builds your app and server into one universal **`.output` directory**</font>. This output is light: minified and removed from any Node.js modules (except polyfills). You can deploy this output on any system supporting JavaScript, from Node.js, Serverless, Workers, Edge-side rendering or purely static.

##### Architecture

Nuxt is composed of different [core packages](https://github.com/nuxt/nuxt/tree/main/packages):

- Core Engine: [nuxt](https://github.com/nuxt/nuxt/tree/main/packages/nuxt)
- Bundlers: [@nuxt/vite-builder](https://github.com/nuxt/nuxt/tree/main/packages/vite) and [@nuxt/webpack-builder](https://github.com/nuxt/nuxt/tree/main/packages/webpack)
- Command line interface: [nuxi](https://github.com/nuxt/nuxt/tree/main/packages/nuxi)
- Server engine: [nitro](https://github.com/unjs/nitro)
- Development kit: [@nuxt/kit](https://github.com/nuxt/nuxt/tree/main/packages/kit)
- Nuxt 2 Bridge: [@nuxt/bridge](https://github.com/nuxt/bridge)

We recommend reading each concept to have a full vision of Nuxt capabilities and the scope of each package.







#### Installation

##### New Project

###### Prerequisites

- **Node.js** - <font color=red>**`v18.0.0`** or newer</font>
- **Text editor** - We recommend [Visual Studio Code](https://code.visualstudio.com/) with the [Volar Extension](https://marketplace.visualstudio.com/items?itemName=Vue.volar)
- **Terminal** - In order to run Nuxt commands

> 💡 <font color=dodgerBlue>Additional notes for an optimal setup:</font>
>
> - **Node.js**: <font color=red>Make sure to use an **even numbered version**</font> (18, 20, etc)
> - <font color=fuchsia>**Nuxtr**</font>: Install the community-developed [Nuxtr extension](https://marketplace.visualstudio.com/items?itemName=Nuxtr.nuxtr-vscode)
> - **Volar**: Either enable [**Take Over Mode**](https://vuejs.org/guide/typescript/overview.html#volar-takeover-mode) (recommended) or add the [TypeScript Vue Plugin](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin)
>
> If you have enabled **Take Over Mode** or installed the **TypeScript Vue Plugin (Volar)**, you can disable generating the shim for `*.vue` files in your [`nuxt.config.ts`](https://nuxt.com/docs/guide/directory-structure/nuxt-config) file:
>
> ```ts
> // nuxt.config.ts
> export default defineNuxtConfig({
>   typescript: {
>     shim: false
>   }
> })
> ```

##### Development Server

Now you'll be able to start your Nuxt app in development mode:

```sh
npm run dev -- -o
```

> 👀 这里文档列出了 yarn pnpm bun 该命令的写法，可以说：和 npm 版本很不一样



#### Configuration

By default, Nuxt is configured to cover most use cases. <font color=red>The [**`nuxt.config.ts`**](https://nuxt.com/docs/guide/directory-structure/nuxt-config) file can **override** or **extend** this default configuration</font>.

##### Nuxt Configuration

The [`nuxt.config.ts`](https://nuxt.com/docs/guide/directory-structure/nuxt-config) file is <font color=lightSeaGreen>located at the root of a Nuxt project</font> and can override or extend the application's behavior.

<font color=red>A minimal configuration file exports the `defineNuxtConfig` function containing an object with your configuration</font>. The <font color=lightSeaGreen>`defineNuxtConfig` helper is **globally available** without import</font>.

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  // My Nuxt config
})
```

This file will often be mentioned in the documentation, <font color=dodgerBlue>for example</font> to <font color=red>add custom scripts, **register modules** or change rendering modes</font>.

> 💡 Every option is described in the [**Configuration Reference**](https://nuxt.com/docs/api/configuration/nuxt-config).

> 💡 <font color=lightSeaGreen>You don't have to use TypeScript to build an application with Nuxt</font>. However, it is <font color=red>strongly recommended to use the `.ts` extension for the `nuxt.config` file</font>. This way you can <font color=lightSeaGreen>**benefit from hints in your IDE to avoid typos and mistakes while editing your configuration**</font>.

##### Environment overrides

You can configure fully typed, <font color=red>per-environment overrides in your nuxt.config</font>

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  $production: {
    routeRules: {
      '/**': { isr: true }
    }
  },
  $development: {
    //
  }
})
```

> 💡 If you're authoring layers, you can also <font color=red>use the `$meta` key to provide metadata</font> that you or the consumers of your layer might use.



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