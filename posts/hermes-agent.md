# 7 个月只有 137★，再 8 个月冲到 257,319★：Hermes Agent 与 OpenClaw，两条都成了，钱的走法相反

> 本站为独立技术分析，与文中项目无商业关系。

取证日期：2026-10-27（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 20 · 可验证性 19 · 独特性 16 · 受众相关度 17 · 多维空间 17 = **89/100**（≥60 才动笔；素材库预分 88）。框架卡 = **对比横评**（第 18 篇是商业画布，轮换合规；上次横评是第 13 篇）：本篇不做功能测评、不罗列漏洞清单，按「**两条成功路径各自怎么达成 → 差异轴 → 差异的代价**」三层走，风险与治理问题只作为路径差异的支线，不写成避坑文。题眼一句话：同一个赛道、同一种爆款曲线，一条路径把钱留在公司账上，一条路径把钱挡在基金会门外——**两条都成了，代价长得完全不一样**。

## 一、横评总表：八格对照，每格双方都有出处

| 对照格 | Hermes Agent（2026-10-27 取证） | OpenClaw（2026-10-26 取证，第 18 篇复用） |
|---|---|---|
| star 双口径 | API **257,319** / 页面 **257,321**（来源 1/2） | API **393,064** / 页面 **393,064**（来源 45/47） |
| 爆发曲线 | 建仓 2025-07-22 → 2026-02-25 仅 **137★** → 10-01 **250,484★**（Wayback ×6）（来源 3/4/5/6/7/8/11） | 建仓 2025-11-24 → 9 周 **+237,633★** → 11 个月 393,064★（来源 47/49/50） |
| 提交 / 贡献者 / 集中度 | **59,466** commits · **401** 贡献者 / **51,995** contributions · top1 **teknium1 24,806 = 47.7%**（来源 1/30/31） | **119,646** commits · **383** 贡献者 / **113,798** contributions · top1 **steipete 67,083 = 58.9%**（来源 46/51/52） |
| issue / PR 关闭率（search 同口径） | issue 开 17,234 / 关 20,031 → **53.7%**；PR **68.3%**（来源 26/27/28/29） | issue 开 5,056 / 关 56,178 → **91.7%**；PR 开 2,768* / 关 117,531 → **97.7%**（来源 48/53/54/55） |
| 渠道数 | 官网 **7**（Telegram/Discord/Slack/WhatsApp/Signal/Email/CLI），README 特性表 6（来源 12/14） | README「and **20+ more**」+ 官网 **29 channels**（来源 45/46） |
| 资金模式 | **公司**（Nous Research, Inc.）：$90M B 轮 @$1.5B（2026-10-07）、累计 $158M；agent MIT 免费，收入靠 Portal 订阅 $0/$20/$100/$200（来源 38/12） | **501(c)(3) 基金会**（2026-07-08）；官网三处明写无订阅、无托管、无 token，零收入（来源 45/56/57） |
| 风险事件 | HN **1,251** 分计费事件；#10232 删评改题；Parallel 默认路由 10 分钟回滚；**CVE×2（CVSS 3.1 = 7.3）**（来源 22/23/25/34/35） | CVE-2026-33579 提权 **9.4**；Anthropic 拒订阅 HN **1,099**、提名字拒答 **1,349**；「security nightmare」397 分（来源 58/59/60/61） |
| HN 声量结构 | 高分几乎全是**事件**（1,251 计费 / 122 迁移），产品帖仅 **52** 分且链到已 451 的 .org（来源 17/18/19） | 产品与对抗话题直接高分（1,349 / 1,099 / 802 / 667）（来源 59/60） |

【事实】八格里没有一格是「谁更强」的结论，八格都是**可复核的读数**；两侧口径差异处（issue 关闭率、渠道数、集中度）已统一为同一算法，计算过程写在第七节。【事实】

## 二、路径 A：Hermes 的 7 个月沉睡，和它之后的 8 个月

仓库创建时间 2025-07-22T22:22:28Z，最早的提交落在 2025-07-26（`hjc-puro` 两条、`Teknium` 合并 PR #1），接着 07-31 一条内容抓取的初版实现——**这就是起点的全部公开动作**。（来源 2/11）【事实】

然后是 7 个月的静默期。仓库页第一份可用的 Wayback 快照是 2026-02-25，读数 **137★**；同期归档里还有 370 个提交的痕迹，说明代码一直在写，只是没人看。（来源 3）【事实】**【存疑】** 2025-07 至 2026-02 之间的仓库快照本轮 CDX 查询返回空体，**没有第二把尺子能证明这段时间的星标轨迹**，本篇只用 02-25 这一个点，不把它外推成「零关注」。

转折点的两个读数几乎重合：官方站 `hermes-agent.nousresearch.com` **2026-03-05 首次被 Wayback 收录**（来源 9），仓库星标 03-08 到 **2,297★**（来源 4），04-01 到 **20,623★**（来源 5）。

| Wayback 快照日 | 星标读数 | 区间增量 | 日均 | 出处 |
|---|---|---|---|---|
| 2026-02-25 | 137 | 建仓起 218 天 | 0.6/天 | 来源 3 |
| 2026-03-08 | 2,297 | +2,160 / 11 天 | 196/天 | 来源 4 |
| 2026-04-01 | 20,623 | +18,326 / 24 天 | 764/天 | 来源 5 |
| 2026-06-01 | 174,961 | +154,338 / 61 天 | 2,530/天 | 来源 6 |
| 2026-08-01 | 223,796 | +48,835 / 61 天 | 800/天 | 来源 7 |
| 2026-10-01 | 250,484 | +26,688 / 61 天 | 438/天 | 来源 8 |
| **2026-10-27（API 实测）** | **257,319** | +6,835 / 26 天 | 263/天 | 来源 2 |

