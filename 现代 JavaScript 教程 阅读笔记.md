# 现代 JavaScript 教程 阅读笔记



#### 数据类型

NaN 是粘性的。任何对 NaN 的进一步操作都会返回 NaN。

**补充：**NaN的全部要点是它们是“粘性的”（也许Monadic是一个更好的术语？），一旦我们在表达式中包含NaN，整个表达式的值就会变为NaN。
摘自：https://www.coder.work/article/6753344



#### 基础运算符，数学

一元运算符 +，可以用作“数字转化”。它的效果和 Number() 相同，但是更加简短。



#### 对 null 和 undefined 进行比较

当使用数学式或其他比较方法 <、 >、 <=、 >= 时：null/undefined 会被转化为数字：null 被转化为 0，undefined 被转化为 NaN。



#### 奇怪的结果：null vs 0

通过比较 null 和 0 可得：

```js
alert( null > 0 );  // (1) false
alert( null == 0 ); // (2) false
alert( null >= 0 ); // (3) true
```

是的，上面的结果完全打破了你对数学的认识。在最后一行代码显示“null 大于等于 0”的情况下，前两行代码中一定会有一个是正确的，然而事实表明它们的结果都是 false。

<font color=FF0000>为什么会出现这种反常结果，这是因为相等性检查 == 和普通比较符 >、 <、 >=、 <= 的代码逻辑是相互独立的</font>。进行值的比较（>、>=、<、<=）时，null 会被转化为数字，因此它被转化为了 0。这就是为什么（3）中 null >= 0 返回值是 true，（1）中 null > 0 返回值是 false。

<font color=FF0000>另一方面，undefined 和 null 在相等性检查 == 中不会进行任何的类型转换，它们有自己独立的比较规则，所以除了它们之间互等外，不会等于任何其他的值</font>。这就解释了为什么（2）中 null == 0 会返回 false。



#### 短路求值（Short-circuit evaluation）。

或运算符 || 的另一个用途是所谓的“短路求值”。这指的是，|| 对其参数进行处理，<font color=FF0000>直到达到第一个真值</font>，然后<font color=FF0000 size='5'>**立即返回该值**</font>，而无需处理其他参数。



与运算 && 的优先级比或运算 || 要高。

两个非运算 !! 有时候用来将某个值转化为布尔类型

非运算符 ! 的优先级在所有逻辑运算符里面最高，所以它总是在 && 和 || 之前执行。



#### 空值合并运算符  ??

空值合并运算符（nullish coalescing operator）的写法为两个问号 ??。

`a ?? b` 的结果是：

- 如果 a 是已定义的，则结果为 a，
- 如果 a 不是已定义的，则结果为 b。

人们对 || 不太满意。因为|| 无法区分 false、0、空字符串 "" 和 null/undefined。它们都一样 —— 假值（falsy values）。所以空值合并运算符 ?? 才在最近被添加到 JavaScript 中。

**?? 和 || 之间重要的区别是：**

- || 返回第一个 真 值。
- ?? 返回第一个 已定义的 值。

?? 运算符的优先级相当低：在 MDN table 中为 5；仅略高于 ? 和 =。



#### ?? 与 && 或 || 一起使用

出于安全原因，JavaScript 禁止将 ?? 运算符与 && 和 || 运算符一起使用，除非使用括号明确指定了优先级。<font color=FF0000>否则，会出现语法错误</font>



#### 后备的默认参数

有些时候，将参数默认值的设置放在函数执行（相较更后期）而不是函数声明的时候，也能行得通。

为了判断参数是否被省略掉，我们可以拿它跟 undefined 做比较：

```js
function showMessage(text) {
  if (text === undefined) {
    text = 'empty message';
  }
  //TODO
}

function showMessage(text) {
  text = text || 'empty'; // 使用text = text ?? 'empty'更好
  ...
}
```



#### 空值的 return 或没有 return 的函数返回值为 undefined

如果函数无返回值，它就会像返回 undefined 一样：

```js
function doNothing() { /* 没有代码 */ }

alert( doNothing() === undefined ); // true
```



属性的值可以是任意类型，让我们加个布尔类型：

```js
user.isAdmin = true;
```

我们可以用 delete 操作符移除属性：

```js
delete user.age;
```

我们也可以用多字词语来作为属性名，但必须给它们加上引号：

```js
let user = {
  name: "John",
  age: 30,
  "likes birds": true  // 多词属性名必须加引号
};
```



#### 深拷贝函数 Object.assign()

**语法：**

```js
Object.assign(dest, [src1, src2, src3...])
```

