# 9 个月 40 倍、星标只涨 5 倍：opencode 221,369★ 背后的三本账，只有两本能对上

> 本站为独立技术分析，与文中项目无商业关系。

取证日期：2026-10-25（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 20 · 可验证性 18 · 独特性 16 · 受众相关度 19 · 多维空间 17 = **90/100**（≥60 才动笔；素材库预分 86）。框架卡 = **增长拆解**（上一篇为技术栈考古，不连用）：本篇不写功能测评、不写技术栈，按「增长曲线 → 渠道 → 驱动事件 → 口径对账 → 增长的代价」这条主线走 —— 一个项目的数字涨得越快，越要先问一句：这把尺子量的是什么。

## 这条曲线长什么样：官方口径 9 个月 40 倍，同期星标 5 倍

opencode 官网首页在取证日挂着一句话：「With over **220,000** GitHub stars, **950** contributors, and over **20,000** commits, OpenCode is used and trusted by over **16M** developers every month.」（来源 1）【事实】这四个数字不是今天才有的，官网底部的三块统计图（Fig 1 GitHub Stars、Fig 2 Contributors、Fig 3 Monthly Devs）从 2026 年初就开始滚动更新，而 Wayback 把每次更新都拍了下来：

| Wayback 快照 | 官网宣称 stars | 官网宣称 contributors | 官网宣称 commits | **官网宣称 Monthly Devs** |
|---|---|---|---|---|
| 2025-06-20 / 2025-10-01 | 尚无大数字块 | — | — | — |
| 2026-01-01 | 41,000 | 450 | 6,000+ | **400,000** |
| 2026-04-01 | 120,000 | 800 | 10,000+ | **5M** |
| 2026-07-01 | 160,000 | 900 | 13,000+ | **7.5M** |
| 2026-10-01 | 208,000 | 950 | 13,000+ | **16M** |
| 2026-10-25（实抓） | 220,000 | 950 | 20,000+ | **16M** |

【事实】（来源 8/9/4/5/6/7/1，快照清单见来源 65）从 2026-01-01 到 2026-10-01 这 9 个月，**月活宣称从 400,000 涨到 16,000,000，是 40 倍**；同一张表里同期星标宣称从 41,000 涨到 208,000，**只有 5.07 倍**。（来源 4/7）【事实】同一页面、同一批字段、同一段时间，两条曲线差了近 8 倍的斜率。

星标的实测读数是另一套数字。GitHub 的 stargazers 明细接口取证日已失效（返回 404，对照组是同日 trending 页 200，说明是端点下线不是网络故障，来源 16），能核的只有档案馆与第三方的离散时点：

| 时点 | 星标读数 | 出处 |
|---|---|---|
| 2025-05-15（建仓后 15 天） | **206★** | （来源 10） |
| 2026-01-02 00:37（sst/opencode URL） | **45,255★** | （来源 11） |
| 2026-01-02 21:19（anomalyco/opencode URL） | **45,642★** | （来源 12） |
| 2026-10-01（官网宣称） | 208,000★ | （来源 7） |
| 2026-10-25（API 实测） | **221,369★** | （来源 2） |
| 2026-10-25（trending 周榜页） | 221,372★，**本周 +6,857★** | （来源 15） |

【事实】206 → 221,369，18 个月涨了 **1,074 倍**；取证当天它仍挂在 GitHub 周榜里，一周再涨 6,857 星。（来源 10/2/15/64）【事实】顺手记两个对表细节：官网 2026-01-01 宣称 41,000★，次日凌晨实测已是 45,255★，**宣称数字是向下取整且滞后一天**；仓库页显示 fork 29,626、watcher 858、`open_issues_count` 3,812。（来源 4/11/2/3）

![GitHub 仓库首屏取证截图：221,369★ / 29,626 fork / 20,893 Commits，取证 2026-10-25](img/repo.png)

**【推断】**把两张表并排，能读出一件不难猜但很少被写出来的事：**官网的月活曲线和 GitHub 的星标曲线不是同一种东西**。星标是 GitHub 上一次性点击、可第三方复核的计数；月活是官网自己维护的展示字段，没有公开的统计方法。前者 5 倍、后者 40 倍，说明这 9 个月里被放大的不是"关注人数"，而是某个由官网定义的量。（来源 4/5/6/7/2）这个量到底怎么算，是本文第 4、5 节要拆的两本账。

## 增长从哪来：渠道拆解，热度一半来自坏消息

先看曝光渠道的可核实部分。Hacker News 的头条检索（按分数排序）读出来是这样一张表：

| 日期 | 标题 | 分数 | 指向 |
|---|---|---|---|
| 2026-03-20 | OpenCode – Open source AI coding agent | **1,274** | 官网首页（来源 38） |
| 2026-10-17 | OpenCode Hacked: User credentials leaked | **975** | 官方泄露披露（来源 36） |
| 2026-10-14 | OpenCode 2.0 | **888** | anoma.ly 发布笔记（来源 39） |
| 2026-03-19 | Anthropic takes legal action against OpenCode | **483** | PR #18186（来源 40） |
| 2026-01-11 | Unauthenticated remote code execution in OpenCode | **432** | cy.md 安全报告（来源 41） |
| 2026-07-20 | Annoying and alarming things about OpenCode | **420** | wren.wtf 批评文（来源 42） |
| 2025-07-06 | Opencode: AI coding agent, built for the terminal | 319 | sst/opencode 首发（来源 43） |
| 2026-01-15 | Anthropic Explicitly Blocking OpenCode | 173 | gist 拦截 PoC（来源 44） |

【事实】（来源 37）八条头条里，**三条是安全事故或法务冲突，一条是要求别用它的长文**，正面的只有 2.0 发布与首页帖。（来源 36/39/40/41/42/43）【事实】另一条渠道是 GitHub trending 周榜：取证日 opencode 以 221,372★、单周 +6,857★ 在榜。（来源 15）

