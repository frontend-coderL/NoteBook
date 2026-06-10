# 一、ES5中实现继承

## 1、对象和函数的原型

### 1.1、认识对象的原型

**JavaScript当中每个对象都有一个特殊的内置属性 [[prototype]]，这个特殊的对象可以指向另外一个对象。**

 那么这个对象有什么用呢？

- 当我们通过引用对象的属性key来获取一个value时，它会触发 [[Get]]的操作；
- 这个操作会首先检查该对象是否有对应的属性，如果有的话就使用它；
- 如果对象中没有改属性，那么会访问对象[[prototype]]内置属性指向的对象上的属性；

如果通过字面量直接创建一个对象，这个对象也会有这样的属性吗？如果有，如何获取这个属性？

- 答案是有的，只要是对象都会有这样的一个内置属性；

获取的方式有两种：

- 方式一：通过对象的 __proto__ 属性可以获取到（这是早期浏览器自己添加的，存在一定的兼容性问题）； 
- 方式二：通过 Object.getPrototypeOf 方法可以获取到；

```javascript
var obj = {
  name: "why",
  age: 18,
};
console.log(obj);

var info = {};

// 获取对象的原型
console.log(obj.name, obj.age);
console.log(obj.__proto__); // 浏览器实现，非标准的
console.log(Object.getPrototypeOf(obj)); // 标准方式获取对象的原型
console.log(obj.__proto__ === Object.getPrototypeOf(obj)); // true

// 疑问: 这个原型有什么用呢?
// 当我们通过[[get]]方式获取一个属性对应的value时
// 1> 它会优先在自己的对象中查找, 如果找到直接返回
// 2> 如果没有找到, 那么会在原型对象中查找
console.log(obj.name);

obj.__proto__.message = "Hello World";
console.log(obj.message);
```

### 1.2、函数的原型 prototype

**所有的函数都有一个prototype的属性（注意：不是__ proto __）**

```javascript
var obj = {};
function foo() {}

// 1.将函数看成是一个普通的对象时, 它是具备__proto__(隐式原型)
// 作用: 查找key对应的value时, 会找到原型身上
console.log(obj.__proto__);
console.log(foo.__proto__);

// 2.将函数看成是一个函数时, 它是具备prototype(显式原型)
// 作用: 用来构建对象时, 给对象设置隐式原型的
console.log(foo.prototype);
// console.log(obj.prototype) 对象是没有prototype
```

## 2、new、constructor

### 2.1、new操作符

new关键字的步骤如下：

- 1.在内存中创建一个新的对象（空对象）；
- 2.这个对象内部的[[prototype]]属性会被赋值为该构造函数的prototype属性；

**那么也就意味着我们通过Person构造函数创建出来的所有对象的[[prototype]]属性都指向Person.prototype：**

```javascript
function Foo() {
  // 1.创建空的对象
  // 2.将Foo的prototype原型(显式隐式)赋值给空的对象的__proto__(隐式原型)
}
console.log(Foo.prototype);

var f1 = new Foo();
var f2 = new Foo();
var f3 = new Foo();
var f4 = new Foo();
var f5 = new Foo();
console.log(f1.__proto__);
console.log(f1.__proto__ === Foo.prototype); // true
console.log(f3.__proto__ === f5.__proto__); // true
```

### 2.2、函数的原型的作用

```javascript
/*
  1.什么是函数的显式原型
    * 区分和对象原型区别
  2.函数的原型的作用
    * 在通过new操作创建对象时, 将这个显式原型赋值给创建出来对象的隐式原型
  3.案例Person, 将所有的函数定义放到了显式原型上
*/

function Student(name, age, sno) {
  this.name = name;
  this.age = age;
  this.sno = sno;

  // 1.方式一: 编写函数, 会创建很多个函数对象
  this.running = function () {
    console.log(this.name + " running");
  };
  this.eating = function () {
    console.log(this.name + " eating");
  };
}

// 方式二
// 使用原型的作用：当我们多个对象拥有共同的值时, 我们可以将它放到构造函数对象的显式原型，由构造函数创建出来的所有对象, 都会共享这些属性
Student.prototype.running = function () {
  console.log(this.name + " running");
};
Student.prototype.eating = function () {
  console.log(this.name + " eating");
};

// 1.创建三个学生
var stu1 = new Student("why", 18, 111);
var stu2 = new Student("kobe", 30, 112);
var stu3 = new Student("james", 18, 111);

// 隐式原型的作用
// 1> stu1的隐式原型是谁? Student.prototype对象
// 2> stu1.running查找:
//  * 先在自己身上查找, 没有找到
//  * 去原型去查找
stu1.running();
stu2.eating();
```

### 2.2、创建对象的内存表现

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.running = function () {
  console.log("running~");
};

