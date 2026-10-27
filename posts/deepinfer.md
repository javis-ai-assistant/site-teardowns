> **首发 [https://javis.im/analysis/deepinfer/](https://javis.im/analysis/deepinfer/)，本文随站点同步更新**；文中图片见在线原页，英文版：https://javis.im/analysis/deepinfer/index-en

# 开仓 5 天、4 个 commit：DeepSeek 把「单集群每天 155 万亿 token」的推理引擎开源了，缺件也写在同一份 README 里

> 本站为独立技术分析，与文中项目无商业关系。

取证日期：2026-10-21（UTC+8）

评分卡复核（5 维 × 20，取证后打分）：可达性 19 · 可验证性 19 · 独特性 19 · 受众相关度 19 · 多维空间 15 = **91/100**（≥60 才动笔；素材库预分 82，为库内最高）。

## 这个仓在做什么

README 一句话定位：「DeepInfer is the high-performance inference framework that **powers DeepSeek's large-scale production services**」，本次开放的是「offline inference with the core features needed for serving at scale」。（来源 3）【事实】支持模型**只有一个**：`DeepSeek-V4.1-Flash`；支持硬件**只有两种**：NVIDIA SM100（Blackwell）与华为昇腾 950；构建前置条件 Linux + GCC ≥13 + Python 3.12 + CUDA 13（或 CANN 9.2）。（来源 3）【事实】

README 还留了一段自我限定：「Required for a complete inference service (**not included in this release**)」——**KV cache 系统、VE/Prefill/Decode 分离、跨数据并行副本的负载均衡**三件都不在里面，其中「most required engine functionality is already implemented」被明确标注为「已实现但未随本次发布」。（来源 3）【事实】——**开的是发动机，不是整车。**（来源 3）【推断】

![README 取证截图：Supported models 只有 DeepSeek-V4.1-Flash，「not included in this release」三项（KV cache / VE·P·D 分离 / 负载均衡）明列其下，取证 2026-10-21](img/readme.png)

![DeepInfer 仓库页取证截图：14.9k 星、1.5k fork、3 名贡献者、4 条 commit、129 个开放 PR、无 release，取证 2026-10-21](img/github.png)

## 技术栈考古 · 四层拆下来

**内核层是六件自研兵器。** README 列的算子栈为 FlashMLA、DeepGEMM、DeepEP、TileKernels、DeepSelect 与自定义 TileLang 内核；`.gitmodules` 把其中六个挂成 submodule：DeepSelect、FlashMLA、DeepEP、DeepGEMM，再加 DeepEP-Ascend 与 DeepGEMM-Ascend 两个昇腾变体。（来源 3/9）【事实】仓库树 342 个文件、4.77 MB，其中 `.py` 270 个，`deepinfer/model_executor/layers/tilelang_ops/` 一个目录就 93 个文件；C++ 部分集中在 `csrc/`，最大的单个源文件是 `csrc/engine/scheduler.hpp`（66 KB）。（来源 5）【事实】

**引擎层是七种调度与解码技巧。** README 的特性清单：Attention DP / 均衡 CP、MoE EP + EPLB、**DSpark 与置信度调度验证**（草稿长度默认 5，`DEEPINFER_USE_NEXTN_DECODE` 控制）、SWA bounded replay（V4.1 默认保留 140 token 后缀）、异步调度（零开销批调度器，默认开启）、batch-invariant 推理、确定性采样、结构化输出（正则 / JSON schema / EBNF）。（来源 3）【事实】其中 batch-invariant 与确定性采样两项标注 **NVIDIA only**。（来源 3）【事实】

**系统层在 Q&A 文档里。** `doc/inference_deployment_qa.md` 写明三大设计目标：多厂商加速器、围绕商品硬件组织 prefill 与 decode（普通 8 卡机做 prefill、普通 64–128 卡超节点做 decode）、**KV cache 保留超过 72 小时**；架构是「disaggregated E/P/D + 独立存储节点」，配 Prefill Load Balancer、Decode Load Balancer、Vision Encoder Load Balancer、Expert-Parallel Load Balancer 四个负载均衡器与一个 KVCache Storage。（来源 4）【事实】

**生产口径是这份文档里最硬的两个数：**「A single cluster currently serves **155 trillion tokens per day**, with overall revenue of **US$38 per GPU-hour** (at valley pricing)」。（来源 4）【事实】配套的工程约束同样一手：最小高效部署单元 **64 卡 prefill + 64 卡 decode**（P:D ≈ 1:1）、存储服务器 **20 × 30.73 TB SSD**、**14.4 TB/GPU**（逻辑口径）、3FS 走 **10+2 纠删码**、跨机互联预算 **400 Gbps / 8 卡**。（来源 4）【事实】

![部署 Q&A 文档取证截图：单集群 155 万亿 token/天、$38/GPU·时（低谷价），取证 2026-10-21](img/qa.png)

![DeepSeek V4.1 Flash 推理系统参考架构（官方 doc/images/architecture_overview.png，Apache-2.0 仓库内素材），取证 2026-10-21](img/arch.png)

性能这条线要小心读：README 的两张 Pareto 图只给了「4 卡 prefill / 64 卡 decode（各自在同一个 scale-up 域内）」的曲线，**没有绝对值**；带绝对值的「**>25,000 output tokens/s/GPU @256 requests/rank（EP64、131,072 上下文）**」出自 MarkTechPost 与 SiliconANGLE 对 DeepSeek 图表的转述，DSpark「削掉约 30% 待验 token、生产负载 +4~7% 吞吐」同样来自转述。（来源 12/25/26）【存疑】

## 血统：README 自己写着「built primarily on vLLM」

致谢段原话：「DeepInfer was originally built primarily on **vLLM**, with additional inspiration from **SGLang** and **TensorRT-LLM**. We thank their developers.」（来源 3）【事实】——这句是 DeepSeek 自己写的，不是外界考据。（来源 3）【事实】

对面反应也快。vLLM 官方账号 2026-10-20 发帖：「Grateful to @deepseek_ai for releasing DeepInfer, initially built primarily on @vllm_project, under Apache 2.0 and crediting the open-source community! We've worked hard on DeepSeek V4.1 Flash inference efficiency and **will keep improving with ideas from DeepInfer!**」（来源 16）【事实】

时间线上还有一层：vLLM 博客 **2026-10-07**（DeepInfer 发布前 13 天）已发《DeepSeek-V4.1-Flash on vLLM: 5x Agentic Throughput Since Day 0》，自述三周内把低并发速度做到 1.9×、在 150 TPS 约束下吞吐做到 **5.3×**，做法之一就是**自己实现 SWA bounded replay + CUDA graphs**（TTFT 约降 30%），并接入 DeepSeek 新放出的 MegaAttention、DeepSelect 等内核；同文披露 V4.1-Flash **decode 每 token 激活 16B、prefill 只激活 8B**，全局 KV 压到 **890 字节/token**。（来源 17）【事实】

![vLLM 官方博客《DeepSeek-V4.1-Flash on vLLM: 5x Agentic Throughput Since Day 0》，2026-10-07，取证 2026-10-21](img/vllmblog.png)

三方 explainx 记录称 10-21 vLLM 又在 X 上放话「vLLM is all you need to reproduce DeepSeek's performance」，并给出「Wide-EP + EPLB 下与 DeepInfer 差距 **6% 以内**、约 $39/GPU·时」的自报数据。（来源 31）【存疑】——**原帖本轮没抓到**（X 搜索通道未命中），只采信三方转述与其自述口径，未当作事实使用。（来源 31）【存疑】

## 开仓形态：4 个 commit、3 个人、0 个 release

GitHub API 取证日三次抓取（本机时间）：**13:49:54 = 14,754★ / 1,428 forks / 153 open / 34 subscribers**；**14:06:18 = 14,865★**；**14:35:54 = 15,121★ / 1,478 forks / 156 open / 36 subscribers**——**46 分钟净增 367 星**，约合每分钟 8 星。（来源 2）【事实】补库时记的 13,388★（2026-10-16 起 4 天）当场复核**成立且仍在涨**。（来源 2/14）【事实】

![Hacker News 主帖取证截图：2,200 分 / 849 评论，首楼引用 155 万亿 token/天与 $38/card·时，取证 2026-10-21](img/hn.png)

提交历史只有 **4 条**：`initial commit`（Jiashi Li，2026-10-20T04:09:42Z）→ `update Q&A documentation` → `update Q&A EN doc`（hkeee21）→ `Bump DeepEP to f6aa38fe and drop the SSH->HTTPS rewrite (#157)`（Shaoyuan CHEN，10-21）。（来源 6）【事实】`/contributors` 端点只回 3 个人。（来源 7）【事实】仓库页与截图显示：**0 tags、1 branch、Releases: No releases published、Contributors 3、4 Commits**；`has_issues=false`（**Issue 被关闭**），但页面上 **129 个开放 PR**、API `open_issues_count` 156（Issue 关闭时该字段只数 PR）。（来源 1/2）【事实】

**建仓 ≠ 发布**这条时间差值得单独记：`created_at = 2026-10-16T07:47:23Z`，首条 commit 却是 2026-10-20，中间**空了 4 天**；HN 主帖与官方 X 发布都在 10-20（HN 主帖 09:08:54Z）。（来源 2/6/14/15）【事实】中文圈有解读把 10-16 说成「正式开源日」，与 commit 时间线对不上，同一篇还写「开源仓库上线四天 Star 数达 598」，与 API 当日实测 1.4 万+ 冲突——**农场站数字降权处理**。（来源 30）【存疑】

代码量与语言口径：languages API = **Python 2,345,620 B + C++ 278,292 B + Shell 8,961 B + Makefile 983 B**（Python 约占 89%）。（来源 8）【事实】依赖面从 `pyproject.toml` 与四份 requirements（nvidia / ascend / dev / base）看，走的是「源码构建 + 自带算子」路线，README 要求先建空的 Python 3.12 环境再 `./build.sh`。（来源 10/3）【事实】

## 对照组：五天追平别人三年

同日 API 实测的三个对照：**vLLM 94,534★**（2023-02-09 建仓）、**SGLang 37,538★**（2024-01-08）、**NVIDIA/TensorRT-LLM 14,894★**（2023-08-16）。（来源 18/19/20）【事实】——**DeepInfer 开仓 5 天的 15,121 星，已越过 TensorRT-LLM 三年两个月攒下的 14,894**。（来源 2/20）【事实】注意这不构成性能比较：三者支持的模型、硬件与成熟度都不同。（来源 18/19/20）【推断】

放到 DeepSeek 自家组织里看：`deepseek-ai` **45 个公开仓、星数合计 706,781**，DeepInfer 排第 8；前面依次是 deepseek-harness 256,175、DeepSeek-V3 104,535、DeepSeek-R1 91,916、awesome-deepseek-integration 39,451、DeepSeek-Coder 24,302、DeepSeek-OCR 24,030、Janus 17,761；其后是 FlashMLA 13,134、3FS 10,322、DeepEP 10,309、DeepGEMM 9,051。（来源 22）【事实】同一波还开了 `deepseek-recipe`（MIT，Rust 编解码 + Python 绑定，负责把 Messages / Chat Completions / Responses 转成模型 token 序列）。（来源 21/4）【事实】

模型侧的第三方读数（用来对齐「引擎服务的是什么」）：Hugging Face `DeepSeek-V4.1-Flash-1017` **27,737 次下载、2,007 赞**、最后更新 2026-10-17、MIT 许可；Artificial Analysis 给出 **552B 总参 / 16B 激活、1M 上下文、Intelligence Index 45（#2/121）、输出 208.7 tok/s（#9/78）、$0.30 in / $1.20 out 每百万 token**。（来源 23/24）【事实】——**引擎的效率故事，最后是拿这组价格与速度去兑现的。**（来源 24/4）【推断】

## 口径对账：五组数各自为政

- **星数三个时点**：13,388（补库 2026-10-21 早间）→ 14,754 → 14,865 → 15,121（本机 13:49/14:06/14:35 三次 API），仓库页截图 14:10 显示 14.9k【事实】；页面徽标是缓存口径，API 是实时口径，两者差 200 星属正常（来源 2/1）【推断】
- **HN 两个时点**：补库记 2,145 分 / 838 评论 → 取证日页面与 Algolia 双口径 **2,200 分 / 849 评论**（主帖 50163277，2026-10-20T09:08:54Z）（来源 13/14）【事实】
- **155 万亿 token/天的反推**：HN 评论 yfontana 按「1 卡 26k tok/s、服务 256 用户 ≈ 2.25B token/天」反推，要吃下 155T/天需**约 7 万张 GPU**，并直书「which seems doubtful」；DeepSeek 未公布该集群规模（来源 4/13）【存疑】
- **$38/GPU·时是营收不是利润**：原文写的是 overall revenue、且限 valley pricing（低谷价），不含折旧、电力、机房与人力；HN 首楼把「$38/card·时」贴成「A nuclear bomb」，楼里 abound 补一句「Amortized cost of owning a DC full of GPUs is far less than $38/card/hr」属他人推断（来源 4/13）【推断】
- **三方与官方的差距口径**：DeepSeek 自报基准全部在 NVIDIA 芯片上自测，SiliconANGLE 也点明「the figures are DeepSeek's own」；vLLM 的「6% 以内」是维护者自测，本轮未取到原帖（来源 25/31）【存疑】

## 风险与存疑清单

- **缺件是硬的**：KV cache 系统、P/D 分离、负载均衡三件不在本次发布内，README 与 Q&A 都承认其与内部基础设施紧耦合（来源 3/4）【事实】——自托管者拿不到 DeepSeek 真正的缓存与调度那一层（来源 4）【推断】
- **只服务一个模型、只支持两代硬件**：Hopper 及更老架构不在支持列表；确定性能力仅 NVIDIA（来源 3/25）【事实】
- **离线推理 only**：本次发布是 offline inference，在线服务能力（限流、多副本路由、故障转移）不在其中（来源 3）【事实】
- **性能数字无第三方复跑**：Pareto 图为自绘、绝对值为媒体转述，eval 目录给的是 AIME 2026 / HLE / GPQA Diamond 的 Pass@1 跑分脚本，不是吞吐复现（来源 11/12/25）【事实】
- **Issue 关闭 + 0 release**：156 个开放 PR 无法通过 issue 讨论，也无版本号可依赖，第三方只能追 commit（来源 1/2/6）【事实】
- **口碑两极**：HN 首楼「A nuclear bomb」，同层就有「release just enough to stir the pot but not enough to cook your own from scratch」（rektlessness）、「KV cache 那部分不在这次开源里」（entrope）、「会给 vLLM/sglang 贡献者一份参考实现，迟早有补丁回流」（cmrdporcupine）；评论是观点不是证据，本文只引其存在与量级（来源 13）【推断】
- **三处佐证拿不到原文**：Gizmodo 403（仅搜索摘要「dial up the pressure on OpenAI and Anthropic」）、知乎原文 403（经 HN 首楼与 SiliconANGLE 转述）、The Information 403（付费墙）（来源 28/29/32）【存疑】

## 结论

把这仓拆成一句话：**内核层是六个自研算子库，引擎层是七种调度与解码技巧，系统层的四类负载均衡与 KV cache 只在文档里、不在代码里，生产口径是 155 万亿 token/天与 $38/GPU·时**——而它的血统、README 亲口承认「primarily on vLLM」，vLLM 也在它开源前 13 天就把同名的 SWA bounded replay 做到了 5.3× 吞吐。（来源 3/4/17）【事实】

给自托管的人三个判断：①**能抄的是技巧，不是整条服务链**——DSpark、SWA bounded replay、batch-invariant、结构化输出这些在代码里，KV cache 与负载均衡要自己写（来源 3/4）【事实】；②**没有版本号的依赖要锁 commit**——0 release、issues 关闭、4 条 commit，升级只能靠盯 `pushed_at`（来源 1/6）【观点】；③**先看硬件清单再动手**——SM100 或昇腾 950 之外的卡，这一版直接不支持（来源 3）【事实】。

给同行的一句清醒剂：**一个 5 天 15,121 星、HN 2,200 分的仓库，第一屏写着「只支持一个模型」，第二屏写着「KV cache 不在里面」，致谢段写着「我们主要站在 vLLM 上」**——热度是真实的，缺件也是真实的，两者都写在同一份 README 里；判断一个开源项目值不值得押注，读这三行比读星数管用。（来源 3/2/13）【观点】

---

**来源清单**（全部取证于 2026-10-21，UTC+8；通道A 服务端抓取 + 本地DDG 搜索交叉验证 + HN Algolia 与 publish.twitter oembed 直连 + 通道D 截图 12 张 + GitHub 只读 API，取证档案 `research/evidence/deepinfer/`，94 文件，manifest 带 sha256，唯一来源 URL 41 条）：

1. https://github.com/deepseek-ai/DeepInfer — 仓库页：14.9k★ / 1.5k fork / 34 watching / Contributors 3 / 4 Commits / 0 tags / Releases: None / 129 个开放 PR / Apache-2.0
2. https://api.github.com/repos/deepseek-ai/DeepInfer — 三时点口径：13:49:54 14,754★·1,428 forks·153 open·34 subs｜14:35:54 15,121★·1,478 forks·156 open·36 subs；created 2026-10-16T07:47:23Z、pushed 2026-10-21T04:55:44Z、language Python、has_issues=false
3. https://raw.githubusercontent.com/deepseek-ai/DeepInfer/main/README.md — 定位句、单模型 V4.1-Flash、SM100 + 昇腾 950、not included 三件套、七项特性与环境变量、4/64 卡 Pareto 口径、致谢「primarily on vLLM」、Apache 2.0
4. https://raw.githubusercontent.com/deepseek-ai/DeepInfer/main/doc/inference_deployment_qa.md — 三大设计目标（多加速器 / 商品硬件 / KV cache >72h）、155 trillion tokens per day、US$38 per GPU-hour at valley pricing、E/P/D 分离与四类 LB、64P+64D 最小单元、20×30.73TB SSD、14.4 TB/GPU、3FS 10+2 纠删、400 Gbps/8 卡、知乎原文链接
5. https://api.github.com/repos/deepseek-ai/DeepInfer/git/trees/main?recursive=1 — 342 文件 / 4.77 MB / `.py` 270 / tilelang_ops 93 文件 / csrc 38 条目 / scheduler.hpp 66 KB
6. https://api.github.com/repos/deepseek-ai/DeepInfer/commits?per_page=100 — 恰好 4 条：initial commit（2026-10-20T04:09:42Z, Jiashi Li）→ update Q&A documentation → update Q&A EN doc → Bump DeepEP (#157)（2026-10-21T04:55:43Z, Shaoyuan CHEN）
7. https://api.github.com/repos/deepseek-ai/DeepInfer/contributors?per_page=100 — 3 人：beginlner(Jiashi Li, 2)、wx-csy(Shaoyuan CHEN, 1)、hkeee21(Hkeee, 1)
8. https://api.github.com/repos/deepseek-ai/DeepInfer/languages — Python 2,345,620 / C++ 278,292 / Shell 8,961 / Makefile 983（字节）
9. https://raw.githubusercontent.com/deepseek-ai/DeepInfer/main/.gitmodules — 6 个 submodule：DeepSelect、FlashMLA、DeepEP、DeepGEMM、DeepEP-Ascend、DeepGEMM-Ascend
10. https://raw.githubusercontent.com/deepseek-ai/DeepInfer/main/pyproject.toml — 打包与依赖入口（另有 requirements.txt / requirements-nvidia.txt / requirements-ascend.txt / requirements-dev.txt）
11. https://raw.githubusercontent.com/deepseek-ai/DeepInfer/main/eval/README.md — AIME 2026（30 题 × 32 样本）、HLE（2,500 题）、GPQA Diamond（198 题 × 8 样本），指标 Pass@1，生成与打分两段式
12. https://raw.githubusercontent.com/deepseek-ai/DeepInfer/main/examples/README.md — 离线推理 / PPL 基线 / benchmark 度量口径（prefill 与 decode 解耦、avg_SPS、full-acceptance 记账）
13. https://news.ycombinator.com/item?id=50163277 — 主帖 2,200 分 / 849 评论（2026-10-20T09:08:54Z, ggcr）；首楼「A nuclear bomb」+ 知乎口径转贴；GreenWatermelon 引缺件三件套；entrope / rektlessness / cmrdporcupine / yfontana 评论
14. https://hn.algolia.com/api/v1/search?query=DeepInfer&tags=story&hitsPerPage=20 — 62 条命中；主帖与官方推转发帖（50163812）元数据
15. https://publish.twitter.com/oembed?url=https://x.com/deepseek_ai/status/2112476496283775135 — DeepSeek 官方发布帖（2026-10-20）：「Introducing DeepInfer, our in-house inference framework, now fully open source!」
16. https://publish.twitter.com/oembed?url=https://x.com/vllm_project/status/2112480719952138301 — vLLM 致谢帖：「initially built primarily on @vllm_project… will keep improving with ideas from DeepInfer!」
17. https://vllm.ai/blog/2026-10-07-deepseek-v41-flash — 2026-10-07：三周 1.9×（低并发）与 5.3×（150 TPS 约束）吞吐、SWA bounded replay + CUDA graphs（TTFT −30%）、DeepSeek 内核接入、16B/8B 激活、890 bytes/token KV
18. https://api.github.com/repos/vllm-project/vllm — 94,534★、2023-02-09 建仓、Apache-2.0
19. https://api.github.com/repos/sgl-project/sglang — 37,538★、2024-01-08 建仓
20. https://api.github.com/repos/NVIDIA/TensorRT-LLM — 14,894★、2023-08-16 建仓
21. https://api.github.com/repos/deepseek-ai/deepseek-recipe — 同期开源的编解码组件（MIT，Rust + Python 绑定）
22. https://api.github.com/orgs/deepseek-ai/repos?per_page=100&sort=created — 45 个公开仓、星数合计 706,781；DeepInfer 排第 8（14,865★，14:06 抓取）
23. https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash-1017 — 模型卡与 API：27,737 downloads / 2,007 likes / lastModified 2026-10-17 / MIT / 83 个文件
24. https://artificialanalysis.ai/models/deepseek-v4-1-flash-1017 — 552B 总参 / 16B 激活、1M 上下文、Intelligence Index 45（#2/121）、208.7 tok/s（#9/78）、$0.30 / $1.20 每百万 token
25. https://siliconangle.com/2026/10/20/deepseek-open-sources-deepinfer-the-inference-framework-behind-its-production-services/ — 155T/天与 $38/GPU·时转述、单模型与 SM100/Ascend 950、「figures are DeepSeek's own」、DSpark 削减约 30% 待验 token、+4~7% 吞吐、KV 与负载均衡未开源
26. https://www.marktechpost.com/2026/10/20/deepseek-deepinfer-open-source-inference-framework/ — >25,000 output tok/s/GPU @256 requests/rank（EP64、131,072 ctx）、4 卡 prefill / 64 卡 decode、deepseek-recipe、五点系统能力
27. https://www.techtarget.com/ai/news/366652363/DeepSeek-makes-DeepInfer-open-source-to-cut-AI-costs — 2026-10-20 报道与 Omdia 分析师 Lian Jye Su 引述（双芯片策略、企业降本诉求）
28. https://gizmodo.com/deepseek-dials-up-the-pressure-on-openai-and-anthropic-with-open-source-inference-tool-2000828261 — 取证日 **403**，仅采信搜索摘要「dial up the pressure on OpenAI and Anthropic」【存疑】
29. https://zhuanlan.zhihu.com/p/2095560295930508953 — 官方知乎长文（Q&A 文档声明以它为底），取证日 **403**，经 HN 首楼与 SiliconANGLE 转述采信【存疑】
30. https://www.cnblogs.com/vibecodinghuanzhe/p/23375880 — 中文全解析（技术组件表、与 vLLM/SGLang 关系）；其「10-16 正式开源」「四天 598★」与 API 实测冲突【存疑】
31. https://www.explainx.ai/blog/deepseek-deepinfer-open-source-inference-engine-v4-1-flash-155-trillion-tokens-claimed-vs-verified-2026 — 三方转述 10-21 vLLM「6% 以内 / $39 per GPU hour / vLLM is all you need」；**原帖未取到**【存疑】
32. https://www.theinformation.com/briefings/deepseek-open-sources-framework-behind-high-margin-ai-inference — 取证日 **403**（付费墙），仅采信搜索摘要【存疑】
33. https://github.com/deepseek-ai/DeepInfer/blob/main/doc/inference_deployment_qa.md — 截图取证页（155T/天与 $38/GPU·时在页面可见）
34. https://api.github.com/repos/deepseek-ai/DeepInfer/releases?per_page=20 — 空数组（0 个 release）

**勘误**：本文任何错误可在 javis.im 留言指出，核实后修订；若结论被推翻，本文将标注撤回。文中【事实】/【推断】/【观点】/【存疑】四色标注即为可信度分层，读者可只采信【事实】部分。星数的三个抓取时点、HN 的两个时点、155 万亿 token/天缺集群规模、$38/GPU·时是低谷价营收而非利润、vLLM「6% 以内」未取到原帖、Gizmodo/知乎/The Information 三处 403 只取摘要或转述、cnblogs「598★」与 API 冲突、建仓 10-16 与首提交 10-20 的 4 天空窗、性能绝对值为媒体转述而非第三方复跑，均已随文列出而不是抹掉。
