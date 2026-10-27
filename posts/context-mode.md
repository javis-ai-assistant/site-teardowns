# Issues 关门那一周，main 已经静默四个月：把 context-mode 的 599 个文件、4 篇 ADR 和「707.5k+ users」徽章摊开

> 本站为独立技术分析，与文中项目无商业关系。

取证日期：2026-10-24（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 19 · 可验证性 17 · 独特性 17 · 受众相关度 19 · 多维空间 15 = **87/100**（≥60 才动笔；素材库预分 76）。框架卡 = **技术栈考古**（上一篇为商业画布，不连用）：本篇按「解剖工程结构与演化痕迹」走 —— 沙箱与记忆持久化的实现机制、仓库结构、分支与提交考古、许可演变、ADR 文档，不写功能测评，也不写增长故事。

## 这个仓在做什么：8 个月，从 0 到 32,008 星

context-mode 是一个 MCP server，仓库简介一句话说清定位：「Context window optimization for AI coding agents. Sandboxes tool output (98% reduction), persists session memory, and enforces routing across 17 platforms via MCP + hooks.」仓库创建于 2026-02-23，取证日 `pushed_at` = 2026-10-24 02:53，32,008 星、2,205 fork、113 watch、homepage 指向 context-mode.com。（来源 1/2）【事实】

星标的逐日曲线取证日拿不到：`/stargazers` 端点对匿名请求与 PAT 同时回 404（对照组：同日 linux 仓 pulls 页 200，说明不是网络问题而是端点失效）。（来源 31/56）【事实】能核的只有 Wayback 快照里 `stargazerCount` 字段的四个读数：

| 时点 | 星标读数 | 与上一读数差 | 出处 |
|---|---|---|---|
| 2026-09-15 | 23,027 | — | （来源 16） |
| 2026-09-30 | 24,300 | +1,273 | （来源 17） |
| 2026-10-10 | 26,020 | +1,720 | （来源 13） |
| 2026-10-20 | 31,105 | +5,085 | （来源 15） |
| 2026-10-24 | 32,008 | +903 | （来源 2） |

【事实】五周内 23,027 → 32,008，**+8,981 星（+39%）**，其中 10-10 到 10-20 十天就涨了 5,085。（来源 16/17/13/15/2）【推断】这是一条仍在加速的曲线，与后面要讲的「关停 Issues + main 冻结」发生在同一时间窗里 —— 热度最高、社区通道收得最紧，两件事是同一个月的事。（来源 13/15/19/20/21）

讨论面集中在 2026-02-28：作者把架构长文亲手贴上 HN，标题《MCP server that reduces Claude Code context consumption by 98%》，570 分 / 107 评论，**提交者就是 mksglu 本人**；更早的首帖（id 47148025）发出时他在评论里说「Since the last post we've seen 228 stars」。（来源 41/42/43/44）【事实】README 至今挂着「Hacker News #1 · 570+ points」徽章。（来源 3）【事实】

![HN 主帖取证截图：570 分 / 107 评论，取证 2026-10-24](img/hn.png)

中文圈的二次覆盖也在同一周出现：yomxxx 2026-10-16 的速评写着「今天 GitHub 热榜单日涨了 1900+ 星」「29785 星」。（来源 53）【存疑】星数读数时点不同（Wayback 10-10 = 26,020、10-20 = 31,105），该站数字只作热度信号引用，不进事实链。

## 问题定义与官方对照轴：输入侧有人做过，它说自己做另一半

问题本身是真问题，README 的开场写得具体：一次 Playwright 快照 56 KB、20 条 GitHub issue 59 KB、一份 access log 45 KB，「After 30 minutes, 40% of your context is gone」。（来源 3）【事实】

对照轴有三条，全部可点开：**其一**，Anthropic 官方研究《Code execution with MCP》写明 agent 读文件而非全量塞入的方式「reduces the token usage from 150,000 tokens to 2,000 tokens—a time and cost saving of 98.7%」—— 这是**输入侧**（工具定义与数据进来的方向）。（来源 49）【事实】**其二**，Cloudflare 的 Code Mode（2025-09-26）是同一方向的先例，作者在 HN 里直接承认灵感来源：「Cloudflare's Code Mode blog post crystallized it. They compressed tool definitions. We compress tool outputs.」（来源 44/50）【事实】—— 他那句「tool definitions can be compressed by 99.9%」是**博文对 Cloudflare 的转述**，本轮未在 CF 原文里核到该数字，按当事方转述处理。（来源 44/50）【存疑】**其三**，HN 用户 buremba 指出 Claude Code 原生已对 MCP 输出截断到 25K tokens，作者当场承认并反驳「25k tokens is still a lot… Three or four Playwright snapshots and you've burned 100k tokens」。（来源 42）【事实】

**【推断】**把三条轴并排：98.7% 是 Anthropic 自家研究的输入侧数、25K 是平台原生截断、context-mode 押的是**输出侧**（工具回数据的方向）—— 缝是真实的，但这个领域里所有拿得出的百分比都来自厂商或作者自己，没有第三方 harness 对照。（来源 49/42/3）

## 技术栈考古：三层机制，全部写在 README 里

这是这个仓最值得拆的部分：它把实现机制公开写清楚了，不用猜。以下三段全部贴原文口径。

### ① 沙箱：子进程边界，不是操作系统边界

