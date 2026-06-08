亮点：

- 分层架构设计：核心层（通用 Axios 封装）→ 配置层（项目自定义）→ Service 层（业务 API），职责清晰可复用
- 插件化架构：取消请求、防重、重试等能力均为独立插件，通过 PluginManager 按需注册，模块化可扩展
- OOP 风格 HttpClient 类，支持拦截器、插件管理、取消请求等按需组合
- Mock 服务基于 vite-plugin-mock，完整 CRUD 示例，开发阶段零后端依赖
- 通用响应类型 ApiResp / PageReq / PageData，前后端统一数据结构约定
- 快速方案保留：防重复请求 + 401 自动跳转 + 小程序兼容的轻量封装

## 架构设计

整个封装分为四层，从底层 Axios 到业务 API 逐层向上：

```
service 层 (API 定义，与业务相关)
       ↑ 调用
项目配置层 (src/http/index.ts，自定义拦截器 / 超时 / 基础路径)
       ↑ 实例化
请求核心封装 (src/http/core/，通用功能，可提取为独立 lib)
       ↑ 依赖
Axios 原生库
```

目录结构：

```
project-root/
├── mock/
│   └── demo.ts                    # Mock 接口定义
├── src/
│   ├── http/
│   │   ├── core/
│   │   │   ├── http-client.ts     # Axios 实例 + 基础请求方法
│   │   │   ├── interceptors.ts    # 拦截器默认实现
│   │   │   ├── plugin.ts          # HttpPlugin 接口定义
│   │   │   ├── plugin-manager.ts  # 插件管理器
│   │   │   ├── request-canceler.ts # 请求取消插件
│   │   │   ├── types.ts           # 通用类型定义
│   │   │   └── index.ts           # 统一导出
│   │   └── index.ts               # 项目配置层，组装并导出实例
│   └── services/
│       └── base-service.ts        # 通用 CRUD 基类
```

## 通用类型定义

`src/http/core/types.ts`：

```TypeScript
/** 通用响应结构 */
export interface ApiResp<T = any> {
  code: number
  message: string
  data: T
}

/** 分页请求结构 */
export interface PageReq {
  pageNum: number
  pageSize: number
}

/** 分页响应数据结构 */
export interface PageData<T> {
  list: T[]
  total: number
}

/** HTTP 请求客户端配置 */
export interface HttpClientConfig {
  baseURL?: string
  timeout?: number
  headers?: Record<string, string>
  interceptor?: InterceptorConfig
  enableCancel?: boolean  // 是否开启请求取消
}

/** 拦截器配置 */
export interface InterceptorConfig {
  request?: {
    onFulfilled?: (config: AxiosRequestConfig) => AxiosRequestConfig | Promise<AxiosRequestConfig>
    onRejected?: (error: AxiosError) => any
  }
  response?: {
    onFulfilled?: (response: AxiosResponse) => AxiosResponse | Promise<AxiosResponse>
    onRejected?: (error: AxiosError) => any
  }
}
```

## HttpClient 核心类

`src/http/core/http-client.ts`：

```TypeScript
import axios, { type AxiosInstance, type AxiosRequestConfig } from 'axios'
import type { HttpClientConfig } from './types'

const defaultConfig: HttpClientConfig = {
  baseURL: import.meta.env.VITE_API_BASE_URL || '/api',
  timeout: 3000,
  headers: { 'Content-Type': 'application/json;charset=utf-8' },
}

export class HttpClient {
  protected instance: AxiosInstance
  protected config: HttpClientConfig

  constructor(config: HttpClientConfig = {}) {
    this.config = { ...defaultConfig, ...config }
    this.instance = this.createInstance()
  }

  private createInstance(): AxiosInstance {
    return axios.create({
      baseURL: this.config.baseURL,
      timeout: this.config.timeout,
      headers: this.config.headers,
    })
  }

  public get(url: string, config?: AxiosRequestConfig): Promise<any> {
    return this.instance.get(url, config)
  }

  public post(url: string, data?: any, config?: AxiosRequestConfig): Promise<any> {
    return this.instance.post(url, data, config)
  }

  public put(url: string, data?: any, config?: AxiosRequestConfig): Promise<any> {
    return this.instance.put(url, data, config)
  }

  public delete(url: string, config?: AxiosRequestConfig): Promise<any> {
    return this.instance.delete(url, config)
  }

  public patch(url: string, data?: any, config?: AxiosRequestConfig): Promise<any> {
    return this.instance.patch(url, data, config)
  }

  public getInstance(): AxiosInstance {
    return this.instance
  }
}
```

# 插件化架构

取消请求、防重、重试等能力均为独立插件，通过 `PluginManager` 按需注册到 Axios 实例，互不耦合。

`src/http/core/plugin.ts` — 插件接口：

```TypeScript
import type { AxiosInstance } from 'axios'

export interface HttpPlugin {
  /** 应用插件到 Axios 实例 */
  apply(instance: AxiosInstance): void
}
```

`src/http/core/plugin-manager.ts` — 插件管理器：

