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
  > >     { path: '/', component: '@/pages/index' },
  > >     { path: '/users', component: '@/pages/users' },
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

- `runtimeConfig` : Private or public tokens that <font color=red>need to be specified **<font size=4>after build</font> using environment variables**</font>.
- `app.config` : <font color=red>Public tokens that are <font size=4>**determined at build time**</font></font>, website configuration such as theme variant, title and any project config that are <font color=red>**not sensitive**</font>.

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

Nuxt provides a customizable route middleware framework you can use throughout your application, ideal for extracting code that <font color=lightSeaGreen>you want to run before navigating to a particular route</font>.

> <font color=lightSeaGreen>**Route middleware runs within the Vue part of your Nuxt app**</font>. Despite the similar name, <font color=red>they are **completely different from server middleware**</font>, which are <font color=lightSeaGreen>run in the Nitro server part of your app</font>.

<font color=dodgerBlue>There are **three kinds of route middleware**:</font>

1. <font color=dodgerBlue>**Anonymous (or inline)** route middleware</font>, which are <font color=red>defined directly in the pages where they are used</font>.
2. <font color=dodgerBlue>**Named** route middleware</font>, which are <font color=red>**placed in the `middleware/` directory**</font> and will <font color=red>be automatically loaded via asynchronous import</font> when used on a page. (**Note**: The route middleware name is normalized to kebab-case, so `someMiddleware` becomes `some-middleware`.)
3. <font color=dodgerBlue>**Global** route middleware</font>, which are <font color=red>placed in the `middleware/` directory (with a `.global` suffix)</font> and will be automatically run on every route change.

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

> 👀 这里有个示例效果视频，另外，除了上面的 `app.vue` （主要是 transition 效果实现），还有其他页面 `pages/index.vue` 和 `pages/about.vue` ，不过没什么东西，这里略

