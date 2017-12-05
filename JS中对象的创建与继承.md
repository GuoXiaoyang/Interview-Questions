## JS中对象的创建与继承总结

其实反反复复看了好几次《JS高程》，每次看的时候也感觉懂了。但是回过头还是很容易忘。所以干脆做一个总结。

### 对象创建

* 直接创建单个对象

  ```javascript
  var person = new Object();
  person.name = "John";
  //对象字面量语法，直观
  var person = {
    name: "John"
  }
  ```

  简单但只适用于单个对象创建，同类相似对象创建重复代码多

* 工厂模式

  ```javascript
  function createPerson(name, age, job){ 
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){ 
      alert(this.name); 
    }; 
    return o;
  }
  var person1 = createPerson("Nicholas", 29, "Software Engineer"); 
  var person2 = createPerson("Greg", 27, "Doctor");
  ```

  工厂模式虽然解决了创建 多个相似对象的问题，但却没有解决对象识别的问题（即怎样知道一个对象的类型）。

* 构造函数模式

  ```javascript
  function Person(name, age, job){ 
    this.name = name;
    this.age = age; 
    this.job = job; 		
    this.sayName = function(){ 
      console.log(this.name); 
    }; 
  }

  var person1 = new Person("Nicholas", 29, "Software Engineer"); 
  var person2 = new Person("Greg", 27, "Doctor");
  ```

  要创建 `Person` 的新实例，必须使用 `new `操作符。以这种方式调用构造函数实际上会经历以下 4 个步骤：

  (1) 创建一个新对象；

  (2) 将构造函数的作用域赋给新对象（因此 `this `就指向了这个新对象）；

  (3) 执行构造函数中的代码（为这个新对象添加属性）；

  (4) 返回新对象。

  构造函数能够解决对象识别的问题，但其生成对象的方法却是各自独立不等的，而我们的需求是不同对象拥有各自独立的变量和共享同一个方法，

* 原型模式

  ```javascript
  function Person(){ }
  Person.prototype = { 
    constructor : Person, //直接赋值对象字面量，constructor属性会重写到Object
    name : "Nicholas", 
    age : 29, 
    job: "Software Engineer", 
    sayName : function () { console.log(this.name); } 
  };
  var person1 = new Person(); 
  person1.sayName(); //"Nicholas"
  var person2 = new Person();
  ```

  根据JS的原型链，对象如果没有查到指定属性，会在原型对象上查找属性，因此原型模式创建的对象都共享同样的变量和方法。

  但由于引用型数据类型浅拷贝的特性，导致一旦某个实例直接修改了引用型变量，原型对象上的该属性也会改变。

* 组合模式

  ```javascript
  function Person(name, age, job){ 
    this.name = name; 
    this.age = age; 
    this.job = job; 
    this.friends = ["Shelby", "Court"]; 
  }
  Person.prototype = { 
    constructor : Person, 
    sayName : function(){ console.log(this.name); } 
  }
  var person1 = new Person("Nicholas", 29, "Software Engineer"); 
  var person2 = new Person("Greg", 27, "Doctor");
  person1.friends.push("Van"); 
  console.log(person1.friends); //"Shelby,Count,Van" 
  console.log(person2.friends); //"Shelby,Count" 
  console.log(person1.friends === person2.friends); //false 
  console.log(person1.sayName === person2.sayName);//true
  ```

  这种构造函数与原型混成的模式，是目前在 ECMAScript 中使用最广泛、认同度最高的一种创建自 定义类型的方法。

* 动态原型模式

  ```javascript
  function Person(name, age, job){
    //属性 
    this.name = name; 
    this.age = age; 
    this.job = job;
    //方法 
    if (typeof this.sayName != "function"){
  	Person.prototype.sayName = function(){ 
        console.log(this.name); 
      };
    }
  }

  var friend = new Person("Nicholas", 29, "Software Engineer");
  friend.sayName();
  ```

  该方法将原型封装到构造函数中，仅在第一次创建对象时为原型添加方法属性。

  ​

