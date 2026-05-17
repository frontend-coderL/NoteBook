```JavaScript
/**
 * LocalStorage通用封装（Vue/React/Uniapp/小程序通用）
 * 支持：存字符串、对象、数组 + 过期时间 + 容错处理 + 键名统一
 */
const STORAGE_KEY_PREFIX = 'frontend_'; // 键名前缀，避免冲突

// 1. 存数据（支持过期时间，单位：秒）
exportconst setStorage = (key, value, expire = 0) => {
try {
    // 处理对象/数组，转为JSON字符串（LocalStorage只能存字符串）
    const data = {
      value: typeof value === 'object' ? JSON.stringify(value) : value,
      expire: expire > 0 ? Date.now() + expire * 1000 : 0// 0表示永久有效
    };
    // 键名加前缀，避免和其他项目/插件冲突
    localStorage.setItem(`${STORAGE_KEY_PREFIX}${key}`, JSON.stringify(data));
  } catch (error) {
    console.error('LocalStorage存储失败：', error);
    // 兼容低版本浏览器/隐私模式（LocalStorage不可用）
    alert('浏览器存储不可用，请开启正常模式后重试');
  }
};

// 2. 取数据（自动处理JSON解析，判断过期）
exportconst getStorage = (key) => {
try {
    const storageKey = `${STORAGE_KEY_PREFIX}${key}`;
    const dataStr = localStorage.getItem(storageKey);
    if (!dataStr) returnnull;

    const data = JSON.parse(dataStr);
    // 判断是否过期（expire=0表示永久有效）
    if (data.expire > 0 && Date.now() > data.expire) {
      // 过期后自动删除，避免无效数据占用内存
      localStorage.removeItem(storageKey);
      returnnull;
    }

    // 自动解析JSON（如果存的是对象/数组）
    try {
      returnJSON.parse(data.value);
    } catch (e) {
      // 不是JSON格式，直接返回原始值（字符串）
      return data.value;
    }
  } catch (error) {
    console.error('LocalStorage获取失败：', error);
    returnnull;
  }
};

// 3. 删除单个数据
exportconst removeStorage = (key) => {
try {
    localStorage.removeItem(`${STORAGE_KEY_PREFIX}${key}`);
  } catch (error) {
    console.error('LocalStorage删除失败：', error);
  }
};

// 4. 清空所有数据（只清空当前项目的，不影响其他项目）
exportconst clearStorage = () => {
try {
    // 只删除带前缀的键，避免清空其他项目的存储
    Object.keys(localStorage).forEach(key => {
      if (key.startsWith(STORAGE_KEY_PREFIX)) {
        localStorage.removeItem(key);
      }
    });
  } catch (error) {
    console.error('LocalStorage清空失败：', error);
  }
};

// 5. 批量存数据
exportconst setStorageBatch = (obj, expire = 0) => {
try {
    Object.entries(obj).forEach(([key, value]) => {
      setStorage(key, value, expire);
    });
  } catch (error) {
    console.error('LocalStorage批量存储失败：', error);
  }
};
```

```JavaScript
// 引入封装的方法（所有框架通用）
import { setStorage, getStorage, removeStorage } from'@/utils/storage';

// 1. 存字符串（比如token）
setStorage('token', 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...', 86400); // 过期时间1天（86400秒）

// 2. 存对象（比如用户信息）
const userInfo = { id: 1, name: '张三', age: 25 };
setStorage('userInfo', userInfo, 86400);

// 3. 存数组（比如历史记录）
const historyList = ['Vue', 'React', 'JS'];
setStorage('historyList', historyList); // 不设过期时间，永久有效

// 4. 取数据（自动解析对象/数组，无需手动JSON.parse）
const token = getStorage('token');
const user = getStorage('userInfo'); // 直接拿到对象，无需解析
const history = getStorage('historyList'); // 直接拿到数组

// 5. 删除数据
removeStorage('token'); // 删除单个
// clearStorage(); // 清空当前项目所有存储
```



> LocalStorage进阶用法

监听 LocalStorage 变化（跨页面通信）

```JavaScript
// 页面A监听存储变化
window.addEventListener('storage', (e) => {
// 只监听当前项目的存储变化（带前缀）
if (e.key?.startsWith(STORAGE_KEY_PREFIX)) {
    console.log('存储变化：', e.key, e.newValue);
    // 比如监听token变化，实现跨页面登录状态同步
    if (e.key === `${STORAGE_KEY_PREFIX}token`) {
      // 处理登录状态更新
    }
  }
});

// 页面B修改存储，页面A会触发监听
setStorage('token', 'newToken');
```

限制存储大小，避免内存溢出

```JavaScript
/ 新增：校验存储大小
const checkStorageSize = (value) => {
const valueStr = typeof value === 'object' ? JSON.stringify(value) : value;
const size = new Blob([valueStr]).size;
// 限制单条数据不超过1MB
if (size > 1 * 1024 * 1024) {
    alert('存储数据过大，建议拆分存储');
    returnfalse;
  }
returntrue;
};

// 在setStorage中添加校验
exportconst setStorage = (key, value, expire = 0) => {
if (!checkStorageSize(value)) return;
// 原有逻辑...
};
```

