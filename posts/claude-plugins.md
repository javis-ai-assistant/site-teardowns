# 11 个月 41 → 43,746 星：Anthropic 官方插件仓的 4,630 次提交里，73.8% 是机器人写的

> 11 个月从 41 星涨到 43,746 星的官方插件仓，4,630 次提交里 73.8% 是机器人写的。本文拆五个台阶的曲线、星从哪来、目录里 262 条其实是别人的，还有治理层与 1,045 个 open issue。

取证日期：2026-10-22（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 19 · 可验证性 19 · 独特性 16 · 受众相关度 18 · 多维空间 15 = **87/100**（≥60 才动笔；素材库预分 77）。框架卡 = **增长拆解**（上一篇为对比横评，不连用）。

## 这个仓在做什么

`anthropics/claude-plugins-official` 的自我介绍是一句：「Official, Anthropic-managed directory of high quality Claude Code Plugins」，homepage 字段指向 `code.claude.com/docs/en/plugins`，许可 Apache-2.0，topics 是 `claude-code / mcp / skills`，建仓 `2025-11-20T18:36:20Z`，取证日最近 push `2026-10-22T07:41:13Z`，43,746 星 / 4,607 forks / 1,185 open（含 PR）/ 258 watchers / 主语言 Python。（来源 1/2）【事实】

仓库结构是**两层抽屉**：`git/trees` 递归共 835 条，其中 `plugins/` 709 条、`external_plugins/` 102 条、`.github/` 19 条；顶层 `plugins/` 下正好 **40 个目录 = 40 个官方插件**（`plugin.json` 计数也是 40），里面 **12 个是语言服务插件**（typescript / pyright / rust-analyzer / gopls / jdtls / kotlin / csharp / php / ruby / swift / lua / clangd），`external_plugins/` 另有 14 个（asana / context7 / discord / github / gitlab / linear / playwright / serena / telegram / terraform 等）。文件计数：skills 216、md 240、py 109、hooks 62、ts/js 50、agents 39、commands 33。（来源 5）【事实】

![仓库页取证截图：43,746 星 / 4,607 forks / Python / Apache-2.0，取证 2026-10-22](img/github.png)

真正的分发机制在**预装**：官方文档原话是「Claude Code adds it the first time you start an interactive terminal session, unless a managed policy or `CLAUDE_CODE_DISABLE_OFFICIAL_MARKETPLACE_AUTOINSTALL` blocks it」——**第一次开交互终端就自动挂上这个目录，想关得自己设环境变量或托管策略**。（来源 13）【事实】

README 顶部则挂了一条对冲声明：「Make sure you trust a plugin before installing, updating, or using it. **Anthropic does not control what MCP servers, files, or other software are included in plugins and cannot verify that they will work as intended or that they won't change.**」（来源 3）【事实】——入口是官方塞给你的，内容官方明说不管，这一对张力贯穿全文。

## 增长曲线：41 → 43,746，五个台阶

GitHub 已下线星标明细 API，曲线只能靠 Wayback 存档的仓库页一格一格拼。取证日抓到 9 个快照 + 1 个 API 时点（全部时间戳可查 CDX 索引，来源 64），下表全部可点开：

| 取证时点 | 星数 | forks | issues | PR |
|---|---|---|---|---|
| 2025-12-14（来源 14） | **41** | 11 | — | 4 |
| 2026-01-28（来源 15） | 5.1k | 536 | 90 | 56 |
| 2026-03-23（来源 16） | 14.2k | 1.5k | 296 | 16 |
| 2026-04-25（来源 17） | 17.8k | 2.1k | 542 | 33 |
| 2026-05-23（来源 18） | 25.9k | 2.8k | 681 | 11 |
| 2026-06-29（来源 19） | 31.3k | 3.4k | 761 | 24 |
| 2026-08-19（来源 20） | 33.7k | 3.8k | 900 | 18 |
| 2026-09-24（来源 21） | 36.7k | 4.1k | — | 22 |
| 2026-10-14（来源 22） | 38.1k | 4.3k | — | 199 |
| **2026-10-22 API**（来源 2） | **43,746** | 4,607 | 1,185（含 PR） | — |

