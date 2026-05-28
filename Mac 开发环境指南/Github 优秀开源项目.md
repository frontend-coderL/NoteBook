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

## Agent 与工作流

- [DeepSeek-TUI](https://github.com/Hmbown/DeepSeek-TUI)：终端原生的 DeepSeek 编程智能体方案。
- [hermes-agent](https://github.com/NousResearch/hermes-agent)：自我提升 AI 代理，强调从经验中积累技能、检索历史对话并持续优化自身行为。

## RAG 与文档处理

- [PageIndex](https://github.com/VectifyAI/PageIndex)：尝试用推理和文档结构替代传统向量检索，适合关注 RAG 可解释性和长文档阅读体验的场景。
  - 关注点：它直接针对传统 RAG 中的语义切分、相似度误召回、检索黑盒和文档结构丢失问题。
- [ppt-master](https://github.com/hugohe3/ppt-master)：把 AI 生成的 SVG 转成 DrawingML，再生成真正可编辑的 PPT 文件。

## Web 自动化与采集

- [browser-use](https://github.com/browser-use/browser-use)：让 AI 代理更稳定地理解和操作网页，适合浏览器自动化任务。
  - 关键思路：把复杂网页转成 AI 更容易理解的编号化结构，让模型能像人一样观察页面后再执行动作。
- [page-agent](https://github.com/alibaba/page-agent)：纯 JavaScript 实现的 GUI Agent，支持用自然语言操作 Web 应用。
- [page-assist](https://github.com/n4ze3m/page-assist)：在浏览网页时调用本地运行的 AI 模型辅助理解页面内容与执行操作。
- [Scrapling](https://github.com/D4Vinci/Scrapling)：一个自适应 Web 爬虫框架，从小脚本到大规模采集都能覆盖。
  - 网站改版：解析器能记住元素特征，页面结构变化后自动重新定位目标节点。
  - 规模化采集：提供类似 Scrapy 的 Spider 框架，支持并发、暂停恢复、代理轮换与实时流式输出。
  - 反爬处理：内置 Cloudflare Turnstile 等常见反爬绕过能力，减少手动调参成本。

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