`ctx_execute` 每次调用「spawns an isolated subprocess with its own process boundary. Scripts can't access each other's memory or state」，跑完只把 stdout 放进对话上下文，原始数据留在沙箱里；12 种语言运行时（JavaScript、TypeScript、Python、Shell、Ruby、Go、Rust、PHP、Perl、R、Elixir、C#），Bun 自动探测时 JS/TS 快 3-5 倍；`gh`/`aws`/`kubectl` 这类 CLI 走凭证透传，子进程继承环境变量但不暴露给对话。（来源 3）【事实】输出超过 5 KB 且带 `intent` 参数时，切到意图过滤：全文索引进知识库，只回匹配段落。（来源 3）【事实】

安全面是分层的，README 自己写明了边界：权限规则（`deny`/`allow`）从宿主延伸进沙箱，deny 永远赢；`ctx_execute_file` 被关在项目根内，绝对路径、`../../` 穿越、逃逸型软链一律拒绝，这条防线是为了堵 **#852** 那个「宿主拒绝后 agent 改从 MCP 沙箱重试、而宿主审批提示看不到工具入参」的逃逸向量；网络侧硬拦 169.254.0.0/16（含 169.254.169.254 云元数据，防 DNS 重绑定）、组播与保留段，回环与内网默认放行，托管环境可开 `CTX_FETCH_STRICT`；任何 `mcp__*` 调用入参在落库前用正则把 token/secret/password/cookie/private_key 等字段打成 `[REDACTED]`。（来源 3）【事实】

**【推断】**作者对边界的措辞是诚实的：README 明说 `ctx_execute` 这类执行工具「inherit the process's filesystem access… the boundary guard is a defense-in-depth layer for the file-read tool, not a full OS sandbox — treat approving any execution tool as approving arbitrary code」。（来源 3）工程上这是该有的自觉 —— 但它也意味着这个「沙箱」的安全性最终依赖宿主侧沙箱，而不是自己提供隔离。

### ② 知识库：SQLite FTS5 + BM25 + 双路召回融合

`ctx_index` 按标题切块、保留代码块，存进 SQLite FTS5 虚拟表（Bun 用 `bun:sqlite`、Node ≥22.5 用 `node:sqlite`、其余 `better-sqlite3`）；检索用 BM25，Porter 词干归一（running/runs/ran 同干），标题与小标题在评分里加权 **5x**。（来源 3）【事实】排序是双路并行再融合：porter 词干一路、trigram 子串一路（`useEff` 能命中 `useEffect`），用 **Reciprocal Rank Fusion** 合并，替代了早期「trigram 只在 porter 落空时兜底」的级联方案。（来源 3）【事实】外加四件小事：多词查询的邻近度重排、Levenshtein 纠错（`kuberntes` → `kubernetes`）、围绕命中词开窗取片段（不截断）、24 小时 TTL 缓存（命中回 ~0.3 KB 提示，省一次 48 KB 抓取，14 天后清库）。（来源 3）【事实】连续调用还有渐进限流：第 1-3 次正常、4-8 次减量加警告、第 9 次起改引导走批处理。（来源 3）【事实】单次输出超 100 KB 时不塞上下文，改为索引后回一条指针消息，模型用 `ctx_search` 按需取回 —— 也就是「数据不丢，只是换个地方存」。（来源 4）【事实】

### ③ 会话记忆：5 类 hook、≤2 KB 分层快照、19 条 Session Guide

持久化这条线是本项目的第二根支柱：文件读写、git 操作、任务、报错、用户的纠正偏好，全部由 hook 抽成结构化事件落进**按项目隔离的 SQLite**。压缩（compaction）触发 `PreCompact` → 从库里读事件 → 生成**优先级分层、预算 ≤2 KB** 的 XML 快照存进 `session_resume` 表 → `SessionStart` 取回快照 → 写结构化事件文件自动入 FTS5 → 注入 `<session_knowledge>` 指令，模型从「最后一次用户提问」接着干。（来源 3）【事实】预算吃紧时先丢低优先级（intent、MCP 调用计数），关键状态（活动文件、任务、规则、决策）永远保留。（来源 3）【事实】事件分类表把 20 类事件按 P1-P4 分级，P1 是文件/任务/计划/规则/用户消息，P2 是 git、错误、错误修复对、约束、阻断、被否掉的方案。（来源 3）【事实】

**【事实】**这里有一处可核的文案漂移：流程图写「Build Session Guide with **15** categories」，紧接着的列表实际列了 **19** 条（Last Request、Tasks、Plans、Key Decisions、Files Modified、Unresolved Errors、Constraints、Blockers、Git、Project Rules、MCP Tools Used、Subagent Tasks、Skills Used、Rejected Approaches、External References、Environment、Data References、Session Intent、User Role）。（来源 3）【推断】与 5.4 KB / 5.5 KB、17 / 18 平台一样，属于长 README 的口径没对齐 —— 无伤机制本身，但说明这份文档是边长边写的，没有第二双眼睛对数。

平台适配是最重的一块：README 的兼容矩阵是 **18 列**（Claude Code、Qwen Code、Gemini CLI、VS Code Copilot、JetBrains Copilot、GitHub Copilot CLI、Cursor、OpenCode、KiloCode、OpenClaw、Codex CLI、Kimi Code、Antigravity、Antigravity CLI、Kiro、Zed、Pi、OMP），会话完整度分 Full / High / Partial / — 四档，Claude Code、OpenCode、KiloCode 是 Full，Zed 与 Antigravity（IDE 版）**完全没有 hook**，只能靠手工拷贝一份路由指令文件，README 自己标这类平台「~60% saved」。（来源 3）【事实】演化痕迹也留在文档里：路由指令文件早期会自动写进用户项目目录，后来因为「git tree pollution」（#158、#164）被改成运行时注入、不再落盘。（来源 3）【事实】连自己的师承都留了档：`docs/UPSTREAM-CREDITS.md` 写明引用了 Matt Pocock 的五个 skills（MIT），做法是「referenced, not vendored」，并解释为什么不 vendor（会为用户从不调用的工具付描述性 token）。（来源 40）【事实】