```TypeScript
import type { AxiosInstance } from 'axios'
import type { HttpPlugin } from './plugin'

export class PluginManager {
  private plugins: HttpPlugin[] = []

  public register(plugin: HttpPlugin): void {
    this.plugins.push(plugin)
  }

  public applyAll(instance: AxiosInstance): void {
    this.plugins.forEach(plugin => plugin.apply(instance))
  }

  public clear(): void {
    this.plugins = []
  }
}
```

HttpClient 集成插件：

```TypeScript
export class HttpClient {
  private pluginManager: PluginManager
  private requestCanceler: RequestCanceler

  constructor(config: HttpClientConfig = {}) {
    this.config = { ...defaultConfig, ...config }
    this.instance = this.createInstance()
    this.pluginManager = new PluginManager()
    this.requestCanceler = new RequestCanceler()
    this.registerPlugins()
    this.setInterceptors()
  }

  private registerPlugins() {
    if (this.config.enableCancel) {
      this.pluginManager.register(this.requestCanceler)
    }
    this.pluginManager.applyAll(this.instance)
  }

  public cancelAll(): void {
    this.requestCanceler.clear()
  }

  public getPluginManager(): PluginManager {
    return this.pluginManager
  }
}
```

## 请求取消插件（RequestCanceler）

适用场景：页面跳转取消未完成请求、搜索框只保留最新请求、用户主动终止。

核心逻辑：

1. 请求拦截器中创建 `AbortController`，将 `signal` 绑定到 `config`，存入 Map
2. 响应拦截器中（无论成功 / 失败）从 Map 移除
3. 取消请求时从 Map 取出 controller 调用 `abort()`

