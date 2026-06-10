# 一、函数

## 1、函数属性和arguments

### 1.1、函数对象的属性

我们知道JavaScript中函数也是一个对象，那么对象中就可以有属性和方法。

属性name：一个函数的名词我们可以通过name来访问；

属性length：属性length用于返回函数参数的个数；

- 注意：rest参数是不参与参数的个数的；

```javascript
// 定义函数
function foo(a, b, c) {}
var bar = function (m, n, ...others) {};

var obj = {};
obj.address = "广州市";
console.log(obj.address);

// 自定义属性
foo.message = "Hello Foo";
console.log(foo.message);

// 默认函数对象中已经有自己的属性
// 1.name属性
console.log(foo.name);
console.log(bar.name);

// // 将两个函数放到数组中(了解)
var fns = [foo, bar];
for (var fn of fns) {
  console.log(fn.name);
}

// 2.length属性: 参数的个数
function test() {}
test(111, 222, 333);
console.log(foo.length);
console.log(bar.length);
console.log(test.length);

// 额外补充
function demo(...args) {}
demo("abc", "cba", "nba");
```

### 1.2、认识arguments

**arguments 是一个 对应于 传递给函数的参数 的 类数组(array-like)对象**

array-like意味着它不是一个数组类型，而是一个对象类型：

- 但是它却拥有数组的一些特性，比如说length，比如可以通过index索引来访问；
- 但是它却没有数组的一些方法，比如filter、map等；

```javascript
function foo(m, n) {
  // arguments 类似数组对象
  console.log(arguments);
  // 1.默认用法:
  // 通过索引获取内容
  console.log(arguments[0]);
  console.log(arguments[1]);

  // // 遍历
  for (var i = 0; i < arguments.length; i++) {
    console.log(arguments[i]);
  }
  for (var arg of arguments) {
    console.log(arg);
  }

  // 2.需求获取所有参数中的偶数
  // 数组 filter
  for (var arg of arguments) {
    if (arg % 2 === 0) {
      console.log(arg);
    }
  }
  var evenNums = arguments.filter((item) => item % 2 === 0);
  console.log(eventNums);

  // 2.1.将arguments转成数组方式一:
  var newArguments = [];
  for (var arg of arguments) {
    newArguments.push(arg);
  }
  console.log(newArguments);

  // 2.2.将arguments转成数组方式三: ES6中方式
  var newArgs1 = Array.from(arguments);
  console.log(newArgs1);
  var newArgs2 = [...arguments];
  console.log(newArgs2);

  // 2.3.将arguments转成数组方式二: 调用slice方法
  var newArgs = [].slice.apply(arguments);
  var newArgs = Array.prototype.slice.apply(arguments);
  console.log(newArgs);
}

foo(10, 25, 32, 41);

// slice方法的回顾: 了解细节
var names = ["abc", "cba", "nba", "mba"];
var newNames = names.slice(); // this -> names

// slice方法 -> 函数
console.log(newNames);
obj.foo(); // this -> obj
```

### 1.3、arguments转Array

在开发中，我们经常需要将arguments转成Array，以便使用数组的一些特性。常见的转化方式如下

- 转化方式一：
  - 遍历arguments，添加到一个新数组中；

- 转化方式二：较难理解（有点绕），了解即可
  - 调用数组slice函数的call方法；

- 转化方式三：ES6中的两个方法 
  - Array.from
  - […arguments]

### 1.4、箭头函数不绑定arguments

箭头函数是不绑定arguments的，所以我们在箭头函数中使用arguments会去上层作用域查找

```javascript
// 1.箭头函数不绑定arguments
var bar = () => {
  console.log(arguments);
};

bar(11, 22, 33);

// 2.函数的嵌套箭头函数
function foo() {
  var bar = () => {
    console.log(arguments);
  };
  bar();
}

foo(111, 222);
```

### 1.5、函数的剩余（rest）参数

ES6中引用了rest parameter，可以将不定数量的参数放入到一个数组中：

- 如果最后一个参数是 ... 为前缀的，那么它会将剩余的参数放到该参数中，并且作为一个数组；