产品面还有三个可点开的入口。官网首页写明三种形态并存：终端、桌面应用、IDE 扩展，接 75+ 模型提供商，**且自带一组免费模型**（"Free models included"），不注册账号也能跑起来。（来源 1）文档站独立成面，README 提供十种语言版本（英/简/繁/韩/德/西/法/意/丹/日）。（来源 61/63）

![官方文档站取证截图：Intro 与左侧目录树，取证 2026-10-25](img/docs.png)

**【推断】**这条增长链里，最像"钩子"的是免费模型加零门槛安装：它把"想试试 AI 编码"的人直接留在自家账号体系里，而不是让人先去办一个 Anthropic 或 OpenAI 的订阅。（来源 1/50）【推断】HN 的坏消息头条客观上带来了同量级曝光——安全事故帖 975 分与批评文 420 分的关注度，和首页帖 1,274 分在同一个数量级，**负面报道在这个项目的增长里不是损耗，而是渠道之一**。（来源 36/42/38）这个推断只说明曝光同向，不说明事故带来正收益，两者的因果本轮无法取证。

## 驱动事件时间线：从 206★ 到 221,369★ 的 18 个月

把可核实的事件按时间排开，这条曲线的拐点很清楚：

| 时间 | 事件 | 可核证据 |
|---|---|---|
| 2025-04-30 | 仓库创建（sst/opencode） | API `created_at`（来源 2） |
| 2025-05-15 | 206★ | Wayback 快照（来源 10） |
| 2025-07-06 | HN 首发，319 分 | HN item（来源 43） |
| 2026-01-02 | **同日两个 URL 都可抓到 200**：00:37 sst/opencode 45,255★、21:19 anomalyco/opencode 首次入档 45,642★ | 双 Wayback + CDX（来源 11/12/13/14） |
| 2026-01-11 | 未授权 RCE 报告上 HN，432 分 | HN item（来源 41） |
| 2026-01-15 | Anthropic 点名拦截，173 分 | HN item + gist（来源 44/47） |
| 2026-01-24 | issue #10416《OpenCode is not private by default?》开题，63 评论 | API（来源 32） |
| 2026-03-19 | PR #18186「anthropic legal requests」19 分钟合并；HN 483 分 | API + HN（来源 48/49/40） |
| 2026-03-20 | 官网首页帖 1,274 分（本项目 HN 峰值） | HN item（来源 38） |
| 2026-07-20 | 《Stop Using OpenCode》420 分 | HN + 原文（来源 42/46） |
| 2026-08-10 | HN 用户预警「它会把你的数据泄露出去」 | HN 评论引用（来源 36） |
| 2026-10-14 | OpenCode 2.0 发布，888 分 | HN + anoma.ly（来源 39/57） |
| 2026-10-17 | 官方承认泄露事件，975 分 | 推文 + HN（来源 34/35/36） |
| 2026-10-25 | 取证日：221,369★、周榜 +6,857★、882 个 release | API + trending（来源 2/3/15） |

【事实】2026-01-02 那一天，`sst/opencode` 与 `anomalyco/opencode` 两个地址在档案馆里各有一张 200 快照，相隔约 21 小时、星数差 387；CDX 索引里 anomalyco 这个地址的最早记录正是 2026-01-02 21:19:59，而 sst 那个地址从 2025-05-15 起就持续有档。（来源 11/12/13/14）【事实】这把**主体迁移窗口锁死在 2026-01-02 前后**，不是传闻，是两条独立档案链的交点。

**【推断】**时间线读下来，这个项目的关注度和它的负面事件几乎同步：2026 年 1 月的 RCE 与拦截、3 月的法务 PR、7 月的"别再用它"、10 月的泄露，每一次都上 HN 头条，而星标曲线在这 18 个月里没有出现过回落的季度。（来源 41/44/40/42/34/10/2）开源项目的热度本来就对"出事"钝感，但**同一年里安全、法务、口碑三线同时告警、增长照走**的样本并不多见。（来源 36/40/46）

## 口径审计 ①：官网四个数字，两个能复现，两个不能

这是本文的核心。官网首页那句话里的四个数，逐个拿第三方口径对表：

| 官网口径 | 第三方复核 | 判定 |
|---|---|---|
| 220,000+ GitHub stars | API `stargazers_count` = **221,369**；trending 页 221,372 | ✅ 对得上（来源 2/15） |
| 20,000+ commits | 仓库页 **20,893 Commits** | ✅ 对得上（来源 3） |
| **950 contributors** | 默认分支注册贡献者 **455**；含匿名 git 身份 **1,069** | ⚠️ 两套口径都对不上（来源 17/18） |
| **16M monthly devs** | 无任何第三方口径；npm 官方 CLI 合计 **≈11.7M** | ⚠️ 自证数字（来源 19/20） |

贡献者这一栏值得多写几行。GitHub contributors 接口默认只统计**默认分支（dev）上的注册身份**：取证日翻到最后一页是第 5 页、55 人，**合计 455 人**；加上 `anon=true` 参数后翻到第 11 页、69 人，**合计 1,069 人**，其中匿名 git 提交身份显示为 `null`。（来源 17/18）【事实】455 与 1,069 分别在 950 的两侧，一个差 2.1 倍、一个差 1.13 倍——**没有任何一套可复现的口径能得出 950**。（来源 1/17/18）【存疑】

还有第二处对表，来自官网自己的页面。2026-04-01 的 Wayback 快照里，除了可见文案，还留着一段服务端渲染的取数载荷：`stars:134293`、`contributors:453`、`release v1.3.13`；同一页的可见文案写的是「120,000 stars、800 contributors」。（来源 5）【事实】2026-10-01 的快照里同一段载荷是 `stars:211284`、`contributors:0`、`release v1.18.34`，可见文案写「208,000、950」。（来源 7）【事实】**2026-04-01 的载荷值 453，和取证日 API 实测的 455 落在同一区间，两者相隔半年、取数路径完全不同**；而同一页的文案写的是 800。（来源 5/17）