var p1 = new Person("why", 18);
var p2 = new Person("kobe", 30);

// 进行操作
console.log(p1.name);
console.log(p2.name);

p1.running();
p2.running();

// 新增属性
Person.prototype.address = "中国";
p1.__proto__.info = "中国很美丽!";

p1.height = 1.88;
p2.isAdmin = true;

// 获取属性
console.log(p1.address);
console.log(p2.isAdmin);
console.log(p1.isAdmin);
console.log(p2.info);

// 修改address
p1.address = "广州市";
console.log(p2.address); // 中国
```

![image-20230320205453918](.\img\image-20230320205453918.png)

![image-20230320205859432](.\img\image-20230320205859432.png)

### 2.3、constructor属性

事实上原型对象上面是有一个属性的：constructor

**默认情况下原型上都会添加一个属性叫做constructor，这个constructor指向当前的函数对象；**

```js
// 非常重要的属性: constructor, 指向Person函数对象
function Person() {}

// 1.对constructor在prototype上的验证
var PersonPrototype = Person.prototype;
console.log(PersonPrototype);
console.log(PersonPrototype.constructor);
console.log(PersonPrototype.constructor === Person); // true

console.log(Person.name);
console.log(PersonPrototype.constructor.name);

// 2.实例对象p
var p = new Person();
console.log(p.__proto__.constructor);
console.log(p.__proto__.constructor.name);
```

### 2.4、重写原型对象

*如果我们需要在原型上添加过多的属性，通常我们会重写整个原型对象*

```js
function Person() {}

// 在原有的原型对象上添加新的属性
Person.prototype.message = "Hello Person";
Person.prototype.info = { name: "哈哈哈", age: 30 };
Person.prototype.running = function () {};
Person.prototype.eating = function () {};

console.log(Person.prototype);
console.log(Object.keys(Person.prototype)); // 没有constructor

// 直接赋值一个新的原型对象
Person.prototype = {
  message: "Hello Person",
  info: { name: "哈哈哈", age: 30 },
  running: function () {},
  eating: function () {},
  constructor: Person
};
Object.defineProperty(Person.prototype, "constructor", {
  enumerable: false,
  configurable: true,
  writable: true,
  value: Person,
});

console.log(Object.keys(Person.prototype)); // 有constructor

// 新建实例对象
var p1 = new Person();
console.log(p1.message);
```

前面我们说过, 每创建一个函数, 就会同时创建它的prototype对象, 这个对象也会自动获取constructor属性；

- 而我们这里相当于给prototype重新赋值了一个对象, 那么这个新对象的constructor属性, 会指向Object构造函数, 而不是 Person构造函数了

### 2.5、原型对象的constructor

上面

### 2.6、面向对象的特性 – 继承

> 面向对象有三大特性：封装、继承、多态、（抽象）

- 封装：我们前面将属性和方法封装到一个类中，可以称之为封装的过程；
- 继承：继承是面向对象中非常重要的，不仅仅可以减少重复代码的数量，也是多态前提（纯面向对象中）；
- 多态：不同的对象在执行时表现出不同的形态；

继承是做什么的？

- 继承可以帮助我们将重复的代码和逻辑抽取到父类中，子类只需要直接继承过来使用即可；
- 在很多编程语言中，继承也是多态的前提；

```js
function Student(name, age, sno, score) {
  this.name = name;
  this.age = age;
  this.sno = sno;
  this.score = score;
}

Student.prototype.running = function () {};
Student.prototype.eating = function () {};
Student.prototype.studying = function () {};

function Teacher(name, age, title) {
  this.name = name;
  this.age = age;
  this.title = tilte;
}

Teacher.prototype.running = function () {};
Teacher.prototype.eating = function () {};
Teacher.prototype.teach = function () {};
```



## 3、原型链的查找顺序

### 3.1、JavaScript原型链

在真正实现继承之前，我们先来理解一个非常重要的概念：原型链。

*我们知道，从一个对象上获取属性，如果在当前对象中没有获取到就会去它的原型上面获取：*

```js
// 1.{}的本质
var info = {};
// 相当于
var info = new Object();
console.log(info.__proto__ === Object.prototype);

// 2.原型链
var obj = {
  name: "why",
  age: 18,
};

// 查找顺序
// 1.obj上面查找
// 2.obj.__proto__上面查找
// 3.obj.__proto__.__proto__ -> null 上面查找(undefined)
// console.log(obj.message)

// 3.对现有代码进行改造
obj.__proto__ = {
  // message: "Hello aaa"
};
obj.__proto__.__proto__ = {
  message: "Hello bbbb",
};
obj.__proto__.__proto__.__proto__ = {
  message: "Hello ccc",
};