（来源 2/14-22）【事实】

两处口径中断按「不可得 ≠ 0」处理：2025-12-14 与 2026-09 之后的快照里 issue 计数在页面上取不到，表里记「—」而不是 0。（来源 14/21/22）【事实】曲线形状因此是**五个台阶**：1 月末破 5k → 4 月中 17.8k → 5 月末 25.9k → 6 月末 31.3k → 10 月中 38.1k，然后是最后 8 天 **+5,646** 的垂直段。（来源 15-22）【事实】

11 个半月从 41 到 43,746，中间没有一次发布节奏可以解释它——**这个仓的 `releases` 与 `tags` 取证日双双为 `[]`，零版本发布，全靠 main 分支滚动**。（来源 23/24）【事实】

## 一周 +5,703：第二个爆点与星从哪来

取证日 GitHub Trending 周榜上，本仓写着「**5,703 stars this week**」、总星 43,746，与上表 10-14 → 10-22 的 +5,646 基本吻合（差 57，属抓取时点差）。（来源 25/2）【事实】同榜还有几个参照物：`morluto/rea` +129,417、`debpalash/VoiceStudio` +15,125（本站第 13 篇的主角）、`NSA/ghidra` +5,769、`K-Dense-AI/scientific-agent-skills` +4,719、`cursor/plugins` +2,049。（来源 25）【事实】

![GitHub Trending 周榜取证截图：本周 +5,703 星，取证 2026-10-22](img/trending.png)

星从哪来，能查到三条腿。**其一是官方社媒**：@ClaudeDevs 在 2026-10-02 推「新插件 You Should Know」，取证时 16,161 赞、1,703,030 次浏览。（来源 26）【事实】**其二是媒体模板稿**：aitoolly 用同一个标题骨架「Anthropic Launches Official Claude Code Plugins Directory on GitHub…」在 08-28、08-29、10-18、10-19 连发四篇，搜索引擎面一次就能捞出四条。（来源 27/28/29/30）【事实】**其三是 Trending 自身的正反馈**：上榜与涨星互为因果。（来源 25）【推断】

**HN 这条腿基本是缺的**：全站检索没有一条以本仓为主角的高分主帖，仓 URL 只出现在别人的评论里；相关主帖 `Discover Claude Code Plugins and Marketplaces`（2025-10-10）只有 1 分 1 评论，且指向的是第三方站；同主题分数最高的是别人家的 skill 仓（202 分）。（来源 31/32）【事实】**讨论主战场不在 HN，在 GitHub issues**（见第 11 节）。（来源 10/37）【推断】

## 谁在贡献：73.8% 是 github-actions[bot]

提交面是这篇最硬的一组数。全仓 **4,630 次提交**（仓库页计数与翻页互证：46 页 × 100 + 末页 30 = 4,630），首提 `2025-11-20T19:47:24Z`，message 是 `creating intital scaffolding for claude code plugins`（scaffolding 拼错的那个字母就留在历史里）。（来源 1/7/33）【事实】

贡献者 42 人，第一名 **github-actions[bot] 3,417 次 = 73.8%**；第二名 jordanecker-ant 360、第三名 bryan-anthropic 242；`claude` 这个账号本身排第 9，**自提 34 次**。（来源 6）【事实】

PR 面同构：全仓 **5,516 个 PR**，其中标题以 `bump(...)` 开头的 **3,721 个 = 67.5%**；取证日拉的最近 100 个 PR 里 **70 个是 github-actions[bot] 的 bump**。（来源 11/12/34）【事实】近 100 次提交落在 2026-10-12 至 10-21，其中 10-16 单日 32 次、10-20 单日 30 次。（来源 8）【事实】

**【推断】这不是社区共建的形状，是自动化流水线加一支内部内容团队的形状**：bot 负责把 262 个外部仓的 ref 版本号往前顶，人负责官方插件与 review，`claude` 负责那 34 次自提。（来源 5/6/12）

