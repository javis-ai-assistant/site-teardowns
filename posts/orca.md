# 9★ 到 119,087★ 只用 7 个月 23 天，Show HN 却只有 21 分：拆 Orca 商业画布，钱不在 IDE 里

> 拆的是 YC W22 团队 Stably AI 开源的 agent 开发环境 Orca：建仓七个半月星标冲到 119,087，Show HN 却只有 21 分。读完你能拿到它的增长曲线、三条钱线与六个同名混淆面，每个数字都附可点开的出处。

取证日期：2026-11-09（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 20 · 可验证性 18 · 独特性 17 · 受众相关度 18 · 多维空间 17 = **90/100**（≥60 才动笔；素材库预分 85）。框架卡 = **商业画布**（第 19 篇是对比横评，轮换合规；本篇不做功能测评）：按「**产品格 → 增长格 → 三条钱线 → 渠道格 → 资源格 → 风险格**」六格走，每一格给可复核读数。题眼一句话：**一个 7 个月 23 天冲到 119,087★ 的 IDE，自己的定价页却是 404——钱从产品里绕出去，走了另外两条线**。（来源 1/25/30）

![GitHub 仓库页取证截图：119,039★、20,062 Commits、MIT，取证 2026-11-09](img/shot_repo.png)

## 一、产品格：把「多 agent 并行」从工作流做成环境

Orca 的一句话定位在仓库 description 与主站首屏是同一句的两个版本：仓库写「Orca lets you run a fleet of coding agents (e.g. Claude Code, Codex) with your own subscription. Available on desktop, mobile and remote runtime」；主站写「Ship 100x with the agent IDE」，上方挂着 **Backed by Y Combinator** 徽标，导航栏右侧常驻 **119k** 星标计数。（来源 1/2/24）

产品结构是四件套的组合：**并行 worktree**（每个 agent 一个隔离 git worktree，互不踩脚）、**Chat/终端双形态**（同一对话能在 UI 与 CLI 间搬）、**远程运行**（SSH 主机/云 VM，笔记本合盖 agent 继续跑）、**移动端伴侣**（iOS/Android 收通知、回消息、放行 PR）。README 点名 **34 个 CLI agent** 的 logo 墙，末尾一个「+ any CLI agent」兜底；主站对比表则写「**27 supported agents**」——同一产品两处口径差 7 个，本文并列不调和，README 给实现清单、主站给对比表口径。（来源 11/24）

![官方站首屏取证截图：Ship 100x with the agent IDE 与 YC 徽标，取证 2026-11-09](img/shot_home.png)

品类命名本身有一条可取证的演进线。仓库 `<title>` 随 Wayback 快照五次改口：2026-03-27「next-gen IDE for **building** with coding agents」→ 06-06「…**working with a fleet** of parallel agents…desktop and mobile」→ 07-04 换词「**ADE** for working with a fleet…」→ 08-04 加「and VPS」→ 10-01「and remote runtime」。主站 FAQ 专门留了一题「How is an ADE different from an IDE?」——**从 IDE 改叫 ADE（Agent Development Environment）发生在 2026 年 7 月初**，是把「编辑器」重定义成「给 agent 住的环境」的一次品类改名。（来源 3/6/7/8/10/24）【事实】

**【推断】** 改名不是修辞游戏：description 里每次加词（fleet → ADE → VPS → remote runtime）都对应一次能力扩面，搜索词也从「IDE」挪向「agent 环境」——品类词换轨通常是为了躲开既有红海的关键词竞争，同时给自己留一个「第一名」的空位。（来源 3/6/7/8/10/24）

## 二、增长格：9★ → 119,087★，7 个月 23 天，而且还在加速

星标曲线全部由 Wayback 快照 `aria-label="… users starred"` 原文读出，非估读；终值用 API 双口径收口。（来源 1/2/3/4/5/6/7/8/9/10/48）

