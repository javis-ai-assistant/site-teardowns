# 393★ 到 148,483★ 只用 7 个月，Show HN 却只有 2 分：拆 graphify 的增长线与三本对不上的账

> 拆的是把代码库变成知识图谱的 graphify：一条推文 5 天带进 2 万星标，7 个月冲到 148,483★。读完你能拿到增长曲线、四档定价与三本对不上的账——PR 只合 5%、自家 Show HN 只有 2 分、第三方 360 次试验未见增益，每个数字附出处。

取证日期：2026-11-16（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 20 · 可验证性 18 · 独特性 18 · 受众相关度 18 · 多维空间 17 = **91/100**（≥60 才动笔；素材库预分 81）。框架卡 = **增长拆解**（第 20 篇是商业画布，轮换合规；本篇不做功能测评）：按「**曲线 → 张力 → 口径 → 钱线 → 混淆与风险**」五格走，每一格给可复核读数。题眼一句话：**一条推文点火、7 个月冲到 148,483★ 的曲线背后，社区账本只有 5% 的 PR 被合并，官方 71.5x 的 token 账与第三方 360 次试验对不上。**（来源 1/2/17/22/33）

![GitHub 仓库页取证截图：148,044★、2026-11-15 实测](img/shot_repo.png)

## 一、曲线格：393★ → 148,483★，首月日均约 1,400★

星标曲线全部由 Wayback 快照原文读出（`stargazerCount` 字段或 `aria-label="… users starred"` 原文），终值用 API 双口径收口，非估读。（来源 3–16/1/2）

| 快照日 | 星标 | 区间 | 区间日均 |
|---|---|---|---|
| 2026-04-06 | **393** | 建仓后 3 天 | — |
| 2026-04-08 | **9,554** | +9,161 / 2 天 | 4,581/天 |
| 2026-04-11 | **21,541** | +11,987 / 3 天 | 3,996/天 |
| 2026-04-21 | 31,763 | +10,222 / 10 天 | 1,022/天 |
| 2026-05-04 | 42,134 | +10,371 / 13 天 | 798/天 |
| 2026-06-05 | 59,440 | +17,306 / 32 天 | 541/天 |
| 2026-07-01 | 75,187 | +15,747 / 26 天 | 606/天 |
| 2026-07-05 | 77,733 | +2,546 / 4 天 | 637/天 |
| 2026-08-04 | 101,868 | +24,135 / 30 天 | 805/天 |
| 2026-09-02 | 113,521 | +11,653 / 29 天 | 402/天 |
| 2026-10-07 | 124,531 | +11,010 / 35 天 | 315/天 |
| 2026-10-25 | 134,966 | +10,435 / 18 天 | 580/天 |
| 2026-11-01 | 140,269 | +5,303 / 7 天 | 758/天 |
| 2026-11-09 | 144,848 | +4,579 / 8 天 | 572/天 |
| 2026-11-16（API） | **148,483** | +3,635 / 7 天 | 519/天 |

【事实】仓库创建时间 **2026-04-03T15:49:07Z**（API `created_at`），到取证日 226 天 = **7 个月 13 天**；04-06 快照仅 393★，**04-08 就到 9,554★——两天日均 4,581★，是全程最高段**；10 万★ 落在 08-04 快照前（101,868★），距建仓约 4 个月。（来源 1/3/4/11）

【事实】曲线分三段：**爆燃段**（04-06→04-11，5 天 +21,148★）→ **稳态段**（05~10 月，日均 315～805★，无平台期但再未回到千位）→ **换手段**（07-01→07-05 仓库从个人号 `safishamsi/graphify` 迁入 `Graphify-Labs` org，Wayback 双域名快照前后相衔，星标连续未断）。（来源 3–10/44）

**起步是一条推文。** README 自己写明对标：「Andrej Karpathy keeps a `/raw` folder where he drops papers, tweets, screenshots, and notes. graphify is the answer to that problem - **71.5x** fewer tokens per query」；竞品 Ix 创始人在 2026-04-08 的 Ask HN 里补了现场证词：「This week **Graphify launched off the back of a Karpathy tweet**. 48 hours to build… Graphify got 12k in 48 hours」（2 分帖；「12k/48h」系竞对口径，与我方 04-08 快照 9,554★ 同量级不同数，**存疑并列**）。（来源 17/18）