对照一条当事人自述会更清楚：Anthropic 引用的一项分析称「Claude Code wrote around **4% of all public commits on GitHub**」。（来源 42）【存疑】——第三方转述的自述口径，无法独立复核，**本文只登记不采信**；但它与 73.8% 这个数放在一起，指向同一件事：**这个生态的「活跃」相当一部分是机器产生的**。（来源 6/42）【推断】

## 目录的真身：315 条里 262 条是别人的

`.claude-plugin/marketplace.json` 取证日当面解析：**315 条插件**，其中 **262 条的 `source` 是外部 GitHub 仓**（`git-subdir` 形式，带 url / path / ref / sha），只有 53 条是仓内路径（`./plugins/` 39 + `./external_plugins/` 14）——**83% 是指针，不是代码**。（来源 4/5）【事实】

按 category 拆：development 123、productivity 69、database 39、monitoring 22、security 18、无 category 14、deployment 9、design 8，其余为个位数长尾；`strict:false` 14 条（skills 直挂）。author 面上出现 **Google LLC 14、SAP SE 9、Amazon Web Services 7、Pendo 5、Grafana 3、Carta Engineering 3、Shopify 3、Oracle 系 6**。（来源 4）【事实】

官方文档自己把这件事说破了：「By default, Claude Code adds the official marketplace, `claude-plugins-official`, for you. **Most of what it lists comes from partners and other authors rather than from Anthropic**: tool vendors publish plugins that connect Claude Code to their services, and Anthropic maintains a smaller set of its own, such as commit-commands, code-review, feature-dev, and the language server plugins.」（来源 13）【事实】

![官方文档三档 marketplace 对照表取证截图，取证 2026-10-22](img/docs_marketplaces.png)

**【推断】它的壁垒不是代码量，是策展权加默认预装位**：40 个自家插件撑不起 43,746 星，但「你一开终端它就在」这件事，加上「由 Anthropic 挑过」的目录身份，决定了大部分用户第一屏看到的就是这份清单。（来源 5/13/2）【推断】

## 治理层：付费墙提交门、verified 与三档 marketplace

**提交是有门的。** 官方文章《Build plugins for Claude with the directory submission portal》日期 2026-09-25：「The directory submission portal is **open to developers on paid Claude plans**」，提供自动校验与安全扫描（「Each submission is checked and safety-scanned as soon as you submit」）、审核状态与反馈、上线后的用量分析。（来源 63）【事实】README 里给的投稿入口是 `clau.de/plugin-directory-submission` 链接。（来源 3）【事实】

**目录是三档的。** 官方对照表：official = `anthropics/claude-plugins-official`（首启自动添加，可用托管策略或环境变量关掉）；community = `anthropics/claude-plugins-community`（4,722 星，建于 2026-03-20，须手动 add，其自述是「Read-only mirror — submit plugins at clau.de/plugin-directory-submission」）；demo = `anthropics/claude-code` 里名为 `claude-code-plugins` 的那个（文档原话：「It isn't the official marketplace.」）。（来源 13/35/36）【事实】同一篇还写着「**Anthropic doesn't review third-party marketplaces**, so read Plugin security and trust before you add one.」（来源 13）【事实】

**名字是不可变的。** README 专门有一节「Plugin names are immutable」：`name` 是 immutable slug，改名会让已装用户报 `plugin-not-found`，真要改就得往顶层 `renames` 表里加一条让加载器自动迁移。（来源 3）【事实】取证日 `marketplace.json` 顶层 `renames` **已积累 10 条**：adlc→agentforce-adlc、airwallex→airwallex-agentos、convex-backend→convex、vals→valtown、wordpress.com→build-with-wordpress、qodo-skills→qodo、atlassian-forge-skills→forge-skills、azure-skills→azure、sonarqube-agent-plugins→sonarqube、data-agent-kit-starter-pack→data-agent-kit。（来源 4）【事实】这不是失败，是**把兼容性成本写成了一张表**；但 10 条记录本身说明改名压力有多大。（来源 4）【推断】

一条花边：HN 评论里有人指出某个插件「Looks like it's been **renamed to ralph-loop for legal reasons**」（2026-02-23），仓里确实躺着 `plugins/ralph-loop`，issues 里也留着 `Ralph-wiggum` 时代的权限报错。（来源 38/10）【事实】

