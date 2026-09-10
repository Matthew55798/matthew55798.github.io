# Vibe Coding 重点摘抄

本文汇总 Vibe Coding / AI Coding 调研过程中标注的重要内容。摘录保留中英文原文，并按概念之间的上下文关系整理；每条内容同时记录来源与简要归纳。

## 一、协作理念

### 1. 从一次性助手转向持续改进的队友

#### 摘要

Codex 更适合作为可持续配置和改进的工程队友，而不是只响应单次指令的工具。有效的长期协作体系包括六个相互衔接的部分：提供准确的任务上下文、用 `AGENTS.md` 固化长期规则、让工具配置匹配团队工作流、通过 MCP 接入外部系统、将重复工作封装为 skills，以及把成熟稳定的流程进一步自动化。

#### English

> Codex works best when you treat it less like a one-off assistant and more like a teammate you configure and improve over time.
>
> A useful way to think about this: start with the right task context, use `AGENTS.md` for durable guidance, configure Codex to match your workflow, connect external systems with MCP, turn repeated work into skills, and automate stable workflows.

#### 中文

> Codex 的最佳使用方式，是不要把它当成一次性的助手，而是当作一个你持续配置、不断改进的队友。
>
> 可以这样理解整体思路：以正确的任务上下文起步，用 `AGENTS.md` 沉淀持久化的指导，配置 Codex 以匹配你的工作流，用 MCP 连接外部系统，把重复性工作封装成 skills，并将稳定的工作流自动化。

#### 来源

