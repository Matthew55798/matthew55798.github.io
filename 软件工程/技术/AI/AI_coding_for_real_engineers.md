# AI Coding for Real Engineers

## 一 约束

1. 上下文成本二次方增长
2. 智能区 / 愚蠢区
3. 不是数据库、是模糊 JPEG
4. 知识截止
5. 无状态
6. 非确定性

## 二 工具与上下文

### 2.1 干净的上下文

处理上下文与记忆的办法：`/clear`、`/handoff`、`/compact`，以及把探索分给子代理。
**要优化的是"干净的上下文"，不是"记得多的上下文"**

```text
新建会话  >  handoff 分流  >  compact 压缩  >  一路自动压缩
```

### 2.2 子代理

代码库探索不足会让代理不了解代码库、实现质量下降。探索多导致智能区减少，规划与实施阶段质量下降。

解法是**子代理（subagent）**：由主代理新建一个**独立上下文窗口**，给它定制系统提示去干活，**只在子代理自己的窗口里大量烧 token，最后只把摘要交回主代理**。

- 可并行开多个；子代理可以使用不同系统提示与**不同（更便宜的）模型**。

![Orchestrator 主代理调度 Subagent 1 / Subagent 2，各自独立上下文窗口](/img/技术/ai-engineering/orchestrator-subagent-loop.png)

### 2.3 代码库探索

| 问法 | 结果 |
| --- | --- |
| "告诉我这个仓库的技术栈" | **不会**派探索子代理，全程只读了 6 个文件，分析不完整 |
| "**探索**这个仓库里 X 的原理" | 立即派探索子代理，读入大量文件、调用多个工具，耗时约 60 秒 |

### 2.4 清除优先 - 自动压缩的沉积物问题

`/context` 里能看到上下文末尾有一段**自动压缩缓冲区（auto-compact buffer）**，约 3.3k token的预留区：用量到时，自动触发一次 `compact`。（P26）

每次回退并压缩，都会在上下文里留下一层沉积物；下次再顶到自动压缩区，再留一层。层层叠加会以不可预测的方式影响输出。每次与"上下文为空"的代理开始会话，输出都更可预测。

### 2.5 /handoff - 把独立小任务剥出去

**主线任务还没做完，中途冒出另一件事**（要改的文件、要修的失败测试）。这时既不该污染当前会话，也不该清掉正在做的任务。（P27）

`handoff`（交接）的做法是：把当前会话压缩成**一份临时 Markdown 交接文档**，路径由 `mktemp` 生成，用于跨代理、跨会话交接。

典型用法：实施中测试挂了 → "把修这个测试交接出去"；grilling 会话太长 → 把需要深入的部分转到新会话；做原型/研究 → 把成果浓缩成交接文档。

## 三 记忆与技能

模型无状态，所以"记忆"必须外置。课程把这件事分成三层：**常驻记忆（CLAUDE.md / AGENTS.md）→ 按需指令（技能 SKILL.md）→ 自动积累（auto-memory）**。

### 3.1 CLAUDE.md 与 AGENTS.md 的关系

**它们本质是同一个东西**，区别只在 Claude Code 只听 `CLAUDE.md`。（P28）

### 3.2 CLAUDE.md 的真实定位 - 一张地图，不是仓库

1. **放进去的内容必须对每一次请求都相关**。只能处理前端代码时，位置参数、service 约定、数据库 ID、时间戳这些指令完全用不上，却每次都占着 token。演示里一个 2000 行的记忆文件占了约 **10%** 的上下文。
2. **它有可能被忽略**。Claude Code 会注入系统提示告诉模型：你的 `CLAUDE.md` 已作为提醒放在用户消息里，"此上下文可能相关也可能无关，除高度相关外不应回应"。
3. **不要用 `/init` 自动生成**。它会把代理自己就能发现（如 `package.json` 里的 scripts）、且很快过时的内容写进去，还会因为忘记同步而失真。
4. **运行中改 `CLAUDE.md` 不会立即生效**，要等新会话；想保留当前上下文，就明确要求它"按更新后的 `CLAUDE.md` 重新审查你的工作"（代价是始终处在智能区边缘）。

### 3.3 渐进式披露

**并非所有指令都适用于每个请求**。正确的做法是**每个分组一个独立 markdown 文件，`CLAUDE.md` 里只留指向它们的链接**。
SKILL - 渐进式披露的载体。
技能文件 `SKILL.md` 顶部的 YAML frontmatter：

```yaml
---
name: 技能名
description: 何时使用此技能、解决什么问题（触发条件写在这里）
---
```

### 3.4 把编码规范改造成技能

