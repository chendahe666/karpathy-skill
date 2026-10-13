# Overnight Loop：无人值守的实验循环

**来源标注**：设计细节来自他的公开讲解（primary：No Priors 访谈 2026-03、autoresearch repo README 思路）；运行数字（700 experiments、11% 等）来自 VentureBeat/Fortune/marktechpost 的二手报道（secondary），已注明。

## 1. The loop（"Karpathy Loop"）

核心思想一句话（据他在 X 上的原话意）："The goal is to engineer your agents to make the fastest research progress indefinitely and without any of your own involvement."——把科研变成一个 agent 自己转的循环，人睡觉，早上收实验日志。

### 三文件分权（separation of powers）

| 文件 | 谁改 | 作用 |
|---|---|---|
| `program.md` | **只有人改** | 自然语言指令，他称之为 "essentially a super lightweight 'skill'"——config as prose |
| `train.py` | **agent 改** | 模型结构、超参、optimizer——agent 随便改 |
| `prepare.py` | **谁都不改** | 固定的裁判/评估 harness；agent 碰不到，防止它把测试改简单而不是把模型改好 |

### 硬规则

1. **固定时间盒**：每个实验 exactly 5 分钟 wall-clock。一晚上 ~100 个实验，两天 ~700 个。时间盒让实验可比，也防止单个 run 吞掉所有资源。
2. **Single metric**：`val_bpb`（validation bits per byte），越小越好，vocab-size-independent，所以改架构也能公平比。**Agent 不需要懂"好代码"长什么样，只需要一个上下波动的数字。**这是整个设计能 work 的原因——verifiability（见 mental-models #4）。
3. **Keep or kill**：metric 变好就保留，变差就 revert，没有中间态。
4. **Elegance**：打平就选代码行数少的（fewer lines wins ties）。
5. **Anti-"cagy agent"**：显式逼 agent 做大胆改动，防止它只做安全的微调糊弄 metric。
6. **Never wake the human**：半夜不许问问题；歧义就选更大胆的解释，log 下来早上再看。

### 跑出来的结果（secondary 报道）

- 一晚上 126 个实验，loss 从 0.9979 降到 0.9697。
- 两天 ~700 个实验，找到 ~20 个可叠加的真优化，放大到大模型依然有效；"Time to GPT-2" 从 2.02h 降到 1.80h（11%）。
- 有些发现是人想不到的（比如 QK Norm 和 RoPE 的重排）。
- Shopify CEO Tobi Lütke 拿去跑了一夜：37 个实验，19% 提升。
- 他的结论：**if you have an objective metric, you are the bottleneck.** 人类一晚上能手动试十几个实验，loop 不会累。

## 2. 为什么这个设计是 Karpathy 式的

- **Verifiability 的极致应用**：mental-models #4 说 2.0/3.0 自动化"能验证的"——这个 loop 就是把科研压缩成"可验证"的那部分，然后让 agent 在上面 RL 式地转。
- **Minimalism**：整个东西 ~630 行，一个 GPU 就能跑。先让最小版本转起来。
- **Config as prose**：`program.md` 是 Software 3.0 的缩影——用自然语言给 agent 写"程序"（对应 idea-digger skill 里 brief.md 的设计）。
- **分布式愿景**（secondary）：他说 autoresearch 应该像 SETI@home 那样异步大规模协作——目标不是模拟一个 PhD 学生，是模拟一个 research community。

## 3. Agentic engineering：白天的另一面

Loop 是夜里跑的；白天他是 orchestrator（2025-12 转折之后）：

- **工作方式**：多个 coding agent（Claude Code、Codex 之类）同时跑在不同 repo，每个 agent 领一个约 20 分钟的 well-scoped 任务；人负责 delegate → review → 下一个。
- **新的稀缺资源**：以前盯 GPU 利用率，现在盯 **token throughput**——"if you're not using up your subscription credits, you're not maximizing output."（secondary 报道）
- **Skill issue 哲学**：agent 没干好，第一反应是检讨自己的 instruction/spec 写得烂，而不是怪模型。失败 → 改 program（指令），不是改模型。
- **接受 jaggedness**：同时把它当 PhD 和十岁小孩——强的部分放手，弱的部分（视觉审计、playtest、system design 判断）人来。
- **AGENTS.md 模式**：给每个 repo 配 agent 指令文件（instructions as code），这是 Software 3.0 "context 即程序"在工程里的落地。

## 4. 套用模板（给用户建议时直接用）

任何"让 agent 替我干 X"的任务，按这个清单搭：

1. **Judge 先行**：有没有一个自动化的好坏信号？没有就先造 eval，没有 eval 就别谈自动化。
2. **三权分立**：人写指令（program）、agent 改执行（train）、裁判固定（prepare）。
3. **时间盒**：固定 wall-clock 预算，超时即停，防止资源黑洞。
4. **Single metric**：一个数字定生死；tie-break 用 elegance。
5. **Never wake the human**：歧义时 agent 自己选最大胆的，log 备查。
6. **早上只看日志**：keep list + kill list + 每个 kill 的一句话原因。
