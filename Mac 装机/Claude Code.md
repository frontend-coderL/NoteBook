## 安装

```Shell
curl -fsSL https://claude.ai/install.sh | bash
```

## 基础配置

- 输入 `/config`，选 Output style，切换到 **Explanatory**（解释型）
- `/tui fullscreen`：全屏模式

## 第三方插件

> 记忆插件

- **claude-mem：Claude Code 的长记忆和知识沉淀插件**

```Shell
/plugin marketplace add thedotmack/claude-mem
/plugin install claude-mem
```

- [GitHub - thedotmack/claude-mem](https://github.com/thedotmack/claude-mem)
- MemOS：[GitHub - MemTensor/MemOS](https://github.com/MemTensor/MemOS)
- 面向大型语言模型和代理系统的 AI 内存操作系统（moltbot、clawdbot、openclaw），实现了持久技能内存，支持跨任务技能的重用和演进。

> 状态栏插件

- **claude-hud**：[GitHub - jarrodwatts/claude-hud](https://github.com/jarrodwatts/claude-hud)

```Shell
/plugin marketplace add jarrodwatts/claude-hud
/plugin install claude-hud
/claude-hud:setup
```

- ccstatusline：[GitHub - sirmalloc/ccstatusline](https://github.com/sirmalloc/ccstatusline)

```Shell
npm install -g ccstatusline-zh
ccstatusline-zh setup
```

## Skills

> 官方市场：[Plugins for Claude Code and Cowork | Anthropic](https://claude.com/plugins)

### 必备

- **Superpowers：**[GitHub - obra/superpowers](https://github.com/obra/superpowers)

  安装：`/plugin install superpowers@claude-plugins-official`

  使用：`/superpowers:brainstorm`

- **claude-plugins-official：**[GitHub - anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official)

- `code-simplifier`：自动识别冗余代码、简化复杂逻辑

- `skill-creator`：根据需求自动生成自定义 Skill

- `code-review`：代码审查，提供规范检查和改进建议

- `security-guidance`：安全编码最佳实践

- **Everything Claude Code：**[GitHub - affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code)

- 一个面向 **Claude Code** 的“全家桶”配置仓库/插件：把常用的 **agents**、**skills**、**slash commands**、**rules**、**hooks**、以及 **MCP server 配置示例**集中在一起，提供一套可直接复用的工程化工作流。

- **last30days-skill：[httpe://github.com/mvanhorn/last30days-skill](httpe://github.com/mvanhorn/last30days-skill)**

- 一个 **多源实时研究引擎，**通过 **强制 30 天时效** + **多源聚合**

### UI/UX 设计

- **impeccable：**[GitHub - pbakaus/impeccable](https://github.com/pbakaus/impeccable)
- 设计增强模块：零设计基础，也能做出专业视觉效果
  - **`adapt`**：响应式设计和跨设备适配
  - **`arrange`**：改善布局、间距和视觉节奏
  - **`bolder`**：增强视觉冲击力和个性
  - **`colorize`**：添加战略性色彩
  - **`quieter`**：调低过于激进的视觉效果
- 交互与动效模块：给页面增加灵魂，不花哨但足够高级
  - **`animate`**：添加有意义的动画和微交互**，**页面死板、没有反馈，用户操作没有体感
  - **`delight`**：增加令人愉悦的瞬间和个性**，**页面冷冰冰的，没有品牌感和温度
  - **`overdrive`**：超越常规的技术实现**，**做一些高级的视觉效果
- 审核与优化模块：给你的代码做一次全面体检
  - **`audit`**：技术质量检查（可访问性、性能等）
  - **`polish`**：UX 评估和设计反馈
  - **`polish`**：UI 性能诊断和优化，页面加载慢、动画卡顿、性能差
- 工程化模块：让你的烂代码，变成规范可维护的生产级代码
  - **`distill`**：简化设计，去除不必要的复杂性**，**代码冗余、逻辑混乱，很多没用的代码和样式
  - **`extract`**：提取可复用的组件和设计令牌**，**代码重复率高，改一个样式要改十几个地方，难维护
  - **`harden`**：提高界面韧性和错误处理**，**页面容错性差，网络异常、用户误操作就会出问题
  - **`normalize`**：与设计系统标准对齐**，**代码风格不统一，不符合行业最佳实践
  - **`polish`**：最终质量打磨
- 内容与引导模块：提升页面的转化率和用户体验
  - **`clarify`**：改善 UX 文案和标签**，**按钮文案模糊、引导语不清晰
  - **`typeset`**：改进字体排印和可读性**，**字体选择乱、字号层级不清晰、行高不合适
  - **`onboard`**：设计入职流程和空状态**，**新用户进来不知道怎么用，空页面光秃秃的，体验很差
- 项目管理模块：一次性搞定项目全局配置
  - **`teach-impeccable`**：一次性项目设计上下文设置。第一次使用时，运行一次 `/teach-impeccable` 指令，让 AI 了解你的项目设计上下文（比如品牌色、目标设备、语气等），它会把这些信息存下来，之后的操作都会基于此保持一致性。
- Taste Skill：[GitHub - Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill)
- **frontend-design：**[GitHub - anthropics/skills: Public repository for Agent Skills](https://github.com/anthropics/skills)
- **UI/UX Pro Max Skill：**[GitHub - nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)

```Shell
/plugin marketplace add nextlevelbuilder/ui-ux-pro-max-skill
/plugin install ui-ux-pro-max@ui-ux-pro-max-skill
```

### Vue3生态

- **vue-skills：**[GitHub - vuejs-ai/skills: Agent skills for Vue 3 development](https://github.com/vuejs-ai/skills)

```Shell
/vue-best-practices
/vueuse-best-practices
/pinia-best-practices
/vue-router-best-practices
```

- **antfu/skills：**[GitHub - antfu/skills: Anthony Fu's curated collection of agent skills.](https://github.com/antfu/skills)

  一组精选的[代理技能](https://agentskills.io/home)合集，反映了 [Anthony Fu](https://github.com/antfu) 的偏好、经验和最佳实践

- 官方文档生成：`vue`、`nuxt`、`pinia`、`vite`、`vitepress`、`vitest`、`unocss`、`pnpm`

- 来自外部仓库：`slidev`、`tsdown`、`turborepo`、`vueuse-functions`、`vue-best-practices`、`vue-testing-best-practices`、`web-design-guidelines`

### React生态

- vercel-react-best-practices
- react:components

### TypeScript

- typescript-advanced-types
- typescript-expert
- openapi-to-typescript

### Node

- mcollina/skills

### 其他

- **planning-with-files：**[GitHub - OthmanAdi/planning-with-files](https://github.com/OthmanAdi/planning-with-files)

  与**Everything Claude Code 冲突**

- **chart-visualization-skills：**[GitHub - antvis/chart-visualization-skills](https://github.com/antvis/chart-visualization-skills)

- AntV 图标库 Skills

- **GitNexus：**[GitNexus](https://github.com/abhigyanpatwari/GitNexus)

## 周边工具

### cc-switch

```Shell
# 通过 Homebrew 安装
brew tap farion1231/ccswitch
brew install --cask cc-switch

# 更新
brew upgrade --cask cc-switch
```

手动下载：[Releases · farion1231/cc-switch](https://github.com/farion1231/cc-switch/releases)

### cc-lens

- Claude Code 的实时监控仪表盘
- [github.com](https://github.com/Arindam200/cc-lens)
- `npx cc-lens`

MMX-CLI：[MiniMax 发布 MMX-CLI：为 Agent 设计的全模态命令行工具](https://mp.weixin.qq.com/s?__biz=MzE5MTA3NzcxMQ==&mid=2247488291&idx=1&sn=b92c388430676cf6c50b68d867a56dae&chksm=97f0ecfd4c13c69eb5cc57e381a5a7cfb199bfae1426c301ffe845e4c3ecea5b3eade1aaa6bc&mpshare=1&scene=1&srcid=0418I8S2HjZPBpVDzMiM84JC&sharer_shareinfo=ef6326ddbad91f571ae20108fdbb02d2&sharer_shareinfo_first=ef6326ddbad91f571ae20108fdbb02d2#rd)

Skills Hub：[github.com](https://github.com/qufei1993/skills-hub)

PromptHub：[GitHub -PromptHub: 一款开源、纯本地 Prompt ，Skill 管理工具，帮助你高效管理、版本控制和复用 Prompt，并一键分发skill](https://github.com/legeling/PromptHub)

SkillHub：[GitHub - iflytek/skillhub](https://github.com/iflytek/skillhub)

- SkillHub 是一个自托管平台，为团队提供私有的、受治理的智能体技能共享空间。发布技能包，推送到命名空间，让其他人通过搜索发现或通过 CLI 安装。