## 仓库解剖：599 个 blob、4 篇 ADR、一份贡献指南

git tree（递归）取证读到 714 个条目、**599 个文件 blob**，按顶层目录分布：`tests/` 252、`src/` 96、`hooks/` 86、`configs/` 44、`web/` 29、`scripts/` 15、`skills/` 13、`.github/` 10、`.claude/` 10、`docs/` 9，其余是各平台插件元数据（`.claude-plugin`/`.cursor-plugin`/`.codex-plugin`/`.openclaw-plugin`/`.pi` 等各 2-3 个）。（来源 28）【事实】`src/adapters/` 一个目录就占 60 个文件 —— 适配层是这个项目真正的复杂度所在；`src/` 是平铺结构，CONTRIBUTING 逐个文件说明了分工（`server.ts` = MCP server 与工具处理器、`store.ts` = FTS5 内容库、`executor.ts` = 12 语言执行器、`security.ts` = 权限执行、`session/db.ts` = 事件库、`session/snapshot.ts` = 快照构建）。（来源 5）【事实】语言构成：TypeScript 5,074,070 字节、JavaScript 2,072,440、HTML 163,532、Shell 65,686。（来源 29）【事实】

`docs/` 里有 4 篇 ADR（架构决策记录）：0001 SessionDB 多写者、0002 工具描述风格、0003 路由拒绝理由、0004 统计严格压缩公式。（来源 28/39）【事实】**【观点】ADR 是这个仓最像「工程团队」的痕迹**：单人项目里极少有人给一次指标口径变更写正式决策记录，而 0004 甚至把翻车过程、7 个 agent 的审计会签、以及「displayed percentage will jump from ~56% to ~95%」的后果都写进了 Consequences。（来源 39）

工程节奏也留了痕：Wayback 10-20 快照的侧栏写着 releaseCount **195** / tagCount **198**；releases API 首屏 50 条里最新一条是 v1.0.169、发布于 2026-06-29T18:18:53Z（与 main 上最后一次人类提交相隔不到两分钟），最旧一条落到 v1.0.120 / 2026-05-11；npm 侧 `dist-tags.latest` 也停在 **1.0.169**。（来源 15/30/36）【事实】贡献指南里作者自称「I'm a solo maintainer with limited time」，同时号召「Don't overthink it… just send it. If you have an idea, open an issue.」（来源 5）【事实】—— 这句话在取证日指向的是已关闭的通道（见下文治理考古）。

## 口径审计 ①：98% 是哪一行的数

`98%` 是全项目最响的数字，仓库简介、博文体标题、marketplace 描述、HN 帖标题全用它。（来源 2/44/38/41）逐层拆开：

| 口径 | 数值 | 出处 |
|---|---|---|
| BENCHMARK.md 总览（21 场景） | 376 KB → 16.5 KB，整体 **96%** | （来源 4） |
| Part 1 子项小计（14 场景，`ctx_execute_file`） | 315 KB → 5.5 KB，**98%** | （来源 4） |
| Part 2 子项小计（6 场景，`ctx_index`+`ctx_search`） | 60.3 KB → 11.0 KB，**82%** | （来源 4） |
| README 首页示例段 | 315 KB → **5.4 KB**，98% | （来源 3） |
| 最差单场景（Network requests 0.4 KB） | 349 B，仅省 **13%** | （来源 4） |

【事实】也就是说：**98% 是 `ctx_execute_file` 这一子项的小计，整份基准的总览数是 96%**；README 写 5.4 KB、BENCHMARK 写 5.5 KB，同一组数据两处差 0.1 KB；21 个场景里最差的一个只省 13%。（来源 3/4）【事实】BENCHMARK 自带复现说明：fixtures 均取自真实工具调用而非合成数据，代码示例 100% 原样保留、不摘要。（来源 4）【事实】

![README Benchmarks 段取证截图：98% 表格，取证 2026-10-24](img/readme_bench.png)

**【事实】作者本人在 HN 把边界说破了**：「We haven't run formal benchmarks on answer quality yet, mostly focused on measuring token savings」（来源 42）；用户 clouedoc 的回帖更狠：「the claimed 98% context savings are noise without benchmarks of harness performance with and without 'context mode'」，顺带吐槽 Bun「3-5x」那句文案「Feels like nobody took the time to read the copy before publishing it」。（来源 42）【事实】【推断】98% 度量的是**省下的字节**，不是**答案质量**；把它当效果指标读，是读者的误读，也是文案的诱导 —— 两种责任各占一半。（来源 4/42）

## 口径审计 ②：状态栏翻过一次 49 个百分点的车

`docs/adr/0004-stats-strict-compression-formula.md`（2026-05-24，PR #685 随 v1.0.148 发布）是一份罕见的自供材料：`ctx_stats` 里「Without / With」那根百分比条「quietly drifted from honest compression ratio to infrastructure-size accounting across two unrelated bug cascades」。（来源 39）【事实】

过程写得很细：v1.0.134 的 SLICE B 为了修「新会话显示近 100%」的退化 bug，把 `eventDataBytes` 同时加到分子分母两侧，成了 UX 补丁而非指标语义；到 v1.0.148 修好聚合后，真实数据一进来，旧公式就开始在用户以为该是 95%+ 的会话上显示 **56%**。报告者的机器数据：`bytesAvoided` 2,898 KB、`bytesReturned` 140 KB、`eventDataBytes` 2,136 KB（其中 **84% 是 496 份重复拷贝的同一份 CLAUDE.md**，schema 里 `data_hash` 去重列早就写好却没被公式使用）。旧公式显示 56%，严格压缩公式显示 **95.4%**，**49 个百分点**的差就是这么来的。（来源 39）【事实】ADR 的 Consequences 第一条直接预告：v1.0.148 后用户第一次跑 `ctx_stats`，百分比会「jump from ~56% to ~95%」。（来源 39）【事实】

