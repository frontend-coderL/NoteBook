亮点：

- 9 个现代浏览器原生 API，覆盖懒加载、空闲任务、动画、尺寸监听、资源预加载、离线缓存、后台计算、页面节流等场景
- 零依赖，无需第三方库，直接调用浏览器能力即可显著提升性能
- IntersectionObserver + preload 组合可立竿见影降低首屏加载时间
- 每个 API 附带可直接复制的代码示例

## API 速查

| API                        | 核心场景  | 关键优势                       |
| -------------------------- | ----- | -------------------------- |
| IntersectionObserver       | 懒加载   | 浏览器原生监听，无重排重绘              |
| requestIdleCallback        | 空闲任务  | 不抢占主线程，交互更跟手               |
| requestAnimationFrame      | 动画    | 与屏幕刷新率同步，自动暂停              |
| ResizeObserver             | 尺寸监听  | 精确监听任意元素，替代 window\.resize |
| performance.now()          | 性能测量  | 微秒精度，不受系统时间干扰              |
| preload / prefetch         | 资源预加载 | 关键资源优先 / 未来资源空闲加载          |
| Cache API + Service Worker | 离线缓存  | 客户端缓存，断网可用                 |
| Web Workers                | 后台计算  | 重任务移出主线程，页面不卡死             |
| visibilityState            | 页面节流  | 不可见时暂停轮询 / 动画 / 视频         |

## 1. IntersectionObserver — 懒加载

替代传统的 scroll 事件监听，交给浏览器原生监听元素可见性：

```JavaScript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target
      img.src = img.dataset.src
      observer.unobserve(img)
    }
  })
})

document.querySelectorAll('img[data-src]').forEach(img => {
  observer.observe(img)
})
```

效果：首屏加载时间降低约 40%，滚动丝滑，CPU 占用降低。

## 2. requestIdleCallback — 空闲任务

将非关键任务（埋点上报、预加载、清理缓存）延迟到浏览器空闲时执行：

```JavaScript
requestIdleCallback(() => {
  sendAnalytics()     // 埋点上报
  preloadNextPage()   // 预加载下一页资源
})
```

不抢占主线程，页面交互更跟手。

## 3. requestAnimationFrame — 流畅动画

替代 setTimeout，与屏幕刷新率（60fps / 120fps）同步，页面不可见时自动暂停：

```JavaScript
function animate() {
  element.style.transform = `translateX(${x}px)`
  if (x < 200) {
    requestAnimationFrame(animate)
  }
}
requestAnimationFrame(animate)
```

## 4. ResizeObserver — 元素尺寸监听

精确监听任意元素的宽高变化，适合图表、自适应容器：

```JavaScript
const observer = new ResizeObserver(entries => {
  entries.forEach(entry => {
    console.log('新尺寸:', entry.contentRect)
    // 调整子元素布局或重绘图表
  })
})

observer.observe(document.getElementById('chart-container'))
```

## 5. performance.now() — 精准计时

高精度时间戳（微秒级），不受系统时间干扰：

```JavaScript
const start = performance.now()
heavyCalculation()
const end = performance.now()
console.log(`耗时: ${end - start}ms`)
```

## 6. preload / prefetch — 资源预加载

`preload` 用于首屏关键资源，浏览器优先加载：

```HTML
<link rel="preload" href="critical.css" as="style">
<link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin>
```

`prefetch` 用于未来可能用到的资源，空闲时预加载：

```HTML
<link rel="prefetch" href="/user/profile.js">
```

两者配合使用，首屏 + 页面跳转体验显著提升。

## 7. Cache API + Service Worker — 离线缓存

将静态资源缓存到客户端，第二次访问直接读缓存，断网也能打开核心页面：

```JavaScript
// service-worker.js
self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request).then(cached => {
      return cached || fetch(event.request)
    })
  )
})
```

## 8. Web Workers — 后台计算

将重计算（大数据处理、复杂排序）移到后台线程，主线程不卡死：

```JavaScript
// main.js
const worker = new Worker('worker.js')
worker.postMessage(data)
worker.onmessage = (e) => {
  console.log('处理完成:', e.data)
}

// worker.js
self.onmessage = function(e) {
  const result = heavyProcess(e.data)
  self.postMessage(result)
}
```

## 9. visibilityState — 页面节流

页面不可见时暂停轮询、视频、动画，回来再恢复，省电省流量：

```JavaScript
document.addEventListener('visibilitychange', () => {
  if (document.visibilityState === 'hidden') {
    stopVideo()
    stopPolling()
  } else {
    resumeVideo()
  }
})
```

