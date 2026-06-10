## Express / Koa / NestJS 对比

三大框架均基于 Node.js，2026 年合计占后端项目 80%+ 份额，定位各有侧重。

| 维度       | Express                        | Koa.js                    | NestJS                              |
| ---------- | ------------------------------ | ------------------------- | ----------------------------------- |
| 定位       | 后端入门首选，轻量灵活         | 轻量高性能，中间件升级    | 企业级首选，规范至上                |
| 学习成本   | 低                             | 中（需掌握 async/await）  | 高（TypeScript + 模块化 + DI）      |
| 核心特点   | 极简核心，中间件机制灵活       | 洋葱模型，核心仅几 KB     | 模块化 + 依赖注入，内置全套企业功能 |
| 生态完善度 | 最完善                         | 较完善                    | 企业级完善                          |
| 性能       | 良好                           | 优秀                      | 良好，适合大型项目                  |
| 适用场景   | 新手入门、中小型项目、快速迭代 | 轻量应用、高性能 API 服务 | 企业级应用、微服务、大型团队协作    |
| 代表用户   | 中小型 / 个人项目              | 阿里、腾讯（轻量项目）    | 字节、阿里、腾讯、Google            |

## 选型建议

### 按项目规模选

- 新手入门 / 个人项目 / 中小型项目：Express，学习成本最低，开发速度快。
- 有一定基础，追求代码优雅和性能：Koa.js，洋葱模型避免回调地狱，适合轻量高性能 API。
- 大厂 / 企业级项目 / 大型团队协作：NestJS，规范严格，可维护性强，大厂首选。

### 前后端搭配

- 中小型项目：Express + Vue 3（最易上手，开发效率高）
- 轻量高性能项目：Koa.js + React（配合 Next.js 全栈开发）
- 企业级项目：NestJS + Angular/React（规范一致，适合大型团队）

## 快速上手代码

### Express

```JavaScript
// 安装：npm install express
const express = require('express');
const app = express();
const port = 3000;

app.get('/api/hello', (req, res) => {
  res.send({ message: 'Hello Express', code: 200 });
});

app.listen(port, () => {
  console.log(`服务器运行在 http://localhost:${port}`);
});
```

### Koa.js

```JavaScript
// 安装：npm install koa
const Koa = require('koa');
const app = new Koa();
const port = 3000;

// 洋葱模型：中间件按顺序进入，逆序返回
app.use(async (ctx, next) => {
  console.log('中间件1：开始');
  await next();
  console.log('中间件1：结束');
});

app.use(async (ctx) => {
  ctx.body = { message: 'Hello Koa', code: 200 };
});

app.listen(port, () => {
  console.log(`服务器运行在 http://localhost:${port}`);
});
```

### NestJS

```TypeScript
// 安装：npm install @nestjs/core @nestjs/common express
import { NestFactory } from '@nestjs/core';
import { Controller, Get, Module } from '@nestjs/common';

@Controller('api')
export class AppController {
  @Get('hello')
  getHello() {
    return { message: 'Hello NestJS', code: 200 };
  }
}

@Module({
  controllers: [AppController],
})
export class AppModule {}

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
  console.log('服务器运行在 http://localhost:3000');
}
bootstrap();
```

## 前后端协作流程

前端框架（React/Vue）负责「页面和交互」，后端框架（Express/Koa/NestJS）负责「数据和接口」。

典型用户登录流程：

1. 前端：用户在登录页输入账号密码，点击登录按钮
2. 前端：通过 axios 将数据传给后端接口（如 `/api/login`）
3. 后端：接收请求，查询数据库验证账号密码
4. 后端：验证通过，返回登录成功 + 用户信息
5. 前端：接收响应，渲染登录成功页面

## 参考链接

- [Express 官网](https://expressjs.com/)
- [Koa.js 官网](https://koajs.com/)
- [NestJS 官网](https://nestjs.com/)
