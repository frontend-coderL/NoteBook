## 1、认识事件处理

Web页面需要经常和用户之间进行交互，而交互的过程中我们可能想要捕捉这个交互的过程：

- 比如用户点击了某个按钮、用户在输入框里面输入了某个文本、用户鼠标经过了某个位置；
- 浏览器需要搭建一条JavaScript代码和事件之间的桥梁；
- 当某个事件发生时，让JavaScript可以响应（执行某个函数），所以我们需要针对事件编写处理程序（handler）；

如何进行事件监听呢？

- 事件监听方式一：在script中直接监听（很少使用）；
- 事件监听方式二：DOM属性，通过元素的on来监听事件；
- 事件监听方式三：通过 EventTarget 中的 addEventListener 来监听；

```javascript
  <body>
    <!-- 直接在html中编写JavaScript代码(了解) -->
    <button onclick="console.log('按钮1发生了点击~');">按钮1</button>
    <button class="btn2">按钮2</button>
    <button class="btn3">按钮3</button>

    <script>
      // 1.获取元素对象
      var btn2El = document.querySelector(".btn2");
      var btn3El = document.querySelector(".btn3");

      // 2.onclick属性(缺陷：只能绑定一个函数)
      function handleClick01() {
        console.log("按钮2发生了点击~");
      }
      function handleClick02() {
        console.log("按钮2的第二个处理函数");
      }
      btn2El.onclick = handleClick01;
      btn2El.onclick = handleClick02;

      // 3.addEventListener(推荐)
      btn3El.addEventListener("click", function () {
        console.log("第一个btn3的事件监听~");
      });
      btn3El.addEventListener("click", function () {
        console.log("第二个btn3的事件监听~");
      });
      btn3El.addEventListener("click", function () {
        console.log("第三个btn3的事件监听~");
      });
    </script>
  </body>
```

### 1.1、常见的事件列表





### 1.2、认识事件流

事实上对于事件有一个概念叫做事件流，为什么会产生事件流呢？

- 我们可以想到一个问题：当我们在浏览器上对着一个元素点击时，你点击的不仅仅是这个元素本身；
- 这是因为我们的HTML元素是存在父子元素叠加层级的；
- 比如一个span元素是放在div元素上的，div元素是放在body元素上的，body元素是放在html元素上的；

```javascript
<body>
  <div class="box">
    <span></span>
  </div>

  <script>
    // 1.获取元素
    var spanEl = document.querySelector("span")
    var divEl = document.querySelector("div")
    var bodyEl = document.body


    // 2.绑定点击事件
    // spanEl.onclick = function() {
    //   console.log("span元素发生了点击~")
    // }
    // divEl.onclick = function() {
    //   console.log("div元素发生了点击~")
    // }
    // bodyEl.onclick = function() {
    //   console.log("body元素发生了点击~")
    // }

    // 默认情况下是事件冒泡
    spanEl.addEventListener("click", function() {
      console.log("span元素发生了点击~冒泡")
    })
    divEl.addEventListener("click", function() {
      console.log("div元素发生了点击~冒泡")
    })
    bodyEl.addEventListener("click", function() {
      console.log("body元素发生了点击~冒泡")
    })

    // 设置希望监听事件捕获的过程
    spanEl.addEventListener("click", function() {
      console.log("span元素发生了点击~捕获")
    }, true)
    divEl.addEventListener("click", function() {
      console.log("div元素发生了点击~捕获")
    }, true)
    bodyEl.addEventListener("click", function() {
      console.log("body元素发生了点击~捕获")
    }, true)
  </script>
</body>
```

## 2、事件冒泡捕获

**我们会发现默认情况下事件是从最内层的span向外依次传递的顺序，这个顺序我们称之为事件冒泡（Event Bubble）; **

**事实上，还有另外一种监听事件流的方式就是从外层到内层（body -> span），这种称之为事件捕获（Event Capture）；**

*为什么会产生两种不同的处理流呢？这是因为早期浏览器开发时，不管是IE还是Netscape公司都发现了这个问题; 但是他们采用了完全相反的事件流来对事件进行了传递；IE采用了事件冒泡的方式，Netscape采用了事件捕获的方式；*

### 2.1、事件捕获和冒泡的过程

如果我们都监听，那么会按照如下顺序来执行：

- 捕获阶段（Capturing phase）： 事件（从 Window）向下走近元素。
- 目标阶段（Target phase）： 事件到达目标元素。
- 冒泡阶段（Bubbling phase）： 事件从元素上开始冒泡。

事实上，我们可以通过event对象来获取当前的阶段： eventPhase 

开发中通常会使用事件冒泡，所以事件捕获了解即可。

