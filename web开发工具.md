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



## WebSocket

WebSocket <font color=FF0000>是一种网络通信**协议**</font>，很多高级功能都需要它。

#### 为什么需要 WebSocket？

> 我们已经有了 HTTP 协议，为什么还需要另一个协议？它能带来什么好处？

答案很简单，因为 <font color=FF0000>HTTP 协议有一个缺陷：通信只能由客户端发起</font>。

这种单向请求的特点，注定了如果服务器有连续的状态变化，客户端要获知就非常麻烦。我们只能使用"轮询"：每隔一段时候，就发出一个询问，了解服务器有没有新的信息。最典型的场景就是聊天室。

轮询的效率低，非常浪费资源（因为必须不停连接，或者 HTTP 连接始终打开）。因此，工程师们一直在思考，有没有更好的方法。WebSocket 就是这样发明的。

#### 简介

WebSocket 协议在2008年诞生，2011年成为国际标准。所有浏览器都已经支持了。

**补充：**WebSocket<font color=FF0000>是HTML5开始提供的一种浏览器与服务器进行**全双工**通讯</font>（双向通信）<font color=FF0000>的网络技术</font>，<font color=FF0000>属于应用层协议</font>。它基于TCP传输协议，并复用HTTP的握手通道。

它的<font color=FF0000>最大特点</font>就是，<font color=FF0000>服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话</font>，属于服务器推送技术的一种。

**其他特点包括：**

- <font color=FF0000>建立在 **TCP 协议**之上</font>，服务器端的实现比较容易。

- <font color=FF0000>与 HTTP 协议有着良好的兼容性。**默认端口**也是80和443，并且握手阶段采用 HTTP 协议</font>，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器。

- <font color=FF0000>数据格式比较轻量，性能开销小，通信高效</font>。

- <font color=FF0000>可以发送文本</font>，也可以发送二进制数据。

- <font color=FF0000>没有同源限制</font>，客户端可以与任意服务器通信。

- <font color=FF0000>协议标识符是ws</font>（如果加密，则为wss），服务器网址就是 URL。

  ```
  ws://example.com:80/some/path
  ```

**补充：**特点

- 支持双向通信，实时性更强。

- <font color=FF0000>更好的二进制支持</font>。

- <font color=FF0000>较少的控制开销</font>：<font color=FF0000>连接创建后，ws客户端、服务端进行数据交换时，协议控制的数据包头部较小</font>。在不包含头部的情况下，服务端到客户端的包头只有2~10字节（取决于数据包长度），客户端到服务端的的话，需要加上额外的4字节的掩码。而HTTP协议每次通信都需要携带完整的头部。

- <font color=FF0000>支持扩展</font>：ws协议定义了扩展，<font color=FF0000>用户可以扩展协议，或者实现自定义的子协议</font>。（比如支持自定义压缩算法等）

#### 简单示例

```js
var ws = new WebSocket("wss://echo.websocket.org");

ws.onopen = function(evt) { 
  console.log("Connection open ..."); 
  ws.send("Hello WebSockets!");
};

ws.onmessage = function(evt) {
  console.log( "Received Message: " + evt.data);
  ws.close();
};

ws.onclose = function(evt) {
  console.log("Connection closed.");
};
```

#### 属性

- **WebSocket.binaryType：**使用二进制的数据类型连接。
- **WebSocket.bufferedAmount：** 只读，未发送至服务器的字节数。

- **WebSocket.extensions：** 只读，服务器选择的扩展。

- **WebSocket.onclose：**用于指定连接关闭后的回调函数。
- **WebSocket.onerror：**用于指定连接失败后的回调函数。
- **WebSocket.onmessage：**用于指定当从服务器接受到信息时的回调函数。
- **WebSocket.onopen：**用于指定连接成功后的回调函数。
- **WebSocket.protocol ：**只读，服务器选择的下属协议。
- **WebSocket.readyState：** 只读，当前的链接状态。
- **WebSocket.url：** 只读，WebSocket 的绝对路径。

#### 客户端API

- **构造函数**

  WebSocket 对象作为一个构造函数，用于新建 WebSocket 实例。

  ```js
  var ws = new WebSocket('ws://localhost:8080');
  ```

  <font color=FF0000>执行上面语句之后，客户端就会与服务器进行连接。</font>

  **语法：**

  ```js
  WebSocket(url[, protocols])
  ```

  返回一个 WebSocket 对象。

