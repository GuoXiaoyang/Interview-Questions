## 代码相关问题



#### 问题：foo的值是什么？

```javascript
var foo = 10 + '20';
```

---
#### 问题：如何实现以下函数？

```javascript
add(2, 5); // 7
add(2)(5); // 7
```

---
#### 问题：下面的语句的返回值是什么？

```javascript
"i'm a lasagna hog".split("").reverse().join("");
```

---
#### 问题：window.foo的值是什么？

```javascript
( window.foo || ( window.foo = "bar" ) );
```

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

---
#### 问题：foo.length的值是什么？

```javascript
var foo = [];
foo.push(1);
foo.push(2);
```

---
#### 问题：foo.x的值是什么？

```javascript
var foo = {n: 1};
var bar = foo;
foo.x = foo = {n: 2};
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