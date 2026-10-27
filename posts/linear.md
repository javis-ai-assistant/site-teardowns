# 一年估值翻倍、零新钱入账：Linear 的商业画布上，写着一次没公告的 25% 涨价和一句「Issue tracking is dead」

> 九格画布里最值钱的一格是「不再需要新钱」：40,000 家付费公司撑起 $100M 进账，一次没公告的 25% 涨价和一句「Issue tracking is dead」也记在上面，外加 118 人团队三个打架的口径。

取证日期：2026-10-23（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 18 · 可验证性 17 · 独特性 18 · 受众相关度 19 · 多维空间 18 = **90/100**（≥60 才动笔；素材库预分 77）。框架卡 = **商业画布**（上一篇为增长拆解，不连用；本篇按客户细分／价值主张／渠道／收入／成本／壁垒六格骨架走，不写功能测评）。

## 画布第一格：把 issue tracker 卖成「产品开发系统」

Linear 成立于 2019 年，官网 About 页署名两位联合创始人：Karri Saarinen（Co-founder, CEO）与 Tuomas Artman（Co-founder, CTO）；第三方拆解补上第三位 Jori Lallo，并称 Karri 是前 Airbnb 设计负责人、Tuomas 是前 Uber 工程负责人。（来源 12/37）【事实】

产品身份在取证窗口内换过一次：首页大标题是「The product development system for teams and agents」，副题「From drafting PRDs to pushing PRs」——2019 年的定位是「fast issue tracker」（HN 首发帖标题即此），2026 年的定位是人和 agent 共用的产品开发系统。（来源 1/25/14）【事实】

![Linear 首页取证截图：product development system for teams and agents，取证 2026-10-23](img/home.png)

它同时经营一套方法论内容资产 Linear Method：官方公开写着「we don't write user stories and think they're an anti-pattern」，主张用大白话写短 issue、每个人都自己写自己的 issue。（来源 20/50/53）【事实】**【推断】这套方法论是获客内容而不是使用说明**——它把「Linear 的工作方式」包装成可传播的行业主张，反对 user story 这种行业默认仪式，正好给「为什么该换工具」提供了理由。（来源 21/20）【推断】

## 客户细分：40,000 家付费公司、127,000 个付费用户，和一个被除出来的客均数

官方口径：「More than 40,000 companies now pay for Linear」，并自证在 2026 年内「passed $100 million in annual recurring revenue」。（来源 7）【事实】官方 /data 报告另给一层用户口径：付费活跃用户 **N = 127,000**（2026 年 1 月与 6 月均活跃），付费用户数从 54,300（2025-06）增至 89,000（2026-06）。（来源 16）【事实】

客群分层在客户页与 now 页可点开对照：**大企业**——Salesforce 与 Figma 在 2026 年内成为客户，Salesforce 是「organically adopted」后才有的 Salesforce Ventures 投资关系；**AI 公司**——OpenAI、Cursor、Cognition、Harvey、Physical Intelligence、Legora、Baseten；**案例墙**——OpenAI 用到 3,000 用户、Ramp 的合并 PR 里 75% 由 coding agent 产生、Scale 把缺陷解决时间压掉 52%、Remote 把 1,000 人团队搬了过来、Oscar 走出了「世界最复杂的 Jira 实例之一」。（来源 7/17）【事实】——其中 2.0x 建单量、3.3x 解决提速、50% issue 由 agent 撰写三项是官方 switch 栏目的自报数，**单一口径、无第三方复核**。（来源 17）【存疑】

**【推断】客均钱数是这条画布最容易被误读的一格**：$100M ÷ 40,000 ≈ **$2,500/公司/年**（Sacra 用同一组官方数算出 blended ARPA ≈ $2,500），而官方价目是 $10–16/席/月——按年付折算 $120–192/席/年，倒推下来**平均每家付费公司约 13–21 席**。这个数是混档位平均（企业合同价更高、免费层不计），只能当量级看。（来源 7/2/32）【推断】

