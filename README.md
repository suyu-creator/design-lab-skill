<div align="center">

# 🧪 Design-Lab

**The unified analysis & design skill — one command from requirement to closed-loop delivery.**

**三合一统一分析/设计 skill —— 需求 → 设计 → 复用 → 验证 → 闭环，单命令走完**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-4A5568.svg)
![Design](https://img.shields.io/badge/Design-Lab-7C3AED.svg)
![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)

</div>

---

## English · 英文版

### Pain Points

Three separate skills each cover one slice, but they don't connect:

- 🔍 **deep-analysis only "finds projects to reuse" but never knows which problem the reused code actually solves** — its researcher writes shallow reasons like "core logic matches the requirement"; hot repos get reused on a whim, with no sense of what to cut or how to adapt
- 🏗️ **arch-designer / agent-designer have the knowledge but no closed loop** — they own the problem→solution map, pitfalls library, and checklists, but only judge designs; they never go to GitHub to find existing implementations
- 📦 **Three separate installs, three separate maintenance tracks** — install arch for architecture, agent for agents, deep for research; easy to reach for the wrong skill at the wrong time

**Design-Lab merges all three: deep-analysis's research loop as the trunk, arch/agent-designer's problem-mapping + pitfalls injected into the reuse-evaluation step, upgrading reuse from "this repo looks good" to "it solves problem Y with approach X, we also have Y, so keep module Z, cut A/B, patch in countermeasure C".**

### With vs Without

| | Without (three skills in silos) | With Design-Lab |
|---|---|---|
| Flow | Research / architecture / agent each run their own path | One `/design-lab <requirement>` walks the whole closed loop |
| Reuse eval | "Core logic matches requirement" | ★**4 problem-fit questions**: what problem does it solve / do we really have it / is the fix appropriate / how to reuse + **trim list** (keep/cut/adapt) |
| Design order | Research first, integrate later; finding code has no basis | **Design first, find code later**: produce "our problem list", then search GitHub against it |
| Knowledge loading | Pick a skill by gut feeling | **Conditional trigger**: no agent knowledge loaded for non-agent requirements |
| Finding gaps | One-pass by intuition | Pre-simulation (pitfalls library + completeness enumeration) + post-simulation (coverage check) |
| Confirmation | Run the whole pipeline, then come back | **Only 4 confirmation gates** — everything else runs without interruption |

### Core Capabilities

- **One-command closed loop**: clarification → domain design → GitHub reuse evaluation → pre-simulation → feasibility integration → closure report (draft + post-sim walkthrough) → CLOSURE.md
- **★Design first, find code later**: Step 2 uses arch/agent knowledge to design "our problem list"; Step 3 searches GitHub with that list in hand — this is what makes problem-fit judgment possible
- **★4 problem-fit questions + trim list**: read actual code (not just the README) to judge what problem the candidate solves, whether we really have it, whether the fix matches the pitfalls library, and how to reuse; every cut must state "it solves a problem we don't have"
- **Conditional agent knowledge**: agent-flow loads only when the requirement involves agents (agent/AI assistant/tool use/multi-agent/RAG), otherwise skipped
- **9 architecture patterns + 30+ agent design patterns**: each covers "what problem / what it looks like / cost / when NOT to use"
- **Merged pitfalls library — 65 entries**: 23 architecture (#01-#23) + 42 agent (#A01-#A42) + real incident files, each = pit → consequence → countermeasure
- **5-group review checklist + delivery gates**: consistency / resilience / scale / security / AI-specific; no CRITICAL → no delivery
- **31 real-system template map + dual simulation**: templates as design starting points; pre-simulation finds gaps (pitfalls library + Actor enumeration + Premortem completeness check), post-simulation verifies coverage
- **★Backend deep-water hooks**: cache/MQ problem→solution groups (penetration/breakdown/avalanche/consistency/backlog) + **living-source deep-dive** (fetch real tutorials/cases on demand) + **build-level findings** (conclusions must land on "pattern + params + pitfalls", noun-level gets sent back) + **quantified hard standards** (every perf/concurrency claim carries numbers)
- **★Multi-agent orchestration protocol**: topology three-way fork (star / hierarchical handoff / event-driven mesh) + 5-field task card (goal/boundary/tools+permissions/acceptance/budget) + 3-part result report (conclusion + key evidence + trace reference, no full-text) + arbitration rules (non-overlapping write ranges, coordinator decides conflicts)
- **Per-domain experience vault**: separate `arch` / `agent` / `general` libraries — it gets to know you better over time (local-private, never pushed)

### How It Works

One command, with only **4 confirmation gates** — everything else runs without interruption:

```
/design-lab <requirement>
  ├─ Step 1  Clarification (adaptive) + path choice ── user answers ①
  │          Few/no questions if clear; 1-2 key ones if vague
  ├─ Step 2  Domain design (design first) ★produces "our problem list"
  │          ├─ arch-flow loaded by default (skipped only for tiny projects) → estimation/patterns/structure/resilience
  │          └─ With agents       → also agent-flow (BDD/patterns/structure/guardrails)
  │          ── ⚠️ gate ②: "Draft + problem list OK?"
  ├─ Step 3  researcher → pre-simulation → integration (continuous, no stops)
  │          ├─ researcher: GitHub reuse + 4 problem-fit questions + trim list
  │          ├─ Pre-simulation: gap scan + completeness enumeration (reads shared pitfalls library)
  │          └─ Integration: reuse Top3 + gaps Top5 + verdict ── ⚠️ gate ③
  └─ Step 4  Draft → post-sim walkthrough → shown together ── gate ④ → write CLOSURE.md
```

**Key design**: design first (know your own problems) → find code (is this repo solving our problem?) → pre-simulate (find gaps + completeness enumeration) → post-simulate (verify coverage). Requirements may change at any moment — pause and roll back to the matching Step.

### Install

```bash
# Clone
git clone https://github.com/suyu-creator/design-lab-skill.git

# Copy to Claude Code skills dir
cp -r design-lab-skills/skills/design-lab ~/.claude/skills/
```

Or manually drop the `skills/design-lab/` folder into `~/.claude/skills/` and restart Claude Code.

**Trigger**: just say "I want to build an XX system/platform", "design an AI customer-service agent", "research and design this", or explicitly `/design-lab <requirement>`. Simple requirements (single page / single API / copy change) are not triggered — you get a one-line plan instead.

### Hard Rules

1. ⚠️ **Only complex requirements trigger** — simple ones get a one-line plan
2. ⚠️ **Reuse first** — "research" path searches GitHub first; "direct plan" path relies on experience (labeled "LLM guess")
3. ⚠️ **Every reuse candidate passes the 4 problem-fit questions + trim list**; gaps without reuse must get a solution, never just listed
4. ⚠️ **No agent knowledge loaded for non-agent requirements** (conditional trigger)
5. ⚠️ **Every claim is sourced** — GitHub code (repo@file:line) or "LLM guess"
6. ⚠️ **Requirement change = pause** — record → assess impact → roll back to matching Step → continue after user confirms
7. ⚠️ **Ask, then stop** — always wait for the answer before advancing

### Module Index

| Need | Look here |
|------|-----------|
| Entry point + Step 1-4 orchestration + hard rules | `SKILL.md` |
| Step 2 architecture design (estimation/patterns/structure/extension points/resilience) | `flows/arch-flow.md` |
| Step 2 agent design (conditional) | `flows/agent-flow.md` |
| Step 3 research + ★problem-fit + trim list | `stages/researcher.md` |
| Step 3 pre-simulation (gap scan + completeness enumeration) | `stages/simulator.md` |
| Step 4 closure draft + post-simulation + write file | `stages/closure.md` |
| 9 architecture patterns + selection mnemonic | `references/01-patterns.md` |
| 30+ agent design patterns | `references/02-agent-patterns.md` |
| ★Merged problem→solution map | `references/03-problem-solution-map.md` |
| Merged pitfalls library (65 + incidents) | `references/04-pitfalls.md` |
| Merged checklists + review + delivery gates | `references/05-checklists.md` |
| 31 real-system template map | `references/06-templates-map.md` |
| Agent templates (prompt/schema/test) | `references/07-agent-templates.md` |
| Architecture methods (8-step/envelope/ADR) | `references/08-design-method.md` |
| Agent production delivery (eval/guardrails/monitoring) | `references/09-delivery.md` |

### Provenance

- **[Merged]** From three published skills: **deep-analysis** (research-loop trunk) + **arch-designer** (architecture knowledge: 9 patterns / 11 pitfalls / incidents / 31 templates) + **agent-designer** (agent knowledge: 20+ patterns / 38 pitfalls / delivery) — the original repos remain independent
- **[Literature]** awesome-architecture, Anthropic《Agentic Design Patterns》, official post-mortems
- **[Practice]** per-domain experience vaults (local-private, U01+)

### FAQ

**Q: Relationship to the original three skills?**
A: This replaces them. Installing design-lab means you no longer need deep-analysis / arch-designer / agent-designer. The three original repos stay independent.

**Q: When does it trigger?**
A: Complex requirements only (multi-module / architecture decisions / agents / research needed / scale-concurrency-security risks / vague scope). Simple ones get a one-line plan.

**Q: Does it write business code?**
A: No. It outputs plans / designs / research reports / a closure document (CLOSURE.md) with a reuse work order and a build-from-scratch work order. Implementation belongs to the development phase.

**Q: Why "design first, find code later"?**
A: Only after knowing your own problems (Step 2 problem list) can you judge whether a candidate repo solves your problem — a hot repo ≠ the right repo. Hit the problem → keep; miss it → cut.

**Q: How do I add hands-on experience?**
A: Say "加入经验库" (add to experience vault) after a project; it walks through 5 questions (scenario/pit/consequence/countermeasure/source) and writes to the matching domain vault. Auto-read at the start of every design (missing file → skipped gracefully).

**Q: Will git pull overwrite my experience?**
A: No. The repo carries **zero `experience/` files** — the vault is a local-private mechanism, git-ignored so your entries can never be pushed. `git pull` updates the skill without touching your local vault; create `experience/arch.md` / `agent.md` / `general.md` locally to start (the format template is in each file's header comment).

---

## 中文版 · Chinese

### 痛点

三个 skill 各管一段，配合有洞：

- 🔍 **deep-analysis 只会"找项目复用"，不知道复用的是哪个问题的解法** —— researcher 写理由只会写"核心逻辑匹配需求"，热门 repo 就拿过来，砍不砍、怎么改全靠感觉
- 🏗️ **arch-designer / agent-designer 有知识没闭环** —— 有 问题→方案映射、踩坑库、检查清单，但只做判断，没有"去 GitHub 找现成实现"的调研闭环
- 📦 **三套独立安装、独立维护** —— 架构需求装 arch、agent 需求装 agent、调研需求装 deep，还容易在错误的时刻用错 skill

**Design-Lab 把三合一：以 deep-analysis 的调研闭环为主干，把 arch/agent-designer 的问题映射 + 踩坑库注入"复用评估"，让复用从"这项目不错"升级为"它用 X 解决了 Y 问题，我们也有 Y，所以保留 Z 模块、砍掉 A/B、补上 C 对策"。**

### 装了 vs 没装

| | 没装（三个 skill 分开用） | 装了 Design-Lab |
|---|---|---|
| 流程 | 调研/架构/智能体三套各走各的 | 单命令 `/design-lab <需求>` 走完整闭环 |
| 复用评估 | 只写"核心逻辑匹配需求" | ★**问题拟合四问**：它解决什么问题/我们真有吗/解法对症吗/怎么复用 + **裁剪清单**（保留/砍/改） |
| 设计顺序 | 先调研后整合，找码没依据 | **先设计后找码**：先产出「我们的问题清单」，再带问题找复用 |
| 知识加载 | 凭感觉选 skill | **条件触发**：无智能体的需求不加载 agent 知识 |
| 找遗漏 | 靠经验扫一遍 | 前模拟（踩坑库 + 完整性穷举）+ 后模拟（覆盖验证） |
| 确认 | 一口气跑完才回头 | **全程只停 4 次**，其余连续跑完不打扰 |

### 核心能力

- **三合一一键闭环**：需求澄清 → 领域设计 → GitHub 复用评估 → 前模拟 → 可行性整合 → 闭环报告（草案+后模拟走查）→ 写 CLOSURE.md
- **★先设计后找码**：Step 2 用 arch/agent 知识先设计出「我们的问题清单」，Step 3 才带着问题去 GitHub——问题拟合才有依据
- **★问题拟合四问 + 裁剪清单**：从代码实读（不只看 README）判断候选 repo 解决什么问题、我们需求里真有没有、解法是否对症踩坑库、怎么复用；每处裁剪都要说"因为它解决的是我们没有的问题"
- **条件触发 agent 知识**：需求含智能体（agent/AI 助手/工具调用/多智能体/RAG）才加载 agent-flow，否则不加载，避免无关内容
- **9 大架构模式 + 30+ 智能体设计模式**：每个含"解决什么问题/长什么样/代价/什么时候别用"
- **合并踩坑库 65 条**：架构 #01-#23（23 条）+ 智能体 #A01-#A42（42 条）+ 真实事故档案，每条 = 坑 → 后果 → 对策
- **5 组审查清单 + 交付门禁**：一致性/韧性/规模/安全/AI 特有，无 CRITICAL 才能交付
- **31 个真实系统模板地图 + 双模拟**：模板当设计起点；前模拟找遗漏（踩坑库 + Actor 穷举 + Premortem 完整性检查）、后模拟验覆盖
- **★后端深水区钩子**：缓存/MQ 问题→方案组（穿透/击穿/雪崩/一致性/积压）+ **活源深挖**（按需 fetch 真实教程/案例）+ **施工级结论**（必须落"模式+参数+坑"级，名词级打回）+ **量化硬标准**（性能/并发声明必带数字）
- **★多智能体编排协议**：拓扑三岔路（星型/层级 handoff/事件驱动 mesh）+ **任务卡 5 字段**（目标/边界/工具权限/验收/预算）+ **回传三件套**（结论+关键证据+trace 引用，禁全文）+ **仲裁两规则**（写入范围不重叠、协调者裁决）
- **分领域经验库**：arch / agent / general 三库独立沉淀，越用越懂你（本地私有，永不进 git）

### 工作原理

单命令走完，**固定停 4 个决策点**，其余连续跑完不打扰：

```
/design-lab <需求>
  ├─ Step 1  需求澄清(自适应) + 路径选择 ── 用户答 ①
  │          需求清楚少问甚至不问；模糊才问 1-2 个关键问题
  ├─ Step 2  领域设计(先设计后找码) ★产出「我们的问题清单」
  │          ├─ 默认加载 arch-flow（仅极小项目跳过）→ 场景估算/模式选型/结构/韧性
  │          └─ 含智能体    → 追加 agent-flow（BDD/模式/结构/护栏测试）
  │          ── ⚠️ 收尾 ②：「设计草案+问题清单 OK 吗？」
  ├─ Step 3  researcher → 前模拟 → 整合 连续执行（中间不停）
  │          ├─ researcher: GitHub 找复用 + 问题拟合四问 + 裁剪清单
  │          ├─ 前模拟: 遗漏扫描 + 完整性穷举(预读共享踩坑库)
  │          └─ 整合: 复用Top3 + 遗漏Top5 + 裁决 ── ⚠️ 收尾 ③
  └─ Step 4  草案→后模拟走查→一起呈现 ── 收尾 ④ ──→ 写 CLOSURE.md
```

**关键设计**：先设计（知道自己有哪些问题）→ 再找码（判断候选解决的是不是自己的问题）→ 前模拟（找遗漏 + 完整性穷举）→ 后模拟（验覆盖）。需求变更随时暂停、回退对应 Step。

### 安装

```bash
# 克隆到本地
git clone https://github.com/suyu-creator/design-lab-skill.git

# 复制到 Claude Code skills 目录
cp -r design-lab-skills/skills/design-lab ~/.claude/skills/
```

或者手动把 `skills/design-lab/` 整个文件夹放进 `~/.claude/skills/`，重启 Claude Code 即生效。

**触发**：直接说「我要做 XX 系统/平台」「帮我设计一个 AI 客服」「这个方案帮我调研+设计」，或显式 `/design-lab <需求>`。简单需求（单页面/单接口/改文案）不触发，直接给 1 句方案。

### 硬规则

1. ⚠️ **复杂需求才触发** —— 简单需求直接给 1 句方案
2. ⚠️ **复用优先** —— 选「先调研」先搜 GitHub，有现成直接引用，不从零想；选「直接给」凭经验（标「LLM 推测」）
3. ⚠️ **复用候选必须过「问题拟合四问」+ 出裁剪清单**；无复用的坑必须写解决方案
4. ⚠️ **无智能体的需求不加载 agent 知识**（条件触发）
5. ⚠️ **每条声明标注来源** —— GitHub 代码(repo@文件:行号) 或 「LLM 推测」
6. ⚠️ **需求变更即停** —— 暂停 → 记录 → 评估影响 → 回退对应 Step → 用户确认后继续
7. ⚠️ **问完必停** —— 每次提问后停下等回答，得到答复前绝不推进下一步

### 模块导航

| 需要什么 | 查哪里 |
|---------|--------|
| 调度入口 + Step 1-4 编排 + 硬规则 | `SKILL.md` |
| Step 2 架构设计（估算/选型/结构/扩展点/韧性） | `flows/arch-flow.md` |
| Step 2 智能体设计（条件触发） | `flows/agent-flow.md` |
| Step 3 调研 + ★问题拟合四问 + 裁剪清单 | `stages/researcher.md` |
| Step 3 前模拟（遗漏扫描 + 完整性穷举） | `stages/simulator.md` |
| Step 4 闭环草案 + 后模拟 + 写文件 | `stages/closure.md` |
| 9 大架构模式 + 选型口诀 | `references/01-patterns.md` |
| 30+ 智能体设计模式 | `references/02-agent-patterns.md` |
| ★合并问题→方案映射（架构5组+智能体14类） | `references/03-problem-solution-map.md` |
| 合并踩坑库（65 条 + 事故） | `references/04-pitfalls.md` |
| 合并检查清单 + 评审标准 + 交付门禁 | `references/05-checklists.md` |
| 31 真实系统模板地图 | `references/06-templates-map.md` |
| agent 模板（prompt/schema/测试） | `references/07-agent-templates.md` |
| 架构方法（八步/信封估算/ADR/演进） | `references/08-design-method.md` |
| agent 生产交付（评估/护栏/监控） | `references/09-delivery.md` |

### 经验来源

- **[合并]** 由三个已发布 skill 合并而成：**deep-analysis**（调研闭环主干）+ **arch-designer**（架构知识：9 模式/11 坑/事故/31 模板）+ **agent-designer**（智能体知识：20+ 模式/38 坑/交付），原仓库各自保留独立
- **[文献]** awesome-architecture、Anthropic《Agentic Design Patterns》、各官方事后分析等提炼
- **[实战]** 分领域经验库（本地私有，编号 U01 起）

### FAQ

**Q: 和原来的三个 skill 什么关系？**
A: 合并替代。装 design-lab 后不需要再装 deep-analysis / arch-designer / agent-designer。三个原仓库保留独立（各自有专门用途），后续可迁移。

**Q: 什么时候触发？**
A: 复杂需求才触发（多模块/需架构决策/涉及智能体/需调研/存在规模并发安全风险/范围模糊）。简单需求（单页面/单接口/改文案）直接给 1 句方案。

**Q: 它会不会自己写业务代码？**
A: 不写。它输出的是**方案/设计/调研报告/闭环文档**（CLOSURE.md），包含复用施工单和自研施工单，落地实现交给开发阶段。

**Q: 为什么"先设计后找码"？**
A: 只有先知道自己系统有哪些问题（Step 2 问题清单），才能在 Step 3 判断候选 repo 解决的是不是自己的问题——热门 repo ≠ 适合我们，命中问题才保留，没命中就砍。

**Q: 怎么把实战经验加进去？**
A: 做完项目说一句「加入经验库」，按 5 问引导（场景/坑/后果/对策/来源）写进对应领域经验库。下次设计开始自动读取参考（文件缺失则跳过，不影响运行）。

**Q: git 更新 skill 会不会覆盖我的经验？**
A: 不会。仓库**不包含任何 `experience/` 文件**——经验库是本地私有机制，已被 `.gitignore` 排除，你的经验永远不会进 git、不会被误推送；`git pull` 更新 skill 完全不动本地经验库。首次使用按格式在本地自建 `experience/arch.md` / `agent.md` / `general.md`（文件头注释即模板）。

---

## License

[MIT](LICENSE) © suyu-creator
