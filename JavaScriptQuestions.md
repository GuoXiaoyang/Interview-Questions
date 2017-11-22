## JavaScript相关问题

#### 请解释事件代理 (event delegation)。

事件代理／委托得益于浏览器的事件冒泡机制，浏览器处理DOM事件分为3个阶段：事件捕获／事件目标／事件冒泡。

在事件冒泡阶段，元素的事件会传递到父元素上，也就是父元素也会接收到该事件。所以使用事件代理，在要监听事件的元素的父元素上绑定相应事件处理器。优点：

* 减少JS的性能消耗
* 方便管理事件函数

---
#### 请解释 JavaScript 中 `this` 是如何工作的。

JS中的 `this` 是在函数执行时来确定的，也就是函数定义的时候并不能确定 `this` 的指向。如果调用者函数，被某一个对象所拥有，那么该函数在调用时，内部的`this`指向该对象。如果函数独立调用，那么该函数内部的this，则指向`undefined`。但是在非严格模式中，当`this`指向`undefined`时，它会被自动指向全局对象<sup><a href="http://www.jianshu.com/p/d647aa6d1ae6">1</a></sup>。 按照上述定义分类：

* 全局范围调用

  在全局上下文执行的代码和全局范围下调用的函数，非严格模式下`this`指向全局对象。

* 方法调用

  函数作为对象的方法调用时，其this指向拥有的那个对象

* 构造器／原型调用

  用new生成对象时，`this`会被绑定到新生成的对象

* `fn.apply`或者`fn.call`调用

  手动确定this的指向，将this绑定到函数的第一个参数（作为上下文）

* bind函数调用

  bind其实就是通过apply来绑定了函数的`this`

* 匿名函数

  this指向全局对象

* 箭头函数

  ES6的箭头函数中没有自己的上下文，`this`是不属于自己的，它会被指向作用域链中最近的一个`this`

---

#### 怎样确保 `setTimeout`中`this`指向正确?



---

#### 请解释原型继承 (prototypal inheritance) 的原理。

JS中面向对象的设计，原型继承是重要的组成部分。

其基本思想<sup>2</sup>是利用原型让一个引用类型继承另一个引用类型的属性和方法。简单回顾一下构造函数、原型和实例的关系：每 个构造函数都有一个原型对象`prototype`，原型对象都包含一个指向构造函数的指针`constructor`，而实例都包含一个指向原型 对象的内部指针`__proto__`。那么，假如我们让原型对象等于另一个类型的实例，结果会怎么样呢？显然，此时的 原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含着一个指向另一个构造函数 的指针。假如另一个原型又是另一个类型的实例，那么上述关系依然成立，如此层层递进，就构成了实 例与原型的链条。这就是所谓原型链的基本概念。

当访问实例对象的属性时，先查找其自身属性，如果没有该属性，则通过`__proto__`查找原型对象上的属性，如果再没有则层层递进查找，由此实现原型继承。

---
#### 你怎么看 AMD vs. CommonJS？

JS中不同的模块加载机制。大概梳理JS模块化的发展吧。

* CommonJS规范

  `require`同步加载模块，`exports`导出模块，该规范最初是适用于服务端的，因为服务端中文件都从磁盘加载，速度影响不大；

* AMD规范

  异步模块定义，`define(id, [depends], callback)`定义模块，`require([module], callback)`加载模块；目前实现主要是`require.js`

  实现浏览器端的异步加载／并行加载多个模块；但不能按需加载，需要提前加载所有的依赖项

* CMD规范

  同样是异步模块规范，实现库尾`sea.js`，通过`define(function(require, exports, module))`可以实现按需加载／浏览器端的异步加载；但模块加载逻辑偏重

* ES6 模块

  目前官方的规范，`import`导入，`export`导出，同步异步都支持

---
#### 请解释为什么接下来这段代码不是 IIFE (立即调用的函数表达式)：`function foo(){ }();`要做哪些改动使它变成 IIFE?

`function foo(){}`只是用`function`声明了一个函数，另一个定义是函数表达式`var foo=function(){}`;

函数声明与函数表达式的不同在于：

* 函数声明在JS创建执行上下文时会进行函数声明提升，也就是即便后边声明的函数也能'提前'调用
* 函数表达式可以直接加括号调用，函数声明不可以

所以`function foo(){}()`不能立即执行函数，但将函数声明变为表达式则可以调用函数，有以下几种办法：

```javascript
(function foo(){}())
(function foo(){})()
!function foo(){}()
+function foo(){}()
-function foo(){}()
var foo = function f(){}()
```



---