console.log(obj.message);
```

### 3.2、Object的原型

那么什么地方是原型链的尽头呢？比如第三个对象是否也是有原型__proto__属性呢？

```js
console.log(obj.__proto__.__proto__.__proto__.__proto__); // [Object: null prototype] {}
```

我们会发现它打印的是 [Object: null prototype] {}

- 事实上这个原型就是我们最顶层的原型了
- 从Object直接创建出来的对象的原型都是 [Object: null prototype] {}。

那么我们可能会问题： [Object: null prototype] {} 原型有什么特殊吗？

- 特殊一：该对象有原型属性，但是它的原型属性已经指向的是null，也就是已经是顶层原型了；
- 特殊二：该对象上有很多默认的属性和方法；

### 3.3、Object是所有类的父类

> 从我们上面的Object原型我们可以得出一个结论：原型链最顶层的原型对象就是Object的原型对象

## 4、原型链实现的继承

```js
// 定义Person构造函数(类)
function Person(name, age, height, address) {
  this.name = name;
  this.age = age;
  this.height = height;
  this.address = address;
}

Person.prototype.running = function () {
  console.log("running~");
};
Person.prototype.eating = function () {
  console.log("eating~");
};

// 定义学生类
function Student(name, age, height, address, sno, score) {
  this.name = name;
  this.age = age;
  this.height = height;
  this.address = address;

  this.sno = sno;
  this.score = score;
}

// 方式一: 父类的原型直接赋值给子类的原型
// 缺点: 父类和子类共享通一个原型对象, 修改了任意一个, 另外一个也被修改
// Student.prototype = Person.prototype

// 方式二: 创建一个父类的实例对象(new Person()), 用这个实例对象来作为子类的原型对象
// 缺点：只继承方法，不继承属性
var p = new Person("why", 18);
Student.prototype = p;

Student.prototype.studying = function () {
  console.log("studying~");
};

// 创建学生
var stu1 = new Student("kobe", 30, 111, 100);
var stu2 = new Student("james", 25, 111, 100);
stu1.running();
stu1.studying();

console.log(stu1.name, stu1.age);
console.log(stu1);
console.log(stu2.name, stu2.age);
```

![image-20230320214331278](.\img\image-20230320214331278.png)

**原型链继承的弊端**

- 第一，我们通过直接打印对象是看不到这个属性的；
- 第二，这个属性会被多个对象共享，如果这个对象是一个引用类型，那么就会造成问题；
- 第三，不能给Person传递参数（让每个stu有自己的属性），因为这个对象是一次性创建的（没办法定制化）；

## 5、借用构造函数继承

```js
// 定义Person构造函数(类)
function Person(name, age, height, address) {
  this.name = name;
  this.age = age;
  this.height = height;
  this.address = address;
}
Person.prototype.running = function () {
  console.log("running~");
};
Person.prototype.eating = function () {
  console.log("eating~");
};

// 定义学生类
function Student(name, age, height, address, sno, score) {
  // 重点: 借用构造函数
  Person.call(this, name, age, height, address);

  this.sno = sno;
  this.score = score;
}

var p = new Person("why", 18);
Student.prototype = p;

Student.prototype.studying = function () {
  console.log("studying~");
};

// 创建学生
var stu1 = new Student("kobe", 30, 111, 100);
var stu2 = new Student("james", 25, 111, 100);
stu1.running();
stu1.studying();
```

**为了解决原型链继承中存在的问题，开发人员提供了一种新的技术: constructor stealing(有很多名称: 借用构造函数或者称之 为经典继承或者称之为伪造对象)：**

*steal是偷窃、剽窃的意思，但是这里可以翻译成借用；*

**借用继承的做法非常简单：在子类型构造函数的内部调用父类型构造函数**

- 因为函数可以在任意的时刻被调用；
- 因此通过apply()和call()方法也可以在新创建的对象上执行构造函数；

### 5.1、组合借用继承的问题

组合继承是JavaScript最常用的继承模式之一

组合继承存在什么问题：

- 组合继承最大的问题就是无论在什么情况下，都会调用两次父类构造函数。
  -  一次在创建子类原型的时候；
  - 另一次在子类构造函数内部(也就是每次创建子类实例的时候)；
- 另外，如果你仔细按照我的流程走了上面的每一个步骤，你会发现：所有的子类实例事实上会拥有两份父类的属性
  -  一份在当前的实例自己里面(也就是person本身的)，另一份在子类对应的原型对象中(也就是person.__proto__里面)；
  - 当然，这两份属性我们无需担心访问出现问题，因为默认一定是访问实例本身这一部分的；

### 5.2、原型式继承函数

原型式继承的渊源

- 这种模式要从道格拉斯·克罗克福德（Douglas Crockford，著名的前端大师，JSON的创立者）在2006年写的一篇文章说起: Prototypal Inheritance in JavaScript(在JavaScript中使用原型式继承)
- 在这篇文章中，它介绍了一种继承方法，而且这种继承方法不是通过构造函数来实现的. 
- 为了理解这种方式，我们先再次回顾一下JavaScript想实现继承的目的：重复利用另外一个对象的属性和方法.

**最终的目的：student对象的原型指向了person对象；**

### 5.3、寄生式继承函数

**寄生式(Parasitic)继承**

- 寄生式(Parasitic)继承是与原型式继承紧密相关的一种思想, 并且同样由道格拉斯·克罗克福德(Douglas Crockford)提出和推 广的；
- 寄生式继承的思路是结合原型类继承和工厂模式的一种方式；
- 即创建一个封装继承过程的函数, 该函数在内部以某种方式来增强对象，最后再将这个对象返回；

```js
// 工具函数
// 创建对象的过程
function createObject(o) {
  function F() {}
  F.prototype = o;
  return new F();
}

