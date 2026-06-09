原生 WebSocket 痛点：断线不自动重连、需手写心跳检测、多组件复用冗余、页面刷新消息丢失。

本文从入门到企业级，三层递进封装方案，Vue2/Vue3 通用。

### 封装思路总览

| 层级           | 适用规模       | 核心功能               |
| ------------ | ---------- | ------------------ |
| 组件内直接使用      | 小项目 / Demo | 基础连接、收发消息          |
| 工具类封装        | 中大型项目      | 心跳 + 断线重连，方便复用     |
| 全局插件 + Pinia | 企业级项目      | 全局状态管理、消息类型分发、离线缓存 |

### 入门版：组件内直接使用

```vue
<template>
  <div>
    <button @click="sendMessage">发送测试消息</button>
    <p>服务端返回：{{ message }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      socket: null,
      message: ''
    };
  },
  created() {
    this.initWebSocket();
  },
  beforeDestroy() {
    this.socket && this.socket.close();
  },
  methods: {
    initWebSocket() {
      this.socket = new WebSocket('ws://localhost:8080');
      this.socket.onopen = () => console.log('WebSocket 连接成功');
      this.socket.onmessage = (event) => {
        this.message = event.data;
      };
      this.socket.onclose = () => console.log('WebSocket 已关闭');
      this.socket.onerror = (err) => console.error('WebSocket 连接错误', err);
    },
    sendMessage() {
      if (this.socket && this.socket.readyState === WebSocket.OPEN) {
        this.socket.send('Hello Server');
      }
    }
  }
};
</script>
```

核心要点：created 初始化、beforeDestroy 关闭、发送前判断 readyState。

### 进阶版：工具类封装（心跳 + 重连）

#### WebSocket 工具类

```javascript
// utils/websocket.js
class WS {
  constructor(url, options = {}) {
    this.url = url;
    this.ws = null;
    this.lockReconnect = false;
    this.heartbeatInterval = options.heartbeatInterval || 5000;
    this.reconnectInterval = options.reconnectInterval || 3000;
    this.heartbeatTimer = null;
    this.reconnectTimer = null;
    this.onMessage = options.onMessage || function () {};
    this.init();
  }

  init() {
    this.ws = new WebSocket(this.url);
    this.ws.onopen = () => {
      console.log('WebSocket 连接成功');
      this.startHeartbeat();
    };
    this.ws.onmessage = (event) => {
      this.onMessage(event.data);
      this.startHeartbeat(); // 收到消息重置心跳，防止误判断线
    };
    this.ws.onclose = () => {
      console.log('WebSocket 已关闭');
      this.reconnect();
    };
    this.ws.onerror = () => {
      console.error('WebSocket 出错');
      this.reconnect();
    };
  }

  send(msg) {
    if (this.ws && this.ws.readyState === WebSocket.OPEN) {
      this.ws.send(msg);
    } else {
      console.warn('WebSocket 未连接，消息未发送');
    }
  }

  // 心跳：定时发送 ping，服务端回 pong（无需前端处理）
  startHeartbeat() {
    clearTimeout(this.heartbeatTimer);
    this.heartbeatTimer = setTimeout(() => {
      if (this.ws.readyState === WebSocket.OPEN) {
        this.ws.send('ping');
      }
    }, this.heartbeatInterval);
  }

  // 重连锁防止多次触发
  reconnect() {
    if (this.lockReconnect) return;
    this.lockReconnect = true;
    clearTimeout(this.reconnectTimer);
    this.reconnectTimer = setTimeout(() => {
      console.log('尝试重连 WebSocket...');
      this.init();
      this.lockReconnect = false;
    }, this.reconnectInterval);
  }
}

export default WS;
```

#### 组件中使用

```vue
<template>
  <div>
    <input v-model="input" placeholder="输入消息..." />
    <button @click="send">发送</button>
    <ul>
      <li v-for="(msg, i) in messages" :key="i">{{ msg }}</li>
    </ul>
  </div>
</template>

<script>
import WS from '@/utils/websocket';

export default {
  data() {
    return { ws: null, input: '', messages: [] };
  },
  created() {
    this.ws = new WS('ws://localhost:8080', {
      onMessage: (msg) => {
        this.messages.push(msg);
      }
    });
  },
  methods: {
    send() {
      if (this.input.trim()) {
        this.ws.send(this.input);
        this.messages.push('我: ' + this.input);
        this.input = '';
      }
    }
  }
};
</script>
```

### 终极版：全局插件 + Pinia（企业级）

#### Pinia Store 管理全局状态

```javascript
// stores/wsStore.js
import { defineStore } from 'pinia';

export const useWsStore = defineStore('ws', {
  state: () => ({
    messages: [],  // { type: 'xxx', content: 'xxx' }
    status: 'closed'  // connected / closed / error
  }),
  actions: {
    addMessage(msgObj) {
      if (!msgObj.type) msgObj.type = 'default';
      this.messages.push(msgObj);
    },
    getMessagesByType(type) {
      return this.messages.filter(msg => msg.type === type);
    },
    setStatus(status) {
      this.status = status;
    },
    clearMessages() {
      this.messages = [];
    },
    // 页面刷新后恢复
    initFromStorage() {
      const cacheMsg = localStorage.getItem('wsMessages');
      if (cacheMsg) this.messages = JSON.parse(cacheMsg);
      const cacheStatus = localStorage.getItem('wsStatus');
      if (cacheStatus) this.status = cacheStatus;
    },
    // 实时缓存到 localStorage
    cacheToStorage() {
      localStorage.setItem('wsMessages', JSON.stringify(this.messages));
      localStorage.setItem('wsStatus', this.status);
    }
  }
});
```