web 侧还有一层「verified」：Claude Marketplace 网页目录会显示安装量并对部分插件打 **Anthropic verified** 标记（来源 13/39）【事实】，官方支持中心另有一篇 skill 与 plugin 的扫描说明（来源 62）【事实】。取证日该目录页由 JS 渲染，**安装量与 verified 数字抓不到原始值**，本文不引。（来源 39）【存疑】

## 口径大战：315 / 341 / 2,000+，以及「launched May 22」

同一件事有三套数：

- **315**：我 10-22 当面解析 `marketplace.json` 的条目数。（来源 4）【事实】
- **341**：TrendinTech（10-18 发，正文写「as of October 17, 2026」）称「Anthropic's official plugin marketplace for Claude Code listed **341 plugins** as of October 17, 2026」，并称数字来自「the company's own marketplace page」。（来源 40）【存疑】
- **2,000+**：Anthropic 官方《Claude Marketplace》通稿：「Add connectors and plugins. **Choose from more than 2,000 available today**, including Atlassian, Google, Microsoft, Notion, Salesforce, and more.」（来源 41）【事实】

三者**不必然矛盾但不可互换**：官方那个 2,000+ 挂在 claude.ai 侧的 Claude Marketplace 上，是 connectors 与 plugins 的合集，跟本仓的 315 条不是一张表。（来源 41/4）【推断】**同一件事在同一个月里有三个对外数字，采信前必须先问「哪张表」。**（来源 4/40/41）【观点】

时间线错位是同一类问题：**仓库 2025-11-20 建、2025-12-14 快照只有 41 星**，但 TechTimes（2026-05-25）写「The repository, `anthropics/claude-plugins-official`, **launched May 22**」，developersdigest（05-22）的标题是「Claude Code's Official Plugin Marketplace Is Here - and It's **Already at 23k**」，claudecamp（05-26）写「Anthropic launched the official Claude Code plugin directory on **May 22, 2026** - Four days later… crossed 20,000 stars」。（来源 43/44/45）【事实】

**【推断】5-22 大概率是插件机制 GA / 媒体集中报道日，不是仓库诞生日**——把爆红日当诞生日，时间线就整体前推了半年，而建仓日期是 API 里可查的硬数。（来源 2/43/44/45）【推断】

## 钱线：仓免费，母厂 $965B

**这个仓本身零收入**：没有 pricing 页、没有公司主体、`releases` 与 `tags` 双 `[]`、Apache-2.0 全文可商用。（来源 2/23/24/46）【事实】价值全在母厂那侧，官方通稿是硬出处：

- 2026-05-28《Series H》：「Anthropic has raised **$65 billion** in Series H funding led by Altimeter, Dragoneer, Greenoaks and Sequoia, valuing the company at **$965 billion post-money**」，并自述「our **run-rate revenue crossed $47 billion** earlier this month」。（来源 47）【事实】

![Series H 官方通稿取证截图：$65B / $965B post-money，取证 2026-10-22](img/series_h.png)

- Sacra（第三方估算）：「Anthropic hit **$65B in annualized revenue in July 2026**, up from $47B in May 2026」，以及「filed **confidentially for an IPO on June 1, 2026**, at a $965 billion valuation」。（来源 48）【存疑】
- ExplodingTopics（转述 Anthropic 自述）：「Claude Code's run-rate revenue reached **$2.5 billion by February 2026**, according to Anthropic. That's up from **$500 million in September 2025**」；同页还称 2026-02 起企业客户占 Claude Code 收入过半。（来源 42）【存疑】

**【推断】这解释了免费分发位为什么值钱**：一个 run-rate $2.5B 的付费产品，它的安装入口、目录排序与 verified 标记本身就是销售漏斗的第一格，所以它能一边零收费、一边把提交门设在付费计划后面。（来源 42/63/13）【推断】

## 分发位之争：cursor/plugins 与 Environments API