// 将Subtype和Supertype联系在一起
// 寄生式函数
function inherit(Subtype, Supertype) {
  Subtype.prototype = createObject(Supertype.prototype);
  Object.defineProperty(Subtype.prototype, "constructor", {
    enumerable: false,
    configurable: true,
    writable: true,
    value: Subtype,
  });
}

/*
  满足什么条件:
    1.必须创建出来一个对象
    2.这个对象的隐式原型必须指向父类的显式原型
    3.将这个对象赋值给子类的显式原型
*/
function Person(name, age, height) {}
function Student() {}

inherit(Student, Person);

// 1.之前的做法:
var p = new Person();
Student.prototype = p;

// 2.方案一:
var obj = {};
// obj.__proto__ = Person.prototype
Object.setPrototypeOf(obj, Person.prototype);
Student.prototype = Person.prototype;

// 3.方案二:
function F() {}
F.prototype = Person.prototype;
Student.prototype = new F();

// 4.方案三:
var obj = Object.create(Person.prototype);
console.log(obj.__proto__ === Person.prototype); // true
Student.prototype = obj;
```

## 6、寄生组合实现继承 --- 最终方案

现在我们来回顾一下之前提出的比较理想的组合继承

- 组合继承是比较理想的继承方式, 但是存在两个问题:
  - 问题一: 构造函数会被调用两次: 一次在创建子类型原型对象的时候, 一次在创建子类型实例的时候.
  - 问题二: 父类型中的属性会有两份: 一份在原型对象中, 一份在子类型实例中.

事实上, 我们现在可以利用寄生式继承将这两个问题给解决掉. 

- 你需要先明确一点: 当我们在子类型的构造函数中调用父类型.call(this, 参数)这个函数的时候, 就会将父类型中的属性和方法复 制一份到了子类型中. 所以父类型本身里面的内容, 我们不再需要.
- 这个时候, 我们还需要获取到一份父类型的原型对象中的属性和方法.
- 能不能直接让子类型的原型对象 = 父类型的原型对象呢?
- 不要这么做, 因为这么做意味着以后修改了子类型原型对象的某个引用类型的时候, 父类型原生对象的引用类型也会被修改.
- 我们使用前面的寄生式思想就可以了.

```js
// inherit_utils.js
// 创建对象的过程
function createObject(o) {
  function F() {}
  F.prototype = o
  return new F()
}

// 将Subtype和Supertype联系在一起
// 寄生式函数
function inherit(Subtype, Supertype) {
  Subtype.prototype = createObject(Supertype.prototype)
  Object.defineProperty(Subtype.prototype, "constructor", {
    enumerable: false,
    configurable: true,
    writable: true,
    value: Subtype
  })
  Object.setPrototypeOf(Subtype, Supertype)
}

```

```js
// 寄生组合式继承
// 原型链/借用/原型式(对象之间)/寄生式函数
function Person(name, age, height) {
  this.name = name;
  this.age = age;
  this.height = height;
}

Person.prototype.running = function () {
  console.log("running~");
};
Person.prototype.eating = function () {
  console.log("eating~");
};

function Student(name, age, height, sno, score) {
  Person.call(this, name, age, height);
  this.sno = sno;
  this.score = score;
}

inherit(Student, Person);
Student.prototype.studying = function () {
  console.log("studying");
};

// 创建实例对象
var stu1 = new Student("why", 18, 1.88, 111, 100);
```

## 7、ES5对象的方法补充

- hasOwnProperty
  - 对象是否有某一个属于自己的属性（不是在原型上的属性）
- in/for in 操作符
  - 判断某个属性是否在某个对象或者对象的原型上
- instanceof
  - 用于检测构造函数（Person、Student类）的pototype，是否出现在某个实例对象的原型链上
- isPrototypeOf
  - 用于检测某个对象，是否出现在某个实例对象的原型链上

```js
var obj = {
  name: "why",
  age: 18,
};