#### 描述以下变量的区别：`null`，`undefined` 或 `undeclared`？该如何检测它们？

主要在于意义不同

* `null`  空对象
* `undefined`  声明但没有初始化
* `undeclared` 未声明的变量，通常会引起语法错误

---

#### 什么是闭包 (closure)，如何使用它，为什么要使用它？

我决定简短点概括下。

* 闭包定义

  1. 闭包是在函数执行阶段生成的
  2. 函数中的内部函数访问了外部函数变量
  3. 外部函数称为闭包（有的教程比如MDN和高程中称内部函数是闭包），chrome中闭包是外部函数，个人觉得我们可以不用太纠结闭包是哪个函数，闭包同时包含的自己的执行环境和所访问到的作用域范围，

* 如何使用

  ```javascript
  function add(num1) {
    return function(num2) {
      return num1+num2;
    }
  }
  var add5 = add(5);  
  add5(2);  //7 此处add5函数访问了外部变量5
  ```

* 使用原因

  首先看到《高程》里和网上很多教程说闭包能模拟块级作用域，个人觉得不太严谨，应该是用立即执行函数来模拟块级作用域。

  * 实现私有变量：在构造函数内部非`this`的变量对于外部代码就是私有的，这个也是稳妥构造函数模式的原理
  * 静态私有变量：创建所有实例共享的静态私有变量，就是在立即执行函数中使用内部函数（构造函数的原型方法）来访问外部私有变量，并将该构造函数传递给全局对象。
  * 模块模式：实现单例，在立即执行函数中返回对象，该对象的成员函数能够访问内部私有函数和变量

---
#### 请举出一个匿名函数的典型用例？

立即执行匿名函数模拟块级作用域，上文中提到的。

---
#### 你是如何组织自己的代码？是使用模块模式，还是使用经典继承的方法？

最近使用React偏多，两者都有吧。如果可能的话，尽量跳出框架用原生JS写几个项目。

---
#### 请指出 JavaScript 宿主对象 (host objects) 和原生对象 (native objects) 的区别？

* 原生对象，JavaScript中的基本数据类型／引用数据类型
* 宿主对象，JavaScript执行时环境的对象，比如浏览器端的window／DOM／BOM，服务端的非本地对象。

---
#### 请指出以下代码的区别：`function Person(){}、var person = Person()、var person = new Person()？`

* `function Person(){}、var person = Person()` 普通函数的执行过程，`person`等于函数的返回结果
* `function Person(){}、var person = new Person()` 分两种情况
  * 当`Person()`没有返回值时，`new`会进行创建对象／`this`指向该对象/执行代码赋值／返回对象，也就是说返回一个对象，对象属性值等于函数中的`this`的属性值；
  * 当`Person()`返回一个基础数据类型时，同上
  * 当`Person()`返回一个引用类型对象时，`new`操作符返回的对象会被`return`的对象所覆盖，`person`等于返回值

---
#### `.call` 和 `.apply` 的区别是什么？

参数的区别，`.call`第二个参数开始其后的参数会按照顺序赋给调用函数；

`.apply`只有两个参数，第二个参数是数组，会作为参数列表赋给调用参数

---
#### 请解释 `Function.prototype.bind`？

其实就是函数绑定`this`+函数柯里化；`Function.prototype.bind(context, ...params)`返回一个绑定了`this`上下文和指定参数的函数

---
#### 在什么时候你会使用 `document.write()`？

测试的时候用过。。据说广告会使用这个

---
#### 请指出浏览器特性检测，特性推断和浏览器 UA 字符串嗅探的区别？

* 浏览器特性检测

  检测浏览器是否支持某一特性

* 浏览器特性推断

  根据特性来判断浏览器

* 浏览器UA字符串嗅探

  根据`navigator.userAgent`判断浏览器

---
#### 请尽可能详尽的解释 Ajax 的工作原理。

Asynchronous JavaScript and XML（异步 JavaScript 和 XML）是一种交互式网页开发技术。客户端向服务端发送请求(get/post/delete等)，通过回调接收到服务端返回的响应来进行后续的网页操作。



---
#### 使用 Ajax 都有哪些优劣？

该技术使得客户端与服务端不用传送大量重复的网页资源，并能异步执行，降低带宽消耗和提升用户体验。

坏处：当禁用javaScript后，无法获取想要的数据来刷新页面

---
#### 请解释 JSONP 的工作原理，以及它为什么不是真正的 Ajax。

* JSONP并不是真正的返回数据，而是通过请求url的`callback`返回一个带有数据的函数调用
* JSONP必须要插入一段JS代码在HTML页面`<head>`内，使得返回的函数调用能够执行，并获取所需的数据
* 不受Ajax的跨域限制，但只支持get请求方式

