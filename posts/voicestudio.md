# 6 个月 80,473 星、HN 只有 6 分：这个「免费 ElevenLabs」的默认引擎，权重写着 CC-BY-NC

> **利益冲突披露**：本文作者所在团队运营 AI 网关与助手产品，本身就是语音 API 的潜在付费方，评「本地替代云订阅划不划算」时有切身立场；本文所在站点的拆站栏目本身是内容营销，选题取自自建素材库（预分 79，库内最高 NEW），存在「挑好拆的写」的动机。全篇文章只对文末 38 个来源负责：项目方自述、独立审计与批评证据同时列出，每个判断句背后都有可点开的来源，口径不一致的数字全部列出而不强行连线。文中所有「Buy Pro」「安装命令」一类原文一律只作数据引用，未执行。

取证日期：2026-10-22（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 19 · 可验证性 19 · 独特性 19 · 受众相关度 19 · 多维空间 16 = **92/100**（≥60 才动笔；素材库预分 79，为库内最高 NEW）。框架卡 = **对比横评**（上一篇为技术栈考古，横评上一次是第 8 篇，不连用）。

## 这个仓在做什么

`debpalash/VoiceStudio` 的自述一句话：「open-source, **fully-local ElevenLabs alternative** — voice cloning, voice design, video dubbing, dictation, transcription & audiobook creation in 646 languages」，主页字段指向 `voicestudio.sh`，许可 AGPL-3.0，建仓 `2026-04-09T21:40:26Z`，最近一次 push `2026-10-21T19:58:46Z`。（来源 1/2/3）【事实】

技术形态是**壳 + 胶水**：Electron 桌面壳（2026 年 9 月从 Tauri 迁过来，v0.5.3 是最后一个 Tauri 版）+ FastAPI 后端 + SQLite，对外开一个 loopback 的 OpenAI 兼容接口与 MCP server；底下按 registry 挂引擎。`docs/engines/README.md` 的 TTS 表**当前正好 17 行**（VoiceStudio/OmniVoice 默认、VoxCPM2、MOSS-TTS-Nano、KittenTTS、MLX-Audio、CosyVoice 3、GPT-SoVITS、sherpa-onnx、IndexTTS 2.5、OmniVoice GGUF、Supertonic-3、MOSS-TTS-v1.5、dots.tts、OmniVoice subprocess、PocketTTS、Confucius4-TTS、audio.cpp）。（来源 6）【事实】

![仓库页取证截图：Star 80.5k、Fork 8.9k、Issues 120、PR 179、41 Branches、42 Tags、4,491 Commits、AGPL-3.0，取证 2026-10-22](img/github.png)

![docs/engines/README.md 的 TTS 引擎表取证截图（17 行），取证 2026-10-22](img/engines.png)

作者面：`debpalash` = Palash Debnath，个人主页写「Creator - VoiceStudio.sh / CTO - Yupcha.com」，所在地 India，账号 2013 年注册、35 个公开仓、1,387 followers；他的其他仓里 Opal 96★、OpenGTM 64★，**VoiceStudio 一个仓是第二名的 800 倍以上**。（来源 12）【事实】

## 横评主表：官网那张「逐行对比」我核了一遍

官网首页自己摆了一张四列对比表（OSS / Pro / Cloud / ElevenLabs Cloud），页脚写着「Facts from each project's own public pages, **checked 30 Sept 2026**」——**这是当事人主张，不是第三方横评**。（来源 14）【事实】我逐行拿到一手源核对，结果是这样：

| 行 | 官网说法 | 我核到的一手源 | 判定 |
|---|---|---|---|
| 源码 | OSS AGPL-3.0 / Pro Closed / Cloud Closed / ElevenLabs Closed | 仓库 LICENSE = AGPL-3.0；`/pro` 页自述「a **separate, closed-source build**」；`/license` 页写 Pro/Lifetime 是闭源商业许可（来源 4/16/15） | 成立【事实】 |
| 语言数 | 646 / 646 / 646 / **74 on paid** | 我逐行数 `docs/languages.md`：**646 行数据行**（表体 648 行 = 表头 + 分隔行 + 646），与「646 languages / 581k hours」自述一致（来源 5）；ElevenLabs 侧官方 docs 口径是 **Eleven v3「70+ languages」、Multilingual v2 = 29、Flash v2.5 = 32、Scribe v2 ASR 90+**，「74」这个数出自官网自己的表（来源 14），官方 docs 只写「70+」（来源 23） | 我方 646 成立；对方 74 **存疑**（官方只说 70+）【存疑】 |
| 音频去哪 | Stays local / Uploaded | `/license` 与 `/faq`：本地应用不留存；Cloud「is a preview contract you can read today. **Nothing is live yet**」（来源 15/18） | 成立，但 Cloud 那一列卖的是期货【推断】 |
| 起步价 | Free / $99·yr or $299 / soon / **Credits, from $6** | `/pro` 页：Pro **$99 per user / year**、Lifetime **$299 per user · one-time**；ElevenLabs 官方 pricing 页 Starter **$6/月**、Creator $22、Pro $99、Scale $299、Business $990（来源 16/22） | 成立【事实】 |

