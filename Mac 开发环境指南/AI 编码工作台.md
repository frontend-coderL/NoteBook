## 管理与接入工具

### cc-switch

跨平台桌面一体化工具，统一管理多个 AI 编码 CLI（Claude Code、Codex、OpenCode、OpenClaw、Gemini CLI 等）的安装、配置、Provider 切换与启动。

- **官方仓库**：[farion1231/cc-switch](https://github.com/farion1231/cc-switch) ⭐

**核心能力**

- **多 CLI 统一管理**：在同一个 GUI 里装、卸、切换 Claude Code / Codex / OpenCode / OpenClaw / Gemini CLI。
- **Provider 切换**：内置多家 API / 中转服务商（官方、第三方代理、自建中转），一键切换当前激活的 Provider，免改环境变量。
- **配置集中化**：CLI 的 config / auth / MCP 配置文件统一管理，避免散落在多个目录。
- **导入现有配置**：可一键发现并导入已存在的 CLI 配置。

### CLIProxyAPI

面向 CLI 的代理服务器，为多种 AI 编码 CLI 提供统一的兼容接口、中转转发与本地代理能力。

- [CLIProxyAPI](https://github.com/router-for-me/CLIProxyAPI)：提供 OpenAI、Gemini、Claude、Codex、Grok 兼容 API，也支持 Claude Code 与 Codex 的 OAuth 接入。

## AI 编码工具

### Claude Code

Anthropic 官方终端编码 Agent，原生支持 MCP、Skills、Hooks、子代理。

- **官方文档**：[Claude Code overview](https://code.claude.com/docs/en/overview) ⭐
- [claude code 最佳实践](https://github.com/shanraisshan/claude-code-best-practice)
- [claude-howto](https://github.com/luongnv89/claude-howto)

#### 接入

- [free-claude-code](https://github.com/Alishahryar1/free-claude-code)：轻量级代理，可将 Claude Code 的 Anthropic API 调用路由到 NVIDIA NIM、OpenRouter、DeepSeek、LM Studio、llama.cpp 或 Ollama。

#### 配置

- `/tui fullscreen`：切换到无闪烁全屏渲染模式。
- `auto mode`：`--permission-mode auto` 开启自动模式，Claude 决定什么是安全的，同时阻止提示注入和风险升级。
- `Chrome`：`--chrome` 通过 Chrome 中的 Claude 实现浏览器自动化，用于测试网页应用、控制台调试、表单自动化和页面数据提取。
- `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS=1`：自动去掉实验性 beta 请求头，避免网关拒绝。
- `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB=1`：子进程自动清除敏感凭证。
- `CLAUDE_CODE_NO_FLICKER=1`：终端不卡顿。
- `CLAUDE_CODE_ENABLE_AWAY_SUMMARY=1`：离开一会儿回来，自动生成会话摘要。

#### 常用命令

- `/init`：`export CLAUDE_CODE_NEW_INIT=1` 开启新范式，[新版 /init 配置新范式](https://mp.weixin.qq.com/s/IsuE58_Hukeabtv6G9BY0w)。
- `/memory`：打开记忆管理界面。
- `/rewind`：回滚到之前的某个对话节点，也可双击 `Esc`。
- `/compact`：压缩上下文信息，上下文达到 40% 到 60% 时就应该进行压缩。
- `/simplify`：识别冗余代码、简化复杂逻辑，聚焦代码可读性、重复逻辑和错误处理。
- `/review`：代码审查，提供规范检查和改进建议。
- `/batch`：将大规模代码修改或迁移任务拆解为多个子任务并行执行。

#### 快捷键

- `Ctrl + U`：清空整个输入缓冲；`Ctrl + Y` 恢复。
- `Ctrl + L`：在清空输入之外增加全屏重绘。
- `Ctrl + R`：搜索历史提示词。
- 前缀 `!`：内联运行 bash 命令，如 `!git status`。

#### CLAUDE.md 与规则组织

- **Slash 命令**：在 `~/.claude/commands/` 下创建 Markdown 文件，文件名即指令名，`$ARGUMENTS` 表示命令后的参数。
- **CLAUDE.md**：尽量小于 200 行，支持用 `@path/to/file` 导入其他文件内容。

```Shell
# 项目概述
参见 @README 获取项目简介
# 可用命令
参见 @package.json 了解所有 npm scripts
# 工作流程
遵循 @docs/git-workflow.md 中的 Git 分支策略
```

- **规则拆分**：大型项目可使用 `.claude/rules/` 组织独立规则文件。

```Shell
your-project/
├── .claude/
│   ├── CLAUDE.md           # 主项目指令
│   └── rules/
│       ├── code-style.md   # 代码风格指南
│       ├── testing.md      # 测试约定
│       ├── api-design.md   # API 设计规范
│       └── security.md     # 安全要求
```

- [awesome-design-md](https://github.com/VoltAgent/awesome-design-md)：一组受流行品牌设计系统启发的 `DESIGN.md` 文件合集，把一个设计约束插件放进项目，让编码代理生成更匹配的界面。
- 技能清单见 [Agent Skills.md](file:///Users/zhonglvshiqi/Desktop/NoteBook/Mac%20开发环境指南/Agent%20Skills.md)。

#### Claude 专属周边

- [ccstatusline](https://github.com/sirmalloc/ccstatusline) ⭐：高度可定制的状态行，显示模型、分支、Token 用量等。
- [claude-hud](https://github.com/jarrodwatts/claude-hud)：插件式 HUD。
- [cc-lens](https://github.com/Arindam200/cc-lens)：Claude Code 的实时监控仪表盘。

### Codex

OpenAI 推出的编码代理，CLI、桌面 App、IDE 插件、云端 Web 共用同一套账号。

- **官方仓库**：[openai/codex](https://github.com/openai/codex)（Apache-2.0，Rust 为主）
- **形态入口**：
  - CLI：`codex`
  - 桌面 App：`codex app` 或 [chatgpt.com/codex](https://chatgpt.com/codex?app-landing-page=true)
  - IDE 插件（VS Code / Cursor / Windsurf）：[developers.openai.com/codex/ide](https://developers.openai.com/codex/ide)
  - 云端 Web：[chatgpt.com/codex](https://chatgpt.com/codex)
- **文档入口**：[developers.openai.com/codex](https://developers.openai.com/codex)

### OpenCode

开源 AI 编码代理，终端、IDE、桌面三端通用。

- **官网**：[opencode.ai](https://opencode.ai/)
- **桌面版**：macOS / Windows / Linux 均有，目前 beta。

#### 模型与认证

- 通过 [Models.dev](https://models.dev/) 接入 75+ LLM 提供商，Claude、GPT、Gemini、本地模型均可。
- 兼容 GitHub Copilot、ChatGPT Plus / Pro 账户直接登录复用，无需额外付费。
- **Zen**：经过基准测试的精选模型，适合不想挑模型的场景。
- **隐私优先**：不存储代码或上下文数据。

#### 核心能力

- **LSP enabled**：自动加载合适的 LSP（TypeScript、Python、Go 等），上下文更准。
- **Multi-session**：并行启动多个 Agent 处理独立任务，适合多分支并行开发。
- **Share links**：会话生成可分享链接，方便协作调试。
- **Any editor**：终端、桌面 App、IDE 插件均可用。

## AI 设计与辅助工具

### open-codesign

开源 Claude Design 替代品，把 Prompt 直接转成可交付的设计产物（原型 / 幻灯片 / PDF）。

- **官方仓库**：[OpenCoworkAI/open-codesign](https://github.com/OpenCoworkAI/open-codesign)

#### 核心特性

- **多模型支持**：Claude、GPT、Gemini、Kimi、GLM、Ollama 自带模型皆可，亦支持 BYOK（自带 API Key）。
- **本地优先**：MIT 协议，代码可在本地跑。
- **一键导入 API Key**：复用已有的 Claude Code / Codex 凭证，免重复配置。
- **多场景输出**：同一个 Prompt 可出网页原型、演示幻灯片、PDF 文档。

## 通用基础设施

### MCP 服务器

通用 MCP 服务器实现，可接入 Claude Code、Codex、Cursor、OpenCode 等所有支持 MCP 协议的客户端。

- **Context7** ⭐：实时检索文档，可将特定版本的文档与代码示例从源码仓库提取到 LLM 上下文。
- [firecrawl-mcp-server](https://github.com/firecrawl/firecrawl-mcp-server)：为 Cursor、Claude 等 LLM 客户端添加网页爬取与搜索能力。
- [github-mcp-server](https://github.com/github/github-mcp-server)：GitHub 官方 MCP 服务器，用自然语言管理代码仓库。
- [supabase-mcp](https://github.com/supabase-community/supabase-mcp)：将 Supabase 连接到 AI 助手，用中文问数据库问题。
- [publora/mcp-server](https://github.com/publora/mcp-server)：通过 Claude、Cursor 等 AI 助手控制社交媒体日程，一条指令多平台发布。

### 记忆 / 图谱 / 压缩

跨 AI 编码工具复用的记忆系统、知识图谱与上下文压缩工具。

- [claude-mem](https://github.com/thedotmack/claude-mem)：自动捕捉编程会话中的所有操作，用 AI 压缩，并在未来会话注入相关上下文；同时支持 Claude Code 与其他 AI 编码工具。
- [context-mode](https://github.com/mksglu/context-mode)：AI 编码代理的上下文窗口优化。
  - **沙盒隔离**：沙盒工具将原始数据排除在上下文窗口外。
  - **会话存档**：SQLite 记录每次文件改动、命令执行、错误。
  - **代码思维**：不读 50 个文件到上下文，而是写脚本计数并 `console.log` 结果。
  - 使用：`/context-mode:ctx-purge`（清空索引）、`/context-mode:ctx-insight`（本地仪表盘）。
- [MemOS](https://github.com/MemTensor/MemOS)：面向 LLM 和代理系统的 AI 内存操作系统（moltbot、clawdbot、openclaw），实现持久技能内存，支持跨任务技能的重用和演进。
- [mempalace](https://github.com/MemPalace/mempalace)：最佳基准测试的开源 AI 记忆系统。
- [OpenViking](https://github.com/volcengine/OpenViking)：专为 AI 智能体设计的开源上下文数据库，用“文件系统范式”统一组织记忆、资源和技能。
  - [Claude Code 记忆插件](https://github.com/volcengine/OpenViking/blob/main/examples/claude-code-memory-plugin/README.md)
- [agentmemory](https://github.com/rohitg00/agentmemory)：基于 iii-engine 的持久化记忆系统。
  - **跨 Agent 共享**：Claude Code、Cursor、OpenClaw 都支持。
  - **自动压缩与检索**：会话结束后自动压缩成可搜索的记忆，下次启动自动注入。
  - **MCP 协议**：任何支持 MCP 的客户端都能接入。
  - **知识图谱**：支持实体关系，不只是文本。
- [Beads](https://github.com/gastownhall/beads)：为编码代理提供持久的结构化内存，用依赖感知图取代混乱的标记列表，通过 Dolt（版本控制 SQL 数据库）存项目任务、依赖、进度。
- [graphify](https://github.com/safishamsi/graphify)：把任意包含代码、文档、论文、图片或视频的文件夹变成可查询的知识图谱。
  - 适用：接手缺乏文档的祖传项目；将零散技术文档、会议记录与代码库强关联，构建团队级知识底座。
- [GitNexus](https://github.com/abhigyanpatwari/GitNexus)：把代码库索引为完整知识图谱，Tree-sitter 解析 AST，再用 KuzuDB 存依赖关系。
  - 一条命令 `npx gitnexus analyze` 完成索引、安装 skill、注册 Claude Code hooks、生成 `AGENTS.md` / `CLAUDE.md`。
  - 14 种语言支持。商业授权另算。
  - 适用：几十万行的巨石应用或 Monorepo，重构前评估“牵一发而动全身”的破坏力。
- [Understand-Anything](https://github.com/Lum1104/Understand-Anything)：多智能体 + 知识图谱，将代码库转为可探索、可搜索、可问答的交互式图谱。
  - 核心：交互式 Dashboard、影响分析、导览路线、模糊语义搜索、架构层可视化。
  - 3 分钟生成初始图谱；Tree-sitter + LLM 架构；中英日韩 6 种语言。
  - 适合：新人入职理解大代码库、接手遗留项目、团队知识共享、架构 review 可视化。
  - 注意：10 万行以上代码库首次初始化建议在空闲时段运行；Token 消耗较高。
- [CodeGraph](https://github.com/colbymchenry/codegraph)：AI 编码工具的缓存层，把代码库建成可查询的知识图谱，降低 Token 消耗。
  - 懒加载索引、熵值监控、MCP 异步支持。
  - 适合：大型代码库维护者、AI 编码工具重度用户、技术债管理。
- [code-review-graph](https://github.com/tirth8205/code-review-graph)：基于 Tree-sitter 的增量式代码知识图谱，6.8 倍到 49 倍 Token 削减。
  - 注意：需要 Python 3.10+；首次建图时间较长；Claude Code 用户安装后需重启生效；不是所有语言都完美支持。
  - 适用：日常业务开发、频繁修 Bug 和提 PR。

**Token 压缩**

- [caveman](https://github.com/JuliusBrussee/caveman)：AI 输出砍半，Token 费用可降 75%；4 种强度模式（Lite / Full / Ultra / 文言文）。
- [rtk](https://github.com/rtk-ai/rtk)：高性能 CLI 代理，智能过滤与压缩命令输出，可省 60% 到 90% Token。

<br />

