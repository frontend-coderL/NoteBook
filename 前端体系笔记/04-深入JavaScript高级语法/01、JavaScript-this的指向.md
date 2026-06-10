## 1、this的绑定规则

*一个让人困惑的问题：定义一个函数，我们采用三种不同的方式对它进行调用，它产生了三种不同的结果*

```javascript
// 定义函数
function foo(name) {
console.log("foo函数:", this);
}

// 1.方式一: 直接调用
foo(); // window

// 2.方式二: 通过对象调起
var obj = { name: "why" };
obj.aaa = foo;
obj.aaa(); // obj

// 方式三：通过call/apply调用
foo.call("abc"); // String {'abc'}对象
```

这个的案例可以给我们什么样的启示呢？

- 1、函数在调用时，JavaScript会默认给this绑定一个值；
- 2、this的绑定和定义的位置（编写的位置）没有关系；
- 3、this的绑定和调用方式以及调用的位置有关系；
- 4、this是在运行时被绑定的；

 那么this到底是怎么样的绑定规则呢？

- 绑定一：默认绑定；
- 绑定二：隐式绑定；
- 绑定三：显示绑定；
- 绑定四：new绑定；

### 1.1、默认绑定

```javascript
// "use strict"

// 定义函数
// 1.普通的函数被独立的调用
function foo() {
    console.log("foo:", this);
}
foo();

// 2.函数定义在对象中, 但是独立调用
var obj = {
    name: "why",
    bar: function () {
        console.log("bar:", this);
    },
};
var baz = obj.bar;
baz();

// 3.高阶函数
function test(fn) {
    fn();
}
test(obj.bar);

// 4.严格模式下, 独立调用的函数中的this指向的是undefined
```

### 1.2、隐式绑定

```javascript
// 隐式绑定
function foo() {
  console.log("foo函数:", this);
}

var obj = {
  bar: foo,
};
obj.bar(); // obj
```

### 1.3、显示绑定

**隐式绑定有一个前提条件：必须在调用的对象内部有一个对函数的引用（比如一个属性）。**

*如果没有这样的引用，在进行调用时，会报找不到该函数的错误，正是通过这个引用，间接的将this绑定到了这个对象上。*

如果我们不希望在 对象内部 包含这个函数的引用，同时又希望在这个对象上进行强制调用，该怎么做呢？

JavaScript所有的函数都可以使用call和apply方法。

- 第一个参数是相同的，要求传入一个对象；
  - 这个对象的作用是什么呢？就是给this准备的。
  - 在调用这个函数时，会将this绑定到这个传入的对象上。
- 后面的参数，apply为数组，call为参数列表；

因为上面的过程，我们明确的绑定了this指向的对象，所以称之为 显式绑定。

```javascript
// 显式绑定
var obj = {
  name: "why",
};

function foo() {
  console.log("foo函数:", this);
}

// 执行函数, 并且函数中的this指向obj对象
obj.foo = foo
obj.foo()

// 执行函数, 并且强制this就是obj对象
foo.call(obj);
foo.call(123); // 创建一个包装类对象进行绑定
foo.call("abc"); // 创建一个包装类对象进行绑定
```

### 1.4、new绑定

```javascript
/*
  1.创建新的空对象
  2.将this指向这个空对象
  3.执行函数体中的代码
  4.没有显示返回非空对象时, 默认返回这个对象
*/
function foo() {
  this.name = "why";
  console.log("foo函数:", this);
}
new foo();
```

## 2、apply、call、bind

### 2.1、apply、call

通过call或者apply绑定this对象 ，显示绑定后，this就会明确的指向绑定的对象

```javascript
// call/apply
function foo(name, age, height) {
  console.log("foo函数被调用:", this);
  console.log("打印参数:", name, age, height);
}

// ()调用
foo("why", 18, 1.88);

// apply
// 第一个参数: 绑定this
// 第二个参数: 传入额外的实参, 以数组的形式
foo.apply("apply", ["kobe", 30, 1.98]);

// call
// 第一个参数: 绑定this
// 参数列表: 后续的参数以多参数的形式传递, 会作为实参
foo.call("call", "james", 25, 2.05);
```