![官网首屏取证截图：220K Stars / 950 Contributors / 16M Monthly Devs 三块统计，取证 2026-10-25](img/home.png)

**【推断】**官网首页的大数字块与页面里另一处 GitHub 取数**不是同一个数据源**：前者是人工维护的展示文案（450→800→900→950 全是整数台阶），后者是接口回的原始值（453→455，半年几乎没动）。（来源 4/5/6/7/17）【推断】950 这个数在两套已测口径之间，可能是某种未公开的合并口径（跨分支、跨历史、或含机器人），也可能是展示侧的估计值——**本轮无法判定是哪一种，只能记为不可复现**。（来源 1/17/18）【存疑】2026-10-01 载荷里 `contributors:0` 的成因同样缺证：可能是取数失败的默认值，也可能是别的原因，本文不替它猜。（来源 7）【存疑】

月活还有一处口径并存：官网首页说「16M developers every month」，而 2026-10-14 的 OpenCode 2.0 发布笔记写的是「draws on **millions of daily users'** experience」——**月活与日活两套口径在同一周对外发声**，谁也没说清楚 16M 月活对应多少日活。（来源 1/57）【事实】

## 口径审计 ②：16M 那个数，和 npm 上的第三名

npm 的下载接口是这条赛道唯一能横向对表的第三方尺子。取 2026-09-23 至 10-22 这个完整窗口：

| 包名 | 月下载 | 归属 |
|---|---|---|
| `@openai/codex` | **84,315,077** | OpenAI（来源 22） |
| `@anthropic-ai/claude-code` | **45,612,676** | Anthropic（来源 21） |
| `opencode-ai` | 10,835,825 | opencode 主 CLI（来源 19） |
| `@opencode/cli` | 902,850 | 新命名空间 CLI（来源 20） |
| 合计 CLI 口径 | **≈11,738,675** | — |
| `@opencode/client` / `@opencode/ai` | 1,269,104 / 754,179 | 客户端与 SDK（来源 20） |
| 若四包全算 | ≈13,761,958 | — |

【事实】按 CLI 口径，opencode 是**第三名，落后第一名 7.2 倍、落后第二名 3.9 倍**；把 client 与 SDK 也算进来，仍只有 13.8M。（来源 19/20/21/22）【事实】另一个小证据：名为 `opencode` 的包在 registry 里不存在（返回 `package opencode not found`），说明历史上有过包名分裂。（来源 20）【事实】逐日数据里 `@opencode/cli` 在 10-06、10-13 至 15、10-19 至 21 出现 0 值，本文一律用月窗口口径，不拿逐日数说事。（来源 23/24）【存疑】

**16M 与 11.7M 之间差 1.37 倍，这个差距本身不算离谱，但两把尺子量的东西不同。**下载量是一行安装脚本被调用的次数，会被 CI、镜像、自动更新重复放大，也不会把桌面应用、网页版、IDE 插件的用户算进来；月活是官网自己定义的"developers every month"，方法未公开。（来源 19/20/1）【事实】npm 这把尺子有明确上限，官网那把尺子没有对照——**所以本文的判定是：16M 属于自证数字，不是被证伪的数字**。（来源 1/19/20/66）【推断】

**【观点】**这里有一个反常识的读法：如果 16M 月活成立，那么 npm 下载只覆盖了其中约七成，意味着**大量用户不经过 npm**（桌面端、网页端、内置免费模型），这恰好解释了为什么一个 npm 第三名的项目敢在首页把月活写到 16M；反过来，如果 npm 才是主入口，那 16M 就需要一份统计说明来支撑。（来源 1/19/20）两种可能都还站得住，这正是它必须标【存疑】的原因。

## 增长的供给侧：173 天发 100 版，第 2 贡献者是机器人

增长不只有需求侧（有人用），还有供给侧（谁在写）。这一段全部来自 GitHub API 可复核数据：

- 仓库页 `releaseCount` **882**、`tagCount` 1,156；API 首屏 100 个 release 覆盖 2026-05-02 至 10-22，**173 天发 100 版，约每 1.7 天一版**；官网 changelog 页取证日可正常打开，与 releases 时间线同源。（来源 3/25/62）【事实】
- 仓库页显示 **20,893 Commits**；取证前抓的两页提交（各 100 条，覆盖 2026-10-21 17:30 至 10-25 12:46，**不到 4 天**）里，作者名带 bot 的占第 1 页 24%、第 2 页 17%，**合计 20.5%，全部是 `opencode-agent[bot]`**；同一窗口人类提交最多的是 Aiden Cline，83 次。（来源 3/26/27）【事实】
- 贡献者榜（默认分支，前 100 名）：第 1 名 `thdxr` 2,912 次，**第 2 名 `opencode-agent[bot]` 2,602 次**，第 3 名 `rekram1-node` 2,404 次；前 100 名合计 18,582 次，**前 10 名占其中 83.1%**，第 6 名是 `actions-user`（981 次）。（来源 17）【事实】
- 社区吞吐：open issue 2,526 / closed 29,067，open PR 1,287 / merged 10,809；仓库字段 `open_issues_count` 3,812 ≈ 2,526 + 1,287（该字段按 issue 与 PR 合计计数，差 1 属抓取时点）。（来源 28/29/30/31/2）【事实】

**【推断】**把这四条放一起：这是一个**发布节奏由自动化托着、贡献高度集中在一个小圈子、机器人排进贡献榜第二名**的项目。（来源 17/25/26/27）【推断】「950 contributors」即使按最宽松口径成立，也描述不了这个结构——**活跃供给集中在前 10 人（83%）与一个 agent bot 上**，剩下几百人更像是累计出现过的署名。（来源 17）这不罕见（大型开源项目普遍如此），但它意味着官网把 950 和 220,000 并排展示时，读的人容易把它理解成"950 个活跃共建者"，而 API 给出的图景更接近"前 10 人 + 一个机器人"。（来源 1/17）【观点】

## 增长的代价 ①：泄露披露，和 68 天前就说过的话