* 寄生构造函数模式

  ```javascript
  function Person(name, age, job){ 
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){ 
      console.log(this.name); 
    }; 
    return o;
  }
  var friend = new Person("Nicholas", 29, "Software Engineer");
  friend.sayName(); //"Nicholas"
  ```

  除了使用 `new `操作符并把使用的包装函数叫做构造函数之外，这个模式跟工厂模式其实 是一模一样的。构造函数在不返回值的情况下，默认会返回新对象实例。而通过在构造函数的末尾添加一个 `return` 语句，可以重写调用构造函数时返回的对象。

  寄生构造函数模式中，返回的对象与构造函数或者与构造函数的原型属性之间没有关系；也就是说，不能依赖 `instanceof` 操作符来确定对象类型。由于存在上述问题，我们建议在可以使用其他模式的情 况下，不要使用这种模式。

  ​

* 稳妥构造函数模式

  所谓稳妥对象，指的是没有公共属性，而且其方法也不引用` this `的对象。稳妥对象最适合在 一些安全的环境中（这些环境中会禁止使用 `this `和` new`），或者在防止数据被其他应用程序改动时使用。稳妥构造函数遵循与寄生构造函数类似的模式，但有两点不同：一是新创建对象的 实例方法不引用 `this`；二是不使用` new `操作符调用构造函数。按照稳妥构造函数的要求，可以将前面 的 Person 构造函数重写如下。

  ```javascript
  function Person(name, age, job){
    //创建要返回的对象 
    var o = new Object();
    //可以在这里定义私有变量和函数
    //添加方法 
    o.sayName = function(){ console.log(name); };
    //返回对象 
    return o;
  }

  ```

  注意，在以这种模式创建的对象中，除了使用 `sayName()`方法之外，没有其他办法访问 `name `的值。 可以像下面使用稳妥的 Person 构造函数。

  ```javascript
  var friend = Person("Nicholas", 29, "Software Engineer"); 
  friend.sayName(); //"Nicholas"
  ```



### 继承

* 原型链继承

  ```javascript
  function SuperType() {
    this.property = true;
  }
  SuperType.prototype.getSuperValue = function(){ 
    return this.property; 
  };

  function SubType(){ 
    this.subproperty = false; 
  }
  //继承了 SuperType 
  SubType.prototype = new SuperType();
  SubType.prototype.getSubValue = function (){ return this.subproperty; };

  var instance = new SubType(); console.log(instance.getSuperValue());

  //true
  ```

  原型链继承能够让子类共享父类原型上的属性，但由于包含引用类型值的原型属性会被所有实例共享，一旦某个实例修改父类的引用类型属性时，所有实例都会共享该变化。

* 构造函数继承

  ```javascript
  function SuperType(name) {
    this.name = name;
  }
  function SubType(name, age) {
    SuperType.call(this);
    this.age = age;
  }
  ```

  同理，通过`call`调用，每个子类的实例都创建自己的属性；但对于实例方法不适用。

* 组合继承

  ```javascript
  function SuperType(name){ 
    this.name = name; 
    this.colors = ["red", "blue", "green"];
  }

  SuperType.prototype.sayName = function(){ 
    console.log(this.name);
  };

  function SubType(name, age){
  //继承属性 
    SuperType.call(this, name);
    this.age = age;
  }

  //继承方法 
  SubType.prototype = new SuperType(); SubType.prototype.constructor = SubType; SubType.prototype.sayAge = function(){ 
    console.log(this.age); 
  };

  var instance1 = new SubType("Nicholas", 29); 
  instance1.colors.push("black"); 
  console.log(instance1.colors); //"red,blue,green,black" 
  instance1.sayName(); //"Nicholas"; 
  instance1.sayAge(); //29

  var instance2 = new SubType("Greg", 27); 
  console.log(instance2.colors); //"red,blue,green" 
  instance2.sayName(); //"Greg"; 
  instance2.sayAge(); //27
  ```

  组合继承中子类对象本身由构造函数生成了属性变量，同时又通过原型继承在原型链上获取属性变量，造成了一定的浪费。

* 原型式继承

  ```javascript
  function object(o){ 
    function F(){} 
    F.prototype = o; 
    return new F(); 
  }
  ```

  该方法要求已知对象的存在，在 `object()`函数内部，先创建了一个临时性的构造函数，然后将传入的对象作为这个构造函数的 原型，最后返回了这个临时类型的一个新实例。从本质上讲，`object()`对传入其中的对象执行了一次浅拷贝。

  ECMAScript 5 通过新` Object.create()`方法规范化了原型式继承。