【事实】**建仓 7 个月只有 137★；此后 7 个多月（至 10-01 快照）吃掉 250,347★，取证日 API 已达 257,319★**；其中 04-01 → 06-01 的 61 天独占 15.4 万★，是整条曲线最陡的一段。全部数字由快照页 `aria-label="… users starred"` 原文读出，不是估读。（来源 3/4/5/6/7/8/2）【事实】

![GitHub 仓库首屏取证截图：59,466 Commits 与 257,321★，取证 2026-10-27](img/repo.png)

**【推断】** 把官网首捕（03-05）与星标三个读数（03-08 的 2,297、04-01 的 20,623、06-01 的 174,961）按时间摆开，可以读出**增长的起点不是建仓日，而是产品化那天**：桌面版 + 官网 + 一键安装脚本把「一个研究仓库」翻译成了「一个能装的东西」，7 个月的代码存量在 3 个月内被兑换成关注。（来源 9/4/5/6/14）【推断】**【观点】** 沉睡期不是浪费——没有那 7 个月的提交密度，官网上线那天也没有可供截图的功能面。

## 三、路径 B：OpenClaw 的 9 周尖峰，与它已经住了 6 个月的平台期

对照方的曲线形态完全不同。第 18 篇的读数：仓库创建 2025-11-24，2026-01-30 已 106,177★，到 04-01 冲到 **343,810★**——**61 天（9 周）+237,633★，日均约 3,896★**；随后 04-01 → 10-01 的 6 个月只 +47,347★，日均约 259★。（来源 47/49/50）【事实】官方在改名公告里自述起点：「Two months ago, I hacked together a weekend project… **over 100,000 GitHub stars and drew 2 million visitors in a single week**」。（来源 56）【事实-当事人主张】

**【推断】** 两条曲线叠在一起读，差别不在「谁涨得快」，而在**引爆机制**：OpenClaw 的扳机是**一次面向全网的改名解释**（Clawd → Moltbot → OpenClaw，公告本身成了发布稿），Hermes 的扳机是**一次产品化上线**（官网 + 桌面版 + 安装脚本）；前者靠事件密度，后者靠交付密度。（来源 56/9/12/14）【推断】

更值得记的是收敛后的速度。Hermes 取证前 26 天的日均是 **263★**（来源 2/8），OpenClaw 平台期 6 个月的日均是 **259★**（来源 49/50）——**两个不同出身、不同节奏的项目，最后都收敛到「每天约 260 星」的存量增长速度**。【事实】

**【观点】** 这大概是爆款开源项目的共同重力：尖峰高度由事件决定，平台期速度由「每天有多少人在装它」决定，而后者跟项目是谁、拿了多少钱关系不大。对两条路径的共同启示是——**别用尖峰期的数字做任何计划**。

## 四、差异轴一：钱的走法相反——$1.5B 的公司，与零收入的基金会

Hermes 侧的钱线是一条完整的公司融资线。TechCrunch 2026-10-07：**$90M B 轮 @$1.5B**，Robot Ventures 领投，Nvidia、Union Square Ventures、Menlo Ventures、Samsung、1789 Capital 参与，**累计融资 $158M**，同期发布 **Hermes for Businesses**。（来源 38）【事实】产品侧的收费面挂在 Nous Portal：**Free $0 / Plus $20 / Super $100 / Ultra $200 每月**，全部档位含月度额度、200+ 模型与内置工具调用；agent 本体仍是 MIT 免费。（来源 12）【事实】

![官方站取证截图：Portal 四档定价 Free/$20/$100/$200 与 200+ 模型，取证 2026-10-27](img/official.png)

OpenClaw 侧是镜像。第 18 篇在三个独立位置核到「没有」：首页「**No subscription. No hosted tier. No token.**」、README「no paid tier, hosted service, or token」、页脚「no enterprise edition and no paid version」；钱从 501(c)(3) 捐赠来，机构捐助方 6 家 + GitHub Sponsors 329 位。（来源 45/56/57）【事实】

**【推断】** 同样是「agent 免费、钱在别处收」，两者的资金结构决定了不同的增长动作：Hermes 需要把星标**兑换成订阅与企业合同**，所以它的官网挂着定价表、README 挂着 `hermes setup --portal` 的一键入口、公司主页写着「the most widely used open source agent harness in the world」；OpenClaw 需要把影响力**兑换成机构预算**，所以它的官网挂着捐助方名单与「Switzerland of AI」的中立宣言。（来源 12/14/39/45/57）【推断】

**【观点】** 这一格是全篇最像「选择」的部分，也是最不自由的部分：**公司驱动能拿 $1.5B 的估值定价，也要用估值对外承诺增长；基金会驱动可以永远不为收入改路线，也永远要向少数捐助方解释自己值不值**。两条路径的后续风险，都埋在各自这一格里。

## 五、差异轴二：渠道 7 vs 20+，而 HN 高分指的东西不一样

渠道数是最直观的差距。Hermes 官网首屏列的接入面是 **Telegram、Discord、Slack、WhatsApp、Signal、Email、CLI 共 7 个**，README 特性表写 6 个（不含 Email），文档站另加 Home Assistant；OpenClaw README 写「Discord, iMessage, Slack, Teams, Telegram, WhatsApp, **and 20+ more**」，官网功能区给当日计数 **29 channels**。（来源 12/14/45/46）【事实】官方自家两套口径的差（Hermes 6 vs 7、OpenClaw 20+ vs 29）本篇并列不调和：README 给下限，官网给当日数。

渠道的第二种读法是 HN 分数结构，这一格两侧几乎反向：（来源 17/18/19/59/60/61）

