





























## 13、创建对象的方式三---构造函数（类）

### 13.1、认识构造函数

> 什么是构造函数

- 构造函数也称之为构造器，通常是我们在创建对象时会调用的函数
- 在其他编程语言中，构造函数是存在于类中的一个方法，称之为构造方法
- 但是JavaScript中不同，构造函数扮演了其他语言中类的角色

> 在JavaScript中，构造函数是类的扮演者

### 13.2、类和对象的关系





### 13.3、JavaScript中的类（ES5）



如果一个函数被使用new操作符调用了，他会执行以下操作：

- 1、在内存中创建一个新的对象（空对象）
- 2、这个对象内部的[[prototype]]属性会被赋值为该构造函数的prototype属性
- 3、构造函数内部的this，会指向创建出来的新对象
- 4、执行函数的内部代码
- 5、如果构造函数没有返回非空对象，则返回创建出来的新对象

```javascript
  <script>
    // JavaScript已经默认提供给了我们可以更加符合JavaScript思维方式(面向对象的思维方式)的一种创建对象的规则
    // 在函数中的this一般指向某一个对象
    /*
      如果一个函数被new操作符调用
        1.创建出来一个新的空对象
        2.让this指向这个空对象
        3.执行函数体的代码块
        4.如果没有明确的返回一个非空对象, 那么this指向的对象会自动返回
    */
    function coder(name, age, height) {
      this.name = name
      this.age = age
      this.height = height
      this.running = function() {
        console.log("running~")
      }
    }

    // 在函数调用的前面加 new 关键字(操作符)
    var stu1 = new coder("why", 18, 1.88)
    var stu2 = new coder("kobe", 30, 1.98)
  </script>
```



### 13.4、构造函数的补充

- 构造函数的名称：大驼峰
- 创建的对象非Onject类型

```javascript
    var obj1 = {}
    var obj2 = new Object()
    var obj3 = new Person() // 类型：Person
```

- 其它特性：原型、原型链、实现继承的方案、ES6中类、继承的实现

## 14、额外补充：全局对象---window

```javascript
    // 浏览器中存在一个全局对象object -> window
    // 作用一: 查找变量时, 最终会找到window头上
    // 作用二: 将一些浏览器全局提供给我们的变量/函数/对象, 放在window对象上面
    // 作用三(了解): 使用var定义的变量会被默认添加到window上面
    console.log(window)
    

    // 使用var定义变量
    var message = "Hello World"

    function foo() {
      // 自己的作用域
      // abc()
      // alert("Hello World")
      console.log(window.console === console)
   
      // 创建一个对象
      // var obj = new Object()
      console.log(window.Object === Object)

      // DOM
      console.log(document)
        
      // window.message
	  console.log(window.message)
    }
	foo()
```

## 15、额外补充：函数也是对象

```javascript
    // 定义原始类型的变量
    var name = "why"
    var age = 18

    // 定义对象类型的变量
    // 地址 - 指针 - 引用
    var obj = {} // 堆内存
    var foo = function() {} // 堆内存
    function bar() {} // 堆内存

    console.log(typeof obj) // object
    console.log(typeof foo) // function -> object

    // var stu = new Student() // stu是一个Student -> Person

    // 引申一些别的知识(了解)
    var info = {}
    info.name = "abc"

    function sayHello() {
    }
    sayHello.age = 18
    console.log(sayHello.age) // 18

    function Dog() {
    }
    // 构造函数上(类上面)添加的函数, 称之为类方法
    Dog.running = function() {}
    Dog.running()
```






