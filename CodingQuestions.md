### 代码相关问题

### 基础语法

#### foo的值是什么？

```javascript
var foo = 10 + '20';
```

类型转换，`foo='1020'`

#### 下面的语句的返回值是什么？

```javascript
"i'm a lasagna hog".split("").reverse().join("");
```

这不就是个常用的字符串逆转嘛；

#### window.foo的值是什么？

```javascript
( window.foo || ( window.foo = "bar" ) );
```

这道题也比较 常见了，逻辑运算里，遇到`||`或则：前部分为真，后部分执行；前部分为假，后部分执行；

- 如果`window`没有属性`foo`或者该属性值为假，则执行后` window.foo = "bar" `
- 如果` window.foo`为真，则值不会变，因为不会执行后半句

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

#### 速答

`typeof []=object`

`typeof arguments=object`

`2+true=3`

`4+3+2+"1"="91"`

`"1"+2+4="124"`

`-'34'+10=-24`

`+'dude'=NaN`

若`var y = 1, x = y = typeof x;`  `x=undefined`

`var a = (2, 3, 5);` 则`a=5`

`var a = (1, 5 - 1) * 2`  则`a=8`

`!'bang'=false`

`parseFloat('12.3.4')=12.3`

`Math.max([2,3,4,5])=NaN;`

`3 instanceof Number=false`

**`null == undefined` true

`!!function(){};`true

`typeof bar = undefined`

`typeof null=object` 

`var a = 2, b =3` ，则`a && b=3`

`var foo = 'outside'; function logIt(){console.log(foo); var foo = 'inside';} logIt();`

输出：`undefined`

`-5%2=-1`

`42..toString()` `"42"`

`4.2..toString` //SyntaxError: Unexpected token .

`42 . toString()` `"42"`

`typeof(NaN)` `"number"`

`2 in [1,2]` `false`



#### 给String对象添加一个方法，传入一个string类型的参数，然后将string的每个字符间价格空格返回，例如：

`addSpace(“hello world”) // -> ‘h e l l o  w o r l d’`

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

#### 如何将浮点数点左边的数每三位添加一个逗号，如12000000.11转化为『12,000,000.11』?

```javascript
function commafy(num){
  return num && num
    .toString()
    .replace(/(\d)(?=(\d{3})+.)/g, function($1, $2){
    return $2 + ',';
  });
}
```

#### 在Javascript中什么是伪数组？如何将伪数组转化为标准数组？

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

#### 请用代码写出(今天是星期x)其中x表示当天是星期几,如果当天是星期一,输出应该是"今天是星期一"

```javascript
var days = ['日','一','二','三','四','五','六'];
var date = new Date();

console.log('今天是星期' + days[date.getDay()]);

```

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

#### `[].forEach.call($("*"),function(a){a.style.outline="1px solid #"+(~~(Math.random()*(1<<24))).toString(16)})` 能解释一下这段代码的意思吗？

#### 如何判断一个对象是否为数组

如果浏览器支持Array.isArray()可以直接判断否则需进行必要判断

```javascript
/**
 * 判断一个对象是否是数组，参数不是对象或者不是数组，返回false
 *
 * @param {Object} arg 需要测试是否为数组的对象
 * @return {Boolean} 传入参数是数组返回true，否则返回false
 */
function isArray(arg) {
    if (typeof arg === 'object') {
        return Object.prototype.toString.call(arg) === '[object Array]';
    }
    return false;
}


```

#### 定义一个log方法，让它可以代理console.log的方法。

```javascript
function log(){
  var args = Array.prototype.slice.call(arguments);
  args.unshift('(app)');
  console.log.apply(console, args);
}
```



#### 如何实现下列代码：`[1,2,3,4,5].duplicator(); // [1,2,3,4,5,1,2,3,4,5]`

```javascript
Array.prototype.duplicator = function() {
  return this.concat(this);
}
```

#### 编写一个函数接受url中query string为参数,返回解析后的Object,query string使用application/x-www-form-urlencoded编码

```javascript
/**
 * 解析query string转换为对象，一个key有多个值时生成数组
 *
 * @param {String} query 需要解析的query字符串，开头可以是?，
 * 按照application/x-www-form-urlencoded编码
 * @return {Object} 参数解析后的对象
 */
function parseQuery(query) {
  var result = {};

  // 如果不是字符串返回空对象
  if (typeof query !== 'string') {
    return result;
  }

  // 去掉字符串开头可能带的?
  if (query.charAt(0) === '?') {
    query = query.substring(1);
  }

  var pairs = query.split('&');
  var pair;
  var key, value;
  var i, len;

  for (i = 0, len = pairs.length; i < len; ++i) {
    pair = pairs[i].split('=');
    // application/x-www-form-urlencoded编码会将' '转换为+
    key = decodeURIComponent(pair[0]).replace(/\+/g, ' ');
    value = decodeURIComponent(pair[1]).replace(/\+/g, ' ');

    // 如果是新key，直接添加
    if (!(key in result)) {
      result[key] = value;
    }
    // 如果key已经出现一次以上，直接向数组添加value
    else if (isArray(result[key])) {
      result[key].push(value);
    }
    // key第二次出现，将结果改为数组
    else {
      var arr = [result[key]];
      arr.push(value);
      result[key] = arr;
    } // end if-else
  } // end for

  return result;
}

function isArray(arg) {
  if (arg && typeof arg === 'object') {
    return Object.prototype.toString.call(arg) === '[object Array]';
  }
  return false;
}
/**
console.log(parseQuery('sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8'));
 */
```

#### 解析一个完整的url,返回Object包含域与window.location相同

