### 核心概念

异步组件在需要渲染时才从服务器按需加载，「用户用到哪，加载到哪」，从根源上减少初始加载的资源体积。

核心价值：

- 减少首屏加载时间，提升 FCP/LCP 指标
- 精准优化带宽使用，只加载用户需要的资源
- 提升应用整体响应速度
- 实现代码分割，让大型项目维护更轻松

加载流程：应用初始化 → 加载核心代码 → 渲染首屏 → 用户交互触发 → 加载异步组件 → 渲染

### 三种注册方式

#### 1. 全局注册（全应用通用组件）

```javascript
import { createApp, defineAsyncComponent } from 'vue'
const app = createApp({})

app.component('AsyncComponent', defineAsyncComponent(() =>
  import('./components/AsyncComponent.vue')
))
```

#### 2. 局部注册（单组件内专用，最常用）

```vue
<script setup>
import { defineAsyncComponent } from 'vue'
const AsyncComponent = defineAsyncComponent(() =>
  import('./components/AsyncComponent.vue')
)
</script>

<template>
  <AsyncComponent />
</template>
```

#### 3. 条件渲染组件（v-if 场景）

```vue
<script setup>
import { defineAsyncComponent } from 'vue'
const AdminPanel = defineAsyncComponent(() =>
  import('./components/AdminPanel.vue')
)
</script>

<template>
  <div v-if="isAdmin">
    <AdminPanel />
  </div>
</template>
```

### 加载 / 错误状态处理

```javascript
import { defineAsyncComponent } from 'vue'
import LoadingComponent from './LoadingComponent.vue'
import ErrorComponent from './ErrorComponent.vue'

const AsyncComponent = defineAsyncComponent({
  loader: () => import('./components/AsyncComponent.vue'),
  loadingComponent: LoadingComponent,
  errorComponent: ErrorComponent,
  delay: 200,      // 延迟 200ms 显示加载组件，避免快速加载时闪烁
  timeout: 3000    // 3 秒超时后显示错误组件
})
```

状态判断逻辑：

1. 触发渲染 → 开始加载
2. 加载超 200ms？→ 显示加载组件 / 直接渲染
3. 加载成功？→ 渲染组件 / 超时？→ 显示错误组件

### 懒加载实战场景

#### 路由懒加载（首屏优化第一招）

```javascript
import { createRouter, createWebHistory } from 'vue-router'

const router = createRouter({
  history: createWebHistory(),
  routes: [
    {
      path: '/',
      component: () => import('../views/HomeView.vue')
    },
    {
      path: '/about',
      component: () => import('../views/AboutView.vue')
    }
  ]
})
```

#### 组件级懒加载（复杂功能按需拆分）

```vue
<script setup>
import { defineAsyncComponent } from 'vue'
const ChartComponent = defineAsyncComponent(() =>
  import('./ChartComponent.vue')
)
</script>

<template>
  <button @click="showChart = true">显示图表</button>
  <ChartComponent v-if="showChart" />
</template>
```

### Suspense + 异步组件

替代单独的 loading 组件，更优雅地管理加载状态：

```vue
<script setup>
import { defineAsyncComponent } from 'vue'
const AsyncComponent = defineAsyncComponent(() =>
  import('./components/AsyncComponent.vue')
)
</script>

<template>
  <Suspense>
    <template #default>
      <AsyncComponent />
    </template>
    <template #fallback>
      <div>加载中...</div>
    </template>
  </Suspense>
</template>
```

### SSR 懒水化策略

懒水化：延迟组件的水化过程，直到特定条件满足后再执行。减少首屏 JS 执行时间。

#### 1. 空闲时水化（非核心组件首选）

```javascript
import { defineAsyncComponent, hydrateOnIdle } from 'vue'

const AsyncComp = defineAsyncComponent({
  loader: () => import('./Comp.vue'),
  hydrate: hydrateOnIdle(5000) // 浏览器空闲时水化，5 秒超时
})
```

#### 2. 可见时水化（首屏下方组件）

```javascript
import { defineAsyncComponent, hydrateOnVisible } from 'vue'

const AsyncComp = defineAsyncComponent({
  loader: () => import('./Comp.vue'),
  hydrate: hydrateOnVisible({ rootMargin: '100px' }) // 进入视口时水化
})
```

#### 3. 媒体查询匹配时水化（适配多端）

```javascript
import { defineAsyncComponent, hydrateOnMediaQuery } from 'vue'

const AsyncComp = defineAsyncComponent({
  loader: () => import('./Comp.vue'),
  hydrate: hydrateOnMediaQuery('(max-width:500px)')
})
```

#### 4. 交互时水化（点击 / 悬浮组件）

```javascript
import { defineAsyncComponent, hydrateOnInteraction } from 'vue'

const AsyncComp = defineAsyncComponent({
  loader: () => import('./Comp.vue'),
  hydrate: hydrateOnInteraction(['click', 'mouseover'])
})
```

### 常见错误排查

| 错误                                           | 原因                | 解决                                  |
| -------------------------------------------- | ----------------- | ----------------------------------- |
| Failed to fetch dynamically imported module  | 路径错误或文件不存在        | 检查导入路径拼写、大小写、后缀                     |
| Timeout exceeded for async component         | 网络问题或组件过大         | 增加 timeout、优化组件体积                   |
| Hydration completed but contains mismatches  | SSR 与客户端 HTML 不一致 | 确保服务端/客户端渲染逻辑一致，用 `v-cloak` 隐藏未水化内容 |
| Async component loader must return a Promise | 加载函数未返回 Promise   | 直接使用 `import()` 语法                  |

### 优化核心逻辑

按需——按需加载、按需水化、按需渲染。让应用「只加载用户需要的资源，只执行必要的代码」。