![官网首页自建的四列对比表取证截图（页脚标注 checked 30 Sept 2026），取证 2026-10-22](img/home_cmp.png)

![ElevenLabs 官方定价页取证截图：Free $0 / Starter $6 / Creator $22 / Pro $99 / Scale $299 / Business $990，取证 2026-10-22](img/el_pricing.png)

这张表**没写**的那一行才是要害，第 5 节单独说。

## 价格这条线：$0 / $99 / $299 对 $6 到 $990

ElevenLabs 官方 pricing 页取证日实抓：Free $0（10k credits/月）→ Starter $6（30k）→ Creator **$22**（121k，首月 5 折到 $11）→ Pro $99（600k）→ Scale $299（1.8M，3 席）→ Business $990（6M，10 席）→ Enterprise 定制。（来源 22）【事实】VoiceStudio 侧：桌面应用 $0（AGPL-3.0、无账号）；Pro $99/用户/年、Lifetime $299/用户一次性，含 3 台设备、3 个并发、7 天无理由退款；Enterprise 走询价表单。（来源 16/17/18）【事实】

同赛道另一侧的独立数字可以拿来对齐量级：HN 上另一个「自建 ElevenLabs 替代」的作者（amuletvoice.com，**不是本文对象**）自报成本 **$0.01/分钟 对 ElevenLabs 约 $0.11/分钟**，并公布两周 10,000+ 用户、700+ 付 $5、**0 人买 $499 买断**。（来源 25）【存疑】——当事人自述、无第三方复核，只作量级参考，也顺带说明「买断制」在这个赛道有多难卖。

## 「646 语言」逐行数：行数是真的，可用的不是

`docs/languages.md` 开头自述「**646 languages** with a total of **581k hours**」。（来源 5）【事实】独立审计 solosoft 用最笨的办法核：数行 = **646 行**、渲染器 `languages.json` = 647 条（646 + 一个 Auto 哨兵）、ISO 639-3 代码零重复，时长加总 **581,489.4 小时**，与自述严丝合缝。（来源 26）【事实】

但把时长排序，形状就很直白：**≥10,000 小时的语言只有 10 个（占目录 1.5%）**，100–1,000 小时 43 个，**10–100 小时 305 个（47.2%）、1–10 小时 254 个（39.3%）、不足 1 小时 11 个**；**265 个语言的训练材料不到 10 小时，中位数 10.18 小时**；目录后一半的 323 个语言加起来 2,715 小时 = 语料的 0.47%；英语一门就 206,061 小时 = 35.4%；真正过 100 小时的只有 76 个、过 1,000 小时的只有 33 个。（来源 26）【事实】最短的是 Macedo-Romanian **0.02 小时 = 72 秒**。（来源 26）【事实】

「646」因此不是 646 个可用嗓音，是 646 个被指过的语言代码——**大约三十几个够上生产级**（来源 26）【推断】。对 Tamil / 普通话 / 西语 / 英语的用户这不是问题；为纳瓦特尔语来的用户，拿到的是一段 180 秒的田野录音（来源 26）【推断】。

## 三层许可：应用干净、默认权重不干净、第三层在子目录里

**第 1 层，应用本身：AGPL-3.0-only。** `/license` 页写得很清楚：「Run it, for anything, including at work, and for money」「**Using VoiceStudio commercially does not by itself require anything from you**」，AGPL 的网络条款只在你改了还架给别人用时才咬。（来源 15/4）【事实】