## 价值主张：50ms 的手感，和一堵「250 条 issue」的墙

价值主张的第一句是快。第三方技术拆解给的量级：Linear 更新一个 issue 只花几毫秒，传统 CRUD 同样操作约 300ms；技术栈被描述为「React, TypeScript, MobX, Postgres, a CDN」+ 自研 local-first 同步引擎，UI 同步重绘本地内存态、后台再把事务批量刷给服务器，Issue 与 Comment 两张最重的表按需 lazy-hydrate，于是「10,000 issue 的工作区启动速度和 100 issue 的差不多」。（来源 41）【事实】官方自己的同步引擎分享页也长期在线。（来源 44/42/43）【事实】

社区对这份「快」的评价不是单向的。HN 两个高分存量帖可点开：首发帖 491 分 / 221 评论，「Settings are not a design failure」488 分 / 378 评论；2022 年主帖 212 分里既有「even under attack, Linear is faster than JIRA」，也有人反驳「50ms user interactions is not what I would consider breathtakingly fast」，还有人抱怨离线模式打开是空白窗、必须先在线登录过。（来源 25/26/27/31）【事实】同期站外批评帖《Linear is always a lagging indicator》只有 14 分 / 9 评论——正面存量与反向声量差了一个量级。（来源 52）【事实】

![Linear 定价页取证截图：四档价格与 250 issue 免费墙，取证 2026-10-23](img/pricing.png)

价值主张的第二句是美学与方法论（见第一格），第三句则是一堵刻意留着的墙：免费版「Unlimited members / 2 teams / **250 issues**」，10MB 文件上限，其余全数上锁。（来源 2）【事实】

## 渠道与客户关系：$36,000 营销费、一个销售，以及新加坡的企业销售岗

这条画布的渠道格几乎写着「没有渠道」。ValueAdd 引 Forbes 2023：Linear 当年营销总花费约 **$36,000**、销售只有 **1 个人**，增长主要来自产品自下而上被采用、老客户加席位；乙方稿 eleken 给的数是「$35k lifetime marketing spend」——两个数接近但口径不同（年度 vs 累计），**都属二手转述**。（来源 37/36）【存疑】

漏斗本身是明码的：免费版撞 250 条 issue 墙 → 购买 Basic（$10/席/月）→ 需要私有团队、guest、Insights、Asks 升 Business（$16/席/月）→ 需要 SAML/SCIM、发票付款进 Enterprise（年付、报价制）。第三方实施顾问的原话是「a trial funnel disguised as a free plan」，并指出月付价「不作为大标题数字展示」、计费口径是「任何角色的未停用成员都计费」。（来源 2/6）【事实】

**【推断】但「零销售」已经不成立了**：取证日 Careers 页列出的在招岗里有 Account Executive（Enterprise / Growth / Startups，**新加坡**）、Customer Success Manager（APAC / Enterprise / Growth，新加坡与北美）、Solutions Engineer（APAC）——企业销售与客户成功正在亚太布点，now 页也写着 30+ 个在招岗位。（来源 13/7）【推断】

客户关系这一格官方想讲的是「create fans」（Careers 十一条原则第 10 条），同时把关系做进 agent：在 Slack/Teams 里 @Linear 把讨论变成 issue、agent 动作全部留痕可回看。（来源 13/19）【事实】

## 收入：$100M ARR，与一次没人公告的涨价 25%

收入格的四个硬数全部来自官方 2026-08-26 的 now 页：**$100M+ ARR**、**40,000+ 付费公司**、**NRR 177%**、2026 年内达成。（来源 7）【事实】Sacra 同口径记为「hit $100M in ARR in July 2026」，并称 NRR 从 2025 年末约 145% 升到 177%。（来源 32）【存疑】——第三方估算，非审计数。

