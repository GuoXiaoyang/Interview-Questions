## JavaScript相关问题

#### JavaScript有几种类型的值？你能画一下他们的内存图吗？

 栈：原始数据类型（Undefined，Null，Boolean，Number、String）
 堆：引用数据类型（对象、数组和函数）

 两种类型的区别是：存储位置不同；
 原始数据类型直接存储在栈(stack)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；
 引用数据类型存储在堆(heap)中的对象,占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体

---

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

#### 原型链

当从一个对象那里调取属性或方法时，如果该对象自身不存在这样的属性或方法，就会去自己关联的`prototype`对象那里寻找，如果prototype没有，就会去prototype关联的前辈prototype那里寻找，如果再没有则继续查找`Prototype.Prototype`引用的对象，依次类推，直到Prototype.….Prototype为undefined（Object的Prototype就是undefined）从而形成了所谓的“原型链”。

其中foo是Function对象的实例。而Function的原型对象同时又是Object的实例。这样就构成了一条原型链。

#### instanceof 确定原型和实例之间的关系

用来判断某个构造函数的prototype属性是否存在另外一个要检测对象的原型链上

对象的`__proto__`指向自己构造函数的prototype。`obj.__proto__.__proto__...`的原型链由此产生，包括我们的操作符instanceof正是通过探测`obj.__proto__.__proto__... === Constructor.prototype`来验证obj是否是Constructor的实例。

```javascript
function C(){}

var o = new C(){}
//true 因为Object.getPrototypeOf(o) === C.prototype
o instanceof C
```

instanceof只能用来判断对象和函数，不能用来判断字符串和数字

#### isPrototypeOf

用于测试一个对象是否存在于另一个对象的原型链上。

判断父级对象 可检查整个原型链

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
#### 解释 “JavaScript 模块模式” 以及你在何时使用它。

- 如果有提到无污染的命名空间，可以考虑加分。
- 如果你的模块没有自己的命名空间会怎么样？

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

1. 创建XMLHttpRequest对象,也就是创建一个异步调用对象.
2. 创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息.
3. 设置响应HTTP请求状态变化的函数.
4. 发送HTTP请求.
5. 获取异步调用返回的数据.
6. 使用JavaScript和DOM实现局部刷新.



#### ajax请求和原理

```
var xhr = new XMLHTTPRequest();
// 请求 method 和 URI
xhr.open('GET', url);
// 请求内容
xhr.send();
// 响应状态
xhr.status
// xhr 对象的事件响应
xhr.onreadystatechange = function() {}
xhr.readyState
// 响应内容
xhr.responseText
```

- AJAX的工作原理

Ajax的工作原理相当于在用户和服务器之间加了—个中间层(AJAX引擎),使用户操作与服务器响应异步化。　Ajax的原理简单来说通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用javascript来操作DOM而更新页面。

- ajax优缺点

优点：无刷新更新数据 异步与服务器通信 前后端负载均衡

缺点：

1）ajax干掉了Back和history功能，对浏览器机制的破坏 2）对搜索引擎支持较弱 3）违背了URI和资源定位的初衷

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
* `var foo = function() {}` 是函数表达式，其后可以直接加括号运行，但没有函数提升

---

#### get与post请求的区别

GET：一般用于信息获取，使用URL传递参数，对所发送信息的数量也有限制，一般在2000个字符
POST：一般用于修改服务器上的资源，对所发送的信息没有限制。

GET方式需要使用Request.QueryString来取得变量的值，而POST方式通过Request.Form来获取变量的值，
也就是说Get是通过地址栏来传值，而Post是通过提交表单来传值。

然而，在以下情况中，请使用 POST 请求：
无法使用缓存文件（更新服务器上的文件或数据库）
向服务器发送大量数据（POST 没有数据量限制）
发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

---

#### `["1", "2", "3"].map(parseInt)` 答案是多少？

 parseInt() 函数能解析一个字符串，并返回一个整数，需要两个参数 (val, radix)，
 其中 radix 表示要解析的数字的基数。[该值介于 2 ~ 36 之间，并且字符串中的数字不能大于radix才能正确返回数字结果值];
 但此处 map 传了 3 个 (element, index, array),我们重写parseInt函数测试一下是否符合上面的规则。

```javascript
function parseInt(str, radix) {
  return str+'-'+radix;
};
var a=["1", "2", "3"];
a.map(parseInt);  // ["1-0", "2-1", "3-2"] 不能大于radix
```

 因为二进制里面，没有数字3,导致出现超范围的radix赋值和不合法的进制解析，才会返回NaN
 所以`["1", "2", "3"].map(parseInt)` 答案也就是：[1, NaN, NaN]

---

#### Javascript中，有一个函数，执行时对象查找时，永远不会去查找原型，这个函数是？


 `hasOwnProperty`

JavaScript中hasOwnProperty函数方法是返回一个布尔值，指出一个对象是否具有指定名称的属性。此方法无法检查该对象的原型链中是否具有该属性；该属性必须是对象本身的一个成员。
使用方法：
`object.hasOwnProperty(proName)`
其中参数object是必选项。一个对象的实例。
proName是必选项。一个属性名称的字符串值。

如果 object 具有指定名称的属性，那么JavaScript中`hasOwnProperty`函数方法返回 true，反之则返回 false。

---

#### JSON 的了解？


 JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。
 它是基于JavaScript的一个子集。数据格式简单, 易于读写, 占用带宽小
 如：{"age":"12", "name":"back"}

 JSON字符串转换为JSON对象:
 `var obj = str.parseJSON();`
 `var obj = JSON.parse(str);`

 JSON对象转换为JSON字符串：
 `var last=obj.toJSONString();`
