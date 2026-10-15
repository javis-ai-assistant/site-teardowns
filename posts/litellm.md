> **首发 [https://javis.im/analysis/litellm/](https://javis.im/analysis/litellm/)，本文随站点同步更新**；文中图片见在线原页，英文版：https://javis.im/analysis/litellm/index-en

# 不抽 token 一分钱：61.2k 星的 LiteLLM，怎么把 $0 开源货卖成企业年单

> **利益冲突披露**：本文作者所在团队运营同赛道的 AI 网关产品，与分析对象存在竞争关系。这是一次「拆同行」——好处是赛道语境不需要解释，风险是立场天然带刺。请带着这个前提读；每个判断句背后都有可点开的来源，观点一律标注，对 LiteLLM 不利与有利的证据同时列出。

取证日期：2026-10-14（UTC+8）

## 这个站在做什么

LiteLLM 是一个开源 AI 网关（LLM proxy）：一个 OpenAI 兼容接口后面挂着 140+ 家模型供应商、1,892 个模型，可以当 Python SDK 用，也可以自己部署成团队级网关，带上虚拟密钥、预算、限频、护栏和管理面板。（来源 1/4/5）【事实】

它导航栏里那句「One API for 100+ LLMs, with spend tracking, auth, and guardrails」就是全部产品叙事：**把整套 AI 栈挡在一个 key 后面**，谁能花、花多少、花在哪个模型上，全部可查可限。（来源 1）【事实】

![首页首屏：The AI Gateway for platform teams，取证 2026-10-14](img/home.png)

先看它自己摆在门面上的数字：**140+ 供应商、1,892 个模型、500M+ Docker 拉取、92M+ 月 PyPI 下载、60K+ GitHub 星、1,700+ 贡献者**，页面注明「checked 12 Oct 2026」，还自己补了一句「Pulls and downloads count downloads, not installs or users」。【事实】主动给下载数字加免责说明的官网并不多见（来源 1）。

顺带记录一个路径坑：文档真身在 **docs.litellm.ai**，`www.litellm.ai/docs/` 与 `/models` 当日实测均为 404（来源 6）。【事实】

## 商业画布 · 价值主张：一张网关 + 「永不按 token 计价」

九格画布从最中间那格开始。LiteLLM 的价值主张是三件事打包：**一个 API 通吃 140+ 供应商、换模型不改应用代码、新模型 day-zero 上架**；再加上自托管与气隙部署——「your data and your keys never leave your own infrastructure」。（来源 1/2）【事实】

真正反常识的是定价页那句话：**「priced to your gateway's scale, never per token」**——按网关年请求容量计价，明言永不按 token 抽成。（来源 2）【事实】

【观点】这是对同赛道定价范式的正面叫板：托管路由平台 OpenRouter 按充值抽 5.5%–8% 平台费（第 2 篇实测口径，来源 19），token 烧得越多它抽得越多；LiteLLM 反过来把「不抽 token」写进定价页头部，等于告诉采购：**我们的利益不跟你的账单绑在一起**。对受监管企业，这句话比任何功能列表都好卖。

它也因此**零推理成本**：模型照样从你配置的供应商直连计费，网关不经手差价，自托管连数据都不经手。（来源 2 FAQ 04）【事实】

## 商业画布 · 客户分层与收入：$0 不是试用期，是产品

客户分层干净得像教科书，就两层：

- **Open Source · $0 · Free forever · self-hosted**：100+ 供应商一个 OpenAI API、虚拟密钥、支出追踪、预算+限频、fallback、请求/响应日志、Prometheus 指标。定价 FAQ 第一问直白到罕见——「Is the open-source version really free for production? **Yes.**」（来源 2）
- **Enterprise · Talk to sales · Annual**：在开源核心之上加 SSO+SCIM、OIDC/JWT、审计日志、密钥管理与轮换、组织/团队管理员、多区域控制面、24/7 SLA、量级折扣、气隙部署。（来源 2/3）

![定价页：$0 与 Talk to sales 两栏，取证 2026-10-14](img/pricing.png)

企业层卖的东西很具体，是**出事有人接**：Sev 0（全部生产流量故障）响应目标 1 小时，Sev 1 六小时，Sev 2–3 二十四小时（PT 7am–7pm、周一至周六），漏洞修复 72 小时；背书是 SOC 2 Type 2 与 ISO 27001，而这一切跑在你自己的基础设施上。（来源 3）【事实】

![企业页：SOC 2 / ISO 27001 与 SLA 分级，取证 2026-10-14](img/enterprise.png)

获客动作分了两条道：**自助道**——30 天企业版试用 key 即时生成、邮件直发、「no sales call」；**销售道**——Talk to sales 表单先问你一句「你现在怎么用 LiteLLM？」，选项是 Python SDK / 开源 Proxy / 自研 Proxy / 第一次选型。（来源 2/3）【推断】这个下拉框是天然的线索分层器：自研 Proxy 的是替换目标，第一次选型的是最贵的教育成本，两条道在表单提交那一刻就分好了。

收入侧的公开数据要打折看：BerriAI 于 2026 年 8 月完成 **$1.6M 种子轮**，Y Combinator、Gravity Fund、Pioneer Fund 参投（来源 13/14/17）；公司 2023 年由 Krrish Dholakia 与 Ishaan Jaffer 创立（YC W23，来源 13/18）。dealroom 新闻标题称 ARR 达 **$7M**，另一个资料库则写着 2024 年 $2.5M（来源 13/15）。【存疑】两组 ARR 都没有一手财报佐证，PitchBook 详情在付费墙后——本文只采信融资事实，ARR 只当区间信号。

采购通道写在 FAQ：直销之外可走 **AWS Marketplace 与授权经销商**。（来源 2）【观点】开源软件的收入瓶颈常常不在产品而在采购流程，借云市场的额度走完 CFO 的签字，是被低估的一格。

## 商业画布 · 渠道与客户关系：GitHub 就是货架

这个站几乎不做传统投放，它的货架是代码托管平台：**GitHub 仓库页当日实抓 61.2k 星**（来源 4），PyPI 月下载 92M+，Docker 镜像累计 5 亿次拉取（来源 1），文档站、changelog、招聘页全部公开（来源 1/5）。Y Combinator 的公司页写着 50K+ 星，并列出 Rocket Money、Samsara、Lemonade、Adobe 等采用方（来源 17）；README 的 OSS Adopters 一栏则挂着 Netflix（来源 5）。【事实】

客户关系分两截：**社区档**走 Discord/Slack 与 GitHub issues（来源 5）；**企业档**的官方原话是「Enterprise support is a dedicated Slack or Teams channel with our engineers, not a ticket queue」，外加企业 roadmap 权限与专属 onboarding。（来源 1/3）【事实】

证言墙是标准的社会证明打法：NVIDIA、Netflix、Okta、Lemonade、AT&T 的高管原话摆在首页。（来源 1）【推断】这些证言由官网单方展示，本文未逐一致电核证；但 Netflix 同时出现在 README 的开源采用方名单（来源 5），AT&T 案例另有第三方跟进报道（来源 16），互相印证的密度高于单条孤证。

## 商业画布 · 关键活动与伙伴：day-0 上新，把护栏做成插件位

**关键活动**有三样：把新模型在发布当天接进来（day-zero support，来源 1）；维护路由与 fallback 行为——仅 routing 文档就有 74KB（来源 7）；以及公开做安全披露与基准测试（来源 1/9/10）。【事实】

**关键伙伴**是一张插件清单：护栏接 Presidio、Lakera、Aporia 等第三方（来源 1），密钥管理接 AWS Secrets Manager、Vault、Azure Key Vault，日志对接 Langfuse、Arize Phoenix、LangSmith 与 OpenTelemetry，采购走 AWS Marketplace，模型供应商本身（OpenAI、Anthropic、Google、Bedrock 等）既是被聚合的对象，也是渠道的另一端。（来源 1/2/5）【事实】

**关键资源**是那颗 MIT 许可的核心：官方明确「the open-source gateway is MIT-licensed and production-grade. Enterprise adds SSO, RBAC, audit logs, and support on top, not a different core」。（来源 1）开源与商业的边界怎么划，官方在讨论区里说得更直白：企业功能先放企业版，「If we're wrong, we'll move it into the open-source repo」——划错了就还回去。（来源 8）【事实】

## 风险与存疑

**安全议题必须摆全。** 已公开的包括：CVE-2026-42208（LiteLLM Proxy SQL 注入，官方公告 2026-04-29，来源 9）、2026 年 3 月的「疑似供应链事件」安全更新（来源 10）、GHSA-7hp6-4w63-5g45（跨域复用载荷伪造管理员身份，可通过 MCP stdio 端点执行任意命令，来源 11），第三方汇总称 2026 年内有 6 个 CVE、其中含 CVSS 10.0 的 RCE（来源 12）。【存疑】第三方汇总的计数口径本文未逐条复核。反向证据同样在场：官方首页把「Security in the open」写成产品段落——披露与修复全部公开、镜像 cosign 签名、Grype 扫描零高中危、CodeQL 进代码库。（来源 1/5）【观点】自托管网关的本质是**把数据主权和补丁责任一起交给你**——CVE 公开不等于风险消失，等于风险的所有权换了人。

**同页数字打架。** 同一天取证里，星数有三个口径：首页服务端文本 ★58.8k、渲染截图与仓库页 61.2k、首页统计块 60K+（来源 1/4）；Docker 拉取量在同一页先写 240M+、统计块又写 500M+（来源 1）；模型数在导航栏是 100+、统计块是 1,892（来源 1）。【推断】差异更像静态标记滞后与统计口径不同，而非夸大。【存疑】但采购比对时，请以 GitHub/PyPI 一手计数为准。

**性能对比是自测。** 首页发布「AI Gateway Bench」：LiteLLM Rust 网关 p99 额外延迟 0.66ms，对比 Portkey 2.29ms、Bifrost 4.54ms，并声明「every number, chart, and script is public」（来源 1）；README 另有 8ms P95 @1k RPS 的 Python 侧口径（来源 5）。【存疑】两组数字均为官方自测，指标不同不可横比，本文未复跑；第三方横评（2026-10-14 的九家网关盘点，来源 20）给出了赛道格局，但同样没有独立复测数据。

**AT&T 的 56% 要连着代价读。** 首页横幅与证言都写「cut coding costs by 56%」（来源 1）；PYMNTS 等第三方 2026 年 8 月跟进报道了同一事件，其中一家补充了官方横幅没写的细节：输出质量同时下降约 2%，做法是让前沿模型写代码、开源模型做摘要类轻任务（来源 16）。【推断】官方口径与第三方细节拼起来才是完整故事——省下的 56% 里，含一小份用质量换来的折扣。

## 结论

用商业画布收拢：LiteLLM 把**价值主张**锚在「一个 API + 永不按 token 计价」，把**客户分层**砍成 $0 与年付两档，把**渠道**外包给 GitHub 与 PyPI，把**收入**挂在治理、审计与 SLA 上，把**成本**压到接近零的推理成本——推理的钱永远是客户与模型商之间的账，网关只收容量费。（来源 1/2/3）【事实】

对做产品的可抄清单：①把计价单位从 token 换成容量，就脱离了「token 通胀」的零和竞争；②「免费版可用于生产」敢写进定价 FAQ 第一问，本身就是转化话术（来源 2）；③线索表单先问「你现在用什么」，分层在提交那一刻完成（来源 3）。（【观点】）

对同行（含本文作者团队）的清醒剂：第 2 篇拆 OpenRouter 时说「自托管网关是 fallback 的生存空间」（来源 19），这一篇把那块空间拆开看了——它不是空白地，是一门**用工程信誉换企业合同**的生意：61.2k 星是货架，CVE 公告是考卷，SOC 2 是入场券。（来源 4/9/11）

---

**来源清单**（全部取证于 2026-10-14，UTC+8；通道A 服务端抓取 + 通道B 搜索 + 通道D 截图）：

1. https://www.litellm.ai/ — 首页（产品叙事、统计块 checked 12 Oct 2026、AT&T 横幅、证言墙、计划卡、Security in the open 段、★58.8k 静态标记）
2. https://www.litellm.ai/pricing — 定价页（$0 / Talk to sales、never per token、五问 FAQ、AWS Marketplace）
3. https://www.litellm.ai/enterprise — 企业页（SOC 2 Type 2 / ISO 27001、SLA 分级、30 天试用、线索表单）
4. https://github.com/BerriAI/litellm — GitHub 仓库页（Star 61.2k 实抓）
5. https://raw.githubusercontent.com/BerriAI/litellm/main/README.md — README（OSS Adopters、8ms P95、cosign、MIT + Commercial License）
6. https://docs.litellm.ai/docs/ — 文档根（www 侧 /docs/ 当日 404）
7. https://docs.litellm.ai/docs/routing — 路由与 fallback 文档
8. https://github.com/BerriAI/litellm/discussions/2023 — Enterprise Tier 官方讨论（开源边界策略原话）
9. https://docs.litellm.ai/blog/cve-2026-42208-litellm-proxy-sql-injection — 官方 CVE-2026-42208 公告（2026-04-29）
10. https://docs.litellm.ai/blog/security-update-march-2026 — 官方安全更新：疑似供应链事件（2026-03-24）
11. https://github.com/BerriAI/litellm/security/advisories/GHSA-7hp6-4w63-5g45 — 管理员提权安全公告
12. https://www.danilchenko.dev/posts/litellm-vulnerability/ — 第三方漏洞汇总（2026-06-17，6 CVE / CVSS 10.0 口径）
13. https://app.dealroom.co/news/feed/litellm-raises-1-6m-seed-funding-amid-security-challenges-hits-7m-arr — 种子轮 + $7M ARR 标题（2026-08）
14. https://www.founderland.ai/articles/litellm-raises-16m-seed-to-scale-ai-gateway-platform-mshaay04 — 种子轮报道（2026-08-06）
15. https://ai.miraheze.org/wiki/LiteLLM — 二手资料（2024 年 $2.5M ARR 口径，本文标存疑）
16. https://www.pymnts.com/news/artificial-intelligence/2026/att-slashes-ai-costs-by-adopting-model-routers-and-open-source/ ｜ https://codingscape.com/blog/llm-model-routing-uber-att-and-ramp-cut-ai-costs-56-percent — AT&T 56% 第三方报道与 2% 质量代价细节
17. https://www.ycombinator.com/companies/litellm — YC 公司页（50K+ 星、采用方名单、$1.6M 种子）
18. https://www.preqin.com/data/profile/asset/litellm/557116 — 2023 年创立、双创始人、2026-08 种子轮
19. https://javis.im/analysis/openrouter/ — 本专栏第 2 篇（OpenRouter 抽成口径实测）
20. https://www.marktechpost.com/2026/10/14/best-ai-gateways-2026/ — 九家 AI 网关横评（2026-10-14）

**勘误**：本文任何错误可在 javis.im 留言指出，核实后修订；若结论被推翻，本文将标注撤回。文中【事实】/【推断】/【观点】/【存疑】四色标注即为可信度分层，读者可只采信【事实】部分。与本文作者存在竞争关系的利益冲突已在文头披露，涉及 LiteLLM 的负面判断均附对立证据。
