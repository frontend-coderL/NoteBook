浏览器原生页面切换动画 API，无需复杂路由动画或第三方库。

### 工作原理

1. 页面切换时，浏览器自动捕捉当前页面状态（旧视图）
2. 执行 DOM 更新（路由跳转、内容替换等）
3. 捕捉更新后的新页面状态（新视图）
4. 自动在两帧之间插值，生成平滑过渡动画

核心优势：无需手写 CSS 动画、自动匹配新旧视图中的相同元素、浏览器底层优化。

### 基础用法

```javascript
// 默认淡入淡出
async function navigateTo(page) {
  document.startViewTransition(() => {
    window.location = page;
  });
}
```

#### 配合 React Router

```javascript
import { useNavigate } from 'react-router-dom';

function App() {
  const navigate = useNavigate();

  async function handleNavigate(path) {
    if (document.startViewTransition) {
      await document.startViewTransition(() => {
        navigate(path);
      }).finished;
    } else {
      navigate(path);
    }
  }

  return (
    <nav>
      <button onClick={() => handleNavigate('/')}>首页</button>
      <button onClick={() => handleNavigate('/about')}>关于</button>
    </nav>
  );
}
```

#### 配合 Vue Router

```javascript
import { useRouter } from 'vue-router';

export default {
  setup() {
    const router = useRouter();

    async function navigate(path) {
      if (document.startViewTransition) {
        await document.startViewTransition(() => {
          router.push(path);
        }).finished;
      } else {
        router.push(path);
      }
    }

    return { navigate };
  }
};
```

### 自定义元素匹配

用 `data-view-transition-name` 标记同一逻辑元素，让它们在切换时保持位置：

```html
<!-- 旧页面 -->
<div class="header" data-view-transition-name="header">
  <h1>首页标题</h1>
</div>

<!-- 新页面 -->
<div class="header" data-view-transition-name="header">
  <h1>关于页面</h1>
</div>
```

效果：header 保持位置不变，文字内容平滑过渡；未标记元素执行默认淡入淡出。

### 自定义过渡动画（CSS 伪元素）

```css
::view-transition-old(root),
::view-transition-new(root) {
  animation-duration: 0.3s;
}

/* 旧视图：从左边滑出 */
::view-transition-old(root) {
  animation: slide-out-left 0.3s ease-in-out;
}

/* 新视图：从右边滑入 */
::view-transition-new(root) {
  animation: slide-in-right 0.3s ease-in-out;
}

@keyframes slide-out-left {
  from { transform: translateX(0); opacity: 1; }
  to   { transform: translateX(-30px); opacity: 0; }
}

@keyframes slide-in-right {
  from { transform: translateX(30px); opacity: 0; }
  to   { transform: translateX(0); opacity: 1; }
}
```

### 实战：图片画廊全屏切换

```html
<style>
  .gallery {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 16px;
  }
  .fullscreen {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.9);
    z-index: 1000;
    align-items: center;
    justify-content: center;
  }
  .fullscreen img { max-width: 90%; max-height: 90%; }
</style>

<div class="gallery">
  <img src="photo1.jpg" data-view-transition-name="photo1" onclick="openFullscreen('photo1')">
  <img src="photo2.jpg" data-view-transition-name="photo2" onclick="openFullscreen('photo2')">
</div>

<div class="fullscreen" id="fullscreen">
  <img id="preview-img" data-view-transition-name="preview">
</div>

<script>
function openFullscreen(name) {
  document.startViewTransition(() => {
    document.querySelector('.fullscreen').style.display = 'flex';
    document.getElementById('preview-img').src = name + '.jpg';
    document.getElementById('preview-img').dataset.viewTransitionName = name;
  });
}

function closeFullscreen() {
  document.startViewTransition(() => {
    document.querySelector('.fullscreen').style.display = 'none';
  });
}
</script>
```

### 兼容性

| 浏览器         | 支持情况      |
| ----------- | --------- |
| Chrome 111+ | ✅         |
| Edge 111+   | ✅         |
| Safari 18+  | ✅         |
| Firefox     | ⚠️ 需实验性启用 |

降级方案：

```javascript
function supportsViewTransitions() {
  return 'startViewTransition' in document;
}

async function navigate(path) {
  if (supportsViewTransitions()) {
    await document.startViewTransition(() => {
      window.location = path;
    }).finished;
  } else {
    window.location = path;
  }
}
```

### 性能优化

- 仅在路由切换时使用，不要在局部内容更新时滥用
- DOM 操作简单直接，避免在回调内做复杂异步操作（会导致动画卡顿）
- 动画时长控制在 300ms 以内

### 常见问题

取消动画：调用 `transition.skipTransition()`

```javascript
const transition = document.startViewTransition(() => {
  window.location = '/new';
});
transition.skipTransition();
```

支持的 CSS 动画属性：`transform`、`opacity`、`filter`

不支持：`width` / `height`（会导致布局变化）、`position`（与 transform 冲突）

SSR 应用：在客户端 hydration 后使用 `useEffect` 检测 API 可用性再挂载。
