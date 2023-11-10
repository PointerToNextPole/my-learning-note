# Nuxt 备忘录



## 文档笔记



### Get Started



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

This file will often be mentioned in the documentation, <font color=dodgerBlue>for example</font> to <font color=red>add custom scripts, **register modules** or **change rendering modes**</font>.

> 💡 Every option is described in the [**Configuration Reference**](https://nuxt.com/docs/api/configuration/nuxt-config).

> 💡 You <font color=dodgerBlue>don't have to use TypeScript to build an application with Nuxt</font>. However, it is <font color=red>strongly recommended to use the `.ts` extension for the `nuxt.config` file</font>. This way you can <font color=lightSeaGreen>**benefit from hints in your IDE to avoid typos and mistakes while editing your configuration**</font>.

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

> 💡 If you're authoring layers（👀 见下面“注”）, you can also <font color=red>use the `$meta` key to provide metadata</font> that you or the consumers of your layer might use.
>
> 👀 上面的话没完全看懂，不过 “authoring layer” 的意思是“编写图层”

##### Environment Variables and Private Tokens

The `runtimeConfig` API  <font color=red>exposes values **like environment variables**</font>（👀 见下面 “注意”） to the rest of your application. <font color=dodgerBlue>By default</font>, <font color=red>**these keys are only available server-side**</font>. <font color=dodgerBlue>The keys within **`runtimeConfig.public`**</font> are <font color=red>also available client-side</font>.

> ⚠️ 需要注意的是：上面所说：“通过 `runtimeConfg` API 暴露出的值” 是 “类似于环境变量”，只是像，也可以被环境变量覆盖；但是，并不是环境变量。另外，优先级也是环境变量（比如通过 `.env` 定义）更高

<font color=red>Those values **should be defined in `nuxt.config`**</font> and can <font color=fuchsia>be overridden using environment variables</font>.

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  runtimeConfig: {
    // The private keys which are only available server-side
    apiSecret: '123',
    // **Keys within** public are also exposed client-side // 👀
    public: {
      apiBase: '/api'
    }
  }
})
```

```yml
# .env
# This will override the value of apiSecret
NUXT_API_SECRET=api_secret_token
```

These variables are exposed to the rest of your application using the [`useRuntimeConfig()`](https://nuxt.com/docs/api/composables/use-runtime-config)composable.

```vue
<script setup lang="ts">
const runtimeConfig = useRuntimeConfig()
</script>
```

##### App Configuration

<font color=dodgerBlue>The `app.config.ts` file</font>, located in the source directory (<font color=lightSeaGreen>by default the root of the project</font>), is <font color=red>used to **expose public variables**</font> that <font color=fuchsia>**can be determined at build time**</font>. Contrary to the `runtimeConfig` option, these <font color=fuchsia>**can not be overridden using environment variables**</font>.

<font color=red>A minimal configuration file exports the `defineAppConfig` function</font> containing an object with your configuration. The <font color=lightSeaGreen>`defineAppConfig` helper is **globally available without import**</font>.

```ts
// app.config.ts
export default defineAppConfig({
  title: 'Hello Nuxt',
  theme: {
    dark: true,
    colors: {
      primary: '#ff0000'
    }
  }
})
```

These variables are exposed to the rest of your application using the [`useAppConfig`](https://nuxt.com/docs/api/composables/use-app-config) composable.

```vue
<script setup lang="ts">
const appConfig = useAppConfig()
</script>
```

##### `runtimeConfig` vs `app.config`

As stated above, `runtimeConfig` and `app.config` are both used to expose variables to the rest of your application. <font color=dodgerBlue>**To determine whether you should use one or the other**, here are some guidelines</font>:

- `runtimeConfig` : Private or public tokens that <font color=red>need to be specified **after build using environment variables**</font>.
- `app.config` : <font color=red>Public tokens that are determined at build time</font>, website configuration such as theme variant, title and any project config that are <font color=red>**not sensitive**</font>.

| Feature                   | `runtimeConfig`       | `app.config` |
| ------------------------- | --------------------- | ------------ |
| Client Side               | Hydrated （🌏 水合的） | Bundled      |
| Environment Variables     | ✅ Yes                 | ❌ No         |
| Reactive                  | ✅ Yes                 | ✅ Yes        |
| Types support             | ✅ Partial             | ✅ Yes        |
| Configuration per Request | ❌ No                  | ✅ Yes        |
| Hot Module Replacement    | ❌ No                  | ✅ Yes        |
| Non primitive JS types    | ❌ No                  | ✅ Yes        |

> ⚠️ 上面的表格中，有些东西上面讲解中没提到，但是直接总结出来了；需要注意下

##### External Configuration Files

Nuxt uses [`nuxt.config.ts`](https://nuxt.com/docs/guide/directory-structure/nuxt-config) file as the single source of trust for configurations and skips reading external configuration files. During the course of building your project, you may have a need to configure those. The following table highlights common configurations and, where applicable, how they can be configured with Nuxt.

| Name                               | Config File             | How To Configure                                             |
| ---------------------------------- | ----------------------- | ------------------------------------------------------------ |
| [Nitro](https://nitro.unjs.io/)    | ~~`nitro.config.ts`~~   | Use [`nitro`](https://nuxt.com/docs/api/nuxt-config#nitro) key in `nuxt.config` |
| [PostCSS](https://postcss.org/)    | ~~`postcss.config.js`~~ | Use [`postcss`](https://nuxt.com/docs/api/nuxt-config#postcss) key in `nuxt.config` |
| [Vite](https://vitejs.dev/)        | ~~`vite.config.ts`~~    | <font color=red>Use [`vite`](https://nuxt.com/docs/api/nuxt-config#vite) key in `nuxt.config`</font> |
| [webpack](https://webpack.js.org/) | ~~`webpack.config.ts`~~ | <font color=red>**Use [`webpack`](https://nuxt.com/docs/api/nuxt-config#webpack-1) key in `nuxt.config`**</font> |

Here is a list of other common config files:

| Name                                          | Config File          | How To Configure                                             |
| --------------------------------------------- | -------------------- | ------------------------------------------------------------ |
| [TypeScript](https://www.typescriptlang.org/) | `tsconfig.json`      | [More Info](https://nuxt.com/docs/guide/concepts/typescript#nuxttsconfigjson) |
| [ESLint](https://eslint.org/)                 | `.eslintrc.js`       | [More Info](https://eslint.org/docs/latest/user-guide/configuring/configuration-files) |
| [Prettier](https://prettier.io/)              | `.prettierrc.json`   | [More Info](https://prettier.io/docs/en/configuration.html)  |
| [Stylelint](https://stylelint.io/)            | `.stylelintrc.json`  | [More Info](https://stylelint.io/user-guide/configure)       |
| [TailwindCSS](https://tailwindcss.com/)       | `tailwind.config.js` | [More Info](https://tailwindcss.nuxtjs.org/tailwind/config)  |
| [Vitest](https://vitest.dev/)                 | `vitest.config.ts`   | [More Info](https://vitest.dev/config)                       |

##### Vue Configuration

###### With Vite

If you need to pass options to `@vitejs/plugin-vue` or `@vitejs/plugin-vue-jsx` , you can do this in your `nuxt.config` file.

- `vite.vue` for `@vitejs/plugin-vue`. Check available options [here](https://github.com/vitejs/vite-plugin-vue/tree/main/packages/plugin-vue).
- `vite.vueJsx` for `@vitejs/plugin-vue-jsx`. Check available options [here](https://github.com/vitejs/vite-plugin-vue/tree/main/packages/plugin-vue-jsx).

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  vite: {
    vue: {                // 👀 vite.vue
      customElement: true
    },
    vueJsx: {             // 👀 vite.vueJsx
      mergeProps: true
    }
  }
})
```