2026-10-17 17:56 UTC，opencode 官方账号发出第一条披露：「We had a security breach last night related to a **large scale distillation attack** that impacted **1.4% of OpenCode Console users**」；12 分钟后的第二条补充：受影响的是「a subset of users who had **external providers linked**」，并称正与 Anthropic、OpenAI、Google 一起处置。（来源 34/35）【事实-当事人主张】HN 当天把它顶到 975 分、185 条评论，标题直接写成「OpenCode Hacked: User credentials leaked」。（来源 36）【事实】

被追问的细节本轮拿不到：官方以"事件仍在进行中"为由未公布时间线与影响面，**1.4% 这个比例的分母（Console 用户总数）没有公开，因此无法复核**。（来源 34/35）【存疑】

更硬的一条时间差是这样的：隐私质疑不是事后才出现的。issue **#10416《OpenCode is not private by default?》**在 2026-01-24 开题，报告人用 nftables 拦出站流量，发现**会话标题是在本机网络之外计算的**，正文直接 @ 了维护者 thdxr；这个 issue 有 63 条评论，直到 **2026-06-23 才关闭，从开题到关闭 150 天**。（来源 32/33）【事实】泄露披露是 2026-10-17，**距该 issue 开题 266 天**。（来源 32/34）

而 HN 的泄露帖里还有一条更扎眼的自述。用户 bewareofscams 在评论区写道：「I told you so… **Me, 68 days ago**: > Beware of the author of tweet, who happens to be author of OpenCode — OpenCode will leak all your data to themselves and to shady 3rd parties…」，并把链接指回同一个 issue #10416——他预警的时间点是 2026-08-10，**比官方披露早 68 天**。（来源 36/32）【事实】

**【事实】**这两个天数都成立但说的不是同一件事：266 天是"公开质疑到官方披露"的间隔，68 天是"个人预警到官方披露"的间隔；把它们混用会夸大或缩小时间差，本文两个数并列。（来源 32/34/36）【事实】**【推断】**一个开了 150 天才关闭、并且被 HN 评论者当作"我早说过"来引用的 issue，说明**在泄露发生之前，"默认不隐私"这件事在公开面上已经被指认过**；至于它与 10 月的事件是否有因果关系，本轮无证据，不作推断。（来源 32/36）【存疑】

## 增长的代价 ②：CVE-2026-22812 与三处「静默修复」

第三方安全研究 cy.md 的报告是本轮证据里结构最完整的一份：受影响软件 `opencode-ai`，编号 **CVE-2026-22812**。（来源 45）【事实】报告正文写明：v1.1.10 之前，OpenCode 会**自动且静默地启动一个无鉴权的 HTTP 服务**（默认端口 4096 起），暴露的接口里包括执行任意 shell、创建交互终端会话、读取任意文件——**任何能连上它的客户端都拿到与当前用户同等的代码执行权限，且运行时没有任何可见提示**。（来源 45）【事实】

报告的攻防对照表是这张：

| 状态 | 条目数 | 代表项 |
|---|---|---|
| **Silent fix**（已修但无公告） | **3** | < v1.0.216 任意网站可执行代码；< v1.1.10 本地任意进程/本机回环页面可执行代码 |
| **Unfixed**（报告时仍未修） | **8** | 服务被手动开启后仍完全无鉴权；`--mdns` 时同网任意机器可执行；**CORS 白名单硬编码 `*.opencode.ai`**；官网被攻破即可打到开启了服务的用户；官网任一子域 XSS 同理 |

【事实】（来源 45）那条 CORS 项是全表最尖的一条：报告写明「If opencode.ai is ever compromised, or an XSS vulnerability is found on any subdomain, attackers could exploit all users who have the server enabled」。（来源 45）【事实】这条报告在 HN 拿到 432 分（2026-01-11），是本项目最早的头条之一。（来源 41）

**【事实】**本文只做阅读与转述：报告里的两段 PoC 脚本本文**未执行、未改写、未验证**，README 的一行式安装脚本同样**只记录不运行**；取证包里保留的是网页快照，不是可执行物。（来源 45）【事实】**【推断】**「3 处静默修复 + 8 项未修」这个组合，说明漏洞的处置节奏与增长节奏之间存在错位：产品以每 1.7 天一版的速度发版，而报告里最尖的几项在报告时点仍未修。（来源 45/25）【推断】这与后面 wren.wtf 提到的「stale bot 关掉安全报告、而且不止一次」是同一方向的两份独立证据。（来源 46）

## 增长的代价 ③：19 分钟合并的法务 PR

GitHub 上有一条读起来像监控录像的记录。PR **#18186，标题就叫「anthropic legal requests」**，由维护者 `thdxr` 于 2026-03-19 04:26:48Z 提交，04:45:24Z 合并——**从开 PR 到合并 18.6 分钟，作者与合并者是同一个人**。（来源 48）【事实】PR 正文四条，全是删除：移除 `anthropic-20250930.txt` 提示词文件、从 provider hints 里移除 anthropic、移除 `opencode-anthropic-auth` 内置插件、从 provider 枚举里移除 anthropic。（来源 48/49）【事实】HN 当天把它做成头条《Anthropic takes legal action against OpenCode》，483 分。（来源 40）

同一周的另一条线是拦截。2026-01-15 的 HN 帖《Anthropic Explicitly Blocking OpenCode》173 分，指向一份 gist PoC：作者用 Claude Pro/Max 的 OAuth 登录后逐一测试系统提示词，结论写在文件头——第二个提示词块几乎可以写任何内容，**唯独「You are OpenCode」会被拦，而「You are Cursor」「You are Pi」等身份声明都能通过**。（来源 44/47）【事实】gist 是自述性 PoC，本轮只读取文本、未运行脚本。（来源 47）

