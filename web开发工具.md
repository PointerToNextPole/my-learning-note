# web开发工具



## Axios



#### 引言

Axios是一个 **异步请求** 技术，即：基于XMLHttpRequest对象发起的请求都是异步请求
**异步请求特点：**请求之后页面不动，响应回来更新的是页面的局部，多个请求之间互不影响，并行执行

**为什么不推荐使用AJAX？：**ajax确实用来发送异步请求，但：

> - Ajax本身是针对MVC编程，不符合前端MVVM的浪潮
> - Ajax基于原生XHR开发，XHR本身的架构不清晰，已经有了fetch的替代方案，jquery整个项目太大，单纯使用ajax却要引入整个jquery非常不合理（采取个性化打包方案又不能享受cdn服务）
> - Ajax不支持浏览器的back按钮
> - 安全问题ajax暴露了与服务器交互的细节
> - 对搜索引擎的支持比较弱
> - 破坏程序的异常机制
> - 不容易调试
>
> 摘自：[ajax和axios的区别是什么？](https://www.html.cn/qa/frontend/19023.html)
同时Vue的作者尤雨溪也推荐使用Axios



#### Axios Post

注意：Axios在发送post方式的<font color=FF0000>**请求时传递的参数如果为对象类型**，axios会自动将对象转为json格式的字符串使用 application/json 的请求头向后端服务接口传递参数</font>。而这时后端无法正常获取到前端传递来的参数。

<mark>如果想让后端接收到参数header的格式必须是：application/www-x-form-urlencoded 才可以，如果是application/json不行。</mark>

<font color=FF0000>**Axios的post请求传递参数的两种方式**</font>

- 使用字符串直接传递（即键值对的拼接），示例："fooKey=fooVal&barKey=barVal"
- 后端接口直接使用@RequestBody注解形式接收参数（Spring）。这时候就需要前端必须传递json格式的字符串且header必须是application/json格式



#### 并发请求

示例如下：

```js
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(), getUserPermissions()])
  .then(axios.spread(function (acct, perms) {
    // 两个请求现在都执行完成
  }));
```

其中：

- axios.all() 用来处理并发请求
- axios.spread() 用来批量处理多个并发请求的执行结果



#### Axios的RESTful风格的API

- **axios.request(config)**
- **axios.get(url[, config])**
- **axios.delete(url[, config])**
- **axios.head(url[, config])**
- **axios.options(url[, config])**
- **axios.post(url[, data[, config]])**
- **axios.put(url[, data[, config]])**
- **axios.patch(url[, data[, config]])**



#### Axios的配置对象

可以使用自定义配置新建一个 axios 实例

**axios.create([config])**

```
const instance = axios.create({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});
```

**实例方法**
以下是可用的实例方法。指定的配置将与实例的配置合并。

- **axios#request(config)**
- **axios#get(url[, config])**
- **axios#delete(url[, config])**
- **axios#head(url[, config])**
- **axios#options(url[, config])**
- **axios#post(url[, data[, config]])**
- **axios#put(url[, data[, config]])**
- **axios#patch(url[, data[, config]])**



#### 请求配置

这些是创建请求时可以用的配置选项。只有 `url` 是必需的。如果没有指定 `method`，请求将默认使用 `get` 方法。

```js
{
   // `url` 是用于请求的服务器 URL
  url: '/user',

  // `method` 是创建请求时使用的方法
  method: 'get', // default

  // `baseURL` 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL。
  // 它可以通过设置一个 `baseURL` 便于为 axios 实例的方法传递相对 URL
  // 个人理解：baseURL相当于对于不同参数的URL，其中共用的URL前缀
  baseURL: 'https://some-domain.com/api/',

  // `transformRequest` 允许在向服务器发送前，修改请求数据
  // 只能用在 'PUT', 'POST' 和 'PATCH' 这几个请求方法
  // 后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或 Stream
  transformRequest: [function (data, headers) {
    // 对 data 进行任意转换处理
    return data;
  }],

  // `transformResponse` 在传递给 then/catch 前，允许修改响应数据
  transformResponse: [function (data) {
    // 对 data 进行任意转换处理
    return data;
  }],

  // `headers` 是即将被发送的自定义请求头
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // `params` 是即将与请求一起发送的 URL 参数
  // 必须是一个无格式对象(plain object)或 URLSearchParams 对象
  params: {
    ID: 12345
  },

   // `paramsSerializer` 是一个负责 `params` 序列化的函数
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function(params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
  },

  // `data` 是作为请求主体被发送的数据
  // 只适用于这些请求方法 'PUT', 'POST', 和 'PATCH'
  // 在没有设置 `transformRequest` 时，必须是以下类型之一：
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - 浏览器专属：FormData, File, Blob
  // - Node 专属： Stream
  data: {
    firstName: 'Fred'
  },

  // `timeout` 指定请求超时的毫秒数(0 表示无超时时间)
  // 如果请求话费了超过 `timeout` 的时间，请求将被中断
  timeout: 1000,

   // `withCredentials` 表示跨域请求时是否需要使用凭证
  withCredentials: false, // default

  // `adapter` 允许自定义处理请求，以使测试更轻松
  // 返回一个 promise 并应用一个有效的响应 (查阅 [response docs](#response-api)).
  adapter: function (config) {
    /* ... */
  },

 // `auth` 表示应该使用 HTTP 基础验证，并提供凭据
  // 这将设置一个 `Authorization` 头，覆写掉现有的任意使用 `headers` 设置的自定义 `Authorization`头
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

   // `responseType` 表示服务器响应的数据类型，可以是 'arraybuffer', 'blob', 'document', 'json', 'text', 'stream'
  responseType: 'json', // default

  // `responseEncoding` indicates encoding to use for decoding responses
  // Note: Ignored for `responseType` of 'stream' or client-side requests
  responseEncoding: 'utf8', // default

   // `xsrfCookieName` 是用作 xsrf token 的值的cookie的名称
  xsrfCookieName: 'XSRF-TOKEN', // default

  // `xsrfHeaderName` is the name of the http header that carries the xsrf token value
  xsrfHeaderName: 'X-XSRF-TOKEN', // default

   // `onUploadProgress` 允许为上传处理进度事件
  onUploadProgress: function (progressEvent) {
    // Do whatever you want with the native progress event
  },

  // `onDownloadProgress` 允许为下载处理进度事件
  onDownloadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

   // `maxContentLength` 定义允许的响应内容的最大尺寸
  maxContentLength: 2000,

  // `validateStatus` 定义对于给定的HTTP 响应状态码是 resolve 或 reject  promise 。如果 `validateStatus` 返回 `true` (或者设置为 `null` 或 `undefined`)，promise 将被 resolve; 否则，promise 将被 rejecte
  validateStatus: function (status) {
    return status >= 200 && status < 300; // default
  },

  // `maxRedirects` 定义在 node.js 中 follow 的最大重定向数目
  // 如果设置为0，将不会 follow 任何重定向
  maxRedirects: 5, // default

  // `socketPath` defines a UNIX Socket to be used in node.js.
  // e.g. '/var/run/docker.sock' to send requests to the docker daemon.
  // Only either `socketPath` or `proxy` can be specified.
  // If both are specified, `socketPath` is used.
  socketPath: null, // default

  // `httpAgent` 和 `httpsAgent` 分别在 node.js 中用于定义在执行 http 和 https 时使用的自定义代理。允许像这样配置选项：
  // `keepAlive` 默认没有启用
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // 'proxy' 定义代理服务器的主机名称和端口
  // `auth` 表示 HTTP 基础验证应当用于连接代理，并提供凭据
  // 这将会设置一个 `Proxy-Authorization` 头，覆写掉已有的通过使用 `header` 设置的自定义 `Proxy-Authorization` 头。
  proxy: {
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // `cancelToken` 指定用于取消请求的 cancel token
  // （查看后面的 Cancellation 这节了解更多）
  cancelToken: new CancelToken(function (cancel) {
  })
}
```



#### 响应结构

某个请求的响应包含以下信息

```
{
  // `data` 由服务器提供的响应
  data: {},

  // `status` 来自服务器响应的 HTTP 状态码
  status: 200,

  // `statusText` 来自服务器响应的 HTTP 状态信息
  statusText: 'OK',

  // `headers` 服务器响应的头
  headers: {},

   // `config` 是为请求提供的配置信息
  config: {},
 // 'request'
  // `request` is the request that generated this response
  // It is the last ClientRequest instance in node.js (in redirects)
  // and an XMLHttpRequest instance the browser
  request: {}
}
```

使用 then 时，你将接收下面这样的响应 :

```
axios.get('/user/12345')
  .then(function(response) {
    console.log(response.data);
    console.log(response.status);
    console.log(response.statusText);
    console.log(response.headers);
    console.log(response.config);
  });
```

在使用 catch 时，或传递 rejection callback 作为 then 的第二个参数时，响应可以通过 error 对象可被使用，正如在错误处理这一节所讲。





## gem

Gem是封装起来的Ruby应用程序或代码库，是 Ruby 模块 (叫做 Gems) 的包管理器。

- **gem包安装**

  ```sh
  gem install packageName     //安装指定gem包，先从本机查找gem包并安装，若本地没有则从远程gem安装。
  gem install -l packageName      //仅从本机安装gem包
  gem install -r packageName      //仅从远程安装gem包
  gem install packageName --no-ri --no-rdoc       //安装gem包，但不安装相关文档文件
  gem install packageName --vision=[ver]      //安装指定版本的gem包12345
  ```

- **卸载gem包**

  ```sh
  gem uninstall packageName
  ```

- **查看已安装的包**

  ```sh
  gem list    //查看本机已安装的所有gem包
  gem list -r keyword     //列出远程RubyGems.org 上有此关键字的gem包（可用正则表达式）
  gem list -r>remote_gem_list.txt     //列出远程RubyGems.org 上所有Gmes清单，并保存到文件。
  gem server      //查看所有gem包文档及资料1234
  ```

- **更新安装包**

  ```sh
  gem update --system     //更新升级RubyGems软件自身
  gem update      //更新所有已安装的gem包
  gem update packageName      //更新指定的gem包
  ```

摘自：[npm和gem](https://blog.csdn.net/u011099640/article/details/53083845)