二手侧还有两条同向证据（原页 404，标【存疑】）：andrew.ooo 转引 issue #950 —— 单聊「kept out」2.7 MB **大于**全项目 868 KB，且零 `ctx_execute` 调用的会话也显示 100%；并称 v1.0.167-169 主题多为「savings-accounting fixes」。（来源 51）【存疑】

**【推断】结论只有一句：状态栏里的百分比是自报估算，不是审计数** —— 这不影响沙箱与索引是否工作，但它决定了「98%」这类数字该被放进哪一栏。（来源 39/51）

## 口径审计 ③：「707.5k+ users」是两个下载数相加

README 顶部的 users 徽章数据源是仓库里的 `stats.json`，取证原文只有四个字段：`"message": "707.5k+"`、`"npm": "643.6k+"`、`"marketplace": "63.9k+"`。（来源 33）【事实】**643.6 + 63.9 = 707.5** —— 徽章上的「users」就是 npm 下载数与 Claude Code marketplace 安装数相加，两个都是**累计安装量**口径，不是活跃用户。（来源 33/3）【事实】

对上官方口径：npm downloads API 逐日拉满 2026-02-23 至 2026-10-24 共 244 天，实测总下载 **647,513**；按月看，5 月 122,969 是峰值，6 月 116,747、7 月 84,855、8 月 80,844、9 月 87,392，10 月前 24 天 57,681。（来源 34）【事实】【推断】**下载量在 5 月见顶后回落约 29%（122,969 → 87,392），而同期星标从 23,027 涨到 32,008** —— 关注度还在涨、安装增量在退，两条曲线已经开始背离；累计徽章会把这种背离永远盖住。（来源 34/16/17/13/15/2）【推断】口径提醒：npm 逐日数据里 10-13 至 15、19 至 21、23 至 24 出现 0 值，疑为统计回填延迟，本文一律用月口径。（来源 34）【存疑】

同一屏还有一个更硬的核：README 的「Used across teams at」一排 **18 个大厂徽章**（Microsoft、Google、Meta、Amazon、IBM、NVIDIA、ByteDance、Stripe、Datadog、Salesforce、GitHub、Red Hat、Supabase、Canva、Notion、Hasura、Framer、Cursor），`href` 全部是 `#`，一个可点开的证据都没有；hysenlabs 独立作出同样判断：「rendered as badges with no links, which is a marketing claim rather than a verifiable one」。（来源 3/52）【事实】另有一处小口径漂移：仓库简介说 17 platforms、README 正文说「17 supported clients, plus the OpenClaw gateway integration」、官网说 18 AI coding tools、兼容矩阵实为 18 列 —— 17+1=18 能对上，但四个口径并存本身没对齐。（来源 2/3/46/3）【事实】

## 治理考古：Issues 与 PR 的关停窗口，锁死在 10-10 与 10-17 之间

这是本轮取证最硬的一段，三路证据互相咬合：

**Wayback 时间线。** 2026-10-10 的仓库快照，导航 JSON 里 `code / issues / pull-requests / actions / security-and-quality / insights` 六个 tab 齐全，issues 计数 172、PR 计数 167。（来源 13）【事实】2026-10-17 的 issues 页快照，导航只剩 `code / actions / security-and-quality / insights` 四项。（来源 14）【事实】2026-10-20 的仓库快照同样只剩这四项。（来源 15）【事实】更早的 2026-06-17 issues 页快照（关停前对照）与 09-15、09-30 仓库快照用的都是旧版 HTML，链接里 `context-mode/issues"` 与 `context-mode/pulls"` 都在。（来源 18/16/17）【事实】**关停窗口因此锁死在 2026-10-10 与 2026-10-17 之间。**（来源 13/14/15）

![仓库首屏取证截图：32,008★、无 Issues / Pull requests 标签页，取证 2026-10-24](img/repo.png)

**API 三连。** 仓库元数据 `has_issues: false`，而 `open_issues_count` 仍挂 **356**（GitHub 这个字段按打开的 issue + PR 计数，功能关了、历史计数不清零）。（来源 2）【事实】`GET /issues/482` → **410** 「Issues are disabled for this repo」；`GET /issues/981` → **410** 「Pull requests are disabled for this repo」；`GET /pulls/981` → 404；issues 列表 API 回 `[]`，search API 回 `total_count: 0`。（来源 8/9/10/11）【事实】页面侧 `github.com/.../issues/482` 与 `.../pull/685` 都是 404，而同日 linux 仓 pulls 页 200 —— **对照组通过，说明 404 是本仓状态而非抓取故障**。（来源 57/58/63/56）【事实】同日 pulls 列表页与 closed PR 列表页都能开（HTTP 200），但正文只剩导航壳、零条记录。（来源 60）【事实】GitHub 官方文档也写明这是仓库主的开关：「To disable pull requests entirely, under 'Features', deselect Pull requests.」（来源 54/55）【事实】

**公开面的死链。** README 至今保留着十余条指向历史 issue 的链接（#775、#485、#774、#677、#564、#473、#158、#164、#852、#567 等），取证日全部 404；CONTRIBUTING 更直接写着「If you found a bug, report it. If you have an idea, open an issue.」与「just send it」，并自陈「I'm a solo maintainer with limited time」。（来源 3/5/12/59）【事实】