**第 2 层，默认引擎的权重：CC-BY-NC。** 默认引擎是 `k2-fsa/OmniVoice`（仓库 15,449★、代码 Apache-2.0），它的 Hugging Face 模型卡 License 段原句：「Our code is released under the Apache 2.0 License. **The pre-trained model is licensed under the CC-BY-NC** due to constraints from its training data (e.g., Emilia).」（来源 19/21）【事实】——这句我当面复核过，同时 HF API 的 `license` 字段是 `null`（**卡片 YAML 里根本没有 license 字段，所以 Hub 不显示许可徽章**），月下载 1,936,235、1,577 likes。（来源 20）【事实】CC-BY-NC = 非商用。

**第 3 层，音频 tokenizer：** 仓内 `backend/.../audio_tokenizer/LICENSE` 是「Boson Higgs Audio 2 Community License Agreement」，自述「based upon the Meta Llama 3 Community License Agreement」——第三份文件、第三个条件集。（来源 26）【事实】

![Hugging Face k2-fsa/OmniVoice 模型卡 License 段取证截图：CC-BY-NC 原句，取证 2026-10-22](img/hf.png)

披露是**分层的**，四份文件我各自抓了原文，逐层看：仓库根 `LICENSE-NOTICE.md` 写得最全——「The default `k2-fsa/OmniVoice` model card identifies its code as Apache-2.0 and pretrained weights as **CC-BY-NC**」「Its `audio_tokenizer/LICENSE` contains separate Boson Higgs Audio 2 and Meta Llama community terms」「**A commercial license for VoiceStudio-owned code does not replace any of those terms**」（来源 37）【事实】；`docs/engine-acceptance.md` 第 2 条要求把 code / weights / tokenizer 分开登记许可，「Unknown or conflicting terms do not count as commercial approval」，并规定「**Pro must reject non-commercial or uncleared assets on the backend**」，后面跟着一句「the controls remain **tracked in #2587**」（来源 38）【事实】；README 的 License 段写「Model weights, tokenizers, voices and generated-output conditions are **separate**; a paid app licence does not grant those rights」（来源 3）【事实】；`/license` 页写「The speech models… carry their own licences… **a few are research-only**」（来源 15）【事实】。

缺的一环在**面向用户的那一屏**：`docs/engines/README.md` 的引擎表列只有 Engine / Guide / Runs on / Cloning / Enabled by，**没有许可列，正文不点名 CC-BY-NC**（来源 6）【事实】；`/pro` 页卖的是「Commercial License for **paid client work**」，同样不提默认权重（来源 16）【事实】——**规则层写到位了，商店层与选择器层还没接上**（来源 6/16/37）【推断】。若严格执行，Pro 后端必须拒绝非商用资产，而默认引擎恰好是 CC-BY-NC；该控制项状态在 #2587 里是「tracked」，**取证日未能确认已强制生效**（来源 38）【存疑】。

时间差也要记：solosoft 2026-09-28 写的是「不在 README、不在官网、不在 App 里」，它引的当时版收引擎标准是「Licence clean for commercial use… **No research-only weights**」，并记录 `LICENSE_GATED_ENGINES = {"supertonic3","pockettts"}` 要弹许可确认、`audiocpp` 被标「weights research/non-commercial」（来源 26）【事实】；取证日该文档已改写得更细（来源 38）【事实】。andrew.ooo 在 10 月更新里仍把这条写成「**the catch nobody mentions**」（来源 27）【事实】——**批评与披露在赛跑**（来源 26/27/37）【观点】。

换许可这件事发生过三次，节奏很能说明作者是懂许可机制的：2026-04-29 `chore(license): switch Studio to FSL-1.1-ALv2; commercial pricing TBD` → 2026-06-06 `chore(license): relicense from FSL-1.1-ALv2 to AGPL-3.0 (open-core)` → 2026-07-16 `docs: ... fix AGPL-3.0 license detection (#1168)`（后者是让 GitHub 探测器报 AGPL-3.0-only 而不是 Other，因为企业扫描器会卡采购）。（来源 26/10）【事实】

第三方对同一处的独立判断是收敛的：andrew.ooo 评测把这条写进标题副句「**one real license catch**」，正文「The catch nobody mentions: the app is AGPL-3.0, but the default model weights are CC-BY-NC. Commercial use requires switching engines.」（来源 27）【事实】

![独立评测 andrew.ooo 取证截图：79,351 星（+12,178/7 天）与 license catch 段落，取证 2026-10-22](img/andrew.png)

