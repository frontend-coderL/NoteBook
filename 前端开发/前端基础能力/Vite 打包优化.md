# Vite 打包优化

亮点：
- rollup-plugin-visualizer 可视化分析打包体积，精准定位大文件
- Gzip / Brotli 双压缩，传输体积缩小 60%~80%
- CDN 外置第三方依赖，打包体积减少 80%+
- manualChunks 手动分包，路由 / 组件按需加载
- 小图片自动转 base64，减少 HTTP 请求

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

## Gzip / Brotli 压缩

性价比最高的优化，体积直接缩小 60%~80%。

```Shell
npm install vite-plugin-compression -D
```

```TypeScript
import compression from 'vite-plugin-compression'

export default defineConfig({
  plugins: [
    compression({
      algorithm: 'gzip',
      ext: '.gz',
      threshold: 10240,     // 大于 10KB 才压缩
      deleteOriginFile: false
    }),
    compression({
      algorithm: 'brotliCompress',  // Brotli 比 gzip 压缩率更高
      ext: '.br',
      threshold: 10240
    })
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

```TypeScript
export default defineConfig({
  build: {
    assetsInlineLimit: 10240,    // 小于 10KB 的图片转 base64
    chunkSizeWarningLimit: 2000  // 超过 2M 才警告
  }
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

## 内网场景注意事项

- 舍弃外网 CDN，依靠路由懒加载、手动分包、本地压缩实现优化
- CDN 依赖外网网络，内网隔离环境直接使用会引发资源请求失败
- `rollup-plugin-visualizer` 仅生成分析报表，不改动产物体积
