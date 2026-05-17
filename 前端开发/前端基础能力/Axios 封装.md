**亮点**：

- 自动防重复请求（基于 URL + 参数）

- Web / 小程序双端兼容（`localStorage` vs `uni.getStorageSync`）

- 401 自动跳登录页

- 返回值直接是 `data`，业务层无需再 `.data.data`

```JavaScript
import axios from'axios'

// ===== 1. 创建实例 =====
const service = axios.create({
baseURL: import.meta.env.VITE_API_BASE_URL || '/api',
timeout: 10000,
headers: {
    'Content-Type': 'application/json;charset=UTF-8'
  }
})

// ===== 2. 防重复请求（关键！）=====
const pending = newMap()
const getPendingKey = (config) =>
  [config.method, config.url, JSON.stringify(config.params), JSON.stringify(config.data)].join('&')

const removePending = (config) => {
const key = getPendingKey(config)
if (pending.has(key)) {
    pending.get(key)?.abort?.() // 取消上一次请求
    pending.delete(key)
  }
}

// ===== 3. 请求拦截器 =====
service.interceptors.request.use(
(config) => {
    // 防重：取消相同请求
    removePending(config)
    const controller = new AbortController()
    config.signal = controller.signal
    pending.set(getPendingKey(config), controller)

    // 自动加 token（兼容 localStorage / uni.getStorageSync）
    const token = typeof localStorage !== 'undefined'
      ? localStorage.getItem('token')
      : uni.getStorageSync('token') // 小程序适配

    if (token) {
      config.headers.Authorization = `Bearer ${token}`
    }
    return config
  },
  (error) => Promise.reject(error)
)

// ===== 4. 响应拦截器 =====
service.interceptors.response.use(
(response) => {
    // 清除 pending
    removePending(response.config)

    const res = response.data
    // 假设后端 code=200 为成功（按实际调整）
    if (res.code === 200) {
      return res.data // 直接返回业务数据
    }

    // 统一错误提示
    uni.showToast?.({ title: res.msg || '操作失败', icon: 'none' }) // 小程序
    alert?.(res.msg || '请求失败') // Web
    returnPromise.reject(res)
  },
  (error) => {
    removePending(error.config)

    let msg = '网络异常，请稍后重试'
    if (error.message?.includes('timeout')) msg = '请求超时'
    if (error.code === 'ECONNABORTED') msg = '请求已取消'
    if (error.response?.status === 401) {
      msg = '登录已过期'
      // 清 token + 跳登录
      localStorage.removeItem?.('token')
      uni.removeStorageSync?.('token')
      location.href = '/login'// Web
      uni.reLaunch?.({ url: '/pages/login/login' }) // 小程序
    }
    if (error.response?.status === 403) msg = '权限不足'
    if (error.response?.status === 500) msg = '服务器开小差了'

    uni.showToast?.({ title: msg, icon: 'none' })
    alert?.(msg)
    returnPromise.reject(error)
  }
)

export default service
```

```JavaScript
import request from'@/utils/request'

// 获取用户信息
exportconst getUserInfo = () => request.get('/user/info')

// 登录
exportconst login = (data) => request.post('/user/login', data)

// 上传头像
exportconst uploadAvatar = (file) => {
const formData = new FormData()
  formData.append('avatar', file)
return request.post('/upload/avatar', formData, {
    headers: { 'Content-Type': 'multipart/form-data' }
  })
}
```