**【推断】**把这两件事和前一节的泄露披露放在一起，能看到这个项目的增长边界不只在用户侧，也在**上游授权侧**：一个以"接任何模型"为卖点的开源 agent，会同时碰到模型厂商的法务、OAuth 提示词策略和品牌使用边界。（来源 48/47/1）【推断】**【存疑】**法务请求的原文、Anthropic 的正式声明，本轮一个都没取到——HN 的标题是社区写法，PR 正文是执行记录，**"legal action" 的具体内容是什么，属缺证项**，本文只写"有这么一份 PR、19 分钟删完"。（来源 40/48）

## 主体与钱线：从 sst 到 Anomaly，三张钱图

主体迁移的证据在第 3 节已经锁死：`sst/opencode` → `anomalyco/opencode`，窗口 2026-01-02 前后。（来源 11/12/13/14）【事实】往上下游看，sst.dev 的页脚写着「© 2026 **Anomaly Innovations**」；GitHub 组织 `anomalyco` 下有 **85 个仓库**，按星数排是 opencode 221,370、sst 26,377、opentui 13,616、openauth 7,418、models.dev 7,380——**opencode 是这个组织的绝对主干**。（来源 52/53/54）【事实】

钱的入口有两条，都写在官网上。

**Zen 托管版**（截图取证）自称「**not for profit**」「**zero markups**」，收费方式是每次充值收一笔手续费：充 $20 收 **$1.23**，明说这是"a small payment processing fee"；FAQ 里写「Zen is not for profit. Zen passes through the costs from the model providers to you.」（来源 50）【事实-当事人主张】代言人一栏里排第一的是「**Dax Raad — ex-CEO, Terminal Products @OpenCode**」，而 `Dax Raad` 正是贡献榜第一名的 `thdxr`、也是 19 分钟合并法务 PR 的那位维护者（提交记录里同一人的署名是 `Dax`，邮箱域 thdxr.com，2025-07-06 的 HN 评论也直接写 "thdxr(dax)(SST)"）。（来源 50/17/48/26/43）【事实】

![Zen 托管版取证截图：$20 充值 + $1.23 手续费、zero markups，取证 2026-10-25](img/zen.png)

**Enterprise 版**（截图取证）走的是完全不同的路：SSO + 内部 AI 网关接入，FAQ 写明「**If you have your own LLM gateway, we do not charge for tokens used**」，定价走定制报价，表单里直接问「current monthly inference spend (USD)」；合规页指向 **Trust Center（trust.opencode.ai），写着 SOC 2 Type 2 报告、安全政策、子处理方清单，文件需 NDA 申请**。（来源 51）【事实】

![Enterprise 取证截图：SSO + 自带网关 + Trust Center / SOC 2 Type 2，取证 2026-10-25](img/enterprise.png)

三个"没有"也要记账：`opencode.ai/pricing` 返回 **404**（没有定价页，钱的入口全在 enterprise 表单）；`.github/FUNDING.yml` 返回 **404**（仓库没有配资助文件），但组织的 GitHub Sponsors 页面是 200。（来源 59/55/56）【事实】**融资与收入是零公开口径**：本轮搜索四链全部风控、备用检索通道 403，没取到任何融资新闻、财报或投资人页面；按本文规则，**"取不到"只能写成"无公开口径"，不能写成"未融资"**。（来源 55/56）【存疑】

**【观点】**「not for profit」是 Zen 这条产品线的自我描述，不是公司层面的法律声明——同一家 Anomaly Innovations 还运营着 sst、openauth、models.dev 等 85 个仓库，而 Enterprise 版在正经收集客户的月度推理预算。（来源 50/51/54/52）【观点】把三张钱图并排读：**Zen 走"零加价+手续费"的口碑定价，Enterprise 走"自带网关免 token 费+定制报价"的企业定价，GitHub 走"完全免费+MIT"的获客定价**——三条线不互斥，但"not for profit"这句话只属于第一条。（来源 50/51/1/2）

## 另一侧口碑：要求别再用它的那篇长文

批评方的证据也要双侧并列。wren.wtf 的《Stop Using OpenCode》2026-07-20 发布，HN 420 分。（来源 46/42）【事实】原文开头给项目定位是「the most popular open source coding agent, and it currently has **161k stars**」，接着给出全篇最刺的一句结论：「OpenCode is **clown-car turboslop** with a security posture of "let me bend over for you daddy". Everyone using it should stop using it.」（来源 46）【事实-引文】作者注明参考的是 git 版本 `baef5cd4`，并声明「I don't consider anything in this post to be a security disclosure」。（来源 46）

这篇文在 2026-07-24 加了更新，承认三项修复已落地：工具调用裁剪默认关闭、跨会话的命令前缀权限被移除、bash 重定向改为路径校验。（来源 46）【事实】但更新里留了最重的一句：「a **GitHub stale bot closing a security report** is jaw-dropping, and **it's happened more than once**」。（来源 46）【事实】作者也顺手祝福了 2.0：「I wish them the best with their release.」（来源 46）

**【事实】**立场要摆清楚：wren.wtf 是与被评项目无经济往来的第三方，但这篇文章的措辞与结论都站在对立面；本文采信其中**可复核的部分**（版本号、修复项、stale bot 这一可查事实），不采信它的形容词。（来源 46）【事实】同理，cy.md 的报告同样立场独立、且比批评文更结构化，但它同样是单方面披露，被指的 8 项未修在本轮没有官方回应可对照。（来源 45）【存疑】

## 官方也在发数：opencode.ai/data 能证明什么

官网上还有一个容易被忽略的面：`opencode.ai/data` 是一个公开的用量排行榜，页签包括 Top Models Leaderboard、Session Cost、Token Cost、Cache Ratio、**Market Share**、**Geo Breakdown**，页头标着「Updated Oct 25, 12:33 PM UTC」。（来源 60）【事实】取证日可读到的第一屏写着：截至 2026-10-25，DeepSeek V4.1 Flash 在过去 7 天以 **114T tokens** 领跑 OpenCode 用量，其后是 MiniMax-M3.1-Flash（37T）与 GLM-5.3-Flash（8.6T）。（来源 60）【事实】

