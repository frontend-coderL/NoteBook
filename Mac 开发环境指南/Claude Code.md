## 安装

- [Claude Code overview - Claude Code Docs](https://code.claude.com/docs/en/overview) ⭐
  ```Shell
  curl -fsSL https://claude.ai/install.sh | bash
  ```
- [cc-switch](https://github.com/farion1231/cc-switch) ⭐：一款跨平台桌面一体化辅助工具，支持 Claude Code、Codex、OpenCode、openclaw 和 Gemini CLI。
  ```Shell
  # 通过 Homebrew 安装
  brew tap farion1231/ccswitch
  brew install --cask cc-switch

  # 更新
  brew upgrade --cask cc-switch
  ```
- [CLIProxyAPI](https://github.com/router-for-me/CLIProxyAPI)：本地中转，Claude Code 只需要接入本地中转**URL+API Key**，中转服务再把请求转发给对应的服务商。
- [free-claude-code](https://github.com/Alishahryar1/free-claude-code)：一个轻量级代理，将 Claude Code 的 Anthropic API 调用路由到 **NVIDIA NIM**（免费 40 次/分钟）、**OpenRouter**（数百个模型）、**DeepSeek**（直接 API）、**LM Studio**（完全本地）、**llama.cpp**（本地，Anthropic 端点）或 **Ollama**（完全本地、原生 Anthropic 消息）。

> coding套餐

- [意心 AI](https://yxai.chat/coding-plan)

## 配置与使用技巧

> [claude code 最佳实践](https://github.com/shanraisshan/claude-code-best-practice)
> [GitHub - luongnv89/claude-howto: A visual, example-driven guide to Claude Code — from basic concepts to advanced agents, with copy-paste templates that bring immediate value.](https://github.com/luongnv89/claude-howto)

### 配置

- `/tui fullscreen`：切换到无闪烁全屏渲染模式
- `auto mode`：`--permission-mode auto` 开启自动模式，Claude 决定什么是安全的，同时阻止提示注入和风险升级
- `Chrome`：`--chrome` 通过 Chrome 中的 Claude 实现浏览器自动化——测试网页应用，使用控制台调试，自动化表单，提取页面数据
- `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS=1`：自动去掉实验性 beta 请求头，避免网关拒绝。
- `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB=1`：子进程自动清除敏感凭证
- `CLAUDE_CODE_NO_FLICKER=1`：终端不卡顿
- `CLAUDE_CODE_ENABLE_AWAY_SUMMARY=1`：离开一会儿回来，自动给你生成会话摘要

### 命令

- `/init`：`export CLAUDE_CODE_NEW_INIT=1`开启新范式，[Claude Code 新版 /init 配置新范式](https://mp.weixin.qq.com/s/IsuE58_Hukeabtv6G9BY0w)
- `/memory`：打开记忆管理界面
- `/rewind`：可以回滚到之前的某个对话节点，也可以双击`Esc`
- `/compact`：压缩上下文信息，上下文达到 40%-60% 就应该进行压缩
- `/simplify`：识别冗余代码、简化复杂逻辑，前身是 `code-simplifier`
  - /simplify 聚焦代码可读性、重复逻辑、冗余复杂度以及错误处理
- `/review`：代码审查，提供规范检查和改进建议，前身是`code-review`
- `/batch`：大规模代码修改或迁移任务自动拆解为多个子任务，并行执行，批量操作功能。

### 快捷键

- `Ctrl` + `U`：改为**清空整个输入缓冲**；`Ctrl+Y` 恢复
- `Ctrl` + `L`：在清空输入之外增加**全屏重绘**
- `Ctrl` + `R`：搜索历史提示词
- 前缀!用于内联运行bash命令，如：`!git status`

### 提升

- `Slash`：预设提示词，`~/.claude/commands/`下创建 md 文件，文件名是指令名，`$ARGUMENTS`是命令后的参数
- `CLAUDE.md`文件
  - CLAUDE.md 尽量小于200行
  - 支持使用 `@path/to/file` 导入其他文件的内容
  ```Shell
  # 项目概述
  参见 @README 获取项目简介
  # 可用命令
  参见 @package.json 了解所有 npm scripts
  # 工作流程
  遵循 @docs/git-workflow.md 中的 Git 分支策略
  ```
  - 使用 .claude/rules/ 组织大型项目
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
- [awesome-design-md](https://github.com/VoltAgent/awesome-design-md)：一组受流行品牌设计系统启发的 DESIGN.md 文件合集。把一个插件放进你的项目，让编码代理生成匹配的用户界面。

## MCP 服务器

- Context7 ⭐：用于实时检索文档。可将特定版本的文档与代码示例从源码仓库提取至大语言模型（LLM）的上下文中。
- [firecrawl-mcp-server](https://github.com/firecrawl/firecrawl-mcp-server)：Firecrawl MCP 服务器——为 Cursor、Claude 及其他大型语言模型客户端添加了强大的网页爬取和搜索功能。让AI能读懂任何网页
- [github-mcp-server](https://github.com/github/github-mcp-server)：GitHub 官方 MCP 服务器，用自然语言管理你的代码仓库
- [supabase-mcp](https://github.com/supabase-community/supabase-mcp)：将 Supabase 连接到您的 AI 助手，用中文问数据库问题
- [publora/mcp-server](https://github.com/publora/mcp-server)——通过 Claude、Cursor 等 AI 助手控制社交媒体日程。一条指令，多平台发布

## 记忆/图谱/压缩

- [claude-mem](https://github.com/thedotmack/claude-mem)\*\*：\*\*一个 Claude Code 插件，能自动捕捉 Claude 在你编程会话中的所有操作，用 AI 压缩，并在未来的会话中注入相关上下文
- [context-mode](https://github.com/mksglu/context-mode)：AI 编码代理的上下文窗口优化。
  - 作用：
    - 沙盒隔离：沙盒工具将原始数据排除在上下文窗口之外。
    - 会话存档：创建一个 SQLite 数据库，每次你改了文件、跑了命令、遇到了错误，全部记录下来。
    - 代码思维：不需要读取 50 个文件到上下文中，而是编写脚本进行计数，并只 `console.log` 结果。
  - 使用：
    - `/context-mode:ctx-purge`：清空所有索引，开新项目
    - `/context-mode:ctx-insight`：打开本地仪表盘，查看15+个使用指标
- MemOS：[GitHub - MemTensor/MemOS](https://github.com/MemTensor/MemOS)
  - 面向大型语言模型和代理系统的 AI 内存操作系统（moltbot、clawdbot、openclaw），实现了持久技能内存，支持跨任务技能的重用和演进。
- [GitHub - MemPalace/mempalace: The best-benchmarked open-source AI memory system. And it's free.](https://github.com/mempalace/mempalace)
- [OpenViking](https://github.com/volcengine/OpenViking)：是专为 AI 智能体设计的开源**上下文数据库**。
  - OpenViking 抛弃了传统 RAG 的碎片化向量存储模型，创新性地采用 **"文件系统范式"** 来统一组织智能体所需的记忆、资源和技能。
  - [Claude Code 的 OpenViking 记忆插件](https://github.com/volcengine/OpenViking/blob/main/examples/claude-code-memory-plugin/README.md)
- [agentmemory](https://github.com/rohitg00/agentmemory)：一个基于 iii-engine 构建的持久化记忆系统。
  - 跨 Agent 共享记忆：Claude Code、Cursor、OpenClaw 都支持，所有 Agent 共享
  - 自动压缩和检索：会话结束后自动压缩成可搜索的记忆，下次启动时自动注入相关上下文
  - 支持 MCP 协议：任何支持 MCP 的客户端都能接入
  - 内置知识图谱：不只是文本记忆，还能建立实体关系
- [Beads](https://github.com/gastownhall/beads)：为编码代理提供持久的结构化内存。它用依赖感知图取代了混乱的标记降计划，使代理能够处理长视野任务而不丢失上下文。把项目任务、依赖关系、进度全部存在一个 Dolt（版本控制 SQL 数据库）里，然后通过 CLI 命令暴露给 AI 编程工具。

> 代码图谱的记忆系统  [让 Agent 读懂代码库:代码知识图谱框架调研(理论篇)](https://mp.weixin.qq.com/s/5B-QYdMEvoCiAPp4FGlk_Q)

- [code-review-graph](https://github.com/tirth8205/code-review-graph)：基于 Tree-sitter 的增量式代码知识图谱构建工具，专门为 AI 辅助代码评审场景优化。6.8倍至 49倍 Token 削减
  - 注意：需要Python 3.10+，首次建图时间可能较长，install 后需要重启Claude Code，不是所有语言都完美支持
  - 适用场景：日常业务代码开发、频繁地修 Bug 和提交 PR。
- [graphify](https://github.com/safishamsi/graphify)：将任何包含代码、文档、论文、图片或视频的文件夹变成可查询的知识图谱。确定的代码结构用 Tree-sitter 本地解析（零 Token 消耗），而模糊的图文资料则用 Vision 模型并行提取。
  - 适用场景：接手一个缺乏文档的祖传项目（需要边看代码边写文档）；或者你需要将零散的技术文档、会议记录与代码库强关联起来，构建一个团队级的知识底座。
- [GitNexus](https://github.com/abhigyanpatwari/GitNexus)：将代码库索引为完整的知识图谱。利用 Tree-sitter 解析成 AST，再用 KuzuDB 这样的图数据库来存储依赖、调用链和集群关系。商业要授权
  - 一条命令 `npx gitnexus analyze` 跑完所有事——索引、装 skill、注册 Claude Code hooks、生成 `AGENTS.md` / `CLAUDE.md`
  - 14 种语言（TypeScript / JavaScript / Python / Java / Kotlin / C# / Go / Rust / PHP / Ruby / Swift / C / C++ / Dart）
  - 适用场景：几十万行的巨石应用或错综复杂的 Monorepo，需要理清模块边界、追踪跨文件的执行流，或者在重构前评估“牵一发而动全身”的破坏力时。
- [Understand-Anything](https://github.com/Lum1104/Understand-Anything)：依托多智能体架构与知识图谱技术，将复杂代码库转化为可探索、可搜索、可问答的交互式图谱。
  - 一键图谱：自动检测项目类型，选择最优索引策略，3 分钟生成初始图谱。
  - 书签功能：重要区域子图保存为书签，点击直达，无需重新搜索。
  - 时间轴视图：展示代码库重要变化节点，对比变化前后的图谱差异。
  - AI 问答：高亮标注回答对应的节点和路径，文字 + 视觉双重理解。
  - 适合：接手陌生代码库、技术 Leader 架构评审、重构前摸底。
  - 注意：超大代码库（10 万行+）首次初始化建议在空闲时段运行；Token 消耗较高、超大图谱可能卡顿。
- [CodeGraph](https://github.com/colbymchenry/codegraph)：AI 编码工具的"缓存层"，把代码库建成可查询的知识图谱，降低 Token 消耗。
  - 懒加载索引：按需索引核心文件，大型代码库首次索引从分钟级降到秒级。
  - 熵值监控：量化代码库无序程度，追踪代码库健康趋势。
  - MCP 异步支持：多个查询并行执行，不互相阻塞。
  - 适合：大型代码库维护者、AI 编码工具重度用户、技术债管理。

> Token 压缩

- [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman)：AI输出砍半，Token费用直接降75%
  - 4种强度模式。Lite轻度压缩，Full标准模式，Ultra极限压缩，还有一个文言文模式，用中文古文输出。
- [rtk](https://github.com/rtk-ai/rtk)：一款高性能 CLI 代理工具，通过智能过滤、压缩命令输出，可为 LLM 节省 60-90% 的 token 消耗。

## 周边工具

> 状态栏插件

- [ccstatusline](https://github.com/sirmalloc/ccstatusline) ⭐：一个高度可定制的 Claude 代码状态行格式化器 ，显示*模型信息、git 分支、令牌使用情况及其他指标，显示终端中的*数据
  ```Shell
  # Run the configuration TUI with npm
  npx -y ccstatusline@latest
  # Or with Bun (faster)
  bunx -y ccstatusline@latest

  # 中文版
  npm install -g ccstatusline-zh
  # or
  bun install -g ccstatusline-zh

  ccstatusline-zh setup
  ```
- **claude-hud**：[GitHub - jarrodwatts/claude-hud](https://github.com/jarrodwatts/claude-hud)
  ```Shell
  /plugin marketplace add jarrodwatts/claude-hud
  /plugin install claude-hud
  /claude-hud:setup
  ```
- [cc-lens](https://github.com/Arindam200/cc-lens)：Claude Code 的实时监控仪表盘
- [codeburn](https://github.com/getagentseal/codeburn?ref=dailydev)：Claude Code、Codex 和 Curser 成本可观察的交互式 TUI 仪表盘。

> 设计系统

- [GitHub - OpenCoworkAI/open-codesign: Open-source Claude Design alternative. One-click import your Claude Code / Codex API key. Prompt → prototype / slides / PDF. Multi-model (Claude, GPT, Gemini, Kimi, GLM, Ollama). BYOK, local-first, MIT.](https://github.com/OpenCoworkAI/open-codesign)