价格是本篇取证最硬的一段：**Wayback 两张快照本机直取对表**——`20251007142418` 快照里 Basic **$8**、Business **$14**（年付价），`20251113092935` 快照里已变成 **$10**、**$16**。同一件事第三方比价站的判定窗口是 2025-10-07 至 11-12（与本机 11-13 快照差一天，属抓取时点/时区差，区间一致）：Basic **+25%**、Business **+14.3%**，每席每月同涨 $2，Free 档未动，**定价页没有加生效日期、也没有公告**。（来源 3/4/5/55）【事实】现行官网价与此一致。（来源 2）【事实】涨价后 10 席团队从 $960/年变 $1,200/年。（来源 5）【事实】

**【推断】静默涨价 + AI credits 是提客单价的两条暗线**：席位价涨在无人播报的角落，而 Coding Sessions 与 Loops 除席位外还需单独购买 AI credits（用量计费行），wmh 与 ValueAdd 均记载了这条叠加收费线。（来源 6/37）【事实】——两条线合起来解释了 Sacra 那句「NRR 加速说明 ARPA 实质抬升」。（来源 32）【推断】

## 成本结构与团队：开业 12 个月即盈利，118 人的口径却互相打架

官方《The profitable startup》自述：发布 12 个月后盈利，2021 年起持续盈利，100 个内测用户几乎全数转付费，「negative lifetime burn rate（银行里的钱比融过的还多）」在 2023 年 Series B 页就写过。（来源 11/10）【事实】同期口径：「a year after launching the product」即 2021 年起盈利。（来源 10）【事实】官方该文在 HN 拿到 248 分，是这套盈利叙事的公开讨论面。（来源 30）【事实】

团队规模的第三方读数互斥：Latka 页面上四个 headcount 读数分别来自 X（2026-06：**118**）、LinkedIn（2025-11：**178**）、Growjo 模型估算（2023-12：88、2022-12：42）——2025-11 到 2026-06 之间读数从 178 掉到 118，来源不同、方向反常，**本文不采信任一单值**。（来源 33）【存疑】

成本结构能确认的部分：Careers 十一条原则里第 8 条「Keep the team small, do more with less」、第 11 条「Fully remote」；now 页自述「resisted many of the usual company-building playbooks」，同时承认这一阶段「needs to grow」并放出 30+ 岗位。（来源 13/7）【事实】**缺证项**：毛利率、销售人效、合同结构、AI 推理成本均未公开，商业核心的分母查不到，这是本篇可验证性扣分的主因。（来源 17/7）【存疑】

## 资本动作：$99M 二手售卖，一年把 $1.25B 抬到 $2.5B

四轮资本事件按时间排开（官方 now 栏目可索引，来源 49）：

| 时点 | 事件 | 金额与口径 | 出处 |
|---|---|---|---|
| 2023-09-14 | Series B（Accel 领） | $35M；Forbes 报道对应约 $400M | （来源 10/37） |
| 2025-06 | Series C（Accel 领，primary + secondary） | **$82M @ $1.25B** | （来源 9/32） |
| 2025-06（同期） | 上一次员工 tender | 与 Series C 同价 $1.25B | （来源 7） |
| 2026-08-26 | 员工 tender | **$99M @ $2.5B** | （来源 7/8） |

（来源 7/8/9/10/32/37）【事实】四轮累计融资 $134.2M（Latka 与 TechCrunch 口径一致）。（来源 33/37）【事实】

官方原话值得整段引：「**Fundraising can become a habit.** … We are cashflow positive, and have more cash in the bank than everything we have raised to date. Raising more primary capital would have meant diluting the team and our shareholders without a clear reason」；Dealroom 把结构说破：「the $99 million went to employees rather than Linear's balance sheet」——**这是 secondary，钱归卖方，不进公司**。（来源 7/8）【事实】