同一周的 Trending 榜上，`cursor/plugins` **13,504 星、本周 +2,049**，描述是「Cursor plugin specification and official plugins」——对手方也在建自己的官方插件位。（来源 25/49）【事实】TrendinTech 把两件事连起来报道：「The milestone lands a day after Cursor… **opened its Environments API to public beta on October 16, 2026**」。（来源 40）【存疑】——单一媒体口径，我未取到 Cursor 官方公告原文。

OpenAI 侧也有动作面：Codex marketplace 的第三方分发指南（2026-04-11、04-24 两篇）已在流通；`developers.openai.com/plugins` 取证日返回 403，**正文未取到，本文不用其内容**。（来源 50/51）【存疑】

用户侧的混乱是竞争的副产品：HN 评论（2026-03-02）原话「**Had trouble with their two marketplace's as there's also another `anthropics-claude-code`**」，指的正是官方仓与 demo 仓的重名问题；issue #587「cannot add `anthropics/claude-plugins-official`」挂了 16 条评论；而很多人以为存在的 `anthropics/claude-plugins` 取证日 **404**。（来源 32/52/53）【事实】

**【观点】分发位之争的胜负手不是插件数，是谁被默认装上**——三档 marketplace 里只有 official 是自动添加的，这一条比 315 与 341 的差值重要得多。（来源 13/4/40）【观点】

## 风险面：PromptArmor 绕过确认与官方免责

安全研究这条线要注意日期：PromptArmor《Hijacking Claude Code via Injected Marketplace Plugins》发表于 **2025-10-16**，**比官方仓建仓（2025-11-20）还早一个月**。（来源 54/2）【事实】文中演示两件事：**其一**，用两种基于 hook 的机制**绕过 Claude 的 human-in-the-loop 审批**——原话「since the attacker has bypassed the human-in-the-loop defense using one of the malicious hook methods from step 2, the [redacted] command is executed immediately without requesting approval」；**其二**，在指令里嵌入间接提示注入，让 Claude 把代码库数据外发。（来源 54）【事实】（本文不复述其可执行步骤，命令原文一律隐去。）

![PromptArmor 攻击研究取证截图，取证 2026-10-22](img/promptarmor.png)

**来源立场要标**：PromptArmor 是安全厂商，发布攻击研究有产品获客动机，且该研究早于官方仓一个月——它是对着**社区市场**打的，不是对着本文这个仓打的。（来源 54/2）【事实】

官方侧的两处对冲：README 顶部「**Anthropic does not control** what MCP servers, files, or other software are included in plugins and cannot verify that they will work as intended or that they won't change」（来源 3）【事实】；《Plugin security and trust》文档则把风险拆开写明——「A plugin can carry content that **runs code on your machine with your user privileges** and content that enters Claude's context as instructions」，并且「when auto-update is on… **the files you reviewed can change on disk**」。（来源 61）【事实】

**【推断】「自动预装 + 内容不管」这组搭配，把信任决策从官方挪到了用户身上**：入口是默认给的，验证是每个插件自己做的，而 auto-update 还会让已审过的内容在磁盘上被换掉。（来源 13/3/61）【推断】——这正是官方要在 README 第一屏写警告的原因（来源 3）【观点】。

## 软文之外：1,045 个 open issue

媒体稿里看不到的声音在 issues 里。取证日计数：issue 总数 **1,277**、open **1,045**；PR 总数 5,516、open 140；**1,045 + 140 = 1,185 = 仓库 API 的 `open_issues_count`**，两个口径严丝合缝。（来源 9/10/11/2）【事实】

评论最多的三条都是同一类问题——**「提交门户显示 Published，目录里却没有」**：

- **#1272**（2026-04-07，**36 条评论**）「Plugin marked as "Published" on plugins/submissions but not available marketplace directory」；（来源 37/55）
- **#984**（2026-03-25，12 条评论）「Synaptic plugin shows "Published" on submission portal but not appearing in marketplace directory」；（来源 56）
- **#1887**（2026-05-16，8 条评论）标题自己写着「follow-up to #1272」。（来源 57）

【事实】其余可点开的抱怨样本：#587 无法添加官方仓（16 评论）、#379「All LSP plugins missing `.lsp.json`」（12 评论）、#6854（**取证日当天**）「security-guidance: UserPromptSubmit blocks every prompt ~4s (up to 30s timeout)」、#6848（前一天）「telegram plugin: orphaned getUpdates pollers pile up and spin, consuming many CPU cores」。（来源 52/58/59/60）【事实】

