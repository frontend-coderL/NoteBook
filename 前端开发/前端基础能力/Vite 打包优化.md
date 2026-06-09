亮点：

- rollup-plugin-visualizer 可视化分析打包体积，精准定位大文件
- Gzip / Brotli 双压缩，传输体积缩小 60%\~80%
- CDN 外置第三方依赖，打包体积减少 80%+
- manualChunks 手动分包，路由 / 组件按需加载
- 小图片自动转 base64，减少 HTTP 请求

## 环境判断

很多优化只需在生产环境生效（如代码混淆、关闭 sourceMap），开发 / 测试环境保留原始配置更方便调试。

```TypeScript
export default defineConfig(({ mode }) => {
  const env = loadEnv(mode, process.cwd(), 'VITE_')
  const isProd = mode === 'prod'
  return {
    // 所有优化配置写在这里
  }
})
```

## 打包分析

先看清楚打包后哪些文件最大，再针对性优化。

```Shell
npm install rollup-plugin-visualizer -D
```

```TypeScript
import { visualizer } from 'rollup-plugin-visualizer'

export default defineConfig({
  plugins: [
    vue(),
    visualizer({
      open: true,       // 打包后自动打开分析页面
      gzipSize: true,
      brotliSize: true
    })
  ]
})
```

执行 `npm run build` 后生成 `stats.html`，红色越大 = 体积越大 = 优先优化。

## 路由懒加载

Vue 项目最大体积来源通常是全量打包所有页面。

```TypeScript
// ❌ 全量打包
import Home from '@/views/Home.vue'
import About from '@/views/About.vue'

// ✅ 懒加载，访问时才加载对应模块
const routes = [
  { path: '/', component: () => import('@/views/Home.vue') },
  { path: '/about', component: () => import('@/views/About.vue') }
]
```

## 代码压缩 + 混淆

Terser 比默认的 esbuild 压缩率更高，还支持代码混淆，完胜 esbuild（esbuild 胜在速度，但压缩率和安全性不如 Terser）。

```Shell
pnpm add terser -D
```

```TypeScript
export default defineConfig(({ mode }) => {
  const isProd = mode === 'prod'
  return {
    build: {
      minify: 'terser',
      sourcemap: !isProd, // 生产环境关闭 sourceMap，防止代码反解
      terserOptions: isProd
        ? {
            compress: {
              drop_console: true,    // 移除 console
              drop_debugger: true,   // 移除 debugger
            },
            mangle: {
              toplevel: true,        // 混淆顶层变量名
              eval: true,            // 混淆 eval 中的变量
            },
            format: {
              comments: false,       // 移除所有注释
            },
          }
        : {},
    },
  }
})
```

## Gzip / Brotli 压缩

性价比最高的优化，体积直接缩小 60%\~80%。推荐用 `vite-plugin-compression2`（新版）。

```Shell
pnpm add vite-plugin-compression2 -D
```

```TypeScript
import { compression } from 'vite-plugin-compression2'

export default defineConfig({
  plugins: [
    compression({
      algorithms: ['gzip'],         // Gzip 压缩
      threshold: 10240,             // 大于 10KB 才压缩
      deleteOriginalAssets: false,  // 不删除原文件，兼容不支持 Gzip 的环境
    }),
    compression({
      algorithms: ['brotliCompress'],  // Brotli 比 gzip 压缩率更高
      threshold: 10240,
    }),
  ]
})
```

Nginx 需配合开启：

```Nginx
server {
  gzip on;
  gzip_types text/javascript text/css application/json image/svg+xml;
  gzip_vary on;
  brotli on;
}
```

## CDN 外置第三方依赖

把 Vue、VueRouter、Axios、ElementPlus 等从打包结果中剔除，用公共 CDN 加载。

- 注意：仅限外网场景，内网隔离环境无法加载公共 CDN，会引发资源请求失败。

```Shell
npm install vite-plugin-cdn-import -D
```

```TypeScript
import importToCDN from 'vite-plugin-cdn-import'

export default defineConfig({
  plugins: [
    importToCDN({
      modules: [
        {
          name: 'vue',
          var: 'Vue',
          path: 'https://cdn.jsdelivr.net/npm/vue@3.4.0/dist/vue.global.prod.js'
        },
        {
          name: 'vue-router',
          var: 'VueRouter',
          path: 'https://cdn.jsdelivr.net/npm/vue-router@4/dist/vue-router.global.prod.js'
        },
        {
          name: 'axios',
          var: 'axios',
          path: 'https://cdn.jsdelivr.net/npm/axios@1.6.0/dist/axios.min.js'
        },
        {
          name: 'element-plus',
          var: 'ElementPlus',
          path: 'https://cdn.jsdelivr.net/npm/element-plus@2.4.0/dist/index.full.min.js',
          css: 'https://cdn.jsdelivr.net/npm/element-plus@2.4.0/dist/index.css'
        }
      ]
    })
  ]
})
```

