## 1、computed计算属性使用

### 1.1、复杂data的处理方式

我们知道，在模板中可以直接通过插值语法显示一些data中的数据。

但是在某些情况，我们可能需要对数据进行一些转化后再显示，或者需要将多个数据结合起来进行显示；

- 比如我们需要对多个data数据进行运算、三元运算符来决定结果、数据进行某种转化后显示；
- 在模板中使用表达式，可以非常方便的实现，但是设计它们的初衷是用于简单的运算；
- 在模板中放入太多的逻辑会让模板过重和难以维护；
- 并且如果多个地方都使用到，那么会有大量重复的代码；

我们有没有什么方法可以将逻辑抽离出去呢？

- 可以，其中一种方式就是将逻辑抽取到一个method中，放到methods的options中；
- 但是，这种做法有一个直观的弊端，就是所有的data使用过程都会变成了一个方法的调用；
- 另外一种方式就是使用计算属性computed；

```html
<div id="app">
  <!-- 插值语法表达式直接进行拼接 -->
  <!-- 1.拼接名字 -->
  <h2>{{ firstName + " " + lastName }}</h2>
  <h2>{{ firstName + " " + lastName }}</h2>
  <!-- method实现 -->
  <h2>{{ getFullname() }}</h2>
  <h2>{{ getFullname() }}</h2>

  <!-- 2.显示分数等级 -->
  <h2>{{ score >= 60 ? '及格': '不及格' }}</h2>
  <!-- method实现 -->
  <h2>{{ getScoreLevel() }}</h2>

  <!-- 3.反转单词显示文本 -->
  <h2>{{ message.split(" ").reverse().join(" ") }}</h2>
  <!-- method实现 -->
  <h2>{{ reverseMessage() }}</h2>
</div>
```

```js
const app = Vue.createApp({
  data() {
    return {
      // 1.姓名
      firstName: "kobe",
      lastName: "bryant",

      // 2.分数: 及格/不及格
      score: 80,

      // 3.一串文本: 对文本中的单词进行反转显示
      message: "my name is why",
    };
  },
  methods: {
    getFullname() {
      return this.firstName + " " + this.lastName;
    },
    getScoreLevel() {
      return this.score >= 60 ? "及格" : "不及格";
    },
    reverseMessage() {
      return this.message.split(" ").reverse().join(" ");
    },
  },
});
app.mount("#app");
```

### 1.2、认识计算属性computed

什么是计算属性呢？

- 官方并没有给出直接的概念解释；
- 而是说：**对于任何包含响应式数据的复杂逻辑，你都应该使用计算属性；**
- 计算属性将被混入到组件实例中
  - 所有 getter 和 setter 的 this 上下文自动地绑定为组件实例；

计算属性的用法：

- 选项：computed
- 类型：{ [key: string]: Function | { get: Function, set: Function } }

```html
<div id="app">
  <!-- 插值语法表达式直接进行拼接 -->
  <!-- 1.拼接名字 -->
  <h2>{{ fullname }}</h2>
  <h2>{{ fullname }}</h2>

  <!-- 2.显示分数等级 -->
  <h2>{{ scoreLevel }}</h2>

  <!-- 3.反转单词显示文本 -->
  <h2>{{ reverseMessage }}</h2>
</div>
```

```js
const app = Vue.createApp({
  data() {
    return {
      // 1.姓名
      firstName: "kobe",
      lastName: "bryant",

      // 2.分数: 及格/不及格
      score: 80,

      // 3.一串文本: 对文本中的单词进行反转显示
      message: "my name is why",
    };
  },
  computed: {
    // 1.计算属性默认对应的是一个函数
    fullname() {
      return this.firstName + " " + this.lastName;
    },
    scoreLevel() {
      return this.score >= 60 ? "及格" : "不及格";
    },
    reverseMessage() {
      return this.message.split(" ").reverse().join(" ");
    },
  },
});
app.mount("#app");
```

## 2、computed和method区别

### 2.1、计算属性 vs methods

在上面的实现思路中，我们会发现计算属性和methods的实现看起来是差别是不大的，而且我们多次提到计算属性有缓存的。

接下来我们来看一下同一个计算多次使用，计算属性和methods的差异：

```html
<div id="app">
  <!-- 1.methods -->
  <h2>{{ getFullname() }}</h2>
  <h2>{{ getFullname() }}</h2>
  <h2>{{ getFullname() }}</h2>

  <!-- 2.computed -->
  <h2>{{ fullname }}</h2>
  <h2>{{ fullname }}</h2>
  <h2>{{ fullname }}</h2>

  <!-- 修改name值 -->
  <button @click="changeLastname">修改lastname</button>
</div>
```