| 项目 | HN 高分帖（分数 / 日期） | 性质 |
|---|---|---|
| Hermes Agent | **1,251** / 2026-04-29《HERMES.md in commit messages causes requests to route to extra usage billing》 | 事件（第三方计费） |
| Hermes Agent | **122** / 2026-06-18《Migrate from OpenClaw》 | 事件（迁移） |
| Hermes Agent | **52** / 2026-06-05《Hermes Agent – Open-source AI agent with persistent memory》 | 产品帖，链接 hermes-agent.org |
| Hermes Agent | **8** / 2026-05-19《Nous Research edits GitHub issue to remove plagiarism claims》 | 事件（治理），声量极低 |
| OpenClaw | **1,349** / 1,099 / 802 / 667 | 对抗、限制、改名，产品与话题同高分 |

【事实】Hermes 侧 `hermes agent` 的 story 检索共 **1,248 条命中**，`Hermes` 且分数 >100 的 story 有 **915 条**，但**分数最高的两条都指向事件而不是产品本身**，唯一的产品帖 52 分还链到一个取证日返回 **451** 的域名。（来源 16/17/19/41）【事实】

**【推断】** 这个结构差异可以还原成传播路径：OpenClaw 的高分帖是「你对我的项目做了什么」（模型厂限制、改名、漏洞），Hermes 的高分帖是「这个项目让第三方对我做了什么」（commit 关键词触发加钱、从 OpenClaw 迁移过来）。**同样是头条曝光，前者的叙事主体是项目，后者的叙事主体是受害者**。（来源 59/60/18/22）【推断】**【存疑】** 分数高低只反映曝光与认同度，不能推成用户规模或采用率，本篇不作此推断。

## 六、差异轴三：治理——删评改题，与把事故写成招聘启事

Hermes 侧的治理张力有一条完整的一手时间线。issue #10232（2026-04-15 由 `SunJun8` 提出）的 events 显示，2026-05-17 05:28:38–05:29:06 的 28 秒内 `NousResearch` 账号连续 **4 次 user_blocked**，05:29:32–05:29:42 的 10 秒内 **4 次 comment_deleted**，随后 05:30:02 `teknium1` 把标题从 `Genuine Question: Did We Just Witness the World's Most Sophisticated "Ctrl+C, Ctrl+V"?` **重命名为 `.`**；该 issue 的标题与正文在取证日都只剩一个点，状态 `closed / not_planned`。（来源 23/24）【事实】对应 HN 帖《Nous Research edits GitHub issue to remove plagiarism claims about Hermes Agent》只有 **8 分**。（来源 20）【事实】抄袭指控本身来自 `evomap.ai` 2026-04-11 的相似度分析长文——**指控方是卖同类基础设施的营销博客**，本篇只登记指控存在，不裁定真伪。（来源 44/47）【存疑】

对照方的同款问题走的是另一条路。OpenClaw 2026-05-05 官方复盘《OpenClaw Had a Rough Week》把发版事故直接写成组织问题：「**OpenClaw was still too founder-driven. Too much release, review, packaging and support work sat with me.**」，解法写在同一段——建基金会、招人；两个月后 10 人全职名单 + 8 个在招岗位公开。（来源 62）【事实-当事人主张】

**【推断】** 两条路径在「事故 → 组织」这条链上的差别是：**OpenClaw 把事故换成了 10 个工位，Hermes 把质疑换成了 4 个封禁和一个句点**。（来源 62/23）【推断】**【观点】** 这不是道德排序，是两种资金结构的必然——基金会要向捐助方解释「治理在改善」，公司要向市场解释「没人能证明我们抄了什么」；**同一类危机，被各自的账本翻译成了不同动作**。

## 七、吞吐与集中度：53.7% vs 91.7%，以及「一个人的项目」这件事

先把口径统一（search API 同日实测，两侧同一算法）：（来源 26/27/28/29/48/53/54/55）

| 指标 | Hermes Agent | OpenClaw | 读法 |
|---|---|---|---|
| issue 开 / 关 | 17,234 / 20,031 → **53.7%** | 5,056 / 56,178 → **91.7%** | 关 ÷（开 + 关） |
| PR 开 / 关 | 33,975 / 73,186 → **68.3%** | 2,768* / 117,531 → **97.7%** | 同上（* = 7,824 − 5,056 推得） |
| 提交 / 贡献者 | 59,466 / **401**（51,995 contributions） | 119,646 / **383**（113,798 contributions） | 仓库页 + contributors 5 页/4 页全量 |
| 第一贡献者占比 | teknium1 **24,806 = 47.7%** | steipete **67,083 = 58.9%** | 占 contributions 总量 |
| 近 100 提交作者 | Teknium 75 / kshitijk4poor 12 / calvinnwq 11 / 其余 2 | 第 18 篇未取该口径 | 单日窗口 |

【事实】两处必须说清楚的更正：① 仓库字段 `open_issues_count = 51,210` 是**开 issue + 开 PR 的和**（17,234 + 33,975 = 51,209，±1），把它当 issue 口径会算出错误的关闭率，本篇一律用 search 同口径；② 「近 20 提交全出自同一人」这类说法在近 100 提交的作者分布上**不成立**（第一人 75 条，另有两人各 12、11 条），集中度只用累计口径衡量。（来源 26/28/32/1）【事实】

**【推断】** 关闭率差距（53.7% vs 91.7%）读的不是「谁更负责」，而是**议题从哪里进来**：OpenClaw 的 issue 队列同时承担讨论区角色（`has_discussions = false`），Hermes 侧则有大量随版本迭代产生、随版本关闭的工单，两者的分母语义不同。（来源 48/54/47）【推断】**【存疑】** 本轮没有对两侧 issue 做抽样分类，「议题来源结构」只是对数字差距的一种解释，不作结论。