**【推断】**这张表是 16M 月活叙事的旁证：能按天吐出十万亿级 token 用量、还能分国家地区出榜的服务，背后必然有真实的调用流量，"完全没人用"的解释可以排除。（来源 60/1）【推断】**【存疑】**但它同样不能换算成人数——token 量会被 agent 的自动循环放大，Market Share 与 Geo Breakdown 具体以什么为分母、是否只统计走官网账号/免费模型的调用，**页面上没有方法说明，本文不作推断**。（来源 60）

**【事实】**这里还有一处需要点破的张力：首页主打「OpenCode does not store any of your code or context data」，而 data 页在持续公布模型用量与地区分布。（来源 1/60）【推断】两者并不必然矛盾——前者说的是代码与上下文内容，后者是聚合后的调用量——但**这个区分是本文的推断，官方页面没有一句话把两者的关系写清楚**，所以在事实层面它仍是缺证的。（来源 1/60）【存疑】

## 结论：曲线是真的，账本要分三本读

**第一本能对上。**star、commits、release、fork、周榜，全部可第三方复核：221,369★、20,893 Commits、882 releases、29,626 fork、周增 6,857★，而且取证当天还在涨。（来源 2/3/15）【事实】从 206★ 到 221,369★ 的 18 个月，是这条赛道里排得上号的增长曲线，没有任何需要辩解的地方。（来源 10/2）

**第二本换了尺子。**npm 上它是第三名，CLI 口径 11.7M 月下载，落后 `@openai/codex` 7.2 倍、落后 `@anthropic-ai/claude-code` 3.9 倍；这把尺子可复核但量的是"安装脚本被调用的次数"，且不覆盖桌面端与网页端。（来源 19/20/21/22）【事实】把它当用户数读会低估，把它当活跃度读则方向正确——**位次与量级可信，绝对值不可信**。（来源 19/20）【推断】

**第三本对不上。**950 contributors 在两套已测口径（455 / 1,069）之间，官网自己 2026-04-01 的页面载荷写的是 453；16M monthly devs 没有任何第三方口径，而同一周官方还在用"millions of daily users"讲日活（发布笔记挂在 anoma.ly，即 Anomaly 自家的博客站）。（来源 17/18/5/1/57/58）【事实】这两个数字**不是被证伪，是不可复现**——本文的处理是标【存疑】并把两套口径都摆出来，而不是替它补一个算法。（来源 1/17/18）【存疑】

**代价那本也是真的。**10-17 的泄露披露（当事人主张 1.4%）、CVE-2026-22812 的 3 处静默修复与 8 项未修、19 分钟删完的法务 PR、被点名拦截的系统提示词、以及 wren.wtf 那篇 420 分的"别再用它"——每一项都有可点开的原始出处。（来源 34/35/45/48/47/46）【事实】**【观点】**把三条账本和一张代价清单并排，我对这个项目的判断是：**它是一个增长方式完全跑通、但口径管理明显落后于增长的项目**——披露动作是有的（泄露当天就发推、报告里的修复也在落地），落后的部分是那些挂在首页、没有方法说明、也没人对表的整数台阶。（来源 1/34/45）【观点】

**【推断】**最后留一个给读者的检查动作，也给我自己：**看到一个大数字，先问它有没有第二把尺子**。有第二把尺子且对得上，这数字能用；只有一把尺子、且这把尺子在对方手里，那它就只是个展示字段。（来源 1/19/2）【推断】

---

## 来源（取证日 2026-10-25，全部可点开）