## Rollup 手动分包

对业务代码做精细化拆分，避免单个 chunk 体积过大。

```TypeScript
export default defineConfig({
  build: {
    rollupOptions: {
      output: {
        manualChunks(id) {
          if (id.includes('node_modules')) return 'vendor'
          if (id.includes('element-plus/icons')) return 'icons'
          if (id.includes('utils/')) return 'utils'
        },
        chunkFileNames: 'js/[name]-[hash].js',
        entryFileNames: 'js/[name]-[hash].js',
        assetFileNames: '[ext]/[name]-[hash].[ext]'
      }
    }
  }
})
```

## 图片优化

#### 构建时内联

```TypeScript
export default defineConfig({
  build: {
    assetsInlineLimit: 10240,    // 小于 10KB 的图片转 base64
    chunkSizeWarningLimit: 2000  // 超过 2M 才警告
  }
})
```

#### 自动压缩 + 格式转换

用 `vite-plugin-image-optimizer` 实现图片自动压缩 + 格式转换，80% 质量肉眼无差别，体积砍半。

```Shell
pnpm add vite-plugin-image-optimizer -D
```

```TypeScript
import { ViteImageOptimizer } from 'vite-plugin-image-optimizer'

export default defineConfig({
  plugins: [
    ViteImageOptimizer({
      png: { quality: 80 },
      jpeg: { quality: 80 },
      webp: { quality: 80 },  // 自动转 WebP
    }),
  ]
})
```

## 依赖预构建

Vite 的依赖预构建把 CommonJS/UMD 格式的依赖转成 ES 模块，避免开发过程中重复转换，启动速度更快。

```TypeScript
export default defineConfig({
  optimizeDeps: {
    include: ['vue', 'vue-router', 'pinia', '@vueuse/core'],
  },
})
```

## 浏览器兼容（Legacy）

Vue3 不支持 IE11，但现代浏览器也有版本差异。用 Vite 官方插件自动生成兼容代码。

```Shell
pnpm add @vitejs/plugin-legacy -D
```

```TypeScript
import legacy from '@vitejs/plugin-legacy'

export default defineConfig({
  plugins: [
    legacy({
      targets: ['defaults', 'not IE 11'],  // 兼容所有现代浏览器，排除 IE11
    }),
  ]
})
```

## 完整配置参考

```TypeScript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { visualizer } from 'rollup-plugin-visualizer'
import compression from 'vite-plugin-compression'
import importToCDN from 'vite-plugin-cdn-import'

export default defineConfig({
  plugins: [
    vue(),

    // 打包分析
    visualizer({ open: true, gzipSize: true, brotliSize: true }),

    // CDN 外置第三方库（仅外网）
    importToCDN({
      modules: [
        { name: 'vue', var: 'Vue', path: 'https://cdn.jsdelivr.net/npm/vue@3.4.0/dist/vue.global.prod.js' },
        { name: 'vue-router', var: 'VueRouter', path: 'https://cdn.jsdelivr.net/npm/vue-router@4/dist/vue-router.global.prod.js' },
        { name: 'axios', var: 'axios', path: 'https://cdn.jsdelivr.net/npm/axios@1.6.0/dist/axios.min.js' },
        { name: 'element-plus', var: 'ElementPlus', path: 'https://cdn.jsdelivr.net/npm/element-plus@2.4.0/dist/index.full.min.js', css: 'https://cdn.jsdelivr.net/npm/element-plus@2.4.0/dist/index.css' }
      ]
    }),

    // Gzip 压缩
    compression({ algorithm: 'gzip', ext: '.gz', threshold: 10240, deleteOriginFile: false }),

    // Brotli 压缩
    compression({ algorithm: 'brotliCompress', ext: '.br', threshold: 10240 })
  ],

  build: {
    chunkSizeWarningLimit: 2000,
    assetsInlineLimit: 10240,

    rollupOptions: {
      output: {
        manualChunks(id) {
          if (id.includes('node_modules')) return 'vendor'
          if (id.includes('utils/')) return 'utils'
        },
        chunkFileNames: 'js/[name]-[hash].js',
        entryFileNames: 'js/[name]-[hash].js',
        assetFileNames: '[ext]/[name]-[hash].[ext]'
      }
    }
  }
})
```

## 拓展优化

- SVG 雪碧图：`vite-plugin-svg-icons`，减少 SVG 请求数
- PWA：`vite-plugin-pwa`，支持离线访问
- CDN 加速：`vite-plugin-cdn-import`，外置第三方依赖
- 按需引入：`unplugin-vue-components`，组件库按需引入避免全量打包

## 内网场景注意事项

- 舍弃外网 CDN，依靠路由懒加载、手动分包、本地压缩实现优化
- CDN 依赖外网网络，内网隔离环境直接使用会引发资源请求失败
- `rollup-plugin-visualizer` 仅生成分析报表，不改动产物体积

