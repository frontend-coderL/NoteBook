亮点：

- 解决字体 fallback 时因 x-height 差异导致的文字视觉大小不一致问题
- 2024 年 7 月成为 Baseline 新可用属性，现代浏览器已广泛支持
- 一行 CSS 属性即可统一多字体场景下的可读性，无需手动微调每个回退字体
- 配合 `@supports` 特性检测和 JS 检测脚本，可做到渐进增强

## 核心问题

当首选字体不可用时，浏览器使用回退字体，但不同字体的小写字母高度（x-height）差异会导致文字视觉大小不一致，影响可读性和页面布局。

## 语法与计算

```CSS
font-size-adjust: none | <number> | inherit;
```

- `none`：默认值，不调整
- `<number>`：指定宽高比值（x-height / font-size），浏览器据此调整回退字体大小
- `inherit`：继承父元素值

常见字体宽高比参考：

| 字体类型            | 宽高比   |
| --------------- | ----- |
| 无衬线（sans-serif） | 约 0.5 |
| 衬线（serif）       | 约 0.4 |
| 等宽（monospace）   | 约 0.6 |

计算公式：

```
实际渲染字体大小 = font-size × (font-size-adjust / 回退字体的宽高比)
```

## 实用场景

### 场景 1：正文字体回退可读性

```CSS
.article {
  font-family: "Source Sans Pro", "Microsoft YaHei", sans-serif;
  font-size: 16px;
  font-size-adjust: 0.49; /* Source Sans Pro 的宽高比 */
}
/* 如果没有 Source Sans Pro，Microsoft YaHei 会按比例调整到视觉相似大小 */
```

### 场景 2：标题字体一致性

```CSS
/* Arial (宽高比 0.52) → Times New Roman (宽高比 0.45) */
h1 {
  font-size: 24px;
  font-family: Arial, "Times New Roman", serif;
  font-size-adjust: 0.52;
}
/* Times New Roman 调整后大小 = 24px × (0.52 / 0.45) ≈ 27.7px */
```

### 场景 3：图标字体大小统一

```CSS
.icon-text {
  font-family: "Material Icons", "Font Awesome", sans-serif;
  font-size: 24px;
  font-size-adjust: 0.48;
}
```

### 场景 4：响应式设计配合 CSS 变量

```CSS
:root {
  --font-aspect-ratio: 0.48;
}

@media (max-width: 768px) {
  .content {
    font-size: 14px;
    font-size-adjust: var(--font-aspect-ratio);
  }
}
```

### 场景 5：系统字体栈通用适配

覆盖全平台系统字体（macOS / Windows / Linux / Android），统一排版效果：

```CSS
body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  font-size-adjust: 0.48; /* 系统字体通用平均值 */
}
```

## 渐进增强方案

### CSS @supports 检测

```CSS
.text {
  font-family: "Modern Font", "Legacy Font", sans-serif;
  font-size: 18px;
  font-size-adjust: 0.5;
  letter-spacing: 0.02em; /* 不支持时的备用方案 */
}

@supports (font-size-adjust: 0.5) {
  .text {
    letter-spacing: normal; /* 关闭备用方案 */
  }
}
```

### JavaScript 检测与回退

```JavaScript
const isFontSizeAdjustSupported = CSS.supports('font-size-adjust', '0.5')

if (!isFontSizeAdjustSupported) {
  document.documentElement.classList.add('no-font-size-adjust')
}
```

```CSS
.no-font-size-adjust .text {
  line-height: 1.6;        /* 增加行高补偿 */
  letter-spacing: 0.01em;
}
```

## 工具：自动检测字体宽高比

```JavaScript
function getFontAspect(fontFamily) {
  const canvas = document.createElement('canvas')
  const ctx = canvas.getContext('2d')
  ctx.font = `100px ${fontFamily}`
  const metrics = ctx.measureText('x')
  return metrics.actualBoundingBoxAscent / 100
}

// 自动检测并应用最佳比例
function optimizeFontAdjust() {
  const prefers = getComputedStyle(document.body)
    .getPropertyValue('font-family')
    .split(',')[0].trim()
  if (prefers) {
    const aspect = getFontAspect(prefers)
    document.documentElement.style.setProperty('--font-aspect', aspect)
  }
}
```

