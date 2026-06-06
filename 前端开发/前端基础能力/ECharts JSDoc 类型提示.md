# ECharts JSDoc 类型提示

亮点：
- 零成本：一行 JSDoc 注释即可在纯 JS 项目中获得 ECharts 配置的完整智能提示，无需引入 TypeScript
- 零侵入：不改变项目架构、不装额外插件、不需要 `@types/echarts`（新版 ECharts 自带类型定义）
- 全场景：折线、柱状、饼图、地图、热力图等所有图表类型均适用
- 效果等同 TS：全字段补全、枚举值提示、hover 文档预览、语法错误预警、嵌套属性精准提示

## 核心写法

只需在 `option` 变量上方加一行 JSDoc，紧贴变量，不能有空行：

```JavaScript
/** @type {import('echarts').EChartsOption} */
const option = {
  title: {
    text: '示例图表',
    left: 'center'
  },
  tooltip: {
    trigger: 'axis'
  },
  xAxis: {
    type: 'category',
    data: ['周一', '周二', '周三', '周四', '周五']
  },
  yAxis: {
    type: 'value'
  },
  series: [
    {
      name: '数据量',
      type: 'bar',
      data: [120, 200, 150, 80, 70]
    }
  ]
}
```

## 语法原理

```JavaScript
/** @type {import('echarts').EChartsOption} */
```

- `import('echarts')`：动态引入 ECharts 内置类型模块，无需手动 import
- `EChartsOption`：ECharts 官方顶级配置类型，覆盖所有图表、组件、样式、交互的完整类型定义
- `@type`：JSDoc 标签，告诉 VSCode 当前变量严格遵循该类型，激活智能提示

等同于 TS 中的：

```TypeScript
import type { EChartsOption } from 'echarts'
const option: EChartsOption = {}
```

## Vue3 组合式 API 完整示例

```Vue
<template>
  <div class="chart-box" ref="chartRef" style="width: 100%; height: 400px;"></div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import * as echarts from 'echarts'

const chartRef = ref(null)
let myChart = null

/** @type {import('echarts').EChartsOption} */
const option = {
  title: {
    text: 'Vue3 JS ECharts 智能提示演示',
    left: 'center',
    textStyle: { color: '#333' }
  },
  tooltip: {
    trigger: 'axis',
    backgroundColor: 'rgba(0,0,0,0.6)'
  },
  legend: { data: ['月度数据'], bottom: 10 },
  xAxis: {
    type: 'category',
    boundaryGap: false,
    data: ['1月', '2月', '3月', '4月', '5月', '6月']
  },
  yAxis: { type: 'value' },
  series: [
    {
      name: '月度数据',
      type: 'line',
      smooth: true,
      data: [820, 932, 901, 934, 1290, 1330],
      areaStyle: { color: 'rgba(180, 238, 180, 0.3)' }
    }
  ]
}

const initChart = () => {
  myChart = echarts.init(chartRef.value)
  myChart.setOption(option)
}

const resizeChart = () => { myChart?.resize() }

onMounted(() => {
  initChart()
  window.addEventListener('resize', resizeChart)
})

onUnmounted(() => {
  window.removeEventListener('resize', resizeChart)
  myChart?.dispose()
})
</script>
```

## 提示失效排查

如果加了注释后没有提示，检查以下 3 点：

1. VSCode JS 类型检查未开启 —— 在项目根目录 `jsconfig.json` 中开启：
   ```JSON
   {
     "compilerOptions": { "checkJs": true },
     "include": ["src/**/*"]
   }
   ```
2. 注释格式不规范 —— 必须紧贴变量上方、无空行、完整多行注释格式（`/** */`）
3. 文件顶部有 `// @ts-nocheck` —— 该指令会禁用所有类型提示，删掉即可
