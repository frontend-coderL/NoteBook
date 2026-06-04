## 编程学习与方法论

- [build-your-own-x](https://github.com/codecrafters-io/build-your-own-x)：通过从零开始重现你喜欢的技术来掌握编程，适合按专题系统补基础。
- [SkillOpt](https://aka.ms/SkillOpt)：把 Agent 的 skill 当作可训练的软件资产来优化，通过真实执行轨迹、结构化编辑和验证门持续改进外部技能文档，适合用来学习 Agent 如何沉淀流程经验，而不只是反复手改提示词。
  - 核心机制：基于 rollout、反思分析和 `add / delete / replace` 编辑迭代 skill 文档，再用 selection split 验证候选改动是否真的有效。
  - 值得关注：它把学习率、验证集、momentum 这类训练纪律迁移到文本技能优化中，适合理解企业 Agent、代码 Agent、办公自动化 Agent 的流程能力如何被系统性训练出来。
- [ai-engineering-from-scratch](https://github.com/rohitg00/ai-engineering-from-scratch) ⭐：项目驱动的 AI 工程学习路线，从本地模型推理到 RAG 系统到 Agent 协作到生产部署，3.1k Star。
  - 企业级项目模板：Docker 健康检查、Prometheus 监控、JSON 日志等生产级工程实践。
  - 跨项目代码积累：前一个项目代码可复用，累积效果可见。
  - 缺陷数据库：常见问题与解决方案汇总，避免重复踩坑。
  - 适合：想系统学习 AI 工程的学生、转行者、需要快速培养团队 AI 能力的技术管理者。
- [advanced-java](https://github.com/doocs/advanced-java)：Java 进阶知识库，82K Star，覆盖高并发、分布式、微服务、中间件等高阶技术，同时汇总大厂面试真题与解析，MIT 协议。
  - 优势：贴合国内大厂技术体系与面试风格，中文讲解通俗易懂，持续迭代更新。
  - 注意：偏知识汇总而非项目实战，适合查漏补缺和面试备考，不适合替代官方文档学习具体框架。

## Agent 与工作流

- [12-factor-agents](https://github.com/humanlayer/12-factor-agents)：构建生产级 LLM 应用的 13 条工程化法则，21.9K Star，灵感来自经典 12-Factor App，每条都有 TypeScript / Python 双版本示例，Apache 2.0。
  - 核心思想：代码管流程、LLM 管判断——别反过来。
  - 优势：不绑定任何框架，拿来读的而非装的；覆盖控制流、提示词管理、错误自愈、人工介入等生产痛点。
  - 注意：是方法论而非框架，不能 `pip install`，需自行按法则改造现有代码。
  - 适合：想把 Agent 从 demo 带到生产环境的团队。
- openai-agents-python：OpenAI Agents SDK 的 Python 实现，用于构建多代理工作流，支持 OpenAI Responses API、Chat Completions API 以及 100+ 其他大模型。
- [DeepSeek-TUI](https://github.com/Hmbown/DeepSeek-TUI)：终端原生的 DeepSeek 编程智能体方案。
- [hermes-agent](https://github.com/NousResearch/hermes-agent)：自我提升 AI 代理，强调从经验中积累技能、检索历史对话并持续优化自身行为。

## RAG 与文档处理

- [PageIndex](https://github.com/VectifyAI/PageIndex)：尝试用推理和文档结构替代传统向量检索，适合关注 RAG 可解释性和长文档阅读体验的场景。
  - 关注点：它直接针对传统 RAG 中的语义切分、相似度误召回、检索黑盒和文档结构丢失问题。
- [ppt-master](https://github.com/hugohe3/ppt-master)：把 AI 生成的 SVG 转成 DrawingML，再生成真正可编辑的 PPT 文件。

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

## 金融数据工具

- [FinceptTerminal](https://github.com/Fincept-Corporation/FinceptTerminal)：开源金融数据终端，行情、技术分析、宏观经济、投资组合、量化回测，462 Star。
  - Python 编写自定义指标与策略，注册后与内置指标同等使用。
  - 跨市场相关矩阵热力图：计算资产组合收益率相关性。
  - 经济日历 AI 摘要：自动生成经济数据的市场预期和影响分析。
  - 夜间模式优化：适合长时间盯盘分析。
  - 适合：个人投资者、量化策略研究者、不愿支付 Bloomberg 月费的专业用户。