var info = createObject(obj);
info.address = "中国";
info.intro = "中国大好河山";

console.log(info.name, info.address);
console.log(info);

// 1.hasOwnProperty
console.log(info.hasOwnProperty("name")); // false
console.log(info.hasOwnProperty("address")); // true

// 2.in操作符
console.log("name" in info); // true
console.log("address" in info); // true
// 注意: for in遍历不仅仅是自己对象上的内容, 也包括原型对象上的内容
for (var key in info) {
  console.log(key);
}

// 3.instanceof
// instanceof用于判断对象和类(构造函数)之间的关系
function Person() {}
function Student() {}
inherit(Student, Person);

// stu实例(instance)对象
var stu = new Student();
console.log(stu instanceof Student); // true
console.log(stu instanceof Person); // true
console.log(stu instanceof Object); // true
console.log(stu instanceof Array); // false

// 4.isPrototypeOf
console.log(Student.prototype.isPrototypeOf(stu)); // true
console.log(Person.prototype.isPrototypeOf(stu)); // true

// 可以用于判断对象之间的继承
console.log(obj.isPrototypeOf(info));
```

## 8、原型继承关系图

![](.\img\image-20230320223738148.png)



# 二、ES6中实现继承

## 1、class方式定义类

### 1.1、认识class定义类

我们会发现，按照前面的构造函数形式创建 类，不仅仅和编写普通的函数过于相似，而且代码并不容易理解。

- 在ES6（ECMAScript2015）新的标准中使用了class关键字来直接定义类；
- 但是类本质上依然是前面所讲的构造函数、原型链的语法糖而已；
- 所以学好了前面的构造函数、原型链更有利于我们理解类的概念和继承关系；

那么，如何使用class来定义一个类呢？

- 可以使用两种方式来声明类：类声明和类表达式；

```js
// ES5中定义类
function Person() {}

// ES6定义类
// {key: value} -> 对象
// {表达式} -> 代码块
// {} -> 类的结构
class Person {}

// 创建实例对象
var p1 = new Person();
var p2 = new Person();
console.log(p1, p2);

// 另外一种定义方法: 表达式写法(了解, 少用)
var Student = class {};
var foo = function () {};

var stu1 = new Student();
console.log(stu1);

// 编程: 高内聚低耦合
class Person {
  // 1.类中的构造函数
  // 当我们通过new关键字调用一个Person类时, 默认调用class中的constructor方法
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  // 2.实例方法
  // 本质上是放在Person.prototype
  running() {
    console.log(this.name + " running~");
  }
  eating() {
    console.log(this.name + " eating~");
  }
}

// 创建实例对象
var p1 = new Person("why", 18);

// 使用实例对象中属性和方法
console.log(p1.name, p1.age);
p1.running();
p1.eating();

// 研究内容
console.log(Person.prototype === p1.__proto__);
console.log(Person.running);
console.log(Person.prototype.running);
```

### 1.2、类和构造函数的异同

我们来研究一下类的一些特性：你会发现它和我们的构造函数的特性其实是一致的；

```js
// function定义类
function Person1(name, age) {
  this.name = name;
  this.age = age;
}

Person1.prototype.running = function () {};
Person1.prototype.eating = function () {};

var p1 = new Person1("why", 18);
console.log(p1.__proto__ === Person1.prototype);
console.log(Person1.prototype.constructor);
console.log(typeof Person1); // function

// 不同点: 作为普通函数去调用
Person1("abc", 100);

// class定义类
class Person2 {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  running() {}
  eating() {}
}

var p2 = new Person2("kobe", 30);
console.log(p2.__proto__ === Person2.prototype);
console.log(Person2.prototype.constructor);
console.log(typeof Person2);