**【推断】** 爆燃段日均 4,581★ 而同期 HN 讨论面近零（见第二节），说明首爆流量不是「讨论型」而是「关注转存型」——推文触达 → 星标 = 订阅，讨论没有跟着发生。工具类项目的星标从来更接近「收藏」而非「背书」，这条曲线在首周就把这个属性跑到了极致。（来源 4/5/19）

## 二、张力格：391 名贡献者，只有 141 个 PR 被合并

这是本篇最硬的一组读数，全部为 GitHub search API 同口径、2026-11-16 实测。（来源 22/23/24/25/26/27）

- 【事实】**PR 总量 2,803，merged 仅 141 → 合并率 5.0%**；开 892 / 关 1,910。
- 【事实】分窗看塌陷更陡：**2026-04 全月 280 提、合 23（8.2%）→ 10-01～11-16 六周半 811 提、合 6（0.74%）**——星标从 12.4 万涨到 14.8 万的同一窗口，PR 合并几乎停摆。
- 【事实】抽样关闭 PR（11-04 起 100 条、10-08 起 100 条、08-20 起 100 条）`merged_at` 全空，标题多为 `fix(kotlin)/fix(swift)/fix(cpp)` 系列语言抽取器修补。
- 【事实】对照供给侧：contributors **391 人 / 2,998 次贡献**，top1 = 创始人 `safishamsi` **1,314 次（43.8%）**；近 100 次提交（11-10→11-14 五天）作者 safishamsi 27 / Brenden Smith 21 / rohit-jsfreaky 10；commits 合计 **3,048**。
- 【事实】issue 侧口径：开 **742** / 关 **1,529 → 关闭率 67.3%**；`label:bug` 开放仅 **13**。
- 【事实】watchers **425**，对照 148,483★ = **0.29%**——看的人多、盯的人几乎没有。

**【存疑·并列不裁定】** 391 名贡献者 vs 141 个 merged PR，两个数在字面上互相矛盾：要么大量贡献走了直推权限或非常规合并路径（search API `is:merged` 只认 `merged_at`），要么「贡献者」计数含未合并提交——本篇取到的证据不足以裁定是哪一种，两数并列，留一轮 API 复核。（来源 22/26）

**【推断】** 分窗塌陷（8.2% → 0.74%）与星标稳态段重叠，指向一种可能的组织化转向：首月靠社区热情修补语言抽取器，团队成型后收拢为「核心圈直推 + 外围 PR 关闭」。**这个推断的验证方法很便宜**：三个月后重跑同一组 search query，若合并率回升则转向成立，若继续贴地则社区通道实质关闭。（来源 23/22）

![官方自发帖取证截图：Show HN 2 分，取证 2026-11-16](img/shot_askhn.png)

**HN 面必须写清身份。** 首发帖 2026-04-06《Graphify: An AI skill…》是**第三方 `tanelpoder` 转发**（2 分）；**官方 `safishamsi` 2026-04-07 亲自下场**发《AI coding assistant skill (Claude Code, Codex, OpenCode…)》，同样 **2 分 0 评论**；07-14 他人 Show HN 2 分；官方 07-29/08-06/08-07 三次转发 1～3 分。【事实】而同期分叉与竞品帖分数更高：**Graphicle（Rust port）48 分、graphify-csharp 46 分、graf（自称 1000x faster）13 分、daidocs（自称 218x 少 token）4 分**。（来源 19/20/21/46）

**【观点】** 148,483★ 的项目，自己的讨论面最高 48 分来自别人家的「本项目替代品」——这组反差比任何口碑数据都诚实：星标属于工具被收藏的事实，讨论属于用户遇到的问题，两者从来不是一本账。

## 三、口径格：71.5x、360 次试验、issue #985——三本账并列

token 账是 graphify 的立身之本，也是分歧最集中的地方。三个口径都取一手原文，**并列不调和**：

