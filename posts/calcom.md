> **首发 [https://javis.im/analysis/calcom/](https://javis.im/analysis/calcom/)，本文随站点同步更新**；文中图片见在线原页，英文版：https://javis.im/analysis/calcom/index-en

# 五年开源、49,185 星：Cal.com 把生产代码搬进私库，公仓只剩自认「别上生产」的 Cal.diy

> 本站为独立技术分析，与文中项目无商业关系。

取证日期：2026-10-21（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 19 · 可验证性 19 · 独特性 18 · 受众相关度 17 · 多维空间 18 = **91/100**（≥60 才动笔；素材库预分 80）。

## 这个站在做什么

取证日官网首屏一句话：**The better way to schedule your meetings**，正文写「A fully customizable scheduling software for individuals, businesses taking calls and developers building scheduling platforms where users meet users.」——卖点是三层：个人预约页、企业排期、**给别人的平台嵌排期**。（来源 1）【事实】导航把「Enterprise / Cal.ai / Developer / Pricing」摆在最前，页脚另挂 Self-hosted、Atoms、Routing、Enterprise API 等十来个入口。（来源 1）【事实】

对外数字两处：customers 页自称「**Trusted by more than 1,000,000+ users**, from ambitious startups to major enterprises」，`cal.com/open` 页自述「The most public private company… operates fully transparent and shares its salaries and core metrics」，mission 写「connect a billion people by 2031 through calendar scheduling」。（来源 2/3）【事实】——薪酬与核心指标所在的手册 `handbook.cal.com` 取证日撞上 Google 登录墙，公开承诺拿不到数。（来源 4）【存疑】

![Cal.com 官网首页首屏：The better way to schedule your meetings，取证 2026-10-21](img/home.png)

## 增长拆解 · 一级火箭：一个仓吃下全组织 92.8% 的星

GitHub API 取证日实抓：`calcom/cal.diy` **49,185 星 / 15,394 forks / 1,507 open issues / 222 subscribers**（`watchers_count` 与 `stargazers_count` 相等，222 才是真订阅数），创建于 **2021-03-22**，最近推送 2026-10-19，许可证 **MIT**。（来源 5）【事实】

组织层面更集中：`calcom` 组织 **43 个公开仓、星数合计 52,977，其中 cal.diy 一家占 92.8%**；其后 Cal Sans 字体 1,730、docker 936、synclinear 408、platform-starter-kit 272，官方 MCP server `cal-mcp` 只有 30 星。（来源 6）【事实】另有一处残留：`wp-plugin` 仓至今挂 **AGPL-3.0**，那是改名前的旧许可体系留下的。（来源 6）【事实】

![GitHub 仓库页实抓：calcom/cal.diy 49.2k 星，README 首行即社区版定位，取证 2026-10-21](img/github.png)

社区热度有两条时间锚：2023-01-24《Cal.com: Open Scheduling Infrastructure》主帖 **576 分 / 305 评论**，楼里吵的正是「AGPLv3 要求你把克隆公开」这一条许可争议；2026-04-15《Cal.com is going closed source》**391 分 / 317 评论**。（来源 7/8）【事实】Algolia 全量检索 `cal.com` 有 **81 条 story**，从 2021 年个位数分数的首发帖一路排到 2026 年的两场大战。（来源 9）【事实】

另一条腿是程序化 SEO：`sitemap_en.xml` 取证日 **1,128 个 URL**，其中 `/blog` **722** 条、`/workflows` 172 条、`/routing` 150 条、`/scheduling` 13 条（含 `calcom-vs-calendly` 对比页），另有 de/fr/nl/pt/es/it 六份语种站点地图。（来源 10）【事实】——**博客 + 功能微页 + 对比页拼成一张搜索网**，这是一条不花广告费的获客线。（来源 10）【推断】

![Cal.com 客户案例页：1,000,000+ users 自述与带数字的案例条目，取证 2026-10-21](img/customers.png)

## 增长拆解 · 二级：从一条链接到「排期基础设施」

平台化痕迹在分发与开发面上：npm `@calcom/atoms` 最近一个自然月下载 **154,380 次**（2026-09-19→10-18），同名老包 `cal.com` 只有 704 次。（来源 11）【事实】官网 integrate 页把 React atoms、OAuth client、`clientSecret` 直接摆成代码示例，页脚「Integrate with 100+ apps」对应 App Store 的集成矩阵。（来源 12/1）【事实】about 页的定位话术已经不是工具：「Cal.com is that **infrastructure**. It sits underneath your workflows, routing, and systems…」，并把 sales、marketplace、healthcare、embedded product 四类客户摆在一排。（来源 13）【事实】

客户案例页给了三组可引用的效率数：营销团队「closed **20% more deals**」、人才市场 Pangea.app「**70% revenue boost**」、语音 AI 公司 Retell AI「saved **thousands of hours** of开发时间」。（来源 2）【事实】——案例数字由被评方与客户共同署名，量级可采信、精确口径无第三方复核。（来源 2）【存疑】

## 转折 · 2026 年 4 月：三份文件把主仓搬进私库

时间线是连着的。2026-04-13 官方博客先发《AI Is Finding Critical Vulnerabilities Faster Than Teams Can Fix Them》，署名是客座作者、Hex Security 创始人 Huzaifa Ahmad，正文讲「modern applications aren't just vulnerable, they're continuously vulnerable」。（来源 14）【事实】次日 2026-04-14，联创 Bailey Pumfleet 发《Cal.com is going closed source. Here's why.》：「After **five years as open source champions**, Cal.com is going closed source… there's **one simple reason: security**」「Being open source is increasingly like giving attackers the **blueprints to the vault**」「Cal.diy will continue as an open option for **hobbyists**」。（来源 15）【事实】——铺垫与公告只隔一天，先后关系是事实，动机链条属【推断】。（来源 14/15）【推断】

![官方公告《Cal.com is going closed source. Here's why.》首屏，取证 2026-10-21](img/announce.png)

2026-04-15 技术篇把账算细：**被移出社区版的功能**包括 Organizations & Teams（多租户、团队可用性、PBAC、API v2 相关端点）、Routing Forms、Workflows、Instant Booking、AI Phone、Attributes & Segments、SAML/SSO、Insights、**整个 API v1**、Enterprise UI、Booking Audit、Impersonation——共 **12 类**；文章同时写明「Cal.diy ships with **API v2 only**」。（来源 16）【事实】

![技术篇《Moving to closed-source: the technical changes》删减清单，取证 2026-10-21](img/tech.png)

许可这条线最值得考古。v6.4 changelog 原话：「Starting today, Cal.com will relaunch all free and open-source code as **Cal.diy under MIT License**… This new community edition is only for self-hosting and use **at your own risk**」，并补了一句关键的自我更正：商业版「was **"source-available"** before, which is now **"closed source"**」。（来源 17）【事实】git 侧完全对得上：`LICENSE` 文件的提交史是 2023-06-20 `chore: added commercial API to LICENSE` → 2024-03-26 三连 `Update LICENSE`（内容 = `ee/` 与 `apps/api/v2/src/ee` 走 Commercial License，其余 **AGPLv3**）→ 2026-04-15 `refactor: Cal.diy (#28903) * feat: Cal.diy — community-driven MIT-licensed fork`。（来源 18/19）【事实】取证日现物三项验证：`LICENSE` 1,073 字节纯 **MIT**、`packages/features/ee` 路径 **404**、`github.com/calcom/cal.com` 返回 **301 → calcom/cal.diy**。（来源 20/21/22）【事实】

![LICENSE 文件实抓：MIT License + Copyright (c) 2020-present Cal.com, Inc.，取证 2026-10-21](img/license.png)

README 的措辞比公告更直白：「**Use at your own risk.** Cal.diy is the open source community edition… It is **strictly recommended for personal, non-production use**」「For any commercial and enterprise-ready scheduling infrastructure, **use Cal.com, not Cal.diy**」。（来源 23）【事实】——一个 49,185 星的仓，在 README 第一屏劝你别把它放上生产。（来源 23/5）【推断】

## 钱 · 四档价目、Calendly 征战页与第二曲线

取证日定价页四档：**Individuals 免费**（1 用户、无限事件类型、100+ 集成、Stripe/PayPal 收款、双向 Salesforce/HubSpot、一键导入 Calendly 数据）；**Teams $12/user/mo**（年付，省 25%，含 round-robin、routing forms、booking analytics、去品牌）；**Organizations $28/user/mo**（无限子团队、公司子域、SAML SSO + SCIM、SOC 2 / HIPAA / ISO 27001）；**Enterprise 自定义**（专属 onboarding、SLA、HRIS 集成、专属数据库）。（来源 24）【事实】对面 Calendly 取证日价目：**Standard $10/seat/mo、Teams $16/seat/mo、Enterprise 起 $15k/yr（50 席起）**。（来源 25）【事实】——**中档 $12 卡在 Calendly Standard 与 Teams 之间，高档 $28 比 Calendly Teams 贵 75%**，靠 SAML/SCIM/合规撑溢价。（来源 24/25）【推断】

![Cal.com 定价页四档实抓，取证 2026-10-21](img/pricing.png)

抢人动作也写在 URL 上：`/calendly`（与 `/scheduling/calcom-vs-calendly` 同源）首屏就是「Migrate from Calendly for free」，承诺「We'll credit **100% of your current Calendly plan** when you switch」，下面逐行对比白标、双向同步、角色权限、自定义 API。（来源 26/10）【事实】第二曲线是 **Cal.ai**：页面挂公开测试号，live demo 由联创 Peer 亲自打，v6.4 changelog 里也出现 Cal.com Agents 与 routing trace 进 Insights 等条目。（来源 27/17）【事实】

![Calendly 征战页：Migrate from Calendly for free + 100% credit 承诺，取证 2026-10-21](img/vs.png)

融资与收入，四段口径各自有出处、互不连线：seed **$7.4M**——Latka 记 2021-07（Bailey 出镜原话），Tracxn 记 **2021-12-17**。（来源 28/29）【事实/存疑】；Series A **$25M**，2022-04-15，**Seven Seven Six（Alexis Ohanian）领投**，Obvious Ventures、OSS Capital、Tobi Lutke、Jack Altman、Anthony Pompliano 参与，累计 **$32.4M**。（来源 30/29）【事实】；估值 **$150M**（Latka 引 VentureBeat，2022-04）。（来源 28）【存疑】；**此后四年半没有新轮**。（来源 28/29）【事实】

收入三读数同出 Latka 且均标「CEO 确认」：2022-05 **$180K ARR**（$15K MRR）、2023-08 **$1.6M**、2024-10 **$5.1M**（同比 +230%）；员工数 **44**（2026-04，EST）。（来源 28）【存疑】——**最近一次收入口径停在 2024-10，取证日已过去 24 个月，官方没有再开口**。（来源 28）【事实】

## 口径对账：五组数各自为政

- **星数时间差**：第三方报道写「over **41,000** GitHub stars」（2026-04 转闭源当时），取证日 API 为 **49,185**——6 个月净增约 8 千【推断】；转闭源没有立刻劝退星星。（来源 31/5）
- **许可三口径**：公告标题叫「open source champions」，v6.4 自我更正为「source-available」，今天公仓是 MIT 社区版 + 闭源商业版；`wp-plugin` 还留着 AGPL-3.0。（来源 15/17/6）
- **watchers 口径**：API `watchers_count` == `stargazers_count`（49,185），真实订阅数是 `subscribers_count` **222**。（来源 5）
- **收入与用户量不同源**：自述 **1,000,000+ users**（官网案例页）对第三方收入 **$5.1M**（Latka，2024-10），两者口径完全不同，不能相互推导。（来源 2/28）【存疑】
- **Open Startup 的公开承诺 vs 登录墙**：`cal.com/open` 说 shares its salaries and core metrics，`handbook.cal.com` 取证日需要 Google 登录。（来源 3/4）【存疑】

## 风险与存疑清单

- **社区版被自家文档劝退**：README 明写 non-production、use at your own risk；自托管者要么用功能被删的社区版，要么申请商业版私仓邀请。（来源 23/16）【事实】
- **安全论证没说服社区**：HN 2026-04-15 帖 391 分 / 317 评论，高赞原话「This is a weird knee-jerk reaction. I feel like this is **more a business decision than a security decision**」（gouthamve）、「**Security is window dressing**」（tecoholic）。（来源 8）【事实】——评论是观点不是证据，本文只引其存在与量级。（来源 8）【推断】
- **创始人 2023 年的表态与今天形成对照**：Bailey Pumfleet 当年在 HN 亲口回「**Open source definitely isn't a growth strategy for us**」，理由是 longevity、privacy、对 OSS 的热爱。（来源 7）【事实】2026 年以安全为由转闭源后，这段话被重新翻出来读。（来源 7/15）【推断】
- **收入与估值全为第三方**：$5.1M、$150M 均出自 Latka 对 CEO 的采访记录与旧年份转述，官方无 in-revenue 页面。（来源 28）【存疑】
- **四年半无新轮**：$32.4M 总融资停在 2022-04，同赛道 Calendly Enterprise 起步价已到 $15k/yr，钱与时间的余量都要打问号。（来源 29/25）【推断】
- **第三方解读权重大于一手**：opencals、trend.hulryung、frontierbeat 等解读文里，frontierbeat 取证日 **403**，只采信搜索摘要；农场站内容一律降权。（来源 32/33/34）【存疑】
- **商品化压力是读者共识而非本文结论**：HN 楼中多次指向 Google Calendar 免费排期与 Meet 的捆绑，属他人观点。（来源 8）【观点】

![HN《Cal.com is going closed source》391 分 / 317 评论原帖，取证 2026-10-21](img/hn.png)

![HN 2023-01-24 主帖 576 分：AGPLv3 许可争议现场，取证 2026-10-21](img/hn2023.png)

## 结论

把三级火箭收成一句话：**第一级靠开源仓 + 722 篇博客把人吸进来（49,185 星、org 占比 92.8%、sitemap 1,128 URL），第二级靠 Atoms/OAuth/App Store 把人变成嵌入方（npm 月 154,380、自称 1,000,000+ 用户），第三级靠四档价目与企业合规收钱（$12 / $28 / Custom）**——2026-04 的转闭源不是火箭换级，而是把第一级的**代码**与第三级的**代码**正式分家：公仓留 MIT 社区版继续获客，生产代码进私库继续收钱。（来源 5/10/11/24/16）【推断】

给做工具的团队三个可抄的动作：①**「source-available」与「open source」要分账**——官方自己都承认以前是 source-available，引用别家开源项目时先看许可证里有没有 `ee/` 这种商业目录（来源 17/18）【观点】；②**迁移留一条付费通道**——Calendly 换机 100% 抵扣这招把对手的沉没成本变成自己的获客成本（来源 26）【观点】；③**弃用社区版要写清楚边界**——README 直说 non-production 反而比悄悄删功能体面，丑话说在前面能省掉一半信任损耗（来源 23）【观点】。

一句清醒剂：**一个把「连接十亿人」写进使命的公司，最近一次公开收入停在 24 个月前的 $5.1M，而它的社区版 README 第一屏写着「别上生产」——开源是获客的门面，收钱的那部分从来不在门面里**；下次看到五万星的仓，先翻 `LICENSE` 与 `ee/` 目录，再决定要不要把业务压上去。（来源 15/17/23/28）【观点】

---

**来源清单**（全部取证于 2026-10-21，UTC+8；通道A 服务端抓取 + 本地DDG 搜索交叉验证 + 通道D 截图 12 张 + GitHub PAT 只读 API + HN Algolia 公开 API + npm downloads API，取证档案 `research/evidence/calcom/`，92 文件，manifest 带 sha256）：

1. https://cal.com/ — 官网首页首屏（The better way to schedule your meetings / 定位句 / Enterprise·Cal.ai·Developer·Pricing 导航 / 页脚 Self-hosted·Atoms·Routing·Enterprise API / 100+ apps）
2. https://cal.com/customers — 「Trusted by more than 1,000,000+ users」、案例条目（closed 20% more deals、Pangea 70% revenue boost、Retell AI 数千小时）
3. https://cal.com/open — The most public private company、shares its salaries and core metrics、mission connect a billion people by 2031
4. https://handbook.cal.com — 取证日 Google 登录墙（公开手册拿不到正文）
5. https://api.github.com/repos/calcom/cal.com — canonical `calcom/cal.diy`：49,185★ / 15,394 forks / 1,507 issues / 222 subscribers / MIT / created 2021-03-22 / pushed 2026-10-19 / homepage cal.diy
6. https://api.github.com/orgs/calcom/repos?per_page=100&type=public — 43 个公开仓、星数合计 52,977（cal.diy 92.8%）、sans 1,730 / docker 936 / synclinear 408 / platform-starter-kit 272 / cal-mcp 30、wp-plugin=AGPL-3.0
7. https://news.ycombinator.com/item?id=34507672 — 2023-01-24《Cal.com: Open Scheduling Infrastructure》576 分 / 305 评论；AGPLv3 公开克隆之争；Bailey Pumfleet「Open source definitely isn't a growth strategy for us」
8. https://news.ycombinator.com/item?id=47780456 — 2026-04-15《Cal.com is going closed source》391 分 / 317 评论；「more a business decision than a security decision」「Security is window dressing」
9. https://hn.algolia.com/api/v1/search?query=cal.com&tags=story&hitsPerPage=40 — 81 条 story（含 2026-04-21 Cal.diy 发布帖 259 分 / 71 评论）
10. https://cal.com/sitemap_en.xml — 1,128 URL：/blog 722、/workflows 172、/routing 150、/scheduling 13；另有 de/fr/nl/pt/es/it 站点地图
11. https://api.npmjs.org/downloads/point/last-month/@calcom/atoms — 154,380（2026-09-19→10-18）；同接口 `cal.com` 为 704
12. https://cal.com/integrate — React atoms + OAuth client + clientSecret 代码示例页
13. https://cal.com/about — 「Cal.com is that infrastructure… sits underneath your workflows, routing, and systems」、四类客户、programmable/flexible/reliable/secure 四条
14. https://cal.com/blog/continuous-ai-pentesting-vulnerability-discovery — 2026-04-13 客座文（Hex Security 创始人）：28 家公司 AI pentest、近半数为 critical、「continuously vulnerable」
15. https://cal.com/blog/cal-com-goes-closed-source-why — 2026-04-14 官方公告：five years as open source champions、one simple reason: security、blueprints to the vault、open option for hobbyists
16. https://cal.com/blog/cal-diy-open-source-to-closed-source — 2026-04-15 技术篇：12 类被移出功能清单、API v2 only、enterprise features removed
17. https://cal.com/blog/calcom-v6-4 — v6.4 changelog：relaunch as Cal.diy under MIT、at your own risk、"source-available" → "closed source"、20x 性能、Cal.com Agents
18. https://api.github.com/repos/calcom/cal.diy/commits?path=LICENSE&per_page=5 — 提交史：2023-06-20 commercial API → 2024-03-26 ×3 → 2026-04-15 `refactor: Cal.diy (#28903)`
19. https://api.github.com/repos/calcom/cal.diy/license?ref=06695f41 — 2024-03 版 LICENSE：ee/ 与 apps/api/v2/src/ee = Commercial License，其余 AGPLv3（含 AGPL 全文）
20. https://raw.githubusercontent.com/calcom/cal.diy/main/LICENSE — 取证日现物 MIT License（1,073 字节）
21. https://github.com/calcom/cal.diy/tree/main/packages/features/ee — 取证日 404（EE 目录已移除）
22. https://github.com/calcom/cal.com — 取证日 301 → https://github.com/calcom/cal.diy
23. https://raw.githubusercontent.com/calcom/cal.diy/main/README.md — Use at your own risk / strictly recommended for personal, non-production use / use Cal.com, not Cal.diy / fork of Cal.com with all enterprise/commercial code removed
24. https://cal.com/pricing — 四档：Free / Teams $12（年付省 25%）/ Organizations $28 / Enterprise Custom；SAML SSO·SCIM·SOC 2·HIPAA·ISO 27001
25. https://calendly.com/pricing — Standard $10/seat/mo、Teams $16/seat/mo、Enterprise starts at $15k/yr（50 seats）
26. https://cal.com/calendly — Migrate from Calendly for free、credit 100% of your current Calendly plan、逐行功能对比
27. https://cal.com/ai — Cal.ai 电话 agent：公开测试号、live demo 由联创 Peer 打、触发器/号码/提示词三步
28. https://getlatka.com/companies/calcom — 收入 $180K ARR(2022-05) → $1.6M(2023-08) → $5.1M(2024-10，+230%，Confirmed by CEO)、估值 $150M(2022-04)、员工 44(2026-04 EST)、seed $7.4M(2021-07)
29. https://tracxn.com/d/companies/calcom/__IEeuL9zwIQRcTDpJAAntiwKZ0CItPOOlE9FD_QgbF4o/funding-and-investors — 累计 $32.4M / 2 轮：seed 2021-12-17、Series A 2022-04-15 $25M
30. https://gritdaily.com/cal-com-raises-25-million-in-series-a-funding-to-bring-order-to-the-chaos/ — 2022-04-15：$25M Series A、Seven Seven Six 领投、Obvious Ventures/OSS Capital/Jack Altman/Tobi Lutke/Anthony Pompliano 参与
31. https://theagenttimes.com/articles/cal-com-goes-closed-source-leaves-stripped-down-cal-diy-for--44ff7214 — 「over 41,000 GitHub stars」（2026-04 口径）+ production codebase moved private
32. https://opencals.com/blog/cal-com-closed-source — 自托管者视角解读（MIT fork 被自家文档标 non-production）
33. https://trend.hulryung.com/en/posts/2026-04-16-1000-calcom-going-closed-source-open-source-sustainability-business-model-2026/ — 与 HashiCorp/Redis/Elastic 许可退潮并列的商业模型解读
34. https://frontierbeat.com/2026/04/15/cal-com-abandons-open-source-ai-hacking-fears/ — 「first company to cite AI hacking specifically」；取证日 **403**，仅采信搜索摘要【存疑】

**勘误**：本文任何错误可在 javis.im 留言指出，核实后修订；若结论被推翻，本文将标注撤回。文中【事实】/【推断】/【观点】/【存疑】四色标注即为可信度分层，读者可只采信【事实】部分。seed 日期 2021-07 与 2021-12 两个口径、$150M 估值的第三方来源、收入三读数的采访窗口差、41,000 与 49,185 两个星数的时点差、「1,000,000+ users」与 $5.1M 收入的不同源、Open Startup 承诺与 handbook 登录墙的落差、frontierbeat 403 只取摘要、转闭源公告与前一日客座文的时间先后（动机属推断），均已随文列出而不是抹掉。