// 不同点: class定义的类, 不能作为一个普通的函数进行调用
Person2("cba", 0);
```

### 1.3、类的构造函数

如果我们希望在创建对象的时候给类传递一些参数，这个时候应该如何做呢？

- 每个类都可以有一个自己的构造函数（方法），这个方法的名称是固定的constructor；
- 当我们通过new操作符，操作一个类的时候会调用这个类的构造函数constructor；
- 每个类只能有一个构造函数，如果包含多个构造函数，那么会抛出异常；

当我们通过new关键字操作类的时候，会调用这个constructor函数，并且执行如下操作：

- 1.在内存中创建一个新的对象（空对象）；
- 2.这个对象内部的[[prototype]]属性会被赋值为该类的prototype属性；
- 3.构造函数内部的this，会指向创建出来的新对象；
- 4.执行构造函数的内部代码（函数体代码）；
- 5.如果构造函数没有返回非空对象，则返回创建出来的新对象；

### 1.4、类的实例方法

在上面我们定义的属性都是直接放到了this上，也就意味着它是放到了创建出来的新对象中：

- 在前面我们说过对于实例的方法，我们是希望放到原型上的，这样可以被多个实例来共享；
- 这个时候我们可以直接在类中定义；

### 1.5、类的访问器方法

我们之前讲对象的属性描述符时有讲过对象可以添加setter和getter函数的，那么类也是可以的：

```js
// 针对对象
// 方式一: 描述符
var obj = {
  _name: "why",
};
Object.defineProperty(obj, "name", {
  configurable: true,
  enumerable: true,
  set: function () {},
  get: function () {},
});

// 方式二: 直接在对象定义访问器
// 监听_name什么时候被访问, 什么设置新的值
var obj = {
  _name: "why",
  // setter方法
  set name(value) {
    this._name = value;
  },
  // getter方法
  get name() {
    return this._name;
  },
};

obj.name = "kobe";
console.log(obj.name);
```

```js
// 1.访问器的编写方式
class Person {
  // 程序员之间的约定: 以_开头的属性和方法, 是不在外界访问
  constructor(name, age) {
    this._name = name;
  }

  set name(value) {
    console.log("设置name");
    this._name = value;
  }

  get name() {
    console.log("获取name");
    return this._name;
  }
}

var p1 = new Person("why", 18);
p1.name = "kobe";
console.log(p1.name);
// console.log(p1._name)

var p2 = new Person("james", 25);
console.log(p2.name);

// 2.访问器的应用场景
class Rectangle {
  constructor(x, y, width, height) {
    this.x = x;
    this.y = y;
    this.width = width;
    this.height = height;
  }

  get position() {
    return { x: this.x, y: this.y };
  }

  get size() {
    return { width: this.width, height: this.height };
  }
}

var rect1 = new Rectangle(10, 20, 100, 200);
console.log(rect1.position);
console.log(rect1.size);
```

### 1.6、类的静态方法

静态方法通常用于定义直接使用类来执行的方法，不需要有类的实例，使用static关键字来定义：

```js
function Person() {}
// 实例方法
Person.prototype.running = function () {};
// 类方法
Person.randomPerson = function () {};

var p1 = new Person();
p1.running();
Person.randomPerson();

// class定义的类
var names = ["abc", "cba", "nba", "mba"];
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  // 实例方法
  running() {
    console.log(this.name + " running~");
  }
  eating() {}

  // 类方法(静态方法)
  static randomPerson() {
    console.log(this);
    var randomName = names[Math.floor(Math.random() * names.length)];
    return new this(randomName, Math.floor(Math.random() * 100));
  }
}

var p1 = new Person();
p1.running();
p1.eating();
var randomPerson = Person.randomPerson();
console.log(randomPerson);
```

## 2、extends实现继承

### 2.1、ES6类的继承 - extends

前面我们花了很大的篇幅讨论了在ES5中实现继承的方案，虽然最终实现了相对满意的继承机制，但是过程却依然是非常繁琐的。

在ES6中新增了使用extends关键字，可以方便的帮助我们实现继承：

```js
// 定义父类
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  running() {
    console.log("running~");
  }
  eating() {
    console.log("eating~");
  }
}

class Student extends Person {
  constructor(name, age, sno, score) {
    super(name, age);
    this.sno = sno;
    this.score = score;
  }

  studying() {
    console.log("studying~");
  }
}

var stu1 = new Student("why", 18, 111, 100);
stu1.running();
stu1.eating();
stu1.studying();

class Teacher extends Person {
  constructor(name, age, title) {
    super(name, age);
    this.title = title;
  }

  teaching() {
    console.log("teaching~");
  }
}
```

### 2.2、super关键字

我们会发现在上面的代码中我使用了一个super关键字，这个super关键字有不同的使用方式：

- 注意：在子（派生）类的构造函数中使用this或者返回默认对象之前，必须先通过super调用父类的构造函数！
- super的使用位置有三个：子类的构造函数、实例方法、静态方法；

```js
class Animal {
  running() {
    console.log("running");
  }
  eating() {
    console.log("eating");
  }

  static sleep() {
    console.log("static animal sleep");
  }
}

class Dog extends Animal {
  // 子类如果对于父类的方法实现不满足(继承过来的方法)
  // 重新实现称之为重写(父类方法的重写)
  running() {
    console.log("dog四条腿");
    // 调用父类的方法
    super.running();
    // console.log("running~")
    // console.log("dog四条腿running~")
  }

