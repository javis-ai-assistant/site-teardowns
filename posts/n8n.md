> **首发 [https://javis.im/analysis/n8n/](https://javis.im/analysis/n8n/)，本文随站点同步更新**；文中图片见在线原页，英文版：https://javis.im/analysis/n8n/index-en

# 自动化老树开新花：n8n 手握 208,124 星、7 个月从 US$2.5B 翻到 US$5.2B，官方文档却写着「我们不称自己为开源」

> 手握 208,124 星、7 个月把定价从 $2.5B 抬到 $5.2B，官方文档却写着「我们不称自己为开源」。本文按商业画布拆执行计费、fair-code 许可与 SAP 这一跳，附同一天四个集成数的对账。

取证日期：2026-10-20（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 20 · 可验证性 19 · 独特性 17 · 受众相关度 18 · 多维空间 18 = **92/100**（≥60 才动笔；素材库预分 80）。

## 这个站在做什么

取证日官网首屏一句话：**Build the agent. Understand its workflow**，正文是「n8n is the platform for agents and workflows you can follow. Open any step, see what it does, and explain to anyone who runs it.」——卖点不是「全自动」，而是**每一步都能打开给人看**。（来源 1）【事实】页面下方的集成口径写「**500+ native integrations. Any API via HTTP. Any agent or coding assistant via MCP**」，并挂着「Trusted by Fortune 500 companies」。（来源 1）【事实】

产品形态是可视化画布 + 代码节点：文档把集成单元叫 **nodes**，内置节点之外可装社区节点，接不到的服务用 HTTP Request 节点补，另有 credential-only 节点与 MCP registry。（来源 23）【事实】定价页把计费单位讲得很直白：**一次执行 = 整条工作流从头跑一遍**，不按步数、不按人头，原话是「n8n workflows you can do things in a single execution that would take 10,000 operations in other tools」。（来源 2）【事实】

![n8n 官网首页首屏：Build the agent. Understand its workflow + 500+ native integrations，取证 2026-10-20](img/home.png)

## 商业画布 · 客户与渠道：170 万搭建者、1,400 家企业，再加一个 SAP

客户分层在官方口径里是两截：**社区侧 1.7 million monthly active builders**，**企业侧 more than 1,400 enterprise customers, including Fortune 500 teams**——这组数出自 2026-05-12 SAP 战投官宣博文。（来源 18）【事实】企业页另给一个更粗的刻度：「more than a third of F500 companies」。（来源 12）【事实】

渠道在这条线上是**分层的**：自助注册的 Cloud 免费试用（1,000 次执行、5 并发、180 秒超时）负责漏斗顶部，Sales assisted procurement 负责 Enterprise 档；案例页给出四个署名样本来证明中大型买家真在用——Vodafone「£2.2M saved on threat intelligence」、Trendyol「700 active workflows in under a year」、Stepstone「From 4 early adopters to 3,000 employees」、Huel CTO「n8n was the big unlock」。（来源 13/2）【事实】

![n8n 企业页：AI you can trust at scale + 500+ integrations + 三分之一 F500，取证 2026-10-20](img/enterprise.png)

2026-05-12 之后多了一条**别人替它卖**的渠道：SAP 战投与多年期商业协议，把 n8n 画布原生嵌进 Joule Studio；新闻稿口径是「a platform born in the open-source community becomes a key AI workflow tool for one of the world's largest enterprises」。（来源 19/18）【事实】SAP 自己的筹码写在同一份稿子里：Christian Klein 说「By integrating n8n into Joule Studio, we're accelerating SAP's ability to help customers design, connect, and scale agentic AI across their core business processes」。（来源 19）【事实】n8n 侧补了一句背景数据：SAP 客户覆盖「99 of the 100 largest companies in the world」。（来源 18）【事实】

## 商业画布 · 收入：按「整条流程跑一遍」计费的四档价目

取证日定价页四档原样（年付月价）：**Starter 20€**（2.5K 次执行）、**Pro 50€**（10K 次执行）、**Business 667€**（40K 次执行、**self-hosted only**）、**Enterprise 联系销售**（自定义执行量）。（来源 2）【事实】页头挂着差异化主张「**If it doesn't run, you don't pay**」——「Unlike other tools that charge per step or user, n8n lets you build freely and only pay when a workflow runs from start to finish.」（来源 2）【事实】另有 **Start-up Plan**：员工少于 20 人的公司可拿 Business 半价。（来源 2）【事实】

![n8n 定价页：Starter 20€ / Pro 50€ / Business 667€ / Enterprise + Community Edition 星数 208,119，取证 2026-10-20](img/pricing.png)

真正的收入杠杆藏在能力表里：**免费层与付费层的分界不是功能而是留痕与治理**——Starter/Pro 的 workflow history 是 1 天/5 天，Enterprise 是 365 天+；并发 5 → 20/50 → 200+；单次执行时长 5 分钟 → 40 分钟 → 40 分钟；SSO/LDAP、审计日志、外部密钥库、环境与 Git 版本控制全部只在 Enterprise 列打勾。（来源 2）【事实】也就是说**自托管的社区版能干活，但不能留档、不能过合规**，这正是企业档的定价理由。（来源 2/21）【推断】

收入规模没有官方数字。能查到的只有两类：官方在 Series C 博文里自述 2025 年「**6x user growth, 10x revenue growth**」，没有基数、没有审计【存疑】（来源 17）；第三方 Sacra 估计「n8n hit $100M in annual recurring revenue (ARR) in April 2026, up from $70M at the end of 2025」，属估算口径【存疑】。（来源 30）

## 商业画布 · 核心资源：208,124 星、13,641 个模板、2.74 亿次镜像拉取

GitHub API 取证日实抓 `n8n-io/n8n`：**208,124 星**、61,194 forks、`open_issues_count` 1,193、`subscribers_count` 1,190、仓库创建 2019-06-22、最后一次 push 2026-10-20T13:44Z、许可证字段 `NOASSERTION`。（来源 3）【事实】语言字节构成 **TypeScript 142,324,245 / 总 154,297,599 = 92.24%**，其后 Vue 6.38%、JavaScript 0.71%——一个 TypeScript 单体加上 Vue 的画布前端。（来源 5）【事实】

![GitHub 仓库页实抓：Star 208.1k、README「1500+ integrations and 9,000+ workflow templates」，取证 2026-10-20](img/github.png)

分发侧另有两条硬数：npm 包 `n8n` 最近一个自然月 **467,680 次下载**（2026-09-19→2026-10-18），过去 383 天累计 **4,548,989 次**，其中 2026-08 单月 567,348 次是全期峰值。（来源 8/9）【事实】Docker Hub 官方镜像 `n8nio/n8n` 累计拉取 **274,104,384 次**（约 2.74 亿），`last_updated` 2026-10-20T09:19Z。（来源 10）【事实】

社区资产在模板市场里：取证日模板页写着「**13,641 Workflow Automation Templates**」，并挂着 affiliate program 招募创作者。（来源 11）【事实】发布节奏同日可查：GitHub releases 最新 `n8n@2.45.0`（2026-10-20T09:10:14Z），npm `latest` 还停在 **2.44.4**、`engines.node >=24.0.0`、`license` 字段写作「SEE LICENSE IN LICENSE.md」。（来源 6/7）【事实】——同一天两个包管理面差一个补丁版本，属正常的发布窗口差【推断】。

![n8n 模板市场：13,641 个社区工作流模板 + affiliate program，取证 2026-10-20](img/workflows.png)

## 商业画布 · 地基：fair-code 许可与「我们不是开源」

这门生意最硬的一块砖是许可。GitHub 仓库根目录 `LICENSE.md` 写明：**文件名含 `.ee.` 或目录含 `.ee` 的源码不受 Sustainable Use License 覆盖，必须持有 n8n Enterprise License**；其余部分在 Sustainable Use License 1.0 之下，核心限制只有一句——「You may use or modify the software only for your own internal business purposes or for non-commercial or personal use.」（来源 20）【事实】

官方文档把话说得比社区还直白：「Although n8n's source code is available under the Sustainable Use License, according to the Open Source Initiative (OSI), open source licenses can't include limitations on use, **so we do not call ourselves open source**.」（来源 21）【事实】License FAQ 补了三个限制条款与迁移史：2022-03-17 之前是 **Apache 2.0 with Commons Clause**，因为「wording that's open to interpretation, created some confusion」以及该条款会限制咨询与支持服务，才换成自创的 fair-code。（来源 22/21）【事实】

![n8n 官方 License FAQ：为什么不使用开源许可、从 Apache 2.0 with Commons Clause 迁移的自述，取证 2026-10-20](img/licfaq.png)

**许可争议从第一天就存在**。2019-10-08 n8n 首登 HN，728 points / 196 comments，创始人自己的顶楼把它称作「a free node-based "Open Source" (Apache 2.0 with Commons Clause) Workflow Automation Tool」，评论区立刻有人指出 Commons Clause 官方 FAQ 自己写着「Is this "Open Source"? No.」。（来源 27）【事实】六年后同一场架还在打：2025-10-09《N8n raises $180M》帖 235 points / 184 comments 里，有人贴出「It's not open source if you can't use it professionally or sell work derived from it」，也有人回「Source-available would be a better descriptor」。（来源 24/25）【事实】

![Hacker News《N8n raises $180M》（235 points / 184 comments）：自托管执行数限额的顶楼吐槽，取证 2026-10-20](img/hn.png)

论战之外还有一条更疼的**产品侧抱怨**，同样来自这篇融资帖顶楼：自托管用户买了服务器、负载均衡、KV、数据库，却仍受执行数配额约束——「even their highest enterprise license was cutting it close, and if you 'run out' of these executions, the service just stops working」。（来源 24）【事实】官方的定价叙事对此有直接回应：一次执行=整条流程跑完，按「流程数」而不是「步骤数」收钱，所以同等复杂度下比按 task/operation 计费的平台便宜。（来源 2/30）【事实】两边都是各自立场的一手表述，本文不判谁对【观点】。

## 商业画布 · 重要伙伴：Accel、NVIDIA，然后是 SAP

融资线两跳，全部有官方出处：2025-10-09 Series C **$180M**、总融资 **$240M**、估值 **$2.5B**，Accel 领投，Meritech、Redpoint、Evantic、Visionaries Club 跟投，企业投资人 **NVentures（NVIDIA 风投部门）**与 T.Capital 加入，Felicis、Sequoia、Highland Europe、HV Capital 跟投。（来源 17）【事实】215 天后的 2026-05-12，SAP 战投把估值顶到 **$5.2bn**，官方原话是「more than double our valuation from less than a year ago」，并伴随「embedding n8n natively inside SAP's Joule Studio」。（来源 18/19）【事实】2.08 倍、七个月，中间没有新一轮融资见诸官方公告【事实】【推断】。

![n8n 官方 SAP 战投博文（2026-05-12）：$5.2bn、1.7M monthly active builders、1,400+ enterprise customers，取证 2026-10-20](img/sap.png)

第三方口径必须分开放：Sacra 称「SAP invested more than EUR 60M in n8n in May 2026」，金额不见于 n8n 与 SAP 的官方稿【存疑】；Tracxn 口径是累计融资 $254M/5 轮，与官方 $240M 不同源【存疑】。（来源 30/17）【存疑】收入侧 Sacra 的 $70M→$100M 同样只按第三方估算采信，本文不与官方「10x revenue growth」连线【存疑】。

## 口径对账：同一天，四个「集成数」

本篇最值得抄走的动作，是把口径打架的数字全部摆出来：

- **集成数（同日四口径）**：GitHub About「**400+ integrations**」（来源 3/4）｜官网首页与企业页「**500+**」（来源 1/12）｜SAP 新闻稿「**over 1,000 integrations**」（来源 19）｜仓库 README/wiki「**1500+ integrations**」（来源 4）
- **模板数（两口径）**：README「**9,000+ workflow templates**」（来源 4）｜模板页实时计数「**13,641**」（来源 11）
- **星数（三处显示）**：GitHub API **208,124**（来源 3）｜定价页 Community Edition 徽章 **208,119**（来源 2）｜博客页脚「Github ★ 207k」（来源 17）

【推断】这不是谁在撒谎：About 描述是注册时写死的文案，README 是发版时快照，官网徽章是缓存，SAP 稿里的「1,000+」大概率含社区节点与 credential-only 连接——**四个数各自都对，合起来说明这类指标没有审计口径**。对做对照的同行，唯一稳的做法是标注来源与时点（本文全部标注为 2026-10-20 实抓）。

## 风险与存疑清单

- **自托管配额是最集中的民怨**：融资帖顶楼把「自己出硬件、执行数用尽即停服务」写成放弃理由，评论区顺势类比 Oracle 的按核授权【事实】；对计划把后端整个搬上 n8n 的团队，这是签合同前必须算清的账【推断】。（来源 24）
- **许可证不是 OSI 开源**，`.ee` 文件另册、CLA 允许公司单方面改许可，官方自己也这么说【事实】——「按开源项目评估 n8n」的任何清单都需要改口径。（来源 20/21）
- **AI 工作流的安全面靠用户自觉**：HN 195 points 帖的首楼是 prompt injection 实例——「Nearly all workflows built using n8n that I've seen face some kind of prompt injection vulnerability」，因为外部数据被直拼进 system prompt；这不是 n8n 独有，但平台的普及度放大了它。（来源 26）【事实】
- **企业级成熟度仍有差评**：同一帖里组织用户吐槽自定义 app 要手工塞进容器 `.npm` 目录、SSO「lackluster」，并给出「expect to do some tinkering if you go beyond what it does out of the box」的结论。（来源 26）【事实】
- **自办报告的中立性**：官网 llms.txt 把《Workflow-based AI Agent Development Tools (2026)》列为头条，页面写「conducted by independent research analyst Andrew Green in Q2 2026」，受评 14 家里含 n8n，页内出现「n8n wins」；报告由被评方托管并分发【存疑】。（来源 16/15）【存疑】
- **性能/规模类主张全部自报**：「6x user growth, 10x revenue growth」「1.7M monthly active builders」均无统计方法与审计【存疑】。（来源 17/18）
- **对照组的一个坑**：检索到的《N8n vs. Windmill vs. Temporal》对比文（33 points）被 HN 读者当场指出满页死链与模板腔，判为 AI 批量生成——**同类横评的站外数字先验可信度要下调**。（来源 28）【事实】
- **收入黑箱**：四档价目、SAP 协议、$240M 融资都可查，收入规模只有第三方估算，本文不做连线。（来源 2/30）

## 结论

把画布收成一句话：**n8n 用「一次执行 = 整条流程」的计费单位把定价锚在产出而不是人头，用 fair-code 许可把 208,124 星的免费分发留在手里，再用 SAP 这一跳把估值从 25 亿抬到 52 亿美元**——三件事互相咬合：没有开源式的分发就没有 1.7M 搭建者，没有企业档的治理能力表就卖不进 1,400 家企业，没有 `.ee` 另册就守不住定价。（来源 3/17/18/2/20）【推断】

给做工具的团队三个可抄的动作：①**把计费单位讲成产品主张**——「If it doesn't run, you don't pay」+ 与「10,000 operations」的对照，把计费差异变成可复述的一句话（来源 2）【观点】；②**主动公开「我不是开源」**——官方文档第一句就把 OSI 定义摆出来，比被社区拆穿再解释成本低得多（来源 21）【观点】；③**给 AI 时代留机器可读入口**——`llms.txt` 指向自家报告的 Markdown 版，是把 AI 检索当成分发渠道的早期动作（来源 15）【观点】。

一句清醒剂给同行：**当四个集成数口径同天并存、而每一方都能自证时，「规模」在证据层面不是一个数，是一个区间**——引用它必须带来源与时点，否则自己就成了那篇被 HN 判死的 AI 对比文。（来源 4/19/28）【观点】

---

**来源清单**（全部取证于 2026-10-20，UTC+8；通道A 服务端抓取 + 通道B 搜索交叉验证 + 通道D 截图，取证档案 `research/evidence/n8n/`，92 文件，manifest 带 sha256）：

1. https://n8n.io/ — 官网首页首屏（Build the agent. Understand its workflow / 500+ native integrations / Any agent or coding assistant via MCP / Trusted by Fortune 500）
2. https://n8n.io/pricing/ — 四档年付价（Starter 20€ 2.5K 执行 / Pro 50€ 10K / Business 667€ 40K self-hosted only / Enterprise）、If it doesn't run you don't pay、executions 定义、Community Edition 徽章 208,119、Start-up Plan 半价、试用与能力表（history/concurrency/SSO/审计分档）
3. https://api.github.com/repos/n8n-io/n8n — 208,124 星 / 61,194 forks / open_issues 1,193 / subscribers 1,190 / created 2019-06-22 / pushed 2026-10-20T13:44:33Z / license NOASSERTION / About 描述「400+ integrations」
4. https://github.com/n8n-io/n8n — 仓库页实抓（About 400+ / README「1500+ integrations and 9,000+ workflow templates」「🔧 1500+ Integrations」）
5. https://api.github.com/repos/n8n-io/n8n/languages — TypeScript 142,324,245 / Vue 9,835,828 / JavaScript 1,096,652 / 总 154,297,599
6. https://api.github.com/repos/n8n-io/n8n/releases — 最新 n8n@2.45.0（2026-10-20T09:10:14Z）及同刻 beta/stable 标签
7. https://registry.npmjs.org/n8n/latest — version 2.44.4、license「SEE LICENSE IN LICENSE.md」、engines.node >=24.0.0
8. https://api.npmjs.org/downloads/point/last-month/n8n — 467,680（2026-09-19→2026-10-18）
9. https://api.npmjs.org/downloads/range/2025-10-01:2026-10-18/n8n — 383 天累计 4,548,989；月度峰值 2026-08 = 567,348
10. https://hub.docker.com/v2/repositories/n8nio/n8n/ — pull_count 274,104,384、description「Free and open fair-code licensed…」、last_updated 2026-10-20T09:19:45Z
11. https://n8n.io/workflows/ — 13,641 Workflow Automation Templates、Verified Creators、affiliate program
12. https://n8n.io/enterprise/ — AI you can trust at scale、500+ integrations、more than a third of F500、shadow AI 引 IBM Cost of a Data Breach Report 2026
13. https://n8n.io/case-studies/ — Vodafone £2.2M saved、Trendyol 700 workflows、Stepstone 4→3,000 employees、Huel「n8n was the big unlock」
14. https://n8n.io/careers/ — remote-first、AI-native company、Culture/Code、六大团队描述
15. https://n8n.io/llms.txt — 机器可读入口，头条为 2026/2025 两版 AI agent 工具报告（含 Markdown 版）
16. https://n8n.io/reports/2026-ai-agent-development-tools/ — 14 家工具、independent research analyst Andrew Green、Q2 2026、页内「n8n wins」
17. https://blog.n8n.io/series-c/ — 2025-10-09：$180M Series C / $240M 总额 / $2.5B / Accel 领投 / NVentures / 6x 用户 10x 收入 / United Nations 与 tinkerers 并列
18. https://blog.n8n.io/n8n-sap/ — 2026-05-12：$5.2bn、1.7M monthly active builders、1,400+ enterprise customers、Joule Studio 嵌入、99 of the 100 largest companies
19. https://www.prnewswire.com/news-releases/n8n-valuation-doubles-to-5-2bn-as-sap-makes-strategic-investment-and-plans-to-embed-the-ai-platform-into-joule-studio-302767222.html — SAP 新闻稿：multi-year commercial agreement、Christian Klein 引语、over 1,000 integrations、$5.2bn vs $2.5bn
20. https://raw.githubusercontent.com/n8n-io/n8n/master/LICENSE.md — Sustainable Use License 1.0 全文、`.ee` 文件需 Enterprise License、内部商业用途限制
21. https://docs.n8n.io/n8n-community-license — 「we do not call ourselves open source」（OSI 定义）、三限制条款、CLA 与改许可权
22. https://blog.n8n.io/announcing-new-sustainable-use-license/ — fair-code 由来、Apache 2.0 with Commons Clause 的退出理由
23. https://docs.n8n.io/integrations/ — nodes 概念、社区节点、credential-only、HTTP Request、MCP registry、Last updated 7 days ago
24. https://news.ycombinator.com/item?id=45525336 —《N8n raises $180M》235 points / 184 comments（2025-10-09）：自托管执行数配额顶楼吐槽
25. https://news.ycombinator.com/item?id=45526463 — 两年体验长评与开源/license 论战（It's absolutely not open source / Source-available would be a better descriptor）
26. https://news.ycombinator.com/item?id=43879282 —《N8n – Flexible AI workflow automation for technical teams》195 points / 99 comments（2025-05-03）：prompt injection 首楼、custom app 与 SSO 吐槽、Windmill 创始人回帖
27. https://news.ycombinator.com/item?id=21191676 — 2019 首发帖 728 points / 196 comments：创始人「Open Source」(Apache 2.0 with Commons Clause) 自述与 Commons Clause FAQ 对撞
28. https://news.ycombinator.com/item?id=45534012 —《N8n vs. Windmill vs. Temporal》33 points：评论区判定该对比文为 AI 批量生成（满页死链）
29. https://news.ycombinator.com/item?id=37274764 — 2023 体验帖：「It's not open source, it's source available with commercial restrictions」
30. https://sacra.com/c/n8n/ — 第三方估算：ARR $100M（2026-04，前值 2025 年底 $70M）、SAP 投资 >EUR 60M、Monetization 三通道（Cloud 订阅/自托管授权/OEM 嵌入）

**勘误**：本文任何错误可在 javis.im 留言指出，核实后修订；若结论被推翻，本文将标注撤回。文中【事实】/【推断】/【观点】/【存疑】四色标注即为可信度分层，读者可只采信【事实】部分。官方自报的 6x/10x 与 1.7M 搭建者、第三方 Sacra 的 ARR 与 SAP 金额、四个集成数口径与三个模板数口径、星数 208,119/208,124/207k 的三处显示差、npm 与 GitHub 的补丁版本差，均已随文列出而不是抹掉。

> 本站为独立技术分析，与文中项目无商业关系。