集中度的读法反而更直白：**两个项目的提交都高度集中**——Hermes 前 100 名贡献者里第一人占全仓 47.7%，OpenClaw 是 58.9%；近 100 条 Hermes 提交里 Teknium 一人占 75 条。（来源 30/31/51/32）【事实】**【观点】** 「383 位贡献者」「401 位贡献者」这类数字适合放在首页，不适合用来预测「如果创始人明天不写代码会怎样」——两条路径在这个问题上一样脆弱，只是其中一条已经用 10 人团队和 8 个空缺岗位把它部分补上了。（来源 18/62）

## 八、平台的两次「点名」：计费事件，与迁移潮

这是两条路径唯一正面相撞的一段。

**第一次：名字进了 commit 就被加钱。** claude-code issue **#53262**（2026-04-25T13:45 创建、同日 23:41 关闭、93 条评论）原文：当 git 提交历史里出现大小写敏感的字符串 `HERMES.md`，Claude Code 会把请求路由到 extra usage 计费而不是套餐额度，报告者实测烧掉 **$200.98**，而小写 `hermes.md`、`HERMES.txt`、`AGENTS.md`、`CLAUDE.md` 全部正常；触发条件是**提交信息里的字符串**，不是磁盘上的文件名。（来源 22）【事实】四天后的 HN 帖拿到 **1,251 分 / 531 条评论**，是本轮 Hermes 相关头条的最高分。（来源 17/18）【事实】

**第二次：迁移潮。** 官方文档站专门开了《Migrate from OpenClaw》页（`hermes import-agent openclaw`，含 dry-run、密钥白名单与预览确认），README 也留了迁移命令入口；2026-06-18 的 HN 帖《Migrate from OpenClaw》**122 分 / 105 条评论**。（来源 15/14/18）【事实】

![官方文档《Migrate from OpenClaw》取证截图，取证 2026-10-27](img/migrate.png)

同一条 HN 线程里，评论区同时点名了两件事：一是 Reddit 帖《Hermes Agent and others default installs are silently routing web traffic to Parallel》（**原帖 403 未取到，此处为 HN 二手转述**）；二是 PR **#46350**《revert(web): remove keyless Parallel search fallback》，由 `teknium1` 创建于 2026-06-14T23:37、合并于 23:47——**从创建到合并 10 分钟**，改动内容是移除无密钥的 Parallel 搜索回退、恢复必须配置 `PARALLEL_API_KEY`。（来源 18/25）【事实】【存疑】

**【推断】** 这一段可以读成一次意外的「互相背书」：模型厂的差异化计费把「用哪个 harness」变成了一个**要付钱的选择题**，于是竞品的迁移文档获得了本来拿不到的头条曝光；10 分钟合并的回滚则说明这类默认路由的暴露窗口极短，但足以进 122 分的帖子。（来源 22/17/25/18）【推断】**【存疑】** 计费路由的内部动机与 Parallel 合作的商业安排均无一手文件，本篇只对时间线做读法。

## 九、安全代价：两条路径各有一份漏洞清单，措辞值得对读

Hermes 侧两条 MITRE 记录的描述几乎逐字相同：CVE-2026-9366（2026-05-24 发布，`agent/prompt_builder.py` 的 `_scan_context_content`，注入类）与 CVE-2026-10221（2026-06-01 发布，`run_agent.py` 的 `_compress_context`，影响 up to 0.12.0），两条都写明「**The vendor was contacted early about this disclosure but did not respond in any way**」，且漏洞利用已公开；**CVSS v3.1 基础分 7.3（HIGH），同一记录的 CVSS 4.0 口径为 6.9**。（来源 34/35）【事实】PyPI 的漏洞栏还挂着第三条（CVE-2026-10224，飞书 webhook 资源耗尽），本篇不展开，只登记存在。（来源 43）【事实】

OpenClaw 侧的头号记录是 CVE-2026-33579：设备配对批准缺少调用方作用域校验导致提权，**CVSS 4.0 = 9.4**，公开于 2026-03-31，修复版本 2026.3.28 之后。（来源 58）【事实】

**【推断】** 两边的差别不在「谁有漏洞」，而在**漏洞之后的组织动作**：OpenClaw 把 9.4 分事件接进了 LTS、第三方审计与成熟度记分卡的叙事（第 18 篇第十节），Hermes 侧本轮公开证据里能点开的只有 CVE 记录本身与「厂商未回应」这句描述。（来源 58/62/34）【推断】**【存疑】** 「厂商未回应」是 CVE 记录作者的陈述，我方无法独立核证 Nous Research 的私下沟通情况，此处只转述不背书。

**【观点】** 对读者的可执行结论只有一条：**看一个 agent 项目的安全面，先看它有没有把事故接到组织改进上**——有接点的，漏洞是成本；没接点的，漏洞会在下一次头条里重新计价。

## 十、假官方域名战：两边都有冒名站，Hermes 这边还在收钱

Hermes 侧的域名面在取证日长这样：（来源 40/41/42/43/44）

| 域名 | 取证日读数 | 性质 |
|---|---|---|
| `hermes-agent.nousresearch.com` | 200，官网 + docs，API `homepage` 字段指向它 | **官方**（来源 2/12/13） |
| `hermes-agent.ai` | 200，页脚明写「This is a **fan website** — not affiliated with Nous Research」，却售卖 **FlyHermes Pro $49/mo（年付 $39/mo）**，首屏仍挂「★ **10k+** GitHub stars」（实际 257,319） | 粉丝**商业**站（来源 40/43） |
| `hermes-agent.org` | **451**（error code 1026） | 归属未证（来源 41） |
| `hermes-agent.org.cn` | 200，宣称原生支持**微信、企业微信、钉钉、飞书**与「**15+ 平台**」「63+ 社区贡献者」，页脚自称「非官方社区，由爱好者维护」 | 第三方社区站（来源 42） |
| `hermes-agent.academy` | **无响应** | 未取证成功（来源 43） |
| npm `hermes-agent` | 描述写着 "**Unofficial npm bridge**"，维护者 wyrtensi，版本却跟到 0.22.5 | 第三方桥（来源 44） |
| PyPI `hermes-agent` | 官方包（owners `dakota-nous`/`sidbin`），停在 **0.19.0（2026-07-20 上传）**，落后 GitHub v0.22.5（2026-10-23） | 官方但滞后（来源 42/33） |