```js
const app = Vue.createApp({
  data() {
    return {
      firstName: "kobe",
      lastName: "bryant",
    };
  },
  methods: {
    getFullname() {
      console.log("getFullname-----");
      return this.firstName + " " + this.lastName;
    },
    changeLastname() {
      this.lastName = "why";
    },
  },
  computed: {
    fullname() {
      console.log("computed fullname-----");
      return this.firstName + " " + this.lastName;
    },
  },
});
app.mount("#app");
```

![image-20230420221250197](.\img\image-20230420221250197.png)

### 2.2、计算属性的缓存

这是什么原因呢？

- 这是因为计算属性会基于它们的依赖关系进行缓存；
- 在数据不发生变化时，计算属性是不需要重新计算的；
- 但是如果依赖的数据发生变化，在使用时，计算属性依然会重新进行计算；

## 3、computed的set和get

### 3.1、计算属性的setter和getter

> 计算属性在大多数情况下，只需要一个getter方法即可，所以我们会将计算属性直接写成一个函数。

但是，如果我们确实想设置计算属性的值呢？

- 这个时候我们也可以给计算属性设置一个setter的方法；

```js
data() {
  return {
    firstname: "coder",
    lastname: "why",
  };
},
computed: {
  // 语法糖的写法
  fullnameCopy() {
    return this.firstname + " " + this.lastname;
  },

  // 完整的写法:
  fullname: {
    get: function () {
      return this.firstname + " " + this.lastname;
    },
    set: function (value) {
      const names = value.split(" ");
      this.firstname = names[0];
      this.lastname = names[1];
    },
  },
},
methods: {
  setFullname() {
    this.fullname = "kobe bryant";
  },
},
```

### 3.2、源码如何对setter和getter处理呢？（了解）

你可能觉得很奇怪，Vue内部是如何对我们传入的是一个getter，还是说是一个包含setter和getter的对象进行处理的呢？

- 事实上非常的简单，Vue源码内部只是做了一个逻辑判断而已；

![image-20230420221927578](.\img\image-20230420221927578.png)

## 4、侦听器watch选项使用

### 4.1、认识侦听器watch

什么是侦听器呢？

- 开发中我们在data返回的对象中定义了数据，这个数据通过插值语法等方式绑定到template中；
- 当数据变化时，template会自动进行更新来显示最新的数据；
- 但是在某些情况下，我们希望在代码逻辑中监听某个数据的变化，这个时候就需要用侦听器watch来完成了；

侦听器的用法如下：

- 选项：watch 
- 类型：{ [key: string]: string | Function | Object | Array}

```js
// Proxy -> Reflect
const app = Vue.createApp({
  data() {
    return {
      message: "Hello Vue",
      info: { name: "why", age: 18 },
    };
  },
  methods: {
    changeMessage() {
      this.message = "你好啊, 李银河!";
      this.info = { name: "kobe" };
    },
  },
  watch: {
    // 1.默认有两个参数: newValue/oldValue
    message(newValue, oldValue) {
      console.log("message数据发生了变化:", newValue, oldValue);
    },
    info(newValue, oldValue) {
      // 2.如果是对象类型, 那么拿到的是代理对象
      // console.log("info数据发生了变化:", newValue, oldValue)
      // console.log(newValue.name, oldValue.name)

      // 3.获取原生对象
      console.log({ ...newValue })
      console.log(Vue.toRaw(newValue));
    },
  },
});
app.mount("#app");
```

### 4.2、侦听器watch的配置选项

我们先来看一个例子：

- 当我们点击按钮的时候会修改info.name的值；
- 这个时候我们使用watch来侦听info，可以侦听到吗？答案是不可以。

这是因为默认情况下，watch只是在侦听info的引用变化，对于内部属性的变化是不会做出响应的：

- 这个时候我们可以使用一个选项deep进行更深层的侦听；
- 注意前面我们说过watch里面侦听的属性对应的也可以是一个Object；

还有另外一个属性，是希望一开始的就会立即执行一次：

- 这个时候我们使用immediate选项；
- 这个时候无论后面数据是否有变化，侦听的函数都会有限执行一次；

```js
const app = Vue.createApp({
  data() {
    return {
      info: { name: "why", age: 18 },
    };
  },
  methods: {
    changeInfo() {
      // 1.创建一个新对象, 赋值给info
      // this.info = { name: "kobe" }

      // 2.直接修改原对象某一个属性
      this.info.name = "kobe";
    },
  },
  watch: {
    // 默认watch监听不会进行深度监听
    // info(newValue, oldValue) {
    //   console.log("侦听到info改变:", newValue, oldValue)
    // }

    // 进行深度监听
    info: {
      handler(newValue, oldValue) {
        console.log("侦听到info改变:", newValue, oldValue);
        console.log(newValue === oldValue);
      },
      // 监听器选项:
      // info进行深度监听
      deep: true,
      // 第一次渲染直接执行一次监听器
      immediate: true,
    },
    "info.name": function (newValue, oldValue) {
      console.log("name发生改变:", newValue, oldValue);
    },
  },
});
app.mount("#app");
```