那么剩余参数和arguments有什么区别呢？

- 剩余参数只包含那些没有对应形参的实参，而 arguments 对象包含了传给函数的所有实参；
- arguments对象不是一个真正的数组，而rest参数是一个真正的数组，可以进行数组的所有操作；
- arguments是早期的ECMAScript中为了方便去获取所有的参数提供的一个数据结构，而rest参数是ES6中提供并且希望以此 来替代arguments的；

剩余参数必须放到最后一个位置，否则会报错。

```javascript
// 剩余参数: rest parameters
function foo(num1, num2, ...otherNums) {
  // otherNums数组
  console.log(otherNums);
}

foo(20, 30, 111, 222, 333);

// 默认一个函数只有剩余参数
function bar(...args) {
  console.log(args);
}

bar("abc", 123, "cba", 321);

// 注意事项: 剩余参数需要写到其他的参数最后
```

## 2、纯函数的理解和应用

### 2.1、理解JavaScript纯函数

函数式编程中有一个非常重要的概念叫纯函数，JavaScript符合函数式编程的范式，所以也有纯函数的概念；

- 在react开发中纯函数是被多次提及的；
-  比如react中组件就被要求像是一个纯函数（为什么是像，因为还有class组件），redux中有一个reducer的概念，也是要求 必须是一个纯函数；
- 所以掌握纯函数对于理解很多框架的设计是非常有帮助的；

纯函数的维基百科定义：

- 在程序设计中，若一个函数符合以下条件，那么这个函数被称为纯函数：
  - 此函数在相同的输入值时，需产生相同的输出。
  - 函数的输出和输入值以外的其他隐藏信息或状态无关，也和由I/O设备产生的外部输出无关。
  - 该函数不能有语义上可观察的函数副作用，诸如“触发事件”，使输出设备输出，或更改输出值以外物件的内容等。

当然上面的定义会过于的晦涩，所以我简单总结一下：

- 确定的输入，一定会产生确定的输出；
- 函数在执行过程中，不能产生副作用；

### 2.2、副作用概念的理解

那么这里又有一个概念，叫做副作用，什么又是副作用呢？

- 副作用（side effect）其实本身是医学的一个概念，比如我们经常说吃什么药本来是为了治病，可能会产生一些其他的副作 用；
- 在计算机科学中，也引用了副作用的概念，表示在执行一个函数时，除了返回函数值之外，还对调用函数产生了附加的影响， 比如修改了全局变量，修改参数或者改变外部的存储；

纯函数在执行的过程中就是不能产生这样的副作用：

- 副作用往往是产生bug的 “温床”。

### 2.3、纯函数的案例

我们来看一个对数组操作的两个函数：

- slice：slice截取数组时不会对原数组进行任何操作,而是生成一个新的数组；
- splice：splice截取数组, 会返回一个新的数组, 也会对原数组进行修改；

slice就是一个纯函数，不会修改数组本身，而splice函数不是一个纯函数；

```javascript
function sum(num1, num2) {
  return num1 + num2;
}

// 不是一个纯函数
var address = "广州市";
function printInfo(info) {
  console.log(info.name, info.age, info.message);
  info.flag = "已经打印结束";
  address = info.address;
}

var obj = {
  name: "why",
  age: 18,
  message: "哈哈哈哈",
};

printInfo(obj);

var names = ["abc", "cba", "nba", "mba"];

// 1.slice: 纯函数
var newNames = [].slice.apply(names, [1, 3]);
console.log(names);

// 2.splice: 操作数组的利器(不是纯函数)
names.splice(2, 2);
console.log(names);
```

### 2.4、纯函数的作用和优势

为什么纯函数在函数式编程中非常重要呢？

- 因为你可以安心的编写和安心的使用；
- 你在写的时候保证了函数的纯度，只是单纯实现自己的业务逻辑即可，不需要关心传入的内容是如何获得的或者依赖其他的 外部变量是否已经发生了修改；
- 你在用的时候，你确定你的输入内容不会被任意篡改，并且自己确定的输入，一定会有确定的输出；