| 快照日 | 星标 | 区间 | 区间日均 |
|---|---|---|---|
| 2026-03-27 | **9** | 建仓后 10 天 | — |
| 2026-04-02 | **131** | +122 / 6 天 | 20/天 |
| 2026-04-15 | **1,064** | +933 / 13 天 | 72/天 |
| 2026-06-06 | **4,256** | +3,192 / 52 天 | 61/天 |
| 2026-07-04 | **11,666** | +7,410 / 28 天 | 265/天 |
| 2026-08-04 | **37,081** | +25,415 / 31 天 | 820/天 |
| 2026-09-01 | **58,480** | +21,399 / 28 天 | 764/天 |
| 2026-10-01 | **82,486** | +24,006 / 30 天 | 800/天 |
| 2026-11-09（API） | **119,087** | +36,601 / 39 天 | 938/天 |

【事实】仓库创建时间 **2026-03-17T03:28:57Z**，到取证日 236 天 = **7 个月 23 天**；10 万★ 落在 10 月 20 日前后（82,486 → 119,087 区间内插），**距建仓约 7 个月**——素材库预分条目写的「约 13 个月冲 10 万★」是错的，本文按 API created_at 更正。（来源 1/10/48）

【事实】更值得记的是**这条曲线没有平台期**：7 月 265/天 → 8 月 820/天 → 9 月 764/天 → 10 月 800/天 → 11 月前 9 天 **938/天**，取证前的增速反而是全程最高段。对照第 19 篇两条爆款曲线最终都收敛到日均约 260★，Orca 这条还在爬坡。（来源 3–10，第 19 篇读数）

**【推断】** 起步段（3 月底到 6 月初，日均 20～72★）说明 Show HN 那 21 分确实没带量；真正的扳机在 6 月中到 8 月初——日均从 61 拉到 820，与主站上线、移动端上架（App Store 2026-05-07）、README 多语言铺开是同一窗口。（来源 4/5/6/7/8/31）

## 三、钱线一：Orca 本体 0 收入——定价页是 404

商业画布最反常识的一格。`onorca.dev/pricing` 取证日返回 **404**（页面原文「This page could not be found」）；主站 FAQ 的 JSON-LD 直接自答「Is Orca free? → **Yes, Orca is completely free and open source under the MIT license.** Download it for macOS, Windows, or Linux at no cost.」；首屏另一处写着「**Bring your own agent / subscription**——Plug in the subscriptions you already have and run them side by side in Orca」。（来源 25/24/11）

**【事实】** 也就是说：用户的模型订阅费（Claude Max、Codex、Grok…）**付给 Anthropic/OpenAI，不经过 Orca 一分钱**；Enterprise 页把这条原则写成口号「**No model in the middle**——Prompts and code go straight to the providers you configure. Orca does not inspect or store them.」。（来源 26/24）

**【观点】** 这不是「先免费换增长」的常规补贴打法，而是刻意把钱线从产品里**摘出去**：一个管 34 个 agent 的环境如果抽 token 成色，就同时得罪 34 家模型商和所有用户；把通道让干净，产品才有资格自称中立的编排层。

## 四、钱线二：母产品 Stably QA 的 $0/$60/$250 订阅

钱的第一条实体线在隔壁。Stably 是同一公司（YC 页名「**Stably AI (Orca)**」）的 AI 测试产品，定价页四档一手可查：**Hobby $0**（含 $10/月 credits、1 并发浏览器）/ **Team $60/月**（含 $60 credits、50 并发）/ **Growth $250/月**（含 $250 credits、100 并发，标 Most popular）/ **Enterprise Custom**（SAML SSO、SLA、7×24）。credits 计价也公开：**浏览器 $0.01/分钟**、**AI tokens 按模型 $0.30–15/百万**。（来源 29）

【事实】Stably 首页把 Orca 挂在最顶上获客：徽标行「**New from Stably · Open source · MIT**」+ 标题「Meet Orca IDE — the agentic IDE for parallel coding agents」+ 一句「Before you test, you build.」——**开源的 Orca 在母公司的商业页里充当入口**。（来源 28）