###### With webpack

If you use webpack and need to configure `vue-loader` , you can do this using `webpack.loaders.vue` key inside your `nuxt.config` file. The available options are [defined here](https://github.com/vuejs/vue-loader/blob/main/src/index.ts#L32-L62).

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  webpack: { // 👀 感觉这里的配置和 webpack.config.js 中的内容没什么区别？
    loaders: {
      vue: {
        hotReload: true,
      }
    }
  }
})
```

###### Enabling Experimental Vue Features

You may need to enable experimental features in Vue, such as `defineModel` or `propsDestructure`. <font color=dodgerBlue>Nuxt provides an easy way to do that in `nuxt.config.ts`</font> , no matter which builder you are using:

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  webpack: {
    loaders: {
      vue: {
        hotReload: true,
      }
    }
  }
})
```



#### Views

##### `app.vue`

By default, Nuxt will treat this file as the **entrypoint** and render its content for every route of the application.

> 💡 If you are familiar with Vue, <font color=dodgerBlue>you might wonder where `main.js` is</font> (the file that normally creates a Vue app). <font color=red>Nuxt does this behind the scene</font>.

##### Components

Most components are reusable pieces of the user interface, like buttons and menus. In Nuxt, <font color=red>you can create these components in the [`components/`](https://nuxt.com/docs/guide/directory-structure/components) directory</font>, and <font color=fuchsia>they will be **automatically available across your application without having to explicitly import them**</font>.

##### Pages

Pages represent views for each specific route pattern. <font color=dodgerBlue>Every file in the [`pages/`](https://nuxt.com/docs/guide/directory-structure/pages) directory</font> <font color=red>**represents a different route** displaying its content</font>.

To use pages, create `pages/index.vue` file and add `<NuxtPage />` component to the [`app.vue`](https://nuxt.com/docs/guide/directory-structure/app) (or remove `app.vue` for default entry). You can now create more pages and their corresponding routes by adding new files in the [`pages/`](https://nuxt.com/docs/guide/directory-structure/pages) directory.

##### Layouts

<font color=red>Layouts are wrappers around pages</font> that <font color=fuchsia>contain a common User Interface for several pages</font>, such as a header and footer display. <font color=red>Layouts are Vue files using `<slot />` components to display the **page** content</font>. The `layouts/default.vue` file will be used by default. Custom layouts can be set as part of your page metadata.

> 💡 If you only have a single layout in your application, we recommend using [`app.vue`](https://nuxt.com/docs/guide/directory-structure/app) with [`<NuxtPage />`](https://nuxt.com/docs/api/components/nuxt-page) instead.

>  👀 还有点没搞懂 Layouts 在 Nuxt 中的作用和实际用途...

##### Advanced: Extending the HTML template

> 💡 If you only need to modify the `<head>`, you can refer to the [SEO and meta section](https://nuxt.com/docs/getting-started/seo-meta).

You can have full control over the HTML template by adding a Nitro plugin that registers a hook. <font color=dodgerBlue>The callback function of the `render:html` hook</font> <font color=red>allows you to mutate the HTML before it is sent to the client</font>.

> 👀 感觉有点像 axios 的拦截器

```ts
// server/plugins/extend-html.ts
export default defineNitroPlugin((nitroApp) => {
  nitroApp.hooks.hook('render:html', (html, { event }) => { 
    // This will be an object representation of the html template.
    console.log(html)
    html.head.push(`<meta name="description" content="My custom description" />`)
  })
  // You can also intercept the response here.
  nitroApp.hooks.hook('render:response', (response, { event }) => { console.log(response) })
})
```



#### Assets

<font color=red>Nuxt offers two options for your assets</font>.

Nuxt uses two directories to handle assets like stylesheets, fonts or images.

- The <font color=dodgerBlue>[`public/`](https://nuxt.com/docs/guide/directory-structure/public) directory content</font> is <font color=red>served at the server root as-is</font>.

  > 👀 感觉这里 `public/` 文件夹是把 Nuxt 当静态服务器来看待。另外，这个想法也得到了 [[#Assets#Public Directory]] 中内容的证实

- The <font color=dodgerBlue>[`assets/`](https://nuxt.com/docs/guide/directory-structure/assets) directory</font> contains by <font color=red>convention every asset that **you want the build tool (Vite or webpack) to process**</font>.

##### Public Directory

The [`public/`](https://nuxt.com/docs/guide/directory-structure/public) directory <font color=red>**is used as a public server**</font> for static assets publicly available at a defined URL of your application.

<font color=lightSeaGreen>You can get a file in the [`public/`](https://nuxt.com/docs/guide/directory-structure/public) directory from your application's code or from a browser by the root URL `/`</font> .

> 👀 即：可以 `public` 的路径可以省略

###### Example

For example, referencing an image file in the `public/img/` directory, available at the static URL `/img/nuxt.png `:

```vue
<template>
  <img src="/img/nuxt.png" alt="Discover Nuxt 3" />
</template>
```

##### Assets Directory

<font color=red>Nuxt uses [Vite](https://vitejs.dev/guide/assets.html) (**default**)</font> or [webpack](https://webpack.js.org/guides/asset-management) to build and bundle your application. The main function of these build tools is to process JavaScript files, but they can be extended through [plugins](https://vitejs.dev/plugins) (for Vite) or [loaders](https://webpack.js.org/loaders)(for webpack) to process other kind of assets, like stylesheets, fonts or SVG. This step transforms the original file mainly for performance or caching purposes (such as stylesheets minification or browser cache invalidation).

By convention, Nuxt uses the [`assets/`](https://nuxt.com/docs/guide/directory-structure/assets) directory to store these files but <font color=red>there is **no** auto-scan functionality for this directory</font>, and you can use any other name for it.

In your application's code, you can reference a file located in the [`assets/`](https://nuxt.com/docs/guide/directory-structure/assets) directory by using the `~/assets/` path.

> 👀 这点和 `public/` 就完全不一样

###### Example

For example, referencing an image file that will be processed if a build tool is configured to handle this file extension:

```vue
<template>
  <img src="~/assets/img/nuxt.png" alt="Discover Nuxt 3" />
</template>
```

> 💡 Nuxt won't serve files in the [`assets/`](https://nuxt.com/docs/guide/directory-structure/assets) directory at a static URL like `/assets/my-file.png`. If you need a static URL, use the [`public/`](https://nuxt.com/docs/getting-started/assets#public-directory) directory.

##### Global Styles Imports

To globally insert statements in your Nuxt components styles, you can use the [`Vite`](https://nuxt.com/docs/api/nuxt-config#vite) option at your [`nuxt.config`](https://nuxt.com/docs/api/nuxt-config) file.

###### Example

In this example, there is a [sass partial](https://sass-lang.com/documentation/at-rules/use#partials) file containing color variables to be used by your Nuxt [pages](https://nuxt.com/docs/guide/directory-structure/pages) and [components](https://nuxt.com/docs/guide/directory-structure/components)

```scss
// assets/colors.scss
$primary: #49240F;
$secondary: #E4A79D;
```

```scss
// assets/colors.sass
$primary: #49240F
$secondary: #E4A79D
```

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  vite: {
    css: {
      preprocessorOptions: {
        scss: {
          additionalData: '@use "@/assets/_colors.scss" as *;'
        },
        sass: {
          additionalData: '@use "@/assets/_colors.sass" as *\n'
        }
      }
    }
  }
})
```



#### Styling

Learn how to style your Nuxt application.

Nuxt is highly flexible when it comes to styling. Write your own styles, or reference local and external stylesheets. You can use CSS preprocessors, CSS frameworks, UI libraries and Nuxt modules to style your application.

##### Local Stylesheets

If you're writing local stylesheets, the natural place to put them is the [`assets/` directory](https://nuxt.com/docs/guide/directory-structure/assets).

###### Importing Within Components

You can import stylesheets in your pages, layouts and components directly. You can use a javascript import, or a css [`@import` statement](https://developer.mozilla.org/en-US/docs/Web/CSS/@import).

```html
<!-- pages/index.vue -->
<script>
// Use a static import for server-side compatibility
import '~/assets/css/first.css'

// Caution: Dynamic imports are not server-side compatible
import('~/assets/css/first.css')
</script>

<style>
@import url("~/assets/css/second.css");
</style>
```

> 💡 The stylesheets will be inlined in the HTML rendered by Nuxt.

##### The CSS Property

You can also use the `css` property in the Nuxt configuration. The natural place for your stylesheets is the [`assets/` directory](https://nuxt.com/docs/guide/directory-structure/assets). You can then reference its path and Nuxt will include it to all the pages of your application.

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  css: ['~/assets/css/main.css']
})
```

> 💡 The stylesheets will be inlined in the HTML rendered by Nuxt, injected globally and present in all pages.



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