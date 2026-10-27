> **首发 [https://javis.im/analysis/supabase/](https://javis.im/analysis/supabase/)，本文随站点同步更新**；文中图片见在线原页，英文版：https://javis.im/analysis/supabase/index-en

# 111,751 星只是副产品：YC 最新批次 55% 用 Supabase、周下载 1,990 万，官方口径 70% 的新数据库由 AI agent 创建

> 111,751 颗星只是副产品：YC 最新批次 55% 用它，周下载 1,990 万，官方口径说 70% 的新数据库由 AI agent 创建。本文把增长拆成五段——YC 火箭、生态拼装、双轨进钱与 agent 接棒。

取证日期：2026-10-19（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 20 · 可验证性 19 · 独特性 18 · 受众相关度 18 · 多维空间 18 = **93/100**（≥60 才动笔；素材库预分 82）。

## 这个站在做什么

取证日的官网首屏是一句 2020 年用到今天的口号：**Build in a weekend / Scale to millions**——Start your project 按钮旁边写着「Start your project with a Postgres database. Add Authentication, Data APIs, Edge Functions, Realtime Data, Storage, and Vector embeddings」，首屏三张产品卡分别是 Postgres Database（卖点行：100% portable / Built-in Auth with RLS / Easy to extend）、Authentication、Edge Functions，导航栏右上角挂着 GitHub 星徽 **111.6K**。（来源 1）【事实】

![Supabase 官网首页首屏：Build in a weekend / Scale to millions + 产品卡 + 星徽 111.6K，取证 2026-10-19](img/home.png)

定位的原话在官方 Architecture 文档里，且开篇就自我划界：「**Supabase is not a 1-to-1 mapping of Firebase**」——功能对齐 Firebase，但技术路线完全不同：「everything we use is open source; and wherever possible, we use and support existing tools rather than developing from scratch」，最要紧的一句是「we use Postgres rather than a NoSQL store … no other database offers the functionality required to compete with Firebase, while maintaining the scalability required to go beyond it」。（来源 3）【事实】

可达性一项先交代：素材库登记的目标 URL `https://www.supabase.com` 与 apex `https://supabase.com` 取证日同为 200 且正文同文（本专栏历史上出现过 apex 挂掉的站，这里两条都活）；docs、pricing、blog（含 `.md` 原文端点）、GitHub API、npm、PyPI、pypistats、HN、YC、PRNewswire 全部 200。（来源 1/2）【事实】

## 增长拆解 · 第一条曲线：星与下载，先摆一手数据

这篇的分析框架卡是**增长拆解**：不评「好不好用」，只拆「曲线从哪来、谁在替它做分发、钱从哪一端进来、哪一段需求正在换挡」。四条曲线里最好证的是星与下载——全部是 API 实抓，不靠转述。

- **星**：GitHub API 实抓 `supabase/supabase` **111,751 星**、17,157 forks、1,153 个 open issues、Apache-2.0、仓库创建 **2019-10-12**、最后一次 push 2026-10-19；org `supabase` 创建于 2019-08-24，174 个 public repos、12,305 followers。（来源 13/14）【事实】时间锚点：Show HN 首帖《Supabase (YC S20) – An open source Firebase alternative》2020-05-27 发出，**1,120 points**（来源 26）【事实】；2025-04 TechCrunch 写「over 81 thousand stars」（来源 23）【事实】；官方博客 2026-04-02 宣布过 **100,000 星**并补一句「eight million developers building with Supabase」（来源 6）【事实】。
- **下载**：npm API 实抓 `@supabase/supabase-js` **周下载 19,907,026 次**（2026-10-10 → 10-16）。（来源 18）【事实】逐日接口被 npm 钳在最近 18 个月（实际返回 2025-04-18 起），按月聚合：**2025-04 3,320,330 → 2026-09 110,674,238，约 33 倍**。（来源 19）【事实】同一窗口里有 **12 个「0 下载」日**和单日 5,833,734 次的尖峰（10-16），毛刺明显，本文只引月度量级、不逐日连线。（来源 19）【存疑】Python 侧 pypistats 实抓：日 723,588、周 7,203,785、**月 22,924,804**。（来源 20）【事实】

![Supabase 官方《100,000 GitHub stars》博文（2026-04-02，eight million developers），取证 2026-10-19](img/stars100k.png)

![GitHub 仓库页实抓：Star 按钮 112k / 侧栏 111.8k / Issues 306 + PRs 847 / 39,190 commits，取证 2026-10-19](img/github.png)

**同一天、同一个星数，三种显示**——这是引用数字时最容易翻车的地方：API 精确值 111,751；GitHub 页面侧栏 111.8k（四舍五入）、Star 按钮 112k（再进位）；Supabase 官网导航徽 **111.6K**（站方缓存，落后约 150 颗）。三者同源不同精度与刷新频率，不是三个互相打架的数。（来源 1/13/17）【推断】

issue 口径同样要拆：API 的 `open_issues_count` 是 **1,153**，页面分列 **Issues 306 + Pull requests 847 = 1,153**——API 把 PR 算进了「open issues」，直接引用会虚高 277%。另一个反直觉的字段是 `watchers_count`：它的值与 `stargazers_count` 完全相同（都是 111,751，GitHub 历史遗留），页面上的「764 watching」其实对应 API 的 `subscribers_count` 字段。（来源 13/17）【事实】

仓库数量也有两个口径：org API 报 **174 个 public repos**，search API `q=org:supabase` 只回 **124**，差值 50 用 `fork:only` 端点实抓正好是 **50 个 fork**——search 默认不返回 fork。（来源 14/15/16）【事实】

## 增长拆解 · 第二条曲线：YC 是第一级火箭

Supabase 自己就是 YC S20 出身（YC 官方页：Summer 2020、Founded 2020、Paul Copplestone 与 Ant Wilson、Team Size 120、San Francisco、状态 Active）。（来源 12）【事实】真正值得拆的是它把「校友网络」做成了可引用的采用率指标：

- **40% → 55%**：TechCrunch 2024-09-25 写「40% of startups in the latest Y Combinator class use Supabase」（来源 22）【事实】；一年多以后官方博客直接把数字抬到「**In the most recent Y Combinator batch, 55% of companies chose to build on Supabase**」，同一篇还公布「more than 1,000 Y Combinator companies have chosen Supabase」与「80+ 其他加速器、早期投资机构的伙伴计划」。（来源 11）【事实】
- **社会证明的密度**：Peak XV 合伙人在 Series C 披露里说 Supabase「over 900,000 sign-ups in four years … unprecedented organic adoption」（来源 22）【事实】；Felicis 管理合伙人在 Series D 说社区「rocketed past 1 million and growing by thousands every day」（来源 23）【事实】。
- **运营动作有档期**：黑客松站列出 Supabase Select 2026 Hackathon（2026-10-03，68 件提交）、Supabase Select 2025（54 件）、Supabase Lisbon AI Hackathon（2025-11）——「一天到两天、给提交数」的轻量赛事常年排期。（来源 31）【事实】SupaSquad 常年招募四轨贡献者（Contributor / Content Creator / Trusted Host / Event Speaker），官方写明的上升通道是「volunteer → paid contributor → paid employee」。（来源 30）【事实】
- **把用户变股东**：Series E 官方宣布「we will do a **$1M community round** open to early customers and contributors」，且「every single round we have allowed our employees to sell 25% of their vested stock」。（来源 8）【事实】

![Supabase 官方 Series F 博文内的累计用户曲线（2020-04 → 2026-04，接近 10M），取证 2026-10-19](img/seriesf.png)

另一条更隐蔽的分发线是**上游投喂**：官方 100k 星博文引用自家产品原则原话——「we employ maintainers, sponsor projects, invest in businesses, and develop our own open source tools」，并点名 Postgres、PostgREST、pgvector、Deno、imgproxy、Elixir Phoenix（Realtime 底座，「almost 1.5B messages daily」）这套「community of communities」。（来源 6）【事实】

【推断】把三条线并起来看：新批次过半数选它 → 早期项目默认栈 → 星与下载的复利 → 下一轮融资的采用率数字。这条链的每一环都有官方或一手出处，但**因果顺序是本文的推断**，Supabase 从未这样自述。

## 增长拆解 · 第三条曲线：产品矩阵与生态位，拼装而不是自研

Architecture 文档把平台拆成十个组件并逐个标了许可证与源码地址，这在商业化产品里并不常见：Postgres 本体（PostgreSQL License，「We do not abstract the Postgres database—you can access it and use it with full privileges」）、Studio（Apache-2，源码就是 `github.com/supabase/supabase`）、GoTrue/Auth（MIT）、PostgREST（MIT，上游独立项目）、Realtime（Apache-2，Elixir）、Storage API（Apache-2，S3 兼容）、Deno（Edge Functions，MIT）、postgres-meta（Apache-2）、Supavisor（Apache-2，连接池）、Envoy（Apache-2，网关）。（来源 3）【事实】

六条产品原则里有四条直接影响增长姿势：**Everything works in isolation**（试金石原话：「Can a user run this product with nothing but a Postgres database?」）、**Everything is extensible**（宁可扩展已有件，不新开品类）、**Everything is portable**（「To avoid lock-in, we make it easy to migrate in and out … If a new standard emerges which competes with a 'Supabase' approach, we will deprecate the approach in favor of the standard」）、**Play the long game**（宁可推动上游，也不做只有自己客户能用的 Postgres fork）。（来源 3）【事实】

星数矩阵印证「拼装」的分布：org 174 仓里头部是 realtime **7,660**、supabase-js **4,588**、pg_graphql **3,359**、auth 2,580、cli 2,437、supavisor 2,276、postgres 1,796；更抢眼的是新面孔 mcp **2,961**（2024-12 建仓）与 agent-skills **2,879**（2026-01 建仓）——两个 AI 时代的组件只用不到两年就冲进 org 前六。（来源 15）【事实】

生态位的第三方背书写在 TechCrunch 三篇报道里：Series B 时 CEO 自述「moving upmarket … an alternative to [Amazon] RDS」（来源 21）【事实】；Series C 点名 GitHub Next、Meta、Netflix、Microsoft 的开发者在用，并提到 Vercel marketplace 合作（来源 22）【事实】；Series F 报道称它是「Bolt、Figma、Lovable、Replit 等 vibe-coding 工具的数据库之选」（来源 24）【事实】。

## 增长拆解 · 第四条曲线：开源 + 云双轨，钱从哪一端进来

定价页四档，取证日原样是：**Free $0**（500 MB 数据库、50,000 MAU、5 GB egress、1 GB 文件存储、**限 2 个活跃项目、1 周不活跃即暂停**）｜**Pro $25/月**（首个项目含，附加项目 $10/月起；100k MAU 后 $0.00325/MAU、8 GB 磁盘后 $0.125/GB、250 GB egress 后 $0.09/GB）｜**Team $599/月**（SOC2 & ISO 27001、SSO、HIPAA 付费附加）｜**Enterprise**（议价、SLA、PrivateLink、7×24 支持）。（来源 4）【事实】

![Supabase 定价页：Free / Pro $25 / Team $599 / Enterprise 四档，取证 2026-10-19](img/pricing.png)

融资线一共七轮，逐段都有出处：Series B **$80M**（TechCrunch 2022-05-10，同期 CEO 披露「more than 80,000 developers who have created over 100,000 databases … growth of 1,900% in the last 12 months」）（来源 21）【事实】；Series C **$80M**（2024-09-25，Peak XV 与 Craft 领投，**累计 $196M**，估值未披露但确认是 up round）（来源 22）【事实】；Series D **$200M @ $2B post-money**（2025-04-22，Accel 领投，**累计约 $398M**）（来源 23）【事实】；Series E **$100M @ $5B pre-money**（2025-10-03 官方，Accel 与 Peak XV 领投，Figma 加入）（来源 8）【事实】；Series F **$500M @ $10B pre-money**（2026-06-04 官方，GIC 领投，Stripe 二次投资，Georgian 与 Salesforce Ventures 新入；报道口径约 **$10.5B post-money**）（来源 7/24）【事实】；2026-10-02 再宣 **$150M 新融资**（GIC 领投，CapitalG、IronArc、SquarePeg 参与），距 Series F 只隔四个月（来源 25）【事实】。

累计口径这里必须停一下：Latka 记「Total raised **$995M, 7 rounds**」（来源 33）【事实】，与「$196M(C) + 200 + 100 + 500 = 996」对得上，说明该口径**未含 2026-10 的 $150M**；若把这轮计入，推算约 $1.15B（来源 7/25/33）【推断】——引用「累计融资」时必须说明截至哪一轮。

收入侧没有任何官方披露：第三方估计 Sacra 给出 $30M（2024 末）→ **$70M ARR**（其 TL;DR 写 2025-08、正文写 2025-09，同页两个日期）→ 标题口径 **$170M/year**（来源 32）【存疑】；Latka 记「Annual revenue **$170M** May 2026 EST」「Employees 350」「Valuation $10.5B」（来源 33）【存疑】。两家的 $170M 都是 EST/估算方法不透明，本文并列不连线，也不拿它算市销率。

<span>【观点】</span>把它放回本专栏的坐标系：OpenRouter 用抽成回答变现（第 2 篇），LiteLLM 用「never per token」回答（第 3 篇），LangChain 用「开源框架当获客层、平台当收入层」（第 5 篇），Supabase 的答案最接近 LangChain 但更彻底——**Apache/MIT 的组件矩阵是免费的获客层，Postgres 的可携带性是反锁定的销售话术，$25 起步的云才是收入层**；连融资都复用了分发逻辑（$1M 社区轮把贡献者变股东）。

## 增长拆解 · 第五段：需求结构换挡，agent 接过增长方向盘

2026 年这条曲线的动力源换了人，官方口径的两次更新写得很直白：

- Series F（2026-06-04 官方）：「database launches on Supabase have grown **600%** in the past year. **More than 60% of new databases are launched by some sort of AI tool**」「Nearly **10 million developers** build on Supabase today, more than doubling since our last fundraising announcement eight months ago」，并把加速归因给 Claude Code 与 Codex「expand the number of people who can build」。（来源 7）【事实】
- PRNewswire 官方新闻稿（2026-10-02）：「Supabase is now adding **more than 1M users and 4M databases per month with 70% of new databases created by agents or AI-driven tools**」，并称 600% 的同比增长发生在「agents were already deploying the majority of new databases」的基础上。（来源 25）【事实】
- 同日官方博文（收购 Turso）：「Supabase is already launching **over one million databases per week**」，理由是「agents are spinning up millions of databases to power the prototypes, explorations, dashboards, and apps they're building」。（来源 9）【事实】

三组口径换算一下：每周 100 万+ ≈ 每月 400 万+，与新闻稿的 4M/month 自洽；60%（6 月）→ 70%（10 月）是同一指标四个月内抬了 10 个百分点。（来源 7/9/25）【推断】

收购动作本身是增长的另一条腿，且是**两种买法**：Triplit（2025-10-08）买的是人和知识——「Our focus isn't to directly integrate Triplit into our platform」，团队成员转去做第三方同步生态（ElectricSQL、Zero、PowerSync）并继续开源 Triplit 代码（来源 10）【事实】；Turso（2026-10-02）买的是整条 SQLite 架构——「a single server can manage millions of databases, loading them when needed and suspending them when they're not」，创始人 Glauber Costa 出任 Head of Agentic Services，客户包括 Superhuman、Sauna.ai、CTO.new、Mastra（来源 9/25）【事实】。

【推断】需求换挡的另一面是风险：官方自己把这批数据库定性为「prototypes, explorations, dashboards」（来源 9），agent 生成的库天然是短命周期、低规格负载；70% 的新库来自 AI 工具，意味着**「开发者数」这个传统代理变量正在失去解释力**——它既可能高估付费转化（一次性原型），也可能低估真实扩容需求（原型转生产）。这条判断是本文推断，官方未披露原型转付费的比率。

## 风险与存疑清单

- **安全侧的一手热点**：HN《Supabase MCP can leak your entire SQL database》（来源 generalanalysis.com）2025-07-08 上榜，取证日页面实抓 **848 points / 470 comments**。（来源 28）【事实】MCP 把数据库权限暴露给 LLM 代理，这条风险与上一条的 70% agent 需求是同一枚硬币的两面。（来源 25/28）【推断】
- **迁移与组件替换的持续噪音**：HN《Migrating from Supabase》（Val Town，2023-05-19，**382 points**）里，CEO Paul Copplestone 用本人账号下场逐条回应，给出 Maturity Models 文档与「migrating away from Supabase was easy for Val Town, because it's just Postgres」的总结；评论区点名的痛点是本地开发体验、把业务逻辑放进数据库（RLS/DB functions）的可维护性、支持响应慢。（来源 27）【事实】2026-05-06 的《From Supabase to Clerk to Better Auth》（**304 points**）则是另一类信号：套件内的 Auth 组件正在被单点替换。（来源 29）【事实】
- **收入与「开发者数」的口径都不可审计**：收入无官方数字，第三方 EST 互相拼不出一致方法论（来源 32/33）【存疑】；开发者数口径在六年里换过至少四种——80,000 developers（2022-05）→ 900,000 sign-ups（2024-09）→ 1.7M developers（2025-04）→ 8M developers（2026-04）→ nearly 10M（2026-06）→ +1M users/月（2026-10），signups/users/developers 混用，本文全部并列，不连线算增长率。（来源 6/7/21/22/23/25）【存疑】
- **估值的 pre/post 必须带口径**：Series E 官方写「$5B **pre-money**」、Series F 官方写「$10B **pre-money**」而媒体报道「about $10.5B post-money」——省略一个词，账面就差 10 亿。（来源 7/8/24）【存疑】
- **npm 计数毛刺**：18 个月窗口内 12 个 0 下载日与单日 5.83M 尖峰并存，且接口把请求钳在 18 个月，本文只引月度量级。（来源 19）【存疑】
- **免费层的门槛**：Free 计划限 2 个活跃项目、1 周不活跃即暂停项目——「免费起步」的分母里有相当比例是休眠项目，采纳数与活跃数不是一回事。（来源 4）【事实】
- **整合执行风险**：Turso 要同时维持 SQLite 与 Postgres 两条栈的承诺，Triplit 那种「只买人不买产品」的买法已给出另一种答案；两种整合是否都兑现没有回溯数据。（来源 9/10）【推断】

## 结论

把四条曲线收成一句话：**Supabase 的增长引擎从来不是那 111,751 颗星——是「YC 批次内部标准化 → 开源件拼装成平台 → agent 需求接棒」三段互相咬合的传动系统**，星和下载只是露在外面的转速表。（来源 6/11/13/25）【推断】官方那句「We never optimized for stars. We optimized for developers having a good time building things and stars are a side effect of that」（来源 6）【事实】，放在这个框架里不是谦辞，是路线说明。

对做基建的团队，三个可抄的动作：①**把采用率做成可引用的官方里程碑**——40%（2024-09，第三方报道）→ 55%（2025-10，官方宣发），一个批次口径喂养两条时间线的报道与自述（来源 11/22）【观点】；②**把流动性当增长工具**——每轮允许员工出售 25% vested、$1M 社区轮向客户与贡献者开放，融资动作本身完成了一次社区动员（来源 8）【观点】；③**组件级开源 + portable 承认反而成了信任广告**——Val Town 搬走容易这件事，被 CEO 主动写进批评帖回复里当卖点（来源 27）【观点】。

一句清醒剂给同行：**当 70% 的新数据库由 agent 创建，「开发者数量」就不再是需求质量的代理变量**——原型转生产的比率、休眠项目占比、周下载里的 CI 权重，这三件事官方一个都没披露，而估值四个月翻一倍（$5B → $10B pre）恰恰建立在前者的乐观读法上。（来源 7/25）【观点】

---

**来源清单**（全部取证于 2026-10-19，UTC+8；通道A 服务端抓取 + 通道B 搜索交叉验证 + 通道D 截图，取证档案 `research/evidence/supabase/` 90 文件，manifest 带 sha256）：

1. https://supabase.com/ — 官网首页首屏（Build in a weekend / Scale to millions、产品卡、导航星徽 111.6K）
2. https://www.supabase.com/ — 素材库目标 URL（与 apex 同文、同 200）
3. https://supabase.com/docs/guides/getting-started/architecture — 官方 Architecture：「not a 1-to-1 mapping of Firebase」、十组件许可证矩阵、六条产品原则
4. https://supabase.com/pricing — 定价四档（Free $0 / Pro $25 / Team $599 / Enterprise）与用量费率
5. https://supabase.com/blog — 官方博客索引（发布节奏与收购公告档期）
6. https://supabase.com/blog/100000-github-stars.md — 官方《100,000 GitHub stars》（2026-04-02：100,000 星、eight million developers、stars are a side effect、community of communities 原则原文）
7. https://supabase.com/blog/supabase-series-f.md — 官方《Supabase Series F》（2026-06-04：$500M @ $10B pre、GIC 领投、600% 数据库增长、60% AI 工具、nearly 10M developers、50+ 国家、累计用户曲线图）
8. https://supabase.com/blog/supabase-series-e — 官方《Supabase Series E》（2025-10-03：$100M @ $5B pre、$1M 社区轮、每轮 25% vested 二级出售、37 国）
9. https://supabase.com/blog/supabase-is-acquiring-turso.md — 官方《Supabase is acquiring Turso》（2026-10-02：每周 100 万+ 新库、SQLite/Rust 架构、prototypes 定性、客户名单）
10. https://supabase.com/blog/triplit-joins-supabase.md — 官方《Triplit joins Supabase》（2025-10-08：买人不买产品、继续开源 Triplit 代码）
11. https://supabase.com/blog/1000-yc-companies.md — 官方《1000 Y Combinator Founders Choose Supabase》（2025-10-03：1,000+ YC 公司、最新批次 55%、80+ 加速器伙伴）
12. https://www.ycombinator.com/companies/supabase — YC 官方公司页（Summer 2020、Founded 2020、Team Size 120、融资新闻时间线）
13. https://api.github.com/repos/supabase/supabase — GitHub API（111,751 星 / 17,157 forks / 1,153 open issues 含 PR / Apache-2.0 / created 2019-10-12 / pushed 2026-10-19 / watchers_count 与 stargazers_count 同值）
14. https://api.github.com/orgs/supabase — GitHub org API（created 2019-08-24、174 public repos、12,305 followers）
15. https://api.github.com/search/repositories?q=org:supabase&sort=stars&order=desc&per_page=30 — org 星数矩阵（realtime 7,660 / supabase-js 4,588 / pg_graphql 3,359 / mcp 2,961 / agent-skills 2,879；total_count 124）
16. https://api.github.com/search/repositories?q=org:supabase+fork:only&per_page=1 — fork 口径核对（50 个 fork，124 + 50 = 174）
17. https://github.com/supabase/supabase — 仓库页实抓（Issues 306 + PRs 847、39,190 commits、1,819 branches、63 tags、Star 按钮 112k、侧栏 111.8k、watching 764、Fork 17.2k）
18. https://api.npmjs.org/downloads/point/last-week/@supabase/supabase-js — npm 周下载 19,907,026（2026-10-10 → 10-16）
19. https://api.npmjs.org/downloads/range/2021-01-01:2026-10-18/@supabase/supabase-js — npm 逐日下载（接口钳制到 2025-04-18 起 18 个月；月度 3.32M → 110.67M；12 个 0 下载日；单日尖峰 5,833,734）
20. https://pypistats.org/api/packages/supabase/recent — Python 包下载（日 723,588 / 周 7,203,785 / 月 22,924,804）
21. https://techcrunch.com/2022/05/10/supabase-raises-80m-series-b-for-its-open-source-firebase-alternative/ — Series B $80M（2022-05-10：80,000 developers、100,000 databases、1,900% 年增长、Postgres-as-a-service vs RDS）
22. https://techcrunch.com/2024/09/25/supabase-a-postgres-centric-developer-platform-raises-80m-series-c/ — Series C $80M（2024-09-25：累计 $196M、40% 最新 YC class、900,000 sign-ups、Vercel marketplace、GitHub Next/Meta/Netflix/Microsoft）
23. https://techcrunch.com/2025/04/22/vibe-coding-helps-supabase-nab-200m-at-2b-valuation-just-seven-months-after-its-last-raise/ — Series D $200M @ $2B post（2025-04-22：累计约 $398M、1.7M developers、81k 星、Felicis「rocketed past 1 million」、Lovable 后端）
24. https://techcrunch.com/2026/06/05/supabase-doubles-valuation-to-10b-in-8-months/ — Series F 报道（2026-06-05：约 $10.5B post、8 个月翻倍、Bolt/Figma/Lovable/Replit、Multigres「operating system」）
25. https://www.prnewswire.com/news-releases/supabase-announces-150m-in-new-funding-and-turso-acquisition-302896752.html — 官方新闻稿（2026-10-02：$150M 由 GIC 领投、CapitalG/IronArc/SquarePeg、每月 +1M 用户 / +4M 数据库、70% 新库来自 agent 或 AI 工具、Turso 收购、Glauber Costa 任命）
26. https://news.ycombinator.com/item?id=23319901 — Show HN《Supabase (YC S20) – An open source Firebase alternative》（2020-05-27，1,120 points）
27. https://news.ycombinator.com/item?id=36004925 — HN《Migrating from Supabase》（2023-05-19，382 points，Val Town 迁移复盘 + CEO 本人逐条回复 + 「everything is portable」）
28. https://news.ycombinator.com/item?id=44502318 — HN《Supabase MCP can leak your entire SQL database》（2025-07-08，848 points / 470 comments）
29. https://hn.algolia.com/api/v1/search?query=supabase&tags=story&hitsPerPage=30 — HN 榜单实抓（《From Supabase to Clerk to Better Auth》304 分 2026-05-06、《Supabase is acquiring Turso》222 分 2026-10-02、《Supabase open sourced their dashboard》331 分 2021-12-01）
30. https://supabase.com/open-source/contributing/supasquad — 官方 SupaSquad 页（四轨贡献者、volunteer → paid contributor → paid employee）
31. https://hackathon.supabase.com/ — 官方黑客松站（Select 2026 68 件提交、Select 2025 54 件、Lisbon AI Hackathon 2025-11）
32. https://sacra.com/research/supabase-at-70m-arr-growing-250-yoy/ — Sacra 收入研究（$30M 2024 末 → $70M ARR 2025-08/09（同页两日期）→ 标题 $170M/year；vibe coding 默认后端；Neon 被 Databricks 以 $1B 收购的品类对照）
33. https://getlatka.com/companies/supabase.com — GetLatka（Annual revenue $170M May 2026 EST、Total raised $995M 7 rounds、Employees 350、Valuation history $2B/$5B/$10.5B）
34. https://supabase.com/docs/guides/getting-started — 官方文档入口（Build with AI tools 等 agent 时代导航）

**勘误**：本文任何错误可在 javis.im 留言指出，核实后修订；若结论被推翻，本文将标注撤回。文中【事实】/【推断】/【观点】/【存疑】四色标注即为可信度分层，读者可只采信【事实】部分。官方自报的增长数字（600%、70%、nearly 10M）、第三方收入估算（$170M EST）、pre/post 估值口径差、npm 计数毛刺，均已随文列出而不是抹掉。

> 本站为独立技术分析，与文中项目无商业关系。