### 2.2、bind

 如果我们希望一个函数总是显示的绑定到一个对象上，可以怎么做呢？

- 使用bind方法，bind() 方法创建一个新的绑定函数（bound function，BF）；
- 绑定函数是一个 exotic function object（怪异函数对象，ECMAScript 2015 中的术语）
- 在 bind() 被调用时，这个新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。

```javascript
function foo(name, age, height, address) {
  console.log("foo:", this);
  console.log("参数:", name, age, height, address);
}
var obj = { name: "why" };

// 需求: 调用foo时, 总是绑定到obj对象身上(不希望obj对象身上有函数)
// 1.bind函数的基本使用
var bar = foo.bind(obj);
bar(); // this -> obj

// 2.bind函数的其他参数(了解)
var bar = foo.bind(obj, "kobe", 18, 1.88);
bar("james");
```

### 2.3、内置函数的调用绑定

有些时候，我们会调用一些JavaScript的内置函数，或者一些第三方库中的内置函数。

- 这些内置函数会要求我们传入另外一个函数；
- 我们自己并不会显示的调用这些函数，而且JavaScript内部或者第三方库内部会帮助我们执行；
- 这些函数中的this又是如何绑定的呢？

```javascript
// 内置函数(第三方库): 根据一些经验
// 1.定时器
setTimeout(function () {
  console.log("定时器函数:", this);
}, 1000);

// 2.按钮的点击监听
var btnEl = document.querySelector("button");
btnEl.onclick = function () {
  console.log("btn的点击:", this);
};
btnEl.addEventListener("click", function () {
  console.log("btn的点击:", this);
});

// // 3.forEach
var names = ["abc", "cba", "nba"];
names.forEach(function (item) {
  console.log("forEach:", this);
}, "aaaa");
```

## 3、this绑定优先级

1、默认规则的优先级最低 

- 毫无疑问，默认规则的优先级是最低的，因为存在其他规则时，就会通过其他规则的方式来绑定this 

2、显示绑定优先级高于隐式绑定

3、new绑定优先级高于隐式绑定 

4、new绑定优先级高于bind

- new绑定和call、apply是不允许同时使用的，所以不存在谁的优先级更高
- new绑定可以和bind一起使用，new绑定优先级更高

```javascript
function foo() {
  console.log("foo:", this);
}

// 比较优先级:

// 1.显式绑定绑定的优先级高于隐式绑定
// 1.1.测试一:apply高于默认绑定
var obj = { foo: foo };
obj.foo.apply("abc");
obj.foo.call("abc");

// 1.2.测试二:bind高于默认绑定
var bar = foo.bind("aaa");
var obj = {
  name: "why",
  baz: bar,
};
obj.baz();

// 2.new绑定优先级高于隐式绑定
var obj = {
  name: "why",
  foo: function () {
    console.log("foo:", this);
    console.log("foo:", this === obj);
  },
};
new obj.foo();

// 3.new/显式
// 3.1. new不可以和apply/call一起使用

// 3.2. new优先级高于bind
function foo() {
  console.log("foo:", this);
}
var bindFn = foo.bind("aaa");
new bindFn();

// 4.bind/apply优先级
// bind优先级高于apply/call
function foo() {
  console.log("foo:", this);
}
var bindFn = foo.bind("aaa");
bindFn.call("bbb");
```

## 4、this绑定之外的情况

- 情况一：忽略显示绑定---如果在显示绑定中，我们传入一个null或者undefined，那么这个显示绑定会被忽略，使用默认规则
- 情况二：间接函数引用---创建一个函数的 间接引用，这种情况使用默认绑定规则。
  - 赋值(obj2.foo = obj1.foo)的结果是foo函数；
  -  foo函数被直接调用，那么是默认绑定；