![独立审计 solosoft 取证截图：40,145 星 + 6 分 HN 帖 + 默认引擎 CC-BY-NC 的标题，取证 2026-10-22](img/solosoft.png)

## 星曲线与 HN 冷遇：渠道错位

补库时记的是 **79,092★**（2026-10-21），取证日 GitHub API **80,473★ / 8,881 forks / 299 open_issues_count / 290 subscribers**，仓库页显示 **80.5k**——**当面复核成立且仍在涨**。（来源 2/1）【事实】

曲线形状靠 Wayback 存的 API 快照重建（solosoft 做的）：2026-09-01 **13,353** → 09-10 22,255 → 09-11 **27,277**（单日 +5,022）→ 09-26 35,774 → 09-28 40,145；andrew.ooo 记录 10-15 **67,173★**（+12,178/7 天）、10-21 **79,351★**（+11,656 或 +12,178/7 天，两篇口径不同）。（来源 26/27）【事实】——**9 月一个月翻了三倍**，之后 17 天再翻一倍多。

同一时间，**Hacker News 几乎没看见它**：solosoft 数出**共 9 次提交、最高 6 分、0 评论**（「VoiceStudio – local open-source ElevenLabs alternative」，2026-09-15），**域名 voicestudio.sh 一次都没被提交过**；上游模型自己的帖「OmniVoice, high-quality TTS for 600+ Languages」只有 **3 分**。（来源 26/24）【事实】取证日抓到的那条 HN 主帖页面确实显示「**6 points by iamsyr**」、无评论区内容。（来源 24）【事实】

![Hacker News 主帖取证截图：6 points、0 评论，取证 2026-10-22](img/hn.png)