![粉丝站 hermes-agent.ai 取证截图：not affiliated 声明与 FlyHermes Pro $49/mo，取证 2026-10-27](img/fansite.png)

对照方也有同款问题：`openclaw-ai.net` 自称「OpenClaw Community Guides」、宣称「50+ Platform Integrations」，与官方 29 渠道对不上。（来源 63）【事实】

**【推断】** 假官方站的密度是**成熟度的反向指标**：只有当「怎么装、有没有中文教程、有没有托管版」这三个问题每天都有人搜，SEO 导流与 $49/mo 的托管转化才划算。（来源 40/42/63）【推断】**【观点】** 但两边的冒名站性质不同——OpenClaw 的仿冒站只导流，Hermes 的粉丝站在**直接售卖与官方关系未证实的托管服务**，这把「品牌混淆」升级成了「付费前先甄别」；官方唯一可信入口是 `hermes-agent.nousresearch.com` 与 GitHub `NousResearch` 组织。（来源 40/2/63）【观点】

## 十一、资本化：$90M B 轮，与三处必须标红的自报数字

![TechCrunch 2026-10-07 报道取证截图：$90M Series B @$1.5B，取证 2026-10-27](img/techcrunch.png)

TechCrunch 2026-10-07 的可核事实是：**$90M B 轮 @$1.5B**，Robot Ventures 领投，Nvidia、Union Square Ventures、Menlo Ventures、Samsung、1789 Capital（Donald Trump Jr. 为合伙人之一）参与，**累计融资 $158M**，资金投向「Hermes for Businesses」企业线。（来源 38）【事实】

同一稿里的三段数字**性质完全不同**，必须分开读：（来源 38）

1. 「cloned more than **24 million** times, driving roughly **2.5%** of global AI token usage」——原文自带归属：**according to the startup's estimates**，公司自估、无第三方审计；
2. 「Nous was at roughly **$36 million** in annualized revenue by mid-September 2026 and expects to pass **$100 million** before the end of 2026」——原文归属是 **The Wall Street Journal** 转述，本篇未取到 WSJ 原文；
3. 「three-year-old startup」——与建仓 2025-07 的时间线一致，属可对齐口径。

【事实】【存疑】 三段里只有第 3 段能自洽核证，前两段在本篇一律按**自报/转述**处理，不进任何比较计算。

对照方的钱是另一套读法：501(c)(3) 公告、六大机构捐助方、329 位 GitHub Sponsors、以及「There is no enterprise edition and no paid version」的页脚原话。（来源 57/56）【事实】

**【推断】** 资本化这一格把两条路径的分歧推到了最远：**Hermes 用 $1.5B 的估值买到了企业线的入场券，也买到了「年底破 $100M」的公开预期**；OpenClaw 用零收入买到了「Switzerland of AI」的中立叙事，也把自己锁进了不能轻易收费的承诺。（来源 38/57）【推断】**【观点】** 到 2026 年底，这两格会各自接受第一次真实检验——一边是收入预期能否兑现，一边是捐助方名单能否继续变长。

## 十二、口径清单：哪些数字能复核，哪些只能当自报读

| 数字 | 口径类型 | 怎么读 |
|---|---|---|
| Hermes 257,319★ / 257,321★、59,466 Commits、401 贡献者 | ✅ 第三方可复核 | API 与页面双口径，Wayback ×6 同向（来源 1/2/31） |
| issue 53.7% / 91.7%、PR 68.3% / 97.7% | ✅ search 同口径 | `total_count` 原值，两侧同一算法（来源 26/27/48/54） |
| teknium1 47.7%、steipete 58.9% | ✅ 页面计数 | contributors 分页全量求和（来源 30/51） |
| 官方站 7 渠道、README 6 渠道 | ✅ 官方双口径 | 并列不调和，README 给下限（来源 12/14） |
| 「24 million clones」「2.5% global AI token usage」 | ⚠️ 公司自估 | 原文标注 startup's estimates，无第三方审计（来源 38） |
| 「$36M → $100M 年化收入」 | ⚠️ 媒体转述 | TechCrunch 引 WSJ，本篇未取到 WSJ 原文（来源 38） |
| 「most widely used open source agent harness in the world」 | ⚠️ 官方自述 | 公司主页原话，无排名口径（来源 39） |
| 粉丝站「10k+ GitHub stars」「63+ 社区贡献者」 | ❌ 与官方冲突 | 第三方站数字不采信（来源 40/42） |
| evomap.ai 指控抄袭 | 🟡 立场性材料 | 指控方营销博客，登记不裁定（来源 44） |

【事实】本篇对每一类都按上表处理，官方自报一律带标签，冲突数字一律不进正文判断。【事实】**【推断】** 横评最容易出错的地方不是数据本身，而是**把一方的自报数字与另一方的接口数字放进同一句比较**——本篇的处理办法是：先分口径，再比大小，比不了的就不比。

## 十三、结论：两条都成了，代价写在各自的账本上

把三层收拢，两条成功路径可以各压成一条线：

- **Hermes 路径（公司驱动）**：7 个月代码存量 → 官网/桌面版产品化引爆（137★ → 250,484★，7 个多月）→ 与 OpenClaw 迁移潮对撞拿到头条 → 模型厂计费事件反向背书 → 资本化（$90M @$1.5B + Hermes for Businesses）。（来源 11/9/5/18/22/38）【事实】
- **OpenClaw 路径（基金会驱动）**：周末项目 → 改名事件 + 口碑引爆（9 周 +237,633★）→ 零收入中立定位吃到个人与机构两类需求 → 事故逼出 10 人团队 + LTS + 第三方审计 → 六大捐助方排队。（来源 56/49/45/62/57）【事实】