  static sleep() {
    console.log("趴着");
    super.sleep();
  }
}

var dog = new Dog();
dog.running();
dog.eating();
Dog.sleep();
```

### 2.3、继承内置类

我们也可以让我们的类继承自内置类，比如Array：

 ```js
 // 1.创建一个新的类, 继承自Array进行扩展
 class HYArray extends Array {
   get lastItem() {
     return this[this.length - 1];
   }
 
   get firstItem() {
     return this[0];
   }
 }
 
 var arr = new HYArray(10, 20, 30);
 console.log(arr);
 console.log(arr.length);
 console.log(arr[0]);
 console.log(arr.lastItem);
 console.log(arr.firstItem);
 
 // 2.直接对Array进行扩展
 Array.prototype.lastItem = function () {
   return this[this.length - 1];
 };
 
 var arr = new Array(10, 20, 30);
 console.log(arr.__proto__ === Array.prototype);
 console.log(arr.lastItem());
 ```

### 2.4、类的混入mixin

```js
// JavaScript只支持单继承(不支持多继承)
function mixinAnimal(BaseClass) {
  return class extends BaseClass {
    running() {
      console.log("running~");
    }
  };
}

function mixinRunner(BaseClass) {
  return class extends BaseClass {
    flying() {
      console.log("flying~");
    }
  };
}

class Bird {
  eating() {
    console.log("eating~");
  }
}

// var NewBird = mixinRunner(mixinAnimal(Bird))
class NewBird extends mixinRunner(mixinAnimal(Bird)) {}
var bird = new NewBird();
bird.flying();
bird.running();
bird.eating();
```

## 3、Babel的ES6转ES5

> 需回顾

## 4、面向对象多态的理解

面向对象的三大特性：封装、继承、多态。

- 前面两个我们都已经详细解析过了，接下来我们讨论一下JavaScript的多态。

JavaScript有多态吗？

- 维基百科对多态的定义：多态（英语：polymorphism）指为不同数据类型的实体提供统一的接口，或使用一个单一的符号 来表示多个不同的类型。
- 非常的抽象，个人的总结：不同的数据类型进行同一个操作，表现出不同的行为，就是多态的体现。

那么从上面的定义来看，JavaScript是一定存在多态的。



## 5、ES6对象的增强

### 5.1、字面量的增强

### 5.2、方法的增强

### 5.3、计算属性名的写法

```js
var name = "why";
var age = 18;

var key = "address" + " city";

var obj = {
  // 1.属性的增强
  name,
  age,

  // 2.方法的增强
  running: function () {
    console.log(this);
  },
  swimming() {
    console.log(this);
  },
  eating: () => {
    console.log(this);
  },

  // 3.计算属性名
  [key]: "广州",
};

obj.running();
obj.swimming();
obj.eating();

function foo() {
  var message = "Hello World";
  var info = "my name is why";
  return { message, info };
}

var result = foo();
console.log(result.message, result.info);
```

## 6、解构Destructuring

ES6中新增了一个从数组或对象中方便获取数据的方法，称之为解构Destructuring

- 解构赋值是一种特殊的语法，它使我们可以将数组或对象“拆包”至一系列变量中

分为：

- 数组的解构
- 对象的解构

```js
var names = ["abc", "cba", undefined, "nba", "mba"];

// 1.数组的解构
var name1 = names[0];
var name2 = names[1];
var name3 = names[2];
// 1.1. 基本使用
var [name1, name2, name3] = names;
console.log(name1, name2, name3);

// 1.2. 顺序问题: 严格的顺序
var [name1, , name3] = names;
console.log(name1, name3);

// 1.3. 解构出数组
var [name1, name2, ...newNames] = names;
console.log(name1, name2, newNames);

// 1.4. 解构的默认值
var [name1, name2, name3 = "default"] = names;
console.log(name1, name2, name3);

// 2.对象的解构
var obj = { name: "why", age: 18, height: 1.88 };
var name = obj.name;
var age = obj.age;
var height = obj.height;
// 2.1. 基本使用
var { name, age, height } = obj;
console.log(name, age, height);

// 2.2. 顺序问题: 对象的解构是没有顺序, 根据key解构
var { height, name, age } = obj;
console.log(name, age, height);

// 2.3. 对变量进行重命名
var { height: wHeight, name: wName, age: wAge } = obj;
console.log(wName, wAge, wHeight);

// 2.4. 默认值
var {
  height: wHeight,
  name: wName,
  age: wAge,
  address: wAddress = "中国",
} = obj;
console.log(wName, wAge, wHeight, wAddress);

// 2.5. 对象的剩余内容
var { name, age, ...newObj } = obj;
console.log(newObj);

