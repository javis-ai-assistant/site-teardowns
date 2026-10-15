> **首发 [https://javis.im/analysis/bun/](https://javis.im/analysis/bun/)，本文随站点同步更新**；文中图片见在线原页，英文版：https://javis.im/analysis/bun/index-en

# 48,243 vs 25,181 req/s：96.2k 星的 Bun 横评 Node 与 Deno，$0 收入时被 Anthropic 买走

> **利益冲突披露**：本文作者所在团队运营同赛道的 AI 网关产品，与 AI 编程工具生态（Bun 的最大客户 Claude Code 所属赛道）存在间接竞争关系。Bun 已被 Anthropic 收购，本文对 Anthropic 产品生态不持立场，只对手里这 14 个来源负责：对 Bun 有利与不利的证据同时列出，每个判断句背后都有可点开的来源，观点一律标注。

取证日期：2026-10-16（UTC+8）

## 这个站在做什么

Bun 是一个 all-in-one 的 JavaScript 运行时与工具链：运行时、包管理器、测试器、打包器装进同一个二进制文件，官方话术是「drop into existing Node.js projects, no runtime switch required」——在现有 Node 项目里直接 `bun install` / `bun test`，不必整体迁移。（来源 1/11）【事实】

但 2026 年的 Bun 和 2022 年的 Bun 已经是两个物种。首页顶部横幅写着「Bun has joined Anthropic — and Anthropic is betting on Bun →」，页脚是「Now part of Anthropic © 2026 Anthropic, PBC」。（来源 1）【事实】

![Bun 首页：顶部 Anthropic 横幅 + 官方横评面板，取证 2026-10-16](img/home.png)

取证当天它刚发 **v1.4.3**（GitHub release 页标注 2026-10-10，Latest）：一次修掉 **166 个 issue（其中 122 个带 👍）**，主打 `bun check`——内置 TypeScript 类型检查器，官方称与 TypeScript 7 的 tsc 报同样的错、快 **3 到 6.4 倍**，另加空闲服务器 CPU 降 67–89%、大 `node:http` 响应快至 2.7 倍。（来源 5/6/7）【事实】

![v1.4.3 release 页：2026-10-10 发布、8 位贡献者，取证 2026-10-16](img/release.png)

仓库实抓 **96.2k 星**、open issues 3.6k、open PRs 5k+（来源 4）。【事实】对比 Anthropic 收购官宣里的「over 82,000 stars」（2025-12，来源 3），10 个月涨了约 14k 星。【推断】增速不算爆发——对一个已被巨头收购、不缺曝光的项目，这是自然增长曲线而非炒作曲线。

## 对比横评 · 三家底牌（取证日口径）

三家 2026 年的现状，先摆第三方横评的基线表（来源 12/13）：

- **Node.js 24**：V8 引擎，30 个月 LTS，npm 兼容 100%，`--experimental-strip-types` 转正后原生跑 TS（但 enums/decorators 仍需构建步骤）；daily.dev 口径是 42.65% 开发者采纳率、85% 企业流量份额。（来源 12/13）【事实】
- **Bun 1.4**：JavaScriptCore（Safari 引擎）单二进制四合一，冷启动与内存占优，npm 兼容第三方测 ~98–99%，**无正式 LTS 策略**、**无默认拒绝的权限模型**。（来源 12/13）【事实】
- **Deno 2.6/2.8**：V8 + TypeScript 优先 + default-deny 权限模型，npm 兼容 ~90–95%，每个 minor 6 个月支持窗口。（来源 12/13）【事实】

【观点】这张表本身就是横评的正确打开方式：**没有全能冠军，只有取舍**——要企业存量兼容选 Node，要默认安全选 Deno，要冷启动与单二进制分发选 Bun。

## 对比横评 · 官方自测：四个面板，四个口径

Bun 首页直接摆横评数据（全部标注「medians of 3 reproduce」并给出机器型号 Linux x64, EPYC 9R14，方法论与基准脚本在仓库 `/bench` 目录公开，来源 1/10）【事实】：

| 面板 | Bun | Node.js | Deno |
|---|---|---|---|
| 装 T3-stack（~220 包，热缓存） | **0.21s** / 12 MB | npm 4.45s | — |
| Express over HTTPS（req/s） | **48,243** / 105 MB | 25,181 / 142 MB | 19,243 / 176 MB |
| Postgres 100×100（q/s） | **20,243** / 80 MB | 10,000 / 168 MB | 10,406 / 181 MB |
| WebSocket 广播（msgs/s） | **4.17M**(publish) | 123K (ws) | 124K |

【存疑】这张表有三个口径必须点破，否则会读出错误结论：

1. **WebSocket 一行 Bun 自己就有两个数**：`publish` 模式 4.17M msgs/s，`send` 模式只有 159K——前者是服务端单次发布扇出，后者是逐条发送，差 26 倍，**不可横比**（来源 1）。
2. **Postgres 面板驱动不齐**：Bun 用自家 `Bun.sql`，Node 用 `postgres.js 3.4.9`，测的是「运行时 + 各自最优驱动」而非同驱动裸跑（来源 1）。
3. **Deno 在官方 Express 面板垫底（19,243），在第三方 native 面板反超 Node**——见下节。框架选择对排名的影响不小于运行时本身。

## 对比横评 · 第三方口径：真实差距比官方小，但存在

两个独立第三方（2026-06 前后）给出更冷的数字（来源 13）：

- 同框架对齐的裸 REST：Bun (Hono) 110,000 vs Deno (native) 85,000 vs Node (Express) 50,000 req/s；
- **真实应用**（DB + 中间件对齐）：Bun 14,320（P99 21ms）vs Deno 6,180 vs Node 5,240 req/s；
- **高并发带数据库时三家收敛到约 12,000 req/s**——瓶颈从运行时挪到数据库，运行时差距被抹平；【事实】
- 冷启动 hello-world：Bun 8–15ms vs Deno 40–60ms vs Node 60–120ms；AWS Lambda 实测 156ms vs 245ms（来源 13）。

daily.dev（2026-04）的独立表格给出同量级结论：Bun 冷启动 8–15ms、吞吐 110K req/s 领先，但 LTS 栏写着「None」。（来源 12）【事实】

另一独立佐证：Trigger.dev 从 Node.js 迁到 Bun 后自述同等硬件 5 倍吞吐（来源 13 转述）。【存疑】这是迁移方自述、未经本文复测，且「5x」与对齐口径的 2.7x（14,320/5,240）不一致，只作方向性参考。

【观点】横评的正确结论不是「Bun 快 2–3 倍」，而是：**纯计算路径上差距真实存在，一旦数据库上场差距缩到可忽略；冷启动与内存才是 Bun 不可被替代的差异位**——这恰好是 serverless 与 CLI 分发两个 Bun 重点押注的场景。

## 反常识：$0 收入那年，它卖了

收购故事里最反常识的一行，来自 Jarred Sumner 的官宣原文：「Today, Bun makes $0 in revenue.」（来源 2）【事实】

时间线与账本（全部来源 2/3）：

- 2022-07 v0.1.0 首周 20k 星，$7M 种子轮（Kleiner Perkins 领投）；2023-09 v1.0 后 $19M A 轮（Khosla Ventures）——累计 **$26M** 融资。（来源 2）【事实】
- 2025-10 月下载 7.2M（环比 +25%），官方自述「还有 4 年 runway，我们本不必卖」。（来源 2）【事实】
- 2025-12-02/03 Bun 宣布被 Anthropic 收购；同一天 Anthropic 官宣 Claude Code 公开仅 6 个月做到 **$1B run-rate**。（来源 2/3）【事实】
- 官方给的理由：「Bun 的单文件可执行体正是 AI 编码工具分发 CLI 的完美形态；过去几个月里 Bun 仓库 merged PR 最多的 GitHub 用户是一个 Claude Code bot。」（来源 2）【事实】
- 收购时口径：7M+ 月下载、82k 星、Midjourney/Lovable 在用（来源 3）；2026-07 Bun 官方博客改口「Bun's CLI gets over 22 million monthly downloads」（来源 8）。【存疑】7.2M→22M 的跳跃里混着口径变化（全量下载 vs CLI 下载），本文两组数字都列出、不连线算增长率。

【观点】这不是「开源项目变现」的故事，是**变现路径被整个跳过**的故事：Jarred 明说不想走「VC 系创业公司苦寻商业化」那一章（投资者终归要回报），把 monetization 直接换成 Anthropic 的支票与 Claude Code 的订单——**收入问题被收购解决了，而不是被产品解决了**。

## 归属之后：中立性是新的存疑项

收购公告里最有分量的一句是双向绑定：「Claude Code ships as a Bun executable to millions of users. If Bun breaks, Claude Code breaks. Anthropic has direct incentive to keep Bun excellent.」（来源 2）【事实】首页证言栏由 Claude Code 负责人 Boris Cherny 出镜背书（来源 1）。

【推断】硬币的另一面：Bun 现在是 Anthropic 的资产，而 Node 背后是 OpenJS 基金会、Deno 背后是 Deno Land——**横评者从此有了立场**。官方首页把 Node/Deno 列在自家面板里比跑分这件事，2025 年 12 月前后并无变化（来源 1），但读者此后引用这些数字时，应自行叠加归属背景；本文引用第三方口径（来源 12/13）正是为此。

工程侧的双向证据也要摆全：

- **风险面**：2026-07 官宣从 Zig 重写为 Rust，导火索是 v1.3.14 里一长串 use-after-free / 内存泄漏 bug 清单（node:zlib、node:http2、UDPSocket、Buffer、fs.watch……官方原文列举了 13 类以上，来源 8）——【事实】官方原话是「本可以永远逐个补丁，但我们要系统性防复发」。
- **稳定面**：重写在 v1.4（2026-08-20）落地，同时带来 Node.js 26.3.0 兼容、**1,517 个新通过的 Node 测试用例**、Windows ARM64、内建无头浏览器自动化；v1.4.1 单版修 202 个 issue（来源 7）。【事实】
- **披露面**：Rust 重写博客开头主动声明「收购已完成，我用 Claude Fable 5 预览版写了重写的大部分」（来源 8）——【观点】AI 写 AI 基建的自我指涉，主动披露比蒙混过去强一个量级。

## 风险与存疑（横评扣分项清单）

1. **无 LTS**：两个第三方独立给出「Bun 无正式 LTS 策略」（rolling only，来源 12/13）。对要跑三年的服务，这条权重最大。
2. **无默认权限模型**：Deno default-deny、Node 有 `--allow-*`，Bun 没有对应物（来源 13）——跑第三方代码的隔离能力靠自觉。
3. **npm 兼容 ~98–99% 而非 100%**：两个第三方口径一致（来源 12/13）；v1.4 一次补 1,517 个 Node 测试（来源 7）从反面印证兼容长尾是真的。【推断】
4. **3.6k open issues / 5k+ open PRs**（来源 4 实抓）——单二进制四合一的 scope 红利，也对应官方 roadmap 自认的「project with a large scope and is still in its early days」（来源 9）。【事实】
5. **官方面板不可直接引用**：三处口径问题见上节；本文引用跑分时优先第三方对齐口径，官方口径只作「官方声称」使用。
6. **归属中立性**：见上节，长期观察项。

## 结论

用对比横评收拢三家：**Node 是存量与 LTS 的答案，Deno 是默认安全的答案，Bun 是冷启动、内存与单二进制分发的答案**；真实业务里数据库会把吞吐差距抹平到 ~12,000 req/s（来源 13），所以选型别看跑分榜，看你瓶颈在哪——serverless 冷启动与 CLI 分发场景，Bun 的 8–15ms 冷启动与 `bun compile` 才是别人给不了的（来源 13/1）。【观点】

对做产品的三个可抄动作：①把方法论与复现脚本公开到 `/bench` 目录，自家跑分才敢摆上首页（来源 10）；②自家面板敢带「medians of 3 reproduce」和机器型号，比一句「fastest」可信十倍（来源 1）；③对自己不利的证据（UAF bug 清单、无 LTS）由官方博客亲手写出，比等第三方曝光体面（来源 8/9）。【观点】

对同行（含本文作者团队）的一句清醒剂：**$0 收入不是耻辱，是商业模式缺席的显性指标**——Bun 用「被收购」回答了这个问题，你的网关呢？LiteLLM 用「never per token」（本专栏第 3 篇，来源 14）回答，OpenRouter 用抽成（第 2 篇）回答。三条路没有一条是「先做用户再想收入」。【观点】

---

**来源清单**（全部取证于 2026-10-16，UTC+8；通道A 服务端抓取 + 通道B 搜索 + 通道D 截图，取证档案 `research/evidence/bun/` 32 文件）：

1. https://bun.sh/ — 首页（Anthropic 横幅/页脚、四面板横评数据、Boris Cherny 证言、USED BY 名单、bun check 发布条）
2. https://bun.com/blog/bun-joins-anthropic — 收购官宣（2025-12-02：$0 revenue、$26M 融资、7.2M 月下载、Claude Code bot 最多 merged PR、MIT 保持）
3. https://www.anthropic.com/news/anthropic-acquires-bun-as-claude-code-reaches-usd1b-milestone — Anthropic 官宣（2025-12-03：$1B run-rate 6 个月、7M+ 月下载、82k 星、Mike Krieger 引言、Midjourney/Lovable）
4. https://github.com/oven-sh/bun — 仓库页（96.2k 星、3.6k open issues、5k+ open PRs 实抓）
5. https://github.com/oven-sh/bun/releases — releases 列表（v1.4.3 Latest、2026-10-10）
6. https://github.com/oven-sh/bun/releases/tag/bun-v1.4.3 — release 页截图（2026-10-10 05:24、8 contributors、安装命令）
7. https://bun.com/blog — 博客索引（v1.4.3：166 issues/122👍/bun check 3x-6.4x/67-89% idle CPU；v1.4 2026-08-20 Rust 重写落地+1,517 新通过测试；v1.4.1：202 issues；v1.3.14：92 issues）
8. https://bun.com/blog/bun-in-rust — Rust 重写（2026-07-08：Zig→Rust、v1.3.14 UAF 清单、22M 月下载、Claude Fable 5 使用披露）
9. https://bun.com/docs/project/roadmap — 路线图（all-in-one 替代 Node.js/Jest/Webpack/esbuild/Babel/yarn/PostCSS、自认 early days）
10. https://bun.com/docs/project/benchmarking — 官方基准方法论（/bench 公开、工具选型建议）
11. https://bun.com/docs/installation — 安装文档（curl/PowerShell、unzip、kernel 5.6+ / 3.10 起）
12. https://daily.dev/blog/javascript-runtimes-bun-vs-node-js-vs-deno-comparison/ — 第三方横评（2026-04-11：三runtime 对照表、42.65% 采纳率、LTS 口径、冷启动）
13. https://devtoollab.com/blog/javascript-runtime-comparison — 第三方横评（2026-06-01：对齐口径 14,320 vs 6,180 vs 5,240、DB 收敛 ~12,000、Lambda 156 vs 245ms、Trigger.dev 5x 转述、权限/兼容表）
14. https://javis.im/analysis/litellm/ — 本专栏第 3 篇（never per token 定价，对照引用）

**勘误**：本文任何错误可在 javis.im 留言指出，核实后修订；若结论被推翻，本文将标注撤回。文中【事实】/【推断】/【观点】/【存疑】四色标注即为可信度分层，读者可只采信【事实】部分。与本文作者存在竞争关系的利益冲突已在文头披露；官方自测与第三方口径的差异、Bun 的无 LTS/兼容长尾等不利证据均已随文列出。