三条我愿意押注的判断：

**第一，尖峰决定不了成败，平台期的性质才决定。** 两个项目现在都收敛到日均约 260★ 的存量速度，真正拉开差距的是同期的 release 节奏与 issue 吞吐——Hermes 取证前 61 天从 0.22.0 走到 0.22.5（来源 33/8/7），OpenClaw 在平台期里把事故换成了 10 人团队与 LTS（来源 62）。【观点】

**第二，两条路径的单点风险是同一个：提交集中度。** 47.7% 与 58.9% 说明「社区驱动」的宣传口径下，两边都是创始人驱动的项目，差别只是其中一条已经把接班问题写进了 8 个在招岗位。（来源 30/51/62）【观点】

**第三，风险事件的处理方式会变成长期资产或长期负债。** OpenClaw 把 9.4 分漏洞与一周事故写成了审计与招聘，Hermes 侧可点开的公开动作是删评改题与 10 分钟回滚——前者会被写进捐助方尽调，后者会被写进下一次头条的引用。（来源 62/23/25/58）【观点】

**【推断】** 最后留一个可执行的检查动作：**拆任何一条「成功路径」，先问三句——钱从哪来（公司、捐助还是订阅）、头条在讲事件还是在讲产品、事故之后组织变了没有。** 三句都有可点开答案的，路径才成立；只有一句是自报数字的，那条路径就还停在 PPT 上。（来源 38/17/62）【推断】

---

## 来源（取证日 2026-10-27，Hermes 侧 49 唯一来源；OpenClaw 侧复用第 18 篇证据包）