思路是"**用技能来写技能**"：调用一个"编写技能"的技能，要求它把 `CLAUDE.md` 里的规范找到渐进式披露的落地方式。产出结构（P33–P34）：

```
.coding-standards/
├── SKILL.md                 # 简短，指向下面的参考文件
└── references/
    ├── database.md          # 数据库相关规范
    ├── frontend-ui.md       # 前端 UI / 根目录与表单
    ├── services-and-testing.md
    └── ...
```

### 3.5 自动记忆

**每隔几周**检查一次 `memory.md`，删掉模型本可自行解决的问题，清掉奇怪与冗余的条目。

### 3.6 大任务

任务一旦大到装不进智能区甚至模型上下文，解法还是规格文档与计划文档：

| 文档 | 别名 | 回答的问题 |
| --- | --- | --- |
| **PRD / 规格说明** | 目标描述 | "我们要到哪去" |
| **plan（计划）** | 旅程描述 | "怎么走过去，分几步" |

![PRD.md 是目的地、PLAN.md 是旅程，两者按阶段推进](/img/技术/ai-engineering/prd-plan-smart-phases.png)

下发给代理的提示固定包含三要素：**执行第 n 阶段 + PRD + 完整计划**。

## 四 需求与计划

### 4.1 先判断 - 这个任务需不需要对齐

| 情况 | 要不要 grill | 理由 |
| --- | --- | --- |
| 任务非常明确、你知道要改哪里、能把它完整描述出来 | **不要** | 此时 grill 会过度，直接做 |
| 需求模糊、设计概念还没对齐（"我也想不清到底要什么"） | **要** | 拷问的本质就是**和 AI 对齐** |
| 涉及外观/交互的决策 | 文字不够，要先做原型 | 涉及外观的决策光靠文字答不了 |

**检查清单**

- [ ] 需求真的模糊吗？明确的任务直接做，不要走仪式。
- [ ] 需要看效果才能决策的，是否先做原型，而不是硬用文字讨论外观？
- [ ] 是否做到"一次一个决策"，并主动采纳/否决它的推荐答案？
- [ ] 面对不可逆决策，是否问过"哪个方案最难回退"？

### 4.2 grill-me / grill-with-docs 的工作方式

**`grill-me`（现 `grill-with-docs`，底层共用 `grilling` 原语）的工作方式**（P38）：

- 一轮**对抗性提问**：一次可能上百个问题，直到共识达成。
- **一次只问一个问题**，逐层推进设计树的每个分支，并逐一解决决策之间的依赖关系。
- **自己给推荐答案**，最喜欢你直接采纳或否决，而不是列 a/b/c 让你猜。
- **代码库能回答的问题，自己去探索**。

### 4.3 write-a-prd - 把对话固化成目标文档

写完（或问到共识）之后调用 `write-a-prd`（现 `to-spec`）（P37–P38）：综合当前对话，先探索代码库了解现状，再按模板落文档。

PRD 的模板结构：

| 段落 | 内容 |
| --- | --- |
| 问题陈述 | 从用户视角描述现在面临的问题 |
| 解决方案 | 同样从用户视角描述要做什么 |
| **编号的用户故事列表** | 谁 + 做什么 + 为什么；每条可在拆任务时逐条引用 |
| 主要模块 | 需要新增/修改的模块（这一节是后加的"预防层"） |
| 技术决策 | 依赖、模式、路由等**不易变更**的决策 |
| 测试决策 | 测试写在哪、怎么 mock、是否用测试库 |
| **超出范围（out of scope）** | 明确不做什么，防止文档越界 |
| 备注 | 人工补充的注记 |

### 4.4 把程序设计移到 PRD

预防架构界别的过渡设计或者错误设计

### 4.5 计划 - 垂直切片（曳光弹）与多阶段计划

错误示例：** 提示：`@` 引入 PRD → "把它转化为多阶段计划，保存为本地 markdown 文件"，拆成四个阶段：

| 阶段 | 内容 | 问题 |
| --- | --- | --- |
| 阶段 1 | 完整的分析服务（含测试） |
| 阶段 2 | 后端架构 + 共享仪表盘组件 + 安装 `recharts` |
| 阶段 3 | 在根目录真正使用该 UI |
| 阶段 4 | 管理员分析路由与用户页面链接 |

问题清单：

1. **横向（水平）展开**：按"数据库 → API → 前端"逐层做完，导致**直到后期阶段才能拿到"整个系统能不能工作"的反馈**。
2. **写死了实现信息**：具体函数名、步骤详实到像实施指南，早期阶段一改实现方式就全部过时。
3. **没有引用 PRD 的用户故事**，无法把阶段链接回需求。
4. 阶段量估算合理、整体清晰，但**第三、四阶段重复**。