**【事实】一年内估值翻倍，且零新钱入账**：$1.25B → $2.5B 的差额来自员工与前员工的股权二级出售，买方为老股东 Accel、01A 加新进的 Salesforce Ventures 与 S32。（来源 7/8）【事实】【推断】它说明这个阶段 Linear 的估值锚已经从「融多少钱」换成了「多少 NRR 与 ARR」——$2.5B ≈ 25× ARR，若按 $100M 口径是 25 倍收入倍数。（来源 7/37）【推断】另有两处细节互相咬合：Salesforce 既是 2026 年新客户又是 tender 新投资人（官方称其为 organic adoption 后的「natural fit」），AI 公司客户名单与「AI-native teams」叙事同框。（来源 7）【事实】

## 关键活动：CEO 自己宣布所处品类死亡

2026-03-24，Karri Saarinen 在 linear.app/next 发表《Issue tracking is dead》：「It was built for a handoff model of software development」，并给出三个数——**75%+ 企业工作区已装编码 agent**、**近三个月 agent 完成的工作量 5x**、**agent 撰写了近 25% 的新 issue**；同日 changelog 上线 Linear Agent。（来源 14/18）【事实】The Register 两天后跟进报道，同时点出官方所有文章与 agent 文档里「one thing not mentioned… is security」，以及「issues will remain as the core items in Linear」——宣布死亡的品类仍是产品核心。（来源 15）【事实】同线的 CI 改造帖在 HN 拿到 317 分 / 408 评论，是这条叙事的讨论面。（来源 28）【事实】

五个月后的 2026-08-26 now 页把数推到新位置：agent 已安装在 **95% 的付费工作区**，agent 创建的工作占比从一年前的 3% 涨到 **50%**，附带 PR 的 issue 数量自 2026 年初增长 **7 倍**。（来源 7）【事实】**口径提醒**：3 月的「近 25% 新 issue」与 8 月的「50% 工作占比」措辞不同（新建 issue 条数 vs 工作量占比），跨月对比只能当趋势看，不能当同一条曲线读。（来源 14/7）【推断】

![Linear AI 页取证截图：agent 平台与 Coding Sessions，取证 2026-10-23](img/ai.png)

## 定量底稿：/data 报告里被算出来的五个数

官方《AI usage patterns in software teams》（Edition 01, 2026）是本篇的定量底稿，可点开核对：**①** N = 127,000 付费用户（1 月与 6 月均活跃）；**②** AI 功能活跃度在 2026 上半年每个职能都翻倍，产品岗 **12% → 34%**、工程 12% → 30%、离代码最远的 GTM 也从 5% → 18%；**③** 产品经理附 PR 的比例从 3% → 10%（两年），设计师 1% → 8%；**④** 接入 coding agent 的团队周 PR 从 **21 → 65**，没接的从 8 → 10；**⑤** 建 issue 侧，两年前 AI 撰写比例不到千分之一，如今「just under half」。（来源 16）【事实】

报告自己写明边界：只覆盖付费工作区、只统计接入 Linear 的仓库（「numbers are floors rather than ceilings」）、只看自家客户而非全市场。（来源 16）【事实】HN 上同题讨论 199 分 / 115 评论，可当外部反应面。（来源 29）【事实】

**【观点】这一页是「AI 吃掉 issue tracker」的自证式底稿，也是最需要外部复核的一类证据**：数据由被分析方自己发布、口径由自己定义，好处是样本巨大（N=127,000），代价是无法独立重算。（来源 16/29）【观点】

![Linear /data 报告取证截图：N=127,000 付费用户与 agent 周 PR 曲线，取证 2026-10-23](img/data.png)

## 壁垒与竞争：66 倍压顶的 Atlassian，和已经关店的 Height

量级对照（同一时点、不同口径，全部可点开）：Atlassian 截至 2026-06-30 的**订阅 ARR $6,606M**（同比 +23%），FY26 Q4 营收 $1,766M（+28%），FY26 全年营收 $6,572M，客户 350,000+，覆盖 85% 的财富 500 强；Linear 是 $100M ARR。（来源 39/7）【事实】**$6,606M ÷ $100M ≈ 66 倍，Linear 约为前者的 1.5%**。（来源 39/7）【推断】

