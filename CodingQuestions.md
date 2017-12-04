## 代码相关问题

#### foo的值是什么？

```javascript
var foo = 10 + '20';
```

类型转换，`foo='1020'`

---
#### 如何实现以下函数？

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
#### 下面的语句的返回值是什么？

```javascript
"i'm a lasagna hog".split("").reverse().join("");
```

这不就是个常用的字符串逆转嘛；

---
#### window.foo的值是什么？

```javascript
( window.foo || ( window.foo = "bar" ) );
```

这道题也比较 常见了，逻辑运算里，遇到`||`或则：前部分为真，后部分执行；前部分为假，后部分执行；

* 如果`window`没有属性`foo`或者该属性值为假，则执行后` window.foo = "bar" `
* 如果` window.foo`为真，则值不会变，因为不会执行后半句

---
#### 下面两个 alert 的结果是什么？

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
#### foo.length的值是什么？

```javascript
var foo = [];
foo.push(1);
foo.push(2);
```

额，莫非出题目的是考察数组的常识？2

---
#### foo.x的值是什么？

```javascript
var foo = {n: 1};
var bar = foo;
foo.x = foo = {n: 2};
```

这道题值得深究下，乍一看觉得结果不就是`{n: 2}`嘛，其实内涵深刻。

我们来深究下，首先JS中连等是允许的，从右往左依次赋值。

前两句代码，主要指明`foo`和`bar`共同指向一个对象（由于JS中变量赋值给变量是浅拷贝）。

执行`foo={n:2}`后，由于赋的值是字面常量，其实`foo`指向的地址已经更改，和`bar`不共享一个地址了。但在`foo.x={n:2}`这个上下文中，`foo`还和`bar`住一起，所以赋值后`bar.x={n:2}`，最后的执行结果就变成：

```javascript
foo={n:2}
bar={n:1,x:{n:2}}
```



---
#### 下面代码的输出是什么？

```javascript
console.log('one');
setTimeout(function() {
  console.log('two');
}, 0);
console.log('three');
```
这个就是JS中的事件机制了，主线程事件执行完后，再依次执行回调栈里的事件。结果 `one three two`

---

#### 下面代码的输出是什么？

```javascript
var name = 'World';
(function(){
  if(typeof name === 'undefined'){
    var name = "Jack";
    console.info('Goodbye '+ name);
  }else{
    console.info('Hello ' + name);
  }
})();
```

这道题考察的是`var`变量提升，尽管全局初始化了`name`变量，但在匿名函数里，变量创建的时候会先识别到`if`中的`name`，但此时只是声明了对象，并没有赋值；所以代码会进入`name==='undefined'`分支，最终输出`Goodbye Jack`。

---

#### 实现对象的深拷贝/实现一个函数clone，可以对JavaScript中的5种主要的数据类型（包括Number、String、Object、Array、Boolean）进行值复制。

考察JS中数据的类型判断和对值传递／引用传递的理解。

```javascript
// 添加原型方法
Object.prototype.clone = function() {
  // 由于数值、字符串和布尔类型是基础数据类型，值传递，直接返回即可
  // 判断类型为Number或者String或者Boolean
  if(typeof this === 'number' ||  ) {
    return this;
  }
  // 数组判断 ES5中有Array.isArray()方法
  // 防止该方法不兼容，可使用Object.prototype.toString.call(this)
  if(Array.isArray(this) || Object.prototype.toString.call(this)){
  return this.concat();
  }
  // 如果为对象类型，如果直接返回实际返回的是引用地址，返回的变量与原变量共享数据，为浅拷贝
  // 因此需要深度拷贝
  // JS中深拷贝可以使用JSON.parse(JSON.stringify(source))，但该方法会丢失正则与函数属性
  if(typeof this === 'object') {
  	
  }
}
```



---

#### 

---

### 继承问题

小贤是一条可爱的小狗(Dog)，它的叫声很好听(wow)，每次看到主人的时候就会乖乖叫一声(yelp)。从这段描述可以得到以下对象：

```JavaScript
functionDog(){
  this.wow=function(){
    alert(’Wow’);
  }
  this.yelp=function(){
    this.wow();
  }
}
```

小芒和小贤一样，原来也是一条可爱的小狗，可是突然有一天疯了(MadDog)，一看到人就会每隔半秒叫一声(wow)地不停叫唤(yelp)。请根据描述，按示例的形式用代码来实。（继承，原型，setInterval）



---

### 下面这个ul，如何点击每一列的时候alert其index?（闭包）

```html
<ul id=”test”>
  <li>这是第一条</li>
  <li>这是第二条</li>
  <li>这是第三条</li>
</ul>
```



---

### 编写一个JavaScript函数，输入指定类型的选择器(仅需支持id，class，tagName三种简单CSS选择器，无需兼容组合选择器)可以返回匹配的DOM节点，需考虑浏览器兼容性和性能。

```javascript
/** 
  @param selector {String} 传入的CSS选择器。 
  @return {Array}
*/
```



---

7.给String对象添加一个方法，传入一个string类型的参数，然后将string的每个字符间价格空格返回，例如：

`addSpace(“hello world”) // -> ‘h e l l o  w o r l d’`



---

8.定义一个log方法，让它可以代理console.log的方法。



---

9.在Javascript中什么是伪数组？如何将伪数组转化为标准数组？



---

11.原生JS的window.onload与Jquery的$(document).ready(function(){})有什么不同？如何用原生JS实现Jq的ready方法？



---



---

### 参考

[^1]: http://www.thatjsdude.com/interview/js1.html