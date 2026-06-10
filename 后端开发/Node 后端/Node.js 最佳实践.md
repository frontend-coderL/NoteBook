#

- [nodebestpractices](https://github.com/goldbergyoni/nodebestpractices)：102K+ Star，涵盖架构、安全、性能、测试等 100+ 条 Node.js 最佳实践合集，每条都讲清背后的「为什么」。
  - 官网：[practica.dev](https://practica.dev)
  - 内容结构：按主题分类，每条包含简明说明 + 原理解释 + 代码示例（好代码 vs 坏代码）+ 拓展链接。
  - 适合人群：Node.js 后端开发者、前端转全栈开发者。

## 精选实践

### 充分利用多核 CPU（cluster 模块）

Node 默认单线程运行，即使服务器有 8 核 CPU 也只能用一核。使用 `cluster` 模块实现多进程并发：

```JavaScript
const cluster = require('cluster');
const os = require('os');
const numCPUs = os.cpus().length;

if (cluster.isMaster) {
  console.log(`主进程 ${process.pid} 正在运行`);
  for (let i = 0; i < numCPUs; i++) cluster.fork();
  cluster.on('exit', (worker) => {
    console.log(`工作进程 ${worker.process.pid} 已退出`);
    cluster.fork();
  });
} else {
  const express = require('express');
  const app = express();
  app.listen(3000, () => {
    console.log(`工作进程 ${process.pid} 监听端口 3000`);
  });
}
```

实测：CPU 利用率从 12% 提升到 80%，吞吐量暴涨 6 倍。

### 避免阻塞事件循环（worker\_threads）

CPU 密集型操作会阻塞事件循环，导致其他请求排队。使用 `worker_threads` 分离计算任务：

```JavaScript
// 主线程
const { Worker } = require('worker_threads');

app.get('/process', (req, res) => {
  const worker = new Worker('./heavy-task.js', {
    workerData: req.body.data,
  });
  worker.on('message', result => res.json(result));
  worker.on('error', err => res.status(500).json({ error: err.message }));
});
```

```JavaScript
// heavy-task.js —— 子线程运行密集逻辑
const { parentPort, workerData } = require('worker_threads');
function heavyCalc(data) {
  return data.map(item => item * 2);
}
parentPort.postMessage(heavyCalc(workerData));
```

### 统一错误处理（中间件 + 自定义异常类）

每个路由重复 try/catch 既啰嗦又容易遗漏，应使用错误处理中间件 + 自定义异常类：

```JavaScript
class NotFoundError extends Error {
  constructor(message) {
    super(message);
    this.name = 'NotFoundError';
  }
}

// 统一错误处理中间件
app.use((err, req, res, next) => {
  console.error('错误信息:', err);
  if (err.name === 'NotFoundError') {
    return res.status(404).json({ error: err.message });
  }
  res.status(500).json({ error: '服务器内部错误' });
});

// 路由中使用 next(err) 传递错误
app.get('/user/:id', async (req, res, next) => {
  try {
    const user = await getUserById(req.params.id);
    if (!user) throw new NotFoundError('用户不存在');
    res.json(user);
  } catch (err) {
    next(err);
  }
});
```

## 前端转后端必看五条

1. 不要在客户端保存敏感信息
2. 使用环境变量管理机密信息
3. 优先使用原生 API 而不是引入 lodash
4. 避免嵌套错误处理
5. 结构清晰的项目目录