---
#### 你使用过 JavaScript 模板系统吗？

没有，ES6支持模板字符串😊

---
#### 如有使用过，请谈谈你都使用过哪些库？

React全家桶吧 React + Redux + React-Router + recompose + redux-form

还有传统的jQuery，但不算很熟悉

---
#### 请解释变量声明提升 (hoisting)。

JS代码的执行分为两个阶段：建立执行上下文 ； 变量赋值／函数引用／执行代码阶段。

也就是说，变量对象是在执行上下文创建阶段生成的，生成顺序http://www.jianshu.com/p/330b1505e41d：

1. 建立arguments对象。检查当前上下文中的参数，建立该对象下的属性与属性值。
2. 检查当前上下文的函数声明，也就是使用function关键字声明的函数。在变量对象中以函数名建立一个属性，属性值为指向该函数所在内存地址的引用。如果函数名的属性已经存在，那么该属性将会被新的引用所覆盖。
3. 检查当前上下文中的变量声明，每找到一个变量声明，就在变量对象中以变量名建立一个属性，属性值为undefined。如果该变量名的属性已经存在，为了防止同名的函数被修改为undefined，则会直接跳过，原属性值不会被修改。

在未进入执行阶段时，变量对象的属性是不能访问的，只有到了执行阶段，变量对象变为活动对象，可以访问赋值。

变量提升也就容易理解了，由于代码执行先要经过变量对象的创建，那么即便是在后边语句声明的`var`变量和`function`函数，也会在执行之前生成对应的属性和执行之前的赋值。

---
#### 请描述事件冒泡机制 (event bubbling)。

JS中DOM事件处理机制的第三个阶段，子元素上触发的事件会冒泡到父元素上，事件代理的原理就是这个。

---
#### "attribute" 和 "property" 的区别是什么？

attribute是DOM对象上的属性

property是JS对象上的属性，更为通用的概念

---
#### 为什么扩展 JavaScript 内置对象不是好的做法？

可能和某个库或者未来添加的语法发生冲突

---
#### 请指出 document load 和 document DOMContentLoaded 两个事件的区别

* load事件：当页面完全加载后（包括所有图像、JavaScript 文件、 CSS 文件等外部资源），就会触发 window 上面的 load 事件。
* DOMContentLoaded是HTML5事件。  load 事件会可能会因为要 加载的外部资源过多而颇费周折。而 DOMContentLoaded 事件则在形成完整的 DOM 树之后就会触发， 不理会图像、JavaScript 文件、CSS 文件或其他资源是否已经下载完毕。 与 load 事件不同， DOMContentLoaded 支持在页面下载的早期添加事件处理程序，这也就意味着用户能够尽早地与页面 进行交互。

---
#### `==` 和 `===` 有什么不同？

`==` 非严格相等<sup><a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Equality_comparisons_and_sameness">3</a></sup>

* 比较的对象会进行隐式的类型转换，转换成相同类型后再比较，转换规则比较复杂

|        | 被比较值 B    |           |         |                       |                               |                                |                                 |
| ------ | --------- | --------- | ------- | --------------------- | ----------------------------- | ------------------------------ | ------------------------------- |
|        |           | Undefined | Null    | Number                | String                        | Boolean                        | Object                          |
| 被比较值 A | Undefined | `true`    | `true`  | `false`               | `false`                       | `false`                        | `IsFalsy(B)`                    |
|        | Null      | `true`    | `true`  | `false`               | `false`                       | `false`                        | `IsFalsy(B)`                    |
|        | Number    | `false`   | `false` | `A === B`             | `A === ToNumber(B)`           | `A=== ToNumber(B)`             | `A=== ToPrimitive(B) `          |
|        | String    | `false`   | `false` | `ToNumber(A) === B`   | `A === B`                     | `ToNumber(A) === ToNumber(B)`  | `ToPrimitive(B) == A`           |
|        | Boolean   | `false`   | `false` | `ToNumber(A) === B`   | `ToNumber(A) === ToNumber(B)` | `A === B`                      | `ToNumber(A) == ToPrimitive(B)` |
|        | Object    | `false`   | `false` | `ToPrimitive(A) == B` | `ToPrimitive(A) == B`         | `ToPrimitive(A) ==ToNumber(B)` | `A === B`                       |



`===` 严格相等

