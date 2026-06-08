<br />

> 来源：[让 AI 生成的代码可以"点哪改哪"：React 可视化编辑器实现原理](https://mp.weixin.qq.com/s?__biz=MzI0NDYwMTY4Mg==\&mid=2247483741\&idx=1\&sn=be7e25cd41b9219887590a0f62deb713\&chksm=e87b85f637ee6408d742fc266b58f18a517ef634e272b8c01036331c50fa945fbaa7a063b410\&mpshare=1\&scene=1\&srcid=0123P3kCklGmhngjzSoaftT1\&sharer_shareinfo=6e757541c6e923ec9b7dfa1e626ad466\&sharer_shareinfo_first=6e757541c6e923ec9b7dfa1e626ad466#rd)

亮点：

- 编译时注入 `data-vid` 标识，将浏览器中的 DOM 元素精准映射回 `.tsx/.jsx` 源码位置
- 浏览器内直接点选元素、编辑样式和内容，保存后回写源码而非仅改 DOM
- AST 修改 + HMR 无缝刷新，改完即生效，刷新不丢失
- 两种实战方案：Vite + Babel（门槛低）和 Next.js + SWC（性能好），均附源码仓库
- 适用场景：AI 生成的代码需要精细微调，非技术用户也能所见即所得地调整页面

## 核心流程

四个步骤形成闭环：

1. 编译时注入标识 → 2. 浏览器点选编辑 → 3. 元素到源码定位 → 4. AST 修改 + HMR

### 1. 编译时注入标识

在构建阶段通过 AST 插件为每个 JSX 节点注入自定义属性 `data-vid`，值为「文件路径 + 元素 + 索引」的组合标识。

- Vite 方案：Babel 插件在 `transform` 阶段遍历 AST 注入
- Next.js 方案：Rust 编写的 SWC 插件（编译为 `wasm32-wasip1`）注入

### 2. 浏览器中的点选与编辑

开启可视化编辑后：

- 选中元素高亮显示
- 元素进入 `contenteditable` 编辑状态
- 弹出样式面板编辑 `className` / `style` 等属性
- 以上修改先作用于 DOM 实现实时预览，后续持久化到源码

### 3. 元素到源码的定位策略

通过 `data-vid` 将浏览器中的元素精确映射回源码的三个信息：哪个文件、哪个节点、改了什么。

两种定位策略对比：

| 策略             | 原理                                                 | 优点           | 缺点               |
| -------------- | -------------------------------------------------- | ------------ | ---------------- |
| AST Path（节点路径） | 记录每个 JSX 节点的父节点链路（如 `body.4.body.body.1.argument`） | 定位精确，格式化后仍有效 | 对代码结构变化敏感        |
| DFS 全局 index   | 按深度优先遍历顺序为 JSX 节点编号                                | 实现简单，对格式化稳定  | 插入 / 删除节点会导致序号漂移 |

### 4. AST 修改源码 + HMR

```
前端保存 → 请求发到 dev server middleware
→ 读取源文件 → 解析为 AST（Babel parser / @swc/core）
→ 根据定位策略找到目标节点
→ 修改 AST（更新 text / className / style 等）
→ 打印回代码字符串（recast / swc print）
→ 写回文件
→ dev server 检测变更触发 HMR
```

## 方案一：Vite + Babel

```JavaScript
// 1. Vite 插件 transform 阶段注入 data-vid
transform(code, id) {
  const ast = parser.parse(code, { plugins: ['jsx'] })
  traverse(ast, {
    JSXOpeningElement(path) {
      const vid = `${id}:${path.node.name.name}:${index++}`
      path.node.attributes.push(
        t.jsxAttribute(
          t.jsxIdentifier('data-vid'),
          t.stringLiteral(vid)
        )
      )
    }
  })
}
```

```JavaScript
// 2. 保存时根据 VID 查找映射，AST 回写源码
const ast = parser.parse(sourceCode, { plugins: ['jsx'] })
traverse(ast, {
  JSXOpeningElement(path) {
    const attr = path.node.attributes.find(a => a.name?.name === 'data-vid')
    if (attr?.value?.value === targetVid) {
      // 更新 style / className / children 等
      updateASTAttributes(path.node, changes)
    }
  }
})
const newCode = generate(ast).code
fs.writeFileSync(filePath, newCode)
// Vite 自动检测文件变更触发 HMR
```

源码仓库：[react-visual-edit-vite](https://github.com/Jiangultimo/react-visual-edit-vite)

## 方案二：Next.js + SWC

```Rust
// Rust SWC 插件编译时注入 data-vid
impl VisitMut for VisualEditPlugin {
    fn visit_mut_jsx_opening_element(&mut self, node: &mut JSXOpeningElement) {
        let vid = format!("{}:{}:{}", self.file_path, element_name, self.index);
        node.attrs.push(JSXAttrOrSpread::JSXAttr(JSXAttr {
            name: JSXAttrName::Ident(Ident::new("data-vid".into(), DUMMY_SP)),
            value: Some(JSXAttrValue::Lit(Lit::Str(Str {
                value: vid.into(), ..Default::default()
            }))),
            span: DUMMY_SP,
        }));
        self.index += 1;
    }
}
```

```JavaScript
// Next.js 配置启用 SWC 插件（注意 .wasm 必须用相对路径）
// next.config.js
module.exports = {
  experimental: {
    swcPlugins: [
      ['./swc_plugin.wasm', {}]
    ]
  }
}
```

```JavaScript
// 保存时 API 路由使用 @swc/core 回写源码
import { parseSync, printSync } from '@swc/core'
const ast = parseSync(sourceCode, { syntax: 'typescript', tsx: true })
// 通过 DFS 全局 index 定位目标节点并修改
// printSync 输出新代码写回文件 → 触发 Next Fast Refresh
```

注意事项：

- `swc_core` 版本必须与 Next.js 匹配，查 [plugins.swc.rs](https://plugins.swc.rs) 确认对应版本
- `swcPlugins` 中 `.wasm` 必须使用相对路径，绝对路径会报 `Module not found`（参考 [next.js#78156](https://github.com/vercel/next.js/issues/78156#issuecomment-3270293327)）

源码仓库：[react-visual-edit-swc](https://github.com/Jiangultimo/react-visual-edit-swc)

## 两种方案对比

| 维度   | Vite + Babel               | Next.js + SWC              |
| ---- | -------------------------- | -------------------------- |
| 编译注入 | JS Babel 插件，开发快            | Rust SWC 插件，编译为 WASM       |
| 定位策略 | AST Path 或 DFS index       | DFS 全局 index               |
| 源码回写 | Babel AST + generator，生态成熟 | @swc/core parse/print，性能好  |
| HMR  | Vite HMR，链路简单              | Next Fast Refresh / reload |
| 开发门槛 | 低，调试友好                     | 高，需 Rust 工具链               |
| 性能   | 中等                         | 高                          |

## 生产环境的待解问题

- 动态内容（条件渲染、列表）的安全编辑边界
- 样式抽象化（CSS-in-JS / Tailwind 等非内联样式的修改入口）
- 源码版本管理与 diff 回滚机制
- 编辑态 dev server 的生命周期管理