同赛道对照组更狠：Height（总融资 **$18.3M**，Redpoint 领的 $14M A 轮在 2021-09）宣布关停，**2025-09-24 为最后服务日**，创始人 Michaël Villar 的告别信被评论为「generic、没说为什么」；一年后 Linear 的 tender 把估值翻到 $2.5B。（来源 40/7）【事实】**【推断】同一品类一年内一家关店一家翻倍**，说明这轮出清不是品类没需求，而是单点工具撑不住——Linear 把自己从 issue tracker 挪到了「产品开发系统 + agent 平台」的位置上。（来源 40/7/14）【推断】

价格侧的对照：toolchew 按 2026-05 价目算 Jira Standard $7.91/席/月、Premium $14.54，均低于 Linear 的 $10/$16，但 Atlassian Access 的 SSO/SCIM 要另加 $4/席/月，100 人规模会「flip the gap」。（来源 38）【存疑】——第三方比价，未逐项复核。Atlassian 侧的反攻同样在取证里：MCP server 与 Teamwork Graph CLI 一个季度翻倍到 100 万 MAU，CEO 在财报里说「In the AI era, context is the edge」。（来源 39）【事实】

**【推断】Linear 的壁垒不在功能清单，而在三样可点开的东西**：local-first 同步引擎带来的手感（41/44）、Method 这套可传播的工作方法论（20/21）、以及 NRR 177% 证明的存量扩张能力（7）。功能清单反而是它按月补齐的（changelog 半月节奏）。（来源 41/44/20/21/7/48）【推断】

## 反差面与口径互斥：一小时权限裸奔、11 条投诉，和三个打架的第三方数

**事故一（美学人设的代价）**：2026-03-24 凌晨的一次代码变更让私有团队数据对同工作区成员（**含 guest**）可见，窗口 **00:07–01:10 UTC、约一小时**；根因是性能优化里的 **variable shadowing bug** 本该回退到原始权限解析器，却把用户级 resolver 全部省略，查询「silently returned results without team-level filtering」。官方还特意声明这段代码「written and reviewed by engineers without AI assistance」，问题出在测试覆盖。回滚在一小时内完成，**邮件通知在 3 月 26 日（≈48 小时后）**。（来源 22）【事实】

**事故二与花边**：2024-01-24 数据库迁移误删生产数据，停机约五小时、恢复持续两天，官方称其为「the largest in Linear's five-year history」；2022 年上线期还遭遇过 DDoS（CEO 在 HN 主帖里当场承认），Better Stack 的事故聚合页累计 25 起记录，其中一起是「DDoS protection rules applied in error」，官方 blog 留有完整复盘可查。（来源 23/27/24/47）【事实】

**投诉面要打两折看**：saasflags 页面自述「Our AI scanner… **paraphrases** what it finds」，累计 11 条投诉，条目里还残留「Plus plan at $14」「$8–$14 per seat」这类旧档位名——**引语非逐字、数据未随涨价更新**，本文只登记不引用其引号内容。（来源 35）【存疑】能独立核到的是这些机制性事实：年付中途取消不退款（停用席位的 credit 只抵未来账单、不折现）、Free 撞 250 issue 墙后无法新建、CSV 导出不含评论与附件、邮件支持响应 48–96 小时（后者源自 G2/Capterra 转述，仍属存疑）。（来源 6/35）【事实/存疑】