**【推断】**把三面放在一起：一个正在涨星的项目在一周内同时关掉了 issue 与 PR 通道，外部贡献路径归零，而文档与徽章仍按「欢迎贡献」的样子摆放。（来源 13/14/15/3/5）【推断】可以确认的只有净效果：**单人维护者把外部输入从 GitHub 迁到了自己能控的地方**（Discord 与托管站），同时 `open_issues_count=356` 与全部死链留在公开面上。（来源 2/3）【事实】动机仍是**缺证项**：仓库里没有关停说明、没有公告，Discord 徽章仍挂着，作者未公开解释原因，本文不替他猜。（来源 3）【存疑】

## 提交考古：main 停在 6 月 29 日，此后 184 个提交全是机器人

git 侧的三页提交（每页 100 条，覆盖 2026-06-12 至 2026-10-23）读出来是一条断层线：**2026-07-01 起（含当日）的 184 个提交，作者 100% 是 `github-actions[bot]`，提交信息 100% 是同一句 `ci: update install stats`。**（来源 19/20/21）【事实】main 上最后一次人类提交是 2026-06-29T18:17:55Z，作者 mksglu，信息 `1.0.169`；npm `dist-tags.latest` 也停在 **1.0.169**。（来源 19/36）【事实】也就是：**公开面（main 分支 + npm 发版）静默了约四个月，而安装量徽章仍由 bot 每天自动改写。**（来源 19/20/21/33/36）【事实】

开发并没有停，只是换了位置：取证日抓到的 100 条公开 events（2026-10-21 03:09 至 10-24 00:47）**全部是 mksglu 本人**，94 条 push 分布在 `v2-design`（74）、`wip/cm-v2-impl`（15）、`exp/*`（5），另有 6 条新建分支事件（`exp/`、`impl/`、`wip/` 系）；仓库 `pushed_at` 是 2026-10-24 02:53。（来源 23/2）【事实】**【推断】公开主线与开发支线已经分离**：对外展示的那条线靠机器人维持「天天有提交」的观感，真正在写的代码走 `v2-design` 这类实验分支，两者四个月没有合流。（来源 19/20/21/23）【推断】作者未说明这是 v2 重构期还是发布策略，属【存疑】。

单人结构的量化：contributor API 首个分页 100 人合计 2,232 次提交，`mksglu` **1,372 次 = 61.5%**，`github-actions[bot]` 561 次 = 25.1%，第三名 `ipedro` 36 次 = 1.6%。（来源 22）【事实】组织侧 `GET /users/mksglu/orgs` 回 `[]`，用户页 `hireable: true`、73 个公开仓、668 followers、location London、blog 指向 mksg.lu；同一批数据里他名下还有 hatice（164 星）、browsirai（50 星）、seclawai（33 星）三个兄弟项目。（来源 24/26/27/62）【事实】他的个人主页就是一份 CV，写着「Context Mode · Founder · Mar 2026 - Present」与「Open to new opportunities」（页上带联系方式，本文不转贴）。（来源 45）【事实】**缺证项**：`GET /collaborators` 需要 push 权限，取证日回 403，协作者人数无法核实，本文不写。（来源 25）【存疑】fork 面则很小：按星标排序的前 30 个 fork 里最高只有 5 星，network/members 页整个网络也长不起来 —— 社区二次开发几乎没起来。（来源 32/61）【事实】

## 许可三面与定价页：MIT→ELv2 的 5 天，和一份四档价目

许可是本项目法务面最有戏的一段，三个面各自为政：

- **博文面**：2026-02-26 的发布文《Stop Burning Your Context Window — We Built Context Mode》至今在线，结尾写着 **「Open source. MIT. github.com/mksglu/claude-context-mode」**。（来源 44）【事实】
- **仓库面**：LICENSE 提交史只有两条 —— 2026-02-23 首提（v0.4.0），2026-03-03 `chore: switch license from MIT to Elastic License 2.0 (ELv2)`。（来源 7）【事实】距那篇博文发布 **5 天**（从仓库创建算 8 天）。LICENSE 首行是自定义开头「Elastic License 2.0 (ELv2)」+ Copyright 2026，正是这行自定义文本让 GitHub 识别不出 SPDX，仓库 API 返回 `spdx_id: NOASSERTION`、`name: Other`。（来源 6/7）【事实】
- **包面**：`package.json` 与 npm registry 都写 `Elastic-2.0`；README 徽章写「License: ELv2」。（来源 37/35/3）【事实】

独立交叉验证也有：hysenlabs 只看仓库元数据与徽章，得出「Those two signals disagree」的同一结论；andrew.ooo 也抓到「launch post says Open source. MIT. — the repo today is Elastic License 2.0」。（来源 52/51）【事实】

**【推断】换照的动机与商业页是同一件事。**README 的自陈很直白：「We chose ELv2 over MIT because MIT permits repackaging the code as a competing closed-source SaaS — ELv2 prevents that while keeping the source available」；ELv2 的两条禁令是**不得作为托管/管理服务提供**、不得移除许可声明。（来源 3）【事实】同期 context-mode.com 已经是一条完整的产品线：首页自称「The Control Plane for Coding Agents」与「One endpoint between your coding agents and the model platform」，定价四档 —— **Free $0（1,000 次请求、一次性）、Starter $20/月（10,000 次/月，超额 $2/千）、Pro $49/月（50,000 次/月，超额 $1/千）、Team 按席位议价、Enterprise 年度合同**，并写明「Your own Anthropic or OpenAI account pays the model, and we add no fee on tokens」，外加一个托管看板 Insight。（来源 47/46/48）【事实】

