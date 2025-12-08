+++
date = '2025-12-06T11:30:20+08:00'
draft = true
title = '聊聊AI 项目落地与能力边界'
+++

过去两年，AI 行业经历了一次从兴奋到冷静的完整周期。模型越来越强、参数越来越大、演示视频越来越惊艳，但真正把 AI 系统接到业务里的人，都能感受到同一件事情：

> **AI 的能力在狂飙，但业务对“确定性”的需求从未改变。**

在一个概率性技术之上构建确定性的商业系统，这是 2025 年所有 AI 项目共同的挑战。本报告尝试从工程、产品和经济角度，梳理当前 AI 落地的真实边界，并给出一套可操作的评估框架，帮助团队避免走进“看起来很酷但做不出来”的项目陷阱。

# 1. 热潮退去后的冷现实：能力强，不代表能落地

2025 模型能力迅速提升，推理能力确实比 2024 年强不少。但 Apple 在 2025 年发布的一项研究给出了一个让工程师们非常“共情”的结论：

> **推理模型的“聪明”，更多是高级模式匹配，而不是真正的逻辑推导。**

当问题复杂度超过某个阈值后，模型的准确率会突然崩掉，像“相变”一样不是一点点下降，而是直接跳水。

这意味着：

- 业务逻辑越复杂，模型越容易乱想
- 增加 token 长度、延长思考时间，对结果不一定有帮助
- 某些场景里，小模型反而比大模型更稳定

对于产品经理来说，一个最重要的启示是：

> **不要以为加参数、加算力就能解决推理问题。很多需求从一开始就不是 AI 能解决的。**

# 2. 概率模型 vs. 业务确定性：天然矛盾

LLM 的运行机制决定了它永远是概率性的。哪怕你用同一个 prompt，两次调用也可能产生不同输出。

这与企业的技术预期存在天然冲突：

| 传统软件                | AI 系统                      |
| ----------------------- | ---------------------------- |
| 输入一样 → 输出必然一样 | 输入一样 → 输出可能变化      |
| 逻辑清晰、可测试        | 不可测试、不可穷举、不可解释 |
| 容错低                  | 容错依赖使用场景             |

因此，项目评估的第一步不是“模型行不行”，而是：

## ** 这是一个需要 100% 正确的任务吗？**

- **是 → 不适合 AI（例如税务计算、积分结算、风控规则）。**
- **否 → 才有可能进入下一阶段评估。**

这条原则能帮产品经理砍掉一半不该做的“AI 项目”。

# 3. 幻觉不是 bug，而是机制注定

模型不知道答案时，会为了最大化下一 token 的概率而编造一个“看起来合理”的内容。这是训练目标导致的内生缺陷，不是升级个版本就能解决的问题。

幻觉在不同领域的风险完全不一样：

| 场景                   | 幻觉容忍度 | 风险               |
| ---------------------- | ---------- | ------------------ |
| 写文案、写创意         | 高         | 可用               |
| 商业分析、总结会议纪要 | 中         | 需要验证           |
| 法律、医疗、金融       | 极低       | 禁用或必须加验证层 |

尤其在法律、金融这类“事实必须正确”的领域，幻觉不是体验问题，是法律风险问题。

# 4. 上下文与检索：长文本不是银弹

今年长上下文模型卷得很厉害，128k、1M token 层出不穷。但实际工程里我们看到的是：

> **能塞进去 ≠ 能读懂 ≠ 能正确推理。**

上下文越长，模型越容易被干扰信息稀释。例如：

- 关键信息淹没在长文中
- 检索到答案但逻辑引用错误
- 注意力被干扰 token 分散

所以，在长文本任务（例如合同审查、标书比对）中，必需采用：

- 分块（Chunking）
- 多级检索（Hierarchical Retrieval）
- 重排序（Reranking）
- 结构化提示 / 模板化

否则效果会迅速掉到不可用。

# 5. 智能体（Agent）风潮：能 demo，但落地难

2025 年最火的趋势之一是 Agentic Workflow —— 自主规划、工具调用、多轮反思。

但现实是：

### **10 步的 AI 流程，即使每一步成功率 95%，整体成功率也只有 ~60%。**

业务流程越长，失败率越高，甚至出现：

- 死循环
- 随机跳步骤
- 工具调用参数乱飞
- 逻辑跑偏

因此真正能落地的代理系统基本都有一个特点：

> **关键环节必须有人类审批。**

全自动代理，是未来, 但不是现在。

# 6. RAG（检索增强生成）：成本被严重低估

RAG 是解决幻觉和知识时效性的主流方案，但它的隐形成本往往比模型更贵：

- 需要维护向量数据库
- 构建和优化索引
- Chunk + 重排序调参非常多
- 文档清洗、更新、增量重建都需要长期投入

很多团队做了 RAG 才发现：

> 模型便宜了，但系统起来更贵了。