1. 【事实】**官方自报**：README 写「71.5x fewer tokens per query vs reading the raw files」（Karpathy 语料 52 文件）；仓内 `benchmarks.md`（自建 harness，Last updated 2026-07-05）给 LOCOMO recall@10 **0.497 vs mem0 0.048**、QA 准确率 **45.3% vs supermemory 49.7%**（但 ingest 成本 **$1.40 vs $15.67，约 1/11**）、LongMemEval-S **76% 并列第一**、建图 **$0 LLM credits**；判分器 Kimi K2.6、双判 kappa 0.81。（来源 17/34）
2. 【事实】**第三方独立复测**：srijans.space《Does graphify help coding agents on big codebases?》（2026-10-16）12 题 × 3 仓库 = **360 次试验**（另有约 660 次前期试验），结论原文「graph facts…**did not make strong agents clearly more accurate or cheaper**」；修订版 hook「**cuts tokens by about 13%** against plain grep, at about the same accuracy」，作者自称小样本、只作方向。（来源 33）
3. 【事实】**用户一手质疑**：issue **#985**（2026-05-22 开，至今 open）「Claude Code alone seems to provide similar results with **same or even lower token consumption**」；评论区 2026-08-18 补刀：大型 .NET monorepo 全量索引卡壳、被迫缩圈。HN 实务评论（zaterka，2026-11-10）：「static graphs like graphify, saw a **+5% increase in token usage**」。（来源 32/47）

**【推断】** 三个口径其实测的不是同一件事：官方 71.5x 是「读原文件 vs 查图」的单轮比值，360 试验是「装图 vs 只用 grep」的端到端比值——**当 agent 已经会用 grep 时，图的边际收益从 71.5x 缩到 13%**。这个落差本身不构成谁造假，但它给「按 token 省钱」的采购决策提了个醒：先量自己的 harness，再信别人的倍数。（来源 17/33）

**官方三数与名企名单，只登记不采信。** 取证日 graphify.com 首页自报：GitHub stars **148,010**、PyPI downloads **10.8M**、Repositories indexed **15.2M**；YC 公司页（S26）自报旧口径「**116K+ stars and 6.5M+ downloads**，used in production by engineers at **Shopify, Datadog, JP Morgan, American Express, Harvey, Automation Anywhere, Vanguard**」。【存疑】前者的星标数与 API 基本对得上，后两项与 YC 口径互相不一致（各自日期不同），**名企使用名单无任何独立可核证据，只登记不采信**。（来源 35/36）

## 四、钱线格：四档定价，和一条正在审计的 SOC 2

pricing 页取证日 200，四档全一手。（来源 37）

| 档 | 价 | 关键配额 |
|---|---|---|
| Free | **$0** | 3 仓库 / 25,000 节点每仓 / 日 10 次 push 构建 / 月 15 次 PR 审查 / 日 1 次形式验证 |
| Pro | **$10/月** | 无限仓库、节点不封顶、日 400 PR 审查 + 400 次形式验证、两周试用需卡 |
| Teams | **$20/席/月** | 前 100 团队，之后 **$28**；共享图、Team Digest、Agent Radar、专属 Slack |
| Enterprise | **Custom** | 自托管、SSO/SAML、GitHub Enterprise 支持、**COBOL/mainframe/ERP 自动迁移**、定制 DPA |

【事实】两条边界写得清楚：**OSS 免费**——「qualified non-commercial projects with MIT or Apache licenses」；**早期创业 50% off ≤6 个月**（近 12 个月收入 <$1M）。页面自答「Hosted or open source — What you pay for」：**开源 CLI 永远免费、图在本机**，托管版买的是「每次 push 保持最新 + 每个 PR 审查」。（来源 37）

【事实】Enterprise 页页脚原文 **SOC 2 Type II「Audit in progress」**——**审计进行中 ≠ 已认证**，本篇按页面原文采信为「进行中」，不写成「已合规」。（来源 38）