**【推断】** 三层结构拼起来是：**Orca（免费、0 收入、要的是星标与安装）→ Stably QA（$0/$60/$250 订阅，credits 吃用量）→ Enterprise（定制合同）**。开源 IDE 在画布里不是收入来源，是漏斗最宽的一截；YC 页写明公司 2022 年由 Jinjing Liang 与 Neil Parker 创立、**25 人旧金山**、批次 **W22（Winter 2022）**——一个做了四年的测试产品公司，2026 年 3 月用一个新仓库切进 agent 赛道。（来源 30/1/29）

![Stably 定价页取证截图：Hobby $0 / Team $60 / Growth $250 / Enterprise，取证 2026-11-09](img/shot_pricing.png)

## 五、钱线三：Enterprise = contact sales + SOC 2 只到「Readiness」

第三条线是团队版销售。Enterprise 页没有价格表，CTA 是「**Get in touch** / **Email us**」，正文承诺三件套：**No silent changes**（每个 agent 改动都落 worktree + PR，和人写的代码同流程评审）、**Audit trail by default**（git 历史、PR、workspace 活动记录谁在何时跑了什么）、**Your providers, your keys**（模型与 CLI 用团队已批准的，Orca 不夹在中间）。（来源 26）

【事实】合规一栏的原文是「**AICPA SOC 2 Readiness**」——**Readiness（就绪/准备中）不等于 Certified（已认证）**；FAQ 里「Is Orca SOC 2 compliant?」的答案按钮在页内、正文未直接给出「已通过」字样。本文按页面原文采信为「就绪状态」，不写成「已合规」。（来源 26）【事实】

![Orca Enterprise 页取证截图：SOC 2 Readiness 与 No silent changes，取证 2026-11-09](img/shot_enterprise.png)

**【观点】** 三条钱线的画布读法：**本体 0 元换规模、母产品订阅吃用量、企业版吃合同**。这种结构的软肋也清楚——一旦 Orca 的增长停在 119k★ 不再往前，漏斗顶端断供，Stably 与 Enterprise 两条线的获客成本就得自己补；所以星标曲线的「还在加速」（第二节）不是虚荣指标，是这条画布的供血读数。

## 六、渠道格：Show HN 21 分——流量不靠讨论

反常识钩子在这一格。2026-03-27，作者 `nwparker`（GitHub bio：**Neil · CTO - Stably.ai**）亲手发《Show HN: Open Source 'Conductor + Ghostty'》，**21 分、10 条评论**；algolia 检索 `stablyai` 的 story 面，唯一另一个产品帖是 2026-02-05 的 agent-slack（99 分）——**一个 119,087★ 的项目，HN 讨论面近似于零**。（来源 21/22/23/35）

评论区留了两处一手细节：`johntash` 当日指出「README 说 MIT，但**仓库里没有 LICENSE 文件**」；`nwparker` 本人逐条回复认领（还自嘲修了链接）。这与 andrew.ooo 独立评测记录的 LICENSE 三段史对上：**无 LICENSE 文件（3 月）→ 对比表与 FAQ 写着 Apache 2.0（9 月修正前）→ 现在全站 MIT**。（来源 22/32/1）

那流量从哪来？四条渠道全部有读数：

- **GitHub trending 周榜**：2026-10-25 抓取周榜在列，**周 +9,828★**（来源 43）
- **双端商店**：App Store **2026-05-07** 上架，**4.69 分 / 74 个评分**，当前版 0.0.63（2026-11-04 更新）（来源 31）
- **插件市场**：**169 个插件 = 43 官方 + 126 社区**（主题/语言/图标/编辑器/命令面板/远程 VM 六类）（来源 27）
- **README 九语言**：中文/日/韩/西/法/葡/土/俄直译铺开（来源 11）

**【推断】** 渠道结构拼出来的结论：**这是「安装分发型」流量，不是「讨论型」流量**——Show HN 21 分与 trending 周 9,828★ 并存，说明用户是「看见 → 装 → 用」，不是「围观 → 转发 → 讨论」。对工具类产品这是更健康的形态：讨论会衰减，安装会累积。（来源 21/43/31）

![Hacker News Show HN 原帖取证截图：21 分，取证 2026-11-09](img/shot_hn.png)