![官网首屏取证截图：导航栏 GitHub 计数 50,000、「#1 Repository Of The Day」徽章、Docker 103K+ 与 646 languages，取证 2026-10-22](img/home.png)

星是从别的地方来的：落地页的社证墙挂着 YouTube（25K、4.4K、1.8K、990 次观看）、X（**鸟哥 234K views**、8K views）、Instagram（1.9K likes、28 likes）与 LinkedIn 的引用，首页还挂着「**#1 Repository Of The Day**」的 Trending 徽章与 Trendshift #1。（来源 14/15）【事实】GitHub Trending 周榜取证日在列。（来源 33）【事实】报道面也是同一个形状：同一家站（aitoolly）在 09-17、09-29、10-15 连发三篇标题模板几乎相同的稿子（「VoiceStudio: Open-Source Local ElevenLabs Alternative…」），独立评测（andrew.ooo）、逐数审计（solosoft）与日文/中文的解读各一——**数量多、复跑少**（来源 32/27/26/30/31）【事实】。——**星数衡量触达，不衡量评审**（来源 26）【观点】；对一个你要把客户工作塞进去的工具，这个区别要命。

## 商业面：Pro 与 Cloud 卖到哪一步

`/pro` 页在卖：Pro **$99/用户/年**、Lifetime **$299/用户**，闭源独立构建，多设备（3 台 / 3 并发）、批配音队列、watch folders、remote GPU workers、Commercial License 发到邮箱；页上明确写「**Closed-source distribution and private hosting aren't in Pro or Lifetime**」。（来源 16）【事实】

支付口径有两条，时间不同：FAQ（**更新于 2026-09-30**）写明「第三方支付：印度 **Razorpay**、其他地区 **Zoho Payments**、部分地区 PayU」「7 天全额退款」；而 solosoft 在 2026-09-28 抓到的是另一套——`pro-license.ts` 调 `https://api.lemonsqueezy.com/v1/licenses`，且规格文档写着「No paid checkout or feature unlock is live until the gates pass」，官网当时还写着「The Pro page does not currently sell or grant a licence」。（来源 18/26）【事实】两条**不矛盾但对不上**：9 月末这套商业面正在从「半建成 + 闸门」切到「可收款」，**取证日没能独立确认 checkout 端到端可用**（`/pro` 页 Buy 按钮只回链到自身 `/pro`，无外部支付域）【存疑】。

Cloud 是**明确的期货**：`/cloud` 页自述「a preview contract you can read today. **Nothing is live yet**」，`/faq` 答「Is Cloud available? **Not yet.**」，只有 `POST /v1/voices`、`/v1/dubbing`、`/v1/audio/transcriptions`、`/v1/audiobooks/plan`、`/v1/usage`、`/v1/jobs`、`/v1/pronunciation` 这组 preview 端点与一个通知名单。（来源 17/18）【事实】——**官网那张四列表里的「Cloud · soon」列，卖的是一份可读的合同**（来源 17）【推断】。

![VoiceStudio Pro 定价页取证截图：$99/年、$299 买断、闭源独立构建、7 天退款，取证 2026-10-22](img/pro.png)

![VoiceStudio 许可页取证截图：AGPL 商用无需任何授权 + Models and dependencies 段，取证 2026-10-22](img/license.png)

## 口径对账：五组数各自为政

- **星数四个时点**：79,092（补库 10-21）→ **80,473**（API 10-22）→ 页面 80.5k → 官网导航栏 GitHub 计数仍显示 **50,000**（截图取证日实见）（来源 2/1/14）【事实】——官网自己的星数计数**落后约 3 万**，而落地页「Figures observed 30 Sept 2026」的说明又标着 9 月底（来源 14）【事实】。
- **下载量三口径**：落地页写「~**596,000** GitHub releases」「Docker Hub **103,171+**」「GHCR **40,443+**」，顶部导航写「**50,000** Download」；solosoft 9-28 数到 534,737 次 release 下载、101,256 次 Docker pull；andrew.ooo 记录同图镜像 `palashdeb/omnivoice-studio` 与 `ghcr.io/debpalash/voicestudio`（linux/amd64 only）。（来源 14/26/27）【事实】——**同一页面上 50,000 与 596,000 并存**，两者差着量级。
- **open issues 口径**：API `open_issues_count` **299** = Issues 标签 120 + PR 179（截图实见）；andrew.ooo 10-21 记录「113 open issues」、solosoft 9-28 记录 17。（来源 1/2/26/27）【事实】——**PR 会混进这个字段**，跨来源比之前要拆开。
- **提交量与人**：仓库页取证显示 **4,491 Commits / 41 Branches / 42 Tags**；solosoft 9-28 口径是 3,381 commits（172 天 ≈ 20 提交/天）、**owner 独写 82.3%**、API 贡献者 71 人、第三名是 dependabot[bot]。（来源 1/26）【事实】我取证日 `contributors` 分页取到 30 人，debpalash **3,402** 次、velixio 404 次、第三名 kevin9327 只有 54 次。（来源 10）【事实】——**bus factor = 1**（来源 26）【推断】。
- **发布节奏**：42 个 release、最新 **v0.5.6（2026-09-23）**，此后近一个月没有新 release 但 commit 一直在推（10-21 仍有 merge）；tags 取证日抓到 42 个。（来源 11/1/10）【事实】

## 与本地同行的位子

「代码 MIT、权重非商用」在本地 TTS 是系统性坑，不是这一家：selfhoststack 的对照表把 **Kokoro-82M（Apache-2.0、可商用、CPU 跑）**、**Chatterbox（MIT、可商用）**、**OpenVoice v2（MIT）**、**Piper（MIT）**、**XTTS-v2（权重非商用 ✘）**、**F5-TTS（权重非商用 ✘）** 排成一列，直接把这条叫「the license trap: a repo can be MIT while the downloadable voice model forbids commercial use」。（来源 28）【事实】

Chatterbox 那条「盲测 63.75% 听众更偏好它而不是 ElevenLabs」要带利益冲突读：**该盲测由 Resemble AI（Chatterbox 的出品方）委托 Podonos 做**。（来源 29）【事实】——同理，官网那张四列对比表的作者是 VoiceStudio 自己（来源 14）【事实】。两份「赢」都是当事人给的。

对 VoiceStudio 的定位，第三方的概括比它自己的更准：「它不是在跟 OmniVoice 或 CosyVoice 竞争，**它把它们装运了**；它竞争的是你原本要花一个下午把它们接起来的那段时间」。（来源 27）【观点】。

## 风险与存疑清单

- **默认权重非商用是硬的**：CC-BY-NC 写在 HF 模型卡 License 段，且 HF API `license=null` 不出徽章；要商用必须换引擎，而 Pro 卖的恰恰是「paid client work」的商业许可（来源 19/20/16）【事实】——**买了商业许可，仍然得离开默认引擎才能商用**（来源 26/19）【推断】。
- **质量证据几乎为零**：`docs/benchmarks.md` 承诺「nothing is estimated」，Results 段却写着「**No verified rows yet**」只有一行占位；`.github/workflows/evals.yml` 写明 LLM-judge evals「NEVER a gate」、无 LLM 后端时报告 skipped，`run_evals.py`「exits 0 whether cases pass or fail」；README 的质量说法全部来自上游模型作者自己跑的论文数据，**不是这个 App 管线的复跑**（来源 26/8）【事实】。
- **安装文档是最大的文档**：`docs/install/troubleshooting.md` 72,160 字节，约为 README 的 6 倍（来源 26）【事实】；README 的硬件表里 Windows on ARM 标「Experimental, validation pending」、Intel Mac「App UI only」（来源 3）【事实】；README 正文本身在 9 月被第三方记为带 warning box 的 active beta（来源 27/31）【事实】。
- **安装器未签名**：Electron 安装包 unsigned / ad-hoc signed、非 Apple 公证，macOS 自动更新未经验证（来源 27）【事实】。
- **克隆域名面**：除官方 `voicestudio.sh`（仓库 homepage 字段唯一指向，来源 2）【事实】外，搜索里还挂着 `voicestudio.cc`（页面内容与官方高度同构，挂同一镜像 `docker pull palashdeb/omnivoice-studio`，**未见任何关联声明**，来源 34）与 `voicestudio.lol`（页脚自称「an independent third-party website… **not affiliated with**… voicestudio.sh」，但站内挂着 Plans and pricing / Refund Policy，来源 35）【存疑】——**两者与官方的关系未能核实，只登记不采信**。
- **面向 agent 的分发位**：README 直接给了一段「Paste into your coding agent (Claude Code, Codex, Cursor, …)」的安装提示词（来源 3）【事实】——这是**有意识的 agent 分发设计**，不是本文执行的指令；同款设计也出现在它给 agent 的 `docs/install/agent.md` 里。
- **声音克隆的同意与滥用**：项目把 consent 做进数据库字段、接了 Meta 的 AudioSeal 水印（同类里少数），FAQ 写「Only with the owner's consent」、HF 模型卡自带「prohibited from using this model for unauthorized voice cloning, voice impersonation, fraud, scams」的免责声明（来源 18/26/19）【事实】；但本地、离线、零审批的克隆能力**把服务方的合规检查换成了用户自己的责任**（来源 30/15），第三方报道也把克隆滥用列为该仓的主要外部风险（来源 36）【推断】。
- **无公司主体与收入口径**：全站与仓库都没有收入、融资、团队规模口径；FAQ 只写了支付通道与 GST 发票（印度）（来源 18/12）【事实】——**商业化进度无法从外部验证**。

## 结论

给要省钱的创作者：**本地替代在「无限量 + 数据不出机器」这件事上是成立的**，$0 对 $22/月（Creator、121k credits）或 $99/月（Pro、600k credits）这笔账在量大时一边倒（来源 22/14）【事实】；代价是你要自己当运维，且第一件要做的是**换掉默认引擎**（来源 19/26）【推断】。

给要接客户工作的人：先读 `LICENSE-NOTICE.md` 与目标模型的权重许可，再看 Pro 那张 $99/年的 Commercial License——它管的是 VoiceStudio 自己的代码，管不到权重（来源 15/26）【事实】。

给做开发者关系的同行：这个项目把**三件事当成一等公民**——给 agent 的安装提示词与 `npx skills add`、落地页的社证墙（YouTube/X/IG/LinkedIn 全挂）、以及把 AGPL 写成白话的许可页，这三件都可直接抄（来源 3/14/15）【观点】；它踩的坑同样可抄：**披露文件越写越全（LICENSE-NOTICE / engine-acceptance / README 三层都改过了），用户真正会碰的那一屏——引擎表——始终没有许可列**（来源 6/37/38）【事实】。README 里还挂了一行「Apply for a paid placement」的赞助位入口（来源 3）【事实】，开源仓把广告位明码标出来，这一手也值得记。

---

## 来源（取证日 2026-10-22，全部可点开）

1. GitHub 仓库页 https://github.com/debpalash/VoiceStudio （截图取证）
2. GitHub API https://api.github.com/repos/debpalash/VoiceStudio
3. README 原文 https://raw.githubusercontent.com/debpalash/VoiceStudio/main/README.md
4. LICENSE 原文 https://raw.githubusercontent.com/debpalash/VoiceStudio/main/LICENSE
5. 语言目录 https://raw.githubusercontent.com/debpalash/VoiceStudio/main/docs/languages.md
6. 引擎表 https://raw.githubusercontent.com/debpalash/VoiceStudio/main/docs/engines/README.md
7. 功能目录 https://raw.githubusercontent.com/debpalash/VoiceStudio/main/docs/feature-catalog.md
8. 性能文档 https://raw.githubusercontent.com/debpalash/VoiceStudio/main/docs/performance.md
9. GitHub API languages https://api.github.com/repos/debpalash/VoiceStudio/languages
10. GitHub API contributors https://api.github.com/repos/debpalash/VoiceStudio/contributors
11. GitHub API releases https://api.github.com/repos/debpalash/VoiceStudio/releases
12. 作者主页 https://api.github.com/users/debpalash
13. GitHub API issues 检索 https://api.github.com/search/issues?q=repo:debpalash/VoiceStudio+type:issue+state:open
14. 官网首页（四列对比表 / 社证墙 / 下载数）https://voicestudio.sh/
15. 官方许可页 https://voicestudio.sh/license
16. 官方 Pro 页 https://voicestudio.sh/pro
17. 官方 Cloud 页 https://voicestudio.sh/cloud
18. 官方 FAQ（支付通道 / 退款）https://voicestudio.sh/faq
19. Hugging Face 模型卡（CC-BY-NC 原句）https://huggingface.co/k2-fsa/OmniVoice
20. HF API https://huggingface.co/api/models/k2-fsa/OmniVoice
21. 上游仓库 https://api.github.com/repos/k2-fsa/OmniVoice
22. ElevenLabs 定价 https://elevenlabs.io/pricing
23. ElevenLabs 语言支持官方文档 https://elevenlabs.io/docs/help-center/other/what-languages-do-you-support
24. Hacker News 主帖（6 points）https://news.ycombinator.com/item?id=49706572
25. Hacker News 另一自建替代的成本自述 https://news.ycombinator.com/item?id=44830043
26. 独立逐数审计（星曲线 / 646 逐行 / 许可三层 / HN 9 提交）https://www.solosoft.dev/post/voicestudio-local-elevenlabs-alternative-audit/
27. 独立评测（license catch / 17 引擎 / 安装面）https://andrew.ooo/posts/voicestudio-review-local-elevenlabs-alternative/
28. 本地同行与许可坑对照 https://selfhoststack.net/elevenlabs-alternatives
29. Chatterbox 盲测（由出品方委托）https://localaimaster.com/blog/elevenlabs-open-source-alternatives
30. 独立英文报道 https://www.oflight.co.jp/en/columns/voicestudio-local-elevenlabs-alternative-2026
31. 报道面（中文/聚合）https://www.remio.ai/post/debpalash-voicestudio-hit-github-trending-but-local-voice-ai-still-has-to-prove
32. 第三方报道（市场定位）https://aitoolly.com/ai-news/article/2026-09-29-voicestudio-emerges-as-open-source-local-elevenlabs-alternative-supporting-646-languages-for-audio-p
33. GitHub Trending 周榜 https://github.com/trending?since=weekly
34. 同名第三方站（无关联声明）https://voicestudio.cc/en/
35. 同名第三方站（页脚自称非官方）https://voicestudio.lol/
36. 第三方风险提示（克隆滥用）https://freeai.help/blog/voicestudio-open-source-local-elevenlabs-alternative-en528316
37. 仓库根 LICENSE-NOTICE.md https://raw.githubusercontent.com/debpalash/VoiceStudio/main/LICENSE-NOTICE.md
38. docs/engine-acceptance.md https://raw.githubusercontent.com/debpalash/VoiceStudio/main/docs/engine-acceptance.md

取证档案：`research/evidence/voicestudio/`（**135 个文件 / 61 条唯一来源 URL**，manifest 带 sha256 与通道标注）。

## 勘误与可撤回

- 本文任一判断被一手源推翻，请在来信指出处；核实后在本文对应句子后加「（勘误 <日期>）」并保留原句划线，**不删旧数字、不改历史口径**。
- 已知存疑项：ElevenLabs「74 语言」（官方只说 70+）、Pro checkout 端到端可用性、克隆域名是否仿冒、amuletvoice 的 $0.01/分钟成本自述、Chatterbox 63.75% 盲测（出品方委托）。这些点**未当作事实使用**。
