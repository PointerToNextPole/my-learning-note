# ES6学习笔记



#### 模版字符串

模板字面量 是允许嵌入表达式的字符串字面量。你可以使用多行字符串和字符串插值功能。它们在ES2015规范的先前版本中被称为“模板字符串”。

**描述：**模板字符串使用反引号 (\` \`) 来代替普通字符串中的用双引号和单引号。模板字符串可以包含特定语法（\`${expression}\`）的占位符。占位符中的表达式和周围的文本会一起传递给一个默认函数，该函数负责将所有的部分连接起来，如果一个模板字符串由表达式开头，则该字符串被称为带标签的模板字符串，该表达式通常是一个函数，它会在模板字符串处理后被调用，在输出最终结果前，你都可以通过该函数来对模板字符串进行操作处理。在模版字符串内使用反引号（`）时，需要在它前面加转义符（\）。

摘自：[MDN - 模板字符串](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/template_strings)



#### 展开运算符

- **函数传参**：<font color=FF0000>展开运算符可以展开数组，所以不再需要apply方法，将数组转为函数的参数了</font>。

  示例：

  ```js
  // ES5 的写法
  function f(x, y, z) {
  // ...
  }
  var args = [0, 1, 2];
  f.apply(null, args);
  
  // ES6 的写法
  function f(x, y, z) {
  // ...
  }
  var args = [0, 1, 2];
  f(...args);
  ```

- **数据解构**：其实就是把数组的每个数据拆开然后放进去。

  示例：

  ```js
  let arr = ['autumn', 'wscats'];
  // 析构数组
  let y;
  [autumn, ...y] = arr;
  console.log(y) // 结果为：["wscats"]
  ```

- **数据构造**：

  - 两个对象连接返回新的对象

    ```js
    let x = {  name: 'autumn'}
    let y = {	 age: 18}
    let z = {...x,...y}
    console.log(z)  //结果为：{name: "autumn", age: 18}
    ```

  - 两个数组连接返回新的数组

    ```js
    let x = {name: 'autumn'}
    let y = {age: 18}
    let z = {...x,...y}
    console.log(z)  //结果为 {name: "autumn", age: 18}
    ```

  - 数组加上对象返回新的数组

    ```js
    let x = [{name: 'autumn'}]
    let y = {name: 'wscats'}
    let z = [...x, y];
    console.log(z);  
    /**
     0: {name: "autumn"}
     1: {name: "wscats"}
     length: 2
     __proto__: Array(0)
    */
    ```

  - 数组+字符串

    ```js
    let x = ['autumn'];
    let y = 'wscats';
    let z = [...x, y];
    console.log(z);
    /**
     0: "autumn"
     1: "wscats"
     length: 2
     __proto__: Array(0)
    */
    ```

  - 数组+对象

    ```js
    let x = {
    	name: ['autumn','wscats'],
    	age:18
    }
    let y = {
    	...x,             //name: ['autumn','wscats'],age:18
    	arr: [...x.name]  //['autumn','wscats']
    }
    console.log(y)
    /**
    	age: 18
    	arr: (2) ["autumn", "wscats"]
    	name: (2) ["autumn", "wscats"]
    	__proto__: Object
    */
    ```

摘自：[ES6(...)展开运算符](https://blog.csdn.net/weixin_41615439/article/details/85319618)