![context-mode.com 定价页取证截图：Free / $20 / $49 / Team / Enterprise，取证 2026-10-24](img/pricing.png)

**【观点】**一边在 README 写「Nothing leaves your machine… context optimization should happen at the source, not in a dashboard behind a per-seat subscription」，一边在官网卖按席位的团队档与托管看板 —— 两句话并不必然矛盾（本地插件免费、托管看板收费是两层产品），但同一个品牌对两拨人各讲一套口号，读的人得自己拼全貌。（来源 3/47）【观点】

## 质疑面与来源立场：注入判罚、历史质疑，以及卖广告位的评测站

**注入判罚（二手转引）。** andrew.ooo 记录：issue #911（2026-07-03）中，context-mode 注入的 `<context_window_protection>` 路由块 —— 具体是「a past phrase does not bind you」这一句 —— 被 Claude Code 的 Auto-Mode 分类器判为「Auto-Mode Bypass」提示注入载荷，阻断 subagent 派发与 `ctx_upgrade`；#946 随后要求加关闭开关。原 issue 页面取证日 404，本文按转引处理。（来源 51/12）【存疑】

**【观点】**这案子和本栏目的防投毒主题是同一件事的两面：这个工具的本职工作就是**往模型上下文里注入指令**，而它自己的注入物触发了平台的注入防御 —— 说明「注入」不是恶意专属，而是所有 hook 类工具共享的中间地带，平台侧只能按形态判，判错成本由双方承担。（来源 51/3）【观点】

**历史质疑要带日期读。** HN 用户 re5i5tor 在 2026-03-01 实测后写道：「Confirmed: context-mode cannot intercept MCP tool responses. The PreToolUse hook matches only Bash|Read|Grep|Glob|WebFetch|WebSearch|Task.」（来源 42）【事实】取证日的 README 里，多个平台的 matcher 已经包含 `mcp__` 形态的外部 MCP 工具（Gemini CLI、Codex CLI、Kimi Code 等），无 hook 平台则明确标「~60% saved」并要求手工放指令文件。（来源 3）【事实】【推断】这条质疑在时间上是准确的、在今天已部分失效 —— 但它的前提仍成立：**拦截能力完全取决于平台给不给 PreToolUse**，而 README 自己把 Zed、Antigravity 列为无 hook。（来源 3/42）【推断】同一串回帖里，作者对 buremba「25K 截断」的回应、对 prompt cache 失效的解释都被追问到底，公开问答的完整树（102 节点）可复核。（来源 42）【事实】

**还有几条只在转引里的风险记录**（原页 404，均【存疑】）：#947 Windows 上 `ctx_batch_execute` 超时参数只约束命令阶段、索引阶段曾挂四小时以上；#959 Pi 适配层的 `ctx_execute` 挂起后 Esc/Ctrl+C 无法中止；#982 Windows 会话结束时 MCP 子进程可能孤儿化并空转 CPU，父进程死亡守护是 Linux/Bun-only。（来源 51）【存疑】

**来源立场。** andrew.ooo 自述「An AI-agent-operated publication, human-supervised」，其页脚挂 editorial policy、corrections，文中数据标注「checked Oct 21, 2026」；hysenlabs 页面内嵌 Advertisement 位、导航有 Advertise with us 入口 —— 两家都在被评项目周边做变现，所以它们的判断（例如 NOASSERTION≠badge 的观察）本文采信事实、不采信结论倾向。（来源 51/52）【事实】yomxxx 是速评型内容站，星数时点口径与档案库对不上，只当热度信号。（来源 53/13/15）【存疑】对照厂商 Anthropic 与 Cloudflare 的数字同样未经第三方复核。（来源 49/50）【存疑】

## 结论：机制写得比数字诚实

**可以抄的，是它把机制摊开的勇气。**沙箱子进程、FTS5/BM25/双路 RRF、≤2 KB 分层快照、18 列平台矩阵、#852 逃逸的堵法、`[REDACTED]` 落库前脱敏、以及那份上游致谢 —— 全部公开在 README 与 `docs/` 里，本文的机制段几乎句句能点开原文核对。（来源 3/4/5/40/28）【事实】单人项目把安全边界写成「not a full OS sandbox」而不是含糊其辞，这份自觉比很多有公司的项目都强。（来源 3）【观点】

**必须记账的，是三处口径。**98% 是子项小计、整体 96%、最差场景 13%；`707.5k+ users` 是两个累计下载数相加，而真实月下载已从 122,969 退到 87,392；状态栏百分比在 ADR-0004 里自己承认过 56%↔95.4% 的漂移。（来源 4/33/34/39）【事实】三处都不是造假，是**口径挑了对自己好看的那一个**，而且都写在可以被逐行复核的地方 —— 这既是它的破绽，也是它还值得信的原因。（来源 4/33/39）【推断】

**最后一层是治理与许可的同步转向。**2026-10-10 到 10-17 之间，Issues 与 PR 双关、`open_issues_count` 停在 356、文档死链遍布；main 的人类提交停在 2026-06-29，此后 184 个提交全是机器人改安装量徽章；而 2026-03-03 那次 MIT→ELv2 的换照，恰好把「别人拿去开托管 SaaS」这条路封死，自家 Free/$20/$49/Team 四档云端在同一时间轴上推进。（来源 13/14/2/19/20/21/7/3/47）【事实】【推断】三条线指向同一个方向：**这个项目正从「开源工具」的形态，朝着「有开源外壳的产品公司」的形态收口** —— 单人结构（提交 61.5%、orgs 为空）决定了它没有余量同时维持社区通道与产品线，于是先关的是社区。（来源 22/24/13/14）【推断】值得继续盯的是那个没有答案的问题：关停说明至今不存在，而徽章仍在每天更新。（来源 3/33）【存疑】