![image-20230226210540734](.\05、JavaScript分支语句和逻辑运算符.assets\image-20230226210540734.png)



## 3、事件对象event

### 3.1、事件对象

当一个事件发生时，就会有和这个事件相关的很多信息：

- 比如事件的类型是什么，你点击的是哪一个元素，点击的位置是哪里等等相关的信息；

- 那么这些信息会被封装到一个Event对象中，这个对象由浏览器创建，称之为event对象；

- 该对象给我们提供了想要的一些属性，以及可以通过该对象进行某些操作；

如何获取这个event对象呢？

- event对象会在传入的事件处理（event handler）函数回调时，被系统传入；

- 我们可以在回调函数中拿到这个event对象；

### 3.2、event常见的属性

常见的属性： 

- type：事件的类型；
- target：当前事件发生的元素；
- currentTarget：当前处理事件的元素；
- eventPhase：事件所处的阶段；
- offsetX、offsetY：事件发生在元素内的位置；
- clientX、clientY：事件发生在客户端内的位置；
- pageX、pageY：事件发生在客户端相对于document的位置；
- screenX、screenY：事件发生相对于屏幕的位置； 

```javascript
<body>
  <div class="box">
    <span class="btn">
      <button>按钮</button>
    </span>
  </div>
  <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
  <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
  <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
  <script>
    var divEl = document.querySelector("div")
    divEl.onclick = function(event) {
      // 1.偶尔会使用
      console.log("事件类型:", event.type)
      console.log("事件阶段:", event.eventPhase)

      // 2.比较少使用
      console.log("事件元素中位置", event.offsetX, event.offsetY)
      console.log("事件客户端中位置", event.clientX, event.clientY)
      console.log("事件页面中位置", event.pageX, event.pageY)
      console.log("事件在屏幕中位置", event.screenX, event.screenY)

      // 3.target/currentTarget
      console.log(event.target)
      console.log(event.currentTarget)
      console.log(event.currentTarget === event.target)
    }
  </script>
</body>
```

### 3.3、event常见的方法

常见的方法：

- preventDefault：取消事件的默认行为；
- stopPropagation：阻止事件的进一步传递（冒泡或者捕获都可以阻止）；

```javascript
<body>
  <a href="http://www.baidu.com">百度一下</a>
  <div class="box">
    <span>
      <button>按钮</button>
    </span>
  </div>
  <script>
    // 1.阻止默认行为
    var aEl = document.querySelector("a")
    aEl.onclick = function(event) {
      console.log("a元素发生了点击~")
      event.preventDefault()
    }

    // 2.阻止事件进一步传递
    var btnEl = document.querySelector("button")
    var spanEl = document.querySelector("span")
    var divEl = document.querySelector("div")
    
    divEl.addEventListener("click", function(event) {
      console.log("div的事件捕获监听~")
      event.stopPropagation()
    }, true)
    spanEl.addEventListener("click", function() {
      console.log("span的事件捕获监听~")
    }, true)
    btnEl.addEventListener("click", function(event) {
      console.log("button的事件捕获监听~")
      event.stopPropagation()
    }, true)
    
    divEl.addEventListener("click", function() {
      console.log("div的事件冒泡监听~")
      event.stopPropagation()
    })
    spanEl.addEventListener("click", function(event) {
      console.log("span的事件冒泡监听~")
      event.stopPropagation()
    })
    btnEl.addEventListener("click", function() {
      console.log("button的事件冒泡监听~")
    })
  </script>
</body>
```

### 3.3、事件处理中的this

**在函数中，我们也可以通过this来获取当前的发生元素：这是因为在浏览器内部，调用event handler是绑定到当前的target上的**

```javascript
  <body>
    <div>
      <button>按钮</button>
    </div>
    <script>
      var btnEl = document.querySelector("button");
      var divEl = document.querySelector("div");

      divEl.onclick = function (event) {
        console.log(this);
        console.log(event.currentTarget);
        console.log(divEl);
        console.log(this === divEl);
      };
      // divEl.addEventListener("click", function() {
      //   console.log(this)
      // })
    </script>
  </body>
```

## 4、EventTarget使用

我们会发现，所有的节点、元素都继承自EventTarget ，事实上Window也继承自EventTarget；

那么这个EventTarget是什么呢？ EventTarget是一个DOM接口，主要用于添加、删除、派发Event事件；

EventTarget常见的方法：

- addEventListener：注册某个事件类型以及事件处理函数；
- removeEventListener：移除某个事件类型以及事件处理函数；
- dispatchEvent：派发某个事件类型到EventTarget上；