` var last=JSON.stringify(obj);`

---

`[].forEach.call($$("*"),function(a){a.style.outline="1px solid #"+(~~(Math.random()*(1<<24))).toString(16)})` 能解释一下这段代码的意思吗？



---

#### 数组和对象有哪些原生方法，列举一下？



---

#### 如何编写高性能的Javascript？哪些操作会造成内存泄漏？



内存泄漏指任何对象在您不再拥有或需要它之后仍然存在。
 垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为 0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收。

 setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。
 闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）

---

#### 需求：实现一个页面操作不会整页刷新的网站，并且能在浏览器前进、后退时正确响应。给出你的技术实现方案？

---

#### 如何判断当前脚本运行在浏览器还是node环境中？（阿里）

```
this === window ? 'browser' : 'node';
通过判断Global对象是否为window，如果不为window，当前脚本没有运行在浏览器中
```

---

new操作符具体干了什么呢?

   1、创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。
   2、属性和方法被加入到 this 引用的对象中。
   3、新创建的对象由 this 所引用，并且最后隐式的返回 this 。

```javascript
var obj  = {};
obj.proto = Base.prototype;
Base.call(obj); 
```

---

#### DOM元素e的e.getAttribute(propName)和e.propName有什么区别和联系

- e.getAttribute()，是标准DOM操作文档元素属性的方法，具有通用性可在任意文档上使用，返回元素在源文件中**设置的属性**
- e.propName通常是在HTML文档中访问特定元素的**特性**，浏览器解析元素后生成对应对象（如a标签生成HTMLAnchorElement），这些对象的特性会根据特定规则结合属性设置得到，对于没有对应特性的属性，只能使用getAttribute进行访问
- e.getAttribute()返回值是源文件中设置的值，类型是字符串或者null（有的实现返回""）
- e.propName返回值可能是字符串、布尔值、对象、undefined等
- 大部分attribute与property是一一对应关系，修改其中一个会影响另一个，如id，title等属性
- 一些布尔属性`<input hidden/>`的检测设置需要hasAttribute和removeAttribute来完成，或者设置对应property
- 像`<a href="../index.html">link</a>`中href属性，转换成property的时候需要通过转换得到完整URL
- 一些attribute和property不是一一对应如：form控件中`<input value="hello"/>`对应的是defaultValue，修改或设置value property修改的是控件当前值，setAttribute修改value属性不会改变value property

---

#### offsetWidth/offsetHeight,clientWidth/clientHeight与scrollWidth/scrollHeight的区别

- offsetWidth/offsetHeight返回值包含**content + padding + border**，效果与e.getBoundingClientRect()相同
- clientWidth/clientHeight返回值只包含**content + padding**，如果有滚动条，也**不包含滚动条**
- scrollWidth/scrollHeight返回值包含**content + padding + 溢出内容的尺寸**

