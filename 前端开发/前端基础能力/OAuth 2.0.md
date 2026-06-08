来源：[阮一峰 - 理解OAuth 2.0](https://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html) | RFC 6749

### 为什么需要 OAuth

场景：「云冲印」网站需要读取用户存在 Google 上的照片。传统做法是用户把 Google 密码给「云冲印」，但这有严重问题：

- 「云冲印」会保存密码，不安全。
- 用户无法限制「云冲印」获得授权的范围和有效期。
- 用户只有修改密码才能收回权力，但这会让所有其他第三方应用全部失效。
- 只要一个第三方应用被破解，就会导致密码及所有数据泄漏。

OAuth 解决这些问题：在「客户端」与「服务提供商」之间设置授权层（authorization layer），客户端不直接登录，只登录授权层，用令牌代替密码。

### 六种角色

- Third-party application / Client（客户端）：想访问用户资源的第三方应用。
- HTTP service / 服务提供商：提供资源访问的服务平台（如 Google）。
- Resource Owner（资源所有者）：用户本人。
- User Agent（用户代理）：通常指浏览器。
- Authorization Server（认证服务器）：专门处理认证、发放 Token 的服务。
- Resource Server（资源服务器）：存放用户资源的服务器，可以和认证服务器是同一台，也可以不同。

### 整体运行流程

1. 用户打开客户端，客户端要求用户给予授权。
2. 用户同意授权。
3. 客户端使用授权，向认证服务器申请令牌。
4. 认证服务器确认无误，发放令牌（access\_token）。
5. 客户端使用令牌，向资源服务器请求资源。
6. 资源服务器确认令牌无误，开放资源。

关键在第 2 步：用户怎样授权给客户端。OAuth 2.0 定义了四种授权模式。

### 四种授权模式

#### 1. 授权码模式（Authorization Code）

功能最完整、流程最严密的模式。通过客户端的后台服务器与认证服务器互动。

流程：

1. 用户访问客户端，客户端将用户导向认证服务器。
2. 用户选择是否授权。
3. 用户同意后，认证服务器将用户重定向到客户端指定的 redirect\_uri，并附带授权码（code）。
4. 客户端收到 code，在后台用 code + redirect\_uri + client\_id 向认证服务器申请令牌（用户不可见）。
5. 认证服务器核对无误，返回 access\_token 和 refresh\_token。

请求参数（步骤 1）：

| 参数             | 必选 | 说明                          |
| -------------- | -- | --------------------------- |
| response\_type | 是  | 固定为 code                    |
| client\_id     | 是  | 客户端 ID                      |
| redirect\_uri  | 否  | 重定向 URI                     |
| scope          | 否  | 申请的权限范围                     |
| state          | 否  | 客户端当前状态，认证服务器原样返回（用于防 CSRF） |

请求示例：

```
GET /authorize?response_type=code&client_id=s6BhdRkqt3&state=xyz
&redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb HTTP/1.1
Host: server.example.com
```

授权码特性：

- 有效期极短（通常 10 分钟）。
- 只能使用一次，重复使用会被拒绝。
- 与 client\_id 和 redirect\_uri 一一对应。

换取令牌（步骤 4）：

| 参数            | 必选 | 说明                      |
| ------------- | -- | ----------------------- |
| grant\_type   | 是  | 固定为 authorization\_code |
| code          | 是  | 上一步的授权码                 |
| redirect\_uri | 是  | 必须与步骤 1 保持一致            |
| client\_id    | 是  | 客户端 ID                  |

请求示例：

```
POST /token HTTP/1.1
Host: server.example.com
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&code=SplxlOBeZQQYbYS6WxSbIA
&redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb
```

响应参数：

| 参数             | 必选 | 说明                         |
| -------------- | -- | -------------------------- |
| access\_token  | 是  | 访问令牌                       |
| token\_type    | 是  | 令牌类型（bearer 或 mac，大小写不敏感）  |
| expires\_in    | 否  | 过期时间（秒）                    |
| refresh\_token | 否  | 更新令牌，用于获取下一次 access\_token |
| scope          | 否  | 权限范围（与申请一致可省略）             |

响应示例：

```json
{
  "access_token": "2YotnFZFEjr1zCsicMWpAA",
  "token_type": "bearer",
  "expires_in": 3600,
  "refresh_token": "tGzv3JOkF0XG5Qx2TlKWIA"
}
```

#### 2. 简化模式（Implicit）

不通过客户端服务器，直接在浏览器中申请令牌，跳过授权码步骤。所有步骤在浏览器中完成，令牌对访问者可见，客户端无需认证。

流程：

1. 客户端将用户导向认证服务器。
2. 用户决定是否授权。
3. 用户同意后，认证服务器将用户重定向到 redirect\_uri，并在 URI 的 Hash 部分包含 access\_token。
4. 浏览器向资源服务器发请求（不包含 Hash 值）。
5. 资源服务器返回一个网页，其中的代码可以获取 Hash 中的令牌。
6. 浏览器执行脚本，提取令牌。
7. 浏览器将令牌发给客户端。

请求参数（步骤 1）：

| 参数             | 必选 | 说明        |
| -------------- | -- | --------- |
| response\_type | 是  | 固定为 token |
| client\_id     | 是  | 客户端 ID    |
| redirect\_uri  | 否  | 重定向 URI   |
| scope          | 否  | 权限范围      |
| state          | 否  | 防 CSRF    |

请求示例：

```
GET /authorize?response_type=token&client_id=s6BhdRkqt3&state=xyz
&redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb HTTP/1.1
Host: server.example.com
```

响应（Hash 中包含令牌）：

```
HTTP/1.1 302 Found
Location: http://example.com/cb#access_token=2YotnFZFEjr1zCsicMWpAA
&state=xyz&token_type=bearer&expires_in=3600
```

注意：令牌在 URL 的 Hash 部分（# 后面），浏览器发请求时不会发送 Hash。简化模式已不推荐使用，被 PKCE 模式取代。

#### 3. 密码模式（Resource Owner Password Credentials）

用户直接向客户端提供用户名和密码，客户端用这些信息向认证服务器索要令牌。

适用场景：用户对客户端高度信任（如操作系统的一部分，或知名公司出品）。认证服务器只应在其他模式无法执行时才考虑此模式。

流程：

1. 用户向客户端提供用户名和密码。
2. 客户端将用户名密码发给认证服务器，请求令牌。
3. 认证服务器确认无误，返回 access\_token。

请求参数：

| 参数          | 必选 | 说明           |
| ----------- | -- | ------------ |
| grant\_type | 是  | 固定为 password |
| username    | 是  | 用户名          |
| password    | 是  | 密码           |
| scope       | 否  | 权限范围         |

请求示例：

```
POST /token HTTP/1.1
Host: server.example.com
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/x-www-form-urlencoded

grant_type=password&username=johndoe&password=A3ddj3w
```

注意：整个过程中客户端不得保存用户密码。

#### 4. 客户端模式（Client Credentials）

客户端以自己的名义（而非用户名义）向认证服务器申请令牌。严格来说不属于 OAuth 要解决的典型授权问题。

适用场景：服务间调用（API to API），客户端直接注册使用服务。

流程：

1. 客户端向认证服务器进行身份认证，请求令牌。
2. 认证服务器确认无误，返回 access\_token。

请求参数：

| 参数          | 必选 | 说明                      |
| ----------- | -- | ----------------------- |
| grant\_type | 是  | 固定为 client\_credentials |
| scope       | 否  | 权限范围                    |

请求示例：

```
POST /token HTTP/1.1
Host: server.example.com
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials
```

### 更新令牌（Refresh Token）

当 access\_token 过期时，客户端使用 refresh\_token 申请新的 access\_token：

| 参数             | 必选 | 说明                          |
| -------------- | -- | --------------------------- |
| grant\_type    | 是  | 固定为 refresh\_token          |
| refresh\_token | 是  | 早前收到的更新令牌                   |
| scope          | 否  | 授权范围（不可超出上一次申请范围，省略则与上一次一致） |

请求示例：

```
POST /token HTTP/1.1
Host: server.example.com
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&refresh_token=tGzv3JOkF0XG5Qx2TlKWIA
```

### 安全要点

- 所有通信必须使用 HTTPS。
- redirect\_uri 必须精确匹配注册时的值（防止授权码被转发到恶意站点）。
- state 参数必须随机且一次性，防止 CSRF 攻击。
- Access Token 过期时间要短，配合 Refresh Token 使用。
- 客户端密钥（client\_secret）绝不能出现在前端代码中。
- Token 响应的 HTTP 头必须指定 no-store，防止缓存泄漏。

