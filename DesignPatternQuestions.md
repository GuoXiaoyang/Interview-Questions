### 设计模式相关问题

#### 说说观察者模式

JS里对观察者模式的实现是通过回调来实现的，它定义了一种一对多的关系，让多个观察者对象同时监听某一个主题对象

观察者模式：对程序中某一个对象的进行实时的观察，当该对象状态发生改变的时候 进行通知

我们为什么要用观察者模式呢，主要是可以实现松散耦合的代码，什么意思？就是 主体和订阅者之间是相互独立的，其二者可以独立运行。

发布／订阅模式



观察者模式与发布／订阅模式区别



---

代理模式



---

装饰器模式



---

#### 个人觉得比较好的单例模式实现：

```javascript
function Single() {
  var instance = this;
  this.name='Tonny';
  // 生成单例后重写构造函数
  Single = function() {return instance;}
}
//test
var instance1 = new Single();
var instance2 = new Single();
console.log(instance1 === instance2);
instance1.age = 29;
console.log(instance1.name, instance1.age);
console.log(instance2.name, instance2.age);
```

​

```javascript
//支持原型属性的单例模式
function Single() {
  var instance;
  // 生成一次单例后重写构造函数
  Single = function() {
    return instance;
  }
  Single.prototype = this; //重写原型对象
  instance = new Single(); //创建实例
  instance.constructor = Single;//重设构造函数指针
  instance.name = 'Tonny';
  return instance;
}

// test
var instance1 = new Single();
var instance2 = new Single();
console.log(instance1 === instance2);
Single.prototype.age = 29;
console.log(instance1.name, instance1.age);
console.log(instance2.name, instance2.age);
```

#### JS常用设计模式的实现思路，单例，工厂，代理，装饰，观察者模式等

参考答案：

```javascript
1) 单例：　任意对象都是单例，无须特别处理
var obj = {name: 'michaelqin', age: 30};

2) 工厂: 就是同样形式参数返回不同的实例
function Person() { this.name = 'Person1'; }
function Animal() { this.name = 'Animal1'; }

function Factory() {}
Factory.prototype.getInstance = function(className) {
  return eval('new ' + className + '()');
}

var factory = new Factory();
var obj1 = factory.getInstance('Person');
var obj2 = factory.getInstance('Animal');
console.log(obj1.name); // Person1
console.log(obj2.name); // Animal1

3) 代理: 就是新建个类调用老类的接口,包一下
function Person() { }
Person.prototype.sayName = function() { console.log('michaelqin'); }
Person.prototype.sayAge = function() { console.log(30); }

function PersonProxy() { 
  this.person = new Person();
  var that = this;
  this.callMethod = function(functionName) {
    console.log('before proxy:', functionName);
    that.person[functionName](); // 代理
    console.log('after proxy:', functionName);
  }
}

var pp = new PersonProxy();
pp.callMethod('sayName'); // 代理调用Person的方法sayName()
pp.callMethod('sayAge'); // 代理调用Person的方法sayAge()    

4) 观察者: 就是事件模式，比如按钮的onclick这样的应用.
function Publisher() {
  this.listeners = [];
}
Publisher.prototype = {
  'addListener': function(listener) {
    this.listeners.push(listener);
  },

  'removeListener': function(listener) {
    delete this.listeners[listener];
  },

  'notify': function(obj) {
    for(var i = 0; i < this.listeners.length; i++) {
      var listener = this.listeners[i];
      if (typeof listener !== 'undefined') {
        listener.process(obj);
      }
    }
  }
}; // 发布者

function Subscriber() {

}
Subscriber.prototype = {
  'process': function(obj) {
    console.log(obj);
  }
};　// 订阅者


var publisher = new Publisher();
publisher.addListener(new Subscriber());
publisher.addListener(new Subscriber());
publisher.notify({name: 'michaelqin', ageo: 30}); // 发布一个对象到所有订阅者
publisher.notify('2 subscribers will both perform process'); // 发布一个字符串到所有订阅者

```

------

#### 

#### 参考

[^1]: 

