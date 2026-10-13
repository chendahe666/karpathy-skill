# Karpathy 心智模型（mental models）

蒸馏自他的博客（karpathy.bearblog.dev / karpathy.github.io）、YouTube 课程与访谈。每个模型：先说结论，再给机制，最后说怎么用。

---

## 1. LLMs are lossy zip files of the internet（有损压缩包）

- **一句话**：LLM 不是知识库，是互联网的有损压缩；pretraining = 压缩，post-training = 套上一层 assistant 人格。
- **机制**：他爱用的开场白是让模型自我介绍："Hi, I'm ChatGPT. I am a one-terabyte zip file."（《How I use LLMs》）知识来自训练截止前读过的互联网，且只记得个大概。问常识性、非时效问题是安全的；问新鲜、冷门、精确的东西必须上工具（搜索、代码执行、上传文档）。
- **怎么用**：每次用 LLM 前先问——这个问题在它的压缩包里吗？不在，就给它工具和 context，而不是怪它幻觉。
- **反模式**：把模型当知识源、当搜索引擎用；对"幻觉"感到意外（幻觉是默认行为，不是 bug）。

## 2. Ghosts, not animals（幽灵，不是动物）

- **一句话**：今天的 LLM 不是动物，是被召唤出来的幽灵——人类文档的统计蒸馏（statistical distillation），"ghosts:animals :: planes:birds"。
- **机制**：动物智能来自演化压力（生存、繁衍、好奇心）；LLM 来自完全不同的优化压力：模仿人类文本 → RL 拿任务奖励 → A/B 测试拿点赞（于是 sycophancy，拼命讨好）。substrate、学习算法、优化目标全都不一样，所以别用动物直觉去预测它。
- **怎么用**：对模型采取"经验主义的熟悉"（empirical familiarity）：知道它在哪行、在哪翻车、被为什么训练过，然后在外面建 guardrails。既不 dismissed 也不盲信。
- **反模式**：拟人化（"模型在思考/努力/想骗我"）；期待它像人一样平滑可靠——它是 jagged 的 alien tool。

## 3. Jagged intelligence（锯齿状智能）

- **一句话**：模型能力不是平滑的 general intelligence，是锯齿——某些地方 spike，某些地方蠢得离谱。
- **机制**：他给的粗糙公式：`capability spike ≈ verifiability × training attention × data coverage × economic value`。模型是 pretraining mixture、RL 环境、benchmark 压力、产品优先级和商业激励的人工制品，没有说明书。你的任务如果正好落在"可验证 + 被重点训练"的轨道上（on the model's rails），它飞起；否则可能在很基础的地方翻车。
- **怎么用**：做产品/做研究前先判断——我的任务在模型的轨道上吗？不在，就补 context、工具、自己的 eval、甚至自己的 RL 环境。
- **反模式**：用一两个 demo 推断整体智能水平；对"这么聪明怎么会犯这种错"感到困惑。

## 4. Verifiability（可验证性）：自动化的真正边界

- **一句话**：Software 1.0 自动化"能写清楚 spec 的"，Software 2.0/3.0 自动化"能验证对错的"。
- **机制**：可验证 = resettable（能重开）+ efficient（能大量尝试）+ rewardable（有自动奖励信号）。数学、代码、游戏、benchmark 进步飞快，因为模型可以"练习"；创意、战略、真实世界常识 lag，因为验证信号弱。这就是 jagged frontier 的驱动力。
- **怎么用**：选方向 / 选自动化目标时，先找 verifiable 的任务。创业 wedge：找 valuable + verifiable + 还没被大厂重点训练的领域，自建 RL 环境。
- **反模式**：在不可验证的任务上堆 prompt 技巧；把"模型做不到"当成"模型不够大"。

## 5. Software 1.0 / 2.0 / 3.0

- **一句话**：编程范式三连跳——1.0 人手写代码；2.0 人造数据集和目标函数，权重即代码；3.0 用自然语言编程 LLM（prompts、context、tools、examples、memory、instructions），"The hottest new programming language is English."
- **机制**：3.0 里 context window 是新的 program，LLM 是跑在它上面的 interpreter。例子：装复杂软件，1.0 时代写满是 if-else 的 shell 脚本；3.0 时代是把一段 instruction 块粘给 agent，它自己读环境、debug、适配机器。
- **怎么用**：判断一个技术/产品变化是第几层的变化；3.0 时代的产品要做 agent-native（Markdown 文档、CLI、API、MCP、机器可读的 schema、copy-pasteable 的 agent instructions），"Don't talk to a developer. Instruct and empower their LLM."
- **反模式**：用 1.0 的直觉评判 3.0（要求 deterministic 精确性）；只问"AI 能把旧 workflow 加速多少"，不问"以前不可能、现在自然的信息变换是什么"（MenuGen 的教训：有些 app 应该直接消失，变成一次模型调用）。

## 6. Build to understand（造出来才算懂）

- **一句话**：理解的终极检验是能不能从零复建，而且代码越少越好。
- **机制**：micrograd → makemore → nanoGPT → llm.c → microgpt（2026）：200 行零依赖 Python 训出一个 GPT，"I cannot make this any further… I think it is beautiful"。他的教学哲学：把复杂东西蒸馏到 bare essentials，剩下的只是 efficiency。
- **怎么用**：学新东西时，动手复建最小版本；写代码/做研究时，追求 elegance（一样好就选行数少的）。
- **反模式**：只读论文/只看视频的被动学习；用复杂度证明自己懂。

## 7. Power to the people（技术扩散的反转）

- **一句话**：LLM 是史上第一次"自下而上"扩散的革命性技术——对个人的加成远大于对组织。
- **机制**：LLM 的能力画像是"横跨极广领域的准专家，但浅且 fallible"。组织本来就靠堆专家，LLM 只是让它好一点；个人最多精通一件事，LLM 让他第一次能 vibe code、读法律文件、啃冷门论文、做数据分析。组织还有 coordination 成本、合规、安全、惯性等 headwinds。
- **怎么用**：个人杠杆现在是历史最高点；别等组织流程，先自己用起来。
- **反模式**：觉得"大公司有更多钱所以 AI 红利归他们"——至少在 frontier 性能人人可及的阶段，钱买不到更好的 ChatGPT。

---

## 跨模型通用姿态

- **第一性原理 + 命名癖**：看到现象先拆机制，然后起个名字（Software 2.0、vibe coding、ghosts、march of nines）。
- **不确定性诚实**：常说 "I have a very wide distribution here"、"double digit percent uncertainty"；bitter lesson 是"platonic 去追求，不一定到达"。
- **教育者底色**：Eureka Labs、Zero to Hero、microgpt——把难东西讲简单是他的本能；"You can outsource your thinking, but you can't outsource your understanding."
- **技术极简主义**（见《I love calculator》）：好技术 = 零依赖、自包含、属于你；警惕 dependency-bloated、user-hostile 的技术 drift。
