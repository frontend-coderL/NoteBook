## 编程学习与方法论

- [Hello-Agents](https://github.com/datawhalechina/hello-agents)：Datawhale 社区发起的系统性智能体学习教程，48K+ Star，16 章从概念到实战，专注 AI Native Agent（而非流程驱动型）。
  - 五大部分：基础理论（定义 / 范式 / Transformer）→ 构建实践（ReAct / Plan-and-Solve / Reflection / 自研框架）→ 高级进阶（Memory / RAG / Context Engineering / MCP / A2A / Agentic RL）→ 综合案例（旅行助手 / DeepResearch / 赛博小镇）。
  - 自研教学框架 HelloAgents，穿透框架表象理解核心原理。
  - 适合：想从零构建 Agent 的开发者、求职者（面试考点）、研究者（Multi-Agent / MCP / RL）。
- [coding-interview-university](https://github.com/jwasham/coding-interview-university)：完整的计算机科学自学计划，347K Star、82.9K Fork，作者非科班出身通过这份清单进入 Amazon，CC-BY-SA-4.0 协议，有中文翻译。
  - 核心内容：Big-O、数据结构（手写实现）、排序、图算法、动态规划、设计模式、系统设计，配合 MIT / UC Berkeley 精选视频和 LeetCode 刷题。
  - 优势：严格有序的学习路径，GitHub checkbox 追踪进度，Fork 后可个人标记。
  - 注意：专为美国大厂设计，国内面试需额外补充《剑指 Offer》和牛客网真题；英文视频为主；体量大，建议先只看必学部分。
- [build-your-own-x](https://github.com/codecrafters-io/build-your-own-x)：通过从零开始重现你喜欢的技术来掌握编程，费曼学习法的编程实践。
  - 50+ 技术方向：操作系统、Git、内存分配器、编程语言、编译器、数据库、Docker、Web 服务器、3D 渲染器、游戏引擎、神经网络、LLM、RAG 等。
  - 多语言实现：同一技术提供 Python / Go / C++ / JavaScript 等多种语言版本，基于真实可运行代码。
  - 适合：想深度理解技术本质的开发者、面试前补基础、系统补计算机科学全貌。
- [SkillOpt](https://aka.ms/SkillOpt)：把 Agent 的 skill 当作可训练的软件资产来优化，通过真实执行轨迹、结构化编辑和验证门持续改进外部技能文档，适合用来学习 Agent 如何沉淀流程经验，而不只是反复手改提示词。
  - 核心机制：基于 rollout、反思分析和 `add / delete / replace` 编辑迭代 skill 文档，再用 selection split 验证候选改动是否真的有效。
  - 值得关注：它把学习率、验证集、momentum 这类训练纪律迁移到文本技能优化中，适合理解企业 Agent、代码 Agent、办公自动化 Agent 的流程能力如何被系统性训练出来。
- [ai-engineering-from-scratch](https://github.com/rohitg00/ai-engineering-from-scratch) ⭐：从零手写 AI 工程的完整课程体系，20 个 Phase / 435 节课 / 4 种语言（Python/TypeScript/Rust/Julia）/ \~320 小时。
  - 核心教学法：每个算法先纯数学从零实现（无框架），再用生产级库（PyTorch、LangChain）重写，把「黑盒」变「白盒」。
  - 路径：数学基础 → ML 原理 → 深度学习 → CV → NLP → 语音 → Transformer → GenAI → RL → 手写 LLM → LLM 工程化 → 多模态 → Agent → 多 Agent Swarm → 基础设施 → 伦理对齐。
  - 配套网站：[aiengineeringfromscratch.com](https://aiengineeringfromscratch.com)，MIT 协议，可拿来做团队内部培训。
  - 适合：转行 AI 工程的后端/前端开发者、已在做 AI 但基础不牢的在校生、需要在团队内建立 AI 工程能力的技术负责人。
- [awesome-agentic-ai-zh](https://github.com/WenyuChiou/awesome-agentic-ai-zh)：Agentic AI 中文学习地图，8 阶段路线图 + 145+ 精选资源 + 27 个练习目录，三语（简中/繁中/English）。
  - 两条主线：CLI Power User（把 Claude Code / Codex / Cursor 用得更稳）和 Agent Builder（设计能调用工具、管理上下文的智能体）。
  - 覆盖：LLM 基础、Prompt 工程、MCP、Skills、Plugins、Subagents、Browser Use、Computer Use、代码沙箱。
  - 适合：想系统理解 Agentic AI 而非追热点名词、准备内部 Agent 培训材料、从「会用工具」过渡到「会造系统」。
- [advanced-java](https://github.com/doocs/advanced-java)：Java 进阶知识库，82K Star，覆盖高并发、分布式、微服务、中间件等高阶技术，同时汇总大厂面试真题与解析，MIT 协议。
  - 优势：贴合国内大厂技术体系与面试风格，中文讲解通俗易懂，持续迭代更新。
  - 注意：偏知识汇总而非项目实战，适合查漏补缺和面试备考，不适合替代官方文档学习具体框架。
- [dive-into-llms](https://github.com/Lordog/dive-into-llms)：上海交通大学开源的大模型编程实践教程，30K+ Star，完全免费，配套 [GitBook 在线文档](https://sjtullm.gitbook.io/dive-into-llms)。
  - 11 个章节覆盖全链路：LLM 综述、预训练微调与部署、提示学习与思维链、知识编辑、模型水印、越狱攻击、多模态、后门攻击、Agent、RAG 等。
  - 每章配有 Slides + Jupyter Notebook + README，可在 Google Colab 或本地环境直接运行，部分实验 CPU 即可跑通。
  - 适合：想系统入门大模型的学生和工程师、研究模型幻觉与 AI 安全、从零训练微调自己的模型。
- [Agents Towards Production](https://github.com/NirDiamant/agents-towards-production)：Agent 生产级全栈教程合集，约 2 万 Star，22 个独立 Jupyter Notebook 覆盖从编排到部署的完整链路。
  - 编排：LangGraph 状态机工作流、MCP 工具协议、FastAPI 服务化。
  - 记忆：Redis 双记忆架构（短期 + 向量长期）、Mem0 自进化记忆、Cognee 知识图谱。
  - 安全：LlamaFirewall 输入输出护栏、Apex 安全评估。
  - 部署：Docker 容器化、Ollama 本地模型、AWS AgentCore 托管、RunPod 弹性 GPU。
  - 其他：Tavily 实时搜索、LangSmith 链路追踪、Streamlit UI、A2A 多 Agent 通信、Fine-tuning 微调。
  - 注意：非商业许可证（学习免费，商用需许可）；部分教程由赞助商支持（Redis / Mem0 / Tavily 等），质量较高但带产品推广倾向；无 CI/CD 保证，依赖版本可能过时。

## Agent 与工作流

- [12-factor-agents](https://github.com/humanlayer/12-factor-agents)：构建生产级 LLM 应用的 13 条工程化法则，21.9K Star，灵感来自经典 12-Factor App，每条都有 TypeScript / Python 双版本示例，Apache 2.0。
  - 核心思想：代码管流程、LLM 管判断——别反过来。
  - 优势：不绑定任何框架，拿来读的而非装的；覆盖控制流、提示词管理、错误自愈、人工介入等生产痛点。
  - 注意：是方法论而非框架，不能 `pip install`，需自行按法则改造现有代码。
  - 适合：想把 Agent 从 demo 带到生产环境的团队。
- openai-agents-python：OpenAI Agents SDK 的 Python 实现，用于构建多代理工作流，支持 OpenAI Responses API、Chat Completions API 以及 100+ 其他大模型。
- [DeepSeek-TUI](https://github.com/Hmbown/DeepSeek-TUI)：终端原生的 DeepSeek 编程智能体方案。
- [hermes-agent](https://github.com/NousResearch/hermes-agent)：自我提升 AI 代理，强调从经验中积累技能、检索历史对话并持续优化自身行为。
- [nanobot](https://github.com/HKUDS/nanobot)：港大 HKUDS 开源的轻量级 AI Agent 运行时，代码量仅为同类框架的 1%，模块化 + 可插拔架构，2 分钟即可部署。
  - 内置能力：长期记忆、联网搜索、文件处理、代码执行、定时任务、AI 审计沙箱、MCP、多 Provider 路由与回退。
  - 适合：个人效率自动化、小团队协作、快速 PoC 验证、想拥有自己 Agent 技术栈的人。
- [GenericAgent](https://github.com/lsdefine/GenericAgent)：极简自进化 AI Agent，11K Star，核心仅 3K 行代码 / Agent Loop 约百行，不预设技能，靠进化获得能力。
  - 9 个原子工具：代码执行、文件读写、网页控制、键鼠操作、屏幕视觉等，Agent Loop 仅百行。
  - 自我进化：每次解决新任务自动将执行路径固化为 Skill，下次遇到类似任务直接调用，用几周后拥有专属技能树。
  - Token 效率：上下文窗口不到 30K，分层记忆系统让关键信息始终在场，Token 消耗约同类 Agent 的 1/6。
- [Odysseus](https://github.com/pewdiepie-archdaemon/odysseus)：PewDiePie 开源的自托管全能 AI 工作台，4 天 5 万+ Star，FastAPI + ChromaDB + SearXNG，Docker 一键部署，数据全部留在本地。
  - 十大模块：聊天、Agent（支持工具调用 / MCP / Shell / 文件读写）、模型管理、深度研究、模型对比、文档编辑器、记忆系统、邮件、待办事项、日历。
  - 亮点功能：Cookbook 扫描硬件自动推荐可跑的本地模型（含 fit score）；盲测模型对比；Deep Research 自动搜资料生成报告；邮件 AI 自动分类 + 摘要 + 回复草稿。
  - 模型接入：vLLM、llama.cpp、Ollama、OpenRouter、OpenAI，支持 GGUF / FP8 / AWQ 量化。
  - 注意：README 自称 “more jank and fun”，界面精致度不及商业产品，偶有小问题；适合想完全掌控数据和模型、不想被订阅锁死的用户。
- [Cognee](https://github.com/topoteretes/cognee)：开源 AI Agent 记忆开发库，17.5K Star，ECL 管道（Extract → Cognify → Load）将非结构化数据转为可检索、可推理的知识图谱，Apache 2.0。
  - 与传统 RAG 的区别：不只做文本切块向量检索，而是用 LLM 提取实体关系构建图谱，支持多跳推理（A → B → C 间接关联）。
  - 开发体验：`uv pip install cognee` + 一个 OpenAI Key 即可跑通；默认 SQLite + LanceDB + Ladybug 全本地，生产可切换 PostgreSQL + Neo4j。
  - API 设计：`remember()` / `recall()` / `improve()` 三个动词覆盖记忆全生命周期；支持 `session_id` 会话缓存与永久图谱双层记忆。
  - 注意：LLM 调用是硬性成本，图谱质量取决于 LLM 能力；适合需要 Agent 长期记忆与关系推理的场景，简单事实检索用传统方案更经济。
- [Memvid](https://github.com/memvid/memvid)：Rust 编写的单文件 AI 记忆层，15.3K Star，将数据 + 嵌入向量 + 搜索结构 + 元数据全部打包进一个 `.mv2` 文件。
  - 与传统 RAG 的区别：无需向量数据库、分块策略、Embedding 服务，一个文件即完整记忆层，完全离线运行。
  - 文件格式：Smart Frames 序列（不可变帧），支持追加写入、时间线回溯（Rewind / Replay / Branch）、崩溃安全。
  - 内部结构：Header + WAL + 数据段 + Tantivy 全文索引 + HNSW 向量索引 + 时间索引 + TOC，无 .wal/.lock/.shm 副产品。
  - Feature Flags 按需开启：lex（BM25）/ vec（HNSW）/ pdf\_extract / clip（CLIP 图像）/ whisper（音频转写）/ encryption（加密胶囊 .mv2e）。
  - SDK：Rust / Python / Node.js / CLI，5 分钟跑通。
  - 适合：长期运行 Agent 跨会话记忆、企业知识库单文件分发、离线优先 AI、可审计 AI 工作流、医疗/法律/金融合规场景。
- [DeerFlow](https://github.com/bytedance/deer-flow)：字节跳动开源的 Super Agent Harness，49.9K Star，基于 LangGraph + LangChain，从零重写，MIT 协议。
  - 定位升级：从「超级智能体架构」演进为「Super Agent Harness」，强调对多 Agent 系统的驾驭工程能力。
  - 技能系统：Markdown 文件定义工作流，按需加载不撑爆上下文；内置研究、报告、幻灯片、网页开发、图片视频生成等技能，可扩展可替换。
  - 子智能体：主导智能体即时生成子智能体并行执行，各自拥有独立上下文和工具，完成后综合输出。
  - 沙盒环境：Docker 容器隔离执行，完整文件系统 + bash + 代码执行，可审计零污染；支持本地 / Docker / K8s 三种模式。
  - 上下文工程：总结已完成子任务、中间结果卸载到文件系统、压缩非即时内容，长任务保持敏锐。
  - 长期记忆：跨会话持久化用户偏好和积累知识，本地存储完全可控。
- [TabClaw](https://github.com/fishsure/TabClaw)：中科大 AGI 组开源的表格数据分析 Agent，拖入 CSV / Excel 后用自然语言提问，全程透明可控。
  - 显式工作流：意图理解（主动发问确认歧义）→ To-Do List 式规划（执行前可编辑步骤）→ Python 沙箱执行（代码级计算，非 LLM 口算）。
  - 持续进化：自动压缩历史对话为「思考板」保持上下文；记忆用户偏好和业务术语；复杂任务完成后自动沉淀为可复用技能。
  - 多 Agent 协作：多表比较时自动为每张表派遣专属分析 Agent 并行执行，汇总 Agent 标注一致发现与矛盾点。
- [Cua](https://github.com/trycua/cua)：AI 编程上下文治理工具，14.6K Star，解决大型项目中模型“上下文过载”导致的输出不稳定问题。
  - 核心逻辑：构建语义索引 + 依赖关系图，根据当前任务动态筛选最相关代码片段，而非全量喂入。
  - 上下文压缩（Context Pruning）：基于调用链和依赖关系自动过滤无关代码，从源头降低噪声。
  - 多源信息整合：结合数据库 Schema、接口文档等，让模型理解更接近真实运行环境。
  - 模型适配优化：针对 Claude 3.5 Sonnet 等主流模型优化，兼顾上下文长度利用率与响应效率。
  - 注意：小项目（单文件 / 少量模块）优势不明显，初次使用需建立索引有一定初始化成本。
- [MiniCode](https://github.com/LiuMengxuan04/MiniCode)：借鉴 Claude Code 设计的轻量级多语言实现（TypeScript / Python / Rust），500+ Star，适合 Agent 初学者学习、二次开发或作为简历项目。
  - 核心特性：兼容 MCP & Skills、后台任务、按项目隔离的会话持久化、provider usage 上下文记账、自动压缩与大工具结果落盘替换。
  - 配套「通过 MiniCode 你可以学习到 ClaudeCode 的哪些设计」学习文档。
- [OpenClaude](https://github.com/Gitlawb/openclaude)：Claude Code 源码泄露后社区重构的开源终端编码 Agent，19K Star，核心优势是模型不可知——支持 GPT / Gemini / DeepSeek / Ollama 等多模型后端。
  - 背景：2026.3 Anthropic 发布 Claude Code npm 包时 source map 泄露，社区还原约 1900 个 TypeScript 文件 / 51 万行源码，催生此项目。
  - 完整支持 MCP 协议、VS Code 扩展、终端优先工作流（文件操作 / 系统命令 / 流式输出 / 视觉能力）。
  - 与 Claude Code 对比：功能体验相近，但不锁定模型供应商、完全开源可审计、支持本地模型隐私优先。

## RAG 与文档处理

- [LLM Wiki](https://github.com/nashsu/llm_wiki)：基于 Karpathy LLM Wiki 理念的跨平台桌面应用，3.3K Star，自动将文档生成结构化个人知识库。
  - 两步链式录入：分析（提取实体 / 概念 / 关联 / 矛盾）→ 生成（写 Wiki 页面 + 交叉引用），质量优于一步到位。
  - 知识图谱可视化：sigma.js + ForceAtlas2 布局，四维度关联评分（直接链接 x3.0 / 来源重叠 x4.0 / Adamic-Adar x1.5 / 类型亲和 x1.0），集成 Louvain 社区发现算法。
  - 图谱洞察：自动发现「意外关联」（跨社区的连接）和「知识缺口」（孤立页面 / 稀疏社区 / 桥接节点），一键触发深度研究补全。
  - 深度研究：LLM 读 purpose.md 理解知识库方向 → 生成搜索词 → Tavily API 搜索 → 综合分析写成研究页面 → 自动录入 Wiki。
  - Chrome 网页剪藏：Readability.js 去广告 + Turndown.js 转 Markdown，支持多项目选择，离线也能预览。
  - 多阶段检索：分词搜索（中文 CJK 二元组分词）+ LanceDB 向量语义搜索 + 2 跳关联遍历，召回率从 58.2% 提升至 71.4%。
- [PageIndex](https://github.com/VectifyAI/PageIndex)：尝试用推理和文档结构替代传统向量检索，适合关注 RAG 可解释性和长文档阅读体验的场景。
  - 关注点：它直接针对传统 RAG 中的语义切分、相似度误召回、检索黑盒和文档结构丢失问题。
- [markitdown](https://github.com/microsoft/markitdown)：Python 工具，用于将文件和办公文档转换为 Markdown 格式。
- [marker](https://github.com/datalab-to/marker)：将 PDF 转换为 Markdown 和 JSON，适合做 PDF 内容抽取与结构化处理。
- [markit](https://github.com/Michaelliv/markit)：将 PDF、Word、PPT、Excel、HTML、EPUB、Jupyter、RSS、图片、音频、ZIP、URL 等多种格式统一转为 Markdown，也支持图像描述和音频转录。
- [opendataloader-pdf](https://github.com/opendataloader-project/opendataloader-pdf)：专为 RAG 管道设计的开源 PDF 解析引擎。
  - 解决三大痛点：XY-Cut++ 算法正确处理多栏阅读顺序、表格结构完整保留、带 bounding box 坐标实现精准溯源。
  - 基准测试综合第一（0.90）：阅读顺序 94%、表格 93%、标题 83%，表格识别远超同类工具（40%–89%）。
  - 多格式输出：同一次转换输出 Markdown / JSON / HTML / Tagged PDF，JSON 带 \[x1,y1,x2,y2] 坐标。
  - Hybrid 模式：可选调用 LLM 增强 OCR 和复杂表格识别，支持 80+ 语言，适合低质量扫描件。
  - AI 安全过滤：内置过滤隐藏文本、页外内容、Prompt 注入攻击，防止恶意 PDF 污染 RAG。
  - 集成：Python 3 行代码、LangChain 原生 Loader、Docker CLI、Node.js / Java SDK 均可。
  - 注意：底层引擎为 Java 实现，Python 包是封装层；Tagged PDF 无障碍功能 Q2 2026 发布。
- [ppt-master](https://github.com/hugohe3/ppt-master)：把 AI 生成的 SVG 转成 DrawingML，再生成真正可编辑的 PPT 文件。
- [RetainPDF](https://github.com/wxyhgk/retain-pdf)：开源全栈 PDF 翻译工具，支持扫描版 PDF，保留版面、公式与结构，适合科研与技术文档。
  - 核心能力：OCR 识别 + 翻译 + 排版重建完整链路，行内公式不崩、代码不误翻、表格可控、自定义翻译策略。
  - 开箱即用：Windows / macOS / Linux 安装包 + Docker Compose 部署（前端 40001 / Rust API 41000 / 同步接口 42000）。

## Web 自动化与采集

- [Firecrawl](https://github.com/firecrawl/firecrawl)：专为 AI Agent 设计的 Web 数据提取工具，125k Star，提供 Search / Scrape / Crawl 三大能力，输出干净结构化 Markdown / JSON / HTML / 截图，LLM-Ready 无需二次清洗。
  - 核心能力：智能渲染引擎（自动处理 JS 重度页面）、内置代理池 + 速率限制、支持 Actions（点击/滚动/输入/等待后再提取）、P95 延迟 3.4s、96% 网站覆盖率。
  - 典型场景：AI 搜索应用实时数据源、Agent 自主浏览网页、竞品情报批量抓取、文档站全量 Crawl 转 Markdown 语料库。
  - SDK：Python（`pip install firecrawl-py`）和 Node.js（`npm install @mendable/firecrawl-js`），配套 MCP Server 可接入 Claude Code / Cursor 等客户端。
  - 注意：开源版覆盖核心抓取能力，云端版额外提供 Agent 级智能交互与高级反爬绕过；需注册获取 API Key。
- [browser-use](https://github.com/browser-use/browser-use)：让 AI 代理更稳定地理解和操作网页，适合浏览器自动化任务。
  - 关键思路：把复杂网页转成 AI 更容易理解的编号化结构，让模型能像人一样观察页面后再执行动作。
- [page-agent](https://github.com/alibaba/page-agent)：纯 JavaScript 实现的 GUI Agent，支持用自然语言操作 Web 应用。
- [page-assist](https://github.com/n4ze3m/page-assist)：在浏览网页时调用本地运行的 AI 模型辅助理解页面内容与执行操作。
- [Scrapling](https://github.com/D4Vinci/Scrapling)：一个自适应 Web 爬虫框架，从小脚本到大规模采集都能覆盖。
  - 网站改版：解析器能记住元素特征，页面结构变化后自动重新定位目标节点。
  - 规模化采集：提供类似 Scrapy 的 Spider 框架，支持并发、暂停恢复、代理轮换与实时流式输出。
  - 反爬处理：内置 Cloudflare Turnstile 等常见反爬绕过能力，减少手动调参成本。
- [CloakBrowser](https://github.com/CloakHQ/CloakBrowser)：隐身版 Chromium，源码级 33 处 C++ 指纹补丁，30/30 通过所有机器人检测测试，可作为 Playwright / Puppeteer 的直接替换。
  - 定位：免费自托管的 Multilogin 替代方案，解决浏览器自动化被反爬检测拦截的问题。
  - 适合：需要绕过反爬的爬虫与自动化场景、AI Agent 浏览器操作。
- [maigret](https://github.com/soxoj/maigret)：通过用户名在 3000+ 网站上搜集目标人物档案的开源 OSINT 工具，无需 API Key，纯网页抓取。
  - 站点覆盖：默认扫描流量最高的 500 个站点，`-a` 全量；可按类别 / 国家过滤。
  - 递归搜索：根据发现的用户名和其他 ID 自动深挖关联账号。
  - 输出能力：Web 界面浏览图谱，HTML / PDF / XMind / JSON / CSV 报告；可选 `--ai` 生成调查摘要。
  - 适合：安全研究、OSINT 调查、记者背景调查。

## 系统与文件工具

- [nginxpulse](https://github.com/likaia/nginxpulse)：实时统计 Nginx 指标，支持 PV 过滤、IP 归属地与客户端解析。
- [fileview](https://github.com/Hiro-Chiba/fileview)：一个快速且轻便的文件与文件夹查看工具，适合临时浏览目录结构和文件内容。
- [paperless-ngx](https://github.com/paperless-ngx/paperless-ngx) ⭐：开源文档管理系统，扫描→OCR→自动分类→打标→全文搜索一体化。
  - 自动 OCR 识别：从图像中提取文字，建立全文搜索索引。
  - 自动分类打标：学习已有分类模式后自动分类，识别字段自动打标签。
  - 文档关联识别：识别文档间的对应关系（附件、补充条款、版本后续）。
  - 多用户权限管理：文档按用户组隔离，财务/人事/管理者可见范围不同。
  - 适合：有大量纸质文档需要数字化的个人和小型团队、自由职业者（合同发票管理）。

## 团队协作与文档

- [ShowDoc](https://github.com/star7th/showdoc)：PHP + Vue 开发的在线文档与接口协作工具，12.8K Star，专为 IT 团队设计，适合接口文档、数据字典、项目手册的统一管理。
  - 配套工具：RunApi 等辅助工具可将 API 直接转为文档。
  - 适合角色：后端 / 前端 / 产品 / 测试 / 运维 / 技术负责人。
- [MinDoc](https://github.com/mindoc-org/mindoc)：Golang 重写（前身 SmartWiki 基于 PHP），7.8K Star，轻量级文档管理系统，内置项目管理、用户管理、权限管理。
  - 适合：中小团队的接口文档、数据库字典、手册说明的存储与分享。
- [Smart-Doc](https://github.com/TongchengOpenSource/smart-doc)：零侵入的 Java 接口文档生成工具，静态解析标准 Java 注释，无需启动项目、无需额外注解。
  - 对比 SpringDoc/Swagger：零注解零侵入、静态解析离线生成、多模块友好、多格式输出（HTML / Markdown / Postman）。
  - 单模块：Maven 插件 + `smart-doc.json` 配置 + 标准 Java 注释，`mvn smart-doc:html` 一键生成。
  - 多模块：各模块独立配置 `sourceCodePaths` 引入 common 源码路径，Makefile 统一管理 CI/CD 集成。
  - 避坑：多模块必须配置 sourceCodePaths、packageFilters 精确到 controller 包、检查权限框架是否拦截 `/doc/**`。
  - 适合：Spring Boot 单模块 / 多模块 / 微服务项目，不想写 Swagger 注解的团队。

## Agent 沙箱与运行时

- [CubeSandbox](https://github.com/TencentCloud/CubeSandbox)：腾讯云开源的高性能 MicroVM 沙箱服务，5.1K Star，Rust 实现，基于 KVM + RustVMM 构建硬件级隔离，eBPF 网络隔离。
  - 核心指标：冷启动 < 60ms（P99 < 150ms）、内存开销 < 5MB、兼容 E2B API 降低迁移成本。
  - 架构：CubeAPI → CubeMaster → Cubelet / CubeProxy / CubeVS → CubeHypervisor / CubeShim → KVM MicroVM。
  - 适合：大规模 Agent 执行平台、多租户代码运行、E2B 替代或成本优化。
  - 注意：主要面向 Linux 生产环境，项目较新，社区和 SDK 生态还在积累中。
- [microsandbox](https://github.com/superradcompany/microsandbox)：本地、安全、可编程的 MicroVM 沙箱，6.0K Star，Rust 实现，强调开发者体验和可嵌入。
  - 技术栈：libkrun + KVM-based microVM + smoltcp 用户态网络 + OCI 镜像，提供 Rust / Python / TypeScript SDK。
  - 优势：可被应用直接嵌入的沙箱 SDK，支持 MCP，跨平台，开发者体验最好。
  - 适合：本地 Agent 开发环境、桌面端或开发工具内嵌沙箱、需要快速验证 Agent 执行能力的产品。
  - 注意：生产验证相对不足，高并发多租户场景仍需重点压测。
- [OpenSandbox](https://github.com/alibaba/OpenSandbox)：阿里巴巴开源的企业级沙箱运行时，10.5K Star，Python 实现，强调安全合规与可扩展性。
  - 定位：企业工程体系里的沙箱框架，权限模型、审计、隔离策略、部署治理优先于极限性能。
  - 适合：企业内部 AI 应用运行时、需要审计合规安全治理的场景、已在阿里云或类似基础设施上的团队。
- [agent-infra/sandbox](https://github.com/agent-infra/sandbox)：All-in-One Agent 沙箱环境，4.5K Star，把 Browser + Shell + File + MCP + VSCode Server + Jupyter 放进一个 Docker 容器。
  - 思路：不追求最强隔离，追求「Agent 一进去就什么都有」，开箱即用。
  - 适合：原型验证、内部开发测试、Agent Demo 环境、需要浏览器 / 终端 / 文件 / IDE 一体化的任务。
  - 注意：不适合强多租户隔离和高风险不可信代码执行；效率优先，不是安全边界优先。
- [kubernetes-sigs/agent-sandbox](https://github.com/kubernetes-sigs/agent-sandbox)：K8s SIG 出品的 Agent 沙箱管理方案，2.1K Star，Go 实现，通过 CRD 管理隔离、有状态、单例工作负载。
  - 资源模型：SandboxClaim → SandboxTemplate → Sandbox → Pod → Runtime，支持 SandboxWarmPool 预热池。
  - 价值：把 Agent 沙箱纳入 K8s 声明式 API / Controller / RBAC / 监控 / GitOps 现有体系。
  - 适合：已有成熟 K8s 平台的团队；小团队和单机开发成本偏高。
- [sandbox-agent](https://github.com/rivet-dev/sandbox-agent)：统一控制多种 Coding Agent 的控制层，1.3K Star，TypeScript 实现，通过 HTTP 控制 Claude Code / Codex / OpenCode / Amp。
  - 核心价值：Universal Agent API，统一会话 schema 输出事件，不让业务系统分别适配每种 CLI。
  - 适合：Coding Agent 平台、多 Agent 对比和路由、统一会话记录回放审计。
  - 注意：不是底层安全沙箱，更合理的组合是底层用 CubeSandbox / microsandbox，上层用 sandbox-agent 统一控制。
- [openbrowser](https://github.com/ntegrals/openbrowser)：让 AI Agent 浏览网页的浏览器自动化工具包，9.4K Star，把浏览器能力封装成更适合 Agent 使用的接口。
  - 解决的问题：直接把 Playwright 暴露给模型会导致上下文噪声和操作不稳定。
  - 适合：Web 自动化 Agent、信息检索和网页操作，作为完整沙箱环境里的浏览器层组件。
  - 注意：浏览器隔离不等于系统隔离，网页操作之外还需完整沙箱承载。
- [context-mode](https://github.com/mksglu/context-mode)：优化 AI Coding Agent 上下文窗口的输出沙箱 / 压缩层，13.5K Star，对工具输出做隔离和压缩。
  - 解决的问题：Agent 执行命令时大量日志、文件内容、搜索结果塞回模型上下文，声称减少 98% 工具输出 token 消耗。
  - 本质：认知隔离而非安全隔离，把执行环境噪声挡在模型上下文之外。
  - 适合：和其他沙箱组合使用，改善长任务 Agent 的上下文质量和 token 成本。

> 选型参考：生产级高性能 → CubeSandbox；本地开发 SDK 嵌入 → microsandbox；快速原型 → agent-infra/sandbox；K8s 原生管理 → kubernetes-sigs/agent-sandbox；企业安全合规 → OpenSandbox。

## 金融数据工具

- [FinceptTerminal](https://github.com/Fincept-Corporation/FinceptTerminal)：开源金融数据终端，行情、技术分析、宏观经济、投资组合、量化回测，462 Star。
  - Python 编写自定义指标与策略，注册后与内置指标同等使用。
  - 跨市场相关矩阵热力图：计算资产组合收益率相关性。
  - 经济日历 AI 摘要：自动生成经济数据的市场预期和影响分析。
  - 夜间模式优化：适合长时间盯盘分析。
  - 适合：个人投资者、量化策略研究者、不愿支付 Bloomberg 月费的专业用户。

