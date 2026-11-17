# 3 颗星、17 次浏览、1 次安装：aibox.im 把 AI 工具最土的一步「装好」写成了可验证的验收标准

> 拆的是 aibox.im：一个把 21 个 AI 桌面工具装到能真的回话的便携安装器，上线 8 天、GitHub 3 颗星、全站 17 次浏览 1 次安装。读完你能拿到它在分发层的坐标、与包管理器的能力边界，以及未签名便携程序与自动写入密钥的风险面。

取证日期：2026-11-17（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 20 · 可验证性 18 · 独特性 15 · 受众相关度 17 · 多维空间 18 = **88/100**。框架卡 = **三框架合并**（第 21 篇用增长拆解，本篇不用；且该站 2026-11-09 才建仓，历史长度本身排除了增长拆解）：按「**行业坐标 → 产品格 → 对比格 → 口径审计 → 钱线格 → 风险格 → 意义与边界**」走。题眼一句话：**它想解决的不是「装软件」，是「装完能不能真的回话」——而这件事恰好落在包管理器的能力边界之外。**（来源 1/6/9/14）

![aibox.im 英文首页取证截图，2026-11-17](img/shot_home.png)

## 一、坐标：这一层在行业里被谁占着

先把位置说清楚，后面才谈得上价值。用户面对「想让 AI 在自己电脑上干活」时的路径无非四条：官方安装器（Ollama、Chatbox 自家的 exe/dmg）、通用包管理器（winget / Chocolatey / Scoop）、AI 应用聚合客户端、以及「安装层本身」——帮别人把别人家的软件装好的那一层。

前三条都有明确的自有边界：官方安装器只负责自家；包管理器负责「文件到位」且以系统级条目为单位；聚合客户端是「一个壳装很多能力」。**第四条很少被单独命名，但确实存在**——2023 年 7 月起运营的 Pinokio 就是这条路，2 年 4 个月做到 8,410★、MIT、474 个 open issues，站内还上线了让 AI agent 把任意东西打包成一键启动器的「Create」功能。（来源 23/24/25/26）

AiBOX 是这条路线上一个 8 天大的新入场者。它的三个自有仓在 **2026-11-09T06:08:52Z** 前后 6 分钟内同时创建，属于一个 **User 账号**（不是组织），0 followers、无 bio、无 location；`aibox-app`（Rust/Tauri）3★、`aibox-registry`（JavaScript）0★、`aibox-site`（TypeScript/Astro）0★，commits 分别 30 / 29 / 41。（来源 15/16/17/18/19）

【事实】这个体量与「行业意义」之间的距离，需要用一组更硬的数来测量：站内公开的 `/api/stats` 直接返回逐应用的 `{views, installs}`——取证日 13 个应用有记录，**views 合计 17（最高 jan 3 次，openclaw 与 aidog 各 2 次），installs 合计 1（仅 ollama 记 1 次，其余全部 0）**，另外 8 个应用连记录都没有；GitHub Release 的两个资产下载计数各为 2。（来源 20/21/22）

![aibox.im 应用目录页取证截图，21 个条目的分类与难度标注，2026-11-17](img/shot_apps.png)

**所以本文要回答的不是「它现在有多大」，而是「它切的那一刀是否正确」**——一个 8 天、1 次安装的项目，如果切的是一道真命题，它的工程选择就值得被拆开看；如果切的是一道伪命题，那 1 次安装本身就是答案。

## 二、产品格：三步背后是「验收标准」而不是「安装流程」

产品自述的三步很短：下载一个便携文件 → 选一个应用 → 跟着点。真正值得写的是每步背后被写进代码的约束。（来源 1/6）