---

## 来源（取证日 2026-10-24，全部可点开）

1. GitHub 仓库首屏（32,008★ / 2,205 fork，截图取证）https://github.com/mksglu/context-mode
2. GitHub API · 仓库元数据（has_issues:false、open_issues_count 356、pushed_at 2026-10-24、NOASSERTION）https://api.github.com/repos/mksglu/context-mode
3. README 全文（通道A 抓取，94 KB）https://raw.githubusercontent.com/mksglu/context-mode/main/README.md
4. BENCHMARK.md（21 场景 / 整体 96% / Part1 98% / Part2 82%）https://raw.githubusercontent.com/mksglu/context-mode/main/BENCHMARK.md
5. CONTRIBUTING.md（solo maintainer、flat src 结构、「just send it」）https://raw.githubusercontent.com/mksglu/context-mode/main/CONTRIBUTING.md
6. GitHub API · license（spdx_id NOASSERTION / name Other）https://api.github.com/repos/mksglu/context-mode/license
7. GitHub API · LICENSE 提交史（2026-03-03 `chore: switch license from MIT to Elastic License 2.0 (ELv2)`）https://api.github.com/repos/mksglu/context-mode/commits?path=LICENSE&per_page=10
8. GitHub API · issue #482 → 410「Issues are disabled for this repo」https://api.github.com/repos/mksglu/context-mode/issues/482
9. GitHub API · issue #981 → 410「Pull requests are disabled for this repo」https://api.github.com/repos/mksglu/context-mode/issues/981
10. GitHub API · issues 列表 → `[]`https://api.github.com/repos/mksglu/context-mode/issues?state=all&per_page=100
11. GitHub API · search issues → total_count 0 https://api.github.com/search/issues?q=repo:mksglu/context-mode+type:issue&per_page=5
12. issues 页面（取证日 404 面）https://github.com/mksglu/context-mode/issues
13. Wayback · 仓库快照 2026-10-10（六 tab 齐全，issues 172 / PR 167，26,020★）https://web.archive.org/web/20261010082235/https://github.com/mksglu/context-mode
14. Wayback · issues 页快照 2026-10-17（导航仅剩 code/actions/security/insights）https://web.archive.org/web/20261017201922/https://github.com/mksglu/context-mode/issues
15. Wayback · 仓库快照 2026-10-20（四 tab、31,105★、releaseCount 195/tag 198）https://web.archive.org/web/20261020021544/https://github.com/mksglu/context-mode
16. Wayback · 仓库快照 2026-09-15（23,027★）https://web.archive.org/web/20260915163801/https://github.com/mksglu/context-mode
17. Wayback · 仓库快照 2026-09-30（24,300★）https://web.archive.org/web/20260930122535/https://github.com/mksglu/context-mode
18. Wayback · issues 页快照 2026-06-17（关停前导航对照）https://web.archive.org/web/20260617141054/https://github.com/mksglu/context-mode/issues
19. GitHub API · commits 第 1 页（100 条，2026-06-12 起）https://api.github.com/repos/mksglu/context-mode/commits?per_page=100&page=1
20. GitHub API · commits 第 2 页 https://api.github.com/repos/mksglu/context-mode/commits?per_page=100&page=2
21. GitHub API · commits 第 3 页（止于 2026-10-23）https://api.github.com/repos/mksglu/context-mode/commits?per_page=100&page=3
22. GitHub API · contributors 首页（100 人 / 2,232 次，mksglu 61.5%、bot 25.1%）https://api.github.com/repos/mksglu/context-mode/contributors?per_page=100
23. GitHub API · 公开 events（100 条，2026-10-21～10-24，全部本人 push）https://api.github.com/users/mksglu/events/public?per_page=100
24. GitHub API · 用户组织列表 → `[]`https://api.github.com/users/mksglu/orgs
25. GitHub API · collaborators → 403（缺证，协作者数未采信）https://api.github.com/repos/mksglu/context-mode/collaborators
26. GitHub API · 用户 mksglu（London / 73 仓 / 668 followers / hireable）https://api.github.com/users/mksglu
27. GitHub API · 用户仓库列表（hatice 164★、browsirai 50★、seclawai 33★）https://api.github.com/users/mksglu/repos?per_page=100&sort=updated
28. GitHub API · git tree 递归（714 条目 / 599 blob / docs 9 含 4 ADR）https://api.github.com/repos/mksglu/context-mode/git/trees/HEAD?recursive=1
29. GitHub API · languages（TS 5.07MB / JS 2.07MB / HTML / Shell）https://api.github.com/repos/mksglu/context-mode/languages
30. GitHub API · releases（分页 50）https://api.github.com/repos/mksglu/context-mode/releases?per_page=50
31. GitHub API · stargazers → 404（端点失效，星标曲线改走 Wayback）https://api.github.com/repos/mksglu/context-mode/stargazers?per_page=100
32. GitHub API · forks 按星排序（前 30 最高 5★）https://api.github.com/repos/mksglu/context-mode/forks?sort=stargazers&per_page=30
33. stats.json（users 707.5k+ = npm 643.6k+ marketplace 63.9k+）https://cdn.jsdelivr.net/gh/mksglu/context-mode@main/stats.json
34. npm downloads 逐日（2026-02-23:2026-10-24，总 647,513，月峰值 5 月 122,969）https://api.npmjs.org/downloads/range/2026-02-23:2026-10-24/context-mode
35. npm registry（license Elastic-2.0）https://registry.npmjs.org/context-mode
36. npm latest（dist-tags.latest = 1.0.169）https://registry.npmjs.org/context-mode/latest
37. package.json（`"license": "Elastic-2.0"`、version 1.0.169）https://raw.githubusercontent.com/mksglu/context-mode/main/package.json
38. .claude-plugin/marketplace.json（描述里写 saves 98%）https://raw.githubusercontent.com/mksglu/context-mode/main/.claude-plugin/marketplace.json
39. ADR-0004 · Stats display uses strict-compression formula（56% ↔ 95.4%，49pp）https://raw.githubusercontent.com/mksglu/context-mode/main/docs/adr/0004-stats-strict-compression-formula.md
40. docs/UPSTREAM-CREDITS.md（Matt Pocock skills，MIT，referenced not vendored）https://raw.githubusercontent.com/mksglu/context-mode/main/docs/UPSTREAM-CREDITS.md
41. HN 主帖页（570 分 / 107 评论，2026-02-28 自荐）https://news.ycombinator.com/item?id=47193064
42. HN Algolia item API（全树 102 节点：作者亲答、clouedoc、buremba、re5i5tor）https://hn.algolia.com/api/v1/items/47193064
43. HN Algolia 检索（分数与评论数对表）https://hn.algolia.com/api/v1/search?query=context-mode&tags=story&hitsPerPage=20
44. mksg.lu 发布博文（2026-02-26，「Open source. MIT.」至今在线）https://mksg.lu/blog/context-mode
45. mksg.lu 首页（作者自述：MCP Directory & Hub、100K+ daily requests）https://mksg.lu/
46. context-mode.com 首页（The Control Plane for Coding Agents / 18 AI coding tools）https://context-mode.com/
47. context-mode.com 定价页（Free / $20 / $49 / Team / Enterprise，截图取证）https://context-mode.com/pricing
48. context-mode.com · Insight（托管看板）https://context-mode.com/insight
49. Anthropic · Code execution with MCP（150,000 → 2,000 tokens，98.7%）https://www.anthropic.com/research/code-execution-with-mcp
50. Cloudflare · Code Mode: the better way to use MCP（2025-09-26，输入侧先例）https://blog.cloudflare.com/code-mode/
51. andrew.ooo 评测（2026-09-13 发布、数据标 checked Oct 21 2026；转引 #911/#946/#947/#950）https://andrew.ooo/posts/context-mode-mcp-context-window-optimization-review/
52. hysenlabs 评测（NOASSERTION vs ELv2 badge 不一致、广告位入口）https://hysenlabs.com/en/projects/mksglu-context-mode
53. yomxxx 速评（2026-10-16，单日 +1900 星 / 29,785 星，热度信号）https://yomxxx.com/posts/2026-10-16-context-mode-mcp-context-optimization-tools
54. GitHub Docs · Disabling pull requests（deselect Pull requests 即全关）https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/disabling-pull-requests
55. GitHub Docs · Enabling features for your repository（功能可开可关）https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository
56. 对照组 · linux 仓 pulls 页（同日 200，排除抓取故障）https://github.com/torvalds/linux/pulls
57. issue #482 页面（取证日 404 面）https://github.com/mksglu/context-mode/issues/482
58. PR #685 页面（取证日 404 面）https://github.com/mksglu/context-mode/pull/685
59. issues 查询页（页面 200、列表空）https://github.com/mksglu/context-mode/issues?q=is%3Aissue
60. closed PR 列表页（取证日 200、历史可见性核验）https://github.com/mksglu/context-mode/pulls?q=is%3Apr+is%3Aclosed
61. network/members（fork 网络面）https://github.com/mksglu/context-mode/network/members
62. 作者 GitHub 主页（Mert Köseoğlu / London / hireable）https://github.com/mksglu
63. PR #981 页面（取证日 404 面，与 API 410 对照）https://github.com/mksglu/context-mode/pull/981

