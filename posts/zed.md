> **首发 [https://javis.im/analysis/zed/](https://javis.im/analysis/zed/)，本文随站点同步更新**；文中图片见在线原页，英文版：https://javis.im/analysis/zed/index-en

# 被 GitHub 关停的 Atom 团队回来了：Zed 91,923 星、Stack Overflow 2026 官方口径 11.4%，而它刚把自己仓库的 PR 关掉

> Atom 团队被 GitHub 关停后回来做 Zed：91,923 星、Stack Overflow 2026 官方口径 11.4%，却刚把自家仓库的 PR 关掉。本文先对账星、fork、issue 的口径，再拆 Rust 换手感与关 PR 的赌注。

取证日期：2026-10-20（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 20 · 可验证性 18 · 独特性 18 · 受众相关度 18 · 多维空间 18 = **92/100**（≥60 才动笔；素材库预分 81）。

## 这个站在做什么

取证日官网首屏一句话：**Your last next editor**——「Zed is a minimal code editor crafted for speed and collaboration with humans and AI」，下面三个卖点词：**Fast**（"Written from scratch in Rust to efficiently leverage multiple CPU cores and your GPU"）、**Agentic**（"Run agents in parallel to smoothly edit files, navigate code, and run tools at native speed"）、**Collaborative**（组队共写、共享屏幕与项目）；下载按钮旁标注支持 macOS、Linux、Windows。（来源 1）【事实】

产品版本节奏可以量化：GitHub API 实抓最新 release 为 **v1.24.3（2026-10-19 发布）**，而 1.0 的官方发布日是 2026-04-29——半年出到 1.24，约等于每 1–2 周一个次版本。（来源 19/5）【事实】【推断】公司面很小：jobs 页原话「All 3 of our founders have spent years in the trenches writing software」，取证日在招 **2 个岗位**（Open Source Engineer、Product Design Engineer）。（来源 15）【事实】

![Zed 官网首页首屏：Your last next editor + Fast/Agentic/Collaborative 三卖点，取证 2026-10-20](img/home.png)

## 对比横评 · 第一组数字：星、fork、issue，先把口径对账干净

这篇的分析框架卡是**对比横评**——不评「哪家强」，只把同一把尺子量出来的数摆齐，先做对账再下判断。

- **主将**：GitHub API 实抓 `zed-industries/zed` **91,923 星**、11,122 forks、`open_issues_count` **3,104**、`subscribers_count` 319、仓库创建 **2021-02-20**、最后一次 push 2026-10-19、`license.spdx_id` = **NOASSERTION**。（来源 16）
- **对账 ①（issue）**：仓库页面分列 **Issues 2.3k + Pull requests 788**，按 API 值 3,104 − 788 反推，2.3k 的实际值是 2,316【推断】——API 把 PR 算进 open issues，直接引用会比页面的 issue 数虚高 34%。（来源 16/17）【事实】
- **对账 ②（watchers）**：API `watchers_count` 值为 91,923、与 `stargazers_count` 完全相同（GitHub 历史遗留），页面侧栏「319 watching」才对应 `subscribers_count`。（来源 16/17）【事实】
- **对账 ③（许可证）**：API 返回 NOASSERTION、页面侧栏显示「License, **GPL-3.0** licenses found」、仓库根目录并列 `LICENSE-APACHE` 与 `LICENSE-GPL` 两个文件、README 原话「primarily under **GPL-3.0-or-later**, with Apache-2.0 components」——同一仓库四种口径，下面风险节展开。（来源 16/17/34）
- **对照组一（在位者）**：`microsoft/vscode` **193,972 星**、45,895 forks、open_issues 21,960、watchers 3,552、创建于 2015-09-03、168,230 commits、MIT。（来源 21/22）页面 Issues 只显示「5k+」+ PRs 2.9k，属粗粒度分档显示，**不能与 API 的 21,960 硬连线**【存疑】。（来源 22）
- **对照组二（遗作）**：`atom/atom` **60,712 星**、MIT、**archived**、创建于 2012-01-20、最后 push 2023-01-03。（来源 23）GitHub 官方 2022-06-08 公告《Sunsetting Atom》：「We are archiving Atom … for an official sunset on December 15, 2022」，理由是「retire Atom in order to further our commitment to … Microsoft Visual Studio Code and GitHub Codespaces」。（来源 24）【事实】

![GitHub 仓库页实抓：Star 91.9k / Issues 2.3k + Pull requests 788 / 40,480 commits / watching 319 / GPL-3.0，取证 2026-10-20](img/github.png)

三个换算放在一起才有画面感：**Zed 的星数是 Atom 遗作的 1.51 倍（+51%）**；是 VS Code 的 **47.5%**（反过来说 VS Code 是它的 2.11 倍）；仓库累计 **40,480 commits**，而它 2024-01-25 才正式开源——HN 首帖《The Zed Text Editor Is Now Open Source》当天只有 **2 points**，起点热度低到可以忽略。（来源 16/22/23/31）【事实】【推断】

## 对比横评 · 第二组数字：不看星，看谁在真用

星数可以靠时间堆，使用率不行。Stack Overflow 2026 Developer Survey 的官方数据页（n=13,193，问题为「过去一年你经常使用哪些开发环境」，**多选**）取证日实抓：

| 开发环境 | 使用人数 | 占比 |
|---|---|---|
| Visual Studio Code | 9,350 | **70.9%** |
| Notepad++ / Visual Studio / Vim | 3,673 / 3,643 / 3,586 | 27.8% / 27.6% / 27.2% |
| IntelliJ IDEA | 3,201 | 24.3% |
| Cursor | 1,971 | 14.9% |
| Neovim | 1,939 | 14.7% |
| **Zed** | **1,504** | **11.4%**（第 11 位） |
| Xcode / Sublime Text | 1,322 / 1,290 | 10.0% / 9.8% |

（来源 27）【事实】

![Stack Overflow 2026 调查 Dev IDEs 数据页：VS Code 70.9%、Zed 11.4%（1,504 人），n=13,193，取证 2026-10-20](img/so.png)

三条读法必须克制：其一，这是**多选自报样本**，11.4% 是「受访者中经常用 Zed 的比例」，不是市场份额，也不能乘以全球开发者数外推；其二，Zed 开源 33 个月、1.0 才 6 个月（2026-04-29），常用率就压过 Xcode、Sublime、Jupyter 进入第 11 位，且排在 Cursor（14.9%）之后、与 Nano（12.1%）同档【事实】；其三，官方分析语写着 VS Code「usage has remained **at or above 71%** since 2021」，而 2026 表值是 **70.9%**——四舍五入的边缘上第一次贴到线下，这个「首次」是本文的读法【推断】。（来源 27）

自报口径的另一侧是官方话术：1.0 博文称「**hundreds of thousands of developers** now rely on Zed to ship software each day」，无审计、无统计方法【存疑】。（来源 5）

## 技术栈考古插曲：97.69% 的 Rust 与「像游戏一样做编辑器」

为什么是重写而不是优化？1.0 博文把动机写得很白：Atom 是「a fork of Chromium」，副产品是 Electron，而「Electron eventually became the foundation of VS Code」——自家技术栈孵化了后来的霸主，也把自家封死了：「No matter how hard we worked, we couldn't make Atom better than the platform it was built on.」（来源 5）【事实】

于是新编辑器「built it like a **video game**, organizing the entire application around feeding data to shaders running on the GPU」，为此从零写了 Rust UI 框架 **GPUI**。（来源 5）【事实】代码构成印证这不是修辞：GitHub languages API 实抓字节数 **Rust 64,144,077 / 总 65,663,831 = 97.69%**，其后 Python 0.79%、Inno Setup 0.32%、Tree-sitter Query 0.28%。（来源 18）【事实】

渲染层的工程动作在 HN 有独立热度：《Zed editor switching graphics lib from blade to wgpu》**317 points / 308 comments**（2026-02-13）；更早的《All Occurrences Search from 1s to 4ms》（2024-02-18）拿 137 分——这个社区对「毫秒级」话题的敏感度本身就是产品叙事的一部分。（来源 29/31）【事实】

同班底的旁证：Tree-sitter（同为三位创始人作品）**27,223 星**、2026-10-19 仍在 push。（来源 25）【事实】而 GPUI 框架本体在 crates.io 只有 **374,259 次累计下载**（0.2.2，近 90 天 210,439，最后更新 2025-10-22）【事实】——与 Zed 的装机量不成比例，说明它主要被 Zed 自己 in-tree 消费、外部生态还很小【推断】。（来源 26）

![Zed 官方 1.0 博文（2026-04-29）：五年、超过百万行代码、built it like a video game，取证 2026-10-20](img/blog10.png)

## 横评的两处「自说自话」：性能表与扩展数

官方对比页《Zed vs. VS Code: An Honest Comparison for 2026》（标注 Updated May 1, 2026）逐行给出：Zed「Native Rust app with **<1s startup, <10ms typing latency, ~600MB RAM**」，VS Code「Good performance out of the box, but slows with many extensions. Can use **3-4GB+ RAM**」。（来源 3）【事实】

![Zed 官方 compare/vscode 对比页：性能与扩展逐行对照表，取证 2026-10-20](img/compare.png)

问题在于**裁判和选手是同一个人**：本文检索全网（DDG，2026-10-20）没有找到同条件的第三方基准——没有第三方拿同一大型仓库测两边的启动时间与打字延迟并公开方法【存疑】。（来源 3/31）站外 SEO 站的复述数字（2 倍启动、16 倍内存差）追根溯源仍指向 Zed 自家口径，不构成独立证据【存疑】。

扩展生态是同一枚硬币的另一面，且数字口径乱成一团：compare 页写 VS Code「Unmatched marketplace with **10,000+ extensions**」，站外统计从 55K 到 120K 不等（marketplace 页面经通道A 实抓只返回 241 字节的壳，官方精确数取证失败）【存疑】；Zed 自家扩展页的自述是「the **hundreds** of community-created extensions」，头部 HTML 扩展安装量 7.5M。（来源 3/14/31）【事实】【存疑】

横评的公平读法：**性能对比表按「当事人主张」采信，扩展数量按「量级差一个数量级以上」采信**，精确到个位的比较在这个证据面上做不了。

## 钱的横评：$0 / $10 / $30 与「卖 token 亏本」的自白

定价页取证日三档原样：**Personal $0 forever**（自带 API key 或外接 Claude Agent、Codex CLI）｜**Pro $10/月**（含 $5 token，超出按量；14 天试用送 $5 GPT-6 Luna 额度）｜**Business $30 per seat/月**（组织级 AI 模型策略、数据治理、RBAC；25 席起可走 order form）。（来源 4）【事实】

![Zed 定价页：Personal $0 / Pro $10 / Business $30 三档，取证 2026-10-20](img/pricing.png)

商业化的两处关键原话：Zed-hosted AI 按「**provider's list price plus a 10% markup**」透传（Business 博文同口径），官方并自曝此前「**selling tokens at a loss to most paying AI users** until we switched to token-based billing」，还顺手把行业补贴模式点名：「much like the early days of Uber and Lyft … We're none of those.」（来源 7/8）【事实】Business 已有署名客户 Mercury 与 Ramp；同时 FAQ 写明 **SSO、SAML、SCIM "planned but not currently available"**、Business 无免费试用。（来源 7/4）【事实】

融资线：**Series B $32M，Sequoia Capital 领投，2025-08-20 官方博文与 FinSMEs 同日双源确认，累计「over $42M」**；官方同时把钱的去向写成产品路线——「a new kind of operation-based version control that incrementally tracks the evolution of your code with edit-level granularity」。（来源 6/32）【事实】收入侧无任何官方口径，本文不给估算、不连线【存疑】。（来源 4/6）

## 第八步棋：关掉自家仓库的 PR

2026-08-12 官方发布 Delta（「A multiplayer environment for coding with agents」），一个月后（2026-09-16）公测并公布里程碑：「Last week, we crossed a key milestone: **we disabled pull requests on Delta's own repository**」，以及量化的一句「**33 of us have landed 570 changes to main since we turned off pull requests**」；页头挂的路线图原话是「**Follow our quest to replace GitHub.com**」。（来源 9/10）【事实】底座 DeltaDB 是「操作型版本控制」，官方强调「compatible with Git」，定位成 GitHub PR 15 年范式的替换项而非皮肤。（来源 9）【事实】

![Zed 官方 Delta 公测博文（2026-09-16）：disabled pull requests on Delta's own repository，取证 2026-10-20](img/delta.png)

第三方视角把它放进更大的局：The New Stack 报道标题即「Everyone's in a race to replace GitHub」，点出 Zed 关掉自家 PR 用共享 agent 线程做评审。（来源 33）【事实】社区执行侧也有档期：两个月内 66 项社区 Git 贡献合入（5 大功能 / 28 个 bug / 33 项改进，file history 一项就有 250+ upvotes、单 PR 超 1,600 行）。（来源 11）【事实】

【推断】「取代 GitHub」的路线与「关掉自家 PR」是同一件事的两面：先把内部工作流搬离 PR，才谈得上让外部团队搬离 GitHub。这条链的每一环都有官方出处，但**能否成规模是本文的推断**，Zed 未披露 Delta 的外部用户数。

## 风险与存疑清单

- **许可证张力是本篇最硬的风险点**：README 说源码「primarily under GPL-3.0-or-later」【事实】，GitHub API 却回 NOASSERTION【事实】，而 compare 页宣传「**Fully open source under permissive licenses**」——GPL 属 copyleft，与 permissive 不是一个词【事实】，两者已经互相打架。更早的导火索是 HN《Ask HN: Is the Zed editor open source?》（2025-02-21）：发帖人摘录官网二进制 EULA「2.2 License Limitations … (e) modify, translate or create derivative works of the Editor」，质疑与 GPL/AGPL 冲突、官网未声明何处非开源。（来源 30/34/3）【事实】官方的回应是动作而非辩论：2026-03-02 发布 ToS 与 Privacy 全文重写博文，自认「The old terms had ambiguous language, so we restructured them」，现行条款写明「Zed's source code is licensed separately under the open source licenses published with that source code」。（来源 12/13）【事实】
- **自动下载事件**：HN《Zed Editor automatically downloads binaries and NPM packages without consent》**239 points / 173 comments**（2024-07-08），讨论重心是编辑器静默拉二进制与 npm 包的供应链面。（来源 28）【事实】评论区同时存在为 VS Code 扩展网络权限辩护的声音，说明这是品类共性问题而非 Zed 独有【推断】。
- **性能数字全部自报**：见上节，无第三方同条件基准，<1s / <10ms / ~600MB 一律按当事人主张采信【存疑】。（来源 3）
- **口碑的阴面样本**：HN《Zed (Editor) Has Suddenly Become Terrible, IMHO》（2025-02-13，10 分）记录过一次体验回退的抱怨；10 分体量说明是孤例而非浪潮，但它在时间线上恰好落在 1.0 前【事实】【推断】。（来源 31）
- **Web 版缺席**：README 明写「Other platforms are not yet available: Web」，web 端仍在 discussion 阶段——对「到处能跑」的团队是硬约束。（来源 34）【事实】
- **SO 11.4% 的口径**：多选、自报、单平台不分主次，且样本是自愿参与者；它能证明「Zed 已进入主流视野」，不能证明份额【存疑】。（来源 27）
- **收入黑箱**：三档定价、+10% 透传、融资 >$42M 都可查，收入规模无官方数字，本文不引第三方估算【存疑】。（来源 4/6/32）

## 结论

把两组横评收成一句话：**Zed 用 97.69% 的 Rust 换来「当事人自报的毫秒级」，用 33 个月的开源之路换来 Stack Overflow 官方口径的 11.4%，再用关掉自家 PR 这个动作把「编辑器之争」升级成「协作范式之争」**——前两项各有口径折扣，第三项是它独有的赌注。（来源 3/5/9/18/27）【推断】对在位者的量化描述只有一句：VS Code 193,972 星、70.9% 常用率，Zed 是它的 47.5% 与 16%（11.4/70.9）。（来源 21/27）

对做工具的团队，三个可抄的动作：①**把「继承的尸体」变成叙事资产**——Atom 被 GitHub 官方关停这件事，被反向写成「我们早就知道 Electron 是天花板」的论证（来源 5/24）【观点】；②**性能主张要敢放在自己域名下接受审阅**——compare 页标明更新日期、逐行给数，同时把弱点（扩展少、企业功能早期）也写进去，可信度是靠自曝短板撑起来的（来源 3）【观点】；③**商业化用透传代替补贴**——provider+10%、亏本期自曝、Business 不捆绑 AI 额度，三条都在博文里公开，把定价道德化成了产品叙事（来源 7/8）【观点】。

一句清醒剂给同行：**当性能对比表的裁判就是选手本人，「更快」在证据层面只是一个主张**——11.4% 的官方调查数据比任何毫秒数都更接近真话，而它来自第三方问卷，不来自任何一家的官网。（来源 3/27）【观点】

---

**来源清单**（全部取证于 2026-10-20，UTC+8；通道A 服务端抓取 + 通道B 搜索交叉验证 + 通道D 截图，取证档案 `research/evidence/zed/` 98 文件，manifest 带 sha256）：

1. https://zed.dev/ — 官网首页首屏（Your last next editor / Fast / Agentic / Collaborative、macOS/Linux/Windows）
2. https://zed.dev/blog — 官方博客索引（2024-01 → 2026-09 发布档期与分类）
3. https://zed.dev/compare/vscode — 官方对比页（Updated May 1, 2026；<1s / <10ms / ~600MB vs 3-4GB+；VS Code "10,000+ extensions"；"Fully open source under permissive licenses"）
4. https://zed.dev/pricing — 定价三档（Personal $0 / Pro $10 含 $5 token / Business $30 per seat）与 FAQ（SSO/SAML/SCIM 未上、Business 无试用、14 天 Pro 试用含 $5 GPT-6 Luna）
5. https://zed.dev/blog/zed-1-0 — 官方《Zed is 1.0》（2026-04-29：五年、超过百万行、Atom→Electron→VS Code 天花板、built it like a video game、GPUI、hundreds of thousands of developers 自报）
6. https://zed.dev/blog/sequoia-backs-zed — 官方《Sequoia Backs Zed's Vision…》（2025-08-20：$32M Series B、累计 over $42M、operation-based version control 愿景）
7. https://zed.dev/blog/zed-for-business — 官方《Introducing Zed for Business》（2026-05-06：$30 per seat GA、Mercury 与 Ramp、provider cost +10%、org 级开关）
8. https://zed.dev/blog/not-building-ai-for-the-money — 官方《We're Not Building AI Features for the Money》（2026-05-05：selling tokens at a loss、+10% 透传、Uber/Lyft 补贴类比、Conrad Irwin 自述手写代码不足一半）
9. https://zed.dev/blog/delta-public-beta — 官方《Replace PRs with Delta – Now in Public Beta》（2026-09-16：disabled PRs on Delta's own repository、33 人 570 次合入、quest to replace GitHub.com、DeltaDB 兼容 Git）
10. https://zed.dev/blog/introducing-delta — 官方《Introducing Delta》（2026-08-12：agent 多人协作环境）
11. https://zed.dev/blog/lets-git-together — 官方《66 Git Improvements In Under 2 Months》（2026-02-05：5 大功能 / 28 bug / 33 改进、file history 250+ upvotes、单 PR 1,600+ 行）
12. https://zed.dev/blog/terms-update — 官方《We Overhauled Our Terms of Service and Privacy Policy》（2026-03-02：old terms had ambiguous language、不卖数据不默认训练）
13. https://zed.dev/terms — 现行条款（Last Updated March 2, 2026；source code is licensed separately；页脚 License: GPL-3.0 / Apache-2.0）
14. https://zed.dev/extensions — 官方扩展页（hundreds of community-created extensions；HTML 扩展 7.5M 安装）
15. https://zed.dev/jobs — 招聘页（All 3 of our founders、取证日在招 2 岗）
16. https://api.github.com/repos/zed-industries/zed — GitHub API（91,923 星 / 11,122 forks / open_issues 3,104 / subscribers 319 / created 2021-02-20 / pushed 2026-10-19 / license NOASSERTION / watchers_count 与 stargazers 同值）
17. https://github.com/zed-industries/zed — 仓库页实抓（Issues 2.3k + Pull requests 788、40,480 commits、Star 91.9k、watching 319、License GPL-3.0、LICENSE-APACHE + LICENSE-GPL 文件在列）
18. https://api.github.com/repos/zed-industries/zed/languages — 语言字节构成（Rust 64,144,077 / 65,663,831 = 97.69%）
19. https://api.github.com/repos/zed-industries/zed/releases/latest — 最新 release v1.24.3（2026-10-19T12:43:19Z）
20. https://api.github.com/search/repositories?q=org:zed-industries&sort=stars&per_page=30 — org 星矩阵（zed 91,923 / extensions 1,932 / awesome-gpui 1,458 / community 1,164 / zed-fonts 1,052）
21. https://api.github.com/repos/microsoft/vscode — VS Code API（193,972 星 / 45,895 forks / open_issues 21,960 / watchers 3,552 / created 2015-09-03）
22. https://github.com/microsoft/vscode — VS Code 仓库页实抓（Star 194k、Issues 5k+ + PRs 2.9k 分档显示、168,230 commits、MIT license）
23. https://api.github.com/repos/atom/atom — Atom API（60,712 星、MIT、archived、created 2012-01-20、pushed 2023-01-03、forks 17,157）
24. https://github.blog/news-insights/product-news/sunsetting-atom/ — GitHub 官方《Sunsetting Atom》（2022-06-08 公告、2022-12-15 正式 sunset、转向 VS Code 与 Codespaces 的原话）
25. https://api.github.com/repos/tree-sitter/tree-sitter — Tree-sitter API（27,223 星、pushed 2026-10-19）
26. https://crates.io/api/v1/crates/gpui — crates.io gpui（0.2.2、累计 374,259 下载、近 90 天 210,439、updated 2025-10-22）
27. https://survey.stackoverflow.co/2026/technology/data/dev-ide — Stack Overflow 2026 官方数据页（n=13,193：VS Code 70.9%、Cursor 14.9%、Zed 11.4%/1,504 第 11 位、Xcode 10.0%、Sublime 9.8%；分析语 "at or above 71% since 2021"）
28. https://news.ycombinator.com/item?id=40902826 — HN《automatically downloads binaries and NPM packages without consent》（2024-07-08，239 points / 173 comments）
29. https://news.ycombinator.com/item?id=47002825 — HN《Zed editor switching graphics lib from blade to wgpu》（2026-02-13，317 points / 308 comments）
30. https://news.ycombinator.com/item?id=43133031 — HN《Ask HN: Is the Zed editor open source?》（2025-02-21，EULA 2.2 License Limitations 摘录与 GPL/AGPL 质疑）
31. https://hn.algolia.com/api/v1/search?query=zed%20editor&tags=story&hitsPerPage=30 — HN 榜单实抓（317 / 274 / 239 / 137 / 100 分诸帖；《Zed (Editor) Has Suddenly Become Terrible, IMHO》10 分 2025-02-13；开源首帖 2 分 2024-01-25）
32. https://www.finsmes.com/2025/08/zed-raises-32m-in-series-b-funding.html — FinSMEs 融资复述（2025-08-20：$32M Series B、Sequoia 领投、Boulder、CEO Nathan Sobo、累计 over $42M）
33. https://thenewstack.io/zed-delta-github-alternative/ — The New Stack 报道（「Everyone's in a race to replace GitHub」、Zed 关闭自家 PR 用共享线程评审）
34. https://raw.githubusercontent.com/zed-industries/zed/main/README.md — 仓库 README（primarily GPL-3.0-or-later + Apache-2.0 components、Web 版未支持、Atom 与 Tree-sitter 出身、hiring 链接）

**勘误**：本文任何错误可在 javis.im 留言指出，核实后修订；若结论被推翻，本文将标注撤回。文中【事实】/【推断】/【观点】/【存疑】四色标注即为可信度分层，读者可只采信【事实】部分。官方自报的性能与「hundreds of thousands of developers」、SO 调查的多选自报口径、compare 页与 README 的许可证用词冲突、扩展市场 10,000+ 与站外 55K–120K 的口径乱战、API 与页面的分档显示差异，均已随文列出而不是抹掉。

> 本站为独立技术分析，与文中项目无商业关系。