1. opencode.ai 官网首页（220,000★ / 950 contributors / 20,000+ commits / 16M monthly devs；FAQ 称 MIT 与"任何人可提 issue/PR"；三形态 + 75+ 提供商 + 免费模型）https://opencode.ai/
2. GitHub API · 仓库元数据（stargazers 221,369 / fork 29,626 / open_issues 3,812 / watcher 858 / created 2025-04-30 / pushed 2026-10-25 / spdx MIT / default dev）https://api.github.com/repos/anomalyco/opencode
3. GitHub 仓库页（20,893 Commits / releaseCount 882 / tagCount 1,156，截图 shot_repo）https://github.com/anomalyco/opencode
4. Wayback · 官网快照 2026-01-01（41,000★ / 450 / 6,000+ / 400,000 Monthly Devs）https://web.archive.org/web/20260101140632/https://opencode.ai/
5. Wayback · 官网快照 2026-04-01（120,000★ / 800 / 10,000+ / 5M；页内载荷 stars:134293、contributors:453、release v1.3.13）https://web.archive.org/web/20260401104543/https://opencode.ai/
6. Wayback · 官网快照 2026-07-01（160,000★ / 900 / 13,000+ / 7.5M）https://web.archive.org/web/20260701134113/https://opencode.ai/
7. Wayback · 官网快照 2026-10-01（208,000★ / 950 / 13,000+ / 16M；页内载荷 stars:211284、contributors:0、release v1.18.34）https://web.archive.org/web/20261001153716/https://opencode.ai/
8. Wayback · 官网快照 2025-06-20（尚无大数字块）https://web.archive.org/web/20250620102547/https://opencode.ai/
9. Wayback · 官网快照 2025-10-01（尚无大数字块）https://web.archive.org/web/20251001204015/https://opencode.ai/
10. Wayback · sst/opencode 快照 2025-05-15（206 users starred）https://web.archive.org/web/20250515002015/https://github.com/sst/opencode
11. Wayback · sst/opencode 快照 2026-01-02 00:37（45,255 users starred）https://web.archive.org/web/20260102003717/https://github.com/sst/opencode
12. Wayback · anomalyco/opencode 快照 2026-01-02 21:19（45,642 users starred，该 URL 首次入档）https://web.archive.org/web/20260102211959/https://github.com/anomalyco/opencode
13. Wayback CDX · anomalyco/opencode（首条时间戳 20260102211959）https://web.archive.org/cdx/search/cdx?url=github.com/anomalyco/opencode&output=json&limit=80&collapse=timestamp:4&filter=statuscode:200&from=2026
14. Wayback CDX · sst/opencode（20250515002015 起多条 200 记录）https://web.archive.org/cdx/search/cdx?url=github.com/sst/opencode&output=json&limit=80&collapse=timestamp:4&filter=statuscode:200&from=2025
15. GitHub trending 周榜（anomalyco/opencode 221,372★、6,857 stars this week）https://github.com/trending?since=weekly
16. GitHub API · stargazers 明细 → 404（端点失效，星标曲线改走 Wayback + 第三方）https://api.github.com/repos/anomalyco/opencode/stargazers?per_page=100&page=20
17. GitHub API · contributors 第 1 页（Link rel=last → page=5，末页 55 人，**注册合计 455**；top1 thdxr 2,912、#2 opencode-agent[bot] 2,602、top100 合计 18,582、top10 占 83.1%）https://api.github.com/repos/anomalyco/opencode/contributors?per_page=100
18. GitHub API · contributors 含匿名（Link rel=last → page=11，末页 69 人，**合计 1,069**，匿名身份 login 为 null）https://api.github.com/repos/anomalyco/opencode/contributors?per_page=100&anon=true
19. npm downloads · opencode-ai（10,835,825，2026-09-23→10-22）https://api.npmjs.org/downloads/point/last-month/opencode-ai
20. npm downloads · @opencode/cli（902,850）/ @opencode/client（1,269,104）/ @opencode/ai（754,179）；`opencode` 包返回 not found https://api.npmjs.org/downloads/point/last-month/@opencode/cli
21. npm downloads · @anthropic-ai/claude-code（45,612,676，同窗口）https://api.npmjs.org/downloads/point/last-month/@anthropic-ai/claude-code
22. npm downloads · @openai/codex（84,315,077，同窗口）https://api.npmjs.org/downloads/point/last-month/@openai/codex
23. npm downloads 逐日 · opencode-ai（2026-09-25→10-25）https://api.npmjs.org/downloads/range/2026-09-25:2026-10-25/opencode-ai
24. npm downloads 逐日 · @opencode/cli（10-06、10-13～15、10-19～21 为 0 值，疑回填延迟）https://api.npmjs.org/downloads/range/2026-10-01:2026-10-22/@opencode%2fcli
25. GitHub API · releases 首屏 100 条（v1.14.32 2026-05-02 → v1.19.4 2026-10-22，共 173 天）https://api.github.com/repos/anomalyco/opencode/releases?per_page=100
26. GitHub API · commits 第 1 页（100 条，2026-10-21T17:30Z 起，bot 24%）https://api.github.com/repos/anomalyco/opencode/commits?per_page=100
27. GitHub API · commits 第 2 页（100 条，bot 17%，人类最高 Aiden Cline）https://api.github.com/repos/anomalyco/opencode/commits?per_page=100&page=2
28. GitHub search · open issues = 2,526 https://api.github.com/search/issues?q=repo:anomalyco/opencode+is:issue+is:open
29. GitHub search · closed issues = 29,067 https://api.github.com/search/issues?q=repo:anomalyco/opencode+is:issue+is:closed
30. GitHub search · merged PR = 10,809 https://api.github.com/search/issues?q=repo:anomalyco/opencode+is:pr+is:merged
31. GitHub search · open PR = 1,287 https://api.github.com/search/issues?q=repo:anomalyco/opencode+is:pr+is:open
32. GitHub API · issue #10416（2026-01-24 开、2026-06-23 关、63 评论、nftables 实测会话标题出网）https://api.github.com/repos/anomalyco/opencode/issues/10416
33. issue #10416 页面 https://github.com/anomalyco/opencode/issues/10416
34. fxtwitter · 官方推 2111516673953935833（2026-10-17 17:56 UTC，large scale distillation attack / 1.4% of OpenCode Console users）https://api.fxtwitter.com/opencode/status/2111516673953935833
35. fxtwitter · 官方推 2111519637049319818（18:08 UTC，external providers linked、与 Anthropic/OpenAI/Google 处置）https://api.fxtwitter.com/opencode/status/2111519637049319818
36. HN · OpenCode Hacked: User credentials leaked（975 分 / 185 评论，2026-10-17；含"Me, 68 days ago"评论，id 50133643 引 49243119）https://news.ycombinator.com/item?id=50133401
37. HN Algolia 检索 · opencode 头条分数总表 https://hn.algolia.com/api/v1/search?query=opencode&tags=story&hitsPerPage=30
38. HN · OpenCode – Open source AI coding agent（1,274 分，2026-03-20，指向 opencode.ai）https://news.ycombinator.com/item?id=47460525
39. HN · OpenCode 2.0（888 分，2026-10-14，指向 anoma.ly 笔记）https://hn.algolia.com/api/v1/items/50094994
40. HN · Anthropic takes legal action against OpenCode（483 分，2026-03-19，指向 PR #18186）https://hn.algolia.com/api/v1/items/47444748
41. HN · Unauthenticated remote code execution in OpenCode（432 分，2026-01-11，指向 cy.md）https://hn.algolia.com/api/v1/items/46581095
42. HN · Annoying and alarming things about OpenCode（420 分，2026-07-20，指向 wren.wtf）https://hn.algolia.com/api/v1/items/48978112
43. HN · Opencode: AI coding agent, built for the terminal（319 分，2025-07-06，指向 sst/opencode）https://hn.algolia.com/api/v1/items/44482504
44. HN · Anthropic Explicitly Blocking OpenCode（173 分，2026-01-15，指向 gist）https://hn.algolia.com/api/v1/items/46625918
45. cy.md · Unauthenticated Remote Code Execution in OpenCode（CVE-2026-22812；3 项 Silent fix、8 项 Unfixed、CORS 硬编码 *.opencode.ai；PoC 未执行）https://cy.md/opencode-rce/
46. wren.wtf · Stop Using OpenCode（2026-07-20 发布、07-24 更新，161k★、三项修复、stale bot 关安全报告不止一次）https://wren.wtf/shower-thoughts/stop-using-opencode/
47. gist · PROOF: Anthropic is specifically blocking "OpenCode"（文件头结论；脚本未运行）https://gist.githubusercontent.com/R44VC0RP/bd391f6a23185c0fed6c6b5fb2bac50e/raw
48. GitHub API · PR #18186（2026-03-19T04:26:48Z 提交 → 04:45:24Z 合并 = 18.6 分钟，作者与合并者均 thdxr，正文四条删除项）https://api.github.com/repos/anomalyco/opencode/pulls/18186
49. PR #18186 页面（标题 anthropic legal requests）https://github.com/anomalyco/opencode/pull/18186
50. opencode.ai/zen（not for profit / zero markups / $1.23 per $20、Dax Raad 等代言人，截图 shot_zen）https://opencode.ai/zen
51. opencode.ai/enterprise（SSO + 内部网关、自带 LLM gateway 不收 token 费、Trust Center / SOC 2 Type 2、月度推理支出表单，截图 shot_enterprise）https://opencode.ai/enterprise
52. sst.dev 首页页脚（© 2026 Anomaly Innovations）https://sst.dev/
53. GitHub 组织页 anomalyco（仓库清单入口）https://github.com/anomalyco
54. GitHub API · org repos（85 仓：opencode 221,370 / sst 26,377 / opentui 13,616 / openauth 7,418 / models.dev 7,380）https://api.github.com/orgs/anomalyco/repos?per_page=100
55. `.github/FUNDING.yml` → 404（仓库未配资助文件）https://raw.githubusercontent.com/anomalyco/opencode/dev/.github/FUNDING.yml
56. GitHub Sponsors · anomalyco（页面 200，title 可核）https://github.com/sponsors/anomalyco
57. anoma.ly · OpenCode 2.0 笔记（"draws on millions of daily users' experience"）https://anoma.ly/notes/opencode-2-0/
58. anoma.ly 首页（Anomaly 博客站）https://anoma.ly/
59. opencode.ai/pricing → 404（无定价页，钱的入口在 enterprise 表单）https://opencode.ai/pricing
60. opencode.ai/data（Top Models / Session Cost / Token Cost / Cache Ratio / Market Share / Geo Breakdown；Updated Oct 25 12:33 PM UTC；DeepSeek V4.1 Flash 7 天 114T tokens）https://opencode.ai/data
61. opencode.ai/docs（文档站，截图 shot_docs）https://opencode.ai/docs/
62. opencode.ai/changelog（变更日志页，200）https://opencode.ai/changelog
63. README · dev 分支（"The open source coding agent"、十语言版本、Discord/npm/build 徽章）https://raw.githubusercontent.com/anomalyco/opencode/dev/README.md
64. star-history 曲线（第三方星标曲线，旁证）https://api.star-history.com/svg?repos=anomalyco/opencode&type=Date
65. Wayback CDX · opencode.ai（官网快照清单，本文 6 个时点由此筛选）https://web.archive.org/cdx/search/cdx?url=opencode.ai&output=json&limit=200&collapse=timestamp:6&filter=statuscode:200
66. npm registry · opencode-ai latest（包元数据与版本）https://registry.npmjs.org/opencode-ai/latest