> Axios v0.22.0+ 已弃用 `CancelToken`，改用浏览器原生 `AbortController`：[官方文档](https://axios-http.com/zh/docs/cancellation)

`src/http/core/request-canceler.ts`：

```TypeScript
import type { AxiosInstance, AxiosRequestConfig, AxiosResponse, AxiosError } from 'axios'
import type { HttpPlugin } from './plugin'

export class RequestCanceler implements HttpPlugin {
  private cancelMap: Map<string, AbortController>

  constructor() {
    this.cancelMap = new Map()
  }

  /** 生成请求唯一标识 */
  private generateKey(config: AxiosRequestConfig): string {
    const { url, method, params, data } = config
    return `${method || 'GET'}-${url}-${JSON.stringify(params || {})}-${JSON.stringify(data || {})}`
  }

  /** 添加请求到取消器（先取消之前相同的请求） */
  public add(config: AxiosRequestConfig): void {
    this.remove(config)
    const key = this.generateKey(config)
    const controller = new AbortController()
    config.signal = controller.signal
    this.cancelMap.set(key, controller)
  }

  /** 取消请求并从映射表中移除 */
  public remove(config: AxiosRequestConfig): void {
    const key = this.generateKey(config)
    if (this.cancelMap.has(key)) {
      this.cancelMap.get(key)?.abort()
      this.cancelMap.delete(key)
    }
  }

  /** 取消所有请求 */
  public clear(): void {
    this.cancelMap.forEach((controller) => controller.abort())
    this.cancelMap.clear()
  }

  /** 应用插件到 Axios 实例 */
  public apply(instance: AxiosInstance): void {
    instance.interceptors.request.use(
      (config: AxiosRequestConfig) => { this.add(config); return config as any },
      (error: AxiosError) => Promise.reject(error)
    )
    instance.interceptors.response.use(
      (response: AxiosResponse) => { this.remove(response.config); return response },
      (error: AxiosError) => { if (error.config) this.remove(error.config); return Promise.reject(error) }
    )
  }
}
```

## 拦截器默认实现

`src/http/core/interceptors.ts`：

```TypeScript
import { type AxiosRequestConfig, type AxiosResponse, AxiosError } from 'axios'

// 请求拦截器 - 成功
export const defaultRequestOnFulfilled = (config: AxiosRequestConfig) => {
  if (import.meta.env.DEV) {
    console.log('Request:', { url: config.url, method: config.method, params: config.params, data: config.data })
  }
  return config
}

// 请求拦截器 - 失败
export const defaultRequestOnRejected = (error: AxiosError): any => {
  return Promise.reject(error)
}

// 响应拦截器 - 成功
export const defaultResponseOnFulfilled = (response: AxiosResponse): any => {
  const { data } = response
  // 标准 API 响应格式：code + message + data
  if (data && typeof data === 'object' && 'code' in data && 'message' in data) {
    const { code, message, data: responseData } = data
    if (code === 0 || code === 200) return responseData
    throw new Error(message || '请求失败')
  }
  return data
}

// 响应拦截器 - 失败
export const defaultResponseOnRejected = (error: AxiosError): any => {
  if (error.response) {
    console.error('request error, status:', error.response.status)
  } else if (error.request) {
    console.error('Network Error')
  } else {
    console.error('Request Config Error:', error.message)
  }
  return Promise.reject(error)
}
```

## Mock 服务（vite-plugin-mock）

安装依赖：

```Shell
pnpm add mockjs @types/mockjs vite-plugin-mock -D
```

`vite.config.ts` 配置：

```TypeScript
import { viteMockServe } from 'vite-plugin-mock'

export default defineConfig({
  plugins: [
    viteMockServe({
      mockPath: './mock',
      enable: true,
    }),
  ],
})
```

`mock/demo.ts` 完整 CRUD 示例：

```TypeScript
import Mock from 'mockjs'
import type { MockMethod } from 'vite-plugin-mock'
import type { ApiResp } from '../src/http/core'

const demoList = Mock.mock({
  'list|100': [{
    'id|+1': 1,
    title: '@ctitle(5, 10)',
    content: '@cparagraph(1, 3)',
    author: '@name',
    status: '@boolean',
    createdAt: '@datetime',
    updatedAt: '@datetime',
  }],
}).list

function success<T>(data: T): ApiResp<T> {
  return { code: 0, message: 'success', data }
}

function error(message: string, code: number = 500): ApiResp<null> {
  return { code, message, data: null }
}

const demoMock: MockMethod[] = [
  {
    url: '/api/demo',
    method: 'get',
    response: ({ query }) => {
      const pageNum = parseInt(query.pageNum) || 1
      const pageSize = parseInt(query.pageSize) || 10
      const keyword = query.keyword || ''
      let filteredList = demoList
      if (keyword) {
        filteredList = demoList.filter(
          (item) => item.title.includes(keyword) || item.content.includes(keyword) || item.author.includes(keyword),
        )
      }
      const start = (pageNum - 1) * pageSize
      const list = filteredList.slice(start, start + pageSize)
      return success({ list, total: filteredList.length })
    },
  },
  {
    url: '/api/demo/:id',
    method: 'get',
    response: ({ query }) => {
      const item = demoList.find((item) => item.id === parseInt(query.id))
      return item ? success(item) : error('Item not found')
    },
  },
  {
    url: '/api/demo',
    method: 'post',
    response: ({ body }) => {
      const newItem = { id: demoList.length + 1, ...body, createdAt: new Date().toISOString(), updatedAt: new Date().toISOString() }
      demoList.push(newItem)
      return success(newItem)
    },
  },
  {
    url: '/api/demo/:id',
    method: 'put',
    response: ({ query, body }) => {
      const index = demoList.findIndex((item) => item.id === parseInt(query.id))
      if (index === -1) return error('Item not found')
      demoList[index] = { ...demoList[index], ...body, updatedAt: new Date().toISOString() }
      return success(demoList[index])
    },
  },
  {
    url: '/api/demo/:id',
    method: 'delete',
    response: ({ query }) => {
      const index = demoList.findIndex((item) => item.id === parseInt(query.id))
      if (index === -1) return error('Item not found')
      demoList.splice(index, 1)
      return success(null)
    },
  },
]

export default demoMock
```

## 快速方案（轻量封装）

适合小型项目或快速原型，直接在单文件中完成所有封装：

```JavaScript
import axios from 'axios'

// ===== 1. 创建实例 =====
const service = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL || '/api',
  timeout: 10000,
  headers: { 'Content-Type': 'application/json;charset=UTF-8' }
})

// ===== 2. 防重复请求 =====
const pending = new Map()
const getPendingKey = (config) =>
  [config.method, config.url, JSON.stringify(config.params), JSON.stringify(config.data)].join('&')

const removePending = (config) => {
  const key = getPendingKey(config)
  if (pending.has(key)) {
    pending.get(key)?.abort?.()
    pending.delete(key)
  }
}

// ===== 3. 请求拦截器 =====
service.interceptors.request.use(
  (config) => {
    removePending(config)
    const controller = new AbortController()
    config.signal = controller.signal
    pending.set(getPendingKey(config), controller)

    const token = typeof localStorage !== 'undefined'
      ? localStorage.getItem('token')
      : uni.getStorageSync('token')
    if (token) config.headers.Authorization = `Bearer ${token}`
    return config
  },
  (error) => Promise.reject(error)
)

// ===== 4. 响应拦截器 =====
service.interceptors.response.use(
  (response) => {
    removePending(response.config)
    const res = response.data
    if (res.code === 200) return res.data
    alert?.(res.msg || '请求失败')
    return Promise.reject(res)
  },
  (error) => {
    removePending(error.config)
    let msg = '网络异常，请稍后重试'
    if (error.message?.includes('timeout')) msg = '请求超时'
    if (error.code === 'ECONNABORTED') msg = '请求已取消'
    if (error.response?.status === 401) {
      msg = '登录已过期'
      localStorage.removeItem?.('token')
      location.href = '/login'
    }
    if (error.response?.status === 403) msg = '权限不足'
    if (error.response?.status === 500) msg = '服务器开小差了'
    alert?.(msg)
    return Promise.reject(error)
  }
)

export default service
```

```JavaScript
import request from '@/utils/request'

export const getUserInfo = () => request.get('/user/info')
export const login = (data) => request.post('/user/login', data)
```

## 响应结构设计争议

两种常见方案：

- 方案 A：无论成功失败都返回 `code + data + message`，前端先判断 HTTP 状态码再判断业务 code（大多数团队的选择）
- 方案 B：成功时只返回业务数据，失败时才返回 `code + message`，前端以 HTTP 200 判断业务成功

