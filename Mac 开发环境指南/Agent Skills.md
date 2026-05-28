## Skills 管理工具

- [skills-manager](https://github.com/xingkongliang/skills-manager) ⭐：轻量级 Skills 管理工具。
- [skills-hub](https://github.com/qufei1993/skills-hub)：跨平台桌面应用，把 Agent Skills 同步到多个 AI 编码工具的全局目录。
- [skills-manage](https://github.com/iamzhihuix/skills-manage)：本地 Skills 管理工具。
- [skill-manager](https://github.com/ryderme/skill-manager)：面向个人技能清单的管理工具。
- [PromptHub](https://github.com/legeling/PromptHub)：本地优先的 Prompt / Skill 管理、版本控制和分发工具。
- [skillhub](https://github.com/iflytek/skillhub)：面向企业的自托管开源 agent skill registry，支持版本、RBAC 和审计日志。

## Skill 市场

- [Claude Plugins](https://claude.com/plugins) ⭐：Anthropic 官方插件市场，集中收录可一键安装到 Claude 的 tools、skills 和 integrations。
- [skills.sh](https://skills.sh/) ⭐：Vercel 推动的开放 Agent Skills 生态目录，适合按仓库、热度和安装量发现通用技能。
- [SkillHub](https://skillhub.cn/) ⭐：面向中文用户优化的 Skills 社区，支持国内加速安装、热榜浏览和中文场景技能发现。
- [ClawHub](https://clawhub.ai/)：社区驱动的 Skills 与 Plugins 市场，强调搜索、安装和创作者发布，适合补充找长尾工具。
- [SkillsLLM](https://skillsllm.com/)：面向 Claude Code、Codex CLI、ChatGPT 等工具的 AI Skills 聚合站，带分类、趋势和项目详情页。
- [SkillsMP](https://skillsmp.com/)：基于开放 `SKILL.md` 生态的技能搜索市场，支持按职业、类别和语义搜索大规模技能库。

## 技能入口与发现

- [find-skills](https://skills.sh/vercel-labs/skills/find-skills) ⭐：一个技能搜索引擎，帮助用户快速定位并调用当前场景下最合适的工具。
- [skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator) ⭐：允许用户将个人经验、特定业务逻辑封装成可复用的技能。

## 开发工作流与工程规范

### 端到端开发工作流

- [Superpowers](https://github.com/obra/superpowers) ⭐：TDD 驱动的多 Agent 开发工作流，通过强制流程约束 AI 的执行过程。
  - `/superpowers:brainstorm`：把模糊想法变成具体设计方案，先理解项目现状、需求、约束，再提出方案并生成设计文档与实施计划。
  - 子智能体驱动开发：可以创建专门化子智能体执行特定任务，主智能体负责协调。
  - Git 工作树隔离：每个任务在独立 Git worktree 里开发，互不干扰。
  - 系统化调试：不是靠“试试看”，而是有结构的问题诊断和解决流程。
- [compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin)：可视为 Superpowers 的增强版。
- [mattpocock/skills](https://github.com/mattpocock/skills) ⭐：一套前端开发工作流技能，用来解决人和 Agent 之间沟通对不齐的问题。
  - 开发类：
    - `/setup-matt-pocock-skills` ⭐：初始化并生成 `CONTEXT.md` 和 ADR 文档。
    - `/grill-with-docs` ⭐：逐个分支拷问方案，对标项目术语表和架构决策记录，并实时更新文档。
    - `/tdd`：测试驱动开发，严格执行 red-green-refactor 循环。
    - `/improve-codebase-architecture` ⭐：定期扫描代码库，寻找可以提升接口清晰度与模块内聚性的重构机会。
    - `/diagnose` ⭐：复杂问题定位流程，要求先复现、再二分、再假设、再验证。
    - `/prototype` ⭐：快速验证想法、对比方案、写出可运行的实验性代码。
  - 工具与工程化类：
    - `/zoom-out`：强制 Agent 站在整个系统视角解释代码。
    - `/git-guardrails-claude-code`：拦截 `push --force`、`reset --hard`、`clean -fd` 等危险命令。
  - 规划与设计类：
    - `/grill-me`：反复追问设计漏洞，直到关键分支都覆盖到。
    - `/to-prd`：把对话整理成 PRD，并在输出前先问清受影响模块。
    - `/to-issues`：把口头描述转成结构化 GitHub Issue。
    - `/triage`：按优先级给出 issue 分类与处理顺序建议。
    - `/handoff`：生成 Agent 交接文档，把上下文留住。
- [get-shit-done](https://github.com/gsd-build/get-shit-done)：规格驱动开发与上下文工程工作流，把大任务拆成多个小计划分上下文执行。
  - `/gsd-map-codebase`：扫描并索引当前代码库状态。
  - `/gsd-new-project`：基于当前上下文创建新的 GSD 规划结构。
- [gstack](https://github.com/garrytan/gstack)：一套 AI 工程工作流工具集，把 Claude Code 变成更完整的工程团队。
  - `/office-hours`：先把产品方向和需求边界问清楚。
  - `/plan-ceo-review`：从商业和产品视角砍掉无效需求，锁定最小可行版本。
  - `/plan-eng-review`：锁定技术架构、数据流和接口契约，输出完整技术方案。
  - `/plan-design-review`：完成 UI/UX 设计评审，锁定交互细节。
  - `/autoplan`：一键审查流水。
- [claude-code-harness](https://github.com/Chachamaru127/claude-code-harness)：在 Claude Code 外套一个有纪律的交付循环，把”计划→实现→审查→发布”变成固定路径。
  - 核心 skill：`/harness-plan`（起草 spec.md 和 Plans.md）、`/harness-work`（TDD 执行已批准任务）、`/harness-review`（实现与审查分离，独立审查）、`/harness-release`（打包发布证据，preflight 检查）
  - `harness.toml`：项目级安全边界和行为策略配置文件，定义权限控制、网络出口过滤、文件系统读取限制和 Worker 自检规则。
  - 适合：觉得 Claude Code 写代码快但质量不稳定的 solo 开发者，用流程约束替代靠运气交付。
- [ECC (Elite Coding Companion)](https://github.com/affaan-m/ECC) ⭐：面向 Claude Code 的”全家桶”配置仓库，集中整理 agents、skills、slash commands、rules、hooks 和 MCP server 配置示例。
  - 本能系统：预配置编码反应模式，自动触发 SQL 注入检测等安全检查。
  - 记忆分层：工作记忆 → 场景记忆 → 长期记忆，跨会话保留项目知识。
  - 安全沙箱：文件/命令/网络操作单独配置允许/拒绝/询问策略。
  - 性能仪表板：记录 Function 调用耗时和 Token 消耗热力图。
  - 使用：`/planner`

### Spec / Plan 驱动开发

- [planning-with-files](https://github.com/OthmanAdi/planning-with-files)：基于文件落盘的规划技能。
- [Spec Kit](https://github.com/github/spec-kit)：用模板和脚手架约束 AI 行为的 Spec-Driven Development 工具。
  - `/speckit.constitution`：定义代码风格、技术栈限制和必须遵守的规范。
  - `/speckit.specify`：把需求发散为功能列表、用户故事和业务逻辑。
  - `/speckit.clarify`：补漏洞。
  - `/speckit.plan`：写方案。
  - `/speckit.tasks`：拆任务。
  - `/speckit.implement`：写代码。
- [OpenSpec](https://github.com/Fission-AI/OpenSpec)：一个轻量化的规格驱动框架。
  - Core Profile（快速路径）：`/opsx:propose`
  - Extended Profile（细粒度控制）：
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
  - 核心是双文件夹模型：`specs/` 作为当前系统事实来源，`changes/` 记录每次变更的完整提案。
  - 注意：规范在构建过程中生成，目前不适合直接套在已有代码库上。
- [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills)：把资深工程师的工作习惯拆成可组合技能，要求 AI 每一步按规范执行。
  - 7 个斜杠命令对应 7 个开发阶段（DEFINE → PLAN → BUILD → VERIFY → REVIEW → SHIP）。
  - 内置“反合理化表”，防止 AI 以各种借口跳步。
  - 每个技能都要求可验证证据，比如测试通过或构建输出。

### 工程规范与架构

- [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) ⭐：强调编码前思考、简洁优先、精准修改、目标驱动执行。
- [wshobson/agents](https://github.com/wshobson/agents)：一个大型 Agent / Skills 集合，适合补齐架构、接口和工程化规范。
  - `/architecture-patterns`：根据业务场景推荐合适架构模式，并说明优缺点与适用场景。
  - `/architecture-decision-records`：按最佳实践编写和维护 ADR。
  - `/api-design-principles`：按 REST、错误码、版本控制、安全机制等规范设计接口。
  - `/python-design-patterns`：强调模块化拆分、职责边界和单元测试覆盖。
  - `/java-spring-boot`：企业级后端能力强化。
  - `/typescript-advanced-types`：构建类型安全、可复用组件和工具的高级 TypeScript 类型模式。
  - `/tailwind-design-system`：Tailwind v4 设计系统实践。
- [mcp-builder](https://github.com/anthropics/skills/tree/main/skills/mcp-builder)：构建高质量 MCP 服务器的全面指南，适合做外部服务接入。
- [claude-skills](https://github.com/jeffallan/claude-skills)：面向全栈开发者的技能合集。

## 前端与 Web 开发

### UI / UX 与界面生成

- [frontend-design](https://github.com/anthropics/skills/tree/main/skills/frontend-design) ⭐：生成完整前端组件与页面代码。
- [ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) ⭐：偏设计系统与界面审计的 UI/UX 技能。
- [impeccable](https://github.com/pbakaus/impeccable)：偏高质量界面生成与视觉打磨的前端技能。
- [garden-skills](https://github.com/ConardLi/garden-skills) ⭐：一套偏设计与内容生产方向的 Agent Skills 集合。
  - `/web-design-engineer`：按设计流程生成更有风格区分度的官网、活动页和展示页。
  - `/web-video-presentation`：把文章、稿子或提纲转成 16:9 网页演示。
  - `/gpt-image-2`：结构化图像提示词生成，适合海报、UI 样机、技术插图等素材场景。
  - `/kb-retriever`：本地知识库检索，按需定位资料，减少把大量文档直接塞进上下文。
- [Taste Skill](https://github.com/Leonxlnx/taste-skill)：让 AI 生成的前端界面不再千篇一律，适合需要风格控制的页面生成场景。
  - 三个可调参数：
    - `DESIGN_VARIANCE`：布局实验性。
    - `MOTION_INTENSITY`：动效强度。
    - `VISUAL_DENSITY`：视觉密度。
  - `/taste-skill`：通用前端页面生成。
  - `/image-to-code-skill`：先图像参考，再代码实现。
  - `/imagegen-frontend-web`、`/imagegen-frontend-mobile`：生成设计参考图。
  - `/brandkit`：生成品牌板、字体和色彩方向图。
- [frontend-dev](https://github.com/MiniMax-AI/skills/tree/main/skills/frontend-dev)：面向 React / Next.js 与 Tailwind 的前端生成技能，覆盖媒体资源、说服力文案和生成艺术场景。

### 前端工程与框架实践

- [vue-skills](https://github.com/vuejs-ai/skills)：Vue 生态技能合集。
  - `/vue-best-practices`
  - `/vueuse-best-practices`
  - `/vue-pinia-best-practices`
  - `/vue-router-best-practices`
  - `/create-adaptable-composable`
- [antfu/skills](https://github.com/antfu/skills) ⭐：Anthony Fu 的前端与工程化技能集合。
  - 官方文档生成：`vue`、`nuxt`、`pinia`、`vite`、`vitepress`、`vitest`、`unocss`、`pnpm`
  - 来自外部仓库：`slidev`、`tsdown`、`turborepo`、`vueuse-functions`、`vue-best-practices`、`vue-testing-best-practices`、`web-design-guidelines`
- [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills)：Vercel 的 Agent Skills 集合。
  - `/web-design-guidelines` ⭐：根据 Vercel 的网页界面指南审核 UI 代码，确保设计和无障碍合规。
  - `/vercel-react-best-practices`：React 和 Next.js 性能优化指南。
- [vercel-labs/next-skills](https://github.com/vercel-labs/next-skills)：Next.js 专项技能集合。
  - `/next-best-practices`：Next.js 最佳实践。

### 测试、调试与网站体检

- [webapp-testing](https://github.com/anthropics/skills/blob/main/skills/webapp-testing)：基于 Playwright 的 Web 自动化测试技能，适合 E2E 测试、界面调试、截图采集和浏览器日志查看。
- [playwright-best-practices-skill](https://github.com/currents-dev/playwright-best-practices-skill)：覆盖 Playwright 测试编写、调试和维护的综合实践指南。
- [chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp)：让 AI 编码助手接入 Chrome DevTools 的自动化、调试和性能分析能力。
  - 官方安装：`/plugin install chrome-devtools-mcp`
- [audit-website](https://github.com/squirrelscan/skills)：网站体检技能，检查 SEO、性能、安全和可访问性问题。

## 搜索、浏览与外部操作

### 搜索与联网

- [Multi Search Engine](https://clawhub.ai/gpyangyoujun/multi-search-engine)：17 个搜索引擎聚合，支持高级搜索语法、时间过滤、站内搜索与知识查询。
- [summarize](https://github.com/steipete/summarize)：快速总结网页、文件、YouTube 视频和播客内容。
- [web-access](https://github.com/eze-is/web-access)：一个强化 Agent 联网与浏览器访问能力的 Skill。

### 浏览器自动化

- [agent-browser](https://github.com/vercel-labs/agent-browser) ⭐：Vercel 出品的浏览器自动化 Skill，适合表单填写、页面点击、截图、动态内容抓取等场景。
- [browser-use](https://github.com/browser-use/browser-use)：让 AI Agent 能访问和操作网站的工具，既可作为 Skill 使用，也能独立运行。
- [browserbase/skills](https://github.com/browserbase/skills)：操控真实浏览器的 Skills 集合。
  - `/autobrowse`：自改进浏览技能。
  - `/ui-test`：对抗性 UI 测试。

### 外部平台操作

- [github](https://clawhub.ai/steipete/github)：用 `gh` CLI 让 AI 直接操作 GitHub，包括查 PR、看 CI、查失败步骤和检索 issue。

## 研究、文档与知识表达

### 文档维护与知识沉淀

- [claude-md-management](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/claude-md-management) ⭐：Claude 官方技能，用于扫描、评估并改进项目 `CLAUDE.md`。
  - `/claude-md-improver`：检查文档质量，补齐不准确或不可执行的说明。
  - `/claude-code-setup`：分析代码库并推荐定制的 Claude 代码自动化 - hooks, skills, MCP 等
- [claude-md-optimizer](https://github.com/wrsmith108/claude-md-optimizer)：利用渐进披露优化超大体量 `CLAUDE.md` 文件的 Claude Code 技能。
- [technical-writer](https://github.com/Shubhamsaboo/awesome-llm-apps/tree/main/awesome_agent_skills/technical-writer?utm_source=chatgpt.com) ⭐：标准化 README 生成、API 文档编写、技术教程与用户指南创作。
- [doc-coauthoring](https://github.com/anthropics/skills/blob/main/skills/doc-coauthoring) ⭐：面向技术提案、系统设计文档和团队规范文档的协作写作技能。
- [KKKKhazix/khazix-skills](https://github.com/KKKKhazix/khazix-skills) ⭐：既适合文档收尾整理，也适合系统性研究。
  - [neat-freak](https://github.com/KKKKhazix/khazix-skills#-neat-freak%E6%B4%81%E7%99%96) ⭐：收尾时自动把本次改动与项目文档、`CLAUDE.md`、Agent 记忆对齐。
  - [hv-analysis（横纵分析法）](https://github.com/KKKKhazix/khazix-skills#-hv-analysis%E6%A8%AA%E7%BA%B5%E5%88%86%E6%9E%90%E6%B3%95) ⭐：系统研究一个产品、公司、概念或技术，输出长篇研究报告。

### 深度研究与内容生产

- [research](https://github.com/tavily-ai/skills/tree/main/skills/tavily-research)：结合 Tavily 搜索做资料搜集、交叉验证与引用式研究报告生成。
- [last30days-skill](https://github.com/mvanhorn/last30days-skill)：一个多源实时研究引擎，通过“最近 30 天 + 多源聚合”抓取海外社区的真实评论与讨论。
- [last30days-skill-cn](https://github.com/Jesseovo/last30days-skill-cn)：一个面向中文互联网的最近 30 天内容研究技能。
- [baoyu-skills](https://github.com/jimliu/baoyu-skills)：内容生成、发布和图像生成技能合集。
- [huashu-skills](https://github.com/alchaincyf/huashu-skills) ⭐：花叔的 Claude Code Skills 合集，21 个经过实战验证的内容创作技能，覆盖从选题到发布的完整工作流。
  - `/huashu-slides`：AI 演示文稿，把文章、提纲或主题快速转成可演示的幻灯片。
  - `/huashu-design`：设计哲学顾问，做高保真原型、交互 demo、动画、设计变体，可导出 MP4 / GIF。
  - `/huashu-proofreading`：三遍审校降 AI 味，去除文本里的 AI 生成痕迹让措辞更自然。
- [humanizer-zh](https://clawhub.ai/liuxy951129-cpu/humanizer-zh)：去除文本里的 AI 生成痕迹，让措辞更自然。
- [knowledge-site-creator](https://github.com/joeseesun/qiaomu-knowledge-site-creator)：一句话生成任意领域的知识型网站。

### 演示、图表与可视化表达

### 综合展示技能

- [anthropics/skills](https://github.com/anthropics/skills)：Anthropic 官方技能仓库。
  - `/web-artifacts-builder`：把资料、数据和结论变成可交互展示的网页。
  - `/canvas-design`：绘制架构图、流程图和简单报告封面图，并导出 PNG、PDF。

### 演示文稿与展示网页

- [frontend-slides](https://github.com/zarazhangrui/frontend-slides)：利用 Claude 的前端能力生成精美 HTML 幻灯片。
  - 适合产品汇报、技术分享和对外演示。
- [ppt-master](https://github.com/hugohe3/ppt-master)：AI 从任意文档生成可原生编辑的 PPTX。
- [guizang-ppt-skill](https://github.com/op7418/guizang-ppt-skill)：生成单文件 HTML 横向翻页 PPT，视觉风格偏”电子杂志 × 电子墨水”。
  - 合适：线下分享、行业内部讲话、私享会、AI 产品发布、demo day。
  - 不合适：大段表格数据、培训课件、多人协作编辑。
- [Kami](https://github.com/tw93/Kami)：让 AI 生成的文档具备更完整的排版表达。
  - 支持一页纸报告、研究报告、正式信函、个人展示、简历、演示幻灯片等文档类型。
- [html-ppt-skill](https://github.com/lewislulu/html-ppt-skill)：HTML PPT Studio，提供多主题、多布局和动画能力。
- [open-slide](https://github.com/1weiho/open-slide)：专为 AI 智能体设计的幻灯片框架，可通过自然语言一键生成 PPT。
- [html-anything](https://github.com/nexu-io/html-anything)：调用本地 AI 编码 CLI 生成专业级单文件 HTML，75 套模板覆盖小红书卡片、演示文稿、办公文档、数据报告、视频帧脚本等 9 大场景。
  - 工作方式：描述需求 → 选择模板 → AI 流式生成 → 实时预览 → 一键导出。
  - 适合：内容创作者快速生成多平台发布成品、职场人做 PPT 和周报、产品/设计师出落地页原型。

### 架构图与解释图

- [ian-handdrawn-ppt](https://github.com/helloianneo/ian-handdrawn-ppt)：把文章、课程笔记或提纲转成统一风格的手绘技术解释图。
- [fireworks-tech-graph](https://github.com/yizhiyanhua-ai/fireworks-tech-graph)：生成 SVG 并导出高清 PNG，适合专业架构图。
- [architecture-diagram-generator](https://github.com/Cocoon-AI/architecture-diagram-generator)：根据文字描述自动生成专业架构图，输出 HTML 文件。
- [awesome-copilot](https://github.com/github/awesome-copilot)：GitHub Copilot 生态资源集合。
  - `/excalidraw-diagram-generator`：生成可手动修改的专业架构图，输出 `.excalidraw` JSON 文件。

### 图表与数据可视化

- [chart-visualization-skills](https://github.com/antvis/chart-visualization-skills)：AntV 图表生成与可视化技能集合。

## Agent 能力增强

### 自我改进与长期记忆

- [self-improving-agent](https://clawhub.ai/pskoett/self-improving-agent)：一套分层记忆系统。
- [Self-Improving](https://clawhub.ai/ivangdavila/self-improving)：一套分层记忆系统。