// 应用: 在函数中(其他类似的地方)
function getPosition({ x, y }) {
  console.log(x, y);
}
getPosition({ x: 10, y: 20 });
getPosition({ x: 25, y: 35 });
function foo(num) {}
foo(123);
```

## 7、手写apply、call、bind函数

### 7.1、手写apply、call

```js
function foo(name, age) {
  console.log(this, name, age);
}

// foo函数可以通过apply/call
// foo.apply("aaa", ["why", 18])
// foo.call("bbb", "kobe", 30)

// 1.给函数对象添加方法: hyapply
Function.prototype.hyapply = function (thisArg, otherArgs) {
  // this -> 调用的函数对象
  // thisArg -> 传入的第一个参数, 要绑定的this
  // console.log(this) // -> 当前调用的函数对象
  // this.apply(thisArg)
  thisArg.fn = this;

  // 1.获取thisArg, 并且确保是一个对象类型
  thisArg =
    thisArg === null || thisArg === undefined ? window : Object(thisArg);

  // thisArg.fn = this
  Object.defineProperty(thisArg, "fn", {
    enumerable: false,
    configurable: true,
    value: this,
  });
  thisArg.fn(...otherArgs);

  delete thisArg.fn;
};

// foo.hyapply({ name: "why" }, ["james", 25])
// foo.hyapply(123, ["why", 18])
// foo.hyapply(null, ["kobe", 30])

// 2.给函数对象添加方法: hycall
Function.prototype.hycall = function (thisArg, ...otherArgs) {
  // 1.获取thisArg, 并且确保是一个对象类型
  thisArg =
    thisArg === null || thisArg === undefined ? window : Object(thisArg);

  // thisArg.fn = this
  Object.defineProperty(thisArg, "fn", {
    enumerable: false,
    configurable: true,
    value: this,
  });
  thisArg.fn(...otherArgs);

  delete thisArg.fn;
};

foo.hycall({ name: "why", fn: "abc" }, "james", 25);
foo.hycall(123, "why", 18);
foo.hycall(null, "kobe", 30);
```

```js
function foo(name, age) {
  console.log(this, name, age);
}

// 1.封装思想
// 1.1.封装到独立的函数中
function execFn(thisArg, otherArgs, fn) {
  // 1.获取thisArg, 并且确保是一个对象类型
  thisArg =
    thisArg === null || thisArg === undefined ? window : Object(thisArg);

  Object.defineProperty(thisArg, "fn", {
    enumerable: false,
    configurable: true,
    value: fn,
  });

  // 执行代码
  thisArg.fn(...otherArgs);

  delete thisArg.fn;
}

// 1.2. 封装原型中
Function.prototype.hyexec = function (thisArg, otherArgs) {
  // 1.获取thisArg, 并且确保是一个对象类型
  thisArg =
    thisArg === null || thisArg === undefined ? window : Object(thisArg);

  Object.defineProperty(thisArg, "fn", {
    enumerable: false,
    configurable: true,
    value: this,
  });
  thisArg.fn(...otherArgs);

  delete thisArg.fn;
};

// 1.给函数对象添加方法: hyapply
Function.prototype.hyapply = function (thisArg, otherArgs) {
  this.hyexec(thisArg, otherArgs);
};
// 2.给函数对象添加方法: hycall
Function.prototype.hycall = function (thisArg, ...otherArgs) {
  this.hyexec(thisArg, otherArgs);
};

foo.hyapply({ name: "why" }, ["james", 25]);
foo.hyapply(123, ["why", 18]);
foo.hyapply(null, ["kobe", 30]);

foo.hycall({ name: "why" }, "james", 25);
foo.hycall(123, "why", 18);
foo.hycall(null, "kobe", 30);
```

### 7.2、bind

```js
// apply/call
function foo(name, age, height, address) {
  console.log(this, name, age, height, address);
}

// Function.prototype
// var newFoo = foo.bind({ name: "why" }, "why", 18)
// newFoo(1.88)

// 实现hybind函数
Function.prototype.hybind = function (thisArg, ...otherArgs) {
  // console.log(this) // -> foo函数对象
  thisArg =
    thisArg === null || thisArg === undefined ? window : Object(thisArg);
  Object.defineProperty(thisArg, "fn", {
    enumerable: false,
    configurable: true,
    writable: false,
    value: this,
  });

  return (...newArgs) => {
    // var allArgs = otherArgs.concat(newArgs)
    var allArgs = [...otherArgs, ...newArgs];
    thisArg.fn(...allArgs);
  };
};

var newFoo = foo.hybind("abc", "kobe", 30);
newFoo(1.88, "广州市");
newFoo(1.88, "广州市");
newFoo(1.88, "广州市");
newFoo(1.88, "广州市");
```