![LLM's Code Horizontally：按 Phase 逐层横向铺开，到后期阶段才能验证整个系统](/img/技术/ai-engineering/llm-codes-horizontally.png)

**追踪子弹（曳光弹）与垂直切片 **tracing bullet**。概念出自《程序员修炼之道》（The Pragmatic Programmer）：

> 二战时机枪每几发装填一发曳光弹，发光的轨迹让防空炮手看见弹道并据此修正射击。寓意就是**尽早反馈、频繁反馈**。

系统的层次（数据库、API、前端）必须**集成**才能构成可用系统，而 LLM 在设计计划时总是按层思考，于是反馈来得太晚。改法是让**每个阶段都穿透每一层**，这还有一个别名：**垂直切片（vertical slice）**。

![Use Tracer Bullets：每个阶段纵向穿透所有层，尽早集成、频繁反馈](/img/技术/ai-engineering/use-tracer-bullets.png)

为什么有效：每个阶段都贯穿全栈，**人类可以在每个阶段结束后介入查看并反馈**，架构失误的概率明显下降。而且代理本来就懂这个概念，你只要说出"使用曳光弹"再加一句简短解释即可。

## 五 实现与验收

### 5.1 反馈循环 - 让代理能自己判断对错

```text
生成代码 → 检查（类型/测试/格式） → 修正 → 再生成 → …直到质量达标
```

### 5.2 红绿重构 - 让实现这一步提升质量

技巧来自 Kent Beck《极限编程实践》的 **red-green-refactor**（测试驱动开发的核心）。三个步骤：

| 步骤 | 做什么 |
| --- | --- |
| 🔴 红 | 先写一个**会失败的测试**并**运行它确认失败** |
| 🟢 绿 | 写**最小实现**让测试通过 |
| 🔁 重构 | 整理代码、提升质量，期间持续跑反馈循环 |

**为什么对 AI 尤其关键**：

- 它**迫使生成的代码可测试**，而可测试就意味着便于修改（测试会捕获错误）；
- **失败测试较难伪造**。

#### 承载它的 `tdd` 技能

```markdown
# Test-Driven Development

TDD is the red → green loop. This skill is the reference that makes that loop produce tests worth keeping: what a good test is, where tests go, the anti-patterns, and the rules of the loop. Every section applies on every cycle — consult them before and during the loop, not after.

When exploring the codebase, read `CONTEXT.md` (if it exists) so test names and interface vocabulary match the project's domain language, and respect ADRs in the area you're touching.

## What a good test is

Tests verify behavior through public interfaces, not implementation details. Code can change entirely; tests shouldn't. A good test reads like a specification — "user can checkout with valid cart" tells you exactly what capability exists — and survives refactors because it doesn't care about internal structure.

See [tests.md](tests.md) for examples and [mocking.md](mocking.md) for mocking guidelines.

## Seams — where tests go

A **seam** is the public boundary you test at: the interface where you observe behavior without reaching inside. Tests live at seams, never against internals.

**Test only at pre-agreed seams.** Before writing any test, write down the seams under test and confirm them with the user. No test is written at an unconfirmed seam. You can't test everything — agreeing the seams up front is how testing effort lands on the critical paths and complex logic instead of every edge case.

Ask: "What's the public interface, and which seams should we test?"

When the shape of that interface is itself in question — how deep the module is, where the seam belongs, what the interface should expose — use the `/codebase-design` skill for the vocabulary. It is the shared source of the module, interface, depth, seam, adapter, leverage and locality terms, and it is a reference to consult, not a session to run.

## Anti-patterns

- **Implementation-coupled** — mocks internal collaborators, tests private methods, or verifies through a side channel (querying the database instead of using the interface). The tell: the test breaks when you refactor but behavior hasn't changed.
- **Tautological** — the assertion recomputes the expected value the way the code does (`expect(add(a, b)).toBe(a + b)`, a snapshot derived by hand the same way, a constant asserted equal to itself), so it passes by construction and can never disagree with the code. Expected values must come from an independent source of truth — a known-good literal, a worked example, the spec.
- **Horizontal slicing** — writing all tests first, then all implementation. Bulk tests verify _imagined_ behavior: you test the _shape_ of things rather than user-facing behavior, the tests go insensitive to real changes, and you commit to test structure before understanding the implementation. Work in **vertical slices** instead — one test → one implementation → repeat, each test a **tracer bullet** that responds to what the last cycle taught you.

## Rules of the loop

- **Red before green.** Write the failing test first, then only enough code to pass it. Don't anticipate future tests or add speculative features.
- **One slice at a time.** One seam, one test, one minimal implementation per cycle.
- **Refactoring is not part of the loop.** It belongs to the review stage (see the `code-review` skill), not the red → green implementation cycle.
```

### 5.3 `implement` 技能 - 一个工作单元怎么走完

**`do-work`（现 `implement`）技能**代表"仓库中的一个工作单元"，最终被压缩成五行：

```markdown
1. 理解任务
2. 如果任务尚未规划，创建计划（已规划则跳过）
3. 实施
4. 跑反馈循环验证（typecheck + test），持续到全部通过
5. 提交
```

```markdown
Implement the work described by the user in the spec or tickets.

Use /tdd where possible, at pre-agreed seams.

Run typechecking regularly, single test files regularly, and the full test
suite once at the end.

Once done, use /code-review to review the work.

Commit your work to the current branch.
```

#### 让检查变成确定性：pre-commit 钩子

到这一步，反馈循环还只是被"鼓励"运行。要变成**确定性**，要用 git hook：

```bash
# .husky/pre-commit
npx lint-staged        # 只对暂存文件跑 prettier
pnpm run typecheck     # 全仓类型检查
pnpm run test          # 全量测试
```

### 5.4 code review

**流水线**：**实施 → 自动化检查（测试、类型检查、pre-commit、格式化）→ 自动化审查 → 人工审查**。

**自动化审查只查两件事**：**是否符合规格**（PRD 是否被正确实现）、**是否符合编码规范**。关键判断是：**把编码规范从"实施"移出，只给"评审"看**——实施要探索代码、推进功能、防错调试，预算已经很满；规范作为单一来源出现在评审阶段，反而提升质量。

```markdown
Two-axis review of the diff between `HEAD` and a fixed point the user supplies:

- **Standards** — does the code conform to this repo's documented coding standards?
- **Spec** — does the code faithfully implement the originating issue / spec?

Both axes run as **parallel sub-agents** so they don't pollute each other's context, then this skill aggregates their findings.

The issue tracker should have been provided to you — run `/setup-matt-pocock-skills` if `docs/agents/issue-tracker.md` is missing.

## Process

### 1. Pin the fixed point

Whatever the user said is the fixed point — a commit SHA, branch name, tag, `main`, `HEAD~5`, etc. If they didn't specify one, ask for it.

Capture the diff command once: `git diff <fixed-point>...HEAD` (three-dot, so the comparison is against the merge-base). Also note the list of commits via `git log <fixed-point>..HEAD --oneline`.

Before going further, confirm the fixed point resolves (`git rev-parse <fixed-point>`) and the diff is non-empty. A bad ref or empty diff should fail here — not inside two parallel sub-agents.

### 2. Identify the spec source

Look for the originating spec, in this order:

1. Issue references in the commit messages (`#123`, `Closes #45`, GitLab `!67`, etc.) — fetch via the workflow in `docs/agents/issue-tracker.md`.
2. A path the user passed as an argument.
3. A spec file under `docs/`, `specs/`, or `.scratch/` matching the branch name or feature.
4. If nothing is found, ask the user where the spec is. If they say there isn't one, the **Spec** sub-agent will skip and report "no spec available".

### 3. Identify the standards sources

Anything in the repo that documents how code should be written, such as `CODING_STANDARDS.md` or `CONTRIBUTING.md`.

On top of whatever the repo documents, the Standards axis always carries the **smell baseline** below — a fixed set of Fowler code smells (_Refactoring_, ch.3) that applies even when a repo documents nothing. Two rules bind it:

- **The repo overrides.** A documented repo standard always wins; where it endorses something the baseline would flag, suppress the smell.
- **Always a judgement call.** Each smell is a labelled heuristic ("possible Feature Envy"), never a hard violation — and, like any standard here, skip anything tooling already enforces.

Each smell reads *what it is* → *how to fix*; match it against the diff:

- **Mysterious Name** — a function, variable, or type whose name doesn't reveal what it does or holds. → rename it; if no honest name comes, the design's murky.
- **Duplicated Code** — the same logic shape appears in more than one hunk or file in the change. → extract the shared shape, call it from both.
- **Feature Envy** — a method that reaches into another object's data more than its own. → move the method onto the data it envies.
- **Data Clumps** — the same few fields or params keep travelling together (a type wanting to be born). → bundle them into one type, pass that.
- **Primitive Obsession** — a primitive or string standing in for a domain concept that deserves its own type. → give the concept its own small type.
- **Repeated Switches** — the same `switch`/`if`-cascade on the same type recurs across the change. → replace with polymorphism, or one map both sites share.
- **Shotgun Surgery** — one logical change forces scattered edits across many files in the diff. → gather what changes together into one module.
- **Divergent Change** — one file or module is edited for several unrelated reasons. → split so each module changes for one reason.
- **Speculative Generality** — abstraction, parameters, or hooks added for needs the spec doesn't have. → delete it; inline back until a real need shows.
- **Message Chains** — long `a.b().c().d()` navigation the caller shouldn't depend on. → hide the walk behind one method on the first object.
- **Middle Man** — a class or function that mostly just delegates onward. → cut it, call the real target direct.
- **Refused Bequest** — a subclass or implementer that ignores or overrides most of what it inherits. → drop the inheritance, use composition.

### 4. Spawn both sub-agents in parallel

**Standards sub-agent prompt** — include:

- The full diff command and commit list.
- The list of standards-source files you found in step 3, **plus the smell baseline from step 3** pasted in full — the sub-agent has no other access to it.
- The brief: "Report — per file/hunk where relevant — (a) every place the diff violates a documented standard: cite the standard (file + the rule); and (b) any baseline smell you spot: name it and quote the hunk. Distinguish hard violations from judgement calls — documented-standard breaches can be hard, but baseline smells are always judgement calls, and a documented repo standard overrides the baseline. Skip anything tooling enforces. Under 400 words."

**Spec sub-agent prompt** — include:

- The diff command and commit list.
- The path or fetched contents of the spec.
- The brief: "Report: (a) requirements the spec asked for that are missing or partial; (b) behaviour in the diff that wasn't asked for (scope creep); (c) requirements that look implemented but where the implementation looks wrong. Quote the spec line for each finding. Under 400 words."

If the spec is missing, skip the Spec sub-agent and note this in the final report.

### 5. Aggregate

Present the two reports under `## Standards` and `## Spec` headings, verbatim or lightly cleaned. Do **not** merge or rerank findings — the two axes are deliberately separate (see _Why two axes_).

End with a one-line summary: total findings per axis, and the worst issue _within each axis_ (if any). Don't pick a single winner across axes — that's the reranking the separation exists to prevent.

## Why two axes

A change can pass one axis and fail the other:

- Code that follows every standard but implements the wrong thing → **Standards pass, Spec fail.**
- Code that does exactly what the issue asked but breaks the project's conventions → **Spec pass, Standards fail.**

Reporting them separately stops one axis from masking the other.
```

## 六 离线执行与人工协同

### 6.1 最终工作流 - 把七阶段和工具对应起来

![七阶段主流程：Grill → Issues → Research → Implement → Prototype → Review → PRD](/img/技术/ai-engineering/seven-phase-workflow.png)


| 环节 | 技能 | 交给下一步的产物 |
| --- | --- | --- |
| 需求拷问 | `grill-with-docs`（旧名 `grill-me`） | 决策记录、领域文档 |
| 规格与拆票 | `to-spec` / `to-tickets`（旧名 `write-a-prd` / `prd-to-issues`） | 规格、工单与阻塞关系 |
| 实现与评审 | `implement`（旧名 `do-work`），内部应用 `tdd` / `code-review` | 代码、测试与评审结果 |
| 上下文交接 | `handoff` | 进度、证据、风险与下一步 |


### 6.2 主流程之外的技能地图

| 场景 | 技能 | 主要用途 |
| --- | --- | --- |
| 外来任务 | `triage` | 分诊外来 issue / PR，形成 agent brief |
| 疑难问题 | `diagnosing-bugs` | 复现、假设、定位、修复与回归 |
| 大型任务 | `wayfinder` | 梳理决策地图与后续路径 |
| 独立能力 | `prototype` / `research` / `wizard` | 原型验证、资料研究、人机操作协作 |
| 基础词汇 | `domain-modeling` / `codebase-design` | 领域模型与模块设计 |

`triage` 用于外来任务分诊，`to-tickets` 用于规格拆票，两者不要混用。

安装与初始化以官方仓库说明为准：<https://github.com/mattpocock/skills>

## 附录 A：引用来源

- 课程视频：[B 站合集《Matt Pocock【中英⚡面向真实工程师的AI编程｜AI Coding for Real Engineers》](https://www.bilibili.com/video/BV16eKb6PEUo/)（BV16eKb6PEUo），92 集，约 10.0 小时
- 技能仓库：[mattpocock/skills](https://github.com/mattpocock/skills)，官方技能文档站 <https://www.aihero.dev/skills>
- 讲师站点：Total TypeScript、AI Hero（`aihero.dev`）

**讲师在课中引用或点名的资料：**

- 《程序员修炼之道》（The Pragmatic Programmer，David Thomas / Andrew Hunt）：软件熵、追踪子弹（曳光弹）
- 《软件设计哲学》（A Philosophy of Software Design，John Ousterhout）："代码库易于修改即更好"、深模块
- 《极限编程实践》（Kent Beck）：红-绿-重构（red-green-refactor）
- 《Shape Up》（Ryan Singer，Basecamp，basecamp.com/shapeup）：精细度（fidelity）分层
- Geoffrey Huntley 关于 Ralph Wiggum / ralph loop 的文章：AFK 循环的思想来源
- Dex Horthy 的分享：上下文窗口"智能区 / 愚蠢区"的提法
- HumanLayer《Optimizing CLAUDE.md》：`CLAUDE.md` 写法参考
- Superpowers 官方仓库 <https://github.com/obra/superpowers>：课程中并行的另一条框架主线，本文未展开，作为自学参考

## 附录 B：FAQ 精选

课程有六场答疑（P47、P48、P89–P92，合计约 4.8 小时）。下面按主题挑出复用价值最高的问答，已合并重复问题、去掉闲聊。

### 流程与方法

**你的方法与 Superpowers、Spec-Driven Development（SDD）有什么本质区别？**

- Superpowers 这类技能集把技能写成"**能力**"：描述里堆满触发条件，让代理自己判断何时调用。**方向盘在代理手里**，非确定性很高，代理可能压根没捕捉到技能。
- 讲师的技能更像"**流程/命令**"：用户显式说"用 grill 做这件事"就一定能触发，代价是你必须自己思考、按刚性步骤走。
- 他用 `disable-model-invocation: true` 把两者分开：**流程技能（人调用）+ 能力技能（模型调用）**。
- 金句："流程技能里你握着方向盘，能力技能里代理握着方向盘。"

**小任务也要先 grill 吗？**

- 会过度。任务非常明确、你完全知道要改哪里、能完整描述出来时，**不需要 grill**。grill 的本质是"和 AI 对齐"，适用于你还不确定设计概念是否一致的时候。

**怎么让 grill 既不流于表面、又不过度钻牛角尖？**

- 不重要的决策直接说"**我不在乎**"，把优先级让给更重要的事，就像初级开发者问资深同事，答案有时就是"随便选一个"。
- 用《Shape Up》里"**精细度（fidelity）**"的概念排层级：文字 < 线框 < 原型 < 实际应用。默认用低精细度成本更低，但涉及外观的决策，光靠文字根本答不了。
- 判断"足够好"的标志是：**决定成败的最关键问题是否已经被回答**。秘密武器是原型——例如做两个对比原型（WebSocket vs SSE、自建 vs 第三方）来回答"哪个感觉更好"。

**循环工程（loop engineering）的热度是怎么回事？**

- 起因是 Peter Steinberger 那条引爆的推文："不要直接提示 AI，让 AI 自己写循环来提示 AI。"
- 讲师的解读：把"想法到上线"的生命周期切成若干阶段，**逐个移除人类检查点**；移除得越多，越能高效并行。
- 他自己的做法更偏**按需自动化**：问题进来先派代理探索，中间留一个小的检查点，再实施、审查、生成 PR 让他过目。**人不会完全消失，但只需最后那一个检查点。**不必非写成循环结构。

### 上下文与模型

**上下文到 120k 了，该压缩还是交接给新会话？**

- 120k 不是断崖，实际更像一条**平缓下滑的曲线**，没有"到某点突然变笨"的开关。
- 判断标准是**剩余任务的复杂度**：如果剩下的活足够简单、可以直接信任基础模型，就一路做完——讲师有时会做到 250k。
- 压缩在**任务中期、目标明确**时很有用；如果在某个执行单元做到一半就压缩，容易忘掉或混淆已做过的操作。
- 理想状态是压缩趋近无损，那样只需把自动压缩阈值调低、让窗口压在 180k 左右。

**规划和构建时 token 成本一直涨，怎么省？**

- 优先优化**质量**而不是成本。质量与成本之间有一条临界线：质量往下掉，你就得花时间修代理的错；往上走，返工减少，直到可以近乎自动合并。
- 压 token 就会压质量；跌到临界线以下，人工评审和返工成本立刻回来。
- 正确的经济账是把**开发者的小时数换算成 token**：能省掉人工小时就是大幅降本。

**怎么选模型，怎么让代理自觉用便宜的模型？**

- 不给具体型号推荐（个人体验不具代表性）；建议团队把编码代理接进**可观测性工具**，看哪些会话成功、哪些失败，用数据决定。
- **探索阶段用 Haiku**：成本低两个数量级、速度极快，能快速大量读文件并总结，效果够用。做法是写进 `CLAUDE.md` 等引导文件。
- 另一种做法：用 Sonnet 实施、Opus 评审，token 更省而质量还在。

**新模型发布（比如 GPT-5）会改变游戏吗？**

- 人们过度关注模型发布。他通常**等一周再试**，也不愿依赖频繁变动的订阅制模型。
- 模型重要，但**你为代理搭的框架和环境同样关键，大致五五开**。
- 与其等新模型修好你的问题，不如现在就解决：有安全问题就用强模型 + 技能定时扫描，需要什么工具就建什么工具。

### 代码库与规范

**有没有通用编码规范？该放在哪、怎么维护？**

- **不存在真正的通用规范**。这类书容易变成"给你一把锤子，看什么都像钉子"（例如深度模块化被滥用到不合适的场景）。
- 规范应该是**项目相关、公司相关的**；同一家公司用相似的库与框架，规范可以在多个项目间复用。
- 他**强烈反对公司里每个人自建一套规范**，而应统一维护一套、走 RFC 流程，并**放在代码库之外被引用**，让 AI 直接应用。
- 不要只让代理读代码库来生成规范（代码本身已自描述）；正确做法是用**你已有的数据集**：每条 PR 评论都能通过 API 获取，收集起来定期喂给代理更新规范，这个数据集会随你的评审自然增长。

**你反对给仓库写摘要文件，那大型仓库怎么办？**

- 代码和测试才是行为的**事实来源**，摘要等于**缓存**，问题是它很快过期。
- 摘要适合**很少变**的东西：第三方库（升级主版本前有不可违背的契约）值得调研后存成本地 markdown。
- 对频繁变化的自有代码，更好的做法是"**local roads / highway**"：在 `CLAUDE.md` 里写**上下文指针**，代理不必蜿蜒探索，顺着指针直达目标区域——便宜且不会过期。他给这类产物起名叫 **just-in-time docs（需时文档）**。

**知识图谱类工具（如 Grafy）能提升 AI 导航代码库的效率吗？**

- 他持怀疑态度：在代码库之上再建一层图或摘要，动机可以理解，但效果存疑。Claude Code 团队也明确表示"在代码库之上再建一层"效果不佳。
- 真正有效的是**让 AI 直接访问文件系统**，并给它 `grep` 这类强力搜索工具。

**多仓库（multi-repo）环境怎么套用你的工作流？**

- 首选 **monorepo**（哪怕要付工具成本）：一次提交改多处、历史易回溯、单一事实来源。
- 次优方案是加一个**包装仓库（wrapper repo）**：在里面引用仓库一和仓库二，所有目录都可见；把 `AGENTS.md`/`CLAUDE.md` 和技能都放在这个包装仓库里，并在这里实际运行代理。

**代理自主写码后，理解力退化、认知债怎么办？**

- 用**基于契约的设计**缓解：把复杂实现藏进深层模块，对外只暴露一个 100% 可测的接口。他的视频编辑器就是例子：静音检测、实时读取、转录都很复杂，但对外只是可完整测试的接口。
- 出问题再深入排查即可，"隐藏信息 + 委派给 AI"能大幅降低认知债。
- 另外他提醒：**你自己的代码多半是 AI 写的，但值得读的代码前所未有地多**，所以更应该以读代码为主；他本人职业生涯的跃升正是从读开源仓库开始的。

### 测试与审查

**前端项目怎么做 TDD、怎么给代理反馈循环？**

- 前端难在有两层：**功能对不对**（点击是否跳转、计时器算得对不对）和**好不好看**。jsdom、Vitest 之类只能测前者。
- 虚拟 DOM 测试调试极其痛苦（没有浏览器可看、跑得慢、不稳定）。实用做法是**给代理浏览器访问权**（Playwright、agent browser 之类），让它对着 dev server 真点、真截图，并通过**可访问性检查器**把界面转成文本读进去。
- 结论：这仍是难题，只能靠"少量前端测试 + 让代理浏览操作"来缓解。

**实施和评审怎么分工？自动审查该查什么？**

- 流水线拆成：**实施 → 自动化检查（测试、类型检查、pre-commit、格式化）→ 自动化审查 → 人工审查**，只有最后一层必须有人。
- 自动化审查只查两件事：**是否符合规格**（PRD 是否被正确实现）、**是否符合编码规范**。
- 关键判断：**把编码规范从"实施"移出，只给"评审"看**。实施要探索代码、推进功能、防错调试，预算已经很满；规范作为单一来源出现在评审阶段，反而提升质量。
- 可以用 Sonnet 做实施、Opus 做评审，token 更省而质量还在；也可以让自动化审查直接修问题，再告知人工改了哪些，让人工看到接近完美的 PR。

**写出的代码不符合我的标准，怎么让它按规范写？**

- 让实现代理同时"实现 + 应用编码规范"符合人的直觉，但对 AI 是**任务过载**，会明显降低表现。
- 正确做法：走红绿重构循环，先让实现代理写基础实现并通过测试 → **清空上下文** → 让新的审查代理看 diff → **立即要求应用规范并修复**。自动审查确实有效，但 token 开销不小。

**发布前该跑哪些测试？平时要不要一直写测试？**

- "发布前的最优测试组合"是**错误心态**。应该保持**持续交付**：每五分钟、每次 CI 通过就是一个版本，把大发布拆小。
- 大批量发布总有一部分代码有问题，而且差异太大难以定位；小批量出问题时你能看清具体 diff，回滚也不会牵连无关功能。
- 最优测试就是**每天跑的 CI 测试**，前提是测试质量要高。他几乎总是写测试，尤其是后端，**在预先约定的接口处测试**，不写琐碎的微单元测试。

**自动化审查该怎么组织、怎么拿到结构化结果？**

- 让代理把结构化结果写进 XML 标签（如 `<output>`），再用 zod schema 校验，取 `output.inlineComments`、`output.replies` 之类字段——这样代理可以作为"**获取结构化输出的手段**"，再驱动后续动作（开别的代理、留评论），而且带重试，所以可靠。
- 最靠谱的建议是"直接看仓库，让你的代理去读那些代码"。

### 安全、团队与个人成长

**使用代理的关键安全考量是什么？**

- 分两层：**你产出的软件**的安全性，和**代理自身**的安全性。
- 企业最大的风险是**数据外泄与提示注入**：代理上网 fetch 时可能被恶意页面操纵，用 `curl` 把整个代码库、环境变量或敏感数据发到外部端点。
- 主要防线是**沙盒**：限制出站网络流量、只允许白名单域名，这类请求就无法完成。在 Docker 里做限制并不难。

**大团队、大代码库怎么落地这套流程？**

- 合适的团队规模大约 **3–10 人**。
- **PRD 之前的一切必须是协作的**：一起开会讨论、把会议记录喂给代理深化成 PRD、原型共享评审，避免信息单点故障。PRD 达成共识后，实现可以交给一个人，再做团队评审。
- PRD 必须放在**多人可审阅的地方**（GitHub issue、Jira、Linear）；本地保存通常是个坏主意，课程里用本地只是为了让起点简单。

**新手该学哪些语言、怎么定路线？**

- 完全取决于你想构建什么。讲师本人基本只专注一门语言（TypeScript/JavaScript，加 Web 必需的 CSS 和 HTML），**只专注一门语言是好目标**，能真正深入成为专家。
- 路线图可以用 `teach` 技能：新建项目、装上技能、说明你想学什么，它会生成一路引导你的 HTML 课程（他用它学会了还原魔方）。
- 关于 CS 学位：学位教的基础（测试、接口、构建好代码库）**比以往更重要**，但要尽早把这些工具融进工作流。**战术编码基本交给 AI，你专注战略层。**

**代理写码之后，怎么保持手艺不退步？**

- 不要迷信"学习风格"：匹配偏好只是让你学得更顺畅，不等于学得更好。
- 值得读的代码前所未有地多，**以读代码为主**；他职业跃升正是从读别人的开源仓库开始的。

**学员最普遍的焦虑是什么？**

- 是"**偏执**"：觉得自己被 AFK 代理束缚住、不知道该委派什么，甚至觉得应该把所有事都委派出去。
- 课程所有工作的目的就是给你一套框架，让 AI 不再是黑箱：**保留思考和方向感，只把需要的那部分交出去。**
- 所以他反复强调"**控制权**"——不是拒绝 AI，而是别把判断力一起交出去。

**课程和技能会一直更新吗？我要一直追吗？**

- **技能是你的**：应该按需改成自己的版本，不必追着作者更新。有人下载后自己改，他很欢迎——"这正是它们存在的意义"。

**谁对你的影响最大？**

- Dex Horthy（"智能区/愚蠢区"的说法）、Geoffrey Huntley（ralph loop，让他看到"造 AFK 代理并开始交付"其实很简单），以及一位用 GitHub Actions 搭"工厂"的 CEO（他现在的整套自动化都建在 GitHub workflow 上）。

**答疑里反复出现的三个主题**

1. **AX 优先于 DX**：区分开发者体验（DX）与代理体验（AX）。开发者拿到大量文档只会快速浏览，**代理会照着文档行动**，所以 AX 是重点。评审要同时看 **UX → DX → AX** 三个维度，AX 大致处在新的优先级顶端。
2. **人的判断力就是上限**：AI 已经吃下战术编程，战略（该构建什么、往哪走）、对齐与价值判断仍归人类。AI 知道怎么构建，**不知道该构建什么**。
3. **小步、可委托的循环**：原型先验证方向，实现与审查分离，小批量持续交付，再把重复环节交给 AFK 与自动化流水线。
