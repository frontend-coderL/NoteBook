核心原则：可维护 > 可读 > 性能 > 简洁，类型安全零容忍。

## 项目结构

```
project-root/
├── src/
│   ├── index.ts                  # 应用入口
│   ├── app.ts                    # Express/Fastify 应用实例
│   ├── config/                   # 配置集中管理
│   ├── routes/                   # 路由定义（薄层）
│   ├── controllers/              # 控制器（薄层，只做参数提取、校验、调用 service）
│   ├── services/                 # 业务逻辑层（核心，所有业务规则在此）
│   ├── repositories/             # 数据访问层（所有数据库 CRUD 封装在此）
│   ├── models/                   # 数据模型 / 实体
│   ├── middleware/               # 中间件（鉴权、错误处理、requestId 等）
│   ├── validators/               # 请求校验（Zod schema）
│   ├── utils/                    # 通用工具（logger、http-error、asyncHandler）
│   ├── types/                    # 全局类型
│   └── constants/                # 常量
├── tests/                        # 测试（镜像 src 结构）
│   ├── unit/
│   ├── integration/
│   └── fixtures/
├── scripts/                      # 脚本（迁移、种子数据）
├── .env.example / .env
├── tsconfig.json
├── eslint.config.mjs
├── vitest.config.ts
└── Dockerfile
```

分层调用链路：`Request → Middleware → Router → Controller → Service → Repository → Database`

## TypeScript 严格配置

```json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "exactOptionalPropertyTypes": true,
    "module": "ESNext",
    "moduleResolution": "bundler",
    "target": "ES2022",
    "outDir": "./dist",
    "rootDir": "./src",
    "declaration": true,
    "sourceMap": true,
    "paths": { "@/*": ["./src/*"], "@tests/*": ["./tests/*"] }
  }
}
```

关键配置说明：

- `strict`：防止 null/undefined 运行时崩溃
- `noUncheckedIndexedAccess`：索引访问必须处理 undefined，防止 `arr[0].prop` 崩溃
- `noImplicitReturns`：所有分支必须有返回值
- `exactOptionalPropertyTypes`：区分「不存在」和「值为 undefined」

## 命名规范

| 元素             | 风格             | 示例                           |
| ---------------- | ---------------- | ------------------------------ |
| 文件             | kebab-case       | `user.service.ts`              |
| 目录             | kebab-case       | `controllers/`                 |
| 类 / 接口 / 类型 | PascalCase       | `UserService`、`CreateUserDto` |
| 函数 / 变量      | camelCase        | `getUserById()`、`userId`      |
| 全局常量         | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT`              |
| 枚举成员         | PascalCase       | `UserRole.Admin`               |

文件后缀约定：`.service.ts` / `.controller.ts` / `.repository.ts` / `.middleware.ts` / `.validator.ts` / `.routes.ts` / `.test.ts`

布尔变量必须以 `is` / `has` / `should` / `can` 开头。

## 类型安全零容忍

| 规则                 | 严重程度 | 替代方案                   |
| -------------------- | -------- | -------------------------- |
| 禁止 `any`           | 阻断     | 使用 `unknown` + 类型守卫  |
| 禁止 `as` 无验证断言 | 阻断     | 使用 Zod 校验              |
| 禁止 `@ts-ignore`    | 阻断     | 修复类型错误               |
| 禁止 `!` 非空断言    | 警告     | 使用可选链 `?.` 或提前判空 |

外部数据必须校验（Zod）：

```TypeScript
import { z } from 'zod';

const CreateUserSchema = z.object({
  name: z.string().min(1).max(100),
  email: z.string().email(),
  age: z.number().int().min(0).max(150).optional(),
});

type CreateUserDto = z.infer<typeof CreateUserSchema>;

// 控制器中使用
const parsed = CreateUserSchema.safeParse(req.body);
if (!parsed.success) {
  return res.status(400).json({ errors: parsed.error.flatten() });
}
const user = await userService.create(parsed.data);
```

可辨识联合类型：

```TypeScript
type AsyncState<T> =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success'; data: T }
  | { status: 'error'; error: Error };
```

## 错误处理

### 自定义错误类

```TypeScript
export class AppError extends Error {
  constructor(
    public statusCode: number,
    message: string,
    public code?: string,
    public details?: unknown,
  ) {
    super(message);
    this.name = 'AppError';
  }
}