* 两个被比较的值在比较前都不进行隐式转换。
* 如果两个被比较的值具有不同的类型，这两个值是不等的。
* 如果两个被比较的值类型相同，值也相同，并且都不是 number 类型时，两个值全等。
* 如果两个值都是 number 类型，当两个都不是 NaN，并且数值相同，或是两个值分别为 +0 和 -0 时，两个值被认为是全等的。

一般来说，几乎都是使用严格相等运算符。

---
#### 请解释 JavaScript 的同源策略 (same-origin policy)。

同源策略限制从一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这是一个用于隔离潜在恶意文件的关键的安全机制。策略如下：

* 同样的协议 
* 同样的域名
* 同样的端口

---
#### 如何实现下列代码：`[1,2,3,4,5].duplicator(); // [1,2,3,4,5,1,2,3,4,5]`

```javascript
Array.prototype.duplicator = function() {
  return this.concat(this);
}
```



---

#### 什么是三元表达式 (Ternary expression)？“三元 (Ternary)” 表示什么意思？

```javascript
test ? expression1 : expression2
```

三元表示三个表达式？

---
#### 什么是 `"use strict";` ? 使用它的好处和坏处分别是什么？

ECMAScript 5 最早引入了“严格模式”（strict mode）的概念。通过严格模式，可以在函数内部 选择进行较为严格的全局或局部的错误条件检测。使用严格模式的好处是

* 可以提早知道代码中 存在的错误，及时捕获一些可能导致编程错误的 ECMAScript 行为
* 规范代码，消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为
* \- 消除代码运行的一些不安全之处，保证代码运行的安全
* 提高编译器效率，增加运行速度
* 为未来新版本的Javascript做好铺垫

坏处：某些正常模式下能运行的代码严格模式下不能运行。

严格模式部分规则：

* 不允许给未声明的变量赋值，变量名不能使用保留的关键字
* 操作对象更严格，不会静默失败
* 函数参数必须唯一
* 全局调用的函数的`this`为`undefined`
* 没有`with`语句

---
#### 请实现一个遍历至 100 的 for loop 循环，在能被 3 整除时输出 "fizz"，在能被 5 整除时输出 "buzz"，在能同时被 3 和 5 整除时输出 "fizzbuzz"。

```javascript
for(let i = 0; i <= 100; i += 1) {
  if (i%3 === 0 && i%5 === 0) {
    console.log('fizzbuzz');
  } else if (i%3 === 0) {
    console.log('fizz');
  } else if (i%5 === 0) {
    console.log('buzz');
  }
}
```



---
#### 为何通常会认为保留网站现有的全局作用域 (global scope) 不去改变它，是较好的选择？

由于全局作用域能被所有的作用域访问，如果改变，可能会造成冲突

---
#### 为何你会使用 load 之类的事件 (event)？此事件有缺点吗？你是否知道其他替代品，以及为何使用它们？

load与DOMContentLoaded的比较

---
#### 请解释什么是单页应用 (single page app), 以及如何使其对搜索引擎友好 (SEO-friendly)。

SPA就是使用单个页面开发的应用，网页的交互通过JavaScript实现，比如DOM的刷新，Ajax获取后端数据。

至于做到SEO友好，可以针对爬虫服务端渲染生成一套静态页面。

一套代码，两处运行。

---
#### 你使用过 Promises 及其 polyfills 吗? 请写出 Promise 的基本用法（ES6）。

JavaScript已经原生支持Promise。Promise定义：

```javascript
var promise = new Promise(function(resolve, reject) {
  // do a thing, possibly async, then…
  if (/* everything turned out fine */) {
    resolve("Stuff worked!");
  }
  else {
    reject(Error("It broke"));
  }
});
```

Promise使用

```javascript
promise.then(function(result) {
  console.log(result); // "Stuff worked!"
}, function(err) {
  console.log(err); // Error: "It broke"
});
```



---
#### 使用 Promises 而非回调 (callbacks) 优缺点是什么？

优点

* 代码可读性强

* 避免在多重回调时陷入回调阱

  Promise 可以很好地处理单一异步结果，不适用于<sup><a href="https://github.com/es6-org/exploring-es6/blob/master/md/24.10.md">4</a></sup>（该部分我也暂时没有懂）：

* 多次触发的事件：如果要处理这种情况，可以了解一下响应式编程（ reactive programming ），这是一种很聪明的链式的处理普通事件的方法。