**【推断】治理层跑得比运维层快**：提交门、verified、renames 迁移表、安全扫描都上线了，但「已发布却不可见」这条链路在 4 月、3 月、5 月三次复发，1,045 个 open issue 是这套体系的待办清单。（来源 63/4/37/56/57/10）【推断】

## 结论：两条可抄 + 一条可抄的坑

**可抄其一，把分发位做进初始化。** 首启自动挂载目录 + 显式关闭开关 + README 第一屏写信任警告，这套组合既拿到了曝光，又把信任责任写清楚了，官方文档连关法都给了。（来源 13/3）【事实】对任何想让工具被「第一眼看到」的团队，这比投广告便宜。（来源 13）【观点】

**可抄其二，把兼容性成本显式化。** 「slug 不可变」写进 README、改名走 `renames` 表由加载器自动迁移，10 条历史记录就是这套机制的运行痕迹——**用一张表替用户扛住了改名的破坏性**。（来源 3/4）【事实】

**可抄的坑：别把流水线活跃度当社区。** 73.8% 的提交来自一个 bot、67.5% 的 PR 是版本号 bump、releases 与 tags 双 0，这些都不妨碍仓库挂在 Trending 上，但它们回答不了「谁在真正用、谁在真正修」——那个答案在 1,045 个 open issue 和「已发布却不见」的 36 条评论里。（来源 6/12/23/24/10/37）【事实】**而当同一件事同时存在 315、341、2,000+ 三个对外数字时，任何一个数字被单独引用都是不完整的**（来源 4/40/41）【观点】——这句是写给我们自己的。

---

## 来源（取证日 2026-10-22，全部可点开）