```javascript
<body>
  <button>按钮</button>
  <script>
    var btnEl = document.querySelector("button")

    // 1.将监听函数移除的过程
    var foo = function() {
      console.log("监听到按钮的点击")
    }
    btnEl.addEventListener("click", foo)
    // 需求: 过5s钟后, 将这个事件监听移除掉
    setTimeout(function() {
      btnEl.removeEventListener("click", foo)
    }, 5000)

    // 这种做法是无法移除的
    btnEl.addEventListener("click", function() {
      console.log("btn监听的处理函数~")
    })
    setTimeout(function() {
      btnEl.removeEventListener("click", function() {})
    }, 5000)

    // 举个例子
    var obj1 = {name: "obj1"}
    var obj2 = {name: "obj2"}
    var obj3 = {name: "obj3"}
    var arr = [obj1, obj2, obj3]
    function removeArr(obj) {
      var index = arr.findIndex(function(item) {
        return item === {}
      })
      arr.splice(index, 1)
    }
    removeArr({})
  </script>
</body>
```

```javascript
  <script>
    // eventtarget就可以实现类似于事件总线的效果
    window.addEventListener("coderwhy", function() {
      console.log("监听到coderwhy的呼唤~")
    })

    setTimeout(function() {
      window.dispatchEvent(new Event("coderwhy"))
    }, 5000)
  </script>
```

## 5、事件委托模式

事件冒泡在某种情况下可以帮助我们实现强大的事件处理模式 – 事件委托模式（也是一种设计模式）

那么这个模式是怎么样的呢？ 因为当子元素被点击时，父元素可以通过冒泡可以监听到子元素的点击， 并且可以通过event.target获取到当前监听的元素。

```javascript
    <style>
      .active {
        color: red;
        font-size: 20px;
        background-color: orange;
      }
    </style>
  <body>
    <ul>
      <li>1</li>
      <li>2</li>
      <li>3</li>
      <li>4</li>
      <li>5</li>
      <li>6</li>
      <li>7</li>
      <li>8</li>
      <li>9</li>
      <li>10</li>
    </ul>
    <script>
      // 1.每一个li都监听自己的点击, 并且有自己的处理函数(自己的函数)
      var liEls = document.querySelectorAll("li");
      for (var liEl of liEls) {
        // 监听点击
        liEl.onclick = function (event) {
          event.currentTarget.classList.add("active");
        };
      }

      // 2.统一在ul中监听
      var ulEl = document.querySelector("ul");
      ulEl.onclick = function (event) {
        console.log("点击了某一个li", event.target);
        event.target.classList.add("active");
      };

      // 3.新需求: 点击li变成active, 其他的取消active
      var ulEl = document.querySelector("ul");
      var activeLiEl = null;
      ulEl.onclick = function (event) {
        // 1.将之前的active移除掉
        for (var i = 0; i < ulEl.children.length; i++) {
          var liEl = ulEl.children[i];
          if (liEl.classList.contains("active")) {
            liEl.classList.remove("active");
          }
        }

        // 1.找到active的li, 移除掉active
        var activeLiEl = ulEl.querySelector(".active");
        activeLiEl && activeLiEl.classList.remove("active");

        // 1.变量记录的方式
        // edge case
        if (activeLiEl) {
          activeLiEl.classList.remove("active");
        }

        // 2.给点击的元素添加active
        event.target.classList.add("active");

        // 3.记录最新的active对应的li
        activeLiEl = event.target;
      };
    </script>
  </body>
```

```javascript
<body>
  <div class="box">
    <button data-action="search">搜索~</button>
    <button data-action="new">新建~</button>
    <button data-action="remove">移除~</button>
    <button>1111</button>
  </div>
  <script>
    var boxEl = document.querySelector(".box")
    boxEl.onclick = function(event) {
      var btnEl = event.target
      var action = btnEl.dataset.action
      switch (action) {
        case "remove":
          console.log("点击了移除按钮")
          break
        case "new":
          console.log("点击了新建按钮")
          break
        case "search":
          console.log("点击了搜索按钮")
          break
        default:
          console.log("点击了其他")
      }
    }
  </script>
</body>
```

## 6、常见的事件

### 6.1、常见的鼠标事件

| 属性        | 描述                                         |
| ----------- | -------------------------------------------- |
| click       | 当用户点击某个对象时调用的事件句柄。         |
| contextmenu | 在用户点击鼠标右键打开上下文菜单时触发       |
| dbclick     | 当用户双击某个对象时调用的事件句柄。         |
| mousedown   | 鼠标按钮被按下。                             |
| mouseup     | 鼠标按键被松开。                             |
| mouseover   | 鼠标移到某元素之上。（支持冒泡）             |
| mouseout    | 鼠标从某元素移开。（支持冒泡）               |
| mouseenter  | 当鼠标指针移动到元素上时触发。（不支持冒泡） |
| mouseleave  | 当鼠标指针移出元素时触发。（不支持冒泡）     |
| mousemove   | 鼠标被移动。                                 |

