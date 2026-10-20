> **首发 [https://javis.im/analysis/braintrust/](https://javis.im/analysis/braintrust/)，本文随站点同步更新**；文中图片见在线原页，英文版：https://javis.im/analysis/braintrust/index-en

# 主仓 29 星、月下载 689 万次：$80M B 轮、第三方记作 $800M 的 Braintrust，开源外壳里包着自研 Rust 数据库，旗舰 proxy 还被自己标了 deprecated

> **利益冲突披露**：本文作者所在团队运营 AI 网关与助手产品，与 Braintrust 不构成直接竞争，但同处「AI 工具」舆论场，评「AI 可观测性平台」时天然带立场；另外 Braintrust 官方文档里就收录着作者团队常用 agent 底座的集成页（Pi Coding Agent SDK），本文对该页只作客观引用。全篇文章只对手里这 37 个来源负责：官方有利与不利证据同时列出，每个判断句背后都有可点开的来源，口径不一致的数字全部列出而不强行连线。

取证日期：2026-10-21（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 20 · 可验证性 19 · 独特性 17 · 受众相关度 16 · 多维空间 17 = **89/100**（≥60 才动笔；素材库预分 78）。

## 这个站在做什么

取证日官网首屏一句话：**The active observability platform for agents**，正文是「Ship quality agents at scale. Discover patterns in production, turn them into evals, and improve quality with every release.」——卖点不是「帮你写 AI」，而是**给已经在跑的 AI 留证据、打分、抓回归**。（来源 1）【事实】产品面分三根柱子：Observe（Trace everything）、Evaluate（Test what ships）、Discover（Find behaviors），外加 Docs / Blog / Academy / Encyclopedia 一整套 eval 教育内容。（来源 1）【事实】

身世有据可查：2023 年 8 月由 Ankur Goyal 创立（Impira 联合创始人、前 Figma ML 平台负责人），公司初期由投资人 Elad Gil 孵化。（来源 7）【事实】2023-09-28 TechCrunch 报道时它还是个四人公司，Goyal 自比「operating system for engineers building AI software」，当轮 $3M，投资方名单里有 Adam D'Angelo、Clem Delangue。（来源 8）【事实】三年后的 2026-02-25，它在旧金山加州科学院办起首届用户大会 Trace，500 人规模。（来源 7）【事实】

![Braintrust 官网首页首屏：The active observability platform for agents + Ship quality agents at scale，取证 2026-10-21](img/home.png)

## 技术栈考古 · 外圈：108 个公开仓、2,365 星，四成五押在一个评测库上

GitHub API 取证日实抓（org 全量分页 + search 交叉）：**braintrustdata 组织共 108 个公开仓，其中 78 个非 fork，星数合计 2,365**；非 fork 语言分布 Python 22 仓、TypeScript 19、Rust 7、Go 3。（来源 17/21）【事实】星数结构极度头重：**autoevals 1,063 星**（MIT/Python，2023-07-11 建仓）一家占全组织约 **45%**，其后 braintrust-proxy 412、agentbehavior 360、cookbook 62——再往后 SDK 主仓 **braintrust-sdk-javascript 只有 29 星**。（来源 18/19/17/20）【事实】

![GitHub 仓库页实抓：braintrustdata/autoevals 1,063 星，取证 2026-10-21](img/github.png)

外圈的许可是真开源：autoevals 与 proxy 用 **MIT**，SDK 三件套用 **Apache-2.0**，另有 lingua——一个「在各家 LLM API 格式之间做零开销翻译」的 Rust 库（28 星，2025-08-30 建仓）。（来源 18/19/20/38）【事实】但把 108 个仓摆在一起看，**开源物全在客户端、评测器、格式转换这些「围绕平台」的位置上；平台本体一个仓都没有**。（来源 17/21）【推断】

## 技术栈考古 · 分发：npm+PyPI 月下载 1,346 万次，主仓 29 星

这是全篇最反常识的一组数。npm 包 `braintrust` 最近一个自然月下载 **6,889,964 次**（2026-09-19→2026-10-18）；PyPI 包 `braintrust` 月下载 **6,574,708**、单日 298,474。（来源 22/26）【事实】两端合计**约 1,346 万次/月**。（来源 22/26）【推断】拉长看更陡：npm 385 天累计 **44,242,439** 次，月度曲线 2026-03 的 2,836,340 → 2026-09 的 7,104,136，**6 个月翻了 2.5 倍**。（来源 23）【事实】

分发的另一半是 autoevals：npm 月下载 **2,438,756**。（来源 24）【事实】包本体也没藏：npm registry 显示 `braintrust` 创建于 **2023-07-09**（首发 0.0.1），最新版 **3.38.0** 发布于 2026-10-19，license MIT，repository 指向 `braintrust-sdk-javascript`。（来源 25）【事实】而这个仓的星数是 **29**，Python SDK 仓 **22**。（来源 20/17）【事实】

【推断】下载量来自 lockfile、CI 和企业内网的自动安装——那是机器行为；星数是人抬手点一下——那是社区行为。**1,346 万比 29，说明它的分发根本不在 GitHub 社区里**，把「星数」当开源项目体温计量到这家公司身上会直接误诊。

## 技术栈考古 · 内核：控制面/数据面分离，加一个自研 Rust 数据库

闭源内核有两层。第一层是**部署拓扑**：官方架构页写明「Braintrust separates the control plane from the data plane」——控制面（UI、认证、元数据）跑在 Braintrust 托管 SaaS，**敏感 AI 数据全在数据面**，可放 Braintrust 托管、也可放你自己的云（BYOC/自托管），自托管数据面组件为 API service、PostgreSQL、Redis、object storage、Brainstore；用 SDK 时数据**直连你的数据面**。（来源 28）【事实】安全页给出合规背书：SOC 2 Type II、HIPAA、GDPR，Trust center 统一披露。（来源 35）【事实】

![Braintrust 架构页：控制面/数据面分离与数据面组件清单，取证 2026-10-21](img/arch.png)

第二层是**Brainstore 自研数据库**。官方基准自述：同负载 390 万条 traces 对比「a leading competitor」，全文检索 **23.9×**（9,587ms → 401ms）、写延迟 **2.55×**（17,775ms → 6,984ms）、span 加载 **3.73×**（1,290ms → 346ms）。（来源 29）【事实】——但**对手未具名、测试由被评方自己跑**，这组数按自办口径采信，横评场景一律标存疑。（来源 29）【存疑】

![Brainstore 基准博客：23.9x 全文检索对比（对手未具名），取证 2026-10-21](img/benchmarks.png)

产品侧的演化也留了痕：月度 changelog 连续归档、导航列表最远回溯到 2023 年中；2026-10 的条目里已经在上 Patterns 严重度分级、PagerDuty 通知、coding agent insights、SQL charts、Loop（平台内置调查 AI agent）——**changelog 里一半条目要求 data plane v2.x+，版本号本身成了给自托管客户的技术栈年轮**。（来源 34）【事实】集成面官方页面计数 **All (88)** = AI providers 26 + SDK integrations 50（含 27 个 agent framework，含 Pi Coding Agent SDK）+ Developer tools 11 + runtime 1，与 `llms.txt` 逐页核算一致。（来源 33/39）【事实】

【推断】BYOC 把「数据主权顾虑」变成 Enterprise 档的定价理由；Brainstore 把「可观测性数据大到查不动」变成技术叙事——**内核的两层各自对应一条卖点**，这不是巧合而是产品设计。

## 演化痕迹：旗舰 proxy 的退役路线

技术栈考古最见功力的地方是看弃用。取证日文档《Use the proxy (deprecated)》原文：「**The AI proxy is deprecated and will no longer be regularly maintained. Use the gateway instead**…For Braintrust-hosted organizations, requests to `https://api.braintrust.dev/v1/proxy` now route to the gateway. Existing integrations continue to work with no code or URL changes.」（来源 31）【事实】——**先宣布不再维护，再把流量无感切到自家网关，老集成一行不改**。（来源 31）【事实】

并列口径：`braintrust-proxy` 仓库同日仍在推送（2026-10-20，412 星，MIT），README 还挂着「unified API / automatic caching / observability『Coming soon』」的原始承诺。（来源 19/32）【事实】文档说不再维护、仓库还在动，两者不矛盾——安全补丁与功能维护是两回事——但引用时必须两句一起给。（来源 19/31）【推断】

![文档《Use the proxy (deprecated)》：Deprecated 原文 + gateway 迁移说明，取证 2026-10-21](img/proxydep.png)

## 钱 · 三档价目与四段资本口径

取证日定价页三档（均为平台费，用量另计）：**Starter $0**（$10 模型额度、1GB 处理量+$4/GB、10K scores+$2.50/1k、**14 天留存**、人工标注分 1/项目）；**Pro $249/月**（$100 额度、5GB+$3/GB、50K scores+$1.50/1k、30 天留存+超期 $0.50/GB/月，含 Loop agent、RBAC、environments、custom charts）；**Enterprise 自定义**（BYOC/自托管、自定义留存、S3 导出）。（来源 2/36）【事实】三档共同点写得很直白：**users / projects / playgrounds / experiments / datasets 全部 Unlimited**，合格初创公司另有 6–12 个月免费。（来源 2）【事实】——**人头不限量，钱从数据量和打分次数上收**。（来源 2）【推断】

![Braintrust 定价页：Starter $0 / Pro $249 / Enterprise 三档，取证 2026-10-21](img/pricing.png)

融资四段各自有出处、互不连线：2023-09 TechCrunch 口径 **$3M**（四人公司当轮）。（来源 8）【事实】；2023-12 种子轮 **$5.1M**、Greylock 领投（Wikipedia 引注口径）。（来源 7）【事实】；2024-10-08 Series A **$36M**、a16z（Martin Casado）领投、**累计 $45M**，名单里出现 Greg Brockman、Arthur Mensch 以及 Datadog、Databricks Ventures，第三方 finsmes 补一句 **$150M post-money**。（来源 4/9）【事实/存疑】；2026-02-17 Series B **$80M**、ICONIQ 领投、a16z/Greylock/Elad Gil 复投，官方博文**通篇不写估值**，**$800M** 出自 SiliconANGLE 等第三方。（来源 3/11）【事实/存疑】累计融资 **$125M** 系 $45M+$80M 相加，官方未给这个数。（来源 4/3）【推断】

客户名单同步升级：Series A 博文点名 Notion、Stripe、Vercel、Airtable、Instacart、Zapier、Coda、The Browser Company；Series B 换成 Notion、Replit、Cloudflare、Ramp、Dropbox。（来源 4/3）【事实】Notion 案例页给出全页最硬的效率数：**70 位工程师 24 小时内对齐 evals、完成新前沿模型部署**（Sarah Sachs，AI Modeling Lead 署名）。（来源 6）【事实】收入规模**没有任何官方口径**，第三方估算本文不引【存疑】。

![Series B 官方博文：$80M、ICONIQ 领投、点名 Notion/Replit/Cloudflare/Ramp/Dropbox，取证 2026-10-21](img/seriesb.png)

## 口径对账：同一家公司，下载算一家、星数算另一家

- **下载 vs 星数（最核心的一组）**：npm 月 **6,889,964** + PyPI 月 **6,574,708** vs JS SDK 主仓 **29 星**、Python SDK 仓 **22 星**（来源 22/26/20/17）
- **npm 与 PyPI 两个「月」不同口径**：npm point 接口是 2026-09-19→10-18 定窗，pypistats 是滚动 30 天，相加只到「约 1,346 万/月」的量级，不是精确同窗（来源 22/26）【推断】
- **集成数**：官方页签 `All (88)` = 26+50+11+1，与 `llms.txt` 逐页一致——这次两个口径对上了（来源 33）
- **估值两段**：$150M post（2024-10，finsmes 第三方）与 $800M（2026-02，SiliconANGLE 第三方），**官方两篇融资博文均未写估值**（来源 9/11/3/4）
- **融资累计**：官方口径停在「total funding to $45 million」（2024-10），加 B 轮 $80M 得 $125M 属本文相加（来源 4/3）【推断】
- **基准**：23.9× 那组数的对照方是「a leading competitor」，全文未点名（来源 29）【存疑】
- **文档域名**：`docs.braintrust.dev` 取证日双通道 530/1016 不可达，正典文档在 `www.braintrust.dev/docs`——引用务必带 www（来源实测）【事实】

## 风险与存疑清单

- **卖质检的自己被打穿**：TechCrunch 2026-05-06 报道，Braintrust 确认一个 AWS 账户遭未授权访问，**该账户存的正是客户托管的模型 API key**，邮件要求「every customer to rotate」，官方称已锁定账户、审计关联系统、轮换内部密钥，暂未见更广暴露。（来源 12/13）【事实】——**质检平台顺带成了钥匙托管方**，这个 attack surface 是产品形态自带的。（来源 12）【推断】
- **社区口碑并不齐**：2023-09-28 Show HN 首发只有 **8 分 / 2 评论**（创始人 ankrgyl 自发）。（来源 14）【事实】2025-06-05 Ask HN《Everything feels half-baked》里有用户写「Braintrust: Interesting approach to evaluation but **feels like an early-stage product. Documentation is sparse** and integration options are limited」——与 88 个集成、大客户名单同框反差明显。（来源 15）【事实】
- **估值与收入全是第三方**：官方沉默，$800M、$150M post 都出自第三方转述，引用必须带媒体名与日期。（来源 9/11）【存疑】
- **proxy 双口径**：deprecated 文档与活跃仓库并存，只引一句都会失真。（来源 19/31）【事实】
- **基准自办**：Brainstore 对比不具名对手、自跑测试；拿它做选型依据前，先找独立复测。（来源 29）【存疑】
- **站外横评先验降权**：Langfuse/LangSmith/Braintrust 的第三方对比文（llmtools.cc，2026-07-26）给出「Braintrust = eval-first regression workhorse、起始付费档 $249/mo」的定位，可作市场共识参考，数字以官网当日为准。（来源 37）【存疑】

![Hacker News Show HN 原帖：8 points / 2 comments，取证 2026-10-21](img/hn.png)

![TechCrunch：AI evaluation startup Braintrust confirms breach, tells every customer to rotate sensitive keys，取证 2026-10-21](img/breach.png)

## 结论

把技术栈收成一句话：**Braintrust 是三层同心圆——最外层 108 个开源仓（MIT/Apache-2.0）负责获客与生态位，中间层 npm+PyPI 月下载 1,346 万的 SDK 负责钻进你 CI，最里层控制面/数据面分离 + 自研 Brainstore 负责收钱**——三层各干各的：没有外圈 autoevals 的 1,063 星就进不了评测工具的候选清单，没有千万级 SDK 下载就撑不起「88 个集成」的叙事，没有 BYOC 数据面就卖不进在乎数据主权的企业档。（来源 17/22/28/33）【推断】

给做工具的团队三个可抄的动作：①**下载量和星数分开记账**——机器装机量证明市占，星数证明社区，拿错指标会做错决策（来源 20/22）【观点】；②**弃用要留迁移话术模板**——「deprecated → gateway，流量我切，你一行不改」这句把退出成本降到零，比悄悄删库体面得多（来源 31）【观点】；③**文档同时给人和机器两个入口**——`llms.txt` 88 个集成索引 + 逐页 `Copy page`，AI 检索时代的分发位它先占了（来源 33）【观点】。

一句清醒剂（给同行，也给本文作者团队）：**一个把「给 AI 打分」当生意的公司，5 月刚因为存了客户的钥匙被要求全员轮换——能力越靠近核心资产，保管责任越重**；下次评估这类平台，先问的不是 benchmark 几倍，是它手里攥着你什么。（来源 12）【观点】

---

**来源清单**（全部取证于 2026-10-21，UTC+8；通道A 服务端抓取 + 本地DDG 搜索交叉验证 + 通道D 截图 12 张 + GitHub PAT 只读 API，取证档案 `research/evidence/braintrust/`，143 文件，manifest 带 sha256）：

1. https://www.braintrust.dev/ — 官网首页首屏（The active observability platform for agents / Ship quality agents at scale / Observe·Evaluate·Discover 三柱 / Academy / Encyclopedia）
2. https://www.braintrust.dev/pricing — 三档平台费（Starter $0 / Pro $249 / Enterprise Custom）、$10/$100 模型额度、1GB+$4/GB、10K scores+$2.50/1k、14/30 天留存、Unlimited users·projects·experiments、6–12 个月免费
3. https://www.braintrust.dev/blog/announcing-series-b — 2026-02-17：$80M、ICONIQ 领投、a16z/Greylock/Elad Gil/basecase 复投、点名 Notion·Replit·Cloudflare·Ramp·Dropbox、自述自研数据库（通篇无估值）
4. https://www.braintrust.dev/blog/announcing-series-a — 2024-10-08：$36M、a16z(Martin Casado) 领投、total $45M、投资人含 Greg Brockman·Arthur Mensch·Guillermo Rauch·Simon Last·Bryan Helmig + Datadog·Databricks Ventures、点名 Notion·Stripe·Vercel·Airtable·Instacart·Zapier·Coda·Browser Company
5. https://www.braintrust.dev/customers — 客户证言页（署名职务）
6. https://www.braintrust.dev/customers/notion — Notion 案例：70 Engineers aligned on evals / <24hrs to deploy a new frontier model / Sarah Sachs 署名
7. https://en.wikipedia.org/wiki/Braintrust — 2023-08 创立、Ankur Goyal、Elad Gil 孵化、2023-12 $5.1M 种子(Greylock)、2024-10 $36M @$150M、2026-02 $80M、Trace 大会、Brainstore/Loop/Gateway、Notion·Stripe·Airtable
8. https://techcrunch.com/2023/09/28/the-newest-new-thing-continuous-feedback-platforms-that-keep-bad-results-from-ai-created-code-from-making-it-into-production/ — 四人公司、$3M、「operating system for engineers building AI software」、Elad Gil/Adam D'Angelo/Clem Delangue
9. https://www.finsmes.com/2024/10/braintrust-raises-36m-in-series-a-funding.html — $36M Series A at $150M post-money（第三方）
10. https://www.finsmes.com/2026/02/braintrust-raises-80m-in-series-b-funding.html — $80M Series B（第三方确认融资事实）
11. https://siliconangle.com/2026/02/17/braintrust-lands-80m-series-b-funding-round-become-observability-layer-ai/ — $800M valuation（第三方，官方博文未载）
12. https://techcrunch.com/2026/05/06/ai-evaluation-startup-braintrust-confirms-breach-tells-every-customer-to-rotate-sensitive-keys/ — AWS 账户未授权访问、存客户模型 API key、every customer to rotate、containment 原话
13. https://tech.yahoo.com/cybersecurity/articles/ai-evaluation-startup-braintrust-confirms-180541225.html — 同一报道转载版（交叉验证）
14. https://news.ycombinator.com/item?id=37692239 — Show HN 2023-09-28，8 points / 2 comments，作者 ankrgyl
15. https://news.ycombinator.com/item?id=44194187 — Ask HN 2025-06-05，「feels like an early-stage product. Documentation is sparse」原话
16. https://hn.algolia.com/api/v1/search?query=braintrust&tags=story — HN 全量检索（该品牌帖普遍个位数至低两位数分数）
17. https://api.github.com/search/repositories?q=org:braintrustdata&sort=stars&per_page=15 — org 78 个非 fork 仓、星数排行（autoevals 1,063 起）
18. https://api.github.com/repos/braintrustdata/autoevals — 1,063 星 / 91 forks / 45 issues / MIT / created 2023-07-11 / pushed 2026-10-08
19. https://api.github.com/repos/braintrustdata/braintrust-proxy — 412 星 / 119 issues / MIT / created 2023-11-22 / pushed 2026-10-20
20. https://api.github.com/repos/braintrustdata/braintrust-sdk-javascript — **29 星** / 74 issues / Apache-2.0 / created 2023-07-17 / pushed 2026-10-20
21. https://api.github.com/orgs/braintrustdata/repos?per_page=100（分页两页，PAT 只读）— 全量 108 仓 / 78 非 fork / 星数合计 2,365 / 语言分布 / created 2023-07-10→2026-07-30
22. https://api.npmjs.org/downloads/point/last-month/braintrust — 6,889,964（2026-09-19→2026-10-18）
23. https://api.npmjs.org/downloads/range/2025-10-01:2026-10-20/braintrust — 385 天累计 44,242,439；月度 2026-03 2,836,340 → 2026-09 7,104,136
24. https://api.npmjs.org/downloads/point/last-month/autoevals — 2,438,756（2026-09-19→2026-10-18）
25. https://registry.npmjs.org/braintrust — created 2023-07-09（0.0.1）、latest 3.38.0（2026-10-19）、MIT、repository→braintrust-sdk-javascript
26. https://pypistats.org/api/packages/braintrust/recent — last_month 6,574,708 / last_week 1,769,701 / last_day 298,474
27. https://pypi.org/pypi/braintrust/json — version 0.47.0、source→braintrust-sdk-python
28. https://www.braintrust.dev/docs/admin/self-hosting/architecture — 控制面/数据面分离、数据面组件（API service/PostgreSQL/Redis/object storage/Brainstore）、SDK 直连数据面
29. https://www.braintrust.dev/blog/brainstore-benchmarks — 390 万 traces、23.9×（9,587→401ms）、2.55×（17,775→6,984ms）、3.73×（1,290→346ms），对手「a leading competitor」未具名
30. https://www.braintrust.dev/blog/brainstore — Brainstore 发布博文（对象存储 + 流式 Rust 引擎动机）
31. https://www.braintrust.dev/docs/deploy/ai-proxy — 「The AI proxy is deprecated and will no longer be regularly maintained. Use the gateway instead」+ v1/proxy 无感路由到 gateway
32. https://raw.githubusercontent.com/braintrustdata/braintrust-proxy/main/README.md — 统一 API/自动缓存/observability「Coming soon」原始承诺
33. https://www.braintrust.dev/docs/integrations — 官方计数 All (88) = providers 26 + SDK integrations 50 + developer tools 11（+llms.txt 逐页核算含 runtime 1）
34. https://www.braintrust.dev/docs/changelog — 月度 changelog（导航回溯 2023-06）、2026-10 条目（Patterns 严重度/PagerDuty/coding agent insights/SQL charts/Loop/trace cursor sessions、data plane v2.x 版本门槛）
35. https://www.braintrust.dev/docs/security — SOC 2 Type II / HIPAA / GDPR / Trust center / 部署选项
36. https://www.braintrust.dev/docs/plans-and-limits — 三档 limits 交叉核对（Starter $0 / Pro $249 / Enterprise、留存与配额）
37. https://llmtools.cc/blog/langfuse-vs-langsmith-vs-braintrust/ — 第三方横评定位（Langfuse 开源默认 / LangSmith LangChain 全家桶 / Braintrust eval-first、起始付费档 $249/mo，2026-07-26）
38. https://github.com/braintrustdata/lingua — Rust 格式翻译库（LLM message format、28 星、created 2025-08-30）
39. https://www.braintrust.dev/docs/integrations/agent-frameworks/pi-coding-agent — 官方集成页「Pi Coding Agent SDK / Trace Pi Coding Agent SDK prompts」

**勘误**：本文任何错误可在 javis.im 留言指出，核实后修订；若结论被推翻，本文将标注撤回。文中【事实】/【推断】/【观点】/【存疑】四色标注即为可信度分层，读者可只采信【事实】部分。利益冲突已在文头披露；$3M 与 $5.1M 两个种子口径、$150M 与 $800M 两个第三方估值、$125M 累计融资的相加、npm 与 PyPI 两个「月下载」的窗口差、Brainstore 自办基准与未具名对手、proxy deprecated 与仓库活跃的双口径、docs 子域不可达与 www 正典，均已随文列出而不是抹掉。