- **webSocket.readyState**

  readyState属性返回实例对象的当前状态，共有四种。

  - **CONNECTING：**值为<font color=FF0000>**0**</font>，表示<font color=FF0000>正在连接</font>
  - **OPEN：**值为<font color=FF0000>**1**</font>，表示<font color=FF0000>连接成功</font>，可以通信了
  - **CLOSING：**值为<font color=FF0000>**2**</font>，表示<font color=FF0000>连接**正在**关闭</font>
  - **CLOSED：**值为<font color=FF0000>**3**</font>，表示<font color=FF0000>连接**已经**关闭</font>，<font color=FF0000>**或者**打开连接失败</font>

- **webSocket.onopen**

  实例对象的onopen属性，用于<font color=FF0000>指定**连接成功后**的**回调函数**</font>。

  ```js
  ws.onopen = function () {
    ws.send('Hello Server!');
  }
  ```

  如果<font color=FF0000>要指定多个回调函数，可以使用addEventListener方法</font>。

  ```js
  ws.addEventListener('open', function (event) {
    ws.send('Hello Server!');
  });
  ```

- **webSocket.onclose**

  实例对象的onclose属性，用于<font color=FF0000>指定**连接关闭后**的**回调函数**</font>。

  ```js
  ws.onclose = function(event) {
    var code = event.code;
    var reason = event.reason;
    var wasClean = event.wasClean;
    // handle close event
  };
  
  ws.addEventListener("close", function(event) {
    var code = event.code;
    var reason = event.reason;
    var wasClean = event.wasClean;
    // handle close event
  });
  ```

- **webSocket.onmessage**

  实例对象的onmessage属性，用于指定<font color=FF0000>收到服务器数据后</font>的<font color=FF0000>回调函数</font>。

  ```js
  ws.onmessage = function(event) {
    var data = event.data;
    // 处理数据
  };
  
  ws.addEventListener("message", function(event) {
    var data = event.data;
    // 处理数据
  });
  ```

  注意，<font color=FF0000>服务器数据可能是文本，也可能是二进制数据（blob对象或Arraybuffer对象）</font>。

  ```js
  ws.onmessage = function(event){
    if(typeof event.data === String) {
      console.log("Received data string");
    }
  
    if(event.data instanceof ArrayBuffer){
      var buffer = event.data;
      console.log("Received arraybuffer");
    }
  }
  ```

  <font color=FF0000>除了动态判断收到的数据类型，也可以使用binaryType属性，显式指定收到的二进制数据类型</font>。

  ```js
  // 收到的是 blob 数据
  ws.binaryType = "blob";
  ws.onmessage = function(e) {
    console.log(e.data.size);
  };
  
  // 收到的是 ArrayBuffer 数据
  ws.binaryType = "arraybuffer";
  ws.onmessage = function(e) {
    console.log(e.data.byteLength);
  };
  ```

- **webSocket.send()**

  实例对象的send()方法<font color=FF0000>用于向服务器发送数据</font>。

  - 发送文本的例子

    ```js
    ws.send('your message');
    ```

  - 发送 Blob 对象的例子

    ```js
    var file = document
      .querySelector('input[type="file"]')
      .files[0];
    ws.send(file);
    ```

  - 发送 ArrayBuffer 对象的例子

    ```js
    // Sending canvas ImageData as ArrayBuffer
    var img = canvas_context.getImageData(0, 0, 400, 320);
    var binary = new Uint8Array(img.data.length);
    for (var i = 0; i < img.data.length; i++) {
      binary[i] = img.data[i];
    }
    ws.send(binary.buffer);
    ```

- **webSocket.bufferedAmount**

  实例对象的bufferedAmount属性，<font color=FF0000>表示还有多少字节的二进制数据没有发送出去</font>。它<font color=FF0000>可以用来判断发送是否结束</font>。

  ```js
  var data = new ArrayBuffer(10000000);
  socket.send(data);
  
  if (socket.bufferedAmount === 0) {
    // 发送完毕
  } else {
    // 发送还没结束
  }
  ```

- **webSocket.onerror**

  实例对象的onerror属性，<font color=FF0000>用于指定报错时的回调函数</font>。

  ```js
  socket.onerror = function(event) {
    // handle error event
  };
  
  socket.addEventListener("error", function(event) {
    // handle error event
  });
  ```