**第一步的约束是「不碰你的系统」。** 单文件便携 exe，双击即跑，数据落 `%LOCALAPPDATA%\AiBOX\`，不写注册表、不改 PATH、不要管理员权限；`bundle.active=false` + `--no-bundle`，明确**不出 NSIS/MSI 安装包**。（来源 6/7）

**第二步的约束是「先体检再动手」。** 目录数据来自公开仓 `aibox-registry`，安装器与网站读的是同一份数据源；每个应用有依赖 DAG 与 `routePlans`，OhOpenClaw 这类需要 Node 的应用会写明版本区间——页面实测给出的是「推荐 Node ≥26.12.1、支持 ≥24.22.1，22/23/25 均不被 OpenClaw 支持」，而 AiBOX 的做法是**另装一个受支持的 Node，不动用户已有的那个**。（来源 6/9/10）

**第三步的约束最硬：probe。** registry 的 JSON Schema 里有一条死规矩（**V2**）——每个包必须有 `install.probe`，原文理由写得很直白：「一个无法验证版本的安装任务就是失败的任务」，且 `probe.expect` 必须能编译成正则。也就是说**安装成功的定义被从「退出码 0」改成了「能读到版本 / 能回一条真实消息」**。（来源 10/12）

其余三条不变量同样指向同一件事：**V1** 每个暴露 `channels[].url` 的包必须带包级 `sha256` + `sizeBytes`，且同一包的多个渠道共用一个哈希（无哈希的 URL 不得合并）；**V3** `configTarget`/`dataPaths`/`residualPaths` 必须是 `~/` 用户路径，禁止系统目录、禁止 `..` 穿越；**V4** 注册源必须声明 ≥2 个可交叉校验来源（jsDelivr / raw / GitHub API）。（来源 10）

**【观点】** 这四条不变量组合起来，描述的其实是一份「不要相信自己」的工程纪律：不相信下载到的文件（V1）、不相信安装脚本的退出码（V2）、不相信写入位置的路径拼接（V3）、不相信单一 CDN（V4）。对一个 8 天大的项目，这份纪律的密度明显高于它的星标数量——这也是把它当行业标本而不是当热门项目来拆的理由。（来源 10/12）

## 三、对比格：与包管理器、官方安装器、同层先行者逐项对

这一格全部用可核事实，不做优劣排序。

| 对照对象 | 起始 | stars | 许可 | 交付单位 | 「装完能用」由谁保证 |
|---|---|---|---|---|---|
| **AiBOX** | 2026-11-09 | 3（app 仓） | **无 LICENSE 文件** | 便携单文件（免管理员） | 包级 `probe` + 写 key + 真实回话验证 |
| Pinokio | 2023-07-09 | 8,410 | MIT | 脚本化 launcher | 脚本自身逻辑，站内生态为大 |
| winget（microsoft/winget-pkgs） | 2019-07-16 | 11,337 | MIT | 系统级包条目 | manifest 安装命令退出即完成 |
| Chocolatey（chocolatey/choco） | 2014-12-30 | 11,568 | Apache-2.0 | 系统级包（企业侧） | 同上 |
| Scoop（ScoopInstaller/Scoop） | 2013-05-11 | 24,819 | Unlicense | **用户级免提权** | 同上 |

（来源 15/23/27/28/29）

**三个必须说清的对照结论：**

1. **「免管理员」不是空白**。Scoop 自 2013 年起就以「用户级、免提权」为核心卖点，24,819★ 是这一诉求成立的最好证据。AiBOX 的差异不在这句口号，而在**连包管理器这一层都不装**——它自己就是一个可直接双击的文件。（来源 29）
2. **「AI 应用一键装」不是空白**。Pinokio 在同一位置运营了 2 年 4 个月，8,410★ 与 474 个 open issues 共同说明这是一门有真实需求、也真有维护债的活。（来源 23/24/25）
3. **AiBOX 真正不可替代的动作只有一个：代写并验证 API key**。【事实】OpenClaw 教程列出了六条取 key 路径，key 最终落 `~/.config/openclaw/openclaw.json5`，写入前备份原文件；FAQ 原文（第一人称自述）为「key 写进应用自己的配置文件，不经过 us，us 连能看到它的服务器都没有」。包管理器与官方安装器都不做这件事——因为这让「装配」多了一道「配置」，而配置恰恰是新手真正的失败点。（来源 9/11）

**【推断】** 这三条合起来给出的定位是：它不在「包管理器」这一层竞争（那一层已由四个成熟项目钉死），而在「包管理器与用户之间的空白处」——即把「版本约束 + 配置填空 + 沉默失败」这三件包管理器不管的事接过来。这个位置成立与否，取决于第 5 条的覆盖率：**21 个应用对上游长尾（GitHub 上 AI 桌面项目数以千计）的覆盖率极低，人工精选的目录是它的天花板，也是它唯一能保持 probe 质量的原因。**（来源 8/10/14）

## 四、口径审计：三项对外表述与实测的逐条对表

**① SHA-256：对上了。** download 页公开两个哈希——`9093bf32…b2662`（Windows exe，16.5MB）与 `cb0985c4…e103a`（macOS dmg，13.3MB）。本文用 GitHub API 的 release asset `digest` 字段逐字符比对：**完全一致**，且 asset size（17,266,176 / 13,986,672 字节）与页面标注体积一致；版本 v0.1.3 发布时刻 2026-11-13T02:18:53Z。（来源 3/4/5）【事实】这一项在同类项目里并不常见——多数项目只给「下载」不给可对表的哈希。

**② 「live」的计数：真值是个位数。** 目录卡片上的 views/installs 在服务端渲染时是占位破折号，由客户端脚本拉 `/api/stats` 后填充（实测等待 networkidle 后 21 张卡片全部有值）。真值即上文：**views 合计 17、installs 合计 1**。这个数字与 Release 资产各下载 2 次互为旁证。（来源 20/21/22）【事实】

**③ 「open source」：三仓都没有 LICENSE 文件。** about 页写「Open source… Read the code, audit the checksums」，llms.txt 写「the AiBOX desktop app (open source…)」，FAQ 第 10 条写「AiBOX is free and open source」；同时三仓 API 返回 `license: null`，根目录 18/13/16 个条目逐一核对**无 LICENSE / LICENSE.md**。（来源 2/13/15/16/17）【事实】公开可读与获得许可是两件事——**无许可文件在法律默认下等于保留全部权利**，代码可以读、可以审校验和，但「用、改、再分发」并未被授予。本文并列这两件事实，不替任何一方作法律判断。

**④ 自承未验证项。** registry README 的 TODO 原文列出：`icons/openclaw.png` 为占位图；Windows 静默安装参数与安装路径**待真机验证**；macOS dmg 安装脚本、Gatekeeper 指引、macOS 的 CN 代理通道**待真机验证**；上游 `openclaw/openclaw` 未发 Windows NSIS 资产；`recommendedModels` 与价格**待 CONFIG 页上线前复核**。（来源 12）【事实】**主动列出未验证项的项目不多，这条比任何「已全面验证」的口号更值得记一笔。**

## 五、钱线格：一张空白画布

- **收入：无。**【事实】全站与三仓未见定价页、订阅、付费档、捐赠或赞助入口；FAQ 直答「How much does it cost? → Nothing. AiBOX is free and open source. You only pay the AI provider whose API key you already have」。（来源 1/13/6/12）
- **主体：不可查。** GitHub 为 User 账号（非组织），无公司页、无团队页、无融资信息、无 location；about 页只自称「independent community project」，联系入口为邮箱与 GitHub Issues。（来源 18/2）
- **许可：未声明**（见第四节），因此连「靠开源许可衍生商业版」这条常见路径也无法从仓内推断。（来源 15/16/17）
- **成本侧可估**：站点为 Astro 静态站 + Cloudflare Pages（响应头 `server: cloudflare`、`cf-cache-status: DYNAMIC`），无数据库；registry 用 GitHub Actions 每日 `0 4 * * *` 探针 + 推 PR 的 `GITHUB_TOKEN`（README 明确「不需要 PAT」）。（来源 6/10/31）

**【观点】** 这张画布目前是空的，但空得**有结构**：零收入 + 零主体 + 未声明许可 + 边缘静态站，四项叠加意味着边际成本接近零、也意味着没有任何商业闭环。对这种项目，「能否持续」不取决于融资，而取决于**单一维护者愿意为一个无人付费的工具承担多久的上游漂移**——而这恰好是它的工程纪律在提前回答的问题（下节）。

## 六、风险格：五个可核的敞口

**① 未签名便携二进制。** 16.5MB exe 与 13.3MB dmg 无代码签名，页面自述 SmartScreen 会提示「unknown publisher」，并给出四步绕过指引；macOS 侧同样给 Gatekeeper 右键 Open 指引。安全证据链只有三条间接项：公开源码（可审）、SHA-256（可对表）、以及 registry V1/V2 的哈希与 probe 强制。【事实】反过来说，**签名缺位意味着用户的第一道防线只有「哈希与页面一致」这一条**，而哈希只能证明「你下到的与发布的一致」，不能证明「发布的本身无害」。（来源 3/6/11/32/33）

**② 密钥写入即持有。** 「不经过服务器」是可核的（无状态后端 + 边缘函数，响应头无 set-cookie），但**写入用户配置文件之后，密钥就以明文形式与目标应用共存**；页面给的三条自保规则写得很直白：「绝不发给任何人、截图打码、泄漏就去 provider 后台吊销」。备份原文件再写入的设计降低了破坏配置的风险，但不改变明文落盘的事实。（来源 9/11）【事实】

**③ 分发载体是 GitHub Releases 上的可执行附件。** 该项目把 16.5MB exe 与 13.3MB dmg 挂在 Release 资产上，CI 在 `tag v*` 时于 Windows runner 构建并附上。**本文只记录这一形态与页面自述，不对任何平台对可执行附件的治理口径作推断**；把它列为风险项的理由是客观的：一旦该账号或该 Release 面不可用，唯一官方分发渠道即刻中断，而站内没有第二分发通道（镜像只用于应用包的下载，不用于 AiBOX 自身）。（来源 6/7/34）

**④ 上游漂移与「不自作主张」的两难。** 每日探针把漂移分 A/B 两类：A 类（上游发新版）自动测新产物、只改版本相关字段、跑校验、重 HEAD 全渠道（0 失败才过）、推分支开 PR，PR 正文附旧→新证据表并可幂等跳过；**B 类（同版本哈希/体积不符、渠道或网关死亡）永不自动改数据**，只开 `upstream-drift` issue 并让 CI 变红，理由原文是「版本没动而内容变了，可能是篡改信号，必须人工」。（来源 10）【观点】这条设计把「自动化」限定在可验证的维度上，是正确的取舍；代价是**B 类完全依赖单个维护者响应**，41 个 registry issue 与 3★ 的体量放在一起看，响应能力的上限很低。（来源 10/19）

**⑤ 镜像与地域通道的单点。** README 记载 `ghfast.top` 对 `ollama/ollama` 全部资产返回 403（2026-11-11 实测），因此 Ollama 只发 `gh-proxy` 单通道；`ghfast.top` 对 Comfy-Org 资产同样 403，ComfyUI 也是单通道；失败候选进 `retiredCandidates`，重测通过才重新准入。（来源 10）【事实】单通道在「主通道失效」时无退路，而这类代理镜像的可用性本身不受项目控制。

**另附两条背景风险**：【事实】一是**上游商标与收录授权**——站方只声明「不隶属、商标归各自所有者」，未声明收录已获各项目方授权；二是**统计口径**——`/api/stats` 的计数规则（是否按 UA 去重、是否含未执行 JS 的访问）未公开，故本文只把它当量级参考，不作精确用户量使用。（来源 8/20）

## 七、意义与边界：它答对了哪道题，又答不了哪道

把六格收拢，可以给出一个不依赖规模的价值判断。

**它答对的那道题：AI 桌面工具的失败点分布，与包管理器的能力边界不重合。** 包管理器交付「文件到位」，而这批应用的失败集中在三处：版本约束（OpenClaw 明确不支持 Node 22/23/25，推荐 ≥26.12.1）、配置填空（六条 provider 取 key 路径、baseURL、模型名）、以及配完不报错的沉默失败。AiBOX 把三处都做成了机制：另装受支持的 Node 而不动用户环境、代写 key 前先备份、probe 强制「能读到版本/能回真实消息」。（来源 6/9/10/12）【事实+推断】

**它的意义因此不在「又一个安装器」，而在于把「能用」变成了可执行、可审计的验收条件。** 这套验收条件（V1–V4 + probe + A/B 漂移分治）是公开可复用的工程范式，其价值不取决于这个项目能活多久：任何人做同类工具都可以直接照抄这四条不变量。（来源 10）【观点】

**它的边界同样清楚，且三条都在项目自己的文档里：**

1. **覆盖率天花板**：21 个应用是人工精选，上游长尾以千计；每增加一个应用就要付一份「sha256 + sizeBytes + probe + 双语教程 + 渠道测速」的成本，这条路不可能靠人力铺满。（来源 8/10/12）
2. **验证深度不均**：README 自承 macOS 安装脚本、Gatekeeper 指引、macOS 的 CN 代理通道、Windows 静默安装参数**都还没在真机上验证完**；「21/21 stable」的状态标注与这份 TODO 并存，说明 stable 是「已可用」而非「已全验」。（来源 12/8）
3. **持续性单点**：零收入、无主体、未声明许可、单人维护，叠加 B 类漂移必须人工处置的设计——**系统对上游变化的响应能力，等于一个人的响应能力**。（来源 10/15/16/17/18）

**【观点】** 所以对这道题的判断是：**切口成立，样本太小**。它切的「装完能不能回话」是一道真命题，且给出的四条不变量值得被行业吸收；但它目前只是这道命题的一次单人验证，1 次安装既是它的现状，也是它必须跨过的起点。（来源 20/21/22）

## 八、三个只需同口径重跑即可复核的问题

1. `/api/stats` 的 installs 在 30 天后是否还停在 1 位数量级？——同一端点重读即可，无需任何授权。
2. registry 的 TODO 里那几项「待真机验证」是否被清空？——README 的 TODO 段是公开的版本化文本，逐条对照即可。
3. 三仓是否补上 LICENSE 文件，使「open source」表述与许可状态一致？——API `license` 字段与根目录列表可一键复核。

## 来源（取证日 2026-11-17，全部可点开）

1. AiBOX 英文首页 https://aibox.im/en/
2. AiBOX About 页（「independent community project… not affiliated… trademarks belong to their respective owners」「Open source… Read the code, audit the checksums」）https://aibox.im/en/about/
3. AiBOX Download 页（v0.1.3、2026-11-13、exe 16.5MB / dmg 13.3MB、SHA-256 `9093bf32…b2662` 与 `cb0985c4…e103a`）https://aibox.im/en/download/
4. GitHub API · aibox-app release v0.1.3（asset digest 与 size：exe 17,266,176B / dmg 13,986,672B）https://api.github.com/repos/imAiBOX/aibox-app/releases/latest
5. GitHub API · aibox-app releases 列表（v0.1.1 2026-11-11 仅 exe；v0.1.2/v0.1.3 2026-11-13 exe+dmg；下载计数各 2）https://api.github.com/repos/imAiBOX/aibox-app/releases?per_page=5
6. aibox-app README（Tauri v2 + React18 + Vite + Tailwind4、三 Rust crate、便携分发与 `%LOCALAPPDATA%\AiBOX\`、**No self-replacing updater** 原文与理由、`bundle.active=false` + `--no-bundle` 不出 NSIS/MSI、CI 表含全历史脱敏扫描）https://raw.githubusercontent.com/imAiBOX/aibox-app/main/README.md
7. GitHub API · aibox-app 仓库（Rust、3★、created 2026-11-09T08:24:54Z、pushed 2026-11-16）https://api.github.com/repos/imAiBOX/aibox-app
8. AiBOX 应用目录页（21 个应用、分类与难度、status stable）https://aibox.im/en/apps/
9. OpenClaw 教程页（Node 推荐 ≥26.12.1 / 支持 ≥24.22.1、22/23/25 不支持、另装新 Node 不动原有、key 落 `~/.config/openclaw/openclaw.json5`、六条取 key 路径）https://aibox.im/en/apps/openclaw/
10. aibox-registry README（SSOT 三层数据流、V1–V4 不变量、每日 `0 4 * * *` 探针五步、A/B 漂移分治与「B 类永不自动改」原文、ghfast.top 对 ollama/Comfy 资产 403、`retiredCandidates` 机制、数据基线带日期、TODO 自承未验证项）https://raw.githubusercontent.com/imAiBOX/aibox-registry/main/README.md
11. AiBOX FAQ（站方自述「key 写进应用自己的配置文件、不经过站方」「无管理员权限」「免费开源」「SmartScreen/Gatekeeper 属正常」逐条问答）https://aibox.im/en/faq/
12. aibox-app CI 表（`tag v*` 于 Windows runner 构建并附 Release；i18n 键位比对、vitest、三 crate `cargo test`、双平台 `cargo check`）https://raw.githubusercontent.com/imAiBOX/aibox-app/main/README.md
13. AiBOX llms.txt（机读站点摘要：21 应用、`/meta/index.json`、MCP `POST https://aibox.im/mcp`、源仓三地址、「open source」表述、默认英文）https://aibox.im/llms.txt
14. AiBOX agents.md（面向 agent 的站点说明）https://aibox.im/agents.md
15. GitHub API · aibox-registry（JavaScript、0★、created 2026-11-09T08:24:49Z、`license: null`）https://api.github.com/repos/imAiBOX/aibox-registry
16. GitHub API · aibox-site（TypeScript/Astro、0★、created 2026-11-09T08:25:02Z、`license: null`）https://api.github.com/repos/imAiBOX/aibox-site
17. GitHub API · 三仓根目录树（18 / 13 / 16 个条目，逐一核对无 LICENSE / LICENSE.md）https://api.github.com/repos/imAiBOX/aibox-app/contents/
18. GitHub API · owner `imAiBOX`（**User** 类型、created 2026-11-09T06:08:52Z、0 followers、bio 空、blog aibox.im）https://api.github.com/users/imAiBOX
19. GitHub API · commits Link 头（aibox-app / registry / site 全量页数 30 / 29 / 41）https://api.github.com/repos/imAiBOX/aibox-app/commits?per_page=1
20. AiBOX `/api/stats`（逐应用 `{views, installs}`、`store: kv`、`updatedAt: 1794915160985`）https://aibox.im/api/stats
21. AiBOX 首页内联脚本（`data-stats-app` / `data-k="views|installs"` 客户端填充逻辑，SSR 为占位破折号）https://aibox.im/en/
22. AiBOX `/meta/index.json`（registryVersion 2026.11.17.1、generatedAt 2026-11-17T05:25:00Z、21 个应用的 category/difficulty/popularity/platforms/status）https://aibox.im/meta/index.json
23. GitHub API · pinokiocomputer/pinokio（8,410★、created 2023-07-09、MIT、474 open issues、pushed 2026-11-05）https://api.github.com/repos/pinokiocomputer/pinokio
24. Pinokio 站点（站内「Create」由 AI agent 打包任意内容为一键启动器，日历显示 2 天前公告）https://pinokio.computer/
25. Pinokio 仓（同上，脚本化 launcher 生态）https://github.com/pinokiocomputer/pinokio
26. Pinokio 站点首页抓取存档（`research/evidence/aibox/pinokio.html`）https://pinokio.computer/
27. GitHub API · microsoft/winget-pkgs（11,337★、created 2019-07-16、MIT、仓库体量 975,695KB）https://api.github.com/repos/microsoft/winget-pkgs
28. GitHub API · chocolatey/choco（11,568★、created 2014-12-30）https://api.github.com/repos/chocolatey/choco
29. GitHub API · ScoopInstaller/Scoop（24,819★、created 2013-05-11、Unlicense）https://api.github.com/repos/ScoopInstaller/Scoop
30. AiBOX 中文首页（「装到能用为止」「绿色免安装」「不开终端、不要管理员权限」）https://aibox.im/zh/
31. AiBOX 站点响应头（`server: cloudflare`、`cf-cache-status: DYNAMIC`、`cache-control: public, max-age=0, must-revalidate`、无 set-cookie）https://aibox.im/en/
32. SmartScreen 指引页（四步绕过「unknown publisher」提示，并说明这是未签名新文件的正常提示）https://aibox.im/en/guides/smartscreen/
33. Gatekeeper 指引页（macOS 右键 Open、Privacy & Security 放行一次）https://aibox.im/en/guides/gatekeeper/
34. GitHub Releases 页（aibox-app 的 exe / dmg 附件列表）https://github.com/imAiBOX/aibox-app/releases