React中就要求我们无论是函数还是class声明一个组件，这个组件都必须像纯函数一样，保护它们的props不被修改

## 3、柯里化的理解和应用

### 3.1、柯里化概念的理解

柯里化也是属于函数式编程里面一个非常重要的概念。

- 是一种关于函数的高阶技术；
- 它不仅被用于 JavaScript，还被用于其他编程语言；

我们先来看一下维基百科的解释：

- 在计算机科学中，柯里化（英语：Currying），又译为卡瑞化或加里化；
- 是把接收多个参数的函数，变成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数，而且返回 结果的新函数的技术；
- 柯里化声称 “如果你固定某些参数，你将得到接受余下参数的一个函数”；

维基百科的结束非常的抽象，我们这里做一个总结：

- 只传递给函数一部分参数来调用它，让它返回一个函数去处理剩余的参数；
- 这个过程就称之为柯里化；

柯里化是一种函数的转换，将一个函数从可调用的 f(a, b, c) 转换为可调用的 f(a)(b)(c)。

- 柯里化不会调用函数。它只是对函数进行转换。

### 3.2、柯里化的代码转换

```javascript
// 普通的函数
function foo1(x, y, z) {
  console.log(x + y + z);
}

foo1(10, 20, 30);
foo1(20, 33, 55);

// 因为foo不是一个柯里化的函数, 所以目前是不能这样调用
// 柯里化函数
function foo2(x) {
  return function (y) {
    return function (z) {
      console.log(x + y + z);
    };
  };
}

foo2(10)(20)(30);
foo2(20)(33)(55);

// 另外一种写法: 箭头函数的写法
function foo3(x) {
  return (y) => {
    return (z) => {
      console.log(x + y + z);
    };
  };
}

var foo3 = (x) => (y) => (z) => {
  console.log(x + y + z);
};

foo3(10)(20)(30);
```

### 3.3、柯里化优势一 - 函数的职责单一

那么为什么需要有柯里化呢？

- 在函数式编程中，我们其实往往希望一个函数处理的问题尽可能的单一，而不是将一大堆的处理过程交给一个函数来处理；
- 那么我们是否就可以将每次传入的参数在单一的函数中进行处理，处理完后在下一个函数中再使用处理后的结果；

### 3.4、柯里化优势二 - 函数的参数复用

另外一个使用柯里化的场景是可以帮助我们可以复用参数逻辑：

- makeAdder函数要求我们传入一个num（并且如果我们需要的话，可以在这里对num进行一些修改）； 
- 在之后使用返回的函数时，我们不需要再继续传入num了；

### 3.5、柯里化高级 - 自动柯里化函数

```javascript
function foo(x, y, z) {
  console.log(x + y + z);
}

function sum(num1, num2) {
  return num1 + num2;
}

function logInfo(date, type, message) {
  console.log(`时间:${date} 类型:${type} 内容:${message}`);
}

// 手动转化
// 封装函数: 自动转化柯里化过程(有一点难度)
function hyCurrying(fn) {
  function curryFn(...args) {
    // 两类操作:
    // 第一类操作: 继续返回一个新的函数, 继续接受参数
    // 第二类操作: 直接执行fn的函数
    if (args.length >= fn.length) {
      // 执行第二类
      // return fn(...args)
      return fn.apply(this, args);
    } else {
      // 执行第一类
      return function (...newArgs) {
        // return curryFn(...args.concat(newArgs))
        return curryFn.apply(this, args.concat(newArgs));
      };
    }
  }

  return curryFn;
}

// 对其他的函数进行柯里化
var fooCurry = hyCurrying(foo);
fooCurry(10)(20)(30);
fooCurry(55, 12, 56);

var sumCurry = hyCurrying(sum);
var sum5 = sumCurry(5);
console.log(sum5(10));
console.log(sum5(15));
console.log(sum5(18));

var logInfoCurry = hyCurrying(logInfo);
logInfoCurry("2022-06-01")("DEBUG")("我发现一个bug, 哈哈哈哈");
```

## 4、组合函数理解和应用

### 4.1、组合函数概念的理解