### 6.2、mouseover和mouseenter的区别

mouseenter和mouseleave

- 不支持冒泡
- 进入子元素依然属于在该元素内，没有任何反应

 mouseover和mouseout

- 支持冒泡
-  进入元素的子元素时
  - 先调用父元素的mouseout 
  - 再调用子元素的mouseover
  - 因为支持冒泡，所以会将mouseover传递到父元素中；

```javascript
  <script>
    // 鼠标事件
    var boxEl = document.querySelector(".box")

    boxEl.onclick = function() {
      console.log("click")
    }
    boxEl.oncontextmenu = function(event) {
      console.log("点击了右键")
      event.preventDefault()
    }

    // 变量记录鼠标是否是点下去的
    var isDown = false
    boxEl.onmousedown = function() {
      console.log("鼠标按下去")
      isDown = true
    }

    boxEl.onmouseup = function() {
      console.log("鼠标抬起来")
      isDown = false
    }

    boxEl.onmousemove = function() {
      if (isDown) {
        console.log("鼠标在div上面移动")
      }
    }
  </script>
```

```javascript
  <body>
    <div class="box">
      <span></span>
    </div>
    <script>
      var boxEl = document.querySelector(".box");
      var spanEl = document.querySelector("span");

      // 1.第一组
      boxEl.onmouseenter = function () {
        console.log("box onmouseenter");
      };
      boxEl.onmouseleave = function () {
        console.log("box onmouseleave");
      };

      spanEl.onmouseenter = function () {
        console.log("span onmouseenter");
      };
      spanEl.onmouseleave = function () {
        console.log("span onmouseleave");
      };

      // 第二组
      boxEl.onmouseover = function () {
        console.log("box onmouseover");
      };
      boxEl.onmouseout = function () {
        console.log("box onmouseout");
      };
    </script>
  </body>
```

### 6.3、常见的键盘事件

| 属性       | 描述                 |
| ---------- | -------------------- |
| onkeydown  | 某个键盘按键被按下。 |
| onkeypress | 某个键盘按键被按下。 |
| onkeyup    | 某个键盘按键被松开。 |

事件的执行顺序是 onkeydown、onkeypress、onkeyup 

- down事件先发生；
- press发生在文本被输入；
- up发生在文本输入完成； 

我们可以通过key和code来区分按下的键：

- code：“按键代码”（"KeyA"，"ArrowLeft" 等），特定于键盘上按键的物理位置。
- key：字符（"A"，"a" 等），对于非字符（non-character）的按键，通常具有与 code 相同的值。）

### 6.4、常见的表单事件

| 属性     | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| onchange | 该事件在表单元素的内容改变时触发( <input>, <keygen>,<br/><select>, 和 <textarea>) |
| oninput  | 元素获取用户输入时触发                                       |
| onfocus  | 元素获取焦点时触发                                           |
| onblur   | 元素失去焦点时触发                                           |
| onreset  | 表单重置时触发                                               |
| onsubmit | 表单提交时触发                                               |

### 6.5、文档加载事件

- DOMContentLoaded：浏览器已完全加载 HTML，并构建了 DOM 树，但像 img和样式表之类的外部资源可能尚未加载 完成。

-  load：浏览器不仅加载完成了 HTML，还加载完成了所有外部资源：图片，样式等。

事件类型：https://developer.mozilla.org/zh-CN/docs/Web/Events

```javascript
  <body>
    <script>
      // 注册事件监听
      window.addEventListener("DOMContentLoaded", function () {
        // 1.这里可以操作box, box已经加载完毕
        var boxEl = document.querySelector(".box");
        boxEl.style.backgroundColor = "orange";
        console.log("HTML内容加载完毕");

        // 2.获取img对应的图片的宽度和高度
        var imgEl = document.querySelector("img");
        console.log("图片的宽度和高度:", imgEl.offsetWidth, imgEl.offsetHeight);
      });

      window.onload = function () {
        console.log("文档中所有资源都加载完毕");
        var imgEl = document.querySelector("img");
        console.log("图片的宽度和高度:", imgEl.offsetWidth, imgEl.offsetHeight);
      };

      window.onresize = function () {
        console.log("窗口大小发生改变时");
      };
    </script>
    <div class="box">
      <p>哈哈哈啊</p>
    </div>
    <a href="#">百度一下</a>
    <img src="../images/kobe01.jpg" alt="" />
  </body>
```







