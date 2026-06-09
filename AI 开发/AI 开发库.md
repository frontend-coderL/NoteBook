## Python

### Agent 框架

- [openai-agents-python](https://github.com/openai/openai-agents-python) ⭐：多代理工作流框架，26.7k Star，支持 OpenAI API 及 100+ 其他 LLM。
  - 核心能力：Agent 配置（指令/工具/护栏）、Sandbox Agent 容器化执行、MCP/函数/托管工具、输入输出验证、人机协作、内置追踪调试。
  - 适合：需要多 Agent 协作和复杂工具调用的项目。
- [TradingAgents](https://github.com/TauricResearch/TradingAgents)：TauricResearch 出品的多智能体 LLM 金融交易框架，模拟真实交易公司协作。
  - 角色分工：基本面 / 情绪 / 新闻 / 技术四类分析师 + 看涨 / 看跌研究员结构化辩论 + 交易员 + 风控 / 投资组合经理。
  - 决策闭环：从分析、辩论到下单与风控审批形成完整链路，决策日志持久化、检查点可恢复。
  - 多模型支持：兼容 OpenAI / Anthropic / Google / xAI / DeepSeek / Qwen / GLM / Ollama 等，基于 LangGraph 构建。
  - 注意：官方明确声明“for research only”，不构成投资建议。
- [Forge](https://github.com/antoinezambelli/forge)：Agent 中间件框架，通过工具约束层和自动修复机制让 8B 小模型在复杂任务上成功率从 53% 拉升到 99%，超越 Sonnet。
  - 核心机制：Pydantic 强类型约束（拦截错误参数）→ 自动修复循环（错误变提示，中断变循环，最多 3 次重试）→ 沙箱执行环境（限制危险操作）。
  - 实测数据：Llama 3 8B + Forge 成功率 99% vs Claude Sonnet 92%，单次任务成本 $0.01 vs $0.25（1/25）。
  - 适合：大规模自动化（数据清洗、运维、批量代码生成）降本、不想依赖昂贵 API 的团队、本地部署数据私有化。

### 文档处理

- [markitdown](https://github.com/microsoft/markitdown) ⭐：AutoGen Team 出品，127k Star，PDF/Office/图片/音频等转 Markdown。
  - 支持：PDF、PPT、Word、Excel、图片（OCR）、音频（转录）、HTML、CSV、ZIP、YouTube 等。
  - 特点：保留文档结构（标题/列表/表格/链接），CLI + Python API，插件可扩展。
  - LLM 增强：可传 `llm_client=OpenAI()` + `llm_model="gpt-4o"`，让 GPT-4o Vision 自动解读图片内容；也可选接入 Azure Document Intelligence / Content Understanding 提升解析质量。
  - 按需安装：`pip install 'markitdown[pdf, docx, pptx]'` 按文件类型选装依赖，节省空间。
  - 对比 marker：更轻量专注结构保留，非高保真转换场景。
- [marker](https://github.com/datalab-to/marker)：PDF 转 Markdown/JSON，35.5k Star，基于深度学习模型管道。
  - 支持：PDF、图片、PPTX、DOCX、XLSX、HTML、EPUB，所有语言。
  - 核心能力：OCR + 布局检测 + 表格/公式/代码块格式化，可选 LLM 提升准确度。
  - 对比 markitdown：速度更快（H100 单页 0.18s）、准确率更高（95.67 分）、表格提取更强。
- [markit](https://github.com/Michaelliv/markit)：30+ 格式转 Markdown，1.2k Star，可插拔架构。
  - 支持：PDF、DOCX、PPTX、XLSX、EPUB、图片、音频、URL、CSV、JSON 等。
  - 特点：内置 LLM provider（图片描述/音频转录），CLI + SDK 双模式。
  - 对比 marker/markitdown：格式覆盖最广、插件化最强，但 Star 数较低偏小众。
- [opendataloader-pdf](https://github.com/opendataloader-project/opendataloader-pdf)：PDF 解析器，基准测试排名第一。
  - 支持：Markdown、HTML、带边界框 JSON 输出。
  - 适合：RAG 管道集成，prompt injection 过滤。

### 记忆与上下文

- [Honcho](https://github.com/plastic-labs/honcho)：为 AI Agent 提供用户持久记忆，持续学习用户行为模式与偏好，3.3K Star，3 年持续开发。
  - 四个核心概念：Workspace（应用容器）、Peer（任意实体：用户/Agent/群组）、Session（对话上下文）、Messages（对话内容）。
  - 自然语言查询用户画像：`alice.chat("这个用户最容易接受哪种学习方式？")`，不用写 SQL 或手动维护向量索引。
  - Deriver 后台推理：持续生成用户摘要、构建本地/全局表征、管理离线深度推理任务。
  - Python + TypeScript SDK，托管服务（app.honcho.dev）或自托管（PostgreSQL + pgvector + FastAPI）。
  - 适合：做 AI 产品想提升用户留存、需要 Agent 真正「认识」用户的场景。
- [OpenViking](https://github.com/volcengine/OpenViking)：字节火山引擎开源的 AI Agent 上下文数据库，用文件系统范式统一管理记忆、资源和技能，2.2W Star。
  - 三种上下文：Resource（外部知识）、Memory（8 类自动提取：用户画像/偏好/实体/事件/案例/模式/工具/技能）、Skill（可调用能力）。
  - L0/L1/L2 三层信息模型：L0（\~100 tokens，向量召回）→ L1（\~2k tokens，Rerank 精排）→ L2（原始文件，按需加载），Token 预算系统化。
  - Viking URI 寻址：`viking://resources/docs/auth/`，`ls` / `mkdir` / `tree` / `read` 文件系统风格操作。
  - Session 管理：`session.commit()` 自动压缩对话、提取记忆写入对应目录，Agent 用得越多知识库越丰富。
  - 检索链路：Intent Analysis → L0 向量召回 → 目录递归检索 → L1 Rerank → L2 按需加载 → 轨迹可视化。
  - 附带 [Claude Code 记忆插件](https://github.com/volcengine/OpenViking/blob/main/examples/claude-code-memory-plugin/README.md)示例，但本质是 Agent 开发基础设施。
- [TencentDB Agent Memory](https://github.com/Tencent/TencentDB-Agent-Memory)：腾讯开源的 AI Agent 记忆系统，核心创新是「符号化记忆 + 分层可下钻」。
  - L0-L3 四层渐进式架构：L0（原始对话 JSONL）→ L1（结构化事实原子）→ L2（场景归纳 Markdown）→ L3（用户画像），任意层可下钻到原始数据，白盒可审计。
  - Mermaid Canvas 符号化短期记忆：工具日志卸载到外部文件，提取关系生成 Mermaid 图谱（几百 token），Agent 按 node\_id 随时恢复原文，token 降低 30%-61%。
  - 双路召回：BM25 + Embedding RRF 融合，PersonaMem 评测准确率 47.85% → 76.10%。
  - 部署：SQLite 本地版（零外部依赖）或腾讯云向量数据库版（企业级）。
  - 适合：长任务/运维排错、需保留完整证据的场景；当前仅支持 OpenClaw/Hermes。

## TypeScript

### Agent 框架

- [Pi (earendil-works)](https://github.com/earendil-works/pi) ⭐：AI Agent 全栈工具箱，56.8k Star，统一 OpenAI/Anthropic/Google 等多模型 API，支持 CLI 和 SDK 两种集成方式。
  - `pi-coding-agent`：终端编码 Agent CLI。核心设计哲学是「极简 + 可扩展」：只内置 4 个工具（read / write / edit / bash），其余能力靠 TypeScript 扩展按需加载；会话以 JSONL 树状结构管理（`/tree` `/fork` 任意节点分支，不丢历史路径）。
  - `pi-agent-core`：运行时核心，工具调用与状态管理，可集成到其他应用。
  - `pi-ai`：统一 LLM API，流式输出 + 工具调用定义。
  - `pi-tui`：终端 UI 库，差分渲染。
  - `pi-chat`：Slack 机器人与聊天自动化。
  - 工程化亮点：依赖版本精确锁定 + shrinkwrap 隔离，供应链安全。
  - 适合：团队构建内部 coding assistant，多模型快速切换对比，软件过程自动化。
- [Mastra](https://github.com/mastra-ai/mastra)：Gatsby 团队打造的 TypeScript AI 全栈框架，24K Star，定位覆盖从原型到生产的全流程，解决「原型半天，上线数周」的断层。
  - 双轨编排引擎：Workflows（图引擎）用 `.then()` / `.branch()` / `.parallel()` 显式控制流，适合确定性任务；Agents 让模型自主推理、选择工具、反复迭代直到满足停止条件，适合开放性任务。
  - 生产工具链内置：Evals（模型输出评估系统）、Observability（执行状态跟踪）、HITL（人机交互暂停与恢复，存储持久化执行状态，可无限期暂停后从断点继续）。
  - MCP 服务器：Agent 和工具暴露为标准协议接口，可被任何支持 MCP 的系统调用，无需写胶水代码集成。
  - TypeScript 优先：可整合 React、Next.js、Node.js，集成 Vercel AI SDK UI 和 CopilotKit，目标用户是 Web 应用开发者。

    <br />