组合（Compose）函数是在JavaScript开发过程中一种对函数的使用技巧、模式：

- 比如我们现在需要对某一个数据进行函数的调用，执行两个函数fn1和fn2，这两个函数是依次执行的； 
- 那么如果每次我们都需要进行两个函数的调用，操作上就会显得重复；
- 那么是否可以将这两个函数组合起来，自动依次调用呢？
- 这个过程就是对函数的组合，我们称之为 组合函数（Compose Function）；

### 4.2、实现组合函数

```javascript
// 第一步对数字*2
function double(num) {
  return num * 2;
}

// 第二步对数字**2
function pow(num) {
  return num ** 2;
}

// 封装的函数: 你传入多个函数, 我自动的将多个函数组合在一起挨个调用
function composeFn(...fns) {
  // 1.边界判断(edge case)
  var length = fns.length;
  if (length <= 0) return;
  for (var i = 0; i < length; i++) {
    var fn = fns[i];
    if (typeof fn !== "function") {
      throw new Error(`index position ${i} must be function`);
    }
  }

  // 2.返回的新函数
  return function (...args) {
    var result = fns[0].apply(this, args);
    for (var i = 1; i < length; i++) {
      var fn = fns[i];
      result = fn.apply(this, [result]);
    }
    return result;
  };
}

var newFn = composeFn(double, pow, console.log);
newFn(100);
newFn(55);
newFn(22);
```

## 5、with、eval的使用

### 5.1、with语句的使用

with语句 扩展一个语句的作用域链。

不建议使用with语句，因为它可能是混淆错误和兼容性问题的根源。

```javascript
var obj = {
  message: "Hello World",
};

with (obj) {
  console.log(message);
}
```

### 5.2、eval函数

内建函数 eval 允许执行一个代码字符串。

-  eval是一个特殊的函数，它可以将传入的字符串当做JavaScript代码来运行；
- eval会将最后一句执行语句的结果，作为返回值；

```javascript
var message = "Hello World";
var codeString = `var name = "why"; console.log(name); console.log(message); "abc";`;
var result = eval(codeString);
console.log(result);

function foo() {}
```



不建议在开发中使用eval：

- eval代码的可读性非常的差（代码的可读性是高质量代码的重要原则）；
- eval是一个字符串，那么有可能在执行的过程中被刻意篡改，那么可能会造成被攻击的风险；
- eval的执行必须经过JavaScript解释器，不能被JavaScript引擎优化；

## 6、严格模式的使用

### 6.1、认识严格模式

JavaScript历史的局限性：

- 长久以来，JavaScript 不断向前发展且并未带来任何兼容性问题；
- 新的特性被加入，旧的功能也没有改变，这么做有利于兼容旧代码；
- 但缺点是 JavaScript 创造者的任何错误或不完善的决定也将永远被保留在 JavaScript 语言中；

在ECMAScript5标准中，JavaScript提出了严格模式的概念（Strict Mode）：

- 严格模式很好理解，是一种具有限制性的JavaScript模式，从而使代码隐式的脱离了 ”懒散（sloppy）模式“；
- 支持严格模式的浏览器在检测到代码中有严格模式时，会以更加严格的方式对代码进行检测和执行；

严格模式对正常的JavaScript语义进行了一些限制：

- 严格模式通过 抛出错误 来消除一些原有的 静默（silent）错误；
- 严格模式让JS引擎在执行代码时可以进行更多的优化（不需要对一些特殊的语法进行处理）；
-  严格模式禁用了在ECMAScript未来版本中可能会定义的一些语法；

### 6.2、开启严格模式

那么如何开启严格模式呢？严格模式支持粒度话的迁移：

- 可以支持在js文件中开启严格模式；
- 也支持对某一个函数开启严格模式；

严格模式通过在文件或者函数开头使用 use strict 来开启。

没有类似于 "no use strict" 这样的指令可以使程序返回默认模式。

-  现代 JavaScript 支持 “class” 和 “module” ，它们会自动启用 use strict；