对于小规模知识库（几万条以内），用结构化数据库 + 模板化提示往往更便宜、更稳定。

# 7. 数据：决定模型生死的基础设施

Andrew Ng 有一句非常准确的判断：

> **很多失败的 AI 项目都不是算法问题，而是数据质量问题。**

2025 年大量团队开始用 LLM 生成“合成数据”来微调小模型，但风险同样明显：

- 合成数据会引入系统性偏差
- 微调模型会放大偏差
- 少量错误数据也可能导致模型失调
- 标注噪声在某些 NER 任务里高达 45%

如果项目没有“黄金验证集”（由人严格标注的小规模高质量数据集），项目的可行性就站不住脚。

# 8. 成本与 ROI：为什么这么多 AI 产品赚不到钱？

虽然模型价格每年下降 10 倍，但 AI 应用有一个传统软件没有的特征：

> **每次调用都有成本（token 成本）。**

如果你的产品：

- 用户高频
- 回答长度长
- 还要进行人工二次复核

那你很可能会遇到一个现实：

> **毛利率比传统 SaaS 低得多，甚至为负。**

真正能跑通 ROI 的 AI 项目目前集中在：

- 任务单价高（比如投顾、法律、医学）
- 代替人工显著省成本
- 复用率高
- 不用大模型也能做到 95% 准确度, 使用规则引擎 + 小模型组合成本更低

换句话说，越“酷”的 AI 产品，越难赚钱。

# 9. 合规：决定很多项目从根源就做不了

2025 年开始，AI 的监管进入实质阶段。欧盟 AI Act 已经明确禁止：

- 潜意识操纵
- 社会信用评分
- 工作场合的情绪识别
- 公共场合实时生物识别
- 基于个人特征的预测性警务

很多需求评审会上出现的点子，实际上是法律禁止的。

即使不是禁止类，涉及招聘、信用、医疗等“高风险 AI”的项目，也会遇到：

- 30% 以上的预算用于合规
- 更长的交付周期
- 必须可解释
- 必须全程有人类监督

对于初创团队，这几乎意味着：**做不了。**

# 10. 一套可真正落地的 AI 项目五步评估框架

综合认知边界、工程边界、数据边界、经济边界与合规边界，我们总结出一个很实用的五步评估法。

## **第 1 步：自动化测试（Automation Test）**

判断是否真的需要 AI。

**如果答案可用规则、SQL、Excel 实现：不要用 AI。**

## **第 2 步：数据就绪度检查（Data Readiness）**

没有良好数据，所有 AI 方案都是空谈。

## **第 3 步：能力边界评估（Cognitive Boundaries）**

判断任务属于：

- 模式匹配型（可做）
- 推理型（谨慎）
- 复杂推理链（大概率失败）

## **第 4 步：工程落地性评估（Engineering Feasibility）**

RAG、长上下文、代理能不能支撑你的需求？

## **第 5 步：ROI 与合规评估（Business & Compliance）**

能不能赚钱？能不能合法上线？

# 结语：AI 的价值来自“裁剪”，不是“堆砌”

2025 年的 AI 技术越来越强，但真正能落地、能产生价值的项目，往往有一个共同点：

> **不是把所有能力都堆上去，而是精准裁剪——只做模型能稳定胜任的部分，再用工程补齐剩下的确定性。**

当我们接受模型的“概率性本质”，就能以更理性的方式去构建可靠的 AI 系统。未来几年，不是 Demo 决定成败，而是工程细节、数据质量、ROI 和合规。

## 参考资料

- [The Hidden Cost of Agentic AI: Why Most Projects Fail Before Reaching Production](https://galileo.ai/blog/hidden-cost-of-agentic-ai)
- [Probabilistic and Deterministic Results in AI Systems](https://www.gaine.com/blog/probabilistic-and-deterministic-results-in-ai-systems)
- [AI Project Feasibility Checker](https://lasoft.org/blog/ai-project-feasibility-checker/)
- [AI Is Not a Panacea: Why Most Projects Fail and How to Use It Wisely](https://timspark.com/blog/why-ai-projects-fail-artificial-intelligence-failures/)
- [Limitations of AI: What’s Holding Artificial Intelligence Back in 2025?](https://visionx.io/blog/limitations-of-ai/)
- [Andrew Ng On AI Agentic Workflows And Their Potential For Driving AI Progress](https://www.youtube.com/watch?v=q1XFm21I-VQ)
- [One year of agentic AI: Six lessons from the people doing the work](https://www.mckinsey.com/capabilities/quantumblack/our-insights/one-year-of-agentic-ai-six-lessons-from-the-people-doing-the-work)
- [The Hidden Costs of AI Deployment: Why Enterprises Need Smarter Optimization](https://trustwise.ai/hidden-costs-of-ai-deployment/)
- [How to Calculate AI ROI for Your Business](https://www.multimodal.dev/post/how-to-calculate-ai-roi)