```javascript
// 1.情况一: 显式绑定null/undefined, 那么使用的规则是默认绑定
function foo() {
  console.log("foo:", this);
}

foo.apply("abc");
foo.apply(null);
foo.apply(undefined);

// 2.情况二: 间接函数引用
var obj1 = {
  name: "obj1",
  foo: function () {
    console.log("foo:", this);
  },
};
var obj2 = {
  name: "obj2",
};

obj2.foo()
(obj2.foo = obj1.foo)(); // window
```

## 5、箭头函数

箭头函数是ES6之后增加的一种编写函数的方法，并且它比函数表达式要更加简洁：

- 箭头函数不会绑定this、arguments属性；
- 箭头函数不能作为构造函数来使用（不能和new一起来使用，会抛出错误）；

箭头函数如何编写呢？

- (): 函数的参数
- {}: 函数的执行体

```javascript
// 1.之前的方式
function foo1() {}
var foo2 = function (name, age) {
  console.log("函数体代码", this, arguments);
  console.log(name, age);
};

// 2.箭头函数完整写法
var foo3 = (name, age) => {
  console.log("箭头函数的函数体");
  console.log(name, age);
};

// 3.箭头函数的练习
// 3.1. forEach
var names = ["abc", "cba", "nba"];
names.forEach((item, index, arr) => {
  console.log(item, index, arr);
});
// 3.2. setTimeout
setTimeout(() => {
  console.log("setTimeout");
}, 3000);
```

> 箭头函数的优化

- 优化一: 如果只有一个参数()可以省略
- 优化二: 如果函数执行体中只有一行代码, 那么可以省略大括号
  - 并且这行代码的返回值会作为整个函数的返回值
- 优化三: 如果函数执行体只有返回一个对象, 那么需要给这个对象加上()

```javascript
names = ["abc", "cba", "nba"];
var nums = [20, 30, 11, 15, 111];

// 1.优化一: 如果箭头函数只有一个参数, 那么()可以省略
names.forEach((item) => {
  console.log(item);
});
var newNums = nums.filter((item) => {
  return item % 2 === 0;
});

// 2.优化二: 如果函数体中只有一行执行代码, 那么{}可以省略
names.forEach((item) => console.log(item));
// 一行代码中不能带return关键字, 如果省略, 需要带return一起省略(下一条规则)
var newNums = nums.filter((item) => {
  return item % 2 === 0;
});

// 3.优化三: 只有一行代码时, 这行代码的表达式结果会作为函数的返回值默认返回的
var newNums = nums.filter((item) => item % 2 === 0);
var newNums = nums.filter((item) => item % 2 === 0);

// 4.优化四: 如果默认返回值是一个对象, 那么这个对象必须加()
// 注意: 在react中我会经常使用 redux
var arrFn = () => ["abc", "cba"];
var arrFn = () => {}; // 注意: 这里是{}执行体
var arrFn = () => ({ name: "why" });
console.log(arrFn());

// 箭头函数实现nums的所有偶数平方的和
var nums = [20, 30, 11, 15, 111];
var result = nums
  .filter((item) => item % 2 === 0)
  .map((item) => item * item)
  .reduce((prevValue, item) => prevValue + item);
console.log(result);
```

> 箭头函数中的this使用（箭头函数中不绑定this）



```javascript
// 1.普通函数中是有this的标识符
function foo() {
  console.log("foo", this);
}

foo();
foo.apply("aaa");

// 2.箭头函数中, 压根没有this
var bar = () => {
  console.log("bar:", this);
};
bar();
// 通过apply调用时, 也是没有this
bar.apply("aaaa");
console.log("全局this:", this);
var message = "global message";

// 3.this的查找规则
var obj = {
  name: "obj",
  foo: function () {
    var bar = () => {
      console.log("bar:", this);
    };
    return bar;
  },
};
var fn = obj.foo();
fn.apply("bbb"); // obj
```

箭头函数this的应用

```javascript
// 网络请求的工具函数
function request(url, callbackFn) {
  var results = ["abc", "cba", "nba"];
  callbackFn(results);
}

// 实际操作的位置(业务)
var obj = {
  names: [],
  network: function () {
    // 1.早期的时候
    var _this = this;
    request("/names", function (res) {
      _this.names = [].concat(res);
    });

    // 2.箭头函数写法
    request("/names", (res) => {
      this.names = [].concat(res);
    });
  },
};

obj.network();
console.log(obj);
```