[Measuring Element Dimension and Location with CSSOM in Windows Internet Explorer 9](http://msdn.microsoft.com/en-us/library/ie/hh781509(v=vs.85).aspx)

[![元素尺寸](https://github.com/qiu-deqing/FE-interview/raw/master/img/element-size.png)](https://github.com/qiu-deqing/FE-interview/blob/master/img/element-size.png)

---

#### XMLHttpRequest通用属性和方法

1. `readyState`:表示请求状态的整数，取值：

- UNSENT（0）：对象已创建
- OPENED（1）：open()成功调用，在这个状态下，可以为xhr设置请求头，或者使用send()发送请求
- HEADERS_RECEIVED(2)：所有重定向已经自动完成访问，并且最终响应的HTTP头已经收到
- LOADING(3)：响应体正在接收
- DONE(4)：数据传输完成或者传输产生错误

1. `onreadystatechange`：readyState改变时调用的函数
2. `status`：服务器返回的HTTP状态码（如，200， 404）
3. `statusText`:服务器返回的HTTP状态信息（如，OK，No Content）
4. `responseText`:作为字符串形式的来自服务器的完整响应
5. `responseXML`: Document对象，表示服务器的响应解析成的XML文档
6. `abort()`:取消异步HTTP请求
7. `getAllResponseHeaders()`: 返回一个字符串，包含响应中服务器发送的全部HTTP报头。每个报头都是一个用冒号分隔开的名/值对，并且使用一个回车/换行来分隔报头行
8. `getResponseHeader(headerName)`:返回headName对应的报头值
9. `open(method, url, asynchronous [, user, password])`:初始化准备发送到服务器上的请求。method是HTTP方法，不区分大小写；url是请求发送的相对或绝对URL；asynchronous表示请求是否异步；user和password提供身份验证
10. `setRequestHeader(name, value)`:设置HTTP报头
11. `send(body)`:对服务器请求进行初始化。参数body包含请求的主体部分，对于POST请求为键值对字符串；对于GET请求，为null



#### fetch和Ajax有什么不同

`XMLHttpRequest` 是一个设计粗糙的 API，不符合关注分离（Separation of Concerns）的原则，配置和调用方式非常混乱，而且基于事件的异步模型写起来也没有现代的 Promise，`generator/yield`，`async/await` 友好。

fetch 是浏览器提供的一个新的 web API，它用来代替 Ajax（XMLHttpRequest），其提供了更优雅的接口，更灵活强大的功能。 Fetch 优点主要有：

- 语法简洁，更加语义化
- 基于标准 Promise 实现，支持 `async/await`

```javascript
fetch(url).then(response => response.json())
  .then(data => console.log(data))
  .catch(e => console.log("Oops, error", e))
```



---

#### focus/blur与focusin/focusout的区别与联系

1. focus/blur不冒泡，focusin/focusout冒泡
2. focus/blur兼容性好，focusin/focusout在除FireFox外的浏览器下都保持良好兼容性，如需使用事件托管，可考虑在FireFox下使用事件捕获elem.addEventListener('focus', handler, true)
3. 可获得焦点的元素：
   1. window
   2. 链接被点击或键盘操作
   3. 表单空间被点击或键盘操作
   4. 设置`tabindex`属性的元素被点击或键盘操作

---

#### mouseover/mouseout与mouseenter/mouseleave的区别与联系

1. mouseover/mouseout是标准事件，**所有浏览器都支持**；mouseenter/mouseleave是IE5.5引入的特有事件后来被DOM3标准采纳，现代标准浏览器也支持
2. mouseover/mouseout是**冒泡**事件；mouseenter/mouseleave**不冒泡**。需要为**多个元素监听鼠标移入/出事件时，推荐mouseover/mouseout托管，提高性能**
3. 标准事件模型中event.target表示发生移入/出的元素,**vent.relatedTarget**对应移出/如元素；在老IE中event.srcElement表示发生移入/出的元素，**event.toElement**表示移出的目标元素，**event.fromElement**表示移入时的来源元素

例子：鼠标从div#target元素移出时进行处理，判断逻辑如下：

```javascript
<div id="target"><span>test</span></div>

<script type="text/javascript">
var target = document.getElementById('target');
if (target.addEventListener) {
  target.addEventListener('mouseout', mouseoutHandler, false);
} else if (target.attachEvent) {
  target.attachEvent('onmouseout', mouseoutHandler);
}

function mouseoutHandler(e) {
  e = e || window.event;
  var target = e.target || e.srcElement;

  // 判断移出鼠标的元素是否为目标元素
  if (target.id !== 'target') {
    return;
  }

  // 判断鼠标是移出元素还是移到子元素
  var relatedTarget = event.relatedTarget || e.toElement;
  while (relatedTarget !== target
    && relatedTarget.nodeName.toUpperCase() !== 'BODY') {
    relatedTarget = relatedTarget.parentNode;
  }

  // 如果相等，说明鼠标在元素内部移动
  if (relatedTarget === target) {
    return;
  }

  // 执行需要操作
  //alert('鼠标移出');

}
</script>

```

---

#### Javascript跨域通信

同源：两个文档同源需满足

1. 协议相同
2. 域名相同
3. 端口相同

跨域通信：js进行DOM操作、通信时如果目标与当前窗口不满足同源条件，浏览器为了安全会阻止跨域操作。跨域通信通常有以下方法

- 如果是log之类的简单**单项通信**，新建`<img>`,`<script>`,`<link>`,`<iframe>`元素，通过src，href属性设置为目标url。实现跨域请求
- 如果请求**json数据**，使用`<script>`进行jsonp请求
- 现代浏览器中**多窗口通信**使用HTML5规范的targetWindow.postMessage(data, origin);其中data是需要发送的对象，origin是目标窗口的origin。window.addEventListener('message', handler, false);handler的event.data是postMessage发送来的数据，event.origin是发送窗口的origin，event.source是发送消息的窗口引用
- 内部服务器代理请求跨域url，然后返回数据
- 跨域请求数据，现代浏览器可使用HTML5规范的CORS功能，只要目标服务器返回HTTP头部**`Access-Control-Allow-Origin: *`**即可像普通ajax一样访问跨域资源

script、image、iframe的src都不受同源策略的影响。

1. JSONP,回调函数+数据就是 JSON With Padding，简单、易部署。（做法：动态插入script标签，设置其src属性指向提供JSONP服务的URL地址，查询字符串中加入 callback 指定回调函数，返回的 JSON 被包裹在回调函数中以字符串的形式被返回，需将script标签插入body底部）。缺点是只支持GET，不支持POST（原因是通过地址栏传参所以只能使用GET）
2. document.domain 跨子域 （ 例如a.qq.com嵌套一个b.qq.com的iframe ，如果a.qq.com设置document.domain为qq.com 。b.qq.com设置document.domain为qq.com， 那么他俩就能互相通信了，不受跨域限制了。 注意：只能跨子域）
3. window.name + iframe ==> [http://www.tuicool.com/articles/viMFbqV，支持跨主域。不支持POST](http://www.tuicool.com/articles/viMFbqV%EF%BC%8C%E6%94%AF%E6%8C%81%E8%B7%A8%E4%B8%BB%E5%9F%9F%E3%80%82%E4%B8%8D%E6%94%AF%E6%8C%81POST)
4. HTML5的postMessage()方法允许来自不同源的脚本采用异步方式进行有限的通信，可以实现跨文本档、多窗口、跨域消息传递。适用于不同窗口iframe之间的跨域
5. CORS（Cross Origin Resource Share）对方服务端设置响应头
6. 服务端代理 在浏览器客户端不能跨域访问，而服务器端调用HTTP接口只是使用HTTP协议，不会执行JS脚本，不需要同源策略，也就没有跨越问题。简单地说，就是浏览器不能跨域，后台服务器可以跨域。（一种是通过http-proxy-middleware插件设置后端代理；另一种是通过使用http模块发出请求）

CORS请求默认不发送Cookie和HTTP认证信息。如果要把Cookie发到服务器，一方面要服务器同意，指定`Access-Control-Allow-Credentials`字段。

---

#### javascript有哪几种数据类型

六种基本数据类型

- undefined
- null
- string
- boolean
- number
- [symbol](https://developer.mozilla.org/en-US/docs/Glossary/Symbol)(ES6)

一种引用类型

- Object

---

#### Javascript有哪几种方法定义函数

1. [函数声明表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function)
2. [function操作符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function)
3. [Function 构造函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function)
4. [ES6:arrow function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/arrow_functions)

重要参考资料：[MDN:Functions_and_function_scope](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope)

---

#### 应用程序存储和离线web应用

HTML5新增应用程序缓存，允许web应用将应用程序自身保存到用户浏览器中，用户离线状态也能访问。 1.为html元素设置manifest属性:`<html manifest="myapp.appcache">`，其中后缀名只是一个约定，真正识别方式是通过`text/cache-manifest`作为MIME类型。所以需要配置服务器保证设置正确 2.manifest文件首行为`CACHE MANIFEST`，其余就是要缓存的URL列表，每个一行，相对路径都相对于manifest文件的url。注释以#开头 3.url分为三种类型：`CACHE`:为默认类型。`NETWORK`：表示资源从不缓存。 `FALLBACK`:每行包含两个url，第二个URL是指需要加载和存储在缓存中的资源， 第一个URL是一个前缀。任何匹配该前缀的URL都不会缓存，如果从网络中载入这样的URL失败的话，就会用第二个URL指定的缓存资源来替代。以下是一个文件例子：

```
CACHE MANIFEST

CACHE:
myapp.html
myapp.css
myapp.js

FALLBACK:
videos/ offline_help.html

NETWORK:
cgi/

```

---

#### sessionStorage,localStorage,cookie区别

1. 都会在浏览器端保存，有大小限制，同源限制
2. cookie会在请求时发送到服务器，作为会话标识，服务器可修改cookie；web storage不会发送到服务器
3. cookie有path概念，子路径可以访问父路径cookie，父路径不能访问子路径cookie
4. 有效期：cookie在设置的有效期内有效，默认为浏览器关闭；sessionStorage在窗口关闭前有效，localStorage长期有效，直到用户删除
5. 共享：sessionStorage不能共享，localStorage在同源文档之间共享，cookie在同源且符合path规则的文档之间共享
6. localStorage的修改会促发其他文档窗口的update事件
7. cookie有secure属性要求HTTPS传输
8. 浏览器不能保存超过300个cookie，单个服务器不能超过20个，每个cookie不能超过4k。web storage大小支持能达到5M

-----

#### 客户端存储localStorage和sessionStorage

- localStorage有效期为永久，sessionStorage有效期为顶层窗口关闭前
- 同源文档可以读取并修改localStorage数据，sessionStorage只允许同一个窗口下的文档访问，如通过iframe引入的同源文档。
- Storage对象通常被当做普通javascript对象使用：**通过设置属性来存取字符串值**，也可以通过**setItem(key, value)设置**，**getItem(key)读取**，**removeItem(key)删除**，**clear()删除所有数据**，**length表示已存储的数据项数目**，**key(index)返回对应索引的key**

```
localStorage.setItem('x', 1); // storge x->1
localStorage.getItem('x); // return value of x

// 枚举所有存储的键值对
for (var i = 0, len = localStorage.length; i < len; ++i ) {
    var name = localStorage.key(i);
    var value = localStorage.getItem(name);
}

localStorage.removeItem('x'); // remove x
localStorage.clear();  // remove all data

```

---

#### 一次js请求一般情况下有哪些地方会有缓存处理？

1. 浏览器端存储
2. 浏览器端文件缓存
3. HTTP缓存304
4. 服务器端文件类型缓存
5. 表现层&DOM缓存

参考《[一次HTTP请求中有哪些地方可以缓存](http://www.nowamagic.net/librarys/veda/detail/162)》

---

#### cookie及其操作

- cookie是web浏览器存储的少量数据，最早设计为服务器端使用，作为HTTP协议的扩展实现。cookie数据会自动在浏览器和服务器之间传输。
- 通过读写cookie检测是否支持
- cookie属性有**名**，**值**，**max-age**，**path**, **domain**，**secure**；
- cookie默认有效期为浏览器会话，一旦用户关闭浏览器，数据就丢失，通过设置**max-age=seconds**属性告诉浏览器cookie有效期
- cookie作用域通过**文档源**和**文档路径**来确定，通过**path**和**domain**进行配置，web页面同目录或子目录文档都可访问
- 通过cookie保存数据的方法为：为document.cookie设置一个符合目标的字符串如下
- 读取document.cookie获得'; '分隔的字符串，key=value,解析得到结果

```
document.cookie = 'name=qiu; max-age=9999; path=/; domain=domain; secure';

document.cookie = 'name=aaa; path=/; domain=domain; secure';
// 要改变cookie的值，需要使用相同的名字、路径和域，新的值
// 来设置cookie，同样的方法可以用来改变有效期

// 设置max-age为0可以删除指定cookie

//读取cookie，访问document.cookie返回键值对组成的字符串，
//不同键值对之间用'; '分隔。通过解析获得需要的值

```

---

#### javascript有哪些方法定义对象

1. 对象字面量： `var obj = {};`
2. 构造函数： `var obj = new Object();`
3. Object.create(): `var obj = Object.create(Object.prototype);`

---

#### ===运算符判断相等的流程是怎样的

1. 如果两个值不是相同类型，它们不相等
2. 如果两个值都是null或者都是undefined，它们相等
3. 如果两个值都是布尔类型true或者都是false，它们相等
4. 如果其中有一个是**NaN**，它们不相等
5. 如果都是数值型并且数值相等，他们相等， -0等于0
6. 如果他们都是字符串并且在相同位置包含相同的16位值，他它们相等；如果在长度或者内容上不等，它们不相等；两个字符串显示结果相同但是编码不同==和===都认为他们不相等
7. 如果他们指向相同对象、数组、函数，它们相等；如果指向不同对象，他们不相等

#### ==运算符判断相等的流程是怎样的

1. 如果两个值类型相同，按照===比较方法进行比较
2. 如果类型不同，使用如下规则进行比较
3. 如果其中一个值是null，另一个是undefined，它们相等
4. 如果一个值是**数字**另一个是**字符串**，将**字符串转换为数字**进行比较
5. 如果有布尔类型，将**true转换为1，false转换为0**，然后用==规则继续比较
6. 如果一个值是对象，另一个是数字或字符串，将对象转换为原始值然后用==规则继续比较
7. **其他所有情况都认为不相等**

#### 对象到字符串的转换步骤

1. 如果对象有toString()方法，javascript调用它。如果返回一个原始值（primitive value如：string number boolean）,将这个值转换为字符串作为结果
2. 如果对象没有toString()方法或者返回值不是原始值，javascript寻找对象的valueOf()方法，如果存在就调用它，返回结果是原始值则转为字符串作为结果
3. 否则，javascript不能从toString()或者valueOf()获得一个原始值，此时throws a TypeError

#### 对象到数字的转换步骤

```
1. 如果对象有valueOf()方法并且返回元素值，javascript将返回值转换为数字作为结果
2. 否则，如果对象有toString()并且返回原始值，javascript将返回结果转换为数字作为结果
3. 否则，throws a TypeError

```

#### <,>,<=,>=的比较规则

所有比较运算符都支持任意类型，但是**比较只支持数字和字符串**，所以需要执行必要的转换然后进行比较，转换规则如下:

1. 如果操作数是对象，转换为原始值：如果valueOf方法返回原始值，则使用这个值，否则使用toString方法的结果，如果转换失败则报错
2. 经过必要的对象到原始值的转换后，如果两个操作数都是字符串，按照字母顺序进行比较（他们的16位unicode值的大小）
3. 否则，如果有一个操作数不是字符串，**将两个操作数转换为数字**进行比较

#### +运算符工作流程

1. 如果有操作数是对象，转换为原始值
2. 此时如果有**一个操作数是字符串**，其他的操作数都转换为字符串并执行连接
3. 否则：**所有操作数都转换为数字并执行加法**

---

#### 函数内部arguments变量有哪些特性,有哪些属性,如何将它转换为数组

- arguments所有函数中都包含的一个局部变量，是一个类数组对象，对应函数调用时的实参。如果函数定义同名参数会在调用时覆盖默认对象
- arguments[index]分别对应函数调用时的实参，并且通过arguments修改实参时会同时修改实参
- arguments.length为实参的个数（Function.length表示形参长度）
- arguments.callee为当前正在执行的函数本身，使用这个属性进行递归调用时需注意this的变化
- arguments.caller为调用当前函数的函数（已被遗弃）
- 转换为数组：`var args = Array.prototype.slice.call(arguments, 0);`

---

#### DOM事件模型是如何的,编写一个EventUtil工具类实现事件管理兼容

- DOM事件包含捕获（capture）和冒泡（bubble）两个阶段：捕获阶段事件从window开始触发事件然后通过祖先节点一次传递到触发事件的DOM元素上；冒泡阶段事件从初始元素依次向祖先节点传递直到window
- 标准事件监听elem.addEventListener(type, handler, capture)/elem.removeEventListener(type, handler, capture)：handler接收保存事件信息的event对象作为参数，event.target为触发事件的对象，handler调用上下文this为绑定监听器的对象，event.preventDefault()取消事件默认行为，event.stopPropagation()/event.stopImmediatePropagation()取消事件传递
- 老版本IE事件监听elem.attachEvent('on'+type, handler)/elem.detachEvent('on'+type, handler)：handler不接收event作为参数，事件信息保存在window.event中，触发事件的对象为event.srcElement，handler执行上下文this为window使用闭包中调用handler.call(elem, event)可模仿标准模型，然后返回闭包，保证了监听器的移除。event.returnValue为false时取消事件默认行为，event.cancleBubble为true时取消时间传播
- 通常利用事件冒泡机制托管事件处理程序提高程序性能。

```javascript
/**
 * 跨浏览器事件处理工具。只支持冒泡。不支持捕获
 * @author  (qiu_deqing@126.com)
 */

var EventUtil = {
    getEvent: function (event) {
        return event || window.event;
    },
    getTarget: function (event) {
        return event.target || event.srcElement;
    },
    // 返回注册成功的监听器，IE中需要使用返回值来移除监听器
    on: function (elem, type, handler) {
        if (elem.addEventListener) {
            elem.addEventListener(type, handler, false);
            return handler;
        } else if (elem.attachEvent) {
            var wrapper = function () {
              var event = window.event;
              event.target = event.srcElement;
              handler.call(elem, event);
            };
            elem.attachEvent('on' + type, wrapper);
            return wrapper;
        }
    },
    off: function (elem, type, handler) {
        if (elem.removeEventListener) {
            elem.removeEventListener(type, handler, false);
        } else if (elem.detachEvent) {
            elem.detachEvent('on' + type, handler);
        }
    },
    preventDefault: function (event) {
        if (event.preventDefault) {
            event.preventDefault();
        } else if ('returnValue' in event) {
            event.returnValue = false;
        }
    },
    stopPropagation: function (event) {
        if (event.stopPropagation) {
            event.stopPropagation();
        } else if ('cancelBubble' in event) {
            event.cancelBubble = true;
        }
    },
    /**
     * keypress事件跨浏览器获取输入字符
     * 某些浏览器在一些特殊键上也触发keypress，此时返回null
     **/
     getChar: function (event) {
        if (event.which == null) {
            return String.fromCharCode(event.keyCode);  // IE
        }
        else if (event.which != 0 && event.charCode != 0) {
            return String.fromCharCode(event.which);    // the rest
        }
        else {
            return null;    // special key
        }
     }
};

```

---

#### 评价一下三种方法实现继承的优缺点,并改进

```javascript
function Shape() {}
function Rect() {}
// 方法1
Rect.prototype = new Shape();
// 方法2
Rect.prototype = Shape.prototype;

// 方法3
Rect.prototype = Object.create(Shape.prototype);
Rect.prototype.area = function () {
  // do something
};

```

方法1：

1. 优点：正确设置原型链实现继承
2. 优点：父类实例属性得到继承，原型链查找效率提高，也能为一些属性提供合理的默认值
3. 缺点：父类实例属性为引用类型时，不恰当地修改会导致所有子类被修改
4. 缺点：创建父类实例作为子类原型时，可能无法确定构造函数需要的合理参数，这样提供的参数继承给子类没有实际意义，当子类需要这些参数时应该在构造函数中进行初始化和设置
5. 总结：继承应该是继承方法而不是属性，为子类设置父类实例属性应该是通过在子类构造函数中调用父类构造函数进行初始化

方法2：

1. 优点：正确设置原型链实现继承
2. 缺点：父类构造函数原型与子类相同。修改子类原型添加方法会修改父类

方法3：

1. 优点：正确设置原型链且避免方法1.2中的缺点
2. 缺点：ES5方法需要注意兼容性

改进：

1. 所有三种方法应该在子类构造函数中调用父类构造函数实现实例属性初始化

```javascript
function Rect() {
    Shape.call(this);
}

```

1. 用新创建的对象替代子类默认原型，设置`Rect.prototype.constructor = Rect;`保证一致性
2. 第三种方法的polyfill：

```javascript
function create(obj) {
    if (Object.create) {
        return Object.create(obj);
    }

    function f() {};
    f.prototype = obj;
    return new f();
}
```

#### 说说你对作用域链的理解

作用域链的作用是保证执行环境里有权访问的变量和函数是有序的，作用域链的变量只能向上访问，变量访问到window对象即被终止，作用域链向下访问变量是不被允许的。

#### js继承方式及其优缺点

- 原型链继承的缺点

一是字面量重写原型会中断关系，使用引用类型的原型，并且子类型还无法给超类型传递参数。

- 借用构造函数（类式继承）

借用构造函数虽然解决了刚才两种问题，但没有原型，则复用无从谈起。所以我们需要原型链+借用构造函数的模式，这种模式称为组合继承

- 组合式继承

组合式继承是比较常用的一种继承方法，其背后的思路是 使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承。这样，既通过在原型上定义方法实现了函数复用，又保证每个实例都有它自己的属性。

[JavaScript继承方式详解](https://segmentfault.com/a/1190000002440502)

---

#### **获得一个DOM元素的绝对位置**

[offsetTop](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetTop)：返回当前元素相对于其 [offsetParent](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetParent) 元素的顶部的距离

[offsetLeft](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetLeft)：返回当前元素相对于其 [offsetParent](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetParent) 元素的左边的距离

[getBoundingClientRect()](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getBoundingClientRect)：返回值是一个[DOMRect](https://developer.mozilla.org/zh-CN/docs/Mozilla/Tech/XPCOM/Reference/Interface/nsIDOMClientRect)对象，它包含了一组用于描述边框的只读属性——left、top、right和bottom，属性单位为像素

参考《[JavaScript中尺寸、坐标](http://www.cnblogs.com/strick/p/4826273.html)》，[查看在线代码](http://codepen.io/strick/pen/XmQaaX)。

------

#### 如何利用JS生成一个table

首先是用[createElement](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createElement)创建一个table，再用[setAttribute](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/setAttribute)设置table的属性，

然后用for循环设置tr和td的内容，用[appendChild](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/appendChild)拼接内容，设置td的时候还用到[innerHTML](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/innerHTML)和[style](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Properties_Reference).padding。

[查看在线代码](http://codepen.io/strick/pen/wKZqpR)。参考《[JavaScript要点归档：DOM表格](http://myweb.jowai.info/javascript-main-points-archive-dom-table/)》《[JavaScript要点归档：DOM](http://myweb.jowai.info/javascript-main-points-archive-dom/)》

------

#### 实现预加载一张图片，加载完成后显示在网页中并设定其高度为50px，宽度为50px

先new [Image](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLImageElement/Image)()获取一个图片对象，然后在图片对象的onload中设置宽度和高度。[查看在线代码](http://codepen.io/strick/pen/vNMJVr)。

---

#### 假设有一个4行tr的table，将table里面tr顺序颠倒

先是通过table.tBodies[0].rows获取到当前tbody中的行，接下来是两种方法处理。获取到的行没有[reverse](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)这个方法。

第一种是将这些行push到另外一个数组中

第二种是用Array.prototype.[slice](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice).call()将那些行变成数组，

接着用reverse倒叙，table再appendChild。[查看在线代码](http://codepen.io/strick/pen/VvNzqX)。

这里我有个疑问，就是在appendChild的时候，并不是在最后把列加上，而是做了替换操作？

---

#### 模拟一个HashTable类，一个类上注册四个方法：包含有add、remove、contains、length方法

先是在构造函数中定义一个数组，然后用push模拟add，splice模拟remove。

四个方法都放在了[prototype](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype)上面。[查看在线代码](http://codepen.io/strick/pen/VvNBom)。

---

#### ES6相关

#### 谈一谈let与var和const的区别？

- let为ES6新添加申明变量的命令，它类似于var，但是有以下不同：
- let命令不存在变量提升，如果在let前使用，会导致报错
- 暂时性死区的本质，其实还是块级作用域必须“先声明后使用”的性质。
- let，const和class声明的全局变量不是全局对象的属性。

const声明的变量与let声明的变量类似，它们的不同之处在于，const声明的变量只可以在声明时赋值，不可随意修改，否则会导致SyntaxError（语法错误）。

const只是保证变量名指向的地址不变，并不保证该地址的数据不变。const可以在多个模块间共享 let 暂时性死区的原因：var 会变量提升，let 不会。

#### 箭头函数

箭头函数不属于普通的 function，所以没有独立的上下文。箭头函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。 由于箭头函数没有自己的this，函数对象中的call、apply、bind三个方法，无法"覆盖"箭头函数中的this值。 箭头函数没有原本(传统)的函数有的隐藏arguments对象。 箭头函数不能当作generators使用，使用yield会产生错误。

在以下场景中不要使用箭头函数去定义：

- 定义对象方法、定义原型方法、定义构造函数、定义事件回调函数。
- 箭头函数里不但没有 this，也没有 arguments, super ……

#### Symbol，Map和Set

Map 对象保存键值对。一个对象的键只能是字符串或者 Symbols，但一个 Map 的键可以是任意值。 Set 对象允许你存储任何类型的唯一值，Set对象是值的集合，Set中的元素只会出现一次 Symbol 是一种特殊的、不可变的数据类型，可以作为对象属性的标识符使用(Symbol([description]) )

```javascript
let mySet = new Set()
mySet.add(1)
mySet.add('hello')
mySet.add('hello')
console.log(mySet.size);//2
console.log(mySet);//Set {1,'hello'}

//Map保存键值对也不能有重复的
let myMap = new Map();
let key1 = 'China',key2 = 'America';
myMap.set(key1,'welcome')
myMap.set(key2,'gold bless you')
console.log(myMap);//Map { 'China' => 'welcome', 'America' => 'gold bless you' }
console.log(myMap.get(key1));//welcome
console.log(myMap.get(key2));//gold bless you

let mySymbol = Symbol('symbol1');
let mySymbol2 = Symbol('symbol1');
console.log(mySymbol == mySymbol2);//false
//Symbols 在 for...in 迭代中不可枚举。
let obj = {}
obj['c'] = 'c'
obj.d ='d'
obj[Symbol('a')] = 'a'
obj[Symbol.for('b')] = 'b'
for(let k in obj){
    console.log(k);//logs 'c' and 'd'
}
```

`for...of`可以用来遍历数组，类数组对象，argument，字符串，Map和Set，`for...in`用来遍历对象

---

#### ES6 module和require/exports/module.exports的区别

ES6 Module 中导入模块的属性或者方法是强绑定的，包括基础类型；而 CommonJS 则是普通的值传递或者引用传递。

CommonJS模块是运行时的，导入导出是通过值的复制来达成的。ES6的模块是静态的，导入导出实际上是建立符号的映射

import必须放在文件最顶部，require不需要；import最终会被babel编译为require

---

#### babel的原理

使用 babylon 解析器对输入的源代码字符串进行解析并生成初始 AST 遍历 AST 树并应用各 transformers（plugin） 生成变换后的 AST 树 利用 babel-generator 将 AST 树输出为转码后的代码字符串 分为三个阶段：

解析：将代码字符串解析成抽象语法树 变换：对抽象语法树进行变换操作 再建：根据变换后的抽象语法树再生成代码字符串

---

#### Promise的原理

现在回顾下Promise的实现过程，其主要使用了设计模式中的观察者模式：

- 通过`Promise.prototype.then`和`Promise.prototype.catch`方法将观察者方法注册到被观察者Promise对象中，同时返回一个新的Promise对象，以便可以链式调用。
- 被观察者管理内部pending、fulfilled和rejected的状态转变，同时通过构造函数中传递的resolve和reject方法以主动触发状态转变和通知观察者。

`Promise.then()`是异步调用的，这也是Promise设计上规定的，其原因在于同步调用和异步调用同时存在会导致混乱。

为了暂停当前的 promise，或者要它等待另一个 promise 完成，只需要简单地在 then() 函数中返回另一个 promise。

Promise 也有一些缺点。首先，无法取消 Promise，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部。第三，当处于 Pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

一般来说，不要在then方法里面定义Reject状态的回调函数（即then的第二个参数），总是使用catch方法，理由是更接近同步的写法。 then的第二个函数参数和catch等价

- Promise.all和Promise.race的区别？

Promise.all 把多个promise实例当成一个promise实例,当这些实例的状态都发生改变时才会返回一个新的promise实例，才会执行then方法。 Promise.race 只要该数组中的 Promise 对象的状态发生变化（无论是resolve还是reject）该方法都会返回。

---

1.什么是类数组对象，如何将类数组对象转为真正的数组

拥有length属性和若干索引属性的对象, 类数组只有索引值和长度，没有数组的各种方法，所以如果要类数组调用数组的方法，就需要使用 Array.prototype.method.call 来实现。

```javascript
var arrayLike = {0: 'name', 1: 'age', 2: 'sex', length: 3 }
// 1. slice
Array.prototype.slice.call(arrayLike); // ["name", "age", "sex"]
// 2. splice
Array.prototype.splice.call(arrayLike, 0); // ["name", "age", "sex"]
// 3. ES6 Array.from
Array.from(arrayLike); // ["name", "age", "sex"]
// 4. apply
Array.prototype.concat.apply([], arrayLike)
```



4.bind返回什么

bind() 方法会返回一个新函数, 又叫绑定函数, 当调用这个绑定函数时, 绑定函数会以创建它时传入 bind() 方法的第一个参数作为当前的上下文, 即this, 传入 bind() 方法的第二个及之后的参数加上绑定函数运行时自身的参数按照顺序作为原函数的参数来调用原函数.

```javascript
var x = 8;
var o = {
  x: 10,
  getX: function(){
      console.log(this.x);
  }
};
var f = o.getX;
f();//8, 由于没有绑定执行时的上下文, this默认指向window, 打印了全局变量x的值
var g = f.bind(o);
g();//10, 绑定this后, 成功的打印了o对象的x属性的值.
```

#### apply, call和bind有什么区别?

参考答案：三者都可以把一个函数应用到其他对象上，call、apply是修改函数的作用域（修改this指向），并且立即执行，而bind是返回了一个新的函数，不是立即执行．apply和call的区别是apply接受数组作为参数，而call是接受逗号分隔的无限多个参数列表，

```javascript
Array.prototype.slice.call(null, args)

function getMax(arr){
  return Math.max.apply(null, arr);
}
//call
function foo() {
  console.log(this);//{id: 42}
}

foo.call({ id: 42 });
```

如果该方法是非严格模式代码中的函数，则null和undefined将替换为全局对象，并且原始值将被包装。 当你调用apply传递给它null时，就像是调用函数而不提供任何对象



6.箭头函数 箭头函数没有它自己的this值，箭头函数内的this值继承自外围作用域

箭头函数不能用作构造器，不能和new一起使用 箭头函数没有原型属性 yield关键字不能在箭头函数使用 在以下场景中不要使用箭头函数去定义：

- 定义对象方法、定义原型方法、定义构造函数、定义事件回调函数。

#### new操作符具体做了什么

1、创建一个空对象，并且this变量引用该对象，同时继承了该函数的原型（实例对象通过`__proto__`属性指向原型对象；`obj.__proto__ = Base.prototype;`） 2、属性和方法被加入到 this 引用的对象中。

```javascript
function Animal(name) {
    this.name = name;
}

Animal.prototype.run = function() {
    console.log(this.name + 'can run...');
}

var cat = new Animal('cat');
//模拟过程
new Animal('cat')=function(){
    let obj={};  //创建一个空对象
    obj.__proto__=Animal.prototype;
    //把该对象的原型指向构造函数的原型对象，就建立起原型了：obj->Animal.prototype->Object.prototype->null
    return Animal.call(obj,'cat');// 绑定this到实例化的对象上
}
```

- ​

---

### 原生DOM操作和事件相关

- 如需替换 HTML DOM 中的元素，请使用`replaceChild(newnode,oldnode)`方法
- 从父元素中删除子元素 `parent.removeChild(child)`;
- `insertBefore(newItem,existingItem)` 在指定的已有子节点之前插入新的子节点
- `appendChild(newListItem`向元素添加新的子节点，作为最后一个子节点 document.documentElement - 全部文档 document.body - 文档的主体

<http://www.w3school.com.cn/jsref/dom_obj_all.asp>

- JS事件：target与currentTarget区别

target在事件流的目标阶段；currentTarget在事件流的捕获，目标及冒泡阶段。只有当事件流处在目标阶段的时候，两个的指向才是一样的， 而当处于捕获和冒泡阶段的时候，target指向被单击的对象而currentTarget指向当前事件活动的对象（一般为父级）。

#### 事件模型

事件捕捉阶段：事件开始由顶层对象触发，然后逐级向下传播，直到目标的元素； 处于目标阶段：处在绑定事件的元素上； 事件冒泡阶段：事件由具体的元素先接收，然后逐级向上传播，直到不具体的元素；

- 阻止 冒泡／捕获 `event.stopPropagation()`和IE的`event.cancelBubble=true`
- DOM事件绑定 1.绑定事件监听函数：addEventListener和attchEvent 2.在JavaScript代码中绑定：获取DOM元素 `dom.onlick = fn` 3.在DOM元素中直接绑定：`<div onclick = 'fn()'>`

DOM事件流包括三个阶段：事件捕获阶段、处于目标阶段、事件冒泡阶段。首先发生的事件捕获，为截获事件提供机会。然后是实际的目标接受事件。最后一个阶段是时间冒泡阶段，可以在这个阶段对事件做出响应。

#### 事件委托

因为事件具有冒泡机制，因此我们可以利用冒泡的原理，把事件加到父级上，触发执行效果。这样做的好处当然就是提高性能了

最重要的是通过`event.target.nodeName`判断子元素

```javascript
<div>
  <ul id = "bubble">
    <li>1</li>
<li>2</li>
<li>3</li>
<li>4</li>
</ul>
</div>

window.onload = function () {
  var aUl = document.getElementsById("bubble");
  var aLi = aUl.getElementsByTagName("li");

  //不管在哪个事件中，只要你操作的那个元素就是事件源。
  // ie：window.event.srcElement
  // 标准下:event.target
  aUl.onmouseover = function (ev) {
    var ev = ev || window.event;
    var target = ev.target || ev.srcElement;

    if(target.nodeName.toLowerCase() == "li"){
      target.style.background = "blue";
    }
  };
};
```



---

#### 参考

[^1]: http://www.jianshu.com/p/d647aa6d1ae6
[^2]: *JavaScript高级程序设计*

[3]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Equality_comparisons_and_sameness

[^4]: https://github.com/es6-org/exploring-es6/blob/master/md/24.10.md
[^5]: http://2ality.com/2011/04/iterating-over-arrays-and-objects-in.html