* 数据流：支持此种情形的[标准](https://streams.spec.whatwg.org/)正在制定中。

ECMAScript 6 Promise 缺少两个有时很有用的特性：

- 不能取消执行。
- 无法获取当前执行的进度信息（比如，要在用户界面展示进度条）。

---
#### 使用一种可以编译成 JavaScript 的语言来写 JavaScript 代码有哪些优缺点？

比如TypeScript／CoffeScript，我没有使用过CoffeeScript，印象中是能够提前实现ES6的语法或一些其他的语法糖。

就说TypeScript，作为静态类型的JavaScript，可以在大型页面中，保证代码的可靠性、可读性和可维护性；缺点就是需要大量的函数类型定义，对于小型页面，负担较大，所以个人项目暂时放弃了😢

---
#### 你使用哪些工具和技术来调试 JavaScript 代码？

VS Code的调试工具 + Chrome DevTools

技术当然就是 打断点／Debugger／Console.log()等

---
#### 你会使用怎样的语言结构来遍历对象属性 (object properties) 和数组内容？

很多种方法<sup><a href="http://2ality.com/2011/04/iterating-over-arrays-and-objects-in.html">5</a></sup>。

* 最直接的，`for`循环，适合数组遍历

  ```javascript
  for ([start]; [condition]; [final-expression])
    statement
  ```

* `for in` 不建议对数组使用，因为会遍历索引属性、普通属性和原型属性

  ```javascript
   > var arr = [ "a", "b", "c" ];
   > arr.foo = true;
   > for(var key in arr) { console.log(key); }
   0
   1
   2
   foo
  ```

- `for of`对迭代器对象进行遍历，得到的是值而不是属性


  ```javascript
  for (var value of iterable) {
    statement
  }
  ```

- 数组方法

  ```javascript
  Array.prototype.forEach(callback)
  Array.prototype.map(callback)
  ```


- Object方法

  ```javascript
  Object.keys()
  Object.entries()
  ```
  ​


---
#### 请解释可变 (mutable) 和不变 (immutable) 对象的区别。

由于JS中JS对象是引用型数据，变量的修改都是在原对象的内存中存储数据的修改，这样可以降低内存消耗。

但是这样会带来不可控，一旦某个地方修改了数据，其他使用这个变量的代码都会受到影响。

所以出现了不变对象，JS原生没有这个概念，都是第三方库实现的。**每次修改一个 Immutable 对象时都会创建一个新的不可变的对象**，在新对象上操作并不会影响到原对象的数据。

总结下，不可变对象就是一旦改变了某个变量数据，必须返回一个新的引用地址，与原变量隔离。

---
#### 请举出 JavaScript 中一个不变性对象 (immutable object) 的例子？

Redux中状态就是不可变对象，还有React中函数组件的更新都是基于传入属性的浅比较；都需要不可变对象实现。

---
#### 不变性 (immutability) 有哪些优缺点？

优点：修改不可变对象不会对现有程序造成影响

缺点：修改对象会开辟新的内存，内存消耗比较大

---
#### 如何用你自己的代码来实现不变性 (immutability)？

用原对象生成新的对象

```javascript
let obj2 = {...obj1, {property}};
let obj2 = Object.assign(obj1, {property}); //ES6中对象的操作
//数组的深拷贝
let arr2 = arr1.slice();
let arr2 = arr1.concat();
```

---
#### 请解释同步 (synchronous) 和异步 (asynchronous) 函数的区别。

* 同步就是执行完毕后继续执行后面语句
* 异步就是当前任务不阻塞主线程，主线程继续运行，直到该任务返回结果后通知调用者执行回调函数

---
#### 什么是事件循环 (event loop)？请问调用栈 (call stack) 和任务队列 (task queue) 的区别是什么？

事件循环机制就是JS执行代码和处理异步事件的机制。

JS在创建执行上下文时，会以栈的形式将执行代码压入调用栈，遇到函数会压入栈，直到运行完毕出栈继续执行上层函数，称为调用栈；

由于异步任务的存在，会生成任务队列，JS遇到异步任务时，会将任务交给其他模块处理，处理完后将回调放在任务队列里；主线程调用栈回到全局环境后会按顺序执行任务队列中的代码

---
#### 解释 `function foo(){}` 与 `var foo = function() {}` 用法的区别

* ` function foo(){}` 是函数声明，JS创建执行上下文环境时，会对函数声明进行提升，即可"先调用后声明"
*  `var foo = function() {}` 是函数表达式，其后可以直接加括号运行，但没有函数提升

---

#### get与post请求的区别



---







[1] http://www.jianshu.com/p/d647aa6d1ae6

[2] *JavaScript高级程序设计*

[3] https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Equality_comparisons_and_sameness

[4] https://github.com/es6-org/exploring-es6/blob/master/md/24.10.md

[5] http://2ality.com/2011/04/iterating-over-arrays-and-objects-in.html