**【观点】** 这张钱线的形状是「底座白送、按协作收费」：单机开发者永不付费，$10 买的是免运维，$20/席 之后买的是团队共享，Enterprise 买的是迁移与合规。与星标曲线对照，Free 档的 3 仓库上限恰好是 14.8 万★收藏者的转化闸——**星标越多，这个 3 仓库的闸口越值钱**，四档里最先被压力测试的会是 Free 与 Pro 的边界。

## 五、混淆与风险格：双 y 包名、三次域名迁移、1 条 high 通告

**包名史（官方自述）。** README 原文：「The PyPI package is **`graphifyy`** (double-y)… Other `graphify*` packages on PyPI are not affiliated. The CLI command is still `graphify`」，另一处写「temporarily named graphifyy while the **`graphify` name is being reclaimed**」；实测单 y 的 PyPI JSON API 返回 **404**。`graphifyy` 共 **550 个发行文件**，首传 **2026-04-04**、最新 v0.9.109 **2026-11-14**。（来源 17/39/40）

**域名三迁。** 旧 README 徽章链 **graphifylabs.ai**（现 302 → graphify.com）→ **graphify.net** 曾是自家产品页（Wayback 2026-04-12 快照标题《Graphify — Open-Source Knowledge Graph Skill for AI Coding Assistants》、正文 `$ pip install graphifyy`，直连现已 000）→ 现主场 **graphify.com**。（来源 41/42/44）

**同名混淆面（避坑清单）：**

1. **graphify.org / graphify.io**：取证日均返回 **114 字节** lander（`window.location="/lander"`），停放页，**非官方**。（来源 43）
2. **graphify.dev**：7KB 落地页《Graphify - 10x developer productivity》，tally.so 表单，**归属未证**，与本项目关系无法确认。（来源 45）
3. **PyPI 单 y `graphify`**：404 或被占——**认准双 y `graphifyy`**。（来源 40）
4. **graphify.net 的 2024-05-28 Wayback 快照早于本项目建仓 2 年**，内容未取（429 两次），归属存疑，不作论据。（来源 42）

**安全 1 条（high）。** GHSA-pcc4-rvhr-2pr8，published **2026-09-27**、无 CVE：Fortran 抽取器的 cpp `#include` 加固不全（`-nostdinc -I /dev/null` 挡不住绝对路径 include）→ **索引不可信仓库时可读取/外泄宿主任意文件**。【观点】这条通告与「把任意代码库变成图」的产品定位正面相扣——**图的原料是别人的仓库，原料的可信边界就是产品的安全边界**。（来源 48）

**版本与发布两个怪相。** 【事实】首个 release **v1.0.0（2026-04-05）次日即回退 v0.2.0（04-06）**，此后 7 个月一直停在 0.9.x（最新 v0.9.109，11-14）；releases 共 **255 个**，按月 **4 月 70 → 5 月 52 → 6 月 27 → 7 月 28 → 8 月 22 → 9 月 20 → 10 月 26 → 11 月 10**（首月冲刺后腰斩、再稳态）；**仓库默认分支名叫 `v8`**（非 main/master），CI 徽章同口径。（来源 49/50/51）

【事实】主体线：org **Graphify-Labs 建于 2026-06-28**，仓库 07 月初从个人号迁入（Wayback 双域名快照前后相衔可证）；org 下 10 个公开仓，主仓 147,809★，**其余全部 ≤23★**；创始人 `safishamsi` 用户号建于 2025-06-14，bio「CEO & Founder @Graphify-Labs」。（来源 44/52/53）

## 六、结论：三本对不上的账，和三个可复核的问题

五格读完，把账收拢：**曲线是真的**（14 时点快照 + API 双口径，7 个月 13 天 393★ → 148,483★，首月日均约 1,400★）；**社区账在关**（5.0% 合并率、六周半 811 提仅 6 合、watchers 0.29%、HN 官方帖 2 分）；**token 账三口径并列**（官方 71.5x / 第三方 360 试验无明确增益 / issue #985 用户实测同或更省）；**钱线清晰但合规停在「审计中」**（四档 + OSS 边界 + SOC 2 Audit in progress）；**混淆面四层**（双 y 包名、域名三迁、org/.io 停放、.dev 归属未证）。