```javascript
<script>
  // 给整个script开启严格模式
  "use strict";

  // 给一个函数开启严格模式
  function foo() {
    "use strict";
  }

  class Person {}
</script>
```

### 6.3、严格模式限制

这里我们来说几个严格模式下的严格语法限制：

- JavaScript被设计为新手开发者更容易上手，所以有时候本来错误语法，被认为也是可以正常被解析的；
- 但是这种方式可能给带来留下来安全隐患；
- 在严格模式下，这种失误就会被当做错误，以便可以快速的发现和修正；

1.  无法意外的创建全局变量
2. 严格模式会使引起静默失败(silently fail,注:不报错也没有任何效果)的赋值操作抛出异常 
3. 严格模式下试图删除不可删除的属性
4. 严格模式不允许函数参数有相同的名称
5. 不允许0的八进制语法
6. 在严格模式下，不允许使用with
7. 在严格模式下，eval不再为上层引用变量
8. 严格模式下，this绑定不会默认转成对象

```javascript
<script>
  "use strict";
  // 1.不会意外创建全局变量
  function foo() {
    message = "Hello World";
  }

  foo();
  console.log(message);

  // 2.发现静默错误
  var obj = {
    name: "why",
  };

  Object.defineProperty(obj, "name", {
    writable: false,
    configurable: false,
  });

  obj.name = "kobe";
  console.log(obj.name);

  // delete obj.name
  console.log(obj);

  // 3.参数名称不能相同
  function foo(num, num) {}

  // 4.不能以0开头
  console.log(0o123);

  // 5.eval函数不能为上层创建变量
  eval(`var message = "Hello World"`);
  console.log(message);

  // 6.严格模式下, this是不会转成对象类型的
  function foo() {
    console.log(this);
  }
  foo.apply("abc");
  foo.apply(123);
  foo.apply(undefined);
  foo.apply(null);

  // 独立函数执行默认模式下, 绑定window对象
  // 在严格模式下, 不绑定全局对象而是undefined
  foo();
</script>
```



# 二、对象

## 1、Object.defineProperty

### 1.1、对属性操作的控制

在前面我们的属性都是直接定义在对象内部，或者直接添加到对象内部的：

- 但是这样来做的时候我们就不能对这个属性进行一些限制：比如这个属性是否是可以通过delete删除的？这个属性是否在forin遍历的时候被遍历出来呢？

如果我们想要对一个属性进行比较精准的操作控制，那么我们就可以使用属性描述符。

- 通过属性描述符可以精准的添加或修改对象的属性；
- 属性描述符需要使用 Object.defineProperty 来对属性进行添加或者修改；

### 1.2、Object.defineProperty

**Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。**

可接收三个参数：

- obj要定义属性的对象；
- prop要定义或修改的属性的名称或 Symbol；
- descriptor要定义或修改的属性描述符；

返回值：

- 被传递给函数的对象。

### 1.3、属性描述符分类

属性描述符的类型有两种：

- 数据属性（Data Properties）描述符（Descriptor）；
- 存取属性（Accessor访问器 Properties）描述符（Descriptor）；

![image-20230319204354021](.\img\image-20230319204354021.png)

## 2、数据属性描述符

数据数据描述符有如下四个特性：

- [[Configurable]]：表示属性是否可以通过delete删除属性，是否可以修改它的特性，或者是否可以将它修改为存取属性描述符；
  - 当我们直接在一个对象上定义某个属性时，这个属性的[[Configurable]]为true；
  - 当我们通过属性描述符定义一个属性时，这个属性的[[Configurable]]默认为false；

- [[Enumerable]]：表示属性是否可以通过for-in或者Object.keys()返回该属性；
  - 当我们直接在一个对象上定义某个属性时，这个属性的[[Enumerable]]为true；
  - 当我们通过属性描述符定义一个属性时，这个属性的[[Enumerable]]默认为false；

- [[Writable]]：表示是否可以修改属性的值；
  - 当我们直接在一个对象上定义某个属性时，这个属性的[[Writable]]为true；
  - 当我们通过属性描述符定义一个属性时，这个属性的[[Writable]]默认为false；

