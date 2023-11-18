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

***

Nuxt uses two directories to handle assets like stylesheets, fonts or images.

- The <font color=dodgerBlue>[`public/`](https://nuxt.com/docs/guide/directory-structure/public) directory content</font> is <font color=red>served at the server root as-is</font>.

  > 👀 感觉这里 `public/` 文件夹是把 Nuxt 当静态服务器来看待。另外，这个想法也得到了 [[#Assets#Public Directory]] 中内容的证实

- The <font color=dodgerBlue>[`assets/`](https://nuxt.com/docs/guide/directory-structure/assets) directory</font> contains by <font color=red>convention every asset that **you want the build tool (Vite or webpack) to process**</font>.

##### Public Directory

The [`public/`](https://nuxt.com/docs/guide/directory-structure/public) directory <font color=red>**is used as a public server**</font> for static assets publicly available at a defined URL of your application.

<font color=lightSeaGreen>You can get a file in the [`public/`](https://nuxt.com/docs/guide/directory-structure/public) directory from your application's code or from a browser by the root URL `/`</font> .

> 👀 即：**路径中，`public` 可以被省略**

###### Example

For example, referencing an image file in the `public/img/` directory, available at the static URL `/img/nuxt.png ` :

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

To globally insert statements in your Nuxt components styles, you can use the [`Vite`](https://nuxt.com/docs/api/nuxt-config#vite) option（👀 见下面示例） at your [`nuxt.config`](https://nuxt.com/docs/api/nuxt-config) file.

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

> 👀 这个配置和 Vue CLI 中的 `css.loaderOptions` 有点类似，详见 [Vue CLI Doc - CSS 相关 # 向预处理器 Loader 传递选项](https://cli.vuejs.org/zh/guide/css.html#%E5%90%91%E9%A2%84%E5%A4%84%E7%90%86%E5%99%A8-loader-%E4%BC%A0%E9%80%92%E9%80%89%E9%A1%B9)



#### Styling

Learn how to style your Nuxt application.

***

<font color=lightSeaGreen>Nuxt is highly flexible when it comes to styling</font>. Write your own styles, or reference local and external stylesheets. You can use CSS preprocessors, CSS frameworks, UI libraries and Nuxt modules to style your application.

##### Local Stylesheets

<font color=dodgerBlue>If you're writing local stylesheets</font>, <font color=red>the natural place to put them is the [`assets/` directory](https://nuxt.com/docs/guide/directory-structure/assets)</font>.

###### Importing Within Components

You can import stylesheets in your pages, layouts and components directly. You can use a javascript import, or a css [`@import` statement](https://developer.mozilla.org/en-US/docs/Web/CSS/@import).

```html
<!-- pages/index.vue -->
<script>
// Use a static import for server-side compatibility
import '~/assets/css/first.css'

// Caution: Dynamic imports are not server-side compatible // ⚠️
import('~/assets/css/first.css')
</script>

<style>
@import url("~/assets/css/second.css");
</style>
```

> 💡 The stylesheets will be inlined in the HTML rendered by Nuxt.

###### The CSS Property

You can also <font color=dodgerBlue>use the **`css` property** in the Nuxt configuration</font>. The natural place for your stylesheets is the [`assets/` directory](https://nuxt.com/docs/guide/directory-structure/assets). You can then reference its path and Nuxt will include it to all the pages of your application.

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  css: ['~/assets/css/main.css']
})
```

> 💡 The stylesheets will be inlined in the HTML rendered by Nuxt, injected globally and present in all pages.

###### Working With Fonts

<font color=red>Place your local fonts files in your `~/public/` directory</font>, for example in `~/public/fonts` . You can then reference them in your stylesheets using `url()`.

```css
// assets/css/main.css
@font-face {
  font-family: 'FarAwayGalaxy';
  src: url('/fonts/FarAwayGalaxy.woff') format('woff');
  font-weight: normal;
  font-style: normal;
  font-display: swap;
}
```

Then reference your fonts by name in your stylesheets, pages or components:

```html
<style>
h1 {
  font-family: 'FarAwayGalaxy', sans-serif;
}
</style>
```

###### Stylesheets Distributed Through NPM

You can also reference stylesheets that are distributed through npm. Let's use the popular `animate.css` library as an example.

```bash
npm install animate.css
```

Then you can <font color=red>reference it directly in your **pages, layouts and components**</font>:

```html
<script>
import 'animate.css'
</script>

<style>
@import url("animate.css");
</style>
```

The package can also be referenced as a string in the css property of your Nuxt configuration.

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  css: ['animate.css']
})
```

##### External Stylesheets

You can include external stylesheets in your application by adding a link element in the head section of your nuxt.config file. <font color=dodgerBlue>You can achieve this result using different methods</font>. <font color=red>Note that local stylesheets can also be included like this</font>.

You can manipulate the head with the [`app.head`](https://nuxt.com/docs/api/nuxt-config#head) property of your Nuxt configuration:

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  app: {
    head: {
      link: [{ rel: 'stylesheet', href: 'https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css' }]
    }
}})
```

###### Dynamically Adding Stylesheets

You can use the [`useHead`](https://nuxt.com/docs/api/composables/use-head) composable to <font color=red>**dynamically set a value**</font> in your head in your code.

```ts
useHead({
  link: [{ rel: 'stylesheet', href: 'https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css' }]
})
```

Nuxt uses `unhead` under the hood, and you can refer to its full documentation [here](https://unhead.unjs.io/).

###### Modifying The Rendered Head With A Nitro Plugin

<font color=dodgerBlue>If you need **more advanced control**</font>, you <font color=red>can **intercept the rendered html with a hook** and modify the head programmatically</font>.

Create a plugin in `~/server/plugins/my-plugin.ts` like this:

```ts
// server/plugins/my-plugin.ts
export default defineNitroPlugin((nitro) => {
  nitro.hooks.hook('render:html', (html) => {
    html.head.push('<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css">')
  })
})
```

<font color=dodgerBlue>External stylesheets are render-blocking resources</font>: they <font color=red>must be loaded and processed **before the browser renders the page**</font>. Web pages that contain unnecessarily large styles take longer to render. You can read more about it on [web.dev](https://web.dev/defer-non-critical-css) .

##### Using Preprocessors

To use a preprocessor like SCSS, Sass, Less or Stylus, install it first.

```bash
npm install sass
```

<font color=red>The natural place to write your stylesheets is the **`assets` directory**</font>. You can <font color=lightSeaGreen>then import your source files in your `app.vue`</font> (or layouts files) using your preprocessor's syntax.

```vue
<style lang="scss">
@use "~/assets/scss/main.scss";
</style>
```

<font color=dodgerBlue>**Alternatively**</font>, you can use the `css` property of your Nuxt configuration.

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  css: ['~/assets/scss/main.scss']
})
```

> 💡 In both cases, the compiled stylesheets will be inlined in the HTML rendered by Nuxt.

If you need to inject code in pre-processed files, like a [sass partial](https://sass-lang.com/documentation/at-rules/use#partials) with color variables, you can do so with the vite [preprocessors options](https://vitejs.dev/config/shared-options.html#css-preprocessoroptions) .

Create some partials in your `assets` directory:

```scss
$primary: #49240F;
$secondary: #E4A79D;
```

> 👀 这里略去了 SASS 的变量示例

Then in your `nuxt.config` :

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

Nuxt uses Vite by default. If you wish to use webpack instead, refer to each preprocessor loader [documentation](https://webpack.js.org/loaders/sass-loader).

##### Single File Components (SFC) Styling

One of the best thing about Vue and SFC is how great it is at naturally dealing with styling. <font color=lightSeaGreen>You can directly write CSS or preprocessor code in the style block of your components file</font>, therefore you will have fantastic developer experience without having to use something like CSS-in-JS. However <font color=dodgerBlue>if you wish to use CSS-in-JS</font>, you can find 3rd party libraries and modules that support it, such as [pinceau](https://pinceau.dev/).

> 👀 这里省略去了 Class And Style Bindings、Dynamic Styles With `v-bind`、Scoped Styles 感觉比较熟悉了

###### CSS Modules

You can use [CSS Modules](https://github.com/css-modules/css-modules) with the module attribute. <font color=red>Access it with the injected `$style` variable</font>.

```vue
<template>
  <p :class="$style.red">This should be red</p>
</template>

<style module>
.red {
  color: red;
}
</style>
```

###### Preprocessors Support

SFC style blocks support preprocessors syntax. Vite come with built-in support for .scss, .sass, .less, .styl and .stylus files without configuration. You just need to install them first, and they will be available directly in SFC with the lang attribute.

You can refer to the Vite CSS docs and the @vitejs/plugin-vue docs. For webpack users, refer to the vue loader docs.

##### Using PostCSS

<font color=red>Nuxt comes with **postcss built-in**</font>. You can configure it in your `nuxt.config` file.

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  postcss: {
    plugins: {
      'postcss-nested': {}
      "postcss-custom-media": {}
    }
  }
})
```

For proper syntax highlighting in SFC, <font color=fuchsia>you can use the postcss lang attribute</font>.

> 👀 如下写法完全没见过

```vue
<style lang="postcss">
  /* Write stylus here */
</style>
```

<font color=red>**By default**</font>, Nuxt comes with the following plugins already pre-configured:

- [postcss-import](https://github.com/postcss/postcss-import): Improves the `@import` rule
- [postcss-url](https://github.com/postcss/postcss-url): Transforms `url()` statements
- [autoprefixer](https://github.com/postcss/autoprefixer): Automatically adds vendor prefixes
- [cssnano](https://cssnano.co/): Minification and purge

###### Leveraging Layouts For Multiple Styles

If you need to style different parts of your application completely differently, you can use layouts. Use different styles for different layouts.

```vue
<template>
  <div class="default-layout">
    <h1>Default Layout</h1>
    <slot />
  </div>
</template>

<style>
.default-layout {
  color: red;
}
</style>
```

##### Third Party Libraries And Modules

Nuxt isn't opinionated when it comes to styling and provides you with a wide variety of options. <font color=lightSeaGreen>You can use any styling tool that you want</font>, such as popular libraries like [UnoCSS](https://unocss.dev/) or [Tailwind CSS](https://tailwindcss.com/).

The community and the Nuxt team have developed plenty of Nuxt modules to makes the integration easier. You can discover them on the [modules section](https://nuxt.com/modules) of the website. Here are a few modules to help you get started:

- [UnoCSS](https://nuxt.com/modules/unocss): Instant on-demand atomic CSS engine
- [Tailwind CSS](https://nuxt.com/modules/tailwindcss): Utility-first CSS framework
- [Fontaine](https://github.com/nuxt-modules/fontaine): Font metric fallback
- [Pinceau](https://pinceau.dev/): Adaptable styling framework
- [Nuxt UI](https://ui.nuxt.com/): A UI Library for Modern Web Apps

<font color=lightSeaGreen>Nuxt modules provide you with a good developer experience out of the box</font>, but remember that if your favorite tool doesn't have a module, it doesn't mean that you can't use it with Nuxt! You can configure it yourself for your own project. <font color=red>Depending on the tool, you might need to use a [Nuxt plugin](https://nuxt.com/docs/guide/directory-structure/plugins) and/or [make your own module](https://nuxt.com/docs/guide/going-further/modules)</font>. Share them with the [community](https://nuxt.com/modules) if you do!

###### Easily Load Webfonts

You can use [the Nuxt Google Fonts module](https://github.com/nuxt-modules/google-fonts) to load Google Fonts.

<font color=dodgerBlue>If you are using [UnoCSS](https://unocss.dev/integrations/nuxt)</font>, note that <font color=red>it comes with a [web fonts presets](https://unocss.dev/presets/web-fonts)</font> to conveniently load fonts from common providers, including Google Fonts and more.

#####  Advanced

###### Transitions

Nuxt comes with the same `<Transition>` element that Vue has, and also has support for the experimental [View Transitions API](https://nuxt.com/docs/getting-started/transitions#view-transitions-api-experimental).

###### Font Advanced Optimization

We would recommend using [Fontaine](https://github.com/nuxt-modules/fontaine) to reduce your [CLS](https://web.dev/cls). If you need something more advanced, consider creating a Nuxt module to extend the build process or the Nuxt runtime.

> 💡 Always remember to take advantage of the various tools and techniques available in the Web ecosystem at large to make styling your application easier and more efficient. Whether you're using native CSS, a preprocessor, postcss, a UI library or a module, Nuxt has got you covered. Happy styling!

###### LCP Advanced optimizations

You can do the following to speed-up the download of your global CSS files:

- Use a CDN so the files are physically closer to your users
- Compress your assets, ideally using Brotli
- Use HTTP2/HTTP3 for delivery
- Host your assets on the same domain (do not use a different subdomain)

Most of these things should be done for you automatically if you're using modern platforms like Cloudflare, Netlify or Vercel. You can find an LCP optimization guide on [web.dev](https://web.dev/optimize-lcp).

If all of your CSS is inlined by Nuxt, you can (experimentally) completely stop external CSS files from being referenced in your rendered HTML. You can achieve that with a hook, that you can place in a module, or in your Nuxt configuration file.

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  hooks: {
    'build:manifest': (manifest) => {
      // find the app entry, css list
      const css = manifest['node_modules/nuxt/dist/app/entry.js']?.css
      if (css) {
        // start from the end of the array and go to the beginning
        for (let i = css.length - 1; i >= 0; i--) {
          // if it starts with 'entry', remove it from the list
          if (css[i].startsWith('entry')) css.splice(i, 1)
        }
      }
    },
  },
})
```



#### Routing

Nuxt <font color=red>file-system routing</font> creates a route for every file in the `pages/` directory.

***

One core feature of Nuxt is the file system router. <font color=fuchsia>Every Vue file inside the [`pages/`](https://nuxt.com/docs/guide/directory-structure/pages) directory **creates a corresponding URL** (or route) that displays the contents of the file</font>. By using dynamic imports for each page, Nuxt leverages code-splitting to ship the minimum amount of JavaScript for the requested route.

##### Pages

Nuxt routing is based on [vue-router](https://router.vuejs.org/) and <font color=fuchsia>**generates the routes** from every component created in the [`pages/` directory](https://nuxt.com/docs/guide/directory-structure/pages), **based on their filename**</font>.

This file system routing uses naming conventions to create dynamic and nested routes:

```js
// Directory Structure
| pages/
---| about.vue
---| index.vue
---| posts/
-----| [id].vue // 👀 注意这里的文件名
```

```js
// Generated Router File
{
  "routes": [
    {
      "path": "/about",
      "component": "pages/about.vue"
    },
    {
      "path": "/",
      "component": "pages/index.vue"
    },
    {
      "path": "/posts/:id",
      "component": "pages/posts/[id].vue"
    }
  ]
}
```

##### Navigation

<font color=fuchsia>The [`<NuxtLink>`](https://nuxt.com/docs/api/components/nuxt-link) component links pages between them</font>. <font color=red>It renders an `<a>` tag with the `href` attribute set to the route of the page</font>. <font color=dodgerBlue>Once the application is hydrated</font>, page transitions are performed in JavaScript by updating the browser URL. This prevents full-page refreshes and allows for animated transitions.

<font color=dodgerBlue>When a [`<NuxtLink>`](https://nuxt.com/docs/api/components/nuxt-link) enters the viewport on the client side</font>, <font color=red>Nuxt will **automatically prefetch** components and **payload (generated pages) of the linked pages** ahead of time</font>, resulting in faster navigation.

```vue
<!-- pages/app.vue -->
<template>
  <header>
    <nav>
      <ul>
        <li><NuxtLink to="/about">About</NuxtLink></li>
        <li><NuxtLink to="/posts/1">Post 1</NuxtLink></li>
        <li><NuxtLink to="/posts/2">Post 2</NuxtLink></li>
      </ul>
    </nav>
  </header>
</template>
```

##### Route Middleware

Nuxt provides a customizable route middleware framework you can use throughout your application, ideal for extracting code that you want to run before navigating to a particular route.

> <font color=lightSeaGreen>**Route middleware runs within the Vue part of your Nuxt app**</font>. Despite the similar name, <font color=red>they are completely different from server middleware</font>, which are run in the Nitro server part of your app.

<font color=dodgerBlue>There are **three kinds of route middleware**:</font>

1. <font color=dodgerBlue>Anonymous (or inline) route middleware</font>, which are <font color=red>defined directly in the pages where they are used</font>.
2. <font color=dodgerBlue>Named route middleware</font>, which are <font color=red>**placed in the [`middleware/`](https://nuxt.com/docs/guide/directory-structure/middleware) directory**</font> and will <font color=red>be automatically loaded via asynchronous import</font> when used on a page. (**Note**: The route middleware name is normalized to kebab-case, so `someMiddleware` becomes `some-middleware`.)
3. <font color=dodgerBlue>Global route middleware</font>, which are <font color=red>placed in the [`middleware/` directory](https://nuxt.com/docs/guide/directory-structure/middleware) (with a `.global` suffix)</font> and will be automatically run on every route change.

<font color=dodgerBlue>Example of an **`auth` middleware protecting the `/dashboard` page**:</font>

```ts
//  middleware/auth.ts
export default defineNuxtRouteMiddleware((to, from) => {
  // isAuthenticated() is an example method verifying if a user is authenticated
  if (isAuthenticated() === false) {
    return navigateTo('/login')
  }
})
```

```vue
<!-- pages/dashboard.vue -->
<script setup lang="ts">
definePageMeta({
  middleware: 'auth'
})
</script>

<template>
  <h1>Welcome to your dashboard</h1>
</template>
```

##### Route Validation

<font color=red>Nuxt **offers route validation** via the `validate` property in [`definePageMeta()`](https://nuxt.com/docs/api/utils/define-page-meta)</font> in each page you wish to validate.

The <font color=red>`validate` property accepts the `route` as an argument</font>. You can <font color=lightSeaGreen>return a boolean value to determine whether or not this is a valid route to be rendered with this page</font>. If you return `false` , and another match can't be found, <font color=red>this will cause a **404 error**</font>. You <font color=red>can also **directly return an object with `statusCode` / `statusMessage` to respond immediately with an error**</font> (other matches will not be checked).

If you have a more complex use case, then you can use anonymous route middleware instead.

```vue
<!-- pages/posts/[id].vue -->
<script setup lang="ts">
definePageMeta({
  validate: async (route) => {
    // Check if the id is made up of digits
    return /^\d+$/.test(route.params.id)
  }
})
</script>
```



#### SEO and Meta

Improve your Nuxt app's SEO with powerful head config, composables and components.

***

##### Defaults

<font color=dodgerBlue>**Out-of-the-box**</font>, <font color=red>Nuxt provides sane defaults</font>, which you can override if needed.

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  app: {
    head: {
      charset: 'utf-8',
      viewport: 'width=device-width, initial-scale=1',
    }
  }
})
```

<font color=red>Providing an [`app.head`](https://nuxt.com/docs/api/nuxt-config#head) property in your [`nuxt.config.ts`](https://nuxt.com/docs/guide/directory-structure/nuxt-config)</font> <font color=lightSeaGreen>allows you to customize the head for your entire app</font>.

> 💡 This method does not allow you to provide reactive data. We recommend to use `useHead()` in `app.vue `.
>
> > 👀 相当于在 nuxt.config.ts 中的是静态配置，而通过 `useHead()` 可以通过编程方式动态实现配置；另外，这里只说了 `app.vue` ，没说其他 `.vue` 文件，应该是唯一的，类似于 `index.html`

Shortcuts are available to make configuration easier: `charset` and `viewport`. You can also provide any of the keys listed below in [Types](https://nuxt.com/docs/getting-started/seo-meta#types).

##### `useHead`

The [`useHead`](https://nuxt.com/docs/api/composables/use-head) composable function allows you to manage your head tags in a programmatic and reactive way, powered by [Unhead](https://unhead.unjs.io/).

As with all composables, it can only be used with a components `setup` and lifecycle hooks.

```vue
<!-- app.vue -->
<script setup lang="ts">
useHead({
  title: 'My App',
  meta: [
    { name: 'description', content: 'My amazing site.' }
  ],
  bodyAttrs: {
    class: 'test'
  },
  script: [ { innerHTML: 'console.log(\'Hello world\')' } ] // 👀
})
</script>
```

We recommend to take a look at the [`useHead`](https://nuxt.com/docs/api/composables/use-head) and [`useHeadSafe`](https://nuxt.com/docs/api/composables/use-head-safe) composables.

##### `useSeoMeta`

<font color=red>The [`useSeoMeta`](https://nuxt.com/docs/api/composables/use-seo-meta) composable lets you **define your site's SEO meta tags**</font> as a flat object with full TypeScript support.

This helps you avoid typos and common mistakes, such as using `name` instead of `property`.

```vue
<!-- app.vue -->
<script setup lang="ts">
useSeoMeta({
  title: 'My Amazing Site',
  ogTitle: 'My Amazing Site',
  description: 'This is my amazing site, let me tell you all about it.',
  ogDescription: 'This is my amazing site, let me tell you all about it.',
  ogImage: 'https://example.com/image.png',
  twitterCard: 'summary_large_image',
})
</script>
```

##### Components

<font color=red>Nuxt provides `<Title>`, `<Base>`, `<NoScript>`, `<Style>`, `<Meta>`, `<Link>`, `<Body>`, `<Html>` and `<Head>` components</font> so that <font color=dodgerBlue>you can interact directly with your metadata within your component's template</font>.

Because these component names match native HTML elements, it is <font color=lightSeaGreen>very important that they are **capitalized in the template**</font>.

`<Head>` and `<Body>` can accept nested meta tags (<font color=lightSeaGreen>for aesthetic reasons</font>) but this <font color=red>has no effect on *where* the nested meta tags are rendered in the final HTML</font>.

```vue
<!-- app.vue -->
<script setup lang="ts">
const title = ref('Hello World')
</script>

<template>
  <div>
    <Head>
      <Title>{{ title }}</Title>
      <Meta name="description" :content="title" />
      <Style type="text/css" children="body { background-color: green; }" />
    </Head>

    <h1>{{ title }}</h1>
  </div>
</template>
```

##### Types

Below are the non-reactive types used for [`useHead`](https://nuxt.com/docs/api/composables/use-head), [`app.head`](https://nuxt.com/docs/api/nuxt-config#head) and components.

```ts
interface MetaObject {
  title?: string
  titleTemplate?: string | ((title?: string) => string)
  templateParams?: Record<string, string | Record<string, string>>
  base?: Base
  link?: Link[]
  meta?: Meta[]
  style?: Style[]
  script?: Script[]
  noscript?: Noscript[];
  htmlAttrs?: HtmlAttributes;
  bodyAttrs?: BodyAttributes;
}
```

See [@unhead/schema](https://github.com/unjs/unhead/blob/main/packages/schema/src/schema.ts) for more detailed types.

##### Features

###### Reactivity

Reactivity is supported on all properties, as computed, getters and reactive.

<font color=red>It's recommended to use getters ( `() => value` ) over computed ( `computed(() => value)` )</font>.

```vue
<!-- useHead -->
<script setup lang="ts">
const description = ref('My amazing site.')

useHead({
  meta: [
    { name: 'description', content: description }
  ],
})
</script>
```

```vue
<!-- useSeoMeta -->
<script setup lang="ts">
const description = ref('My amazing site.')

useSeoMeta({
  description
})
</script>
```

```vue
<!-- Component -->
<script setup lang="ts">
const description = ref('My amazing site.')
</script>

<template>
  <div>
    <Meta name="description" :content="description" />
  </div>
</template>
```

###### Title Template

You can <font color=lightSeaGreen>use the `titleTemplate` option to provide a dynamic template for customizing the title of your site</font>. for example, by adding the name of your site to the title of every page.

The `titleTemplate` can either be a string, where `%s` is replaced with the title, or a function.

If you want to use a function (<font color=lightSeaGreen>for full control</font>), then <font color=lightSeaGreen>this cannot be set in your `nuxt.config`</font>, and <font color=red>it is recommended instead to set it within your `app.vue`</font> file, where it will apply to all pages on your site:

```vue
<script setup lang="ts">
useHead({
  titleTemplate: (titleChunk) => {
    return titleChunk ? `${titleChunk} - Site Title` : 'Site Title';
  }
})
</script>
```

Now, if you set the title to `My Page` with [`useHead`](https://nuxt.com/docs/api/composables/use-head) on another page of your site, the title would appear as 'My Page - Site Title' in the browser tab. You could also pass `null` to default to the site title.

###### Body Tags

You can <font color=dodgerBlue>use the `tagPosition: 'bodyClose'` option on applicable tags</font> to <font color=red>**append them to the end of the `<body>` tag**</font>.

```vue
<script setup lang="ts">
useHead({
  script: [
    {
      src: 'https://third-party-script.com',
      // valid options are: 'head' | 'bodyClose' | 'bodyOpen' // ⭐️
      tagPosition: 'bodyClose'
    }
  ]
})
</script>
```

##### Examples

###### With `definePageMeta`

<font color=dodgerBlue>Within your [`pages/` directory](https://nuxt.com/docs/guide/directory-structure/pages)</font>, you can <font color=red>use `definePageMeta` along with [`useHead`](https://nuxt.com/docs/api/composables/use-head) to set metadata based on the current route</font>.

For example, you can first set the current page title (this is extracted at build time via a macro, so it can't be set dynamically):

```vue
<!-- pages/some-page.vue -->
<script setup lang="ts">
definePageMeta({
  title: 'Some Page'
})
</script>
```

And then in your layout file, you might use the route's metadata you have previously set:

```vue
<!-- layouts/default.vue -->
<script setup lang="ts">
const route = useRoute()

useHead({
  meta: [{ property: 'og:title', content: `App Name - ${route.meta.title}` }]
})
</script>
```

##### Dynamic Title

In the example below, `titleTemplate` is <font color=red>set either as a string with the `%s` placeholder</font> <font color=dodgerBlue>**or**</font> <font color=red>as a `function`</font>, which allows greater flexibility in setting the page title dynamically for each route of your Nuxt app:

```vue
<!-- app.vue -->
<script setup lang="ts">
useHead({
  // as a string,
  // where `%s` is replaced with the title
  titleTemplate: '%s - Site Title',
  // ... or as a function
  titleTemplate: (productCategory) => {
    return productCategory
      ? `${productCategory} - Site Title`
      : 'Site Title'
  }
})
</script>
```

`nuxt.config` is also used as an alternative way of setting the page title. However, <font color=lightSeaGreen>`nuxt.config` does not allow the page title to be dynamic</font>. Therefore, it is <font color=red>recommended to use `titleTemplate` in the `app.vue` file to add a dynamic title</font>, which is then applied to all routes of your Nuxt app.

###### External CSS

<font color=dodgerBlue>The example below **shows how you might enable Google Fonts**</font> using either the `link` property of the [`useHead`](https://nuxt.com/docs/api/composables/use-head) composable or using the `<Link>` component:

```vue
<!-- useHead -->
<script setup lang="ts">
useHead({
  link: [
    {
      rel: 'preconnect',
      href: 'https://fonts.googleapis.com'
    },
    {
      rel: 'stylesheet',
      href: 'https://fonts.googleapis.com/css2?family=Roboto&display=swap',
      crossorigin: ''
    }
  ]
})
</script>
```

```vue
<!-- Components -->
<template>
  <div>
    <Link rel="preconnect" href="https://fonts.googleapis.com" />
    <Link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Roboto&display=swap" crossorigin="" />
  </div>
</template>
```



#### Transitions

Apply transitions between pages and layouts with Vue or native browser View Transitions.

***

> 💡 <font color=dodgerBlue>Nuxt leverages Vue's [`<Transition>`](https://vuejs.org/guide/built-ins/transition.html#the-transition-component) component to apply transitions between pages and layouts</font>.

##### Page transitions

You <font color=red>can enable page transitions to apply an **automatic transition for all your [pages](https://nuxt.com/docs/guide/directory-structure/pages)**</font>.

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  app: {
    pageTransition: { name: 'page', mode: 'out-in' }
  },
})
```

> 💡 <font color=dodgerBlue>If you are changing layouts as well as page, the **page transition** you set here will not run</font>. Instead, you should set a [layout transition](https://nuxt.com/docs/getting-started/transitions#layout-transitions).

To start adding transition between your pages, add the following CSS to your [`app.vue`](https://nuxt.com/docs/guide/directory-structure/app):

```vue
<!-- app.vue -->
<template>
  <NuxtPage />
</template>

<style>
.page-enter-active,
.page-leave-active {
  transition: all 0.4s;
}
.page-enter-from,
.page-leave-to {
  opacity: 0;
  filter: blur(1rem);
}
</style>
```

> 👀 这里有个示例，





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