- OpenAI Codex 官方文档：[Best practices](https://developers.openai.com/codex/learn/best-practices)，开篇概述。
- 本地归档：[AI Coding 团队实践资料中英文对照翻译](/软件工程/技术/参考资料/AI-Coding-团队实践资料中英文对照翻译.md)，`Codex - Best practices（最佳实践）`。
- 摘录日期：2026-07-26。

## 二、任务上下文与提示词

### 1. 用四要素提高任务结果的可靠性

#### 摘要

完美的提示词不是使用 Codex 的前提，但清晰、结构化的任务描述可以显著提高结果的稳定性，在大型代码库和高风险任务中尤其重要。任务描述应尽量覆盖四个要素：明确要实现的目标，指出相关文件、文档和错误等上下文，说明必须遵守的技术与安全约束，并给出可以验证的完成条件。

#### English

> **Strong first use: Context and prompts**
>
> Codex is already strong enough to be useful even when your prompt isn't perfect. You can often hand it a hard problem with minimal setup and still get a strong result. Clear prompting isn't required to get value, but it does make results more reliable, especially in larger codebases or higher-stakes tasks.
>
> If you work in a large or complex repository, the biggest unlock is giving Codex the right task context and a clear structure for what you want done.
>
> A good default is to include four things in your prompt:
>
> - **Goal:** What are you trying to change or build?
> - **Context:** Which files, folders, docs, examples, or errors matter for this task? You can @ mention certain files as context.
> - **Constraints:** What standards, architecture, safety requirements, or conventions should Codex follow?
> - **Done when:** What should be true before the task is complete, such as tests passing, behavior changing, or a bug no longer reproducing?

#### 中文

> **首次使用的关键：上下文与提示词**
>
> 即使你的提示词不够完美，Codex 也已经足够强大，能够产生有用的结果。你常常只需最少量的准备，就能把一个难题交给它并得到不错的输出。清晰的提示词并非获得价值的必要条件，但它确实能让结果更可靠——尤其是在大型代码库或高风险任务中。
>
> 如果你在大型或复杂的代码仓库中工作，最大的提升杠杆是给 Codex 正确的任务上下文，以及对你期望成果的清晰结构化描述。
>
> 一个不错的默认做法是在提示词中包含四项内容：
>
> - **Goal（目标）：** 你想要改动或构建什么？
> - **Context（上下文）：** 哪些文件、目录、文档、示例或报错与本任务相关？你可以用 @ 提及某些文件作为上下文。
> - **Constraints（约束）：** Codex 应遵循哪些标准、架构、安全要求或约定？
> - **Done when（完成条件）：** 任务完成前应满足什么条件，例如测试通过、行为已改变、或某个 bug 不再复现？

#### 来源

- OpenAI Codex 官方文档：[Best practices](https://developers.openai.com/codex/learn/best-practices)，`Strong first use: Context and prompts`。
- 本地归档：[AI Coding 团队实践资料中英文对照翻译](/软件工程/技术/参考资料/AI-Coding-团队实践资料中英文对照翻译.md)，`首次使用的关键：上下文与提示词`。
- 摘录日期：2026-07-26。

## 三、项目级持久指导

### 1. 用 `AGENTS.md` 沉淀可复用的仓库规则

#### 摘要

任务提示词负责描述当前要做什么，`AGENTS.md` 则负责沉淀跨任务复用的仓库级协作规则。它会自动进入 agent 的上下文，适合记录项目结构、运行与验证命令、工程规范、交付期望、行为边界和完成标准，让 Codex 在不同任务和会话中持续遵循团队的实际工作方式。

#### English

> **Make guidance reusable with `AGENTS.md`**
>
> Think of `AGENTS.md` as an open-format README for agents. It loads into context automatically and is the best place to encode how you and your team want Codex to work in a repository.
>
> A good `AGENTS.md` covers:
>
> - repo layout and important directories
> - How to run the project
> - Build, test, and lint commands
> - Engineering conventions and PR expectations
> - Constraints and do-not rules
> - What done means and how to verify work

#### 中文

> **用 `AGENTS.md` 沉淀可复用的指导**
>
> 把 `AGENTS.md` 想象成给 agent（智能体）看的开放式 README。它会自动加载到上下文中，是记录你和团队希望 Codex 在某个仓库中如何运作的最佳位置。
>
> 一个合格的 `AGENTS.md` 应包含：
>
> - 仓库布局与重要目录
> - 如何运行项目
> - 构建、测试和 lint 命令
> - 工程约定与 PR（pull request，代码合并请求）期望
> - 约束与禁止规则
> - “完成”的含义以及如何验证成果

#### 来源

- OpenAI Codex 官方文档：[Best practices](https://developers.openai.com/codex/learn/best-practices)，`Make guidance reusable with AGENTS.md`。
- 本地归档：[AI Coding 团队实践资料中英文对照翻译](/软件工程/技术/参考资料/AI-Coding-团队实践资料中英文对照翻译.md)，`用 AGENTS.md 沉淀可复用的指导`。
- 摘录日期：2026-07-26。

## 四、可复用能力与 Skills

### 1. Skills 封装的四类能力

#### 摘要

Skills 用于把特定领域中可重复执行的复杂任务封装为稳定能力。它不仅能保存多步骤操作流程，还能说明如何调用特定工具和 API、补充模型原本不具备的组织领域知识，并随流程提供脚本、参考文档和素材。相比面向整个仓库的 `AGENTS.md`，Skill 更聚焦于某一类任务或专业领域的完整执行方法。

#### English

> **What Skills Provide**
>
> 1. **Specialized workflows** - Multi-step procedures for specific domains
> 2. **Tool integrations** - Instructions for working with specific file formats or APIs
> 3. **Domain expertise** - Company-specific knowledge, schemas, business logic
> 4. **Bundled resources** - Scripts, references, and assets for complex and repetitive tasks

#### 中文

> **Skills 提供的能力**
>
> 1. **专业化工作流**——针对特定领域的多步骤流程
> 2. **工具集成**——与特定文件格式或 API 协作的指令
> 3. **领域专业知识**——公司特定的知识、数据模式、业务逻辑
> 4. **捆绑资源**——用于复杂和重复任务的脚本、参考文档和素材

#### 来源

- OpenAI Codex 官方文档：[Agent Skills](https://developers.openai.com/codex/skills)，`What Skills Provide`。
- 本地归档：[AI Coding 团队实践资料中英文对照翻译](/软件工程/技术/参考资料/AI-Coding-团队实践资料中英文对照翻译.md)，`Skills 提供的能力`。
- 摘录日期：2026-07-26。

### 2. Skill 的目录结构

#### 摘要

一个 Skill 的最小组成是目录中的 `SKILL.md`。该文件必须包含带 `name` 和 `description` 的 YAML frontmatter，以及 Markdown 格式的执行指令。Skill 还可以提供用于界面展示的 `agents/openai.yaml`，并按需捆绑可执行脚本、参考文档和输出素材，使说明、执行逻辑与配套资源能够作为一个完整能力单元共同分发。

#### English

> Every skill consists of a required `SKILL.md` file and optional bundled resources:

```text
skill-name/
├── SKILL.md (required)
│   ├── YAML frontmatter metadata (required)
│   │   ├── name: (required)
│   │   └── description: (required)
│   └── Markdown instructions (required)
├── agents/ (recommended)
│   └── openai.yaml - UI metadata for skill lists and chips
└── Bundled Resources (optional)
    ├── scripts/          - Executable code (Python/Bash/etc.)
    ├── references/       - Documentation intended to be loaded into context as needed
    └── assets/           - Files used in output (templates, icons, fonts, etc.)
```

#### 中文

> **Skill 结构**
>
> 一个 skill 是一个包含 `SKILL.md` 文件以及可选脚本和参考文档的目录。`SKILL.md` 文件必须包含 `name` 和 `description`。

#### 来源

- OpenAI Codex 官方文档：[Agent Skills](https://developers.openai.com/codex/skills)，`Skill Structure`。
- 本地归档：[AI Coding 团队实践资料中英文对照翻译](/软件工程/技术/参考资料/AI-Coding-团队实践资料中英文对照翻译.md)，`Skill 结构`。
- 摘录日期：2026-07-26。
