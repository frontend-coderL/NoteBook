亮点：

- 无状态认证：Token 自包含用户信息，服务端无需存储 Session，天然适合分布式和微服务
- 跨域友好：不依赖 Cookie，Token 通过 `Authorization` 头传递，无同源策略限制
- 灵活的加密方式：支持 HMAC SHA256（对称）和 RSA / ECDSA（非对称），非对称适合微服务架构

## Session + Cookie 的局限

Session + Cookie 是有状态认证：每个 Token 都需要在服务端维护对应关系（内存 / Redis / 数据库），验证时先取 Cookie 再查表。

主要问题：

- Cookie 受同源策略限制，跨域场景困难
- 用户禁用 Cookie 则方案失效
- CSRF 钓鱼链接风险（浏览器已有部分安全措施）
- 用户量大时 Session 占用大量服务器内存
- 分布式场景需引入分布式 Session 方案，可扩展性一般

## JWT 格式与原理

JWT（JSON Web Tokens）遵循 RFC 7519 标准，三段式结构：

```
Header.Payload.Signature
```

### Header（头部）

```JSON
{
  "alg": "HS256",
  "typ": "JWT"
}
```

- `alg`：签名算法，常用 HMAC SHA256（对称）或 RSA（非对称）
- `typ`：令牌类型，固定为 JWT

### Payload（载荷）

包含三类声明：

- Registered claims：标准化字段（可选），如 `iss`、`exp`、`sub`、`iat` 等
- Public claims：公开字段，建议使用 [IANA JWT 注册表](https://www.iana.org/assignments/jwt/jwt.xhtml) 中的命名
- Private claims：完全自定义字段，按业务需求定义

### Signature（签名）

```
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret
)
```

三段 Base64Url 编码结果用点号连接，即为标准 JWT。

## 对称 vs 非对称加密选择

| 方式    | 算法          | 适用场景        |
| ----- | ----------- | ----------- |
| 对称加密  | HMAC SHA256 | 单体应用、简单服务   |
| 非对称加密 | RSA / ECDSA | 分布式系统、微服务架构 |

RSA 方案：认证服务用私钥签发 JWT，各微服务用公钥验签，既保证安全性又避免微服务频繁调用认证服务。

## 代码实现

### 登录下发 Token

```JavaScript
// 验证用户名密码后，签发 JWT 返回给前端
const token = jwt.sign(
  { sub: user.id, role: user.role },
  SECRET_KEY,
  { expiresIn: '7d' }
)

return { code: 200, data: { token } }
```

前端存储 Token 后，后续请求在 `Authorization` 头携带：

```
Authorization: Bearer <token>
```

### 验证 Token

```JavaScript
async function verifyAuth(req) {
  const authHeader = req.headers['authorization']
  if (!authHeader) throw new Error('未授权')

  const token = authHeader.split(' ')[1]
  const decoded = jwt.verify(token, SECRET_KEY)

  // 验签通过后可继续校验角色等权限
  return decoded
}
```

## 注意事项

- JWT 不应存储经常变化的信息（如实时权限），否则无法及时生效
- JWT 不应存储敏感信息（如密码），Payload 只是 Base64Url 编码，可被直接解码查看
- 尽量让 JWT 有效期短一点，配合 Refresh Token 机制续期
- 如需存储可能变化的信息，不能是关键信息