#### WebSocket 全局插件

```javascript
// plugins/websocket.js
import { useWsStore } from '@/stores/wsStore';

class WSPlugin {
  constructor(url, options = {}) {
    this.url = url;
    this.ws = null;
    this.lockReconnect = false;
    this.heartbeatInterval = options.heartbeatInterval || 5000;
    this.reconnectInterval = options.reconnectInterval || 3000;
    this.heartbeatTimer = null;
    this.reconnectTimer = null;
    this.store = useWsStore();
    this.customMessageHandler = options.onMessage || null;
    this.init();
  }

  init() {
    this.ws = new WebSocket(this.url);
    this.ws.onopen = () => {
      console.log('WebSocket 已连接');
      this.store.setStatus('connected');
      this.store.cacheToStorage();
      this.startHeartbeat();
    };
    this.ws.onmessage = (event) => {
      if (event.data === 'pong') return;  // 心跳响应不处理
      let msgObj;
      try {
        msgObj = JSON.parse(event.data);
      } catch {
        msgObj = { type: 'default', content: event.data };
      }
      this.store.addMessage(msgObj);
      this.store.cacheToStorage();
      if (typeof this.customMessageHandler === 'function') {
        this.customMessageHandler(msgObj);
      }
      this.startHeartbeat();
    };
    this.ws.onclose = () => {
      this.store.setStatus('closed');
      this.store.cacheToStorage();
      this.reconnect();
    };
    this.ws.onerror = (err) => {
      this.store.setStatus('error');
      this.store.cacheToStorage();
      this.reconnect();
    };
  }

  send(msg) {
    if (this.ws && this.ws.readyState === WebSocket.OPEN) {
      if (typeof msg === 'object') msg = JSON.stringify(msg);
      this.ws.send(msg);
    } else {
      console.warn('WebSocket 未连接，消息未发送');
    }
  }

  startHeartbeat() {
    clearTimeout(this.heartbeatTimer);
    this.heartbeatTimer = setTimeout(() => {
      if (this.ws.readyState === WebSocket.OPEN) {
        this.ws.send('ping');
      }
    }, this.heartbeatInterval);
  }

  reconnect() {
    if (this.lockReconnect) return;
    this.lockReconnect = true;
    clearTimeout(this.reconnectTimer);
    this.reconnectTimer = setTimeout(() => {
      this.init();
      this.lockReconnect = false;
    }, this.reconnectInterval);
  }

  close() {
    clearTimeout(this.heartbeatTimer);
    clearTimeout(this.reconnectTimer);
    this.ws && this.ws.close();
  }
}

// Vue 插件：Vue2/Vue3 通用
export default {
  install(app, options) {
    const wsInstance = new WSPlugin(options.url, options);
    if (app.config) {
      app.config.globalProperties.$ws = wsInstance;  // Vue3
    } else {
      app.prototype.$ws = wsInstance;  // Vue2
    }
  }
};
```

#### 全局注册（main.js）

```javascript
import { createApp } from 'vue';
import { createPinia } from 'pinia';
import App from './App.vue';
import WSPlugin from './plugins/websocket';

const app = createApp(App);
app.use(createPinia());
app.use(WSPlugin, {
  url: 'ws://localhost:8080',
  heartbeatInterval: 5000,
  reconnectInterval: 3000,
  onMessage: (msg) => console.log('全局收到消息:', msg)
});
app.mount('#app');
```

#### 组件中使用

```vue
<template>
  <div>
    <h4>聊天消息</h4>
    <ul>
      <li v-for="(msg, i) in chatMessages" :key="i">{{ msg.content }}</li>
    </ul>
    <h4>系统通知</h4>
    <ul>
      <li v-for="(msg, i) in notifications" :key="i">{{ msg.content }}</li>
    </ul>
    <input v-model="input" />
    <button @click="sendChat">发送聊天</button>
    <button @click="clearAll">清空所有消息</button>
    <p>当前连接状态：{{ status }}</p>
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue';
import { useWsStore } from '@/stores/wsStore';

export default {
  setup() {
    const wsStore = useWsStore();
    const input = ref('');

    onMounted(() => wsStore.initFromStorage());

    const chatMessages = computed(() => wsStore.getMessagesByType('chat'));
    const notifications = computed(() => wsStore.getMessagesByType('notification'));
    const status = computed(() => wsStore.status);

    const sendChat = () => {
      if (!input.value.trim()) return;
      wsStore.$root?.$ws.send({ type: 'chat', content: input.value });
      wsStore.addMessage({ type: 'chat', content: '我: ' + input.value });
      input.value = '';
    };

    const clearAll = () => wsStore.clearMessages();

    return { input, chatMessages, notifications, status, sendChat, clearAll };
  }
};
</script>
```

### 核心能力对比

| 能力     | 入门版 | 进阶版    | 终极版             |
| ------ | --- | ------ | --------------- |
| 基础通信   | ✅   | ✅      | ✅               |
| 心跳检测   | ❌   | ✅      | ✅               |
| 断线重连   | ❌   | ✅（重连锁） | ✅（重连锁）          |
| 全局状态管理 | ❌   | ❌      | ✅（Pinia）        |
| 消息类型分发 | ❌   | ❌      | ✅               |
| 离线消息缓存 | ❌   | ❌      | ✅（localStorage） |
| 页面刷新恢复 | ❌   | ❌      | ✅               |

