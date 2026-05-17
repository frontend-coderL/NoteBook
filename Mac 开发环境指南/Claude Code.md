## 安装

- [Claude Code overview - Claude Code Docs ⭐](https://code.claude.com/docs/en/overview)

    ```Shell
    curl -fsSL https://claude.ai/install.sh | bash
    ```

- [cc-switch ⭐](https://github.com/farion1231/cc-switch)：一款跨平台桌面一体化辅助工具，支持 Claude Code、Codex、OpenCode、openclaw 和 Gemini CLI。

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
[GitHub - luongnv89/claude-howto: A visual, example-driven guide to Claude Code — from basic concepts to advanced agents, with copy-paste templates that bring immediate value.](https://github.com/luongnv89/claude-howto)



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



## 工作 Skills

> 官方市场：[Plugins for Claude Code and Cowork | Anthropic](https://claude.com/plugins)
Vercel市场：[The Agent Skills Directory](https://skills.sh/)
[SkillHub-专为中国用户优化的Skills社区](https://skillhub.cn/)
[ClawHub](https://clawhub.ai/)
[SkillsLLM](https://skillsllm.com/)：[SkillsLLM - AI Skills Marketplace](https://skillsllm.com/)
[Agent Skills Marketplace - Claude, Codex & ChatGPT Skills | SkillsMP](https://skillsmp.com/)



### 工具技能

- [find-skills ⭐](https://skills.sh/vercel-labs/skills/find-skills)：一个技能搜索引擎，帮助用户快速定位并调用当前场景下最合适的工具。

- [skill-creator ⭐](https://github.com/anthropics/skills/tree/main/skills/skill-creator)：允许用户将个人的经验、特定的业务逻辑封装成可复用的技能。



### 工作流

- [Superpowers ⭐](https://github.com/obra/superpowers)：TDD（测试驱动开发）- 多代理协作的完整开发工作流，强制流程约束 AI 的执行过程。

    - `/superpowers:brainstorm`：设计规划技能 — 把模糊的想法变成具体的设计方案。了解项目现状；逐一提问，理解需求、目的、约束；提出 2-3 个方案并给出推荐；产出设计文档并保存到文件；最终调用 writing-plans 生成实施计划

    - 子智能体驱动开发：可以创建专门化的子智能体执行特定任务，主智能体负责协调

    - Git工作树隔离：每个任务在独立的Git工作树里开发，互不干扰

    - 系统化调试技能：不是靠"试试看"，而是有结构的问题诊断和解决流程

- [mattpocock/skills](https://github.com/mattpocock/skills)[ ⭐](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/claude-md-management)：一套前端开发工具，解决人和 Agent 之间沟通对不齐的问题

    - **开发类**

        - `/setup-matt-pocock-skills`⭐：初始化，生成CONTEXT.md和ADR 文档。（术语表和架构决策记录）

        - `/grill-with-docs`⭐：逐个分支拷问你的计划，对标项目已有术语表（CONTEXT.md）和架构决策记录（ADRs），同时实时更新。

        - `/tdd`：测试驱动开发，严格的 red-green-refactor 循环

        - `/improve-codebase-architecture`⭐：反熵增任务 - 每隔几天跑一次，找代码深度化的机会。扫描代码库，寻找可以让接口更简洁、内聚更强的重构机会。

        - `/diagnose`⭐：bug 找不到根因、问题复杂时使用 - 先复现、再二分、再形成假设、再验证。禁止猜

        - `/prototype`⭐：快速验证一个想法、可选方案对比、实验性代码；快速写出可运行代码

    - **工具与工程化类**

        - `/zoom-out` 强制 Agent 站在整个系统视角解释代码

        - `/git-guardrails-claude-code`：拦截 `push --force`/`reset --hard`/`clean -fd` 等危险命令

    - 规划与设计类

        - `/grill-me`：反复追问设计中的漏洞，直到所有分支都覆盖到

        - `/to-prd`：把对话变成 PRD - 在生成 PRD 之前先问清楚涉及哪些模块

        - `/to-issues`：把口头描述、随意想法转化成结构化的 GitHub Issue，局限性：主要是 GitHub Issues 格式

        - `/triage`：按优先级把 issue 分类，输出 issue 处理顺序建议，局限性：主要是 GitHub Issues 流程

        - `/handoff`生成 Agent 交接文档，把上下文留住。



### 开发规范与文档维护

- [andrej-karpathy-skills/karpathy-guidelines](https://github.com/forrestchang/andrej-karpathy-skills) ⭐：四原则：编码前思考、简洁优先、精准修改、目标驱动执行

- [claude-plugins-official/claude-md-management ⭐](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/claude-md-management)：Claude官方技能

    - `/claude-md-improver`：扫描、评估并改进项目 CLAUDE.md 文件质量的工具，确保文档准确、可操作。

- [KKKKhazix/khazix-skills](https://github.com/KKKKhazix/khazix-skills)[ ⭐](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/claude-md-management)：数字生命卡兹克开源的 AI Skills 合集

    - [neat-freak](https://github.com/KKKKhazix/khazix-skills#-neat-freak%E6%B4%81%E7%99%96)[ ⭐](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/claude-md-management)：干完活跑一下 ，自动把你这次改的东西跟项目文档、CLAUDE.md、Agent 记忆全部对齐

    - [hv-analysis（横纵分析法）](https://github.com/KKKKhazix/khazix-skills#-hv-analysis%E6%A8%AA%E7%BA%B5%E5%88%86%E6%9E%90%E6%B3%95)[⭐](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/claude-md-management)：想搞懂一个产品/公司/概念是怎么回事，给一份万字 PDF 研究报告

- [technical-writer](https://github.com/Shubhamsaboo/awesome-llm-apps/tree/main/awesome_agent_skills/technical-writer?utm_source=chatgpt.com) ⭐：标准化 README 生成；API 接口文档编写；技术教程、用户指南创作；中英文文档翻译

- [doc-coauthoring](https://github.com/anthropics/skills/blob/main/skills/doc-coauthoring) ⭐：技术提案（RFC）撰写；系统设计文档创作；团队规范文档打磨；支持多轮迭代优化

- [wshobson/agents](https://github.com/wshobson/agents)：一个全面的系统，结合了 185 个专业 AI 代理 、16 个多代理工作流程编排器、**153** 个代理技能和 **100 个命令，** 组织成 **80 个专注于** Claude 代码的单一用途插件。

    - `/architecture-patterns`：后端开发架构设计专家 - 根据业务场景推荐合适的架构模式，讲解各类架构的优缺点与适用场景，给出架构设计的最佳实践

    - `/architecture-decision-records`：建构记录专家 - 按照技术决策文档的最佳实践编写并维护架构决策记录（ADR）。用于记录重大技术决策、回顾过往架构选择或建立决策流程时。

    - `/api-design-principles`：接口规范专家 - 强制按照 REST 规范设计、错误码设计、版本控制、安全机制（OAuth2、JWT）设计接口

    - `/python-design-patterns`：工程化思维强化器 - 模块化拆分、单一职责边界清晰、单元测试覆盖

    - `/java-spring-boot`：企业级后端能力

    - `/typescript-advanced-types`：用于构建类型安全、可重用组件和工具的高级 TypeScript 类型系统模式。

    - `/tailwind-design-system`：Tailwind v4 采用 CSS 优先设计系统框架，包含令牌、组件和响应式模式。



### UI/UX 设计

- [frontend-design ⭐](https://github.com/anthropics/skills/tree/main/skills/frontend-design)：生成新代码，生成完整组件代码

- [ui-ux-pro-max-skill ⭐](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)：设计系统+审计

- **impeccable：**[GitHub - pbakaus/impeccable](https://github.com/pbakaus/impeccable)

- [Taste Skill](https://github.com/Leonxlnx/taste-skill)：让 AI 生成的前端界面不再是千篇一律，

    - 三个可调参数

        - `DESIGN_VARIANCE`——布局实验性（1-3: 简洁居中 / 8-10: 非对称现代）

        - `MOTION_INTENSITY`——动效强度（1-3: 简单悬停 / 8-10: 磁性 + 滚动触发）

        - `VISUAL_DENSITY`——视觉密度（1-3: 宽松奢华 / 8-10: 信息密集仪表板）

    - `/taste-skill`：通用前端页面，输出代码

    - `/image-to-code-skill`：先图像参考，再代码实现

    - `/imagegen-frontend-web`、`/imagegen-frontend-mobile`：生成设计参考图，输出图片

    - `/brandkit`：品牌板、字体、色彩方向，输出图片

- [MiniMax-AI/skills/frontend-dev](https://github.com/MiniMax-AI/skills/tree/main/skills/frontend-dev)：API 生成媒体资源（图片、视频、音频、音乐、TTS）、基于 AIDA 框架的说服力文案、生成艺术（p5.js、Three.js、Canvas）。技术栈：React / Next.js、Tailwind CSS。



### 前端开发

- [webapp-testing](https://github.com/anthropics/skills/blob/main/skills/webapp-testing)：Web自动化测试，基于 Playwright 生成 E2E 测试用例，使用 Playwright 与本地网络应用交互和测试的工具包。支持前端功能验证、调试界面行为、截图采集和查看浏览器日志。

- [playwright-best-practices-skill](https://github.com/currents-dev/playwright-best-practices-skill)：涵盖所有测试类型和场景的 Playwright 测试的编写、调试和维护的综合参考指南。适合已经在做 E2E 的团队

- [ChromeDevTools/chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp)：使你的 AI 编码助手能够全面使用 Chrome DevTools 的强大功能，实现可靠的自动化、深入调试和性能分析。

    - 官方安装：`/plugin install chrome-devtools-mcp`

- [audit-website](https://github.com/squirrelscan/skills)：网站体检专家 - 检查 SEO 问题、性能瓶颈、安全漏洞、可访问性缺陷

- [vue-skills](https://github.com/vuejs-ai/skills)

    - `/vue-best-practices`

    - `/vueuse-best-practices`

    - `/vue-pinia-best-practices`

    - `/vue-router-best-practices`

    - `/create-adaptable-composable`

- [antfu/skills](https://github.com/antfu/skills) ⭐

    - 官方文档生成：`vue`、`nuxt`、`pinia`、`vite`、`vitepress`、`vitest`、`unocss`、`pnpm`

    - 来自外部仓库：`slidev`、`tsdown`、`turborepo`、`vueuse-functions`、`vue-best-practices`、`vue-testing-best-practices`、`web-design-guidelines`

- [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills)

    - `/web-design-guidelines`⭐：根据 Vercel 的网页界面指南审核 UI 代码，以确保设计和无障碍合规性。

    - `/vercel-react-best-practices`：React 和 Next.js 性能优化指南，包含 8 个类别的 64 条优先级规则。

- [vercel-labs/next-skills](https://github.com/vercel-labs/next-skills)

    - `/next-best-practices`：Next.js 最佳实践



### 其他

- [claude-md-optimizer](https://github.com/wrsmith108/claude-md-optimizer)：利用渐进披露优化超大容量 CLAUDE.md 文件的 Claude Code 技能。分析内容层级，检测加密约束，创建子文档，并重写主文件——零信息丢失。

- [architecture-diagram-generator](https://github.com/Cocoon-AI/architecture-diagram-generator)：生成漂亮的暗色主题系统架构图，作为独立的 HTML/SVG 文件。

- **planning-with-files：**[GitHub - OthmanAdi/planning-with-files](https://github.com/OthmanAdi/planning-with-files)

    与**Everything Claude Code 冲突**

- **chart-visualization-skills：**[GitHub - antvis/chart-visualization-skills](https://github.com/antvis/chart-visualization-skills)

    - AntV 图标库 Skills

- [research](https://github.com/tavily-ai/skills/tree/main/skills/tavily-research)：research 技能能自动执行全方位信息搜集与深度分析。配合 Tavily 的智能搜索能力，它可以从海量网络资源中筛选权威信源、交叉验证信息并生成带引用的高质量研究报告

- 

- [knowledge-site-creator](https://github.com/joeseesun/qiaomu-knowledge-site-creator)：一句话生成任何领域的知识型网站。



- [mcp-builder](https://github.com/anthropics/skills/tree/main/skills/mcp-builder)**：**构建高质量 MCP 服务器的全面指南，将 LLM 连接到外部服务。

- claude-skills：66 项全栈开发者的专业技能。[GitHub - Jeffallan/claude-skills](https://github.com/jeffallan/claude-skills)



### 其他开发工作流

- [compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin)：Superpowers 升级版

- [get-shit-done](https://github.com/gsd-build/get-shit-done)：一个轻量级且强大的元提示、上下文工程和规格驱动开发系统，解决上下文腐败问题，把大任务拆成多个小计划，每个计划都在全新的上下文窗口中执行。

    - `/gsd-map-codebase`：扫描并索引你当前的代码库状态

    - `/gsd-new-project`：以上下文创建新的 GSD 规划结构

- [Everything Claude Code](https://github.com/affaan-m/everything-claude-code)

    - 一个面向 Claude Code 的“全家桶”配置仓库/插件：把常用的 agents、skills、slash commands、rules、hooks、以及 MCP server 配置示例集中在一起，提供一套可直接复用的工程化工作流。

    - 使用：`/planner`

- [gstack](https://github.com/garrytan/gstack)：一套 AI 工程工作流工具集，把 Claude Code 变成一个有完整流程的工程团队。

    - `/office-hours`：强制回答六个问题，把产品方向和需求想清楚。

    - `/plan-ceo-review`：从商业和产品视角砍掉无效需求，锁定最小可行版本

    - `/plan-eng-review`，锁死技术架构、数据流、接口契约，输出完整技术方案

    - `/plan-design-review`，完成UI/UX设计评审，锁定产品交互细节

    - `/autoplan`：一键审查流水

- [Spec Kit](https://github.com/github/spec-kit)：Spec Kit本质上是一个CLI工具，它通过模板和脚手架来规范AI的行为。Spec-Driven Development：规格驱动开发，简称SDD

    - `/speckit.constitution`：立规矩，告诉AI这个项目的代码风格、技术栈限制、必须遵守的规范。

    - `/speckit.specify`：定需求，发散成详细的功能列表、用户故事和业务逻辑。

    - `/speckit.clarify`：补漏洞

    - `/speckit.plan`：写方案

    - `/speckit.tasks`：拆任务

    - `/speckit.implement`：写代码

- [OpenSpec](https://github.com/Fission-AI/OpenSpec)：一个轻量化的规范驱动框架，Spec-Driven Development：规格驱动开发，简称SDD

    - OpenSpec 提供两种工作流：[OpenSpec 深度解析：最佳实践四步法](https://mp.weixin.qq.com/s/1yroDQp_tyNI0Fed1sgYsQ)

        - Core Profile（快速路径）：`/opsx:propose`

        - Extended Profile（细粒度控制）

            ```Shell
            /opsx:explore    ← 探索想法
            /opsx:new        ← 创建变更脚手架
            /opsx:continue   ← 继续未完成的变更
            /opsx:ff         ← 快速创建所有规划文档
            /opsx:apply      ← 实现任务
            /opsx:verify     ← 验证实现是否符合规范
            /opsx:sync       ← 同步规范与代码状态
            /opsx:archive    ← 归档
            ```

    - OpenSpec 的核心是**双文件夹模型**：`specs/`- 当前系统的事实来源（规范文件）；`changes/`-每次变更的完整提案

    - 注意：规范会在你构建过程中生成，目前不支持在现有代码库上使用

- [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills)：把资深工程师的工作习惯拆成可组合的 Skill，约束 AI 每一步按规范干活

    - 7个斜杠命令对应7个开发阶段（DEFINE → PLAN → BUILD → VERIFY → REVIEW → SHIP），每个阶段激活不同的技能集

    - 内置了一张"反合理化表"——AI想偷懒跳过步骤？这张表列出了常见借口和反驳理由

    - 每个技能都要求可验证的证据，比如测试通过、构建输出，不接受"应该没问题"这种模糊说法





## MCP 服务器

- Context7 ⭐：用于实时检索文档。可将特定版本的文档与代码示例从源码仓库提取至大语言模型（LLM）的上下文中。

- [firecrawl-mcp-server](https://github.com/firecrawl/firecrawl-mcp-server)：Firecrawl MCP 服务器——为 Cursor、Claude 及其他大型语言模型客户端添加了强大的网页爬取和搜索功能。让AI能读懂任何网页

- [github-mcp-server](https://github.com/github/github-mcp-server)：GitHub 官方 MCP 服务器，用自然语言管理你的代码仓库

- [supabase-mcp](https://github.com/supabase-community/supabase-mcp)：将 Supabase 连接到您的 AI 助手，用中文问数据库问题

- [publora/mcp-server](https://github.com/publora/mcp-server)——通过 Claude、Cursor 等 AI 助手控制社交媒体日程。一条指令，多平台发布



## 记忆/图谱/压缩

- [claude-mem](https://github.com/thedotmack/claude-mem)**：**一个 Claude Code 插件，能自动捕捉 Claude 在你编程会话中的所有操作，用 AI 压缩，并在未来的会话中注入相关上下文

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

- [Understand-Anything](https://github.com/Lum1104/Understand-Anything)：依托多智能体架构与知识图谱技术，将复杂代码库转化为可探索、可搜索、可问答的交互式图谱

    - Token 消耗较高、超大图谱卡顿

> Token 压缩

- [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman)：AI输出砍半，Token费用直接降75%

    - 4种强度模式。Lite轻度压缩，Full标准模式，Ultra极限压缩，还有一个文言文模式，用中文古文输出。

- [rtk](https://github.com/rtk-ai/rtk)：一款高性能 CLI 代理工具，通过智能过滤、压缩命令输出，可为 LLM 节省 60-90% 的 token 消耗。



## 周边工具

> 状态栏插件

- [ccstatusline ⭐](https://github.com/sirmalloc/ccstatusline)：一个高度可定制的 Claude 代码状态行格式化器 ，显示*模型信息、git 分支、令牌使用情况及其他指标，显示终端中的*数据

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

> Skills 管理工具

- [skills-manager](https://github.com/xingkongliang/skills-manager)

- [skills-hub: A cross-platform desktop app to manage Agent Skills in one place and sync them to multiple AI coding tools’ global skills directories — “Install once, sync everywhere”.](https://github.com/qufei1993/skills-hub)

- [https://github.com/iamzhihuix/skills-manage](https://github.com/iamzhihuix/skills-manage)

- [GitHub - ryderme/skill-manager](https://github.com/ryderme/skill-manager)

- [GitHub - legeling/PromptHub: 一款开源、纯本地 Prompt ，Skill 管理工具，帮助你高效管理、版本控制和复用 Prompt，并一键分发skill | An open-source, local-first AI prompt management tool that helps you efficiently manage, version-control, and reuse your prompts](https://github.com/legeling/PromptHub)

- [GitHub - iflytek/skillhub: Self-hosted, open-source agent skill registry for enterprises. Publish & version skill packages, govern with RBAC and audit logs, deploy   on-premise with Docker or Kubernetes.](https://github.com/iflytek/skillhub)

> 设计系统

- [GitHub - OpenCoworkAI/open-codesign: Open-source Claude Design alternative. One-click import your Claude Code / Codex API key. Prompt → prototype / slides / PDF. Multi-model (Claude, GPT, Gemini, Kimi, GLM, Ollama). BYOK, local-first, MIT.](https://github.com/OpenCoworkAI/open-codesign)



## 龙虾技能

- [Multi Search Engine](https://clawhub.ai/gpyangyoujun/multi-search-engine)：17 个引擎：国内 8 个、国际 9 个，支持高级搜索语法、时间过滤、站内搜索、隐私引擎切换，还有 WolframAlpha 知识查询。

- [steipete/summarize](https://github.com/steipete/summarize)： 网页、文件、YouTube 视频和播客迅速总结提炼核心观点

- [eze-is/web-access](https://github.com/eze-is/web-access)：[Web Access：一个Skill，拉满Agent联网和浏览器能力](https://mp.weixin.qq.com/s/rps5YVB6TchT9npAaIWKCw)

- [agent-browser](https://github.com/vercel-labs/agent-browser) ⭐：Vercel 出品的浏览器自动化 Skill，让 AI Agent 能操作浏览器。比如可以自动填表单、点击按钮、截图、抓取动态渲染的内容等，非常适合做端到端测试、自动化爬虫、网页监控等场景。

- [browser-use](https://github.com/browser-use/browser-use)：让 AI Agent 能访问和操作网站的工具（不仅是 Skill，也可以独立使用），功能强大，可以用来做自动化测试、数据抓取、网页操作等。

- [browserbase/skills](https://github.com/browserbase/skills)：操控真实浏览器，像人一样打开网页、点击按钮、填写表单、截图、读取页面内容。

    - `/autobrowse`：自改进的浏览技能

    - `/ui-test`：对抗性 UI 测试

- [self-improving-agent](https://clawhub.ai/pskoett/self-improving-agent)：一套分层记忆系统

- [Self-Improving](https://clawhub.ai/ivangdavila/self-improving)：一套分层记忆系统

- [humanizer-zh](https://clawhub.ai/liuxy951129-cpu/humanizer-zh)：去除文本中的 AI 生成痕迹。适用于编辑或审阅文本，使其听起来更自然、更像人类书写。



## 个人 skill

### 开发类

- [Github](https://clawhub.ai/steipete/github)：用 `gh` CLI 让 AI 直接操作 GitHub——查 PR、看 CI 跑了几条、看哪个 Step 挂了、用 API 拿到你想要的数据、翻 issue 列表





### 内容生成处理类

- [last30days-skill](httpe://github.com/mvanhorn/last30days-skill)：一个 **多源实时研究引擎，**通过 **强制 30 天时效** + **多源聚合，**一句话，帮你抓取海外10+社区平台（Reddit、X、YouTube、Hacker News、TikTok、Polymarket等）的真实评论

- [https://github.com/Jesseovo/last30days-skill-cn](https://github.com/Jesseovo/last30days-skill-cn)：一个 AI Agent 技能（Skill），能够自动搜索中国互联网 8 大主流平台最近 30 天的内容，综合分析后生成有据可查的研究报告。

- [anthropics/skills](https://github.com/anthropics/skills)：

    - `/web-artifacts-builder`：把资料、数据、结论变成一个能交互展示的网页

    - `/canvas-design`：绘制 架构图、流程图、简单的报告封面图 必备，能直接导出 PNG 和PDF 格式。



- [baoyu-skills](https://github.com/jimliu/baoyu-skills)：内容生成和发布技能、图像生成

- [frontend-slides](https://github.com/zarazhangrui/frontend-slides)：利用 Claude 的前端技能，在网上制作精美的 HTML 幻灯片

    - 正经的演示文稿，适合产品汇报、技术分享、对外演示。

- [ppt-master](https://github.com/hugohe3/ppt-master)：AI 从任何文档生成可原生编辑的 PPTX

- [guizang-ppt-skill](https://github.com/op7418/guizang-ppt-skill)：一个网页 PPT 技能,用于生成单文件 HTML 横向翻页 PPT,视觉基调是"电子杂志 × 电子墨水"。

    - **✅** 合适:线下分享 / 行业内部讲话 / 私享会 / AI 产品发布 / demo day / 带强烈个人风格的演讲

    - **❌** 不合适:大段表格数据 / 培训课件(信息密度不够)/ 需要多人协作编辑(静态 HTML)

- [Kami](https://github.com/tw93/Kami)：让 AI 生成的文档，有值得一看的排版！

    - 支持六种核心文档类型：一页纸报告、研究报告、正式信函、个人展示、简历、演示幻灯片

- [huashu-skills](https://github.com/alchaincyf/huashu-skills)：AI审校、选题生成、视频大纲、素材搜索等 11 个实用技能

- [helloianneo/ian-handdrawn-ppt](https://github.com/helloianneo/ian-handdrawn-ppt)：把你的文章、课程笔记、提纲，生成统一风格的手绘技术解释图。

- [html-ppt-skill](https://github.com/lewislulu/html-ppt-skill)：HTML PPT Studio — AgentSkill with 24 themes, 31 layouts, 20+ animations for building professional HTML presentations

- [GitHub - 1weiho/open-slide: A slide framework built for agents.](https://github.com/1weiho/open-slide)：一个专为 AI 智能体（如 Claude Code、Cursor）设计的幻灯片框架。只需自然语言描述，就可以一键生成精美 PPT

- [yizhiyanhua-ai/fireworks-tech-graph](https://github.com/yizhiyanhua-ai/fireworks-tech-graph)：生成 SVG，导出成高清 PNG，生成专业架构图

- [Cocoon-AI/architecture-diagram-generator](https://github.com/Cocoon-AI/architecture-diagram-generator)：根据文字描述，自动生成专业架构图，输出 HTML 文件。

- [awesome-copilot](https://github.com/github/awesome-copilot)

    - `/excalidraw-diagram-generator`：生成专业架构图，能手动改，生成的是 .excalidraw JSON 文件。



