> **首发 [https://javis.im/analysis/langchain/](https://javis.im/analysis/langchain/)，本文随站点同步更新**；文中图片见在线原页，英文版：https://javis.im/analysis/langchain/index-en

# 148,045 星、月下载 1.70 亿：LangChain 三年把自己写的高层抽象废掉两轮，1.0 留下的那个还搬了两次家

> 148,045 星、月下载 1.70 亿的框架，三年里把自己写的高层抽象废掉两轮，1.0 留下的那个还搬了两次家。本文用官方带日期的家谱拆四代抽象的生与死，附四组数字的正确读法。

取证日期：2026-10-19（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 18 · 可验证性 19 · 独特性 17 · 受众相关度 18 · 多维空间 18 = **90/100**（≥60 才动笔；素材库预分 84）。

## 这个站在做什么

LangChain 的官网标题写的是「the open agent platform to own your intelligence」，文档站首页给自己的定义是「The open agent engineering ecosystem」——三层分工：`langchain` 负责快速起 agent，`langgraph` 负责低层可控编排，`deepagents` 负责长任务 harness，底下统一跑在 LangGraph 上，外面再套一个 LangSmith 平台做 Build / Test / Deploy / Monitor / Govern 全流程。（来源 1/3/23）【事实】

取证日的官网首屏是 **LangSmith Engine** 的产品广告（"Smarter after every launch"），下方是 Rippling、Vanta、Harvey、Expedia、NVIDIA、Coinbase、ServiceNow 等客户 logo 墙——**开源框架已经退到导航栏 Products → Open Source 里**，官网本体是平台营销页。（来源 1）【事实】

![LangChain 官网首页：LangSmith Engine 首屏 + 客户 logo 墙，取证 2026-10-19](img/home.png)

一个必须先摆出来的可达性问题：素材库登记的目标 URL `https://langchain.ai` 在取证日**双通道都不可达**——通道A 服务端抓取返回 530（Cloudflare error 1016，origin DNS 解析失败），本机代理直连在 TLS 握手阶段被断开；同域的 `https://www.langchain.com` 两条通道都是 200。（来源 1/2）【存疑】本文所有引用一律指向 www.langchain.com、docs.langchain.com 与 GitHub/PyPI，apex 域名是否已弃用无一手信息。

## 技术栈考古 · 官方带日期的家谱，先摆一手证据

这篇的分析框架卡是**技术栈考古**：不评「好不好用」，只挖「抽象换了几代、谁推翻了谁、每次换代的原话是什么」。最好的一手材料来自 LangChain 自己——官方文档 Philosophy 页里嵌着一条**带日期徽章的 History 时间线**（本文用通道D 浏览器抓取 innerText 归档）：（来源 3）

- **2022-10-24 v0.0.1**：比 ChatGPT 早一个月发布，两大组件就是「LLM 抽象」和「Chains（预定计算步骤）」，名字也来自 Language + Chains；
- **2022-12** 加入基于 ReAct 论文的第一代通用 agent（生成 JSON 表示工具调用再解析）｜**2023-01** ChatCompletion API 适配 + JS 版发布｜**2023-02** LangChain Inc. 成立｜**2023-03** function calling 转为首选工具调用方式｜**2023-06** LangSmith 闭源平台上线；
- **2024-01 v0.1.0**：官方措辞是「its first non-0.0.x」，行业从原型走向生产，稳定性提级；
- **2024-02 LangGraph 作为独立开源库发布**，官方给的理由写得很直白：原版 LangChain「missing a low-level orchestration layer that allowed developers to control the exact flow of their agent」；
- **2024-06** 集成数破 700 并拆包（核心集成独立成包、其余进 `langchain-community`）；
- **2024-10 LangGraph 成为「任何比单次 LLM 调用更复杂的应用」的首选**，同一条目原话：「Most chains and agents were marked as deprecated in LangChain with guides on how to migrate them to LangGraph」；
- **2025-10-20 v1.0.0**：chains 与 agents 全部推倒重做，只剩一个高层抽象——建在 LangGraph 上的 agent 抽象；不想升级的人可以装 `langchain-classic` 继续用旧链；
- **2026-03-15** Deep Agents 作为基于 LangGraph 的 agent harness 发布（时间线条目标的版本号是 deepagents 自己的 v0.5.3，与 langchain 本体版本线不是一回事，引用时注意）。

![官方 Philosophy 页 History 时间线（2022-10-24 v0.0.1 起），取证 2026-10-19](img/history.png)

## 技术栈考古 · 四代抽象的生与死

**第一代 Chain（2022-10）**：`LLMChain`、`ConversationChain` 这类「把 prompt 模板、模型、输出解析焊死在一起」的预置链，配 ReAct 解析 JSON 的 agent。它在 2023 年的教程里无处不在，也成了后来所有批评的靶子。（来源 3/14）【事实】

**第二代 LCEL（官宣 2023-08-01）**：LangChain Expression Language，用 `|` 管道把 prompt、model、parser 组合起来，官方 TL;DR 是「a new syntax to create chains with composition」，batch、async、streaming 开箱即用。（来源 7）【事实】这正是后来被 Octomind 工程师点名的那三行管道：同样一件事，裸 OpenAI SDK 一个调用搞定，LCEL 要三个类、四个函数调用、三个新抽象。（来源 14）

**第三代 LangGraph（官宣 2024-01-17，时间线记 2024-02）**：低层图编排，节点和边就是 Python 函数。真正关键的是官方自己在 2024-06-27 的 v0.1 官宣里写的那句话：「This was true of the legacy LangChain AgentExecutor as well, but what we learned through iteration and experience is that **the higher level abstraction hides too many details from the developer, ultimately resulting in a system lacking the control needed to accomplish tasks reliably**」。（来源 10）【事实】

**第四代 1.0（2025-10）**：`create_agent` + middleware 成为唯一门面。v1 迁移指南把 `LLMChain`、`ConversationChain`、旧 retriever/index/hub 全部挪进 `langchain-classic`（该包取证日仍在发版，最新 1.0.8），并给出 `from langchain_classic.chains import LLMChain` 的兼容写法。（来源 4/5/19）【事实】

反常识的一幕写在官方时间线的 1.0 条目里：这唯一保留的高层抽象「was the high-level abstraction that was **originally created in LangGraph, but just moved to LangChain**」；结合同页 2024-10 条目（LangGraph 里那个 agent 抽象「has the same interface as the ReAct agents from LangChain」）与 v1 迁移指南（1.0 之前官方推荐的是 `langgraph.prebuilt.create_react_agent`），完整轨迹是：**ReAct agent 抽象 2022 年诞生在 langchain，2024 年整体搬进 langgraph，2025 年又被搬回 langchain 当 1.0 的唯一门面——同一个抽象在两个包之间搬了两次家**。（来源 3/5）【事实】【推断】

1.0 官宣里官方自陈的体感数据同样直白：「we've heard consistent feedback: **LangChain's abstractions were sometimes too heavy, the package surface area had grown unwieldy**, and developers wanted more control over the agent loop」。（来源 11）【事实】

版本节奏本身也是一条考古证据：0.0.x 时代熬了约 14 个月才发 0.1.0（官宣 2024-01-08），随后 0.2.0（PyPI 上传 2024-05-17）、0.3.0（2024-09-13）、1.0.0（2025-10-17），到取证日已是 langchain 1.4.6（2026-10-14）与 langchain-core 1.6.12（2026-10-16）在发版。（来源 19）【事实】

## 论战 · 过度工程之争，双方出处都是一手的

**批评方**：Octomind 的 Fabian Both 把 12 个月生产使用写成《Why we no longer use LangChain for building our AI agents》（2024-06）——「When our team began spending as much time understanding and debugging LangChain as it did building features, it wasn't a good sign」，结论是「All LangChain has achieved is increased the complexity of the code with no perceivable benefits」，并给出 LCEL vs 裸 SDK 的逐行对照。（来源 14）【事实】此文在 HN 拿到 **480 points、297 comments**（2024-06-20 提交，取证日页面实抓）。（来源 13）【事实】

![HN 论战帖：480 points / 297 comments / 2024-06-20，取证 2026-10-19](img/hn.png)

评论区的社群声音同样是原文可引：「LangChain is _the_ definition of death by abstraction」「I have read the whole source of LangChain in Rust … it definitely seems over-engineered」「an incredible amount of abstraction and paradigms that were completely unnecessary for what I was doing」。（来源 13）【事实】

**官方认账**：同帖第二天，LangChain CEO Harrison Chase 以 `hwchase17` 身份下场回复，原话是——「**The initial version of LangChain was pretty high level and absolutely abstracted away too much.** We're moving more and more to low level abstractions」，并承认 LangGraph 的目标是「replace the LangChain AgentExecutor (which as they noted was opaque)」。（来源 13）【事实】

![CEO Harrison Chase 在 HN 帖内的认账评论（2024-06-21），取证 2026-10-19](img/hn_ceo.png)

**外部旁证**：Anthropic 的工程方法论文章（2024-12-19）写的是「the most successful implementations use **simple, composable patterns rather than complex frameworks**」——注意它没有点名 LangChain，本文只把它当作对「框架重抽象」这条路线的行业提醒，不作直接证据使用。（来源 15）【事实】更直接的一手旁证是 humanlayer 的 12-factor-agents（2025-03-30 创建、取证日 26,764 星）：作者写自己试遍所有 agent 框架后，「Most of them are rolling the stack themselves. I don't see a lot of frameworks in production customer-facing agents.」（来源 16）【事实】

## 口径对账 · 四组数字的正确读法

1. **星数**：GitHub API 实抓 `langchain-ai/langchain` **148,045 星**（24,852 forks、924 watchers、MIT、仓库创建 2022-10-17），页面 HTML 显示 148.0k——同源不同精度，不是两个数。同期 `langgraph` **43,469 星**、`langchainjs` **18,279 星**，Python 侧星数约为 JS 侧的 8.1 倍。（来源 17/18）【事实】仓库创建日 2022-10-17 比官方时间线的 2022-10-24 早 7 天，属「建仓先行、发包在后」的正常差。（来源 3/17）【推断】
2. **issue 数**：API 的 `open_issues_count` 是 **657**，页面 HTML 分列 **542 issues + 115 pull requests**——657 = 542 + 115，API 口径把 PR 算进去了。引用「多少 issue」时必须说明是哪种口径，否则会虚高 21%。（来源 13/17）【事实】
3. **下载量**：pypistats 实抓 `langchain` **170,457,766 次/月**（last_day 4,809,481）、`langgraph` **44,769,952 次/月**；npm 注册表实抓 `langchain` JS 包 **12,760,965 次/月**（2026-09-17→10-16）——Python 侧约是 JS 侧的 13 倍。（来源 19/20）【事实】Harrison 在 2025-10-20 的三年反思里说「80 million monthly downloads」，与取证日的 1.70 亿不在同一时点、也未说明是单包还是全家桶口径，本文两组数字并列，不连线算增长率。（来源 12/19）【存疑】
4. **发布日期**：LangChain 1.0 有三个官方日期——PyPI 上传 **2025-10-17**、官方时间线标 **2025-10-20**、官宣博客 **2025-10-22**；LangGraph 同样有三个——仓库创建 **2023-08-09**、官宣博客 **2024-01-17**、官方时间线标 **2024-02**，v0.1 稳定版则是 **2024-06-27**。引用任何一个都要带上出处。（来源 3/9/10/11/19/18）【存疑】

## 商业侧一笔带过（同一份证据链）

2025-10-20，Harrison 在《Reflections on Three Years of Building LangChain》里同时官宣 **$125M 融资、$1.25B 估值**，并给出「over a million developers」与「80 million monthly downloads」两个规模口径；同一篇里他也承认 LangGraph「did have a higher learning curve」，1.0 是「massive breaking changes」的产物，社区反馈是「We hear you」。（来源 12）【事实】到 2026 年，LangSmith 已扩成 Agent Engineering Platform（Build/Test/Deploy/Monitor/Govern + LLM Gateway + Fleet 无代码 agent + Engine 自动修问题），2026-07-25 的最新战略文标题是《Own Your Intelligence》。（来源 1/21/22）【事实】

【观点】把它放回本专栏的坐标系：OpenRouter 用抽成回答变现（第 2 篇），LiteLLM 用「never per token」回答（第 3 篇），LangChain 的答案是**开源框架当获客层、平台当收入层**——170M 月下载买的不是云资源账单，是 LangSmith 的销售漏斗入场券。

## 风险与存疑清单

1. **apex 域名不可达**：`langchain.ai` 通道A 530/1016、通道C TLS 断；www 正常。发外链一律用 www 或子域。（来源 1/2）【事实】
2. **双轨承诺的边界**：1.0 官宣承诺「no breaking changes until 2.0」，但 1.0 本身就是这篇三年史里最大的一次 breaking change——承诺只覆盖 1.x 区间；旧链靠 `langchain-classic`（1.0.8）延命，双轨维护成本最终由使用者承担。（来源 11/19）【推断】
3. **中立性/漏斗质疑**：2026-04-02 有第三方媒体写《LangChain Is Quietly Losing Developers》，核心论点是「framework 已经变成 commercial product 的 free tier」，其引用的 r/LangChain 线索本文未复核，仅作为「2026 年仍有持续质疑声」的信号，不作事实采信。（来源 24）【存疑】
4. **流传数字不采信**：搜索结果里有「45% of developers never use LangChain in production」一类标题，未找到一手方法论与样本说明，本文不引用其数值。（来源 25）【存疑】
5. **下载量的天花板**：170M/月是「安装即计数」，CI、镜像、依赖树连锁拉取占比未知，与「活跃开发者数」不可互换。（来源 19）【推断】
6. **官方时间线并非无误**：Deep Agents 条目标的 v0.5.3 是 deepagents 包版本、与 langchain 本体版本线混淆易读错。（来源 3）【事实】

## 结论

把考古结果收成一句话：**LangChain 三年内把自己写的高层抽象废掉了两轮**——2024-10 官方亲手把大部分 chains 与 agents 标为 deprecated（来源 3），2025-10 的 1.0 只留一个 `create_agent`，而它还是从 LangGraph 搬回来的老抽象（来源 3/5）。这不是反复无常的黑历史，更像一个pattern 用了三年才稳定下来的实录：CEO 在 2024 年就承认「abstracted away too much」（来源 13），Anthropic 同期提醒「simple, composable patterns rather than complex frameworks」（来源 15），两边其实是同一句话的正反面。【观点】

对做基建的团队，三个可抄的动作：①**把带日期的 History 时间线直接写进官方文档**，包括「我们把什么标了 deprecated」——自曝式透明比第三方考古体面得多（来源 3）；②**批评帖里 CEO 亲自下场、先认账再给路线图**，480 分的负面热帖被回成了产品发布公告（来源 13）；③**旧抽象整体打包成 `langchain-classic` 而不是硬删**，给百万级存量项目一条不用当天迁移的路（来源 5/12）。【观点】

一句清醒剂给同行：**在 pattern 稳定之前，高层抽象不是资产是负债**——LangChain 用 148,045 星和 170,457,766 次月下载证明了另一件事：分发靠的是先占心智，抽象对不对，得等 1.0 这种「敢承诺不再大改」的版本出来才算数。（来源 17/19）【观点】

---

**来源清单**（全部取证于 2026-10-19，UTC+8；通道A 服务端抓取 + 通道B/C 搜索与交叉验证 + 通道D 截图，取证档案 `research/evidence/langchain/` 102 文件，manifest 带 sha256）：

1. https://www.langchain.com/ — 官网首页（标题 own your intelligence、LangSmith Engine 首屏、客户 logo 墙、导航结构）
2. https://langchain.ai/ — 素材库目标 URL（通道A 530/CF 1016、通道C TLS 断，本文存疑项）
3. https://docs.langchain.com/oss/python/langchain/philosophy — 官方 Philosophy + History 时间线（2022-10-24 v0.0.1 → 2026-03-15 Deep Agents 全部带日期，通道D innerText 归档）
4. https://docs.langchain.com/oss/python/releases/langchain-v1.md — What's new in LangChain v1（create_agent、middleware、content_blocks、langchain-classic 拆包）
5. https://docs.langchain.com/oss/python/migrate/langchain-v1.md — v1 迁移指南（LLMChain/ConversationChain → langchain-classic 的对照表）
6. https://docs.langchain.com/oss/python/versioning.md — 语义化版本与 API 稳定性分级（stable/beta/alpha/deprecated/internal）
7. https://www.langchain.com/blog/langchain-expression-language — LCEL 官宣（2023-08-01，batch/async/streaming 开箱即用）
8. https://www.langchain.com/blog/langchain-v0-1-0 — LangChain 0.1.0 官宣（2024-01-08，first stable version）
9. https://www.langchain.com/blog/langgraph — LangGraph 官宣（2024-01-17，stateful、multi-actor、cyclical workflows）
10. https://www.langchain.com/blog/langgraph-cloud — LangGraph v0.1 & Cloud（2024-06-27，官方承认高层抽象藏细节、legacy AgentExecutor 不透明）
11. https://www.langchain.com/blog/langchain-langgraph-1dot0 — LangChain 1.0 & LangGraph 1.0 官宣（2025-10-22，no breaking changes until 2.0；官方自陈「abstractions were sometimes too heavy, the package surface area had grown unwieldy」、create_agent 建于 LangGraph 运行时）
12. https://www.langchain.com/blog/three-years-langchain — Harrison Chase《Reflections on Three Years》（2025-10-20：$125M @ $1.25B、over a million developers、80M monthly downloads、LangGraph higher learning curve、langchain-classic 兼容承诺）
13. https://news.ycombinator.com/item?id=40739982 — HN 论战帖（2024-06-20，480 points / 297 comments；含 CEO hwchase17 2024-06-21 认账评论全文与截图）
14. https://fabianboth.dev/blog/why-we-no-longer-use-langchain-for-building-our-ai-agents/ — Octomind/Fabian Both 批评原文（12 个月生产使用、LCEL 三抽象对照、抽象反噬）
15. https://www.anthropic.com/engineering/building-effective-agents — Anthropic《Building Effective AI Agents》（2024-12-19：simple, composable patterns rather than complex frameworks）
16. https://github.com/humanlayer/12-factor-agents — 12-factor-agents（创建 2025-03-30、26,764 星实抓；「I don't see a lot of frameworks in production customer-facing agents」）
17. https://api.github.com/repos/langchain-ai/langchain — GitHub API（148,045 星 / 24,852 forks / 657 open issues 含 PR / MIT / created 2022-10-17；HTML 页 542 issues + 115 PRs 交叉）
18. https://api.github.com/repos/langchain-ai/langgraph — GitHub API（langgraph 43,469 星、created 2023-08-09；langchainjs 18,279 星同法实抓）
19. https://pypi.org/project/langchain/ — PyPI 与 pypistats（langchain 1.4.6 发版 2026-10-14、core 1.6.12 发版 2026-10-16、1.0.0 上传 2025-10-17、0.3.0 2024-09-13、0.2.0 2024-05-17；月下载 170,457,766、langgraph 44,769,952、langchain-classic 1.0.8）
20. https://api.npmjs.org/downloads/point/last-month/langchain — npm 注册表（langchain JS 包 12,760,965 次/月，2026-09-17→10-16）
21. https://docs.langchain.com/ — 新文档站首页（The open agent engineering ecosystem、Build/Test/Deploy/Monitor/Govern 产品地图）
22. https://www.langchain.com/blog/own-your-intelligence — Harrison Chase《Own Your Intelligence》（2026-07-25，未来五年每家公司用 AI 的两种方式）
23. https://www.langchain.com/blog — 博客索引（2026 年在售产品线与更新节奏）
24. https://www.roborhythms.com/langchain-losing-developers-2026/ — 第三方媒体评论（2026-04-02，二手来源，文中已标存疑不作事实采信）
25. search_anywhere 检索快照（DDG 三组查询，通道③本地DDG，归档于 `research/evidence/langchain/search_criticism.full.txt`）——用于记录「45% 开发者」类流传数字的检索面

**勘误**：本文任何错误可在 javis.im 留言指出，核实后修订；若结论被推翻，本文将标注撤回。文中【事实】/【推断】/【观点】/【存疑】四色标注即为可信度分层，读者可只采信【事实】部分。官方时间线与 PyPI/博客/仓库的日期口径差、第三方媒体的漏斗质疑、流传的无源统计数字，均已随文列出而不是抹掉。

> 本站为独立技术分析，与文中项目无商业关系。