## 七、资源格：467 名贡献者，top1 是 CTO 本人

仓库的家底四组读数（全部 2026-11-09 实测）：

- **提交**：仓库页 **20,062 Commits**；近 100 条提交作者分布 OrcaWin 35 / nwparker 25 / AmoabaKelvin 12 / AmethystLiang 8——核心圈层稳定，非 bot 主导（github-actions 累计仅 795）（来源 2/15/14）
- **贡献者**：Link 头 `rel=last page=5` 全量 **467 人 / 19,884 次贡献**；top10 = nwparker **7,488（37.7%）**、brennanb2025 2,941、Jinwoo-H 2,315、AmethystLiang 2,209、OrcaWin 2,109、github-actions[bot] 795、AmoabaKelvin 653…（来源 14/35）
- **发布**：releases API 翻满 10 页 **1,000 个 release / 16,067 个资产，合计 158,707,178 次下载**；最早 2026-03-22、最新 v1.4.253（2026-11-07），230 天约 **4.3 个 release/天**，v1.4.x 线占 491 个；README 徽章自报 **158m**，与 API 加总对得上（来源 12/13）
- **关注**：watchers **仅 158**，对照 119,087★——**看的人多、盯的人极少**，与第六节「安装分发型」互证（来源 1/2）

**【推断】** top1 = CTO 本人 37.7% 的集中度，对「开源项目单点风险」是一半一半的读法：好处是方向不散、review口令统一（467 人里绝大多数是外围提交），代价是关键路径压在一个人身上——近 100 提交里 nwparker 仍占 25%，与 3 个月前的密度没有衰减。（来源 14/15）

## 八、风险格：LICENSE 三段史、bug 挂账 1,232、六个同名混淆面

**同口径的健康度**（search API 同算法，2026-11-09）：开 issue **4,492** / 关 **6,014** → 关闭率 **57.2%**；开 PR 3,651 / 关 **25,431** → **87.4%**；`label:bug` 开放 **1,232**。（来源 16/17/18/19/20）【事实】素材库预分条目写的「8,565 open issues」是 `open_issues_count` 的 **issue+PR 混口径**（4,492+3,651≈8,148），与第 19 篇 Hermes 踩过的是同一个坑，本文按同口径更正。（来源 16/18）

**LICENSE 三段史**（见第六节）：3 月无文件 → 9 月前表内 Apache 2.0 残留 → 现 MIT。（来源 22/32/1）**安全面**：GitHub Security Advisories **published 0 条**、MITRE/cve.org 无本项目 CVE；security 页披露范围写明「本地或网络可达的 Orca 服务」在范围、第三方 agent CLI 不在。（来源 33/34）第三方站 releaseport.com 声称 v1.4.201/196 含「CVE fixes」，与 0 条通告冲突，**系镜像/SEO 站口径，不采信**。（来源 46）

**六个同名混淆面（避坑清单）**：

1. **meetc-orca.ai**「Orca AI」个人助理，Starter **$29.99/月**、Pro **$39.99/月**——他家产品，与本项目无关（来源 36）
2. **github.com/orca-ae/orca-agent-engine**：另一「Orca」运行时，HN 仅 2 分（来源 37）
3. **github.com/mathisdrn/orca**：同名数据仓库（来源 38）
4. **OrcaSlicer**：3D 打印切片软件，HN 上同名热度常年更高（来源 39 侧证）
5. **微软 Orca**（LLM 论文系）：OpenCVE 按 `product=orca&vendor=microsoft` 检索到的是它（来源 39）
6. **orca.dev 的停放史**：Wayback 显示该域名 2024-06-25、2025-07-16 两度都是 **Porkbun 停放页**，现 302 跳到 onorca.dev；onorca.dev 首份快照 2026-04-03——**主域名与仓库 homepage 字段（orca.dev）不同步**（来源 40/41/42/1）

【事实】引用第三方数字必须带日期：andrew.ooo（独立评测，2026-11-08 自述实测）**118,790★ / 7 天 +5,873 / 4,478 open issues**；quantaailabs 写 29,131★、agentconn 写 67K+——**均为滞后或营销口径，不作真值**；韩文第三方 zerodraftlab 称「4 人团队」与 YC 页「25 人」冲突，只登记不裁定。（来源 32/44/45/47/30）