## 5、侦听器watch其他写法

### 5.1、侦听器watch的其他方式（一）

![image-20230420223415239](.\img\image-20230420223415239.png)

![image-20230420223431426](.\img\image-20230420223431426.png)

### 5.2、侦听器watch的其他方式（二）

另外一个是Vue3文档中没有提到的，但是Vue2文档中有提到的是侦听对象的属性：

![image-20230420223526291](.\img\image-20230420223526291.png)

还有另外一种方式就是使用 $watch 的API：

我们可以在created的生命周期中，使用 this.$watchs 来侦听；

- 第一个参数是要侦听的源；
- 第二个参数是侦听的回调函数callback；
- 第三个参数是额外的其他选项，比如deep、immediate；

```js
const app = Vue.createApp({
  data() {
    return {
      message: "Hello Vue",
    };
  },
  methods: {
    changeMessage() {
      this.message = "你好啊, 李银河!";
    },
  },
  // 生命周期回调函数: 当前的组件被创建时自动执行
  // 一般在该函数中, 会进行网络请求
  created() {
    // ajax/fetch/axios
    console.log("created");

    this.$watch(
      "message",
      (newValue, oldValue) => {
        console.log("message数据变化:", newValue, oldValue);
      },
      { deep: true }
    );
  },
});
app.mount("#app");
```

## 6、阶段性综合案例练习

![image-20230420223949871](.\img\image-20230420223949871.png)

案例说明：

- 1.在界面上以表格的形式，显示一些书籍的数据；
- 2.在底部显示书籍的总价格；
- 3.点击+或者-可以增加或减少书籍数量（如果为1，那么不能继续-）；
- 4.点击移除按钮，可以将书籍移除（当所有的书籍移除完毕时，显示：购物车为空~）；

```css
table {
  border-collapse: collapse;
  /* text-align: center; */
}

thead {
  background-color: #f5f5f5;
}

th,
td {
  border: 1px solid #aaa;
  padding: 8px 16px;
}

.active {
  background-color: skyblue;
}
```

```html
<div id="app">
  <!-- 1.搭建界面内容 -->
  <template v-if="books.length">
    <table>
      <thead>
        <tr>
          <th>序号</th>
          <th>书籍名称</th>
          <th>出版日期</th>
          <th>价格</th>
          <th>购买数量</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(item, index) in books"
          :key="item.id"
          @click="rowClick(index)"
          :class="{ active: index === currentIndex }"
        >
          <td>{{ index + 1 }}</td>
          <td>{{ item.name }}</td>
          <td>{{ item.date }}</td>
          <td>{{ formatPrice(item.price) }}</td>
          <td>
            <button
              :disabled="item.count <= 1"
              @click="decrement(index, item)"
            >
              -
            </button>
            {{ item.count }}
            <button @click="increment(index, item)">+</button>
          </td>
          <td>
            <button @click="removeBook(index, item)">移除</button>
          </td>
        </tr>
      </tbody>
    </table>

    <h2>总价: {{ formatPrice(totalPrice) }}</h2>
  </template>

  <template v-else>
    <h1>购物车为空, 请添加喜欢的书籍~</h1>
    <p>商场中有大量的IT类的书籍, 请选择添加学习, 注意保护好自己的头发!</p>
  </template>
</div>

```

```js
const books = [
  {
    id: 1,
    name: '《算法导论》',
    date: '2006-9',
    price: 85.00,
    count: 1
  },
  {
    id: 2,
    name: '《UNIX编程艺术》',
    date: '2006-2',
    price: 59.00,
    count: 1
  },
  {
    id: 3,
    name: '《编程珠玑》',
    date: '2008-10',
    price: 39.00,
    count: 1
  },
  {
    id: 4,
    name: '《代码大全》',
    date: '2006-3',
    price: 128.00,
    count: 1
  },
  {
    id: 5,
    name: '《你不知道JavaScript》',
    date: '2014-8',
    price: 88.00,
    count: 1
  },
]

const app = Vue.createApp({
  data() {
    return {
      books: books,
      currentIndex: 0,
    };
  },
  // computed
  computed: {
    totalPrice() {
      // 1.直接遍历books
      // let price = 0
      // for (const item of this.books) {
      //   price += item.price * item.count
      // }
      // return price

      // 2.reduce(自己决定)
      return this.books.reduce((preValue, item) => {
        return preValue + item.price * item.count;
      }, 0);
    },
  },
  methods: {
    formatPrice(price) {
      return "¥" + price;
    },

    // 监听-和+操作
    decrement(index, item) {
      console.log("点击-");
      // this.books[index].count--
      item.count--;
    },
    increment(index, item) {
      console.log("点击+:", index);
      // this.books[index].count++
      item.count++;
    },
    removeBook(index, item) {
      this.books.splice(index, 1);
    },
    rowClick(index) {
      this.currentIndex = index;
    },
  },
});
app.mount("#app");
```