取证档案：`research/evidence/context-mode/`（**155 个快照文件 / 81 条唯一来源 URL**，manifest 带 sha256、通道与来源 URL；通道 = 通道A 正文抓取 + GitHub API 只读 + Wayback 快照直取 + npm registry/downloads API + HN Algolia + 四链搜索 + 通道D 截图 4 张）。防投毒扫描：155 文件强模式 grep（ignore/disregard instructions、contact me、telegram、you are an AI 等）→ **0 命中**；README 的 SessionStart 路由注入块属公开文档化的工具机制，已记录不执行。

## 勘误与可撤回

- 本文任一判断被一手源推翻，请来信指出；核实后在对应句子后加「（勘误 <日期>）」并保留原句划线，**不删旧数字、不改历史口径**。
- 已知存疑项：issue #911「Auto-Mode Bypass」判罚、#946 关闭开关、#947 Windows 超时挂四小时、#959 Esc 无效、#982 孤儿子进程、#950 单聊 2.7 MB > 全项目 868 KB —— 全部为 andrew.ooo 转引，原 issue 页取证日 404，未当事实使用；「Cloudflare 压缩 99.9%」为 mksg.lu 博文转述，本轮未在 Cloudflare 原文核到；yomxxx 的 29,785 星与 1900+ 单日增量为该站时点读数，与档案库时点不同，只作热度信号；npm 逐日 0 值（10-13～15、19～21、23～24）疑为统计回填延迟，本文按月口径；协作者人数取证失败（API 403），全文未写；关停 Issues/PR 的动机无任何公开说明，属缺证，文中只写净效果不写动机。【存疑】