![YC 公司页取证截图：Stably AI (Orca) · W22 · 25 employees，取证 2026-11-09](img/shot_yc.png)

## 九、画布总评：六格读完，剩下的三个问题

把六格收拢：**产品格**（并行 worktree 环境，IDE→ADE 改名有据）｜**增长格**（236 天 9→119,087★，日均还在爬到 938）｜**钱线一**（本体 404 定价、BYO 订阅、No model in the middle）｜**钱线二**（Stably QA $0/$60/$250 + Orca 首页入口）｜**渠道格**（Show HN 21 分 vs trending 周 9,828★，安装分发型）｜**风险格**（LICENSE 三段史、SOC 2 仅 Readiness、同口径关闭率 57.2%、六个同名面）。

**【观点】** 这张画布最值得同行抄的一格，是把「收入」和「增长」拆到了两个互不打架的账户里：Orca 永远不收钱，所以 34 家模型商和所有用户都不把它当敌人；Stably 收订阅，但漏斗由一个谁也锁不住的开源入口喂养。代价也写在页上——**合规停在 Readiness、watchers 只有 158、top1 是 CTO 本人**：规模是借来的注意力，组织化还没跟上。

三个留给下轮取证的问题：① 1,000 个 release 的日更节奏能否撑过星标拐点（现读数 938/天仍在升）② Enterprise 有没有第一份公开可查的客户案例③ issue 关闭率 57.2% 会不会在团队扩张后继续掉——三个问题都只需同一组 API 重跑即可复核。

## 来源