export class NotFoundError extends AppError {
  constructor(resource: string, id: string) {
    super(404, `${resource} ${id} not found`, 'NOT_FOUND');
  }
}
```

### 全局错误处理中间件

```TypeScript
export function errorHandler(err: Error, req: Request, res: Response) {
  if (err instanceof ZodError) {
    return res.status(400).json({
      success: false, code: 'VALIDATION_ERROR', errors: err.flatten(),
    });
  }
  if (err instanceof AppError) {
    return res.status(err.statusCode).json({
      success: false, code: err.code, message: err.message,
    });
  }
  logger.error({ err, requestId: req.id }, 'Unhandled error');
  return res.status(500).json({
    success: false, code: 'INTERNAL_ERROR', message: 'Unexpected error',
  });
}
```

### asyncHandler 包装

```TypeScript
export function asyncHandler(fn: AsyncRequestHandler) {
  return (req: Request, res: Response, next: NextFunction) => {
    Promise.resolve(fn(req, res, next)).catch(next);
  };
}
```

### 错误处理铁律

- 永不吞异常：所有 catch 必须记录日志或向上抛
- 区分业务错误和系统错误：业务用 AppError，系统记录后返回 500
- 不泄露敏感信息：500 错误不暴露堆栈、数据库结构
- Promise 拒绝必须处理：每个 Promise 链必须有 catch
- 全局兜底：监听 `unhandledRejection` 和 `uncaughtException`

## 日志规范

推荐 Pino，结构化日志，开发环境用 `pino-pretty`：

```TypeScript
import pino from 'pino';

export const logger = pino({
  level: process.env.LOG_LEVEL ?? 'info',
  transport: process.env.NODE_ENV === 'development'
    ? { target: 'pino-pretty', options: { colorize: true } }
    : undefined,
  base: { service: 'my-service' },
  redact: ['password', 'token', 'authorization', 'cookie'],
});
```

日志级别：`fatal`（系统不可用）→ `error`（功能异常）→ `warn`（潜在问题）→ `info`（关键业务节点）→ `debug`（开发调试）

requestId 中间件：每个请求生成唯一 ID（`node:crypto` 的 `randomUUID`），便于链路追踪。

## API 设计规范

RESTful 命名：复数名词、kebab-case、避免动词。

| 方法   | 路径       | 用途     |
| ------ | ---------- | -------- |
| GET    | /users     | 列表     |
| GET    | /users/:id | 详情     |
| POST   | /users     | 创建     |
| PUT    | /users/:id | 全量更新 |
| PATCH  | /users/:id | 部分更新 |
| DELETE | /users/:id | 删除     |

统一响应格式：

```json
// 成功
{ "success": true, "data": {...}, "meta": { "page": 1, "total": 150 } }
// 错误
{ "success": false, "code": "VALIDATION_ERROR", "message": "..." }
```

HTTP 状态码：200 查询成功 / 201 创建成功 / 204 删除成功 / 400 参数校验失败 / 401 未认证 / 403 无权限 / 404 不存在 / 409 冲突 / 429 限频 / 500 服务器错误。

## 测试策略

测试金字塔：单元测试 60%（Service、Utils）→ 集成测试 30%（API 端点、数据库）→ E2E 10%（核心用户流程）。

工具选型：Vitest（单元 + 集成）+ Supertest（HTTP 集成测试）+ Testcontainers（真实数据库测试）。

```TypeScript
import { describe, it, expect, vi } from 'vitest';

describe('UserService.create', () => {
  it('should create user when email is unique', async () => {
    const mockRepo = { findByEmail: vi.fn().mockResolvedValue(null), create: vi.fn() };
    const service = new UserService(mockRepo as any, {} as any);
    await service.create({ name: 'Test', email: 'test@test.com' });
    expect(mockRepo.create).toHaveBeenCalledTimes(1);
  });

  it('should throw 409 when email exists', async () => {
    const mockRepo = { findByEmail: vi.fn().mockResolvedValue({ id: '1' }), create: vi.fn() };
    const service = new UserService(mockRepo as any, {} as any);
    await expect(
      service.create({ name: 'Test', email: 'exist@test.com' })
    ).rejects.toThrow('Email already registered');
  });
});
```