To set a different transition for a page, set the `pageTransition` key in [`definePageMeta`](https://nuxt.com/docs/api/utils/define-page-meta) of the page:

```vue
<!-- pages/about.vue -->
<script setup lang="ts">
definePageMeta({
  pageTransition: {
    name: 'rotate'
  }
})
</script>
```

```vue
<!-- app.vue -->
<template>
  <NuxtPage />
</template>

<style>
/* ... */
.rotate-enter-active,
.rotate-leave-active {
  transition: all 0.4s;
}
.rotate-enter-from,
.rotate-leave-to {
  opacity: 0;
  transform: rotate3d(1, 1, 1, 15deg);
}
</style>
```

> 👀 感觉这里还是使用了 Vue 中 `<transition>` 组件的 CSS class

##### Layout transitions

You can <font color=red>enable layout transitions to apply an automatic transition for all your [layouts](https://nuxt.com/docs/guide/directory-structure/layouts)</font>.

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  app: {
    layoutTransition: { name: 'layout', mode: 'out-in' }
  },
})
```

// TODO



#### Data fetching

Nuxt provides composables to handle data fetching within your application.

***

<font color=red>Nuxt comes with two composables and a built-in library</font> to perform data-fetching in browser or server environments: `useFetch`, [`useAsyncData`](https://nuxt.com/docs/api/composables/use-async-data) and `$fetch`.

In a nutshell:

- [`useFetch`](https://nuxt.com/docs/api/composables/use-fetch) is the <font color=lightSeaGreen>**most straightforward way**</font> to handle data fetching <font color=red>in a component setup function</font>.
- [`$fetch`](https://nuxt.com/docs/api/utils/dollarfetch) is great to make network requests based on user interaction.
- [`useAsyncData`](https://nuxt.com/docs/api/composables/use-async-data), combined with `$fetch`, offers more fine-grained control.

<font color=red>Both `useFetch` and `useAsyncData` share a common set of options and patterns</font> that we will detail in the last sections.

Before that, it's imperative to know why these composables exist in the first place.

##### Why using specific composables?

When using a framework like Nuxt that <font color=red>**can perform calls and render pages on both client and server environments**</font>, <font color=dodgerBlue>some challenges must be addressed</font>. This is why Nuxt provides composables to wrap your queries, instead of letting the developer rely on [`$fetch`](https://nuxt.com/docs/api/utils/dollarfetch) calls alone.

###### Network calls duplication

<font color=lightSeaGreen>The [`useFetch`](https://nuxt.com/docs/api/composables/use-fetch) and [`useAsyncData`](https://nuxt.com/docs/api/composables/use-async-data) composables ensure</font> that <font color=red>**once an API call is made on the server**, the data is properly forwarded to the client in the payload</font>.

The payload is a JavaScript object accessible through [`useNuxtApp().payload`](https://nuxt.com/docs/api/composables/use-nuxt-app#payload). It is used on the client to avoid refetching the same data when the code is executed in the browser.

###### Suspense

<font color=fuchsia>Nuxt uses Vue’s `<Suspense>` component under the hood to prevent navigation before every async data is available to the view</font>. The data fetching composables can help you leverage this feature and use what suits best on a per-calls basis.





## Codewhy 体系课 SSR 部分笔记



#### SSR 原理

SSR 应用的页面是在服务端渲染的，用户每请求一个 SSR 页面都会先在服务端进行渲染，然后将渲染好的页面返回给浏览器呈现。 

<img src="https://s2.loli.net/2023/11/25/JGEFzw3qTfhAp2I.png" alt="image-20231125164417268" style="zoom:50%;" />

这里会产生两个产物：`server_bundle.js` 和 `client_bundle.js` 。前者和静态页面相关，后者和页面的可交互性相关。另外，这里也有两个函数值得注意 [`createSSRApp`](https://cn.vuejs.org/api/application.html#createssrapp) 和 [`renderToString`](https://cn.vuejs.org/api/ssr.html#rendertostring) 。

在 Vue 中创建SSR应用，需要调用 createSSRApp 函数，而不是 createApp。

- `createApp` ：创建应用,直接挂载到页面上
- `createSSRApp` ：创建应用，是在激活的模式下挂载应用

 服务端用 `@vue/server-renderer` 包中的 `renderToString` 来进行渲染。

> `createSSRApp()` ：以 [SSR 激活](https://cn.vuejs.org/guide/scaling-up/ssr.html#client-hydration)模式创建一个应用实例。<font color=red>用法与 `createApp()` 完全相同</font>。
>
> 摘自：[Vue3 doc - 应用实例 API # `createSSRApp()`](https://cn.vuejs.org/api/application.html#createssrapp)

> [`renderToString()`](https://cn.vuejs.org/api/ssr.html#rendertostring) <font color=red>接收一个 **Vue 应用实例** 作为参数</font>，返回一个 Promise，当 Promise resolve 时得到应用渲染的 HTML。当然你也可以使用 [Node.js Stream API](https://nodejs.org/api/stream.html) 或者 [Web Streams API](https://developer.mozilla.org/zh-CN/docs/Web/API/Streams_API) 来执行流式渲染。
>
> 摘自：[Vue3 doc - 服务端渲染 (SSR) # 渲染一个应用](https://cn.vuejs.org/guide/scaling-up/ssr.html#rendering-an-app)



#### SSR 优缺点

##### 优点

###### 更快的首屏渲染速度

浏览器显示静态页面的内容要比 JavaScript 动态生成的内容快得多。<font color=red>当用户访问首页时可立即返回 **静态页面内容**</font>，而<font color=red>不需要等待浏览器先加载完整个应用程序</font>。

###### 更好的 SEO

<font color=red>**爬虫是最擅长爬取静态的 HTML 页面**</font>，服务器端直接返回一个静态的 HTML 给浏览器。这样有利于爬虫快速抓取网页内容，并编入索引，有利于 SEO。SSR 应用程序<font color=red>在 Hydration 之后依然可以保留 Web 应用程序的交互性</font>。比如：前端路由、响应式数据、虚拟DOM等。

#####  缺点

- SSR 通常需要对服务器进行更多 API 调用，以及在服务器端渲染需要消耗更多的服务器资源，成本高。
- 增加了一定的开发成本，用户需要关心哪些代码是运行在服务器端，哪些代码是运行在浏览器端。
- SSR 配置站点的缓存通常会比 SPA 站点要复杂一点。



#### SSR 的实现方案

1. 模版引擎：php、jsp...
2. 从零搭建 SSR 项目 ( Node + webpack/Vite + Vue/React )
3. 直接使用流行的框架（推荐）
   - React : Next.js
   -  Vue3 : Nuxt
   -  Angular : Anglular Universal
   - Svelte : Sapper



#### SSR 使用场景

- SaaS 产品：电子邮件网站、在线游戏、客户关系管理系统 ( CRM )、采购系统等
- 门户网站、电子商务、零售网站
- 单个页面、静态网站、文档类网站



#### 跨请求状态污染

<font color=dodgerBlue>在 SPA 中</font>，整个生命周期中只有一个 App 对象实例或一个 Router 对象实例或一个 Store 对象实例都是可以的，<font color=red>因为每个用户在使用浏览器访问 SPA 应用时，应用模块都会重新初始化</font>，这也是一种单例模式。

然而，<font color=dodgerBlue>**在 SSR 环境下**</font>，<font color=red>App 应用模块通常只在服务器启动时初始化一次</font>。同一个应用模块会在多个服务器请求之间被复用，而我们的单例状态对象也一样，也会在多个请求之间被复用，比如：当某个用户对共享的单例状态进行修改，那么这个状态可能会意外地泄露给另一个在请求的用户。<font color=red>这种情况称为：跨请求状态污染</font>。

<font color=dodgerBlue>为了避免这种跨请求转台污染，SSR 的解决方案是：</font>

<font color=red>在 **每个请求** 中为整个应用 **创建一个全新的实例**</font>，包括 Router 和全局 Store 实例。所以在创建 App、Router 或 Store 对象时都是使用一个函数来创建，保证每个请求都会创建一个全新的实例。这样也会有缺点：需要消耗更多的服务器的资源。

> 👀 有点类似于 Vue 组件 data 是一个函数



#### 现代 web 应用程序所需技术与 Nuxt 的选择

- 支持数据双向绑定 和 组件化（Nuxt 选择了 Vue.js）。

- 处理客户端的导航（Nuxt 选择了 vue-router）。

- 支持开发中热模块替换和生产环境代码打包（Nuxt 支持 webpack 5 和 Vite）。

- 兼容旧版浏览器，支持最新的 JavaScript 语法转译（Nuxt 使用 esbuild ）。

- 应用程序支持开发环境服务器，也支持服务器端渲染 或 API 接口开发。

- Nuxt 使用 [h3](https://github.com/unjs/h3) 来实现部署可移植性（ h3 是一个极小的高性能的 http 框架）

  > 👀 准确的说是：[Nitro](https://github.com/unjs/nitro)（ Nuxt 的服务引擎 ）基于 h3 开发。另外，Nitro 是 Nuxt3 才引入的

  如：支持在 Serverless、Workers 和 Node.js 环境中运行。

> 💡 补充
>
> h3 是一种 http 框架，这么说并不直观：类似的有 express。
>
> > 使用示例如下：
> >
> > ```js
> > import { createServer } from "node:http";
> > import { createApp, eventHandler, toNodeListener } from "h3";
> > 
> > const app = createApp();
> > app.use("/", eventHandler(() => "Hello world!"));
> > 
> > createServer(toNodeListener(app)).listen(process.env.PORT || 3000);
> > ```
> >
> > 摘自：[GitHub - h3 - readme](https://github.com/unjs/h3)
>
> Serverless 是指 vercel 和 netlify 之类的自动部署服务
>
> Worker 有 Cloudflare Workers

自 2016年10月以来，Nuxt 专门负责集成上述所描述的事情,并提供前端和后端的功能。Nuxt 框架可以用来快速构建下一个 Vue.js 应用程序，如支持 CSR、SSR、SSG 渲染模式的应用等。



#### Nuxt3 的特点

- Vue技术栈： Nuxt3 是基于 Vue3 + Vue Router + Vite 等技术栈，全程 Vue3+Vite 开发体验（Fast) 。
- 自动导包
  - Nuxt 会自动导入辅助函数、Composition API 和 Vue API，无需手动导入。
  - 基于规范的目录结构，Nuxt 还可以对自己的组件、插件使用自动导入。
- <font color=fuchsia>约定式路由</font>（目录结构即路由）：Nuxt 路由基于 vue-router，在 `pages/` 目录中创建的每个页面，都会根据目录结构和文件名来自动生成
- 渲染模式：Nuxt 支持多种渲染模式（SSR、CSR、SSG等）。具体来说包含：
  - 通用渲染（服务器端渲染和水合）
  - 仅客户端渲染
  - 全静态站点生成
  - <font color=red>混合渲染</font>（每条路由（自定义）缓存策略）
- 利于搜索引擎优化：服务器端渲染模式，不但可以提高首屏渲染速度，还利于 SEO
- 服务器引擎
  - 在开发环境中，它使用 Rollup 和 Node.js
  - <font color=dodgerBlue>在生产环境中</font>，使用 <font color=red>Nitro 将您的应用程序和服务器构建到一个通用 `.output` 目录中</font>。Nitro 服务引擎提供了跨平台部署的支持，包括 Node、Deno、 Serverless、Workers 等平台上部署。



#### Nuxt3 项目搭建

##### 环境配置

- Node 18 及以上

  > **Node.js** - [`v18.0.0`](https://nodejs.org/en) or newer
  >
  > **Node.js**: Make sure to use an even numbered version (18, 20, etc)
  >
  > 摘自：[Nuxt3 doc - Get Started - Installation # New Project](https://nuxt.com/docs/getting-started/installation#new-project)

- VS Code : Volar、ESLint、Prettier

##### 新建项目的方法

如下三种方法均可

- 使用 `npx` ：`npx nuxi init hello-nuxt` 
- 使用 `pnpm dlx` ：`pnpm dlx nuxi init hello-nuxt `
- 全局安装 nuxi ，并使用 `nuxt init hello-nuxt`

> 💡补充：在项目创建好了之后，可以通过 `npx nuxi add page pagePath/pageName` 来新建页面（这和 Nest CLI 有点类似）

##### Nuxt 项目介绍

###### 自带 npm scripts 介绍

```json
"scripts": {
  "build": "nuxt build", // 打包正式版本，通过 nitro 将会生成 .output 文件夹
  "dev": "nuxt dev", // 开发环境运行
  "generate": "nuxt generate", // 正式版本项目打包，另外，会提前预渲染每个路由。功能类似于 nuxt build --prerender
  "preview": "nuxt preview", // 打包项目之后的预览
  "postinstall": "nuxt prepare" // npm install 运行后运行。作用是：生成 .nuxt 文件夹和 ts 类型等
},
```

###### 目录结构

- `app.config.ts` ：应用程序的配置，比如可以放置一些常量
- `nuxt.config.ts` ：可定制的 Nuxt 框架的配置，比如 css、ssr、vite、app、modules 等
- `app.vue` ：整个应用程序
- `components/` ：组件目录
- `plugins/` ：插件目录
- `composables/ ` ：composition api 目录
- `aseets/` ：资源目录
- `public/` ：静态资源目录，不参与打包
- `pages/` ：定义页面文件
  - `pages/index.vue` ：项目的首页

##### 应用入口 app.vue

默认情况下，Nuxt 会将此文件视为入口点，并为应用程序的每个路由呈现其内容，常用于: 

- 定义页面布局 Layout 或 自定义布局，如：NuxtLayout
- 定义路由的占位，如：NuxtPage
- 编写全局样式
- 全局监听路由 等等



#### `nuxt.config.ts`

`nuxt.config.ts` 配置文件位于项目的根目录，可对 Nuxt 进行自定义配置。比如，可以进行如下配置：

##### `runtimeConfig`

运行时配置，即定义<font color=red>环境变量</font>。 不过，`runtimeConfig` 也算是有限制的，因为它在打完包之后，`runtimeConfig` 的内容将无法修改

`.env` 一般用于某些终端 <font color=red>**启动应用**</font> 时 <font color=red>**动态指定**</font> 配置，同时支持 dev 和 prod 。可通过 `.env` 文件中的环境变量来覆盖，<font color=red>`.env`  优先级大于 `runtimeConfig`</font> 。`.env` 的变量可以通过 `process.env` 获取，符合规则的会覆盖 `runtimeConfig` 的变量。示例如下：

`.env` 中定义的 `NUXT_APP_KEY` （通过 `process.env.NUXT_APP_KEY` 获取），可以覆盖 `defineNuxtConfig` 中定义的 `runtimeConfig.apiKey` ；类似的： `NUXT_PUBLIC_BASE_URL` 可以覆盖 `runtimeConfig.public.baseURL`

> 💡 如下是相关的一点补充

###### 如下方法可以修改运行时配置

- `"dev": "nuxt dev --port 3456"` 

- `"dev": "PORT=3456 nuxt dev"`
- 在 `.env` 定义 `PORT=3456` ，在执行 npm scripts 前将会自动找到

###### 判断当前环境（在 server 还是 client 中）

- 通过 `process.server` 和 `process.client` 来判断是否在 server 或 client 中
- 通过 `typeof window === 'object'` 来判断是否在 client 中（虽然感觉非常不严谨）

###### 示例

```ts
export default defineNuxtConfig({
  devtools: { enabled: true },
  runtimeConfig: {
    apiKey: 'foo', // 非 public 只有 server 可以访问。打包时，会被注入 server 的产物中
    public: { // public 中的定义，server 和 client 均可以访问。打包时，会被注入 server 和 client 的产物中
      baseURL: 'bar'
    }
  },
  appConfig: {
  }
})
```

##### `appConfig`

`appConfig` 是应用配置，定义在 构建时确定的公共变量，如：theme。`appConfig` 不仅可以在 `nuxt.config.js` 中定义，还可以抽离出来：用单独的文件 `app.config.ts` 定义。

`nuxt.config.ts` 中 `appConfig` 的配置会和 `app.config.ts` 中的配置合并。但合并时，难免会出现同名的配置，此时：`app.config.ts` 的优先级 大于 `nuxt.config.ts` 的 `appConfig`  。

另外，<font color=dodgerBlue>**和 `runtimeConfig` 不一致的是**</font>：`appConfig` 中的配置，默认在 server 和 client 两端都可以拿到

> 💡`runtimeConfig` 和 `app.config` 的区别
>
> runtimeConfig 和 app.config 都用于向应用程序公开变量。要确定是否应该使用其中一种，以下是一些指导原则:
>
> - runtimeConfig ：定义环境变量,比如：<font color=fuchsia>**运行时需要指定**</font>的 私有或 公共 token
> - app.config：定义公共变量，比如：在<font color=fuchsia>**构建时确定**</font>的公共 token、网站配置。
>
> 除此之外，还可以看下 [[#`runtimeConfig` vs `app.config`]]

##### `app`

 app 配置。常见的用法是在 `app` 中 定义 `head` 。

###### `head`

给每个页面上设置 head 信息，也支持 `useHead` 函数配置和 内置组件 `<Head>` 的配置，优先级依次为 `<Head>` 大于 `useHead` 函数 大于 `head`。

###### 示例

关于 `nuxt.config.ts` 中 `head` 的使用：

```ts
export default defineNuxtConfig({
  app: {
    head: {
      title: 'title',
      charset: 'utf-8',
      viewport: 'width=device-width, initial-scale=1',
      meta: [
        { name: 'keyword', content: 'keyword' }
        { name: 'description', content: 'description' }
      ],
      link: [
        { rel: 'shortcut icon' href='favicon.ico' type: 'image/x-icon' }
      ],
      style: [
        children: `body { background-color: green; }`                          
      ],
      script: [
        { script: 'scriptUrl' }  
      ],
    }
  }
})
```

关于 `.vue` 中 `useHead` 的使用：

```ts
useHead({
  meta: [
    { name: 'description', content: 'description' },
    // 甚至这里可以用一些常见的表达式，比如三目运算
    // 除此之外，如下有一个 body 选项，用来控制位置；比如这个 script 标签将会出现在 body 中，而不是 head 中
    script: [
      { src: 'srcUrl', body: true }
    ]
  ],
})
```

关于 `.vue` 中 `<Head>` 的使用：

```vue
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