1. GitHub API · repo stablyai/orca（created 2026-03-17T03:28:57Z、API 星标 119,087、MIT、watchers 158、open_issues 8,148）https://api.github.com/repos/stablyai/orca
2. GitHub 仓库页（页面星标 119,039、20,062 Commits，2026-11-09 实测）https://github.com/stablyai/orca
3. Wayback · 仓库快照 2026-03-27（9★；title「next-gen IDE for building with coding agents」）https://web.archive.org/web/20260327225932id_/https://github.com/stablyai/orca
4. Wayback · 仓库快照 2026-04-02（131★）https://web.archive.org/web/20260402191809id_/https://github.com/stablyai/orca
5. Wayback · 仓库快照 2026-04-15（1,064★）https://web.archive.org/web/20260415104242id_/https://github.com/stablyai/orca
6. Wayback · 仓库快照 2026-06-06（4,256★；title 加「fleet of parallel agents…desktop and mobile」）https://web.archive.org/web/20260606020311id_/https://github.com/stablyai/orca
7. Wayback · 仓库快照 2026-07-04（11,666★；title 换词「ADE」）https://web.archive.org/web/20260704000406id_/https://github.com/stablyai/orca
8. Wayback · 仓库快照 2026-08-04（37,081★；title 加「and VPS」）https://web.archive.org/web/20260804120937id_/https://github.com/stablyai/orca
9. Wayback · 仓库快照 2026-09-01（58,480★）https://web.archive.org/web/20260901022949id_/https://github.com/stablyai/orca
10. Wayback · 仓库快照 2026-10-01（82,486★；title 加「remote runtime」）https://web.archive.org/web/20261001041721id_/https://github.com/stablyai/orca
11. README · main 分支（34 个点名 CLI agent + any CLI agent、Bring your own subscription、九语言目录）https://raw.githubusercontent.com/stablyai/orca/main/README.md
12. 下载徽章 SVG（downloads: 158m，Total downloads across all releases）https://raw.githubusercontent.com/stablyai/orca/main/docs/assets/readme-downloads.svg
13. GitHub API · releases 第 1 页 + Link 头（rel=last 指向 page=11；10 页全量 1,000 版 / 16,067 资产合计 158,707,178 次下载；最早 2026-03-22、最新 v1.4.253 2026-11-07）https://api.github.com/repos/stablyai/orca/releases?per_page=100
14. GitHub API · contributors Link 头与末页（rel=last page=5 → 467 人 / 19,884 次贡献；top1 nwparker 7,488）https://api.github.com/repositories/1183888342/contributors?per_page=100&page=1
15. GitHub API · commits 近 100 条（OrcaWin 35 / nwparker 25 / AmoabaKelvin 12 / AmethystLiang 8，窗口 2026-11-08）https://api.github.com/repos/stablyai/orca/commits?per_page=100
16. GitHub search · 开 issue（total_count = 4,492）https://api.github.com/search/issues?q=repo:stablyai/orca+type:issue+state:open&per_page=1
17. GitHub search · 关 issue（total_count = 6,014 → 57.2%）https://api.github.com/search/issues?q=repo:stablyai/orca+type:issue+state:closed&per_page=1
18. GitHub search · 开 PR（total_count = 3,651）https://api.github.com/search/issues?q=repo:stablyai/orca+type:pr+state:open&per_page=1
19. GitHub search · 关 PR（total_count = 25,431 → 87.4%）https://api.github.com/search/issues?q=repo:stablyai/orca+type:pr+state:closed&per_page=1
20. GitHub search · label:bug 开放（total_count = 1,232）https://api.github.com/search/issues?q=repo:stablyai/orca+type:issue+state:open+label:bug&per_page=1
21. HN · Show HN: Open Source 'Conductor + Ghostty'（21 分，作者 nwparker，2026-03-27T22:26:40Z；人工入口 news.ycombinator.com/item?id=47549197）https://hn.algolia.com/api/v1/items/47549197
22. HN · 该帖评论检索（10 条；johntash「no LICENSE file in the repo」、nwparker 本人逐条回复）https://hn.algolia.com/api/v1/search?tags=comment,story_47549197&hitsPerPage=20
23. HN · stablyai story 检索（唯一另一产品帖 agent-slack 99 分，2026-02-05）https://hn.algolia.com/api/v1/search?query=stablyai&tags=story&hitsPerPage=30
24. Orca 官网首页（Ship 100x with the agent IDE、Backed by Y Combinator、119k 导航徽章、27 supported agents 对比表、ADE FAQ、Is Orca free? JSON-LD 自答）https://www.onorca.dev/
25. Orca 官网 /pricing（取证日 404，页面原文 This page could not be found）https://www.onorca.dev/pricing
26. Orca Enterprise 页（AICPA SOC 2 Readiness、No silent changes、Audit trail by default、Your providers your keys、No model in the middle、Get in touch / Email us）https://www.onorca.dev/enterprise
27. Orca 插件市场（All 169 = Official 43 + Community 126；六类筛选器）https://www.onorca.dev/plugins
28. Stably 官网首页（New from Stably · Open source · MIT 推广块、Meet Orca IDE、Before you test, you build）https://www.stably.ai/
29. Stably 定价页（Hobby $0 含 $10 credits / Team $60 / Growth $250 / Enterprise Custom；浏览器 $0.01/分钟、AI tokens $0.30–15/百万）https://www.stably.ai/pricing
30. YC 公司页 · Stably AI (Orca)（批次 W22 Winter 2022、2022 年由 Jinjing Liang 与 Neil Parker 创立、25 人旧金山、one_liner「We make Orca, MIT open source terminal-based agent orchestrator」）https://www.ycombinator.com/companies/stably-ai-orca
31. App Store lookup API · Orca IDE（2026-05-07 上架、4.689 分 / 74 个评分、当前 0.0.63、2026-11-04 更新）https://itunes.apple.com/lookup?id=6766130217
32. andrew.ooo 独立评测（2026-11-08 自述实测 118,790★ / +5,873 七天 / 4,478 open issues；2026-10-27 记 107,683★；LICENSE 修正史与 Apache 2.0 残留记录）https://andrew.ooo/posts/orca-stablyai-parallel-coding-agents-ide-review/
33. GitHub API · Security Advisories（state=published 返回 0 条）https://api.github.com/repos/stablyai/orca/security-advisories?state=published
34. GitHub Security 页（披露范围：本地或网络可达的 Orca 服务；第三方 agent CLI/用户 Git/SSH 不在范围）https://github.com/stablyai/orca/security
35. GitHub users · nwparker（bio：Neil · CTO - Stably.ai，company @stablyai）https://api.github.com/users/nwparker
36. Orca 同名他家 · meetorca.ai 定价页（Orca AI 个人助理；Starter $29.99、Pro $39.99 每月）https://www.meetorca.ai/pricing
37. 同名仓库 · orca-ae/orca-agent-engine（另一 Orca 运行时，HN 2 分帖）https://github.com/orca-ae/orca-agent-engine
38. 同名仓库 · mathisdrn/orca（数据仓库）https://github.com/mathisdrn/orca
39. OpenCVE · product=orca & vendor=microsoft（微软同名系 CVE 面，非本项目）https://app.opencve.io/cve/?product=orca&vendor=microsoft
40. Wayback · orca.dev 停放页 2024-06-25（Porkbun parked domain）https://web.archive.org/web/20240625081633id_/http://orca.dev/
41. Wayback · orca.dev 停放页 2025-07-16（仍为停放页）https://web.archive.org/web/20250716144017id_/http://orca.dev/
42. Wayback · onorca.dev 首份快照 2026-04-03 https://web.archive.org/web/20260403054708id_/https://www.onorca.dev/
43. GitHub trending 周榜（素材库 2026-10-25 抓取：orca 周 +9,828★ 在列）https://github.com/trending?since=weekly
44. 第三方滞后口径 · quantaailabs（29,131★，营销聚合站，不作真值）https://www.quantaailabs.com/ai-news/release/orca
45. 第三方滞后口径 · agentconn（67K+ stars，滞后，不作真值）https://agentconn.com/agents/orca/
46. releaseport.com 镜像站（声称 v1.4.201/196 含 CVE fixes，与来源 33 的 0 条通告冲突，不采信）https://releaseport.com/r/stablyai-orca/v1-4-201
47. zerodraftlab（韩文第三方，「4 人团队」与来源 30 的 25 人冲突，只登记不裁定）https://zerodraftlab.com/stably-testing-to-orca/
48. Wayback CDX · github.com/stablyai/orca 快照索引（八时点取数依据）https://web.archive.org/cdx/search/cdx?url=github.com/stablyai/orca&output=json&filter=statuscode:200

