来源：[Vue3 内置组件 TransitionGroup：让你的列表动画如丝般顺滑](https://mp.weixin.qq.com/s/...)

Vue3 将 TransitionGroup 纳入核心的三重考量：

- 开发者体验：列表是高频场景（后台管理系统中列表组件占比超 40%），内置 = 零依赖、开箱即用
- 性能与一致性：针对 FLIP 动画（First, Last, Invert, Play）深度优化，大量数据变动依然流畅
- 统一心智模型：继承 Transition 的类名规则（-enter、-leave、-move），学会一个另一个自然上手

Demo 地址：<https://gitee.com/benxiaohai1071/bxh-admin-vue3/tree/master/src/views/study/builtIncomponent/transitionGroup>

高频场景：基础列表增删、列表排序、网格布局、标签页切换、消息通知队列、拖拽排序。

### 核心用法：三步上手

#### 第一步：包裹列表

用 `<TransitionGroup>` 替换父容器，指定 tag 和 name：

```vue
<TransitionGroup name="list" tag="ul">
  <li v-for="item in items" :key="item.id">{{ item.name }}</li>
</TransitionGroup>
```

列表项必须绑定唯一 `:key`，这是 Vue 识别元素身份、执行动画的前提。

#### 第二步：定义动画 CSS

Vue 自动注入的类名：

| 类名                   | 触发时机       |
| -------------------- | ---------- |
| `.list-enter-from`   | 元素插入前      |
| `.list-enter-active` | 元素插入过程中    |
| `.list-enter-to`     | 元素插入完成     |
| `.list-leave-from`   | 元素离开前      |
| `.list-leave-active` | 元素离开过程中    |
| `.list-leave-to`     | 元素离开完成     |
| `.list-move`         | 位置变化时（核心！） |

```css
/* 进入动画 */
.list-enter-active {
  transition: all 0.4s ease;
}
.list-enter-from {
  opacity: 0;
  transform: translateX(-30px);
}

/* 离开动画 */
.list-leave-active {
  transition: all 0.4s ease;
  position: absolute; /* 关键：脱离文档流，让其他元素平滑移动 */
  width: 100%;        /* 保持宽度，避免布局抖动 */
}
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

/* 位置变化动画 — TransitionGroup 的灵魂 */
.list-move {
  transition: transform 0.4s ease;
}
```

#### 第三步：操作数据

```javascript
// 添加
items.value.push({ id: Date.now(), name: '新项目' });
// 删除
items.value.splice(index, 1);
// 排序（触发 move 动画）
items.value.sort((a, b) => a.priority - b.priority);
```

### Transition vs TransitionGroup

| 特性  | `<Transition>`           | `<TransitionGroup>`      |
| --- | ------------------------ | ------------------------ |
| 用途  | 单元素 / 组件过渡               | 多元素列表过渡                  |
| 渲染  | 不渲染额外 DOM                | 默认渲染 `<span>`，可通过 tag 指定 |
| 模式  | 支持 mode（in-out / out-in） | 不支持 mode                 |
| 特殊类 | 无                        | 支持 move-class            |

### tag 属性

```vue
<!-- 渲染为 ul -->
<TransitionGroup tag="ul" name="list">
  <li v-for="item in items" :key="item.id">{{ item.text }}</li>
</TransitionGroup>

<!-- 渲染为 div -->
<TransitionGroup tag="div" name="grid" class="grid-container">
  <div v-for="card in cards" :key="card.id">{{ card.title }}</div>
</TransitionGroup>
```

### move-class：自定义位置变化类名

Vue 使用 FLIP 技术自动计算位置差异，只需定义过渡效果：

```css
/* 方式一：默认命名 */
.list-move {
  transition: transform 0.5s ease;
}
```

```vue
<!-- 方式二：自定义类名 -->
<TransitionGroup name="list" move-class="custom-move">
```

### 关键注意事项

#### 离开动画必须 position: absolute

```css
.list-leave-active {
  position: absolute;
  width: 100%; /* 保持宽度，避免布局抖动 */
}
```

#### key 必须唯一，不能用索引

```vue
<!-- 正确：使用唯一 key -->
<li v-for="item in items" :key="item.id">{{ item.name }}</li>

<!-- 错误：用索引当 key，排序时不会触发 move 动画 -->
<li v-for="(item, index) in items" :key="index">{{ item.name }}</li>
```

Vue 默认「就地更新」策略 — 数据项顺序变化时不会移动 DOM 元素，而是更新元素内容。必须提供唯一 `:key` 才能追踪元素身份并触发 FLIP 动画。
