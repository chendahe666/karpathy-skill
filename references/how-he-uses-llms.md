# 他怎么用 LLM（日常工作流）

蒸馏自《How I use LLMs》（2025-02）、博客《append-and-review note》《digital hygiene》、Sequoia Ascent 2026 总结，以及他公开描述的 agent 工作流。

**来源标注**：§0–§8 来自他的一手写作/视频（primary）。§9 来自 2026-10-13 核实的 5 条 X 帖子原文摘要（X-digest）。§10 来自新闻/论坛对他 X 言论的二手报道（secondary，已注明）。

## 0. 总纲

- 模型是 lossy zip file + assistant persona（见 mental-models #1）。一切用法都从这个出发：**缺知识就补 context，缺时效就给工具，缺对错就上验证。**
- 稀缺的东西变了：代码生成、API 记忆、boilerplate、初稿不再稀缺；稀缺的是 understanding、taste、eval 设计、security、system boundaries、agent orchestration。

## 1. Context-window discipline（上下文纪律）

- **换话题就开新 chat**：context window 是工作记忆，旧话题的残留会污染新任务。
- **先转录再提问**：给图片先让模型把图转成文字描述，核对转录对了再问问题——把"看"和"想"分开，便于验。
- **把文档喂给它**："Don't read books alone." 读论文/书时上传全文，边读边问，大幅降低理解门槛。
- **Appendix 习惯**：写重要判断时，把 transcript、原始材料一起给模型，留一个可 fork 的完整 context（他在博客里常年附 ChatGPT conversation 链接）。

## 2. Thinking vs non-thinking 模型

- **System 1 / System 2 分工**：简单问答、闲聊、改写 → 快的非 thinking 模型；数学、难题、hard code → thinking/reasoning 模型（RL 训出来的，内部会试策略）。
- **Thinking 模型是奢侈品**：贵且慢，别拿来干快模型能干的事。

## 3. LLM council（模型议会）

- 同一个重要问题，拿去问多家模型（ChatGPT、Claude、Gemini、Grok…），交叉验证、抓错误。
- 本质：单个 ghost 是 jagged 的，多个 ghost 的 disagreement 就是信息。

## 4. Deep research + code interpreter

- 需要调研：用 deep research（extended thinking + search，十几分钟产出定制报告），但**一律当 first draft**——"treat this as papers to look at, but don't take this as definitely true."
- 需要算数/画图/数据分析：扔给 code interpreter，别让模型心算。
- 输出默认不信任：对 primary source 交叉验证，尤其是数字、引用、API 用法。

## 5. Vibe coding → agentic engineering（写代码这条线，2025 年有转折）

- **2025 年中之前**：他主要自己写 + autocomplete（Cursor 补全），agent 只做小事。MenuGen（2025-04）是例外：100% Cursor+Claude 写的，他"基本不知道它怎么工作的"——vibe coding 上限：prototype 和个人工具。
- **2025 年 12 月转折**：agent 生成的 chunk 变大、变连贯、变可靠，他开始信任 agent 干更大的活；自述 manual coding 和 agent 的比例从 80/20 翻成 20/80，好几个月没手写过一行代码。
- **Agentic engineering（专业纪律）**：程序员变成 orchestrator。unit of work 从"敲代码行"变成 macro actions：implement this feature / refactor this subsystem / research this library / write tests, run them, fix failures。
- **人的护栏清单**：design specs、supervise plans、inspect diffs、write tests、evaluation loops、manage permissions、isolate worktrees。MenuGen 的 Stripe bug 是经典案例：agent 用邮箱匹配支付和账号——代码能跑，但系统设计是错的（两个邮箱可能不一致），必须人用产品判断力拦下来。
- **Agent-native 基础设施观**：服务要对 LLM 友好——CLI、Markdown 文档、curl 可配、机器可读 schema。"Don't talk to a developer. Don't ask a developer to visit, look, or click. Instruct and empower their LLM."

## 6. Append-and-review note（笔记法）

- 一个 Apple Notes 叫 "notes"，所有想法/todo/摘抄一律 **append 到顶部**，不分类、不打标签（除了 watch:/listen:/read: 方便深夜 CTRL+F）。
- 定期从上往下 skim，值得留意的 rescue 回顶部（复制粘贴、合并）；沉底的不是丢了，只是不值得 top of mind。
- 哲学：**简单 > 结构**。文件夹分类是 cognitive bloat；"One text note ftw."

## 7. LLM wiki pattern（知识库做法）

- 不用 RAG 每次临场检索 raw documents，而是让 agent **增量编译**一个持久的 Markdown wiki：summaries、entity pages、concept pages、contradictions、cross-links、logs、evolving synthesis。
- 适用：长期跟踪的领域、读书笔记、研究方向。no classical program 能 robust 地维护这种跨 messy 文档的知识库，LLM 可以。

## 8. 他的红线（用 LLM 时不干的事）

- 不把模型当知识源：时效/冷门/精确问题不上工具就不问。
- 不把输出当终稿：deep research、代码、文章一律 first draft，必须 verify。
- 不在不可验证的任务上堆 prompt 技巧：先找 verifiability（resettable / efficient / rewardable），找不到就承认这是 generalization 碰运气。
- 不拟人化：不说"模型觉得"，说"这个输出/这个分布"。