**三个打架的第三方数**：northmetric 用流量与价目建模出「~$71.8M ARR」，与官方 $100M 互斥（来源 34）【存疑】；Latka 页面直接写着「Linear App **was acquired**」并以「before its buyer bought it」叙述 $100M ARR——与官方仍在独立运营、tender 系员工卖股权的事实矛盾（来源 33/7）【存疑】；eleken 乙方稿通篇停在「$400M valuation」的 2023 年旧数（来源 36）【存疑】；925studios 的设计拆解同属乙方稿（来源 45）【存疑】；另有三处取证空壳登记在此：Designer Fund 访谈取证日 429（来源 46）、官方旧 AI 路径 /product/ai 只回 17 字节（来源 54）、docs 页为 JS 壳（来源 51），正文均未取到、不采信。【存疑】**【观点】卖数据的站、做案例的乙方、AI 改写的投诉页，各自有各自的利益；把三个数并排放出来，比挑一个信更有信息量。**

## 结论：画布上最值钱的一格，是「不再需要新钱」

**可抄其一，把涨价放在没人看的地方**。$8→$10 涨在 2025 年 10-11 月之间，没有公告、没有生效日期，官网现行价就是新价——它是被 Wayback 和比价站抓出来的，不是被自己说出来的。（来源 3/4/5）【事实】这招有效但有代价：被档案库抓住的静默涨价，可信度成本会记在品牌账上。（来源 5）【观点】

**可抄其二，用方法论卖软件**。Linear Method 公开反对 user story、主张自写 issue，changelog 半月一更，《Issue tracking is dead》这种「CEO 宣布自己品类死亡」的檄文，本质是把内容当渠道用——它换来 The Register 跟进、HN 199 分讨论和 95% 付费工作区装上 agent 的转化面。（来源 21/48/14/15/29/7）【事实】

**可抄的坑，是把美学当免责**。variable shadowing 事故里，权限边界失效一小时、两天后才发邮件，而这段代码还是「没用 AI 写的」——工艺叙事挡不住测试覆盖缺口。（来源 22）【事实】叠加「年付不退、导出锁数据、250 条墙」这些机制，Linear 的反差面提醒我们：**产品手感与客户关系是两格画布，手感再好也替代不了后者的条款设计**。（来源 6/35/2）【观点】

最后一句留给这门生意本身：一年内估值翻倍、公司账上一分新钱没进，$99M 全部流向员工与前员工的口袋——**当一家公司能同时对内宣布「不再需要融资」、对外宣布「issue tracking is dead」，它的商业画布最值钱的一格已经不是客户或渠道，而是「不需要资本也能继续涨」这个姿态本身**。（来源 7/8/14）【观点】

---

## 来源（取证日 2026-10-23，全部可点开）