1. GitHub 仓库页 · NousResearch/hermes-agent（59,466 Commits、页内 stargazerCount 257,321、56,367 forks，截图 shot_repo）https://github.com/NousResearch/hermes-agent
2. GitHub API · 仓库元数据（stargazers 257,319 / forks 56,367 / open_issues 51,210 / watchers 987 / created 2025-07-22T22:22:28Z / pushed 2026-10-26 / MIT / homepage 官网）https://api.github.com/repos/NousResearch/hermes-agent
3. Wayback · 仓库快照 2026-02-25（aria-label 137 users starred）http://web.archive.org/web/20260225235636id_/https://github.com/NousResearch/hermes-agent
4. Wayback · 仓库快照 2026-03-08（2,297）http://web.archive.org/web/20260308221343id_/https://github.com/NousResearch/hermes-agent
5. Wayback · 仓库快照 2026-04-01（20,623）http://web.archive.org/web/20260401043902id_/https://github.com/NousResearch/hermes-agent
6. Wayback · 仓库快照 2026-06-01（174,961）http://web.archive.org/web/20260601050816id_/https://github.com/NousResearch/hermes-agent
7. Wayback · 仓库快照 2026-08-01（223,796）http://web.archive.org/web/20260801204155id_/https://github.com/NousResearch/hermes-agent
8. Wayback · 仓库快照 2026-10-01（250,484）http://web.archive.org/web/20261001130258id_/https://github.com/NousResearch/hermes-agent
9. Wayback CDX · 官网快照清单（首捕 20260305154131，此后每月一捕）http://web.archive.org/cdx/search/cdx?url=hermes-agent.nousresearch.com&output=json
10. Wayback CDX · 仓库 2025-07 → 2026-03 查询（本轮返回空体，记为空窗，不作零关注证据）http://web.archive.org/cdx/search/cdx?url=github.com/NousResearch/hermes-agent&output=json&from=20250701&to=20260301&collapse=timestamp:6m
11. GitHub API · commits（until=2025-08-01，最早提交 2025-07-26 hjc-puro、Teknium 合并 PR #1、07-31 内容抓取实现）https://api.github.com/repos/NousResearch/hermes-agent/commits?per_page=5&until=2025-08-01T00:00:00Z
12. 官方站首屏（7 渠道 Telegram/Discord/Slack/WhatsApp/Signal/Email/CLI、Portal 四档定价 $0/$20/$100/$200、200+ 模型、Five backends、MIT 免费 FAQ，截图 shot_official）https://hermes-agent.nousresearch.com
13. 官方 docs 首页（安装入口、Quick Links、入口文案）https://hermes-agent.nousresearch.com/docs/
14. README · main 分支（Six built-in terminal backends local/Docker/SSH/Modal/Daytona/Vercel Sandbox + Singularity、特性表 6 渠道、迁移命令入口、Portal 300+ models）https://raw.githubusercontent.com/NousResearch/hermes-agent/main/README.md
15. 官方 docs《Migrate from OpenClaw》（hermes import-agent openclaw、dry-run、migrate-secrets，截图 shot_migrate）https://hermes-agent.nousresearch.com/docs/guides/migrate-from-openclaw
16. HN Algolia 检索 · hermes agent story（nbHits 1,248）https://hn.algolia.com/api/v1/search?query=hermes%20agent&tags=story&hitsPerPage=30
17. HN Algolia 检索 · Hermes story 分数 >100（nbHits 915；含 1,251 分计费帖与 122 分迁移帖）https://hn.algolia.com/api/v1/search?query=Hermes&tags=story&hitsPerPage=100&numericFilters=points%3E100
18. HN · Migrate from OpenClaw（122 分 / 105 评论，2026-06-18，评论区含 Reddit Parallel 转述与 PR #46350 链接）https://hn.algolia.com/api/v1/items/48586005
19. HN · Hermes Agent – Open-source AI agent with persistent memory（52 分，2026-06-05，url 指向 hermes-agent.org）https://hn.algolia.com/api/v1/items/48419000
20. HN · Nous Research edits GitHub issue to remove plagiarism claims（8 分，2026-05-19）https://hn.algolia.com/api/v1/items/48187581
21. HN Algolia 检索 · Nous Research plagiarism（4 命中）https://hn.algolia.com/api/v1/search?query=Nous%20Research%20plagiarism&hitsPerPage=20
22. GitHub issue · anthropics/claude-code #53262（HERMES.md 触发 extra usage、实测 $200.98、93 评论、2026-04-25 当日创建与关闭）https://api.github.com/repos/anthropics/claude-code/issues/53262
23. GitHub issue events · hermes-agent #10232（2026-05-17：4 次 user_blocked + 4 次 comment_deleted + teknium1 重命名为「.」）https://api.github.com/repos/NousResearch/hermes-agent/issues/10232/events
24. GitHub issue · hermes-agent #10232（标题/正文均为「.」，closed/not_planned，原提问人 SunJun8）https://api.github.com/repos/NousResearch/hermes-agent/issues/10232
25. GitHub PR · #46350 revert(web): remove keyless Parallel search fallback（teknium1，创建 2026-06-14T23:37:44Z、合并 23:47:58Z）https://api.github.com/repos/NousResearch/hermes-agent/pulls/46350
26. GitHub search · Hermes 开 issue（total_count = 17,234）https://api.github.com/search/issues?q=repo:NousResearch/hermes-agent+type:issue+state:open&per_page=1
27. GitHub search · Hermes 关 issue（total_count = 20,031）https://api.github.com/search/issues?q=repo:NousResearch/hermes-agent+type:issue+state:closed&per_page=1
28. GitHub search · Hermes 开 PR（total_count = 33,975）https://api.github.com/search/issues?q=repo:NousResearch/hermes-agent+type:pr+state:open&per_page=1
29. GitHub search · Hermes 关 PR（total_count = 73,186）https://api.github.com/search/issues?q=repo:NousResearch/hermes-agent+type:pr+state:closed&per_page=1
30. GitHub API · contributors 第 1 页（teknium1 24,806、第 2–4 页与末页合计 401 人 / 51,995 contributions）https://api.github.com/repos/NousResearch/hermes-agent/contributors?per_page=100&page=1
31. GitHub API · contributors Link 头与末页（rel=last 指向 page=5，末页 1 人 → 401 人）https://api.github.com/repositories/1024554267/contributors?per_page=100&page=5
32. GitHub API · commits 近 100 条（作者分布 Teknium 75 / kshitijk4poor 12 / calvinnwq 11 / Jeffrey Quesnelle 1 / emozilla 1，窗口 2026-10-25→10-26）https://api.github.com/repos/NousResearch/hermes-agent/commits?per_page=100
33. GitHub API · releases（最新 v0.22.5 2026-10-23；v0.22.0 "The Threshold Release" 2026-10-14 等 20 条）https://api.github.com/repos/NousResearch/hermes-agent/releases?per_page=20
34. MITRE CVE API · CVE-2026-9366（_scan_context_content 注入、CVSS 3.1 = 7.3 / 4.0 = 6.9、"did not respond in any way"、发布 2026-05-24）https://cveawg.mitre.org/api/cve/CVE-2026-9366
35. MITRE CVE API · CVE-2026-10221（_compress_context 注入、up to 0.12.0、CVSS 3.1 = 7.3 / 4.0 = 6.9、发布 2026-06-01）https://cveawg.mitre.org/api/cve/CVE-2026-10221
36. cve.org · CVE-2026-9366 记录页（同号记录的官方展示面）https://www.cve.org/CVERecord?id=CVE-2026-9366
37. cve.org · CVE-2026-10221 记录页 https://www.cve.org/CVERecord?id=CVE-2026-10221
38. TechCrunch 2026-10-07《Nous Research confirms it hit $1.5B valuation, launches AI agents for business users》（$90M B 轮、Robot Ventures 领投、Nvidia/USV/Menlo/Samsung/1789、累计 $158M、24M clones 与 2.5% token 为 startup's estimates、WSJ 转述 $36M→$100M、Hermes for Businesses，截图 shot_techcrunch）https://techcrunch.com/2026/10/07/nous-research-confirms-it-hit-1-5b-valuation-launches-ai-agents-for-business-users/
39. Nous Research 官网（"We created Hermes Agent, the most widely used open source agent harness in the world"，官方自述）https://nousresearch.com
40. 粉丝站 hermes-agent.ai（页脚 "This is a fan website — not affiliated with Nous Research"、FlyHermes Pro $49/mo 或 $39/mo 年付、首屏 "10k+ GitHub stars"，截图 shot_fansite）https://hermes-agent.ai
41. hermes-agent.org（取证日 451，error code 1026；归属未证）https://hermes-agent.org
42. hermes-agent.org.cn（宣称微信/企业微信/钉钉/飞书原生支持、15+ 平台、63+ 社区贡献者；页脚「非官方社区，由爱好者维护」）https://hermes-agent.org.cn
43. PyPI · hermes-agent（官方包，owners dakota-nous/sidbin，latest 0.19.0 上传 2026-07-20；vulnerabilities 栏含 CVE-2026-10221 与 CVE-2026-10224）https://pypi.org/pypi/hermes-agent/json
44. npm · hermes-agent（description "Unofficial npm bridge for Hermes Agent"、maintainer wyrtensi、latest 0.22.5）https://registry.npmjs.org/hermes-agent
45. OpenClaw 官网首页（"No subscription. No hosted tier. No token."、29 channels、页脚 no enterprise edition and no paid version）https://openclaw.ai/
46. OpenClaw README · main 分支（"no paid tier, hosted service, or token"、Discord/iMessage/Slack/Teams/Telegram/WhatsApp and 20+ more）https://raw.githubusercontent.com/openclaw/openclaw/main/README.md
47. GitHub API · openclaw/openclaw（stargazers 393,064 / forks 82,618 / open_issues 7,824 / created 2025-11-24，第 18 篇证据包）https://api.github.com/repos/openclaw/openclaw
48. GitHub search · OpenClaw issue（开 5,056 / 关 56,178，第 18 篇证据包 + 本次复核）https://api.github.com/search/issues?q=repo:openclaw/openclaw+type:issue+state:closed
49. Wayback · OpenClaw 仓库快照 2026-01-30（106,177★，第 18 篇证据包）https://web.archive.org/web/20260130041944id_/https://github.com/openclaw/openclaw
50. Wayback · OpenClaw 仓库快照 2026-04-01（343,810★，第 18 篇证据包）https://web.archive.org/web/20260401084632id_/https://github.com/openclaw/openclaw
51. GitHub API · OpenClaw contributors 第 1 页（steipete 67,083、top100 合计 109,404，第 18 篇证据包）https://api.github.com/repos/openclaw/openclaw/contributors?per_page=100&page=1
52. GitHub 仓库页 · openclaw/openclaw（119,646 Commits、页内 stargazerCount 393,064，第 18 篇证据包；第 18 篇正文写 119,647，差 1 为逐日计数浮动）https://github.com/openclaw/openclaw
53. GitHub search · OpenClaw 开 issue（5,056）https://api.github.com/search/issues?q=repo:openclaw/openclaw+type:issue+state:open
54. GitHub search · OpenClaw 关 issue（56,178 → 91.7%）https://api.github.com/search/issues?q=repo:openclaw/openclaw+type:issue+state:closed
55. GitHub search · OpenClaw 关 PR（117,531 → 97.7%）https://api.github.com/search/issues?q=repo:openclaw/openclaw+type:pr+state:closed
56. OpenClaw 官方博客《Introducing OpenClaw》（2026-01-29，周末项目自述、Clawd→Moltbot→OpenClaw、100,000★ 与 2 million visitors）https://openclaw.ai/blog/introducing-openclaw
57. OpenClaw 官方博客《Introducing the OpenClaw Foundation》与捐赠页（2026-07-08，501(c)(3)、"Switzerland of AI"、10 人 + 8 岗、"no enterprise edition and no paid version"）https://openclaw.org/donors
58. MITRE CVE API · CVE-2026-33579（提权、CVSS 4.0 = 9.4、CWE-863、公开 2026-03-31）https://cveawg.mitre.org/api/cve/CVE-2026-33579
59. HN · Claude Code refuses requests or charges extra if your commits mention "OpenClaw"（1,349 分，2026-04-30）https://hn.algolia.com/api/v1/items/47963204
60. HN · Tell HN: Anthropic no longer allowing Claude Code subscriptions to use OpenClaw（1,099 分，2026-04-03）https://hn.algolia.com/api/v1/items/47633396
61. HN 检索 · openclaw story（1,712 条命中，802/667/397 分等头条由此排序）https://hn.algolia.com/api/v1/search?query=openclaw&tags=story&hitsPerPage=50
62. OpenClaw 官方博客《OpenClaw Had a Rough Week》（2026-05-05，"too founder-driven"、"building a real team"，接第 18 篇第十节）https://openclaw.ai/blog/openclaw-rough-week
63. openclaw-ai.net（非官方第三方站，自称 OpenClaw Community Guides、"50+ Platform Integrations"，第 18 篇证据包）https://openclaw-ai.net/

