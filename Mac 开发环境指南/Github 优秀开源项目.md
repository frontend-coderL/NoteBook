## 编程学习与方法论

- [build-your-own-x](https://github.com/codecrafters-io/build-your-own-x)：通过从零开始重现你喜欢的技术来掌握编程，适合按专题系统补基础。
- [SkillOpt](https://aka.ms/SkillOpt)：把 Agent 的 skill 当作可训练的软件资产来优化，通过真实执行轨迹、结构化编辑和验证门持续改进外部技能文档，适合用来学习 Agent 如何沉淀流程经验，而不只是反复手改提示词。
  - 核心机制：基于 rollout、反思分析和 `add / delete / replace` 编辑迭代 skill 文档，再用 selection split 验证候选改动是否真的有效。
  - 值得关注：它把学习率、验证集、momentum 这类训练纪律迁移到文本技能优化中，适合理解企业 Agent、代码 Agent、办公自动化 Agent 的流程能力如何被系统性训练出来。

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