1. Linear 首页（product development system for teams and agents）https://linear.app/ （截图取证）
2. Linear 定价页（Free/Basic $10/Business $16/Enterprise + 250 issue 墙）https://linear.app/pricing （截图取证）
3. Wayback 2025-10-07 定价快照（Basic $8 / Business $14）https://web.archive.org/web/20251007142418id_/https://linear.app/pricing
4. Wayback 2025-11-13 定价快照（Basic $10 / Business $16）https://web.archive.org/web/20251113092935id_/https://linear.app/pricing
5. getpricepulse · Linear Price Increase（SEO 比价站，窗口 10-07～11-12）https://www.getpricepulse.com/blog/linear-price-increase-2026.html
6. WorkManagementHub · Linear Pricing 2026（计费口径：任何角色未停用席位均计费、月付价不作大标题）https://workmanagementhub.com/linear-pricing-2026/
7. 官方 now · Sharing Linear's growth with the people building it（$99M tender @$2.5B / $100M ARR / 40,000 公司 / NRR 177% / 95% agent）https://linear.app/now/sharing-growth-with-the-people-building-linear
8. Dealroom · Linear doubles valuation to $2.5B（$99M 系 secondary、归员工）https://dealroom.co/news/147084-linear-doubles-valuation-to-2-5b-defying-the-saas-is-dead-narrative/
9. 官方 · Building our way: Announcing our Series C（$82M @ $1.25B，Accel 领）https://linear.app/now/building-our-way
10. 官方 · Linear raises $35M Series B（2023-09-14，negative lifetime burn rate）https://linear.app/now/series-b
11. 官方 · The profitable startup（发布 12 个月盈利、100 内测用户转化）https://linear.app/now/the-profitable-startup
12. 官方 About（2019 成立、Karri CEO / Tuomas CTO、40,000 公司）https://linear.app/about
13. 官方 Careers（11 条原则 + 新加坡企业销售/CSM 岗）https://linear.app/careers
14. 官方 · Issue tracking is dead（2026-03-24，75%+ / 5x / 近 25% issue）https://linear.app/next
15. The Register · Linear adopts agentic AI as CEO declares issue tracking dead（2026-03-26）https://assets.theregister.com/2026/03/26/linear_agent/
16. 官方 · The AI usage patterns in software teams / data 报告（N=127,000、12%→34%、3%→10%、21→65、just under half）https://linear.app/data （截图取证）
17. 官方 Customers（OpenAI 3,000 / Ramp 75% / Scale -52% / Remote 1,000 人 / 2.0x·3.3x·50% 自报数）https://linear.app/customers
18. 官方 changelog · Introducing Linear Agent（2026-03-24）https://linear.app/changelog/2026-03-24-introducing-linear-agent
19. 官方 AI 页（agent 上下文、Slack/Teams、skills、loops、Coding Sessions）https://linear.app/ai （截图取证）
20. 官方 Linear Method 索引 https://linear.app/method
21. Method · Write issues not user stories（「user stories are an anti-pattern」）https://linear.app/method/write-issues-not-user-stories
22. 官方 · Linear incident on Mar 24th, 2026（variable shadowing / 约一小时 / 3-26 邮件通知）https://linear.app/now/linear-incident-on-mar-24th-2026
23. 官方 · Post mortem on Linear incident from Jan 24th, 2024（迁移误删生产数据 / 五年最大事故）https://linear.app/blog/linear-incident-on-jan-24th-2024
24. Better Stack 事故聚合（25 起，含 DDoS 防护规则误配）https://apistatuscheck.com/incidents/linear
25. HN · Linear – A fast issue tracker（491 分 / 221 评论）https://news.ycombinator.com/item?id=23693029
26. HN · Settings are not a design failure（488 分 / 378 评论）https://news.ycombinator.com/item?id=30181291
27. HN · Linear.app（212 分 / 125 评论，Karri 亲述 DDoS；50ms 与离线白屏的正反评）https://news.ycombinator.com/item?id=33199304
28. HN · AI coding has made CI a bottleneck（317 分 / 408 评论）https://news.ycombinator.com/item?id=49792067
29. HN · AI usage patterns in software teams（199 分 / 115 评论）https://news.ycombinator.com/item?id=49353432
30. HN · The profitable startup（248 分）https://news.ycombinator.com/item?id=45778984
31. HN Algolia 检索（分数与评论数对表）https://hn.algolia.com/api/v1/search?query=linear.app&tags=story&hitsPerPage=30
32. Sacra · Linear revenue, valuation & funding（$100M ARR、blended ARPA ≈$2,500、NRR 145%→177%，付费墙估算）https://sacra.com/c/linear/
33. Latka · Linear App（$134.2M 四轮、118 人(X)、headcount 四读数、「was acquired」口径）https://getlatka.com/companies/linear.app
34. northmetric · Linear Revenue & ARR Estimate（模型估算 ~$71.8M，与官方 $100M 互斥）https://www.northmetric.io/estimate/linear
35. saasflags · Linear complaints（11 条、自述 AI 扫描改写、残留旧档位名）https://saasflags.com/products/linear/complaints
36. eleken · Linear App Case Study（乙方稿，通篇「$400M 估值」旧数）https://www.eleken.co/blog-posts/linear-app-case-study
37. ValueAdd VC · How does Linear make money（引 Forbes 2023：$36,000 营销费 / 1 名销售；$134.2M、13–21 席反推的基础数）https://valueaddvc.com/blog/how-does-linear-make-money-per-seat-pricing-product-led-growth-and-the-2b-valuation-breakdown
38. toolchew · Linear vs Jira 2026（$7.91/$14.54 vs $10/$16 比价）https://toolchew.com/en/linear-vs-jira/
39. Atlassian · Q4 & FY2026 Results（订阅 ARR $6,606M +23%、Q4 $1,766M、FY26 $6,572M、350k+ 客户、MCP 1M MAU）https://www.nasdaq.com/press-release/atlassian-announces-fourth-quarter-and-fiscal-year-2026-results-2026-08-06
40. creativerly · Height app is shutting down（$18.3M 总融资、$14M A 轮、2025-09-24 停服）https://www.creativerly.com/height-app-is-shutting-down/
41. performance.dev · How Linear is so fast（几毫秒 vs 300ms、React/TS/MobX/Postgres、lazy-hydrate）https://performance.dev/how-is-linear-so-fast-a-technical-breakdown
42. iocombats · Linear sync engine local-first architecture https://iocombats.com/blogs/linear-sync-engine-local-first-architecture-explained
43. billmill · How is Linear so fast（笔记）https://notes.billmill.org/link_blog/2026/06/How_s_Linear_so_fast_.html
44. 官方 · Scaling the Linear Sync Engine（Tuomas Artman，2023-06-29）https://linear.app/now/scaling-the-linear-sync-engine
45. 925studios · Linear design breakdown（乙方设计稿）https://www.925studios.co/blog/linear-design-breakdown-saas-ui-2026
46. Designer Fund · AI × Linear（取证日 429，正文未取到，不采信其内容）https://designerfund.substack.com/p/ai-design-linear
47. 官方 Blog https://linear.app/blog
48. 官方 Changelog（2026 年半月节奏）https://linear.app/changelog
49. 官方 Now 索引 https://linear.app/now
50. 官方 Method · Introduction https://linear.app/method/introduction
51. 官方 Docs（取证日为 JS 壳，仅登记）https://linear.app/docs
52. HN · Linear is always a lagging indicator（14 分 / 9 评论）https://news.ycombinator.com/item?id=48913953
53. 官方 Method · Principles and practices https://linear.app/method/principles-and-practices/introduction
54. 官方旧路径 /product/ai（取证日仅回 17 字节壳，正文未取到）https://linear.app/product/ai
55. Wayback CDX · linear.app/pricing 快照索引（涨价窗口时间戳可复核）https://web.archive.org/cdx/search/cdx?url=linear.app%2Fpricing&output=json&fl=timestamp,original,statuscode&limit=50