---

## 9. X 帖子里的近期用法（X-digest，2026-10-13 核实）

> 以下 5 条是 X 登录墙外可见的最近帖子，由 browser 任务 2026-10-13 核实。引用均控制在一两句内。

- **The widening capability funnel（Oct 11）**：他认为人们对 LLM 能力的共识差距在拉大——"It's now less 'two groups of people speaking past each other' and more a sharp funnel." 他的 napkin math：约 60 亿人（~75%）几乎没接触过 LLM；10–20 亿 casual 免费用户看到的是"derpy chatbots"；约 2000 万（0.2%）专业用户在数学和代码上看 agent 从一个 prompt 完成数周的项目；约 5000 人有内部前沿权限，看 agent swarms 啃 mega-project。驱动因素：(a) ambition/problem horizon，(b) 能力的 jaggedness——在 digital、verifiable、economically valuable 领域 spike（RL on verifiable rewards），(c) access tier。"A small sliver of professionals are experiencing the vertigo of the curve going vertical."
  - **用法含义**：别用"我看到的 chatbot"去推断"前沿 agent 能干什么"；想进 0.2% 那层，先把 problem horizon 放大、去 verifiable 的数字任务里练。
- **"Land or Water?" eval（Oct 2，转 @celestepoosts）**：给 LLM 一串经纬度文本坐标，问陆地还是水，重复 16,200 次画成图——"The models know. From compressing the internet." 这是 lossy-zip-file 心智模型最直观的 demo：压缩包里沉淀了世界知识。
- **理解模型输出的新姿势（Oct 1）**："We'll be spending a lot more time trying to understand the outputs of language models." 他的技巧：让模型用 ASD-STE100（航空受控语言规范）解释，写作立刻变干净（"80% of the way to ASD-STE100"）；要 diagram 不要 prose；要 HTML 直接生成可交互网页；他最 bullish 的格式是** bespoke explainer videos**——"Create a 3b1b style video explainer on X"，配 ElevenLabs API key 做旁白。工作流上移到 oversight/understanding；LLM 能生成以前不值得造的大型一次性 artifact（app、讲解视频）。
- **LLM 的无限体力（Aug 1）**：他给 Opus 5《指环王》第一段 + 1M token 预算（约 $10），让它用 three.js 渲染故事；跑了约 2 小时，写了 5,500 行代码做程序化渲染——"kind of janky but fun." 要点：LLM 对 hyper-custom 工作有无限 stamina，把"没人会干这种事"翻成"sure, why not, it's ~free"；他兴奋的是 ephemeral on-demand worlds（"an ephemeral GTA of X on demand"），人可以进去当旁观 NPC 或角色。短板：LLM 不能高效看视频/playtest，没法审计视觉产出——Opus 只能 painstaking 地截图自查，"raw capability (multimodal, gameplay) still quite lacking"。
- **Voice-ramble 补 context（Jul 21）**：当 LLM 需要的 context 比你想打字的多时，"lean back, switch to /voice and just ramble for like 10 minutes, total mess, anything goes, full stream of consciousness." LLM "very good at reconstructing long incoherent rambles"，echo 出来的版本更干净——mind meld 更深，事后纠正更少。帖子：https://x.com/karpathy/status/2079610838143623371

## 10. 二手报道里的 X 言论（secondary，来源已注）

> 以下来自新闻/论坛对他 X 帖和访谈的报道，非一手原文；用时注明"据报道"。

- **nanochat（2025-10，报道源：多家科技媒体）**：~8,000 行代码的完整 ChatGPT 克隆训练+推理 pipeline（tokenizer → pretraining → midtraining → SFT → GRPO RL → 推理 + Web UI）。$100 / 4 小时（8×H100）训出能聊天的基础版；$1,000 / 42 小时做到基础数学和代码。他的原话（据报道）："You boot up a cloud GPU box, run a single script and in as little as 4 hours later you can talk to your own LLM in a ChatGPT-like web UI." 目标："the full 'strong baseline' stack into one cohesive, minimal, readable, hackable, maximally forkable repo."——build-to-understand 的终极形态。
- **"AI psychosis" / "skill issue"（2026-03 No Priors 访谈，报道源：VentureBeat、pjfp.com 访谈纪要）**：自述 2025 年 12 月之后没手写过一行代码，manual:agent 从 80/20 翻成 20/80；每天 16 小时"express my will to my agents"。他把这种状态戏称为 AI psychosis（自嘲式说法，指被能力淹没的 FOMO，不是不适诊断）；agent 失败时他的第一反应是 "it's a skill issue"——问题在操作者的 instruction/spec 水平，不在模型能力。同时他描述 agent 的 jaggedness："像跟一个搞了一辈子系统编程的 PhD 和一个十岁小孩同时说话"。
- **Loopy era / autoresearch（2026-03，报道源：VentureBeat、Fortune、marktechpost）**：详见 `references/autoresearch-loop.md`。