##### `ssr`

指定应用渲染模式。通过布尔值来确定是 使用 SSR 还是 SPA

##### `router`

配置路由相关的信息，比如在客户端渲染可以配置 hash 路由

```ts
export default defineNuxtConfig({
  ssr: false, // 注意：ssr 为 true，只能用 history mode
  router: {
    options: {
      hashMode: true
    }
  }
})
```

##### `alias`

路径的别名，默认已配好

##### `modules`

配置 Nuxt 扩展的模块，比如: `@pinia/nuxt`、 `@nuxt/image`

##### `routeRules`

定义路由规则，可更改路由的渲染模式或分配基于路由缓存策略

##### `builder`

可指定用 vite 还是 webpack 来构建应用，默认是 vite。如切换为 webpack 还需要安装额外的依赖。

##### `css`

可用于设置全局样式

###### 全局样式

```ts
export default defineNuxtConfig({
  app: {
    css: ['cssPath', 'scssPath'], // 关于 scss 需要安装 scss，less 和 stylus 应该是类似的
  }
})
```

###### 自动导入

```ts
export default defineNuxtConfig({
  vite: {
    css: {
      preprocessorOptions: {
        scss: {
          additionalData: '@use "@/assets/_colors.scss" as *;'
        }
      }
    }
  }
})
```