1. GitHub 仓库页 https://github.com/anthropics/claude-plugins-official （截图取证）
2. GitHub API 仓库 https://api.github.com/repos/anthropics/claude-plugins-official
3. README 原文 https://raw.githubusercontent.com/anthropics/claude-plugins-official/main/README.md
4. marketplace.json 原文（315 条 / renames 10 条）https://raw.githubusercontent.com/anthropics/claude-plugins-official/main/.claude-plugin/marketplace.json
5. GitHub API 递归文件树（835 条）https://api.github.com/repos/anthropics/claude-plugins-official/git/trees/af59ee3a1051a29dda4a2e7a2af221aa457ae08b
6. GitHub API 贡献者（bot 3,417 / claude 34）https://api.github.com/repos/anthropics/claude-plugins-official/contributors?per_page=100
7. GitHub API 提交第 1 页 https://api.github.com/repos/anthropics/claude-plugins-official/commits?per_page=100
8. GitHub API 提交（近期密度）https://api.github.com/repos/anthropics/claude-plugins-official/commits?per_page=100&page=2
9. GitHub 检索 issue 总数（1,277）https://api.github.com/search/issues?q=repo:anthropics/claude-plugins-official+type:issue
10. GitHub 检索 open issue（1,045）https://api.github.com/search/issues?q=repo:anthropics/claude-plugins-official+type:issue+state:open
11. GitHub 检索 PR 总数（5,516）https://api.github.com/search/issues?q=repo:anthropics/claude-plugins-official+type:pr
12. GitHub 检索 bump 标题 PR（3,721）https://api.github.com/search/issues?q=repo:anthropics/claude-plugins-official+type:pr+bump+in:title
13. 官方文档 · 三档 marketplace 与预装开关 https://code.claude.com/docs/en/plugins/anthropic-marketplaces （截图取证）
14. Wayback 2025-12-14（41★）https://web.archive.org/web/20251214042254id_/https://github.com/anthropics/claude-plugins-official
15. Wayback 2026-01-28（5.1k）https://web.archive.org/web/20260128022013id_/https://github.com/anthropics/claude-plugins-official
16. Wayback 2026-03-23（14.2k）https://web.archive.org/web/20260323064939id_/https://github.com/anthropics/claude-plugins-official
17. Wayback 2026-04-25（17.8k）https://web.archive.org/web/20260425190630id_/https://github.com/anthropics/claude-plugins-official
18. Wayback 2026-05-23（25.9k）https://web.archive.org/web/20260523125524id_/https://github.com/anthropics/claude-plugins-official
19. Wayback 2026-06-29（31.3k）https://web.archive.org/web/20260629071335id_/https://github.com/anthropics/claude-plugins-official
20. Wayback 2026-08-19（33.7k）https://web.archive.org/web/20260819083302id_/https://github.com/anthropics/claude-plugins-official
21. Wayback 2026-09-24（36.7k）https://web.archive.org/web/20260924154939id_/https://github.com/anthropics/claude-plugins-official
22. Wayback 2026-10-14（38.1k）https://web.archive.org/web/20261014234400id_/https://github.com/anthropics/claude-plugins-official/
23. GitHub API releases（`[]`）https://api.github.com/repos/anthropics/claude-plugins-official/releases?per_page=100
24. GitHub API tags（`[]`）https://api.github.com/repos/anthropics/claude-plugins-official/tags?per_page=100
25. GitHub Trending 周榜（+5,703 / cursor +2,049）https://github.com/trending?since=weekly （截图取证）
26. @ClaudeDevs 推文（16,161 赞 / 1,703,030 浏览）https://x.com/ClaudeDevs/status/2106118517447876618
27. aitoolly 模板稿 2026-08-28 https://aitoolly.com/ai-news/article/2026-08-28-anthropic-launches-official-claude-code-plugins-directory-on-github-for-enhanced-developer-workflow
28. aitoolly 模板稿 2026-08-29 https://aitoolly.com/ai-news/article/2026-08-29-anthropic-launches-official-claude-code-plugins-directory-on-github-to-enhance-developer-ecosystem
29. aitoolly 模板稿 2026-10-18 https://aitoolly.com/ai-news/article/2026-10-18-anthropic-launches-official-claude-code-plugins-directory-on-github-to-curate-high-quality-ai-extens
30. aitoolly 模板稿 2026-10-19 https://aitoolly.com/ai-news/article/2026-10-19-anthropic-launches-official-claude-code-plugins-directory-on-github-for-high-quality-developer-exten
31. Hacker News 主帖 · Discover Claude Code Plugins and Marketplaces（1 分）https://news.ycombinator.com/item?id=45544549
32. Hacker News 评论 · 两个 marketplace 的困惑（2026-03-02）https://news.ycombinator.com/item?id=47217579
33. GitHub API 提交第 47 页（末页 30 条 / 首提 2025-11-20）https://api.github.com/repos/anthropics/claude-plugins-official/commits?per_page=100&page=47
34. GitHub API 最近 100 个 PR（70 个 bump）https://api.github.com/repos/anthropics/claude-plugins-official/pulls?state=all&per_page=100
35. GitHub 仓库 · claude-plugins-community（4,722★ / 2026-03-20）https://github.com/anthropics/claude-plugins-community
36. GitHub 仓库 · claude-code（demo marketplace）https://github.com/anthropics/claude-code
37. issue #1272（36 评论 · Published 却不在目录）https://github.com/anthropics/claude-plugins-official/issues/1272
38. Hacker News 评论 · ralph-loop 因法务改名（2026-02-23）https://news.ycombinator.com/item?id=47124314
39. Claude Marketplace 网页目录（Anthropic verified）https://claude.com/marketplace/plugins （截图取证）
40. TrendinTech（341 条 / Environments API 10-16 公测）https://trendintech.com/claude-code-plugins-341-cursor-environments-api-2026/
41. 官方通稿 · Claude Marketplace（2,000+）https://claude.com/resources/articles/claude-marketplace
42. ExplodingTopics · Anthropic 收入盘点（Claude Code run-rate $2.5B / 4% 公开提交）https://explodingtopics.com/blog/anthropic-revenue
43. TechTimes（launched May 22）https://www.techtimes.com/articles/317139/20260525/claude-code-plugins-get-official-directory-anthropic-flags-unverified-mcp-risks.htm
44. developersdigest（Already at 23k / 2026-05-22）https://www.developersdigest.tech/blog/github-trending-claude-plugins-official-2026-05-22
45. claudecamp（launched May 22, 2026）https://claudecamp.ai/blog/claude-code-plugins-official-directory
46. GitHub API LICENSE（Apache-2.0）https://api.github.com/repos/anthropics/claude-plugins-official/contents/LICENSE?ref=main
47. Anthropic 官方通稿 · Series H（$65B / $965B / run-rate $47B）https://www.anthropic.com/news/series-h （截图取证）
48. Sacra · Anthropic（$65B 年化 / 6-1 保密递交 IPO）https://sacra.com/c/anthropic/
49. GitHub 仓库 · cursor/plugins（13,504★）https://github.com/cursor/plugins
50. Codex marketplace 分发指南（2026-04-11）https://codex.danielvaughan.com/2026/04/11/codex-marketplace-plugin-distribution/
51. OpenAI 插件文档（取证日 403，未采信）https://developers.openai.com/plugins/build/plugins
52. issue #587（16 评论 · cannot add 官方仓）https://github.com/anthropics/claude-plugins-official/issues/587
53. GitHub API · anthropics/claude-plugins（404）https://api.github.com/repos/anthropics/claude-plugins
54. PromptArmor · Hijacking Claude Code via Injected Marketplace Plugins（2025-10-16）https://promptarmor.substack.com/p/hijacking-claude-code-via-injected （截图取证）
55. GitHub API · 评论最多的 issue 排序 https://api.github.com/repos/anthropics/claude-plugins-official/issues?state=all&sort=comments&direction=desc&per_page=100
56. issue #984（12 评论 · 同型复发）https://github.com/anthropics/claude-plugins-official/issues/984
57. issue #1887（8 评论 · follow-up to #1272）https://github.com/anthropics/claude-plugins-official/issues/1887
58. issue #379（12 评论 · LSP 缺 .lsp.json）https://github.com/anthropics/claude-plugins-official/issues/379
59. issue #6854（取证日 · 每次输入卡 ~4s）https://github.com/anthropics/claude-plugins-official/issues/6854
60. issue #6848（2026-10-21 · telegram 插件 CPU 空转）https://github.com/anthropics/claude-plugins-official/issues/6848
61. 官方文档 · Plugin security and trust（插件可带代码 / auto-update 换文件）https://code.claude.com/docs/en/plugins/security
62. 官方支持中心 · skill 与 plugin 扫描 https://support.claude.com/en/articles/15927065-get-started-with-skill-and-plugin-scanning
63. 官方文章 · 提交门户（付费计划 / 安全扫描 / 2026-09-25）https://claude.com/resources/articles/build-plugins-for-claude
64. Wayback CDX 快照索引（本仓全部存档时间戳）https://web.archive.org/cdx/search/cdx?url=github.com%2Fanthropics%2Fclaude-plugins-official&output=json&fl=timestamp,original,statuscode,digest&collapse=timestamp:8&limit=200

取证档案：`research/evidence/claude-plugins/`（**234 个文件 / 99 条唯一来源 URL**，manifest 带 sha256 与通道标注；通道 = 通道A 正文抓取 + GitHub REST 只读 GET + Wayback CDX/快照 + HN Algolia + fxtwitter + 四链搜索 + 通道D 截图 6 张）。

## 勘误与可撤回

- 本文任一判断被一手源推翻，请在来信指出处；核实后在本文对应句子后加「（勘误 <日期>）」并保留原句划线，**不删旧数字、不改历史口径**。
- 已知存疑项：插件数 341（媒体口径）、Sacra 的 $65B 年化与 6-1 保密递交 IPO、Claude Code run-rate $2.5B 与「Claude Code 写了 4% 公开提交」（均系第三方转述 Anthropic 自述）、Cursor Environments API 公测日期（单一媒体口径）、Claude Marketplace 的 verified 与安装量原始数（JS 渲染未取到）、`anthropics/claude-plugins-community` 与 demo 仓的当面星数会随时间变化。这些点**未当作事实使用**。

> 本站为独立技术分析，与文中项目无商业关系。