- [[value]]：属性的value值，读取属性时会返回该值，修改属性时，会对其进行修改；
  - 默认情况下这个值是undefined；

```javascript
var obj = {
  name: "why", // configurable: true
  age: 18,
};

Object.defineProperty(obj, "name", {
  configurable: false, // 告诉js引擎, obj对象的name属性不可以被删除
  enumerable: false, // 告诉js引擎, obj对象的name属性不可枚举(for in/Object.keys)
  writable: false, // 告诉js引擎, obj对象的name属性不写入(只读属性 readonly)
  value: "coderwhy", // 告诉js引擎, 返回这个value
});

delete obj.name;
console.log(obj);

// 通过Object.defineProperty添加一个新的属性
Object.defineProperty(obj, "address", {});
delete obj.address;
console.log(obj);

console.log(Object.keys(obj));

obj.name = "kobe";
console.log(obj.name);
```

## 3、存取属性描述符

数据数据描述符有如下四个特性：

- [[Configurable]]：表示属性是否可以通过delete删除属性，是否可以修改它的特性，或者是否可以将它修改为存取属性描述符；
  - 和数据属性描述符是一致的；
  - 当我们直接在一个对象上定义某个属性时，这个属性的[[Configurable]]为true；
  - 当我们通过属性描述符定义一个属性时，这个属性的[[Configurable]]默认为false；
- [[Enumerable]]：表示属性是否可以通过for-in或者Object.keys()返回该属性；
  - 和数据属性描述符是一致的；
  - 当我们直接在一个对象上定义某个属性时，这个属性的[[Enumerable]]为true；
  - 当我们通过属性描述符定义一个属性时，这个属性的[[Enumerable]]默认为false；
- [[get]]：获取属性时会执行的函数。默认为undefined
- [[set]]：设置属性时会执行的函数。默认为undefined

```javascript
// vue2响应式原理
var obj = {
  name: "why",
};

// 对obj对象中的name添加描述符(存取属性描述符)
var _name = "";
Object.defineProperty(obj, "name", {
  configurable: true,
  enumerable: false,
  set: function (value) {
    console.log("set方法被调用了", value);
    _name = value;
  },
  get: function () {
    console.log("get方法被调用了");
    return _name;
  },
});

obj.name = "kobe";
obj.name = "jame";
obj.name = "curry";
obj.name = "coderwhy";

// 获取值
console.log(obj.name);
```



## 4、Object.defineProperties

**同时定义多个属性**

Object.defineProperties() 方法直接在一个对象上定义 多个 新的属性或修改现有属性，并且返回该对象。

```javascript
var obj = {
  name: "why",
  age: 18,
  height: 1.88,
};

Object.defineProperty(obj, "name", {});
Object.defineProperty(obj, "age", {});
Object.defineProperty(obj, "height", {});

// 新增的方法
Object.defineProperties(obj, {
  name: {
    configurable: true,
    enumerable: true,
    writable: false,
  },
  age: {},
  height: {},
});
```

## 5、对象的其他方法补充

- 获取对象的属性描述符：
  - getOwnPropertyDescriptor
  - getOwnPropertyDescriptors
- 禁止对象扩展新属性：preventExtensions
  - 给一个对象添加新的属性会失败（在严格模式下会报错）； 
- 密封对象，不允许配置和删除属性：seal
  - 实际是调用preventExtensions
  - 并且将现有属性的configurable:false
- 冻结对象，不允许修改现有属性： freeze
  - 实际上是调用seal
  - 并且将现有属性的writable: false

```javascript
var obj = {
  name: "why",
  age: 18,
};

// 1.获取属性描述符
console.log(Object.getOwnPropertyDescriptor(obj, "name"));
console.log(Object.getOwnPropertyDescriptors(obj));

// 2.阻止对象的扩展
Object.preventExtensions(obj);
obj.address = "广州市";
console.log(obj);

// 3.密封对象(不能进行配置)
Object.seal(obj);
delete obj.name;
console.log(obj);

// 4.冻结对象(不能进行写入)
Object.freeze(obj);
obj.name = "kobe";
console.log(obj);
```