这相当于在每一个 scss 模块（包含 `.scss` 文件 和 `<style lang="scss">` 等 ） 中添加 `@use "@/assets/_colors.scss" as *` 这个代码。另外，值得注意的是 `as *` 相当于是省略了命名空间，`as foo` 相当于给这个模块起了一个名为 `foo` 的命名空间（用于避免多个模块之间变量和 mixin 重复，出现相互覆盖的问题。比如 `foo.$color` 和 `bar.$color` ）；以上 `@use` 和 `as *` 都是 scss 的语法



#### Nuxt3 内置组件

Nuxt3 框架也提供一些内置的组件（无需导入，直接使用），常用的如下：

- SEO 组件：Html、Body、Head、Title、 Meta、 Style、 Link、 NoScript、 Base
- NuxtWelcome：欢迎页面组件，该组件是 `@nuxt/ui` 的一部分
- NuxtLayout：是 Nuxt自带的页面布局组件
- NuxtPage：是 Nuxt 自带的页面占位组件（ `<NuxtPage>` 是对 `<router-view>` 的封装 ）
  - 需要显示位于目录中的顶级或嵌套页面 `pages/`
  -  是对 router-view 的封装
- NuxtLink ：和 NuxtPage 类似，是对 `<router-link>` 的封装
- ClientOnly：该组件中的默认插槽的内容只在客户端渲染。指定哪些组件在客户端渲染，哪些组件在服务端渲染

##### NuxtLink

`<NuxtLink>` 是 Nuxt 内置组件，用来实现页面导航，是对 RouterLink 的扩展；比如：进入视口的链接启用预取资源（通过 `prefetch` 属性）等。

底层是一个 `<a>` 标签，因此使用 `<a>` + `href` 属性也支持路由导航；但用 `a` 标签导航会有触发浏览器默认刷新事件，而NuxtLink 不会；NuxtLink 还扩展了其它的属性和功能

应用 Hydration 后（已激活，可交互），页面导航会通过前端路由来实现。这可以防止整页刷新。当然，手动输入 URL 后，点击刷新浏览器也可导航，这会导致整个页面刷新

######  NuxtLink 组件属性

- `to` ：支持路由路径、<font color=fuchsia>**路由对象**</font>（👀 见下面示例）、URL

  ```vue
  <NuxtLink :to="{ path: '/foo' , query: { bar: 'quz'} }"></NuxtLink>
  ```

- `href` ：`to` 的别名

- `replace` ：默认为 `false` ，<font color=red>是否替换当前路由</font>

- `activeClass` ：激活链接的类名

- `target` ：和 `a` 标签的 `target` 一样，指定何种方式显示新页面

- 等等