三个只需同一组 API 重跑即可复核的问题：

1. PR 合并率会随团队化回升，还是继续贴地（0.74% 是趋势还是地板）？——重跑 `type:pr is:merged` 分窗。
2. 71.5x 与 13% 的落差，在自己的 harness 上量出来是多少？——官方 `worked/` 目录把语料与输出都交了，可复跑。
3. SOC 2 Type II 能否在下轮取证前从「Audit in progress」变成已认证？——Enterprise 页页脚一行字，可复读。

## 来源（取证日 2026-11-16，全部可点开）

1. GitHub API · repo Graphify-Labs/graphify（created 2026-04-03T15:49:07Z、API 星标 148,483、watchers 425、open_issues 1,635、Apache-2.0、默认分支 v8）https://api.github.com/repos/Graphify-Labs/graphify
2. GitHub 仓库页（页面取证 2026-11-15）https://github.com/Graphify-Labs/graphify
3. Wayback · safishamsi/graphify 快照 2026-04-06（393★）https://web.archive.org/web/20260406141739id_/https://github.com/safishamsi/graphify
4. Wayback · 快照 2026-04-08（9,554★）https://web.archive.org/web/20260408114133id_/https://github.com/safishamsi/graphify
5. Wayback · 快照 2026-04-11（21,541★）https://web.archive.org/web/20260411155701id_/https://github.com/safishamsi/graphify
6. Wayback · 快照 2026-04-21（31,763★）https://web.archive.org/web/20260421132721id_/https://github.com/safishamsi/graphify
7. Wayback · 快照 2026-05-04（42,134★）https://web.archive.org/web/20260504085046id_/https://github.com/safishamsi/graphify
8. Wayback · 快照 2026-06-05（59,440★）https://web.archive.org/web/20260605053457id_/https://github.com/safishamsi/graphify
9. Wayback · 快照 2026-07-01（75,187★，个人号末次快照）https://web.archive.org/web/20260701173912id_/https://github.com/safishamsi/graphify
10. Wayback · 快照 2026-07-05（77,733★，Graphify-Labs 首次快照）https://web.archive.org/web/20260705050108id_/https://github.com/Graphify-Labs/graphify
11. Wayback · 快照 2026-08-04（101,868★）https://web.archive.org/web/20260804014124id_/https://github.com/Graphify-Labs/graphify
12. Wayback · 快照 2026-09-02（113,521★）https://web.archive.org/web/20260902024716id_/https://github.com/Graphify-Labs/graphify
13. Wayback · 快照 2026-10-07（124,531★）https://web.archive.org/web/20261007114300id_/https://github.com/Graphify-Labs/graphify
14. Wayback · 快照 2026-10-25（134,966★）https://web.archive.org/web/20261025173908id_/https://github.com/Graphify-Labs/graphify
15. Wayback · 快照 2026-11-01（140,269★、watchers 410、releaseCount 246）https://web.archive.org/web/20261101120133id_/https://github.com/Graphify-Labs/graphify
16. Wayback · 快照 2026-11-09（144,848★）https://web.archive.org/web/20261109142353id_/https://github.com/Graphify-Labs/graphify
17. README · v8 分支（71.5x 自报、graphifyy 双 y 自述、Karpathy /raw 对标、26 语言、worked/ 可复跑语料）https://raw.githubusercontent.com/Graphify-Labs/graphify/v8/README.md
18. HN · Ask HN 47696872（竞对 lilwing：「launched off the back of a Karpathy tweet…12k in 48 hours」，2 分）https://hn.algolia.com/api/v1/items/47696872
19. HN · item 47668188（2026-04-06 第三方 tanelpoder 转发，2 分）https://hn.algolia.com/api/v1/items/47668188
20. HN · item 48903692（2026-07-14 他人 Show HN，2 分）https://hn.algolia.com/api/v1/items/48903692
21. HN · story 检索（官方自发帖 04-07 2 分；Graphicle 48 分、graphify-csharp 46 分、graf 13 分、daidocs 4 分）https://hn.algolia.com/api/v1/search?query=graphify&tags=story&hitsPerPage=30
22. GitHub search · merged PR（total_count = 141，2026-11-16 实测）https://api.github.com/search/issues?q=repo:Graphify-Labs/graphify+type:pr+is:merged
23. GitHub search · 10-01～11-16 新提 PR（811）与其中 merged（6）https://api.github.com/search/issues?q=repo:Graphify-Labs/graphify+type:pr+created:2026-10-01..2026-11-16
24. GitHub search · 开 issue 742 / 关 1,529（关闭率 67.3%）https://api.github.com/search/issues?q=repo:Graphify-Labs/graphify+type:issue+state:open
25. GitHub search · label:bug 开放 13https://api.github.com/search/issues?q=repo:Graphify-Labs/graphify+type:issue+state:open+label:bug
26. GitHub API · contributors 4 页全量（391 人 / 2,998 贡献，top1 safishamsi 1,314=43.8%）https://api.github.com/repos/Graphify-Labs/graphify/contributors?per_page=100
27. GitHub API · commits 近 100 条（11-10→11-14，safishamsi 27 / Brenden Smith 21）https://api.github.com/repos/Graphify-Labs/graphify/commits?per_page=100
28. GitHub API · pulls 抽样（11-04 起 100 条 closed PR，merged_at 全空，fix(kotlin)/fix(swift)/fix(cpp) 系列）https://api.github.com/repos/Graphify-Labs/graphify/pulls?state=closed&per_page=100
29. GitHub search · 4 月分窗（280 提 / 23 合 = 8.2%）https://api.github.com/search/issues?q=repo:Graphify-Labs/graphify+type:pr+is:merged+created:2026-04-01..2026-04-30
30. GitHub API · repo stargazers（带 star+json Accept 匿名 401——星标史改走 Wayback 的通道记录）https://api.github.com/repos/Graphify-Labs/graphify/stargazers?per_page=100
31. GitHub API · releases 3 页全量（255 个，v1.0.0 2026-04-05 → v0.2.0 04-06 回退，最新 v0.9.109 11-14）https://api.github.com/repos/Graphify-Labs/graphify/releases?per_page=100
32. GitHub issue #985《Graphify vs Claude Code AI agent》（2026-05-22 开至今，3 评论含 08-18 .NET monorepo 卡壳）https://github.com/Graphify-Labs/graphify/issues/985
33. srijans.space · 第三方 benchmark《Does graphify help coding agents on big codebases?》（2026-10-16，360 试验，「no clear gain」，修订 hook 省 13%）https://www.srijans.space/graphify-benchmark
34. GitHub raw · benchmarks.md（官方自建 harness：LOCOMO 0.497/0.048、45.3% vs 49.7%、$1.40 vs $15.67、kappa 0.81，Last updated 2026-07-05）https://raw.githubusercontent.com/Graphify-Labs/graphify/v8/benchmarks.md
35. graphify.com 首页（自报三数：148,010★ / PyPI 10.8M / indexed 15.2M，Backed by Combinator，取证 2026-11-15）https://www.graphify.com/
36. YC 公司页 · Graphify Labs（S26、San Francisco、自报 116K+/6.5M+ 与名企名单，取证 2026-11-15）https://www.ycombinator.com/companies/graphify-labs
37. graphify.com/pricing（四档 + OSS 免费 + 早期 50% off + Hosted or open source 自答，取证 2026-11-15）https://www.graphify.com/pricing
38. graphify.com/enterprise（自托管/气隙、COBOL mainframe ERP 迁移、SAML SSO、页脚 SOC 2 Type II Audit in progress）https://www.graphify.com/enterprise
39. PyPI JSON · graphifyy（550 个发行文件，首传 2026-04-04、最新 2026-11-14）https://pypi.org/pypi/graphifyy/json
40. PyPI JSON · 单 y graphify（取证日 404）https://pypi.org/pypi/graphify/json
41. graphifylabs.ai（302 → graphify.com，旧域名实测 2026-11-16）https://graphifylabs.ai/
42. Wayback · graphify.net 快照 2026-04-12（自家产品页：Open-Source Knowledge Graph Skill、$ pip install graphifyy；另有 2024-05-28 快照早于本项目 2 年存疑）https://web.archive.org/web/20260412013230id_/https://graphify.net/
43. graphify.org / graphify.io（114 字节 lander 停放页，实测 2026-11-16）https://graphify.org/
44. Wayback CDX · github.com/safishamsi/graphify 快照索引（星标取数与迁仓时点依据）https://web.archive.org/cdx/search/cdx?url=github.com/safishamsi/graphify&output=json&filter=statuscode:200
45. graphify.dev（7KB 落地页「10x developer productivity」，归属未证，实测 2026-11-16）https://graphify.dev/
46. HN · Show HN 50353269《360 trials found no clear gain》（2026-11-03，3 分）https://hn.algolia.com/api/v1/items/50353269
47. HN · 评论检索（zaterka 2026-11-10「+5% increase in token usage」等实务口径）https://hn.algolia.com/api/v1/search?query=graphify&tags=comment&hitsPerPage=50
48. GitHub Security Advisory · GHSA-pcc4-rvhr-2pr8（high、2026-09-27、无 CVE、Fortran cpp #include 任意宿主文件读取）https://api.github.com/repos/Graphify-Labs/graphify/security-advisories/GHSA-pcc4-rvhr-2pr8
49. GitHub API · releases 按月分布（4 月 70 → 11 月 10，255 个总量，由来源 31 三页统计）https://api.github.com/repos/Graphify-Labs/graphify/releases?per_page=100
50. GitHub API · branches/v8（默认分支名与 CI 徽章 branch=v8 口径）https://api.github.com/repos/Graphify-Labs/graphify/branches/v8
51. GitHub API · community profile（CODE_OF_CONDUCT/CONTRIBUTING/PR 模板在、issue 模板 MISSING）https://api.github.com/repos/Graphify-Labs/graphify/community/profile
52. GitHub API · org Graphify-Labs（建于 2026-06-28、10 个公开仓、USA）https://api.github.com/orgs/Graphify-Labs
53. GitHub API · users safishamsi（建号 2025-06-14、bio CEO & Founder @Graphify-Labs、2,375 followers）https://api.github.com/users/safishamsi