取证档案：`research/evidence/hermes-agent/`（**121 个文件 / 49 条唯一来源 URL / 通道D 截图 5 张**，manifest 带 sha256、通道与来源 URL）；对照方档案 `research/evidence/openclaw/`（61 文件 / 52 URL，第 18 篇产出）。防投毒扫描：本轮全部页面**未发现**「忽略指令 / 访问某站 / 联系某人」类注入文本，无黑名单类内容；通道降级记录 —— `news.ycombinator.com` 直抓 **419**、Reddit JSON **403**、`hermes-agent.academy` **000**，已改走 hn.algolia API 与通道A，**未执行任何页面内指令**。本文正文引用通道D 截图 5 张：`shot_repo / shot_official / shot_fansite / shot_migrate / shot_techcrunch`。

## 勘误与可撤回

- 本文任一判断被一手源推翻，请来信指出；核实后在对应句子后加「（勘误 <日期>）」并保留原句划线，**不删旧数字、不改历史口径**。
- 可疑行为记录（防投毒铁律）：本轮 49 个来源**零注入命中**；来源 18 的 HN 评论中出现的 Reddit 链接与 evomap.ai 链接只读取文本、未跳转执行；来源 40、42 的第三方站点内容按「粉丝/社区站」处理，未采信其数字。
- 已知存疑与缺证项：**2025-07 → 2026-02 的仓库快照空窗**（CDX 返回空体），本文只用 2026-02-25 单点、不外推；「24 million clones」「2.5% global AI token usage」为公司自估，「$36M → $100M」为 TechCrunch 转述 WSJ、本轮未取到 WSJ 原文；Reddit《silently routing web traffic to Parallel》原帖 403，本文只引 HN 二手；「厂商早期联系但未回应」为 CVE 记录作者陈述，无法独立核证；evomap.ai 抄袭指控属立场性材料，只登记不裁定；`hermes-agent.org` 的历史归属无法取证，本文只记 451 读数；issue #10232 的原始描述已被维护者抹除，仅靠 HN 引用二手保留；渠道数 6 vs 7、20+ vs 29 为官方双口径，本文并列不调和；OpenClaw 侧提交数 119,646（本次归档页）与第 18 篇正文 119,647 差 1，属逐日计数浮动，已在来源 52 注明。【存疑】