> 💡 更多属性可以看下 [Nuxt doc - components - nuxt-link # Props](https://nuxt.com/docs/api/components/nuxt-link#props)



#### 编程导航

Nuxt3 除了可以通过 `<NuxtLink>` 内置组件来实现导航，同时也支持编程导航：`navigateTo` 。<font color=dodgerBlue>通过编程导航，在应用程序中就可以轻松实现动态导航</font>，但是<font color=fuchsia>**编程导航不利于 SEO**</font>（ 👀 这是之前完全没有想到的）。

`navigateTo` 函数在服务器端和客户端都可用，也可以在插件、中间件 中使用，也可以直接调用以执行页面导航，例如：当用户触发该 `goToProfile()` 方法时，我们通过 `navigateTo` 函数来实现动态导航。建议：`goToProfile` 方法总是返回 `navigateTo` 函数（该函数不需要导入）或返回异步函数

##### `navigateTo(to, options)` 函数

- `to` ：可以是纯字符串或 外部URL 或 路由对象

- `options` ： 导航配置，可选

  - `replace` ：默认为 `false` ，为 `true` 时会替换当前路由页面
  - `external` ：默认为 `false` ，不允许导航到外部连接，`true` 则允许
  - 等等。

  > 💡 更多属性可以看下 [Nuxt doc - api - `navigateTo`](https://nuxt.com/docs/api/utils/navigate-to)

> 💡 与 `navigateTo` 对应的有 [`abortNavigation`](https://nuxt.com/docs/api/utils/abort-navigation)
>
> > abortNavigation is a helper function that <font color=red>prevents navigation from taking place</font> and <font color=red>**throws an error** if one is set as a parameter</font>.
> >
> > 摘自：[Nuxt3 doc - api - utils - `abortNavigation`](https://nuxt.com/docs/api/utils/abort-navigation)

##### useRouter

 Nuxt3 的编程导航除了可以通过 `navigateTo` 实现，也支持 `useRouter`（或 Options API 的 `this.$router` )

###### `useRouter` 常用的 API

- `back` ：页面返回，功能同 `router.go(-1)`

- `forward` ：页面前进，功能同 `router.go(1)`

- `go` ：页面返回或前进，功能同 `router.go(-1)` 或 `router.go(1)`

- `push` ：以编程方式导航到新页面。建议改用 `navigateTo` ，支持性更好

- `replace` ：以编程方式导航到新页面，但会替换当前路由。建议改用 `navigateTo` ，支持性更好

  > 💡 关于上面所说的 “支持性更好” 或者兼容性更好，因为 `navigateTo` 在 server 和 client 两端都可用，而 `useRouter` 不会

- `beforeEach` ：路由守卫钩子，每次导航前执行（用于全局监听）

- `afterEach` ：路由守卫钩子，每次导航后执行（用于全局监听）

- 等等

> 💡 更多 API 详见 [Nuxt doc - api - `use-router`](https://nuxt.com/docs/api/composables/use-router)



#### 动态路由

Nuxt3 和 Vue一样，也支持动态路由，不过在 Nuxt3 中，动态路由也是根据目录结构和文件的名称自动生成。

##### 动态路由语法

页面组件目录 或 页面组件文件都支持 `[]` 方括号语法，方括号里编写动态路由的参数

```
pages/
--| about.vue
--| posts.vue
----| [id].vue
```

如上文件结构等价于：

```js
{
  "routers": [
    {
      "path": "/about",
      "components": "pages/about.vue"
    },
    {
      "path": "/posts/:id",
      "components": "pages/posts/[id].vue"
    },
  ]
}
```

##### 动态路由支持的写法

- `pages/detail/[id].vue`               -> `/detail/:id`
- `pages/detail/user-[id].vue`     -> `/detail/user-:id`
- `pages/detail/[role]/[id].vue` -> `/detail/:role/:id`
- `pages/detail-[role]/[id].vue` -> `/detail-:role/:id`

在 Nuxt 中，动态路由和 `index.vue` 可以共存；另外，Next.js 中也可以

###### 动态路由 `params` 的获取

动态路由参数 `params` 可以通过 `useRoute().params` 获取，示例如下

```ts
const route = useRoute()
const { param, param2, ... } = route.params
```

如上面代码所示：<font color=red>**`route.params` 是一个对象**</font>，其中包含路径中所有的 params。以 `prefix-[foo]/[bar]-suffix` 为例，`params` 的结构就是 `{ foo, bar }` 。

另外，值得注意的是，一般 params 对象中的属性是字符串；也有特殊情况，对于 `[...slug].vue` 中 `params` 一般是 `{ slug: [ ... ] }` ，以 `/foo/bar/quz` 为例，`params` 会是 `{ slug: ['foo', 'bar', 'quz'] }`

类似的， `query` 也可以通过 `useRoute().query` 获取：

```ts
const route = useRoute()
const { queryId } = route.query
```

##### 404 页面

可以通过定义 404 not found 页面来捕获所有不配路由，可以通过定义 `[...slug].vue` 文件来匹配所有无法匹配的路由；其中 `slug` 可以是其它字符串，比如 `[...foo].vue`

`[...slug].vue` 文件除了支持在 `pages/` 根目录下创建，也支持在其子目录中创建。

> 👀 感觉就是和作用域有点像？

Nuxt3 正式版不支持 `404.vue` 页面了（被 `[...slug].vue` 替代），开始的候选版是支持的 ；但 Next.js 支持。

404 页面( `[...slug].vue` ) 也可以通过 `useRoute().param.xxx` 来获取路由参数。



#### 路由中间件

Nuxt 提供了一个可定制的路由中间件，用来监听路由的导航，包括：局部和全局监听（支持再服务器和客户端执行）

##### 路由中间件分为三种

###### 匿名（或内联）路由中间件

在页面中使用 `definePageMeta` 函数定义，可监听局部路由。当注册多个中间件时，会按照注册顺序来执行。

```vue
<script setup lang="ts">
  definePageMeta({
    middleware: [
      function(to, from) {
        // do sth ...
        // return navigateTo('/prefix-11-suffix')
        // return abortNavigation()
      },
      function(to, from) { ... },
      /* ... */
    ]
  })
</script>
```

如果返回的是 `""` 、`null` ，或没有返回语句，name就会执行下一个路由中间件。如果返回的是 `navigateTo`  或者 `abortNavigation`，直接导航到新的页面，不会执行后面的路由中间件。注意，必须是 `return navigateTo()` 或者 `return abortNavigation()` ，没有 `return` 后面的路由中间件依然会执行

###### 命名路由中间件

在 `middleware/` 目录下定义，并会自动加载中间件（无需手动 import 导入），直接在 `definePageMeta.middleware` 中注册。在定义和使用时，需要遵循 kebab-case 命名规范

```ts
// middleware/named-middleware.ts
export default defineNuxtRouteMiddleware((to, from) => {
  console.log('named middleware')
})
```

```vue
<script setup lang="ts">
  definePageMeta({
    middleware: [
      'named-middleware',
      function (to, from) { ... },
      // ...
    ]
  })
</script>
```

###### 全局路由中间件

在 `middleware/` 目录下定义，名称需要加上 `.global` 后缀；比如 `glob-middleware.global.ts` 。定义相较命名路由中间件没什么区别，不过，因为是全局的，不需要在 `definePageMeta.middleware` 中注册即可全局生效，自动运行；<font color=fuchsia>所以 **每次路由跳转都会执行**</font>。

全局路由中间件的优先级是最高的，所以也会比其他两种中间件先执行。

另外，全局路由中间件是 <font color=red>client 和 server 端都支持</font>



#### 路由校验

Nuxt 支持对每个页面路由进行验证，可以通过 `definePageMeta` 中的 `validate` 属性对路由进行验证。

validate属性接受一个回调函数，回调函数中以 route 作为参数回调函数的返回值支持：

- 返回 bool 值来确定是否放行路由：`true` 放行路由，`false` 默认重定向到内置的 404 页面 。即，相当于 `{ stautsCode: 404 }` ）

- 返回对象：`{ statusCode: 401 }` ，即：返回自定义的 401 页面，验证失败。

  另外，这个返回对象中，还可以加上 `statusMessage` 属性，表示 401 的更多信息

路由验证失败，可以自定义错误页面：在项目根目录（不是 `pages/` 目录）新建 `error.vue` 。

在 `error.vue` 中，可以通过 `defineProp` 来接收抛出的 error 对象，并使用；示例如下

```ts
const prop = defineProp({
  error: Object
})
```



#### Layout

Layout 布局 是页面的包装器，可以将多个页面共性东西抽取到 Layout 中。例如：每个页面的页眉和页脚组件，这些具有共性的组件我们是可以写到一个 Layout 中。

Layout布局是使用 `<slot>` 组件来显示页面中的内容

##### Layout 有两种使用方式

- 默认布局： 在 `layouts/` 目录下新建默认的布局组件，比如：`layouts/default.vue` 然后在 `app.vue` 中通过 `<NuxtLayout>` 内置组件来使用

  ###### 定义示例

  ```vue
  <!-- layouts/default.vue -->
  <template>
    <div>
      <p>Some default layout content shared across all pages</p>
      <slot />
    </div>
  </template>
  ```

  ###### 使用示例

  ```vue
  <template>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </template>
  ```

- 自定义布局 ( Custom Layout ) ：在 `layouts/` 文件夹下新建 Layout 组件，比如：`layouts/custom-layout.vue` 然后在 `app.vue` 中给 `<NuxtLayout>` 内置组件指定 `name` 属性的值 `custom-layout` ；也支持在页面中使用 `definePageMeta` 宏函数来指定 layout 布局

  ```vue
  <!-- pages/about.vue -->
  <script setup lang="ts">
  definePageMeta({
    layout: 'custom'
  })
  </script>
  ```

  ```vue
  <script setup lang="ts">
  // You might choose this based on an API call or logged-in status
  const layout = "custom";
  </script>
  
  <template>
    <NuxtLayout :name="layout">
      <NuxtPage />
    </NuxtLayout>
  </template>
  ```



#### 渲染模式

浏览器和服务器都可以解释 JavaScript 代码，都可将 Vue.js 组件呈现为 HTML 元素，此过程称为渲染。

在客户端将 Vue.js 组件呈现为 HTML 元素，称为：客户端渲染模式。在服务器将 Vue.js 组件呈现为 HTML 元素，称为：服务器渲染模式

Nuxt3 支持多种渲染模式，比如：

- 客户端渲染模式 ( CSR )：只需将 `ssr` 设置为 false

- 服务器端渲染模式 ( SSR ) ：只需将 `ssr` 设置为 true

-  混合渲染模式 ( SSR / CSR / SSG / SWR ) ：需在 `routeRules` 根据每个路由动态配置渲染模式。示例如下：

  ```js
  export default defineNuxtConfig({
    routeRules: {
      // Homepage pre-rendered at build time
      '/': { prerender: true },
      // Product page generated on-demand, revalidates in background
      '/products/**': { swr: 3600 },
      // Blog post generated on-demand once until next deploy
      '/blog/**': { isr: true },
      // Admin dashboard renders only on client-side
      '/admin/**': { ssr: false },
      // Add cors headers on API routes
      '/api/**': { cors: true },
      // Redirects legacy urls
      '/old-page': { redirect: '/new-page' }
    }
  })
  ```



#### Nuxt 插件

Nuxt3 支持自定义插件进行扩展，创建插件有两种方式：

- 直接使用 `useNuxtApp()` 中的 `provide(name, vlaue)` 方法直接创建，比如：可在 `App.vue` 中创建
  `useNuxtApp` 提供了访问 Nuxt 共享运行时上下文的方法和属性（两端可用） ： provide, hooks, callhook, vueApp 等。示例如下：

  ```vue
  <!-- app.vue -->
  <script setup lang="ts">
    const nuxtApp = useNuxtApp()
    // 定义
    nuxtApp.provide('version', '1.0.0')
    
    // 使用
    console.log(nuxtApp.$version)
  </script>
  ```

  一般是在 app.vue 中定义。虽然也可以在一般的页面中定义，但只有访问这个页面时该插件才会被加载和注册，这显然限制过大，不方便使用。

- 在 `plugins/` 目录中创建插件（推荐）

  顶级和子目录index文件写的插件会在创建Vue应用程序时会自动加载和注册。

  `.server` 或 `.client` 后缀名插件（比如： `plugin.client.ts`），可区分服务器端或客户端，用时需区分环境，设置只能在哪一端使用

  因为插件之间可能会有顺序依赖，所以插件注册顺序可以通过在文件名前加上一个数字前缀来控制插件注册的顺序；比如 `1.plugin.ts`

  ###### 示例如下

  ```ts
  // plugins/plugin.ts
  export default defineNuxtPlugin(nuxtApp => {
    return {
      provide: {
        formatPrice: (price: number) => {
          return price.toFixed(2)
        },
        // ... 更多插件定义
      }
    }
  })
  ```

  

#### App lifecycle hooks

#####  监听App的生命周期的Hooks

 App Hooks 主要可由 Nuxt 插件 使用 hooks 来监听 生命周期，也可用于 Vue 组合 API。但是，如在组件中编写hooks来监听，那create和setup hooks就监听不了，因为这些hooks已经触发完了监听。

##### 语法

```js
nuxtApp.hook(app:lifecycleName, func)
```

##### Nuxt Hooks (build time) 列表

Check the [app source code](https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/nuxt.ts#L27) for all available hooks.

| Hook                     | Arguments           | Environment     | Description                                                  |
| ------------------------ | ------------------- | --------------- | ------------------------------------------------------------ |
| `app:created`            | `vueApp`            | Server & Client | Called when initial `vueApp` instance is created.            |
| `app:error`              | `err`               | Server & Client | Called when a fatal error occurs.                            |
| `app:error:cleared`      | `{ redirect? }`     | Server & Client | Called when a fatal error occurs.                            |
| `app:data:refresh`       | `keys?`             | Server & Client | (internal)                                                   |
| `vue:setup`              | -                   | Server & Client | (internal)                                                   |
| `vue:error`              | `err, target, info` | Server & Client | Called when a vue error propagates to the root component. [Learn More](https://vuejs.org/api/composition-api-lifecycle.html#onerrorcaptured). |
| `app:rendered`           | `renderContext`     | Server          | Called when SSR rendering is done.                           |
| `app:redirected`         | -                   | Server          | Called before SSR redirection.                               |
| `app:beforeMount`        | `vueApp`            | Client          | Called before mounting the app, called only on client side.  |
| `app:mounted`            | `vueApp`            | Client          | Called when Vue app is initialized and mounted in browser.   |
| `app:suspense:resolve`   | `appComponent`      | Client          | On [Suspense](https://vuejs.org/guide/built-ins/suspense.html#suspense) resolved event. |
| `link:prefetch`          | `to`                | Client          | Called when a `<NuxtLink>` is observed to be prefetched.     |
| `page:start`             | `pageComponent?`    | Client          | Called on [Suspense](https://vuejs.org/guide/built-ins/suspense.html#suspense) pending event. |
| `page:finish`            | `pageComponent?`    | Client          | Called on [Suspense](https://vuejs.org/guide/built-ins/suspense.html#suspense) resolved event. |
| `page:transition:finish` | `pageComponent?`    | Client          | After page transition [onAfterLeave](https://vuejs.org/guide/built-ins/transition.html#javascript-hooks) event. |



#### 组件生命周期

##### 组件生命周期钩子

因为没有任何动态更新，所以像 `mounted` 或者 `updated` 这样的生命周期钩子不会在 SSR 期间被调用，只会在客户端运行。<font color=red>只有 `beforeCreate` 和 `created`  这两个钩子会在 SSR 期间被调用</font>。

所以，应避免在 `beforeCreate` 和 `created` 中使用会产生副作用且需要被清理的代码。这类副作用的常见例子是使 `setInterval` 设置定时器。可能会在客户端特有的代码中设置定时器，然后在 `beforeUnmount` 或 `unmounted` 中清除。然而，由于 `unmount` 钩子不会在 SSR 期间被调用，所以定时器会永远存在；为了避免这种情况，请将含有副作用的代码放到 mounted 中。

##### 服务器端渲染的生命周期

只支持 beforeCreate， created ，如果使用 compostion api 还要加上 setup



#### 获取数据

在 Nuxt 中数据的获取主要是通过四种函数来实现（支持 Server 和 Client ）：

- `useAsyncData(key, func)` ：专门解决异步获取数据的函数,会阻止页面导航。

  发起异步请求需用到 `$fetch` 全局函数（类似 Fetch API），`$fetch(url, opts)`基于 [ofetch](https://github.com/unjs/ofetch)，一个类原生fetch 的跨平台请求库。另外，在刷新页面时，`useAsyncData` 可以减少客户端发起的网络请求

  ```ts
  const { data : count } = await useAsyncData('count', () => $fetch('/api/count') )
  ```

- `useFetch(url, options)` ：用于获取任意的URL地址的数据，会阻止页面导航

  本质是 `useAsyncData(key, () => $fetch(url, options) )` 的语法糖。

  默认会阻塞页面的导航（ `lazy` 默认为 `false` ），直到获取到数据，才会停止阻塞；如果要改变默认行为，阻止默认的阻塞，需要修改 `lazy` 为 `true` 。此外，还有一个简写为 `useLazyFetch` ，`lazy` 为 `true`

  ```ts
  const { data : count } = await useFetch('/api/count')
  ```

- `useLazyFetch(url, options)` ：用于获取任意 URL 数据，不会阻止页面导航。本质和 `useFetch` 的 `lazy` 属性设置为 `true` 一样

  ```ts
  const { pending, data : posts } = useLazyFetch('/api/posts')
  ```

- `useLazyAsyncData(key, func)` ：专门解决异步获取数据的函数。不会阻止页面导航

  本质和 `useAsyncData` 的 `lazy` 属性设置为 `true` 一样

###### 注意事项

这些函数只能用在 setup or Lifecycle Hooks 中

##### `useFetch` v.s. axios

获取数据的方法， Nuxt 推荐使用 `useFetch` 函数，而不是 axios

`useFetch` 底层调用的是 `$fetch` 函数，该函数基于 [unjs/ofetch](https://github.com/unjs/ofetch) 请求库，并与原生的 Fetch API 有者相同 API 

ofetch 是一个跨端请求库：

> GitHub repo About: 😱 A better fetch API. Works on node, browser and workers.

- 如果运行在服务器上，它可以智能的处理对 API 接口的直接调用。
- 如果运行在客户端行，它可以对后台提供的API接口正常的调用（类似 axios） ，也支持第三方接口的调用
- 会自动解析响应和对数据进行字符串化

`useFetch` 支持智能的类型提示和智能的推断 API 响应类型。

在 `setup` 中用 `useFetch` 获取数据，刷新页面时会减去客户端重复发起的请求。



#### Server API

可以在 Nuxt 项目中的 `server/api` 文件夹下（这是 Nuxt 约定的文件夹），写接口（如 `userInfoApi.ts` ），以供页面使用（比如作为 mock ）

```ts
// server/api/userApi.ts
export default defineEventHandler(event => {
  return {
    code: 200,
    data: { name: 'foo', age: 18 }
  }
})
```

调用接口也很简单，以下面的 `userInfoApi.ts` 为例：

```ts
const { code, userInfo } = await useFetch('api/userInfoApi')
```

就会返回 `ref` 响应式的 `userInfo` ， 

##### api 名称后面的 method 后缀

可以在 api 名称后面加上 http method 后缀，来约束请求的 http方法，比如 `userInfo.get.ts` ；如果对 `api/userInfo` 接口使用 post 方法，将会抛出 405 Method Not Allowed 状态码

##### event.node

`event.node` 对象和 一般的 node http 服务器类似，包含 `req` 和 `res` 两个属性对象，可以开始进行一般 node http 服务器的操作；比如获取：`req.method` 和 `req.url` 

##### 辅助函数

- `getQuery()` 可以获得 query
- `getMethod()` 可以获得 method
- `await readBody(event)` 可以获得 body
- `await readRawBody(event)` 可以获得原始的 body

- `useCookie(name, options)` 可以用来对 cookie 进行操作，看起来和 [js-cookie](https://github.com/js-cookie/js-cookie) 作用相同



#### 全局状态共享

Nuxt 跨页面、跨组件全局状态共享可使用 `useState`（支持 Server 和 Client ）

```ts
useState<T>(init?: () => T | Ref<T>): Ref<T>
useState<T>(key: string, init?: () => T | Ref<T>): Ref<T>
```

###### 参数

- `init` ：为状态提供初始值的函数，该函数也支持返回一个Ref类型
- `key` ：唯一key，确保在跨请求获取该数据时，保证数据的唯一性。为空时会根据文件和行号自动生成唯一key

###### 返回值

Ref 响应式对象

##### 使用步骤

一般放在 `composables/` 文件夹下，`composables/` 文件夹一般用于存放编写 hooks 模块，同时在 `composables/` 文件夹下定义的 hooks，会被自动导入；即：在使用时，无需手动导入

###### 定义

```ts
// composables/useCounter.ts
// 注意这里是匿名的函数，在使用时，名称参考文件名
export default function() {
  return useState('counter', () => 100)
}

// 另一种写法，这里是具名的；在使用时，名称按照定义的函数名
export const useCounter = () => {
  return useState('counter', () => 100)
}
```

###### 使用

```ts
const counter = useCounter()
counter.value ++
```

> ⚠️ 注意：这里 `counter.value ++` 执行完成之后，是所有使用 counter 的地方都会变化。所以，不要以为 counter 中的 `() => T` 是工厂函数，所以只有此处的 counter 会变，其他的不会；不是这样！！

##### 注意事项

`useState` 只能用在 `setup` 函数 和 Lifecycle Hooks 中。

`useState` 不支持 classes、functions 或者 symbols 类型，因为这些类型不支持序列化



#### Nuxt3 集成 Pinia

##### 安装

###### 安装 `@pinia/nuxt`

```sh
npm install @pinia/nuxt --save
```

`@pinia/nuxt` 会处理 state 同步问题，比如不需要关心序列化 或 XSS 攻击等问题

###### 安装 `pinia`

```sh
npm install pinia --save
```

> ⚠️ `@pinia/nuxt` 和 `pinia` 两个都要安装

如有遇到 pinia 安装失败，可以添加 `--legacy-peer-deps` 告诉 npm 忽略对等依赖并继续安装；或者使用 yarn 进行安装

##### 配置 pinia

在 nuxt.config.ts 中配置

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  modules: ['@pinia/nuxt']
})
```

在 `store/` 中写一般 pinia 的代码

```ts
import { defineStore } from 'pinia'

export const useUserStore = defineStore('user', {
  store: () => {
    return { ... }
  },
  actions: { ... }
})
```



#### useState v.s. Pinia

Nuxt 跨页面、跨组件全局状态共享，既可以使用 useState，也可以使用 Pinia，它们的异同如下：

##### 共同点

- 都支持全局状态共享，共享的数据

- 都是响应式数据都支持服务器端和客户端共享

##### Pinia 的优势

Pinia 比 useState 有更多的优势，比如:

- 开发工具支持 ( Devtools )
  - 跟踪动作，更容易调试
  - store 可以出现在使用它的组件中
- 模块热更换
  - 无需重新加载页面即可修改 store 数据
  - 在开发时保持任何现有状态
- 插件：可以使用插件扩展 Pinia 功能
- 提供适当的 TypeScript 支持或自动完成



#### 集成 Element Plus

步骤如下

##### 安装依赖

```sh
npm install element-plus --save
npm install unplugin-element-plus --save-dev
```

##### 配置

- 配置Babel对EP的转译
- 配置自动导入样式

```ts
import ElementPlus from 'unplugin-element-plus/vite'

export default defineNuxtcConfig({
  build: {
    // 使用 babel 进行语法转换
    transpile: ['element-plus/es']
  },
  vite: {
    // 自动导入样式
    plugins: [ElementPlus()]
  }
})
```

##### 在组件中导入并使用

> 👀 似乎还不支持自动导入，只能通过手动导入

```vue
<template>
  <el-button type="primary">button</el-button>
</template>

<script setup lang="ts">
import { ElButton } from 'element-plus'
</script>
```





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