以上摘自：[阮一峰 - WebSocket 教程](http://www.ruanyifeng.com/blog/2017/05/websocket.html)，补充内容摘自：[MDN - WebSocket](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket) 和 [WebSocket协议：5分钟从入门到精通](https://zhuanlan.zhihu.com/p/32739737)（另外，该文章中包含大量原理性的内容，比如报文格式等，由于HTTP相关遗忘了大半，所以以后再看）**//TODO**



## Server Sent Event (SSE)

#### 使用服务器发送事件

在Web应用程序中使用服务器发送事件很简单。<mark>在服务器端，只需要按照一定的格式返回事件流，在客户端中，只需要为一些事件类型绑定监听函数，和处理其他普通的事件没多大区别</mark>。

**从服务器接受事件**

<font color=FF0000>服务器发送事件API也就是 **EventSource接口**</font>，在你创建一个新的EventSource对象的同时，你可以指定一个接受事件的URI。例如：

```js
const evtSource = new EventSource("ssedemo.php");
```

<font color=FF0000>如果发送事件的脚本<font size=4>**不同源**</font>，**应该创建一个新的包含URL和options参数的EventSource对象**</font>。例如，假设客户端脚本在example.com上：

```js
const evtSource = new EventSource("//api.example.com/ssedemo.php", { withCredentials: true } );
```

一旦你成功初始化了一个事件源，就可以<font color=FF0000>（在浏览器端）对 message 事件添加一个处理函数开始监听从服务器发出的消息</font>了:

```js
evtSource.onmessage = function(event) {
  const newElement = document.createElement("li");
  const eventList = document.getElementById("list");

  newElement.innerHTML = "message: " + event.data;
  eventList.appendChild(newElement);
}
```

上面的代码监听了那些从服务器发送来的所有没有指定事件类型的消息(没有event字段的消息)，然后把消息内容显示在页面文档中。

你<mark>也可以使用addEventListener()方法来监听其他类型的事件</mark>（代码略，详见网页）

**错误处理**

当发生错误(例如请求超时或与HTTP访问控制（CORS）有关的问题), 会生成一个错误事件. 您可以通过在EventSource对象上使用onerror回调来对此采取措施：

```js
evtSource.onerror = function(err) {
  console.error("EventSource failed:", err);
};
```

**关闭事件流**

默认情况下，如果客户端和服务器之间的连接关闭，则连接将重新启动。可以使用.close()方法终止连接。

```js
evtSource.close();
```

摘自：[MDN - 使用服务器发送事件](https://developer.mozilla.org/zh-CN/docs/Web/API/Server-sent_events/Using_server-sent_events)

#### EventSource

<font color=FF0000>**EventSource 是服务器推送的一个网络事件接口**</font>。<font color=FF0000>一个EventSource实例会**对HTTP服务开启一个持久化的连接**，以 **text/event-stream 格式**发送事件, 会一直保持开启直到被要求关闭</font>。

一旦连接开启，来自服务端传入的消息会以事件的形式分发至你代码中。如果接收消息中有一个事件字段，触发的事件与事件字段的值相同。如果没有事件字段存在，则将触发通用事件。

<mark>与 WebSockets不同的是，服务端推送是单向的</mark>。数据信息被单向从服务端到客户端分发. 当不需要以消息形式将数据从客户端发送到服务器时，这使它们成为绝佳的选择。例如，对于处理社交媒体状态更新，新闻提要或将数据传递到客户端存储机制（如IndexedDB或Web存储）之类的，EventSource无疑是一个有效方案。

- **构造函数**
  - <font color=FF0000>**EventSource()：**以指定的 USVString 创建一个新的 EventSource。</font>
- **属性：**此接口从其父接口 EventTarget 继承属性。
  - **EventSource.onerror：**是一个 event handler，当<font color=FF0000>发生错误</font>时被调用，并且在此对象上派发 error 事件。
  - **EventSource.onmessage：**是一个 event handler，当收到一个 <font color=FF0000>message 事件</font>，即消息来自源头时被调用。
  - **EventSource.onopen：**是一个 event handler，当收到一个<font color=FF0000>open 事件</font>，即连接刚打开时被调用。
  - **EventSource.readyState：**只读，一个 unsigned short 值，<font color=FF0000>代表连接状态</font>。<font color=FF0000>可能值是 CONNECTING (0), OPEN (1), 或者 CLOSED (2)</font>。
  - **EventSource.url：**只读，一个DOMString，<font color=FF0000>代表事件源的 URL</font>。
- **事件接收器**
  - EventSource.onerror
  - EventSource.onmessage
  - EventSource.onopen
- **方法：**此接口从其父接口 EventTarget 继承方法。
  - **EventSource.close()：**<font color=FF0000>如果存在，则关闭连接，并且设置 readyState 属性为 CLOSED</font>。<font color=FF0000>如果连接已经被关闭，此方法不会再进行任何操作</font>。
- **事件**
  - error
  - message
  - open

摘自：[MDN - EventSource](https://developer.mozilla.org/zh-CN/docs/Web/API/EventSource)

***

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



## QRCode.js

Github地址：https://github.com/davidshimjs/qrcodejs

**官方示例：**

```html
<div id="qrcode"></div>
<script type="text/javascript">
var qrcode = new QRCode(document.getElementById("qrcode"), {
	text: "http://jindo.dev.naver.com/collie",
	width: 128,
	height: 128,
	colorDark : "#000000",
	colorLight : "#ffffff",
	correctLevel : QRCode.CorrectLevel.H
});
</script>
```

and you can use some methods

```
qrcode.clear(); // clear the code.
qrcode.makeCode("http://naver.com"); // make another code.
```

**参数**

| 参数         | 默认值                | 说明             | 备注                                                         |
| ------------ | --------------------- | ---------------- | ------------------------------------------------------------ |
| text         | string                | 二维码内容字符串 | 如果是url的话，为了微信和QQ可以识别，连接中的中文使用encodeURIComponent进行编码 |
| width        | 256                   | 图像宽度         | 单位像素（百分比不行）                                       |
| height       | 256                   | 图像高度         | 单位像素（百分比不行）                                       |
| colorDark    | '#000'                | 二维码前景色     | 英文\十六进制\rgb\rgba\transparent都可以                     |
| colorLight   | ‘#fff’                | 二维码背景色     | 英文\十六进制\rgb\rgba\transparent都可以                     |
| correctLevel | QRCode.CorrectLevel.L | 容错级别         | <font color=FF0000>**由低到高 .L M Q H**</font>              |

**方法**

- clear：清除QR code
- makeCode(text: String)：重新绘制QR code （仅在不支持 Canvas 的浏览器下有效）

**常见问题**

- 长字符串显示模糊问题
  - **原因：**显示模糊的问题，是canvas的问题。由于字符串比较长，尤其是需要传一个连接地址，后面还加一些参数的时候，就会加大二维码的像素复杂度，而本身canvas在绘制的时候，就一直有像素模糊的问题，尤其是在手机上的时候
  - **解决方法：**先将生成的二维码进行倍数扩大，然后在css上面固定其显示宽高，这样就可以扩大显示像素精度。

- 因为url太长，导致二维码加载报错。一般报错提醒 Error: code length overflow. (1756>1056)
  - 一般都是容错率设置为最高导致的，此时把容错率调低一级便可以

摘自：[前端QRCode.js生成二维码（解决长字符串模块和报错问题）](https://www.cnblogs.com/whkl-m/p/10797776.html)



#### html2canvas

**一些杂乱的经验总结：**

- **用法：**

  ```js
  import html2canvas from 'html2canvas'
  
  html2canvas(element[, options]).then(function(canvas) {
    // ...
    // do something with canvas
  }) 
  ```

  - 其中element是DOM对象，通过`document.querySelector()`等获取元素的方法获取
  - options对应的是设置Configuration / Options，具体可以查看 [html2canvas - document - configuration / options](https://html2canvas.hertzen.com/configuration)

- options中 的 width和height的不需要添加单位，默认为`px`。

- html2canvas的将html转换为canvas是根据 屏幕像素 进行的，如果使用插入图片、且使用`object-fit`、`background-size`的需要注意，如果图片的容器比图片的像素值小，想导出图片、插入的图片会有损。

- Scale的默认值为「**当前屏幕**」的`window.devicePixelRatio`，其中踩到坑的是MacBook Pro的 `window.devicePixelRatio`为2。生成的canvas会比预期大一倍，要将scale设为1

更多参考文章：[高质量前端快照方案：来自页面的「自拍」](https://segmentfault.com/a/1190000021275782)