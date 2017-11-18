## 代码相关问题



#### 问题：foo的值是什么？

```javascript
var foo = 10 + '20';
```

类型转换，`foo='1020'`

---
#### 问题：如何实现以下函数？

```javascript
add(2, 5); // 7
add(2)(5); // 7
```

第一个就是普通函数了，第二个明显就是闭包来传递参数；

```javascript
const add1 = (num1, num2) => (num1+num2);
const add2 = num1 => {
  return num2 => (num1+num2);
};
```



---
#### 问题：下面的语句的返回值是什么？

```javascript
"i'm a lasagna hog".split("").reverse().join("");
```

这不就是个常用的字符串逆转嘛；

---
#### 问题：window.foo的值是什么？

```javascript
( window.foo || ( window.foo = "bar" ) );
```

这道题也比较 常见了，逻辑运算里，遇到`||`或则：前部分为真，后部分执行；前部分为假，后部分执行；

* 如果`window`没有属性`foo`或者该属性值为假，则执行后` window.foo = "bar" `
* 如果` window.foo`为真，则值不会变，因为不会执行后半句

---
#### 问题：下面两个 alert 的结果是什么？

```javascript
var foo = "Hello";
(function() {
  var bar = " World";
  alert(foo + bar);
})();
alert(foo + bar);
```

这道题就是考察变量的作用域了，`var`声明的变量是有函数作用域的，所以第一个`alert`输出`Hello World`，第二个 因为`bar`变量不存在会报错(本来以为会输出`Hello undefined`)。

---
#### 问题：foo.length的值是什么？

```javascript
var foo = [];
foo.push(1);
foo.push(2);
```

额，莫非出题目的是考察数组的常识？2

---
#### 问题：foo.x的值是什么？

```javascript
var foo = {n: 1};
var bar = foo;
foo.x = foo = {n: 2};
```

这道题值得深究下，乍一看觉得结果不就是`{n: 2}`嘛，其实内涵深刻。

我们来深究下，首先JS中连等是允许的，从右往左依次赋值。

前两句代码，主要指明`foo`和`bar`共同指向一个对象（由于JS中变量赋值给变量是浅拷贝）。

执行`foo={n:2}`后，由于赋的值是字面常量，其实`foo`指向的地址已经更改，和`bar`不共享一个地址了。但在`foo.x={n:2}`这个上下文中，foo还和bar住一起，所以赋值后`bar.x={n:2}`，最后的执行结果就变成：

```javascript
foo={n:2}
bar={n:1,x:{n:2}}
```



---
#### 问题：下面代码的输出是什么？

```javascript
console.log('one');
setTimeout(function() {
  console.log('two');
}, 0);
console.log('three');
```
这个就是JS中的事件回调机制了，主线程事件执行完后，再依次执行回调栈里的事件。结果 `one three two`