取证档案：`research/evidence/linear/`（**121 个文件 / 55 条唯一来源 URL**，manifest 带 sha256 与通道标注；通道 = 通道A 正文抓取 + Wayback 快照直取 + HN 页面/Algolia + 四链搜索 + 通道D 截图 4 张）。

## 勘误与可撤回

- 本文任一判断被一手源推翻，请在来信指出处；核实后在本文对应句子后加「（勘误 <日期>）」并保留原句划线，**不删旧数字、不改历史口径**。
- 已知存疑项：$71.8M ARR（northmetric 模型估算）、「Linear App was acquired」（Latka 口径，与官方在营事实矛盾）、「$400M 估值」（eleken 旧数）、「拿下 Jira 30% 份额」（tech-insider，无出处、未归档）、「CEO Peter Yang」（proudfrog，人名错误）；员工数 118/178/88/42 四读数互斥；邮件支持 48–96 小时（G2/Capterra 转述）、投诉页全部引号内容（saasflags 自述 AI 改写）；Sacra 的 NRR 145%→177% 与 ARPA $2,500、toolchew 比价、2.0x/3.3x/50% 客户效果数（官方单一口径）；官方 /data 报告为当事方自发布、口径自定义；Designer Fund 429、/product/ai 空壳、docs 为 JS 壳，三处正文未取到。这些点**未当作事实使用**。【存疑】

> 本站为独立技术分析，与文中项目无商业关系。