取证档案：`research/evidence/opencode/`（**242 个快照文件 / 72 条唯一来源 URL**，manifest 带 sha256、通道与来源 URL；通道 = 通道A 正文抓取 + GitHub REST 只读（PAT 零回显）+ Wayback CDX/快照 + npm downloads API + HN Algolia + fxtwitter + 通道D 截图 5 张）。防投毒扫描：对 242 个快照文件跑强模式 grep（ignore/disregard previous instructions、you are an AI、contact me、telegram、as an AI language 等）→ **3 个文件命中、2 件内容，已逐条公示、全部未执行**：① wren.wtf 批评文的 HTML 与纯文本两份快照里，作者在嘲讟 Claude Code 系统提示词的行文时自己写了一句玩笑话「Disregard previous instructions.」——上下文是讽刺提示词工程，不是发给读者的指令，本文只记录不执行；② `gh_closed_issues.json` 里一条 issue 写着「The consumer is a Telegram bridge」，是产品功能描述，与注入无关；其余模式（ignore previous instructions、you are an AI、contact me）全部 0 命中。官网一行式安装脚本与 cy.md、gist 的 PoC 均只记录未运行。本文正文各节引用其中 5 张（`shot_home / shot_repo / shot_zen / shot_enterprise / shot_docs`）。

## 勘误与可撤回

- 本文任一判断被一手源推翻，请来信指出；核实后在对应句子后加「（勘误 <日期>）」并保留原句划线，**不删旧数字、不改历史口径**。
- 可疑行为记录（防投毒铁律）：wren.wtf 批评文正文里有一句「Disregard previous instructions.」，出自作者重写 Claude Code 系统提示词、嘲笑提示词工程的段落——是文章修辞，不是发给读者的指令，已登记、未执行、未转为任何行动指令；`gh_closed_issues.json` 中的「The consumer is a Telegram bridge」为产品功能描述。本文引用的 cy.md 与 gist PoC、官网一行式安装命令，均只读未运行。【存疑】
- 已知存疑项：**950 contributors 无可复现口径**（已测 455 / 1,069，官网 2026-04-01 页内载荷 453、2026-10-01 载荷 0，0 的成因缺证）；**16M monthly devs 与 millions of daily users 均为官方自证、无方法说明**；泄露事件「1.4% of Console users」的分母未公开，无法复核；CVE-2026-22812 的 8 项 Unfixed 在本轮没有官方回应可对照，属单方面披露；法务请求原文与 Anthropic 正式声明未取到，「legal action」一词出自 HN 社区标题；`opencode.ai/data` 的 Market Share / Geo Breakdown 分母与统计范围无方法说明；融资与收入**零公开口径**（搜索通道风控、备用检索 403），按规则只记"无公开口径"，不记"未融资"；npm 逐日 0 值疑为回填延迟，全文用月窗口口径。【存疑】