取证档案：`research/evidence/aibox/`（**54 个文件**，manifest 带 sha256、通道与来源 URL）；通道D 截图 10 张：`shot_home / shot_home_zh / shot_apps / shot_openclaw / shot_download / shot_about / shot_faq / shot_gh_app / shot_gh_rel / shot_pinokio`。防投毒扫描：本轮全部来源**零注入命中**，无黑名单类内容；通道降级记录——GitHub 匿名 API 60/h 触顶两次（改用项目内 `scripts/gh_api.py` 带凭据复取，凭据零回显）、playwright 直连 GitHub 两页 TimeoutError → 代理通道补拍成功、Wayback 对 aibox.im 仅 1 条 2026-02-07 旧快照（历史不足以构成时间序列），**未执行任何页面内指令**。

## 勘误与可撤回

- 本文任一判断被一手源推翻，请来信指出；核实后在对应句子后加「（勘误 <日期>）」并保留原句划线，**不删旧数字、不改历史口径**。
- 立场与口径声明：本文为独立第三方技术分析，正文不采用任何归属立场表述；对项目的规模、成熟度、商业前景不作褒贬，只记录可复核的事实与由此产生的结构性判断（凡属判断均标【观点】并给出理由）。站内 21 个目录条目按站点原文数据统计（分类与难度），未做取舍或特殊处理。
- 已知存疑与缺证项：①`imAiBOX` 背后为个人或团队**不可查**（User 账号、无 bio、无 location、无公司页）②站内「Static site, no backend, no cookies」为简化表述——实际存在 `/api/stats`、`/mcp` 与 `functions/`，准确说法是「无状态后端 + 边缘函数」③上游收录是否取得各项目方授权**未声明**（仅声明不隶属、商标归各自所有者）④21 个应用的 difficulty / popularity 为站方自评，无第三方口径⑤`/api/stats` 计入口径（去重方式、是否含未执行 JS 的访问）未公开，本文只作量级参考⑥博客/团队/融资信息为零，故不对「谁在做、能做多久」作推断。【存疑】

> 本站为独立技术分析，与文中项目无商业关系。