## 6、this面试题分析

```javascript
var name = "window";
var person = {
  name: "person",
  sayName: function () {
    console.log(this.name);
  },
};

function sayName() {
  var sss = person.sayName;

  sss(); // 绑定: 默认绑定, window -> window
  person.sayName(); // 绑定: 隐式绑定, person -> person
  person.sayName(); // 绑定: 隐式绑定, person -> person
  (b = person.sayName)(); // 术语: 间接函数引用, window -> window
}

sayName();
```

```javascript
var name = "window";
var person1 = {
  name: "person1",
  foo1: function () {
    console.log(this.name);
  }, 
  foo2: () => console.log(this.name),
  foo3: function () {
    return function () {
      console.log(this.name);
    };
  },
  foo4: function () {
    // console.log(this) // 第一个表达式this -> person1
    // console.log(this) // 第二个表达式this -> person2
    // console.log(this) // 第三个表达式this -> person1

    return () => {
      console.log(this.name);
    };
  },
};

var person2 = { name: "person2" };

// 开始题目:
person1.foo1(); // 隐式绑定: person1
person1.foo1.call(person2); // 显式绑定: person2

person1.foo2(); // 上层作用域: window
person1.foo2.call(person2); // 上层作用域: window

person1.foo3()(); // 默认绑定: window
person1.foo3.call(person2)(); // 默认绑定: window
person1.foo3().call(person2); // 显式绑定: person2

person1.foo4()(); // person1
person1.foo4.call(person2)(); // person2
person1.foo4().call(person2); // person1
```

```javascript
var name = "window";

/*
  1.创建一个空的对象
  2.将这个空的对象赋值给this
  3.执行函数体中代码
  4.将这个新的对象默认返回
*/
function Person(name) {
  this.name = name;
  (this.foo1 = function () {
    console.log(this.name);
  }),
    (this.foo2 = () => console.log(this.name)),
    (this.foo3 = function () {
      return function () {
        console.log(this.name);
      };
    }),
    (this.foo4 = function () {
      return () => {
        console.log(this.name);
      };
    });
}

// person1/person都是对象(实例instance)
var person1 = new Person("person1");
var person2 = new Person("person2");

// 面试题目:
person1.foo1(); // 隐式绑定: person1
person1.foo1.call(person2); // 显式绑定: person2

person1.foo2(); // 上层作用域查找: person1
person1.foo2.call(person2); // 上层作用域查找: person1

person1.foo3()(); // 默认绑定: window
person1.foo3.call(person2)(); // 默认绑定: window
person1.foo3().call(person2); // 显式绑定: person2

person1.foo4()(); // 上层作用域查找: person1(隐式绑定)
person1.foo4.call(person2)(); //  上层作用域查找: person2(显式绑定)
person1.foo4().call(person2); // 上层作用域查找: person1(隐式绑定)
```

```javascript
var name = "window";

/*
  1.创建一个空的对象
  2.将这个空的对象赋值给this
  3.执行函数体中代码
  4.将这个新的对象默认返回
*/
function Person(name) {
  this.name = name;
  this.obj = {
    name: "obj",
    foo1: function () {
      return function () {
        console.log(this.name);
      };
    },
    foo2: function () {
      return () => {
        console.log(this.name);
      };
    },
  };
}

var person1 = new Person("person1");
var person2 = new Person("person2");

person1.obj.foo1()(); // 默认绑定: window
person1.obj.foo1.call(person2)(); // 默认绑定: window
person1.obj.foo1().call(person2); // 显式绑定: person2

person1.obj.foo2()(); // 上层作用域查找: obj(隐式绑定)
person1.obj.foo2.call(person2)(); // 上层作用域查找: person2(显式绑定)
person1.obj.foo2().call(person2); // 上层作用域查找: obj(隐式绑定)

```