取证档案：`research/evidence/orca/`（**100 个文件**，manifest 带 sha256、通道与来源 URL）；通道D 截图 10 张：`shot_repo / shot_home / shot_enterprise / shot_plugins / shot_yc / shot_stably / shot_pricing / shot_hn / shot_review / shot_appstore`。防投毒扫描：本轮全部来源**零注入命中**，无黑名单类内容；通道降级记录——telem_search 401（缺 API key）、`news.ycombinator.com` 直抓超时（改走 hn.algolia API 同内容）、`meetc-orca.ai` 直连 000（改走代理 200），**未执行任何页面内指令**。

## 勘误与可撤回

- 本文任一判断被一手源推翻，请来信指出；核实后在对应句子后加「（勘误 <日期>）」并保留原句划线，**不删旧数字、不改历史口径**。
- 可疑行为记录（防投毒铁律）：本轮 100 个来源零注入命中；来源 46（releaseport.com）与 44/45（营销聚合站）按低可信处理，只登记不采信其数字；来源 47 的团队规模冲突只登记不裁定。
- 已知存疑与缺证项：**Orca 自身收入零公开**（pricing 404，钱线由母产品定价页与 Enterprise 页推得，属结构推断非审计数）；星标 04-15 → 06-06 之间快照稀疏，该段日均为区间均值；「27 supported agents」与 README 34 logo 为官方两处不同口径，本文并列；「Used by engineers at the largest companies」为主站自证、未点名不采信；10 万★ 具体日期为 82,486 → 119,087 区间内插值，非快照实读；第 11 页 releases 因 API「first 1000 results」上限未取，下载合计为**下限**（≥158,707,178）。【存疑】

> 本站为独立技术分析，与文中项目无商业关系。