* 寄生式继承

  寄生式（parasitic）继承是与原型式继承紧密相关的一种思路，寄生构造函数和工厂模式类似，即创建一个仅用于封装继承过程的函数，该 函数在内部以某种方式来增强对象，最后再像真地是它做了所有工作一样返回对象。原型式继承和寄生式继承都是针对已有实例对象的继承。

  ```javascript
  function createAnother(original){ 
    var clone = object(original); 
    clone.sayHi = function(){ console.log("hi"); }; 
    return clone; 
  }
  ```

  ​

* 寄生组合式继承

  组合继承是 JavaScript 最常用的继承模式；不过，它也有自己的不足。组合继承最大的问题就是无论什么情况下，都会调用两次超类型构造函数：一次是在创建子类型原型的时候，另一次是 在子类型构造函数内部。没错，子类型最终会包含超类型对象的全部实例属性，但我们不得不在调用子 类型构造函数时重写这些属性。再来看一看下面组合继承的例子。

  ```javascript
  function SuperType(name){ 
    this.name = name; 
    this.colors = ["red", "blue", "green"]; 
  }

  SuperType.prototype.sayName = function(){ 
    console.log(this.name); 
  };

  function SubType(name, age){ 
    SuperType.call(this, name);
    //第二次调用 SuperType()
    this.age = age;
  }

  SubType.prototype = new SuperType(); 
  SubType.prototype.constructor = SubType; 
  SubType.prototype.sayAge = function(){ console.log(this.age); };

  ```

  第一次调用 `SuperType `构造函数时， `SubType.prototype `会得到两个属性：`name `和 `colors`；它们都是 `SuperType` 的实例属性，只不过现在位于` SubType` 的原型中。当调用 `SubType` 构造函数时，又会调用一次 `SuperType` 构造函数，这 一次又在新对象上创建了实例属性 `name` 和 `colors`。于是，这两个属性就屏蔽了原型中的两个同名属 性。于是对于`SubType`的实例而言，有两组 `name `和 `colors `属性：一组在实例上，一组在 `SubType `原型中。这就是调 用两次 `SuperType `构造函数的结果。

  所谓寄生组合式继承，即通过借用构造函数来继承属性，通过原型链的混成形式来继承方法。其背后的基本思路是：不必为了指定子类型的原型而调用超类型的构造函数，我们所需要的无非就是超类型原型的一个副本而已。本质上，就是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型 的原型。寄生组合式继承的基本模式如下所示。

  ```javascript
  function inheritPrototype(subType, superType){ 
    var prototype = object(superType.prototype); //创建对象
    prototype.constructor = subType; //增强对象
    subType.prototype = prototype; //指定对象
  }
  ```

  这个示例中的 `inheritPrototype()`函数实现了寄生组合式继承的最简单形式。这个函数接收两 个参数：子类型构造函数和超类型构造函数。在函数内部，第一步是创建超类型原型的一个副本。第二步是为创建的副本添加 `constructor` 属性，从而弥补因重写原型而失去的默认的 `constructor `属性。 最后一步， 将新创建的对象（即副本）赋值给子类型的原型。 这样， 我们就可以用调用 `inheritPrototype()`函数的语句，去替换前面例子中为子类型原型赋值的语句了，例如：

  ```javascript
  function SuperType(name){ 
    this.name = name; 
    this.colors = ["red", "blue", "green"]; 
  }

  SuperType.prototype.sayName = function(){ 
    console.log(this.name); 
  };

  function SubType(name, age){ 
    SuperType.call(this, name);
    this.age = age;
  }
  inheritPrototype(SubType, SuperType);
  SubType.prototype.sayAge = function(){ console.log(this.age); };	
  ```

  这个例子的高效率体现在它只调用了一次 `SuperType` 构造函数，并且因此避免了在` SubType. prototype `上面创建不必要的、多余的属性。与此同时，原型链还能保持不变；因此，还能够正常使用 `instanceof `和 `isPrototypeOf()`。开发人员普遍认为寄生组合式继承是引用类型最理想的继承范式。



小结：个人感觉，JS中面向对象程序设计基本就是原型链和`this`指向的揉合，掌握这两者就能慢慢领悟了。