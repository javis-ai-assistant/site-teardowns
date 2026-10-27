> **首发 [https://javis.im/analysis/plausible/](https://javis.im/analysis/plausible/)，本文随站点同步更新**；文中图片见在线原页，英文版：https://javis.im/analysis/plausible/index-en

# 773 字节脚本、24 小时销毁的盐、10 个人服务 21,000 家付费：审计 Plausible 的「免横幅」承诺

> 773 字节的脚本、24 小时就销毁的盐、10 个人撑着 21,000 家付费：本文把 Plausible 的「免横幅」承诺拆成四层审计——脚本、数据管道、法律距离与生意底稿，逐层给出成立与否。

取证日期：2026-10-19（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 20 · 可验证性 19 · 独特性 18 · 受众相关度 18 · 多维空间 19 = **94/100**（≥60 才动笔；素材库预分 80，框架卡 = 风险审计）。

## 这个站在做什么

取证日的官网首屏是一串卖点清单：**Simple analytics dashboard / Lightweight script / Privacy-friendly analytics（No cookie banner required）/ Open source / EU-hosted**，其中隐私一条的原文是「Built with visitor privacy in mind. No cookie banner required.」（来源 1）【事实】官方文档首页把承诺压成一句话：「**No cookies, no personal data collection, no consent banners required.** One lightweight script, one easy-to-read dashboard. Running since 2018 and used by 21,000+ teams.」（来源 6）【事实】

![Plausible 官网首页首屏：五大卖点 + 定价锚点，取证 2026-10-19](img/home.png)

可达性一项先交代：素材库登记的 `https://plausible.io` 取证日 200；有意思的细节是单独请求 `/pricing` 会返回 404——它的定价并不在独立页面，导航栏的 Pricing 链到首页锚点 `#pricing`，三档价格写在首页长页里（来源 1）【事实】。docs、about、data-policy、compliance、changelog、status、vs 系列页、GitHub API、HN 五个帖子、欧盟委员会新闻与法务分析全部 200，无登录墙、无挑战页（来源 1-33）【事实】。

这篇的分析框架卡是**风险审计**：不评「好不好用」，只做四层拆解——**脚本抓下来数一遍、数据管道逐条对账、法律距离量一遍、商业底稿翻一遍**，最后给出风险与存疑清单。

## 风险审计 · 第一层：把脚本抓下来数

审计一个统计工具，最硬的证据是它的脚本本身。取证日实抓 `plausible.io/js/script.js`，源码尾部写着 `window.plausible.v=36`（版本 36），文件正文 **1,644 字节**，gzip-9 压缩后 **773 字节**（本文本地实测）；把源码全文逐字扫描，`document.cookie`、`localStorage`、`sessionStorage`、像素对象 `Image` 的出现次数**全部为 0**（来源 4）【事实】

也就是说，「无 Cookie」这条声明在**脚本层面成立**——它不是营销措辞，而是源码里根本不存在写 Cookie 的调用。（来源 4）【事实】

体积这一侧给出三个口径，全部并列不连线：①本文实测 gzip 773 字节；②官方 vs-GA 页面自报「The Google Analytics tracking script is 135KB gzipped. Plausible's is 2.5KB, which is 54 times smaller」（来源 5）【存疑】——135KB 与 2.5KB 都是官方口径，本文未做第三方复核，且 2.5KB 与本文 773 字节的差异大概率来自压缩与否、是否含扩展脚本的口径不同；③Show HN 原帖里创始人 2020 年自己写的「The script is lightweight at **0.7 KB**」（来源 19）【事实】。

【推断】把三个口径叠起来看，真正稳定的信号是**六年没变的量级**：2020 年 0.7KB，2026 年还是亚 KB 级。对一个靠「不影响页面速度」当卖点的产品，脚本体积是它必须守住的承诺，涨上去就等于打脸。

![GitHub 仓库页实抓：29,551 星 / AGPL-3.0 / 公开 issue，取证 2026-10-19](img/github.png)

体积守得住，不代表数据管道就没别的东西——第二层才是这次审计的重点。

## 风险审计 · 第二层：数据管道里到底存了什么

官方 data-policy 页把收集范围列成表格，九类数据逐条给例子与用途：**页面 URL（只存主机名与路径）**、来源与战役参数、浏览器/操作系统/设备类型（由 User-Agent 派生）、**国家/地区/城市（由 IP 派生）**、engagement 度量（滚动深度、停留时长）、页面浏览与自定义事件、自定义属性、收入字段；原话是「We do not store raw User-Agent strings or IP addresses」，查询参数除受支持的战役参数外一律丢弃（来源 3）【事实】

唯一访客的算法写得像代码注释一样直白：

`hash(daily_salt + website_domain + ip_address + user_agent)`——**盐每 24 小时轮换并删除**，原始 IP 与 UA 从不落库，跨日无法把同一个人接起来（来源 3）【事实】

![Plausible data-policy 页：收集清单与去标识化算法，取证 2026-10-19](img/datapolicy.png)

对账的关键在于**时间一致性**：同一个公式在 2020-10-23 的 HN 论战帖里由创始人以原帖回复的形式给出过一遍（来源 20）【事实】，与取证日 data-policy 上的版本逐字相同。（来源 3/20）【事实】

【推断】六年里产品加了 engagement 追踪、漏斗、用户旅程（来源 12）【事实】，但**去标识化算法没有漂移**——对开源项目来说，这种「承诺与代码同龄」的连续性是最难伪造的可信度资产；闭源厂商给出同样的声明，读者只能选择相信。

这一层的边界也要说清楚：官方在 when-not 页自己写明，因为不生成跨日持久标识，**产品没有 new-vs-returning（新老访客）分段**，「这是设计，不是待修的缺陷」（来源 11）【事实】。同一份 data-policy 里，城市级地理定位来自 IP 派生，尽管 raw IP 不落库，**高敏感站点仍应把它算进自己的威胁模型**（来源 3）【推断】。

## 风险审计 · 第三层：法律距离——「免横幅」是主张，判决还没来

Plausible 给出的合规论证有三层：技术层是 data-policy 的算法披露（来源 3）【事实】；合规层是 compliance 页的原话——「An independent review by a data protection lawyer confirms Plausible requires no cookie consent and falls outside the scope of GDPR personal data processing」，并提供全文法律评估链接，DPA（数据处理协议）对所有客户自动生效（来源 9）【事实】；审计层是开源代码本身（来源 4）【事实】。

反方意见在 HN 有一手记录：2020-10-23《**Plausible Analytics Isn't GDPR Compliant**》**54 points / 75 comments**，争论焦点集中在「IP 地址算不算个人数据」——有人搬出 Breyer 判例，有人引用 GDPR「匿名化必须不可逆」的条文，也有人认为「GDPR 根本不在乎你存不存储，处理即触发」；两位创始人 ukutaht 与 markosaric 亲自在帖内逐条回复（来源 20）【事实】。2024-07-08《Plausible Analytics: GDPR Compliance w/o Cookie Consent Banner》**145 points / 137 comments**、2024-07-10《Plausible Community Edition》**125 points / 103 comments**，说明这条线的公共讨论六年没断（来源 21/22）【事实】。

![HN 一手论战帖《Plausible Analytics Isn't GDPR Compliant》（2020-10-23，54 points），取证 2026-10-19](img/hn.png)

监管这一侧的距离必须量出来：

- 奥地利 DPA 2022-01-13 依据 noyb 的模型案裁定，某奥地利网站使用 Google Analytics 构成向美国的非法数据传输（来源 25）【事实】；2022-04-22 的第二份决定（D155.026）驳回了「风险自评估仍可依赖」的路径（来源 26）【事实】——**GA 的问题在传输，不在 Cookie 数量**，这正是隐私统计赛道的法律起点。
- EDPB《Guidelines 03/2022》终版 2023-11-14 通过，对 ePrivacy 指令 Art 5(3) 采**技术中立的广义解释**：读取设备信息即触发规则，不只限于传统 Cookie（来源 27）【事实】。
- 风向标在 2025 年转向：欧盟 ePrivacy 条例的撤回 2025-10-06 刊登于欧盟官方公报（OJ C, C/2025/5423）（来源 30）【事实】；**Digital Omnibus（COM(2025) 837）于 2025-11-19 提出**，把 cookie 规则并入 GDPR 新增 Art 88a/88b，并**首次列出「第一方受众测量」为免同意场景**（限服务提供方自身服务、聚合数据、仅供内部使用），同时给出「拒绝后 6 个月不得重复索要」等条款（来源 28/29）【事实】。该豁免的口径被法务分析评价为「narrowly framed」，且 **Art 88a 要在法案生效 6 个月后才适用、88b 为 24 个月**——截至取证日，它仍是**提交欧洲议会与理事会审议中的提案**（来源 28/29）【事实】。

【推断】把两头放一起：Plausible 押的不是某条现成法条，而是**工程结构 + 监管口径**——不产生跨日标识 → 主张不构成个人数据处理 → 若 Art 88a 的第一方统计豁免按窄口径落地，它是首批受益者；若按 EDPB 的广义路线收紧，「免横幅」在部分成员国就有被推翻的风险。**到取证日为止，它拿到的是律师意见与合规自述，不是任何一国监管或法院的裁决**——这一层是本文四项审计里唯一「未闭环」的。（来源 9/27/28/29）【事实】

## 底稿 · 第四层：10 个人的零投资生意

about 页的披露密度过高，值得整段引用：**21,000+ 付费订阅用户**，客户名单包括 Stripe、Mozilla、Basecamp、Ghost、Hugging Face、Home Assistant、MongoDB、Python 软件基金会与 Harvard University（来源 2）【事实】；Uku Täht 于 2018 年 12 月独自创立，2019 年开始收费，2020 年 Marko Saric 加入做市场；**2022 年跨过 $1M ARR**；**自筹资金且盈利，没有外部投资人，没有出售计划，订阅费是唯一收入，从未在广告、联盟或付费背书上花过一分钱**；**团队 10 人**（来源 2）【事实】。

定价锚在首页：**Starter $9 / Growth $14 / Business $19 每月**（年付口径，页面提供年/月切换），Enterprise 计价（来源 1）【事实】；计费口径是「所有站点的 pageviews + 自定义事件总量」，15% 折扣只给非营利、教育与开源项目的 Business 年付，试用 30 天免信用卡（来源 8）【事实】。

收入没有官方数字：Latka 记「Annual revenue **$3.1M** Oct 2024 EST」，而最后一次本人出镜实报是 2021-05 的 **$279.6K**（约 $23,300 MRR）（来源 31）【存疑】。做个量级核对：21,000 家 × 最低档 $9 × 12 个月 ≈ **$2.27M/年**，与 $3.1M 的估算同量级，说明第三方 EST 不算离谱——但这是本文的算术推断，不是任何一方的口径。（来源 2/31）【推断】

![首页定价锚点：Starter $9 / Growth $14 / Business $19，取证 2026-10-19](img/pricing.png)

## 生态位：星数排不到第一，付费订阅才是结构

同一日、同一口径的 GitHub API 实抓三个开源统计项目的星数：**umami 39,458 · plausible 29,551 · matomo 21,960**；fork 数则是 umami 8,343 · plausible 1,913 · matomo 2,918（来源 14/15/16）【事实】。**反常识点在这里：Plausible 在开源热度榜上只排中间档**——它 2018-12-04 建仓、AGPL-3.0、取证日仍有推送（来源 14）【事实】，但把它捧成「开源统计之王」的说法与数据不符。

产品节奏倒是在第一梯队：changelog 显示 2026 年内已发 Flexible funnels（09-23）、漏斗对比与收入（09-09）、图表注释（07-14）、AI Assistants 流量渠道（06-10）、User journeys（05-27）、严格顺序漏斗（04-14）等，2025 年补上 SSO（07-29）与 Managed Proxy（07-08）（来源 12）【事实】——10 个人的发布密度说明「小团队」不是开发能力的上限，而是产品边界的选择（来源 2/12）【推断】。

![官方 changelog：2026 年发布节奏，取证 2026-10-19](img/changelog.png)

社区线的数字同样一手：Show HN 2020-10-06 **351 points / 139 comments**，帖内自报「5,000+ sites、三个月 1.8 亿 page views」（来源 19）【事实】；《Lessons from building Plausible Analytics to $1.2M ARR in public》2023-03-12 **210 points / 76 comments**（来源 23）【事实】——三百万美元 ARR 的故事在 HN 是被追捧的样本，而它恰恰是「不融资也能活」叙事的一部分。

## 风险与存疑清单

- **数字全部自报**：21,000 付费、$1M ARR（2022）、10 人团队、客户名单均来自 about 页，无审计口径；收入第三方只有 EST（来源 2/31）【存疑】。
- **「免横幅」未闭环**：合规依据是律师评估 + 自述，Art 88a 仍是提案，成员国无统一裁决（来源 9/28/29）【事实】。
- **绕过广告拦截的路径**：docs 提供托管代理（Managed Proxy）与自定义域名方案，Enterprise 档含 managed proxy（来源 33/12）【事实】——【推断】这与「尊重访客选择」的叙事存在张力：拦截器是访客表达不同意的最后手段，代理路径改变了这场博弈。
- **敏感数据责任在配置方**：官方明说不适用 HIPAA、不提供 BAA，URL 里的病人编号/邮箱若不先过滤就会被记录（来源 11）【事实】。
- **分析能力有硬边界**：没有 new-vs-returning、没有会话回放、不是产品分析、托管版没有永久免费层——官方原话是「Google Analytics is free because Google monetizes the data. Plausible is not willing to do that」（来源 11）【事实】。
- **城市级定位来自 IP 派生**：raw IP 不落库，但地理精度到城市一级，高敏场景需自行评估（来源 3）【推断】。

![官方《When Plausible is not the right fit》自曝短板页，取证 2026-10-19](img/whennot.png)

## 结论

四层审计收成一句话：**脚本层通过**（1,644 字节原文、gzip 773 字节、源码零 Cookie API）、**管道层通过**（salt 24 小时销毁、六年公式未变）、**商业层自洽**（零外部投资、10 个人、21,000 家付费、$9 起步，估算与实报同量级）、**法律层未闭环**——「不需要 Cookie 横幅」目前是主张与律师意见，不是判决，而欧盟正在草案里替整个品类写的豁免条款（Art 88a）还没生效。（来源 2/3/4/9/28/31）【事实】

对做产品与内容的同行，三个可抄的动作：①**把方法论披露到公式级**——data-policy 直接写出 hash 公式与盐的生命周期，让审计者有东西可验（来源 3）【观点】；②**把短板写成页面**——《When Plausible is not the right fit》六条自曝不但没劝退客户，反而成为合规审查里的信任资产（来源 9/11）【观点】；③**口径纪律**——它家每个数字都能对上时间戳与出处，这在「隐私即卖点」的品类里是成本最低的可信度凭证。（来源 1/2/12）【观点】

一句清醒剂给包括本文作者在内的同行：**「no trackers」是比「合规统计」更极端的一档**——javis.im 页脚写 No trackers、连脚本都不加载，代价是拿不到任何行为数据；Plausible 选择了中间道路：可验证的最小收集 + 订阅制收入。两条路都成立，但别把「主张」当成「裁决」来讲。（来源 9/28/29）【观点】

---

**来源清单**（全部取证于 2026-10-19，UTC+8；通道A 服务端抓取 + 通道B 搜索交叉验证 + 通道D 截图 + 本地直连自测，取证档案 `research/evidence/plausible/` 112 文件 / 48 条唯一 URL，manifest 带 sha256）：

1. https://plausible.io/ — 官网首页（五大卖点、导航、定价锚点 Starter $9 / Growth $14 / Business $19）
2. https://plausible.io/about — 关于页（21,000+ 付费订阅、客户名单、2018-12 创立、2022 过 $1M ARR、自筹资金无投资人、团队 10 人、零广告投放）
3. https://plausible.io/data-policy — 数据政策（九类收集清单、hash(daily_salt+domain+ip+ua) 算法、盐 24h 轮换删除、不存 raw IP/UA、Estonia 实体）
4. https://plausible.io/js/script.js — 跟踪脚本源码（v36、1,644 字节原文 / 773 字节 gzip 本地实测、零 document.cookie/localStorage/sessionStorage/Image）
5. https://plausible.io/vs-google-analytics — 官方口径（GA 135KB gzipped vs Plausible 2.5KB＝54×；Basecamp 证言）
6. https://plausible.io/docs — 文档首页（No cookies, no personal data collection, no consent banners required；since 2018、21,000+ teams）
7. https://plausible.io/docs/plausible-script — 安装文档（片段插入 head、30 天试用免卡）
8. https://plausible.io/docs/subscription-plans — 订阅口径（pageviews+custom events 计费、15% 非营利/教育/OSS 折扣、Enterprise 非自助）
9. https://plausible.io/compliance — 合规页（独立数据保护律师评估、DPA 自动生效、Hugging Face/Harvard/苏格兰与威尔士政府审查通过）
10. https://plausible.io/eu-hosted-web-analytics — 欧盟托管（Hetzner 德国 Falkenstein 存访客数据、UpCloud 芬兰、Bunny 斯洛文尼亚，仅此三家且全欧盟企业）
11. https://plausible.io/when-not-to-use-plausible — 自曝短板页（六条不适合、无 new-vs-returning、非 HIPAA、托管版无永久免费层、GA 免费因 Google 变现数据）
12. https://plausible.io/changelog — 产品更新（2026-09-23 Flexible funnels、09-09 漏斗对比+收入、07-14 注释、06-10 AI Assistants 渠道、05-27 User journeys、04-14 严格顺序漏斗；2025-07-29 SSO、07-08 Managed Proxy）
13. https://plausible.io/status — 服务状态页（取证日全绿）
14. https://api.github.com/repos/plausible/analytics — GitHub API（29,551 星 / 1,913 forks / AGPL-3.0 / created 2018-12-04 / pushed 2026-10-19 / subscribers 159）
15. https://api.github.com/repos/umami-software/umami — GitHub API（39,458 星 / 8,343 forks / MIT / created 2020-07-17）
16. https://api.github.com/repos/matomo-org/matomo — GitHub API（21,960 星 / 2,918 forks / GPL-3.0 / created 2011-03-30）
17. https://raw.githubusercontent.com/plausible/analytics/master/README.md — README（Community Edition、自托管、技术栈）
18. https://github.com/plausible/analytics — 仓库页（截图取证）
19. https://news.ycombinator.com/item?id=24696145 — Show HN《Plausible – Self-Hosted Google Analytics alternative》（2020-10-06，351 points / 139 comments；5,000+ sites、三个月 1.8 亿 PV、脚本 0.7 KB、Elixir/Phoenix + Postgres + ClickHouse）
20. https://news.ycombinator.com/item?id=24868012 — HN《Plausible Analytics Isn't GDPR Compliant》（2020-10-23，54 points / 75 comments；IP 是否 PII、Breyer 判例、匿名化不可逆之争、创始人回复）
21. https://news.ycombinator.com/item?id=40909006 — HN《Plausible Analytics: GDPR Compliance w/o Cookie Consent Banner》（2024-07-08，145 points / 137 comments）
22. https://news.ycombinator.com/item?id=40925266 — HN《Plausible Community Edition》（2024-07-10，125 points / 103 comments；AGPL 义务讨论）
23. https://news.ycombinator.com/item?id=35121435 — HN《Lessons from building Plausible Analytics to $1.2M ARR in public》（2023-03-12，210 points / 76 comments）
24. https://hn.algolia.com/api/v1/search?query=plausible&tags=story&hitsPerPage=30 — HN 榜单实抓（本地直连，761 条命中，用于选帖）
25. https://noyb.eu/en/austrian-dsb-eu-us-data-transfers-google-analytics-illegal — 奥地利 DPA 2022-01-13 决定（noyb 模型案：GA 传输违法）
26. https://inplp.com/latest-news/article/use-of-google-analytics-still-breaches-the-gdpr-austrian-data-protection-authority-rejects-risk-based-approach/ — 奥地利 DPA 第二决定 D155.026（2022-04-22，驳回风险自评估路径）
27. https://privacymatters.dlapiper.com/2023/11/eu-new-edpb-guidelines-on-the-scope-of-the-cookie-rule/ — EDPB《Guidelines 03/2022》终版（2023-11-14 通过，Art 5(3) 广义技术中立解释）
28. https://commission.europa.eu/news-and-media/news/simpler-digital-rules-help-eu-businesses-grow-2025-11-19_en — 欧盟委员会新闻（Digital Omnibus 2025-11-19 提出，交欧洲议会与理事会通过）
29. https://www.taylorwessing.com/en/global-data-hub/2026/the-digital-omnibus-proposal/gdh---the-digital-omnibus---cookies — Taylor Wessing 法务分析（2026-02-02：Art 88a 免同意场景含第一方受众测量、口径窄、88a 生效后 6 个月 / 88b 24 个月适用）
30. https://www.recordinglaw.com/world-laws/world-data-privacy-laws/eu-data-privacy-laws/eprivacy-directive-cookie-law/ — ePrivacy 条例撤回（OJ C, C/2025/5423，2025-10-06 生效）
31. https://getlatka.com/companies/plausible-analytics — GetLatka（Annual revenue $3.1M Oct 2024 EST；最后一次实报 $279.6K May 2021 ≈ $23,300 MRR）
32. https://plausible.io/privacy-focused-web-analytics — 官方隐私主张页（无 Cookie、无跨站/跨设备追踪）
33. https://plausible.io/docs/proxy/introduction — 托管代理文档（自定义域名绕过广告拦截路径）

**勘误**：本文任何错误可在 javis.im 留言指出，核实后修订；若结论被推翻，本文将标注撤回。文中【事实】/【推断】/【观点】/【存疑】四色标注即为可信度分层，读者可只采信【事实】部分。官方自报的 21,000 付费、$1M ARR、135KB vs 2.5KB 体积对比，第三方收入 EST（$3.1M），以及 Art 88a 仍为提案的状态，均已随文列出而不是抹掉。

> 本站为独立技术分析，与文中项目无商业关系。