```javascript
/**
 * 解析一个url并生成window.location对象中包含的域
 * location:
 * {
 *      href: '包含完整的url',
 *      origin: '包含协议到pathname之前的内容',
 *      protocol: 'url使用的协议，包含末尾的:',
 *      username: '用户名', // 暂时不支持
 *      password: '密码',  // 暂时不支持
 *      host: '完整主机名，包含:和端口',
 *      hostname: '主机名，不包含端口'
 *      port: '端口号',
 *      pathname: '服务器上访问资源的路径/开头',
 *      search: 'query string，?开头',
 *      hash: '#开头的fragment identifier'
 * }
 *
 * @param {string} url 需要解析的url
 * @return {Object} 包含url信息的对象
 */
function parseUrl(url) {
  var result = {};
  var keys = ['href', 'origin', 'protocol', 'host',
              'hostname', 'port', 'pathname', 'search', 'hash'];
  var i, len;
  var regexp = /(([^:]+:)\/\/(([^:\/\?#]+)(:\d+)?))(\/[^?#]*)?(\?[^#]*)?(#.*)?/;

  var match = regexp.exec(url);

  if (match) {
    for (i = keys.length - 1; i >= 0; --i) {
      result[keys[i]] = match[i] ? match[i] : '';
    }
  }

  return result;
}


```



---

### 函数/作用域

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

#### 下面这段代码想要循环延时输出结果0 1 2 3 4,请问输出结果是否正确,如果不正确,请说明为什么,并修改循环内的代码使其输出正确结果

```javascript
for (var i = 0; i < 5; ++i) {
  setTimeout(function () {
    console.log(i + ' ');
  }, 100);
}

```

不能输出正确结果，因为循环中setTimeout接受的参数函数通过闭包访问变量i。javascript运行环境为单线程，setTimeout注册的函数需要等待线程空闲才能执行，此时for循环已经结束，i值为5.五个定时输出都是5 修改方法：将setTimeout放在函数立即调用表达式中，将i值作为参数传递给包裹函数，创建新闭包

```javascript
for (var i = 0; i < 5; ++i) {
  (function (i) {
    setTimeout(function () {
      console.log(i + ' ');
    }, 100);
  }(i));
}

```

#### 如何判断一个对象是否为函数

```javascript
/**
 * 判断对象是否为函数，如果当前运行环境对可调用对象（如正则表达式）
 * 的typeof返回'function'，采用通用方法，否则采用优化方法
 *
 * @param {Any} arg 需要检测是否为函数的对象
 * @return {boolean} 如果参数是函数，返回true，否则false
 */
function isFunction(arg) {
    if (arg) {
        if (typeof (/./) !== 'function') {
            return typeof arg === 'function';
        } else {
            return Object.prototype.toString.call(arg) === '[object Function]';
        }
    } // end if
    return false;
}


```



---

### 事件

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

### 应用

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

#### 编写javascript深度克隆函数deepClone

```javascript
function deepClone(obj) {
    var _toString = Object.prototype.toString;

    // null, undefined, non-object, function
    if (!obj || typeof obj !== 'object') {
        return obj;
    }

    // DOM Node
    if (obj.nodeType && 'cloneNode' in obj) {
        return obj.cloneNode(true);
    }

    // Date
    if (_toString.call(obj) === '[object Date]') {
        return new Date(obj.getTime());
    }

    // RegExp
    if (_toString.call(obj) === '[object RegExp]') {
        var flags = [];
        if (obj.global) { flags.push('g'); }
        if (obj.multiline) { flags.push('m'); }
        if (obj.ignoreCase) { flags.push('i'); }

        return new RegExp(obj.source, flags.join(''));
    }

    var result = Array.isArray(obj) ? [] :
        obj.constructor ? new obj.constructor() : {};

    for (var key in obj ) {
        result[key] = deepClone(obj[key]);
    }

    return result;
}

function A() {
    this.a = a;
}

var a = {
    name: 'qiu',
    birth: new Date(),
    pattern: /qiu/gim,
    container: document.body,
    hobbys: ['book', new Date(), /aaa/gim, 111]
};

var c = new A();
var b = deepClone(c);
console.log(c.a === b.a);
console.log(c, b);


```



---

### 对象构造／继承

#### 小贤是一条可爱的小狗(Dog)，它的叫声很好听(wow)，每次看到主人的时候就会乖乖叫一声(yelp)。从这段描述可以得到以下对象：

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

#### 现有一个Page类,其原型对象上有许多以post开头的方法(如postMsg);另有一拦截函数check,只返回true或false。请设计一个函数，该函数应批量改造原Page的postXXX方法,在保留其原有功能的同时,为每个postXXX方法增加拦截验证功能,当check返回true时继续执行原postXXX方法,返回false时不再执行原postXXX方法

```javascript
function Page() {}

Page.prototype = {
  constructor: Page,

  postA: function (a) {
    console.log('a:' + a);
  },
  postB: function (b) {
    console.log('b:' + b);
  },
  postC: function (c) {
    console.log('c:' + c);
  },
  check: function () {
    return Math.random() > 0.5;
  }
}

function checkfy(obj) {
  for (var key in obj) {
    if (key.indexOf('post') === 0 && typeof obj[key] === 'function') {
      (function (key) {
        var fn = obj[key];
        obj[key] = function () {
          if (obj.check()) {
            fn.apply(obj, arguments);
          }
        };
      }(key));
    }
  }
} // end checkfy()

checkfy(Page.prototype);

var obj = new Page();

obj.postA('checkfy');
obj.postB('checkfy');
obj.postC('checkfy');

```

---

#### 参考

[^1]: http://www.thatjsdude.com/interview/js1.html
[^2]: [深拷贝与浅拷贝](https://github.com/wengjq/Blog/issues/3)