- 第一个参数 dest 是指目标对象。
- 更后面的参数 src1, ..., srcN（可按需传递多个参数）是源对象。
- 该方法将所有源对象的属性拷贝到目标对象 dest 中。换句话说，从第二个开始的所有参数的属性都被拷贝到第一个参数的对象中。
- 调用结果返回 dest。
- 如果被拷贝的属性的属性名已经存在，那么它会被覆盖


<font color=FF0000>不过要注意的是，该深拷贝的范围只有一层；也就是说：如果被拷贝的对象是对象中包含对象，则第二层级的对象将无法被顺利神拷贝</font>。如果真的要实现：我们可以用递归来实现。<font color=FF0000>或者不自己造轮子，使用现成的实现，例如 JavaScript 库 lodash 中的 _.cloneDeep(obj)</font>。



#### 任务队列 Event Loop

更详细的事件循环算法（尽管与 [规范](https://html.spec.whatwg.org/multipage/webappapis.html#event-loop-processing-model) 相比仍然是简化过的）：

1. 从 **宏任务** 队列（例如 “script”）中出队（dequeue）并执行最早的任务。
2. 执行所有微任务：
   - 当微任务队列非空时：
     - 出队（dequeue）并执行最早的微任务。
3. 执行渲染，如果有。
4. 如果宏任务队列为空，则休眠直到出现宏任务。
5. 转到步骤 1。



#### TypeArray

还有另外两个术语，用于对二进制数据进行操作的方法的描述：

- ArrayBufferView 是所有这些视图的总称。
- BufferSource 是 ArrayBuffer 或 ArrayBufferView 的总称。



#### 前端下载文件示例（场景：根据前端页面，截图以下载图片）

通过点击链接，你可以下载一个具有动态生成的内容为 hello world 的 Blob 的文件：

```html
<!-- download 特性（attribute）强制浏览器下载而不是导航 -->
<a download="hello.txt" href='#' id="link">Download</a>

<script>
let blob = new Blob(["Hello, world!"], {type: 'text/plain'});

link.href = URL.createObjectURL(blob);
</script>
```

下面是类似的代码，此代码可以让用户无需任何 HTML 即可下载动态生成的 Blob（译注：也就是通过代码模拟用户点击，从而自动下载）：

```js
let link = document.createElement('a');
link.download = 'hello.txt';

let blob = new Blob(['Hello, world!'], {type: 'text/plain'});

link.href = URL.createObjectURL(blob);

link.click();

URL.revokeObjectURL(link.href);
```

下面是下载 Blob 的示例，这次是通过 base-64：

```js
let link = document.createElement('a');
link.download = 'hello.txt';

let blob = new Blob(['Hello, world!'], {type: 'text/plain'});

let reader = new FileReader();
reader.readAsDataURL(blob); // 将 Blob 转换为 base64 并调用 onload

reader.onload = function() {
  link.href = reader.result; // data url
  link.click();
};
```



#### 简单请求和非简单请求的区别

简单请求和其他请求的本质区别在于，自古以来使用 \<form> 或 \<script> 标签进行简单请求就是可行的，而长期以来浏览器都不能进行非简单请求。

实际区别在于，<font color=FF0000>**简单请求会使用 Origin header 并立即发送**</font>，而对于<font color=FF0000>**其他请求，浏览器会发出初步的“预检”请求，以请求许可**</font>。并且，<font color=FF0000 size=4>除非服务器明确通过 header 进行确认，否则非简单请求不会被发送</font>。



##### 对于简单请求

- → 浏览器发送带有源的 Origin header。

- ← 对于没有凭据的请求（默认不发送），服务器应该设置：
  - Access-Control-Allow-Origin 为 * 或与 Origin 的值相同
  
    > 服务器可以检查 `Origin`，<font color=FF0000>如果同意接受这样的请求，就会在响应中添加一个特殊的 header `Access-Control-Allow-Origin`</font>。该 header ( Access-Control-Allow-Origin ) 包含了允许的源（在我们的示例中是 `https://javascript.info`），或者一个星号 `*`。然后响应成功，否则报错。
    >
    > <font color=FF0000>**浏览器在这里扮演受被信任的中间人的角色**</font>：
    >
    > 1. 它确保发送的跨源请求带有正确的 `Origin`。
    > 2. 它检查响应中的许可 `Access-Control-Allow-Origin`，如果存在，则允许 JavaScript 访问响应，否则将失败并报错。
    >
    > ![](https://zh.javascript.info/article/fetch-crossorigin/xhr-another-domain.svg)
    >
    > 摘自：[现代JS教程 - Fetch：跨源请求 - 用于简单请求的 CORS](https://zh.javascript.info/fetch-crossorigin#yong-yu-jian-dan-qing-qiu-de-cors) 
  
- ← 对于具有凭据的请求，服务器应该设置：
  - Access-Control-Allow-Origin 值与 Origin 的相同
  - Access-Control-Allow-Credentials 为 true

此外，<font color=FF0000>要授予 JavaScript 访问 除 Cache-Control，Content-Language，Content-Type，Expires，Last-Modified 或 Pragma 外的任何 response header 的权限</font>（注：即，这些响应头（Cache-Control 等）是默认可以被 用户代理 读取的），<font color=FF0000>服务器应该在 header `Access-Control-Expose-Headers` 中列出允许的哪些 header</font>，通过逗号分隔（**注：**在文章 [Vuejs之axios获取Http响应头](https://segmentfault.com/a/1190000009125333) 中有类似说法 ）



##### 对于 <font color=FF0000>非简单请求，会 <font size=4>在请求之前发出初步“预检”请求</font></font>

- → 浏览器将具有以下 header 的 OPTIONS 请求发送到相同的 URL：
  - Access-Control-Request-Method 有请求方法。
  - Access-Control-Request-Headers 以逗号分隔的“非简单” header 列表。

  **注：感觉有点过于概括，非总结的言语如下：**

  > <font color=FF0000>**预检请求使用 OPTIONS 方法**</font>，它<font color=FF0000>**没有 body**</font>，但是<font color=FF0000>**有两个 header**</font>：
  >
  > - **Access-Control-Request-Method** header <font color=FF0000>带有非简单请求的方法</font>。
  > - **Access-Control-Request-Headers** header <font color=FF0000>提供一个以逗号分隔的 **非简单 HTTP-header 列表**</font>。
  >
  > 摘自：[现代JS教程 - Fetch：跨源请求 - “非简单”请求](https://zh.javascript.info/fetch-crossorigin#fei-jian-dan-qing-qiu)

- ← 服务器应该响应状态码为 200 和 header：
  - Access-Control-Allow-Methods 带有允许的方法的列表，
  - Access-Control-Allow-Headers 带有允许的 header 的列表，
  - Access-Control-Max-Age 带有指定缓存权限的秒数。

  **注：感觉有点过于概括，非总结的言语如下：**

  > <font color=FF0000>如果服务器同意处理请求，那么它会进行响应</font>，此响应的状态码应该为 200，<font color=FF0000>没有 body，具有 header</font>：
  >
  > - Access-Control-Allow-Origin 必须为 `*` <font color=FF0000>或</font> 进行请求的源（ 例如 `https://javascript.info` ）才能允许此请求。
  > - Access-Control-Allow-Methods <font color=FF0000>必须具有允许的方法</font>。
  > - Access-Control-Allow-Headers <font color=FF0000>必须具有一个允许的 header 列表</font>。
  > - 另外，header <font color=FF0000>**Access-Control-Max-Age** 可以指定缓存此权限的秒数</font>（**注：**这里即表达：在Access-Control-Max-Age 之内的时间内，不必在进行预检 ）。因此，浏览器不是必须为满足给定权限的后续请求发送预检。
  >
  > ![](https://zh.javascript.info/article/fetch-crossorigin/xhr-preflight.svg)
  >
  > 摘自：[现代JS教程 - Fetch：跨源请求 - “非简单”请求](https://zh.javascript.info/fetch-crossorigin#fei-jian-dan-qing-qiu)

- 然后，发出实际请求，应用先前的“简单”方案。



#### 非简单请求过程示例

- **Step 1 预检请求（preflight request）**

  在发送我们的请求前，浏览器会自己发送如下所示的预检请求：

  ```http
  OPTIONS /service.json
  Host: site.com
  Origin: https://javascript.info
  Access-Control-Request-Method: PATCH
  Access-Control-Request-Headers: Content-Type,API-Key
  ```

  - **方法：**OPTIONS。

  - **路径：**与主请求完全相同：`/service.json`。

  - **特殊跨源头：**
    - **Origin：**来源。
    - **Access-Control-Request-Method：**请求方法。
    - **Access-Control-Request-Headers：**以逗号分隔的“非简单” header 列表

- **Step 2 预检响应（preflight response）**

  服务应响应状态 200 和 header：

  - `Access-Control-Allow-Origin: https://javascript.info`
  - `Access-Control-Allow-Methods: PATCH`
  - `Access-Control-Allow-Headers: Content-Type,API-Key`。

  这将允许后续通信，否则会触发错误。

  如果服务器将来期望其他方法和 header，则可以通过将这些方法和 header 添加到列表中来预先允许它们。

  例如，此响应还允许 `PUT`、`DELETE` 以及其他 header：

  ```http
  200 OK
  Access-Control-Allow-Origin: https://javascript.info
  Access-Control-Allow-Methods: PUT,PATCH,DELETE
  Access-Control-Allow-Headers: API-Key,Content-Type,If-Modified-Since,Cache-Control
  Access-Control-Max-Age: 86400
  ```

  现在，浏览器可以看到 `PATCH` 在 `Access-Control-Allow-Methods` 中，`Content-Type,API-Key` 在列表 `Access-Control-Allow-Headers` 中，因此它将发送主请求。

  如果 `Access-Control-Max-Age` 带有一个表示秒的数字，则在给定的时间内，预检权限会被缓存。<font color=FF0000>上面的响应将被缓存 86400 秒，也就是一天。在此时间范围内，后续请求将不会触发预检。假设它们符合缓存的配额，则将直接发送它们</font>。

- **Step 3 实际请求（actual request）**

  预检成功后，浏览器现在发出主请求。这里的算法与简单请求的算法相同。

  主请求具有 `Origin` header（因为它是跨源的）：

  ```http
  PATCH /service.json
  Host: site.com
  Content-Type: application/json
  API-Key: secret
  Origin: https://javascript.info
  ```

- **Step 4 实际响应（actual response）**

  服务器不应该忘记在主响应中添加 Access-Control-Allow-Origin。成功的预检并不能免除此要求：

  ```http
  Access-Control-Allow-Origin: https://javascript.info
  ```

然后，JavaScript 可以读取主服务器响应了。



#### referrerPolicy选项

**referrerPolicy 选项为 Referer 设置一般的规则**

请求分为 3 种类型：

1. 同源请求。
2. 跨源请求。
3. 从 HTTPS 到 HTTP 的请求 (从安全协议到不安全协议)。

与 referrer 选项允许设置确切的 Referer 值不同，referrerPolicy 告诉浏览器针对各个请求类型的一般的规则。

可能的值在 Referrer Policy 规范中有详细描述：

- "no-referrer-when-downgrade" —— 默认值：除非我们从 HTTPS 发送请求到 HTTP（到安全性较低的协议），否则始终会发送完整的 Referer。
- "no-referrer" —— 从不发送 Referer。
- "origin" —— 只发送在 Referer 中的域，而不是完整的页面 URL，例如，只发送 `http://site.com` 而不是 `http://site.com/path`。
- "origin-when-cross-origin" —— 发送完整的 Referer 到相同的源，但对于跨源请求，只发送域部分（同上）。
- "same-origin" —— 发送完整的 Referer 到相同的源，但对于跨源请求，不发送 Referer。
- "strict-origin" —— 只发送域，对于 HTTPS→HTTP 请求，则不发送中则不发送 Referer。
- "strict-origin-when-cross-origin" —— 对于同源情况下则发送完整的 Referer，对于跨源情况下，则只发送域，如果是 HTTPS→HTTP 请求，则什么都不发送。
- "unsafe-url" —— 在 Referer 中始终发送完整的 url，即使是 HTTPS→HTTP 请求。

这是一个包含所有组合的表格：

| 值                                         | 同源       | 跨源       | HTTPS→HTTP |
| :----------------------------------------- | :--------- | :--------- | :--------- |
| "no-referrer"                              | -          | -          | -          |
| "no-referrer-when-downgrade" 或 ""（默认） | 完整的 url | 完整的 url | -          |
| "origin"                                   | 仅域       | 仅域       | 仅域       |
| "origin-when-cross-origin"                 | 完整的 url | 仅域       | 仅域       |
| "same-origin"                              | 完整的 url | -          | -          |
| "strict-origin"                            | 仅域       | 仅域       | -          |
| "strict-origin-when-cross-origin"          | 完整的 url | 仅域       | -          |
| "unsafe-url"                               | 完整的 url | 完整的 url | 完整的 url |



#### XHR的上传进度

这里有另一个对象，它没有方法，它专门用于跟踪上传事件：xhr.upload。

它会生成事件，类似于 xhr，但是 xhr.upload 仅在上传时触发它们：

- loadstart —— 上传开始。
- progress —— 上传期间定期触发。
- abort —— 上传中止。
- error —— 非 HTTP 错误。
- load —— 上传成功完成。
- timeout —— 上传超时（如果设置了 timeout 属性）。
- loadend —— 上传完成，无论成功还是 error。