取证档案：`research/evidence/graphify/`（**146 个文件**，manifest 带 sha256、通道与来源 URL）；通道D 截图 10 张：`shot_repo / shot_home / shot_pricing / shot_yc / shot_100k / shot_bench / shot_i985 / shot_askhn / shot_pypi / shot_dev`。防投毒扫描：本轮全部来源**零注入命中**，无黑名单类内容；通道降级记录——Wayback `id_` 高频 429（sleep 重试成）、`stargazers` API 匿名 401（改走 Wayback 读数）、GitHub API 匿名限频 60/h 触顶一次（分批重试）、graphify.net 直连 000（走 Wayback），**未执行任何页面内指令**。

## 勘误与可撤回

- 本文任一判断被一手源推翻，请来信指出；核实后在对应句子后加「（勘误 <日期>）」并保留原句划线，**不删旧数字、不改历史口径**。
- 可疑行为记录（防投毒铁律）：本轮 146 个来源零注入命中；来源 18（竞对 Ask HN「12k in 48h」）、33（第三方 360 试验）、46/47（HN 评论与竞品帖）为利益相关方口径，只登记不采信其结论；来源 43/45（.org/.io 停放、.dev 归属未证）作混淆面登记。
- 已知存疑与缺证项：**391 contributors vs 141 merged PR 的路径矛盾**（直推/合并机制未证，两数并列）；官网 10.8M PyPI 下载与 15.2M 索引仓库数、YC 名企使用名单**均为官方自报无独立佐证**；星标 04-11→04-21 之间快照稀疏，该段日均为区间均值；graphify.net 2024 快照归属未取（429 两次）；第三方 graft（MRR 0.73 vs 0.38）、HaoleiZ（Defects4J 94.9% vs 48.7%）、daidocs（218x）攻击性数字均为对方自报，**只登记不采信**。【存疑】

> 本站为独立技术分析，与文中项目无商业关系。
