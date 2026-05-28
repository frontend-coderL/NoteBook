## Python

### Agent 框架

- [openai-agents-python](https://github.com/openai/openai-agents-python) ⭐：多代理工作流框架，26.7k Star，支持 OpenAI API 及 100+ 其他 LLM。
  - 核心能力：Agent 配置（指令/工具/护栏）、Sandbox Agent 容器化执行、MCP/函数/托管工具、输入输出验证、人机协作、内置追踪调试。
  - 适合：需要多 Agent 协作和复杂工具调用的项目。

### 文档处理

- [markitdown](https://github.com/microsoft/markitdown) ⭐：AutoGen Team 出品，127k Star，PDF/Office/图片/音频等转 Markdown。
  - 支持：PDF、PPT、Word、Excel、图片（OCR）、音频（转录）、HTML、CSV、ZIP、YouTube 等。
  - 特点：保留文档结构（标题/列表/表格/链接），CLI + Python API，插件可扩展。
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

## TypeScript

### Agent 框架

- [Pi (earendil-works)](https://github.com/earendil-works/pi) ⭐：AI Agent 全栈工具箱，56.8k Star，统一 OpenAI/Anthropic/Google 等多模型 API，支持 CLI 和 SDK 两种集成方式。
  - `pi-coding-agent`：终端编码 Agent CLI。
  - `pi-agent-core`：运行时核心，工具调用与状态管理，可集成到其他应用。
  - `pi-ai`：统一 LLM API，流式输出 + 工具调用定义。
  - `pi-tui`：终端 UI 库，差分渲染。
  - `pi-chat`：Slack 机器人与聊天自动化。
  - 工程化亮点：依赖版本精确锁定 + shrinkwrap 隔离，供应链安全。
  - 适合：团队构建内部 coding assistant，多模型快速切换对比，软件过程自动化。