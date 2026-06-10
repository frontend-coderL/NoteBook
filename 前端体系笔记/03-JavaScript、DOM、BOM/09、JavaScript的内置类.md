## 1、认识包装类型

### 1.1、原始类型的包装

JavaScript的原始类型并非对象类型，所以从理论上来说，它们是没有办法获取属性或者调用方法的。

但是，在开发中会看到，我们会经常这样操作：

```java
    console.log(name.length)
    console.log(name.split(" "))
    console.log(height.toFixed(2))
```

**为什么会出现这样奇怪的现象呢？**

- 原始类型是简单的值，默认并不能调用属性和方法； 
- **这是因为JavaScript为了可以使其可以获取属性和调用方法，对其封装了对应的包装类型；**

### 1.2、包装类型的使用过程







```javascript
      var name = "Hello World";
      var height = 1.8888888;

      function String(str) {
        this.str = str;
        this.length = 11;
        this.split = function () {};
      }

      // 在调用原始类型的属性或者方法时, 内部的操作 name = new String(name)
      console.log(name.length);
      console.log(name.split(" "));
      console.log(height.toFixed(2));

      // var obj = {
      //   name: "kobe",
      //   running: function() {

      //   }
      // }
      // obj.running()

      // 原始类型默认也是可以手动的创建对象(没有必要这样来做)
      var name1 = new String("Hello World");
      console.log(typeof name, typeof name1);
```



## 2、数字类型Number

> Number属性补充

- Number.MAX_SAFE_INTEGER：JavaScript 中最大的安全整数 (2^53 - 1)； 
-  Number.MIN_SAFE_INTEGER：JavaScript 中最小的安全整数 -(2^53 - 1)



## 3、数学对象Math







## 4、字符串类型String









## 5、数组Array使用详解







## 6、Date类型的使用







