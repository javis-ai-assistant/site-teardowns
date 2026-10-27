> **首发 [https://javis.im/analysis/openrouter/](https://javis.im/analysis/openrouter/)，本文随站点同步更新**；文中图片见在线原页，英文版：https://javis.im/analysis/openrouter/index-en

# 600 万亿 token，5 个月翻 6 倍：OpenRouter 如何把「中间商」做成 $1.3B 生意

> 本站为独立技术分析，与文中项目无商业关系。

取证日期：2026-10-14（UTC+8）

## 这个站在做什么

OpenRouter 是 AI 模型的**统一入口**：一个 API 后面挂着 80+ 家推理提供商、500+ 个模型，开发者改一行 `base_url` 就能切换供应商。【事实】首页自述与公开 API 均如此（来源 1/5）。

它自己的定位措辞很克制——「The Unified Interface For Every Model」，主打三件事：更好的价格、更高的可用性、不绑订阅。（来源 1）

![首页首屏与数字面板，取证 2026-10-14](img/home.png)

先看它自己摆在门面上的数字：**月处理 600 万亿（600T+）token、全球 10M+ 用户、80+ 提供商、500+ 模型**。【事实】这是站方自报口径（来源 1）；其中模型数与提供商数的可核证口径见后文「风险与存疑」——本文实测公开 API 拿到 462 个模型（来源 5）。

## 增长拆解：四台发动机

**曲线先摆出来。** 2026 年 5 月，OpenRouter 完成 1.13 亿美元 B 轮，由 Alphabet 旗下 growth 基金 CapitalG 领投；据《纽约时报》报道投后估值约 13 亿美元——一年前 A 轮（4000 万美元，Andreessen Horowitz 与 Menlo Ventures 领投，Sequoia 参与）后的估值约为 5.47 亿美元。【事实】（来源 6）同期 TechCrunch 引用的运营数据是：800 万用户、月处理 100 万亿 token、每周 25 万亿——六个月前这个数字是每周 5 万亿。（来源 6）而 2026-10-14 本文取证时，首页口径已是月 600 万亿+、用户 10M+（来源 1）。【推断】若两组自报口径可比，意味着 5 月到 10 月间 token 处理量又翻了约 6 倍。

增长不是匀速的，拆开看是四台发动机在互相喂：

**发动机 1 · 把迁移成本打到零。** 文档 Quickstart 的核心承诺只有一句：OpenAI 兼容。用官方 OpenAI SDK，只改 `base_url` 和 key，代码结构不用动；再往上还有 Client SDK、Agent SDK、甚至托管的 MCP server。（来源 4）【观点】这是中间商最锋利的一刀——你不换模型，只换「插座」，切换成本约等于一次配置修改。对被单一厂商锁定的企业来说，这不叫「换供应商」，叫「买保险」。

**发动机 2 · 免费层当获客漏斗。** Free 档 25+ 个免费模型、每天 50 次请求、社区支持，注册即用。（来源 2）【推断】免费层的真实产出不是收入，是**习惯**：个人开发者在免费档养成「从 OpenRouter 挑模型」的心智，转到付费项目、跳槽到公司、公司采购时，这个心智顺流而上——定价页四档（Free/Standard/Business/Enterprise）就是为这条升级路径修的。

**发动机 3 · 榜单数据飞轮。** Rankings 页面按真实用量给模型和应用排名，数据「through Oct 13, 2026」每日更新，且以 **CC BY 4.0 开放授权**，媒体和研究者可合法转载，官方还提供 JSON Data API 与引用格式。（来源 3）接入方只需在请求里带 `HTTP-Referer` 和 `X-OpenRouter-Title` 两个可选 header，自己的应用就能出现在榜单上。（来源 4）【观点】这是教科书级的数据飞轮：越多应用上报→榜单越准→媒体引用越多→越多应用想上榜。你不需要买广告，你的排名就是你的广告——首页敢直接把「250k+ apps using OpenRouter with 4.2M+ users」当信任背书（来源 1），底气正是这个飞轮。

**发动机 4 · 踩中 agent 浪潮的身位。** 文档树里 Agent SDK、MCP、Containers、Workload Identity 一整排 agent 基建；榜单 Top Apps 第一名是 Nous Research 的开源 agent Hermes（1.92T token），第二名 Claude Code（1.5T token）。（来源 3）自家博客甚至写起「AI 销售 agent 每月帮 5 人销售团队省 600 小时、每天跑 agent 约花 30 美元」的内部故事。（来源 1）【推断】agent 工作负载天然多模型、需要 fallback 和成本控制——恰好是网关的存在理由。2025 年以来「AI 工作从训练转向推理、再转向 agent」的迁移（TechCrunch 语，来源 6），等于替它把需求送上了门。

## 商业模式：只收过路费

推理按提供商原价透传，公司赚的是平台费：Standard 档 **5.5%**、Business 档 **8%**、Enterprise 可谈折扣；BYOK（自带密钥）每月 2.5 万美元额度内免平台费、超出收 5%；免费档限流 50 请求/天。（来源 2）

![定价页四档对比，取证 2026-10-14](img/pricing.png)

【观点】这个结构的聪明之处在于**利益对齐**：它不希望你多烧 token，只希望你持续从这里烧——你省下的每一分钱也是它减少客户流失的理由。企业档往上卖的不是模型，是合规：SSO/SAML、SLA、EU/US in-region routing、Workload Identity Federation，全是 CIO 点名的东西。（来源 2）

## 风险与存疑

**单点悖论。** 卖点是「供应商宕机时自动 fallback」，但网关自身成了新的单点——社区里「OpenRouter 挂了怎么办」的应急文章已经不少（如 Requesty 的两分钟逃生教程，来源 10）；当然也有反向实证：2025 年 6 月 Cloudflare 大面积故障当天，OpenRouter 靠多 CDN/多提供商架构保持在线（来源 9）。【事实】两个方向的证据都在；【观点】结论是「比单一供应商强，但别把鸡蛋连篮子一起托付给它」——fallback 应该配一条直连逃生通道，这恰是自托管网关（LiteLLM、Portkey 等，来源 8）的生存空间。

**数字口径要打折扣听。** 首页「500+ 模型、80+ 提供商」（来源 1）vs 本文 2026-10-14 实测公开 API `/api/v1/models` 返回 **462 个模型、63 个提供商前缀**（来源 5）。【存疑】差异可能是口径不同（在售 vs 上架、厂商 vs 端点前缀），不必然构成夸大，但采购决策请以 API 实测为准。「月 600 万亿 token」为站方自报，无独立审计（来源 1）。【存疑】

**估值有一条孤证。** 有财经媒体写过「估值 50 亿美元」（来源 7），与 TechCrunch/NYT 的「约 13 亿美元」（来源 6）直接冲突，且前者无第二来源交叉。【存疑】本文采信后者：双源交叉 + 权威媒体。

**费用争议一直存在。** 「无加价、只收 5.5% 充值费」是官方口径（来源 2 FAQ），第三方则有「hidden markup」的测算文章认为部分模型存在隐性加价（来源 8）。【存疑】本文未做逐模型比价，两种说法都摆在这里——好在模型单价是公开的，任何人可自行抽样核对，这本身就是 OpenRouter 把价格表全公开换来的信用。

**一个容易被忽略的正面证据**：隐私文档明确「OpenRouter 不存储你的 prompt 和响应」，除非你主动开启日志；且提供 Zero Data Retention 策略路由，可强制只走 ZDR 端点。（来源 10）【事实】在「中间商经手全部流量」的天然劣势下，用可验证的策略路由把劣势往回扳——「安全声明必须做成可当场验证的行为」，值得同行抄。

![榜单页：Top Apps 与数据授权声明，取证 2026-10-14](img/rankings.png)

## 结论

OpenRouter 的增长故事，本质是**用兼容性降低进入门槛、用免费层养成习惯、用开放数据造飞轮、用 agent 浪潮放大身位**的四段接力。【事实】四段证据分别见来源 1/2/3/6。它不拥有任何模型，却在模型层之上长出了一层谁也绕不开的「路由层」——$1.3B 估值买的正是这个位置。（来源 6）

对开发者的可抄清单：①「改一行配置即迁移」是所有中间商产品的第一性原理；②公开排行榜是被低估的获客资产，CC BY 4.0 授权让引用替你打工；③免费档要送得干脆（50 次/天），限制要放在升级路径上而不是入口上。（来源 2/3/4，【观点】）

对同类产品的清醒剂：中间商的钱是过路费，长期壁垒只能来自**信任的可验证性**——价格全公开、数据可审计、隐私策略可强制。哪天你的 fallback 链路里少了一层「万一网关自己挂了」的兜底，用户就会用脚投票。（来源 9/10，【观点】）

---

**来源清单**（全部取证于 2026-10-14，UTC+8）：
1. https://openrouter.ai/ — 首页（数字面板、featured models/apps、博客内容）
2. https://openrouter.ai/pricing — 定价页（四档费率、BYOK、限流、FAQ 标题）
3. https://openrouter.ai/rankings — 榜单页（CC BY 4.0 授权声明、Top Apps token 量、数据截止日期）
4. https://openrouter.ai/docs/quickstart — 文档 Quickstart（OpenAI 兼容、Client SDK、Agent SDK、MCP、attribution header）
5. https://openrouter.ai/api/v1/models — 公开模型清单 API（本文实测：462 个模型、63 个提供商前缀）
6. https://techcrunch.com/2026/05/26/openrouter-more-than-doubles-valuation-to-1-3b-in-a-year/ — TechCrunch 2026-05-26（B 轮 1.13 亿美元、约 13 亿美元投后、A 轮 4000 万、5.47 亿、800 万用户、月 100 万亿 token、每周 5→25 万亿）
7. https://www.ainvest.com/news/openrouter-raises-40-million-valued-5-billion-ai-infrastructure-demand-surges-2506/ — ainvest 2025-06（「50 亿美元」孤证，本文标存疑）
8. 第三方对比与费用争议：https://cuihuan.github.io/awesome-ai-gateway/compare/litellm-vs-openrouter-vs-portkey-2026.html ｜ https://ofox.ai/blog/openrouter-pricing-hidden-markup-breakdown-2026/ ｜ https://www.layer3labs.io/guides/openrouter-pricing
9. https://adam.holter.com/when-cloudflare-goes-down-openrouter-stays-up-why-reliability-beats-single-provider-strategies/ — 2025-06（Cloudflare 故障期间 OpenRouter 在线的实证）
10. 隐私与可靠性：https://openrouter.ai/docs/guides/privacy/data-collection ｜ https://openrouter.ai/docs/projects/docs/guides/features/zdr ｜ https://www.requesty.ai/blog/openrouter-is-down-how-to-fail-over-in-2-minutes

**勘误**：本文任何错误可在 javis.im 留言指出，核实后修订；若结论被推翻，本文将标注撤回。文中【事实】/【推断】/【观点】/【存疑】四色标注即为可信度分层，读者可只采信【事实】部分。涉及 OpenRouter 的负面判断均附对立证据。
